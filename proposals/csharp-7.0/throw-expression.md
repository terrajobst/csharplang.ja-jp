---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483536"
---
# <a name="throw-expression"></a><span data-ttu-id="18eb0-101">Throw 式</span><span class="sxs-lookup"><span data-stu-id="18eb0-101">Throw expression</span></span>

<span data-ttu-id="18eb0-102">次のように、式フォームのセットを拡張します。</span><span class="sxs-lookup"><span data-stu-id="18eb0-102">We extend the set of expression forms to include</span></span>

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

<span data-ttu-id="18eb0-103">型の規則は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="18eb0-103">The type rules are as follows:</span></span>

- <span data-ttu-id="18eb0-104">*Throw_expression*には型がありません。</span><span class="sxs-lookup"><span data-stu-id="18eb0-104">A *throw_expression* has no type.</span></span>
- <span data-ttu-id="18eb0-105">*Throw_expression*は、暗黙的な変換によってすべての型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="18eb0-105">A *throw_expression* is convertible to every type by an implicit conversion.</span></span>

<span data-ttu-id="18eb0-106">*Throw 式*は、 *null_coalescing_expression*を評価することによって生成された値をスローします。これは `System.Exception` から派生したクラス型の値、または有効な基本クラスとして `System.Exception` (またはサブクラス) を持つ型パラメーター型の `System.Exception`値を示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="18eb0-106">A *throw expression* throws the value produced by evaluating the *null_coalescing_expression*, which must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="18eb0-107">式の評価によって `null`が生成された場合は、代わりに `System.NullReferenceException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="18eb0-107">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="18eb0-108">*Throw 式*の評価の実行時の動作は、 [ *throw ステートメント*に対して指定された動作と](../../spec/statements.md#the-throw-statement)同じです。</span><span class="sxs-lookup"><span data-stu-id="18eb0-108">The behavior at runtime of the evaluation of a *throw expression* is the same [as specified for a *throw statement*](../../spec/statements.md#the-throw-statement).</span></span>

<span data-ttu-id="18eb0-109">フロー分析の規則は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="18eb0-109">The flow-analysis rules are as follows:</span></span>

- <span data-ttu-id="18eb0-110">すべての変数*v*については、 *throw_expression*の*null_coalescing_expression*する前に*v*が確実に割り当てられ、 *throw_expression*の前に確実に代入されます。</span><span class="sxs-lookup"><span data-stu-id="18eb0-110">For every variable *v*, *v* is definitely assigned before the *null_coalescing_expression* of a *throw_expression* iff it is definitely assigned before the *throw_expression*.</span></span>
- <span data-ttu-id="18eb0-111">すべての変数*v*に対して、 *throw_expression*の後に*v*が確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18eb0-111">For every variable *v*, *v* is definitely assigned after *throw_expression*.</span></span>

<span data-ttu-id="18eb0-112">*Throw 式*は、次の構文コンテキストでのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="18eb0-112">A *throw expression* is permitted in only the following syntactic contexts:</span></span>
- <span data-ttu-id="18eb0-113">三項条件演算子の2番目または3番目のオペランドとして `?:`</span><span class="sxs-lookup"><span data-stu-id="18eb0-113">As the second or third operand of a ternary conditional operator `?:`</span></span>
- <span data-ttu-id="18eb0-114">Null 合体演算子の2番目のオペランドとして `??`</span><span class="sxs-lookup"><span data-stu-id="18eb0-114">As the second operand of a null coalescing operator `??`</span></span>
- <span data-ttu-id="18eb0-115">式形式のラムダまたはメソッドの本体として。</span><span class="sxs-lookup"><span data-stu-id="18eb0-115">As the body of an expression-bodied lambda or method.</span></span>
