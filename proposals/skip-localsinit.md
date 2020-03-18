---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483488"
---
# <a name="suppress-emitting-of-localsinit-flag"></a><span data-ttu-id="ac371-101">`localsinit` フラグの出力を抑制します。</span><span class="sxs-lookup"><span data-stu-id="ac371-101">Suppress emitting of `localsinit` flag.</span></span>

* <span data-ttu-id="ac371-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="ac371-102">[x] Proposed</span></span>
* <span data-ttu-id="ac371-103">[] プロトタイプ: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="ac371-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="ac371-104">[] の実装: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="ac371-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="ac371-105">[] 仕様: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="ac371-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="ac371-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="ac371-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ac371-107">`SkipLocalsInitAttribute` 属性を使用した `localsinit` フラグの生成の抑制を許可します。</span><span class="sxs-lookup"><span data-stu-id="ac371-107">Allow suppressing emit of `localsinit` flag via `SkipLocalsInitAttribute` attribute.</span></span> 

## <a name="motivation"></a><span data-ttu-id="ac371-108">目的</span><span class="sxs-lookup"><span data-stu-id="ac371-108">Motivation</span></span>
[motivation]: #motivation


### <a name="background"></a><span data-ttu-id="ac371-109">バックグラウンド</span><span class="sxs-lookup"><span data-stu-id="ac371-109">Background</span></span>
<span data-ttu-id="ac371-110">参照を含まない CLR 仕様のローカル変数は、VM/JIT によって特定の値に初期化されることはありません。</span><span class="sxs-lookup"><span data-stu-id="ac371-110">Per CLR spec local variables that do not contain references are not initialized to a particular value by the VM/JIT.</span></span> <span data-ttu-id="ac371-111">初期化せずにこのような変数から読み取ることはタイプセーフですが、それ以外の場合、動作は未定義で実装固有です。</span><span class="sxs-lookup"><span data-stu-id="ac371-111">Reading from such variables without initialization is type-safe, but otherwise the behavior is undefined and implementation specific.</span></span> <span data-ttu-id="ac371-112">通常、初期化されていないローカルには、スタックフレームによって現在使用されているメモリに残っていた値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ac371-112">Typically uninitialized locals contain whatever values were left in the memory that is now occupied by the stack frame.</span></span> <span data-ttu-id="ac371-113">これにより、非決定的な動作が発生し、バグを再現するのが困難になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-113">That could lead to nondeterministic behavior and hard to reproduce bugs.</span></span> 

<span data-ttu-id="ac371-114">ローカル変数を "割り当てる" には、次の2つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-114">There are two ways to "assign" a local variable:</span></span> 
- <span data-ttu-id="ac371-115">値を格納するか、</span><span class="sxs-lookup"><span data-stu-id="ac371-115">by storing a value or</span></span> 
- <span data-ttu-id="ac371-116">`localsinit` フラグを指定することによって、ローカルメモリプールを強制的にゼロ初期化されないようにします。これには、ローカル変数と `stackalloc` データの両方が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ac371-116">by specifying `localsinit` flag which forces everything that is allocated form the local memory pool to be zero-initialized NOTE: this includes both local variables and `stackalloc` data.</span></span>    

<span data-ttu-id="ac371-117">初期化されていないデータは使用しないことをお勧めします。検証可能なコードでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="ac371-117">Use of uninitialized data is discouraged and is not allowed in verifiable code.</span></span> <span data-ttu-id="ac371-118">フロー分析の手段によって証明される可能性もありますが、検証アルゴリズムを控えめにすることが許可されており、単に `localsinit` が設定されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-118">While it might be possible to prove that by the means of flow analysis, it is permitted for the verification algorithm to be conservative and simply require that `localsinit` is set.</span></span>

<span data-ttu-id="ac371-119">以前C#のコンパイラでは、ローカルを宣言するすべてのメソッドに `localsinit` フラグが生成されました。</span><span class="sxs-lookup"><span data-stu-id="ac371-119">Historically C# compiler emits `localsinit` flag on all methods that declare locals.</span></span>

