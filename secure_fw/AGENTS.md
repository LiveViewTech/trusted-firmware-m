## Context

Secure Processing Environment binary (`tfm_s`): SPM plus PSA secure partitions (Crypto, ITS, PS, Attestation, FWU) and NS agents.

## Tech

- C, CMake (`secure_fw/CMakeLists.txt`), partition Kconfigs under `secure_fw/partitions/*/Kconfig*`
- Backends: IPC or SFN (`CONFIG_TFM_SPM_BACKEND` in `config/config_base.cmake`)
- FIH wrappers around isolation HAL (`lib/fih/`, used from `secure_fw/spm/core/main.c`)

## Architecture

1. SPM entry `secure_fw/spm/core/main.c` — static boundaries, platform init, backend start
2. PSA dispatch `secure_fw/spm/core/psa_api.c`; backends `backend_ipc.c` / `backend_sfn.c`
3. Partitions under `secure_fw/partitions/` loaded from manifests (`tools/tfm_manifest_list.yaml`)
4. Shared SPE runtime in `secure_fw/shared/`

## Patterns

- DO: add/change services via partition YAML + `tools/tfm_manifest_list.yaml`, then regenerate through `tools/tfm_parse_manifest_list.py`
- DO: respect isolation level when touching SPM or partition boundaries (`TFM_ISOLATION_LEVEL`)
- DON'T: call platform peripherals before `tfm_hal_set_up_static_boundaries` in `spm/core/main.c`
- DON'T: bypass PSA APIs from NS — use `interface/` veneers after install

## Key Files

- `secure_fw/CMakeLists.txt` — `tfm_s` executable
- `secure_fw/spm/core/main.c` — SPM init
- `secure_fw/spm/core/psa_api.c` — PSA API core
- `secure_fw/partitions/crypto/crypto_init.c` — crypto partition
- `secure_fw/partitions/internal_trusted_storage/tfm_internal_trusted_storage.c`
- `secure_fw/partitions/protected_storage/tfm_protected_storage.c`
