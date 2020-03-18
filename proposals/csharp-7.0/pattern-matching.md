---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484070"
---
# <a name="pattern-matching-for-c-7"></a><span data-ttu-id="09f05-101">7のC#パターンマッチング</span><span class="sxs-lookup"><span data-stu-id="09f05-101">Pattern Matching for C# 7</span></span>

<span data-ttu-id="09f05-102">パターンマッチング拡張機能C#を使用すると、代数データ型と関数型言語のパターンマッチングの多くの利点が得られますが、基になる言語の感覚とスムーズに統合されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-102">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="09f05-103">基本的な機能は、[レコード型](https://github.com/dotnet/csharplang/blob/master/proposals/records.md)です。これは、データの形によって記述される意味を持つ型です。また、パターンマッチングは、これらのデータ型を大幅に簡潔に分解できる新しい式の形式です。</span><span class="sxs-lookup"><span data-stu-id="09f05-103">The basic features are: [record types](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), which are types whose semantic meaning is described by the shape of the data; and pattern matching, which is a new expression form that enables extremely concise multilevel decomposition of these data types.</span></span> <span data-ttu-id="09f05-104">このアプローチの要素は、プログラミング言語[F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "軽量言語を使用した拡張可能なパターンマッチング")の関連機能と、[スケール a](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "オブジェクトとパターンの一致")によって実現されています。</span><span class="sxs-lookup"><span data-stu-id="09f05-104">Elements of this approach are inspired by related features in the programming languages [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Matching Objects With Patterns").</span></span>

## <a name="is-expression"></a><span data-ttu-id="09f05-105">Is 式</span><span class="sxs-lookup"><span data-stu-id="09f05-105">Is expression</span></span>

<span data-ttu-id="09f05-106">`is` 演算子は、*パターン*に対して式をテストするために拡張されています。</span><span class="sxs-lookup"><span data-stu-id="09f05-106">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="09f05-107">この形式の*relational_expression*は、 C#仕様に含まれる既存のフォームに追加されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-107">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="09f05-108">`is` トークンの左側の*relational_expression*が値を指定していないか、型を持たない場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-108">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="09f05-109">パターンのすべての*識別子*は、`is` 演算子が `true` た後に*確実に割り当てら*れる新しいローカル変数を導入します (つまり、 *true の場合は確実に割り当て*られます)。</span><span class="sxs-lookup"><span data-stu-id="09f05-109">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="09f05-110">メモ: 厳密には、`is-expression` と*constant_pattern*の*型*の間にあいまいさがあります。どちらも、修飾された識別子の有効な解析である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="09f05-110">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="09f05-111">以前のバージョンの言語との互換性のために、型としてバインドしようとしています。失敗した場合にのみ、他のコンテキストの場合と同様に、最初に見つかったもの (定数または型のいずれかである必要があります) に解決されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-111">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="09f05-112">このあいまいさは、`is` 式の右辺にのみ存在します。</span><span class="sxs-lookup"><span data-stu-id="09f05-112">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

## <a name="patterns"></a><span data-ttu-id="09f05-113">パターン</span><span class="sxs-lookup"><span data-stu-id="09f05-113">Patterns</span></span>

<span data-ttu-id="09f05-114">パターンは、`is` 演算子および*switch_statement*内で使用され、受信データの比較対象となるデータの構造を表します。</span><span class="sxs-lookup"><span data-stu-id="09f05-114">Patterns are used in the `is` operator and in a *switch_statement* to express the shape of data against which incoming data is to be compared.</span></span> <span data-ttu-id="09f05-115">パターンは再帰的であるため、データの一部がサブパターンと照合される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="09f05-115">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> <span data-ttu-id="09f05-116">メモ: 厳密には、`is-expression` と*constant_pattern*の*型*の間にあいまいさがあります。どちらも、修飾された識別子の有効な解析である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="09f05-116">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="09f05-117">以前のバージョンの言語との互換性のために、型としてバインドしようとしています。失敗した場合にのみ、他のコンテキストの場合と同様に、最初に見つかったもの (定数または型のいずれかである必要があります) に解決されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-117">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="09f05-118">このあいまいさは、`is` 式の右辺にのみ存在します。</span><span class="sxs-lookup"><span data-stu-id="09f05-118">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="declaration-pattern"></a><span data-ttu-id="09f05-119">宣言パターン</span><span class="sxs-lookup"><span data-stu-id="09f05-119">Declaration pattern</span></span>

