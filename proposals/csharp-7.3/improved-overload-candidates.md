---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483578"
---
# <a name="improved-overload-candidates"></a><span data-ttu-id="9e53c-101">オーバーロード候補の改善</span><span class="sxs-lookup"><span data-stu-id="9e53c-101">Improved overload candidates</span></span>

## <a name="summary"></a><span data-ttu-id="9e53c-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="9e53c-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="9e53c-103">オーバーロードの解決規則はほぼすべてC#の言語の更新プログラムで更新されており、プログラマのエクスペリエンスが向上します。あいまいな呼び出しを行う場合は、"明確な" 選択を選択します。</span><span class="sxs-lookup"><span data-stu-id="9e53c-103">The overload resolution rules have been updated in nearly every C# language update to improve the experience for programmers, making ambiguous invocations select the "obvious" choice.</span></span> <span data-ttu-id="9e53c-104">これは、旧バージョンとの互換性を維持するために慎重に行う必要がありますが、通常はエラーが発生する状況を解決するため、これらの拡張機能は正常に機能します。</span><span class="sxs-lookup"><span data-stu-id="9e53c-104">This has to be done carefully to preserve backward compatibility, but since we are usually resolving what would otherwise be error cases, these enhancements usually work out nicely.</span></span>

1. <span data-ttu-id="9e53c-105">メソッドグループにインスタンスと静的メンバーの両方が含まれている場合、インスタンスの受信者またはコンテキストなしで呼び出された場合はインスタンスメンバーを破棄し、インスタンスレシーバーで呼び出された場合は静的メンバーを破棄します。</span><span class="sxs-lookup"><span data-stu-id="9e53c-105">When a method group contains both instance and static members, we discard the instance members if invoked without an instance receiver or context, and discard the static members if invoked with an instance receiver.</span></span> <span data-ttu-id="9e53c-106">レシーバーが存在しない場合は、静的コンテキストに静的メンバーのみが含まれます。それ以外の場合は、静的メンバーとインスタンスメンバーの両方が含まれます。</span><span class="sxs-lookup"><span data-stu-id="9e53c-106">When there is no receiver, we include only static members in a static context, otherwise both static and instance members.</span></span> <span data-ttu-id="9e53c-107">色の状況が原因で、受信者がインスタンスまたは型をあいまいした場合、両方が含まれます。</span><span class="sxs-lookup"><span data-stu-id="9e53c-107">When the receiver is ambiguously an instance or type due to a color-color situation, we include both.</span></span> <span data-ttu-id="9e53c-108">静的コンテキスト (このインスタンスレシーバーを暗黙的に使用することはできません) には、静的メンバーなど、このが定義されていないメンバーの本体、およびフィールド初期化子やコンストラクター初期化子などの使用できない場所が含まれます。</span><span class="sxs-lookup"><span data-stu-id="9e53c-108">A static context, where an implicit this instance receiver cannot be used, includes the body of members where no this is defined, such as static members, as well as places where this cannot be used, such as field initializers and constructor-initializers.</span></span>
2. <span data-ttu-id="9e53c-109">型引数が制約を満たしていない複数のジェネリック メソッドがメソッド グループに含まれている場合、そのグループのメンバーは候補セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="9e53c-109">When a method group contains some generic methods whose type arguments do not satisfy their constraints, these members are removed from the candidate set.</span></span>
3. <span data-ttu-id="9e53c-110">メソッド グループの変換では、戻り値の型がデリゲートの戻り値の型と一致しない候補メソッドが候補セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="9e53c-110">For a method group conversion, candidate methods whose return type doesn't match up with the delegate's return type are removed from the set.</span></span>
