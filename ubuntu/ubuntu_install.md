# ubuntu 22.04LTS と NVIDIA ドライバのインストール

## ubuntu 22.04LTS のインストール

- インストールUSBメモリを作成する
  - この辺に情報あり。私もRufsで。
    - <https://kantan-prog.com/ubuntu-install-usb/>
- BIOS(UEFI)（私は Level ∞ でして、UEFI Version : Z890-Plus 1.45.MS12）
  - BIOS への入り方
    - Windows から、SHIFTキーを押しながら再起動
    - 再起動してのメニュー
      - トラブルシューティング
        - 詳細オプション
          - UEFI ファームウェアの設定
    - 再起動
  - BIOS設定
    - Boot
      - FAST BOOT を Disable に
      - Save Changes and Exit
    - 再起動で DEL キーでも入れるようになる
    - Security
      - Secure Boot
        - Secure Boot を Disable に
      - Boot
        - Boot Option Priorities
        - 適切にBoot順を設定する
          - USB → ubuntu → Windows とする
          - Save Changes and Exit
          - 再起動
    - USB Ubuntu 起動
      - Try or Install Ubuntu （一番上）のメニューを選択
      - Alt-F7 で画面スクロール可能です（これ、焦りました）
      - 手動でパーティションを設定する
        - 500MB 程度でEFIパーティションを作成
        - 残りを ext4 で マウントポイント "/" に
        - インストール開始
      - インストールが終了したら再起動
    - GRUB のメニューが出る（ubuntu を優先起動に設定しておいたら）
      - Boot順は、USB → ubuntu → Windows がよい
    - Windows 起動確認
      - PINの再設定を求められた。
      - BIOS(UEFI)のSecure BootはDisableのまま運用しているが、Enableにしても大丈夫とは思うが、
        Video Card の ドライバのインストールなどで面倒なことになるので、可能なら Disable が良いと思う。
    - Ubuntu 起動確認
      - 普通に起動すればOK
- [GRUBの設定](GRUB.md)

- 再起動

```bash
sudo reboot
```

---

## nvidia driver の インストール

- Delete NVIDIA

```bash
sudo apt --purge remove -y '*nvidia*'
sudo apt --purge remove -y '*cuda*'
sudo apt --purge remove -y '*cudnn*'
sudo apt autoremove –y
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
wget https://developer.download.nvidia.com/compute/cuda/12.8.1/local_installers/cuda_12.8.1_570.124.06_linux.run
sudo sh cuda_12.8.1_570.124.06_linux.run
```

- .bashrc に以下のパス設定も入れ込む

```text
(.bashrc)
# Set PATHS for CUDA
export PATH=/usr/local/cuda-12.2/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-12.8/lib64:$LD_LIBRARY_PATH
```
