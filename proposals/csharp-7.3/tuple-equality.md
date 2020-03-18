---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484142"
---
# <a name="support-for--and--on-tuple-types"></a><span data-ttu-id="3a1f5-101">タプル型での = = および! = のサポート</span><span class="sxs-lookup"><span data-stu-id="3a1f5-101">Support for == and != on tuple types</span></span>

<span data-ttu-id="3a1f5-102">式を許可します。 `t1` と `t2` が同じカーディナリティのタプル型または null 許容型のタプル型であり、それらをほぼ `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` (`var temp1 = t1; var temp2 = t2;`と想定) として評価 `t1 == t2` ます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-102">Allow expressions `t1 == t2` where `t1` and `t2` are tuple or nullable tuple types of same cardinality, and evaluate them roughly as `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` (assuming `var temp1 = t1; var temp2 = t2;`).</span></span>

<span data-ttu-id="3a1f5-103">逆に、`t1 != t2` を許可し、`temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`として評価します。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-103">Conversely it would allow `t1 != t2` and evaluate it as `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`.</span></span>

<span data-ttu-id="3a1f5-104">Null 許容の場合は、`temp1.HasValue` と `temp2.HasValue` の追加チェックが使用されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-104">In the nullable case, additional checks for `temp1.HasValue` and `temp2.HasValue` are used.</span></span> <span data-ttu-id="3a1f5-105">たとえば、`nullableT1 == nullableT2` は `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`として評価されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-105">For instance, `nullableT1 == nullableT2` evaluates as `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`.</span></span>

<span data-ttu-id="3a1f5-106">要素ごとの比較でブール以外の結果が返された場合 (たとえば、ブール型以外のユーザー定義 `operator ==` または `operator !=` が使用されている場合、または動的比較の場合)、その結果は `bool` に変換されるか、`operator true` または `operator false` を使用して実行され、`bool`を取得します。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-106">When an element-wise comparison returns a non-bool result (for instance, when a non-bool user-defined `operator ==` or `operator !=` is used, or in a dynamic comparison), then that result will be either converted to `bool` or run through `operator true` or `operator false` to get a `bool`.</span></span> <span data-ttu-id="3a1f5-107">タプルの比較では、常に `bool`が返されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-107">The tuple comparison always ends up returning a `bool`.</span></span>

<span data-ttu-id="3a1f5-108">7\.2 のC#場合、ユーザー定義の `operator==`がない限り、このようなコードではエラー (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`) が生成されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-108">As of C# 7.2, such code produces an error (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`), unless there is a user-defined `operator==`.</span></span>

## <a name="details"></a><span data-ttu-id="3a1f5-109">詳細</span><span class="sxs-lookup"><span data-stu-id="3a1f5-109">Details</span></span>

<span data-ttu-id="3a1f5-110">`==` (または `!=`) 演算子をバインドする場合、既存のルールは次のようになります (1) 動的なケース、(2) オーバーロードの解決、(3) が失敗します。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-110">When binding the `==` (or `!=`) operator, the existing rules are: (1) dynamic case, (2) overload resolution, and (3) fail.</span></span>
<span data-ttu-id="3a1f5-111">この提案は、(1) と (2) の間に組の大文字と小文字を追加します。比較演算子の両方のオペランドがタプル (タプル型またはタプルリテラル) で、一致するカーディナリティがある場合、比較は要素ごとに実行されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-111">This proposal adds a tuple case between (1) and (2): if both operands of a comparison operator are tuples (have tuple types or are tuple literals) and have matching cardinality, then the comparison is performed element-wise.</span></span> <span data-ttu-id="3a1f5-112">この組の等値は、null 許容の組にもリフトされています。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-112">This tuple equality is also lifted onto nullable tuples.</span></span>

<span data-ttu-id="3a1f5-113">両方のオペランド (および組リテラルの場合は、その要素) が左から右に評価されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-113">Both operands (and, in the case of tuple literals, their elements) are evaluated in order from left to right.</span></span> <span data-ttu-id="3a1f5-114">次に、要素の各ペアをオペランドとして使用して、演算子 `==` (または `!=`) を再帰的にバインドします。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-114">Each pair of elements is then used as operands to bind the operator `==` (or `!=`), recursively.</span></span> <span data-ttu-id="3a1f5-115">コンパイル時の型 `dynamic` を持つ要素では、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-115">Any elements with compile-time type `dynamic` cause an error.</span></span> <span data-ttu-id="3a1f5-116">これらの要素ごとの比較の結果は、条件演算子と (または) 演算子のチェーンでオペランドとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-116">The results of those element-wise comparisons are used as operands in a chain of conditional AND (or OR) operators.</span></span>

<span data-ttu-id="3a1f5-117">たとえば、`(int, (int, int)) t1, t2;`のコンテキストでは、`t1 == (1, (2, 3))` は `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`として評価されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-117">For instance, in the context of `(int, (int, int)) t1, t2;`, `t1 == (1, (2, 3))` would evaluate as `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`.</span></span>

