---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "79484022"
---
# <a name="readonly-references"></a><span data-ttu-id="22f02-101">読み取り専用の参照</span><span class="sxs-lookup"><span data-stu-id="22f02-101">Readonly references</span></span>

* <span data-ttu-id="22f02-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="22f02-102">[x] Proposed</span></span>
* <span data-ttu-id="22f02-103">[x] プロトタイプ</span><span class="sxs-lookup"><span data-stu-id="22f02-103">[x] Prototype</span></span>
* <span data-ttu-id="22f02-104">[x] 実装: 開始しました</span><span class="sxs-lookup"><span data-stu-id="22f02-104">[x] Implementation: Started</span></span>
* <span data-ttu-id="22f02-105">[] 仕様: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="22f02-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="22f02-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="22f02-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="22f02-107">"読み取り専用の参照" 機能は実際には、参照によって変数を渡す効率性を活用し、変更するデータを公開しない機能のグループです。</span><span class="sxs-lookup"><span data-stu-id="22f02-107">The "readonly references" feature is actually a group of features that leverage the efficiency of passing variables by reference, but without exposing the data to modifications:</span></span>  
- <span data-ttu-id="22f02-108">`in` パラメーター</span><span class="sxs-lookup"><span data-stu-id="22f02-108">`in` parameters</span></span>
- <span data-ttu-id="22f02-109">`ref readonly` 戻り値</span><span class="sxs-lookup"><span data-stu-id="22f02-109">`ref readonly` returns</span></span>
- <span data-ttu-id="22f02-110">`readonly` 構造体</span><span class="sxs-lookup"><span data-stu-id="22f02-110">`readonly` structs</span></span>
- <span data-ttu-id="22f02-111">`ref`/`in` 拡張メソッド</span><span class="sxs-lookup"><span data-stu-id="22f02-111">`ref`/`in` extension methods</span></span>
- <span data-ttu-id="22f02-112">`ref readonly` ローカル</span><span class="sxs-lookup"><span data-stu-id="22f02-112">`ref readonly` locals</span></span>
- <span data-ttu-id="22f02-113">`ref` 条件式</span><span class="sxs-lookup"><span data-stu-id="22f02-113">`ref` conditional expressions</span></span>

## <a name="passing-arguments-as-readonly-references"></a><span data-ttu-id="22f02-114">読み取り専用参照として引数を渡す。</span><span class="sxs-lookup"><span data-stu-id="22f02-114">Passing arguments as readonly references.</span></span>

<span data-ttu-id="22f02-115">このトピックに触れている既存の提案は、特に多くの詳細を説明することなく、読み取り専用パラメーターの特殊なケースとして https://github.com/dotnet/roslyn/issues/115 ます。</span><span class="sxs-lookup"><span data-stu-id="22f02-115">There is an existing proposal that touches this topic https://github.com/dotnet/roslyn/issues/115 as a special case of readonly parameters without going into many details.</span></span>
<span data-ttu-id="22f02-116">ここでは、それ自体がまったく新しいものではないということを認識したいだけです。</span><span class="sxs-lookup"><span data-stu-id="22f02-116">Here I just want to acknowledge that the idea by itself is not very new.</span></span>

### <a name="motivation"></a><span data-ttu-id="22f02-117">目的</span><span class="sxs-lookup"><span data-stu-id="22f02-117">Motivation</span></span>

