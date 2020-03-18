---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "79483914"
---
# <a name="target-typed-default-literal"></a><span data-ttu-id="3a4b1-101">ターゲット型の "default" リテラル</span><span class="sxs-lookup"><span data-stu-id="3a4b1-101">Target-typed "default" literal</span></span>

* <span data-ttu-id="3a4b1-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="3a4b1-102">[x] Proposed</span></span>
* <span data-ttu-id="3a4b1-103">[x] プロトタイプ</span><span class="sxs-lookup"><span data-stu-id="3a4b1-103">[x] Prototype</span></span>
* <span data-ttu-id="3a4b1-104">[x] 実装</span><span class="sxs-lookup"><span data-stu-id="3a4b1-104">[x] Implementation</span></span>
* <span data-ttu-id="3a4b1-105">[] 仕様</span><span class="sxs-lookup"><span data-stu-id="3a4b1-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="3a4b1-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="3a4b1-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3a4b1-107">ターゲット型の `default` 機能は、`default(T)` 演算子の短い形式のバリエーションであり、型を省略できます。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-107">The target-typed `default` feature is a shorter form variation of the `default(T)` operator, which allows the type to be omitted.</span></span> <span data-ttu-id="3a4b1-108">その型は、代わりにターゲット入力によって推論されます。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-108">Its type is inferred by target-typing instead.</span></span> <span data-ttu-id="3a4b1-109">それ以外の場合は、`default(T)`のように動作します。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-109">Aside from that, it behaves like `default(T)`.</span></span>

## <a name="motivation"></a><span data-ttu-id="3a4b1-110">目的</span><span class="sxs-lookup"><span data-stu-id="3a4b1-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3a4b1-111">主な動機は、冗長な情報を入力しないようにすることです。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-111">The main motivation is to avoid typing redundant information.</span></span>

<span data-ttu-id="3a4b1-112">たとえば、`void Method(ImmutableArray<SomeType> array)`を呼び出すときに、*既定*のリテラルを使用すると `M(default(ImmutableArray<SomeType>))`の代わりに `M(default)` できます。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-112">For instance, when invoking `void Method(ImmutableArray<SomeType> array)`, the *default* literal allows `M(default)` in place of `M(default(ImmutableArray<SomeType>))`.</span></span>

<span data-ttu-id="3a4b1-113">これは、次のようなさまざまなシナリオで適用されます。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-113">This is applicable in a number of scenarios, such as:</span></span>