<span data-ttu-id="ac371-120">C#では、厳密な割り当て分析を使用します。これは、CLR 仕様C#で必要とされるよりも厳格です (ローカルのスコープを考慮する必要もあります) が、結果のコードが正式に検証可能であるとは限りません。</span><span class="sxs-lookup"><span data-stu-id="ac371-120">While C# employs definite-assignment analysis which is more strict than what CLR spec would require (C# also needs to consider scoping of locals), it is not strictly guaranteed that the resulting code would be formally verifiable:</span></span>
- <span data-ttu-id="ac371-121">CLR およびC#規則は、ローカルのを `out` 引数として渡すことが `use`であるかどうかには同意しません。</span><span class="sxs-lookup"><span data-stu-id="ac371-121">CLR and C# rules may not agree on whether passing a local as `out` argument is a `use`.</span></span>
- <span data-ttu-id="ac371-122">条件がC#わかっている場合 (定数伝達)、CLR とルールが条件分岐の処理に同意しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-122">CLR and C# rules may not agree on treatment of conditional branches when conditions are known (constant propagation).</span></span>
- <span data-ttu-id="ac371-123">CLR は、許可されているため、単に `localinits`を要求することもできます。</span><span class="sxs-lookup"><span data-stu-id="ac371-123">CLR could as well simply require `localinits`, since that is permitted.</span></span>  

### <a name="problem"></a><span data-ttu-id="ac371-124">問題</span><span class="sxs-lookup"><span data-stu-id="ac371-124">Problem</span></span>
<span data-ttu-id="ac371-125">高パフォーマンスのアプリケーションでは、強制ゼロ初期化のコストが顕著になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-125">In high-performance application the cost of forced zero-initialization could be noticeable.</span></span> <span data-ttu-id="ac371-126">`stackalloc` が使用されている場合は特に顕著です。</span><span class="sxs-lookup"><span data-stu-id="ac371-126">It is particularly noticeable when `stackalloc` is used.</span></span>

<span data-ttu-id="ac371-127">場合によっては、そのような初期化が後続の割り当てによって "強制終了" されるときに、JIT が個々のローカルの初期ゼロ初期化を解除することがあります。</span><span class="sxs-lookup"><span data-stu-id="ac371-127">In some cases JIT can elide initial zero-initialization of individual locals when such initialization is "killed" by subsequent assignments.</span></span> <span data-ttu-id="ac371-128">一部の JITs には、このような最適化に制限があるわけではありません。</span><span class="sxs-lookup"><span data-stu-id="ac371-128">Not all JITs do this and such optimization has limits.</span></span> <span data-ttu-id="ac371-129">`stackalloc`には役立ちません。</span><span class="sxs-lookup"><span data-stu-id="ac371-129">It does not help with `stackalloc`.</span></span>

<span data-ttu-id="ac371-130">問題が実際に発生していることを示すために、`IL` ローカル変数を含まないメソッドに `localsinit` フラグがないという既知のバグがあります。</span><span class="sxs-lookup"><span data-stu-id="ac371-130">To illustrate that the problem is real - there is a known bug where a method not containing any `IL` locals would not have `localsinit` flag.</span></span> <span data-ttu-id="ac371-131">このバグは、初期化コストを回避するために意図的に `stackalloc` をこのようなメソッドに配置することによって、ユーザーによって既に悪用されています。</span><span class="sxs-lookup"><span data-stu-id="ac371-131">The bug is already being exploited by users by putting `stackalloc` into such methods - intentionally to avoid initialization costs.</span></span> <span data-ttu-id="ac371-132">しかし、`IL` ローカルがないことは、不安定なメトリックであり、codegen 戦略の変化によって異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-132">That is despite the fact that absence of `IL` locals is an unstable metric and may vary depending on changes in codegen strategy.</span></span> <span data-ttu-id="ac371-133">バグは修正する必要があり、ユーザーはフラグを非表示にするために、より文書化された信頼性の高い方法を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-133">The bug should be fixed and users should get a more documented and reliable way of suppressing the flag.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="ac371-134">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="ac371-134">Detailed design</span></span>

