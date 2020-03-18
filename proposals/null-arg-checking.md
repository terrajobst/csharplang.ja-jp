---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483542"
---
# <a name="simplified-null-argument-checking"></a><span data-ttu-id="b9c5e-101">簡略化された Null 引数のチェック</span><span class="sxs-lookup"><span data-stu-id="b9c5e-101">Simplified Null Argument Checking</span></span>

## <a name="summary"></a><span data-ttu-id="b9c5e-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="b9c5e-102">Summary</span></span>
<span data-ttu-id="b9c5e-103">この提案では、メソッドの引数を検証するための簡略化された構文を提供し、`ArgumentNullException` を適切にスロー `null` します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-103">This proposal provides a simplified syntax for validating method arguments are not `null` and throwing `ArgumentNullException` appropriately.</span></span>

## <a name="motivation"></a><span data-ttu-id="b9c5e-104">目的</span><span class="sxs-lookup"><span data-stu-id="b9c5e-104">Motivation</span></span>
<span data-ttu-id="b9c5e-105">Null 許容の参照型を設計する作業で、`null` 引数の検証に必要なコードを確認しました。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-105">The work on designing nullable reference types has caused us to examine the code necessary for `null` argument validation.</span></span> <span data-ttu-id="b9c5e-106">NRT がコード実行開発者に影響を与えない場合でも、完全に `null` クリーンであるプロジェクトでも、`if (arg is null) throw` ボイラープレートコードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-106">Given that NRT doesn't affect code execution developers still must add `if (arg is null) throw` boiler plate code even in projects which are fully `null` clean.</span></span> <span data-ttu-id="b9c5e-107">これにより、言語で検証 `null` 引数の最小構文を調べることが求められました。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-107">This gave us the desire to explore a minimal syntax for argument `null` validation in the language.</span></span> 

<span data-ttu-id="b9c5e-108">この `null` パラメーター検証構文は、NRT と頻繁にペアになることが想定されていますが、提案は完全に独立しています。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-108">While this `null` parameter validation syntax is expected to pair frequently with NRT the proposal is fully independent of it.</span></span> <span data-ttu-id="b9c5e-109">構文は `#nullable` ディレクティブとは無関係に使用できます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-109">The syntax can be used independent of `#nullable` directives.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b9c5e-110">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="b9c5e-110">Detailed Design</span></span> 

### <a name="null-validation-parameter-syntax"></a><span data-ttu-id="b9c5e-111">Null 検証パラメーターの構文</span><span class="sxs-lookup"><span data-stu-id="b9c5e-111">Null validation parameter syntax</span></span>
<span data-ttu-id="b9c5e-112">パラメーターリストのパラメーター名の後に、感嘆符演算子 (`!`) を配置できます。これにC#より、コンパイラは、そのパラメーターの標準 `null` チェックコードを出力します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-112">The bang operator, `!`, can be positioned after a parameter name in a parameter list and this will cause the C# compiler to emit standard `null` checking code for that parameter.</span></span> <span data-ttu-id="b9c5e-113">これは `null` 検証パラメーターの構文と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-113">This is referred to as `null` validation parameter syntax.</span></span> <span data-ttu-id="b9c5e-114">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-114">For example:</span></span>

``` csharp
void M(string name!) {
    ...
}
```

<span data-ttu-id="b9c5e-115">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-115">Will be translated into:</span></span>

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

<span data-ttu-id="b9c5e-116">生成された `null` チェックは、開発者がメソッド内でコードを作成する前に発生します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-116">The generated `null` check will occur before any developer authored code in the method.</span></span> <span data-ttu-id="b9c5e-117">複数のパラメーターに `!` 演算子が含まれている場合は、パラメーターが宣言されている順序でチェックが行われます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-117">When multiple parameters contain the `!` operator then the checks will occur in the same order as the parameters are declared.</span></span>

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

<span data-ttu-id="b9c5e-118">このチェックは、特に `null`との参照の等価性のために行われますが、`==` またはユーザー定義の演算子は呼び出しません。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-118">The check will be specifically for reference equality to `null`, it does not invoke `==` or any user defined operators.</span></span> <span data-ttu-id="b9c5e-119">また、`!` 演算子は、`null`に対して型が等しいかどうかをテストできるパラメーターにのみ追加できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-119">This also means the `!` operator can only be added to parameters whose type can be tested for equality against `null`.</span></span> <span data-ttu-id="b9c5e-120">これは、型が値型であることがわかっているパラメーターでは使用できないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-120">This means it can't be used on a parameter whose type is known to be a value type.</span></span>

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

