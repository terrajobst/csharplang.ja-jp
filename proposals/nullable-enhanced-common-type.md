---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483524"
---
# <a name="nullable-enhanced-common-type"></a><span data-ttu-id="ec22a-101">Null 許容型拡張共通型</span><span class="sxs-lookup"><span data-stu-id="ec22a-101">Nullable-Enhanced Common Type</span></span>

* <span data-ttu-id="ec22a-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="ec22a-102">[x] Proposed</span></span>
* <span data-ttu-id="ec22a-103">[] プロトタイプ: なし</span><span class="sxs-lookup"><span data-stu-id="ec22a-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="ec22a-104">[] の実装: なし</span><span class="sxs-lookup"><span data-stu-id="ec22a-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="ec22a-105">[] 仕様: 以下を参照してください</span><span class="sxs-lookup"><span data-stu-id="ec22a-105">[ ] Specification: See below</span></span>

## <a name="summary"></a><span data-ttu-id="ec22a-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="ec22a-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ec22a-107">現在の共通型アルゴリズムの結果が非常に直観的で、コードに冗長なキャストのような感覚を追加するという状況があります。</span><span class="sxs-lookup"><span data-stu-id="ec22a-107">There is a situation in which the current common-type algorithm results are counter-intuitive, and results in the programmer adding what feels like a redundant cast to the code.</span></span> <span data-ttu-id="ec22a-108">この変更により、`condition ? 1 : null` などの式では `int?`型の値が返され、`condition ? x : 1.0` などの式では、`x` が型 `int?` の場合は `double?`型の値になります。</span><span class="sxs-lookup"><span data-stu-id="ec22a-108">With this change, an expression such as `condition ? 1 : null` would result in a value of type `int?`, and an expression such as `condition ? x : 1.0` where `x` is of type `int?` would result in a value of type `double?`.</span></span>

## <a name="motivation"></a><span data-ttu-id="ec22a-109">目的</span><span class="sxs-lookup"><span data-stu-id="ec22a-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ec22a-110">これは、不要な定型コードのようにプログラマが感じていることの一般的な原因です。</span><span class="sxs-lookup"><span data-stu-id="ec22a-110">This is a common cause of what feels to the programmer like needless boilerplate code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="ec22a-111">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="ec22a-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ec22a-112">次の状況に影響を与える、[一連の式の中で最適な共通型を見つける](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions)ための仕様を変更します。</span><span class="sxs-lookup"><span data-stu-id="ec22a-112">We modify the specification for [finding the best common type of a set of expressions](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) to affect the following situations:</span></span>

- <span data-ttu-id="ec22a-113">一方の式が null 非許容の値型 `T` で、もう一方が null リテラルの場合、結果は `T?`型になります。</span><span class="sxs-lookup"><span data-stu-id="ec22a-113">If one expression is of a non-nullable value type `T` and the other is a null literal, the result is of type `T?`.</span></span>
- <span data-ttu-id="ec22a-114">一方の式が null 許容値型 `T?` であり、もう一方の式が値型 `U`であり、`T` から `U`への暗黙的な変換がある場合、結果は `U?`型になります。</span><span class="sxs-lookup"><span data-stu-id="ec22a-114">If one expression is of a nullable value type `T?` and the other is of a value type `U`, and there is an implicit conversion from `T` to `U`, then the result is of type `U?`.</span></span>

<span data-ttu-id="ec22a-115">これは、次の言語の側面に影響します。</span><span class="sxs-lookup"><span data-stu-id="ec22a-115">This is expected to affect the following aspects of the language:</span></span>

