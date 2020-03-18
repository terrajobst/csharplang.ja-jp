---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483482"
---
# <a name="readonly-locals-and-parameters"></a><span data-ttu-id="d03a7-101">readonly ローカルおよびパラメーター</span><span class="sxs-lookup"><span data-stu-id="d03a7-101">readonly locals and parameters</span></span>

* <span data-ttu-id="d03a7-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="d03a7-102">[x] Proposed</span></span>
* <span data-ttu-id="d03a7-103">[] プロトタイプ</span><span class="sxs-lookup"><span data-stu-id="d03a7-103">[ ] Prototype</span></span>
* <span data-ttu-id="d03a7-104">[] の実装</span><span class="sxs-lookup"><span data-stu-id="d03a7-104">[ ] Implementation</span></span>
* <span data-ttu-id="d03a7-105">[] 仕様</span><span class="sxs-lookup"><span data-stu-id="d03a7-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="d03a7-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="d03a7-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d03a7-107">ローカルおよびパラメーターに読み取り専用として注釈を付けて、これらのローカルとパラメーターが緩やかに変化しないようにします。</span><span class="sxs-lookup"><span data-stu-id="d03a7-107">Allow locals and parameters to be annotated as readonly in order to prevent shallow mutation of those locals and parameters.</span></span>

## <a name="motivation"></a><span data-ttu-id="d03a7-108">目的</span><span class="sxs-lookup"><span data-stu-id="d03a7-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d03a7-109">今日では、`readonly` キーワードをフィールドに適用できます。これには、構築時に (静的フィールドの場合は静的な構築、インスタンスフィールドの場合はインスタンスの構築時には) フィールドを確実に書き込むことができるという効果があります。これにより、開発者は、変更する必要のない状態を誤って上書きして間違いを防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="d03a7-109">Today, the `readonly` keyword can be applied to fields; this has the effect of ensuring that a field can only be written to during construction (static construction in the case of a static field, or instance construction in the case of an instance field), which helps developers avoid mistakes by accidentally overwriting state which should not be modified.</span></span> <span data-ttu-id="d03a7-110">ただし、開発者が値を変換しないようにする必要があるのは、フィールドだけではありません。</span><span class="sxs-lookup"><span data-stu-id="d03a7-110">But fields aren't the only places developers want to ensure that values aren't mutated.</span></span> <span data-ttu-id="d03a7-111">特に、一時的な状態を格納するためのローカル変数を作成するのは一般的ですが、誤って計算やそのようなバグが発生する可能性があります。そのような場合は特に、このようなリフトされたフィールドを `readonly`としてマークする方法はありません。</span><span class="sxs-lookup"><span data-stu-id="d03a7-111">In particular, it's common to create a local variable to store temporary state, and accidentally updating that temporary state can result in erroneous calculations and other such bugs, especially when such "locals" are captured in lambdas, at which point they are lifted to fields, but there's no way today to mark such lifted fields as `readonly`.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d03a7-112">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="d03a7-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d03a7-113">ローカル変数は、宣言時にのみ設定されるようにコンパイラによって `readonly` として注釈が付けられます ( C# ' foreach ' ループ内の反復変数や ' using ' ブロックの使用されている変数など、の特定のローカル変数は既に暗黙的に読み取り専用ですが、現在の開発者は他のローカルを `readonly`としてマークする</span><span class="sxs-lookup"><span data-stu-id="d03a7-113">Locals will be annotatable as `readonly` as well, with the compiler ensuring that they're only set at the time of declaration (certain locals in C# are already implicitly readonly, such as the iteration variable in a 'foreach' loop or the used variable in a 'using' block, but currently a developer has no ability to mark other locals as `readonly`).</span></span> <span data-ttu-id="d03a7-114">このような `readonly` ローカルには初期化子が必要です。</span><span class="sxs-lookup"><span data-stu-id="d03a7-114">Such `readonly` locals must have an initializer:</span></span>

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="d03a7-115">`readonly var`の短縮形として、既存のコンテキストキーワード `let` を使用することもできます。たとえば、</span><span class="sxs-lookup"><span data-stu-id="d03a7-115">And as shorthand for `readonly var`, the existing contextual keyword `let` may be used, e.g.</span></span>

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="d03a7-116">初期化子の機能に関する特別な制約はありません。また、現在、ローカルの初期化子として有効なすべてのものにすることができます。たとえば、</span><span class="sxs-lookup"><span data-stu-id="d03a7-116">There are no special constraints for what the initializer can be, and can be anything currently valid as an initializer for locals, e.g.</span></span>

```csharp
readonly T data = arg1 ?? arg2;
```

