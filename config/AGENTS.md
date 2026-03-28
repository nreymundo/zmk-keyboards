# CONFIG KNOWLEDGE BASE

## OVERVIEW
Primary source of truth for local keyboard behavior. Edit Corne and Cradio here first; everything else downstream should follow from these files.

## WHERE TO LOOK
- `corne.keymap` — Corne layers, combos, macros, custom behaviors, conditional layers.
- `cradio.keymap` — Cradio/Sweep layers, combos, hold-tap behavior, settings layer.
- `corne.conf` — Corne board features: sleep, debounce, BLE, split, pointing, studio, display.
- `cradio.conf` — Cradio power, debounce, and keyboard identity settings.
- `west.yml` — Dependency manifest for upstream ZMK; environment boundary, not board behavior.
- `../build.yaml` — CI matrix deciding which board/shield combinations get built.

## EDITING RULES
- Start in `.keymap` when changing layout logic, combos, macros, layer bindings, or custom behaviors.
- Start in `.conf` when changing Kconfig-style board features, timing, radio, display, or power behavior.
- Keep `corne.*` and `cradio.*` paired mentally: same directory, different hardware scope, different feature depth.
- Update `../build.yaml` when a new board/shield target should be built in CI.
- Treat `../assets/keymaps/` as downstream output. Keep it consistent with source changes; do not use it as the primary editing surface.
- Touch `west.yml` only when changing upstream revision/module layout. It is not a shortcut for board-specific tweaks.

## CONVENTIONS
- Corne keymap is the more advanced config here: pointing bindings, mouse movement, studio unlock, richer behavior definitions.
- Cradio keymap is leaner and more layer-centric; keep naming and structure readable when extending it.
- Group related combos, macros, behaviors, and layers together instead of scattering them through the file.
- Keep comments short and useful; prefer documenting non-obvious timing or behavior choices.
- Put hardware capability switches in `.conf`, not inline in keymap logic.

## ANTI-PATTERNS
- Editing rendered YAML/SVG artifacts instead of the corresponding `.keymap` source.
- Moving board-specific settings into `west.yml`.
- Adding hardware targets in source files without updating `../build.yaml`.
- Copy-pasting Corne-specific features into Cradio without checking hardware support.
- Mixing feature flags, layout changes, and dependency updates in one unfocused edit.

## VERIFY
- Re-read the matching `.conf` after any keymap change that depends on hardware features.
- Confirm `build.yaml` still covers the intended board/shield combinations.
- Check that downstream keymap assets still match the source after CI redraw/build runs.
- Watch for devicetree, Kconfig, and unsupported-feature warnings in build output.
- For timing-sensitive edits, validate on hardware when possible; hold-tap and combo changes are easy to make syntactically correct but behaviorally wrong.
