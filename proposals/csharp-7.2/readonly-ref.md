---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "79484022"
---
# <a name="readonly-references"></a>読み取り専用の参照

* [x] が提案されています
* [x] プロトタイプ
* [x] 実装: 開始しました
* [] 仕様: 開始されていません

## <a name="summary"></a>まとめ
[summary]: #summary

"読み取り専用の参照" 機能は実際には、参照によって変数を渡す効率性を活用し、変更するデータを公開しない機能のグループです。  
- `in` パラメーター
- `ref readonly` 戻り値
- `readonly` 構造体
- `ref`/`in` 拡張メソッド
- `ref readonly` ローカル
- `ref` 条件式

## <a name="passing-arguments-as-readonly-references"></a>読み取り専用参照として引数を渡す。

このトピックに触れている既存の提案は、特に多くの詳細を説明することなく、読み取り専用パラメーターの特殊なケースとして https://github.com/dotnet/roslyn/issues/115 ます。
ここでは、それ自体がまったく新しいものではないということを認識したいだけです。

### <a name="motivation"></a>目的

この機能C#の前には、構造体変数をメソッド呼び出しに渡すことを目的とした効率的な方法がありませんでした。変更する必要はありません。 標準の値渡し引数は、不要なコストを追加するコピーを意味します。  を使用すると、ユーザーは-ref 引数を渡し、コメント/ドキュメントに依存して、呼び出し先によるデータの変換が想定されていないことを示すことができます。 多くの理由により、これは適切な解決策ではありません。  
例としては、パフォーマンスに関する考慮事項のために、 [XNA](https://msdn.microsoft.com/library/bb194944.aspx)のように、単純に ref オペランドがあることがわかります。 Roslyn コンパイラ自体には、割り当てを回避するために構造体を使用するコードがあり、その後、コストのコピーを回避するために参照によって渡されます。

### <a name="solution-in-parameters"></a>ソリューション (`in` パラメーター)

`out` パラメーターと同様に、`in` パラメーターは、呼び出し先からの追加の保証を持つマネージ参照として渡されます。  
他の用途の前に呼び出し先によって割り当てられる_必要が_ある `out` パラメーターとは異なり、`in` パラメーターを呼び出し先で割り当てることはできません。

結果として `in` パラメーターを使用して、呼び出し先によって変更に引数を公開せずに間接的な引数を渡すことができます。

### <a name="declaring-in-parameters"></a>`in` パラメーターの宣言

`in` パラメーターは、パラメーターシグネチャの修飾子として `in` キーワードを使用して宣言されます。

すべての目的で、`in` パラメーターは `readonly` 変数として扱われます。 メソッド内での `in` パラメーターの使用に関する制限のほとんどは、`readonly` のフィールドと同じです。

> 実際、`in` パラメーターは `readonly` フィールドを表す場合があります。 制限の類似性は偶然ではありません。

たとえば、構造体型を持つ `in` パラメーターのフィールドは、すべて `readonly` 変数として再帰的に分類されます。

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- `in` パラメーターは、通常の byval パラメーターが許可されている任意の場所で使用できます。 これには、インデクサー、演算子 (変換を含む)、デリゲート、ラムダ、ローカル関数が含まれます。

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- `in` は `out` と組み合わせて使用することも、`out` をと組み合わせることもできません。

- `ref`/`out`のオーバーロードは許可されていません。 `in` の相違点 /。

- 通常の byval でのオーバーロードと `in` の違いが許可されます。

- OHI の目的 (オーバーロード、非表示、実装) では、`in` は `out` パラメーターと同様に動作します。
同じルールがすべて適用されます。
たとえば、オーバーライドするメソッドは、`in` パラメーターと、id 変換可能な型の `in` パラメーターを一致させる必要があります。

- デリゲート/ラムダ/メソッドグループ変換のために、`in` は `out` パラメーターと同様に動作します。
ラムダと適用可能なメソッドグループ変換の候補は、ターゲットデリゲートのパラメーターと、id 変換可能な型の `in` パラメーターを `in` 一致させる必要があります。

- 一般的な分散のために、`in` パラメーターは非バリアントです。

> 注: 参照型またはプリミティブ型を持つパラメーター `in` には警告がありません。
一般には意味がないかもしれませんが、場合によっては、ユーザーが `in`としてプリミティブを渡す必要があります。 例-`T` が `int`に置き換えられたとき、またはのようなメソッドがあるときに、`Method(in T param)` などのジェネリックメソッドをオーバーライドすると、`Volatile.Read(in int location)`
>
> `in` パラメーターが非効率的に使用された場合に警告するアナライザーがあると想定されていますが、このような分析のルールは、言語仕様の一部になるにはあいまいすぎます。

### <a name="use-of-in-at-call-sites-in-arguments"></a>呼び出しサイトでの `in` の使用。 (`in` の引数)

引数を `in` パラメーターに渡すには、2つの方法があります。

#### <a name="in-arguments-can-match-in-parameters"></a>`in` の引数は `in` パラメーターと一致できます。

呼び出しサイトで `in` 修飾子を持つ引数は、`in` パラメーターと一致できます。

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- `in` 引数は_読み取り可能_な左辺値 (*) である必要があります。
例: `M1(in 42)` が無効です

> (*)[左辺値と右辺](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue)値の概念は、言語によって異なります。  
ここで、左辺値は、直接参照できる場所を表す式を意味します。
右辺値は、それ自体には保持されない一時的な結果を生成する式を意味します。  

- 具体的には、`readonly` フィールド、`in` パラメーター、またはその他の正式 `readonly` 変数を `in` 引数として渡すことが有効です。
例: `dictionary[in Guid.Empty]` は有効です。 `Guid.Empty` は、静的な読み取り専用フィールドです。

- `in` 引数には、パラメーターの型に_変換_可能な型を指定する必要があります。
例: `M1<object>(in Guid.Empty)` が無効です。 `Guid.Empty` は、 _id に変換_できません `object`

上記の規則の目的は、`in` 引数が引数の変数の_別名_を保証することです。 呼び出し先は、常に、引数によって表される同じ場所への直接参照を受け取ります。

- まれに、同じ呼び出しのオペランドとして使用される `await` 式によって `in` の引数がスタックに含まれる必要がある場合は、`out` 引数と `ref` 引数と同じ動作が使用されます。この変数には、透過的な方法で書き込まれない場合、エラーが報告されます。

例 :
1. `M1(in staticField, await SomethingAsync())` が有効です。
`staticField` は、観測可能な副作用なしで複数回アクセスできる静的フィールドです。 したがって、副作用の順序とエイリアスの要件の両方を指定できます。
2. `M1(in RefReturningMethod(), await SomethingAsync())` によってエラーが生成されます。
`RefReturningMethod()` は、メソッドを返す `ref` です。 メソッド呼び出しには観測可能な副作用がある可能性があるため、`SomethingAsync()` オペランドの前に評価する必要があります。 ただし、呼び出しの結果は、直接的な参照を必要としない `await` 中断ポイントを越えて保持できない参照になります。

> 注: スタックの書き込むエラーは、実装固有の制限事項と見なされます。 したがって、オーバーロードの解決やラムダの推論には影響しません。

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a>通常の byval 引数は `in` パラメーターと一致できます。

修飾子のない通常の引数は `in` パラメーターと一致させることができます。 このような場合、引数の緩やかな制約は、通常の byval 引数と同じになります。

このシナリオの目的は、Api のパラメーターを `in`、直接参照として引数を渡すことができない場合 (例: リテラル、計算結果、`await`の結果、またはより具体的な型を持つ必要がある引数) に、ユーザーが防ぎになる可能性があることです。  
これらのすべてのケースには、引数の値を適切な型の一時的なローカルに格納し、そのローカルを `in` 引数として渡すという、簡単なソリューションがあります。  
このような定型コードコンパイラの必要性を軽減するには、必要に応じて、呼び出しサイトに `in` 修飾子が存在しないときに、同じ変換を実行します。  

また、演算子の呼び出しや拡張メソッドの `in` など、場合によっては、`in` を指定する構文的な方法はありません。 それだけでは、`in` パラメーターに一致するときに通常の byval 引数の動作を指定する必要があります。

特に次の点に違いがあります。

- 右辺値を渡すことができます。
このような場合は、一時への参照が渡されます。
例:
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- 暗黙的な変換は許可されます。

> これは、実際には右辺値を渡す特殊なケースです。  

このような場合は、変換された一時的な値への参照が渡されます。
例:
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- `in` 拡張メソッドの受信者の場合 (`ref` 拡張メソッドではなく)、右辺値または暗黙的な_引数変換_が許可されます。
このような場合は、変換された一時的な値への参照が渡されます。
例:
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
`ref`/`in` 拡張メソッドの詳細については、こちらのドキュメントを参照してください。

- `await` オペランドによる引数の書き込むは、必要に応じて "値渡し" できます。
場合によっては、引数への直接参照を指定することはできないので、引数の値のコピーは代わりに使用され `await`。  
例:
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
副作用のある呼び出しの結果は、`await` の中断をまたいで保持できない参照なので、実際の値を含む一時は (通常の byval パラメーターの場合と同様に) 保持されます。

#### <a name="omitted-optional-arguments"></a>省略可能な引数を省略します。

`in` パラメーターで既定値を指定できます。 これにより、対応する引数が省略可能になります。

呼び出しサイトで省略可能な引数を省略すると、一時によって既定値が渡されます。

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a>一般的なエイリアス動作

`ref` と `out` 変数と同じように、`in` 変数は既存の場所への参照または別名です。

呼び出し先は、それらへの書き込みを許可されていませんが、`in` パラメーターを読み取ると、他の評価の副作用として異なる値が観察される可能性があります。

例:

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a>ローカル変数のパラメーターとキャプチャを `in` します。  
ラムダ/非同期キャプチャのために `in` パラメーターは `out` および `ref` パラメーターと同じように動作します。

- クロージャで `in` パラメーターをキャプチャすることはできません
- `in` のパラメーターは反復子メソッドでは使用できません
- `in` パラメーターは、非同期メソッドでは使用できません

### <a name="temporary-variables"></a>一時変数。  
`in` パラメーター渡しを使用する場合は、一時的なローカル変数を間接的に使用することが必要になる場合があります。  
- 呼び出しサイトで `in`を使用する場合、`in` の引数は常に直接エイリアスとして渡されます。 このような場合、一時は使用されません。
- 呼び出しサイトで `in`が使用されていない場合、`in` の引数を直接エイリアスにする必要はありません。 引数が左辺値でない場合は、一時的なを使用できます。
- `in` パラメーターに既定値を指定できます。 呼び出しサイトで対応する引数を省略すると、既定値は一時的に渡されます。
- `in` の引数には、id を保持していないものも含め、暗黙的な変換を含めることができます。 そのような場合は、一時が使用されます。
- 通常の構造体呼び出しのレシーバーは、書き込み可能な左辺値ではない場合があります (**既存の case!** )。 そのような場合は、一時が使用されます。

引数一時要素の有効期間は、呼び出しサイトの最も近いスコープに一致します。

一時変数の正式な有効期間は、参照によって返される変数のエスケープ分析に関連するシナリオで意味的に意味があります。

### <a name="metadata-representation-of-in-parameters"></a>`in` パラメーターのメタデータ表現。
Byref パラメーターに `System.Runtime.CompilerServices.IsReadOnlyAttribute` が適用されている場合は、パラメーターが `in` パラメーターであることを意味します。

また、メソッドが*abstract*または*virtual*の場合、このようなパラメーターのシグネチャ (およびそのようなパラメーターのみ) には `modreq[System.Runtime.InteropServices.InAttribute]`が必要です。

**動機**: `in` パラメーターをオーバーライドまたは実装するメソッドが一致する場合に、この処理が行われます。

デリゲートの `Invoke` メソッドにも同じ要件が適用されます。

**動機**: デリゲートの作成時または割り当て時に既存のコンパイラが単に `readonly` を無視できないようにします。

## <a name="returning-by-readonly-reference"></a>読み取り専用の参照によって返されます。

### <a name="motivation"></a>目的
このサブ機能の目的は、`in` パラメーターの理由 (コピーの回避、戻り側での処理) にほぼ対称です。 この機能を使用する前に、メソッドまたはインデクサーに2つのオプションがありました。 1) 参照によって返され、変更または2に公開されることがありますが、値によって返されます。

