# PROJECT KNOWLEDGE BASE

**Generated:** 2026-03-28 Europe/Berlin
**Commit:** 28e8394
**Branch:** master

## OVERVIEW
Personal ZMK user-config repository for Nico's Corne and Cradio/Sweep layouts. Local source of truth lives in `config/`; CI builds firmware and redraws layout assets from that source.

## STRUCTURE
```text
./
├── config/              # hand-authored keymaps, board config, West manifest
├── assets/keymaps/      # drawer config plus generated-like YAML/SVG outputs
├── .github/workflows/   # thin wrappers around external reusable workflows
├── build.yaml           # CI build matrix for board/shield combinations
├── zephyr/module.yml    # local Zephyr module hook; board_root points here
├── boards/shields/      # placeholder only in current repo state
└── .zmk/                # embedded upstream ZMK workspace; reference context
```

## WHERE TO LOOK
| Task | Location | Notes |
|------|----------|-------|
| Change Corne or Cradio behavior | `config/*.keymap` | Primary authoring surface |
| Change board features or power settings | `config/*.conf` | Sleep, debounce, BLE, split, pointing, studio, display |
| Change upstream revision/source layout | `config/west.yml` | West manifest; not per-board logic |
| Add or remove CI firmware targets | `build.yaml` | Controls board/shield matrix |
| Change keymap rendering rules | `assets/keymaps/keymap_drawer.config.yaml` | Source for drawer output style |
| Understand CI behavior | `.github/workflows/*.yml` | Local wrappers delegate to upstream workflows |
| Understand Zephyr integration | `zephyr/module.yml` | Minimal local module config |
| Inspect upstream ZMK implementation | `.zmk/zmk/` | Read for reference; not normal local edit target |

## CONVENTIONS
- Treat `config/` as the canonical local source; treat `assets/keymaps/*.yaml` and `*.svg` as downstream artifacts.
- `build.yaml` is the root build surface. Keep board/shield selections aligned with files present in `config/`.
- `.github/workflows/build.yml` and `draw-zmk.yml` are wrappers around reusable workflows, not bespoke pipelines.
- `config/west.yml` points at upstream `zmkfirmware`; changes there affect the dependency boundary, not a single board.
- `zephyr/module.yml` is intentionally tiny. Keep root-level integration minimal.
- `.zmk/` is embedded upstream context. Read it to understand behaviors, docs, and tests; avoid casual edits there.

## ANTI-PATTERNS (THIS PROJECT)
- Editing `assets/keymaps/*.svg` or `assets/keymaps/*.yaml` first.
- Treating `.zmk/` as part of the normal local edit surface.
- Adding AGENTS or policy files under placeholder-only paths like `boards/shields/`.
- Duplicating logic from external reusable workflows into local workflow wrappers.
- Using `config/west.yml` for board-specific behavior that belongs in `.keymap` or `.conf` files.

## UNIQUE STYLES
- Repo is intentionally small at the root and relies on upstream ZMK for most firmware implementation.
- Corne uses a richer feature set than Cradio: pointing, split BLE, studio, and display options live in `corne.conf`.
- Keymap visualization is part of the normal workflow; rendered assets sit beside source, but are not the source.

## COMMANDS
```bash
# Sync upstream ZMK workspace
west update

# CI build matrix lives here
read build.yaml

# Firmware build entrypoints in GitHub Actions
read .github/workflows/build.yml
read .github/workflows/draw-zmk.yml
```

## NOTES
- No existing `AGENTS.md` hierarchy was present before this file.
- The biggest subtree by volume is `.zmk/`, but it is upstream-heavy and should be governed from here, not by nested local agent files.
- If `.github/workflows/` grows beyond thin wrappers, or if `.zmk/` becomes actively patched locally, re-evaluate the hierarchy.
- See `config/AGENTS.md` for the practical editing rules that apply to day-to-day layout changes.
