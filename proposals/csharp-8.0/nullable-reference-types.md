---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483596"
---
# <a name="nullable-reference-types-in-c"></a><span data-ttu-id="62dc8-101">での null 値が許容される参照型C#</span><span class="sxs-lookup"><span data-stu-id="62dc8-101">Nullable reference types in C#</span></span> #

<span data-ttu-id="62dc8-102">この機能の目的は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="62dc8-102">The goal of this feature is to:</span></span>

* <span data-ttu-id="62dc8-103">変数、パラメーター、または参照型の結果が null であるかどうかを開発者が表すことができるようにします。</span><span class="sxs-lookup"><span data-stu-id="62dc8-103">Allow developers to express whether a variable, parameter or result of a reference type is intended to be null or not.</span></span>
* <span data-ttu-id="62dc8-104">このような変数、パラメーター、および結果がその目的に従って使用されない場合は、警告を表示します。</span><span class="sxs-lookup"><span data-stu-id="62dc8-104">Provide warnings when such variables, parameters and results are not used according to that intent.</span></span>

## <a name="expression-of-intent"></a><span data-ttu-id="62dc8-105">インテントの式</span><span class="sxs-lookup"><span data-stu-id="62dc8-105">Expression of intent</span></span>

<span data-ttu-id="62dc8-106">この言語には、値型の `T?` 構文が既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="62dc8-106">The language already contains the `T?` syntax for value types.</span></span> <span data-ttu-id="62dc8-107">参照型にこの構文を拡張するのは簡単です。</span><span class="sxs-lookup"><span data-stu-id="62dc8-107">It is straightforward to extend this syntax to reference types.</span></span>

<span data-ttu-id="62dc8-108">非修飾参照型 `T` の目的は、null 以外にする必要があることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="62dc8-108">It is assumed that the intent of an unadorned reference type `T` is for it to be non-null.</span></span>

## <a name="checking-of-nullable-references"></a><span data-ttu-id="62dc8-109">Null 許容参照のチェック</span><span class="sxs-lookup"><span data-stu-id="62dc8-109">Checking of nullable references</span></span>

<span data-ttu-id="62dc8-110">フロー分析は、null 許容の参照変数を追跡します。</span><span class="sxs-lookup"><span data-stu-id="62dc8-110">A flow analysis tracks nullable reference variables.</span></span> <span data-ttu-id="62dc8-111">分析では、null ではないと判断された場合 (たとえば、チェックや代入の後)、その値は null 以外の参照と見なされます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-111">Where the analysis deems that they would not be null (e.g. after a check or an assignment), their value will be considered a non-null reference.</span></span>

<span data-ttu-id="62dc8-112">また、null 許容参照は、後置 `x!` 演算子 ("dammit" 演算子) を使用して null 以外の値として明示的に処理することもできます。これは、フロー分析が、開発者が知っている null 以外の状況を確立できない場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="62dc8-112">A nullable reference can also explicitly be treated as non-null with the postfix `x!` operator (the "dammit" operator), for when flow analysis cannot establish a non-null situation that the developer knows is there.</span></span>

<span data-ttu-id="62dc8-113">それ以外の場合、null 許容参照が逆参照されるか、null 以外の型に変換されると、警告が示されます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-113">Otherwise, a warning is given if a nullable reference is dereferenced, or is converted to a non-null type.</span></span>

<span data-ttu-id="62dc8-114">`S[]` から `T?[]`、`S?[]` から `T[]`に変換するときに警告が通知されます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-114">A warning is given when converting from `S[]` to `T?[]` and from `S?[]` to `T[]`.</span></span>

<span data-ttu-id="62dc8-115">型パラメーターが共変 (`out`) である場合や、型パラメーターが反変 (`C<T>`) の場合を除き、`C<S?>` から`in`に変換する場合を除き、`C<S>` から `C<T?>` への変換時に警告が通知されます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-115">A warning is given when converting from `C<S>` to `C<T?>` except when the type parameter is covariant (`out`), and when converting from `C<S?>` to `C<T>` except when the type parameter is contravariant (`in`).</span></span>

<span data-ttu-id="62dc8-116">型パラメーターに null 以外の制約がある場合、`C<T?>` で警告が示されます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-116">A warning is given on `C<T?>` if the type parameter has non-null constraints.</span></span> 

## <a name="checking-of-non-null-references"></a><span data-ttu-id="62dc8-117">Null 以外の参照のチェック</span><span class="sxs-lookup"><span data-stu-id="62dc8-117">Checking of non-null references</span></span>

<span data-ttu-id="62dc8-118">Null リテラルが null 以外の変数に割り当てられているか、null 以外のパラメーターとして渡された場合、警告が返されます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-118">A warning is given if a null literal is assigned to a non-null variable or passed as a non-null parameter.</span></span>

