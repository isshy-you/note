# Ubuntuの標準ブートローダー GRUB の設定例

参考: [第743回　Ubuntuの標準ブートローダーであるGRUBを改めて見直す | gihyo.jp](https://gihyo.jp/)

---

## /etc/default/grub の編集例

```sh
GRUB_DEFAULT=2                # デフォルトでWindowsを起動
GRUB_TIMEOUT_STYLE=countdown  # カウントダウン表示
GRUB_TIMEOUT=10               # 10秒後に自動起動
GRUB_GFXMODE=1920x1080-24     # 解像度指定
GRUB_GFXPAYLOAD_LINUX=keep    # Linuxカーネルにも同じ解像度を適用
```

- `menu` では有線キーボード必須のため Bluetoothキーボードの場合 `countdown` を推奨
- この設定では、10秒でWindowsが自動起動
- Ubuntuは有線キーボードで選択して起動することになる
- NVIDIAのグラボがNVIDIAのドライバーで動き出したら、解像度が4kになって、字が小さく、描画が遅くなったので解像度を指定した。

---

## 設定反映コマンド

```sh
sudo update-grub
```