<span data-ttu-id="22f02-118">この機能C#の前には、構造体変数をメソッド呼び出しに渡すことを目的とした効率的な方法がありませんでした。変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="22f02-118">Prior to this feature C# did not have an efficient way of expressing a desire to pass struct variables into method calls for readonly purposes with no intention of modifying.</span></span> <span data-ttu-id="22f02-119">標準の値渡し引数は、不要なコストを追加するコピーを意味します。</span><span class="sxs-lookup"><span data-stu-id="22f02-119">Regular by-value argument passing implies copying, which adds unnecessary costs.</span></span>  <span data-ttu-id="22f02-120">を使用すると、ユーザーは-ref 引数を渡し、コメント/ドキュメントに依存して、呼び出し先によるデータの変換が想定されていないことを示すことができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-120">That drives users to use by-ref argument passing and rely on comments/documentation to indicate that the data is not supposed to be mutated by the callee.</span></span> <span data-ttu-id="22f02-121">多くの理由により、これは適切な解決策ではありません。</span><span class="sxs-lookup"><span data-stu-id="22f02-121">It is not a good solution for many reasons.</span></span>  
<span data-ttu-id="22f02-122">例としては、パフォーマンスに関する考慮事項のために、 [XNA](https://msdn.microsoft.com/library/bb194944.aspx)のように、単純に ref オペランドがあることがわかります。</span><span class="sxs-lookup"><span data-stu-id="22f02-122">The examples are numerous - vector/matrix math operators in graphics libraries like [XNA](https://msdn.microsoft.com/library/bb194944.aspx) are known to have ref operands purely because of performance considerations.</span></span> <span data-ttu-id="22f02-123">Roslyn コンパイラ自体には、割り当てを回避するために構造体を使用するコードがあり、その後、コストのコピーを回避するために参照によって渡されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-123">There is code in Roslyn compiler itself that uses structs to avoid allocations and then passes them by reference to avoid copying costs.</span></span>

### <a name="solution-in-parameters"></a><span data-ttu-id="22f02-124">ソリューション (`in` パラメーター)</span><span class="sxs-lookup"><span data-stu-id="22f02-124">Solution (`in` parameters)</span></span>

<span data-ttu-id="22f02-125">`out` パラメーターと同様に、`in` パラメーターは、呼び出し先からの追加の保証を持つマネージ参照として渡されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-125">Similarly to the `out` parameters, `in` parameters are passed as managed references with additional guarantees from the callee.</span></span>  
<span data-ttu-id="22f02-126">他の用途の前に呼び出し先によって割り当てられる_必要が_ある `out` パラメーターとは異なり、`in` パラメーターを呼び出し先で割り当てることはできません。</span><span class="sxs-lookup"><span data-stu-id="22f02-126">Unlike `out` parameters which _must_ be assigned by the callee before any other use, `in` parameters cannot be assigned by the callee at all.</span></span>

<span data-ttu-id="22f02-127">結果として `in` パラメーターを使用して、呼び出し先によって変更に引数を公開せずに間接的な引数を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-127">As a result `in` parameters allow for effectiveness of indirect argument passing without exposing arguments to mutations by the callee.</span></span>

### <a name="declaring-in-parameters"></a><span data-ttu-id="22f02-128">`in` パラメーターの宣言</span><span class="sxs-lookup"><span data-stu-id="22f02-128">Declaring `in` parameters</span></span>

<span data-ttu-id="22f02-129">`in` パラメーターは、パラメーターシグネチャの修飾子として `in` キーワードを使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-129">`in` parameters are declared by using `in` keyword as a modifier in the parameter signature.</span></span>

<span data-ttu-id="22f02-130">すべての目的で、`in` パラメーターは `readonly` 変数として扱われます。</span><span class="sxs-lookup"><span data-stu-id="22f02-130">For all purposes the `in` parameter is treated as a `readonly` variable.</span></span> <span data-ttu-id="22f02-131">メソッド内での `in` パラメーターの使用に関する制限のほとんどは、`readonly` のフィールドと同じです。</span><span class="sxs-lookup"><span data-stu-id="22f02-131">Most of the restrictions on the use of `in` parameters inside the method are the same as with `readonly` fields.</span></span>

> <span data-ttu-id="22f02-132">実際、`in` パラメーターは `readonly` フィールドを表す場合があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-132">Indeed an `in` parameter may represent a `readonly` field.</span></span> <span data-ttu-id="22f02-133">制限の類似性は偶然ではありません。</span><span class="sxs-lookup"><span data-stu-id="22f02-133">Similarity of restrictions is not a coincidence.</span></span>

<span data-ttu-id="22f02-134">たとえば、構造体型を持つ `in` パラメーターのフィールドは、すべて `readonly` 変数として再帰的に分類されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-134">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- <span data-ttu-id="22f02-135">`in` パラメーターは、通常の byval パラメーターが許可されている任意の場所で使用できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-135">`in` parameters are allowed anywhere where ordinary byval parameters are allowed.</span></span> <span data-ttu-id="22f02-136">これには、インデクサー、演算子 (変換を含む)、デリゲート、ラムダ、ローカル関数が含まれます。</span><span class="sxs-lookup"><span data-stu-id="22f02-136">This includes indexers, operators (including conversions), delegates, lambdas, local functions.</span></span>

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- <span data-ttu-id="22f02-137">`in` は `out` と組み合わせて使用することも、`out` をと組み合わせることもできません。</span><span class="sxs-lookup"><span data-stu-id="22f02-137">`in` is not allowed in combination with `out` or with anything that `out` does not combine with.</span></span>

- <span data-ttu-id="22f02-138">`ref`/`out`のオーバーロードは許可されていません。 `in` の相違点 /。</span><span class="sxs-lookup"><span data-stu-id="22f02-138">It is not permitted to overload on `ref`/`out`/`in` differences.</span></span>

- <span data-ttu-id="22f02-139">通常の byval でのオーバーロードと `in` の違いが許可されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-139">It is permitted to overload on ordinary byval and `in` differences.</span></span>

- <span data-ttu-id="22f02-140">OHI の目的 (オーバーロード、非表示、実装) では、`in` は `out` パラメーターと同様に動作します。</span><span class="sxs-lookup"><span data-stu-id="22f02-140">For the purpose of OHI (Overloading, Hiding, Implementing), `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="22f02-141">同じルールがすべて適用されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-141">All the same rules apply.</span></span>
<span data-ttu-id="22f02-142">たとえば、オーバーライドするメソッドは、`in` パラメーターと、id 変換可能な型の `in` パラメーターを一致させる必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-142">For example the overriding method will have to match `in` parameters with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="22f02-143">デリゲート/ラムダ/メソッドグループ変換のために、`in` は `out` パラメーターと同様に動作します。</span><span class="sxs-lookup"><span data-stu-id="22f02-143">For the purpose of delegate/lambda/method group conversions, `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="22f02-144">ラムダと適用可能なメソッドグループ変換の候補は、ターゲットデリゲートのパラメーターと、id 変換可能な型の `in` パラメーターを `in` 一致させる必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-144">Lambdas and applicable method group conversion candidates will have to match `in` parameters of the target delegate with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="22f02-145">一般的な分散のために、`in` パラメーターは非バリアントです。</span><span class="sxs-lookup"><span data-stu-id="22f02-145">For the purpose of generic variance, `in` parameters are nonvariant.</span></span>

> <span data-ttu-id="22f02-146">注: 参照型またはプリミティブ型を持つパラメーター `in` には警告がありません。</span><span class="sxs-lookup"><span data-stu-id="22f02-146">NOTE: There are no warnings on `in` parameters that have reference or primitives types.</span></span>
<span data-ttu-id="22f02-147">一般には意味がないかもしれませんが、場合によっては、ユーザーが `in`としてプリミティブを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-147">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="22f02-148">例-`T` が `int`に置き換えられたとき、またはのようなメソッドがあるときに、`Method(in T param)` などのジェネリックメソッドをオーバーライドすると、`Volatile.Read(in int location)`</span><span class="sxs-lookup"><span data-stu-id="22f02-148">Examples - overriding a generic method like `Method(in T param)` when `T` was substituted to be `int`, or when having methods like `Volatile.Read(in int location)`</span></span>
>
> <span data-ttu-id="22f02-149">`in` パラメーターが非効率的に使用された場合に警告するアナライザーがあると想定されていますが、このような分析のルールは、言語仕様の一部になるにはあいまいすぎます。</span><span class="sxs-lookup"><span data-stu-id="22f02-149">It is conceivable to have an analyzer that warns in cases of inefficient use of `in` parameters, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="use-of-in-at-call-sites-in-arguments"></a><span data-ttu-id="22f02-150">呼び出しサイトでの `in` の使用。</span><span class="sxs-lookup"><span data-stu-id="22f02-150">Use of `in` at call sites.</span></span> <span data-ttu-id="22f02-151">(`in` の引数)</span><span class="sxs-lookup"><span data-stu-id="22f02-151">(`in` arguments)</span></span>

<span data-ttu-id="22f02-152">引数を `in` パラメーターに渡すには、2つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-152">There are two ways to pass arguments to `in` parameters.</span></span>

#### <a name="in-arguments-can-match-in-parameters"></a><span data-ttu-id="22f02-153">`in` の引数は `in` パラメーターと一致できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-153">`in` arguments can match `in` parameters:</span></span>

<span data-ttu-id="22f02-154">呼び出しサイトで `in` 修飾子を持つ引数は、`in` パラメーターと一致できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-154">An argument with an `in` modifier at the call site can match `in` parameters.</span></span>

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- <span data-ttu-id="22f02-155">`in` 引数は_読み取り可能_な左辺値 (\*) である必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-155">`in` argument must be a _readable_ LValue(\*).</span></span>
<span data-ttu-id="22f02-156">例: `M1(in 42)` が無効です</span><span class="sxs-lookup"><span data-stu-id="22f02-156">Example: `M1(in 42)` is invalid</span></span>

> <span data-ttu-id="22f02-157">(\*)[左辺値と右辺](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue)値の概念は、言語によって異なります。</span><span class="sxs-lookup"><span data-stu-id="22f02-157">(\*) The notion of [LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) vary between languages.</span></span>  
<span data-ttu-id="22f02-158">ここで、左辺値は、直接参照できる場所を表す式を意味します。</span><span class="sxs-lookup"><span data-stu-id="22f02-158">Here, by LValue I mean an expression that represent a location that can be referred to directly.</span></span>
<span data-ttu-id="22f02-159">右辺値は、それ自体には保持されない一時的な結果を生成する式を意味します。</span><span class="sxs-lookup"><span data-stu-id="22f02-159">And RValue means an expression that yields a temporary result which does not persist on its own.</span></span>  

- <span data-ttu-id="22f02-160">具体的には、`readonly` フィールド、`in` パラメーター、またはその他の正式 `readonly` 変数を `in` 引数として渡すことが有効です。</span><span class="sxs-lookup"><span data-stu-id="22f02-160">In particular it is valid to pass `readonly` fields, `in` parameters or other formally `readonly` variables as `in` arguments.</span></span>
<span data-ttu-id="22f02-161">例: `dictionary[in Guid.Empty]` は有効です。</span><span class="sxs-lookup"><span data-stu-id="22f02-161">Example: `dictionary[in Guid.Empty]` is legal.</span></span> <span data-ttu-id="22f02-162">`Guid.Empty` は、静的な読み取り専用フィールドです。</span><span class="sxs-lookup"><span data-stu-id="22f02-162">`Guid.Empty` is a static readonly field.</span></span>

- <span data-ttu-id="22f02-163">`in` 引数には、パラメーターの型に_変換_可能な型を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-163">`in` argument must have type _identity-convertible_ to the type of the parameter.</span></span>
<span data-ttu-id="22f02-164">例: `M1<object>(in Guid.Empty)` が無効です。</span><span class="sxs-lookup"><span data-stu-id="22f02-164">Example: `M1<object>(in Guid.Empty)` is invalid.</span></span> <span data-ttu-id="22f02-165">`Guid.Empty` は、 _id に変換_できません `object`</span><span class="sxs-lookup"><span data-stu-id="22f02-165">`Guid.Empty` is not _identity-convertible_ to `object`</span></span>

<span data-ttu-id="22f02-166">上記の規則の目的は、`in` 引数が引数の変数の_別名_を保証することです。</span><span class="sxs-lookup"><span data-stu-id="22f02-166">The motivation for the above rules is that `in` arguments guarantee _aliasing_ of the argument variable.</span></span> <span data-ttu-id="22f02-167">呼び出し先は、常に、引数によって表される同じ場所への直接参照を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="22f02-167">The callee always receives a direct reference to the same location as represented by the argument.</span></span>

- <span data-ttu-id="22f02-168">まれに、同じ呼び出しのオペランドとして使用される `await` 式によって `in` の引数がスタックに含まれる必要がある場合は、`out` 引数と `ref` 引数と同じ動作が使用されます。この変数には、透過的な方法で書き込まれない場合、エラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-168">in rare situations when `in` arguments must be stack-spilled due to `await` expressions used as operands of the same call, the behavior is the same as with `out` and `ref` arguments - if the variable cannot be spilled in referentially-transparent manner, an error is reported.</span></span>

<span data-ttu-id="22f02-169">例 :</span><span class="sxs-lookup"><span data-stu-id="22f02-169">Examples:</span></span>
1. <span data-ttu-id="22f02-170">`M1(in staticField, await SomethingAsync())` が有効です。</span><span class="sxs-lookup"><span data-stu-id="22f02-170">`M1(in staticField, await SomethingAsync())`  is valid.</span></span>
<span data-ttu-id="22f02-171">`staticField` は、観測可能な副作用なしで複数回アクセスできる静的フィールドです。</span><span class="sxs-lookup"><span data-stu-id="22f02-171">`staticField` is a static field which can be accessed more than once without observable side effects.</span></span> <span data-ttu-id="22f02-172">したがって、副作用の順序とエイリアスの要件の両方を指定できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-172">Therefore both the order of side effects and aliasing requirements can be provided.</span></span>
2. <span data-ttu-id="22f02-173">`M1(in RefReturningMethod(), await SomethingAsync())` によってエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-173">`M1(in RefReturningMethod(), await SomethingAsync())`  will produce an error.</span></span>
<span data-ttu-id="22f02-174">`RefReturningMethod()` は、メソッドを返す `ref` です。</span><span class="sxs-lookup"><span data-stu-id="22f02-174">`RefReturningMethod()` is a `ref` returning method.</span></span> <span data-ttu-id="22f02-175">メソッド呼び出しには観測可能な副作用がある可能性があるため、`SomethingAsync()` オペランドの前に評価する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-175">A method call may have observable side effects, therefore it must be evaluated before the `SomethingAsync()` operand.</span></span> <span data-ttu-id="22f02-176">ただし、呼び出しの結果は、直接的な参照を必要としない `await` 中断ポイントを越えて保持できない参照になります。</span><span class="sxs-lookup"><span data-stu-id="22f02-176">However the result of the invocation is a reference that cannot be preserved across the `await` suspension point which make the direct reference requirement impossible.</span></span>

> <span data-ttu-id="22f02-177">注: スタックの書き込むエラーは、実装固有の制限事項と見なされます。</span><span class="sxs-lookup"><span data-stu-id="22f02-177">NOTE: the stack spilling errors are considered to be implementation-specific limitations.</span></span> <span data-ttu-id="22f02-178">したがって、オーバーロードの解決やラムダの推論には影響しません。</span><span class="sxs-lookup"><span data-stu-id="22f02-178">Therefore they do not have effect on overload resolution or lambda inference.</span></span>

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a><span data-ttu-id="22f02-179">通常の byval 引数は `in` パラメーターと一致できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-179">Ordinary byval arguments can match `in` parameters:</span></span>

<span data-ttu-id="22f02-180">修飾子のない通常の引数は `in` パラメーターと一致させることができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-180">Regular arguments without modifiers can match `in` parameters.</span></span> <span data-ttu-id="22f02-181">このような場合、引数の緩やかな制約は、通常の byval 引数と同じになります。</span><span class="sxs-lookup"><span data-stu-id="22f02-181">In such case the arguments have the same relaxed constraints as an ordinary byval arguments would have.</span></span>

<span data-ttu-id="22f02-182">このシナリオの目的は、Api のパラメーターを `in`、直接参照として引数を渡すことができない場合 (例: リテラル、計算結果、`await`の結果、またはより具体的な型を持つ必要がある引数) に、ユーザーが防ぎになる可能性があることです。</span><span class="sxs-lookup"><span data-stu-id="22f02-182">The motivation for this scenario is that `in` parameters in APIs may result in inconveniences for the user when arguments cannot be passed as a direct reference - ex: literals, computed or `await`-ed results or arguments that happen to have more specific types.</span></span>  
<span data-ttu-id="22f02-183">これらのすべてのケースには、引数の値を適切な型の一時的なローカルに格納し、そのローカルを `in` 引数として渡すという、簡単なソリューションがあります。</span><span class="sxs-lookup"><span data-stu-id="22f02-183">All these cases have a trivial solution of storing the argument value in a temporary local of appropriate type and passing that local as an `in` argument.</span></span>  
<span data-ttu-id="22f02-184">このような定型コードコンパイラの必要性を軽減するには、必要に応じて、呼び出しサイトに `in` 修飾子が存在しないときに、同じ変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="22f02-184">To reduce the need for such boilerplate code compiler can perform the same transformation, if needed, when `in` modifier is not present at the call site.</span></span>  

<span data-ttu-id="22f02-185">また、演算子の呼び出しや拡張メソッドの `in` など、場合によっては、`in` を指定する構文的な方法はありません。</span><span class="sxs-lookup"><span data-stu-id="22f02-185">In addition, in some cases, such as invocation of operators, or `in` extension methods, there is no syntactical way to specify `in` at all.</span></span> <span data-ttu-id="22f02-186">それだけでは、`in` パラメーターに一致するときに通常の byval 引数の動作を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-186">That alone requires specifying the behavior of ordinary byval arguments when they match `in` parameters.</span></span>

<span data-ttu-id="22f02-187">特に次の点に違いがあります。</span><span class="sxs-lookup"><span data-stu-id="22f02-187">In particular:</span></span>

- <span data-ttu-id="22f02-188">右辺値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-188">it is valid to pass RValues.</span></span>
<span data-ttu-id="22f02-189">このような場合は、一時への参照が渡されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-189">A reference to a temporary is passed in such case.</span></span>
<span data-ttu-id="22f02-190">例:</span><span class="sxs-lookup"><span data-stu-id="22f02-190">Example:</span></span>
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- <span data-ttu-id="22f02-191">暗黙的な変換は許可されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-191">implicit conversions are allowed.</span></span>

> <span data-ttu-id="22f02-192">これは、実際には右辺値を渡す特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="22f02-192">This is actually a special case of passing an RValue</span></span>  

<span data-ttu-id="22f02-193">このような場合は、変換された一時的な値への参照が渡されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-193">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="22f02-194">例:</span><span class="sxs-lookup"><span data-stu-id="22f02-194">Example:</span></span>
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- <span data-ttu-id="22f02-195">`in` 拡張メソッドの受信者の場合 (`ref` 拡張メソッドではなく)、右辺値または暗黙的な_引数変換_が許可されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-195">in a case of a receiver of an `in` extension method (as opposed to `ref` extension methods), RValues or implicit _this-argument-conversions_ are allowed.</span></span>
<span data-ttu-id="22f02-196">このような場合は、変換された一時的な値への参照が渡されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-196">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="22f02-197">例:</span><span class="sxs-lookup"><span data-stu-id="22f02-197">Example:</span></span>
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
<span data-ttu-id="22f02-198">`ref`/`in` 拡張メソッドの詳細については、こちらのドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="22f02-198">More information on `ref`/`in` extension methods is provided further in this document.</span></span>

- <span data-ttu-id="22f02-199">`await` オペランドによる引数の書き込むは、必要に応じて "値渡し" できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-199">argument spilling due to `await` operands could spill "by-value", if necessary.</span></span>
<span data-ttu-id="22f02-200">場合によっては、引数への直接参照を指定することはできないので、引数の値のコピーは代わりに使用され `await`。</span><span class="sxs-lookup"><span data-stu-id="22f02-200">In scenarios where providing a direct reference to the argument is not possible due to intervening `await` a copy of the argument's value is spilled instead.</span></span>  
<span data-ttu-id="22f02-201">例:</span><span class="sxs-lookup"><span data-stu-id="22f02-201">Example:</span></span>
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
<span data-ttu-id="22f02-202">副作用のある呼び出しの結果は、`await` の中断をまたいで保持できない参照なので、実際の値を含む一時は (通常の byval パラメーターの場合と同様に) 保持されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-202">Since the result of a side-effecting invocation is a reference that cannot be preserved across `await` suspension, a temporary containing the actual value will be preserved instead (as it would in an ordinary byval parameter case).</span></span>

#### <a name="omitted-optional-arguments"></a><span data-ttu-id="22f02-203">省略可能な引数を省略します。</span><span class="sxs-lookup"><span data-stu-id="22f02-203">Omitted optional arguments</span></span>

<span data-ttu-id="22f02-204">`in` パラメーターで既定値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-204">It is permitted for an `in` parameter to specify a default value.</span></span> <span data-ttu-id="22f02-205">これにより、対応する引数が省略可能になります。</span><span class="sxs-lookup"><span data-stu-id="22f02-205">That makes the corresponding argument optional.</span></span>

<span data-ttu-id="22f02-206">呼び出しサイトで省略可能な引数を省略すると、一時によって既定値が渡されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-206">Omitting optional argument at the call site results in passing the default value via a temporary.</span></span>

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a><span data-ttu-id="22f02-207">一般的なエイリアス動作</span><span class="sxs-lookup"><span data-stu-id="22f02-207">Aliasing behavior in general</span></span>

<span data-ttu-id="22f02-208">`ref` と `out` 変数と同じように、`in` 変数は既存の場所への参照または別名です。</span><span class="sxs-lookup"><span data-stu-id="22f02-208">Just like `ref` and `out` variables, `in` variables are references/aliases to existing locations.</span></span>

<span data-ttu-id="22f02-209">呼び出し先は、それらへの書き込みを許可されていませんが、`in` パラメーターを読み取ると、他の評価の副作用として異なる値が観察される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-209">While callee is not allowed to write into them, reading an `in` parameter can observe different values as a side effect of other evaluations.</span></span>

<span data-ttu-id="22f02-210">例:</span><span class="sxs-lookup"><span data-stu-id="22f02-210">Example:</span></span>

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a><span data-ttu-id="22f02-211">ローカル変数のパラメーターとキャプチャを `in` します。</span><span class="sxs-lookup"><span data-stu-id="22f02-211">`in` parameters and capturing of local variables.</span></span>  
<span data-ttu-id="22f02-212">ラムダ/非同期キャプチャのために `in` パラメーターは `out` および `ref` パラメーターと同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="22f02-212">For the purpose of lambda/async capturing `in` parameters behave the same as `out` and `ref` parameters.</span></span>

- <span data-ttu-id="22f02-213">クロージャで `in` パラメーターをキャプチャすることはできません</span><span class="sxs-lookup"><span data-stu-id="22f02-213">`in` parameters cannot be captured in a closure</span></span>
- <span data-ttu-id="22f02-214">`in` のパラメーターは反復子メソッドでは使用できません</span><span class="sxs-lookup"><span data-stu-id="22f02-214">`in` parameters are not allowed in iterator methods</span></span>
- <span data-ttu-id="22f02-215">`in` パラメーターは、非同期メソッドでは使用できません</span><span class="sxs-lookup"><span data-stu-id="22f02-215">`in` parameters are not allowed in async methods</span></span>

### <a name="temporary-variables"></a><span data-ttu-id="22f02-216">一時変数。</span><span class="sxs-lookup"><span data-stu-id="22f02-216">Temporary variables.</span></span>  
<span data-ttu-id="22f02-217">`in` パラメーター渡しを使用する場合は、一時的なローカル変数を間接的に使用することが必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-217">Some uses of `in` parameter passing may require indirect use of a temporary local variable:</span></span>  
- <span data-ttu-id="22f02-218">呼び出しサイトで `in`を使用する場合、`in` の引数は常に直接エイリアスとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-218">`in` arguments are always passed as direct aliases when call-site uses `in`.</span></span> <span data-ttu-id="22f02-219">このような場合、一時は使用されません。</span><span class="sxs-lookup"><span data-stu-id="22f02-219">Temporary is never used in such case.</span></span>
- <span data-ttu-id="22f02-220">呼び出しサイトで `in`が使用されていない場合、`in` の引数を直接エイリアスにする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="22f02-220">`in` arguments are not required to be direct aliases when call-site does not use `in`.</span></span> <span data-ttu-id="22f02-221">引数が左辺値でない場合は、一時的なを使用できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-221">When argument is not an LValue, a temporary may be used.</span></span>
- <span data-ttu-id="22f02-222">`in` パラメーターに既定値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-222">`in` parameter may have default value.</span></span> <span data-ttu-id="22f02-223">呼び出しサイトで対応する引数を省略すると、既定値は一時的に渡されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-223">When corresponding argument is omitted at the call site, the default value are passed via a temporary.</span></span>
- <span data-ttu-id="22f02-224">`in` の引数には、id を保持していないものも含め、暗黙的な変換を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-224">`in` arguments may have implicit conversions, including those that do not preserve identity.</span></span> <span data-ttu-id="22f02-225">そのような場合は、一時が使用されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-225">A temporary is used in those cases.</span></span>
- <span data-ttu-id="22f02-226">通常の構造体呼び出しのレシーバーは、書き込み可能な左辺値ではない場合があります (**既存の case!** )。</span><span class="sxs-lookup"><span data-stu-id="22f02-226">receivers of ordinary struct calls may not be writeable LValues (**existing case!**).</span></span> <span data-ttu-id="22f02-227">そのような場合は、一時が使用されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-227">A temporary is used in those cases.</span></span>

<span data-ttu-id="22f02-228">引数一時要素の有効期間は、呼び出しサイトの最も近いスコープに一致します。</span><span class="sxs-lookup"><span data-stu-id="22f02-228">The life time of the argument temporaries matches the closest encompassing scope of the call-site.</span></span>

<span data-ttu-id="22f02-229">一時変数の正式な有効期間は、参照によって返される変数のエスケープ分析に関連するシナリオで意味的に意味があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-229">The formal life time of temporary variables is semantically significant in scenarios involving escape analysis of variables returned by reference.</span></span>

### <a name="metadata-representation-of-in-parameters"></a><span data-ttu-id="22f02-230">`in` パラメーターのメタデータ表現。</span><span class="sxs-lookup"><span data-stu-id="22f02-230">Metadata representation of `in` parameters.</span></span>
<span data-ttu-id="22f02-231">Byref パラメーターに `System.Runtime.CompilerServices.IsReadOnlyAttribute` が適用されている場合は、パラメーターが `in` パラメーターであることを意味します。</span><span class="sxs-lookup"><span data-stu-id="22f02-231">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a byref parameter, it means that the parameter is an `in` parameter.</span></span>

<span data-ttu-id="22f02-232">また、メソッドが*abstract*または*virtual*の場合、このようなパラメーターのシグネチャ (およびそのようなパラメーターのみ) には `modreq[System.Runtime.InteropServices.InAttribute]`が必要です。</span><span class="sxs-lookup"><span data-stu-id="22f02-232">In addition, if the method is *abstract* or *virtual*, then the signature of such parameters (and only such parameters) must have `modreq[System.Runtime.InteropServices.InAttribute]`.</span></span>

<span data-ttu-id="22f02-233">**動機**: `in` パラメーターをオーバーライドまたは実装するメソッドが一致する場合に、この処理が行われます。</span><span class="sxs-lookup"><span data-stu-id="22f02-233">**Motivation**: this is done to ensure that in a case of method overriding/implementing the `in` parameters match.</span></span>

<span data-ttu-id="22f02-234">デリゲートの `Invoke` メソッドにも同じ要件が適用されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-234">Same requirements apply to `Invoke` methods in delegates.</span></span>

<span data-ttu-id="22f02-235">**動機**: デリゲートの作成時または割り当て時に既存のコンパイラが単に `readonly` を無視できないようにします。</span><span class="sxs-lookup"><span data-stu-id="22f02-235">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when creating or assigning delegates.</span></span>

## <a name="returning-by-readonly-reference"></a><span data-ttu-id="22f02-236">読み取り専用の参照によって返されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-236">Returning by readonly reference.</span></span>

### <a name="motivation"></a><span data-ttu-id="22f02-237">目的</span><span class="sxs-lookup"><span data-stu-id="22f02-237">Motivation</span></span>
<span data-ttu-id="22f02-238">このサブ機能の目的は、`in` パラメーターの理由 (コピーの回避、戻り側での処理) にほぼ対称です。</span><span class="sxs-lookup"><span data-stu-id="22f02-238">The motivation for this sub-feature is roughly symmetrical to the reasons for the `in` parameters - avoiding copying, but on the returning side.</span></span> <span data-ttu-id="22f02-239">この機能を使用する前に、メソッドまたはインデクサーに2つのオプションがありました。 1) 参照によって返され、変更または2に公開されることがありますが、値によって返されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-239">Prior to this feature, a method or an indexer had two options: 1) return by reference and be exposed to possible mutations or 2) return by value which results in copying.</span></span>

