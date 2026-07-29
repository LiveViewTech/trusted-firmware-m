## Context

MCUboot-based BL2 secure boot (default `BL2=ON`). Authenticates SPE/NSPE images, then hands off to secure firmware.

## Tech

- C + MCUboot (fetched `v2.2.0` via `lib/ext/mcuboot/`, version in `config/config_base.cmake`)
- Kconfig: `bl2/ext/mcuboot/Kconfig`
- Flash map / security counter: `bl2/src/`

## Architecture

1. Entry `bl2/ext/mcuboot/bl2_main.c` — bootutil + FIH + crypto
2. Flash layout via `bl2/src/flash_map.c` (and extended/legacy variants under `bl2/ext/mcuboot/`)
3. Image signing artifacts installed under `api_ns/image_signing` after SPE build

## Patterns

- DO: replace sample keys in `bl2/ext/mcuboot/keys.c` and `bl2/ext/mcuboot/root-*.pem` for production
- DO: keep `MCUBOOT_PATH` / `MCUBOOT_VERSION` consistent when vendoring (`config/config_base.cmake`)
- DON'T: treat bundled PEMs as device secrets
- DON'T: mix signing layouts across platforms without checking `signing_layout.c.in`

## Key Files

- `bl2/ext/mcuboot/bl2_main.c` — BL2 entry
- `bl2/ext/mcuboot/keys.c` — key material (sample)
- `bl2/src/flash_map.c` — flash map
- `bl2/src/security_cnt.c` — security counter
- `bl2/CMakeLists.txt` — BL2 target wiring
