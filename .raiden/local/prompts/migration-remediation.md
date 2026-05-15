# Migration Remediation Handoff — TaskbarMusicLoungeAE — Edict v0.4.0 Pre-Migration

## Prompt ID

`raiden.shared.handoff.v1`

## Purpose

TaskbarMusicLoungeAE's RAIDEN v0.2.0 install was previously committed. The v0.4.0
migration (v0.3.0 skipped) was halted because one tracked source file has uncommitted
operator WIP: `taskbar-music-lounge-ae.wh.cpp`. This must be committed or stashed before
the RAIDEN central agent can proceed.

## Template

```text
You are continuing a bounded work package inside the current repo.

Read first:
- AGENTS.md (if present)
- .raiden/instance/metadata.json

Current objective:
- Commit or stash the modified taskbar-music-lounge-ae.wh.cpp so the RAIDEN central
  agent can run the Edict v0.4.0 migration (v0.3.0 skipped).

Known constraints:
- Do NOT modify any file under .raiden/writ/ — these are RAIDEN-managed.
- Do NOT run the workspace audit.
- Commit attribution: no Co-Authored-By or agent attribution lines.

Already true (as of step-2 halt, 2026-05-13):
- RAIDEN v0.2.0 install is committed.
- Dirty tree at halt: M taskbar-music-lounge-ae.wh.cpp (tracked, modified, operator WIP).
- Current branch: main.
- installed_edict_version in metadata.json: 0.2.0.

Still open:
1. Handle taskbar-music-lounge-ae.wh.cpp:
   - If the changes are complete: commit them in a normal operator commit.
   - If the changes are in-progress: stash them (git stash).
2. Verify git status --porcelain is empty.
3. Signal to the operator: TaskbarMusicLoungeAE is ready for the RAIDEN central agent
   to run the v0.4.0 migration prompt from
   /mnt/e/Raiden/toolkit/prompts/audit-protocol-migration-v0.4.0-prompt.md
   targeting --instance /mnt/e/TaskbarMusicLoungeAE. (v0.3.0 skipped; v0.4.0 direct.)

Do not:
- reopen settled naming or architecture
- treat review artifacts as canon unless adopted
- broaden the task beyond clearing the dirty working tree
- run the workspace audit

Close out with:
- result: operator WIP committed or stashed, working tree clean, operator notified
- evidence checked: git status --porcelain empty
- remaining risks: stashed changes should be re-applied after v0.4.0 migration completes
```
