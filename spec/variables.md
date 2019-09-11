---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876797"
---
# <a name="variables"></a><span data-ttu-id="18b90-101">変数</span><span class="sxs-lookup"><span data-stu-id="18b90-101">Variables</span></span>

<span data-ttu-id="18b90-102">変数は、ストレージの場所を表します。</span><span class="sxs-lookup"><span data-stu-id="18b90-102">Variables represent storage locations.</span></span> <span data-ttu-id="18b90-103">すべての変数には、変数に格納できる値を決定する型があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="18b90-104">C#はタイプセーフな言語であり、コンパイラC#は変数に格納されている値が常に適切な型であることを保証します。</span><span class="sxs-lookup"><span data-stu-id="18b90-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="18b90-105">変数の値は、代入によって、または演算子`++`と`--`演算子を使用して変更できます。</span><span class="sxs-lookup"><span data-stu-id="18b90-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="18b90-106">値を取得する前に、変数を***確実***に代入 ([明確な代入](variables.md#definite-assignment)) する必要があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="18b90-107">以下のセクションで説明するように、変数は***最初に割り当てら***れるか、***最初***に割り当てが解除されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="18b90-108">最初に割り当てられた変数には、明確に定義された初期値があり、常に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="18b90-109">初期値が割り当てられていない変数には初期値がありません。</span><span class="sxs-lookup"><span data-stu-id="18b90-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="18b90-110">最初に割り当てられていない変数を特定の場所で確実に割り当てられるようにするには、その場所に至る可能性のあるすべての実行パスで、変数への代入を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="18b90-111">変数のカテゴリ</span><span class="sxs-lookup"><span data-stu-id="18b90-111">Variable categories</span></span>

<span data-ttu-id="18b90-112">C#変数の7つのカテゴリ (静的変数、インスタンス変数、配列要素、値パラメーター、参照パラメーター、出力パラメーター、およびローカル変数) を定義します。</span><span class="sxs-lookup"><span data-stu-id="18b90-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="18b90-113">以下のセクションでは、これらの各カテゴリについて説明します。</span><span class="sxs-lookup"><span data-stu-id="18b90-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="18b90-114">この例では、</span><span class="sxs-lookup"><span data-stu-id="18b90-114">In the example</span></span>
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
<span data-ttu-id="18b90-115">`x`は静的`y`変数、はインスタンス`v[0]`変数、は配列要素`a` 、は`c`値パラメーター `b` 、は参照パラメーター、は出力パラメーター `i` 、はローカル変数です.</span><span class="sxs-lookup"><span data-stu-id="18b90-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="18b90-116">静的変数</span><span class="sxs-lookup"><span data-stu-id="18b90-116">Static variables</span></span>

