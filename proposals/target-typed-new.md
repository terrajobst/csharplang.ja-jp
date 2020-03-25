---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217204"
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
```

### <a name="miscellaneous"></a>その他

`throw new()` は許可されていません。

ターゲット型の `new` は、バイナリ演算子では使用できません。

対象となる型がない場合は、単項演算子、`foreach`のコレクションです。 `using`の場合、`await` 式では、匿名型のプロパティ (`new { Prop = new() }`)、`lock` ステートメント、`sizeof`演算子のオペランドとして、LINQ クエリ内では、`fixed` 演算子の左オペランドとして、動的にディスパッチされる操作 (`new().field`) において、ステートメントを使用して、ステートメントに含まれます。,  ...`someDynamic.Method(new())``is``??`

また、`ref`として許可されていません。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

互換性に影響する変更の新しいカテゴリを作成するために、ターゲット型の `new` にはいくつかの問題がありましたが、既に `null` と `default`を使用していますが、これは重大な問題ではありませんでした。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

フィールドの初期化時に型引数が長すぎることに関する苦情のほとんどは、型自体で*はない型*引数に関するものであり、`new Dictionary(...)` (または同様) のような型引数だけを推論し、引数またはコレクション初期化子からローカルに型引数を推論することもできます。

## <a name="questions"></a>疑問がある場合
[questions]: #questions

- 式ツリーで使用できないようにする必要がありますか。 番号
- 機能が `dynamic` の引数とどのように連動するか。 (特別な処理はありません)
- IntelliSense での `new()`の使用方法 (1 つのターゲット型がある場合のみ)

## <a name="design-meetings"></a>会議のデザイン

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
