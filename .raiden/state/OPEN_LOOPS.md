# Open Loops

## Active

_(none — current version stable)_

## Planned — v4.1.0 (next version)

| # | Feature | Description | Notes |
|---|---------|-------------|-------|
| F1 | Hover-expand panel | Hovering over the cover art expands the widget upward to show a larger cover + full title/artist details | Moderate complexity — animated SetWindowPos upward, third timer at ~30fps to avoid DWM tearing, expanded DrawMediaPanel pass |

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

## Resolved — v4.0.1-SD (Session 2)

| Item | Fix commits | Description |
|------|------------|-------------|
| Libby blank icon | f0760f2, 4685c7e | Fetch cover from Open Library Covers API instead of transparent GSMTC thumbnail |
| Libby title strip | f307a8c | Strip "Libby - Open: " prefix to surface just the book title |
| OL false positive | c5a58f8 | Switch to title= search + exact-match selection to avoid wrong-book covers |
| Lock-held-during-IO | f0760f2 | Restructured UpdateMediaInfo — cover fetch now runs outside g_MediaState.lock |
