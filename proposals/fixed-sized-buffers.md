---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483572"
---
# <a name="fixed-sized-buffers"></a><span data-ttu-id="9cc0f-101">固定サイズのバッファー</span><span class="sxs-lookup"><span data-stu-id="9cc0f-101">Fixed Sized Buffers</span></span>

* <span data-ttu-id="9cc0f-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="9cc0f-102">[x] Proposed</span></span>
* <span data-ttu-id="9cc0f-103">[] プロトタイプ: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="9cc0f-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="9cc0f-104">[] の実装: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="9cc0f-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="9cc0f-105">[] 仕様: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="9cc0f-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="9cc0f-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="9cc0f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="9cc0f-107">固定サイズのバッファーをC#言語に宣言するための汎用的な安全な機構を提供します。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-107">Provide a general-purpose and safe mechanism for declaring fixed sized buffers to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="9cc0f-108">目的</span><span class="sxs-lookup"><span data-stu-id="9cc0f-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="9cc0f-109">現在、ユーザーは、安全でないコンテキストで固定サイズのバッファーを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-109">Today, users have the ability to create fixed-sized buffers in an unsafe-context.</span></span> <span data-ttu-id="9cc0f-110">ただし、この場合は、ユーザーがポインターを処理し、手動で範囲チェックを実行する必要があります。また、サポートされている型の種類 (`bool`、`byte`、`char`、`short`、`int`、`long`、`sbyte`、`ushort`、`uint`、`ulong`、`float`、`double`) は限られています。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-110">However, this requires the user to deal with pointers, manually perform bounds checks, and only supports a limited set of types (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`, and `double`).</span></span>

<span data-ttu-id="9cc0f-111">最も一般的な問題は、固定サイズのバッファーに安全なコードでインデックスを作成できないことです。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-111">The most common complaint is that fixed-size buffers cannot be indexed in safe code.</span></span> <span data-ttu-id="9cc0f-112">2番目の型を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-112">Inability to use more types is the second.</span></span>

<span data-ttu-id="9cc0f-113">いくつかの小さな微調整で、任意の型をサポートする汎用の固定サイズのバッファーを提供し、安全なコンテキストで使用し、自動的に境界チェックを実行できます。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-113">With a few minor tweaks, we could provide general-purpose fixed-sized buffers which support any type, can be used in a safe context, and have automatic bounds checking performed.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="9cc0f-114">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="9cc0f-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="9cc0f-115">1つは、次のものを使用して、安全な固定サイズのバッファーを宣言することです。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-115">One would declare a safe fixed-sized buffer via the following:</span></span>

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

<span data-ttu-id="9cc0f-116">宣言は、次のようなコンパイラによって内部表現に変換されます。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-116">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

<span data-ttu-id="9cc0f-117">このような固定サイズのバッファーでは `fixed`を使用する必要がなくなるため、任意の要素の型を許可することが理にかなっています。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-117">Since such fixed-sized buffers no longer require use of `fixed`, it makes sense to allow any element type.</span></span>  

> <span data-ttu-id="9cc0f-118">注: `fixed` は引き続きサポートされますが、要素型が `blittable` の場合のみ</span><span class="sxs-lookup"><span data-stu-id="9cc0f-118">NOTE: `fixed` will still be supported, but only if the element type is `blittable`</span></span>

## <a name="drawbacks"></a><span data-ttu-id="9cc0f-119">短所</span><span class="sxs-lookup"><span data-stu-id="9cc0f-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

* <span data-ttu-id="9cc0f-120">旧バージョンとの互換性に関するいくつかの問題がある場合もありますが、既存の固定サイズのバッファーがプリミティブ型の選択でのみ機能する場合は、ユーザーが固定バッファーをとして処理した場合に、コンパイラが "単に機能している" ままになる可能性があります。pointer.</span><span class="sxs-lookup"><span data-stu-id="9cc0f-120">There could be some challenges with backwards compatibility, but given that the existing fixed-sized buffers only work with a selection of primitive types, it should be possible for the compiler to continue "just-working" if the user treats the fixed-buffer as a pointer.</span></span>
* <span data-ttu-id="9cc0f-121">互換性のないコンストラクトでは、古いコンパイラのフィールドを非表示にするために、若干異なる `v2` エンコードを使用する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-121">Incompatible constructs may need to use slightly different `v2` encoding to hide the fields from old compiler.</span></span>
* <span data-ttu-id="9cc0f-122">パッキングは、ジェネリック型の IL 仕様では適切に定義されていません。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-122">Packing is not well defined in IL spec for generic types.</span></span> <span data-ttu-id="9cc0f-123">このアプローチは機能しますが、ドキュメントに記載されていない動作に境界ます。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-123">While the approach should work, we will be bordering on undocumented behavior.</span></span> <span data-ttu-id="9cc0f-124">このドキュメントを文書化し、他の JITs と同じ Mono が動作することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-124">We should make that documented and make sure other JITs like Mono have the same behavior.</span></span>
* <span data-ttu-id="9cc0f-125">すべての長さに対して個別の型を指定する (サポートされている場合、`readonly` フィールドに別の型を指定すると、メタデータに影響します)。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-125">Specifying a separate type for every length (an possibly another for `readonly` fields, if supported) will have impact on metadata.</span></span> <span data-ttu-id="9cc0f-126">この値は、特定のアプリのさまざまなサイズの配列の数によってバインドされます。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-126">It will be bound by the number of arrays of different sizes in the given app.</span></span>
* <span data-ttu-id="9cc0f-127">`ref` の数値演算は、安全ではないため、正式に検証できません。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-127">`ref` math is not formally verifiable (since it is unsafe).</span></span> <span data-ttu-id="9cc0f-128">検証規則を更新して、使用に問題がないことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-128">We will need to find a way to update verification rules to know that our use is ok.</span></span>

## <a name="alternatives"></a><span data-ttu-id="9cc0f-129">代替</span><span class="sxs-lookup"><span data-stu-id="9cc0f-129">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="9cc0f-130">手動で構造体を宣言し、unsafe コードを使用してインデクサーを構築します。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-130">Manually declare your structures and use unsafe code to construct indexers.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="9cc0f-131">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="9cc0f-131">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="9cc0f-132">`readonly`を許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-132">should we allow `readonly`?</span></span>  <span data-ttu-id="9cc0f-133">(readonly インデクサーを含む)</span><span class="sxs-lookup"><span data-stu-id="9cc0f-133">(with readonly indexer)</span></span>
- <span data-ttu-id="9cc0f-134">配列初期化子を許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-134">should we allow array initializers?</span></span>
- <span data-ttu-id="9cc0f-135">`fixed` キーワードは必須ですか?</span><span class="sxs-lookup"><span data-stu-id="9cc0f-135">is `fixed` keyword necessary?</span></span>
- <span data-ttu-id="9cc0f-136">`foreach`?</span><span class="sxs-lookup"><span data-stu-id="9cc0f-136">`foreach`?</span></span>
- <span data-ttu-id="9cc0f-137">構造体のインスタンスフィールドのみ</span><span class="sxs-lookup"><span data-stu-id="9cc0f-137">only instance fields in structs?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="9cc0f-138">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="9cc0f-138">Design meetings</span></span>

<span data-ttu-id="9cc0f-139">この提案に影響を与えるデザインメモにリンクし、それぞれの変更内容について1つの文で説明します。</span><span class="sxs-lookup"><span data-stu-id="9cc0f-139">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>