---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483980"
---
# <a name="recursive-pattern-matching"></a>再帰的なパターンマッチング

## <a name="summary"></a>まとめ
[summary]: #summary

パターンマッチング拡張機能C#を使用すると、代数データ型と関数型言語のパターンマッチングの多くの利点が得られますが、基になる言語の感覚とスムーズに統合されます。 このアプローチの要素は、プログラミング言語[F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "軽量言語を使用した拡張可能なパターンマッチング")の関連機能と、[スケール a](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "オブジェクトとパターンの一致、ページ273")によって実現されています。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

### <a name="is-expression"></a>Is 式

`is` 演算子は、*パターン*に対して式をテストするために拡張されています。

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

この形式の*relational_expression*は、 C#仕様に含まれる既存のフォームに追加されます。 `is` トークンの左側の*relational_expression*が値を指定していないか、型を持たない場合、コンパイル時エラーになります。

パターンのすべての*識別子*は、`is` 演算子が `true` た後に*確実に割り当てら*れる新しいローカル変数を導入します (つまり、 *true の場合は確実に割り当て*られます)。

> メモ: 厳密には、`is-expression` と*constant_pattern*の*型*の間にあいまいさがあります。どちらも、修飾された識別子の有効な解析である可能性があります。 以前のバージョンの言語との互換性のために、型としてバインドしようとしています。このエラーが発生した場合にのみ、他のコンテキストで式を実行するときに、最初に見つかったもの (定数または型のいずれかである必要があります) に対して解決されます。 このあいまいさは、`is` 式の右辺にのみ存在します。

### <a name="patterns"></a>パターン

パターンは、 *is_pattern*演算子、 *switch_statement*、および*switch_expression*内で使用され、入力データ (入力値と呼ばれる) が比較されるデータの構造を表します。 パターンは再帰的であるため、データの一部がサブパターンと照合される可能性があります。

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a>宣言パターン

```antlr
declaration_pattern
    : type simple_designation
    ;
```

*Declaration_pattern*は、式が特定の型であるかどうかをテストし、テストが成功した場合はその型にキャストします。 指定された型が*single_variable_designation*である場合、指定された識別子によって指定された型のローカル変数を導入する可能性があります。 このローカル変数は、パターン一致操作の結果が `true`ときに*確実に割り当てら*れます。

この式のランタイムセマンティックは、パターンの*型*に対して左辺の*relational_expression*オペランドのランタイム型をテストすることです。  そのランタイム型 (または一部のサブタイプ) で `null`ではない場合、`is operator` の結果は `true`ます。

左側の静的な型と指定された型の特定の組み合わせは互換性がないと見なされ、コンパイル時エラーが発生します。 静的な型 `E` の値は、id 変換、暗黙の参照変換、ボックス化変換、明示的な参照変換、または `E` から `T`へのアンボックス変換、または型のいずれかがオープン型である場合に、型 `T` との*パターン互換*と呼ばれます。 `E` 型の入力が、照合される型パターンの*型*とパターンとの*互換性*がない場合、コンパイル時エラーになります。

型パターンは、参照型のランタイム型テストを実行する場合に便利です。

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

少し簡潔に

```csharp
if (expr is Type v) { // code using v
```

*型*が null 許容値型の場合、エラーになります。

型パターンは、null 許容型の値をテストするために使用できます。型 `Nullable<T>` (またはボックス化された `T`) の値が null 以外で、`T2` の型が `T`である場合、または `T`の基本型またはインターフェイスがある場合は、型 `T2 id` パターンと一致します。 たとえば、コード片で

```csharp
int? x = 3;
if (x is int v) { // code using v
```

`if` ステートメントの条件は実行時に `true` され `v` 変数は、ブロック内の `int` 型の値 `3` を保持します。 ブロックの後には、変数 `v` がスコープ内にありますが、確実に割り当てられるわけではありません。

#### <a name="constant-pattern"></a>定数パターン

```antlr
constant_pattern
    : constant_expression
    ;
```

定数パターンは、定数値に対して式の値をテストします。 定数は、リテラル、宣言された `const` 変数の名前、列挙定数など、任意の定数式にすることができます。 入力値がオープン型でない場合、定数式は、一致した式の型に暗黙的に変換されます。入力値の型が定数式の型とパターンとの*互換性*がない場合、パターン一致操作はエラーになります。

パターン*c*は、`object.Equals(c, e)` が `true`を返す場合に、変換された入力値*e*と一致していると見なされます。

ユーザー定義の `operator==`を呼び出すことができないため、新しく作成されたコードで `null` をテストするための最も一般的な方法として `e is null` が表示されることが予想されます。

