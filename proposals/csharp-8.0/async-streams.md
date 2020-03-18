---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484148"
---
# <a name="async-streams"></a><span data-ttu-id="84704-101">非同期ストリーム</span><span class="sxs-lookup"><span data-stu-id="84704-101">Async Streams</span></span>

* <span data-ttu-id="84704-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="84704-102">[x] Proposed</span></span>
* <span data-ttu-id="84704-103">[x] プロトタイプ</span><span class="sxs-lookup"><span data-stu-id="84704-103">[x] Prototype</span></span>
* <span data-ttu-id="84704-104">[] の実装</span><span class="sxs-lookup"><span data-stu-id="84704-104">[ ] Implementation</span></span>
* <span data-ttu-id="84704-105">[] 仕様</span><span class="sxs-lookup"><span data-stu-id="84704-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="84704-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="84704-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="84704-107">C#では、反復子メソッドと非同期メソッドがサポートされていますが、反復子と非同期のメソッドの両方をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="84704-107">C# has support for iterator methods and async methods, but no support for a method that is both an iterator and an async method.</span></span>  <span data-ttu-id="84704-108">これを修正するには、`async` 反復子の新しい形式で `await` を使用できるようにします。これにより、`IEnumerable<T>` または `IEnumerator<T>`ではなく `IAsyncEnumerable<T>` または `IAsyncEnumerator<T>` を返し、新しい `IAsyncEnumerable<T>` で使用できるようになります。`await foreach`</span><span class="sxs-lookup"><span data-stu-id="84704-108">We should rectify this by allowing for `await` to be used in a new form of `async` iterator, one that returns an `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` rather than an `IEnumerable<T>` or `IEnumerator<T>`, with `IAsyncEnumerable<T>` consumable in a new `await foreach`.</span></span>  <span data-ttu-id="84704-109">`IAsyncDisposable` インターフェイスは、非同期のクリーンアップを有効にするためにも使用されます。</span><span class="sxs-lookup"><span data-stu-id="84704-109">An `IAsyncDisposable` interface is also used to enable asynchronous cleanup.</span></span>

## <a name="related-discussion"></a><span data-ttu-id="84704-110">関連の説明</span><span class="sxs-lookup"><span data-stu-id="84704-110">Related discussion</span></span>
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a><span data-ttu-id="84704-111">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="84704-111">Detailed design</span></span>
[design]: #detailed-design

## <a name="interfaces"></a><span data-ttu-id="84704-112">インターフェイス</span><span class="sxs-lookup"><span data-stu-id="84704-112">Interfaces</span></span>

### <a name="iasyncdisposable"></a><span data-ttu-id="84704-113">IAsyncDisposable</span><span class="sxs-lookup"><span data-stu-id="84704-113">IAsyncDisposable</span></span>

