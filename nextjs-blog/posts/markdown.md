---
title: 'markdownについてまとめ'
date: '2022-10-04'
---
## 目次

[Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one#features)の拡張でサクっと入れられる  
ctrl + shift + pから、create table of contentsを呼び出す

- [目次](#目次)
- [改行、段落](#改行段落)
- [見出し2](#見出し2)
	- [見出し3](#見出し3)
		- [見出し4](#見出し4)
- [リスト](#リスト)
- [区切り線](#区切り線)
- [強調](#強調)
- [コードブロック](#コードブロック)
- [リンク](#リンク)
- [画像](#画像)
- [引用](#引用)
- [チェックボックス](#チェックボックス)
- [テーブル(表)](#テーブル表)

## 改行、段落

markdownの書き方とか、表示の確認用
改行一つだけだと同じ文章扱いになる

一行離すと別段落で改行される

文末にスペースを2つつけると  
同段落の改行になるけど見えないので扱いにくい

HTMLの改行タグはそのままだと使えなかった  
shift+enterでスペース２個と改行いれるようにして対応  
必要ならスペースを可視化する設定にすればなんとかなる

[shift+enterで改行が入るきようにキーボードショートカットを変更](https://zenn.dev/anzu_natsukawa/articles/c345fd08a400d9)

---
<!-- omit in TOC -->
# 見出し1

setting.jsonにmarkdonwlintの項目追加、MD025をfalseで黙らせた  
どうもこいつがへんなとこにあると目次の自動システムも調子が悪いっぽい  
目次にいれたくない見出しは`<!-- omit in TOC -->`をひとつ上の行に入れるとスキップできる

---

## 見出し2

普段遣いの見出し

---

### 見出し3

---

#### 見出し4

---

## リスト

- NJPW
  - strong
  - tamashii
  
1. AEW
   1. dynamite
   2. rampage

---

## 区切り線

上の文章

---

下の文章

---

## 強調

この文章で**特に大事なのはこの部分**です。  
文字を*斜体*にもできるっす。  
vscodeにMarkdown All In One の拡張を入れてると、ctrl+bで太字にできた

---

## コードブロック

```php
/**
 * バッククォートの後に言語名を書くのがlintのしきたり
 * ```php
 * echo("test");
 * ```
 **/
echo("test");
```

文章の中にコードを挟むにはバッククォート1つで囲む。

例：とりあえず`isset()`使っとけばなんとかなるんや

---

## リンク

[ローカルホストへのリンク](https://localhost:3000)  
[production環境へのリンク](https://nextjsblog-karamiso14.vercel.app/)

こんな感じで書く。

`[リンク名称](https://localhost:3000)`

こんな感じでも書けた  
本文中に[URL][1]が突っ込まれると邪魔なときに良いかもしれない

`[リンク名称][1]`  
`[1]:https://nextjsblog-karamiso14.vercel.app/`

[1]:https://nextjsblog-karamiso14.vercel.app/

---

## 画像

`![ALTテキスト](../public/images/profile.jpg "画像タイトル")`

![サンプルのプロフ画像](../public/images/profile.jpg "プロフィール画像")

previewでは出てたけど、アプリだと出てなかった。  
next.jsで使うにはImageタグとかに変換してやる必要がありそう。

## 引用

>引用文書
>>さらに引用する
>>>とどめの引用  

引用は改行なしでも特にlintチェック入らないみたい

## チェックボックス

- [ ] 未チェック
- [x] チェック済み

```markdown
- [ ] 未チェック
- [x] チェック済み
```

なかなか使い所難しそう。  
カーソル合わせてあれば、alt+cでチェックのオンオフができるっぽい(Markdown All In One)

## テーブル(表)

| ID  | TEXT           | NUMBER |
| --- | :------------- | -----: |
| 1   | オデュッセウス |   0277 |
| 2   | キルケー       |   0192 |

セルの中で左寄せにしたり右寄せにしたりもできるけど  
純粋に手が疲れるので入力補助が必須だと思った

```markdown
|ID|TEXT|NUMBER|
|--|:--|--:|
|1|オデュッセウス|0277|
|2|キルケー|0192|
```

vscodeならこれをalt+shift+F(または右クリメニューから)で整形できて、  
こうなる↓↓

```markdown
| ID  | TEXT           | NUMBER |
| --- | :------------- | -----: |
| 1   | オデュッセウス |   0277 |
| 2   | キルケー       |   0192 |
```
