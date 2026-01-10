# Playstation 3 controller を windows11 で使う

## DsHidMini を使って有線接続

- 配布元：[nefarius/DsHidMini](https://github.com/nefarius/DsHidMini)
- 記載時点の最新：[v2.2.282](https://github.com/nefarius/DsHidMini/releases/tag/v2.2.282.0)
- ダウンロード：[dshidmini_v2.2.282.0.zip](https://github.com/nefarius/DsHidMini/releases/download/v2.2.282.0/dshidmini_v2.2.282.0.zip)
- 解凍後、\dshidmini_v2.2.282.0\x64\ の **dshidmini.inf** を右クリックでインストールする。
- 次に、同じフォルダの **igfilter.inf** を右クリックでインストールする。
- ここで再起動が必要かもしれない
- \dshidmini_v2.2.282.0\x64\ にある **DSHMC.exe** を起動
- PS3 コントローラをUSBで接続する
- DSHMC.exe の画面でPS3 Controller があればOK

## BthPS3 を使って Bluetooth 接続

- 配布元：[nefarius/BthPS3](https://github.com/nefarius/BthPS3)
- 記載時点の最新：[v2.17.0](https://github.com/nefarius/BthPS3/releases/tag/setup-v2.17.0)
- ダウンロード：[bthps3_v2.17.0.zip](https://github.com/nefarius/BthPS3/releases/download/setup-v2.17.0/bthps3_v2.17.0.zip)
- Nefarius_BthPS3_Drivers_x64_arm64_v2.17.0.msi を実行
- ここで再起動が必要かもしれない
- \dshidmini_v2.2.282.0\x64\ にある **DSHMC.exe** を管理者権限で起動（右クリックで管理者として実行）

## 接続確認

- Windows の 設定 -> Bluettothとデバイス -> その他のデバイス 欄に PLAYSTATION(R)3 Controller が出ていると大丈夫です
- コントロールパネル
  - Bluetooth
    - Nefarius Bluetooth PS Emurator があります
  - Nefarius HID Devices
    - PLAYSTATION(R)3 Controller があります
    - XINPUT compatible HID device があります

## 動作テスト

- STEAM設定 -> コントローラ -> デバイスの入力をテスト　で動作確認しました
