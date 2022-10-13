---
title: "Docker学習メモ"
date: "2022-10-12"
---

## Dockerチュートリアル

まずは[公式のチュートリアル](https://docs.docker.jp/get-started/overview.html)から。  
続きは明日。

## Docker Desktopインストール

公式から持ってきて入れたら問題発生  
WSL2のinstallation is incompleteエラー。  
古いubuntuだとダメっぽいのでWSLのアプデから開始

公式参考に対応  

[WSLアップグレード](https://learn.microsoft.com/ja-jp/windows/wsl/install#step-2---update-to-wsl-2)

- wsl --list --online
  - install可能リスト
- wsl --install -d Ubuntu-20.04
  - installの実行
- wsl -l -v
  - 確認
- wsl -s Ubuntu-20.04
  - 切り替え
- wsl --set-version Ubuntu-20.04 2
  - version2に切り替え……だけど失敗

[カーネル更新プログラムパッケージ](https://learn.microsoft.com/ja-jp/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)  
これ入れてからv2切り替えコマンドでうまくいった

### docker desktop起動最初のチュートリアル

- alpineをclone
- alpineをbuild
- alpineを実行
- tagつけてhubにpush

だいたいこんな感じのことをしてた  
このあとはローカルに作ったコンテナの中で色々試していくらしい  
一応URL[http://localhost/tutorial/](http://localhost/tutorial/)

ユーザ直下にgetting-startedをcloneされたので折を見て消すこと
