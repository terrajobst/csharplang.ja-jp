---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483602"
---
# <a name="unmanaged-type-constraint"></a>アンマネージ型制約

## <a name="summary"></a>まとめ
[summary]: #summary

アンマネージ制約機能は、言語仕様の "アンマネージ型" C#と呼ばれる型のクラスに言語の適用を提供します。 これは、参照型ではなく、入れ子の任意のレベルの参照型フィールドを含まない型として、セクション18.2 で定義されています。  

## <a name="motivation"></a>目的
[motivation]: #motivation

主な動機は、で低レベルのC#相互運用コードを簡単に作成できるようにすることです。 アンマネージ型は相互運用コードの中核となる構成要素の1つですが、ジェネリックのサポートがないため、すべてのアンマネージ型で再利用可能なルーチンを作成することはできません。 代わりに、開発者はライブラリ内のすべてのアンマネージ型に対して同じボイラープレートコードを作成することが強制されます。

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

この種類のシナリオを有効にするには、言語に新しい制約を導入します。アンマネージ:

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

この制約は、 C#言語仕様のアンマネージ型定義に適合する型によってのみ満たすことができます。これを調べる別の方法として、型がアンマネージ制約を満たしていて、ポインターとして使用することもできます。 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

アンマネージ制約がある型パラメーターは、アンマネージ型に使用できるすべての機能 (ポインター、固定など) を使用できます。 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

この制約により、構造化データとバイトストリームの間で効率的な変換を行うこともできます。 これは、ネットワークスタックとシリアル化層で一般的な操作です。

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

このようなルーチンは、コンパイル時には安全であり、割り当てが provably されるため、便利です。  現在、相互運用の作成者はこれを行うことはできません (パフォーマンスが重要なレイヤーにある場合でも)。  代わりに、値が正しく管理されていないことを確認するために、高コストのランタイムチェックを含むルーチンの割り当てに依存する必要があります。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

この言語では、`unmanaged`という名前の新しい制約が導入されます。 この制約を満たすためには、型は構造体である必要があり、型のすべてのフィールドは次のいずれかのカテゴリに分類される必要があります。

- `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`の種類があります。`bool``IntPtr``UIntPtr`
- 任意の `enum` の種類を指定します。
- ポインター型である必要があります。
- `unmanaged` 制約を satsifies ユーザー定義構造体であること。

自動実装されたプロパティなど、コンパイラによって生成されるインスタンスフィールドも、これらの制約を満たしている必要があります。 

次に例を示します。

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

`unmanaged` 制約は、`struct`、`class`、または `new()`と組み合わせることはできません。 この制限は、`unmanaged` が `struct` を意味するため、その他の制約は意味を持たないからです。

`unmanaged` 制約は、CLR によって適用されるのではなく、言語によってのみ適用されます。 他の言語による mis の使用を防ぐために、この制約を持つメソッドは mod 要求によって保護されます。これにより、アンマネージ型ではない型引数が他の言語で使用されるのを防ぐことができます。

制約内の `unmanaged` トークンはキーワードでもコンテキストキーワードでもありません。 代わりに、その場所で評価され、次のいずれかの方法で `var` のようになります。

- `unmanaged`という名前のユーザー定義または参照型にバインドします。これは、他の名前付きの型制約が処理されるのと同じように扱われます。 
- No type にバインドする: これは、`unmanaged` 制約として解釈されます。

`unmanaged` という名前の型があり、現在のコンテキストに限定せずに使用できる場合、`unmanaged` 制約を使用する方法はありません。 これは、機能 `var` を囲む規則と、同じ名前のユーザー定義型に似ています。 

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

この機能の主な欠点は、少数の開発者が提供することです。通常は、低レベルのライブラリ作成者またはフレームワークです。  そのため、少数の開発者にとって貴重な言語の時間が費やされています。 

ただし、多くの場合、これらのフレームワークは .NET アプリケーションの大半の基礎となります。  そのため、このレベルではパフォーマンスと正確性が優先され、.NET エコシステムに波及効果を与えることができます。  これにより、対象ユーザーが制限されていても、この機能を考慮する価値があります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

考慮すべき選択肢がいくつかあります。

- 現状では、この機能は独自のメリットには合わないため、開発者は暗黙的なオプトイン動作を引き続き使用します。

## <a name="questions"></a>疑問がある場合
[quesions]: #questions

### <a name="metadata-representation"></a>メタデータ表現

F#言語では、署名ファイル内の制約がエンコードC#されます。これは、その表現を再利用できないことを意味します。 この制約には、新しい属性を選択する必要があります。 さらに、この制約を持つメソッドは、mod 要求によって保護されている必要があります。

### <a name="blittable-vs-unmanaged"></a>Blittable とアンマネージ
このF#言語には、アンマネージキーワードを使用する非常に[よく似た機能](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints)があります。 Blittable 名は、Midori の使用から取得されます。  ここで優先順位を指定し、代わりにアンマネージを使用することをお勧めします。 

**解決策**アンマネージドを使用することを決定する言語 

### <a name="verifier"></a>検証ツール

ジェネリック型パラメーターへのポインターの使用を理解するために、検証ツール/ランタイムを更新する必要がありますか。  または、変更せずにそのまま機能しますか。

**解決策**変更は必要ありません。 すべてのポインター型は単に検証できません。 

## <a name="design-meetings"></a>会議のデザイン

該当なし
