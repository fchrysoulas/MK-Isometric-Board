# Repository guidance

## Project

MK-Isometric-Board is a standalone, system-independent Foundry VTT module targeting v13 and v14. It overlays an adjustable diamond or trapezoid board on a Scene and anchors token interaction to the feet of the artwork.

## Repository map

- [module.json](./module.json): module identity, compatibility, runtime assets, and release URLs.
- [isometric-board.js](./scripts/isometric-board/isometric-board.js): Foundry hooks, overlay rendering, settings, controls, token movement, and the public module API.
- [geometry.js](./scripts/isometric-board/geometry.js): board transforms, footprints, distances, and artwork anchors.
- [token-ruler.js](./scripts/isometric-board/token-ruler.js): subclass of the configured native token ruler for base-aligned paths and labels.
- [isometric-board.css](./styles/isometric-board.css) and [en.json](./lang/en.json): scoped UI styles and localized text.
- [isometric-board.test.mjs](./tests/isometric-board.test.mjs): Node tests using injected Foundry, PIXI, and DOM mocks.

## Development and validation

- Use native JavaScript ES modules and the existing code style. The project has no build step or installed dependencies.
- Use Node.js 22 or newer for development. Run `npm test` after runtime changes; on Windows with a restricted PowerShell execution policy, use `npm.cmd test` or `node --test`.
- For focused checks, use `node --test --test-name-pattern="<pattern>" tests/isometric-board.test.mjs`.
- Add regression coverage for meaningful behavior changes, especially movement, visibility, geometry, and lifecycle cleanup. Extend the existing environment injection rather than requiring Foundry to run the unit tests.
- Keep user-facing strings under the existing `MK_ISOMETRIC_BOARD.isometricBoard` localization namespace and CSS scoped to the module UI.
- Update [README.md](./README.md) when setup, supported behavior, or compatibility changes.

## Foundry v13 and v14 compatibility

- Preserve the minimum of v13 and support both generations. Prefer capability checks to hard-coded version branches.
- Scene controls use the keyed `controls.tokens.tools` collection and `onChange(event, active)`. Preserve native tools and tools owned by other modules.
- Use `TokenDocument.move` for board movement and `preMoveToken` to reject invalid destinations. Do not mutate Foundry movement waypoints or bypass its permissions, walls, regions, or animation workflow.
- v13 uses `getCenterPoint` for movement origins; v14 provides `getMovementOrigin`, including token-depth elevation. Preserve that elevation when substituting artwork-base coordinates. Temporary overrides must be restored even when native code throws.
- Extend `CONFIG.Token.rulerClass` through supported ruler style and label methods. Preserve the configured class and fall back to native behavior when the board is disabled or the token belongs to another Scene.
- Token drag methods prefixed with `_` are internal Foundry integration points. Check their availability and signatures in both API versions whenever changing their wrappers; retain native `this`, arguments, and return values on fallback paths.
- Keep the board below token artwork, preserve native grid visibility, and clean up module graphics, event listeners, animation frames, and tickers on teardown.
- Compare against the official [v13 API](https://foundryvtt.com/api/v13/) and [v14 API](https://foundryvtt.com/api/v14/) when changing Foundry integration. Unit tests use mocks and do not prove live compatibility. Report API review, automated tests, and live smoke tests separately; only raise the manifest's `compatibility.verified` after live verification.

## Behavior to preserve

- Store shared board geometry and color under Scene flags `mk-isometric-board.isometricBoard`; opacity and panel position are client preferences.
- Keep geometry editing and board configuration GM-only, and token movement limited to tokens the user may update.
- Preserve hidden-token and hidden-movement privacy in highlights, selectors, paths, and labels.
- Measure board movement as 5 feet per cell, including diagonals, and use complete token footprints for occupancy and range calculations.
- Board configuration must not reposition existing tokens or rewrite the native Scene grid. Guard against overlaps and footprints that extend outside the board.
- Remain independent of game systems and the original MK-Shadowdark module; retain this module's own flags, settings, and API namespace.

## Releases

- Keep the version and versioned download URL in [module.json](./module.json) consistent when preparing a release. Preserve the stable manifest URL.
- Do not bump versions or create release archives for routine changes unless the task includes release preparation.
