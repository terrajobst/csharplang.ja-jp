---
ms.openlocfilehash: b7bb7dd575d9e2e6d5dd85bdd3e535411e29fcf4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488865"
---
# <a name="variables"></a><span data-ttu-id="1f0ce-101">変数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-101">Variables</span></span>

<span data-ttu-id="1f0ce-102">変数は、記憶域の場所を表します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-102">Variables represent storage locations.</span></span> <span data-ttu-id="1f0ce-103">すべての変数ができる値を指定する型の変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="1f0ce-104">C# は、タイプ セーフな言語と c# コンパイラでは、変数に格納されている値は常に適切な型のことが保証されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="1f0ce-105">割り当てまたはを使用して変数の値を変更できる、`++`と`--`演算子。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="1f0ce-106">変数である必要があります***確実に代入***([確実な代入](variables.md#definite-assignment)) 前に、その値を取得できます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="1f0ce-107">いずれかの変数は、次のセクションで説明した、***最初に割り当てられた***または***最初に割り当てられていない***します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="1f0ce-108">最初に割り当てられている変数は、明確に定義された初期値を備え、明示的に代入すると考えられます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="1f0ce-109">最初に未割り当ての変数には、初期値はありません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="1f0ce-110">特定の場所に明示的に代入して考慮することを最初に割り当てられていない変数、その位置に至るすべての可能な実行パスで、変数への代入があります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="1f0ce-111">変数のカテゴリ</span><span class="sxs-lookup"><span data-stu-id="1f0ce-111">Variable categories</span></span>

<span data-ttu-id="1f0ce-112">C# 7 つのカテゴリの変数を定義します。 静的変数、インスタンス変数、配列の要素、パラメーターの値、参照パラメーター、出力パラメーター、およびローカル変数。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="1f0ce-113">次のセクションでは、これらの各カテゴリについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="1f0ce-114">例</span><span class="sxs-lookup"><span data-stu-id="1f0ce-114">In the example</span></span>
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
<span data-ttu-id="1f0ce-115">`x` 静的変数、`y`インスタンス変数は、`v[0]`配列要素である`a`値パラメーターでは、`b`参照パラメーターは、`c`は出力パラメーターと`i`はローカル変数です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="1f0ce-116">静的変数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-116">Static variables</span></span>

