---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483500"
---
# <a name="digit-separators"></a><span data-ttu-id="58cdb-101">桁区切り文字</span><span class="sxs-lookup"><span data-stu-id="58cdb-101">Digit separators</span></span>

<span data-ttu-id="58cdb-102">大きな数値リテラルの数字をグループ化できると、読みやすさに大きな影響があり、重大な欠点はありません。</span><span class="sxs-lookup"><span data-stu-id="58cdb-102">Being able to group digits in large numeric literals would have great readability impact and no significant downside.</span></span> 

<span data-ttu-id="58cdb-103">バイナリリテラル (#215) を追加すると、数値リテラルが長くなる可能性が高くなるため、2つの機能が相互に拡張されます。</span><span class="sxs-lookup"><span data-stu-id="58cdb-103">Adding binary literals (#215) would increase the likelihood of numeric literals being long, so the two features enhance each other.</span></span> 

<span data-ttu-id="58cdb-104">ここでは、Java などに従い、桁区切り記号としてアンダースコア `_` を使用します。</span><span class="sxs-lookup"><span data-stu-id="58cdb-104">We would follow Java and others, and use an underscore `_` as a digit separator.</span></span> <span data-ttu-id="58cdb-105">数値リテラル (最初と最後の文字を除く) のどこでも実行できるようになります。これは、さまざまなシナリオで、特に異なる数値ベースに対して異なるグループ化が意味を持つ可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="58cdb-105">It would be able to occur everywhere in a numeric literal (except as the first and last character), since different groupings may make sense in different scenarios and especially for different numeric bases:</span></span>

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

<span data-ttu-id="58cdb-106">数字のシーケンスはアンダースコアで区切ることができ、場合によっては2つの連続する数字の間に複数のアンダースコアを使用できます。</span><span class="sxs-lookup"><span data-stu-id="58cdb-106">Any sequence of digits may be separated by underscores, possibly more than one underscore between two consecutive digits.</span></span> <span data-ttu-id="58cdb-107">これらは小数点と指数で許可されますが、前の規則に従うと、小数点の横 (`10_.0`)、指数文字の横 (`1.1e_1`)、または型指定子 (`10_f`) の横に表示されない場合があります。</span><span class="sxs-lookup"><span data-stu-id="58cdb-107">They are allowed in decimals as well as exponents, but following the previous rule, they may not appear next to the decimal (`10_.0`), next to the exponent character (`1.1e_1`), or next to the type specifier (`10_f`).</span></span> <span data-ttu-id="58cdb-108">バイナリリテラルおよび16進数リテラルで使用した場合、`0x` または `0b`の直後に表示されないことがあります。</span><span class="sxs-lookup"><span data-stu-id="58cdb-108">When used in binary and hexadecimal literals, they may not appear immediately following the `0x` or `0b`.</span></span>

<span data-ttu-id="58cdb-109">構文は単純で、区切り記号はセマンティックに影響を与えないので、単に無視されます。</span><span class="sxs-lookup"><span data-stu-id="58cdb-109">The syntax is straightforward, and the separators have no semantic impact - they are simply ignored.</span></span>

<span data-ttu-id="58cdb-110">これには広範な値があり、簡単に実装できます。</span><span class="sxs-lookup"><span data-stu-id="58cdb-110">This has broad value and is easy to implement.</span></span>
