---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "79483962"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a><span data-ttu-id="e5e4e-101">参照のような型の安全性のコンパイル時間の強制</span><span class="sxs-lookup"><span data-stu-id="e5e4e-101">Compile time enforcement of safety for ref-like types</span></span>

## <a name="introduction"></a><span data-ttu-id="e5e4e-102">はじめに</span><span class="sxs-lookup"><span data-stu-id="e5e4e-102">Introduction</span></span>

<span data-ttu-id="e5e4e-103">`Span<T>` や `ReadOnlySpan<T>` などの型を扱うときに安全性規則を追加する主な理由は、そのような型を実行スタックに限定する必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-103">The main reason for the additional safety rules when dealing with types like `Span<T>` and `ReadOnlySpan<T>` is that such types must be confined to the execution stack.</span></span>
 
<span data-ttu-id="e5e4e-104">`Span<T>` と同様の型がスタックのみの型である必要がある理由は2つあります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-104">There are two reasons why `Span<T>` and similar types must be a stack-only types.</span></span>

1. <span data-ttu-id="e5e4e-105">`Span<T>` は、参照と範囲 `(ref T data, int length)`を含む構造体に意味があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-105">`Span<T>` is semantically a struct containing a reference and a range - `(ref T data, int length)`.</span></span> <span data-ttu-id="e5e4e-106">実際の実装に関係なく、このような構造体への書き込みはアトミックではありません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-106">Regardless of actual implementation, writes to such struct would not be atomic.</span></span> <span data-ttu-id="e5e4e-107">このような構造体の同時 "ティアリング" によって、`data`が一致しなくなり、範囲外のアクセスやタイプセーフ `length` の違反が発生する可能性があります。最終的には、"安全な" コードで GC ヒープが破損する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-107">Concurrent "tearing" of such struct would lead to the possibility of `length` not matching the `data`, causing out-of-range accesses and type-safety violations, which ultimately could result in GC heap corruption in seemingly "safe" code.</span></span>
2. <span data-ttu-id="e5e4e-108">`Span<T>` の一部の実装では、そのフィールドのいずれかにマネージポインターが文字どおり含まれています。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-108">Some implementations of `Span<T>` literally contain a managed pointer in one of its fields.</span></span> <span data-ttu-id="e5e4e-109">マネージポインターは、ヒープオブジェクトのフィールドとしてはサポートされていません。また、GC ヒープにマネージポインターを配置するために管理するコードは、通常、JIT 時にクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-109">Managed pointers are not supported as fields of heap objects and code that manages to put a managed pointer on the GC heap typically crashes at JIT time.</span></span>

<span data-ttu-id="e5e4e-110">`Span<T>` のインスタンスが実行スタックにのみ存在するように制約されている場合、上記の問題はすべて緩和されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-110">All the above problems would be alleviated if instances of `Span<T>` are constrained to exist only on the execution stack.</span></span> 

<span data-ttu-id="e5e4e-111">合成によって追加の問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-111">An additional problem arises due to composition.</span></span> <span data-ttu-id="e5e4e-112">通常は、`Span<T>` と `ReadOnlySpan<T>` インスタンスを埋め込む複雑なデータ型を作成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-112">It would be generally desirable to build more complex data types that would embed `Span<T>` and `ReadOnlySpan<T>` instances.</span></span> <span data-ttu-id="e5e4e-113">このような複合型は構造体である必要があり、`Span<T>`のすべての危険と要件を共有します。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-113">Such composite types would have to be structs and would share all the hazards and requirements of `Span<T>`.</span></span> <span data-ttu-id="e5e4e-114">そのため、ここで説明する安全性規則は、 **_参照のような型_** の範囲全体に適用できるものとして表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-114">As a result the safety rules described here should be viewed as applicable to the whole range of **_ref-like types_**.</span></span>

