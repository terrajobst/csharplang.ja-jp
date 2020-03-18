---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "79483962"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a>参照のような型の安全性のコンパイル時間の強制

## <a name="introduction"></a>はじめに

`Span<T>` や `ReadOnlySpan<T>` などの型を扱うときに安全性規則を追加する主な理由は、そのような型を実行スタックに限定する必要があるためです。
 
`Span<T>` と同様の型がスタックのみの型である必要がある理由は2つあります。

1. `Span<T>` は、参照と範囲 `(ref T data, int length)`を含む構造体に意味があります。 実際の実装に関係なく、このような構造体への書き込みはアトミックではありません。 このような構造体の同時 "ティアリング" によって、`data`が一致しなくなり、範囲外のアクセスやタイプセーフ `length` の違反が発生する可能性があります。最終的には、"安全な" コードで GC ヒープが破損する可能性があります。
2. `Span<T>` の一部の実装では、そのフィールドのいずれかにマネージポインターが文字どおり含まれています。 マネージポインターは、ヒープオブジェクトのフィールドとしてはサポートされていません。また、GC ヒープにマネージポインターを配置するために管理するコードは、通常、JIT 時にクラッシュします。

`Span<T>` のインスタンスが実行スタックにのみ存在するように制約されている場合、上記の問題はすべて緩和されます。 

合成によって追加の問題が発生します。 通常は、`Span<T>` と `ReadOnlySpan<T>` インスタンスを埋め込む複雑なデータ型を作成することをお勧めします。 このような複合型は構造体である必要があり、`Span<T>`のすべての危険と要件を共有します。 そのため、ここで説明する安全性規則は、 **_参照のような型_** の範囲全体に適用できるものとして表示する必要があります。