<span data-ttu-id="62dc8-119">コンストラクターが null 以外の参照フィールドを明示的に初期化しない場合も、警告が示されます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-119">A warning is also given if a constructor does not explicitly initialize non-null reference fields.</span></span>

<span data-ttu-id="62dc8-120">Null 以外の参照の配列のすべての要素が初期化されていることを適切に追跡することはできません。</span><span class="sxs-lookup"><span data-stu-id="62dc8-120">We cannot adequately track that all elements of an array of non-null references are initialized.</span></span> <span data-ttu-id="62dc8-121">ただし、新しく作成された配列の要素が割り当てられる前に、配列の読み取りまたは書き込みが行われる前に、警告を発行することができます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-121">However, we could issue a warning if no element of a newly created array is assigned to before the array is read from or passed on.</span></span> <span data-ttu-id="62dc8-122">これは、ノイズが発生することなく、一般的なケースを処理する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-122">That might handle the common case without being too noisy.</span></span>

<span data-ttu-id="62dc8-123">`default(T)` が警告を生成するかどうか、または単に `T?`型として処理するかどうかを決定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-123">We need to decide whether `default(T)` generates a warning, or is simply treated as being of the type `T?`.</span></span>

## <a name="metadata-representation"></a><span data-ttu-id="62dc8-124">メタデータ表現</span><span class="sxs-lookup"><span data-stu-id="62dc8-124">Metadata representation</span></span>

<span data-ttu-id="62dc8-125">Null 値許容要素は、メタデータで属性として表す必要があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-125">Nullability adornments should be represented in metadata as attributes.</span></span> <span data-ttu-id="62dc8-126">これは、ダウンレベルコンパイラによって無視されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="62dc8-126">This means that downlevel compilers will ignore them.</span></span>

<span data-ttu-id="62dc8-127">Null 値を許容する注釈だけを含めるかどうかを判断する必要があります。または、アセンブリ内で null 以外が "on" であるかどうかを示すこともできます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-127">We need to decide if only nullable annotations are included, or there's also some indication of whether non-null was "on" in the assembly.</span></span>

## <a name="generics"></a><span data-ttu-id="62dc8-128">ジェネリック</span><span class="sxs-lookup"><span data-stu-id="62dc8-128">Generics</span></span>

<span data-ttu-id="62dc8-129">型パラメーター `T` に null 非許容の制約がある場合、そのスコープ内で null 非許容として扱われます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-129">If a type parameter `T` has non-nullable constraints, it is treated as non-nullable within its scope.</span></span>

<span data-ttu-id="62dc8-130">型パラメーターに制約がない場合、または null 値を許容する制約がある場合、状況は少し複雑になります。これは、対応する型引数が null 許容または null 非許容の*どちらか*になる可能性があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="62dc8-130">If a type parameter is unconstrained or has only nullable constraints, the situation is a little more complex: this means that the corresponding type argument could be *either* nullable or non-nullable.</span></span> <span data-ttu-id="62dc8-131">そのような場合の安全な方法は、型パラメーターを null 許容型と null 非許容型の*両方*として扱い、いずれかの違反があったときに警告を発することです。</span><span class="sxs-lookup"><span data-stu-id="62dc8-131">The safe thing to do in that situation is to treat the type parameter as *both* nullable and non-nullable, giving warnings when either is violated.</span></span> 

<span data-ttu-id="62dc8-132">明示的に null 許容の参照制約を許可するかどうかを検討してください。</span><span class="sxs-lookup"><span data-stu-id="62dc8-132">It is worth considering whether explicit nullable reference constraints should be allowed.</span></span> <span data-ttu-id="62dc8-133">ただし、null 値を許容する参照型を使用することは、特定のケース (継承された制約) の制約として*暗黙的*に避けることができないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="62dc8-133">Note, however, that we cannot avoid having nullable reference types *implicitly* be constraints in certain cases (inherited constraints).</span></span>

<span data-ttu-id="62dc8-134">`class` 制約が null ではありません。</span><span class="sxs-lookup"><span data-stu-id="62dc8-134">The `class` constraint is non-null.</span></span> <span data-ttu-id="62dc8-135">`class?` が "nullable reference type" を示す有効な null 許容制約であるかどうかを検討できます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-135">We can consider whether `class?` should be a valid nullable constraint denoting "nullable reference type".</span></span>

## <a name="type-inference"></a><span data-ttu-id="62dc8-136">型の推論</span><span class="sxs-lookup"><span data-stu-id="62dc8-136">Type inference</span></span>

