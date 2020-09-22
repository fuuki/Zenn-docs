---
title: "アナリシスパターン：観測と測定"
emoji: "🔭"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["モデリング", "型", "アナリシスパターン"]
published: true
---

## 概要

アナリシスパターン[^ref-ap]第3章の観測と測定について、ベン図をざーっと書き下してみた。

## 量

- システムで扱う数字は、基本的に「数値」と「単位」からなる「量」であることが多い

![image][img1]

- 換算率をモデル化することができる
- 時間をもたせることでレートも表現できる

![image][img2]

- 原子単位・複合単位として、単位の関係を表すことも可能
- 例：力=[質量・長さ・時間$^{-2}$]

![image][img3]

## 観測

- 知識・操作レベルを使うと、↓のような観測のモデルができる

![image][img4]

- 間違えた観測は削除せず、記録しておくべきである
- 否認する観測（根拠となる観測）にリンクさせる

![image][img5]

## 双時間記録

- ほとんどの事象には、発生時間と記録時間という別々の時間がある

![image][img6]

[^ref-ap]: Martin Fowler (2002) アナリシスパターン: 再利用可能なオブジェクトモデル, ピアソンエデュケーション

::: message
この記事は 2020年5月31日に Qrunch にて公開していた記事を移管したものです
:::

[img1]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-observations/1.png
[img2]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-observations/2.png
[img3]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-observations/3.png
[img4]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-observations/4.png
[img5]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-observations/5.png
[img6]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-observations/6.png
