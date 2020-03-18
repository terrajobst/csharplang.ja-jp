---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "79483938"
---
# <a name="efficient-params-and-string-formatting"></a><span data-ttu-id="f922f-101">効率的な Params と文字列の書式設定</span><span class="sxs-lookup"><span data-stu-id="f922f-101">Efficient Params and String Formatting</span></span>

## <a name="summary"></a><span data-ttu-id="f922f-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="f922f-102">Summary</span></span>
<span data-ttu-id="f922f-103">この機能の組み合わせにより、`string` の値を書式設定したり `params` スタイル引数を渡したりする効率が向上します。</span><span class="sxs-lookup"><span data-stu-id="f922f-103">This combination of features will increase the efficiency of formatting `string` values and passing of `params` style arguments.</span></span>

## <a name="motivation"></a><span data-ttu-id="f922f-104">目的</span><span class="sxs-lookup"><span data-stu-id="f922f-104">Motivation</span></span>
<span data-ttu-id="f922f-105">`string` 値の書式設定による割り当てのオーバーヘッドによって、テキストベースのアプリケーションのパフォーマンスが低下することがあります。たとえば、`struct` 型のボックス化の代償、`params` の `object[]` 割り当て、`string` 呼び出し中の中間 `string.Format` 割り当てなどです。</span><span class="sxs-lookup"><span data-stu-id="f922f-105">The allocation overhead of formatting `string` values can dominate the performance of many text based applications: from the boxing penalty of `struct` types, the `object[]` allocation for `params` and the intermediate `string` allocations during `string.Format` calls.</span></span> <span data-ttu-id="f922f-106">このようなアプリケーションの効率を維持するために、多くの場合、`params` や `string` の補間などの生産性機能を破棄し、標準ではないコード化されたソリューションに移行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-106">In order to maintain efficiency such applications often need to abandon productivity features such as `params` and `string` interpolation and move to non-standard, hand coded solutions.</span></span> 

<span data-ttu-id="f922f-107">例として MSBuild を考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="f922f-107">Consider MSBuild as an example.</span></span> <span data-ttu-id="f922f-108">これは、パフォーマンスを意識する開発C#者によって、多数の最新機能を使用して作成されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-108">This is written using a lot of modern C# features by developers who are conscious of performance.</span></span> <span data-ttu-id="f922f-109">ただし、1つの代表的なビルドサンプルでは、最小の詳細度を使用して、26 MB の `string` 割り当てが生成されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-109">Yet in one representative build sample MSBuild will generate 262MB of `string` allocation using minimal verbosity.</span></span> <span data-ttu-id="f922f-110">この1/2 の割り当ては、`string.Format`内の短時間の割り当てです。</span><span class="sxs-lookup"><span data-stu-id="f922f-110">Of that 1/2 of the allocations are short lived allocations inside `string.Format`.</span></span> <span data-ttu-id="f922f-111">これらの機能により、.NET デスクトップでの多くが削除され、.NET Core では、が利用可能になったため、ほぼゼロになります `Span<T>`</span><span class="sxs-lookup"><span data-stu-id="f922f-111">These features would remove much of that on .NET Desktop and get it down to nearly zero on .NET Core due to the availability of `Span<T>`</span></span>

<span data-ttu-id="f922f-112">ここで説明されている言語機能のセットを使用すると、アプリケーションコードベースを大幅に変更することなく、これらの機能を使用し続けることができ、ほとんどの場合に意図しない割り当てオーバーヘッドが解消されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-112">The set of language features described here will enable applications to continue using these features, with very little or no churn to their application code base, while removing the unintended allocation overhead in the majority of cases.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f922f-113">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="f922f-113">Detailed Design</span></span> 
<span data-ttu-id="f922f-114">ここでは、次のような結果を得るために使用される一連の機能があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-114">There are a set of features that will be used here to achieve these results:</span></span>

- <span data-ttu-id="f922f-115">`params` を展開して、より広範なコレクション型のセットをサポートします。</span><span class="sxs-lookup"><span data-stu-id="f922f-115">Expanding `params` to support a broader set of collection types.</span></span>
- <span data-ttu-id="f922f-116">開発者が `string` 補間を実現する方法をカスタマイズできるようにします。</span><span class="sxs-lookup"><span data-stu-id="f922f-116">Allowing for developers to customize how `string` interpolation is achieved.</span></span> 
- <span data-ttu-id="f922f-117">`string` を補間することで、より効率的な `string.Format` オーバーロードにバインドできます。</span><span class="sxs-lookup"><span data-stu-id="f922f-117">Allowing for interpolated `string` to bind to more efficient `string.Format` overloads.</span></span>

