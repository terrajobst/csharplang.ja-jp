---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483860"
---
# <a name="expression-variables-in-initializers"></a><span data-ttu-id="933ef-101">初期化子における式の変数</span><span class="sxs-lookup"><span data-stu-id="933ef-101">Expression variables in initializers</span></span>

## <a name="summary"></a><span data-ttu-id="933ef-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="933ef-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="933ef-103">7でC#導入された機能を拡張して、フィールド初期化子、プロパティ初期化子、.ctor 初期化子、およびクエリ句で式変数 (out 変数の宣言と宣言パターン) を含む式を許可します。</span><span class="sxs-lookup"><span data-stu-id="933ef-103">We extend the features introduced in C# 7 to permit expressions containing expression variables (out variable declarations and declaration patterns) in field initializers, property initializers, ctor-initializers, and query clauses.</span></span>

## <a name="motivation"></a><span data-ttu-id="933ef-104">目的</span><span class="sxs-lookup"><span data-stu-id="933ef-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="933ef-105">これにより、時間が不足しているためC# 、言語のいくつかの大まかな端が完成します。</span><span class="sxs-lookup"><span data-stu-id="933ef-105">This completes a couple of the rough edges left in the C# language due to lack of time.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="933ef-106">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="933ef-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="933ef-107">.Ctor 初期化子の式変数 (out 変数宣言と宣言パターン) の宣言を禁止する制限を削除します。</span><span class="sxs-lookup"><span data-stu-id="933ef-107">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a ctor-initializer.</span></span> <span data-ttu-id="933ef-108">このように宣言された変数は、コンストラクターの本体全体でスコープ内にあります。</span><span class="sxs-lookup"><span data-stu-id="933ef-108">Such a declared variable is in scope throughout the body of the constructor.</span></span>

<span data-ttu-id="933ef-109">フィールドまたはプロパティ初期化子での式変数の宣言 (out 変数の宣言と宣言パターン) を禁止する制限を削除します。</span><span class="sxs-lookup"><span data-stu-id="933ef-109">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a field or property initializer.</span></span> <span data-ttu-id="933ef-110">このように宣言された変数は、初期化式全体でスコープ内にあります。</span><span class="sxs-lookup"><span data-stu-id="933ef-110">Such a declared variable is in scope throughout the initializing expression.</span></span>

<span data-ttu-id="933ef-111">ラムダの本体に変換されるクエリ式の句で、式変数 (out 変数宣言と宣言パターン) の宣言を禁止する制限を削除します。</span><span class="sxs-lookup"><span data-stu-id="933ef-111">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a query expression clause that is translated into the body of a lambda.</span></span> <span data-ttu-id="933ef-112">このように宣言された変数は、クエリ句の式全体でスコープ内にあります。</span><span class="sxs-lookup"><span data-stu-id="933ef-112">Such a declared variable is in scope throughout that expression of the query clause.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="933ef-113">短所</span><span class="sxs-lookup"><span data-stu-id="933ef-113">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="933ef-114">[なし] :</span><span class="sxs-lookup"><span data-stu-id="933ef-114">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="933ef-115">代替</span><span class="sxs-lookup"><span data-stu-id="933ef-115">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="933ef-116">これらのコンテキストで宣言された式変数の適切な範囲は明らかではなく、さらに LDM による説明です。</span><span class="sxs-lookup"><span data-stu-id="933ef-116">The appropriate scope for expression variables declared in these contexts is not obvious, and deserves further LDM discussion.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="933ef-117">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="933ef-117">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="933ef-118">[] これらの変数の適切なスコープは何ですか。</span><span class="sxs-lookup"><span data-stu-id="933ef-118">[ ] What is the appropriate scope for these variables?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="933ef-119">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="933ef-119">Design meetings</span></span>

<span data-ttu-id="933ef-120">[なし] :</span><span class="sxs-lookup"><span data-stu-id="933ef-120">None.</span></span>