<span data-ttu-id="84704-114">`IAsyncDisposable` (https://github.com/dotnet/roslyn/issues/114) など) については、よく説明されています。</span><span class="sxs-lookup"><span data-stu-id="84704-114">There has been much discussion of `IAsyncDisposable` (e.g. https://github.com/dotnet/roslyn/issues/114) and whether it's a good idea.</span></span>  <span data-ttu-id="84704-115">ただし、非同期反復子のサポートを追加するために必要な概念です。</span><span class="sxs-lookup"><span data-stu-id="84704-115">However, it's a required concept to add in support of async iterators.</span></span>  <span data-ttu-id="84704-116">`finally` ブロックには `await`が含まれる場合があり、反復子の破棄の一部として `finally` ブロックを実行する必要があるため、非同期に破棄する必要があります。</span><span class="sxs-lookup"><span data-stu-id="84704-116">Since `finally` blocks may contain `await`s, and since `finally` blocks need to be run as part of disposing of iterators, we need async disposal.</span></span>  <span data-ttu-id="84704-117">また、一般に、リソースのクリーンアップでは、ファイルを閉じる (フラッシュが必要)、コールバックを解除する、登録解除が完了したことを知る方法を提供するなど、時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="84704-117">It's also just generally useful any time cleaning up of resources might take any period of time, e.g. closing files (requiring flushes), deregistering callbacks and providing a way to know when deregistration has completed, etc.</span></span>

<span data-ttu-id="84704-118">次のインターフェイスがコア .NET ライブラリに追加されます (例: System.private.corelib/System)。</span><span class="sxs-lookup"><span data-stu-id="84704-118">The following interface is added to the core .NET libraries (e.g. System.Private.CoreLib / System.Runtime):</span></span>
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
<span data-ttu-id="84704-119">`Dispose`と同様に、`DisposeAsync` を複数回呼び出すことは可能であり、その後の呼び出しは nops として処理され、同期的に完了したタスクを返す必要があります`DisposeAsync` (ただし、スレッドセーフである必要がありますが、同時呼び出しはサポートされていません)。</span><span class="sxs-lookup"><span data-stu-id="84704-119">As with `Dispose`, invoking `DisposeAsync` multiple times is acceptable, and subsequent invocations after the first should be treated as nops, returning a synchronously completed successful task (`DisposeAsync` need not be thread-safe, though, and need not support concurrent invocation).</span></span>  <span data-ttu-id="84704-120">さらに、型には `IDisposable` と `IAsyncDisposable`の両方が実装されている場合がありますが、その場合は `Dispose` を呼び出して `DisposeAsync` またはその逆を行うこともできますが、それ以降のいずれかの呼び出しは nop である必要があります。</span><span class="sxs-lookup"><span data-stu-id="84704-120">Further, types may implement both `IDisposable` and `IAsyncDisposable`, and if they do, it's similarly acceptable to invoke `Dispose` and then `DisposeAsync` or vice versa, but only the first should be meaningful and subsequent invocations of either should be a nop.</span></span>  <span data-ttu-id="84704-121">そのため、型が両方を実装している場合、コンシューマーは1回だけ呼び出すことをお勧めします。また、コンテキストに基づいて関連性の高いメソッドを1回だけ呼び出し、同期コンテキストで `Dispose` し、非同期のメソッドを `DisposeAsync` します。</span><span class="sxs-lookup"><span data-stu-id="84704-121">As such, if a type does implement both, consumers are encouraged to call once and only once the more relevant method based on the context, `Dispose` in synchronous contexts and `DisposeAsync` in asynchronous ones.</span></span>

<span data-ttu-id="84704-122">(`IAsyncDisposable` が `using` とどのように対話するかについて説明しておきます。</span><span class="sxs-lookup"><span data-stu-id="84704-122">(I'm leaving discussion of how `IAsyncDisposable` interacts with `using` to a separate discussion.</span></span>  <span data-ttu-id="84704-123">`foreach` との相互作用については、この提案で後ほど扱います)。</span><span class="sxs-lookup"><span data-stu-id="84704-123">And coverage of how it interacts with `foreach` is handled later in this proposal.)</span></span>

<span data-ttu-id="84704-124">検討対象の候補:</span><span class="sxs-lookup"><span data-stu-id="84704-124">Alternatives considered:</span></span>
- <span data-ttu-id="84704-125">_`CancellationToken`を受け入れる`DisposeAsync`_ : 理論的には、すべての非同期を取り消すことができますが、破棄はクリーンアップ、終了、free'ing リソースなど、通常はキャンセルすべきものではないことを意味します。取り消された作業にはクリーンアップが依然として重要です。</span><span class="sxs-lookup"><span data-stu-id="84704-125">_`DisposeAsync` accepting a `CancellationToken`_: while in theory it makes sense that anything async can be canceled, disposal is about cleanup, closing things out, free'ing resources, etc., which is generally not something that should be canceled; cleanup is still important for work that's canceled.</span></span>  <span data-ttu-id="84704-126">実際の作業が取り消される原因となった同じ `CancellationToken` は、通常 `DisposeAsync`に渡されるトークンと同じであり、作業の取り消しによって `DisposeAsync` が nop になる可能性があるため `DisposeAsync` 意味がありません。</span><span class="sxs-lookup"><span data-stu-id="84704-126">The same `CancellationToken` that caused the actual work to be canceled would typically be the same token passed to `DisposeAsync`, making `DisposeAsync` worthless because cancellation of the work would cause `DisposeAsync` to be a nop.</span></span>  <span data-ttu-id="84704-127">破棄を待機しているユーザーがブロックされないようにする場合は、結果の `ValueTask`で待機したり、一定の期間だけ待機したりすることを避けることができます。</span><span class="sxs-lookup"><span data-stu-id="84704-127">If someone wants to avoid being blocked waiting for disposal, they can avoid waiting on the resulting `ValueTask`, or wait on it only for some period of time.</span></span>
- <span data-ttu-id="84704-128">_`Task`を返す`DisposeAsync`_ は、非ジェネリックの `ValueTask` が存在し、`IValueTaskSource`から構築できるようになったので、`ValueTask` から `DisposeAsync` を返すことにより、既存のオブジェクトを `DisposeAsync`の非同期完了を表す promise として再利用できるようになり、`Task` が非同期に完了した場合の `DisposeAsync` 割り当てが保存されます。</span><span class="sxs-lookup"><span data-stu-id="84704-128">_`DisposeAsync` returning a `Task`_: Now that a non-generic `ValueTask` exists and can be constructed from an `IValueTaskSource`, returning `ValueTask` from `DisposeAsync` allows an existing object to be reused as the promise representing the eventual async completion of `DisposeAsync`, saving a `Task` allocation in the case where `DisposeAsync` completes asynchronously.</span></span>
- <span data-ttu-id="84704-129">_`bool continueOnCapturedContext` (`ConfigureAwait`) を使用した `DisposeAsync` の構成_: このような概念が `using`、`foreach`、およびこれを使用するその他の言語構造にどのように公開されているかに関連する問題が発生する場合がありますが、インターフェイスの観点からは、実際には何も実行されず、構成するものはありません。`ValueTask` のコンシューマーは、必要に応じてそれを使用できます。`await`</span><span class="sxs-lookup"><span data-stu-id="84704-129">_Configuring `DisposeAsync` with a `bool continueOnCapturedContext` (`ConfigureAwait`)_: While there may be issues related to how such a concept is exposed to `using`, `foreach`, and other language constructs that consume this, from an interface perspective it's not actually doing any `await`'ing and there's nothing to configure... consumers of the `ValueTask` can consume it however they wish.</span></span>
- <span data-ttu-id="84704-130">_`IDisposable`を継承する`IAsyncDisposable`_ : 1 つのまたは一方だけを使用する必要があるため、型を強制的に実装することは意味がありません。</span><span class="sxs-lookup"><span data-stu-id="84704-130">_`IAsyncDisposable` inheriting `IDisposable`_:  Since only one or the other should be used, it doesn't make sense to force types to implement both.</span></span>
- <span data-ttu-id="84704-131">_`IAsyncDisposable`ではなく`IDisposableAsync`_ : "非同期的なもの" という名前が付けられているのに対し、操作は "非同期的な処理" です。そのため、型はプレフィックスとして "async" を持ち、メソッドはサフィックスとして "async" を持ちます。</span><span class="sxs-lookup"><span data-stu-id="84704-131">_`IDisposableAsync` instead of `IAsyncDisposable`_: We've been following the naming that things/types are an "async something" whereas operations are "done async", so types have "Async" as a prefix and methods have "Async" as a suffix.</span></span>

### <a name="iasyncenumerable--iasyncenumerator"></a><span data-ttu-id="84704-132">IAsyncEnumerable / IAsyncEnumerator</span><span class="sxs-lookup"><span data-stu-id="84704-132">IAsyncEnumerable / IAsyncEnumerator</span></span>

<span data-ttu-id="84704-133">コア .NET ライブラリには、次の2つのインターフェイスが追加されます。</span><span class="sxs-lookup"><span data-stu-id="84704-133">Two interfaces are added to the core .NET libraries:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

<span data-ttu-id="84704-134">(追加の言語機能を使用しない) 一般的な消費は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="84704-134">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="84704-135">破棄されるオプション:</span><span class="sxs-lookup"><span data-stu-id="84704-135">Discarded options considered:</span></span>
- <span data-ttu-id="84704-136">_`Task<bool> MoveNextAsync(); T current { get; }`_ : `Task<bool>` を使用すると、キャッシュされたタスクオブジェクトを使用して同期的に成功した `MoveNextAsync` 呼び出しを表すことができますが、非同期の完了には割り当てが依然として必要になります。</span><span class="sxs-lookup"><span data-stu-id="84704-136">_`Task<bool> MoveNextAsync(); T current { get; }`_: Using `Task<bool>` would support using a cached task object to represent synchronous, successful `MoveNextAsync` calls, but an allocation would still be required for asynchronous completion.</span></span>  <span data-ttu-id="84704-137">`ValueTask<bool>`を返すことにより、列挙子オブジェクト自体が `IValueTaskSource<bool>` を実装し、`MoveNextAsync`から返された `ValueTask<bool>` のバッキングとして使用できるようになります。これにより、オーバーヘッドを大幅に削減できます。</span><span class="sxs-lookup"><span data-stu-id="84704-137">By returning `ValueTask<bool>`, we enable the enumerator object to itself implement `IValueTaskSource<bool>` and be used as the backing for the `ValueTask<bool>` returned from `MoveNextAsync`, which in turn allows for significantly reduced overheads.</span></span>
- <span data-ttu-id="84704-138">_`ValueTask<(bool, T)> MoveNextAsync();`_ : 使用するのが困難なだけでなく、`T` を共変にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="84704-138">_`ValueTask<(bool, T)> MoveNextAsync();`_: It's not only harder to consume, but it means that `T` can no longer be covariant.</span></span>
- <span data-ttu-id="84704-139">_`ValueTask<T?> TryMoveNextAsync();`_ : 共変ではありません。</span><span class="sxs-lookup"><span data-stu-id="84704-139">_`ValueTask<T?> TryMoveNextAsync();`_: Not covariant.</span></span>
- <span data-ttu-id="84704-140">_`Task<T?> TryMoveNextAsync();`_ : 共変ではなく、すべての呼び出しに対する割り当てなどです。</span><span class="sxs-lookup"><span data-stu-id="84704-140">_`Task<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="84704-141">_`ITask<T?> TryMoveNextAsync();`_ : 共変ではなく、すべての呼び出しに対する割り当てなどです。</span><span class="sxs-lookup"><span data-stu-id="84704-141">_`ITask<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="84704-142">_`ITask<(bool,T)> TryMoveNextAsync();`_ : 共変ではなく、すべての呼び出しに対する割り当てなどです。</span><span class="sxs-lookup"><span data-stu-id="84704-142">_`ITask<(bool,T)> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="84704-143">_`Task<bool> TryMoveNextAsync(out T result);`_ : `out` の結果は、操作が同期的に返されたときに設定する必要があります。これは、今後長時間に及ぶ可能性があるタスクを非同期に完了するときではなく、結果を伝達する手段がないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="84704-143">_`Task<bool> TryMoveNextAsync(out T result);`_: The `out` result would need to be set when the operation returns synchronously, not when it asynchronously completes the task potentially sometime long in the future, at which point there'd be no way to communicate the result.</span></span>
- <span data-ttu-id="84704-144">_`IAsyncEnumerator<T>` `IAsyncDisposable`を実装していませ_ん。これらを分離することもできます。</span><span class="sxs-lookup"><span data-stu-id="84704-144">_`IAsyncEnumerator<T>` not implementing `IAsyncDisposable`_: We could choose to separate these.</span></span>  <span data-ttu-id="84704-145">ただし、そのようにすることで、提案の他の特定の領域が複雑になります。これにより、コードは列挙子が破棄を提供しない可能性を処理できるようになるため、パターンベースのヘルパーを記述するのが困難になります。</span><span class="sxs-lookup"><span data-stu-id="84704-145">However, doing so complicates certain other areas of the proposal, as code must then be able to deal with the possibility that an enumerator doesn't provide disposal, which makes it difficult to write pattern-based helpers.</span></span>  <span data-ttu-id="84704-146">さらに、列挙子が破棄を必要とすることがよくあります (たとえばC# 、finally ブロックを持つ非同期反復子、ほとんどの場合、ネットワーク接続からデータを列挙します)。それ以外の場合は、単純なオーバーヘッドを最小限に抑えて、メソッドを単に `public ValueTask DisposeAsync() => default(ValueTask);` として実装するのが簡単です。</span><span class="sxs-lookup"><span data-stu-id="84704-146">Further, it will be common for enumerators to have a need for disposal (e.g. any C# async iterator that has a finally block, most things enumerating data from a network connection, etc.), and if one doesn't, it is simple to implement the method purely as `public ValueTask DisposeAsync() => default(ValueTask);` with minimal additional overhead.</span></span>
- <span data-ttu-id="84704-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: キャンセルトークンパラメーターがありません。</span><span class="sxs-lookup"><span data-stu-id="84704-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: No cancellation token parameter.</span></span>

#### <a name="viable-alternative"></a><span data-ttu-id="84704-148">実行可能な代替手段:</span><span class="sxs-lookup"><span data-stu-id="84704-148">Viable alternative:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

<span data-ttu-id="84704-149">`TryGetNext` は、同期的に使用できる限り、1つのインターフェイス呼び出しで項目を使用するために、内側のループで使用されます。</span><span class="sxs-lookup"><span data-stu-id="84704-149">`TryGetNext` is used in an inner loop to consume items with a single interface call as long as they're available synchronously.</span></span>  <span data-ttu-id="84704-150">次の項目を同期的に取得できない場合、false が返され、false が返された場合は、呼び出し元は、次の項目が使用可能になるまで待機するか、別の項目がないことを確認するために `WaitForNextAsync` を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="84704-150">When the next item can't be retrieved synchronously, it returns false, and any time it returns false, a caller must subsequently invoke `WaitForNextAsync` to either wait for the next item to be available or to determine that there will never be another item.</span></span> <span data-ttu-id="84704-151">(追加の言語機能を使用しない) 一般的な消費は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="84704-151">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="84704-152">これの利点は、2つのフォールドと1つの重要な点です。</span><span class="sxs-lookup"><span data-stu-id="84704-152">The advantage of this is two-fold, one minor and one major:</span></span>
- <span data-ttu-id="84704-153">_Minor: 列挙子が複数のコンシューマーをサポートできるように_します。</span><span class="sxs-lookup"><span data-stu-id="84704-153">_Minor: Allows for an enumerator to support multiple consumers_.</span></span> <span data-ttu-id="84704-154">複数の同時実行コンシューマーをサポートする列挙子にとって価値があるシナリオもあります。</span><span class="sxs-lookup"><span data-stu-id="84704-154">There may be scenarios where it's valuable for an enumerator to support multiple concurrent consumers.</span></span>  <span data-ttu-id="84704-155">`MoveNextAsync` と `Current` が分離されているため、実装で使用できないようにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="84704-155">That can't be achieved when `MoveNextAsync` and `Current` are separate such that an implementation can't make their usage atomic.</span></span>  <span data-ttu-id="84704-156">これに対し、この方法では、列挙子を前方にプッシュして次の項目を取得することをサポートする1つのメソッド `TryGetNext` が提供されるため、必要に応じて列挙子を有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="84704-156">In contrast, this approach provides a single method `TryGetNext` that supports pushing the enumerator forward and getting the next item, so the enumerator can enable atomicity if desired.</span></span>  <span data-ttu-id="84704-157">ただし、このようなシナリオは、各コンシューマーに、共有されている列挙可能な独自の列挙子を与えることで有効にすることもできます。</span><span class="sxs-lookup"><span data-stu-id="84704-157">However, it's likely that such scenarios could also be enabled by giving each consumer its own enumerator from a shared enumerable.</span></span>  <span data-ttu-id="84704-158">さらに、すべての列挙子が同時使用をサポートしないようにすることをお勧めします。これにより、ほとんどの場合、これを必要としない大部分のオーバーヘッドが発生します。つまり、インターフェイスのコンシューマーは一般にこのような方法に依存することはできません。</span><span class="sxs-lookup"><span data-stu-id="84704-158">Further, we don't want to enforce that every enumerator support concurrent usage, as that would add non-trivial overheads to the majority case that doesn't require it, which means a consumer of the interface generally couldn't rely on this any way.</span></span>
- <span data-ttu-id="84704-159">_Major: パフォーマンス_。</span><span class="sxs-lookup"><span data-stu-id="84704-159">_Major: Performance_.</span></span> <span data-ttu-id="84704-160">`MoveNextAsync`の /`Current` アプローチでは、操作ごとに2つのインターフェイス呼び出しが必要ですが、`WaitForNextAsync`/の最適なケースは、ほとんどの反復処理が同期的に完了し、操作ごとに1つのインターフェイス呼び出しのみが可能な、`TryGetNext` を使用して厳密な内側ループを有効にすることです。`TryGetNext`</span><span class="sxs-lookup"><span data-stu-id="84704-160">The `MoveNextAsync`/`Current` approach requires two interface calls per operation, whereas the best case for `WaitForNextAsync`/`TryGetNext` is that most iterations complete synchronously, enabling a tight inner loop with `TryGetNext`, such that we only have one interface call per operation.</span></span>  <span data-ttu-id="84704-161">これは、インターフェイス呼び出しが計算を独占する場合に、大きな影響を与える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="84704-161">This can have a measurable impact in situations where the interface calls dominate the computation.</span></span>

<span data-ttu-id="84704-162">ただし、これらを手動で使用する場合の複雑さが大幅に増加し、それらを使用したときにバグが発生する可能性が高くなるなど、重要ではない欠点があります。</span><span class="sxs-lookup"><span data-stu-id="84704-162">However, there are non-trivial downsides, including significantly increased complexity when consuming these manually, and an increased chance of introducing bugs when using them.</span></span>  <span data-ttu-id="84704-163">また、パフォーマンス上の利点はマイクロベンチマークにも反映されていますが、実際の使用の大部分にインパクトされるとは思えません。</span><span class="sxs-lookup"><span data-stu-id="84704-163">And while the performance benefits show up in microbenchmarks, we don't believe they'll be impactful in the vast majority of real usage.</span></span>  <span data-ttu-id="84704-164">そのようなことが判明した場合は、2番目のインターフェイスセットを軽量な方法で導入できます。</span><span class="sxs-lookup"><span data-stu-id="84704-164">If it turns out they are, we can introduce a second set of interfaces in a light-up fashion.</span></span>

<span data-ttu-id="84704-165">破棄されるオプション:</span><span class="sxs-lookup"><span data-stu-id="84704-165">Discarded options considered:</span></span>
- <span data-ttu-id="84704-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` パラメーターを共変にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="84704-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parameters can't be covariant.</span></span>  <span data-ttu-id="84704-167">ここでは、参照型の結果に対してランタイム書き込みバリアが発生する可能性があるという小さな影響もあります (一般的な try パターンの問題)。</span><span class="sxs-lookup"><span data-stu-id="84704-167">There's also a small impact here (an issue with the try pattern in general) that this likely incurs a runtime write barrier for reference type results.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="84704-168">キャンセル</span><span class="sxs-lookup"><span data-stu-id="84704-168">Cancellation</span></span>

<span data-ttu-id="84704-169">キャンセルをサポートするには、いくつかの方法が考えられます。</span><span class="sxs-lookup"><span data-stu-id="84704-169">There are several possible approaches to supporting cancellation:</span></span>
1. <span data-ttu-id="84704-170">`IAsyncEnumerable<T>`の /`IAsyncEnumerator<T>` はキャンセルに依存しません。 `CancellationToken` はどこにも表示されません。</span><span class="sxs-lookup"><span data-stu-id="84704-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` are cancellation-agnostic: `CancellationToken` doesn't appear anywhere.</span></span>  <span data-ttu-id="84704-171">取り消しを行うには、任意の方法で列挙型または列挙子に `CancellationToken` を論理的に焼きます。たとえば、反復子を呼び出すときに、反復子メソッドの引数として `CancellationToken` を渡し、反復子の本体でその他のパラメーターと共に使用します。</span><span class="sxs-lookup"><span data-stu-id="84704-171">Cancellation is achieved by logically baking the `CancellationToken` into the enumerable and/or enumerator in whatever manner is appropriate, e.g. when calling an iterator, passing the `CancellationToken` as an argument to the iterator method and using it in the body of the iterator, as is done with any other parameter.</span></span>
2. <span data-ttu-id="84704-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: `GetAsyncEnumerator`に `CancellationToken` を渡すと、それ以降の `MoveNextAsync` 操作は可能です。</span><span class="sxs-lookup"><span data-stu-id="84704-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: You pass a `CancellationToken` to `GetAsyncEnumerator`, and subsequent `MoveNextAsync` operations respect it however it can.</span></span>
3. <span data-ttu-id="84704-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: 個々の `MoveNextAsync` の呼び出しに `CancellationToken` を渡します。</span><span class="sxs-lookup"><span data-stu-id="84704-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: You pass a `CancellationToken` to each individual `MoveNextAsync` call.</span></span>
4. <span data-ttu-id="84704-174">1 & & 2: `CancellationToken`s を列挙可能な/列挙子に埋め込み、`CancellationToken`を `GetAsyncEnumerator`に渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="84704-174">1 && 2: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `GetAsyncEnumerator`.</span></span>
5. <span data-ttu-id="84704-175">1 & & 3: `CancellationToken`s を列挙可能な/列挙子に埋め込み、`CancellationToken`を `MoveNextAsync`に渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="84704-175">1 && 3: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `MoveNextAsync`.</span></span>

<span data-ttu-id="84704-176">純粋な理論上の観点からは、(5) が最も堅牢です。つまり、(a) `CancellationToken` を受け入れる `MoveNextAsync` によって、キャンセルされたものを最も細かく制御できます。 (b) `CancellationToken` は、反復子に引数として渡すことができる他の型、任意の型に埋め込むことができます。</span><span class="sxs-lookup"><span data-stu-id="84704-176">From a purely theoretical perspective, (5) is the most robust, in that (a) `MoveNextAsync` accepting a `CancellationToken` enables the most fine-grained control over what's canceled, and (b) `CancellationToken` is just any other type that can passed as an argument into iterators, embedded in arbitrary types, etc.</span></span>

<span data-ttu-id="84704-177">ただし、この方法には複数の問題があります。</span><span class="sxs-lookup"><span data-stu-id="84704-177">However, there are multiple problems with that approach:</span></span>
- <span data-ttu-id="84704-178">`CancellationToken` がに渡されて、反復子の本体に `GetAsyncEnumerator` される方法を教えてください。</span><span class="sxs-lookup"><span data-stu-id="84704-178">How does a `CancellationToken` passed to `GetAsyncEnumerator` make it into the body of the iterator?</span></span>  <span data-ttu-id="84704-179">`GetEnumerator`に渡される `CancellationToken` にアクセスするために、をドットで切ることができる新しい `iterator` キーワードを公開できます。しかし、a) これは多くの追加メカニズムです。 b) これは非常にファーストクラスの市民になります。 c) 99% のケースは、反復子を呼び出して `GetAsyncEnumerator` を呼び出すのと同じコードであるように見えます。この場合、`CancellationToken` を引数としてメソッドに渡すだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="84704-179">We could expose a new `iterator` keyword that you could dot off of to get access to the `CancellationToken` passed to `GetEnumerator`, but a) that's a lot of additional machinery, b) we're making it a very first-class citizen, and c) the 99% case would seem to be the same code both calling an iterator and calling `GetAsyncEnumerator` on it, in which case it can just pass the `CancellationToken` as an argument into the method.</span></span>
- <span data-ttu-id="84704-180">`CancellationToken` がメソッドの本体に渡される `MoveNextAsync` に渡す方法を教えてください。</span><span class="sxs-lookup"><span data-stu-id="84704-180">How does a `CancellationToken` passed to `MoveNextAsync` get into the body of the method?</span></span>  <span data-ttu-id="84704-181">これは、`iterator` ローカルオブジェクトから公開されているかのように、その値が待機中に変化する可能性があるということです。これは、トークンに登録されているすべてのコードが、待機する前にそのトークンを登録解除してから再登録する必要があることを意味します。また、コンパイラによって反復子に実装されているか、開発者によって手動で実装されているかに関係なく、`MoveNextAsync` のすべての呼び出しで登録と登録解除を行う必要がある場合もあります。</span><span class="sxs-lookup"><span data-stu-id="84704-181">This is even worse, as if it's exposed off of an `iterator` local object, its value could change across awaits, which means any code that registered with the token would need to unregister from it prior to awaits and then re-register after; it's also potentially quite expensive to need to do such registering and unregistering in every `MoveNextAsync` call, regardless of whether implemented by the compiler in an iterator or by a developer manually.</span></span>
- <span data-ttu-id="84704-182">開発者は `foreach` ループをキャンセルするにはどうすればよいですか。</span><span class="sxs-lookup"><span data-stu-id="84704-182">How does a developer cancel a `foreach` loop?</span></span>  <span data-ttu-id="84704-183">列挙可能な/列挙子に `CancellationToken` を与えることによって実行される場合、または、列挙子に対して `foreach`' をサポートする必要があります。これにより、これらのクラスがファーストクラスの市民になるようになりました。列挙子 (LINQ メソッドなど) や b などに基づいて構築されたエコシステムについて考える必要があります。次に、返された構造体に `GetAsyncEnumerator` するときに、指定され `CancellationToken` たトークンを格納する `IAsyncEnumerable<T>` の `WithCancellation` 拡張メソッドを使用します `GetAsyncEnumerator`が呼び出されます (そのトークンは無視されます)。</span><span class="sxs-lookup"><span data-stu-id="84704-183">If it's done by giving a `CancellationToken` to an enumerable/enumerator, then either a) we need to support `foreach`'ing over enumerators, which raises them to being first-class citizens, and now you need to start thinking about an ecosystem built up around enumerators (e.g. LINQ methods) or b) we need to embed the `CancellationToken` in the enumerable anyway by having some `WithCancellation` extension method off of `IAsyncEnumerable<T>` that would store the provided token and then pass it into  the wrapped enumerable's `GetAsyncEnumerator` when the `GetAsyncEnumerator` on the returned struct is invoked (ignoring that token).</span></span>  <span data-ttu-id="84704-184">または、foreach の本文にある `CancellationToken` を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="84704-184">Or, you can just use the `CancellationToken` you have in the body of the foreach.</span></span>
- <span data-ttu-id="84704-185">クエリの包含がサポートされている場合、`GetEnumerator` または `MoveNextAsync` に渡された `CancellationToken` はどのようにして各句に渡されますか?</span><span class="sxs-lookup"><span data-stu-id="84704-185">If/when query comprehensions are supported, how would the `CancellationToken` supplied to `GetEnumerator` or `MoveNextAsync` be passed into each clause?</span></span>  <span data-ttu-id="84704-186">最も簡単な方法は、単に句をキャプチャすることです。この時点で、`GetAsyncEnumerator`/`MoveNextAsync` に渡されるトークンは無視されます。</span><span class="sxs-lookup"><span data-stu-id="84704-186">The easiest way would simply be for the clause to capture it, at which point whatever token is passed to `GetAsyncEnumerator`/`MoveNextAsync` is ignored.</span></span>

