# Windows への リモートデスクトップ接続

## Microsoft Account での接続

- 参考：[Microsoftアカウントでリモートデスクトップ接続](https://zenn.dev/splusl_at/articles/windows-remotedesktop-microsoft-account)

### 前準備

- 接続される側のPCSの前準備
- 設定
  - システム
    - リモートデスクトップ
      - PC名をメモ
      - リモートデスクトップユーザ
        - 自分の Microsoft Account を「追加」で登録する
　- アカウント
    - サインインオプション
      - 追加の設定
        - セキュリティ工場のため、このデバイスでは Microsoft アカウント用に Windows Hello サインインのみ許可する(推奨)　を　「オフ」する
- [WINDOWS]-[L] でロック画面に
  - ログイン画面へ（スペースキーなど押して）
  - サインインオプション から パスワード入力で Microsoft Account のパスワードでログイン

- 接続する側のPC
- 「リモートデスクトップ接続」アプリを実行（スタートメニューから検索で）
- オプションの表示(O) を開く
  - コンピュータ(C): PC名を記載
  - ユーザ名: Microsoft Account を指定
  - 接続設定を保存しておけば便利です
  - 接続(N) で接続されるはず