[ドラフト言語仕様](#draft-language-specification)は、ref に似た型の値がスタックでのみ発生するようにすることを目的としています。

## <a name="generalized-ref-like-types-in-source-code"></a>ソースコード内の一般化された `ref-like` 型

`ref-like` 構造体は、`ref` 修飾子を使用してソースコードで明示的にマークされます。

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

構造体を ref like として指定すると、構造体には、参照に似たインスタンスフィールドを設定できます。また、構造体に対しても、ref に似た型のすべての要件が適用されます。 

## <a name="metadata-representation-or-ref-like-structs"></a>メタデータ表現または参照のような構造体

Ref に似た構造体は、 **System.runtime.compilerservices 属性**属性でマークされます。

属性は、`mscorlib`などの共通の基本ライブラリに追加されます。 属性が使用できない場合、コンパイラは、`IsReadOnlyAttribute`などの他の埋め込み要求属性と同様に内部的なを生成します。

安全性規則 (この機能が実装されている前のコンパイラを含むC# ) では、ref に似た構造体をコンパイラで使用しないようにするために、追加のメジャーが作成されます。 

サービスを使用せずに古いコンパイラで動作する、その他の適切な代替手段がない場合、既知の文字列を持つ `Obsolete` の属性が、すべての参照に似た構造体に追加されます。 Ref に似た型の使用方法を認識しているコンパイラは、この特定の形式の `Obsolete`を無視します。

一般的なメタデータ表現は次のとおりです。

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

注: 以前のコンパイラで参照のような型を使用すると、100% が失敗するようにするための目標ではありません。 これは、実現するのは困難であり、厳密には必要ありません。 たとえば、常に、動的なコードを使用して `Obsolete` を回避する方法があります。たとえば、リフレクションを使用して、参照に似た型の配列を作成することができます。

特に、ユーザーが `Obsolete` または `Deprecated` の属性を参照のような型に実際に配置する場合、`Obsolete` 属性を複数回適用することはできないため、定義済みの属性を生成しない以外に選択肢はありません。「」を参照してください。  

## <a name="examples"></a>例 :

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a>ドラフト言語の仕様

以下では、これらの型の値がスタックでのみ確実に発生するようにするために、参照のような型 (`ref struct`s) の安全規則のセットについて説明します。 参照渡しでローカルに渡すことができない場合は、より単純な安全性規則のセットを使用できます。 この仕様では、ref ローカルの安全な再割り当ても許可されます。

### <a name="overview"></a>概要

コンパイル時には、式のエスケープが許可されているスコープの概念である "安全な" エスケープ "に関連付けられています。 同様に、各左辺値については、参照されているスコープの概念は "ref-safe-escape" になります。 指定された左辺値式では、これらは異なる場合があります。

これらは、ref ローカル機能の "安全に戻る" に似ていますが、さらにきめ細かです。 式の "セーフツーリターン" によって、外側のメソッドが完全にエスケープされるかどうかだけが記録されます。これは、そのスコープがエスケープされる可能性がある (スコープはエスケープされない場合があります)。 基本的な安全性メカニズムは、次のように適用されます。 安全-エスケープスコープ S1 を持つ式 E1 から (左辺値) 式 E2 を使用して、安全な値を持つ S2 に割り当てられている場合、S2 が S1 よりも広いスコープの場合、エラーになります。 構築上、2つのスコープ S1 と S2 は、式を囲むスコープから常に安全に戻ることができるため、入れ子関係にあります。

そのためには、分析のために、メソッドの外部のスコープとメソッドの最上位スコープの2つのスコープのみをサポートするために、十分な時間を確保します。 これは、内部スコープを持つ参照のような値を作成できず、ref ローカルが再割り当てをサポートしていないためです。 ただし、ルールでは、3つ以上のスコープレベルをサポートできます。

式の*信頼できる*状態の状態を計算するための正確な規則と、式の正規表現を制御する規則に従います。

### <a name="ref-safe-to-escape"></a>参照セーフ-エスケープ

*参照セーフツーエスケープ*は、左辺値式を囲む範囲であり、左辺値がエスケープされるまでの参照が安全です。 このスコープがメソッド全体である場合は、左辺値への参照がメソッドから*安全に戻る*ことができます。

### <a name="safe-to-escape"></a>安全-エスケープ

*セーフツーエスケープ*は、式を囲むスコープで、値がエスケープされても安全です。 このスコープがメソッド全体である場合は、値がメソッドから返されることが*安全*であると言います。

型が `ref struct` 型でない式は、外側のメソッド全体から*安全に戻る*ことができます。 それ以外の場合は、以下の規則を参照してください。

#### <a name="parameters"></a>パラメーター

仮パラメーターを指定する左辺値は、次のように、参照によって (参照によって) 参照が*安全*です。
- パラメーターが `ref`、`out`、または `in` パラメーターの場合は、メソッド全体 (たとえば、`return ref` ステートメント) からの*ref セーフツーエスケープ*です。それ以外
- パラメーターが構造体型の `this` パラメーターである場合、そのパラメーターは、メソッドの最上位スコープ (ただし、メソッド自体からではなく) に対して、*参照セーフからエスケープ*されます。[サンプル](#struct-this-escape)
- それ以外の場合、パラメーターは値パラメーターであり、(メソッド自体からではなく) メソッドの最上位のスコープに対して*参照セーフからエスケープ*されます。

仮パラメーターの使用を指定する右辺値である式は、メソッド全体から (値によって) (たとえば、`return` ステートメントによって) 完全*にエスケープ*されます。 これは `this` パラメーターにも当てはまります。

#### <a name="locals"></a>ローカル

ローカル変数を指定する左辺値は、次のように、参照によって (参照によって) 参照が*安全*です。
- 変数が `ref` 変数である場合、その変数の*ref*と escape は、初期化式の*ref セーフツーエスケープ*から取得されます。それ以外
- 変数は、宣言されたスコープを*参照セーフでエスケープ*します。

ローカル変数の使用を指定する右辺値である式は、次のように*安全にエスケープでき*ます。
- ただし、上記の一般的なルールでは、型が `ref struct` 型ではないローカルは、外側のメソッド全体から*安全に戻り*ます。
- 変数が `foreach` ループの反復変数である場合、変数の*セーフツーエスケープ*のスコープは、`foreach` ループの式の*安全なエスケープ*と同じになります。
- `ref struct` 型のローカルのと、宣言の時点で初期化されていないは、外側のメソッド全体からは*安全に戻り*ます。
- それ以外の場合、変数の型は `ref struct` 型であり、変数の宣言には初期化子が必要です。 変数の*セーフツーエスケープ*のスコープは、その初期化子の*安全なエスケープ*と同じです。

#### <a name="field-reference"></a>フィールド参照

フィールドへの参照を指定する左辺値 (`e.F`) は、次のように、参照によって*安全にエスケープ*されます。
- `e` が参照型の場合は、メソッド全体からの*ref セーフツーエスケープ*です。それ以外
- `e` が値型の場合、その*参照セーフからエスケープ*は、`e`の*ref セーフツーエスケープ*から取得されます。

フィールドへの参照を指定する右辺値 (`e.F`) には、`e`の*安全なエスケープ*と同じ、*安全にエスケープ*できるスコープがあります。

#### <a name="operators-including-"></a>`?:` を含む演算子

ユーザー定義の演算子のアプリケーションは、メソッドの呼び出しとして扱われます。

`e1 + e2` や `c ? e1 : e2`などの右辺値を生成する演算子については、演算子のオペランドの*セーフツーエスケープ*の範囲内で最も狭いスコープが結果の*安全*な範囲になります。  その結果、`+e`などの右辺値を生成する単項演算子の場合、結果の*安全に*エスケープされるのは、オペランドを*安全にエスケープ*することです。

`c ? ref e1 : ref e2` などの左辺値を生成する演算子の場合
- 結果の*ref セーフツーエスケープ*は、演算子のオペランドの*ref セーフツーエスケープ*の範囲の中で最も狭いスコープです。
- オペランドが*安全にエスケープ*される必要があります。これは、結果として得られる左辺値の安全な*エスケープ*です。

#### <a name="method-invocation"></a>メソッドの呼び出し

参照を返すメソッド呼び出しの結果として得られる左辺値は、次のスコープのうち最小のものを*参照セーフ*`e1.M(e2, ...)` ます。
- 外側のメソッド全体。
- すべての `ref` および `out` 引数式の*ref セーフツーエスケープ*(受信側を除く)
- メソッドの各 `in` パラメーターに対して、左辺値、その*参照セーフ、エスケープ*、または最も近い外側のスコープである対応する式がある場合は、
- すべての引数式 (受信側を含む) の*安全なエスケープ*。

> 注: 次のようなコードを処理するには、最後の行頭文字が必要です。
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> or
> ```csharp
> return ref M(sp, 0);
> ```

メソッド呼び出し `e1.M(e2, ...)` の結果として得られる右辺値は、次のスコープのうち最も小さい方から*安全にエスケープでき*ます。
- 外側のメソッド全体。
- すべての引数式 (受信側を含む) の*安全なエスケープ*。

#### <a name="an-rvalue"></a>右辺値
右辺値は、最も近い外側のスコープから、*参照セーフからエスケープ*されます。 これは、`M(ref d.Length)` などの呼び出しで、`d` が `dynamic`型である場合に発生します。 また、`in` のパラメーターに対応する引数の処理 (および場合によっては subsumes) にも一貫性があります。 *

#### <a name="property-invocations"></a>プロパティの呼び出し

プロパティ呼び出し (`get` または `set`) は、上記の規則によって基になるメソッドのメソッド呼び出しとして扱われます。

#### `stackalloc`

Stackalloc 式は、メソッドの最上位スコープに対して*安全にエスケープ*できる右辺値です (ただし、メソッド自体からではありません)。

#### <a name="constructor-invocations"></a>コンストラクターの呼び出し

コンストラクターを呼び出す `new` 式は、構築される型を返すと見なされるメソッド呼び出しと同じ規則に従います。

さらに、オブジェクト初期化子式のすべての引数またはオペランドの、*セーフ*ツーエスケープの最小値を超えない*よう*にします。これは、初期化子が存在する場合に再帰的に行われます。 

#### <a name="span-constructor"></a>Span コンストラクター
この言語は、次の形式のコンストラクターを持たない `Span<T>` に依存しています。

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

このコンストラクターは、`ref` フィールドと区別できないフィールドとして使用される `Span<T>` を作成します。 このドキュメントで説明する安全性規則は、または .NET で有効な構成要素C#ではない `ref` フィールドによって異なります。

#### <a name="default-expressions"></a>`default` 式

`default` 式は、外側のメソッド全体から*安全にエスケープ*できます。

## <a name="language-constraints"></a>言語の制約

`ref` ローカル変数と `ref struct` 型の変数が存在しないことを確認したい場合は、スタックメモリまたは現在は動作していない変数を参照してください。 そのため、次の言語の制約があります。

- Ref パラメーター、ref local、または `ref struct` 型のローカルパラメーターをラムダまたはローカル関数に変換することはできません。

- Ref パラメーターも `ref struct` 型のパラメーターも、iterator メソッドまたは `async` メソッドの引数である可能性があります。

- Ref ローカルと `ref struct` 型のローカルのどちらも、`yield return` ステートメントまたは `await` 式の時点でスコープ内に存在することはできません。

- `ref struct` 型は、型引数として使用したり、タプル型の要素型として使用したりすることはできません。

- `ref struct` 型は、他の `ref struct`のインスタンスフィールドとして宣言された型であることを除いて、フィールドの宣言された型にすることはできません。

- `ref struct` 型を配列の要素型にすることはできません。

- `ref struct` 型の値をボックス化することはできません。
  - `ref struct` 型から `object` 型、または `System.ValueType`型への変換は行われません。
  - `ref struct` 型は、インターフェイスを実装するように宣言することはできません
  - `object` または `System.ValueType` で宣言されているが、`ref struct` 型でオーバーライドされていないインスタンスメソッドは、その `ref struct` 型のレシーバーで呼び出すことができます。
  - メソッドをデリゲート型に変換することによって、`ref struct` 型のインスタンスメソッドをキャプチャすることはできません。

- 参照の再割り当て `ref e1 = ref e2`の場合、`e2` の*ref セーフツーエスケープ*は、`e1`の*ref セーフツーエスケープ*の範囲内である必要があります。

- Ref return ステートメント `return ref e1`では、`e1` の*ref セーフツーエスケープ*は、メソッド全体からの*ref セーフツーエスケープ*である必要があります。 (TODO: `e1`、メソッド全体から*安全にエスケープ*する必要がある規則や、冗長であるという規則も必要です。

- Return ステートメント `return e1`の場合、`e1` の*安全*な相互エスケープは、メソッド全体から*安全にエスケープ*する必要があります。

- 割り当て `e1 = e2`の場合、`e1` の型が `ref struct` 型である場合、`e2` の*安全なエスケープ*は、`e1`の*安全なエスケープ*と同じ範囲内である必要があります。

- `ref struct` 型の `ref` または `out` 引数 (レシーバーを含む) がある場合のメソッド呼び出しについては、*セーフツーエスケープ*E1 を使用した場合、引数 (受信側を含む) の方が、E1 より*も幅が*狭くなることがあります。 [サンプル](#method-arguments-must-match)

- ローカル関数または匿名関数が、外側のスコープで宣言されている `ref struct` 型のローカルまたはパラメーターを参照していない可能性があります。

> ***懸案事項を開く:*** たとえば、コード内の await 式で `ref struct` 型のスタック値をスピルする必要がある場合に、エラーが発生することを許可するルールが必要です。
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a>コメント
これらの説明とサンプルは、上記の安全性規則の多くが存在する理由を説明するのに役立ちます。

### <a name="method-arguments-must-match"></a>メソッドの引数は一致しなければなりません
`out`が存在するメソッドを呼び出すときに、受信者を含む `ref struct` である `ref` パラメーターには、すべての `ref struct` が同じ有効期間を持つ必要があります。 は、メソッドのC#シグネチャで使用可能な情報と呼び出しサイトの値の有効期間に基づいて、有効期間の安全性に関するすべての決定を行う必要があるため、この設定が必要です。 

`ref struct` されている `ref` パラメーターがある場合は、おそれがコンテンツを交換できることがあります。 そのため、呼び出しサイトでは、これらの**可能性**のあるすべてのスワップに互換性があることを確認する必要があります。 言語によって強制されなかった場合は、次のような不適切なコードを使用できます。

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

受信側の制限は、その内容がいずれも ref セーフツーエスケープではないのに、指定された値を格納できるため、受信側の制限が必要です。 つまり、有効期間が一致しない場合は、次の方法でタイプセーフホールを作成できます。

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a>Struct This Escape
安全性規則に関しては、インスタンスメンバーの `this` 値は、メンバーのパラメーターとしてモデル化されます。 `struct` では、`this` の型は `ref S` 実際には `S` (という名前の `class / struct` のメンバーの場合) `class` になります。 

ただし `this` には、他の `ref` パラメーターとは異なるエスケープ規則があります。 具体的には、他のパラメーターは次のように、参照セーフツーエスケープではありません。

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

この制限の理由は、実際には `struct` メンバーの呼び出しとはあまりありません。 受信側が右辺値である `struct` メンバーのメンバー呼び出しに関しては、いくつかの規則を使用する必要があります。 しかし、これは非常に便利です。 

この制限の理由は、実際にはインターフェイスの呼び出しに関するものです。 具体的には、次のサンプルをコンパイルする必要があるかどうかについては、次のようになります。

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

`T` が `struct`としてインスタンス化される場合を考えてみます。 `this` パラメーターが参照セーフ-エスケープの場合、`p.Get` の戻り値はスタックを指している可能性があります (具体的には、`T`のインスタンス化された型の内部のフィールドである可能性があります)。 つまり、言語では、このサンプルをコンパイルすることを許可できませんでした。これは、スタック位置に `ref` を返す可能性があるためです。 一方、`this` が参照セーフでエスケープされていない場合、`p.Get` はスタックを参照できないため、これを安全に返すことができます。 

このため、`struct` 内の `this` の escapability ビリティは、実際にはインターフェイスに関するものです。 この機能は正常に実行できますが、トレードオフになっています。 インターフェイスの柔軟性を高めるために、最終的に設計はダウンしました。 

ただし、今後この問題を緩和する可能性はあります。 

## <a name="future-considerations"></a>将来の注意事項

### <a name="length-one-spant-over-ref-values"></a>Ref 値で >\<T の長さの1スパン
現時点では有効ではありませんが、値に対して1つの `Span<T>` インスタンスを作成する方が有益な場合があります。

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

この機能は、より長い長さの `Span<T>` インスタンスに許容されるように、[固定サイズのバッファー](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md)に対して制限を引き上げると、より説得力が高まります。 

このパスを下げる必要がある場合は、このような `Span<T>` インスタンスが下方向にのみ表示されるようにすることで、このような状況に対応できます。 これは、作成されたスコープに対してのみ*安全にエスケープ*されたことです。 これにより、`ref struct`の `ref struct` の戻り値またはフィールドを使用してメソッドをエスケープする `ref` 値を、言語で考慮する必要がなくなりました。 また、このような方法で `ref` パラメーターをキャプチャするように、このようなコンストラクターを認識するためにさらに変更が必要になる場合もあります。
