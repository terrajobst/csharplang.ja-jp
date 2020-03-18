---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2019
ms.locfileid: "79484046"
---
# <a name="permit-stackalloc-in-nested-contexts"></a><span data-ttu-id="07891-101">入れ子になったコンテキストでの `stackalloc` を許可する</span><span class="sxs-lookup"><span data-stu-id="07891-101">Permit `stackalloc` in nested contexts</span></span>

### <a name="stack-allocation"></a><span data-ttu-id="07891-102">スタック割り当て</span><span class="sxs-lookup"><span data-stu-id="07891-102">Stack allocation</span></span>

<span data-ttu-id="07891-103">C#言語仕様のセクション[*スタック割り当て*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation)を変更して、`stackalloc` 式が表示される可能性がある場所を緩めることができます。</span><span class="sxs-lookup"><span data-stu-id="07891-103">We modify the section [*Stack allocation*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) of the C# language specification to relax the places when a `stackalloc` expression may appear.</span></span> <span data-ttu-id="07891-104">削除</span><span class="sxs-lookup"><span data-stu-id="07891-104">We delete</span></span>

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="07891-105">をに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="07891-105">and replace them with</span></span>

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

<span data-ttu-id="07891-106">*Stackalloc_initializer*への*array_initializer*の追加 (およびインデックス式のオプションの作成) は[7.3 C#の拡張機能](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md)であり、ここでは説明しません。</span><span class="sxs-lookup"><span data-stu-id="07891-106">Note that the addition of an *array_initializer* to *stackalloc_initializer* (and making the index expression optional) was an [extension in C# 7.3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) and is not described here.</span></span>

<span data-ttu-id="07891-107">`stackalloc` 式の*要素の型*は、stackalloc 式に指定された*unmanaged_type* (存在する場合)、または*array_initializer*の要素間の共通型である場合は、それ以外の場合はです。</span><span class="sxs-lookup"><span data-stu-id="07891-107">The *element type* of the `stackalloc` expression is the *unmanaged_type* named in the stackalloc expression, if any, or the common type among the elements of the *array_initializer* otherwise.</span></span>

<span data-ttu-id="07891-108">*要素型*が `K` *stackalloc_initializer*の型は、その構文コンテキストによって異なります。</span><span class="sxs-lookup"><span data-stu-id="07891-108">The type of the *stackalloc_initializer* with *element type* `K` depends on its syntactic context:</span></span>
- <span data-ttu-id="07891-109">*Stackalloc_initializer*が*local_variable_declaration*ステートメントまたは*for_initializer*の*local_variable_initializer*として直接表示される場合、その型は `K*`になります。</span><span class="sxs-lookup"><span data-stu-id="07891-109">If the *stackalloc_initializer* appears directly as the *local_variable_initializer* of a *local_variable_declaration* statement or a *for_initializer*, then its type is `K*`.</span></span>
- <span data-ttu-id="07891-110">それ以外の場合は、その型が `System.Span<K>`になります。</span><span class="sxs-lookup"><span data-stu-id="07891-110">Otherwise its type is `System.Span<K>`.</span></span>

### <a name="stackalloc-conversion"></a><span data-ttu-id="07891-111">Stackalloc の変換</span><span class="sxs-lookup"><span data-stu-id="07891-111">Stackalloc Conversion</span></span>

<span data-ttu-id="07891-112">*Stackalloc 変換*は、新しい組み込みの式からの暗黙的な変換です。</span><span class="sxs-lookup"><span data-stu-id="07891-112">The *stackalloc conversion* is a new built-in implicit conversion from expression.</span></span> <span data-ttu-id="07891-113">*Stackalloc_initializer*の型が `K*`場合、 *stackalloc_initializer*から型 `System.Span<K>`への暗黙的な*stackalloc 変換*が存在します。</span><span class="sxs-lookup"><span data-stu-id="07891-113">When the type of a *stackalloc_initializer* is `K*`, there is an implicit *stackalloc conversion* from the *stackalloc_initializer* to the type `System.Span<K>`.</span></span>
