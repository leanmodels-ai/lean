# lean

Run large mixture-of-experts models on hardware that should not fit them.

`lean` streams expert weights between VRAM, system RAM and disk on demand, so a
model whose weights are several times your VRAM still runs locally. A 122B model
serves from a single 24 GB card; the 397B models run on 48 GB.

Single binary. No Python, no Docker, no cloud dependency. Linux and Windows,
NVIDIA GPU required.

## Install

Linux:

```sh
curl -sSf https://leanmodels.ai/install.sh | sh
```

Windows (PowerShell):

```powershell
irm https://leanmodels.ai/install.ps1 | iex
```

Both install the CUDA build, which is what you want to run models on an NVIDIA
GPU. Pass `--cpu` / `-Cpu` for a CPU-only build - fine for `lean pull` and
`lean verify`, but far too slow to serve these models.

### The CUDA build needs the CUDA Toolkit, not just a driver

lean links cuBLAS, cuRAND and NVRTC directly, and those ship with the
[CUDA Toolkit](https://developer.nvidia.com/cuda-downloads) rather than with the
NVIDIA display driver. A machine with a working `nvidia-smi` but no toolkit is
missing them.

They are resolved by the OS before lean starts, so a missing one is not a lean
error message - it is `error while loading shared libraries` on Linux, or a
missing-DLL dialog on Windows. Both installers check for the libraries and tell
you if they are absent.

Both installers verify the download against the `SHA256SUMS` published with each
release and refuse to install on a mismatch. You can read either script before
running it: they are linked from every release below.

## Quick start

```sh
lean tune                    # what your hardware can run
lean pull lean-agent-light   # free, runs on 12 GB of VRAM
lean run lean-agent-light    # interactive
lean serve lean-agent-light  # OpenAI-compatible HTTP API
```

Full documentation lives at <https://leanmodels.ai/docs/>, and the model catalog
at <https://leanmodels.ai/models/>.

## What is in this repository

Released binaries, their checksums, and the installer scripts. Releases are
published here automatically when a version is tagged.

The inference engine itself is not open source and is not in this repository.
Issues and questions about `lean` are welcome here.

## Support

- Documentation: <https://leanmodels.ai/docs/>
- Models and pricing: <https://leanmodels.ai/models/>
