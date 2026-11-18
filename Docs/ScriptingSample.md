# Scripting sample

Illustration of the intended script shape (C73-C84). Not a final API: names, signatures and helpers will be settled during implementation.

## Scenario

First beach scene. Three items lie on the sand. Each has an examine line and a comment on pickup. When all three are picked up, the character comments and the path inland appears.

## Conventions shown here

- **No magic strings.** Items, scenes, flags and faces are types or enums. Assets and hotspot masks are found by convention from namespace and class/field names.
- **Items live in their own folder**, one class per item. Assets are picked up automatically from the matching asset folder.
- **Costs come from per-entity-type defaults** and are overridden only when a specific case needs it.

## Folder layout (game project)

```
Game/
  Flags.cs
  Defaults.cs
  Items/
    Containers/EmptyBottle.cs
    Food/Coconut.cs
    Materials/Twigs.cs
  Scenes/
    Beach1.cs
Assets/
  Items/
    Containers/EmptyBottle/  icon.png  ground.png  ground_shadow.png  shallow.png
    Food/Coconut/            icon.png  ground.png  ground_shadow.png  hanging.png
    Materials/Twigs/         icon.png  ground.png  ground_shadow.png
  Scenes/
    Beach1/                  background.png  ToJungleEdge.png (hotspot mask)
```

- Namespace `Game.Items.Food` + class `Coconut` maps to `Assets/Items/Food/Coconut/`.
- `icon.png` is the inventory icon (UI grid). Placement sprites (`ground`, `shallow`, `hanging`, ...) are world-grid sprites for different positions in a scene; an item has only the placements it has files for.
- The placement sprite is chosen automatically from the item's position in the scene (C84) and can be overridden with `Placement = ...`.
- Scene `Beach1` maps to `Assets/Scenes/Beach1/`. A hotspot mask file is named after the field that uses it (`ToJungleEdge.png`).

## Items

```csharp
namespace Game.Items.Containers;

public class EmptyBottle : Item
{
    public override string Name    => "Empty bottle";
    public override string Examine => "A glass bottle. Empty, but not cracked.";
}
```

```csharp
namespace Game.Items.Food;

public class Coconut : Item
{
    public override string Name       => "Coconut";
    public override string Examine    => "A coconut. Hard shell, sweet inside.";
    public override bool   SafeToLose => true;
}
```

```csharp
namespace Game.Items.Materials;

public class Twigs : Item
{
    public override string Name       => "Twigs";
    public override string Examine    => "Dry twigs. They would burn nicely.";
    public override bool   SafeToLose => true;
}
```

## Flags

```csharp
namespace Game;

public enum Flags
{
    Beach1Searched,
    // ...
}
```

## Per-type defaults

```csharp
namespace Game;

public static class Defaults
{
    public static readonly Cost Pickup = new(minutes: 5);
    public static readonly Cost Exit   = new(minutes: 15);
    // Harvest, Build, Craft, ...
}
```

Defaults live in the game project, not the engine, so the engine stays game-agnostic.

## Scene script

```csharp
namespace Game.Scenes;

public class Beach1 : Scene
{
    // Waking up on the beach
    public override async Task OnEnter()
    {
        if (FirstVisit)
        {
            await Say(Face.Tired, "Ugh... my head. Sand everywhere.");
            await Say("Nobody else on this beach. Let's see what washed up.");
        }
    }

    // Empty bottle in the shallow water (shallow sprite picked automatically from position)
    public Pickup<EmptyBottle> Bottle = new()
    {
        Position = (230, 109),
        OnPickup = async () => await Say(Face.Happy, "Something to carry water in. Lucky."),
    };

    // Coconut under the palm, with a scene-specific examine line
    public Pickup<Coconut> Coconut = new()
    {
        Position = (186, 132),
        Examine  = async () => await Say("Fell from the palm above. Breakfast, if I can get it open."),
        OnPickup = async () => await Say("Heavy. Good. Heavy means full."),
    };

    // Twigs tangled in seaweed, slower than a normal pickup
    public Pickup<Twigs> Twigs = new()
    {
        Position = (125, 102),
        Cost     = new(minutes: 15),
        OnPickup = async () => await Say("If I can get a fire going, at least the nights won't be pitch black."),
    };

    // Path inland, appears once the beach is searched
    public Exit<JungleEdge> ToJungleEdge = new()
    {
        Visible = () => IsSet(Flags.Beach1Searched),
    };

    // Runs after every completed action in this scene
    public override async Task AfterAction()
    {
        if (IsSet(Flags.Beach1Searched) || !AllPicked(Bottle, Coconut, Twigs))
            return;

        Set(Flags.Beach1Searched);
        await Say("That's everything on this stretch of sand.");
        await Say("The trees up north look thick, but there's a gap. Worth a look.");
    }
}
```

