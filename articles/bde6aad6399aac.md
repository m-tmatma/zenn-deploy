---
title: "getopt によるコマンドライン引数解析"
emoji: "🗂"
type: "tech"
topics:
  - "getopt"
published: true
published_at: "2022-05-05 17:48"
---

# getopt によるコマンドライン引数解析

## getopt のサンプルプログラム

bash 用のサンプルは以下

https://github.com/util-linux/util-linux/blob/master/misc-utils/getopt-example.bash

Ubuntu 22.04 では `/usr/share/doc/util-linux/examples/getopt-example.bash` にインストールされている

## getopt の使い方のポイント

* bash で利用可能 ([tcsh用のサンプル](https://github.com/util-linux/util-linux/blob/master/misc-utils/getopt-example.tcsh)もあるので tcshも可能な模様)
* `-o` で短いオプションを指定する
* `--long` または `-l` で長いオプションを指定する。各項目はカンマで区切る
* 短いオプション、長いオプションともに引数をとるオプションの場合は `:` を指定する
* オプション引数が省略可能な場合は `::` を指定する。
* while ループで順番に引数を処理する。
* 処理が終わった引数に関しては shift でずらす。
* 現在処理中のオプションに関しては `$1` で参照する。
* オプションの引数は `$2` で参照する。
* オプション引数が省略可能な場合は `$2` が空かどうか確認する。
* 引数ありのオプションの場合 `shift 2` でずらす

## getopt のヘルプ

http://manpages.ubuntu.com/manpages/bionic/ja/man1/getopt.1.html