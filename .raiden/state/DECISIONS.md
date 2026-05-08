# Decisions

## 2026-05-08 — Rename source file

**Decision:** Renamed `Source Code.txt` → `taskbar-music-lounge-sd.cpp`.

**Reason:** The file is a C++ Windhawk mod. A `.cpp` extension is required for the
compiler to process it and for tooling to apply correct syntax handling. The original
name was a plain-text label from the upstream source distribution.

## 2026-05-08 — Fork identity

**Decision:** Updated Windhawk mod header fields:
- `@id` → `taskbar-music-lounge-sd`
- `@name` → `Taskbar Music Lounge SD`

**Reason:** The SD fork must not collide with the upstream `taskbar-music-lounge` mod
ID in the Windhawk registry. Changing the ID and name creates a distinct installation
that can coexist with the upstream version.
