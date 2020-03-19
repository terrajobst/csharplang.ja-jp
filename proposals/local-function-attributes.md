---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509493"
---
# <a name="attributes-on-local-functions"></a><span data-ttu-id="fdd73-101">ローカル関数の属性</span><span class="sxs-lookup"><span data-stu-id="fdd73-101">Attributes on local functions</span></span>

## <a name="attributes"></a><span data-ttu-id="fdd73-102">属性</span><span class="sxs-lookup"><span data-stu-id="fdd73-102">Attributes</span></span>

<span data-ttu-id="fdd73-103">ローカル関数宣言で[属性](../spec/attributes.md)を使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="fdd73-103">Local function declarations are now permitted to have [attributes](../spec/attributes.md).</span></span> <span data-ttu-id="fdd73-104">ローカル関数のパラメーターと型パラメーターも属性を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="fdd73-104">Parameters and type parameters on local functions are also allowed to have attributes.</span></span>

<span data-ttu-id="fdd73-105">メソッド、パラメーター、またはその型パラメーターに適用されるときに意味が指定された属性は、ローカル関数、パラメーター、またはその型パラメーターにそれぞれ適用される場合と同じ意味になります。</span><span class="sxs-lookup"><span data-stu-id="fdd73-105">Attributes with a specified meaning when applied to a method, its parameters, or its type parameters will have the same meaning when applied to a local function, its parameters, or its type parameters, respectively.</span></span>

<span data-ttu-id="fdd73-106">ローカル関数は、`[ConditionalAttribute]`で修飾することによって、条件付き[メソッド](../spec/attributes.md#the-conditional-attribute)と同じ意味で条件を設定できます。</span><span class="sxs-lookup"><span data-stu-id="fdd73-106">A local function can be made conditional in the same sense as a [conditional method](../spec/attributes.md#the-conditional-attribute) by decorating it with a `[ConditionalAttribute]`.</span></span> <span data-ttu-id="fdd73-107">条件付きローカル関数も `static`する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fdd73-107">A conditional local function must also be `static`.</span></span> <span data-ttu-id="fdd73-108">条件付きメソッドに対するすべての制限は、条件付きローカル関数にも適用されます。これには、戻り値の型を `void`する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fdd73-108">All restrictions on conditional methods also apply to conditional local functions, including that the return type must be `void`.</span></span>

## <a name="extern"></a><span data-ttu-id="fdd73-109">不十分</span><span class="sxs-lookup"><span data-stu-id="fdd73-109">Extern</span></span>

<span data-ttu-id="fdd73-110">ローカル関数で `extern` 修飾子が許可されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="fdd73-110">The `extern` modifier is now permitted on local functions.</span></span> <span data-ttu-id="fdd73-111">これにより、外部[メソッド](../spec/classes.md#external-methods)と同じ意味でローカル関数が使用されるようになります。</span><span class="sxs-lookup"><span data-stu-id="fdd73-111">This makes the local function external in the same sense as an [external method](../spec/classes.md#external-methods).</span></span>

<span data-ttu-id="fdd73-112">外部メソッドと同様に、外部ローカル関数の*ローカル関数本体*はセミコロンである必要があります。</span><span class="sxs-lookup"><span data-stu-id="fdd73-112">Similarly to an external method, the *local-function-body* of an external local function must be a semicolon.</span></span> <span data-ttu-id="fdd73-113">セミコロンの*ローカル関数本体*は、外部ローカル関数でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="fdd73-113">A semicolon *local-function-body* is only permitted on an external local function.</span></span> 

<span data-ttu-id="fdd73-114">外部ローカル関数も `static`する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fdd73-114">An external local function must also be `static`.</span></span>

## <a name="syntax"></a><span data-ttu-id="fdd73-115">構文</span><span class="sxs-lookup"><span data-stu-id="fdd73-115">Syntax</span></span>

<span data-ttu-id="fdd73-116">[ローカル関数の文法](csharp-7.0/local-functions.md#syntax-grammar)は、次のように変更されます。</span><span class="sxs-lookup"><span data-stu-id="fdd73-116">The [local functions grammar](csharp-7.0/local-functions.md#syntax-grammar) is modified as follows:</span></span>
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
