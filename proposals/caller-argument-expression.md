---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483518"
---
# <a name="callerargumentexpression"></a><span data-ttu-id="b138c-101">CallerArgumentExpression</span><span class="sxs-lookup"><span data-stu-id="b138c-101">CallerArgumentExpression</span></span>

* <span data-ttu-id="b138c-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="b138c-102">[x] Proposed</span></span>
* <span data-ttu-id="b138c-103">[] プロトタイプ: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="b138c-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="b138c-104">[] の実装: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="b138c-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="b138c-105">[] 仕様: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="b138c-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="b138c-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="b138c-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b138c-107">開発者がメソッドに渡された式をキャプチャして、診断/テスト Api でより適切なエラーメッセージを有効にし、キーストロークを減らすことができるようにします。</span><span class="sxs-lookup"><span data-stu-id="b138c-107">Allow developers to capture the expressions passed to a method, to enable better error messages in diagnostic/testing APIs and reduce keystrokes.</span></span>

## <a name="motivation"></a><span data-ttu-id="b138c-108">目的</span><span class="sxs-lookup"><span data-stu-id="b138c-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b138c-109">アサーションまたは引数の検証が失敗した場合、開発者は、失敗した場所と理由についてできるだけ多くのことを知りたいと考えています。</span><span class="sxs-lookup"><span data-stu-id="b138c-109">When an assertion or argument validation fails, the developer wants to know as much as possible about where and why it failed.</span></span> <span data-ttu-id="b138c-110">ただし、現在の診断 Api では、これを完全には実現できません。</span><span class="sxs-lookup"><span data-stu-id="b138c-110">However, today's diagnostic APIs do not fully facilitate this.</span></span> <span data-ttu-id="b138c-111">次の方法を考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="b138c-111">Consider the following method:</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

<span data-ttu-id="b138c-112">アサートのいずれかが失敗した場合は、ファイル名、行番号、およびメソッド名のみがスタックトレースに提供されます。</span><span class="sxs-lookup"><span data-stu-id="b138c-112">When one of the asserts fail, only the filename, line number, and method name will be provided in the stack trace.</span></span> <span data-ttu-id="b138c-113">開発者は、この情報から失敗したアサートを見分けることができません。ファイルを開き、指定された行番号に移動して問題が発生していることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b138c-113">The developer will not be able to tell which assert failed from this information-- (s)he will have to open the file and navigate to the provided line number to see what went wrong.</span></span>

<span data-ttu-id="b138c-114">これは、テストフレームワークがさまざまな assert メソッドを提供する必要がある理由でもあります。</span><span class="sxs-lookup"><span data-stu-id="b138c-114">This is also the reason testing frameworks have to provide a variety of assert methods.</span></span> <span data-ttu-id="b138c-115">XUnit では、`Assert.True` と `Assert.False` は、失敗した内容に関する十分なコンテキストを提供しないため、頻繁には使用されません。</span><span class="sxs-lookup"><span data-stu-id="b138c-115">With xUnit, `Assert.True` and `Assert.False` are not frequently used because they do not provide enough context about what failed.</span></span>

<span data-ttu-id="b138c-116">無効な引数の名前は開発者に表示されるので、引数の検証には少しの状況が適していますが、開発者はこれらの名前を例外に手動で渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="b138c-116">While the situation is a bit better for argument validation because the names of invalid arguments are shown to the developer, the developer must pass these names to exceptions manually.</span></span> <span data-ttu-id="b138c-117">上記の例が `Debug.Assert`ではなく従来の引数の検証を使用するように書き換えられた場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b138c-117">If the above example were rewritten to use traditional argument validation instead of `Debug.Assert`, it would look like</span></span>

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

