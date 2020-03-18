---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/26/2020
ms.locfileid: "79484106"
---
# <a name="nullable-reference-types-specification"></a><span data-ttu-id="3f201-101">Null 許容の参照型の仕様</span><span class="sxs-lookup"><span data-stu-id="3f201-101">Nullable Reference Types Specification</span></span>

<span data-ttu-id="3f201-102">これは進行中の作業です。いくつかの部分が不足しているか、不完全です。</span><span class="sxs-lookup"><span data-stu-id="3f201-102">\*\*\* This is a work in progress - several parts are missing or incomplete.</span></span> ***

## <a name="syntax"></a><span data-ttu-id="3f201-103">構文</span><span class="sxs-lookup"><span data-stu-id="3f201-103">Syntax</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="3f201-104">null 許容参照型</span><span class="sxs-lookup"><span data-stu-id="3f201-104">Nullable reference types</span></span>

<span data-ttu-id="3f201-105">Null 許容型の参照型には、null 許容値型の短い形式と同じ構文 `T?` ますが、対応する長い形式はありません。</span><span class="sxs-lookup"><span data-stu-id="3f201-105">Nullable reference types have the same syntax `T?` as the short form of nullable value types, but do not have a corresponding long form.</span></span>

<span data-ttu-id="3f201-106">この仕様では、現在の `nullable_type` 運用環境の名前が `nullable_value_type`に変更され、`nullable_reference_type` 運用環境が追加されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-106">For the purposes of the specification, the current `nullable_type` production is renamed to `nullable_value_type`, and a `nullable_reference_type` production is added:</span></span>

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

<span data-ttu-id="3f201-107">`nullable_reference_type` 内の `non_nullable_reference_type` は、null 非許容の参照型 (クラス、インターフェイス、デリゲート、または配列) であるか、null 非許容の参照型 (`class` 制約を通じて、または `object`以外のクラス) であることが制限されている型パラメーターである必要があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-107">The `non_nullable_reference_type` in a `nullable_reference_type` must be a non-nullable reference type (class, interface, delegate or array), or a type parameter that is constrained to be a non-nullable reference type (through the `class` constraint, or a class other than `object`).</span></span>

<span data-ttu-id="3f201-108">Null 許容の参照型は、次の位置では発生しません。</span><span class="sxs-lookup"><span data-stu-id="3f201-108">Nullable reference types cannot occur in the following positions:</span></span>

- <span data-ttu-id="3f201-109">基底クラスまたはインターフェイスとして</span><span class="sxs-lookup"><span data-stu-id="3f201-109">as a base class or interface</span></span>
- <span data-ttu-id="3f201-110">`member_access` の受信者として</span><span class="sxs-lookup"><span data-stu-id="3f201-110">as the receiver of a `member_access`</span></span>
- <span data-ttu-id="3f201-111">`object_creation_expression` 内の `type` として</span><span class="sxs-lookup"><span data-stu-id="3f201-111">as the `type` in an `object_creation_expression`</span></span>
- <span data-ttu-id="3f201-112">`delegate_creation_expression` 内の `delegate_type` として</span><span class="sxs-lookup"><span data-stu-id="3f201-112">as the `delegate_type` in a `delegate_creation_expression`</span></span>
- <span data-ttu-id="3f201-113">`is_expression`内の `type` として `catch_clause` または `type_pattern`</span><span class="sxs-lookup"><span data-stu-id="3f201-113">as the `type` in an `is_expression`, a `catch_clause` or a `type_pattern`</span></span>
- <span data-ttu-id="3f201-114">完全修飾インターフェイスメンバー名の `interface` として</span><span class="sxs-lookup"><span data-stu-id="3f201-114">as the `interface` in a fully qualified interface member name</span></span>

<span data-ttu-id="3f201-115">Null 許容の注釈コンテキストが無効になっている `nullable_reference_type` で警告が示されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-115">A warning is given on a `nullable_reference_type` where the nullable annotation context is disabled.</span></span>

### <a name="nullable-class-constraint"></a><span data-ttu-id="3f201-116">Nullable クラスの制約</span><span class="sxs-lookup"><span data-stu-id="3f201-116">Nullable class constraint</span></span>

<span data-ttu-id="3f201-117">`class` 制約には、null 許容の対応する `class?`があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-117">The `class` constraint has a nullable counterpart `class?`:</span></span>

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a><span data-ttu-id="3f201-118">Null を許容しない演算子</span><span class="sxs-lookup"><span data-stu-id="3f201-118">The null-forgiving operator</span></span>

<span data-ttu-id="3f201-119">修正後の `!` 演算子は、null 非厳格演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="3f201-119">The post-fix `!` operator is called the null-forgiving operator.</span></span>

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

<span data-ttu-id="3f201-120">`primary_expression` は参照型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-120">The `primary_expression` must be of a reference type.</span></span>  

<span data-ttu-id="3f201-121">後置 `!` 演算子には、ランタイムの影響はありません。基になる式の結果に評価されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-121">The postfix `!` operator has no runtime effect - it evaluates to the result of the underlying expression.</span></span> <span data-ttu-id="3f201-122">唯一のロールは、式の null 状態を変更し、使用時に指定された警告を制限することです。</span><span class="sxs-lookup"><span data-stu-id="3f201-122">Its only role is to change the null state of the expression, and to limit warnings given on its use.</span></span>

### <a name="nullable-implicitly-typed-local-variables"></a><span data-ttu-id="3f201-123">暗黙的に型指定された null 許容のローカル変数</span><span class="sxs-lookup"><span data-stu-id="3f201-123">nullable implicitly typed local variables</span></span>

<span data-ttu-id="3f201-124">`var` は、参照型の注釈付きの型を推測します。</span><span class="sxs-lookup"><span data-stu-id="3f201-124">`var` infers an annotated type for reference types.</span></span>
<span data-ttu-id="3f201-125">たとえば `var s = "";` では、`var` は `string?`として推論されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-125">For instance, in `var s = "";` the `var` is inferred as `string?`.</span></span>

### <a name="nullable-compiler-directives"></a><span data-ttu-id="3f201-126">Null 許容のコンパイラディレクティブ</span><span class="sxs-lookup"><span data-stu-id="3f201-126">Nullable compiler directives</span></span>

<span data-ttu-id="3f201-127">`#nullable` ディレクティブは、null 値を許容する注釈と警告コンテキストを制御します。</span><span class="sxs-lookup"><span data-stu-id="3f201-127">`#nullable` directives control the nullable annotation and warning contexts.</span></span>

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

<span data-ttu-id="3f201-128">`#pragma warning` ディレクティブが拡張され、null 許容の警告コンテキストを変更できるようになりました。また、既定で無効になっている場合でも、で個々の警告を有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="3f201-128">`#pragma warning` directives are expanded to allow changing the nullable warning context, and to allow individual warnings to be enabled on even when they're disabled by default:</span></span>

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

<span data-ttu-id="3f201-129">`pragma_warning_body` の新しい形式では、`warning_action`ではなく `nullable_action`が使用されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3f201-129">Note that the new form of `pragma_warning_body` uses `nullable_action`, not `warning_action`.</span></span>