#### <a name="var-pattern"></a>Var パターン

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

*指定*が*simple_designation*の場合、式*e*はパターンと一致します。 言い換えると、 *var パターン*との一致は常に*simple_designation*で成功します。 *Simple_designation*が*single_variable_designation*の場合、 *e*の値は新しく導入されたローカル変数の範囲になります。 ローカル変数の型は、 *e*の静的な型です。

*指定*されている*tuple_designation*の場合、パターンは `(var`*指定*の形式の*positional_pattern*に相当します。 `)` は、 *tuple_designation*内で検出さ*れたもの*です。  たとえば、`var (x, (y, z))` パターンは `(var x, (var y, var z))`と同じです。

名前 `var` 型にバインドされている場合、エラーになります。

#### <a name="discard-pattern"></a>パターンの破棄

```antlr
discard_pattern
    : '_'
    ;
```

式*e*は、パターン `_` 常に一致します。 つまり、すべての式は、破棄パターンに一致します。

破棄パターンを*is_pattern_expression*のパターンとして使用することはできません。

#### <a name="positional-pattern"></a>位置指定パターン

位置パターンは、入力値が `null`されていないことを確認し、適切な `Deconstruct` メソッドを呼び出し、結果の値に対してさらにパターン一致を実行します。  入力値の型が `Deconstruct`を含む型と同じ場合、または入力値の型がタプル型である場合、または入力値の型が `object` または `ITuple` であり、式のランタイム型が `ITuple`を実装している場合には、型を指定せずに、組のようなパターン構文もサポートします。

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

*型*を省略した場合は、入力値の静的な型になります。

入力値がパターンの*種類*`(` *subpattern_list* `)`と一致した場合は、*型*を検索して `Deconstruct` のアクセス可能な宣言を検索し、分解宣言と同じルールを使用してその中の1つを選択することで、メソッドが選択されます。

*Positional_pattern*が型を省略し、*識別子*のないサブ*パターン*が1つある場合、 *property_subpattern*がなく、 *simple_designation*がない場合、エラーになります。 これは、かっこで囲まれた*constant_pattern*と*positional_pattern*で明確ます。

リスト内のパターンと照合する値を抽出するには、
- *Type*が省略され、入力値の型がタプル型である場合、サブパターンの数は組のカーディナリティと同じである必要があります。 各タプル要素は対応するサブ*パターン*と照合され、すべて成功した場合は一致と見なされます。 サブ*パターン*に*識別子*がある場合は、タプル型の対応する位置にタプル要素の名前を指定する必要があります。
- それ以外の場合、適切な `Deconstruct` が*型*のメンバーとして存在する場合、入力値の型が*型*と*パターン互換*でない場合、コンパイル時エラーになります。 実行時に、入力値が*型*に対してテストされます。 これが失敗した場合、位置指定パターンの照合は失敗します。 成功した場合は、入力値がこの型に変換され、コンパイラによって生成された新しい変数で `out` パラメーターを受け取る `Deconstruct` が呼び出されます。 受信した各値は対応するサブ*パターン*と照合され、すべて成功した場合は一致と見なされます。 サブ*パターン*に*識別子*がある場合は、`Deconstruct`の対応する位置にパラメーター名を指定する必要があります。
- それ以外の場合、 *type*が省略され、入力値が `object` 型または `ITuple` 型、または暗黙的な参照変換によって `ITuple` に変換できる型である場合は、`ITuple`を使用して照合*されます*。
- それ以外の場合、パターンはコンパイル時エラーになります。

実行時にサブパターンが一致する順序は指定されておらず、一致しなかった場合は、すべてのサブパターンとの照合が試行されない可能性があります。

##### <a name="example"></a>例

この例では、この仕様で説明されている機能の多くを使用します。

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a>プロパティパターン

プロパティパターンは、入力値が `null` されていないことを確認し、アクセス可能なプロパティまたはフィールドの使用によって抽出された値を再帰的に一致させます。

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

_Property_pattern_のサブ_パターン_に_識別子_が含まれていない場合はエラーになります (_識別子_を持つ2番目の形式である必要があります)。  最後のサブパターンの後に続くコンマは省略可能です。

Null チェックパターンは、単純なプロパティパターンから除外されることに注意してください。 文字列 `s` が null でないかどうかを確認するには、次のいずれかの形式を記述します。

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