### <a name="solution-ref-readonly-returns"></a>ソリューション (`ref readonly` が返す)  
この機能により、メンバーは変数を変更に公開せずに参照渡しで返すことができます。

### <a name="declaring-ref-readonly-returning-members"></a>メンバーを返す `ref readonly` の宣言

戻り値のシグネチャに `ref readonly` 修飾子の組み合わせは、メンバーが読み取り専用の参照を返すことを示すために使用されます。

すべての目的において、`ref readonly` メンバーは、`readonly` フィールドや `in` パラメーターと同様に `readonly` 変数として扱われます。

たとえば、構造体型を持つ `ref readonly` メンバーのフィールドはすべて、`readonly` 変数として再帰的に分類されます。 -`in` 引数として渡すことはできますが、`ref` または `out` 引数として渡すことはできません。

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- `ref readonly` の戻り値は、同じ場所で許可されていますが `ref` 返されます。
これには、インデクサー、デリゲート、ラムダ、ローカル関数が含まれます。

- `ref`/`ref readonly`/相違点でのオーバーロードは許可されていません。

- 通常の byval でのオーバーロードが許可され、`ref readonly` 戻り値の違いがあります。

- OHI (オーバーロード、非表示、実装) のために `ref readonly` は似ていますが `ref`とは異なります。
たとえば、`ref readonly` 1 をオーバーライドするメソッドは、それ自体が `ref readonly` で、id 変換可能な型を持つ必要があります。

