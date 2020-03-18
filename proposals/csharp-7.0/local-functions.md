---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483446"
---
# <a name="local-functions"></a><span data-ttu-id="fe4ac-101">ローカル関数</span><span class="sxs-lookup"><span data-stu-id="fe4ac-101">Local functions</span></span>

<span data-ttu-id="fe4ac-102">ブロックスコープC#内の関数の宣言をサポートするようにを拡張します。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-102">We extend C# to support the declaration of functions in block scope.</span></span> <span data-ttu-id="fe4ac-103">ローカル関数は、外側のスコープから変数を使用 (キャプチャ) できます。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-103">Local functions may use (capture) variables from the enclosing scope.</span></span>

<span data-ttu-id="fe4ac-104">コンパイラは、フロー分析を使用して、ローカル関数が値を割り当てる前に、どの変数を使用するかを検出します。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-104">The compiler uses flow analysis to detect which variables a local function uses before assigning it a value.</span></span> <span data-ttu-id="fe4ac-105">関数を呼び出すたびに、このような変数を確実に代入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-105">Every call of the function requires such variables to be definitely assigned.</span></span> <span data-ttu-id="fe4ac-106">同様に、コンパイラは、戻り時に確実に代入される変数を決定します。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-106">Similarly the compiler determines which variables are definitely assigned on return.</span></span> <span data-ttu-id="fe4ac-107">このような変数は、ローカル関数が呼び出された後に確実に割り当てられたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-107">Such variables are considered definitely assigned after the local function is invoked.</span></span>

<span data-ttu-id="fe4ac-108">ローカル関数は、定義の前の構文ポイントから呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-108">Local functions may be called from a lexical point before its definition.</span></span> <span data-ttu-id="fe4ac-109">ローカル関数宣言ステートメントは、到達できない場合に警告を発生させません。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-109">Local function declaration statements do not cause a warning when they are not reachable.</span></span>

<span data-ttu-id="fe4ac-110">TODO:_書き込み仕様_</span><span class="sxs-lookup"><span data-stu-id="fe4ac-110">TODO: _WRITE SPEC_</span></span>

## <a name="syntax-grammar"></a><span data-ttu-id="fe4ac-111">構文文法</span><span class="sxs-lookup"><span data-stu-id="fe4ac-111">Syntax grammar</span></span>

<span data-ttu-id="fe4ac-112">この文法は、現在の仕様の文法と比較して表されます。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-112">This grammar is represented as a diff from the current spec grammar.</span></span>

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

<span data-ttu-id="fe4ac-113">ローカル関数では、外側のスコープで定義された変数を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-113">Local functions may use variables defined in the enclosing scope.</span></span> <span data-ttu-id="fe4ac-114">現在の実装では、ローカル関数内で読み取られるすべての変数が、定義の時点でローカル関数を実行する場合と同様に、確実に代入される必要があります。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-114">The current implementation requires that every variable read inside a local function be definitely assigned, as if executing the local function at its point of definition.</span></span> <span data-ttu-id="fe4ac-115">また、ローカル関数の定義は、任意の使用ポイントで "実行" されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-115">Also, the local function definition must have been "executed" at any use point.</span></span>

<span data-ttu-id="fe4ac-116">そのビットを試してみると (たとえば、2つの相互再帰的なローカル関数を定義することはできません)、明確な代入を確実に行う方法を変更しました。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-116">After experimenting with that a bit (for example, it is not possible to define two mutually recursive local functions), we've since revised how we want the definite assignment to work.</span></span> <span data-ttu-id="fe4ac-117">リビジョン (まだ実装されていません) は、ローカル関数を呼び出すたびに、ローカル関数で読み込まれるすべてのローカル変数が確実に割り当てられる必要があります。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-117">The revision (not yet implemented) is that all local variables read in a local function must be definitely assigned at each invocation of the local function.</span></span> <span data-ttu-id="fe4ac-118">実際には、それほど巧妙ではなく、作業を行うために残っている作業がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-118">That's actually more subtle than it sounds, and there is a bunch of work remaining to make it work.</span></span> <span data-ttu-id="fe4ac-119">この処理が完了すると、ローカル関数をそれを囲むブロックの末尾に移動できるようになります。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-119">Once it is done you'll be able to move your local functions to the end of its enclosing block.</span></span>

<span data-ttu-id="fe4ac-120">新しい明確な代入規則は、ローカル関数の戻り値の型を推論することと互換性がないため、戻り値の型を推論するためのサポートが削除される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-120">The new definite assignment rules are incompatible with inferring the return type of a local function, so we'll likely be removing support for inferring the return type.</span></span>

<span data-ttu-id="fe4ac-121">ローカル関数をデリゲートに変換しない限り、キャプチャは値型のフレームに対して行われます。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-121">Unless you convert a local function to a delegate, capturing is done into frames that are value types.</span></span> <span data-ttu-id="fe4ac-122">これは、キャプチャでローカル関数を使用することによる GC の圧力が得られないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-122">That means you don't get any GC pressure from using local functions with capturing.</span></span>

### <a name="reachability"></a><span data-ttu-id="fe4ac-123">経由で到達可能</span><span class="sxs-lookup"><span data-stu-id="fe4ac-123">Reachability</span></span>

<span data-ttu-id="fe4ac-124">仕様に追加します。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-124">We add to the spec</span></span>

> <span data-ttu-id="fe4ac-125">ステートメント形式のラムダ式またはローカル関数の本体は、到達可能と見なされます。</span><span class="sxs-lookup"><span data-stu-id="fe4ac-125">The body of a statement-bodied lambda expression or local function is considered reachable.</span></span>
