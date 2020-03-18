---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2019
ms.locfileid: "79484046"
---
# <a name="permit-stackalloc-in-nested-contexts"></a>入れ子になったコンテキストでの `stackalloc` を許可する

### <a name="stack-allocation"></a>スタック割り当て

C#言語仕様のセクション[*スタック割り当て*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation)を変更して、`stackalloc` 式が表示される可能性がある場所を緩めることができます。 削除

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

をに置き換えます。

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

*Stackalloc_initializer*への*array_initializer*の追加 (およびインデックス式のオプションの作成) は[7.3 C#の拡張機能](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md)であり、ここでは説明しません。

`stackalloc` 式の*要素の型*は、stackalloc 式に指定された*unmanaged_type* (存在する場合)、または*array_initializer*の要素間の共通型である場合は、それ以外の場合はです。

*要素型*が `K` *stackalloc_initializer*の型は、その構文コンテキストによって異なります。
- *Stackalloc_initializer*が*local_variable_declaration*ステートメントまたは*for_initializer*の*local_variable_initializer*として直接表示される場合、その型は `K*`になります。
- それ以外の場合は、その型が `System.Span<K>`になります。

### <a name="stackalloc-conversion"></a>Stackalloc の変換

*Stackalloc 変換*は、新しい組み込みの式からの暗黙的な変換です。 *Stackalloc_initializer*の型が `K*`場合、 *stackalloc_initializer*から型 `System.Span<K>`への暗黙的な*stackalloc 変換*が存在します。
