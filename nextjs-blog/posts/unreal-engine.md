---
title: "Unreal Engine"
date: "2022-10-08"
---

## 参考リンク

- [公式チュートリアル](https://www.unrealengine.com/ja/learn)
- [いい感じにまとめてくれてるサイト](https://ue5study.com/unrealengine-basic-operation/)
- [UE5(UE4)とGitHubでバージョン管理する方法｜UnrealEngine5(UE5)の教科書[旧]](https://zenn.dev/daichi_gamedev/books/unreal-engine-5/viewer/versioncontroll-git)

## とりあえず真島先生の動画見た

- ブループリント
  - プレハブみたいなもの
- レベル
  - シーンっぽい
- アクタ
  - レベル上に存在するオブジェクト全般

ブループリントでステートマシンみたいなやつに条件設定してGUIでonCollisionEnterみたいなことしてた  
ライトにつくるならこれでもよい？実際の作業手法としてメジャーなのか？  
unityだとコンポーネントにスクリプトをはっつけまくって動かしてた感じ  
実際のところどうなのかいまいちわからんが基本は同じっぽい？

### その他メモ

- udemyの有名な有料チュートリアルが２万超えててビビるなど
- 毎月無料のアセットあるからそれは確保がおすすめ

## 「UE5でのアニメーショ音の流用方法について」を見た

[state of Unreal 技術講演に日本語ついたよのお知らせ](https://www.youtube.com/watch?v=nb8P_VmRKog) あとでみる

UE4の無料アセットをUE5に突っ込む時どうすればいいか、がわかるって感じの動画

### UE4では２つ方法があった

- 同じskeltonでのアニメーションリターゲット
  - 猫でもわかるUE4のAnimation Blueprintの運用について に詳細あり
- 異なるskeltonでのアニメーションリターゲット
  - 流用はできるが共有はできない 別アセットになるので数が増えてしまう

### UE5では

- Compatible Skeltonで大体解決
  - スケルトンにストアから持ってきたパックのスケルトンをコンパチ設定すると使える
  - 一回開き直す必要がこのときはあった今は知らん
  - retargeting optionをいじって骨優先的な対応するとうまくいったりする
  - ボーン名一致、構成が類似が必要。二足歩行同士とか
- IK Retargeter で大体解決

### 気になったワード

- skeltal mesh  

## とりあえず触った感じ

さわってからかく
