---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484142"
---
# <a name="support-for--and--on-tuple-types"></a>タプル型での = = および! = のサポート

式を許可します。 `t1` と `t2` が同じカーディナリティのタプル型または null 許容型のタプル型であり、それらをほぼ `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` (`var temp1 = t1; var temp2 = t2;`と想定) として評価 `t1 == t2` ます。

逆に、`t1 != t2` を許可し、`temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`として評価します。

Null 許容の場合は、`temp1.HasValue` と `temp2.HasValue` の追加チェックが使用されます。 たとえば、`nullableT1 == nullableT2` は `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`として評価されます。

要素ごとの比較でブール以外の結果が返された場合 (たとえば、ブール型以外のユーザー定義 `operator ==` または `operator !=` が使用されている場合、または動的比較の場合)、その結果は `bool` に変換されるか、`operator true` または `operator false` を使用して実行され、`bool`を取得します。 タプルの比較では、常に `bool`が返されます。

7\.2 のC#場合、ユーザー定義の `operator==`がない限り、このようなコードではエラー (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`) が生成されます。

## <a name="details"></a>詳細

`==` (または `!=`) 演算子をバインドする場合、既存のルールは次のようになります (1) 動的なケース、(2) オーバーロードの解決、(3) が失敗します。
この提案は、(1) と (2) の間に組の大文字と小文字を追加します。比較演算子の両方のオペランドがタプル (タプル型またはタプルリテラル) で、一致するカーディナリティがある場合、比較は要素ごとに実行されます。 この組の等値は、null 許容の組にもリフトされています。

両方のオペランド (および組リテラルの場合は、その要素) が左から右に評価されます。 次に、要素の各ペアをオペランドとして使用して、演算子 `==` (または `!=`) を再帰的にバインドします。 コンパイル時の型 `dynamic` を持つ要素では、エラーが発生します。 これらの要素ごとの比較の結果は、条件演算子と (または) 演算子のチェーンでオペランドとして使用されます。

たとえば、`(int, (int, int)) t1, t2;`のコンテキストでは、`t1 == (1, (2, 3))` は `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`として評価されます。

組リテラルをオペランドとして使用すると (両側で)、要素ごとの変換によって形成される、変換されたタプル型を受け取ります。これは、演算子を `==` (または `!=`) 要素ごとにバインドするときに導入されます。 

たとえば、`(1L, 2, "hello") == (1, 2L, null)`では、両方の組リテラルの変換後の型が `(long, long, string)`、2番目のリテラルには自然型がありません。


### <a name="deconstruction-and-conversions-to-tuple"></a>分解と組への変換
`(a, b) == x`では、`x` が2つの要素に分解ことができるという事実は、役割を果たしません。 これは今後の提案になる可能性がありますが、`x == y` に関する質問が発生する可能性があります (これは、単純な比較または要素ごとの比較ですが、カーディナリティを使用している場合)。
同様に、タプルへの変換はロールを持ちません。

### <a name="tuple-element-names"></a>タプル要素名

タプルリテラルを変換するときに、リテラルに明示的なタプル要素名が指定されているが、ターゲットのタプル要素名と一致しない場合に警告します。
組の比較でも同じ規則を使用するので、`t == (c, d: 0)`で `d` に対して警告 `(int a, int b) t` ことを前提としています。

### <a name="non-bool-element-wise-comparison-results"></a>Bool 以外の要素ごとの比較結果

要素ごとの比較が組の等価性で動的である場合は、演算子 `false` の動的呼び出しを使用して、`bool` を取得し、さらに要素ごとの比較を続行するようにします。 

要素ごとの比較によってタプル内の他の非 bool 型が返される場合は、次の2つのケースがあります。
- ブール型以外の型が `bool`に変換される場合は、その変換を適用します。
- そのような変換がないが、型に `false`演算子がある場合は、それを使用して結果を否定します。

組の非等値では、演算子 `false`の代わりに演算子 `true` (否定なし) を使用する点を除いて、同じ規則が適用されます。

これらの規則は、`if` ステートメントおよびその他の既存のコンテキストでブール型以外の型を使用する場合に関係する規則に似ています。

## <a name="evaluation-order-and-special-cases"></a>評価順序と特別なケース
左側の値が最初に評価され、次に右側の値が評価されます。次に、左から右 (変換を含む) と、条件付き演算子または OR 演算子の既存の規則に基づく早期終了を含む要素ごとの比較が行われます。

たとえば、型 `A` から型 `B` とメソッド `(A, A) GetTuple()`の変換がある場合、`(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` を評価すると次のようになります。
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- 次に、要素ごとの変換と比較および条件ロジックが評価されます (`new A(1)` を型 `B`に変換し、それを `new B(4)`と比較します)。

### <a name="comparing-null-to-null"></a>`null` と `null` の比較

これは、通常の比較の特殊なケースで、タプルの比較に引き継がれます。 `null == null` 比較が許可され、`null` リテラルには型が一切取得されません。
タプルの等価性では、これは `(0, null) == (0, null)` も許可され、`null` と組リテラルは型を取得しません。

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a>Null 許容の構造体と `null` を比較した場合 `operator==`

これは、タプルの比較に関する別の特殊なケースです。
`operator==`のない `struct S` がある場合は、`(S?)x == null` の比較が許可され、`((S?).x).HasValue`として解釈されます。
タプルの等価性では、同じルールが適用されるので `(0, (S?)x) == (0, null)` が許可されます。

## <a name="compatibility"></a>互換性

比較演算子の実装を使用して、他のユーザーが独自の `ValueTuple` 型を記述した場合は、オーバーロードの解決によって以前に取得されていた可能性があります。 ただし、新しい組のケースはオーバーロードの解決の前にあるため、ユーザー定義の比較に頼るのではなく、タプルの比較でこのケースを処理します。

----

[関係演算子および型テスト演算子](../../spec/expressions.md#relational-and-type-testing-operators)に関連する[#190](https://github.com/dotnet/csharplang/issues/190)