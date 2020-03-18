---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483608"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a><span data-ttu-id="f73da-101">移動可能または移動できないコンテキストに関係なく、`fixed` フィールドのインデックスを固定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f73da-101">Indexing `fixed` fields should not require pinning regardless of the movable/unmovable context.</span></span> #

<span data-ttu-id="f73da-102">この変更にはバグ修正のサイズがあります。</span><span class="sxs-lookup"><span data-stu-id="f73da-102">The change has the size of a bug fix.</span></span> <span data-ttu-id="f73da-103">これは7.3 である可能性があり、さらに取得する方向と競合しません。</span><span class="sxs-lookup"><span data-stu-id="f73da-103">It can be in 7.3 and does not conflict with whatever direction we take further.</span></span>
<span data-ttu-id="f73da-104">この変更は、`s` が移動可能な場合でも、次のシナリオを使用できるようにすることだけです。</span><span class="sxs-lookup"><span data-stu-id="f73da-104">This change is only about allowing the following scenario to work even though `s` is moveable.</span></span> <span data-ttu-id="f73da-105">`s` を移動できない場合は既に有効です。</span><span class="sxs-lookup"><span data-stu-id="f73da-105">It is already valid when `s` is not moveable.</span></span> 

<span data-ttu-id="f73da-106">注: どちらの場合も、`unsafe` のコンテキストが必要です。</span><span class="sxs-lookup"><span data-stu-id="f73da-106">NOTE: in either case, it still requires `unsafe` context.</span></span> <span data-ttu-id="f73da-107">初期化されていないデータを読み取ることも、範囲外にすることもできます。</span><span class="sxs-lookup"><span data-stu-id="f73da-107">It is possible to read uninitialized data or even out of range.</span></span> <span data-ttu-id="f73da-108">変更されていません。</span><span class="sxs-lookup"><span data-stu-id="f73da-108">That is not changing.</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

<span data-ttu-id="f73da-109">ここに表示される主な "チャレンジ" は、仕様の緩和を説明する方法です。特に、次のようにピン留めする必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="f73da-109">The main “challenge” that I see here is how to explain the relaxation in the spec. In particular, since the following would still need pinning.</span></span> <span data-ttu-id="f73da-110">(`s` は移動可能であり、フィールドはポインターとして明示的に使用されるため)</span><span class="sxs-lookup"><span data-stu-id="f73da-110">(because `s` is moveable and we explicitly use the field as a pointer)</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

<span data-ttu-id="f73da-111">移動可能なときにターゲットのピン留めが必要になる理由の1つは、コード生成戦略の成果物であり、常にアンマネージポインターに変換されるため、ユーザーは `fixed` ステートメントを使用してピン留めすることになります。</span><span class="sxs-lookup"><span data-stu-id="f73da-111">One reason why we require pinning of the target when it is movable is the artifact of our code generation strategy, - we always convert to an unmanaged pointer and thus force the user to pin via `fixed` statement.</span></span> <span data-ttu-id="f73da-112">ただし、インデックス作成を行う場合、アンマネージへの変換は不要です。</span><span class="sxs-lookup"><span data-stu-id="f73da-112">However, conversion to unmanaged is unnecessary when doing indexing.</span></span> <span data-ttu-id="f73da-113">アンセーフポインターの数値演算は、受信者がマネージポインターの形式である場合にも同様に適用されます。</span><span class="sxs-lookup"><span data-stu-id="f73da-113">The same unsafe pointer math is equally applicable when we have the receiver in the form of a managed pointer.</span></span> <span data-ttu-id="f73da-114">これを行うと、中間参照は (GC 追跡) 管理され、固定は不要になります。</span><span class="sxs-lookup"><span data-stu-id="f73da-114">If we do that, then the intermediate ref is managed (GC-tracked) and the pinning is unnecessary.</span></span>

<span data-ttu-id="f73da-115">変更 https://github.com/dotnet/roslyn/pull/24966 は、この要件を緩和されするプロトタイプ PR です。</span><span class="sxs-lookup"><span data-stu-id="f73da-115">The change https://github.com/dotnet/roslyn/pull/24966 is a prototype PR that relaxes this requirement.</span></span>
