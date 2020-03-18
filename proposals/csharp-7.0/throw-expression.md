---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483536"
---
# <a name="throw-expression"></a>Throw 式

次のように、式フォームのセットを拡張します。

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

型の規則は次のとおりです。

- *Throw_expression*には型がありません。
- *Throw_expression*は、暗黙的な変換によってすべての型に変換できます。

*Throw 式*は、 *null_coalescing_expression*を評価することによって生成された値をスローします。これは `System.Exception` から派生したクラス型の値、または有効な基本クラスとして `System.Exception` (またはサブクラス) を持つ型パラメーター型の `System.Exception`値を示す必要があります。 式の評価によって `null`が生成された場合は、代わりに `System.NullReferenceException` がスローされます。

*Throw 式*の評価の実行時の動作は、 [ *throw ステートメント*に対して指定された動作と](../../spec/statements.md#the-throw-statement)同じです。

フロー分析の規則は次のとおりです。

- すべての変数*v*については、 *throw_expression*の*null_coalescing_expression*する前に*v*が確実に割り当てられ、 *throw_expression*の前に確実に代入されます。
- すべての変数*v*に対して、 *throw_expression*の後に*v*が確実に割り当てられます。

*Throw 式*は、次の構文コンテキストでのみ許可されます。
- 三項条件演算子の2番目または3番目のオペランドとして `?:`
- Null 合体演算子の2番目のオペランドとして `??`
- 式形式のラムダまたはメソッドの本体として。
