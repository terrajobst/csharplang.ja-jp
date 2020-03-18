---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483476"
---
# <a name="static-delegates"></a><span data-ttu-id="96694-101">静的デリゲート</span><span class="sxs-lookup"><span data-stu-id="96694-101">Static Delegates</span></span>

* <span data-ttu-id="96694-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="96694-102">[x] Proposed</span></span>
* <span data-ttu-id="96694-103">[] プロトタイプ: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="96694-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="96694-104">[] の実装: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="96694-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="96694-105">[] 仕様: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="96694-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="96694-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="96694-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="96694-107">汎用の軽量のコールバック機能をC#言語に提供します。</span><span class="sxs-lookup"><span data-stu-id="96694-107">Provide a general-purpose, lightweight callback capability to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="96694-108">目的</span><span class="sxs-lookup"><span data-stu-id="96694-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="96694-109">現在、ユーザーは `System.Delegate` 型を使用してコールバックを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="96694-109">Today, users have the ability to create callbacks using the `System.Delegate` type.</span></span> <span data-ttu-id="96694-110">ただし、これらはかなり重いです (ヒープ割り当てが必要であり、常にコールバックを連結するように処理しているなど)。</span><span class="sxs-lookup"><span data-stu-id="96694-110">However, these are fairly heavyweight (such as requiring a heap allocation and always having handling for chaining callbacks together).</span></span>

<span data-ttu-id="96694-111">さらに、`System.Delegate` はアンマネージ関数ポインターとの最適な相互運用機能を提供しません。つまり、非 blittable であり、マネージ/アンマネージ境界を越えるたびにマーシャリングが必要になるためです。</span><span class="sxs-lookup"><span data-stu-id="96694-111">Additionally, `System.Delegate` does not provide the best interop with unmanaged function pointers, namely due being non-blittable and requiring marshalling anytime it crosses the managed/unmanaged boundary.</span></span>

<span data-ttu-id="96694-112">いくつかの小さな微調整で、ネイティブコードを使用して、軽量、汎用、および対話形式の新しいデリゲートを提供できます。</span><span class="sxs-lookup"><span data-stu-id="96694-112">With a few minor tweaks, we could provide a new type of delegate that is lightweight, general-purpose, and interops well with native code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="96694-113">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="96694-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="96694-114">1つは、次のようにして静的デリゲートを宣言することです。</span><span class="sxs-lookup"><span data-stu-id="96694-114">One would declare a static delegate via the following:</span></span>

```C#
static delegate int Func()
```

<span data-ttu-id="96694-115">さらに、呼び出し規則、文字列のマーシャリング、および最後のエラー動作の設定を制御できるように、`System.Runtime.InteropServices.UnmanagedFunctionPointer` に似た方法で宣言の属性を設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="96694-115">One could additionally attribute the declaration with something similar to `System.Runtime.InteropServices.UnmanagedFunctionPointer` so that the calling convention, string marshalling, and set last error behavior can be controlled.</span></span> <span data-ttu-id="96694-116">注: `System.Runtime.InteropServices.UnmanagedFunctionPointer` 自体を使用することはできません。デリゲートでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="96694-116">NOTE: Using `System.Runtime.InteropServices.UnmanagedFunctionPointer` itself will not work, as it is only usable on Delegates.</span></span>

<span data-ttu-id="96694-117">宣言は、次のようなコンパイラによって内部表現に変換されます。</span><span class="sxs-lookup"><span data-stu-id="96694-117">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

