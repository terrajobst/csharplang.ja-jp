---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483584"
---
# <a name="ref-local-reassignment"></a>参照ローカル再割り当て

7\.3 C#では、ref ローカル変数または ref パラメーターの参照を再バインドするためのサポートを追加しています。

`assignment_operator`のセットに次のを追加します。

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

`=ref` 演算子は、 ***ref 代入演算子***と呼ばれます。 *複合代入演算子*ではありません。 左オペランドは、ref ローカル変数、ref パラメーター (`this`以外)、または out パラメーターにバインドする式である必要があります。 右オペランドは、左辺オペランドと同じ型の値を示す左辺値を生成する式である必要があります。

右オペランドは、ref 割り当ての時点で確実に代入される必要があります。

左オペランドが `out` パラメーターにバインドされている場合、ref 代入演算子の先頭で `out` パラメーターが確実に割り当てられていないと、エラーになります。

左側のオペランドが書き込み可能な参照 (つまり、`ref readonly` ローカルパラメーターまたは `in` パラメーター以外を指定する) の場合、右オペランドは書き込み可能な左辺値である必要があります。

参照代入演算子は、割り当てられた型の左辺値を生成します。 左側のオペランドが書き込み可能である場合 (つまり、`ref readonly` または `in`ではありません) は、書き込み可能です。

この演算子の安全性規則は次のとおりです。

- 参照の再割り当て `e1 = ref e2`の場合、`e2` の*ref セーフツーエスケープ*は、`e1`の*ref セーフツーエスケープ*の範囲内である必要があります。

Ref のような相互*にエスケープする*と、ref に[似た型の安全性](../csharp-7.2/span-safety.md)が定義されます。