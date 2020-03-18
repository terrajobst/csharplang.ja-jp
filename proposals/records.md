---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2019
ms.locfileid: "79483878"
---
# <a name="records"></a><span data-ttu-id="9eb21-101">records</span><span class="sxs-lookup"><span data-stu-id="9eb21-101">records</span></span>

* <span data-ttu-id="9eb21-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="9eb21-102">[x] Proposed</span></span>
* <span data-ttu-id="9eb21-103">[] プロトタイプ:[完了](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="9eb21-103">[ ] Prototype: [Complete](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="9eb21-104">[] の実装:[実行](https://github.com/dotnet/roslyn/BRANCH_NAME)中</span><span class="sxs-lookup"><span data-stu-id="9eb21-104">[ ] Implementation: [In Progress](https://github.com/dotnet/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="9eb21-105">[] 仕様: ドラフト仕様を囲みます</span><span class="sxs-lookup"><span data-stu-id="9eb21-105">[ ] Specification: Draft specification enclosed</span></span>

## <a name="summary"></a><span data-ttu-id="9eb21-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="9eb21-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="9eb21-107">レコードは、クラス型および構造体型C#の新しい単純な宣言形式で、多数の単純な機能の利点を組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-107">Records are a new, simplified declaration form for C# class and struct types that combine the benefits of a number of simpler features.</span></span> <span data-ttu-id="9eb21-108">ここでは、新しい機能 (呼び出し元のパラメーターと *-expression*s) について説明し、レコード宣言の構文とセマンティクスを指定して、いくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-108">We describe the new features (caller-receiver parameters and *with-expression*s), give the syntax and semantics for record declarations, and then provide some examples.</span></span>


## <a name="motivation"></a><span data-ttu-id="9eb21-109">目的</span><span class="sxs-lookup"><span data-stu-id="9eb21-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="9eb21-110">のC#型宣言の多くは、型指定されたデータの集約コレクションよりもわずかです。</span><span class="sxs-lookup"><span data-stu-id="9eb21-110">A significant number of type declarations in C# are little more than aggregate collections of typed data.</span></span> <span data-ttu-id="9eb21-111">残念ながら、このような型を宣言するには、大量の定型コードが必要です。</span><span class="sxs-lookup"><span data-stu-id="9eb21-111">Unfortunately, declaring such types requires a great deal of boilerplate code.</span></span> <span data-ttu-id="9eb21-112">*レコード*は、集計のメンバーと追加のコード、または通常の定型句からの偏差 (存在する場合) を記述することによって、データ型を宣言するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-112">*Records* provide a mechanism for declaring a datatype by describing the members of the aggregate along with additional code or deviations from the usual boilerplate, if any.</span></span>

<span data-ttu-id="9eb21-113">以下の[例](#examples)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9eb21-113">See [Examples](#examples), below.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="9eb21-114">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="9eb21-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a><span data-ttu-id="9eb21-115">呼び出し元-受信側パラメーター</span><span class="sxs-lookup"><span data-stu-id="9eb21-115">caller-receiver parameters</span></span>

<span data-ttu-id="9eb21-116">現在、メソッドパラメーターの*既定の引数*はである必要があります</span><span class="sxs-lookup"><span data-stu-id="9eb21-116">Currently a method parameter's *default-argument* must be</span></span> 
- <span data-ttu-id="9eb21-117">*定数式*です。もしくは</span><span class="sxs-lookup"><span data-stu-id="9eb21-117">a *constant-expression*; or</span></span>
- <span data-ttu-id="9eb21-118">`S` が値型の `new S()` 形式の式。もしくは</span><span class="sxs-lookup"><span data-stu-id="9eb21-118">an expression of the form `new S()` where `S` is a value type; or</span></span>
- <span data-ttu-id="9eb21-119">`S` が値型である `default(S)` フォームの式</span><span class="sxs-lookup"><span data-stu-id="9eb21-119">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="9eb21-120">これを拡張して次のものを追加します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-120">We extend this to add the following</span></span>
- <span data-ttu-id="9eb21-121">フォームの式 `this.Identifier`</span><span class="sxs-lookup"><span data-stu-id="9eb21-121">an expression of the form `this.Identifier`</span></span>

<span data-ttu-id="9eb21-122">この新しい形式は、*呼び出し元と受信側の既定の引数*と呼ばれ、次のすべてが満たされている場合にのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-122">This new form is called a *caller-receiver default-argument*, and is allowed only if all of the following are satisfied</span></span>
- <span data-ttu-id="9eb21-123">このメソッドが表示されるメソッドは、インスタンスメソッドです。そして</span><span class="sxs-lookup"><span data-stu-id="9eb21-123">The method in which it appears is an instance method; and</span></span>
- <span data-ttu-id="9eb21-124">式 `this.Identifier`、それを囲む型のインスタンスメンバーにバインドされます。これは、フィールドまたはプロパティである必要があります。そして</span><span class="sxs-lookup"><span data-stu-id="9eb21-124">The expression `this.Identifier` binds to an instance member of the enclosing type, which must be either a field or a property; and</span></span>
- <span data-ttu-id="9eb21-125">バインド先のメンバー (およびプロパティである場合は `get` アクセサー) は、少なくともメソッドと同じようにアクセスできます。そして</span><span class="sxs-lookup"><span data-stu-id="9eb21-125">The member to which it binds (and the `get` accessor, if it is a property) is at least as accessible as the method; and</span></span>
- <span data-ttu-id="9eb21-126">`this.Identifier` の型は、id または null 許容型からパラメーターの型への変換によって暗黙的に変換できます (これは*既定の引数*に対する既存の制約です)。</span><span class="sxs-lookup"><span data-stu-id="9eb21-126">The type of `this.Identifier` is implicitly convertible by an identity or nullable conversion to the type of the parameter (this is an existing constraint on *default-argument*).</span></span>

<span data-ttu-id="9eb21-127">*呼び出し元の既定の引数*を使用して、対応する省略可能なパラメーターの関数メンバーを呼び出すことで引数を省略した場合、受信側のメンバーの値は暗黙的に渡されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-127">When an argument is omitted from an invocation of a function member for a corresponding optional parameter with a *caller-receiver default-argument*, the value of the receiver's member is implicitly passed.</span></span> 

> <span data-ttu-id="9eb21-128">**設計メモ**: 呼び出し元のパラメーターの主な理由は、 *with 式*をサポートするためです。</span><span class="sxs-lookup"><span data-stu-id="9eb21-128">**Design Notes**: the main reason for the caller-receiver parameter is to support the *with-expression*.</span></span> <span data-ttu-id="9eb21-129">これは、次のようにメソッドを宣言できるということです。</span><span class="sxs-lookup"><span data-stu-id="9eb21-129">The idea is that you can declare a method like this</span></span>
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> <span data-ttu-id="9eb21-130">次のように使用します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-130">and then use it like this</span></span>
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> <span data-ttu-id="9eb21-131">既存の `Point` と同じように新しい `Point` を作成し `X` の値を変更した場合。</span><span class="sxs-lookup"><span data-stu-id="9eb21-131">To create a new `Point` just like an existing `Point` but with the value of `X` changed.</span></span>
> 
> <span data-ttu-id="9eb21-132">呼び出し元のパラメーターがサポートされている場合に、*式*の構文形式を追加する必要があるかどうかにかかわらず、未解決の質問になります。したがって、 *with 式* *に加え*て*ではなく、これを*行うことができます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-132">It is an open question whether or not the syntactic form of the *with-expression* is worth adding once we have support for caller-receiver parameters, so it is possible we would do this *instead of* rather than *in addition to* the *with-expression*.</span></span>

- <span data-ttu-id="9eb21-133">[]**オープン懸案事項**:*呼び出し元の既定の引数*が他の引数に対して評価される順序は何ですか?</span><span class="sxs-lookup"><span data-stu-id="9eb21-133">[ ] **Open issue**: What is the order in which a *caller-receiver default-argument* is evaluated with respect to other arguments?</span></span> <span data-ttu-id="9eb21-134">指定されていない場合は、</span><span class="sxs-lookup"><span data-stu-id="9eb21-134">Should we say that it is unspecified?</span></span>

### <a name="with-expressions"></a><span data-ttu-id="9eb21-135">式の使用</span><span class="sxs-lookup"><span data-stu-id="9eb21-135">with-expressions</span></span>

<span data-ttu-id="9eb21-136">新しい式の形式が提案されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-136">A new expression form is proposed:</span></span>

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

<span data-ttu-id="9eb21-137">`with` トークンは、新しい状況依存のキーワードです。</span><span class="sxs-lookup"><span data-stu-id="9eb21-137">The token `with` is a new context-sensitive keyword.</span></span>

<span data-ttu-id="9eb21-138">*With_initializer*の左側の各*識別子*は、アクセス可能なインスタンスフィールドまたは*with_expression*の*primary_expression*の型のプロパティにバインドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-138">Each *identifier* on the left of a *with_initializer* must bind to an accessible instance field or property of the type of the *primary_expression* of the *with_expression*.</span></span> <span data-ttu-id="9eb21-139">指定された*with_expression*のこれらの識別子の間に重複する名前がない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-139">There may be no duplicated name among these identifiers of a given *with_expression*.</span></span>

<span data-ttu-id="9eb21-140">フォームの*with_expression*</span><span class="sxs-lookup"><span data-stu-id="9eb21-140">A *with_expression* of the form</span></span>

> <span data-ttu-id="9eb21-141">*e1* `with` `{`*識別子* = *e2*,... `}`</span><span class="sxs-lookup"><span data-stu-id="9eb21-141">*e1* `with` `{` *identifier* = *e2*, ... `}`</span></span>

<span data-ttu-id="9eb21-142">は、フォームの呼び出しとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-142">is treated as an invocation of the form</span></span>

> <span data-ttu-id="9eb21-143">*e1*`.With(`*identifier2*`:` *e2*,...`)`</span><span class="sxs-lookup"><span data-stu-id="9eb21-143">*e1*`.With(`*identifier2*`:` *e2*, ...`)`</span></span>

<span data-ttu-id="9eb21-144">ここでは、`With` という名前の各メソッドに対して、 *e1*のアクセス可能なインスタンスメンバーである場合、 *identifier2*を、そのメソッドの最初のパラメーターの名前として選択します。このメソッドは、*識別子*にバインドされたインスタンスフィールドまたはプロパティと同じメンバーである呼び出し側のパラメーターを持ちます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-144">Where, for each method named `With` that is an accessible instance member of *e1*, we select *identifier2* as the name of the first parameter in that method that has a caller-receiver parameter that is the same member as the instance field or property bound to *identifier*.</span></span> <span data-ttu-id="9eb21-145">そのようなパラメーターを特定できない場合は、メソッドが考慮から除外されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-145">If no such parameter can be identified that method is eliminated from consideration.</span></span> <span data-ttu-id="9eb21-146">呼び出されるメソッドは、オーバーロードの解決によって残りの候補の中から選択されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-146">The method to be invoked is selected from among the remaining candidates by overload resolution.</span></span>

> <span data-ttu-id="9eb21-147">**設計メモ**: 呼び出し元のパラメーターを指定すると、この特殊な構文形式を使用しなくても、*式*の多くの利点が得られます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-147">**Design Notes**: Given caller-receiver parameters, many of the benefits of the *with-expression* are available without this special syntax form.</span></span> <span data-ttu-id="9eb21-148">このため、必要かどうかを検討しています。</span><span class="sxs-lookup"><span data-stu-id="9eb21-148">We are therefore considering whether or not it is needed.</span></span> <span data-ttu-id="9eb21-149">主な利点は、パラメーターの名前ではなく、フィールドとプロパティの名前を使用してプログラムをプログラミングできることです。</span><span class="sxs-lookup"><span data-stu-id="9eb21-149">Its main benefit is allowing one to program in terms of the names of fields and properties, rather than in terms of the names of parameters.</span></span> <span data-ttu-id="9eb21-150">この方法では、読みやすさとツールの品質の両方が向上します (たとえば、 *with_expression*の識別子に対する "定義への移動" は、メソッドパラメーターではなく、プロパティに移動します)。</span><span class="sxs-lookup"><span data-stu-id="9eb21-150">In this way we improve both readability and the quality of tooling (e.g. go-to-definition on the identifier of a *with_expression* would navigate to the property rather than to a method parameter).</span></span>

- <span data-ttu-id="9eb21-151">[]**未解決の問題**: 拡張メソッドをサポートするには、この説明を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-151">[ ] **Open issue**: This description should be modified to support extension methods.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="9eb21-152">パターン一致</span><span class="sxs-lookup"><span data-stu-id="9eb21-152">pattern-matching</span></span>

<span data-ttu-id="9eb21-153">`Deconstruct` の仕様とパターンマッチングとの関係については、[パターンマッチングの仕様](csharp-8.0/patterns.md#positional-pattern)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9eb21-153">See the [Pattern Matching Specification](csharp-8.0/patterns.md#positional-pattern) for a specification of `Deconstruct` and its relationship to pattern-matching.</span></span>

> <span data-ttu-id="9eb21-154">**設計メモ**: ここで指定されているコンパイラで生成された `Deconstruct` と、パターンマッチングの仕様 (レコードの宣言)</span><span class="sxs-lookup"><span data-stu-id="9eb21-154">**Design Notes**: By virtue of the compiler-generated `Deconstruct` as specified herein, and the specification for pattern-matching, a record declaration</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="9eb21-155">では、次のように位置パターン一致がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-155">will support positional pattern-matching as follows</span></span>
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a><span data-ttu-id="9eb21-156">レコード型の宣言</span><span class="sxs-lookup"><span data-stu-id="9eb21-156">record type declarations</span></span>

<span data-ttu-id="9eb21-157">`class` または `struct` 宣言の構文は、値パラメーターをサポートするために拡張されています。パラメーターは、型のプロパティになります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-157">The syntax for a `class` or `struct` declaration is extended to support value parameters; the parameters become properties of the type:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> <span data-ttu-id="9eb21-158">**設計**上の注意: レコードの種類は、クラス本体で明示的に宣言されたメンバーを必要としない場合に便利です。そのため、宣言の構文を変更して、本文が単にセミコロンになるようにします。</span><span class="sxs-lookup"><span data-stu-id="9eb21-158">**Design Notes**: Because record types are often useful without the need for any members explicitly declared in a class-body, we modify the syntax of the declaration to allow a body to be simply a semicolon.</span></span>

<span data-ttu-id="9eb21-159">レコード*パラメーター*を使用して宣言されたクラス (構造体) はレコード*クラス*(*レコード構造体*) と呼ばれ、そのいずれかが*レコード型*です。</span><span class="sxs-lookup"><span data-stu-id="9eb21-159">A class (struct) declared with the *record-parameters* is called a *record class* (*record struct*), either of which is a *record type*.</span></span>

- <span data-ttu-id="9eb21-160">[] **Open issue**: レコード型宣言内に表示できるように、文法に*primary_constructor_body*を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-160">[ ] **Open issue**: We need to include *primary_constructor_body* in the grammar so that it can appear inside a record type declaration.</span></span>
- <span data-ttu-id="9eb21-161">[]**未解決の問題**: パラメーター名の名前の競合ルールは何ですか?</span><span class="sxs-lookup"><span data-stu-id="9eb21-161">[ ] **Open issue**: What are the name conflict rules for the parameter names?</span></span> <span data-ttu-id="9eb21-162">1つの型パラメーターまたは別の*レコードパラメーター*との競合は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="9eb21-162">Presumably one is not allowed to conflict with a type parameter or another *record-parameter*.</span></span>
- <span data-ttu-id="9eb21-163">[]**未解決の問題**: レコードパラメーターのスコープを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-163">[ ] **Open issue**: We need to specify the scope of the record-parameters.</span></span> <span data-ttu-id="9eb21-164">使用できる場所</span><span class="sxs-lookup"><span data-stu-id="9eb21-164">Where can they be used?</span></span> <span data-ttu-id="9eb21-165">インスタンスフィールド初期化子と*primary_constructor_body*少なくともであるとします。</span><span class="sxs-lookup"><span data-stu-id="9eb21-165">Presumably within instance field initializers and *primary_constructor_body* at least.</span></span>
- <span data-ttu-id="9eb21-166">[] **Open issue**: レコードの種類の宣言を部分的に使用できますか?</span><span class="sxs-lookup"><span data-stu-id="9eb21-166">[ ] **Open issue**: Can a record type declaration be partial?</span></span> <span data-ttu-id="9eb21-167">その場合、各部分でパラメーターを繰り返す必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="9eb21-167">If so, must the parameters be repeated on each part?</span></span>

#### <a name="members-of-a-record-type"></a><span data-ttu-id="9eb21-168">レコードの種類のメンバー</span><span class="sxs-lookup"><span data-stu-id="9eb21-168">Members of a record type</span></span>

<span data-ttu-id="9eb21-169">レコード型には、*クラス本体*で宣言されたメンバーに加えて、次の追加メンバーがあります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-169">In addition to the members declared in the *class-body*, a record type has the following additional members:</span></span>

##### <a name="primary-constructor"></a><span data-ttu-id="9eb21-170">プライマリコンストラクター</span><span class="sxs-lookup"><span data-stu-id="9eb21-170">Primary Constructor</span></span>

<span data-ttu-id="9eb21-171">レコード型には、型宣言の値パラメーターに対応するシグネチャを持つ `public` コンストラクターがあります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-171">A record type has a `public` constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="9eb21-172">これは、型の*プライマリコンストラクター*と呼ばれ、暗黙的に宣言された*既定のコンストラクター*は抑制されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-172">This is called the *primary constructor* for the type, and causes the implicitly declared *default constructor* to be suppressed.</span></span>

<span data-ttu-id="9eb21-173">実行時のプライマリコンストラクター</span><span class="sxs-lookup"><span data-stu-id="9eb21-173">At runtime the primary constructor</span></span>

* <span data-ttu-id="9eb21-174">値パラメーターに対応するプロパティについて、コンパイラによって生成されるバッキングフィールドを初期化します (これらのプロパティがコンパイラによって指定されている場合)。[「1.1.2」を参照してください](#1.1.2))。そうしたら</span><span class="sxs-lookup"><span data-stu-id="9eb21-174">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; [see 1.1.2](#1.1.2)); then</span></span>
* <span data-ttu-id="9eb21-175">*クラス本体*に出現するインスタンスフィールド初期化子を実行します。それから</span><span class="sxs-lookup"><span data-stu-id="9eb21-175">executes the instance field initializers appearing in the *class-body*; and then</span></span>
* <span data-ttu-id="9eb21-176">基底クラスのコンストラクターを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-176">invokes a base class constructor:</span></span>
    * <span data-ttu-id="9eb21-177">*Record_base_arguments*に引数がある場合は、これらの引数を使用してオーバーロードの解決によって選択された基本コンストラクターが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-177">If there are arguments in the *record_base_arguments*, a base constructor selected by overload resolution with these arguments is invoked;</span></span>
    * <span data-ttu-id="9eb21-178">それ以外の場合は、引数なしで基底コンストラクターが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-178">Otherwise a base constructor is invoked with no arguments.</span></span>
* <span data-ttu-id="9eb21-179">各*primary_constructor_body*の本文をソースの順序で実行します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-179">executes the body of each *primary_constructor_body*, if any, in source order.</span></span>

- <span data-ttu-id="9eb21-180">[]**未解決の問題**: 特にパーシャルのコンパイル単位全体で、その順序を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-180">[ ] **Open issue**: We need to specify that order, particularly across compilation units for partials.</span></span>
- <span data-ttu-id="9eb21-181">[]**未解決の問題**: 明示的に宣言されたすべてのコンストラクターがプライマリコンストラクターにチェーンされる必要があることを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-181">[ ] **Open Issue**: We need to specify that every explicitly declared constructor must chain to the primary constructor.</span></span>
- <span data-ttu-id="9eb21-182">[] **Open issue**: プライマリコンストラクターでアクセス修飾子を変更することが許可されているかどうか。</span><span class="sxs-lookup"><span data-stu-id="9eb21-182">[ ] **Open issue**: Should it be allowed to change the access modifier on the primary constructor?</span></span>
- <span data-ttu-id="9eb21-183">[] **Open issue**: レコード構造体では、レコードパラメーターがないというエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-183">[ ] **Open issue**: In a record struct, it is an error for there to be no record parameters?</span></span>

##### <a name="primary-constructor-body"></a><span data-ttu-id="9eb21-184">プライマリコンストラクター本体</span><span class="sxs-lookup"><span data-stu-id="9eb21-184">Primary constructor body</span></span>

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

<span data-ttu-id="9eb21-185">*Primary_constructor_body*は、レコード型の宣言内でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-185">A *primary_constructor_body* may only be used within a record type declaration.</span></span> <span data-ttu-id="9eb21-186">*Primary_constructor_body*の*識別子*は、宣言されているレコードの種類に名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-186">The *identifier* of a *primary_constructor_body* shall name the record type in which it is declared.</span></span>

<span data-ttu-id="9eb21-187">*Primary_constructor_body*は、自身でメンバーを宣言するのではなく、プログラマがレコード型のプライマリコンストラクターの属性を提供し、そのアクセスを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="9eb21-187">The *primary_constructor_body* does not declare a member on its own, but is a way for the programmer to provide attributes for, and specify the access of, a record type's primary constructor.</span></span> <span data-ttu-id="9eb21-188">また、レコードの種類のインスタンスが構築されたときに実行されるコードを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-188">It also enables the programmer to provide additional code that will be executed when an instance of the record type is constructed.</span></span>

- <span data-ttu-id="9eb21-189">[]**未解決の問題**: 構造体の既定のコンストラクターがこれをバイパスすることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9eb21-189">[ ] **Open issue**: We should note that a struct default constructor bypasses this.</span></span>
- <span data-ttu-id="9eb21-190">[]**未解決の問題**: 初期化の実行順序を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-190">[ ] **Open issue**: We should specify the execution order of initialization.</span></span>
- <span data-ttu-id="9eb21-191">[]**オープン問題**: レコード型以外の宣言で (属性や修飾子を使用せずに) *primary_constructor_body*のような処理を許可し、インスタンスフィールド初期化子のコードと同じように扱う必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="9eb21-191">[ ] **Open issue**: Should we allow something like a *primary_constructor_body* (presumably without attributes and modifiers) in a non-record type declaration, and treat it like we would the code of an instance field initializer?</span></span>

##### <a name="properties"></a><span data-ttu-id="9eb21-192">Properties</span><span class="sxs-lookup"><span data-stu-id="9eb21-192">Properties</span></span>

<span data-ttu-id="9eb21-193">レコード型宣言の各レコードパラメーターには、対応する `public` プロパティメンバーがあります。このメンバーの名前と型は、値パラメーターの宣言から取得されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-193">For each record parameter of a record type declaration there is a corresponding `public` property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="9eb21-194">この名前は、 *record_property_name*の*識別子*、存在する場合は*record_parameter*の識別子、それ以外の場合は*の識別子です*。</span><span class="sxs-lookup"><span data-stu-id="9eb21-194">Its name is the *identifier* of the *record_property_name*, if present, or the *identifier* of the *record_parameter* otherwise.</span></span> <span data-ttu-id="9eb21-195">`get` アクセサーを持つ具象 (つまり非抽象) パブリックプロパティが明示的に宣言または継承されていない場合は、コンパイラによって次のように生成されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-195">If no concrete (i.e. non-abstract) public property with a `get` accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

* <span data-ttu-id="9eb21-196">*レコード構造体*または `sealed`*レコードクラス*の場合:</span><span class="sxs-lookup"><span data-stu-id="9eb21-196">For a *record struct* or a `sealed` *record class*:</span></span>
 * <span data-ttu-id="9eb21-197">`private` `readonly` フィールドは、`readonly` プロパティのバッキングフィールドとして生成されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-197">A `private` `readonly` field is produced as a backing field for a `readonly` property.</span></span> <span data-ttu-id="9eb21-198">この値は、対応するプライマリコンストラクターパラメーターの値を使用して構築時に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-198">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span>
 * <span data-ttu-id="9eb21-199">バッキングフィールドの値を返すために、プロパティの `get` アクセサーが実装されています。</span><span class="sxs-lookup"><span data-stu-id="9eb21-199">The property's `get` accessor is implemented to return the value of the backing field.</span></span>
 * <span data-ttu-id="9eb21-200">各 "一致する" 継承された仮想プロパティの `get` アクセサーはオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-200">Each "matching" inherited virtual property's `get` accessor is overridden.</span></span>

> <span data-ttu-id="9eb21-201">**デザインメモ**: つまり、基本クラスを拡張する場合、またはレコードパラメーターと同じ名前と型を持つパブリック抽象プロパティを宣言するインターフェイスを実装する場合は、そのプロパティがオーバーライドまたは実装されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-201">**Design notes**: In other words, if you extend a base class or implement an interface that declares a public abstract property with the same name and type as a record parameter, that property is overridden or implemented.</span></span>

- <span data-ttu-id="9eb21-202">[] **Open issue**: 明示的に宣言されている場合、プロパティのアクセス修飾子を変更できるようにする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="9eb21-202">[ ] **Open issue**: Should it be possible to change the access modifier on a property when it is explicitly declared?</span></span>
- <span data-ttu-id="9eb21-203">[]**未解決の問題**: プロパティのフィールドを置き換えることができるかどうか。</span><span class="sxs-lookup"><span data-stu-id="9eb21-203">[ ] **Open issue**: Should it be possible to substitute a field for a property?</span></span>

##### <a name="object-methods"></a><span data-ttu-id="9eb21-204">オブジェクト メソッド</span><span class="sxs-lookup"><span data-stu-id="9eb21-204">Object Methods</span></span>

<span data-ttu-id="9eb21-205">*レコード構造体*または `sealed`*レコードクラス*では、ユーザーが指定しない限り、`object.GetHashCode()` および `object.Equals(object)` メソッドの実装がコンパイラによって生成されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-205">For a *record struct* or a `sealed` *record class*, implementations of the methods `object.GetHashCode()` and `object.Equals(object)` are produced by the compiler unless provided by the user.</span></span>

- <span data-ttu-id="9eb21-206">[]**未解決の問題**: 実装を正確に指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-206">[ ] **Open issue**: We should precisely specify their implementation.</span></span>
- <span data-ttu-id="9eb21-207">[]**未解決の問題**: レコードの種類に `IEquatable<T>` インターフェイスを追加し、実装が提供されることを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-207">[ ] **Open issue**: We should also add the interface `IEquatable<T>` for the record type and specify that implementations are provided.</span></span>
- <span data-ttu-id="9eb21-208">[]**未解決の問題**: すべての `IEquatable<T>.Equals`を実装するように指定する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-208">[ ] **Open issue**: We should also specify that we implement every `IEquatable<T>.Equals`.</span></span>
- <span data-ttu-id="9eb21-209">[]**オープン問題**: レコードの継承の面で、Equals の問題の解決方法を正確に指定する必要があります。具体的には、対称、推移、再帰などの等値メソッドを生成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-209">[ ] **Open issue**: We should specify precisely how we solve the problem of Equals in the face of record inheritance: specifically how we generate equality methods such that they are symmetric, transitive, reflexive, etc.</span></span>
- <span data-ttu-id="9eb21-210">[]**未解決の問題**: レコードの種類の `operator ==` と `operator !=` を実装することが提案されています。</span><span class="sxs-lookup"><span data-stu-id="9eb21-210">[ ] **Open issue**: It has been proposed that we implement `operator ==` and `operator !=` for record types.</span></span>
- <span data-ttu-id="9eb21-211">[]**未解決の問題**: `object.ToString`の実装を自動生成しますか?</span><span class="sxs-lookup"><span data-stu-id="9eb21-211">[ ] **Open issue**: Should we auto-generate an implementation of `object.ToString`?</span></span>

##### `Deconstruct`

<span data-ttu-id="9eb21-212">レコード型には、ユーザーによって署名が指定されていない限り、コンパイラによって生成される `public` メソッド `void Deconstruct` があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-212">A record type has a compiler-generated `public` method `void Deconstruct` unless one with any signature is provided by the user.</span></span> <span data-ttu-id="9eb21-213">各パラメーターは、レコード型の対応するパラメーターと同じ名前および型の `out` パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="9eb21-213">Each parameter is an `out` parameter of the same name and type as the corresponding parameter of the record type.</span></span> <span data-ttu-id="9eb21-214">コンパイラによって提供されるこのメソッドの実装では、各 `out` パラメーターに対応するプロパティの値を割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-214">The compiler-provided implementation of this method shall assign each `out` parameter with the value of the corresponding property.</span></span>

<span data-ttu-id="9eb21-215">`Deconstruct`のセマンティクスについては[、パターンマッチングの仕様](csharp-8.0/patterns.md#positional-pattern)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9eb21-215">See [the pattern-matching specification](csharp-8.0/patterns.md#positional-pattern) for the semantics of `Deconstruct`.</span></span>

##### <a name="with-method"></a><span data-ttu-id="9eb21-216">`With` メソッド</span><span class="sxs-lookup"><span data-stu-id="9eb21-216">`With` method</span></span>

<span data-ttu-id="9eb21-217">`With` という名前のユーザー宣言メンバーが宣言されていない限り、レコード型には、その戻り値の型がレコード型自体である `With` という名前のコンパイラが提供するメソッドがあり、これらのパラメーターがレコード型宣言に出現する順序で各*レコードパラメーター*に対応する1つの値パラメーターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9eb21-217">Unless there is a user-declared member named `With` declared, a record type has a compiler-provided method named `With` whose return type is the record type itself, and containing one value parameter corresponding to each *record-parameter* in the same order that these parameters appear in the record type declaration.</span></span> <span data-ttu-id="9eb21-218">各パラメーターには、対応するプロパティの*呼び出し元と受信*側の既定の引数が必要です。</span><span class="sxs-lookup"><span data-stu-id="9eb21-218">Each parameter shall have a *caller-receiver default-argument* of the corresponding property.</span></span>

<span data-ttu-id="9eb21-219">`abstract` レコードクラスでは、コンパイラによって提供される `With` メソッドは abstract です。</span><span class="sxs-lookup"><span data-stu-id="9eb21-219">In an `abstract` record class, the compiler-provided `With` method is abstract.</span></span> <span data-ttu-id="9eb21-220">レコード構造体または sealed レコードクラスでは、コンパイラによって提供される `With` メソッドが `sealed`ます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-220">In a record struct, or a sealed record class, the compiler-provided `With` method is `sealed`.</span></span> <span data-ttu-id="9eb21-221">それ以外の場合、コンパイラによって提供される `With` メソッドは ' 仮想であり、その実装では、パラメーターを引数として使用してプライマリコンストラクターを呼び出し、パラメーターから新しいインスタンスを作成し、その新しいインスタンスを返すことによって生成される新しいインスタンスを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-221">Otherwise the compiler-provided `With` method is \`virtual and its implementation shall return a new instance produced by invoking the primary constructor with the parameters as arguments to create a new instance from the parameters, and return that new instance.</span></span>

- <span data-ttu-id="9eb21-222">[]**オープンの問題**: オーバーライドする条件、または実装されたインターフェイスから継承された仮想 `With` メソッドまたは `With` メソッドを実装する条件の下でも指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-222">[ ] **Open issue**: We should also specify under what conditions we override or implement inherited virtual `With` methods or `With` methods from implemented interfaces.</span></span>
- <span data-ttu-id="9eb21-223">[]**未解決の問題**: 非仮想 `With` メソッドを継承するとどうなるかを説明する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-223">[ ] **Open issue**: We should say what happens when we inherit a non-virtual `With` method.</span></span>

> <span data-ttu-id="9eb21-224">**設計メモ**: レコードの種類は既定では不変であるため、`With` メソッドを使用すると、既存のインスタンスと同じで、新しい値を指定したプロパティを持つ新しいインスタンスを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-224">**Design notes**: Because record types are by default immutable, the `With` method provides a way of creating a new instance that is the same as an existing instance but with selected properties given new values.</span></span> <span data-ttu-id="9eb21-225">たとえば、次のように指定します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-225">For example, given</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="9eb21-226">コンパイラによって指定されたメンバーが存在します</span><span class="sxs-lookup"><span data-stu-id="9eb21-226">there is a compiler-provided member</span></span>
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> <span data-ttu-id="9eb21-227">これにより、レコードの種類の変数が有効になります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-227">Which enables an variable of the record type</span></span>
> ```cs
> var p = new Point(3, 4);
> ```
> <span data-ttu-id="9eb21-228">1つ以上のプロパティが異なるインスタンスに置き換える場合</span><span class="sxs-lookup"><span data-stu-id="9eb21-228">to be replaced with an instance that has one or more properties different</span></span>
> ```cs
>     p = p.With(X: 5);
> ```
> <span data-ttu-id="9eb21-229">これは、 *with_expression*を使用して表すこともできます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-229">This can also be expressed using the *with_expression*:</span></span>
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a><span data-ttu-id="9eb21-230">例</span><span class="sxs-lookup"><span data-stu-id="9eb21-230">Examples</span></span>

#### <a name="compatibility-of-record-types"></a><span data-ttu-id="9eb21-231">レコードの種類の互換性</span><span class="sxs-lookup"><span data-stu-id="9eb21-231">Compatibility of record types</span></span>

<span data-ttu-id="9eb21-232">プログラマはレコードの種類の宣言にメンバーを追加できるので、多くの場合、既存のクライアントに影響を与えずにレコードの要素のセットを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-232">Because the programmer can add members to a record type declaration, it is often possible to change the set of record elements without affecting existing clients.</span></span> <span data-ttu-id="9eb21-233">たとえば、レコード型の初期バージョンが指定されているとします。</span><span class="sxs-lookup"><span data-stu-id="9eb21-233">For example, given an initial version of a record type</span></span>

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

<span data-ttu-id="9eb21-234">レコード型の新しい要素は、バイナリまたはソースの互換性に影響を与えることなく、型の次のリビジョンで方に追加できます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-234">A new element of the record type can be compatibly added in the next revision of the type without affecting binary or source compatibility:</span></span>

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a><span data-ttu-id="9eb21-235">レコード構造体の例</span><span class="sxs-lookup"><span data-stu-id="9eb21-235">record struct example</span></span>

<span data-ttu-id="9eb21-236">このレコード構造体</span><span class="sxs-lookup"><span data-stu-id="9eb21-236">This record struct</span></span>

```cs
public struct Pair(object First, object Second);
```

<span data-ttu-id="9eb21-237">このコードに変換されます</span><span class="sxs-lookup"><span data-stu-id="9eb21-237">is translated to this code</span></span>

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- <span data-ttu-id="9eb21-238">[] **Open issue**: Equals (pair other) の実装がペアのパブリックメンバーである必要がありますか?</span><span class="sxs-lookup"><span data-stu-id="9eb21-238">[ ] **Open issue**: should the implementation of Equals(Pair other) be a public member of Pair?</span></span>
- <span data-ttu-id="9eb21-239">[]**未解決の懸案事項**: 継承の場合、この `Equals` の実装は対称ではありません。</span><span class="sxs-lookup"><span data-stu-id="9eb21-239">[ ] **Open issue**: This implementation of `Equals` is not symmetric in the face of inheritance.</span></span>
- <span data-ttu-id="9eb21-240">[] **Open issue**: レコードが `operator ==` を宣言し、`operator !=`しますか?</span><span class="sxs-lookup"><span data-stu-id="9eb21-240">[ ] **Open issue**: Should a record declare `operator ==` and `operator !=`?</span></span>

> <span data-ttu-id="9eb21-241">**設計メモ**: 1 つのレコードの種類は別のレコード型から継承でき、この `Equals` の実装は対称ではないため、正しくありません。</span><span class="sxs-lookup"><span data-stu-id="9eb21-241">**Design notes**: Because one record type can inherit from another, and this implementation of `Equals` would not be symmetric in that case, it is not correct.</span></span> <span data-ttu-id="9eb21-242">ここでは、次のように等価性を実装することを提案します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-242">We propose to implement equality this way:</span></span>
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> <span data-ttu-id="9eb21-243">派生レコードは `override EqualityContract`します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-243">Derived records would `override EqualityContract`.</span></span> <span data-ttu-id="9eb21-244">代替手段として、継承を制限する方が適しています。</span><span class="sxs-lookup"><span data-stu-id="9eb21-244">The less attractive alternative is to restrict inheritance.</span></span>

#### <a name="sealed-record-example"></a><span data-ttu-id="9eb21-245">シールされたレコードの例</span><span class="sxs-lookup"><span data-stu-id="9eb21-245">sealed record example</span></span>

<span data-ttu-id="9eb21-246">このシールされたレコードクラス</span><span class="sxs-lookup"><span data-stu-id="9eb21-246">This sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa);
```

<span data-ttu-id="9eb21-247">は、次のコードに変換されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-247">is translated into this code</span></span>

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a><span data-ttu-id="9eb21-248">abstract record クラスの例</span><span class="sxs-lookup"><span data-stu-id="9eb21-248">abstract record class example</span></span>

<span data-ttu-id="9eb21-249">この抽象レコードクラス</span><span class="sxs-lookup"><span data-stu-id="9eb21-249">This abstract record class</span></span>

```cs
public abstract class Person(string Name);
```

<span data-ttu-id="9eb21-250">は、次のコードに変換されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-250">is translated into this code</span></span>

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a><span data-ttu-id="9eb21-251">抽象レコードとシールレコードの結合</span><span class="sxs-lookup"><span data-stu-id="9eb21-251">combining abstract and sealed records</span></span>

<span data-ttu-id="9eb21-252">上記の抽象レコードクラス `Person`、この sealed レコードクラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="9eb21-252">Given the abstract record class `Person` above, this sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

<span data-ttu-id="9eb21-253">は、次のコードに変換されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-253">is translated into this code</span></span>

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a><span data-ttu-id="9eb21-254">短所</span><span class="sxs-lookup"><span data-stu-id="9eb21-254">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="9eb21-255">他の言語機能と同様に、この機能の恩恵を受けるプログラムのC#本文に対して、さらに複雑な言語を端的にする必要があるかどうかを質問する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9eb21-255">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="9eb21-256">代替</span><span class="sxs-lookup"><span data-stu-id="9eb21-256">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="9eb21-257">6では、*プライマリコンストラクター*の追加をC#検討しています。</span><span class="sxs-lookup"><span data-stu-id="9eb21-257">We considered adding *primary constructors* in C# 6.</span></span> <span data-ttu-id="9eb21-258">この提案と同じ構文サーフェイスが利用されていますが、レコードによって提供される利点が不足していることがわかりました。</span><span class="sxs-lookup"><span data-stu-id="9eb21-258">Although they occupy the same syntactic surface as this proposal, we found that they fell short of the advantages offered by records.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="9eb21-259">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="9eb21-259">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="9eb21-260">オープンな質問は、提案の本文に表示されます。</span><span class="sxs-lookup"><span data-stu-id="9eb21-260">Open questions appear in the body of the proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="9eb21-261">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="9eb21-261">Design meetings</span></span>

<span data-ttu-id="9eb21-262">TBD</span><span class="sxs-lookup"><span data-stu-id="9eb21-262">TBD</span></span>
