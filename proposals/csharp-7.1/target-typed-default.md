---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "79483914"
---
# <a name="target-typed-default-literal"></a>ターゲット型の "default" リテラル

* [x] が提案されています
* [x] プロトタイプ
* [x] 実装
* [] 仕様

## <a name="summary"></a>まとめ
[summary]: #summary

ターゲット型の `default` 機能は、`default(T)` 演算子の短い形式のバリエーションであり、型を省略できます。 その型は、代わりにターゲット入力によって推論されます。 それ以外の場合は、`default(T)`のように動作します。

## <a name="motivation"></a>目的
[motivation]: #motivation

主な動機は、冗長な情報を入力しないようにすることです。

たとえば、`void Method(ImmutableArray<SomeType> array)`を呼び出すときに、*既定*のリテラルを使用すると `M(default(ImmutableArray<SomeType>))`の代わりに `M(default)` できます。

これは、次のようなさまざまなシナリオで適用されます。

- ローカルの宣言 (`ImmutableArray<SomeType> x = default;`)
- 三項演算 (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)
- メソッドとラムダを返す (`return default;`)
- 省略可能なパラメーターの既定値の宣言 (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)
- 配列作成式に既定値を含める (`var x = new[] { default, ImmutableArray.Create(y) };`)


## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

新しい式が導入されました。*既定*のリテラルです。 この分類の式は、*既定のリテラル変換*によって、任意の型に暗黙的に変換できます。 

*既定*のリテラルの型の推論は、 *null*リテラルの場合と同じように動作します。ただし、参照型だけでなく、任意の型が許可される点が異なります。

この変換により、推定される型の既定値が生成されます。

*既定*のリテラルは、推論された型に応じて定数値を持つことができます。 `const int x = default;` は有効ですが、`const int? y = default;` は有効ではありません。

*既定*のリテラルは、他のオペランドが型を持つ限り、等値演算子のオペランドにすることができます。 `default == x` と `x == default` は有効な式ですが、`default == default` は無効です。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

小さな欠点は、ほとんどのコンテキストで*既定*のリテラルを*null*リテラルの代わりに使用できることです。 2つの例外は `throw null;` と `null == null`であり、 *null*リテラルでは許可されますが、*既定*のリテラルは使用できません。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

考慮すべき選択肢がいくつかあります。

- 現状では、この機能は独自のメリットには合わないため、開発者は、明示的な型で既定の演算子を引き続き使用します。
- Null リテラルの拡張: これは `Nothing`を使用した VB のアプローチです。 `int x = null;`を許可することができます。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- [x]*は、is*演算子または*as*演算子のオペランドとして*既定*で許可されますか? 回答: `default is T`を許可しない、`x is default`を許可する、`default as RefType` を許可する (常に null 警告を含む)

## <a name="design-meetings"></a>会議のデザイン

- [LDM 3/7/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [LDM 3/28/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [LDM 5/31/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
