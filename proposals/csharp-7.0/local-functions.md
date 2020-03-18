---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483446"
---
# <a name="local-functions"></a>ローカル関数

ブロックスコープC#内の関数の宣言をサポートするようにを拡張します。 ローカル関数は、外側のスコープから変数を使用 (キャプチャ) できます。

コンパイラは、フロー分析を使用して、ローカル関数が値を割り当てる前に、どの変数を使用するかを検出します。 関数を呼び出すたびに、このような変数を確実に代入する必要があります。 同様に、コンパイラは、戻り時に確実に代入される変数を決定します。 このような変数は、ローカル関数が呼び出された後に確実に割り当てられたと見なされます。

ローカル関数は、定義の前の構文ポイントから呼び出すことができます。 ローカル関数宣言ステートメントは、到達できない場合に警告を発生させません。

TODO:_書き込み仕様_

## <a name="syntax-grammar"></a>構文文法

この文法は、現在の仕様の文法と比較して表されます。

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

ローカル関数では、外側のスコープで定義された変数を使用できます。 現在の実装では、ローカル関数内で読み取られるすべての変数が、定義の時点でローカル関数を実行する場合と同様に、確実に代入される必要があります。 また、ローカル関数の定義は、任意の使用ポイントで "実行" されている必要があります。

そのビットを試してみると (たとえば、2つの相互再帰的なローカル関数を定義することはできません)、明確な代入を確実に行う方法を変更しました。 リビジョン (まだ実装されていません) は、ローカル関数を呼び出すたびに、ローカル関数で読み込まれるすべてのローカル変数が確実に割り当てられる必要があります。 実際には、それほど巧妙ではなく、作業を行うために残っている作業がいくつかあります。 この処理が完了すると、ローカル関数をそれを囲むブロックの末尾に移動できるようになります。

新しい明確な代入規則は、ローカル関数の戻り値の型を推論することと互換性がないため、戻り値の型を推論するためのサポートが削除される可能性があります。

ローカル関数をデリゲートに変換しない限り、キャプチャは値型のフレームに対して行われます。 これは、キャプチャでローカル関数を使用することによる GC の圧力が得られないことを意味します。

### <a name="reachability"></a>経由で到達可能

仕様に追加します。

> ステートメント形式のラムダ式またはローカル関数の本体は、到達可能と見なされます。
