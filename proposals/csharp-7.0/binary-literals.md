---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483434"
---
# <a name="binary-literals"></a>バイナリ リテラル

と VB にバイナリリテラルをC#追加するための比較的一般的な要求があります。 ビットマスク (例: フラグの列挙) の場合、これは純粋役に立つように見えますが、単なる教育目的でも優れています。

バイナリリテラルは次のようになります。

```csharp
int nineteen = 0b10011;
```

構文的には、意味的には16進数リテラルと同じです。ただし、`b`/`B` を使用するのでは `x`なく、数字 /と `X`だけを持ち、16の代わりに基数2で解釈されます。`0``1`

これらの実装にはほとんどコストがかかりませんが、言語のユーザーにとってはほとんどの概念上のオーバーヘッドが発生しません。

## <a name="syntax"></a>構文

文法は次のようになります。

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