- デリゲート/ラムダ/メソッドグループ変換のために `ref readonly` は似ていますが `ref`とは異なります。
ラムダおよび適用可能なメソッドグループ変換の候補は、id 変換可能な型の `ref readonly` 戻り値を使用して、ターゲットデリゲートの `ref readonly` 返す必要があります。

- 一般的な分散のために、`ref readonly` 戻り値は非バリアントです。

> 注: 参照型またはプリミティブ型を持つ `ref readonly` が返された場合、警告は発生しません。
一般には意味がないかもしれませんが、場合によっては、ユーザーが `in`としてプリミティブを渡す必要があります。 例-`T` が置換されたときに、`ref readonly T Method()` などのジェネリックメソッドをオーバーライドして `int`します。
>
>`ref readonly` の戻り値が非効率的に使用された場合に警告するアナライザーがあると考えられますが、このような分析のルールは、言語仕様の一部になるにはあいまいすぎます。

### <a name="returning-from-ref-readonly-members"></a>`ref readonly` メンバーからの戻り
メソッド本体内では、構文は通常の ref 戻り値と同じです。 `readonly` は、含んでいるメソッドから推論されます。

これは、`return ref readonly <expression>` が不要であり、常にエラーになる `readonly` 部分でのみ不一致が発生する可能性があることです。
ただし、厳密なエイリアスと値によって何らかの値が渡される他のシナリオとの一貫性を確保するには、`ref` が必要です。

