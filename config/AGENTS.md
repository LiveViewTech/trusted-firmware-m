## Context

CMake/Kconfig defaults, profile presets, and config validation for SPE builds.

## Tech

- CMake caches: `config/config_base.cmake`, `config/set_config.cmake`, `config/check_config.cmake`
- Header config: `config/config_base.h`
- Profiles: `config/profile/profile_{small,medium,medium_arotless,large}.*`
- Optional Kconfig path: `config/kconfig.cmake` when `USE_KCONFIG_TOOL`
- PSA API test sizing: `config/tests/Kconfig.test_psa_api`

## Architecture

1. Root `CMakeLists.txt` runs `pre_config` → `set_config` or Kconfig → `post_config`
2. `TFM_PROFILE` selects a profile file that overlays base options
3. `check_config.cmake` enforces combinations after targets are declared

## Patterns

- DO: set `TFM_PLATFORM`, toolchain, and profile on the cmake command line before changing sources
- DO: pin dependency versions via `MBEDCRYPTO_VERSION` / `MCUBOOT_VERSION` in `config_base.cmake`
- DON'T: raise `TFM_ISOLATION_LEVEL` or enable `TFM_FIH_PROFILE` without platform support
- DON'T: ignore `CONFIG_TFM_SPM_BACKEND` when writing partition communication code

## Key Files

- `config/config_base.cmake` — primary caches (platform, BL1/BL2, isolation, FIH, deps)
- `config/set_config.cmake` — non-Kconfig config path
- `config/kconfig.cmake` — Kconfig config path
- `config/check_config.cmake` — consistency checks
- `config/profile/profile_small.cmake` — example profile
