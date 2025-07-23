
# nvidia driver の インストール

- Delete NVIDIA

```bash
sudo apt --purge remove -y '*nvidia*'
sudo apt --purge remove -y '*cuda*'
sudo apt --purge remove -y '*cudnn*'
sudo apt autoremove
```

- NVIDIA Driver install
  - 参考記事：<https://qiita.com/tmasada/items/f77808c870c829c076fa>
  - NVIDIA 公式からダウンロードしてインストールが良い
  - ここで、自分のグラボを選ぶ：<https://www.nvidia.com/ja-jp/drivers/>
    - GeForce
    - GeForce RTX 50 Series
    - NVIDIA GeForece RTX 5070 Ti
    - Linux 64bit
    - Japanese
  - 以上を選択すると、2025年7月2日時点では、
    - 570.169 のドライバのページになる。
    - 「詳細を見る」 で、ダウンロード画面に。
    - NVIDIA-Linux-x86_64-570.168.run のダウンロードが始まる。
    - 以下でインストールできる

```bash
sudo sh NVIDIA-Linux-x86_64-570.168.run
```

- Pytorch のバージョンのチェック
  - <https://pytorch.org/get-started/locally/> にて、pytorch がどの CUDA バージョンに対応しているかを確認
    (2025.07.08 時点は以下を選択)
    - Stable (2.7.1)
    - Linux
    - Pip
    - Python
    - CUDA 11.8 or CUDA 12.6 or CUDA12.8
  - CUDA 12.8 を選択
    - 参考記事：<https://zenn.dev/yumizz/articles/73d6c7d1085d2f>
    - 手持ちの GPU 型番とCUDAの紐づけが必要
      - ここで確認：<https://en.wikipedia.org/wiki/CUDA#GPUs_supported>
        - 私は、Compute capability = 12.0 (Blackwell) なので
          CUDA SDK version = 12.8 であることがわかる
        - 上記 pytorch 対応情報からも CUDA 12.8 以下で良いことがわかる。

- CUDA install
  - <https://developer.nvidia.com/cuda-toolkit-archive> に 古いバージョンがある。
  　該当のバージョンのページへ。
  - CUDA 12.8.1 は、<https://developer.nvidia.com/cuda-12-8-1-download-archive>
    - Linux
    - x86_64
    - Ubuntu
    - 22.04
    - runfile (local)
  - 以上を選ぶと、sh スクリプトが現れる。これを実行すればよい。

```bash
wget https://developer.download.nvidia.com/compute/cuda/12.8.0/local_installers/cuda_12.8.0_570.86.10_linux.run
sudo sh cuda_12.8.0_570.86.10_linux.run
```

- cuda に対応したドライババージョンの入手
  - [Disiplay Driver Archive](https://www.nvidia.com/ja-jp/drivers/unix/linux-amd64-display-archive/)
    - [570.124.04](https://www.nvidia.com/ja-jp/drivers/details/241269/)

- cuda の インストーラで driver もインストールできる
  - EULA への accept で次のページへ
  - [x] Driver
    - [x] Driver 560.124.06
  - [x] CUDA Toolkit 12.8
  - [x] CUDA Demo Suite 12.8
  - [x] CUDA Documentationo 12.8
  - [ ] Kernel Objects
    - [ ] nvidia-Fs

```bash
sudo apt --purge remove -y '*nvidia*'
sudo apt --purge remove -y '*cuda*'
sudo apt --purge remove -y '*cudnn*'
sudo apt autoremove –y
wget https://developer.download.nvidia.com/compute/cuda/12.8.1/local_installers/cuda_12.8.1_570.124.06_linux.run
sudo sh cuda_12.8.1_570.124.06_linux.run
```

- .bashrc に以下のパス設定も入れ込む

```text
(.bashrc)
# Set PATHS for CUDA
export PATH=/usr/local/cuda-12.8/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-12.8/lib64:$LD_LIBRARY_PATH
```
