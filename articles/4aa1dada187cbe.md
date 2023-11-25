---
title: "USB GPS レシーバー GR-7BN"
emoji: "📌"
type: "tech"
topics:
  - "gps"
  - "gr7bn"
published: true
published_at: "2023-03-26 17:03"
---

USB GPS レシーバー GR-7BN

https://www.amazon.co.jp/dp/B082Y1GLQK
を購入して使ってみた。

パッケージの中に `USB GPSレシーバー GR-7BN 【クイックガイド】` という紙が入っている。

https://www.u-blox.com/en/product/u-center

説明書にこちらをクリックと説明があるのでダウンロードする。(`u-centersetup_v22.07.zip`)

![](https://storage.googleapis.com/zenn-user-upload/d28349865ba9-20230326.png)

`u-centersetup_v22.07.zip` を解凍して、`u-center_v22.07.exe` をダブルクリックして
インストールする。

インストールすると `「MSVCR120.dll が見つからないため、コードの実行を続行できません。プログラムを再インストールすると、この問題が解決する可能性があります。」` と表示される。

1. これは Visual Studio 2013 の Visual C++ 再頒布可能パッケージがインストールされていないため
2. https://www.microsoft.com/ja-jp/download/details.aspx?id=40784 からダウンロードをクリックして、 `vcredist_x86.exe` および `vcredist_x64.exe` をダウンロードする。
3. それぞれをインストールする。(おそらく `vcredist_x86.exe` だけでいいと思われるが、 `vcredist_x64.exe` も念のためインストールした)

`u-center_v22.07` のアプリを起動して、以下のように COM ポートの設定を行う。

![](https://storage.googleapis.com/zenn-user-upload/850623813f44-20230326.png)

以下のように COM の転送レートを設定する。(デフォルトで 9600 になっていたので設定の変更は不要かも)

![](https://storage.googleapis.com/zenn-user-upload/f56a9340fd0c-20230326.png)

数分放置していると、GPS の電波を受信して位置が表示される。

Windowsキーを押して `位置` を入力すると `位置情報のプライバシー設定` が選択肢に出てくるので選ぶ。位置情報サービスを On にするとアプリから利用できるようになる。

この状態でマップアプリを起動すると位置情報を使うか確認ダイアログが出るので、OK を選ぶと
現在の位置がマップ上で確認できる。

![](https://storage.googleapis.com/zenn-user-upload/408dd0969f70-20230326.png)




