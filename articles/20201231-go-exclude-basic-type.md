---
title: '構造体から基本型を排除する'
emoji: '🥊'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['go', 'モデリング']
published: true
---

## 概要

Go は概念表現がしやすい言語です。

## 基本型を使った構造体

```go
type User struct {
	Name  string
	Email string
}
```

概念モデルを意識し始めてから、上のようなコードを見ると **Name と Email は別概念のはずなのに、同じ型で気持ち悪い** と思うようになりました。
Go だと Defined Type を使うことで、構造体から基本型を排除できます。

## 基本型の排除

```go
type Name string
type Email string

type User struct {
	Name  Name
	Email Email
}
```

`Name` と `Email` の概念を切り分けることができました。
これだけだと外出しして別名を付けただけのようですが、Go は構造的に型がつけられるので、ここに追記していくことで概念表現を精緻化できます。

例えば、下のようにバリデーションチェックを実装できます。

```go
package main

import (
	"fmt"
	"reflect"
	"strings"
)

type Name string
type Email string

type User struct {
	Name  Name
	Email Email
}

// ValidationProp は検査対象プロパティ
type ValidationProp interface {
	IsValid() bool
}

// IsValid は Email が定義域内かを検証する
func (src Email) IsValid() bool {
	// @ を含まないものは不可
	return strings.Contains(string(src), "@")
}

// Check は構造体内のプロパティが定義域内かを検証する
func Check(value interface{}) bool {
	v := reflect.ValueOf(value)
	t := v.Type()
	// 構造体のプロパティを 1つずつ確認
	for i := 0; i < t.NumField(); i++ {
		fv := v.Field(i)
		// ValidationProp 型の場合は検証する
		if prop, ok := fv.Interface().(ValidationProp); ok {
			if !prop.IsValid() {
				return false
			}
		}
	}
	return true
}

func main() {
	alice := User{Name: "Alice", Email: "alice@email.com"}
	fmt.Printf("User: %+v, Check: %t\n", alice, Check(alice))
	// => User: {Name:Alice Email:alice@email.com}, Check: true
	bob := User{Name: "Bob", Email: "bob email"}
	fmt.Printf("User: %+v, Check: %t\n", bob, Check(bob))
	// => User: {Name:Bob Email:bob email}, Check: false
}
```

https://play.golang.org/p/1NgYus1jBpT

上のコードでは

- `IsValid()` メソッドが定義されている型を `ValidationProp` と呼ぶ
- `Check()` により、構造体に含まれる `ValidationProp` が適切かを判定する
- `Email` 型に `IsValid()` を定義することで `Check()` の検査対象に含める

を実現しています。

したがって、例えば「`Name` は空であることを許可しない」という条件を追加する場合は

```go
func (src Name) IsValid() bool {
	return src != ""
}
```

を追加するだけで実現できます。

:::details 追加後コード全文

```go
package main

import (
	"fmt"
	"reflect"
	"strings"
)

type Name string
type Email string

type User struct {
	Name  Name
	Email Email
}

// ValidationProp は検査対象プロパティ
type ValidationProp interface {
	IsValid() bool
}

// IsValid は Email が定義域内かを検証する
func (src Email) IsValid() bool {
	// @ を含まないものは不可
	return strings.Contains(string(src), "@")
}

// IsValid は Name が定義域内かを検証する
func (src Name) IsValid() bool {
	return src != ""
}

// Check は構造体内のプロパティが定義域内かを検証する
func Check(value interface{}) bool {
	v := reflect.ValueOf(value)
	t := v.Type()
	// 構造体のプロパティを 1つずつ確認
	for i := 0; i < t.NumField(); i++ {
		fv := v.Field(i)
		// ValidationProp 型の場合は検証する
		if prop, ok := fv.Interface().(ValidationProp); ok {
			if !prop.IsValid() {
				return false
			}
		}
	}
	return true
}

func main() {
	alice := User{Name: "Alice", Email: "alice@email.com"}
	fmt.Printf("User: %+v, Check: %t\n", alice, Check(alice))
	// => User: {Name:Alice Email:alice@email.com}, Check: true
	bob := User{Name: "Bob", Email: "bob email"}
	fmt.Printf("User: %+v, Check: %t\n", bob, Check(bob))
	// => User: {Name:Bob Email:bob email}, Check: false
	anonymous := User{Name: "", Email: "who@email.com"}
	fmt.Printf("User: %+v, Check: %t\n", anonymous , Check(anonymous ))
	// => User: {Name: Email:who@email.com}, Check: false
}

```

:::

https://play.golang.org/p/G1KOORTtpZF

このように Go は概念を切り分けたり、後から追加したりが気楽にできるので、概念モデルを活かしやすいのではと思います。

## おまけ

埋め込みを使うことで、（やろうと思えば）こんな定義もできます。

```go
type Name string
type Email string

type User struct {
	Name
	Email
}
```

:::details 変更後コード全文

```go
package main

import (
	"fmt"
	"reflect"
	"strings"
)

type Name string
type Email string

type User struct {
	Name
	Email
}

// ValidationProp は検査対象プロパティ
type ValidationProp interface {
	IsValid() bool
}

// IsValid は Email が定義域内かを検証する
func (src Email) IsValid() bool {
	// @ を含まないものは不可
	return strings.Contains(string(src), "@")
}

// Check は構造体内のプロパティが定義域内かを検証する
func Check(value interface{}) bool {
	v := reflect.ValueOf(value)
	t := v.Type()
	// 構造体のプロパティを 1つずつ確認
	for i := 0; i < t.NumField(); i++ {
		fv := v.Field(i)
		// ValidationProp 型の場合は検証する
		if prop, ok := fv.Interface().(ValidationProp); ok {
			if !prop.IsValid() {
				return false
			}
		}
	}
	return true
}

func main() {
	alice := User{Name: "Alice", Email: "alice@email.com"}
	fmt.Printf("User: %+v, Check: %t\n", alice, Check(alice))
	// => User: {Name:Alice Email:alice@email.com}, Check: true
	bob := User{Name: "Bob", Email: "bob email"}
	fmt.Printf("User: %+v, Check: %t\n", bob, Check(bob))
	// => User: {Name:Bob Email:bob email}, Check: false
}
```

:::

https://play.golang.org/p/fvOzcOXodzl

見た目はとてもきれいです。
