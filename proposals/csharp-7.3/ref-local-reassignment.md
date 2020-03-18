---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483584"
---
# <a name="ref-local-reassignment"></a><span data-ttu-id="1c1d2-101">参照ローカル再割り当て</span><span class="sxs-lookup"><span data-stu-id="1c1d2-101">Ref Local Reassignment</span></span>

<span data-ttu-id="1c1d2-102">7\.3 C#では、ref ローカル変数または ref パラメーターの参照を再バインドするためのサポートを追加しています。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-102">In C# 7.3, we add support for rebinding the referent of a ref local variable or a ref parameter.</span></span>

<span data-ttu-id="1c1d2-103">`assignment_operator`のセットに次のを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-103">We add the following to the set of `assignment_operator`s.</span></span>

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

<span data-ttu-id="1c1d2-104">`=ref` 演算子は、 ***ref 代入演算子***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-104">The `=ref` operator is called the ***ref assignment operator***.</span></span> <span data-ttu-id="1c1d2-105">*複合代入演算子*ではありません。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-105">It is not a *compound assignment operator*.</span></span> <span data-ttu-id="1c1d2-106">左オペランドは、ref ローカル変数、ref パラメーター (`this`以外)、または out パラメーターにバインドする式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-106">The left operand must be an expression that binds to a ref local variable, a ref parameter (other than `this`), or an out parameter.</span></span> <span data-ttu-id="1c1d2-107">右オペランドは、左辺オペランドと同じ型の値を示す左辺値を生成する式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-107">The right operand must be an expression that yields an lvalue designating a value of the same type as the left operand.</span></span>

<span data-ttu-id="1c1d2-108">右オペランドは、ref 割り当ての時点で確実に代入される必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-108">The right operand must be definitely assigned at the point of the ref assignment.</span></span>

<span data-ttu-id="1c1d2-109">左オペランドが `out` パラメーターにバインドされている場合、ref 代入演算子の先頭で `out` パラメーターが確実に割り当てられていないと、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-109">When the left operand binds to an `out` parameter, it is an error if that `out` parameter has not been definitely assigned at the beginning of the ref assignment operator.</span></span>

<span data-ttu-id="1c1d2-110">左側のオペランドが書き込み可能な参照 (つまり、`ref readonly` ローカルパラメーターまたは `in` パラメーター以外を指定する) の場合、右オペランドは書き込み可能な左辺値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-110">If the left operand is a writeable ref (i.e. it designates anything other than a `ref readonly` local or  `in` parameter), then the right operand must be a writeable lvalue.</span></span>

<span data-ttu-id="1c1d2-111">参照代入演算子は、割り当てられた型の左辺値を生成します。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-111">The ref assignment operator yields an lvalue of the assigned type.</span></span> <span data-ttu-id="1c1d2-112">左側のオペランドが書き込み可能である場合 (つまり、`ref readonly` または `in`ではありません) は、書き込み可能です。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-112">It is writeable if the left operand is writeable (i.e. not `ref readonly` or `in`).</span></span>

<span data-ttu-id="1c1d2-113">この演算子の安全性規則は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-113">The safety rules for this operator are:</span></span>

- <span data-ttu-id="1c1d2-114">参照の再割り当て `e1 = ref e2`の場合、`e2` の*ref セーフツーエスケープ*は、`e1`の*ref セーフツーエスケープ*の範囲内である必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-114">For a ref reassignment `e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

<span data-ttu-id="1c1d2-115">Ref のような相互*にエスケープする*と、ref に[似た型の安全性](../csharp-7.2/span-safety.md)が定義されます。</span><span class="sxs-lookup"><span data-stu-id="1c1d2-115">Where *ref-safe-to-escape* is defined in [Safety for ref-like types](../csharp-7.2/span-safety.md)</span></span>
