---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483560"
---
# <a name="lambda-attributes"></a>ラムダ属性

* [x] が提案されています
* [] プロトタイプ
* [] の実装
* [] 仕様

## <a name="summary"></a>まとめ
[summary]: #summary

属性をラムダ (および匿名メソッド) およびラムダ/匿名メソッドのパラメーターに適用できるようにします。これらは通常のメソッドに対して使用できます。

## <a name="motivation"></a>目的
[motivation]: #motivation

2つの主な動機:

1. コンパイル時にアナライザーに表示されるメタデータを提供するため。
2. 実行時にリフレクションおよびツールに表示されるメタデータを提供するため。

(1) の例として、パフォーマンスを重視するコードの場合、状態を閉じるラムダに対してクロージャとデリゲートが割り当てられているときにフラグを設定するアナライザーを作成できると便利です。  多くの場合、このようなコードの開発者は、状態をキャプチャしないようにするために、コンパイラがメソッドの静的メソッドとキャッシュ可能なデリゲートを生成できるようにするため、または終了する唯一の状態が `this`であることを確認します。これにより、クロージャオブジェクトの割り当てを少なくすることができます。  しかし、キャプチャできるものを制限するための言語サポートがないと、状態が誤って閉じられることがあります。  開発者が属性を使用してラムダに注釈を設定し、どの状態を閉じることができるかを示すことができる場合は、次のようになります。

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

次の例のように、状態が正しくキャプチャされていない場合に、アナライザーにフラグを設定することができます。

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

- 通常のメソッドと同じ属性構文を使用して、ラムダまたは匿名メソッドの先頭に属性を適用することができます。次に例を示します。

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- 属性がラムダメソッドに適用されるか、いずれかの引数に適用されるかについてあいまいさを避けるために、属性は、次のように、引数の周りで使用する場合にのみ使用できます。

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- 匿名メソッドでは、次の例のように、属性を `delegate` キーワードの前にメソッドに適用するために、かっこは必要ありません。

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- 次の例のように、コンマ区切りの標準構文またはフル属性構文を使用して、複数の属性を適用できます。

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- 属性は、匿名メソッドまたはラムダのパラメーターに適用できます。ただし、次の例のように、引数の周りにかっこが使用されている場合に限ります。

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- 次の例のように、コンマ区切りまたはフル属性構文を使用して、匿名メソッドまたはラムダのパラメーターに複数の属性を適用できます。

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- `return`対象の属性は、次のようにラムダでも使用できます。

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- コンパイラは、他のメソッドの場合と同様に、生成されたメソッドと引数に属性を出力します。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

該当なし

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

該当なし

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

該当なし

## <a name="design-meetings"></a>会議のデザイン

該当なし