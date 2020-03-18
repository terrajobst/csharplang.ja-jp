---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483668"
---
# <a name="conditional-ref-expressions"></a><span data-ttu-id="24ee7-101">条件付き参照式</span><span class="sxs-lookup"><span data-stu-id="24ee7-101">Conditional ref expressions</span></span>

<span data-ttu-id="24ee7-102">参照変数を1つまたは別の式にバインドするパターンは、条件付きC#で現在は表現できません。</span><span class="sxs-lookup"><span data-stu-id="24ee7-102">The pattern of binding a ref variable to one or another expression conditionally is not currently expressible in C#.</span></span>

<span data-ttu-id="24ee7-103">一般的な回避策は、次のようなメソッドを導入することです。</span><span class="sxs-lookup"><span data-stu-id="24ee7-103">The typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="24ee7-104">これは、すべての引数が呼び出しサイトで評価される必要があるため、三項の正確な置換ではないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="24ee7-104">Note that this is not an exact replacement of a ternary since all arguments must be evaluated at the call site.</span></span>

<span data-ttu-id="24ee7-105">次のものは期待どおりに機能しません。</span><span class="sxs-lookup"><span data-stu-id="24ee7-105">The following will not work as expected:</span></span>

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

<span data-ttu-id="24ee7-106">提案された構文は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="24ee7-106">The proposed syntax would look like:</span></span>

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

<span data-ttu-id="24ee7-107">上記の "Choice" の試行は、ref 三項を使用して_正しく_記述することができます。</span><span class="sxs-lookup"><span data-stu-id="24ee7-107">The above attempt with "Choice" can be _correctly_ written using ref ternary as:</span></span>

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="24ee7-108">選択肢との違いは、結果と代替式が_本当_に条件付きでアクセスされるため、クラッシュが発生しないことが ```arr == null```</span><span class="sxs-lookup"><span data-stu-id="24ee7-108">The difference from Choice is that consequence and alternative expressions are accessed in a _truly_ conditional manner, so we do not see a crash if ```arr == null```</span></span>

<span data-ttu-id="24ee7-109">三項参照は、代替と結果の両方が refs である三項です。</span><span class="sxs-lookup"><span data-stu-id="24ee7-109">The ternary ref is just a ternary where both alternative and consequence are refs.</span></span> <span data-ttu-id="24ee7-110">結果または代替のオペランドが左辺値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="24ee7-110">It will naturally require that consequence/alternative operands are LValues.</span></span> <span data-ttu-id="24ee7-111">また、結果と代替として、相互に変換できる id を持つ型が必要になります。</span><span class="sxs-lookup"><span data-stu-id="24ee7-111">It will also require that consequence and alternative have types that are identity convertible to each other.</span></span>

<span data-ttu-id="24ee7-112">式の型は、通常の三項の型と同様に計算されます。</span><span class="sxs-lookup"><span data-stu-id="24ee7-112">The type of the expression will be computed similarly to the one for the regular ternary.</span></span> <span data-ttu-id="24ee7-113">つまり、</span><span class="sxs-lookup"><span data-stu-id="24ee7-113">I.E.</span></span> <span data-ttu-id="24ee7-114">結果と代替の id が変換可能であるものの、型が異なる場合は、既存の型マージルールが適用されます。</span><span class="sxs-lookup"><span data-stu-id="24ee7-114">in a case if consequence and alternative have identity convertible, but different types, the existing type-merging rules will apply.</span></span>

<span data-ttu-id="24ee7-115">セーフツーリターンは、条件付きオペランドからの控えめと見なされます。</span><span class="sxs-lookup"><span data-stu-id="24ee7-115">Safe-to-return will be assumed conservatively from the conditional operands.</span></span> <span data-ttu-id="24ee7-116">どちらかが安全でない場合は、を返すことは安全ではありません。</span><span class="sxs-lookup"><span data-stu-id="24ee7-116">If either is unsafe to return the whole thing is unsafe to return.</span></span>

<span data-ttu-id="24ee7-117">Ref 三項は左辺値であるため、参照渡しで渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="24ee7-117">Ref ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="24ee7-118">左辺値である場合は、に割り当てることもできます。</span><span class="sxs-lookup"><span data-stu-id="24ee7-118">Being an LValue, it can also be assigned to.</span></span> 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

<span data-ttu-id="24ee7-119">Ref 三項は、通常の (非 ref) コンテキストでも使用できます。</span><span class="sxs-lookup"><span data-stu-id="24ee7-119">Ref ternary can be used in a regular (not ref) context as well.</span></span> <span data-ttu-id="24ee7-120">標準の三項を使用することもできるので、一般的ではありません。</span><span class="sxs-lookup"><span data-stu-id="24ee7-120">Although it would not be common since you could as well just use a regular ternary.</span></span>

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

<span data-ttu-id="24ee7-121">実装に関する注意事項:</span><span class="sxs-lookup"><span data-stu-id="24ee7-121">Implementation notes:</span></span> 

<span data-ttu-id="24ee7-122">実装の複雑さは、中程度から大規模なバグ修正のサイズであるように見えます。</span><span class="sxs-lookup"><span data-stu-id="24ee7-122">The complexity of the implementation would seem to be the size of a moderate-to-large bug fix.</span></span> <span data-ttu-id="24ee7-123">-I. E コストがかかりません。</span><span class="sxs-lookup"><span data-stu-id="24ee7-123">- I.E not very expensive.</span></span>
<span data-ttu-id="24ee7-124">構文や解析を変更する必要はないと思います。</span><span class="sxs-lookup"><span data-stu-id="24ee7-124">I do not think we need any changes to the syntax or parsing.</span></span>
<span data-ttu-id="24ee7-125">メタデータまたは相互運用には影響しません。</span><span class="sxs-lookup"><span data-stu-id="24ee7-125">There is no effect on metadata or interop.</span></span> <span data-ttu-id="24ee7-126">この機能は完全に式ベースです。</span><span class="sxs-lookup"><span data-stu-id="24ee7-126">The feature is completely expression based.</span></span>
<span data-ttu-id="24ee7-127">デバッグ/PDB には影響しません</span><span class="sxs-lookup"><span data-stu-id="24ee7-127">No effect on debugging/PDB either</span></span>
