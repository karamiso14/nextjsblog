---
title: '技術ロードマップ'
date: '2022-10-05'
---

## ざっくり仮置き

やりながら直しながら更新しながらやっていく  

- 基礎知識
  - 逆に何からやればいいのかわからん
  - 書籍から吸収がよさそう？
  - Git
    - [GithubSkills](https://skills.github.com/)
    - github-pagesのチュートリアルをやってみる
      - Jekyllというものを使ってるらしい
        - 静的サイトジェネレーターらしい
      - [成果物](https://karamiso13.github.io/githubpages/)
        - あんまりいいチュートリアルじゃない印象……
    - あとでGitの単独ページを作ってそこに飛ばすようにする
    - webhook的なのとかは使えそう。rebaseまわりも使えそう。
    - いつもの魔法
      - git commit -am "github-pages tutorial" --date="Oct 05 23:59:59 2022 +0900"
    - `git rebase -i commit_id`
      - コミットをまとめる。push前専用。
      - commit_idを入れた次～最新までが候補になる
      - pick を squashに書き換えて保存するとまとまる

- プログラミング能力向上
  - デザインパターン
  - アルゴリズム ぶっちゃけ学んだことがない

- Unreal Engine
  - 基礎くらいは分かってたほうが良いし、フロントバック両方やる的な雰囲気もあるらしい？
  - 公式チュートリアルをやる[リンク](https://www.unrealengine.com/ja/learn)
  - [いい感じにまとめてくれてるサイト](https://ue5study.com/unrealengine-basic-operation/)
  - [別ページへ](unreal-engine.md)
  - スペック足りなくてなんもで状態。一旦後回し。

- フロントエンド
  - HTML/CSS/javascript
    - シンプルでいいので自分でゼロから作れるように
  - next.js チュートリアルやった すごく良い
    - [Create a Next.js App | Learn Next.js](https://nextjs.org/learn/basics/create-nextjs-app)
    - 成果物はこちら というかココ
      - [Next.js Sample website](https://nextjsblog-karamiso14.vercel.app/)

- バックエンド
  - DBの深掘り
    - クエリチューニングとかパフォーマンスとか
  - Laravelの深掘り
    - ミドルウェアとかキャッシュまわり
  - Rubyやったことないのでやる
  - シェルスクリプト
    - Unix知識も
    - [jqコマンドとシェルスクリプトの上手い速い使い方](https://zenn.dev/ko1nksm/articles/4e93d16b45b5f2)

- テスト
  - オライリーのなんか読んだけど概念だけ、経験はなし

- CI/CD
  - テストと関連しそうだけど一応
  - jenkinsはもう古い？Circle CIとか？

- Docker
  - なにか勉強しようとしたときにサクっと立てられると便利
  - k8s、docker swarmあたりも？ECSに繋がるやつをやる
  - [別ページへのリンク](./docker.md)

- AWS
  - チュートリアルやハンズオンが充実してる
  - 金かかるし専門でやるつもりはないから後の方になりそう

## 参考リンクとか

[具体的にどう本物のエンジニアになるかというお話（バックエンドエンジニアのロードマップ付） - Qiita](https://qiita.com/mackeee-orange/items/afbed5ec3816d4af2e58)  
roadmapサービスは後で見てみてもいいかも。