<span data-ttu-id="84704-187">このドキュメントの以前のバージョンでは、(1) が推奨されていましたが、(4) に切り替えました。</span><span class="sxs-lookup"><span data-stu-id="84704-187">An earlier version of this document recommended (1), but we since switched to (4).</span></span>

<span data-ttu-id="84704-188">(1) の主な問題は次の2つです。</span><span class="sxs-lookup"><span data-stu-id="84704-188">The two main problems with (1):</span></span>
- <span data-ttu-id="84704-189">キャンセル可能な列挙体のプロデューサーは、いくつかの定型を実装する必要があり、コンパイラによる非同期反復子のサポートのみを利用して `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` メソッドを実装できます。</span><span class="sxs-lookup"><span data-stu-id="84704-189">producers of cancellable enumerables have to implement some boilerplate, and can only leverage the compiler's support for async-iterators to implement a `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` method.</span></span>
- <span data-ttu-id="84704-190">多くのプロデューサーでは、非同期の列挙可能なシグネチャに `CancellationToken` パラメーターを追加することが必要になる可能性があります。これにより、コンシューマーは、`IAsyncEnumerable` の種類を指定したときに必要なキャンセルトークンを渡すことができなくなります。</span><span class="sxs-lookup"><span data-stu-id="84704-190">it is likely that many producers would be tempted to just add a `CancellationToken` parameter to their async-enumerable signature instead, which will prevent consumers from passing the cancellation token they want when they are given an `IAsyncEnumerable` type.</span></span>

