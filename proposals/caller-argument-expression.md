---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483518"
---
# <a name="callerargumentexpression"></a>CallerArgumentExpression

* [x] が提案されています
* [] プロトタイプ: 開始されていません
* [] の実装: 開始されていません
* [] 仕様: 開始されていません

## <a name="summary"></a>まとめ
[summary]: #summary

開発者がメソッドに渡された式をキャプチャして、診断/テスト Api でより適切なエラーメッセージを有効にし、キーストロークを減らすことができるようにします。

## <a name="motivation"></a>目的
[motivation]: #motivation

アサーションまたは引数の検証が失敗した場合、開発者は、失敗した場所と理由についてできるだけ多くのことを知りたいと考えています。 ただし、現在の診断 Api では、これを完全には実現できません。 次の方法を考えてみましょう。

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

アサートのいずれかが失敗した場合は、ファイル名、行番号、およびメソッド名のみがスタックトレースに提供されます。 開発者は、この情報から失敗したアサートを見分けることができません。ファイルを開き、指定された行番号に移動して問題が発生していることを確認する必要があります。

これは、テストフレームワークがさまざまな assert メソッドを提供する必要がある理由でもあります。 XUnit では、`Assert.True` と `Assert.False` は、失敗した内容に関する十分なコンテキストを提供しないため、頻繁には使用されません。

無効な引数の名前は開発者に表示されるので、引数の検証には少しの状況が適していますが、開発者はこれらの名前を例外に手動で渡す必要があります。 上記の例が `Debug.Assert`ではなく従来の引数の検証を使用するように書き換えられた場合は、次のようになります。

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

`nameof(array)` は各例外に渡す必要があることに注意してください。ただし、どの引数が無効であるかは既に明確であることに注意してください。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

上記の例では、assert メッセージに文字列 `"array != null"` や `"array.Length == 1"` を含めて、失敗したものを開発者が判断するのに役立ちます。 `CallerArgumentExpression`を入力します。フレームワークが特定のメソッド引数に関連付けられている文字列を取得するために使用できる属性です。 次のように `Debug.Assert` に追加します。

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

上記の例のソースコードは変わりません。 ただし、コンパイラによって実際に生成されるコードは、

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

コンパイラは、`Debug.Assert`の属性を特別に認識します。 このメソッドは、呼び出しサイトで属性のコンストラクター (この場合は `condition`) で参照される引数に関連付けられている文字列を渡します。 いずれかのアサートが失敗した場合、開発者には、false だった条件が表示され、失敗したものがわかるようになります。

引数の検証では、属性を直接使用することはできませんが、ヘルパークラスを使用してを使用することができます。

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

このようなヘルパークラスをフレームワークに追加するための提案は、 https://github.com/dotnet/corefx/issues/17068で進行中です。 この言語機能が実装されている場合は、この機能を利用するために提案を更新することができます。

### <a name="extension-methods"></a>拡張メソッド

拡張メソッドの `this` パラメーターは、`CallerArgumentExpression`によって参照される場合があります。 次に例を示します。

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

`thisExpression` は、ドットの前にあるオブジェクトに対応する式を受け取ります。 `Ext.ShouldBe(contestant.Points, 1337)`などの静的メソッド構文で呼び出された場合、最初のパラメーターが `this`としてマークされていないかのように動作します。

常に、`this` パラメーターに対応する式を指定する必要があります。 クラスのインスタンスがそれ自体の拡張メソッドを呼び出す場合 (コレクション型の内部から `this.Single()` 場合でも、`this` はコンパイラによって義務付けられるので、`"this"` が渡されます。 将来このルールが変更された場合は、`null` または空の文字列を渡すことを検討できます。

### <a name="extra-details"></a>追加の詳細

- `CallerMemberName`などの他の `Caller*` 属性と同様に、この属性は既定値を持つパラメーターでのみ使用できます。
- 上に示すように、`CallerArgumentExpression` でマークされた複数のパラメーターを使用できます。
- 属性の名前空間が `System.Runtime.CompilerServices`されます。
- `null` またはパラメーター名以外の文字列 (`"notAParameterName"`など) が指定されている場合、コンパイラは空の文字列を渡します。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

- Decompilers の使用方法を知っている人は、この属性でマークされたメソッドの呼び出しサイトでソースコードの一部を確認できます。 これは、クローズドソースソフトウェアで望ましくないか、予期しないものである可能性があります。

- これは機能自体の欠陥ではありませんが、問題の原因として、現時点では、`bool`のみを取得する `Debug.Assert` API が存在している可能性があります。 メッセージを受け取るオーバーロードに、この属性でマークされた2番目のパラメーターがあり、省略可能に設定されている場合でも、コンパイラはオーバーロードの解決でメッセージなしのを取得します。 このため、この機能を利用するには、メッセージを使用しないオーバーロードを削除する必要があります。これはバイナリ (ソースではありませんが) 互換性に影響する変更です。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

- この属性を使用するメソッドの呼び出しサイトでソースコードを確認できる場合は、属性の効果をオプトインすることができます。 開発者は、`AssemblyInfo.cs`に格納されているアセンブリ全体の `[assembly: EnableCallerArgumentExpression]` 属性を使用してこれを有効にします。
  - 属性の効果が有効になっていない場合、属性でマークされたメソッドの呼び出しはエラーにはなりません。これにより、既存のメソッドで属性を使用し、ソース互換性を維持することができます。 ただし、属性は無視され、指定された既定値を使用してメソッドが呼び出されます。

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- `Debug.Assert`に新しい呼び出し元情報を追加するたびに[バイナリ互換性の問題][ drawbacks]が発生しないようにするために、別の解決策として、呼び出し元に関する必要なすべての情報を格納する `CallerInfo` 構造体をフレームワークに追加する方法があります。

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

これはもともと https://github.com/dotnet/csharplang/issues/87で提案されています。

このアプローチにはいくつかの欠点があります。

- 必要なプロパティを指定できるようにすることで、従量課金制としても、アサートが成功した場合でも、式の配列を割り当てたり、`MethodBase.GetCurrentMethod` を呼び出したりすることによって、パフォーマンスが大幅に低下する可能性があります。

- さらに、`CallerInfo` 属性に新しいフラグを渡すことは、互換性に影響する変更ではありませんが、以前のバージョンのメソッドに対してコンパイルされた呼び出しサイトから、その新しいパラメーターを実際に受け取ることが `Debug.Assert` 保証されることはありません。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

TBD

## <a name="design-meetings"></a>会議のデザイン

該当なし