<span data-ttu-id="b9c5e-121">コンストラクターの場合は、コンストラクター内の他のコードの前に `null` 検証が行われます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-121">In the case of a constructor the `null` validation will occur before any other code in the constructor.</span></span> <span data-ttu-id="b9c5e-122">次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-122">That includes:</span></span> 

- <span data-ttu-id="b9c5e-123">`this` または `base` を使用した他のコンストラクターへのチェーン</span><span class="sxs-lookup"><span data-stu-id="b9c5e-123">Chaining to other constructors with `this` or `base`</span></span> 
- <span data-ttu-id="b9c5e-124">コンストラクターで暗黙的に発生するフィールド初期化子</span><span class="sxs-lookup"><span data-stu-id="b9c5e-124">Field initializers which implicitly occur in the constructor</span></span>

<span data-ttu-id="b9c5e-125">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-125">For example:</span></span>

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

<span data-ttu-id="b9c5e-126">は、次のように解釈されます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-126">Will be roughly translated into the following:</span></span>

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

<span data-ttu-id="b9c5e-127">注: これは、有効C#なコードではなく、実装によって実行される内容の概数です。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-127">Note: this is not legal C# code but instead just an approximation of what the implementation does.</span></span> 

<span data-ttu-id="b9c5e-128">`null` 検証パラメーターの構文は、ラムダパラメーターリストでも有効です。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-128">The `null` validation parameter syntax will also be valid on lambda parameter lists.</span></span> <span data-ttu-id="b9c5e-129">これは、1つのパラメーターの構文で、parens が不足している場合でも有効です。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-129">This is valid even in the single parameter syntax that lacks parens.</span></span>

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

<span data-ttu-id="b9c5e-130">構文は、反復子メソッドのパラメーターでも有効です。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-130">The syntax is also valid on parameters to iterator methods.</span></span> <span data-ttu-id="b9c5e-131">反復子内の他のコードとは異なり、基になる列挙子が開始されたときではなく、反復子メソッドが呼び出されると `null` 検証が行われます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-131">Unlike other code in the iterator the `null` validation will occur when the iterator method is invoked, not when the underlying enumerator is walked.</span></span> <span data-ttu-id="b9c5e-132">これは、従来の反復子または `async` 反復子の場合に当てはまります。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-132">This is true for traditional or `async` iterators.</span></span>

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

<span data-ttu-id="b9c5e-133">`!` 演算子は、メソッド本体が関連付けられているパラメーターリストに対してのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-133">The `!` operator can only be used for parameter lists which have an associated method body.</span></span> <span data-ttu-id="b9c5e-134">これは、`abstract` メソッド、`interface`、`delegate`、または `partial` メソッドの定義では使用できないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-134">This means it cannot be used in an `abstract` method, `interface`, `delegate` or `partial` method definition.</span></span>

### <a name="extending-is-null"></a><span data-ttu-id="b9c5e-135">拡張が null</span><span class="sxs-lookup"><span data-stu-id="b9c5e-135">Extending is null</span></span>
<span data-ttu-id="b9c5e-136">式 `is null` が有効な型は、制約のない型パラメーターを含むように拡張されます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-136">The types for which the expression `is null` is valid will be extended to include unconstrained type parameters.</span></span> <span data-ttu-id="b9c5e-137">これにより、`null` チェックが有効になっているすべての型に対して `null` をチェックする目的を満たすことができます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-137">This will allow it to fill the intent of checking for `null` on all types which a `null` check is valid.</span></span> <span data-ttu-id="b9c5e-138">特に、値型として知られていない型です。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-138">Specifically that is types which are not definitely known to be value types.</span></span> <span data-ttu-id="b9c5e-139">たとえば、`struct` に制約される型パラメーターは、この構文では使用できません。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-139">For example Type parameters which are constrained to `struct` cannot be used with this syntax.</span></span>

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

<span data-ttu-id="b9c5e-140">型パラメーターに対する `is null` の動作は、今日 `== null` と同じになります。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-140">The behavior of `is null` on a type parameter will be the same as `== null` today.</span></span> <span data-ttu-id="b9c5e-141">型パラメーターが値型としてインスタンス化されている場合、コードは `false`として評価されます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-141">In the cases where the type parameter is instantiated as a value type the code will be evaluated as `false`.</span></span> <span data-ttu-id="b9c5e-142">参照型の場合、コードは適切な `is null` チェックを行います。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-142">For cases where it is a reference type the code will do a proper `is null` check.</span></span>