## What the engine does without being told

- `Pickup<T>` adds the shared "Pick up" action (C77), charges `Defaults.Pickup` unless `Cost` is set, runs the progress circle (C63), adds the item, shows the gain indication (C64), hides the sprite and saves that it was picked.
- Placement is chosen from position unless overridden; default examine is the item's own `Examine` text.
- `Exit<T>` uses `Defaults.Exit` unless overridden, and its hotspot mask is found by field name.
- Hover outline, context menu, Examine, stat decay over spent time.
- `Visible = false` objects and exits are neither drawn nor clickable.

## Derived localization keys (C78, C80)

```
Items.Containers.EmptyBottle.Name    = "Empty bottle"
Items.Containers.EmptyBottle.Examine = "A glass bottle. Empty, but not cracked."
Scenes.Beach1.OnEnter.1              = "Ugh... my head. Sand everywhere."
Scenes.Beach1.Bottle.OnPickup        = "Something to carry water in. Lucky."
Scenes.Beach1.Coconut.Examine        = "Fell from the palm above. Breakfast, if I can get it open."
Scenes.Beach1.AfterAction.1          = "That's everything on this stretch of sand."
```

A Russian file overrides only what it translates:

```
Items.Containers.EmptyBottle.Examine = "Стеклянная бутылка. Пустая, но целая."
```

---

# Scene 2: JungleEdge (first camp)

Reached from Beach1. Flat clearing at the jungle edge with two build spots (shelter, campfire), a renewable palm, a few pickups, and three exits: back to the beach, east to the creek (opens after the shelter is built, tutorial pacing C24), and north into the jungle (blocked by vines until the player has a machete).

## New supporting definitions

```csharp
namespace Game;

public enum Flags
{
    Beach1Searched,
    ShelterBuilt,
}

public enum Notes
{
    BuildShelter,
    VinesNorth,
}

// Shared action labels, written once (C77)
public static class Labels
{
    public static readonly Label PickUp = "Pick up";
    public static readonly Label Gather = "Gather";
    public static readonly Label Build  = "Build";
    public static readonly Label Sleep  = "Sleep";
    public static readonly Label Cook   = "Cook";
    public static readonly Label Cut    = "Cut through";
}

public static class Defaults
{
    public static readonly Cost Pickup = new(minutes: 5);
    public static readonly Cost Gather = new(minutes: 20);
    public static readonly Cost Exit   = new(minutes: 15);
    public static readonly Cost Build  = new(minutes: 120, energy: 20);
}
```

## Buildables

