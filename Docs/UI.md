# UI and rendering spec

Summary of the designer-supplied UI pack in `Assets/UIPack/`. The pack's own `README.md` is authoritative for exact coordinates, draw order and fill math; `layout.json` is the machine-readable copy. This file records what matters for design and implementation.

Pack layout as stored in the repo:
- `pixels/` - runtime sprites (UI at 640x360 grid, world/background/items at 320x180 grid).
- `sources/` - native-resolution originals and mask/material derivatives, for reprocessing.
- `mockup/` - reference mockups (`game_640x360.png`, `game_1280x720.png`, `world_320x180.png`, transparent HUD, fill examples) and the layered OpenRaster file `island_ui_editable.ora`.
- `README.md`, `ASSET_SOURCES.md`, `layout.json` - specs.
- Omitted from the repo: `layers/` (convenience exports of the .ora) and `verification/` (delivery checks, result PASS, 0 mismatched pixels). The pack README still mentions them.

## Canvases and scaling

- **World canvas: 320x180.** Background and world props are drawn here.
- **UI canvas: 640x360.** The finished world canvas is drawn onto it at exactly 2x, nearest neighbor; then UI sprites at 1x.
- The 640x360 frame is scaled to the window with point filtering, integer positions, no mipmaps. Window resize letterboxes (C13).
- UI sprites must never be rendered at 320x180 first.
- PNGs are straight alpha, 8-bit sRGB, plain source-over blending. In MonoGame: premultiply once on load + `BlendState.AlphaBlend`, or keep straight + `BlendState.NonPremultiplied`. Never premultiply twice. Shadows have partial alpha; do not threshold.

## World props

- Each ground item is a 32x32 PNG with a separate 32x32 shadow PNG drawn first at the same position.
- Ground anchor is local (16,28). Layout tables give top-left positions.
- Hover outline (C04) applies at world resolution.

## HUD (640x360)

- **Bamboo rails:** left 0..49, right 590..639, full height. They cover 50 UI px per side = **25 world px per side**.
  - Scene authoring rule: keep interactive objects and exits within world x 25..294 (proposed safe zone, to confirm).
- **Left wood block (3,7):** Health (heart), Food (fork and spoon), Water (drop).
- **Right wood block (602,7):** Rest (crescent), Hygiene (folded cloth), Comfort/Mood (wooden smiling mask; pack id `comfort`, "comfort and sanity combined").
- **Clock (5,236):** painted wood face with day/night halves, single rotating hand. Noon at top, midnight at bottom, clockwise. Angle = 2*pi*((timeFraction + 0.5) % 1), timeFraction measured from midnight. Pivot at (36,268), origin (32,32).
- **Buttons (stone tiles, 39x38):** Inventory (598,208), Map (598,262), Skills (598,314), Menu (5,316). Symbols are separate layers.

## Stat indicators

Each stat folder has `empty.png`, `fill.png`, `mask.png`, `mask_alpha.png`, plus demo files only for reproducing the mockup.

Runtime fill for value v in [0,1]:
1. `cut = round(H * (1 - v))`, ties to even.
2. Draw `empty.png`.
3. Draw rows `cut..H-1` of `fill.png` at the same offset (source rect `(0,cut,W,H-cut)`, destination `(X, Y+cut)`, no stretching). Skip at v=0.

The fill rises from the bottom. Up/down trend indication (C64) and threshold flashing (C65) are not in the pack yet.

## Not in the pack yet

- Inventory panel (designer will supply).
- Dialogue box and portraits (VN style, C62).
- Context menu, journal, map screen, crafting menus, progress circle (C63), item gain indication (C64).
- Day counter, weather indicator.
- Fonts.