<span data-ttu-id="ac371-135">`localsinit` フラグを出力しないようにコンパイラに指示する方法として、`System.Runtime.CompilerServices.SkipLocalsInitAttribute` を指定できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ac371-135">Allow specifying `System.Runtime.CompilerServices.SkipLocalsInitAttribute` as a way to tell the compiler to not emit `localsinit` flag.</span></span>
 
<span data-ttu-id="ac371-136">この結果、ローカルが JIT によってゼロに初期化されない可能性があります。これは、ほとんどの場合、 C#unobservable にあります。</span><span class="sxs-lookup"><span data-stu-id="ac371-136">The end result of this will be that the locals may not be zero-initialized by the JIT, which is in most cases unobservable in C#.</span></span>  
<span data-ttu-id="ac371-137">それに加えて、`stackalloc` データはゼロ初期化されません。</span><span class="sxs-lookup"><span data-stu-id="ac371-137">In addition to that `stackalloc` data will not be zero-initialized.</span></span> <span data-ttu-id="ac371-138">これは明らかに観測可能ですが、最もスタブのあるシナリオです。</span><span class="sxs-lookup"><span data-stu-id="ac371-138">That is definitely observable, but also is the most motivating scenario.</span></span>

<span data-ttu-id="ac371-139">許可および認識される属性ターゲットは次のとおりです: `Method`、`Property`、`Module`、`Class`、`Struct`、`Interface`、`Constructor`。</span><span class="sxs-lookup"><span data-stu-id="ac371-139">Permitted and recognized attribute targets are: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span></span> <span data-ttu-id="ac371-140">ただし、コンパイラでは、一覧表示されたターゲットで属性が定義されている必要はありません。また、属性が定義されているアセンブリにも注意してください。</span><span class="sxs-lookup"><span data-stu-id="ac371-140">However compiler will not require that attribute is defined with the listed targets nor it will care in which assembly the attribute is defined.</span></span> 

<span data-ttu-id="ac371-141">コンテナー (`class`、`module`、入れ子になったメソッドのメソッドを含む) で属性が指定されている場合、フラグは、コンテナー内に含まれるすべてのメソッドに影響します。</span><span class="sxs-lookup"><span data-stu-id="ac371-141">When attribute is specified on a container (`class`, `module`, containing method for a nested method, ...), the flag affects all methods contained within the container.</span></span>

<span data-ttu-id="ac371-142">合成されたメソッドは、論理コンテナー/所有者からフラグを "継承" します。</span><span class="sxs-lookup"><span data-stu-id="ac371-142">Synthesized methods "inherit" the flag from the logical container/owner.</span></span> 

<span data-ttu-id="ac371-143">フラグは、実際のメソッド本体の codegen 戦略にのみ影響します。</span><span class="sxs-lookup"><span data-stu-id="ac371-143">The flag affects only codegen strategy for actual method bodies.</span></span> <span data-ttu-id="ac371-144">つまり、</span><span class="sxs-lookup"><span data-stu-id="ac371-144">I.E.</span></span> <span data-ttu-id="ac371-145">このフラグは、抽象メソッドには影響しません。メソッドのオーバーライド/実装には反映されません。</span><span class="sxs-lookup"><span data-stu-id="ac371-145">the flag has no effect on abstract methods and is not propagated to overriding/implementing methods.</span></span>