<span data-ttu-id="09f05-120">*Declaration_pattern*は、式が特定の型であるかどうかをテストし、テストが成功した場合はその型にキャストします。</span><span class="sxs-lookup"><span data-stu-id="09f05-120">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="09f05-121">*Simple_designation*が識別子の場合は、指定された識別子によって指定された型のローカル変数を導入します。</span><span class="sxs-lookup"><span data-stu-id="09f05-121">If the *simple_designation* is an identifier, it introduces a local variable of the given type named by the given identifier.</span></span> <span data-ttu-id="09f05-122">このローカル変数は、パターン一致操作の結果が true の場合に*確実に割り当てら*れます。</span><span class="sxs-lookup"><span data-stu-id="09f05-122">That local variable is *definitely assigned* when the result of the pattern-matching operation is true.</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="09f05-123">この式のランタイムセマンティックは、パターンの*型*に対して左辺の*relational_expression*オペランドのランタイム型をテストすることです。</span><span class="sxs-lookup"><span data-stu-id="09f05-123">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span> <span data-ttu-id="09f05-124">そのランタイム型 (または一部のサブタイプ) の場合、`is operator` の結果は `true`ます。</span><span class="sxs-lookup"><span data-stu-id="09f05-124">If it is of that runtime type (or some subtype), the result of the `is operator` is `true`.</span></span> <span data-ttu-id="09f05-125">これは、結果が `true`ときに左側のオペランドの値が割り当てられた*識別子*によって指定された新しいローカル変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="09f05-125">It declares a new local variable named by the *identifier* that is assigned the value of the left-hand operand when the result is `true`.</span></span>

<span data-ttu-id="09f05-126">左側の静的な型と指定された型の特定の組み合わせは互換性がないと見なされ、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="09f05-126">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="09f05-127">静的な型 `E` の値は、id 変換、暗黙の参照変換、ボックス化変換、明示的な参照変換、または `E` から `T`へのアンボックス変換が存在する場合に、型 `T` との*パターン互換性*があると言われます。</span><span class="sxs-lookup"><span data-stu-id="09f05-127">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`.</span></span> <span data-ttu-id="09f05-128">`E` 型の式が、照合される型パターンの型と互換性のあるパターンでない場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-128">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

> <span data-ttu-id="09f05-129">注: [7.1 C#で](../csharp-7.1/generics-pattern-match.md)は、入力型または型 `T` がオープン型である場合に、パターンマッチング操作を許可するようにこれを拡張します。</span><span class="sxs-lookup"><span data-stu-id="09f05-129">Note: [In C# 7.1 we extend this](../csharp-7.1/generics-pattern-match.md) to permit a pattern-matching operation if either the input type or the type `T` is an open type.</span></span> <span data-ttu-id="09f05-130">この段落は、次のように置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="09f05-130">This paragraph is replaced by the following:</span></span>
> 
> <span data-ttu-id="09f05-131">左側の静的な型と指定された型の特定の組み合わせは互換性がないと見なされ、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="09f05-131">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="09f05-132">静的な型 `E` の値は、id 変換、暗黙の参照変換、ボックス変換、明示的な参照変換、または `E` から `T`へのアンボックス変換、または **`E` または `T` のいずれかがオープン型**である場合に、型 `T` との*パターン互換性*があると言われます。</span><span class="sxs-lookup"><span data-stu-id="09f05-132">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, **or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="09f05-133">`E` 型の式が、照合される型パターンの型と互換性のあるパターンでない場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-133">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

<span data-ttu-id="09f05-134">宣言パターンは、参照型のランタイム型テストを実行する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="09f05-134">The declaration pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

<span data-ttu-id="09f05-135">少し簡潔に</span><span class="sxs-lookup"><span data-stu-id="09f05-135">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v }
```

<span data-ttu-id="09f05-136">*型*が null 許容値型の場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-136">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="09f05-137">宣言パターンは、null 許容型の値をテストするために使用できます。値が null 以外で、`T2` の型が `T`である場合、または `T`の基本型またはインターフェイスである場合は、型 `Nullable<T>` (またはボックス化された `T`) の値が型 `T2 id` パターンと一致します。</span><span class="sxs-lookup"><span data-stu-id="09f05-137">The declaration pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="09f05-138">たとえば、コード片で</span><span class="sxs-lookup"><span data-stu-id="09f05-138">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

<span data-ttu-id="09f05-139">`if` ステートメントの条件は実行時に `true` され `v` 変数は、ブロック内の `int` 型の値 `3` を保持します。</span><span class="sxs-lookup"><span data-stu-id="09f05-139">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span>

### <a name="constant-pattern"></a><span data-ttu-id="09f05-140">定数パターン</span><span class="sxs-lookup"><span data-stu-id="09f05-140">Constant pattern</span></span>

```antlr
constant_pattern
    : shift_expression
    ;
```

<span data-ttu-id="09f05-141">定数パターンは、定数値に対して式の値をテストします。</span><span class="sxs-lookup"><span data-stu-id="09f05-141">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="09f05-142">定数には、リテラル、宣言された `const` 変数の名前、列挙定数、`typeof` 式など、任意の定数式を指定できます。</span><span class="sxs-lookup"><span data-stu-id="09f05-142">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant, or a `typeof` expression.</span></span>

<span data-ttu-id="09f05-143">*E*と*c*の両方が整数型の場合、`e == c` 式の結果が `true`場合は、パターンが一致したと見なされます。</span><span class="sxs-lookup"><span data-stu-id="09f05-143">If both *e* and *c* are of integral types, the pattern is considered matched if the result of the expression `e == c` is `true`.</span></span>