<span data-ttu-id="3a1f5-118">組リテラルをオペランドとして使用すると (両側で)、要素ごとの変換によって形成される、変換されたタプル型を受け取ります。これは、演算子を `==` (または `!=`) 要素ごとにバインドするときに導入されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-118">When a tuple literal is used as operand (on either side), it receives a converted tuple type formed by the element-wise conversions which are introduced when binding the operator `==` (or `!=`) element-wise.</span></span> 

<span data-ttu-id="3a1f5-119">たとえば、`(1L, 2, "hello") == (1, 2L, null)`では、両方の組リテラルの変換後の型が `(long, long, string)`、2番目のリテラルには自然型がありません。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-119">For instance, in `(1L, 2, "hello") == (1, 2L, null)`, the converted type for both tuple literals is `(long, long, string)` and the second literal has no natural type.</span></span>


### <a name="deconstruction-and-conversions-to-tuple"></a><span data-ttu-id="3a1f5-120">分解と組への変換</span><span class="sxs-lookup"><span data-stu-id="3a1f5-120">Deconstruction and conversions to tuple</span></span>
<span data-ttu-id="3a1f5-121">`(a, b) == x`では、`x` が2つの要素に分解ことができるという事実は、役割を果たしません。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-121">In `(a, b) == x`, the fact that `x` can deconstruct into two elements does not play a role.</span></span> <span data-ttu-id="3a1f5-122">これは今後の提案になる可能性がありますが、`x == y` に関する質問が発生する可能性があります (これは、単純な比較または要素ごとの比較ですが、カーディナリティを使用している場合)。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-122">That could conceivably be in a future proposal, although it would raise questions about `x == y` (is this a simple comparison or an element-wise comparison, and if so using what cardinality?).</span></span>
<span data-ttu-id="3a1f5-123">同様に、タプルへの変換はロールを持ちません。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-123">Similarly, conversions to tuple play no role.</span></span>

### <a name="tuple-element-names"></a><span data-ttu-id="3a1f5-124">タプル要素名</span><span class="sxs-lookup"><span data-stu-id="3a1f5-124">Tuple element names</span></span>

<span data-ttu-id="3a1f5-125">タプルリテラルを変換するときに、リテラルに明示的なタプル要素名が指定されているが、ターゲットのタプル要素名と一致しない場合に警告します。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-125">When converting a tuple literal, we warn when an explicit tuple element name was provided in the literal, but it doesn't match the target tuple element name.</span></span>
<span data-ttu-id="3a1f5-126">組の比較でも同じ規則を使用するので、`t == (c, d: 0)`で `d` に対して警告 `(int a, int b) t` ことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-126">We use the same rule in tuple comparison, so that assuming `(int a, int b) t` we warn on `d` in `t == (c, d: 0)`.</span></span>

### <a name="non-bool-element-wise-comparison-results"></a><span data-ttu-id="3a1f5-127">Bool 以外の要素ごとの比較結果</span><span class="sxs-lookup"><span data-stu-id="3a1f5-127">Non-bool element-wise comparison results</span></span>

<span data-ttu-id="3a1f5-128">要素ごとの比較が組の等価性で動的である場合は、演算子 `false` の動的呼び出しを使用して、`bool` を取得し、さらに要素ごとの比較を続行するようにします。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-128">If an element-wise comparison is dynamic in a tuple equality, we use a dynamic invocation of the operator `false` and negate that to get a `bool` and continue with further element-wise comparisons.</span></span> 

<span data-ttu-id="3a1f5-129">要素ごとの比較によってタプル内の他の非 bool 型が返される場合は、次の2つのケースがあります。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-129">If an element-wise comparison returns some other non-bool type in a tuple equality, there are two cases:</span></span>
- <span data-ttu-id="3a1f5-130">ブール型以外の型が `bool`に変換される場合は、その変換を適用します。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-130">if the non-bool type converts to `bool`, we apply that conversion,</span></span>
- <span data-ttu-id="3a1f5-131">そのような変換がないが、型に `false`演算子がある場合は、それを使用して結果を否定します。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-131">if there is no such conversion, but the type has an operator `false`, we'll use that and negate the result.</span></span>

<span data-ttu-id="3a1f5-132">組の非等値では、演算子 `false`の代わりに演算子 `true` (否定なし) を使用する点を除いて、同じ規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-132">In a tuple inequality, the same rules apply except that we'll use the operator `true` (without negation) instead of the operator `false`.</span></span>

<span data-ttu-id="3a1f5-133">これらの規則は、`if` ステートメントおよびその他の既存のコンテキストでブール型以外の型を使用する場合に関係する規則に似ています。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-133">Those rules are similar to the rules involved for using a non-bool type in an `if` statement and some other existing contexts.</span></span>

