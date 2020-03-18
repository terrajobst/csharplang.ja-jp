---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "79483890"
---
# <a name="null-coalescing-assignment"></a><span data-ttu-id="ca610-101">null 合体演算子割り当て</span><span class="sxs-lookup"><span data-stu-id="ca610-101">null coalescing assignment</span></span>

* <span data-ttu-id="ca610-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="ca610-102">[x] Proposed</span></span>
* <span data-ttu-id="ca610-103">[] プロトタイプ: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="ca610-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="ca610-104">[] の実装: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="ca610-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="ca610-105">[] 仕様: 以下</span><span class="sxs-lookup"><span data-stu-id="ca610-105">[ ] Specification: Below</span></span>

## <a name="summary"></a><span data-ttu-id="ca610-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="ca610-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ca610-107">変数が null の場合に値が割り当てられる、一般的なコーディングパターンを簡略化します。</span><span class="sxs-lookup"><span data-stu-id="ca610-107">Simplifies a common coding pattern where a variable is assigned a value if it is null.</span></span>

<span data-ttu-id="ca610-108">この提案の一環として、`??` の型の要件を緩和して、型が制約のない型パラメーターである式を左辺で使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ca610-108">As part of this proposal, we will also loosen the type requirements on `??` to allow an expression whose type is an unconstrained type parameter to be used on the left-hand side.</span></span>

## <a name="motivation"></a><span data-ttu-id="ca610-109">目的</span><span class="sxs-lookup"><span data-stu-id="ca610-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ca610-110">フォームのコードを確認するのが一般的です。</span><span class="sxs-lookup"><span data-stu-id="ca610-110">It is common to see code of the form</span></span>

```csharp
if (variable == null)
{
    variable = expression;
}
```

<span data-ttu-id="ca610-111">この提案は、この関数を実行する言語に、オーバーロードできない二項演算子を追加します。</span><span class="sxs-lookup"><span data-stu-id="ca610-111">This proposal adds a non-overloadable binary operator to the language that performs this function.</span></span>

<span data-ttu-id="ca610-112">この機能には、少なくとも8つの個別のコミュニティ要求があります。</span><span class="sxs-lookup"><span data-stu-id="ca610-112">There have been at least eight separate community requests for this feature.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="ca610-113">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="ca610-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ca610-114">新しいフォームの代入演算子を追加します。</span><span class="sxs-lookup"><span data-stu-id="ca610-114">We add a new form of assignment operator</span></span>

``` antlr
assignment_operator
    : '??='
    ;
```

