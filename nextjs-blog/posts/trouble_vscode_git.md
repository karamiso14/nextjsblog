---
title: "vscodeからgithubにアクセスできなくなった件"
date: "2022-10-14"
---

WSLを更新してUbuntu20.04のversion2にした時？とりあえず昨日から、  
vscode内のターミナルからgithubへのアクセスがpermission deniedになっていた。  
ubuntuのターミナルからなら動いていて、ユーザが違うとかではなかった。

## 解決策

[俺はただwindows 10のvscodeからgit pushしたかっただけ - ajishixoの日記](https://ajishixo.hatenablog.com/entry/2019/03/23/133206)  

[PowershellのgitではpushできるのにVSCodeのgit(GUI)ではpushできない問題 | EMC2NARY](https://utautattaro.blog/post/84240ik3ui5m/)

どうもwindowsのgitが使うsshコマンドがよろしくないのになってしまったらしい。  
gitconfigに下記を追記した

```config
[core]
	sshCommand = /mnt/c/Windows/System32/OpenSSH/ssh.exe
```

これでvscodeのターミナルからでもssh鍵でアクセスできる。  
pushして即vscodeを閉じたので次の日になるまで気づかなかった。  
おかげで13日の草が生えてなかったので手動で生やした。  
14日はこの記事で生やす。

と思ったけどやっぱ動いてなかったわ  
sshkey作り直してgithubに置いたら動くようになった  
疲れた
configを削除すると動かなくなるから、一応やる意味はあったっぽい。
