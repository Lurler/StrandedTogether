# Scripting sample

Illustration of the intended script shape (C73-C80, C81-C85). Not a final API: names, signatures and helpers will be settled during implementation.

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

    // Empty bottle in the shallow water
    public Pickup<EmptyBottle> Bottle = new()
    {
        Position  = (230, 109),
        Placement = Placement.Shallow,
        OnPickup  = async () => await Say(Face.Happy, "Something to carry water in. Lucky."),
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
- Default placement is `Ground`; default examine is the item's own `Examine` text.
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
