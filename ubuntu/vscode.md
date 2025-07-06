# install VScode

- [Visual Studio Code on Linux](https://code.visualstudio.com/docs/setup/linux#_install-vs-code-on-linux)

## download

- [.deb package(64bit)](https://go.microsoft.com/fwlink/?LinkID=760868)
- download page : [https://code.visualstudio.com/Download]

```bash
sudo apt install ./code_1.101.2-1750797935_amd64.deb
```

## setting

- SFHIT-CTRL-P でコマンドパレットに
  - "configure display language"

- git の設定
- ターミナルで以下の設定をしないと git commit で怒られる
  - git config --global user.email "e-mail address"
  - git config --global user.name "isshy-you(ubuntu)"
    - (ubuntu)と user.name に付けてどこからコミットしたかわかるようにしている
