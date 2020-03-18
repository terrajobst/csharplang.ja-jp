---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483632"
---
# <a name="pattern-based-fixed-statement"></a><span data-ttu-id="596fb-101">パターン ベースの `fixed` ステートメント</span><span class="sxs-lookup"><span data-stu-id="596fb-101">Pattern-based `fixed` statement</span></span>

## <a name="summary"></a><span data-ttu-id="596fb-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="596fb-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="596fb-103">`fixed` ステートメントに型を含めることができるパターンを導入します。</span><span class="sxs-lookup"><span data-stu-id="596fb-103">Introduce a pattern that would allow types to participate in `fixed` statements.</span></span> 

## <a name="motivation"></a><span data-ttu-id="596fb-104">目的</span><span class="sxs-lookup"><span data-stu-id="596fb-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="596fb-105">この言語は、マネージデータをピン留めし、基になるバッファーへのネイティブポインターを取得するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="596fb-105">The language provides a mechanism for pinning managed data and obtain a native pointer to the underlying buffer.</span></span>

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

<span data-ttu-id="596fb-106">`fixed` に参加できる型のセットはハードコーディングされており、配列と `System.String`に制限されています。</span><span class="sxs-lookup"><span data-stu-id="596fb-106">The set of types that can participate in `fixed` is hardcoded and limited to arrays and `System.String`.</span></span> <span data-ttu-id="596fb-107">`ImmutableArray<T>`、`Span<T>`、`Utf8String` などの新しいプリミティブが導入された場合、"特別な" 型をハードコーディングすることはできません。</span><span class="sxs-lookup"><span data-stu-id="596fb-107">Hardcoding "special" types does not scale when new primitives such as `ImmutableArray<T>`, `Span<T>`, `Utf8String` are introduced.</span></span> 

<span data-ttu-id="596fb-108">さらに、`System.String` の現在のソリューションは、非常に厳密な API に依存しています。</span><span class="sxs-lookup"><span data-stu-id="596fb-108">In addition, the current solution for `System.String` relies on a fairly rigid API.</span></span> <span data-ttu-id="596fb-109">API の構造は、`System.String` が、オブジェクトヘッダーからの固定オフセットで、UTF16 でエンコードされたデータを埋め込む連続したオブジェクトであることを意味します。</span><span class="sxs-lookup"><span data-stu-id="596fb-109">The shape of the API implies that `System.String` is a contiguous object that embeds UTF16 encoded data at a fixed offset from the object header.</span></span> <span data-ttu-id="596fb-110">このような方法は、基になるレイアウトを変更する必要があるいくつかの提案で問題が見つかりました。</span><span class="sxs-lookup"><span data-stu-id="596fb-110">Such approach has been found problematic in several proposals that could require changes to the underlying layout.</span></span> <span data-ttu-id="596fb-111">`System.String` オブジェクトを、アンマネージ相互運用の目的で内部表現から分離することで、より柔軟なものに切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="596fb-111">It would be desirable to be able to switch to something more flexible that decouples `System.String` object from its internal representation for the purpose of unmanaged interop.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="596fb-112">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="596fb-112">Detailed design</span></span>
[design]: #detailed-design

## <a name="pattern"></a><span data-ttu-id="596fb-113">*パターン*</span><span class="sxs-lookup"><span data-stu-id="596fb-113">*Pattern*</span></span> ##
<span data-ttu-id="596fb-114">実行可能なパターンベースの "固定" のニーズは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="596fb-114">A viable pattern-based “fixed” need to:</span></span>
-   <span data-ttu-id="596fb-115">インスタンスをピン留めするためのマネージ参照を指定し、ポインターを初期化します (これは同じ参照であることが望ましい)</span><span class="sxs-lookup"><span data-stu-id="596fb-115">Provide the managed references to pin the instance and to initialize the pointer (preferably this is the same reference)</span></span>
-   <span data-ttu-id="596fb-116">アンマネージ要素の型 ("string" の "char" など) を明確に伝達します。</span><span class="sxs-lookup"><span data-stu-id="596fb-116">Convey unambiguously the type of the unmanaged element   (i.e. “char” for “string”)</span></span>
-   <span data-ttu-id="596fb-117">参照するものがない場合、"empty" の場合の動作を指定します。</span><span class="sxs-lookup"><span data-stu-id="596fb-117">Prescribe the behavior in "empty" case when there is nothing to refer to.</span></span> 
-   <span data-ttu-id="596fb-118">`fixed`の外部で型を使用するのを困難にする設計上の決定に対して、API 作成者をプッシュしないでください。</span><span class="sxs-lookup"><span data-stu-id="596fb-118">Should not push API authors toward design decisions that hurt the use of the type outside of `fixed`.</span></span>

<span data-ttu-id="596fb-119">前述のように、特別に名前が付けられた ref 戻りメンバーを認識することで、上記の条件を満たすことができます。 `ref [readonly] T GetPinnableReference()`。</span><span class="sxs-lookup"><span data-stu-id="596fb-119">I think the above could be satisfied by recognizing a specially named ref-returning member: `ref [readonly] T GetPinnableReference()`.</span></span>

<span data-ttu-id="596fb-120">`fixed` ステートメントで使用するには、次の条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="596fb-120">In order to be used by the `fixed` statement the following conditions must be met:</span></span>

