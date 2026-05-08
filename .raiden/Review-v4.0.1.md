# Taskbar Music Lounge — Code Review

**Reviewed:** 2026-05-08  
**Version:** 4.0.1  
**File:** `Source Code.txt`

---

## Architecture

The process model is well-designed for a Windhawk mod. The tool mod pattern (lines 852–1008) spawns a dedicated `windhawk.exe` subprocess via `CreateProcessInternalW`, hooks its entry point to prevent normal startup, and runs the mod there instead — avoiding all interference with `explorer.exe` internals. This is clean and robust.

The worker thread (`MediaThread`, line 750) owns its own COM apartment, GDI+ lifetime, window class, and message loop. Teardown (`WhTool_ModUninit`) sends `APP_WM_CLOSE` and joins cleanly. No obvious resource leaks in the happy path.

---

## Bugs and Issues

### 1. Null dereference on window creation failure — line 796

`SetLayeredWindowAttributes` is called unconditionally after both `CreateWindowInBand` and `CreateWindowEx` attempts, but if both fail `g_hMediaWindow` is `NULL`. No null check:

```cpp
// Both branches can fail, g_hMediaWindow can be NULL here
SetLayeredWindowAttributes(g_hMediaWindow, 0, 255, LWA_ALPHA);
```

### 2. `WM_APP` collision — line 523

`APP_WM_CLOSE` is defined as `WM_APP` (exactly `0x8000`). `WM_APP` is the *base* of the user-defined range, not a specific message. Any framework or shell message in this range could accidentally trigger `DestroyWindow`. Using `WM_APP + 1` or higher would be safer.

### 3. `g_Running` is set but never read — line 175

The `g_Running` atomic flag is declared and set in `WhTool_ModInit`/`WhTool_ModUninit`, but the `GetMessage` loop never checks it. Shutdown works only because `APP_WM_CLOSE` → `DestroyWindow` → `PostQuitMessage` breaks the loop. The flag is dead code — it should either be used or removed.

### 4. Media commands go to current session, not the playing session — line 313

`UpdateMediaInfo` correctly walks all sessions to find the *actively playing* one, but `SendMediaCommand` always calls `GetCurrentSession()`, which may point at a different session. A paused Spotify with Chrome playing would have commands go to the wrong app.

### 5. WinRT blocking calls on the message thread — line 279

`TryGetMediaPropertiesAsync().get()` and `OpenReadAsync().get()` are blocking `.get()` calls inside `WM_TIMER` on the message thread. For slow media sources (AIMP, network streams), this can block the entire window message pump, stall repaints, and delay event hook responses.

### 6. Scroll offset resets while timer is still active — lines 473–476

When text stops scrolling (becomes short enough to fit), `g_ScrollOffset = 0` and `g_IsScrolling = false` are set inside `DrawMediaPanel`, but the `IDT_ANIMATION` timer is only killed in the *next* timer fire (line 622). One stray animation tick can trigger a spurious `InvalidateRect` after the reset.

### 7. `HIWORD`/`LOWORD` for mouse coords — lines 676–677

```cpp
int x = LOWORD(lParam);
int y = HIWORD(lParam);
```

These macros return unsigned 16-bit values. On multi-monitor setups with negative screen coordinates, `x` or `y` can wrap to large positive values. `GET_X_LPARAM`/`GET_Y_LPARAM` from `<windowsx.h>` should be used instead.

---

## Design Observations

**Good:**
- Event-driven taskbar tracking via `SetWinEventHook` scoped to the taskbar thread/PID is exactly the right approach — zero CPU when idle.
- Double-buffering in `WM_PAINT` (lines 722–731) prevents flicker correctly.
- Mutex-guarded `MediaState` copy before drawing avoids holding the lock during GDI+ work.
- `TaskbarCreated` message handling (lines 736–743) properly re-registers the hook after Explorer restarts.
- `ZBID_IMMERSIVE_NOTIFICATION` with graceful fallback is a thoughtful z-ordering strategy.

**Worth revisiting:**
- The tint color formula on line 348 in the manual-color path `(g_Settings.bgOpacity << 24) | (0xFFFFFF)` ignores the user's `TextColor` and always uses white as the tint color. That may or may not be intentional.
- Volume control via `keybd_event` (line 714) is the old API; `SendInput` is the correct modern replacement, though for volume keys the behavior is identical in practice.
- `using namespace winrt; using namespace Windows::Media::Control;` both at global scope (lines 91–93) can cause ambiguous name resolution issues in larger projects. Not a problem here given the single-file scope.

---

## Summary

The architecture is solid for a Windhawk mod. The most actionable fixes are the **null dereference on window creation failure** (crash risk), **`SendMediaCommand` targeting wrong session** (functional bug), and **blocking WinRT calls on the message thread** (reliability). The rest are minor correctness issues.
