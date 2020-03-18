---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483608"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a>移動可能または移動できないコンテキストに関係なく、`fixed` フィールドのインデックスを固定する必要はありません。 #

この変更にはバグ修正のサイズがあります。 これは7.3 である可能性があり、さらに取得する方向と競合しません。
この変更は、`s` が移動可能な場合でも、次のシナリオを使用できるようにすることだけです。 `s` を移動できない場合は既に有効です。 

注: どちらの場合も、`unsafe` のコンテキストが必要です。 初期化されていないデータを読み取ることも、範囲外にすることもできます。 変更されていません。

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

ここに表示される主な "チャレンジ" は、仕様の緩和を説明する方法です。特に、次のようにピン留めする必要があるためです。 (`s` は移動可能であり、フィールドはポインターとして明示的に使用されるため)

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

移動可能なときにターゲットのピン留めが必要になる理由の1つは、コード生成戦略の成果物であり、常にアンマネージポインターに変換されるため、ユーザーは `fixed` ステートメントを使用してピン留めすることになります。 ただし、インデックス作成を行う場合、アンマネージへの変換は不要です。 アンセーフポインターの数値演算は、受信者がマネージポインターの形式である場合にも同様に適用されます。 これを行うと、中間参照は (GC 追跡) 管理され、固定は不要になります。

変更 https://github.com/dotnet/roslyn/pull/24966 は、この要件を緩和されするプロトタイプ PR です。