> `in` パラメーターの場合とは異なり、`ref readonly` はローカルコピー経由で返されることはありません。 このような手法が返された直後にコピーが存在しないことを考慮すると、無意味で危険になります。 したがって `ref readonly` 戻り値は常に直接参照です。

例:

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- `return ref` の引数は左辺値である必要があります (**既存のルール**)
- `return ref` の引数は "return to return" (既存の**ルール**) にする必要があります。
- `ref readonly` メンバーでは、`return ref` の引数は_書き込み可能である必要はありません_。
たとえば、このようなメンバーは、readonly フィールドまたはその `in` パラメーターの1つを参照して返すことができます。

### <a name="safe-to-return-rules"></a>規則を返すことが安全です。
参照用の規則を返す通常の安全な方法は、読み取り専用の参照にも当てはまります。

`ref readonly` は、通常の `ref` ローカル/パラメーター/戻り値から取得できますが、他の方法は取得できないことに注意してください。 それ以外の場合、`ref readonly` が返す安全性は、通常の `ref` が返す場合と同じ方法で推定されます。

右辺値を `in` パラメーターとして渡し、`ref readonly` として返すことを考慮すると、もう1つの規則が必要になります。これ**は、参照渡しでは安全に返すことができません**。

> 右辺値がコピーを介して `in` パラメーターに渡され、`ref readonly`の形式で返される場合を考えてみます。 呼び出し元のコンテキストでは、このような呼び出しの結果はローカルデータへの参照であるため、を返すのは安全ではありません。
> 右辺値が返されても安全でない場合は、既存のルール `#6` によって既に処理されています。

