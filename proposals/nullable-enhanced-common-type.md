---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483524"
---
# <a name="nullable-enhanced-common-type"></a>Null 許容型拡張共通型

* [x] が提案されています
* [] プロトタイプ: なし
* [] の実装: なし
* [] 仕様: 以下を参照してください

## <a name="summary"></a>まとめ
[summary]: #summary

現在の共通型アルゴリズムの結果が非常に直観的で、コードに冗長なキャストのような感覚を追加するという状況があります。 この変更により、`condition ? 1 : null` などの式では `int?`型の値が返され、`condition ? x : 1.0` などの式では、`x` が型 `int?` の場合は `double?`型の値になります。

## <a name="motivation"></a>目的
[motivation]: #motivation

これは、不要な定型コードのようにプログラマが感じていることの一般的な原因です。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

次の状況に影響を与える、[一連の式の中で最適な共通型を見つける](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions)ための仕様を変更します。

- 一方の式が null 非許容の値型 `T` で、もう一方が null リテラルの場合、結果は `T?`型になります。
- 一方の式が null 許容値型 `T?` であり、もう一方の式が値型 `U`であり、`T` から `U`への暗黙的な変換がある場合、結果は `U?`型になります。

これは、次の言語の側面に影響します。

- [三項式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)
- 暗黙的に型指定された[配列作成式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)
- 型の推定に関する[ラムダの戻り値の型](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type)の推論
- `M(1, null)`としての `M<T>(T a, T b)` の呼び出しなど、ジェネリックを含むケース。

詳細については、仕様の次のセクションを変更します (太字で挿入し、取り消し線を削除します)。

> #### <a name="output-type-inferences"></a>出力の型の推論
> 
> *出力型の推論*は、次のように `T` 型*に*`E` 式*から*実行されます。
> 
> *  `E` が、推論された戻り値の型 `U` ([推定戻り値の型](expressions.md#inferred-return-type)) の匿名関数で、`T` が戻り値の型が `Tb`であるデリゲート型または式ツリー型である場合は、*下限の推論*([下限](expressions.md#lower-bound-inferences)の推論) が `U`*から*`Tb`*に*作成されます。
> *  それ以外の場合、`E` がメソッドグループで、`T` がパラメーター型 `T1...Tk` および戻り値の型 `Tb`を持つデリゲート型または式ツリー型であり、型 `E` のオーバーロード解決では、戻り値の型が *`T1...Tk` の 1*つのメソッドが生成*to* *され*ます。`U``U``Tb`
> *  \* * それ以外の場合、`E` が null 許容値型 `U?`の式である場合、`U`*から*`T`*に* *下限の推定*が行われ、 *null バインド*が `T`に追加されます。 **
> *  それ以外の場合、`E` が `U`型の式である場合、`U`*から*`T`*に* *下限の推論*が行われます。
> *  **それ以外の場合、`E` が値 `null`の定数式である場合、 *null バインド*がに追加され `T`** 
> *  それ以外の場合、推論は行われません。

> #### <a name="fixing"></a>写真の
> 
> 一連の境界を持つ固定されてい*ない型変数*`Xi` は、次のように*固定*されています。
> 
> *  *候補の種類*のセット `Uj`、`Xi`の一連の境界内のすべての型のセットとして開始されます。
> *  次に、それぞれのバインドされている `Xi` を確認します。 `Xi` の完全バインド `U` については、`U` と同じではないすべての型 `Uj` を候補セットから削除します。 `Xi` の下限 `U` ごとに、`U` からの暗黙の変換が*行われていない*すべての型 `Uj` 候補セットから削除されます。 `U` への暗黙的な変換が*行われていない*すべての型 `Uj` `Xi` の上限 `U` は、候補セットから削除されます。
> *  他の候補の型の中に、他のすべての候補の型への暗黙の型変換が存在する `Uj` 一意の型 `V` がある場合、 ~~`Xi` は `V`に固定されます。~~
>     -  **`V` が値型で、`Xi`に null が*バインド*されている場合、`Xi` はに固定され `V?`**
>     -  **それ以外の場合 `Xi` は `V` に固定されます。**
> *  それ以外の場合、型の推定は失敗します。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

この提案によって導入された非互換性がある可能性があります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

[なし] :

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- [] この提案によって導入された非互換性の重要度 (存在する場合) と、それをモデレートする方法を指定します。

## <a name="design-meetings"></a>会議のデザイン

[なし] :
