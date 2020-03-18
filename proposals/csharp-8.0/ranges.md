---
ms.openlocfilehash: d6519ff57b4a98c4eec8ccbf310303432ac3255e
ms.sourcegitcommit: 65ea1e6dc02853e37e7f2088e2b6cc08d01d1044
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "79483950"
---
# <a name="ranges"></a>範囲

## <a name="summary"></a>まとめ

この機能は、`System.Index` と `System.Range` オブジェクトの構築を可能にし、実行時にそれらを使用してコレクションのインデックス作成とスライスを行うことができる2つの新しい演算子を提供することです。

## <a name="overview"></a>概要

### <a name="well-known-types-and-members"></a>既知の型とメンバー

`System.Index` と `System.Range`に新しい構文形式を使用するには、使用される構文形式に応じて、新しい既知の型とメンバーが必要になることがあります。

"Hat" 演算子 (`^`) を使用するには、次のものが必要です。

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

`System.Index` 型を配列要素アクセスの引数として使用するには、次のメンバーが必要です。

```csharp
int System.Index.GetOffset(int length);
```

`System.Range` の `..` 構文には、`System.Range` の種類と、次のメンバーの1つ以上が必要です。

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

`..` 構文では、どちらの場合も、いずれの引数も指定できません。 引数の数に関係なく、`Range` 構文を使用するには、`Range` コンストラクターが常に十分です。 ただし、他のメンバーのいずれかが存在し、1つ以上の `..` 引数が不足している場合は、適切なメンバーを置き換えることができます。

最後に、配列要素のアクセス式で使用される `System.Range` 型の値については、次のメンバーが存在する必要があります。

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a>System.string

C#では、最終的にコレクションのインデックスを作成する方法はありませんが、ほとんどのインデクサーは "from start" の概念を使用するか、"length-i" 式を実行します。 "終了" を意味する新しいインデックス式が導入されています。 この機能により、新しい単項プレフィックス "hat" 演算子が導入されます。 1つのオペランドは `System.Int32`に変換可能である必要があります。 このメソッドは、適切な `System.Index` ファクトリメソッド呼び出しに下げられます。

次の追加の構文形式を使用して、 *unary_expression*の文法を拡張します。

```antlr
unary_expression
    : '^' unary_expression
    ;
```

*End 演算子の index*を呼び出します。 終了演算子*の*定義済みインデックスは次のとおりです。

```csharp
    System.Index operator ^(int fromEnd);
```

この演算子の動作は、0以上の入力値に対してのみ定義されます。

例 :

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a>System.string

C#には、コレクションの "範囲" または "スライス" にアクセスする構文的な方法はありません。 通常、ユーザーは、メモリのスライスをフィルター処理したり操作したりするための複雑な構造を実装したり、`list.Skip(5).Take(2)`のような LINQ メソッドを活用したりすることが求められます。 `System.Span<T>` とその他の類似する型を追加することで、このような操作を言語/ランタイムのより深いレベルでサポートし、インターフェイスを統合することが重要になります。

この言語では、新しい範囲演算子 `x..y`が導入されます。 2つの式を受け取るバイナリ挿入演算子です。 どちらのオペランドも省略できます (以下の例を参照)。 `System.Index`に変換可能である必要があります。 このメソッドは、適切な `System.Range` ファクトリメソッド呼び出しに下げられます。

\- C# *Multiplicative_expression*の文法規則を次のように置き換えます (新しい優先順位を導入するため)。

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

*範囲演算子*のすべての形式の優先順位は同じです。 この新しい優先順位グループは、*単項演算子*よりも小さく、 *mulitiplicative 算術演算子*よりも高くなっています。

`..` 演算子を*範囲演算子*と呼びます。 組み込みの範囲演算子は、次の形式の組み込み演算子の呼び出しに対応するように、ほぼ理解できます。

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

例 :

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

さらに、多次元署名に対して整数とインデックスを混在させるためにオーバーロードする必要がないように、`System.Index` は `System.Int32`からの暗黙の型変換を必要とします。

## <a name="adding-index-and-range-support-to-existing-library-types"></a>既存のライブラリの種類へのインデックスと範囲のサポートの追加

### <a name="implicit-index-support"></a>暗黙のインデックスのサポート

この言語では、次の条件を満たす型の `Index` 型の単一のパラメーターを持つインスタンスインデクサーメンバーが提供されます。

- 種類は不可能です。
- 型には、引数として1つの `int` を受け取る、アクセス可能なインスタンスインデクサーがあります。
- この型には、最初のパラメーターとして `Index` を受け取るアクセス可能なインスタンスインデクサーがありません。 `Index` は唯一のパラメーターであるか、またはその他のパラメーターは省略可能である必要があります。

型は、`Length` という名前のプロパティがある場合、またはアクセス可能な getter と戻り値の型が `int`の `Count` である場合に、***不可能***になります。 この言語では、このプロパティを使用して `Index` 型の式を式の位置で `int` に変換することができます。 `Index` 型を使用する必要はありません。 `Length` と `Count` の両方が存在する場合は、`Length` が優先されます。 わかりやすくするために、提案では、`Count` または `Length`を表すために `Length` という名前を使用します。

