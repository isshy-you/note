# stable diffusion の windows へのインストール

## stable-diffusion-webui の windows へのインストール

### stable-diffusion-webui の クローン

```pwsh
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
```

### python 3.10.6 のインストール

- ".venv" ではなく "venv" にしておく

```pwsh
cd stable-diffusion-webui
pyenv install 3.10.6
pyenv local 3.10.6
python -m venv venv
.venv/Scripts/activate
```

### stable-diffusion-webui の実行

- 実行（ライブラリ類のダウンロードが始まり時間がかかる）

```pwsh
./webui-user.bat
```

### stable-diffusion-webui の実行でエラーとなったら

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

### エラー対策

## エラー

- 以下のエラーは PORT が使われている場合。 stable-diffusion-webui を実行したあとだと発生しました

```text
OSError: Cannot find empty port in range: 7860-7860. You can specify a different port by setting the GRADIO_SERVER_PORT environment variable or passing the `server_port` parameter to `launch()`.
```

- 以下で確認

```pwsh
 netstat -ano | findstr :7860
```

- 以下なら、 32124 というプロセスが使っている。これを殺せばOK。殺し方は、タスクマネージャが便利。

```text
  TCP         127.0.0.1:7860         0.0.0.0:0              LISTENING       32124
```

- 以下でも殺せる(番号は、上記で確認したプロセス番号を)

```pwsh
Stop-Process -Id 32124
```

### 起動順序

- インストール場所へ移動

```pwsh
cd .\stable-diffusion-webui\
```

- python 仮想環境立ち上げ

```pwsh
./venv/Scripts/activate.bat
```

- stable diffusion 実行

```pwsh
./webui-user.bat
```

- 私は、ここまでを run.bat ファイルにしています。

```run.bat
call ./venv/Scripts/activate.bat
./webui-user.bat
```

### Stable Diffusion を 動画拡張(animatediff)

- [【画像付き】AnimateDiffの使い方を解説！動かない時の対処法も紹介](https://ai-bo.jp/animatediff/)
- ここの通りにやったら動きました

### Stable Diffusion 参考資料

- [github](https://github.com/Stability-AI/stablediffusion)
- [stable-diffusion-webui/wiki/Features](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features)

---

## 今後のメモ

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

- [binary distribution for Windows 10 + NVIDIA](https://github.com/AUTOMATIC1111/stable-diffusion-webui/releases/tag/v1.0.0-pre)
  - 試していない
