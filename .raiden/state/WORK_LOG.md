# Work Log

## 2026-05-08 — Session 1: First run / scaffold + all 7 bug fixes

**Agent:** RAIDEN working agent (claude-sonnet-4-6)

### Actions taken

1. Read `Review.md` in full; read source header to understand mod structure.
2. Renamed `Source Code.txt` → `taskbar-music-lounge-sd.cpp`.
3. Updated Windhawk mod header: `@id` → `taskbar-music-lounge-sd`, `@name` → `Taskbar Music Lounge SD`.
4. Populated DECISIONS.md, GOALS.md, OPEN_LOOPS.md, CURRENT_STATE.md, WORK_LOG.md.
5. Scaffold commit (1f70ae8).
6. Bug 1 fix: guarded `SetLayeredWindowAttributes` against null `g_hMediaWindow`; added early-exit with cleanup (8b60012).
7. Bug 4 fix: introduced `g_ActiveSession` global; `UpdateMediaInfo` writes the chosen session; `SendMediaCommand` reads from it (f307a8c).
8. Bug 5 fix: offloaded `UpdateMediaInfo` to a detached `std::thread`; blocking IO no longer runs on message thread; added `WM_APP+11` to repaint after each update (f307a8c, same commit as Bug 4).
9. Bug 2 fix: changed `APP_WM_CLOSE` from `WM_APP` to `WM_APP + 1` (2c68642).
10. Bug 3 fix: removed dead `g_Running` flag and all three sites (cbea593).
11. Bug 6 fix: added `else KillTimer(hwnd, IDT_ANIMATION)` in `WM_PAINT` to terminate animation timer immediately when scrolling stops (babe4a3).
12. Bug 7 fix: added `<windowsx.h>`, replaced `LOWORD`/`HIWORD` with `GET_X_LPARAM`/`GET_Y_LPARAM` (39f2d58).
13. Updated CURRENT_STATE.md, OPEN_LOOPS.md, WORK_LOG.md.

### Status at session end

All 7 bugs resolved. No open loops. Mod is ready for integration testing.

---

## 2026-05-08 — Session 2: Libby cover art + title improvements

**Agent:** RAIDEN working agent (claude-sonnet-4-6)

### Actions taken

1. Researched Libby/OverDrive cover art options; concluded Open Library Covers API is best no-auth path.
2. Researched Open Library API in depth: rate limits, coverage, caching, stability, WinHTTP notes.
3. Implemented Open Library cover fetch (f0760f2):
   - Added `-lwinhttp` compiler option and `#include <winhttp.h>`
   - `UrlEncodeTitle`: percent-encodes title for URL query param
   - `HttpsGet`: synchronous HTTPS GET via WinHTTP (background thread safe)
   - `FetchOpenLibraryCover`: two-step title→cover_i→JPEG→Bitmap*
   - `GetOrFetchCover`: LRU cache (5 entries) with miss sentinels
   - Cover cache teardown in WM_DESTROY
   - Restructured `UpdateMediaInfo`: cover fetch outside g_MediaState.lock
   - Removed all [THUMB] diagnostic logging
4. Fixed WINHTTP_NO_REQUEST_BODY (not a real macro → nullptr) (4685c7e).
5. Tested: cover appeared but "Leviathan" returned "Leviathan Wakes" (false positive).
6. Fixed title matching (c5a58f8):
   - Switched from `q=` to `title=` search parameter
   - Fetch 5 candidates instead of 1
   - `PickCoverId`: selects exact case-insensitive title match first, falls back to first result
7. Logged hover-expand panel feature as F1 in OPEN_LOOPS.md for v4.1.0.
8. Updated CURRENT_STATE.md, OPEN_LOOPS.md, WORK_LOG.md.

### Status at session end

Libby cover art and title handling complete and tested. One confirmed false-positive fix applied.
Hover-expand panel deferred to v4.1.0. Cover art refinement for other audiobook sources (Audible etc.) in progress next.
