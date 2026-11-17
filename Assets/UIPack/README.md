# Stranded Together - finished island UI pack

This pack contains the current Style A UI: heart, food and water on the left; crescent rest, folded washcloth hygiene, and rectangular wooden mood mask on the right. It includes the accepted bamboo supports, blank wood blocks, stone menu buttons, and painted wood clock. Only the current art is included.

## Contents

- `sources/`: genuine native-resolution sources, plus explicitly named source-sized material/mask derivatives. Each visible asset has an individual source image. The clock hand and ground shadows were authored on their final grid; their source images are intentionally not enlarged.
- `pixels/`: individual clean PNG sprites. UI sprites are at the 640x360 game grid; background and ground props are at the 320x180 world grid.
- `mockup/`: supplied game mockups, transparent HUD, world reference, fill examples, and layered OpenRaster document.
- `layers/`: each visible mockup component as its own aligned 640x360 transparent PNG. These are convenience exports, not inputs to the reconstruction below.
- `ASSET_SOURCES.md`: per-file parent source paths, native dimensions, and processing notes.
- `layout.json`: a machine-readable copy of the coordinate tables and status positions.
- `verification/`: mockup rebuilt from this README after extraction, pixel-difference image, and comparison report.
- `SHA256SUMS.txt`: checksums of all delivered files except this checksum file itself.

## Coordinate and rendering rules

1. Make two transparent RGBA canvases: `world` = 320x180 and `ui` = 640x360. Coordinates below are integer top-left coordinates, with x increasing right and y increasing down.
2. Draw the WORLD table in its listed order onto `world`. Then draw the UI table in its listed order onto `ui`.
3. `W` and `H` are the actual source PNG dimensions, including transparent padding. Never crop/recenter a sprite by its visible bounds. A destination rectangle is `(X, Y, W*Scale, H*Scale)`; its right/bottom edges are exclusive.
4. `@world` means the completed in-memory 320x180 world canvas, enlarged exactly 2x to 640x360 with nearest-neighbor sampling. No other row has scaling. UI sprites must never be rendered at 320x180 first.
5. For every row, load its PNG as straight-alpha RGBA. If `Clear top rows = N`, set alpha to zero in local rows `0 .. N-1`, leaving all lower rows unchanged. Zero means no clipping. Then perform ordinary source-over alpha compositing at the listed coordinates. Do not tint the pixels.
6. Use point/nearest-neighbor sampling, integer positions and no mipmaps. Preserve the original sRGB byte colors. Reference compositing uses ordinary 8-bit sRGB source-over, not linear-light blending. For a 1280x720 display preview, enlarge the finished 640x360 image exactly 2x with nearest-neighbor.
7. PNGs contain straight alpha. In MonoGame, either premultiply once during import/load and use `BlendState.AlphaBlend`, or keep straight data and use `BlendState.NonPremultiplied`. Do not premultiply twice. Ground shadows use partial alpha; do not threshold them.

## Exact world draw order (320x180)

All props are separate 32x32 PNGs. Their ground anchor is local `(16,28)`. The table already contains their sprite top-left positions; do not subtract the anchor again.

| Order | Image | X | Y | W | H | Scale | Clear top rows | Purpose |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| 0 | `pixels/background/creek.png` | 0 | 0 | 320 | 180 | 1 | 0 | Creek background |
| 1 | `pixels/items/twigs_shadow.png` | 125 | 102 | 32 | 32 | 1 | 0 | twigs shadow |
| 2 | `pixels/items/twigs_sprite.png` | 125 | 102 | 32 | 32 | 1 | 0 | twigs sprite |
| 3 | `pixels/items/empty_bottle_shadow.png` | 230 | 109 | 32 | 32 | 1 | 0 | empty_bottle shadow |
| 4 | `pixels/items/empty_bottle_sprite.png` | 230 | 109 | 32 | 32 | 1 | 0 | empty_bottle sprite |
| 5 | `pixels/items/palm_frond_shadow.png` | 99 | 143 | 32 | 32 | 1 | 0 | palm_frond shadow |
| 6 | `pixels/items/palm_frond_sprite.png` | 99 | 143 | 32 | 32 | 1 | 0 | palm_frond sprite |
| 7 | `pixels/items/coconut_shadow.png` | 186 | 132 | 32 | 32 | 1 | 0 | coconut shadow |
| 8 | `pixels/items/coconut_sprite.png` | 186 | 132 | 32 | 32 | 1 | 0 | coconut sprite |
| 9 | `pixels/items/shell_shadow.png` | 247 | 144 | 32 | 32 | 1 | 0 | shell shadow |
| 10 | `pixels/items/shell_sprite.png` | 247 | 144 | 32 | 32 | 1 | 0 | shell sprite |
| 11 | `pixels/items/rock_shadow.png` | 269 | 127 | 32 | 32 | 1 | 0 | rock shadow |
| 12 | `pixels/items/rock_sprite.png` | 269 | 127 | 32 | 32 | 1 | 0 | rock sprite |

## Exact UI draw order (640x360)

