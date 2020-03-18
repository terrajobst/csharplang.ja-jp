---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483560"
---
# <a name="lambda-attributes"></a><span data-ttu-id="2394a-101">ラムダ属性</span><span class="sxs-lookup"><span data-stu-id="2394a-101">Lambda Attributes</span></span>

* <span data-ttu-id="2394a-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="2394a-102">[x] Proposed</span></span>
* <span data-ttu-id="2394a-103">[] プロトタイプ</span><span class="sxs-lookup"><span data-stu-id="2394a-103">[ ] Prototype</span></span>
* <span data-ttu-id="2394a-104">[] の実装</span><span class="sxs-lookup"><span data-stu-id="2394a-104">[ ] Implementation</span></span>
* <span data-ttu-id="2394a-105">[] 仕様</span><span class="sxs-lookup"><span data-stu-id="2394a-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="2394a-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="2394a-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="2394a-107">属性をラムダ (および匿名メソッド) およびラムダ/匿名メソッドのパラメーターに適用できるようにします。これらは通常のメソッドに対して使用できます。</span><span class="sxs-lookup"><span data-stu-id="2394a-107">Allow attributes to be applied to lambdas (and anonymous methods) and to lambda / anonymous method parameters, as they can be on regular methods.</span></span>

## <a name="motivation"></a><span data-ttu-id="2394a-108">目的</span><span class="sxs-lookup"><span data-stu-id="2394a-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="2394a-109">2つの主な動機:</span><span class="sxs-lookup"><span data-stu-id="2394a-109">Two primary motivations:</span></span>

1. <span data-ttu-id="2394a-110">コンパイル時にアナライザーに表示されるメタデータを提供するため。</span><span class="sxs-lookup"><span data-stu-id="2394a-110">To provide metadata visible to analyzers at compile-time.</span></span>
2. <span data-ttu-id="2394a-111">実行時にリフレクションおよびツールに表示されるメタデータを提供するため。</span><span class="sxs-lookup"><span data-stu-id="2394a-111">To provide metadata visible to reflection and tooling at run-time.</span></span>

<span data-ttu-id="2394a-112">(1) の例として、パフォーマンスを重視するコードの場合、状態を閉じるラムダに対してクロージャとデリゲートが割り当てられているときにフラグを設定するアナライザーを作成できると便利です。</span><span class="sxs-lookup"><span data-stu-id="2394a-112">As an example of (1): For performance-sensitive code, it is helpful to be able to have an analyzer that flags when closures and delegates are being allocated for lambdas that close over state.</span></span>  <span data-ttu-id="2394a-113">多くの場合、このようなコードの開発者は、状態をキャプチャしないようにするために、コンパイラがメソッドの静的メソッドとキャッシュ可能なデリゲートを生成できるようにするため、または終了する唯一の状態が `this`であることを確認します。これにより、クロージャオブジェクトの割り当てを少なくすることができます。</span><span class="sxs-lookup"><span data-stu-id="2394a-113">Often a developer of such code will go out of his or her way to avoid capturing any state, so that the compiler can generate a static method and a cacheable delegate for the method, or the developer will ensure that the only state being closed over is `this`, allowing the compiler at least to avoid allocating a closure object.</span></span>  <span data-ttu-id="2394a-114">しかし、キャプチャできるものを制限するための言語サポートがないと、状態が誤って閉じられることがあります。</span><span class="sxs-lookup"><span data-stu-id="2394a-114">But, without language support for limiting what may be captured, it is all too easy to accidentally close over state.</span></span>  <span data-ttu-id="2394a-115">開発者が属性を使用してラムダに注釈を設定し、どの状態を閉じることができるかを示すことができる場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2394a-115">It would be valuable if a developer could annotate lambdas with attributes to indicate what state they're allowed to close over, for example:</span></span>

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

<span data-ttu-id="2394a-116">次の例のように、状態が正しくキャプチャされていない場合に、アナライザーにフラグを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="2394a-116">Then an analyzer can be written to flag when state is captured incorrectly, for example:</span></span>

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a><span data-ttu-id="2394a-117">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="2394a-117">Detailed design</span></span>
[design]: #detailed-design

- <span data-ttu-id="2394a-118">通常のメソッドと同じ属性構文を使用して、ラムダまたは匿名メソッドの先頭に属性を適用することができます。次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2394a-118">Using the same attribute syntax as on normal methods, attributes may be applied at the beginning of a lambda or anonymous method, for example:</span></span>

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- <span data-ttu-id="2394a-119">属性がラムダメソッドに適用されるか、いずれかの引数に適用されるかについてあいまいさを避けるために、属性は、次のように、引数の周りで使用する場合にのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="2394a-119">To avoid ambiguity as to whether an attribute applies to the lambda method or to one of the arguments, attributes may only be used when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- <span data-ttu-id="2394a-120">匿名メソッドでは、次の例のように、属性を `delegate` キーワードの前にメソッドに適用するために、かっこは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="2394a-120">With anonymous methods, parens are not needed in order to apply an attribute to the method before the `delegate` keyword, for example:</span></span>

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- <span data-ttu-id="2394a-121">次の例のように、コンマ区切りの標準構文またはフル属性構文を使用して、複数の属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="2394a-121">Multiple attributes may be applied, either via standard comma-delimited syntax or via full-attribute syntax, for example:</span></span>

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- <span data-ttu-id="2394a-122">属性は、匿名メソッドまたはラムダのパラメーターに適用できます。ただし、次の例のように、引数の周りにかっこが使用されている場合に限ります。</span><span class="sxs-lookup"><span data-stu-id="2394a-122">Attributes may be applied to the parameters to an anonymous method or lambda, but only when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- <span data-ttu-id="2394a-123">次の例のように、コンマ区切りまたはフル属性構文を使用して、匿名メソッドまたはラムダのパラメーターに複数の属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="2394a-123">Multiple attributes may be applied to the parameters of an anonymous method or lambda, using either the comma-delimited or full-attribute syntax, for example:</span></span>

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- <span data-ttu-id="2394a-124">`return`対象の属性は、次のようにラムダでも使用できます。</span><span class="sxs-lookup"><span data-stu-id="2394a-124">`return`-targeted attributes may also be used on lambdas, for example:</span></span>

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- <span data-ttu-id="2394a-125">コンパイラは、他のメソッドの場合と同様に、生成されたメソッドと引数に属性を出力します。</span><span class="sxs-lookup"><span data-stu-id="2394a-125">The compiler outputs the attributes onto the generated method and arguments to those methods as it would for any other method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="2394a-126">短所</span><span class="sxs-lookup"><span data-stu-id="2394a-126">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="2394a-127">該当なし</span><span class="sxs-lookup"><span data-stu-id="2394a-127">n/a</span></span>

## <a name="alternatives"></a><span data-ttu-id="2394a-128">代替</span><span class="sxs-lookup"><span data-stu-id="2394a-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="2394a-129">該当なし</span><span class="sxs-lookup"><span data-stu-id="2394a-129">n/a</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="2394a-130">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="2394a-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="2394a-131">該当なし</span><span class="sxs-lookup"><span data-stu-id="2394a-131">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="2394a-132">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="2394a-132">Design meetings</span></span>

<span data-ttu-id="2394a-133">該当なし</span><span class="sxs-lookup"><span data-stu-id="2394a-133">n/a</span></span>