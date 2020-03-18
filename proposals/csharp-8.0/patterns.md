---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483980"
---
# <a name="recursive-pattern-matching"></a><span data-ttu-id="da229-101">再帰的なパターンマッチング</span><span class="sxs-lookup"><span data-stu-id="da229-101">Recursive Pattern Matching</span></span>

## <a name="summary"></a><span data-ttu-id="da229-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="da229-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="da229-103">パターンマッチング拡張機能C#を使用すると、代数データ型と関数型言語のパターンマッチングの多くの利点が得られますが、基になる言語の感覚とスムーズに統合されます。</span><span class="sxs-lookup"><span data-stu-id="da229-103">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="da229-104">このアプローチの要素は、プログラミング言語[F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "軽量言語を使用した拡張可能なパターンマッチング")の関連機能と、[スケール a](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "オブジェクトとパターンの一致、ページ273")によって実現されています。</span><span class="sxs-lookup"><span data-stu-id="da229-104">Elements of this approach are inspired by related features in the programming languages [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Matching Objects With Patterns, page 273").</span></span>

## <a name="detailed-design"></a><span data-ttu-id="da229-105">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="da229-105">Detailed design</span></span>
[design]: #detailed-design

### <a name="is-expression"></a><span data-ttu-id="da229-106">Is 式</span><span class="sxs-lookup"><span data-stu-id="da229-106">Is Expression</span></span>

<span data-ttu-id="da229-107">`is` 演算子は、*パターン*に対して式をテストするために拡張されています。</span><span class="sxs-lookup"><span data-stu-id="da229-107">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="da229-108">この形式の*relational_expression*は、 C#仕様に含まれる既存のフォームに追加されます。</span><span class="sxs-lookup"><span data-stu-id="da229-108">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="da229-109">`is` トークンの左側の*relational_expression*が値を指定していないか、型を持たない場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="da229-109">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="da229-110">パターンのすべての*識別子*は、`is` 演算子が `true` た後に*確実に割り当てら*れる新しいローカル変数を導入します (つまり、 *true の場合は確実に割り当て*られます)。</span><span class="sxs-lookup"><span data-stu-id="da229-110">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="da229-111">メモ: 厳密には、`is-expression` と*constant_pattern*の*型*の間にあいまいさがあります。どちらも、修飾された識別子の有効な解析である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="da229-111">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="da229-112">以前のバージョンの言語との互換性のために、型としてバインドしようとしています。このエラーが発生した場合にのみ、他のコンテキストで式を実行するときに、最初に見つかったもの (定数または型のいずれかである必要があります) に対して解決されます。</span><span class="sxs-lookup"><span data-stu-id="da229-112">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do an expression in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="da229-113">このあいまいさは、`is` 式の右辺にのみ存在します。</span><span class="sxs-lookup"><span data-stu-id="da229-113">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="patterns"></a><span data-ttu-id="da229-114">パターン</span><span class="sxs-lookup"><span data-stu-id="da229-114">Patterns</span></span>

<span data-ttu-id="da229-115">パターンは、 *is_pattern*演算子、 *switch_statement*、および*switch_expression*内で使用され、入力データ (入力値と呼ばれる) が比較されるデータの構造を表します。</span><span class="sxs-lookup"><span data-stu-id="da229-115">Patterns are used in the *is_pattern* operator, in a *switch_statement*, and in a *switch_expression* to express the shape of data against which incoming data  (which we call the input value) is to be compared.</span></span> <span data-ttu-id="da229-116">パターンは再帰的であるため、データの一部がサブパターンと照合される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="da229-116">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

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

#### <a name="declaration-pattern"></a><span data-ttu-id="da229-117">宣言パターン</span><span class="sxs-lookup"><span data-stu-id="da229-117">Declaration Pattern</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="da229-118">*Declaration_pattern*は、式が特定の型であるかどうかをテストし、テストが成功した場合はその型にキャストします。</span><span class="sxs-lookup"><span data-stu-id="da229-118">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="da229-119">指定された型が*single_variable_designation*である場合、指定された識別子によって指定された型のローカル変数を導入する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="da229-119">This may introduce a local variable of the given type named by the given identifier, if the designation is a *single_variable_designation*.</span></span> <span data-ttu-id="da229-120">このローカル変数は、パターン一致操作の結果が `true`ときに*確実に割り当てら*れます。</span><span class="sxs-lookup"><span data-stu-id="da229-120">That local variable is *definitely assigned* when the result of the pattern-matching operation is `true`.</span></span>