### <a name="solution-ref-readonly-returns"></a><span data-ttu-id="22f02-240">ソリューション (`ref readonly` が返す)</span><span class="sxs-lookup"><span data-stu-id="22f02-240">Solution (`ref readonly` returns)</span></span>  
<span data-ttu-id="22f02-241">この機能により、メンバーは変数を変更に公開せずに参照渡しで返すことができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-241">The feature allows a member to return variables by reference without exposing them to mutations.</span></span>

### <a name="declaring-ref-readonly-returning-members"></a><span data-ttu-id="22f02-242">メンバーを返す `ref readonly` の宣言</span><span class="sxs-lookup"><span data-stu-id="22f02-242">Declaring `ref readonly` returning members</span></span>

<span data-ttu-id="22f02-243">戻り値のシグネチャに `ref readonly` 修飾子の組み合わせは、メンバーが読み取り専用の参照を返すことを示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-243">A combination of modifiers `ref readonly` on the return signature is used to to indicate that the member returns a readonly reference.</span></span>

<span data-ttu-id="22f02-244">すべての目的において、`ref readonly` メンバーは、`readonly` フィールドや `in` パラメーターと同様に `readonly` 変数として扱われます。</span><span class="sxs-lookup"><span data-stu-id="22f02-244">For all purposes a `ref readonly` member is treated as a `readonly` variable - similar to `readonly` fields and `in` parameters.</span></span>

