# Stable Video Diffusion のインストール

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

- 以下のエラーは PORT がすでに使われている場合発生。
- 以前に stable-diffusion-webui を使っていて正常に終わっていなかったようでした。

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