<span data-ttu-id="da229-121">この式のランタイムセマンティックは、パターンの*型*に対して左辺の*relational_expression*オペランドのランタイム型をテストすることです。</span><span class="sxs-lookup"><span data-stu-id="da229-121">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span>  <span data-ttu-id="da229-122">そのランタイム型 (または一部のサブタイプ) で `null`ではない場合、`is operator` の結果は `true`ます。</span><span class="sxs-lookup"><span data-stu-id="da229-122">If it is of that runtime type (or some subtype) and not `null`, the result of the `is operator` is `true`.</span></span>

<span data-ttu-id="da229-123">左側の静的な型と指定された型の特定の組み合わせは互換性がないと見なされ、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="da229-123">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="da229-124">静的な型 `E` の値は、id 変換、暗黙の参照変換、ボックス化変換、明示的な参照変換、または `E` から `T`へのアンボックス変換、または型のいずれかがオープン型である場合に、型 `T` との*パターン互換*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="da229-124">A value of static type `E` is said to be *pattern-compatible* with a type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, or if one of those types is an open type.</span></span> <span data-ttu-id="da229-125">`E` 型の入力が、照合される型パターンの*型*とパターンとの*互換性*がない場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="da229-125">It is a compile-time error if an input of type `E` is not *pattern-compatible* with the *type* in a type pattern that it is matched with.</span></span>

<span data-ttu-id="da229-126">型パターンは、参照型のランタイム型テストを実行する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="da229-126">The type pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

<span data-ttu-id="da229-127">少し簡潔に</span><span class="sxs-lookup"><span data-stu-id="da229-127">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v
```

<span data-ttu-id="da229-128">*型*が null 許容値型の場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="da229-128">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="da229-129">型パターンは、null 許容型の値をテストするために使用できます。型 `Nullable<T>` (またはボックス化された `T`) の値が null 以外で、`T2` の型が `T`である場合、または `T`の基本型またはインターフェイスがある場合は、型 `T2 id` パターンと一致します。</span><span class="sxs-lookup"><span data-stu-id="da229-129">The type pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="da229-130">たとえば、コード片で</span><span class="sxs-lookup"><span data-stu-id="da229-130">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v
```

<span data-ttu-id="da229-131">`if` ステートメントの条件は実行時に `true` され `v` 変数は、ブロック内の `int` 型の値 `3` を保持します。</span><span class="sxs-lookup"><span data-stu-id="da229-131">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span> <span data-ttu-id="da229-132">ブロックの後には、変数 `v` がスコープ内にありますが、確実に割り当てられるわけではありません。</span><span class="sxs-lookup"><span data-stu-id="da229-132">After the block the variable `v` is in scope but not definitely assigned.</span></span>

#### <a name="constant-pattern"></a><span data-ttu-id="da229-133">定数パターン</span><span class="sxs-lookup"><span data-stu-id="da229-133">Constant Pattern</span></span>

```antlr
constant_pattern
    : constant_expression
    ;
```

<span data-ttu-id="da229-134">定数パターンは、定数値に対して式の値をテストします。</span><span class="sxs-lookup"><span data-stu-id="da229-134">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="da229-135">定数は、リテラル、宣言された `const` 変数の名前、列挙定数など、任意の定数式にすることができます。</span><span class="sxs-lookup"><span data-stu-id="da229-135">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant.</span></span> <span data-ttu-id="da229-136">入力値がオープン型でない場合、定数式は、一致した式の型に暗黙的に変換されます。入力値の型が定数式の型とパターンとの*互換性*がない場合、パターン一致操作はエラーになります。</span><span class="sxs-lookup"><span data-stu-id="da229-136">When the input value is not an open type, the constant expression is implicitly converted to the type of the matched expression; if the type of the input value is not *pattern-compatible* with the type of the constant expression, the pattern-matching operation is an error.</span></span>

