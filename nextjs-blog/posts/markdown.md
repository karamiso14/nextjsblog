---
title: 'markdownについてまとめ'
date: '2022-10-04'
---

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

# 見出し1

lintによると文頭推奨のため警告がつく

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
