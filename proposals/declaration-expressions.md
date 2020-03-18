---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483566"
---
# <a name="declaration-expressions"></a>宣言式

式としての宣言の割り当てをサポートします。

## <a name="motivation"></a>目的
[motivation]: #motivation

多くの場合、宣言の時点で初期化を許可し、コードを簡略化し、`var` を使用できるようにします。

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

`out var`に似た `ref` 引数の宣言を許可します。

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

式は、宣言の割り当てを含むように拡張されます。 優先順位は割り当てと同じです。

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

宣言の割り当ては、単一のローカルのです。

宣言代入式の型は、宣言の型です。
型が `var`の場合、推論される型は初期化式の型になります。 

宣言代入式は、特に `ref` 引数値の左辺値にすることができます。

宣言代入式で値型が宣言され、式が右辺値の場合、式の値はコピーになります。

宣言代入式では、ローカル `ref` を宣言できます。
`ref` 引数の宣言式に `ref` が使用されている場合、あいまいさが発生します。
ローカル変数初期化子は、宣言がローカル `ref` であるかどうかを判断します。

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

宣言代入式で宣言されたローカルのスコープは、C # 7.0 の対応する宣言式のスコープと同じです。

宣言式の前にあるローカルのをテキストで参照すると、コンパイル時エラーになります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives
変更はありません。 この機能は、構文の省略形にすぎません。

一般的なシーケンス式については、「 [#377](https://github.com/dotnet/csharplang/issues/377)」を参照してください。

`var` を使用できるようにするには、`var` ローカルの個別の宣言と割り当てを許可し、すべてのコードパスからの割り当てから型を推論します。

## <a name="see-also"></a>参照
[see-also]: #see-also
[#595](https://github.com/dotnet/csharplang/issues/595)の「基本宣言式」を参照してください。

[分解](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md)機能の「分解宣言」を参照してください。