例:
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

更新された `safe to return` ルール:

1.  **ヒープ上の変数への参照は、安全に返すことができる**
2.  **ref/in パラメーターは
を確実に返すことが**できます `in` パラメーターは読み取り専用としてのみ返すことができます。
3.  **out パラメーターは**、既に使用されているように、確実に返すことができます (ただし、既に割り当てられている必要があります)。
4.  **インスタンス構造体フィールドは、受信側が安全に戻ることができる限り、安全に返すことができる**
5.  **' this ' は、構造体メンバーからは安全ではありません**
6.  **他のメソッドから返される参照は、仮パラメーターとしてそのメソッドに渡されたすべての refs/アウトが安全に返すことができた場合に、安全に返すことができます。** 具体的には、受信側*が構造体、クラス、ジェネリック型パラメーターとして型指定されているかどうかに関係なく、受信側が安全に戻ることができるかどうかは関係ありません
。*
7.  **右辺値は、参照渡しでは安全ではありません。** 
*具体的には、右辺値をパラメーターとして渡すことが安全です。*

> 注: ref のような型と参照の再割り当てが関係している場合は、戻り値の安全性に関する追加の規則があります。
> これらの規則は `ref` と `ref readonly` メンバーにも適用されるため、ここでは説明しません。

### <a name="aliasing-behavior"></a>エイリアス動作。
`ref readonly` メンバーは、通常の `ref` メンバーと同じエイリアス動作を提供します (readonly の場合を除く)。
したがって、ラムダ、非同期、反復子、stack 書き込むなどでキャプチャするために使用します。同じ制限が適用されます。 :. 実際の参照をキャプチャできないため、またメンバーの評価に副作用があるため、このようなシナリオは許可されません。

> これは許可されており、通常の書き込み可能な参照として `this` を受け取る通常の構造体メソッドの受信側で `ref readonly` が返される場合に、コピーを作成する必要があります。 以前は、このような呼び出しが読み取り専用変数に適用されるすべての場合、ローカルコピーが作成されていました。

### <a name="metadata-representation"></a>メタデータ表現。
Byref を返すメソッドの戻り値に `System.Runtime.CompilerServices.IsReadOnlyAttribute` が適用されている場合は、メソッドが読み取り専用の参照を返すことを意味します。

また、このようなメソッドの結果のシグネチャ (およびそれらのメソッドのみ) には `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`が必要です。

