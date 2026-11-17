# Native source map

Every runtime PNG in `pixels/` is listed below. Sizes are final pixels; parent dimensions are native authoring pixels. Derived masks and material separations retain their original art parents.

| Runtime image | Size | Native source image(s) | Processing |
| --- | --- | --- | --- |
| `pixels/background/creek.png` | 320x180 | `sources/background/creek_painting.png` (1672x941) | Nearest-neighbor resize to 320x180, preserving colors. |
| `pixels/items/twigs_shadow.png` | 32x32 | `sources/items/twigs_shadow_native.png` (32x32) | Procedural shadow authored at 32x32; its original is this native-grid PNG. |
| `pixels/items/twigs_sprite.png` | 32x32 | `sources/items/twigs.png` (1343x688) | Alpha cleanup, crop, nearest-neighbor sprite resize; 32x32 padded canvas, anchor (16,28). |
| `pixels/items/empty_bottle_shadow.png` | 32x32 | `sources/items/empty_bottle_shadow_native.png` (32x32) | Procedural shadow authored at 32x32; its original is this native-grid PNG. |
| `pixels/items/empty_bottle_sprite.png` | 32x32 | `sources/items/empty_bottle.png` (1299x660) | Alpha cleanup, crop, nearest-neighbor sprite resize; 32x32 padded canvas, anchor (16,28). |
| `pixels/items/palm_frond_shadow.png` | 32x32 | `sources/items/palm_frond_shadow_native.png` (32x32) | Procedural shadow authored at 32x32; its original is this native-grid PNG. |
| `pixels/items/palm_frond_sprite.png` | 32x32 | `sources/items/palm_frond.png` (1643x695) | Alpha cleanup, crop, nearest-neighbor sprite resize; 32x32 padded canvas, anchor (16,28). |
| `pixels/items/coconut_shadow.png` | 32x32 | `sources/items/coconut_shadow_native.png` (32x32) | Procedural shadow authored at 32x32; its original is this native-grid PNG. |
| `pixels/items/coconut_sprite.png` | 32x32 | `sources/items/coconut.png` (719x640) | Alpha cleanup, crop, nearest-neighbor sprite resize; 32x32 padded canvas, anchor (16,28). |
| `pixels/items/shell_shadow.png` | 32x32 | `sources/items/shell_shadow_native.png` (32x32) | Procedural shadow authored at 32x32; its original is this native-grid PNG. |
| `pixels/items/shell_sprite.png` | 32x32 | `sources/items/shell.png` (888x689) | Alpha cleanup, crop, nearest-neighbor sprite resize; 32x32 padded canvas, anchor (16,28). |
| `pixels/items/rock_shadow.png` | 32x32 | `sources/items/rock_shadow_native.png` (32x32) | Procedural shadow authored at 32x32; its original is this native-grid PNG. |
| `pixels/items/rock_sprite.png` | 32x32 | `sources/items/rock.png` (898x503) | Alpha cleanup, crop, nearest-neighbor sprite resize; 32x32 padded canvas, anchor (16,28). |
| `pixels/ui/bamboo/left.png` | 50x360 | `sources/bamboo/left.png` (131x941); `sources/bamboo/generated_frame.png` (1672x941); `sources/bamboo/original_reference_frame.png` (1672x941) | Restored bare rails; alpha cleanup and original visible-rail retouch patches. Final processed pixels are authoritative. |
| `pixels/ui/bamboo/right.png` | 50x360 | `sources/bamboo/right.png` (131x941); `sources/bamboo/generated_frame.png` (1672x941); `sources/bamboo/original_reference_frame.png` (1672x941) | Restored bare rails; alpha cleanup and original visible-rail retouch patches. Final processed pixels are authoritative. |
| `pixels/ui/wood/status_block.png` | 36x128 | `sources/wood/status_block.png` (103x311) | Alpha-cleaned wood; resized to 36x128. Reuse the same sprite on both sides. |
| `pixels/ui/indicators/health/empty.png` | 20x19 | `sources/indicators/health/empty.png` (419x393) | Native texture-preserving underpaint; all residual painted color removed for a truly empty runtime cavity, then nearest-neighbor resize. |
| `pixels/ui/indicators/health/fill.png` | 20x19 | `sources/indicators/health/fill.png` (419x393) | Source-sized material/mask derivation, then nearest-neighbor resize; painted texture retained. |
| `pixels/ui/indicators/health/demo_fill.png` | 20x19 | `sources/indicators/health/demo_fill.png` (419x393) | Source-sized material/mask derivation, then nearest-neighbor resize; painted texture retained. |
| `pixels/ui/indicators/health/demo.png` | 20x19 | `sources/indicators/health/original.png` (419x393) | Nearest-neighbor source reduction. Demo is the accepted authored partial-fill image. |
| `pixels/ui/indicators/health/mask.png` | 20x19 | `sources/indicators/health/mask.png` (419x393) | Source-sized material/mask derivation, then nearest-neighbor resize; painted texture retained. |
| `pixels/ui/indicators/health/mask_alpha.png` | 20x19 | `sources/indicators/health/mask.png` (419x393) | White RGB, alpha equals luminance mask; same sprite size and origin. |
| `pixels/ui/indicators/food/empty.png` | 20x22 | `sources/indicators/food/empty.png` (366x398) | Native texture-preserving underpaint; all residual painted color removed for a truly empty runtime cavity, then nearest-neighbor resize. |
| `pixels/ui/indicators/food/fill.png` | 20x22 | `sources/indicators/food/fill.png` (366x398) | Source-sized material/mask derivation, then nearest-neighbor resize; painted texture retained. |
| `pixels/ui/indicators/food/demo_fill.png` | 20x22 | `sources/indicators/food/demo_fill.png` (366x398) | Source-sized material/mask derivation, then nearest-neighbor resize; painted texture retained. |
| `pixels/ui/indicators/food/demo.png` | 20x22 | `sources/indicators/food/original.png` (366x398) | Nearest-neighbor source reduction. Demo is the accepted authored partial-fill image. |
| `pixels/ui/indicators/food/mask.png` | 20x22 | `sources/indicators/food/mask.png` (366x398) | Source-sized material/mask derivation, then nearest-neighbor resize; painted texture retained. |
| `pixels/ui/indicators/food/mask_alpha.png` | 20x22 | `sources/indicators/food/mask.png` (366x398) | White RGB, alpha equals luminance mask; same sprite size and origin. |
| `pixels/ui/indicators/water/empty.png` | 18x25 | `sources/indicators/water/empty.png` (302x412) | Native texture-preserving underpaint; all residual painted color removed for a truly empty runtime cavity, then nearest-neighbor resize. |
| `pixels/ui/indicators/water/fill.png` | 18x25 | `sources/indicators/water/fill.png` (302x412) | Source-sized material/mask derivation, then nearest-neighbor resize; painted texture retained. |
| `pixels/ui/indicators/water/demo_fill.png` | 18x25 | `sources/indicators/water/demo_fill.png` (302x412) | Source-sized material/mask derivation, then nearest-neighbor resize; painted texture retained. |
| `pixels/ui/indicators/water/demo.png` | 18x25 | `sources/indicators/water/original.png` (302x412) | Nearest-neighbor source reduction. Demo is the accepted authored partial-fill image. |
| `pixels/ui/indicators/water/mask.png` | 18x25 | `sources/indicators/water/mask.png` (302x412) | Source-sized material/mask derivation, then nearest-neighbor resize; painted texture retained. |
| `pixels/ui/indicators/water/mask_alpha.png` | 18x25 | `sources/indicators/water/mask.png` (302x412) | White RGB, alpha equals luminance mask; same sprite size and origin. |
| `pixels/ui/indicators/rest/empty.png` | 19x22 | `sources/indicators/rest/empty.png` (515x595) | Nearest-neighbor resize; demo paint clears 11 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/rest/fill.png` | 19x22 | `sources/indicators/rest/fill.png` (515x595) | Nearest-neighbor resize; demo paint clears 11 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/rest/mask.png` | 19x22 | `sources/indicators/rest/mask.png` (515x595) | Nearest-neighbor resize; demo paint clears 11 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/rest/mask_alpha.png` | 19x22 | `sources/indicators/rest/mask.png` (515x595) | Nearest-neighbor resize; demo paint clears 11 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/rest/demo_fill.png` | 19x22 | `sources/indicators/rest/demo_fill.png` (515x595) | Nearest-neighbor resize; demo paint clears 11 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/rest/demo.png` | 19x22 | `sources/indicators/rest/demo.png` (515x595) | Nearest-neighbor resize; demo paint clears 11 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/hygiene/empty.png` | 22x17 | `sources/indicators/hygiene/empty.png` (596x466) | Nearest-neighbor resize; demo paint clears 6 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/hygiene/fill.png` | 22x17 | `sources/indicators/hygiene/fill.png` (596x466) | Nearest-neighbor resize; demo paint clears 6 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/hygiene/mask.png` | 22x17 | `sources/indicators/hygiene/mask.png` (596x466) | Nearest-neighbor resize; demo paint clears 6 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/hygiene/mask_alpha.png` | 22x17 | `sources/indicators/hygiene/mask.png` (596x466) | Nearest-neighbor resize; demo paint clears 6 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/hygiene/demo_fill.png` | 22x17 | `sources/indicators/hygiene/demo_fill.png` (596x466) | Nearest-neighbor resize; demo paint clears 6 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/hygiene/demo.png` | 22x17 | `sources/indicators/hygiene/demo.png` (596x466) | Nearest-neighbor resize; demo paint clears 6 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/comfort/empty.png` | 19x24 | `sources/indicators/comfort/empty.png` (426x532) | Nearest-neighbor resize; demo paint clears 4 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/comfort/fill.png` | 19x24 | `sources/indicators/comfort/fill.png` (426x532) | Nearest-neighbor resize; demo paint clears 4 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/comfort/mask.png` | 19x24 | `sources/indicators/comfort/mask.png` (426x532) | Nearest-neighbor resize; demo paint clears 4 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/comfort/mask_alpha.png` | 19x24 | `sources/indicators/comfort/mask.png` (426x532) | Nearest-neighbor resize; demo paint clears 4 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/comfort/demo_fill.png` | 19x24 | `sources/indicators/comfort/demo_fill.png` (426x532) | Nearest-neighbor resize; demo paint clears 4 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/indicators/comfort/demo.png` | 19x24 | `sources/indicators/comfort/demo.png` (426x532) | Nearest-neighbor resize; demo paint clears 4 top rows. Mask alpha and paint alpha match exactly. |
| `pixels/ui/clock/face.png` | 62x64 | `sources/clock/face.png` (1254x1254) | Painted wooden face and bezel generated as one object, alpha-cleaned, fit within 64x64 with nearest-neighbor. |
| `pixels/ui/clock/hand_noon.png` | 64x64 | `sources/clock/hand_noon.png` (64x64) | Hand was authored directly on the 64x64 UI grid. This is its native source, not an enlarged stand-in. |
| `pixels/ui/clock/hand_demo.png` | 64x64 | `sources/clock/hand_demo.png` (64x64) | Hand was authored directly on the 64x64 UI grid. This is its native source, not an enlarged stand-in. |
| `pixels/ui/buttons/inventory_stone.png` | 39x38 | `sources/buttons/inventory_stone.png` (329x323) | Original crop, alpha cleanup / symbol isolation, nearest-neighbor fit. Accepted pixel file copied unchanged. |
| `pixels/ui/buttons/inventory_symbol.png` | 22x26 | `sources/buttons/inventory_symbol.png` (224x265) | Original crop, alpha cleanup / symbol isolation, nearest-neighbor fit. Accepted pixel file copied unchanged. |
| `pixels/ui/buttons/map_stone.png` | 39x38 | `sources/buttons/map_stone.png` (331x322) | Original crop, alpha cleanup / symbol isolation, nearest-neighbor fit. Accepted pixel file copied unchanged. |
| `pixels/ui/buttons/map_symbol.png` | 25x22 | `sources/buttons/map_symbol.png` (270x237) | Original crop, alpha cleanup / symbol isolation, nearest-neighbor fit. Accepted pixel file copied unchanged. |
| `pixels/ui/buttons/skills_stone.png` | 39x38 | `sources/buttons/skills_stone.png` (333x323) | Original crop, alpha cleanup / symbol isolation, nearest-neighbor fit. Accepted pixel file copied unchanged. |
| `pixels/ui/buttons/skills_symbol.png` | 25x22 | `sources/buttons/skills_symbol.png` (278x249) | Original crop, alpha cleanup / symbol isolation, nearest-neighbor fit. Accepted pixel file copied unchanged. |
| `pixels/ui/buttons/menu_stone.png` | 39x38 | `sources/buttons/menu_stone.png` (329x324) | Original crop, alpha cleanup / symbol isolation, nearest-neighbor fit. Accepted pixel file copied unchanged. |
| `pixels/ui/buttons/menu_symbol.png` | 25x25 | `sources/buttons/menu_symbol.png` (253x251) | Original crop, alpha cleanup / symbol isolation, nearest-neighbor fit. Accepted pixel file copied unchanged. |
| `pixels/ui/indicators/health/demo_empty.png` | 20x19 | `sources/indicators/health/demo_empty.png` (419x393) | Exact accepted demo underpaint; paired with demo_fill.png only. Runtime uses empty.png. |
| `pixels/ui/indicators/food/demo_empty.png` | 20x22 | `sources/indicators/food/demo_empty.png` (366x398) | Exact accepted demo underpaint; paired with demo_fill.png only. Runtime uses empty.png. |
| `pixels/ui/indicators/water/demo_empty.png` | 18x25 | `sources/indicators/water/demo_empty.png` (302x412) | Exact accepted demo underpaint; paired with demo_fill.png only. Runtime uses empty.png. |

Mockups, full-canvas layers, difference images and the layered document are derivatives of the ordered composition in README.md.