<span data-ttu-id="22f02-245">たとえば、構造体型を持つ `ref readonly` メンバーのフィールドはすべて、`readonly` 変数として再帰的に分類されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-245">For example fields of `ref readonly` member which has a struct type are all recursively classified as `readonly` variables.</span></span> <span data-ttu-id="22f02-246">-`in` 引数として渡すことはできますが、`ref` または `out` 引数として渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="22f02-246">- It is permitted to pass them as `in` arguments, but not as `ref` or `out` arguments.</span></span>

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- <span data-ttu-id="22f02-247">`ref readonly` の戻り値は、同じ場所で許可されていますが `ref` 返されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-247">`ref readonly` returns are allowed in the same places were `ref` returns are allowed.</span></span>
<span data-ttu-id="22f02-248">これには、インデクサー、デリゲート、ラムダ、ローカル関数が含まれます。</span><span class="sxs-lookup"><span data-stu-id="22f02-248">This includes indexers, delegates, lambdas, local functions.</span></span>

- <span data-ttu-id="22f02-249">`ref`/`ref readonly`/相違点でのオーバーロードは許可されていません。</span><span class="sxs-lookup"><span data-stu-id="22f02-249">It is not permitted to overload on `ref`/`ref readonly` /  differences.</span></span>

- <span data-ttu-id="22f02-250">通常の byval でのオーバーロードが許可され、`ref readonly` 戻り値の違いがあります。</span><span class="sxs-lookup"><span data-stu-id="22f02-250">It is permitted to overload on ordinary byval and `ref readonly` return differences.</span></span>

- <span data-ttu-id="22f02-251">OHI (オーバーロード、非表示、実装) のために `ref readonly` は似ていますが `ref`とは異なります。</span><span class="sxs-lookup"><span data-stu-id="22f02-251">For the purpose of OHI (Overloading, Hiding, Implementing), `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="22f02-252">たとえば、`ref readonly` 1 をオーバーライドするメソッドは、それ自体が `ref readonly` で、id 変換可能な型を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-252">For example the a method that overrides `ref readonly` one, must itself be `ref readonly` and have identity-convertible type.</span></span>

- <span data-ttu-id="22f02-253">デリゲート/ラムダ/メソッドグループ変換のために `ref readonly` は似ていますが `ref`とは異なります。</span><span class="sxs-lookup"><span data-stu-id="22f02-253">For the purpose of delegate/lambda/method group conversions, `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="22f02-254">ラムダおよび適用可能なメソッドグループ変換の候補は、id 変換可能な型の `ref readonly` 戻り値を使用して、ターゲットデリゲートの `ref readonly` 返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-254">Lambdas and applicable method group conversion candidates have to match `ref readonly` return of the target delegate with `ref readonly` return of the type that is identity-convertible.</span></span>

- <span data-ttu-id="22f02-255">一般的な分散のために、`ref readonly` 戻り値は非バリアントです。</span><span class="sxs-lookup"><span data-stu-id="22f02-255">For the purpose of generic variance, `ref readonly` returns are nonvariant.</span></span>

> <span data-ttu-id="22f02-256">注: 参照型またはプリミティブ型を持つ `ref readonly` が返された場合、警告は発生しません。</span><span class="sxs-lookup"><span data-stu-id="22f02-256">NOTE: There are no warnings on `ref readonly` returns that have reference or primitives types.</span></span>
<span data-ttu-id="22f02-257">一般には意味がないかもしれませんが、場合によっては、ユーザーが `in`としてプリミティブを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-257">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="22f02-258">例-`T` が置換されたときに、`ref readonly T Method()` などのジェネリックメソッドをオーバーライドして `int`します。</span><span class="sxs-lookup"><span data-stu-id="22f02-258">Examples - overriding a generic method like `ref readonly T Method()` when `T` was substituted to be `int`.</span></span>
>
><span data-ttu-id="22f02-259">`ref readonly` の戻り値が非効率的に使用された場合に警告するアナライザーがあると考えられますが、このような分析のルールは、言語仕様の一部になるにはあいまいすぎます。</span><span class="sxs-lookup"><span data-stu-id="22f02-259">It is conceivable to have an analyzer that warns in cases of inefficient use of `ref readonly` returns, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="returning-from-ref-readonly-members"></a><span data-ttu-id="22f02-260">`ref readonly` メンバーからの戻り</span><span class="sxs-lookup"><span data-stu-id="22f02-260">Returning from `ref readonly` members</span></span>
<span data-ttu-id="22f02-261">メソッド本体内では、構文は通常の ref 戻り値と同じです。</span><span class="sxs-lookup"><span data-stu-id="22f02-261">Inside the method body the syntax is the same as with regular ref returns.</span></span> <span data-ttu-id="22f02-262">`readonly` は、含んでいるメソッドから推論されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-262">The `readonly` will be inferred from the containing method.</span></span>

<span data-ttu-id="22f02-263">これは、`return ref readonly <expression>` が不要であり、常にエラーになる `readonly` 部分でのみ不一致が発生する可能性があることです。</span><span class="sxs-lookup"><span data-stu-id="22f02-263">The motivation is that `return ref readonly <expression>` is unnecessary long and only allows for mismatches on the `readonly` part that would always result in errors.</span></span>
<span data-ttu-id="22f02-264">ただし、厳密なエイリアスと値によって何らかの値が渡される他のシナリオとの一貫性を確保するには、`ref` が必要です。</span><span class="sxs-lookup"><span data-stu-id="22f02-264">The `ref` is, however, required for consistency with other scenarios where something is passed via strict aliasing vs. by value.</span></span>

> <span data-ttu-id="22f02-265">`in` パラメーターの場合とは異なり、`ref readonly` はローカルコピー経由で返されることはありません。</span><span class="sxs-lookup"><span data-stu-id="22f02-265">Unlike the case with `in` parameters, `ref readonly` returns never return via a local copy.</span></span> <span data-ttu-id="22f02-266">このような手法が返された直後にコピーが存在しないことを考慮すると、無意味で危険になります。</span><span class="sxs-lookup"><span data-stu-id="22f02-266">Considering that the copy would cease to exist immediately upon returning such practice would be pointless and dangerous.</span></span> <span data-ttu-id="22f02-267">したがって `ref readonly` 戻り値は常に直接参照です。</span><span class="sxs-lookup"><span data-stu-id="22f02-267">Therefore `ref readonly` returns are always direct references.</span></span>