## <a name="nullable-contexts"></a><span data-ttu-id="3f201-130">null 許容コンテキスト</span><span class="sxs-lookup"><span data-stu-id="3f201-130">Nullable contexts</span></span>

<span data-ttu-id="3f201-131">ソースコードのすべての行には、 *null 許容の注釈コンテキスト*と*null 許容の警告コンテキスト*があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-131">Every line of source code has a *nullable annotation context* and a *nullable warning context*.</span></span> <span data-ttu-id="3f201-132">これらは、null 許容の注釈が有効かどうか、および null 値許容の警告を与えるかどうかを制御します。</span><span class="sxs-lookup"><span data-stu-id="3f201-132">These control whether nullable annotations have effect, and whether nullability warnings are given.</span></span> <span data-ttu-id="3f201-133">特定の行の注釈コンテキストが*無効*または*有効に*なっています。</span><span class="sxs-lookup"><span data-stu-id="3f201-133">The annotation context of a given line is either *disabled* or *enabled*.</span></span> <span data-ttu-id="3f201-134">指定された行の警告コンテキストが*無効に*なっ*ているか、有効になって*います。</span><span class="sxs-lookup"><span data-stu-id="3f201-134">The warning context of a given line is either *disabled* or *enabled*.</span></span>

<span data-ttu-id="3f201-135">どちらのコンテキストも、プロジェクトレベル (ソースコードのC#外) で指定することも、`#nullable` プリプロセッサディレクティブを使用してソースファイル内の任意の場所で指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="3f201-135">Both contexts can be specified at the project level (outside of C# source code), or anywhere within a source file via `#nullable` pre-processor directives.</span></span> <span data-ttu-id="3f201-136">プロジェクトレベルの設定が指定されていない場合、既定では、両方のコンテキストが*無効*になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-136">If no project level settings are provided the default is for both contexts to be *disabled*.</span></span>

<span data-ttu-id="3f201-137">`#nullable` ディレクティブは、ソーステキスト内の注釈と警告コンテキストを制御し、プロジェクトレベルの設定よりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-137">The `#nullable` directive controls the annotation and warning contexts within the source text, and take precedence over the project-level settings.</span></span>

<span data-ttu-id="3f201-138">ディレクティブは、後続のコード行に対して制御するコンテキストを、別のディレクティブがオーバーライドするまで、またはソースファイルの末尾まで設定します。</span><span class="sxs-lookup"><span data-stu-id="3f201-138">A directive sets the context(s) it controls for subsequent lines of code, until another directive overrides it, or until the end of the source file.</span></span>

<span data-ttu-id="3f201-139">ディレクティブの効果は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="3f201-139">The effect of the directives is as follows:</span></span>

- <span data-ttu-id="3f201-140">`#nullable disable`: null 許容の注釈と警告コンテキストを*無効*に設定します。</span><span class="sxs-lookup"><span data-stu-id="3f201-140">`#nullable disable`: Sets the nullable annotation and warning contexts to *disabled*</span></span>
- <span data-ttu-id="3f201-141">`#nullable enable`: null 許容の注釈と警告コンテキストを*有効*に設定します</span><span class="sxs-lookup"><span data-stu-id="3f201-141">`#nullable enable`: Sets the nullable annotation and warning contexts to *enabled*</span></span>
- <span data-ttu-id="3f201-142">`#nullable restore`: null 許容の注釈と警告コンテキストをプロジェクトの設定に復元します</span><span class="sxs-lookup"><span data-stu-id="3f201-142">`#nullable restore`: Restores the nullable annotation and warning contexts to project settings</span></span>
- <span data-ttu-id="3f201-143">`#nullable disable annotations`: null 許容の注釈コンテキストを*無効*に設定します</span><span class="sxs-lookup"><span data-stu-id="3f201-143">`#nullable disable annotations`: Sets the nullable annotation context to *disabled*</span></span>
- <span data-ttu-id="3f201-144">`#nullable enable annotations`: null 許容の注釈コンテキストを*有効*に設定します</span><span class="sxs-lookup"><span data-stu-id="3f201-144">`#nullable enable annotations`: Sets the nullable annotation context to *enabled*</span></span>
- <span data-ttu-id="3f201-145">`#nullable restore annotations`: null 許容の注釈コンテキストをプロジェクト設定に復元します</span><span class="sxs-lookup"><span data-stu-id="3f201-145">`#nullable restore annotations`: Restores the nullable annotation context to project settings</span></span>
- <span data-ttu-id="3f201-146">`#nullable disable warnings`: null 許容の警告コンテキストを*無効*に設定します</span><span class="sxs-lookup"><span data-stu-id="3f201-146">`#nullable disable warnings`: Sets the nullable warning context to *disabled*</span></span>
- <span data-ttu-id="3f201-147">`#nullable enable warnings`: null 許容の警告コンテキストを*有効*に設定します</span><span class="sxs-lookup"><span data-stu-id="3f201-147">`#nullable enable warnings`: Sets the nullable warning context to *enabled*</span></span>
- <span data-ttu-id="3f201-148">`#nullable restore warnings`: プロジェクト設定に null 許容の警告コンテキストを復元します</span><span class="sxs-lookup"><span data-stu-id="3f201-148">`#nullable restore warnings`: Restores the nullable warning context to project settings</span></span>

## <a name="nullability-of-types"></a><span data-ttu-id="3f201-149">型の null 値の許容</span><span class="sxs-lookup"><span data-stu-id="3f201-149">Nullability of types</span></span>

<span data-ttu-id="3f201-150">指定された型は、*無関係*、 *null 値*、 *nullable* 、 *unknown*の4つの nullabilities のいずれかを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="3f201-150">A given type can have one of four nullabilities: *Oblivious*, *nonnullable*, *nullable* and *unknown*.</span></span> 

<span data-ttu-id="3f201-151">潜在的な `null` 値が割り当てられている場合、 *null 値*および*不明な*型によって警告が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-151">*Nonnullable* and *unknown* types may cause warnings if a potential `null` value is assigned to them.</span></span> <span data-ttu-id="3f201-152">ただし、*無関係*と*null 許容*型は "*null 割り当て*可能" であり、警告なしで `null` 値を割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="3f201-152">*Oblivious* and *nullable* types, however, are "*null-assignable*" and can have `null` values assigned to them without warnings.</span></span> 

<span data-ttu-id="3f201-153">*無関係*および*null 値*型は、警告なしで逆参照または割り当てできます。</span><span class="sxs-lookup"><span data-stu-id="3f201-153">*Oblivious* and *nonnullable* types can be dereferenced or assigned without warnings.</span></span> <span data-ttu-id="3f201-154">ただし、null 値が*許容*される型と*不明な*型の値は "*null*を生成する" ものであり、適切な null チェックを行わずに逆参照または割り当てを行うと警告が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-154">Values of *nullable* and *unknown* types, however, are "*null-yielding*" and may cause warnings when dereferenced or assigned without proper null checking.</span></span> 

<span data-ttu-id="3f201-155">Null 値を生成する型の*既定の null 状態*は、"null になる可能性があります" です。</span><span class="sxs-lookup"><span data-stu-id="3f201-155">The *default null state* of a null-yielding type is "maybe null".</span></span> <span data-ttu-id="3f201-156">Null 以外の型の既定の null 状態は、"not null" です。</span><span class="sxs-lookup"><span data-stu-id="3f201-156">The default null state of a non-null-yielding type is "not null".</span></span>

<span data-ttu-id="3f201-157">型の種類と、それが発生する null 許容の注釈コンテキストは、null 値を許容するかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="3f201-157">The kind of type and the nullable annotation context it occurs in determine its nullability:</span></span>

- <span data-ttu-id="3f201-158">Null 値値型 `S` は常に*null 値*</span><span class="sxs-lookup"><span data-stu-id="3f201-158">A nonnullable value type `S` is always *nonnullable*</span></span>
- <span data-ttu-id="3f201-159">Null 許容型 `S?` は常に*null*値を許容します。</span><span class="sxs-lookup"><span data-stu-id="3f201-159">A nullable value type `S?` is always *nullable*</span></span>
- <span data-ttu-id="3f201-160">*無効*な注釈コンテキストで `C` 注釈が付けられていない参照型は、*無関係*です。</span><span class="sxs-lookup"><span data-stu-id="3f201-160">An unannotated reference type `C` in a *disabled* annotation context is *oblivious*</span></span>
- <span data-ttu-id="3f201-161">*有効な*注釈コンテキストで `C` 注釈が付けられていない参照型は、 *null 値*</span><span class="sxs-lookup"><span data-stu-id="3f201-161">An unannotated reference type `C` in an *enabled* annotation context is *nonnullable*</span></span>
- <span data-ttu-id="3f201-162">Null 許容の参照型 `C?` *null*値は許容されますが、*無効*な注釈コンテキストで警告が生成される可能性があります)</span><span class="sxs-lookup"><span data-stu-id="3f201-162">A nullable reference type `C?` is *nullable* (but a warning may be yielded in a *disabled* annotation context)</span></span>

