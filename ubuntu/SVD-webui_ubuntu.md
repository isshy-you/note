# Stable Video Diffusion の ubuntu へのインストール

- [【初心者向け】Stable Video Diffusionの使い方！画像から動画を簡単生成しよう](https://romptn.com/article/55309)

- PowerShell のインストール（SVD-webui のインストーラは Powershellのみ）
  - [PowerShell 7.5 (ubuntu)](https://learn.microsoft.com/en-us/powershell/scripting/install/install-ubuntu?view=powershell-7.5)
  - インストールの確認

    ``` pwsh
    cat /etc/shells
    ```

    - "/opt/microsoft/powershell/7/pwsh" があればOK

- SVD-webui の クローン
  - github : [sdbds/SDVD-webui](https://github.com/sdbds/SVD-webui)

    ```bash
    git clone --recurse-submodules https://github.com/sdbds/SVD-webui/
    ```

- python 3.10.6 のインストール

- ".venv" ではなく "venv" にしておく

  ``` pwsh
  cd SVD-webui
  pyenv install 3.10.6
  pyenv local 3.10.6
  python -m venv venv
  ./venv/bin/activate.ps1
  git submodule update --recursive --init
  ./install.ps1
  ```

- 自分の環境に合わせた pytorch をインストール(以下は CUDA 12.8の場合)
  - [pytorch.org](https://pytorch.org/get-started/locally/)
  - xformers も一緒にインストールしたほうが良さそう

  ``` pwsh
  pip3 uninstall torch torchvision torchaudio
  pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128 xformers
  ```

- モデルのダウンロード
  - svd.safetensors（14フレーム生成用、VRAM 15GB、約9GB）
    - [svd.safetensors](https://huggingface.co/stabilityai/stable-video-diffusion-img2vid/blob/main/svd.safetensors)
  - svd_xt.safetensors（25フレーム生成用、VRAM 18GB、約9GB）
    - [svd_xt.safetensors](https://huggingface.co/stabilityai/stable-video-diffusion-img2vid-xt/blob/main/svd_xt.safetensors)

- ダウンロードしたモデルを配置
  - SVD-webui/checkpoints 以下に置きます。

  ```pwsh
  mv svd.safetensors ~/SVD-webui/checkpoints
  ```

  - run_gui.ps1 を以下のように書き換えました

  ```txt
  $model_path="./checkpoints/svd.safetensors"
  $outputs="./outputs"
  $port=7860

  Set-Location $PSScriptRoot
  .\venv\bin\Activate.ps1

  $Env:HF_HOME = "./huggingface"
  $Env:XFORMERS_FORCE_DISABLE_TRITON = "1"
  $ext_args = [System.Collections.ArrayList]::new()

  if ($port -ne 7860) {
    [void]$ext_args.Add("--port=$port")
  }

  python3 webui.py `
  --model_path=$model_path `
  --outputs=$outputs $ext_args
  ```

  ```pwsh
  ./run_gui.ps1
  ```

- まだまだうまく動かない
