---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483578"
---
# <a name="improved-overload-candidates"></a>オーバーロード候補の改善

## <a name="summary"></a>まとめ
[summary]: #summary

オーバーロードの解決規則はほぼすべてC#の言語の更新プログラムで更新されており、プログラマのエクスペリエンスが向上します。あいまいな呼び出しを行う場合は、"明確な" 選択を選択します。 これは、旧バージョンとの互換性を維持するために慎重に行う必要がありますが、通常はエラーが発生する状況を解決するため、これらの拡張機能は正常に機能します。

1. メソッドグループにインスタンスと静的メンバーの両方が含まれている場合、インスタンスの受信者またはコンテキストなしで呼び出された場合はインスタンスメンバーを破棄し、インスタンスレシーバーで呼び出された場合は静的メンバーを破棄します。 レシーバーが存在しない場合は、静的コンテキストに静的メンバーのみが含まれます。それ以外の場合は、静的メンバーとインスタンスメンバーの両方が含まれます。 色の状況が原因で、受信者がインスタンスまたは型をあいまいした場合、両方が含まれます。 静的コンテキスト (このインスタンスレシーバーを暗黙的に使用することはできません) には、静的メンバーなど、このが定義されていないメンバーの本体、およびフィールド初期化子やコンストラクター初期化子などの使用できない場所が含まれます。
2. 型引数が制約を満たしていない複数のジェネリック メソッドがメソッド グループに含まれている場合、そのグループのメンバーは候補セットから削除されます。
3. メソッド グループの変換では、戻り値の型がデリゲートの戻り値の型と一致しない候補メソッドが候補セットから削除されます。