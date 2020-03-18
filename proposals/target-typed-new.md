---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483530"
---

# <a name="target-typed-new-expressions"></a>ターゲット型の `new` 式

* [x] が提案されています
* [x][プロトタイプ](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* [] の実装
* [] 仕様

## <a name="summary"></a>まとめ
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

- **任意の構造体型**
- **任意の参照型**
- コンストラクターまたは `struct` 制約を持つ**任意の型パラメーター**

次の例外があります。

- **列挙型:** すべての列挙型に定数ゼロが含まれていないため、明示的な enum メンバーを使用することをお勧めします。
- **インターフェイス型:** これはニッチ機能であり、型を明示的に言及することをお勧めします。
- **配列型:** 配列の長さを指定するには、特別な構文が必要です。
- **構造体の既定のコンストラクター**: この規則により、すべてのプリミティブ型とほとんどの値の型が除外されます。 このような型の既定値を使用する場合は、代わりに `default` を記述できます。

*Object_creation_expression*で許可されていないその他のすべての型も、ポインター型など、除外されます。

> **懸案事項を開く:** デリゲートとタプルをターゲットタイプとして許可する必要がありますか。

上記の規則には、デリゲート (参照型) と組 (構造体型) が含まれます。 どちらの型も構築可能ですが、型が inferable の場合は、匿名関数またはタプルリテラルを使用できます。
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> **懸案事項を開く:** `Exception` をターゲットの種類として `throw new()` できるようにする必要がありますか。

現在 `throw null` していますが、`throw default` はありません (ただし、同じ効果があります)。 一方、`throw new()` は、`throw new Exception(...)`の短縮形として実際に役立つ可能性があります。 現在の仕様によって既に許可されていることに注意してください。 `Exception` は参照型であり、throw ステートメントの仕様では、式が `Exception`に変換されることを示します。

> **懸案事項を開く:** ユーザー定義の比較演算子と算術演算子を使用して、ターゲット型の `new` の使用を許可する必要がありますか。

比較のために、`default` でサポートされるのは、等値 (ユーザー定義および組み込み) 演算子だけです。 `new()` のために他の演算子をサポートすることは理にかなっていますか。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

[なし] :

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
