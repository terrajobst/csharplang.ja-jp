---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483686"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a><span data-ttu-id="63ebc-101">タプル名 (別名) を推測します。</span><span class="sxs-lookup"><span data-stu-id="63ebc-101">Infer tuple names (aka.</span></span> <span data-ttu-id="63ebc-102">タプルプロジェクション初期化子)</span><span class="sxs-lookup"><span data-stu-id="63ebc-102">tuple projection initializers)</span></span>

## <a name="summary"></a><span data-ttu-id="63ebc-103">まとめ</span><span class="sxs-lookup"><span data-stu-id="63ebc-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="63ebc-104">多くの一般的なケースでは、この機能によりタプル要素名を省略し、代わりに推論することができます。</span><span class="sxs-lookup"><span data-stu-id="63ebc-104">In a number of common cases, this feature allows the tuple element names to be omitted and instead be inferred.</span></span> <span data-ttu-id="63ebc-105">たとえば、`(f1: x.f1, f2: x?.f2)`を入力する代わりに、要素名 "f1" と "f2" を `(x.f1, x?.f2)`から推論できます。</span><span class="sxs-lookup"><span data-stu-id="63ebc-105">For instance, instead of typing `(f1: x.f1, f2: x?.f2)`, the element names "f1" and "f2" can be inferred from `(x.f1, x?.f2)`.</span></span>

<span data-ttu-id="63ebc-106">これは匿名型の動作と同じであり、作成時にメンバー名を推論できます。</span><span class="sxs-lookup"><span data-stu-id="63ebc-106">This parallels the behavior of  anonymous types, which allow inferring member names during creation.</span></span> <span data-ttu-id="63ebc-107">たとえば、`new { x.f1, y?.f2 }` メンバー "f1" と "f2" を宣言します。</span><span class="sxs-lookup"><span data-stu-id="63ebc-107">For instance, `new { x.f1, y?.f2 }` declares members "f1" and "f2".</span></span>

<span data-ttu-id="63ebc-108">これは、LINQ でタプルを使用する場合に特に便利です。</span><span class="sxs-lookup"><span data-stu-id="63ebc-108">This is particularly handy when using tuples in LINQ:</span></span>

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a><span data-ttu-id="63ebc-109">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="63ebc-109">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="63ebc-110">変更には次の2つの部分があります。</span><span class="sxs-lookup"><span data-stu-id="63ebc-110">There are two parts to the change:</span></span>

