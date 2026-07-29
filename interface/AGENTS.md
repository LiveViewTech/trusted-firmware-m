## Context

PSA Functional API headers and non-secure veneers. After SPE `install`, NSPE builds consume headers/libs from `api_ns/interface/`.

## Tech

- C headers under `interface/include/psa/` (see `interface/include/psa/README.rst`)
- NS API sources: `interface/src/tfm_*_api.c`, TrustZone client `interface/src/tfm_tz_psa_ns_api.c`
- CMake INTERFACE library `psa_interface` in `interface/CMakeLists.txt`

## Architecture

1. Secure partitions implement services inside SPE
2. NSPE links installed interface sources/headers from `CMAKE_INSTALL_PREFIX` (`api_ns` by default)
3. Calls cross the isolation boundary via PSA client APIs (IPC/SFN on the secure side)

## Patterns

- DO: include PSA headers from the installed `api_ns` tree for NSPE, not ad-hoc copies
- DO: read `interface/include/psa/README.rst` / `interface/include/mbedtls/README.rst` for provenance notes
- DON'T: implement crypto in NS when the partition is enabled — call through `tfm_crypto_api.c` veneers

## Key Files

- `interface/CMakeLists.txt` — interface target
- `interface/include/psa/crypto.h` — PSA Crypto API
- `interface/src/tfm_tz_psa_ns_api.c` — NS TrustZone PSA client
- `interface/src/tfm_crypto_api.c` — crypto veneers
- `cmake/install.cmake` — install layout into `api_ns`
