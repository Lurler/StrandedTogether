# Scripting sample

Illustration of the intended script shape (C73-C90). Not a final API: names, signatures and helpers will be settled during implementation.

## Layers

**Engine** (game-agnostic, no gameplay rules):
- Scenes, scene objects (sprite or hotspot), exits, actions (label, duration, condition, handler).
- `Visible` / condition hooks on every object, exit and action. The engine evaluates them; it never knows why.
- Game clock and time events, saved state store (any enum flags/counters), autosave, localization.
- Rendering, hover outline, context menu, progress circle, dialogue box with portraits, fades.
- Standard UI panels the game fills with data: inventory grid, list panel, requirements panel (C86), notes list.
- Sprite variant selection by vertical position (C87).

**Game project** (rules + definitions + content):
- Survival rules: stats, decay, collapse, sleep, theft.
- Items, recipes, buildables, journal notes, flags, faces.
- Higher-level templates built from engine primitives: `Pickup<T>`, `Renewable<T>`, `BuildSpot<T>`, `Station<T>`.
- `Balance.cs`: every tunable number, including default costs (C90).
- Scenes.

Rule: if the engine would need to know what "building", "sleeping" or "hunger" means, it belongs in the game project.

## Folder layout (game project)

```
Game/
  Balance.cs
  Flags.cs
  Notes.cs
  Labels.cs
  Templates/      Pickup.cs  Renewable.cs  BuildSpot.cs  Station.cs
  Rules/          Stats.cs  Sleep.cs  Collapse.cs
  Items/
    Containers/EmptyBottle.cs
    Food/Coconut.cs
    Materials/Twigs.cs
  Buildables/
    Shelter.cs
    Campfire.cs
  Scenes/
    Beach1.cs
    JungleEdge.cs
Assets/
  Items/
    Food/Coconut/      icon.png  near.png  middle.png  far.png  hanging.png  (+ *_shadow.png where needed)
  Buildables/
    Shelters/LeanTo/   sprite.png
  Scenes/
    Beach1/            background.png  <hotspot masks named after fields>
```

- Namespace + class name maps to the asset folder (`Game.Items.Food.Coconut` -> `Assets/Items/Food/Coconut/`).
- `icon.png` is the inventory icon (UI grid). `near`, `middle`, `far` are world sprites at different depths; `hanging` and other special variants are only used by manual override. An item only has the variants it has files for.
- Variant is chosen automatically from the object's vertical position (C87) and can be overridden with `Placement = ...`.

## Balance.cs: where defaults live

```csharp
namespace Game;

// Every tunable number in one file
public static class Balance
{
    public static class Costs
    {
        public static readonly Cost Pickup = new(minutes: 5);
        public static readonly Cost Gather = new(minutes: 20);
        public static readonly Cost Travel = new(minutes: 15);
        public static readonly Cost Build  = new(minutes: 120, energy: 20);
        public static readonly Cost Craft  = new(minutes: 30);
    }

    public static class Sleep
    {
        public const int WakeHour = 6;
    }
}
```

Templates read their default from here; any instance can set its own `Cost`:

```csharp
namespace Game.Templates;

public class Pickup<T> : SceneObject where T : Item, new()
{
    public Cost Cost     { get; init; } = Balance.Costs.Pickup;
    public int  Quantity { get; init; } = 1;
    // builds a normal engine action from these values
}
```

## Items

```csharp
namespace Game.Items.Food;

public class Coconut : Item
{
    public override string Name       => "Coconut";
    public override string Examine    => "A coconut. Hard shell, sweet inside.";
    public override bool   SafeToLose => true;
}
```

## Flags, notes, labels

```csharp
public enum Flags { Beach1Searched, ShelterBuilt, VinesCut }

public enum Notes { BuildShelter, VinesNorth }

// Shared action labels, written once (C77)
public static class Labels
{
    public static readonly Label PickUp = "Pick up";
    public static readonly Label Gather = "Gather";
    public static readonly Label Sleep  = "Sleep";
    public static readonly Label Cut    = "Cut through";
}
```

