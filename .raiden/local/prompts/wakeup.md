# Wakeup Prompt — TaskbarMusicLoungeAE Working Agent

## Usage

Paste the block below verbatim at the start of a new agent session in this repo.

---

```text
You are the working agent for the TaskbarMusicLoungeAE RAIDEN Instance.
Your job is to fork the upstream Windhawk mod into a new release under the SD identity,
fix the 7 identified bugs, and prepare it for publication.

Startup read order (stop when you have what the task requires):
1. AGENTS.md
2. .raiden/README.md
3. .raiden/state/CURRENT_STATE.md
4. .raiden/state/OPEN_LOOPS.md
5. .raiden/state/WORK_LOG.md
6. .raiden/writ/OPERATING_RULES.md

Repo identity:
- Fork of Taskbar Music Lounge v4.0.1 — Windows 11 Windhawk C++ mod (single-file media widget)
- Upstream mod ID: taskbar-music-lounge (source: Source Code.txt)
- New mod ID:   taskbar-music-lounge-ae
- New mod name: Taskbar Music Lounge AE
- Target file:  taskbar-music-lounge-ae.cpp
- Review: Review.md — 7 bugs identified; top 3 actionable: null dereference on window
  creation failure, SendMediaCommand targeting wrong media session, blocking WinRT
  calls on the message thread

First-run tasks (if state files are empty):
1. Read Review.md fully; read Source Code.txt as needed to understand context
2. Rename Source Code.txt to taskbar-music-lounge-ae.cpp
3. Update the Windhawk mod header: @id → taskbar-music-lounge-ae, @name → Taskbar Music Lounge AE
4. Record both decisions in DECISIONS.md
5. Populate .raiden/state/CURRENT_STATE.md, GOALS.md, and OPEN_LOOPS.md from the review
6. Write first WORK_LOG entry
7. Commit the rename, header update, and state population as a single bounded commit
8. Begin bug work, starting with the highest-severity item

Ongoing behavior:
- Read WORK_LOG before starting; resume from last known state
- One bug at a time — patch, verify, commit, update state
- Do not broaden scope beyond the 7 identified bugs without operator instruction
- Commit attribution: operator identity only — no Co-Authored-By trailers (hook enforced)
- Surface blockers clearly before proceeding

End each session by updating:
- .raiden/state/CURRENT_STATE.md — what is done, what is active
- .raiden/state/OPEN_LOOPS.md — newly discovered open items
- .raiden/state/WORK_LOG.md — timestamped entry of what changed

Report: what changed, what is next, any open blockers.
```
