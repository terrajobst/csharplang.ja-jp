---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483590"
---
# <a name="stackalloc-array-initializers"></a>stackalloc 配列初期化子

## <a name="summary"></a>まとめ
[summary]: #summary

`stackalloc`で配列初期化子構文を使用できるようにします。

## <a name="motivation"></a>目的
[motivation]: #motivation

通常の配列は、作成時に初期化された要素を持つことができます。 `stackalloc` の場合は、これを許可することが妥当であると思われます。

このような構文を使用できない理由については、`stackalloc` が非常に頻繁に発生します。  
例については、「」を参照してください[#1112](https://github.com/dotnet/csharplang/issues/1112)

## <a name="detailed-design"></a>詳細なデザイン

通常の配列は、次の構文を使用して作成できます。

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

次のようにして、スタックの割り当て済み配列を作成できるようにする必要があります。  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

すべてのケースのセマンティクスは、配列の場合とほぼ同じです。  
たとえば、最後のケースでは、要素型は初期化子から推論され、"アンマネージ" 型である必要があります。

注: この機能は、ターゲットが `Span<T>`であることに依存していません。 `T*` の場合に適用されるだけなので、`Span<T>` ケースで述語を行うのは妥当ではないようです。  

## <a name="translation"></a>翻訳

単純な実装では、一連の要素ごとの代入を通じて、配列を作成した直後に初期化するだけで済みます。  

配列の場合と同様に、すべてまたはほとんどの要素が blittable 型であるケースを検出し、すべての定数要素の事前に作成された状態をコピーすることで、より効率的な手法を使用することができます。 

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

これは便利な機能です。 何もすることはできません。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>会議のデザイン

まだありません。 
