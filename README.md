# lean

Run large mixture-of-experts models on hardware that should not fit them.

`lean` streams expert weights between VRAM, system RAM and disk on demand, so a
model whose weights are several times your VRAM still runs locally. A 122B model
serves from a single 24 GB card; the 397B models run on 48 GB.

Single binary. No Python, no Docker, no cloud dependency. Linux and Windows,
NVIDIA or AMD GPU, including both in one machine.

## Install

Linux:

```sh
curl -sSf https://leanmodels.ai/install.sh | sh
```

Windows (PowerShell):

```powershell
irm https://leanmodels.ai/install.ps1 | iex
```

There is one build per platform. It runs on NVIDIA and AMD GPUs, and on the
CPU when it finds neither: fine for `lean pull` and `lean verify`, far too slow
to serve these models.

### What your GPU needs

- **Windows: only the graphics driver.** The first time lean uses the GPU it
  downloads the libraries the driver does not include (NVIDIA's CUDA libraries,
  about 330 MB, or AMD's HIP libraries, 65 to 185 MB), once, verified against
  checksums built into lean. `LEAN_NO_RUNTIME_DOWNLOAD=1` turns that off if
  you would rather install the CUDA Toolkit or AMD HIP SDK yourself. The Visual
  C++ runtime ships in the zip next to `lean.exe`.
- **Linux: the driver plus the vendor's runtime libraries.** For NVIDIA,
  cuBLAS, cuRAND and NVRTC from the
  [CUDA Toolkit 12](https://developer.nvidia.com/cuda-downloads). For AMD,
  [ROCm](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/). The
  installer checks for them and names anything missing.

Supported GPUs (RTX 30/40 series; RDNA 3 and RDNA 4 Radeon cards) are listed at
<https://leanmodels.ai/docs/hardware/>.

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
It is licensed under the terms in [LICENSE.txt](LICENSE.txt), which every
release archive also carries along with `THIRD-PARTY-NOTICES.txt`; `lean license`
prints both. Issues and questions about `lean` are welcome here.

## Support

- Documentation: <https://leanmodels.ai/docs/>
- Models and pricing: <https://leanmodels.ai/models/>
