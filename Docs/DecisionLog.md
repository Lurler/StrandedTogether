# Decision log

Three categories, kept strictly separate:
- **Confirmed** - the designer agreed. Binding until changed here.
- **Recommended** - proposed by Claude, awaiting the designer's answer. Not binding.
- **Open** - unresolved questions.

Working title: **Stranded Together** (from the repository name; not confirmed as final).

## Confirmed

| ID | Area | Decision | Date |
|---|---|---|---|
| C01 | Genre | Point-and-click survival/adventure with fixed illustrated scenes. No direct character walking. | 2026-11-16 |
| C02 | Setting | Tropical island: beaches, jungle clearings, creeks, rocky shores, caves, wrecks. | 2026-11-16 |
| C03 | Art | Painted source art downscaled to low-res pixel art (nearest neighbor). Bright, vibrant, organic, simple compositions. | 2026-11-16 |
| C04 | Art | Interactive objects are separate sprites over backgrounds; 1 px outline on hover. | 2026-11-16 |
| C05 | Tech | Small internal resolution scaled up to the screen. C#; MonoGame likely but not locked (depends on UI approach). | 2026-11-16 |
| C06 | Phase | Design first. No code or architecture until the design is settled. | 2026-11-16 |
| C07 | Experience | Cozy castaway (Castaway DS style). Survival is real but forgiving; fun comes from routine, camp growth and people. Only a tiny mystery element as exploration flavor. | 2026-11-16 |
| C08 | Failure | No death. On collapse, something bad but harmless happens (e.g. monkeys steal some items, Castaway DS style). Only items flagged "safe to lose" can be taken. Stats drop, but critically low stats are reset to relatively safe values so play continues. | 2026-11-16 |
| C09 | Progression | Spine is gathering, crafting, camp upgrading and exploring. No adventure puzzles in the initial version; interactions are resource-cost actions (click river, spend time and energy, get fish). Real puzzles may be considered later during polish. | 2026-11-16 |
| C10 | Length | Target at least 5 hours of gameplay. Exact length open; Castaway DS (about 12 h) is a loose reference. | 2026-11-16 |
| C11 | Platform | PC, Windows first, Steam-ready. Mouse only, keyboard shortcuts as extras. No gamepad or touch in v1. | 2026-11-16 |
| C12 | Resolution | Two layers: backgrounds at very low res (320x180 or 480x270), UI at twice the background res. Both pixelated. | 2026-11-16 |
| C13 | Scaling | Point filtering only, no smoothing. Window resize adds black bars (letterbox/pillarbox). | 2026-11-16 |
| C14 | Art source | All art generated in ChatGPT and supplied by the designer. | 2026-11-16 |
| C15 | Business | Commercial project. | 2026-11-16 |

## Recommended (pending)

| ID | Area | Recommendation |
|---|---|---|
| R01 | Resolution | Scene layer 320x180, UI layer 640x360. Whole-number scaling to 720p, 1080p, 1440p, 4K. 480x270/960x540 leaves big bars at 1440p. |
| R02 | Resolution | Item sprites, hover outline and characters drawn at scene resolution; text and UI at UI resolution. |
| R03 | Art pipeline | Avoid needing multiple versions of one background. Time of day and weather via palette tint and overlays; camp upgrades as overlay sprites on a fixed camp background. |
| R04 | Business | Steam requires disclosing AI-generated content on the store page; plan for it. |

Round 2 (time, needs, daily loop) recommendations are in the conversation; they get logged here once answered.

## Open

- Release timeline / milestones (not answered yet, not blocking).
- Final background resolution: 320x180 vs 480x270 (see R01).
- What triggers collapse, and exact list of "safe to lose" items.
- Time model, action costs, needs list, day/night.
- Scene interaction, travel, environment changes (weather, tides).
- Inventory, tools, crafting, food, resource renewal.
- Camp upgrades and their effect on daily play.
- Objectives, story beats, ending.
- NPCs, relationships, contributions.
- Minigames.
- Character presentation and action feedback without walking.
- UI, accessibility, saving.
- Content scope and asset workload.