<span data-ttu-id="da229-137">パターン*c*は、`object.Equals(c, e)` が `true`を返す場合に、変換された入力値*e*と一致していると見なされます。</span><span class="sxs-lookup"><span data-stu-id="da229-137">The pattern *c* is considered matching the converted input value *e* if `object.Equals(c, e)` would return `true`.</span></span>

<span data-ttu-id="da229-138">ユーザー定義の `operator==`を呼び出すことができないため、新しく作成されたコードで `null` をテストするための最も一般的な方法として `e is null` が表示されることが予想されます。</span><span class="sxs-lookup"><span data-stu-id="da229-138">We expect to see `e is null` as the most common way to test for `null` in newly written code, as it cannot invoke a user-defined `operator==`.</span></span>

#### <a name="var-pattern"></a><span data-ttu-id="da229-139">Var パターン</span><span class="sxs-lookup"><span data-stu-id="da229-139">Var Pattern</span></span>

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

<span data-ttu-id="da229-140">*指定*が*simple_designation*の場合、式*e*はパターンと一致します。</span><span class="sxs-lookup"><span data-stu-id="da229-140">If the *designation* is a *simple_designation*, an expression *e* matches the pattern.</span></span> <span data-ttu-id="da229-141">言い換えると、 *var パターン*との一致は常に*simple_designation*で成功します。</span><span class="sxs-lookup"><span data-stu-id="da229-141">In other words, a match to a *var pattern* always succeeds with a *simple_designation*.</span></span> <span data-ttu-id="da229-142">*Simple_designation*が*single_variable_designation*の場合、 *e*の値は新しく導入されたローカル変数の範囲になります。</span><span class="sxs-lookup"><span data-stu-id="da229-142">If the *simple_designation* is a *single_variable_designation*, the value of *e* is bounds to a newly introduced local variable.</span></span> <span data-ttu-id="da229-143">ローカル変数の型は、 *e*の静的な型です。</span><span class="sxs-lookup"><span data-stu-id="da229-143">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="da229-144">*指定*されている*tuple_designation*の場合、パターンは `(var`*指定*の形式の*positional_pattern*に相当します。 `)` は、 *tuple_designation*内で検出さ*れたもの*です。</span><span class="sxs-lookup"><span data-stu-id="da229-144">If the *designation* is a *tuple_designation*, then the pattern is equivalent to a *positional_pattern* of the form `(var` *designation*, ... `)` where the *designation*s are those found within the *tuple_designation*.</span></span>  <span data-ttu-id="da229-145">たとえば、`var (x, (y, z))` パターンは `(var x, (var y, var z))`と同じです。</span><span class="sxs-lookup"><span data-stu-id="da229-145">For example, the pattern `var (x, (y, z))` is equivalent to `(var x, (var y, var z))`.</span></span>

<span data-ttu-id="da229-146">名前 `var` 型にバインドされている場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="da229-146">It is an error if the name `var` binds to a type.</span></span>

#### <a name="discard-pattern"></a><span data-ttu-id="da229-147">パターンの破棄</span><span class="sxs-lookup"><span data-stu-id="da229-147">Discard Pattern</span></span>

```antlr
discard_pattern
    : '_'
    ;
```

<span data-ttu-id="da229-148">式*e*は、パターン `_` 常に一致します。</span><span class="sxs-lookup"><span data-stu-id="da229-148">An expression *e* matches the pattern `_` always.</span></span> <span data-ttu-id="da229-149">つまり、すべての式は、破棄パターンに一致します。</span><span class="sxs-lookup"><span data-stu-id="da229-149">In other words, every expression matches the discard pattern.</span></span>

<span data-ttu-id="da229-150">破棄パターンを*is_pattern_expression*のパターンとして使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="da229-150">A discard pattern may not be used as the pattern of an *is_pattern_expression*.</span></span>

#### <a name="positional-pattern"></a><span data-ttu-id="da229-151">位置指定パターン</span><span class="sxs-lookup"><span data-stu-id="da229-151">Positional Pattern</span></span>

