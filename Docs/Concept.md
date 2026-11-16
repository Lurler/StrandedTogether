# Original concept brief

Snapshot of the designer's initial brief. Kept for reference; decisions live in `DecisionLog.md`.

## Concept

A tropical island survival game inspired primarily by The Sims 2: Castaway (Nintendo DS), with additional inspiration from Lost in Blue 1 and 2 (DS) and some modern games.

Central idea: adapt the appeal of Castaway DS into a point-and-click quest with fixed scenes, without directly walking a character around.

The player explores island locations, collects resources, makes tools, improves their living conditions, interacts with survivors, and solves problems to reach new areas. The balance between survival, adventure puzzles, crafting, character interactions and minigames is open.

Goals: meaningful progression, a satisfying island routine, a scope an independent developer can realistically finish.

## Established direction (from the brief)

- Fixed, illustrated island scenes with clickable objects and exits.
- No direct character walking controls.
- Tropical locations: beaches, jungle clearings, creeks, rocky shores, caves, wrecks.
- Collectible objects are separate sprites over backgrounds. Hovering an interactive thing shows a 1 pixel outline.
- Bright, vibrant colors, organic shapes, simple compositions, painted old adventure-game feel.
- Background source art is processed into low-res pixel art using nearest-neighbor resizing.
- Small internal resolution, scaled up to modern screens.
- C#, likely MonoGame; may change depending on UI library needs. Graphics are simple: backgrounds, interactive objects, game UI.

## Ideas (not decisions)

- Time advances through committed actions; inspecting and considering is free. Examine gives a visual-novel style comment from the character. Pick up costs e.g. 10 minutes and affects stats.
- Travel is instant but costs in-game time.
- A visibly improving home camp is a major source of progression.
- Tools unlock new interactions in familiar locations.
- Weather, tides, time of day and resource regeneration create reasons to revisit locations.
- Minigames for activities like spear fishing, catching bugs, etc.
- Start alone, later meet two other characters who join the camp, help with routine tasks, and develop relationships.
- Possible needs: health, hunger, hydration, energy, comfort/sanity, hygiene. Undecided.
- Possible first playable slice: about 6 scenes, a dozen functional items, simple crafting, a few camp upgrades. Estimates, not requirements.
