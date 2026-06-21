# BaikalMine KRX Miner

High performance Keryx / OPoI CUDA miner build for BaikalMine.

## Download

Use the latest release:

https://github.com/BaikalMine/krx-miner/releases

Release assets:

- `keryx-miner-v0.1.3-OPoI-win64-amd64.zip`
- `keryx-miner-v0.1.3-OPoI-linux-amd64-cuda.tar.gz`
- `keryx-miner-v0.1.3-OPoI-hiveos.tar.gz`

## Supported GPUs

- NVIDIA RTX 20 series
- NVIDIA RTX 30 series
- NVIDIA RTX 40 series
- NVIDIA RTX 50 series

The miner includes CUDA kernels for Turing, Ampere, Ada and Blackwell GPUs.

## Model Selection

Model tiers are ceilings. The miner can automatically downgrade the selected tier when VRAM is too low, but it never upgrades above the selected mode.

- default: TinyLlama + DeepSeek-R1-8B maximum
- `--light`: TinyLlama only, best for 6-8 GB cards
- `--high`: DeepSeek-R1-32B maximum, requires a 24 GB class card
- `--very-high`: LLaMA-3.3-70B maximum, requires a 32 GB class card

For mixed GPU rigs, the weakest detected NVIDIA GPU limits the model tier.

Approximate automatic caps:

- `<10 GB VRAM`: light
- `10-22 GB VRAM`: default
- `23-29 GB VRAM`: high
- `30+ GB VRAM`: very-high

Before mining starts, every selected model is verified on disk and the CUDA OPoI runtime is probed. Models are loaded only when OPoI work is actually requested, so PoW mining is not blocked by loading large models during startup. If no model is verified, PoW mining does not start. CPU OPoI is never used unless `--cpu-inference` is explicitly set.

## CUDA Performance

The CUDA build includes native PTX for RTX 20/30/40/50 families. Workload starts from a conservative family-specific default, then auto-tunes toward the fastest stable workload:

- fast stable runs can raise workload gradually
- slow runs or CUDA launch errors lower workload automatically
- the tuner remembers the last stable workload and avoids repeatedly pushing into known-bad ranges
- `--cuda-workload` can still be used for manual tuning
- `--cuda-no-blocking-sync` is recommended for low-latency pool mining

The live console dashboard separates raw GPU speed from pauses:

- `HASHRATE`: active CUDA speed while the GPU is hashing
- `60S AVG`: effective one-minute average including OPoI pauses and waiting time

## Windows

Requirements:

- NVIDIA driver with CUDA support
- CUDA runtime/cuBLAS DLLs available in `PATH` or next to `keryx-miner.exe`

1. Download and extract `keryx-miner-v0.1.3-OPoI-win64-amd64.zip`.
2. Edit `start-keryx-pool.bat`.
3. Replace the wallet in `--mining-address`.
4. Run `start-keryx-pool.bat`.

Example:

```bat
keryx-miner.exe --cuda-no-blocking-sync --keryxd-address stratum+tcp://krx.baikalmine.com:9020 --threads 0 --mining-address keryx:YOUR_WALLET.YOUR_WORKER
```

## Linux

Requirements:

- x86_64 Linux
- NVIDIA driver with CUDA support
- CUDA 12 runtime libraries available on the system

Run:

```bash
tar -xzf keryx-miner-v0.1.3-OPoI-linux-amd64-cuda.tar.gz
cd keryx-miner-v0.1.3-OPoI-linux-amd64-cuda
chmod +x keryx-miner
LD_LIBRARY_PATH="$PWD:${LD_LIBRARY_PATH}" ./keryx-miner --cuda-no-blocking-sync --keryxd-address stratum+tcp://krx.baikalmine.com:9020 --threads 0 --mining-address keryx:YOUR_WALLET.YOUR_WORKER
```

## HiveOS Flight Sheet

Create a Flight Sheet with a custom miner.

HiveOS uses the custom miner `Name` / `miner_alt` as the install folder name.
Use this miner name:

```text
keryx-miner-v0.1.3-OPoI
```

Use this install URL:

```text
https://github.com/BaikalMine/krx-miner/releases/download/v0.1.3/keryx-miner-v0.1.3-OPoI-hiveos.tar.gz
```

Use this pool URL:

```text
stratum+tcp://krx.baikalmine.com:9020
```

Recommended miner config:

```text
--threads 0 --keryxd-address stratum+tcp://krx.baikalmine.com:9020 --mining-address %WAL%.%WORKER_NAME%
```

Example HiveOS Flight Sheet JSON:

```json
{
  "name": "BaikalMine KRX",
  "isFavorite": false,
  "items": [
    {
      "coin": "KRX",
      "pool_ssl": false,
      "wal_id": 0,
      "dpool_ssl": false,
      "miner": "custom",
      "miner_alt": "keryx-miner-v0.1.3-OPoI",
      "miner_config": {
        "url": "stratum+tcp://krx.baikalmine.com:9020",
        "miner": "keryx-miner-v0.1.3-OPoI",
        "template": "%WAL%.%WORKER_NAME%",
        "install_url": "https://github.com/BaikalMine/krx-miner/releases/download/v0.1.3/keryx-miner-v0.1.3-OPoI-hiveos.tar.gz",
        "user_config": "--threads 0 --keryxd-address stratum+tcp://krx.baikalmine.com:9020 --mining-address %WAL%.%WORKER_NAME%"
      },
      "pool_geo": []
    }
  ]
}
```

Set `wal_id` to your HiveOS wallet id, or create the Flight Sheet manually in the HiveOS UI.

## Developer Fee

- BaikalMine pools (`*.baikalmine.com`): `1.5%`
- Other pools: `3%`

For stratum pool mining, the fee session is pre-warmed near the fee window and closed after cooldown. Main mining does not need to reconnect when the fee window starts or ends.

## Notes

- Do not share your generated `escrow.key`.
- Models are not bundled in the release archives.
- The miner uses local models when present and verifies every model file against the built-in raw SHA-256 manifest before marking it ready.
- The miner probes CUDA startup kernels before mining, so missing CUDA runtime libraries and PTX/JIT errors stop before shares are submitted.
- Missing models download from the Hugging Face mirror first, then fall back to the IPFS gateway.
- To use your own model mirror, set `KERYX_MODEL_BASE_URL` to a folder with the same layout and file names, for example `https://models.example.com/keryx`.
- HiveOS upgrades migrate an existing `models/` directory from older `keryx-miner-v*` custom miner folders before startup.
- Keep `escrow.key` safe if you mine OPoI rewards.
- HiveOS archive root folder is `keryx-miner-v0.1.3-OPoI/` and must match `miner_alt`.
