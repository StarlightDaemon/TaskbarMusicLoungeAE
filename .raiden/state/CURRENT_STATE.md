# Current State

## As of 2026-05-08 (Session 4 — v1.0.0 release)

### Completed

**Session 4 — Version shift & Windhawk compliance**
- [x] Renamed `taskbar-music-lounge-ae.cpp` → `taskbar-music-lounge-ae.wh.cpp` (required extension)
- [x] Fixed `@github` to profile URL (`https://github.com/StarlightDaemon`)
- [x] Quoted `ButtonScale` and `TextColor` settings defaults as YAML strings
- [x] Shifted versioning: internal v4.1.3 → public **v1.0.0** (Audiobook Edition)
- [x] Readme: renamed title to "Taskbar Music Lounge AE", removed upstream v3/v4 table, added clean attribution to Hashah2311 v4.0.1
- [x] Changelog: replaced multi-era versioning with single v1.0.0 entry; upstream changelog preserved below divider

**Session 3 — Bug fixes (internal v4.1.1–v4.1.3)**
- [x] Bug 1: HandleCoverWrong / poll race — added `fetchInProgress` guard field
- [x] Bug 2: mid-fetch title check — guards `g_MediaState.title == newTitle` before writing cover
- [x] Bug 3: HandleCoverLock no-op on evicted entry — creates new locked entry
- [x] Cleanups: SendInput, deferred RegisterWindowMessage, PickCoverId escape fix, surrogate pairs, persistent WinHTTP session

**Session 1 — Bug fixes (upstream baseline)**
- [x] Renamed `Source Code.txt` → source file
- [x] Bug 1: null dereference on window creation failure
- [x] Bug 2: WM_APP collision
- [x] Bug 3: g_Running dead code
- [x] Bug 4: SendMediaCommand targets wrong session
- [x] Bug 5: blocking WinRT calls on message thread
- [x] Bug 6: scroll offset reset races timer
- [x] Bug 7: HIWORD/LOWORD mouse coords

**Session 2 — Libby support**
- [x] Libby title: strip "Libby - Open: " prefix
- [x] Libby cover: Open Library two-pass fetch with subtitle stripping
- [x] Libby cover: exact title match — no wrong-book fallback
- [x] Libby cover: 5-entry LRU in-memory cache with miss sentinels
- [x] Libby cover: right-click menu — Try Different Cover, Remove Cover, Lock Cover, Restore Cover
- [x] Libby cover: Lock Cover pins image, greys destructive items, shows 🔒/🔓 emoji toggle
- [x] Libby cover: race condition fix — concurrent poll respects tried IDs, no duplicate cache entries
- [x] Lock hygiene: cover fetch runs outside g_MediaState.lock

### Active

None.

### Pending — v1.1.0

- [ ] **F1: Hover-expand panel** — hover over cover art expands widget upward showing large cover + full title/artist. Moderate complexity: animated SetWindowPos upward, 30fps timer, expanded DrawMediaPanel pass.

### Blockers

None.

### Next steps (when resumed)

- Begin v1.1.0 with hover-expand panel (F1)
- Monitor upstream (Hashah2311) for new releases; review and reconcile before bumping AE version
- Consider Audible cover art support once live testing is available