<span data-ttu-id="09f05-144">それ以外の場合、`object.Equals(e, c)` が `true`を返す場合、パターンは一致と見なされます。</span><span class="sxs-lookup"><span data-stu-id="09f05-144">Otherwise the pattern is considered matching if `object.Equals(e, c)` returns `true`.</span></span> <span data-ttu-id="09f05-145">この場合、 *e*の静的な型が定数の型と互換性のある*パターン*でない場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-145">In this case it is a compile-time error if the static type of *e* is not *pattern compatible* with the type of the constant.</span></span>

### <a name="var-pattern"></a><span data-ttu-id="09f05-146">Var パターン</span><span class="sxs-lookup"><span data-stu-id="09f05-146">Var pattern</span></span>

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

<span data-ttu-id="09f05-147">式*e*は常に*var_pattern*と一致します。</span><span class="sxs-lookup"><span data-stu-id="09f05-147">An expression *e* matches a *var_pattern* always.</span></span> <span data-ttu-id="09f05-148">言い換えると、 *var パターン*との一致は常に成功します。</span><span class="sxs-lookup"><span data-stu-id="09f05-148">In other words, a match to a *var pattern* always succeeds.</span></span> <span data-ttu-id="09f05-149">*Simple_designation*が識別子の場合は、実行時に*e*の値が新しく導入されたローカル変数にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="09f05-149">If the *simple_designation* is an identifier, then at runtime the value of *e* is bound to a newly introduced local variable.</span></span> <span data-ttu-id="09f05-150">ローカル変数の型は、 *e*の静的な型です。</span><span class="sxs-lookup"><span data-stu-id="09f05-150">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="09f05-151">名前 `var` 型にバインドされている場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-151">It is an error if the name `var` binds to a type.</span></span>

## <a name="switch-statement"></a><span data-ttu-id="09f05-152">Switch ステートメント</span><span class="sxs-lookup"><span data-stu-id="09f05-152">Switch statement</span></span>

<span data-ttu-id="09f05-153">`switch` ステートメントは、 *switch 式*と一致するパターンが関連付けられた最初のブロックを実行するために選択するように拡張されています。</span><span class="sxs-lookup"><span data-stu-id="09f05-153">The `switch` statement is extended to select for execution the first block having an associated pattern that matches the *switch expression*.</span></span>

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

<span data-ttu-id="09f05-154">パターンが一致する順序は定義されていません。</span><span class="sxs-lookup"><span data-stu-id="09f05-154">The order in which patterns are matched is not defined.</span></span> <span data-ttu-id="09f05-155">コンパイラは、パターンを順序どおりに一致させ、既に一致したパターンの結果を再利用して、他のパターンとの照合結果を計算することができます。</span><span class="sxs-lookup"><span data-stu-id="09f05-155">A compiler is permitted to match patterns out of order, and to reuse the results of already matched patterns to compute the result of matching of other patterns.</span></span>

<span data-ttu-id="09f05-156">*ケースガード*が存在する場合、その式は `bool`型になります。</span><span class="sxs-lookup"><span data-stu-id="09f05-156">If a *case-guard* is present, its expression is of type `bool`.</span></span> <span data-ttu-id="09f05-157">これは、満たされた場合に満たす必要がある追加の条件として評価されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-157">It is evaluated as an additional condition that must be satisfied for the case to be considered satisfied.</span></span>

<span data-ttu-id="09f05-158">*Switch_label*が実行時に影響を与えない場合、これはエラーになります。これは、そのパターンが前のケースで包括されているためです。</span><span class="sxs-lookup"><span data-stu-id="09f05-158">It is an error if a *switch_label* can have no effect at runtime because its pattern is subsumed by previous cases.</span></span> <span data-ttu-id="09f05-159">[TODO: この判断に至るためにコンパイラが使用する必要がある手法について、より正確に指定する必要があります。]</span><span class="sxs-lookup"><span data-stu-id="09f05-159">[TODO: We should be more precise about the techniques the compiler is required to use to reach this judgment.]</span></span>

<span data-ttu-id="09f05-160">*Switch_label*で宣言されたパターン変数は、その case ブロックに1つの*switch_label*が含まれている場合にのみ、その case ブロックで確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="09f05-160">A pattern variable declared in a *switch_label* is definitely assigned in its case block if and only if that case block contains precisely one *switch_label*.</span></span>

<span data-ttu-id="09f05-161">[TODO:*スイッチブロック*に到達できる場合は、を指定する必要があります。]</span><span class="sxs-lookup"><span data-stu-id="09f05-161">[TODO: We should specify when a *switch block* is reachable.]</span></span>

### <a name="scope-of-pattern-variables"></a><span data-ttu-id="09f05-162">パターン変数のスコープ</span><span class="sxs-lookup"><span data-stu-id="09f05-162">Scope of pattern variables</span></span>

<span data-ttu-id="09f05-163">パターンで宣言された変数のスコープは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="09f05-163">The scope of a variable declared in a pattern is as follows:</span></span>

- <span data-ttu-id="09f05-164">パターンが case ラベルの場合、変数のスコープは*case ブロック*になります。</span><span class="sxs-lookup"><span data-stu-id="09f05-164">If the pattern is a case label, then the scope of the variable is the *case block*.</span></span>

