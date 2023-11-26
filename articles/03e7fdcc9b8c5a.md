---
title: "Linux でユーザーを追加する"
emoji: "🙆"
type: "tech"
topics:
  - "linux"
  - "sudo"
published: true
published_at: "2021-12-19 08:50"
---

# Linux でユーザーを追加する

## 手順 (ユーザー追加)

user1 というユーザーを追加する場合の例。`user1` を適宜自分の追加したいユーザーに読み替えてください。

```
sudo useradd -s /bin/bash -m user1
sudo passwd user1
```


## 手順 (ユーザーに sudo を許可)

user1 のユーザーで sudo を許可する

```
sudo usermod -aG sudo user1
```


## 解説

### ユーザーの追加

`useradd` コマンドでユーザーを追加します。
`-m` を指定することによりユーザーのホームディレクトリを作成します。
`-s` を指定することによりユーザーのログインシェルを設定します。ここでは bash を指定します。

`passwd` コマンドにより、ユーザーのパスワードを設定します。


### ユーザーに sudo を許可

`sudo` グループに user1 を追加することにより指定したユーザーに sudo の操作を許可します。sudo を許可しない場合には不要な手順です。

### ユーザーの追加で `-s` を指定し忘れた時

ユーザーの追加で `-s` を指定し忘れた時 `chsh` で作成後にログインシェルを変更可能です。

```
$ chsh
パスワード:
user1 のログインシェルを変更中
新しい値を入力してください。標準設定値を使うならリターンを押してください
	ログインシェル [/bin/sh]: /bin/bash
```
### 参考サイト

https://atmarkit.itmedia.co.jp/ait/articles/1811/02/news035.html
https://qiita.com/orangain/items/056db6ffc16d765a8187
https://eng-entrance.com/linux-command-chsh


