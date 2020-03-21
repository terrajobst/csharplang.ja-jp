---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108390"
---
# <a name="function-pointers"></a><span data-ttu-id="f256d-101">関数ポインター</span><span class="sxs-lookup"><span data-stu-id="f256d-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="f256d-102">要約</span><span class="sxs-lookup"><span data-stu-id="f256d-102">Summary</span></span>

<span data-ttu-id="f256d-103">この提案には、現在のC#ところ効率的にアクセスできない IL オペコードを公開する言語構造、つまり `ldftn` および `calli`が用意されています。</span><span class="sxs-lookup"><span data-stu-id="f256d-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="f256d-104">これらの IL オペコードは、高パフォーマンスのコードでは重要であり、開発者はそれらにアクセスするための効率的な方法を必要とします。</span><span class="sxs-lookup"><span data-stu-id="f256d-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="f256d-105">目的</span><span class="sxs-lookup"><span data-stu-id="f256d-105">Motivation</span></span>

<span data-ttu-id="f256d-106">この機能の動機と背景については、次の問題で説明されています (この機能が実装される可能性があります)。</span><span class="sxs-lookup"><span data-stu-id="f256d-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="f256d-107">これは、[コンパイラの組み込み](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)に対する別の設計提案です。</span><span class="sxs-lookup"><span data-stu-id="f256d-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f256d-108">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="f256d-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="f256d-109">関数ポインター</span><span class="sxs-lookup"><span data-stu-id="f256d-109">Function pointers</span></span>

<span data-ttu-id="f256d-110">この言語では、`delegate*` 構文を使用して関数ポインターを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="f256d-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="f256d-111">完全な構文については次のセクションで詳しく説明しますが、`Func` および `Action` 型宣言で使用される構文に似ています。</span><span class="sxs-lookup"><span data-stu-id="f256d-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="f256d-112">これらの型は、ECMA-335 で説明されているように、関数ポインター型を使用して表されます。</span><span class="sxs-lookup"><span data-stu-id="f256d-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="f256d-113">つまり、`delegate*` の呼び出しでは `calli` が使用され、`delegate` の呼び出しでは、`Invoke` メソッドで `callvirt` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="f256d-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="f256d-114">構文的には、どちらのコンストラクトでも呼び出しは同じです。</span><span class="sxs-lookup"><span data-stu-id="f256d-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="f256d-115">メソッドポインターの ECMA-335 定義には、型シグネチャ (セクション 7.1) の一部として呼び出し規約が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f256d-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="f256d-116">既定の呼び出し規約は `managed`になります。</span><span class="sxs-lookup"><span data-stu-id="f256d-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="f256d-117">代替形式を指定するには、`delegate*` 構文の後に適切な修飾子を追加します。 `managed`、`cdecl`、`stdcall`、`thiscall`、または `unmanaged`です。</span><span class="sxs-lookup"><span data-stu-id="f256d-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="f256d-118">例:</span><span class="sxs-lookup"><span data-stu-id="f256d-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="f256d-119">`delegate*` 型間の変換は、呼び出し規約を含むシグネチャに基づいて行われます。</span><span class="sxs-lookup"><span data-stu-id="f256d-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

<span data-ttu-id="f256d-120">`delegate*` 型はポインター型です。これは、標準ポインター型のすべての機能と制限が含まれていることを意味します。</span><span class="sxs-lookup"><span data-stu-id="f256d-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="f256d-121">`unsafe` コンテキストでのみ有効です。</span><span class="sxs-lookup"><span data-stu-id="f256d-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="f256d-122">`delegate*` パラメーターまたは戻り値の型を含むメソッドは、`unsafe` のコンテキストからのみ呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="f256d-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="f256d-123">を `object`に変換することはできません。</span><span class="sxs-lookup"><span data-stu-id="f256d-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="f256d-124">を汎用引数として使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="f256d-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="f256d-125">`delegate*` を `void*`に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="f256d-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="f256d-126">`void*` から `delegate*`に明示的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="f256d-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="f256d-127">制限:</span><span class="sxs-lookup"><span data-stu-id="f256d-127">Restrictions:</span></span>

