## Context

Optional early boot (BL1_1 then BL1_2) when `BL1` and `PLATFORM_DEFAULT_BL1` are enabled in the root build. Handles OTP/provisioning paths and measures the next stage before BL2.

## Tech

- C + CMake + Kconfig (`bl1/Kconfig`)
- Image helpers: `bl1/bl1_1/scripts/`, `bl1/bl1_2/scripts/`
- Wired from root `CMakeLists.txt` only if `BL1 AND PLATFORM_DEFAULT_BL1`

## Architecture

1. BL1_1 (`bl1/bl1_1/main.c`) — platform init, crypto/OTP, authenticate/measure BL1_2
2. BL1_2 (`bl1/bl1_2/main.c`) — next-stage load toward BL2/SPE
3. Platform HAL supplies OTP/provisioning (`tfm_plat_otp`, `tfm_plat_provisioning` usage from SPM/boot paths)

## Patterns

- DO: gate BL1 work on platform defaults and `BL1` cache — see root `CMakeLists.txt` and `config/config_base.cmake`
- DO: keep provisioning scripts aligned with the selected `TFM_PLATFORM` under `platform/ext/target/`
- DON'T: assume BL1 is built for every platform (default `BL1` is OFF in `config/config_base.cmake`)

## Key Files

- `bl1/bl1_1/main.c` — BL1_1 entry
- `bl1/bl1_2/main.c` — BL1_2 entry
- `bl1/Kconfig` — BL1 Kconfig surface
- `bl1/bl1_1/CMakeLists.txt` / `bl1/bl1_2/CMakeLists.txt` — stage targets
