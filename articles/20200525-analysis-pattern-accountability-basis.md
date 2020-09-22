---
title: "アナリシスパターン：責任関係（基礎）"
emoji: "🕴"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["モデリング", "型", "アナリシスパターン"]
published: true
---

## 概要

アナリシスパターン[^ref-ap]第2章より、責任関係の基本的な事項を書き出します。
難しい本ですが、ベン図に書き下すことで理解しようと試みます。

責任関係は人と人、人と組織などの関係性を汎化した概念です。
この記事では基本的な内容だけを抑えていきます。

## 責任関係とは

- 人や組織が他者に対して責任を負っていることを表す概念
- 例
  - 組織構造（部署AにはBさんがいる。また、部署AにはCチームとDチームがある）
  - 雇用（A社がBさんを雇う）
  - 売買関係（AさんはコンビニBで買い物をする。コンビニBは問屋Cから仕入れをする）
  - など

## パーティ

![image][img1]

- さまざまな行為が人や組織でやりとりされる
  - 例：手紙のやり取り、売買関係、雇用など
- 汎化して **パーティ** とする

## 責任関係

![image][img2]

- 様々な関係性を責任関係として示す
- 責任関係は多対多で作られ、 **委任者** と **責任者** に分けられる

## 知識レベルと操作レベル

- 対応付けによってサブタイプを規定するとき、それを **パワータイプ** と呼ぶ

### 型図

![image][img3]

- 型図の注釈は平たく言うと「知識レベルでリンクがないような責任者・委任者の関係性を操作レベルで持たせてはいけない」

### ベン図

![image][img4]

ベン図は次のような内容を表している

- John はマネージャーであり、1276型サービスチームの管理者である
- Alice はエンジニアであり、1276型サービスチームの専門家である
- チームの管理者はマネージャーに限られる
- チームの専門家はエンジニアに限られる

[^ref-ap]: Martin Fowler (2002) アナリシスパターン: 再利用可能なオブジェクトモデル, ピアソンエデュケーション

::: message
この記事は 2020年5月25日に Qrunch にて公開していた記事を移管したものです
:::

[img1]: https://raw.githubusercontent.com/fuuki/Images/master/20200525-analysis-pattern-accountability-basis/party-model.png
[img2]: https://raw.githubusercontent.com/fuuki/Images/master/20200525-analysis-pattern-accountability-basis/party-venn.png
[img3]: https://raw.githubusercontent.com/fuuki/Images/master/20200525-analysis-pattern-accountability-basis/accountability-type-level.png
[img4]: https://raw.githubusercontent.com/fuuki/Images/master/20200525-analysis-pattern-accountability-basis/accountability-venn-level.png
