---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483674"
---
# <a name="pattern-matching-with-generics"></a>ジェネリックを使用したパターン一致

* [x] が提案されています
* [] プロトタイプ:
* [] の実装:
* [] 仕様:

## <a name="summary"></a>まとめ
[summary]: #summary

[ C#既存の as 演算子](../../spec/expressions.md#the-as-operator)の仕様では、がオープン型である場合、オペランドの型と指定された型の間で変換を行うことはできません。 ただし、7 C#では、`Type identifier` パターンでは、入力の型と指定された型の間の変換が必要です。

`expression is Type identifier`この問題を緩和し、7でC#許可されている条件で許可されていることに加え、`expression as Type` が許可された場合にも許可されるようにすることをお勧めします。 具体的には、式または指定された型がオープン型である場合に、新しいケースが発生します。 

## <a name="motivation"></a>目的
[motivation]: #motivation

現在、パターンマッチングが "明らか" に許可されている場合は、コンパイルに失敗します。 例については、 https://github.com/dotnet/roslyn/issues/16195を参照してください。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

パターンマッチングの仕様の段落を変更します (提案された追加は太字で示されています)。

> 左側の静的な型と指定された型の特定の組み合わせは互換性がないと見なされ、コンパイル時エラーが発生します。 静的な型 `E` の値は、id 変換、暗黙の参照変換、ボックス変換、明示的な参照変換、または `E` から `T`へのアンボックス変換、または **`E` または `T` のいずれかがオープン型**である場合に、型 `T` との*パターン互換性*があると言われます。 `E` 型の式が、照合される型パターンの型と互換性のあるパターンでない場合、コンパイル時エラーになります。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

[なし] :

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

[なし] :

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

[なし] :

## <a name="design-meetings"></a>会議のデザイン

LDM はこの質問を検討し、バグ修正レベルの変更であると考えました。 言語がリリースされた後に変更を加えるだけで、上位互換性がなくなるため、これを別の言語機能として扱います。 提案された変更を使用するには、プログラマが言語バージョン7.1 を指定する必要があります。