These rows reproduce `mockup/game_640x360.png` exactly. Use the cropped files under `pixels/`, not a flattened HUD or any convenience file in `layers/` or `mockup/`.

| Order | Image | X | Y | W | H | Scale | Clear top rows | Purpose |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| 0 | `@world` | 0 | 0 | 320 | 180 | 2 | 0 | Completed world enlarged 2x |
| 1 | `pixels/ui/bamboo/left.png` | 0 | 0 | 50 | 360 | 1 | 0 | left bamboo |
| 2 | `pixels/ui/bamboo/right.png` | 590 | 0 | 50 | 360 | 1 | 0 | right bamboo |
| 3 | `pixels/ui/wood/status_block.png` | 3 | 7 | 36 | 128 | 1 | 0 | Left status wood |
| 4 | `pixels/ui/wood/status_block.png` | 602 | 7 | 36 | 128 | 1 | 0 | Right status wood |
| 5 | `pixels/ui/indicators/health/demo_empty.png` | 11 | 22 | 20 | 19 | 1 | 0 | health cavity |
| 6 | `pixels/ui/indicators/health/demo_fill.png` | 11 | 22 | 20 | 19 | 1 | 0 | health authored demo paint |
| 7 | `pixels/ui/indicators/food/demo_empty.png` | 11 | 61 | 20 | 22 | 1 | 0 | food cavity |
| 8 | `pixels/ui/indicators/food/demo_fill.png` | 11 | 61 | 20 | 22 | 1 | 0 | food authored demo paint |
| 9 | `pixels/ui/indicators/water/demo_empty.png` | 12 | 100 | 18 | 25 | 1 | 0 | water cavity |
| 10 | `pixels/ui/indicators/water/demo_fill.png` | 12 | 100 | 18 | 25 | 1 | 0 | water authored demo paint |
| 11 | `pixels/ui/indicators/rest/empty.png` | 610 | 21 | 19 | 22 | 1 | 0 | rest cavity |
| 12 | `pixels/ui/indicators/rest/fill.png` | 610 | 21 | 19 | 22 | 1 | 11 | rest demo fill |
| 13 | `pixels/ui/indicators/hygiene/empty.png` | 609 | 64 | 22 | 17 | 1 | 0 | hygiene cavity |
| 14 | `pixels/ui/indicators/hygiene/fill.png` | 609 | 64 | 22 | 17 | 1 | 6 | hygiene demo fill |
| 15 | `pixels/ui/indicators/comfort/empty.png` | 610 | 100 | 19 | 24 | 1 | 0 | comfort cavity |
| 16 | `pixels/ui/indicators/comfort/fill.png` | 610 | 100 | 19 | 24 | 1 | 4 | comfort demo fill |
| 17 | `pixels/ui/clock/face.png` | 5 | 236 | 62 | 64 | 1 | 0 | Painted wood clock |
| 18 | `pixels/ui/clock/hand_demo.png` | 4 | 236 | 64 | 64 | 1 | 0 | Single clock hand, demo pose |
| 19 | `pixels/ui/buttons/inventory_stone.png` | 598 | 208 | 39 | 38 | 1 | 0 | inventory stone |
| 20 | `pixels/ui/buttons/inventory_symbol.png` | 606 | 214 | 22 | 26 | 1 | 0 | inventory symbol |
| 21 | `pixels/ui/buttons/map_stone.png` | 598 | 262 | 39 | 38 | 1 | 0 | map stone |
| 22 | `pixels/ui/buttons/map_symbol.png` | 605 | 270 | 25 | 22 | 1 | 0 | map symbol |
| 23 | `pixels/ui/buttons/skills_stone.png` | 598 | 314 | 39 | 38 | 1 | 0 | skills stone |
| 24 | `pixels/ui/buttons/skills_symbol.png` | 605 | 322 | 25 | 22 | 1 | 0 | skills symbol |
| 25 | `pixels/ui/buttons/menu_stone.png` | 5 | 316 | 39 | 38 | 1 | 0 | menu stone |
| 26 | `pixels/ui/buttons/menu_symbol.png` | 12 | 322 | 25 | 25 | 1 | 0 | menu symbol |

## Runtime status fills

Every status has the same file names under `pixels/ui/indicators/<id>/`:

- `empty.png`: empty carved wood cavity, with original rim/shadow and unchanged dimensions.
- `fill.png`: full-range textured color insert. Its alpha is already the fill mask. Do not replace this painted texture with a flat color.
- `mask.png`: 8-bit luminance mask, white = fillable, black = excluded. `mask_alpha.png` contains the same mask in the alpha channel with white RGB.
- `demo.png`: ready-composited indicator exactly as it appears in the supplied mockup.
- `demo_fill.png`: the standalone paint layer for that exact demo state.
- `demo_empty.png` (left three only): the exact accepted demo underpaint. The exact-build table pairs it with `demo_fill.png`. Runtime uses the corrected `empty.png`, which removes residual paint tint at zero fill.

