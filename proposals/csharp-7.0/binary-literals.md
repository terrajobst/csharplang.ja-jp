---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483434"
---
# <a name="binary-literals"></a><span data-ttu-id="3b84b-101">バイナリ リテラル</span><span class="sxs-lookup"><span data-stu-id="3b84b-101">Binary literals</span></span>

<span data-ttu-id="3b84b-102">と VB にバイナリリテラルをC#追加するための比較的一般的な要求があります。</span><span class="sxs-lookup"><span data-stu-id="3b84b-102">There’s a relatively common request to add binary literals to C# and VB.</span></span> <span data-ttu-id="3b84b-103">ビットマスク (例: フラグの列挙) の場合、これは純粋役に立つように見えますが、単なる教育目的でも優れています。</span><span class="sxs-lookup"><span data-stu-id="3b84b-103">For bitmasks (e.g. flag enums) this seems genuinely useful, but it would also be great just for educational purposes.</span></span>

<span data-ttu-id="3b84b-104">バイナリリテラルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3b84b-104">Binary literals would look like this:</span></span>

```csharp
int nineteen = 0b10011;
```

<span data-ttu-id="3b84b-105">構文的には、意味的には16進数リテラルと同じです。ただし、`b`/`B` を使用するのでは `x`なく、数字 /と `X`だけを持ち、16の代わりに基数2で解釈されます。`0``1`</span><span class="sxs-lookup"><span data-stu-id="3b84b-105">Syntactically and semantically they are identical to hexadecimal literals, except for using `b`/`B` instead of `x`/`X`, having only digits `0` and `1` and being interpreted in base 2 instead of 16.</span></span>

<span data-ttu-id="3b84b-106">これらの実装にはほとんどコストがかかりませんが、言語のユーザーにとってはほとんどの概念上のオーバーヘッドが発生しません。</span><span class="sxs-lookup"><span data-stu-id="3b84b-106">There’s little cost to implementing these, and little conceptual overhead to users of the language.</span></span>

## <a name="syntax"></a><span data-ttu-id="3b84b-107">構文</span><span class="sxs-lookup"><span data-stu-id="3b84b-107">Syntax</span></span>

<span data-ttu-id="3b84b-108">文法は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3b84b-108">The grammar would be as follows:</span></span>

```antlr
integer-literal:
    : ...
    | binary-integer-literal
    ;
binary-integer-literal:
    : `0b` binary-digits integer-type-suffix-opt
    | `0B` binary-digits integer-type-suffix-opt
    ;
binary-digits:
    : binary-digit
    | binary-digits binary-digit
    ;
binary-digit:
    : `0`
    | `1`
    ;
```
