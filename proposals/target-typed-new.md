---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281971"
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

## <a name="specification"></a>仕様
[design]: #detailed-design

*Object_creation_expression*の*target_typed_new*新しい構文形式が受け入れられます。この*型*は省略可能です。

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

*Target_typed_new*式に型がありません。 ただし、新しい*オブジェクト作成変換*は、 *target_typed_new*からすべての型に存在する式からの暗黙的な変換です。

`T`対象の型を指定すると、`T` が `System.Nullable`のインスタンスである場合、型 `T0` は `T`基になる型になります。 それ以外の場合は `T0` が `T`ます。 型 `T` に変換される*target_typed_new*式の意味は、型として `T0` を指定する、対応する*object_creation_expression*の意味と同じです。

単項演算子または二項演算子のオペランドとして*target_typed_new*が使用されている場合、または*オブジェクトの作成変換*の対象とならない場所で使用されている場合は、コンパイル時エラーになります。

> **懸案事項を開く:** デリゲートとタプルをターゲットタイプとして許可する必要がありますか。

上記の規則には、デリゲート (参照型) と組 (構造体型) が含まれます。 どちらの型も構築可能ですが、型が inferable の場合は、匿名関数またはタプルリテラルを使用できます。
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>その他

仕様の結果は次のようになります。

- `throw new()` が許可されています (ターゲットの種類は `System.Exception`)
- ターゲット型の `new` は、バイナリ演算子では使用できません。
- 対象となる型がない場合は、単項演算子、`foreach`のコレクションです。 `using`の場合、`await` 式では、匿名型のプロパティ (`new { Prop = new() }`)、`lock` ステートメント、`sizeof`演算子のオペランドとして、LINQ クエリ内では、`fixed` 演算子の左オペランドとして、動的にディスパッチされる操作 (`new().field`) において、ステートメントを使用して、ステートメントに含まれます。,  ...`someDynamic.Method(new())``is``??`
- また、`ref`として許可されていません。
- 次の種類の型は、変換のターゲットとして許可されていません。
  - **列挙型:** `new()` は動作します (`new Enum()` が既定値を提供するために動作します) が、列挙型にコンストラクターがないため `new(1)` は機能しません。
  - **インターフェイスの種類:** これは、COM 型の対応する作成式と同じように動作します。
  - **配列型:** 配列の長さを指定するには、特別な構文が必要です。    
  - **動的:** `new dynamic()`は許可されていないため、`dynamic` を対象の型として `new()` することはできません。
  - **タプル:** これらは、基になる型を使用したオブジェクトの作成と同じ意味を持ちます。
  - *Object_creation_expression*で許可されていないその他のすべての型も、ポインター型など、除外されます。   

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
- [LDM-2020-03-25](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