To show a runtime value `v`, clamp it to [0,1], compute `cut = round(H * (1-v))` with ties rounded to even, draw `empty.png`, then draw only local source rows `cut .. H-1` of `fill.png` at the same full-sprite position. Keep the rim, empty cavity and mask stationary. A shader can instead suppress fragments whose local integer row is less than `cut`. The insert already excludes the exterior and face marks.

With a source-rectangle draw, use source `(0,cut,W,H-cut)` and destination top-left `(X,Y+cut)`, without stretching. Skip the paint draw at `v=0`. Draw the full insert at `v=1`.

| ID | Parameter / symbol | X | Y | W | H | Exact demo | Demo top rows cleared |
| --- | --- | ---: | ---: | ---: | ---: | --- | --- |
| health | Health / heart | 11 | 22 | 20 | 19 | Authored painted boundary | Use demo_fill.png |
| food | Food / fork and spoon | 11 | 61 | 20 | 22 | Authored painted boundary | Use demo_fill.png |
| water | Water / drop | 12 | 100 | 18 | 25 | Authored painted boundary | Use demo_fill.png |
| rest | Rest / crescent | 610 | 21 | 19 | 22 | 0.5 | 11 |
| hygiene | Hygiene / folded cloth | 609 | 64 | 22 | 17 | 0.62 | 6 |
| comfort | Comfort and sanity combined / wooden mask | 610 | 100 | 19 | 24 | 0.83 | 4 |

The left three accepted icons were originally painted with irregular partial-fill boundaries. Their exact demo is retained with `demo_empty.png` plus `demo_fill.png`; no numeric gameplay value is implied by that brush-painted boundary. They now also have complete full-range masks and color inserts for runtime 0-100% fills. Their originally unpainted upper cavity areas were colorized from the existing painted material while retaining the wood texture; the accepted demo pixels are unchanged. The right three full-range masks and inserts come from the fully colored crescent, cloth and rectangular mask source images.

All six empty/paint/mask sets have identical local dimensions and origins within their own status. The filled paint is a separate layer; the wood panel behind each group remains a separate reusable sprite.

## Clock assembly and rotation

- Paint and wood bezel are one image: `pixels/ui/clock/face.png`, 62x64 at `(5,236)`.
- Demo hand: `pixels/ui/clock/hand_demo.png`, 64x64 at `(4,236)`. Use it unchanged for the exact mockup; do not regenerate it by rotating the other hand.
- Runtime hand: `pixels/ui/clock/hand_noon.png`, 64x64. Local pivot `(32,32)` must sit at canvas `(36,268)`. In MonoGame, draw at `(36,268)` with origin `(32,32)`, scale 1.
- The runtime hand points up at zero rotation. Rotate only that one hand, clockwise: noon = 0, dusk = pi/2, midnight = pi, dawn = 3*pi/2. For normalized in-game time measured from midnight, angle = `2*pi*((timeFraction+0.5)%1)`.
- Do not rotate the painted face, change its size, or add another hand.

## Button bounds

These are UI pixel rectangles for the visible stone tiles. An alpha-aware hit test can refine them; the symbols remain separate draw layers.

| Action | X | Y | W | H |
| --- | ---: | ---: | ---: | ---: |
| Inventory | 598 | 208 | 39 | 38 |
| Map | 598 | 262 | 39 | 38 |
| Skills | 598 | 314 | 39 | 38 |
| Menu | 5 | 316 | 39 | 38 |

## Original sources and processing

The native source for every processed file is listed in `ASSET_SOURCES.md`. Sources are genuine generated/native authoring images, never enlarged copies of pixel sprites. Derived native cavity, paint and mask images are named explicitly so you can edit either the original art or its separated material layers.

General processing was alpha cleanup at 128 for opaque UI/object art, component cleanup, tight crop, then nearest-neighbor resize to the recorded final dimensions. No palette reduction or dithering was applied. Ground shadows retain their authored partial alpha. The bamboo rails include restored hidden areas and small original-rail retouch patches; their source crops, full restoration source and original reference frame are all included. The finished rail pixels are authoritative. Separate stone and symbol sources are included for every button.

The background is the native creek painting resized directly to 320x180 with nearest-neighbor. The six ground-item source images and their individual finished sprites/shadows are included so the world can be rebuilt rather than using an already composited scene.

## Verification contract

Before delivery, the archive is extracted into a fresh directory. A reconstruction process reads ONLY this README for canvas sizes, file paths, draw order, coordinates, scale and top-row cutoffs; it then opens only the individual source sprites named in the two draw tables. It does not read `layout.json`, the OpenRaster document, existing full-canvas layers, or any precomposited background/HUD.

The resulting native image is compared byte-for-byte as decoded RGBA pixels with `mockup/game_640x360.png`. The supplied 1280x720 preview is also checked against a nearest-neighbor 2x enlargement. See `verification/README_REBUILD_REPORT.txt` for the measured result and `verification/rebuilt_from_readme_640x360.png` for the independent reconstruction. `verification/pixel_difference.png` is black wherever all pixels match.

The editable OpenRaster document contains the same world objects and UI components as separate layers on a 640x360 canvas, plus six hidden fill masks. `layers/` exports those visible layers as aligned transparent PNGs. No program source files are included.