<span data-ttu-id="ca610-115">これは、[複合代入演算子の既存のセマンティック規則](../../spec/expressions.md#compound-assignment)に従います。ただし、左辺が null 以外の場合は代入を解除します。</span><span class="sxs-lookup"><span data-stu-id="ca610-115">Which follows the [existing semantic rules for compound assignment operators](../../spec/expressions.md#compound-assignment), except that we elide the assignment if the left-hand side is non-null.</span></span> <span data-ttu-id="ca610-116">この機能の規則は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ca610-116">The rules for this feature are as follows.</span></span>

<span data-ttu-id="ca610-117">`a ??= b`、`A` は `a`の種類、`B` は `b`の種類、`A0` が null 許容値型である場合は `A` の基になる型です。`A`</span><span class="sxs-lookup"><span data-stu-id="ca610-117">Given `a ??= b`, where `A` is the type of `a`, `B` is the type of `b`, and `A0` is the underlying type of `A` if `A` is a nullable value type:</span></span>

1. <span data-ttu-id="ca610-118">`A` が存在しないか、null 非許容の値型である場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ca610-118">If `A` does not exist or is a non-nullable value type, a compile-time error occurs.</span></span>
2. <span data-ttu-id="ca610-119">`B` が `A` または `A0` に暗黙的に変換できない場合 (`A0` 存在する場合)、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ca610-119">If `B` is not implicitly convertible to `A` or `A0` (if `A0` exists), a compile-time error occurs.</span></span>
3. <span data-ttu-id="ca610-120">`A0` が存在し、`B` が暗黙的に `A0`に変換可能で、`B` が動的でない場合、`a ??= b` の型は `A0`になります。</span><span class="sxs-lookup"><span data-stu-id="ca610-120">If `A0` exists and `B` is implicitly convertible to `A0`, and `B` is not dynamic, then the type of `a ??= b` is `A0`.</span></span> <span data-ttu-id="ca610-121">`a ??= b` は、次のように実行時に評価されます。</span><span class="sxs-lookup"><span data-stu-id="ca610-121">`a ??= b` is evaluated at runtime as:</span></span>
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   <span data-ttu-id="ca610-122">ただし `a` が評価されるのは1回だけです。</span><span class="sxs-lookup"><span data-stu-id="ca610-122">Except that `a` is only evaluated once.</span></span>
4. <span data-ttu-id="ca610-123">それ以外の場合、`a ??= b` の種類は `A`です。</span><span class="sxs-lookup"><span data-stu-id="ca610-123">Otherwise, the type of `a ??= b` is `A`.</span></span> <span data-ttu-id="ca610-124">`a ??= b` は `a ?? (a = b)`として実行時に評価されますが、`a` は1回だけ評価される点が異なります。</span><span class="sxs-lookup"><span data-stu-id="ca610-124">`a ??= b` is evaluated at runtime as `a ?? (a = b)`, except that `a` is only evaluated once.</span></span>


<span data-ttu-id="ca610-125">`??`の型要件の緩和については、現在のところ、`a ?? b`の場合、`A` は `a`の種類であるという仕様を更新します。</span><span class="sxs-lookup"><span data-stu-id="ca610-125">For the relaxation of the type requirements of `??`, we update the spec where it currently states that, given `a ?? b`, where `A` is the type of `a`:</span></span>

> 1. <span data-ttu-id="ca610-126">が存在し、null 許容型または参照型でない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ca610-126">If A exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>

<span data-ttu-id="ca610-127">次の要件を緩和します。</span><span class="sxs-lookup"><span data-stu-id="ca610-127">We relax this requirement to:</span></span>

1. <span data-ttu-id="ca610-128">が存在し、null 非許容の値型である場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ca610-128">If A exists and is a non-nullable value type, a compile-time error occurs.</span></span>

<span data-ttu-id="ca610-129">これにより、制約のない型パラメーター T が存在し、が null 許容型ではなく、参照型ではないため、null 合体演算子を制約のない型パラメーターで使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca610-129">This allows the null coalescing operator to work on unconstrained type parameters, as the unconstrained type parameter T exists, is not a nullable type, and is not a reference type.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="ca610-130">短所</span><span class="sxs-lookup"><span data-stu-id="ca610-130">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ca610-131">他の言語機能と同様に、この機能の恩恵を受けるプログラムのC#本文に対して、さらに複雑な言語を端的にする必要があるかどうかを質問する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca610-131">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ca610-132">代替</span><span class="sxs-lookup"><span data-stu-id="ca610-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ca610-133">プログラマは、手動で `(x = x ?? y)`、`if (x == null) x = y;`、または `x ?? (x = y)` を作成できます。</span><span class="sxs-lookup"><span data-stu-id="ca610-133">The programmer can write `(x = x ?? y)`, `if (x == null) x = y;`, or `x ?? (x = y)` by hand.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="ca610-134">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="ca610-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="ca610-135">[] には LDM レビューが必要です</span><span class="sxs-lookup"><span data-stu-id="ca610-135">[ ] Requires LDM review</span></span>
- <span data-ttu-id="ca610-136">[] `&&=` および `||=` の演算子もサポートする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="ca610-136">[ ] Should we also support `&&=` and `||=` operators?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="ca610-137">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="ca610-137">Design meetings</span></span>

<span data-ttu-id="ca610-138">[なし] :</span><span class="sxs-lookup"><span data-stu-id="ca610-138">None.</span></span>
