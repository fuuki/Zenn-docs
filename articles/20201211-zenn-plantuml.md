---
title: 'Zenn で plantuml を使うときのいい方法'
emoji: '💪'
type: 'idea' # tech: 技術記事 / idea: アイデア
topics: ['zenn', 'plantuml']
published: true
---

## 概要

VSCode では Markdown Preview Enhanced を利用することでコードの plantuml をプレビュー表示できるのですが、現状 Zenn にはそのような機能は有りません。そこでいろいろ試行錯誤した結果行き着いた方法を紹介します。

![image][img1]

## 条件

- VSCode を使用している
  - Markdown Preview Enhanced を利用し、plantuml のプレビューを見ながら書いている

## 書き方

1. 下のコードのように書きます
2. plantuml コード部分を描画した画像をどこかにアップロードし、URL を該当箇所に記載します

````
![image][img1]

:::details plantuml
```plantuml

@startuml
{{ 何かしらのUML }}
@enduml

```
:::

[img1]: {{ plantuml で描画したイメージのURL }}
````

この書き方で書いた記事の例が [こちら](https://zenn.dev/fuuki/articles/20201206-movie-ticket-modeling) です。
（また、トップの画像は、上記の記事の編集画面です。）

## この書き方の良いところ

Markdown Preview Enhanced で `:::details plantuml` ...... `:::` という記法が無視されるので、VSCode でプレビューを開くと plantuml のコードが描画された状態で確認できます。
画像アップロード後は同じ画像が 2 枚連続で表示されることにはなりますが、差異があるかの確認がしやすいです。

また、閲覧する人も plantuml のコードが確認できるので、参考にしやすい記事になると思います。

[img1]: https://raw.githubusercontent.com/fuuki/Images/master/20201211-zenn-plantuml/1.png