<span data-ttu-id="b138c-118">`nameof(array)` は各例外に渡す必要があることに注意してください。ただし、どの引数が無効であるかは既に明確であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b138c-118">Notice that `nameof(array)` must be passed to each exception, although it's already clear from context which argument is invalid.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b138c-119">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="b138c-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b138c-120">上記の例では、assert メッセージに文字列 `"array != null"` や `"array.Length == 1"` を含めて、失敗したものを開発者が判断するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="b138c-120">In the above examples, including the string `"array != null"` or `"array.Length == 1"` in the assert message would help the developer determine what failed.</span></span> <span data-ttu-id="b138c-121">`CallerArgumentExpression`を入力します。フレームワークが特定のメソッド引数に関連付けられている文字列を取得するために使用できる属性です。</span><span class="sxs-lookup"><span data-stu-id="b138c-121">Enter `CallerArgumentExpression`: it's an attribute the framework can use to obtain the string associated with a particular method argument.</span></span> <span data-ttu-id="b138c-122">次のように `Debug.Assert` に追加します。</span><span class="sxs-lookup"><span data-stu-id="b138c-122">We would add it to `Debug.Assert` like so</span></span>

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

<span data-ttu-id="b138c-123">上記の例のソースコードは変わりません。</span><span class="sxs-lookup"><span data-stu-id="b138c-123">The source code in the above example would stay the same.</span></span> <span data-ttu-id="b138c-124">ただし、コンパイラによって実際に生成されるコードは、</span><span class="sxs-lookup"><span data-stu-id="b138c-124">However, the code the compiler actually emits would correspond to</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

<span data-ttu-id="b138c-125">コンパイラは、`Debug.Assert`の属性を特別に認識します。</span><span class="sxs-lookup"><span data-stu-id="b138c-125">The compiler specially recognizes the attribute on `Debug.Assert`.</span></span> <span data-ttu-id="b138c-126">このメソッドは、呼び出しサイトで属性のコンストラクター (この場合は `condition`) で参照される引数に関連付けられている文字列を渡します。</span><span class="sxs-lookup"><span data-stu-id="b138c-126">It passes the string associated with the argument referred to in the attribute's constructor (in this case, `condition`) at the call site.</span></span> <span data-ttu-id="b138c-127">いずれかのアサートが失敗した場合、開発者には、false だった条件が表示され、失敗したものがわかるようになります。</span><span class="sxs-lookup"><span data-stu-id="b138c-127">When either assert fails, the developer will be shown the condition that was false and will know which one failed.</span></span>

<span data-ttu-id="b138c-128">引数の検証では、属性を直接使用することはできませんが、ヘルパークラスを使用してを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="b138c-128">For argument validation, the attribute cannot be used directly, but can be made use of through a helper class:</span></span>

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

<span data-ttu-id="b138c-129">このようなヘルパークラスをフレームワークに追加するための提案は、 https://github.com/dotnet/corefx/issues/17068で進行中です。</span><span class="sxs-lookup"><span data-stu-id="b138c-129">A proposal to add such a helper class to the framework is underway at https://github.com/dotnet/corefx/issues/17068.</span></span> <span data-ttu-id="b138c-130">この言語機能が実装されている場合は、この機能を利用するために提案を更新することができます。</span><span class="sxs-lookup"><span data-stu-id="b138c-130">If this language feature was implemented, the proposal could be updated to take advantage of this feature.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="b138c-131">拡張メソッド</span><span class="sxs-lookup"><span data-stu-id="b138c-131">Extension methods</span></span>

<span data-ttu-id="b138c-132">拡張メソッドの `this` パラメーターは、`CallerArgumentExpression`によって参照される場合があります。</span><span class="sxs-lookup"><span data-stu-id="b138c-132">The `this` parameter in an extension method may be referenced by `CallerArgumentExpression`.</span></span> <span data-ttu-id="b138c-133">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="b138c-133">For example:</span></span>

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

<span data-ttu-id="b138c-134">`thisExpression` は、ドットの前にあるオブジェクトに対応する式を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="b138c-134">`thisExpression` will receive the expression corresponding to the object before the dot.</span></span> <span data-ttu-id="b138c-135">`Ext.ShouldBe(contestant.Points, 1337)`などの静的メソッド構文で呼び出された場合、最初のパラメーターが `this`としてマークされていないかのように動作します。</span><span class="sxs-lookup"><span data-stu-id="b138c-135">If it's called with static method syntax, e.g. `Ext.ShouldBe(contestant.Points, 1337)`, it will behave as if first parameter wasn't marked `this`.</span></span>