<span data-ttu-id="22f02-268">例:</span><span class="sxs-lookup"><span data-stu-id="22f02-268">Example:</span></span>

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- <span data-ttu-id="22f02-269">`return ref` の引数は左辺値である必要があります (**既存のルール**)</span><span class="sxs-lookup"><span data-stu-id="22f02-269">An argument of `return ref` must be an LValue (**existing rule**)</span></span>
- <span data-ttu-id="22f02-270">`return ref` の引数は "return to return" (既存の**ルール**) にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-270">An argument of `return ref` must be "safe to return" (**existing rule**)</span></span>
- <span data-ttu-id="22f02-271">`ref readonly` メンバーでは、`return ref` の引数は_書き込み可能である必要はありません_。</span><span class="sxs-lookup"><span data-stu-id="22f02-271">In a `ref readonly` member an argument of `return ref` is _not required to be writeable_ .</span></span>
<span data-ttu-id="22f02-272">たとえば、このようなメンバーは、readonly フィールドまたはその `in` パラメーターの1つを参照して返すことができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-272">For example such member can ref-return a readonly field or one of its `in` parameters.</span></span>

### <a name="safe-to-return-rules"></a><span data-ttu-id="22f02-273">規則を返すことが安全です。</span><span class="sxs-lookup"><span data-stu-id="22f02-273">Safe to Return rules.</span></span>
<span data-ttu-id="22f02-274">参照用の規則を返す通常の安全な方法は、読み取り専用の参照にも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="22f02-274">Normal safe to return rules for references will apply to readonly references as well.</span></span>

<span data-ttu-id="22f02-275">`ref readonly` は、通常の `ref` ローカル/パラメーター/戻り値から取得できますが、他の方法は取得できないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="22f02-275">Note that a `ref readonly` can be obtained from a regular `ref` local/parameter/return, but not the other way around.</span></span> <span data-ttu-id="22f02-276">それ以外の場合、`ref readonly` が返す安全性は、通常の `ref` が返す場合と同じ方法で推定されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-276">Otherwise the safety of `ref readonly` returns is inferred the same way as for regular `ref` returns.</span></span>

<span data-ttu-id="22f02-277">右辺値を `in` パラメーターとして渡し、`ref readonly` として返すことを考慮すると、もう1つの規則が必要になります。これ**は、参照渡しでは安全に返すことができません**。</span><span class="sxs-lookup"><span data-stu-id="22f02-277">Considering that RValues can be passed as `in` parameter and returned as `ref readonly` we need one more rule - **RValues are not safe-to-return by reference**.</span></span>

> <span data-ttu-id="22f02-278">右辺値がコピーを介して `in` パラメーターに渡され、`ref readonly`の形式で返される場合を考えてみます。</span><span class="sxs-lookup"><span data-stu-id="22f02-278">Consider the situation when an RValue is passed to an `in` parameter via a copy and then returned back in a form of a `ref readonly`.</span></span> <span data-ttu-id="22f02-279">呼び出し元のコンテキストでは、このような呼び出しの結果はローカルデータへの参照であるため、を返すのは安全ではありません。</span><span class="sxs-lookup"><span data-stu-id="22f02-279">In the context of the caller the result of such invocation is a reference to local data and as such is unsafe to return.</span></span>
> <span data-ttu-id="22f02-280">右辺値が返されても安全でない場合は、既存のルール `#6` によって既に処理されています。</span><span class="sxs-lookup"><span data-stu-id="22f02-280">Once RValues are not safe to return, the existing rule `#6` already handles this case.</span></span>

<span data-ttu-id="22f02-281">例:</span><span class="sxs-lookup"><span data-stu-id="22f02-281">Example:</span></span>
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

<span data-ttu-id="22f02-282">更新された `safe to return` ルール:</span><span class="sxs-lookup"><span data-stu-id="22f02-282">Updated `safe to return` rules:</span></span>

1.  <span data-ttu-id="22f02-283">**ヒープ上の変数への参照は、安全に返すことができる**</span><span class="sxs-lookup"><span data-stu-id="22f02-283">**refs to variables on the heap are safe to return**</span></span>
2.  <span data-ttu-id="22f02-284">**ref/in パラメーターは
を確実に返すことが**できます `in` パラメーターは読み取り専用としてのみ返すことができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-284">**ref/in parameters are safe to return**
`in` parameters naturally can only be returned as readonly.</span></span>
3.  <span data-ttu-id="22f02-285">**out パラメーターは**、既に使用されているように、確実に返すことができます (ただし、既に割り当てられている必要があります)。</span><span class="sxs-lookup"><span data-stu-id="22f02-285">**out parameters are safe to return** (but must be definitely assigned, as is already the case today)</span></span>
4.  <span data-ttu-id="22f02-286">**インスタンス構造体フィールドは、受信側が安全に戻ることができる限り、安全に返すことができる**</span><span class="sxs-lookup"><span data-stu-id="22f02-286">**instance struct fields are safe to return as long as the receiver is safe to return**</span></span>
5.  <span data-ttu-id="22f02-287">**' this ' は、構造体メンバーからは安全ではありません**</span><span class="sxs-lookup"><span data-stu-id="22f02-287">**'this' is not safe to return from struct members**</span></span>
6.  <span data-ttu-id="22f02-288">**他のメソッドから返される参照は、仮パラメーターとしてそのメソッドに渡されたすべての refs/アウトが安全に返すことができた場合に、安全に返すことができます。** 具体的には、受信側*が構造体、クラス、ジェネリック型パラメーターとして型指定されているかどうかに関係なく、受信側が安全に戻ることができるかどうかは関係ありません
。*</span><span class="sxs-lookup"><span data-stu-id="22f02-288">**a ref, returned from another method is safe to return if all refs/outs passed to that method as formal parameters were safe to return.**
*Specifically it is irrelevant if receiver is safe to return, regardless whether receiver is a struct, class or typed as a generic type parameter.*</span></span>
7.  <span data-ttu-id="22f02-289">**右辺値は、参照渡しでは安全ではありません。** 
*具体的には、右辺値をパラメーターとして渡すことが安全です。*</span><span class="sxs-lookup"><span data-stu-id="22f02-289">**RValues are not safe to return by reference.**
*Specifically RValues are safe to pass as in parameters.*</span></span>

> <span data-ttu-id="22f02-290">注: ref のような型と参照の再割り当てが関係している場合は、戻り値の安全性に関する追加の規則があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-290">NOTE: There are additional rules regarding safety of returns that come into play when ref-like types and ref-reassignments are involved.</span></span>
> <span data-ttu-id="22f02-291">これらの規則は `ref` と `ref readonly` メンバーにも適用されるため、ここでは説明しません。</span><span class="sxs-lookup"><span data-stu-id="22f02-291">The rules equally apply to `ref` and `ref readonly` members and therefore are not mentioned here.</span></span>

### <a name="aliasing-behavior"></a><span data-ttu-id="22f02-292">エイリアス動作。</span><span class="sxs-lookup"><span data-stu-id="22f02-292">Aliasing behavior.</span></span>
<span data-ttu-id="22f02-293">`ref readonly` メンバーは、通常の `ref` メンバーと同じエイリアス動作を提供します (readonly の場合を除く)。</span><span class="sxs-lookup"><span data-stu-id="22f02-293">`ref readonly` members provide the same aliasing behavior as ordinary `ref` members (except for being readonly).</span></span>
<span data-ttu-id="22f02-294">したがって、ラムダ、非同期、反復子、stack 書き込むなどでキャプチャするために使用します。同じ制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-294">Therefore for the purpose of capturing in lambdas, async, iterators, stack spilling etc... the same restrictions apply.</span></span> <span data-ttu-id="22f02-295">:.</span><span class="sxs-lookup"><span data-stu-id="22f02-295">- I.E.</span></span> <span data-ttu-id="22f02-296">実際の参照をキャプチャできないため、またメンバーの評価に副作用があるため、このようなシナリオは許可されません。</span><span class="sxs-lookup"><span data-stu-id="22f02-296">due to inability to capture the actual references and due to side-effecting nature of member evaluation such scenarios are disallowed.</span></span>

> <span data-ttu-id="22f02-297">これは許可されており、通常の書き込み可能な参照として `this` を受け取る通常の構造体メソッドの受信側で `ref readonly` が返される場合に、コピーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-297">It is permitted and required to make a copy when `ref readonly` return is a receiver of regular struct methods, which take `this` as an ordinary writeable reference.</span></span> <span data-ttu-id="22f02-298">以前は、このような呼び出しが読み取り専用変数に適用されるすべての場合、ローカルコピーが作成されていました。</span><span class="sxs-lookup"><span data-stu-id="22f02-298">Historically in all cases where such invocations are applied to readonly variable a local copy is made.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="22f02-299">メタデータ表現。</span><span class="sxs-lookup"><span data-stu-id="22f02-299">Metadata representation.</span></span>
<span data-ttu-id="22f02-300">Byref を返すメソッドの戻り値に `System.Runtime.CompilerServices.IsReadOnlyAttribute` が適用されている場合は、メソッドが読み取り専用の参照を返すことを意味します。</span><span class="sxs-lookup"><span data-stu-id="22f02-300">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to the return of a byref returning method, it means that the method returns a readonly reference.</span></span>

