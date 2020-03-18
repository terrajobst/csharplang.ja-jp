---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483494"
---
# <a name="null-conditional-await"></a>null 条件 await

* [x] が提案されています
* [] プロトタイプ: なし
* [] の実装: なし
* [] 仕様: 開始しました。以下を参照してください。

## <a name="summary"></a>まとめ
[summary]: #summary

`await? e`形式の式をサポートします。これは、null 以外の場合に `e` を待機します。それ以外の場合は `null`になります。

## <a name="motivation"></a>目的
[motivation]: #motivation

これは一般的なコーディングパターンであり、この機能は、既存の null を反映する演算子と null 合体演算子によって優れた相乗効果が得られます。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

*Await_expression*の新しいフォームを追加します。

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

Null 条件 `await` 演算子は、そのオペランドが null 以外の場合にのみオペランドを待機します。 それ以外の場合は、演算子を適用した結果が null になります。

結果の型は、 [null 条件演算子のルール](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator)を使用して計算されます。

> **注:** `e` が `Task`型の場合、`await? e;` は `e` が `null`されている場合は何も行いません。 `e` ない場合は、`null`待機します。
>
> `e` が `Task<K>` 型の場合、`K` は値型であるため、`await? e` は `K?`型の値を生成します。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

他の言語機能と同様に、この機能の恩恵を受けるプログラムのC#本文に対して、さらに複雑な言語を端的にする必要があるかどうかを質問する必要があります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

いくつかの定型コードが必要ですが、この演算子の使用は、多くの場合、`(e == null) ? null : await e` や `if (e != null) await e`のようなステートメントに置き換えることができます。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- [] には LDM レビューが必要です

## <a name="design-meetings"></a>会議のデザイン

[なし] :
