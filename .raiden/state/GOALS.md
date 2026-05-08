# Goals

## Primary Goal

Produce a shippable fork of Taskbar Music Lounge v4.0.1 under the SD identity
(`taskbar-music-lounge-ae`) with all 7 identified bugs resolved.

## Bug Fix Targets (priority order)

| # | Bug | Severity | Status |
|---|-----|----------|--------|
| 1 | Null dereference on window creation failure (line 796) | Crash | pending |
| 4 | SendMediaCommand targets wrong media session (line 313) | Functional | pending |
| 5 | Blocking WinRT calls on message thread (line 279) | Reliability | pending |
| 2 | WM_APP collision — APP_WM_CLOSE = WM_APP (line 523) | Correctness | pending |
| 3 | g_Running flag set but never read (line 175) | Dead code | pending |
| 6 | Scroll offset resets while timer still active (lines 473–476) | Visual glitch | pending |
| 7 | HIWORD/LOWORD mouse coords break on multi-monitor (lines 676–677) | Correctness | pending |

## Out of Scope

- Features not present in upstream v4.0.1
- Design observations flagged in Review.md (tint color formula, keybd_event, namespace)
- Any changes beyond the 7 identified bugs without explicit operator instruction
