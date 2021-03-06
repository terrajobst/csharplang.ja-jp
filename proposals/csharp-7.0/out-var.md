---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483506"
---
# <a name="out-variable-declarations"></a>out 変数宣言

*Out 変数宣言*機能を使用すると、`out` 引数として渡される場所で変数を宣言できます。

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

このように宣言された変数は、 *out 変数*と呼ばれます。 変数の型には、コンテキストキーワード `var` を使用できます。 このスコープは、パターンマッチングによって導入された*パターン変数*の場合と同じです。

言語仕様 (セクション7.6.7 要素アクセス) によれば、要素アクセス (インデックス作成式) の引数リストに ref または out 引数は含まれていません。 ただし、`out`を受け入れるメタデータで宣言されたインデクサーなど、さまざまなシナリオでコンパイラによって許可されます。

Argument_value によって導入されるローカル変数のスコープ内では、宣言の前にあるテキストの位置でローカル変数を参照するコンパイル時エラーになります。

また、宣言がすぐに含まれる同じ引数リストで、暗黙的に型指定された (8.5.1) out 変数を参照することもできません。

オーバーロードの解決は次のように変更されます。

新しい変換を追加します。

> *式から*、暗黙的に型指定された out 変数宣言からすべての型への変換が行われます。

また

> 明示的に型指定された out 変数の引数の型は、宣言された型です。

and

> 暗黙的に型指定された out 変数の引数に型がありません。

暗黙的に型指定された out 変数宣言からの*変換*は、*式から*の他の変換よりも優れているとは限りません。

暗黙的に型指定される out 変数の型は、オーバーロードの解決によって選択されたメソッドのシグネチャにおける対応するパラメーターの型です。

新しい構文ノード `DeclarationExpressionSyntax` が追加され、out var 引数の宣言を表します。
