# Repository Guidelines

## Project Structure & Module Organization
- `config/`: ZMK configuration and keymaps. Primary files are `config/cradio.keymap` and `config/cradio.conf`.
- `config/behaviors/`: Custom behavior definitions (`*.dtsi`) included by the keymap.
- `config/west.yml`: West manifest for ZMK and extra modules.
- `build.yaml`: GitHub Actions build matrix for left/right/reset firmware.
- `images/`: Layer screenshots referenced by `README.org` and `README-zh_CN.org`.
- `zephyr/module.yml`: Declares this repo as a Zephyr module.

## Build, Test, and Development Commands
- Local setup (one-time) to build without GitHub Actions:
  - `python3 -m venv .venv && source .venv/bin/activate`
  - `pip install west && west init -l config && west update && west zephyr-export`
  - Install Zephyr SDK and set `ZEPHYR_SDK_INSTALL_DIR`.
- `west build -s zmk/app -b nice_nano -- -DSHIELD=cradio_left`
  Builds the left half firmware (mirror CI’s left build in `build.yaml`).
- `west build -s zmk/app -b nice_nano -- -DSHIELD=cradio_right`
  Builds the right half firmware.
- `west build -s zmk/app -b nice_nano -- -DSHIELD=settings_reset`
  Builds the reset image to clear Bluetooth pairings.
- CI builds and publishes artifacts from the `build.yaml` matrix; prefer that flow if you don’t have a local ZMK toolchain.

## Coding Style & Naming Conventions
- Use 4-space indentation in `.keymap`, `.dtsi`, and `.conf` files; keep keymap diagrams aligned.
- Layer constants are uppercase (`MAC`, `WIN`, `RIG`), while node names use lower_snake_case (`default_layer`).
- Keep behavior includes grouped at the top of the keymap (`config/behaviors/*.dtsi`).

## Testing Guidelines
- No automated test suite in this repo.
- Validation is a successful firmware build plus a quick flash test on left/right halves.

## Commit & Pull Request Guidelines
- Commit messages are short, imperative, sentence case (e.g., “Add …”, “Update …”, “Swap …”).
- PRs should include:
  - A concise description of keymap/config changes.
  - Updated layer images in `images/` and references in `README.org` if layouts changed.
  - Notes on how the firmware was built or validated (CI or local).

## Configuration Notes
- Tune firmware behavior in `config/cradio.conf` (Bluetooth, battery, pointing, sleep).
- Adjust key behaviors and layers in `config/cradio.keymap` and `config/behaviors/`.