<span data-ttu-id="1f0ce-117">宣言されたフィールド、`static`修飾子と呼ばれる、***静的変数***します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="1f0ce-118">静的変数は、静的コンス トラクターの実行前に存在するように、([静的コンス トラクター](classes.md#static-constructors)) その親の種類、および関連付けられているアプリケーション ドメインが存在しなくなるときに存在する停止します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="1f0ce-119">静的変数の初期値は、既定値 ([既定値](variables.md#default-values)) の変数の型。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="1f0ce-120">確実な代入をチェックのために、静的変数は最初に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="1f0ce-121">インスタンス変数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-121">Instance variables</span></span>

<span data-ttu-id="1f0ce-122">なしで宣言されたフィールド、`static`修飾子と呼ばれる、***インスタンス変数***します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="1f0ce-123">クラスのインスタンス変数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-123">Instance variables in classes</span></span>

<span data-ttu-id="1f0ce-124">クラスのインスタンス変数は、そのクラスの新しいインスタンスが作成され、そのインスタンスへの参照がないと、インスタンスのデストラクター (存在する場合) が実行時に存在しなくなったときに、存在するように得られます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="1f0ce-125">クラスのインスタンス変数の初期値は、既定値 ([既定値](variables.md#default-values)) の変数の型。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="1f0ce-126">クラスのインスタンス変数は、確実な代入をチェックするために、最初に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="1f0ce-127">構造体にインスタンス変数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-127">Instance variables in structs</span></span>

<span data-ttu-id="1f0ce-128">構造体のインスタンス変数には、正確に同じ有効期間が所属する構造体変数としてがあります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="1f0ce-129">つまり、構造体の型の変数にまたは場合、存在しなくなりますすぎる構造体のインスタンス変数の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="1f0ce-130">構造体のインスタンス変数の初期の割り当ての状態は、構造体の変数の場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="1f0ce-131">つまり、構造体変数が最初と見なされるタイミング割り当てられると、これがそのインスタンス変数と構造体変数が最初に割り当てられていないと見なされると、そのインスタンス変数が同様に割り当てられています。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="1f0ce-132">配列の要素</span><span class="sxs-lookup"><span data-stu-id="1f0ce-132">Array elements</span></span>

<span data-ttu-id="1f0ce-133">配列の要素は、配列インスタンスの作成時に存在することになるし、その配列インスタンスへの参照がない場合に存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="1f0ce-134">各配列の要素の初期値は、既定値 ([既定値](variables.md#default-values)) の配列の要素の型。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="1f0ce-135">配列の要素は、確実な代入をチェックするために、最初に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="1f0ce-136">値パラメーター</span><span class="sxs-lookup"><span data-stu-id="1f0ce-136">Value parameters</span></span>

<span data-ttu-id="1f0ce-137">パラメーターなしで宣言された、`ref`または`out`修飾子は、***値パラメーター***。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="1f0ce-138">関数メンバー (メソッド、インスタンス コンス トラクター、アクセサー、または演算子) または匿名関数の呼び出し時に存在するように値を持つパラメーターには、パラメーターが属していると、呼び出しで指定された引数の値で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="1f0ce-139">値を持つパラメーターは、通常、関数メンバーまたは匿名関数が戻るときに削除されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="1f0ce-140">ただし、値パラメーターが匿名関数によってキャプチャされたかどうか ([匿名関数式](expressions.md#anonymous-function-expressions))、その有効期間をデリゲートには少なくとも拡張または作成の匿名関数式ツリーが対象であります。ガベージ コレクション。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="1f0ce-141">値を持つパラメーターは、確実な代入をチェックするために、最初に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="1f0ce-142">参照パラメーター</span><span class="sxs-lookup"><span data-stu-id="1f0ce-142">Reference parameters</span></span>

<span data-ttu-id="1f0ce-143">宣言したパラメーター、`ref`修飾子は、***パラメーター参照***します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="1f0ce-144">参照パラメーターは、新しい記憶域の場所を作成できません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="1f0ce-145">代わりに、参照パラメーターは、関数メンバーまたは匿名関数の呼び出しで引数として指定された変数と同じストレージ場所を表します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="1f0ce-146">したがって、参照パラメーターの値は常に、基になる変数と同じです。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="1f0ce-147">次の確実な代入ルールは、参照パラメーターに適用されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="1f0ce-148">説明されている出力パラメーターのさまざまな規則に注意してください[出力パラメーター](variables.md#output-parameters)します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="1f0ce-149">変数を明示的に代入する必要があります ([確実な代入](variables.md#definite-assignment)) 関数のメンバーまたはデリゲートの呼び出しで参照パラメーターとして渡す前にします。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="1f0ce-150">関数メンバーまたは匿名関数の中で参照パラメーターが最初に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="1f0ce-151">インスタンス メソッドまたはインスタンス アクセサー、構造体型の内、`this`キーワードは構造体の型の参照パラメーターとまったく同じ動作 ([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="1f0ce-152">出力パラメーター</span><span class="sxs-lookup"><span data-stu-id="1f0ce-152">Output parameters</span></span>

<span data-ttu-id="1f0ce-153">宣言したパラメーター、`out`修飾子は、***出力パラメーター***します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="1f0ce-154">出力パラメーターは、新しい記憶域の場所を作成できません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="1f0ce-155">代わりに、出力パラメーターは、関数のメンバーまたはデリゲートの呼び出しで引数として指定された変数と同じストレージ場所を表します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="1f0ce-156">したがって、出力パラメーターの値は常に、基になる変数と同じです。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="1f0ce-157">次の確実な代入ルールは、出力パラメーターに適用されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="1f0ce-158">説明されている参照パラメーターのさまざまな規則に注意してください[パラメーターを参照して](variables.md#reference-parameters)します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="1f0ce-159">変数が必要ないに明示的に代入関数メンバーの出力パラメーターとして渡すことができますが、デリゲートの呼び出しまたは。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="1f0ce-160">関数メンバーまたはデリゲートの呼び出しが正常に完了した後、その実行パスと見なされます、出力パラメーターとして渡された各変数が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="1f0ce-161">関数メンバーまたは匿名関数の中で、出力パラメーターが最初に割り当てられていないと見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="1f0ce-162">関数のメンバーまたは匿名関数のすべての出力パラメーターを明示的に代入する必要があります ([確実な代入](variables.md#definite-assignment)) 関数の前にメンバーまたは匿名関数が通常どおり戻る。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="1f0ce-163">構造体の型のインスタンス コンス トラクター内で、`this`キーワードは構造体の型の出力パラメーターとまったく同じ動作 ([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="1f0ce-164">ローカル変数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-164">Local variables</span></span>

<span data-ttu-id="1f0ce-165">A***ローカル変数***で宣言されて、 *local_variable_declaration*で発生する可能性があります、*ブロック*、 *for_statement*、 *switch_statement*または*using_statement*または、 *foreach_statement*または*specific_catch_clause*をの *。try_statement*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="1f0ce-166">ローカル変数の有効期間は、プログラムの実行を予約する記憶域を保証する部分です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="1f0ce-167">この有効期間を拡張のエントリから、少なくとも、*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*それが関連付けられている、その実行まで*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*任意の方法で終了します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="1f0ce-168">(入力、囲まれた*ブロック*メソッドを呼び出すが、中断、終わらない、現在の実行または*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*)。匿名関数でローカル変数をキャプチャするかどうか ([外部変数をキャプチャ](expressions.md#captured-outer-variables))、その有効期間に付属するその他のオブジェクトと共に、匿名関数から作成されたデリゲートまたは式ツリーまで少なくとも拡張キャプチャされた変数への参照はガベージ コレクションの対象とします。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="1f0ce-169">場合、親*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*入力が再帰的に、ローカル変数の新しいインスタンスのたびが作成し、その*local_variable_initializer*、いずれかが評価される場合、各時間です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="1f0ce-170">導入されたローカル変数、 *local_variable_declaration*が自動的に初期化されていないと、そのため、既定値がありません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="1f0ce-171">確実な代入をチェックするためには、ローカル変数がで導入された、 *local_variable_declaration*は最初に割り当てられていないと見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="1f0ce-172">A *local_variable_declaration*含めることができます、 *local_variable_initializer*、その場合、変数と見なされます明示的に代入、初期化式の後にのみ ([宣言ステートメント](variables.md#declaration-statements))。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="1f0ce-173">導入されたローカル変数のスコープ内で、 *local_variable_declaration*、前にあるテキストの位置でそのローカル変数を参照すると、コンパイル時エラー、 *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="1f0ce-174">ローカル変数宣言に暗黙の型がある場合 ([ローカル変数の宣言](statements.md#local-variable-declarations))、内の変数に参照するとエラーもその*local_variable_declarator*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="1f0ce-175">導入されたローカル変数、 *foreach_statement*または*specific_catch_clause*全体のスコープに確実に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="1f0ce-176">ローカル変数の実際の有効期間は、実装によって異なります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="1f0ce-177">たとえば、コンパイラ可能性があります静的に決定できるブロック内のローカル変数は、そのブロックの小さな部分にのみ使用します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="1f0ce-178">この分析を使用して、コンパイラはその包含ブロックよりも短い有効期間を持つ変数の記憶域になるコードを生成可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="1f0ce-179">ローカル参照変数によって参照されるストレージはローカル参照変数の有効期間とは独立して再利用 ([自動メモリ管理](basic-concepts.md#automatic-memory-management))。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="1f0ce-180">既定の値</span><span class="sxs-lookup"><span data-stu-id="1f0ce-180">Default values</span></span>

<span data-ttu-id="1f0ce-181">次のカテゴリの変数は、既定値に自動的に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="1f0ce-182">静的変数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-182">Static variables.</span></span>
*  <span data-ttu-id="1f0ce-183">クラスのインスタンスのインスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="1f0ce-184">配列の要素。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-184">Array elements.</span></span>

<span data-ttu-id="1f0ce-185">変数の既定値は、変数の型に依存し、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="1f0ce-186">変数の*value_type*、既定値はによって計算された値と同じ、 *value_type*の既定のコンス トラクター ([既定のコンス トラクター](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="1f0ce-187">変数の*reference_type*、既定値は`null`します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="1f0ce-188">メモリ マネージャーによって既定値に初期化が普通または使用に割り当てられる前に、ガベージ コレクターがすべてのビットが 0 にメモリを初期化します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="1f0ce-189">このため、null 参照を表すすべてのビットが 0 を使用すると便利です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="1f0ce-190">確実な代入</span><span class="sxs-lookup"><span data-stu-id="1f0ce-190">Definite assignment</span></span>

<span data-ttu-id="1f0ce-191">関数メンバーの実行可能コード内の指定した位置に変数があると言います***確実に代入***、特定の静的フロー解析でコンパイラを証明できるかどうか ([確実なを決定するための厳密な規則割り当て](variables.md#precise-rules-for-determining-definite-assignment))、変数が自動的に初期化されるまたは少なくとも 1 つの割り当ての対象になっています。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="1f0ce-192">確実な代入ルールは、します非公式説明すると。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="1f0ce-193">最初に割り当て済みの変数 ([変数を最初に割り当てられた](variables.md#initially-assigned-variables)) 確実に割り当てられていると考えられます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="1f0ce-194">最初に未割り当ての変数を ([変数を最初に割り当てられていない](variables.md#initially-unassigned-variables)) と見なされます確実に割り当てられている特定の場所にその場所につながるすべての可能な実行パスでは、次の少なくとも 1 つ含まれている場合。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="1f0ce-195">単純な代入 ([単純な代入](expressions.md#simple-assignment)) で、変数は、左側のオペランド。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="1f0ce-196">Invocation 式 ([Invocation 式](expressions.md#invocation-expressions)) またはオブジェクトの作成式 ([オブジェクト作成式](expressions.md#object-creation-expressions))、出力パラメーターとして、変数に合格します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="1f0ce-197">ローカル変数、ローカル変数宣言 ([ローカル変数宣言](statements.md#local-variable-declarations)) 変数の初期化子が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="1f0ce-198">上記の非公式なルールを基になる正式な仕様については、「[変数最初に割り当てられている](variables.md#initially-assigned-variables)、[変数最初に割り当てられていない](variables.md#initially-unassigned-variables)、および[を決定するための厳密な規則確実な代入](variables.md#precise-rules-for-determining-definite-assignment)します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="1f0ce-199">インスタンス変数の確実な割り当ての状態、 *struct_type*変数が全体としても個別に追跡されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="1f0ce-200">上記の規則に、次の規則にも適用されます*struct_type*変数とそのインスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-200">In additional to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="1f0ce-201">インスタンス変数と見なされます確実に割り当てられている場合はそれを含んでいる*struct_type*変数が確実に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="1f0ce-202">A *struct_type*変数と見なされます確実に割り当てられている場合は確実に割り当てられているそれぞれのインスタンス変数と見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="1f0ce-203">確実な代入では、次のコンテキストでの要件を示します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="1f0ce-204">変数は、その値が取得される各場所では明示的に代入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="1f0ce-205">これにより、未定義の値が発生しません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="1f0ce-206">式内の変数の出現が場合を除き、変数の値を取得すると見なされます</span><span class="sxs-lookup"><span data-stu-id="1f0ce-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="1f0ce-207">変数は、単純な割り当ての左オペランドです。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="1f0ce-208">変数が出力パラメーターとして渡されたか、</span><span class="sxs-lookup"><span data-stu-id="1f0ce-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="1f0ce-209">変数は、 *struct_type*変数とメンバー アクセスの左のオペランドとして発生します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="1f0ce-210">変数は、明示的に参照パラメーターとして渡されますが、それぞれの場所に代入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="1f0ce-211">これにより、呼び出される関数メンバーが最初に割り当てられている参照パラメーターを検討できます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="1f0ce-212">関数メンバーを返しますの各場所で明示的関数メンバーのすべての出力パラメーターに代入する必要があります (を通じて、`return`ステートメントまたは実行関数メンバーの本文の終わりに達するまで)。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="1f0ce-213">これにより、関数メンバー未定義の値に返しません、出力パラメーター、変数への代入と等価の出力パラメーターとして変数を受け取る関数メンバーの呼び出しを考慮するコンパイラが有効にします。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="1f0ce-214">`this`の変数を*struct_type*インスタンス コンス トラクターをそのコンス トラクターを返しますの各場所で間違いなく割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="1f0ce-215">最初に割り当てられた変数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-215">Initially assigned variables</span></span>

<span data-ttu-id="1f0ce-216">次のカテゴリの変数は、初期割り当てられている分類されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="1f0ce-217">静的変数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-217">Static variables.</span></span>
*  <span data-ttu-id="1f0ce-218">クラスのインスタンスのインスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="1f0ce-219">最初に割り当てられている構造体の変数のインスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="1f0ce-220">配列の要素。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-220">Array elements.</span></span>
*  <span data-ttu-id="1f0ce-221">値のパラメーター。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-221">Value parameters.</span></span>
*  <span data-ttu-id="1f0ce-222">参照パラメーター。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-222">Reference parameters.</span></span>
*  <span data-ttu-id="1f0ce-223">宣言された変数を`catch`句または`foreach`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="1f0ce-224">最初に割り当てられていない変数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-224">Initially unassigned variables</span></span>

<span data-ttu-id="1f0ce-225">次のカテゴリの変数は、初期代入なしに分類されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="1f0ce-226">最初に割り当てられていない構造体の変数のインスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="1f0ce-227">出力パラメーター、`this`構造体のインスタンス コンス トラクターの変数。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="1f0ce-228">宣言されたローカル変数を除く、`catch`句または`foreach`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="1f0ce-229">確実な代入を決定するための正確な規則</span><span class="sxs-lookup"><span data-stu-id="1f0ce-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="1f0ce-230">を使用する各変数を明示的に代入を判断するために、コンパイラは、このセクションで説明したように相当するプロセスを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="1f0ce-231">コンパイラは、最初に割り当てられていない 1 つまたは複数の変数を持つ各関数メンバーの本文を処理します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="1f0ce-232">最初に割り当てられていない各変数の*v*、コンパイラが判断を***確実な代入状態***の*v*関数メンバーの次の点の各。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="1f0ce-233">各ステートメントの先頭に</span><span class="sxs-lookup"><span data-stu-id="1f0ce-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="1f0ce-234">最後の時点で ([エンドポイントと到達可能性](statements.md#end-points-and-reachability)) の各ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="1f0ce-235">それぞれの円弧の制御を移す別のステートメントまたはステートメントの終了点</span><span class="sxs-lookup"><span data-stu-id="1f0ce-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="1f0ce-236">各式の先頭に</span><span class="sxs-lookup"><span data-stu-id="1f0ce-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="1f0ce-237">各式の最後に</span><span class="sxs-lookup"><span data-stu-id="1f0ce-237">At the end of each expression</span></span>

<span data-ttu-id="1f0ce-238">確実な代入状態*v*いずれかになります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="1f0ce-239">明示的に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-239">Definitely assigned.</span></span> <span data-ttu-id="1f0ce-240">この時点ですべての可能な制御フローのことを示しますこれは、 *v*に値が代入されています。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="1f0ce-241">間違いなく割り当てられています。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-241">Not definitely assigned.</span></span> <span data-ttu-id="1f0ce-242">型の式の最後に変数の状態の`bool`、間違いなく月が割り当てられていない (ただし、必ずしも) 変数の状態は、次のサブ状態のいずれかに分類されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="1f0ce-243">間違いなく true 式の後に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="1f0ce-244">この状態では、ことを示します*v*ブール式が true として評価は、ブール式が false と評価された場合とは限りません割り当てられていない場合は確実に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="1f0ce-245">False の式の後に確実に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="1f0ce-246">この状態では、ことを示します*v*ブール式が false として評価は、ブール式が true と評価された場合とは限りません割り当てられていない場合は確実に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="1f0ce-247">次の規則が、どのように変数の状態*v*は場所ごとに決定されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="1f0ce-248">ステートメントの一般的な規則</span><span class="sxs-lookup"><span data-stu-id="1f0ce-248">General rules for statements</span></span>

*  <span data-ttu-id="1f0ce-249">*v*関数メンバーの本文の先頭に確実に代入できません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="1f0ce-250">*v*が確実に到達不可能なステートメントの先頭に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="1f0ce-251">確実な代入状態*v*の確実な割り当ての状態をチェックして、他のステートメントの先頭に決定されます*v*の先頭を対象とするすべてのコントロール フロー転送ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="1f0ce-252">場合 (および場合にのみ) *v*し、割り当て、このようなすべてのコントロールのフローの転送に間違いなく*v*が確実に割り当て、ステートメントの先頭にします。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="1f0ce-253">ステートメントの到達可能性のチェックと同じ方法で移動する可能性のある制御フローのセットが決定されます ([エンドポイントと到達可能性](statements.md#end-points-and-reachability))。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="1f0ce-254">確実な代入状態*v*ブロックの終了点で`checked`、 `unchecked`、 `if`、 `while`、 `do`、 `for`、 `foreach`、 `lock`、 `using`、または`switch`の確実な割り当ての状態を確認してステートメントが決定されます*v*をそのステートメントの終点を対象とするすべてのコントロール フロー転送します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="1f0ce-255">場合*v*が確実に割り当て、このようなすべてのコントロールのフローの転送で、 *v*が確実に割り当て、ステートメントの終了時点。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="1f0ce-256">それ以外の場合。*v*ステートメントの終了時点で確実に代入できません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="1f0ce-257">ステートメントの到達可能性のチェックと同じ方法で移動する可能性のある制御フローのセットが決定されます ([エンドポイントと到達可能性](statements.md#end-points-and-reachability))。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="1f0ce-258">オンになっている、ブロックのステートメントと unchecked ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="1f0ce-259">確実な代入状態*v*ブロックにおけるステートメント リストの最初のステートメント (またはステートメントの一覧が空の場合は、ブロックの終了点) の転送では、コントロールがの確実な代入ステートメントと同じ*v* 、ブロックの前に`checked`、または`unchecked`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="1f0ce-260">式ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-260">Expression statements</span></span>

<span data-ttu-id="1f0ce-261">式ステートメントの*stmt*式で構成される*expr*:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="1f0ce-262">*v*の先頭に同じ状態の確実な代入*expr*の先頭として*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-263">場合*v*の最後に確実に割り当てられている場合*expr*、間違いなくの終了ポイントに割り当てられている*stmt*それの終点で間違いなく割り当てがありません*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="1f0ce-264">宣言ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-264">Declaration statements</span></span>

*  <span data-ttu-id="1f0ce-265">場合*stmt*宣言のステートメントは、初期化子のない、 *v*の終了時点で確実な代入状態の同じ*stmt* の先頭として*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-266">場合*stmt*と初期化子、宣言ステートメントは次の明確な割り当ての状態は、 *v*決定場合と*stmt*が 1 つの割り当てと、ステートメントの一覧(宣言) の順序で初期化子による各宣言ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="1f0ce-267">場合ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-267">If statements</span></span>

<span data-ttu-id="1f0ce-268">`if`ステートメント*stmt*の形式。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="1f0ce-269">*v*の先頭に同じ状態の確実な代入*expr*の先頭として*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-270">場合*v*の最後に明示的に代入が*expr*、確実に制御フローの移動で割り当てられているし、 *then_stmt*とするか、 *else_stmt*またはのエンドポイントに*stmt* else 句がない場合。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="1f0ce-271">場合*v*の末尾に「間違いなく true 式の後に割り当て済み」の状態を持つ*expr*、確実に制御フローの移動で割り当てられているし、 *then_stmt*、および not間違いなくいずれかを制御フローの移動で割り当てられている*else_stmt*またはのエンドポイントに*stmt* else 句がない場合。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="1f0ce-272">場合*v*の末尾に「を代入式が false の後に」の状態を持つ*expr*、確実に制御フローの移動で割り当てられているし、 *else_stmt*、および not制御フローの移動の代入*then_stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="1f0ce-273">間違いなくのエンドポイントに割り当てられている*stmt*間違いなくのエンドポイントに割り当てられている場合にのみ*then_stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="1f0ce-274">それ以外の場合、 *v*はいないと見なされるいずれかを制御フローの転送を確実に割り当てられた、 *then_stmt*または*else_stmt*、またはのエンドポイントに*stmt* else 句がない場合。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="1f0ce-275">Switch ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-275">Switch statements</span></span>

<span data-ttu-id="1f0ce-276">`switch`ステートメント*stmt*制御式と*expr*:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="1f0ce-277">確実な代入状態*v*の先頭に*expr*の状態と同じでは、 *v*の先頭に*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-278">確実な代入状態*v*到達可能なスイッチ ブロック ステートメントの一覧への転送では、制御フローでが確実な割り当ての状態のと同じ*v*の最後に*expr*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="1f0ce-279">While ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-279">While statements</span></span>

<span data-ttu-id="1f0ce-280">`while`ステートメント*stmt*の形式。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="1f0ce-281">*v*の先頭に同じ状態の確実な代入*expr*の先頭として*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-282">場合*v*の最後に明示的に代入が*expr*、確実に制御フローの移動で割り当てられているし、 *while_body*と終点を*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-283">場合*v*の末尾に「間違いなく true 式の後に割り当て済み」の状態を持つ*expr*、確実に制御フローの移動で割り当てられているし、 *while_body*がありませんエンドポイントで確実に代入*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-284">場合*v*の末尾に「を代入式が false の後に」の状態を持つ*expr*、間違いなくの終了点への制御フローの移動で割り当てられているし、 *stmt*、への制御フローの移動では間違いなく割り当てられていませんが、 *while_body*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="1f0ce-285">Do ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-285">Do statements</span></span>

<span data-ttu-id="1f0ce-286">`do`ステートメント*stmt*の形式。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="1f0ce-287">*v*の先頭からの制御フローの移動で同じ状態の確実な代入*stmt*に*do_body*の先頭として*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-288">*v*の先頭に同じ状態の確実な代入*expr*の終了時点として*do_body*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="1f0ce-289">場合*v*の最後に明示的に代入が*expr*、間違いなくの終了点への制御フローの移動で割り当てられているし、 *stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-290">場合*v*の末尾に「を代入式が false の後に」の状態を持つ*expr*、間違いなくの終了点への制御フローの移動で割り当てられているし、 *stmt*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="1f0ce-291">ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-291">For statements</span></span>

<span data-ttu-id="1f0ce-292">確実な代入のチェック、`for`形式のステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="1f0ce-293">ステートメントが記述された場合とが行われます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="1f0ce-294">場合、 *for_condition*から省略すると、`for`ステートメント、し、確実な代入処理の進行状況の評価として*for_condition*に置き換えられました`true`で上記の拡張.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="1f0ce-295">中断、続行、および goto ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="1f0ce-296">確実な代入状態*v*による制御フローの転送、 `break`、 `continue`、または`goto`ステートメントが確実な割り当ての状態のと同じ*v*で、ステートメントの先頭。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="1f0ce-297">Throw ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-297">Throw statements</span></span>

<span data-ttu-id="1f0ce-298">ステートメントの*stmt*のフォーム</span><span class="sxs-lookup"><span data-stu-id="1f0ce-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="1f0ce-299">確実な代入状態*v*の先頭に*expr*の確実な割り当ての状態と同じ*v*の先頭に*stmt*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="1f0ce-300">Return ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-300">Return statements</span></span>

<span data-ttu-id="1f0ce-301">ステートメントの*stmt*のフォーム</span><span class="sxs-lookup"><span data-stu-id="1f0ce-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="1f0ce-302">確実な代入状態*v*の先頭に*expr*の確実な割り当ての状態と同じ*v*の先頭に*stmt*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-303">場合*v*が出力パラメーターでは、間違いなく割り当てる必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="1f0ce-304">後*expr*</span><span class="sxs-lookup"><span data-stu-id="1f0ce-304">after *expr*</span></span>
    * <span data-ttu-id="1f0ce-305">またはの最後に、`finally`のブロックを`try` - `finally`または`try` - `catch` - `finally`を囲む、`return`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="1f0ce-306">形式のステートメント stmt:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="1f0ce-307">場合*v*が出力パラメーターでは、間違いなく割り当てる必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="1f0ce-308">前に*stmt*</span><span class="sxs-lookup"><span data-stu-id="1f0ce-308">before *stmt*</span></span>
    * <span data-ttu-id="1f0ce-309">またはの最後に、`finally`のブロックを`try` - `finally`または`try` - `catch` - `finally`を囲む、`return`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="1f0ce-310">Try-catch ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-310">Try-catch statements</span></span>

<span data-ttu-id="1f0ce-311">ステートメントの*stmt*の形式。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="1f0ce-312">確実な代入状態*v*の先頭に*try_block*の確実な割り当ての状態と同じ*v*の先頭に*stmt*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-313">確実な代入状態*v*の先頭に*catch_block_i* (いずれかの*は*) の明確な割り当ての状態と同じ*v*の先頭に*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-314">確実な代入状態*v*のエンドポイントで*stmt*は確実に割り当てられている場合 (および場合にのみ) *v*のエンドポイントで確実に代入が*try_block* 、毎回*catch_block_i* (のすべて*は*1 ~ *n*)。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="1f0ce-315">Try finally ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-315">Try-finally statements</span></span>

<span data-ttu-id="1f0ce-316">`try`ステートメント*stmt*の形式。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="1f0ce-317">確実な代入状態*v*の先頭に*try_block*の確実な割り当ての状態と同じ*v*の先頭に*stmt*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-318">確実な代入状態*v*の先頭に*finally_block*の確実な割り当ての状態と同じ*v*の先頭に*stmt*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-319">確実な代入状態*v*のエンドポイントで*stmt*は確実に割り当てられている場合 (および場合にのみ)、次の少なくとも 1 つが true:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="1f0ce-320">*v*のエンドポイントで確実に代入が*try_block*</span><span class="sxs-lookup"><span data-stu-id="1f0ce-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="1f0ce-321">*v*のエンドポイントで確実に代入が*finally_block*</span><span class="sxs-lookup"><span data-stu-id="1f0ce-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="1f0ce-322">制御フローの転送の場合 (など、`goto`ステートメント) が行われた内で開始*try_block*、以外の終了と*try_block*、し*v*も場合、その制御フローの転送を確実に割り当てられていると見なされます*v*のエンドポイントで確実に代入が*finally_block*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="1f0ce-323">(これは、場合にのみではありません — 場合*v*が間違いなくこの制御フローの転送時に別の理由を割り当てられて明示的に代入みなされますし、)。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="1f0ce-324">Try – catch – finally ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-324">Try-catch-finally statements</span></span>

<span data-ttu-id="1f0ce-325">分析を確実な代入を`try` - `catch` - `finally`形式のステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="1f0ce-326">場合、ステートメントが同じように行われますが、 `try` - `finally`ステートメントを囲む、 `try` - `catch`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="1f0ce-327">次の例で方法の異なるブロック、`try`ステートメント ([try ステートメント](statements.md#the-try-statement)) に影響する確実な代入です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
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

#### <a name="foreach-statements"></a><span data-ttu-id="1f0ce-328">Foreach ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-328">Foreach statements</span></span>

<span data-ttu-id="1f0ce-329">`foreach`ステートメント*stmt*の形式。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="1f0ce-330">確実な代入状態*v*の先頭に*expr*の状態と同じでは、 *v*の先頭に*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-331">確実な代入状態*v*への制御フローの移動で*embedded_statement*またはの終点を*stmt*の状態と同じでは、 *v*の最後に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="1f0ce-332">ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-332">Using statements</span></span>

<span data-ttu-id="1f0ce-333">`using`ステートメント*stmt*の形式。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="1f0ce-334">確実な代入状態*v*の先頭に*resource_acquisition*の状態と同じでは、 *v*の先頭に*stmt*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-335">確実な代入状態*v*への制御フローの移動で*embedded_statement*の状態と同じでは、 *v*の最後に*resource_買収*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="1f0ce-336">Lock ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-336">Lock statements</span></span>

<span data-ttu-id="1f0ce-337">`lock`ステートメント*stmt*の形式。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="1f0ce-338">確実な代入状態*v*の先頭に*expr*の状態と同じでは、 *v*の先頭に*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-339">確実な代入状態*v*への制御フローの移動で*embedded_statement*の状態と同じでは、 *v*の最後に*expr*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="1f0ce-340">Yield ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f0ce-340">Yield statements</span></span>

<span data-ttu-id="1f0ce-341">`yield return`ステートメント*stmt*の形式。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="1f0ce-342">確実な代入状態*v*の先頭に*expr*の状態と同じでは、 *v*の先頭に*stmt*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="1f0ce-343">確実な代入状態*v*の最後に*stmt*の状態と同じでは、 *v*の最後に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="1f0ce-344">A`yield break`ステートメントが確実な代入の状態に影響を与えません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="1f0ce-345">単純式での一般的な規則</span><span class="sxs-lookup"><span data-stu-id="1f0ce-345">General rules for simple expressions</span></span>

<span data-ttu-id="1f0ce-346">これらの種類の式に、次の規則が適用されますリテラル ([リテラル](expressions.md#literals))、単純な名前 ([簡易名](expressions.md#simple-names))、メンバー アクセス式 ([メンバー アクセス](expressions.md#member-access))、。インデックスのないベース アクセス式 ([Base アクセス](expressions.md#base-access))、`typeof`式 ([typeof 演算子](expressions.md#the-typeof-operator))、既定の値式 ([既定の値式](expressions.md#default-value-expressions)) と`nameof`式 ([Nameof 式](expressions.md#nameof-expressions))。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="1f0ce-347">確実な代入状態*v*はの確実な割り当ての状態と同じような式の末尾に*v*式の先頭にします。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="1f0ce-348">埋め込み式を持つ式の一般的な規則</span><span class="sxs-lookup"><span data-stu-id="1f0ce-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="1f0ce-349">これらの種類の式に、次の規則が適用されます: かっこで囲まれた式 ([かっこで囲まれた式](expressions.md#parenthesized-expressions))、要素アクセス式 ([要素へのアクセス](expressions.md#element-access))、基本アクセスを含む式インデックスの作成 ([Base アクセス](expressions.md#base-access))、インクリメントおよびデクリメント式 ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、[前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))、キャスト式 ([キャスト式](expressions.md#cast-expressions))、単項`+`、 `-`、 `~`、`*`式、バイナリ`+`、 `-`、 `*`、`/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`、 `|`、`^`式 ([算術演算子](expressions.md#arithmetic-operators)、[シフト演算子](expressions.md#shift-operators)、[関係式と型検査演算子](expressions.md#relational-and-type-testing-operators)、[論理演算子](expressions.md#logical-operators))、複合代入式 ([複合代入](expressions.md#compound-assignment))、`checked`と`unchecked`式 ([checked と unchecked演算子](expressions.md#the-checked-and-unchecked-operators))、さらに配列とデリゲート作成式 ([new 演算子](expressions.md#the-new-operator))。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="1f0ce-350">それぞれの式は、無条件に一定の順序で評価される 1 つまたは複数のサブ式があります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="1f0ce-351">たとえば、バイナリ`%`演算子を評価し、演算子の左側、右側にあります。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="1f0ce-352">インデックス作成操作では、インデックス付きの式を評価しの各インデックス式、順序は左から右に評価し、します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="1f0ce-353">式の*expr*、サブ式を持つ*e1、e2、…、eN*、その順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="1f0ce-354">確実な代入状態*v*の先頭に*e1*の先頭に明確な割り当ての状態と同じ*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="1f0ce-355">確実な代入状態*v*の先頭に*ei* (*は*1 より大きい) 以前のサブ式の末尾に明確な割り当ての状態の場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="1f0ce-356">確実な代入状態*v*の最後に*expr*の最後に明確な割り当ての状態と同じ*eN*</span><span class="sxs-lookup"><span data-stu-id="1f0ce-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="1f0ce-357">Invocation 式とオブジェクト作成式</span><span class="sxs-lookup"><span data-stu-id="1f0ce-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="1f0ce-358">Invocation 式の*expr*の形式。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="1f0ce-359">または、フォームのオブジェクト作成式:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="1f0ce-360">Invocation 式での確実な割り当ての状態の*v*する前に*primary_expression*の状態と同じでは、 *v*する前に*expr*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1f0ce-361">Invocation 式での確実な割り当ての状態の*v*する前に*arg1*の状態と同じでは、 *v*後*primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="1f0ce-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="1f0ce-362">オブジェクト作成式での確実な割り当ての状態を*v*する前に*arg1*の状態と同じでは、 *v*する前に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1f0ce-363">各引数の*argi*の確実な代入状態*v*後*argi*は無視して、通常の式の規則によって決まります`ref`または`out`修飾子。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="1f0ce-364">各引数の*argi*は*は*1 の確実な割り当ての状態よりも大きい*v*する前に*argi*の状態の場合と同じです*v*前後*arg*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="1f0ce-365">場合、変数*v*として渡される、`out`引数 (つまり、フォームの引数`out v`)、引数は、次の状態のいずれかで*v*後*expr*間違いなく割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="1f0ce-366">それ以外の場合。状態*v*後*expr*の状態と同じでは、 *v*後*argN*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="1f0ce-367">配列初期化子の ([配列作成式](expressions.md#array-creation-expressions))、オブジェクト初期化子 ([オブジェクト初期化子](expressions.md#object-initializers))、コレクション初期化子 ([コレクション初期化子](expressions.md#collection-initializers)) と匿名オブジェクト初期化子 ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions))、これらのコンストラクトがの観点で定義されている拡張によって明確な割り当ての状態が決定されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="1f0ce-368">単純な代入式</span><span class="sxs-lookup"><span data-stu-id="1f0ce-368">Simple assignment expressions</span></span>

<span data-ttu-id="1f0ce-369">式の*expr*フォームの`w = expr_rhs`:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="1f0ce-370">確実な代入状態*v*する前に*expr_rhs*の確実な割り当ての状態と同じ*v*する前に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1f0ce-371">確実な代入状態*v*後*expr*によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="1f0ce-372">場合*w*として変数が同じ*v*の確実な代入状態*v*後*expr*は確実に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="1f0ce-373">場合、構造体の型のインスタンス コンス トラクター内で、割り当てが行われた場合*w* 、自動的に実装されたプロパティを指定するプロパティへのアクセスは、 *P*で構築されるインスタンス*v*の非表示のバッキング フィールドは、 *P*の確実な代入状態*v*後*expr*は間違いなく割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="1f0ce-374">それ以外の場合の確実な代入状態*v*後*expr*の確実な割り当ての状態と同じ*v*後*expr_rhs*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="1f0ce-375">& & (AND 条件付き) 式</span><span class="sxs-lookup"><span data-stu-id="1f0ce-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="1f0ce-376">式の*expr*フォームの`expr_first && expr_second`:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="1f0ce-377">確実な代入状態*v*する前に*expr_first*の確実な割り当ての状態と同じ*v*する前に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1f0ce-378">確実な代入状態*v*する前に*expr_second*場合が確実に割り当ての状態*v*後*expr_first*いずれかです明示的に代入または「間違いなく true 式の後に割り当てられている」します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="1f0ce-379">それ以外の場合、これは間違いなく割り当てられません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="1f0ce-380">確実な代入状態*v*後*expr*によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="1f0ce-381">場合*expr_first*の定数式の値では、`false`の確実な代入状態*v*後*expr*確実な代入と同じです状態の*v*後*expr_first*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="1f0ce-382">の場合の状態*v*後*expr_first*が確実に割り当て、次の状態*v*後*expr*は確実に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1f0ce-383">の場合の状態*v*後*expr_second*が確実に割り当て、および状態の*v*後*expr_first*は"間違いなく割り当てられた式が false の後に"、その後の状態*v*後*expr*は確実に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1f0ce-384">の場合、状態の*v*後*expr_second*が確実に割り当てまたは「明示的に代入式が true の後に」後の状態*v*後*expr* 「は確実に代入式の true」です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="1f0ce-385">の場合、状態の*v*後*expr_first* 「明示的に代入式が false の後に」、"と"の状態は、 *v*後*expr_second* 「明示的に代入式が false の後に」、その後の状態は、 *v*後*expr* 「は確実に代入式が false の後に」。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="1f0ce-386">それ以外の場合、状態の*v*後*expr*が確実に割り当てできません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="1f0ce-387">例</span><span class="sxs-lookup"><span data-stu-id="1f0ce-387">In the example</span></span>
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
<span data-ttu-id="1f0ce-388">変数`i`の埋め込みステートメントのいずれかで確実に割り当てられていると見なされます、`if`ステートメントが他にない。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="1f0ce-389">`if`メソッドでステートメント`F`、変数`i`ために埋め込みの最初のステートメントで間違いなく割り当てられた式の実行`(i = y)`この埋め込みステートメントの実行の前に必ずします。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="1f0ce-390">これに対して、変数`i`が間違いなく割り当てられていない 2 番目の埋め込みステートメントの後`x >= 0`可能性がありますの検査が false の場合、変数に`i`未割り当て。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="1f0ce-391">||(条件付き OR) 式</span><span class="sxs-lookup"><span data-stu-id="1f0ce-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="1f0ce-392">式の*expr*フォームの`expr_first || expr_second`:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="1f0ce-393">確実な代入状態*v*する前に*expr_first*の確実な割り当ての状態と同じ*v*する前に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1f0ce-394">確実な代入状態*v*する前に*expr_second*場合が確実に割り当ての状態*v*後*expr_first*いずれかです明示的に代入または「false 式の後に間違いなく割り当て済み」です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="1f0ce-395">それ以外の場合、これは間違いなく割り当てられません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="1f0ce-396">確実な代入ステートメントの*v*後*expr*によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="1f0ce-397">場合*expr_first*の定数式の値では、`true`の確実な代入状態*v*後*expr*確実な代入と同じです状態の*v*後*expr_first*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="1f0ce-398">の場合の状態*v*後*expr_first*が確実に割り当て、次の状態*v*後*expr*は確実に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1f0ce-399">の場合の状態*v*後*expr_second*が確実に割り当て、および状態の*v*後*expr_first*は"間違いなく割り当てられている場合は true。 式の後に"、その後の状態*v*後*expr*は確実に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1f0ce-400">の場合、状態の*v*後*expr_second*が確実に割り当てまたは「明示的に代入式が false の後に」後の状態*v* 後*expr* 「は確実に代入式が false の後に」。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="1f0ce-401">の場合、状態の*v*後*expr_first* 「明示的に代入式の true」、"と"の状態は、 *v*後*expr_second*「明示的に代入式の true」、その後の状態は、 *v*後*expr* 「は確実に代入式の true」です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="1f0ce-402">それ以外の場合、状態の*v*後*expr*が確実に割り当てできません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="1f0ce-403">例</span><span class="sxs-lookup"><span data-stu-id="1f0ce-403">In the example</span></span>
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
<span data-ttu-id="1f0ce-404">変数`i`の埋め込みステートメントのいずれかで確実に割り当てられていると見なされます、`if`ステートメントが他にない。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="1f0ce-405">`if`メソッドでステートメント`G`、変数`i`ために 2 番目の埋め込みステートメントで間違いなく割り当てられた式の実行`(i = y)`この埋め込みステートメントの実行の前に必ずします。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="1f0ce-406">これに対して、変数`i`が間違いなく割り当てられていない最初の埋め込みステートメントの後`x >= 0`可能性がありますの検査が true の場合、変数に`i`未割り当て。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="1f0ce-407">!</span><span class="sxs-lookup"><span data-stu-id="1f0ce-407">!</span></span> <span data-ttu-id="1f0ce-408">(論理否定) 式</span><span class="sxs-lookup"><span data-stu-id="1f0ce-408">(logical negation) expressions</span></span>

<span data-ttu-id="1f0ce-409">式の*expr*フォームの`! expr_operand`:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="1f0ce-410">確実な代入状態*v*する前に*expr_operand*の確実な割り当ての状態と同じ*v*する前に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1f0ce-411">確実な代入状態*v*後*expr*によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="1f0ce-412">場合の状態*v*後 \* expr_operand \* が確実に割り当て、次の状態*v*後*expr*は確実に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1f0ce-413">場合の状態*v*後 \* expr_operand \* が間違いなく割り当てられていない、状態の*v*後*expr*が確実に割り当てできません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="1f0ce-414">場合の状態*v*後 \* expr_operand *「明示的に代入式が false の後に」、その後の状態は、 *v*後*expr\* "は確実に代入後 true式"です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="1f0ce-415">場合の状態*v*後 \* expr_operand *"は true 式の後には割り当て間違いなく、"次の状態は、 *v*後*expr\* "は確実に代入後 false。式"です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="1f0ce-416">??</span><span class="sxs-lookup"><span data-stu-id="1f0ce-416">??</span></span> <span data-ttu-id="1f0ce-417">式の (null 合体演算子)</span><span class="sxs-lookup"><span data-stu-id="1f0ce-417">(null coalescing) expressions</span></span>

<span data-ttu-id="1f0ce-418">式の*expr*フォームの`expr_first ?? expr_second`:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="1f0ce-419">確実な代入状態*v*する前に*expr_first*の確実な割り当ての状態と同じ*v*する前に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1f0ce-420">確実な代入状態*v*する前に*expr_second*の確実な割り当ての状態と同じ*v*後*expr_first*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="1f0ce-421">確実な代入ステートメントの*v*後*expr*によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="1f0ce-422">場合*expr_first*定数式です ([定数式](expressions.md#constant-expressions))、null 値を持つ、状態の*v*後*expr*は同じ状態と*v*後*expr_second*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="1f0ce-423">それ以外の場合、状態の*v*後*expr*の確実な代入状態と同じでは、 *v*後*expr_first*。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="1f0ce-424">?: (条件) 式</span><span class="sxs-lookup"><span data-stu-id="1f0ce-424">?: (conditional) expressions</span></span>

<span data-ttu-id="1f0ce-425">式の*expr*フォームの`expr_cond ? expr_true : expr_false`:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="1f0ce-426">確実な代入状態*v*する前に*expr_cond*の状態と同じでは、 *v*する前に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="1f0ce-427">確実な代入状態*v*する前に*expr_true*次のいずれかを保持する場合にのみが確実に割り当て。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="1f0ce-428">*expr_cond*値を持つ定数式です `false`</span><span class="sxs-lookup"><span data-stu-id="1f0ce-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="1f0ce-429">状態*v*後*expr_cond*が明示的に代入するか、「間違いなく true 式の後に割り当てられている」。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="1f0ce-430">確実な代入状態*v*する前に*expr_false*次のいずれかを保持する場合にのみが確実に割り当て。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="1f0ce-431">*expr_cond*値を持つ定数式です `true`</span><span class="sxs-lookup"><span data-stu-id="1f0ce-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="1f0ce-432">状態*v*後*expr_cond*が明示的に代入するか、「を代入式が false の後に」。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="1f0ce-433">確実な代入状態*v*後*expr*によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="1f0ce-434">場合*expr_cond*定数式です ([定数式](expressions.md#constant-expressions)) 値を持つ`true`の状態、 *v*後*expr*状態と同じでは、 *v*後*expr_true*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="1f0ce-435">の場合*expr_cond*定数式です ([定数式](expressions.md#constant-expressions)) 値を持つ`false`の状態、 *v*後*expr*の状態と同じでは、 *v*後*expr_false*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="1f0ce-436">の場合、状態の*v*後*expr_true*が確実に割り当ての状態と*v*後*expr_false*は間違いなく状態、割り当てられた*v*後*expr*は確実に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="1f0ce-437">それ以外の場合、状態の*v*後*expr*が確実に割り当てできません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="1f0ce-438">匿名関数</span><span class="sxs-lookup"><span data-stu-id="1f0ce-438">Anonymous functions</span></span>

<span data-ttu-id="1f0ce-439">*Lambda_expression*または*anonymous_method_expression* *expr*の本文 (か*ブロック*または*式*)*本文*:</span><span class="sxs-lookup"><span data-stu-id="1f0ce-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="1f0ce-440">外部変数の確実な代入状態*v*する前に*本文*の状態と同じでは、 *v*する前に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="1f0ce-441">つまり、外部変数の状態を確実な代入では、匿名関数のコンテキストから継承されます。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="1f0ce-442">外部変数の確実な代入状態*v*後*expr*の状態と同じでは、 *v*する前に*expr*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="1f0ce-443">例では、</span><span class="sxs-lookup"><span data-stu-id="1f0ce-443">The example</span></span>
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
<span data-ttu-id="1f0ce-444">以降のコンパイル時エラーが生成されます`max`が間違いなく割り当てられていない匿名関数が宣言されています。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="1f0ce-445">例では、</span><span class="sxs-lookup"><span data-stu-id="1f0ce-445">The example</span></span>
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
<span data-ttu-id="1f0ce-446">代入後も、コンパイル時エラーが生成されます`n`匿名関数に効力はなくなりましたの確実な代入状態`n`匿名関数の外側です。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="1f0ce-447">変数参照</span><span class="sxs-lookup"><span data-stu-id="1f0ce-447">Variable references</span></span>

<span data-ttu-id="1f0ce-448">A *variable_reference*は、*式*変数として分類します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="1f0ce-449">A *variable_reference*を現在の値をフェッチして新しい値を格納する両方にアクセスできる記憶域の場所を表します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="1f0ce-450">C とC++、 *variable_reference*と呼ばれますが、*左辺値*します。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="1f0ce-451">変数参照の原子性</span><span class="sxs-lookup"><span data-stu-id="1f0ce-451">Atomicity of variable references</span></span>

<span data-ttu-id="1f0ce-452">次のデータ型の読み取りや書き込みはアトミック: `bool`、 `char`、 `byte`、 `sbyte`、 `short`、 `ushort`、 `uint`、 `int`、`float`と参照型。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="1f0ce-453">さらに、上記の一覧では、基になる型と列挙型の読み取りや書き込みもアトミックです。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="1f0ce-454">など、他の型の読み書き`long`、 `ulong`、 `double`、および`decimal`とユーザー定義の型がアトミックである保証されません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="1f0ce-455">目的のために設計されたライブラリ関数を除けば、インクリメントまたはデクリメントの場合など、分割不可能な読み取り/変更/書き込みの保証はありません。</span><span class="sxs-lookup"><span data-stu-id="1f0ce-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

