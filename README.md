# wsl-RTL8188EU

Build kernel modules for the Realtek RTL8188EU Wi‑Fi driver targeting a WSL kernel.

## What this does
This repository provides a GitHub Actions workflow that:
- Fetches the matching WSL kernel sources (by tag or kernel release),
- Prepares kernel headers,
- Clones a driver repo (default: `https://github.com/lwfinger/rtl8188eu`),
- Builds the out‑of‑tree module and uploads the built `.ko` artifacts.

See the workflow definition at [.github/workflows/wsl-RTL8188EU.yml](.github/workflows/wsl-RTL8188EU.yml).

## Quick usage
1. Open the workflow in the Actions tab and "Run workflow" (or use the workflow_dispatch input UI).
2. Provide either:
   - `wsl_kernel_tag` (exact tag, e.g. `linux-msft-wsl-6.6.36.1`), or
   - `kernel_release` (your `uname -r`, e.g. `6.6.36.1-microsoft-standard-WSL2`).
3. Optionally override `driver_repo` and `module_name`.
4. After the run completes, download the artifact named `{module_name}-{tag}` which contains `*.ko` files.

The workflow file documents the inputs and behavior: [.github/workflows/wsl-RTL8188EU.yml](.github/workflows/wsl-RTL8188EU.yml).

## Installing the module into WSL
- Download the `.ko` artifact from the workflow run.
- Copy the module into WSL and install (example):
  - sudo cp <module>.ko /lib/modules/$(uname -r)/kernel/drivers/net/wireless/
  - sudo depmod -a
  - sudo modprobe <module_name>

Notes: installation may require a custom WSL kernel that permits loading modules. The workflow builds modules against the exact WSL kernel tag you specify.

## License
This project is licensed under the Apache‑2.0 License. See [LICENSE](LICENSE) for details.
