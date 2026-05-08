# Current State

## As of 2026-05-08 (Session 3 — complete, v4.1.1 released)

### Completed

**Session 3 — Bug fixes (v4.1.1)**
- [x] Bug 1: HandleCoverWrong / poll race — added `fetchInProgress` guard field; poll skips re-fetch while dedicated thread is running
- [x] Bug 2: mid-fetch title check — commit condition now guards `g_MediaState.title == newTitle` before writing cover
- [x] Bug 3: HandleCoverLock no-op on evicted entry — creates new locked entry with eviction logic mirroring HandleCoverRemove

### Completed

**Session 1 — Bug fixes (v4.0.1-SD)**
- [x] Renamed `Source Code.txt` → `taskbar-music-lounge-sd.cpp`
- [x] Updated mod header: `@id taskbar-music-lounge-sd`, `@name Taskbar Music Lounge SD`
- [x] Bug 1: null dereference on window creation failure — **fixed** (8b60012)
- [x] Bug 2: WM_APP collision — **fixed** (2c68642)
- [x] Bug 3: g_Running dead code — **fixed** (cbea593)
- [x] Bug 4: SendMediaCommand targets wrong session — **fixed** (f307a8c)
- [x] Bug 5: blocking WinRT calls on message thread — **fixed** (f307a8c)
- [x] Bug 6: scroll offset reset races timer — **fixed** (babe4a3)
- [x] Bug 7: HIWORD/LOWORD mouse coords — **fixed** (39f2d58)

**Session 2 — Libby support (v4.1.0)**
- [x] Libby title: strip "Libby - Open: " prefix → shows book title only
- [x] Libby cover: Open Library two-pass fetch (title= then q=) with subtitle stripping
- [x] Libby cover: exact title match — no wrong-book fallback
- [x] Libby cover: 5-entry LRU in-memory cache with miss sentinels
- [x] Libby cover: right-click menu — Try Different Cover, Remove Cover, Lock Cover, Restore Cover
- [x] Libby cover: Lock Cover pins image, greys destructive items, shows 🔒/🔓 emoji toggle
- [x] Libby cover: race condition fix — concurrent poll respects tried IDs, no duplicate cache entries
- [x] Lock hygiene: cover fetch runs outside g_MediaState.lock
- [x] `#include <algorithm>` added for std::remove_if / std::find
- [x] @version 4.0.1 → 4.1.0; readme updated with Libby Support bullet
- [x] Changelog.txt updated

### Active

None.

### Pending — v4.2.0

- [ ] **F1: Hover-expand panel** — hover over cover art expands widget upward showing large cover + full title/artist. Moderate complexity: animated SetWindowPos upward, 30fps timer, expanded DrawMediaPanel pass.

### Blockers

None.

### Next steps (when resumed)

- Begin v4.2.0 with hover-expand panel (F1)
- Consider Audible cover art support once live testing is available
