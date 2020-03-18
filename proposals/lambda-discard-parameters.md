---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2019
ms.locfileid: "79484058"
---
# <a name="lambda-discard-parameters"></a><span data-ttu-id="7b689-101">ラムダ破棄のパラメーター</span><span class="sxs-lookup"><span data-stu-id="7b689-101">Lambda discard parameters</span></span>

## <a name="summary"></a><span data-ttu-id="7b689-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="7b689-102">Summary</span></span>

<span data-ttu-id="7b689-103">破棄 (`_`) をラムダおよび匿名メソッドのパラメーターとして使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="7b689-103">Allow discards (`_`) to be used as parameters of lambdas and anonymous methods.</span></span>
<span data-ttu-id="7b689-104">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="7b689-104">For example:</span></span>
- <span data-ttu-id="7b689-105">ラムダ: `(_, _) => 0`、`(int _, int _) => 0`</span><span class="sxs-lookup"><span data-stu-id="7b689-105">lambdas: `(_, _) => 0`, `(int _, int _) => 0`</span></span>
- <span data-ttu-id="7b689-106">匿名メソッド: `delegate(int _, int _) { return 0; }`</span><span class="sxs-lookup"><span data-stu-id="7b689-106">anonymous methods: `delegate(int _, int _) { return 0; }`</span></span>

## <a name="motivation"></a><span data-ttu-id="7b689-107">目的</span><span class="sxs-lookup"><span data-stu-id="7b689-107">Motivation</span></span>

<span data-ttu-id="7b689-108">未使用のパラメーターに名前を付ける必要はありません。</span><span class="sxs-lookup"><span data-stu-id="7b689-108">Unused parameters do not need to be named.</span></span> <span data-ttu-id="7b689-109">破棄の目的は明確ではありません。つまり、未使用または破棄されます。</span><span class="sxs-lookup"><span data-stu-id="7b689-109">The intent of discards is clear, i.e. they are unused/discarded.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="7b689-110">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="7b689-110">Detailed design</span></span>

<span data-ttu-id="7b689-111">[メソッドパラメーター](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters)`_`という名前のパラメーターが複数あるラムダまたは匿名メソッドのパラメーターリストでは、このようなパラメーターは破棄パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="7b689-111">[Method parameters](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) In the parameter list of a lambda or anonymous method with more than one parameter named `_`, such parameters are discard parameters.</span></span>
<span data-ttu-id="7b689-112">注: 1 つのパラメーターに `_` という名前が付けられている場合は、旧バージョンとの互換性を保つために通常のパラメーターになります。</span><span class="sxs-lookup"><span data-stu-id="7b689-112">Note: if a single parameter is named `_` then it is a regular parameter for backwards compatibility reasons.</span></span>

<span data-ttu-id="7b689-113">破棄パラメーターでは、どのスコープにも名前は導入されません。</span><span class="sxs-lookup"><span data-stu-id="7b689-113">Discard parameters do not introduce any names to any scopes.</span></span>
<span data-ttu-id="7b689-114">これは、`_` (アンダースコア) 名が非表示になることを意味します。</span><span class="sxs-lookup"><span data-stu-id="7b689-114">Note this implies they do not cause any `_` (underscore) names to be hidden.</span></span>

<span data-ttu-id="7b689-115">[簡易名](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names)`K` がゼロで、 *simple_name*が*ブロック*内に存在し、*ブロック*の (またはそれを囲む*ブロック*の) ローカル変数宣言領域 ([宣言](basic-concepts.md#declarations)) にローカル変数、パラメーター (破棄パラメーターを除く)、または名前が `I`の定数が含まれている場合、 *simple_name*はそのローカル変数、パラメーター、または定数を参照し、変数または値として分類</span><span class="sxs-lookup"><span data-stu-id="7b689-115">[Simple names](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter (with the exception of discard parameters) or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>

<span data-ttu-id="7b689-116">[スコープ](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes)Discard パラメーターを除き、 *lambda_expression* ([匿名関数式](expressions.md#anonymous-function-expressions)) で宣言されたパラメーターのスコープは、discard パラメーターを除き、 *lambda_expression*の*anonymous_function_body*であり、 *anonymous_method_expression* ([匿名関数式](expressions.md#anonymous-function-expressions)) で宣言されたパラメーターのスコープはその*anonymous_method_expression*の*ブロック*です。</span><span class="sxs-lookup"><span data-stu-id="7b689-116">[Scopes](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) With the exception of discard parameters, the scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression* With the exception of discard parameters, the scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>

## <a name="related-spec-sections"></a><span data-ttu-id="7b689-117">関連するスペックセクション</span><span class="sxs-lookup"><span data-stu-id="7b689-117">Related spec sections</span></span>
- [<span data-ttu-id="7b689-118">対応するパラメーター</span><span class="sxs-lookup"><span data-stu-id="7b689-118">Corresponding parameters</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
