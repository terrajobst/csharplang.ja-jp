---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "79483998"
---
# <a name="static-lambdas"></a><span data-ttu-id="d857b-101">静的ラムダ</span><span class="sxs-lookup"><span data-stu-id="d857b-101">Static lambdas</span></span>

## <a name="summary"></a><span data-ttu-id="d857b-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="d857b-102">Summary</span></span>

<span data-ttu-id="d857b-103">外側のスコープからの状態のキャプチャを禁止するラムダをサポートします。</span><span class="sxs-lookup"><span data-stu-id="d857b-103">Support lambdas that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="d857b-104">目的</span><span class="sxs-lookup"><span data-stu-id="d857b-104">Motivation</span></span>

<span data-ttu-id="d857b-105">外側のコンテキストから状態を誤ってキャプチャしないようにします。</span><span class="sxs-lookup"><span data-stu-id="d857b-105">Avoid unintentionally capturing state from the enclosing context.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d857b-106">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="d857b-106">Detailed design</span></span>

<span data-ttu-id="d857b-107">`static` を持つラムダは、外側のスコープから状態をキャプチャできません。</span><span class="sxs-lookup"><span data-stu-id="d857b-107">A lambdas with `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="d857b-108">その結果、外側のスコープのローカル、パラメーター、および `this` は、`static` ラムダ内では使用できません。</span><span class="sxs-lookup"><span data-stu-id="d857b-108">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` lambda.</span></span>

<span data-ttu-id="d857b-109">`static` のラムダは、暗黙的または明示的な `this` または `base` 参照からインスタンスメンバーを参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="d857b-109">A `static` lambda cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="d857b-110">`static` のラムダは、外側のスコープから `static` メンバーを参照できます。</span><span class="sxs-lookup"><span data-stu-id="d857b-110">A `static` lambda may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="d857b-111">`static` のラムダでは、外側のスコープから `constant` 定義を参照できます。</span><span class="sxs-lookup"><span data-stu-id="d857b-111">A `static` lambda may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="d857b-112">`static` ラムダ内の `nameof()` は、外側のスコープからローカル、パラメーター、または `this` または `base` を参照できます。</span><span class="sxs-lookup"><span data-stu-id="d857b-112">`nameof()` in a `static` lambda may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="d857b-113">外側のスコープ内の `private` メンバーのアクセシビリティ規則は、`static` および非`static` のラムダでも同じです。</span><span class="sxs-lookup"><span data-stu-id="d857b-113">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` lambdas.</span></span>

<span data-ttu-id="d857b-114">`static` のラムダ定義をメタデータの `static` メソッドとして出力するかどうかは保証されません。</span><span class="sxs-lookup"><span data-stu-id="d857b-114">No guarantee is made as to whether a `static` lambda definition is emitted as a `static` method in metadata.</span></span> <span data-ttu-id="d857b-115">これは、最適化のためにコンパイラの実装に残されます。</span><span class="sxs-lookup"><span data-stu-id="d857b-115">This is left up to the compiler implementation to optimize.</span></span>

<span data-ttu-id="d857b-116">非`static` ローカル関数またはラムダは、外側の `static` ラムダから状態をキャプチャできますが、外側の `static` ラムダの外側で状態をキャプチャすることはできません。</span><span class="sxs-lookup"><span data-stu-id="d857b-116">A non-`static` local function or lambda can capture state from an enclosing `static` lambda but cannot capture state outside the enclosing `static` lambda.</span></span>

<span data-ttu-id="d857b-117">`static` のラムダは、式ツリーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="d857b-117">A `static` lambda can be used in an expression tree.</span></span>

<span data-ttu-id="d857b-118">有効なプログラムでラムダから `static` 修飾子を削除しても、プログラムの意味は変わりません。</span><span class="sxs-lookup"><span data-stu-id="d857b-118">Removing the `static` modifier from a lambda in a valid program does not change the meaning of the program.</span></span>
