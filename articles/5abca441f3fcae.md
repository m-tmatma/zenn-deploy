---
title: "GitLens で File history View でのファイル名変更の追跡有効化"
emoji: "👻"
type: "tech"
topics:
  - "vscode"
  - "gitlens"
published: true
published_at: "2022-08-20 10:01"
---

# GitLens で File history View でのファイル名変更の追跡有効化

`gitlens.advanced.fileHistoryFollowsRenames` を有効にする

1. VSCode で Control + , を押す
2. `fileHistoryFollowsRenames` を入力する
3. `Specifies whether file histories will follow renames - will affect how merge commits are show in histories` にチェックを入れる

https://vjeko.com/2020/11/24/understanding-renaming-moving-files-with-git/
https://github.com/gitkraken/vscode-gitlens/issues/1156
https://newreleases.io/project/github/gitkraken/vscode-gitlens/release/v8.0.0
