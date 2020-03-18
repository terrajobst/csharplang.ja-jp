---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483974"
---
# <a name="pattern-based-using-and-using-declarations"></a><span data-ttu-id="b4f9c-101">"パターンベースの使用" と "宣言の使用"</span><span class="sxs-lookup"><span data-stu-id="b4f9c-101">"pattern-based using" and "using declarations"</span></span>

## <a name="summary"></a><span data-ttu-id="b4f9c-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="b4f9c-102">Summary</span></span>

<span data-ttu-id="b4f9c-103">この言語では、リソース管理をより簡単にするために、`using` ステートメントの周囲に2つの新機能が追加されています。 `using` は `IDisposable` に加えて破棄可能なパターンを認識し、言語に `using` 宣言を追加します。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-103">The language will add two new capabilities around the `using` statement in order to make resource management simpler: `using` should recognize a disposable pattern in addition to `IDisposable` and add a `using` declaration to the language.</span></span>

## <a name="motivation"></a><span data-ttu-id="b4f9c-104">目的</span><span class="sxs-lookup"><span data-stu-id="b4f9c-104">Motivation</span></span>

<span data-ttu-id="b4f9c-105">`using` ステートメントは、今日のリソース管理のための効果的なツールですが、かなりの手続きが必要です。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-105">The `using` statement is an effective tool for resource management today but it requires quite a bit of ceremony.</span></span> <span data-ttu-id="b4f9c-106">多数のリソースを管理するメソッドでは、一連の `using` ステートメントを使用して、構文的に行き詰まるを取得できます。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-106">Methods that have a number of resources to manage can get syntactically bogged down with a series of `using` statements.</span></span> <span data-ttu-id="b4f9c-107">この構文の負担は、ほとんどのコーディングスタイルのガイドラインで、このシナリオで中かっこを囲む例外が明示的に設定されているためです。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-107">This syntax burden is enough that most coding style guidelines explicitly have an exception around braces for this scenario.</span></span> 

<span data-ttu-id="b4f9c-108">`using` 宣言では、この方法の多くを削除C#し、リソース管理ブロックを含む他の言語と同等のものを取得します。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-108">The `using` declaration removes much of the ceremony here and gets C# on par with other languages that include resource management blocks.</span></span> <span data-ttu-id="b4f9c-109">さらに、パターンベースの `using` を使用すると、開発者はここで使用できる型のセットを展開できます。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-109">Additionally the pattern-based `using` lets developers expand the set of types that can participate here.</span></span> <span data-ttu-id="b4f9c-110">多くの場合、`using` のステートメントで値を使用できるようにするためにのみ存在するラッパー型を作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-110">In many cases removing the need to create wrapper types that only exist to allow for a values use in a `using` statement.</span></span> 

<span data-ttu-id="b4f9c-111">これらの機能を組み合わせることにより、開発者は `using` を適用できるシナリオを簡略化および拡張できます。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-111">Together these features allow developers to simplify and expand the scenarios where `using` can be applied.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b4f9c-112">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="b4f9c-112">Detailed Design</span></span> 

### <a name="using-declaration"></a><span data-ttu-id="b4f9c-113">using 宣言</span><span class="sxs-lookup"><span data-stu-id="b4f9c-113">using declaration</span></span>

<span data-ttu-id="b4f9c-114">この言語では、`using` をローカル変数宣言に追加できます。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-114">The language will allow for `using` to be added to a local variable declaration.</span></span> <span data-ttu-id="b4f9c-115">このような宣言は、同じ場所にある `using` ステートメントで変数を宣言するのと同じ効果があります。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-115">Such a declaration will have the same effect as declaring the variable in a `using` statement at the same location.</span></span>

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

<span data-ttu-id="b4f9c-116">`using` ローカルの有効期間は、宣言されているスコープの末尾まで拡張されます。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-116">The lifetime of a `using` local will extend to the end of the scope in which it is declared.</span></span> <span data-ttu-id="b4f9c-117">`using` ローカル変数は、宣言された順序と逆の順序で破棄されます。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-117">The `using` locals will then be disposed in the reverse order in which they are declared.</span></span> 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