<span data-ttu-id="da229-152">位置パターンは、入力値が `null`されていないことを確認し、適切な `Deconstruct` メソッドを呼び出し、結果の値に対してさらにパターン一致を実行します。</span><span class="sxs-lookup"><span data-stu-id="da229-152">A positional pattern checks that the input value is not `null`, invokes an appropriate `Deconstruct` method, and performs further pattern matching on the resulting values.</span></span>  <span data-ttu-id="da229-153">入力値の型が `Deconstruct`を含む型と同じ場合、または入力値の型がタプル型である場合、または入力値の型が `object` または `ITuple` であり、式のランタイム型が `ITuple`を実装している場合には、型を指定せずに、組のようなパターン構文もサポートします。</span><span class="sxs-lookup"><span data-stu-id="da229-153">It also supports a tuple-like pattern syntax (without the type being provided) when the type of the input value is the same as the type containing `Deconstruct`, or if the type of the input value is a tuple type, or if the type of the input value is `object` or `ITuple` and the runtime type of the expression implements `ITuple`.</span></span>

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

<span data-ttu-id="da229-154">*型*を省略した場合は、入力値の静的な型になります。</span><span class="sxs-lookup"><span data-stu-id="da229-154">If the *type* is omitted, we take it to be the static type of the input value.</span></span>

<span data-ttu-id="da229-155">入力値がパターンの*種類*`(` *subpattern_list* `)`と一致した場合は、*型*を検索して `Deconstruct` のアクセス可能な宣言を検索し、分解宣言と同じルールを使用してその中の1つを選択することで、メソッドが選択されます。</span><span class="sxs-lookup"><span data-stu-id="da229-155">Given a match of an input value to the pattern *type* `(` *subpattern_list* `)`, a method is selected by searching in *type* for accessible declarations of `Deconstruct` and selecting one among them using the same rules as for the deconstruction declaration.</span></span>

<span data-ttu-id="da229-156">*Positional_pattern*が型を省略し、*識別子*のないサブ*パターン*が1つある場合、 *property_subpattern*がなく、 *simple_designation*がない場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="da229-156">It is an error if a *positional_pattern* omits the type, has a single *subpattern* without an *identifier*, has no *property_subpattern* and has no *simple_designation*.</span></span> <span data-ttu-id="da229-157">これは、かっこで囲まれた*constant_pattern*と*positional_pattern*で明確ます。</span><span class="sxs-lookup"><span data-stu-id="da229-157">This disambiguates between a *constant_pattern* that is parenthesized and a *positional_pattern*.</span></span>

<span data-ttu-id="da229-158">リスト内のパターンと照合する値を抽出するには、</span><span class="sxs-lookup"><span data-stu-id="da229-158">In order to extract the values to match against the patterns in the list,</span></span>
- <span data-ttu-id="da229-159">*Type*が省略され、入力値の型がタプル型である場合、サブパターンの数は組のカーディナリティと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="da229-159">If *type* was omitted and the input value's type is a tuple type, then the number of subpatterns is required to be the same as the cardinality of the tuple.</span></span> <span data-ttu-id="da229-160">各タプル要素は対応するサブ*パターン*と照合され、すべて成功した場合は一致と見なされます。</span><span class="sxs-lookup"><span data-stu-id="da229-160">Each tuple element is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="da229-161">サブ*パターン*に*識別子*がある場合は、タプル型の対応する位置にタプル要素の名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da229-161">If any *subpattern* has an *identifier*, then that must name a tuple element at the corresponding position in the tuple type.</span></span>
- <span data-ttu-id="da229-162">それ以外の場合、適切な `Deconstruct` が*型*のメンバーとして存在する場合、入力値の型が*型*と*パターン互換*でない場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="da229-162">Otherwise, if a suitable `Deconstruct` exists as a member of *type*, it is a compile-time error if the type of the input value is not *pattern-compatible* with *type*.</span></span> <span data-ttu-id="da229-163">実行時に、入力値が*型*に対してテストされます。</span><span class="sxs-lookup"><span data-stu-id="da229-163">At runtime the input value is tested against *type*.</span></span> <span data-ttu-id="da229-164">これが失敗した場合、位置指定パターンの照合は失敗します。</span><span class="sxs-lookup"><span data-stu-id="da229-164">If this fails then the positional pattern match fails.</span></span> <span data-ttu-id="da229-165">成功した場合は、入力値がこの型に変換され、コンパイラによって生成された新しい変数で `out` パラメーターを受け取る `Deconstruct` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="da229-165">If it succeeds,  the input value is converted to this type and `Deconstruct` is invoked with fresh compiler-generated variables to receive the `out` parameters.</span></span> <span data-ttu-id="da229-166">受信した各値は対応するサブ*パターン*と照合され、すべて成功した場合は一致と見なされます。</span><span class="sxs-lookup"><span data-stu-id="da229-166">Each value that was received is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="da229-167">サブ*パターン*に*識別子*がある場合は、`Deconstruct`の対応する位置にパラメーター名を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da229-167">If any *subpattern* has an *identifier*, then that must name a parameter at the corresponding position of `Deconstruct`.</span></span>
- <span data-ttu-id="da229-168">それ以外の場合、 *type*が省略され、入力値が `object` 型または `ITuple` 型、または暗黙的な参照変換によって `ITuple` に変換できる型である場合は、`ITuple`を使用して照合*されます*。</span><span class="sxs-lookup"><span data-stu-id="da229-168">Otherwise if *type* was omitted, and the input value is of type `object` or `ITuple` or some type that can be converted to `ITuple` by an implicit reference conversion, and no *identifier* appears among the subpatterns, then we match using `ITuple`.</span></span>
- <span data-ttu-id="da229-169">それ以外の場合、パターンはコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="da229-169">Otherwise the pattern is a compile-time error.</span></span>

