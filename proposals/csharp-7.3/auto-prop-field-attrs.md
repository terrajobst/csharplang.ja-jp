---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483638"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a><span data-ttu-id="d2321-101">自動実装プロパティフィールド-対象属性</span><span class="sxs-lookup"><span data-stu-id="d2321-101">Auto-Implemented Property Field-Targeted Attributes</span></span>

## <a name="summary"></a><span data-ttu-id="d2321-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="d2321-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d2321-103">この機能を使用すると、開発者は自動実装プロパティのバッキングフィールドに属性を直接適用できます。</span><span class="sxs-lookup"><span data-stu-id="d2321-103">This feature intends to allow developers to apply attributes directly to the backing fields of auto-implemented properties.</span></span>

## <a name="motivation"></a><span data-ttu-id="d2321-104">目的</span><span class="sxs-lookup"><span data-stu-id="d2321-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d2321-105">現在、自動実装プロパティのバッキングフィールドに属性を適用することはできません。</span><span class="sxs-lookup"><span data-stu-id="d2321-105">Currently it is not possible to apply attributes to the backing fields of auto-implemented properties.</span></span>  <span data-ttu-id="d2321-106">開発者がフィールドターゲット属性を使用する必要がある場合は、フィールドを手動で宣言し、より詳細なプロパティ構文を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2321-106">In those cases where the developer must use a field-targeting attribute they are forced to declare the field manually and use the more verbose property syntax.</span></span>  <span data-ttu-id="d2321-107">イベントにC#対して生成されるバッキングフィールドに対して常にサポートされているフィールドターゲット属性がある場合は、そのプロパティ kin に同じ機能を拡張するのが理にかなっています。</span><span class="sxs-lookup"><span data-stu-id="d2321-107">Given that C# has always supported field-targeted attributes on the generated backing field for events it makes sense to extend the same functionality to their property kin.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d2321-108">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="d2321-108">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d2321-109">つまり、次のことは有効C#であり、警告が生成されません。</span><span class="sxs-lookup"><span data-stu-id="d2321-109">In short, the following would be legal C# and not produce a warning:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

<span data-ttu-id="d2321-110">これにより、フィールドターゲット属性がコンパイラによって生成されるバッキングフィールドに適用されます。</span><span class="sxs-lookup"><span data-stu-id="d2321-110">This would result in the field-targeted attributes being applied to the compiler-generated backing field:</span></span>

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

<span data-ttu-id="d2321-111">前述のように、これにより、次C#のように既に有効であり、想定どおりに動作するため、1.0 からのイベント構文にパリティが加わります。</span><span class="sxs-lookup"><span data-stu-id="d2321-111">As mentioned, this brings parity with event syntax from C# 1.0 as the following is already legal and behaves as expected:</span></span>

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a><span data-ttu-id="d2321-112">短所</span><span class="sxs-lookup"><span data-stu-id="d2321-112">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d2321-113">この変更を実装する際には、次の2つの欠点が考えられます。</span><span class="sxs-lookup"><span data-stu-id="d2321-113">There are two potential drawbacks to implementing this change:</span></span>

1. <span data-ttu-id="d2321-114">自動実装プロパティのフィールドに属性を適用しようとすると、そのブロックの属性が無視されることを示すコンパイラ警告が生成されます。</span><span class="sxs-lookup"><span data-stu-id="d2321-114">Attempting to apply an attribute to the field of an auto-implemented property produces a compiler warning that the attributes in that block will be ignored.</span></span>  <span data-ttu-id="d2321-115">これらの属性をサポートするようにコンパイラが変更された場合は、後続の再コンパイルのバッキングフィールドに適用されるため、実行時にプログラムの動作が変更される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d2321-115">If the compiler were changed to support those attributes they would be applied to the backing field on a subsequent recompilation which could alter the behavior of the program at runtime.</span></span>
1. <span data-ttu-id="d2321-116">コンパイラは、属性の AttributeUsage ターゲットを自動実装プロパティのフィールドに適用しようとすると、現在は検証されません。</span><span class="sxs-lookup"><span data-stu-id="d2321-116">The compiler does not currently validate the AttributeUsage targets of the attributes when attempting to apply them to the field of the auto-implemented property.</span></span>  <span data-ttu-id="d2321-117">フィールドターゲットの属性をサポートするようにコンパイラが変更され、問題の属性をフィールドに適用できない場合、コンパイラは警告ではなくエラーを生成してビルドを中断します。</span><span class="sxs-lookup"><span data-stu-id="d2321-117">If the compiler were changed to support field-targeted attributes and the attribute in question cannot be applied to a field the compiler would emit an error instead of a warning, breaking the build.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d2321-118">代替</span><span class="sxs-lookup"><span data-stu-id="d2321-118">Alternatives</span></span>
[alternatives]: #alternatives

## <a name="unresolved-questions"></a><span data-ttu-id="d2321-119">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="d2321-119">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="d2321-120">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="d2321-120">Design meetings</span></span>
