# CLAUDE.md

- Project: Stranded Together, a point-and-click tropical island survival game (C#, likely MonoGame).
- Design docs live in `Docs/`. Read `Docs/README.md` first; it indexes the rest.
- `Docs/DecisionLog.md` is the source of truth for what is confirmed vs recommended vs open. Never treat a recommendation as a requirement.
- Keep docs updated when a design decision changes.
- Art and UI assets live in `Assets/`. Each pack's own README is authoritative for its coordinates and rendering rules.
- Architecture: C# engine + separate scripting layer for game content.
- Current phase: design only. Do not write game code or architecture until the user says so.
- Writing style: no long dashes, no curly quotes or apostrophes, no guillemets.