- <span data-ttu-id="f256d-128">カスタム属性は、`delegate*` またはその要素には適用できません。</span><span class="sxs-lookup"><span data-stu-id="f256d-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="f256d-129">`delegate*` パラメーターを `params` としてマークすることはできません</span><span class="sxs-lookup"><span data-stu-id="f256d-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="f256d-130">`delegate*` 型には、通常のポインター型のすべての制限があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="f256d-131">関数ポインターの構文</span><span class="sxs-lookup"><span data-stu-id="f256d-131">Function pointer syntax</span></span>

<span data-ttu-id="f256d-132">関数ポインターの完全な構文は、次の文法で表されます。</span><span class="sxs-lookup"><span data-stu-id="f256d-132">The full function pointer syntax is represented by the following grammar:</span></span>

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

<span data-ttu-id="f256d-133">`unmanaged` 呼び出し規約は、現在のプラットフォームでのネイティブコードの既定の呼び出し規約を表し、winapi としてエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="f256d-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="f256d-134">すべての `calling_convention`s は、`delegate*`が前にある場合のコンテキストキーワードです。</span><span class="sxs-lookup"><span data-stu-id="f256d-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a><span data-ttu-id="f256d-135">関数ポインターの変換</span><span class="sxs-lookup"><span data-stu-id="f256d-135">Function pointer conversions</span></span>

<span data-ttu-id="f256d-136">Unsafe コンテキストでは、次の暗黙的なポインター変換を含むように、使用可能な暗黙の変換 (暗黙の変換) のセットが拡張されます。</span><span class="sxs-lookup"><span data-stu-id="f256d-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="f256d-137">_既存の変換_</span><span class="sxs-lookup"><span data-stu-id="f256d-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="f256d-138">次のすべての条件に該当する場合は、ファンク_ptr\_型_`F0` を別のファンク_ptr\_型_`F1`に指定します。</span><span class="sxs-lookup"><span data-stu-id="f256d-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="f256d-139">`F0` と `F1` は同じ数のパラメーターを持ち、`F0` 内の各 `D0n` パラメーターには `ref`の対応するパラメーター `out`と同じ `in`、`D1n`、または `F1`の修飾子があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="f256d-140">各値パラメーター (`ref`、`out`、または `in` 修飾子のないパラメーター) では、`F0` のパラメーター型から `F1`の対応するパラメーター型に、id 変換、暗黙の参照変換、または暗黙的なポインター変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="f256d-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="f256d-141">`ref`、`out`、または `in` パラメーターごとに、`F0` のパラメーターの型は `F1`の対応するパラメーターの型と同じになります。</span><span class="sxs-lookup"><span data-stu-id="f256d-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="f256d-142">戻り値の型が値渡し (`ref` も `ref readonly`) である場合、`F1` の戻り値の型から `F0`の戻り値の型に、id、暗黙の参照、または暗黙的なポインターの変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="f256d-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="f256d-143">戻り値の型が参照渡し (`ref` または `ref readonly`) の場合、`F1` の戻り値の型と `ref` 修飾子は `ref` の戻り値の型と `F0`修飾子と同じになります。</span><span class="sxs-lookup"><span data-stu-id="f256d-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="f256d-144">`F0` の呼び出し規約は、`F1`の呼び出し規約と同じです。</span><span class="sxs-lookup"><span data-stu-id="f256d-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="f256d-145">ターゲットメソッドへのアドレスの許可</span><span class="sxs-lookup"><span data-stu-id="f256d-145">Allow address-of to target methods</span></span>

<span data-ttu-id="f256d-146">アドレス式の引数として、メソッドグループが許可されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="f256d-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="f256d-147">このような式の型は、ターゲットメソッドとマネージ呼び出し規約の等価のシグネチャを持つ `delegate*` になります。</span><span class="sxs-lookup"><span data-stu-id="f256d-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

