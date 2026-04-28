# KRX Miner for BaikalMine

Windows GPU miner package for Keryx/KRX mining on the BaikalMine pool.

## English Guide

### Download

Download the Windows release archive:

`releases/krx-miner-windows-v0.2.1-gpu-0.7.zip`

Extract the archive to any folder, for example:

`D:\Mine\keryx miner`

### Files

- `keryx-miner.exe` - miner executable
- `keryxcuda.dll` - CUDA GPU plugin
- `keryxopencl.dll` - OpenCL GPU plugin
- `start-keryx-pool.bat` - ready-to-use BaikalMine pool launcher

### Pool Configuration

The included `start-keryx-pool.bat` uses:

```bat
keryx-miner.exe --keryxd-address stratum+tcp://krx.baikalmine.com:9020 --threads 0 --mining-address keryx:qqmkzvmaqavsddukx4h0qszv8mfeheqevzzl7n6u2w8a4zvtvczcyp4ms4x3u
```

Replace the wallet address with your own Keryx address before mining.

### First Run

On first launch, the miner may download TinyLlama model files. This can be about 2.2 GB and happens once.

### Antivirus Notice

Mining software is often flagged by antivirus products. If Windows removes the miner, add your miner folder to antivirus exclusions, then extract the archive again.

### GPU Notes

For NVIDIA GPUs, install recent NVIDIA drivers. For AMD or other OpenCL devices, install drivers with OpenCL support.

## Русский гайд

### Скачать

Скачайте Windows-архив:

`releases/krx-miner-windows-v0.2.1-gpu-0.7.zip`

Распакуйте архив в любую папку, например:

`D:\Mine\keryx miner`

### Файлы

- `keryx-miner.exe` - исполняемый файл майнера
- `keryxcuda.dll` - GPU-плагин для CUDA
- `keryxopencl.dll` - GPU-плагин для OpenCL
- `start-keryx-pool.bat` - готовый запуск для пула BaikalMine

### Настройка пула

В комплекте есть `start-keryx-pool.bat` со строкой:

```bat
keryx-miner.exe --keryxd-address stratum+tcp://krx.baikalmine.com:9020 --threads 0 --mining-address keryx:qqmkzvmaqavsddukx4h0qszv8mfeheqevzzl7n6u2w8a4zvtvczcyp4ms4x3u
```

Перед запуском замените адрес кошелька на свой Keryx-адрес.

### Первый запуск

При первом запуске майнер может скачать модель TinyLlama. Размер около 2.2 GB, скачивается один раз.

### Антивирус

Антивирусы часто ругаются на майнеры. Если Windows удаляет `keryx-miner.exe`, добавьте папку майнера в исключения антивируса и распакуйте архив заново.

### GPU

Для NVIDIA установите актуальные драйверы NVIDIA. Для AMD или других OpenCL-устройств нужны драйверы с поддержкой OpenCL.
