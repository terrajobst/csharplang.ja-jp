---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108371"
---

# <a name="target-typed-new-expressions"></a>ターゲット型の `new` 式

* [x] が提案されています
* [x][プロトタイプ](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* [] の実装
* [] 仕様

## <a name="summary"></a>要約
[summary]: #summary

型がわかっている場合は、コンストラクターの型指定を必要としません。 

## <a name="motivation"></a>目的
[motivation]: #motivation

型を複製せずにフィールドの初期化を許可します。
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

使用法から推論できる場合は、型の省略を許可します。
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

型のスペルを修正せずにオブジェクトをインスタンス化します。
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

*Object_creation_expression*構文は、かっこが存在する場合に*型*を省略可能にするように変更されます。 これは、 *anonymous_object_creation_expression*のあいまいさを解決するために必要です。
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

ターゲット型の `new` は、任意の型に変換できます。 そのため、オーバーロードの解決には関与しません。 これは主に、予期しない重大な変更を回避するためのものです。

引数リストと初期化子式は、型が決定された後にバインドされます。

式の型は、次のいずれかである必要があるターゲット型から推論されます。

- **任意の構造体型**(タプル型を含む)
- **任意の参照型**(デリゲート型を含む)
- コンストラクターまたは `struct` 制約を持つ**任意の型パラメーター**

次の例外があります。

- **列挙型:** すべての列挙型に定数ゼロが含まれていないため、明示的な enum メンバーを使用することをお勧めします。
- **インターフェイス型:** これはニッチ機能であり、型を明示的に言及することをお勧めします。
- **配列型:** 配列の長さを指定するには、特別な構文が必要です。
- **動的:** `new dynamic()`は許可されていないため、`dynamic` を対象の型として `new()` することはできません。

*Object_creation_expression*で許可されていないその他のすべての型も、ポインター型など、除外されます。

対象の型が null 許容の値型である場合、対象の型指定された `new` は、null 許容型ではなく、基になる型に変換されます。

> **懸案事項を開く:** デリゲートとタプルをターゲットタイプとして許可する必要がありますか。

上記の規則には、デリゲート (参照型) と組 (構造体型) が含まれます。 どちらの型も構築可能ですが、型が inferable の場合は、匿名関数またはタプルリテラルを使用できます。
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