<span data-ttu-id="f256d-148">Unsafe コンテキストでは、メソッド `M` は、次のすべてに該当する場合に `F` 関数ポインター型と互換性があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="f256d-149">`M` と `F` は同じ数のパラメーターを持ち、`D` 内の各パラメーターには、`out`内の対応するパラメーターと同じ `ref`、`in`、または `F`修飾子があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="f256d-150">各値パラメーター (`ref`、`out`、または `in` 修飾子のないパラメーター) では、`M` のパラメーター型から `F`の対応するパラメーター型に、id 変換、暗黙の参照変換、または暗黙的なポインター変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="f256d-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="f256d-151">`ref`、`out`、または `in` パラメーターごとに、`M` のパラメーターの型は `F`の対応するパラメーターの型と同じになります。</span><span class="sxs-lookup"><span data-stu-id="f256d-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="f256d-152">戻り値の型が値渡し (`ref` も `ref readonly`) である場合、`F` の戻り値の型から `M`の戻り値の型に、id、暗黙の参照、または暗黙的なポインターの変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="f256d-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="f256d-153">戻り値の型が参照渡し (`ref` または `ref readonly`) の場合、`F` の戻り値の型と `ref` 修飾子は `ref` の戻り値の型と `M`修飾子と同じになります。</span><span class="sxs-lookup"><span data-stu-id="f256d-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="f256d-154">`M` の呼び出し規約は、`F`の呼び出し規約と同じです。</span><span class="sxs-lookup"><span data-stu-id="f256d-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="f256d-155">`M` は静的メソッドです。</span><span class="sxs-lookup"><span data-stu-id="f256d-155">`M` is a static method.</span></span>

<span data-ttu-id="f256d-156">Unsafe コンテキストでは、次に示すように、ターゲットがメソッドグループであり、互換性のある関数ポインター `F` 型に `E` メソッドグループであるか、`F`のパラメーターの型と修飾子を使用して構築された引数リストに適用可能なメソッドが少なくとも1つ `E` 含まれている場合は、暗黙的な変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="f256d-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="f256d-157">フォーム `E(A)` のメソッド呼び出しに対応する1つのメソッド `M` が、次のように変更されました。</span><span class="sxs-lookup"><span data-stu-id="f256d-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="f256d-158">引数リスト `A` は式の一覧であり、それぞれが変数として分類され、\_の対応する_正式\_パラメーター `D`リスト_の型および修飾子 (`ref`、`out`、または `in`) を使用します。</span><span class="sxs-lookup"><span data-stu-id="f256d-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="f256d-159">候補となるメソッドは、通常の形式で適用可能なメソッドのみであり、拡張された形式では適用できません。</span><span class="sxs-lookup"><span data-stu-id="f256d-159">The candidate methods are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
   - <span data-ttu-id="f256d-160">候補となるメソッドは、静的メソッドのみです。</span><span class="sxs-lookup"><span data-stu-id="f256d-160">The candidate methods are only those methods that are static.</span></span>
- <span data-ttu-id="f256d-161">メソッド呼び出しのアルゴリズムによってエラーが発生した場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f256d-161">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="f256d-162">それ以外の場合、`F` と同じ数のパラメーターを持ち、変換が存在していると見なされる `M`、1つの最適なメソッドが生成されます。</span><span class="sxs-lookup"><span data-stu-id="f256d-162">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="f256d-163">選択されたメソッド `M` は、関数ポインター型 `F`と互換性がある (前述のとおり) 必要があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-163">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="f256d-164">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f256d-164">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="f256d-165">変換の結果は `F`型の関数ポインターになります。</span><span class="sxs-lookup"><span data-stu-id="f256d-165">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="f256d-166">`E`に `M` 静的メソッドが1つしかない場合は、ターゲットがメソッドグループ `void*` `E` であるアドレス式から暗黙的な変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="f256d-166">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="f256d-167">1つの静的メソッドがある場合は、`E` の1つの最適なメソッドが `M`ます。</span><span class="sxs-lookup"><span data-stu-id="f256d-167">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="f256d-168">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f256d-168">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="f256d-169">これは、開発者がアドレス演算子と組み合わせて動作するために、オーバーロードの解決規則に依存することを意味します。</span><span class="sxs-lookup"><span data-stu-id="f256d-169">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