<span data-ttu-id="e5e4e-115">[ドラフト言語仕様](#draft-language-specification)は、ref に似た型の値がスタックでのみ発生するようにすることを目的としています。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-115">The [draft language specification](#draft-language-specification) is intended to ensure that values of a ref-like type occurs only on the stack.</span></span>

## <a name="generalized-ref-like-types-in-source-code"></a><span data-ttu-id="e5e4e-116">ソースコード内の一般化された `ref-like` 型</span><span class="sxs-lookup"><span data-stu-id="e5e4e-116">Generalized `ref-like` types in source code</span></span>

<span data-ttu-id="e5e4e-117">`ref-like` 構造体は、`ref` 修飾子を使用してソースコードで明示的にマークされます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-117">`ref-like` structs are explicitly marked in the source code using `ref` modifier:</span></span>

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

<span data-ttu-id="e5e4e-118">構造体を ref like として指定すると、構造体には、参照に似たインスタンスフィールドを設定できます。また、構造体に対しても、ref に似た型のすべての要件が適用されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-118">Designating a struct as ref-like will allow the struct to have ref-like instance fields and will also make all the requirements of ref-like types applicable to the struct.</span></span> 

## <a name="metadata-representation-or-ref-like-structs"></a><span data-ttu-id="e5e4e-119">メタデータ表現または参照のような構造体</span><span class="sxs-lookup"><span data-stu-id="e5e4e-119">Metadata representation or ref-like structs</span></span>

<span data-ttu-id="e5e4e-120">Ref に似た構造体は、 **System.runtime.compilerservices 属性**属性でマークされます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-120">Ref-like structs will be marked with **System.Runtime.CompilerServices.IsRefLikeAttribute** attribute.</span></span>

<span data-ttu-id="e5e4e-121">属性は、`mscorlib`などの共通の基本ライブラリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-121">The attribute will be added to common base libraries such as `mscorlib`.</span></span> <span data-ttu-id="e5e4e-122">属性が使用できない場合、コンパイラは、`IsReadOnlyAttribute`などの他の埋め込み要求属性と同様に内部的なを生成します。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-122">In a case if the attribute is not available, compiler will generate an internal one similarly to other embedded-on-demand attributes such as `IsReadOnlyAttribute`.</span></span>

<span data-ttu-id="e5e4e-123">安全性規則 (この機能が実装されている前のコンパイラを含むC# ) では、ref に似た構造体をコンパイラで使用しないようにするために、追加のメジャーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-123">An additional measure will be taken to prevent the use of ref-like structs in compilers not familiar with the safety rules (this includes C# compilers prior to the one in which this feature is implemented).</span></span> 

<span data-ttu-id="e5e4e-124">サービスを使用せずに古いコンパイラで動作する、その他の適切な代替手段がない場合、既知の文字列を持つ `Obsolete` の属性が、すべての参照に似た構造体に追加されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-124">Having no other good alternatives that work in old compilers without servicing, an `Obsolete` attribute with a known string will be added to all ref-like structs.</span></span> <span data-ttu-id="e5e4e-125">Ref に似た型の使用方法を認識しているコンパイラは、この特定の形式の `Obsolete`を無視します。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-125">Compilers that know how to use ref-like types will ignore this particular form of `Obsolete`.</span></span>

<span data-ttu-id="e5e4e-126">一般的なメタデータ表現は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-126">A typical metadata representation:</span></span>

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

<span data-ttu-id="e5e4e-127">注: 以前のコンパイラで参照のような型を使用すると、100% が失敗するようにするための目標ではありません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-127">NOTE: it is not the goal to make it so that any use of ref-like types on old compilers fails 100%.</span></span> <span data-ttu-id="e5e4e-128">これは、実現するのは困難であり、厳密には必要ありません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-128">That is hard to achieve and is not strictly necessary.</span></span> <span data-ttu-id="e5e4e-129">たとえば、常に、動的なコードを使用して `Obsolete` を回避する方法があります。たとえば、リフレクションを使用して、参照に似た型の配列を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-129">For example there would always be a way to get around the `Obsolete` using dynamic code or, for example, creating an array of ref-like types through reflection.</span></span>

<span data-ttu-id="e5e4e-130">特に、ユーザーが `Obsolete` または `Deprecated` の属性を参照のような型に実際に配置する場合、`Obsolete` 属性を複数回適用することはできないため、定義済みの属性を生成しない以外に選択肢はありません。「」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-130">In particular, if user wants to actually put an `Obsolete` or `Deprecated` attribute on a ref-like type, we will have no choice other than not emitting the predefined one since `Obsolete` attribute cannot be applied more than once..</span></span>  

## <a name="examples"></a><span data-ttu-id="e5e4e-131">例 :</span><span class="sxs-lookup"><span data-stu-id="e5e4e-131">Examples:</span></span>

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a><span data-ttu-id="e5e4e-132">ドラフト言語の仕様</span><span class="sxs-lookup"><span data-stu-id="e5e4e-132">Draft language specification</span></span>

<span data-ttu-id="e5e4e-133">以下では、これらの型の値がスタックでのみ確実に発生するようにするために、参照のような型 (`ref struct`s) の安全規則のセットについて説明します。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-133">Below we describe a set of safety rules for ref-like types (`ref struct`s) to ensure that values of these types occur only on the stack.</span></span> <span data-ttu-id="e5e4e-134">参照渡しでローカルに渡すことができない場合は、より単純な安全性規則のセットを使用できます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-134">A different, simpler set of safety rules would be possible if locals cannot be passed by reference.</span></span> <span data-ttu-id="e5e4e-135">この仕様では、ref ローカルの安全な再割り当ても許可されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-135">This specification would also permit the safe reassignment of ref locals.</span></span>

### <a name="overview"></a><span data-ttu-id="e5e4e-136">概要</span><span class="sxs-lookup"><span data-stu-id="e5e4e-136">Overview</span></span>

<span data-ttu-id="e5e4e-137">コンパイル時には、式のエスケープが許可されているスコープの概念である "安全な" エスケープ "に関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-137">We associate with each expression at compile-time the concept of what scope that expression is permitted to escape to, "safe-to-escape".</span></span> <span data-ttu-id="e5e4e-138">同様に、各左辺値については、参照されているスコープの概念は "ref-safe-escape" になります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-138">Similarly, for each lvalue we maintain a concept of what scope a reference to it is permitted to escape to, "ref-safe-to-escape".</span></span> <span data-ttu-id="e5e4e-139">指定された左辺値式では、これらは異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-139">For a given lvalue expression, these may be different.</span></span>

<span data-ttu-id="e5e4e-140">これらは、ref ローカル機能の "安全に戻る" に似ていますが、さらにきめ細かです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-140">These are analogous to the "safe to return" of the ref locals feature, but it is more fine-grained.</span></span> <span data-ttu-id="e5e4e-141">式の "セーフツーリターン" によって、外側のメソッドが完全にエスケープされるかどうかだけが記録されます。これは、そのスコープがエスケープされる可能性がある (スコープはエスケープされない場合があります)。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-141">Where the "safe-to-return" of an expression records only whether (or not) it may escape the enclosing method as a whole, the safe-to-escape records which scope it may escape to (which scope it may not escape beyond).</span></span> <span data-ttu-id="e5e4e-142">基本的な安全性メカニズムは、次のように適用されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-142">The basic safety mechanism is enforced as follows.</span></span> <span data-ttu-id="e5e4e-143">安全-エスケープスコープ S1 を持つ式 E1 から (左辺値) 式 E2 を使用して、安全な値を持つ S2 に割り当てられている場合、S2 が S1 よりも広いスコープの場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-143">Given an assignment from an expression E1 with a safe-to-escape scope S1, to an (lvalue) expression E2 with safe-to-escape scope S2, it is an error if S2 is a wider scope than S1.</span></span> <span data-ttu-id="e5e4e-144">構築上、2つのスコープ S1 と S2 は、式を囲むスコープから常に安全に戻ることができるため、入れ子関係にあります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-144">By construction, the two scopes S1 and S2 are in a nesting relationship, because a legal expression is always safe-to-return from some scope enclosing the expression.</span></span>

<span data-ttu-id="e5e4e-145">そのためには、分析のために、メソッドの外部のスコープとメソッドの最上位スコープの2つのスコープのみをサポートするために、十分な時間を確保します。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-145">For the time being it is sufficient, for the purpose of the analysis, to support just two scopes - external to the method, and top-level scope of the method.</span></span> <span data-ttu-id="e5e4e-146">これは、内部スコープを持つ参照のような値を作成できず、ref ローカルが再割り当てをサポートしていないためです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-146">That is because ref-like values with inner scopes cannot be created and ref locals do not support re-assignment.</span></span> <span data-ttu-id="e5e4e-147">ただし、ルールでは、3つ以上のスコープレベルをサポートできます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-147">The rules, however, can support more than two scope levels.</span></span>

<span data-ttu-id="e5e4e-148">式の*信頼できる*状態の状態を計算するための正確な規則と、式の正規表現を制御する規則に従います。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-148">The precise rules for computing the *safe-to-return* status of an expression, and the rules governing the legality of expressions, follow.</span></span>

### <a name="ref-safe-to-escape"></a><span data-ttu-id="e5e4e-149">参照セーフ-エスケープ</span><span class="sxs-lookup"><span data-stu-id="e5e4e-149">ref-safe-to-escape</span></span>

<span data-ttu-id="e5e4e-150">*参照セーフツーエスケープ*は、左辺値式を囲む範囲であり、左辺値がエスケープされるまでの参照が安全です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-150">The *ref-safe-to-escape* is a scope, enclosing an lvalue expression, to which it is safe for a ref to the lvalue to escape to.</span></span> <span data-ttu-id="e5e4e-151">このスコープがメソッド全体である場合は、左辺値への参照がメソッドから*安全に戻る*ことができます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-151">If that scope is the entire method, we say that a ref to the lvalue is *safe to return* from the method.</span></span>

### <a name="safe-to-escape"></a><span data-ttu-id="e5e4e-152">安全-エスケープ</span><span class="sxs-lookup"><span data-stu-id="e5e4e-152">safe-to-escape</span></span>

<span data-ttu-id="e5e4e-153">*セーフツーエスケープ*は、式を囲むスコープで、値がエスケープされても安全です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-153">The *safe-to-escape* is a scope, enclosing an expression, to which it is safe for the value to escape to.</span></span> <span data-ttu-id="e5e4e-154">このスコープがメソッド全体である場合は、値がメソッドから返されることが*安全*であると言います。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-154">If that scope is the entire method, we say that a the value is *safe to return* from the method.</span></span>

<span data-ttu-id="e5e4e-155">型が `ref struct` 型でない式は、外側のメソッド全体から*安全に戻る*ことができます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-155">An expression whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span> <span data-ttu-id="e5e4e-156">それ以外の場合は、以下の規則を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-156">Otherwise we refer to the rules below.</span></span>

#### <a name="parameters"></a><span data-ttu-id="e5e4e-157">パラメーター</span><span class="sxs-lookup"><span data-stu-id="e5e4e-157">Parameters</span></span>

<span data-ttu-id="e5e4e-158">仮パラメーターを指定する左辺値は、次のように、参照によって (参照によって) 参照が*安全*です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-158">An lvalue designating a formal parameter is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="e5e4e-159">パラメーターが `ref`、`out`、または `in` パラメーターの場合は、メソッド全体 (たとえば、`return ref` ステートメント) からの*ref セーフツーエスケープ*です。それ以外</span><span class="sxs-lookup"><span data-stu-id="e5e4e-159">If the parameter is a `ref`, `out`, or `in` parameter, it is *ref-safe-to-escape* from the entire method (e.g. by a `return ref` statement); otherwise</span></span>
- <span data-ttu-id="e5e4e-160">パラメーターが構造体型の `this` パラメーターである場合、そのパラメーターは、メソッドの最上位スコープ (ただし、メソッド自体からではなく) に対して、*参照セーフからエスケープ*されます。[サンプル](#struct-this-escape)</span><span class="sxs-lookup"><span data-stu-id="e5e4e-160">If the parameter is the `this` parameter of a struct type, it is *ref-safe-to-escape* to the top-level scope of the method (but not from the entire method itself); [Sample](#struct-this-escape)</span></span>
- <span data-ttu-id="e5e4e-161">それ以外の場合、パラメーターは値パラメーターであり、(メソッド自体からではなく) メソッドの最上位のスコープに対して*参照セーフからエスケープ*されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-161">Otherwise the parameter is a value parameter, and it is *ref-safe-to-escape* to the top-level scope of the method (but not from the method itself).</span></span>

<span data-ttu-id="e5e4e-162">仮パラメーターの使用を指定する右辺値である式は、メソッド全体から (値によって) (たとえば、`return` ステートメントによって) 完全*にエスケープ*されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-162">An expression that is an rvalue designating the use of a formal parameter is *safe-to-escape* (by value) from the entire method (e.g. by a `return` statement).</span></span> <span data-ttu-id="e5e4e-163">これは `this` パラメーターにも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-163">This applies to the `this` parameter as well.</span></span>

#### <a name="locals"></a><span data-ttu-id="e5e4e-164">ローカル</span><span class="sxs-lookup"><span data-stu-id="e5e4e-164">Locals</span></span>

<span data-ttu-id="e5e4e-165">ローカル変数を指定する左辺値は、次のように、参照によって (参照によって) 参照が*安全*です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-165">An lvalue designating a local variable is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="e5e4e-166">変数が `ref` 変数である場合、その変数の*ref*と escape は、初期化式の*ref セーフツーエスケープ*から取得されます。それ以外</span><span class="sxs-lookup"><span data-stu-id="e5e4e-166">If the variable is a `ref` variable, then its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of its initializing expression; otherwise</span></span>
- <span data-ttu-id="e5e4e-167">変数は、宣言されたスコープを*参照セーフでエスケープ*します。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-167">The variable is *ref-safe-to-escape* the scope in which it was declared.</span></span>

<span data-ttu-id="e5e4e-168">ローカル変数の使用を指定する右辺値である式は、次のように*安全にエスケープでき*ます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-168">An expression that is an rvalue designating the use of a local variable is *safe-to-escape* (by value) as follows:</span></span>
- <span data-ttu-id="e5e4e-169">ただし、上記の一般的なルールでは、型が `ref struct` 型ではないローカルは、外側のメソッド全体から*安全に戻り*ます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-169">But the general rule above, a local whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="e5e4e-170">変数が `foreach` ループの反復変数である場合、変数の*セーフツーエスケープ*のスコープは、`foreach` ループの式の*安全なエスケープ*と同じになります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-170">If the variable is an iteration variable of a `foreach` loop, then the variable's *safe-to-escape* scope is the same as the *safe-to-escape* of the `foreach` loop's expression.</span></span>
- <span data-ttu-id="e5e4e-171">`ref struct` 型のローカルのと、宣言の時点で初期化されていないは、外側のメソッド全体からは*安全に戻り*ます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-171">A local of `ref struct` type and uninitialized at the point of declaration is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="e5e4e-172">それ以外の場合、変数の型は `ref struct` 型であり、変数の宣言には初期化子が必要です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-172">Otherwise the variable's type is a `ref struct` type, and the variable's declaration requires an initializer.</span></span> <span data-ttu-id="e5e4e-173">変数の*セーフツーエスケープ*のスコープは、その初期化子の*安全なエスケープ*と同じです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-173">The variable's *safe-to-escape* scope is the same as the *safe-to-escape* of its initializer.</span></span>

#### <a name="field-reference"></a><span data-ttu-id="e5e4e-174">フィールド参照</span><span class="sxs-lookup"><span data-stu-id="e5e4e-174">Field reference</span></span>

<span data-ttu-id="e5e4e-175">フィールドへの参照を指定する左辺値 (`e.F`) は、次のように、参照によって*安全にエスケープ*されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-175">An lvalue designating a reference to a field, `e.F`, is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="e5e4e-176">`e` が参照型の場合は、メソッド全体からの*ref セーフツーエスケープ*です。それ以外</span><span class="sxs-lookup"><span data-stu-id="e5e4e-176">If `e` is of a reference type, it is *ref-safe-to-escape* from the entire method; otherwise</span></span>
- <span data-ttu-id="e5e4e-177">`e` が値型の場合、その*参照セーフからエスケープ*は、`e`の*ref セーフツーエスケープ*から取得されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-177">If `e` is of a value type, its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of `e`.</span></span>

<span data-ttu-id="e5e4e-178">フィールドへの参照を指定する右辺値 (`e.F`) には、`e`の*安全なエスケープ*と同じ、*安全にエスケープ*できるスコープがあります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-178">An rvalue designating a reference to a field, `e.F`, has a *safe-to-escape* scope that is the same as the *safe-to-escape* of `e`.</span></span>

#### <a name="operators-including-"></a><span data-ttu-id="e5e4e-179">`?:` を含む演算子</span><span class="sxs-lookup"><span data-stu-id="e5e4e-179">Operators including `?:`</span></span>

<span data-ttu-id="e5e4e-180">ユーザー定義の演算子のアプリケーションは、メソッドの呼び出しとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-180">The application of a user-defined operator is treated as a method invocation.</span></span>

<span data-ttu-id="e5e4e-181">`e1 + e2` や `c ? e1 : e2`などの右辺値を生成する演算子については、演算子のオペランドの*セーフツーエスケープ*の範囲内で最も狭いスコープが結果の*安全*な範囲になります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-181">For an operator that yields an rvalue, such as `e1 + e2` or `c ? e1 : e2`, the *safe-to-escape* of the result is the narrowest scope among the *safe-to-escape* of the operands of the operator.</span></span>  <span data-ttu-id="e5e4e-182">その結果、`+e`などの右辺値を生成する単項演算子の場合、結果の*安全に*エスケープされるのは、オペランドを*安全にエスケープ*することです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-182">As a consequence, for a unary operator that yields an rvalue, such as `+e`, the *safe-to-escape* of the result is the *safe-to-escape* of the operand.</span></span>

<span data-ttu-id="e5e4e-183">`c ? ref e1 : ref e2` などの左辺値を生成する演算子の場合</span><span class="sxs-lookup"><span data-stu-id="e5e4e-183">For an operator that yields an lvalue, such as `c ? ref e1 : ref e2`</span></span>
- <span data-ttu-id="e5e4e-184">結果の*ref セーフツーエスケープ*は、演算子のオペランドの*ref セーフツーエスケープ*の範囲の中で最も狭いスコープです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-184">the *ref-safe-to-escape* of the result is the narrowest scope among the *ref-safe-to-escape* of the operands of the operator.</span></span>
- <span data-ttu-id="e5e4e-185">オペランドが*安全にエスケープ*される必要があります。これは、結果として得られる左辺値の安全な*エスケープ*です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-185">the *safe-to-escape* of the operands must agree, and that is the *safe-to-escape* of the resulting lvalue.</span></span>

#### <a name="method-invocation"></a><span data-ttu-id="e5e4e-186">メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="e5e4e-186">Method invocation</span></span>

<span data-ttu-id="e5e4e-187">参照を返すメソッド呼び出しの結果として得られる左辺値は、次のスコープのうち最小のものを*参照セーフ*`e1.M(e2, ...)` ます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-187">An lvalue resulting from a ref-returning method invocation `e1.M(e2, ...)` is *ref-safe-to-escape* the smallest of the following scopes:</span></span>
- <span data-ttu-id="e5e4e-188">外側のメソッド全体。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-188">The entire enclosing method</span></span>
- <span data-ttu-id="e5e4e-189">すべての `ref` および `out` 引数式の*ref セーフツーエスケープ*(受信側を除く)</span><span class="sxs-lookup"><span data-stu-id="e5e4e-189">the *ref-safe-to-escape* of all `ref` and `out` argument expressions (excluding the receiver)</span></span>
- <span data-ttu-id="e5e4e-190">メソッドの各 `in` パラメーターに対して、左辺値、その*参照セーフ、エスケープ*、または最も近い外側のスコープである対応する式がある場合は、</span><span class="sxs-lookup"><span data-stu-id="e5e4e-190">For each `in` parameter of the method, if there is a corresponding expression that is an lvalue, its *ref-safe-to-escape*, otherwise the nearest enclosing scope</span></span>
- <span data-ttu-id="e5e4e-191">すべての引数式 (受信側を含む) の*安全なエスケープ*。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-191">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

> <span data-ttu-id="e5e4e-192">注: 次のようなコードを処理するには、最後の行頭文字が必要です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-192">Note: the last bullet is necessary to handle code such as</span></span>
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> <span data-ttu-id="e5e4e-193">or</span><span class="sxs-lookup"><span data-stu-id="e5e4e-193">or</span></span>
> ```csharp
> return ref M(sp, 0);
> ```

<span data-ttu-id="e5e4e-194">メソッド呼び出し `e1.M(e2, ...)` の結果として得られる右辺値は、次のスコープのうち最も小さい方から*安全にエスケープでき*ます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-194">An rvalue resulting from a method invocation `e1.M(e2, ...)` is *safe-to-escape* from the smallest of the following scopes:</span></span>
- <span data-ttu-id="e5e4e-195">外側のメソッド全体。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-195">The entire enclosing method</span></span>
- <span data-ttu-id="e5e4e-196">すべての引数式 (受信側を含む) の*安全なエスケープ*。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-196">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

#### <a name="an-rvalue"></a><span data-ttu-id="e5e4e-197">右辺値</span><span class="sxs-lookup"><span data-stu-id="e5e4e-197">An Rvalue</span></span>
<span data-ttu-id="e5e4e-198">右辺値は、最も近い外側のスコープから、*参照セーフからエスケープ*されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-198">An rvalue is *ref-safe-to-escape* from the nearest enclosing scope.</span></span> <span data-ttu-id="e5e4e-199">これは、`M(ref d.Length)` などの呼び出しで、`d` が `dynamic`型である場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-199">This occurs for example in an invocation such as `M(ref d.Length)` where `d` is of type `dynamic`.</span></span> <span data-ttu-id="e5e4e-200">また、`in` のパラメーターに対応する引数の処理 (および場合によっては subsumes) にも一貫性があります。 \*</span><span class="sxs-lookup"><span data-stu-id="e5e4e-200">It is also consistent with (and perhaps subsumes) our handling of arguments corresponding to `in` parameters.\*</span></span>

#### <a name="property-invocations"></a><span data-ttu-id="e5e4e-201">プロパティの呼び出し</span><span class="sxs-lookup"><span data-stu-id="e5e4e-201">Property invocations</span></span>

<span data-ttu-id="e5e4e-202">プロパティ呼び出し (`get` または `set`) は、上記の規則によって基になるメソッドのメソッド呼び出しとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-202">A property invocation (either `get` or `set`) it treated as a method invocation of the underlying method by the above rules.</span></span>

#### `stackalloc`

<span data-ttu-id="e5e4e-203">Stackalloc 式は、メソッドの最上位スコープに対して*安全にエスケープ*できる右辺値です (ただし、メソッド自体からではありません)。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-203">A stackalloc expression is an rvalue that is *safe-to-escape* to the top-level scope of the method (but not from the entire method itself).</span></span>

#### <a name="constructor-invocations"></a><span data-ttu-id="e5e4e-204">コンストラクターの呼び出し</span><span class="sxs-lookup"><span data-stu-id="e5e4e-204">Constructor invocations</span></span>

<span data-ttu-id="e5e4e-205">コンストラクターを呼び出す `new` 式は、構築される型を返すと見なされるメソッド呼び出しと同じ規則に従います。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-205">A `new` expression that invokes a constructor obeys the same rules as a method invocation that is considered to return the type being constructed.</span></span>

<span data-ttu-id="e5e4e-206">さらに、オブジェクト初期化子式のすべての引数またはオペランドの、*セーフ*ツーエスケープの最小値を超えない*よう*にします。これは、初期化子が存在する場合に再帰的に行われます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-206">In addition *safe-to-escape* is no wider than the smallest of the *safe-to-escape* of all arguments/operands of the object initializer expressions, recursively, if initializer is present.</span></span> 

#### <a name="span-constructor"></a><span data-ttu-id="e5e4e-207">Span コンストラクター</span><span class="sxs-lookup"><span data-stu-id="e5e4e-207">Span constructor</span></span>
<span data-ttu-id="e5e4e-208">この言語は、次の形式のコンストラクターを持たない `Span<T>` に依存しています。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-208">The language relies on `Span<T>` not having a constructor of the following form:</span></span>

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

<span data-ttu-id="e5e4e-209">このコンストラクターは、`ref` フィールドと区別できないフィールドとして使用される `Span<T>` を作成します。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-209">Such a constructor makes `Span<T>` which are used as fields indistinguishable from a `ref` field.</span></span> <span data-ttu-id="e5e4e-210">このドキュメントで説明する安全性規則は、または .NET で有効な構成要素C#ではない `ref` フィールドによって異なります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-210">The safety rules described in this document depend on `ref` fields not being a valid construct in C#, or .NET.</span></span>

#### <a name="default-expressions"></a><span data-ttu-id="e5e4e-211">`default` 式</span><span class="sxs-lookup"><span data-stu-id="e5e4e-211">`default` expressions</span></span>

<span data-ttu-id="e5e4e-212">`default` 式は、外側のメソッド全体から*安全にエスケープ*できます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-212">A `default` expression is *safe-to-escape* from the entire enclosing method.</span></span>

## <a name="language-constraints"></a><span data-ttu-id="e5e4e-213">言語の制約</span><span class="sxs-lookup"><span data-stu-id="e5e4e-213">Language Constraints</span></span>

<span data-ttu-id="e5e4e-214">`ref` ローカル変数と `ref struct` 型の変数が存在しないことを確認したい場合は、スタックメモリまたは現在は動作していない変数を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-214">We wish to ensure that no `ref` local variable, and no variable of `ref struct` type, refers to stack memory or variables that are no longer alive.</span></span> <span data-ttu-id="e5e4e-215">そのため、次の言語の制約があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-215">We therefore have the following language constraints:</span></span>

- <span data-ttu-id="e5e4e-216">Ref パラメーター、ref local、または `ref struct` 型のローカルパラメーターをラムダまたはローカル関数に変換することはできません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-216">Neither a ref parameter, nor a ref local, nor a parameter or local of a `ref struct` type can be lifted into a lambda or local function.</span></span>

- <span data-ttu-id="e5e4e-217">Ref パラメーターも `ref struct` 型のパラメーターも、iterator メソッドまたは `async` メソッドの引数である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-217">Neither a ref parameter nor a parameter of a `ref struct` type may be an argument on an iterator method or an `async` method.</span></span>

- <span data-ttu-id="e5e4e-218">Ref ローカルと `ref struct` 型のローカルのどちらも、`yield return` ステートメントまたは `await` 式の時点でスコープ内に存在することはできません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-218">Neither a ref local, nor a local of a `ref struct` type may be in scope at the point of a `yield return` statement or an `await` expression.</span></span>

- <span data-ttu-id="e5e4e-219">`ref struct` 型は、型引数として使用したり、タプル型の要素型として使用したりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-219">A `ref struct` type may not be used as a type argument, or as an element type in a tuple type.</span></span>

- <span data-ttu-id="e5e4e-220">`ref struct` 型は、他の `ref struct`のインスタンスフィールドとして宣言された型であることを除いて、フィールドの宣言された型にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-220">A `ref struct` type may not be the declared type of a field, except that it may be the declared type of an instance field of another `ref struct`.</span></span>

- <span data-ttu-id="e5e4e-221">`ref struct` 型を配列の要素型にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-221">A `ref struct` type may not be the element type of an array.</span></span>

- <span data-ttu-id="e5e4e-222">`ref struct` 型の値をボックス化することはできません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-222">A value of a `ref struct` type may not be boxed:</span></span>
  - <span data-ttu-id="e5e4e-223">`ref struct` 型から `object` 型、または `System.ValueType`型への変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-223">There is no conversion from a `ref struct` type to the type `object` or the type `System.ValueType`.</span></span>
  - <span data-ttu-id="e5e4e-224">`ref struct` 型は、インターフェイスを実装するように宣言することはできません</span><span class="sxs-lookup"><span data-stu-id="e5e4e-224">A `ref struct` type may not be declared to implement any interface</span></span>
  - <span data-ttu-id="e5e4e-225">`object` または `System.ValueType` で宣言されているが、`ref struct` 型でオーバーライドされていないインスタンスメソッドは、その `ref struct` 型のレシーバーで呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-225">No instance method declared in `object` or in `System.ValueType` but not overridden in a `ref struct` type may be called with a receiver of that `ref struct` type.</span></span>
  - <span data-ttu-id="e5e4e-226">メソッドをデリゲート型に変換することによって、`ref struct` 型のインスタンスメソッドをキャプチャすることはできません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-226">No instance method of a `ref struct` type may be captured by method conversion to a delegate type.</span></span>

- <span data-ttu-id="e5e4e-227">参照の再割り当て `ref e1 = ref e2`の場合、`e2` の*ref セーフツーエスケープ*は、`e1`の*ref セーフツーエスケープ*の範囲内である必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-227">For a ref reassignment `ref e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="e5e4e-228">Ref return ステートメント `return ref e1`では、`e1` の*ref セーフツーエスケープ*は、メソッド全体からの*ref セーフツーエスケープ*である必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-228">For a ref return statement `return ref e1`, the *ref-safe-to-escape* of `e1` must be *ref-safe-to-escape* from the entire method.</span></span> <span data-ttu-id="e5e4e-229">(TODO: `e1`、メソッド全体から*安全にエスケープ*する必要がある規則や、冗長であるという規則も必要です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-229">(TODO: Do we also need a rule that `e1` must be *safe-to-escape* from the entire method, or is that redundant?)</span></span>

- <span data-ttu-id="e5e4e-230">Return ステートメント `return e1`の場合、`e1` の*安全*な相互エスケープは、メソッド全体から*安全にエスケープ*する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-230">For a return statement `return e1`, the *safe-to-escape* of `e1` must be *safe-to-escape* from the entire method.</span></span>

- <span data-ttu-id="e5e4e-231">割り当て `e1 = e2`の場合、`e1` の型が `ref struct` 型である場合、`e2` の*安全なエスケープ*は、`e1`の*安全なエスケープ*と同じ範囲内である必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-231">For an assignment `e1 = e2`, if the type of `e1` is a `ref struct` type, then the *safe-to-escape* of `e2` must be at least as wide a scope as the *safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="e5e4e-232">`ref struct` 型の `ref` または `out` 引数 (レシーバーを含む) がある場合のメソッド呼び出しについては、*セーフツーエスケープ*E1 を使用した場合、引数 (受信側を含む) の方が、E1 より*も幅が*狭くなることがあります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-232">For a method invocation if there is a `ref` or `out` argument of a `ref struct` type (including the receiver), with *safe-to-escape* E1, then no argument (including the receiver) may have a narrower *safe-to-escape* than E1.</span></span> [<span data-ttu-id="e5e4e-233">サンプル</span><span class="sxs-lookup"><span data-stu-id="e5e4e-233">Sample</span></span>](#method-arguments-must-match)

- <span data-ttu-id="e5e4e-234">ローカル関数または匿名関数が、外側のスコープで宣言されている `ref struct` 型のローカルまたはパラメーターを参照していない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-234">A local function or anonymous function may not refer to a local or parameter of `ref struct` type declared in an enclosing scope.</span></span>

> <span data-ttu-id="e5e4e-235">***懸案事項を開く:*** たとえば、コード内の await 式で `ref struct` 型のスタック値をスピルする必要がある場合に、エラーが発生することを許可するルールが必要です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-235">***Open Issue:*** We need some rule that permits us to produce an error when needing to spill a stack value of a `ref struct` type at an await expression, for example in the code</span></span>
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a><span data-ttu-id="e5e4e-236">コメント</span><span class="sxs-lookup"><span data-stu-id="e5e4e-236">Explanations</span></span>
<span data-ttu-id="e5e4e-237">これらの説明とサンプルは、上記の安全性規則の多くが存在する理由を説明するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-237">These explanations and samples help explain why many of the safety rules above exist</span></span>

### <a name="method-arguments-must-match"></a><span data-ttu-id="e5e4e-238">メソッドの引数は一致しなければなりません</span><span class="sxs-lookup"><span data-stu-id="e5e4e-238">Method Arguments Must Match</span></span>
<span data-ttu-id="e5e4e-239">`out`が存在するメソッドを呼び出すときに、受信者を含む `ref struct` である `ref` パラメーターには、すべての `ref struct` が同じ有効期間を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-239">When invoking a method where there is an `out`, `ref` parameter that is a `ref struct` including the receiver then all of the `ref struct` need to have the same lifetime.</span></span> <span data-ttu-id="e5e4e-240">は、メソッドのC#シグネチャで使用可能な情報と呼び出しサイトの値の有効期間に基づいて、有効期間の安全性に関するすべての決定を行う必要があるため、この設定が必要です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-240">This is necessary because C# must make all of it's decisions around lifetime safety based on the information available in the signature of the method and the lifetime of the values at the call site.</span></span> 

<span data-ttu-id="e5e4e-241">`ref struct` されている `ref` パラメーターがある場合は、おそれがコンテンツを交換できることがあります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-241">When there are `ref` parameters that are `ref struct` then there is the possiblity they could swap around their contents.</span></span> <span data-ttu-id="e5e4e-242">そのため、呼び出しサイトでは、これらの**可能性**のあるすべてのスワップに互換性があることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-242">Hence at the call site we must ensure all of these **potential** swaps are compatible.</span></span> <span data-ttu-id="e5e4e-243">言語によって強制されなかった場合は、次のような不適切なコードを使用できます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-243">If the language didn't enforce that then it will allow for bad code like the following.</span></span>

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

<span data-ttu-id="e5e4e-244">受信側の制限は、その内容がいずれも ref セーフツーエスケープではないのに、指定された値を格納できるため、受信側の制限が必要です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-244">The restriction on the receiver is necessary because while none of its contents are ref-safe-to-escape it can store the provided values.</span></span> <span data-ttu-id="e5e4e-245">つまり、有効期間が一致しない場合は、次の方法でタイプセーフホールを作成できます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-245">This means with mismatched lifetimes you could create a type safety hole in the following way:</span></span>

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a><span data-ttu-id="e5e4e-246">Struct This Escape</span><span class="sxs-lookup"><span data-stu-id="e5e4e-246">Struct This Escape</span></span>
<span data-ttu-id="e5e4e-247">安全性規則に関しては、インスタンスメンバーの `this` 値は、メンバーのパラメーターとしてモデル化されます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-247">When it comes to span safety rules the `this` value in an instance member is modeled as a parameter to the member.</span></span> <span data-ttu-id="e5e4e-248">`struct` では、`this` の型は `ref S` 実際には `S` (という名前の `class / struct` のメンバーの場合) `class` になります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-248">Now for a `struct` the type of `this` is actually `ref S` where in a `class` it's simply `S` (for members of a `class / struct` named S).</span></span> 

<span data-ttu-id="e5e4e-249">ただし `this` には、他の `ref` パラメーターとは異なるエスケープ規則があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-249">Yet `this` has different escaping rules than other `ref` parameters.</span></span> <span data-ttu-id="e5e4e-250">具体的には、他のパラメーターは次のように、参照セーフツーエスケープではありません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-250">Specifically it is not ref-safe-to-escape while other parameters are:</span></span>

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

<span data-ttu-id="e5e4e-251">この制限の理由は、実際には `struct` メンバーの呼び出しとはあまりありません。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-251">The reason for this restriction actually has little to do with `struct` member invocation.</span></span> <span data-ttu-id="e5e4e-252">受信側が右辺値である `struct` メンバーのメンバー呼び出しに関しては、いくつかの規則を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-252">There are some rules that need to be worked out with respect to member invocation on `struct` members where the receiver is an rvalue.</span></span> <span data-ttu-id="e5e4e-253">しかし、これは非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-253">But that is very approachable.</span></span> 

<span data-ttu-id="e5e4e-254">この制限の理由は、実際にはインターフェイスの呼び出しに関するものです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-254">The reason for this restriction is actually about interface invocation.</span></span> <span data-ttu-id="e5e4e-255">具体的には、次のサンプルをコンパイルする必要があるかどうかについては、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-255">Specifically it comes down to whether or not the following sample should or should not compile;</span></span>

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

<span data-ttu-id="e5e4e-256">`T` が `struct`としてインスタンス化される場合を考えてみます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-256">Consider the case where `T` is instantiated as a `struct`.</span></span> <span data-ttu-id="e5e4e-257">`this` パラメーターが参照セーフ-エスケープの場合、`p.Get` の戻り値はスタックを指している可能性があります (具体的には、`T`のインスタンス化された型の内部のフィールドである可能性があります)。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-257">If the `this` parameter is ref-safe-to-escape then the return of `p.Get` could point to the stack (specifically it could be a field inside of the instantiated type of `T`).</span></span> <span data-ttu-id="e5e4e-258">つまり、言語では、このサンプルをコンパイルすることを許可できませんでした。これは、スタック位置に `ref` を返す可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-258">That means the language could not allow this sample to compile as it could be returning a `ref` to a stack location.</span></span> <span data-ttu-id="e5e4e-259">一方、`this` が参照セーフでエスケープされていない場合、`p.Get` はスタックを参照できないため、これを安全に返すことができます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-259">On the other hand if `this` is not ref-safe-to-escape then `p.Get` cannot refer to the stack and hence it's safe to return.</span></span> 

<span data-ttu-id="e5e4e-260">このため、`struct` 内の `this` の escapability ビリティは、実際にはインターフェイスに関するものです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-260">This is why the escapability of `this` in a `struct` is really all about interfaces.</span></span> <span data-ttu-id="e5e4e-261">この機能は正常に実行できますが、トレードオフになっています。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-261">It can absolutely be made to work but it has a trade off.</span></span> <span data-ttu-id="e5e4e-262">インターフェイスの柔軟性を高めるために、最終的に設計はダウンしました。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-262">The design eventually came down in favor of making interfaces more flexible.</span></span> 

<span data-ttu-id="e5e4e-263">ただし、今後この問題を緩和する可能性はあります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-263">There is potential for us to relax this in the future though.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="e5e4e-264">将来の注意事項</span><span class="sxs-lookup"><span data-stu-id="e5e4e-264">Future Considerations</span></span>

### <a name="length-one-spant-over-ref-values"></a><span data-ttu-id="e5e4e-265">Ref 値で >\<T の長さの1スパン</span><span class="sxs-lookup"><span data-stu-id="e5e4e-265">Length one Span\<T> over ref values</span></span>
<span data-ttu-id="e5e4e-266">現時点では有効ではありませんが、値に対して1つの `Span<T>` インスタンスを作成する方が有益な場合があります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-266">Though not legal today there are cases where creating a length one `Span<T>` instance over a value would be beneficial:</span></span>

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

<span data-ttu-id="e5e4e-267">この機能は、より長い長さの `Span<T>` インスタンスに許容されるように、[固定サイズのバッファー](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md)に対して制限を引き上げると、より説得力が高まります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-267">This feature gets more compelling if we lift the restrictions on [fixed sized buffers](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) as it would allow for `Span<T>` instances of even greater length.</span></span> 

<span data-ttu-id="e5e4e-268">このパスを下げる必要がある場合は、このような `Span<T>` インスタンスが下方向にのみ表示されるようにすることで、このような状況に対応できます。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-268">If there is ever a need to go down this path then the language could accommodate this by ensuring such `Span<T>` instances were downward facing only.</span></span> <span data-ttu-id="e5e4e-269">これは、作成されたスコープに対してのみ*安全にエスケープ*されたことです。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-269">That is they were only ever *safe-to-escape* to the scope in which they were created.</span></span> <span data-ttu-id="e5e4e-270">これにより、`ref struct`の `ref struct` の戻り値またはフィールドを使用してメソッドをエスケープする `ref` 値を、言語で考慮する必要がなくなりました。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-270">This ensure the language never had to consider a `ref` value escaping a method via a `ref struct` return or field of `ref struct`.</span></span> <span data-ttu-id="e5e4e-271">また、このような方法で `ref` パラメーターをキャプチャするように、このようなコンストラクターを認識するためにさらに変更が必要になる場合もあります。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-271">This would likely also require further changes to recognize such constructors as capturing a `ref` parameter in this way though.</span></span>