<span data-ttu-id="84704-191">主に次の2つのシナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="84704-191">There are two main consumption scenarios:</span></span>
1. <span data-ttu-id="84704-192">コンシューマーが非同期反復子メソッドを呼び出す場所 `await foreach (var i in GetData(token)) ...`</span><span class="sxs-lookup"><span data-stu-id="84704-192">`await foreach (var i in GetData(token)) ...` where the consumer calls the async-iterator method,</span></span>
2. <span data-ttu-id="84704-193">コンシューマーが特定の `IAsyncEnumerable` インスタンスを処理する `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...`。</span><span class="sxs-lookup"><span data-stu-id="84704-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` where the consumer deals with a given `IAsyncEnumerable` instance.</span></span>

<span data-ttu-id="84704-194">非同期ストリームのプロデューサーとコンシューマーの両方にとって便利な方法で両方のシナリオをサポートするための適切な妥協点は、async iterator メソッドで特別に注釈が付けられたパラメーターを使用することです。</span><span class="sxs-lookup"><span data-stu-id="84704-194">We find that a reasonable compromise to support both scenarios in a way that is convenient for both producers and consumers of async-streams is to use a specially annotated parameter in the async-iterator method.</span></span> <span data-ttu-id="84704-195">この目的には、`[EnumeratorCancellation]` 属性が使用されます。</span><span class="sxs-lookup"><span data-stu-id="84704-195">The `[EnumeratorCancellation]` attribute is used for this purpose.</span></span> <span data-ttu-id="84704-196">この属性をパラメーターに配置すると、トークンが `GetAsyncEnumerator` メソッドに渡される場合に、パラメーターに渡された値の代わりにそのトークンを使用する必要があることをコンパイラに指示します。</span><span class="sxs-lookup"><span data-stu-id="84704-196">Placing this attribute on a parameter tells the compiler that if a token is passed to the `GetAsyncEnumerator` method, that token should be used instead of the value originally passed for the parameter.</span></span>