<span data-ttu-id="09f05-165">それ以外の場合、変数は*is_pattern*式で宣言され、そのスコープは、次のように*is_pattern*式を含む式をすぐに囲むコンストラクトに基づいています。</span><span class="sxs-lookup"><span data-stu-id="09f05-165">Otherwise the variable is declared in an *is_pattern* expression, and its scope is based on the construct immediately enclosing the expression containing the *is_pattern* expression as follows:</span></span>

- <span data-ttu-id="09f05-166">式が式形式のラムダ内にある場合、そのスコープはラムダの本体です。</span><span class="sxs-lookup"><span data-stu-id="09f05-166">If the expression is in an expression-bodied lambda, its scope is the body of the lambda.</span></span>
- <span data-ttu-id="09f05-167">式が式形式のメソッドまたはプロパティに含まれている場合、そのスコープはメソッドまたはプロパティの本体です。</span><span class="sxs-lookup"><span data-stu-id="09f05-167">If the expression is in an expression-bodied method or property, its scope is the body of the method or property.</span></span>
- <span data-ttu-id="09f05-168">式が `catch` 句の `when` 句に含まれている場合、そのスコープは `catch` 句になります。</span><span class="sxs-lookup"><span data-stu-id="09f05-168">If the expression is in a `when` clause of a `catch` clause, its scope is that `catch` clause.</span></span>
- <span data-ttu-id="09f05-169">式が*iteration_statement*内にある場合、そのスコープはそのステートメントにすぎません。</span><span class="sxs-lookup"><span data-stu-id="09f05-169">If the expression is in an *iteration_statement*, its scope is just that statement.</span></span>
- <span data-ttu-id="09f05-170">それ以外の場合、式が他のステートメント形式に含まれていると、そのスコープがステートメントを含むスコープになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-170">Otherwise if the expression is in some other statement form, its scope is the scope containing the statement.</span></span>

<span data-ttu-id="09f05-171">スコープを決定するために、 *embedded_statement*は独自のスコープ内にあると見なされます。</span><span class="sxs-lookup"><span data-stu-id="09f05-171">For the purpose of determining the scope, an *embedded_statement* is considered to be in its own scope.</span></span> <span data-ttu-id="09f05-172">たとえば、 *if_statement*の文法は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-172">For example, the grammar for an *if_statement* is</span></span>

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="09f05-173">したがって、 *if_statement*の制御されたステートメントがパターン変数を宣言している場合、そのスコープはその*embedded_statement*に制限されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-173">So if the controlled statement of an *if_statement* declares a pattern variable, its scope is restricted to that *embedded_statement*:</span></span>

```csharp
if (x) M(y is var z);
```

<span data-ttu-id="09f05-174">この場合、`z` のスコープは、埋め込みステートメント `M(y is var z);`です。</span><span class="sxs-lookup"><span data-stu-id="09f05-174">In this case the scope of `z` is the embedded statement `M(y is var z);`.</span></span>

<span data-ttu-id="09f05-175">その他の理由 (たとえば、パラメーターの既定値や属性など) でエラーが発生した場合は、これらのコンテキストには定数式が必要であるため、どちらもエラーになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-175">Other cases are errors for other reasons (e.g. in a parameter's default value or an attribute, both of which are an error because those contexts require a constant expression).</span></span>

