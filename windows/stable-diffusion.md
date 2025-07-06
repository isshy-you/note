# stable-diffusion-webui の windows へのインストール

## stable-diffusion-webui の クローン

```pwsh
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
```

## python 3.10.6 のインストール

- ".venv" ではなく "venv" にしておく

```pwsh
cd stable-diffusion-webui
pyenv install 3.10.6
pyenv local
python -m venv venv
.venv/Scripts/activate
```

## stable-diffusion-webui の実行

- 実行（ライブラリ類のダウンロードが始まり時間がかかる）

```pwsh
./webui-user.bat
```

## stable-diffusion-webui の実行でエラーとなったら

- CUDA のバージョンを合わせる。cuda 12.1 でインストールされる。
- バージョン不一致でエラーとなる

```pwsh
nvidia-smi
nvcc -V
```

``` nvidia-smi
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 576.80                 Driver Version: 576.80         CUDA Version: 12.9     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                  Driver-Model | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 5070 Ti   WDDM  |   00000000:02:00.0  On |                  N/A |
|  0%   46C    P8             31W /  300W |    2203MiB /  16303MiB |      2%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
```

```NVCC -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2025 NVIDIA Corporation
Built on Fri_Feb_21_20:42:46_Pacific_Standard_Time_2025
Cuda compilation tools, release 12.8, V12.8.93
Build cuda_12.8.r12.8/compiler.35583870_0
```

- [pytorch](https://pytorch.org/get-started/locally/) から 合ったバージョンにする

```pwsh
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
```

- 再び stable-diffusion-webui の実行

```pwsh
./webui-user.bat
```

- **これで動いた(^^)**

- [Stable Diffusion で使えるプロンプト大全](https://romptn.com/article/27449) をみてプロンプトを考えねば。。。

---
---
---

- 参考：[Stable-Diffusion WebUIがcuda云々で動かないときの対処法](https://zenn.dev/ziaensochan/articles/dc58b18c8cd203)
  - cuda 12.8 なので、requirements_versions.txt を編集する
  - これではだめだった

``` requirements_versions.txt
--extra-index-url https://download.pytorch.org/whl/cu121
torch
```

---

- [binary ddistribution for Windows 10 + NVIDIA](https://github.com/AUTOMATIC1111/stable-diffusion-webui/releases/tag/v1.0.0-pre)
  - 試していない