<span data-ttu-id="84704-197">`IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)` を検討します。</span><span class="sxs-lookup"><span data-stu-id="84704-197">Consider `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span></span> <span data-ttu-id="84704-198">このメソッドの実装者は、メソッドの本体でパラメーターを使用するだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="84704-198">The implementer of this method can simply use the parameter in the method body.</span></span> <span data-ttu-id="84704-199">コンシューマーは、上記のいずれかの消費パターンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="84704-199">The consumer can use either consumption patterns above:</span></span>
1. <span data-ttu-id="84704-200">`GetData(token)`を使用すると、トークンは非同期の列挙型に保存され、イテレーションで使用されます。</span><span class="sxs-lookup"><span data-stu-id="84704-200">if you use `GetData(token)`, then the token is saved into the async-enumerable and will be used in iteration,</span></span>
2. <span data-ttu-id="84704-201">`givenIAsyncEnumerable.WithCancellation(token)`を使用すると、`GetAsyncEnumerator` に渡されたトークンは、非同期の列挙可能なトークンに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="84704-201">if you use `givenIAsyncEnumerable.WithCancellation(token)`, then the token passed to `GetAsyncEnumerator` will supersede any token saved in the async-enumerable.</span></span>

## <a name="foreach"></a><span data-ttu-id="84704-202">foreach</span><span class="sxs-lookup"><span data-stu-id="84704-202">foreach</span></span>

<span data-ttu-id="84704-203">`foreach` は、`IEnumerable<T>`の既存のサポートに加えて、`IAsyncEnumerable<T>` をサポートするように強化されます。</span><span class="sxs-lookup"><span data-stu-id="84704-203">`foreach` will be augmented to support `IAsyncEnumerable<T>` in addition to its existing support for `IEnumerable<T>`.</span></span>  <span data-ttu-id="84704-204">また、関連するメンバーがパブリックに公開されている場合は、インターフェイスを直接使用することを避けるために、関連するメンバーがパブリックに公開されている場合は、インターフェイスを直接使用する `IAsyncEnumerable<T>` のではなく、`MoveNextAsync` と `DisposeAsync`の戻り値の型として代替の awaitables 使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="84704-204">And it will support the equivalent of `IAsyncEnumerable<T>` as a pattern if the relevant members are exposed publicly, falling back to using the interface directly if not, in order to enable struct-based extensions that avoid allocating as well as using alternative awaitables as the return type of `MoveNextAsync` and `DisposeAsync`.</span></span>

### <a name="syntax"></a><span data-ttu-id="84704-205">構文</span><span class="sxs-lookup"><span data-stu-id="84704-205">Syntax</span></span>

<span data-ttu-id="84704-206">次の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="84704-206">Using the syntax:</span></span>

```csharp
foreach (var i in enumerable)
```

<span data-ttu-id="84704-207">C#は、`enumerable` を同期的な列挙可能として処理し続けます。これにより、非同期列挙体 (パターンの公開またはインターフェイスの実装) に関連する Api を公開する場合でも、同期 Api のみが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="84704-207">C# will continue to treat `enumerable` as a synchronous enumerable, such that even if it exposes the relevant APIs for async enumerables (exposing the pattern or implementing the interface), it will only consider the synchronous APIs.</span></span>

<span data-ttu-id="84704-208">非同期 Api のみを考慮するように `foreach` を強制するために、`await` は次のように挿入されます。</span><span class="sxs-lookup"><span data-stu-id="84704-208">To force `foreach` to instead only consider the asynchronous APIs, `await` is inserted as follows:</span></span>

```csharp
await foreach (var i in enumerable)
```

<span data-ttu-id="84704-209">非同期 Api または同期 Api の使用をサポートする構文は提供されません。開発者は、使用する構文に基づいて選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="84704-209">No syntax would be provided that would support using either the async or the sync APIs; the developer must choose based on the syntax used.</span></span>

<span data-ttu-id="84704-210">破棄されるオプション:</span><span class="sxs-lookup"><span data-stu-id="84704-210">Discarded options considered:</span></span>
- <span data-ttu-id="84704-211">_`foreach (var i in await enumerable)`_ : これは既に有効な構文です。その意味を変更すると、互換性に影響する変更になります。</span><span class="sxs-lookup"><span data-stu-id="84704-211">_`foreach (var i in await enumerable)`_: This is already valid syntax, and changing its meaning would be a breaking change.</span></span>  <span data-ttu-id="84704-212">つまり、`enumerable`を `await` し、同期的に反復可能なされたものを取得してから、同期的に反復処理します。</span><span class="sxs-lookup"><span data-stu-id="84704-212">This means to `await` the `enumerable`, get back something synchronously iterable from it, and then synchronously iterate through that.</span></span>
- <span data-ttu-id="84704-213">_`foreach (var i await in enumerable)`、`foreach (var await i in enumerable)`、`foreach (await var i in enumerable)`_ : これらはすべて、次の項目を待機していることを示していますが、foreach には他の待機が含まれています。特に、列挙型が `IAsyncDisposable`の場合は、非同期の破棄を `await`します。</span><span class="sxs-lookup"><span data-stu-id="84704-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_: These all suggest that we're awaiting the next item, but there are other awaits involved in foreach, in particular if the enumerable is an `IAsyncDisposable`, we will be `await`'ing its async disposal.</span></span>  <span data-ttu-id="84704-214">この await は、個々の要素に対してではなく foreach のスコープとして使用されます。したがって、`await` キーワードは `foreach` レベルである必要があります。</span><span class="sxs-lookup"><span data-stu-id="84704-214">That await is as the scope of the foreach rather than for each individual element, and thus the `await` keyword deserves to be at the `foreach` level.</span></span>  <span data-ttu-id="84704-215">さらに、`foreach` に関連付けられている場合は、"await foreach" など、別の用語で `foreach` を記述する方法が提供されます。</span><span class="sxs-lookup"><span data-stu-id="84704-215">Further, having it associated with the `foreach` gives us a way to describe the `foreach` with a different term, e.g. a "await foreach".</span></span>  <span data-ttu-id="84704-216">しかし、さらに重要なのは、`foreach` 構文を `using` 構文と同時に考慮して、相互に一貫性を保ち、`using (await ...)` が既に有効な構文であるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="84704-216">But more importantly, there's value in considering `foreach` syntax at the same time as `using` syntax, so that they remain consistent with each other, and `using (await ...)` is already valid syntax.</span></span>
- _`foreach await (var i in enumerable)`_

<span data-ttu-id="84704-217">引き続き検討してください。</span><span class="sxs-lookup"><span data-stu-id="84704-217">Still to consider:</span></span>
- <span data-ttu-id="84704-218">`foreach` 現在、列挙子の反復処理はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="84704-218">`foreach` today does not support iterating through an enumerator.</span></span>  <span data-ttu-id="84704-219">`IAsyncEnumerator<T>`が渡される方が一般的であると考えられます。したがって、`IAsyncEnumerable<T>` と `IAsyncEnumerator<T>`の両方で `await foreach` のサポートが必要になります。</span><span class="sxs-lookup"><span data-stu-id="84704-219">We expect it will be more common to have `IAsyncEnumerator<T>`s handed around, and thus it's tempting to support `await foreach` with both `IAsyncEnumerable<T>` and `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="84704-220">ただし、このようなサポートを追加すると、`IAsyncEnumerator<T>` がファーストクラスの市民であるかどうか、および列挙体に加えて列挙子を操作する連結子のオーバーロードが必要かどうかの質問が導入されます。</span><span class="sxs-lookup"><span data-stu-id="84704-220">But once we add such support, it introduces the question of whether `IAsyncEnumerator<T>` is a first-class citizen, and whether we need to have overloads of combinators that operate on enumerators in addition to enumerables?</span></span>    <span data-ttu-id="84704-221">列挙体ではなく列挙子を返すメソッドを推奨しますか。</span><span class="sxs-lookup"><span data-stu-id="84704-221">Do we want to encourage methods to return enumerators rather than enumerables?</span></span> <span data-ttu-id="84704-222">この点については、引き続き説明します。</span><span class="sxs-lookup"><span data-stu-id="84704-222">We should continue to discuss this.</span></span>  <span data-ttu-id="84704-223">サポートしないと判断した場合は、列挙子を引き続き `foreach`できる拡張メソッド `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` を導入することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="84704-223">If we decide we don't want to support it, we might want to introduce an extension method `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` that would allow an enumerator to still be `foreach`'d.</span></span>  <span data-ttu-id="84704-224">サポートを希望する場合は、`await foreach` が列挙子で `DisposeAsync` を呼び出す必要があるかどうかを決定する必要があります。また、回答が "いいえ" であると考えられる場合は、破棄の制御は `GetEnumerator`を呼び出した人によって処理される必要があります。</span><span class="sxs-lookup"><span data-stu-id="84704-224">If we decide we do want to support it, we'll need to also decide on whether the `await foreach` would be responsible for calling `DisposeAsync` on the enumerator, and the answer is likely "no, control over disposal should be handled by whoever called `GetEnumerator`."</span></span>

