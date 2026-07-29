## Context

Board/vendor HAL and targets. Almost all sources live under `platform/ext/` (`common/`, `driver/`, `accelerator/`, `target/`).

## Tech

- C + heavy CMake (`platform/CMakeLists.txt`, per-target CMake under `platform/ext/target/`)
- Kconfig: `platform/Kconfig.platform` and related
- Optional HW crypto: `platform/ext/accelerator/` when `CRYPTO_HW_ACCELERATOR`

## Architecture

1. Root selects `TFM_PLATFORM` → path under `platform/ext/target/<vendor>/...`
2. Common boot/fault hooks: `platform/ext/common/` (e.g. `boot_hal_bl2.c`, `faults.c`)
3. NS platform pieces install to `api_ns/platform` for NSPE builds
4. Vendors present: `adi`, `arm`, `armchina`, `cypress`, `infineon`, `lairdconnectivity`, `nordic_nrf`, `nuvoton`, `nxp`, `rpi`, `stm`

## Patterns

- DO: name platforms as relative paths from `platform/ext/target` (docs example: `arm/mps2/an521`)
- DO: put out-of-tree platforms behind an absolute `TFM_PLATFORM` path
- DON'T: edit a vendor tree expecting it to apply to unrelated boards
- DON'T: skip platform `post_config.cmake` hooks referenced from root `CMakeLists.txt`

## Key Files

- `platform/CMakeLists.txt` — platform aggregation
- `platform/ext/common/boot_hal_bl2.c` — BL2 boot HAL
- `platform/ext/common/faults.c` — fault handling
- `platform/Kconfig.platform` — platform Kconfig
- `config/config_base.cmake` — `TFM_PLATFORM` cache
