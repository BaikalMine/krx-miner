# BaikalMine KRX Miner

High performance Keryx / OPoI CUDA miner build for BaikalMine.

## Download

Use the latest release:

https://github.com/BaikalMine/krx-miner/releases

Release assets:

- `keryx-miner-v0.1.2-OPoI-win64-amd64.zip`
- `keryx-miner-v0.1.2-OPoI-linux-amd64-cuda.tar.gz`
- `keryx-miner-v0.1.2-OPoI-hiveos.tar.gz`

## Supported GPUs

- NVIDIA RTX 20 series
- NVIDIA RTX 30 series
- NVIDIA RTX 40 series
- NVIDIA RTX 50 series

The miner includes CUDA kernels for Turing, Ampere, Ada and Blackwell GPUs.

## Model Selection

Model tiers are ceilings. The miner can automatically downgrade the selected tier when VRAM is too low, but it never upgrades above the selected mode.

- default: TinyLlama + DeepSeek-R1-8B maximum
- `--light`: TinyLlama only
- `--high`: DeepSeek-R1-32B maximum
- `--very-high`: LLaMA-3.3-70B maximum

For mixed GPU rigs, the weakest detected NVIDIA GPU limits the model tier.

## Windows

1. Download and extract `keryx-miner-v0.1.2-OPoI-win64-amd64.zip`.
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
tar -xzf keryx-miner-v0.1.2-OPoI-linux-amd64-cuda.tar.gz
cd keryx-miner-v0.1.2-OPoI-linux-amd64
chmod +x keryx-miner
LD_LIBRARY_PATH="$PWD:${LD_LIBRARY_PATH}" ./keryx-miner --cuda-no-blocking-sync --keryxd-address stratum+tcp://krx.baikalmine.com:9020 --threads 0 --mining-address keryx:YOUR_WALLET.YOUR_WORKER
```

## HiveOS Flight Sheet

Create a Flight Sheet with a custom miner.

HiveOS uses the custom miner `Name` / `miner_alt` as the install folder name.
Use this miner name:

```text
keryx-miner-v0.1.2-OPoI
```

Use this install URL:

```text
https://github.com/BaikalMine/krx-miner/releases/download/v0.1.2-beta/keryx-miner-v0.1.2-OPoI-hiveos.tar.gz
```

Use this pool URL:

```text
stratum+tcp://krx.baikalmine.com:9020
```

Recommended miner config:

```text
--threads 0 --keryxd-address stratum+tcp://krx.baikalmine.com:9020
--mining-address %WAL%.%WORKER_NAME%
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
      "miner_alt": "keryx-miner-v0.1.2-OPoI",
      "miner_config": {
        "url": "stratum+tcp://krx.baikalmine.com:9020",
        "miner": "keryx-miner-v0.1.2-OPoI",
        "template": "%WAL%.%WORKER_NAME%",
        "install_url": "https://github.com/BaikalMine/krx-miner/releases/download/v0.1.2-beta/keryx-miner-v0.1.2-OPoI-hiveos.tar.gz",
        "user_config": "--threads 0 --keryxd-address stratum+tcp://krx.baikalmine.com:9020\n--mining-address %WAL%.%WORKER_NAME%"
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

## Notes

- Do not share your generated `escrow.key`.
- Models are not bundled in the release archives.
- The miner uses local models when present and prepares required model files on first run.
- Keep `escrow.key` safe if you mine OPoI rewards.
- HiveOS archive root folder is `keryx-miner-v0.1.2-OPoI/` and must match `miner_alt`.
