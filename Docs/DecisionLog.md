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
| C16 | Art | Time of day, weather (rain etc.) done via color grading and effects, never redrawn backgrounds. Camp upgrades as overlays on a fixed camp background. | 2026-11-16 |
| C17 | Art specs | Exact resolutions and layer specs will be supplied by the designer together with the art. | 2026-11-16 |
| C18 | Time | Free clock. The player can do anything at any hour, but without rest they eventually pass out. Resting is arranged so the player wakes up in the morning where possible. | 2026-11-16 |
| C19 | Action cost | Tiers: free (examine, inventory, browsing crafting, eating carried food, talking), quick ~5-10 min (pick up loose item), work 30-120 min (harvest, fish, chop, craft, build), travel 10-30 min by distance. | 2026-11-16 |
| C20 | Action cost | Stats deplete with time by default. Specific actions can define extra stat costs (hard work: extra energy; dirty work: extra hygiene). | 2026-11-16 |
| C21 | Needs | Six stats from the brief: health, hunger, hydration, energy, comfort/sanity, hygiene (exact roles pending, see R05). | 2026-11-16 |
| C22 | Collapse | Collapse when energy or health hits zero. Hunger/thirst at zero only drain health slowly. | 2026-11-16 |
| C23 | Daily curve | Upkeep dominates early days; camp upgrades reduce it over time, leaving more time for exploring and building. | 2026-11-16 |
| C24 | Onboarding | The opening is orchestrated: only tutorial-relevant actions are available until the player learns the basics, then full freedom. | 2026-11-16 |
| C25 | Camp | Upgrades give concrete effects, e.g. better shelter: higher energy on waking, less affected by bad weather while sleeping, new interactions. | 2026-11-16 |
| C26 | Goal | Quest structure: explore the whole island by finding and crafting the right items. Once everything is unlocked, a way off the island appears and finishing it wins the game. No deadline. | 2026-11-16 |

## Recommended (pending)

| ID | Area | Recommendation |
|---|---|---|
| R05 | Needs | Roles for the six stats: Hunger/Thirst/Energy/Health as survival core; Hygiene slow-decay (about 2-3 days), low causes illness risk and Mood loss; Comfort/Sanity as "Mood", driven by shelter, food variety, weather, company, decor; low Mood makes actions slower, high Mood gives small bonuses. |

Round 3 (scenes, travel, exploration, environment) recommendations are in the conversation.

## Open

- Release timeline / milestones (not answered yet, not blocking).
- Resolutions: designer will supply specs with the art.
- What triggers collapse, and exact list of "safe to lose" items.
- Exact stat effects and decay rates.
- Where sleeping is allowed outside camp.
- Scene interaction, travel, environment changes (weather, tides).
- Inventory, tools, crafting, food, resource renewal.
- Camp upgrades and their effect on daily play.
- Objectives, story beats, ending.
- NPCs, relationships, contributions.
- Minigames.
- Character presentation and action feedback without walking.
- UI, accessibility, saving.
- Content scope and asset workload.
