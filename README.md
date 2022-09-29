# next.js Tutorial Blog

## ファイルツリー覚書

- pages
  - index.js　トップページ
  - _app.js　グローバルCSSを当てるためのやつ？アプリ全体をコンポーネントとして見てる？
  - posts　ルーティング用
    - first-post.js　チュートリアルで作成するページ
- components
  - layout.js　ただのコンポーネント。一番外側のタグで中身.containerで囲む
  - layout.module.css　コンポーネント単位にCSS反映　スコープで名前が被らない
- public
  - 画像とかおくところ
- styles グローバル用のCSSをおくとこ？
  - global.css　グローバルcss本体
  - utils.module.css　いろいろなCSSをまとめたやつ　ページをまたいでつかう系？
- posts　外部ファイル読み込みの項目で使うやつ

- npm run dev でlocalhost:3000にローカル起動
- ついでにvcsでmarkdownが書けるように拡張を入れる
  - markdown all in one
  - markdown lint

## 9/20(多分)

- ページ代わりになるpages配下のファイルがCSS適用されたコンポーネントを読み込んでいる
pages/posts/first-post.js　→　components/layout.js　→　layout.module.css

## 9/21

- _app.jsってファイル名とかAppってクラス名は決まってる？あとglobal.cssって名前も
  - 決まっていそうな感じはする
- git configの設定をlocalでやってたらしく、こっちのリポジトリ更新しても草が生えてなかった……

## 9/22

- CSSももうちょい調べておいたほうが良いかも。0 1remとかようわからん
- 一旦ファイル構成を整理
- color:inheritって何
- homeはbool変数でindex.jsかどうかをあらわしてる
- からっぽのタグは後で埋める用？なんか気持ち悪い
- javascript?の&&演算子って動き違う？ !home && ()って書いてるところが変

## 9/23

### 座学回 page10 CSS

- SASSとかPortCSSとかようわからん
- CSSはやっぱり一回ちゃんと見ておく
- ドットインストールとかにあった気がするので見ておく　progateでも

### Next.jsのしくみ（飛ばしてたところ)

- コンパイル、ミニファイ、バンドル、コード分割
- サーバーサイドレンダリングがSSRのはず
- 静的サイト生成はStatic Site Generation?あってた
- 普通のブラウザ側レンダリングがClient Side Rendering
- jsxとか半HTMLみたいな状態で書くから、ちゃんとしたHTMLに誰かが出力しないといけない
  - それをサーバー側でやるかクライアント側でやるか、リアルタイムにやるか事前にやっとくかみたいな話
- ハイドレーションの意味合いがweb独自用語っぽくて理解しづらかった
  - 静的なものを動的にすることを言うらしい　水を与える？の比喩的な？
- Edgeがはしっこ　って訳されてるのがかわいかった
  - CDNと同じ意味だと思ってたら違うっぽい。CDNはエッジに含まれる。

### プリレンダリング

- 動的なページじゃないなら、SSR(多分)をつかってjsを無効化しても動くという話
- Reactは標準だとSSRじゃないからjsなくて動かないようになる
- 一個前にやった（実際はもっと前にやるべきだった）基本のまとめと似たような話が続く

## 9/25　事前レンダリングとデータ取得 page4

- だいたい同じ話？開発環境(npm run dev)だとSSGでも毎回生成するのは覚えておいたほうがよさそう
- SSGとSSRを好きなページで切り替えて使用できるので、場面によって使い分けをする

## 9/26

- ブログのエントリを外部から持ってくる
- gray matterとかいうのを入れる。markdownのメタデータを見て管理できるらしい
- libディレクトリを作って（特に名前が決まってるわけではないらしい）、gray-matterを使うメソッドを用意した
- ファイルシステム読み取りやパス指定なんかもやっていて、基本的な動きっぽいので覚えておく
- matterにかけると、メタデータの属性（dateとか)を簡単に取れるようになる　ぽい
- ...matterResult.data　の頭の３つの.は何を意味しているのか、意味は無いのか……

### VSCの文字がにじむ件

- ウィンドウを非アクティブにしたりすると、めちゃくちゃ文字がにじむ
- ショートカットのリンク先にオプションで --disable-gpu を足したところよくなった
- そもそもフォントもなんかイマイチ　やけに細い　長時間やると目をやられそう

## 9/27　事前レンダリングとデータ取得 page7

- 別ディレクトリの.mdファイルを読み込む
  - 前回作ったgray-matterかけたデータをindex.jsで受け取る
- importでlibに用意したメソッドを持ってくる
- もってきたpropをmapでぶん回してid,date,titleを取ってきて表示
  - ３つの要素が取れてくるのはgray-matterがいい感じにしてくれてるから？ここブラックボックスなのでよくわかってない
- デバッグで吐き出したりすればよりわかりそう

## 9/28 事前レンダリングとデータ取得 page8

- 0時回ってしまったので、コミット時にオプションを使ってみる　忘れないように
  - git commit -am "page8 座学" --date="Sep 28 23:59:59 2022 +0900"
  - こんな感じで過去に草が生える　0時過ぎたらこれ使うしかない
- 座学回だったので特に作業はなし
- SSGの別パターン、サーバ内ファイルじゃなくデータベースや外部へのfetchでも基本は同じということ
- 向こうのリポジトリ側のREADMEにずっと描いてたので自分とこのに移植した。
- もうちょっとで最初のアプリチュートリアルが終わりそう。
- 終わったらDocker立てるところをするか、CSSをやるか、なんか作るか……

## 9/29 事前レンダリングとデータ取得 page9

- 外部ファイル取ってくるのをSSRでやる方法
- getStaticPropsをgetServerSidePropsに変えるだけでいいらしい。便利。
- 今の仕組みだとブログ記事が増えれば増えるほどビルドが重くなっていくはず
- とはいえmarkdownをいい感じに置いておく場所もないので今はSSGのまま
- CSRをやるときはいい感じのライブラリ用意してあるから使ってねとのこと
  - SWR　団体っぽい名前
- 関係ないけどmarkdownのlintが優秀でありがたい