<span data-ttu-id="b4f9c-118">`goto`、または `using` 宣言の表面でのその他の制御フローコンストラクトに関する制限はありません。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-118">There are no restrictions around `goto`, or any other control flow construct in the face of a `using` declaration.</span></span> <span data-ttu-id="b4f9c-119">代わりに、コードは同等の `using` ステートメントの場合と同様に動作します。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-119">Instead the code acts just as it would for the equivalent `using` statement:</span></span>

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

<span data-ttu-id="b4f9c-120">`using` ローカル宣言で宣言されたローカルは、暗黙的に読み取り専用になります。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-120">A local declared in a `using` local declaration will be implicitly read-only.</span></span> <span data-ttu-id="b4f9c-121">これは、`using` ステートメントで宣言されたローカルの動作と一致します。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-121">This matches the behavior of locals declared in a `using` statement.</span></span> 

<span data-ttu-id="b4f9c-122">`using` 宣言の言語文法は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-122">The language grammar for `using` declarations will be the following:</span></span>

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

<span data-ttu-id="b4f9c-123">`using` 宣言に関する制限事項は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-123">Restrictions around `using` declaration:</span></span>

- <span data-ttu-id="b4f9c-124">`case` ラベル内に直接指定することはできませんが、代わりに `case` ラベル内のブロック内にある必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-124">May not appear directly inside a `case` label but instead must be within a block inside the `case` label.</span></span>
- <span data-ttu-id="b4f9c-125">`out` 変数宣言の一部として表示されない場合があります。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-125">May not appear as part of an `out` variable declaration.</span></span> 
- <span data-ttu-id="b4f9c-126">各宣言子の初期化子が必要です。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-126">Must have an initializer for each declarator.</span></span>
- <span data-ttu-id="b4f9c-127">ローカル型は `IDisposable` に暗黙的に変換できるか、または `using` パターンを満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-127">The local type must be implicitly convertible to `IDisposable` or fulfill the `using` pattern.</span></span>

### <a name="pattern-based-using"></a><span data-ttu-id="b4f9c-128">パターンベースの使用</span><span class="sxs-lookup"><span data-stu-id="b4f9c-128">pattern-based using</span></span>

<span data-ttu-id="b4f9c-129">この言語は、アクセス可能な `Dispose` インスタンスメソッドを持つ型である、破棄可能なパターンの概念を追加します。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-129">The language will add the notion of a disposable pattern: that is a type which has an accessible `Dispose` instance method.</span></span> <span data-ttu-id="b4f9c-130">破棄可能なパターンに適合する型は、`IDisposable`を実装する必要がない `using` のステートメントまたは宣言に含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-130">Types which fit the disposable pattern can participate in a `using` statement or declaration without being required to implement `IDisposable`.</span></span> 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

<span data-ttu-id="b4f9c-131">これにより、開発者はいくつかの新しいシナリオで `using` を活用できます。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-131">This will allow developers to leverage `using` in a number of new scenarios:</span></span>

- <span data-ttu-id="b4f9c-132">`ref struct`: これらの型は現在、インターフェイスを実装することができないため、`using` ステートメントに参加することはできません。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-132">`ref struct`: These types can't implement interfaces today and hence can't participate in `using` statements.</span></span>
- <span data-ttu-id="b4f9c-133">拡張メソッドを使用すると、開発者は、`using` ステートメントに参加する他のアセンブリの型を拡張できます。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-133">Extension methods will allow developers to augment types in other assemblies to participate in `using` statements.</span></span>

