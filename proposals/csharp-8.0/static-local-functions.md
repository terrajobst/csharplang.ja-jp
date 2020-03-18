---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2019
ms.locfileid: "79484010"
---
# <a name="static-local-functions"></a><span data-ttu-id="51081-101">静的ローカル関数</span><span class="sxs-lookup"><span data-stu-id="51081-101">Static local functions</span></span>

## <a name="summary"></a><span data-ttu-id="51081-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="51081-102">Summary</span></span>

<span data-ttu-id="51081-103">外側のスコープからの状態のキャプチャを禁止するローカル関数をサポートします。</span><span class="sxs-lookup"><span data-stu-id="51081-103">Support local functions that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="51081-104">目的</span><span class="sxs-lookup"><span data-stu-id="51081-104">Motivation</span></span>

<span data-ttu-id="51081-105">外側のコンテキストから状態を誤ってキャプチャしないようにします。</span><span class="sxs-lookup"><span data-stu-id="51081-105">Avoid unintentionally capturing state from the enclosing context.</span></span>
<span data-ttu-id="51081-106">`static` メソッドが必要なシナリオでは、ローカル関数の使用を許可します。</span><span class="sxs-lookup"><span data-stu-id="51081-106">Allow local functions to be used in scenarios where a `static` method is required.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="51081-107">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="51081-107">Detailed design</span></span>

<span data-ttu-id="51081-108">`static` 宣言されたローカル関数は、外側のスコープから状態をキャプチャできません。</span><span class="sxs-lookup"><span data-stu-id="51081-108">A local function declared `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="51081-109">その結果、外側のスコープからのローカル、パラメーター、および `this` は、`static` ローカル関数内では使用できません。</span><span class="sxs-lookup"><span data-stu-id="51081-109">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` local function.</span></span>

<span data-ttu-id="51081-110">`static` ローカル関数は、暗黙的または明示的な `this` または `base` 参照からインスタンスメンバーを参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="51081-110">A `static` local function cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="51081-111">`static` ローカル関数は、外側のスコープから `static` メンバーを参照できます。</span><span class="sxs-lookup"><span data-stu-id="51081-111">A `static` local function may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="51081-112">`static` ローカル関数は、外側のスコープから `constant` 定義を参照できます。</span><span class="sxs-lookup"><span data-stu-id="51081-112">A `static` local function may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="51081-113">`static` ローカル関数内の `nameof()` は、外側のスコープからローカル、パラメーター、または `this` または `base` を参照できます。</span><span class="sxs-lookup"><span data-stu-id="51081-113">`nameof()` in a `static` local function may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="51081-114">外側のスコープ内の `private` メンバーのアクセシビリティ規則は、`static` および非`static` ローカル関数で同じです。</span><span class="sxs-lookup"><span data-stu-id="51081-114">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` local functions.</span></span>

<span data-ttu-id="51081-115">`static` ローカル関数定義は、デリゲートでのみ使用されている場合でも、メタデータの `static` メソッドとして出力されます。</span><span class="sxs-lookup"><span data-stu-id="51081-115">A `static` local function definition is emitted as a `static` method in metadata, even if only used in a delegate.</span></span>

<span data-ttu-id="51081-116">`static` 以外のローカル関数またはラムダは、外側の `static` ローカル関数から状態をキャプチャできますが、外側の `static` ローカル関数の外部で状態をキャプチャすることはできません。</span><span class="sxs-lookup"><span data-stu-id="51081-116">A non-`static` local function or lambda can capture state from an enclosing `static` local function but cannot capture state outside the enclosing `static` local function.</span></span>

<span data-ttu-id="51081-117">`static` ローカル関数を式ツリーで呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="51081-117">A `static` local function cannot be invoked in an expression tree.</span></span>

<span data-ttu-id="51081-118">ローカル関数の呼び出しは、ローカル関数が `static`かどうかに関係なく、`callvirt`ではなく `call` として出力されます。</span><span class="sxs-lookup"><span data-stu-id="51081-118">A call to a local function is emitted as `call` rather than `callvirt`, regardless of whether the local function is `static`.</span></span>

<span data-ttu-id="51081-119">ローカル関数内の呼び出しのオーバーロードの解決。ローカル関数が `static`かどうかの影響を受けません。</span><span class="sxs-lookup"><span data-stu-id="51081-119">Overload resolution of a call within a local function not affected by whether the local function is `static`.</span></span>

<span data-ttu-id="51081-120">有効なプログラムのローカル関数から `static` 修飾子を削除しても、プログラムの意味は変わりません。</span><span class="sxs-lookup"><span data-stu-id="51081-120">Removing the `static` modifier from a local function in a valid program does not change the meaning of the program.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="51081-121">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="51081-121">Design meetings</span></span>

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