<span data-ttu-id="f256d-170">アドレス演算子は `ldftn` 命令を使用して実装されます。</span><span class="sxs-lookup"><span data-stu-id="f256d-170">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="f256d-171">この機能の制限:</span><span class="sxs-lookup"><span data-stu-id="f256d-171">Restrictions of this feature:</span></span>

- <span data-ttu-id="f256d-172">`static`としてマークされたメソッドにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="f256d-172">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="f256d-173">`static` 以外のローカル関数は、`&`では使用できません。</span><span class="sxs-lookup"><span data-stu-id="f256d-173">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="f256d-174">これらのメソッドの実装の詳細は、言語によって意図的に指定されていません。</span><span class="sxs-lookup"><span data-stu-id="f256d-174">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="f256d-175">これには、静的であるかインスタンスであるか、またはそれらが出力されるシグネチャが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f256d-175">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="f256d-176">より優れた関数メンバー</span><span class="sxs-lookup"><span data-stu-id="f256d-176">Better function member</span></span>

<span data-ttu-id="f256d-177">より適切な関数メンバーの指定は、次の行を含むように変更されます。</span><span class="sxs-lookup"><span data-stu-id="f256d-177">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="f256d-178">`delegate*` の方がより具体的 `void*`</span><span class="sxs-lookup"><span data-stu-id="f256d-178">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="f256d-179">つまり、`void*` と `delegate*` でオーバーロードしても、sensibly 演算子を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="f256d-179">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="open-issues"></a><span data-ttu-id="f256d-180">懸案事項を開く</span><span class="sxs-lookup"><span data-stu-id="f256d-180">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="f256d-181">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="f256d-181">NativeCallableAttribute</span></span>

<span data-ttu-id="f256d-182">これは、を呼び出すときにマネージドからネイティブへのプロローグを回避するために CLR によって使用される属性です。</span><span class="sxs-lookup"><span data-stu-id="f256d-182">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="f256d-183">この属性でマークされたメソッドは、ネイティブコードからのみ呼び出すことができます。マネージ (メソッドを呼び出したり、デリゲートを作成したりすることはできません) などです。</span><span class="sxs-lookup"><span data-stu-id="f256d-183">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="f256d-184">この属性は mscorlib に特別ではありません。ランタイムは、この名前を持つすべての属性を同じセマンティクスで扱います。</span><span class="sxs-lookup"><span data-stu-id="f256d-184">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="f256d-185">ランタイムと言語が連携して、これを完全にサポートすることができます。</span><span class="sxs-lookup"><span data-stu-id="f256d-185">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="f256d-186">この言語では、指定された呼び出し規約を持つ `delegate*` として、`NativeCallable` 属性を持つアドレス `static` メンバーを扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="f256d-186">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