<span data-ttu-id="b138c-136">常に、`this` パラメーターに対応する式を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b138c-136">There should always be an expression corresponding to the `this` parameter.</span></span> <span data-ttu-id="b138c-137">クラスのインスタンスがそれ自体の拡張メソッドを呼び出す場合 (コレクション型の内部から `this.Single()` 場合でも、`this` はコンパイラによって義務付けられるので、`"this"` が渡されます。</span><span class="sxs-lookup"><span data-stu-id="b138c-137">Even if an instance of a class calls an extension method on itself, e.g. `this.Single()` from inside a collection type, the `this` is mandated by the compiler so `"this"` will get passed.</span></span> <span data-ttu-id="b138c-138">将来このルールが変更された場合は、`null` または空の文字列を渡すことを検討できます。</span><span class="sxs-lookup"><span data-stu-id="b138c-138">If this rule is changed in the future, we can consider passing `null` or the empty string.</span></span>

### <a name="extra-details"></a><span data-ttu-id="b138c-139">追加の詳細</span><span class="sxs-lookup"><span data-stu-id="b138c-139">Extra details</span></span>

- <span data-ttu-id="b138c-140">`CallerMemberName`などの他の `Caller*` 属性と同様に、この属性は既定値を持つパラメーターでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="b138c-140">Like the other `Caller*` attributes, such as `CallerMemberName`, this attribute may only be used on parameters with default values.</span></span>
- <span data-ttu-id="b138c-141">上に示すように、`CallerArgumentExpression` でマークされた複数のパラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b138c-141">Multiple parameters marked with `CallerArgumentExpression` are permitted, as shown above.</span></span>
- <span data-ttu-id="b138c-142">属性の名前空間が `System.Runtime.CompilerServices`されます。</span><span class="sxs-lookup"><span data-stu-id="b138c-142">The attribute's namespace will be `System.Runtime.CompilerServices`.</span></span>
- <span data-ttu-id="b138c-143">`null` またはパラメーター名以外の文字列 (`"notAParameterName"`など) が指定されている場合、コンパイラは空の文字列を渡します。</span><span class="sxs-lookup"><span data-stu-id="b138c-143">If `null` or a string that is not a parameter name (e.g. `"notAParameterName"`) is provided, the compiler will pass in an empty string.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="b138c-144">短所</span><span class="sxs-lookup"><span data-stu-id="b138c-144">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="b138c-145">Decompilers の使用方法を知っている人は、この属性でマークされたメソッドの呼び出しサイトでソースコードの一部を確認できます。</span><span class="sxs-lookup"><span data-stu-id="b138c-145">People who know how to use decompilers will be able to see some of the source code at call sites for methods marked with this attribute.</span></span> <span data-ttu-id="b138c-146">これは、クローズドソースソフトウェアで望ましくないか、予期しないものである可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b138c-146">This may be undesirable/unexpected for closed-source software.</span></span>

