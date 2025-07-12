# YouTube動画をダウンロードしてDVDに焼く

YouTube動画をダウンロードし、DVDに焼くための手順をまとめたメモです。

## 1. YouTube動画をダウンロードする

このPythonスクリプトは `yt-dlp` を利用して、指定されたURLから動画をダウンロードします。

### Pythonスクリプト

```python
import yt_dlp
import os

# --- 設定 ---
# ダウンロードしたい動画のURLを設定します
video_url = input("ダウンロードしたいYouTube動画のURLを入力してください: ")

# 動画を保存するディレクトリを設定します
output_dir = "downloads"
os.makedirs(output_dir, exist_ok=True)

# --- yt-dlp オプション ---
# その他のオプションについては、以下を参照してください:
# https://github.com/yt-dlp/yt-dlp#embedding-yt-dlp
ydl_opts = {
    # 最高品質の映像と音声をダウンロードして結合します
    'format': 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best',
    # 出力ファイル名のテンプレートを設定します (動画タイトル.拡張子)
    'outtmpl': os.path.join(output_dir, '%(title)s.%(ext)s'),
    # ファイルにメタデータを追加します
    'addmetadata': True,
    # ファイルにサムネイルを追加します
    'writethumbnail': True,
}

# --- ダウンロード処理 ---
try:
    with yt_dlp.YoutubeDL(ydl_opts) as ydl:
        print(f"ダウンロードを開始します: {video_url}")
        ydl.download([video_url])
    print("\nダウンロードが完了しました！")
    print(f"動画は '{output_dir}' フォルダに保存されました。")
except Exception as e:
    print(f"\nエラーが発生しました: {e}")

```

## 2. DVD の作成

ここでは "DVD Flick" を使います

- [ここで紹介されています](https://www.gigafree.net/media/dvdconv/dvdflick.html)

- インストールしたら、動画ファイルをDrag&DropすればOKです
