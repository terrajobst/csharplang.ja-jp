---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281945"
---
# <a name="simple-programs"></a><span data-ttu-id="b9ca8-101">単純なプログラム</span><span class="sxs-lookup"><span data-stu-id="b9ca8-101">Simple programs</span></span>

* <span data-ttu-id="b9ca8-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="b9ca8-102">[x] Proposed</span></span>
* <span data-ttu-id="b9ca8-103">[x] プロトタイプ: 開始しました</span><span class="sxs-lookup"><span data-stu-id="b9ca8-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="b9ca8-104">[] の実装: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="b9ca8-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="b9ca8-105">[] 仕様: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="b9ca8-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="b9ca8-106">要約</span><span class="sxs-lookup"><span data-stu-id="b9ca8-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b9ca8-107">*Compilation_unit*の*namespace_member_declaration*s の直前 (つまりソースファイル) に、*ステートメント*のシーケンスを実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="b9ca8-108">セマンティクスは、このような一連の*ステートメント*が存在する場合、実際の型名とメソッド名をモジュロする次の型宣言が生成されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="b9ca8-109">https://github.com/dotnet/csharplang/issues/3117 も参照してください。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="b9ca8-110">目的</span><span class="sxs-lookup"><span data-stu-id="b9ca8-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b9ca8-111">明示的な `Main` 方法が必要になるため、プログラムの中でも最も単純なものがあります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="b9ca8-112">これは、言語学習とプログラムのわかりやすさを実現するためのものです。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="b9ca8-113">この機能の主な目的は、学習者C#やコードの明瞭さを避けるために、不要な定型句を使用しないプログラムを許可することです。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b9ca8-114">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="b9ca8-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="b9ca8-115">構文</span><span class="sxs-lookup"><span data-stu-id="b9ca8-115">Syntax</span></span>

<span data-ttu-id="b9ca8-116">追加の構文では、コンパイル単位で*ステートメント*のシーケンスを許可するだけで、 *namespace_member_declaration*s の直前になります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="b9ca8-117">1つの*compilation_unit*のみが*ステートメント*を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-117">Only one *compilation_unit* is allowed to have *statement*s.</span></span> 

<span data-ttu-id="b9ca8-118">例:</span><span class="sxs-lookup"><span data-stu-id="b9ca8-118">Example:</span></span>

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="b9ca8-119">Semantics</span><span class="sxs-lookup"><span data-stu-id="b9ca8-119">Semantics</span></span>

<span data-ttu-id="b9ca8-120">最上位レベルのステートメントがプログラムのコンパイル単位に存在する場合、次のように、その意味は、グローバル名前空間の `Program` クラスの `Main` メソッドのブロック本体で結合されているかのようになります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="b9ca8-121">"Program" と "Main" という名前は、図の目的でのみ使用されます。コンパイラで使用される実際の名前は実装に依存しており、メソッドをソースコードから名前で参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="b9ca8-122">メソッドは、プログラムのエントリポイントとして指定されます。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="b9ca8-123">規約によって明示的に宣言されたメソッドは、エントリポイントの候補と見なされることがあります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="b9ca8-124">警告は、発生した場合に報告されます。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-124">A warning is reported when that happens.</span></span> <span data-ttu-id="b9ca8-125">最上位レベルのステートメントがある場合に `-main:<type>` コンパイラスイッチを指定するとエラーになります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-125">It is an error to specify `-main:<type>` compiler switch when there are top-level statements.</span></span>

<span data-ttu-id="b9ca8-126">非同期操作は、通常の非同期エントリポイントメソッド内のステートメントで許可されるレベルまで、最上位レベルのステートメントで許可されます。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-126">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="b9ca8-127">ただし、`await` 式やその他の非同期操作を省略すると、警告は生成されません。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-127">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="b9ca8-128">代わりに、生成されたエントリポイントメソッドのシグネチャはと同じになります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-128">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="b9ca8-129">上記の例では、次の `$Main` メソッド宣言が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-129">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="b9ca8-130">同時に、次のような例があります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-130">At the same time an example like this:</span></span>
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="b9ca8-131">次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-131">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="b9ca8-132">最上位レベルのローカル変数とローカル関数のスコープ</span><span class="sxs-lookup"><span data-stu-id="b9ca8-132">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="b9ca8-133">最上位のローカル変数と関数は、生成されたエントリポイントメソッドに "ラップ" されますが、すべてのコンパイル単位でプログラム全体のスコープ内に存在している必要があります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-133">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program in every compilation unit.</span></span>
<span data-ttu-id="b9ca8-134">単純な名前の評価を目的として、グローバル名前空間に到達した場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-134">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="b9ca8-135">最初に、生成されたエントリポイントメソッド内で名前を評価しようとしましたが、この試行が失敗した場合にのみ、</span><span class="sxs-lookup"><span data-stu-id="b9ca8-135">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="b9ca8-136">グローバル名前空間宣言内の "regular" 評価が実行されます。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-136">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="b9ca8-137">これにより、グローバル名前空間内で宣言されている名前空間と型、およびインポートされた名前のシャドウに名前が付けられる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-137">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="b9ca8-138">単純な名前の評価が最上位レベルのステートメントの外部で発生し、評価でトップレベルのローカル変数または関数が生成される場合、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-138">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="b9ca8-139">この方法では、"最上位の関数" (https://github.com/dotnet/csharplang/issues/3117)でのシナリオ 2) をより適切に解決できるようになります。また、サポートされていると誤解したユーザーに役立つ診断を提供できます。</span><span class="sxs-lookup"><span data-stu-id="b9ca8-139">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

