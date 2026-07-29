## Purpose

Zephyr fork of Trusted Firmware-M (v2.2.2): builds the Arm PSA Secure Processing Environment — secure boot, SPM isolation, and Crypto/ITS/PS/FWU/Attestation services — for Cortex-M and dual-core platforms.

## Project Snapshot

- Tech: C/C++/ASM, CMake >=3.21, Kconfig, Python tooling, Sphinx docs
- Type: single SPE firmware tree consumed as Zephyr module (`zephyr/module.yml`)
- Version fallback: `TFM_VERSION_MANUAL` in `cmake/version.cmake` → 2.2.2
- Subguides:
  - `bl1/AGENTS.md`
  - `bl2/AGENTS.md`
  - `secure_fw/AGENTS.md`
  - `platform/AGENTS.md`
  - `interface/AGENTS.md`
  - `tools/AGENTS.md`
  - `config/AGENTS.md`

## Commands

```bash
pip3 install -r tools/requirements.txt
pip3 install -r docs/requirements.txt
cmake -S . -B build -DTFM_PLATFORM=arm/mps2/an521
cmake --build build -- install
cmake -S . -B build -DTFM_PLATFORM=arm/mps2/an521 -GNinja -DTFM_TOOLCHAIN_FILE=toolchain_ARMCLANG.cmake -DCMAKE_BUILD_TYPE=Debug
cmake -S docs -B build_docs
cmake --build build_docs -- tfm_docs_refman_html tfm_docs_refman_pdf
cmake --build build_docs -- tfm_docs_userguide_html tfm_docs_userguide_pdf
git clone https://git.trustedfirmware.org/TF-M/tf-m-tests.git
```

- SPE install default: `build/api_ns` (`CMAKE_INSTALL_PREFIX` in `config/config_base.cmake`)
- Tests: separate `tf-m-tests` repo at revision `TF-Mv2.2.2` (`lib/ext/tf-m-tests/version.txt`); wire via `CONFIG_TFM_TEST_DIR` — see `docs/building/tests_build_instruction.rst`

## Conventions

- Commits: `git commit -s` (DCO Signed-off-by) — `docs/contributing/contributing_process.rst`
- SPDX: BSD-3-Clause on TF-M-owned files; BL2/MCUboot and `lib/ext` follow their own licenses (`docs/introduction/readme.rst`)
- Config: prefer CMake caches / profiles under `config/`; optional Kconfig via `USE_KCONFIG_TOOL` (`CMakeLists.txt`, `config/kconfig.cmake`)
- Toolchains: `TFM_TOOLCHAIN_FILE` — default `toolchain_GNUARM.cmake`; also `toolchain_ARMCLANG.cmake`, `toolchain_CLANG.cmake`, `toolchain_IARARM.cmake`
- Deps: default `DOWNLOAD` FetchContent; override with `MBEDCRYPTO_PATH` / `MCUBOOT_PATH` / `localrepos.cmake`

## Directory Map

- `bl1/` → `bl1/AGENTS.md`
- `bl2/` → `bl2/AGENTS.md`
- `secure_fw/` → `secure_fw/AGENTS.md`
- `platform/` → `platform/AGENTS.md`
- `interface/` → `interface/AGENTS.md`
- `tools/` → `tools/AGENTS.md`
- `config/` → `config/AGENTS.md`
- `lib/` — FIH (`lib/fih/`), logging, FetchContent wrappers (`lib/ext/`)
- `cmake/` — version, install, utils
- `docs/` — Sphinx sources; intro at `docs/introduction/readme.rst`
- `zephyr/` — module metadata only

## Gotchas

- **`TFM_ISOLATION_LEVEL` / `TFM_FIH_PROFILE` / `CONFIG_TFM_SPM_BACKEND`** in `config/config_base.cmake` change security semantics (defaults: level 1, FIH OFF, SFN).
- **`TFM_PLATFORM`** must resolve under `platform/ext/target/` (e.g. `arm/mps2/an521`) or an absolute out-of-tree path.
- **Sample boot keys** live in `bl2/ext/mcuboot/` (`keys.c`, `root-*.pem`) — not for production.
- Offline builds need `*_PATH` overrides or pre-populated FetchContent dirs; network required for default DOWNLOAD.