<span data-ttu-id="b4f9c-134">型を暗黙的に `IDisposable` に変換して、破棄可能なパターンに適合させることができる場合、`IDisposable` が優先されます。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-134">In the situation where a type can be implicitly converted to `IDisposable` and also fits the disposable pattern, then `IDisposable` will be preferred.</span></span> <span data-ttu-id="b4f9c-135">これは `foreach` の反対のアプローチ (インターフェイスよりも優先されるパターン) を使用しますが、下位互換性を確保するために必要です。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-135">While this takes the opposite approach of `foreach` (pattern preferred over interface) it is necessary for backwards compatibility.</span></span>

<span data-ttu-id="b4f9c-136">従来の `using` ステートメントと同じ制限がここでも適用されます。 `using` で宣言されたローカル変数は読み取り専用で、`null` の値によって例外がスローされることはありません。などです。コード生成が異なるのは、Dispose を呼び出す前に `IDisposable` するキャストがないということだけです。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-136">The same restrictions from a traditional `using` statement apply here as well: local variables declared in the `using` are read-only, a `null` value will not cause an exception to be thrown, etc ... The code generation will be different only in that there will not be a cast to `IDisposable` before calling Dispose:</span></span>

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

<span data-ttu-id="b4f9c-137">破棄可能なパターンに合わせるには、`Dispose` メソッドにアクセスできる必要があります。また、パラメーター化されていない `void` 戻り値の型を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-137">In order to fit the disposable pattern the `Dispose` method must be accessible, parameterless and have a `void` return type.</span></span> <span data-ttu-id="b4f9c-138">その他の制限はありません。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-138">There are no other restrictions.</span></span> <span data-ttu-id="b4f9c-139">これは、明示的に拡張メソッドを使用できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-139">This explicitly means that extension methods can be used here.</span></span>

## <a name="considerations"></a><span data-ttu-id="b4f9c-140">考慮事項</span><span class="sxs-lookup"><span data-stu-id="b4f9c-140">Considerations</span></span>

### <a name="case-labels-without-blocks"></a><span data-ttu-id="b4f9c-141">ブロックのない case ラベル</span><span class="sxs-lookup"><span data-stu-id="b4f9c-141">case labels without blocks</span></span>

<span data-ttu-id="b4f9c-142">実際の有効期間に関係があるため、`using declaration` は `case` ラベル内では直接無効です。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-142">A `using declaration` is illegal directly inside a `case` label due to complications around its actual lifetime.</span></span> <span data-ttu-id="b4f9c-143">考えられる解決策の1つは、同じ場所の `out var` と同じ有効期間を与えることです。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-143">One potential solution is to simply give it the same lifetime as an `out var` in the same location.</span></span> <span data-ttu-id="b4f9c-144">これは、機能の実装について複雑さが増し、回避策 (`case` ラベルにブロックを追加するだけで済みます) がこのルートの作成には合わないと見なされました。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-144">It was deemed the extra complexity to the feature implementation and the ease of the work around (just add a block to the `case` label) didn't justify taking this route.</span></span>

## <a name="future-expansions"></a><span data-ttu-id="b4f9c-145">将来の展開</span><span class="sxs-lookup"><span data-stu-id="b4f9c-145">Future Expansions</span></span>

### <a name="fixed-locals"></a><span data-ttu-id="b4f9c-146">固定ローカル</span><span class="sxs-lookup"><span data-stu-id="b4f9c-146">fixed locals</span></span>

<span data-ttu-id="b4f9c-147">`fixed` ステートメントには、`using` ローカルを持つことを実現する `using` ステートメントのすべてのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-147">A `fixed` statement has all of the properties of `using` statements that motivated the ability to have `using` locals.</span></span> <span data-ttu-id="b4f9c-148">この機能を拡張してローカル `fixed` する場合も考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-148">Consideration should be given to extending this feature to `fixed` locals as well.</span></span> <span data-ttu-id="b4f9c-149">有効期間と順序の規則は、ここで `using` と `fixed` にも同様に適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4f9c-149">The lifetime and ordering rules should apply equally well for `using` and `fixed` here.</span></span>