- <span data-ttu-id="3a4b1-114">ローカルの宣言 (`ImmutableArray<SomeType> x = default;`)</span><span class="sxs-lookup"><span data-stu-id="3a4b1-114">declaring locals (`ImmutableArray<SomeType> x = default;`)</span></span>
- <span data-ttu-id="3a4b1-115">三項演算 (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span><span class="sxs-lookup"><span data-stu-id="3a4b1-115">ternary operations (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span></span>
- <span data-ttu-id="3a4b1-116">メソッドとラムダを返す (`return default;`)</span><span class="sxs-lookup"><span data-stu-id="3a4b1-116">returning in methods and lambdas (`return default;`)</span></span>
- <span data-ttu-id="3a4b1-117">省略可能なパラメーターの既定値の宣言 (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span><span class="sxs-lookup"><span data-stu-id="3a4b1-117">declaring default values for optional parameters (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span></span>
- <span data-ttu-id="3a4b1-118">配列作成式に既定値を含める (`var x = new[] { default, ImmutableArray.Create(y) };`)</span><span class="sxs-lookup"><span data-stu-id="3a4b1-118">including default values in array creation expressions (`var x = new[] { default, ImmutableArray.Create(y) };`)</span></span>


## <a name="detailed-design"></a><span data-ttu-id="3a4b1-119">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="3a4b1-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="3a4b1-120">新しい式が導入されました。*既定*のリテラルです。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-120">A new expression is introduced, the *default* literal.</span></span> <span data-ttu-id="3a4b1-121">この分類の式は、*既定のリテラル変換*によって、任意の型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-121">An expression with this classification can be implicitly converted to any type, by a *default literal conversion*.</span></span> 

<span data-ttu-id="3a4b1-122">*既定*のリテラルの型の推論は、 *null*リテラルの場合と同じように動作します。ただし、参照型だけでなく、任意の型が許可される点が異なります。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-122">The inference of the type for the *default* literal works the same as that for the *null* literal, except that any type is allowed (not just reference types).</span></span>

<span data-ttu-id="3a4b1-123">この変換により、推定される型の既定値が生成されます。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-123">This conversion produces the default value of the inferred type.</span></span>

<span data-ttu-id="3a4b1-124">*既定*のリテラルは、推論された型に応じて定数値を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-124">The *default* literal may have a constant value, depending on the inferred type.</span></span> <span data-ttu-id="3a4b1-125">`const int x = default;` は有効ですが、`const int? y = default;` は有効ではありません。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-125">So `const int x = default;` is legal, but `const int? y = default;` is not.</span></span>

<span data-ttu-id="3a4b1-126">*既定*のリテラルは、他のオペランドが型を持つ限り、等値演算子のオペランドにすることができます。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-126">The *default* literal can be the operand of equality operators, as long as the other operand has a type.</span></span> <span data-ttu-id="3a4b1-127">`default == x` と `x == default` は有効な式ですが、`default == default` は無効です。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-127">So `default == x` and `x == default` are valid expressions, but `default == default` is illegal.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="3a4b1-128">短所</span><span class="sxs-lookup"><span data-stu-id="3a4b1-128">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="3a4b1-129">小さな欠点は、ほとんどのコンテキストで*既定*のリテラルを*null*リテラルの代わりに使用できることです。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-129">A minor drawback is that *default* literal can be used in place of *null* literal in most contexts.</span></span> <span data-ttu-id="3a4b1-130">2つの例外は `throw null;` と `null == null`であり、 *null*リテラルでは許可されますが、*既定*のリテラルは使用できません。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-130">Two of the exceptions are `throw null;` and `null == null`, which are allowed for the *null* literal, but not the *default* literal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="3a4b1-131">代替</span><span class="sxs-lookup"><span data-stu-id="3a4b1-131">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3a4b1-132">考慮すべき選択肢がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-132">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="3a4b1-133">現状では、この機能は独自のメリットには合わないため、開発者は、明示的な型で既定の演算子を引き続き使用します。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-133">The status quo:  The feature is not justified on its own merits and developers continue to use the default operator with an explicit type.</span></span>
- <span data-ttu-id="3a4b1-134">Null リテラルの拡張: これは `Nothing`を使用した VB のアプローチです。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-134">Extending the null literal: This is the VB approach with `Nothing`.</span></span> <span data-ttu-id="3a4b1-135">`int x = null;`を許可することができます。</span><span class="sxs-lookup"><span data-stu-id="3a4b1-135">We could allow `int x = null;`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="3a4b1-136">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="3a4b1-136">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="3a4b1-137">[x]*は、is*演算子または*as*演算子のオペランドとして*既定*で許可されますか?</span><span class="sxs-lookup"><span data-stu-id="3a4b1-137">[x] Should *default* be allowed as the operand of the *is* or *as* operators?</span></span> <span data-ttu-id="3a4b1-138">回答: `default is T`を許可しない、`x is default`を許可する、`default as RefType` を許可する (常に null 警告を含む)</span><span class="sxs-lookup"><span data-stu-id="3a4b1-138">Answer:  disallow `default is T`, allow `x is default`, allow `default as RefType` (with always-null warning)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="3a4b1-139">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="3a4b1-139">Design meetings</span></span>

- [<span data-ttu-id="3a4b1-140">LDM 3/7/2017</span><span class="sxs-lookup"><span data-stu-id="3a4b1-140">LDM 3/7/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [<span data-ttu-id="3a4b1-141">LDM 3/28/2017</span><span class="sxs-lookup"><span data-stu-id="3a4b1-141">LDM 3/28/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [<span data-ttu-id="3a4b1-142">LDM 5/31/2017</span><span class="sxs-lookup"><span data-stu-id="3a4b1-142">LDM 5/31/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