### <a name="pattern-based-compilation"></a><span data-ttu-id="84704-225">パターンに基づくコンパイル</span><span class="sxs-lookup"><span data-stu-id="84704-225">Pattern-based Compilation</span></span>

<span data-ttu-id="84704-226">コンパイラは、存在する場合はパターンベースの Api にバインドし、インターフェイスを使用してこれらを優先します (パターンはインスタンスメソッドまたは拡張メソッドで満たされる場合があります)。</span><span class="sxs-lookup"><span data-stu-id="84704-226">The compiler will bind to the pattern-based APIs if they exist, preferring those over using the interface (the pattern may be satisfied with instance methods or extension methods).</span></span>  <span data-ttu-id="84704-227">パターンの要件は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="84704-227">The requirements for the pattern are:</span></span>
- <span data-ttu-id="84704-228">列挙可能なは、引数なしで呼び出される可能性があり、関連するパターンを満たす列挙子を返す `GetAsyncEnumerator` メソッドを公開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="84704-228">The enumerable must expose a `GetAsyncEnumerator` method that may be called with no arguments and that returns an enumerator that meets the relevant pattern.</span></span>
- <span data-ttu-id="84704-229">列挙子は、引数なしで呼び出される可能性のある `MoveNextAsync` メソッドを公開する必要があります。このメソッドは、`await`ed で、`GetResult()` が `bool`を返す可能性があるものを返します。</span><span class="sxs-lookup"><span data-stu-id="84704-229">The enumerator must expose a `MoveNextAsync` method that may be called with no arguments and that returns something which may be `await`ed and whose `GetResult()` returns a `bool`.</span></span>
- <span data-ttu-id="84704-230">列挙子は、列挙されるデータの種類を表す `T` を返す getter を持つ `Current` プロパティも公開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="84704-230">The enumerator must also expose `Current` property whose getter returns a `T` representing the kind of data being enumerated.</span></span>
- <span data-ttu-id="84704-231">列挙子は、必要に応じて、引数を指定せずに呼び出すことができ、ed `await`できるものを返し、その `GetResult()` が `void`を返す可能性のある `DisposeAsync` メソッドを公開する場合があります。</span><span class="sxs-lookup"><span data-stu-id="84704-231">The enumerator may optionally expose a `DisposeAsync` method that may be invoked with no arguments and that returns something that can be `await`ed and whose `GetResult()` returns `void`.</span></span>

<span data-ttu-id="84704-232">このコードによって以下が行われます。</span><span class="sxs-lookup"><span data-stu-id="84704-232">This code:</span></span>

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

<span data-ttu-id="84704-233">はに相当するものに変換されます。</span><span class="sxs-lookup"><span data-stu-id="84704-233">is translated to the equivalent of:</span></span>

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

<span data-ttu-id="84704-234">反復される型が適切パターンを公開していない場合は、インターフェイスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="84704-234">If the iterated type doesn't expose the right pattern, the interfaces will be used.</span></span>

### <a name="configureawait"></a><span data-ttu-id="84704-235">ConfigureAwait</span><span class="sxs-lookup"><span data-stu-id="84704-235">ConfigureAwait</span></span>

<span data-ttu-id="84704-236">このパターンに基づくコンパイルでは、`ConfigureAwait` の拡張メソッドを使用して、すべての待機で `ConfigureAwait` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="84704-236">This pattern-based compilation will allow `ConfigureAwait` to be used on all of the awaits, via a `ConfigureAwait` extension method:</span></span>

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

<span data-ttu-id="84704-237">これは .NET にも追加する型に基づいて行われます。これは、おそらく、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="84704-237">This will be based on types we'll add to .NET as well, likely to System.Threading.Tasks.Extensions.dll:</span></span>

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

