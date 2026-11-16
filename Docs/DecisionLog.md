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
| C09 | Progression | Spine is gathering, crafting, camp upgrading and exploring. Interactions are resource-cost actions (click river, spend time and energy, get fish). "Use item X on object Y" counts as the puzzle form and is in from the start. No bespoke logic puzzles for now. | 2026-11-16 |
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
| C21 | Needs | Six stats: Health (drained by zero hunger/thirst, injury, illness; restored by sleep, good food, medicine), Energy (time + hard work; restored by sleep, shelter quality matters), Hunger (time; food), Thirst (time, faster, heat; water, fruit), Hygiene (slow decay ~2-3 days + dirty work; bathing, camp wash spot; low gives illness risk and Mood loss), Mood (bad weather, poor sleep, monotonous food, loneliness; shelter, varied food, decor, NPC time; low slows actions, high gives small bonuses). Name "Mood" preferred over "Comfort", not final. | 2026-11-16 |
| C22 | Collapse | Collapse when energy or health hits zero. Hunger/thirst at zero only drain health slowly. | 2026-11-16 |
| C23 | Daily curve | Upkeep dominates early days; camp upgrades reduce it over time, leaving more time for exploring and building. | 2026-11-16 |
| C24 | Onboarding | The opening is orchestrated: only tutorial-relevant actions are available until the player learns the basics, then full freedom. | 2026-11-16 |
| C25 | Camp | Upgrades give concrete effects, e.g. better shelter: higher energy on waking, less affected by bad weather while sleeping, new interactions. | 2026-11-16 |
| C26 | Goal | Quest structure: explore the whole island by finding and crafting the right items. Once everything is unlocked, a way off the island appears and finishing it wins the game. No deadline. | 2026-11-16 |
| C27 | Interaction | LMB on anything opens a context menu of currently possible actions with time cost. RMB closes any menu (context or opened panel like inventory) and cancels states (e.g. item held on cursor). Examine is free. | 2026-11-16 |
| C28 | Tools | Owned tools add their actions to object menus automatically (e.g. "Chop" on a tree with an axe). Used for almost everything. | 2026-11-16 |
| C29 | Item use | Manual item use exists for specific unhinted cases: inventory, LMB item, "Use on", inventory closes, item icon attaches to cursor, click a world object. RMB cancels. | 2026-11-16 |
| C30 | Travel | Small arrows in scenes lead to neighbor scenes. Island map allows travel to any previously explored scene; costs in-game time, shown on hover. | 2026-11-16 |
| C31 | Gating | All gate types available (tool, item, environment, NPC). Each gate is individually scripted per interaction. | 2026-11-16 |
| C32 | Environment | Day/night and rain/storms everywhere via grading and overlays. Rain affects water collection, fire and Mood. No seasons. | 2026-11-16 |
| C33 | Tides | Binary (high/low), only in specific scenes. Rolled per day: on some days a location is at low tide. Waiting in the scene does not change it. | 2026-11-16 |
| C34 | Sleep | Mostly at camp. Several camps can be built across the island over the game (e.g. basic beach camp early, better inland camp later). Extra rest spots can be added if needed. | 2026-11-16 |
| C35 | Minigames | No minigames for now (changed from the brief). May be considered later. | 2026-11-16 |
| C36 | Camps | Multiple camps allowed (art cost not a concern; AI generated). Old camps stay usable. NPCs can be brought to a new camp and told to stay there. | 2026-11-16 |
| C37 | Inventory | Unlimited inventory. No camp storage (adventure game, not micromanagement). | 2026-11-16 |
| C38 | Recipes | Learned from triggers (holding ingredients, examining things gives the character an idea). Exact rules to be formalized later. | 2026-11-16 |
| C39 | Crafting | Crafting happens at specific objects: e.g. flat stone for simple crafting, campfire for cooking. Such objects can exist anywhere in the world, and the player can also build them at assigned spots. | 2026-11-16 |
| C40 | Durability | Nothing wears out. No durability, no consumable tools. | 2026-11-16 |
| C41 | Food | Raw food plus fixed cooking recipes unlocked over time. Raw meat/fish carries illness risk. Food variety raises Mood. No spoilage. | 2026-11-16 |
| C42 | Water | Drink directly at the creek. Carrying water needs a bottle. Rain catcher fills itself when it rains. | 2026-11-16 |
| C43 | Resources | Three types: unique one-time items; renewable nodes (respawn when enough time has passed and the player sleeps); unlimited activity spots (each attempt costs time, random result). All scripted per node. | 2026-11-16 |

## Recommended (pending)

Round 5 (camp building) recommendations are in the conversation.

## Open

- Release timeline / milestones (not answered yet, not blocking).
- Resolutions: designer will supply specs with the art.
- Exact list of "safe to lose" items.
- Exact stat effects and decay rates.
- How NPCs are useful without separate storage/inventory.
- Balance: with unlimited inventory and no spoilage, upkeep is limited only by production rate (node regrowth, time per attempt). Needs tuning so stockpiling does not erase the C23 curve.
- Camp upgrades and their effect on daily play.
- Objectives, story beats, ending.
- NPCs, relationships, contributions.
- Character presentation and action feedback without walking.
- UI, accessibility, saving.
- Content scope and asset workload.
