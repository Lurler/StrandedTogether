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
| C12 | Resolution | World canvas 320x180 (backgrounds, props), drawn 2x into a 640x360 UI canvas. Details in `Docs/UI.md`. | 2026-11-16 |
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
| C27 | Interaction | LMB on anything opens a context menu of currently possible actions (costs not shown, see C69). RMB closes any menu (context or opened panel like inventory) and cancels states (e.g. item held on cursor). Examine is free. | 2026-11-16 |
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
| C44 | Building | Each camp scene has fixed build spots. Clicking one shows buildable options with materials and time. Each spot has a fixed upgrade chain (one overlay sprite per level). | 2026-11-16 |
| C45 | Upgrades | Categories: shelter, fire/cooking, crafting station, water, food production, Mood/decor. Every upgrade has one clear gameplay effect (decor: Mood). Specific items decided when scripting scenes. | 2026-11-16 |
| C46 | Passive production | A small number (3-5 in the game) of upgrades produce resources over time, collected via the normal context menu. | 2026-11-16 |
| C47 | Camps | Each camp has unique build options suited to its location, plus shared basics. Specific options decided later. | 2026-11-16 |
| C48 | Damage | Weather never damages the camp. One-off scripted events are allowed if the story needs them. | 2026-11-16 |
| C49 | Gating | Some camp upgrades are required for main progression; the rest are quality of life. | 2026-11-16 |
| C50 | Balance | Stockpiling handled by tuning; activity spots may also deplete after a few attempts (per-spot scripting). | 2026-11-16 |
| C51 | Protagonist | Fixed male protagonist with a name and background. No protagonist exposition; the player drives all actions. Exposition is for NPCs only. | 2026-11-16 |
| C52 | NPCs | The two survivors are female castaways. Light romance element between them and the protagonist. | 2026-11-16 |
| C53 | Objectives | When the player finds a clear blocker, the character writes a journal note (quest task). It is removed when resolved. Purpose: help players keep track. | 2026-11-16 |
| C54 | Hints | Main-path blockers are always deducible (e.g. vines + machete); exact hinting method designed later, not too obvious. Some optional content can stay fully secret. | 2026-11-16 |
| C55 | Story | Rough four-chapter shape: alone on the beach (tutorial, first camp); jungle (first tools, first survivor); interior (second camp, second survivor); escape. Details later. | 2026-11-16 |
| C56 | Cutscenes | Important milestones can use mini cutscenes: a background image with text over it. | 2026-11-16 |
| C57 | Tone | Mostly light, with a few sincere moments, mainly in NPC scenes. | 2026-11-16 |
| C58 | Romance | Both relationships progress independently during play, non-exclusive. A short ending epilogue reflects the closer relationship; only the epilogue branches. | 2026-11-16 |
| C59 | NPC needs | NPCs have no simulated needs and never collapse. Occasional requests can read like needs. | 2026-11-16 |
| C60 | NPC travel | NPCs normally stay at their camp. At a specific progression block requiring a second person, the player can ask one to accompany them there; afterwards she goes back. | 2026-11-16 |
| C61 | NPC joining | Survivors join via scripted story events (found in a scene, help needed, then joins). Specifics decided with the story. | 2026-11-16 |
| C62 | Protagonist art | Protagonist never appears in scenes or cutscenes. He has a VN-style dialogue portrait with a few expressions (e.g. neutral, happy, tired, disgusted, surprised). | 2026-11-16 |
| C63 | Action feedback | Simple actions (no window) show a progress circle that appears for about a second, fills, and the action completes. Complex actions like crafting use separate menus. | 2026-11-16 |
| C64 | Result feedback | Gained items get a visible indication (e.g. "+3 Wood"); placement decided when building the UI. No numeric stat change popups, but some up/down indication per stat may be needed. Rare or important results get a character comment. | 2026-11-16 |
| C65 | Stat warnings | Bars change color or flash at thresholds; the character comments once per threshold crossing. | 2026-11-16 |
| C66 | Transitions | Scene change probably a quick fade (about 0.2-0.3 s); to be tuned by experiment. | 2026-11-16 |
| C67 | Architecture | Two parts: a scripting layer for game content and an engine written in C#. Scripting language not chosen yet. | 2026-11-17 |
| C68 | HUD | Designer-supplied UI pack (`Assets/UIPack/`, summary in `Docs/UI.md`): bamboo side rails, Health/Food/Water left, Rest/Hygiene/Comfort right, 24h clock bottom-left, Inventory/Map/Skills buttons right, Menu bottom-left. | 2026-11-17 |
| C69 | Costs | Action costs are not shown to the player; they learn by playing. | 2026-11-17 |
| C70 | Saving | Autosave only, one slot. | 2026-11-17 |
| C71 | Localization | Many languages at release; English only during development. All text externalized and localization-ready from the start. | 2026-11-17 |
| C72 | Inventory UI | Inventory panel art will be supplied by the designer later. | 2026-11-17 |
| C73 | Scripting | Not pure data-driven scene definitions. Real scripting: the entire game progression is implemented in high-level scripts using an API exposed by the engine. Language and API shape in discussion. | 2026-11-18 |

## Recommended (pending)

Round 10 (technical setup and scope) recommendations are in the conversation.

## Open

- Release timeline / milestones (not answered yet, not blocking).
- Exact list of "safe to lose" items.
- Exact stat effects and decay rates.
- Story details, escape method, post-ending, mystery content: deferred until a prototype exists.
- NPC relationship growth mechanics and NPC usefulness (ideas in conversation round 7: talking, gifts, requests, levels; daily jobs, specialty, two-person tasks, company). Deferred.
- "Skills" HUD button: no skill system is defined yet. What is it?
- Where the journal (C53) is opened from; HUD has no journal button.
- Does map hover still show travel time (C30) given C69?
- Autosave triggers (when exactly).
- Release languages (affects font choice; CJK is hard at 640x360).
- World safe zone under bamboo rails (proposed x 25..294).
- Options/accessibility: deferred.
- Content scope and asset workload.
