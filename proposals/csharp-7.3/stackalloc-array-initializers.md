---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483590"
---
# <a name="stackalloc-array-initializers"></a><span data-ttu-id="3b66b-101">stackalloc 配列初期化子</span><span class="sxs-lookup"><span data-stu-id="3b66b-101">Stackalloc array initializers</span></span>

## <a name="summary"></a><span data-ttu-id="3b66b-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="3b66b-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3b66b-103">`stackalloc`で配列初期化子構文を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="3b66b-103">Allow array initializer syntax to be used with `stackalloc`.</span></span>

## <a name="motivation"></a><span data-ttu-id="3b66b-104">目的</span><span class="sxs-lookup"><span data-stu-id="3b66b-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3b66b-105">通常の配列は、作成時に初期化された要素を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="3b66b-105">Ordinary arrays can have their elements initialized at creation time.</span></span> <span data-ttu-id="3b66b-106">`stackalloc` の場合は、これを許可することが妥当であると思われます。</span><span class="sxs-lookup"><span data-stu-id="3b66b-106">It seems reasonable to allow that in `stackalloc` case.</span></span>

<span data-ttu-id="3b66b-107">このような構文を使用できない理由については、`stackalloc` が非常に頻繁に発生します。</span><span class="sxs-lookup"><span data-stu-id="3b66b-107">The question of why such syntax is not allowed with `stackalloc` arises fairly frequently.</span></span>  
<span data-ttu-id="3b66b-108">例については、「」を参照してください[#1112](https://github.com/dotnet/csharplang/issues/1112)</span><span class="sxs-lookup"><span data-stu-id="3b66b-108">See, for example, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3b66b-109">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="3b66b-109">Detailed design</span></span>

<span data-ttu-id="3b66b-110">通常の配列は、次の構文を使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="3b66b-110">Ordinary arrays can be created through the following syntax:</span></span>

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

<span data-ttu-id="3b66b-111">次のようにして、スタックの割り当て済み配列を作成できるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b66b-111">We should allow stack allocated arrays be created through:</span></span>  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

<span data-ttu-id="3b66b-112">すべてのケースのセマンティクスは、配列の場合とほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="3b66b-112">The semantics of all cases is roughly the same as with arrays.</span></span>  
<span data-ttu-id="3b66b-113">たとえば、最後のケースでは、要素型は初期化子から推論され、"アンマネージ" 型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b66b-113">For example: in the last case the element type is inferred from the initializer and must be an "unmanaged" type.</span></span>

<span data-ttu-id="3b66b-114">注: この機能は、ターゲットが `Span<T>`であることに依存していません。</span><span class="sxs-lookup"><span data-stu-id="3b66b-114">NOTE: the feature is not dependent on the target being a `Span<T>`.</span></span> <span data-ttu-id="3b66b-115">`T*` の場合に適用されるだけなので、`Span<T>` ケースで述語を行うのは妥当ではないようです。</span><span class="sxs-lookup"><span data-stu-id="3b66b-115">It is just as applicable in `T*` case, so it does not seem reasonable to predicate it on `Span<T>` case.</span></span>  

## <a name="translation"></a><span data-ttu-id="3b66b-116">翻訳</span><span class="sxs-lookup"><span data-stu-id="3b66b-116">Translation</span></span>

<span data-ttu-id="3b66b-117">単純な実装では、一連の要素ごとの代入を通じて、配列を作成した直後に初期化するだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="3b66b-117">The naive implementation could just initialize the array right after creation through a series of element-wise assignments.</span></span>  

<span data-ttu-id="3b66b-118">配列の場合と同様に、すべてまたはほとんどの要素が blittable 型であるケースを検出し、すべての定数要素の事前に作成された状態をコピーすることで、より効率的な手法を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3b66b-118">Similarly to the case with arrays, it might be possible and desirable to detect cases where all or most of the elements are blittable types and use more efficient techniques by copying over the pre-created state of all the constant elements.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="3b66b-119">短所</span><span class="sxs-lookup"><span data-stu-id="3b66b-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

## <a name="alternatives"></a><span data-ttu-id="3b66b-120">代替</span><span class="sxs-lookup"><span data-stu-id="3b66b-120">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3b66b-121">これは便利な機能です。</span><span class="sxs-lookup"><span data-stu-id="3b66b-121">This is a convenience feature.</span></span> <span data-ttu-id="3b66b-122">何もすることはできません。</span><span class="sxs-lookup"><span data-stu-id="3b66b-122">It is possible to just do nothing.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="3b66b-123">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="3b66b-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="3b66b-124">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="3b66b-124">Design meetings</span></span>

<span data-ttu-id="3b66b-125">まだありません。</span><span class="sxs-lookup"><span data-stu-id="3b66b-125">None yet.</span></span> 
