---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "79483890"
---
# <a name="null-coalescing-assignment"></a>null 合体演算子割り当て

* [x] が提案されています
* [] プロトタイプ: 開始されていません
* [] の実装: 開始されていません
* [] 仕様: 以下

## <a name="summary"></a>まとめ
[summary]: #summary

変数が null の場合に値が割り当てられる、一般的なコーディングパターンを簡略化します。

この提案の一環として、`??` の型の要件を緩和して、型が制約のない型パラメーターである式を左辺で使用できるようにします。

## <a name="motivation"></a>目的
[motivation]: #motivation

フォームのコードを確認するのが一般的です。

```csharp
if (variable == null)
{
    variable = expression;
}
```

この提案は、この関数を実行する言語に、オーバーロードできない二項演算子を追加します。

この機能には、少なくとも8つの個別のコミュニティ要求があります。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

新しいフォームの代入演算子を追加します。

``` antlr
assignment_operator
    : '??='
    ;
```

これは、[複合代入演算子の既存のセマンティック規則](../../spec/expressions.md#compound-assignment)に従います。ただし、左辺が null 以外の場合は代入を解除します。 この機能の規則は次のとおりです。

`a ??= b`、`A` は `a`の種類、`B` は `b`の種類、`A0` が null 許容値型である場合は `A` の基になる型です。`A`

1. `A` が存在しないか、null 非許容の値型である場合、コンパイル時エラーが発生します。
2. `B` が `A` または `A0` に暗黙的に変換できない場合 (`A0` 存在する場合)、コンパイル時エラーが発生します。
3. `A0` が存在し、`B` が暗黙的に `A0`に変換可能で、`B` が動的でない場合、`a ??= b` の型は `A0`になります。 `a ??= b` は、次のように実行時に評価されます。
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   ただし `a` が評価されるのは1回だけです。
4. それ以外の場合、`a ??= b` の種類は `A`です。 `a ??= b` は `a ?? (a = b)`として実行時に評価されますが、`a` は1回だけ評価される点が異なります。


`??`の型要件の緩和については、現在のところ、`a ?? b`の場合、`A` は `a`の種類であるという仕様を更新します。

> 1. が存在し、null 許容型または参照型でない場合は、コンパイル時エラーが発生します。

次の要件を緩和します。

1. が存在し、null 非許容の値型である場合、コンパイル時エラーが発生します。

これにより、制約のない型パラメーター T が存在し、が null 許容型ではなく、参照型ではないため、null 合体演算子を制約のない型パラメーターで使用できます。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

他の言語機能と同様に、この機能の恩恵を受けるプログラムのC#本文に対して、さらに複雑な言語を端的にする必要があるかどうかを質問する必要があります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

プログラマは、手動で `(x = x ?? y)`、`if (x == null) x = y;`、または `x ?? (x = y)` を作成できます。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- [] には LDM レビューが必要です
- [] `&&=` および `||=` の演算子もサポートする必要がありますか。

## <a name="design-meetings"></a>会議のデザイン

[なし] :