1.  <span data-ttu-id="63ebc-111">明示的な名前を持たない各タプル要素の候補名を推測します。</span><span class="sxs-lookup"><span data-stu-id="63ebc-111">Try to infer a candidate name for each tuple element which does not have an explicit name:</span></span>
    -   <span data-ttu-id="63ebc-112">匿名型の名前の推論と同じ規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="63ebc-112">Using same rules as name inference for anonymous types.</span></span>
        - <span data-ttu-id="63ebc-113">でC#は、`y` (識別子)、`x.y` (単純メンバーアクセス)、および `x?.y` (条件付きアクセス) という3つのケースが可能です。</span><span class="sxs-lookup"><span data-stu-id="63ebc-113">In C#, this allows three cases: `y` (identifier), `x.y` (simple member access) and `x?.y` (conditional access).</span></span>
        - <span data-ttu-id="63ebc-114">VB では、`x.y()`などの追加のケースに対応できます。</span><span class="sxs-lookup"><span data-stu-id="63ebc-114">In VB, this allows for additional cases, such as `x.y()`.</span></span>
    -   <span data-ttu-id="63ebc-115">予約された組名の拒否 ( C#では大文字と小文字が区別されますが、VB では大文字と小文字は区別されません)。これらは禁止または暗黙的です</span><span class="sxs-lookup"><span data-stu-id="63ebc-115">Rejecting reserved tuple names (case-sensitive in C#, case-insensitive in VB), as they are either forbidden or already implicit.</span></span> <span data-ttu-id="63ebc-116">たとえば、`ItemN`、`Rest`、`ToString`などです。</span><span class="sxs-lookup"><span data-stu-id="63ebc-116">For instance, such as `ItemN`, `Rest`, and `ToString`.</span></span>
    -   <span data-ttu-id="63ebc-117">組全体で、候補名が重複しているC#場合 (大文字と小文字が区別され、VB では大文字と小文字が区別されます)、これらの候補は削除されます。</span><span class="sxs-lookup"><span data-stu-id="63ebc-117">If any candidate names are duplicates (case-sensitive in C#, case-insensitive in VB) within the entire tuple, we drop those candidates,</span></span>
2.  <span data-ttu-id="63ebc-118">変換中に (タプルリテラルからの名前の削除を確認して警告する)、推論された名前は警告を生成しません。</span><span class="sxs-lookup"><span data-stu-id="63ebc-118">During conversions (which check and warn about dropping names from tuple literals), inferred names would not produce any warnings.</span></span> <span data-ttu-id="63ebc-119">これにより、既存の組コードの破損を回避できます。</span><span class="sxs-lookup"><span data-stu-id="63ebc-119">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="63ebc-120">重複を処理するルールは、匿名型の場合とは異なります。</span><span class="sxs-lookup"><span data-stu-id="63ebc-120">Note that the rule for handling duplicates is different than that for anonymous types.</span></span> <span data-ttu-id="63ebc-121">たとえば、`new { x.f1, x.f1 }` ではエラーが生成されますが、推論された名前がなくても `(x.f1, x.f1)` は引き続き許可されます。</span><span class="sxs-lookup"><span data-stu-id="63ebc-121">For instance, `new { x.f1, x.f1 }` produces an error, but `(x.f1, x.f1)` would still be allowed (just without any inferred names).</span></span> <span data-ttu-id="63ebc-122">これにより、既存の組コードの破損を回避できます。</span><span class="sxs-lookup"><span data-stu-id="63ebc-122">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="63ebc-123">一貫性を確保するために、分解によって生成されるタプル ( C#) にも同じことが当てはまります。</span><span class="sxs-lookup"><span data-stu-id="63ebc-123">For consistency, the same would apply to tuples produced by deconstruction-assignments (in C#):</span></span>

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

<span data-ttu-id="63ebc-124">同じことが VB のタプルにも適用されます。 vb 固有のルールを使用して、式から名前を推論し、大文字と小文字を区別しない名前比較を行います。</span><span class="sxs-lookup"><span data-stu-id="63ebc-124">The same would also apply to VB tuples, using the VB-specific rules for inferring name from expression and case-insensitive name comparisons.</span></span>

<span data-ttu-id="63ebc-125">言語バージョン " C# 7.0" で7.1 コンパイラ (またはそれ以降) を使用する場合、要素名は (機能が使用されていないにもかかわらず) 推定されますが、アクセスしようとしたときに使用するサイトエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="63ebc-125">When using the C# 7.1 compiler (or later) with language version "7.0", the element names will be inferred (despite the feature not being available), but there will be a use-site error for trying to access them.</span></span> <span data-ttu-id="63ebc-126">これにより、後で互換性の問題が発生する新しいコード (以下で説明) の追加が制限されます。</span><span class="sxs-lookup"><span data-stu-id="63ebc-126">This will limit additions of new code that would later face the compatibility issue (described below).</span></span>

## <a name="drawbacks"></a><span data-ttu-id="63ebc-127">短所</span><span class="sxs-lookup"><span data-stu-id="63ebc-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="63ebc-128">主な欠点は、 C# 7.0 との互換性が失われることです。</span><span class="sxs-lookup"><span data-stu-id="63ebc-128">The main drawback is that this introduces a compatibility break from C# 7.0:</span></span>

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

<span data-ttu-id="63ebc-129">互換性の問題を検出した場合は、制限されていることを確認してください。 C#また、(7.0 で) 組が発送されてからの時間枠は短くなっています。</span><span class="sxs-lookup"><span data-stu-id="63ebc-129">The compatibility council found this break acceptable, given that it is limited and the time window since tuples shipped (in C# 7.0) is short.</span></span>

## <a name="references"></a><span data-ttu-id="63ebc-130">References</span><span class="sxs-lookup"><span data-stu-id="63ebc-130">References</span></span>
- [<span data-ttu-id="63ebc-131">2017年4月4日</span><span class="sxs-lookup"><span data-stu-id="63ebc-131">LDM April 4th 2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- <span data-ttu-id="63ebc-132">[Github のディスカッション](https://github.com/dotnet/csharplang/issues/370)(この問題を発生させるための @alrz に感謝)</span><span class="sxs-lookup"><span data-stu-id="63ebc-132">[Github discussion](https://github.com/dotnet/csharplang/issues/370) (thanks @alrz for bringing this issue up)</span></span>
- [<span data-ttu-id="63ebc-133">タプルの設計</span><span class="sxs-lookup"><span data-stu-id="63ebc-133">Tuples design</span></span>](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
