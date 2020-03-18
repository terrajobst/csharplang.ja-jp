---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483686"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a>タプル名 (別名) を推測します。 タプルプロジェクション初期化子)

## <a name="summary"></a>まとめ
[summary]: #summary

多くの一般的なケースでは、この機能によりタプル要素名を省略し、代わりに推論することができます。 たとえば、`(f1: x.f1, f2: x?.f2)`を入力する代わりに、要素名 "f1" と "f2" を `(x.f1, x?.f2)`から推論できます。

これは匿名型の動作と同じであり、作成時にメンバー名を推論できます。 たとえば、`new { x.f1, y?.f2 }` メンバー "f1" と "f2" を宣言します。

これは、LINQ でタプルを使用する場合に特に便利です。

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

変更には次の2つの部分があります。

1.  明示的な名前を持たない各タプル要素の候補名を推測します。
    -   匿名型の名前の推論と同じ規則を使用します。
        - でC#は、`y` (識別子)、`x.y` (単純メンバーアクセス)、および `x?.y` (条件付きアクセス) という3つのケースが可能です。
        - VB では、`x.y()`などの追加のケースに対応できます。
    -   予約された組名の拒否 ( C#では大文字と小文字が区別されますが、VB では大文字と小文字は区別されません)。これらは禁止または暗黙的です たとえば、`ItemN`、`Rest`、`ToString`などです。
    -   組全体で、候補名が重複しているC#場合 (大文字と小文字が区別され、VB では大文字と小文字が区別されます)、これらの候補は削除されます。
2.  変換中に (タプルリテラルからの名前の削除を確認して警告する)、推論された名前は警告を生成しません。 これにより、既存の組コードの破損を回避できます。

重複を処理するルールは、匿名型の場合とは異なります。 たとえば、`new { x.f1, x.f1 }` ではエラーが生成されますが、推論された名前がなくても `(x.f1, x.f1)` は引き続き許可されます。 これにより、既存の組コードの破損を回避できます。

一貫性を確保するために、分解によって生成されるタプル ( C#) にも同じことが当てはまります。

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

同じことが VB のタプルにも適用されます。 vb 固有のルールを使用して、式から名前を推論し、大文字と小文字を区別しない名前比較を行います。

言語バージョン " C# 7.0" で7.1 コンパイラ (またはそれ以降) を使用する場合、要素名は (機能が使用されていないにもかかわらず) 推定されますが、アクセスしようとしたときに使用するサイトエラーが発生します。 これにより、後で互換性の問題が発生する新しいコード (以下で説明) の追加が制限されます。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

主な欠点は、 C# 7.0 との互換性が失われることです。

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

互換性の問題を検出した場合は、制限されていることを確認してください。 C#また、(7.0 で) 組が発送されてからの時間枠は短くなっています。

## <a name="references"></a>References
- [2017年4月4日](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- [Github のディスカッション](https://github.com/dotnet/csharplang/issues/370)(この問題を発生させるための @alrz に感謝)
- [タプルの設計](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