<span data-ttu-id="62dc8-137">型推論では、貢献する型が null 許容の参照型である場合、結果の型は nullable である必要があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-137">In type inference, if a contributing type is a nullable reference type, the resulting type should be nullable.</span></span> <span data-ttu-id="62dc8-138">言い換えると、null 性伝達されます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-138">In other words, nullness is propagated.</span></span>

<span data-ttu-id="62dc8-139">`null` リテラルを参加式として含めるかどうかを検討する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-139">We should consider whether the `null` literal as a participating expression should contribute nullness.</span></span> <span data-ttu-id="62dc8-140">現在のところ、値型の場合はエラーが発生しますが、参照型の場合は null がプレーン型に正常に変換されます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-140">It doesn't today: for value types it leads to an error, whereas for reference types the null successfully converts to the plain type.</span></span> 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a><span data-ttu-id="62dc8-141">重大な変更</span><span class="sxs-lookup"><span data-stu-id="62dc8-141">Breaking changes</span></span>

<span data-ttu-id="62dc8-142">Null 以外の警告は、既存のコードでは明らかに互換性に影響する変更であり、オプトインメカニズムを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-142">Non-null warnings are an obvious breaking change on existing code, and should be accompanied with an opt-in mechanism.</span></span>

<span data-ttu-id="62dc8-143">当然ながら、null 許容型からの警告 (前述のように) は、null 値の許容属性が暗黙的である特定のシナリオでは、既存のコードの互換性に影響する変更です。</span><span class="sxs-lookup"><span data-stu-id="62dc8-143">Less obviously, warnings from nullable types (as described above) are a breaking change on existing code in certain scenarios where the nullability is implicit:</span></span>

* <span data-ttu-id="62dc8-144">制約のない型パラメーターは暗黙的に null 値が許容されるものとして扱われるので、`object` に割り当てたり、`ToString` にアクセスしたりすると、警告が生成されます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-144">Unconstrained type parameters will be treated as implicitly nullable, so assigning them to `object` or accessing e.g. `ToString` will yield warnings.</span></span>
* <span data-ttu-id="62dc8-145">型の推定によって `null` 式から null 値が推測される場合、既存のコードで null 非許容型ではなく null 許容型が生成され、新しい警告が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-145">if type inference infers nullness from `null` expressions, then existing code will sometimes yield nullable rather than non-nullable types, which can lead to new warnings.</span></span>

<span data-ttu-id="62dc8-146">Null 許容の警告も省略可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-146">So nullable warnings also need to be optional</span></span>

<span data-ttu-id="62dc8-147">最後に、既存の API に注釈を追加すると、警告が表示されたユーザーがライブラリをアップグレードしたときに重大な変更が発生します。</span><span class="sxs-lookup"><span data-stu-id="62dc8-147">Finally, adding annotations to an existing API will be a breaking change to users who have opted in to warnings, when they upgrade the library.</span></span> <span data-ttu-id="62dc8-148">これは、オプトインまたはオプトアウトする機能にもメリットがあります。「バグを修正する必要がありますが、新しい注釈を処理する準備ができていません」</span><span class="sxs-lookup"><span data-stu-id="62dc8-148">This, too, merits the ability to opt in or out. "I want the bug fixes, but I am not ready to deal with their new annotations"</span></span>

<span data-ttu-id="62dc8-149">要約すると、以下をオプトインできる必要があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-149">In summary, you need to be able to opt in/out of:</span></span>
* <span data-ttu-id="62dc8-150">Null 値を許容する警告</span><span class="sxs-lookup"><span data-stu-id="62dc8-150">Nullable warnings</span></span>
* <span data-ttu-id="62dc8-151">Null 以外の警告</span><span class="sxs-lookup"><span data-stu-id="62dc8-151">Non-null warnings</span></span>
* <span data-ttu-id="62dc8-152">他のファイルの注釈からの警告</span><span class="sxs-lookup"><span data-stu-id="62dc8-152">Warnings from annotations in other files</span></span>

<span data-ttu-id="62dc8-153">オプトインの粒度により、アナライザーに似たモデルが提案されます。ここでは、コードの膨大がプラグマと重大度レベルを選択し、ユーザーが選択できます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-153">The granularity of the opt-in suggests an analyzer-like model, where swaths of code can opt in and out with pragmas and severity levels can be chosen by the user.</span></span> <span data-ttu-id="62dc8-154">さらに、ライブラリごとのオプション (「JSON.NET の注釈を無視する準備ができないようにする」) は、コード内で属性として表現される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-154">Additionally, per-library options ("ignore the annotations from JSON.NET until I'm ready to deal with the fall out") may be expressible in code as attributes.</span></span>

