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
