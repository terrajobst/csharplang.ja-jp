---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483632"
---
# <a name="pattern-based-fixed-statement"></a>パターン ベースの `fixed` ステートメント

## <a name="summary"></a>まとめ
[summary]: #summary

`fixed` ステートメントに型を含めることができるパターンを導入します。 

## <a name="motivation"></a>目的
[motivation]: #motivation

この言語は、マネージデータをピン留めし、基になるバッファーへのネイティブポインターを取得するためのメカニズムを提供します。

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

`fixed` に参加できる型のセットはハードコーディングされており、配列と `System.String`に制限されています。 `ImmutableArray<T>`、`Span<T>`、`Utf8String` などの新しいプリミティブが導入された場合、"特別な" 型をハードコーディングすることはできません。 

さらに、`System.String` の現在のソリューションは、非常に厳密な API に依存しています。 API の構造は、`System.String` が、オブジェクトヘッダーからの固定オフセットで、UTF16 でエンコードされたデータを埋め込む連続したオブジェクトであることを意味します。 このような方法は、基になるレイアウトを変更する必要があるいくつかの提案で問題が見つかりました。 `System.String` オブジェクトを、アンマネージ相互運用の目的で内部表現から分離することで、より柔軟なものに切り替えることができます。 

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

## <a name="pattern"></a>*パターン* ##
実行可能なパターンベースの "固定" のニーズは次のとおりです。
-   インスタンスをピン留めするためのマネージ参照を指定し、ポインターを初期化します (これは同じ参照であることが望ましい)
-   アンマネージ要素の型 ("string" の "char" など) を明確に伝達します。
-   参照するものがない場合、"empty" の場合の動作を指定します。 
-   `fixed`の外部で型を使用するのを困難にする設計上の決定に対して、API 作成者をプッシュしないでください。

前述のように、特別に名前が付けられた ref 戻りメンバーを認識することで、上記の条件を満たすことができます。 `ref [readonly] T GetPinnableReference()`。

`fixed` ステートメントで使用するには、次の条件を満たす必要があります。

1. 型に対して指定されたメンバーは1つだけです。
1. `ref` または `ref readonly`によってを返します。 (`readonly` は、安全なコードで使用できる書き込み可能な API を追加せずに、変更できない型または readonly の型の作成者がパターンを実装できるようにすることが許可されています)
1. T はアンマネージ型です。
(`T*` がポインター型になるためです。 制限は、"アンマネージ" の概念が展開されている場合には自然に拡張されます)
1. ピン留めするデータがない場合に、マネージ `nullptr` を返します。これは、空である可能性が最も低い方法である可能性があります。
(文字列が null で終わるため、"" という文字列は ' \ 0 ' への参照を返すことに注意してください)

また `#3` では、空のケースの結果が未定義または実装固有であることを許可できます。 ただし、これにより、API の危険性が高くなり、誤用や意図しない互換性の負担が発生しやすくなります。 

## <a name="translation"></a>*翻訳* ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

は次の擬似コードになります ( C#では表現できないものがあります)

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

- GetPinnableReference は `fixed`でのみ使用することを意図していますが、セーフコードでは使用できません。実装では、この点を念頭に置く必要があります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

ユーザーは GetPinnableReference または類似したメンバーを導入し、
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

代替ソリューションが必要な場合は、`System.String` の解決策はありません。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- [] 動作が "empty" 状態になります。`nullptr` または `undefined` を  - しますか? 
- [] 拡張メソッドを考慮する必要がありますか。 
- [] `System.String`でパターンが検出された場合、それを勝ちにしますか? 

## <a name="design-meetings"></a>会議のデザイン

まだありません。 
