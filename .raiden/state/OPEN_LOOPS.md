# Open Loops

## Active

### BUG-1: Null dereference on window creation failure
- **Location:** line 796 (`SetLayeredWindowAttributes` called unconditionally)
- **Risk:** crash if both `CreateWindowInBand` and `CreateWindowEx` fail
- **Next action:** add null check before `SetLayeredWindowAttributes`

### BUG-4: SendMediaCommand targets wrong session
- **Location:** line 313 (`GetCurrentSession()` used instead of active-playing session)
- **Risk:** media commands (play/pause/skip) go to wrong app when multiple sessions exist
- **Next action:** capture and reuse the session pointer found during `UpdateMediaInfo`

### BUG-5: Blocking WinRT calls on message thread
- **Location:** line 279 (`TryGetMediaPropertiesAsync().get()`, `OpenReadAsync().get()` inside WM_TIMER)
- **Risk:** message pump stalls on slow media sources
- **Next action:** offload blocking calls to a worker thread or use coroutine continuation

### BUG-2: WM_APP collision
- **Location:** line 523 (`APP_WM_CLOSE` defined as `WM_APP` = 0x8000)
- **Risk:** any shell/framework message at 0x8000 could trigger DestroyWindow
- **Next action:** redefine as `WM_APP + 1`

### BUG-3: g_Running dead code
- **Location:** line 175
- **Risk:** none (dead code), but misleading
- **Next action:** remove the flag and its set/clear sites

### BUG-6: Scroll offset reset races timer
- **Location:** lines 473–476 (reset in DrawMediaPanel), line 622 (timer killed on next tick)
- **Risk:** spurious InvalidateRect after reset
- **Next action:** kill `IDT_ANIMATION` immediately when text fits, before resetting offset

### BUG-7: HIWORD/LOWORD mouse coords
- **Location:** lines 676–677
- **Risk:** wrong coords on multi-monitor setups with negative screen coordinates
- **Next action:** replace with `GET_X_LPARAM` / `GET_Y_LPARAM`

## Resolved

_(none yet)_
