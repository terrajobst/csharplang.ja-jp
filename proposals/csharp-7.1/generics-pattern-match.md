---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483674"
---
# <a name="pattern-matching-with-generics"></a><span data-ttu-id="271c0-101">ジェネリックを使用したパターン一致</span><span class="sxs-lookup"><span data-stu-id="271c0-101">pattern-matching with generics</span></span>

* <span data-ttu-id="271c0-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="271c0-102">[x] Proposed</span></span>
* <span data-ttu-id="271c0-103">[] プロトタイプ:</span><span class="sxs-lookup"><span data-stu-id="271c0-103">[ ] Prototype:</span></span>
* <span data-ttu-id="271c0-104">[] の実装:</span><span class="sxs-lookup"><span data-stu-id="271c0-104">[ ] Implementation:</span></span>
* <span data-ttu-id="271c0-105">[] 仕様:</span><span class="sxs-lookup"><span data-stu-id="271c0-105">[ ] Specification:</span></span>

## <a name="summary"></a><span data-ttu-id="271c0-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="271c0-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="271c0-107">[ C#既存の as 演算子](../../spec/expressions.md#the-as-operator)の仕様では、がオープン型である場合、オペランドの型と指定された型の間で変換を行うことはできません。</span><span class="sxs-lookup"><span data-stu-id="271c0-107">The specification for the [existing C# as operator](../../spec/expressions.md#the-as-operator) permits there to be no conversion between the type of the operand and the specified type when either is an open type.</span></span> <span data-ttu-id="271c0-108">ただし、7 C#では、`Type identifier` パターンでは、入力の型と指定された型の間の変換が必要です。</span><span class="sxs-lookup"><span data-stu-id="271c0-108">However, in C# 7 the `Type identifier` pattern requires there be a conversion between the type of the input and the given type.</span></span>

<span data-ttu-id="271c0-109">`expression is Type identifier`この問題を緩和し、7でC#許可されている条件で許可されていることに加え、`expression as Type` が許可された場合にも許可されるようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="271c0-109">We propose to relax this and change `expression is Type identifier`, in addition to being permitted in the conditions when it is permitted in C# 7, to also be permitted when `expression as Type` would be allowed.</span></span> <span data-ttu-id="271c0-110">具体的には、式または指定された型がオープン型である場合に、新しいケースが発生します。</span><span class="sxs-lookup"><span data-stu-id="271c0-110">Specifically, the new cases are cases where the type of the expression or the specified type is an open type.</span></span> 

## <a name="motivation"></a><span data-ttu-id="271c0-111">目的</span><span class="sxs-lookup"><span data-stu-id="271c0-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="271c0-112">現在、パターンマッチングが "明らか" に許可されている場合は、コンパイルに失敗します。</span><span class="sxs-lookup"><span data-stu-id="271c0-112">Cases where pattern-matching should "obviously" be permitted currently fail to compile.</span></span> <span data-ttu-id="271c0-113">例については、 https://github.com/dotnet/roslyn/issues/16195を参照してください。</span><span class="sxs-lookup"><span data-stu-id="271c0-113">See, for example, https://github.com/dotnet/roslyn/issues/16195.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="271c0-114">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="271c0-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="271c0-115">パターンマッチングの仕様の段落を変更します (提案された追加は太字で示されています)。</span><span class="sxs-lookup"><span data-stu-id="271c0-115">We change the paragraph in the pattern-matching specification (the proposed addition is shown in bold):</span></span>

> <span data-ttu-id="271c0-116">左側の静的な型と指定された型の特定の組み合わせは互換性がないと見なされ、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="271c0-116">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="271c0-117">静的な型 `E` の値は、id 変換、暗黙の参照変換、ボックス変換、明示的な参照変換、または `E` から `T`へのアンボックス変換、または **`E` または `T` のいずれかがオープン型**である場合に、型 `T` との*パターン互換性*があると言われます。</span><span class="sxs-lookup"><span data-stu-id="271c0-117">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`**, or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="271c0-118">`E` 型の式が、照合される型パターンの型と互換性のあるパターンでない場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="271c0-118">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="271c0-119">短所</span><span class="sxs-lookup"><span data-stu-id="271c0-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="271c0-120">[なし] :</span><span class="sxs-lookup"><span data-stu-id="271c0-120">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="271c0-121">代替</span><span class="sxs-lookup"><span data-stu-id="271c0-121">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="271c0-122">[なし] :</span><span class="sxs-lookup"><span data-stu-id="271c0-122">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="271c0-123">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="271c0-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="271c0-124">[なし] :</span><span class="sxs-lookup"><span data-stu-id="271c0-124">None.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="271c0-125">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="271c0-125">Design meetings</span></span>

<span data-ttu-id="271c0-126">LDM はこの質問を検討し、バグ修正レベルの変更であると考えました。</span><span class="sxs-lookup"><span data-stu-id="271c0-126">LDM considered this question and felt it was a bug-fix level change.</span></span> <span data-ttu-id="271c0-127">言語がリリースされた後に変更を加えるだけで、上位互換性がなくなるため、これを別の言語機能として扱います。</span><span class="sxs-lookup"><span data-stu-id="271c0-127">We are treating it as a separate language feature because just making the change after the language has been released would introduce a forward incompatibility.</span></span> <span data-ttu-id="271c0-128">提案された変更を使用するには、プログラマが言語バージョン7.1 を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="271c0-128">Using the proposed change requires that the programmer specify language version 7.1.</span></span>
