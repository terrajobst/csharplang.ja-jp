---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483476"
---
# <a name="static-delegates"></a>静的デリゲート

* [x] が提案されています
* [] プロトタイプ: 開始されていません
* [] の実装: 開始されていません
* [] 仕様: 開始されていません

## <a name="summary"></a>まとめ
[summary]: #summary

汎用の軽量のコールバック機能をC#言語に提供します。

## <a name="motivation"></a>目的
[motivation]: #motivation

現在、ユーザーは `System.Delegate` 型を使用してコールバックを作成することができます。 ただし、これらはかなり重いです (ヒープ割り当てが必要であり、常にコールバックを連結するように処理しているなど)。

さらに、`System.Delegate` はアンマネージ関数ポインターとの最適な相互運用機能を提供しません。つまり、非 blittable であり、マネージ/アンマネージ境界を越えるたびにマーシャリングが必要になるためです。

いくつかの小さな微調整で、ネイティブコードを使用して、軽量、汎用、および対話形式の新しいデリゲートを提供できます。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

1つは、次のようにして静的デリゲートを宣言することです。

```C#
static delegate int Func()
```

さらに、呼び出し規則、文字列のマーシャリング、および最後のエラー動作の設定を制御できるように、`System.Runtime.InteropServices.UnmanagedFunctionPointer` に似た方法で宣言の属性を設定することもできます。 注: `System.Runtime.InteropServices.UnmanagedFunctionPointer` 自体を使用することはできません。デリゲートでのみ使用できます。

宣言は、次のようなコンパイラによって内部表現に変換されます。

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

つまり、型 `IntPtr` の1つのメンバーを持つ構造体によって内部的に表現されます (構造体は blittable であり、ヒープ割り当ては発生しません)。
* メンバーには、コールバックとなる関数のアドレスが含まれます。
* この型は、コールバックのメソッドシグネチャに一致するメソッドを宣言します。
* 構造体の名前は、内部的に生成された他のバッキング構造体と同様に、ユーザーが構築できないようにする必要があります。
 * 例: 固定サイズバッファーは、`<name>e__FixedBuffer` の形式で名前を持つ構造体を生成します (`<` と `>` は識別子の一部であり、識別子は構築C#可能であり、IL では使用できます)。
* メソッド宣言の名前は、すべての静的なデリゲート型で使用される既知の名前である必要があります (これにより、コンパイラは、署名を決定するときに検索する名前を知ることができます)。

静的デリゲートの値は、コールバックのシグネチャと一致する静的メソッドにのみバインドできます。

コールバックの連結はサポートされていません。

コールバックの呼び出しは、`calli` 命令によって実装されます。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

静的デリゲートは、通常のデリゲートを使用する既存の Api では機能しません (1 つは、同じシグネチャの通常のデリゲートで静的デリゲートをラップする必要があります)。
* `System.Delegate` が `object` および `IntPtr` フィールドのセットとして内部的に表現されている場合 (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs)では、メソッドシグネチャが一致する `System.Delegate` に静的デリゲートを暗黙的に変換できる可能性があります。 また、静的なデリゲートとしてのすべての要件に対して `System.Delegate` 最適化されている場合は、逆方向に明示的な変換を提供することもできます。

コアフレームワークで静的デリゲートをすぐに使用できるようにするには、追加の作業が必要になります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

TBD

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

設計のどの部分も未定ですか。

## <a name="design-meetings"></a>会議のデザイン

この提案に影響を与えるデザインメモにリンクし、それぞれの変更内容について1つの文で説明します。


