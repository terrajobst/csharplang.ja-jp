---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483752"
---
# <a name="async-main"></a><span data-ttu-id="98355-101">Async Main</span><span class="sxs-lookup"><span data-stu-id="98355-101">Async Main</span></span>

* <span data-ttu-id="98355-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="98355-102">[x] Proposed</span></span>
* <span data-ttu-id="98355-103">[] プロトタイプ</span><span class="sxs-lookup"><span data-stu-id="98355-103">[ ] Prototype</span></span>
* <span data-ttu-id="98355-104">[] の実装</span><span class="sxs-lookup"><span data-stu-id="98355-104">[ ] Implementation</span></span>
* <span data-ttu-id="98355-105">[] 仕様</span><span class="sxs-lookup"><span data-stu-id="98355-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="98355-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="98355-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="98355-107">Entrypoint が `Task` / `Task<int>` を返し、`async`としてマークできるようにすることで、アプリケーションの Main/entrypoint メソッドで `await` を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="98355-107">Allow `await` to be used in an application's Main / entrypoint method by allowing the entrypoint to return `Task` / `Task<int>` and be marked `async`.</span></span>

## <a name="motivation"></a><span data-ttu-id="98355-108">目的</span><span class="sxs-lookup"><span data-stu-id="98355-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="98355-109">これはC#、コンソールベースのユーティリティを記述する場合や、メインから `async` メソッドを呼び出して `await` するために小さなテストアプリを作成する場合によく見られます。</span><span class="sxs-lookup"><span data-stu-id="98355-109">It is very common when learning C#, when writing console-based utilities, and when writing small test apps to want to call and `await` `async` methods from Main.</span></span>  <span data-ttu-id="98355-110">今日では、このような `await`を強制的に別の非同期メソッドで実行するように強制することで、複雑さのレベルを追加しています。そのため、開発者は、作業を開始するために次のような定型を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="98355-110">Today we add a level of complexity here by forcing such `await`'ing to be done in a separate async method, which causes developers to need to write boilerplate like the following just to get started:</span></span>

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

<span data-ttu-id="98355-111">この定型句の必要性を排除して、`await`を使用できるように、メイン自体を `async` できるようにするだけで簡単に開始できるようになります。</span><span class="sxs-lookup"><span data-stu-id="98355-111">We can remove the need for this boilerplate and make it easier to get started simply by allowing Main itself to be `async` such that `await`s can be used in it.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="98355-112">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="98355-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="98355-113">現在、次のシグネチャは entrypoints が許可されています。</span><span class="sxs-lookup"><span data-stu-id="98355-113">The following signatures are currently allowed entrypoints:</span></span>

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

<span data-ttu-id="98355-114">次のように、許可されている entrypoints の一覧を拡張します。</span><span class="sxs-lookup"><span data-stu-id="98355-114">We extend the list of allowed entrypoints to include:</span></span>

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

<span data-ttu-id="98355-115">互換性のリスクを回避するために、これらの新しい署名は、前のセットのオーバーロードが存在しない場合にのみ有効な entrypoints と見なされます。</span><span class="sxs-lookup"><span data-stu-id="98355-115">To avoid compatibility risks, these new signatures will only be considered as valid entrypoints if no overloads of the previous set are present.</span></span>
<span data-ttu-id="98355-116">言語/コンパイラでは、entrypoint を `async`としてマークする必要はありませんが、ほとんどの用途はそのようにマークされることが予想されます。</span><span class="sxs-lookup"><span data-stu-id="98355-116">The language / compiler will not require that the entrypoint be marked as `async`, though we expect the vast majority of uses will be marked as such.</span></span>

<span data-ttu-id="98355-117">これらのいずれかがエントリポイントとして識別されると、コンパイラは、次のコード化されたメソッドのいずれかを呼び出す実際の entrypoint メソッドを合成します。</span><span class="sxs-lookup"><span data-stu-id="98355-117">When one of these is identified as the entrypoint, the compiler will synthesize an actual entrypoint method that calls one of these coded methods:</span></span>
- <span data-ttu-id="98355-118">```static Task Main()``` によって、コンパイラはと同等のものを出力し ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="98355-118">```static Task Main()``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="98355-119">```static Task Main(string[])``` によって、コンパイラはと同等のものを出力し ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="98355-119">```static Task Main(string[])``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="98355-120">```static Task<int> Main()``` によって、コンパイラはと同等のものを出力し ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="98355-120">```static Task<int> Main()``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="98355-121">```static Task<int> Main(string[])``` によって、コンパイラはと同等のものを出力し ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="98355-121">```static Task<int> Main(string[])``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>

<span data-ttu-id="98355-122">使用例:</span><span class="sxs-lookup"><span data-stu-id="98355-122">Example usage:</span></span>

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a><span data-ttu-id="98355-123">短所</span><span class="sxs-lookup"><span data-stu-id="98355-123">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="98355-124">主な欠点は、単に追加のエントリポイントシグネチャをサポートする複雑さです。</span><span class="sxs-lookup"><span data-stu-id="98355-124">The main drawback is simply the additional complexity of supporting additional entrypoint signatures.</span></span>

## <a name="alternatives"></a><span data-ttu-id="98355-125">代替</span><span class="sxs-lookup"><span data-stu-id="98355-125">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="98355-126">考慮されたその他のバリアント:</span><span class="sxs-lookup"><span data-stu-id="98355-126">Other variants considered:</span></span>

<span data-ttu-id="98355-127">`async void`を許可します。</span><span class="sxs-lookup"><span data-stu-id="98355-127">Allowing `async void`.</span></span>  <span data-ttu-id="98355-128">このセマンティクスは、直接呼び出すコードでも同じにしておく必要があります。これにより、生成された entrypoint が呼び出しを行うのが困難になります (タスクは返されません)。</span><span class="sxs-lookup"><span data-stu-id="98355-128">We need to keep the semantics the same for code calling it directly, which would then make it difficult for a generated entrypoint to call it (no Task returned).</span></span>  <span data-ttu-id="98355-129">この問題を解決するには、次の2つのメソッドを生成します。</span><span class="sxs-lookup"><span data-stu-id="98355-129">We could solve this by generating two other methods, e.g.</span></span>

```csharp
public static async void Main()
{
   ... // await code
}
```

<span data-ttu-id="98355-130">→</span><span class="sxs-lookup"><span data-stu-id="98355-130">becomes</span></span>

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

<span data-ttu-id="98355-131">`async void`の使用を促進することにも懸念事項があります。</span><span class="sxs-lookup"><span data-stu-id="98355-131">There are also concerns around encouraging usage of `async void`.</span></span>

<span data-ttu-id="98355-132">名前として "Main" ではなく "MainAsync" を使用します。</span><span class="sxs-lookup"><span data-stu-id="98355-132">Using "MainAsync" instead of "Main" as the name.</span></span>  <span data-ttu-id="98355-133">タスクを返すメソッドには非同期サフィックスを使用することをお勧めしますが、主にライブラリ機能に関するものであり、Main はサポートされておらず、"Main" の後に追加のエントリポイント名をサポートすることは価値がありません。</span><span class="sxs-lookup"><span data-stu-id="98355-133">While the async suffix is recommended for Task-returning methods, that's primarily about library functionality, which Main is not, and supporting additional entrypoint names beyond "Main" is not worth it.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="98355-134">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="98355-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="98355-135">該当なし</span><span class="sxs-lookup"><span data-stu-id="98355-135">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="98355-136">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="98355-136">Design meetings</span></span>

<span data-ttu-id="98355-137">該当なし</span><span class="sxs-lookup"><span data-stu-id="98355-137">n/a</span></span>
