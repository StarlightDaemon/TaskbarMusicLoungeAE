# Current State

## As of 2026-05-08 (Session 2 — complete)

### Completed

**Session 1 — Bug fixes**
- [x] Renamed `Source Code.txt` → `taskbar-music-lounge-sd.cpp`
- [x] Updated mod header: `@id taskbar-music-lounge-sd`, `@name Taskbar Music Lounge SD`
- [x] Bug 1: null dereference on window creation failure — **fixed** (8b60012)
- [x] Bug 2: WM_APP collision — **fixed** (2c68642)
- [x] Bug 3: g_Running dead code — **fixed** (cbea593)
- [x] Bug 4: SendMediaCommand targets wrong session — **fixed** (f307a8c)
- [x] Bug 5: blocking WinRT calls on message thread — **fixed** (f307a8c)
- [x] Bug 6: scroll offset reset races timer — **fixed** (babe4a3)
- [x] Bug 7: HIWORD/LOWORD mouse coords — **fixed** (39f2d58)

**Session 2 — Libby improvements**
- [x] Libby title: strip "Libby - Open: " prefix → shows "Wool", "Leviathan" etc.
- [x] Libby cover: fetch from Open Library Covers API (two-step: title→cover_i→JPEG)
- [x] Libby cover: exact title match selection to avoid false positives (c5a58f8)
- [x] Lock hygiene: cover fetch moved outside g_MediaState.lock (f0760f2)
- [x] WinHTTP: WINHTTP_NO_REQUEST_BODY compile fix (4685c7e)
- [x] Cover cache: LRU 5-entry in-memory cache with miss sentinels
- [x] Cleanup: all [THUMB] diagnostic logging removed

### Active

None.

### Pending — v4.1.0

- [ ] **F1: Hover-expand panel** — hover over cover art expands widget upward showing large cover + full title/artist. Moderate complexity, held for next version.

### Blockers

None.

### Next steps

- Live test cover art for more Libby titles (confirm exact-match logic holds)
- Increment `@version` from 4.0.1 when ready to publish
- Begin v4.1.0 with hover-expand panel (F1)