<span data-ttu-id="3f201-163">さらに、型パラメーターによって、制約が考慮されるようになります。</span><span class="sxs-lookup"><span data-stu-id="3f201-163">Type parameters additionally take their constraints into account:</span></span>

- <span data-ttu-id="3f201-164">すべての制約 (存在する場合) が null を生成する型 (null 値が*許容*されるか*不明*) であるか、または `class?` 制約が*不明*である `T` 型パラメーター。</span><span class="sxs-lookup"><span data-stu-id="3f201-164">A type parameter `T` where all constraints (if any) are either null-yielding types (*nullable* and *unknown*) or the `class?` constraint is *unknown*</span></span>
- <span data-ttu-id="3f201-165">型パラメーター `T`、少なくとも1つの制約が*無関係*または*null 値*のいずれかであるか、またはいずれかの `struct` または `class` 制約がです。</span><span class="sxs-lookup"><span data-stu-id="3f201-165">A type parameter `T` where at least one constraint is either *oblivious* or *nonnullable* or one of the `struct` or `class` constraints is</span></span>
    - <span data-ttu-id="3f201-166">*無効*な注釈コンテキストでの*無関係*</span><span class="sxs-lookup"><span data-stu-id="3f201-166">*oblivious* in a *disabled* annotation context</span></span>
    - <span data-ttu-id="3f201-167">*有効な*注釈コンテキスト内の*null 値*</span><span class="sxs-lookup"><span data-stu-id="3f201-167">*nonnullable* in an *enabled* annotation context</span></span>
- <span data-ttu-id="3f201-168">Null 許容型パラメーター `T?` `T`の制約の少なくとも1つが*無関係*または*null 値*であるか、または `struct` または `class` の制約の1つがです。</span><span class="sxs-lookup"><span data-stu-id="3f201-168">A nullable type parameter `T?` where at least one of `T`'s constraints is *oblivious* or *nonnullable* or one of the `struct` or `class` constraints, is</span></span>
    - <span data-ttu-id="3f201-169">*無効*な注釈コンテキストでは null 値が*許容*されますが、警告が生成されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-169">*nullable* in a *disabled* annotation context (but a warning is yielded)</span></span>
    - <span data-ttu-id="3f201-170">*enabled*注釈コンテキストで null 値を*許容*</span><span class="sxs-lookup"><span data-stu-id="3f201-170">*nullable* in an *enabled* annotation context</span></span>

<span data-ttu-id="3f201-171">型パラメーター `T`の場合、`T?` は、`T` が値型であることがわかっているか、参照型であることがわかっている場合にのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-171">For a type parameter `T`, `T?` is only allowed if `T` is known to be a value type or known to be a reference type.</span></span>

### <a name="oblivious-vs-nonnullable"></a><span data-ttu-id="3f201-172">無関係 vs null 値</span><span class="sxs-lookup"><span data-stu-id="3f201-172">Oblivious vs nonnullable</span></span>

<span data-ttu-id="3f201-173">型の最後のトークンがそのコンテキスト内にある場合、`type` は特定の注釈コンテキストで発生すると見なされます。</span><span class="sxs-lookup"><span data-stu-id="3f201-173">A `type` is deemed to occur in a given annotation context when the last token of the type is within that context.</span></span>

<span data-ttu-id="3f201-174">ソースコードで `C` 特定の参照型が無関係または null 値として解釈されるかどうかは、そのソースコードの注釈コンテキストに依存します。</span><span class="sxs-lookup"><span data-stu-id="3f201-174">Whether a given reference type `C` in source code is interpreted as oblivious or nonnullable depends on the annotation context of that source code.</span></span> <span data-ttu-id="3f201-175">ただし、いったん確立されると、その型の一部と見なされ、"これを使用して" 移動します (ジェネリック型引数の置換中など)。</span><span class="sxs-lookup"><span data-stu-id="3f201-175">But once established, it is considered part of that type, and "travels with it" e.g. during substitution of generic type arguments.</span></span> <span data-ttu-id="3f201-176">型には `?` のような注釈がありますが、非表示です。</span><span class="sxs-lookup"><span data-stu-id="3f201-176">It is as if there is an annotation like `?` on the type, but invisible.</span></span>

## <a name="constraints"></a><span data-ttu-id="3f201-177">制約</span><span class="sxs-lookup"><span data-stu-id="3f201-177">Constraints</span></span>

<span data-ttu-id="3f201-178">Null 許容の参照型は、ジェネリック制約として使用できます。</span><span class="sxs-lookup"><span data-stu-id="3f201-178">Nullable reference types can be used as generic constraints.</span></span> <span data-ttu-id="3f201-179">さらに `object` が明示的な制約として有効になりました。</span><span class="sxs-lookup"><span data-stu-id="3f201-179">Furthermore `object` is now valid as an explicit constraint.</span></span> <span data-ttu-id="3f201-180">制約が存在しない場合は、(`object`ではなく) `object?` 制約に相当します `object` が、明示的な制約として `object?` は禁止されています。</span><span class="sxs-lookup"><span data-stu-id="3f201-180">Absence of a constraint is now equivalent to an `object?` constraint (instead of `object`), but (unlike `object` before) `object?` is not prohibited as an explicit constraint.</span></span>

<span data-ttu-id="3f201-181">`class?` は、"null 値が許容される可能性があります" という新しい制約で、`class` は "null 値 reference type" を表します。</span><span class="sxs-lookup"><span data-stu-id="3f201-181">`class?` is a new constraint denoting "possibly nullable reference type", whereas `class` denotes "nonnullable reference type".</span></span>

