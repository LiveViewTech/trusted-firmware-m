## Context

Build-time Python tooling: partition manifest parsing, Kconfig helper, signing/conversion modules.

## Tech

- Python 3 + deps in `tools/requirements.txt` (Jinja2, PyYAML, imgtool, kconfiglib, cryptography, …)
- Entry: `tools/tfm_parse_manifest_list.py`
- Lists: `tools/tfm_manifest_list.yaml`, `tools/tfm_generated_file_list.yaml`
- Kconfig: `tools/kconfig/tfm_kconfig.py`

## Architecture

1. Manifest list points at partition YAML under `secure_fw/partitions/` (and platform overlays)
2. Parser + Jinja templates under `tools/templates/` emit generated load/info sources into the build
3. `tools/modules/` helpers support image encrypt/sign and binary conversion

## Patterns

- DO: install tools deps before configure: `pip3 install -r tools/requirements.txt`
- DO: register new native partitions in `tools/tfm_manifest_list.yaml`
- DON'T: hand-edit generated outputs that the parser owns — change YAML/templates instead
- DON'T: confuse `tools/requirements.txt` with docs-only `docs/requirements.txt`

## Key Files

- `tools/tfm_parse_manifest_list.py` — manifest codegen
- `tools/tfm_manifest_list.yaml` — partition list
- `tools/tfm_generated_file_list.yaml` — generated file list
- `tools/requirements.txt` — Python deps
- `tools/kconfig/tfm_kconfig.py` — Kconfig integration helper
