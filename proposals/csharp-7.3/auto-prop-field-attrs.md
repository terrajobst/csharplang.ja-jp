---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483638"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a>自動実装プロパティフィールド-対象属性

## <a name="summary"></a>まとめ
[summary]: #summary

この機能を使用すると、開発者は自動実装プロパティのバッキングフィールドに属性を直接適用できます。

## <a name="motivation"></a>目的
[motivation]: #motivation

現在、自動実装プロパティのバッキングフィールドに属性を適用することはできません。  開発者がフィールドターゲット属性を使用する必要がある場合は、フィールドを手動で宣言し、より詳細なプロパティ構文を使用する必要があります。  イベントにC#対して生成されるバッキングフィールドに対して常にサポートされているフィールドターゲット属性がある場合は、そのプロパティ kin に同じ機能を拡張するのが理にかなっています。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

つまり、次のことは有効C#であり、警告が生成されません。

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

これにより、フィールドターゲット属性がコンパイラによって生成されるバッキングフィールドに適用されます。

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

前述のように、これにより、次C#のように既に有効であり、想定どおりに動作するため、1.0 からのイベント構文にパリティが加わります。

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

この変更を実装する際には、次の2つの欠点が考えられます。

1. 自動実装プロパティのフィールドに属性を適用しようとすると、そのブロックの属性が無視されることを示すコンパイラ警告が生成されます。  これらの属性をサポートするようにコンパイラが変更された場合は、後続の再コンパイルのバッキングフィールドに適用されるため、実行時にプログラムの動作が変更される可能性があります。
1. コンパイラは、属性の AttributeUsage ターゲットを自動実装プロパティのフィールドに適用しようとすると、現在は検証されません。  フィールドターゲットの属性をサポートするようにコンパイラが変更され、問題の属性をフィールドに適用できない場合、コンパイラは警告ではなくエラーを生成してビルドを中断します。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>会議のデザイン