<span data-ttu-id="3f201-182">型引数または制約の null 値の許容は、型が制約を満たしているかどうかに影響しません。ただし、現在のケースが既に存在する場合を除きます (null 許容値型は `struct` 制約を満たしません)。</span><span class="sxs-lookup"><span data-stu-id="3f201-182">The nullability of a type argument or of a constraint does not impact whether the type satisfies the constraint, except where that is already the case today (nullable value types do not satisfy the `struct` constraint).</span></span> <span data-ttu-id="3f201-183">ただし、型引数が制約の null 値許容の要件を満たしていない場合は、警告が表示されることがあります。</span><span class="sxs-lookup"><span data-stu-id="3f201-183">However, if the type argument does not satisfy the nullability requirements of the constraint, a warning may be given.</span></span>

## <a name="null-state-and-null-tracking"></a><span data-ttu-id="3f201-184">Null 状態と null 追跡</span><span class="sxs-lookup"><span data-stu-id="3f201-184">Null state and null tracking</span></span>

<span data-ttu-id="3f201-185">特定のソースの場所にあるすべての式には*null 状態*があり、null として評価される可能性があるかどうかが示されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-185">Every expression in a given source location has a *null state*, which indicated whether it is believed to potentially evaluate to null.</span></span> <span data-ttu-id="3f201-186">Null 状態は、"not null" または "null の可能性があります" のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="3f201-186">The null state is either "not null" or "maybe null".</span></span> <span data-ttu-id="3f201-187">Null 状態は、null unsafe 変換と逆参照について警告を指定するかどうかを判断するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-187">The null state is used to determine whether a warning should be given about null-unsafe conversions and dereferences.</span></span>

### <a name="null-tracking-for-variables"></a><span data-ttu-id="3f201-188">変数の Null 追跡</span><span class="sxs-lookup"><span data-stu-id="3f201-188">Null tracking for variables</span></span>

<span data-ttu-id="3f201-189">変数またはプロパティを示す特定の式の場合、null 状態は、それらに対する割り当て、それらに対して実行されるテスト、およびそれらの間の制御フローに基づいて追跡されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-189">For certain expressions denoting variables or properties, the null state is tracked between occurrences, based on assignments to them, tests performed on them and the control flow between them.</span></span> <span data-ttu-id="3f201-190">これは、変数の代入を確実に追跡する方法に似ています。</span><span class="sxs-lookup"><span data-stu-id="3f201-190">This is similar to how definite assignment is tracked for variables.</span></span> <span data-ttu-id="3f201-191">追跡対象の式は、次の形式のものです。</span><span class="sxs-lookup"><span data-stu-id="3f201-191">The tracked expressions are the ones of the following form:</span></span>

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

<span data-ttu-id="3f201-192">ここで、識別子はフィールドまたはプロパティを表します。</span><span class="sxs-lookup"><span data-stu-id="3f201-192">Where the identifiers denote fields or properties.</span></span>

<span data-ttu-id="3f201-193">***明確な代入に似た null 状態遷移の説明***</span><span class="sxs-lookup"><span data-stu-id="3f201-193">***Describe null state transitions similar to definite assignment***</span></span>

### <a name="null-state-for-expressions"></a><span data-ttu-id="3f201-194">式の Null 状態</span><span class="sxs-lookup"><span data-stu-id="3f201-194">Null state for expressions</span></span>

<span data-ttu-id="3f201-195">式の null 状態は、その形式と型、およびそれに関連する変数の null 状態から派生します。</span><span class="sxs-lookup"><span data-stu-id="3f201-195">The null state of an expression is derived from its form and type, and from the null state of variables involved in it.</span></span>

### <a name="literals"></a><span data-ttu-id="3f201-196">リテラル</span><span class="sxs-lookup"><span data-stu-id="3f201-196">Literals</span></span>

<span data-ttu-id="3f201-197">`null` リテラルの null 状態は、"null になる可能性があります" です。</span><span class="sxs-lookup"><span data-stu-id="3f201-197">The null state of a `null` literal is "maybe null".</span></span> <span data-ttu-id="3f201-198">Null 値値型ではないことがわかっている型に変換される `default` リテラルの null 状態は、"null になる可能性があります" です。</span><span class="sxs-lookup"><span data-stu-id="3f201-198">The null state of a `default` literal that is being converted to a type that is known not to be a nonnullable value type is "maybe null".</span></span> <span data-ttu-id="3f201-199">他のリテラルの null 状態は、"not null" です。</span><span class="sxs-lookup"><span data-stu-id="3f201-199">The null state of any other literal is "not null".</span></span>

### <a name="simple-names"></a><span data-ttu-id="3f201-200">簡易名</span><span class="sxs-lookup"><span data-stu-id="3f201-200">Simple names</span></span>

<span data-ttu-id="3f201-201">`simple_name` が値として分類されていない場合、その null 状態は "not null" になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-201">If a `simple_name` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="3f201-202">それ以外の場合は追跡対象の式であり、null 状態はこのソースの場所で追跡された null 状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-202">Otherwise it is a tracked expression, and its null state is its tracked null state at this source location.</span></span>

### <a name="member-access"></a><span data-ttu-id="3f201-203">メンバー アクセス。</span><span class="sxs-lookup"><span data-stu-id="3f201-203">Member access</span></span>

<span data-ttu-id="3f201-204">`member_access` が値として分類されていない場合、その null 状態は "not null" になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-204">If a `member_access` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="3f201-205">それ以外の場合、追跡対象の式の場合、null 状態はこのソースの場所で追跡された null 状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-205">Otherwise, if it is a tracked expression, its null state is its tracked null state at this source location.</span></span> <span data-ttu-id="3f201-206">それ以外の場合、null 状態はその型の既定の null 状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-206">Otherwise its null state is the default null state of its type.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="3f201-207">Invocation 式</span><span class="sxs-lookup"><span data-stu-id="3f201-207">Invocation expressions</span></span>

<span data-ttu-id="3f201-208">特殊な null 動作に対して1つ以上の属性を使用して宣言されたメンバーを `invocation_expression` が呼び出す場合、null 状態はそれらの属性によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-208">If an `invocation_expression` invokes a member that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="3f201-209">それ以外の場合、式の null 状態はその型の既定の null 状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-209">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="element-access"></a><span data-ttu-id="3f201-210">要素アクセス</span><span class="sxs-lookup"><span data-stu-id="3f201-210">Element access</span></span>

<span data-ttu-id="3f201-211">特殊な null 動作に対して1つ以上の属性を使用して宣言されたインデクサーを `element_access` が呼び出す場合、null 状態はそれらの属性によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-211">If an `element_access` invokes an indexer that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="3f201-212">それ以外の場合、式の null 状態はその型の既定の null 状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-212">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="base-access"></a><span data-ttu-id="3f201-213">基本アクセス</span><span class="sxs-lookup"><span data-stu-id="3f201-213">Base access</span></span>