### <a name="extending-params"></a><span data-ttu-id="f922f-118">パラメーターの拡張</span><span class="sxs-lookup"><span data-stu-id="f922f-118">Extending params</span></span>
<span data-ttu-id="f922f-119">この言語では、メソッドシグネチャ内の `params` が `Span<T>`、`ReadOnlySpan<T>`、および `IEnumerable<T>`型を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="f922f-119">The language will allow for `params` in a method signature to have the types `Span<T>`, `ReadOnlySpan<T>` and `IEnumerable<T>`.</span></span> <span data-ttu-id="f922f-120">`params T[]`に適用されるこれらの新しい型には、呼び出しの場合と同じ規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-120">The same rules for invocation will apply to these new types that apply to `params T[]`:</span></span>

- <span data-ttu-id="f922f-121">唯一の違いが `params` キーワードの場合は、オーバーロードできません。</span><span class="sxs-lookup"><span data-stu-id="f922f-121">Can't overload where the only difference is a `params` keyword.</span></span>
- <span data-ttu-id="f922f-122">は `T` に暗黙的に変換できる一連の引数を渡すことによってを呼び出すことができます。また、1つの `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` 引数に渡すこともできます。</span><span class="sxs-lookup"><span data-stu-id="f922f-122">Can invoke by passing a series of arguments that are implicitly convertible to `T` or a single `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` argument.</span></span>
- <span data-ttu-id="f922f-123">は、メソッドシグネチャの最後のパラメーターである必要があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-123">Must be the last parameter in a method signature.</span></span>
- <span data-ttu-id="f922f-124">その他...</span><span class="sxs-lookup"><span data-stu-id="f922f-124">Etc ...</span></span> 

<span data-ttu-id="f922f-125">`Span<T>` と `ReadOnlySpan<T>` のバリアントは、わかりやすくするために以下の `Span<T>` と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="f922f-125">The `Span<T>` and `ReadOnlySpan<T>` variants will be referred to as `Span<T>` below for simplicity.</span></span> <span data-ttu-id="f922f-126">`ReadOnlySpan<T>` の動作が異なる場合は、明示的に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-126">In cases where the behavior of `ReadOnlySpan<T>` differs it will be explicitly called out.</span></span> 

<span data-ttu-id="f922f-127">`params` の `Span<T>` バリアントの利点は、`Span<T>` 値のバッキングストレージを割り当てる方法がコンパイラによって非常に柔軟になることです。</span><span class="sxs-lookup"><span data-stu-id="f922f-127">The advantage the `Span<T>` variants of `params` provides is it gives the compiler great flexibility in how it allocates the backing storage for the `Span<T>` value.</span></span> <span data-ttu-id="f922f-128">`params T[]` を使用すると、コンパイラは `params` メソッドを呼び出すたびに新しい `T[]` を割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-128">With a `params T[]` the compiler must allocate a new `T[]` for every invocation of a `params` method.</span></span> <span data-ttu-id="f922f-129">再使用はできません。これは、呼び出し先が格納され、パラメーターを再利用する必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="f922f-129">Re-use is not possible because it must assume the callee stored and reused the parameter.</span></span> <span data-ttu-id="f922f-130">これにより、多数の `params` 呼び出しがあるメソッドで、大きな非効率性が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-130">This can lead to a large inefficiency in methods with lots of `params` invocations.</span></span>

<span data-ttu-id="f922f-131">指定された `Span<T>` バリアントは `ref struct` 呼び出し先が引数を格納できません。</span><span class="sxs-lookup"><span data-stu-id="f922f-131">Given `Span<T>` variants are `ref struct` the callee cannot store the argument.</span></span> <span data-ttu-id="f922f-132">そのため、コンパイラは、引数の再利用などのアクションを実行して、呼び出しサイトを最適化できます。</span><span class="sxs-lookup"><span data-stu-id="f922f-132">Hence the compiler can optimize the call sites by taking actions like re-using the argument.</span></span> <span data-ttu-id="f922f-133">これにより、`T[]`の場合と比べて、呼び出しが非常に効率的になります。</span><span class="sxs-lookup"><span data-stu-id="f922f-133">This can make repeated invocations very efficient as compared to `T[]`.</span></span> <span data-ttu-id="f922f-134">ただし、この言語では、このような callsite がどのように最適化されるかについて、特定の保証はありません。</span><span class="sxs-lookup"><span data-stu-id="f922f-134">The language though will make no specific guarantees about how such callsites are optimized.</span></span> <span data-ttu-id="f922f-135">`params Span<T>` メソッドを呼び出すときに、コンパイラが `T[]` 以外の値を自由に使用できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f922f-135">Only note that the compiler is free to use values other than `T[]` when invoking a `params Span<T>` method.</span></span> 

<span data-ttu-id="f922f-136">このような考えられる実装の1つは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f922f-136">One such potential implementation is the following.</span></span> <span data-ttu-id="f922f-137">メソッド本体ですべての `params` 呼び出しを検討します。</span><span class="sxs-lookup"><span data-stu-id="f922f-137">Consider all `params` invocation in a method body.</span></span> <span data-ttu-id="f922f-138">コンパイラは、最大の `params` 呼び出しと同じサイズの配列を割り当てることができ、配列上で適切にサイズ設定された `Span<T>` インスタンスを作成することによって、すべての呼び出しでそれを使用します。</span><span class="sxs-lookup"><span data-stu-id="f922f-138">The compiler could allocate an array which has a size equal to the largest `params` invocation and use that for all of the invocations by creating appropriately sized `Span<T>` instances over the array.</span></span> <span data-ttu-id="f922f-139">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f922f-139">For example:</span></span>

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

