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

名前付きボリュームはDBの代わりに使って、データの保存をした。  
ここではバインドマウントをコードの即時反映のために使用する。

-vの後の指定が違う

- -v myvolume:/usr/local/data これは名前付きボリューム
- -v /path/to/data:/usr/local/data これがバインドマウント

```sh
docker run -dp 3000:3000 \
    -w /app -v "$(pwd):/app" \
    node:12-alpine \
    sh -c "yarn install && yarn run dev"
```

元々用意されていたyarn(パッケージリストみたいなもの？)でnodemonをインストールして動かすようになっている  
package.jsonにdevという名前でnodemonを呼ぶようになっていた

おそらく/app以下をnodemonが監視し、src/static/js/app.jsとかを編集すると自動でnodeを再起動しているっぽい？  
自動保存と相性悪いかもしれない。

次はdocker-composeの話

## 複数コンテナのアプリ

MySQLを追加するけど、同じコンテナに入れたりはしない。  
１つ１つのコンテナが、１つのことをしっかりと実行すべき。らしい

- DBとは別にAPIとフロントエンドをスケールする(?)
- コンテナを分けると現在のバージョンと更新したバージョンを分離できる
- 今はローカルのSQLiteをコンテナが使っているが、別のものを使いたくなるかも
- コンテナは基本１プロセスなので、複数プロセスになるとプロセスマネージャとやらを使わないといけなくなり複雑化する

解決方法は、コンテナ同士を同じネットワークに所属させること。

``docker network create todo-app``  
todo-appという名前のネットワークを作成

```sh
docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:5.7
```
-vのマウントで、コンテナ側が/var/lib/mysqlなのはどうやって調べるんだろうか？
一応(mysqlのdocker hub)[https://hub.docker.com/_/mysql/]に書いてあるっぽいが、わかるかこれ……？

todo-mysql-dataの名前付きボリュームはdockerが勝手に作ってくれるらしい。

mysqlに接続しようという段階で急にnicolaka/netshootコンテナをいれることになった  
ネットワークの問題に対するトラブルシュート用のツールがいっぱい入ってるらしい  
[nicolaka/netshoot: a Docker + Kubernetes network trouble-shooting swiss-army container](https://github.com/nicolaka/netshoot)

``docker run -it --network todo-app nicolaka/netshoot``  
これでコンテナ実行して、mysqlのIPとかを調べに行く。

``dig mysql``  
docker実行時に、netowrk-aliasでmysqlってつけたのがここで効く。  

```dig
;; ANSWER SECTION:
mysql.                  600     IN      A       172.18.0.2
```

Aレコードのことはあんまりわからんが名前解決していることがわかる。

todoアプリ側に用意されている環境変数に諸々設定していく。  
開発時はいいけど、本番環境では環境変数を通した設定はよくないらしい。  
説明してるブログは意味わからんかった。

todoアプリでtodoを作ると、mysqlにデータとして登録されているのがわかる。  
特にコード変えてないのに、SQliteからmysqlになったのは環境変数の有無で動作が切り替わるようになっていたからか……？
この辺は実際に動かす時ハマりそう。

とりあえずこれでMySQLのコンテナと連携することができた。  
次は、DockerComposeでアプリ立ち上げの手間とかを減らす。

## DockerComposeを使う

複数コンテナを管理する仕組み  
利点は、どのアプリを使うのかなどをファイルに定義し、バージョン管理し、誰でも簡単に実行して環境を作れるようになること。

基本的に、長ったらしいdocker runコマンドをdocker-compose.ymlにちょっと違う書き方で入れていく。長いので省略

```yml
version: "3.7"

services:
  app:
    image: node:12-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:5.7
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

docker runの時自動で作られていたmysql用の名前付きボリュームは、最後の行のところにあるようにdocker comopseでは明示的に作成しないといけない。

あとはymlのあるところで``docker-compose up -d`` するだけ。  
networkは自動で作ってくれるから、ymlには書いていない。

``docker-compose logs -f``でそれぞれのコンテナのログが入り混じったやつが見られる  
-fのあとにappとかつけると、そのコンテナの分だけ見られる。

``docker-compose down``でコンテナを止められる。  
volumeは消えないので、--volumesフラグも追加すること。  
ダッシュボードからは指定できないのでvolumeは別で消す。

次はDockerfileのベストプラクティスについて。  
実は今まで使ってたものには問題があるらしい

## イメージ構築のベストプラクティス

``docker scan``コマンドを使うと、セキュリティ脆弱性とかをチェックしてくれる。  
docker hubへのログインが必須。

``docker image history image名``  
これで階層化の順番が見られる。  
--no-trunc オプションを追加すると、省略された行も出てくる

Dockerfile内のcopyの順序を変えることで、無駄な依存関係の更新を避けられる。  
先にpackage.jsonとかyarn.lockをコピーしておいてから、他のファイルをコピーする。  
これで依存関係に変化があった時以外は、そっちのレイヤキャッシュ更新が走らなくなる。

あとはnodejsならnode_modulesを.dockerignoreに追加するなどのテクがある。

build時のstepで、using cacheが走ってるかどうかでキャッシュが効いているかどうかがわかる。