**動機**: `ref readonly` が返されたメソッドを呼び出すときに、既存のコンパイラが単に `readonly` を無視できないようにするためです。

## <a name="readonly-structs"></a>Readonly 構造体
Short-コンストラクターを除き、構造体のすべてのインスタンスメンバーの `this` パラメーターを作成する機能。これは、`in` パラメーターです。

### <a name="motivation"></a>目的
コンパイラは、構造体インスタンスでメソッドを呼び出すとインスタンスが変更される可能性があると想定する必要があります。 実際には、書き込み可能な参照は `this` パラメーターとしてメソッドに渡され、この動作を完全に有効にします。 `readonly` 変数に対してこのような呼び出しを許可するために、呼び出しは一時コピーに適用されます。 これはにくいになる可能性があり、場合によっては、パフォーマンス上の理由から `readonly` を破棄することがあります。  
例: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/

`in` パラメーターと `ref readonly` のサポートを追加した後は、読み取り専用の変数がより一般的になるため、防御的なコピーの問題が悪化します。

### <a name="solution"></a>解決策
コンストラクターを除くすべての構造体インスタンスメソッドで、`this` を `in` パラメーターとして処理することになる構造体宣言で `readonly` 修飾子を許可します。

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a>Readonly 構造体のメンバーに関する制限事項
- 読み取り専用の構造体のインスタンスフィールドは読み取り専用である必要があります。  
**動機:** 外部にのみ書き込むことができますが、メンバーを経由することはできません。
- 読み取り専用の構造体のインスタンス autoproperties は、get 専用である必要があります。  
**動機:** インスタンスフィールドに対する制限の結果。
- Readonly 構造体は、フィールドのようなイベントを宣言することはできません。  
**動機:** インスタンスフィールドに対する制限の結果。

### <a name="metadata-representation"></a>メタデータ表現。
`System.Runtime.CompilerServices.IsReadOnlyAttribute` が値型に適用される場合、その型が `readonly struct`であることを意味します。

特に次の点に違いがあります。
-  `IsReadOnlyAttribute` 型の id は重要ではありません。 実際には、必要に応じて、それを含んでいるアセンブリのコンパイラによって埋め込むことができます。

