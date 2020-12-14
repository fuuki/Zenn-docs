---
title: '無料漫画アプリの概念モデリング_閲覧チケット編'
emoji: '📚'
type: 'idea' # tech: 技術記事 / idea: アイデア
topics: ['モデリング', '型', '概念']
published: true
---

## 概要

概念モデリングの練習として、無料漫画アプリの概念モデルを作成してみます。思ってたより大きくなりそうだったので、複数記事に分けてやっていこうと思います。

（注：私がアプリの開発に関わっているわけではないです）

## 閲覧チケットの概要

- コインを使うことで、漫画を読むことができる
  - コインには 2 種類あり、毎日 9 時に補充されるもの（コイン）と、指定された行動を取ることで得られるもの（SP コイン）がある
- チケットを購入することでも、漫画を読むことができる

ざっくりとこんな感じです。細かい仕様はモデリングしながら補足していきます。

## コイン／チケット関連のモデリング

コイン／チケットが複数種あるので、とりあえず記述します。

![image](<https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/1.png> =300x)

:::details plantuml

```plantuml

@startuml

hide circle
hide methods
skinparam shadowing false

class 量{}

class スーパーコイン{}

class コイン{}
class SPコイン{}
class チケット{}

class ユーザー

量 "*"-"*" スーパーコイン

スーパーコイン <|-- コイン
スーパーコイン <|-- SPコイン
スーパーコイン <|-- チケット

コイン "1"--"*" ユーザー
SPコイン "1"--"*" ユーザー
チケット "1"--"*" ユーザー

@enduml

```

:::

このアプリにおいて、コインやチケットは漫画と交換する貨幣のような役割を持っています。この貨幣を概念化したかったのですが、名称が全く思いつかなかったので **スーパーコイン** という安直な名前をとりあえずつけました。

ベン図で書くと次のようなイメージです。

![image](<https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/2.png> =300x)

:::details plantuml

```plantuml

@startuml

skinparam shadowing false
left to right direction

storage ユーザー {
  rectangle Alice
  rectangle Bob
}

storage スーパーコイン {
  storage コイン {
    rectangle "量: 4" as l1
    rectangle "量: 3" as l2
  }
  storage SPコイン {
    rectangle "量: 50" as s1
    rectangle "量: 32" as s2
  }
  storage チケット {
    rectangle "量: 8" as t
  }
}

Alice -- l1
Alice -- s1
Alice -- t
Bob -- l2
Bob -- s2
Bob -- t

@enduml

```

:::

Alice と Bob はチケットのエンティティ 「量: 8」 を共有していますが、スーパーコインの概念は各コイン・チケットの量を表すのみの概念なので、共有しても問題有りません。これらは、Alice と Bob がチケットを 8 持っているという事実を表す役割のみを持ちます。

このあたりの概念は、DDD の値オブジェクトと同じような考え方だと思います。

### コイン／チケットを使用する

漫画を読む際にコイン／チケットを使用するのですが、1 つの作品を見るのに使用するコイン／チケットは必ず 1 種類で、1 度に 2 以上消費することもあります。

![image](<https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/3.png> =400x)

:::details plantuml

```plantuml

@startuml

hide circle
hide methods
skinparam shadowing false

class 量{}

class スーパーコイン{}

class コイン{}
class SPコイン{}
class チケット{}

class ユーザー

class 作品

class 引換{
  引換時点
}

量 "*"-"*" スーパーコイン

スーパーコイン <|-- コイン
スーパーコイン <|-- SPコイン
スーパーコイン <|-- チケット

コイン "1"--"*" ユーザー: < 保有
SPコイン "1"--"*" ユーザー: < 保有
チケット "1"--"*" ユーザー: < 保有

ユーザー "*"--"*" 作品
(ユーザー,作品)..引換
スーパーコイン "1"--"*" 引換


@enduml

```

:::

これをベン図で例示すると、次のようになります。
これは、「Alice がチケットを 2 ずつ消費（つまり、2x2=4 消費）して、火の鳥の 第 3 話、第 4 話 を引換した」ことを表します。

![image](<https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/4.png> =600x)

:::details plantuml

```plantuml

@startuml

skinparam shadowing false
left to right direction

storage ユーザー {
  rectangle Alice
}

storage スーパーコイン {
  storage チケット {
    rectangle "量: 2" as tkt
  }
}

storage 引換 {
  rectangle "引換時点: 2020/12/12 17:21" as buy1
  rectangle "引換時点: 2020/12/12 17:43" as buy2
}

storage 作品 {
  rectangle "火の鳥 第3話" as cmc1
  rectangle "火の鳥 第4話" as cmc2
}

Alice -- buy1
Alice -- buy2
buy1 -- tkt
buy2 -- tkt
cmc1 -- buy1
cmc2 -- buy2

@enduml

```