- <span data-ttu-id="ec22a-116">[三項式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span><span class="sxs-lookup"><span data-stu-id="ec22a-116">the [ternary expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span></span>
- <span data-ttu-id="ec22a-117">暗黙的に型指定された[配列作成式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span><span class="sxs-lookup"><span data-stu-id="ec22a-117">implicitly typed [array creation expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span></span>
- <span data-ttu-id="ec22a-118">型の推定に関する[ラムダの戻り値の型](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type)の推論</span><span class="sxs-lookup"><span data-stu-id="ec22a-118">inferring the [return type of a lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) for type inference</span></span>
- <span data-ttu-id="ec22a-119">`M(1, null)`としての `M<T>(T a, T b)` の呼び出しなど、ジェネリックを含むケース。</span><span class="sxs-lookup"><span data-stu-id="ec22a-119">cases involving generics, such as invoking `M<T>(T a, T b)` as `M(1, null)`.</span></span>

<span data-ttu-id="ec22a-120">詳細については、仕様の次のセクションを変更します (太字で挿入し、取り消し線を削除します)。</span><span class="sxs-lookup"><span data-stu-id="ec22a-120">More precisely, we change the following sections of the specification (insertions in bold, deletions in strikethrough):</span></span>

> #### <a name="output-type-inferences"></a><span data-ttu-id="ec22a-121">出力の型の推論</span><span class="sxs-lookup"><span data-stu-id="ec22a-121">Output type inferences</span></span>
> 
> <span data-ttu-id="ec22a-122">*出力型の推論*は、次のように `T` 型*に*`E` 式*から*実行されます。</span><span class="sxs-lookup"><span data-stu-id="ec22a-122">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>
> 
> *  <span data-ttu-id="ec22a-123">`E` が、推論された戻り値の型 `U` ([推定戻り値の型](expressions.md#inferred-return-type)) の匿名関数で、`T` が戻り値の型が `Tb`であるデリゲート型または式ツリー型である場合は、*下限の推論*([下限](expressions.md#lower-bound-inferences)の推論) が `U`*から*`Tb`*に*作成されます。</span><span class="sxs-lookup"><span data-stu-id="ec22a-123">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="ec22a-124">それ以外の場合、`E` がメソッドグループで、`T` がパラメーター型 `T1...Tk` および戻り値の型 `Tb`を持つデリゲート型または式ツリー型であり、型 `E` のオーバーロード解決では、戻り値の型が *`T1...Tk` の 1*つのメソッドが生成*to* *され*ます。`U``U``Tb`</span><span class="sxs-lookup"><span data-stu-id="ec22a-124">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="ec22a-125">\* \* それ以外の場合、`E` が null 許容値型 `U?`の式である場合、`U`*から*`T`*に* *下限の推定*が行われ、 *null バインド*が `T`に追加されます。</span><span class="sxs-lookup"><span data-stu-id="ec22a-125">\*\*Otherwise, if `E` is an expression with nullable value type `U?`, then a *lower-bound inference* is made *from* `U` *to* `T` and a *null bound* is added to `T`.</span></span> **
> *  <span data-ttu-id="ec22a-126">それ以外の場合、`E` が `U`型の式である場合、`U`*から*`T`*に* *下限の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="ec22a-126">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
> *  <span data-ttu-id="ec22a-127">**それ以外の場合、`E` が値 `null`の定数式である場合、 *null バインド*がに追加され `T`**</span><span class="sxs-lookup"><span data-stu-id="ec22a-127">**Otherwise, if `E` is a constant expression with value `null`, then a *null bound* is added to `T`**</span></span> 
> *  <span data-ttu-id="ec22a-128">それ以外の場合、推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="ec22a-128">Otherwise, no inferences are made.</span></span>

> #### <a name="fixing"></a><span data-ttu-id="ec22a-129">写真の</span><span class="sxs-lookup"><span data-stu-id="ec22a-129">Fixing</span></span>
> 
> <span data-ttu-id="ec22a-130">一連の境界を持つ固定されてい*ない型変数*`Xi` は、次のように*固定*されています。</span><span class="sxs-lookup"><span data-stu-id="ec22a-130">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>
> 
> *  <span data-ttu-id="ec22a-131">*候補の種類*のセット `Uj`、`Xi`の一連の境界内のすべての型のセットとして開始されます。</span><span class="sxs-lookup"><span data-stu-id="ec22a-131">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
> *  <span data-ttu-id="ec22a-132">次に、それぞれのバインドされている `Xi` を確認します。 `Xi` の完全バインド `U` については、`U` と同じではないすべての型 `Uj` を候補セットから削除します。</span><span class="sxs-lookup"><span data-stu-id="ec22a-132">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="ec22a-133">`Xi` の下限 `U` ごとに、`U` からの暗黙の変換が*行われていない*すべての型 `Uj` 候補セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="ec22a-133">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="ec22a-134">`U` への暗黙的な変換が*行われていない*すべての型 `Uj` `Xi` の上限 `U` は、候補セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="ec22a-134">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
> *  <span data-ttu-id="ec22a-135">他の候補の型の中に、他のすべての候補の型への暗黙の型変換が存在する `Uj` 一意の型 `V` がある場合、 ~~`Xi` は `V`に固定されます。~~</span><span class="sxs-lookup"><span data-stu-id="ec22a-135">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then ~~`Xi` is fixed to `V`.~~</span></span>
>     -  <span data-ttu-id="ec22a-136">**`V` が値型で、`Xi`に null が*バインド*されている場合、`Xi` はに固定され `V?`**</span><span class="sxs-lookup"><span data-stu-id="ec22a-136">**If `V` is a value type and there is a *null bound* for `Xi`, then `Xi` is fixed to `V?`**</span></span>
>     -  <span data-ttu-id="ec22a-137">**それ以外の場合 `Xi` は `V` に固定されます。**</span><span class="sxs-lookup"><span data-stu-id="ec22a-137">**Otherwise   `Xi` is fixed to `V`**</span></span>
> *  <span data-ttu-id="ec22a-138">それ以外の場合、型の推定は失敗します。</span><span class="sxs-lookup"><span data-stu-id="ec22a-138">Otherwise, type inference fails.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="ec22a-139">短所</span><span class="sxs-lookup"><span data-stu-id="ec22a-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ec22a-140">この提案によって導入された非互換性がある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ec22a-140">There may be some incompatibilities introduced by this proposal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ec22a-141">代替</span><span class="sxs-lookup"><span data-stu-id="ec22a-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ec22a-142">[なし] :</span><span class="sxs-lookup"><span data-stu-id="ec22a-142">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="ec22a-143">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="ec22a-143">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="ec22a-144">[] この提案によって導入された非互換性の重要度 (存在する場合) と、それをモデレートする方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="ec22a-144">[ ] What is the severity of incompatibility introduced by this proposal, if any, and how can it be moderated?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="ec22a-145">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="ec22a-145">Design meetings</span></span>

<span data-ttu-id="ec22a-146">[なし] :</span><span class="sxs-lookup"><span data-stu-id="ec22a-146">None.</span></span>