式 e がパターン*型*`{` *property_pattern_list* *`}`に一致*した場合、式*e*が*型*で指定された*t*型との*パターン互換*でない場合、コンパイル時エラーになります。 型が存在しない場合は、 *e*の静的な型になります。 *識別子*が存在する場合は、type*型*のパターン変数を宣言します。 *Property_pattern_list*の左側に表示される各識別子では、アクセス可能な読み取り可能なプロパティまたは*T*のフィールドを指定する必要があります。*Property_pattern*の*simple_designation*が存在する場合は、 *T*型のパターン変数を定義します。

実行時に、式は*T*に対してテストされます。これが失敗した場合、プロパティパターン一致は失敗し、結果は `false`になります。 成功した場合は、各*property_subpattern*フィールドまたはプロパティが読み取られ、その値が対応するパターンと一致します。 完全一致の結果は、これらのいずれかの結果が `false`場合にのみ `false` ます。 サブパターンが一致する順序が指定されていないため、実行時に失敗した一致がすべてのサブパターンと一致しない可能性があります。 一致が成功し、 *property_pattern*の*simple_designation*が*single_variable_designation*の場合は、一致する値が割り当てられている型*T*の変数を定義します。

> 注: プロパティパターンは、匿名型を使用したパターン一致に使用できます。

##### <a name="example"></a>例

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a>Switch 式

式コンテキストの `switch`のようなセマンティクスをサポートするために*switch_expression*が追加されました。

言語C#の構文は、次の構文を使用して強化されています。

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

*Switch_expression*は*expression_statement*として許可されていません。

> 今後のリビジョンでは、このような緩和を検討しています。

*Switch_expression*の型は、このような型が存在し、switch 式のすべての arm 内の式を暗黙的にその型に変換できる場合に、 *switch_expression_arm*の `=>` トークンの右側に表示される式の中で[*最適な*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions)型です。  また、新しい*switch 式の変換*を追加します。これは、switch 式から、各 arm の式から `T`への暗黙的な変換が存在する `T` すべての型への、定義済みの暗黙的な変換です。

前のパターンとガードが常に一致するため、一部の*switch_expression_arm*のパターンが結果に影響を与えない場合、エラーになります。

Switch 式の一部の arm では、入力のすべての値が処理される場合、スイッチ式は*完全*であると言います。  スイッチ式が*完全*でない場合、コンパイラは警告を生成する必要があります。

実行時には、 *switch_expression*の結果が、 *switch_expression*の左側の式が*switch_expression_arm*のパターンと一致し、 *case_guard*の*switch_expression_arm* (存在する場合) が `true`に評価される最初の*switch_expression_arm*の*式*の値になっています。 このような*switch_expression_arm*がない場合、 *switch_expression*は例外 `System.Runtime.CompilerServices.SwitchExpressionException`のインスタンスをスローします。

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a>タプルリテラルでの切り替え時のオプションのかっこ

*Switch_statement*を使用して組リテラルを切り替えるためには、冗長なものとして表示されるものを記述する必要があります。

```csharp
switch ((a, b))
{
```

許可する

```csharp
switch (a, b)
{
```

switch ステートメントのかっこは、切り替え先の式がタプルリテラルである場合は省略可能です。

### <a name="order-of-evaluation-in-pattern-matching"></a>パターンマッチングでの評価の順序

パターン照合中に実行される操作の順序をコンパイラで柔軟に指定すると、パターンマッチングの効率を向上させるために使用できる柔軟性を高めることができます。 (強制されていない) 要件としては、パターンでアクセスされるプロパティと分解メソッドが "純粋な" (副作用、べき等など) である必要があります。 これは、言語の概念として純度を追加するという意味ではなく、コンパイラの柔軟性を使用して操作の順序を変更できるということではありません。

**解決方法 2018-04-04 LDM**: 確認済み: コンパイラは、`ITuple`の `Deconstruct`、プロパティへのアクセス、メソッドの呼び出しに対する呼び出しの順序を変更できます。また、返された値が複数の呼び出しと同じであると見なすことができます。 コンパイラは、結果に影響を与えない関数を呼び出すことはできません。コンパイラによって生成された評価の順序を今後変更する前に、細心の注意を払ってください。

### <a name="some-possible-optimizations"></a>考えられるいくつかの最適化

パターンマッチングのコンパイルでは、パターンの共通部分を利用できます。 たとえば、 *switch_statement*内の2つの連続するパターンの最上位レベルのテストが同じ型である場合、生成されたコードは2番目のパターンの型テストをスキップできます。

いくつかのパターンが整数または文字列の場合、コンパイラは、以前のバージョンの言語では、switch ステートメントに対して生成されるものと同じ種類のコードを生成できます。

これらの最適化の詳細については、 [[Scott And Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "一致とコンパイルのヒューリスティックが重要な場合")を参照してください。
