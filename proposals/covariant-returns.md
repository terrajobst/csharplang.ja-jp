---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483458"
---
# <a name="covariant-return-types"></a><span data-ttu-id="5d94d-101">共変の戻り値の型</span><span class="sxs-lookup"><span data-stu-id="5d94d-101">covariant return types</span></span>

* <span data-ttu-id="5d94d-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="5d94d-102">[x] Proposed</span></span>
* <span data-ttu-id="5d94d-103">[] プロトタイプ: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="5d94d-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="5d94d-104">[] の実装: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="5d94d-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="5d94d-105">[] 仕様: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="5d94d-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="5d94d-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="5d94d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="5d94d-107">_共変の戻り値の型_をサポートします。</span><span class="sxs-lookup"><span data-stu-id="5d94d-107">Support _covariant return types_.</span></span> <span data-ttu-id="5d94d-108">具体的には、オーバーライドするメソッドに、オーバーライドするメソッドよりも派生型の方が多いことを許可します。</span><span class="sxs-lookup"><span data-stu-id="5d94d-108">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span>

## <a name="motivation"></a><span data-ttu-id="5d94d-109">目的</span><span class="sxs-lookup"><span data-stu-id="5d94d-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="5d94d-110">これは、オーバーライドされたメソッドと同じ型を返す必要がある言語の制約を回避するために、さまざまなメソッド名を開発する必要がある、コード内の一般的なパターンです。</span><span class="sxs-lookup"><span data-stu-id="5d94d-110">It is a common pattern in code that different method names have to be invented to work around the language constraint that overrides must return the same type as the overridden method.</span></span> <span data-ttu-id="5d94d-111">Roslyn コードベースの例については、以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5d94d-111">See below for an example from the Roslyn code base.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="5d94d-112">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="5d94d-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="5d94d-113">_共変の戻り値の型_をサポートします。</span><span class="sxs-lookup"><span data-stu-id="5d94d-113">Support _covariant return types_.</span></span> <span data-ttu-id="5d94d-114">具体的には、オーバーライドするメソッドに、オーバーライドするメソッドよりも派生型の方が多いことを許可します。</span><span class="sxs-lookup"><span data-stu-id="5d94d-114">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span> <span data-ttu-id="5d94d-115">これは、メソッドおよびプロパティに適用され、クラスおよびインターフェイスでサポートされます。</span><span class="sxs-lookup"><span data-stu-id="5d94d-115">This would apply to methods and properties, and be supported in classes and interfaces.</span></span>

<span data-ttu-id="5d94d-116">これは、ファクトリパターンで役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="5d94d-116">This would be useful in the factory pattern.</span></span> <span data-ttu-id="5d94d-117">たとえば、Roslyn コードベースでは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5d94d-117">For example, in the Roslyn code base we would have</span></span>

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

<span data-ttu-id="5d94d-118">この実装は、オーバーライドするメソッドを、基底クラスのメソッドを非表示にする "new" 仮想メソッドとしてコンパイラによって、派生クラスのメソッドの呼び出しで基本クラスのメソッドを実装する_ブリッジメソッド_として出力することになります。</span><span class="sxs-lookup"><span data-stu-id="5d94d-118">The implementation of this would be for the compiler to emit the overriding method as a "new" virtual method that hides the base class method, along with a _bridge method_ that implements the base class method with a call to the derived class method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="5d94d-119">短所</span><span class="sxs-lookup"><span data-stu-id="5d94d-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="5d94d-120">[] すべての言語の変更は、それ自体に対して支払う必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d94d-120">[ ] Every language change must pay for itself.</span></span>
- <span data-ttu-id="5d94d-121">[] 詳細継承階層の場合でも、パフォーマンスが妥当であることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d94d-121">[ ] We should ensure that the performance is reasonable, even in the case of deep inheritance hierarchies</span></span>
- <span data-ttu-id="5d94d-122">[] 以前のコンパイラから新しい IL を使用している場合でも、翻訳戦略の成果物が言語のセマンティクスに影響しないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d94d-122">[ ] We should ensure that artifacts of the translation strategy do not affect language semantics, even when consuming new IL from old compilers.</span></span>

## <a name="alternatives"></a><span data-ttu-id="5d94d-123">代替</span><span class="sxs-lookup"><span data-stu-id="5d94d-123">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="5d94d-124">ソースでは、言語ルールを少し緩和して、</span><span class="sxs-lookup"><span data-stu-id="5d94d-124">We could relax the language rules slightly to allow, in source,</span></span>

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a><span data-ttu-id="5d94d-125">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="5d94d-125">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="5d94d-126">[] この機能を使用するようにコンパイルされた Api は、以前のバージョンの言語で動作しますか。</span><span class="sxs-lookup"><span data-stu-id="5d94d-126">[ ] How will APIs that have been compiled to use this feature work in older versions of the language?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="5d94d-127">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="5d94d-127">Design meetings</span></span>

<span data-ttu-id="5d94d-128">まだありません。</span><span class="sxs-lookup"><span data-stu-id="5d94d-128">None yet.</span></span> <span data-ttu-id="5d94d-129"><https://github.com/dotnet/roslyn/issues/357>にはいくつかの議論がありました。</span><span class="sxs-lookup"><span data-stu-id="5d94d-129">There has been some discussion at <https://github.com/dotnet/roslyn/issues/357>.</span></span>