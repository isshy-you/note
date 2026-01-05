# [stable-diffusion-webui(github)](https://github.com/AUTOMATIC1111/stable-diffusion-webui)

- git clone

```bash
sudo add-apt-repository ppa:deadsnakes/ppa
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui
```

- 仮想環境

```bash
cd stable-diffuison-webui
pyenv install 3.11.7
pyenv local 3.11.7
python -m venv venv
source venv/bin/activate
pip install --upgrade pip
```

- 環境設定
  - ライブラリ類のinstall が始まる。しばらく待つ。

```bash
./webui.sh
```

- 私のCUDA 12.8 に合わせて pytorch のインストール

```bash
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
```
