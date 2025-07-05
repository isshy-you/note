# WSL2 + Ubuntu 開発環境構築

- [Windows上のLinux開発環境構築ガイド：WSL 2とUbuntuのインストール・設定・運用](https://www.kkaneko.jp/cc/dev/wsl.html#S1)

---

## WSL2 のインストール（PowerShell）

```sh
wsl --install
wsl -l -v
wsl --list --online
wsl --install -d Ubuntu-22.04
wsl --set-default Ubuntu-22.04
wsl --export Ubuntu-24.04 ubuntu-backup.tar
wsl --import MyUbuntu C:\WSL\MyUbuntu ubuntu-backup.tar
```

---

## Ubuntu の初期セットアップ

```sh
sudo apt update
sudo apt -yV upgrade
sudo apt -yV dist-upgrade
sudo apt install gedit -y
sudo apt -y install build-essential python3-dev python3-pip wget ca-certificates
```

## ビルドツールのインストール

- [チュートリアル: WSL 2 と Visual Studio 2022 を使用して C++ をビルドおよびデバッグする](https://learn.microsoft.com/ja-jp/cpp/build/walkthrough-build-debug-wsl2?view=msvc-170)

```sh
sudo apt update
sudo apt install g++ gdb make ninja-build rsync zip
```

---

## pyenv のインストール

- [Ubuntuにpyenvをインストールする](https://qiita.com/middle_aged_rookie_programmer/items/0eb574e92a52c923e7ec)

```sh
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

`.bashrc` に以下を追記：

```sh
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

---

## CUDA Toolkit のインストール

### 方法1: パッケージでインストール

- [CUDA on WSL User Guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)

- CUDA 12.8 UPDATE 1 の場合

```sh
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.8.1/local_installers/cuda-repo-wsl-ubuntu-12-8-local_12.8.1-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-12-8-local_12.8.1-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-12-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-8
```

### 方法2: ランファイルでインストール（こちらが楽）

```sh
wget https://developer.download.nvidia.com/compute/cuda/12.8.1/local_installers/cuda_12.8.1_570.124.06_linux.run
sudo sh cuda_12.8.1_570.124.06_linux.run
```

`.bashrc` に以下を追記：

```sh
export PATH=/usr/local/cuda/bin:${PATH}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:${LD_LIBRARY_PATH}
```

---

## AI Challenge でのトラブル対策

- `facl` コマンドが無い場合

    ```sh
    sudo apt update
    sudo apt install acl
    ```

- `docker_build.sh` の `--progress=plain` を `--progress plain` に修正  
  （`=` を半角スペースに変更）

- エラーは取れるが、WSL2では動かない。バイナリが対応していないのだと思う。

---