1. <span data-ttu-id="596fb-121">型に対して指定されたメンバーは1つだけです。</span><span class="sxs-lookup"><span data-stu-id="596fb-121">There is only one such member provided for a type.</span></span>
1. <span data-ttu-id="596fb-122">`ref` または `ref readonly`によってを返します。</span><span class="sxs-lookup"><span data-stu-id="596fb-122">Returns by `ref` or `ref readonly`.</span></span> <span data-ttu-id="596fb-123">(`readonly` は、安全なコードで使用できる書き込み可能な API を追加せずに、変更できない型または readonly の型の作成者がパターンを実装できるようにすることが許可されています)</span><span class="sxs-lookup"><span data-stu-id="596fb-123">(`readonly` is permitted so that authors of immutable/readonly types could implement the pattern without adding writeable API that could be used in safe code)</span></span>
1. <span data-ttu-id="596fb-124">T はアンマネージ型です。</span><span class="sxs-lookup"><span data-stu-id="596fb-124">T is an unmanaged type.</span></span>
<span data-ttu-id="596fb-125">(`T*` がポインター型になるためです。</span><span class="sxs-lookup"><span data-stu-id="596fb-125">(since `T*` becomes the pointer type.</span></span> <span data-ttu-id="596fb-126">制限は、"アンマネージ" の概念が展開されている場合には自然に拡張されます)</span><span class="sxs-lookup"><span data-stu-id="596fb-126">The restriction will naturally expand if/when the notion of "unmanaged" is expanded)</span></span>
1. <span data-ttu-id="596fb-127">ピン留めするデータがない場合に、マネージ `nullptr` を返します。これは、空である可能性が最も低い方法である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="596fb-127">Returns managed `nullptr` when there is no data to pin – probably the cheapest way to convey emptiness.</span></span>
<span data-ttu-id="596fb-128">(文字列が null で終わるため、"" という文字列は ' \ 0 ' への参照を返すことに注意してください)</span><span class="sxs-lookup"><span data-stu-id="596fb-128">(note that “” string returns a ref to '\0' since strings are null-terminated)</span></span>

<span data-ttu-id="596fb-129">また `#3` では、空のケースの結果が未定義または実装固有であることを許可できます。</span><span class="sxs-lookup"><span data-stu-id="596fb-129">Alternatively for the `#3` we can allow the result in empty cases be undefined or implementation-specific.</span></span> <span data-ttu-id="596fb-130">ただし、これにより、API の危険性が高くなり、誤用や意図しない互換性の負担が発生しやすくなります。</span><span class="sxs-lookup"><span data-stu-id="596fb-130">That, however, may make the API more dangerous and prone to abuse and unintended compatibility burdens.</span></span> 

## <a name="translation"></a><span data-ttu-id="596fb-131">*翻訳*</span><span class="sxs-lookup"><span data-stu-id="596fb-131">*Translation*</span></span> ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

<span data-ttu-id="596fb-132">は次の擬似コードになります ( C#では表現できないものがあります)</span><span class="sxs-lookup"><span data-stu-id="596fb-132">becomes the following pseudocode (not all expressible in C#)</span></span>

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a><span data-ttu-id="596fb-133">短所</span><span class="sxs-lookup"><span data-stu-id="596fb-133">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="596fb-134">GetPinnableReference は `fixed`でのみ使用することを意図していますが、セーフコードでは使用できません。実装では、この点を念頭に置く必要があります。</span><span class="sxs-lookup"><span data-stu-id="596fb-134">GetPinnableReference is intended to be used only in `fixed`, but nothing prevents its use in safe code, so implementor must keep that in mind.</span></span>

## <a name="alternatives"></a><span data-ttu-id="596fb-135">代替</span><span class="sxs-lookup"><span data-stu-id="596fb-135">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="596fb-136">ユーザーは GetPinnableReference または類似したメンバーを導入し、</span><span class="sxs-lookup"><span data-stu-id="596fb-136">Users can introduce GetPinnableReference or similar member and use it as</span></span>
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

<span data-ttu-id="596fb-137">代替ソリューションが必要な場合は、`System.String` の解決策はありません。</span><span class="sxs-lookup"><span data-stu-id="596fb-137">There is no solution for `System.String` if alternative solution is desired.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="596fb-138">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="596fb-138">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="596fb-139">[] 動作が "empty" 状態になります。</span><span class="sxs-lookup"><span data-stu-id="596fb-139">[ ] Behavior in "empty" state.</span></span><span data-ttu-id="596fb-140">`nullptr` または `undefined` を  - しますか?</span><span class="sxs-lookup"><span data-stu-id="596fb-140"> - `nullptr` or `undefined` ?</span></span> 
- <span data-ttu-id="596fb-141">[] 拡張メソッドを考慮する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="596fb-141">[ ] Should the extension methods be considered ?</span></span> 
- <span data-ttu-id="596fb-142">[] `System.String`でパターンが検出された場合、それを勝ちにしますか?</span><span class="sxs-lookup"><span data-stu-id="596fb-142">[ ] If a pattern is detected on `System.String`, should it win over ?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="596fb-143">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="596fb-143">Design meetings</span></span>

<span data-ttu-id="596fb-144">まだありません。</span><span class="sxs-lookup"><span data-stu-id="596fb-144">None yet.</span></span> 