<span data-ttu-id="3f201-214">`B` が外側の型の基本型を表している場合、`base.I` は `((B)this).I` と同じ null 状態であり、`base[E]` は `((B)this)[E]`と同じ null 状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-214">If `B` denotes the base type of the enclosing type, `base.I` has the same null state as `((B)this).I` and `base[E]` has the same null state as `((B)this)[E]`.</span></span>

### <a name="default-expressions"></a><span data-ttu-id="3f201-215">既定式</span><span class="sxs-lookup"><span data-stu-id="3f201-215">Default expressions</span></span>

<span data-ttu-id="3f201-216">`T` が null 値値型であることがわかっている場合、`default(T)` は null 以外の状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-216">`default(T)` has the null state "non-null" if `T` is known to be a nonnullable value type.</span></span> <span data-ttu-id="3f201-217">それ以外の場合、null 状態は "null の可能性があります" になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-217">Otherwise it has the null state "maybe null".</span></span>

### <a name="null-conditional-expressions"></a><span data-ttu-id="3f201-218">Null 条件式</span><span class="sxs-lookup"><span data-stu-id="3f201-218">Null-conditional expressions</span></span>

<span data-ttu-id="3f201-219">`null_conditional_expression` の状態が null である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-219">A `null_conditional_expression` has the null state "maybe null".</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="3f201-220">キャスト式</span><span class="sxs-lookup"><span data-stu-id="3f201-220">Cast expressions</span></span>

<span data-ttu-id="3f201-221">キャスト式 `(T)E` がユーザー定義の変換を呼び出す場合、式の null 状態はその型の既定の null 状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-221">If a cast expression `(T)E` invokes a user-defined conversion, then the null state of the expression is the default null state for its type.</span></span> <span data-ttu-id="3f201-222">それ以外の場合、`T` が null (*null*値が許容または*不明*) の場合、null 状態は "null になる可能性があります" になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-222">Otherwise, if `T` is null-yielding (*nullable* or *unknown*) then the null state is "maybe null".</span></span> <span data-ttu-id="3f201-223">それ以外の場合、null 状態は `E`の null 状態と同じになります。</span><span class="sxs-lookup"><span data-stu-id="3f201-223">Otherwise the null state is the same as the null state of `E`.</span></span>

### <a name="await-expressions"></a><span data-ttu-id="3f201-224">Await 式</span><span class="sxs-lookup"><span data-stu-id="3f201-224">Await expressions</span></span>

<span data-ttu-id="3f201-225">`await E` の null 状態は、その型の既定の null 状態です。</span><span class="sxs-lookup"><span data-stu-id="3f201-225">The null state of `await E` is the default null state of its type.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="3f201-226">`as` 演算子</span><span class="sxs-lookup"><span data-stu-id="3f201-226">The `as` operator</span></span>

<span data-ttu-id="3f201-227">`as` 式の状態が null である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-227">An `as` expression has the null state "maybe null".</span></span>

### <a name="the-null-coalescing-operator"></a><span data-ttu-id="3f201-228">Null 合体演算子</span><span class="sxs-lookup"><span data-stu-id="3f201-228">The null-coalescing operator</span></span>

<span data-ttu-id="3f201-229">`E1 ?? E2` には、と同じ null 状態があり `E2`</span><span class="sxs-lookup"><span data-stu-id="3f201-229">`E1 ?? E2` has the same null state as `E2`</span></span>

### <a name="the-conditional-operator"></a><span data-ttu-id="3f201-230">条件演算子</span><span class="sxs-lookup"><span data-stu-id="3f201-230">The conditional operator</span></span>

<span data-ttu-id="3f201-231">`E2` と `E3` の両方の null 状態が "not null" の場合、`E1 ? E2 : E3` の null 状態は "not null" になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-231">The null state of `E1 ? E2 : E3` is "not null" if the null state of both `E2` and `E3` are "not null".</span></span> <span data-ttu-id="3f201-232">それ以外の場合は、"null の可能性があります" になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-232">Otherwise it is "maybe null".</span></span>

### <a name="query-expressions"></a><span data-ttu-id="3f201-233">クエリ式</span><span class="sxs-lookup"><span data-stu-id="3f201-233">Query expressions</span></span>

<span data-ttu-id="3f201-234">クエリ式の null 状態は、その型の既定の null 状態です。</span><span class="sxs-lookup"><span data-stu-id="3f201-234">The null state of a query expression is the default null state of its type.</span></span>

### <a name="assignment-operators"></a><span data-ttu-id="3f201-235">代入演算子</span><span class="sxs-lookup"><span data-stu-id="3f201-235">Assignment operators</span></span>

<span data-ttu-id="3f201-236">暗黙の変換が適用された後、`E1 = E2` と `E1 op= E2` は `E2` と同じ null 状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-236">`E1 = E2` and `E1 op= E2` have the same null state as `E2` after any implicit conversions have been applied.</span></span>

### <a name="unary-and-binary-operators"></a><span data-ttu-id="3f201-237">単項演算子と二項演算子</span><span class="sxs-lookup"><span data-stu-id="3f201-237">Unary and binary operators</span></span>

<span data-ttu-id="3f201-238">単項演算子または二項演算子が、特殊な null 動作で1つ以上の属性を使用して宣言されたユーザー定義演算子を呼び出す場合、null 状態はこれらの属性によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-238">If a unary or binary operator invokes an user-defined operator that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="3f201-239">それ以外の場合、式の null 状態はその型の既定の null 状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-239">Otherwise the null state of the expression is the default null state of its type.</span></span>

<span data-ttu-id="3f201-240">***文字列とデリゲートに対するバイナリ `+` に特別なことはありますか。***</span><span class="sxs-lookup"><span data-stu-id="3f201-240">***Something special to do for binary `+` over strings and delegates?***</span></span>

### <a name="expressions-that-propagate-null-state"></a><span data-ttu-id="3f201-241">Null 状態を反映する式</span><span class="sxs-lookup"><span data-stu-id="3f201-241">Expressions that propagate null state</span></span>

<span data-ttu-id="3f201-242">`(E)`、`checked(E)`、および `unchecked(E)` はすべて `E`と同じ null 状態になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-242">`(E)`, `checked(E)` and `unchecked(E)` all have the same null state as `E`.</span></span>

### <a name="expressions-that-are-never-null"></a><span data-ttu-id="3f201-243">Null にならない式</span><span class="sxs-lookup"><span data-stu-id="3f201-243">Expressions that are never null</span></span>

<span data-ttu-id="3f201-244">次の式形式の null 状態は常に "not null" です。</span><span class="sxs-lookup"><span data-stu-id="3f201-244">The null state of the following expression forms is always "not null":</span></span>

