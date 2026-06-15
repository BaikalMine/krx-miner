# BaikalMine KRX Miner

High performance Keryx / OPoI CUDA miner build for BaikalMine.

## Download

Use the latest release:

https://github.com/BaikalMine/krx-miner/releases

Release assets:

- `keryx-miner-v0.1.1-OPoI-win64-amd64.zip`
- `keryx-miner-v0.1.1-OPoI-linux-amd64-cuda.tar.gz`
- `keryx-miner-v0.1.1-OPoI-hiveos.tar.gz`

## Supported GPUs

- NVIDIA RTX 30 series
- NVIDIA RTX 40 series
- NVIDIA RTX 50 series

The miner includes CUDA kernels for Ampere, Ada and Blackwell GPUs.

## Windows

1. Download and extract `keryx-miner-v0.1.1-OPoI-win64-amd64.zip`.
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
tar -xzf keryx-miner-v0.1.1-OPoI-linux-amd64-cuda.tar.gz
cd keryx-miner-v0.1.1-OPoI-linux-amd64-cuda
chmod +x keryx-miner
LD_LIBRARY_PATH="$PWD:${LD_LIBRARY_PATH}" ./keryx-miner --cuda-no-blocking-sync --keryxd-address stratum+tcp://krx.baikalmine.com:9020 --threads 0 --mining-address keryx:YOUR_WALLET.YOUR_WORKER
```

## HiveOS Flight Sheet

Create a Flight Sheet with a custom miner.

Use this install URL:

```text
https://github.com/BaikalMine/krx-miner/releases/download/v0.1.1/keryx-miner-v0.1.1-OPoI-hiveos.tar.gz
```

Use this pool URL:

```text
stratum+tcp://krx.baikalmine.com:9020
```

Recommended miner config:

```text
keryx-miner --cuda-no-blocking-sync --threads 0 --keryxd-address stratum+tcp://krx.baikalmine.com:9020
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
      "miner_alt": "keryx-miner-v0.1.1-OPoI",
      "miner_config": {
        "url": "stratum+tcp://krx.baikalmine.com:9020",
        "miner": "keryx-miner-v0.1.1-OPoI",
        "template": "%WAL%.%WORKER_NAME%",
        "install_url": "https://github.com/BaikalMine/krx-miner/releases/download/v0.1.1/keryx-miner-v0.1.1-OPoI-hiveos.tar.gz",
        "user_config": "keryx-miner --cuda-no-blocking-sync --threads 0 --keryxd-address stratum+tcp://krx.baikalmine.com:9020\n--mining-address %WAL%.%WORKER_NAME%"
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