### <a name="intersection-with-nullable-reference-types"></a><span data-ttu-id="b9c5e-143">Null 値が許容される参照型との積集合</span><span class="sxs-lookup"><span data-stu-id="b9c5e-143">Intersection with Nullable Reference Types</span></span>
<span data-ttu-id="b9c5e-144">名前に `!` 演算子が適用されているすべてのパラメーターは、null 許容の状態が `null`ではなくなります。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-144">Any parameter which has a `!` operator applied to it's name will start with the nullable state being not `null`.</span></span> <span data-ttu-id="b9c5e-145">これは、パラメーター自体の型が `null`可能性がある場合でも同様です。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-145">This is true even if the type of the parameter itself is potentially `null`.</span></span> <span data-ttu-id="b9c5e-146">これは、`string?`のような明示的な null 許容型、または制約のない型パラメーターを使用して発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-146">That can occur with an explicitly nullable type, such as say `string?`, or with an unconstrained type parameter.</span></span> 

<span data-ttu-id="b9c5e-147">パラメーターの `!` 構文をパラメーターの明示的な null 許容型と組み合わせると、コンパイラによって警告が発行されます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-147">When a `!` syntax on parameters is combined with an explicitly nullable type on the parameter then a warning will be issued by the compiler:</span></span>

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a><span data-ttu-id="b9c5e-148">懸案事項を開く</span><span class="sxs-lookup"><span data-stu-id="b9c5e-148">Open Issues</span></span>
<span data-ttu-id="b9c5e-149">なし</span><span class="sxs-lookup"><span data-stu-id="b9c5e-149">None</span></span>

## <a name="considerations"></a><span data-ttu-id="b9c5e-150">考慮事項</span><span class="sxs-lookup"><span data-stu-id="b9c5e-150">Considerations</span></span>

### <a name="constructors"></a><span data-ttu-id="b9c5e-151">コンストラクター</span><span class="sxs-lookup"><span data-stu-id="b9c5e-151">Constructors</span></span>
<span data-ttu-id="b9c5e-152">コンストラクターのコード生成では、現在、標準的な `null` の検証から、`null` 検証パラメーターの構文 (`!`) への移行時に、監視が可能で、監視可能な動作が変更されています。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-152">The code generation for constructors means there is a small, but observable, behavior change when moving from standard `null` validation today and the `null` validation parameter syntax (`!`).</span></span> <span data-ttu-id="b9c5e-153">標準検証の `null` のチェックインは、フィールド初期化子と `base` または `this` の両方の呼び出しの後に発生します。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-153">The `null` check in standard validation occurs after both field initializers and any `base` or `this` calls.</span></span> <span data-ttu-id="b9c5e-154">つまり、開発者は、`null` 検証の100% を新しい構文に必ずしも移行することはできません。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-154">This means a developer can't necessarily migrate 100% of their `null` validation to the new syntax.</span></span> <span data-ttu-id="b9c5e-155">コンストラクターでは、少なくとも検査が必要です。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-155">Constructors at least require some inspection.</span></span>

<span data-ttu-id="b9c5e-156">議論した後で、非常に多くの導入に関する問題が発生する可能性が非常に高いと判断しました。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-156">After discussion though it was decided that this is very unlikely to cause any significant adoption issues.</span></span> <span data-ttu-id="b9c5e-157">コンストラクター内のロジックが実行される前に `null` チェックが実行されると、より論理的なものになります。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-157">It's more logical that the `null` check run before any logic in the constructor does.</span></span> <span data-ttu-id="b9c5e-158">重大な互換性の問題が検出された場合は、を再実行できます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-158">Can revisit if significant compat issues are discovered.</span></span>

### <a name="warning-when-mixing--and-"></a><span data-ttu-id="b9c5e-159">混合時の警告</span><span class="sxs-lookup"><span data-stu-id="b9c5e-159">Warning when mixing ?</span></span> <span data-ttu-id="b9c5e-160">そして！</span><span class="sxs-lookup"><span data-stu-id="b9c5e-160">and !</span></span>
<span data-ttu-id="b9c5e-161">Null 許容型に明示的に型指定されたパラメーターに `!` 構文が適用されている場合に、警告を発行するかどうかについては、時間がかかることがありました。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-161">There was a lengthy discussion on whether or not a warning should be issued when the `!` syntax is applied to a parameter which is explicitly typed to a nullable type.</span></span> <span data-ttu-id="b9c5e-162">この画面では、開発者による無意味宣言のように見えますが、型階層で開発者がこのような状況に強制的に適用されることもあります。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-162">On the surface it seems like a nonsensical declaration by the developer but there are cases where type hierarchies could force developers into such a situation.</span></span> 

<span data-ttu-id="b9c5e-163">一連のアセンブリに対して次のクラス階層を考えてみます (すべてが `null` チェックが有効になっていることを前提としています)。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-163">Consider the following class hierarchy across a series of assemblies (assuming all are compiled with `null` checking enabled):</span></span>

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