- <span data-ttu-id="3f201-245">`this` アクセス</span><span class="sxs-lookup"><span data-stu-id="3f201-245">`this` access</span></span>
- <span data-ttu-id="3f201-246">挿入文字列</span><span class="sxs-lookup"><span data-stu-id="3f201-246">interpolated strings</span></span>
- <span data-ttu-id="3f201-247">`new` 式 (オブジェクト、デリゲート、匿名オブジェクト、および配列作成式)</span><span class="sxs-lookup"><span data-stu-id="3f201-247">`new` expressions (object, delegate, anonymous object and array creation expressions)</span></span>
- <span data-ttu-id="3f201-248">`typeof` 式</span><span class="sxs-lookup"><span data-stu-id="3f201-248">`typeof` expressions</span></span>
- <span data-ttu-id="3f201-249">`nameof` 式</span><span class="sxs-lookup"><span data-stu-id="3f201-249">`nameof` expressions</span></span>
- <span data-ttu-id="3f201-250">匿名関数 (匿名メソッドとラムダ式)</span><span class="sxs-lookup"><span data-stu-id="3f201-250">anonymous functions (anonymous methods and lambda expressions)</span></span>
- <span data-ttu-id="3f201-251">null-寛容な式</span><span class="sxs-lookup"><span data-stu-id="3f201-251">null-forgiving expressions</span></span>
- <span data-ttu-id="3f201-252">`is` 式</span><span class="sxs-lookup"><span data-stu-id="3f201-252">`is` expressions</span></span>

## <a name="type-inference"></a><span data-ttu-id="3f201-253">型の推論</span><span class="sxs-lookup"><span data-stu-id="3f201-253">Type inference</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="3f201-254">`var` の型の推論</span><span class="sxs-lookup"><span data-stu-id="3f201-254">Type inference for `var`</span></span>

<span data-ttu-id="3f201-255">`var` で宣言されたローカル変数に対して推論される型は、初期化式の null 状態によって通知されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-255">The type inferred for local variables declared with `var` is informed by the null state of the initializing expression.</span></span>

```csharp
var x = E;
```

<span data-ttu-id="3f201-256">`E` の型が null 許容の参照型で `C?`、`E` の null 状態が "not null" の場合、`x` に対して推論される型は `C`になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-256">If the type of `E` is a nullable reference type `C?` and the null state of `E` is "not null" then the type inferred for `x` is `C`.</span></span> <span data-ttu-id="3f201-257">それ以外の場合、推論された型は `E`の型になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-257">Otherwise, the inferred type is the type of `E`.</span></span>

<span data-ttu-id="3f201-258">`x` に対して推論される型の null 値の許容属性は、型がその位置に明示的に指定されているかのように、前に説明したように、`var`の注釈コンテキストに基づいて決定されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-258">The nullability of the type inferred for `x` is determined as described above, based on the annotation context of the `var`, just as if the type had been given explicitly in that position.</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="3f201-259">`var?` の型の推論</span><span class="sxs-lookup"><span data-stu-id="3f201-259">Type inference for `var?`</span></span>

<span data-ttu-id="3f201-260">`var?` で宣言されたローカル変数に対して推論される型は、初期化式の null 状態には依存しません。</span><span class="sxs-lookup"><span data-stu-id="3f201-260">The type inferred for local variables declared with `var?` is independent of the null state of the initializing expression.</span></span>

```csharp
var? x = E;
```

<span data-ttu-id="3f201-261">`E` の型 `T` が null 許容値型または null 許容の参照型である場合、`x` に対して推論される型は `T`です。</span><span class="sxs-lookup"><span data-stu-id="3f201-261">If the type `T` of `E` is a nullable value type or a nullable reference type then the type inferred for `x` is `T`.</span></span> <span data-ttu-id="3f201-262">それ以外の場合、`T` が null 値値型である場合 `S` 推論される型は `S?`です。</span><span class="sxs-lookup"><span data-stu-id="3f201-262">Otherwise, if `T` is a nonnullable value type `S` the type inferred is `S?`.</span></span> <span data-ttu-id="3f201-263">それ以外の場合、`T` が null 値参照型である場合 `C` 推論される型は `C?`です。</span><span class="sxs-lookup"><span data-stu-id="3f201-263">Otherwise, if `T` is a nonnullable reference type `C` the type inferred is `C?`.</span></span> <span data-ttu-id="3f201-264">それ以外の場合、宣言は無効です。</span><span class="sxs-lookup"><span data-stu-id="3f201-264">Otherwise, the declaration is illegal.</span></span>

<span data-ttu-id="3f201-265">`x` に対して推論される型の null 値の許容属性は、常に null 値を*許容*します。</span><span class="sxs-lookup"><span data-stu-id="3f201-265">The nullability of the type inferred for `x` is always *nullable*.</span></span>

### <a name="generic-type-inference"></a><span data-ttu-id="3f201-266">ジェネリック型の推論</span><span class="sxs-lookup"><span data-stu-id="3f201-266">Generic type inference</span></span>

<span data-ttu-id="3f201-267">ジェネリック型の推論は、推論された参照型が null 許容かどうかを判断するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="3f201-267">Generic type inference is enhanced to help decide whether inferred reference types should be nullable or not.</span></span> <span data-ttu-id="3f201-268">これはベストエフォートであり、ではなく、それ自体が警告を生成することはありませんが、選択されたオーバーロードの推定型が引数に適用されると、null 許容の警告が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-268">This is a best effort, and does not in and of itself yield warnings, but may lead to nullable warnings when the inferred types of the selected overload are applied to the arguments.</span></span>

<span data-ttu-id="3f201-269">型推論は、受信型の注釈コンテキストに依存しません。</span><span class="sxs-lookup"><span data-stu-id="3f201-269">The type inference does not rely on the annotation context of incoming types.</span></span> <span data-ttu-id="3f201-270">代わりに、明示的に表現されている場合は、その独自の注釈コンテキストを取得する `type` が推論されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-270">Instead a `type` is inferred which acquires its own annotation context from where it "would have been" if it had been expressed explicitly.</span></span> <span data-ttu-id="3f201-271">これにより、独自に作成したものの便宜を目的として、型の推定の役割が決まります。</span><span class="sxs-lookup"><span data-stu-id="3f201-271">This underscores the role of type inference as a convenience for what you could have written yourself.</span></span>

<span data-ttu-id="3f201-272">より正確に言えば、推論された型引数の注釈のコンテキストは、`<...>` 型パラメーターリストが後に存在する可能性のあるトークンのコンテキストになります。つまり、呼び出されるジェネリックメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="3f201-272">More precisely, the annotation context for an inferred type argument is the context of the token that would have been followed by the `<...>` type parameter list, had there been one; i.e. the name of the generic method being called.</span></span> <span data-ttu-id="3f201-273">このような呼び出しに変換されるクエリ式の場合、コンテキストは、呼び出しの生成元であるクエリ句の最初のコンテキストキーワードから取得されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-273">For query expressions that translate to such calls, the context is taken from the initial contextual keyword of the query clause from which the call is generated.</span></span>

### <a name="the-first-phase"></a><span data-ttu-id="3f201-274">最初のフェーズ</span><span class="sxs-lookup"><span data-stu-id="3f201-274">The first phase</span></span>

