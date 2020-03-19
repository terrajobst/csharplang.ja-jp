---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509493"
---
# <a name="attributes-on-local-functions"></a>ローカル関数の属性

## <a name="attributes"></a>属性

ローカル関数宣言で[属性](../spec/attributes.md)を使用できるようになりました。 ローカル関数のパラメーターと型パラメーターも属性を持つことができます。

メソッド、パラメーター、またはその型パラメーターに適用されるときに意味が指定された属性は、ローカル関数、パラメーター、またはその型パラメーターにそれぞれ適用される場合と同じ意味になります。

ローカル関数は、`[ConditionalAttribute]`で修飾することによって、条件付き[メソッド](../spec/attributes.md#the-conditional-attribute)と同じ意味で条件を設定できます。 条件付きローカル関数も `static`する必要があります。 条件付きメソッドに対するすべての制限は、条件付きローカル関数にも適用されます。これには、戻り値の型を `void`する必要があります。

## <a name="extern"></a>不十分

ローカル関数で `extern` 修飾子が許可されるようになりました。 これにより、外部[メソッド](../spec/classes.md#external-methods)と同じ意味でローカル関数が使用されるようになります。

外部メソッドと同様に、外部ローカル関数の*ローカル関数本体*はセミコロンである必要があります。 セミコロンの*ローカル関数本体*は、外部ローカル関数でのみ使用できます。 

外部ローカル関数も `static`する必要があります。

## <a name="syntax"></a>構文

[ローカル関数の文法](csharp-7.0/local-functions.md#syntax-grammar)は、次のように変更されます。
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
