# Scripting sample

Illustration of the intended script shape (C73-C79). Not a final API: names, signatures and helpers will be settled during implementation.

## Scenario

First beach scene. Three items lie on the sand. Each has an examine line and a comment on pickup. When all three are picked up, the character comments and the path inland appears.

## Scene script

```csharp
using static Engine.Script;   // Say, Flag, SetFlag, FirstVisit, ...

public class Beach1 : Scene
{
    public Beach1() : base(background: "beach1") { }

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
    public Pickup Bottle = new()
    {
        Item     = Items.EmptyBottle,
        Sprite   = "empty_bottle",
        Position = (230, 109),
        Examine  = async () => await Say("A glass bottle. Empty, but not cracked."),
        OnPickup = async () => await Say(Face.Happy, "Something to carry water in. Lucky."),
    };

    // Coconut under the palm
    public Pickup Coconut = new()
    {
        Item     = Items.Coconut,
        Sprite   = "coconut",
        Position = (186, 132),
        Examine  = async () => await Say("A coconut. Breakfast, if I can get it open."),
        OnPickup = async () => await Say("Heavy. Good. Heavy means full."),
    };

    // Pile of dry twigs
    public Pickup Twigs = new()
    {
        Item     = Items.Twigs,
        Sprite   = "twigs",
        Position = (125, 102),
        Examine  = async () => await Say("Dry twigs. They would burn nicely."),
        OnPickup = async () => await Say("If I can get a fire going, at least the nights won't be pitch black."),
    };

    // Path inland, appears once the beach is searched
    public Exit ToJungleEdge = new()
    {
        Hotspot = "exit_north",
        Target  = nameof(JungleEdge),
        Minutes = 15,
        Visible = () => Flag("beach1.searched"),
    };

    // Runs after every completed action in this scene
    public override async Task AfterAction()
    {
        if (Flag("beach1.searched") || !AllPicked(Bottle, Coconut, Twigs))
            return;

        SetFlag("beach1.searched");
        await Say("That's everything on this stretch of sand.");
        await Say("The trees up north look thick, but there's a gap. Worth a look.");
    }
}
```

## Item definitions (game project)

```csharp
public static class Items
{
    public static readonly Item EmptyBottle = new("Empty bottle", icon: "empty_bottle");
    public static readonly Item Coconut     = new("Coconut",      icon: "coconut", tags: SafeToLose);
    public static readonly Item Twigs       = new("Twigs",        icon: "twigs",   tags: SafeToLose);
}
```

## What the engine does without being told

- `Pickup` is a template: it adds the shared "Pick up" action (label from the common collection, C77), charges the default pickup time (C19), runs the progress circle (C63), adds the item, shows the gain indication (C64), hides the sprite and remembers it was picked (saved state).
- Hover outline, context menu, Examine on RMB/menu, stat decay over spent time.
- `Visible = false` objects and exits are neither drawn nor clickable.

## Derived localization keys (C78, R10)

```
Scenes.Beach1.OnEnter.1        = "Ugh... my head. Sand everywhere."
Scenes.Beach1.OnEnter.2        = "Nobody else on this beach. Let's see what washed up."
Scenes.Beach1.Bottle.Examine   = "A glass bottle. Empty, but not cracked."
Scenes.Beach1.Bottle.OnPickup  = "Something to carry water in. Lucky."
Scenes.Beach1.Coconut.Examine  = ...
Scenes.Beach1.AfterAction.1    = "That's everything on this stretch of sand."
Scenes.Beach1.AfterAction.2    = ...
Items.EmptyBottle.Name         = "Empty bottle"
```

A Russian file overrides only what it translates:

```
Scenes.Beach1.Bottle.Examine = "Стеклянная бутылка. Пустая, но целая."
```