<span data-ttu-id="3f201-275">Null 許容の参照型は、以下で説明するように、初期式からの境界にフローします。</span><span class="sxs-lookup"><span data-stu-id="3f201-275">Nullable reference types flow into the bounds from the initial expressions, as described below.</span></span> <span data-ttu-id="3f201-276">さらに、2つの新しい種類の境界 (`null` と `default` が導入されています。</span><span class="sxs-lookup"><span data-stu-id="3f201-276">In addition, two new kinds of bounds, namely `null` and `default` are introduced.</span></span> <span data-ttu-id="3f201-277">その目的は、入力式で `null` または `default` を実行することです。これにより、推論された型は、そうでない場合でも null 許容になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-277">Their purpose is to carry through occurrences of `null` or `default` in the input expressions, which may cause an inferred type to be nullable, even when it otherwise wouldn't.</span></span> <span data-ttu-id="3f201-278">これは、推論プロセスで "nullness" を取得するように強化された null 許容*値*型に対しても機能します。</span><span class="sxs-lookup"><span data-stu-id="3f201-278">This works even for nullable *value* types, which are enhanced to pick up "nullness" in the inference process.</span></span>

<span data-ttu-id="3f201-279">最初のフェーズで追加する境界の決定は、次のように強化されています。</span><span class="sxs-lookup"><span data-stu-id="3f201-279">The determination of what bounds to add in the first phase are enhanced as follows:</span></span>

<span data-ttu-id="3f201-280">引数 `Ei` に参照型がある場合、推論に使用される型 `U` は、`Ei` の null 状態と宣言された型によって異なります。</span><span class="sxs-lookup"><span data-stu-id="3f201-280">If an argument `Ei` has a reference type, the type `U` used for inference depends on the null state of `Ei` as well as its declared type:</span></span>
- <span data-ttu-id="3f201-281">宣言された型が null 値参照型 `U0` または null 許容の参照型の場合 `U0?`</span><span class="sxs-lookup"><span data-stu-id="3f201-281">If the declared type is a nonnullable reference type `U0` or a nullable reference type `U0?` then</span></span>
    - <span data-ttu-id="3f201-282">`Ei` の null 状態が "not null" の場合、`U` は `U0`</span><span class="sxs-lookup"><span data-stu-id="3f201-282">if the null state of `Ei` is "not null" then `U` is `U0`</span></span>
    - <span data-ttu-id="3f201-283">`Ei` の null 状態が "おそらく null" の場合、`U` は `U0?`</span><span class="sxs-lookup"><span data-stu-id="3f201-283">if the null state of `Ei` is "maybe null" then `U` is `U0?`</span></span>
- <span data-ttu-id="3f201-284">それ以外の場合 `Ei` が宣言された型である場合、`U` はその型になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-284">Otherwise if `Ei` has a declared type, `U` is that type</span></span>
- <span data-ttu-id="3f201-285">それ以外の場合、`Ei` が `null` 場合、`U` は特殊なバインド `null`</span><span class="sxs-lookup"><span data-stu-id="3f201-285">Otherwise if `Ei` is `null` then `U` is the special bound `null`</span></span>
- <span data-ttu-id="3f201-286">それ以外の場合、`Ei` が `default` 場合、`U` は特殊なバインド `default`</span><span class="sxs-lookup"><span data-stu-id="3f201-286">Otherwise if `Ei` is `default` then `U` is the special bound `default`</span></span>
- <span data-ttu-id="3f201-287">それ以外の場合、推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="3f201-287">Otherwise no inference is made.</span></span>

### <a name="exact-upper-bound-and-lower-bound-inferences"></a><span data-ttu-id="3f201-288">正確、上限、下限の推論</span><span class="sxs-lookup"><span data-stu-id="3f201-288">Exact, upper-bound and lower-bound inferences</span></span>

<span data-ttu-id="3f201-289">`U` 型*から*型 `V`*へ*の推論では、`V` が null 許容の参照型 `V0?`である場合、次の句では `V0` ではなく `V` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-289">In inferences *from* the type `U` *to* the type `V`, if `V` is a nullable reference type `V0?`, then `V0` is used instead of `V` in the following clauses.</span></span>
- <span data-ttu-id="3f201-290">`V` が固定されていない型の変数の1つである場合、`U` は、のように正確な値、上限、または下限として追加されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-290">If `V` is one of the unfixed type variables, `U` is added as an exact, upper or lower bound as before</span></span>
- <span data-ttu-id="3f201-291">それ以外の場合、`U` が `null` または `default`の場合、推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="3f201-291">Otherwise, if `U` is `null` or `default`, no inference is made</span></span>
- <span data-ttu-id="3f201-292">それ以外の場合、`U` が null 許容の参照型 `U0?`である場合は、後続の句で `U` の代わりに `U0` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-292">Otherwise, if `U` is a nullable reference type `U0?`, then `U0` is used instead of `U` in the subsequent clauses.</span></span>

<span data-ttu-id="3f201-293">基本的に、いずれかの固定されていない型の変数に直接関連する null 値の許容属性は、その境界に保持されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-293">The essence is that nullability that pertains directly to one of the unfixed type variables is preserved into its bounds.</span></span> <span data-ttu-id="3f201-294">逆に、ソースとターゲットの種類に再帰的に再帰する推論の場合、null 値の許容は無視されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-294">For the inferences that recurse further into the source and target types, on the other hand, nullability is ignored.</span></span> <span data-ttu-id="3f201-295">これは、一致しない場合と一致しない場合がありますが、それ以外の場合は、オーバーロードが選択されて適用されると、後で警告が発行されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-295">It may or may not match, but if it doesn't, a warning will be issued later if the overload is chosen and applied.</span></span>

### <a name="fixing"></a><span data-ttu-id="3f201-296">写真の</span><span class="sxs-lookup"><span data-stu-id="3f201-296">Fixing</span></span>

<span data-ttu-id="3f201-297">現在、この仕様では、複数の境界が相互に変換可能な id であるが異なる場合に何が起こるかを説明するのは適切ではありません。</span><span class="sxs-lookup"><span data-stu-id="3f201-297">The spec currently does not do a good job of describing what happens when multiple bounds are identity convertible to each other, but are different.</span></span> <span data-ttu-id="3f201-298">これは、`object` と `dynamic`の間、要素名だけが異なるタプル型の間で、その型の間、および参照型の `C` と `C?` の間でも発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-298">This may happen between `object` and `dynamic`, between tuple types that differ only in element names, between types constructed thereof and now also between `C` and `C?` for reference types.</span></span>

<span data-ttu-id="3f201-299">さらに、入力式から結果の型に "nullness" を伝達する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3f201-299">In addition we need to propagate "nullness" from the input expressions to the result type.</span></span> 

<span data-ttu-id="3f201-300">これらの処理を行うには、修正するフェーズを追加します。これは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3f201-300">To handle these we add more phases to fixing, which is now:</span></span>

1. <span data-ttu-id="3f201-301">すべての境界内のすべての型を候補として収集し、null 値を許容する参照型であるすべてから `?` を削除します。</span><span class="sxs-lookup"><span data-stu-id="3f201-301">Gather all the types in all the bounds as candidates, removing `?` from all that are nullable reference types</span></span>
2. <span data-ttu-id="3f201-302">正確、下限、上限の要件に基づいて候補を排除する (`null` と `default` の範囲を維持する)</span><span class="sxs-lookup"><span data-stu-id="3f201-302">Eliminate candidates based on requirements of exact, lower and upper bounds (keeping `null` and `default` bounds)</span></span>
3. <span data-ttu-id="3f201-303">他のすべての候補に暗黙的に変換されない候補を除外する</span><span class="sxs-lookup"><span data-stu-id="3f201-303">Eliminate candidates that do not have an implicit conversion to all the other candidates</span></span>
4. <span data-ttu-id="3f201-304">残りの候補がすべての id を相互に変換していない場合、型の推定は失敗します。</span><span class="sxs-lookup"><span data-stu-id="3f201-304">If the remaining candidates do not all have identity conversions to one another, then type inference fails</span></span>
5. <span data-ttu-id="3f201-305">次に示すように、残りの候補を*マージ*します。</span><span class="sxs-lookup"><span data-stu-id="3f201-305">*Merge* the remaining candidates as described below</span></span>
6. <span data-ttu-id="3f201-306">結果として得られる候補が参照型または null 値値型であり、*すべて*の完全な*境界または下限の境界*が null 許容の値型、null 許容の参照型、`null` または `default`である場合、`?` が結果の候補に追加され、null 許容値型または参照型になります。</span><span class="sxs-lookup"><span data-stu-id="3f201-306">If the resulting candidate is a reference type or a nonnullable value type and *all* of the exact bounds or *any* of the lower bounds are nullable value types, nullable reference types, `null` or `default`, then `?` is added to the resulting candidate, making it a nullable value type or reference type.</span></span>

<span data-ttu-id="3f201-307">*マージ*は、2つの候補の種類の間で記述されます。</span><span class="sxs-lookup"><span data-stu-id="3f201-307">*Merging* is described between two candidate types.</span></span> <span data-ttu-id="3f201-308">これは推移的なものであり、同じ最終的な結果を含む任意の順序で候補をマージできます。</span><span class="sxs-lookup"><span data-stu-id="3f201-308">It is transitive and commutative, so the candidates can be merged in any order with the same ultimate result.</span></span> <span data-ttu-id="3f201-309">2つの候補の型が相互に変換可能な id ではない場合、これは未定義です。</span><span class="sxs-lookup"><span data-stu-id="3f201-309">It is undefined if the two candidate types are not identity convertible to each other.</span></span>

<span data-ttu-id="3f201-310">*Merge*関数は、次の2つの候補の型と方向 ( *+* または *-* ) を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="3f201-310">The *Merge* function takes two candidate types and a direction (*+* or *-*):</span></span>

- <span data-ttu-id="3f201-311">*Merge*(`T`、`T`、 *d*) = t</span><span class="sxs-lookup"><span data-stu-id="3f201-311">*Merge*(`T`, `T`, *d*) = T</span></span>
- <span data-ttu-id="3f201-312">*Merge (* `S`、`T?`、 *+* *) = merge (`S?`* 、`T`、 *+* ) = *merge*(`S`、`T`、 *+* )`?`</span><span class="sxs-lookup"><span data-stu-id="3f201-312">*Merge*(`S`, `T?`, *+*) = *Merge*(`S?`, `T`, *+*) = *Merge*(`S`, `T`, *+*)`?`</span></span>
- <span data-ttu-id="3f201-313">*Merge (* `S`、`T?`、 *-* *) = merge (`S?`* 、`T`、 *-* ) = *merge*(`S`、`T`、 *-* )</span><span class="sxs-lookup"><span data-stu-id="3f201-313">*Merge*(`S`, `T?`, *-*) = *Merge*(`S?`, `T`, *-*) = *Merge*(`S`, `T`, *-*)</span></span>
- <span data-ttu-id="3f201-314">*Merge (* `C<S1,...,Sn>`、`C<T1,...,Tn>`、 *+* *) `C<`= merge (* `S1`、`T1`、 *d1*)`,...,`*merge*(`Sn`、`Tn`、 *dn*)`>`、 *where*</span><span class="sxs-lookup"><span data-stu-id="3f201-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="3f201-315">`C<...>` の `i`' th type パラメーターが共変である場合に *+* を `di` = </span><span class="sxs-lookup"><span data-stu-id="3f201-315">`di` = *+* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="3f201-316">`C<...>` の `i`' th type パラメーターが負またはインバリアントの場合は、`di` =  *-* 。</span><span class="sxs-lookup"><span data-stu-id="3f201-316">`di` = *-* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="3f201-317">*Merge (* `C<S1,...,Sn>`、`C<T1,...,Tn>`、 *-* *) `C<`= merge (* `S1`、`T1`、 *d1*)`,...,`*merge*(`Sn`、`Tn`、 *dn*)`>`、 *where*</span><span class="sxs-lookup"><span data-stu-id="3f201-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="3f201-318">`C<...>` の `i`' th type パラメーターが共変である場合に *-* を `di` = </span><span class="sxs-lookup"><span data-stu-id="3f201-318">`di` = *-* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="3f201-319">`C<...>` の `i`' th type パラメーターが負またはインバリアントの場合は、`di` =  *+* 。</span><span class="sxs-lookup"><span data-stu-id="3f201-319">`di` = *+* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="3f201-320">*Merge (* `(S1 s1,..., Sn sn)`、`(T1 t1,..., Tn tn)`、 *d* *) `(`= merge (* `S1`、`T1`、 *d)* `n1,...,`*merge*(`Sn`、 *`Tn`、d*) `nn)`、 *where*</span><span class="sxs-lookup"><span data-stu-id="3f201-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*Merge*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *where*</span></span>
    - <span data-ttu-id="3f201-321">`si` と `ti` が異なる場合、または両方が存在しない場合、`ni` は存在しません。</span><span class="sxs-lookup"><span data-stu-id="3f201-321">`ni` is absent if `si` and `ti` differ, or if both are absent</span></span>
    - <span data-ttu-id="3f201-322">`si` と `ti` が同じ場合、`ni` は `si`</span><span class="sxs-lookup"><span data-stu-id="3f201-322">`ni` is `si` if `si` and `ti` are the same</span></span>
- <span data-ttu-id="3f201-323">*Merge*(`object`, `dynamic`) = *merge*(`dynamic`, `object`) = `dynamic`</span><span class="sxs-lookup"><span data-stu-id="3f201-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span></span>

## <a name="warnings"></a><span data-ttu-id="3f201-324">警告</span><span class="sxs-lookup"><span data-stu-id="3f201-324">Warnings</span></span>

### <a name="potential-null-assignment"></a><span data-ttu-id="3f201-325">可能性のある null 代入</span><span class="sxs-lookup"><span data-stu-id="3f201-325">Potential null assignment</span></span>

### <a name="potential-null-dereference"></a><span data-ttu-id="3f201-326">可能性のある null の逆参照</span><span class="sxs-lookup"><span data-stu-id="3f201-326">Potential null dereference</span></span>

### <a name="constraint-nullability-mismatch"></a><span data-ttu-id="3f201-327">制約の null 値の許容が一致しません</span><span class="sxs-lookup"><span data-stu-id="3f201-327">Constraint nullability mismatch</span></span>

### <a name="nullable-types-in-disabled-annotation-context"></a><span data-ttu-id="3f201-328">無効な注釈コンテキスト内の null 許容型</span><span class="sxs-lookup"><span data-stu-id="3f201-328">Nullable types in disabled annotation context</span></span>

## <a name="attributes-for-special-null-behavior"></a><span data-ttu-id="3f201-329">特殊な null 動作の属性</span><span class="sxs-lookup"><span data-stu-id="3f201-329">Attributes for special null behavior</span></span>


