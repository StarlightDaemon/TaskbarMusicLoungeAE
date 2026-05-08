# Open Loops

## Active

_(none — v4.1.0 stable)_

## Planned — v4.2.0 (next version)

| # | Feature | Description | Notes |
|---|---------|-------------|-------|
| F1 | Hover-expand panel | Hovering over cover art expands the widget upward showing large cover + full title/artist | Moderate complexity — animated SetWindowPos upward, 30fps timer to avoid DWM tearing, expanded DrawMediaPanel pass |

## Resolved — v4.0.1-SD (Session 1)

| Bug | Fix commit | Description |
|-----|-----------|-------------|
| 1 | 8b60012 | Null dereference on window creation failure |
| 4 | f307a8c | SendMediaCommand targeted wrong session |
| 5 | f307a8c | Blocking WinRT calls on message thread |
| 2 | 2c68642 | WM_APP collision (APP_WM_CLOSE = WM_APP) |
| 3 | cbea593 | g_Running flag was dead code |
| 6 | babe4a3 | Scroll offset reset raced animation timer |
| 7 | 39f2d58 | HIWORD/LOWORD mouse coords on multi-monitor |

## Resolved — v4.1.0 (Session 2)

| Item | Fix commits | Description |
|------|------------|-------------|
| Libby blank icon | f0760f2, 4685c7e | Open Library Covers API fetch (two-pass, subtitle-strip, exact-match) |
| Libby title strip | f307a8c | Strip "Libby - Open: " prefix to surface just the book title |
| OL false positive | c5a58f8, 3c59aaa | title= field search, exact-match only, no first-result fallback |
| Lock-held-during-IO | f0760f2 | Cover fetch now runs outside g_MediaState.lock |
| Wrong-cover revert | 2239ae2 | Race condition: GetOrFetchCover now carries triedCoverIds, updates in-place |
| Cover menu | 3c59aaa, f4fdafa, b9ff69b | Right-click: Try Different, Remove, Lock (🔒/🔓), Restore |
