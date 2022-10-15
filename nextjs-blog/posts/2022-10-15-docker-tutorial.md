---
title: "Dockerチュートリアル Part1 ~ Part2"
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

## サンプル・アプリケーション

いきなりややこしくて混乱した

- getting-stareted → チュートリアル全体、実行すると80portでチュートリアルが読める(ただし全部English)
- getting-startedの中から一部を抜き出してきてimage化して実行、3000portでサンプルが動く

### Dockerfile

```Dockerfile
# syntax=docker/dockerfile:1
FROM node:12-alpine
RUN apk add --no-cache python2 g++ make
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

いまいち説明ないまま進んでいくので別途調査した  
[Dckerfileの基本的な書き方 | システム・アプリ・ゲームUnity｜オルターボ株式会社](https://alterbo.jp/blog/ryu2-2106/)

**FROM** はベースのイメージを指定していて、docker desktopに存在してなければ自動で取ってくる

**WORKDIR** は操作対象の設定。初期位置はどこかわからないが/appに移動しているイメージ  
起動時のカレントディレクトリもこれで変わるらしい

**RUN** はコマンドを実行している /appで実行したい内容があるため2回目が実行されている  
ここの工程が増えるほどサイズが大きくなるらしく、&&でつないで1行にするのを推奨らしい

**CMD** はコンテナ起動時に実行されるものでRUNとはまた別

**COPY** はdockerを実行するマシンのファイルを、コンテナの中にコピーする  
今回の例だとローカルのappディレクトリで実行するようにチュートリアルで指定されている  
ローカルの/appを、コンテナの中の/appにcopyしている  
コンテンツ的な箇所はローカルで開発(gitで管理)して、土台になる部分だけdockerに持たせたいから？

こちらにも大事そうな話があったのでメモ。  
どういうことをイメージしながらDockerfileを書けばよいのかなんとなくわかる

[Dockerfile のベストプラクティス — Docker-docs-ja 1.9.0b ドキュメント](https://docs.docker.jp/engine/articles/dockerfile_best-practice.html)

### docker build

ファイルを用意したらbuildを実行する

`` $ docker build -t getting-started . ``

tはimageにタグをつけるオプション。タグといってもほぼ名前みたいなもの？

末尾の.は、Dockerfileを検索するパスを示している。  
/app配下にDockerfileをつくり、/app配下でこのコマンドを実行しているので.で良いということ

### docker run

``$  docker run -dp 3000:3000 getting-started ``

imageを作成したのであとは3000番のportで実行(next.jsのローカルと被ってるから変えてもいいかも)

この章は一旦ここまで。