このような型の場合、言語は `T this[Index index]` フォームのインデックスメンバーがあるかのように動作します。 `T` は、`ref` スタイルの注釈を含む `int` ベースのインデクサーの戻り値の型です。 新しいメンバーには、`int` インデクサーと一致するアクセシビリティを持つメンバーと同じ `get` と `set` が設定されます。 

`Index` 型の引数を `int` に変換し、`int` ベースのインデクサーへの呼び出しを出力することによって、新しいインデクサーが実装されます。 説明のために、`receiver[expr]`の例を使用します。 `expr` から `int` への変換は次のように行われます。

- 引数が `^expr2` の形式で、`expr2` の型が `int`場合、その引数は `receiver.Length - expr2`に変換されます。
- それ以外の場合は、`expr.GetOffset(receiver.Length)`として変換されます。

これにより、開発者は、既存の型で `Index` 機能を使用して、変更を加える必要がなくなります。 次に例を示します。

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

`receiver` と `Length` の式は、副作用が1回だけ実行されるように、必要に応じて書き込まれます。 次に例を示します。

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

このコードは、"Get Length 3" を出力します。

### <a name="implicit-range-support"></a>暗黙的な範囲のサポート

この言語では、次の条件を満たす型の `Range` 型の単一のパラメーターを持つインスタンスインデクサーメンバーが提供されます。

- 種類は不可能です。
- 型には、`int`型の2つのパラメーターを持つ `Slice` という名前のアクセス可能なメンバーがあります。
- この型には、最初のパラメーターとして1つの `Range` を受け取るインスタンスインデクサーがありません。 `Range` は唯一のパラメーターであるか、またはその他のパラメーターは省略可能である必要があります。

このような型の場合、言語は `T this[Range range]` フォームのインデックスメンバーがあるかのようにバインドされます。 `T` は `Slice` メソッドの戻り値の型であり、`ref` スタイルの注釈が含まれます。 また、新しいメンバーのアクセシビリティは、`Slice`と一致します。 

`Range` ベースのインデクサーが `receiver`という名前の式でバインドされている場合、`Range` 式を2つの値に変換し、その後 `Slice` メソッドに渡されることで、このインデクサーは減少します。 説明のために、`receiver[expr]`の例を使用します。

`Slice` の最初の引数は、次のように型指定された式を変換することによって取得されます。

- `expr` の形式が `expr1..expr2` (`expr2` は省略可能) で、`expr1` の型が `int`の場合、`expr1`として出力されます。
- `expr` が `^expr1..expr2` 形式 (`expr2` を省略できる) の場合、`receiver.Length - expr1`として出力されます。
- `expr` が `..expr2` 形式 (`expr2` を省略できる) の場合、`0`として出力されます。
- それ以外の場合は、`expr.Start.GetOffset(receiver.Length)`として出力されます。

この値は、2番目の `Slice` 引数の計算で再利用されます。 この操作を実行すると、`start`と呼ばれます。 `Slice` の2番目の引数は、次のように、型指定された式を次のように変換することによって取得します。

- `expr` の形式が `expr1..expr2` (`expr1` は省略可能) で、`expr2` の型が `int`の場合、`expr2 - start`として出力されます。
- `expr` が `expr1..^expr2` 形式 (`expr1` を省略できる) の場合、`(receiver.Length - expr2) - start`として出力されます。
- `expr` が `expr1..` 形式 (`expr1` を省略できる) の場合、`receiver.Length - start`として出力されます。
- それ以外の場合は、`expr.End.GetOffset(receiver.Length) - start`として出力されます。

`receiver`、`Length`、および `expr` の式は、副作用が1回だけ実行されるようにするために必要に応じて書き込まれます。 次に例を示します。

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

このコードは、"Get Length 2" を出力します。

この言語では、次の既知の型が特別なケースになります。 

- `string`: `Slice`の代わりに `Substring` メソッドが使用されます。
- `array`: `Slice`の代わりに `System.Reflection.CompilerServices.GetSubArray` メソッドが使用されます。

## <a name="alternatives"></a>代替

新しい演算子 (`^` と `..`) は構文砂糖です。 この機能は `System.Index` および `System.Range` ファクトリメソッドを明示的に呼び出すことによって実装できますが、多くの定型コードが生成され、エクスペリエンスはにくいになります。

## <a name="il-representation"></a>IL 表現

これら2つの演算子は、通常のインデクサー/メソッド呼び出しに対して低下しますが、それ以降のコンパイラレイヤーは変更されません。

## <a name="runtime-behavior"></a>実行時の動作

- コンパイラでは、配列や文字列などの組み込み型のインデクサーを最適化し、適切な既存のメソッドへのインデックス作成を下げることができます。
- 負の値を使用して構築された場合、`System.Index` はをスローします。
- `^0` はスローしませんが、指定されたコレクションまたは列挙型の長さに変換されます。
- `Range.All` は `0..^0`と意味的に同等であり、これらのインデックスに分解ことができます。

## <a name="considerations"></a>考慮事項

