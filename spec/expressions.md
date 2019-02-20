# <a name="expressions"></a><span data-ttu-id="60db1-101">式</span><span class="sxs-lookup"><span data-stu-id="60db1-101">Expressions</span></span>

<span data-ttu-id="60db1-102">式は、演算子とオペランドのシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="60db1-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="60db1-103">この章では、構文、オペランドと演算子の評価と式の意味の順序を定義します。</span><span class="sxs-lookup"><span data-stu-id="60db1-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="60db1-104">式の分類</span><span class="sxs-lookup"><span data-stu-id="60db1-104">Expression classifications</span></span>

<span data-ttu-id="60db1-105">式は、次のいずれかに分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="60db1-106">値。</span><span class="sxs-lookup"><span data-stu-id="60db1-106">A value.</span></span> <span data-ttu-id="60db1-107">すべての値には、型が関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="60db1-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="60db1-108">変数です。</span><span class="sxs-lookup"><span data-stu-id="60db1-108">A variable.</span></span> <span data-ttu-id="60db1-109">すべての変数は、関連付けられた型、変数の宣言型つまりを持っています。</span><span class="sxs-lookup"><span data-stu-id="60db1-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="60db1-110">名前空間。</span><span class="sxs-lookup"><span data-stu-id="60db1-110">A namespace.</span></span> <span data-ttu-id="60db1-111">この分類の式の左側にあることができますのみに、 *member_access* ([メンバー アクセス](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="60db1-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="60db1-112">他のコンテキストでは、名前空間として分類される式は、コンパイル時エラーを発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="60db1-113">型。</span><span class="sxs-lookup"><span data-stu-id="60db1-113">A type.</span></span> <span data-ttu-id="60db1-114">この分類の式の左側にあることができますのみに、 *member_access* ([メンバー アクセス](expressions.md#member-access))、またはオペランドとして、`as`演算子 ([演算子として、](expressions.md#the-as-operator))、`is`演算子 ([、演算子は、](expressions.md#the-is-operator))、または`typeof`演算子 ([typeof 演算子](expressions.md#the-typeof-operator))。</span><span class="sxs-lookup"><span data-stu-id="60db1-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="60db1-115">他のコンテキストでは、型として分類される式は、コンパイル時エラーを発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="60db1-116">一連のオーバー ロードされたメソッドをメンバー検索の結果は、メソッド グループ ([メンバー ルックアップ](expressions.md#member-lookup))。</span><span class="sxs-lookup"><span data-stu-id="60db1-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="60db1-117">メソッド グループには、関連付けられたインスタンス式と関連付けられている型の引数リストがあります。</span><span class="sxs-lookup"><span data-stu-id="60db1-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="60db1-118">インスタンス式の評価の結果になりますで表されるインスタンスがインスタンス メソッドが呼び出されたときに`this`([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="60db1-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="60db1-119">メソッド グループが許可されている、 *invocation_expression* ([Invocation 式](expressions.md#invocation-expressions))、 *delegate_creation_expression* ([デリゲートの作成式](expressions.md#delegate-creation-expressions)) の左側として、演算子、および互換性のあるデリゲート型に暗黙的に変換できます ([メソッド グループ変換](conversions.md#method-group-conversions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="60db1-120">他のコンテキストでは、メソッド グループに分類される式は、コンパイル時エラーを発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="60db1-121">Null リテラルです。</span><span class="sxs-lookup"><span data-stu-id="60db1-121">A null literal.</span></span> <span data-ttu-id="60db1-122">この分類の式は、参照型または null 許容型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="60db1-123">匿名関数です。</span><span class="sxs-lookup"><span data-stu-id="60db1-123">An anonymous function.</span></span> <span data-ttu-id="60db1-124">この分類の式は、互換性のあるデリゲート型または式ツリー型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="60db1-125">プロパティ アクセス。</span><span class="sxs-lookup"><span data-stu-id="60db1-125">A property access.</span></span> <span data-ttu-id="60db1-126">すべてのプロパティ アクセスは、関連付けられた型、つまり、プロパティの型を持ちます。</span><span class="sxs-lookup"><span data-stu-id="60db1-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="60db1-127">さらに、プロパティ アクセスには、関連付けられたインスタンス式があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="60db1-128">アクセサー (、`get`または`set`ブロック) のインスタンスのプロパティへのアクセスが呼び出される、インスタンス式の評価の結果によって表されるインスタンスになります`this`([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="60db1-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="60db1-129">イベントへのアクセス。</span><span class="sxs-lookup"><span data-stu-id="60db1-129">An event access.</span></span> <span data-ttu-id="60db1-130">イベントのすべてのアクセスが、関連付けられた型、つまり、イベントの種類。</span><span class="sxs-lookup"><span data-stu-id="60db1-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="60db1-131">さらに、イベントへのアクセスには、関連付けられたインスタンス式があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="60db1-132">左側のオペランドとして、イベントへのアクセスがあります、`+=`と`-=`演算子 ([イベント割り当て](expressions.md#event-assignment))。</span><span class="sxs-lookup"><span data-stu-id="60db1-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="60db1-133">他のコンテキストでは、イベント アクセスとして分類される式は、コンパイル時エラーを発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="60db1-134">インデクサー アクセス。</span><span class="sxs-lookup"><span data-stu-id="60db1-134">An indexer access.</span></span> <span data-ttu-id="60db1-135">すべてのインデクサー アクセスは、関連付けられた型、インデクサーの要素型つまりを持ちます。</span><span class="sxs-lookup"><span data-stu-id="60db1-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="60db1-136">さらに、インデクサー アクセスは、関連付けられたインスタンス式と、関連する引数リストがいます。</span><span class="sxs-lookup"><span data-stu-id="60db1-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="60db1-137">アクセサー (、`get`または`set`ブロック)、インデクサーのアクセスが呼び出される、インスタンス式の評価の結果によって表されるインスタンスになります`this`([このアクセス](expressions.md#this-access))、およびの結果引数リストを評価するには、呼び出しのパラメーター リストになります。</span><span class="sxs-lookup"><span data-stu-id="60db1-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="60db1-138">何もない。</span><span class="sxs-lookup"><span data-stu-id="60db1-138">Nothing.</span></span> <span data-ttu-id="60db1-139">これは、式の戻り値の型には、メソッドの呼び出しは、するときに発生します。`void`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="60db1-140">コンテキストで有効では何もとして分類される式を*statement_expression* ([式ステートメント](statements.md#expression-statements))。</span><span class="sxs-lookup"><span data-stu-id="60db1-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="60db1-141">式の最終的な結果は、名前空間、型、メソッド グループ、またはイベントへのアクセスには。</span><span class="sxs-lookup"><span data-stu-id="60db1-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="60db1-142">代わりに、前述のように、これらの式のカテゴリには特定のコンテキストでのみ許可されている中間的な構造です。</span><span class="sxs-lookup"><span data-stu-id="60db1-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="60db1-143">プロパティ アクセスまたはインデクサーのアクセスは常に再分類を値としての呼び出しを実行することによって、 *get アクセサー*または*set アクセサー*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="60db1-144">特定のアクセサーは、プロパティまたはインデクサーのアクセスのコンテキストによって決まります。アクセスが、割り当ての対象である場合、 *set アクセサー*新しい値を割り当てるために呼び出される ([単純な代入](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="60db1-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="60db1-145">それ以外の場合、 *get アクセサー* 、現在の値を取得するために呼び出す ([式の値](expressions.md#values-of-expressions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="60db1-146">式の値</span><span class="sxs-lookup"><span data-stu-id="60db1-146">Values of expressions</span></span>

<span data-ttu-id="60db1-147">式を伴う構造のほとんどは最終的に式を表すことを必要とする***値***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="60db1-148">このような場合は、実際の式は、名前空間、型、メソッド グループ、または、何を表す場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="60db1-149">ただし、式では、プロパティ アクセス、インデクサー アクセス、または変数を表して、プロパティ、インデクサー、または変数の値に暗黙的に代入します。</span><span class="sxs-lookup"><span data-stu-id="60db1-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="60db1-150">変数の値は、変数で識別される記憶域の場所に格納されている値だけです。</span><span class="sxs-lookup"><span data-stu-id="60db1-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="60db1-151">変数は、明示的に代入考慮する必要があります ([確実な代入](variables.md#definite-assignment)) 前に、その値を取得する、またはそれ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="60db1-152">呼び出して、プロパティ アクセス式の値を取得、 *get アクセサー*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="60db1-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="60db1-153">プロパティがにない場合*get アクセサー*コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="60db1-154">それ以外の場合、関数メンバーの呼び出し ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) が実行され、呼び出しの結果、プロパティ アクセス式の値になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="60db1-155">呼び出してインデクサー アクセス式の値を取得、 *get アクセサー*のインデクサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="60db1-156">インデクサーがにない場合*get アクセサー*コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="60db1-157">それ以外の場合、関数メンバーの呼び出し ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 引数で実行されるインデクサー アクセス式に関連付けられているリストと呼び出しの結果値になりますインデクサー アクセス式。</span><span class="sxs-lookup"><span data-stu-id="60db1-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="60db1-158">静的および動的バインディング</span><span class="sxs-lookup"><span data-stu-id="60db1-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="60db1-159">型または構成する式 (引数、オペランド、受信側) の値に基づく操作の意味を決定するプロセスとして呼ば***バインド***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="60db1-160">たとえば、受信側と引数の型に基づいて、メソッド呼び出しの意味が決まります。</span><span class="sxs-lookup"><span data-stu-id="60db1-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="60db1-161">演算子の意味では、そのオペランドの型に基づいて決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="60db1-162">に基づいてその構成要素である式のコンパイル時の型にコンパイル時に、操作の意味が決定されます、通常、c# でします。</span><span class="sxs-lookup"><span data-stu-id="60db1-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="60db1-163">同様に、式にエラーが含まれている場合、エラーが検出され、コンパイラによって報告されました。</span><span class="sxs-lookup"><span data-stu-id="60db1-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="60db1-164">このアプローチと呼ばれる***静的バインディング***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="60db1-165">ただし、式が動的な式である場合 (つまり、型を持つ`dynamic`) に含まれている任意のバインディングである型ではなくの実行時の型 (つまりの実際の型オブジェクトの実行時にことを示します) に基づく必要があることを示しますこのコンパイル時。</span><span class="sxs-lookup"><span data-stu-id="60db1-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="60db1-166">このような操作のバインディングはそのため、操作がプログラムの実行中に実行時まで延期されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="60db1-167">呼ばれる***動的バインド***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="60db1-168">操作が動的にバインドされている場合は、コンパイラによって実行がほとんどまたはまったくないチェックします。</span><span class="sxs-lookup"><span data-stu-id="60db1-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="60db1-169">代わりに、実行時間のバインドに失敗した場合、実行時に例外としてエラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="60db1-170">C# では、次の操作は、バインドが適用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="60db1-171">メンバー アクセス。 `e.M`</span><span class="sxs-lookup"><span data-stu-id="60db1-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="60db1-172">メソッドの呼び出し: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="60db1-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="60db1-173">デリゲートの呼び出し:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="60db1-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="60db1-174">要素へのアクセス: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="60db1-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="60db1-175">オブジェクトの作成: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="60db1-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="60db1-176">オーバー ロードされた単項演算子: `+`、 `-`、 `!`、 `~`、 `++`、 `--`、 `true`、 `false`</span><span class="sxs-lookup"><span data-stu-id="60db1-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="60db1-177">オーバー ロードされた二項演算子: `+`、 `-`、 `*`、 `/`、 `%`、 `&`、 `&&`、 `|`、 `||`、 `??`、 `^`、 `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="60db1-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="60db1-178">代入演算子: `=`、 `+=`、 `-=`、 `*=`、 `/=`、 `%=`、 `&=`、 `|=`、 `^=`、 `<<=`、 `>>=`</span><span class="sxs-lookup"><span data-stu-id="60db1-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="60db1-179">明示的および暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="60db1-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="60db1-180">動的な式が使用されないときに c# 既定値は静的バインディングは、構成する式のコンパイル時の型が、選択プロセスで使用されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="60db1-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="60db1-181">ただし、動的な式は、上記の操作で構成する式のいずれかが、操作は代わりに動的にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="60db1-182">バインディング時間</span><span class="sxs-lookup"><span data-stu-id="60db1-182">Binding-time</span></span>

<span data-ttu-id="60db1-183">静的バインディングは、動的バインドが実行時に行いますが、コンパイル時に、配置します。</span><span class="sxs-lookup"><span data-stu-id="60db1-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="60db1-184">次のセクションで、用語***バインド時***コンパイル時または実行時にバインディングが行われる時期に応じてのいずれかを参照します。</span><span class="sxs-lookup"><span data-stu-id="60db1-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="60db1-185">次の例は、静的および動的バインディングおよびバインド時の概念を示しています。</span><span class="sxs-lookup"><span data-stu-id="60db1-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="60db1-186">最初の 2 つの呼び出しが静的にバインドされている: のオーバー ロード`Console.WriteLine`引数のコンパイル時の型に基づいて取得されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="60db1-187">したがって、バインド時は、コンパイル時です。</span><span class="sxs-lookup"><span data-stu-id="60db1-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="60db1-188">3 番目の呼び出しが動的にバインドされている: のオーバー ロード`Console.WriteLine`引数の実行時の型に基づいて取得されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="60db1-189">引数は、動的な式--、コンパイル時の型は`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="60db1-190">そのため、3 番目の呼び出しのバインド時は、実行時です。</span><span class="sxs-lookup"><span data-stu-id="60db1-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="60db1-191">動的バインド</span><span class="sxs-lookup"><span data-stu-id="60db1-191">Dynamic binding</span></span>

<span data-ttu-id="60db1-192">動的バインドの目的は、c# プログラムとの対話ができるように、***動的オブジェクト***、つまり、c# の通常の規則に従っていないオブジェクトがシステムを入力します。</span><span class="sxs-lookup"><span data-stu-id="60db1-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="60db1-193">さまざまな種類のシステムと他のプログラミング言語からオブジェクトを動的オブジェクトがあります。 またはさまざまな操作の独自のバインディング セマンティクスを実装するためにセットアップ プログラムであるオブジェクトがあります。</span><span class="sxs-lookup"><span data-stu-id="60db1-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="60db1-194">動的オブジェクトが自身のセマンティクスを実装するメカニズムは、実装定義です。</span><span class="sxs-lookup"><span data-stu-id="60db1-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="60db1-195">--もう一度実装定義 - 特定のインターフェイスは、C# で実行時に特別な意味があることを通知する動的オブジェクトによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="60db1-196">したがっての動的オブジェクトの操作が動的にバインドされている、ときに、このドキュメントで指定されている c# のではなく、独自のバインディング セマンティクスを引き継ぎます。</span><span class="sxs-lookup"><span data-stu-id="60db1-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="60db1-197">動的バインドの目的は、動的オブジェクトとの相互運用を許可するが、C# では、動的バインド、すべてのオブジェクトでは動的かどうかどうか。</span><span class="sxs-lookup"><span data-stu-id="60db1-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="60db1-198">これにより、それらに対する操作の結果が、動的なオブジェクト自体ではありませんが、プログラマがコンパイル時に不明な型のままの動的オブジェクトは、スムーズに統合できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="60db1-199">動的バインドが含まれるオブジェクトには、動的オブジェクトがない場合でも、リフレクション ベースのエラーを起こしやすいコードを排除を支援することができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="60db1-200">動的バインドを適用した場合、何のコンパイル時のチェック - - いずれかが適用される場合と、どのようなコンパイル結果と式分類が正確には、言語では、各構成要素の次のセクションでは、について説明します。</span><span class="sxs-lookup"><span data-stu-id="60db1-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="60db1-201">構成要素である式の種類</span><span class="sxs-lookup"><span data-stu-id="60db1-201">Types of constituent expressions</span></span>

<span data-ttu-id="60db1-202">操作が静的にバインドされている場合 (例: 受信側、引数、インデックスまたはオペランド) の構成要素である式の型は常と見なされますその式のコンパイル時の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="60db1-203">操作が動的にバインドされている場合は、構成要素である式のコンパイル時の型に応じてさまざまな方法で構成要素である式の型が決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="60db1-204">コンパイル時の型の構成要素である式`dynamic`式は、実行時に評価されます実際の値の型を持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="60db1-205">コンパイル時の型は型パラメーターの構成要素である式は実行時に、型パラメーターがバインドされている型であると見なされます</span><span class="sxs-lookup"><span data-stu-id="60db1-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="60db1-206">それ以外の場合、構成要素である式は、コンパイル時の型であると見なされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="60db1-207">演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-207">Operators</span></span>

<span data-ttu-id="60db1-208">式がから構築された***オペランド***と***演算子***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="60db1-209">式の演算子は、オペランドに適用する演算を表します。</span><span class="sxs-lookup"><span data-stu-id="60db1-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="60db1-210">演算子の例として、`+`、`-`、`*`、`/`、および `new` などがあります。</span><span class="sxs-lookup"><span data-stu-id="60db1-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="60db1-211">オペランドの例としては、リテラル、フィールド、ローカル変数、式などがあります。</span><span class="sxs-lookup"><span data-stu-id="60db1-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="60db1-212">演算子の 3 つの種類があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="60db1-213">単項演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-213">Unary operators.</span></span> <span data-ttu-id="60db1-214">単項演算子が 1 つのオペランドを受け取り、いずれかのプレフィックス表記を使用して (よう`--x`) または notation の後置 (など`x++`)。</span><span class="sxs-lookup"><span data-stu-id="60db1-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="60db1-215">二項演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-215">Binary operators.</span></span> <span data-ttu-id="60db1-216">二項演算子が 2 つのオペランドを取るし、挿入辞表記を使用して、すべて (など`x + y`)。</span><span class="sxs-lookup"><span data-stu-id="60db1-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="60db1-217">三項演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-217">Ternary operator.</span></span> <span data-ttu-id="60db1-218">1 つだけの三項演算子`?:`が存在する 3 つのオペランドを受け取るし、挿入辞表記を使用して (`c ? x : y`)。</span><span class="sxs-lookup"><span data-stu-id="60db1-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="60db1-219">式の演算子の評価の順序が続く、***優先順位***と***結合規則***演算子の ([演算子の優先順位と結合規則](expressions.md#operator-precedence-and-associativity)).</span><span class="sxs-lookup"><span data-stu-id="60db1-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="60db1-220">式のオペランドは、左から右に評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="60db1-221">たとえば、 `F(i) + G(i++) * H(i)`、メソッド`F`の古い値を使用して呼び出された`i`、then メソッド`G`の古い値を使用して呼び出した`i`、および、最後に、メソッド`H`の新しい値を使用して呼び出した`i`.</span><span class="sxs-lookup"><span data-stu-id="60db1-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="60db1-222">演算子の優先順位とは別と関連付けられていないではありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="60db1-223">特定の演算子を指定できます***オーバー ロードされた***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="60db1-224">演算子のオーバー ロードは、1 つの操作に対して指定するユーザー定義演算子の実装を許可またはユーザー定義のクラスまたは構造体型の両方のオペランドは ([演算子のオーバー ロード](expressions.md#operator-overloading))。</span><span class="sxs-lookup"><span data-stu-id="60db1-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="60db1-225">演算子の優先順位と結合規則</span><span class="sxs-lookup"><span data-stu-id="60db1-225">Operator precedence and associativity</span></span>

<span data-ttu-id="60db1-226">複数の演算子を含む式の場合、演算子の***優先順位***によって各々の演算子が評価される順序が決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="60db1-227">たとえば、式`x + y * z`として評価されます`x + (y * z)`ため、`*`演算子は、バイナリよりも優先順位の高い`+`演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="60db1-228">演算子の優先順位は、その関連する文法の運用環境の定義によって確立されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="60db1-229">たとえば、 *additive_expression*のシーケンスから成る*multiplicative_expression*で区切られた s`+`または`-`ため、演算子、`+`と`-`演算子よりも優先順位を下げる、 `*`、 `/`、および`%`演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="60db1-230">次の表は、優先順位の高いものから最下位の順序ですべての演算子をまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="60db1-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="60db1-231">__セクション__</span><span class="sxs-lookup"><span data-stu-id="60db1-231">__Section__</span></span>                                                                                   | <span data-ttu-id="60db1-232">__カテゴリ__</span><span class="sxs-lookup"><span data-stu-id="60db1-232">__Category__</span></span>                | <span data-ttu-id="60db1-233">__演算子__</span><span class="sxs-lookup"><span data-stu-id="60db1-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="60db1-234">一次式</span><span class="sxs-lookup"><span data-stu-id="60db1-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="60db1-235">1 次式</span><span class="sxs-lookup"><span data-stu-id="60db1-235">Primary</span></span>                     | <span data-ttu-id="60db1-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="60db1-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="60db1-237">単項演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="60db1-238">単項</span><span class="sxs-lookup"><span data-stu-id="60db1-238">Unary</span></span>                       | <span data-ttu-id="60db1-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="60db1-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="60db1-240">算術演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="60db1-241">乗法</span><span class="sxs-lookup"><span data-stu-id="60db1-241">Multiplicative</span></span>              | <span data-ttu-id="60db1-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="60db1-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="60db1-243">算術演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="60db1-244">加法</span><span class="sxs-lookup"><span data-stu-id="60db1-244">Additive</span></span>                    | <span data-ttu-id="60db1-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="60db1-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="60db1-246">シフト演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="60db1-247">シフト</span><span class="sxs-lookup"><span data-stu-id="60db1-247">Shift</span></span>                       | <span data-ttu-id="60db1-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="60db1-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="60db1-249">関係演算子と型検査演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="60db1-250">関係式と型検査</span><span class="sxs-lookup"><span data-stu-id="60db1-250">Relational and type testing</span></span> | <span data-ttu-id="60db1-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="60db1-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="60db1-252">関係演算子と型検査演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="60db1-253">等価比較</span><span class="sxs-lookup"><span data-stu-id="60db1-253">Equality</span></span>                    | <span data-ttu-id="60db1-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="60db1-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="60db1-255">論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="60db1-256">論理 AND</span><span class="sxs-lookup"><span data-stu-id="60db1-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="60db1-257">論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="60db1-258">論理 XOR</span><span class="sxs-lookup"><span data-stu-id="60db1-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="60db1-259">論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="60db1-260">論理 OR</span><span class="sxs-lookup"><span data-stu-id="60db1-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="60db1-261">条件論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="60db1-262">条件 AND</span><span class="sxs-lookup"><span data-stu-id="60db1-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="60db1-263">条件論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="60db1-264">条件 OR</span><span class="sxs-lookup"><span data-stu-id="60db1-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="60db1-265">null 合体演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="60db1-266">Null 合体演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="60db1-267">条件演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="60db1-268">条件</span><span class="sxs-lookup"><span data-stu-id="60db1-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="60db1-269">[代入演算子](expressions.md#assignment-operators)、[匿名関数式](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="60db1-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="60db1-270">代入式とラムダ式</span><span class="sxs-lookup"><span data-stu-id="60db1-270">Assignment and lambda expression</span></span> | <span data-ttu-id="60db1-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="60db1-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="60db1-272">優先順位が同じ 2 つの演算子のオペランドが発生したときに、演算子の結合規則操作が実行される順序を制御します。</span><span class="sxs-lookup"><span data-stu-id="60db1-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="60db1-273">すべてのバイナリ演算子は、代入演算子と null 合体演算子を除く***左結合***、左から右に操作を実行することを意味します。</span><span class="sxs-lookup"><span data-stu-id="60db1-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="60db1-274">たとえば、`x + y + z` は `(x + y) + z` と評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="60db1-275">代入演算子、null 合体演算子と条件演算子 (`?:`) は***右から左***、つまり演算は右から左に実行されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="60db1-276">たとえば、`x = y = z` は `x = (y = z)` と評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="60db1-277">優先順位と結合性は、かっこを使用して制御することができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="60db1-278">たとえば、`x + y * z` は最初に `y` と `z` を掛け、そして結果を `x` に足しますが、`(x + y) * z` では最初に `x` と `y` を足してから `z` を掛けます。</span><span class="sxs-lookup"><span data-stu-id="60db1-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="60db1-279">演算子のオーバーロード</span><span class="sxs-lookup"><span data-stu-id="60db1-279">Operator overloading</span></span>

<span data-ttu-id="60db1-280">すべての単項および二項演算子には定義済みの実装では、任意の式で自動的に利用可能があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="60db1-281">定義済みの実装だけでなくユーザー定義の実装を導入できます`operator`クラスと構造体の宣言 ([演算子](classes.md#operators))。</span><span class="sxs-lookup"><span data-stu-id="60db1-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="60db1-282">ユーザー定義演算子の実装に常に優先定義済みの演算子の実装。該当するユーザー定義演算子の実装には存在しない場合、定義済みの演算子の実装と見なされるで説明されている専用[単項演算子のオーバー ロードの解決](expressions.md#unary-operator-overload-resolution)と[二項演算子のオーバー ロード解像度](expressions.md#binary-operator-overload-resolution)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="60db1-283">***オーバー ロードされた単項演算子***は。</span><span class="sxs-lookup"><span data-stu-id="60db1-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="60db1-284">`true`と`false`式で明示的に使用されていない (そのため、優先順位表では含まれませんと[演算子の優先順位と結合規則](expressions.md#operator-precedence-and-associativity))、いるため、演算子と見なされますいくつかの式のコンテキストで呼び出される: ブール式 ([ブール式](expressions.md#boolean-expressions)) と、条件式を含む式 ([条件演算子](expressions.md#conditional-operator))、および論理条件演算子 ([条件付き論理演算子](expressions.md#conditional-logical-operators))。</span><span class="sxs-lookup"><span data-stu-id="60db1-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="60db1-285">***二項演算子のオーバー ロード可能な***は。</span><span class="sxs-lookup"><span data-stu-id="60db1-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="60db1-286">上記の演算子のみをオーバー ロードできます。</span><span class="sxs-lookup"><span data-stu-id="60db1-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="60db1-287">具体的には、メンバー アクセスは、メソッドの呼び出しのオーバー ロードすることはできませんまたは`=`、 `&&`、 `||`、 `??`、 `?:`、 `=>`、 `checked`、 `unchecked`、 `new`、 `typeof`、 `default`、 `as`、および`is`演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="60db1-288">二項演算子をオーバーロードすると、対応する代入演算子がある場合、これも暗黙的にオーバーロードされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="60db1-289">演算子のオーバー ロードではたとえば、`*`演算子のオーバー ロードも`*=`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="60db1-290">詳細についてはこの[複合代入](expressions.md#compound-assignment)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="60db1-291">なお、代入演算子自体 (`=`) オーバー ロードできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="60db1-292">常に割り当てでは、変数に値の単純なビットごとのコピーを実行します。</span><span class="sxs-lookup"><span data-stu-id="60db1-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="60db1-293">キャスト演算など`(T)x`、ユーザー定義の変換を提供することではオーバー ロード ([ユーザー定義の変換](conversions.md#user-defined-conversions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="60db1-294">要素にアクセスするよう`a[x]`、オーバー ロードされた演算子とは見なされません。</span><span class="sxs-lookup"><span data-stu-id="60db1-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="60db1-295">代わりに、インデクサーによってサポートがユーザー定義のインデックス作成 ([インデクサー](classes.md#indexers))。</span><span class="sxs-lookup"><span data-stu-id="60db1-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="60db1-296">式では、演算子が演算子の表記を使用して参照されているし、機能の表記を使用して、宣言で演算子が参照されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="60db1-297">次の表では、演算子と単項および二項演算子関数の表記の間の関係を示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="60db1-298">最初のエントリで*op*プレフィックスのオーバー ロードされた単項演算子を表します。</span><span class="sxs-lookup"><span data-stu-id="60db1-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="60db1-299">2 番目のエントリで*op*単項後置形式を表します`++`と`--`演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="60db1-300">3 番目のエントリで*op*オーバー ロード可能な 2 項演算子を表します。</span><span class="sxs-lookup"><span data-stu-id="60db1-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="60db1-301">__演算子表記__</span><span class="sxs-lookup"><span data-stu-id="60db1-301">__Operator notation__</span></span> | <span data-ttu-id="60db1-302">__機能の表記__</span><span class="sxs-lookup"><span data-stu-id="60db1-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="60db1-303">ユーザー定義演算子の宣言では、少なくとも 1 つのパラメーターは、演算子の宣言を含むクラスまたは構造体の型が常に要求します。</span><span class="sxs-lookup"><span data-stu-id="60db1-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="60db1-304">したがって、定義済みの演算子と同じシグネチャを持つユーザー定義演算子のことはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="60db1-305">ユーザー定義演算子の宣言には、構文、優先順位、または演算子の結合規則を変更できません。</span><span class="sxs-lookup"><span data-stu-id="60db1-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="60db1-306">たとえば、`/`演算子は常に二項演算子では、常には、優先順位レベルで指定[演算子の優先順位と結合規則](expressions.md#operator-precedence-and-associativity)、左からの結合は常にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="60db1-307">どのような計算を実行するユーザー定義演算子のことはできますが、直感的に予想されるもの以外の結果を生成するための実装は使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="60db1-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="60db1-308">実装ではたとえば、 `operator ==` 2 つのオペランドが等しいかどうかを比較し、適切なを返す必要があります`bool`結果。</span><span class="sxs-lookup"><span data-stu-id="60db1-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="60db1-309">個々 の演算子の説明については、[一次式](expressions.md#primary-expressions)を通じて[条件付き論理演算子](expressions.md#conditional-logical-operators)演算子および適用される追加の規則の定義済みの実装を指定します。それぞれの演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="60db1-310">説明の用語を使用して、***単項演算子のオーバー ロードの解決***、***二項演算子のオーバー ロードの解決***、および***数値プロモーション***は他の定義次のセクションでは、記載されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="60db1-311">単項演算子のオーバー ロードの解決</span><span class="sxs-lookup"><span data-stu-id="60db1-311">Unary operator overload resolution</span></span>

<span data-ttu-id="60db1-312">フォームの操作を`op x`または`x op`ここで、`op`オーバー ロードされた単項演算子、および`x`型の式は、 `X`、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="60db1-313">一連の候補ユーザー定義の演算子によって提供される`X`操作`operator op(x)`の規則を使用して決定されます[候補ユーザー定義演算子](expressions.md#candidate-user-defined-operators)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="60db1-314">ユーザー定義演算子の候補のセットが空でない場合は、この操作の演算子の候補のセットをなります。</span><span class="sxs-lookup"><span data-stu-id="60db1-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="60db1-315">それ以外の場合、定義済みの単項`operator op`がリフトのフォームを含めて、実装操作の演算子の候補の集合になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="60db1-316">特定の演算子の定義済みの実装が演算子の説明で指定されます ([一次式](expressions.md#primary-expressions)と[単項演算子](expressions.md#unary-operators))。</span><span class="sxs-lookup"><span data-stu-id="60db1-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="60db1-317">オーバー ロード解決規則[オーバー ロードの解決](expressions.md#overload-resolution)引数リストに対して最適な演算子を選択する演算子を候補のセットに適用される`(x)`、この演算子のオーバー ロードの結果になります解決プロセスです。</span><span class="sxs-lookup"><span data-stu-id="60db1-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="60db1-318">1 つの最適な演算子を選択するオーバー ロードの解決に失敗した場合、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="60db1-319">二項演算子のオーバー ロードの解決</span><span class="sxs-lookup"><span data-stu-id="60db1-319">Binary operator overload resolution</span></span>

<span data-ttu-id="60db1-320">フォームの操作を`x op y`ここで、 `op` 、オーバー ロードされた二項演算子は、`x`型の式は、 `X`、および`y`型の式は、 `Y`、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="60db1-321">一連の候補ユーザー定義の演算子によって提供される`X`と`Y`操作`operator op(x,y)`決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="60db1-322">セットによって提供される演算子の候補の和集合から成る`X`によって提供される演算子の候補と`Y`それぞれ特定の規則を使用して、[候補ユーザー定義演算子](expressions.md#candidate-user-defined-operators)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="60db1-323">場合`X`と`Y`は同じ型の場合、または`X`と`Y`演算子候補、結合されたセットで 1 回しか発生し、共通の基本型から派生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="60db1-324">ユーザー定義演算子の候補のセットが空でない場合は、この操作の演算子の候補のセットをなります。</span><span class="sxs-lookup"><span data-stu-id="60db1-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="60db1-325">それ以外の場合、定義済みのバイナリ`operator op`がリフトのフォームを含めて、実装操作の演算子の候補の集合になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="60db1-326">特定の演算子の定義済みの実装が演算子の説明で指定されます ([算術演算子](expressions.md#arithmetic-operators)を通じて[条件付き論理演算子](expressions.md#conditional-logical-operators))。</span><span class="sxs-lookup"><span data-stu-id="60db1-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="60db1-327">定義済みの列挙型とデリゲートの演算子と見なされる演算子のみでは、オペランドの 1 つのバインド時の型である列挙型、またはデリゲートの型で定義されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="60db1-328">オーバー ロード解決規則[オーバー ロードの解決](expressions.md#overload-resolution)引数リストに対して最適な演算子を選択する演算子を候補のセットに適用される`(x,y)`、この演算子のオーバー ロードの結果になります解決プロセスです。</span><span class="sxs-lookup"><span data-stu-id="60db1-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="60db1-329">1 つの最適な演算子を選択するオーバー ロードの解決に失敗した場合、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="60db1-330">ユーザー定義演算子の候補</span><span class="sxs-lookup"><span data-stu-id="60db1-330">Candidate user-defined operators</span></span>

<span data-ttu-id="60db1-331">型を指定して`T`と操作`operator op(A)`ここで、`op`オーバー ロードされた演算子と`A`引数リストを候補のセットによって提供されるユーザー定義の演算子は、`T`の`operator op(A)`決定されます次のようにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="60db1-332">種類を決定`T0`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-332">Determine the type `T0`.</span></span> <span data-ttu-id="60db1-333">場合`T`が null 許容型では、`T0`基になる型をそれ以外の場合は`T0`と等しい`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="60db1-334">すべての`operator op`内の宣言`T0`と少なくとも 1 つの演算子が適用可能な場合は、すべてのリフトのような演算子では、形式 ([適用可能な関数メンバー](expressions.md#applicable-function-member)) 引数リストに対して`A`のセットでは、演算子の候補のような該当するすべての演算子から成る`T0`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="60db1-335">の場合`T0`は`object`、演算子の候補のセットが空です。</span><span class="sxs-lookup"><span data-stu-id="60db1-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="60db1-336">によって提供される演算子の候補のセットの場合は、`T0`の直接の基本クラスによって提供される演算子の候補のセットは、 `T0`、または有効な基本クラスの`T0`場合`T0`型パラメーターします。</span><span class="sxs-lookup"><span data-stu-id="60db1-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="60db1-337">数値の上位変換</span><span class="sxs-lookup"><span data-stu-id="60db1-337">Numeric promotions</span></span>

<span data-ttu-id="60db1-338">数値の上位変換は、定義済みの単項および二項の数値演算子のオペランドの特定の暗黙的な変換を自動的に実行するので構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="60db1-339">数値の上位変換はなく、個別のメカニズムではなく、定義済みの演算子をオーバー ロードの解決を適用する効果です。</span><span class="sxs-lookup"><span data-stu-id="60db1-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="60db1-340">数値の上位変換具体的には影響しませんユーザー定義の演算子の評価のような影響が発生するユーザー定義演算子を実装することができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="60db1-341">数値の上位変換の例は、としては、定義済みのバイナリの実装を考えてみましょう。`*`演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="60db1-342">ときにオーバー ロードの解決ルール ([オーバー ロードの解決](expressions.md#overload-resolution)) このセットに適用される演算子の結果はオペランドの型から最初の暗黙的な変換が存在する演算子を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="60db1-343">操作の例では、`b * s`ここで、`b`は、`byte`と`s`は、 `short`、オーバー ロードの解決方法の選択`operator *(int,int)`最善の演算子として。</span><span class="sxs-lookup"><span data-stu-id="60db1-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="60db1-344">そのため、効果は`b`と`s`に変換されます`int`、および結果の型は`int`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="60db1-345">同様に、操作の`i * d`ここで、`i`は、`int`と`d`は、 `double`、オーバー ロードの解決方法の選択`operator *(double,double)`最善の演算子として。</span><span class="sxs-lookup"><span data-stu-id="60db1-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="60db1-346">単項の数値の上位変換</span><span class="sxs-lookup"><span data-stu-id="60db1-346">Unary numeric promotions</span></span>

<span data-ttu-id="60db1-347">定義済みのオペランドの単項の数値の昇格が発生した`+`、 `-`、および`~`単項演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="60db1-348">単項の数値の上位変換は、型のオペランドを変換するだけで構成される`sbyte`、 `byte`、 `short`、 `ushort`、または`char`入力`int`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="60db1-349">さらに、単項の`-`演算子、単項の数値の上位変換型のオペランドの変換`uint`入力`long`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="60db1-350">バイナリ数値プロモーション</span><span class="sxs-lookup"><span data-stu-id="60db1-350">Binary numeric promotions</span></span>

<span data-ttu-id="60db1-351">定義済みのオペランドのバイナリ数値昇格が発生した`+`、 `-`、 `*`、 `/`、 `%`、 `&`、 `|`、 `^`、 `==`、 `!=`、`>`、 `<`、 `>=`、および`<=`二項演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="60db1-352">バイナリ数値の上位変換は、非リレーショナルの演算子が発生した場合、操作の結果の型にもなりますが、共通の型に暗黙的に両方のオペランドを変換します。</span><span class="sxs-lookup"><span data-stu-id="60db1-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="60db1-353">数値のバイナリの上位変換は、ここに表示される順序で、次の規則を適用することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="60db1-354">いずれかのオペランドの型の場合`decimal`、もう一方のオペランドを型に変換されます`decimal`、バインド時のエラーは、もう一方のオペランドの型の場合に発生します。 または`float`または`double`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="60db1-355">それ以外の場合、いずれかのオペランドの型の場合`double`、もう一方のオペランドを型に変換されます`double`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="60db1-356">それ以外の場合、いずれかのオペランドの型の場合`float`、もう一方のオペランドを型に変換されます`float`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="60db1-357">それ以外の場合、いずれかのオペランドの型の場合`ulong`、もう一方のオペランドを型に変換されます`ulong`、バインド時のエラーは、もう一方のオペランドの型の場合に発生します。 または`sbyte`、 `short`、 `int`、または`long`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="60db1-358">それ以外の場合、いずれかのオペランドの型の場合`long`、もう一方のオペランドを型に変換されます`long`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="60db1-359">それ以外の場合、いずれかのオペランドの型の場合`uint`ともう一方のオペランド型`sbyte`、 `short`、または`int`、両方のオペランドを型に変換されます`long`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="60db1-360">それ以外の場合、いずれかのオペランドの型の場合`uint`、もう一方のオペランドを型に変換されます`uint`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="60db1-361">それ以外の場合、両方のオペランドは、型に変換されます`int`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="60db1-362">最初の規則が混在するすべての操作を禁止することに注意してください、`decimal`の種類を`double`と`float`型。</span><span class="sxs-lookup"><span data-stu-id="60db1-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="60db1-363">間の暗黙的な変換がないという事実から、ルールに依存して、`decimal`型と`double`と`float`型。</span><span class="sxs-lookup"><span data-stu-id="60db1-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="60db1-364">なお、オペランドの型のことはできません`ulong`もう一方のオペランドが符号付き整数型の場合します。</span><span class="sxs-lookup"><span data-stu-id="60db1-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="60db1-365">理由は、整数型が存在しないことの全範囲を表すことができます`ulong`と符号付き整数型。</span><span class="sxs-lookup"><span data-stu-id="60db1-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="60db1-366">どちらの場合も、もう一方のオペランドと互換性がある型に 1 つのオペランドを明示的に変換するキャスト式を使用できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="60db1-367">例</span><span class="sxs-lookup"><span data-stu-id="60db1-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="60db1-368">バインディング時のエラーが発生、`decimal`によって乗算することはできません、`double`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="60db1-369">2 番目のオペランドを明示的な変換によって、エラーが解決`decimal`、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="60db1-370">リフトされた演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-370">Lifted operators</span></span>

<span data-ttu-id="60db1-371">***リフト演算子***もそれらの型の null 許容のフォームで使用する null 非許容値型を操作する定義済み、ユーザー定義の演算子を許可します。</span><span class="sxs-lookup"><span data-stu-id="60db1-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="60db1-372">リフトされた演算子は、次の説明に従って特定の要件を満たしている定義済み、ユーザー定義の演算子から構築されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="60db1-373">単項演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="60db1-374">場合、オペランドと結果の型は両方が null 非許容値型にリフトされた演算子のフォームが存在します。</span><span class="sxs-lookup"><span data-stu-id="60db1-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="60db1-375">リフトの形式が、1 つを追加することで構築された`?`修飾子をオペランドと結果の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="60db1-376">リフトされた演算子は、オペランドが null の場合、null 値を生成します。</span><span class="sxs-lookup"><span data-stu-id="60db1-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="60db1-377">それ以外の場合、リフトされた演算子はオペランドのラップを解除し、基になる演算子を適用し、結果をラップします。</span><span class="sxs-lookup"><span data-stu-id="60db1-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="60db1-378">2 項演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="60db1-379">オペランドと結果の型がすべての null 非許容値型である場合、リフトされた演算子のフォームが存在します。</span><span class="sxs-lookup"><span data-stu-id="60db1-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="60db1-380">リフトの形式が、1 つを追加することで構築された`?`オペランドと結果の種類ごとに修飾子。</span><span class="sxs-lookup"><span data-stu-id="60db1-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="60db1-381">1 つの場合、リフトされた演算子は null 値を生成または両方のオペランドが null (例外である、`&`と`|`の演算子、 `bool?` 」の説明に従って、入力[ブール論理演算子](expressions.md#boolean-logical-operators))。</span><span class="sxs-lookup"><span data-stu-id="60db1-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="60db1-382">それ以外の場合、リフトされた演算子はオペランドのラップを解除し、基になる演算子を適用し、結果をラップします。</span><span class="sxs-lookup"><span data-stu-id="60db1-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="60db1-383">等値演算子の</span><span class="sxs-lookup"><span data-stu-id="60db1-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="60db1-384">オペランドの型が null 非許容値型と結果型がある場合は、リフトされた演算子のフォームが存在する`bool`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="60db1-385">リフトの形式が、1 つを追加することで構築された`?`修飾子を追加します。</span><span class="sxs-lookup"><span data-stu-id="60db1-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="60db1-386">リフトされた演算子には、2 つの null 値は等しく、および null 値が null 以外の値に等しくないです。</span><span class="sxs-lookup"><span data-stu-id="60db1-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="60db1-387">両方のオペランドが null 以外の場合は、リフトされた演算子はオペランドのラップを解除しを生成する基になる演算子を適用、`bool`結果。</span><span class="sxs-lookup"><span data-stu-id="60db1-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="60db1-388">関係演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="60db1-389">オペランドの型が null 非許容値型と結果型がある場合は、リフトされた演算子のフォームが存在する`bool`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="60db1-390">リフトの形式が、1 つを追加することで構築された`?`修飾子を追加します。</span><span class="sxs-lookup"><span data-stu-id="60db1-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="60db1-391">リフトされた演算子の値を生成する`false`1 つまたは両方のオペランドが null の場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="60db1-392">それ以外の場合、リフトされた演算子はオペランドのラップを解除し、生成するために基になる演算子を適用、`bool`結果。</span><span class="sxs-lookup"><span data-stu-id="60db1-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="60db1-393">メンバー ルックアップ</span><span class="sxs-lookup"><span data-stu-id="60db1-393">Member lookup</span></span>

<span data-ttu-id="60db1-394">メンバー検索は、型のコンテキストで名前の意味を決定するというプロセスです。</span><span class="sxs-lookup"><span data-stu-id="60db1-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="60db1-395">メンバー参照が評価の一部として発生することが、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバー アクセス](expressions.md#member-access)) で、式。</span><span class="sxs-lookup"><span data-stu-id="60db1-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="60db1-396">場合、 *simple_name*または*member_access*として発生する、 *primary_expression*の*invocation_expression* ([メソッドの呼び出し](expressions.md#method-invocations))、呼び出されるメンバーと呼びます。</span><span class="sxs-lookup"><span data-stu-id="60db1-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="60db1-397">メンバーがメソッドまたはイベントの場合、または定数、フィールド、またはデリゲート型のプロパティである場合 ([デリゲート](delegates.md))、または型`dynamic`([動的な型](types.md#the-dynamic-type))、メンバーはと呼ばれますし、*invocable*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="60db1-398">メンバー参照では、メンバーが型パラメーターと、メンバーがアクセスできるかどうかの数が、メンバーの名前だけでなくと見なします。</span><span class="sxs-lookup"><span data-stu-id="60db1-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="60db1-399">メンバー検索のためには、それぞれの宣言に示されている型パラメーターの数であるジェネリック メソッドと入れ子になったジェネリック型とその他のすべてのメンバーは、0 個の型パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="60db1-400">名前のメンバー検索 `N`で`K` 型にパラメーターを入力 `T`ように処理されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="60db1-401">最初に、アクセス可能なメンバーがという名前のセットを `N`決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="60db1-402">場合`T`、型パラメーター セットがアクセス可能なメンバーがという名前のセットの和集合が `N`制約のプライマリまたはセカンダリの制約として指定された型の各 ([パラメーターの制約入力](classes.md#type-parameter-constraints)) の `T`、アクセス可能なメンバーがという名前のセットと共に `N`で`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="60db1-403">それ以外の場合、すべてにアクセスできるのセットが ([メンバー アクセス](basic-concepts.md#member-access)) という名前のメンバー `N`で `T`継承されたメンバーと名前付きアクセス可能なメンバーを含めて、 `N`で`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="60db1-404">場合`T`構築された型は、メンバーのセットが」の説明に従って、型引数を代入することによって取得した[構築された型のメンバー](classes.md#members-of-constructed-types)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="60db1-405">メンバーが含まれる、`override`修飾子は、セットから除外されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="60db1-406">次に、if`K`は 0、型の宣言には、型パラメーターが含まれては削除が入れ子になったすべてです。</span><span class="sxs-lookup"><span data-stu-id="60db1-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="60db1-407">場合`K`、異なる数のパラメーターを削除する型を持つすべてのメンバーに 0 以外です。</span><span class="sxs-lookup"><span data-stu-id="60db1-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="60db1-408">場合`K`が 0 個、メソッドのある型の型の推論プロセス以降のパラメーターは削除されません ([型推論](expressions.md#type-inference)) 型引数の推論できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="60db1-409">次に、メンバーがある場合*呼び出さ*、以外のすべて-*invocable*メンバーをセットから削除します。</span><span class="sxs-lookup"><span data-stu-id="60db1-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="60db1-410">次に、他のメンバーが隠ぺいされているメンバーは、セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="60db1-411">すべてのメンバーの`S.M`、セット内で`S`の種類は、メンバー `M`宣言されると、次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="60db1-412">場合`M`の基本型で宣言されているすべてのメンバー、定数、フィールド、プロパティ、イベント、または列挙型のメンバーは、`S`セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="60db1-413">場合`M`が型の宣言型以外のすべての基本型で宣言された`S`、セットから削除はすべて、同じ数と型パラメーターの宣言型と`M`の基本型で宣言された`S`が削除されますセット。</span><span class="sxs-lookup"><span data-stu-id="60db1-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="60db1-414">場合`M`、メソッドの基本型で宣言されているすべてのメソッド以外のメンバーが`S`セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="60db1-415">次に、クラス メンバーが隠ぺいされているインターフェイス メンバーは、セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="60db1-416">ここでは、場合にのみ`T`型パラメーターと`T`以外の両方を有効な基本クラスを持つ`object`と空でない有効なインターフェイス セット ([パラメーターの制約入力](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="60db1-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="60db1-417">すべてのメンバーの`S.M`、セット内で`S`の種類は、メンバー`M`が宣言されている場合に、次の規則が適用される`S`以外のクラス宣言は、 `object`:</span><span class="sxs-lookup"><span data-stu-id="60db1-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="60db1-418">場合`M`が定数、フィールド、プロパティ、イベント、列挙型メンバー、または型の宣言、インターフェイス宣言で宣言されているすべてのメンバーは、セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="60db1-419">場合`M`、インターフェイス宣言で宣言されているすべてのメソッド以外のメンバーは、セット、およびと同じシグネチャを持つすべてのメソッドから削除されますが、メソッド、`M`宣言インターフェイスの宣言は、セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="60db1-420">最後に、非表示のメンバーを削除すると、検索の結果が決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="60db1-421">セット メソッドではない 1 つのメンバーの場合、このメンバーは、参照の結果です。</span><span class="sxs-lookup"><span data-stu-id="60db1-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="60db1-422">それ以外の場合、セットにメソッドのみが含まれている場合、参照の結果はメソッドのこのグループにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="60db1-423">それ以外の場合、参照があいまいであり、バインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-424">型パラメーターとのインターフェイス以外の型のメンバーの検索と厳密な単一継承インターフェイス メンバーの検索 (継承チェーン内の各インターフェイスが厳密に 0 個または 1 つの直接基底インターフェイス)、参照ルールの影響は、同じ名前またはシグネチャを持つメンバーの非表示にする基本メンバーを派生するだけです。</span><span class="sxs-lookup"><span data-stu-id="60db1-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="60db1-425">このような単一継承の参照があいまいなことはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="60db1-426">多重継承インターフェイス メンバー検索から発生する可能性のあいまいさが記載されて[インターフェイス メンバーへのアクセス](interfaces.md#interface-member-access)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="60db1-427">基本データ型</span><span class="sxs-lookup"><span data-stu-id="60db1-427">Base types</span></span>

<span data-ttu-id="60db1-428">メンバーの検索、一種の目的で`T`次の基本型を持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="60db1-429">場合`T`は`object`、し`T`基本データ型がありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="60db1-430">場合`T`は、 *enum_type*の基本型`T`クラス型である`System.Enum`、 `System.ValueType`、および`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="60db1-431">場合`T`は、 *struct_type*の基本型`T`クラス型である`System.ValueType`と`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="60db1-432">場合`T`は、 *class_type*の基本型`T`の基本クラス`T`、クラス型を含む`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="60db1-433">場合`T`は、 *interface_type*の基本型`T`の基本インターフェイス`T`クラス型と`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="60db1-434">場合`T`は、 *array_type*の基本型`T`クラス型である`System.Array`と`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="60db1-435">場合`T`は、 *delegate_type*の基本型`T`クラス型である`System.Delegate`と`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="60db1-436">関数メンバー</span><span class="sxs-lookup"><span data-stu-id="60db1-436">Function members</span></span>

<span data-ttu-id="60db1-437">関数メンバーは、実行可能なステートメントが含まれているメンバーです。</span><span class="sxs-lookup"><span data-stu-id="60db1-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="60db1-438">関数メンバーは型のメンバーでは常に、名前空間のメンバーであることはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="60db1-439">C# 関数メンバーの次のカテゴリを定義します。</span><span class="sxs-lookup"><span data-stu-id="60db1-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="60db1-440">メソッド</span><span class="sxs-lookup"><span data-stu-id="60db1-440">Methods</span></span>
*  <span data-ttu-id="60db1-441">プロパティ</span><span class="sxs-lookup"><span data-stu-id="60db1-441">Properties</span></span>
*  <span data-ttu-id="60db1-442">イベント</span><span class="sxs-lookup"><span data-stu-id="60db1-442">Events</span></span>
*  <span data-ttu-id="60db1-443">インデクサー</span><span class="sxs-lookup"><span data-stu-id="60db1-443">Indexers</span></span>
*  <span data-ttu-id="60db1-444">ユーザー定義演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-444">User-defined operators</span></span>
*  <span data-ttu-id="60db1-445">インスタンス コンス トラクター</span><span class="sxs-lookup"><span data-stu-id="60db1-445">Instance constructors</span></span>
*  <span data-ttu-id="60db1-446">静的コンストラクター</span><span class="sxs-lookup"><span data-stu-id="60db1-446">Static constructors</span></span>
*  <span data-ttu-id="60db1-447">デストラクター</span><span class="sxs-lookup"><span data-stu-id="60db1-447">Destructors</span></span>

<span data-ttu-id="60db1-448">デストラクター (これは、明示的に呼び出すことはできません)、静的コンス トラクターを除く、関数メンバーに含まれているステートメントは、関数メンバーの呼び出しを通じて実行されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="60db1-449">関数メンバーの呼び出しを記述するための実際の構文は、特定の関数メンバーのカテゴリに依存します。</span><span class="sxs-lookup"><span data-stu-id="60db1-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="60db1-450">引数リスト ([引数リスト](expressions.md#argument-lists)) 関数メンバーの呼び出しでは、実際の値または変数参照、関数メンバーのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="60db1-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="60db1-451">ジェネリック メソッドの呼び出しは、メソッドに渡す型引数のセットを決定する型の推定を採用できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="60db1-452">このプロセスについては、「[型推論](expressions.md#type-inference)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="60db1-453">メソッド、インデクサー、演算子、およびインスタンス コンス トラクターの呼び出しは、候補の関数メンバーを呼び出す一連の判断するためにオーバー ロードの解決を使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="60db1-454">このプロセスについては、「[オーバー ロードの解決](expressions.md#overload-resolution)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="60db1-455">バインディング時に特定の関数メンバーを特定したら、可能性があるオーバー ロードの解決、関数メンバーを呼び出して実際の実行時プロセスがで説明されている[コンパイル時の動的なオーバー ロードの解決のチェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="60db1-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="60db1-456">次の表では、明示的に呼び出される関数メンバーの 6 つのカテゴリに関連する構成要素で発生する処理をまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="60db1-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="60db1-457">表に、 `e`、 `x`、 `y`、および`value`変数または値として分類される式を示す`T`型として分類される式を示します`F`メソッドをおよびの単純な名前を指定`P`プロパティの単純な名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="60db1-458">__コンス トラクター__</span><span class="sxs-lookup"><span data-stu-id="60db1-458">__Construct__</span></span>     | <span data-ttu-id="60db1-459">__例__</span><span class="sxs-lookup"><span data-stu-id="60db1-459">__Example__</span></span>    | <span data-ttu-id="60db1-460">__説明__</span><span class="sxs-lookup"><span data-stu-id="60db1-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="60db1-461">メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="60db1-462">最適な方法を選択するオーバー ロードの解決が適用される`F`包含クラスまたは構造体でします。</span><span class="sxs-lookup"><span data-stu-id="60db1-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="60db1-463">引数リストを持つメソッドが呼び出される`(x,y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="60db1-464">メソッドではない場合`static`、インスタンス式が`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="60db1-465">最適な方法を選択するオーバー ロードの解決が適用される`F`クラスまたは構造体で`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="60db1-466">メソッドではない場合、バインド時のエラーが発生する`static`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="60db1-467">引数リストを持つメソッドが呼び出される`(x,y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="60db1-468">クラス、構造体、またはインターフェイスの型で指定された F の最適な方法を選択するオーバー ロードの解決が適用される`e`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="60db1-469">メソッドの場合、バインド時のエラーが発生する`static`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="60db1-470">インスタンス式が、このメソッドは`e`および引数リスト`(x,y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="60db1-471">「プロパティ アクセス」</span><span class="sxs-lookup"><span data-stu-id="60db1-471">Property access</span></span>   | `P`            | <span data-ttu-id="60db1-472">`get`プロパティのアクセサー`P`包含クラスまたは構造体でが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="60db1-473">コンパイル時エラーが発生します`P`は書き込み専用です。</span><span class="sxs-lookup"><span data-stu-id="60db1-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="60db1-474">場合`P`ない`static`、インスタンス式が`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="60db1-475">`set`プロパティのアクセサー`P`包含クラスまたは構造体では引数リストによって呼び出される`(value)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="60db1-476">コンパイル時エラーが発生します`P`は読み取り専用です。</span><span class="sxs-lookup"><span data-stu-id="60db1-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="60db1-477">場合`P`ない`static`、インスタンス式が`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="60db1-478">`get`プロパティのアクセサー`P`クラスまたは構造体で`T`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="60db1-479">コンパイル時エラーが発生します`P`ない`static`場合`P`は書き込み専用です。</span><span class="sxs-lookup"><span data-stu-id="60db1-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="60db1-480">`set`プロパティのアクセサー`P`クラスまたは構造体で`T`が呼び出されると、引数リスト`(value)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="60db1-481">コンパイル時エラーが発生します`P`ない`static`場合`P`は読み取り専用です。</span><span class="sxs-lookup"><span data-stu-id="60db1-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="60db1-482">`get`プロパティのアクセサー`P`クラス、構造体、またはインターフェイスの型で指定された`e`が呼び出されるインスタンス式と`e`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="60db1-483">バインディング時のエラーが発生します`P`は`static`場合`P`は書き込み専用です。</span><span class="sxs-lookup"><span data-stu-id="60db1-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="60db1-484">`set`プロパティのアクセサー`P`クラス、構造体、またはインターフェイスの型で指定された`e`が呼び出されるインスタンス式と`e`および引数リスト`(value)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="60db1-485">バインディング時のエラーが発生します`P`は`static`場合`P`は読み取り専用です。</span><span class="sxs-lookup"><span data-stu-id="60db1-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="60db1-486">イベントへのアクセス</span><span class="sxs-lookup"><span data-stu-id="60db1-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="60db1-487">`add`イベントのアクセサー`E`包含クラスまたは構造体でが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="60db1-488">場合`E`は静的でないインスタンス式が`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="60db1-489">`remove`イベントのアクセサー`E`包含クラスまたは構造体でが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="60db1-490">場合`E`は静的でないインスタンス式が`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="60db1-491">`add`イベントのアクセサー`E`クラスまたは構造体で`T`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="60db1-492">バインディング時のエラーが発生します`E`は静的でありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="60db1-493">`remove`イベントのアクセサー`E`クラスまたは構造体で`T`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="60db1-494">バインディング時のエラーが発生します`E`は静的でありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="60db1-495">`add`イベントのアクセサー`E`クラス、構造体、またはインターフェイスの型で指定された`e`が呼び出されるインスタンス式と`e`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="60db1-496">バインディング時のエラーが発生します`E`は静的です。</span><span class="sxs-lookup"><span data-stu-id="60db1-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="60db1-497">`remove`イベントのアクセサー`E`クラス、構造体、またはインターフェイスの型で指定された`e`が呼び出されるインスタンス式と`e`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="60db1-498">バインディング時のエラーが発生します`E`は静的です。</span><span class="sxs-lookup"><span data-stu-id="60db1-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="60db1-499">インデクサーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="60db1-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="60db1-500">クラス、構造体、または電子メールの種類によって指定されたインターフェイスで最適なインデクサーを選択するのには、オーバー ロードの解決が適用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="60db1-501">`get`インスタンス式が、インデクサーのアクセサーが呼び出される`e`および引数リスト`(x,y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="60db1-502">インデクサーが書き込み専用の場合は、バインド エラーになります。</span><span class="sxs-lookup"><span data-stu-id="60db1-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="60db1-503">クラス、構造体、またはインターフェイスの型で指定された最適なインデクサーを選択するオーバー ロードの解決が適用される`e`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="60db1-504">`set`インスタンス式が、インデクサーのアクセサーが呼び出される`e`および引数リスト`(x,y,value)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="60db1-505">バインディング エラーは、インデクサーは読み取り専用である場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="60db1-506">演算子の呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="60db1-507">クラスまたは構造体の型で指定されたで最善の単項演算子を選択するオーバー ロードの解決が適用される`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="60db1-508">引数リストで選択した演算子が呼び出される`(x)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="60db1-509">クラスまたは構造体の型で指定された、最適な二項演算子を選択するオーバー ロードの解決が適用される`x`と`y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="60db1-510">引数リストで選択した演算子が呼び出される`(x,y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="60db1-511">インスタンス コンス トラクターの呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="60db1-512">クラスまたは構造体で最適なインスタンス コンス トラクターを選択するオーバー ロードの解決が適用される`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="60db1-513">インスタンス コンス トラクターが呼び出される引数リストと`(x,y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="60db1-514">引数リスト</span><span class="sxs-lookup"><span data-stu-id="60db1-514">Argument lists</span></span>

<span data-ttu-id="60db1-515">すべての関数メンバーおよびデリゲートの呼び出しには、関数メンバーのパラメーターの実際の値または変数参照を提供する引数リストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="60db1-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="60db1-516">関数メンバーの呼び出しの引数リストを指定する構文は、関数メンバーのカテゴリによって異なります。</span><span class="sxs-lookup"><span data-stu-id="60db1-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="60db1-517">インスタンスのコンス トラクター、メソッド、インデクサー、およびデリゲートを引数は、 *argument_list*以下の説明に従って、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="60db1-518">インデクサーの場合を呼び出すときに、`set`アクセサー、さらに含まれています、代入演算子の右側のオペランドとして指定された式の引数リスト。</span><span class="sxs-lookup"><span data-stu-id="60db1-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="60db1-519">プロパティについては、引数リストが空を呼び出すときに、`get`アクセサーを呼び出すときに、代入演算子の右側のオペランドとして指定された式で構成されて、`set`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="60db1-520">右側のオペランドとして指定された式の引数リストは、イベントの場合、`+=`または`-=`演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="60db1-521">ユーザー定義演算子では、引数リストは、単項演算子の 1 つのオペランドまたは二項演算子の 2 つのオペランドで構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="60db1-522">プロパティの引数 ([プロパティ](classes.md#properties))、イベント ([イベント](classes.md#events))、およびユーザー定義演算子 ([演算子](classes.md#operators)) は、常に値パラメーターとして渡されます ([パラメーターの値](classes.md#value-parameters))。</span><span class="sxs-lookup"><span data-stu-id="60db1-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="60db1-523">インデクサーの引数 ([インデクサー](classes.md#indexers)) は、常に値パラメーターとして渡されます ([パラメーターの値](classes.md#value-parameters)) またはパラメーターの配列 ([パラメーター配列](classes.md#parameter-arrays))。</span><span class="sxs-lookup"><span data-stu-id="60db1-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="60db1-524">これらの関数メンバーのカテゴリでは、パラメーターの参照を出力パラメーターはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="60db1-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="60db1-525">インスタンス コンス トラクター、メソッド、インデクサー、またはデリゲート呼び出しの引数として指定されます、 *argument_list*:</span><span class="sxs-lookup"><span data-stu-id="60db1-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

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

<span data-ttu-id="60db1-526">*Argument_list* 1 つまたは複数から成る*引数*s、コンマで区切られました。</span><span class="sxs-lookup"><span data-stu-id="60db1-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="60db1-527">省略可能なは、各引数*argument_name*続けて、 *argument_value*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="60db1-528">*引数*で、 *argument_name*と呼ばれます、***名前付き引数***であるのに対し、*引数*せず、 *argument_name*は、***位置指定引数***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="60db1-529">位置指定引数の名前付き引数の後に表示されるエラーは、 *argument_list*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="60db1-530">*Argument_value*形式は次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="60db1-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="60db1-531">*式*を示す値を持つパラメーターとして渡される引数 ([パラメーターの値](classes.md#value-parameters))。</span><span class="sxs-lookup"><span data-stu-id="60db1-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="60db1-532">キーワード`ref`続けて、 *variable_reference* ([変数参照](variables.md#variable-references))、参照パラメーターとして渡される引数を示す ([パラメーターを参照](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="60db1-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="60db1-533">変数を明示的に代入する必要があります ([確実な代入](variables.md#definite-assignment)) 前に、参照パラメーターとして渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="60db1-534">キーワード`out`続けて、 *variable_reference* ([変数参照](variables.md#variable-references))、出力パラメーターとして渡される引数を示す ([出力パラメーター](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="60db1-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="60db1-535">変数が確実に割り当てられていると見なされます ([確実な代入](variables.md#definite-assignment)) 次の変数が出力パラメーターとして渡された関数メンバーの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="60db1-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="60db1-536">対応するパラメーター</span><span class="sxs-lookup"><span data-stu-id="60db1-536">Corresponding parameters</span></span>

<span data-ttu-id="60db1-537">引数リストの各引数の関数メンバーまたは呼び出されるデリゲートに対応するパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="60db1-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="60db1-538">次で使用されるパラメーターのリストは、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="60db1-539">仮想メソッドとクラスで定義されているインデクサーでは、パラメーター リストの最も固有の宣言からピッキングまたは以降で、受信側の静的な型とその基本クラスを検索、関数メンバーのオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="60db1-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="60db1-540">パラメーター リストを取得するインターフェイスのメソッドとインデクサーはインターフェイス型で始まると、基本インターフェイスを検索、メンバーの最も固有の定義を形成します。</span><span class="sxs-lookup"><span data-stu-id="60db1-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="60db1-541">一意のパラメーター リストが見つからないかどうか、アクセスできない名前と省略可能なパラメーターなしのパラメーター リストが構築するための呼び出しが名前付きパラメーターを使用して、または省略可能な引数を省略することはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="60db1-542">部分メソッドの定義の部分メソッド宣言のパラメーター リストが使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="60db1-543">その他のすべての関数メンバーとデリゲートを使用する 1 つのパラメーター リストだけがあります。</span><span class="sxs-lookup"><span data-stu-id="60db1-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="60db1-544">引数またはパラメーターの位置は、引数または引数のリストまたはパラメーター リスト内の前のパラメーターの数として定義されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="60db1-545">関数メンバーの引数に対応するパラメーターは、次のように確立されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="60db1-546">引数、 *argument_list*インスタンス コンス トラクター、メソッド、インデクサー、およびデリゲートの。</span><span class="sxs-lookup"><span data-stu-id="60db1-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="60db1-547">パラメーター リスト内の同じ位置に固定のパラメーターが発生した位置指定引数は、そのパラメーターに対応します。</span><span class="sxs-lookup"><span data-stu-id="60db1-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="60db1-548">標準形式で呼び出されたパラメーター配列を持つ関数メンバーの位置指定引数は、パラメーター リストの同じ位置にある必要がありますをパラメーター配列に対応します。</span><span class="sxs-lookup"><span data-stu-id="60db1-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="60db1-549">拡張形式、場所、パラメーター リスト内の同じ位置で固定のパラメーターは行われませんで呼び出されたパラメーター配列を持つ関数メンバーの位置指定引数は、パラメーター配列内の要素に対応します。</span><span class="sxs-lookup"><span data-stu-id="60db1-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="60db1-550">名前付き引数は、パラメーター リスト内の同じ名前のパラメーターに対応します。</span><span class="sxs-lookup"><span data-stu-id="60db1-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="60db1-551">インデクサーの場合を呼び出すときに、`set`アクセサー、暗黙に対応する代入演算子の右側のオペランドとして指定された式`value`のパラメーター、`set`アクセサーの宣言。</span><span class="sxs-lookup"><span data-stu-id="60db1-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="60db1-552">呼び出すときに、プロパティの`get`ありますアクセサーに引数がありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="60db1-553">呼び出すときに、`set`アクセサー、暗黙に対応する代入演算子の右側のオペランドとして指定された式`value`のパラメーター、`set`アクセサーの宣言。</span><span class="sxs-lookup"><span data-stu-id="60db1-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="60db1-554">(変換を含む) ユーザー定義の単項演算子の 1 つのオペランドは、演算子の宣言の 1 つのパラメーターに対応します。</span><span class="sxs-lookup"><span data-stu-id="60db1-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="60db1-555">二項演算子のユーザー定義の最初のパラメーターに対応する左のオペランドと右のオペランドが演算子の宣言の 2 番目のパラメーターに対応しています。</span><span class="sxs-lookup"><span data-stu-id="60db1-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="60db1-556">引数リストの実行時の評価</span><span class="sxs-lookup"><span data-stu-id="60db1-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="60db1-557">関数メンバーの呼び出しの実行時の処理中に ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))、式または引数リストの変数の参照として、左から順番に評価されます次に示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="60db1-558">引数式の評価の値を持つパラメーター、および暗黙的な変換 ([暗黙的な変換](conversions.md#implicit-conversions)) の対応するパラメーター型が実行されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="60db1-559">結果の値では、関数メンバーの呼び出しで値パラメーターの初期値になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="60db1-560">参照または出力パラメーターの場合は、変数参照が評価され、結果ストレージの場所は、関数メンバーの呼び出しでパラメーターによって表される記憶域の場所になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="60db1-561">参照または出力パラメーターとして指定された変数の参照の配列要素である場合、 *reference_type*配列の要素の型がパラメーターの型と同じであることを確認する実行時チェックが行われます。</span><span class="sxs-lookup"><span data-stu-id="60db1-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="60db1-562">このチェックに失敗した場合、`System.ArrayTypeMismatchException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="60db1-563">メソッド、インデクサー、およびインスタンス コンス トラクターはパラメーター配列の最も右にあるパラメーターを宣言できます ([パラメーター配列](classes.md#parameter-arrays))。</span><span class="sxs-lookup"><span data-stu-id="60db1-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="60db1-564">このような関数メンバーは、標準形式、またはどちらが該当するによって、拡張の形式で呼び出される ([適用可能な関数メンバー](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="60db1-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="60db1-565">パラメーター配列を持つ関数メンバーが、標準形式で呼び出されると、パラメーター配列の指定された引数が暗黙的に変換可能である 1 つの式をある必要があります ([暗黙的な変換](conversions.md#implicit-conversions)) に、パラメーター配列の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="60db1-566">この場合、パラメーター配列は、値を持つパラメーターとまったく同じように機能します。</span><span class="sxs-lookup"><span data-stu-id="60db1-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="60db1-567">拡張形式でパラメーター配列を持つ関数メンバーが呼び出されると、呼び出しがそれぞれの引数が、暗黙的に変換する式は、パラメーター配列の 0 個以上の位置指定引数を指定する必要があります ([暗黙の型変換](conversions.md#implicit-conversions)) パラメーターの配列の要素の型にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="60db1-568">ここでは、呼び出し引数の数に対応する長さを持つパラメーターの配列型のインスタンスを作成します。 指定された引数に値を使用して、配列インスタンスの要素を初期化し、実際に新しく作成された配列のインスタンスを使用引数。</span><span class="sxs-lookup"><span data-stu-id="60db1-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="60db1-569">引数リストの式は常に記述されている順序で評価します。</span><span class="sxs-lookup"><span data-stu-id="60db1-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="60db1-570">例では、そのため、</span><span class="sxs-lookup"><span data-stu-id="60db1-570">Thus, the example</span></span>
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
<span data-ttu-id="60db1-571">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-571">produces the output</span></span>
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="60db1-572">配列の共変性規則 ([配列の共変性](arrays.md#array-covariance)) 配列型の値を許可`A[]`配列型のインスタンスへの参照である`B[]`から暗黙の参照変換が存在する限り、`B`に`A`.</span><span class="sxs-lookup"><span data-stu-id="60db1-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="60db1-573">配列の要素のときに、これらの規則のため、 *reference_type*が渡される参照または出力パラメーターとして、実行時チェックが配列の実際の要素の型が、パラメーターの場合と同じであることを確認するために必要です。</span><span class="sxs-lookup"><span data-stu-id="60db1-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="60db1-574">例</span><span class="sxs-lookup"><span data-stu-id="60db1-574">In the example</span></span>
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
<span data-ttu-id="60db1-575">2 番目の呼び出しの`F`により、`System.ArrayTypeMismatchException`が、実際の要素の入力のためにスローされる`b`は`string`および not `object`。</span><span class="sxs-lookup"><span data-stu-id="60db1-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="60db1-576">場合と同様、配列初期化子で配列作成式拡張形式でパラメーター配列を持つ関数メンバーが呼び出されると、呼び出しは処理されます ([配列作成式](expressions.md#array-creation-expressions)) 周囲に挿入されて、拡張パラメーター。</span><span class="sxs-lookup"><span data-stu-id="60db1-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="60db1-577">たとえば、宣言について考えます。</span><span class="sxs-lookup"><span data-stu-id="60db1-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="60db1-578">メソッドの拡張形式の次の呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="60db1-579">正確に対応しています</span><span class="sxs-lookup"><span data-stu-id="60db1-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="60db1-580">具体的には、パラメーター配列の指定された 0 個の引数がある場合、空の配列が作成されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="60db1-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="60db1-581">対応する省略可能なパラメーターを持つ関数メンバーからは、引数が省略された場合、関数メンバー宣言の既定の引数は暗黙的に渡されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="60db1-582">これらの定数は常に、ための評価は、残りの引数の評価順序は影響しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="60db1-583">型の推論</span><span class="sxs-lookup"><span data-stu-id="60db1-583">Type inference</span></span>

<span data-ttu-id="60db1-584">型引数を指定せずにジェネリック メソッドが呼び出されたときに、***型推論***プロセスは呼び出しの引数の型を推論しようとしています。</span><span class="sxs-lookup"><span data-stu-id="60db1-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="60db1-585">型の推定の存在は、ジェネリック メソッドの呼び出しに使用する便利な構文を使用でき、プログラマの冗長の種類の情報を指定することを回避するためにです。</span><span class="sxs-lookup"><span data-stu-id="60db1-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="60db1-586">たとえば、メソッドの宣言を考えてみます。</span><span class="sxs-lookup"><span data-stu-id="60db1-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="60db1-587">呼び出すことは、`Choose`型引数を明示的に指定しないでメソッド。</span><span class="sxs-lookup"><span data-stu-id="60db1-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="60db1-588">型推論、型引数を通じて`int`と`string`メソッドに引数から決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="60db1-589">メソッドの呼び出しのバインド時の処理の一部として型の推定が発生します ([メソッドの呼び出し](expressions.md#method-invocations)) と前の呼び出しのオーバー ロードの解決手順に、を実施します。</span><span class="sxs-lookup"><span data-stu-id="60db1-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="60db1-590">特定のメソッドのグループがメソッドの呼び出しで指定して、型引数がメソッドの呼び出しの一部として指定されていない、ときに、型の推論はメソッド グループの各ジェネリック メソッドに適用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="60db1-591">型の推論に成功した場合、推論された型引数は、後続のオーバー ロードの解決用引数の種類を決定に使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="60db1-592">オーバー ロードの解決選択した場合、ジェネリック メソッドを呼び出すと、推論された型引数は、特定の呼び出しの実際の型引数として使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="60db1-593">特定のメソッドの型の推定が失敗した場合、そのメソッドはオーバー ロードの解決には参加しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="60db1-594">型の推定、自体の障害では、バインド時のエラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="60db1-595">ただし、多くの場合、リード、バインド時のエラーを適切なメソッドを検索するオーバー ロードの解決が、失敗したときにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="60db1-596">指定された引数の数が、メソッドのパラメーターの数よりも異なる場合、推論すぐに失敗します。</span><span class="sxs-lookup"><span data-stu-id="60db1-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="60db1-597">それ以外の場合、ジェネリック メソッドは、次のシグネチャであると仮定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="60db1-598">フォームのメソッドの呼び出しで`M(E1...Em)`型の推定のタスクは、一意の型引数を検索する`S1...Sn`型パラメーターの各`X1...Xn`ように呼び出し`M<S1...Sn>(E1...Em)`が有効になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="60db1-599">推論の処理中に、各型パラメーター`Xi`か*固定*特定の種類に`Si`または*fixed でない*関連付けられている一連の*境界*.</span><span class="sxs-lookup"><span data-stu-id="60db1-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="60db1-600">何らかの種類は、各範囲`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="60db1-601">最初に、各型変数`Xi`境界の空のセットを固定することはありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="60db1-602">型の推定は、フェーズで行われます。</span><span class="sxs-lookup"><span data-stu-id="60db1-602">Type inference takes place in phases.</span></span> <span data-ttu-id="60db1-603">各フェーズは前のフェーズの結果に基づいて複数の型の変数の型引数を推測しようとします。</span><span class="sxs-lookup"><span data-stu-id="60db1-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="60db1-604">最初のフェーズでは、2 番目のフェーズは、型の変数を特定の種類を修正し、境界をさらに推論は、境界の初期推論。</span><span class="sxs-lookup"><span data-stu-id="60db1-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="60db1-605">2 番目のフェーズがありますが何度も繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="60db1-606">*注:* 型の推論は行われますがジェネリック メソッドが呼び出されたときにだけでなく。</span><span class="sxs-lookup"><span data-stu-id="60db1-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="60db1-607">メソッド グループの変換の型の推定については、「[メソッド グループの変換の推論を入力](expressions.md#type-inference-for-conversion-of-method-groups)に記載されて、一連の式の最適な一般的な種類を検索および[一連の最適一般的な種類を検索します。式の](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="60db1-608">最初のフェーズ</span><span class="sxs-lookup"><span data-stu-id="60db1-608">The first phase</span></span>

<span data-ttu-id="60db1-609">各メソッドの引数の`Ei`:</span><span class="sxs-lookup"><span data-stu-id="60db1-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="60db1-610">場合`Ei`、匿名関数、*明示的なパラメーター型の推論*([明示的なパラメーター型の推論](expressions.md#explicit-parameter-type-inferences)) から行われる`Ei`に `Ti`</span><span class="sxs-lookup"><span data-stu-id="60db1-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="60db1-611">の場合`Ei`型を持つ`U`と`xi`値を持つパラメーターには、*下限の推論*される*から* `U` *に*`Ti`.</span><span class="sxs-lookup"><span data-stu-id="60db1-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="60db1-612">の場合`Ei`型を持つ`U`と`xi`は、`ref`または`out`パラメーター、*の推定が正確な*される*から* `U`*に*`Ti`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="60db1-613">それ以外の場合、この引数の推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="60db1-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="60db1-614">2 番目のフェーズ</span><span class="sxs-lookup"><span data-stu-id="60db1-614">The second phase</span></span>

<span data-ttu-id="60db1-615">2 番目のフェーズは、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="60db1-616">すべて*fixed でない*変数を入力`Xi`かどうか*依存*([依存](expressions.md#dependence))、`Xj`は固定 ([修正](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="60db1-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="60db1-617">このような型の変数が存在しない場合、すべて*fixed でない*入力変数`Xi`は*固定*は次のすべてを保持します。</span><span class="sxs-lookup"><span data-stu-id="60db1-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="60db1-618">少なくとも 1 つの型の変数がある`Xj`に依存します。 `Xi`</span><span class="sxs-lookup"><span data-stu-id="60db1-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="60db1-619">`Xi` 以外の空のセットを持つ境界</span><span class="sxs-lookup"><span data-stu-id="60db1-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="60db1-620">このような型の変数が存在しない場合にまだあります*fixed でない*変数の型、型推論は失敗します。</span><span class="sxs-lookup"><span data-stu-id="60db1-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="60db1-621">それ以外の場合、これ以上ない場合*fixed でない*型の変数の存在は、型の推定が成功するとします。</span><span class="sxs-lookup"><span data-stu-id="60db1-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="60db1-622">それ以外のすべての引数の`Ei`対応するパラメーターの型と`Ti`場所、*種類の出力*([出力タイプ](expressions.md#output-types)) が含まれて*fixed でない*変数を入力`Xj`が、*入力の種類*([入力の種類](expressions.md#input-types)) はない、*出力の型推論*([出力型の推論](expressions.md#output-type-inferences)) が行われた*から* `Ei` *に*`Ti`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="60db1-623">2 番目のフェーズが繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="60db1-624">入力の種類</span><span class="sxs-lookup"><span data-stu-id="60db1-624">Input types</span></span>

<span data-ttu-id="60db1-625">場合`E`メソッド グループ、または匿名関数を暗黙的に型指定と`T`がデリゲート型または式ツリー型のパラメーターの型をすべて`T`は*入力型*の`E` *型*`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="60db1-626">出力の種類</span><span class="sxs-lookup"><span data-stu-id="60db1-626">Output types</span></span>

<span data-ttu-id="60db1-627">場合`E`はメソッド グループ、または匿名関数と`T`がデリゲート型または式ツリー型の戻り値の型`T`は、*出力の種類の* `E` *型* `T`.</span><span class="sxs-lookup"><span data-stu-id="60db1-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="60db1-628">依存関係</span><span class="sxs-lookup"><span data-stu-id="60db1-628">Dependence</span></span>

<span data-ttu-id="60db1-629">*Fixed でない*型の変数`Xi`*に直接依存*、fixed でない型の変数`Xj`いくつかの引数の場合`Ek`型`Tk` `Xj`発生する、*入力型*の`Ek`型で`Tk`と`Xi`で発生する、*出力の種類*の`Ek`型`Tk`。</span><span class="sxs-lookup"><span data-stu-id="60db1-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="60db1-630">`Xj` *依存*`Xi`場合`Xj`*に直接依存*`Xi`場合`Xi`*に直接依存*`Xk`と`Xk`*異なります*`Xj`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="60db1-631">したがって、「依存」に直接「依存」の推移的、再帰しないクロージャ。</span><span class="sxs-lookup"><span data-stu-id="60db1-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="60db1-632">出力型の推論</span><span class="sxs-lookup"><span data-stu-id="60db1-632">Output type inferences</span></span>

<span data-ttu-id="60db1-633">*出力の型推論*される*から*式`E`*に*型`T`次のように。</span><span class="sxs-lookup"><span data-stu-id="60db1-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="60db1-634">場合`E`推論された戻り値の型を持つ匿名関数は、 `U` ([推定型を返す](expressions.md#inferred-return-type)) と`T`がデリゲート型または戻り値の型の式ツリー型`Tb`し*下限の推論*([下限の推論](expressions.md#lower-bound-inferences)) が行われた*から* `U` *に*`Tb`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="60db1-635">の場合`E`メソッド グループと`T`がデリゲート型またはパラメーターの型の式ツリー型`T1...Tk`型を返すと`Tb`、オーバー ロードの解決と`E`型`T1...Tk`が得られます、1 つの戻り値の型を持つメソッド`U`、*下限の推論*される*から* `U` *に*`Tb`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="60db1-636">の場合`E`型の式は、 `U`、、*下限の推論*される*から* `U` *に* `T`.</span><span class="sxs-lookup"><span data-stu-id="60db1-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="60db1-637">それ以外の場合、推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="60db1-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="60db1-638">明示的なパラメーター型の推論</span><span class="sxs-lookup"><span data-stu-id="60db1-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="60db1-639">*明示的なパラメーター型の推論*される*から*式`E`*に*型`T`次のように。</span><span class="sxs-lookup"><span data-stu-id="60db1-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="60db1-640">場合`E`パラメーターの型を明示的に型指定された匿名関数`U1...Uk`と`T`がデリゲート型またはパラメーターの型の式ツリー型`V1...Vk`各し`Ui`、*正確な推論*([推測の正確な](expressions.md#exact-inferences)) が行われた*から* `Ui` *に*、対応する`Vi`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="60db1-641">正確な推論</span><span class="sxs-lookup"><span data-stu-id="60db1-641">Exact inferences</span></span>

<span data-ttu-id="60db1-642">*の推定が正確な\*\*から*型`U`*に*型`V`次のようになります。</span><span class="sxs-lookup"><span data-stu-id="60db1-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="60db1-643">場合`V`の 1 つ、 *fixed でない*`Xi`し`U`の正確な境界のセットに追加されます`Xi`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="60db1-644">それ以外の場合、設定`V1...Vk`と`U1...Uk`は、次の場合のいずれかが当てはまる場合にチェックによって決まります。</span><span class="sxs-lookup"><span data-stu-id="60db1-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="60db1-645">`V` 配列型は、`V1[...]`と`U`配列型は、`U1[...]`同じランクの</span><span class="sxs-lookup"><span data-stu-id="60db1-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="60db1-646">`V` 種類は、`V1?`と`U`型です `U1?`</span><span class="sxs-lookup"><span data-stu-id="60db1-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="60db1-647">`V` 構築された型は、`C<V1...Vk>`と`U`は構築された型です。 `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="60db1-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="60db1-648">このような場合のいずれかが当てはまる場合、*の推定が正確な*される*から*各`Ui`*に*、対応する`Vi`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="60db1-649">それ以外の場合の推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="60db1-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="60db1-650">下限の推論</span><span class="sxs-lookup"><span data-stu-id="60db1-650">Lower-bound inferences</span></span>

<span data-ttu-id="60db1-651">A*下限の推論\*\*から*型`U`*に*型`V`次のようになります。</span><span class="sxs-lookup"><span data-stu-id="60db1-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="60db1-652">場合`V`の 1 つ、 *fixed でない*`Xi`し`U`の下限のセットに追加されます`Xi`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="60db1-653">の場合`V`型は、`V1?`と`U`型は、`U1?`から下限の境界の推論が行われる`U1`に`V1`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="60db1-654">それ以外の場合、設定`U1...Uk`と`V1...Vk`は、次の場合のいずれかが当てはまる場合にチェックによって決まります。</span><span class="sxs-lookup"><span data-stu-id="60db1-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="60db1-655">`V` 配列型は、`V1[...]`と`U`配列型は、 `U1[...]` (または有効な基本型が型パラメーター `U1[...]`) に同じランク</span><span class="sxs-lookup"><span data-stu-id="60db1-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="60db1-656">`V` 1 つ`IEnumerable<V1>`、`ICollection<V1>`または`IList<V1>`と`U`1 次元配列の型は、 `U1[]`(または有効な基本型が型パラメーター `U1[]`)</span><span class="sxs-lookup"><span data-stu-id="60db1-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="60db1-657">`V` 構築されたクラス、構造体、インターフェイスまたはデリゲートの型は、`C<V1...Vk>`一意の型があると`C<U1...Uk>`ように`U`(または、`U`が型パラメーター、その効果的な基底クラスまたはその有効なインターフェイス セットのすべてのメンバー) は、同じで (直接または間接的に) を継承または実装 (直接または間接的に)`C<U1...Uk>`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="60db1-658">(「一意性」制限が大文字と小文字のインターフェイスのことを意味`C<T> {} class U: C<X>, C<Y> {}`、推論がから推論するときに行われることはない`U`に`C<T>`ため`U1`可能性があります`X`または`Y`)。</span><span class="sxs-lookup"><span data-stu-id="60db1-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="60db1-659">このような場合のいずれかが当てはまる推論が行われる場合*から*各`Ui`*に*、対応する`Vi`次のように。</span><span class="sxs-lookup"><span data-stu-id="60db1-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="60db1-660">場合`Ui`は認識されない参照型であるため、*の推定が正確な*されます</span><span class="sxs-lookup"><span data-stu-id="60db1-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="60db1-661">の場合`U`が配列型、*下限の推論*されます</span><span class="sxs-lookup"><span data-stu-id="60db1-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="60db1-662">の場合`V`は`C<V1...Vk>`推論が、i 番目の型パラメーターに依存し、 `C`:</span><span class="sxs-lookup"><span data-stu-id="60db1-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="60db1-663">共変である場合、*下限の推論*されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="60db1-664">反変である場合、*上限推論*されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="60db1-665">バリアント型でない場合、*の推定が正確な*されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="60db1-666">それ以外の場合、推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="60db1-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="60db1-667">上限の推論</span><span class="sxs-lookup"><span data-stu-id="60db1-667">Upper-bound inferences</span></span>

<span data-ttu-id="60db1-668">*上限の推論\*\*から*型`U`*に*型`V`次のようになります。</span><span class="sxs-lookup"><span data-stu-id="60db1-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="60db1-669">場合`V`の 1 つ、 *fixed でない*`Xi`し`U`の上限のセットに追加されます`Xi`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="60db1-670">それ以外の場合、設定`V1...Vk`と`U1...Uk`は、次の場合のいずれかが当てはまる場合にチェックによって決まります。</span><span class="sxs-lookup"><span data-stu-id="60db1-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="60db1-671">`U` 配列型は、`U1[...]`と`V`配列型は、`V1[...]`同じランクの</span><span class="sxs-lookup"><span data-stu-id="60db1-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="60db1-672">`U` 1 つ`IEnumerable<Ue>`、`ICollection<Ue>`または`IList<Ue>`と`V`は 1 次元配列の型です。 `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="60db1-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="60db1-673">`U` 種類は、`U1?`と`V`型です `V1?`</span><span class="sxs-lookup"><span data-stu-id="60db1-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="60db1-674">`U` 構築されたクラス、構造体、インターフェイスまたはデリゲートの型は、`C<U1...Uk>`と`V`(直接または間接的に)、クラス、構造体、インターフェイスまたはデリゲートの型と一致する、(直接または間接的に) を継承または実装は、一意の型 `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="60db1-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="60db1-675">(「一意性」制限ことを意味がある場合`interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`、推論がから推論するときに行われることはない`C<U1>`に`V<Q>`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="60db1-676">推論が行われていない`U1`いずれかに`X<Q>`または`Y<Q>`)。</span><span class="sxs-lookup"><span data-stu-id="60db1-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="60db1-677">このような場合のいずれかが当てはまる推論が行われる場合*から*各`Ui`*に*、対応する`Vi`次のように。</span><span class="sxs-lookup"><span data-stu-id="60db1-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="60db1-678">場合`Ui`は認識されない参照型であるため、*の推定が正確な*されます</span><span class="sxs-lookup"><span data-stu-id="60db1-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="60db1-679">の場合`V`が配列型、*上限推論*されます</span><span class="sxs-lookup"><span data-stu-id="60db1-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="60db1-680">の場合`U`は`C<U1...Uk>`推論が、i 番目の型パラメーターに依存し、 `C`:</span><span class="sxs-lookup"><span data-stu-id="60db1-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="60db1-681">共変である場合、*上限推論*されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="60db1-682">反変である場合、*下限の推論*されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="60db1-683">バリアント型でない場合、*の推定が正確な*されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="60db1-684">それ以外の場合、推論は行われません。</span><span class="sxs-lookup"><span data-stu-id="60db1-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="60db1-685">修正</span><span class="sxs-lookup"><span data-stu-id="60db1-685">Fixing</span></span>

<span data-ttu-id="60db1-686">*Fixed でない*型の変数`Xi`境界のセットでは*固定*次のようにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="60db1-687">一連の*候補型*`Uj`の境界のセット内のすべての型のセットとしては、まず`Xi`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="60db1-688">各バインドを考察し`Xi`順番。各正確なバインドの`U`の`Xi`すべての種類`Uj`と同じされない`U`候補セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="60db1-689">各下限の境界の`U`の`Xi`すべての種類`Uj`があるが*いない*からの暗黙的な変換`U`候補セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="60db1-690">各上限の`U`の`Xi`すべての種類`Uj`からどの*いない*への暗黙的な変換`U`候補セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="60db1-691">残りの候補の種類の間での if`Uj`一意の型がある`V`が暗黙的な変換をすべて、その他の候補の種類にしから`Xi`に固定されて`V`。</span><span class="sxs-lookup"><span data-stu-id="60db1-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="60db1-692">それ以外の場合、型の推論は失敗します。</span><span class="sxs-lookup"><span data-stu-id="60db1-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="60db1-693">推論された戻り値の型</span><span class="sxs-lookup"><span data-stu-id="60db1-693">Inferred return type</span></span>

<span data-ttu-id="60db1-694">推論される匿名関数の型を返す`F`型推論とオーバー ロードの解決中に使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="60db1-695">すべてのパラメーター型がわかっていますが、いずれかが明示的に指定するための匿名関数の変換によって提供される、または推論型の推定をそれを囲むジェネリック中に、匿名関数に対する推論された戻り値の型を決定のみメソッドの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="60db1-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="60db1-696">***結果型を推論***は次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="60db1-697">場合、本文の`F`は、*式*型の場合の推定の結果の型を持つ`F`その式の種類です。</span><span class="sxs-lookup"><span data-stu-id="60db1-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="60db1-698">場合、本文の`F`は、*ブロック*とブロックの内の式のセット`return`ステートメントが最適な一般的な型`T`([式のセットの最適な一般的な種類を検索する](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) の推定の結果の型、`F`は`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="60db1-699">場合は、結果型を推論できません`F`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="60db1-700">***戻り値の型を推論***は次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="60db1-701">場合`F`async およびの本文は`F`nothing として分類される式は、([式の分類](expressions.md#expression-classifications))、または return ステートメントに式が、されていないステートメント ブロック、推論された戻り値の型が `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="60db1-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="60db1-702">場合`F`非同期であり、推論された結果型を持つ`T`、推論される戻り値の型が`System.Threading.Tasks.Task<T>`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="60db1-703">場合`F`では、非同期であり、推論された結果型を持つ`T`、推論される戻り値の型が`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="60db1-704">それ以外の場合の戻り値の型を推論できません`F`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="60db1-705">匿名関数に関連する型の推定の例に、`Select`で宣言されている拡張メソッド、`System.Linq.Enumerable`クラス。</span><span class="sxs-lookup"><span data-stu-id="60db1-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
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

<span data-ttu-id="60db1-706">仮定すると、`System.Linq`名前空間がインポートされた、`using`句では、クラスと`Customer`で、`Name`型のプロパティ`string`、`Select`メソッドを使用して、顧客の一覧の名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="60db1-707">拡張メソッドの呼び出し ([拡張メソッド呼び出し](expressions.md#extension-method-invocations)) の`Select`静的メソッドの呼び出しに呼び出しを書き直すことによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="60db1-708">型引数が明示的に指定されていないために、型の推定は、型引数の推論に使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="60db1-709">最初に、`customers`引数に関連する、`source`パラメーターへの推論`T`する`Customer`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="60db1-710">推論プロセスが、上記で説明したが、匿名関数を使用して入力`c`型が指定`Customer`と式`c.Name`の戻り値の型に関連する、`selector`パラメーターへの推論`S`にする`string`.</span><span class="sxs-lookup"><span data-stu-id="60db1-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="60db1-711">したがって、呼び出しと同じです。</span><span class="sxs-lookup"><span data-stu-id="60db1-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="60db1-712">され、結果は型の`IEnumerable<string>`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="60db1-713">次の例では、匿名関数の型の推定により、ジェネリック メソッドの呼び出しで引数の間を"flow"に型情報。</span><span class="sxs-lookup"><span data-stu-id="60db1-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="60db1-714">メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="60db1-715">呼び出しの推論を入力します。</span><span class="sxs-lookup"><span data-stu-id="60db1-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="60db1-716">次のように処理されます。最初に、引数`"1:15:30"`に関連する、`value`パラメーターへの推論`X`する`string`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="60db1-717">Then の場合は、最初の匿名関数のパラメーター `s`、推論された型が指定`string`と式`TimeSpan.Parse(s)`の戻り値の型に関連する`f1`推論するとき、`Y`する`System.TimeSpan`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="60db1-718">2 番目の匿名関数のパラメーターでは最後に、 `t`、推論された型が指定`System.TimeSpan`、および式`t.TotalSeconds`の戻り値の型に関連する`f2`推論するとき、`Z`する`double`。</span><span class="sxs-lookup"><span data-stu-id="60db1-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="60db1-719">したがって、呼び出しの結果は型`double`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="60db1-720">メソッド グループの変換の型の推論</span><span class="sxs-lookup"><span data-stu-id="60db1-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="60db1-721">ジェネリック メソッドの呼び出しと同様に、型の推論する必要がありますも時に適用される、メソッド グループ`M`を特定のデリゲート型に変換がジェネリック メソッドを含む`D`([メソッド グループ変換](conversions.md#method-group-conversions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="60db1-722">メソッドの指定</span><span class="sxs-lookup"><span data-stu-id="60db1-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="60db1-723">メソッド グループ`M`デリゲート型に割り当てられている`D`型の推定のタスクは、型引数を検索する`S1...Sn`ように式。</span><span class="sxs-lookup"><span data-stu-id="60db1-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="60db1-724">互換性のあるになります ([デリゲートの宣言](delegates.md#delegate-declarations)) と`D`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="60db1-725">ジェネリック メソッドの呼び出しの型推論アルゴリズムとは異なりこの場合がある引数だけ*型*、引数なし*式*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="60db1-726">具体的には、匿名関数がないため、推論の複数のフェーズの必要はありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="60db1-727">代わりに、すべて`Xi`と見なされます*fixed でない*、および*下限の推論*される*から*各引数の型`Uj`の`D`*に*対応するパラメーター型`Tj`の`M`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="60db1-728">いずれかの場合、`Xi`境界が見つからなかったため、型推論が失敗します。</span><span class="sxs-lookup"><span data-stu-id="60db1-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="60db1-729">それ以外の場合、すべて`Xi`は*固定*を対応する`Si`型の推定の結果であります。</span><span class="sxs-lookup"><span data-stu-id="60db1-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="60db1-730">一連の式の最適な一般的な種類を検索します。</span><span class="sxs-lookup"><span data-stu-id="60db1-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="60db1-731">場合によっては、共通の型を式のセットを推論する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="60db1-732">特定の暗黙的に型指定された配列の要素の型と匿名関数の戻り値の型で*ブロック*本文にこの方法であります。</span><span class="sxs-lookup"><span data-stu-id="60db1-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="60db1-733">直感的に、与えられた一連の式の`E1...Em`この推定は、メソッドの呼び出しに相当する必要があります</span><span class="sxs-lookup"><span data-stu-id="60db1-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="60db1-734">`Ei`引数として。</span><span class="sxs-lookup"><span data-stu-id="60db1-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="60db1-735">正確には、推論から始まって、 *fixed でない*型の変数`X`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="60db1-736">*出力型の推論*、割り当てられている*から*各`Ei`*に*`X`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="60db1-737">最後に、`X`は*固定*し、成功した場合、その結果、入力`S`は、式の結果として得られる最適な一般的な型です。</span><span class="sxs-lookup"><span data-stu-id="60db1-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="60db1-738">そのような場合`S`存在する場合、式は、最高の一般的な型であるありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="60db1-739">オーバー ロードの解決</span><span class="sxs-lookup"><span data-stu-id="60db1-739">Overload resolution</span></span>

<span data-ttu-id="60db1-740">オーバー ロードの解決は、指定された引数リストおよび候補関数メンバーのセットを起動する最適な関数メンバーを選択するためのバインド時メカニズムです。</span><span class="sxs-lookup"><span data-stu-id="60db1-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="60db1-741">オーバー ロードの解決は、c# の次の異なるコンテキストで呼び出す関数メンバーを選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="60db1-742">という名前のメソッドの呼び出し、 *invocation_expression* ([メソッドの呼び出し](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="60db1-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="60db1-743">名前付きインスタンス コンス トラクターの呼び出し、 *object_creation_expression* ([オブジェクト作成式](expressions.md#object-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="60db1-744">インデクサーのアクセサーでの呼び出し、 *element_access* ([要素へのアクセス](expressions.md#element-access))。</span><span class="sxs-lookup"><span data-stu-id="60db1-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="60db1-745">式で参照されている定義済みまたはユーザー定義演算子の呼び出し ([単項演算子のオーバー ロードの解決](expressions.md#unary-operator-overload-resolution)と[二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="60db1-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="60db1-746">前のセクションで詳しく説明されているように固有の方法で候補関数メンバーのセットと引数のリストを定義これらのコンテキストの各とします。</span><span class="sxs-lookup"><span data-stu-id="60db1-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="60db1-747">メソッドの呼び出しの候補のセットでマークされているメソッドが含まれませんなど`override`([メンバー ルックアップ](expressions.md#member-lookup)) との基本クラスのメソッドの候補場合は派生クラスで任意のメソッドは、適用可能な ([メソッドの呼び出し](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="60db1-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="60db1-748">関数メンバーの候補と引数リストを特定すると、最適な関数メンバーの選択は常に同じです。</span><span class="sxs-lookup"><span data-stu-id="60db1-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="60db1-749">該当する候補の関数メンバーのセットを指定するには、最善の関数メンバーであるセットが配置されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="60db1-750">セットの 1 つだけの関数メンバーの場合は、その関数のメンバーは最適な関数メンバーです。</span><span class="sxs-lookup"><span data-stu-id="60db1-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="60db1-751">最適な関数メンバーは 1 つの関数メンバーを関数の各メンバーは、の規則を使用して他のすべての関数メンバーと比較されるときに、指定した引数リストに対して他のすべての関数メンバーよりも優れていますが、それ以外の場合、 [最適な関数メンバー](expressions.md#better-function-member)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="60db1-752">他のすべての関数メンバーより 1 つの関数メンバーが一致しない場合は、関数メンバーの呼び出しがあいまいですし、およびバインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-753">次のセクションでは、用語の正確な意味を定義する***適用可能な関数メンバー***と***最適な関数メンバー***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="60db1-754">適用可能な関数メンバー</span><span class="sxs-lookup"><span data-stu-id="60db1-754">Applicable function member</span></span>

<span data-ttu-id="60db1-755">関数メンバーはモード、***適用可能な関数メンバー***引数リストに対して`A`が true で、次のすべての場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="60db1-756">各引数`A`関数メンバーの宣言でのパラメーターに対応しています」の説明に従って[対応するパラメーター](expressions.md#corresponding-parameters)引数に対応するない任意のパラメーターは省略可能なパラメーター。</span><span class="sxs-lookup"><span data-stu-id="60db1-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="60db1-757">各引数に`A`、渡す引数のパラメーター (つまり、値は、 `ref`、または`out`) は、対応するパラメーターのパラメーター渡しモードと同じですし、</span><span class="sxs-lookup"><span data-stu-id="60db1-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="60db1-758">値を持つパラメーターまたはパラメーターの配列では、暗黙的な変換 ([暗黙的な変換](conversions.md#implicit-conversions)) 引数から、対応するパラメーターの型に存在するか</span><span class="sxs-lookup"><span data-stu-id="60db1-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="60db1-759">`ref`または`out`パラメーターの引数の型は、対応するパラメーターの型と同じです。</span><span class="sxs-lookup"><span data-stu-id="60db1-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="60db1-760">結局、`ref`または`out`パラメーターが渡される引数の別名です。</span><span class="sxs-lookup"><span data-stu-id="60db1-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="60db1-761">パラメーター配列を含む関数メンバーの関数メンバーは、上記の規則が適切である場合に適用されると言われますその***正規形***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="60db1-762">パラメーター配列を含む関数のメンバーが、標準形式で適用されない場合は、関数メンバーがで該当する代わりに可能性、***フォーム展開***:</span><span class="sxs-lookup"><span data-stu-id="60db1-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="60db1-763">関数メンバーの宣言でパラメーターの配列を 0 に置き換えることにより拡張されたフォームを構築またはパラメーターの要素型の複数値パラメーターの配列など、引数リストの引数の数`A`合計と一致します。パラメーターの数。</span><span class="sxs-lookup"><span data-stu-id="60db1-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="60db1-764">場合`A`関数メンバーの宣言で固定のパラメーター数よりも少ない引数を持つ場合、関数メンバーの拡張の形式は構築できませんのでは適用されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="60db1-765">それ以外の場合、拡張の形式は、の各引数の場合適用`A`引数のパラメーター渡しモードは、対応するパラメーターのパラメーター渡しモードと同じですし、</span><span class="sxs-lookup"><span data-stu-id="60db1-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="60db1-766">固定値を持つパラメーターまたは拡張、暗黙的な変換によって作成された値を持つパラメーター ([暗黙的な変換](conversions.md#implicit-conversions)) 引数の型から、対応するパラメーターの型に存在するか、</span><span class="sxs-lookup"><span data-stu-id="60db1-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="60db1-767">`ref`または`out`パラメーターの引数の型は、対応するパラメーターの型と同じです。</span><span class="sxs-lookup"><span data-stu-id="60db1-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="60db1-768">最適な関数メンバー</span><span class="sxs-lookup"><span data-stu-id="60db1-768">Better function member</span></span>

<span data-ttu-id="60db1-769">最適な関数メンバーを決定するためには、式を含むだけ、引数自体、元の引数リストに表示される順序で取り除いた引数リスト A が構築されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="60db1-770">パラメーターは、各メンバーは、次のように構築されている候補関数の一覧です。</span><span class="sxs-lookup"><span data-stu-id="60db1-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="60db1-771">関数メンバーが展開された形式でのみ使用できる場合は、拡張の形式が使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="60db1-772">対応する引数なしで省略可能なパラメーターがパラメーター リストから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="60db1-773">パラメーターの順序は変更を対応する引数として、引数リスト内の同じ位置にあります。</span><span class="sxs-lookup"><span data-stu-id="60db1-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="60db1-774">引数リストを指定された`A`一連の引数の式の`{E1, E2, ..., En}`と 2 つの適用可能な関数メンバー`Mp`と`Mq`パラメーターの型で`{P1, P2, ..., Pn}`と`{Q1, Q2, ..., Qn}`、`Mp`として定義されている、***最適な関数メンバー***より`Mq`場合</span><span class="sxs-lookup"><span data-stu-id="60db1-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="60db1-775">各引数の暗黙の変換から`Ex`に`Qx`から暗黙の変換よりも適切でない`Ex`に`Px`と</span><span class="sxs-lookup"><span data-stu-id="60db1-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="60db1-776">少なくとも 1 つの引数からの変換の`Ex`に`Px`からの変換よりもをお勧め`Ex`に`Qx`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="60db1-777">場合、この評価を実行するときに`Mp`または`Mq`が展開されたフォームでは、該当する`Px`または`Qx`パラメーター リストの拡張の形式でパラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="60db1-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="60db1-778">場合は、パラメーター型シーケンス `{P1, P2, ..., Pn}`と`{Q1, Q2, ..., Qn}`は同等です (つまり各`Pi`が、対応する id 変換`Qi`)、良いを判断する順序で、次のリンク付け規則が適用されます関数のメンバー。</span><span class="sxs-lookup"><span data-stu-id="60db1-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="60db1-779">場合`Mp`は非ジェネリック メソッドと`Mq`はジェネリック メソッドでは、`Mp`よりは`Mq`。</span><span class="sxs-lookup"><span data-stu-id="60db1-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="60db1-780">の場合`Mp`オプションは、標準形式に該当し、`Mq`が、`params`配列し、は、拡張形式でのみ使用できる`Mp`よりも優れていますが`Mq`。</span><span class="sxs-lookup"><span data-stu-id="60db1-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="60db1-781">の場合`Mp`よりもパラメーターを宣言の詳細が`Mq`、し`Mp`よりは`Mq`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="60db1-782">これは、両方の方法がある場合に発生することが`params`配列と、拡張形式でのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="60db1-783">それ以外の場合のすべてのパラメーター`Mp`で少なくとも 1 つの省略可能なパラメーターの代わりに使用する既定の引数が必要がありますが、対応する引数を持つ`Mq`し`Mp`よりは`Mq`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="60db1-784">の場合`Mp`よりも具体的なパラメーターの型を持つ`Mq`、し`Mp`よりは`Mq`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="60db1-785">ように`{R1, R2, ..., Rn}`と`{S1, S2, ..., Sn}`のインスタンス化されていないと、展開されていないパラメーターの型を表す`Mp`と`Mq`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="60db1-786">`Mp`パラメーターの型がよりも具体的`Mq`の場合、各パラメーターの`Rx`よりも小さい固有ではない`Sx`と少なくとも 1 つのパラメーターの`Rx`よりも特定`Sx`:</span><span class="sxs-lookup"><span data-stu-id="60db1-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="60db1-787">型パラメーターは、非型パラメーターよりも小さい固有です。</span><span class="sxs-lookup"><span data-stu-id="60db1-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="60db1-788">再帰的に構築された型が別の構築された型 (型引数の同じ番号) を持つ場合、少なくとも 1 つの入力引数の指定がより具体的と型引数が、それ以外の対応する型の引数よりも限定しないよりも限定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="60db1-789">配列型は、最初の要素の型は、2 番目の要素の型よりも具体的である場合 (同じ次元数) で別の配列型より固有です。</span><span class="sxs-lookup"><span data-stu-id="60db1-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="60db1-790">それ以外の場合 1 つのメンバーは非リフトされた演算子と、リフトされた演算子、もう一方は、非リフトされた 1 つが向上します。</span><span class="sxs-lookup"><span data-stu-id="60db1-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="60db1-791">それ以外の場合、どちらの関数メンバーお勧めします。</span><span class="sxs-lookup"><span data-stu-id="60db1-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="60db1-792">式からより適切な変換</span><span class="sxs-lookup"><span data-stu-id="60db1-792">Better conversion from expression</span></span>

<span data-ttu-id="60db1-793">暗黙的な変換を指定`C1`式から変換`E`型に`T1`、および暗黙的な変換を`C2`式から変換`E`型に`T2`、 `C1`***変換の改善***より`C2`場合`E`は完全に一致しない`T2`し、次の少なくとも 1 つを保持します。</span><span class="sxs-lookup"><span data-stu-id="60db1-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="60db1-794">`E` 完全に一致する`T1`([式に正確に一致する](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="60db1-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="60db1-795">`T1` 優れた変換ターゲット`T2`([優れた変換ターゲット](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="60db1-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="60db1-796">完全に一致する式</span><span class="sxs-lookup"><span data-stu-id="60db1-796">Exactly matching Expression</span></span>

<span data-ttu-id="60db1-797">式が指定`E`と型`T`、`E`完全に一致する`T`次のいずれかが保持している場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="60db1-798">`E` 型を持つ`S`からの id 変換が存在して`S`に `T`</span><span class="sxs-lookup"><span data-stu-id="60db1-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="60db1-799">`E` 匿名関数、`T`デリゲート型は、`D`または式ツリー型`Expression<D>`し、次のいずれかを保持します。</span><span class="sxs-lookup"><span data-stu-id="60db1-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="60db1-800">推論された戻り値の型`X`が存在する`E`のパラメーター リストのコンテキストで`D`([推定型を返す](expressions.md#inferred-return-type)) からの id 変換が存在して`X`の戻り値の型に `D`</span><span class="sxs-lookup"><span data-stu-id="60db1-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="60db1-801">いずれか`E`は非同期ではないと`D`戻り値の型を持つ`Y`または`E`は async と`D`戻り値の型を持つ`Task<Y>`、し、次のいずれかを保持します。</span><span class="sxs-lookup"><span data-stu-id="60db1-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="60db1-802">本文`E`を完全に一致する式は、 `Y`</span><span class="sxs-lookup"><span data-stu-id="60db1-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="60db1-803">本文`E`を完全に一致するすべての return ステートメント返しますが、式、ステートメント ブロックには `Y`</span><span class="sxs-lookup"><span data-stu-id="60db1-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="60db1-804">変換対象の向上</span><span class="sxs-lookup"><span data-stu-id="60db1-804">Better conversion target</span></span>

<span data-ttu-id="60db1-805">2 つの種類を指定`T1`と`T2`、`T1`よりも優れた、変換対象`T2`場合からの暗黙的な変換なし`T2`に`T1`が存在して、次の少なくとも 1 つを保持します。</span><span class="sxs-lookup"><span data-stu-id="60db1-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="60db1-806">暗黙的な変換`T1`に`T2`が存在します。</span><span class="sxs-lookup"><span data-stu-id="60db1-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="60db1-807">`T1` デリゲート型は、`D1`または式ツリー型`Expression<D1>`、`T2`デリゲート型は、`D2`または式ツリー型`Expression<D2>`、`D1`戻り値の型を持つ`S1`のいずれかと、次の保留:</span><span class="sxs-lookup"><span data-stu-id="60db1-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="60db1-808">`D2` void を返す</span><span class="sxs-lookup"><span data-stu-id="60db1-808">`D2` is void returning</span></span>
   * <span data-ttu-id="60db1-809">`D2` 戻り値の型を持つ`S2`、および`S1`よりも優れた変換対象は、 `S2`</span><span class="sxs-lookup"><span data-stu-id="60db1-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="60db1-810">`T1` `Task<S1>`、`T2`は`Task<S2>`、および`S1`よりも優れた変換対象は、 `S2`</span><span class="sxs-lookup"><span data-stu-id="60db1-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="60db1-811">`T1` `S1`または`S1?`場所`S1`は符号付き整数型と`T2`は`S2`または`S2?`場所`S2`は符号なし整数型。</span><span class="sxs-lookup"><span data-stu-id="60db1-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="60db1-812">具体的には、次のように使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-812">Specifically:</span></span>
   * <span data-ttu-id="60db1-813">`S1` `sbyte`と`S2`は`byte`、 `ushort`、 `uint`、または `ulong`</span><span class="sxs-lookup"><span data-stu-id="60db1-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="60db1-814">`S1` `short`と`S2`は`ushort`、 `uint`、または `ulong`</span><span class="sxs-lookup"><span data-stu-id="60db1-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="60db1-815">`S1` `int`と`S2`は`uint`、または `ulong`</span><span class="sxs-lookup"><span data-stu-id="60db1-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="60db1-816">`S1` `long`と`S2`は `ulong`</span><span class="sxs-lookup"><span data-stu-id="60db1-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="60db1-817">ジェネリック クラスのオーバー ロード</span><span class="sxs-lookup"><span data-stu-id="60db1-817">Overloading in generic classes</span></span>

<span data-ttu-id="60db1-818">宣言された署名は、一意である必要があります、シグネチャが同じ結果を型引数の置換することができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="60db1-819">このような場合は、上記のオーバー ロードの解決の対フスェケェルェニェヒ規則は、特定のメンバーが選択されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="60db1-820">次の例では、有効であり、このルールに従って無効であるオーバー ロードを示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="60db1-821">コンパイル時の動的なオーバー ロードの解決の確認</span><span class="sxs-lookup"><span data-stu-id="60db1-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="60db1-822">最も動的にバインドされた操作の解決候補のセットはコンパイル時に既知ではありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="60db1-823">ただし、場合によっては候補の集合がコンパイル時に呼ばれるは。</span><span class="sxs-lookup"><span data-stu-id="60db1-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="60db1-824">動的引数を持つ静的メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="60db1-825">受信側が動的な式ではないインスタンス メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="60db1-826">インデクサー呼び出しの受信側が動的な式ではありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="60db1-827">動的引数を持つコンス トラクターの呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="60db1-828">このような場合は、制限付きのコンパイル時チェックが実行時にいずれかが適用可能性があるかどうかに表示するには、各候補に対して実行されます。このチェックは、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="60db1-829">部分型の推定:任意の型引数を型の引数に直接または間接的には依存しません`dynamic`推論のルールを使用して[型推論](expressions.md#type-inference)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="60db1-830">残りの型引数は既知ではありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="60db1-831">部分的な適用性チェック:適用性のチェックによる[適用可能な関数メンバー](expressions.md#applicable-function-member)、既知の型がないパラメーターは無視されますが、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="60db1-832">このテストに合格する候補がない、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="60db1-833">関数メンバーの呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-833">Function member invocation</span></span>

<span data-ttu-id="60db1-834">このセクションでは、特定の関数メンバーを呼び出すための実行時に発生するプロセスについて説明します。</span><span class="sxs-lookup"><span data-stu-id="60db1-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="60db1-835">バインディング時の処理の呼び出しは、場合によって適用することでオーバー ロードの解決、一連の関数メンバーの候補にするには、特定のメンバーが既に判断したと見なされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="60db1-836">呼び出しプロセスを記述するために、関数メンバーは、2 つのカテゴリに分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="60db1-837">静的な関数メンバー。</span><span class="sxs-lookup"><span data-stu-id="60db1-837">Static function members.</span></span> <span data-ttu-id="60db1-838">これらは、インスタンス コンス トラクター、静的メソッド、静的なプロパティ アクセサー、およびユーザー定義の演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="60db1-839">静的な関数メンバーは、非仮想では常にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="60db1-840">関数のインスタンス メンバーです。</span><span class="sxs-lookup"><span data-stu-id="60db1-840">Instance function members.</span></span> <span data-ttu-id="60db1-841">これらは、インスタンス メソッド、インスタンス プロパティ アクセサー、およびインデクサーのアクセサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="60db1-842">インスタンス関数メンバーは、仮想でないか、仮想、およびは、特定のインスタンスに常に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="60db1-843">インスタンスがインスタンス式によって計算され、関数はメンバー内でアクセス可能になります`this`([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="60db1-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="60db1-844">関数メンバーの呼び出しの実行時の処理は、次の手順で構成されていますここ`M`関数メンバーは、し、場合は、`M`インスタンス メンバー`E`インスタンス式です。</span><span class="sxs-lookup"><span data-stu-id="60db1-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="60db1-845">場合`M`は静的な関数メンバーです。</span><span class="sxs-lookup"><span data-stu-id="60db1-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="60db1-846">」の説明に従って、引数リストが評価される[引数リスト](expressions.md#argument-lists)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="60db1-847">`M` 呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-847">`M` is invoked.</span></span>

*  <span data-ttu-id="60db1-848">場合`M`インスタンス関数のメンバーで宣言、 *value_type*:</span><span class="sxs-lookup"><span data-stu-id="60db1-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="60db1-849">`E` 評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-849">`E` is evaluated.</span></span> <span data-ttu-id="60db1-850">この評価は、例外を発生させ場合、その後の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="60db1-851">場合`E`変数、その後の一時ローカル変数として分類されない`E`の型が作成されると値の`E`その変数に代入されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="60db1-852">`E` 一時ローカル変数への参照として再分類します。</span><span class="sxs-lookup"><span data-stu-id="60db1-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="60db1-853">一時変数のアクセシビリティが`this`内`M`が、その他の方法ではありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="60db1-854">したがって、場合にのみ`E`が true の変数は、呼び出し元が、変更を確認できますを`M`を`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="60db1-855">」の説明に従って、引数リストが評価される[引数リスト](expressions.md#argument-lists)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="60db1-856">`M` 呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-856">`M` is invoked.</span></span> <span data-ttu-id="60db1-857">によって参照される変数`E`によって参照される変数になります`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="60db1-858">場合`M`インスタンス関数のメンバーで宣言、 *reference_type*:</span><span class="sxs-lookup"><span data-stu-id="60db1-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="60db1-859">`E` 評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-859">`E` is evaluated.</span></span> <span data-ttu-id="60db1-860">この評価は、例外を発生させ場合、その後の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="60db1-861">」の説明に従って、引数リストが評価される[引数リスト](expressions.md#argument-lists)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="60db1-862">場合の種類`E`は、 *value_type*、ボックス化変換 ([ボックス化変換](types.md#boxing-conversions)) に変換する実行`E`を入力する`object`と`E`と見なされます型にする`object`で、次の手順。</span><span class="sxs-lookup"><span data-stu-id="60db1-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="60db1-863">この場合、`M`のメンバーである可能性がありますのみ`System.Object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="60db1-864">値`E`がチェックを有効にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="60db1-865">場合の値`E`は`null`、`System.NullReferenceException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="60db1-866">呼び出す関数メンバーの実装が決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="60db1-867">バインディング時の型の場合`E`インターフェイスで呼び出す関数メンバーの実装は、`M`によって参照されるインスタンスの実行時の型によって提供される`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="60db1-868">この関数のメンバーがインターフェイスのマッピング規則を適用して決定されます ([インターフェイス マップ](interfaces.md#interface-mapping)) の実装を決定する`M`によって参照されるインスタンスの実行時の型によって提供される`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="60db1-869">の場合`M`仮想関数のメンバーでは、呼び出す関数メンバーの実装は、`M`によって参照されるインスタンスの実行時の型によって提供される`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="60db1-870">この関数のメンバーが、最派生実装を決定するルールを適用して決定されます ([仮想メソッド](classes.md#virtual-methods)) の`M`によって参照されるインスタンスの実行時の型に関して`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="60db1-871">それ以外の場合、`M`非仮想関数メンバー、および関数メンバーを呼び出すが、`M`自体。</span><span class="sxs-lookup"><span data-stu-id="60db1-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="60db1-872">上記の手順で決定関数メンバーの実装が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="60db1-873">によって参照されるオブジェクト`E`によって参照されるオブジェクトになります`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="60db1-874">ボックス化されたインスタンス上の呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-874">Invocations on boxed instances</span></span>

<span data-ttu-id="60db1-875">関数メンバーの実装で、 *value_type*のボックス化されたインスタンスを呼び出すことができます*value_type*次の状況で。</span><span class="sxs-lookup"><span data-stu-id="60db1-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="60db1-876">関数メンバーの場合、`override`型から継承されたメソッドの`object`が型のインスタンス式から呼び出されると`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="60db1-877">関数メンバーがインターフェイスの関数メンバーの実装がのインスタンス式から呼び出される、 *interface_type*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="60db1-878">ときに関数メンバーはデリゲートを介して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="60db1-879">変数を含むこのような状況でボックス化されたインスタンスと見なされます、 *value_type*、この変数で参照される変数になり`this`関数メンバーの呼び出し内で。</span><span class="sxs-lookup"><span data-stu-id="60db1-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="60db1-880">具体的には、関数メンバーがボックス化されたインスタンスで呼び出されると、ボックス化されたインスタンスに含まれる値を変更する関数メンバーの可能なは、これは意味します。</span><span class="sxs-lookup"><span data-stu-id="60db1-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="60db1-881">基本式</span><span class="sxs-lookup"><span data-stu-id="60db1-881">Primary expressions</span></span>

<span data-ttu-id="60db1-882">一次式には、式の最も簡単なフォームが含まれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-882">Primary expressions include the simplest forms of expressions.</span></span>

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

<span data-ttu-id="60db1-883">一次式に分かれています*array_creation_expression*s と*primary_no_array_creation_expression*秒。</span><span class="sxs-lookup"><span data-stu-id="60db1-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="60db1-884">この方法では、配列作成式を扱う、と共に、他の式の単純なフォームを指定することではなくによりなどの混乱を招く可能性のあるコードを許可しないようにする文法</span><span class="sxs-lookup"><span data-stu-id="60db1-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="60db1-885">それ以外の場合として解釈されるします。</span><span class="sxs-lookup"><span data-stu-id="60db1-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="60db1-886">リテラル</span><span class="sxs-lookup"><span data-stu-id="60db1-886">Literals</span></span>

<span data-ttu-id="60db1-887">A *primary_expression*で構成される、*リテラル*([リテラル](lexical-structure.md#literals)) 値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="60db1-888">挿入文字列</span><span class="sxs-lookup"><span data-stu-id="60db1-888">Interpolated strings</span></span>

<span data-ttu-id="60db1-889">*Interpolated_string_expression*から成る、`$`記号の後に正規表現または逐語的文字列リテラル、穴の区切られ`{`と`}`式を囲みますおよび書式設定仕様。</span><span class="sxs-lookup"><span data-stu-id="60db1-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="60db1-890">補間文字列式の結果とは、 *interpolated_string_literal*が分割されている個々 のトークンに」の説明に従って[リテラル文字列の補間](lexical-structure.md#interpolated-string-literals)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

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

<span data-ttu-id="60db1-891">*Constant_expression* 、補間で暗黙的に変換をいる必要があります`int`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="60db1-892">*Interpolated_string_expression*値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="60db1-893">場合にすぐに変換されます`System.IFormattable`または`System.FormattableString`補間文字列の暗黙的な変換を使用 ([補間文字列の変換を暗黙的な](conversions.md#implicit-interpolated-string-conversions))、補間文字列式がその型。</span><span class="sxs-lookup"><span data-stu-id="60db1-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="60db1-894">それ以外の場合、型を持つ`string`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="60db1-895">補間文字列の型が場合`System.IFormattable`または`System.FormattableString`、意味の呼び出しは、 `System.Runtime.CompilerServices.FormattableStringFactory.Create`。</span><span class="sxs-lookup"><span data-stu-id="60db1-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="60db1-896">型の場合`string`、式の意味はへの呼び出し`string.Format`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="60db1-897">どちらの場合も、呼び出しの引数リストは、各補間のプレース ホルダーと引数のプレース ホルダーに対応する各式のリテラル書式指定文字列で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="60db1-898">形式の文字列リテラルは、次のように構築された、`N`で補間の数が、 *interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="60db1-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="60db1-899">場合、 *interpolated_regular_string_whole*または*interpolated_verbatim_string_whole*に依存して、`$`形式の文字列リテラルはそのトークンに署名します。</span><span class="sxs-lookup"><span data-stu-id="60db1-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="60db1-900">それ以外の場合、リテラルの書式指定文字列で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="60db1-901">最初、 *interpolated_regular_string_start*または*interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="60db1-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="60db1-902">各数値し`I`から`0`に`N-1`:</span><span class="sxs-lookup"><span data-stu-id="60db1-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="60db1-903">10 進表記 `I`</span><span class="sxs-lookup"><span data-stu-id="60db1-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="60db1-904">場合、対応する*補間*が、 *constant_expression*、`,`の値の 10 進表記 (コンマ) 後に、 *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="60db1-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="60db1-905">次に、 *interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_mid*または*interpolated_verbatim_string_end*直後に対応する補間します。</span><span class="sxs-lookup"><span data-stu-id="60db1-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="60db1-906">後続の引数は単に、*式*から、*補間*(ある場合) の順序で。</span><span class="sxs-lookup"><span data-stu-id="60db1-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="60db1-907">TODO: 例。</span><span class="sxs-lookup"><span data-stu-id="60db1-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="60db1-908">単純な名前</span><span class="sxs-lookup"><span data-stu-id="60db1-908">Simple names</span></span>

<span data-ttu-id="60db1-909">A *simple_name*型引数リストが続く必要に応じて、識別子で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="60db1-910">A *simple_name*形式のいずれかです`I`またはフォームの`I<A1,...,Ak>`ここで、`I`は、単一の識別子と`<A1,...,Ak>`は省略可能な*type_argument_list*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="60db1-911">ない場合*type_argument_list*が指定するを検討してください`K`を 0 にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="60db1-912">*Simple_name*が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="60db1-913">場合`K`は 0 です、 *simple_name*内に表示する、*ブロック*場合に、*ブロック*の (または、それを囲む*ブロック*の) ローカル変数の宣言領域 ([宣言](basic-concepts.md#declarations)) ローカル変数、パラメーターまたは名前の定数を含む `I`、 *simple_name* 、そのローカル変数を参照パラメーターまたは定数、変数または値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="60db1-914">場合`K`ゼロ、 *simple_name*ジェネリック メソッドの宣言の本文内に表示し、その宣言には、名前の型パラメーターが含まれている場合 `I`、 *simple_name*その型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="60db1-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="60db1-915">それ以外の場合、各インスタンス タイプ `T`([インスタンス型](classes.md#the-instance-type))、すぐ外側の型宣言のインスタンスの型を起動し、各外側のクラスまたは構造体のインスタンスの型を続行宣言 (ある場合):</span><span class="sxs-lookup"><span data-stu-id="60db1-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="60db1-916">場合`K`は 0 との宣言`T`名前の型パラメーターを含む `I`、 *simple_name*その型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="60db1-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="60db1-917">それ以外の場合、メンバーの検索 ([メンバー ルックアップ](expressions.md#member-lookup)) の`I`で`T`で`K` 型引数が一致するものが生成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="60db1-918">場合`T`すぐ外側のクラスまたは構造体の型のインスタンスの型は、検索でいずれかが識別や以上のメソッドは、結果は、メソッドのグループの関連付けられたインスタンス式がある`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="60db1-919">ジェネリック メソッドの呼び出しで使用される型引数リストが指定されている場合 ([メソッドの呼び出し](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="60db1-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="60db1-920">の場合`T`すぐ外側のクラスまたは構造体の型のインスタンスの型は、検索は、インスタンス メンバーを識別し、インスタンス コンス トラクター、インスタンス メソッドまたはインスタンス アクセサーの本文内の参照が発生した場合、メンバー アクセスと同じになります ([メンバー アクセス](expressions.md#member-access)) フォームの`this.I`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="60db1-921">これはのみに発生時に`K`は 0 です。</span><span class="sxs-lookup"><span data-stu-id="60db1-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="60db1-922">それ以外の場合、結果は、メンバー アクセスと同じ ([メンバー アクセス](expressions.md#member-access)) フォームの`T.I`または`T.I<A1,...,Ak>`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="60db1-923">ここでは、バインド エラーには、 *simple_name*インスタンス メンバーを参照します。</span><span class="sxs-lookup"><span data-stu-id="60db1-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="60db1-924">それ以外の名前空間ごとに `N`を名前空間を以降の*simple_name*発生すると、次の手順は、各名前空間 (存在する場合)、および末尾に、グローバル名前空間を続行します。エンティティが見つかるまで評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="60db1-925">場合`K`ゼロと`I`で名前空間の名前を指定 `N`、し。</span><span class="sxs-lookup"><span data-stu-id="60db1-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="60db1-926">場合、場所を*simple_name*が発生した名前空間の宣言で囲まれた`N`名前空間宣言が含まれています、 *extern_alias_directive*または*using_alias_directive*名前に関連付ける `I`型、または名前空間、 *simple_name*があいまいですし、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="60db1-927">それ以外の場合、 *simple_name*という名前の名前空間を指す`I`で`N`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="60db1-928">の場合`N`名前を持つアクセス可能な型が含まれています `I`と`K` し、パラメーターを入力します。</span><span class="sxs-lookup"><span data-stu-id="60db1-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="60db1-929">場合`K`は 0 と場所を*simple_name*が発生した名前空間の宣言で囲まれた`N`名前空間宣言が含まれています、 *extern_alias_directive*または*using_alias_directive*名前に関連付ける `I`型、または名前空間、 *simple_name*があいまいですし、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="60db1-930">それ以外の場合、 *namespace_or_type_name*は特定の型引数を使用して構築型を表します。</span><span class="sxs-lookup"><span data-stu-id="60db1-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="60db1-931">の場合、場所を、 *simple_name*が発生した名前空間の宣言で囲まれた `N`:</span><span class="sxs-lookup"><span data-stu-id="60db1-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="60db1-932">場合`K`がゼロで、名前空間宣言が含まれています、 *extern_alias_directive*または*using_alias_directive*名前に関連付ける `I`でインポートされた名前空間または型、 *simple_name*その名前空間または型を参照します。</span><span class="sxs-lookup"><span data-stu-id="60db1-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="60db1-933">それ以外の場合、名前空間と型の宣言をインポートした場合、 *using_namespace_directive*s と*using_static_directive*名前空間宣言の s がアクセス可能な型の 1 つだけを含めるか、拡張機能の非静的メンバーの名前を持つ `I`と`K` パラメーター、入力、 *simple_name*参照すると、その型またはメンバーの型引数を使用して構築します。</span><span class="sxs-lookup"><span data-stu-id="60db1-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="60db1-934">それ以外の場合、名前空間と型をインポートした場合、 *using_namespace_directive*名前空間の宣言の 1 つ以上のアクセス可能な型または名前を持つ拡張メソッドでは非静的メンバーを含む `I`と`K` パラメーター、入力、 *simple_name*があいまい、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="60db1-935">この全体の手順では、対応する手順の処理中に完全に並列に注意してください、 *namespace_or_type_name* ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names))。</span><span class="sxs-lookup"><span data-stu-id="60db1-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="60db1-936">それ以外の場合、 *simple_name*は未定義となり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="60db1-937">かっこで囲まれた式</span><span class="sxs-lookup"><span data-stu-id="60db1-937">Parenthesized expressions</span></span>

<span data-ttu-id="60db1-938">A *parenthesized_expression*から成る、*式*かっこで囲まれています。</span><span class="sxs-lookup"><span data-stu-id="60db1-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="60db1-939">A *parenthesized_expression*は評価することによって評価されます、*式*かっこで囲まれています。</span><span class="sxs-lookup"><span data-stu-id="60db1-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="60db1-940">場合、*式*かっこで囲ま表します名前空間または型、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="60db1-941">それ以外の場合の結果、 *parenthesized_expression*の格納されている評価の結果は、*式*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="60db1-942">メンバー アクセス。</span><span class="sxs-lookup"><span data-stu-id="60db1-942">Member access</span></span>

<span data-ttu-id="60db1-943">A *member_access*から成る、 *primary_expression*、 *predefined_type*、または*qualified_alias_member*"と、その後`.`"トークン、その後に、*識別子*、必要に応じてその後に、 *type_argument_list*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

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

<span data-ttu-id="60db1-944">*Qualified_alias_member*で定義されている運用[Namespace エイリアス修飾子](namespaces.md#namespace-alias-qualifiers)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="60db1-945">A *member_access*形式のいずれかです`E.I`またはフォームの`E.I<A1, ..., Ak>`ここで、`E`プライマリ式は、`I`は、単一の識別子と`<A1, ..., Ak>`は省略可能な*type_argument_list*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="60db1-946">ない場合*type_argument_list*が指定するを検討してください`K`を 0 にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="60db1-947">A *member_access*で、 *primary_expression*型の`dynamic`が動的にバインドされている ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="60db1-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="60db1-948">ここで、コンパイラが型のプロパティ アクセスにメンバー アクセスを分類`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="60db1-949">意味を調べるには、次の規則、 *member_access*ランタイムのコンパイル時の型ではなく、実行時の型を使用するのには、適用、 *primary_expression*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="60db1-950">このランタイムの分類は、メソッド グループにつながるかどうかは、メンバー アクセスである必要があります、 *primary_expression*の*invocation_expression*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="60db1-951">*Member_access*が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="60db1-952">場合`K`ゼロと`E`名前空間と`E`入れ子になった名前空間が含まれています `I`、結果はその名前空間。</span><span class="sxs-lookup"><span data-stu-id="60db1-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="60db1-953">の場合`E`名前空間と`E`名前を持つアクセス可能な型が含まれています `I`と`K`  、結果は指定された型引数で構築された型のパラメーターを入力します。</span><span class="sxs-lookup"><span data-stu-id="60db1-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="60db1-954">場合`E`は、 *predefined_type*または*primary_expression*場合は、型に分類`E`が型パラメーターでない場合、メンバーの検索 ([メンバー ルックアップ](expressions.md#member-lookup))`I`で`E`で`K` し、型パラメーターに一致するが生成されます`E.I`が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="60db1-955">場合`I`結果はその型指定された型引数を使用して構築型を識別します。</span><span class="sxs-lookup"><span data-stu-id="60db1-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="60db1-956">場合`I`結果は関連付けられたインスタンス式を指定せずに、メソッド グループに 1 つまたは複数のメソッドを識別します。</span><span class="sxs-lookup"><span data-stu-id="60db1-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="60db1-957">ジェネリック メソッドの呼び出しで使用される型引数リストが指定されている場合 ([メソッドの呼び出し](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="60db1-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="60db1-958">場合`I`識別、`static`プロパティ、結果は、プロパティへのアクセスに関連付けられたインスタンス式がありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="60db1-959">場合`I`識別、`static`フィールド。</span><span class="sxs-lookup"><span data-stu-id="60db1-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="60db1-960">フィールドの場合`readonly`クラスまたは構造体のフィールドが宣言されているの静的コンス トラクターの外部参照が発生し、結果は、値、つまり静的フィールドの値と `I`で `E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="60db1-961">それ以外の場合、結果は、変数、静的フィールド namely `I`で `E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="60db1-962">場合`I`識別、`static`イベント。</span><span class="sxs-lookup"><span data-stu-id="60db1-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="60db1-963">クラスまたは構造体をイベントを宣言し、イベントなしで宣言された参照が発生したかどうか*event_accessor_declarations* ([イベント](classes.md#events))、し`E.I`が正確に処理されます。まるで`I`スタティック フィールドします。</span><span class="sxs-lookup"><span data-stu-id="60db1-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="60db1-964">それ以外の場合、関連付けられたインスタンス式を指定イベント アクセスになりません。</span><span class="sxs-lookup"><span data-stu-id="60db1-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="60db1-965">場合`I`結果は、値、つまりその定数の値に定数を識別します。</span><span class="sxs-lookup"><span data-stu-id="60db1-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="60db1-966">場合`I`結果は、値、つまりその列挙体メンバーの値を列挙メンバーを識別します。</span><span class="sxs-lookup"><span data-stu-id="60db1-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="60db1-967">それ以外の場合、`E.I`無効なメンバー参照であり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="60db1-968">場合`E`プロパティへのアクセス、インデクサーのアクセス、変数、または、型が値 `T`、およびメンバーの検索 ([メンバー ルックアップ](expressions.md#member-lookup)) の`I`で`T`で`K`  型引数をし、一致する生成`E.I`が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="60db1-969">最初に、場合`E`プロパティまたはインデクサーのアクセスは、プロパティの値またはインデクサー アクセスが取得される ([式の値](expressions.md#values-of-expressions)) と`E`値として再分類します。</span><span class="sxs-lookup"><span data-stu-id="60db1-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="60db1-970">場合`I`、結果は、メソッドのグループの関連付けられたインスタンス式が 1 つまたは複数のメソッドを識別する`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="60db1-971">ジェネリック メソッドの呼び出しで使用される型引数リストが指定されている場合 ([メソッドの呼び出し](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="60db1-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="60db1-972">場合`I`インスタンス プロパティを識別します。</span><span class="sxs-lookup"><span data-stu-id="60db1-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="60db1-973">場合`E`は`this`、 `I` 、自動的に実装されたプロパティを識別します ([自動実装プロパティ](classes.md#automatically-implemented-properties)) のインスタンス コンス トラクター内で set アクセス操作子、および参照が発生せず、クラスまたは構造体型`T`、結果は、変数によって指定される自動プロパティの非表示のバッキング フィールド namely`I`のインスタンスで`T`によって指定された`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="60db1-974">結果の式が関連付けられているインスタンスのプロパティ アクセスは、それ以外の場合、 `E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="60db1-975">場合`T`は、 *class_type*と`I`のインスタンス フィールドを識別する*class_type*:</span><span class="sxs-lookup"><span data-stu-id="60db1-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="60db1-976">場合の値`E`は`null`、`System.NullReferenceException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="60db1-977">それ以外の場合、フィールドの場合`readonly`フィールドが宣言されているクラスのインスタンス コンス トラクターの外部参照が発生し、結果は、値、つまり、フィールドの値と `I`によって参照されるオブジェクトで `E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="60db1-978">それ以外の場合、結果は、変数、つまりフィールド `I`によって参照されるオブジェクトで `E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="60db1-979">場合`T`は、 *struct_type*と`I`のインスタンス フィールドを識別する*struct_type*:</span><span class="sxs-lookup"><span data-stu-id="60db1-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="60db1-980">場合`E`、値は、フィールドの場合、または`readonly`フィールドが宣言されている構造体のインスタンス コンス トラクターの外部参照が発生し、結果は、値、つまり、フィールドの値と `I`で指定された構造体のインスタンスで `E`.</span><span class="sxs-lookup"><span data-stu-id="60db1-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="60db1-981">それ以外の場合、結果は、変数、つまりフィールド `I`で指定された構造体のインスタンスで `E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="60db1-982">場合`I`インスタンス イベントを識別します。</span><span class="sxs-lookup"><span data-stu-id="60db1-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="60db1-983">クラスまたは構造体をイベントを宣言し、イベントなしで宣言された参照が発生したかどうか*event_accessor_declarations* ([イベント](classes.md#events)) に、参照が発生しないと、左側にある、`+=`または`-=`演算子、`E.I`が正確に処理される場合と`I`されたインスタンス フィールド。</span><span class="sxs-lookup"><span data-stu-id="60db1-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="60db1-984">それ以外の場合、結果は、イベントの関連付けられたインスタンス式があるアクセス `E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="60db1-985">それ以外の場合、しようとしましたが処理`E.I`拡張メソッド呼び出しとして ([拡張メソッド呼び出し](expressions.md#extension-method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="60db1-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="60db1-986">これが失敗すると、`E.I`無効なメンバー参照であり、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="60db1-987">同一の簡易名と型名</span><span class="sxs-lookup"><span data-stu-id="60db1-987">Identical simple names and type names</span></span>

<span data-ttu-id="60db1-988">形式のメンバー アクセスで`E.I`場合は、 `E` 、単一の識別子は、場合の意味`E`として、 *simple_name* ([簡易名](expressions.md#simple-names)) 定数、フィールド、プロパティ、ローカル変数またはパラメーターの意味と同じ型と`E`として、 *type_name* ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names))、しの両方の可能性のある意味`E`は許可されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="60db1-989">2 つの可能性のある意味`E.I`されないため、あいまいな`I`型のメンバーであるとは限りません`E`どちらの場合もします。</span><span class="sxs-lookup"><span data-stu-id="60db1-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="60db1-990">つまり、ルールだけアクセスを許可する静的メンバーと入れ子にされた型の`E`コンパイル時エラーが発生した、それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="60db1-991">例:</span><span class="sxs-lookup"><span data-stu-id="60db1-991">For example:</span></span>
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

#### <a name="grammar-ambiguities"></a><span data-ttu-id="60db1-992">文法のあいまいさ</span><span class="sxs-lookup"><span data-stu-id="60db1-992">Grammar ambiguities</span></span>

<span data-ttu-id="60db1-993">生産*simple_name* ([簡易名](expressions.md#simple-names)) と*member_access* ([メンバー アクセス](expressions.md#member-access)) のあいまいさを得ることができます、式の文法。</span><span class="sxs-lookup"><span data-stu-id="60db1-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="60db1-994">たとえば、次のようなステートメントがあるとします。</span><span class="sxs-lookup"><span data-stu-id="60db1-994">For example, the statement:</span></span>
```
F(G<A,B>(7));
```
<span data-ttu-id="60db1-995">呼び出しとして解釈される可能性`F`、2 つの引数で`G < A`と`B > (7)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="60db1-996">呼び出しとしても解釈される可能性`F`1 つの引数はジェネリック メソッドの呼び出し `G`2 つの型引数と 1 つの通常の引数。</span><span class="sxs-lookup"><span data-stu-id="60db1-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="60db1-997">トークンのシーケンスを (コンテキストで) として解析できるかどうか、 *simple_name* ([簡易名](expressions.md#simple-names))、 *member_access* ([メンバー アクセス](expressions.md#member-access))、または*pointer_member_access* ([ポインターのメンバー アクセス](unsafe-code.md#pointer-member-access)) で終わる、 *type_argument_list* ([引数を入力](types.md#type-arguments))、トークンの終了の直後に続く`>`トークンが調べられます。</span><span class="sxs-lookup"><span data-stu-id="60db1-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="60db1-998">いずれかである場合</span><span class="sxs-lookup"><span data-stu-id="60db1-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="60db1-999">次に、 *type_argument_list*はの一部として保持され、 *simple_name*、 *member_access*または*pointer_member_access*と、トークンのシーケンスの考えられるその他の解析は破棄されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="60db1-1000">それ以外の場合、 *type_argument_list*の一部であるとは見なされません、 *simple_name*、 *member_access*または*pointer_member_access*トークンのシーケンスの考えられるその他の解析がない場合でも、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="60db1-1001">解析するときに、これらの規則がないので注意が適用される、 *type_argument_list*で、 *namespace_or_type_name* ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="60db1-1002">次のステートメント、</span><span class="sxs-lookup"><span data-stu-id="60db1-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="60db1-1003">このルールに従って解釈されますへの呼び出しとして`F`1 つの引数はジェネリック メソッドの呼び出し`G`2 つの型引数と 1 つの通常の引数。</span><span class="sxs-lookup"><span data-stu-id="60db1-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="60db1-1004">ステートメント</span><span class="sxs-lookup"><span data-stu-id="60db1-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="60db1-1005">呼び出しとして解釈されます各`F`2 つの引数を使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="60db1-1006">次のステートメント、</span><span class="sxs-lookup"><span data-stu-id="60db1-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="60db1-1007">ステートメントを記述した場合と、小なり演算子、演算子と単項プラス演算子よりも大きいと解釈されます`x = (F < A) > (+y)`の代わりとして、 *simple_name*で、 *type_argument_list*二項プラス演算子を続けています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="60db1-1008">ステートメントで</span><span class="sxs-lookup"><span data-stu-id="60db1-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="60db1-1009">トークン`C<T>`として解釈されます、 *namespace_or_type_name*で、 *type_argument_list*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="60db1-1010">Invocation 式</span><span class="sxs-lookup"><span data-stu-id="60db1-1010">Invocation expressions</span></span>

<span data-ttu-id="60db1-1011">*Invocation_expression*メソッドを呼び出すために使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="60db1-1012">*Invocation_expression*が動的にバインドされている ([動的バインド](expressions.md#dynamic-binding))、次の少なくとも 1 つ保持している場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="60db1-1013">*Primary_expression*コンパイル時の型を持つ`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="60db1-1014">オプションの少なくとも 1 つの引数*argument_list*コンパイル時の型を持つ`dynamic`と*primary_expression*デリゲート型がありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="60db1-1015">この場合、コンパイラは、分類、 *invocation_expression*型の値として`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="60db1-1016">意味を調べるには、次の規則、 *invocation_expression*実行時、のコンパイル時の型ではなく、実行時の型を使用するのには、適用、 *primary_expression*とコンパイル時の型引数`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="60db1-1017">場合、 *primary_expression*コンパイル時の型が`dynamic`、メソッドの呼び出しで」の説明に従って、制限付きのコンパイル時のチェックが行われ、[コンパイル時の動的なオーバー ロードの解決の確認](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="60db1-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="60db1-1018">*Primary_expression*の*invocation_expression*メソッド グループ、またはの値にする必要があります、 *delegate_type*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="60db1-1019">場合、 *primary_expression* 、メソッド グループには、 *invocation_expression*メソッドの呼び出しは、([メソッドの呼び出し](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="60db1-1020">場合、 *primary_expression*の値であり、 *delegate_type*、 *invocation_expression*デリゲートの呼び出しは、([デリゲートの呼び出し](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="60db1-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="60db1-1021">場合、 *primary_expression*はメソッド グループでの値も、 *delegate_type*、バインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-1022">省略可能な*argument_list* ([引数リスト](expressions.md#argument-lists)) メソッドのパラメーター値または変数参照を提供します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="60db1-1023">評価した結果、 *invocation_expression*次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="60db1-1024">場合、 *invocation_expression*メソッドまたはを返すデリゲートが呼び出される`void`結果はありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="60db1-1025">コンテキストでのみ許可されているが何に分類される式を*statement_expression* ([式ステートメント](statements.md#expression-statements)) またはの本文として、 *lambda_expression*([匿名関数式](expressions.md#anonymous-function-expressions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="60db1-1026">それ以外の場合、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="60db1-1027">それ以外の場合、メソッドまたはデリゲートによって返される型の値になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="60db1-1028">メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-1028">Method invocations</span></span>

<span data-ttu-id="60db1-1029">メソッドの呼び出し、 *primary_expression*の*invocation_expression*メソッド グループである必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="60db1-1030">メソッド グループでは、1 つのメソッドを呼び出すまたは特定のメソッドを呼び出すを選択できるオーバー ロードされたメソッドのセットを識別します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="60db1-1031">後者の場合、呼び出す特定のメソッドの決定は、引数の型によって提供されるコンテキストに基づきます、 *argument_list*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="60db1-1032">フォームのメソッドの呼び出しのバインド時の処理`M(A)`ここで、`M`メソッド グループ (を含めることも、 *type_argument_list*)、および`A`は省略可能な*argument_リスト*、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="60db1-1033">メソッドの呼び出しの候補のメソッドのセットが構築されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="60db1-1034">各メソッドの`F`メソッド グループに関連付けられた`M`:</span><span class="sxs-lookup"><span data-stu-id="60db1-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="60db1-1035">場合`F`非ジェネリック型`F`候補とき。</span><span class="sxs-lookup"><span data-stu-id="60db1-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="60db1-1036">`M` 型引数リストがないと</span><span class="sxs-lookup"><span data-stu-id="60db1-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="60db1-1037">`F` 適用できる`A`([適用可能な関数メンバー](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="60db1-1038">場合`F`がジェネリックと`M`型引数のリストを持たない`F`候補とき。</span><span class="sxs-lookup"><span data-stu-id="60db1-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="60db1-1039">型の推論 ([型推論](expressions.md#type-inference)) 成功すると、呼び出し、型引数の一覧への推論と</span><span class="sxs-lookup"><span data-stu-id="60db1-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="60db1-1040">F のパラメーター リストですべて構築された型が、その制約を満たす、推論された型引数は、対応するメソッドの型パラメーターに置き換え、([制約条件を満たす](types.md#satisfying-constraints))、およびのパラメーターのリスト`F`を適用できる`A`([適用可能な関数メンバー](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="60db1-1041">場合`F`ジェネリックと`M`型引数リストが含まれています`F`候補とき。</span><span class="sxs-lookup"><span data-stu-id="60db1-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="60db1-1042">`F` 型引数リストで指定されたメソッド型パラメーターの同じ番号を持つと</span><span class="sxs-lookup"><span data-stu-id="60db1-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="60db1-1043">F のパラメーター リストですべて構築された型が、その制約を満たす型引数は、対応するメソッドの型パラメーターに置き換え、([制約条件を満たす](types.md#satisfying-constraints))、およびパラメーター リストの`F`適用できる`A`([適用可能な関数メンバー](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="60db1-1044">候補メソッドのセットは、最派生型からのみを含むに縮小されます。各メソッドの`C.F`、セット内で`C`の種類は、メソッド`F`の基本型で宣言されているすべてのメソッド宣言`C`セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="60db1-1045">さらに場合、`C`クラス型は他にも、`object`インターフェイス型で宣言されているすべてのメソッドは、セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="60db1-1046">(この後者の規則のみが影響メソッド グループがオブジェクト以外の有効な基本クラスと、空でない有効なインターフェイス セットを持つ型パラメーターでメンバーの検索の結果です。)</span><span class="sxs-lookup"><span data-stu-id="60db1-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="60db1-1047">候補メソッドの結果セットが空の場合は、さらに、次の手順に沿った処理は中止され、代わりに試行されると、呼び出しを拡張メソッド呼び出しとして処理する ([拡張メソッド呼び出し](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="60db1-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="60db1-1048">これが失敗した場合、適用可能なメソッドが存在していないため、し、バインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="60db1-1049">オーバー ロード解決規則を使用して候補メソッドのセットの最適な方法が識別される[オーバー ロードの解決](expressions.md#overload-resolution)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="60db1-1050">、1 つの最適な方法を識別できない場合、メソッドの呼び出しはあいまいであり、バインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="60db1-1051">オーバー ロードの解決を実行するには、対応するメソッドの型パラメーターのジェネリック メソッドのパラメーターが型引数 (指定または推論) を代入した後と見なされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="60db1-1052">選択した最適な方法の最終的な検証が実行されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="60db1-1053">メソッドは、メソッド グループのコンテキストで検証されます。最適な方法が静的メソッドの場合、メソッド グループが得、 *simple_name*または*member_access*型を通じてします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="60db1-1054">最適な方法が、インスタンス メソッドの場合、メソッド グループが得、 *simple_name*、 *member_access*変数または値、または*base_access*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="60db1-1055">これらの要件のどちらが true の場合、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="60db1-1056">(指定または推論) の型引数が制約に照らしてチェックが最適なメソッドがジェネリック メソッドの場合 ([制約条件を満たす](types.md#satisfying-constraints)) ジェネリック メソッドで宣言します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="60db1-1057">すべての型引数が、型パラメーターに対応する制約を満たしていない場合、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-1058">説明されている関数メンバーの呼び出しの規則に従って実行時の実際の呼び出しを処理するメソッドを選択し、上記の手順によって、バインド時に検証されましたが、[コンパイル時の動的なオーバー ロードの解決の確認](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="60db1-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="60db1-1059">上記で説明した解決ルールの直感的な影響は次のとおりです。メソッドの呼び出しによって起動される特定のメソッドを検索するにはメソッドの呼び出しで示される型で始めて、少なくとも 1 つの適用可能なアクセス可能なオーバーライドではないメソッドの宣言が見つかるまで、継承チェーンを続行できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="60db1-1060">型の推定を実行しオーバー ロードの解決では、その型で宣言されている適用可能なアクセス可能なオーバーライドではないメソッドのセットと、選択されているため、メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="60db1-1061">メソッドが見つからなかった場合は、呼び出しを拡張メソッド呼び出しとして処理する代わりにしてください。</span><span class="sxs-lookup"><span data-stu-id="60db1-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="60db1-1062">拡張メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-1062">Extension method invocations</span></span>

<span data-ttu-id="60db1-1063">メソッドの呼び出しで ([ボックス化されたインスタンス上の呼び出し](expressions.md#invocations-on-boxed-instances))、フォームの 1 つの</span><span class="sxs-lookup"><span data-stu-id="60db1-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="60db1-1064">呼び出しの通常の処理に適用可能なメソッドが検出されない場合、拡張メソッドの呼び出しとして構造の処理を試行します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="60db1-1065">場合*expr*またはのいずれか、 *args*コンパイル時の型を持つ`dynamic`、拡張メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="60db1-1066">目的は、一番*type_name* `C`、対応する静的メソッド呼び出しを実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="60db1-1067">拡張メソッド`Ci.Mj`は***対象となる***場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="60db1-1068">`Ci` 非ジェネリックの入れ子になっていないクラス</span><span class="sxs-lookup"><span data-stu-id="60db1-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="60db1-1069">名前`Mj`は*識別子*</span><span class="sxs-lookup"><span data-stu-id="60db1-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="60db1-1070">`Mj` アクセス可能で、上記のように、引数に静的メソッドとして適用されるときに適用できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="60db1-1071">暗黙的な id、参照変換またはボックス化変換が存在する*expr*の最初のパラメーターの型に`Mj`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="60db1-1072">検索`C`次のように進みます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="60db1-1073">以降、それを囲む各名前空間宣言を続行し、コンパイル単位で終わるの最も近いの外側の名前空間宣言での拡張メソッドの候補セットを連続する試行が行われます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="60db1-1074">指定された名前空間またはコンパイル ユニットに直接汎用でない型宣言が含まれるかどうか`Ci`資格のある拡張メソッドで`Mj`、それらの拡張メソッドのセットは、候補の集合です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="60db1-1075">場合型`Ci`によってインポートされた*using_static_declarations*によってインポートされた名前空間で直接宣言と*using_namespace_directive*で指定された名前空間またはコンパイル ユニットを直接 s対象となる拡張メソッドを含む`Mj`、それらの拡張メソッドのセットは、候補の集合。</span><span class="sxs-lookup"><span data-stu-id="60db1-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="60db1-1076">それを囲む名前空間の宣言またはコンパイル単位の候補のセットが見つからない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="60db1-1077">示すように設定する候補にオーバー ロードの解決を適用する場合は、([オーバー ロードの解決](expressions.md#overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="60db1-1078">1 つの最適な方法が見つからない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="60db1-1079">`C` 拡張メソッドとして最適な方法を宣言する型です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="60db1-1080">使用して`C`静的メソッドの呼び出しとして処理し、メソッドの呼び出しのターゲットとして ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="60db1-1081">インスタンス メソッドが拡張メソッドより優先順位を取得、拡張メソッドの内部名前空間の宣言で使用可能な優先される拡張メソッドの外部名前空間の宣言、およびその拡張機能で使用できるという意味で、上記の規則名前空間で直接宣言されたメソッドを使用して、その同じ空間にインポートされた拡張メソッドよりも優先名前空間ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="60db1-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="60db1-1082">例:</span><span class="sxs-lookup"><span data-stu-id="60db1-1082">For example:</span></span>
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

<span data-ttu-id="60db1-1083">例では、`B`の最初の拡張メソッドより優先されるメソッドと`C`のメソッドが両方の拡張メソッドより優先されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

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

<span data-ttu-id="60db1-1084">この例の出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1084">The output of this example is:</span></span>
```
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="60db1-1085">`D.G` も優先`C.G`、および`E.F`両方よりも優先`D.F`と`C.F`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="60db1-1086">デリゲートの呼び出し</span><span class="sxs-lookup"><span data-stu-id="60db1-1086">Delegate invocations</span></span>

<span data-ttu-id="60db1-1087">デリゲートの呼び出しの*primary_expression*の*invocation_expression*の値を指定する必要があります、 *delegate_type*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="60db1-1088">さらに、検討、 *delegate_type*と同じパラメーター リストを持つ関数メンバーである、 *delegate_type*、 *delegate_type* (該当する必要があります[適用可能な関数メンバー](expressions.md#applicable-function-member)) を*argument_list*の*invocation_expression*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="60db1-1089">フォームのデリゲートの呼び出しの実行時の処理`D(A)`ここで、`D`は、 *primary_expression*の*delegate_type*と`A`は省略可能な*argument_list*、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="60db1-1090">`D` 評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1090">`D` is evaluated.</span></span> <span data-ttu-id="60db1-1091">この評価は、例外を発生させ、その後の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="60db1-1092">値`D`がチェックを有効にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="60db1-1093">場合の値`D`は`null`、`System.NullReferenceException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="60db1-1094">それ以外の場合、`D`デリゲート インスタンスへの参照です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="60db1-1095">関数メンバーの呼び出し ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) の各デリゲートの呼び出しリストで呼び出し可能なエンティティで実行されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="60db1-1096">呼び出し可能なエンティティのインスタンスとインスタンス メソッドで構成される場合は、特定の呼び出しのインスタンスは、呼び出し可能なエンティティに含まれているインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="60db1-1097">要素へのアクセス</span><span class="sxs-lookup"><span data-stu-id="60db1-1097">Element access</span></span>

<span data-ttu-id="60db1-1098">*Element_access*から成る、 *primary_no_array_creation_expression*、その後に、"`[`"トークン、その後に、 *argument_list*、その後に、"`]`"トークンです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="60db1-1099">*Argument_list* 1 つまたは複数から成る*引数*s、コンマで区切られました。</span><span class="sxs-lookup"><span data-stu-id="60db1-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="60db1-1100">*Argument_list*の*element_access*を格納することはできません`ref`または`out`引数。</span><span class="sxs-lookup"><span data-stu-id="60db1-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="60db1-1101">*Element_access*が動的にバインドされている ([動的バインド](expressions.md#dynamic-binding))、次の少なくとも 1 つ保持している場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="60db1-1102">*Primary_no_array_creation_expression*コンパイル時の型を持つ`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="60db1-1103">少なくとも 1 つの式、 *argument_list*コンパイル時の型を持つ`dynamic`と*primary_no_array_creation_expression*配列型はありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="60db1-1104">この場合、コンパイラは、分類、 *element_access*型の値として`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="60db1-1105">意味を調べるには、次の規則、 *element_access*実行時、のコンパイル時の型ではなく、実行時の型を使用するのには、適用、 *primary_no_array_creation_expression*と*argument_list*コンパイル時の型を持つことが式`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="60db1-1106">場合、 *primary_no_array_creation_expression*コンパイル時の型が`dynamic`、」の説明に従って、要素へのアクセスが制限付きのコンパイル時のチェックを受けて、[コンパイル時の動的なチェックオーバー ロードの解決](expressions.md#compile-time-checking-of-dynamic-overload-resolution)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="60db1-1107">場合、 *primary_no_array_creation_expression*の*element_access*の値であり、 *array_type*、 *element_access*が、配列のアクセス ([配列アクセス](expressions.md#array-access))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="60db1-1108">それ以外の場合、 *primary_no_array_creation_expression*変数またはクラス、構造体、またはインターフェイスの種類を 1 つまたは複数のインデクサーのメンバーを持つ場合の値を指定する必要があります、 *element_access*が、インデクサー アクセス ([インデクサー アクセス](expressions.md#indexer-access))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="60db1-1109">配列へのアクセス</span><span class="sxs-lookup"><span data-stu-id="60db1-1109">Array access</span></span>

<span data-ttu-id="60db1-1110">配列アクセスの場合、 *primary_no_array_creation_expression*の*element_access*の値を指定する必要があります、 *array_type*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="60db1-1111">さらに、 *argument_list*配列のアクセスは名前付き引数を格納する許可されません。式の数、 *argument_list*のランクと同じである必要があります、 *array_type*、各式は、型にする必要がありますと`int`、 `uint`、 `long`、 `ulong`、または 1 つ以上のこれらの型に暗黙的に変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="60db1-1112">配列のアクセスの評価結果は、namely で式の値で選択されている配列要素配列の要素型の変数、 *argument_list*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="60db1-1113">フォームの配列のアクセスの実行時の処理`P[A]`ここで、`P`は、 *primary_no_array_creation_expression*の*array_type*と`A`には*argument_list*、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="60db1-1114">`P` 評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1114">`P` is evaluated.</span></span> <span data-ttu-id="60db1-1115">この評価は、例外を発生させ、その後の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="60db1-1116">インデックスの式、 *argument_list*左から右への順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="60db1-1117">次の各インデックスの式の暗黙の変換の評価 ([暗黙的な変換](conversions.md#implicit-conversions))、次の種類のいずれかに実行されます: `int`、 `uint`、 `long`、`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="60db1-1118">暗黙的な変換が存在するこのリストの最初の型が選択されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="60db1-1119">たとえば、型の場合は、インデックスの式`short`への暗黙的な変換し`int`以降からの暗黙的な変換を実行`short`に`int`との間`short`に`long`が考えられます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="60db1-1120">インデックス式またはそれ以降の暗黙的な変換の評価は、例外を発生させ場合、は、さらにインデックスの式は評価されませんしそれ以上の手順が実行されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="60db1-1121">値`P`がチェックを有効にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="60db1-1122">場合の値`P`は`null`、`System.NullReferenceException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="60db1-1123">内の各式の値、 *argument_list*によって参照される配列インスタンスの各次元の実際の境界と照合されます`P`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="60db1-1124">1 つまたは複数の値が範囲外にある場合、`System.IndexOutOfRangeException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="60db1-1125">インデックス式で指定された配列要素の位置を計算し、この場所の配列へのアクセスの結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="60db1-1126">インデクサーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="60db1-1126">Indexer access</span></span>

<span data-ttu-id="60db1-1127">インデクサー アクセス用、 *primary_no_array_creation_expression*の*element_access*変数またはクラス、構造体、またはインターフェイスの型の値を指定する必要があり、この型は、1 つまたは複数を実装する必要があります適用できるインデクサー、 *argument_list*の*element_access*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="60db1-1128">フォームのインデクサー アクセスのバインド時の処理`P[A]`ここで、`P`は、 *primary_no_array_creation_expression*クラス、構造体、またはインターフェイス型の`T`、および`A`が、*argument_list*、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="60db1-1129">一連のインデクサーによって提供される`T`を構築します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="60db1-1130">宣言されているすべてのインデクサーはセット`T`の基本型または`T`れていない`override`宣言とは、現在のコンテキストでアクセスできるように ([メンバー アクセス](basic-concepts.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="60db1-1131">セットは、その他のインデクサーによって隠ぺいされていない適用可能なインデクサーだけに縮小されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="60db1-1132">次の規則は、各インデクサーに適用されます`S.I`、セット内で`S`の種類は、インデクサー`I`は宣言されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="60db1-1133">場合`I`を適用可能でない`A`([適用可能な関数メンバー](expressions.md#applicable-function-member))、し`I`セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="60db1-1134">場合`I`を適用できる`A`([適用可能な関数メンバー](expressions.md#applicable-function-member)) の基本型ですべてのインデクサーを宣言し、`S`セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="60db1-1135">場合`I`を適用できる`A`([適用可能な関数メンバー](expressions.md#applicable-function-member)) と`S`クラス型は他にも、`object`インターフェイスで宣言されているすべてのインデクサーは、セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="60db1-1136">インデクサーの候補の結果セットが空の場合、該当するインデクサーが存在していないため、し、バインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="60db1-1137">オーバー ロード解決規則を使用して、一連の候補のインデクサーの最適なインデクサーが識別される[オーバー ロードの解決](expressions.md#overload-resolution)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="60db1-1138">1 つの最適なインデクサーを識別できない場合、インデクサー アクセスはあいまいであり、バインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="60db1-1139">インデックスの式、 *argument_list*左から右への順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="60db1-1140">インデクサーのアクセスの処理の結果は、インデクサー アクセスとして分類される式です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="60db1-1141">インデクサー アクセス式は、上記の手順で決定されたインデクサーが参照し、の関連付けられたインスタンス式があります`P`と関連付けられている引数リストの`A`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="60db1-1142">インデクサー アクセスのいずれかの呼び出しの原因が使用されるコンテキストに応じて、 *get アクセサー*または*set アクセサー*のインデクサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="60db1-1143">インデクサーへのアクセスが、割り当ての対象である場合、 *set アクセサー*新しい値を割り当てるために呼び出される ([単純な代入](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="60db1-1144">その他のすべてのケースで、 *get アクセサー* 、現在の値を取得するために呼び出す ([式の値](expressions.md#values-of-expressions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="60db1-1145">このアクセス</span><span class="sxs-lookup"><span data-stu-id="60db1-1145">This access</span></span>

<span data-ttu-id="60db1-1146">A *this_access*予約語から成る`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="60db1-1147">A *this_access*でのみ許可されている、*ブロック*のインスタンス コンス トラクター、インスタンス メソッドまたはインスタンス アクセサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="60db1-1148">意味は次のいずれかがあります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="60db1-1149">ときに`this`で使用されて、 *primary_expression*クラスのインスタンス コンス トラクター、内の値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="60db1-1150">値の型は、インスタンスの型 ([インスタンス型](classes.md#the-instance-type)) で、使用状況が発生した、値が構築されるオブジェクトへの参照をクラスの。</span><span class="sxs-lookup"><span data-stu-id="60db1-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="60db1-1151">ときに`this`で使用されて、 *primary_expression*値として、インスタンス メソッドまたはクラスのインスタンスのアクセサー内で分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="60db1-1152">値の型は、インスタンスの型 ([インスタンス型](classes.md#the-instance-type)) で、使用状況が発生した、値がメソッドまたはアクセサーが呼び出されたオブジェクトへの参照をクラスの。</span><span class="sxs-lookup"><span data-stu-id="60db1-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="60db1-1153">ときに`this`で使用されて、 *primary_expression*構造体のインスタンス コンス トラクター内で変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="60db1-1154">変数の型は、インスタンスの型 ([インスタンス型](classes.md#the-instance-type)) の構造体で、使用状況が発生した、変数が作成される構造体を表します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="60db1-1155">`this`構造体のインスタンス コンス トラクターの変数はまったく同じように動作する`out`構造体の型のパラメーター-具体的には、つまり、変数を間違いなく、インスタンスのすべての実行パスで割り当てる必要がありますコンス トラクターです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="60db1-1156">ときに`this`で使用されて、 *primary_expression*内で、インスタンス メソッドまたはインスタンス アクセサー構造体の変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="60db1-1157">変数の型は、インスタンスの型 ([インスタンス型](classes.md#the-instance-type)) 使用する構造体の。</span><span class="sxs-lookup"><span data-stu-id="60db1-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="60db1-1158">メソッドまたはアクセサーは、反復子ではない場合 ([反復子](classes.md#iterators))、`this`変数は、対象の構造体、メソッドまたはアクセサーが呼び出されたとまったく同じ動作を表します、`ref`構造体の型のパラメーター。</span><span class="sxs-lookup"><span data-stu-id="60db1-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="60db1-1159">メソッドまたはアクセサーが、反復子の場合、`this`変数は、対象のメソッドまたはアクセサーが呼び出され、構造体の型の値を持つパラメーターとまったく同じ動作には、構造体のコピーを表します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="60db1-1160">使用`this`で、 *primary_expression*上に挙げた以外のコンテキストでは、コンパイル時エラー。</span><span class="sxs-lookup"><span data-stu-id="60db1-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="60db1-1161">具体的を参照することはできません`this`または静的プロパティのアクセサーでは、静的メソッドで、 *variable_initializer*フィールド宣言のです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="60db1-1162">基本アクセス</span><span class="sxs-lookup"><span data-stu-id="60db1-1162">Base access</span></span>

<span data-ttu-id="60db1-1163">A *base_access*の予約語で構成されます`base`か後に、"`.`"トークン識別子や、 *argument_list*角かっこで囲まれています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="60db1-1164">A *base_access*現在のクラスまたは構造体で同じ名前のメンバーでは非表示の基本クラス メンバーにアクセスするために使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="60db1-1165">A *base_access*でのみ許可されている、*ブロック*のインスタンス コンス トラクター、インスタンス メソッドまたはインスタンス アクセサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="60db1-1166">ときに`base.I`クラスまたは構造体で発生した`I`そのクラスまたは構造体の基本クラスのメンバーを表す必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="60db1-1167">同様に、`base[E]`を適用できるインデクサーは、基底クラスに存在する必要があります、クラス内に発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="60db1-1168">バインディング時に、 *base_access*形式の式`base.I`と`base[E]`書き込まれている場合と同じように評価されます`((B)this).I`と`((B)this)[E]`ここで、`B`クラスの基本クラスですまたは、構造体、構造に発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="60db1-1169">したがって、`base.I`と`base[E]`に対応`this.I`と`this[E]`、除く`this`基底クラスのインスタンスとして表示されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="60db1-1170">ときに、 *base_access*うち決定関数の実行時に呼び出すメンバー仮想関数メンバー (メソッド、プロパティ、またはインデクサー) を参照して ([コンパイル時の動的なオーバー ロードの解決の確認](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) が変更されました。</span><span class="sxs-lookup"><span data-stu-id="60db1-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="60db1-1171">呼び出される関数メンバーは、最派生実装を見つけることによって決まります ([仮想メソッド](classes.md#virtual-methods)) を関数メンバーの`B`(の代わりの実行時の型に関して`this`、としてなりますベース以外のアクセスに通常)。</span><span class="sxs-lookup"><span data-stu-id="60db1-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="60db1-1172">内でそのため、`override`の`virtual`関数のメンバー、 *base_access*関数メンバーの継承された実装を呼び出すために使用できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="60db1-1173">によって参照される関数のメンバーである場合、 *base_access*は抽象でバインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="60db1-1174">置インクリメント演算子と前置デクリメント演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="60db1-1175">オペランドの後置インクリメントまたはデクリメント操作は、変数、プロパティ アクセス、またはインデクサー アクセスとして分類される式をする必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="60db1-1176">操作の結果は、オペランドと同じ型の値です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="60db1-1177">場合、 *primary_expression*コンパイル時の型を持つ`dynamic`演算子が動的にバインドし、([動的バインド](expressions.md#dynamic-binding))、 *post_increment_expression*または*post_decrement_expression*コンパイル時の型を持つ`dynamic`の実行時の型を使用して実行時に次の規則を適用し、 *primary_expression*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="60db1-1178">場合は、オペランドを後置のインクリメントまたはデクリメント演算は、プロパティまたはインデクサー アクセス、プロパティまたはインデクサーが両方必要、`get`と`set`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="60db1-1179">サポートしていない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-1180">単項演算子のオーバー ロードの解決 ([単項演算子のオーバー ロードの解決](expressions.md#unary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-1181">定義済みの`++`と`--`演算子は、次の種類の存在: `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、 `decimal`、および任意の列挙型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="60db1-1182">定義済み`++`演算子がオペランドを定義済みに 1 を追加することによって生成された値を返す`--`演算子オペランドから 1 を引いた値を返します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="60db1-1183">`checked`場合この加算または減算の結果が結果の型の範囲外と結果型が整数型または列挙型では、コンテキスト、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="60db1-1184">実行時の処理の後置インクリメントまたはデクリメント演算子は、フォームの`x++`または`x--`次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="60db1-1185">場合`x`変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="60db1-1186">`x` 変数を生成するために評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="60db1-1187">値`x`保存されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="60db1-1188">選択した演算子が呼び出されるの保存された値と`x`引数として。</span><span class="sxs-lookup"><span data-stu-id="60db1-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="60db1-1189">評価によって指定した位置に、演算子によって返される値が格納されている`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="60db1-1190">保存された値`x`操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="60db1-1191">場合`x`プロパティまたはインデクサーのアクセスに分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="60db1-1192">インスタンス式 (場合`x`ない`static`) および引数リスト (場合`x`インデクサー アクセス) に関連付けられている`x`が評価されると、その後で、結果の使用`get`と`set`アクセサーの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="60db1-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="60db1-1193">`get`のアクセサー`x`が呼び出される、返される値を保存します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="60db1-1194">選択した演算子が呼び出されるの保存された値と`x`引数として。</span><span class="sxs-lookup"><span data-stu-id="60db1-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="60db1-1195">`set`のアクセサー`x`が呼び出されると演算子によって返される値とその`value`引数。</span><span class="sxs-lookup"><span data-stu-id="60db1-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="60db1-1196">保存された値`x`操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="60db1-1197">`++`と`--`演算子もサポートしてプレフィックス表記 ([前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="60db1-1198">結果では通常、`x++`または`x--`の値は、`x`操作の前に一方の結果`++x`または`--x`の値は、`x`操作の完了後します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="60db1-1199">いずれの場合も、`x`自体、操作の後に同じ値に含まれています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="60db1-1200">`operator ++`または`operator --`実装は、いずれかの後置または前置表記を使用して呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="60db1-1201">2 つの表記の個別の演算子の実装を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="60db1-1202">新しい演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1202">The new operator</span></span>

<span data-ttu-id="60db1-1203">`new`演算子を使用して、型の新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="60db1-1204">3 つの形式がある`new`式。</span><span class="sxs-lookup"><span data-stu-id="60db1-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="60db1-1205">オブジェクト作成式は、クラス型と値の型の新しいインスタンスを作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="60db1-1206">配列作成式は、配列型の新しいインスタンスを作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="60db1-1207">型のデリゲートの新しいインスタンスを作成するデリゲート作成式が使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="60db1-1208">`new`演算子は、型のインスタンスの作成を意味しますが、メモリの動的な割り当てを必ずしも意味しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="60db1-1209">具体的には、値型のインスタンスには存在して、動的な割り当ては発生しませんが、変数を超える追加のメモリ必要ないとき`new`値型のインスタンスを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="60db1-1210">オブジェクト作成式</span><span class="sxs-lookup"><span data-stu-id="60db1-1210">Object creation expressions</span></span>

<span data-ttu-id="60db1-1211">*Object_creation_expression*の新しいインスタンスを作成するために使用する*class_type*または*value_type*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

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

<span data-ttu-id="60db1-1212">*型*の*object_creation_expression*必要があります、 *class_type*、 *value_type*または*type_parameter*.</span><span class="sxs-lookup"><span data-stu-id="60db1-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="60db1-1213">*型*することはできません、 `abstract` *class_type*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="60db1-1214">省略可能な*argument_list* ([引数リスト](expressions.md#argument-lists)) 場合にのみが許可されている、*型*は、 *class_type*または*struct_型*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="60db1-1215">オブジェクト作成式でコンス トラクターの引数リストを省略でき、外側のかっこ、オブジェクト初期化子またはコレクション初期化子が含まれています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="60db1-1216">コンス トラクターの引数リストを省略して、外側のかっこは、空の引数リストを指定することと同じです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="60db1-1217">オブジェクト初期化子またはコレクション初期化子を含むオブジェクト作成式の処理は、最初にインスタンス コンス トラクターを処理し、オブジェクト初期化子 (で指定されたメンバーまたは要素の初期化を処理し、[オブジェクト初期化子](expressions.md#object-initializers)) またはコレクション初期化子 ([コレクション初期化子](expressions.md#collection-initializers))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="60db1-1218">場合、省略可能な引数のいずれか*argument_list*コンパイル時の型を持つ`dynamic`、 *object_creation_expression*が動的にバインドされている ([の動的バインド](expressions.md#dynamic-binding))これらの引数の実行時の型を使用して実行時に次の規則を適用し、 *argument_list*コンパイル時の型がある`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="60db1-1219">オブジェクトの作成に限定のコンパイル時のチェックが行われます」の説明に従って、[コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="60db1-1220">バインド時の処理、 *object_creation_expression*フォームの`new T(A)`ここで、`T`は、 *class_type*または*value_type*と`A`は省略可能な*argument_list*、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="60db1-1221">場合`T`は、 *value_type*と`A`が存在しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="60db1-1222">*Object_creation_expression*は既定のコンス トラクターの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="60db1-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="60db1-1223">結果、 *object_creation_expression*型の値は、`T`の既定値 namely`T`で定義されている[、System.ValueType 型](types.md#the-systemvaluetype-type)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="60db1-1224">の場合`T`は、 *type_parameter*と`A`が存在しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="60db1-1225">値型の制約またはコンス トラクター制約のない場合 ([入力パラメーターの制約](classes.md#type-parameter-constraints)) が指定されている`T`、バインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="60db1-1226">結果、 *object_creation_expression*型パラメーターのバインドが、実行時型の値は、その型の既定のコンス トラクターの呼び出しの namely の結果。</span><span class="sxs-lookup"><span data-stu-id="60db1-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="60db1-1227">実行時の型には、参照型または値型があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="60db1-1228">の場合`T`は、 *class_type*または*struct_type*:</span><span class="sxs-lookup"><span data-stu-id="60db1-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="60db1-1229">場合`T`は、 `abstract` *class_type*コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="60db1-1230">インスタンス コンス トラクターを呼び出すには、オーバー ロード解決規則を使用して決定されます[オーバー ロードの解決](expressions.md#overload-resolution)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="60db1-1231">インスタンス コンス トラクターの候補のセットの構成で宣言されているすべてのアクセス可能なインスタンス コンス トラクターが`T`に関して適用される`A`([適用可能な関数メンバー](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="60db1-1232">インスタンス コンス トラクターの候補のセットが空の場合、または 1 つの最適なインスタンス コンス トラクターを識別できない場合は、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="60db1-1233">結果、 *object_creation_expression*型の値は、 `T`、つまり前の手順で決定されたインスタンス コンス トラクターを呼び出すことによって生成される値。</span><span class="sxs-lookup"><span data-stu-id="60db1-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="60db1-1234">それ以外の場合、 *object_creation_expression*が有効でないバインド時のエラーが発生したとします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-1235">場合でも、 *object_creation_expression*は動的にバインドすると、コンパイル時の型がまだ`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="60db1-1236">実行時の処理、 *object_creation_expression*フォームの`new T(A)`ここで、`T`は*class_type*または*struct_type*と`A`は省略可能な*argument_list*、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="60db1-1237">場合`T`は、 *class_type*:</span><span class="sxs-lookup"><span data-stu-id="60db1-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="60db1-1238">クラスの新しいインスタンス`T`が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="60db1-1239">新しいインスタンスを割り当てることができる十分なメモリがない場合、`System.OutOfMemoryException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="60db1-1240">新しいインスタンスのすべてのフィールドが既定値に初期化されます ([既定値](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="60db1-1241">関数メンバーの呼び出しの規則に従って、インスタンス コンス トラクターが呼び出されます ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="60db1-1242">新しく割り当てられたインスタンスへの参照が自動的にインスタンス コンス トラクターに渡されます、としてコンス トラクターの中から、インスタンスにアクセスできる`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="60db1-1243">場合`T`は、 *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="60db1-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="60db1-1244">型のインスタンス`T`は一時ローカル変数を割り当てることによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="60db1-1245">以降のインスタンス コンス トラクター、 *struct_type*一時変数の初期化する必要はありませんが、作成中のインスタンスの各フィールドに値を確実に代入するが必要です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="60db1-1246">関数メンバーの呼び出しの規則に従って、インスタンス コンス トラクターが呼び出されます ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="60db1-1247">新しく割り当てられたインスタンスへの参照が自動的にインスタンス コンス トラクターに渡されます、としてコンス トラクターの中から、インスタンスにアクセスできる`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="60db1-1248">オブジェクト初期化子</span><span class="sxs-lookup"><span data-stu-id="60db1-1248">Object initializers</span></span>

<span data-ttu-id="60db1-1249">***オブジェクト初期化子***フィールド、プロパティまたはオブジェクトのインデックス付きの要素の 0 個以上の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

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

<span data-ttu-id="60db1-1250">オブジェクト初期化子で囲まれた、メンバー初期化子のシーケンスから成る`{`と`}`トークンし、コンマで区切られました。</span><span class="sxs-lookup"><span data-stu-id="60db1-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="60db1-1251">各*member_initializer*初期化するためのターゲットを指定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="60db1-1252">*識別子*一方、アクセス可能なフィールドまたはプロパティが初期化されているオブジェクトの名前、 *argument_list*で囲む角かっこでアクセス可能なインデクサーの引数を指定する必要があります、初期化されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="60db1-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="60db1-1253">オブジェクト初期化子を同じフィールドまたはプロパティの 1 つ以上のメンバー初期化子を含めるとエラーになります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="60db1-1254">各*initializer_target*等号と式、オブジェクト初期化子またはコレクション初期化子を続けています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="60db1-1255">新しく作成されたオブジェクトが初期化を参照するオブジェクト初期化子内の式のことはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="60db1-1256">割り当てと同じ方法で、等号が処理された後に式を指定するメンバーの初期化子 ([単純な代入](expressions.md#simple-assignment)) をターゲットにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="60db1-1257">等号の後に、オブジェクト初期化子を指定するメンバーの初期化子、***入れ子になったオブジェクトの初期化子***、つまり、埋め込みオブジェクトの初期化。</span><span class="sxs-lookup"><span data-stu-id="60db1-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="60db1-1258">フィールドまたはプロパティに新しい値を割り当てる代わりに、入れ子になったオブジェクト初期化子の割り当ては、フィールドまたはプロパティのメンバーへの割り当てとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="60db1-1259">入れ子になったオブジェクト初期化子は、値の型を持つプロパティ、または値型を持つ読み取り専用のフィールドに適用できません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="60db1-1260">等号の後に、コレクション初期化子を指定するメンバーの初期化子は、コレクションが埋め込まれたの初期化です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="60db1-1261">新しいコレクションを対象のフィールド、プロパティまたはインデクサーに割り当てる代わりに、初期化子で指定された要素は、ターゲットによって参照されるコレクションに追加されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="60db1-1262">コレクション型で指定された要件を満たす必要がありますの対象に[コレクション初期化子](expressions.md#collection-initializers)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="60db1-1263">インデックス初期化子の引数は、1 回だけ常に評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="60db1-1264">したがって、引数は、(例: 空の入れ子になった初期化子) により使用される作業終了、場合でもは、副作用が評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="60db1-1265">次のクラスは、2 つの座標の点を表します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="60db1-1266">インスタンス`Point`作成および初期化を次のようにできます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="60db1-1267">同じ効果があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="60db1-1268">場所`__a`一時変数で、それ以外の場合に非表示とアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="60db1-1269">次のクラスは、2 つの点から作成された四角形を表します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="60db1-1270">インスタンス`Rectangle`作成および初期化を次のようにできます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="60db1-1271">同じ効果があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1271">which has the same effect as</span></span>
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
<span data-ttu-id="60db1-1272">場所`__r`、`__p1`と`__p2`一時変数アクセス、それ以外の場合です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="60db1-1273">場合`Rectangle`のコンス トラクターでは、2 つが埋め込まれた`Point`インスタンス</span><span class="sxs-lookup"><span data-stu-id="60db1-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="60db1-1274">次の構成体は、埋め込みの初期化に使用できる`Point`の新しいインスタンスを割り当てる代わりにインスタンス。</span><span class="sxs-lookup"><span data-stu-id="60db1-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="60db1-1275">同じ効果があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="60db1-1276">C では、次の例では、の適切な定義を指定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1276">Given an appropriate definition of C, the following example:</span></span>
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
<span data-ttu-id="60db1-1277">この一連の割り当てと同等です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1277">is equivalent to this series of assignments:</span></span>
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
<span data-ttu-id="60db1-1278">場所`__c`など、生成された変数は非表示と、ソース コードにアクセスできないです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="60db1-1279">なお、引数の`[0,0]`は 1 回だけ評価、および引数を`[1,2]`使用されない場合でも、1 回評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="60db1-1280">コレクション初期化子</span><span class="sxs-lookup"><span data-stu-id="60db1-1280">Collection initializers</span></span>

<span data-ttu-id="60db1-1281">コレクション初期化子では、コレクションの要素を指定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1281">A collection initializer specifies the elements of a collection.</span></span>

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

<span data-ttu-id="60db1-1282">コレクション初期化子で囲まれた、要素の初期化子のシーケンスから成る`{`と`}`トークンし、コンマで区切られました。</span><span class="sxs-lookup"><span data-stu-id="60db1-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="60db1-1283">各要素の初期化子の初期化中にコレクション オブジェクトに追加する要素を指定およびで囲まれた式の一覧から成る`{`と`}`トークンし、コンマで区切られました。</span><span class="sxs-lookup"><span data-stu-id="60db1-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="60db1-1284">1 つの式の要素の初期化子は、中かっこなしで記述できますが、メンバー初期化子とあいまいさを避けるために、代入式にすることはできませんし。</span><span class="sxs-lookup"><span data-stu-id="60db1-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="60db1-1285">*Non_assignment_expression*で定義されている運用[式](expressions.md#expression)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="60db1-1286">コレクション初期化子を含むオブジェクト作成式の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="60db1-1287">実装する型のコレクション オブジェクトをコレクション初期化子を適用する必要があります`System.Collections.IEnumerable`コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="60db1-1288">コレクション初期化子を呼び出す順序で要素を指定された各、`Add`ターゲット上のメソッドは、引数のリストを使用して、通常のメンバーの参照を適用する要素初期化子の式のリストを持つオブジェクトし、オーバー ロードの呼び出しごとの解決。</span><span class="sxs-lookup"><span data-stu-id="60db1-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="60db1-1289">そのため、コレクション オブジェクトが、名前の該当するインスタンスまたは拡張メソッドをいる必要があります`Add`の各要素の初期化子。</span><span class="sxs-lookup"><span data-stu-id="60db1-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="60db1-1290">次のクラスは、名前と電話番号のリストで連絡先を表します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="60db1-1291">A`List<Contact>`作成および初期化を次のようにできます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
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
<span data-ttu-id="60db1-1292">同じ効果があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1292">which has the same effect as</span></span>
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
<span data-ttu-id="60db1-1293">場所`__clist`、`__c1`と`__c2`一時変数アクセス、それ以外の場合です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="60db1-1294">配列作成式</span><span class="sxs-lookup"><span data-stu-id="60db1-1294">Array creation expressions</span></span>

<span data-ttu-id="60db1-1295">*Array_creation_expression*の新しいインスタンスを作成するために使用する*array_type*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="60db1-1296">最初の形式の配列作成式では、それぞれの個別の式を式の一覧から削除した結果の型の配列インスタンスを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="60db1-1297">配列作成式ではたとえば、`new int[10,20]`型の配列インスタンスの生成`int[,]`と、配列作成式`new int[10][,]`型の配列が生成される`int[][,]`。</span><span class="sxs-lookup"><span data-stu-id="60db1-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="60db1-1298">式のリスト内の各式は型でなければなりません`int`、 `uint`、 `long`、または`ulong`、または 1 つ以上のこれらの型に暗黙的に変換します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="60db1-1299">各式の値は、新しく割り当てられた配列のインスタンスに対応する次元の長さを決定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="60db1-1300">コンパイル時エラーには、配列の次元の長さは 0 以上である必要があります、ため、 *constant_expression*負の値式の一覧。</span><span class="sxs-lookup"><span data-stu-id="60db1-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="60db1-1301">Unsafe コンテキストでは可 ([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts))、配列のレイアウトは指定されていません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="60db1-1302">配列作成式最初のフォームにはには、配列初期化子が含まれている場合は、式のリスト内の各式は定数である必要があり、配列初期化子の式のリストで指定されたランクと次元の長さが一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="60db1-1303">2 番目または 3 番目のフォームの配列作成式で配列初期化子を指定した配列の型、ランク付けの指定子のランクが一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="60db1-1304">各次元の長さは、配列初期化子の対応する入れ子のレベルの各要素の数から推論されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="60db1-1305">したがって、式</span><span class="sxs-lookup"><span data-stu-id="60db1-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="60db1-1306">正確に対応しています</span><span class="sxs-lookup"><span data-stu-id="60db1-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="60db1-1307">3 番目の形式の配列作成式として参照されます、***配列作成式を暗黙的に型指定された***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="60db1-1308">配列の要素の型が明示的に指定されていないが、最も一般的な種類として決定する点を除いて、2 番目の形式に似ています ([一連の式の最適な一般的な種類を検索する](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) の配列内の式のセット初期化子。</span><span class="sxs-lookup"><span data-stu-id="60db1-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="60db1-1309">、多次元配列の 1 つの場所など、 *rank_specifier*に少なくとも 1 つのコンマを含んでいますこのセットはすべてで構成されます*式*s がで検出された入れ子になった*array_initializer*秒。</span><span class="sxs-lookup"><span data-stu-id="60db1-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="60db1-1310">配列初期化子の詳細については[配列初期化子](arrays.md#array-initializers)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="60db1-1311">配列作成式の評価結果は、新しく割り当てられた配列インスタンスへの参照を namely の値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="60db1-1312">配列作成式の実行時の処理は、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="60db1-1313">ディメンションの長さの式、 *expression_list*左から右への順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="60db1-1314">暗黙的な変換をそれぞれの式の評価に従って ([暗黙的な変換](conversions.md#implicit-conversions))、次の種類のいずれかに実行されます: `int`、 `uint`、 `long`、`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="60db1-1315">暗黙的な変換が存在するこのリストの最初の型が選択されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="60db1-1316">式またはそれ以降の暗黙的な変換の評価は、例外を発生させ場合、は、それ以上の式は評価されませんしさらに手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="60db1-1317">次元の長さの計算値は、次のように検証されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="60db1-1318">1 つ以上の値は 0 より小さい場合、`System.OverflowException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="60db1-1319">指定した次元の長さを持つ配列インスタンスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="60db1-1320">新しいインスタンスを割り当てることができる十分なメモリがない場合、`System.OutOfMemoryException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="60db1-1321">新しい配列インスタンスのすべての要素は、既定値に初期化されます ([既定値](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="60db1-1322">配列作成式に配列初期化子が含まれている場合、配列初期化子内の各式が評価され、配列の対応する要素に割り当てられています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="60db1-1323">評価と割り当てが、式が配列初期化子で記述された順序で実行、つまり、要素が昇順に最初に増やすと、右端の次元でインデックス順に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="60db1-1324">指定された式または対応する配列要素への割り当てを後続の評価は、例外を発生させ場合、残りの要素が初期化されていません (し、残りの要素が既定値がそのため)。</span><span class="sxs-lookup"><span data-stu-id="60db1-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="60db1-1325">配列作成式が配列型の要素の配列をインスタンス化を許可しますが、このような配列の要素を手動で初期化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="60db1-1326">たとえば、ステートメント</span><span class="sxs-lookup"><span data-stu-id="60db1-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="60db1-1327">型の 100 個の要素を持つ 1 次元配列を作成`int[]`です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="60db1-1328">各要素の初期値は`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="60db1-1329">サブ配列、およびステートメントのインスタンス化も同じ配列作成式のことはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="60db1-1330">コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1330">results in a compile-time error.</span></span> <span data-ttu-id="60db1-1331">サブ配列のインスタンス化は、手動で代わりに実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="60db1-1332">配列の配列に、サブ配列は、すべて同じ長さの場合は、「四角形」図形がある場合は、多次元配列を使用する方が効率的です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="60db1-1333">上記の例では、配列の配列のインスタンス化が 101 個のオブジェクトを作成します。-外側の配列が 1 つと 100 のサブ配列。</span><span class="sxs-lookup"><span data-stu-id="60db1-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="60db1-1334">それに対して</span><span class="sxs-lookup"><span data-stu-id="60db1-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="60db1-1335">のみ、1 つのオブジェクト、2 次元の配列を作成し、単一のステートメントで割り当てを行うこと。</span><span class="sxs-lookup"><span data-stu-id="60db1-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="60db1-1336">暗黙的に型指定された配列作成式の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="60db1-1337">最後の式がコンパイル時エラーを発生も`int`も`string`、他の暗黙的に変換できるし、入力はない最も一般的な存在です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="60db1-1338">明示的に型指定された配列作成式に必要ここでは、たとえば、型を指定する`object[]`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="60db1-1339">または、要素のいずれかに共通の基本型が推論される要素の型になりますし、キャストできます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="60db1-1340">暗黙的に型指定された配列作成式は、匿名オブジェクト初期化子と組み合わせることができます ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) 匿名で作成するデータ構造を入力します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="60db1-1341">例:</span><span class="sxs-lookup"><span data-stu-id="60db1-1341">For example:</span></span>
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

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="60db1-1342">デリゲート作成式</span><span class="sxs-lookup"><span data-stu-id="60db1-1342">Delegate creation expressions</span></span>

<span data-ttu-id="60db1-1343">A *delegate_creation_expression*の新しいインスタンスを作成するために使用する*delegate_type*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="60db1-1344">デリゲート作成式の引数が、メソッド グループ、匿名関数またはコンパイル時の型の値にする必要があります`dynamic`または*delegate_type*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="60db1-1345">引数が、メソッド グループである場合は、識別し、インスタンス メソッド、デリゲートを作成する対象のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="60db1-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="60db1-1346">引数が匿名関数の場合、パラメーターと、デリゲート ターゲットのメソッド本体が直接定義します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="60db1-1347">引数が値の場合、コピーを作成するためのデリゲート インスタンスを識別します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="60db1-1348">場合、*式*コンパイル時の型を持つ`dynamic`、 *delegate_creation_expression*が動的にバインドされている ([動的バインド](expressions.md#dynamic-binding))、および次の規則実行時の型を使用して実行時に適用されます、*式*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="60db1-1349">それ以外の場合、ルールは、コンパイル時に適用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="60db1-1350">バインド時の処理、 *delegate_creation_expression*フォームの`new D(E)`ここで、`D`は、 *delegate_type*と`E`は、*式*、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="60db1-1351">場合`E`はメソッド グループでは、デリゲート作成式はメソッド グループ変換と同じ方法で処理 ([メソッド グループ変換](conversions.md#method-group-conversions)) から`E`に`D`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="60db1-1352">場合`E`、匿名関数は、匿名関数の変換と同じ方法でデリゲート作成式が処理される ([匿名関数の変換](conversions.md#anonymous-function-conversions)) から`E`に`D`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="60db1-1353">場合`E`、値は、`E`互換である必要があります ([デリゲートの宣言](delegates.md#delegate-declarations)) と`D`、され、結果は新しく作成された型のデリゲートへの参照を`D`同じ呼び出しを参照ボックスの一覧として`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="60db1-1354">場合`E`と互換性がない`D`コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="60db1-1355">実行時の処理、 *delegate_creation_expression*フォームの`new D(E)`ここで、`D`は、 *delegate_type*と`E`は、*式*、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="60db1-1356">場合`E`はメソッド グループでは、メソッド グループ変換としてデリゲート作成式が評価されます ([メソッド グループ変換](conversions.md#method-group-conversions)) から`E`に`D`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="60db1-1357">場合`E`、匿名関数は、デリゲートの作成は、匿名関数の変換からとして評価されます。`E`に`D`([匿名関数の変換](conversions.md#anonymous-function-conversions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="60db1-1358">場合`E`の値であり、 *delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="60db1-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="60db1-1359">`E` 評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1359">`E` is evaluated.</span></span> <span data-ttu-id="60db1-1360">この評価は、例外を発生させ、その後の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="60db1-1361">場合の値`E`は`null`、`System.NullReferenceException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="60db1-1362">デリゲート型の新しいインスタンス`D`が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="60db1-1363">新しいインスタンスを割り当てることができる十分なメモリがない場合、`System.OutOfMemoryException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="60db1-1364">指定されたデリゲート インスタンスと同じ呼び出しリストを持つ新しいデリゲート インスタンスが初期化されて`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="60db1-1365">デリゲートがインスタンス化され、デリゲートの有効期間にわたって一定時に、デリゲートの呼び出しリストが決まります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="60db1-1366">つまりが作成されたら、デリゲートの呼び出し可能なターゲット エンティティを変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="60db1-1367">ときに 2 つのデリゲートを組み合わせるか、別の 1 つが削除された ([デリゲートの宣言](delegates.md#delegate-declarations))、新しいデリゲートの結果は、既存のデリゲートには、その内容が変更はありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="60db1-1368">場合によっては、プロパティ、インデクサー、ユーザー定義演算子、インスタンス コンス トラクター、デストラクター、または静的コンス トラクターを参照するデリゲートを作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="60db1-1369">前述のように、デリゲートがから作成するとき、メソッド グループ、仮パラメーター リストと、デリゲートの戻り値の型を特定のオーバー ロードされたメソッドを選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="60db1-1370">例</span><span class="sxs-lookup"><span data-stu-id="60db1-1370">In the example</span></span>
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
<span data-ttu-id="60db1-1371">`A.f`フィールドは、2 つ目を参照するデリゲートを使用して初期化`Square`メソッド メソッドは、仮パラメーター リストおよび戻り値の型に正確に一致するため、`DoubleFunc`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="60db1-1372">2 つ目が`Square`メソッドが存在していない、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="60db1-1373">匿名のオブジェクト作成式</span><span class="sxs-lookup"><span data-stu-id="60db1-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="60db1-1374">*Anonymous_object_creation_expression*匿名型のオブジェクトを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

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

<span data-ttu-id="60db1-1375">匿名オブジェクト初期化子は、匿名型を宣言し、その型のインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="60db1-1376">匿名型は、名前のないクラス型から直接継承される`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="60db1-1377">匿名型のメンバーは、一連の型のインスタンスを作成するために使用する匿名オブジェクト初期化子から推論される読み取り専用プロパティです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="60db1-1378">フォームの匿名オブジェクト初期化子具体的には、</span><span class="sxs-lookup"><span data-stu-id="60db1-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="60db1-1379">フォームの匿名型を宣言します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1379">declares an anonymous type of the form</span></span>
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
<span data-ttu-id="60db1-1380">各`Tx`対応する式の型は、`ex`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="60db1-1381">使用される式を*member_declarator*型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="60db1-1382">したがって、式のコンパイル時エラーが、 *member_declarator*を null または匿名関数です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="60db1-1383">安全でない型を持つ式のコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="60db1-1384">匿名型のパラメーターの名前、`Equals`メソッドは、コンパイラによって自動的に生成され、プログラム テキストでは参照できません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="60db1-1385">同じ順序で一連のコンパイル時の型と同じ名前のプロパティを指定する 2 つの匿名オブジェクト初期化子では、同じプログラム内で、同じ匿名型のインスタンスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="60db1-1386">例</span><span class="sxs-lookup"><span data-stu-id="60db1-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="60db1-1387">ため、最後の行で割り当てが許可`p1`と`p2`が同じ匿名型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="60db1-1388">`Equals`と`GetHashcode`匿名型のメソッドから継承されたメソッドをオーバーライド`object`の観点で定義されていると、`Equals`と`GetHashcode`プロパティの同じ匿名型の 2 つのインスタンスが等しくないように場合そのすべてのプロパティが等しい場合のみです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="60db1-1389">メンバー宣言子は、簡易名を省略できます ([型推論](expressions.md#type-inference))、メンバー アクセス ([コンパイル時の動的なオーバー ロードの解決の確認](expressions.md#compile-time-checking-of-dynamic-overload-resolution))、ベース アクセス ([へのアクセスの基本](expressions.md#base-access))または、null 条件メンバー アクセス ([プロジェクション初期化子として Null 条件式](expressions.md#null-conditional-expressions-as-projection-initializers))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="60db1-1390">これと呼ばれますが、***プロジェクション初期化子***の宣言と同じ名前のプロパティへの代入を短縮したものとします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="60db1-1391">具体的には、フォームのメンバー宣言の子</span><span class="sxs-lookup"><span data-stu-id="60db1-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="60db1-1392">それぞれ、次に、まったく同じです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="60db1-1393">そのため、プロジェクション初期化子で、*識別子*値とフィールドまたは値が割り当てられているプロパティを選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="60db1-1394">直感的には、プロジェクション初期化子は、値だけでなく、値の名前を射影します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="60db1-1395">Typeof 演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1395">The typeof operator</span></span>

<span data-ttu-id="60db1-1396">`typeof`演算子を使用して、取得、`System.Type`型のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="60db1-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

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

<span data-ttu-id="60db1-1397">最初のフォーム*typeof_expression*から成る、`typeof`キーワードの後に、かっこで囲まれた*型*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="60db1-1398">この形式の式の結果は、`System.Type`示された型のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="60db1-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="60db1-1399">1 つしかない`System.Type`指定された型のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="60db1-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="60db1-1400">これにより、型の `T`、`typeof(T) == typeof(T)`は常に true です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="60db1-1401">*型*することはできません`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="60db1-1402">2 番目の形式の*typeof_expression*から成る、`typeof`キーワードの後に、かっこで囲まれた*unbound_type_name*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="60db1-1403">*Unbound_type_name*とよく似ていますが、 *type_name* ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names)) ことを除いて、 *unbound_type_name*が含まれています*generic_dimension_specifier*s を*type_name*が含まれています*type_argument_list*秒。</span><span class="sxs-lookup"><span data-stu-id="60db1-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="60db1-1404">ときのオペランドを*typeof_expression*を両方の文法を満たす一連のトークンは、 *unbound_type_name*と*type_name*、つまりそれが含まれる場合どちらも、 *generic_dimension_specifier*も*type_argument_list*、トークンのシーケンスがあると見なされます、 *type_name*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="60db1-1405">意味、 *unbound_type_name*は次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="60db1-1406">トークンのシーケンスに変換を*type_name*それぞれを置き換えることで、 *generic_dimension_specifier*で、 *type_argument_list*ことと同じ数のコンマとキーワード`object`として各*type_argument*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="60db1-1407">結果を評価*type_name*、中に、すべての型パラメーターの制約は無視されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="60db1-1408">*Unbound_type_name*構築された型に関連付けられているバインドされていないジェネリック型に解決される ([バインドされ、型がバインドされていない](types.md#bound-and-unbound-types))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="60db1-1409">結果、 *typeof_expression*は、`System.Type`オブジェクトのジェネリック型がバインドされていない結果します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="60db1-1410">3 番目の形式*typeof_expression*から成る、`typeof`キーワードの後に、かっこで囲まれた`void`キーワード。</span><span class="sxs-lookup"><span data-stu-id="60db1-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="60db1-1411">この形式の式の結果は、`System.Type`がない場合、型を表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="60db1-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="60db1-1412">によって返される型オブジェクト`typeof(void)`任意の型に対して返される型のオブジェクトとは異なります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="60db1-1413">この特別な種類のオブジェクトは、言語では、メソッドにリフレクションを使用できるようにするのインスタンスとの void メソッドを含む、任意のメソッドの戻り値の型を表す方法があるクラス ライブラリで役立ちます`System.Type`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="60db1-1414">`typeof`演算子は、型パラメーターで使用できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="60db1-1415">結果は、`System.Type`型パラメーターにバインドされていた、実行時の型のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="60db1-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="60db1-1416">`typeof`演算子は、構築された型またはバインドされていないジェネリック型でも使用できます ([バインドされ、型がバインドされていない](types.md#bound-and-unbound-types))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="60db1-1417">`System.Type` 、バインドされていないジェネリック型が同じオブジェクト、`System.Type`インスタンス型のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="60db1-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="60db1-1418">インスタンスの型は常に実行時に構築されたクローズ型ためその`System.Type`オブジェクトは、バインドされていないジェネリック型に型引数があるないときに使用して、実行時の型引数に依存します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="60db1-1419">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-1419">The example</span></span>
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
<span data-ttu-id="60db1-1420">次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1420">produces the following output:</span></span>
```
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

<span data-ttu-id="60db1-1421">なお`int`と`System.Int32`は同じ型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="60db1-1422">また、結果の`typeof(X<>)`型引数がの結果に依存しません`typeof(X<T>)`は。</span><span class="sxs-lookup"><span data-stu-id="60db1-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="60db1-1423">Checked と unchecked 演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="60db1-1424">`checked`と`unchecked`演算子を使用してコントロールを***オーバーフロー チェック コンテキスト***整数型の算術演算と変換します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="60db1-1425">`checked`演算子が checked コンテキストで含まれている式を評価し、`unchecked`演算子が unchecked コンテキストで含まれている式を評価します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="60db1-1426">A *checked_expression*または*unchecked_expression*に対応して、 *parenthesized_expression* ([のかっこで囲まれた式](expressions.md#parenthesized-expressions)) が含まれている式は、特定のオーバーフロー チェック コンテキストで評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="60db1-1427">オーバーフロー チェック コンテキストを制御することも、`checked`と`unchecked`ステートメント ([checked と unchecked ステートメント](statements.md#the-checked-and-unchecked-statements))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="60db1-1428">オーバーフロー チェック コンテキストを確立して、次の操作が影響を受ける、`checked`と`unchecked`演算子およびステートメント。</span><span class="sxs-lookup"><span data-stu-id="60db1-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="60db1-1429">定義済み`++`と`--`単項演算子 ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)と[前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)) オペランドが整数の場合、入力します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="60db1-1430">定義済み`-`単項演算子 ([単項マイナス演算子](expressions.md#unary-minus-operator))、整数型のオペランドがある場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="60db1-1431">定義済み`+`、 `-`、 `*`、および`/`二項演算子 ([算術演算子](expressions.md#arithmetic-operators))、両方のオペランドが整数型の場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="60db1-1432">明示的な数値変換 ([明示的な数値変換](conversions.md#explicit-numeric-conversions)) もう 1 つの整数型との間に 1 つの整数型から`float`または`double`を整数型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="60db1-1433">大きすぎて、変換先の型、操作が実行されるコントロールでありコンテキストの結果の動作を表すである結果を生成、上述の操作のいずれかの場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="60db1-1434">`checked`コンテキスト、操作が定数式である場合 ([定数式](expressions.md#constant-expressions))、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="60db1-1435">それ以外の場合、実行時に、操作が実行されると、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="60db1-1436">`unchecked`コンテキスト、結果は変換先の型に収まらない上位ビットが破棄されてによって切り捨てられます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="60db1-1437">非定数式 (実行時に評価される式) のいずれかで囲まれていない`checked`または`unchecked`演算子またはステートメントでは、既定のオーバーフロー チェック コンテキストは、 `unchecked` (コンパイラなどの外部要因しない限り、スイッチと実行環境の構成) の呼び出しの`checked`評価します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="60db1-1438">定数式 (コンパイル時に完全に評価される式)、既定のオーバーフロー チェック コンテキストは常に`checked`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="60db1-1439">定数式を明示的に配置しない限り、`unchecked`コンテキストでは、常に式のコンパイル時の評価中に発生するオーバーフローにコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="60db1-1440">匿名関数の本体は受けません`checked`または`unchecked`匿名関数が発生するコンテキスト。</span><span class="sxs-lookup"><span data-stu-id="60db1-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="60db1-1441">例</span><span class="sxs-lookup"><span data-stu-id="60db1-1441">In the example</span></span>
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
<span data-ttu-id="60db1-1442">コンパイル時に評価される式のどちらでもないために、コンパイル時エラーは報告されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="60db1-1443">実行時、`F`メソッドがスローされます、 `System.OverflowException`、および`G`-727379968 (範囲外の結果の下位 32 ビット) を返します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="60db1-1444">動作、`H`メソッドは、既定のオーバーフロー チェック コンテキストをコンパイル時にによって異なりますが、これと同じであるか`F`または同じ`G`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="60db1-1445">例</span><span class="sxs-lookup"><span data-stu-id="60db1-1445">In the example</span></span>
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
<span data-ttu-id="60db1-1446">内の定数式を評価するときに発生するオーバーフロー`F`と`H`で式を評価するために報告されることをコンパイル時エラーが発生する、`checked`コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="60db1-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="60db1-1447">定数式を評価するときにも、オーバーフローが発生`G`評価が行われるので、`unchecked`コンテキスト、オーバーフローが報告されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="60db1-1448">`checked`と`unchecked`演算子、オーバーフロー チェック コンテキスト内でテキストに含まれるこれらの操作の影響を受けるのみ、"`(`「と」`)`"トークンです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="60db1-1449">演算子は、式を評価した結果として呼び出される関数メンバーに影響を与えるありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="60db1-1450">例</span><span class="sxs-lookup"><span data-stu-id="60db1-1450">In the example</span></span>
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
<span data-ttu-id="60db1-1451">使用`checked`で`F`の評価には影響しません`x * y`で`Multiply`ため、`x * y`は既定のオーバーフロー チェック コンテキストで評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="60db1-1452">`unchecked`演算子は、16 進数表記で符号付き整数型の定数を記述するときに便利です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="60db1-1453">例:</span><span class="sxs-lookup"><span data-stu-id="60db1-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="60db1-1454">上記の 16 進定数の両方が型の`uint`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="60db1-1455">定数に含まれないため、`int`せず、範囲、`unchecked`演算子、キャストを`int`コンパイル時エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="60db1-1456">`checked`と`unchecked`演算子とステートメントは、数値計算をいくつかの特定の側面を制御するプログラマを許可します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="60db1-1457">ただし、一部の数値演算子の動作は、そのオペランドのデータ型によって異なります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="60db1-1458">たとえば、小数点以下 2 桁を常に乗算することで例外が発生オーバーフロー内でも、明示的に`unchecked`を構築します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="60db1-1459">同様に、2 つの乗算フローティング状態になったことはありません結果オーバーフロー例外は発生内でも、明示的に`checked`を構築します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="60db1-1460">さらに、その他の演算子はチェックのモードで影響しない、既定のかどうか、または明示的な。</span><span class="sxs-lookup"><span data-stu-id="60db1-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="60db1-1461">既定値の式</span><span class="sxs-lookup"><span data-stu-id="60db1-1461">Default value expressions</span></span>

<span data-ttu-id="60db1-1462">既定値の式は、既定値を取得するために使用 ([既定値](variables.md#default-values)) の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="60db1-1463">通常、可能性がありますが認識されていない場合は、型パラメーターが値型または参照型であるため、型パラメーターの既定値の式が使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="60db1-1464">(から変換が存在しない、`null`型パラメーターが参照型とわかっている場合を除き、型パラメーターにリテラルです)。</span><span class="sxs-lookup"><span data-stu-id="60db1-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="60db1-1465">場合、*型*で、 *default_value_expression*評価参照型には、実行時に、結果が`null`その型に変換します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="60db1-1466">場合、*型*で、 *default_value_expression*評価値の型には、実行時に、結果が、 *value_type*の既定値 ([既定コンス トラクター](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="60db1-1467">A *default_value_expression*定数式です ([定数式](expressions.md#constant-expressions)) 場合は、型が参照型または参照型があることがわかっている型パラメーター ([型パラメーター制約](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="60db1-1468">さらに、 *default_value_expression*定数式は、型は、次の値の型のいずれかの場合: `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、`long`、 `ulong`、 `char`、 `float`、 `double`、 `decimal`、 `bool`、または列挙型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="60db1-1469">Nameof 式</span><span class="sxs-lookup"><span data-stu-id="60db1-1469">Nameof expressions</span></span>

<span data-ttu-id="60db1-1470">A *nameof_expression*文字列定数としてプログラム エンティティの名前を取得するために使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

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

<span data-ttu-id="60db1-1471">文法的に言うと、 *named_entity*オペランドが式では常にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="60db1-1472">`nameof`は予約済みキーワードの場合は、nameof 式は、簡易名の呼び出しで構文的にあいまいな常に`nameof`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="60db1-1473">互換性の理由から、名の参照の場合 ([簡易名](expressions.md#simple-names)) 名の`nameof`成功すると、式として扱われます、 *invocation_expression*呼び出しは、かどうかに関係なく--法律です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="60db1-1474">それ以外の場合は、 *nameof_expression*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="60db1-1475">意味、 *named_entity*の*nameof_expression*ことの意味を式としては、いずれかとして、 *simple_name*、 *base_access*または*member_access*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="60db1-1476">ただし、場所、ルックアップで説明されている[簡易名](expressions.md#simple-names)と[メンバー アクセス](expressions.md#member-access)静的においては、インスタンス メンバーが見つかったため、エラーが発生、 *nameof_expression*このようなエラーを生成しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="60db1-1477">コンパイル時エラーには、 *named_entity*が、メソッド グループを指定する、 *type_argument_list*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="60db1-1478">コンパイル時間のエラーには、 *named_entity_target*に、型を持つ`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="60db1-1479">A *nameof_expression*型の定数式は、 `string`、実行時に影響しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="60db1-1480">具体的には、その*named_entity*は評価されず、および限定代入分析の目的では無視されます ([単純式での一般的な規則](variables.md#general-rules-for-simple-expressions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="60db1-1481">その値の最後の識別子では、 *named_entity* 、省略可能な最後の前に*type_argument_list*、次のように変換されました。</span><span class="sxs-lookup"><span data-stu-id="60db1-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="60db1-1482">プレフィックス"`@`"を使用する場合は、削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="60db1-1483">各*unicode_escape_sequence*対応する Unicode 文字に変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="60db1-1484">すべて*formatting_characters*が削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="60db1-1485">これらは、同じ変換の適用[識別子](lexical-structure.md#identifiers)識別子の間に等しいかどうかをテストするときにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="60db1-1486">TODO: 例</span><span class="sxs-lookup"><span data-stu-id="60db1-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="60db1-1487">匿名メソッド式</span><span class="sxs-lookup"><span data-stu-id="60db1-1487">Anonymous method expressions</span></span>

<span data-ttu-id="60db1-1488">*Anonymous_method_expression*匿名関数を定義する 2 つの方法の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="60db1-1489">さらに詳細は[匿名関数式](expressions.md#anonymous-function-expressions)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="60db1-1490">単項演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1490">Unary operators</span></span>

<span data-ttu-id="60db1-1491">`?`、 `+`、 `-`、 `!`、 `~`、 `++`、`--`キャスト、および`await`演算子は、単項演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

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

<span data-ttu-id="60db1-1492">場合のオペランドを*unary_expression*コンパイル時の型を持つ`dynamic`、動的にバインドされている ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="60db1-1493">この場合、コンパイル時はの入力、 *unary_expression*は`dynamic`、オペランドの実行時の型を使用して実行時に以下に示す解決が行わします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="60db1-1494">Null 条件演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1494">Null-conditional operator</span></span>

<span data-ttu-id="60db1-1495">Null 条件演算子では、そのオペランドが null でない場合にのみに、操作の一覧が、オペランドに適用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="60db1-1496">それ以外の場合は、演算子を適用した結果、`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1496">Otherwise the result of applying the operator is `null`.</span></span>

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

<span data-ttu-id="60db1-1497">操作の一覧には、メンバー アクセスと要素アクセス操作 (これは、null 条件自体である可能性があります)、だけでなく、呼び出しを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="60db1-1498">たとえば、式`a.b?[0]?.c()`は、 *null_conditional_expression*で、 *primary_expression* `a.b`と*null_conditional_operations* `?[0]` (要素の null 条件付きアクセス)、 `?.c` (null 条件メンバー アクセス) および`()`(呼び出し)。</span><span class="sxs-lookup"><span data-stu-id="60db1-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="60db1-1499">*Null_conditional_expression* `E`で、 *primary_expression* `P`、`E0`ヘッダーファイル リード削除することで取得した式を指定する`?`のそれぞれから、 *null_conditional_operations*の`E`がある 1 つ。</span><span class="sxs-lookup"><span data-stu-id="60db1-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="60db1-1500">概念的には、`E0`によって表される null チェックの場合に評価される式です、 `?`s は検索、`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="60db1-1501">を描く遷移`E1`ヘッダーファイル先頭削除することで取得した式を指定する`?`のみからの最初、 *null_conditional_operations*で`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="60db1-1502">これが生じる、*プライマリ式*(1 つだけを使用する必要がある場合`?`) または別*null_conditional_expression*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="60db1-1503">たとえば場合、`E`式です`a.b?[0]?.c()`、し`E0`式です`a.b[0].c()`と`E1`式は、`a.b[0]?.c()`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="60db1-1504">場合`E0`し、何もとして分類されます`E`nothing として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="60db1-1505">それ以外の場合、E は値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="60db1-1506">`E0` `E1`の意味を判断するために使用`E`:</span><span class="sxs-lookup"><span data-stu-id="60db1-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="60db1-1507">場合`E`として発生する*statement_expression*の意味`E`ステートメントと同じです</span><span class="sxs-lookup"><span data-stu-id="60db1-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="60db1-1508">P は 1 回だけ評価されることを除きます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="60db1-1509">の場合`E0`コンパイル時エラーが発生した nothing として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="60db1-1510">それ以外の場合、ように`T0`の型である`E0`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="60db1-1511">場合`T0`型パラメーターには、参照型または null 非許容値型では、コンパイル時エラーが発生したが不明です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="60db1-1512">場合`T0`null 非許容値型では、次の種類は`E`は`T0?`との意味`E`と同じです</span><span class="sxs-lookup"><span data-stu-id="60db1-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="60db1-1513">点を除いて`P`は 1 回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="60db1-1514">それ以外の場合、電子メールの種類は、T0、および電子メールの意味は同じ</span><span class="sxs-lookup"><span data-stu-id="60db1-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="60db1-1515">点を除いて`P`は 1 回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="60db1-1516">場合`E1`自体では、 *null_conditional_expression*、し、この規則が適用されます、もう一度のテストを入れ子`null`なくなるまで`?`の式が、一番下まで減少したとプライマリ式に`E0`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="60db1-1517">たとえば場合、式`a.b?[0]?.c()`ステートメントのようにステートメントの式、として発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="60db1-1518">その意味と同等です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="60db1-1519">もう一度と同等です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="60db1-1520">点を除いて`a.b`と`a.b[0]`1 回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="60db1-1521">場合に、その値が使用されているでのコンテキストで行われます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="60db1-1522">その意味と等価の最後の呼び出しの種類が null 非許容値型がない、.</span><span class="sxs-lookup"><span data-stu-id="60db1-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="60db1-1523">点を除いて`a.b`と`a.b[0]`1 回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="60db1-1524">プロジェクション初期化子として null 条件式</span><span class="sxs-lookup"><span data-stu-id="60db1-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="60db1-1525">Null 条件式としてのみ使用できます、 *member_declarator*で、 *anonymous_object_creation_expression* ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) 場合(null 条件の必要に応じて) メンバー アクセスで終了します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="60db1-1526">文法的に、この要件は、として表現できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="60db1-1527">これは、特殊なケースの文法の*null_conditional_expression*上。</span><span class="sxs-lookup"><span data-stu-id="60db1-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="60db1-1528">運用*member_declarator*で[匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)のみを含む、 *null_conditional_member_access*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="60db1-1529">ステートメントの式と null 条件式</span><span class="sxs-lookup"><span data-stu-id="60db1-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="60db1-1530">Null 条件式としてのみ使用できます、 *statement_expression* ([式ステートメント](statements.md#expression-statements)) 場合は、呼び出しで終了します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="60db1-1531">文法的に、この要件は、として表現できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="60db1-1532">これは、特殊なケースの文法の*null_conditional_expression*上。</span><span class="sxs-lookup"><span data-stu-id="60db1-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="60db1-1533">運用*statement_expression*で[式ステートメント](statements.md#expression-statements)のみを含む、 *null_conditional_invocation_expression*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="60db1-1534">単項プラス演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1534">Unary plus operator</span></span>

<span data-ttu-id="60db1-1535">フォームの操作の`+x`、単項演算子のオーバー ロードの解決 ([単項演算子のオーバー ロードの解決](expressions.md#unary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-1536">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="60db1-1537">定義済みの単項プラス演算子は。</span><span class="sxs-lookup"><span data-stu-id="60db1-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="60db1-1538">これらの演算子のそれぞれについて、単に、オペランドの値になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="60db1-1539">単項マイナス演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1539">Unary minus operator</span></span>

<span data-ttu-id="60db1-1540">フォームの操作の`-x`、単項演算子のオーバー ロードの解決 ([単項演算子のオーバー ロードの解決](expressions.md#unary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-1541">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="60db1-1542">定義済みの否定演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="60db1-1543">整数の否定:</span><span class="sxs-lookup"><span data-stu-id="60db1-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="60db1-1544">結果を差し引いて計算`x`ゼロから。</span><span class="sxs-lookup"><span data-stu-id="60db1-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="60db1-1545">場合の値の`x`オペランドの型の表現可能な最小の値は、(-2 ^31`int`または-2 ^63 `long`) の算術否定し`x`オペランドの型で表すことはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1545">If the value of of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="60db1-1546">内でこのような場合、`checked`コンテキスト、`System.OverflowException`スロー; 内に発生する場合は、`unchecked`コンテキスト、結果はオペランドの値と、オーバーフローが報告されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="60db1-1547">あるかどうか、否定演算子のオペランドも型`uint`、型に変換されます`long`、および結果の型は`long`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="60db1-1548">例外は、許可するルール、 `int` -2147483648 の値 (-2 ^31)、10 進数の整数リテラルとして記述すること ([整数リテラル](lexical-structure.md#integer-literals))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="60db1-1549">あるかどうか、否定演算子のオペランドも型`ulong`コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="60db1-1550">例外は、許可するルール、`long`値-9223372036854775808 (-2 ^63)、10 進数の整数リテラルとして記述すること ([整数リテラル](lexical-structure.md#integer-literals))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="60db1-1551">浮動小数点の否定:</span><span class="sxs-lookup"><span data-stu-id="60db1-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="60db1-1552">結果の値は、`x`符号を反転させたとします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="60db1-1553">場合`x`が NaN の場合、結果が NaN でも。</span><span class="sxs-lookup"><span data-stu-id="60db1-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="60db1-1554">10 進数の否定:</span><span class="sxs-lookup"><span data-stu-id="60db1-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="60db1-1555">結果を差し引いて計算`x`ゼロから。</span><span class="sxs-lookup"><span data-stu-id="60db1-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="60db1-1556">10 進数の否定は、単項マイナス型の演算子を使用すると、`System.Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="60db1-1557">論理否定演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1557">Logical negation operator</span></span>

<span data-ttu-id="60db1-1558">フォームの操作の`!x`、単項演算子のオーバー ロードの解決 ([単項演算子のオーバー ロードの解決](expressions.md#unary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-1559">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="60db1-1560">1 つだけの定義済みの論理否定演算子が存在します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="60db1-1561">この演算子はオペランドの論理否定演算を計算します。オペランドが場合`true`、結果は`false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="60db1-1562">オペランドが場合`false`、結果は`true`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="60db1-1563">ビットごとの補数演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1563">Bitwise complement operator</span></span>

<span data-ttu-id="60db1-1564">フォームの操作の`~x`、単項演算子のオーバー ロードの解決 ([単項演算子のオーバー ロードの解決](expressions.md#unary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-1565">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="60db1-1566">定義済みのビットごとの補数演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="60db1-1567">操作の結果のビットごとの補数はこれらの演算子のそれぞれについて、`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="60db1-1568">すべての列挙型`E`暗黙的に次のビットごとの補数演算子を提供します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="60db1-1569">評価結果`~x`ここで、`x`列挙型の式を指定`E`基になる型`U`、評価と同じでは正確に`(E)(~(U)x)`ことを除いてへの変換`E`はとして常に実行される場合は、`unchecked`コンテキスト ([checked と unchecked 演算子](expressions.md#the-checked-and-unchecked-operators))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="60db1-1570">前置インクリメントとデクリメント演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="60db1-1571">オペランドを前置インクリメントまたはデクリメントの操作が、変数、プロパティ アクセス、またはインデクサー アクセスとして分類される式をする必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="60db1-1572">操作の結果は、オペランドと同じ型の値です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="60db1-1573">場合は、プレフィックスのオペランドがインクリメントまたはデクリメント演算は、プロパティまたはインデクサー アクセス、プロパティまたはインデクサーが両方必要、`get`と`set`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="60db1-1574">サポートしていない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-1575">単項演算子のオーバー ロードの解決 ([単項演算子のオーバー ロードの解決](expressions.md#unary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-1576">定義済みの`++`と`--`演算子は、次の種類の存在: `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、 `decimal`、および任意の列挙型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="60db1-1577">定義済み`++`演算子がオペランドを定義済みに 1 を追加することによって生成された値を返す`--`演算子オペランドから 1 を引いた値を返します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="60db1-1578">`checked`場合この加算または減算の結果が結果の型の範囲外と結果型が整数型または列挙型では、コンテキスト、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="60db1-1579">前置インクリメントの実行時の処理またはフォームの操作をデクリメント`++x`または`--x`次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="60db1-1580">場合`x`変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="60db1-1581">`x` 変数を生成するために評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="60db1-1582">値は、選択した演算子が呼び出される`x`引数として。</span><span class="sxs-lookup"><span data-stu-id="60db1-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="60db1-1583">評価によって指定した位置に、演算子によって返される値が格納されている`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="60db1-1584">演算子によって返される値は、操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="60db1-1585">場合`x`プロパティまたはインデクサーのアクセスに分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="60db1-1586">インスタンス式 (場合`x`ない`static`) および引数リスト (場合`x`インデクサー アクセス) に関連付けられている`x`が評価されると、その後で、結果の使用`get`と`set`アクセサーの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="60db1-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="60db1-1587">`get`のアクセサー`x`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="60db1-1588">選択した演算子が呼び出されるによって返される値と、`get`引数にアクセサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="60db1-1589">`set`のアクセサー`x`が呼び出されると演算子によって返される値とその`value`引数。</span><span class="sxs-lookup"><span data-stu-id="60db1-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="60db1-1590">演算子によって返される値は、操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="60db1-1591">`++`と`--`演算子もサポートして後置表記法 ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="60db1-1592">結果では通常、`x++`または`x--`の値は、`x`操作の前に一方の結果`++x`または`--x`の値は、`x`操作の完了後します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="60db1-1593">いずれの場合も、`x`自体、操作の後に同じ値に含まれています。</span><span class="sxs-lookup"><span data-stu-id="60db1-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="60db1-1594">`operator++`または`operator--`実装は、いずれかの後置または前置表記を使用して呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="60db1-1595">2 つの表記の個別の演算子の実装を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="60db1-1596">キャスト式</span><span class="sxs-lookup"><span data-stu-id="60db1-1596">Cast expressions</span></span>

<span data-ttu-id="60db1-1597">A *cast_expression*式を指定した型に明示的に変換するために使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="60db1-1598">A *cast_expression*フォームの`(T)E`ここで、`T`は、*型*と`E`は、 *unary_expression*、明示的なを実行します変換 ([明示的な変換](conversions.md#explicit-conversions)) の値の`E`入力`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="60db1-1599">明示的な変換が存在しない場合`E`に`T`、バインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="60db1-1600">それ以外の場合、結果は、明示的な変換によって生成された値になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="60db1-1601">結果は常に、値として分類場合でも`E`変数を表します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="60db1-1602">文法、 *cast_expression*特定構文のあいまいさにつながります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="60db1-1603">たとえば、式`(x)-y`いずれかとして解釈される可能性を*cast_expression* (のキャスト`-y`を入力する`x`) か、または、 *additive_expression*と組み合わせて、*parenthesized_expression* (値を計算する`x - y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="60db1-1604">解決するのには*cast_expression*あいまいさは、次のルールが存在します。1 つまたは複数のシーケンス*トークン*s ([空白](lexical-structure.md#white-space)) 囲まれていると見なされますの開始をかっこで囲まれた、 *cast_expression*次の少なくとも 1 つが true の場合にのみ。</span><span class="sxs-lookup"><span data-stu-id="60db1-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="60db1-1605">トークンのシーケンスの文法が正しいは、*型*ではなく、*式*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="60db1-1606">トークンのシーケンスの文法が正しいは、*型*、閉じかっこの直後に続くトークンは、トークンと"`~`"、トークン"`!`"、トークン"`(`"、 *識別子*([Unicode 文字のエスケープ シーケンス](lexical-structure.md#unicode-character-escape-sequences))、*リテラル*([リテラル](lexical-structure.md#literals))、または any*キーワード*([キーワード](lexical-structure.md#keywords)) を除く`as`と`is`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="60db1-1607">トークンのシーケンスは、特定の文法的な運用環境に従う必要があるだけのことを意味上の「正しい文法」ターム。</span><span class="sxs-lookup"><span data-stu-id="60db1-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="60db1-1608">構成要素である識別子の実際の意味具体的には考慮されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="60db1-1609">たとえば場合、`x`と`y`は、識別子、`x.y`正しい文法型の場合は、場合でも`x.y`型を表すは実際にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="60db1-1610">場合、次の曖昧性除去ルールから`x`と`y`識別子、 `(x)y`、`(x)(y)`と`(x)(-y)`は*cast_expression*、s が`(x)-y`でない場合でも`x`型を識別します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="60db1-1611">ただし場合、`x`は定義済みの型を識別するキーワードです (など`int`)、4 つの形式は*cast_expression*s (このようなキーワード可能性があるできなかったため、式を単独で)。</span><span class="sxs-lookup"><span data-stu-id="60db1-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="60db1-1612">Await 式</span><span class="sxs-lookup"><span data-stu-id="60db1-1612">Await expressions</span></span>

<span data-ttu-id="60db1-1613">Await 演算子は、オペランドで表される非同期操作が完了するまでに、外側の非同期関数の評価を中断に使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="60db1-1614">*Await_expression*は非同期関数の本体でのみ使用できます ([反復子](classes.md#iterators))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="60db1-1615">最も外側の非同期関数では、 *await_expression*これらの場所で発生しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="60db1-1616">入れ子になった (非 async) 匿名関数の内部</span><span class="sxs-lookup"><span data-stu-id="60db1-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="60db1-1617">ブロックの内側、 *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="60db1-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="60db1-1618">Unsafe コンテキストで</span><span class="sxs-lookup"><span data-stu-id="60db1-1618">In an unsafe context</span></span>

<span data-ttu-id="60db1-1619">なお、 *await_expression*内のほとんどの場所で発生することはできません、 *query_expression*ものが構文的にでは、非同期ラムダ式を使用して変換されるため、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="60db1-1620">非同期関数の内部で`await`識別子として使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="60db1-1621">そのため、await 式と識別子を含むさまざまな式の構文のあいまいさはありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="60db1-1622">非同期関数では、外部`await`は通常の識別子として機能します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="60db1-1623">オペランドを*await_expression*と呼ばれますが、***タスク***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="60db1-1624">時に完了できない可能性がありますまたは可能性のある非同期操作を表す、 *await_expression*が評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="60db1-1625">Await 演算子では、待機中のタスクが完了するまでに、外側の非同期関数の実行を中断し、その結果を取得します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="60db1-1626">待機可能な式</span><span class="sxs-lookup"><span data-stu-id="60db1-1626">Awaitable expressions</span></span>

<span data-ttu-id="60db1-1627">Await 式のタスクである必要が***待機可能物***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="60db1-1628">式`t`は、次のいずれかが保持している場合は待機可能物。</span><span class="sxs-lookup"><span data-stu-id="60db1-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="60db1-1629">`t` コンパイル時の型です。 `dynamic`</span><span class="sxs-lookup"><span data-stu-id="60db1-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="60db1-1630">`t` アクセス可能なインスタンスまたは拡張メソッドが呼び出されますが`GetAwaiter`パラメーターと型パラメーターと戻り値の型を`A`は次のすべてを保持します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="60db1-1631">`A` インターフェイスを実装する`System.Runtime.CompilerServices.INotifyCompletion`(と呼びます`INotifyCompletion`簡潔にするため)</span><span class="sxs-lookup"><span data-stu-id="60db1-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="60db1-1632">`A` インスタンスがアクセスできる、読み取り可能なプロパティを持つ`IsCompleted`型の `bool`</span><span class="sxs-lookup"><span data-stu-id="60db1-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="60db1-1633">`A` アクセス可能なインスタンス メソッドを持つ`GetResult`パラメーターと型パラメーターを</span><span class="sxs-lookup"><span data-stu-id="60db1-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="60db1-1634">目的、`GetAwaiter`メソッドが取得するには、 ***awaiter***タスク。</span><span class="sxs-lookup"><span data-stu-id="60db1-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="60db1-1635">型`A`が呼び出されます、 ***awaiter 型***の await 式。</span><span class="sxs-lookup"><span data-stu-id="60db1-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="60db1-1636">目的、`IsCompleted`プロパティは、タスクが完了して既にかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="60db1-1637">そうである場合、評価を中断する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="60db1-1638">目的、`INotifyCompletion.OnCompleted`メソッドが「継続」タスクをサインアップするにはデリゲートつまり (型の`System.Action`) をタスクが完了すると呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="60db1-1639">目的、`GetResult`メソッドは、それが完了したら、タスクの結果を取得します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="60db1-1640">この結果、場合によっては、結果の値で正常に完了場合がありますまたはによってスローされる例外がある可能性があります、`GetResult`メソッド。</span><span class="sxs-lookup"><span data-stu-id="60db1-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="60db1-1641">分類の await 式</span><span class="sxs-lookup"><span data-stu-id="60db1-1641">Classification of await expressions</span></span>

<span data-ttu-id="60db1-1642">式`await t`式と同じように分類されます`(t).GetAwaiter().GetResult()`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="60db1-1643">したがって、戻り値の型の場合`GetResult`は`void`、 *await_expression* nothing として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="60db1-1644">非 void の戻り値の型がある場合`T`、 *await_expression*型の値として分類されます`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="60db1-1645">Await 式の実行時の評価</span><span class="sxs-lookup"><span data-stu-id="60db1-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="60db1-1646">式は、実行時に`await t`は次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="60db1-1647">Awaiter`a`式を評価することによって取得`(t).GetAwaiter()`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="60db1-1648">A `bool` `b`式を評価することによって取得`(a).IsCompleted`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="60db1-1649">場合`b`は`false`評価がかどうかに依存し、`a`インターフェイスを実装する`System.Runtime.CompilerServices.ICriticalNotifyCompletion`(と呼びます`ICriticalNotifyCompletion`簡潔にするため)。</span><span class="sxs-lookup"><span data-stu-id="60db1-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="60db1-1650">このチェックはバインドの時間で行われます実行時につまり場合`a`コンパイル時の型が`dynamic`、し、それ以外の場合コンパイル時にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="60db1-1651">ように`r`再開デリゲートを表します ([反復子](classes.md#iterators))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="60db1-1652">場合`a`実装しない`ICriticalNotifyCompletion`、式、`(a as (INotifyCompletion)).OnCompleted(r)`が評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="60db1-1653">場合`a`を実装して`ICriticalNotifyCompletion`、式、`(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)`が評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="60db1-1654">評価が中断し、および非同期関数の現在の呼び出し元に制御が返されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="60db1-1655">いずれかの直後に (場合`b`が`true`)、または再開デリゲートの起動の後に (場合`b`が`false`)、式`(a).GetResult()`が評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="60db1-1656">その値がの結果で値を返された場合、 *await_expression*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="60db1-1657">それ以外の場合、結果はありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="60db1-1658">インターフェイスのメソッドの実装を awaiter の`INotifyCompletion.OnCompleted`と`ICriticalNotifyCompletion.UnsafeOnCompleted`発生することは、デリゲート`r`多くても 1 回呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="60db1-1659">それ以外の場合、外側の非同期関数の動作は定義されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="60db1-1660">算術演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1660">Arithmetic operators</span></span>

<span data-ttu-id="60db1-1661">`*`、 `/`、 `%`、 `+`、および`-`演算子は、算術演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

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

<span data-ttu-id="60db1-1662">算術演算子のオペランドがコンパイル時の型を持つかどうか`dynamic`、式を動的にバインドし、([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="60db1-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="60db1-1663">この場合、式のコンパイル時の型は`dynamic`、コンパイル時の型を持つこれらのオペランドの実行時の型を使用して実行時に以下に示す解決が行わ`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="60db1-1664">乗算演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1664">Multiplication operator</span></span>

<span data-ttu-id="60db1-1665">フォームの操作の`x * y`、二項演算子のオーバー ロードの解決 ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-1666">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="60db1-1667">定義済みの乗算演算子は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="60db1-1668">すべての演算子の積が計算`x`と`y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="60db1-1669">整数の乗算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="60db1-1670">`checked`製品が結果の型の範囲外の場合は、コンテキスト、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="60db1-1671">`unchecked`コンテキスト、オーバーフローが報告されず、結果型の範囲外有意の上位ビットは破棄されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="60db1-1672">浮動小数点の乗算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="60db1-1673">製品は、IEEE 754 の演算の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="60db1-1674">次の表では、有限値が 0 以外の場合の可能なすべての組み合わせ、ゼロ、無限大および NaN の結果を示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="60db1-1675">表に、`x`と`y`は正の有限値。</span><span class="sxs-lookup"><span data-stu-id="60db1-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="60db1-1676">`z` 結果は、`x * y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="60db1-1677">変換先の型に対して、結果が大きすぎる場合`z`無限大です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="60db1-1678">変換先の型の結果が小さすぎる場合`z`は 0 です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="60db1-1679">+ y</span><span class="sxs-lookup"><span data-stu-id="60db1-1679">+y</span></span>   | <span data-ttu-id="60db1-1680">-y</span><span class="sxs-lookup"><span data-stu-id="60db1-1680">-y</span></span>   | <span data-ttu-id="60db1-1681">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1681">+0</span></span>  | <span data-ttu-id="60db1-1682">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1682">-0</span></span>  | <span data-ttu-id="60db1-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1683">+inf</span></span> | <span data-ttu-id="60db1-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1684">-inf</span></span> | <span data-ttu-id="60db1-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1685">NaN</span></span> | 
   | <span data-ttu-id="60db1-1686">+x</span><span class="sxs-lookup"><span data-stu-id="60db1-1686">+x</span></span>   | <span data-ttu-id="60db1-1687">+ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1687">+z</span></span>   | <span data-ttu-id="60db1-1688">~ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1688">-z</span></span>   | <span data-ttu-id="60db1-1689">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1689">+0</span></span>  | <span data-ttu-id="60db1-1690">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1690">-0</span></span>  | <span data-ttu-id="60db1-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1691">+inf</span></span> | <span data-ttu-id="60db1-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1692">-inf</span></span> | <span data-ttu-id="60db1-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1693">NaN</span></span> | 
   | <span data-ttu-id="60db1-1694">-x</span><span class="sxs-lookup"><span data-stu-id="60db1-1694">-x</span></span>   | <span data-ttu-id="60db1-1695">~ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1695">-z</span></span>   | <span data-ttu-id="60db1-1696">+ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1696">+z</span></span>   | <span data-ttu-id="60db1-1697">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1697">-0</span></span>  | <span data-ttu-id="60db1-1698">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1698">+0</span></span>  | <span data-ttu-id="60db1-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1699">-inf</span></span> | <span data-ttu-id="60db1-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1700">+inf</span></span> | <span data-ttu-id="60db1-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1701">NaN</span></span> | 
   | <span data-ttu-id="60db1-1702">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1702">+0</span></span>   | <span data-ttu-id="60db1-1703">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1703">+0</span></span>   | <span data-ttu-id="60db1-1704">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1704">-0</span></span>   | <span data-ttu-id="60db1-1705">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1705">+0</span></span>  | <span data-ttu-id="60db1-1706">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1706">-0</span></span>  | <span data-ttu-id="60db1-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1707">NaN</span></span>  | <span data-ttu-id="60db1-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1708">NaN</span></span>  | <span data-ttu-id="60db1-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1709">NaN</span></span> | 
   | <span data-ttu-id="60db1-1710">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1710">-0</span></span>   | <span data-ttu-id="60db1-1711">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1711">-0</span></span>   | <span data-ttu-id="60db1-1712">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1712">+0</span></span>   | <span data-ttu-id="60db1-1713">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1713">-0</span></span>  | <span data-ttu-id="60db1-1714">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1714">+0</span></span>  | <span data-ttu-id="60db1-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1715">NaN</span></span>  | <span data-ttu-id="60db1-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1716">NaN</span></span>  | <span data-ttu-id="60db1-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1717">NaN</span></span> | 
   | <span data-ttu-id="60db1-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1718">+inf</span></span> | <span data-ttu-id="60db1-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1719">+inf</span></span> | <span data-ttu-id="60db1-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1720">-inf</span></span> | <span data-ttu-id="60db1-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1721">NaN</span></span> | <span data-ttu-id="60db1-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1722">NaN</span></span> | <span data-ttu-id="60db1-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1723">+inf</span></span> | <span data-ttu-id="60db1-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1724">-inf</span></span> | <span data-ttu-id="60db1-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1725">NaN</span></span> | 
   | <span data-ttu-id="60db1-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1726">-inf</span></span> | <span data-ttu-id="60db1-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1727">-inf</span></span> | <span data-ttu-id="60db1-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1728">+inf</span></span> | <span data-ttu-id="60db1-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1729">NaN</span></span> | <span data-ttu-id="60db1-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1730">NaN</span></span> | <span data-ttu-id="60db1-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1731">-inf</span></span> | <span data-ttu-id="60db1-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1732">+inf</span></span> | <span data-ttu-id="60db1-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1733">NaN</span></span> | 
   | <span data-ttu-id="60db1-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1734">NaN</span></span>  | <span data-ttu-id="60db1-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1735">NaN</span></span>  | <span data-ttu-id="60db1-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1736">NaN</span></span>  | <span data-ttu-id="60db1-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1737">NaN</span></span> | <span data-ttu-id="60db1-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1738">NaN</span></span> | <span data-ttu-id="60db1-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1739">NaN</span></span>  | <span data-ttu-id="60db1-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1740">NaN</span></span>  | <span data-ttu-id="60db1-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1741">NaN</span></span> | 

*  <span data-ttu-id="60db1-1742">10 進数の乗算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="60db1-1743">結果の値が大きすぎてで表すかどうか、`decimal`形式、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="60db1-1744">結果値はで表現するには小さすぎるかどうか、`decimal`形式、結果は 0 です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="60db1-1745">丸めを行う前に、結果の小数点以下桁数は、2 つのオペランドのスケールの合計です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="60db1-1746">10 進数の乗算は型の乗算演算子を使用すると、`System.Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="60db1-1747">除算演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1747">Division operator</span></span>

<span data-ttu-id="60db1-1748">フォームの操作の`x / y`、二項演算子のオーバー ロードの解決 ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-1749">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="60db1-1750">定義済みの除算演算子は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="60db1-1751">すべての演算子の商を計算する`x`と`y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="60db1-1752">整数除算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="60db1-1753">右側のオペランドの値が 0 の場合、`System.DivideByZeroException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="60db1-1754">除算では、0 方向に結果を丸めます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="60db1-1755">したがって、結果の絶対値が 2 つのオペランドの商の絶対値と等しいまたはそれより小さい最大整数です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="60db1-1756">結果は、2 つのオペランドが同じ符号がある、または記号の反対側の 2 つのオペランドが負の場合は 0 または正の値。</span><span class="sxs-lookup"><span data-stu-id="60db1-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="60db1-1757">左側のオペランドが表現可能な最小の場合`int`または`long`値と右辺オペランドは`-1`オーバーフローが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="60db1-1758">`checked`コンテキスト、これにより、 `System.ArithmeticException` (またはそのサブクラス) がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="60db1-1759">`unchecked`実装で定義されたかどうかは、そのコンテキストを`System.ArithmeticException`(またはそのサブクラス) がスローされますまたは結果の中、左側のオペランドの値に、オーバーフローが報告されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="60db1-1760">浮動小数点除算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="60db1-1761">商は、IEEE 754 の演算の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="60db1-1762">次の表では、有限値が 0 以外の場合の可能なすべての組み合わせ、ゼロ、無限大および NaN の結果を示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="60db1-1763">表に、`x`と`y`は正の有限値。</span><span class="sxs-lookup"><span data-stu-id="60db1-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="60db1-1764">`z` 結果は、`x / y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="60db1-1765">変換先の型に対して、結果が大きすぎる場合`z`無限大です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="60db1-1766">変換先の型の結果が小さすぎる場合`z`は 0 です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="60db1-1767">+ y</span><span class="sxs-lookup"><span data-stu-id="60db1-1767">+y</span></span>   | <span data-ttu-id="60db1-1768">-y</span><span class="sxs-lookup"><span data-stu-id="60db1-1768">-y</span></span>   | <span data-ttu-id="60db1-1769">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1769">+0</span></span>   | <span data-ttu-id="60db1-1770">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1770">-0</span></span>   | <span data-ttu-id="60db1-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1771">+inf</span></span> | <span data-ttu-id="60db1-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1772">-inf</span></span> | <span data-ttu-id="60db1-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1773">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1774">+x</span><span class="sxs-lookup"><span data-stu-id="60db1-1774">+x</span></span>   | <span data-ttu-id="60db1-1775">+ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1775">+z</span></span>   | <span data-ttu-id="60db1-1776">~ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1776">-z</span></span>   | <span data-ttu-id="60db1-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1777">+inf</span></span> | <span data-ttu-id="60db1-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1778">-inf</span></span> | <span data-ttu-id="60db1-1779">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1779">+0</span></span>   | <span data-ttu-id="60db1-1780">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1780">-0</span></span>   | <span data-ttu-id="60db1-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1781">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1782">-x</span><span class="sxs-lookup"><span data-stu-id="60db1-1782">-x</span></span>   | <span data-ttu-id="60db1-1783">~ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1783">-z</span></span>   | <span data-ttu-id="60db1-1784">+ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1784">+z</span></span>   | <span data-ttu-id="60db1-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1785">-inf</span></span> | <span data-ttu-id="60db1-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1786">+inf</span></span> | <span data-ttu-id="60db1-1787">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1787">-0</span></span>   | <span data-ttu-id="60db1-1788">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1788">+0</span></span>   | <span data-ttu-id="60db1-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1789">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1790">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1790">+0</span></span>   | <span data-ttu-id="60db1-1791">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1791">+0</span></span>   | <span data-ttu-id="60db1-1792">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1792">-0</span></span>   | <span data-ttu-id="60db1-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1793">NaN</span></span>  | <span data-ttu-id="60db1-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1794">NaN</span></span>  | <span data-ttu-id="60db1-1795">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1795">+0</span></span>   | <span data-ttu-id="60db1-1796">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1796">-0</span></span>   | <span data-ttu-id="60db1-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1797">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1798">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1798">-0</span></span>   | <span data-ttu-id="60db1-1799">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1799">-0</span></span>   | <span data-ttu-id="60db1-1800">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1800">+0</span></span>   | <span data-ttu-id="60db1-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1801">NaN</span></span>  | <span data-ttu-id="60db1-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1802">NaN</span></span>  | <span data-ttu-id="60db1-1803">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1803">-0</span></span>   | <span data-ttu-id="60db1-1804">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1804">+0</span></span>   | <span data-ttu-id="60db1-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1805">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1806">+inf</span></span> | <span data-ttu-id="60db1-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1807">+inf</span></span> | <span data-ttu-id="60db1-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1808">-inf</span></span> | <span data-ttu-id="60db1-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1809">+inf</span></span> | <span data-ttu-id="60db1-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1810">-inf</span></span> | <span data-ttu-id="60db1-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1811">NaN</span></span>  | <span data-ttu-id="60db1-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1812">NaN</span></span>  | <span data-ttu-id="60db1-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1813">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1814">-inf</span></span> | <span data-ttu-id="60db1-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1815">-inf</span></span> | <span data-ttu-id="60db1-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1816">+inf</span></span> | <span data-ttu-id="60db1-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1817">-inf</span></span> | <span data-ttu-id="60db1-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1818">+inf</span></span> | <span data-ttu-id="60db1-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1819">NaN</span></span>  | <span data-ttu-id="60db1-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1820">NaN</span></span>  | <span data-ttu-id="60db1-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1821">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1822">NaN</span></span>  | <span data-ttu-id="60db1-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1823">NaN</span></span>  | <span data-ttu-id="60db1-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1824">NaN</span></span>  | <span data-ttu-id="60db1-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1825">NaN</span></span>  | <span data-ttu-id="60db1-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1826">NaN</span></span>  | <span data-ttu-id="60db1-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1827">NaN</span></span>  | <span data-ttu-id="60db1-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1828">NaN</span></span>  | <span data-ttu-id="60db1-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1829">NaN</span></span>  | 

*  <span data-ttu-id="60db1-1830">小数除算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="60db1-1831">右側のオペランドの値が 0 の場合、`System.DivideByZeroException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="60db1-1832">結果の値が大きすぎてで表すかどうか、`decimal`形式、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="60db1-1833">結果値はで表現するには小さすぎるかどうか、`decimal`形式、結果は 0 です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="60db1-1834">結果の小数点以下桁数と等しい、結果を保持する最小のスケール、実際の計算結果を 10 進値を表現できる最も近い。</span><span class="sxs-lookup"><span data-stu-id="60db1-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="60db1-1835">10 進数の除算は型の除算演算子を使用すると、`System.Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="60db1-1836">剰余演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1836">Remainder operator</span></span>

<span data-ttu-id="60db1-1837">フォームの操作の`x % y`、二項演算子のオーバー ロードの解決 ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-1838">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="60db1-1839">定義済みの剰余演算子は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="60db1-1840">すべての演算子の間での除算の剰余を計算`x`と`y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="60db1-1841">整数の剰余。</span><span class="sxs-lookup"><span data-stu-id="60db1-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="60db1-1842">結果`x % y`によって値が生成される`x - (x / y) * y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="60db1-1843">場合`y`0 の場合は、`System.DivideByZeroException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="60db1-1844">左側のオペランドが最も場合`int`または`long`値と右辺オペランドは`-1`、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="60db1-1845">ない場合は`x % y`例外をスロー場所`x / y`は例外をスローできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="60db1-1846">浮動小数点の剰余。</span><span class="sxs-lookup"><span data-stu-id="60db1-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="60db1-1847">次の表では、有限値が 0 以外の場合の可能なすべての組み合わせ、ゼロ、無限大および NaN の結果を示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="60db1-1848">表に、`x`と`y`は正の有限値。</span><span class="sxs-lookup"><span data-stu-id="60db1-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="60db1-1849">`z` 結果は、`x % y`を計算して`x - n * y`ここで、`n`に等しいまたはそれより小さい最大整数を`x / y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="60db1-1850">残りの部分をコンピューティングするのには、このメソッドは整数オペランドで使用される類似していますが、IEEE 754 の定義とは異なります (これで`n`に最も近い整数は、 `x / y`)。</span><span class="sxs-lookup"><span data-stu-id="60db1-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="60db1-1851">+ y</span><span class="sxs-lookup"><span data-stu-id="60db1-1851">+y</span></span>   | <span data-ttu-id="60db1-1852">-y</span><span class="sxs-lookup"><span data-stu-id="60db1-1852">-y</span></span>   | <span data-ttu-id="60db1-1853">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1853">+0</span></span>   | <span data-ttu-id="60db1-1854">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1854">-0</span></span>   | <span data-ttu-id="60db1-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1855">+inf</span></span> | <span data-ttu-id="60db1-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1856">-inf</span></span> | <span data-ttu-id="60db1-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1857">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1858">+x</span><span class="sxs-lookup"><span data-stu-id="60db1-1858">+x</span></span>   | <span data-ttu-id="60db1-1859">+ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1859">+z</span></span>   | <span data-ttu-id="60db1-1860">+ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1860">+z</span></span>   | <span data-ttu-id="60db1-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1861">NaN</span></span>  | <span data-ttu-id="60db1-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1862">NaN</span></span>  | <span data-ttu-id="60db1-1863">x</span><span class="sxs-lookup"><span data-stu-id="60db1-1863">x</span></span>    | <span data-ttu-id="60db1-1864">x</span><span class="sxs-lookup"><span data-stu-id="60db1-1864">x</span></span>    | <span data-ttu-id="60db1-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1865">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1866">-x</span><span class="sxs-lookup"><span data-stu-id="60db1-1866">-x</span></span>   | <span data-ttu-id="60db1-1867">~ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1867">-z</span></span>   | <span data-ttu-id="60db1-1868">~ z</span><span class="sxs-lookup"><span data-stu-id="60db1-1868">-z</span></span>   | <span data-ttu-id="60db1-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1869">NaN</span></span>  | <span data-ttu-id="60db1-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1870">NaN</span></span>  | <span data-ttu-id="60db1-1871">-x</span><span class="sxs-lookup"><span data-stu-id="60db1-1871">-x</span></span>   | <span data-ttu-id="60db1-1872">-x</span><span class="sxs-lookup"><span data-stu-id="60db1-1872">-x</span></span>   | <span data-ttu-id="60db1-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1873">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1874">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1874">+0</span></span>   | <span data-ttu-id="60db1-1875">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1875">+0</span></span>   | <span data-ttu-id="60db1-1876">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1876">+0</span></span>   | <span data-ttu-id="60db1-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1877">NaN</span></span>  | <span data-ttu-id="60db1-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1878">NaN</span></span>  | <span data-ttu-id="60db1-1879">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1879">+0</span></span>   | <span data-ttu-id="60db1-1880">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1880">+0</span></span>   | <span data-ttu-id="60db1-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1881">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1882">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1882">-0</span></span>   | <span data-ttu-id="60db1-1883">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1883">-0</span></span>   | <span data-ttu-id="60db1-1884">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1884">-0</span></span>   | <span data-ttu-id="60db1-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1885">NaN</span></span>  | <span data-ttu-id="60db1-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1886">NaN</span></span>  | <span data-ttu-id="60db1-1887">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1887">-0</span></span>   | <span data-ttu-id="60db1-1888">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1888">-0</span></span>   | <span data-ttu-id="60db1-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1889">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1890">+inf</span></span> | <span data-ttu-id="60db1-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1891">NaN</span></span>  | <span data-ttu-id="60db1-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1892">NaN</span></span>  | <span data-ttu-id="60db1-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1893">NaN</span></span>  | <span data-ttu-id="60db1-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1894">NaN</span></span>  | <span data-ttu-id="60db1-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1895">NaN</span></span>  | <span data-ttu-id="60db1-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1896">NaN</span></span>  | <span data-ttu-id="60db1-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1897">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1898">-inf</span></span> | <span data-ttu-id="60db1-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1899">NaN</span></span>  | <span data-ttu-id="60db1-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1900">NaN</span></span>  | <span data-ttu-id="60db1-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1901">NaN</span></span>  | <span data-ttu-id="60db1-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1902">NaN</span></span>  | <span data-ttu-id="60db1-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1903">NaN</span></span>  | <span data-ttu-id="60db1-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1904">NaN</span></span>  | <span data-ttu-id="60db1-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1905">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1906">NaN</span></span>  | <span data-ttu-id="60db1-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1907">NaN</span></span>  | <span data-ttu-id="60db1-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1908">NaN</span></span>  | <span data-ttu-id="60db1-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1909">NaN</span></span>  | <span data-ttu-id="60db1-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1910">NaN</span></span>  | <span data-ttu-id="60db1-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1911">NaN</span></span>  | <span data-ttu-id="60db1-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1912">NaN</span></span>  | <span data-ttu-id="60db1-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1913">NaN</span></span>  | 

*  <span data-ttu-id="60db1-1914">10 進数の剰余。</span><span class="sxs-lookup"><span data-stu-id="60db1-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="60db1-1915">右側のオペランドの値が 0 の場合、`System.DivideByZeroException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="60db1-1916">丸めを行う前に、結果の小数点以下桁数は 2 つのオペランドの規模の大きいと、結果の符号は、0 以外の場合はいるのと同じ`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="60db1-1917">10 進数の残りの部分は型の剰余演算子を使用すると`System.Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="60db1-1918">加算演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-1918">Addition operator</span></span>

<span data-ttu-id="60db1-1919">フォームの操作の`x + y`、二項演算子のオーバー ロードの解決 ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-1920">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="60db1-1921">定義済みの加算演算子は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="60db1-1922">数値型と列挙型の場合は、定義済みの加算演算子は、2 つのオペランドの合計を計算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="60db1-1923">1 つまたは両方のオペランドが文字列型、定義済みの加算演算子はオペランドの文字列表現を連結します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="60db1-1924">整数の加算:</span><span class="sxs-lookup"><span data-stu-id="60db1-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="60db1-1925">`checked`コンテキスト、合計が、結果の型の範囲外の場合、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="60db1-1926">`unchecked`コンテキスト、オーバーフローが報告されず、結果型の範囲外有意の上位ビットは破棄されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="60db1-1927">浮動小数点加算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="60db1-1928">合計は、IEEE 754 の演算の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="60db1-1929">次の表では、有限値が 0 以外の場合の可能なすべての組み合わせ、ゼロ、無限大および NaN の結果を示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="60db1-1930">表に、`x`と`y`は 0 以外の値の有限値と`z`の結果は、`x + y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="60db1-1931">場合`x`と`y`絶対値が同じ記号、方向が逆`z`は正の 0 になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="60db1-1932">場合`x + y`が大きすぎて、変換先の型で表す`z`、無限大と同じ符号で`x + y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="60db1-1933">Y</span><span class="sxs-lookup"><span data-stu-id="60db1-1933">y</span></span>    | <span data-ttu-id="60db1-1934">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1934">+0</span></span>   | <span data-ttu-id="60db1-1935">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1935">-0</span></span>   | <span data-ttu-id="60db1-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1936">+inf</span></span> | <span data-ttu-id="60db1-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1937">-inf</span></span> | <span data-ttu-id="60db1-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1938">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1939">x</span><span class="sxs-lookup"><span data-stu-id="60db1-1939">x</span></span>    | <span data-ttu-id="60db1-1940">z</span><span class="sxs-lookup"><span data-stu-id="60db1-1940">z</span></span>    | <span data-ttu-id="60db1-1941">x</span><span class="sxs-lookup"><span data-stu-id="60db1-1941">x</span></span>    | <span data-ttu-id="60db1-1942">x</span><span class="sxs-lookup"><span data-stu-id="60db1-1942">x</span></span>    | <span data-ttu-id="60db1-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1943">+inf</span></span> | <span data-ttu-id="60db1-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1944">-inf</span></span> | <span data-ttu-id="60db1-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1945">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1946">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1946">+0</span></span>   | <span data-ttu-id="60db1-1947">Y</span><span class="sxs-lookup"><span data-stu-id="60db1-1947">y</span></span>    | <span data-ttu-id="60db1-1948">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1948">+0</span></span>   | <span data-ttu-id="60db1-1949">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1949">+0</span></span>   | <span data-ttu-id="60db1-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1950">+inf</span></span> | <span data-ttu-id="60db1-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1951">-inf</span></span> | <span data-ttu-id="60db1-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1952">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1953">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1953">-0</span></span>   | <span data-ttu-id="60db1-1954">Y</span><span class="sxs-lookup"><span data-stu-id="60db1-1954">y</span></span>    | <span data-ttu-id="60db1-1955">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-1955">+0</span></span>   | <span data-ttu-id="60db1-1956">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-1956">-0</span></span>   | <span data-ttu-id="60db1-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1957">+inf</span></span> | <span data-ttu-id="60db1-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1958">-inf</span></span> | <span data-ttu-id="60db1-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1959">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1960">+inf</span></span> | <span data-ttu-id="60db1-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1961">+inf</span></span> | <span data-ttu-id="60db1-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1962">+inf</span></span> | <span data-ttu-id="60db1-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1963">+inf</span></span> | <span data-ttu-id="60db1-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1964">+inf</span></span> | <span data-ttu-id="60db1-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1965">NaN</span></span>  | <span data-ttu-id="60db1-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1966">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1967">-inf</span></span> | <span data-ttu-id="60db1-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1968">-inf</span></span> | <span data-ttu-id="60db1-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1969">-inf</span></span> | <span data-ttu-id="60db1-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1970">-inf</span></span> | <span data-ttu-id="60db1-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1971">NaN</span></span>  | <span data-ttu-id="60db1-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-1972">-inf</span></span> | <span data-ttu-id="60db1-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1973">NaN</span></span>  | 
   | <span data-ttu-id="60db1-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1974">NaN</span></span>  | <span data-ttu-id="60db1-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1975">NaN</span></span>  | <span data-ttu-id="60db1-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1976">NaN</span></span>  | <span data-ttu-id="60db1-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1977">NaN</span></span>  | <span data-ttu-id="60db1-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1978">NaN</span></span>  | <span data-ttu-id="60db1-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1979">NaN</span></span>  | <span data-ttu-id="60db1-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-1980">NaN</span></span>  | 

*  <span data-ttu-id="60db1-1981">10 進数の追加:</span><span class="sxs-lookup"><span data-stu-id="60db1-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="60db1-1982">結果の値が大きすぎてで表すかどうか、`decimal`形式、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="60db1-1983">丸めを行う前に、結果の小数点以下桁数は 2 つのオペランドのスケールのうち、大きい方です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="60db1-1984">10 進数の加算は型の加算演算子を使用すると、`System.Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="60db1-1985">列挙型の加算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1985">Enumeration addition.</span></span> <span data-ttu-id="60db1-1986">すべての列挙型を暗黙的に、次の定義済みの演算子、提供、`E`列挙型と`U`の基になる型は、 `E`:</span><span class="sxs-lookup"><span data-stu-id="60db1-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="60db1-1987">実行時にこれらの演算子が正確に評価されます`(E)((U)x + (U)y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="60db1-1988">文字列の連結。</span><span class="sxs-lookup"><span data-stu-id="60db1-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="60db1-1989">これらのオーバー ロードのバイナリの`+`演算子が文字列の連結を実行します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="60db1-1990">文字列の連結のオペランドは場合`null`、空の文字列に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="60db1-1991">仮想を呼び出すことによって、任意の文字列以外の引数を文字列形式に変換がそれ以外の場合、`ToString`型から継承されたメソッド`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="60db1-1992">場合`ToString`返します`null`、空の文字列に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="60db1-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

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

   文字列連結演算子の結果は、左オペランドと右辺オペランドの文字が続くの文字で構成される文字列です。 文字列連結演算子が返すことはありません、`null`値。 <span data-ttu-id="60db1-1995">A`System.OutOfMemoryException`結果の文字列を割り当てることができる十分なメモリがない場合にスローされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="60db1-1996">デリゲートの組み合わせ。</span><span class="sxs-lookup"><span data-stu-id="60db1-1996">Delegate combination.</span></span> <span data-ttu-id="60db1-1997">すべてのデリゲート型が暗黙的に次の定義済みの演算子を提供します、`D`はデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="60db1-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="60db1-1998">バイナリ`+`演算子は、両方のオペランドがいくつかのデリゲート型の場合、デリゲートの組み合わせを実行`D`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="60db1-1999">(この場合、オペランドに異なるデリゲート型では、バインド時のエラーが発生します。)最初のオペランドが場合`null`、操作の結果は、2 番目のオペランドの値 (である場合でも`null`)。</span><span class="sxs-lookup"><span data-stu-id="60db1-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="60db1-2000">それ以外の場合、2 番目のオペランドが場合`null`操作の結果は最初のオペランドの値。</span><span class="sxs-lookup"><span data-stu-id="60db1-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="60db1-2001">それ以外の場合、操作の結果は、新しいデリゲート インスタンスが、呼び出される、最初のオペランドを呼び出すし、2 番目のオペランドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="60db1-2002">デリゲートの組み合わせの例については、次を参照してください。[減算演算子](expressions.md#subtraction-operator)と[デリゲート呼び出し](delegates.md#delegate-invocation)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="60db1-2003">`System.Delegate` 、デリゲート型ではない`operator` `+`用に定義されていません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="60db1-2004">減算演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2004">Subtraction operator</span></span>

<span data-ttu-id="60db1-2005">フォームの操作の`x - y`、二項演算子のオーバー ロードの解決 ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-2006">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="60db1-2007">定義済みの減算演算子は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="60db1-2008">すべての減算演算子`y`から`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="60db1-2009">整数の減算:</span><span class="sxs-lookup"><span data-stu-id="60db1-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="60db1-2010">`checked`コンテキスト、違いが結果の型の範囲外の場合、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="60db1-2011">`unchecked`コンテキスト、オーバーフローが報告されず、結果型の範囲外有意の上位ビットは破棄されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="60db1-2012">浮動小数点の減算:</span><span class="sxs-lookup"><span data-stu-id="60db1-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="60db1-2013">違いは、IEEE 754 の演算の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="60db1-2014">次の表では、0 以外の値の有限値の可能なすべての組み合わせ、ゼロ、無限大および Nan の結果を示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="60db1-2015">表に、`x`と`y`は 0 以外の値の有限値と`z`の結果は、`x - y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="60db1-2016">場合`x`と`y`が等しいか、`z`は正の 0 になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="60db1-2017">場合`x - y`が大きすぎて、変換先の型で表す`z`、無限大と同じ符号で`x - y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   | <span data-ttu-id="60db1-2018">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2018">NaN</span></span>  | <span data-ttu-id="60db1-2019">Y</span><span class="sxs-lookup"><span data-stu-id="60db1-2019">y</span></span>    | <span data-ttu-id="60db1-2020">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-2020">+0</span></span>   | <span data-ttu-id="60db1-2021">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-2021">-0</span></span>   | <span data-ttu-id="60db1-2022">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2022">+inf</span></span> | <span data-ttu-id="60db1-2023">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2023">-inf</span></span> | <span data-ttu-id="60db1-2024">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2024">NaN</span></span> | 
   | <span data-ttu-id="60db1-2025">x</span><span class="sxs-lookup"><span data-stu-id="60db1-2025">x</span></span>    | <span data-ttu-id="60db1-2026">z</span><span class="sxs-lookup"><span data-stu-id="60db1-2026">z</span></span>    | <span data-ttu-id="60db1-2027">x</span><span class="sxs-lookup"><span data-stu-id="60db1-2027">x</span></span>    | <span data-ttu-id="60db1-2028">x</span><span class="sxs-lookup"><span data-stu-id="60db1-2028">x</span></span>    | <span data-ttu-id="60db1-2029">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2029">-inf</span></span> | <span data-ttu-id="60db1-2030">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2030">+inf</span></span> | <span data-ttu-id="60db1-2031">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2031">NaN</span></span> | 
   | <span data-ttu-id="60db1-2032">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-2032">+0</span></span>   | <span data-ttu-id="60db1-2033">-y</span><span class="sxs-lookup"><span data-stu-id="60db1-2033">-y</span></span>   | <span data-ttu-id="60db1-2034">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-2034">+0</span></span>   | <span data-ttu-id="60db1-2035">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-2035">+0</span></span>   | <span data-ttu-id="60db1-2036">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2036">-inf</span></span> | <span data-ttu-id="60db1-2037">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2037">+inf</span></span> | <span data-ttu-id="60db1-2038">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2038">NaN</span></span> | 
   | <span data-ttu-id="60db1-2039">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-2039">-0</span></span>   | <span data-ttu-id="60db1-2040">-y</span><span class="sxs-lookup"><span data-stu-id="60db1-2040">-y</span></span>   | <span data-ttu-id="60db1-2041">-0</span><span class="sxs-lookup"><span data-stu-id="60db1-2041">-0</span></span>   | <span data-ttu-id="60db1-2042">+0</span><span class="sxs-lookup"><span data-stu-id="60db1-2042">+0</span></span>   | <span data-ttu-id="60db1-2043">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2043">-inf</span></span> | <span data-ttu-id="60db1-2044">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2044">+inf</span></span> | <span data-ttu-id="60db1-2045">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2045">NaN</span></span> | 
   | <span data-ttu-id="60db1-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2046">+inf</span></span> | <span data-ttu-id="60db1-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2047">+inf</span></span> | <span data-ttu-id="60db1-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2048">+inf</span></span> | <span data-ttu-id="60db1-2049">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2049">+inf</span></span> | <span data-ttu-id="60db1-2050">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2050">NaN</span></span>  | <span data-ttu-id="60db1-2051">+inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2051">+inf</span></span> | <span data-ttu-id="60db1-2052">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2052">NaN</span></span> | 
   | <span data-ttu-id="60db1-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2053">-inf</span></span> | <span data-ttu-id="60db1-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2054">-inf</span></span> | <span data-ttu-id="60db1-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2055">-inf</span></span> | <span data-ttu-id="60db1-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2056">-inf</span></span> | <span data-ttu-id="60db1-2057">-inf</span><span class="sxs-lookup"><span data-stu-id="60db1-2057">-inf</span></span> | <span data-ttu-id="60db1-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2058">NaN</span></span>  | <span data-ttu-id="60db1-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2059">NaN</span></span> | 
   | <span data-ttu-id="60db1-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2060">NaN</span></span>  | <span data-ttu-id="60db1-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2061">NaN</span></span>  | <span data-ttu-id="60db1-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2062">NaN</span></span>  | <span data-ttu-id="60db1-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2063">NaN</span></span>  | <span data-ttu-id="60db1-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2064">NaN</span></span>  | <span data-ttu-id="60db1-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2065">NaN</span></span>  | <span data-ttu-id="60db1-2066">NaN</span><span class="sxs-lookup"><span data-stu-id="60db1-2066">NaN</span></span> | 

*  <span data-ttu-id="60db1-2067">10 進数の減算:</span><span class="sxs-lookup"><span data-stu-id="60db1-2067">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="60db1-2068">結果の値が大きすぎてで表すかどうか、`decimal`形式、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2068">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="60db1-2069">丸めを行う前に、結果の小数点以下桁数は 2 つのオペランドのスケールのうち、大きい方です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2069">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="60db1-2070">10 進数の減算は、型の減算演算子を使用すると`System.Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2070">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="60db1-2071">列挙型の減算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2071">Enumeration subtraction.</span></span> <span data-ttu-id="60db1-2072">すべての列挙型が暗黙的に次の定義済みの演算子を提供、`E`列挙型と`U`の基になる型は、 `E`:</span><span class="sxs-lookup"><span data-stu-id="60db1-2072">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="60db1-2073">この演算子の評価とまったく同じ`(U)((U)x - (U)y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2073">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="60db1-2074">つまりの序数値の差の計算演算子`x`と`y`、列挙体の基になる型であり、結果の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-2074">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="60db1-2075">この演算子の評価とまったく同じ`(E)((U)x - y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2075">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="60db1-2076">つまり、演算子は、減算、列挙体の基になる型の値列挙体の値として生成します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2076">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="60db1-2077">デリゲートの削除。</span><span class="sxs-lookup"><span data-stu-id="60db1-2077">Delegate removal.</span></span> <span data-ttu-id="60db1-2078">すべてのデリゲート型が暗黙的に次の定義済みの演算子を提供します、`D`はデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2078">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="60db1-2079">バイナリ`-`演算子は、両方のオペランドがいくつかのデリゲート型の場合、デリゲートの削除を実行`D`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2079">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="60db1-2080">オペランドに異なるデリゲート型がある場合、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2080">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="60db1-2081">最初のオペランドが場合`null`、操作の結果は`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2081">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="60db1-2082">それ以外の場合、2 番目のオペランドが場合`null`操作の結果は最初のオペランドの値。</span><span class="sxs-lookup"><span data-stu-id="60db1-2082">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="60db1-2083">それ以外の場合、両方のオペランドが呼び出しリストを表す ([デリゲートの宣言](delegates.md#delegate-declarations)) 1 つまたは複数のエントリと、結果はから削除された、2 番目のオペランドのエントリで、最初のオペランドの一覧から成る新しい呼び出しリスト最初の連続するサブリストは、2 番目のオペランドの一覧を提供です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2083">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="60db1-2084">(デリゲートの等値演算子とサブリスト、対応するエントリが比較されます ([デリゲート等値演算子](expressions.md#delegate-equality-operators)).)それ以外の場合、結果は、左側のオペランドの値です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2084">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="60db1-2085">プロセスのどちらのオペランドのリストが変更されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2085">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="60db1-2086">2 番目のオペランドの一覧には、最初のオペランドのリスト内の連続したエントリの複数のサブリストが一致すると、連続するエントリの右端の一致するサブリストは削除されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2086">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="60db1-2087">削除の結果、空のリストになる場合、結果は`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2087">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="60db1-2088">例:</span><span class="sxs-lookup"><span data-stu-id="60db1-2088">For example:</span></span>

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

## <a name="shift-operators"></a><span data-ttu-id="60db1-2089">シフト演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2089">Shift operators</span></span>

<span data-ttu-id="60db1-2090">`<<`と`>>`演算子を使用して、ビット シフト演算を実行します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2090">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="60db1-2091">オペランドの場合、 *shift_expression*コンパイル時の型を持つ`dynamic`、式を動的にバインドし、([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2091">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="60db1-2092">この場合、式のコンパイル時の型は`dynamic`、コンパイル時の型を持つこれらのオペランドの実行時の型を使用して実行時に以下に示す解決が行わ`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2092">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="60db1-2093">フォームの操作の`x << count`または`x >> count`、二項演算子のオーバー ロードの解決 ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2093">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-2094">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-2094">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="60db1-2095">最初のオペランドの型では、クラスまたは構造体の演算子の宣言を含むをある必要が常にオーバー ロードされた shift 演算子を宣言するときにあり、2 番目のオペランドの型は常にあります`int`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2095">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="60db1-2096">定義済みのシフト演算子は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2096">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="60db1-2097">左にシフトします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2097">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="60db1-2098">`<<`演算子シフト`x`ビット数だけ左が以下に示すように計算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2098">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="60db1-2099">結果型の範囲外の上位ビット`x`が破棄されるので、残りのビットが左にシフトされ、下位の空のビット位置が 0 に設定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2099">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="60db1-2100">右シフトします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2100">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="60db1-2101">`>>`演算子シフト`x`ビット数だけ右が以下に示すように計算します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2101">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="60db1-2102">ときに`x`の種類は`int`または`long`の下位ビット`x`は破棄されるので、残りのビットが右にシフトし、空の上位ビット位置は場合は 0 に設定されます`x`負でないと設定は、1 つの場合に`x`が負の値。</span><span class="sxs-lookup"><span data-stu-id="60db1-2102">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="60db1-2103">ときに`x`の種類は`uint`または`ulong`の下位ビット`x`が破棄されるので、残りのビットは右にシフトして、空の上位ビット位置が 0 に設定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2103">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="60db1-2104">定義済みの演算子のとおりにシフトするビット数が計算されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2104">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="60db1-2105">場合の種類`x`は`int`または`uint`の下位 5 ビットで、シフト数を指定`count`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2105">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="60db1-2106">つまり、シフト数がから計算された`count & 0x1F`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2106">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="60db1-2107">場合の種類`x`は`long`または`ulong`の下位 6 ビットで、シフト数を指定`count`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2107">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="60db1-2108">つまり、シフト数がから計算された`count & 0x3F`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2108">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="60db1-2109">シフト演算子は単の値を返す結果として得られる、シフト数が 0 の場合は、`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2109">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="60db1-2110">オーバーフローが発生してで同じ結果を生成しないのシフト演算を`checked`と`unchecked`コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="60db1-2110">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="60db1-2111">ときの左オペランド、`>>`演算子が符号付き整数型は、算術右シフトのオペランドの最上位ビット (符号ビット) の値を反映する、空の上位ビット位置を実行します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2111">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="60db1-2112">ときの左オペランド、`>>`演算子が符号なし整数型は、演算子、論理右シフトが行われ、空の上位ビット位置には常に 0 が設定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2112">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="60db1-2113">オペランドの型から推論の反対側の操作を実行するには、明示的なキャストを使用していることができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2113">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="60db1-2114">たとえば場合、`x`型の変数は、 `int`、操作`unchecked((int)((uint)x >> y))`論理の右シフトを実行します。`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2114">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="60db1-2115">関係演算子と型検査演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2115">Relational and type-testing operators</span></span>

<span data-ttu-id="60db1-2116">`==`、 `!=`、 `<`、 `>`、 `<=`、 `>=`、`is`と`as`演算子は、リレーショナルと型検査演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2116">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

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

<span data-ttu-id="60db1-2117">`is`演算子については、「 [、演算子は、](expressions.md#the-is-operator)と`as`演算子については、「[演算子として](expressions.md#the-as-operator)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2117">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="60db1-2118">`==`、 `!=`、 `<`、 `>`、`<=`と`>=`演算子は***比較演算子***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2118">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="60db1-2119">比較演算子のオペランドがコンパイル時の型を持つかどうか`dynamic`、式を動的にバインドし、([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2119">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="60db1-2120">この場合、式のコンパイル時の型は`dynamic`、コンパイル時の型を持つこれらのオペランドの実行時の型を使用して実行時に以下に示す解決が行わ`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2120">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="60db1-2121">フォームの操作の`x` *op* `y`ここで、 *op*比較演算子をオーバー ロードの解決 ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2121">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-2122">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-2122">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="60db1-2123">定義済みの比較演算子は、次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2123">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="60db1-2124">すべての定義済みの比較演算子は、型の結果を返す`bool`次の表で説明するようにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2124">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="60db1-2125">__操作__</span><span class="sxs-lookup"><span data-stu-id="60db1-2125">__Operation__</span></span> | <span data-ttu-id="60db1-2126">__結果__</span><span class="sxs-lookup"><span data-stu-id="60db1-2126">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="60db1-2127">`true` 場合`x`と等しい`y`、`false`それ以外の場合</span><span class="sxs-lookup"><span data-stu-id="60db1-2127">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="60db1-2128">`true` 場合`x`が等しくない`y`、`false`それ以外の場合</span><span class="sxs-lookup"><span data-stu-id="60db1-2128">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="60db1-2129">`true` 場合`x`がより小さい`y`、`false`それ以外の場合</span><span class="sxs-lookup"><span data-stu-id="60db1-2129">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="60db1-2130">`true` 場合`x`がより大きい`y`、`false`それ以外の場合</span><span class="sxs-lookup"><span data-stu-id="60db1-2130">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="60db1-2131">`true` 場合`x`に等しいまたはそれよりも小さい`y`、`false`それ以外の場合</span><span class="sxs-lookup"><span data-stu-id="60db1-2131">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="60db1-2132">`true` 場合`x`がより大きいまたは等しい`y`、`false`それ以外の場合</span><span class="sxs-lookup"><span data-stu-id="60db1-2132">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="60db1-2133">整数の比較演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2133">Integer comparison operators</span></span>

<span data-ttu-id="60db1-2134">定義済みの整数の比較演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2134">The predefined integer comparison operators are:</span></span>
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

<span data-ttu-id="60db1-2135">返します、2 つの整数オペランドの数値を比較これらの各演算子を`bool`、特定の関係は、かどうかを示す値`true`または`false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2135">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="60db1-2136">浮動小数点の比較演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2136">Floating-point comparison operators</span></span>

<span data-ttu-id="60db1-2137">定義済みの浮動小数点数の比較演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2137">The predefined floating-point comparison operators are:</span></span>
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

<span data-ttu-id="60db1-2138">演算子は、IEEE 754 標準の規則に従ってオペランドを比較します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2138">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="60db1-2139">結果は、いずれかのオペランドが NaN の場合は、`false`を除くすべての演算子の`!=`、結果が対象の`true`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2139">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="60db1-2140">任意の 2 つのオペランドの`x != y`と同じ結果を常に生成`!(x == y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2140">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="60db1-2141">ただし、1 つまたは両方のオペランドが NaN の場合が場合、 `<`、 `>`、 `<=`、および`>=`演算子は、反対側の演算子の論理否定演算と同じ結果を生成しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2141">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="60db1-2142">たとえば、いずれかの`x`と`y`が NaN の場合、`x < y`は`false`が`!(x >= y)`は`true`。</span><span class="sxs-lookup"><span data-stu-id="60db1-2142">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="60db1-2143">演算子が順序に関して 2 つの浮動小数点のオペランドの値を比較してどちらのオペランドが NaN の場合は、</span><span class="sxs-lookup"><span data-stu-id="60db1-2143">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="60db1-2144">場所`min`と`max`は、最小および最大の正の有限値を指定した浮動小数点形式で表すことができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2144">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="60db1-2145">この順序付けの注目すべき効果は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2145">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="60db1-2146">負と正のゼロは等しいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2146">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="60db1-2147">負の無限大は小さい他のすべての値よりも、もう 1 つの負の無限大とは等しいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2147">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="60db1-2148">正の無限大は、その他のすべての値より大きい値がもう 1 つの正の無限大に等しいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2148">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="60db1-2149">10 進数の比較演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2149">Decimal comparison operators</span></span>

<span data-ttu-id="60db1-2150">定義済みの 10 進数の比較演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2150">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="60db1-2151">2 つの 10 進数のオペランドと返しますの数値を比較これらの各演算子を`bool`、特定の関係は、かどうかを示す値`true`または`false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2151">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="60db1-2152">各 10 進数の比較では、対応するリレーショナルまたは型の等値演算子を使用すると`System.Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2152">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="60db1-2153">ブール等価演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2153">Boolean equality operators</span></span>

<span data-ttu-id="60db1-2154">定義済みのブール値の等値演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2154">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="60db1-2155">結果`==`は`true`両方`x`と`y`は`true`両方または`x`と`y`は`false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2155">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="60db1-2156">それ以外の場合、結果は `false` です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2156">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="60db1-2157">結果`!=`は`false`両方`x`と`y`は`true`両方または`x`と`y`は`false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2157">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="60db1-2158">それ以外の場合、結果は `true` です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2158">Otherwise, the result is `true`.</span></span> <span data-ttu-id="60db1-2159">型のオペランドが場合`bool`、`!=`演算子と同じ結果を生成する、`^`演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-2159">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="60db1-2160">列挙体の比較演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2160">Enumeration comparison operators</span></span>

<span data-ttu-id="60db1-2161">すべての列挙型には、次の定義済みの比較演算子が暗黙的に用意されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2161">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="60db1-2162">評価結果`x op y`ここで、`x`と`y`列挙型の式は`E`基になる型`U`、および`op`比較演算子の 1 つと同じでは正確に評価する`((U)x) op ((U)y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2162">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="60db1-2163">つまり、列挙型の比較演算子は、単に 2 つのオペランドの基になる整数値を比較します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2163">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="60db1-2164">参照型の等値演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2164">Reference type equality operators</span></span>

<span data-ttu-id="60db1-2165">定義済みの参照型の等値演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2165">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="60db1-2166">演算子は、2 つの参照が等値演算子または非等値比較の結果を返します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2166">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="60db1-2167">定義済みの参照型の等値演算子は、型のオペランドを受け入れるため`object`、適用可能な宣言しないすべての種類に適用される`operator ==`と`operator !=`メンバー。</span><span class="sxs-lookup"><span data-stu-id="60db1-2167">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="60db1-2168">逆に、任意の該当するユーザー定義の等値演算子には、定義済みの参照型の等値演算子効果的に非表示にします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2168">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="60db1-2169">定義済みの参照型の等値演算子では、次のいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2169">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="60db1-2170">両方のオペランドがある既知の型の値を*reference_type*またはリテラル`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2170">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="60db1-2171">さらに、明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) のいずれかのオペランドの型からもう一方のオペランドの型に存在します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2171">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="60db1-2172">1 つのオペランド型の値は、`T`場所`T`は、 *type_parameter*もう一方のオペランドは、リテラルと`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2172">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="60db1-2173">さらに`T`値型の制約はありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2173">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="60db1-2174">これらの条件のいずれかに該当する場合を除き、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2174">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="60db1-2175">これらのルールの主な特性は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2175">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="60db1-2176">別のバインド時に認識されている 2 つの参照を比較する定義済みの参照型の等値演算子を使用すると、バインド時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2176">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="60db1-2177">たとえば、オペランドのバインド時の型は、2 つのクラス型`A`と`B`、どちらの場合と`A`も`B`の 2 つのオペランドが同じオブジェクトを参照することはできませんし、もう一方の派生です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2177">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="60db1-2178">そのため、操作は、バインド時のエラーと見なされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2178">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="60db1-2179">定義済みの参照型の等値演算子は値と比較する型のオペランドを許可しない操作を行います。</span><span class="sxs-lookup"><span data-stu-id="60db1-2179">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="60db1-2180">そのため、構造体型で独自の等値演算子を宣言しない限り、その構造体の型の値を比較することはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2180">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="60db1-2181">定義済みの参照型の等値演算子には、オペランドにボックス化操作が発生しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2181">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="60db1-2182">新しく割り当てられたボックス化されたインスタンスへの参照は、必然的に他のすべての参照と異なっているために、このようなボックス化操作を実行しても意味があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2182">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="60db1-2183">場合、型パラメーターの型のオペランド`T`と比較する`null`の実行時の型と`T`値型である比較の結果は`false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2183">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="60db1-2184">次の例は、制約のない型パラメーターの型の引数がかどうかを確認します。`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2184">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="60db1-2185">`x == null`コンストラクトは許可されている場合でも`T`値の型を表すことができ、結果は、単に定義`false`とき`T`は値型です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2185">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="60db1-2186">フォームの操作の`x == y`または`x != y`任意の該当する場合は、`operator ==`または`operator !=`存在する場合、演算子のオーバー ロードの解決 ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution)) ルールを選択します。定義済みの参照型の等値演算子ではなく演算子です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2186">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="60db1-2187">ただし、いずれかまたは両方のオペランドを型を明示的にキャストすることによって、定義済みの参照型の等値演算子を選択することは常に`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2187">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="60db1-2188">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2188">The example</span></span>
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
<span data-ttu-id="60db1-2189">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2189">produces the output</span></span>
```
True
False
False
False
```

<span data-ttu-id="60db1-2190">`s`と`t`変数を指す 2 つの異なる`string`同じ文字を含むインスタンス。</span><span class="sxs-lookup"><span data-stu-id="60db1-2190">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="60db1-2191">最初の比較出力`True`ため、定義済みの文字列の等値演算子 ([文字列等値演算子](expressions.md#string-equality-operators)) が選択されているは、両方のオペランド型の場合`string`。</span><span class="sxs-lookup"><span data-stu-id="60db1-2191">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="60db1-2192">残りの比較をすべて出力`False`1 つまたは両方のオペランドの型の場合、定義済みの参照型の等値演算子が選択されているため、`object`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2192">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="60db1-2193">前述の手法が意味のある値型ではないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="60db1-2193">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="60db1-2194">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2194">The example</span></span>
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
<span data-ttu-id="60db1-2195">出力`False`キャストの 2 つのインスタンスへの参照を作成するためのボックス化`int`値。</span><span class="sxs-lookup"><span data-stu-id="60db1-2195">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="60db1-2196">文字列の等値演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2196">String equality operators</span></span>

<span data-ttu-id="60db1-2197">定義済みの文字列の等値演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2197">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="60db1-2198">2 つ`string`値が等しいと見なされる、次のいずれかが true の場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-2198">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="60db1-2199">両方の値が`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2199">Both values are `null`.</span></span>
*  <span data-ttu-id="60db1-2200">両方の値に各文字の位置と同じ長さと同じ文字を含む文字列のインスタンスに null 参照です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2200">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="60db1-2201">文字列の等値演算子は、文字列の参照ではなく、文字列値を比較します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2201">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="60db1-2202">2 つの別個の文字列のインスタンスに正確な同じ一連の文字が含まれている場合は、文字列の値が等しいかが、参照は異なります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2202">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="60db1-2203">」の説明に従って[参照型の等値演算子](expressions.md#reference-type-equality-operators)参照型の等値演算子は、文字列値ではなく文字列の参照を比較するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2203">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="60db1-2204">デリゲートの等値演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2204">Delegate equality operators</span></span>

<span data-ttu-id="60db1-2205">すべてのデリゲート型には、次の定義済みの比較演算子が暗黙的に用意されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2205">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="60db1-2206">2 つのデリゲート インスタンスが等しいとおり。</span><span class="sxs-lookup"><span data-stu-id="60db1-2206">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="60db1-2207">デリゲートのインスタンスのいずれかの場合`null`、両方とも場合にのみが等しい`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2207">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="60db1-2208">デリゲートがある別の実行時の型と等しいもことはありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2208">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="60db1-2209">呼び出しリストが両方のデリゲート インスタンスがあるかどうか ([デリゲートの宣言](delegates.md#delegate-declarations))、場合にだけ、それぞれの呼び出しリストが同じ長さと 1 つの呼び出しリスト内の各エントリは (下記を参照) のように、それらのインスタンスが等しいその他の呼び出しリスト内の順序で対応するエントリ。</span><span class="sxs-lookup"><span data-stu-id="60db1-2209">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="60db1-2210">次の規則は、呼び出しリストのエントリが等しいかどうかを制御します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2210">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="60db1-2211">2 つの呼び出しリストのエントリが両方が同じ静的に参照している場合、メソッド、エントリは等しくなります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2211">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="60db1-2212">2 つの呼び出しリストのエントリが両方が同じターゲット オブジェクトで同じ非静的メソッドを参照してください (参照の等値演算子で定義) された場合、エントリは等しくなります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2212">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="60db1-2213">呼び出しリストのエントリが意味的に同一の評価から生成された*anonymous_method_expression*s または*lambda_expression*キャプチャされた外部変数の同じ (場合によっては空) のセットをインスタンスは許可されている (ただし必要ありません) と同じにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2213">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="60db1-2214">等値演算子と null</span><span class="sxs-lookup"><span data-stu-id="60db1-2214">Equality operators and null</span></span>

<span data-ttu-id="60db1-2215">`==`と`!=`演算子を null 許容型およびその他がの値の 1 つのオペランドの許可、`null`操作の定義済みまたはユーザー定義の演算子 (でリフトされていない形式またはリフト形式) が存在しない場合でも、リテラル。</span><span class="sxs-lookup"><span data-stu-id="60db1-2215">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="60db1-2216">フォームのいずれかの操作</span><span class="sxs-lookup"><span data-stu-id="60db1-2216">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="60db1-2217">場所`x`演算子のオーバー ロードの解決の場合、null 許容型の式は、([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution)) から、適用可能な演算子の結果の検索に失敗が計算された代わりに、 `HasValue`プロパティの`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2217">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="60db1-2218">具体的には、最初の 2 つのフォームに変換`!x.HasValue`、および最後の 2 つのフォームに変換が`x.HasValue`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2218">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="60db1-2219">演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2219">The is operator</span></span>

<span data-ttu-id="60db1-2220">`is`演算子を使用して動的にオブジェクトの実行時の型が指定された型と互換性をチェックします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2220">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="60db1-2221">操作の結果`E is T`ここで、`E`式を指定し、`T`型、ブール値を示す値かどうか`E`型に正常に変換できます`T`をボックス化の参照変換によって変換またはボックス化解除の変換。</span><span class="sxs-lookup"><span data-stu-id="60db1-2221">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="60db1-2222">操作は、すべての型パラメーターの型引数が置き換えられた後に次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2222">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="60db1-2223">場合`E`、匿名関数は、コンパイル時エラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="60db1-2223">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="60db1-2224">場合`E`メソッド グループ、または`null`リテラルの場合は、の種類`E`参照型または null 許容型の値が`E`は、結果は false、null。</span><span class="sxs-lookup"><span data-stu-id="60db1-2224">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="60db1-2225">それ以外の場合、`D`の動的な型を表す`E`次のようにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2225">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="60db1-2226">場合の種類`E`、参照型では、`D`によってインスタンスの参照の実行時の型は、`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2226">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="60db1-2227">場合の種類`E`が null 許容型では、 `D` null 許容型の基になる型です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2227">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="60db1-2228">場合の種類`E`null 非許容値型では、`D`の型である`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2228">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="60db1-2229">操作の結果が異なります`D`と`T`次のようにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2229">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="60db1-2230">場合`T`、参照型では、結果は true 場合`D`と`T`は場合に、同じ型で`D`は、参照型からの暗黙的な参照変換`D`に`T`が存在する場合、または`D`値型からボックス化変換`D`に`T`存在します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2230">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="60db1-2231">場合`T`が null 許容型では、結果は true 場合`D`の基になる型は、`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2231">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="60db1-2232">場合`T`null 非許容値型では、結果は true 場合`D`と`T`は同じ型。</span><span class="sxs-lookup"><span data-stu-id="60db1-2232">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="60db1-2233">それ以外の場合、結果は false です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2233">Otherwise, the result is false.</span></span>

<span data-ttu-id="60db1-2234">ユーザー定義変換がによっていないと見なされることに注意してください、`is`演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-2234">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="60db1-2235">演算子として、</span><span class="sxs-lookup"><span data-stu-id="60db1-2235">The as operator</span></span>

<span data-ttu-id="60db1-2236">`as`演算子を使用して、特定の参照型または null 許容型に値を明示的に変換します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2236">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="60db1-2237">キャスト式とは異なり ([キャスト式](expressions.md#cast-expressions))、`as`演算子が例外をスローしません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2237">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="60db1-2238">代わりに、指定された変換ができない場合、結果の値は`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2238">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="60db1-2239">フォームの操作で`E as T`、`E`式を指定する必要がありますと`T`参照型、参照型、または null 許容型を既知の型パラメーターである必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2239">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="60db1-2240">さらに、true の場合、次の少なくとも 1 つある必要があります。 またはそれ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2240">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="60db1-2241">Id ([恒等変換](conversions.md#identity-conversion))、暗黙的な null 許容型 ([null 許容型の暗黙的な変換](conversions.md#implicit-nullable-conversions))、暗黙の参照 ([暗黙の参照変換](conversions.md#implicit-reference-conversions))、ボックス化 ([ボックス化変換](conversions.md#boxing-conversions)) 明示的な null 許容型 ([明示的な null 許容変換](conversions.md#explicit-nullable-conversions))、明示的な参照 ([明示的な参照変換](conversions.md#explicit-reference-conversions))、またはボックス化解除 ([ボックス化解除変換](conversions.md#unboxing-conversions)) からの変換が存在する`E`に`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2241">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="60db1-2242">型`E`または`T`がオープン型。</span><span class="sxs-lookup"><span data-stu-id="60db1-2242">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="60db1-2243">`E` `null`リテラル。</span><span class="sxs-lookup"><span data-stu-id="60db1-2243">`E` is the `null` literal.</span></span>

<span data-ttu-id="60db1-2244">コンパイル時の型の場合`E`ない`dynamic`、操作`E as T`と同じ結果になります</span><span class="sxs-lookup"><span data-stu-id="60db1-2244">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="60db1-2245">ただし、`E` が評価されるのは 1 回だけです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2245">except that `E` is only evaluated once.</span></span> <span data-ttu-id="60db1-2246">最適化するために、コンパイラが期待できます`E as T`上記の拡張が含まれる 2 つの動的な型チェックではなく多くて 1 つ動的な型チェックを実行します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2246">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="60db1-2247">コンパイル時の型の場合`E`は`dynamic`、キャスト演算子とは異なり、`as`演算子が動的にバインドされていない ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2247">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="60db1-2248">そのため、拡張がここでは。</span><span class="sxs-lookup"><span data-stu-id="60db1-2248">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="60db1-2249">ユーザー定義変換など、いくつかの変換が可能でないことに注意してください、`as`演算子と、キャスト式を使用する代わりに実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2249">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="60db1-2250">例</span><span class="sxs-lookup"><span data-stu-id="60db1-2250">In the example</span></span>
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
<span data-ttu-id="60db1-2251">型パラメーター`T`の`G`は、クラスの制約があるため、参照型に呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2251">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="60db1-2252">型パラメーター`U`の`H`ないただし。 そのための使用、`as`演算子`H`は許可されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2252">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="60db1-2253">論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2253">Logical operators</span></span>

<span data-ttu-id="60db1-2254">`&`、 `^`、および`|`演算子は論理演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2254">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

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

<span data-ttu-id="60db1-2255">論理演算子のオペランドがコンパイル時の型を持つかどうか`dynamic`、式を動的にバインドし、([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2255">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="60db1-2256">この場合、式のコンパイル時の型は`dynamic`、コンパイル時の型を持つこれらのオペランドの実行時の型を使用して実行時に以下に示す解決が行わ`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2256">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="60db1-2257">フォームの操作の`x op y`ここで、`op`論理演算子をオーバー ロードの解決のいずれか ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution)) を適用して特定の演算子の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2257">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="60db1-2258">オペランドは、選択した演算子のパラメーターの型に変換され、結果の型が演算子の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="60db1-2258">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="60db1-2259">定義済みの論理演算子は、次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2259">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="60db1-2260">整数の論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2260">Integer logical operators</span></span>

<span data-ttu-id="60db1-2261">定義済みの整数の論理演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2261">The predefined integer logical operators are:</span></span>
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

<span data-ttu-id="60db1-2262">`&`演算子はビットごとの計算論理`AND`の 2 つのオペランド、`|`演算子はビット演算を計算論理`OR`の 2 つのオペランドと`^`演算子はビットごとの排他的論理を計算`OR`の 2 つのオペランド。</span><span class="sxs-lookup"><span data-stu-id="60db1-2262">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="60db1-2263">オーバーフローは、これらの操作からではありません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2263">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="60db1-2264">列挙体の論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2264">Enumeration logical operators</span></span>

<span data-ttu-id="60db1-2265">すべての列挙型`E`暗黙的に、次の定義済みの論理演算子を提供します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2265">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="60db1-2266">評価結果`x op y`ここで、`x`と`y`列挙型の式は`E`基になる型`U`、および`op`論理演算子の 1 つと同じでは正確に評価する`(E)((U)x op (U)y)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2266">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="60db1-2267">つまり、列挙型の論理演算子は、2 つのオペランドの基になる型の論理操作を実行するだけです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2267">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="60db1-2268">ブール論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2268">Boolean logical operators</span></span>

<span data-ttu-id="60db1-2269">定義済みのブール型の論理演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2269">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="60db1-2270">`x` と `y` の両方が `true` であれば、`x & y` の結果は `true` です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2270">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="60db1-2271">それ以外の場合、結果は `false` です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2271">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="60db1-2272">結果`x | y`は`true`場合`x`または`y`は`true`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2272">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="60db1-2273">それ以外の場合、結果は `false` です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2273">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="60db1-2274">結果`x ^ y`は`true`場合`x`は`true`と`y`は`false`、または`x`は`false`と`y`は`true`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2274">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="60db1-2275">それ以外の場合、結果は `false` です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2275">Otherwise, the result is `false`.</span></span> <span data-ttu-id="60db1-2276">型のオペランドが場合`bool`、`^`演算子と同じ結果を計算する、`!=`演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-2276">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="60db1-2277">Null 許容のブール論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2277">Nullable boolean logical operators</span></span>

<span data-ttu-id="60db1-2278">Null 許容のブール型`bool?`3 つの値を表すことができます`true`、 `false`、および`null`、概念的には、SQL のブール式に使用される 3 つの値の型に似ています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2278">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="60db1-2279">によって生成される結果を確実に、`&`と`|`演算子の`bool?`オペランドは、SQL の 3 値論理と一貫性のあるが、次の定義済みの演算子が提供は。</span><span class="sxs-lookup"><span data-stu-id="60db1-2279">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="60db1-2280">次の表に、これらの演算子、値のすべての組み合わせによって生成される結果`true`、 `false`、および`null`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2280">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

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

## <a name="conditional-logical-operators"></a><span data-ttu-id="60db1-2281">条件付き論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2281">Conditional logical operators</span></span>

<span data-ttu-id="60db1-2282">`&&`と`||`演算子は、条件付き論理演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2282">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="60db1-2283">「ショート サーキット」論理演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2283">They are also called the "short-circuiting" logical operators.</span></span>

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

<span data-ttu-id="60db1-2284">`&&`と`||`演算子は、条件付きのバージョンの`&`と`|`演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-2284">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="60db1-2285">操作`x && y`、操作に対応する`x & y`ことを除いて、`y`場合にのみ評価されます`x`ない`false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2285">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="60db1-2286">操作`x || y`、操作に対応する`x | y`ことを除いて、`y`場合にのみ評価されます`x`ない`true`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2286">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="60db1-2287">条件付き論理演算子のオペランドがコンパイル時の型を持つかどうか`dynamic`、式を動的にバインドし、([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2287">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="60db1-2288">この場合、式のコンパイル時の型は`dynamic`、コンパイル時の型を持つこれらのオペランドの実行時の型を使用して実行時に以下に示す解決が行わ`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2288">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="60db1-2289">フォームの操作を`x && y`または`x || y`オーバー ロードの解決を適用することによって処理されます ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution))、操作が書き込まれた場合、`x & y`または`x | y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2289">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="60db1-2290">そうしたら</span><span class="sxs-lookup"><span data-stu-id="60db1-2290">Then,</span></span>

*  <span data-ttu-id="60db1-2291">1 つの最適な演算子を検索するオーバー ロードの解決が失敗した場合、またはオーバー ロードの解決では、定義済みの整数の論理演算子のいずれかが選択した場合は、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2291">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="60db1-2292">それ以外の場合、選択した演算子が定義済みのブール型の論理演算子のいずれかである場合 ([ブール論理演算子](expressions.md#boolean-logical-operators)) または null 許容のブール論理演算子 ([null 許容のブール論理演算子](expressions.md#nullable-boolean-logical-operators))、操作が処理されるは」の説明に従って[ブール条件付き論理演算子](expressions.md#boolean-conditional-logical-operators)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2292">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="60db1-2293">それ以外の場合、選択された演算子は、ユーザー定義の演算子と」の説明に従って、操作が処理される[ユーザー定義の条件付き論理演算子](expressions.md#user-defined-conditional-logical-operators)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2293">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="60db1-2294">条件付き論理演算子を直接オーバー ロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2294">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="60db1-2295">ただし、通常の論理演算子の観点から、条件付き論理演算子が評価されるため、通常の論理演算子のオーバー ロードは、一定の制限もと見なされます、条件付き論理演算子のオーバー ロード。</span><span class="sxs-lookup"><span data-stu-id="60db1-2295">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="60db1-2296">詳細についてはこの[ユーザー定義の条件付き論理演算子](expressions.md#user-defined-conditional-logical-operators)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2296">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="60db1-2297">ブール型の条件付き論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2297">Boolean conditional logical operators</span></span>

<span data-ttu-id="60db1-2298">ときのオペランドは、`&&`または`||`型`bool`、や、該当する定義しない型のオペランドが`operator &`または`operator |`への暗黙的な変換を定義する操作を行いますが、`bool`操作が次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2298">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="60db1-2299">操作`x && y`として評価されます`x ? y : false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2299">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="60db1-2300">つまり、`x`が最初に評価され、型に変換する`bool`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2300">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="60db1-2301">その後、場合`x`は`true`、`y`が評価され、型に変換`bool`操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2301">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="60db1-2302">操作の結果は、それ以外の場合、`false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2302">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="60db1-2303">操作`x || y`として評価されます`x ? true : y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2303">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="60db1-2304">つまり、`x`が最初に評価され、型に変換する`bool`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2304">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="60db1-2305">場合はその後、`x`は`true`、操作の結果は`true`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2305">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="60db1-2306">それ以外の場合、`y`が評価され、型に変換する`bool`操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2306">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="60db1-2307">ユーザー定義の条件付き論理演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2307">User-defined conditional logical operators</span></span>

<span data-ttu-id="60db1-2308">ときに、オペランドの`&&`または`||`は、該当する宣言型のユーザー定義`operator &`または`operator |`、場所で true の場合、ある、次の両方必要があります`T`は選択した演算子が宣言されている型です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2308">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="60db1-2309">戻り値の型と、選択した演算子の各パラメーターの型である必要があります`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2309">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="60db1-2310">つまり、演算子を論理計算`AND`または論理`OR`型の 2 つのオペランドの`T`、型の結果を返す必要があります`T`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2310">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="60db1-2311">`T` 宣言を含める必要があります`operator true`と`operator false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2311">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="60db1-2312">バインディング エラーは、これらの要件のいずれかが満たされない場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2312">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="60db1-2313">それ以外の場合、`&&`または`||`操作は、ユーザー定義を組み合わせることによって評価されます`operator true`または`operator false`選択したユーザー定義演算子を使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2313">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="60db1-2314">操作`x && y`として評価されます`T.false(x) ? x : T.&(x, y)`ここで、`T.false(x)`の呼び出し、`operator false`で宣言されている`T`、および`T.&(x, y)`の選択した呼び出し`operator &`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2314">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="60db1-2315">つまり、`x`が最初に評価および`operator false`がどうかを判断の結果に対して呼び出す`x`が確実に false。</span><span class="sxs-lookup"><span data-stu-id="60db1-2315">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="60db1-2316">その後、if`x`が間違いなく false の場合、操作の結果は、以前に計算値`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2316">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="60db1-2317">それ以外の場合、`y`が評価されと、選択した`operator &`が以前に計算値で呼び出される`x`計算された値と`y`操作の結果を生成します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2317">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="60db1-2318">操作`x || y`として評価されます`T.true(x) ? x : T.|(x, y)`ここで、`T.true(x)`の呼び出し、`operator true`で宣言されている`T`、および`T.|(x,y)`の選択した呼び出し`operator|`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2318">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="60db1-2319">つまり、`x`が最初に評価および`operator true`がどうかを判断の結果に対して呼び出す`x`が確実に true です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2319">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="60db1-2320">その後、if`x`が間違いなく true の場合、操作の結果は、以前に計算値`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2320">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="60db1-2321">それ以外の場合、`y`が評価されと、選択した`operator |`が以前に計算値で呼び出される`x`計算された値と`y`操作の結果を生成します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2321">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="60db1-2322">これらの操作が指定された式のいずれかで`x`はによって指定された式で 1 回、評価だけ`y`はないか、評価、1 回だけ評価します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2322">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="60db1-2323">実装する型の例については`operator true`と`operator false`を参照してください[ブール型のデータベース](structs.md#database-boolean-type)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2323">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="60db1-2324">Null 合体演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2324">The null coalescing operator</span></span>

<span data-ttu-id="60db1-2325">`??`演算子は、null 合体演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2325">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="60db1-2326">フォームの場合は、null 結合式`a ?? b`必要`a`null 許容型または参照型であります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2326">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="60db1-2327">場合`a`null 以外の場合は、結果`a ?? b`は`a`、それ以外の結果は`b`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2327">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="60db1-2328">操作の評価`b`場合にのみ`a`が null です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2328">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="60db1-2329">Null 合体演算子の右から左は"右から左へ"です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2329">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="60db1-2330">たとえば、フォームの式`a ?? b ?? c`として評価されます`a ?? (b ?? c)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2330">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="60db1-2331">一般に用語、フォームの式`E1 ?? E2 ?? ... ?? En`オペランドの最初の null 以外の場合、またはすべてのオペランドが null の場合は null を返します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2331">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="60db1-2332">式の型`a ?? b`暗黙の変換がオペランドに対して使用可能なによって異なります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2332">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="60db1-2333">種類の優先度の順序で`a ?? b`は`A0`、 `A`、または`B`ここで、`A`の種類は、 `a` (される`a`型があります)、`B`の種類です`b`(される`b`型があります)、および`A0`の基になる型は、`A`場合`A`が null 許容型では、または`A`それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-2333">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="60db1-2334">具体的には、`a ?? b`ように処理されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2334">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="60db1-2335">場合`A`が存在し、ない null 許容型または参照型では、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2335">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="60db1-2336">場合`b`動的な式は、結果型は`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2336">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="60db1-2337">実行時に、`a`が最初に評価します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2337">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="60db1-2338">場合`a`が null でない`a`は、動的に変換し、これが結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2338">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="60db1-2339">それ以外の場合、`b`が評価され、結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2339">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="60db1-2340">の場合`A`が存在する null 許容型であり、からの暗黙的な変換が存在する`b`に`A0`、結果型は`A0`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2340">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="60db1-2341">実行時に、`a`が最初に評価します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2341">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="60db1-2342">場合`a`が null でない`a`型にラップされて`A0`結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2342">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="60db1-2343">それ以外の場合、`b`が評価され、型に変換する`A0`結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2343">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="60db1-2344">の場合`A`が存在するからの暗黙的な変換が存在して`b`に`A`、結果型は`A`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2344">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="60db1-2345">実行時に、`a`が最初に評価します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2345">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="60db1-2346">場合`a`が null でない`a`結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2346">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="60db1-2347">それ以外の場合、`b`が評価され、型に変換する`A`結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2347">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="60db1-2348">の場合`b`型を持つ`B`からの暗黙的な変換が存在して`a`に`B`、結果型は`B`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2348">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="60db1-2349">実行時に、`a`が最初に評価します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2349">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="60db1-2350">場合`a`が null でない`a`型にラップされて`A0`(場合`A`が存在し、null 許容) と型に変換された`B`結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2350">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="60db1-2351">それ以外の場合、`b`が評価され、結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2351">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="60db1-2352">それ以外の場合、`a`と`b`は、互換性のないと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2352">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="60db1-2353">条件演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2353">Conditional operator</span></span>

<span data-ttu-id="60db1-2354">`?:`演算子は条件演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2354">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="60db1-2355">三項演算子にも呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2355">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="60db1-2356">フォームの条件式`b ? x : y`条件を最初に評価する`b`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2356">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="60db1-2357">その後、if`b`は`true`、`x`が評価され、操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2357">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="60db1-2358">それ以外の場合、`y`が評価され、操作の結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2358">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="60db1-2359">条件式を決して両方評価`x`と`y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2359">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="60db1-2360">条件演算子の右から左は"右から左へ"です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2360">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="60db1-2361">たとえば、フォームの式`a ? b : c ? d : e`として評価されます`a ? b : (c ? d : e)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2361">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="60db1-2362">最初のオペランド、`?:`演算子に暗黙的に変換できる式である必要があります`bool`、または実装する型の式`operator true`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2362">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="60db1-2363">どちらもこれらの要件が満たされている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2363">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="60db1-2364">2 番目と 3 番目のオペランド`x`と`y`の`?:`演算子は、条件式の種類を制御します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2364">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="60db1-2365">場合`x`型を持つ`X`と`y`型を持つ`Y`し</span><span class="sxs-lookup"><span data-stu-id="60db1-2365">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="60db1-2366">暗黙の変換 ([暗黙的な変換](conversions.md#implicit-conversions)) から存在する`X`に`Y`がからではなく`Y`に`X`、し`Y`条件式の種類です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="60db1-2367">暗黙の変換 ([暗黙的な変換](conversions.md#implicit-conversions)) から存在する`Y`に`X`がからではなく`X`に`Y`、し`X`条件式の種類です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2367">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="60db1-2368">それ以外の場合、式の型を決定できないと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2368">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="60db1-2369">だけの場合のいずれかの`x`と`y`種類、およびその両方を持つ`x`と`y`の条件付きの式の型は、その型に暗黙的に変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2369">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="60db1-2370">それ以外の場合、式の型を決定できないと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2370">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="60db1-2371">フォームの条件式の実行時の処理`b ? x : y`次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2371">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="60db1-2372">最初に、`b`が評価され、および`bool`の値`b`決定されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2372">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="60db1-2373">暗黙の変換の種類から`b`に`bool`この暗黙の変換が生成するために実行されますが、存在する、`bool`値。</span><span class="sxs-lookup"><span data-stu-id="60db1-2373">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="60db1-2374">それ以外の場合、`operator true`の型によって定義された`b`が生成するために呼び出される、`bool`値。</span><span class="sxs-lookup"><span data-stu-id="60db1-2374">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="60db1-2375">場合、`bool`前の手順で生成される値は`true`、し`x`が評価され、条件付きの式の型に変換し、条件付きの式の結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2375">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="60db1-2376">それ以外の場合、`y`が評価され、条件付きの式の型に変換し、条件付きの式の結果になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2376">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="60db1-2377">匿名関数式</span><span class="sxs-lookup"><span data-stu-id="60db1-2377">Anonymous function expressions</span></span>

<span data-ttu-id="60db1-2378">***匿名関数***"line"のメソッド定義を表す式を指定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2378">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="60db1-2379">匿名関数はありません値または型自体が互換性のあるデリゲートまたは式ツリー型に変換可能です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2379">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="60db1-2380">匿名関数の変換の評価は、変換のターゲットの種類によって異なります。デリゲート型では、変換は、匿名関数を定義する方法を参照するデリゲート値に評価します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2380">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="60db1-2381">式ツリー型である場合は、オブジェクトの構造体としてのメソッドの構造を表す式ツリーに変換が評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2381">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="60db1-2382">歴史的な理由は 2 つ構文種類あります匿名関数は、namely *lambda_expression*s と*anonymous_method_expression*秒。</span><span class="sxs-lookup"><span data-stu-id="60db1-2382">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="60db1-2383">ほぼすべての目的で、 *lambda_expression*s は簡潔でより表現力豊かな*anonymous_method_expression*で、これは下位互換性の言語で表示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2383">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

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

<span data-ttu-id="60db1-2384">`=>` 演算子と代入 (`=`) は優先順位が同じで、結合規則が右から左です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2384">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="60db1-2385">ある匿名関数を`async`修飾子は、非同期関数でありで説明されている規則に従って[反復子](classes.md#iterators)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2385">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="60db1-2386">形式で匿名関数のパラメーターを*lambda_expression*明示的または暗黙的に型指定できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2386">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="60db1-2387">明示的に型指定されたパラメーター リストの各パラメーターの型を明示的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2387">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="60db1-2388">匿名関数が発生したコンテキストから、暗黙的に型指定されたパラメーターの一覧で、パラメーターの型の推論-互換性のあるデリゲート型または型を提供する式ツリーの型、匿名関数が変換される場合に具体的には、パラメーターの型 ([匿名関数の変換](conversions.md#anonymous-function-conversions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2388">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="60db1-2389">1 つ、暗黙的に型指定されたパラメーターを使用して、匿名関数にパラメーター リストから、かっこは省略できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2389">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="60db1-2390">つまり、フォームの匿名関数</span><span class="sxs-lookup"><span data-stu-id="60db1-2390">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="60db1-2391">省略することができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2391">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="60db1-2392">形式で匿名関数のパラメーター リスト、 *anonymous_method_expression*は省略可能です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2392">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="60db1-2393">指定した場合、パラメーターを明示的に入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2393">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="60db1-2394">匿名の関数は任意のパラメーターを持つデリゲートに変換できる、そうでない場合の一覧が含まれていない`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="60db1-2394">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="60db1-2395">A*ブロック*匿名関数の本体に到達 ([エンドポイントと到達可能性](statements.md#end-points-and-reachability)) しない限り、匿名関数で到達できないステートメント内に発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2395">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="60db1-2396">匿名関数の例をいくつかは、以下に従います。</span><span class="sxs-lookup"><span data-stu-id="60db1-2396">Some examples of anonymous functions follow below:</span></span>

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

<span data-ttu-id="60db1-2397">動作*lambda_expression*s と*anonymous_method_expression*s は、次の点を除けば同じです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2397">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="60db1-2398">*anonymous_method_expression*s 許可を完全に省略パラメーター リストでもデリゲート パラメーターの値の一覧の型を生成します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2398">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="60db1-2399">*lambda_expression*s パラメーターの型は省略され、一方の推論を許可する*anonymous_method_expression*s がパラメーターの型を明示的に指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2399">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="60db1-2400">本文を*lambda_expression*は式またはステートメント ブロックを指定できますの本文を*anonymous_method_expression*ステートメント ブロックにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2400">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="60db1-2401">のみ*lambda_expression*s がある互換性のある式ツリー型への変換 ([式ツリー型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2401">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="60db1-2402">匿名関数のシグネチャ</span><span class="sxs-lookup"><span data-stu-id="60db1-2402">Anonymous function signatures</span></span>

<span data-ttu-id="60db1-2403">省略可能な*anonymous_function_signature*匿名関数の名前と必要に応じて、匿名関数の仮パラメーターの種類を定義します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2403">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="60db1-2404">匿名関数のパラメーターのスコープは、 *anonymous_function_body*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2404">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="60db1-2405">([スコープ](basic-concepts.md#scopes)) 匿名メソッド本体の宣言領域の構成 (指定された) 場合は、パラメーター リストと共に ([宣言](basic-concepts.md#declarations))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2405">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="60db1-2406">ローカル変数、ローカル定数またはそのスコープに含まれるパラメーターの名前と一致する匿名関数のパラメーターの名前のコンパイル時エラーであるため、 *anonymous_method_expression*または*lambda_式*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2406">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="60db1-2407">匿名関数がある場合、 *explicit_anonymous_function_signature*、互換性のあるデリゲート型と式ツリー型のセットは同じ順序で、同じパラメーターの型および修飾子を持つものに制限されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2407">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="60db1-2408">メソッド グループ変換とは対照的 ([メソッド グループ変換](conversions.md#method-group-conversions))、匿名関数のパラメーター型の反変性がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2408">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="60db1-2409">匿名関数がない場合、 *anonymous_function_signature*、互換性のあるデリゲート型と式ツリー型のセットがないものだけに制限し、`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="60db1-2409">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="60db1-2410">なお、 *anonymous_function_signature*属性またはパラメーター配列に含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2410">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="60db1-2411">ただし、 *anonymous_function_signature*パラメーター リスト持つにはには、パラメーター配列が含まれています。 デリゲート型と互換性があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2411">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="60db1-2412">互換性のあるがコンパイル時に引き続き失敗する場合でも、式ツリー型に変換をまた ([式ツリー型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2412">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="60db1-2413">匿名関数の本体</span><span class="sxs-lookup"><span data-stu-id="60db1-2413">Anonymous function bodies</span></span>

<span data-ttu-id="60db1-2414">本文 (*式*または*ブロック*) 匿名関数は、次の規則に従います。</span><span class="sxs-lookup"><span data-stu-id="60db1-2414">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="60db1-2415">匿名関数には、署名が含まれている場合、シグネチャで指定されたパラメーターは本体で使用できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2415">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="60db1-2416">デリゲート型またはパラメーターを持つ式の型に変換できる場合は、匿名関数のシグネチャはありません ([匿名関数の変換](conversions.md#anonymous-function-conversions)) が、パラメーターは、本文にアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2416">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="60db1-2417">除く`ref`または`out`最も外側の署名 (存在する場合) で指定されたパラメーター、コンパイル時エラー、本文にアクセスするには匿名関数を`ref`または`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="60db1-2417">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="60db1-2418">ときの種類`this`、構造体型には、コンパイル時エラー、本文にアクセスするには`this`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2418">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="60db1-2419">これは true かどうか、アクセスが明示的な (としてで`this.x`) または暗黙的な (と`x`場所`x`構造体のインスタンス メンバー)。</span><span class="sxs-lookup"><span data-stu-id="60db1-2419">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="60db1-2420">単に、このルールは、このようなアクセスを禁止し、メンバーの検索結果、構造体のメンバーであるかどうかには影響しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2420">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="60db1-2421">本体が外側の変数へのアクセス ([外部変数](expressions.md#outer-variables)) 匿名関数の。</span><span class="sxs-lookup"><span data-stu-id="60db1-2421">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="60db1-2422">外部変数へのアクセスは、一度にアクティブになっている変数のインスタンスを参照、 *lambda_expression*または*anonymous_method_expression*が評価されます ([の評価匿名関数式](expressions.md#evaluation-of-anonymous-function-expressions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2422">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="60db1-2423">コンパイル時エラーを含む本文には、`goto`ステートメントでは、`break`ステートメント、または`continue`対象である、本文の外側、または含まれている匿名関数の本体内のステートメント。</span><span class="sxs-lookup"><span data-stu-id="60db1-2423">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="60db1-2424">A`return`本体のステートメントでは、最も近い外側の呼び出しから制御を返します、外側の関数メンバーからではなく、匿名関数です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2424">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="60db1-2425">指定された式を`return`ステートメントは、デリゲート型または式ツリー型の戻り値の型に暗黙的に変換可能である必要があります囲む最も近い*lambda_expression*または*anonymous_method_expression*変換されます ([匿名関数の変換](conversions.md#anonymous-function-conversions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2425">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="60db1-2426">評価版の呼び出しから以外の匿名関数のブロックを実行する方法があるかどうかは明示的に指定されていません、 *lambda_expression*または*anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="60db1-2426">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="60db1-2427">具体的には、コンパイラが 1 つを合成することで、匿名関数を実装することをまたは以上の名前付きメソッドまたは型。</span><span class="sxs-lookup"><span data-stu-id="60db1-2427">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="60db1-2428">このような合成要素の名前は、コンパイラを使用するために予約された形式でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2428">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="60db1-2429">オーバー ロードの解決と匿名関数</span><span class="sxs-lookup"><span data-stu-id="60db1-2429">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="60db1-2430">匿名関数の引数リストでは、型の推定に参加し、オーバー ロードの解決。</span><span class="sxs-lookup"><span data-stu-id="60db1-2430">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="60db1-2431">参照してください[型推論](expressions.md#type-inference)と[オーバー ロードの解決](expressions.md#overload-resolution)の厳密な規則です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2431">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="60db1-2432">次の例は、匿名関数のオーバー ロードの解決への影響を示しています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2432">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

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

<span data-ttu-id="60db1-2433">`ItemList<T>`クラスには 2 つ`Sum`メソッド。</span><span class="sxs-lookup"><span data-stu-id="60db1-2433">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="60db1-2434">それぞれ、`selector`引数で、リスト項目から経由での合計に値を抽出します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2434">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="60db1-2435">抽出された値には、いずれかを指定できます、`int`または`double`し、結果の合計は、いずれかでも同様に、`int`または`double`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2435">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="60db1-2436">`Sum`リストの順序の詳細行の合計を計算するメソッドを使用例でした。</span><span class="sxs-lookup"><span data-stu-id="60db1-2436">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

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

<span data-ttu-id="60db1-2437">最初の呼び出しで`orderDetails.Sum`の両方を`Sum`メソッドは、適用可能なため、匿名関数`d => d. UnitCount`は両方と互換性が`Func<Detail,int>`と`Func<Detail,double>`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2437">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="60db1-2438">ただし、オーバー ロードの解決が最初に検出`Sum`メソッドのためへの変換`Func<Detail,int>`への変換よりも優れて`Func<Detail,double>`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2438">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="60db1-2439">2 つ目の呼び出しで`orderDetails.Sum`し、2 番目`Sum`メソッドは、適用可能なため、匿名関数`d => d.UnitPrice * d.UnitCount`型の値を生成`double`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2439">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="60db1-2440">したがって、オーバー ロードの解決は、2 つ目を取得`Sum`その呼び出しのメソッド。</span><span class="sxs-lookup"><span data-stu-id="60db1-2440">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="60db1-2441">匿名関数と動的バインディング</span><span class="sxs-lookup"><span data-stu-id="60db1-2441">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="60db1-2442">匿名関数は、受信者、引数または動的に連結演算のオペランドにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2442">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="60db1-2443">外部変数</span><span class="sxs-lookup"><span data-stu-id="60db1-2443">Outer variables</span></span>

<span data-ttu-id="60db1-2444">任意のローカル変数、パラメーターの値、またはがスコープに含まれるパラメーター配列、 *lambda_expression*または*anonymous_method_expression*と呼ばれますが、***外部変数***匿名関数です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2444">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="60db1-2445">クラスのインスタンス関数のメンバーで、`this`値が値を持つパラメーターと見なされ、関数メンバー内に含まれるすべての匿名関数の外部変数は、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2445">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="60db1-2446">外部変数のキャプチャ</span><span class="sxs-lookup"><span data-stu-id="60db1-2446">Captured outer variables</span></span>

<span data-ttu-id="60db1-2447">外部変数は、匿名関数によって参照される、外部変数があると言います。***キャプチャ***によって匿名関数です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2447">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="60db1-2448">通常、ローカル変数の有効期間は、ブロックまたはそれが関連付けられているステートメントの実行に制限されています ([ローカル変数](variables.md#local-variables))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2448">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="60db1-2449">ただし、キャプチャされた外部変数の有効期間は、デリゲートまで少なくとも拡張または匿名関数から作成された式ツリーがガベージ コレクションの対象になります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2449">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="60db1-2450">例</span><span class="sxs-lookup"><span data-stu-id="60db1-2450">In the example</span></span>
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
<span data-ttu-id="60db1-2451">ローカル変数`x`は匿名の関数との有効期間によってキャプチャ`x`から返されたデリゲートまで少なくとも拡張`F`(の末尾まで発生しませんが、ガベージ コレクションの対象になります。プログラム)。</span><span class="sxs-lookup"><span data-stu-id="60db1-2451">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="60db1-2452">匿名関数の呼び出しごとの動作の同じインスタンスであるため`x`例の出力には。</span><span class="sxs-lookup"><span data-stu-id="60db1-2452">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```
1
2
3
```

<span data-ttu-id="60db1-2453">匿名関数によって、ローカル変数または値を持つパラメーターがキャプチャされると、ローカル変数またはパラメーターは不要になったと見なされます固定変数 ([固定属性と移動可能変数](unsafe-code.md#fixed-and-moveable-variables)) は代わりに、移動可能と見なされますが、変数。</span><span class="sxs-lookup"><span data-stu-id="60db1-2453">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="60db1-2454">したがって、`unsafe`キャプチャされた外部変数のアドレスを取得するコードを使用する必要があります最初、`fixed`ステートメント、変数を解決します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2454">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="60db1-2455">なお uncaptured の変数とは異なり、キャプチャされたローカル変数を複数の実行スレッドを同時に公開できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2455">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="60db1-2456">ローカル変数のインスタンス化</span><span class="sxs-lookup"><span data-stu-id="60db1-2456">Instantiation of local variables</span></span>

<span data-ttu-id="60db1-2457">ローカル変数があると見なされます***インスタンス化***実行が、変数のスコープに入ったとき。</span><span class="sxs-lookup"><span data-stu-id="60db1-2457">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="60db1-2458">次のメソッドの呼び出し時に、ローカル変数など、`x`がインスタンス化され、3 回の初期化など、ループの反復ごとに 1 回。</span><span class="sxs-lookup"><span data-stu-id="60db1-2458">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="60db1-2459">ただしの宣言の移動`x`に 1 つのインスタンス化のループの外側`x`:</span><span class="sxs-lookup"><span data-stu-id="60db1-2459">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="60db1-2460">キャプチャされていないときに、ローカル変数がインスタンス化される正確にどのくらいの頻度を確認する方法はありません: インスタンス化の有効期間が互いに離れて、ため、単に同じストレージの場所を使用するには、各インスタンスのことができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2460">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="60db1-2461">ただし、ローカル変数をキャプチャする匿名関数をインスタンス化の効果は明らかになります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2461">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="60db1-2462">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2462">The example</span></span>
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
<span data-ttu-id="60db1-2463">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2463">produces the output:</span></span>
```
1
3
5
```

<span data-ttu-id="60db1-2464">ただしの宣言`x`ループの外側に移動されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2464">However, when the declaration of `x` is moved outside the loop:</span></span>
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
<span data-ttu-id="60db1-2465">出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2465">the output is:</span></span>
```
5
5
5
```

<span data-ttu-id="60db1-2466">For ループでは、繰り返し変数を宣言すると、その変数自体をループの外で宣言すると見なされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2466">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="60db1-2467">したがって、自体の反復変数をキャプチャする例を変更した場合。</span><span class="sxs-lookup"><span data-stu-id="60db1-2467">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="60db1-2468">反復変数の 1 つだけのインスタンスをキャプチャしたら、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2468">only one instance of the iteration variable is captured, which produces the output:</span></span>
```
3
3
3
```

<span data-ttu-id="60db1-2469">匿名関数のデリゲートは、いくつかのキャプチャされた変数を共有しながら他のユーザーの別のインスタンスがあることができます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2469">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="60db1-2470">たとえば場合、`F`に変更されます</span><span class="sxs-lookup"><span data-stu-id="60db1-2470">For example, if `F` is changed to</span></span>
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
<span data-ttu-id="60db1-2471">3 つのデリゲートの同じインスタンスをキャプチャする`x`がのインスタンスを区切る`y`出力は。</span><span class="sxs-lookup"><span data-stu-id="60db1-2471">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```
1 1
2 1
3 1
```

<span data-ttu-id="60db1-2472">個別の匿名関数には、外部変数の同じインスタンスをキャプチャできます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2472">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="60db1-2473">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2473">In the example:</span></span>
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
<span data-ttu-id="60db1-2474">2 つの匿名関数は、ローカル変数の同じインスタンスをキャプチャ`x`、したがって「通信できる」変数を通じてとします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2474">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="60db1-2475">この例の出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2475">The output of the example is:</span></span>
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="60db1-2476">匿名関数式の評価</span><span class="sxs-lookup"><span data-stu-id="60db1-2476">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="60db1-2477">匿名関数`F`常にデリゲート型に変換する必要があります`D`または式ツリー型`E`、直接、またはデリゲート作成式の実行により`new D(F)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2477">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="60db1-2478">」の説明に従って、この変換は、匿名関数の結果を決定します。[匿名関数の変換](conversions.md#anonymous-function-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2478">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="60db1-2479">クエリ式</span><span class="sxs-lookup"><span data-stu-id="60db1-2479">Query expressions</span></span>

<span data-ttu-id="60db1-2480">***クエリ式***SQL や XQuery などのリレーショナルで階層的なクエリ言語に類似したクエリに対して統合言語の構文を提供します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2480">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

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

<span data-ttu-id="60db1-2481">クエリ式の始まりを`from`句といずれかで終わる、`select`または`group`句。</span><span class="sxs-lookup"><span data-stu-id="60db1-2481">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="60db1-2482">初期`from`句の後に 0 個以上をできます`from`、 `let`、 `where`、`join`または`orderby`句。</span><span class="sxs-lookup"><span data-stu-id="60db1-2482">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="60db1-2483">各`from`句は、ジェネレーターの概要、***範囲変数***の要素範囲を***シーケンス***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2483">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="60db1-2484">各`let`句には前の範囲変数を使用して計算値を表す範囲変数が導入されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2484">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="60db1-2485">各`where`句は結果から項目を除外するフィルター。</span><span class="sxs-lookup"><span data-stu-id="60db1-2485">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="60db1-2486">各`join`句がソース シーケンスの指定のキーと一致するペアを生成するもう 1 つのシーケンスのキーを比較します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2486">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="60db1-2487">各`orderby`句が指定した条件に従って項目を並べ替えます。最終的な`select`または`group`句は、範囲変数の観点から結果の形状を指定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2487">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="60db1-2488">最後に、`into`句は、1 つのクエリの結果を後続のクエリでのジェネレーターとして扱うことでクエリを「接続」を使用できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2488">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="60db1-2489">クエリ式のあいまいさ</span><span class="sxs-lookup"><span data-stu-id="60db1-2489">Ambiguities in query expressions</span></span>

<span data-ttu-id="60db1-2490">クエリ式には、「コンテキスト キーワード」、つまりを特定のコンテキストで特別な意味を持つ識別子の数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2490">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="60db1-2491">具体的には、これらは`from`、 `where`、 `join`、 `on`、 `equals`、 `into`、 `let`、 `orderby`、 `ascending`、 `descending`、 `select`、`group`と`by`.</span><span class="sxs-lookup"><span data-stu-id="60db1-2491">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="60db1-2492">キーワードまたは簡易名としてこれらの識別子の混在の使用によって発生するクエリ式のあいまいさを回避するためにこれらの識別子はキーワード時に考慮クエリ式内で任意の場所に発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2492">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="60db1-2493">この目的では、クエリ式は任意の式で始まる"`from identifier`「を除く任意のトークンが続く」`;`「,」`=`「または」`,`"。</span><span class="sxs-lookup"><span data-stu-id="60db1-2493">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="60db1-2494">クエリ式内で識別子としてこれらの単語を使用するには、先頭"`@`"([識別子](lexical-structure.md#identifiers))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2494">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="60db1-2495">クエリ式の変換</span><span class="sxs-lookup"><span data-stu-id="60db1-2495">Query expression translation</span></span>

<span data-ttu-id="60db1-2496">C# 言語では、クエリ式の実行のセマンティクスが指定されていません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2496">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="60db1-2497">代わりに、クエリ式に準拠しているメソッドの呼び出しに変換、*クエリ式パターン*([、クエリ式パターン](expressions.md#the-query-expression-pattern))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2497">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="60db1-2498">具体的には、クエリ式は、名前付きメソッドの呼び出しに変換`Where`、 `Select`、 `SelectMany`、 `Join`、 `GroupJoin`、 `OrderBy`、 `OrderByDescending`、 `ThenBy`、 `ThenByDescending`、 `GroupBy`、および`Cast`します。」の説明に従って、これらのメソッドが特定のシグネチャと結果型が予想される[、クエリ式パターン](expressions.md#the-query-expression-pattern)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2498">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="60db1-2499">これらのメソッドは、クエリ対象のオブジェクトのインスタンス メソッドまたは外部のオブジェクトにある拡張メソッドを指定でき、クエリの実際の実行を実装します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2499">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="60db1-2500">クエリ式からのメソッド呼び出しへの変換は、型のバインディングの前に発生した構文マップまたはオーバー ロードの解決が実行されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2500">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="60db1-2501">構文的に正しい変換のことが保証されますが、意味的に正しい c# コードを生成するためには保証されません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2501">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="60db1-2502">次のクエリ式の変換は、通常のメソッドの呼び出しとして処理されます結果として得られるメソッドの呼び出しをこの可能性がありますさらに発見し、エラーの例では、メソッドが存在しない場合、引数が正しくない型を持つ場合、またはメソッドがジェネリックの場合、。型の推論は失敗します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2502">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="60db1-2503">クエリ式は、考えられるさらなる削減がなくなるまで繰り返し次の変換を適用することで処理されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2503">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="60db1-2504">翻訳は、アプリケーションの順序で並んでいる: 各セクションでは、前のセクションで翻訳が徹底的に、実行されることと、不足すると後、セクションがいない後で再訪問する同じクエリ式の処理に前提としています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2504">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="60db1-2505">クエリ式では、範囲変数に代入することはできません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2505">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="60db1-2506">ただし、c# の実装は、常にではありませんので、これも不可能になるここに示す構文変換の方法で、この制限を適用する許可されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2506">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="60db1-2507">特定の翻訳によって示される透過的な識別子を持つ範囲変数に挿入`*`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2507">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="60db1-2508">透過的な識別子の特殊なプロパティの説明でさらに[透過的識別子](expressions.md#transparent-identifiers)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2508">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="60db1-2509">継続を選択し、groupby 句</span><span class="sxs-lookup"><span data-stu-id="60db1-2509">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="60db1-2510">連続したクエリ式</span><span class="sxs-lookup"><span data-stu-id="60db1-2510">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="60db1-2511">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2511">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="60db1-2512">次のセクションでの翻訳がクエリされていないことを想定しています`into`継続します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2512">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="60db1-2513">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2513">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="60db1-2514">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2514">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="60db1-2515">最終的な変換は、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2515">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="60db1-2516">明示的な範囲変数の型</span><span class="sxs-lookup"><span data-stu-id="60db1-2516">Explicit range variable types</span></span>

<span data-ttu-id="60db1-2517">A`from`範囲変数の型を明示的に指定する句</span><span class="sxs-lookup"><span data-stu-id="60db1-2517">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="60db1-2518">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2518">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="60db1-2519">A`join`範囲変数の型を明示的に指定する句</span><span class="sxs-lookup"><span data-stu-id="60db1-2519">A `join` clause that explicitly specifies a range variable type</span></span>
```
join T x in e on k1 equals k2
```
<span data-ttu-id="60db1-2520">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2520">is translated into</span></span>
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="60db1-2521">次のセクションでの翻訳では、クエリに明示的な範囲変数の型があるないことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2521">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="60db1-2522">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2522">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="60db1-2523">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2523">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="60db1-2524">最終的な変換は、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2524">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="60db1-2525">明示的な範囲変数の型は、非ジェネリックを実装するコレクションを照会するための便利な`IEnumerable`インターフェイスがジェネリックではない`IEnumerable<T>`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="60db1-2525">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="60db1-2526">上記の例では、となり場合`customers`型の`ArrayList`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2526">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="60db1-2527">逆クエリ式</span><span class="sxs-lookup"><span data-stu-id="60db1-2527">Degenerate query expressions</span></span>

<span data-ttu-id="60db1-2528">形式のクエリ式</span><span class="sxs-lookup"><span data-stu-id="60db1-2528">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="60db1-2529">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2529">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="60db1-2530">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2530">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="60db1-2531">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2531">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="60db1-2532">逆クエリ式では、普通に元の要素を選択する 1 つです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2532">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="60db1-2533">翻訳の後のフェーズでは、逆のクエリには、ソースで置き換えることで、その他の翻訳の手順で導入されたを削除します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2533">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="60db1-2534">こと、クエリの結果を確認するは決して自体には、ソース オブジェクトに式の種類とソースの id を明らかに、クエリのクライアント。</span><span class="sxs-lookup"><span data-stu-id="60db1-2534">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="60db1-2535">そのため、この手順が明示的に呼び出すことによって、ソース コードに直接書き込まれた逆のクエリを保護`Select`ソース。</span><span class="sxs-lookup"><span data-stu-id="60db1-2535">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="60db1-2536">実装側の役目です`Select`とこれらのメソッドが、ソース オブジェクト自体を返さないことを確認するには、その他のクエリ演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-2536">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="60db1-2537">Let、where、結合と orderby 句</span><span class="sxs-lookup"><span data-stu-id="60db1-2537">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="60db1-2538">2 番目のクエリ式`from`句が続く、`select`句</span><span class="sxs-lookup"><span data-stu-id="60db1-2538">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="60db1-2539">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2539">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="60db1-2540">2 番目のクエリ式`from`句が以外のものに後に、`select`句。</span><span class="sxs-lookup"><span data-stu-id="60db1-2540">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="60db1-2541">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2541">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="60db1-2542">クエリ式、`let`句</span><span class="sxs-lookup"><span data-stu-id="60db1-2542">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="60db1-2543">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2543">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="60db1-2544">クエリ式、`where`句</span><span class="sxs-lookup"><span data-stu-id="60db1-2544">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="60db1-2545">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2545">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="60db1-2546">クエリ式、`join`句なしで、`into`続けて、`select`句</span><span class="sxs-lookup"><span data-stu-id="60db1-2546">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="60db1-2547">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2547">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="60db1-2548">クエリ式、`join`句なしで、`into`以外のものによってその後に、`select`句</span><span class="sxs-lookup"><span data-stu-id="60db1-2548">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="60db1-2549">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2549">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="60db1-2550">クエリ式、`join`句、`into`続けて、`select`句</span><span class="sxs-lookup"><span data-stu-id="60db1-2550">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="60db1-2551">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2551">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="60db1-2552">クエリ式、`join`句、`into`以外のものによってその後に、`select`句</span><span class="sxs-lookup"><span data-stu-id="60db1-2552">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="60db1-2553">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2553">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="60db1-2554">クエリ式、`orderby`句</span><span class="sxs-lookup"><span data-stu-id="60db1-2554">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="60db1-2555">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2555">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="60db1-2556">句を指定している場合は、順序付け、`descending`方向インジケーターの呼び出しが、`OrderByDescending`または`ThenByDescending`代わりに生成されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2556">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="60db1-2557">次の変換があることを想定していますありません`let`、 `where`、`join`または`orderby`句、および 1 つ初期`from`各クエリ式の句。</span><span class="sxs-lookup"><span data-stu-id="60db1-2557">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="60db1-2558">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2558">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="60db1-2559">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2559">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="60db1-2560">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2560">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="60db1-2561">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2561">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="60db1-2562">最終的な変換は、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2562">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="60db1-2563">場所`x`アクセスは、それ以外の場合、コンパイラによって生成された識別子です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2563">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="60db1-2564">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2564">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="60db1-2565">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2565">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="60db1-2566">最終的な変換は、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2566">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="60db1-2567">場所`x`アクセスは、それ以外の場合、コンパイラによって生成された識別子です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2567">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="60db1-2568">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2568">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="60db1-2569">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2569">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="60db1-2570">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2570">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="60db1-2571">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2571">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="60db1-2572">最終的な変換は、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2572">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="60db1-2573">場所`x`と`y`は非表示とアクセスできない、それ以外の場合はコンパイラによって生成された識別子です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2573">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="60db1-2574">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2574">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="60db1-2575">最終的な変換には</span><span class="sxs-lookup"><span data-stu-id="60db1-2575">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="60db1-2576">Select 句</span><span class="sxs-lookup"><span data-stu-id="60db1-2576">Select clauses</span></span>

<span data-ttu-id="60db1-2577">形式のクエリ式</span><span class="sxs-lookup"><span data-stu-id="60db1-2577">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="60db1-2578">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2578">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="60db1-2579">v が識別子の場合を除き、x、翻訳は単に</span><span class="sxs-lookup"><span data-stu-id="60db1-2579">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="60db1-2580">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2580">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="60db1-2581">単に変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2581">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="60db1-2582">Groupby 句</span><span class="sxs-lookup"><span data-stu-id="60db1-2582">Groupby clauses</span></span>

<span data-ttu-id="60db1-2583">形式のクエリ式</span><span class="sxs-lookup"><span data-stu-id="60db1-2583">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="60db1-2584">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2584">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="60db1-2585">v が識別子の場合を除き、x、翻訳は、</span><span class="sxs-lookup"><span data-stu-id="60db1-2585">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="60db1-2586">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2586">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="60db1-2587">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2587">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="60db1-2588">透過的な識別子</span><span class="sxs-lookup"><span data-stu-id="60db1-2588">Transparent identifiers</span></span>

<span data-ttu-id="60db1-2589">特定の翻訳を持つ範囲変数を挿入する***透過的識別子***によって示される`*`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2589">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="60db1-2590">透過的な識別子は、適切な言語機能です。クエリ式の変換処理では中間の手順としてのみ存在するとします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2590">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="60db1-2591">クエリ変換では、透過的な識別子を挿入、ときに、変換の手順は匿名関数と匿名オブジェクト初期化子に透過識別子を伝達さらに。</span><span class="sxs-lookup"><span data-stu-id="60db1-2591">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="60db1-2592">これらのコンテキストでは、透過的な識別子は、次の動作をあります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2592">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="60db1-2593">透過識別子が、匿名関数にパラメーターとして発生したときに、関連付けられている匿名型のメンバーは自動的に匿名関数の本体のスコープ。</span><span class="sxs-lookup"><span data-stu-id="60db1-2593">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="60db1-2594">透過的な識別子を持つメンバーがスコープ内にある場合、そのメンバーのメンバーは、スコープもになります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2594">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="60db1-2595">透過的な識別子は、匿名オブジェクト初期化子のメンバー宣言子としてときに、透過的な識別子を持つメンバーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2595">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="60db1-2596">上記で説明した変換の手順で透過的な識別子は常に 1 つのオブジェクトのメンバーとして複数の範囲変数をキャプチャする目的で、匿名型と共に導入されました。</span><span class="sxs-lookup"><span data-stu-id="60db1-2596">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="60db1-2597">C# の実装は、匿名型よりも、別のメカニズムを使用して、複数の範囲変数をグループ化する許可されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2597">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="60db1-2598">次の変換の例には、匿名型を使用して透明度の識別子を表示することが前提としていますから変換できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2598">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="60db1-2599">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2599">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="60db1-2600">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2600">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="60db1-2601">これはさらに変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2601">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="60db1-2602">、透過的な識別子が消去されると、ときに、</span><span class="sxs-lookup"><span data-stu-id="60db1-2602">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="60db1-2603">場所`x`アクセスは、それ以外の場合、コンパイラによって生成された識別子です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2603">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="60db1-2604">例では、</span><span class="sxs-lookup"><span data-stu-id="60db1-2604">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="60db1-2605">変換されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2605">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="60db1-2606">これはさらに縮小</span><span class="sxs-lookup"><span data-stu-id="60db1-2606">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="60db1-2607">最終的な変換は、します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2607">the final translation of which is</span></span>
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
<span data-ttu-id="60db1-2608">場所`x`、 `y`、および`z`は非表示とアクセスできない、それ以外の場合はコンパイラによって生成された識別子です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2608">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="60db1-2609">クエリ式パターン</span><span class="sxs-lookup"><span data-stu-id="60db1-2609">The query expression pattern</span></span>

<span data-ttu-id="60db1-2610">***クエリ式パターン***型は、クエリ式をサポートするために実装できるメソッドのパターンを確立します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2610">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="60db1-2611">クエリ式は、構文のマップを使用して、メソッドの呼び出しに変換は、ために、かなり柔軟にクエリ式パターンを実装する方法がある型。</span><span class="sxs-lookup"><span data-stu-id="60db1-2611">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="60db1-2612">たとえば、パターンのメソッド実装できますインスタンス メソッド、または拡張メソッドとして、2 つが、同じ呼び出し構文があるため、匿名関数は両方に変換するために、メソッドがデリゲートまたは式ツリーを要求できます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2612">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="60db1-2613">ジェネリック型の推奨される図形`C<T>`をサポートする、クエリ式パターンを次に示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2613">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="60db1-2614">ジェネリック型は、パラメーターと結果の型の間で、適切な関係を説明するために使用されますが、非ジェネリック型に同様のパターンを実装することは。</span><span class="sxs-lookup"><span data-stu-id="60db1-2614">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

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

<span data-ttu-id="60db1-2615">上記のメソッドを使用して、汎用デリゲート型`Func<T1,R>`と`Func<T1,T2,R>`が、でした同様が他のデリゲートまたは式ツリー型と共に使用パラメーターと結果の種類で同じリレーションシップ。</span><span class="sxs-lookup"><span data-stu-id="60db1-2615">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="60db1-2616">推奨の関係に注意してください`C<T>`と`O<T>`を確実、`ThenBy`と`ThenByDescending`メソッドがの結果でのみ使用できますが、`OrderBy`または`OrderByDescending`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2616">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="60db1-2617">また、推奨される図形の結果の`GroupBy`--各内部シーケンスに追加して、シーケンスのシーケンス`Key`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="60db1-2617">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="60db1-2618">`System.Linq`名前空間を実装する任意の型のクエリ演算子のパターンの実装を提供する、`System.Collections.Generic.IEnumerable<T>`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="60db1-2618">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="60db1-2619">代入演算子</span><span class="sxs-lookup"><span data-stu-id="60db1-2619">Assignment operators</span></span>

<span data-ttu-id="60db1-2620">代入演算子は、変数、プロパティ、イベント、またはインデクサーの要素を新しい値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2620">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

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

<span data-ttu-id="60db1-2621">代入式の左側のオペランドは、変数、プロパティ アクセス、インデクサー アクセス、またはイベント アクセスとして分類される式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2621">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="60db1-2622">`=`演算子が呼び出される、***単純代入演算子***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2622">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="60db1-2623">左側のオペランドで指定されている変数、プロパティ、またはインデクサーの要素に右側のオペランドの値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2623">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="60db1-2624">単純な代入演算子の左側のオペランドをイベント アクセスできない可能性があります (」に記載されていない限り[フィールドのようなイベント](classes.md#field-like-events))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2624">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="60db1-2625">単純な代入演算子が記載[単純な代入](expressions.md#simple-assignment)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2625">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="60db1-2626">以外の代入演算子、`=`演算子を呼び出す、***複合代入演算子***します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2626">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="60db1-2627">これらの演算子は、2 つのオペランドで指定された操作を実行し、左側のオペランドで指定されている変数、プロパティ、またはインデクサーの要素に、結果の値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2627">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="60db1-2628">複合代入演算子が記載されて[複合代入](expressions.md#compound-assignment)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2628">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="60db1-2629">`+=`と`-=`イベントへのアクセス式で、左側のオペランドと演算子が呼び出されます、*イベント代入演算子*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2629">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="60db1-2630">その他の代入演算子はありません、イベントのアクセス権を持つ有効な左側のオペランドとして。</span><span class="sxs-lookup"><span data-stu-id="60db1-2630">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="60db1-2631">イベントの代入演算子が記載されて[イベント割り当て](expressions.md#event-assignment)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2631">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="60db1-2632">代入演算子が右から左の操作が右から左にまとめられていることを意味します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2632">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="60db1-2633">たとえば、フォームの式`a = b = c`として評価されます`a = (b = c)`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2633">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="60db1-2634">単純代入</span><span class="sxs-lookup"><span data-stu-id="60db1-2634">Simple assignment</span></span>

<span data-ttu-id="60db1-2635">`=`演算子は、単純な代入演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2635">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="60db1-2636">フォームの単純な代入の左辺のオペランドが`E.P`または`E[Ei]`場所`E`コンパイル時の型を持つ`dynamic`、割り当てが動的にバインドし、([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2636">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="60db1-2637">この場合、代入式のコンパイル時の型は`dynamic`の実行時の型に基づいて実行時に以下に示す解決が行わ`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2637">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="60db1-2638">単純な代入では、右側のオペランドが左のオペランドの型に暗黙的に変換する式必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2638">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="60db1-2639">操作は、左側のオペランドで指定されている変数、プロパティ、またはインデクサーの要素を右側のオペランドの値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2639">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="60db1-2640">単純な代入式の結果は、左側のオペランドに割り当てられている値です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2640">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="60db1-2641">結果は、左オペランドと同じ型を備え、値として常に分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2641">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="60db1-2642">左側のオペランドがプロパティまたはインデクサーのアクセスの場合は、プロパティまたはインデクサーがいる必要があります、`set`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-2642">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="60db1-2643">サポートしていない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2643">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-2644">フォームの単純な割り当ての実行時の処理`x = y`次の手順で構成されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2644">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="60db1-2645">場合`x`変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2645">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="60db1-2646">`x` 変数を生成するために評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2646">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="60db1-2647">`y` 評価して、必要な場合、変換の型に`x`暗黙的な変換を行って ([暗黙的な変換](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2647">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="60db1-2648">場合によって指定される変数`x`の配列要素である、 *reference_type*の値を計算することを確認する実行時チェックが行われます`y`うち配列インスタンスとの互換性は`x`が、要素。</span><span class="sxs-lookup"><span data-stu-id="60db1-2648">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="60db1-2649">場合、チェックは成功`y`は`null`、暗黙の参照変換の場合、または ([暗黙の参照変換](conversions.md#implicit-reference-conversions)) 参照しているインスタンスの実際の型からが存在する`y`実際の要素の型に含んでいる配列インスタンスの`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2649">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="60db1-2650">それ以外の場合は、`System.ArrayTypeMismatchException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2650">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="60db1-2651">評価との変換から得られた値`y`の評価によって指定された場所に格納されます`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2651">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="60db1-2652">場合`x`プロパティまたはインデクサーのアクセスに分類されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2652">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="60db1-2653">インスタンス式 (場合`x`ない`static`) および引数リスト (場合`x`インデクサー アクセス) に関連付けられている`x`が評価されると、その後で、結果の使用`set`アクセサーの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="60db1-2653">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="60db1-2654">`y` 評価して、必要な場合、変換の型に`x`暗黙的な変換を行って ([暗黙的な変換](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2654">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="60db1-2655">`set`のアクセサー`x`に対して計算された値で呼び出される`y`としてその`value`引数。</span><span class="sxs-lookup"><span data-stu-id="60db1-2655">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="60db1-2656">配列の共変性規則 ([配列の共変性](arrays.md#array-covariance)) 配列型の値を許可`A[]`配列型のインスタンスへの参照である`B[]`から暗黙の参照変換が存在する限り、`B`に`A`.</span><span class="sxs-lookup"><span data-stu-id="60db1-2656">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="60db1-2657">これらの規則の配列要素への割り当てのため、 *reference_type*割り当てられている値が配列のインスタンスと互換性があることを確認する実行時チェックが必要です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2657">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="60db1-2658">例</span><span class="sxs-lookup"><span data-stu-id="60db1-2658">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="60db1-2659">最後の割り当てにより、`System.ArrayTypeMismatchException`がスローされるインスタンスの`ArrayList`の要素に格納することはできません、 `string[]`。</span><span class="sxs-lookup"><span data-stu-id="60db1-2659">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="60db1-2660">プロパティまたはインデクサーを宣言で、 *struct_type*インスタンス式を代入式のターゲットに関連付けられているプロパティまたはインデクサー アクセスは、変数として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2660">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="60db1-2661">インスタンス式が値として分類は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2661">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="60db1-2662">ため[メンバー アクセス](expressions.md#member-access)、同じルールは、フィールドにも適用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2662">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="60db1-2663">宣言があるとします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2663">Given the declarations:</span></span>
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
<span data-ttu-id="60db1-2664">例</span><span class="sxs-lookup"><span data-stu-id="60db1-2664">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="60db1-2665">割り当て`p.X`、 `p.Y`、 `r.A`、および`r.B`ので許可されます`p`と`r`変数します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2665">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="60db1-2666">ただしの例</span><span class="sxs-lookup"><span data-stu-id="60db1-2666">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="60db1-2667">割り当ては、以降、すべての無効な`r.A`と`r.B`変数ではないです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2667">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="60db1-2668">複合代入。</span><span class="sxs-lookup"><span data-stu-id="60db1-2668">Compound assignment</span></span>

<span data-ttu-id="60db1-2669">フォームの複合代入の左辺のオペランドが`E.P`または`E[Ei]`場所`E`コンパイル時の型を持つ`dynamic`、割り当てが動的にバインドし、([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2669">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="60db1-2670">この場合、代入式のコンパイル時の型は`dynamic`の実行時の型に基づいて実行時に以下に示す解決が行わ`E`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2670">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="60db1-2671">フォームの操作を`x op= y`二項演算子のオーバー ロードの解決を適用することによって処理されます ([二項演算子のオーバー ロードの解決](expressions.md#binary-operator-overload-resolution))、操作が書き込まれた場合、`x op y`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2671">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="60db1-2672">そうしたら</span><span class="sxs-lookup"><span data-stu-id="60db1-2672">Then,</span></span>

*  <span data-ttu-id="60db1-2673">選択した演算子の戻り値の型がの型に暗黙的に変換できる場合は`x`、操作として評価されます`x = x op y`ことを除いて、`x`は 1 回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2673">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="60db1-2674">それ以外の場合、選択した演算子の定義済みの演算子の型に明示的に変換できる場合は、選択した演算子の戻り値の型場合`x`、場合`y`の型に暗黙的に変換できる`x`または演算子が、演算子をシフトとして、操作が評価されます`x = (T)(x op y)`ここで、`T`の型である`x`ことを除いて、`x`は 1 回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2674">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="60db1-2675">それ以外の場合、複合代入が有効でないと、バインド エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2675">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-2676">「1 回だけ評価」という用語では、ことを意味の評価で`x op y`の構成要素である式の結果`x`一時的に保存されからへの代入を実行するときに再使用`x`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2676">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="60db1-2677">割り当ての例では、`A()[B()] += C()`ここで、`A`を返すメソッドは、 `int[]`、および`B`と`C`を返すメソッド`int`、メソッドは、順序で 1 回だけ呼び出されます`A`、`B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="60db1-2677">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="60db1-2678">プロパティまたはインデクサーが、両方にいる必要があります複合代入の左辺のオペランドは、プロパティ アクセスまたはインデクサーのアクセスには、ときに、`get`アクセサーと`set`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="60db1-2678">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="60db1-2679">サポートしていない場合は、バインド時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2679">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-2680">前記の 2 番目のルール`x op= y`として評価される`x = (T)(x op y)`特定のコンテキストでします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2680">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="60db1-2681">定義済みの演算子の左側のオペランドの型はときに、複合演算子として使用できるように、ルールが存在する`sbyte`、 `byte`、 `short`、 `ushort`、または`char`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2681">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="60db1-2682">両方の引数がこれらの型のいずれかの場合でも表示、定義済みの演算子で型の結果を生成`int`」の説明に従って、[バイナリ数値プロモーション](expressions.md#binary-numeric-promotions)します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2682">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="60db1-2683">したがってをキャストなしことはできません、左側のオペランドに結果を代入すること。</span><span class="sxs-lookup"><span data-stu-id="60db1-2683">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="60db1-2684">定義済みの演算子のルールの直感的な効果には単には、`x op= y`両方が許可されているの`x op y`と`x = y`が許可されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2684">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="60db1-2685">例</span><span class="sxs-lookup"><span data-stu-id="60db1-2685">In the example</span></span>
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
<span data-ttu-id="60db1-2686">各エラーの直感的な理由は、対応する単純な代入もされていること、エラーです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2686">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="60db1-2687">これは、複合代入演算が操作を持ち上げることも意味します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2687">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="60db1-2688">例</span><span class="sxs-lookup"><span data-stu-id="60db1-2688">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="60db1-2689">リフトされた演算子`+(int?,int?)`使用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2689">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="60db1-2690">イベントの割り当て</span><span class="sxs-lookup"><span data-stu-id="60db1-2690">Event assignment</span></span>

<span data-ttu-id="60db1-2691">場合の左オペランド、`+=`または`-=`演算子は、イベント アクセスとして分類し、次のように、式が評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2691">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="60db1-2692">インスタンス式では、存在する場合、イベントへのアクセスの評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2692">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="60db1-2693">右オペランド、`+=`または`-=`演算子が評価され、必要な場合は、暗黙的な変換を左側のオペランドの型に変換 ([暗黙的な変換](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2693">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="60db1-2694">右側のオペランドを評価した後で構成される引数リストで、イベントのイベント アクセサーが呼び出されると、必要に応じて、変換します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2694">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="60db1-2695">演算子が場合`+=`、`add`アクセサーが呼び出されます。 演算子が場合`-=`、`remove`アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2695">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="60db1-2696">イベントの代入式では、値を生成しません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2696">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="60db1-2697">したがって、イベントの代入式のコンテキストでのみ有効ですが、 *statement_expression* ([式ステートメント](statements.md#expression-statements))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2697">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="60db1-2698">正規表現</span><span class="sxs-lookup"><span data-stu-id="60db1-2698">Expression</span></span>

<span data-ttu-id="60db1-2699">*式*か、 *non_assignment_expression*または*割り当て*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2699">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

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

## <a name="constant-expressions"></a><span data-ttu-id="60db1-2700">定数式</span><span class="sxs-lookup"><span data-stu-id="60db1-2700">Constant expressions</span></span>

<span data-ttu-id="60db1-2701">A *constant_expression*コンパイル時に完全に評価される式を指定します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2701">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="60db1-2702">定数式である必要があります、`null`リテラル値または、次の種類のいずれかの値: `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、 `decimal`、 `bool`、 `object`、 `string`、または列挙型。</span><span class="sxs-lookup"><span data-stu-id="60db1-2702">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="60db1-2703">定数式で、次の構成要素のみが許可されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2703">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="60db1-2704">リテラル (など、`null`リテラル) です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2704">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="60db1-2705">参照`const`クラスと構造体の型のメンバー。</span><span class="sxs-lookup"><span data-stu-id="60db1-2705">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="60db1-2706">列挙型のメンバーへの参照。</span><span class="sxs-lookup"><span data-stu-id="60db1-2706">References to members of enumeration types.</span></span>
*  <span data-ttu-id="60db1-2707">参照`const`パラメーターまたはローカル変数</span><span class="sxs-lookup"><span data-stu-id="60db1-2707">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="60db1-2708">かっこで囲まれたサブ式自体は定数式です。</span><span class="sxs-lookup"><span data-stu-id="60db1-2708">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="60db1-2709">指定されたターゲット型のキャスト式では、上記の型の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2709">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="60db1-2710">`checked` `unchecked`式</span><span class="sxs-lookup"><span data-stu-id="60db1-2710">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="60db1-2711">既定値の式</span><span class="sxs-lookup"><span data-stu-id="60db1-2711">Default value expressions</span></span>
*  <span data-ttu-id="60db1-2712">Nameof 式</span><span class="sxs-lookup"><span data-stu-id="60db1-2712">Nameof expressions</span></span>
*  <span data-ttu-id="60db1-2713">定義済み`+`、 `-`、 `!`、および`~`単項演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-2713">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="60db1-2714">定義済み`+`、 `-`、 `*`、 `/`、 `%`、 `<<`、 `>>`、 `&`、 `|`、 `^`、 `&&`、 `||`、 `==`、 `!=`、 `<`、 `>`、 `<=`、および`>=`上に示した型の各オペランドは、二項の演算子が提供されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2714">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="60db1-2715">`?:`条件演算子。</span><span class="sxs-lookup"><span data-stu-id="60db1-2715">The `?:` conditional operator.</span></span>

<span data-ttu-id="60db1-2716">次の変換は、定数式で許可されています。</span><span class="sxs-lookup"><span data-stu-id="60db1-2716">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="60db1-2717">恒等変換</span><span class="sxs-lookup"><span data-stu-id="60db1-2717">Identity conversions</span></span>
*  <span data-ttu-id="60db1-2718">数値変換</span><span class="sxs-lookup"><span data-stu-id="60db1-2718">Numeric conversions</span></span>
*  <span data-ttu-id="60db1-2719">列挙型の変換</span><span class="sxs-lookup"><span data-stu-id="60db1-2719">Enumeration conversions</span></span>
*  <span data-ttu-id="60db1-2720">定数式の変換</span><span class="sxs-lookup"><span data-stu-id="60db1-2720">Constant expression conversions</span></span>
*  <span data-ttu-id="60db1-2721">暗黙的および明示的な参照変換、変換のソースが null の値に評価される定数式であることを表示します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2721">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="60db1-2722">その他の変換では、ボックス化を含む、null 以外の値のボックス化解除と暗黙の参照変換は定数式で許可されていません。</span><span class="sxs-lookup"><span data-stu-id="60db1-2722">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="60db1-2723">例:</span><span class="sxs-lookup"><span data-stu-id="60db1-2723">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="60db1-2724">初期化のボックス化変換が必要なため、i は、エラーです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2724">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="60db1-2725">Str の初期化は、null 以外の値から暗黙の参照変換が必要になるために、エラーです。</span><span class="sxs-lookup"><span data-stu-id="60db1-2725">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="60db1-2726">式は、上記の要件を満たして、ときに、式はコンパイル時に評価されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2726">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="60db1-2727">これは、式が非定数の構成要素を含むより大きな式のサブ式である場合でも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="60db1-2727">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="60db1-2728">定数式のコンパイル時の評価は、実行時の評価をスローすると、例外、コンパイル時の評価によって発生するコンパイル時エラーが発生する点を除いて、非定数式の評価を実行時と同じ規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2728">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="60db1-2729">定数式を明示的に配置しない限り、`unchecked`コンパイル時エラーが発生するコンテキストには、常に式のコンパイル時の評価時に整数型の算術演算および変換で発生するオーバーフロー ([定数式](expressions.md#constant-expressions))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2729">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="60db1-2730">定数式は、以下のコンテキストで発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2730">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="60db1-2731">これらのコンテキストでは、コンパイル時エラーは、式はコンパイル時に完全に評価できない場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2731">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="60db1-2732">定数宣言 ([定数](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2732">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="60db1-2733">列挙体メンバーの宣言 ([列挙型メンバー](enums.md#enum-members))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2733">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="60db1-2734">既定の仮パラメーター リストの引数 ([メソッド パラメーター](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="60db1-2734">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="60db1-2735">`case` ラベルを`switch`ステートメント ([switch ステートメント](statements.md#the-switch-statement))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2735">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="60db1-2736">`goto case` ステートメント ([goto ステートメント](statements.md#the-goto-statement))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2736">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="60db1-2737">次元の配列作成式の長さ ([配列作成式](expressions.md#array-creation-expressions)) 初期化子が含まれます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2737">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="60db1-2738">属性 ([属性](attributes.md))。</span><span class="sxs-lookup"><span data-stu-id="60db1-2738">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="60db1-2739">定数式が暗黙的な変換を ([定数式が暗黙的な変換](conversions.md#implicit-constant-expression-conversions)) により、型の定数式`int`に変換する`sbyte`、 `byte`、 `short`、 `ushort`、 `uint`、または`ulong`定数式の値が変換先の型の範囲内に提供します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2739">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="60db1-2740">ブール式</span><span class="sxs-lookup"><span data-stu-id="60db1-2740">Boolean expressions</span></span>

<span data-ttu-id="60db1-2741">A *boolean_expression*型の結果を生成する式は、`bool`いずれかのアプリケーションまたは直接`operator true`以下で指定されている特定のコンテキストでします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2741">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="60db1-2742">制御の条件付きの式、 *if_statement* ([if ステートメント](statements.md#the-if-statement))、 *while_statement* ([、while ステートメント](statements.md#the-while-statement))、*do_statement* ([do ステートメント](statements.md#the-do-statement))、または*for_statement* ([、ステートメントの](statements.md#the-for-statement)) は、 *boolean_式*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2742">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="60db1-2743">制御の条件付きの式、`?:`演算子 ([条件演算子](expressions.md#conditional-operator)) と同じ規則に従います、 *boolean_expression*演算子の上の理由から、優先順位が分類されますが、として、 *conditional_or_expression*します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2743">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="60db1-2744">A *boolean_expression* `E`型の値を生成するためにできるようにするために必要な`bool`、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="60db1-2744">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="60db1-2745">場合`E`暗黙的に変換できる`bool`実行時に暗黙的な変換が適用されます。</span><span class="sxs-lookup"><span data-stu-id="60db1-2745">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="60db1-2746">それ以外の場合、単項演算子のオーバー ロード解決 ([単項演算子のオーバー ロードの解決](expressions.md#unary-operator-overload-resolution)) 演算子の一意の最適な実装を検索するために使用`true`で`E`、その実装は実行時に適用します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2746">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="60db1-2747">このような演算子が検出されない場合、バインド時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2747">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="60db1-2748">`DBBool`で構造体型[ブール型のデータベース](structs.md#database-boolean-type)を実装する型の例を示します`operator true`と`operator false`します。</span><span class="sxs-lookup"><span data-stu-id="60db1-2748">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
