# パソコン工房 ゲーミングPC LEVEL ∞ に Windows 11 と ubuntu 22.04 のデュアルブート環境 を

- [iiyama PC LEVEL-R789-LC265K-UKX-PALIT](https://www.pc-koubou.jp/products/detail.php?product_id=1155413)
  - case : middle tower
  - mother board : intel Z890
  - OS : Windows 11 pro
  - CPU : Core Ultra 7 265K (369mm 水冷クーラー)
  - Memory : 16GBx2 DDR5
  - Storage : 1TB NVMe対応 M.2 SSD
  - GPU : GeForece RTX 5070 Ti 16GB GDDR7 / intel graphics(CPU内蔵)
- これに 内蔵SSD(M.2 SSD 1TB) を追加で、お手軽にすることとした

## SSDのインストール

- 1GB のSSDを購入
- PCケースを開ける(左側の上川のネジを取り、パカッと開ける。ちょっと硬い場合は、頑張る。ネジを外せばはまっているだけ。)
- 2つ目のM2スロットにはめ込んだ
  - [マザーボード画像](https://www.pc-koubou.jp/web_images/image/z890_plusbto_fix_top.jpg)
  - グラボの下側の左側。
  - ネジ２つで放熱板だけついています
  - ネジを外してSSDをはめ込んで（斜めから指して抑え込む）
    - SSDのシールを外してチップむき出しにしました。放熱板に貼り付けるので
  - 放熱板とともにネジで閉める
- Windows からSSDを認識していることを確認
  - コントロールパネル→システムとセキュリティ→記憶域の管理→新しいプールと記憶域の作成
  でディスク２があればひとまず大丈夫。私は一旦NTFSでフォーマットして動作確認をしました。
  - CrystalDiskInfo 等を持っていれば、それでも確認可能。

---
