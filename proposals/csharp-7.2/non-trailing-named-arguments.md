---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483644"
---
# <a name="non-trailing-named-arguments"></a><span data-ttu-id="31e16-101">末尾以外の名前付き引数</span><span class="sxs-lookup"><span data-stu-id="31e16-101">Non-trailing named arguments</span></span>

## <a name="summary"></a><span data-ttu-id="31e16-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="31e16-102">Summary</span></span>
[summary]: #summary
<span data-ttu-id="31e16-103">名前付き引数は、正しい位置で使用されている限り、末尾以外の位置で使用できます。</span><span class="sxs-lookup"><span data-stu-id="31e16-103">Allow named arguments to be used in non-trailing position, as long as they are used in their correct position.</span></span> <span data-ttu-id="31e16-104">(例: `DoSomething(isEmployed:true, name, age);`)。</span><span class="sxs-lookup"><span data-stu-id="31e16-104">For example: `DoSomething(isEmployed:true, name, age);`.</span></span>

## <a name="motivation"></a><span data-ttu-id="31e16-105">目的</span><span class="sxs-lookup"><span data-stu-id="31e16-105">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="31e16-106">主な動機は、冗長な情報を入力しないようにすることです。</span><span class="sxs-lookup"><span data-stu-id="31e16-106">The main motivation is to avoid typing redundant information.</span></span> <span data-ttu-id="31e16-107">通常は、引数を順序どおりに渡すのではなく、コードを明確にするために、リテラル (`null`、`true`など) に名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="31e16-107">It is common to name an argument that is a literal (such as `null`, `true`) for the purpose of clarifying the code, rather than of passing arguments out-of-order.</span></span>
<span data-ttu-id="31e16-108">次のすべての引数にも名前が付いている場合を除き、現在のところ (`CS1738`) は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="31e16-108">That is currently disallowed (`CS1738`) unless all the following arguments are also named.</span></span>

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

<span data-ttu-id="31e16-109">その他の例:</span><span class="sxs-lookup"><span data-stu-id="31e16-109">Some additional examples:</span></span>
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

<span data-ttu-id="31e16-110">これは、params でも機能します。</span><span class="sxs-lookup"><span data-stu-id="31e16-110">This would also work with params:</span></span>
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a><span data-ttu-id="31e16-111">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="31e16-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="31e16-112">7\.5.1 (引数リスト) では、現在の仕様では次のように示されています。</span><span class="sxs-lookup"><span data-stu-id="31e16-112">In §7.5.1 (Argument lists), the spec currently says:</span></span>
> <span data-ttu-id="31e16-113">引数*名*を指定*した引数*は、__名前付き引数__と呼ばれますが、引数*名*のない*引数*は__位置引数__と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="31e16-113">An *argument* with an *argument-name* is referred to as a __named argument__, whereas an *argument* without an *argument-name* is a __positional argument__.</span></span> <span data-ttu-id="31e16-114">*引数リスト*の名前付き引数の後に位置引数を指定すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="31e16-114">It is an error for a positional argument to appear after a named argument in an *argument-list*.</span></span>

<span data-ttu-id="31e16-115">この提案では、このエラーを削除し、引数に対応するパラメーター (7.5.1.1 を参照) を検索するためのルールを更新します。</span><span class="sxs-lookup"><span data-stu-id="31e16-115">The proposal is to remove this error and update the rules for finding the corresponding parameter for an argument (§7.5.1.1):</span></span>

<span data-ttu-id="31e16-116">引数: インスタンスコンストラクター、メソッド、インデクサー、およびデリゲートのリスト。</span><span class="sxs-lookup"><span data-stu-id="31e16-116">Arguments in the argument-list of instance constructors, methods, indexers and delegates:</span></span>
- <span data-ttu-id="31e16-117">[既存のルール]</span><span class="sxs-lookup"><span data-stu-id="31e16-117">[existing rules]</span></span>
- <span data-ttu-id="31e16-118">無名の名前付き引数または名前付き params 引数の後にある場合、名前のない引数はパラメーターなしに対応します。</span><span class="sxs-lookup"><span data-stu-id="31e16-118">An unnamed argument corresponds to no parameter when it is after an out-of-position named argument or a named params argument.</span></span>

