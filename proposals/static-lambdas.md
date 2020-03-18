---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "79483998"
---
# <a name="static-lambdas"></a>静的ラムダ

## <a name="summary"></a>まとめ

外側のスコープからの状態のキャプチャを禁止するラムダをサポートします。

## <a name="motivation"></a>目的

外側のコンテキストから状態を誤ってキャプチャしないようにします。

## <a name="detailed-design"></a>詳細なデザイン

`static` を持つラムダは、外側のスコープから状態をキャプチャできません。
その結果、外側のスコープのローカル、パラメーター、および `this` は、`static` ラムダ内では使用できません。

`static` のラムダは、暗黙的または明示的な `this` または `base` 参照からインスタンスメンバーを参照することはできません。

`static` のラムダは、外側のスコープから `static` メンバーを参照できます。

`static` のラムダでは、外側のスコープから `constant` 定義を参照できます。

`static` ラムダ内の `nameof()` は、外側のスコープからローカル、パラメーター、または `this` または `base` を参照できます。

外側のスコープ内の `private` メンバーのアクセシビリティ規則は、`static` および非`static` のラムダでも同じです。

`static` のラムダ定義をメタデータの `static` メソッドとして出力するかどうかは保証されません。 これは、最適化のためにコンパイラの実装に残されます。

非`static` ローカル関数またはラムダは、外側の `static` ラムダから状態をキャプチャできますが、外側の `static` ラムダの外側で状態をキャプチャすることはできません。

`static` のラムダは、式ツリーで使用できます。

有効なプログラムでラムダから `static` 修飾子を削除しても、プログラムの意味は変わりません。