<span data-ttu-id="22f02-301">また、このようなメソッドの結果のシグネチャ (およびそれらのメソッドのみ) には `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`が必要です。</span><span class="sxs-lookup"><span data-stu-id="22f02-301">In addition, the result signature of such methods (and only those methods) must have `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span></span>

<span data-ttu-id="22f02-302">**動機**: `ref readonly` が返されたメソッドを呼び出すときに、既存のコンパイラが単に `readonly` を無視できないようにするためです。</span><span class="sxs-lookup"><span data-stu-id="22f02-302">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when invoking methods with `ref readonly` returns</span></span>

## <a name="readonly-structs"></a><span data-ttu-id="22f02-303">Readonly 構造体</span><span class="sxs-lookup"><span data-stu-id="22f02-303">Readonly structs</span></span>
<span data-ttu-id="22f02-304">Short-コンストラクターを除き、構造体のすべてのインスタンスメンバーの `this` パラメーターを作成する機能。これは、`in` パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="22f02-304">In short - a feature that makes `this` parameter of all instance members of a struct, except for constructors, an `in` parameter.</span></span>

### <a name="motivation"></a><span data-ttu-id="22f02-305">目的</span><span class="sxs-lookup"><span data-stu-id="22f02-305">Motivation</span></span>
<span data-ttu-id="22f02-306">コンパイラは、構造体インスタンスでメソッドを呼び出すとインスタンスが変更される可能性があると想定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-306">Compiler must assume that any method call on a struct instance may modify the instance.</span></span> <span data-ttu-id="22f02-307">実際には、書き込み可能な参照は `this` パラメーターとしてメソッドに渡され、この動作を完全に有効にします。</span><span class="sxs-lookup"><span data-stu-id="22f02-307">Indeed a writeable reference is passed to the method as `this` parameter and fully enables this behavior.</span></span> <span data-ttu-id="22f02-308">`readonly` 変数に対してこのような呼び出しを許可するために、呼び出しは一時コピーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-308">To allow such invocations on `readonly` variables, the invocations are applied to temp copies.</span></span> <span data-ttu-id="22f02-309">これはにくいになる可能性があり、場合によっては、パフォーマンス上の理由から `readonly` を破棄することがあります。</span><span class="sxs-lookup"><span data-stu-id="22f02-309">That could be unintuitive and sometimes forces people to abandon `readonly` for performance reasons.</span></span>  
<span data-ttu-id="22f02-310">例: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span><span class="sxs-lookup"><span data-stu-id="22f02-310">Example: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span></span>

<span data-ttu-id="22f02-311">`in` パラメーターと `ref readonly` のサポートを追加した後は、読み取り専用の変数がより一般的になるため、防御的なコピーの問題が悪化します。</span><span class="sxs-lookup"><span data-stu-id="22f02-311">After adding support for `in` parameters and `ref readonly` returns the problem of defensive copying will get worse since readonly variables will become more common.</span></span>

### <a name="solution"></a><span data-ttu-id="22f02-312">解決策</span><span class="sxs-lookup"><span data-stu-id="22f02-312">Solution</span></span>
<span data-ttu-id="22f02-313">コンストラクターを除くすべての構造体インスタンスメソッドで、`this` を `in` パラメーターとして処理することになる構造体宣言で `readonly` 修飾子を許可します。</span><span class="sxs-lookup"><span data-stu-id="22f02-313">Allow `readonly` modifier on struct declarations which would result in `this` being treated as `in` parameter on all struct instance methods except for constructors.</span></span>

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a><span data-ttu-id="22f02-314">Readonly 構造体のメンバーに関する制限事項</span><span class="sxs-lookup"><span data-stu-id="22f02-314">Restrictions on members of readonly struct</span></span>
- <span data-ttu-id="22f02-315">読み取り専用の構造体のインスタンスフィールドは読み取り専用である必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-315">Instance fields of a readonly struct must be readonly.</span></span>  
<span data-ttu-id="22f02-316">**動機:** 外部にのみ書き込むことができますが、メンバーを経由することはできません。</span><span class="sxs-lookup"><span data-stu-id="22f02-316">**Motivation:** can only be written to externally, but not through members.</span></span>
- <span data-ttu-id="22f02-317">読み取り専用の構造体のインスタンス autoproperties は、get 専用である必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-317">Instance autoproperties of a readonly struct must be get-only.</span></span>  
<span data-ttu-id="22f02-318">**動機:** インスタンスフィールドに対する制限の結果。</span><span class="sxs-lookup"><span data-stu-id="22f02-318">**Motivation:** consequence of restriction on instance fields.</span></span>
- <span data-ttu-id="22f02-319">Readonly 構造体は、フィールドのようなイベントを宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="22f02-319">Readonly struct may not declare field-like events.</span></span>  
<span data-ttu-id="22f02-320">**動機:** インスタンスフィールドに対する制限の結果。</span><span class="sxs-lookup"><span data-stu-id="22f02-320">**Motivation:** consequence of restriction on instance fields.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="22f02-321">メタデータ表現。</span><span class="sxs-lookup"><span data-stu-id="22f02-321">Metadata representation.</span></span>
<span data-ttu-id="22f02-322">`System.Runtime.CompilerServices.IsReadOnlyAttribute` が値型に適用される場合、その型が `readonly struct`であることを意味します。</span><span class="sxs-lookup"><span data-stu-id="22f02-322">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a value type, it means that the type is a `readonly struct`.</span></span>

<span data-ttu-id="22f02-323">特に次の点に違いがあります。</span><span class="sxs-lookup"><span data-stu-id="22f02-323">In particular:</span></span>
-  <span data-ttu-id="22f02-324">`IsReadOnlyAttribute` 型の id は重要ではありません。</span><span class="sxs-lookup"><span data-stu-id="22f02-324">The identity of the `IsReadOnlyAttribute` type is unimportant.</span></span> <span data-ttu-id="22f02-325">実際には、必要に応じて、それを含んでいるアセンブリのコンパイラによって埋め込むことができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-325">In fact it can be embedded by the compiler in the containing assembly if needed.</span></span>

## <a name="refin-extension-methods"></a><span data-ttu-id="22f02-326">`ref`/`in` 拡張メソッド</span><span class="sxs-lookup"><span data-stu-id="22f02-326">`ref`/`in` extension methods</span></span>
<span data-ttu-id="22f02-327">実際には、既存の提案 (https://github.com/dotnet/roslyn/issues/165) とそれに対応するプロトタイプ PR (https://github.com/dotnet/roslyn/pull/15650)があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-327">There is actually an existing proposal (https://github.com/dotnet/roslyn/issues/165) and corresponding prototype PR (https://github.com/dotnet/roslyn/pull/15650).</span></span>
<span data-ttu-id="22f02-328">このアイデアがまったく新しいものではないことを確認したいだけです。</span><span class="sxs-lookup"><span data-stu-id="22f02-328">I just want to acknowledge that this idea is not entirely new.</span></span> <span data-ttu-id="22f02-329">ただし、このような方法に関する最も悪化問題が `ref readonly` によって簡単に削除されるため、ここで関連しています。 RValue レシーバーを使用した場合の対処方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="22f02-329">It is, however, relevant here since `ref readonly` elegantly removes the most contentious issue about such methods - what to do with RValue receivers.</span></span>

<span data-ttu-id="22f02-330">一般的な考え方は、型が構造体型であることがわかっている限り、拡張メソッドが参照によって `this` パラメーターを取得できるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="22f02-330">The general idea is allowing extension methods to take the `this` parameter by reference, as long as the type is known to be a struct type.</span></span>

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

<span data-ttu-id="22f02-331">このような拡張メソッドを作成する理由は主に次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="22f02-331">The reasons for writing such extension methods are primarily:</span></span>  
1.  <span data-ttu-id="22f02-332">レシーバーが大きな構造体である場合はコピーしない</span><span class="sxs-lookup"><span data-stu-id="22f02-332">Avoid copying when receiver is a large struct</span></span>
2.  <span data-ttu-id="22f02-333">構造体の拡張メソッドの変更を許可する</span><span class="sxs-lookup"><span data-stu-id="22f02-333">Allow mutating extension methods on structs</span></span>

<span data-ttu-id="22f02-334">クラスでこれを許可しない理由</span><span class="sxs-lookup"><span data-stu-id="22f02-334">The reasons why we do not want to allow this on classes</span></span>  
1.  <span data-ttu-id="22f02-335">その目的は非常に限られています。</span><span class="sxs-lookup"><span data-stu-id="22f02-335">It would be of very limited purpose.</span></span>
2.  <span data-ttu-id="22f02-336">メソッドの呼び出しによって、呼び出し後に`null` 以外の受信者が `null` になることはないので、長時間不変ではなくなります。</span><span class="sxs-lookup"><span data-stu-id="22f02-336">It would break long standing invariant that a method call cannot turn non-`null` receiver to become `null` after invocation.</span></span>
> <span data-ttu-id="22f02-337">実際には、`ref` または `out`によって_明示的_に割り当てられていない限り、`null` ない変数を `null` にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="22f02-337">In fact, currently a non-`null` variable cannot become `null` unless _explicitly_ assigned or passed by `ref` or `out`.</span></span>
> <span data-ttu-id="22f02-338">読みやすくするため、またはその他の形式の "ここでは null にすることができます" 分析が非常に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="22f02-338">That greatly aids readability or other forms of "can this be a null here" analysis.</span></span>
3.  <span data-ttu-id="22f02-339">Null 条件付きアクセスの "評価1回のみ" セマンティクスによって調整するのは困難です。</span><span class="sxs-lookup"><span data-stu-id="22f02-339">It would be hard to reconcile with "evaluate once" semantics of null-conditional accesses.</span></span>
<span data-ttu-id="22f02-340">例: `obj.stringField?.RefExtension(...)`-`stringField` のコピーをキャプチャして null チェックを意味を持つようにする必要がありますが、RefExtension 内の `this` への代入はフィールドに反映されません。</span><span class="sxs-lookup"><span data-stu-id="22f02-340">Example: `obj.stringField?.RefExtension(...)` - need to capture a copy of `stringField` to make the null check meaningful, but then assignments to `this` inside RefExtension would not be reflected back to the field.</span></span>

<span data-ttu-id="22f02-341">参照渡しで最初の引数を受け取る**構造体**に対して拡張メソッドを宣言する機能は、長時間要求でした。</span><span class="sxs-lookup"><span data-stu-id="22f02-341">An ability to declare extension methods on **structs** that take the first argument by reference was a long-standing request.</span></span> <span data-ttu-id="22f02-342">ブロックの考慮事項の1つは、"レシーバーが左辺値ではない場合の動作" です。</span><span class="sxs-lookup"><span data-stu-id="22f02-342">One of the blocking consideration was "what happens if receiver is not an LValue?".</span></span>

- <span data-ttu-id="22f02-343">すべての拡張メソッドを静的メソッドとして呼び出すこともできます (あいまいさを解決する唯一の方法である場合もあります)。</span><span class="sxs-lookup"><span data-stu-id="22f02-343">There is a precedent that any extension method could also be called as a static method (sometimes it is the only way to resolve ambiguity).</span></span> <span data-ttu-id="22f02-344">これは、右辺値レシーバーを許可しないことを指定します。</span><span class="sxs-lookup"><span data-stu-id="22f02-344">It would dictate that RValue receivers should be disallowed.</span></span>
- <span data-ttu-id="22f02-345">一方、構造体のインスタンスメソッドが関係する場合は、同様の状況でコピーに対して呼び出しを行う方法もあります。</span><span class="sxs-lookup"><span data-stu-id="22f02-345">On the other hand there is a practice of making invocation on a copy in similar situations when struct instance methods are involved.</span></span>

<span data-ttu-id="22f02-346">"暗黙的なコピー" が存在する理由は、構造体のメソッドの大部分は実際には構造体を変更せず、を示すことができないためです。</span><span class="sxs-lookup"><span data-stu-id="22f02-346">The reason why the "implicit copying" exists is because the majority of struct methods do not actually modify the struct while not being able to indicate that.</span></span> <span data-ttu-id="22f02-347">そのため、最も実用的な解決策は、コピーでの呼び出しを行うだけでしたが、この方法は、パフォーマンスの低下やバグの原因となっていることがわかっています。</span><span class="sxs-lookup"><span data-stu-id="22f02-347">Therefore the most practical solution was to just make the invocation on a copy, but this practice is known for harming performance and causing bugs.</span></span>

<span data-ttu-id="22f02-348">現在、`in` パラメーターを使用できるようになったため、拡張機能によって意図が通知される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-348">Now, with availability of `in` parameters, it is possible for an extension to signal the intent.</span></span> <span data-ttu-id="22f02-349">そのため、難問拡張機能が書き込み可能な受信側で呼び出されることを `ref` 要求することで、を解決できます。 `in` 拡張機能では、必要に応じて暗黙的なコピーが許可されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-349">Therefore the conundrum can be resolved by requiring `ref` extensions to be called with writeable receivers while `in` extensions permit implicit copying if necessary.</span></span>

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a><span data-ttu-id="22f02-350">`in` 拡張機能とジェネリック。</span><span class="sxs-lookup"><span data-stu-id="22f02-350">`in` extensions and generics.</span></span>
<span data-ttu-id="22f02-351">`ref` 拡張メソッドの目的は、受信側を直接変更するか、または変化するメンバーを呼び出すことです。</span><span class="sxs-lookup"><span data-stu-id="22f02-351">The purpose of `ref` extension methods is to mutate the receiver directly or by invoking mutating members.</span></span> <span data-ttu-id="22f02-352">したがって、`T` が構造体として制約されている限り、`ref this T` の拡張機能を使用できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-352">Therefore `ref this T` extensions are allowed as long as `T` is constrained to be a struct.</span></span>

<span data-ttu-id="22f02-353">一方 `in` 拡張メソッドは、暗黙的なコピーを減らすために特に存在します。</span><span class="sxs-lookup"><span data-stu-id="22f02-353">On the other hand `in` extension methods exist specifically to reduce implicit copying.</span></span> <span data-ttu-id="22f02-354">ただし、`in T` パラメーターの使用は、インターフェイスメンバーを介して行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-354">However any use of an `in T` parameter will have to be done through an interface member.</span></span> <span data-ttu-id="22f02-355">すべてのインターフェイスメンバーは変化していると見なされるため、このような使用ではコピーが必要になります。</span><span class="sxs-lookup"><span data-stu-id="22f02-355">Since all interface members are considered mutating, any such use would require a copy.</span></span> <span data-ttu-id="22f02-356">-コピーを減らすのではなく、その逆の効果が得られます。</span><span class="sxs-lookup"><span data-stu-id="22f02-356">- Instead of reducing copying, the effect would be the opposite.</span></span> <span data-ttu-id="22f02-357">したがって、制約に関係なく `T` がジェネリック型パラメーターである場合、`in this T` は許可されません。</span><span class="sxs-lookup"><span data-stu-id="22f02-357">Therefore `in this T` is not allowed when `T` is a generic type parameter regardless of constraints.</span></span>

### <a name="valid-kinds-of-extension-methods-recap"></a><span data-ttu-id="22f02-358">有効な拡張メソッドの種類 (要約):</span><span class="sxs-lookup"><span data-stu-id="22f02-358">Valid kinds of extension methods (recap):</span></span>
<span data-ttu-id="22f02-359">拡張メソッドでは、次の形式の `this` 宣言が許可されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="22f02-359">The following forms of `this` declaration in an extension method are now allowed:</span></span>
1) <span data-ttu-id="22f02-360">`this T arg`-通常の byval 拡張。</span><span class="sxs-lookup"><span data-stu-id="22f02-360">`this T arg` - regular byval extension.</span></span> <span data-ttu-id="22f02-361">(**既存のケース**)</span><span class="sxs-lookup"><span data-stu-id="22f02-361">(**existing case**)</span></span>
- <span data-ttu-id="22f02-362">T には、参照型または型パラメーターを含む任意の型を指定できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-362">T can be any type, including reference types or type parameters.</span></span>
<span data-ttu-id="22f02-363">インスタンスは、呼び出しの後に同じ変数になります。</span><span class="sxs-lookup"><span data-stu-id="22f02-363">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="22f02-364">_この引数変換_の種類の暗黙的な変換を許可します。</span><span class="sxs-lookup"><span data-stu-id="22f02-364">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="22f02-365">右辺値で呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-365">Can be called on RValues.</span></span>

- <span data-ttu-id="22f02-366"> - `in` 拡張機能を `in this T self`します。</span><span class="sxs-lookup"><span data-stu-id="22f02-366">`in this T self` - `in` extension.</span></span>
<span data-ttu-id="22f02-367">T は実際の構造体型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-367">T must be an actual struct type.</span></span>
<span data-ttu-id="22f02-368">インスタンスは、呼び出しの後に同じ変数になります。</span><span class="sxs-lookup"><span data-stu-id="22f02-368">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="22f02-369">_この引数変換_の種類の暗黙的な変換を許可します。</span><span class="sxs-lookup"><span data-stu-id="22f02-369">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="22f02-370">右辺値で呼び出すことができます (必要に応じて、一時で呼び出すことができます)。</span><span class="sxs-lookup"><span data-stu-id="22f02-370">Can be called on RValues (may be invoked on a temp if needed).</span></span>

- <span data-ttu-id="22f02-371"> - `ref` 拡張機能を `ref this T self`します。</span><span class="sxs-lookup"><span data-stu-id="22f02-371">`ref this T self` - `ref` extension.</span></span>
<span data-ttu-id="22f02-372">T は構造体型であるか、または構造体として制約されるジェネリック型パラメーターである必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-372">T must be a struct type or a generic type parameter constrained to be a struct.</span></span>
<span data-ttu-id="22f02-373">インスタンスは、呼び出しによって書き込まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-373">Instance may be written to by the invocation.</span></span>
<span data-ttu-id="22f02-374">Id 変換のみを許可します。</span><span class="sxs-lookup"><span data-stu-id="22f02-374">Allows only identity conversions.</span></span>
<span data-ttu-id="22f02-375">書き込み可能な左辺値で呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-375">Must be called on writeable LValue.</span></span> <span data-ttu-id="22f02-376">(temp 経由では呼び出されません)。</span><span class="sxs-lookup"><span data-stu-id="22f02-376">(never invoked via a temp).</span></span>

## <a name="readonly-ref-locals"></a><span data-ttu-id="22f02-377">Readonly ref ローカル変数。</span><span class="sxs-lookup"><span data-stu-id="22f02-377">Readonly ref locals.</span></span>

### <a name="motivation"></a><span data-ttu-id="22f02-378">動機.</span><span class="sxs-lookup"><span data-stu-id="22f02-378">Motivation.</span></span>
<span data-ttu-id="22f02-379">`ref readonly` メンバーが導入されると、適切な種類のローカルとペアにする必要があることを明確にしました。</span><span class="sxs-lookup"><span data-stu-id="22f02-379">Once `ref readonly` members were introduced, it was clear from the use that they need to be paired with appropriate kind of local.</span></span> <span data-ttu-id="22f02-380">メンバーを評価すると副作用が生じる可能性があります。したがって、結果を複数回使用する必要がある場合は、格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-380">Evaluation of a member may produce or observe side effects, therefore if the result must be used more than once, it needs to be stored.</span></span> <span data-ttu-id="22f02-381">通常 `ref` ローカルは、`readonly` 参照を割り当てることができないため、ここでは役に立ちません。</span><span class="sxs-lookup"><span data-stu-id="22f02-381">Ordinary `ref` locals do not help here since they cannot be assigned a `readonly` reference.</span></span>   

### <a name="solution"></a><span data-ttu-id="22f02-382">Solution.</span><span class="sxs-lookup"><span data-stu-id="22f02-382">Solution.</span></span>
<span data-ttu-id="22f02-383">`ref readonly` ローカルの宣言を許可します。</span><span class="sxs-lookup"><span data-stu-id="22f02-383">Allow declaring `ref readonly` locals.</span></span> <span data-ttu-id="22f02-384">これは、書き込み可能でない新しい種類の `ref` ローカルです。</span><span class="sxs-lookup"><span data-stu-id="22f02-384">This is a new kind of `ref` locals that is not writeable.</span></span> <span data-ttu-id="22f02-385">その結果、ローカル `ref readonly` は、これらの変数を書き込みに公開せずに、読み取り専用変数への参照を受け入れることができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-385">As a result `ref readonly` locals can accept references to readonly variables without exposing these variables to writes.</span></span>

### <a name="declaring-and-using-ref-readonly-locals"></a><span data-ttu-id="22f02-386">`ref readonly` ローカルの宣言と使用</span><span class="sxs-lookup"><span data-stu-id="22f02-386">Declaring and using `ref readonly` locals.</span></span>

<span data-ttu-id="22f02-387">このようなローカルの構文では、宣言サイトで (特定の順序で) `ref readonly` 修飾子を使用します。</span><span class="sxs-lookup"><span data-stu-id="22f02-387">The syntax of such locals uses `ref readonly` modifiers at declaration site (in that specific order).</span></span> <span data-ttu-id="22f02-388">通常の `ref` ローカルの場合と同様に、`ref readonly` のローカル変数は宣言時に ref 初期化される必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-388">Similarly to ordinary `ref` locals, `ref readonly` locals must be ref-initialized at declaration.</span></span> <span data-ttu-id="22f02-389">通常の `ref` ローカルの場合とは異なり、`ref readonly` ローカルは `in` パラメーター、`readonly` フィールド、`ref readonly` メソッドなどの `readonly` LValues を参照できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-389">Unlike regular `ref` locals, `ref readonly` locals can refer to `readonly` LValues like `in` parameters, `readonly` fields, `ref readonly` methods.</span></span>

<span data-ttu-id="22f02-390">すべての目的において、`ref readonly` ローカルは `readonly` 変数として扱われます。</span><span class="sxs-lookup"><span data-stu-id="22f02-390">For all purposes a `ref readonly` local is treated as a `readonly` variable.</span></span> <span data-ttu-id="22f02-391">使用に関する制限事項のほとんどは、`readonly` フィールドまたは `in` パラメーターと同じです。</span><span class="sxs-lookup"><span data-stu-id="22f02-391">Most of the restrictions on the use are the same as with `readonly` fields or `in` parameters.</span></span>

<span data-ttu-id="22f02-392">たとえば、構造体型を持つ `in` パラメーターのフィールドは、すべて `readonly` 変数として再帰的に分類されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-392">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a><span data-ttu-id="22f02-393">`ref readonly` ローカルの使用に関する制限事項</span><span class="sxs-lookup"><span data-stu-id="22f02-393">Restrictions on use of `ref readonly` locals</span></span>
<span data-ttu-id="22f02-394">`readonly` の性質を除き、`ref readonly` ローカルは通常の `ref` ローカルと同様に動作し、まったく同じ制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-394">Except for their `readonly` nature, `ref readonly` locals behave like ordinary `ref` locals and are subject to exactly same restrictions.</span></span>  
<span data-ttu-id="22f02-395">クロージャでのキャプチャに関連する制限の例として、`async` メソッドでの宣言または `safe-to-return` 分析は `ref readonly` ローカルにも同様に適用されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-395">For example restrictions related to capturing in closures, declaring in `async` methods or the `safe-to-return` analysis equally applies to `ref readonly` locals.</span></span>

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a><span data-ttu-id="22f02-396">三項 `ref` 式。</span><span class="sxs-lookup"><span data-stu-id="22f02-396">Ternary `ref` expressions.</span></span> <span data-ttu-id="22f02-397">("条件付き左辺値" とも呼ばれる)</span><span class="sxs-lookup"><span data-stu-id="22f02-397">(aka "Conditional LValues")</span></span>

### <a name="motivation"></a><span data-ttu-id="22f02-398">目的</span><span class="sxs-lookup"><span data-stu-id="22f02-398">Motivation</span></span>
<span data-ttu-id="22f02-399">`ref` と `ref readonly` の使用は、条件に基づいて1つまたは別のターゲット変数を使用して、ローカルの参照を ref に初期化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-399">Use of `ref` and `ref readonly` locals exposed a need to ref-initialize such locals with one or another target variable based on a condition.</span></span>

<span data-ttu-id="22f02-400">一般的な回避策は、次のようなメソッドを導入することです。</span><span class="sxs-lookup"><span data-stu-id="22f02-400">A typical workaround is to introduce a method like:</span></span>

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

<span data-ttu-id="22f02-401">`Choice` は、_すべて_の引数が呼び出しサイトで評価される必要があるため、にくいの動作とバグにつながるため、三項が完全に置換されるわけではないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="22f02-401">Note that `Choice` is not an exact replacement of a ternary since _all_ arguments must be evaluated at the call site, which was leading to unintuitive behavior and bugs.</span></span>

<span data-ttu-id="22f02-402">次のものは期待どおりに機能しません。</span><span class="sxs-lookup"><span data-stu-id="22f02-402">The following will not work as expected:</span></span>

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a><span data-ttu-id="22f02-403">解決策</span><span class="sxs-lookup"><span data-stu-id="22f02-403">Solution</span></span>
<span data-ttu-id="22f02-404">条件に基づいて左辺値の引数のいずれかへの参照に評価される特殊な種類の条件式を許可します。</span><span class="sxs-lookup"><span data-stu-id="22f02-404">Allow special kind of conditional expression that evaluates to a reference to one of LValue argument based on a condition.</span></span>

### <a name="using-ref-ternary-expression"></a><span data-ttu-id="22f02-405">`ref` 三項式を使用します。</span><span class="sxs-lookup"><span data-stu-id="22f02-405">Using `ref` ternary expression.</span></span>

<span data-ttu-id="22f02-406">条件式の `ref` フレーバーの構文は `<condition> ? ref <consequence> : ref <alternative>;`</span><span class="sxs-lookup"><span data-stu-id="22f02-406">The syntax for the `ref` flavor of a conditional expression is `<condition> ? ref <consequence> : ref <alternative>;`</span></span>

<span data-ttu-id="22f02-407">通常の条件式の場合と同様に、ブール条件式の結果に応じて `<alternative>` が評価されるのは、`<consequence>` の場合のみです。</span><span class="sxs-lookup"><span data-stu-id="22f02-407">Just like with the ordinary conditional expression only `<consequence>` or `<alternative>` is evaluated depending on result of the boolean condition expression.</span></span>

<span data-ttu-id="22f02-408">通常の条件式とは異なり、`ref` 条件式:</span><span class="sxs-lookup"><span data-stu-id="22f02-408">Unlike ordinary conditional expression, `ref` conditional expression:</span></span>
- <span data-ttu-id="22f02-409">`<consequence>` と `<alternative>` が左辺値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-409">requires that `<consequence>` and `<alternative>` are LValues.</span></span>
- <span data-ttu-id="22f02-410">`ref` 条件式自体は左辺値であり、</span><span class="sxs-lookup"><span data-stu-id="22f02-410">`ref` conditional expression itself is an LValue and</span></span>
- <span data-ttu-id="22f02-411">`<consequence>` と `<alternative>` の両方が書き込み可能な値である場合 `ref` 条件式は書き込み可能です</span><span class="sxs-lookup"><span data-stu-id="22f02-411">`ref` conditional expression is writeable if both `<consequence>` and `<alternative>` are writeable LValues</span></span>

<span data-ttu-id="22f02-412">例 :</span><span class="sxs-lookup"><span data-stu-id="22f02-412">Examples:</span></span>  
<span data-ttu-id="22f02-413">`ref` 三項は左辺値であるため、参照渡しで渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-413">`ref` ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="22f02-414">左辺値である場合は、に割り当てることもできます。</span><span class="sxs-lookup"><span data-stu-id="22f02-414">Being an LValue, it can also be assigned to.</span></span>
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

<span data-ttu-id="22f02-415">は、メソッド呼び出しの受信者として使用でき、必要に応じてコピーをスキップします。</span><span class="sxs-lookup"><span data-stu-id="22f02-415">Can be used as a receiver of a method call and skip copying if necessary.</span></span>
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

<span data-ttu-id="22f02-416">`ref` 三項は、通常の (非 ref) コンテキストでも使用できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-416">`ref` ternary can be used in a regular (not ref) context as well.</span></span>
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a><span data-ttu-id="22f02-417">短所</span><span class="sxs-lookup"><span data-stu-id="22f02-417">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="22f02-418">参照と読み取り専用参照の拡張サポートに対する2つの主な引数を確認できます。</span><span class="sxs-lookup"><span data-stu-id="22f02-418">I can see two major arguments against enhanced support for references and readonly references:</span></span>

1) <span data-ttu-id="22f02-419">ここで解決される問題は、非常に古いものです。</span><span class="sxs-lookup"><span data-stu-id="22f02-419">The problems that are solved here are very old.</span></span> <span data-ttu-id="22f02-420">これは、特に既存のコードが役に立ちません。</span><span class="sxs-lookup"><span data-stu-id="22f02-420">Why suddenly solve them now, especially since it would not help existing code?</span></span>

<span data-ttu-id="22f02-421">新しいドメインでC#使用されていると .net では、いくつかの問題が目立つようになります。</span><span class="sxs-lookup"><span data-stu-id="22f02-421">As we find C# and .Net used in new domains, some problems become more prominent.</span></span>  
<span data-ttu-id="22f02-422">計算オーバーヘッドに関する平均よりも重要な環境の例としては、</span><span class="sxs-lookup"><span data-stu-id="22f02-422">As examples of environments that are more critical than average about computation overheads, I can list</span></span>

* <span data-ttu-id="22f02-423">計算が請求され、応答性が高いクラウド/データセンターのシナリオは、競争上の利点です。</span><span class="sxs-lookup"><span data-stu-id="22f02-423">cloud/datacenter scenarios where computation is billed for and responsiveness is a competitive advantage.</span></span>
* <span data-ttu-id="22f02-424">待機時間に関するソフトリアルタイムの要件を持つゲーム/VR/AR</span><span class="sxs-lookup"><span data-stu-id="22f02-424">Games/VR/AR with soft-realtime requirements on latencies</span></span>     

<span data-ttu-id="22f02-425">この機能では、タイプセーフなどの既存の長所を一切犠牲にすることはできませんが、一般的なシナリオではオーバーヘッドを低く抑えることができます。</span><span class="sxs-lookup"><span data-stu-id="22f02-425">This feature does not sacrifice any of the existing strengths such as type-safety, while allowing to lower overheads in some common scenarios.</span></span>

2) <span data-ttu-id="22f02-426">`readonly` コントラクトを使用すると、呼び出し先がルールによって再生されることを合理的に保証できますか。</span><span class="sxs-lookup"><span data-stu-id="22f02-426">Can we reasonably guarantee that the callee will play by the rules when it opts into `readonly` contracts?</span></span>

<span data-ttu-id="22f02-427">`out`を使用する場合も同様の信頼があります。</span><span class="sxs-lookup"><span data-stu-id="22f02-427">We have similar trust when using `out`.</span></span> <span data-ttu-id="22f02-428">`out` の実装が正しくないと、特定できない動作が発生することがありますが、実際にはほとんど発生しません。</span><span class="sxs-lookup"><span data-stu-id="22f02-428">Incorrect implementation of `out` can cause unspecified behavior, but in reality it rarely happens.</span></span>  

<span data-ttu-id="22f02-429">`ref readonly` に習熟している正式な検証規則を作成すると、信頼の問題がさらに軽減されます。</span><span class="sxs-lookup"><span data-stu-id="22f02-429">Making the formal verification rules familiar with `ref readonly` would further mitigate the trust issue.</span></span>

### <a name="alternatives"></a><span data-ttu-id="22f02-430">代替</span><span class="sxs-lookup"><span data-stu-id="22f02-430">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="22f02-431">競合の主な設計は、実際には "do nothing" です。</span><span class="sxs-lookup"><span data-stu-id="22f02-431">The main competing design is really "do nothing".</span></span>

### <a name="unresolved-questions"></a><span data-ttu-id="22f02-432">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="22f02-432">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a><span data-ttu-id="22f02-433">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="22f02-433">Design meetings</span></span>

<span data-ttu-id="22f02-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span><span class="sxs-lookup"><span data-stu-id="22f02-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span></span>
