This is a starter template for [Learn Next.js](https://nextjs.org/learn).

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