## <a name="evaluation-order-and-special-cases"></a><span data-ttu-id="3a1f5-134">評価順序と特別なケース</span><span class="sxs-lookup"><span data-stu-id="3a1f5-134">Evaluation order and special cases</span></span>
<span data-ttu-id="3a1f5-135">左側の値が最初に評価され、次に右側の値が評価されます。次に、左から右 (変換を含む) と、条件付き演算子または OR 演算子の既存の規則に基づく早期終了を含む要素ごとの比較が行われます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-135">The left-hand-side value is evaluated first, then the right-hand-side value, then the element-wise comparisons from left to right (including conversions, and with early exit based on existing rules for conditional AND/OR operators).</span></span>

<span data-ttu-id="3a1f5-136">たとえば、型 `A` から型 `B` とメソッド `(A, A) GetTuple()`の変換がある場合、`(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` を評価すると次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-136">For instance, if there is a conversion from type `A` to type `B` and a method `(A, A) GetTuple()`, evaluating `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` means:</span></span>
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- <span data-ttu-id="3a1f5-137">次に、要素ごとの変換と比較および条件ロジックが評価されます (`new A(1)` を型 `B`に変換し、それを `new B(4)`と比較します)。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-137">then the element-wise conversions and comparisons and conditional logic is evaluated (convert `new A(1)` to type `B`, then compare it with `new B(4)`, and so on).</span></span>

### <a name="comparing-null-to-null"></a><span data-ttu-id="3a1f5-138">`null` と `null` の比較</span><span class="sxs-lookup"><span data-stu-id="3a1f5-138">Comparing `null` to `null`</span></span>

<span data-ttu-id="3a1f5-139">これは、通常の比較の特殊なケースで、タプルの比較に引き継がれます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-139">This is a special case from regular comparisons, that carries over to tuple comparisons.</span></span> <span data-ttu-id="3a1f5-140">`null == null` 比較が許可され、`null` リテラルには型が一切取得されません。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-140">The `null == null` comparison is allowed, and the `null` literals do not get any type.</span></span>
<span data-ttu-id="3a1f5-141">タプルの等価性では、これは `(0, null) == (0, null)` も許可され、`null` と組リテラルは型を取得しません。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-141">In tuple equality, this means, `(0, null) == (0, null)` is also allowed and the `null` and tuple literals don't get a type either.</span></span>

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a><span data-ttu-id="3a1f5-142">Null 許容の構造体と `null` を比較した場合 `operator==`</span><span class="sxs-lookup"><span data-stu-id="3a1f5-142">Comparing a nullable struct to `null` without `operator==`</span></span>

<span data-ttu-id="3a1f5-143">これは、タプルの比較に関する別の特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-143">This is another special case from regular comparisons, that carries over to tuple comparisons.</span></span>
<span data-ttu-id="3a1f5-144">`operator==`のない `struct S` がある場合は、`(S?)x == null` の比較が許可され、`((S?).x).HasValue`として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-144">If you have a `struct S` without `operator==`, the `(S?)x == null` comparison is allowed, and it is interpreted as `((S?).x).HasValue`.</span></span>
<span data-ttu-id="3a1f5-145">タプルの等価性では、同じルールが適用されるので `(0, (S?)x) == (0, null)` が許可されます。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-145">In tuple equality, the same rule is applied, so `(0, (S?)x) == (0, null)` is allowed.</span></span>

## <a name="compatibility"></a><span data-ttu-id="3a1f5-146">互換性</span><span class="sxs-lookup"><span data-stu-id="3a1f5-146">Compatibility</span></span>

<span data-ttu-id="3a1f5-147">比較演算子の実装を使用して、他のユーザーが独自の `ValueTuple` 型を記述した場合は、オーバーロードの解決によって以前に取得されていた可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-147">If someone wrote their own `ValueTuple` types with  an implementation of the comparison operator, it would have previously been picked up by overload resolution.</span></span> <span data-ttu-id="3a1f5-148">ただし、新しい組のケースはオーバーロードの解決の前にあるため、ユーザー定義の比較に頼るのではなく、タプルの比較でこのケースを処理します。</span><span class="sxs-lookup"><span data-stu-id="3a1f5-148">But since the new tuple case comes before overload resolution, we would handle this case with tuple comparison instead of relying on the user-defined comparison.</span></span>

----

<span data-ttu-id="3a1f5-149">[関係演算子および型テスト演算子](../../spec/expressions.md#relational-and-type-testing-operators)に関連する[#190](https://github.com/dotnet/csharplang/issues/190)</span><span class="sxs-lookup"><span data-stu-id="3a1f5-149">Relates to [relational and type testing operators](../../spec/expressions.md#relational-and-type-testing-operators) Relates to [#190](https://github.com/dotnet/csharplang/issues/190)</span></span>