<span data-ttu-id="da229-170">実行時にサブパターンが一致する順序は指定されておらず、一致しなかった場合は、すべてのサブパターンとの照合が試行されない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="da229-170">The order in which subpatterns are matched at runtime is unspecified, and a failed match may not attempt to match all subpatterns.</span></span>

##### <a name="example"></a><span data-ttu-id="da229-171">例</span><span class="sxs-lookup"><span data-stu-id="da229-171">Example</span></span>

<span data-ttu-id="da229-172">この例では、この仕様で説明されている機能の多くを使用します。</span><span class="sxs-lookup"><span data-stu-id="da229-172">This example uses many of the features described in this specification</span></span>

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a><span data-ttu-id="da229-173">プロパティパターン</span><span class="sxs-lookup"><span data-stu-id="da229-173">Property Pattern</span></span>

<span data-ttu-id="da229-174">プロパティパターンは、入力値が `null` されていないことを確認し、アクセス可能なプロパティまたはフィールドの使用によって抽出された値を再帰的に一致させます。</span><span class="sxs-lookup"><span data-stu-id="da229-174">A property pattern checks that the input value is not `null` and recursively matches values extracted by the use of accessible properties or fields.</span></span>

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

<span data-ttu-id="da229-175">_Property_pattern_のサブ_パターン_に_識別子_が含まれていない場合はエラーになります (_識別子_を持つ2番目の形式である必要があります)。</span><span class="sxs-lookup"><span data-stu-id="da229-175">It is an error if any _subpattern_ of a _property_pattern_ does not contain an _identifier_ (it must be of the second form, which has an _identifier_).</span></span>  <span data-ttu-id="da229-176">最後のサブパターンの後に続くコンマは省略可能です。</span><span class="sxs-lookup"><span data-stu-id="da229-176">A trailing comma after the last subpattern is optional.</span></span>

<span data-ttu-id="da229-177">Null チェックパターンは、単純なプロパティパターンから除外されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="da229-177">Note that a null-checking pattern falls out of a trivial property pattern.</span></span> <span data-ttu-id="da229-178">文字列 `s` が null でないかどうかを確認するには、次のいずれかの形式を記述します。</span><span class="sxs-lookup"><span data-stu-id="da229-178">To check if the string `s` is non-null, you can write any of the following forms</span></span>

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

