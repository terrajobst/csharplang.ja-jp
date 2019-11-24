---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704087"
---
# <a name="expressions"></a><span data-ttu-id="a3946-101">式</span><span class="sxs-lookup"><span data-stu-id="a3946-101">Expressions</span></span>

<span data-ttu-id="a3946-102">式は一連の演算子とオペランドで構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="a3946-103">この章では、構文、オペランドと演算子の評価順序、および式の意味を定義します。</span><span class="sxs-lookup"><span data-stu-id="a3946-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="a3946-104">式の分類</span><span class="sxs-lookup"><span data-stu-id="a3946-104">Expression classifications</span></span>

<span data-ttu-id="a3946-105">式は、次のいずれかに分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="a3946-106">値。</span><span class="sxs-lookup"><span data-stu-id="a3946-106">A value.</span></span> <span data-ttu-id="a3946-107">すべての値には、型が関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="a3946-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="a3946-108">変数。</span><span class="sxs-lookup"><span data-stu-id="a3946-108">A variable.</span></span> <span data-ttu-id="a3946-109">すべての変数には、関連付けられた型、つまり変数の宣言型があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="a3946-110">名前空間。</span><span class="sxs-lookup"><span data-stu-id="a3946-110">A namespace.</span></span> <span data-ttu-id="a3946-111">この分類の式は、 *member_access* ([メンバーアクセス](expressions.md#member-access)) の左側としてのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="a3946-112">その他のコンテキストでは、名前空間として分類された式によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="a3946-113">型。</span><span class="sxs-lookup"><span data-stu-id="a3946-113">A type.</span></span> <span data-ttu-id="a3946-114">この分類の式は、 *member_access* ([メンバーアクセス](expressions.md#member-access)) の左側として、または `as` 演算子 ([as 演算子](expressions.md#the-as-operator))、`is` 演算子 ([is 演算子](expressions.md#the-is-operator))、または `typeof` 演算子 ([typeof 演算子](expressions.md#the-typeof-operator)) のオペランドとしてのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="a3946-115">その他のコンテキストでは、型として分類された式によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="a3946-116">メソッドグループ。メンバー参照 ([メンバー参照](expressions.md#member-lookup)) の結果として得られる一連のオーバーロードされたメソッドです。</span><span class="sxs-lookup"><span data-stu-id="a3946-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="a3946-117">メソッドグループには、関連付けられたインスタンス式と、関連する型引数リストを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="a3946-118">インスタンスメソッドが呼び出されると、インスタンス式を評価した結果が `this` ([このアクセス](expressions.md#this-access)) によって表されるインスタンスになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="a3946-119">メソッドグループは、 *invocation_expression* ([呼び出し式](expressions.md#invocation-expressions))、 *delegate_creation_expression* ([デリゲート作成式](expressions.md#delegate-creation-expressions))、および is 演算子の左辺として許可され、互換性のあるデリゲート型 ([メソッドグループ変換](conversions.md#method-group-conversions)) に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="a3946-120">その他のコンテキストでは、メソッドグループとして分類された式によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="a3946-121">Null リテラル。</span><span class="sxs-lookup"><span data-stu-id="a3946-121">A null literal.</span></span> <span data-ttu-id="a3946-122">この分類を持つ式は、参照型または null 許容型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="a3946-123">匿名関数。</span><span class="sxs-lookup"><span data-stu-id="a3946-123">An anonymous function.</span></span> <span data-ttu-id="a3946-124">この分類の式は、互換性のあるデリゲート型または式ツリー型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="a3946-125">プロパティアクセス。</span><span class="sxs-lookup"><span data-stu-id="a3946-125">A property access.</span></span> <span data-ttu-id="a3946-126">すべてのプロパティアクセスには、関連付けられた型、つまりプロパティの型があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="a3946-127">さらに、プロパティアクセスには、関連付けられたインスタンス式が含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="a3946-128">インスタンスプロパティアクセスのアクセサー (`get` または `set` ブロック) が呼び出されると、インスタンス式を評価した結果が、`this` ([このアクセス](expressions.md#this-access)) によって表されるインスタンスになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="a3946-129">イベントアクセス。</span><span class="sxs-lookup"><span data-stu-id="a3946-129">An event access.</span></span> <span data-ttu-id="a3946-130">すべてのイベントアクセスには、関連付けられた型、つまりイベントの種類があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="a3946-131">さらに、イベントアクセスには、インスタンス式が関連付けられている場合もあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="a3946-132">イベントアクセスは、`+=` 演算子と `-=` 演算子 ([イベント割り当て](expressions.md#event-assignment)) の左側のオペランドとして表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="a3946-133">その他のコンテキストでは、イベントアクセスとして分類された式によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="a3946-134">インデクサーアクセス。</span><span class="sxs-lookup"><span data-stu-id="a3946-134">An indexer access.</span></span> <span data-ttu-id="a3946-135">すべてのインデクサーアクセスには、関連付けられた型、つまりインデクサーの要素型があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="a3946-136">さらに、インデクサーアクセスには、インスタンス式と関連付けられた引数リストが関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="a3946-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="a3946-137">インデクサーアクセスのアクセサー (`get` または `set` ブロック) が呼び出されると、インスタンス式を評価した結果が `this` ([このアクセス](expressions.md#this-access)) によって表されるインスタンスになり、引数リストを評価した結果が呼び出しのパラメーターリストになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="a3946-138">Nothing。</span><span class="sxs-lookup"><span data-stu-id="a3946-138">Nothing.</span></span> <span data-ttu-id="a3946-139">このエラーは、式が `void`の戻り値の型を持つメソッドの呼び出しである場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="a3946-140">Nothing として分類された式は、 *statement_expression* ([式ステートメント](statements.md#expression-statements)) のコンテキストでのみ有効です。</span><span class="sxs-lookup"><span data-stu-id="a3946-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="a3946-141">式の最終的な結果は、名前空間、型、メソッドグループ、またはイベントアクセスにはなりません。</span><span class="sxs-lookup"><span data-stu-id="a3946-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="a3946-142">前述のように、これらの式のカテゴリは、特定のコンテキストでのみ許可される中間構造です。</span><span class="sxs-lookup"><span data-stu-id="a3946-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="a3946-143">プロパティアクセスまたはインデクサーアクセスは、 *get アクセサー*または*set アクセサー*の呼び出しを実行することによって、常に値として再分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="a3946-144">特定のアクセサーは、プロパティまたはインデクサーアクセスのコンテキストによって決定されます。アクセスが割り当てのターゲットである場合は、 *set アクセサー*が呼び出され、新しい値 ([単純な割り当て](expressions.md#simple-assignment)) が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="a3946-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="a3946-145">それ以外の場合は、 *get アクセサー*を呼び出して、現在の値 ([式の値](expressions.md#values-of-expressions)) を取得します。</span><span class="sxs-lookup"><span data-stu-id="a3946-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="a3946-146">式の値</span><span class="sxs-lookup"><span data-stu-id="a3946-146">Values of expressions</span></span>

<span data-ttu-id="a3946-147">式を含むほとんどのコンストラクトでは、最終的に***値***を示す式が必要になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="a3946-148">このような場合、実際の式が名前空間、型、メソッドグループ、または nothing を表していると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="a3946-149">ただし、式がプロパティアクセス、インデクサーアクセス、または変数を表している場合、プロパティ、インデクサー、または変数の値は暗黙的に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="a3946-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="a3946-150">変数の値は、変数によって識別されるストレージの場所に現在格納されている値にすぎません。</span><span class="sxs-lookup"><span data-stu-id="a3946-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="a3946-151">変数は、値を取得する前に確実に代入 ([明確な代入](variables.md#definite-assignment)) される必要があります。そうしないと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="a3946-152">プロパティアクセス式の値は、プロパティの*get アクセサー*を呼び出すことによって取得されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="a3946-153">プロパティに*get アクセサー*がない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="a3946-154">それ以外の場合は、関数メンバーの呼び出し ([動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) が実行され、呼び出しの結果がプロパティアクセス式の値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="a3946-155">インデクサーアクセス式の値は、インデクサーの*get アクセサー*を呼び出すことによって取得されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="a3946-156">インデクサーに*get アクセサー*がない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="a3946-157">それ以外の場合、関数メンバー呼び出し ([動的なオーバーロード解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) は、インデクサーアクセス式に関連付けられている引数リストを使用して実行され、呼び出しの結果がインデクサーアクセス式の値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="a3946-158">静的バインドと動的バインド</span><span class="sxs-lookup"><span data-stu-id="a3946-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="a3946-159">構成式 (引数、オペランド、レシーバー) の型または値に基づいて操作の意味を判断するプロセスは、多くの場合、***バインド***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="a3946-160">たとえば、メソッド呼び出しの意味は、受信側と引数の型に基づいて決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="a3946-161">演算子の意味は、そのオペランドの型に基づいて決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="a3946-162">でC#は、通常、操作の意味はコンパイル時に決定され、その構成式のコンパイル時の型に基づいて決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="a3946-163">同様に、式にエラーが含まれている場合は、コンパイラによってエラーが検出され、報告されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="a3946-164">この方法は、***静的バインド***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="a3946-165">ただし、式が動的な式 (つまり、型が `dynamic`) の場合、その式が参加しているすべてのバインディングは、コンパイル時に含まれる型ではなく、その実行時の型 (つまり、実行時に示されるオブジェクトの実際の型) に基づいている必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="a3946-166">そのため、このような操作のバインドは、プログラムの実行中に操作が実行される時間まで延期されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="a3946-167">これを***動的バインド***と呼びます。</span><span class="sxs-lookup"><span data-stu-id="a3946-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="a3946-168">操作が動的にバインドされる場合、コンパイラによるチェックはほとんどまたはまったく実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="a3946-169">代わりに、実行時のバインドが失敗した場合、実行時にエラーが例外として報告されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="a3946-170">のC#次の操作は、バインドの対象となります。</span><span class="sxs-lookup"><span data-stu-id="a3946-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="a3946-171">メンバーアクセス: `e.M`</span><span class="sxs-lookup"><span data-stu-id="a3946-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="a3946-172">メソッドの呼び出し: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="a3946-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="a3946-173">デリゲート呼び出し:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="a3946-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="a3946-174">要素アクセス: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="a3946-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="a3946-175">オブジェクトの作成: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="a3946-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="a3946-176">オーバーロードされた単項演算子: `+`、`-`、`!`、`~`、`++`、`--`、`true`、`false`</span><span class="sxs-lookup"><span data-stu-id="a3946-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="a3946-177">オーバーロードされた二項演算子: `+`、`-`、`*`、`/`、`%`、`&`、`&&`、`|`、`||`、`??`、`^`、`<<`、`>>`、`==`、`!=``>``<``>=``<=`</span><span class="sxs-lookup"><span data-stu-id="a3946-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="a3946-178">代入演算子: `=`、`+=`、`-=`、`*=`、`/=`、`%=`、`&=`、`|=`、`^=`、`<<=`、`>>=`</span><span class="sxs-lookup"><span data-stu-id="a3946-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="a3946-179">暗黙的な変換と明示的な変換</span><span class="sxs-lookup"><span data-stu-id="a3946-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="a3946-180">動的な式が関係しないC#場合、既定では静的バインディングが使用されます。これは、構成式のコンパイル時の型が選択プロセスで使用されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="a3946-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="a3946-181">ただし、上記の操作のいずれかの構成式が動的な式である場合、その操作は動的にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="a3946-182">バインド-時間</span><span class="sxs-lookup"><span data-stu-id="a3946-182">Binding-time</span></span>

<span data-ttu-id="a3946-183">静的バインドはコンパイル時に行われますが、動的バインドは実行時に行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="a3946-184">次のセクションでは、バインド***時間***とは、バインディングがいつ行われるかに応じて、コンパイル時または実行時を指します。</span><span class="sxs-lookup"><span data-stu-id="a3946-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="a3946-185">次の例は、静的バインディングと動的バインドの概念とバインド時の概念を示しています。</span><span class="sxs-lookup"><span data-stu-id="a3946-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="a3946-186">最初の2つの呼び出しは静的にバインドされます。 `Console.WriteLine` のオーバーロードは、引数のコンパイル時の型に基づいて選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="a3946-187">したがって、バインディング時間はコンパイル時です。</span><span class="sxs-lookup"><span data-stu-id="a3946-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="a3946-188">3番目の呼び出しは動的にバインドされます。 `Console.WriteLine` のオーバーロードは、引数の実行時の型に基づいて選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="a3946-189">これは、引数が動的な式である (コンパイル時の型が `dynamic`) ために発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="a3946-190">そのため、3番目の呼び出しのバインディング時間は実行時です。</span><span class="sxs-lookup"><span data-stu-id="a3946-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="a3946-191">動的バインディング</span><span class="sxs-lookup"><span data-stu-id="a3946-191">Dynamic binding</span></span>

<span data-ttu-id="a3946-192">動的バインディングの目的は、プログラムがC# ***動的オブジェクト***(つまり、 C#型システムの通常の規則に従っていないオブジェクト) と対話できるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="a3946-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="a3946-193">動的オブジェクトは、異なる型システムを使用する他のプログラミング言語のオブジェクトである場合もあれば、異なる操作のために独自のバインディングセマンティクスを実装するようにプログラムで設定されるオブジェクトの場合もあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="a3946-194">動的オブジェクトが独自のセマンティクスを実装する機構は、実装が定義されていることです。</span><span class="sxs-lookup"><span data-stu-id="a3946-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="a3946-195">指定されたインターフェイス--再実装が定義されている場合は、特殊C#なセマンティクスがあることをランタイムに通知するために、動的オブジェクトによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="a3946-196">したがって、動的なオブジェクトに対する操作が動的にバインドされる場合、このドキュメントで指定C#されているものではなく、独自のバインディングセマンティクスが引き継がれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="a3946-197">動的バインドの目的は動的オブジェクトとの相互運用を可能にC#することであるのに対し、は動的なバインドであるかどうかにかかわらず、すべてのオブジェクトで動的バインドを許可します。</span><span class="sxs-lookup"><span data-stu-id="a3946-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="a3946-198">これにより、動的オブジェクトに対する操作の結果が動的なオブジェクトになるとは限りませんが、コンパイル時にはプログラマにとって未知の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="a3946-199">また、動的バインドを使用すると、オブジェクトが動的オブジェクトに関連しない場合でも、エラーが発生しやすいリフレクションベースのコードを排除できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="a3946-200">次のセクションでは、動的バインドが適用されたときの言語の各構成要素、コンパイル時のチェック (適用されている場合)、およびコンパイル時の結果と式の分類について説明します。</span><span class="sxs-lookup"><span data-stu-id="a3946-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="a3946-201">構成式の型</span><span class="sxs-lookup"><span data-stu-id="a3946-201">Types of constituent expressions</span></span>

<span data-ttu-id="a3946-202">操作が静的にバインドされている場合、構成式の型 (たとえば、レシーバー、引数、インデックス、またはオペランド) は、常にその式のコンパイル時の型と見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="a3946-203">操作が動的にバインドされる場合、構成式の型は、構成式のコンパイル時の型に応じて、さまざまな方法で決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="a3946-204">コンパイル時の型 `dynamic` の構成式は、実行時に式が評価する実際の値の型を持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="a3946-205">コンパイル時の型が型パラメーターである構成式は、実行時に型パラメーターがバインドされる型を持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="a3946-206">それ以外の場合、構成式はコンパイル時の型を持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="a3946-207">演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-207">Operators</span></span>

<span data-ttu-id="a3946-208">式は、***オペランド***と***演算子***から構築されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="a3946-209">式の演算子は、オペランドに適用する演算を表します。</span><span class="sxs-lookup"><span data-stu-id="a3946-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="a3946-210">演算子の例として、`+`、`-`、`*`、`/`、および `new` などがあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="a3946-211">オペランドの例としては、リテラル、フィールド、ローカル変数、式などがあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="a3946-212">演算子には、次の3種類があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="a3946-213">単項演算子。</span><span class="sxs-lookup"><span data-stu-id="a3946-213">Unary operators.</span></span> <span data-ttu-id="a3946-214">単項演算子は1つのオペランドを受け取り、プレフィックス表記 (`--x`など) または後置表記 (`x++`など) のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="a3946-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="a3946-215">二項演算子。</span><span class="sxs-lookup"><span data-stu-id="a3946-215">Binary operators.</span></span> <span data-ttu-id="a3946-216">二項演算子は2つのオペランドを受け取り、すべて挿入辞表記 (`x + y`など) を使用します。</span><span class="sxs-lookup"><span data-stu-id="a3946-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="a3946-217">三項演算子。</span><span class="sxs-lookup"><span data-stu-id="a3946-217">Ternary operator.</span></span> <span data-ttu-id="a3946-218">三項演算子 `?:`は1つだけ存在します。3つのオペランドを受け取り、挿入辞表記 (`c ? x : y`) を使用します。</span><span class="sxs-lookup"><span data-stu-id="a3946-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="a3946-219">式における演算子の評価の順序は、演算子の***優先順位***と***結合規則***(演算子の[優先順位と結合規則](expressions.md#operator-precedence-and-associativity)) によって決まります。</span><span class="sxs-lookup"><span data-stu-id="a3946-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="a3946-220">式のオペランドは左から右に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="a3946-221">たとえば、`F(i) + G(i++) * H(i)`では、メソッド `F` は `i`の古い値を使用して呼び出され、メソッド `G` は `i`の古い値を使用して呼び出され、最後にメソッド `H` が `i`の新しい値で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="a3946-222">これは、演算子の優先順位とは別のものです。</span><span class="sxs-lookup"><span data-stu-id="a3946-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="a3946-223">特定の演算子は***オーバーロード***できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="a3946-224">演算子のオーバーロードでは、一方または両方のオペランドがユーザー定義のクラスまたは構造体の型 ([演算子のオーバーロード](expressions.md#operator-overloading)) である操作に対して、ユーザー定義の演算子の実装を指定できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="a3946-225">演算子の優先順位と結合規則</span><span class="sxs-lookup"><span data-stu-id="a3946-225">Operator precedence and associativity</span></span>

<span data-ttu-id="a3946-226">複数の演算子を含む式の場合、演算子の***優先順位***によって各々の演算子が評価される順序が決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="a3946-227">たとえば、式 `x + y * z` は `x + (y * z)` として評価されます。これは、`*` 演算子の優先順位がバイナリ `+` 演算子よりも高いためです。</span><span class="sxs-lookup"><span data-stu-id="a3946-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="a3946-228">演算子の優先順位は、関連付けられている文法の定義によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="a3946-229">たとえば、 *additive_expression*は `+` または `-` の演算子で区切られた一連の*multiplicative_expression*で構成されます。したがって、`+` 演算子と `-` 演算子の優先順位は、`*`、`/`、および `%` になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="a3946-230">次の表は、すべての演算子を優先順位の高い順に示しています。</span><span class="sxs-lookup"><span data-stu-id="a3946-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="a3946-231">__Section__</span><span class="sxs-lookup"><span data-stu-id="a3946-231">__Section__</span></span>                                                                                   | <span data-ttu-id="a3946-232">__カテゴリ__</span><span class="sxs-lookup"><span data-stu-id="a3946-232">__Category__</span></span>                | <span data-ttu-id="a3946-233">__演算子__</span><span class="sxs-lookup"><span data-stu-id="a3946-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="a3946-234">一次式</span><span class="sxs-lookup"><span data-stu-id="a3946-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="a3946-235">1 次式</span><span class="sxs-lookup"><span data-stu-id="a3946-235">Primary</span></span>                     | <span data-ttu-id="a3946-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="a3946-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="a3946-237">単項演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="a3946-238">単項</span><span class="sxs-lookup"><span data-stu-id="a3946-238">Unary</span></span>                       | <span data-ttu-id="a3946-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="a3946-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="a3946-240">算術演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="a3946-241">乗法</span><span class="sxs-lookup"><span data-stu-id="a3946-241">Multiplicative</span></span>              | <span data-ttu-id="a3946-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="a3946-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="a3946-243">算術演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="a3946-244">加法</span><span class="sxs-lookup"><span data-stu-id="a3946-244">Additive</span></span>                    | <span data-ttu-id="a3946-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="a3946-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="a3946-246">シフト演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="a3946-247">シフト</span><span class="sxs-lookup"><span data-stu-id="a3946-247">Shift</span></span>                       | <span data-ttu-id="a3946-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="a3946-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="a3946-249">関係演算子と型検査演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="a3946-250">関係式と型検査</span><span class="sxs-lookup"><span data-stu-id="a3946-250">Relational and type testing</span></span> | <span data-ttu-id="a3946-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="a3946-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="a3946-252">関係演算子と型検査演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="a3946-253">等価比較</span><span class="sxs-lookup"><span data-stu-id="a3946-253">Equality</span></span>                    | <span data-ttu-id="a3946-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="a3946-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="a3946-255">論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="a3946-256">論理 AND</span><span class="sxs-lookup"><span data-stu-id="a3946-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="a3946-257">論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="a3946-258">論理 XOR</span><span class="sxs-lookup"><span data-stu-id="a3946-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="a3946-259">論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="a3946-260">論理 OR</span><span class="sxs-lookup"><span data-stu-id="a3946-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="a3946-261">条件論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="a3946-262">条件 AND</span><span class="sxs-lookup"><span data-stu-id="a3946-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="a3946-263">条件論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="a3946-264">条件 OR</span><span class="sxs-lookup"><span data-stu-id="a3946-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="a3946-265">null 合体演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="a3946-266">Null 合体演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="a3946-267">条件演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="a3946-268">条件</span><span class="sxs-lookup"><span data-stu-id="a3946-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="a3946-269">[代入演算子](expressions.md#assignment-operators)、[匿名関数式](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="a3946-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="a3946-270">代入式とラムダ式</span><span class="sxs-lookup"><span data-stu-id="a3946-270">Assignment and lambda expression</span></span> | <span data-ttu-id="a3946-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="a3946-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="a3946-272">同じ優先順位を持つ2つの演算子の間にオペランドがある場合、演算子の結合規則によって、操作の実行順序が制御されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="a3946-273">代入演算子および null 合体演算子を除き、すべての二項演算子は左から右へと***結合***されます。つまり、操作は左から右に実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="a3946-274">たとえば、 `x + y + z` は `(x + y) + z`と評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="a3946-275">代入演算子、null 合体演算子、および条件演算子 (`?:`***) は、右から***左に操作が実行されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="a3946-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="a3946-276">たとえば、 `x = y = z` は `x = (y = z)`と評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="a3946-277">優先順位と結合性は、かっこを使用して制御することができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="a3946-278">たとえば、`x + y * z` は最初に `y` と `z` を掛け、そして結果を `x` に足しますが、`(x + y) * z` では最初に `x` と `y` を足してから `z` を掛けます。</span><span class="sxs-lookup"><span data-stu-id="a3946-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="a3946-279">演算子のオーバーロード</span><span class="sxs-lookup"><span data-stu-id="a3946-279">Operator overloading</span></span>

<span data-ttu-id="a3946-280">すべての単項演算子と二項演算子には、任意の式で自動的に使用できる定義済みの実装があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="a3946-281">定義済みの実装に加えて、クラスと構造体 ([演算子](classes.md#operators)) に `operator` 宣言を含めることによって、ユーザー定義の実装を導入することができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="a3946-282">ユーザー定義の演算子の実装は、常に定義済みの演算子の実装よりも優先されます。ユーザー定義の演算子の実装が存在しない場合のみ、[単項演算子のオーバーロードの解決](expressions.md#unary-operator-overload-resolution)と[二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)に関するページで説明されているように、定義済みの演算子の実装が考慮されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="a3946-283">オーバーロード可能な***単項演算子***は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="a3946-284">`true` と `false` は式では明示的に使用されません (したがって、演算子の[優先順位と結合規則](expressions.md#operator-precedence-and-associativity)の優先順位テーブルには含まれません) が、演算子と見なされます。これは、ブール式 ([ブール式](expressions.md#boolean-expressions)) と条件付き演算子 (条件付き[演算子](expressions.md#conditional-operator)) を含む式 (条件付き[論理演算子](expressions.md#conditional-logical-operators)) で呼び出されるためです。</span><span class="sxs-lookup"><span data-stu-id="a3946-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="a3946-285">オーバーロードされた***二項演算子***は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="a3946-286">上に示した演算子のみをオーバーロードできます。</span><span class="sxs-lookup"><span data-stu-id="a3946-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="a3946-287">特に、メンバーアクセス、メソッド呼び出し、`=`、`&&`、`||`、`??`、`?:`、`=>`、`checked`、`unchecked`、`new`、`typeof`、`default`、`as`、`is` の各演算子をオーバーロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="a3946-288">二項演算子をオーバーロードすると、対応する代入演算子がある場合、これも暗黙的にオーバーロードされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="a3946-289">たとえば、operator `*` のオーバーロードは、operator `*=`のオーバーロードでもあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="a3946-290">詳細については、「[複合代入](expressions.md#compound-assignment)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="a3946-291">代入演算子自体 (`=`) はオーバーロードできないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="a3946-292">代入は常に、値の単純なビット単位のコピーを変数に対して実行します。</span><span class="sxs-lookup"><span data-stu-id="a3946-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="a3946-293">`(T)x`などのキャスト操作は、ユーザー定義の変換 ([ユーザー定義の変換](conversions.md#user-defined-conversions)) を提供することによってオーバーロードされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="a3946-294">`a[x]`などの要素アクセスは、オーバーロード可能な演算子とは見なされません。</span><span class="sxs-lookup"><span data-stu-id="a3946-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="a3946-295">代わりに、インデクサー ([インデクサー](classes.md#indexers)) を使用してユーザー定義のインデックス作成がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="a3946-296">式では、演算子は演算子表記を使用して参照され、宣言では、演算子は関数表記を使用して参照されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="a3946-297">次の表は、単項演算子と二項演算子の演算子と関数表記の関係を示しています。</span><span class="sxs-lookup"><span data-stu-id="a3946-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="a3946-298">最初のエントリで、 *op*はオーバーロードされた単項前置演算子を表します。</span><span class="sxs-lookup"><span data-stu-id="a3946-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="a3946-299">2番目のエントリでは、 *op*は単項後置 `++` と `--` 演算子を表します。</span><span class="sxs-lookup"><span data-stu-id="a3946-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="a3946-300">3番目のエントリでは、 *op*はオーバーロード可能な任意の二項演算子を表します。</span><span class="sxs-lookup"><span data-stu-id="a3946-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="a3946-301">__演算子の表記__</span><span class="sxs-lookup"><span data-stu-id="a3946-301">__Operator notation__</span></span> | <span data-ttu-id="a3946-302">__関数型表記__</span><span class="sxs-lookup"><span data-stu-id="a3946-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="a3946-303">ユーザー定義の演算子宣言には、常に、演算子宣言を含むクラスまたは構造体型のパラメーターが少なくとも1つ必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="a3946-304">したがって、ユーザー定義の演算子は、定義済みの演算子と同じシグネチャを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="a3946-305">ユーザー定義の演算子宣言では、演算子の構文、優先順位、または結合規則を変更できません。</span><span class="sxs-lookup"><span data-stu-id="a3946-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="a3946-306">たとえば、`/` 演算子は常に二項演算子であり、[演算子の優先順位と結合規則](expressions.md#operator-precedence-and-associativity)で指定された優先順位レベルを常に持ち、常に左から結合されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="a3946-307">ユーザー定義の演算子は、pleases の計算を実行することができますが、直感的に想定されているもの以外の結果を生成する実装は避けることを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a3946-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="a3946-308">たとえば、`operator ==` の実装では、2つのオペランドが等しいかどうかを比較し、適切な `bool` 結果を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="a3946-309">[条件付き論理演算子](expressions.md#conditional-logical-operators)を使用した[主な式](expressions.md#primary-expressions)の各演算子の説明では、演算子の定義済みの実装と、各演算子に適用される追加の規則を指定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="a3946-310">この説明では、***単項演算子のオーバーロードの解決***、***二項演算子のオーバーロードの解決***、および数値の***上位変換***という用語を使用します。これらの定義については、次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="a3946-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="a3946-311">単項演算子のオーバーロードの解決</span><span class="sxs-lookup"><span data-stu-id="a3946-311">Unary operator overload resolution</span></span>

<span data-ttu-id="a3946-312">`op x` または `x op`の形式の演算。ここで `op` は、オーバーロード可能な単項演算子で、`x` は `X`型の式で、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="a3946-313">操作 `operator op(x)` に対して `X` によって提供される一連のユーザー定義演算子は、[ユーザー定義の候補演算子](expressions.md#candidate-user-defined-operators)の規則を使用して決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="a3946-314">一連のユーザー定義演算子が空でない場合は、これが操作の候補演算子のセットになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="a3946-315">それ以外の場合、定義済みの単項 `operator op` 実装 (リフトされた形式を含む) は、操作の候補演算子のセットになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="a3946-316">特定の演算子の定義済みの実装は、演算子の説明 (主な[式](expressions.md#primary-expressions)および[単項演算子](expressions.md#unary-operators)) で指定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="a3946-317">[オーバーロード解決](expressions.md#overload-resolution)のオーバーロード解決規則は候補演算子のセットに適用され、`(x)`引数リストに対して最適な演算子を選択します。この演算子は、オーバーロードの解決プロセスの結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="a3946-318">オーバーロードの解決で1つの最適な演算子を選択できなかった場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="a3946-319">二項演算子のオーバーロードの解決</span><span class="sxs-lookup"><span data-stu-id="a3946-319">Binary operator overload resolution</span></span>

<span data-ttu-id="a3946-320">`x op y`形式の演算。ここで `op` は、オーバーロード可能な二項演算子で、`x` は型 `X`の式で、`y` は型 `Y`の式で、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="a3946-321">操作 `operator op(x,y)` に対して `X` および `Y` によって提供される一連の候補ユーザー定義演算子が決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="a3946-322">このセットは、`X` によって提供される候補演算子と `Y`によって提供される候補演算子の和集合で構成されます。各演算子は、[ユーザー定義演算子の候補](expressions.md#candidate-user-defined-operators)の規則を使用して決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="a3946-323">`X` と `Y` が同じ型である場合、または `X` と `Y` が共通の基本型から派生している場合、共有候補の演算子は、結合されたセット内で1回だけ発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="a3946-324">一連のユーザー定義演算子が空でない場合は、これが操作の候補演算子のセットになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="a3946-325">それ以外の場合、定義済みのバイナリ `operator op` 実装 (リフトされた形式を含む) が操作の候補演算子のセットになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="a3946-326">特定の演算子の定義済みの実装は、演算子の説明で指定されます ([条件付き論理演算子](expressions.md#conditional-logical-operators)を使用した[算術演算子](expressions.md#arithmetic-operators))。</span><span class="sxs-lookup"><span data-stu-id="a3946-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="a3946-327">定義済みの列挙型およびデリゲート演算子の場合、考慮される演算子は、列挙型またはデリゲート型によって定義されている演算子のうち、いずれかのオペランドのバインド時の型であるものだけです。</span><span class="sxs-lookup"><span data-stu-id="a3946-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="a3946-328">[オーバーロード解決](expressions.md#overload-resolution)のオーバーロード解決規則は候補演算子のセットに適用され、`(x,y)`引数リストに対して最適な演算子を選択します。この演算子は、オーバーロードの解決プロセスの結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="a3946-329">オーバーロードの解決で1つの最適な演算子を選択できなかった場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="a3946-330">ユーザー定義演算子の候補</span><span class="sxs-lookup"><span data-stu-id="a3946-330">Candidate user-defined operators</span></span>

<span data-ttu-id="a3946-331">型 `T` と操作 `operator op(A)`が指定されている場合、`op` はオーバーロード可能な演算子であり、`A` は引数リストであるため、`T` の `operator op(A)` によって提供される候補ユーザー定義演算子のセットは、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="a3946-332">`T0`型を決定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-332">Determine the type `T0`.</span></span> <span data-ttu-id="a3946-333">`T` が null 許容型である場合、`T0` はその基になる型であり、それ以外の場合は `T0` が `T`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="a3946-334">`T0` 内のすべての `operator op` 宣言、およびそのような演算子のすべてのリフトされた形式では、`A`の引数リストに対して少なくとも1つの演算子 ([適用可能な関数メンバー](expressions.md#applicable-function-member)) が適用される場合、候補演算子のセットは `T0`内の該当するすべての演算子で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="a3946-335">それ以外の場合、`T0` が `object`場合、候補演算子のセットは空になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="a3946-336">それ以外の場合、`T0` によって提供される候補演算子のセットは、`T0`の直接基底クラスによって提供される候補演算子のセット、または `T0` が型パラメーターである場合は `T0` の有効な基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="a3946-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="a3946-337">数値の上位変換</span><span class="sxs-lookup"><span data-stu-id="a3946-337">Numeric promotions</span></span>

<span data-ttu-id="a3946-338">数値の上位変換は、定義済みの単項演算子と二項数値演算子のオペランドの特定の暗黙的な変換を自動的に実行することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="a3946-339">数値の上位変換は個別のメカニズムではなく、定義済みの演算子にオーバーロードの解決を適用した場合の効果です。</span><span class="sxs-lookup"><span data-stu-id="a3946-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="a3946-340">数値の上位変換は、特にユーザー定義の演算子の評価には影響しませんが、同様の効果をもたらすためにユーザー定義の演算子を実装することはできます。</span><span class="sxs-lookup"><span data-stu-id="a3946-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="a3946-341">数値の上位変換の例として、バイナリ `*` 演算子の定義済みの実装について考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="a3946-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="a3946-342">オーバーロードの解決規則 ([オーバーロードの解決](expressions.md#overload-resolution)) がこの一連の演算子に適用される場合、結果として、オペランドの型から暗黙的な変換が存在する最初の演算子が選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="a3946-343">たとえば、操作 `b * s`の場合、`b` は `byte`、`s` は `short`であるため、オーバーロードの解決では最適な演算子として `operator *(int,int)` が選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="a3946-344">したがって、`b` と `s` は `int`に変換され、結果の型は `int`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="a3946-345">同様に、操作 `i * d`では、`i` は `int`、`d` は `double`であるため、オーバーロードの解決では、最適な演算子として `operator *(double,double)` が選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="a3946-346">単項数値の上位変換</span><span class="sxs-lookup"><span data-stu-id="a3946-346">Unary numeric promotions</span></span>

<span data-ttu-id="a3946-347">単項数値の上位変換は、定義済みの `+`、`-`、および `~` 単項演算子のオペランドに対して行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="a3946-348">単項数値の上位変換は、型 `sbyte`、`byte`、`short`、`ushort`、または `char` のオペランドを型 `int`に変換するだけで構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="a3946-349">さらに、単項 `-` 演算子では、単項数値の上位変換では `uint` 型のオペランドが `long`型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="a3946-350">バイナリ数値の上位変換</span><span class="sxs-lookup"><span data-stu-id="a3946-350">Binary numeric promotions</span></span>

<span data-ttu-id="a3946-351">バイナリ数値の上位変換は、定義済みの `+`、`-`、`*`、`/`、`%`、`&`、`|`、`^`、`==`、`!=`、`>`、`<`、`>=`、および `<=` 二項演算子のオペランドに対して行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="a3946-352">バイナリ数値の昇格では、両方のオペランドが共通の型に暗黙的に変換されます。これは、非関係演算子の場合、演算の結果の型にもなります。</span><span class="sxs-lookup"><span data-stu-id="a3946-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="a3946-353">バイナリ数値の上位変換では、次のルールがここに表示される順序で適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="a3946-354">いずれかのオペランドが `decimal`型である場合、もう一方のオペランドが `decimal`型に変換されるか、または他のオペランドが `float` または `double`型である場合は、バインディング時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="a3946-355">それ以外の場合、いずれかのオペランドが `double`型である場合、もう一方のオペランドは `double`型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="a3946-356">それ以外の場合、いずれかのオペランドが `float`型である場合、もう一方のオペランドは `float`型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="a3946-357">それ以外の場合、いずれかのオペランドが `ulong`型である場合は、もう一方のオペランドが `ulong`型に変換されるか、または他のオペランドが `sbyte`、`short`、`int`、または `long`型の場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="a3946-358">それ以外の場合、いずれかのオペランドが `long`型である場合、もう一方のオペランドは `long`型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="a3946-359">それ以外の場合、いずれかのオペランドが `uint` 型で、もう一方のオペランドが `sbyte`、`short`、または `int`型である場合、両方のオペランドが型 `long`に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="a3946-360">それ以外の場合、いずれかのオペランドが `uint`型である場合、もう一方のオペランドは `uint`型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="a3946-361">それ以外の場合、両方のオペランドが `int`型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="a3946-362">最初のルールでは、`decimal` の種類と `double` と `float` の種類を混在させる操作は許可されていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="a3946-363">このルールは、`decimal` 型と `double` と `float` の型の間に暗黙的な変換がないという事実から、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="a3946-364">もう一方のオペランドが符号付き整数型の場合、オペランドを `ulong` 型にすることはできないことにも注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="a3946-365">これは、`ulong` の完全な範囲と符号付き整数型を表すことができる整数型が存在しないためです。</span><span class="sxs-lookup"><span data-stu-id="a3946-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="a3946-366">どちらの場合も、キャスト式を使用して、あるオペランドを他のオペランドと互換性のある型に明示的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="a3946-367">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="a3946-368">`decimal` に `double`で乗算できないため、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="a3946-369">このエラーは、次のように、2番目のオペランドを `decimal`に明示的に変換することによって解決されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="a3946-370">リフト演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-370">Lifted operators</span></span>

<span data-ttu-id="a3946-371">***リフト***された演算子を使用すると、null 非許容の値型を操作する定義済みの演算子とユーザー定義の演算子を、これらの型の null 許容形式でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="a3946-372">リフトされた演算子は、以下で説明するように、特定の要件を満たす定義済み演算子およびユーザー定義演算子から構築されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="a3946-373">単項演算子の場合</span><span class="sxs-lookup"><span data-stu-id="a3946-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="a3946-374">オペランドと結果の型が両方とも null 非許容の値型である場合、演算子のリフトされた形式が存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="a3946-375">リフトされた形式を作成するには、オペランドと結果の型に1つの `?` 修飾子を追加します。</span><span class="sxs-lookup"><span data-stu-id="a3946-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="a3946-376">オペランドが null の場合、リフトされた演算子は null 値を生成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="a3946-377">それ以外の場合、リフトされた演算子はオペランドのラップを解除し、基になる演算子を適用して、結果をラップします。</span><span class="sxs-lookup"><span data-stu-id="a3946-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="a3946-378">二項演算子の場合</span><span class="sxs-lookup"><span data-stu-id="a3946-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="a3946-379">オペランドと結果の型がすべて null 非許容の値型である場合、演算子のリフトされた形式が存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="a3946-380">リフトされた形式を作成するには、各オペランドと結果の型に1つの `?` 修飾子を追加します。</span><span class="sxs-lookup"><span data-stu-id="a3946-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="a3946-381">リフトされた演算子は、一方または両方のオペランドが null の場合、null 値を生成します (ブール型の[論理演算子](expressions.md#boolean-logical-operators)に関するページで説明されているように、`bool?` 型の `&` 演算子と `|` 演算子である例外)。</span><span class="sxs-lookup"><span data-stu-id="a3946-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="a3946-382">それ以外の場合、リフトされた演算子はオペランドのラップを解除し、基になる演算子を適用して、結果をラップします。</span><span class="sxs-lookup"><span data-stu-id="a3946-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="a3946-383">等値演算子の場合</span><span class="sxs-lookup"><span data-stu-id="a3946-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="a3946-384">オペランドの型が null 非許容の値型であり、結果の型が `bool`場合、演算子のリフトされた形式が存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="a3946-385">リフトされた形式を作成するには、各オペランド型に1つの `?` 修飾子を追加します。</span><span class="sxs-lookup"><span data-stu-id="a3946-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="a3946-386">リフトされた演算子は、2つの null 値が等しいと見なされ、null 値は null 以外の値と等しくなりません。</span><span class="sxs-lookup"><span data-stu-id="a3946-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="a3946-387">両方のオペランドが null 以外の場合、リフトされた演算子はオペランドのラップを解除し、基になる演算子を適用して `bool` の結果を生成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="a3946-388">関係演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="a3946-389">オペランドの型が null 非許容の値型であり、結果の型が `bool`場合、演算子のリフトされた形式が存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="a3946-390">リフトされた形式を作成するには、各オペランド型に1つの `?` 修飾子を追加します。</span><span class="sxs-lookup"><span data-stu-id="a3946-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="a3946-391">リフトされた演算子は、一方または両方のオペランドが null の場合に `false` 値を生成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="a3946-392">それ以外の場合、リフトされた演算子はオペランドのラップを解除し、基になる演算子を適用して `bool` の結果を生成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="a3946-393">メンバーの参照</span><span class="sxs-lookup"><span data-stu-id="a3946-393">Member lookup</span></span>

<span data-ttu-id="a3946-394">メンバー参照とは、型のコンテキストにおける名前の意味が決定されるプロセスです。</span><span class="sxs-lookup"><span data-stu-id="a3946-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="a3946-395">メンバー参照は、式での*simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバーアクセス](expressions.md#member-access)) の評価の一部として発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="a3946-396">*Simple_name*または*member_access*が*invocation_expression* ([メソッド呼び出し](expressions.md#method-invocations)) の*primary_expression*として発生した場合、メンバーは呼び出されていると言います。</span><span class="sxs-lookup"><span data-stu-id="a3946-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="a3946-397">メンバーがメソッドまたはイベントの場合、またはデリゲート型 ([デリゲート](delegates.md)) または型 `dynamic` ([動的型](types.md#the-dynamic-type)) の定数、フィールド、またはプロパティである場合、メンバーは*invocable*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="a3946-398">メンバーの検索では、メンバーの名前だけでなく、メンバーに含まれる型パラメーターの数と、メンバーにアクセスできるかどうかも考慮されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="a3946-399">メンバー参照のために、ジェネリックメソッドと入れ子になったジェネリック型にはそれぞれの宣言で指定された型パラメーターの数があり、他のすべてのメンバーにはゼロ型パラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="a3946-400">型 `T` の  型パラメーターを `K` `N` 名前のメンバー参照は、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="a3946-401">まず、 `N` という名前のアクセス可能なメンバーのセットが決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="a3946-402">`T` が型パラメーターの場合、 `T`のプライマリ制約またはセカンダリ制約 ([型パラメーター制約](classes.md#type-parameter-constraints)) として指定された各型の `N` という名前のアクセス可能なメンバーのセットは、`object`内の `N` という名前のアクセス可能なメンバーのセットと共に結合されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="a3946-403">それ以外の場合、このセットは、 `T`内の `N` という名前のすべてのアクセス可能な ([メンバーアクセス](basic-concepts.md#member-access)) メンバーで構成されます。これには、継承されたメンバーや、`object`内の `N` という名前のアクセス可能</span><span class="sxs-lookup"><span data-stu-id="a3946-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="a3946-404">`T` が構築された型である場合は、「構築された[型のメンバー](classes.md#members-of-constructed-types)」で説明されているように、型引数を置き換えることによってメンバーのセットを取得します。</span><span class="sxs-lookup"><span data-stu-id="a3946-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="a3946-405">`override` 修飾子を含むメンバーは、セットから除外されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="a3946-406">次に、`K` がゼロの場合、宣言に型パラメーターを含む入れ子になったすべての型が削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="a3946-407">`K` がゼロでない場合は、型パラメーターの数が異なるメンバーがすべて削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="a3946-408">`K` がゼロの場合、型パラメーターを持つメソッドは削除されません。これは、型の推論プロセス ([型の推定](expressions.md#type-inference)) が型引数を推論できる可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="a3946-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="a3946-409">次に、メンバーが*呼び出さ*れると、*invocable*以外のすべてのメンバーがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="a3946-410">次に、他のメンバーによって非表示になっているメンバーがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="a3946-411">Set 内のすべてのメンバー `S.M` について、`S` はメンバー `M` が宣言されている型であるため、次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="a3946-412">`M` が定数、フィールド、プロパティ、イベント、または列挙型のメンバーである場合は、`S` の基本型で宣言されたすべてのメンバーがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="a3946-413">`M` が型宣言の場合、`S` の基本型で宣言されているすべての非型は、セットから削除され、`S` の基本型で宣言された `M` と同じ数の型パラメーターを持つすべての型宣言は、セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="a3946-414">`M` がメソッドの場合、`S` の基本型で宣言されているすべての非メソッドメンバーがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="a3946-415">次に、クラスメンバーによって隠ぺいされたインターフェイスメンバーがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="a3946-416">このステップは、`T` が型パラメーターであり、`object` 以外の有効な基本クラスと、空でない有効なインターフェイスセット ([型パラメーター制約](classes.md#type-parameter-constraints)) の両方が `T` 場合にのみ効果があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="a3946-417">Set 内のすべてのメンバー `S.M` について、`S` はメンバー `M` が宣言されている型であるため、`S` が `object`以外のクラス宣言の場合、次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="a3946-418">`M` が定数、フィールド、プロパティ、イベント、列挙型のメンバー、または型の宣言である場合、インターフェイス宣言で宣言されているすべてのメンバーがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="a3946-419">`M` がメソッドの場合は、インターフェイス宣言で宣言されているすべての非メソッドメンバーがセットから削除され、インターフェイス宣言で宣言された `M` と同じシグネチャを持つすべてのメソッドがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="a3946-420">最後に、非表示のメンバーを削除すると、参照の結果が決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="a3946-421">セットが、メソッドではない1つのメンバーで構成されている場合、このメンバーは参照の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="a3946-422">それ以外の場合、セットにメソッドが含まれている場合、このメソッドのグループは参照の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="a3946-423">それ以外の場合、参照はあいまいであり、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-424">型パラメーターとインターフェイス以外の型のメンバー参照、および厳密に単一継承であるインターフェイス内のメンバー参照 (継承チェーンの各インターフェイスには、完全に0または1つの直接基底インターフェイスがある) の場合、検索規則の効果は派生メンバーが同じ名前またはシグネチャを持つ基本メンバーを非表示にするだけです。</span><span class="sxs-lookup"><span data-stu-id="a3946-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="a3946-425">このような単一継承の参照があいまいになることはありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="a3946-426">複数継承インターフェイスのメンバー参照で生じる可能性があるあいまいさについては、「[インターフェイスメンバーアクセス](interfaces.md#interface-member-access)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="a3946-427">基本型</span><span class="sxs-lookup"><span data-stu-id="a3946-427">Base types</span></span>

<span data-ttu-id="a3946-428">メンバー検索の目的では、型 `T` は次の基本型を持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="a3946-429">`T` が `object`場合、`T` には基本型がありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="a3946-430">`T` が*enum_type*の場合、`T` の基本型は `System.Enum`、`System.ValueType`、および `object`のクラス型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="a3946-431">`T` が*struct_type*の場合、`T` の基本型は `System.ValueType` および `object`のクラス型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="a3946-432">`T` が*class_type*の場合、`T` の基本型は、クラス型 `object`を含む `T`の基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="a3946-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="a3946-433">`T` が*interface_type*の場合、`T` の基本型は `T` の基本インターフェイスであり、クラス型 `object`です。</span><span class="sxs-lookup"><span data-stu-id="a3946-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="a3946-434">`T` が*array_type*の場合、`T` の基本型は `System.Array` および `object`のクラス型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="a3946-435">`T` が*delegate_type*の場合、`T` の基本型は `System.Delegate` および `object`のクラス型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="a3946-436">関数のメンバー</span><span class="sxs-lookup"><span data-stu-id="a3946-436">Function members</span></span>

<span data-ttu-id="a3946-437">関数メンバーは、実行可能なステートメントを含むメンバーです。</span><span class="sxs-lookup"><span data-stu-id="a3946-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="a3946-438">関数メンバーは常に型のメンバーであり、名前空間のメンバーになることはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="a3946-439">C#では、次のカテゴリの関数メンバーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="a3946-440">メソッド</span><span class="sxs-lookup"><span data-stu-id="a3946-440">Methods</span></span>
*  <span data-ttu-id="a3946-441">プロパティ</span><span class="sxs-lookup"><span data-stu-id="a3946-441">Properties</span></span>
*  <span data-ttu-id="a3946-442">イベント</span><span class="sxs-lookup"><span data-stu-id="a3946-442">Events</span></span>
*  <span data-ttu-id="a3946-443">インデクサー</span><span class="sxs-lookup"><span data-stu-id="a3946-443">Indexers</span></span>
*  <span data-ttu-id="a3946-444">ユーザー定義の演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-444">User-defined operators</span></span>
*  <span data-ttu-id="a3946-445">インスタンスコンストラクター</span><span class="sxs-lookup"><span data-stu-id="a3946-445">Instance constructors</span></span>
*  <span data-ttu-id="a3946-446">静的コンストラクター</span><span class="sxs-lookup"><span data-stu-id="a3946-446">Static constructors</span></span>
*  <span data-ttu-id="a3946-447">デストラクター</span><span class="sxs-lookup"><span data-stu-id="a3946-447">Destructors</span></span>

<span data-ttu-id="a3946-448">デストラクターと静的コンストラクター (明示的に呼び出すことはできません) を除き、関数メンバーに含まれるステートメントは、関数メンバー呼び出しを通じて実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="a3946-449">関数メンバー呼び出しを記述する実際の構文は、特定の関数メンバーカテゴリによって異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="a3946-450">関数メンバー呼び出しの引数リスト ([引数](expressions.md#argument-lists)リスト) は、関数メンバーのパラメーターに対する実際の値または変数参照を提供します。</span><span class="sxs-lookup"><span data-stu-id="a3946-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="a3946-451">ジェネリックメソッドの呼び出しでは、型の推定を使用して、メソッドに渡す型引数のセットを決定できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="a3946-452">このプロセスについては、 [「型の推定](expressions.md#type-inference)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="a3946-453">メソッド、インデクサー、演算子、およびインスタンスコンストラクターの呼び出しは、オーバーロードの解決を使用して、呼び出し元の候補となる関数メンバーのセットを決定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="a3946-454">このプロセスの詳細については、「[オーバーロードの解決](expressions.md#overload-resolution)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="a3946-455">バインディング時に特定の関数メンバーが特定された後、場合によってはオーバーロードの解決によって、関数メンバーを呼び出す実際の実行時プロセスについては、「[動的なオーバーロードの解決のコンパイル時のチェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)」で説明されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="a3946-456">次の表は、明示的に呼び出すことができる関数メンバーの6つのカテゴリを含む構造体で行われる処理をまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="a3946-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="a3946-457">テーブルでは、`e`、`x`、`y`、および `value` は、変数または値として分類された式を示します。 `T` は、型として分類される式、`F` はメソッドの簡易名、`P` はプロパティの簡易名です。</span><span class="sxs-lookup"><span data-stu-id="a3946-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="a3946-458">__構築__</span><span class="sxs-lookup"><span data-stu-id="a3946-458">__Construct__</span></span>     | <span data-ttu-id="a3946-459">__例__</span><span class="sxs-lookup"><span data-stu-id="a3946-459">__Example__</span></span>    | <span data-ttu-id="a3946-460">__説明__</span><span class="sxs-lookup"><span data-stu-id="a3946-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="a3946-461">メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="a3946-462">オーバーロードの解決は、含んでいるクラスまたは構造体で `F` 最適なメソッドを選択するために適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="a3946-463">`(x,y)`引数リストを使用してメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="a3946-464">メソッドが `static`ない場合、インスタンス式は `this`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="a3946-465">オーバーロードの解決は、クラスまたは構造体 `T`で `F` 最適なメソッドを選択するために適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="a3946-466">メソッドが `static`ない場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="a3946-467">`(x,y)`引数リストを使用してメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="a3946-468">オーバーロードの解決は、`e`の型によって指定されたクラス、構造体、またはインターフェイスで最適なメソッド F を選択するために適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="a3946-469">メソッドが `static`場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="a3946-470">インスタンス式 `e` と引数リスト `(x,y)`を使用してメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="a3946-471">「プロパティ アクセス」</span><span class="sxs-lookup"><span data-stu-id="a3946-471">Property access</span></span>   | `P`            | <span data-ttu-id="a3946-472">含んでいるクラスまたは構造体のプロパティ `P` の `get` アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="a3946-473">`P` が書き込み専用の場合は、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="a3946-474">`P` が `static`されていない場合、インスタンス式は `this`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="a3946-475">含んでいるクラスまたは構造体のプロパティ `P` の `set` アクセサーは `(value)`引数リストを使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="a3946-476">`P` が読み取り専用の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="a3946-477">`P` が `static`されていない場合、インスタンス式は `this`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="a3946-478">クラスまたは構造体 `T` で `P` プロパティの `get` アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="a3946-479">`P` が `static` されていない場合、または `P` が書き込み専用の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="a3946-480">クラスまたは構造体 `T` 内のプロパティ `P` の `set` アクセサーが、`(value)`の引数リストを使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="a3946-481">`P` が `static` されていない場合、または `P` が読み取り専用の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="a3946-482">`e` の型によって指定されたクラス、構造体、またはインターフェイス内のプロパティ `P` の `get` アクセサーは、インスタンス式 `e`を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="a3946-483">`P` が `static` 場合、または `P` が書き込み専用の場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="a3946-484">`e` の型によって指定されたクラス、構造体、またはインターフェイス内のプロパティ `P` の `set` アクセサーは、インスタンス式 `e` および引数リスト `(value)`を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="a3946-485">`P` が `static` 場合、または `P` が読み取り専用の場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="a3946-486">イベントアクセス</span><span class="sxs-lookup"><span data-stu-id="a3946-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="a3946-487">含んでいるクラスまたは構造体のイベント `E` の `add` アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="a3946-488">`E` が静的でない場合は、インスタンス式が `this`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="a3946-489">含んでいるクラスまたは構造体のイベント `E` の `remove` アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="a3946-490">`E` が静的でない場合は、インスタンス式が `this`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="a3946-491">クラスまたは構造体 `T` のイベント `E` の `add` アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="a3946-492">`E` が静的でない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="a3946-493">クラスまたは構造体 `T` のイベント `E` の `remove` アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="a3946-494">`E` が静的でない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="a3946-495">`e` の型によって指定されたクラス、構造体、またはインターフェイス内のイベント `E` の `add` アクセサーは、インスタンス式 `e`を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="a3946-496">`E` が静的である場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="a3946-497">`e` の型によって指定されたクラス、構造体、またはインターフェイス内のイベント `E` の `remove` アクセサーは、インスタンス式 `e`を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="a3946-498">`E` が静的である場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="a3946-499">インデクサーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="a3946-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="a3946-500">オーバーロードの解決は、e の型によって指定されたクラス、構造体、またはインターフェイスで最適なインデクサーを選択するために適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="a3946-501">インデクサーの `get` アクセサーは、インスタンス式 `e` と `(x,y)`引数リストを使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="a3946-502">インデクサーが書き込み専用の場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="a3946-503">オーバーロードの解決は、`e`の型によって指定されたクラス、構造体、またはインターフェイスで最適なインデクサーを選択するために適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="a3946-504">インデクサーの `set` アクセサーは、インスタンス式 `e` と `(x,y,value)`引数リストを使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="a3946-505">インデクサーが読み取り専用の場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="a3946-506">演算子の呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="a3946-507">オーバーロードの解決は、`x`の型によって指定されたクラスまたは構造体の中で最適な単項演算子を選択するために適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="a3946-508">選択した演算子が引数リスト `(x)`で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="a3946-509">オーバーロードの解決は、`x` と `y`の型によって指定されたクラスまたは構造体の中で最適な二項演算子を選択するために適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="a3946-510">選択した演算子が引数リスト `(x,y)`で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="a3946-511">インスタンスコンストラクターの呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="a3946-512">オーバーロードの解決は、クラスまたは構造体 `T`で最適なインスタンスコンストラクターを選択するために適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="a3946-513">`(x,y)`引数リストを使用してインスタンスコンストラクターが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="a3946-514">引数リスト</span><span class="sxs-lookup"><span data-stu-id="a3946-514">Argument lists</span></span>

<span data-ttu-id="a3946-515">すべての関数メンバーとデリゲートの呼び出しには、関数メンバーのパラメーターの実際の値または変数参照を提供する引数リストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a3946-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="a3946-516">関数メンバー呼び出しの引数リストを指定する構文は、関数メンバーカテゴリによって異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="a3946-517">インスタンスコンストラクター、メソッド、インデクサー、およびデリゲートでは、次に示すように、引数が*argument_list*として指定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="a3946-518">インデクサーの場合、`set` アクセサーを呼び出すときに、引数リストに代入演算子の右オペランドとして指定された式が追加されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="a3946-519">プロパティの場合、引数リストは `get` アクセサーを呼び出すときに空になり、`set` アクセサーを呼び出すときに代入演算子の右オペランドとして指定された式で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="a3946-520">イベントの場合、引数リストは、`+=` または `-=` 演算子の右オペランドとして指定された式で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="a3946-521">ユーザー定義演算子の場合、引数リストは単項演算子の1つのオペランド、または二項演算子の2つのオペランドで構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="a3946-522">プロパティ ([プロパティ](classes.md#properties))、イベント ([イベント](classes.md#events))、およびユーザー定義演算子 ([演算子](classes.md#operators)) の引数は、常に値パラメーター ([値パラメーター](classes.md#value-parameters)) として渡されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="a3946-523">インデクサー ([インデクサー](classes.md#indexers)) の引数は、常に値パラメーター ([値パラメーター](classes.md#value-parameters)) またはパラメーター配列 ([パラメーター配列](classes.md#parameter-arrays)) として渡されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="a3946-524">参照パラメーターと出力パラメーターは、これらのカテゴリの関数メンバーではサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="a3946-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="a3946-525">インスタンスコンストラクター、メソッド、インデクサー、またはデリゲートの呼び出しの引数は、 *argument_list*として指定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

<span data-ttu-id="a3946-526">*Argument_list*は、コンマで区切られた1つ以上の*引数*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="a3946-527">各引数は、省略可能な*argument_name*とそれに続く*argument_value*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="a3946-528">*Argument_name*を持つ*引数*は***名前付き引数***と呼ばれますが、 *argument_name*を持たない*引数*は***位置引数***です。</span><span class="sxs-lookup"><span data-stu-id="a3946-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="a3946-529">*Argument_list*内の名前付き引数の後に位置引数を指定すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="a3946-530">*Argument_value*は、次のいずれかの形式を取ることができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="a3946-531">値パラメーター ([値パラメーター](classes.md#value-parameters)) として引数が渡されることを示す*式*。</span><span class="sxs-lookup"><span data-stu-id="a3946-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="a3946-532">キーワード `ref` 後に*variable_reference* ([変数参照](variables.md#variable-references)) が続き、引数が参照パラメーター ([参照パラメーター](classes.md#reference-parameters)) として渡されることを示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="a3946-533">参照パラメーターとして渡すには、変数を確実に代入 ([明確な代入](variables.md#definite-assignment)) する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="a3946-534">キーワード `out` 後に*variable_reference* ([変数参照](variables.md#variable-references)) が続き、引数が出力パラメーター ([出力パラメーター](classes.md#output-parameters)) として渡されることを示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="a3946-535">変数は、出力パラメーターとして渡される関数メンバー呼び出しの後に、確実に代入 ([明確な代入](variables.md#definite-assignment)) と見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="a3946-536">対応するパラメーター</span><span class="sxs-lookup"><span data-stu-id="a3946-536">Corresponding parameters</span></span>

<span data-ttu-id="a3946-537">引数リストの引数ごとに、呼び出される関数メンバーまたはデリゲートに対応するパラメーターが必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="a3946-538">次のように使用されるパラメーターリストは、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="a3946-539">クラスで定義された仮想メソッドとインデクサーの場合、パラメーターリストは、受信側の静的な型から開始し、その基本クラスを検索することで、関数メンバーの最も限定的な宣言またはオーバーライドから選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="a3946-540">インターフェイスメソッドとインデクサーの場合、パラメーターリストは、インターフェイス型から開始し、基本インターフェイスを検索する、メンバーの最も具体的な定義を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="a3946-541">一意のパラメーターリストが見つからない場合、呼び出しで名前付きパラメーターを使用したり、省略可能な引数を省略したりできないように、アクセスできない名前のパラメーターリストと、省略可能なパラメーターは構築されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="a3946-542">部分メソッドの場合は、部分メソッド宣言を定義するパラメーターリストが使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="a3946-543">他のすべての関数メンバーとデリゲートでは、1つのパラメーターリストのみが使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="a3946-544">引数またはパラメーターの位置は、引数リストまたはパラメーターリストの前にある引数またはパラメーターの数として定義されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="a3946-545">関数メンバー引数の対応するパラメーターは、次のように設定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="a3946-546">インスタンスコンストラクター、メソッド、インデクサー、およびデリゲートの*argument_list*内の引数:</span><span class="sxs-lookup"><span data-stu-id="a3946-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="a3946-547">固定パラメーターがパラメーターリスト内の同じ位置に出現する位置引数は、そのパラメーターに対応します。</span><span class="sxs-lookup"><span data-stu-id="a3946-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="a3946-548">通常の形式で呼び出されたパラメーター配列を持つ関数メンバーの位置指定引数は、パラメーターリスト内の同じ位置に出現する必要があるパラメーター配列に対応します。</span><span class="sxs-lookup"><span data-stu-id="a3946-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="a3946-549">拡張された形式で呼び出されたパラメーター配列を持つ関数メンバーの位置引数。パラメーターリスト内の同じ位置に固定パラメーターが存在しない場合は、パラメーター配列内の要素に対応します。</span><span class="sxs-lookup"><span data-stu-id="a3946-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="a3946-550">名前付き引数は、パラメーターリスト内の同じ名前のパラメーターに対応します。</span><span class="sxs-lookup"><span data-stu-id="a3946-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="a3946-551">インデクサーの場合、`set` アクセサーを呼び出すときに、代入演算子の右オペランドとして指定された式が、`set` アクセサー宣言の暗黙的な `value` パラメーターに対応します。</span><span class="sxs-lookup"><span data-stu-id="a3946-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="a3946-552">プロパティの場合、`get` アクセサーを呼び出すときに引数はありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="a3946-553">`set` アクセサーを呼び出すと、代入演算子の右オペランドとして指定された式が、`set` アクセサー宣言の暗黙的な `value` パラメーターに対応します。</span><span class="sxs-lookup"><span data-stu-id="a3946-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="a3946-554">ユーザー定義の単項演算子 (変換を含む) の場合、1つのオペランドは演算子宣言の1つのパラメーターに対応します。</span><span class="sxs-lookup"><span data-stu-id="a3946-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="a3946-555">ユーザー定義の二項演算子の場合、左側のオペランドは最初のパラメーターに対応し、右のオペランドは演算子宣言の2番目のパラメーターに対応します。</span><span class="sxs-lookup"><span data-stu-id="a3946-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="a3946-556">引数リストの実行時評価</span><span class="sxs-lookup"><span data-stu-id="a3946-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="a3946-557">関数メンバー呼び出しの実行時の処理中 ([動的なオーバーロード解決のコンパイル時のチェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) では、引数リストの式または変数の参照は、左から右の順に、次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="a3946-558">値パラメーターの場合、引数式が評価され、対応するパラメーター型への暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) が実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="a3946-559">結果の値は、関数メンバー呼び出しの値パラメーターの初期値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="a3946-560">参照または出力パラメーターの場合、変数参照が評価され、結果として得られるストレージの場所が、関数メンバー呼び出しのパラメーターで表されるストレージの場所になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="a3946-561">参照または出力パラメーターとして指定された変数参照が*reference_type*の配列要素である場合は、配列の要素の型がパラメーターの型と同じであることを確認するために、ランタイムチェックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="a3946-562">このチェックが失敗すると、`System.ArrayTypeMismatchException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="a3946-563">メソッド、インデクサー、およびインスタンスコンストラクターは、右端のパラメーターをパラメーター配列 ([パラメーター](classes.md#parameter-arrays)配列) として宣言できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="a3946-564">このような関数メンバーは、適用可能な ([適用可能な関数メンバー](expressions.md#applicable-function-member)) に応じて、通常の形式または拡張された形式で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="a3946-565">パラメーター配列を持つ関数メンバーが通常の形式で呼び出される場合、パラメーター配列に指定された引数は、暗黙的に変換可能な ([暗黙の変換](conversions.md#implicit-conversions)) 1 つの式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="a3946-566">この場合、パラメーター配列は、値パラメーターとまったく同様に動作します。</span><span class="sxs-lookup"><span data-stu-id="a3946-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="a3946-567">パラメーター配列を持つ関数メンバーが拡張された形式で呼び出される場合、呼び出しではパラメーター配列に0個以上の位置引数を指定する必要があります。各引数は、暗黙的に変換できる式 ([暗黙的な変換](conversions.md#implicit-conversions)) です。) をパラメーター配列の要素型にします。</span><span class="sxs-lookup"><span data-stu-id="a3946-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="a3946-568">この場合、呼び出しは、引数の数に対応する長さを持つパラメーター配列型のインスタンスを作成し、指定された引数値を使用して配列インスタンスの要素を初期化し、新しく作成された配列インスタンスを実際のとして使用します。引数.</span><span class="sxs-lookup"><span data-stu-id="a3946-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="a3946-569">引数リストの式は、常に記述された順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="a3946-570">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-570">Thus, the example</span></span>
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
<span data-ttu-id="a3946-571">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-571">produces the output</span></span>
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="a3946-572">配列のクロス分散規則 ([配列の共変性](arrays.md#array-covariance)) では、配列型の値を `A[]` 配列型のインスタンスへの参照にすることが許可されています。これは、`B` から `A`への暗黙的な参照変換が存在する場合に `B[]`です。</span><span class="sxs-lookup"><span data-stu-id="a3946-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="a3946-573">これらの規則により、 *reference_type*の配列要素が参照パラメーターまたは出力パラメーターとして渡されるときに、配列の実際の要素型がパラメーターの要素の型と同じであることを確認するために、ランタイムチェックが必要になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="a3946-574">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-574">In the example</span></span>
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
<span data-ttu-id="a3946-575">`F` の2回目の呼び出しでは、`b` の実際の要素の型が `string` で `object`ではないため、`System.ArrayTypeMismatchException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="a3946-576">パラメーター配列を持つ関数メンバーが拡張された形式で呼び出されると、配列初期化子 (配列[作成式](expressions.md#array-creation-expressions)) を含む配列作成式が展開されたパラメーターの周囲に挿入された場合とまったく同じように、呼び出しが処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="a3946-577">たとえば、次のように宣言したとします。</span><span class="sxs-lookup"><span data-stu-id="a3946-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="a3946-578">メソッドの拡張形式の次の呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="a3946-579">はに正確に対応します。</span><span class="sxs-lookup"><span data-stu-id="a3946-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="a3946-580">特に、パラメーター配列に引数が指定されていない場合は、空の配列が作成されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="a3946-581">対応する省略可能なパラメーターを使用して関数メンバーから引数を省略すると、関数メンバー宣言の既定の引数が暗黙的に渡されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="a3946-582">これらは常に定数であるため、他の引数の評価順序には影響しません。</span><span class="sxs-lookup"><span data-stu-id="a3946-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="a3946-583">型の推論</span><span class="sxs-lookup"><span data-stu-id="a3946-583">Type inference</span></span>

<span data-ttu-id="a3946-584">型引数を指定せずにジェネリックメソッドを呼び出すと、***型推論***プロセスは、その呼び出しの型引数を推論しようとします。</span><span class="sxs-lookup"><span data-stu-id="a3946-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="a3946-585">型の推定が存在することにより、ジェネリックメソッドを呼び出すためにより便利な構文を使用できるようになり、冗長な型情報の指定を避けることができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="a3946-586">たとえば、次のようなメソッド宣言があるとします。</span><span class="sxs-lookup"><span data-stu-id="a3946-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="a3946-587">型引数を明示的に指定しなくても、`Choose` メソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="a3946-588">型の推定により、型引数 `int` と `string` は、メソッドの引数から決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="a3946-589">型の推定は、メソッド呼び出し ([メソッド呼び出し](expressions.md#method-invocations)) のバインド時の処理の一部として発生し、呼び出しのオーバーロードの解決手順の前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="a3946-590">メソッドの呼び出しで特定のメソッドグループが指定され、メソッドの呼び出しの一部として型引数が指定されていない場合、メソッドグループの各ジェネリックメソッドに型の推定が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="a3946-591">型の推定が成功した場合は、推論される型引数を使用して、後続のオーバーロードの解決のための引数の型を決定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="a3946-592">オーバーロードの解決によって、呼び出し元としてジェネリックメソッドが選択された場合、推定される型引数が呼び出しの実際の型引数として使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="a3946-593">特定のメソッドの型の推定が失敗した場合、そのメソッドはオーバーロードの解決に関与しません。</span><span class="sxs-lookup"><span data-stu-id="a3946-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="a3946-594">型の推定の失敗は、それ自体のおよびでは、バインディング時エラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="a3946-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="a3946-595">ただし、オーバーロードの解決によって適用可能なメソッドが見つからない場合は、多くの場合、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="a3946-596">指定された引数の数が、メソッドのパラメーターの数と異なる場合、推論はすぐに失敗します。</span><span class="sxs-lookup"><span data-stu-id="a3946-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="a3946-597">それ以外の場合は、ジェネリックメソッドに次のシグネチャがあると仮定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="a3946-598">フォームのメソッド呼び出しを使用して `M(E1...Em)` 型の推論のタスクでは、呼び出し `M<S1...Sn>(E1...Em)` が有効になるように、型パラメーター `X1...Xn` ごとに一意の型引数 `S1...Sn` を検索します。</span><span class="sxs-lookup"><span data-stu-id="a3946-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="a3946-599">各型パラメーターの推定処理中に、`Xi` は特定の `Si` 型に固定さ*れるか、または*関連付けられた一連の*境界*で*固定*されていません。</span><span class="sxs-lookup"><span data-stu-id="a3946-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="a3946-600">各境界は `T`型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="a3946-601">初期状態では `Xi` それぞれの型変数は、空の境界セットでは修正されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="a3946-602">型の推定はフェーズで行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-602">Type inference takes place in phases.</span></span> <span data-ttu-id="a3946-603">各フェーズでは、前のフェーズの結果に基づいて、より多くの型変数の型引数を推論しようとします。</span><span class="sxs-lookup"><span data-stu-id="a3946-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="a3946-604">最初のフェーズでは、境界の初期推論を行います。2番目のフェーズでは、型の変数を特定の型に修正し、さらに境界を推測します。</span><span class="sxs-lookup"><span data-stu-id="a3946-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="a3946-605">2番目のフェーズは、繰り返しが必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="a3946-606">*注:* 型の推定は、ジェネリックメソッドが呼び出された場合にのみ行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="a3946-607">メソッドグループの変換に対する型の推論については、「[メソッドグループの変換の型の推定](expressions.md#type-inference-for-conversion-of-method-groups)」で説明されています。また、式のセットの最適な共通型の検索については、「[式セットの最適な共通型の検索](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)」で説明されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="a3946-608">最初のフェーズ</span><span class="sxs-lookup"><span data-stu-id="a3946-608">The first phase</span></span>

<span data-ttu-id="a3946-609">`Ei`のメソッド引数ごとに、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="a3946-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="a3946-610">`Ei` が匿名関数の場合は、*明示的なパラメーター型の推論*([明示的なパラメーター型](expressions.md#explicit-parameter-type-inferences)の推論) が `Ei` からに作成され `Ti`</span><span class="sxs-lookup"><span data-stu-id="a3946-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="a3946-611">それ以外の場合、`Ei` に `U` 型があり、`xi` が値パラメーターである場合は、`U`*から*`Ti`*に* *下限の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="a3946-612">それ以外の場合、`Ei` に `U` 型があり、`xi` が `ref` または `out` のパラメーターである場合、`U`*から*`Ti`*に* *正確な推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="a3946-613">それ以外の場合、この引数に対して推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="a3946-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="a3946-614">2番目のフェーズ</span><span class="sxs-lookup"><span data-stu-id="a3946-614">The second phase</span></span>

<span data-ttu-id="a3946-615">2番目のフェーズは、次のように進行します。</span><span class="sxs-lookup"><span data-stu-id="a3946-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="a3946-616">すべての未修正*の型の*変数は、([依存](expressions.md#dependence)関係)*に依存*しないすべての `Xj` を修正 ([修正](expressions.md#fixing)) `Xi` ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="a3946-617">このような型の変数が存在しない場合は、`Xi` すべての未*修正*の型変数が固定され、次*のすべての*要素が保持されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="a3946-618">に依存する型変数 `Xj` が少なくとも1つあり `Xi`</span><span class="sxs-lookup"><span data-stu-id="a3946-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="a3946-619">`Xi` に空でない境界セットがあります</span><span class="sxs-lookup"><span data-stu-id="a3946-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="a3946-620">このような型の変数が存在*せず、まだ修正*されていない型の変数がある場合、型の推定は失敗します。</span><span class="sxs-lookup"><span data-stu-id="a3946-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="a3946-621">それ以外の場合は、それ以上の非修正型変数が存在しない場合 *、型の*推定は成功します。</span><span class="sxs-lookup"><span data-stu-id="a3946-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="a3946-622">それ以外の場合、*出力の種類*([出力](expressions.md#output-types)型) には固定されていない型の変数*が含まれ*ていますが、*入力*の型 ([入力型](expressions.md#input-types)) には `Xj` の出力*型の推論*([出力](expressions.md#output-type-inferences)型の推論) が `Ei`*から*`Ti`*に*対して行われるので、すべての引数に対応するパラメーターの型 `Ti` `Ei` ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="a3946-623">次に、2番目のフェーズが繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="a3946-624">入力の種類</span><span class="sxs-lookup"><span data-stu-id="a3946-624">Input types</span></span>

<span data-ttu-id="a3946-625">`E` がメソッドグループまたは暗黙的に型指定された匿名関数で、`T` がデリゲート型または式ツリー型である場合、`T` のすべてのパラメーターの型は `T`*型*の `E` の*入力型*になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="a3946-626">出力型</span><span class="sxs-lookup"><span data-stu-id="a3946-626">Output types</span></span>

<span data-ttu-id="a3946-627">`E` がメソッドグループまたは匿名関数で、`T` がデリゲート型または式ツリー型である場合、`T` の戻り値の型は `T`*型*の `E`*の出力型*になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="a3946-628">依存</span><span class="sxs-lookup"><span data-stu-id="a3946-628">Dependence</span></span>

<span data-ttu-id="a3946-629">固定されていない型変数 `Xi` は、固定されてい*ない型変数* *に直接依存*しています。 `Xj` 型の引数 `Tk` `Ek` では、型 `Xj` の `Ek` の*入力型*`Tk` に発生し、`Xi` 型の `Ek` の*出力型*で発生する場合があります。`Tk`</span><span class="sxs-lookup"><span data-stu-id="a3946-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="a3946-630">`Xj` が `Xi`*に直接依存*している場合、または `Xi` が `Xk`*に直接*依存しており、`Xk` が `Xj`*に*依存している場合、`Xj` は `Xi`*に依存*します。</span><span class="sxs-lookup"><span data-stu-id="a3946-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="a3946-631">したがって、"依存関係" は推移的ですが、"直接に依存している" の再帰クロージャではありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="a3946-632">出力の型の推論</span><span class="sxs-lookup"><span data-stu-id="a3946-632">Output type inferences</span></span>

<span data-ttu-id="a3946-633">*出力型の推論*は、次のように `T` 型*に*`E` 式*から*実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="a3946-634">`E` が、推論された戻り値の型 `U` ([推定戻り値の型](expressions.md#inferred-return-type)) の匿名関数で、`T` が戻り値の型が `Tb`であるデリゲート型または式ツリー型である場合は、*下限の推論*([下限](expressions.md#lower-bound-inferences)の推論) が `U`*から*`Tb`*に*作成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="a3946-635">それ以外の場合、`E` がメソッドグループで、`T` がパラメーター型 `T1...Tk` および戻り値の型 `Tb`を持つデリゲート型または式ツリー型であり、型 `E` のオーバーロード解決では、戻り値の型が *`T1...Tk` の 1*つのメソッドが生成 *され*ます。`U``U``Tb`</span><span class="sxs-lookup"><span data-stu-id="a3946-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="a3946-636">それ以外の場合、`E` が `U`型の式である場合、`U`*から*`T`*に* *下限の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="a3946-637">それ以外の場合、推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="a3946-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="a3946-638">明示的なパラメーター型の推論</span><span class="sxs-lookup"><span data-stu-id="a3946-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="a3946-639">*明示的なパラメーター型の推論*は、次のように `T` 型*に*`E` 式*から*作成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="a3946-640">`E` がパラメーターの `U1...Uk` 型を持つ明示的に型指定された匿名関数であり、`T` が `V1...Vk` パラメーターの型を持つデリゲート型または式ツリー型である場合、各 `Ui` に対して*正確な推論*([正確](expressions.md#exact-inferences)な推論) が `Ui`*から*対応する `Vi`*に*行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="a3946-641">正確な推論</span><span class="sxs-lookup"><span data-stu-id="a3946-641">Exact inferences</span></span>

<span data-ttu-id="a3946-642">`U` 型*から*`V` 型への*厳密な推論*は、次のよう*に*行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="a3946-643">`V` が固定*されていない `Xi` の*1 つである場合、`U` は `Xi`の完全な境界のセットに追加されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="a3946-644">それ以外の場合は、次のいずれかのケースに該当するかどうかを確認することによって、`V1...Vk` を設定し `U1...Uk` を決定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="a3946-645">`V` は配列型で `V1[...]` `U` は同じランクの配列型 `U1[...]` です。</span><span class="sxs-lookup"><span data-stu-id="a3946-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="a3946-646">`V` は `V1?` 型で、`U` は型です `U1?`</span><span class="sxs-lookup"><span data-stu-id="a3946-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="a3946-647">`V` は構築された型 `C<V1...Vk>`で、`U` は構築された型です `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="a3946-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="a3946-648">これらのいずれかのケースに該当する場合は、各 `Ui`*から*対応する `Vi`*に* *正確な推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="a3946-649">それ以外の場合、推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="a3946-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="a3946-650">下限の推論</span><span class="sxs-lookup"><span data-stu-id="a3946-650">Lower-bound inferences</span></span>

<span data-ttu-id="a3946-651">型 `U` から型 `V` へ*の* *下限の推論*は、次のよう*に*行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="a3946-652">`V` が未修正の `Xi` の1つである*場合、`U`* は `Xi`の下限のセットに追加されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="a3946-653">それ以外の場合、`V` が `V1?`型で、`U` が `U1?` 型である場合は、`U1` から `V1`に下限の推論が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="a3946-654">それ以外の場合は、次のいずれかのケースに該当するかどうかを確認することによって、`U1...Uk` を設定し `V1...Vk` を決定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="a3946-655">`V` は配列型で `V1[...]` `U` は配列型 `U1[...]` (または、有効な基本型が `U1[...]`である型パラメーター) です。</span><span class="sxs-lookup"><span data-stu-id="a3946-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="a3946-656">`V` が `IEnumerable<V1>`、`ICollection<V1>` または `IList<V1>` のいずれかで、`U` が1次元配列型 `U1[]`(または、有効な基本型が `U1[]`である型パラメーター) です。</span><span class="sxs-lookup"><span data-stu-id="a3946-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="a3946-657">`V` は、構築され `U` たクラス、構造体、インターフェイス、またはデリゲート型 `C<V1...Vk>` で `C<U1...Uk>` あり、`U` が型パラメーターの場合、その有効な基本クラスまたはその有効なインターフェイスセットのメンバーが (直接または間接的に) (直接または間接的に) (直接または間接的に) `C<U1...Uk>`を実装 (直接または間接的に) する</span><span class="sxs-lookup"><span data-stu-id="a3946-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="a3946-658">("一意性" の制限とは、`C<T> {} class U: C<X>, C<Y> {}`ケースインターフェイスでは、`U1` を `X` または `Y`することができるため、`U` から `C<T>` に推論するときに推論が行われないことを意味します)。</span><span class="sxs-lookup"><span data-stu-id="a3946-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="a3946-659">これらのいずれかのケースに該当する場合は、次のように各 `Ui`*から*対応する `Vi`*に*推論が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="a3946-660">`Ui` が参照型でないことがわかっている場合は、*正確な推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="a3946-661">それ以外の場合、`U` が配列型である場合は、*下限の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="a3946-662">それ以外の場合、`V` が `C<V1...Vk>` 場合、推論は `C`の i 番目の型パラメーターに依存します。</span><span class="sxs-lookup"><span data-stu-id="a3946-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="a3946-663">共変の場合は、*下限の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="a3946-664">反変の場合は、*上限の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="a3946-665">不変である場合は、*正確な推定*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="a3946-666">それ以外の場合、推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="a3946-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="a3946-667">上限の推論</span><span class="sxs-lookup"><span data-stu-id="a3946-667">Upper-bound inferences</span></span>

<span data-ttu-id="a3946-668">型 `U` から型 `V` へ*の* *上限の推論*は、次のよう*に*行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="a3946-669">`V` が固定*されていない `Xi` の*1 つである場合、`U` が `Xi`の上限のセットに追加されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="a3946-670">それ以外の場合は、次のいずれかのケースに該当するかどうかを確認することによって、`V1...Vk` を設定し `U1...Uk` を決定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="a3946-671">`U` は配列型で `U1[...]` `V` は同じランクの配列型 `V1[...]` です。</span><span class="sxs-lookup"><span data-stu-id="a3946-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="a3946-672">`U` は `IEnumerable<Ue>`、`ICollection<Ue>` または `IList<Ue>` のいずれかで、`V` は1次元配列型です `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="a3946-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="a3946-673">`U` は `U1?` 型で、`V` は型です `V1?`</span><span class="sxs-lookup"><span data-stu-id="a3946-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="a3946-674">`U` は、構築されたクラス、構造体、インターフェイス、またはデリゲート型 `C<U1...Uk>` であり、`V` はクラス、構造体、インターフェイス、またはデリゲート型です。これは、(直接または間接的に) を継承するか、または一意の型を (直接または間接的に) 実装します `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="a3946-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="a3946-675">("一意性" の制限は、`interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`がある場合に `C<U1>` から `V<Q>`に推論するときに推論が行われないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="a3946-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="a3946-676">`U1` から `X<Q>` または `Y<Q>`に推論は行われません)。</span><span class="sxs-lookup"><span data-stu-id="a3946-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="a3946-677">これらのいずれかのケースに該当する場合は、次のように各 `Ui`*から*対応する `Vi`*に*推論が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="a3946-678">`Ui` が参照型でないことがわかっている場合は、*正確な推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="a3946-679">それ以外の場合、`V` が配列型であれば、*上限の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="a3946-680">それ以外の場合、`U` が `C<U1...Uk>` 場合、推論は `C`の i 番目の型パラメーターに依存します。</span><span class="sxs-lookup"><span data-stu-id="a3946-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="a3946-681">共変の場合は、*上限の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="a3946-682">反変の場合は、*下限の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="a3946-683">不変である場合は、*正確な推定*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="a3946-684">それ以外の場合、推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="a3946-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="a3946-685">写真の</span><span class="sxs-lookup"><span data-stu-id="a3946-685">Fixing</span></span>

<span data-ttu-id="a3946-686">一連の境界を持つ固定されてい*ない型変数*`Xi` は、次のように*固定*されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="a3946-687">*候補の種類*のセット `Uj`、`Xi`の一連の境界内のすべての型のセットとして開始されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="a3946-688">次に、それぞれのバインドされている `Xi` を確認します。 `Xi` の完全バインド `U` については、`U` と同じではないすべての型 `Uj` を候補セットから削除します。</span><span class="sxs-lookup"><span data-stu-id="a3946-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="a3946-689">`Xi` の下限 `U` ごとに、`U` からの暗黙の変換が*行われていない*すべての型 `Uj` 候補セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="a3946-690">`U` への暗黙的な変換が*行われていない*すべての型 `Uj` `Xi` の上限 `U` は、候補セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="a3946-691">他の候補の型の中に、他のすべての候補の型への暗黙の型変換が存在する `Uj` 一意の型 `V` がある場合、`Xi` は `V`に固定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="a3946-692">それ以外の場合、型の推定は失敗します。</span><span class="sxs-lookup"><span data-stu-id="a3946-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="a3946-693">推定される戻り値の型</span><span class="sxs-lookup"><span data-stu-id="a3946-693">Inferred return type</span></span>

<span data-ttu-id="a3946-694">匿名関数 `F` の推定戻り値の型は、型の推定およびオーバーロードの解決の際に使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="a3946-695">推論された戻り値の型は、すべてのパラメーター型が明示的に指定されているか、匿名関数の変換によって指定されるか、または外側のジェネリックの型の推論中に推論されるため、すべてのパラメーター型がわかっている場合にのみ特定できます。メソッドの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="a3946-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="a3946-696">推論される***結果の型***は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="a3946-697">`F` の本体が型を持つ*式*である場合、`F` の推定結果型はその式の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="a3946-698">`F` の本体が*ブロック*で、ブロックの `return` ステートメント内の式のセットが最適な共通型 `T` ([一連の式の中で最適な共通型を見つける](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) の場合、`F` の推定結果型は `T`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="a3946-699">それ以外の場合、`F`に対して結果の型を推論することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="a3946-700">推論される***戻り値の型***は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="a3946-701">`F` が async で、`F` の本体が nothing ([式の分類](expressions.md#expression-classifications)) として分類された式、または return ステートメントに式が含まれていないステートメントブロックのいずれかである場合、推論される戻り値の型は `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="a3946-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="a3946-702">`F` が async で、推論された結果型が `T`の場合、推論される戻り値の型は `System.Threading.Tasks.Task<T>`です。</span><span class="sxs-lookup"><span data-stu-id="a3946-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="a3946-703">`F` が非同期ではなく、推論された結果型 `T`の場合、推論される戻り値の型は `T`です。</span><span class="sxs-lookup"><span data-stu-id="a3946-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="a3946-704">それ以外の場合、`F`の戻り値の型を推論することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="a3946-705">匿名関数を含む型推論の例として、`System.Linq.Enumerable` クラスで宣言された `Select` 拡張メソッドを考えてみます。</span><span class="sxs-lookup"><span data-stu-id="a3946-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

<span data-ttu-id="a3946-706">`System.Linq` 名前空間が `using` 句を使用してインポートされ、`string`型の `Name` プロパティを持つクラス `Customer` された場合、`Select` メソッドを使用して顧客の一覧の名前を選択できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="a3946-707">`Select` の拡張メソッドの呼び出し ([拡張メソッド](expressions.md#extension-method-invocations)の呼び出し) は、静的メソッドの呼び出しへの呼び出しを書き換えて処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="a3946-708">型引数が明示的に指定されていないため、型引数を推論するために型の推定が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="a3946-709">まず、`customers` 引数は、`Customer`する `T` を推論する `source` パラメーターに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="a3946-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="a3946-710">次に、上記で説明した匿名関数型推論プロセスを使用して、`c` に型 `Customer`を指定し、式 `c.Name` を `selector` パラメーターの戻り値の型に関連付けて、`S` する `string`を推論します。</span><span class="sxs-lookup"><span data-stu-id="a3946-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="a3946-711">したがって、呼び出しはと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="a3946-712">結果は `IEnumerable<string>`型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="a3946-713">次の例では、匿名関数型推論によって、ジェネリックメソッド呼び出しの引数間で型情報を "flow" できるようにする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="a3946-714">次のメソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="a3946-715">呼び出しの型の推定:</span><span class="sxs-lookup"><span data-stu-id="a3946-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="a3946-716">次のように処理を進めます。最初に、引数 `"1:15:30"` が `value` パラメーターに関連付けられ、`string`する `X` を推論します。</span><span class="sxs-lookup"><span data-stu-id="a3946-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="a3946-717">次に、最初の匿名関数 `s`のパラメーターに推論される型 `string`を指定します。また、式 `TimeSpan.Parse(s)` は `f1`の戻り値の型に関連付けられ、`Y` する `System.TimeSpan`を推論します。</span><span class="sxs-lookup"><span data-stu-id="a3946-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="a3946-718">最後に、2番目の匿名関数 `t`のパラメーターには、推論される型 `System.TimeSpan`が与えられ、式 `t.TotalSeconds` は `f2`の戻り値の型に関連付けられ、`Z` する `double`が推論されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="a3946-719">したがって、呼び出しの結果は `double`型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="a3946-720">メソッドグループの変換の型推論</span><span class="sxs-lookup"><span data-stu-id="a3946-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="a3946-721">ジェネリックメソッドの呼び出しと同様に、ジェネリックメソッドを含むメソッドグループ `M` が特定のデリゲート `D` 型に変換される場合 ([メソッドグループ変換](conversions.md#method-group-conversions)) にも、型推論を適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="a3946-722">指定されたメソッド</span><span class="sxs-lookup"><span data-stu-id="a3946-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="a3946-723">また、メソッドグループ `M` デリゲート型に割り当てられ `D` 型引数を検索して式を `S1...Sn` します。</span><span class="sxs-lookup"><span data-stu-id="a3946-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="a3946-724">は、`D`と互換性のある ([デリゲート宣言](delegates.md#delegate-declarations)) になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="a3946-725">ジェネリックメソッド呼び出しの型推論アルゴリズムとは異なり、この例では引数の*型*のみが存在し、引数の*式*はありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="a3946-726">特に、匿名関数は存在しないため、推論の複数のフェーズを行う必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="a3946-727">代わりに、すべての `Xi` が未修正*と見なされ、`D`* の各引数の型 `Uj`*から*`M`の対応するパラメーターの型 `Tj`*に* *下限の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="a3946-728">いずれかの `Xi` 境界が見つからなかった場合、型の推定は失敗します。</span><span class="sxs-lookup"><span data-stu-id="a3946-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="a3946-729">それ以外の場合は、すべての `Xi` が対応する `Si`に*固定*されます。これは、型の推定の結果です。</span><span class="sxs-lookup"><span data-stu-id="a3946-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="a3946-730">一連の式の中で最もよく使われる型の検索</span><span class="sxs-lookup"><span data-stu-id="a3946-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="a3946-731">場合によっては、一連の式に対して共通の型を推論する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="a3946-732">特に、暗黙的に型指定された配列の要素型と*ブロック*本体を持つ匿名関数の戻り値の型は、この方法で見つかります。</span><span class="sxs-lookup"><span data-stu-id="a3946-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="a3946-733">直感的に言えば、一連の式を指定すると `E1...Em` この推論は、メソッドを呼び出すことと同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="a3946-734">`Ei` を引数として使用します。</span><span class="sxs-lookup"><span data-stu-id="a3946-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="a3946-735">より正確には、推定は、固定されてい*ない型変数*`X`で開始されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="a3946-736">その後、各 `Ei`*から*`X`*に*対して、出力の*型の推論*が行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="a3946-737">最後に、`X` が*修正*され、成功した場合、結果として得られる型 `S` が、式に対して生成される最適な共通型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="a3946-738">このような `S` が存在しない場合、式には最適な共通型がありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="a3946-739">オーバーロードの解決</span><span class="sxs-lookup"><span data-stu-id="a3946-739">Overload resolution</span></span>

<span data-ttu-id="a3946-740">オーバーロードの解決は、引数リストと候補となる関数メンバーのセットを呼び出すための最適な関数メンバーを選択するためのバインディング時の機構です。</span><span class="sxs-lookup"><span data-stu-id="a3946-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="a3946-741">オーバーロードの解決では、内C#の次の異なるコンテキストで呼び出す関数メンバーを選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="a3946-742">*Invocation_expression* ([メソッド呼び出し](expressions.md#method-invocations)) で指定されたメソッドの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="a3946-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="a3946-743">*Object_creation_expression*で指定されたインスタンスコンストラクターの呼び出し ([オブジェクト作成式](expressions.md#object-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="a3946-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="a3946-744">*Element_access* ([要素アクセス](expressions.md#element-access)) を使用したインデクサーアクセサーの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="a3946-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="a3946-745">式で参照されている定義済みまたはユーザー定義の演算子の呼び出し ([単項演算子のオーバーロードの解決](expressions.md#unary-operator-overload-resolution)と[二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="a3946-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="a3946-746">これらの各コンテキストでは、上記のセクションで詳しく説明したように、候補となる関数メンバーのセットと引数のリストが独自の一意の方法で定義されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="a3946-747">たとえば、メソッド呼び出しの候補のセットに `override` ([メンバー検索](expressions.md#member-lookup)) とマークされたメソッドは含まれません。派生クラスのメソッドが適用可能な場合 ([メソッド呼び出し](expressions.md#method-invocations))、基底クラスのメソッドは候補になりません。</span><span class="sxs-lookup"><span data-stu-id="a3946-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="a3946-748">候補の関数メンバーと引数リストが識別されると、最適な関数メンバーの選択は常に同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="a3946-749">適用候補の関数メンバーのセットが見つかった場合、そのセット内の最適な関数メンバーが配置されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="a3946-750">セットに含まれる関数メンバーが1つだけの場合は、その関数メンバーが最適な関数メンバーになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="a3946-751">それ以外の場合、最適な関数メンバーは、指定された引数リストに対する他のすべての関数メンバーと比較して適切な1つの関数メンバーになります。つまり、各関数メンバーは、[より優れた関数](expressions.md#better-function-member)メンバーの規則を使用して他のすべての関数メンバーと比較されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="a3946-752">他のすべての関数メンバーよりも適切な関数メンバーが1つしかない場合は、関数メンバー呼び出しがあいまいになり、バインディング時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-753">以下のセクションでは、***適用可能な関数メンバー***と、***より優れた関数メンバー***という用語の正確な意味を定義します。</span><span class="sxs-lookup"><span data-stu-id="a3946-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="a3946-754">適用可能な関数メンバー</span><span class="sxs-lookup"><span data-stu-id="a3946-754">Applicable function member</span></span>

<span data-ttu-id="a3946-755">関数メンバーは、次のすべてに該当する場合に、引数リストに対して***適用可能な関数メンバー***と呼ばれます `A`。</span><span class="sxs-lookup"><span data-stu-id="a3946-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="a3946-756">`A` の各引数は、[対応するパラメーター](expressions.md#corresponding-parameters)で説明されているように、関数メンバー宣言内のパラメーターに対応します。引数が対応していないパラメーターは、省略可能なパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="a3946-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="a3946-757">`A`の引数ごとに、引数のパラメーター渡しモード (つまり、value、`ref`、または `out`) は、対応するパラメーターのパラメーター渡しモードと同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="a3946-758">値パラメーターまたはパラメーター配列の場合、引数から対応するパラメーターの型への暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) が存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="a3946-759">`ref` または `out` パラメーターの場合、引数の型は、対応するパラメーターの型と同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="a3946-760">結局のところ、`ref` または `out` パラメーターは渡された引数のエイリアスです。</span><span class="sxs-lookup"><span data-stu-id="a3946-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="a3946-761">パラメーター配列を含む関数メンバーでは、関数メンバーが上記の規則によって適用される場合、その関数メンバーは通常の***形式***で適用されると言われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="a3946-762">パラメーター配列を含む関数メンバーを通常の形式で適用できない場合は、その関数メンバーが拡張された***形式***で適用される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="a3946-763">拡張されたフォームを作成するには、関数メンバー宣言内のパラメーター配列をパラメーター配列の要素型の0個以上の値パラメーターに置き換えます。これにより、引数リスト内の引数の数がパラメーターの合計数と一致 `A` ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="a3946-764">`A` の引数が関数メンバー宣言の固定パラメーター数よりも小さい場合、関数メンバーの拡張形式は構築できないため、適用できません。</span><span class="sxs-lookup"><span data-stu-id="a3946-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="a3946-765">それ以外の場合、拡張されたフォームは、引数のパラメーター渡しモードが対応するパラメーターのパラメーター渡しモードと同じ `A` の各引数に対して適用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="a3946-766">拡張によって作成された固定値パラメーターまたは値パラメーターの場合、引数の型から対応するパラメーターの型への暗黙の変換 ([暗黙](conversions.md#implicit-conversions)の変換) が存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="a3946-767">`ref` または `out` パラメーターの場合、引数の型は、対応するパラメーターの型と同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="a3946-768">より優れた関数メンバー</span><span class="sxs-lookup"><span data-stu-id="a3946-768">Better function member</span></span>

<span data-ttu-id="a3946-769">より優れた関数メンバーを決定するために、削除された引数リストは、元の引数リストに出現する順序で引数式だけを含むように構築されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="a3946-770">各候補関数メンバーのパラメーターリストは、次のように構築されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="a3946-771">拡張されたフォームは、関数メンバーが展開されたフォームでのみ適用された場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="a3946-772">対応する引数のない省略可能なパラメーターは、パラメーターリストから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="a3946-773">パラメーターは、引数リスト内の対応する引数と同じ位置に出現するように順序が変わります。</span><span class="sxs-lookup"><span data-stu-id="a3946-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="a3946-774">引数のリストに `A` 引数の式 `{E1, E2, ..., En}` と、適用可能な2つの関数メンバー `Mp` `Mq` およびパラメーターの型 `{P1, P2, ..., Pn}` と `{Q1, Q2, ..., Qn}`に対して指定されている場合、`Mp` は `Mq` よりも***適切な関数メンバー***として定義されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="a3946-775">引数ごとに、`Ex` から `Qx` への暗黙的な変換は `Ex` から `Px`への暗黙的な変換よりも適切ではありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="a3946-776">少なくとも1つの引数については、`Ex` から `Px` への変換は `Ex` から `Qx`への変換よりも優れています。</span><span class="sxs-lookup"><span data-stu-id="a3946-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="a3946-777">この評価を実行する場合、展開された形式で `Mp` または `Mq` を適用できる場合、`Px` または `Qx` は、拡張された形式のパラメーターリストのパラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="a3946-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="a3946-778">パラメーターの型のシーケンス `{P1, P2, ..., Pn}` および `{Q1, Q2, ..., Qn}` が等しい場合 (つまり、各 `Pi` に対応する `Qi`への id 変換がある場合)、より適切な関数メンバーを決定するために、次のような相互に関連する規則が順番に適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="a3946-779">`Mp` が非ジェネリックメソッドで `Mq` がジェネリックメソッドである場合、`Mp` は `Mq`よりも優れています。</span><span class="sxs-lookup"><span data-stu-id="a3946-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="a3946-780">それ以外の場合、`Mp` が通常の形式で適用可能で `Mq` `params` 配列を持ち、その拡張フォームにのみ適用できる場合、`Mp` は `Mq`よりも優れています。</span><span class="sxs-lookup"><span data-stu-id="a3946-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="a3946-781">それ以外の場合、`Mp` が `Mq`よりも宣言されたパラメーターを持つ場合、`Mp` は `Mq`よりも優れています。</span><span class="sxs-lookup"><span data-stu-id="a3946-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="a3946-782">これは、両方のメソッドが `params` 配列を持ち、拡張されたフォームにのみ適用できる場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="a3946-783">それ以外の場合、`Mp` のすべてのパラメーターに対応する引数があるのに対し、既定の引数は `Mq` で少なくとも1つの省略可能なパラメーターの代わりに使用する必要があり、`Mp` は `Mq`よりも優れています。</span><span class="sxs-lookup"><span data-stu-id="a3946-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="a3946-784">それ以外の場合、`Mp` のパラメーターの型が `Mq`よりも多い場合、`Mp` は `Mq`よりも優れています。</span><span class="sxs-lookup"><span data-stu-id="a3946-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="a3946-785">`{R1, R2, ..., Rn}` と `{S1, S2, ..., Sn}` は、`Mp` と `Mq`のインスタンスおよび展開されていないパラメーターの種類を表します。</span><span class="sxs-lookup"><span data-stu-id="a3946-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="a3946-786">`Mp`のパラメーターの型は、各パラメーターについては `Mq`よりも具体的で、`Rx` が `Sx`よりも限定的ではなく、少なくとも1つのパラメーターについては `Rx` よりも `Sx`よりも具体的です。</span><span class="sxs-lookup"><span data-stu-id="a3946-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="a3946-787">型パラメーターが非型パラメーターよりも限定的ではありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="a3946-788">1つ以上の型引数がより限定的で、型引数がもう一方の型引数よりも限定的でない場合、構築された型は再帰的に (型引数の数が同じである) 別の構築型よりも固有になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="a3946-789">配列型は、1つ目の要素の型が2番目の要素の型よりも固有である場合、(次元数が同じ) 別の配列型よりも具体的です。</span><span class="sxs-lookup"><span data-stu-id="a3946-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="a3946-790">それ以外の場合、一方のメンバーがリフトされていない演算子で、もう一方がリフトされた演算子の場合、リフトされていない1つの方が適しています。</span><span class="sxs-lookup"><span data-stu-id="a3946-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="a3946-791">それ以外の場合は、どちらの関数メンバーも適していません。</span><span class="sxs-lookup"><span data-stu-id="a3946-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="a3946-792">式からの変換の向上</span><span class="sxs-lookup"><span data-stu-id="a3946-792">Better conversion from expression</span></span>

<span data-ttu-id="a3946-793">式の `E` から型 `T1`に変換する暗黙的な `C1` 変換と、式の `E` を型 `T2`に変換する暗黙的な変換 `C2` を指定した場合、`C1` が `C2` と完全に一致せず、次の少なくとも1つのが保持されていると、`E` よりも***変換が適切***になります。`T2`</span><span class="sxs-lookup"><span data-stu-id="a3946-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="a3946-794">`E` `T1` (正確に[一致する式](expressions.md#exactly-matching-expression)) と完全に一致します。</span><span class="sxs-lookup"><span data-stu-id="a3946-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="a3946-795">`T1` は `T2` よりも適切な変換ターゲットです ([変換ターゲットの方が適し](expressions.md#better-conversion-target)ています)。</span><span class="sxs-lookup"><span data-stu-id="a3946-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="a3946-796">正確に一致する式</span><span class="sxs-lookup"><span data-stu-id="a3946-796">Exactly matching Expression</span></span>

<span data-ttu-id="a3946-797">式 `E` と型 `T`が指定されている場合、次のいずれかの条件を満たしている場合は `T` に正確に一致 `E` ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="a3946-798">`E` には `S`型があり、`S` からに id 変換が存在し `T`</span><span class="sxs-lookup"><span data-stu-id="a3946-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="a3946-799">`E` は匿名関数 `T` であり、デリゲート型 `D` または式ツリー型 `Expression<D>` で、次のいずれかが保持されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="a3946-800">`D` ([推定戻り値の型](expressions.md#inferred-return-type)) のパラメーターリストのコンテキストで `E` に対して推論された戻り値の型 `X` 存在します。また、`X` からの戻り値の型への id 変換が存在し `D`</span><span class="sxs-lookup"><span data-stu-id="a3946-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="a3946-801">`E` が非同期ではなく、`D` の戻り値の型が `Y` または `E` が async で、`D` の戻り値の型が `Task<Y>`である場合、次のいずれかが保持されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="a3946-802">`E` の本体は、正確に一致する式です `Y`</span><span class="sxs-lookup"><span data-stu-id="a3946-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="a3946-803">`E` の本体は、すべての return ステートメントが正確に一致する式を返すステートメントブロックです `Y`</span><span class="sxs-lookup"><span data-stu-id="a3946-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="a3946-804">変換ターゲットの向上</span><span class="sxs-lookup"><span data-stu-id="a3946-804">Better conversion target</span></span>

<span data-ttu-id="a3946-805">`T1` と `T2`の2つの異なる型を指定した場合、`T2` から `T1` への暗黙的な変換が存在せず、少なくとも次のいずれかが保持されている場合、`T1` は `T2` よりも変換ターゲットとして適しています。</span><span class="sxs-lookup"><span data-stu-id="a3946-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="a3946-806">`T1` から `T2` への暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="a3946-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="a3946-807">`T1` はデリゲート型 `D1` または式ツリー型 `Expression<D1>`のいずれかで、`T2` はデリゲート型 `D2` または式ツリー型 `Expression<D2>`、`D1` には戻り値の型 `S1`、次のいずれかが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="a3946-808">`D2` は void を返します。</span><span class="sxs-lookup"><span data-stu-id="a3946-808">`D2` is void returning</span></span>
   * <span data-ttu-id="a3946-809">`D2` に `S2`戻り値の型があり、`S1` がより適切な変換ターゲットです `S2`</span><span class="sxs-lookup"><span data-stu-id="a3946-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="a3946-810">`T1` は `Task<S1>`、`T2` は `Task<S2>`、`S1` はより優れた変換ターゲットです `S2`</span><span class="sxs-lookup"><span data-stu-id="a3946-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="a3946-811">`T1` は、`S1` が符号付き整数型であり、`T2` が符号なし整数型である `S2` または `S2?` である場合に `S1` または `S1?` ます。`S2`</span><span class="sxs-lookup"><span data-stu-id="a3946-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="a3946-812">具体的な内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-812">Specifically:</span></span>
   * <span data-ttu-id="a3946-813">`S1` は `sbyte` で、`S2` は `byte`、`ushort`、`uint`、または `ulong`</span><span class="sxs-lookup"><span data-stu-id="a3946-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="a3946-814">`S1` は `short` で、`S2` は `ushort`、`uint`、または `ulong`</span><span class="sxs-lookup"><span data-stu-id="a3946-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="a3946-815">`S1` は `int` で `S2` は `uint`、または `ulong`</span><span class="sxs-lookup"><span data-stu-id="a3946-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="a3946-816">`S1` は `long` で `S2` は `ulong`</span><span class="sxs-lookup"><span data-stu-id="a3946-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="a3946-817">オーバーロード (ジェネリッククラスでの)</span><span class="sxs-lookup"><span data-stu-id="a3946-817">Overloading in generic classes</span></span>

<span data-ttu-id="a3946-818">宣言されたシグネチャは一意である必要がありますが、型引数の代入によって同じシグネチャが生成される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="a3946-819">このような場合、上記のオーバーロードの解決規則によって、最も限定的なメンバーが選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="a3946-820">次の例は、この規則に従って有効で無効なオーバーロードを示しています。</span><span class="sxs-lookup"><span data-stu-id="a3946-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="a3946-821">動的なオーバーロードの解決のコンパイル時チェック</span><span class="sxs-lookup"><span data-stu-id="a3946-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="a3946-822">動的にバインドされたほとんどの操作では、コンパイル時に解決できる候補のセットは不明です。</span><span class="sxs-lookup"><span data-stu-id="a3946-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="a3946-823">ただし、場合によっては、候補セットはコンパイル時に認識されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="a3946-824">動的引数を使用した静的メソッド呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="a3946-825">受信側が動的な式でないインスタンスメソッド呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="a3946-826">受信側が動的な式ではないインデクサー呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="a3946-827">動的引数を使用したコンストラクター呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="a3946-828">このような場合、実行時に適用される可能性があるかどうかを確認するために、各候補に対して限定的なコンパイル時チェックが実行されます。この確認は、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="a3946-829">部分型推論: `dynamic` 型の引数に直接または間接的に依存していない型引数は、[型推論](expressions.md#type-inference)の規則を使用して推論されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="a3946-830">その他の型引数は不明です。</span><span class="sxs-lookup"><span data-stu-id="a3946-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="a3946-831">部分適用性の確認: 適用性は、[適用可能な関数メンバー](expressions.md#applicable-function-member)に従ってチェックされますが、型が不明のパラメーターは無視されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="a3946-832">このテストに合格しなかった場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="a3946-833">関数メンバーの呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-833">Function member invocation</span></span>

<span data-ttu-id="a3946-834">ここでは、特定の関数メンバーを呼び出すために実行時に実行されるプロセスについて説明します。</span><span class="sxs-lookup"><span data-stu-id="a3946-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="a3946-835">バインド時のプロセスでは、呼び出される特定のメンバーが既に決定されていることを前提としています。これは、候補となる関数メンバーのセットにオーバーロードの解決を適用することによって行われる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="a3946-836">呼び出しプロセスを記述する目的で、関数メンバーは次の2つのカテゴリに分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="a3946-837">静的関数のメンバー。</span><span class="sxs-lookup"><span data-stu-id="a3946-837">Static function members.</span></span> <span data-ttu-id="a3946-838">これらは、インスタンスコンストラクター、静的メソッド、静的プロパティアクセサー、およびユーザー定義の演算子です。</span><span class="sxs-lookup"><span data-stu-id="a3946-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="a3946-839">静的関数メンバーは常に非仮想です。</span><span class="sxs-lookup"><span data-stu-id="a3946-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="a3946-840">インスタンス関数のメンバー。</span><span class="sxs-lookup"><span data-stu-id="a3946-840">Instance function members.</span></span> <span data-ttu-id="a3946-841">これらは、インスタンスメソッド、インスタンスプロパティアクセサー、およびインデクサーアクセサーです。</span><span class="sxs-lookup"><span data-stu-id="a3946-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="a3946-842">インスタンス関数メンバーは、仮想でも仮想でもなく、常に特定のインスタンスで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="a3946-843">インスタンスはインスタンス式によって計算され、`this` ([このアクセス](expressions.md#this-access)) として関数メンバー内でアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="a3946-844">関数メンバー呼び出しの実行時の処理は、次の手順で構成されます。ここで `M` は関数メンバーで、`M` がインスタンスメンバーである場合は `E` インスタンス式です。</span><span class="sxs-lookup"><span data-stu-id="a3946-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="a3946-845">`M` が静的関数メンバーである場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="a3946-846">引数リストは、「[引数リスト](expressions.md#argument-lists)」で説明されているように評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="a3946-847">`M` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-847">`M` is invoked.</span></span>

*  <span data-ttu-id="a3946-848">`M` が*value_type*で宣言されたインスタンス関数メンバーの場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="a3946-849">`E` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-849">`E` is evaluated.</span></span> <span data-ttu-id="a3946-850">この評価によって例外が発生した場合、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="a3946-851">`E` が変数として分類されていない場合は、`E`の型の一時的なローカル変数が作成され、`E` の値がその変数に代入されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="a3946-852">その後、`E` は、その一時的なローカル変数への参照として再分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="a3946-853">一時変数は、`M`内の `this` としてアクセスできますが、他の方法ではアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="a3946-854">したがって、`E` が真の変数である場合にのみ、呼び出し元は `this`する `M` の変更を観察できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="a3946-855">引数リストは、「[引数リスト](expressions.md#argument-lists)」で説明されているように評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="a3946-856">`M` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-856">`M` is invoked.</span></span> <span data-ttu-id="a3946-857">`E` によって参照される変数は、`this`によって参照される変数になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="a3946-858">`M` が*reference_type*で宣言されたインスタンス関数メンバーの場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="a3946-859">`E` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-859">`E` is evaluated.</span></span> <span data-ttu-id="a3946-860">この評価によって例外が発生した場合、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="a3946-861">引数リストは、「[引数リスト](expressions.md#argument-lists)」で説明されているように評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="a3946-862">`E` の型が*value_type*である場合、`E` を型 `object`に変換するためのボックス変換 ([ボックス](types.md#boxing-conversions)化変換) が実行され、`E` は次の手順では `object` 型と見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="a3946-863">この場合、`M` は `System.Object`のメンバーである必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="a3946-864">`E` の値が有効であるかどうかがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="a3946-865">`E` の値が `null`場合は `System.NullReferenceException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="a3946-866">呼び出す関数メンバーの実装は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="a3946-867">`E` のバインド時の型がインターフェイスの場合、呼び出す関数メンバーは `E`によって参照されるインスタンスの実行時の型によって提供される `M` の実装です。</span><span class="sxs-lookup"><span data-stu-id="a3946-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="a3946-868">この関数メンバーは、`E`によって参照されるインスタンスのランタイム型によって提供される `M` の実装を決定するために、インターフェイスマッピング規則 ([インターフェイスマッピング](interfaces.md#interface-mapping)) を適用することによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="a3946-869">それ以外の場合、`M` が仮想関数メンバーである場合、呼び出す関数メンバーは `E`によって参照されるインスタンスの実行時の型によって提供される `M` の実装です。</span><span class="sxs-lookup"><span data-stu-id="a3946-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="a3946-870">この関数メンバーは、`E`によって参照されるインスタンスの実行時の型に対して、`M` の最も派生する実装 ([仮想メソッド](classes.md#virtual-methods)) を決定するための規則を適用することによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="a3946-871">それ以外の場合、`M` は非仮想関数メンバーであり、呼び出す関数メンバーは `M` ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="a3946-872">上記の手順で決定した関数メンバーの実装が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="a3946-873">`E` によって参照されるオブジェクトは、`this`によって参照されるオブジェクトになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="a3946-874">ボックス化されたインスタンスでの呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-874">Invocations on boxed instances</span></span>

<span data-ttu-id="a3946-875">*Value_type*に実装されている関数メンバーは、次のような場合に*value_type*のボックス化されたインスタンスを使用して呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="a3946-876">関数メンバーが `object` 型から継承されたメソッドの `override` であり、`object`型のインスタンス式を通じて呼び出される場合。</span><span class="sxs-lookup"><span data-stu-id="a3946-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="a3946-877">関数メンバーがインターフェイス関数メンバーの実装であり、 *interface_type*のインスタンス式を通じて呼び出される場合。</span><span class="sxs-lookup"><span data-stu-id="a3946-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="a3946-878">デリゲートを通じて関数メンバーが呼び出されたとき。</span><span class="sxs-lookup"><span data-stu-id="a3946-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="a3946-879">このような場合、ボックス化されたインスタンスには*value_type*の変数が含まれていると見なされ、この変数は関数メンバー呼び出し内で `this` によって参照される変数になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="a3946-880">これは特に、ボックス化されたインスタンスで関数メンバーが呼び出されたときに、関数メンバーがボックス化されたインスタンスに含まれる値を変更できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="a3946-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="a3946-881">一次式</span><span class="sxs-lookup"><span data-stu-id="a3946-881">Primary expressions</span></span>

<span data-ttu-id="a3946-882">主な式には、最も単純な形式の式が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-882">Primary expressions include the simplest forms of expressions.</span></span>

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

<span data-ttu-id="a3946-883">プライマリ式は*array_creation_expression*s と*primary_no_array_creation_expression*の間で分けられます。</span><span class="sxs-lookup"><span data-stu-id="a3946-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="a3946-884">この方法で配列作成式を扱うことは、他の単純な式フォームと共にリストするのではなく、文法によって、次のような混乱の可能性のあるコードを許可しないようにします。</span><span class="sxs-lookup"><span data-stu-id="a3946-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="a3946-885">これは、</span><span class="sxs-lookup"><span data-stu-id="a3946-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="a3946-886">リテラル</span><span class="sxs-lookup"><span data-stu-id="a3946-886">Literals</span></span>

<span data-ttu-id="a3946-887">*リテラル*([リテラル](lexical-structure.md#literals)) で構成される*primary_expression*は、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="a3946-888">挿入文字列</span><span class="sxs-lookup"><span data-stu-id="a3946-888">Interpolated strings</span></span>

<span data-ttu-id="a3946-889">*Interpolated_string_expression*は、`$` の符号とそれに続く通常のリテラルまたは逐語的な文字列リテラルで構成され、`{` と `}`によって区切られ、式と書式指定の仕様を囲みます。</span><span class="sxs-lookup"><span data-stu-id="a3946-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="a3946-890">補間文字列式は、「[補間文字列リテラル](lexical-structure.md#interpolated-string-literals)」で説明されているように、個々のトークンに分割された*interpolated_string_literal*の結果です。</span><span class="sxs-lookup"><span data-stu-id="a3946-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

<span data-ttu-id="a3946-891">補間内の*constant_expression*には、`int`への暗黙的な変換が必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="a3946-892">*Interpolated_string_expression*は値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="a3946-893">`System.IFormattable` に直ちに変換される場合、または暗黙的な挿入文字列変換 (暗黙的な補[間](conversions.md#implicit-interpolated-string-conversions)文字列変換) を使用して `System.FormattableString` 場合、挿入文字列式の型はです。</span><span class="sxs-lookup"><span data-stu-id="a3946-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="a3946-894">それ以外の場合は `string`型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="a3946-895">挿入文字列の型が `System.IFormattable` または `System.FormattableString`の場合、意味は `System.Runtime.CompilerServices.FormattableStringFactory.Create`を呼び出すことになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="a3946-896">型が `string`の場合、式の意味は `string.Format`の呼び出しになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="a3946-897">どちらの場合も、呼び出しの引数リストは、各補間のプレースホルダーを含む書式指定文字列リテラルと、プレースホルダーに対応する各式の引数で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="a3946-898">書式指定文字列リテラルは次のように構成されます。ここで `N` は*interpolated_string_expression*内の補間の数です。</span><span class="sxs-lookup"><span data-stu-id="a3946-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="a3946-899">*Interpolated_regular_string_whole*または*interpolated_verbatim_string_whole*が `$` 記号の後に続く場合、書式文字列リテラルはそのトークンです。</span><span class="sxs-lookup"><span data-stu-id="a3946-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="a3946-900">それ以外の場合、書式指定文字列リテラルは次の要素で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="a3946-901">最初の*interpolated_regular_string_start*または*interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="a3946-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="a3946-902">次に、`0` から `N-1`に `I` 数値ごとに、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="a3946-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="a3946-903">`I` の10進数表現</span><span class="sxs-lookup"><span data-stu-id="a3946-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="a3946-904">次に、対応する*補間*に*constant_expression*がある場合、`,` (コンマ) の後に*constant_expression*の値の10進数表現が続きます。</span><span class="sxs-lookup"><span data-stu-id="a3946-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="a3946-905">次に、対応する補間の直後に*interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_mid* 、または*interpolated_verbatim_string_end*ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="a3946-906">後続の引数は、単に*補間*(存在する場合) からの*式*です。</span><span class="sxs-lookup"><span data-stu-id="a3946-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="a3946-907">TODO: 例。</span><span class="sxs-lookup"><span data-stu-id="a3946-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="a3946-908">簡易名</span><span class="sxs-lookup"><span data-stu-id="a3946-908">Simple names</span></span>

<span data-ttu-id="a3946-909">*Simple_name*は識別子で構成され、その後に型引数リストが続きます。</span><span class="sxs-lookup"><span data-stu-id="a3946-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="a3946-910">*Simple_name*は、フォーム `I` またはフォーム `I<A1,...,Ak>`のいずれかです。ここで `I` は単一の識別子で、`<A1,...,Ak>` は省略可能な*type_argument_list*です。</span><span class="sxs-lookup"><span data-stu-id="a3946-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="a3946-911">*Type_argument_list*が指定されていない場合は、`K` を0にすることを検討してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="a3946-912">*Simple_name*が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="a3946-913">`K` がゼロで*simple_name*が*ブロック*内に存在し、*ブロック*の (またはそれを囲む*ブロック*の) ローカル変数宣言領域 ([宣言](basic-concepts.md#declarations)) に、 `I`という名前のローカル変数、パラメーター、または定数が含まれている場合、 *simple_name*はそのローカル変数、パラメーター、または定数を参照し、変数または値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="a3946-914">`K` がゼロで、 *simple_name*がジェネリックメソッド宣言の本体内に存在し、その宣言に `I`という名前の型パラメーターが含まれている場合、 *simple_name*はその型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="a3946-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="a3946-915">それ以外の場合は、インスタンス型 `T` ([インスタンス型](classes.md#the-instance-type)) に対して、すぐ外側の型宣言のインスタンス型から開始し、外側の各クラスまたは構造体宣言のインスタンス型 (存在する場合) を続行します。</span><span class="sxs-lookup"><span data-stu-id="a3946-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="a3946-916">`K` がゼロで、`T` の宣言に `I`という名前の型パラメーターが含まれている場合、 *simple_name*はその型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="a3946-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="a3946-917">それ以外の場合、`K` 型引数を使用して `T` 内の `I` のメンバー参照 ([メンバー参照](expressions.md#member-lookup)) が一致すると、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="a3946-918">`T` がすぐ外側のクラスまたは構造体型のインスタンス型であり、参照が1つ以上のメソッドを識別する場合、結果は `this`のインスタンス式が関連付けられたメソッドグループになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="a3946-919">型引数リストが指定されている場合は、ジェネリックメソッド ([メソッド呼び出し](expressions.md#method-invocations)) の呼び出しで使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="a3946-920">それ以外の場合、`T` が、すぐに外側のクラスまたは構造体型のインスタンス型である場合、参照がインスタンスメンバーを識別し、インスタンスコンストラクター、インスタンスメソッド、またはインスタンスアクセサーの本体内で参照が発生した場合、結果は `this.I`フォームのメンバーアクセス ([メンバー](expressions.md#member-access)アクセス) と同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="a3946-921">これは、`K` が0の場合にのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="a3946-922">それ以外の場合、結果は `T.I` または `T.I<A1,...,Ak>`フォームのメンバーアクセス ([メンバーアクセス](expressions.md#member-access)) と同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="a3946-923">この場合、 *simple_name*がインスタンスメンバーを参照するためのバインド時エラーです。</span><span class="sxs-lookup"><span data-stu-id="a3946-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="a3946-924">それ以外の場合は、名前空間 `N`ごとに、 *simple_name*が発生する名前空間から開始し、外側の各名前空間 (存在する場合) を続行し、グローバル名前空間で終わると、エンティティが見つかるまで次の手順が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="a3946-925">`K` がゼロで、`I` が `N`内の名前空間の名前である場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="a3946-926">*Simple_name*が発生した場所が `N` の名前空間宣言によって囲まれていて、名前空間に名前空間または型が関連付けられている*extern_alias_directive*または*using_alias_directive*が名前空間宣言に含まれている場合、 * `I`* はあいまいで、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="a3946-927">それ以外の場合、 *simple_name*は `N`内の `I` という名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="a3946-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="a3946-928">それ以外の場合、`N` に名前 `I` と `K` 型パラメーターを持つアクセス可能な型が含まれている場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="a3946-929">`K` がゼロで、 *simple_name*が発生した場所が `N` の名前空間宣言によって囲まれており、名前空間を名前空間または型に関連付ける*extern_alias_directive*または*using_alias_directive*が名前空間宣言に含まれている場合、 * `I`* はあいまいで、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="a3946-930">それ以外の場合、 *namespace_or_type_name*は、指定された型引数を使用して構築された型を参照します。</span><span class="sxs-lookup"><span data-stu-id="a3946-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="a3946-931">それ以外の場合、 *simple_name*が発生した場所が `N`の名前空間宣言によって囲まれている場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="a3946-932">`K` がゼロで、名前空間の宣言に、名前 `I` をインポートした名前空間または型に関連付ける*extern_alias_directive*または*using_alias_directive*が含まれている場合、 *simple_name*はその名前空間または型を参照します。</span><span class="sxs-lookup"><span data-stu-id="a3946-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="a3946-933">それ以外の場合、 *using_namespace_directive*s および名前空間宣言の*using_static_directive*によってインポートされた名前空間と型の宣言に、アクセス可能な型または非拡張静的メンバーが1つだけ含まれており、その名前が `I` で、型 * * パラメーターが `K`場合は、指定された型引数を使用して構築されたその型またはメンバーを</span><span class="sxs-lookup"><span data-stu-id="a3946-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="a3946-934">それ以外の場合、名前空間宣言の*using_namespace_directive*s によってインポートされた名前空間と型に複数のアクセス可能な型または非拡張メソッドの静的メンバーが含まれていて、名前が `I` で、型パラメーターが  `K`場合、 *simple_name*はあいまいで、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="a3946-935">この手順全体は、 *namespace_or_type_name* ([名前空間と型名](basic-concepts.md#namespace-and-type-names)) を処理するための対応する手順と完全に並列であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="a3946-936">それ以外の場合、 *simple_name*は定義されていないため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="a3946-937">かっこで囲まれる式</span><span class="sxs-lookup"><span data-stu-id="a3946-937">Parenthesized expressions</span></span>

<span data-ttu-id="a3946-938">*Parenthesized_expression*は、かっこで囲まれた*式*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="a3946-939">*Parenthesized_expression*は、かっこ内の*式*を評価することによって評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="a3946-940">かっこ内の*式*が名前空間または型を表している場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="a3946-941">それ以外の場合、 *parenthesized_expression*の結果は、含まれている*式*を評価した結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="a3946-942">メンバー アクセス。</span><span class="sxs-lookup"><span data-stu-id="a3946-942">Member access</span></span>

<span data-ttu-id="a3946-943">*Member_access*は、 *primary_expression*、 *predefined_type*、または*qualified_alias_member*で構成され、その後に "`.`" トークン、*識別子*、および必要に応じて、後に*type_argument_list*が続きます。</span><span class="sxs-lookup"><span data-stu-id="a3946-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

<span data-ttu-id="a3946-944">*Qualified_alias_member*運用環境は、[名前空間エイリアス修飾子](namespaces.md#namespace-alias-qualifiers)で定義されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="a3946-945">*Member_access*は、フォーム `E.I` またはフォーム `E.I<A1, ..., Ak>`のいずれかです。ここで `E` はプライマリ式であり、`I` は単一の識別子で、`<A1, ..., Ak>` は省略可能な*type_argument_list*です。</span><span class="sxs-lookup"><span data-stu-id="a3946-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="a3946-946">*Type_argument_list*が指定されていない場合は、`K` を0にすることを検討してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="a3946-947">`dynamic` 型の*primary_expression*を持つ*member_access*が動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a3946-948">この場合、コンパイラはメンバーアクセスを `dynamic`型のプロパティアクセスとして分類します。</span><span class="sxs-lookup"><span data-stu-id="a3946-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="a3946-949">次の規則は、 *member_access*の意味を判断するために、 *primary_expression*のコンパイル時の型ではなく、実行時の型を使用して実行時に適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="a3946-950">この実行時の分類によってメソッドグループが発生する場合は、メンバーアクセスが*invocation_expression*の*primary_expression*である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="a3946-951">*Member_access*が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="a3946-952">`K` がゼロで `E` が名前空間で、`E` に名前が `I`で入れ子になった名前空間が含まれている場合、結果はその名前空間になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="a3946-953">それ以外の場合、`E` が名前空間で、`E` に名前 `I` および  `K`型パラメーターを持つアクセス可能な型が含まれている場合、結果は指定された型引数を使用して構築された型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="a3946-954">`E` が型として分類された*predefined_type*または*primary_expression*の場合、`E` が型パラメーターではなく、`I` の `E` のメンバー参照 ([メンバー参照](expressions.md#member-lookup)) が `K` 型パラメーターで一致すると、`E.I` が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="a3946-955">`I` が型を識別する場合、結果は指定された型引数を使用して構築された型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="a3946-956">`I` が1つ以上のメソッドを識別する場合、結果はインスタンス式が関連付けられていないメソッドグループになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="a3946-957">型引数リストが指定されている場合は、ジェネリックメソッド ([メソッド呼び出し](expressions.md#method-invocations)) の呼び出しで使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="a3946-958">`I` が `static` プロパティを識別する場合、結果は、インスタンス式が関連付けられていないプロパティアクセスになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="a3946-959">`I` が `static` フィールドを識別する場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="a3946-960">フィールドが `readonly`、フィールドが宣言されているクラスまたは構造体の静的コンストラクターの外側で参照が発生した場合、結果は値になります。つまり、 `E`の静的フィールド `I` の値です。</span><span class="sxs-lookup"><span data-stu-id="a3946-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="a3946-961">それ以外の場合、結果は変数になります。つまり、 `E`の静的フィールド `I` です。</span><span class="sxs-lookup"><span data-stu-id="a3946-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="a3946-962">`I` が `static` イベントを識別する場合:</span><span class="sxs-lookup"><span data-stu-id="a3946-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="a3946-963">イベントが宣言されているクラスまたは構造体内で参照が発生し、イベントが*event_accessor_declarations* ([イベント](classes.md#events)) を使用せずに宣言されている場合、`E.I` は `I` が静的フィールドであった場合とまったく同じように処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="a3946-964">それ以外の場合、結果はインスタンス式が関連付けられていないイベントアクセスになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="a3946-965">`I` が定数を識別する場合、結果は値、つまりその定数の値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="a3946-966">`I` が列挙型のメンバーを識別する場合、結果はその列挙体のメンバーの値、つまり値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="a3946-967">それ以外の場合、`E.I` は無効なメンバー参照であり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="a3946-968">`E` がプロパティアクセス、インデクサーアクセス、変数、または値であり、その型が `T`である場合、および `K` 型引数を持つ `T` 内の `I` のメンバー参照 ([メンバー参照](expressions.md#member-lookup)) によって一致が生成されると、`E.I` が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="a3946-969">最初に、`E` がプロパティまたはインデクサーアクセスである場合は、プロパティまたはインデクサーアクセスの値 ([式の値](expressions.md#values-of-expressions)) を取得し、`E` を値として再分類します。</span><span class="sxs-lookup"><span data-stu-id="a3946-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="a3946-970">`I` が1つ以上のメソッドを識別する場合、結果は `E`のインスタンス式が関連付けられたメソッドグループになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="a3946-971">型引数リストが指定されている場合は、ジェネリックメソッド ([メソッド呼び出し](expressions.md#method-invocations)) の呼び出しで使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="a3946-972">`I` インスタンスのプロパティを識別する場合は、</span><span class="sxs-lookup"><span data-stu-id="a3946-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="a3946-973">`E` が `this`場合、`I` は自動的に実装されるプロパティ ([自動的に実装](classes.md#automatically-implemented-properties)されるプロパティ) を setter を指定せずに識別します。参照は、クラスまたは構造体の `T`型のインスタンスコンストラクター内で行われます。その後、結果は変数になります。つまり、`I` によって指定された `T` によって指定された自動`this`</span><span class="sxs-lookup"><span data-stu-id="a3946-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="a3946-974">それ以外の場合、結果は、 `E`のインスタンス式が関連付けられたプロパティアクセスになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="a3946-975">`T` が*class_type*で、`I` *class_type*のインスタンスフィールドを識別する場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="a3946-976">`E` の値が `null`場合は、`System.NullReferenceException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="a3946-977">それ以外の場合、フィールドが `readonly`、参照がフィールドが宣言されているクラスのインスタンスコンストラクターの外部で発生した場合、結果は値になります。つまり、 `E`によって参照されるオブジェクト内のフィールド `I` の値です。</span><span class="sxs-lookup"><span data-stu-id="a3946-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="a3946-978">それ以外の場合、結果は変数、つまり `E`によって参照されるオブジェクト内のフィールド `I` になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="a3946-979">`T` が*struct_type*で、`I` *struct_type*のインスタンスフィールドを識別する場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="a3946-980">`E` が値の場合、またはフィールドが `readonly` で、そのフィールドが宣言されている構造体のインスタンスコンストラクターの外側で参照が発生した場合、結果は値になります。つまり、 `E`によって指定された構造体のインスタンスコンストラクター内のフィールド `I` の値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="a3946-981">それ以外の場合、結果は変数、つまり `E`によって指定された構造体インスタンス内のフィールド `I` になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="a3946-982">`I` がインスタンスイベントを識別する場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="a3946-983">イベントが宣言されているクラスまたは構造体内で参照が発生し、イベントが*event_accessor_declarations* ([イベント](classes.md#events)) なしで宣言されており、参照が `+=` または `-=` 演算子の左側として発生していない場合、`E.I` は `I` がインスタンスフィールドである場合とまったく同じように処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="a3946-984">それ以外の場合、結果は、 `E`のインスタンス式が関連付けられたイベントアクセスになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="a3946-985">それ以外の場合、拡張メソッドの呼び出し ([拡張メソッドの呼び出し](expressions.md#extension-method-invocations)) として `E.I` を処理しようとしました。</span><span class="sxs-lookup"><span data-stu-id="a3946-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="a3946-986">これが失敗した場合、`E.I` は無効なメンバー参照であり、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="a3946-987">同じ簡易名と型名</span><span class="sxs-lookup"><span data-stu-id="a3946-987">Identical simple names and type names</span></span>

<span data-ttu-id="a3946-988">`E.I`フォームのメンバーアクセスでは、`E` が単一の識別子であり、 *simple_name* ([簡易名](expressions.md#simple-names)) としての `E` の意味が、`E` ([名前空間と型名](basic-concepts.md#namespace-and-type-names)) とし*て type_name の*意味と同じ型を持つ定数、フィールド、プロパティ、ローカル変数、またはパラメーターである場合、`E` の両方の意味を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="a3946-989">`I` は常に `E` 型のメンバーである必要があるため、`E.I` の2つの意味があいまいになることはありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="a3946-990">つまり、この規則は、コンパイル時エラーが発生した `E` の静的メンバーおよび入れ子にされた型へのアクセスを許可するだけです。</span><span class="sxs-lookup"><span data-stu-id="a3946-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="a3946-991">例 :</span><span class="sxs-lookup"><span data-stu-id="a3946-991">For example:</span></span>
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a><span data-ttu-id="a3946-992">文法のあいまい性</span><span class="sxs-lookup"><span data-stu-id="a3946-992">Grammar ambiguities</span></span>

<span data-ttu-id="a3946-993">*Simple_name* ([簡易名](expressions.md#simple-names)) と*member_access* ([メンバーアクセス](expressions.md#member-access)) の生産によって、式の文法があいまいになる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="a3946-994">たとえば、ステートメントは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-994">For example, the statement:</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="a3946-995">は、`G < A` と `B > (7)`の2つの引数を持つ `F` の呼び出しとして解釈される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="a3946-996">または、1つの引数を持つ `F` の呼び出しとして解釈することもできます。これは、2つの型引数と1つの標準引数を持つジェネリックメソッド `G` の呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="a3946-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="a3946-997">トークンのシーケンスを*simple_name* ([簡易名](expressions.md#simple-names))、 *member_access* ([メンバーアクセス](expressions.md#member-access))、または*pointer_member_access* ([ポインターメンバーアクセス](unsafe-code.md#pointer-member-access)) で*type_argument_list*終わり ([型引数](types.md#type-arguments)) として解析できる場合、終了 `>` トークンの直後にあるトークンが検査されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="a3946-998">のいずれかである場合</span><span class="sxs-lookup"><span data-stu-id="a3946-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="a3946-999">その後、 *type_argument_list*は*simple_name*、 *member_access*または*pointer_member_access*の一部として保持され、その他のトークンシーケンスの解析はすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="a3946-1000">それ以外の場合、トークンのシーケンスを解析できない場合でも、 *type_argument_list*は*simple_name*、 *member_access* 、または*pointer_member_access*の一部とは見なされません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="a3946-1001">これらの規則は、 *namespace_or_type_name* ([名前空間と型名](basic-concepts.md#namespace-and-type-names)) の*type_argument_list*を解析するときには適用されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="a3946-1002">次のステートメント、</span><span class="sxs-lookup"><span data-stu-id="a3946-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="a3946-1003">は、この規則に従って、1つの引数を持つ `F` の呼び出しとして解釈されます。これは、2つの型引数と1つの標準引数を持つジェネリックメソッド `G` の呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="a3946-1004">ステートメント</span><span class="sxs-lookup"><span data-stu-id="a3946-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="a3946-1005">は、2つの引数を持つ `F` の呼び出しとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="a3946-1006">次のステートメント、</span><span class="sxs-lookup"><span data-stu-id="a3946-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="a3946-1007">は、 *type_argument_list*の後に二項プラス演算子が続く*simple_name*としてではなく `x = (F < A) > (+y)`に記述されているかのように、小なり演算子、大なり演算子、および単項プラス演算子として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="a3946-1008">ステートメント内</span><span class="sxs-lookup"><span data-stu-id="a3946-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="a3946-1009">`C<T>` トークンは、 *type_argument_list*の*namespace_or_type_name*として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="a3946-1010">Invocation 式</span><span class="sxs-lookup"><span data-stu-id="a3946-1010">Invocation expressions</span></span>

<span data-ttu-id="a3946-1011">*Invocation_expression*は、メソッドを呼び出すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="a3946-1012">次のいずれかに当てはまる場合、 *invocation_expression*は動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="a3946-1013">*Primary_expression*には、コンパイル時の型 `dynamic`があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="a3946-1014">オプションの*argument_list*の少なくとも1つの引数にコンパイル時の型 `dynamic` があり、 *primary_expression*にデリゲート型がありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="a3946-1015">この場合、コンパイラは*invocation_expression*を型 `dynamic`の値として分類します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="a3946-1016">次の規則は、 *invocation_expression*の意味を判断するために、実行時に実行時に適用されます。これには、コンパイル時の型 `dynamic`を持つ*primary_expression*および引数のコンパイル時の型ではなく、実行時の型が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="a3946-1017">*Primary_expression*にコンパイル時の型 `dynamic`がない場合、「[動的なオーバーロードの解決のコンパイル時のチェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)」で説明されているように、メソッドの呼び出しでコンパイル時間が制限されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="a3946-1018">*Invocation_expression*の*primary_expression*は、メソッドグループまたは*delegate_type*の値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="a3946-1019">*Primary_expression*がメソッドグループの場合、 *invocation_expression*はメソッド呼び出し ([メソッド](expressions.md#method-invocations)呼び出し) です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="a3946-1020">*Primary_expression*が*delegate_type*の値の場合、 *Invocation_expression*はデリゲート呼び出し ([デリゲート](expressions.md#delegate-invocations)呼び出し) です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="a3946-1021">*Primary_expression*がメソッドグループでも*delegate_type*の値でもない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-1022">省略可能な*argument_list* ([引数リスト](expressions.md#argument-lists)) は、メソッドのパラメーターの値または変数参照を提供します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="a3946-1023">*Invocation_expression*を評価した結果は、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="a3946-1024">*Invocation_expression*が `void`を返すメソッドまたはデリゲートを呼び出すと、結果は nothing になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="a3946-1025">Nothing として分類される式は、 *statement_expression* ([式ステートメント](statements.md#expression-statements)) のコンテキスト、または*lambda_expression* ([匿名関数式](expressions.md#anonymous-function-expressions)) の本体としてのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="a3946-1026">それ以外の場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="a3946-1027">それ以外の場合、結果は、メソッドまたはデリゲートによって返される型の値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="a3946-1028">メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-1028">Method invocations</span></span>

<span data-ttu-id="a3946-1029">メソッド呼び出しの場合、 *invocation_expression*の*primary_expression*はメソッドグループである必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="a3946-1030">メソッドグループは、呼び出す1つのメソッド、または特定のメソッドを呼び出すオーバーロードされたメソッドのセットを識別します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="a3946-1031">後者の場合、呼び出す特定のメソッドの決定は、 *argument_list*内の引数の型によって提供されるコンテキストに基づいて行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="a3946-1032">`M(A)`フォームのメソッド呼び出しのバインド時の処理。 `M` は、( *type_argument_list*を含む) メソッドグループであり、`A` は省略可能な*argument_list*で、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="a3946-1033">メソッド呼び出しの候補メソッドのセットが構築されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="a3946-1034">メソッドグループに関連付けられている各メソッド `F` `M`を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="a3946-1035">`F` が非ジェネリックの場合、`F` は次のような場合に候補になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="a3946-1036">`M` に型引数リストがありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="a3946-1037">`F` は `A` ([適用可能な関数メンバー](expressions.md#applicable-function-member)) に対して適用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="a3946-1038">`F` がジェネリックで `M` に型引数リストがない場合、`F` は次のような場合に候補になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="a3946-1039">型の推定 ([型の推定](expressions.md#type-inference)) が成功し、呼び出しの型引数のリストが推論されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="a3946-1040">推論された型引数が対応するメソッドの型パラメーターに置き換えられると、F のパラメーターリスト内に構築されたすべての型は、制約を満たす ([制約を満たす](types.md#satisfying-constraints)) ことができます。また、`F` のパラメーターリストは、`A` ([適用可能な関数メンバー](expressions.md#applicable-function-member)) に対して適用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="a3946-1041">`F` がジェネリックで、`M` 型引数リストを含む場合、`F` は次のような場合に候補になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="a3946-1042">`F` には、型引数リストで指定されたものと同じ数のメソッド型パラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="a3946-1043">型引数を対応するメソッドの型パラメーターの代わりに使用すると、F のパラメーターリスト内の構築されたすべての型は、制約を満たす ([制約を満たす](types.md#satisfying-constraints)) と共に、`F` のパラメーターリストを `A` ([適用可能な関数メンバー](expressions.md#applicable-function-member)) に対して適用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="a3946-1044">候補メソッドのセットは、ほとんどの派生型のメソッドのみを含むように縮小されます。 set 内のメソッド `C.F` ごとに、`C` はメソッド `F` が宣言されている型で、`C` の基本型で宣言されているすべてのメソッドがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="a3946-1045">さらに、`C` が `object`以外のクラス型である場合は、インターフェイス型で宣言されたすべてのメソッドがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="a3946-1046">(この後者の規則は、メソッドグループが、オブジェクト以外の有効な基本クラスと空でない有効なインターフェイスセットを持つ型パラメーターに対するメンバー参照の結果である場合にのみ影響を与えます)。</span><span class="sxs-lookup"><span data-stu-id="a3946-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="a3946-1047">結果として得られる一連のメソッドが空の場合、次の手順に従ってさらに処理が中止され、その代わりに、拡張メソッドの呼び出し ([拡張メソッド](expressions.md#extension-method-invocations)呼び出し) として呼び出しを処理しようとしました。</span><span class="sxs-lookup"><span data-stu-id="a3946-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="a3946-1048">これが失敗した場合、適用可能なメソッドは存在せず、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="a3946-1049">候補のメソッドセットの最適なメソッドは、[オーバーロード解決](expressions.md#overload-resolution)のオーバーロードの解決規則を使用して識別されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="a3946-1050">1つの最適なメソッドを識別できない場合、メソッドの呼び出しがあいまいになり、バインディング時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="a3946-1051">オーバーロードの解決を実行する場合、ジェネリックメソッドのパラメーターは、対応するメソッドの型パラメーターに対して型引数 (指定または推論) を代入した後に考慮されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="a3946-1052">選択した最適な方法の最終的な検証が実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="a3946-1053">メソッドは、メソッドグループのコンテキストで検証されます。最適なメソッドが静的メソッドの場合、メソッドグループは、型によって*simple_name*または*member_access*から生成されたものである必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="a3946-1054">最適なメソッドがインスタンスメソッドの場合、メソッドグループの結果は、 *simple_name*、変数または値を使用した*member_access* 、または*base_access*である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="a3946-1055">これらの要件のいずれも当てはまらない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="a3946-1056">最適なメソッドがジェネリックメソッドの場合は、ジェネリックメソッドで宣言されている制約 ([制約を満たす](types.md#satisfying-constraints)) に対して、型引数 (指定または推論) がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="a3946-1057">型引数が型パラメーターの対応する制約を満たしていない場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-1058">前の手順でバインド時にメソッドを選択して検証すると、実際のランタイム呼び出しは、「[動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)」で説明されている関数メンバー呼び出しの規則に従って処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="a3946-1059">上記で説明した解決規則の直感的な効果は次のとおりです。メソッド呼び出しによって呼び出された特定のメソッドを検索するには、メソッドの呼び出しによって示された型から開始し、少なくとも1つのが適用されるまで継承チェーンを進めます。アクセス可能で、オーバーライドされていないメソッド宣言が見つかりました。</span><span class="sxs-lookup"><span data-stu-id="a3946-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="a3946-1060">次に、その型で宣言されている、アクセス可能で、オーバーライドできないメソッドのセットに対して、型の推定とオーバーロードの解決を実行し、選択したメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="a3946-1061">メソッドが見つからなかった場合は、代わりに拡張メソッドの呼び出しとして呼び出しを処理するようにしてください。</span><span class="sxs-lookup"><span data-stu-id="a3946-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="a3946-1062">拡張メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-1062">Extension method invocations</span></span>

<span data-ttu-id="a3946-1063">フォームのいずれかのメソッド呼び出し (ボックス化された[インスタンスでの呼び出し](expressions.md#invocations-on-boxed-instances))</span><span class="sxs-lookup"><span data-stu-id="a3946-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="a3946-1064">呼び出しの通常の処理で適用可能なメソッドが検出されなかった場合は、拡張メソッドの呼び出しとしてコンストラクトを処理しようとしました。</span><span class="sxs-lookup"><span data-stu-id="a3946-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="a3946-1065">*Expr*またはいずれかの*引数*にコンパイル時の型 `dynamic`がある場合、拡張メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="a3946-1066">目的は、対応する静的メソッドの呼び出しを実行できるように、最適な*type_name* `C`を見つけることです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="a3946-1067">拡張メソッド `Ci.Mj` は、次の場合に***適し***ています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="a3946-1068">非ジェネリックの非入れ子クラスである `Ci`</span><span class="sxs-lookup"><span data-stu-id="a3946-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="a3946-1069">`Mj` の名前は*識別子*です</span><span class="sxs-lookup"><span data-stu-id="a3946-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="a3946-1070">上に示すように、引数に静的メソッドとして適用すると、`Mj` にアクセスして適用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="a3946-1071">暗黙的な id、参照、またはボックス化変換は、 *expr*から `Mj`の最初のパラメーターの型に存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="a3946-1072">`C` の検索は次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="a3946-1073">最も近い外側の名前空間宣言から開始し、外側の名前空間宣言のそれぞれを継続し、次に、一連の拡張メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="a3946-1074">指定された名前空間またはコンパイル単位に、対象となる拡張メソッド `Mj`の `Ci` 非ジェネリック型宣言が直接含まれている場合、これらの拡張メソッドのセットは候補セットになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="a3946-1075">型 `Ci` *using_static_declarations*によってインポートされ、特定の名前空間またはコンパイル単位の*using_namespace_directive*によってインポートされた名前空間で直接宣言されている場合、適切な拡張メソッド `Mj`が直接含まれているため、これらの拡張メソッドのセットは候補セットになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="a3946-1076">外側の名前空間宣言またはコンパイル単位で候補セットが見つからない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="a3946-1077">それ以外の場合は、「([オーバーロードの解決](expressions.md#overload-resolution))」の説明に従って、オーバーロードの解決が候補セットに適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="a3946-1078">1つの最適なメソッドが見つからない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="a3946-1079">`C` は、最適なメソッドが拡張メソッドとして宣言されている型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="a3946-1080">`C` をターゲットとして使用すると、メソッドの呼び出しは静的メソッドの呼び出しとして処理されます ([動的なオーバーロードの解決のコンパイル時のチェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="a3946-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="a3946-1081">上記の規則は、インスタンスメソッドが拡張メソッドよりも優先されることを意味します。内部名前空間宣言で使用できる拡張メソッドは、外側の名前空間宣言で使用できる拡張メソッドよりも優先されます。名前空間で直接宣言されたメソッドは、using namespace ディレクティブを使用して同じ名前空間にインポートされた拡張メソッドよりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="a3946-1082">例 :</span><span class="sxs-lookup"><span data-stu-id="a3946-1082">For example:</span></span>
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

<span data-ttu-id="a3946-1083">この例では、`B`のメソッドは最初の拡張メソッドよりも優先され、`C`のメソッドは両方の拡張メソッドよりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

<span data-ttu-id="a3946-1084">この例の出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1084">The output of this example is:</span></span>
```console
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="a3946-1085">`D.G` は `C.G`よりも優先され、`E.F` は `D.F` と `C.F`の両方に優先します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="a3946-1086">デリゲートの呼び出し</span><span class="sxs-lookup"><span data-stu-id="a3946-1086">Delegate invocations</span></span>

<span data-ttu-id="a3946-1087">デリゲート呼び出しの場合、 *invocation_expression*の*primary_expression*は*delegate_type*の値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="a3946-1088">さらに、 *delegate_type*が*delegate_type*と同じパラメーターリストを持つ関数メンバーになることを考慮して、 *delegate_type*は*invocation_expression*の*argument_list*に対して適用可能 ([適用可能な関数メンバー](expressions.md#applicable-function-member)) である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="a3946-1089">`D(A)`フォームのデリゲート呼び出しの実行時処理。 `D` は*delegate_type*の*primary_expression*であり、`A` は省略可能な*argument_list*で、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="a3946-1090">`D` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1090">`D` is evaluated.</span></span> <span data-ttu-id="a3946-1091">この評価によって例外が発生した場合、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="a3946-1092">`D` の値が有効であるかどうかがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="a3946-1093">`D` の値が `null`場合は `System.NullReferenceException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="a3946-1094">それ以外の場合、`D` はデリゲートインスタンスへの参照です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="a3946-1095">関数メンバー呼び出し ([動的なオーバーロード解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) は、デリゲートの呼び出しリスト内の呼び出し可能な各エンティティに対して実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="a3946-1096">インスタンスメソッドとインスタンスメソッドで構成される呼び出し可能なエンティティの場合、呼び出しのインスタンスは、呼び出し可能なエンティティに含まれるインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="a3946-1097">要素アクセス</span><span class="sxs-lookup"><span data-stu-id="a3946-1097">Element access</span></span>

<span data-ttu-id="a3946-1098">*Element_access*は、 *primary_no_array_creation_expression*で構成され、その後に "`[`" トークン、その後に続く*argument_list*、"`]`" トークンが続きます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="a3946-1099">*Argument_list*は、コンマで区切られた1つ以上の*引数*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="a3946-1100">*Element_access*の*argument_list*に `ref` または `out` 引数を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="a3946-1101">次のいずれかに当てはまる場合、 *element_access*は動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="a3946-1102">*Primary_no_array_creation_expression*には、コンパイル時の型 `dynamic`があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="a3946-1103">*Argument_list*の少なくとも1つの式のコンパイル時の型が `dynamic` で、 *primary_no_array_creation_expression*に配列型がありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="a3946-1104">この場合、コンパイラは*element_access*を型 `dynamic`の値として分類します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="a3946-1105">次の規則は、 *element_access*の意味を判断するために、実行時に実行時に適用されます。これには、コンパイル時の型 `dynamic`を持つ*primary_no_array_creation_expression*および*argument_list*式のコンパイル時の型ではなく、ランタイム型が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="a3946-1106">*Primary_no_array_creation_expression*にコンパイル時の型 `dynamic`がない場合は、「[動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)」で説明されているように、要素へのアクセスが制限付きコンパイル時間になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="a3946-1107">*Element_access*の*primary_no_array_creation_expression*が*array_type*の値の場合、 *element_access*は配列アクセス ([配列アクセス](expressions.md#array-access)) です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="a3946-1108">それ以外の場合、 *primary_no_array_creation_expression*は、1つ以上のインデクサーメンバーを持つクラス、構造体、またはインターフェイス型の変数または値である必要があります。この場合、 *element_access*はインデクサーアクセス ([インデクサーアクセス](expressions.md#indexer-access)) になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="a3946-1109">配列へのアクセス</span><span class="sxs-lookup"><span data-stu-id="a3946-1109">Array access</span></span>

<span data-ttu-id="a3946-1110">配列アクセスの場合、 *element_access*の*primary_no_array_creation_expression*は*array_type*の値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="a3946-1111">さらに、配列アクセスの*argument_list*に名前付き引数を含めることはできません。*Argument_list*内の式の数は、 *array_type*のランクと同じである必要があります。また、各式は `int`、`uint`、`long`、`ulong`の型であるか、またはこれらの型の1つ以上に暗黙的に変換可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="a3946-1112">配列アクセスを評価した結果は、配列の要素型、つまり、 *argument_list*内の式の値によって選択された配列要素の変数になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="a3946-1113">`P[A]`フォームへの配列アクセスの実行時処理では、`P` は*array_type*の*primary_no_array_creation_expression*であり、`A` は*argument_list*で、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="a3946-1114">`P` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1114">`P` is evaluated.</span></span> <span data-ttu-id="a3946-1115">この評価によって例外が発生した場合、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="a3946-1116">*Argument_list*のインデックス式は、左から右に順番に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="a3946-1117">各インデックス式を評価すると、次のいずれかの型への暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) が実行されます: `int`、`uint`、`long`、`ulong`。</span><span class="sxs-lookup"><span data-stu-id="a3946-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="a3946-1118">暗黙的な変換が存在する、この一覧の最初の型が選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="a3946-1119">たとえば、インデックス式が `short` 型の場合、`int` への暗黙的な変換が実行されます。これは、`short` から `int` への暗黙の変換と `short` から `long` への暗黙的な変換が可能であるためです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="a3946-1120">インデックス式またはそれ以降の暗黙的な変換の評価によって例外が発生した場合、それ以降のインデックス式は評価されず、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="a3946-1121">`P` の値が有効であるかどうかがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="a3946-1122">`P` の値が `null`場合は `System.NullReferenceException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="a3946-1123">*Argument_list*内の各式の値は、`P`によって参照される配列インスタンスの各次元の実際の境界に照らし合わせてチェックされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="a3946-1124">1つ以上の値が範囲外の場合は、`System.IndexOutOfRangeException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="a3946-1125">インデックス式によって指定された配列要素の位置が計算され、この位置が配列アクセスの結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="a3946-1126">インデクサーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="a3946-1126">Indexer access</span></span>

<span data-ttu-id="a3946-1127">インデクサーアクセスの場合、 *element_access*の*primary_no_array_creation_expression*はクラス、構造体、またはインターフェイス型の変数または値である必要があり、この型は*element_access*の*argument_list*に対して適用できる1つ以上のインデクサーを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="a3946-1128">`P[A]`形式のインデクサーアクセスのバインド時の処理では、`P` はクラス、構造体、またはインターフェイス型 `T`の*primary_no_array_creation_expression*であり、`A` は*argument_list*で、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="a3946-1129">`T` によって提供されるインデクサーのセットが構築されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="a3946-1130">このセットは、`T` で宣言されたすべてのインデクサーと、`override` 宣言ではなく、現在のコンテキスト ([メンバーアクセス](basic-concepts.md#member-access)) でアクセスできる `T` の基本型で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="a3946-1131">このセットは、他のインデクサーによって適用可能で非表示にされるインデクサーに限定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="a3946-1132">次の規則は、セット内の各インデクサー `S.I` に適用されます。 `S` は、インデクサー `I` が宣言されている型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="a3946-1133">`I` が `A` ([該当する関数メンバー](expressions.md#applicable-function-member)) に対して適用されない場合は、`I` がセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="a3946-1134">`I` が `A` ([適用可能な関数メンバー](expressions.md#applicable-function-member)) に対して適用できる場合、`S` の基本型で宣言されたすべてのインデクサーがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="a3946-1135">`I` が `A` ([適用可能な関数メンバー](expressions.md#applicable-function-member)) に対して適用可能で、`S` が `object`以外のクラス型である場合、インターフェイスで宣言されているすべてのインデクサーがセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="a3946-1136">結果として得られる一連のインデクサーが空の場合、適用可能なインデクサーは存在せず、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="a3946-1137">一連の候補インデクサーの最適なインデクサーは、[オーバーロード解決](expressions.md#overload-resolution)のオーバーロードの解決規則を使用して識別されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="a3946-1138">1つの最適なインデクサーを識別できない場合、インデクサーアクセスはあいまいであり、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="a3946-1139">*Argument_list*のインデックス式は、左から右に順番に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="a3946-1140">インデクサーアクセスの処理結果は、インデクサーアクセスとして分類される式です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="a3946-1141">インデクサーアクセス式は、上記の手順で特定されたインデクサーを参照し、`P` のインスタンス式と、関連付けられた `A`の引数リストを持ちます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="a3946-1142">インデクサーアクセスは、使用されるコンテキストに応じて、インデクサーの*get アクセサー*または*set アクセサー*のいずれかを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="a3946-1143">インデクサーアクセスが割り当てのターゲットである場合は、 *set アクセサー*が呼び出され、新しい値 ([単純な割り当て](expressions.md#simple-assignment)) が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="a3946-1144">それ以外の場合は、 *get アクセサー*を呼び出して現在の値 ([式の値](expressions.md#values-of-expressions)) を取得します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="a3946-1145">このアクセス権</span><span class="sxs-lookup"><span data-stu-id="a3946-1145">This access</span></span>

<span data-ttu-id="a3946-1146">*This_access*は、予約語 `this`で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="a3946-1147">*This_access*は、インスタンスコンストラクター、インスタンスメソッド、またはインスタンスアクセサーの*ブロック*内でのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="a3946-1148">次のいずれかの意味があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="a3946-1149">クラスのインスタンスコンストラクター内の*primary_expression*で `this` を使用すると、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="a3946-1150">値の型は、使用が発生するクラスのインスタンス型 ([インスタンス型](classes.md#the-instance-type)) であり、値は構築されているオブジェクトへの参照です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="a3946-1151">クラスのインスタンスメソッドまたはインスタンスアクセサー内の*primary_expression*で `this` を使用すると、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="a3946-1152">値の型は、使用が発生するクラスのインスタンスの型 ([インスタンス型](classes.md#the-instance-type)) です。この値は、メソッドまたはアクセサーが呼び出されたオブジェクトへの参照です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="a3946-1153">構造体のインスタンスコンストラクター内の*primary_expression*で `this` を使用すると、変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="a3946-1154">変数の型は、使用が発生する構造体のインスタンス型 ([インスタンス型](classes.md#the-instance-type)) であり、変数は構築されている構造体を表します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="a3946-1155">構造体のインスタンスコンストラクターの `this` 変数は、構造体型の `out` パラメーターとまったく同じように動作します。特に、これは、インスタンスコンストラクターのすべての実行パスで変数を確実に割り当てる必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="a3946-1156">構造体のインスタンスメソッドまたはインスタンスアクセサー内の*primary_expression*で `this` を使用すると、変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="a3946-1157">変数の型は、使用状況が発生する構造体のインスタンスの型 ([インスタンス型](classes.md#the-instance-type)) です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="a3946-1158">メソッドまたはアクセサーが反復子 ([反復子](classes.md#iterators)) でない場合、`this` 変数は、メソッドまたはアクセサーが呼び出された構造体を表し、構造体型の `ref` パラメーターとまったく同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="a3946-1159">メソッドまたはアクセサーが反復子の場合、`this` 変数は、メソッドまたはアクセサーが呼び出された構造体のコピーを表し、構造体型の値パラメーターとまったく同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="a3946-1160">上記以外のコンテキストで*primary_expression*内の `this` を使用すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="a3946-1161">特に、静的メソッド、静的プロパティアクセサー、またはフィールド宣言の*variable_initializer*で `this` を参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="a3946-1162">基本アクセス</span><span class="sxs-lookup"><span data-stu-id="a3946-1162">Base access</span></span>

<span data-ttu-id="a3946-1163">*Base_access*は、予約語 `base` の後に "`.`" トークン、識別子、または角かっこで囲まれた*argument_list*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="a3946-1164">*Base_access*は、現在のクラスまたは構造体の同じ名前のメンバーによって隠ぺいされている基底クラスのメンバーにアクセスするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="a3946-1165">*Base_access*は、インスタンスコンストラクター、インスタンスメソッド、またはインスタンスアクセサーの*ブロック*内でのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="a3946-1166">クラスまたは構造体で `base.I` が発生した場合、`I` は、そのクラスまたは構造体の基底クラスのメンバーを示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="a3946-1167">同様に、クラスで `base[E]` が発生した場合は、適用可能なインデクサーが基本クラスに存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="a3946-1168">バインド時には、フォーム `base.I` と `base[E]` の*base_access*式が `((B)this).I` および `((B)this)[E]`に記述されているかのように正確に評価されます。 `B` は、コンストラクトが発生するクラスまたは構造体の基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="a3946-1169">したがって、`base.I` と `base[E]` は `this.I` と `this[E]`に対応しますが、`this` は基本クラスのインスタンスとして表示される点が異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="a3946-1170">*Base_access*が仮想関数メンバー (メソッド、プロパティ、またはインデクサー) を参照する場合、実行時に呼び出す関数メンバーの決定 ([動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) が変更されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="a3946-1171">呼び出される関数メンバーを決定するには、関数メンバーの最も派生された実装 ([仮想メソッド](classes.md#virtual-methods)) を検索します。これは、`this`の実行時の型に対してではなく、`B` に対して行われます。これは、非基本アクセスの場合と同様です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="a3946-1172">したがって、`virtual` 関数メンバーの `override` 内では、 *base_access*を使用して、関数メンバーの継承された実装を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="a3946-1173">*Base_access*によって参照される関数メンバーが abstract の場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="a3946-1174">後置インクリメント演算子と後置デクリメント演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="a3946-1175">後置インクリメントまたはデクリメント演算のオペランドには、変数、プロパティアクセス、またはインデクサーアクセスとして分類される式を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="a3946-1176">演算の結果は、オペランドと同じ型の値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="a3946-1177">*Primary_expression*にコンパイル時の型が `dynamic` 場合、演算子は動的バインド ([動的バインド](expressions.md#dynamic-binding))、 *post_increment_expression*または*post_decrement_expression*にはコンパイル時の型が `dynamic`、実行時には*primary_expression*のランタイム型を使用して次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="a3946-1178">後置インクリメントまたはデクリメント演算のオペランドがプロパティまたはインデクサーアクセスの場合、プロパティまたはインデクサーには、`get` と `set` の両方のアクセサーが必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="a3946-1179">そうでない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-1180">単項演算子のオーバーロードの解決 ([単項演算子のオーバーロードの解決](expressions.md#unary-operator-overload-resolution)) は、特定の演算子の実装を選択するために適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-1181">定義済みの `++` と `--` の演算子は、`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、列挙型の各型に存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="a3946-1182">定義済みの `++` 演算子は、オペランドに1を加算して生成された値を返します。定義済みの `--` 演算子は、オペランドから1を減算して生成された値を返します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="a3946-1183">`checked` コンテキストでは、この加算または減算の結果が結果型の範囲外で、結果型が整数型または列挙型の場合、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="a3946-1184">`x++` または `x--` フォームの後置インクリメントまたはデクリメント操作の実行時処理は、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="a3946-1185">`x` が変数として分類される場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="a3946-1186">変数を生成するために `x` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="a3946-1187">`x` の値が保存されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="a3946-1188">選択した演算子は、引数として `x` の保存値を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="a3946-1189">演算子によって返される値は、`x`の評価によって指定された場所に格納されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="a3946-1190">`x` の保存された値は、操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="a3946-1191">`x` がプロパティまたはインデクサーアクセスとして分類される場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="a3946-1192">インスタンス式 (`x` が `static`でない場合) と `x` に関連付けられている引数リスト (`x` がインデクサーアクセスの場合) が評価され、その結果が後続の `get` および `set` アクセサー呼び出しで使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="a3946-1193">`x` の `get` アクセサーが呼び出され、戻り値が保存されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="a3946-1194">選択した演算子は、引数として `x` の保存値を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="a3946-1195">`x` の `set` アクセサーは、`value` 引数として演算子によって返された値を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="a3946-1196">`x` の保存された値は、操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="a3946-1197">`++` 演算子と `--` 演算子では、プレフィックスの[インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)もサポートされています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="a3946-1198">通常、`x++` または `x--` の結果は、操作の前の `x` の値になります。一方、`++x` または `--x` の結果は、操作後の `x` の値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="a3946-1199">どちらの場合も、`x` 自体の値は、操作の後で同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="a3946-1200">`operator ++` または `operator --` の実装は、後置表記またはプレフィックス表記のいずれかを使用して呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="a3946-1201">2つの表記に対して個別の演算子を実装することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="a3946-1202">新しい演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1202">The new operator</span></span>

<span data-ttu-id="a3946-1203">`new` 演算子は、型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="a3946-1204">`new` 式には、次の3つの形式があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="a3946-1205">オブジェクト作成式は、クラス型と値型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="a3946-1206">配列作成式は、配列型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="a3946-1207">デリゲート作成式は、デリゲート型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="a3946-1208">`new` 演算子は、型のインスタンスの作成を意味しますが、必ずしもメモリの動的割り当てを意味するわけではありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="a3946-1209">特に、値型のインスタンスは、それが存在する変数以外に追加のメモリを必要としません。値型のインスタンスを作成するために `new` を使用する場合、動的な割り当ては発生しません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="a3946-1210">オブジェクト作成式</span><span class="sxs-lookup"><span data-stu-id="a3946-1210">Object creation expressions</span></span>

<span data-ttu-id="a3946-1211">*Object_creation_expression*は、 *class_type*または*value_type*の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

<span data-ttu-id="a3946-1212">*Object_creation_expression*の*種類*は、 *class_type*、 *value_type* 、または*type_parameter*である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="a3946-1213">*型*を `abstract` *class_type*にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="a3946-1214">省略可能な*argument_list* ([引数リスト](expressions.md#argument-lists)) は、*型*が*class_type*または*struct_type*の場合にのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="a3946-1215">オブジェクトの作成式では、コンストラクターの引数リストと、オブジェクト初期化子またはコレクション初期化子が含まれている場合に囲むかっこを省略できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="a3946-1216">コンストラクターの引数リストと囲んでいるかっこを省略することは、空の引数リストを指定することと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="a3946-1217">オブジェクト初期化子またはコレクション初期化子を含むオブジェクト作成式の処理では、最初にインスタンスコンストラクターを処理した後、オブジェクト初期化子 ([オブジェクト初期化](expressions.md#object-initializers)子) またはコレクション初期化子 ([コレクション](expressions.md#collection-initializers)初期化子) によって指定されたメンバーまたは要素の初期化を処理します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="a3946-1218">省略可能な*argument_list*内のいずれかの引数にコンパイル時の型が `dynamic` 場合、 *object_creation_expression*は動的にバインドされ ([動的バインディング](expressions.md#dynamic-binding))、次の規則は、コンパイル時の型 `dynamic`を持つ*argument_list*の引数の実行時の型を使用して実行時に適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="a3946-1219">ただし、オブジェクトの作成では、「[動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)」で説明されているように、コンパイル時間の制限があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="a3946-1220">フォーム `new T(A)`の*object_creation_expression*のバインド時の処理。 `T` は*class_type*または*value_type*で、`A` は省略可能な*argument_list*で、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="a3946-1221">`T` が*value_type*で、`A` が存在しない場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="a3946-1222">*Object_creation_expression*は、既定のコンストラクターの呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="a3946-1223">*Object_creation_expression*の結果は `T`型の値です。つまり、[システムの ValueType 型](types.md#the-systemvaluetype-type)で定義されている `T` の既定値です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="a3946-1224">それ以外の場合、`T` が*type_parameter*で `A` が存在しない場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="a3946-1225">`T`に値型の制約またはコンストラクターの制約 ([型パラメーターの制約](classes.md#type-parameter-constraints)) が指定されていない場合、バインディング時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="a3946-1226">*Object_creation_expression*の結果は、型パラメーターのバインド先であるランタイム型の値です。つまり、その型の既定のコンストラクターを呼び出した結果です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="a3946-1227">実行時の型は、参照型または値型にすることができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="a3946-1228">それ以外の場合、`T` が*class_type*または*struct_type*の場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="a3946-1229">`T` が `abstract` *class_type*の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="a3946-1230">呼び出すインスタンスコンストラクターは、[オーバーロード解決](expressions.md#overload-resolution)のオーバーロードの解決規則を使用して決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="a3946-1231">候補インスタンスコンストラクターのセットは、`A` ([適用可能な関数メンバー](expressions.md#applicable-function-member)) に対して適用できる、`T` で宣言されたすべてのアクセス可能なインスタンスコンストラクターで構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="a3946-1232">候補インスタンスコンストラクターのセットが空である場合、または1つの最適なインスタンスコンストラクターを識別できない場合は、バインディング時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="a3946-1233">*Object_creation_expression*の結果は `T`型の値です。つまり、上記の手順で特定されたインスタンスコンストラクターを呼び出すことによって生成される値です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="a3946-1234">それ以外の場合、 *object_creation_expression*は無効であり、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-1235">*Object_creation_expression*が動的にバインドされている場合でも、コンパイル時の型は引き続き `T`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="a3946-1236">フォーム `new T(A)`の*object_creation_expression*の実行時処理。 `T` は*class_type*または*struct_type*で、`A` は省略可能な*argument_list*で、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="a3946-1237">`T` が*class_type*の場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="a3946-1238">クラス `T` の新しいインスタンスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="a3946-1239">新しいインスタンスの割り当てに使用できるメモリが不足している場合は、`System.OutOfMemoryException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="a3946-1240">新しいインスタンスのすべてのフィールドは、既定値 ([既定値](variables.md#default-values)) に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="a3946-1241">インスタンスコンストラクターは、関数メンバー呼び出しの規則 ([動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) に従って呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="a3946-1242">新しく割り当てられたインスタンスへの参照がインスタンスコンストラクターに自動的に渡され、そのコンストラクター内から `this`としてインスタンスにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="a3946-1243">`T` が*struct_type*の場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="a3946-1244">`T` 型のインスタンスは、一時的なローカル変数を割り当てることによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="a3946-1245">*Struct_type*のインスタンスコンストラクターは、作成されるインスタンスの各フィールドに値を確実に割り当てる必要があるため、一時変数を初期化する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="a3946-1246">インスタンスコンストラクターは、関数メンバー呼び出しの規則 ([動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) に従って呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="a3946-1247">新しく割り当てられたインスタンスへの参照がインスタンスコンストラクターに自動的に渡され、そのコンストラクター内から `this`としてインスタンスにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="a3946-1248">オブジェクト初期化子</span><span class="sxs-lookup"><span data-stu-id="a3946-1248">Object initializers</span></span>

<span data-ttu-id="a3946-1249">オブジェクト***初期化子***は、オブジェクトの0個以上のフィールド、プロパティ、またはインデックス付き要素の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

<span data-ttu-id="a3946-1250">オブジェクト初期化子は、`{` および `}` トークンで囲まれ、コンマで区切られた一連のメンバー初期化子で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="a3946-1251">各*member_initializer*は、初期化のターゲットを指定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="a3946-1252">*識別子*は、初期化されるオブジェクトのアクセス可能なフィールドまたはプロパティに名前を付ける必要があります。一方、角かっこで囲まれた*argument_list*は、初期化されるオブジェクトのアクセス可能なインデクサーの引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="a3946-1253">オブジェクト初期化子に、同じフィールドまたはプロパティに対して複数のメンバー初期化子が含まれていると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="a3946-1254">各*initializer_target*には、等号と、式、オブジェクト初期化子、またはコレクション初期化子のいずれかが続きます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="a3946-1255">オブジェクト初期化子内の式は、初期化中の新しく作成されたオブジェクトを参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="a3946-1256">等号の後にある式を指定するメンバー初期化子は、ターゲットへの代入 ([単純な代入](expressions.md#simple-assignment)) と同じ方法で処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="a3946-1257">等号の後にオブジェクト初期化子を指定するメンバー初期化子は、入れ子になったオブジェクト***初期化子***、つまり埋め込みオブジェクトの初期化です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="a3946-1258">フィールドまたはプロパティに新しい値を割り当てる代わりに、入れ子になったオブジェクト初期化子の割り当ては、フィールドまたはプロパティのメンバーへの割り当てとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="a3946-1259">入れ子になったオブジェクト初期化子は、値型のプロパティ、または値の型を持つ読み取り専用フィールドには適用できません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="a3946-1260">等号の後にコレクション初期化子を指定するメンバー初期化子は、埋め込みコレクションを初期化します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="a3946-1261">ターゲットフィールド、プロパティ、またはインデクサーに新しいコレクションを割り当てる代わりに、初期化子に指定された要素が、ターゲットによって参照されるコレクションに追加されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="a3946-1262">ターゲットは、[コレクション初期化子](expressions.md#collection-initializers)に指定された要件を満たすコレクション型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="a3946-1263">インデックス初期化子への引数は、常に1回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="a3946-1264">したがって、引数が最終的に使用されない場合 (たとえば、入れ子になった空の初期化子があるため)、副作用について評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="a3946-1265">次のクラスは、2つの座標を持つ点を表します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="a3946-1266">`Point` のインスタンスは、次のようにして作成および初期化できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="a3946-1267">これは、</span><span class="sxs-lookup"><span data-stu-id="a3946-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="a3946-1268">`__a` は、非表示であり、アクセスできない一時的な変数です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="a3946-1269">次のクラスは、2つの点から作成された四角形を表します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="a3946-1270">`Rectangle` のインスタンスは、次のようにして作成および初期化できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="a3946-1271">これは、</span><span class="sxs-lookup"><span data-stu-id="a3946-1271">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
<span data-ttu-id="a3946-1272">`__r`、`__p1`、および `__p2` は、非表示でアクセスできない一時的な変数です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="a3946-1273">`Rectangle`のコンストラクターが2つの埋め込み `Point` インスタンスを割り当てる場合</span><span class="sxs-lookup"><span data-stu-id="a3946-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="a3946-1274">次のコンストラクトは、新しいインスタンスを割り当てる代わりに、埋め込み `Point` インスタンスを初期化するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="a3946-1275">これは、</span><span class="sxs-lookup"><span data-stu-id="a3946-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="a3946-1276">次の例では、C の適切な定義が指定されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1276">Given an appropriate definition of C, the following example:</span></span>
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
<span data-ttu-id="a3946-1277">は、次の一連の代入と同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1277">is equivalent to this series of assignments:</span></span>
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
<span data-ttu-id="a3946-1278">`__c`などは、ソースコードからは見えず、アクセスできない生成変数です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="a3946-1279">`[0,0]` の引数は1回だけ評価され、使用されていない場合でも `[1,2]` の引数は1回だけ評価されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="a3946-1280">コレクション初期化子</span><span class="sxs-lookup"><span data-stu-id="a3946-1280">Collection initializers</span></span>

<span data-ttu-id="a3946-1281">コレクション初期化子は、コレクションの要素を指定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1281">A collection initializer specifies the elements of a collection.</span></span>

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

<span data-ttu-id="a3946-1282">コレクション初期化子は、`{` および `}` トークンで囲まれ、コンマで区切られた要素初期化子のシーケンスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="a3946-1283">各要素初期化子は、初期化されるコレクションオブジェクトに追加する要素を指定します。また、`{` および `}` トークンで囲まれ、コンマで区切られた式のリストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="a3946-1284">単一式の要素初期化子は、かっこを使用せずに記述できますが、メンバー初期化子とのあいまいさを避けるために代入式にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="a3946-1285">*Non_assignment_expression*の運用環境は、[式](expressions.md#expression)で定義されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="a3946-1286">次に、コレクション初期化子を含むオブジェクト作成式の例を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="a3946-1287">コレクション初期化子が適用されるコレクションオブジェクトは、`System.Collections.IEnumerable` を実装する型であるか、またはコンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="a3946-1288">コレクション初期化子は、指定された要素ごとに、引数リストとして要素初期化子の式リストを使用してターゲットオブジェクトの `Add` メソッドを呼び出し、各呼び出しに対して通常のメンバー参照とオーバーロードの解決を適用します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="a3946-1289">したがって、コレクションオブジェクトには、要素初期化子ごとに `Add` という名前の適用可能なインスタンスまたは拡張メソッドが必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="a3946-1290">次のクラスは、名前と電話番号のリストを持つ連絡先を表します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="a3946-1291">`List<Contact>` は、次のようにして作成および初期化できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
<span data-ttu-id="a3946-1292">これは、</span><span class="sxs-lookup"><span data-stu-id="a3946-1292">which has the same effect as</span></span>
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
<span data-ttu-id="a3946-1293">`__clist`、`__c1`、および `__c2` は、非表示でアクセスできない一時的な変数です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="a3946-1294">配列作成式</span><span class="sxs-lookup"><span data-stu-id="a3946-1294">Array creation expressions</span></span>

<span data-ttu-id="a3946-1295">*Array_creation_expression*は、 *array_type*の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="a3946-1296">最初の形式の配列作成式は、式リストから各式を削除した結果として得られる型の配列インスタンスを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="a3946-1297">たとえば、配列作成式 `new int[10,20]` によって `int[,]`型の配列インスタンスが生成され、配列作成式 `new int[10][,]` によって `int[][,]`型の配列が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="a3946-1298">式リストの各式は、型 `int`、`uint`、`long`、または `ulong`であるか、またはこれらの型の1つ以上に暗黙的に変換できなければなりません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="a3946-1299">各式の値によって、新しく割り当てられた配列インスタンス内の対応する次元の長さが決まります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="a3946-1300">配列の次元の長さは負ではない必要があるため、式の一覧に負の値を持つ*constant_expression*を持つコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="a3946-1301">Unsafe コンテキスト ([unsafe](unsafe-code.md#unsafe-contexts)コンテキスト) の場合を除き、配列のレイアウトは指定されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="a3946-1302">最初の形式の配列作成式に配列初期化子が含まれている場合、式リスト内の各式は定数である必要があり、式リストで指定されたランクと次元の長さは配列初期化子の式と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="a3946-1303">2番目または3番目の形式の配列作成式では、指定された配列型またはランク指定子のランクが配列初期化子のランクと一致している必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="a3946-1304">個々の次元の長さは、配列初期化子の対応する入れ子のレベルそれぞれに含まれる要素の数から推論されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="a3946-1305">したがって、式</span><span class="sxs-lookup"><span data-stu-id="a3946-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="a3946-1306">完全に対応</span><span class="sxs-lookup"><span data-stu-id="a3946-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="a3946-1307">3番目の形式の配列作成式は、暗黙的に***型指定された配列作成式***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="a3946-1308">2番目の形式に似ていますが、配列の要素の型は明示的に指定されていませんが、配列初期化子内の式のセットの最適な共通型 ([式セットの最適な共通型](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) として決定される点が異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="a3946-1309">多次元配列の場合、つまり、 *rank_specifier*に少なくとも1つのコンマが含まれている場合、このセットは、入れ子になった*array_initializer*s で見つかったすべての*式*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="a3946-1310">配列初期化子の詳細については、「[配列初期化子](arrays.md#array-initializers)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="a3946-1311">配列作成式を評価した結果は、新たに割り当てられた配列インスタンスへの参照として、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="a3946-1312">配列作成式の実行時の処理は、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="a3946-1313">*Expression_list*のディメンション長式は、左から右の順に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="a3946-1314">各式を評価すると、次のいずれかの型への暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) が実行されます: `int`、`uint`、`long`、`ulong`。</span><span class="sxs-lookup"><span data-stu-id="a3946-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="a3946-1315">暗黙的な変換が存在する、この一覧の最初の型が選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="a3946-1316">式またはそれ以降の暗黙的な変換によって例外が発生した場合、それ以降の式は評価されず、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="a3946-1317">ディメンションの長さの計算値は、次のように検証されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="a3946-1318">1つ以上の値が0未満の場合は、`System.OverflowException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="a3946-1319">次元の長さを指定して配列インスタンスが割り当てられています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="a3946-1320">新しいインスタンスの割り当てに使用できるメモリが不足している場合は、`System.OutOfMemoryException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="a3946-1321">新しい配列インスタンスのすべての要素は、既定値 ([既定値](variables.md#default-values)) に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="a3946-1322">配列作成式に配列初期化子が含まれている場合は、配列初期化子内の各式が評価され、対応する配列要素に代入されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="a3946-1323">評価と割り当ては、配列初期化子で式が記述されている順序で実行されます。つまり、要素は、一番右にあるディメンションが優先され、インデックスの昇順で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="a3946-1324">指定された式の評価、または対応する配列要素への後続の代入によって例外が発生した場合、それ以上要素は初期化されません (残りの要素には既定値が設定されます)。</span><span class="sxs-lookup"><span data-stu-id="a3946-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="a3946-1325">配列作成式では、配列型の要素を含む配列のインスタンス化が許可されますが、このような配列の要素は手動で初期化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="a3946-1326">ステートメントの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="a3946-1327">`int[]`型の100要素を含む1次元配列を作成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="a3946-1328">各要素の初期値は `null`です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="a3946-1329">同じ配列作成式で、サブ配列とステートメントをインスタンス化することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="a3946-1330">コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1330">results in a compile-time error.</span></span> <span data-ttu-id="a3946-1331">代わりに、サブ配列のインスタンス化を手動で実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="a3946-1332">配列の配列に "四角形" の形状がある場合、つまり、サブ配列の長さがすべて同じである場合は、多次元配列を使用する方が効率的です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="a3946-1333">上の例では、配列の配列のインスタンス化によって101オブジェクト (1 つの外部配列と100サブ配列) が作成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="a3946-1334">それに対して</span><span class="sxs-lookup"><span data-stu-id="a3946-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="a3946-1335">1つのオブジェクトと2次元配列だけを作成し、1つのステートメントで割り当てを行います。</span><span class="sxs-lookup"><span data-stu-id="a3946-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="a3946-1336">暗黙的に型指定された配列作成式の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="a3946-1337">最後の式では、`int` も `string` も暗黙的に他方に変換できないため、コンパイル時エラーが発生します。したがって、最適な共通型はありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="a3946-1338">この場合は、`object[]`する型を指定するなど、明示的に型指定された配列作成式を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="a3946-1339">または、要素の1つを共通の基本型にキャストして、その要素の型が推論されるようにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="a3946-1340">暗黙的に型指定された配列作成式を匿名オブジェクト初期化子 ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) と組み合わせて、匿名型のデータ構造を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="a3946-1341">例 :</span><span class="sxs-lookup"><span data-stu-id="a3946-1341">For example:</span></span>
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="a3946-1342">デリゲート作成式</span><span class="sxs-lookup"><span data-stu-id="a3946-1342">Delegate creation expressions</span></span>

<span data-ttu-id="a3946-1343">*Delegate_creation_expression*は、 *delegate_type*の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="a3946-1344">デリゲート作成式の引数には、メソッドグループ、匿名関数、またはコンパイル時の型 `dynamic` または*delegate_type*のいずれかの値を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="a3946-1345">引数がメソッドグループの場合、メソッドと、インスタンスメソッドの場合は、デリゲートを作成する対象のオブジェクトを識別します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="a3946-1346">引数が匿名関数の場合、デリゲートターゲットのパラメーターとメソッドの本体が直接定義されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="a3946-1347">引数が値の場合は、コピーを作成する対象のデリゲートインスタンスを識別します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="a3946-1348">*式*に `dynamic`コンパイル時の型がある場合、 *delegate_creation_expression*は動的にバインドされ ([動的バインディング](expressions.md#dynamic-binding))、次の規則は実行時に*式*の実行時の型を使用して適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="a3946-1349">それ以外の場合は、コンパイル時に規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="a3946-1350">フォーム `new D(E)`の*delegate_creation_expression*のバインド時の処理。 `D` は*delegate_type*で、`E` は*式*です。は、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="a3946-1351">`E` がメソッドグループの場合、デリゲート作成式は、メソッドグループの変換 ([メソッドグループ](conversions.md#method-group-conversions)の変換) と同じ方法で `E` から `D`に処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="a3946-1352">`E` が匿名関数の場合、デリゲート作成式は、匿名関数の変換 ([匿名関数](conversions.md#anonymous-function-conversions)の変換) と同じ方法で、`E` から `D`に処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="a3946-1353">`E` が値の場合、`E` は `D`と互換性がある ([デリゲート宣言](delegates.md#delegate-declarations)) 必要があります。また、結果は、`E`と同じ呼び出しリストを参照する、`D` 型の新しく作成されたデリゲートへの参照になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="a3946-1354">`E` が `D`と互換性がない場合は、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="a3946-1355">フォーム `new D(E)`の*delegate_creation_expression*の実行時処理。 `D` は*delegate_type*で、`E` は*式*です。は、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="a3946-1356">`E` がメソッドグループの場合、デリゲート作成式は、メソッドグループの変換 ([メソッドグループ](conversions.md#method-group-conversions)の変換) として `E` から `D`に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="a3946-1357">`E` が匿名関数の場合、デリゲートの作成は、`E` から `D` ([匿名関数の変換](conversions.md#anonymous-function-conversions)) への匿名関数の変換として評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="a3946-1358">`E` が*delegate_type*の値の場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="a3946-1359">`E` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1359">`E` is evaluated.</span></span> <span data-ttu-id="a3946-1360">この評価によって例外が発生した場合、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="a3946-1361">`E` の値が `null`場合は `System.NullReferenceException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="a3946-1362">`D` デリゲート型の新しいインスタンスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="a3946-1363">新しいインスタンスの割り当てに使用できるメモリが不足している場合は、`System.OutOfMemoryException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="a3946-1364">`E`によって指定されたデリゲートインスタンスと同じ呼び出しリストを使用して、新しいデリゲートインスタンスが初期化されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="a3946-1365">デリゲートの呼び出しリストは、デリゲートがインスタンス化されるときに決定され、その後、デリゲートの有効期間全体にわたって定数のままになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="a3946-1366">つまり、作成されたデリゲートのターゲット呼び出し可能エンティティを変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="a3946-1367">2つのデリゲートを結合する場合、または一方を別のデリゲート[宣言 (デリゲート宣言](delegates.md#delegate-declarations)) から削除する場合は、新しいデリゲートの結果はです。既存のデリゲートの内容が変更されていません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="a3946-1368">プロパティ、インデクサー、ユーザー定義の演算子、インスタンスコンストラクター、デストラクター、または静的コンストラクターを参照するデリゲートを作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="a3946-1369">前述のように、メソッドグループからデリゲートを作成すると、デリゲートの仮パラメーターリストと戻り値の型によって、どのオーバーロードされたメソッドを選択するかが決まります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="a3946-1370">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-1370">In the example</span></span>
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
<span data-ttu-id="a3946-1371">`A.f` フィールドは、2番目の `Square` メソッドを参照するデリゲートで初期化されます。これは、そのメソッドが `DoubleFunc`の仮パラメーターリストおよび戻り値の型と完全に一致するためです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="a3946-1372">2番目の `Square` メソッドが存在しない場合、コンパイル時エラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="a3946-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="a3946-1373">匿名オブジェクト作成式</span><span class="sxs-lookup"><span data-stu-id="a3946-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="a3946-1374">*Anonymous_object_creation_expression*は、匿名型のオブジェクトを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

<span data-ttu-id="a3946-1375">匿名オブジェクト初期化子は、匿名型を宣言し、その型のインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="a3946-1376">匿名型は、`object`から直接継承する無名のクラス型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="a3946-1377">匿名型のメンバーは、型のインスタンスを作成するために使用される匿名オブジェクト初期化子から推論される読み取り専用プロパティのシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="a3946-1378">具体的には、フォームの匿名オブジェクト初期化子</span><span class="sxs-lookup"><span data-stu-id="a3946-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="a3946-1379">フォームの匿名型を宣言します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1379">declares an anonymous type of the form</span></span>
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
<span data-ttu-id="a3946-1380">各 `Tx` は `ex`対応する式の型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="a3946-1381">*Member_declarator*で使用する式には型が必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="a3946-1382">したがって、 *member_declarator*内の式が null または匿名関数である場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="a3946-1383">また、式に安全でない型がある場合のコンパイル時エラーでもあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="a3946-1384">匿名型の名前と `Equals` メソッドに対するパラメーターの名前は、コンパイラによって自動的に生成され、プログラムテキストで参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="a3946-1385">同じプログラム内で、同じ名前の一連のプロパティとコンパイル時の型を同じ順序で指定する2つの匿名オブジェクト初期化子は、同じ匿名型のインスタンスを生成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="a3946-1386">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="a3946-1387">`p1` と `p2` が同じ匿名型であるため、最後の行の代入が許可されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="a3946-1388">匿名型の `Equals` および `GetHashcode` メソッドは、`object`から継承されたメソッドをオーバーライドし、プロパティの `Equals` および `GetHashcode` の観点から定義されます。これにより、同じ匿名型の2つのインスタンスが等しい場合に限り、同じ匿名型の2つのインスタンスが等しいようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="a3946-1389">メンバー宣言子は、単純な名前 (型の[推定](expressions.md#type-inference))、メンバーアクセス ([動的なオーバーロードの解決のコンパイル時のチェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))、基本アクセス ([基本アクセス](expressions.md#base-access))、または null 条件付きのメンバーアクセス ([プロジェクション初期化子としての null 条件式](expressions.md#null-conditional-expressions-as-projection-initializers)) に省略できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="a3946-1390">これは、***射影初期化子***と呼ばれ、同じ名前を持つプロパティの宣言と代入のための短縮形です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="a3946-1391">具体的には、フォームのメンバー宣言子</span><span class="sxs-lookup"><span data-stu-id="a3946-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="a3946-1392">は、それぞれ次のものに相当します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="a3946-1393">したがって、プロジェクション初期化子では、値が割り当てられる値とフィールドまたはプロパティの両方が*識別子*によって選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="a3946-1394">直感的に言えば、プロジェクション初期化子は、値だけでなく、値の名前でもあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="a3946-1395">Typeof 演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1395">The typeof operator</span></span>

<span data-ttu-id="a3946-1396">`typeof` 演算子は、型の `System.Type` オブジェクトを取得するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

<span data-ttu-id="a3946-1397">*Typeof_expression*の最初の形式は、`typeof` キーワードとそれに続くかっこで囲まれた*型*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="a3946-1398">この形式の式の結果は、指定された型の `System.Type` オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="a3946-1399">指定された型には `System.Type` オブジェクトが1つだけあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="a3946-1400">つまり、型 `T`の場合、`typeof(T) == typeof(T)` は常に true になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="a3946-1401">*型*を `dynamic`にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="a3946-1402">2番目の形式の*typeof_expression*は、`typeof` キーワードとそれに続くかっこで囲まれた*unbound_type_name*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="a3946-1403">*Unbound_type_name*は*type_name* ([名前空間と型名](basic-concepts.md#namespace-and-type-names)) と非常によく似ていますが、 *type_name*に*type_argument_list*が含まれる*generic_dimension_specifier*が*unbound_type_name*に含まれている点が異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="a3946-1404">*Typeof_expression*のオペランドが*unbound_type_name*と*type_name*の両方の文法を満たすトークンのシーケンスである場合、つまり、 *generic_dimension_specifier*も*type_argument_list*も含まれていない場合、トークンのシーケンスは*type_name*と見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="a3946-1405">*Unbound_type_name*の意味は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="a3946-1406">各*generic_dimension_specifier*を同じ数のコンマを持つ*type_argument_list*に置き換えることにより、トークンのシーケンスを*type_name*に変換します。各*type_argument*には `object` キーワードを使用します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="a3946-1407">すべての型パラメーター制約を無視して、結果の*type_name*を評価します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="a3946-1408">*Unbound_type_name*は、結果の構築された型 ([バインドおよびバインド](types.md#bound-and-unbound-types)解除された型) に関連付けられたバインドされていないジェネリック型に解決されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="a3946-1409">*Typeof_expression*の結果は、結果としてバインドされていないジェネリック型の `System.Type` オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="a3946-1410">3番目の形式の*typeof_expression*は、`typeof` キーワードの後にかっこで囲んだ `void` キーワードで構成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="a3946-1411">この形式の式の結果は、型が存在しないことを表す `System.Type` オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="a3946-1412">`typeof(void)` によって返される型オブジェクトは、任意の型に対して返される型オブジェクトとは異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="a3946-1413">この特別な型のオブジェクトは、言語のメソッドへのリフレクションを可能にするクラスライブラリで役立ちます。これらのメソッドは、void メソッドを含むメソッドの戻り値の型を `System.Type`のインスタンスと共に表す方法を必要とします。</span><span class="sxs-lookup"><span data-stu-id="a3946-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="a3946-1414">`typeof` 演算子は、型パラメーターで使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="a3946-1415">結果として、型パラメーターにバインドされたランタイム型の `System.Type` オブジェクトが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="a3946-1416">`typeof` 演算子は、構築された型またはバインドされていないジェネリック型 ([バインドおよび](types.md#bound-and-unbound-types)バインド解除された型) でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="a3946-1417">バインドされていないジェネリック型の `System.Type` オブジェクトは、インスタンス型の `System.Type` オブジェクトと同じではありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="a3946-1418">インスタンス型は、実行時に常にクローズ構築型です。そのため、`System.Type` オブジェクトは使用するランタイム型引数に依存しますが、バインドされていないジェネリック型には型引数がありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="a3946-1419">例</span><span class="sxs-lookup"><span data-stu-id="a3946-1419">The example</span></span>
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
<span data-ttu-id="a3946-1420">では、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1420">produces the following output:</span></span>
```console
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

<span data-ttu-id="a3946-1421">`int` と `System.Int32` は同じ型であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="a3946-1422">また、`typeof(X<>)` の結果は型引数に依存しないことに注意してくださいが、`typeof(X<T>)` の結果は異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="a3946-1423">checked 演算子と unchecked 演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="a3946-1424">`checked` 演算子と `unchecked` 演算子を使用して、整数型の算術演算および変換の***オーバーフローチェックコンテキスト***を制御します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="a3946-1425">`checked` 演算子は checked コンテキストで含まれている式を評価し、`unchecked` 演算子は、チェックされていないコンテキストで含まれている式を評価します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="a3946-1426">*Checked_expression*または*unchecked_expression*は、指定されたオーバーフローチェックコンテキストで含まれる式が評価される点を除いて、 *parenthesized_expression* ([かっこで囲ま](expressions.md#parenthesized-expressions)れた式) と正確に対応します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="a3946-1427">オーバーフローチェックコンテキストは、`checked` および `unchecked` ステートメント ([checked ステートメントと unchecked ステートメント](statements.md#the-checked-and-unchecked-statements)) を使用して制御することもできます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="a3946-1428">次の操作は、`checked` によって確立されたオーバーフローチェックコンテキストと `unchecked` の演算子およびステートメントによって影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="a3946-1429">オペランドが整数型の場合、定義済みの `++` と `--` の単項演算子 ([後置インクリメントおよびデクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、[前置インクリメントおよびデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="a3946-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="a3946-1430">オペランドが整数型の場合、定義済みの `-` 単項演算子 ([単項マイナス演算子](expressions.md#unary-minus-operator))。</span><span class="sxs-lookup"><span data-stu-id="a3946-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="a3946-1431">両方のオペランドが整数型の場合、定義済みの `+`、`-`、`*`、および `/` 二項演算子 ([算術演算子](expressions.md#arithmetic-operators))。</span><span class="sxs-lookup"><span data-stu-id="a3946-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="a3946-1432">ある整数型から別の整数型へ、または `float` または `double` から整数型への明示的な数値変換 ([明示的な数値変換](conversions.md#explicit-numeric-conversions))。</span><span class="sxs-lookup"><span data-stu-id="a3946-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="a3946-1433">上記のいずれかの操作によって、変換先の型で表現するには大きすぎる結果が生成された場合、演算が実行されるコンテキストによって結果の動作が制御されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="a3946-1434">`checked` コンテキストでは、操作が定数式 ([定数式](expressions.md#constant-expressions)) の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="a3946-1435">それ以外の場合、実行時に操作が実行されると、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="a3946-1436">`unchecked` のコンテキストでは、変換先の型に適合しない上位ビットを破棄することによって結果が切り捨てられます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="a3946-1437">`checked` または `unchecked` の演算子またはステートメントでは囲まれていない非定数式 (実行時に評価される式) の場合、`checked` 評価のために外部要因 (コンパイラスイッチや実行環境の構成など) が呼び出されない限り、既定のオーバーフローチェックコンテキストは `unchecked` ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="a3946-1438">定数式 (コンパイル時に完全に評価できる式) の場合、既定のオーバーフローチェックコンテキストは常に `checked`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="a3946-1439">定数式が `unchecked` コンテキストに明示的に配置されていない限り、式のコンパイル時の評価中にオーバーフローが発生すると、常にコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="a3946-1440">匿名関数の本体は、匿名関数が発生する `checked` または `unchecked` のコンテキストの影響を受けません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="a3946-1441">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-1441">In the example</span></span>
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
<span data-ttu-id="a3946-1442">コンパイル時のエラーは報告されません。コンパイル時に式を評価することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="a3946-1443">実行時には、`F` メソッドによって `System.OverflowException`がスローされ、`G` メソッドによって-727379968 (範囲外の結果の下位32ビット) が返されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="a3946-1444">`H` メソッドの動作は、コンパイルの既定のオーバーフローチェックコンテキストによって異なりますが、`F` または `G`と同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="a3946-1445">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-1445">In the example</span></span>
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
<span data-ttu-id="a3946-1446">`F` および `H` の定数式を評価するときにオーバーフローが発生すると、式は `checked` コンテキストで評価されるため、コンパイル時のエラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="a3946-1447">`G`で定数式を評価するときにもオーバーフローが発生しますが、評価は `unchecked` のコンテキストで行われるため、オーバーフローは報告されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="a3946-1448">`checked` 演算子と `unchecked` 演算子は、"`(`" トークンと "`)`" トークンに含まれる操作のオーバーフローチェックコンテキストにのみ影響します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="a3946-1449">演算子は、含まれている式を評価した結果として呼び出される関数メンバーには影響しません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="a3946-1450">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-1450">In the example</span></span>
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
<span data-ttu-id="a3946-1451">`F` での `checked` の使用は `Multiply`の `x * y` の評価には影響しないため、既定のオーバーフローチェックコンテキストで `x * y` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="a3946-1452">`unchecked` 演算子は、符号付き整数型の定数を16進表記で記述する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="a3946-1453">例 :</span><span class="sxs-lookup"><span data-stu-id="a3946-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="a3946-1454">上記の16進定数はどちらも `uint`型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="a3946-1455">定数は `int` 範囲の外側にあり、`unchecked` 演算子を使用しないと、`int` するキャストによってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="a3946-1456">`checked` および `unchecked` の演算子とステートメントを使用すると、プログラマはいくつかの数値計算の特定の側面を制御できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="a3946-1457">ただし、一部の数値演算子の動作は、そのオペランドのデータ型によって異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="a3946-1458">たとえば、2つの10進数を乗算すると、明示的な `unchecked` コンストラクト内であってもオーバーフローでは例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="a3946-1459">同様に、2つの浮動小数点数を乗算しても、明示的な `checked` コンストラクト内であってもオーバーフローで例外が発生することはありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="a3946-1460">また、他の演算子は、既定でも明示的でも、チェックモードの影響を受けません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="a3946-1461">既定値の式</span><span class="sxs-lookup"><span data-stu-id="a3946-1461">Default value expressions</span></span>

<span data-ttu-id="a3946-1462">既定値の式は、型の既定値 ([既定](variables.md#default-values)値) を取得するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="a3946-1463">通常、型パラメーターには既定値式が使用されます。これは、型パラメーターが値型または参照型であるかどうかが不明な場合があるためです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="a3946-1464">(型パラメーターが参照型であることがわかっている場合を除き、`null` リテラルから型パラメーターへの変換は存在しません)。</span><span class="sxs-lookup"><span data-stu-id="a3946-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="a3946-1465">*Default_value_expression*の*型*が実行時に参照型に評価される場合、結果はその型に変換 `null` れます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="a3946-1466">*Default_value_expression*の*型*が実行時に値型に評価される場合、結果は*Value_type*の既定値 ([既定のコンストラクター](types.md#default-constructors)) になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="a3946-1467">型が参照型または参照型であることがわかっている型パラメーター ([型パラメーターの制約](classes.md#type-parameter-constraints)) の場合、 *default_value_expression*は定数式 ([定数式](expressions.md#constant-expressions)) です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="a3946-1468">また、型が `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、または任意の列挙型のいずれかの値型の場合、 *default_value_expression*は定数式です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="a3946-1469">すべてのの表記</span><span class="sxs-lookup"><span data-stu-id="a3946-1469">Nameof expressions</span></span>

<span data-ttu-id="a3946-1470">*Nameof_expression*は、プログラムエンティティの名前を定数文字列として取得するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

<span data-ttu-id="a3946-1471">文法的に言えば、 *named_entity*のオペランドは常に式です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="a3946-1472">`nameof` は予約されたキーワードではないため、名前の単純な名前 `nameof`の呼び出しによって、名前の指定式は常に構文的にあいまいになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="a3946-1473">互換性の理由により、名前の名前の参照 ([簡易名](expressions.md#simple-names)) が成功した場合、呼び出しが有効であるかどうかに関係なく、式は*invocation_expression*として扱われます。 `nameof`。</span><span class="sxs-lookup"><span data-stu-id="a3946-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="a3946-1474">それ以外の場合は、 *nameof_expression*になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="a3946-1475">*Nameof_expression*の*named_entity*の意味は、式として意味があります。つまり、 *simple_name*、 *base_access*または*member_access*のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="a3946-1476">ただし、[単純名](expressions.md#simple-names)と[メンバーアクセス](expressions.md#member-access)で説明されている参照では、インスタンスメンバーが静的コンテキストで見つかったため、エラーが発生します。 *nameof_expression*では、このようなエラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="a3946-1477">メソッドグループに*type_argument_list*が指定されている*named_entity*は、コンパイル時にエラーになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="a3946-1478">*Named_entity_target*が型 `dynamic`を持つ場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="a3946-1479">*Nameof_expression*は `string`型の定数式であり、実行時には効果がありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="a3946-1480">具体的には、その*named_entity*は評価されず、明確な代入分析 ([単純式の一般的な規則](variables.md#general-rules-for-simple-expressions)) のために無視されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="a3946-1481">この値は、省略可能な最終*type_argument_list*の前の*named_entity*の最後の識別子で、次のように変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="a3946-1482">プレフィックス "`@`" (使用されている場合) は削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="a3946-1483">各*unicode_escape_sequence*は、対応する unicode 文字に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="a3946-1484">*Formatting_characters*はすべて削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="a3946-1485">これらは、識別子が等しいかどうかをテストするときに[識別子](lexical-structure.md#identifiers)に適用される変換と同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="a3946-1486">TODO: 例</span><span class="sxs-lookup"><span data-stu-id="a3946-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="a3946-1487">匿名メソッドの式</span><span class="sxs-lookup"><span data-stu-id="a3946-1487">Anonymous method expressions</span></span>

<span data-ttu-id="a3946-1488">*Anonymous_method_expression*は、匿名関数を定義する2つの方法のうちの1つです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="a3946-1489">これらの詳細については、「[匿名関数の式](expressions.md#anonymous-function-expressions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="a3946-1490">単項演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1490">Unary operators</span></span>

<span data-ttu-id="a3946-1491">`?`、`+`、`-`、`!`、`~`、`++`、`--`、cast、および `await` の各演算子を単項演算子と呼びます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

<span data-ttu-id="a3946-1492">*Unary_expression*のオペランドにコンパイル時の型 `dynamic`がある場合、そのオペランドは動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a3946-1493">この場合、 *unary_expression*のコンパイル時の型が `dynamic`、次に示す解決方法は、実行時にオペランドの実行時の型を使用して行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="a3946-1494">Null 条件演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1494">Null-conditional operator</span></span>

<span data-ttu-id="a3946-1495">Null 条件演算子では、オペランドが null 以外の場合にのみ、演算のリストがオペランドに適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="a3946-1496">それ以外の場合、演算子を適用した結果が `null`されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1496">Otherwise the result of applying the operator is `null`.</span></span>

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="a3946-1497">操作の一覧には、メンバーアクセスと要素アクセス操作 (それ自体が null 条件の場合もあります)、および呼び出しを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="a3946-1498">たとえば、`a.b?[0]?.c()` 式は、 *primary_expression* `a.b` *、null_conditional_operations (* null 条件要素アクセス)、`?[0]` (null 条件メンバーアクセス)、`?.c` (呼び出し) を持つ*null_conditional_expression*です。`()`</span><span class="sxs-lookup"><span data-stu-id="a3946-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="a3946-1499">*Primary_expression* `P`の*null_conditional_expression* `E` については、`E0` を持つ `?` の各*null_conditional_operations*から先頭の `E` を削除することによって得られる式にすることができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="a3946-1500">概念的には、`E0` は、`?`によって表される null チェックに `null`が見つからない場合に評価される式です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="a3946-1501">また、`E`内の最初の*null_conditional_operations*から先頭の `?` を削除することによって得られる式を `E1` します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="a3946-1502">これに*より、プライマリ式*(`?`が1つだけの場合) または別の*null_conditional_expression*が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="a3946-1503">たとえば、`E` が式 `a.b?[0]?.c()`の場合、`E0` は式 `a.b[0].c()`、`E1` は式 `a.b[0]?.c()`です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="a3946-1504">`E0` が何も分類されていない場合、`E` は nothing として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="a3946-1505">それ以外の場合、E は値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="a3946-1506">`E0` と `E1` は、`E`の意味を決定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="a3946-1507">`E` が*statement_expression*として発生した場合、`E` の意味は、ステートメントと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="a3946-1508">ただし、P は1回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="a3946-1509">それ以外の場合、`E0` が何も分類されていないと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="a3946-1510">それ以外の場合は `T0` `E0`の型にします。</span><span class="sxs-lookup"><span data-stu-id="a3946-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="a3946-1511">`T0` が参照型または null 非許容の値型として認識されない型パラメーターである場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="a3946-1512">`T0` が null 非許容の値型である場合、`E` の種類は `T0?`、`E` の意味は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="a3946-1513">ただし、`P` は1回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="a3946-1514">それ以外の場合、E の型は T0 になり、E の意味はと同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="a3946-1515">ただし、`P` は1回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="a3946-1516">`E1` がそれ自体が*null_conditional_expression*である場合は、これらの規則が再度適用され、それ以上 `?`がなくなるまで `null` のテストが入れ子になり、式はすべてプライマリ式の `E0`まで縮小されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="a3946-1517">たとえば、次のステートメントのように、式 `a.b?[0]?.c()` がステートメント式として出現する場合です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="a3946-1518">その意味は、次の場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="a3946-1519">これは、次の場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="a3946-1520">ただし、`a.b` と `a.b[0]` は1回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="a3946-1521">次のように、その値が使用されているコンテキストで発生する場合:</span><span class="sxs-lookup"><span data-stu-id="a3946-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="a3946-1522">最後の呼び出しの型が null 非許容の値型ではないと仮定した場合、その意味は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="a3946-1523">ただし、`a.b` と `a.b[0]` は1回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="a3946-1524">Null 条件式 (プロジェクション初期化子としての)</span><span class="sxs-lookup"><span data-stu-id="a3946-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="a3946-1525">Null 条件式は、(必要に応じて null 条件付き) メンバーアクセスで終了する場合にのみ、 *anonymous_object_creation_expression* ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) の*member_declarator*としてのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="a3946-1526">文法的には、この要件は次のように表現できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="a3946-1527">これは、上記の*null_conditional_expression*の文法の特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="a3946-1528">[匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)の*member_declarator*の実稼働には、 *null_conditional_member_access*のみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="a3946-1529">ステートメント式としての Null 条件式</span><span class="sxs-lookup"><span data-stu-id="a3946-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="a3946-1530">Null 条件式は、呼び出しで終了した場合にのみ*statement_expression* ([式ステートメント](statements.md#expression-statements)) として使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="a3946-1531">文法的には、この要件は次のように表現できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="a3946-1532">これは、上記の*null_conditional_expression*の文法の特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="a3946-1533">[式ステートメント](statements.md#expression-statements)内の*statement_expression*の実稼働には、 *null_conditional_invocation_expression*のみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="a3946-1534">単項プラス演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1534">Unary plus operator</span></span>

<span data-ttu-id="a3946-1535">`+x`フォームの操作では、単項演算子のオーバーロードの解決 ([単項演算子のオーバーロードの解決](expressions.md#unary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-1536">オペランドは選択された演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="a3946-1537">定義済みの単項プラス演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="a3946-1538">これらの各演算子について、結果は単純にオペランドの値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="a3946-1539">単項マイナス演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1539">Unary minus operator</span></span>

<span data-ttu-id="a3946-1540">`-x`フォームの操作では、単項演算子のオーバーロードの解決 ([単項演算子のオーバーロードの解決](expressions.md#unary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-1541">オペランドは選択された演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="a3946-1542">定義済みの否定演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="a3946-1543">整数の否定:</span><span class="sxs-lookup"><span data-stu-id="a3946-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="a3946-1544">結果は、0から `x` を減算することによって計算されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="a3946-1545">`x` の値がオペランド型の最小表現可能値 (`int` の場合は-2 ^ 31、`long`の場合は-2 ^ 63) である場合、`x` の算術否定はオペランド型では表現できません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="a3946-1546">これが `checked` コンテキスト内で発生すると、`System.OverflowException` がスローされます。`unchecked` のコンテキスト内で発生した場合、結果はオペランドの値になり、オーバーフローは報告されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="a3946-1547">否定演算子のオペランドが `uint`型である場合は、型 `long`に変換され、結果の型は `long`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="a3946-1548">例外として、`int` 値-2147483648 (-2 ^ 31) を10進整数リテラル ([整数リテラル](lexical-structure.md#integer-literals)) として書き込むことを許可するルールがあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="a3946-1549">否定演算子のオペランドが `ulong`型である場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="a3946-1550">例外として、`long` 値-9223372036854775808 (-2 ^ 63) を10進数の整数リテラル ([整数リテラル](lexical-structure.md#integer-literals)) として書き込むことを許可するルールがあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="a3946-1551">浮動小数点否定:</span><span class="sxs-lookup"><span data-stu-id="a3946-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="a3946-1552">結果は、符号が反転された `x` の値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="a3946-1553">`x` が NaN の場合、結果は NaN にもなります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="a3946-1554">10進数の否定:</span><span class="sxs-lookup"><span data-stu-id="a3946-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="a3946-1555">結果は、0から `x` を減算することによって計算されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="a3946-1556">10進数の否定は、`System.Decimal`型の単項マイナス演算子を使用することと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="a3946-1557">論理否定演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1557">Logical negation operator</span></span>

<span data-ttu-id="a3946-1558">`!x`フォームの操作では、単項演算子のオーバーロードの解決 ([単項演算子のオーバーロードの解決](expressions.md#unary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-1559">オペランドは選択された演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="a3946-1560">定義済みの論理否定演算子は1つだけ存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="a3946-1561">この演算子は、オペランドの論理否定を計算します。オペランドが `true`場合、結果は `false`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="a3946-1562">オペランドが `false`場合、結果は `true`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="a3946-1563">ビットごとの補数演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1563">Bitwise complement operator</span></span>

<span data-ttu-id="a3946-1564">`~x`フォームの操作では、単項演算子のオーバーロードの解決 ([単項演算子のオーバーロードの解決](expressions.md#unary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-1565">オペランドは選択された演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="a3946-1566">定義済みのビットごとの補数演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="a3946-1567">操作の結果は、これらの各演算子について、`x`のビットごとの補数となります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="a3946-1568">すべての列挙型 `E` は、暗黙的に次のビットごとの補数演算子を提供します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="a3946-1569">`~x`を評価した結果 `x` は、基になる型 `U`の列挙 `E` 型の式であり、`(E)(~(U)x)`への変換が `E` のコンテキスト ([checked および unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) の場合と同じように実行される点を除き、`unchecked` の評価とまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="a3946-1570">前置インクリメント演算子と前置デクリメント演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="a3946-1571">前置インクリメントまたはデクリメント演算のオペランドは、変数、プロパティアクセス、またはインデクサーアクセスとして分類される式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="a3946-1572">演算の結果は、オペランドと同じ型の値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="a3946-1573">前置インクリメントまたはデクリメント演算のオペランドがプロパティまたはインデクサーアクセスの場合、プロパティまたはインデクサーには、`get` と `set` の両方のアクセサーが必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="a3946-1574">そうでない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-1575">単項演算子のオーバーロードの解決 ([単項演算子のオーバーロードの解決](expressions.md#unary-operator-overload-resolution)) は、特定の演算子の実装を選択するために適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-1576">定義済みの `++` と `--` の演算子は、`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、列挙型の各型に存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="a3946-1577">定義済みの `++` 演算子は、オペランドに1を加算して生成された値を返します。定義済みの `--` 演算子は、オペランドから1を減算して生成された値を返します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="a3946-1578">`checked` コンテキストでは、この加算または減算の結果が結果型の範囲外で、結果型が整数型または列挙型の場合、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="a3946-1579">`++x` または `--x` フォームのプレフィックスインクリメントまたはデクリメント操作の実行時処理は、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="a3946-1580">`x` が変数として分類される場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="a3946-1581">変数を生成するために `x` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="a3946-1582">選択した演算子は、引数として `x` の値を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="a3946-1583">演算子によって返される値は、`x`の評価によって指定された場所に格納されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="a3946-1584">演算子によって返される値は、操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="a3946-1585">`x` がプロパティまたはインデクサーアクセスとして分類される場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="a3946-1586">インスタンス式 (`x` が `static`でない場合) と `x` に関連付けられている引数リスト (`x` がインデクサーアクセスの場合) が評価され、その結果が後続の `get` および `set` アクセサー呼び出しで使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="a3946-1587">`x` の `get` アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="a3946-1588">選択した演算子は、引数として `get` アクセサーによって返された値を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="a3946-1589">`x` の `set` アクセサーは、`value` 引数として演算子によって返された値を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="a3946-1590">演算子によって返される値は、操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="a3946-1591">`++` 演算子と `--` 演算子は、後置表記 ([後置インクリメントおよびデクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)) もサポートしています。</span><span class="sxs-lookup"><span data-stu-id="a3946-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="a3946-1592">通常、`x++` または `x--` の結果は、操作の前の `x` の値になります。一方、`++x` または `--x` の結果は、操作後の `x` の値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="a3946-1593">どちらの場合も、`x` 自体の値は、操作の後で同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="a3946-1594">`operator++` または `operator--` の実装は、後置表記またはプレフィックス表記のいずれかを使用して呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="a3946-1595">2つの表記に対して個別の演算子を実装することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="a3946-1596">キャスト式</span><span class="sxs-lookup"><span data-stu-id="a3946-1596">Cast expressions</span></span>

<span data-ttu-id="a3946-1597">*Cast_expression*は、式を指定した型に明示的に変換するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="a3946-1598">フォーム `(T)E`の*cast_expression* 。 `T` は*型*で `E` は*unary_expression*であり、`E` の値の明示的な変換 ([明示的な変換](conversions.md#explicit-conversions)) を実行して `T`を入力します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="a3946-1599">`E` から `T`への明示的な変換が存在しない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="a3946-1600">それ以外の場合、結果は明示的な変換によって生成される値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="a3946-1601">`E` が変数を表している場合でも、結果は常に値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="a3946-1602">*Cast_expression*の文法によって、特定の構文のあいまいさが生じます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="a3946-1603">たとえば、式 `(x)-y` は、 *cast_expression* (型 `x`に `-y` をキャスト) として解釈するか、additive_expression*と組み合わせて parenthesized_expression (* 値 *`x - y)`を計算*することができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="a3946-1604">*Cast_expression*のあいまいさを解決するために、次の規則が存在します。かっこで囲まれた1つ以上の*トークン*s ([空白](lexical-structure.md#white-space)) のシーケンスは、次のいずれかが true の場合にのみ、 *cast_expression*の開始と見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="a3946-1605">トークンのシーケンスは、*型*の正しい文法ですが、*式*には対応していません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="a3946-1606">トークンのシーケンスは、*型*の正しい文法です。閉じかっこの直後に続くトークンは、トークン "`~`"、トークン "`!`"、トークン "`(`"、*識別子*([Unicode 文字のエスケープシーケンス](lexical-structure.md#unicode-character-escape-sequences))、*リテラル*([リテラル](lexical-structure.md#literals))、または `as` および `is`を除く任意の*キーワード*([キーワード](lexical-structure.md#keywords)) です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="a3946-1607">上記の "正しい文法" という用語は、トークンのシーケンスが、特定の文章の作成に準拠している必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="a3946-1608">具体的には、構成要素の実際の意味は考慮されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="a3946-1609">たとえば、`x` と `y` が識別子の場合、`x.y` が実際に型を表していない場合でも、`x.y` は型の正しい文法です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="a3946-1610">これは、`x` と `y` が識別子である場合、`(x)y`、`(x)(y)`、および `(x)(-y)` が*cast_expression*であるにもかかわらず、`(x)-y` が型を識別する場合でも、この規則に従っています。`x`</span><span class="sxs-lookup"><span data-stu-id="a3946-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="a3946-1611">ただし、`x` が定義済みの型 (`int`など) を識別するキーワードである場合、4つのすべての形式が*cast_expression*ます (このようなキーワードは、式自体ではない可能性があります)。</span><span class="sxs-lookup"><span data-stu-id="a3946-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="a3946-1612">Await 式</span><span class="sxs-lookup"><span data-stu-id="a3946-1612">Await expressions</span></span>

<span data-ttu-id="a3946-1613">Await 演算子は、オペランドによって表される非同期操作が完了するまで、外側の非同期関数の評価を中断するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="a3946-1614">*Await_expression*は、非同期関数 ([反復子](classes.md#iterators)) の本体でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="a3946-1615">最も近い外側の非同期関数内では、次の場所では*await_expression*が発生しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="a3946-1616">入れ子になった (非同期ではない) 匿名関数内</span><span class="sxs-lookup"><span data-stu-id="a3946-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="a3946-1617">*Lock_statement*のブロック内</span><span class="sxs-lookup"><span data-stu-id="a3946-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="a3946-1618">Unsafe コンテキスト内</span><span class="sxs-lookup"><span data-stu-id="a3946-1618">In an unsafe context</span></span>

<span data-ttu-id="a3946-1619">*Await_expression*は、非同期のラムダ式を使用するように構文的に変換されるため、 *query_expression*内のほとんどの場所では発生しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="a3946-1620">非同期関数の内部では、`await` を識別子として使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="a3946-1621">したがって、await 式と、識別子を含むさまざまな式の間に構文のあいまいさはありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="a3946-1622">非同期関数の外部では、`await` は通常の識別子として機能します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="a3946-1623">*Await_expression*のオペランドは***タスク***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="a3946-1624">これは、 *await_expression*の評価時に完了しない可能性のある非同期操作を表します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="a3946-1625">Await 演算子の目的は、待機中のタスクが完了するまで、外側の非同期関数の実行を中断し、その結果を取得することです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="a3946-1626">待機可能式</span><span class="sxs-lookup"><span data-stu-id="a3946-1626">Awaitable expressions</span></span>

<span data-ttu-id="a3946-1627">Await 式のタスクは、***待機可能***である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="a3946-1628">次のいずれかの場合、式 `t` は待機可能です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="a3946-1629">`t` コンパイル時の型 `dynamic`</span><span class="sxs-lookup"><span data-stu-id="a3946-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="a3946-1630">`t` には、`GetAwaiter` と呼ばれるアクセス可能なインスタンスまたは拡張メソッドがありますが、パラメーターがなく、型パラメーターもありません。戻り値の型 `A` は、次のすべてのを保持します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="a3946-1631">`A` はインターフェイス `System.Runtime.CompilerServices.INotifyCompletion` を実装します (これは、簡潔に `INotifyCompletion` として知られています)。</span><span class="sxs-lookup"><span data-stu-id="a3946-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="a3946-1632">`A` には、型の `IsCompleted` アクセス可能で読み取り可能なインスタンスプロパティがあり `bool`</span><span class="sxs-lookup"><span data-stu-id="a3946-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="a3946-1633">`A` には、パラメーターを持たず、型パラメーターを持たない、アクセス可能なインスタンスメソッド `GetResult` があります</span><span class="sxs-lookup"><span data-stu-id="a3946-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="a3946-1634">`GetAwaiter` メソッドの目的は、タスクの***awaiter***を取得することです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="a3946-1635">`A` 型は、await 式の***awaiter 型***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="a3946-1636">`IsCompleted` プロパティの目的は、タスクが既に完了しているかどうかを判断することです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="a3946-1637">その場合は、評価を中断する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="a3946-1638">`INotifyCompletion.OnCompleted` メソッドの目的は、タスクに "継続" をサインアップすることです。つまり、タスクが完了した後に呼び出されるデリゲート (型 `System.Action`)。</span><span class="sxs-lookup"><span data-stu-id="a3946-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="a3946-1639">`GetResult` メソッドの目的は、完了後にタスクの結果を取得することです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="a3946-1640">この結果は、正常に完了している可能性があります (結果値がある場合もあります)。または、`GetResult` メソッドによってスローされる例外の場合もあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="a3946-1641">Await 式の分類</span><span class="sxs-lookup"><span data-stu-id="a3946-1641">Classification of await expressions</span></span>

<span data-ttu-id="a3946-1642">式 `await t` は、式 `(t).GetAwaiter().GetResult()`と同じように分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="a3946-1643">したがって、`GetResult` の戻り値の型が `void`場合、 *await_expression*は nothing として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="a3946-1644">`T`void 以外の戻り値の型がある場合、 *await_expression*は `T`型の値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="a3946-1645">Await 式のランタイム評価</span><span class="sxs-lookup"><span data-stu-id="a3946-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="a3946-1646">実行時に `await t` 式は次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="a3946-1647">式 `(t).GetAwaiter()`を評価することによって、awaiter `a` を取得します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="a3946-1648">`bool` `b` は、式 `(a).IsCompleted`を評価することによって取得されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="a3946-1649">`b` が `false` 場合、評価は `a` がインターフェイス `System.Runtime.CompilerServices.ICriticalNotifyCompletion` を実装しているかどうかによって異なります (簡潔にするために `ICriticalNotifyCompletion` として知られています)。</span><span class="sxs-lookup"><span data-stu-id="a3946-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="a3946-1650">このチェックは、バインド時に行われます。つまり、`a` がコンパイル時の型 `dynamic`であり、コンパイル時にそれ以外の場合は、実行時に実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="a3946-1651">再開デリゲート ([反復子](classes.md#iterators)) を示す `r` を使用します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="a3946-1652">`a` が `ICriticalNotifyCompletion`を実装していない場合は、式 `(a as (INotifyCompletion)).OnCompleted(r)` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="a3946-1653">`a` が `ICriticalNotifyCompletion`を実装している場合、式 `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="a3946-1654">その後、評価は中断され、非同期関数の現在の呼び出し元に制御が返されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="a3946-1655">の直後 (`b` が `true`の場合)、または後で再開デリゲートを呼び出したとき (`b` が `false`の場合)、式 `(a).GetResult()` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="a3946-1656">値が返された場合、その値は*await_expression*の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="a3946-1657">それ以外の場合、結果は nothing です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="a3946-1658">インターフェイスメソッド `INotifyCompletion.OnCompleted` および `ICriticalNotifyCompletion.UnsafeOnCompleted` の awaiter の実装では、デリゲート `r` を最大で1回呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="a3946-1659">それ以外の場合、外側の非同期関数の動作は未定義です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="a3946-1660">算術演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1660">Arithmetic operators</span></span>

<span data-ttu-id="a3946-1661">`*`、`/`、`%`、`+`、および `-` の各演算子は、算術演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

<span data-ttu-id="a3946-1662">算術演算子のオペランドに `dynamic`コンパイル時の型がある場合、式は動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a3946-1663">この場合、式のコンパイル時の型は `dynamic`であり、以下に示す解決方法は、コンパイル時の型 `dynamic`を持つオペランドの実行時の型を使用して実行時に実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="a3946-1664">乗算演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1664">Multiplication operator</span></span>

<span data-ttu-id="a3946-1665">`x * y`フォームの操作では、二項演算子のオーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-1666">オペランドは選択した演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a3946-1667">定義済みの乗算演算子を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="a3946-1668">すべての演算子は、`x` と `y`の積を計算します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="a3946-1669">整数乗算:</span><span class="sxs-lookup"><span data-stu-id="a3946-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="a3946-1670">`checked` コンテキストでは、製品が結果の型の範囲外の場合、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a3946-1671">`unchecked` のコンテキストでは、オーバーフローは報告されず、結果の型の範囲外の重要な上位ビットはすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="a3946-1672">浮動小数点乗算:</span><span class="sxs-lookup"><span data-stu-id="a3946-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="a3946-1673">この製品は、IEEE 754 算術のルールに従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="a3946-1674">次の表に、0以外の有限値 (0、無限大、および NaN) のすべての可能な組み合わせの結果を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="a3946-1675">テーブルでは、`x` と `y` が正の有限値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="a3946-1676">`z` は `x * y`の結果です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="a3946-1677">結果が変換先の型に対して大きすぎる場合、`z` は無限大です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="a3946-1678">結果が変換先の型に対して小さすぎる場合、`z` は0になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="a3946-1679">\+ y</span><span class="sxs-lookup"><span data-stu-id="a3946-1679">+y</span></span>   | <span data-ttu-id="a3946-1680">-y</span><span class="sxs-lookup"><span data-stu-id="a3946-1680">-y</span></span>   | <span data-ttu-id="a3946-1681">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1681">+0</span></span>  | <span data-ttu-id="a3946-1682">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1682">-0</span></span>  | <span data-ttu-id="a3946-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1683">+inf</span></span> | <span data-ttu-id="a3946-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1684">-inf</span></span> | <span data-ttu-id="a3946-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1685">NaN</span></span> | 
   | <span data-ttu-id="a3946-1686">+x</span><span class="sxs-lookup"><span data-stu-id="a3946-1686">+x</span></span>   | <span data-ttu-id="a3946-1687">\+ z</span><span class="sxs-lookup"><span data-stu-id="a3946-1687">+z</span></span>   | <span data-ttu-id="a3946-1688">-z</span><span class="sxs-lookup"><span data-stu-id="a3946-1688">-z</span></span>   | <span data-ttu-id="a3946-1689">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1689">+0</span></span>  | <span data-ttu-id="a3946-1690">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1690">-0</span></span>  | <span data-ttu-id="a3946-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1691">+inf</span></span> | <span data-ttu-id="a3946-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1692">-inf</span></span> | <span data-ttu-id="a3946-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1693">NaN</span></span> | 
   | <span data-ttu-id="a3946-1694">-x</span><span class="sxs-lookup"><span data-stu-id="a3946-1694">-x</span></span>   | <span data-ttu-id="a3946-1695">-z</span><span class="sxs-lookup"><span data-stu-id="a3946-1695">-z</span></span>   | <span data-ttu-id="a3946-1696">\+ z</span><span class="sxs-lookup"><span data-stu-id="a3946-1696">+z</span></span>   | <span data-ttu-id="a3946-1697">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1697">-0</span></span>  | <span data-ttu-id="a3946-1698">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1698">+0</span></span>  | <span data-ttu-id="a3946-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1699">-inf</span></span> | <span data-ttu-id="a3946-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1700">+inf</span></span> | <span data-ttu-id="a3946-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1701">NaN</span></span> | 
   | <span data-ttu-id="a3946-1702">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1702">+0</span></span>   | <span data-ttu-id="a3946-1703">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1703">+0</span></span>   | <span data-ttu-id="a3946-1704">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1704">-0</span></span>   | <span data-ttu-id="a3946-1705">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1705">+0</span></span>  | <span data-ttu-id="a3946-1706">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1706">-0</span></span>  | <span data-ttu-id="a3946-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1707">NaN</span></span>  | <span data-ttu-id="a3946-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1708">NaN</span></span>  | <span data-ttu-id="a3946-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1709">NaN</span></span> | 
   | <span data-ttu-id="a3946-1710">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1710">-0</span></span>   | <span data-ttu-id="a3946-1711">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1711">-0</span></span>   | <span data-ttu-id="a3946-1712">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1712">+0</span></span>   | <span data-ttu-id="a3946-1713">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1713">-0</span></span>  | <span data-ttu-id="a3946-1714">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1714">+0</span></span>  | <span data-ttu-id="a3946-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1715">NaN</span></span>  | <span data-ttu-id="a3946-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1716">NaN</span></span>  | <span data-ttu-id="a3946-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1717">NaN</span></span> | 
   | <span data-ttu-id="a3946-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1718">+inf</span></span> | <span data-ttu-id="a3946-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1719">+inf</span></span> | <span data-ttu-id="a3946-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1720">-inf</span></span> | <span data-ttu-id="a3946-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1721">NaN</span></span> | <span data-ttu-id="a3946-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1722">NaN</span></span> | <span data-ttu-id="a3946-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1723">+inf</span></span> | <span data-ttu-id="a3946-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1724">-inf</span></span> | <span data-ttu-id="a3946-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1725">NaN</span></span> | 
   | <span data-ttu-id="a3946-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1726">-inf</span></span> | <span data-ttu-id="a3946-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1727">-inf</span></span> | <span data-ttu-id="a3946-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1728">+inf</span></span> | <span data-ttu-id="a3946-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1729">NaN</span></span> | <span data-ttu-id="a3946-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1730">NaN</span></span> | <span data-ttu-id="a3946-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1731">-inf</span></span> | <span data-ttu-id="a3946-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1732">+inf</span></span> | <span data-ttu-id="a3946-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1733">NaN</span></span> | 
   | <span data-ttu-id="a3946-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1734">NaN</span></span>  | <span data-ttu-id="a3946-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1735">NaN</span></span>  | <span data-ttu-id="a3946-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1736">NaN</span></span>  | <span data-ttu-id="a3946-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1737">NaN</span></span> | <span data-ttu-id="a3946-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1738">NaN</span></span> | <span data-ttu-id="a3946-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1739">NaN</span></span>  | <span data-ttu-id="a3946-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1740">NaN</span></span>  | <span data-ttu-id="a3946-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1741">NaN</span></span> | 

*  <span data-ttu-id="a3946-1742">10進数の乗算:</span><span class="sxs-lookup"><span data-stu-id="a3946-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="a3946-1743">結果の値が大きすぎて `decimal` 形式で表現できない場合は、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a3946-1744">結果値が小さすぎて `decimal` 形式で表現できない場合、結果は0になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="a3946-1745">結果の小数点以下桁数は、2つのオペランドのスケールの合計になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="a3946-1746">Decimal 型の乗算は、`System.Decimal`型の乗算演算子を使用することと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="a3946-1747">除算演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1747">Division operator</span></span>

<span data-ttu-id="a3946-1748">`x / y`フォームの操作では、二項演算子のオーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-1749">オペランドは選択した演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a3946-1750">定義済みの除算演算子を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="a3946-1751">すべての演算子は、`x` と `y`の商を計算します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="a3946-1752">整数除算:</span><span class="sxs-lookup"><span data-stu-id="a3946-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="a3946-1753">右オペランドの値が0の場合は、`System.DivideByZeroException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="a3946-1754">除算は、結果を0方向に丸めます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="a3946-1755">したがって、結果の絶対値は、2つのオペランドの商の絶対値以下の最大の整数になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="a3946-1756">2つのオペランドの符号が逆の場合、結果は0または正になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="a3946-1757">左側のオペランドが表現可能な最小 `int` または `long` 値で、右オペランドが `-1`の場合は、オーバーフローが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="a3946-1758">`checked` コンテキストでは、これにより `System.ArithmeticException` (またはサブクラス) がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="a3946-1759">`unchecked` のコンテキストでは、`System.ArithmeticException` (またはサブクラス) がスローされるか、または、結果の値が左オペランドの値であることを示すオーバーフローが報告されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="a3946-1760">浮動小数点除算:</span><span class="sxs-lookup"><span data-stu-id="a3946-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="a3946-1761">商は、IEEE 754 算術のルールに従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="a3946-1762">次の表に、0以外の有限値 (0、無限大、および NaN) のすべての可能な組み合わせの結果を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="a3946-1763">テーブルでは、`x` と `y` が正の有限値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="a3946-1764">`z` は `x / y`の結果です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="a3946-1765">結果が変換先の型に対して大きすぎる場合、`z` は無限大です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="a3946-1766">結果が変換先の型に対して小さすぎる場合、`z` は0になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="a3946-1767">\+ y</span><span class="sxs-lookup"><span data-stu-id="a3946-1767">+y</span></span>   | <span data-ttu-id="a3946-1768">-y</span><span class="sxs-lookup"><span data-stu-id="a3946-1768">-y</span></span>   | <span data-ttu-id="a3946-1769">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1769">+0</span></span>   | <span data-ttu-id="a3946-1770">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1770">-0</span></span>   | <span data-ttu-id="a3946-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1771">+inf</span></span> | <span data-ttu-id="a3946-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1772">-inf</span></span> | <span data-ttu-id="a3946-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1773">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1774">+x</span><span class="sxs-lookup"><span data-stu-id="a3946-1774">+x</span></span>   | <span data-ttu-id="a3946-1775">\+ z</span><span class="sxs-lookup"><span data-stu-id="a3946-1775">+z</span></span>   | <span data-ttu-id="a3946-1776">-z</span><span class="sxs-lookup"><span data-stu-id="a3946-1776">-z</span></span>   | <span data-ttu-id="a3946-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1777">+inf</span></span> | <span data-ttu-id="a3946-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1778">-inf</span></span> | <span data-ttu-id="a3946-1779">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1779">+0</span></span>   | <span data-ttu-id="a3946-1780">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1780">-0</span></span>   | <span data-ttu-id="a3946-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1781">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1782">-x</span><span class="sxs-lookup"><span data-stu-id="a3946-1782">-x</span></span>   | <span data-ttu-id="a3946-1783">-z</span><span class="sxs-lookup"><span data-stu-id="a3946-1783">-z</span></span>   | <span data-ttu-id="a3946-1784">\+ z</span><span class="sxs-lookup"><span data-stu-id="a3946-1784">+z</span></span>   | <span data-ttu-id="a3946-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1785">-inf</span></span> | <span data-ttu-id="a3946-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1786">+inf</span></span> | <span data-ttu-id="a3946-1787">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1787">-0</span></span>   | <span data-ttu-id="a3946-1788">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1788">+0</span></span>   | <span data-ttu-id="a3946-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1789">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1790">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1790">+0</span></span>   | <span data-ttu-id="a3946-1791">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1791">+0</span></span>   | <span data-ttu-id="a3946-1792">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1792">-0</span></span>   | <span data-ttu-id="a3946-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1793">NaN</span></span>  | <span data-ttu-id="a3946-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1794">NaN</span></span>  | <span data-ttu-id="a3946-1795">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1795">+0</span></span>   | <span data-ttu-id="a3946-1796">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1796">-0</span></span>   | <span data-ttu-id="a3946-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1797">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1798">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1798">-0</span></span>   | <span data-ttu-id="a3946-1799">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1799">-0</span></span>   | <span data-ttu-id="a3946-1800">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1800">+0</span></span>   | <span data-ttu-id="a3946-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1801">NaN</span></span>  | <span data-ttu-id="a3946-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1802">NaN</span></span>  | <span data-ttu-id="a3946-1803">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1803">-0</span></span>   | <span data-ttu-id="a3946-1804">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1804">+0</span></span>   | <span data-ttu-id="a3946-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1805">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1806">+inf</span></span> | <span data-ttu-id="a3946-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1807">+inf</span></span> | <span data-ttu-id="a3946-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1808">-inf</span></span> | <span data-ttu-id="a3946-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1809">+inf</span></span> | <span data-ttu-id="a3946-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1810">-inf</span></span> | <span data-ttu-id="a3946-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1811">NaN</span></span>  | <span data-ttu-id="a3946-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1812">NaN</span></span>  | <span data-ttu-id="a3946-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1813">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1814">-inf</span></span> | <span data-ttu-id="a3946-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1815">-inf</span></span> | <span data-ttu-id="a3946-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1816">+inf</span></span> | <span data-ttu-id="a3946-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1817">-inf</span></span> | <span data-ttu-id="a3946-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1818">+inf</span></span> | <span data-ttu-id="a3946-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1819">NaN</span></span>  | <span data-ttu-id="a3946-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1820">NaN</span></span>  | <span data-ttu-id="a3946-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1821">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1822">NaN</span></span>  | <span data-ttu-id="a3946-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1823">NaN</span></span>  | <span data-ttu-id="a3946-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1824">NaN</span></span>  | <span data-ttu-id="a3946-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1825">NaN</span></span>  | <span data-ttu-id="a3946-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1826">NaN</span></span>  | <span data-ttu-id="a3946-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1827">NaN</span></span>  | <span data-ttu-id="a3946-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1828">NaN</span></span>  | <span data-ttu-id="a3946-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1829">NaN</span></span>  | 

*  <span data-ttu-id="a3946-1830">小数点以下桁数:</span><span class="sxs-lookup"><span data-stu-id="a3946-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="a3946-1831">右オペランドの値が0の場合は、`System.DivideByZeroException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="a3946-1832">結果の値が大きすぎて `decimal` 形式で表現できない場合は、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a3946-1833">結果値が小さすぎて `decimal` 形式で表現できない場合、結果は0になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="a3946-1834">結果の小数点以下桁数は、実際の数値の結果に最も近い表現可能な10進値と等しい結果を保持する最小のスケールになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="a3946-1835">小数点除算は、`System.Decimal`型の除算演算子を使用することと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="a3946-1836">剰余演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1836">Remainder operator</span></span>

<span data-ttu-id="a3946-1837">`x % y`フォームの操作では、二項演算子のオーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-1838">オペランドは選択した演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a3946-1839">定義済みの剰余演算子を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="a3946-1840">すべての演算子は、`x` と `y`間の除算の剰余を計算します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="a3946-1841">整数の剰余:</span><span class="sxs-lookup"><span data-stu-id="a3946-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="a3946-1842">`x % y` の結果は `x - (x / y) * y`によって生成される値です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="a3946-1843">`y` がゼロの場合、`System.DivideByZeroException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="a3946-1844">左オペランドが最小 `int` 値または `long` 値で、右オペランドが `-1`の場合、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a3946-1845">`x / y` が例外をスローしない場合、`x % y` は例外をスローしません。</span><span class="sxs-lookup"><span data-stu-id="a3946-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="a3946-1846">浮動小数点の剰余:</span><span class="sxs-lookup"><span data-stu-id="a3946-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="a3946-1847">次の表に、0以外の有限値 (0、無限大、および NaN) のすべての可能な組み合わせの結果を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="a3946-1848">テーブルでは、`x` と `y` が正の有限値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="a3946-1849">`z` は `x % y` の結果であり、`x - n * y`として計算されます。 `n` は、`x / y`以下の最大の整数です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="a3946-1850">余りを計算するこの方法は、整数オペランドに使用されるものと似ていますが、IEEE 754 定義とは異なります (`n` が `x / y`に最も近い整数)。</span><span class="sxs-lookup"><span data-stu-id="a3946-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="a3946-1851">\+ y</span><span class="sxs-lookup"><span data-stu-id="a3946-1851">+y</span></span>   | <span data-ttu-id="a3946-1852">-y</span><span class="sxs-lookup"><span data-stu-id="a3946-1852">-y</span></span>   | <span data-ttu-id="a3946-1853">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1853">+0</span></span>   | <span data-ttu-id="a3946-1854">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1854">-0</span></span>   | <span data-ttu-id="a3946-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1855">+inf</span></span> | <span data-ttu-id="a3946-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1856">-inf</span></span> | <span data-ttu-id="a3946-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1857">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1858">+x</span><span class="sxs-lookup"><span data-stu-id="a3946-1858">+x</span></span>   | <span data-ttu-id="a3946-1859">\+ z</span><span class="sxs-lookup"><span data-stu-id="a3946-1859">+z</span></span>   | <span data-ttu-id="a3946-1860">\+ z</span><span class="sxs-lookup"><span data-stu-id="a3946-1860">+z</span></span>   | <span data-ttu-id="a3946-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1861">NaN</span></span>  | <span data-ttu-id="a3946-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1862">NaN</span></span>  | <span data-ttu-id="a3946-1863">x</span><span class="sxs-lookup"><span data-stu-id="a3946-1863">x</span></span>    | <span data-ttu-id="a3946-1864">x</span><span class="sxs-lookup"><span data-stu-id="a3946-1864">x</span></span>    | <span data-ttu-id="a3946-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1865">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1866">-x</span><span class="sxs-lookup"><span data-stu-id="a3946-1866">-x</span></span>   | <span data-ttu-id="a3946-1867">-z</span><span class="sxs-lookup"><span data-stu-id="a3946-1867">-z</span></span>   | <span data-ttu-id="a3946-1868">-z</span><span class="sxs-lookup"><span data-stu-id="a3946-1868">-z</span></span>   | <span data-ttu-id="a3946-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1869">NaN</span></span>  | <span data-ttu-id="a3946-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1870">NaN</span></span>  | <span data-ttu-id="a3946-1871">-x</span><span class="sxs-lookup"><span data-stu-id="a3946-1871">-x</span></span>   | <span data-ttu-id="a3946-1872">-x</span><span class="sxs-lookup"><span data-stu-id="a3946-1872">-x</span></span>   | <span data-ttu-id="a3946-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1873">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1874">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1874">+0</span></span>   | <span data-ttu-id="a3946-1875">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1875">+0</span></span>   | <span data-ttu-id="a3946-1876">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1876">+0</span></span>   | <span data-ttu-id="a3946-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1877">NaN</span></span>  | <span data-ttu-id="a3946-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1878">NaN</span></span>  | <span data-ttu-id="a3946-1879">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1879">+0</span></span>   | <span data-ttu-id="a3946-1880">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1880">+0</span></span>   | <span data-ttu-id="a3946-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1881">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1882">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1882">-0</span></span>   | <span data-ttu-id="a3946-1883">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1883">-0</span></span>   | <span data-ttu-id="a3946-1884">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1884">-0</span></span>   | <span data-ttu-id="a3946-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1885">NaN</span></span>  | <span data-ttu-id="a3946-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1886">NaN</span></span>  | <span data-ttu-id="a3946-1887">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1887">-0</span></span>   | <span data-ttu-id="a3946-1888">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1888">-0</span></span>   | <span data-ttu-id="a3946-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1889">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1890">+inf</span></span> | <span data-ttu-id="a3946-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1891">NaN</span></span>  | <span data-ttu-id="a3946-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1892">NaN</span></span>  | <span data-ttu-id="a3946-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1893">NaN</span></span>  | <span data-ttu-id="a3946-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1894">NaN</span></span>  | <span data-ttu-id="a3946-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1895">NaN</span></span>  | <span data-ttu-id="a3946-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1896">NaN</span></span>  | <span data-ttu-id="a3946-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1897">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1898">-inf</span></span> | <span data-ttu-id="a3946-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1899">NaN</span></span>  | <span data-ttu-id="a3946-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1900">NaN</span></span>  | <span data-ttu-id="a3946-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1901">NaN</span></span>  | <span data-ttu-id="a3946-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1902">NaN</span></span>  | <span data-ttu-id="a3946-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1903">NaN</span></span>  | <span data-ttu-id="a3946-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1904">NaN</span></span>  | <span data-ttu-id="a3946-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1905">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1906">NaN</span></span>  | <span data-ttu-id="a3946-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1907">NaN</span></span>  | <span data-ttu-id="a3946-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1908">NaN</span></span>  | <span data-ttu-id="a3946-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1909">NaN</span></span>  | <span data-ttu-id="a3946-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1910">NaN</span></span>  | <span data-ttu-id="a3946-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1911">NaN</span></span>  | <span data-ttu-id="a3946-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1912">NaN</span></span>  | <span data-ttu-id="a3946-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1913">NaN</span></span>  | 

*  <span data-ttu-id="a3946-1914">10進数の剰余:</span><span class="sxs-lookup"><span data-stu-id="a3946-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="a3946-1915">右オペランドの値が0の場合は、`System.DivideByZeroException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="a3946-1916">丸めの前の結果の小数点以下桁数は、2つのオペランドのスケールのうち、大きい方になります。0以外の場合は、結果の符号が `x`の値と同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="a3946-1917">Decimal 剰余は、`System.Decimal`型の剰余演算子を使用することと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="a3946-1918">加算演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-1918">Addition operator</span></span>

<span data-ttu-id="a3946-1919">`x + y`フォームの操作では、二項演算子のオーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-1920">オペランドは選択した演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a3946-1921">定義済みの加算演算子を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="a3946-1922">数値型と列挙型の場合、定義済みの加算演算子は、2つのオペランドの合計を計算します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="a3946-1923">一方または両方のオペランドが string 型の場合、定義済みの加算演算子は、オペランドの文字列形式を連結します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="a3946-1924">整数加算:</span><span class="sxs-lookup"><span data-stu-id="a3946-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="a3946-1925">`checked` のコンテキストでは、合計が結果の型の範囲外の場合、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a3946-1926">`unchecked` のコンテキストでは、オーバーフローは報告されず、結果の型の範囲外の重要な上位ビットはすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="a3946-1927">浮動小数点加算:</span><span class="sxs-lookup"><span data-stu-id="a3946-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="a3946-1928">合計は、IEEE 754 算術のルールに従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="a3946-1929">次の表に、0以外の有限値 (0、無限大、および NaN) のすべての可能な組み合わせの結果を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="a3946-1930">この表では、`x` と `y` は0以外の有限値であり、`z` は `x + y`の結果です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="a3946-1931">`x` と `y` の大きさは同じですが、符号が逆の場合、`z` は正のゼロになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="a3946-1932">`x + y` が変換先の型で表すには大きすぎる場合、`z` は `x + y`と同じ符号を持つ無限大です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="a3946-1933">y</span><span class="sxs-lookup"><span data-stu-id="a3946-1933">y</span></span>    | <span data-ttu-id="a3946-1934">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1934">+0</span></span>   | <span data-ttu-id="a3946-1935">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1935">-0</span></span>   | <span data-ttu-id="a3946-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1936">+inf</span></span> | <span data-ttu-id="a3946-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1937">-inf</span></span> | <span data-ttu-id="a3946-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1938">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1939">x</span><span class="sxs-lookup"><span data-stu-id="a3946-1939">x</span></span>    | <span data-ttu-id="a3946-1940">z</span><span class="sxs-lookup"><span data-stu-id="a3946-1940">z</span></span>    | <span data-ttu-id="a3946-1941">x</span><span class="sxs-lookup"><span data-stu-id="a3946-1941">x</span></span>    | <span data-ttu-id="a3946-1942">x</span><span class="sxs-lookup"><span data-stu-id="a3946-1942">x</span></span>    | <span data-ttu-id="a3946-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1943">+inf</span></span> | <span data-ttu-id="a3946-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1944">-inf</span></span> | <span data-ttu-id="a3946-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1945">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1946">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1946">+0</span></span>   | <span data-ttu-id="a3946-1947">y</span><span class="sxs-lookup"><span data-stu-id="a3946-1947">y</span></span>    | <span data-ttu-id="a3946-1948">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1948">+0</span></span>   | <span data-ttu-id="a3946-1949">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1949">+0</span></span>   | <span data-ttu-id="a3946-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1950">+inf</span></span> | <span data-ttu-id="a3946-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1951">-inf</span></span> | <span data-ttu-id="a3946-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1952">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1953">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1953">-0</span></span>   | <span data-ttu-id="a3946-1954">y</span><span class="sxs-lookup"><span data-stu-id="a3946-1954">y</span></span>    | <span data-ttu-id="a3946-1955">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-1955">+0</span></span>   | <span data-ttu-id="a3946-1956">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-1956">-0</span></span>   | <span data-ttu-id="a3946-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1957">+inf</span></span> | <span data-ttu-id="a3946-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1958">-inf</span></span> | <span data-ttu-id="a3946-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1959">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1960">+inf</span></span> | <span data-ttu-id="a3946-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1961">+inf</span></span> | <span data-ttu-id="a3946-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1962">+inf</span></span> | <span data-ttu-id="a3946-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1963">+inf</span></span> | <span data-ttu-id="a3946-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1964">+inf</span></span> | <span data-ttu-id="a3946-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1965">NaN</span></span>  | <span data-ttu-id="a3946-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1966">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1967">-inf</span></span> | <span data-ttu-id="a3946-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1968">-inf</span></span> | <span data-ttu-id="a3946-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1969">-inf</span></span> | <span data-ttu-id="a3946-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1970">-inf</span></span> | <span data-ttu-id="a3946-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1971">NaN</span></span>  | <span data-ttu-id="a3946-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-1972">-inf</span></span> | <span data-ttu-id="a3946-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1973">NaN</span></span>  | 
   | <span data-ttu-id="a3946-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1974">NaN</span></span>  | <span data-ttu-id="a3946-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1975">NaN</span></span>  | <span data-ttu-id="a3946-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1976">NaN</span></span>  | <span data-ttu-id="a3946-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1977">NaN</span></span>  | <span data-ttu-id="a3946-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1978">NaN</span></span>  | <span data-ttu-id="a3946-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1979">NaN</span></span>  | <span data-ttu-id="a3946-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-1980">NaN</span></span>  | 

*  <span data-ttu-id="a3946-1981">小数点の追加:</span><span class="sxs-lookup"><span data-stu-id="a3946-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="a3946-1982">結果の値が大きすぎて `decimal` 形式で表現できない場合は、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a3946-1983">結果の小数点以下の桁数は、2つのオペランドのスケールのうち、大きい方になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="a3946-1984">Decimal 加算は、`System.Decimal`型の加算演算子を使用することと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="a3946-1985">列挙型の追加。</span><span class="sxs-lookup"><span data-stu-id="a3946-1985">Enumeration addition.</span></span> <span data-ttu-id="a3946-1986">すべての列挙型には、暗黙的に次の定義済みの演算子が用意されています。 `E` は列挙型であり、`U` は `E`の基になる型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="a3946-1987">実行時に、これらの演算子は `(E)((U)x + (U)y)`として正確に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="a3946-1988">文字列の連結:</span><span class="sxs-lookup"><span data-stu-id="a3946-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="a3946-1989">これらのバイナリ `+` 演算子のオーバーロードは、文字列の連結を実行します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="a3946-1990">文字列連結のオペランドが `null`場合、空の文字列が置換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="a3946-1991">それ以外の場合、文字列以外の引数は、型 `object`から継承された仮想 `ToString` メソッドを呼び出すことによって文字列形式に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="a3946-1992">`ToString` が `null`を返す場合、空の文字列が置換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   文字列連結演算子の結果は、左オペランドの文字と右オペランドの文字で構成される文字列です。 文字列連結演算子は、`null` 値を返しません。 <span data-ttu-id="a3946-1995">結果の文字列を割り当てるために十分なメモリがない場合、`System.OutOfMemoryException` がスローされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="a3946-1996">デリゲートの組み合わせ。</span><span class="sxs-lookup"><span data-stu-id="a3946-1996">Delegate combination.</span></span> <span data-ttu-id="a3946-1997">すべてのデリゲート型は、次の定義済み演算子を暗黙的に提供します。 `D` はデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="a3946-1998">二項 `+` 演算子は、両方のオペランドがデリゲート型 `D`である場合に、デリゲートの組み合わせを実行します。</span><span class="sxs-lookup"><span data-stu-id="a3946-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="a3946-1999">(オペランドのデリゲート型が異なる場合、バインド時エラーが発生します)。最初のオペランドが `null`場合、演算の結果は2番目のオペランドの値になります (その場合も `null`)。</span><span class="sxs-lookup"><span data-stu-id="a3946-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="a3946-2000">それ以外の場合、2番目のオペランドが `null`場合、演算の結果は最初のオペランドの値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="a3946-2001">それ以外の場合、操作の結果は新しいデリゲートインスタンスになり、呼び出されると、最初のオペランドが呼び出され、2番目のオペランドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="a3946-2002">デリゲートの組み合わせの例については、「[減算演算子](expressions.md#subtraction-operator)と[デリゲート呼び出し](delegates.md#delegate-invocation)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="a3946-2003">`System.Delegate` はデリゲート型ではないため、`operator` `+` は定義されていません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="a3946-2004">減算演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2004">Subtraction operator</span></span>

<span data-ttu-id="a3946-2005">`x - y`フォームの操作では、二項演算子のオーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-2006">オペランドは選択した演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a3946-2007">次に、定義済みの減算演算子を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="a3946-2008">すべての演算子は、`x`から `y` を減算します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="a3946-2009">整数の減算:</span><span class="sxs-lookup"><span data-stu-id="a3946-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="a3946-2010">`checked` コンテキストでは、差分が結果の型の範囲外の場合、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a3946-2011">`unchecked` のコンテキストでは、オーバーフローは報告されず、結果の型の範囲外の重要な上位ビットはすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="a3946-2012">浮動小数点の減算:</span><span class="sxs-lookup"><span data-stu-id="a3946-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="a3946-2013">この違いは、IEEE 754 算術のルールに従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="a3946-2014">次の表に、0以外の有限値、ゼロ、無限大、および Nan のすべての可能な組み合わせの結果を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="a3946-2015">この表では、`x` と `y` は0以外の有限値であり、`z` は `x - y`の結果です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="a3946-2016">`x` と `y` が等しい場合、`z` は正のゼロになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="a3946-2017">`x - y` が変換先の型で表すには大きすぎる場合、`z` は `x - y`と同じ符号を持つ無限大です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="a3946-2018">y</span><span class="sxs-lookup"><span data-stu-id="a3946-2018">y</span></span>    | <span data-ttu-id="a3946-2019">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-2019">+0</span></span>   | <span data-ttu-id="a3946-2020">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-2020">-0</span></span>   | <span data-ttu-id="a3946-2021">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2021">+inf</span></span> | <span data-ttu-id="a3946-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2022">-inf</span></span> | <span data-ttu-id="a3946-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2023">NaN</span></span> | 
   | <span data-ttu-id="a3946-2024">x</span><span class="sxs-lookup"><span data-stu-id="a3946-2024">x</span></span>    | <span data-ttu-id="a3946-2025">z</span><span class="sxs-lookup"><span data-stu-id="a3946-2025">z</span></span>    | <span data-ttu-id="a3946-2026">x</span><span class="sxs-lookup"><span data-stu-id="a3946-2026">x</span></span>    | <span data-ttu-id="a3946-2027">x</span><span class="sxs-lookup"><span data-stu-id="a3946-2027">x</span></span>    | <span data-ttu-id="a3946-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2028">-inf</span></span> | <span data-ttu-id="a3946-2029">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2029">+inf</span></span> | <span data-ttu-id="a3946-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2030">NaN</span></span> | 
   | <span data-ttu-id="a3946-2031">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-2031">+0</span></span>   | <span data-ttu-id="a3946-2032">-y</span><span class="sxs-lookup"><span data-stu-id="a3946-2032">-y</span></span>   | <span data-ttu-id="a3946-2033">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-2033">+0</span></span>   | <span data-ttu-id="a3946-2034">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-2034">+0</span></span>   | <span data-ttu-id="a3946-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2035">-inf</span></span> | <span data-ttu-id="a3946-2036">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2036">+inf</span></span> | <span data-ttu-id="a3946-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2037">NaN</span></span> | 
   | <span data-ttu-id="a3946-2038">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-2038">-0</span></span>   | <span data-ttu-id="a3946-2039">-y</span><span class="sxs-lookup"><span data-stu-id="a3946-2039">-y</span></span>   | <span data-ttu-id="a3946-2040">横-0</span><span class="sxs-lookup"><span data-stu-id="a3946-2040">-0</span></span>   | <span data-ttu-id="a3946-2041">+0</span><span class="sxs-lookup"><span data-stu-id="a3946-2041">+0</span></span>   | <span data-ttu-id="a3946-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2042">-inf</span></span> | <span data-ttu-id="a3946-2043">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2043">+inf</span></span> | <span data-ttu-id="a3946-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2044">NaN</span></span> | 
   | <span data-ttu-id="a3946-2045">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2045">+inf</span></span> | <span data-ttu-id="a3946-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2046">+inf</span></span> | <span data-ttu-id="a3946-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2047">+inf</span></span> | <span data-ttu-id="a3946-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2048">+inf</span></span> | <span data-ttu-id="a3946-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2049">NaN</span></span>  | <span data-ttu-id="a3946-2050">+inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2050">+inf</span></span> | <span data-ttu-id="a3946-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2051">NaN</span></span> | 
   | <span data-ttu-id="a3946-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2052">-inf</span></span> | <span data-ttu-id="a3946-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2053">-inf</span></span> | <span data-ttu-id="a3946-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2054">-inf</span></span> | <span data-ttu-id="a3946-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2055">-inf</span></span> | <span data-ttu-id="a3946-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="a3946-2056">-inf</span></span> | <span data-ttu-id="a3946-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2057">NaN</span></span>  | <span data-ttu-id="a3946-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2058">NaN</span></span> | 
   | <span data-ttu-id="a3946-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2059">NaN</span></span>  | <span data-ttu-id="a3946-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2060">NaN</span></span>  | <span data-ttu-id="a3946-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2061">NaN</span></span>  | <span data-ttu-id="a3946-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2062">NaN</span></span>  | <span data-ttu-id="a3946-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2063">NaN</span></span>  | <span data-ttu-id="a3946-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2064">NaN</span></span>  | <span data-ttu-id="a3946-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="a3946-2065">NaN</span></span> | 

*  <span data-ttu-id="a3946-2066">10進数の減算:</span><span class="sxs-lookup"><span data-stu-id="a3946-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="a3946-2067">結果の値が大きすぎて `decimal` 形式で表現できない場合は、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a3946-2068">結果の小数点以下の桁数は、2つのオペランドのスケールのうち、大きい方になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="a3946-2069">Decimal 減算は、`System.Decimal`型の減算演算子を使用することと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="a3946-2070">列挙型の減算。</span><span class="sxs-lookup"><span data-stu-id="a3946-2070">Enumeration subtraction.</span></span> <span data-ttu-id="a3946-2071">すべての列挙型には、暗黙的に次の定義済みの演算子が用意されています。 `E` は列挙型であり、`U` は `E`の基になる型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="a3946-2072">この演算子は `(U)((U)x - (U)y)`として正確に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="a3946-2073">言い換えると、演算子は `x` と `y`の序数値の差を計算し、結果の型は列挙型の基になる型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="a3946-2074">この演算子は `(E)((U)x - y)`として正確に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="a3946-2075">つまり、演算子は列挙体の基になる型から値を減算し、列挙体の値を生成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="a3946-2076">デリゲートの削除。</span><span class="sxs-lookup"><span data-stu-id="a3946-2076">Delegate removal.</span></span> <span data-ttu-id="a3946-2077">すべてのデリゲート型は、次の定義済み演算子を暗黙的に提供します。 `D` はデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="a3946-2078">二項 `-` 演算子は、両方のオペランドがデリゲート型 `D`である場合に、デリゲートの削除を実行します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="a3946-2079">オペランドのデリゲート型が異なる場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="a3946-2080">最初のオペランドが `null` の場合は、演算結果は `null` になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="a3946-2081">それ以外の場合、2番目のオペランドが `null`場合、演算の結果は最初のオペランドの値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="a3946-2082">それ以外の場合、どちらのオペランドも1つ以上のエントリを持つ呼び出しリスト ([デリゲート宣言](delegates.md#delegate-declarations)) を表します。結果は、2番目のオペランドのリストが最初ののサブリストである場合に、2番目のオペランドのエントリが削除された最初のオペランドリストで構成される新しい呼び出しリストになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="a3946-2083">(サブリストが等しいかどうかを判断するために、対応するエントリは、デリゲート等値演算子 ([デリゲート等値演算子](expressions.md#delegate-equality-operators)) と比較されます。それ以外の場合、結果は左オペランドの値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="a3946-2084">このプロセスでは、オペランドのリストはいずれも変更されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="a3946-2085">2番目のオペランドのリストが、最初のオペランドのリスト内の連続するエントリの複数のサブリストと一致する場合、連続するエントリの右側に一致するサブリストが削除されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="a3946-2086">削除によりリストが空になる場合、結果は `null` になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="a3946-2087">例 :</span><span class="sxs-lookup"><span data-stu-id="a3946-2087">For example:</span></span>

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a><span data-ttu-id="a3946-2088">シフト演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2088">Shift operators</span></span>

<span data-ttu-id="a3946-2089">`<<` 演算子と `>>` 演算子は、ビットシフト演算を実行するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="a3946-2090">*Shift_expression*のオペランドにコンパイル時の型 `dynamic`がある場合、式は動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a3946-2091">この場合、式のコンパイル時の型は `dynamic`であり、以下に示す解決方法は、コンパイル時の型 `dynamic`を持つオペランドの実行時の型を使用して実行時に実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="a3946-2092">`x << count` または `x >> count`の形式の操作では、二項演算子のオーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-2093">オペランドは選択した演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a3946-2094">オーバーロードされたシフト演算子を宣言する場合、最初のオペランドの型は、常に演算子宣言を含むクラスまたは構造体である必要があります。また、2番目のオペランドの型は常に `int`である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="a3946-2095">次に、定義済みのシフト演算子を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="a3946-2096">左シフト:</span><span class="sxs-lookup"><span data-stu-id="a3946-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="a3946-2097">`<<` 演算子は、次に示すように計算されたビット数だけ、`x` をシフトします。</span><span class="sxs-lookup"><span data-stu-id="a3946-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="a3946-2098">`x` の結果の型の範囲外の上位ビットは破棄され、残りのビットは左にシフトされ、下位の空のビット位置は0に設定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="a3946-2099">右シフト:</span><span class="sxs-lookup"><span data-stu-id="a3946-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="a3946-2100">`>>` 演算子は、次に示すように、計算されたビット数だけ右に `x` をシフトします。</span><span class="sxs-lookup"><span data-stu-id="a3946-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="a3946-2101">`x` が `int` または `long`型の場合、`x` の下位ビットは破棄され、残りのビットは右にシフトされます。また、`x` が負ではない場合、上位の空のビット位置は0に設定されます。`x`</span><span class="sxs-lookup"><span data-stu-id="a3946-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="a3946-2102">`x` が `uint` または `ulong`型の場合、`x` の下位ビットは破棄され、残りのビットは右にシフトされ、上位の空のビット位置は0に設定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="a3946-2103">定義済みの演算子の場合、シフトするビット数は次のように計算されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="a3946-2104">`x` の種類が `int` または `uint`の場合、シフト数は `count`の下位5ビットによって指定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="a3946-2105">つまり、シフト数は `count & 0x1F`から計算されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="a3946-2106">`x` の種類が `long` または `ulong`の場合、シフト数は `count`の下位6ビットによって指定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="a3946-2107">つまり、シフト数は `count & 0x3F`から計算されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="a3946-2108">結果として得られるシフト数が0の場合、シフト演算子は単に `x`の値を返します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="a3946-2109">シフト操作ではオーバーフローが発生することはなく、`checked` と `unchecked` のコンテキストでも同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="a3946-2110">`>>` 演算子の左オペランドが符号付き整数型の場合、演算子は、オペランドの最上位ビット (符号ビット) の値が上位の空のビット位置に反映される、算術シフト右を実行します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="a3946-2111">`>>` 演算子の左オペランドが符号なし整数型の場合、演算子は論理シフト右を実行します。この場合、上位の空のビット位置は常に0に設定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="a3946-2112">オペランド型から推論されたの逆の演算を実行するには、明示的なキャストを使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="a3946-2113">たとえば、`x` が `int`型の変数である場合、操作 `unchecked((int)((uint)x >> y))` `x`の論理右シフトを実行します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="a3946-2114">関係演算子と型検査演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="a3946-2115">`==`、`!=`、`<`、`>`、`<=`、`>=`、`is`、および `as` の各演算子は、関係演算子と型テスト演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

<span data-ttu-id="a3946-2116">`is` 演算子は[is 演算子](expressions.md#the-is-operator)で説明されており、`as` 演算子は[as 演算子](expressions.md#the-as-operator)で説明されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="a3946-2117">`==`、`!=`、`<`、`>`、`<=`、および `>=` の各演算子は、***比較演算子***です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="a3946-2118">比較演算子のオペランドに `dynamic`コンパイル時の型がある場合、式は動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a3946-2119">この場合、式のコンパイル時の型は `dynamic`であり、以下に示す解決方法は、コンパイル時の型 `dynamic`を持つオペランドの実行時の型を使用して実行時に実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="a3946-2120">Op を `x` *op* `y`形式の操作では、 *op*は比較演算子であり、オーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-2121">オペランドは選択した演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a3946-2122">定義済みの比較演算子については、次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="a3946-2123">次の表で説明するように、すべての定義済みの比較演算子は `bool`型の結果を返します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="a3946-2124">__操作__</span><span class="sxs-lookup"><span data-stu-id="a3946-2124">__Operation__</span></span> | <span data-ttu-id="a3946-2125">__結果__</span><span class="sxs-lookup"><span data-stu-id="a3946-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="a3946-2126">`true` `x` が `y`に等しい場合は、それ以外の場合は `false`</span><span class="sxs-lookup"><span data-stu-id="a3946-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="a3946-2127">`true` `x` が `y`と等しくない場合は、それ以外の場合は `false`</span><span class="sxs-lookup"><span data-stu-id="a3946-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="a3946-2128">`true` `x` が `y`より小さい場合は、それ以外の場合は `false`</span><span class="sxs-lookup"><span data-stu-id="a3946-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="a3946-2129">`true` `x` が `y`より大きい場合は、それ以外の場合は `false`</span><span class="sxs-lookup"><span data-stu-id="a3946-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="a3946-2130">`true` `x` が `y`以下の場合は、それ以外の場合は `false`</span><span class="sxs-lookup"><span data-stu-id="a3946-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="a3946-2131">`true` `x` が `y`以上の場合は、それ以外の場合は `false`</span><span class="sxs-lookup"><span data-stu-id="a3946-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="a3946-2132">整数の比較演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2132">Integer comparison operators</span></span>

<span data-ttu-id="a3946-2133">定義済みの整数の比較演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2133">The predefined integer comparison operators are:</span></span>
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

<span data-ttu-id="a3946-2134">これらの各演算子は、2つの整数オペランドの数値を比較し、特定のリレーションシップが `true` または `false`かどうかを示す `bool` 値を返します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="a3946-2135">浮動小数点比較演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="a3946-2136">定義済みの浮動小数点比較演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2136">The predefined floating-point comparison operators are:</span></span>
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

<span data-ttu-id="a3946-2137">演算子は、IEEE 754 標準の規則に従って、オペランドを比較します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="a3946-2138">どちらかのオペランドが NaN の場合、結果は `!=`を除くすべての演算子に対して `false` ます。この場合、結果は `true`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="a3946-2139">2つのオペランドの場合、`x != y` は常に `!(x == y)`と同じ結果を生成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="a3946-2140">一方または両方のオペランドが NaN の場合、`<`、`>`、`<=`、および `>=` の各演算子は、逆の演算子の論理否定と同じ結果を生成しません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="a3946-2141">たとえば、`x` と `y` のいずれかが NaN の場合、`x < y` は `false`ますが、`!(x >= y)` は `true`です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="a3946-2142">どちらのオペランドも NaN の場合、演算子は、2つの浮動小数点オペランドの値を順序付けと比較します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="a3946-2143">ここで `min` と `max` は、指定された浮動小数点形式で表すことができる最小値と最大値の正の有限値です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="a3946-2144">この順序の主な影響は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="a3946-2145">負のゼロと正のゼロは等しいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="a3946-2146">負の無限大は、他のすべての値よりも小さいと見なされますが、負の負の無限大と同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="a3946-2147">正の無限大は、他のすべての値よりも大きいと見なされますが、もう1つの正の無限大と同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="a3946-2148">10進数の比較演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2148">Decimal comparison operators</span></span>

<span data-ttu-id="a3946-2149">定義済みの10進数比較演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="a3946-2150">これらの各演算子は、2つの10進オペランドの数値を比較し、特定のリレーションシップが `true` または `false`かどうかを示す `bool` 値を返します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="a3946-2151">各10進数の比較は、`System.Decimal`型の対応する関係演算子または等値演算子を使用することと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="a3946-2152">ブール等値演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2152">Boolean equality operators</span></span>

<span data-ttu-id="a3946-2153">定義済みのブール等価演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="a3946-2154">`x` と `y` の両方が `true` である場合、または `x` と `y` の両方が `false`場合は、`==` の結果が `true` ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="a3946-2155">それ以外の場合、結果は `false` です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="a3946-2156">`x` と `y` の両方が `true` である場合、または `x` と `y` の両方が `false`場合は、`!=` の結果が `false` ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="a3946-2157">それ以外の場合、結果は `true` です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="a3946-2158">オペランドの型が `bool`の場合、`!=` 演算子は `^` 演算子と同じ結果を生成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="a3946-2159">列挙型の比較演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="a3946-2160">すべての列挙型には、次の定義済みの比較演算子が暗黙的に用意されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="a3946-2161">`x op y`を評価した結果 `x` と `y` は、基になる型 `U`で `E` 列挙型の式であり、比較演算子の1つである `op` は `((U)x) op ((U)y)`の評価とまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="a3946-2162">つまり、列挙型の比較演算子は、単に2つのオペランドの基になる整数値を比較します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="a3946-2163">参照型等値演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2163">Reference type equality operators</span></span>

<span data-ttu-id="a3946-2164">定義済みの参照型等値演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="a3946-2165">演算子は、2つの参照の等価性または非等価性を比較した結果を返します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="a3946-2166">定義済みの参照型等値演算子は `object`型のオペランドを受け入れるため、適用可能な `operator ==` および `operator !=` メンバーを宣言しないすべての型に適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="a3946-2167">反対に、適用可能なユーザー定義等値演算子は、定義済みの参照型等値演算子を効果的に非表示にします。</span><span class="sxs-lookup"><span data-stu-id="a3946-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="a3946-2168">定義済みの参照型等値演算子には、次のいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="a3946-2169">どちらのオペランドも、 *reference_type*またはリテラル `null`であると認識される型の値です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="a3946-2170">さらに、明示的な参照変換 ([明示的な参照](conversions.md#explicit-reference-conversions)変換) は、いずれかのオペランドの型からもう一方のオペランドの型に存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="a3946-2171">1つのオペランドは `T` 型の値で、`T` は*type_parameter*であり、もう一方のオペランドはリテラル `null`です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="a3946-2172">さらに `T` には値型の制約がありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="a3946-2173">これらの条件のいずれかが満たされていない限り、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="a3946-2174">これらの規則の主な影響は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="a3946-2175">定義済みの参照型等値演算子を使用して、バインド時に異なることがわかっている2つの参照を比較するためのバインド時エラーです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="a3946-2176">たとえば、オペランドのバインド時の型が `A` および `B`の2つのクラス型であり、どちらも `A` も `B` も派生していない場合は、2つのオペランドが同じオブジェクトを参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="a3946-2177">このため、この操作はバインド時エラーと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="a3946-2178">定義済みの参照型等値演算子では、値型のオペランドを比較することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="a3946-2179">したがって、構造体型で独自の等値演算子が宣言されていない限り、その構造体型の値を比較することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="a3946-2180">定義済みの参照型等値演算子は、オペランドに対してボックス化操作が発生することはありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="a3946-2181">新しく割り当てられたボックス化されたインスタンスへの参照は、他のすべての参照とは必ず異なるため、このようなボックス化操作を実行することは無意味です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="a3946-2182">`T` 型パラメーター型のオペランドが `null`と比較され、`T` の実行時の型が値型である場合、比較の結果は `false`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="a3946-2183">次の例では、制約のない型パラメーター型の引数が `null`かどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="a3946-2184">`x == null` コンストラクトは、`T` が値型を表している場合でも許可されます。 `T` が値型の場合、結果は単純に `false` されるように定義されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="a3946-2185">`x == y` または `x != y`の形式の操作では、適用可能な `operator ==` または `operator !=` が存在する場合、演算子のオーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) ルールによって、定義済みの参照型等値演算子ではなく、その演算子が選択されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="a3946-2186">ただし、オペランドの一方または両方を明示的に `object`型にキャストすることで、定義済みの参照型等値演算子を常に選択できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="a3946-2187">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2187">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
<span data-ttu-id="a3946-2188">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2188">produces the output</span></span>
```console
True
False
False
False
```

<span data-ttu-id="a3946-2189">`s` 変数と `t` 変数は、同じ文字を含む2つの異なる `string` インスタンスを参照します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="a3946-2190">2つのオペランドの型が `string`の場合、定義済みの文字列等値演算子 ([文字列等値](expressions.md#string-equality-operators)演算子) が選択されているので、最初の比較で `True` が出力されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="a3946-2191">一方または両方のオペランドが `object`型の場合、定義済みの参照型等値演算子が選択されているので、残りのすべての出力 `False` になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="a3946-2192">上記の手法は、値型では意味がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="a3946-2193">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2193">The example</span></span>
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
<span data-ttu-id="a3946-2194">キャストによって、ボックス化された `int` 値の2つの異なるインスタンスへの参照が作成されるため、`False` 出力されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="a3946-2195">文字列等値演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2195">String equality operators</span></span>

<span data-ttu-id="a3946-2196">事前定義された文字列等値演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="a3946-2197">次のいずれかに該当する場合、2つの `string` 値は等しいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="a3946-2198">両方の値が `null`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="a3946-2199">両方の値は、長さが同じ文字列インスタンスへの null 参照と、各文字位置に同一の文字を持ちます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="a3946-2200">文字列の等値演算子は、文字列参照ではなく文字列値を比較します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="a3946-2201">2つの個別の文字列インスタンスにまったく同じ文字シーケンスが含まれている場合、文字列の値は同じですが、参照が異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="a3946-2202">「[参照型等値演算子](expressions.md#reference-type-equality-operators)」で説明されているように、参照型等値演算子を使用すると、文字列値ではなく文字列参照を比較できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="a3946-2203">デリゲート等値演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2203">Delegate equality operators</span></span>

<span data-ttu-id="a3946-2204">すべてのデリゲート型には、次の定義済みの比較演算子が暗黙的に用意されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="a3946-2205">次の2つのデリゲートインスタンスが等しいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="a3946-2206">いずれかのデリゲートインスタンスが `null`場合、両方が `null`場合に限り、これらのインスタンスは等しいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="a3946-2207">デリゲートの実行時の型が異なる場合、それらは等しくなりません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="a3946-2208">両方のデリゲートインスタンスが呼び出しリスト ([デリゲート宣言](delegates.md#delegate-declarations)) を持っている場合、それらのインスタンスは、呼び出しリストが同じ長さであり、1つの呼び出しリスト内の各エントリが、他の呼び出しリストにおいて、対応するエントリに順番に等しいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="a3946-2209">次の規則は、呼び出しリストエントリが等しいかどうかを制御します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="a3946-2210">2つの呼び出しリストエントリが両方とも同じ静的メソッドを参照している場合、エントリは同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="a3946-2211">2つの呼び出しリストエントリが、(参照等値演算子で定義されているように) 同じターゲットオブジェクトで同じ非静的メソッドを参照している場合、エントリは同じになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="a3946-2212">同じ (場合によっては空の) キャプチャされた外部変数インスタンスのセットを持つ、意味的に同一の*anonymous_method_expression*s または*lambda_expression*の評価から生成された呼び出しリストのエントリは、同じにすることができます (ただし、必須ではありません)。</span><span class="sxs-lookup"><span data-stu-id="a3946-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="a3946-2213">等値演算子と null</span><span class="sxs-lookup"><span data-stu-id="a3946-2213">Equality operators and null</span></span>

<span data-ttu-id="a3946-2214">`==` 演算子と `!=` 演算子では、1つのオペランドを null 許容型の値にすることができます。また、操作に対して定義済みまたはユーザー定義の演算子 (unlifted またはリフト形式) が存在しない場合でも、一方のオペランドは `null` リテラルになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="a3946-2215">いずれかの形式の操作の場合</span><span class="sxs-lookup"><span data-stu-id="a3946-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="a3946-2216">ここで `x` は null 許容型の式で、演算子のオーバーロード解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) が適用可能な演算子を見つけることができない場合、結果は `x`の `HasValue` プロパティから計算されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="a3946-2217">具体的には、最初の2つの形式は `!x.HasValue`に変換され、最後の2つの形式は `x.HasValue`に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="a3946-2218">Is 演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2218">The is operator</span></span>

<span data-ttu-id="a3946-2219">`is` 演算子は、オブジェクトの実行時の型が特定の型と互換性があるかどうかを動的に確認するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="a3946-2220">演算の結果 `E is T`。ここで `E` は式で、`T` は型であり、参照変換、ボックス化変換、またはボックス化解除変換によって `E` を `T` 型に正常に変換できるかどうかを示すブール値です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="a3946-2221">型引数がすべての型パラメーターに置き換えられた後、演算は次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="a3946-2222">`E` が匿名関数の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="a3946-2223">`E` がメソッドグループまたは `null` リテラルである場合、`E` の型が参照型または null 許容型で、`E` の値が null の場合、結果は false になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="a3946-2224">それ以外の場合は、次のように `E` の動的な型を `D` します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="a3946-2225">`E` の型が参照型の場合、`D` は `E`によってインスタンス参照の実行時の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="a3946-2226">`E` の型が null 許容型である場合、`D` はその null 許容型の基になる型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="a3946-2227">`E` の型が null 非許容の値型の場合、`D` は `E`の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="a3946-2228">操作の結果は `D` と `T` によって次のように異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="a3946-2229">`T` が参照型の場合、結果は true になります。これは、`D` と `T` が同じ型である場合、`D` が参照型である場合、および `D` から `T` 存在する暗黙的な参照変換である場合、または `D` から `D` へのボックス変換が存在する場合に発生します。`T`</span><span class="sxs-lookup"><span data-stu-id="a3946-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="a3946-2230">`T` が null 許容型である場合、`D` が `T`の基になる型である場合、結果は true になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="a3946-2231">`T` が null 非許容の値型である場合、`D` と `T` が同じ型である場合、結果は true になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="a3946-2232">それ以外の場合、結果は false になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="a3946-2233">ユーザー定義の変換は、`is` 演算子では考慮されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="a3946-2234">As 演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2234">The as operator</span></span>

<span data-ttu-id="a3946-2235">`as` 演算子は、特定の参照型または null 許容型に値を明示的に変換するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="a3946-2236">キャスト式 ([cast 式](expressions.md#cast-expressions)) とは異なり、`as` 演算子は例外をスローしません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="a3946-2237">代わりに、指定された変換ができない場合、結果の値は `null`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="a3946-2238">`E as T`フォームの操作では、`E` は式である必要があり、`T` は参照型、参照型であることがわかっている型パラメーター、または null 許容型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="a3946-2239">さらに、次のうち少なくとも1つが true である必要があります。そうでない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="a3946-2240">Id ([id 変換](conversions.md#identity-conversion))、暗黙の null 許容型変換 ([暗黙の null 許容](conversions.md#implicit-nullable-conversions)型変換)、暗黙的参照 ([暗黙的な参照変換](conversions.md#implicit-reference-conversions))、ボックス化 ([ボックス](conversions.md#boxing-conversions)化変換)、明示的な null 許容型変換[(明示的](conversions.md#explicit-reference-conversions)な[Null 許容変換](conversions.md#explicit-nullable-conversions))、またはボックス化解除 ([ボックス化変換](conversions.md#unboxing-conversions)) 変換は、`E` から `T`に存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="a3946-2241">`E` または `T` の型がオープン型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="a3946-2242">`E` は `null` リテラルです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="a3946-2243">`E` のコンパイル時の型が `dynamic`でない場合、操作 `E as T` によって同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="a3946-2244">ただし、`E` が評価されるのは 1 回だけです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="a3946-2245">コンパイラは、上記の拡張によって暗黙的に指定された2つの動的な型チェックではなく、最大で1つの動的な型チェックを実行するように `E as T` を最適化することを想定できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="a3946-2246">`E` のコンパイル時の型が `dynamic`場合、cast 演算子とは異なり、`as` 演算子は動的にバインドされません ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a3946-2247">したがって、この場合の展開は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="a3946-2248">ユーザー定義変換など、一部の変換は `as` 演算子では実行できません。代わりに、キャスト式を使用して実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="a3946-2249">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-2249">In the example</span></span>
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
<span data-ttu-id="a3946-2250">`G` の型パラメーター `T` は、クラスの制約があるため、参照型であることがわかっています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="a3946-2251">`H` の型パラメーター `U` がではありません。したがって、`H` での `as` 演算子の使用は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="a3946-2252">論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2252">Logical operators</span></span>

<span data-ttu-id="a3946-2253">`&`、`^`、および `|` の各演算子は、論理演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

<span data-ttu-id="a3946-2254">論理演算子のオペランドに `dynamic`コンパイル時の型がある場合、式は動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a3946-2255">この場合、式のコンパイル時の型は `dynamic`であり、以下に示す解決方法は、コンパイル時の型 `dynamic`を持つオペランドの実行時の型を使用して実行時に実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="a3946-2256">`x op y`形式の操作の場合、`op` は論理演算子の1つであるため、オーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) が適用され、特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a3946-2257">オペランドは選択した演算子のパラメーターの型に変換され、結果の型は演算子の戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a3946-2258">定義済みの論理演算子については、次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="a3946-2259">整数の論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2259">Integer logical operators</span></span>

<span data-ttu-id="a3946-2260">定義済みの整数の論理演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2260">The predefined integer logical operators are:</span></span>
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

<span data-ttu-id="a3946-2261">`&` 演算子は、2つのオペランドのビットごとの論理 `AND` を計算します。 `|` `OR` 演算子は、2つのオペランドのビットごとの論理和を計算し、`^` 演算子は2つのオペランドのビットごとの論理和を計算します。`OR`</span><span class="sxs-lookup"><span data-stu-id="a3946-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="a3946-2262">これらの操作によってオーバーフローが発生することはありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="a3946-2263">列挙論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2263">Enumeration logical operators</span></span>

<span data-ttu-id="a3946-2264">すべての列挙型 `E` は、次の定義済みの論理演算子を暗黙的に提供します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="a3946-2265">`x op y`を評価した結果 `x` と `y` は、基になる型 `U`で `E` 列挙型の式であり、`op` は論理演算子の1つであり、`(E)((U)x op (U)y)`の評価とまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="a3946-2266">言い換えると、列挙型の論理演算子は、2つのオペランドの基になる型に対して論理演算を実行するだけです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="a3946-2267">ブール論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2267">Boolean logical operators</span></span>

<span data-ttu-id="a3946-2268">定義済みのブール型の論理演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="a3946-2269">`x & y` と `true` の両方が `x` であれば、`y` の結果は `true` です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="a3946-2270">それ以外の場合、結果は `false` です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="a3946-2271">`x` または `y` が `true`の場合、`x | y` の結果は `true` です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="a3946-2272">それ以外の場合、結果は `false` です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="a3946-2273">`x` が `true` で `y` が `false`である場合、または `x` が `false` で `y` が `true`場合、`x ^ y` の結果は `true` ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="a3946-2274">それ以外の場合、結果は `false` です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="a3946-2275">オペランドの型が `bool`の場合、`^` 演算子は `!=` 演算子と同じ結果を計算します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="a3946-2276">Null 許容のブール型論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="a3946-2277">Null 許容型 `bool?` は、3つの値、`true`、`false`、および `null`を表すことができ、概念的には、SQL のブール式に使用される3つの値の型に似ています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="a3946-2278">`bool?` オペランドの `&` および `|` 演算子によって生成された結果が SQL の3つの値のロジックと一致することを確認するために、次の定義済みの演算子が用意されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="a3946-2279">次の表に、`true`、`false`、および `null`値のすべての組み合わせについて、これらの演算子によって生成される結果を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a><span data-ttu-id="a3946-2280">条件付き論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2280">Conditional logical operators</span></span>

<span data-ttu-id="a3946-2281">`&&` 演算子と `||` 演算子は、条件付き論理演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="a3946-2282">"ショートサーキット" 論理演算子とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2282">They are also called the "short-circuiting" logical operators.</span></span>

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

<span data-ttu-id="a3946-2283">`&&` 演算子と `||` 演算子は、`&` および `|` 演算子の条件付きバージョンです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="a3946-2284">操作 `x && y` は操作 `x & y`に対応しますが、`x` が `false`されていない場合にのみ `y` が評価される点が異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="a3946-2285">操作 `x || y` は操作 `x | y`に対応しますが、`x` が `true`されていない場合にのみ `y` が評価される点が異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="a3946-2286">条件付き論理演算子のオペランドに `dynamic`コンパイル時の型がある場合、式は動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a3946-2287">この場合、式のコンパイル時の型は `dynamic`であり、以下に示す解決方法は、コンパイル時の型 `dynamic`を持つオペランドの実行時の型を使用して実行時に実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="a3946-2288">`x && y` または `x || y` の形式の操作は、操作が `x & y` または `x | y`に記述されているかのように、オーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) を適用することによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="a3946-2289">そうしたら</span><span class="sxs-lookup"><span data-stu-id="a3946-2289">Then,</span></span>

*  <span data-ttu-id="a3946-2290">オーバーロードの解決が1つの最適な演算子を見つけることができない場合、またはオーバーロードの解決で定義済みの整数の論理演算子が1つ選択された場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="a3946-2291">それ以外の場合、選択した演算子が、定義済みのブール型論理演算子 ([ブール](expressions.md#boolean-logical-operators)型論理演算子) または null 許容ブール値論理演算子 ([null 許容のブール値論理](expressions.md#nullable-boolean-logical-operators)演算子) のいずれかである場合、この操作は、「[ブール条件論理演算子](expressions.md#boolean-conditional-logical-operators)」の説明に従って処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="a3946-2292">それ以外の場合、選択された演算子はユーザー定義の演算子であり、操作は[ユーザー定義の条件付き論理演算子](expressions.md#user-defined-conditional-logical-operators)の説明に従って処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="a3946-2293">条件付き論理演算子を直接オーバーロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="a3946-2294">ただし、条件付き論理演算子は、通常の論理演算子の観点で評価されるため、通常の論理演算子のオーバーロードは、特定の制限がある、条件付き論理演算子のオーバーロードとも見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="a3946-2295">詳細については[、ユーザー定義の条件付き論理演算子](expressions.md#user-defined-conditional-logical-operators)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="a3946-2296">ブール条件論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="a3946-2297">`&&` または `||` のオペランドが `bool`型である場合、またはオペランドが適用可能な `operator &` または `operator |`を定義しない型の場合、または `bool`への暗黙的な変換を定義する場合、操作は次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="a3946-2298">操作 `x && y` は `x ? y : false`として評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="a3946-2299">つまり、`x` は最初に評価され、`bool`型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="a3946-2300">次に、`x` が `true`場合、`y` が評価され `bool`型に変換され、これが操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="a3946-2301">それ以外の場合、操作の結果は `false`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="a3946-2302">操作 `x || y` は `x ? true : y`として評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="a3946-2303">つまり、`x` は最初に評価され、`bool`型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="a3946-2304">次に、`x` が `true`場合、操作の結果は `true`ます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="a3946-2305">それ以外の場合、`y` が評価され `bool`型に変換され、これが操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="a3946-2306">ユーザー定義の条件付き論理演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="a3946-2307">`&&` または `||` のオペランドが、適用可能なユーザー定義の `operator &` または `operator |`を宣言する型である場合、次の両方が true である必要があります。ここで、`T` は、選択した演算子が宣言されている型です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="a3946-2308">戻り値の型と、選択した演算子の各パラメーターの型は `T`である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="a3946-2309">言い換えると、演算子は `T`型の2つのオペランドの論理 `AND` または論理 `OR` を計算する必要があり、型 `T`の結果を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="a3946-2310">`T` に `operator true` と `operator false`の宣言が含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="a3946-2311">これらの要件のいずれかが満たされていない場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="a3946-2312">それ以外の場合、`&&` または `||` 操作は、ユーザー定義の `operator true` または `operator false` を選択したユーザー定義演算子と組み合わせて評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="a3946-2313">操作 `x && y` は `T.false(x) ? x : T.&(x, y)`として評価されます。 `T.false(x)` は `T`で宣言された `operator false` の呼び出しで、`T.&(x, y)` は選択した `operator &`の呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="a3946-2314">つまり、`x` は最初に評価され、結果に対して `operator false` が呼び出され、`x` が確実に false であるかどうかが判断されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="a3946-2315">`x` が確実に false の場合、操作の結果は、以前に `x`に対して計算された値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="a3946-2316">それ以外の場合、`y` が評価され、選択した `operator &` が `x` に対して以前に計算された値と、操作の結果を生成するために `y` に対して計算された値に対して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="a3946-2317">操作 `x || y` は `T.true(x) ? x : T.|(x, y)`として評価されます。 `T.true(x)` は `T`で宣言された `operator true` の呼び出しで、`T.|(x,y)` は選択した `operator|`の呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="a3946-2318">つまり、`x` が最初に評価され、結果に対して `operator true` が呼び出され、`x` が確実に真であるかどうかが判断されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="a3946-2319">`x` が確実に true の場合、操作の結果は、以前に `x`に対して計算された値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="a3946-2320">それ以外の場合、`y` が評価され、選択した `operator |` が `x` に対して以前に計算された値と、操作の結果を生成するために `y` に対して計算された値に対して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="a3946-2321">これらのいずれの操作でも、`x` によって指定された式は1回だけ評価され、`y` によって指定された式は評価されず、正確には評価されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="a3946-2322">`operator true` と `operator false`を実装する型の例については、「[データベースのブール型](structs.md#database-boolean-type)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="a3946-2323">Null 合体演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2323">The null coalescing operator</span></span>

<span data-ttu-id="a3946-2324">`??` 演算子は、null 合体演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="a3946-2325">`a ?? b` 形式の null 結合式では、`a` null 許容型または参照型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="a3946-2326">`a` が null 以外の場合、`a ?? b` の結果は `a`ます。それ以外の場合、結果は `b`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="a3946-2327">操作は、`a` が null の場合にのみ `b` を評価します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="a3946-2328">Null 合体演算子は、右から左の操作がグループ化されていることを意味します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="a3946-2329">たとえば、`a ?? b ?? c` フォームの式は `a ?? (b ?? c)`として評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="a3946-2330">一般的に、`E1 ?? E2 ?? ... ?? En` 形式の式では、null 以外のオペランドの最初の値が返されます。すべてのオペランドが null の場合は null になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="a3946-2331">`a ?? b` 式の型は、オペランドで使用できる暗黙の変換によって異なります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="a3946-2332">優先順位では、`a ?? b` の型は `A0`、`A`、または `B`です。ここで `A` は `a` の型 (`a` に型がある場合)、`B` が null 許容型である場合は `b` の基になる型であり、それ以外の場合は `b` になります。`A0``A``A``A`</span><span class="sxs-lookup"><span data-stu-id="a3946-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="a3946-2333">具体的には、`a ?? b` は次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="a3946-2334">`A` が存在し、null 許容型または参照型でない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="a3946-2335">`b` が動的な式の場合、結果の型は `dynamic`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="a3946-2336">実行時には、`a` が最初に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="a3946-2337">`a` が null でない場合、`a` は動的に変換され、これが結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="a3946-2338">それ以外の場合は `b` が評価され、これが結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="a3946-2339">それ以外の場合、`A` 存在し、null 許容型であり、`b` から `A0`への暗黙的な変換が存在する場合、結果の型は `A0`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="a3946-2340">実行時には、`a` が最初に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="a3946-2341">`a` が null でない場合、`a` は `A0`型にラップ解除され、結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="a3946-2342">それ以外の場合、`b` が評価され、`A0`型に変換されて、結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="a3946-2343">それ以外の場合、`A` が存在し、`b` から `A`への暗黙的な変換が存在する場合、結果の型は `A`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="a3946-2344">実行時には、`a` が最初に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="a3946-2345">`a` が null でない場合、`a` が結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="a3946-2346">それ以外の場合、`b` が評価され、`A`型に変換されて、結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="a3946-2347">それ以外の場合、`b` に `B` 型があり、`a` から `B`への暗黙的な変換が存在する場合、結果の型は `B`になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="a3946-2348">実行時には、`a` が最初に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="a3946-2349">`a` が null でない場合、`a` は `A0` 型にラップ解除され (`A` 存在し、null 値が許容される場合)、型 `B`に変換され、これが結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="a3946-2350">それ以外の場合、`b` が評価され、結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="a3946-2351">それ以外の場合、`a` と `b` に互換性がなく、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="a3946-2352">条件演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2352">Conditional operator</span></span>

<span data-ttu-id="a3946-2353">`?:` 演算子は、条件演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="a3946-2354">これは、三項演算子とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="a3946-2355">`b ? x : y` フォームの条件式は、最初に条件 `b`を評価します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="a3946-2356">次に、`b` が `true`場合、`x` が評価され、操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="a3946-2357">それ以外の場合、`y` が評価され、操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="a3946-2358">条件式では、`x` と `y`の両方が評価されることはありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="a3946-2359">条件演算子は、右から左の操作がグループ化されていることを意味します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="a3946-2360">たとえば、`a ? b : c ? d : e` フォームの式は `a ? b : (c ? d : e)`として評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="a3946-2361">`?:` 演算子の最初のオペランドは、`bool`、または `operator true`を実装する型の式に暗黙的に変換できる式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="a3946-2362">これらの要件のいずれも満たされない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="a3946-2363">`?:` 演算子の2番目と3番目のオペランドである `x` および `y`は、条件式の型を制御します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="a3946-2364">`x` の型が `X` で `y` 型がある場合は `Y`</span><span class="sxs-lookup"><span data-stu-id="a3946-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="a3946-2365">暗黙の変換 ([暗黙](conversions.md#implicit-conversions)の変換) が `X` から `Y`に存在するが、`Y` から `X`には存在しない場合、`Y` が条件式の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="a3946-2366">暗黙の変換 ([暗黙](conversions.md#implicit-conversions)の変換) が `Y` から `X`に存在するが、`X` から `Y`には存在しない場合、`X` が条件式の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="a3946-2367">それ以外の場合は、式の型を特定できないため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="a3946-2368">`x` と `y` の1つだけが型を持ち、`x` と `y`の両方がその型に暗黙的に変換可能な場合は、それが条件式の型になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="a3946-2369">それ以外の場合は、式の型を特定できないため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="a3946-2370">`b ? x : y` フォームの条件式の実行時処理は、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="a3946-2371">まず、`b` が評価され、`b` の `bool` 値が決定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="a3946-2372">`b` の型から `bool` への暗黙的な変換が存在する場合、この暗黙の変換は `bool` 値を生成するために実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="a3946-2373">それ以外の場合、`b` の型によって定義された `operator true` は、`bool` 値を生成するために呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="a3946-2374">上記の手順で生成された `bool` 値が `true`場合、`x` が評価され、条件式の型に変換され、これが条件式の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="a3946-2375">それ以外の場合、`y` が評価され、条件式の型に変換されます。これは条件式の結果になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="a3946-2376">匿名関数式</span><span class="sxs-lookup"><span data-stu-id="a3946-2376">Anonymous function expressions</span></span>

<span data-ttu-id="a3946-2377">***匿名関数***は、"インライン" メソッド定義を表す式です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="a3946-2378">匿名関数は、それ自体の値または型を持っていませんが、互換性のあるデリゲートまたは式ツリー型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="a3946-2379">匿名関数の変換の評価は、変換の対象の型によって異なります。これがデリゲート型である場合、変換は、匿名関数が定義するメソッドを参照するデリゲート値に評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="a3946-2380">式ツリー型の場合、変換は、オブジェクト構造としてのメソッドの構造を表す式ツリーに評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="a3946-2381">歴史的な理由により、匿名関数には2つの構文があります。つまり*lambda_expression*s と*anonymous_method_expression*s です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="a3946-2382">ほとんどの場合、 *lambda_expression*は*anonymous_method_expression*より簡潔で表現力が豊かで、下位互換性のために言語に残されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

<span data-ttu-id="a3946-2383">`=>` 演算子と代入 (`=`) は優先順位が同じで、結合規則が右から左です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="a3946-2384">`async` 修飾子を持つ匿名関数は、非同期関数であり、「[反復子](classes.md#iterators)」で説明されている規則に従います。</span><span class="sxs-lookup"><span data-stu-id="a3946-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="a3946-2385">*Lambda_expression*形式の匿名関数のパラメーターは、明示的または暗黙的に型指定できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="a3946-2386">明示的に型指定されたパラメーターリストでは、各パラメーターの型が明示的に指定されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="a3946-2387">暗黙的に型指定されたパラメーターリストでは、匿名関数が発生したコンテキストからパラメーターの型が推論されます。具体的には、匿名関数が互換性のあるデリゲート型または式ツリー型に変換されると、その型はパラメーターの型 ([匿名関数の変換](conversions.md#anonymous-function-conversions)) を提供します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="a3946-2388">暗黙的に型指定された単一のパラメーターを持つ匿名関数では、パラメーターリストからかっこを省略できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="a3946-2389">つまり、という形式の匿名関数</span><span class="sxs-lookup"><span data-stu-id="a3946-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="a3946-2390">をに短縮できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="a3946-2391">*Anonymous_method_expression*形式の匿名関数のパラメーターリストは省略可能です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="a3946-2392">指定されている場合は、パラメーターを明示的に型指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="a3946-2393">それ以外の場合、匿名関数は `out` パラメーターを含まない任意のパラメーターリストを持つデリゲートに変換できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="a3946-2394">匿名関数の*ブロック*本体に到達可能 ([エンドポイントと到達可能性](statements.md#end-points-and-reachability)) は、匿名関数が到達できないステートメントの内部にある場合を除きます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="a3946-2395">次に、匿名関数の例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2395">Some examples of anonymous functions follow below:</span></span>

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

<span data-ttu-id="a3946-2396">*Lambda_expression*s と*anonymous_method_expression*s の動作は、次の点を除いて同じです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="a3946-2397">*anonymous_method_expression*s を使用すると、パラメーターリストを完全に省略でき、値パラメーターのリストのデリゲート型に convertibility が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="a3946-2398">*lambda_expression*s では、パラメーターの型を省略し、推論することができますが、 *anonymous_method_expression*s ではパラメーターの型を明示的に指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="a3946-2399">*Lambda_expression*の本体は、式またはステートメントブロックにすることができます。一方、 *anonymous_method_expression*の本体は、ステートメントブロックである必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="a3946-2400">互換性のある式ツリー型 ([式ツリー型](types.md#expression-tree-types)) に変換できるのは*lambda_expression*s だけです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="a3946-2401">匿名関数のシグネチャ</span><span class="sxs-lookup"><span data-stu-id="a3946-2401">Anonymous function signatures</span></span>

<span data-ttu-id="a3946-2402">匿名関数の省略可能な*anonymous_function_signature*は、匿名関数の名前と、必要に応じて、仮パラメーターの型を定義します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="a3946-2403">匿名関数のパラメーターのスコープは、 *anonymous_function_body*です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="a3946-2404">([スコープ](basic-concepts.md#scopes))パラメーターリストと共に (指定されている場合)、匿名メソッド本体は宣言空間 ([宣言](basic-concepts.md#declarations)) を構成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="a3946-2405">したがって、匿名関数のパラメーター名がローカル変数の名前、ローカル定数、またはスコープに*anonymous_method_expression*または*lambda_expression*が含まれている場合、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="a3946-2406">匿名関数に*explicit_anonymous_function_signature*がある場合、互換性のあるデリゲート型と式ツリー型のセットは、同じ順序で同じパラメーターの型と修飾子を持つものに制限されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="a3946-2407">メソッドグループの変換 ([メソッドグループの変換](conversions.md#method-group-conversions)) とは対照的に、匿名関数のパラメーター型の反変性はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="a3946-2408">匿名関数に*anonymous_function_signature*がない場合、互換性のあるデリゲート型と式ツリー型のセットは、`out` パラメーターを持たない型に制限されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="a3946-2409">*Anonymous_function_signature*には、属性またはパラメーター配列を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="a3946-2410">ただし、パラメーターリストにパラメーター配列が含まれているデリゲート型との互換性がある*anonymous_function_signature* 。</span><span class="sxs-lookup"><span data-stu-id="a3946-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="a3946-2411">互換性がある場合でも、式ツリー型への変換は、コンパイル時 ([式ツリー型](types.md#expression-tree-types)) で失敗する可能性があることにも注意してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="a3946-2412">匿名関数本体</span><span class="sxs-lookup"><span data-stu-id="a3946-2412">Anonymous function bodies</span></span>

<span data-ttu-id="a3946-2413">匿名関数の本体 (*式*または*ブロック*) には、次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="a3946-2414">匿名関数に署名が含まれている場合は、署名に指定されているパラメーターを本文で使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="a3946-2415">匿名関数にシグネチャがない場合は、パラメーターを持つデリゲート型または式の型に変換できます ([匿名関数の変換](conversions.md#anonymous-function-conversions))。ただし、本文内のパラメーターにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="a3946-2416">最も近い外側にある匿名関数のシグネチャ (存在する場合) に指定された `ref` パラメーターまたは `out` パラメーターを除き、本文が `ref` または `out` パラメーターにアクセスする場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="a3946-2417">`this` の型が構造体型の場合、本文が `this`にアクセスするためのコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="a3946-2418">これは、アクセスが明示的 (`this.x`) であるか暗黙である (`x` のように、`x` が構造体のインスタンスメンバーである) かどうかによっても当てはまります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="a3946-2419">この規則は、このようなアクセスを禁止するだけで、メンバーの参照結果が構造体のメンバーであるかどうかには影響しません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="a3946-2420">本文は、匿名関数の外部変数 ([外部変数](expressions.md#outer-variables)) にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="a3946-2421">外部変数にアクセスすると、 *lambda_expression*または*anonymous_method_expression*の評価時 ([匿名関数式の評価](expressions.md#evaluation-of-anonymous-function-expressions)) にアクティブになっている変数のインスタンスが参照されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="a3946-2422">本文に含まれている `goto` ステートメント、`break` ステートメント、または `continue` ステートメントが本体の外部または含まれている匿名関数の本体内に含まれている場合は、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="a3946-2423">本体の `return` ステートメントは、外側の関数メンバーからではなく、最も近い外側の匿名関数の呼び出しから制御を返します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="a3946-2424">`return` ステートメントで指定された式は、最も近い外側の*lambda_expression*または*anonymous_method_expression*が変換されるデリゲート型または式ツリー型の戻り値の型に暗黙的に変換可能である必要があります ([匿名関数の変換](conversions.md#anonymous-function-conversions))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="a3946-2425">*Lambda_expression*または*anonymous_method_expression*の評価と呼び出し以外で匿名関数のブロックを実行する方法があるかどうかは、明示的に指定されていません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="a3946-2426">特に、コンパイラは、1つまたは複数の名前付きメソッドまたは型をからことによって、匿名関数を実装することができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="a3946-2427">このような合成された要素の名前は、コンパイラ用に予約された形式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="a3946-2428">オーバーロードの解決と匿名関数</span><span class="sxs-lookup"><span data-stu-id="a3946-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="a3946-2429">引数リスト内の匿名関数は、型の推定とオーバーロードの解決に関与します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="a3946-2430">厳密なルールについては、 [「型の推定](expressions.md#type-inference)と[オーバーロードの解決](expressions.md#overload-resolution)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="a3946-2431">次の例は、オーバーロードの解決での匿名関数の効果を示しています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

<span data-ttu-id="a3946-2432">`ItemList<T>` クラスには、2つの `Sum` メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="a3946-2433">各は `selector` 引数を受け取ります。これにより、リスト項目から合計値が抽出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="a3946-2434">抽出された値には、`int` または `double` のいずれかを指定できます。また、結果として得られる合計は、`int` または `double`のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="a3946-2435">たとえば、`Sum` メソッドを使用して、詳細行のリストから注文の合計を計算できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

<span data-ttu-id="a3946-2436">`orderDetails.Sum`の最初の呼び出しでは、匿名関数 `d => d. UnitCount` が `Func<Detail,int>` と `Func<Detail,double>`の両方と互換性があるため、両方の `Sum` メソッドが適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="a3946-2437">ただし、オーバーロードの解決では、最初の `Sum` メソッドが選択されます。これは、`Func<Detail,int>` への変換が `Func<Detail,double>`への変換よりも優れているためです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="a3946-2438">`orderDetails.Sum`の2番目の呼び出しでは、匿名関数 `d => d.UnitPrice * d.UnitCount` が `double`型の値を生成するため、2番目の `Sum` メソッドのみが適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="a3946-2439">したがって、オーバーロードの解決は、その呼び出しの2つ目の `Sum` メソッドを選択します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="a3946-2440">匿名関数と動的バインド</span><span class="sxs-lookup"><span data-stu-id="a3946-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="a3946-2441">匿名関数は、動的にバインドされた操作のレシーバー、引数、またはオペランドにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="a3946-2442">外部変数</span><span class="sxs-lookup"><span data-stu-id="a3946-2442">Outer variables</span></span>

<span data-ttu-id="a3946-2443">スコープに*lambda_expression*または*anonymous_method_expression*が含まれているすべてのローカル変数、値パラメーター、またはパラメーター配列は、匿名関数の***外部変数***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="a3946-2444">クラスのインスタンス関数メンバーでは、`this` 値は値パラメーターと見なされ、関数メンバー内に含まれるすべての匿名関数の外部変数になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="a3946-2445">キャプチャされた外部変数</span><span class="sxs-lookup"><span data-stu-id="a3946-2445">Captured outer variables</span></span>

<span data-ttu-id="a3946-2446">外部変数が匿名関数によって参照されている場合、外部変数は匿名関数によって***キャプチャ***されたと言います。</span><span class="sxs-lookup"><span data-stu-id="a3946-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="a3946-2447">通常、ローカル変数の有効期間は、関連付けられているブロックまたはステートメント ([ローカル変数](variables.md#local-variables)) の実行に制限されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="a3946-2448">ただし、取得した外部変数の有効期間は、匿名関数から作成されたデリゲートまたは式ツリーがガベージコレクションの対象になるまで、少なくとも拡張されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="a3946-2449">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-2449">In the example</span></span>
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
<span data-ttu-id="a3946-2450">ローカル変数 `x` は匿名関数によってキャプチャされ、`F` から返されたデリゲートがガベージコレクションの対象になるまで、`x` の有効期間が少なくとも拡張されます (これは、プログラムの終わりまで発生しません)。</span><span class="sxs-lookup"><span data-stu-id="a3946-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="a3946-2451">匿名関数の各呼び出しは `x`の同じインスタンスで動作するため、この例の出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```console
1
2
3
```

<span data-ttu-id="a3946-2452">ローカル変数または値パラメーターが匿名関数によってキャプチャされると、ローカル変数またはパラメーターは固定変数 ([固定変数および](unsafe-code.md#fixed-and-moveable-variables)移動可能変数) とは見なされなくなりますが、代わりに移動可能な変数と見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="a3946-2453">したがって、キャプチャされた外部変数のアドレスを取得する `unsafe` コードでは、まず `fixed` ステートメントを使用して変数を修正する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="a3946-2454">Uncaptured 変数とは異なり、キャプチャされたローカル変数は複数の実行スレッドに同時に公開できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="a3946-2455">ローカル変数のインスタンス化</span><span class="sxs-lookup"><span data-stu-id="a3946-2455">Instantiation of local variables</span></span>

<span data-ttu-id="a3946-2456">ローカル変数は、実行時に変数のスコープに入ると、***インスタンス化***されると見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="a3946-2457">たとえば、次のメソッドが呼び出されると、ローカル変数 `x` がインスタンス化され、ループの反復ごとに3回初期化されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="a3946-2458">ただし、`x` の宣言をループの外側に移動すると、`x`の1つのインスタンスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="a3946-2459">キャプチャされていない場合、ローカル変数がインスタンス化される頻度を正確に観察する方法はありません。インスタンス化の有効期間が不整合になるため、各インスタンス化で同じストレージの場所を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="a3946-2460">ただし、匿名関数がローカル変数をキャプチャすると、インスタンス化の効果が明らかになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="a3946-2461">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2461">The example</span></span>
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
<span data-ttu-id="a3946-2462">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2462">produces the output:</span></span>
```console
1
3
5
```

<span data-ttu-id="a3946-2463">ただし、`x` の宣言をループの外側に移動した場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
<span data-ttu-id="a3946-2464">出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2464">the output is:</span></span>
```console
5
5
5
```

<span data-ttu-id="a3946-2465">For ループが反復変数を宣言している場合、その変数自体がループの外側で宣言されていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="a3946-2466">したがって、反復変数自体をキャプチャするように例が変更された場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="a3946-2467">次のように出力を生成する反復変数のインスタンスは1つだけキャプチャされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```console
3
3
3
```

<span data-ttu-id="a3946-2468">匿名関数デリゲートは、キャプチャされたいくつかの変数を共有し、他の変数のインスタンスを個別に持つことができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="a3946-2469">たとえば、`F` がに変更された場合、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2469">For example, if `F` is changed to</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
<span data-ttu-id="a3946-2470">この3つのデリゲートは、`x` の同じインスタンスをキャプチャし、`y`のインスタンスを分離します。出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```console
1 1
2 1
3 1
```

<span data-ttu-id="a3946-2471">個別の匿名関数は、外部変数の同じインスタンスをキャプチャできます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="a3946-2472">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2472">In the example:</span></span>
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
<span data-ttu-id="a3946-2473">この2つの匿名関数は、ローカル変数 `x`の同じインスタンスをキャプチャし、その変数を使用して "通信" できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="a3946-2474">この例の出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2474">The output of the example is:</span></span>
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="a3946-2475">匿名関数式の評価</span><span class="sxs-lookup"><span data-stu-id="a3946-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="a3946-2476">匿名関数 `F` は、直接的に、またはデリゲート作成式 `new D(F)`の実行を通じて、デリゲート型 `D` または式ツリー `E`型に常に変換される必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="a3946-2477">この変換は、「[匿名関数の変換](conversions.md#anonymous-function-conversions)」で説明されているように、匿名関数の結果を決定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="a3946-2478">クエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2478">Query expressions</span></span>

<span data-ttu-id="a3946-2479">***クエリ式***には、SQL や XQuery などのリレーショナルクエリ言語と階層クエリ言語に似たクエリの言語統合構文が用意されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

<span data-ttu-id="a3946-2480">クエリ式が `from` 句で始まり、`select` または `group` 句のいずれかで終わります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="a3946-2481">最初の `from` 句の後には、0個以上の `from`、`let`、`where`、`join` または `orderby` 句を指定できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="a3946-2482">各 `from` 句は、***シーケンス***の要素を範囲とする***範囲変数***を導入するジェネレーターです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="a3946-2483">各 `let` 句には、前の範囲変数によって計算された値を表す範囲変数が導入されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="a3946-2484">各 `where` 句は、結果から項目を除外するフィルターです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="a3946-2485">各 `join` 句は、ソースシーケンスの指定されたキーを別のシーケンスのキーと比較し、一致するペアを生成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="a3946-2486">各 `orderby` 句は、指定された条件に従って項目を並べ替えます。最後の `select` 句または `group` 句は、範囲変数の観点から結果の形状を指定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="a3946-2487">最後に、1つのクエリの結果を後続のクエリでジェネレーターとして扱うことによって、`into` 句を使用してクエリを "スプライス" できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="a3946-2488">クエリ式のあいまいさ</span><span class="sxs-lookup"><span data-stu-id="a3946-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="a3946-2489">クエリ式には、さまざまな "コンテキストキーワード" が含まれています。つまり、特定のコンテキストで特別な意味を持つ識別子です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="a3946-2490">具体的には、`from`、`where`、`join`、`on`、`equals`、`into`、`let`、`orderby`、`ascending`、`descending`、`select`、`group`、`by`です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="a3946-2491">これらの識別子をキーワードまたは単純名として使用することによるクエリ式のあいまいさを回避するために、これらの識別子は、クエリ式内の任意の場所で出現するときにキーワードと見なされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="a3946-2492">このため、クエリ式は、"`from identifier`" で始まり、"`;`"、"`=`"、または "`,`" 以外の任意のトークンで始まる任意の式です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="a3946-2493">これらの単語をクエリ式内で識別子として使用するために、プレフィックスとして "`@`" ([識別子](lexical-structure.md#identifiers)) を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="a3946-2494">クエリ式の変換</span><span class="sxs-lookup"><span data-stu-id="a3946-2494">Query expression translation</span></span>

<span data-ttu-id="a3946-2495">このC#言語では、クエリ式の実行セマンティクスが指定されていません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="a3946-2496">代わりに、クエリ式は、クエリ*式パターン*([クエリ式パターン](expressions.md#the-query-expression-pattern)) に準拠するメソッドの呼び出しに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="a3946-2497">具体的には、クエリ式は、`Where`、`Select`、`SelectMany`、`Join`、`GroupJoin`、`OrderBy`、`OrderByDescending`、`ThenBy`、`ThenByDescending`、`GroupBy`、`Cast`という名前のメソッドの呼び出しに変換されます。これらのメソッドには、[クエリ式パターン](expressions.md#the-query-expression-pattern)で説明されているように、特定のシグネチャと結果型が必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="a3946-2498">これらのメソッドは、クエリを実行するオブジェクトのインスタンスメソッド、またはオブジェクトの外部にある拡張メソッドにすることができ、クエリの実際の実行を実装します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="a3946-2499">クエリ式からメソッド呼び出しへの変換は、型のバインディングまたはオーバーロードの解決が行われる前に行われる構文のマッピングです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="a3946-2500">変換は構文的に正しいことが保証されていますが、意味のC#ある正しいコードを生成することは保証されていません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="a3946-2501">クエリ式の変換に続くと、結果として得られるメソッド呼び出しは通常のメソッド呼び出しとして処理され、メソッドが存在しない場合、引数の型が正しくない場合、またはメソッドがジェネリックで、型の推定は失敗します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="a3946-2502">クエリ式は、さらに縮小することができなくなるまで、次の変換を繰り返し適用することで処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="a3946-2503">翻訳はアプリケーション順に一覧表示されます。各セクションでは、前のセクションで説明した翻訳が徹底的に実行されたことを前提としています。また、1つのセクションは、後で同じクエリ式の処理中に再検討されることはありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="a3946-2504">範囲変数への代入は、クエリ式では許可されていません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="a3946-2505">ただし、 C#実装では、この制限を常に適用しないようにすることができます。これは、ここで説明する構文変換スキームでは不可能な場合があるためです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="a3946-2506">特定の翻訳では、`*`によって示される透過的な識別子を使用して範囲変数が挿入されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="a3946-2507">透過的な識別子の特別なプロパティについては、「[透過的識別子](expressions.md#transparent-identifiers)」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="a3946-2508">継続を含む Select および groupby 句</span><span class="sxs-lookup"><span data-stu-id="a3946-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="a3946-2509">継続を含むクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="a3946-2510">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="a3946-2511">以降のセクションの変換では、クエリに `into` 継続がないことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="a3946-2512">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="a3946-2513">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="a3946-2514">最終的な変換は、</span><span class="sxs-lookup"><span data-stu-id="a3946-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="a3946-2515">明示的な範囲変数の型</span><span class="sxs-lookup"><span data-stu-id="a3946-2515">Explicit range variable types</span></span>

<span data-ttu-id="a3946-2516">範囲変数の型を明示的に指定する `from` 句</span><span class="sxs-lookup"><span data-stu-id="a3946-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="a3946-2517">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="a3946-2518">範囲変数の型を明示的に指定する `join` 句</span><span class="sxs-lookup"><span data-stu-id="a3946-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```csharp
join T x in e on k1 equals k2
```
<span data-ttu-id="a3946-2519">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2519">is translated into</span></span>
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="a3946-2520">以降のセクションの変換では、クエリに明示的な範囲変数の型がないと想定しています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="a3946-2521">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="a3946-2522">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="a3946-2523">最終的な変換は、</span><span class="sxs-lookup"><span data-stu-id="a3946-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="a3946-2524">明示的な範囲変数型は、非ジェネリック `IEnumerable` インターフェイスを実装するコレクションに対してクエリを実行する場合に便利ですが、ジェネリック `IEnumerable<T>` インターフェイスを実装することはできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="a3946-2525">上記の例では、`customers` が `ArrayList`型である場合、これが当てはまります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="a3946-2526">逆クエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2526">Degenerate query expressions</span></span>

<span data-ttu-id="a3946-2527">フォームのクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="a3946-2528">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="a3946-2529">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="a3946-2530">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="a3946-2531">逆クエリ式とは、ソースの要素を普通に選択したものです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="a3946-2532">変換の後の段階では、他の変換ステップによって導入された逆のクエリをソースに置き換えることによって削除します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="a3946-2533">ただし、クエリ式の結果がソースオブジェクト自体にならないようにすることが重要です。これにより、クエリのクライアントに対するソースの型と id が明らかになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="a3946-2534">したがって、この手順では、ソースで `Select` を明示的に呼び出すことによって、ソースコードに直接書き込まれた低次元クエリを保護します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="a3946-2535">その後、これらのメソッドがソースオブジェクト自体を返さないようにするために、`Select` およびその他のクエリ演算子を実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="a3946-2536">From、let、where、join、orderby 句</span><span class="sxs-lookup"><span data-stu-id="a3946-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="a3946-2537">2番目の `from` 句の後に `select` 句を指定したクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="a3946-2538">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="a3946-2539">2番目の `from` 句の後に `select` 句以外のものが続くクエリ式:</span><span class="sxs-lookup"><span data-stu-id="a3946-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="a3946-2540">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="a3946-2541">`let` 句を含むクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="a3946-2542">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="a3946-2543">`where` 句を含むクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="a3946-2544">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="a3946-2545">`into` の後に `select` 句が指定されていない `join` 句を含むクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="a3946-2546">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="a3946-2547">`into` の後に `select` 句以外のものが指定されていない `join` 句を含むクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="a3946-2548">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="a3946-2549">`into` の後に `select` 句が続く `join` 句を含むクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="a3946-2550">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="a3946-2551">`into` の後に `select` 句以外のものが含まれている `join` 句を含むクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="a3946-2552">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="a3946-2553">`orderby` 句を含むクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="a3946-2554">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="a3946-2555">順序句で `descending` 方向インジケーターが指定されている場合は、代わりに `OrderByDescending` または `ThenByDescending` の呼び出しが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="a3946-2556">次の変換では、`let`、`where`、`join`、または `orderby` 句がなく、各クエリ式に1つの初期 `from` 句が含まれていないことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="a3946-2557">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="a3946-2558">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="a3946-2559">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="a3946-2560">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="a3946-2561">最終的な変換は、</span><span class="sxs-lookup"><span data-stu-id="a3946-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="a3946-2562">ここで `x` はコンパイラによって生成される識別子であり、それ以外は非表示でアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="a3946-2563">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="a3946-2564">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="a3946-2565">最終的な変換は、</span><span class="sxs-lookup"><span data-stu-id="a3946-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="a3946-2566">ここで `x` はコンパイラによって生成される識別子であり、それ以外は非表示でアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="a3946-2567">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="a3946-2568">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="a3946-2569">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="a3946-2570">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="a3946-2571">最終的な変換は、</span><span class="sxs-lookup"><span data-stu-id="a3946-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="a3946-2572">`x` と `y` は、非表示でアクセスできない、コンパイラによって生成される識別子です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="a3946-2573">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="a3946-2574">最終翻訳</span><span class="sxs-lookup"><span data-stu-id="a3946-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="a3946-2575">Select 句</span><span class="sxs-lookup"><span data-stu-id="a3946-2575">Select clauses</span></span>

<span data-ttu-id="a3946-2576">フォームのクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="a3946-2577">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="a3946-2578">v が識別子 x の場合を除き、変換は単純です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="a3946-2579">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="a3946-2580">は単にに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="a3946-2581">Groupby 句</span><span class="sxs-lookup"><span data-stu-id="a3946-2581">Groupby clauses</span></span>

<span data-ttu-id="a3946-2582">フォームのクエリ式</span><span class="sxs-lookup"><span data-stu-id="a3946-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="a3946-2583">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="a3946-2584">v が識別子 x である場合を除き、変換は</span><span class="sxs-lookup"><span data-stu-id="a3946-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="a3946-2585">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="a3946-2586">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="a3946-2587">透過的な識別子</span><span class="sxs-lookup"><span data-stu-id="a3946-2587">Transparent identifiers</span></span>

<span data-ttu-id="a3946-2588">特定の翻訳では、`*`によって示される***透過的な識別子***を使用して範囲変数が挿入されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="a3946-2589">透過的な識別子は、適切な言語機能ではありません。これらは、クエリ式の変換プロセスの中間手順としてのみ存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="a3946-2590">クエリ変換によって透過的識別子が挿入されると、その後の変換ステップによって透過的識別子が匿名関数および匿名オブジェクト初期化子に反映されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="a3946-2591">これらのコンテキストでは、透過的な識別子の動作は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="a3946-2592">匿名関数のパラメーターとして透過的識別子が発生した場合、関連付けられた匿名型のメンバーは、匿名関数の本体のスコープ内に自動的に含まれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="a3946-2593">透過的な識別子を持つメンバーがスコープ内にある場合、そのメンバーのメンバーもスコープ内にあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="a3946-2594">透過的な識別子が匿名オブジェクト初期化子のメンバー宣言子として発生すると、透過的な識別子を持つメンバーが導入されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="a3946-2595">前述の変換手順では、透過的識別子は常に匿名型と共に導入され、複数の範囲変数を1つのオブジェクトのメンバーとしてキャプチャすることを目的としています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="a3946-2596">のC#実装では、匿名型とは異なる機構を使用して複数の範囲変数をグループ化することが許可されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="a3946-2597">次の変換例では、匿名型が使用されていることを前提として、透過的な識別子をどのように変換できるかを示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="a3946-2598">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="a3946-2599">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="a3946-2600">これは、さらにに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="a3946-2601">これは、透過的な識別子が消去されると、</span><span class="sxs-lookup"><span data-stu-id="a3946-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="a3946-2602">ここで `x` はコンパイラによって生成される識別子であり、それ以外は非表示でアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="a3946-2603">例</span><span class="sxs-lookup"><span data-stu-id="a3946-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="a3946-2604">はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="a3946-2605">これは、</span><span class="sxs-lookup"><span data-stu-id="a3946-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="a3946-2606">最終的な変換は、</span><span class="sxs-lookup"><span data-stu-id="a3946-2606">the final translation of which is</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
<span data-ttu-id="a3946-2607">`x`、`y`、および `z` は、それ以外は非表示でアクセスできない、コンパイラによって生成される識別子です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="a3946-2608">クエリ式パターン</span><span class="sxs-lookup"><span data-stu-id="a3946-2608">The query expression pattern</span></span>

<span data-ttu-id="a3946-2609">***クエリ式パターン***は、型がクエリ式をサポートするために実装できるメソッドのパターンを確立します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="a3946-2610">クエリ式は構文マッピングによってメソッドの呼び出しに変換されるため、型はクエリ式パターンを実装する方法において非常に柔軟です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="a3946-2611">たとえば、パターンのメソッドは、インスタンスメソッドとして実装することも、拡張メソッドとして実装することもできます。これは、2つの呼び出し構文が同じであるためです。また、匿名関数は両方に変換可能であるため、メソッドはデリゲートまたは式ツリーを要求できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="a3946-2612">次に、クエリ式パターンをサポートするジェネリック型 `C<T>` の推奨される図形を示します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="a3946-2613">ジェネリック型は、パラメーターと結果の型の間の適切なリレーションシップを示すために使用されますが、非ジェネリック型のパターンを実装することもできます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

<span data-ttu-id="a3946-2614">上記のメソッドでは、ジェネリックデリゲート型 `Func<T1,R>` および `Func<T1,T2,R>`が使用されますが、パラメーターと結果型では、他のデリゲート型または式ツリー型を同じリレーションシップで使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="a3946-2615">`C<T>` と `O<T>` の間に推奨されるリレーションシップがあることに注意してください。これにより、`ThenBy` および `ThenByDescending` メソッドを、`OrderBy` または `OrderByDescending`の結果でのみ使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="a3946-2616">また、`GroupBy` の結果の推奨される図形に注目してください。各内部シーケンスには、追加の `Key` プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="a3946-2617">`System.Linq` 名前空間は、`System.Collections.Generic.IEnumerable<T>` インターフェイスを実装する任意の型に対するクエリ演算子パターンの実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="a3946-2618">代入演算子</span><span class="sxs-lookup"><span data-stu-id="a3946-2618">Assignment operators</span></span>

<span data-ttu-id="a3946-2619">代入演算子は、変数、プロパティ、イベント、またはインデクサー要素に新しい値を代入します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

<span data-ttu-id="a3946-2620">代入の左オペランドは、変数、プロパティアクセス、インデクサーアクセス、またはイベントアクセスとして分類される式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="a3946-2621">`=` 演算子は、***単純代入演算子***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="a3946-2622">右オペランドの値を、左オペランドによって指定された変数、プロパティ、またはインデクサー要素に代入します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="a3946-2623">単純代入演算子の左オペランドは、イベントアクセスでない可能性があります (「[フィールドに似たイベント](classes.md#field-like-events)」で説明されている場合を除く)。</span><span class="sxs-lookup"><span data-stu-id="a3946-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="a3946-2624">単純な代入演算子については、「[単純な代入](expressions.md#simple-assignment)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="a3946-2625">`=` 演算子以外の代入演算子は、***複合代入演算子***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="a3946-2626">これらの演算子は、2つのオペランドに対して指定された演算を実行し、結果の値を、左オペランドによって指定された変数、プロパティ、またはインデクサー要素に代入します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="a3946-2627">複合代入演算子の詳細については、「[複合代入](expressions.md#compound-assignment)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="a3946-2628">左オペランドとしてイベントアクセス式を持つ `+=` 演算子と `-=` 演算子は、*イベント代入演算子*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="a3946-2629">左オペランドとしてイベントアクセスが有効な他の代入演算子はありません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="a3946-2630">イベント代入演算子の詳細については、「[イベントの割り当て](expressions.md#event-assignment)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a3946-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="a3946-2631">代入演算子は、右から左にグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="a3946-2632">たとえば、`a = b = c` フォームの式は `a = (b = c)`として評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="a3946-2633">単純代入</span><span class="sxs-lookup"><span data-stu-id="a3946-2633">Simple assignment</span></span>

<span data-ttu-id="a3946-2634">`=` 演算子は、単純代入演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="a3946-2635">単純な代入の左側のオペランドが `E.P` または `E[Ei]` の形式であり、`E` にコンパイル時の型 `dynamic`がある場合、割り当ては動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a3946-2636">この場合、代入式のコンパイル時の型は `dynamic`であり、以下に説明する解決方法は、`E`の実行時の型に基づいて実行時に行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="a3946-2637">単純な代入では、右オペランドは左オペランドの型に暗黙的に変換できる式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="a3946-2638">操作は、左オペランドによって指定された変数、プロパティ、またはインデクサー要素に右オペランドの値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="a3946-2639">単純な代入式の結果は、左側のオペランドに割り当てられた値になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="a3946-2640">結果は左オペランドと同じ型であり、常に値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="a3946-2641">左オペランドがプロパティまたはインデクサーアクセスの場合、プロパティまたはインデクサーには `set` アクセサーが必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="a3946-2642">そうでない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-2643">フォーム `x = y` の単純な割り当ての実行時処理は、次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="a3946-2644">`x` が変数として分類される場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="a3946-2645">変数を生成するために `x` が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="a3946-2646">`y` が評価され、必要に応じて、暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) によって `x` の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="a3946-2647">`x` によって指定された変数が*reference_type*の配列要素である場合は、`y` に対して計算された値が、`x` が要素である配列インスタンスと互換性があることを確認するために、実行時チェックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="a3946-2648">`y` が `null`場合、または `y` によって参照されるインスタンスの実際の型から `x`を含む配列インスタンスの実際の要素型への暗黙の参照変換 ([暗黙的な参照](conversions.md#implicit-reference-conversions)変換) が存在する場合、チェックは成功します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="a3946-2649">それ以外の場合は、`System.ArrayTypeMismatchException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="a3946-2650">`y` の評価および変換の結果として得られる値は、`x`の評価によって指定された場所に格納されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="a3946-2651">`x` がプロパティまたはインデクサーアクセスとして分類される場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="a3946-2652">インスタンス式 (`x` が `static`でない場合) と `x` に関連付けられている引数リスト (`x` がインデクサーアクセスの場合) が評価され、その結果が後続の `set` アクセサー呼び出しで使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="a3946-2653">`y` が評価され、必要に応じて、暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) によって `x` の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="a3946-2654">`x` の `set` アクセサーは、`value` 引数として `y` に対して計算された値を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="a3946-2655">配列のクロス分散規則 ([配列の共変性](arrays.md#array-covariance)) では、配列型の値を `A[]` 配列型のインスタンスへの参照にすることが許可されています。これは、`B` から `A`への暗黙的な参照変換が存在する場合に `B[]`です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="a3946-2656">これらのルールにより、 *reference_type*の配列要素への代入では、割り当てられている値が配列インスタンスと互換性があることを確認するためにランタイムチェックが必要になります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="a3946-2657">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="a3946-2658">`ArrayList` のインスタンスを `string[]`の要素に格納することはできないため、最後の割り当てによって `System.ArrayTypeMismatchException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="a3946-2659">*Struct_type*で宣言されたプロパティまたはインデクサーが割り当てのターゲットである場合は、プロパティまたはインデクサーアクセスに関連付けられているインスタンス式を変数として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="a3946-2660">インスタンス式が値として分類されている場合、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="a3946-2661">[メンバーアクセス](expressions.md#member-access)により、同じ規則がフィールドにも適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="a3946-2662">次の宣言を指定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2662">Given the declarations:</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
<span data-ttu-id="a3946-2663">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="a3946-2664">`p.X`、`p.Y`、`r.A`、および `r.B` への割り当ては、`p` と `r` が変数であるため、許可されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="a3946-2665">ただし、この例では</span><span class="sxs-lookup"><span data-stu-id="a3946-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="a3946-2666">`r.A` と `r.B` は変数ではないため、割り当てはすべて無効です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="a3946-2667">複合代入。</span><span class="sxs-lookup"><span data-stu-id="a3946-2667">Compound assignment</span></span>

<span data-ttu-id="a3946-2668">複合代入の左オペランドが `E.P` または `E[Ei]` の形式で、`E` がコンパイル時の型 `dynamic`を持つ場合、割り当ては動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a3946-2669">この場合、代入式のコンパイル時の型は `dynamic`であり、以下に説明する解決方法は、`E`の実行時の型に基づいて実行時に行われます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="a3946-2670">`x op= y` フォームの操作は、演算が `x op y`書き込まれたかのように、二項演算子のオーバーロードの解決 ([二項演算子のオーバーロードの解決](expressions.md#binary-operator-overload-resolution)) を適用することによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="a3946-2671">そうしたら</span><span class="sxs-lookup"><span data-stu-id="a3946-2671">Then,</span></span>

*  <span data-ttu-id="a3946-2672">選択した演算子の戻り値の型が `x`の型に暗黙的に変換可能な場合、`x` が1回だけ評価される点を除いて、操作は `x = x op y`として評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="a3946-2673">また、選択した演算子が定義済みの演算子である場合、選択した演算子の戻り値の型が `x`の型に明示的に変換可能で、`y` が `x` の型に暗黙的に変換可能であるか、または演算子がシフト演算子の場合は、`T` が1回だけ評価される点を除い `x = (T)(x op y)`て、`x`は `x` の型です</span><span class="sxs-lookup"><span data-stu-id="a3946-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="a3946-2674">それ以外の場合、複合代入は無効であり、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-2675">"評価された1回のみ" という用語は、`x op y`の評価では、`x` の構成式の結果が一時的に保存され、`x`への割り当てを実行するときに再利用されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="a3946-2676">`A()[B()] += C()`たとえば、`A` が `int[]`を返すメソッドであり、`B` および `C` が `int`を返すメソッドである場合、メソッドは、順序 `A`、`B`、`C`で一度だけ呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="a3946-2677">複合代入の左オペランドがプロパティアクセスまたはインデクサーアクセスの場合、プロパティまたはインデクサーには `get` アクセサーと `set` アクセサーの両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="a3946-2678">そうでない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-2679">上記の2番目のルールでは、`x op= y` を特定のコンテキストで `x = (T)(x op y)` として評価することを許可します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="a3946-2680">この規則は、左側のオペランドの型が `sbyte`、`byte`、`short`、`ushort`、または `char`の場合に、定義済みの演算子を複合演算子として使用できるようにするために存在します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="a3946-2681">両方の引数がこれらの型のいずれかであっても、定義済みの演算子は、「[バイナリ数値の上位](expressions.md#binary-numeric-promotions)変換」で説明されているように `int`型の結果を生成します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="a3946-2682">したがって、キャストがないと、結果を左オペランドに代入できません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="a3946-2683">定義済みの演算子のルールの直感的な効果は、`x op y` と `x = y` の両方が許可されている場合に `x op= y` が許可されることです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="a3946-2684">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-2684">In the example</span></span>
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
<span data-ttu-id="a3946-2685">各エラーの直感的な理由は、対応する単純な割り当てもエラーになることです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="a3946-2686">これは、複合代入演算でリフト演算がサポートされていることも意味します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="a3946-2687">この例では、</span><span class="sxs-lookup"><span data-stu-id="a3946-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="a3946-2688">リフトされた演算子 `+(int?,int?)` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="a3946-2689">イベントの割り当て</span><span class="sxs-lookup"><span data-stu-id="a3946-2689">Event assignment</span></span>

<span data-ttu-id="a3946-2690">`+=` または `-=` 演算子の左オペランドがイベントアクセスとして分類されている場合、式は次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="a3946-2691">イベントアクセスのインスタンス式 (存在する場合) が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="a3946-2692">`+=` または `-=` 演算子の右オペランドが評価され、必要に応じて、暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) によって左オペランドの型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="a3946-2693">イベントのイベントアクセサーが呼び出されます。引数リストは右オペランド、評価後、必要に応じて変換されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="a3946-2694">演算子が `+=`場合は、`add` アクセサーが呼び出されます。演算子が `-=`されている場合は、`remove` アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="a3946-2695">イベントの割り当て式では、値は生成されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="a3946-2696">したがって、イベントの割り当て式は、 *statement_expression* ([式ステートメント](statements.md#expression-statements)) のコンテキストでのみ有効です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="a3946-2697">式</span><span class="sxs-lookup"><span data-stu-id="a3946-2697">Expression</span></span>

<span data-ttu-id="a3946-2698">*式*は、 *non_assignment_expression*または*代入*のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="a3946-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a><span data-ttu-id="a3946-2699">定数式</span><span class="sxs-lookup"><span data-stu-id="a3946-2699">Constant expressions</span></span>

<span data-ttu-id="a3946-2700">*Constant_expression*は、コンパイル時に完全に評価できる式です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="a3946-2701">定数式は、`null` リテラル、または次のいずれかの型の値である必要があります: `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、または任意の列挙型。`decimal``bool``object``string`</span><span class="sxs-lookup"><span data-stu-id="a3946-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="a3946-2702">定数式では、次のコンストラクトのみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="a3946-2703">リテラル (`null` リテラルを含む)。</span><span class="sxs-lookup"><span data-stu-id="a3946-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="a3946-2704">クラス型および構造体型の `const` メンバーへの参照。</span><span class="sxs-lookup"><span data-stu-id="a3946-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="a3946-2705">列挙型のメンバーへの参照。</span><span class="sxs-lookup"><span data-stu-id="a3946-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="a3946-2706">`const` パラメーターまたはローカル変数への参照</span><span class="sxs-lookup"><span data-stu-id="a3946-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="a3946-2707">かっこで囲まれたサブ式は、それ自体が定数式です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="a3946-2708">キャスト式は、ターゲット型が上記のいずれかの型である場合に指定します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="a3946-2709">`checked` と `unchecked` 式</span><span class="sxs-lookup"><span data-stu-id="a3946-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="a3946-2710">既定値の式</span><span class="sxs-lookup"><span data-stu-id="a3946-2710">Default value expressions</span></span>
*  <span data-ttu-id="a3946-2711">すべてのの表記</span><span class="sxs-lookup"><span data-stu-id="a3946-2711">Nameof expressions</span></span>
*  <span data-ttu-id="a3946-2712">定義済みの `+`、`-`、`!`、および `~` 単項演算子。</span><span class="sxs-lookup"><span data-stu-id="a3946-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="a3946-2713">定義済みの `+`、`-`、`*`、`/`、`%`、`<<`、`>>`、`&`、`|`、`^`、`&&`、`||`、`==`、`!=`、`<`、`>`、`<=`、`>=` の各オペランドが上記の型である場合は、二項演算子が定義されています。</span><span class="sxs-lookup"><span data-stu-id="a3946-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="a3946-2714">`?:` 条件演算子。</span><span class="sxs-lookup"><span data-stu-id="a3946-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="a3946-2715">定数式では、次の変換が許可されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="a3946-2716">Id 変換</span><span class="sxs-lookup"><span data-stu-id="a3946-2716">Identity conversions</span></span>
*  <span data-ttu-id="a3946-2717">数値変換</span><span class="sxs-lookup"><span data-stu-id="a3946-2717">Numeric conversions</span></span>
*  <span data-ttu-id="a3946-2718">列挙型変換</span><span class="sxs-lookup"><span data-stu-id="a3946-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="a3946-2719">定数式の変換</span><span class="sxs-lookup"><span data-stu-id="a3946-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="a3946-2720">変換元が null 値に評価される定数式である場合、暗黙的および明示的な参照変換。</span><span class="sxs-lookup"><span data-stu-id="a3946-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="a3946-2721">非 null 値のボックス化、ボックス化解除、および暗黙的な参照変換を含むその他の変換は、定数式では許可されません。</span><span class="sxs-lookup"><span data-stu-id="a3946-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="a3946-2722">例 :</span><span class="sxs-lookup"><span data-stu-id="a3946-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="a3946-2723">ボックス化変換が必要なため、i の初期化はエラーになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="a3946-2724">Null 以外の値からの暗黙的な参照変換が必要であるため、str の初期化はエラーになります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="a3946-2725">式が上記の要件を満たしている場合は、コンパイル時に式が評価されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="a3946-2726">これは、式が定数以外の構造を含む大きな式のサブ式である場合にも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="a3946-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="a3946-2727">定数式のコンパイル時の評価では、非定数式の実行時の評価と同じ規則が使用されます。ただし、ランタイム評価で例外がスローされた場合は、コンパイル時の評価によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="a3946-2728">定数式が `unchecked` コンテキストに明示的に配置されていない限り、式のコンパイル時の評価時に、整数型の算術演算および変換で発生したオーバーフローは、常にコンパイル時エラー ([定数式](expressions.md#constant-expressions)) を発生させます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="a3946-2729">定数式は、以下に示すコンテキストで発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="a3946-2730">これらのコンテキストでは、コンパイル時に式を完全に評価できない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="a3946-2731">定数宣言 ([定数](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="a3946-2732">列挙メンバー宣言 ([列挙型メンバー](enums.md#enum-members))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="a3946-2733">仮パラメーターリストの既定の引数 ([メソッドパラメーター](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="a3946-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="a3946-2734">`switch` ステートメント ([switch ステートメント](statements.md#the-switch-statement)) のラベルを `case` します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="a3946-2735">`goto case` ステートメント ([goto ステートメント](statements.md#the-goto-statement))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="a3946-2736">初期化子を含む配列作成式 ([配列作成](expressions.md#array-creation-expressions)式) の次元の長さ。</span><span class="sxs-lookup"><span data-stu-id="a3946-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="a3946-2737">属性 ([属性](attributes.md))。</span><span class="sxs-lookup"><span data-stu-id="a3946-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="a3946-2738">暗黙の定数式の変換 ([暗黙の定数](conversions.md#implicit-constant-expression-conversions)式の変換) を使用すると、定数式の値が変換先の型の範囲内にある場合に、`int` 型の定数式を `sbyte`、`byte`、`short`、`ushort`、`uint`、または `ulong`に変換できます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="a3946-2739">ブール式</span><span class="sxs-lookup"><span data-stu-id="a3946-2739">Boolean expressions</span></span>

<span data-ttu-id="a3946-2740">*Boolean_expression*は `bool`型の結果を生成する式です。次のように指定されている特定のコンテキストで、直接または `operator true` のアプリケーションによって実行します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="a3946-2741">*If_statement* ([if ステートメント](statements.md#the-if-statement))、 *while_statement* ([while ステートメント](statements.md#the-while-statement))、 *do_statement* ([do ステートメント](statements.md#the-do-statement))、または*for_statement* ([for ステートメント](statements.md#the-for-statement)) の制御条件式は、 *boolean_expression*です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="a3946-2742">`?:` 演算子 ([条件演算子](expressions.md#conditional-operator)) の制御条件式は、 *boolean_expression*と同じ規則に従いますが、演算子の優先順位の理由は*conditional_or_expression*として分類されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="a3946-2743">次のように `bool`型の値を生成できるようにするには、 *boolean_expression* `E` が必要です。</span><span class="sxs-lookup"><span data-stu-id="a3946-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="a3946-2744">`E` が暗黙的 `bool` に変換可能な場合は、実行時に暗黙的な変換が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a3946-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="a3946-2745">それ以外の場合は、単項演算子のオーバーロードの解決 ([単項演算子のオーバーロードの解決](expressions.md#unary-operator-overload-resolution)) を使用して、`E`での演算子 `true` の一意の最適な実装を検索し、その実装を実行時に適用します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="a3946-2746">そのような演算子が見つからない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="a3946-2747">[データベースブール型](structs.md#database-boolean-type)の `DBBool` 構造体型は、`operator true` と `operator false`を実装する型の例を提供します。</span><span class="sxs-lookup"><span data-stu-id="a3946-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
