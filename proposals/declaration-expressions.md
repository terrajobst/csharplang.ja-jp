---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483566"
---
# <a name="declaration-expressions"></a><span data-ttu-id="869f9-101">宣言式</span><span class="sxs-lookup"><span data-stu-id="869f9-101">Declaration expressions</span></span>

<span data-ttu-id="869f9-102">式としての宣言の割り当てをサポートします。</span><span class="sxs-lookup"><span data-stu-id="869f9-102">Support declaration assignments as expressions.</span></span>

## <a name="motivation"></a><span data-ttu-id="869f9-103">目的</span><span class="sxs-lookup"><span data-stu-id="869f9-103">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="869f9-104">多くの場合、宣言の時点で初期化を許可し、コードを簡略化し、`var` を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="869f9-104">Allow initialization at the point of declaration in more cases, simplifying code, and allowing `var` to be used.</span></span>

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

<span data-ttu-id="869f9-105">`out var`に似た `ref` 引数の宣言を許可します。</span><span class="sxs-lookup"><span data-stu-id="869f9-105">Allow declarations for `ref` arguments, similar to `out var`.</span></span>

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a><span data-ttu-id="869f9-106">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="869f9-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="869f9-107">式は、宣言の割り当てを含むように拡張されます。</span><span class="sxs-lookup"><span data-stu-id="869f9-107">Expressions are extended to include declaration assignment.</span></span> <span data-ttu-id="869f9-108">優先順位は割り当てと同じです。</span><span class="sxs-lookup"><span data-stu-id="869f9-108">Precedence is the same as assignment.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

<span data-ttu-id="869f9-109">宣言の割り当ては、単一のローカルのです。</span><span class="sxs-lookup"><span data-stu-id="869f9-109">The declaration assignment is of a single local.</span></span>

<span data-ttu-id="869f9-110">宣言代入式の型は、宣言の型です。</span><span class="sxs-lookup"><span data-stu-id="869f9-110">The type of a declaration assignment expression is the type of the declaration.</span></span>
<span data-ttu-id="869f9-111">型が `var`の場合、推論される型は初期化式の型になります。</span><span class="sxs-lookup"><span data-stu-id="869f9-111">If the type is `var`, the inferred type is the type of the initializing expression.</span></span> 

<span data-ttu-id="869f9-112">宣言代入式は、特に `ref` 引数値の左辺値にすることができます。</span><span class="sxs-lookup"><span data-stu-id="869f9-112">The declaration assignment expression may be an l-value, for `ref` argument values in particular.</span></span>

<span data-ttu-id="869f9-113">宣言代入式で値型が宣言され、式が右辺値の場合、式の値はコピーになります。</span><span class="sxs-lookup"><span data-stu-id="869f9-113">If the declaration assignment expression declares a value type, and the expression is an r-value, the value of the expression is a copy.</span></span>

<span data-ttu-id="869f9-114">宣言代入式では、ローカル `ref` を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="869f9-114">The declaration assignment expression may declare a `ref` local.</span></span>
<span data-ttu-id="869f9-115">`ref` 引数の宣言式に `ref` が使用されている場合、あいまいさが発生します。</span><span class="sxs-lookup"><span data-stu-id="869f9-115">There is an ambiguity when `ref` is used for a declaration expression in a `ref` argument.</span></span>
<span data-ttu-id="869f9-116">ローカル変数初期化子は、宣言がローカル `ref` であるかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="869f9-116">The local variable initializer determines whether the declaration is a `ref` local.</span></span>

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

<span data-ttu-id="869f9-117">宣言代入式で宣言されたローカルのスコープは、C # 7.0 の対応する宣言式のスコープと同じです。</span><span class="sxs-lookup"><span data-stu-id="869f9-117">The scope of locals declared in declaration assignment expressions is the same the scope of corresponding declaration expressions from C#7.0.</span></span>

<span data-ttu-id="869f9-118">宣言式の前にあるローカルのをテキストで参照すると、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="869f9-118">It is a compile time error to refer to a local in text preceding the declaration expression.</span></span>

## <a name="alternatives"></a><span data-ttu-id="869f9-119">代替</span><span class="sxs-lookup"><span data-stu-id="869f9-119">Alternatives</span></span>
[alternatives]: #alternatives
<span data-ttu-id="869f9-120">変更はありません。</span><span class="sxs-lookup"><span data-stu-id="869f9-120">No change.</span></span> <span data-ttu-id="869f9-121">この機能は、構文の省略形にすぎません。</span><span class="sxs-lookup"><span data-stu-id="869f9-121">This feature is just syntactic shorthand after all.</span></span>

<span data-ttu-id="869f9-122">一般的なシーケンス式については、「 [#377](https://github.com/dotnet/csharplang/issues/377)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="869f9-122">More general sequence expressions: see [#377](https://github.com/dotnet/csharplang/issues/377).</span></span>

<span data-ttu-id="869f9-123">`var` を使用できるようにするには、`var` ローカルの個別の宣言と割り当てを許可し、すべてのコードパスからの割り当てから型を推論します。</span><span class="sxs-lookup"><span data-stu-id="869f9-123">To allow use of `var` in more cases, allow separate declaration and assignment of `var` locals, and infer the type from assignments from all code paths.</span></span>

## <a name="see-also"></a><span data-ttu-id="869f9-124">参照</span><span class="sxs-lookup"><span data-stu-id="869f9-124">See also</span></span>
[see-also]: #see-also
<span data-ttu-id="869f9-125">[#595](https://github.com/dotnet/csharplang/issues/595)の「基本宣言式」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="869f9-125">See Basic Declaration Expression in [#595](https://github.com/dotnet/csharplang/issues/595).</span></span>

<span data-ttu-id="869f9-126">[分解](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md)機能の「分解宣言」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="869f9-126">See Deconstruction Declaration in the [deconstruction](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) feature.</span></span>
