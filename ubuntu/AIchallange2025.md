
# AI Challenge 2025 セットアップ手順

## [ワークスペースのクローン](https://automotiveaichallenge.github.io/aichallenge-documentation-2025/setup/workspace-setup.html)

### git の インストール

```sh
sudo apt update
sudo apt install -y git
```

---

### レポジトリのクローン

```sh
git clone https://github.com/AutomotiveAIChallenge/aichallenge-2025.git
```

## [仮想環境のインストール](https://automotiveaichallenge.github.io/aichallenge-documentation-2025/setup/docker.html)

---

### 必要なライブラリのインストール

```sh
sudo apt update
sudo apt install -y python3-pip ca-certificates curl gnupg
```

### Docker のインストール

```sh
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=\"$(dpkg --print-architecture)\" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  \"$(. /etc/os-release && echo \"$VERSION_CODENAME\")\" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker $USER
newgrp docker
sudo docker run hello-world
```

### rocker のインストール

- rocker のインストール
- .bashrc へのパス追加

```sh
pip install rocker
echo export PATH='$HOME/.local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

### Autoware環境のDockerイメージ取得

```sh
docker pull ghcr.io/automotiveaichallenge/autoware-universe:humble-latest
docker images
```

---

### 仮想環境作成

```sh
sudo apt update
sudo apt install build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev \
liblzma-dev
pyenv install 3.11.7
python -m venv .venv
source .venv/bin/activate
```

## [描画ありAWSIMの起動 (参考)](https://automotiveaichallenge.github.io/aichallenge-documentation-2025/setup/visible-simulation.html)

### NVIDIA driver インストール

- [nvidia インストール](nvidia-install.md)

---

### NVIDIA Container Toolkit インストール

```sh
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

---

### AWSIM のインストール

#### 下準備

```sh
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
  && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
        sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
        sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

#### インストール

```sh
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

#### 確認

```sh
sudo docker run --rm --runtime=nvidia --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
```

#### Vulkan のインストール

```sh
sudo apt update
sudo apt install -y libvulkan1
```

---

#### AWSIM download

- [AWSIM](https://tier4inc-my.sharepoint.com/personal/taiki_tanaka_tier4_jp/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Ftaiki%5Ftanaka%5Ftier4%5Fjp%2FDocuments%2FAutonomousAIChallenge&ga=1) 最新は確認してください
- 権限の変更

```bash
unzip AWSIM.zip
mv AWSIM aichallenge-2025/aichallenge/simulator
chmod -R 755 aichallenge-2025/aichallenge/simulator/AWSIM
```

#### AWSIM 起動確認

```sh
cd aichallenge-2025
./docker_build.sh dev
./docker_run.sh dev gpu
```

- ここではすでに docker の中にいる

```sh
cd /aichallenge
./build_autoware.bash
# 必要に応じて .bashrc を編集
./run_evaluation.bash
```

## [進め方](https://automotiveaichallenge.github.io/aichallenge-documentation-2025/development/workspace-usage.html)

- to be continued
