---
title: 'Windows の PlantUML で GraphViz インストールが不要になってた'
emoji: '🏭'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['plantuml']
published: true
---

## 概要

PlantUML の設定を見直していたら

> **Installation under Windows**
> Starting from 1.2020.21
> If you use a recent version (that is at least version 1.2020.21), you don't need to manually install GraphViz anymore !

ということで、GraphViz インストールが不要になってました。
詳細は以下のリンクに書いてあります（上の引用もリンクからです）。

https://plantuml.com/ja/graphviz-dot

## 設定

1. https://github.com/plantuml/graphviz-distributions の zip をダウンロード
2. 解凍してフォルダを適当なところに設置
3. 環境変数を設定
   1. 変数: GRAPHVIZ_DOT
   2. 値: {解凍したフォルダへのパス}\dot.exe

2020/12/27 時点での情報なので、設定するときはリンク先も参照してください