<span data-ttu-id="84704-238">このアプローチでは、パターンベースの列挙体で `ConfigureAwait` を使用できないことに注意してください。ここでも、`ConfigureAwait` は `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` の拡張機能としてのみ公開されており、任意の待機可能には適用できません。これは、タスクに適用した場合 (タスクの継続サポートに実装されている動作を制御します)、待機可能がタスクでない可能性があるパターンを使用する場合には意味が</span><span class="sxs-lookup"><span data-stu-id="84704-238">Note that this approach will not enable `ConfigureAwait` to be used with pattern-based enumerables, but then again it's already the case that the `ConfigureAwait` is only exposed as an extension on `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` and can't be applied to arbitrary awaitable things, as it only makes sense when applied to Tasks (it controls a behavior implemented in Task's continuation support), and thus doesn't make sense when using a pattern where the awaitable things may not be tasks.</span></span>  <span data-ttu-id="84704-239">待機可能を返す人は、このような高度なシナリオで独自のカスタム動作を提供できます。</span><span class="sxs-lookup"><span data-stu-id="84704-239">Anyone returning awaitable things can provide their own custom behavior in such advanced scenarios.</span></span>

<span data-ttu-id="84704-240">(スコープまたはアセンブリレベルの `ConfigureAwait` ソリューションをサポートする何らかの方法を使用できる場合は、これは必要ありません)。</span><span class="sxs-lookup"><span data-stu-id="84704-240">(If we can come up with some way to support a scope- or assembly-level `ConfigureAwait` solution, then this won't be necessary.)</span></span>

## <a name="async-iterators"></a><span data-ttu-id="84704-241">非同期反復子</span><span class="sxs-lookup"><span data-stu-id="84704-241">Async Iterators</span></span>

<span data-ttu-id="84704-242">言語/コンパイラは、その使用に加えて `IAsyncEnumerable<T>`s と `IAsyncEnumerator<T>`の生成をサポートします。</span><span class="sxs-lookup"><span data-stu-id="84704-242">The language / compiler will support producing `IAsyncEnumerable<T>`s and `IAsyncEnumerator<T>`s in addition to consuming them.</span></span> <span data-ttu-id="84704-243">現在、この言語では次のような反復子の作成がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="84704-243">Today the language supports writing an iterator like:</span></span>

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="84704-244">ただし `await` は、これらの反復子の本体では使用できません。</span><span class="sxs-lookup"><span data-stu-id="84704-244">but `await` can't be used in the body of these iterators.</span></span>  <span data-ttu-id="84704-245">サポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="84704-245">We will add that support.</span></span>

### <a name="syntax"></a><span data-ttu-id="84704-246">構文</span><span class="sxs-lookup"><span data-stu-id="84704-246">Syntax</span></span>

<span data-ttu-id="84704-247">反復子に対する既存の言語サポートは、`yield`が含まれているかどうかに基づいて、メソッドの反復子の性質を推論します。</span><span class="sxs-lookup"><span data-stu-id="84704-247">The existing language support for iterators infers the iterator nature of the method based on whether it contains any `yield`s.</span></span>  <span data-ttu-id="84704-248">非同期反復子についても同じことが当てはまります。</span><span class="sxs-lookup"><span data-stu-id="84704-248">The same will be true for async iterators.</span></span>  <span data-ttu-id="84704-249">このような非同期反復子は、シグネチャに `async` を追加することによって同期反復子と区別され、戻り値の型として `IAsyncEnumerable<T>` または `IAsyncEnumerator<T>` を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="84704-249">Such async iterators will be demarcated and differentiated from synchronous iterators via adding `async` to the signature, and must then also have either `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` as its return type.</span></span>  <span data-ttu-id="84704-250">たとえば、上の例は、次のように、非同期反復子として記述できます。</span><span class="sxs-lookup"><span data-stu-id="84704-250">For example, the above example could be written as an async iterator as follows:</span></span>

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="84704-251">検討対象の候補:</span><span class="sxs-lookup"><span data-stu-id="84704-251">Alternatives considered:</span></span>
- <span data-ttu-id="84704-252">_シグネチャで `async` を使用しない_: `async` を使用すると、そのコンテキストで `await` が有効かどうかを判断するために使用されるため、コンパイラによって技術的に要求される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="84704-252">_Not using `async` in the signature_: Using `async` is likely technically required by the compiler, as it uses it to determine whether `await` is valid in that context.</span></span>  <span data-ttu-id="84704-253">ただし、必要でない場合でも、`await` が `async`としてマークされたメソッドでのみ使用されることがあり、一貫性を保つことが重要であるように思えました。</span><span class="sxs-lookup"><span data-stu-id="84704-253">But even if it's not required, we've established that `await` may only be used in methods marked as `async`, and it seems important to keep the consistency.</span></span>
- <span data-ttu-id="84704-254">_`IAsyncEnumerable<T>`に対してカスタムビルダーを有効_にすると、これが将来のものになりますが、機械は複雑になり、対応する同期に対してはサポートされません。</span><span class="sxs-lookup"><span data-stu-id="84704-254">_Enabling custom builders for `IAsyncEnumerable<T>`_:  That's something we could look at for the future, but the machinery is complicated and we don't support that for the synchronous counterparts.</span></span>
- <span data-ttu-id="84704-255">_シグネチャに `iterator` キーワードを_指定すると、非同期反復子はシグネチャで `async iterator` を使用し、`yield` は `iterator`を含む `async` メソッドでのみ使用できます。`iterator` は、同期反復子でも省略可能になります。</span><span class="sxs-lookup"><span data-stu-id="84704-255">_Having an `iterator` keyword in the signature_: Async iterators would use `async iterator` in the signature, and `yield` could only be used in `async` methods that included `iterator`; `iterator` would then be made optional on synchronous iterators.</span></span>  <span data-ttu-id="84704-256">パースペクティブによっては、`yield` が許可されているかどうか、メソッドが `yield` を使用するかどうかに基づいて、コンパイラによる製造ではなく `IAsyncEnumerable<T>` 型のインスタンスを実際に返すかどうかを、メソッドのシグネチャによって明確にするという利点があります。</span><span class="sxs-lookup"><span data-stu-id="84704-256">Depending on your perspective, this has the benefit of making it very clear by the signature of the method whether `yield` is allowed and whether the method is actually meant to return instances of type `IAsyncEnumerable<T>` rather than the compiler manufacturing one based on whether the code uses `yield` or not.</span></span>  <span data-ttu-id="84704-257">ただし、これは同期反復子とは異なり、要求を要求することはできません。</span><span class="sxs-lookup"><span data-stu-id="84704-257">But it is different from synchronous iterators, which don't and can't be made to require one.</span></span>  <span data-ttu-id="84704-258">さらに、開発者によっては、余分な構文が気に入らない場合もあります。</span><span class="sxs-lookup"><span data-stu-id="84704-258">Plus some developers don't like the extra syntax.</span></span>  <span data-ttu-id="84704-259">最初からデザインする場合は、これが必要になることがありますが、この時点で、非同期反復子を同期反復子の近くに維持することには、より多くの価値があります。</span><span class="sxs-lookup"><span data-stu-id="84704-259">If we were designing it from scratch, we'd probably make this required, but at this point there's much more value in keeping async iterators close to sync iterators.</span></span>

## <a name="linq"></a><span data-ttu-id="84704-260">LINQ</span><span class="sxs-lookup"><span data-stu-id="84704-260">LINQ</span></span>

<span data-ttu-id="84704-261">`System.Linq.Enumerable` クラスには ~ 200 のオーバーロードがあり、これらはすべて `IEnumerable<T>`の観点で機能します。これらのうちのいくつかは `IEnumerable<T>`を受け入れますが、そのうちのいくつかは `IEnumerable<T>`を生成します。</span><span class="sxs-lookup"><span data-stu-id="84704-261">There are over ~200 overloads of methods on the `System.Linq.Enumerable` class, all of which work in terms of `IEnumerable<T>`; some of these accept `IEnumerable<T>`, some of them produce `IEnumerable<T>`, and many do both.</span></span>  <span data-ttu-id="84704-262">`IAsyncEnumerable<T>` の LINQ サポートを追加すると、このようなオーバーロードをすべて複製することが必要になる可能性があります。これは、別の ~ 200 です。</span><span class="sxs-lookup"><span data-stu-id="84704-262">Adding LINQ support for `IAsyncEnumerable<T>` would likely entail duplicating all of these overloads for it, for another ~200.</span></span>  <span data-ttu-id="84704-263">`IAsyncEnumerator<T>` は、同期の世界にいる `IEnumerator<T>` よりも、非同期環境ではスタンドアロンエンティティとして一般的になる可能性が高いため、`IAsyncEnumerator<T>`で動作する別の ~ 200 のオーバーロードが必要になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="84704-263">And since `IAsyncEnumerator<T>` is likely to be more common as a standalone entity in the asynchronous world than `IEnumerator<T>` is in the synchronous world, we could potentially need another ~200 overloads that work with `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="84704-264">さらに、多くのオーバーロードは、述語 (`Func<T, bool>`を受け取る `Where` など) を処理します。また、同期述語と非同期述語の両方を処理する `IAsyncEnumerable<T>`ベースのオーバーロードを使用することをお勧めします (`Func<T, bool>`に加えて `Func<T, ValueTask<bool>>` など)。</span><span class="sxs-lookup"><span data-stu-id="84704-264">Plus, a large number of the overloads deal with predicates (e.g. `Where` that takes a `Func<T, bool>`), and it may be desirable to have `IAsyncEnumerable<T>`-based overloads that deal with both synchronous and asynchronous predicates (e.g. `Func<T, ValueTask<bool>>` in addition to `Func<T, bool>`).</span></span>  <span data-ttu-id="84704-265">これは、現在 ~ 400 の新しいオーバーロードには適用されませんが、大まかな計算として、半分に適用できることが挙げられます。これは、合計で600の新しいメソッドに対して、別の ~ 200 のオーバーロードを意味します。</span><span class="sxs-lookup"><span data-stu-id="84704-265">While this isn't applicable to all of the now ~400 new overloads, a rough calculation is that it'd be applicable to half, which means another ~200 overloads, for a total of ~600 new methods.</span></span>

<span data-ttu-id="84704-266">これは、対話的な拡張機能 (Ix) のような拡張ライブラリが検討されている場合に、さらに多くの Api を使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="84704-266">That is a staggering number of APIs, with the potential for even more when extension libraries like Interactive Extensions (Ix) are considered.</span></span>  <span data-ttu-id="84704-267">ただし、Ix にはこれらの多くの実装が既に存在しているので、その作業を複製するのに大きな理由があるとは思えません。代わりに、開発者が `IAsyncEnumerable<T>`で LINQ を使用する場合には、community を改善し、それを推奨することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="84704-267">But Ix already has an implementation of many of these, and there doesn't seem to be a great reason to duplicate that work; we should instead help the community improve Ix and recommend it for when developers want to use LINQ with `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="84704-268">クエリの読解構文にも問題があります。</span><span class="sxs-lookup"><span data-stu-id="84704-268">There is also the issue of query comprehension syntax.</span></span>  <span data-ttu-id="84704-269">クエリの包含のパターンベースの性質により、いくつかの演算子を使用することができます。たとえば、Ix に次のようなメソッドが用意されているとします。</span><span class="sxs-lookup"><span data-stu-id="84704-269">The pattern-based nature of query comprehensions would allow them to "just work" with some operators, e.g. if Ix provides the following methods:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

<span data-ttu-id="84704-270">次にC# 、このコードは "作業のみ" になります。</span><span class="sxs-lookup"><span data-stu-id="84704-270">then this C# code will "just work":</span></span>

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

<span data-ttu-id="84704-271">ただし、次の例のように、句での `await` の使用をサポートするクエリの読解構文はありません。</span><span class="sxs-lookup"><span data-stu-id="84704-271">However, there is no query comprehension syntax that supports using `await` in the clauses, so if Ix added, for example:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

<span data-ttu-id="84704-272">次に、これは "仕事のみ" になります。</span><span class="sxs-lookup"><span data-stu-id="84704-272">then this would "just work":</span></span>

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

<span data-ttu-id="84704-273">ただし、`select` 句に `await` インラインで書き込む方法はありません。</span><span class="sxs-lookup"><span data-stu-id="84704-273">but there'd be no way to write it with the `await` inline in the `select` clause.</span></span>  <span data-ttu-id="84704-274">別の作業として、`async { ... }` 式を言語に追加してみましょう。この時点で、クエリの包含での使用を許可できます。上記の式は、次のように記述できます。</span><span class="sxs-lookup"><span data-stu-id="84704-274">As a separate effort, we could look into adding `async { ... }` expressions to the language, at which point we could allow them to be used in query comprehensions and the above could instead be written as:</span></span>

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

<span data-ttu-id="84704-275">または、`async from`のサポートなどによって `await` を式で直接使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="84704-275">or to enabling `await` to be used directly in expressions, such as by supporting `async from`.</span></span>  <span data-ttu-id="84704-276">ただし、ここでは、機能セットの他の部分に影響を与える可能性はほとんどありません。これは、現時点では特に投資が重要な価値の高いものではないため、ここでは何も追加しないことを提案します。</span><span class="sxs-lookup"><span data-stu-id="84704-276">However, it's unlikely a design here would impact the rest of the feature set one way or the other, and this isn't a particularly high-value thing to invest in right now, so the proposal is to do nothing additional here right now.</span></span>

## <a name="integration-with-other-asynchronous-frameworks"></a><span data-ttu-id="84704-277">他の非同期フレームワークとの統合</span><span class="sxs-lookup"><span data-stu-id="84704-277">Integration with other asynchronous frameworks</span></span>

<span data-ttu-id="84704-278">`IObservable<T>` およびその他の非同期フレームワーク (リアクティブストリームなど) との統合は、言語レベルではなくライブラリレベルで実行されます。</span><span class="sxs-lookup"><span data-stu-id="84704-278">Integration with `IObservable<T>` and other asynchronous frameworks (e.g. reactive streams) would be done at the library level rather than at the language level.</span></span>  <span data-ttu-id="84704-279">たとえば、`IAsyncEnumerator<T>` のすべてのデータを `IObserver<T>` にパブリッシュするには、列挙子に対して "ing" を `await foreach`し、データをオブザーバーに `OnNext`するだけです。そのため、`AsObservable<T>` の拡張メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="84704-279">For example, all of the data from an `IAsyncEnumerator<T>` can be published to an `IObserver<T>` simply by `await foreach`'ing over the enumerator and `OnNext`'ing the data to the observer, so an `AsObservable<T>` extension method is possible.</span></span>  <span data-ttu-id="84704-280">`await foreach` で `IObservable<T>` を使用するには、データのバッファリングが必要です (前の項目がまだ処理されている間に別の項目がプッシュされた場合)。ただし、このようなプッシュプルアダプターを簡単に実装して、`IObservable<T>` を `IAsyncEnumerator<T>`でプルできるようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="84704-280">Consuming an `IObservable<T>` in a `await foreach` requires buffering the data (in case another item is pushed while the previous item is still being processing), but such a push-pull adapter can easily be implemented to enable an `IObservable<T>` to be pulled from with an `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="84704-281">等. Rx/Ix は、このような実装のプロトタイプを既に提供しています。また、 https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels などのライブラリでは、さまざまな種類のバッファリングデータ構造を提供しています。</span><span class="sxs-lookup"><span data-stu-id="84704-281">Etc.  Rx/Ix already provide prototypes of such implementations, and libraries like https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels provide various kinds of buffering data structures.</span></span>  <span data-ttu-id="84704-282">この段階では、言語を使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="84704-282">The language need not be involved at this stage.</span></span>
