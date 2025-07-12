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

---

## stable-video-diffusion の windows へのインストール

- [【初心者向け】Stable Video Diffusionの使い方！画像から動画を簡単生成しよう](https://romptn.com/article/55309)

- SVD-webui の クローン

```pwsh
git clone --recurse-submodules https://github.com/sdbds/SVD-webui/
```

- python 3.10.6 のインストール

- ".venv" ではなく "venv" にしておく

```pwsh
cd SVD-webui
pyenv install 3.10.6
pyenv local 3.10.6
python -m venv venv
.venv/Scripts/activate
git submodule update --recursive --init
./install.ps1
```

- なぜか、Windows の問題とかでエラーになったので、
  - [Maximum Path Length Limitation](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=powershell#enable-long-paths-in-windows-10-version-1607-and-later) から 管理者でPowerShellを開いて、

```pwsh
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" `
-Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```

- モデルのダウンロード
  - svd.safetensors（14フレーム生成用、約9GB）
    - [svd.safetensors](https://huggingface.co/stabilityai/stable-video-diffusion-img2vid/blob/main/svd.safetensors)
  - svd_xt.safetensors（25フレーム生成用、約9GB）
    - [svd_xt.safetensors](https://huggingface.co/stabilityai/stable-video-diffusion-img2vid-xt/blob/main/svd_xt.safetensors)

```pwsh
mv *.safetensors checkpoints
```

```pwsh
./run_gui.ps1
```

- なぜか、"omegaconf" モジュールがないと言われるので、

```pwsh
pip install omegaconf
```

- pytorch もないと言われるので、
- [pytorch](https://pytorch.org/get-started/locally/) から 合ったバージョンにする

```pwsh
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
```

- note : 以下で環境チェックが可能

```pwsh
 python -m torch.utils.collect_env
```

- まだまだ足りないモジュールがある

```pwsh
pip install opencv-python
pip install einops
pip install pytorch_lightning
pip install kornia
pip install open_clip_torch
pip install transformers
pip install scipy
```

- これでようやく、立ち上がった(^^)
  - open_clip_model.safetensors 3.94GB のダウンロードが始まります
  - が、エラーで途中終了。。。

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

- 以下のエラーは、ライブラリが足りない。

```text
NameError: name 'xformers' is not defined
```

```pwsh
pip install -U xformers --index-url https://download.pytorch.org/whl/cu128
```

- 以下のエラーはまだ未解決

```text
SVD-webui\venv\lib\site-packages\torch\_dynamo\eval_frame.py:838: UserWarning: torch.utils.checkpoint: the use_reentrant parameter should be passed explicitly. In version 2.5 we will raise an exception if use_reentrant is not passed. use_reentrant=False is recommended, but if you need to preserve the current default behavior, you can pass use_reentrant=True. Refer to docs for more details on the differences between the two variants.
  return fn(*args, **kwargs)

SVD-webui\venv\lib\site-packages\torch\utils\checkpoint.py:86: UserWarning: None of the inputs have requires_grad=True. Gradients will be None
  warnings.warn(

CUDA error (C:/a/xformers/xformers/third_party/flash-attention/hopper\flash_fwd_launch_template.h:188): invalid argument

```

```text
use_reentrant パラメータの警告
PyTorch 2.5 以降では、torch.utils.checkpoint() に use_reentrant を 明示的に指定しないとエラーになります。

requires_grad=True がない警告
これは、チェックポイント対象の関数に渡される 入力テンソルがすべて勾配不要になっている場合に出ます。

CUDA error: invalid argument
これは xformers の Flash Attention 実装で、CUDA カーネルに 不正な引数が渡されたときに出るエラーです。
```