<span data-ttu-id="31e16-119">特に、これにより `M(c: false, valueB);`で `void M(bool a = true, bool b = true, bool c = true, );` を呼び出すことができなくなります。</span><span class="sxs-lookup"><span data-stu-id="31e16-119">In particular, this prevents invoking `void M(bool a = true, bool b = true, bool c = true, );` with `M(c: false, valueB);`.</span></span> <span data-ttu-id="31e16-120">最初の引数は、位置を指定しないで使用されます (引数は最初の位置で使用されますが、"c" という名前のパラメーターは3番目の位置にあります)。したがって、次の引数はという名前にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="31e16-120">The first argument is used out-of-position (the argument is used in first position, but the parameter named "c" is in third position), so the following arguments should be named.</span></span>

<span data-ttu-id="31e16-121">言い換えると、末尾以外の名前付き引数は、名前と位置が同じ対応パラメーターを検索する場合にのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="31e16-121">In other words, non-trailing named arguments are only allowed when the name and the position result in finding the same corresponding parameter.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="31e16-122">短所</span><span class="sxs-lookup"><span data-stu-id="31e16-122">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="31e16-123">この提案では、オーバーロードの解決で名前付き引数を使用して、既存の微妙な exacerbates を行います。</span><span class="sxs-lookup"><span data-stu-id="31e16-123">This proposal exacerbates existing subtleties with named arguments in overload resolution.</span></span> <span data-ttu-id="31e16-124">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="31e16-124">For instance:</span></span>

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

<span data-ttu-id="31e16-125">この状況は、次のようにパラメーターを交換することで取得できます。</span><span class="sxs-lookup"><span data-stu-id="31e16-125">You could get this situation today by swapping the parameters:</span></span>

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

<span data-ttu-id="31e16-126">同様に、`void M(int a, int b)` と `void M(int x, string y)`の2つのメソッドがある場合、誤った呼び出し `M(x: 1, 2)` によって、2番目のオーバーロードに基づく診断が生成されます ("int ' から ' string ' に変換することはできません)。</span><span class="sxs-lookup"><span data-stu-id="31e16-126">Similarly, if you have two methods `void M(int a, int b)` and `void M(int x, string y)`, the mistaken invocation `M(x: 1, 2)` will produce a diagnostic based on the second overload ("cannot convert from 'int' to 'string'").</span></span> <span data-ttu-id="31e16-127">名前付き引数が末尾の位置で使用されている場合、この問題は既に存在しています。</span><span class="sxs-lookup"><span data-stu-id="31e16-127">This problem already exists when the named argument is used in a trailing position.</span></span>

## <a name="alternatives"></a><span data-ttu-id="31e16-128">代替</span><span class="sxs-lookup"><span data-stu-id="31e16-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="31e16-129">考慮すべき選択肢がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="31e16-129">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="31e16-130">現状</span><span class="sxs-lookup"><span data-stu-id="31e16-130">The status quo</span></span>
- <span data-ttu-id="31e16-131">中央に特定の名前を入力するときに、末尾の引数のすべての名前を入力するための IDE サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="31e16-131">Providing IDE assistance to fill-in all the names of trailing arguments when you type specific a name in the middle.</span></span>

<span data-ttu-id="31e16-132">どちらも、引数リストの先頭にリテラルの名前が1つ必要な場合でも、複数の名前付き引数が導入されるため、詳細度が高くなります。</span><span class="sxs-lookup"><span data-stu-id="31e16-132">Both of those suffer from more verbosity, as they introduce multiple named arguments even if you just need one name of a literal at the beginning of the argument list.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="31e16-133">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="31e16-133">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="31e16-134">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="31e16-134">Design meetings</span></span>
[ldm]: #ldm
<span data-ttu-id="31e16-135">この機能は、LDM では、2017年5月16日に承認され、原則 (提案/プロトタイプに移行することは ok) について簡単に説明しました。</span><span class="sxs-lookup"><span data-stu-id="31e16-135">The feature was briefly discussed in LDM on May 16th 2017, with approval in principle (ok to move to proposal/prototype).</span></span> <span data-ttu-id="31e16-136">また、2017年6月28日にも簡単に説明しました。</span><span class="sxs-lookup"><span data-stu-id="31e16-136">It was also briefly discussed on June 28th 2017.</span></span>

<span data-ttu-id="31e16-137">Championed の問題に関連する最初のディスカッションに関連 https://github.com/dotnet/csharplang/issues/518 https://github.com/dotnet/csharplang/issues/570</span><span class="sxs-lookup"><span data-stu-id="31e16-137">Relates to initial discussion https://github.com/dotnet/csharplang/issues/518 Relates to championed issue https://github.com/dotnet/csharplang/issues/570</span></span>