<span data-ttu-id="f256d-187">また、言語も次のようにします。</span><span class="sxs-lookup"><span data-stu-id="f256d-187">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="f256d-188">`NativeCallable` でタグ付けされたメソッドに対するマネージ呼び出しに対して、エラーとしてフラグを付けます。</span><span class="sxs-lookup"><span data-stu-id="f256d-188">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="f256d-189">関数をマネージコードから呼び出すことができない場合、コンパイラは、開発者がこのような呼び出しを試行できないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-189">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="f256d-190">メソッドが `NativeCallable`にタグ付けされている場合に、メソッドグループの変換を `delegate` できないようにします。</span><span class="sxs-lookup"><span data-stu-id="f256d-190">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="f256d-191">ただし、`NativeCallable` をサポートするためには必要ありません。</span><span class="sxs-lookup"><span data-stu-id="f256d-191">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="f256d-192">コンパイラは、既存の構文を使用するのと同様に、`NativeCallable` 属性をサポートできます。</span><span class="sxs-lookup"><span data-stu-id="f256d-192">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="f256d-193">このプログラムは、正しい `delegate*` シグネチャにキャストする前に、`void*` にキャストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-193">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="f256d-194">これは現在のサポートよりも悪くはありません。</span><span class="sxs-lookup"><span data-stu-id="f256d-194">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="f256d-195">アンマネージ呼び出し規約の拡張可能なセット</span><span class="sxs-lookup"><span data-stu-id="f256d-195">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="f256d-196">現在の ECMA-335 エンコーディングでサポートされているアンマネージ呼び出し規約のセットは古くなっています。</span><span class="sxs-lookup"><span data-stu-id="f256d-196">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="f256d-197">アンマネージ呼び出し規約のサポートを追加するように要求されました。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f256d-197">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="f256d-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span><span class="sxs-lookup"><span data-stu-id="f256d-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="f256d-199">StdCall と明示的にこの https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span><span class="sxs-lookup"><span data-stu-id="f256d-199">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="f256d-200">この機能を設計するには、今後、必要に応じてアンマネージ呼び出し規約のセットを拡張できるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-200">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="f256d-201">この問題には、エンコード呼び出し規則のための制限された領域 (16 個の値から `IMAGE_CEE_CS_CALLCONV_MASK`) と、新しい呼び出し規則を追加するために触れる必要がある場所の数が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f256d-201">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="f256d-202">解決策として、 [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention)列挙型を使用して呼び出し規約を表す新しいエンコーディングを導入することが考えられます。</span><span class="sxs-lookup"><span data-stu-id="f256d-202">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="f256d-203">参照用に、 https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h には、LLVM でサポートされている呼び出し規約の一覧があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-203">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="f256d-204">.NET では、すべてをサポートする必要があるとは考えられませんが、呼び出し規約の領域が非常に豊富であることを示しています。</span><span class="sxs-lookup"><span data-stu-id="f256d-204">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="f256d-205">考慮事項</span><span class="sxs-lookup"><span data-stu-id="f256d-205">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="f256d-206">インスタンスメソッドを許可する</span><span class="sxs-lookup"><span data-stu-id="f256d-206">Allow instance methods</span></span>