<span data-ttu-id="f922f-140">コンパイラは、次のように `Go` の本体を出力するように選択できます。</span><span class="sxs-lookup"><span data-stu-id="f922f-140">The compiler could choose to emit the body of `Go` as follows:</span></span>

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

<span data-ttu-id="f922f-141">これにより、アプリケーションで割り当てられた配列の数を大幅に減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="f922f-141">This can significantly reduce the number of arrays allocated in an application.</span></span> <span data-ttu-id="f922f-142">ランタイムが配列のよりスマートなスタック割り当てを行うためのユーティリティを提供する場合、割り当てをさらに小さくすることができます。</span><span class="sxs-lookup"><span data-stu-id="f922f-142">Allocations can be even further reduced if the runtime provides utilities for smarter stack allocation of arrays.</span></span>

<span data-ttu-id="f922f-143">ただし、この最適化は常に適用できません。</span><span class="sxs-lookup"><span data-stu-id="f922f-143">This optimization cannot always be applied though.</span></span> <span data-ttu-id="f922f-144">呼び出し先は `params` 引数をキャプチャできませんが、`ref` またはそれ自体が `ref struct` 型である `out / ref` パラメーターがある場合は、呼び出し元でキャプチャできます。</span><span class="sxs-lookup"><span data-stu-id="f922f-144">Even though the callee cannot capture the `params` argument it can still be captured in the caller when there is a `ref` or a `out / ref` parameter that is itself a `ref struct` type.</span></span> 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

<span data-ttu-id="f922f-145">ただし、これらのケースは静的に検出できます。</span><span class="sxs-lookup"><span data-stu-id="f922f-145">These cases are statically detectable though.</span></span> <span data-ttu-id="f922f-146">`ref` が返された場合、または `out` または `ref`によって渡された `ref struct` パラメーターがある場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-146">It potentially occurs whenever there is a `ref` return or a `ref struct` parameter passed by `out` or `ref`.</span></span> <span data-ttu-id="f922f-147">このような場合、コンパイラは、すべての呼び出しに対して新しい `T[]` を割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-147">In such a case the compiler must allocate a fresh `T[]` for every invocation.</span></span> 

<span data-ttu-id="f922f-148">このドキュメントの最後では、いくつかの潜在的な最適化戦略について説明します。</span><span class="sxs-lookup"><span data-stu-id="f922f-148">Several other potential optimization strategies are discussed at the end of this document.</span></span>

<span data-ttu-id="f922f-149">`IEnumerable<T>` バリアントは、単に便利なオーバーロードです。</span><span class="sxs-lookup"><span data-stu-id="f922f-149">The `IEnumerable<T>` variant is a merely a convenience overload.</span></span> <span data-ttu-id="f922f-150">`IEnumerable<T>` が頻繁に使用されていても `params` 使用量が多いシナリオでは便利です。</span><span class="sxs-lookup"><span data-stu-id="f922f-150">It's useful in scenarios which have frequent uses of `IEnumerable<T>` but also have lots of `params` usage.</span></span> <span data-ttu-id="f922f-151">`T` argument 形式で呼び出されると、現在の `params T[]` の場合と同様に、バッキングストレージが `T[]` として割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="f922f-151">When invoked in `T` argument form the backing storage will be allocated as a `T[]` just as `params T[]` is done today.</span></span>

### <a name="params-overload-resolution-changes"></a><span data-ttu-id="f922f-152">params のオーバーロードの解決の変更</span><span class="sxs-lookup"><span data-stu-id="f922f-152">params overload resolution changes</span></span>
<span data-ttu-id="f922f-153">この提案では、言語に4つのバリアント `params` があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="f922f-153">This proposal means the language now has four variants of `params` where before it had one.</span></span> <span data-ttu-id="f922f-154">メソッドでは、`params` 宣言の型のみが異なるメソッドのオーバーロードを定義するのが賢明です。</span><span class="sxs-lookup"><span data-stu-id="f922f-154">It is sensible for methods to define overloads of methods that differ only on the type of a `params` declarations.</span></span> 

<span data-ttu-id="f922f-155">`StringBuilder.AppendFormat` によって、`params object[]`に加えて `params ReadOnlySpan<object>` オーバーロードが確実に追加されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f922f-155">Consider that `StringBuilder.AppendFormat` would certainly add a `params ReadOnlySpan<object>` overload in addition to the `params object[]`.</span></span> <span data-ttu-id="f922f-156">これにより、呼び出し元のコードを変更しなくてもコレクションの割り当てを減らすことで、パフォーマンスを大幅に向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="f922f-156">This would allow it to substantially improve performance by reducing collection allocations without requiring any changes to the calling code.</span></span> 

