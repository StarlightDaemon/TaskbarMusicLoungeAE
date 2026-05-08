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

## 2026-05-08 — Session 2: Libby cover art, right-click menu, v4.1.0 release

**Agent:** RAIDEN working agent (claude-sonnet-4-6)

### Actions taken

1. Researched Libby/OverDrive cover art options; concluded Open Library Covers API is best no-auth path.
2. Researched Open Library API in depth: rate limits (100 req/5min), coverage (~70% popular titles), caching, stability (15yr uptime), WinHTTP implementation notes.
3. Implemented Open Library cover fetch (f0760f2):
   - Added `-lwinhttp` compiler option, `#include <winhttp.h>`, `#include <algorithm>`
   - `UrlEncodeTitle`: percent-encodes title for URL query param
   - `HttpsGet`: synchronous HTTPS GET via WinHTTP, background-thread safe
   - `FetchOpenLibraryCover`: two-step title→cover_i→JPEG→Bitmap*
   - `GetOrFetchCover`: LRU cache (5 entries) with miss sentinels
   - Cover cache teardown in WM_DESTROY
   - Restructured `UpdateMediaInfo`: cover fetch outside g_MediaState.lock (fixed existing lock-held-during-IO issue)
   - Removed all [THUMB] diagnostic logging
4. Fixed WINHTTP_NO_REQUEST_BODY compile error — not a real WinHTTP macro, replaced with nullptr (4685c7e).
5. Live test: cover appeared but "Leviathan" returned "Leviathan Wakes" (false positive from popularity ranking).
6. Fixed title matching (c5a58f8): switched to `title=` field search, fetch 5 candidates, exact-match selection.
7. v4.1.0 refinements + right-click cover menu (3c59aaa):
   - `StripSubtitle`: strips after `:` or ` - ` before querying OL (handles "Title: Subtitle" audiobook naming)
   - `PickCoverId`: no first-result fallback; accepts stripped/full/subtitle-variant matches; skips tried IDs
   - `OLSearchPass` + `DecodeCoverBytes`: refactored into composable helpers
   - Two-pass fetch: `title=` (precise) then `q=` (broader), both with exclusions
   - `CoverCacheEntry`: added `suppressed`, `locked`, `currentCoverId`, `triedCoverIds`
   - `MediaState`: added `isLibby` flag
   - Right-click menu on cover art (Libby sessions only, art-area hit-test):
     - "Try Different Cover" — background refetch skipping tried cover IDs
     - "Remove Cover" — suppresses cover for this title
     - "🔓 Lock Cover" / "🔒 Unlock Cover" — pins current cover, greys out destructive items
     - "Restore Cover" — clears suppression/lock, re-fetches on next poll
   - @version bumped 4.0.1 → 4.1.0; readme updated with Libby Support bullet
8. Fixed race condition: "Try Different Cover" reverted to wrong cover within 1 second (2239ae2):
   - Root cause: poll timer fired while HandleCoverWrong's thread was mid-fetch; GetOrFetchCover used empty excluded list and fetched wrong cover; whichever thread finished last won
   - Fix 1: GetOrFetchCover carries `triedCoverIds` from fetched=false entry so concurrent poll uses same exclusions
   - Fix 2: GetOrFetchCover updates existing cache entry in-place (not push_back) to prevent duplicate entries
9. Added Lock Cover feature (f4fdafa, b9ff69b):
   - `locked` flag on CoverCacheEntry; GetOrFetchCover returns pinned bitmap immediately when locked
   - Menu item toggles label: "🔓 Lock Cover" → "🔒 Unlock Cover"
   - Locked state greys out Try Different Cover and Remove Cover
10. Updated CURRENT_STATE.md, OPEN_LOOPS.md, WORK_LOG.md, Changelog.txt.

### Commits this session

| Commit  | Description |
|---------|-------------|
| f0760f2 | feat: fetch Libby cover art from Open Library Covers API |
| 4685c7e | fix: WINHTTP_NO_REQUEST_BODY → nullptr |
| c5a58f8 | fix: improve OL title matching (title= search, exact match, 5 candidates) |
| 1241d03 | chore: RAIDEN state mid-session checkpoint |
| 3c59aaa | feat: v4.1.0 — refinements + right-click cover menu |
| 2239ae2 | fix: prevent wrong-cover revert race condition |
| f4fdafa | feat: Lock Cover right-click action |
| b9ff69b | feat: lock/unlock emoji label toggle on Lock Cover item |

### Status at session end

v4.1.0 shipped. All Libby cover art features complete and tested.
Hover-expand panel deferred to v4.2.0. Session closed.