## <a name="refin-extension-methods"></a>`ref`/`in` 拡張メソッド
実際には、既存の提案 (https://github.com/dotnet/roslyn/issues/165) とそれに対応するプロトタイプ PR (https://github.com/dotnet/roslyn/pull/15650)があります。
このアイデアがまったく新しいものではないことを確認したいだけです。 ただし、このような方法に関する最も悪化問題が `ref readonly` によって簡単に削除されるため、ここで関連しています。 RValue レシーバーを使用した場合の対処方法について説明します。

一般的な考え方は、型が構造体型であることがわかっている限り、拡張メソッドが参照によって `this` パラメーターを取得できるようにすることです。

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

このような拡張メソッドを作成する理由は主に次のとおりです。  
1.  レシーバーが大きな構造体である場合はコピーしない
2.  構造体の拡張メソッドの変更を許可する

クラスでこれを許可しない理由  
1.  その目的は非常に限られています。
2.  メソッドの呼び出しによって、呼び出し後に`null` 以外の受信者が `null` になることはないので、長時間不変ではなくなります。
> 実際には、`ref` または `out`によって_明示的_に割り当てられていない限り、`null` ない変数を `null` にすることはできません。
> 読みやすくするため、またはその他の形式の "ここでは null にすることができます" 分析が非常に役立ちます。
3.  Null 条件付きアクセスの "評価1回のみ" セマンティクスによって調整するのは困難です。
例: `obj.stringField?.RefExtension(...)`-`stringField` のコピーをキャプチャして null チェックを意味を持つようにする必要がありますが、RefExtension 内の `this` への代入はフィールドに反映されません。

参照渡しで最初の引数を受け取る**構造体**に対して拡張メソッドを宣言する機能は、長時間要求でした。 ブロックの考慮事項の1つは、"レシーバーが左辺値ではない場合の動作" です。

- すべての拡張メソッドを静的メソッドとして呼び出すこともできます (あいまいさを解決する唯一の方法である場合もあります)。 これは、右辺値レシーバーを許可しないことを指定します。
- 一方、構造体のインスタンスメソッドが関係する場合は、同様の状況でコピーに対して呼び出しを行う方法もあります。

"暗黙的なコピー" が存在する理由は、構造体のメソッドの大部分は実際には構造体を変更せず、を示すことができないためです。 そのため、最も実用的な解決策は、コピーでの呼び出しを行うだけでしたが、この方法は、パフォーマンスの低下やバグの原因となっていることがわかっています。

現在、`in` パラメーターを使用できるようになったため、拡張機能によって意図が通知される可能性があります。 そのため、難問拡張機能が書き込み可能な受信側で呼び出されることを `ref` 要求することで、を解決できます。 `in` 拡張機能では、必要に応じて暗黙的なコピーが許可されます。

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a>`in` 拡張機能とジェネリック。
`ref` 拡張メソッドの目的は、受信側を直接変更するか、または変化するメンバーを呼び出すことです。 したがって、`T` が構造体として制約されている限り、`ref this T` の拡張機能を使用できます。

一方 `in` 拡張メソッドは、暗黙的なコピーを減らすために特に存在します。 ただし、`in T` パラメーターの使用は、インターフェイスメンバーを介して行う必要があります。 すべてのインターフェイスメンバーは変化していると見なされるため、このような使用ではコピーが必要になります。 -コピーを減らすのではなく、その逆の効果が得られます。 したがって、制約に関係なく `T` がジェネリック型パラメーターである場合、`in this T` は許可されません。

### <a name="valid-kinds-of-extension-methods-recap"></a>有効な拡張メソッドの種類 (要約):
拡張メソッドでは、次の形式の `this` 宣言が許可されるようになりました。
1) `this T arg`-通常の byval 拡張。 (**既存のケース**)
- T には、参照型または型パラメーターを含む任意の型を指定できます。
インスタンスは、呼び出しの後に同じ変数になります。
_この引数変換_の種類の暗黙的な変換を許可します。
右辺値で呼び出すことができます。

-  - `in` 拡張機能を `in this T self`します。
T は実際の構造体型である必要があります。
インスタンスは、呼び出しの後に同じ変数になります。
_この引数変換_の種類の暗黙的な変換を許可します。
右辺値で呼び出すことができます (必要に応じて、一時で呼び出すことができます)。

-  - `ref` 拡張機能を `ref this T self`します。
T は構造体型であるか、または構造体として制約されるジェネリック型パラメーターである必要があります。
インスタンスは、呼び出しによって書き込まれる場合があります。
Id 変換のみを許可します。
書き込み可能な左辺値で呼び出す必要があります。 (temp 経由では呼び出されません)。

## <a name="readonly-ref-locals"></a>Readonly ref ローカル変数。

### <a name="motivation"></a>動機.
`ref readonly` メンバーが導入されると、適切な種類のローカルとペアにする必要があることを明確にしました。 メンバーを評価すると副作用が生じる可能性があります。したがって、結果を複数回使用する必要がある場合は、格納する必要があります。 通常 `ref` ローカルは、`readonly` 参照を割り当てることができないため、ここでは役に立ちません。   

### <a name="solution"></a>Solution.
`ref readonly` ローカルの宣言を許可します。 これは、書き込み可能でない新しい種類の `ref` ローカルです。 その結果、ローカル `ref readonly` は、これらの変数を書き込みに公開せずに、読み取り専用変数への参照を受け入れることができます。

### <a name="declaring-and-using-ref-readonly-locals"></a>`ref readonly` ローカルの宣言と使用

