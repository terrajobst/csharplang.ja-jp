---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483506"
---
# <a name="out-variable-declarations"></a><span data-ttu-id="f2ed3-101">out 変数宣言</span><span class="sxs-lookup"><span data-stu-id="f2ed3-101">Out variable declarations</span></span>

<span data-ttu-id="f2ed3-102">*Out 変数宣言*機能を使用すると、`out` 引数として渡される場所で変数を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-102">The *out variable declaration* feature enables a variable to be declared at the location that it is being passed as an `out` argument.</span></span>

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

<span data-ttu-id="f2ed3-103">このように宣言された変数は、 *out 変数*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-103">A variable declared this way is called an *out variable*.</span></span> <span data-ttu-id="f2ed3-104">変数の型には、コンテキストキーワード `var` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-104">You may use the contextual keyword `var` for the variable's type.</span></span> <span data-ttu-id="f2ed3-105">このスコープは、パターンマッチングによって導入された*パターン変数*の場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-105">The scope will be the same as for a *pattern-variable* introduced via pattern-matching.</span></span>

<span data-ttu-id="f2ed3-106">言語仕様 (セクション7.6.7 要素アクセス) によれば、要素アクセス (インデックス作成式) の引数リストに ref または out 引数は含まれていません。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-106">According to Language Specification (section 7.6.7 Element access) the argument-list of an element-access (indexing expression) does not contain ref or out arguments.</span></span> <span data-ttu-id="f2ed3-107">ただし、`out`を受け入れるメタデータで宣言されたインデクサーなど、さまざまなシナリオでコンパイラによって許可されます。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-107">However, they are permitted by the compiler for various scenarios, for example indexers declared in metadata that accept `out`.</span></span>

<span data-ttu-id="f2ed3-108">Argument_value によって導入されるローカル変数のスコープ内では、宣言の前にあるテキストの位置でローカル変数を参照するコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-108">Within the scope of a local variable introduced by an argument_value, it is a compile-time error to refer to that local variable in a textual position that precedes its declaration.</span></span>

<span data-ttu-id="f2ed3-109">また、宣言がすぐに含まれる同じ引数リストで、暗黙的に型指定された (8.5.1) out 変数を参照することもできません。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-109">It is also an error to reference an implicitly-typed (§8.5.1) out variable in the same argument list that immediately contains its declaration.</span></span>

<span data-ttu-id="f2ed3-110">オーバーロードの解決は次のように変更されます。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-110">Overload resolution is modified as follows:</span></span>

<span data-ttu-id="f2ed3-111">新しい変換を追加します。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-111">We add a new conversion:</span></span>

> <span data-ttu-id="f2ed3-112">*式から*、暗黙的に型指定された out 変数宣言からすべての型への変換が行われます。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-112">There is a *conversion from expression* from an implicitly-typed out variable declaration to every type.</span></span>

<span data-ttu-id="f2ed3-113">また</span><span class="sxs-lookup"><span data-stu-id="f2ed3-113">Also</span></span>

> <span data-ttu-id="f2ed3-114">明示的に型指定された out 変数の引数の型は、宣言された型です。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-114">The type of an explicitly-typed out variable argument is the declared type.</span></span>

<span data-ttu-id="f2ed3-115">and</span><span class="sxs-lookup"><span data-stu-id="f2ed3-115">and</span></span>

> <span data-ttu-id="f2ed3-116">暗黙的に型指定された out 変数の引数に型がありません。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-116">An implicitly-typed out variable argument has no type.</span></span>

<span data-ttu-id="f2ed3-117">暗黙的に型指定された out 変数宣言からの*変換*は、*式から*の他の変換よりも優れているとは限りません。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-117">The *conversion from expression* from an implicitly-typed out variable declaration is not considered better than any other *conversion from expression*.</span></span>

<span data-ttu-id="f2ed3-118">暗黙的に型指定される out 変数の型は、オーバーロードの解決によって選択されたメソッドのシグネチャにおける対応するパラメーターの型です。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-118">The type of an implicitly-typed out variable is the type of the corresponding parameter in the signature of the method selected by overload resolution.</span></span>

<span data-ttu-id="f2ed3-119">新しい構文ノード `DeclarationExpressionSyntax` が追加され、out var 引数の宣言を表します。</span><span class="sxs-lookup"><span data-stu-id="f2ed3-119">The new syntax node `DeclarationExpressionSyntax` is added to represent the declaration in an out var argument.</span></span>
