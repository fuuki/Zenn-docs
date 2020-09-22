---
title: "アナリシスパターン：オブジェクトへの参照"
emoji: "🖇"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["モデリング", "型", "アナリシスパターン"]
published: true
---

## 概要

アナリシスパターン[^ref-ap]第5章のオブジェクトへの参照について、ベン図をざーっと書き下してみた。

## 識別体系

- 複雑なシステムでは1つのオブジェクトに複数の識別子がつく

![image][img1]

## オブジェクトの併合

- 同一だと識別されるべきオブジェクトが複数作成されてしまうことがある
- 例
  - 病院に患者が来て治療を受けた
  - 数日後、別の部局の外来患者でもあることが判明した
  - 病院のシステムには、その患者の別の記録が作成されてしまった
- 概念的に2つのオブジェクトを結びつける仕掛けが必要
- 方針
  - 複写置換
  - 乗っ取り
  - 本質／外見

### 複写置換

- あるオブジェクトのすべての属性をもう一方のオブジェクトに複写し、元のオブジェクトを削除する方法
- 削除したオブジェクトへの識別子を残った方のオブジェクトへ対応付ける
  - 不変性ルールが破られる
  - 別名識別子（識別子に対しUNIQUEをかけない）が許されればOK
- 削除されたオブジェクトへの参照すべてを把握しなければならないので危険
- ベン図で書けない（概念化できてない）ので、あまりやらないほうがよさそう

![image][img2]

### 乗っ取り（supersede）

- 一方のオブジェクトを乗っ取られたオブジェクトとして型づけする
  - 現行オブジェクトへリンク
  - すべての作業は現行オブジェクトが行う
  - 乗っ取られたオブジェクトは履歴のためだけに残る
- 乗っ取られたオブジェクトのデータは現行オブジェクトへ複写、もしくは作業時に常にチェックされる

![image][img3]

### 本質／外見

- オブジェクトを互いにリンクするためだけの別のオブジェクトをおく
- オブジェクトを再分割する可能性がある場合に適している

![image][img4]

## オブジェクトの等価性

- オブジェクトの等価性は視点によって異なることがある
- 例
  - 多くの意思はG型肝炎とGBC型肝炎を同じ病気と考えるが普遍的ではない
  - 医師がG型肝炎の患者リストを要求するとき、その医師が等価性に合意していればGBC型肝炎の患者も答えとして返す

![image][img5]

[^ref-ap]: Martin Fowler (2002) アナリシスパターン: 再利用可能なオブジェクトモデル, ピアソンエデュケーション

::: message
この記事は 2020年5月31日に Qrunch にて公開していた記事を移管したものです
:::

[img1]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-referring-objects/1.png
[img2]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-referring-objects/2.png
[img3]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-referring-objects/3.png
[img4]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-referring-objects/4.png
[img5]: https://raw.githubusercontent.com/fuuki/Images/master/20200531-analysis-pattern-referring-objects/5.png
