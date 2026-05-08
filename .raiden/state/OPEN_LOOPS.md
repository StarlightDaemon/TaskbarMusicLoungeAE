# Open Loops

## Active

_(none — v1.0.0 stable)_

## Planned — v1.1.0 (next version)

| # | Feature | Description | Notes |
|---|---------|-------------|-------|
| F1 | Hover-expand panel | Hovering over cover art expands the widget upward showing large cover + full title/artist | Moderate complexity — animated SetWindowPos upward, 30fps timer to avoid DWM tearing, expanded DrawMediaPanel pass |

## Resolved — v1.0.0 (Sessions 1–4)

| Item | Description |
|------|-------------|
| Upstream bug 1 | Null dereference on window creation failure |
| Upstream bug 2 | WM_APP collision (APP_WM_CLOSE = WM_APP) |
| Upstream bug 3 | g_Running flag was dead code |
| Upstream bug 4 | SendMediaCommand targeted wrong session |
| Upstream bug 5 | Blocking WinRT calls on message thread |
| Upstream bug 6 | Scroll offset reset raced animation timer |
| Upstream bug 7 | HIWORD/LOWORD mouse coords on multi-monitor |
| Libby blank icon | Open Library Covers API fetch (two-pass, subtitle-strip, exact-match) |
| Libby title strip | Strip "Libby - Open: " prefix to surface just the book title |
| OL false positive | title= field search, exact-match only, no first-result fallback |
| Lock-held-during-IO | Cover fetch now runs outside g_MediaState.lock |
| Wrong-cover revert | Race condition: GetOrFetchCover carries triedCoverIds, updates in-place |
| Cover menu | Right-click: Try Different, Remove, Lock (🔒/🔓), Restore |
| Cover poll race | fetchInProgress guard; mid-fetch title check before committing cover |
| Lock on evicted entry | Creates new locked entry with correct eviction logic |
| Windhawk compliance | .wh.cpp extension, @github profile URL, quoted YAML settings defaults |
| Version shift | Internal v4.x history collapsed to public v1.0.0 (Audiobook Edition) |