<span data-ttu-id="96694-118">つまり、型 `IntPtr` の1つのメンバーを持つ構造体によって内部的に表現されます (構造体は blittable であり、ヒープ割り当ては発生しません)。</span><span class="sxs-lookup"><span data-stu-id="96694-118">That is to say, it is internally represented by a struct that has a single member of type `IntPtr` (such a struct is blittable and does not incur any heap allocations):</span></span>
* <span data-ttu-id="96694-119">メンバーには、コールバックとなる関数のアドレスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="96694-119">The member contains the address of the function that is to be the callback.</span></span>
* <span data-ttu-id="96694-120">この型は、コールバックのメソッドシグネチャに一致するメソッドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="96694-120">The type declares a method matching the method signature of the callback.</span></span>
* <span data-ttu-id="96694-121">構造体の名前は、内部的に生成された他のバッキング構造体と同様に、ユーザーが構築できないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="96694-121">The name of the struct should not be user-constructible (as we do with other internally generated backing structures).</span></span>
 * <span data-ttu-id="96694-122">例: 固定サイズバッファーは、`<name>e__FixedBuffer` の形式で名前を持つ構造体を生成します (`<` と `>` は識別子の一部であり、識別子は構築C#可能であり、IL では使用できます)。</span><span class="sxs-lookup"><span data-stu-id="96694-122">For example: fixed size buffers generate a struct with a name in the format of `<name>e__FixedBuffer` (`<` and `>` are part of the identifier and make the identifier not constructible in C#, but still useable in IL).</span></span>
* <span data-ttu-id="96694-123">メソッド宣言の名前は、すべての静的なデリゲート型で使用される既知の名前である必要があります (これにより、コンパイラは、署名を決定するときに検索する名前を知ることができます)。</span><span class="sxs-lookup"><span data-stu-id="96694-123">The name of the method declaration should be a well known name used across all static delegate types (this allows the compiler to know the name to look for when determining the signature).</span></span>

<span data-ttu-id="96694-124">静的デリゲートの値は、コールバックのシグネチャと一致する静的メソッドにのみバインドできます。</span><span class="sxs-lookup"><span data-stu-id="96694-124">The value of the static delegate can only be bound to a static method that matches the signature of the callback.</span></span>

<span data-ttu-id="96694-125">コールバックの連結はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="96694-125">Chaining callbacks together is not supported.</span></span>

<span data-ttu-id="96694-126">コールバックの呼び出しは、`calli` 命令によって実装されます。</span><span class="sxs-lookup"><span data-stu-id="96694-126">Invocation of the callback would be implemented by the `calli` instruction.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="96694-127">短所</span><span class="sxs-lookup"><span data-stu-id="96694-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="96694-128">静的デリゲートは、通常のデリゲートを使用する既存の Api では機能しません (1 つは、同じシグネチャの通常のデリゲートで静的デリゲートをラップする必要があります)。</span><span class="sxs-lookup"><span data-stu-id="96694-128">Static Delegates would not work with existing APIs that use regular delegates (one would need to wrap said static delegate in a regular delegate of the same signature).</span></span>
* <span data-ttu-id="96694-129">`System.Delegate` が `object` および `IntPtr` フィールドのセットとして内部的に表現されている場合 (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs)では、メソッドシグネチャが一致する `System.Delegate` に静的デリゲートを暗黙的に変換できる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="96694-129">Given that `System.Delegate` is represented internally as a set of `object` and `IntPtr` fields (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), it would be possible to allow implicit conversion of a static delegate to a `System.Delegate` that has a matching method signature.</span></span> <span data-ttu-id="96694-130">また、静的なデリゲートとしてのすべての要件に対して `System.Delegate` 最適化されている場合は、逆方向に明示的な変換を提供することもできます。</span><span class="sxs-lookup"><span data-stu-id="96694-130">It would also be possible to provide an explicit conversion in the opposite direction, provided the `System.Delegate` conformed to all the requirements of being a static delegate.</span></span>

<span data-ttu-id="96694-131">コアフレームワークで静的デリゲートをすぐに使用できるようにするには、追加の作業が必要になります。</span><span class="sxs-lookup"><span data-stu-id="96694-131">Additional work would be needed to make Static Delegate readily usable in the core framework.</span></span>

## <a name="alternatives"></a><span data-ttu-id="96694-132">代替</span><span class="sxs-lookup"><span data-stu-id="96694-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="96694-133">TBD</span><span class="sxs-lookup"><span data-stu-id="96694-133">TBD</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="96694-134">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="96694-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="96694-135">設計のどの部分も未定ですか。</span><span class="sxs-lookup"><span data-stu-id="96694-135">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="96694-136">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="96694-136">Design meetings</span></span>

<span data-ttu-id="96694-137">この提案に影響を与えるデザインメモにリンクし、それぞれの変更内容について1つの文で説明します。</span><span class="sxs-lookup"><span data-stu-id="96694-137">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>