### <a name="detect-indexable-based-on-icollection"></a>ICollection に基づいてインデックス可能の検出

この動作のインスピレーションは、コレクション初期化子でした。 型の構造体を使用して、機能が選択されたことを伝えます。 コレクション初期化子型の場合は、インターフェイス `IEnumerable` (非ジェネリック) を実装することによって、機能を選択できます。

この提案では、最初に、型がインデックス可能と見なされるために `ICollection` を実装する必要があります。 ただし、次のような特殊なケースが必要でした。

- `ref struct`: これらのインターフェイスを実装することはできません `Span<T>` のような型は、インデックス/範囲のサポートに適しています。 
- `string`: `ICollection` を実装せず、`interface` に大きなコストを追加します。

このため、キーの種類をサポートするには、特別な大文字と小文字の区別が既に必要です。 `string` の特殊な大文字と小文字の区別は、他の領域 (`foreach`、定数など) での言語の方が興味深いものではありません。`ref struct` の特殊な大文字と小文字の区別は、型のクラス全体に対して特殊な大文字と小文字が区別されるため、より詳細になります。 戻り値の型が `int`の `Count` という名前のプロパティがあるだけであれば、インデックスを作成できます。 

この設計は、戻り値の型が `int` の `Length`  / プロパティ `Count`を持つ任意の型がインデックス可能であるということを考慮しています。 これにより、`string` と配列の場合でも、特殊な大文字と小文字がすべて削除されます。

### <a name="detect-just-count"></a>カウントのみ検出

`Count` または `Length` のプロパティ名を検出すると、設計が少し複雑になります。 1つだけを選択するだけでは、多数の型を除外した場合には不十分です。

- `Length`の使用: システムコレクションとサブ名前空間のすべてのコレクションを除外します。 これらは `ICollection` から派生する傾向があるため、長さが `Count` を優先します。
- `Count`を使用: `string`、配列、`Span<T>`、およびほとんどの `ref struct` ベースの型を除外します。

インデックス可能な型の初期検出に対する余分な複雑さは、他の側面での単純化によって上回るされます。

### <a name="choice-of-slice-as-a-name"></a>名前としてのスライスの選択

.NET でのスライススタイル操作の事実上の標準名として `Slice` 名前が選択されました。 Netcoreapp 2.1 以降では、すべてのスパンスタイルの種類でスライス操作に `Slice` 名前が使用されます。 Netcoreapp 2.1 より前の例では、スライスの例はありません。 `List<T>`、`ArraySegment<T>`、`SortedList<T>` などの型は、スライスには理想的ですが、型が追加されたときの概念は存在しませんでした。 

そのため、唯一の例として、名前として選択された `Slice` ます。

### <a name="index-target-type-conversion"></a>インデックスターゲット型の変換

インデクサー式で `Index` 変換を表示するもう1つの方法は、ターゲット型の変換です。 `return_type this[Index]`フォームのメンバーであるかのようにバインドするのではなく、変換先の型指定された変換を `int`に割り当てます。 

この概念は、不可能型のすべてのメンバーアクセスに一般化されている可能性があります。 `Index` 型の式がインスタンスメンバー呼び出しの引数として使用され、受信側が不可能である場合、式は `int`に変換先の型を変換します。 この変換に適用できるメンバー呼び出しには、メソッド、インデクサー、プロパティ、拡張メソッドなどがあります。受信者が存在しないため、コンストラクターのみが除外されます。 

ターゲット型の変換は、`Index`の型を持つ任意の式に対して次のように実装されます。 説明のために、`receiver[expr]`の例を使用します。

- `expr` が `^expr2` フォームで、`expr2` の種類が `int`場合、`receiver.Length - expr2`に変換されます。
- それ以外の場合は、`expr.GetOffset(receiver.Length)`として変換されます。

`receiver` と `Length` の式は、副作用が1回だけ実行されるように、必要に応じて書き込まれます。 次に例を示します。

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

このコードは、"Get Length 3" を出力します。 

この機能は、インデックスを表すパラメーターを持つ任意のメンバーに役立ちます。 たとえば、「 `List<T>.InsertAt` 」のように指定します。 また、式がインデックス作成に使用されるかどうかについてのガイダンスを言語で提供できないため、混乱が生じる可能性もあります。 不可能型でメンバーを呼び出すときに、すべての `Index` 式を `int` に変換することができます。 

制限:

- この変換は、`Index` 型の式がメンバーの引数として直接指定されている場合にのみ適用されます。 入れ子になった式には適用されません。

## <a name="decisions-made-during-implementation"></a>実装時に行われる決定

- パターン内のすべてのメンバーはインスタンスメンバーである必要があります
- 長さのメソッドが見つかり、戻り値の型が正しくない場合は、カウントの検索を続行します。
- インデックスパターンに使用されるインデクサーには、int パラメーターを1つだけ指定しなければなりません。
- 範囲パターンに使用するスライスメソッドには、2つの int パラメーターを指定しなければなりません。
- パターンメンバーを検索する場合は、構築されたメンバーではなく、元の定義を探します。

## <a name="design-meetings"></a>会議のデザイン

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a>疑問がある場合