- <span data-ttu-id="b138c-147">これは機能自体の欠陥ではありませんが、問題の原因として、現時点では、`bool`のみを取得する `Debug.Assert` API が存在している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b138c-147">Although this is not a flaw in the feature itself, a source of concern may be that there exists a `Debug.Assert` API today that only takes a `bool`.</span></span> <span data-ttu-id="b138c-148">メッセージを受け取るオーバーロードに、この属性でマークされた2番目のパラメーターがあり、省略可能に設定されている場合でも、コンパイラはオーバーロードの解決でメッセージなしのを取得します。</span><span class="sxs-lookup"><span data-stu-id="b138c-148">Even if the overload taking a message had its second parameter marked with this attribute and made optional, the compiler would still pick the no-message one in overload resolution.</span></span> <span data-ttu-id="b138c-149">このため、この機能を利用するには、メッセージを使用しないオーバーロードを削除する必要があります。これはバイナリ (ソースではありませんが) 互換性に影響する変更です。</span><span class="sxs-lookup"><span data-stu-id="b138c-149">Therefore, the no-message overload would have to be removed to take advantage of this feature, which would be a binary (although not source) breaking change.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b138c-150">代替</span><span class="sxs-lookup"><span data-stu-id="b138c-150">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="b138c-151">この属性を使用するメソッドの呼び出しサイトでソースコードを確認できる場合は、属性の効果をオプトインすることができます。</span><span class="sxs-lookup"><span data-stu-id="b138c-151">If being able to see source code at call sites for methods that use this attribute proves to be a problem, we can make the attribute's effects opt-in.</span></span> <span data-ttu-id="b138c-152">開発者は、`AssemblyInfo.cs`に格納されているアセンブリ全体の `[assembly: EnableCallerArgumentExpression]` 属性を使用してこれを有効にします。</span><span class="sxs-lookup"><span data-stu-id="b138c-152">Developers will enable it through an assembly-wide `[assembly: EnableCallerArgumentExpression]` attribute they put in `AssemblyInfo.cs`.</span></span>
  - <span data-ttu-id="b138c-153">属性の効果が有効になっていない場合、属性でマークされたメソッドの呼び出しはエラーにはなりません。これにより、既存のメソッドで属性を使用し、ソース互換性を維持することができます。</span><span class="sxs-lookup"><span data-stu-id="b138c-153">In the case the attribute's effects are not enabled, calling methods marked with the attribute would not be an error, to allow existing methods to use the attribute and maintain source compatibility.</span></span> <span data-ttu-id="b138c-154">ただし、属性は無視され、指定された既定値を使用してメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b138c-154">However, the attribute would be ignored and the method would be called with whatever default value was provided.</span></span>

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

- <span data-ttu-id="b138c-155">`Debug.Assert`に新しい呼び出し元情報を追加するたびに[バイナリ互換性の問題][ drawbacks]が発生しないようにするために、別の解決策として、呼び出し元に関する必要なすべての情報を格納する `CallerInfo` 構造体をフレームワークに追加する方法があります。</span><span class="sxs-lookup"><span data-stu-id="b138c-155">To prevent the [binary compatibility problem][drawbacks] from occurring every time we want to add new caller info to `Debug.Assert`, an alternative solution would be to add a `CallerInfo` struct to the framework that contains all the necessary information about the caller.</span></span>

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

<span data-ttu-id="b138c-156">これはもともと https://github.com/dotnet/csharplang/issues/87で提案されています。</span><span class="sxs-lookup"><span data-stu-id="b138c-156">This was originally proposed at https://github.com/dotnet/csharplang/issues/87.</span></span>

<span data-ttu-id="b138c-157">このアプローチにはいくつかの欠点があります。</span><span class="sxs-lookup"><span data-stu-id="b138c-157">There are a few disadvantages of this approach:</span></span>

- <span data-ttu-id="b138c-158">必要なプロパティを指定できるようにすることで、従量課金制としても、アサートが成功した場合でも、式の配列を割り当てたり、`MethodBase.GetCurrentMethod` を呼び出したりすることによって、パフォーマンスが大幅に低下する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b138c-158">Despite being pay-for-play friendly by allowing you to specify which properties you need, it could still hurt perf significantly by allocating an array for the expressions/calling `MethodBase.GetCurrentMethod` even when the assert passes.</span></span>

- <span data-ttu-id="b138c-159">さらに、`CallerInfo` 属性に新しいフラグを渡すことは、互換性に影響する変更ではありませんが、以前のバージョンのメソッドに対してコンパイルされた呼び出しサイトから、その新しいパラメーターを実際に受け取ることが `Debug.Assert` 保証されることはありません。</span><span class="sxs-lookup"><span data-stu-id="b138c-159">Additionally, while passing a new flag to the `CallerInfo` attribute won't be a breaking change, `Debug.Assert` won't be guaranteed to actually receive that new parameter from call sites that compiled against an old version of the method.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="b138c-160">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="b138c-160">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="b138c-161">TBD</span><span class="sxs-lookup"><span data-stu-id="b138c-161">TBD</span></span>

## <a name="design-meetings"></a><span data-ttu-id="b138c-162">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="b138c-162">Design meetings</span></span>

<span data-ttu-id="b138c-163">該当なし</span><span class="sxs-lookup"><span data-stu-id="b138c-163">N/A</span></span>