<span data-ttu-id="da229-179">式 e がパターン*型*`{` *property_pattern_list* *`}`に一致*した場合、式*e*が*型*で指定された*t*型との*パターン互換*でない場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="da229-179">Given a match of an expression *e* to the pattern *type* `{` *property_pattern_list* `}`, it is a compile-time error if the expression *e* is not *pattern-compatible* with the type *T* designated by *type*.</span></span> <span data-ttu-id="da229-180">型が存在しない場合は、 *e*の静的な型になります。</span><span class="sxs-lookup"><span data-stu-id="da229-180">If the type is absent, we take it to be the static type of *e*.</span></span> <span data-ttu-id="da229-181">*識別子*が存在する場合は、type*型*のパターン変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="da229-181">If the *identifier* is present, it declares a pattern variable of type *type*.</span></span> <span data-ttu-id="da229-182">*Property_pattern_list*の左側に表示される各識別子では、アクセス可能な読み取り可能なプロパティまたは*T*のフィールドを指定する必要があります。*Property_pattern*の*simple_designation*が存在する場合は、 *T*型のパターン変数を定義します。</span><span class="sxs-lookup"><span data-stu-id="da229-182">Each of the identifiers appearing on the left-hand-side of its *property_pattern_list* must designate an accessible readable property or field of *T*. If the *simple_designation* of the *property_pattern* is present, it defines a pattern variable of type *T*.</span></span>

<span data-ttu-id="da229-183">実行時に、式は*T*に対してテストされます。これが失敗した場合、プロパティパターン一致は失敗し、結果は `false`になります。</span><span class="sxs-lookup"><span data-stu-id="da229-183">At runtime, the expression is tested against *T*. If this fails then the property pattern match fails and the result is `false`.</span></span> <span data-ttu-id="da229-184">成功した場合は、各*property_subpattern*フィールドまたはプロパティが読み取られ、その値が対応するパターンと一致します。</span><span class="sxs-lookup"><span data-stu-id="da229-184">If it succeeds, then each *property_subpattern* field or property is read and its value matched against its corresponding pattern.</span></span> <span data-ttu-id="da229-185">完全一致の結果は、これらのいずれかの結果が `false`場合にのみ `false` ます。</span><span class="sxs-lookup"><span data-stu-id="da229-185">The result of the whole match is `false` only if the result of any of these is `false`.</span></span> <span data-ttu-id="da229-186">サブパターンが一致する順序が指定されていないため、実行時に失敗した一致がすべてのサブパターンと一致しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="da229-186">The order in which subpatterns are matched is not specified, and a failed match may not match all subpatterns at runtime.</span></span> <span data-ttu-id="da229-187">一致が成功し、 *property_pattern*の*simple_designation*が*single_variable_designation*の場合は、一致する値が割り当てられている型*T*の変数を定義します。</span><span class="sxs-lookup"><span data-stu-id="da229-187">If the match succeeds and the *simple_designation* of the *property_pattern* is a *single_variable_designation*, it defines a variable of type *T* that is assigned the matched value.</span></span>

> <span data-ttu-id="da229-188">注: プロパティパターンは、匿名型を使用したパターン一致に使用できます。</span><span class="sxs-lookup"><span data-stu-id="da229-188">Note: The property pattern can be used to pattern-match with anonymous types.</span></span>

##### <a name="example"></a><span data-ttu-id="da229-189">例</span><span class="sxs-lookup"><span data-stu-id="da229-189">Example</span></span>

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a><span data-ttu-id="da229-190">Switch 式</span><span class="sxs-lookup"><span data-stu-id="da229-190">Switch Expression</span></span>

<span data-ttu-id="da229-191">式コンテキストの `switch`のようなセマンティクスをサポートするために*switch_expression*が追加されました。</span><span class="sxs-lookup"><span data-stu-id="da229-191">A *switch_expression* is added to support `switch`-like semantics for an expression context.</span></span>

<span data-ttu-id="da229-192">言語C#の構文は、次の構文を使用して強化されています。</span><span class="sxs-lookup"><span data-stu-id="da229-192">The C# language syntax is augmented with the following syntactic productions:</span></span>

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

<span data-ttu-id="da229-193">*Switch_expression*は*expression_statement*として許可されていません。</span><span class="sxs-lookup"><span data-stu-id="da229-193">The *switch_expression* is not permitted as an *expression_statement*.</span></span>

