# MK-Isometric-Board

Place an adjustable diamond or trapezoid grid over a map in Foundry VTT. Drag full-body tokens by their feet and see their bases, destinations, and movement distances on the board.

## Install

In Foundry Setup, open **Add-on Modules → Install Module** and paste this manifest URL.

    https://github.com/fchrysoulas/MK-Isometric-Board/releases/latest/download/module.json

Then enable **MK-Isometric-Board** in **Manage Modules** in your world. The module targets Foundry v13 and v14 and uses Foundry's token and canvas APIs.

## Set up a scene

1. Create a Scene with the isometric map as its background and choose a gridless native Scene grid.
2. In the GM's **Token Controls**, click the chessboard to enable the overlay for that Scene. Click the sliders to open its settings.
3. Choose **Diamond** or **Trapezoid** under **Grid shape**, then select **6 × 6 to 18 × 18** cells. Trapezoid has a shorter top edge and a wider bottom edge; use **Top width** to adjust the taper. Adjust tilt and rotation, then use **Move Grid** to align the board to the floor. Drag the corner handles to resize each axis, or drag the circular-arrow handle above the frame to rotate around the grid's center. Choose **Done** to finish.
4. Place upright transparent tokens and drag them by the base. The board checks whether their footprint fits and whether other tokens occupy the destination. Set walls and doors along the floor edges of the artwork.

The board overlays an existing image. The image determines the perspective of the terrain. Board geometry and line color are saved per Scene. Opacity is saved per client and can also be set in **Configure Settings → Module Settings**.

Trapezoid cells use perspective spacing: both cell width and row depth shrink toward the narrow top edge and expand toward the wider bottom edge. **Top width** controls the perspective strength, while tilt and axis resizing control the overall proportions. Existing trapezoid boards keep their outer frame, but the corrected interior spacing may require realigning the grid with the map. Existing tokens stay at their current map positions.

The board ruler counts **5 feet per cell**, including diagonals. The range colors mark 5 and 30 feet. The ruler reports board distance. Foundry's native spell templates use the Scene's coordinate system.

When using both MK modules in one world, turn off the original MK-Shadowdark board for a Scene before enabling the standalone board there. Each module stores its own Scene settings.

## Development

Run `npm test` with Node.js 22 or newer. The tests exercise the geometry, token movement, ruler, and configuration with Foundry mocks.

### Foundry compatibility

The manifest targets Foundry v13 and v14 (`minimum: 13`, `verified: 14`). The compatibility review on 2026-11-12 compared the module with the official v13.350 and v14.370 API documentation and the locally installed v14.370 client source. No incompatible API usage was found in the reviewed integrations. All 120 automated tests passed on Node.js 24.18.0.

| Integration | v13 | v14 |
| --- | --- | --- |
| Scene controls | Keyed token tools and `onChange(event, active)` | Same interface |
| Native movement and snapping | `TokenDocument.move`, `getSnappedPosition`, `preMoveToken` | Same entry points; final waypoints remain immutable |
| Wall-check origin | Artwork base substituted through `getCenterPoint` during synchronous constraints | Uses `getMovementOrigin`, preserving depth-adjusted elevation |
| Token ruler | Configured ruler subclass with waypoint, segment, highlight, and label extension points | Same extension points; new optional waypoint shapes do not affect overrides |
| Artwork bounds and rendering | `getSize`, texture alpha bounds, and PIXI graphics | Same interfaces |

References: [v13 token document](https://foundryvtt.com/api/v13/classes/foundry.documents.TokenDocument.html), [v14 token document](https://foundryvtt.com/api/v14/classes/foundry.documents.TokenDocument.html), [v13 token ruler](https://foundryvtt.com/api/v13/classes/foundry.canvas.placeables.tokens.TokenRuler.html), [v14 token ruler](https://foundryvtt.com/api/v14/classes/foundry.canvas.placeables.tokens.TokenRuler.html), and [scene controls](https://foundryvtt.com/api/v14/functions/hookEvents.getSceneControlButtons.html).

This review used API inspection and mocked tests; live Foundry smoke tests were not run. Internal token drag wrappers remain sensitive to future Foundry changes. Before a release, check the following in a disposable world on each supported generation:

1. Enable the module on a gridless Scene, toggle the board, open settings as GM, and confirm players cannot edit the board.
2. Adjust both grid shapes, size, tilt, rotation, position, color, and opacity. Reload and switch Scenes to check persistence and cleanup.
3. Drag Small through Gargantuan tokens, add waypoints, and use the board movement API. Check foot alignment, ruler labels, occupied cells, board edges, walls, pause, and ownership restrictions.
4. Check hidden tokens and movement with a GM and a player connected, then cancel and complete drags to confirm paths and selectors clear correctly.
5. On v14, also check nonzero token depth, elevation changes, Scene levels, regions, and movement planning. These workflows still need live verification.

The board was extracted from [MK-Shadowdark](https://github.com/fchrysoulas/MK-Shadowdark) and is distributed under the [MIT license](LICENSE).