<span data-ttu-id="f256d-207">提案は、`EXPLICITTHIS` CLI の呼び出し規約 (コードでC#は `instance` と呼ばれます) を利用して、インスタンスメソッドをサポートするように拡張できます。</span><span class="sxs-lookup"><span data-stu-id="f256d-207">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="f256d-208">この形式の CLI 関数ポインターは、関数ポインター構文の明示的な最初のパラメーターとして `this` パラメーターを格納します。</span><span class="sxs-lookup"><span data-stu-id="f256d-208">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="f256d-209">これはサウンドですが、提案にはいくつかの複雑なものが追加されます。</span><span class="sxs-lookup"><span data-stu-id="f256d-209">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="f256d-210">特に、呼び出し規約 `instance` と `managed` が異なる関数ポインターは、同じC#シグネチャを持つマネージメソッドを呼び出すために使用される場合でも、互換性がないことになります。</span><span class="sxs-lookup"><span data-stu-id="f256d-210">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="f256d-211">また、どのような場合でも、`static` のローカル関数を使用することによって、これがどのような場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="f256d-211">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="f256d-212">宣言で unsafe を必要としない</span><span class="sxs-lookup"><span data-stu-id="f256d-212">Don't require unsafe at declaration</span></span>

<span data-ttu-id="f256d-213">`delegate*`を使用するたびに `unsafe` を要求するのではなく、メソッドグループが `delegate*`に変換される時点でのみ必要になります。</span><span class="sxs-lookup"><span data-stu-id="f256d-213">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="f256d-214">ここで、主要な安全性の問題が発生します (値が生きている間は、含んでいるアセンブリをアンロードできないことがわかっています)。</span><span class="sxs-lookup"><span data-stu-id="f256d-214">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="f256d-215">他の場所での `unsafe` の要求は、過剰に表示されることがあります。</span><span class="sxs-lookup"><span data-stu-id="f256d-215">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="f256d-216">これは、当初の設計の意図です。</span><span class="sxs-lookup"><span data-stu-id="f256d-216">This is how the design was originally intended.</span></span> <span data-ttu-id="f256d-217">しかし、結果として得られる言語規則は非常に厄介です。</span><span class="sxs-lookup"><span data-stu-id="f256d-217">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="f256d-218">これはポインター値であるという事実を隠すことはできず、`unsafe` キーワードを使用しなくてもピークを維持することはできません。</span><span class="sxs-lookup"><span data-stu-id="f256d-218">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="f256d-219">たとえば、`object` への変換は許可できません。 `class`のメンバーにすることはできません...このC#設計では、すべてのポインターでを使用するために `unsafe` が必要になります。したがって、この設計はそれに従っています。</span><span class="sxs-lookup"><span data-stu-id="f256d-219">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="f256d-220">開発者は、現在の通常のポインター型と同じように、`delegate*` の値の上に_安全_なラッパーを提示することもできます。</span><span class="sxs-lookup"><span data-stu-id="f256d-220">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="f256d-221">検討事項:</span><span class="sxs-lookup"><span data-stu-id="f256d-221">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="f256d-222">デリゲートの使用</span><span class="sxs-lookup"><span data-stu-id="f256d-222">Using delegates</span></span>

<span data-ttu-id="f256d-223">新しい構文要素 `delegate*`使用する代わりに、型に続く `*` で既存の `delegate` 型を使用するだけです。</span><span class="sxs-lookup"><span data-stu-id="f256d-223">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="f256d-224">呼び出し規約の処理は、`CallingConvention` 値を指定する属性を使用して `delegate` 型に注釈を付けることによって行うことができます。</span><span class="sxs-lookup"><span data-stu-id="f256d-224">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="f256d-225">属性がない場合は、マネージ呼び出し規約を示します。</span><span class="sxs-lookup"><span data-stu-id="f256d-225">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="f256d-226">IL でこれをエンコードすると、問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="f256d-226">Encoding this in IL is problematic.</span></span> <span data-ttu-id="f256d-227">基になる値はポインターとして表す必要がありますが、次のことも必要です。</span><span class="sxs-lookup"><span data-stu-id="f256d-227">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="f256d-228">異なる関数ポインター型のオーバーロードに対して許可する一意の型を持つ。</span><span class="sxs-lookup"><span data-stu-id="f256d-228">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="f256d-229">アセンブリの境界を越えて、OHI を使用する場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="f256d-229">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="f256d-230">最後の点は特に問題になります。</span><span class="sxs-lookup"><span data-stu-id="f256d-230">The last point is particularly problematic.</span></span> <span data-ttu-id="f256d-231">これは、`Func<int>*` がアセンブリ内で定義されていても制御できない場合でも、`Func<int>*` を使用するすべてのアセンブリが同等の型をメタデータでエンコードする必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="f256d-231">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="f256d-232">また、mscorlib ではないアセンブリ内の `System.Func<T>` 名前で定義されているその他の型は、mscorlib で定義されているバージョンとは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-232">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="f256d-233">探索された1つのオプションは、このようなポインターを `mod_req(Func<int>) void*`として生成していました。</span><span class="sxs-lookup"><span data-stu-id="f256d-233">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="f256d-234">これは、`mod_req` が `TypeSpec` にバインドできず、ジェネリックのインスタンス化をターゲットにできないため、機能しません。</span><span class="sxs-lookup"><span data-stu-id="f256d-234">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="f256d-235">名前付き関数ポインター</span><span class="sxs-lookup"><span data-stu-id="f256d-235">Named function pointers</span></span>

<span data-ttu-id="f256d-236">関数ポインターの構文は、特に、入れ子になった関数ポインターのような複雑なケースでは、煩雑になることがあります。</span><span class="sxs-lookup"><span data-stu-id="f256d-236">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="f256d-237">開発者は、`delegate`で実行されるように、言語で関数ポインターの名前付き宣言を使用できるようになるたびに署名を入力するのではなく、</span><span class="sxs-lookup"><span data-stu-id="f256d-237">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="f256d-238">ここでの問題の一部は、基になる CLI プリミティブには名前がないためC# 、これは純粋に発明であり、有効にするために多少のメタデータ作業が必要になります。</span><span class="sxs-lookup"><span data-stu-id="f256d-238">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="f256d-239">これは取り上げですが、作業の重要な部分です。</span><span class="sxs-lookup"><span data-stu-id="f256d-239">That is doable but is a significant about of work.</span></span> <span data-ttu-id="f256d-240">基本的に、 C#これらの名前の型 def テーブルに対するコンパニオンが必要です。</span><span class="sxs-lookup"><span data-stu-id="f256d-240">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="f256d-241">また、名前付き関数ポインターの引数を調べると、他の多くのシナリオにも同様に適用される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-241">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="f256d-242">たとえば、名前付きタプルを宣言するだけで、すべての場合に完全な署名を入力する必要性を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="f256d-242">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="f256d-243">説明した後、`delegate*` 型の名前付き宣言を許可しないことにしました。</span><span class="sxs-lookup"><span data-stu-id="f256d-243">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="f256d-244">お客様の利用状況に関するフィードバックに基づいて、このことに大きなニーズがあることがわかった場合は、関数ポインター、タプル、ジェネリックなどに対して機能する名前付けソリューションを調査します。これは、言語でのフル `typedef` サポートなどの他の提案と同様に似ている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-244">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="f256d-245">将来の注意事項</span><span class="sxs-lookup"><span data-stu-id="f256d-245">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="f256d-246">静的ローカル関数</span><span class="sxs-lookup"><span data-stu-id="f256d-246">static local functions</span></span>

<span data-ttu-id="f256d-247">これは、ローカル関数で `static` 修飾子を許可する[提案](https://github.com/dotnet/csharplang/issues/1565)を指します。</span><span class="sxs-lookup"><span data-stu-id="f256d-247">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="f256d-248">このような関数は、ソースコードで正確なシグネチャを指定した `static` として出力されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="f256d-248">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="f256d-249">このような関数は、ローカル関数の現在の問題を含んでいないため、`&` の有効な引数である必要があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-249">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="f256d-250">静的デリゲート</span><span class="sxs-lookup"><span data-stu-id="f256d-250">static delegates</span></span>

<span data-ttu-id="f256d-251">これは、`static` のメンバーのみを参照できる `delegate` 型の宣言を許可する[提案](https://github.com/dotnet/csharplang/issues/302)を指します。</span><span class="sxs-lookup"><span data-stu-id="f256d-251">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="f256d-252">このような `delegate` インスタンスの利点は、パフォーマンスを重視するシナリオでは、自由に割り当てできます。</span><span class="sxs-lookup"><span data-stu-id="f256d-252">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="f256d-253">関数ポインター機能が実装されている場合、`static delegate` の提案は終了する可能性があります。この機能の利点として、割り当ての自由な特性が挙げられます。</span><span class="sxs-lookup"><span data-stu-id="f256d-253">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="f256d-254">ただし、最近の調査では、アセンブリのアンロードによって実現できないことがわかりました。</span><span class="sxs-lookup"><span data-stu-id="f256d-254">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="f256d-255">アセンブリがアンロードされないようにするには、`static delegate` から参照するメソッドまでの厳密なハンドルが必要です。</span><span class="sxs-lookup"><span data-stu-id="f256d-255">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="f256d-256">すべての `static delegate` インスタンスを維持するには、提案の目標に対してカウンターを実行する新しいハンドルを割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-256">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="f256d-257">いくつかの設計では、割り当てを1つの呼び出しサイトに1回の割り当てに償却できましたが、それは少し複雑で、トレードオフではないと思われました。</span><span class="sxs-lookup"><span data-stu-id="f256d-257">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="f256d-258">つまり、開発者は、基本的に次のトレードオフを決定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f256d-258">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="f256d-259">アセンブリのアンロード時の安全性: これには割り当てが必要であるため、`delegate` は既に十分なオプションです。</span><span class="sxs-lookup"><span data-stu-id="f256d-259">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="f256d-260">アセンブリのアンロードに関して安全ではありません: `delegate*`を使用します。</span><span class="sxs-lookup"><span data-stu-id="f256d-260">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="f256d-261">これは、コードの残りの部分で `unsafe` コンテキスト以外の使用を許可するために `struct` にラップできます。</span><span class="sxs-lookup"><span data-stu-id="f256d-261">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