> <span data-ttu-id="da229-194">今後のリビジョンでは、このような緩和を検討しています。</span><span class="sxs-lookup"><span data-stu-id="da229-194">We are looking at relaxing this in a future revision.</span></span>

<span data-ttu-id="da229-195">*Switch_expression*の型は、このような型が存在し、switch 式のすべての arm 内の式を暗黙的にその型に変換できる場合に、 *switch_expression_arm*の `=>` トークンの右側に表示される式の中で[*最適な*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions)型です。</span><span class="sxs-lookup"><span data-stu-id="da229-195">The type of the *switch_expression* is the [*best common type*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) of the expressions appearing to the right of the `=>` tokens of the *switch_expression_arm*s if such a type exists and the expression in every arm of the switch expression can be implicitly converted to that type.</span></span>  <span data-ttu-id="da229-196">また、新しい*switch 式の変換*を追加します。これは、switch 式から、各 arm の式から `T`への暗黙的な変換が存在する `T` すべての型への、定義済みの暗黙的な変換です。</span><span class="sxs-lookup"><span data-stu-id="da229-196">In addition, we add a new *switch expression conversion*, which is a predefined implicit conversion from a switch expression to every type `T` for which there exists an implicit conversion from each arm's expression to `T`.</span></span>

<span data-ttu-id="da229-197">前のパターンとガードが常に一致するため、一部の*switch_expression_arm*のパターンが結果に影響を与えない場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="da229-197">It is an error if some *switch_expression_arm*'s pattern cannot affect the result because some previous pattern and guard will always match.</span></span>

<span data-ttu-id="da229-198">Switch 式の一部の arm では、入力のすべての値が処理される場合、スイッチ式は*完全*であると言います。</span><span class="sxs-lookup"><span data-stu-id="da229-198">A switch expression is said to be *exhaustive* if some arm of the switch expression handles every value of its input.</span></span>  <span data-ttu-id="da229-199">スイッチ式が*完全*でない場合、コンパイラは警告を生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da229-199">The compiler shall produce a warning if a switch expression is not *exhaustive*.</span></span>

<span data-ttu-id="da229-200">実行時には、 *switch_expression*の結果が、 *switch_expression*の左側の式が*switch_expression_arm*のパターンと一致し、 *case_guard*の*switch_expression_arm* (存在する場合) が `true`に評価される最初の*switch_expression_arm*の*式*の値になっています。</span><span class="sxs-lookup"><span data-stu-id="da229-200">At runtime, the result of the *switch_expression* is the value of the *expression* of the first *switch_expression_arm* for which the expression on the left-hand-side of the *switch_expression* matches the *switch_expression_arm*'s pattern, and for which the *case_guard* of the *switch_expression_arm*, if present, evaluates to `true`.</span></span> <span data-ttu-id="da229-201">このような*switch_expression_arm*がない場合、 *switch_expression*は例外 `System.Runtime.CompilerServices.SwitchExpressionException`のインスタンスをスローします。</span><span class="sxs-lookup"><span data-stu-id="da229-201">If there is no such *switch_expression_arm*, the *switch_expression* throws an instance of the exception `System.Runtime.CompilerServices.SwitchExpressionException`.</span></span>

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a><span data-ttu-id="da229-202">タプルリテラルでの切り替え時のオプションのかっこ</span><span class="sxs-lookup"><span data-stu-id="da229-202">Optional parens when switching on a tuple literal</span></span>

<span data-ttu-id="da229-203">*Switch_statement*を使用して組リテラルを切り替えるためには、冗長なものとして表示されるものを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da229-203">In order to switch on a tuple literal using the *switch_statement*, you have to write what appear to be redundant parens</span></span>

```csharp
switch ((a, b))
{
```

<span data-ttu-id="da229-204">許可する</span><span class="sxs-lookup"><span data-stu-id="da229-204">To permit</span></span>

```csharp
switch (a, b)
{
```

<span data-ttu-id="da229-205">switch ステートメントのかっこは、切り替え先の式がタプルリテラルである場合は省略可能です。</span><span class="sxs-lookup"><span data-stu-id="da229-205">the parentheses of the switch statement are optional when the expression being switched on is a tuple literal.</span></span>