> <span data-ttu-id="09f05-176">[7.3 C#では、](../csharp-7.3/expression-variables-in-initializers.md) pattern 変数が宣言される次のコンテキストを追加しました。</span><span class="sxs-lookup"><span data-stu-id="09f05-176">[In C# 7.3 we added the following contexts](../csharp-7.3/expression-variables-in-initializers.md) in which a pattern variable may be declared:</span></span>
> - <span data-ttu-id="09f05-177">式が*コンストラクター初期化子*内にある場合、そのスコープは*コンストラクター初期化子*とコンストラクターの本体になります。</span><span class="sxs-lookup"><span data-stu-id="09f05-177">If the expression is in a *constructor initializer*, its scope is the *constructor initializer* and the constructor's body.</span></span>
> - <span data-ttu-id="09f05-178">式がフィールド初期化子に含まれている場合、そのスコープは、その式が表示される*equals_value_clause*になります。</span><span class="sxs-lookup"><span data-stu-id="09f05-178">If the expression is in a field initializer, its scope is the *equals_value_clause* in which it appears.</span></span>
> - <span data-ttu-id="09f05-179">ラムダの本体に変換するように指定されているクエリ句に式が含まれている場合、その式のスコープはその式にすぎません。</span><span class="sxs-lookup"><span data-stu-id="09f05-179">If the expression is in a query clause that is specified to be translated into the body of a lambda, its scope is just that expression.</span></span>

## <a name="changes-to-syntactic-disambiguation"></a><span data-ttu-id="09f05-180">構文のあいまいさを解消するための変更</span><span class="sxs-lookup"><span data-stu-id="09f05-180">Changes to syntactic disambiguation</span></span>

<span data-ttu-id="09f05-181">ジェネリックにはC#文法があいまいであり、言語仕様によってあいまいさを解決する方法が示されています。</span><span class="sxs-lookup"><span data-stu-id="09f05-181">There are situations involving generics where the C# grammar is ambiguous, and the language spec says how to resolve those ambiguities:</span></span>

> #### <a name="7652-grammar-ambiguities"></a><span data-ttu-id="09f05-182">7.6.5.2 文法のあいまい性</span><span class="sxs-lookup"><span data-stu-id="09f05-182">7.6.5.2 Grammar ambiguities</span></span>
> <span data-ttu-id="09f05-183">*単純名*(7.6.3) と*メンバーアクセス*(7.6.5 を参照) の生産によって、式の文法があいまいになる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="09f05-183">The productions for *simple-name* (§7.6.3) and *member-access* (§7.6.5) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="09f05-184">たとえば、ステートメントは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-184">For example, the statement:</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="09f05-185">は、`G < A` と `B > (7)`の2つの引数を持つ `F` の呼び出しとして解釈される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="09f05-185">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="09f05-186">または、1つの引数を持つ `F` の呼び出しとして解釈することもできます。これは、2つの型引数と1つの標準引数を持つジェネリックメソッド `G` の呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="09f05-186">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

> <span data-ttu-id="09f05-187">トークンのシーケンスを*単純な名前*(7.6.3)、*メンバーアクセス*(§ 7.6.5)、または*ポインターメンバーアクセス*(18.5.2 を参照) として解析できる場合は、終わりの `>` トークンの直後にあるトークンが調べられます (「4.4.1)」と*入力*します。</span><span class="sxs-lookup"><span data-stu-id="09f05-187">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="09f05-188">のいずれかである場合</span><span class="sxs-lookup"><span data-stu-id="09f05-188">If it is one of</span></span>
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> <span data-ttu-id="09f05-189">その後、*型引数リスト*は、*単純名*、*メンバーアクセス*、または*ポインターメンバーアクセス*の一部として保持され、トークンのシーケンスのその他の可能な解析はすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-189">then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="09f05-190">それ以外の場合、トークンのシーケンスを解析できない場合でも、*型引数リスト*は、*単純名*、*メンバーアクセス*、または >*ポインターメンバーアクセス*の一部とは見なされません。</span><span class="sxs-lookup"><span data-stu-id="09f05-190">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or > *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="09f05-191">*名前空間または型名*で*型引数リスト*を解析する場合、これらの規則は適用されないことに注意してください (3.8 を参照)。</span><span class="sxs-lookup"><span data-stu-id="09f05-191">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span> <span data-ttu-id="09f05-192">次のステートメント、</span><span class="sxs-lookup"><span data-stu-id="09f05-192">The statement</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="09f05-193">は、この規則に従って、1つの引数を持つ `F` の呼び出しとして解釈されます。これは、2つの型引数と1つの標準引数を持つジェネリックメソッド `G` の呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="09f05-193">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="09f05-194">ステートメント</span><span class="sxs-lookup"><span data-stu-id="09f05-194">The statements</span></span>
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> <span data-ttu-id="09f05-195">は、2つの引数を持つ `F` の呼び出しとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-195">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="09f05-196">次のステートメント、</span><span class="sxs-lookup"><span data-stu-id="09f05-196">The statement</span></span>
> ```csharp
> x = F < A > +y;
> ```
> <span data-ttu-id="09f05-197">は、ステートメントが `x = (F < A) > (+y)`記述されているかのように、小なり演算子、大なり演算子、および単項プラス演算子として解釈されます。これは、*型引数リスト*の後に二項プラス演算子が続く*単純な名前*ではありません。</span><span class="sxs-lookup"><span data-stu-id="09f05-197">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple-name* with a *type-argument-list* followed by a binary plus operator.</span></span> <span data-ttu-id="09f05-198">ステートメント内</span><span class="sxs-lookup"><span data-stu-id="09f05-198">In the statement</span></span>
> ```csharp
> x = y is C<T> + z;
> ```
> <span data-ttu-id="09f05-199">`C<T>` トークンは、*型引数リスト*を持つ*名前空間または型名*として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-199">the tokens `C<T>` are interpreted as a *namespace-or-type-name* with a *type-argument-list*.</span></span>

<span data-ttu-id="09f05-200">7では、このようなあいまいさC#を解消するために、言語の複雑さを処理するのに十分ではないいくつかの変更が導入されています。</span><span class="sxs-lookup"><span data-stu-id="09f05-200">There are a number of changes being introduced in C# 7 that make these disambiguation rules no longer sufficient to handle the complexity of the language.</span></span>

### <a name="out-variable-declarations"></a><span data-ttu-id="09f05-201">out 変数宣言</span><span class="sxs-lookup"><span data-stu-id="09f05-201">Out variable declarations</span></span>

<span data-ttu-id="09f05-202">Out 引数で変数を宣言できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="09f05-202">It is now possible to declare a variable in an out argument:</span></span>

```csharp
M(out Type name);
```

<span data-ttu-id="09f05-203">ただし、型はジェネリックである場合があります。</span><span class="sxs-lookup"><span data-stu-id="09f05-203">However, the type may be generic:</span></span> 

```csharp
M(out A<B> name);
```

<span data-ttu-id="09f05-204">引数の言語文法では*式*が使用されるため、このコンテキストにはあいまいさの排除規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-204">Since the language grammar for the argument uses *expression*, this context is subject to the disambiguation rule.</span></span> <span data-ttu-id="09f05-205">この場合、終了 `>` の後に識別子が続きます。この*識別子*は、*型引数リスト*として扱うことを許可するトークンの1つではありません。</span><span class="sxs-lookup"><span data-stu-id="09f05-205">In this case the closing `>` is followed by an *identifier*, which is not one of the tokens that permits it to be treated as a *type-argument-list*.</span></span> <span data-ttu-id="09f05-206">そのため、あいまいさを解消する**トークンのセットに*識別子*を追加すること\*type-argument-list**\* を提案します。</span><span class="sxs-lookup"><span data-stu-id="09f05-206">I therefore propose to **add *identifier* to the set of tokens that triggers the disambiguation to a *type-argument-list*.**</span></span>

### <a name="tuples-and-deconstruction-declarations"></a><span data-ttu-id="09f05-207">タプルと分解宣言</span><span class="sxs-lookup"><span data-stu-id="09f05-207">Tuples and deconstruction declarations</span></span>

<span data-ttu-id="09f05-208">組リテラルは、まったく同じ問題に実行されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-208">A tuple literal runs into exactly the same issue.</span></span> <span data-ttu-id="09f05-209">タプル式を考えます。</span><span class="sxs-lookup"><span data-stu-id="09f05-209">Consider the tuple expression</span></span>

```csharp
(A < B, C > D, E < F, G > H)
```

<span data-ttu-id="09f05-210">前C#の6つの規則で引数リストを解析する場合、これは、最初の要素として `A < B` から始まる4つの要素を持つタプルとして解析されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-210">Under the old C# 6 rules for parsing an argument list, this would parse as a tuple with four elements, starting with `A < B` as the first.</span></span> <span data-ttu-id="09f05-211">ただし、分解の左側にこれが表示される場合は、前述のように、*識別子*トークンによってトリガーされるあいまいさを解消する必要があります。</span><span class="sxs-lookup"><span data-stu-id="09f05-211">However, when this appears on the left of a deconstruction, we want the disambiguation triggered by the *identifier* token as described above:</span></span>

```csharp
(A<B,C> D, E<F,G> H) = e;
```

<span data-ttu-id="09f05-212">これは、2つの変数を宣言する分解宣言です。最初の変数は `A<B,C>` 型で、名前付き `D`です。</span><span class="sxs-lookup"><span data-stu-id="09f05-212">This is a deconstruction declaration which declares two variables, the first of which is of type `A<B,C>` and named `D`.</span></span> <span data-ttu-id="09f05-213">言い換えると、組リテラルには2つの式が含まれ、それぞれが宣言式です。</span><span class="sxs-lookup"><span data-stu-id="09f05-213">In other words, the tuple literal contains two expressions, each of which is a declaration expression.</span></span>

<span data-ttu-id="09f05-214">仕様とコンパイラを簡単にするために、このタプルリテラルを (代入式の左側に表示するかどうかにかかわらず) 任意の場所に2要素のタプルとして解析することを提案します。</span><span class="sxs-lookup"><span data-stu-id="09f05-214">For simplicity of the specification and compiler, I propose that this tuple literal be parsed as a two-element tuple wherever it appears (whether or not it appears on the left-hand-side of an assignment).</span></span> <span data-ttu-id="09f05-215">これは、前のセクションで説明した非不明瞭の自然な結果になります。</span><span class="sxs-lookup"><span data-stu-id="09f05-215">That would be a natural result of the disambiguation described in the previous section.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="09f05-216">パターンマッチング</span><span class="sxs-lookup"><span data-stu-id="09f05-216">Pattern-matching</span></span>

<span data-ttu-id="09f05-217">パターンマッチングでは、式型のあいまいさが発生する新しいコンテキストが導入されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-217">Pattern matching introduces a new context where the expression-type ambiguity arises.</span></span> <span data-ttu-id="09f05-218">以前は、`is` 演算子の右辺は型でした。</span><span class="sxs-lookup"><span data-stu-id="09f05-218">Previously the right-hand-side of an `is` operator was a type.</span></span> <span data-ttu-id="09f05-219">これで、型または式にすることができます。型である場合は、識別子の後に指定できます。</span><span class="sxs-lookup"><span data-stu-id="09f05-219">Now it can be a type or expression, and if it is a type it may be followed by an identifier.</span></span> <span data-ttu-id="09f05-220">これにより、技術的には既存のコードの意味を変更できます。</span><span class="sxs-lookup"><span data-stu-id="09f05-220">This can, technically, change the meaning of existing code:</span></span>

```csharp
var x = e is T < A > B;
```

<span data-ttu-id="09f05-221">これは、次のように C# 6 規則で解析できます。</span><span class="sxs-lookup"><span data-stu-id="09f05-221">This could be parsed under C#6 rules as</span></span>

```csharp
var x = ((e is T) < A) > B;
```

<span data-ttu-id="09f05-222">ただし、C# 7 の規則 (上記のあいまいさを排除したもの) は、として解析されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-222">but under under C#7 rules (with the disambiguation proposed above) would be parsed as</span></span>

```csharp
var x = e is T<A> B;
```

<span data-ttu-id="09f05-223">`T<A>`型の変数 `B` を宣言します。</span><span class="sxs-lookup"><span data-stu-id="09f05-223">which declares a variable `B` of type `T<A>`.</span></span> <span data-ttu-id="09f05-224">さいわい、ネイティブコンパイラと Roslyn コンパイラには、C# 6 コードに構文エラーが発生するというバグがあります。</span><span class="sxs-lookup"><span data-stu-id="09f05-224">Fortunately, the native and Roslyn compilers have a bug whereby they give a syntax error on the C#6 code.</span></span> <span data-ttu-id="09f05-225">そのため、このような重大な変更は考慮されません。</span><span class="sxs-lookup"><span data-stu-id="09f05-225">Therefore this particular breaking change is not a concern.</span></span>

<span data-ttu-id="09f05-226">パターンマッチングでは、型の選択に向けたあいまいさの解決を推進する必要がある追加のトークンが導入されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-226">Pattern-matching introduces additional tokens that should drive the ambiguity resolution toward selecting a type.</span></span> <span data-ttu-id="09f05-227">次の例では、既存の有効な C# 6 コードは、その他のあいまいさを排除する規則なしに分割されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-227">The following examples of existing valid C#6 code would be broken without additional disambiguation rules:</span></span>

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a><span data-ttu-id="09f05-228">排除規則に対する変更の提案</span><span class="sxs-lookup"><span data-stu-id="09f05-228">Proposed change to the disambiguation rule</span></span>

<span data-ttu-id="09f05-229">明確化トークンの一覧を変更するように仕様を修正することを提案します。</span><span class="sxs-lookup"><span data-stu-id="09f05-229">I propose to revise the specification to change the list of disambiguating tokens from</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

<span data-ttu-id="09f05-230">から</span><span class="sxs-lookup"><span data-stu-id="09f05-230">to</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

<span data-ttu-id="09f05-231">また、特定のコンテキストでは、*識別子*を明確化トークンとして扱います。</span><span class="sxs-lookup"><span data-stu-id="09f05-231">And, in certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="09f05-232">これらのコンテキストは、明確、`case`、`out`のいずれかの `is`キーワードが直後にあるか、またはタプルリテラルの最初の要素を解析するときに発生します (この場合、トークンの前には `(` または `:`、識別子の後に `,`)、またはタプルリテラルの後続の要素が含まれます。</span><span class="sxs-lookup"><span data-stu-id="09f05-232">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case`, or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>

### <a name="modified-disambiguation-rule"></a><span data-ttu-id="09f05-233">変更されたあいまいさの排除ルール</span><span class="sxs-lookup"><span data-stu-id="09f05-233">Modified disambiguation rule</span></span>

<span data-ttu-id="09f05-234">修正されていないあいまいさの規則は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="09f05-234">The revised disambiguation rule would be something like this</span></span>

> <span data-ttu-id="09f05-235">トークンのシーケンスを*単純な名前*(7.6.3 を参照)、*メンバーアクセス*(§ 7.6.5)、または*ポインターメンバーアクセス*(参照 18.5.2) で終わる*場合 (4.4.1*を参照)、終了 `>` トークンの直後にあるトークンが検査され、次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-235">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined, to see if it is</span></span>
> - <span data-ttu-id="09f05-236">`(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`のいずれかです。もしくは</span><span class="sxs-lookup"><span data-stu-id="09f05-236">One of `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or</span></span>
> - <span data-ttu-id="09f05-237">関係演算子の1つ `<  >  <=  >=  is as`;もしくは</span><span class="sxs-lookup"><span data-stu-id="09f05-237">One of the relational operators `<  >  <=  >=  is as`; or</span></span>
> - <span data-ttu-id="09f05-238">クエリ式の内部に表示されるコンテキストクエリキーワードもしくは</span><span class="sxs-lookup"><span data-stu-id="09f05-238">A contextual query keyword appearing inside a query expression; or</span></span>
> - <span data-ttu-id="09f05-239">特定のコンテキストでは、*識別子*を明確化トークンとして扱います。</span><span class="sxs-lookup"><span data-stu-id="09f05-239">In certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="09f05-240">これらのコンテキストは、明確、`case` または `out`のいずれかの `is`キーワードが直後にあるか、またはタプルリテラルの最初の要素を解析するときに発生します (この場合、トークンの前に `(` または `:`、識別子の後に `,`) またはタプルリテラルの後続の要素が含まれます。</span><span class="sxs-lookup"><span data-stu-id="09f05-240">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case` or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>
> 
> <span data-ttu-id="09f05-241">次のトークンがこのリストまたはこのようなコンテキストの識別子の中にある場合、*型引数リスト*は*単純名*、*メンバーアクセス*、または*ポインターメンバーアクセス*の一部として保持され、その他のトークンシーケンスの解析はすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-241">If the following token is among this list, or an identifier in such a context, then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or  *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span>  <span data-ttu-id="09f05-242">それ以外の場合、トークンのシーケンスを解析できない場合でも、*型引数リスト*は、*単純名*、*メンバーアクセス*、または*ポインターメンバーアクセス*の一部とは見なされません。</span><span class="sxs-lookup"><span data-stu-id="09f05-242">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="09f05-243">*名前空間または型名*で*型引数リスト*を解析する場合、これらの規則は適用されないことに注意してください (3.8 を参照)。</span><span class="sxs-lookup"><span data-stu-id="09f05-243">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span>

### <a name="breaking-changes-due-to-this-proposal"></a><span data-ttu-id="09f05-244">この提案による重大な変更</span><span class="sxs-lookup"><span data-stu-id="09f05-244">Breaking changes due to this proposal</span></span>

<span data-ttu-id="09f05-245">この修正候補の規則により、互換性に影響する変更は認識されません。</span><span class="sxs-lookup"><span data-stu-id="09f05-245">No breaking changes are known due to this proposed disambiguation rule.</span></span>

### <a name="interesting-examples"></a><span data-ttu-id="09f05-246">興味深い例</span><span class="sxs-lookup"><span data-stu-id="09f05-246">Interesting examples</span></span>

<span data-ttu-id="09f05-247">これらの非不明瞭な規則の興味深い結果を次に示します。</span><span class="sxs-lookup"><span data-stu-id="09f05-247">Here are some interesting results of these disambiguation rules:</span></span>

<span data-ttu-id="09f05-248">`(A < B, C > D)` 式は2つの要素を持つ組で、それぞれが比較されます。</span><span class="sxs-lookup"><span data-stu-id="09f05-248">The expression `(A < B, C > D)` is a tuple with two elements, each a comparison.</span></span>

<span data-ttu-id="09f05-249">`(A<B,C> D, E)` 式は2つの要素を持つタプルで、最初の要素は宣言式です。</span><span class="sxs-lookup"><span data-stu-id="09f05-249">The expression `(A<B,C> D, E)` is a tuple with two elements, the first of which is a declaration expression.</span></span>

<span data-ttu-id="09f05-250">呼び出し `M(A < B, C > D, E)` には3つの引数があります。</span><span class="sxs-lookup"><span data-stu-id="09f05-250">The invocation `M(A < B, C > D, E)` has three arguments.</span></span>

<span data-ttu-id="09f05-251">呼び出し `M(out A<B,C> D, E)` には2つの引数があり、最初の引数は `out` 宣言です。</span><span class="sxs-lookup"><span data-stu-id="09f05-251">The invocation `M(out A<B,C> D, E)` has two arguments, the first of which is an `out` declaration.</span></span>

<span data-ttu-id="09f05-252">式 `e is A<B> C` は宣言式を使用します。</span><span class="sxs-lookup"><span data-stu-id="09f05-252">The expression `e is A<B> C` uses a declaration expression.</span></span>

<span data-ttu-id="09f05-253">Case ラベル `case A<B> C:` は宣言式を使用します。</span><span class="sxs-lookup"><span data-stu-id="09f05-253">The case label `case A<B> C:` uses a declaration expression.</span></span>

## <a name="some-examples-of-pattern-matching"></a><span data-ttu-id="09f05-254">パターンマッチングの例</span><span class="sxs-lookup"><span data-stu-id="09f05-254">Some examples of pattern matching</span></span>

### <a name="is-as"></a><span data-ttu-id="09f05-255">は</span><span class="sxs-lookup"><span data-stu-id="09f05-255">Is-As</span></span>

<span data-ttu-id="09f05-256">表現を置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="09f05-256">We can replace the idiom</span></span>

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

<span data-ttu-id="09f05-257">少し簡潔で、直接</span><span class="sxs-lookup"><span data-stu-id="09f05-257">With the slightly more concise and direct</span></span>

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a><span data-ttu-id="09f05-258">テスト (null 許容)</span><span class="sxs-lookup"><span data-stu-id="09f05-258">Testing nullable</span></span>

<span data-ttu-id="09f05-259">表現を置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="09f05-259">We can replace the idiom</span></span>

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

<span data-ttu-id="09f05-260">少し簡潔で、直接</span><span class="sxs-lookup"><span data-stu-id="09f05-260">With the slightly more concise and direct</span></span>

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a><span data-ttu-id="09f05-261">算術単純化</span><span class="sxs-lookup"><span data-stu-id="09f05-261">Arithmetic simplification</span></span>

<span data-ttu-id="09f05-262">(別の提案に従って) 式を表す一連の再帰型を定義するとします。</span><span class="sxs-lookup"><span data-stu-id="09f05-262">Suppose we define a set of recursive types to represent expressions (per a separate proposal):</span></span>

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

<span data-ttu-id="09f05-263">これで、式の (縮小されていない) 派生を計算する関数を定義できます。</span><span class="sxs-lookup"><span data-stu-id="09f05-263">Now we can define a function to compute the (unreduced) derivative of an expression:</span></span>

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

<span data-ttu-id="09f05-264">位置指定パターンを示す式 simplifier を次に示します。</span><span class="sxs-lookup"><span data-stu-id="09f05-264">An expression simplifier demonstrates positional patterns:</span></span>

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