<span data-ttu-id="18b90-117">`static`修飾子を使用して宣言されたフィールドは、***静的変数***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="18b90-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="18b90-118">静的変数は、それを含んでいる型に対して静的コンストラクター ([静的](classes.md#static-constructors)コンストラクター) を実行する前に存在し、関連付けられているアプリケーションドメインが存在しなくなったときには存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="18b90-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="18b90-119">静的変数の初期値は、変数の型の既定値 ([既定](variables.md#default-values)値) です。</span><span class="sxs-lookup"><span data-stu-id="18b90-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="18b90-120">確実な代入のチェックのために、静的変数は最初に割り当てられたものと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="18b90-121">インスタンス変数</span><span class="sxs-lookup"><span data-stu-id="18b90-121">Instance variables</span></span>

<span data-ttu-id="18b90-122">修飾子を`static`指定せずに宣言されたフィールドは、***インスタンス変数***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="18b90-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="18b90-123">クラスのインスタンス変数</span><span class="sxs-lookup"><span data-stu-id="18b90-123">Instance variables in classes</span></span>

<span data-ttu-id="18b90-124">クラスのインスタンス変数は、そのクラスの新しいインスタンスが作成されるときに存在し、そのインスタンスへの参照がなく、インスタンスのデストラクター (存在する場合) が実行されたときに存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="18b90-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="18b90-125">クラスのインスタンス変数の初期値は、変数の型の既定値 ([既定](variables.md#default-values)値) です。</span><span class="sxs-lookup"><span data-stu-id="18b90-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="18b90-126">明確な代入を確認するために、クラスのインスタンス変数は最初に割り当てられたものと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="18b90-127">構造体のインスタンス変数</span><span class="sxs-lookup"><span data-stu-id="18b90-127">Instance variables in structs</span></span>

<span data-ttu-id="18b90-128">構造体のインスタンス変数の有効期間は、それが属する構造体変数とまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="18b90-129">つまり、構造体型の変数が存在する場合、または存在しなくなった場合は、構造体のインスタンス変数を取得します。</span><span class="sxs-lookup"><span data-stu-id="18b90-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="18b90-130">構造体のインスタンス変数の初期割り当ての状態は、それを含んでいる構造体の変数と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="18b90-131">つまり、構造体変数が最初に割り当てられていると見なされると、そのインスタンス変数もあります。また、struct 変数が最初に割り当てられていないと見なされると、そのインスタンス変数は同様に割り当て解除されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="18b90-132">配列要素</span><span class="sxs-lookup"><span data-stu-id="18b90-132">Array elements</span></span>

<span data-ttu-id="18b90-133">配列の要素は、配列インスタンスの作成時に存在し、その配列インスタンスへの参照がない場合は存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="18b90-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="18b90-134">配列の各要素の初期値は、配列要素の型の既定値 ([既定](variables.md#default-values)値) です。</span><span class="sxs-lookup"><span data-stu-id="18b90-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="18b90-135">明確な割り当てチェックのために、配列要素は最初に割り当てられたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="18b90-136">値パラメーター</span><span class="sxs-lookup"><span data-stu-id="18b90-136">Value parameters</span></span>

<span data-ttu-id="18b90-137">または`ref` `out`修飾子を指定せずに宣言されたパラメーターは、***値パラメーター***です。</span><span class="sxs-lookup"><span data-stu-id="18b90-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="18b90-138">値パラメーターは、関数メンバー (メソッド、インスタンスコンストラクター、アクセサー、または演算子) の呼び出し時、またはパラメーターが属する匿名関数の呼び出し時に存在し、呼び出しで指定された引数の値で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="18b90-139">通常、値パラメーターは、関数メンバーまたは匿名関数を返したときに存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="18b90-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="18b90-140">ただし、値パラメーターが匿名関数 ([匿名関数式](expressions.md#anonymous-function-expressions)) によってキャプチャされる場合、その匿名関数から作成されたデリゲートまたは式ツリーがガベージコレクションの対象になるまで、その有効期間は少なくともになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="18b90-141">明確な割り当てチェックのために、値パラメーターは最初に割り当てられたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="18b90-142">参照パラメーター</span><span class="sxs-lookup"><span data-stu-id="18b90-142">Reference parameters</span></span>

<span data-ttu-id="18b90-143">修飾子を`ref`使用して宣言されたパラメーターは***参照パラメーター***です。</span><span class="sxs-lookup"><span data-stu-id="18b90-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="18b90-144">参照パラメーターでは、新しいストレージの場所は作成されません。</span><span class="sxs-lookup"><span data-stu-id="18b90-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="18b90-145">代わりに、参照パラメーターは、関数メンバーまたは匿名関数呼び出しで引数として指定された変数と同じ格納場所を表します。</span><span class="sxs-lookup"><span data-stu-id="18b90-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="18b90-146">したがって、参照パラメーターの値は、常に基になる変数と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="18b90-147">参照パラメーターには、次の明確な代入規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="18b90-148">「[出力パラメーター](variables.md#output-parameters)」で説明されている出力パラメーターのさまざまな規則に注意してください。</span><span class="sxs-lookup"><span data-stu-id="18b90-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="18b90-149">変数は、関数メンバーまたはデリゲート呼び出しで参照パラメーターとして渡す前に、確実に代入 ([明確な代入](variables.md#definite-assignment)) する必要があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="18b90-150">関数メンバーまたは匿名関数内では、参照パラメーターは最初に割り当てられたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="18b90-151">インスタンスメソッドまたは構造体型のインスタンスアクセサー内では`this` 、キーワードは構造体型の参照パラメーターとまったく同じように動作します ([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="18b90-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="18b90-152">出力パラメーター</span><span class="sxs-lookup"><span data-stu-id="18b90-152">Output parameters</span></span>

<span data-ttu-id="18b90-153">`out`修飾子を使用して宣言されたパラメーターは、***出力パラメーター***です。</span><span class="sxs-lookup"><span data-stu-id="18b90-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="18b90-154">出力パラメーターでは、新しいストレージの場所は作成されません。</span><span class="sxs-lookup"><span data-stu-id="18b90-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="18b90-155">代わりに、出力パラメーターは、関数メンバーまたはデリゲート呼び出しの引数として指定された変数と同じ格納場所を表します。</span><span class="sxs-lookup"><span data-stu-id="18b90-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="18b90-156">したがって、出力パラメーターの値は、常に基になる変数と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="18b90-157">出力パラメーターには、次の明確な代入規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="18b90-158">参照パラメーターについては、「[参照パラメーター](variables.md#reference-parameters)」で説明しているさまざまな規則に注意してください。</span><span class="sxs-lookup"><span data-stu-id="18b90-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="18b90-159">変数は、関数メンバーまたはデリゲート呼び出しの出力パラメーターとして渡す前に、確実に割り当てる必要はありません。</span><span class="sxs-lookup"><span data-stu-id="18b90-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="18b90-160">関数メンバーの通常の完了またはデリゲート呼び出しの後に、出力パラメーターとして渡された各変数は、その実行パスで割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="18b90-161">関数メンバーまたは匿名関数内では、出力パラメーターは最初に割り当てられていないと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="18b90-162">関数メンバーまたは匿名関数のすべての出力パラメーターは、関数メンバーまたは匿名関数が正常に返される前に、確実に代入される必要があります ([明確な代入](variables.md#definite-assignment))。</span><span class="sxs-lookup"><span data-stu-id="18b90-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="18b90-163">構造体型のインスタンスコンストラクター内では、 `this`キーワードは構造体型の出力パラメーターとして正確に動作します ([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="18b90-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="18b90-164">ローカル変数</span><span class="sxs-lookup"><span data-stu-id="18b90-164">Local variables</span></span>

<span data-ttu-id="18b90-165">***ローカル変数***は、 *local_variable_declaration*によって宣言されます。これは、*ブロック*、 *for_statement*、 *switch_statement* 、または*using_statement*で発生する可能性があります。または、 *foreach_statement*または*try_statement*の*specific_catch_clause* 。</span><span class="sxs-lookup"><span data-stu-id="18b90-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="18b90-166">ローカル変数の有効期間は、ストレージが予約されていることが保証されるプログラム実行の部分です。</span><span class="sxs-lookup"><span data-stu-id="18b90-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="18b90-167">この有効期間は、少なくともの entry から、それが関連付けられている*block*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*にまで及びます。その*block*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*の実行は、どのような形で終了します。</span><span class="sxs-lookup"><span data-stu-id="18b90-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="18b90-168">(囲まれた*ブロック*を入力するか、メソッドを呼び出すと、現在の*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または specific_ の実行が中断されますが、終了することはありません。 *catch_clause*)ローカル変数が匿名関数 (キャプチャされた[外部変数](expressions.md#captured-outer-variables)) によってキャプチャされる場合、その有効期間は、匿名関数から作成されたデリゲートまたは式ツリーと、キャプチャされた変数は、ガベージコレクションの対象になります。</span><span class="sxs-lookup"><span data-stu-id="18b90-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="18b90-169">親*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*が再帰的に入力されると、ローカル変数の新しいインスタンスが作成されます。時間とその*local_variable_initializer*がある場合は、毎回評価されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="18b90-170">*Local_variable_declaration*によって導入されるローカル変数は自動的に初期化されないため、既定値はありません。</span><span class="sxs-lookup"><span data-stu-id="18b90-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="18b90-171">明確な割り当てチェックの目的で、 *local_variable_declaration*によって導入されたローカル変数は、最初に未割り当てと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="18b90-172">*Local_variable_declaration*には、 *local_variable_initializer*を含めることができます。この場合、変数は初期化式 ([宣言ステートメント](variables.md#declaration-statements)) の後にのみ確実に割り当てられたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="18b90-173">*Local_variable_declaration*によって導入されるローカル変数のスコープ内では、そのローカル変数を*local_variable_declarator*の前にあるテキスト位置で参照するコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="18b90-174">ローカル変数宣言が暗黙的 ([ローカル変数宣言](statements.md#local-variable-declarations)) である場合は、 *local_variable_declarator*内で変数を参照することもエラーになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="18b90-175">*Foreach_statement*または*specific_catch_clause*によって導入されたローカル変数は、そのスコープ全体で確実に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="18b90-176">ローカル変数の実際の有効期間は実装に依存します。</span><span class="sxs-lookup"><span data-stu-id="18b90-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="18b90-177">たとえば、コンパイラは、ブロック内のローカル変数が、そのブロックの一部にのみ使用されることを静的に判断する場合があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="18b90-178">この分析を使用すると、コンパイラは、変数のストレージの有効期間が、それを含んでいるブロックよりも短くなるようなコードを生成できます。</span><span class="sxs-lookup"><span data-stu-id="18b90-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="18b90-179">ローカル参照変数によって参照されるストレージは、ローカル参照変数の有効期間 ([自動メモリ管理](basic-concepts.md#automatic-memory-management)) とは無関係に再利用されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="18b90-180">既定の値</span><span class="sxs-lookup"><span data-stu-id="18b90-180">Default values</span></span>

<span data-ttu-id="18b90-181">次のカテゴリの変数は、自動的に既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="18b90-182">静的変数</span><span class="sxs-lookup"><span data-stu-id="18b90-182">Static variables.</span></span>
*  <span data-ttu-id="18b90-183">クラスインスタンスのインスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="18b90-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="18b90-184">配列要素。</span><span class="sxs-lookup"><span data-stu-id="18b90-184">Array elements.</span></span>

<span data-ttu-id="18b90-185">変数の既定値は、変数の型によって異なり、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="18b90-186">*Value_type*の変数の既定値は、 *value_type*の既定のコンストラクター ([既定のコンストラクター](types.md#default-constructors)) によって計算された値と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="18b90-187">*Reference_type*の変数の場合、既定値は`null`です。</span><span class="sxs-lookup"><span data-stu-id="18b90-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="18b90-188">既定値への初期化は、通常、メモリマネージャーまたはガベージコレクターが、使用のために割り当てられる前にすべてのビットゼロにメモリを初期化することによって行われます。</span><span class="sxs-lookup"><span data-stu-id="18b90-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="18b90-189">このため、null 参照を表すためにすべて-bit-0 を使用すると便利です。</span><span class="sxs-lookup"><span data-stu-id="18b90-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="18b90-190">明確な代入</span><span class="sxs-lookup"><span data-stu-id="18b90-190">Definite assignment</span></span>

<span data-ttu-id="18b90-191">関数メンバーの実行可能コード内の特定の場所では、コンパイラが特定の静的フロー分析 ([明確な代入を決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) によって、変数が確実に代入される場合、変数は***確実に割り当てら***れると言われます。が自動的に初期化されたか、少なくとも1つの割り当ての対象になっています。</span><span class="sxs-lookup"><span data-stu-id="18b90-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="18b90-192">非公式に言うと、明確な割り当ての規則は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="18b90-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="18b90-193">最初に割り当てられた変数 ([最初に割り当てられた変数](variables.md#initially-assigned-variables)) は、常に代入されていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="18b90-194">最初に割り当てられていない変数 ([最初に割り当て](variables.md#initially-unassigned-variables)られていない変数) は、その場所を指すすべての実行パスに次のうち少なくとも1つが含まれている場合、特定の場所で確実に割り当てられたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="18b90-195">単純な代入 ([単純な代入](expressions.md#simple-assignment))。変数は左オペランドです。</span><span class="sxs-lookup"><span data-stu-id="18b90-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="18b90-196">出力パラメーターとして変数を渡す呼び出し式 ([呼び出し式](expressions.md#invocation-expressions)) またはオブジェクト作成式 ([オブジェクト作成式](expressions.md#object-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="18b90-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="18b90-197">ローカル変数の場合は、変数初期化子を含むローカル変数宣言 ([ローカル変数宣言](statements.md#local-variable-declarations))。</span><span class="sxs-lookup"><span data-stu-id="18b90-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="18b90-198">上記の非公式な規則の基になる正式な仕様については、[最初に割り当てら](variables.md#initially-assigned-variables)れた変数、[初期未](variables.md#initially-unassigned-variables)割り当ての変数、[明確な割り当てを決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)について説明します。</span><span class="sxs-lookup"><span data-stu-id="18b90-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="18b90-199">*Struct_type*変数のインスタンス変数の明示的な割り当ての状態は、まとめて、個別に追跡されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="18b90-200">上記の規則に加えて、 *struct_type*変数とそのインスタンス変数には次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-200">In addition to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="18b90-201">インスタンス変数は、含まれている*struct_type*変数が確実に代入されたと見なされる場合、確実に割り当てられたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="18b90-202">*Struct_type*変数は、各インスタンス変数が確実に割り当てられたと見なされる場合、確実に割り当てられたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="18b90-203">明確な代入は、次のコンテキストの要件です。</span><span class="sxs-lookup"><span data-stu-id="18b90-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="18b90-204">変数は、値が取得される場所ごとに確実に割り当てられる必要があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="18b90-205">これにより、未定義の値が発生することはありません。</span><span class="sxs-lookup"><span data-stu-id="18b90-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="18b90-206">式での変数の出現は、の場合を除き、変数の値を取得すると見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="18b90-207">変数は、単純な代入の左オペランドです。</span><span class="sxs-lookup"><span data-stu-id="18b90-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="18b90-208">変数は、出力パラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="18b90-209">変数は*struct_type*変数で、メンバーアクセスの左オペランドとして発生します。</span><span class="sxs-lookup"><span data-stu-id="18b90-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="18b90-210">変数は、参照パラメーターとして渡される場所ごとに確実に代入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="18b90-211">これにより、呼び出される関数メンバーは最初に割り当てられた参照パラメーターを考慮することができます。</span><span class="sxs-lookup"><span data-stu-id="18b90-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="18b90-212">関数メンバーのすべての出力パラメーターは、関数メンバーがを返す各場所 (ステートメントを`return`使用するか、関数メンバー本体の末尾に到達するまで) に確実に代入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="18b90-213">これにより、関数メンバーが出力パラメーターで未定義の値を返さないようにすることができます。これにより、コンパイラは、変数への代入と同等の出力パラメーターとして変数を受け取る関数メンバーの呼び出しを考慮することができます。</span><span class="sxs-lookup"><span data-stu-id="18b90-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="18b90-214">Struct_type `this` instance コンストラクターの変数は、そのインスタンスコンストラクターが返す各場所で、確実に割り当てられる必要があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="18b90-215">最初に割り当てられた変数</span><span class="sxs-lookup"><span data-stu-id="18b90-215">Initially assigned variables</span></span>

<span data-ttu-id="18b90-216">次のカテゴリの変数が最初に割り当てられたものとして分類されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="18b90-217">静的変数</span><span class="sxs-lookup"><span data-stu-id="18b90-217">Static variables.</span></span>
*  <span data-ttu-id="18b90-218">クラスインスタンスのインスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="18b90-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="18b90-219">最初に割り当てられた構造体変数のインスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="18b90-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="18b90-220">配列要素。</span><span class="sxs-lookup"><span data-stu-id="18b90-220">Array elements.</span></span>
*  <span data-ttu-id="18b90-221">値パラメーター。</span><span class="sxs-lookup"><span data-stu-id="18b90-221">Value parameters.</span></span>
*  <span data-ttu-id="18b90-222">参照パラメーター。</span><span class="sxs-lookup"><span data-stu-id="18b90-222">Reference parameters.</span></span>
*  <span data-ttu-id="18b90-223">`catch` 句`foreach`またはステートメントで宣言された変数。</span><span class="sxs-lookup"><span data-stu-id="18b90-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="18b90-224">初期未割り当ての変数</span><span class="sxs-lookup"><span data-stu-id="18b90-224">Initially unassigned variables</span></span>

<span data-ttu-id="18b90-225">次のカテゴリの変数は、最初に未割り当てとして分類されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="18b90-226">初期割り当てられていない構造体変数のインスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="18b90-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="18b90-227">出力パラメーター (構造体`this`インスタンスコンストラクターの変数を含む)。</span><span class="sxs-lookup"><span data-stu-id="18b90-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="18b90-228">ローカル変数 ( `catch`句`foreach`またはステートメントで宣言されているものを除く)。</span><span class="sxs-lookup"><span data-stu-id="18b90-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="18b90-229">明確な代入を決定するための正確な規則</span><span class="sxs-lookup"><span data-stu-id="18b90-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="18b90-230">使用される変数が確実に代入されることを判断するために、コンパイラは、このセクションで説明したものと同等のプロセスを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="18b90-231">コンパイラは、最初に割り当てられていない変数を1つ以上持つ各関数メンバーの本体を処理します。</span><span class="sxs-lookup"><span data-stu-id="18b90-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="18b90-232">最初に割り当てられていない変数*v*について、コンパイラは、関数メンバーの次の各点で*v*の***確実な割り当て状態***を判断します。</span><span class="sxs-lookup"><span data-stu-id="18b90-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="18b90-233">各ステートメントの先頭にある</span><span class="sxs-lookup"><span data-stu-id="18b90-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="18b90-234">各ステートメントのエンドポイント ([エンドポイントと到達可能性](statements.md#end-points-and-reachability))</span><span class="sxs-lookup"><span data-stu-id="18b90-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="18b90-235">別のステートメントまたはステートメントの終了位置に制御を転送する各弧</span><span class="sxs-lookup"><span data-stu-id="18b90-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="18b90-236">各式の先頭にある</span><span class="sxs-lookup"><span data-stu-id="18b90-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="18b90-237">各式の最後</span><span class="sxs-lookup"><span data-stu-id="18b90-237">At the end of each expression</span></span>

<span data-ttu-id="18b90-238">*V*の明確な割り当ての状態は、次のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="18b90-239">確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-239">Definitely assigned.</span></span> <span data-ttu-id="18b90-240">これは、可能なすべての制御フローで、 *v*に値が割り当てられていることを示します。</span><span class="sxs-lookup"><span data-stu-id="18b90-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="18b90-241">確実には割り当てられません。</span><span class="sxs-lookup"><span data-stu-id="18b90-241">Not definitely assigned.</span></span> <span data-ttu-id="18b90-242">型`bool`の式の最後にある変数の状態では、明示的に代入されていない変数の状態は、次のいずれかのサブ状態に分類される場合があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="18b90-243">True 式の後に確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="18b90-244">この状態は、ブール式が true と評価された場合に*v*が確実に代入されることを示しますが、ブール式が false と評価された場合には必ずしも割り当てられるとは限りません。</span><span class="sxs-lookup"><span data-stu-id="18b90-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="18b90-245">False 式の後に確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="18b90-246">この状態は、ブール式が false と評価された場合に*v*が確実に割り当てられることを示しますが、ブール式が true と評価された場合に必ずしも割り当てられるとは限りません。</span><span class="sxs-lookup"><span data-stu-id="18b90-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="18b90-247">次の規則は、変数*v*の状態が各場所でどのように決定されるかを制御します。</span><span class="sxs-lookup"><span data-stu-id="18b90-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="18b90-248">ステートメントの一般的な規則</span><span class="sxs-lookup"><span data-stu-id="18b90-248">General rules for statements</span></span>

*  <span data-ttu-id="18b90-249">*v*は、関数メンバー本体の先頭では確実に割り当てられません。</span><span class="sxs-lookup"><span data-stu-id="18b90-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="18b90-250">*v*は、到達できないステートメントの先頭に確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="18b90-251">他のステートメントの先頭にある*v*の明確な割り当ての状態は、そのステートメントの先頭を対象とするすべての制御フロー転送で*v*の確実な割り当て状態を確認することによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="18b90-252">このようなすべての制御フロー転送で*v*が確実に割り当てられている場合は、ステートメントの先頭に*v*が確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="18b90-253">可能な制御フロー転送のセットは、ステートメントの到達可能性 ([エンドポイントと到達可能性](statements.md#end-points-and-reachability)) をチェックするのと同じ方法で決定されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="18b90-254">ブロック`using` `lock` `foreach` `checked`の終点`for`での v の明確な割り当ての状態は、、、、、、、、、、またはです。 `unchecked` `if` `while` `do` `switch`ステートメントは、そのステートメントのエンドポイントを対象とするすべての制御フロー転送で*v*の確実な割り当て状態を確認することによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="18b90-255">このようなすべての制御フロー転送で*v*が確実に割り当てられている場合は、ステートメントの終了時に*v*が確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="18b90-256">それ以外*v*は、ステートメントのエンドポイントでは確実に割り当てられません。</span><span class="sxs-lookup"><span data-stu-id="18b90-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="18b90-257">可能な制御フロー転送のセットは、ステートメントの到達可能性 ([エンドポイントと到達可能性](statements.md#end-points-and-reachability)) をチェックするのと同じ方法で決定されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="18b90-258">ブロックステートメント、checked、および unchecked ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="18b90-259">ブロック内のステートメントリストの最初のステートメント (または、ステートメントリストが空の場合は、ブロックの終了点) への、制御転送での*v*の確実な割り当て状態は、ブロックの前の*v*の明確な代入ステートメントと同じです。、 `checked`、また`unchecked`はステートメント。</span><span class="sxs-lookup"><span data-stu-id="18b90-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="18b90-260">式ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-260">Expression statements</span></span>

<span data-ttu-id="18b90-261">式ステートメントの*stmt*には、式*expr*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="18b90-262">*v*は、 *stmt*の先頭にある*expr*の先頭で同じ明確な割り当て状態を持ちます。</span><span class="sxs-lookup"><span data-stu-id="18b90-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-263">*Expr*の終わりに*v*が明示的に割り当てられている場合は、 *stmt*の終点に確実に代入されます。それ以外これは、 *stmt*のエンドポイントでは確実に割り当てられません。</span><span class="sxs-lookup"><span data-stu-id="18b90-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="18b90-264">宣言ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-264">Declaration statements</span></span>

*  <span data-ttu-id="18b90-265">*Stmt*が初期化子を含まない宣言ステートメントである場合、 *v*は stmt の先頭にある*stmt*の終了時点で同じ明確な割り当て状態に*なります。*</span><span class="sxs-lookup"><span data-stu-id="18b90-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-266">*Stmt*が初期化子を持つ宣言ステートメントである場合、 *v*の明確な割り当ての状態は、 *stmt*がステートメントリストであるかのように決定されます。これには、初期化子を持つ宣言ごとに1つの代入ステートメントがあります (宣言)。</span><span class="sxs-lookup"><span data-stu-id="18b90-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="18b90-267">If ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-267">If statements</span></span>

<span data-ttu-id="18b90-268">次の形式のステートメントのstmtの`if`場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="18b90-269">*v*は、 *stmt*の先頭にある*expr*の先頭で同じ明確な割り当て状態を持ちます。</span><span class="sxs-lookup"><span data-stu-id="18b90-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-270">*Expr*の最後に*v*が明示的に割り当てられている場合、 *then_stmt*に制御フロー転送が割り当てられ、else 句が存在しない場合は、 *else_stmt*または*stmt*の終了ポイントのいずれかに確実に代入されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="18b90-271">*Expr*の終わりに "true 式の後に明示的に代入されました" という状態が*v*にある場合は、制御フロー転送で*then_stmt*に確実に割り当てられ、else_ への制御フロー転送では確実に割り当てられません。else 句が*ない場合は、stmt の*終了位置に stmt またはを指定します。</span><span class="sxs-lookup"><span data-stu-id="18b90-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="18b90-272">*Expr*の終わりに、 *v*が "false 式の後に確実に代入されました" という状態がある場合は、 *else_stmt*への制御フロー転送で確実に割り当てられ、then_stmt への制御フロー転送では確実に割り当てられません。.</span><span class="sxs-lookup"><span data-stu-id="18b90-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="18b90-273">これは、 *then_stmt*のエンドポイントで確実に割り当てられている場合にのみ、 *stmt*のエンドポイントで確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="18b90-274">それ以外の場合、 *v*は、制御フロー転送で*then_stmt*または*else_stmt*のいずれかに割り当てられているとは限りません。また、else 句がない場合は、 *stmt*のエンドポイントにも代入されません。</span><span class="sxs-lookup"><span data-stu-id="18b90-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="18b90-275">Switch ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-275">Switch statements</span></span>

<span data-ttu-id="18b90-276">制御式`switch` *expr*を持つステートメントの*stmt*で、次のように指定します。</span><span class="sxs-lookup"><span data-stu-id="18b90-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="18b90-277">*Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-278">到達可能なスイッチブロックステートメントリストへの制御フロー転送の*v*の明示的な割り当て状態は、 *expr*の最後に*v*を指定した場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="18b90-279">While ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-279">While statements</span></span>

<span data-ttu-id="18b90-280">次の形式のステートメントのstmtの`while`場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="18b90-281">*v*は、 *stmt*の先頭にある*expr*の先頭で同じ明確な割り当て状態を持ちます。</span><span class="sxs-lookup"><span data-stu-id="18b90-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-282">*Expr*の最後に*v*が明示的に割り当てられている場合、明示的には制御フロー転送で*while_body*に割り当てられ、終了ポイントは*stmt*に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="18b90-283">*Expr*の終わりに "true 式の後に明示的に代入されました" という状態*がある場合*、明示的に割り当てられますが、 *while_body*への制御フロー転送は確実に代入されますが、 *stmt*のエンドポイントでは確実に割り当てられません。</span><span class="sxs-lookup"><span data-stu-id="18b90-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="18b90-284">*Expr*の終わりに*v*が "false 式の後に確実に代入されました" という状態がある場合は、明示的に制御フロー転送で*stmt*の終了ポイントに割り当てられますが、その間の制御フロー転送では確実に割り当てられません。 *本文 (_c)*</span><span class="sxs-lookup"><span data-stu-id="18b90-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="18b90-285">Do ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-285">Do statements</span></span>

<span data-ttu-id="18b90-286">次の形式のステートメントのstmtの`do`場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="18b90-287">*v*は、stmt の先頭から*do_body*への制御フロー転送において、 *stmt*の先頭と同じ*ように明確*な割り当て状態になります。</span><span class="sxs-lookup"><span data-stu-id="18b90-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-288">*v*は、 *do_body*の終点と同様に、 *expr*の先頭に同じ明確な割り当て状態を持ちます。</span><span class="sxs-lookup"><span data-stu-id="18b90-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="18b90-289">*Expr*の最後に*v*が明示的に割り当てられている場合は、制御フロー転送で*stmt*の終点に確実に代入されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="18b90-290">*Expr*の最後に、"false 式の後に確実に代入されました" という状態が*v*にある場合は、明示的に制御フロー転送で*stmt*の終点に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="18b90-291">For ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-291">For statements</span></span>

<span data-ttu-id="18b90-292">次の形式のステートメント`for`に対して明確な代入をチェックします。</span><span class="sxs-lookup"><span data-stu-id="18b90-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="18b90-293">は、ステートメントが書き込まれたかのように実行されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="18b90-294">*For_condition* `for`がステートメントから省略されている場合は、上記の展開で*for_condition*がに`true`置き換えられたかのように、明確な割り当ての評価が行われます。</span><span class="sxs-lookup"><span data-stu-id="18b90-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="18b90-295">Break、continue、および goto ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="18b90-296">、 `break` 、または`goto`ステートメントによって発生する制御フロー転送の v の明確な割り当て状態は、ステートメントの先頭にある v の明確な代入の状態と同じです。 `continue`</span><span class="sxs-lookup"><span data-stu-id="18b90-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="18b90-297">Throw ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-297">Throw statements</span></span>

<span data-ttu-id="18b90-298">フォームのステートメントの*stmt*の場合</span><span class="sxs-lookup"><span data-stu-id="18b90-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="18b90-299">*Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭にある*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="18b90-300">Return ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-300">Return statements</span></span>

<span data-ttu-id="18b90-301">フォームのステートメントの*stmt*の場合</span><span class="sxs-lookup"><span data-stu-id="18b90-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="18b90-302">*Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭にある*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-303">*V*が出力パラメーターの場合は、次のいずれかが確実に割り当てられている必要があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="18b90-304">after *expr*</span><span class="sxs-lookup"><span data-stu-id="18b90-304">after *expr*</span></span>
    * <span data-ttu-id="18b90-305">また`finally`は、 `try` ステートメントを`return`囲むまたは`try` のブロック-の末尾にあります。 `catch` - `finally` - `finally`</span><span class="sxs-lookup"><span data-stu-id="18b90-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="18b90-306">次の形式のステートメントの stmt の場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="18b90-307">*V*が出力パラメーターの場合は、次のいずれかが確実に割り当てられている必要があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="18b90-308">*stmt*の前</span><span class="sxs-lookup"><span data-stu-id="18b90-308">before *stmt*</span></span>
    * <span data-ttu-id="18b90-309">また`finally`は、 `try` ステートメントを`return`囲むまたは`try` のブロック-の末尾にあります。 `catch` - `finally` - `finally`</span><span class="sxs-lookup"><span data-stu-id="18b90-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="18b90-310">Try-catch ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-310">Try-catch statements</span></span>

<span data-ttu-id="18b90-311">次の形式のステートメントの*stmt*の場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="18b90-312">*Try_block*の開始時の*v*の明確な割り当て状態は、 *stmt*の開始時の*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-313">*Catch_block_i*の開始時の*v*の明確な割り当ての状態 (任意の*i*) は、 *stmt*の先頭にある*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-314">*Try_block*のエンドポイント*で v が*確実に割り当てられている場合は、 *stmt*のエンドポイントでの*v*の明確な割り当て状態が確実に割り当てられます (1 から n まで*のすべての* *catch_block_i* )。).</span><span class="sxs-lookup"><span data-stu-id="18b90-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="18b90-315">Try-finally ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-315">Try-finally statements</span></span>

<span data-ttu-id="18b90-316">次の形式のステートメントのstmtの`try`場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="18b90-317">*Try_block*の開始時の*v*の明確な割り当て状態は、 *stmt*の開始時の*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-318">*Finally_block*の開始時の*v*の明確な割り当て状態は、 *stmt*の開始時の*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-319">次のいずれかが true の場合にのみ (およびの場合のみ)、 *stmt*のエンドポイントでの*v*の明確な割り当て状態が確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="18b90-320">*v*は*try_block*のエンドポイントで確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="18b90-321">*v*は*finally_block*のエンドポイントで確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="18b90-322">制御フロー `goto`転送 (ステートメントなど) が*try_block*内で開始され、 *try_block*の外部で終了した場合、v がである場合は、その制御フロー転送で*v*が明示的に割り当てられていると見なされます。*finally_block*のエンドポイントで確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="18b90-323">(これは、この制御フロー転送の別の理由で*v*が確実に割り当てられている場合にだけではありません)。</span><span class="sxs-lookup"><span data-stu-id="18b90-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="18b90-324">Try-catch ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-324">Try-catch-finally statements</span></span>

<span data-ttu-id="18b90-325">次の形式の`try` `catch` - ステートメント`finally`の-明確な代入分析。</span><span class="sxs-lookup"><span data-stu-id="18b90-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="18b90-326">ステートメントがステートメントを囲む`try` `finally` `try` - ステートメント-であるかのように実行されます。 `catch`</span><span class="sxs-lookup"><span data-stu-id="18b90-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="18b90-327">次の例では、 `try`ステートメントのさまざまなブロック ([try ステートメント](statements.md#the-try-statement)) が確実な代入にどのように影響するかを示します。</span><span class="sxs-lookup"><span data-stu-id="18b90-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a><span data-ttu-id="18b90-328">Foreach ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-328">Foreach statements</span></span>

<span data-ttu-id="18b90-329">次の形式のステートメントのstmtの`foreach`場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="18b90-330">*Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-331">*Embedded_statement*または*stmt*の終点に対する制御フロー転送の*v*の明確な割り当て状態は、 *expr*の最後の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="18b90-332">ステートメントの使用</span><span class="sxs-lookup"><span data-stu-id="18b90-332">Using statements</span></span>

<span data-ttu-id="18b90-333">次の形式のステートメントのstmtの`using`場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="18b90-334">*Resource_acquisition*の開始時の*v*の明確な割り当て状態は、 *stmt*の先頭の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-335">*Embedded_statement*への制御フロー転送での*v*の明確な割り当て状態は、 *resource_acquisition*の最後の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="18b90-336">Lock ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-336">Lock statements</span></span>

<span data-ttu-id="18b90-337">次の形式のステートメントのstmtの`lock`場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="18b90-338">*Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-339">*Embedded_statement*への制御フロー転送での*v*の明確な割り当て状態は、 *expr*の最後の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="18b90-340">Yield ステートメント</span><span class="sxs-lookup"><span data-stu-id="18b90-340">Yield statements</span></span>

<span data-ttu-id="18b90-341">次の形式のステートメントのstmtの`yield return`場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="18b90-342">*Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="18b90-343">*Stmt*の終了時の*v*の明確な割り当て状態は、 *expr*の最後の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="18b90-344">ステートメント`yield break`は、確実な割り当て状態には影響しません。</span><span class="sxs-lookup"><span data-stu-id="18b90-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="18b90-345">単純式の一般的な規則</span><span class="sxs-lookup"><span data-stu-id="18b90-345">General rules for simple expressions</span></span>

<span data-ttu-id="18b90-346">リテラル ([リテラル](expressions.md#literals))、単純な名前 ([簡易名](expressions.md#simple-names))、メンバーアクセス式 ([メンバーアクセス](expressions.md#member-access))、インデックス付けされていないベースアクセス式 ([ベースアクセス](expressions.md#base-access)) `typeof`には、次の規則が適用されます。式 ([typeof 演算子](expressions.md#the-typeof-operator))、既定値の式 ([既定値の式](expressions.md#default-value-expressions)) `nameof` 、および[式 (値の式)](expressions.md#nameof-expressions)。</span><span class="sxs-lookup"><span data-stu-id="18b90-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="18b90-347">このような式の最後にある*v*の明確な割り当ての状態は、式の先頭にある*v*の明確な代入の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="18b90-348">埋め込み式を使用した式の一般的な規則</span><span class="sxs-lookup"><span data-stu-id="18b90-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="18b90-349">次の規則は、かっこで囲まれた式 ([かっこで囲ま](expressions.md#parenthesized-expressions)れた式)、要素アクセス式 ([要素アクセス](expressions.md#element-access))、インデックスを使用した基本アクセス式 ([ベースアクセス](expressions.md#base-access))、増分、およびの各式に適用されます。デクリメント式 ([後置インクリメントおよびデクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、[前置インクリメント演算子とデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))、キャスト式`~`([キャスト式](expressions.md#cast-expressions)) `+`、 `-`単項`*`演算子、式、バイナリ`+` `-` 、`*`、 `/` 、、`%`、、、、、、、 `<<` `>>` `<` `<=` `>` `>=``==` 、`!=`、 、`as`、 、、`^`式([算術演算子](expressions.md#arithmetic-operators)、[シフト演算子](expressions.md#shift-operators)、関係、および型のテスト) `|` `&` `is` [演算子](expressions.md#relational-and-type-testing-operators)、[論理演算子](expressions.md#logical-operators))、複合代入式 ([複合代入](expressions.md#compound-assignment))、 `checked`および`unchecked`式 ([checked および unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) に加えて、配列とデリゲート作成式 ([新しい演算子](expressions.md#the-new-operator))。</span><span class="sxs-lookup"><span data-stu-id="18b90-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="18b90-350">これらの各式には、無条件で決まった順序で評価される1つ以上のサブ式があります。</span><span class="sxs-lookup"><span data-stu-id="18b90-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="18b90-351">たとえば、二項`%`演算子は演算子の左辺、次に右辺を評価します。</span><span class="sxs-lookup"><span data-stu-id="18b90-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="18b90-352">インデックス作成操作によって、インデックス付きの式が評価され、左から右に順番に各インデックス式が評価されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="18b90-353">式の*expr*では、サブ式*e1, e2,..., eN*がこの順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="18b90-354">*E1*の開始時の*v*の明確な割り当ての状態は、 *expr*の先頭での確定代入の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="18b90-355">*Ei*の先頭における*v*の明確な割り当ての状態 (*i*が1を超える) は、前のサブ式の最後にある明確な代入の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="18b90-356">*Expr*の最後における*v*の明確な割り当ての状態は、 *eN*の終わりにある明確な代入の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="18b90-357">呼び出し式とオブジェクト作成式</span><span class="sxs-lookup"><span data-stu-id="18b90-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="18b90-358">次の形式の呼び出し式の*expr* 。</span><span class="sxs-lookup"><span data-stu-id="18b90-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="18b90-359">または、次の形式のオブジェクト作成式。</span><span class="sxs-lookup"><span data-stu-id="18b90-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="18b90-360">呼び出し式の場合、 *primary_expression*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="18b90-361">呼び出し式の場合、 *arg1*の前の*v*の明確な割り当て状態は、 *primary_expression*後の*v*の状態と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="18b90-362">オブジェクト作成式の場合、 *arg1*より前の*v*の明確な割り当ての状態は、 *expr*より前の*v*の状態と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="18b90-363">各引数*argi*に対して、 *argi* after の明示*代入の状態*は、正規`ref`表現の規則によって決定`out`され、または修飾子は無視されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="18b90-364">各引数*argi*が1より大きい場合、 *argi*前の*v*の明確な割り当ての状態は、前*の引数の*後の*v*の状態と同じ*になります*。</span><span class="sxs-lookup"><span data-stu-id="18b90-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="18b90-365">引数のいずれかで変数*v*が`out`引数として渡された場合 (つまり`out v`、形式の引数)、 *v* after *expr*の状態は確実に代入されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="18b90-366">それ以外*v* after *expr*の状態は、 *argN*の後の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="18b90-367">配列初期化子 ([配列作成式](expressions.md#array-creation-expressions))、オブジェクト初期化子 ([オブジェクト初期化](expressions.md#object-initializers)子)、コレクション初期化子 ([コレクション初期化子](expressions.md#collection-initializers))、および匿名オブジェクト初期化子 ([匿名オブジェクトの作成) の場合式](expressions.md#anonymous-object-creation-expressions)) の場合、明確な割り当ての状態は、これらの構成要素がに関して定義されている展開によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="18b90-368">単純な代入式</span><span class="sxs-lookup"><span data-stu-id="18b90-368">Simple assignment expressions</span></span>

<span data-ttu-id="18b90-369">式*expr*の形式`w = expr_rhs`は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="18b90-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="18b90-370">*Expr_rhs*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="18b90-371">*V* after *expr*の明確な割り当て状態は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="18b90-372">*W*が*v*と同じ変数の場合、 *v* after *expr*の明確な代入の状態は確実に代入されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="18b90-373">それ以外の場合、割り当てが構造体型のインスタンスコンストラクター内で行われる場合、 *w*がプロパティアクセスである場合、構築されるインスタンスに自動的に実装されたプロパティ*P*が指定され、 *v*が非表示のバッキングフィールドになります。*P*の場合、 *v* after *expr*の明確な割り当て状態は確実に代入されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="18b90-374">それ以外の場合、 *v* after *expr*の明示代入の状態は、 *expr_rhs*後の*v*の明確な代入の状態と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="18b90-375">& & (条件式および式式)</span><span class="sxs-lookup"><span data-stu-id="18b90-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="18b90-376">式*expr*の形式`expr_first && expr_second`は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="18b90-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="18b90-377">*Expr_first*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="18b90-378">*Expr_first*の状態が明示的に割り当てられている場合、または "true 式の後に確実に割り当てら*れた"* 場合は、 *expr_second*より前の*v*の明確な割り当て状態が確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="18b90-379">それ以外の場合は、確実に割り当てられません。</span><span class="sxs-lookup"><span data-stu-id="18b90-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="18b90-380">*V* after *expr*の明確な割り当て状態は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="18b90-381">*Expr_first* `false`が値を持つ定数式である場合、 *v* after *expr*の明示的な代入の状態は、 *expr_first*の*v*の明示的な代入の状態と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="18b90-382">それ以外の場合、 *expr_first*後の*v*の状態が確実に割り当てられると、 *v* after *expr*の状態は確実に代入されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="18b90-383">それ以外の場合、 *v* after *expr_second*の状態が確実に割り当てられ、 *expr_first*後の状態が "false 式の後に確実に割り当てられました" になると、 *v* *after* *expr*の状態は確実になります。担当.</span><span class="sxs-lookup"><span data-stu-id="18b90-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="18b90-384">それ以外の場合、 *v* after *expr_second*が明示的に割り当てられているか、"true 式の後に確実に割り当てられました" の場合、 *v* after *expr*の状態は "true expression の後に確実に割り当てられます" になります。</span><span class="sxs-lookup"><span data-stu-id="18b90-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="18b90-385">それ以外の場合は、 *expr_first*後の*v*の状態が "false expression の後に確実に割り当てられました" で、 *expr_second*が "false *expression* *の後に明示的に割り当てられました" という状態になります。expr*は、"false 式の後に確実に代入されます" です。</span><span class="sxs-lookup"><span data-stu-id="18b90-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="18b90-386">それ以外の場合、 *v* after *expr*の状態は確実に代入されません。</span><span class="sxs-lookup"><span data-stu-id="18b90-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="18b90-387">この例では、</span><span class="sxs-lookup"><span data-stu-id="18b90-387">In the example</span></span>
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="18b90-388">変数`i`は、 `if`ステートメントの埋め込みステートメントの1つでは確実に代入されますが、他方には存在しないと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="18b90-389">`if`メソッド`i` `(i = y)`のステートメントでは、最初の埋め込みステートメントで変数が確実に代入されます。これは、式の実行が常にこの埋め込みステートメントの実行に先行するからです。 `F`</span><span class="sxs-lookup"><span data-stu-id="18b90-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="18b90-390">これに対して、 `i`変数は、2番目の埋め込みステートメントで`x >= 0`は確実に割り当てられません。 `i`これは、false がテストされ、変数が割り当て解除されるためです。</span><span class="sxs-lookup"><span data-stu-id="18b90-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="18b90-391">||(条件式または) 式</span><span class="sxs-lookup"><span data-stu-id="18b90-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="18b90-392">式*expr*の形式`expr_first || expr_second`は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="18b90-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="18b90-393">*Expr_first*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="18b90-394">*Expr_first*の状態が明示的に割り当てられている場合、または "false 式の後に確実に割り当てら*れた"* 場合は、 *expr_second*より前の*v*の明確な割り当て状態が確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="18b90-395">それ以外の場合は、確実に割り当てられません。</span><span class="sxs-lookup"><span data-stu-id="18b90-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="18b90-396">*V*の明示代入ステートメントは、*次の方法で決定され*ます。</span><span class="sxs-lookup"><span data-stu-id="18b90-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="18b90-397">*Expr_first* `true`が値を持つ定数式である場合、 *v* after *expr*の明示的な代入の状態は、 *expr_first*の*v*の明示的な代入の状態と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="18b90-398">それ以外の場合、 *expr_first*後の*v*の状態が確実に割り当てられると、 *v* after *expr*の状態は確実に代入されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="18b90-399">それ以外の場合、 *v* after *expr_second*の状態が確実に割り当てられ、 *expr_first*後の*v*の状態が "true expression の後に確実に割り当てられました" である場合、 *v* after *expr*の状態は確実になります。担当.</span><span class="sxs-lookup"><span data-stu-id="18b90-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="18b90-400">それ以外の場合、 *v* after *expr_second*の状態が確実に割り当てられているか、"false 式の後に確実に代入されました" の場合、 *v* after *expr*の状態は "false 式の後に確実に割り当てられます" になります。</span><span class="sxs-lookup"><span data-stu-id="18b90-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="18b90-401">それ以外の場合は、 *expr_first*後の*v*の状態が "true expression の後に確実に割り当てられました" で、 *expr_second*が "true *expression の後*に*明示的に*割り当てられています" という状態になります。は "true 式の後に確実に代入されました" です。</span><span class="sxs-lookup"><span data-stu-id="18b90-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="18b90-402">それ以外の場合、 *v* after *expr*の状態は確実に代入されません。</span><span class="sxs-lookup"><span data-stu-id="18b90-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="18b90-403">この例では、</span><span class="sxs-lookup"><span data-stu-id="18b90-403">In the example</span></span>
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="18b90-404">変数`i`は、 `if`ステートメントの埋め込みステートメントの1つでは確実に代入されますが、他方には存在しないと見なされます。</span><span class="sxs-lookup"><span data-stu-id="18b90-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="18b90-405">`if`メソッド`i` `(i = y)`のステートメントでは、2番目の embedded ステートメントで変数が確実に代入されます。これは、式の実行が常にこの埋め込みステートメントの実行に先行するためです。 `G`</span><span class="sxs-lookup"><span data-stu-id="18b90-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="18b90-406">これに対して、 `i`変数は、最初の埋め込みステートメントでは確実`x >= 0`に割り当てられません。これは`i` 、true がテストされている可能性があり、その結果、変数が割り当て解除されるためです。</span><span class="sxs-lookup"><span data-stu-id="18b90-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="18b90-407">!</span><span class="sxs-lookup"><span data-stu-id="18b90-407">!</span></span> <span data-ttu-id="18b90-408">(論理否定) 式</span><span class="sxs-lookup"><span data-stu-id="18b90-408">(logical negation) expressions</span></span>

<span data-ttu-id="18b90-409">式*expr*の形式`! expr_operand`は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="18b90-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="18b90-410">*Expr_operand*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="18b90-411">*V* after *expr*の明確な割り当て状態は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="18b90-412">\* Expr_operand \* の後の*v*の状態が確実に割り当てられている場合、 *v* after *expr*の状態は確実に代入されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="18b90-413">\* Expr_operand \* の後の*v*の状態が確実に割り当てられていない場合、 *v* after *expr*の状態は確実に代入されません。</span><span class="sxs-lookup"><span data-stu-id="18b90-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="18b90-414">\* Expr_operand \* 後の*v*の状態が "false 式の後に確実に割り当てられました" の場合、 *v* after *expr*の状態は "true expression の後に確実に割り当てられます" になります。</span><span class="sxs-lookup"><span data-stu-id="18b90-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="18b90-415">\* Expr_operand \* 後の*v*の状態が "true expression の後に確実に割り当てられました" の場合、 *v* after *expr*の状態は、"false 式の後に確実に割り当てられます" になります。</span><span class="sxs-lookup"><span data-stu-id="18b90-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="18b90-416">??</span><span class="sxs-lookup"><span data-stu-id="18b90-416">??</span></span> <span data-ttu-id="18b90-417">(null 合体) 式</span><span class="sxs-lookup"><span data-stu-id="18b90-417">(null coalescing) expressions</span></span>

<span data-ttu-id="18b90-418">式*expr*の形式`expr_first ?? expr_second`は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="18b90-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="18b90-419">*Expr_first*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="18b90-420">*Expr_second*より前の*v*の明確な割り当ての状態は、 *expr_first*の後の*v*の明確な割り当て状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="18b90-421">*V*の明示代入ステートメントは、*次の方法で決定され*ます。</span><span class="sxs-lookup"><span data-stu-id="18b90-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="18b90-422">*Expr_first*が定数式 ([定数式](expressions.md#constant-expressions)) で、値が null の場合、 *v* after *expr*の状態は、 *expr_second*の後の*v*の状態と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="18b90-423">それ以外の場合、 *v* after *expr*の状態は、 *expr_first*の後の*v*の明確な割り当て状態と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="18b90-424">?: (条件) 式</span><span class="sxs-lookup"><span data-stu-id="18b90-424">?: (conditional) expressions</span></span>

<span data-ttu-id="18b90-425">式*expr*の形式`expr_cond ? expr_true : expr_false`は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="18b90-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="18b90-426">*Expr_cond*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="18b90-427">次のいずれかに当てはまる場合にのみ、 *expr_true*より前の*v*の明確な割り当て状態が確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="18b90-428">*expr_cond*は、値を持つ定数式です。`false`</span><span class="sxs-lookup"><span data-stu-id="18b90-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="18b90-429">*expr_cond*後の*v*の状態は、確実に割り当てられるか、"true 式の後に確実に代入されました" です。</span><span class="sxs-lookup"><span data-stu-id="18b90-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="18b90-430">次のいずれかに当てはまる場合にのみ、 *expr_false*より前の*v*の明確な割り当て状態が確実に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="18b90-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="18b90-431">*expr_cond*は、値を持つ定数式です。`true`</span><span class="sxs-lookup"><span data-stu-id="18b90-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="18b90-432">*expr_cond*後の*v*の状態は、確実に割り当てられるか、"false 式の後に確実に代入されました" です。</span><span class="sxs-lookup"><span data-stu-id="18b90-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="18b90-433">*V* after *expr*の明確な割り当て状態は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="18b90-434">`true` *Expr_cond*が値を持つ定数式 ([定数式](expressions.md#constant-expressions)) である場合、 *v* after *expr*の状態は、 *expr_true*の後の*v*の状態と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="18b90-435">それ以外の場合、 *expr_cond*が値`false`を持つ定数式 ([定数式](expressions.md#constant-expressions)) である場合、 *v* after *expr*の状態は、 *expr_false*の後の*v*の状態と同じになります。</span><span class="sxs-lookup"><span data-stu-id="18b90-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="18b90-436">それ以外の場合、 *expr_true*後の*v*の状態が確実に割り当てられ、 *expr_false*後の*v*の状態が確実に割り当てられると、 *v* after *expr*の状態は確実に代入されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="18b90-437">それ以外の場合、 *v* after *expr*の状態は確実に代入されません。</span><span class="sxs-lookup"><span data-stu-id="18b90-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="18b90-438">匿名関数</span><span class="sxs-lookup"><span data-stu-id="18b90-438">Anonymous functions</span></span>

<span data-ttu-id="18b90-439">本文 (*ブロック*または*式*) の*本体*を持つ*lambda_expression*または*anonymous_method_expression* *expr*の場合:</span><span class="sxs-lookup"><span data-stu-id="18b90-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="18b90-440">*本体*より前の外部変数*v*の明確な割り当ての状態は、 *expr*より前の*v*の状態と同じです。</span><span class="sxs-lookup"><span data-stu-id="18b90-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="18b90-441">つまり、外部変数の明確な割り当て状態は、匿名関数のコンテキストから継承されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="18b90-442">外部変数*v*の明示的な代入の状態は、 *expr*より前の*v*の状態と同じ*です。*</span><span class="sxs-lookup"><span data-stu-id="18b90-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="18b90-443">例</span><span class="sxs-lookup"><span data-stu-id="18b90-443">The example</span></span>
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
<span data-ttu-id="18b90-444">では、匿名関数が宣言`max`されている場所が確実に割り当てられないため、コンパイル時エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="18b90-445">例</span><span class="sxs-lookup"><span data-stu-id="18b90-445">The example</span></span>
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
<span data-ttu-id="18b90-446">では、匿名関数のへ`n`の代入が匿名関数の外部の確実な`n`割り当て状態に影響を与えないため、コンパイル時エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="18b90-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="18b90-447">変数参照</span><span class="sxs-lookup"><span data-stu-id="18b90-447">Variable references</span></span>

<span data-ttu-id="18b90-448">*Variable_reference*は、変数として分類される*式*です。</span><span class="sxs-lookup"><span data-stu-id="18b90-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="18b90-449">*Variable_reference*は、現在の値を取得し、新しい値を格納するためにアクセスできるストレージの場所を表します。</span><span class="sxs-lookup"><span data-stu-id="18b90-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="18b90-450">C およびC++では、 *variable_reference*は*左辺*値と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="18b90-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="18b90-451">変数参照の原子性</span><span class="sxs-lookup"><span data-stu-id="18b90-451">Atomicity of variable references</span></span>

<span data-ttu-id="18b90-452">`bool`次のデータ型の読み取りと書き込みは`int` `uint` `short` `ushort` `byte`、atomic: `char`、、 `sbyte` 、`float`、、、、、、およびの各参照型です。</span><span class="sxs-lookup"><span data-stu-id="18b90-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="18b90-453">また、前の一覧の基になる型を持つ列挙型の読み取りと書き込みもアトミックです。</span><span class="sxs-lookup"><span data-stu-id="18b90-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="18b90-454">`long` 、`ulong`、 、`decimal`など、他の型の読み取りと書き込みは、ユーザー定義型と同様にアトミックであるとは限りません。 `double`</span><span class="sxs-lookup"><span data-stu-id="18b90-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="18b90-455">この目的のために設計されたライブラリ関数とは別に、インクリメントまたはデクリメントの場合など、アトミック読み取り/変更/書き込みの保証はありません。</span><span class="sxs-lookup"><span data-stu-id="18b90-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