### <a name="order-of-evaluation-in-pattern-matching"></a><span data-ttu-id="da229-206">パターンマッチングでの評価の順序</span><span class="sxs-lookup"><span data-stu-id="da229-206">Order of evaluation in pattern-matching</span></span>

<span data-ttu-id="da229-207">パターン照合中に実行される操作の順序をコンパイラで柔軟に指定すると、パターンマッチングの効率を向上させるために使用できる柔軟性を高めることができます。</span><span class="sxs-lookup"><span data-stu-id="da229-207">Giving the compiler flexibility in reordering the operations executed during pattern-matching can permit flexibility that can be used to improve the efficiency of pattern-matching.</span></span> <span data-ttu-id="da229-208">(強制されていない) 要件としては、パターンでアクセスされるプロパティと分解メソッドが "純粋な" (副作用、べき等など) である必要があります。</span><span class="sxs-lookup"><span data-stu-id="da229-208">The (unenforced) requirement would be that properties accessed in a pattern, and the Deconstruct methods, are required to be "pure" (side-effect free, idempotent, etc).</span></span> <span data-ttu-id="da229-209">これは、言語の概念として純度を追加するという意味ではなく、コンパイラの柔軟性を使用して操作の順序を変更できるということではありません。</span><span class="sxs-lookup"><span data-stu-id="da229-209">That doesn't mean that we would add purity as a language concept, only that we would allow the compiler flexibility in reordering operations.</span></span>

<span data-ttu-id="da229-210">**解決方法 2018-04-04 LDM**: 確認済み: コンパイラは、`ITuple`の `Deconstruct`、プロパティへのアクセス、メソッドの呼び出しに対する呼び出しの順序を変更できます。また、返された値が複数の呼び出しと同じであると見なすことができます。</span><span class="sxs-lookup"><span data-stu-id="da229-210">**Resolution 2018-04-04 LDM**: confirmed: the compiler is permitted to reorder calls to `Deconstruct`, property accesses, and invocations of methods in `ITuple`, and may assume that returned values are the same from multiple calls.</span></span> <span data-ttu-id="da229-211">コンパイラは、結果に影響を与えない関数を呼び出すことはできません。コンパイラによって生成された評価の順序を今後変更する前に、細心の注意を払ってください。</span><span class="sxs-lookup"><span data-stu-id="da229-211">The compiler should not invoke functions that cannot affect the result, and we will be very careful before making any changes to the compiler-generated order of evaluation in the future.</span></span>

### <a name="some-possible-optimizations"></a><span data-ttu-id="da229-212">考えられるいくつかの最適化</span><span class="sxs-lookup"><span data-stu-id="da229-212">Some Possible Optimizations</span></span>

<span data-ttu-id="da229-213">パターンマッチングのコンパイルでは、パターンの共通部分を利用できます。</span><span class="sxs-lookup"><span data-stu-id="da229-213">The compilation of pattern matching can take advantage of common parts of patterns.</span></span> <span data-ttu-id="da229-214">たとえば、 *switch_statement*内の2つの連続するパターンの最上位レベルのテストが同じ型である場合、生成されたコードは2番目のパターンの型テストをスキップできます。</span><span class="sxs-lookup"><span data-stu-id="da229-214">For example, if the top-level type test of two successive patterns in a *switch_statement* is the same type, the generated code can skip the type test for the second pattern.</span></span>

<span data-ttu-id="da229-215">いくつかのパターンが整数または文字列の場合、コンパイラは、以前のバージョンの言語では、switch ステートメントに対して生成されるものと同じ種類のコードを生成できます。</span><span class="sxs-lookup"><span data-stu-id="da229-215">When some of the patterns are integers or strings, the compiler can generate the same kind of code it generates for a switch-statement in earlier versions of the language.</span></span>

<span data-ttu-id="da229-216">これらの最適化の詳細については、 [[Scott And Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "一致とコンパイルのヒューリスティックが重要な場合")を参照してください。</span><span class="sxs-lookup"><span data-stu-id="da229-216">For more on these kinds of optimizations, see [[Scott and Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "When Do Match-Compilation Heuristics Matter?").</span></span>