<span data-ttu-id="62dc8-155">オプトイン/移行エクスペリエンスの設計は、この機能の成功と有用性に不可欠です。</span><span class="sxs-lookup"><span data-stu-id="62dc8-155">The design of the opt-in/transition experience is crucial to the success and usefulness of this feature.</span></span> <span data-ttu-id="62dc8-156">次のことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-156">We need to make sure that:</span></span>

* <span data-ttu-id="62dc8-157">ユーザーは、必要に応じて、null 値を許容するかどうかを段階的に確認できます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-157">Users can adopt nullability checking gradually as they want to</span></span>
* <span data-ttu-id="62dc8-158">ライブラリ作成者は、顧客の侵入を心配することなく、null 値を許容する注釈を追加できます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-158">Library authors can add nullability annotations without fear of breaking customers</span></span>
* <span data-ttu-id="62dc8-159">このような場合でも、"構成の悪夢" は意味がありません。</span><span class="sxs-lookup"><span data-stu-id="62dc8-159">Despite these, there is not a sense of "configuration nightmare"</span></span>

## <a name="tweaks"></a><span data-ttu-id="62dc8-160">改変</span><span class="sxs-lookup"><span data-stu-id="62dc8-160">Tweaks</span></span>

<span data-ttu-id="62dc8-161">ローカルでは `?` 注釈を使用しないことを検討できますが、割り当てられている内容に従って使用されているかどうかを観察するだけです。</span><span class="sxs-lookup"><span data-stu-id="62dc8-161">We could consider not using the `?` annotations on locals, but just observing whether they are used in accordance with what gets assigned to them.</span></span> <span data-ttu-id="62dc8-162">これを優先しません。私たちは、その意図を明確に表現する必要があると考えています。</span><span class="sxs-lookup"><span data-stu-id="62dc8-162">I don't favor this; I think we should uniformly let people express their intent.</span></span>

<span data-ttu-id="62dc8-163">パラメーターの短縮形 `T! x` を検討し、ランタイムの null チェックを自動生成します。</span><span class="sxs-lookup"><span data-stu-id="62dc8-163">We could consider a shorthand `T! x` on parameters, that auto-generates a runtime null check.</span></span>

<span data-ttu-id="62dc8-164">`FirstOrDefault` や `TryGet`など、ジェネリック型の特定のパターンでは、特定の状況で既定値が明示的に生成されるため、null 非許容型引数と若干奇妙な動作があります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-164">Certain patterns on generic types, such as `FirstOrDefault` or `TryGet`, have slightly weird behavior with non-nullable type arguments, because they explicitly yield default values in certain situations.</span></span> <span data-ttu-id="62dc8-165">これらの改善に対応するために、型システムの機能を強化することができます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-165">We could try to nuance the type system to accommodate these better.</span></span> <span data-ttu-id="62dc8-166">たとえば、型引数が既に null 値を許容できる場合でも、制約のない型パラメーターに対して `?` を許可することができます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-166">For instance, we could allow `?` on unconstrained type parameters, even though the type argument could already be nullable.</span></span> <span data-ttu-id="62dc8-167">これは価値があると思います。 null 許容*値*型との対話に関連する weirdness につながります。</span><span class="sxs-lookup"><span data-stu-id="62dc8-167">I doubt that it is worth it, and it leads to weirdness related to interaction with nullable *value* types.</span></span> 

## <a name="nullable-value-types"></a><span data-ttu-id="62dc8-168">null 許容値型</span><span class="sxs-lookup"><span data-stu-id="62dc8-168">Nullable value types</span></span>

<span data-ttu-id="62dc8-169">Null 許容値型に対しても上記のセマンティクスの一部を採用することを検討できます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-169">We could consider adopting some of the above semantics for nullable value types as well.</span></span>

<span data-ttu-id="62dc8-170">型の推定については既に説明しました。ここでは、エラーを提供するだけでなく、`(7, null)`から `int?` を推測できます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-170">We already mentioned type inference, where we could infer `int?` from `(7, null)`, instead of just giving an error.</span></span>

<span data-ttu-id="62dc8-171">また、フロー分析を null 許容の値型に適用することもできます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-171">Another opportunity is to apply the flow analysis to nullable value types.</span></span> <span data-ttu-id="62dc8-172">Null 以外と見なされる場合は、特定の方法 (メンバーアクセスなど) で null 非許容型としてを使用できるようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="62dc8-172">When they are deemed non-null, we could actually allow using as the non-nullable type in certain ways (e.g. member access).</span></span> <span data-ttu-id="62dc8-173">これは、戻り値の型に対して*既に*実行できるものが、バック互換性の理由で優先されることを気にする必要があるだけです。</span><span class="sxs-lookup"><span data-stu-id="62dc8-173">We just have to be careful that the things that you can *already* do on a nullable value type will be preferred, for back compat reasons.</span></span>
