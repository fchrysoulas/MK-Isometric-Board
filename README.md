# MK-Isometric-Board

Place an adjustable diamond grid over an isometric map in Foundry VTT. Drag full-body tokens by their feet and see their bases, destinations, and movement distances on the board.

## Install

In Foundry Setup, open **Add-on Modules → Install Module** and paste this manifest URL.

    https://github.com/fchrysoulas/MK-Isometric-Board/releases/latest/download/module.json

Then enable **MK-Isometric-Board** in **Manage Modules** in your world. The module targets Foundry v13 and v14 and uses Foundry's token and canvas APIs.

## Set up a scene

1. Create a Scene with the isometric map as its background and choose a gridless native Scene grid.
2. In the GM's **Token Controls**, click the chessboard to enable the overlay for that Scene. Click the sliders to open its settings.
3. Select **6 × 6 to 18 × 18** cells. Adjust tilt and rotation, then use **Move Grid** to align the board to the floor. Drag the corner handles to resize each axis and choose **Done** to finish.
4. Place upright transparent tokens and drag them by the base. The board checks whether their footprint fits and whether other tokens occupy the destination. Set walls and doors along the floor edges of the artwork.

The board overlays an existing image. The image determines the perspective of the terrain. Board geometry and line color are saved per Scene. Opacity is saved per client and can also be set in **Configure Settings → Module Settings**.

The board ruler counts **5 feet per cell**, including diagonals. The range colors mark 5 and 30 feet. The ruler reports board distance. Foundry's native spell templates use the Scene's coordinate system.

When using both MK modules in one world, turn off the original MK-Shadowdark board for a Scene before enabling the standalone board there. Each module stores its own Scene settings.

## Development

Run `npm test` with Node.js 22 or newer. The tests exercise the geometry, token movement, ruler, and configuration with Foundry mocks.

The board was extracted from [MK-Shadowdark](https://github.com/fchrysoulas/MK-Shadowdark) and is distributed under the [MIT license](LICENSE).
