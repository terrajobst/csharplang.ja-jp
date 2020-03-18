---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483860"
---
# <a name="expression-variables-in-initializers"></a>初期化子における式の変数

## <a name="summary"></a>まとめ
[summary]: #summary

7でC#導入された機能を拡張して、フィールド初期化子、プロパティ初期化子、.ctor 初期化子、およびクエリ句で式変数 (out 変数の宣言と宣言パターン) を含む式を許可します。

## <a name="motivation"></a>目的
[motivation]: #motivation

これにより、時間が不足しているためC# 、言語のいくつかの大まかな端が完成します。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

.Ctor 初期化子の式変数 (out 変数宣言と宣言パターン) の宣言を禁止する制限を削除します。 このように宣言された変数は、コンストラクターの本体全体でスコープ内にあります。

フィールドまたはプロパティ初期化子での式変数の宣言 (out 変数の宣言と宣言パターン) を禁止する制限を削除します。 このように宣言された変数は、初期化式全体でスコープ内にあります。

ラムダの本体に変換されるクエリ式の句で、式変数 (out 変数宣言と宣言パターン) の宣言を禁止する制限を削除します。 このように宣言された変数は、クエリ句の式全体でスコープ内にあります。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

[なし] :

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

これらのコンテキストで宣言された式変数の適切な範囲は明らかではなく、さらに LDM による説明です。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- [] これらの変数の適切なスコープは何ですか。

## <a name="design-meetings"></a>会議のデザイン

[なし] :