:::

### コイン／チケットを獲得する

それぞれの獲得方法は次のとおりです。

| 種類      | 獲得方法                                 |
| --------- | ---------------------------------------- |
| コイン    | 毎日 9 時に自動的に 4 になる             |
| SP コイン | 広告動画視聴などの特定の行動によって獲得 |
| チケット  | 購入することで獲得                       |

まずは、各獲得方法を愚直に記述してみます。

![image][img5]

:::details plantuml

```plantuml

@startuml

hide circle
hide methods
skinparam shadowing false

class コイン{
  量
}
class SPコイン{
  量
}
class チケット{
  量
}

class 自動獲得ルール{
  時刻
}
class チケット販売{
  金額
}
class 広告動画視聴{
  動画
}
class キャンペーン応募{
  キャンペーンリンク
}
class アプリダウンロード{
  ダウンロードリンク
}

コイン "1"--"*" 自動獲得ルール
チケット "1"--"*" チケット販売
SPコイン "1"--"*" 広告動画視聴
SPコイン "1"--"*" キャンペーン応募
SPコイン "1"--"*" アプリダウンロード

@enduml

```

:::

それぞれを詳しく見ていきます。

#### コイン

「毎日 9 時に自動的に 4 になる」ので、以下のようなベン図で表されるはずです。

![image](<https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/6.png> =300x)

:::details plantuml

```plantuml

@startuml

hide circle
hide methods
skinparam shadowing false

storage コイン {
  rectangle "量: 4" as lf
}

storage 自動獲得ルール {
  rectangle "時刻: 9:00" as at
}

lf - at

@enduml

```

:::

自動獲得ルールのエンティティは 1 つのみになりそうですが、システムで扱う概念を記述するのが目的なので、これも型として扱います。

#### SP コイン

こちらは獲得方法が複数あります。動画視聴やアプリダウンロードなど、それぞれ処理が異なることが想定されます。また、その処理のために使用する属性も異なるため、継承により実現する必要がありそうです。

![image](<https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/7.png> =400x)

:::details plantuml

```plantuml

@startuml

hide circle
hide methods
skinparam shadowing false

class SPコイン{
  量
}

class SPコイン獲得ルール{

}

class 広告動画視聴{
  動画
}
class キャンペーン応募{
  キャンペーンリンク
}
class アプリダウンロード{
  ダウンロードリンク
}

SPコイン "1"-"*" SPコイン獲得ルール
SPコイン獲得ルール <|-- 広告動画視聴
SPコイン獲得ルール <|-- キャンペーン応募
SPコイン獲得ルール <|-- アプリダウンロード

@enduml

```

:::

#### チケット

チケットは購入によって獲得するため、価格を設定します。

### 獲得履歴

#### SP コインの獲得履歴

ユーザーは、SP コインは 1 つの獲得ルールにつき 1 度ずつしか適用されないため、履歴を保つ必要があります。履歴を持たないもののみ **取得可能** として定義します。

![image](<https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/8.png> =500x)

:::details plantuml

```plantuml

@startuml

hide circle
hide methods
skinparam shadowing false

class ユーザー{
}

class SPコイン獲得ルール{
}

class SPコイン獲得履歴{
  取得時点
}

ユーザー "*"-"*" SPコイン獲得ルール
(ユーザー,SPコイン獲得ルール)..SPコイン獲得履歴
ユーザー "*"-"*" SPコイン獲得ルール: > /取得可能
note on link: [導出]SPコイン獲得履歴を持たない関連のみ

@enduml

```

:::

### チケットの獲得履歴

同様に、チケットも購入履歴を閲覧できるようにします。

![image](<https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/9.png> =300x)

:::details plantuml

```plantuml

@startuml

hide circle
hide methods
skinparam shadowing false

class ユーザー{
}

class チケット獲得ルール{
}

class チケット獲得履歴{
  取得時点
}

ユーザー "*"-"*" チケット獲得ルール
(ユーザー,チケット獲得ルール)..チケット獲得履歴

@enduml

```

:::

## つづく

もう少し汎化できる気もしたのですが、それぞれのコイン／チケットは明確にことなる獲得方法を持っているため、獲得についてはあまり汎化する必要はないかなと思いました。

次は漫画のジャンル分けなどをやってみたいと思います。

[img1]: https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/1.png
[img2]: https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/2.png
[img3]: https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/3.png
[img4]: https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/4.png
[img5]: https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/5.png
[img6]: https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/6.png
[img7]: https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/7.png
[img8]: https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/8.png
[img9]: https://raw.githubusercontent.com/fuuki/Images/master/20201211-manga-app-modeling/9.png