このようなローカルの構文では、宣言サイトで (特定の順序で) `ref readonly` 修飾子を使用します。 通常の `ref` ローカルの場合と同様に、`ref readonly` のローカル変数は宣言時に ref 初期化される必要があります。 通常の `ref` ローカルの場合とは異なり、`ref readonly` ローカルは `in` パラメーター、`readonly` フィールド、`ref readonly` メソッドなどの `readonly` LValues を参照できます。

すべての目的において、`ref readonly` ローカルは `readonly` 変数として扱われます。 使用に関する制限事項のほとんどは、`readonly` フィールドまたは `in` パラメーターと同じです。

たとえば、構造体型を持つ `in` パラメーターのフィールドは、すべて `readonly` 変数として再帰的に分類されます。   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a>`ref readonly` ローカルの使用に関する制限事項
`readonly` の性質を除き、`ref readonly` ローカルは通常の `ref` ローカルと同様に動作し、まったく同じ制限が適用されます。  
クロージャでのキャプチャに関連する制限の例として、`async` メソッドでの宣言または `safe-to-return` 分析は `ref readonly` ローカルにも同様に適用されます。

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a>三項 `ref` 式。 ("条件付き左辺値" とも呼ばれる)

### <a name="motivation"></a>目的
`ref` と `ref readonly` の使用は、条件に基づいて1つまたは別のターゲット変数を使用して、ローカルの参照を ref に初期化する必要があります。

一般的な回避策は、次のようなメソッドを導入することです。

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

`Choice` は、_すべて_の引数が呼び出しサイトで評価される必要があるため、にくいの動作とバグにつながるため、三項が完全に置換されるわけではないことに注意してください。

次のものは期待どおりに機能しません。

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a>解決策
条件に基づいて左辺値の引数のいずれかへの参照に評価される特殊な種類の条件式を許可します。

### <a name="using-ref-ternary-expression"></a>`ref` 三項式を使用します。

条件式の `ref` フレーバーの構文は `<condition> ? ref <consequence> : ref <alternative>;`

通常の条件式の場合と同様に、ブール条件式の結果に応じて `<alternative>` が評価されるのは、`<consequence>` の場合のみです。

通常の条件式とは異なり、`ref` 条件式:
- `<consequence>` と `<alternative>` が左辺値である必要があります。
- `ref` 条件式自体は左辺値であり、
- `<consequence>` と `<alternative>` の両方が書き込み可能な値である場合 `ref` 条件式は書き込み可能です

例 :  
`ref` 三項は左辺値であるため、参照渡しで渡すことができます。
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

左辺値である場合は、に割り当てることもできます。
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

は、メソッド呼び出しの受信者として使用でき、必要に応じてコピーをスキップします。
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

`ref` 三項は、通常の (非 ref) コンテキストでも使用できます。
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

参照と読み取り専用参照の拡張サポートに対する2つの主な引数を確認できます。

1) ここで解決される問題は、非常に古いものです。 これは、特に既存のコードが役に立ちません。

新しいドメインでC#使用されていると .net では、いくつかの問題が目立つようになります。  
計算オーバーヘッドに関する平均よりも重要な環境の例としては、

* 計算が請求され、応答性が高いクラウド/データセンターのシナリオは、競争上の利点です。
* 待機時間に関するソフトリアルタイムの要件を持つゲーム/VR/AR     

この機能では、タイプセーフなどの既存の長所を一切犠牲にすることはできませんが、一般的なシナリオではオーバーヘッドを低く抑えることができます。

2) `readonly` コントラクトを使用すると、呼び出し先がルールによって再生されることを合理的に保証できますか。

`out`を使用する場合も同様の信頼があります。 `out` の実装が正しくないと、特定できない動作が発生することがありますが、実際にはほとんど発生しません。  

`ref readonly` に習熟している正式な検証規則を作成すると、信頼の問題がさらに軽減されます。

### <a name="alternatives"></a>代替
[alternatives]: #alternatives

競合の主な設計は、実際には "do nothing" です。

### <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a>会議のデザイン

https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md