<span data-ttu-id="d03a7-117">ローカルでの `readonly` は、ラムダとクロージャを操作する場合に特に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="d03a7-117">`readonly` on locals is particularly valuable when working with lambdas and closures.</span></span> <span data-ttu-id="d03a7-118">匿名メソッドまたはラムダが外側のスコープからローカル状態にアクセスすると、その状態は、"display class" によって表されるコンパイラによってクロージャにキャプチャされます。</span><span class="sxs-lookup"><span data-stu-id="d03a7-118">When an anonymous method or lambda accesses local state from the enclosing scope, that state is captured into a closure by the compiler, which is represented by a "display class."</span></span>  <span data-ttu-id="d03a7-119">キャプチャされた各 "ローカル" はこのクラスのフィールドですが、コンパイラはこのフィールドを代わりに生成するため、ラムダが "ローカル" に誤って書き込まれないようにするために `readonly` として注釈を付けることはできません (少なくとも結果の MSIL にはないため、引用符で囲まれています)。</span><span class="sxs-lookup"><span data-stu-id="d03a7-119">Each "local" that's captured is a field in this class, yet because the compiler is generating this field on your behalf, you have no opportunity to annotate it as `readonly` in order to prevent the lambda from erroneously writing to the "local" (in quotes because it's really not a local, at least not in the resulting MSIL).</span></span> <span data-ttu-id="d03a7-120">`readonly` ローカル変数を使用すると、コンパイラはラムダがローカルに書き込まれるのを防ぐことができます。これは、間違った書き込みが発生する可能性がありますが、同時実行のバグが発生する可能性がありますが、非常に複雑な場合は、マルチスレッドが関係するシナリオで特に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="d03a7-120">With `readonly` locals, the compiler can prevent the lambda from writing to local, which is particularly valuable in scenarios involving multithreading where an erroneous write could result in a dangerous but rare and hard-to-find concurrency bug.</span></span>

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

<span data-ttu-id="d03a7-121">特別な形式のローカルでは、パラメーターも `readonly`として注釈付けされます。</span><span class="sxs-lookup"><span data-stu-id="d03a7-121">As a special form of local, parameters will also be annotatable as `readonly`.</span></span> <span data-ttu-id="d03a7-122">これは、メソッドの呼び出し元がパラメーターに渡すことができることには影響しません (`readonly` フィールドに格納できる値に関する制約はありません) が、`readonly` ローカルの場合と同様に、コンパイラは宣言後のパラメーターへのコードの書き込みを禁止します。つまり、メソッドの本体でパラメーターへの書き込みが禁止されます。</span><span class="sxs-lookup"><span data-stu-id="d03a7-122">This would have no effect on what the caller of the method is able to pass to the parameter (just as there's no constraint on what values may be stored into a `readonly` field), but as with any `readonly` local, the compiler would prohibit code from writing to the parameter after declaration, which means the body of the method is prohibited from writing to the parameter.</span></span>

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

<span data-ttu-id="d03a7-123">`readonly` パラメーターは、そのメソッドのコンパイラによって出力される署名/メタデータには影響しません。また、メソッドの本体のコンパイルをコンパイラが処理する方法にのみ影響します。</span><span class="sxs-lookup"><span data-stu-id="d03a7-123">`readonly` parameters do not affect the signature/metadata emitted by the compiler for that method, and simply affect how the compiler handles the compilation of the method's body.</span></span> <span data-ttu-id="d03a7-124">したがって、たとえば、基本の仮想メソッドは `readonly` パラメーターを持つことができ、そのパラメーターをオーバーライドで書き込み可能にすることができます。</span><span class="sxs-lookup"><span data-stu-id="d03a7-124">Thus, for example, a base virtual method could have a `readonly` parameter, and that parameter could be writable in an override.</span></span>

<span data-ttu-id="d03a7-125">フィールドの場合と同様に、ローカルおよびパラメーターの `readonly` は浅いため、ストレージの場所に影響しますが、オブジェクトグラフに推移的に影響することはありません。</span><span class="sxs-lookup"><span data-stu-id="d03a7-125">As with fields, `readonly` for locals and parameters is shallow, affecting the storage location but not transitively affecting the object graph.</span></span> <span data-ttu-id="d03a7-126">ただし、フィールドと同様に、`readonly` ローカル/パラメーター構造体でメソッドを呼び出すと、実際には構造体のコピーが作成され、そのコピーに対してメソッドが呼び出され、`this`の内部的な変更を回避できます。</span><span class="sxs-lookup"><span data-stu-id="d03a7-126">However, also as with fields, calling a method on a `readonly` local/parameter struct will actually make a copy of the struct and call the method on the copy, in order to avoid internal mutation of `this`.</span></span>

<span data-ttu-id="d03a7-127">`ref readonly` もサポートされている場合を除き、`ref` または `out` 引数として `readonly` ローカルパラメーターとパラメーターを渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="d03a7-127">`readonly` locals and parameters can't be passed as `ref` or `out` arguments, unless/until `ref readonly` is also supported.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d03a7-128">代替</span><span class="sxs-lookup"><span data-stu-id="d03a7-128">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="d03a7-129">`val` は、`let`に代わる代替手段として使用できます。</span><span class="sxs-lookup"><span data-stu-id="d03a7-129">`val` could be used as an alternative shorthand to `let`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d03a7-130">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="d03a7-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="d03a7-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: この提案とは別に、`ref readonly` を処理する方法についての質問を残しました。</span><span class="sxs-lookup"><span data-stu-id="d03a7-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: I've left the question of how to handle `ref readonly` as separate from this proposal.</span></span>
- <span data-ttu-id="d03a7-132">この提案では、読み取り専用の構造体/変更できない型は解決されません。</span><span class="sxs-lookup"><span data-stu-id="d03a7-132">This proposal does not tackle readonly structs / immutable types.</span></span> <span data-ttu-id="d03a7-133">これは別の提案のために残されています。</span><span class="sxs-lookup"><span data-stu-id="d03a7-133">That is left for a separate proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d03a7-134">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="d03a7-134">Design meetings</span></span>

- <span data-ttu-id="d03a7-135">2015年1月21日 (<https://github.com/dotnet/roslyn/issues/98>) について簡単に説明します。</span><span class="sxs-lookup"><span data-stu-id="d03a7-135">Briefly discussed on Jan 21, 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span></span>