<span data-ttu-id="b9c5e-164">ここで `C3` の作成者は、パラメーター `o`に検証 `null` を追加することに決定しました。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-164">Here the author of `C3` decided to add `null` validation to the parameter `o`.</span></span> <span data-ttu-id="b9c5e-165">これは、機能がどのように使用されているかを完全に示すものです。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-165">This is completely in line with how the feature is intended to be used.</span></span>

<span data-ttu-id="b9c5e-166">後で、Assembly2 の作成者が次のオーバーライドを追加することにしたとします。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-166">Now imagine at a later date the author of Assembly2 decides to add the following override:</span></span>

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

<span data-ttu-id="b9c5e-167">これは、入力位置に対してコントラクトの柔軟性を高めるために有効であるため、null 許容型参照型によって許可されます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-167">This is allowed by nullable reference types as it's legal to make the contract more flexible for input positions.</span></span> <span data-ttu-id="b9c5e-168">NRT の一般的な機能を使用すると、パラメーターまたは戻り値の null 値の許容範囲に対して、妥当な co/反変性を実現できます</span><span class="sxs-lookup"><span data-stu-id="b9c5e-168">The NRT feature in general allows for reasonable co/contravariance on parameter / return nullability.</span></span> <span data-ttu-id="b9c5e-169">ただし、この言語では、元の宣言ではなく、最も限定的なオーバーライドに基づいて、共同/反変性がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-169">However the language does the co/contravariance checking based on the most specific override, not the original declaration.</span></span> <span data-ttu-id="b9c5e-170">つまり、Assembly3 の作成者には、一致しない `o` の種類に関する警告が表示されます。これを回避するには、シグネチャを次のように変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-170">This means the author of Assembly3 will get a warning about the type of `o` not matching and will need to change the signature to the following to eliminate it:</span></span> 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

<span data-ttu-id="b9c5e-171">この時点で、Assembly3 の作成者にはいくつかの選択肢があります。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-171">At this point the author of Assembly3 has a few choices:</span></span>

- <span data-ttu-id="b9c5e-172">`object?` と `object` の不一致に関する警告を受け入れたり非表示にしたりすることができます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-172">They can accept / suppress the warning about `object?` and `object` mismatch.</span></span>
- <span data-ttu-id="b9c5e-173">`object?` と `!` の不一致に関する警告を受け入れたり非表示にしたりすることができます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-173">They can accept / suppress the warning about `object?` and `!` mismatch.</span></span>
- <span data-ttu-id="b9c5e-174">`null` 検証チェックを削除する (`!` を削除して、明示的なチェックを行う) だけです。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-174">They can just remove the `null` validation check (delete `!` and do explicit checking)</span></span>

<span data-ttu-id="b9c5e-175">これは実際のシナリオですが、ここでは警告を使用して先に進むことを考えています。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-175">This is a real scenario but for now the idea is to move forward with the warning.</span></span> <span data-ttu-id="b9c5e-176">警告が予想よりも頻繁に発生する場合は、後で削除することができます (逆は true ではありません)。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-176">If it turns out the warning happens more frequently than we anticipate then we can remove it later (the reverse is not true).</span></span>

### <a name="implicit-property-setter-arguments"></a><span data-ttu-id="b9c5e-177">暗黙的なプロパティ setter 引数</span><span class="sxs-lookup"><span data-stu-id="b9c5e-177">Implicit property setter arguments</span></span>
<span data-ttu-id="b9c5e-178">パラメーターの `value` 引数は暗黙的であり、どのパラメーターリストにも表示されません。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-178">The `value` argument of a parameter is implicit and does not appear in any parameter list.</span></span> <span data-ttu-id="b9c5e-179">つまり、この機能のターゲットにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-179">That means it cannot be a target of this feature.</span></span> <span data-ttu-id="b9c5e-180">プロパティ setter 構文を拡張して、`!` 演算子を適用できるようにするパラメーターリストを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-180">The property setter syntax could be extended to include a parameter list to allow the `!` operator to be applied.</span></span> <span data-ttu-id="b9c5e-181">しかし、この機能を利用すると、`null` 検証がより簡単になります。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-181">But that cuts against the idea of this feature making `null` validation simpler.</span></span> <span data-ttu-id="b9c5e-182">このように、暗黙的な `value` 引数は、この機能では機能しません。</span><span class="sxs-lookup"><span data-stu-id="b9c5e-182">As such the implicit `value` argument just won't work with this feature.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="b9c5e-183">将来の注意事項</span><span class="sxs-lookup"><span data-stu-id="b9c5e-183">Future Considerations</span></span>
<span data-ttu-id="b9c5e-184">なし</span><span class="sxs-lookup"><span data-stu-id="b9c5e-184">None</span></span>