<span data-ttu-id="ac371-146">これは明示的に **_コンパイラ機能_** で **_あり、言語機能ではありません_** 。</span><span class="sxs-lookup"><span data-stu-id="ac371-146">This is explicitly a **_compiler feature_** and **_not a language feature_**.</span></span>  
<span data-ttu-id="ac371-147">コンパイラのコマンドラインスイッチと同様に、この機能は特定の codegen 戦略の実装の詳細を制御します。 C#仕様で必要とされる必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ac371-147">Similarly to compiler command line switches the feature controls implementation details of a particular codegen strategy and does not need to be required by the C# spec.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="ac371-148">短所</span><span class="sxs-lookup"><span data-stu-id="ac371-148">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="ac371-149">古い/その他のコンパイラは、属性に従わない場合があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-149">Old/other compilers may not honor the attribute.</span></span>
<span data-ttu-id="ac371-150">属性は、互換性のある動作として無視されます。</span><span class="sxs-lookup"><span data-stu-id="ac371-150">Ignoring the attribute is compatible behavior.</span></span> <span data-ttu-id="ac371-151">パフォーマンスがわずかに低下する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-151">Only may result in a slight perf hit.</span></span>

- <span data-ttu-id="ac371-152">`localinits` フラグのないコードは、検証エラーをトリガーする場合があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-152">The code without `localinits` flag may trigger verification failures.</span></span>
<span data-ttu-id="ac371-153">この機能を要求するユーザーは、通常、検証可能性とは関係がありません。</span><span class="sxs-lookup"><span data-stu-id="ac371-153">Users that ask for this feature are generally unconcerned with verifiability.</span></span> 
 
- <span data-ttu-id="ac371-154">属性を個々のメソッドより高いレベルで適用すると、非ローカルの効果があります。これは `stackalloc` が使用されている場合に監視できます。</span><span class="sxs-lookup"><span data-stu-id="ac371-154">Applying the attribute at higher levels than an individual method has nonlocal effect, which is observable when `stackalloc` is used.</span></span> <span data-ttu-id="ac371-155">ただし、これは最も要求の厳しいシナリオです。</span><span class="sxs-lookup"><span data-stu-id="ac371-155">Yet, this is the most requested scenario.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ac371-156">代替</span><span class="sxs-lookup"><span data-stu-id="ac371-156">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="ac371-157">メソッドが `unsafe` コンテキストで宣言されている場合は、`localinits` フラグを省略します。</span><span class="sxs-lookup"><span data-stu-id="ac371-157">omit `localinits` flag when method is declared in `unsafe` context.</span></span> <span data-ttu-id="ac371-158">これにより、`stackalloc`の場合に、サイレントで危険な動作が決定的から非決定的に変わる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac371-158">That could cause silent and dangerous behavior change from deterministic to nondeterministic in a case of `stackalloc`.</span></span>

- <span data-ttu-id="ac371-159">常に `localinits` フラグを省略します。</span><span class="sxs-lookup"><span data-stu-id="ac371-159">omit `localinits` flag always.</span></span>
<span data-ttu-id="ac371-160">上記よりも悪くなります。</span><span class="sxs-lookup"><span data-stu-id="ac371-160">Even worse than above.</span></span>

- <span data-ttu-id="ac371-161">メソッドの本体で `stackalloc` が使用されていない場合は、`localinits` フラグを省略します。</span><span class="sxs-lookup"><span data-stu-id="ac371-161">omit `localinits` flag unless `stackalloc` is used in the method body.</span></span>
<span data-ttu-id="ac371-162">は、要求された最も多くのシナリオに対応せず、元に戻すオプションを指定せずにコードをオフにすることができます。</span><span class="sxs-lookup"><span data-stu-id="ac371-162">Does not address the most requested scenario and may turn code unverifiable with no option to revert that back.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="ac371-163">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="ac371-163">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="ac371-164">属性がメタデータに実際に出力される必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="ac371-164">Should the attribute be actually emitted to metadata?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="ac371-165">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="ac371-165">Design meetings</span></span>

<span data-ttu-id="ac371-166">まだありません。</span><span class="sxs-lookup"><span data-stu-id="ac371-166">None yet.</span></span> 