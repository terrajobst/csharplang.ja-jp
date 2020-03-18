---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2019
ms.locfileid: "79484010"
---
# <a name="static-local-functions"></a>静的ローカル関数

## <a name="summary"></a>まとめ

外側のスコープからの状態のキャプチャを禁止するローカル関数をサポートします。

## <a name="motivation"></a>目的

外側のコンテキストから状態を誤ってキャプチャしないようにします。
`static` メソッドが必要なシナリオでは、ローカル関数の使用を許可します。

## <a name="detailed-design"></a>詳細なデザイン

`static` 宣言されたローカル関数は、外側のスコープから状態をキャプチャできません。
その結果、外側のスコープからのローカル、パラメーター、および `this` は、`static` ローカル関数内では使用できません。

`static` ローカル関数は、暗黙的または明示的な `this` または `base` 参照からインスタンスメンバーを参照することはできません。

`static` ローカル関数は、外側のスコープから `static` メンバーを参照できます。

`static` ローカル関数は、外側のスコープから `constant` 定義を参照できます。

`static` ローカル関数内の `nameof()` は、外側のスコープからローカル、パラメーター、または `this` または `base` を参照できます。

外側のスコープ内の `private` メンバーのアクセシビリティ規則は、`static` および非`static` ローカル関数で同じです。

`static` ローカル関数定義は、デリゲートでのみ使用されている場合でも、メタデータの `static` メソッドとして出力されます。

`static` 以外のローカル関数またはラムダは、外側の `static` ローカル関数から状態をキャプチャできますが、外側の `static` ローカル関数の外部で状態をキャプチャすることはできません。

`static` ローカル関数を式ツリーで呼び出すことはできません。

ローカル関数の呼び出しは、ローカル関数が `static`かどうかに関係なく、`callvirt`ではなく `call` として出力されます。

ローカル関数内の呼び出しのオーバーロードの解決。ローカル関数が `static`かどうかの影響を受けません。

有効なプログラムのローカル関数から `static` 修飾子を削除しても、プログラムの意味は変わりません。

## <a name="design-meetings"></a>会議のデザイン

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