A buildable is a chain of levels. Each level is its own class with its own overlay sprite, found by convention (`Game.Buildables.Shelters.LeanTo` -> `Assets/Buildables/Shelters/LeanTo/sprite.png`). Interfaces on a level add standard actions automatically: `IRestSpot` adds "Sleep", `ICraftingStation<T>` adds "Cook" (or the station's label) and opens the crafting menu.

```csharp
namespace Game.Buildables;

// Shelter chain: lean-to, then hut
public class Shelter : Buildable
{
    public override BuildLevel[] Levels => [new LeanTo(), new Hut()];
}

// Campfire chain: a single level for now
public class Campfire : Buildable
{
    public override BuildLevel[] Levels => [new FirePit()];
}
```

```csharp
namespace Game.Buildables.Shelters;

public class LeanTo : BuildLevel, IRestSpot
{
    public override string Name      => "Lean-to";
    public override string Examine   => "Fronds over a frame of sticks. Home, for now.";
    public override Need[] Materials => [Need<PalmFrond>(3), Need<Twigs>(1)];
    public float SleepQuality        => 0.6f;
}

public class Hut : BuildLevel, IRestSpot
{
    public override string Name      => "Hut";
    public override string Examine   => "Walls, a roof, and almost no leaks.";
    public override Need[] Materials => [Need<Bamboo>(8), Need<Rope>(2), Need<PalmFrond>(6)];
    public override bool   Requires  => Has<StoneAxe>();
    public float SleepQuality        => 0.85f;
}
```

```csharp
namespace Game.Buildables.Campfires;

public class FirePit : BuildLevel, ICraftingStation<CookingRecipes>
{
    public override string Name      => "Fire pit";
    public override string Examine   => "A ring of stones and a small, stubborn fire.";
    public override Need[] Materials => [Need<Rock>(3), Need<Twigs>(2)];
    public override Cost   Cost      => new(minutes: 60, energy: 10);
}
```

## Scene script

```csharp
namespace Game.Scenes;

public class JungleEdge : Scene
{
    // First arrival: the tutorial points at the shelter
    public override async Task OnEnter()
    {
        if (FirstVisit)
        {
            await Say("Shade, flat ground, and the beach close by. Good spot for a camp.");
            await Say(Face.Worried, "The sun's already dropping. I need some kind of shelter before dark.");
            Journal.Add(Notes.BuildShelter, "Build a shelter at the jungle edge before nightfall.");
        }
    }

    // ---------- Build spots ----------

    // Flat ground under the big palm, room for a shelter
    public BuildSpot<Shelter> ShelterSpot = new()
    {
        Position = (205, 96),
        Examine  = async () => await Say("Flat, dry, shaded. A shelter would fit right here."),
        OnBuilt  = async level =>
        {
            if (level is LeanTo)
            {
                Set(Flags.ShelterBuilt);
                Journal.Resolve(Notes.BuildShelter);
                await Say(Face.Happy, "Not pretty, but it'll keep the dew off.");
            }
        },
    };

    // Bare sand away from the trees, safe for a fire
    public BuildSpot<Campfire> FireSpot = new()
    {
        Position = (140, 128),
        Examine  = async () => await Say("Bare sand, nothing nearby to catch fire. Good place for a campfire."),
        OnBuilt  = async level => await Say("Fire. Real fire. I could kiss these rocks."),
    };

    // ---------- Resources ----------

    // Young palm by the clearing, fronds grow back
    public Renewable<PalmFrond> Palm = new()
    {
        Max     = 3,
        Regrow  = 1,   // per sleep, once enough time has passed (C43)
        Examine = async () => await Say("A young palm. The lower fronds are within reach."),
        OnEmpty = async () => await Say("That's all the fronds I can reach. They'll grow back."),
    };

    // Rocks scattered at the edge of the clearing
    public Pickup<Rock> Rocks = new()
    {
        Position = (62, 142),
        Quantity = 3,
        OnPickup = async () => await Say("Good size. A ring of these would keep a fire in place."),
    };

    // Dead branches under the bushes
    public Pickup<Twigs> DeadBranches = new()
    {
        Position = (250, 150),
        Quantity = 2,
    };

    // Coconut wedged in a low bush, sprite forced to the hanging variant
    public Pickup<Coconut> BushCoconut = new()
    {
        Position  = (282, 88),
        Placement = Placement.Hanging,
        Examine   = async () => await Say("A coconut stuck in the bush. Someone up there likes me."),
    };

    // ---------- Exits ----------

    // Back to the beach
    public Exit<Beach1> ToBeach = new();

    // East to the creek, opens once the camp is started (tutorial pacing)
    public Exit<Creek> ToCreek = new()
    {
        Cost    = new(minutes: 25),
        Visible = () => IsSet(Flags.ShelterBuilt),
    };

    // Overgrown path north, needs a blade
    public BlockedExit<DeepJungle> ToDeepJungle = new()
    {
        Examine = async () =>
        {
            await Say("Vines as thick as my wrist. I'm not getting through with bare hands.");
            Journal.Add(Notes.VinesNorth, "Find something sharp to cut the vines north of camp.");
        },
        Unblock = new()
        {
            Label    = Labels.Cut,
            Requires = () => Has<Machete>(),
            Cost     = new(minutes: 45, energy: 15),
            Run      = async () =>
            {
                Journal.Resolve(Notes.VinesNorth);
                await Say("Took forever, but there's a path now.");
            },
        },
    };
}
```

## What the engine does without being told

- `BuildSpot<T>` shows an empty spot (hotspot mask by field name), offers "Build <next level>" with its materials when the player has them and `Requires` passes, charges `Defaults.Build` or the level's own cost, consumes materials, draws the level's overlay sprite, saves the level, and adds actions from the level's interfaces (Sleep, Cook).
- `Renewable<T>` adds "Gather" with `Defaults.Gather`, tracks the remaining count and regrowth in saved state.
- `BlockedExit<T>` is a visible exit that cannot be used until its `Unblock` action succeeds; afterwards it behaves as a normal exit. Its blocked/unblocked state is saved without a flag.
- `Pickup<T>` with `Quantity` gives that many units in one action.

## Tutorial check

Beach1 gives 1 twig. JungleEdge gives 3 fronds, 3 rocks and 2 twigs. Lean-to needs 3 fronds + 1 twig; fire pit needs 3 rocks + 2 twigs. Both can be built on day 1 with what these two scenes provide.
