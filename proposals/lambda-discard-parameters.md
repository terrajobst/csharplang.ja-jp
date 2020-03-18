---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2019
ms.locfileid: "79484058"
---
# <a name="lambda-discard-parameters"></a>ラムダ破棄のパラメーター

## <a name="summary"></a>まとめ

破棄 (`_`) をラムダおよび匿名メソッドのパラメーターとして使用できるようにします。
次に例を示します。
- ラムダ: `(_, _) => 0`、`(int _, int _) => 0`
- 匿名メソッド: `delegate(int _, int _) { return 0; }`

## <a name="motivation"></a>目的

未使用のパラメーターに名前を付ける必要はありません。 破棄の目的は明確ではありません。つまり、未使用または破棄されます。

## <a name="detailed-design"></a>詳細なデザイン

[メソッドパラメーター](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters)`_`という名前のパラメーターが複数あるラムダまたは匿名メソッドのパラメーターリストでは、このようなパラメーターは破棄パラメーターです。
注: 1 つのパラメーターに `_` という名前が付けられている場合は、旧バージョンとの互換性を保つために通常のパラメーターになります。

破棄パラメーターでは、どのスコープにも名前は導入されません。
これは、`_` (アンダースコア) 名が非表示になることを意味します。

[簡易名](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names)`K` がゼロで、 *simple_name*が*ブロック*内に存在し、*ブロック*の (またはそれを囲む*ブロック*の) ローカル変数宣言領域 ([宣言](basic-concepts.md#declarations)) にローカル変数、パラメーター (破棄パラメーターを除く)、または名前が `I`の定数が含まれている場合、 *simple_name*はそのローカル変数、パラメーター、または定数を参照し、変数または値として分類

[スコープ](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes)Discard パラメーターを除き、 *lambda_expression* ([匿名関数式](expressions.md#anonymous-function-expressions)) で宣言されたパラメーターのスコープは、discard パラメーターを除き、 *lambda_expression*の*anonymous_function_body*であり、 *anonymous_method_expression* ([匿名関数式](expressions.md#anonymous-function-expressions)) で宣言されたパラメーターのスコープはその*anonymous_method_expression*の*ブロック*です。

## <a name="related-spec-sections"></a>関連するスペックセクション
- [対応するパラメーター](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