## Scene 1: Beach1

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

    // Empty bottle near the waterline
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

    // Path inland, hidden until the beach is searched
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

## Buildables (game project)

A buildable is a chain of levels. Each level is a class with its overlay sprite found by convention. Anything a level lets you do (like sleeping) is an ordinary action it declares; there are no marker interfaces.

```csharp
namespace Game.Buildables;

public class Shelter : Buildable
{
    public override BuildLevel[] Levels => [new LeanTo(), new Hut()];
}
```

```csharp
namespace Game.Buildables.Shelters;

public class LeanTo : BuildLevel
{
    public override string Name      => "Lean-to";
    public override string Examine   => "Fronds over a frame of sticks. Home, for now.";
    public override Need[] Materials => [Need<PalmFrond>(3), Need<Twigs>(1)];
    public override Act[]  Actions   => [Sleep.Action(quality: 0.6f)];
}
```

`Sleep.Action` is a game helper in `Rules/Sleep.cs` that returns a normal action. The engine sees only "an action labelled Sleep with a handler".

## Standard UI for building and crafting (C86)

The engine has one generic **requirements panel**: picture, name, description, a list of required entries (icon + have/need), an optional blocking reason, and a confirm button with a label. It knows nothing about building or crafting.

- `BuildSpot<T>` (game) fills it with the next level's sprite, materials and "Build".
- `Station<T>` (game) opens the engine's list panel with known recipes; picking one opens the requirements panel with "Craft" / "Cook".

```csharp
// Inside the game's BuildSpot<T> template
await Ui.Requirements(new()
{
    Picture  = next.Sprite,
    Title    = next.Name,
    Text     = next.Examine,
    Entries  = next.Materials.Select(m => new Entry(m.Item.Icon, Have: Count(m.Item), Need: m.Amount)),
    Blocked  = next.Requires ? null : next.RequiresHint,
    Confirm  = Labels.Build,
    OnConfirm = () => BuildNext(),
});
```

## Scene 2: JungleEdge (first camp)

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

    // Young palm by the clearing, about 1.3 fronds per day on average, randomized (C89)
    public Renewable<PalmFrond> Palm = new()
    {
        Max     = 3,
        PerDay  = 1.3,
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

    // ---------- Vines blocking the north path ----------

    // The vines are their own object; cutting them sets a flag
    public SceneObject Vines = new()
    {
        Visible = () => !IsSet(Flags.VinesCut),
        Examine = async () =>
        {
            await Say("Vines as thick as my wrist. I'm not getting through with bare hands.");
            Journal.Add(Notes.VinesNorth, "Find something sharp to cut the vines north of camp.");
        },
        Actions =
        [
            new()
            {
                Label = Labels.Cut,
                When  = () => Has<Machete>(),
                Cost  = new(minutes: 45, energy: 15),
                Run   = async () =>
                {
                    Set(Flags.VinesCut);
                    Journal.Resolve(Notes.VinesNorth);
                    await Say("Took forever, but there's a path now.");
                },
            },
        ],
    };

    // ---------- Exits ----------

    // Back to the beach
    public Exit<Beach1> ToBeach = new();

    // East to the creek, hidden until the shelter is built (tutorial pacing, game decision)
    public Exit<Creek> ToCreek = new()
    {
        Cost    = new(minutes: 25),
        Visible = () => IsSet(Flags.ShelterBuilt),
    };

    // North into the jungle, a plain exit that appears once the vines are cut
    public Exit<DeepJungle> ToDeepJungle = new()
    {
        Visible = () => IsSet(Flags.VinesCut),
    };
}
```

## Tutorial check

Beach1 gives 1 twig. JungleEdge gives 3 fronds (palm starts full), 3 rocks and 2 twigs. Lean-to needs 3 fronds + 1 twig; fire pit needs 3 rocks + 2 twigs. Both can be built on day 1.
