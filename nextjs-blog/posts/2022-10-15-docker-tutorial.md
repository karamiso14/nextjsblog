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
初期位置は/だが、ベースイメージの中で最後に読んだ場所になっているらしい。

**RUN** はコマンドを実行している /appで実行したい内容があるため2回目が実行されている  
ここの工程が増えるほどサイズが大きくなるらしく、&&でつないで1行にするのを推奨らしい

**CMD** はコンテナ起動時に実行されるものでRUNとはまた別

**COPY** はdockerを実行するマシンのファイルを、コンテナの中にコピーする  
今回の例だとローカルのappディレクトリで実行するようにチュートリアルで指定されている  
ローカルの/appを、コンテナの中の/appにcopyしている  
コンテンツ的な箇所はローカルで開発(gitで管理)して、土台になる部分だけdockerに持たせたいから？

**EXPOSE** ポートの指定。ここで指定した上で、docker run -pで指定すると繋がって動くようになる、らしい。

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

``$  docker run -dp 3000:3000 getting-started``

imageを作成したのであとは3000番のportで実行(next.jsのローカルと被ってるから変えてもいいかも)

## アプリケーションの更新

next.jsのtodoアプリの、初期表示文言を更新する。  
更新したら ``docker build -t getting-started .``でビルドし直す。

その後``docker run -dp 3000:3000 getting-started``を実行すると、3000ポートはすでに使ってるのでエラーが出る。

```sh
docker: Error response from daemon: driver failed programming external connectivity on endpoint cranky_cori (b87a5ace3500bc77a1e848ec7551ef88390ca004a6a4345d8341b1d2da145f35): Bind for 0.0.0.0:3000 failed: port is already allocated.
```

まずはdockerコンテナを停止し、削除する必要がある。

- CLIから削除
  - docker psでコンテナIDを調べたら
  - ``docker stop container-id``で停止
  - ``docker rm container-id``で削除
    - ``docker rm -rf container-id``なら一行で済む
- ダッシュボード(GUI)から削除
  - ゴミ箱ボタンを押すだけ

再度docker runすると、文言が変わっていることがわかる

注意点として、

- データが消えている
- 再構築を必要としないコードの編集方法など改良点がある

らしいので、のちのステップで見ていくっぽい

## アプリケーションの共有

docker imageをgithubみたいにまとめて管理するdocker hubの話

```sh
You can push a new image to this repository using the CLI

docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
```

pushを使うとローカルからhubへアップロードできる

pushの前にimageにtagをつける  
``docker tag getting-started karamso14/getting-started``

それからpush  
``docker push karamso14/getting-started``

[Play with Docker](https://labs.play-with-docker.com/)というお試し環境が提供されているので、まだイメージが作られてない環境から実行してみる  
4時間後にこのインスタンスは消滅する仕組み

ADD NEW INSTANCEするとターミナルが出てくるので、
``docker run -dp 3000:3000 karamso14/getting-started``  
これを実行してOPEN PORTから3000を入力すると、さっきのTodoサービスが立ち上がる（文言修正したやつ）

これでimageを他の人にdocker hub経由で渡せば同じ環境が立ち上がる状態になった。

## DBの保持

いわゆるデータの永続化の話  
コンテナの中に保存しても再起動すると消える(imageに存在しない)  
そこをなんとかするのがvolumeで、2種類ある  
一つは、名前付きボリューム(named volume)

サンプルアプリのデータはSQLiteに保存されている  
``/etc/todo/todo.db``  
1ファイルで構成されているので、これをvolumeに置く

まずvolumeの作成  
``docker volume create todo-db``  
一旦この時点で起動している、データが消えるコンテナは消しておく

それから、-vオプションでvolumeを指定してdocker runする  
``docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started``  
todoリストにデータを入れたあとコンテナを再起動しても、データが残る

実際getting-staretedの/appにデータが作られるわけじゃなく、実態は別のところにある。  
``docker volume inspect todo-db``こうするとjsonぽいのがでてくる  
それによると、``"Mountpoint": "/var/lib/docker/volumes/todo-db/_data"``というところに実体があり、ホストからは権限で触れないよう隔離してある。

次は、毎回image再構築をしないで済むようになるらしいバインドマウントについて。

## バインドマウントの使用

名前付きボリュームはシンプルで扱いやすいが、実態がどこにあるかよくわからなくなる。  
きっちりパスを指定して使うのがバインドマウント。  
ただしdriverが使えないなど名前付きボリュームに比べて若干機能は少ない？

ハンズオンしたら書く

次はdocker-composeの話