<span data-ttu-id="f922f-157">これを容易にするために、この言語では、次のオーバーロードの解決規則に違反する規則が導入されています。</span><span class="sxs-lookup"><span data-stu-id="f922f-157">To facilitate this the language will introduce the following overload resolution tie breaking rule.</span></span> <span data-ttu-id="f922f-158">候補のメソッドが `params` パラメーターのみで異なる場合、候補は次の順序で優先されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-158">When the candidate methods differ only by the `params` parameter then the candidates will be preferred in the following order:</span></span>

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

<span data-ttu-id="f922f-159">この順序は、一般的なケースで最も効率的ではありません。</span><span class="sxs-lookup"><span data-stu-id="f922f-159">This order is the most to the least efficient for the general case.</span></span>

### <a name="variant"></a><span data-ttu-id="f922f-160">Variant</span><span class="sxs-lookup"><span data-stu-id="f922f-160">Variant</span></span>
<span data-ttu-id="f922f-161">CoreFX は、[バリアント](https://github.com/dotnet/corefxlab/pull/2595)という名前の新しいマネージ型のプロトタイプを作成しています。</span><span class="sxs-lookup"><span data-stu-id="f922f-161">CoreFX is prototyping a new managed type named [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span></span> <span data-ttu-id="f922f-162">この型は、異種値を必要とするものの、パラメーターとして `object` を使用することによってオーバーヘッドを発生させたくない Api で使用することを意図しています。</span><span class="sxs-lookup"><span data-stu-id="f922f-162">This type is meant to be used in APIs which expect heterogeneous values but don't want the overhead brought on by using `object` as the parameter.</span></span> <span data-ttu-id="f922f-163">`Variant` 型はユニバーサルストレージを提供しますが、最も一般的に使用される型に対してボックス化割り当てを回避します。</span><span class="sxs-lookup"><span data-stu-id="f922f-163">The `Variant` type provides universal storage but avoids the boxing allocation for the most commonly used types.</span></span> <span data-ttu-id="f922f-164">この型を `string.Format` のような Api で使用すると、ほとんどの場合にボックス化オーバーヘッドがなくなります。</span><span class="sxs-lookup"><span data-stu-id="f922f-164">Using this type in APIs like `string.Format` can eliminate the boxing overhead in the majority of cases.</span></span>

<span data-ttu-id="f922f-165">この型自体は、必ずしも言語に固有のものではありません。</span><span class="sxs-lookup"><span data-stu-id="f922f-165">This type itself is not necessarily special to the language.</span></span> <span data-ttu-id="f922f-166">このドキュメントでは、提案の他の部分の実装について詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="f922f-166">It is being introduced in this document separately though as it becomes an implementation detail of other parts of the proposal.</span></span> 

### <a name="efficient-interpolated-strings"></a><span data-ttu-id="f922f-167">効率的な挿入文字列</span><span class="sxs-lookup"><span data-stu-id="f922f-167">Efficient interpolated strings</span></span>
<span data-ttu-id="f922f-168">補間文字列は、でC#はまだ非効率的な機能です。</span><span class="sxs-lookup"><span data-stu-id="f922f-168">Interpolated strings are a popular yet inefficient feature in C#.</span></span> <span data-ttu-id="f922f-169">`string`として補間 `string` を使用する最も一般的な構文は `string.Format(string, params object[])` 呼び出しに変換されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-169">The most common syntax, using an interpolated `string` as a `string`, translates into a `string.Format(string, params object[])` call.</span></span> <span data-ttu-id="f922f-170">これによって、すべての値型に対するボックス化割り当てが発生します。また、実装では、`string.Format`の "高速" オーバーロードで引数の数がパラメーターの数を超えた場合に、配列の割り当てに加えて、書式設定に `object.ToString` を使用することが多いため、中間 `string` 割り当てが発生します。</span><span class="sxs-lookup"><span data-stu-id="f922f-170">That will incur boxing allocations for all value types, intermediate `string` allocations as the implementation largely uses `object.ToString` for formatting as well as array allocations once the number of arguments exceeds the amount of parameters on the "fast" overloads of `string.Format`.</span></span> 

<span data-ttu-id="f922f-171">言語によって、`string.Format`の代替オーバーロードが考慮されるように補間が変化します。</span><span class="sxs-lookup"><span data-stu-id="f922f-171">The language will change its interpolation lowering to consider alternate overloads of `string.Format`.</span></span> <span data-ttu-id="f922f-172">`string.Format(string, params)` のすべての形式を考慮し、引数の型を満たす "最適" のオーバーロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="f922f-172">It will consider all forms of `string.Format(string, params)` and pick the "best" overload which satisfies the argument types.</span></span>
<span data-ttu-id="f922f-173">"Best" `params` のオーバーロードは、前述の規則によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-173">The "best" `params` overload will be determined by the rules discussed above.</span></span> <span data-ttu-id="f922f-174">これは、補間 `string` が `string.Format(string format, params ReadOnlySpan<Variant> args)`などの非常に効率的なオーバーロードにバインドできるようになったことを意味します。</span><span class="sxs-lookup"><span data-stu-id="f922f-174">This means interpolated `string` can now bind to very efficient overloads like `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span></span> <span data-ttu-id="f922f-175">多くの場合、これによりすべての中間割り当てが削除されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-175">In many cases this will remove all intermediate allocations.</span></span>

### <a name="customizable-interpolated-strings"></a><span data-ttu-id="f922f-176">カスタマイズ可能な挿入文字列</span><span class="sxs-lookup"><span data-stu-id="f922f-176">Customizable interpolated strings</span></span>
<span data-ttu-id="f922f-177">開発者は、`FormattableString`を使用して、挿入文字列の動作をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="f922f-177">Developers are able to customize the behavior of interpolated strings with `FormattableString`.</span></span> <span data-ttu-id="f922f-178">これには、挿入文字列に変換されるデータが含まれます。形式 `string` と引数を配列として指定します。</span><span class="sxs-lookup"><span data-stu-id="f922f-178">This contains the data which goes into an interpolated string: the format `string` and the arguments as an array.</span></span> <span data-ttu-id="f922f-179">ただし、この場合も、ボックス化と引数の配列割り当てと `FormattableString` (`abstract class`) の割り当てがあります。</span><span class="sxs-lookup"><span data-stu-id="f922f-179">This though still has the boxing and argument array allocation as well as the allocation for `FormattableString` (it's an `abstract class`).</span></span> <span data-ttu-id="f922f-180">したがって、`string` の書式設定に大きな割り当てを行うアプリケーションにはほとんど使用しません。</span><span class="sxs-lookup"><span data-stu-id="f922f-180">Hence it's of little use to applications which are allocation heavy in `string` formatting.</span></span>

<span data-ttu-id="f922f-181">挿入文字列の書式設定を効率的にするために、言語は `System.ValueFormattableString`新しい型を認識します。</span><span class="sxs-lookup"><span data-stu-id="f922f-181">To make interpolated string formatting efficient the language will recognize a new type: `System.ValueFormattableString`.</span></span> <span data-ttu-id="f922f-182">すべての補間文字列は、この型への変換対象の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-182">All interpolated strings will have a target type conversion to this type.</span></span> <span data-ttu-id="f922f-183">これは、現在 `FormattableString.Create` の場合とまったく同じように、挿入文字列を呼び出し `ValueFormattableString.Create` に変換することによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-183">This will be implemented by translating the interpolated string into the call `ValueFormattableString.Create` exactly as is done for `FormattableString.Create` today.</span></span> <span data-ttu-id="f922f-184">言語は、このドキュメントで説明されているすべての `params` オプションをサポートしており、最適な `ValueFormattableString.Create` 方法を探しています。</span><span class="sxs-lookup"><span data-stu-id="f922f-184">The language will support all `params` options described in this document when looking for the most suitable `ValueFormattableString.Create` method.</span></span> 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

<span data-ttu-id="f922f-185">オーバーロードの解決規則は、引数が挿入文字列の場合に `string` より `ValueFormattableString` を優先するように変更されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-185">Overload resolution rules will be changed to prefer `ValueFormattableString` over `string` when the argument is an interpolated string.</span></span> <span data-ttu-id="f922f-186">つまり、`string` と `ValueFormattableString`のみが異なるオーバーロードを使用することが重要になります。</span><span class="sxs-lookup"><span data-stu-id="f922f-186">This means it will be valuable to have overloads which differ only on `string` and `ValueFormattableString`.</span></span> <span data-ttu-id="f922f-187">現時点では、`FormattableString` によるオーバーロードは、コンパイラが常に (開発者が明示的なキャストを使用しない限り) `string` バージョンを優先するため、価値がありません。</span><span class="sxs-lookup"><span data-stu-id="f922f-187">Such an overload today with `FormattableString` is not valuable as the compiler will always prefer the `string` version (unless the developer uses an explicit cast).</span></span> 

## <a name="open-issues"></a><span data-ttu-id="f922f-188">懸案事項を開く</span><span class="sxs-lookup"><span data-stu-id="f922f-188">Open Issues</span></span>

### <a name="valueformattablestring-breaking-change"></a><span data-ttu-id="f922f-189">ValueFormattableString の重大な変更</span><span class="sxs-lookup"><span data-stu-id="f922f-189">ValueFormattableString breaking change</span></span>
<span data-ttu-id="f922f-190">`string` に対するオーバーロードの解決中に `ValueFormattableString` 優先される変更は、互換性に影響する変更です。</span><span class="sxs-lookup"><span data-stu-id="f922f-190">The change to prefer `ValueFormattableString` during overload resolution over `string` is a breaking change.</span></span> <span data-ttu-id="f922f-191">開発者は `ValueFormattableString` 今日と呼ばれる型を定義し、`string`でメソッドのオーバーロードで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="f922f-191">It is possible for a developer to have defined a type called `ValueFormattableString` today and use it in method overloads with `string`.</span></span> <span data-ttu-id="f922f-192">この提案された変更により、この一連の機能が実装された後、コンパイラは異なるオーバーロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="f922f-192">This proposed change would cause the compiler to pick a different overload once this set of features was implemented.</span></span> 

<span data-ttu-id="f922f-193">このような可能性は適度に低いと思われます。</span><span class="sxs-lookup"><span data-stu-id="f922f-193">The possibility of this seems reasonably low.</span></span> <span data-ttu-id="f922f-194">型には完全な名前 `System.ValueFormattableString` が必要であり、`Create`という名前の `static` メソッドが必要になります。</span><span class="sxs-lookup"><span data-stu-id="f922f-194">The type would need the full name `System.ValueFormattableString` and it would need to have `static` methods named `Create`.</span></span> <span data-ttu-id="f922f-195">開発者が `System` 名前空間の型を定義しないことを強くお勧めしないので、このブレークは妥当な妥協のように見えます。</span><span class="sxs-lookup"><span data-stu-id="f922f-195">Given that developers are strongly discouraged from defining any type in the `System` namespace this break seems like a reasonable compromise.</span></span>

### <a name="expanding-to-more-types"></a><span data-ttu-id="f922f-196">その他の型への展開</span><span class="sxs-lookup"><span data-stu-id="f922f-196">Expanding to more types</span></span>
<span data-ttu-id="f922f-197">ここでは、`params` がサポートされている一連のコレクションに `IList<T>`、`ICollection<T>` および `IReadOnlyList<T>` を追加することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="f922f-197">Given we're in the area we should consider adding `IList<T>`, `ICollection<T>` and `IReadOnlyList<T>` to the set of collections for which `params` is supported.</span></span> <span data-ttu-id="f922f-198">実装に関しては、ここでは他の作業について若干のコストがかかります。</span><span class="sxs-lookup"><span data-stu-id="f922f-198">In terms of implementation it will cost a small amount over the other work here.</span></span>

<span data-ttu-id="f922f-199">LDM では、言語の複雑さを考慮する必要があるかどうかを判断する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-199">LDM needs to decide if the complication to the language is worth it though.</span></span> <span data-ttu-id="f922f-200">`IEnumerable<T>` を追加すると、非常に具体的な摩擦ポイントが削除されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-200">The addition of `IEnumerable<T>` removes a very specific friction point.</span></span> <span data-ttu-id="f922f-201">この `params` ソリューションがない場合、多くのお客様は `params` メソッドを呼び出すときに `IEnumerable<T>` から `T[]` を割り当てようとしました。</span><span class="sxs-lookup"><span data-stu-id="f922f-201">Lacking this `params` solution many customers were forced to allocate `T[]` from an `IEnumerable<T>` when calling a `params` method.</span></span> <span data-ttu-id="f922f-202">ただし、`IEnumerable<T>` を追加することによって修正されます。</span><span class="sxs-lookup"><span data-stu-id="f922f-202">The addition of `IEnumerable<T>` fixes this though.</span></span> <span data-ttu-id="f922f-203">ここでは、他のインターフェイスが修正する特定の摩擦点はありません。</span><span class="sxs-lookup"><span data-stu-id="f922f-203">There is no specific friction point that the other interfaces fix here.</span></span> 

## <a name="considerations"></a><span data-ttu-id="f922f-204">考慮事項</span><span class="sxs-lookup"><span data-stu-id="f922f-204">Considerations</span></span>

### <a name="variant2-and-variant3"></a><span data-ttu-id="f922f-205">Variant2 と Variant3</span><span class="sxs-lookup"><span data-stu-id="f922f-205">Variant2 and Variant3</span></span>
<span data-ttu-id="f922f-206">CoreFX チームは、最大3つの `Variant` 引数に対して、割り当てられていないストレージ型のセットも持っています。</span><span class="sxs-lookup"><span data-stu-id="f922f-206">The CoreFX team also has a non-allocating set of storage types for up to three `Variant` arguments.</span></span> <span data-ttu-id="f922f-207">これらは、1つの `Variant`、`Variant2` と `Variant3`です。</span><span class="sxs-lookup"><span data-stu-id="f922f-207">These are a single `Variant`, `Variant2` and `Variant3`.</span></span> <span data-ttu-id="f922f-208">すべてには、`CreateSpan` と `KeepAlive`の割り当てを自由に `Span<Variant>` するためのメソッドのペアがあります。</span><span class="sxs-lookup"><span data-stu-id="f922f-208">All have a pair of methods for getting an allocation free `Span<Variant>` off of them: `CreateSpan` and `KeepAlive`.</span></span> <span data-ttu-id="f922f-209">これは、最大3つの引数の `params Span<Variant>` に対して、呼び出しサイトが完全に解放される可能性があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="f922f-209">This means for a `params Span<Variant>` of up to three arguments the call site can be entirely allocation free.</span></span>

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="f922f-210">`Go` メソッドは、次のように下げることができます。</span><span class="sxs-lookup"><span data-stu-id="f922f-210">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

<span data-ttu-id="f922f-211">これには、`params Span<T>` の呼び出し間で `T[]` を再利用するために、提案の上で非常に少ない作業が必要になります。</span><span class="sxs-lookup"><span data-stu-id="f922f-211">This requires very little work on top of the proposal to re-use `T[]` between `params Span<T>` calls.</span></span> <span data-ttu-id="f922f-212">コンパイラは、呼び出しごとに一時的なを管理する必要があります。また、の後で作業をクリーンアップする必要があります (1 つのケースの場合でも、内部一時を free としてマークするだけです)。</span><span class="sxs-lookup"><span data-stu-id="f922f-212">The compiler already needs to manage a temporary per call and do clean up work after (even if in one case it's just marking an internal temp as free).</span></span> 

<span data-ttu-id="f922f-213">注: `KeepAlive` 関数は、デスクトップでのみ必要です。</span><span class="sxs-lookup"><span data-stu-id="f922f-213">Note: the `KeepAlive` function is only necessary on desktop.</span></span> <span data-ttu-id="f922f-214">.NET Core では、メソッドは使用できないため、コンパイラは呼び出しを生成しません。</span><span class="sxs-lookup"><span data-stu-id="f922f-214">On .NET Core the method will not be available and hence the compiler won't emit a call to it.</span></span>

### <a name="clr-stack-allocation-helpers"></a><span data-ttu-id="f922f-215">CLR スタック割り当てヘルパー</span><span class="sxs-lookup"><span data-stu-id="f922f-215">CLR stack allocation helpers</span></span>
<span data-ttu-id="f922f-216">CLR は、連続したメモリのスタック割り当てに対してのみ[localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2)を提供します。</span><span class="sxs-lookup"><span data-stu-id="f922f-216">The CLR only provides only [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) for stack allocation of contiguous memory.</span></span> <span data-ttu-id="f922f-217">この命令は `unmanaged` 型に対してのみ機能することに制限されています。</span><span class="sxs-lookup"><span data-stu-id="f922f-217">This instruction is limited in that it only works for `unmanaged` types.</span></span> <span data-ttu-id="f922f-218">これは、`params 
 Span<T>`のバッキングストレージを効率的に割り当てるためのユニバーサルソリューションとして使用できないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="f922f-218">This means it can't be used as a universal solution for efficiently allocating the backing storage for `params 
 Span<T>`.</span></span> 

<span data-ttu-id="f922f-219">ただし、この制限は基本的な制限事項ではありませんが、履歴の成果物が多くなります。</span><span class="sxs-lookup"><span data-stu-id="f922f-219">This limitation is not some fundamental restriction though but instead more an artifact of history.</span></span> <span data-ttu-id="f922f-220">CLR は、ユニバーサルスタック割り当てを提供する新しい op コード/組み込みを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="f922f-220">The CLR could choose to add new op codes / intrinsics which provide universal stack allocation.</span></span> <span data-ttu-id="f922f-221">その後、これらを使用して、ほとんどの `params Span<T>` 呼び出しのバッキングストレージを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="f922f-221">These could then be used to allocate the backing storage for most `params Span<T>` calls.</span></span>

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="f922f-222">`Go` メソッドは、次のように下げることができます。</span><span class="sxs-lookup"><span data-stu-id="f922f-222">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

<span data-ttu-id="f922f-223">このアプローチはヒープ効率が非常に高いのに対し、余分なスタック使用が発生します。</span><span class="sxs-lookup"><span data-stu-id="f922f-223">While this approach is very heap efficient it does cause extra stack usage.</span></span> <span data-ttu-id="f922f-224">ディープスタックと `params` の使用量が多いアルゴリズムでは、単純な `T[]` 割り当てが成功すると、`StackOverflowException` が生成される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-224">In an algorithm which has a deep stack and lots of `params` usage it's possible this could cause a `StackOverflowException` to be generated where a simple `T[]` allocation would succeed.</span></span> 

<span data-ttu-id="f922f-225">残念C#ながら、メソッド間分析の種類については設定されていませんが、呼び出しでは `params`のスタックまたはヒープ割り当てを使用するかどうかを教育的に判断できるようになっています。</span><span class="sxs-lookup"><span data-stu-id="f922f-225">Unfortunately C# is not set up for the type of inter-method analysis where it could make an educated determination of whether or not call should use stack or heap allocation of `params`.</span></span> <span data-ttu-id="f922f-226">実際には、それぞれの呼び出しを独自に検討するだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="f922f-226">It can only really consider each call on its own.</span></span>

<span data-ttu-id="f922f-227">CLR は、実行時にこの種類の決定を行うのに最適な設定です。</span><span class="sxs-lookup"><span data-stu-id="f922f-227">The CLR is best setup for making this type of determination at runtime.</span></span> <span data-ttu-id="f922f-228">このため、ランタイムには、ユニバーサルスタック割り当てに対して2つのメソッドが用意されている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-228">Hence we'd likely have the runtime provide two methods for universal stack allocation:</span></span>

1. <span data-ttu-id="f922f-229">`Span<T> StackAlloc<T>(int length)`: これは、`localloc` の動作および制限と同じですが、`T`の型で動作することはできません。</span><span class="sxs-lookup"><span data-stu-id="f922f-229">`Span<T> StackAlloc<T>(int length)`: this has the same behaviors and limitations of `localloc` except it can work on any type `T`.</span></span> 
1. <span data-ttu-id="f922f-230">`Span<T> MaybeStackAlloc<T>(int length)`: このランタイムは、スタックまたはヒープの割り当てを行うことによって、これを実装することを選択できます。</span><span class="sxs-lookup"><span data-stu-id="f922f-230">`Span<T> MaybeStackAlloc<T>(int length)`: this runtime can choose to implement this by doing a stack or heap allocation.</span></span> <span data-ttu-id="f922f-231">ランタイムは、呼び出し元の実行コンテキストを使用して、`Span<T>` の割り当て方法を決定できます。</span><span class="sxs-lookup"><span data-stu-id="f922f-231">The runtime can then use the execution context in which it's called to determine how the `Span<T>` is allocated.</span></span> <span data-ttu-id="f922f-232">ただし、呼び出し元は、スタック割り当てされているかのように、常にそれを処理します。</span><span class="sxs-lookup"><span data-stu-id="f922f-232">The caller though will always treat it as if it were stack allocated.</span></span>

<span data-ttu-id="f922f-233">1 ~ 2 個の引数など、非常に単純なC#ケースでは、コンパイラは常に `StackAlloc<T>` variant を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f922f-233">For very simple cases, like one to two arguments, the C# compiler could always use `StackAlloc<T>` variant.</span></span> <span data-ttu-id="f922f-234">ほとんどの場合、これはスタック枯渇に多大な影響を与えることはほとんどありません。</span><span class="sxs-lookup"><span data-stu-id="f922f-234">This is unlikely to significantly contribute to stack exhaustion in most cases.</span></span> <span data-ttu-id="f922f-235">その他のケースでは、コンパイラは代わりに `MaybeStackAlloc<T>` を使用し、ランタイムが呼び出しを行うようにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="f922f-235">For other cases the compiler could choose to use `MaybeStackAlloc<T>` instead and let the runtime make the call.</span></span>

<span data-ttu-id="f922f-236">どのように選択するかは、実際のアプリをより深く調査し、調査することが必要になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f922f-236">How we choose will likely require a deeper investigation and examination of real world apps.</span></span> <span data-ttu-id="f922f-237">ただし、これらの新しい組み込みを利用できる場合は、このような柔軟性が得られます。</span><span class="sxs-lookup"><span data-stu-id="f922f-237">But if these new intrinsics are available then it will give us this type of flexibility.</span></span>

### <a name="why-not-varargs"></a><span data-ttu-id="f922f-238">Varargs でないのはなぜですか。</span><span class="sxs-lookup"><span data-stu-id="f922f-238">Why not varargs?</span></span> 
<span data-ttu-id="f922f-239">既存の[varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017)機能は、考えられる解決策として検討されました。</span><span class="sxs-lookup"><span data-stu-id="f922f-239">The existing [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) feature was considered here as a possible solution.</span></span> <span data-ttu-id="f922f-240">ただし、この機能は主にC++/cli のシナリオに使用され、他のシナリオでは既知のホールを持ちます。</span><span class="sxs-lookup"><span data-stu-id="f922f-240">This feature though is meant primarily for C++/CLI scenarios and has known holes for other scenarios.</span></span> <span data-ttu-id="f922f-241">さらに、Unix への移植にも大きなコストがかかります。</span><span class="sxs-lookup"><span data-stu-id="f922f-241">Additionally there is significant cost in porting this to Unix.</span></span> <span data-ttu-id="f922f-242">そのため、これは実用的な解決策としては見えませんでした。</span><span class="sxs-lookup"><span data-stu-id="f922f-242">Hence it wasn't seen as a viable solution.</span></span>

## <a name="related-issues"></a><span data-ttu-id="f922f-243">関連する問題</span><span class="sxs-lookup"><span data-stu-id="f922f-243">Related Issues</span></span>
<span data-ttu-id="f922f-244">この仕様は、次の問題に関連しています。</span><span class="sxs-lookup"><span data-stu-id="f922f-244">This spec is related to the following issues:</span></span> 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

