---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483680"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a><span data-ttu-id="9376a-101">0b または0x の後に桁区切り記号を使用する</span><span class="sxs-lookup"><span data-stu-id="9376a-101">Allow digit separator after 0b or 0x</span></span>

<span data-ttu-id="9376a-102">7\.2 C#では、桁区切り記号 (アンダースコア文字) が整数リテラルに含まれる場所のセットを拡張します。</span><span class="sxs-lookup"><span data-stu-id="9376a-102">In C# 7.2, we extend the set of places that digit separators (the underscore character) can appear in integral literals.</span></span> <span data-ttu-id="9376a-103">[7.0 以降C#では、リテラルの数字の間に区切り記号を使用でき](../csharp-7.0/digit-separators.md)ます。</span><span class="sxs-lookup"><span data-stu-id="9376a-103">[Beginning in C# 7.0, separators are permitted between the digits of a literal](../csharp-7.0/digit-separators.md).</span></span> <span data-ttu-id="9376a-104">現在、7.2 C#では、プレフィックスの後に、バイナリまたは16進リテラルの最初の有効桁数の前に桁区切り記号を付けることもできます。</span><span class="sxs-lookup"><span data-stu-id="9376a-104">Now, in C# 7.2, we also permit digit separators before the first significant digit of a binary or hexadecimal literal, after the prefix.</span></span>

```csharp
    123      // permitted in C# 1.0 and later
    1_2_3    // permitted in C# 7.0 and later
    0x1_2_3  // permitted in C# 7.0 and later
    0b101    // binary literals added in C# 7.0
    0b1_0_1  // permitted in C# 7.0 and later

    // in C# 7.2, _ is permitted after the `0x` or `0b`
    0x_1_2   // permitted in C# 7.2 and later
    0b_1_0_1 // permitted in C# 7.2 and later
```

<span data-ttu-id="9376a-105">10進数の整数リテラルに先頭にアンダースコアを付けることは許可されません。</span><span class="sxs-lookup"><span data-stu-id="9376a-105">We do not permit a decimal integer literal to have a leading underscore.</span></span> <span data-ttu-id="9376a-106">`_123` などのトークンは識別子です。</span><span class="sxs-lookup"><span data-stu-id="9376a-106">A token such as `_123` is an identifier.</span></span>
