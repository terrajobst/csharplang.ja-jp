---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704038"
---
# <a name="statements"></a><span data-ttu-id="51045-101">ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-101">Statements</span></span>

<span data-ttu-id="51045-102">C#には、さまざまなステートメントが用意されています。</span><span class="sxs-lookup"><span data-stu-id="51045-102">C# provides a variety of statements.</span></span> <span data-ttu-id="51045-103">これらのステートメントのほとんどは、C およびC++でプログラミングされている開発者にとってはよく知られています。</span><span class="sxs-lookup"><span data-stu-id="51045-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

<span data-ttu-id="51045-104">*Embedded_statement*非終端要素は、他のステートメント内に出現するステートメントに使用されます。</span><span class="sxs-lookup"><span data-stu-id="51045-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="51045-105">*ステートメント*ではなく*embedded_statement*を使用すると、これらのコンテキストで宣言ステートメントとラベル付きステートメントの使用が除外されます。</span><span class="sxs-lookup"><span data-stu-id="51045-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="51045-106">例</span><span class="sxs-lookup"><span data-stu-id="51045-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="51045-107">`if` ステートメントでは、if 分岐の*ステートメント*ではなく*embedded_statement*が必要になるため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="51045-108">このコードが許可された場合、 `i`変数は宣言されますが、使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="51045-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="51045-109">ただし、の宣言をブロックに`i`配置すると、この例は有効です。</span><span class="sxs-lookup"><span data-stu-id="51045-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="51045-110">エンドポイントと到達可能性</span><span class="sxs-lookup"><span data-stu-id="51045-110">End points and reachability</span></span>

<span data-ttu-id="51045-111">すべてのステートメントに***エンドポイント***があります。</span><span class="sxs-lookup"><span data-stu-id="51045-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="51045-112">直感的に言うと、ステートメントの終了点は、ステートメントの直後にある場所です。</span><span class="sxs-lookup"><span data-stu-id="51045-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="51045-113">複合ステートメント (埋め込みステートメントを含むステートメント) の実行ルールでは、コントロールが埋め込みステートメントのエンドポイントに到達したときに実行されるアクションを指定します。</span><span class="sxs-lookup"><span data-stu-id="51045-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="51045-114">たとえば、コントロールがブロック内のステートメントの終了位置に到達すると、コントロールはブロック内の次のステートメントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="51045-115">実行によってステートメントに到達できる場合は、ステートメントが***到達***可能であると言います。</span><span class="sxs-lookup"><span data-stu-id="51045-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="51045-116">逆に、ステートメントが実行される可能性がない場合、ステートメントは "***到達不能***" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="51045-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="51045-117">この例では、</span><span class="sxs-lookup"><span data-stu-id="51045-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="51045-118">の2番目`Console.WriteLine`の呼び出しは、ステートメントが実行される可能性がないため、到達できません。</span><span class="sxs-lookup"><span data-stu-id="51045-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="51045-119">ステートメントに到達できないとコンパイラが判断した場合、警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="51045-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="51045-120">具体的には、ステートメントに到達できない場合のエラーではありません。</span><span class="sxs-lookup"><span data-stu-id="51045-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="51045-121">特定のステートメントまたはエンドポイントに到達できるかどうかを判断するために、コンパイラは、ステートメントごとに定義された到達可能性規則に従ってフロー分析を実行します。</span><span class="sxs-lookup"><span data-stu-id="51045-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="51045-122">フロー分析では、ステートメントの動作を制御する定数式 ([定数式](expressions.md#constant-expressions)) の値が考慮されますが、非定数式で使用できる値は考慮されません。</span><span class="sxs-lookup"><span data-stu-id="51045-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="51045-123">つまり、制御フロー分析の目的で、指定された型の非定数式は、その型の可能な値を持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="51045-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="51045-124">この例では、</span><span class="sxs-lookup"><span data-stu-id="51045-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="51045-125">`if`ステートメントのブール式は、 `==`演算子の両方のオペランドが定数であるため、定数式です。</span><span class="sxs-lookup"><span data-stu-id="51045-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="51045-126">定数式がコンパイル時に評価され、値`false` `Console.WriteLine`が生成されると、呼び出しは到達不能と見なされます。</span><span class="sxs-lookup"><span data-stu-id="51045-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="51045-127">ただし、が`i`ローカル変数に変更された場合は、</span><span class="sxs-lookup"><span data-stu-id="51045-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="51045-128">`Console.WriteLine`呼び出しは到達可能と見なされますが、実際には実行されません。</span><span class="sxs-lookup"><span data-stu-id="51045-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="51045-129">関数メンバーの*ブロック*は常に到達可能と見なされます。</span><span class="sxs-lookup"><span data-stu-id="51045-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="51045-130">ブロック内の各ステートメントの到達可能性規則を連続して評価することにより、特定のステートメントの到達可能性を特定できます。</span><span class="sxs-lookup"><span data-stu-id="51045-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="51045-131">この例では、</span><span class="sxs-lookup"><span data-stu-id="51045-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="51045-132">2番目`Console.WriteLine`の到達可能性は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="51045-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="51045-133">最初`Console.WriteLine`の式ステートメントに到達できるのは、 `F`メソッドのブロックに到達可能であるためです。</span><span class="sxs-lookup"><span data-stu-id="51045-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="51045-134">最初`Console.WriteLine`の式ステートメントの終点に到達できるのは、そのステートメントに到達可能であるためです。</span><span class="sxs-lookup"><span data-stu-id="51045-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="51045-135">`if` 最初`Console.WriteLine`の式ステートメントの終点に到達可能であるため、ステートメントに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="51045-136">ステートメントのブール式に定数値が指定`Console.WriteLine` `false`されていないため、2番目の式ステートメントに到達できます。 `if`</span><span class="sxs-lookup"><span data-stu-id="51045-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="51045-137">ステートメントのエンドポイントが到達可能になるには、コンパイル時にエラーが発生するという2つの状況があります。</span><span class="sxs-lookup"><span data-stu-id="51045-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="51045-138">`switch`ステートメントでは、switch セクションが次の switch セクションに "フォールスルー" されることは許可されていないため、switch セクションのステートメントリストのエンドポイントが到達可能であると、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="51045-139">このエラーが発生した場合は、通常、 `break`ステートメントが存在しないことを示しています。</span><span class="sxs-lookup"><span data-stu-id="51045-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="51045-140">これは、到達可能な値を計算する関数メンバーのブロックのエンドポイントに対するコンパイル時のエラーです。</span><span class="sxs-lookup"><span data-stu-id="51045-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="51045-141">このエラーが発生した場合は、通常、 `return`ステートメントが存在しないことを示しています。</span><span class="sxs-lookup"><span data-stu-id="51045-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="51045-142">ブロック</span><span class="sxs-lookup"><span data-stu-id="51045-142">Blocks</span></span>

<span data-ttu-id="51045-143">"*ブロック*" を使用すると、1 つのステートメントしか使用できないコンテキストで複数のステートメントを記述できます。</span><span class="sxs-lookup"><span data-stu-id="51045-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="51045-144">*ブロック*は、省略可能な*statement_list* ([ステートメントリスト](statements.md#statement-lists)) で構成され、中かっこで囲まれています。</span><span class="sxs-lookup"><span data-stu-id="51045-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="51045-145">ステートメントリストが省略されている場合、ブロックは空であると言われます。</span><span class="sxs-lookup"><span data-stu-id="51045-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="51045-146">ブロックには、宣言ステートメント ([宣言ステートメント](statements.md#declaration-statements)) を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="51045-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="51045-147">ブロックで宣言されているローカル変数または定数のスコープは、ブロックです。</span><span class="sxs-lookup"><span data-stu-id="51045-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="51045-148">ブロックは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="51045-149">ブロックが空の場合、制御はブロックの終点に転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="51045-150">ブロックが空でない場合、制御はステートメントリストに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="51045-151">コントロールがステートメントリストの終点に到達した場合、制御はブロックの終点に移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="51045-152">ブロック自体に到達可能な場合、ブロックのステートメントリストに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="51045-153">ブロックが空の場合、またはステートメントリストの終点に到達できる場合は、ブロックの終点に到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="51045-154">1つ`yield`以上のステートメントを含むブロック ([yield ステートメント](statements.md#the-yield-statement)) は、反復子ブロックと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="51045-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="51045-155">反復子ブロックは、関数メンバーを反復子 ([反復子](classes.md#iterators)) として実装するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="51045-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="51045-156">反復子ブロックには、いくつかの追加の制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="51045-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="51045-157">`return`ステートメントが反復子ブロックに出現する場合、コンパイル時エラーになります (ただし`yield return` 、ステートメントは許可されます)。</span><span class="sxs-lookup"><span data-stu-id="51045-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="51045-158">反復子ブロックに unsafe コンテキスト ([unsafe](unsafe-code.md#unsafe-contexts)コンテキスト) が含まれていると、コンパイル時にエラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="51045-159">反復子ブロックは、その宣言が unsafe コンテキストで入れ子になっている場合でも、常に安全なコンテキストを定義します。</span><span class="sxs-lookup"><span data-stu-id="51045-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="51045-160">ステートメントの一覧</span><span class="sxs-lookup"><span data-stu-id="51045-160">Statement lists</span></span>

<span data-ttu-id="51045-161">***ステートメントリスト***は、順番に記述された1つ以上のステートメントで構成されます。</span><span class="sxs-lookup"><span data-stu-id="51045-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="51045-162">ステートメントリストは、*ブロック*s ([ブロック](statements.md#blocks)) と*switch_block*s ([switch ステートメント](statements.md#the-switch-statement)) で発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="51045-163">ステートメントリストを実行するには、最初のステートメントに制御を移します。</span><span class="sxs-lookup"><span data-stu-id="51045-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="51045-164">コントロールがステートメントの終点に到達した場合、制御は次のステートメントに移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="51045-165">コントロールが最後のステートメントの終点に達した場合、制御はステートメントリストのエンドポイントに移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="51045-166">次のいずれかの条件に該当する場合は、ステートメントリスト内のステートメントに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="51045-167">ステートメントが最初のステートメントであり、ステートメントリスト自体に到達可能である。</span><span class="sxs-lookup"><span data-stu-id="51045-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="51045-168">先行するステートメントの終点に到達できる。</span><span class="sxs-lookup"><span data-stu-id="51045-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="51045-169">ステートメントがラベル付きステートメントであり、到達可能`goto`なステートメントによってラベルが参照されています。</span><span class="sxs-lookup"><span data-stu-id="51045-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="51045-170">リスト内の最後のステートメントの終点に到達できる場合は、ステートメントリストの終点に到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="51045-171">空のステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-171">The empty statement</span></span>

<span data-ttu-id="51045-172">*Empty_statement*は何も行いません。</span><span class="sxs-lookup"><span data-stu-id="51045-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="51045-173">ステートメントが必要なコンテキストで実行する操作がない場合は、空のステートメントが使用されます。</span><span class="sxs-lookup"><span data-stu-id="51045-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="51045-174">空のステートメントを実行すると、単に制御がステートメントのエンドポイントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="51045-175">空のステートメントに到達できる場合、空のステートメントのエンドポイントに到達できるようになります。</span><span class="sxs-lookup"><span data-stu-id="51045-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="51045-176">空のステートメントは、本文が null の`while`ステートメントを記述するときに使用できます。</span><span class="sxs-lookup"><span data-stu-id="51045-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="51045-177">また、空のステートメントを使用して、ブロックの終わりの "`}`" の直前にラベルを宣言することもできます。</span><span class="sxs-lookup"><span data-stu-id="51045-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="51045-178">ラベル付きステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-178">Labeled statements</span></span>

<span data-ttu-id="51045-179">*Labeled_statement*を使用すると、ステートメントの先頭にラベルを付けることができます。</span><span class="sxs-lookup"><span data-stu-id="51045-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="51045-180">ラベル付きステートメントはブロックで許可されますが、埋め込みステートメントとして使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="51045-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="51045-181">ラベル付きステートメントは、*識別子*によって指定された名前を持つラベルを宣言します。</span><span class="sxs-lookup"><span data-stu-id="51045-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="51045-182">ラベルのスコープは、入れ子になったブロックを含め、ラベルが宣言されているすべてのブロックです。</span><span class="sxs-lookup"><span data-stu-id="51045-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="51045-183">同じ名前の2つのラベルが重複するスコープを持つ場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="51045-184">ラベルは、ラベルのスコープ`goto`内のステートメント ([goto ステートメント](statements.md#the-goto-statement)) から参照できます。</span><span class="sxs-lookup"><span data-stu-id="51045-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="51045-185">つまり`goto` 、ステートメントは、ブロック内およびブロック内で制御を転送できますが、ブロックには移動できません。</span><span class="sxs-lookup"><span data-stu-id="51045-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="51045-186">ラベルには独自の宣言領域があり、他の識別子に干渉することはありません。</span><span class="sxs-lookup"><span data-stu-id="51045-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="51045-187">例</span><span class="sxs-lookup"><span data-stu-id="51045-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="51045-188">は有効で、パラメーターと`x`ラベルの両方として名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="51045-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="51045-189">ラベル付きステートメントの実行は、ラベルに続くステートメントの実行に正確に対応します。</span><span class="sxs-lookup"><span data-stu-id="51045-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="51045-190">ラベルが到達可能`goto`なステートメントによって参照されている場合、通常の制御フローによって提供される到達可能性に加えて、ラベル付きステートメントに到達できるようになります。</span><span class="sxs-lookup"><span data-stu-id="51045-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="51045-191">例外的`try` `try` `finally`ステートメントがブロックを`finally`含む内にあり、ラベルが付けられたステートメントがの外側にあり、ブロックの終点に到達できない場合、ラベルが付けられたステートメントはから到達できません。 `goto`この`goto`ステートメントです。)</span><span class="sxs-lookup"><span data-stu-id="51045-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="51045-192">宣言ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-192">Declaration statements</span></span>

<span data-ttu-id="51045-193">*Declaration_statement*は、ローカル変数または定数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="51045-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="51045-194">宣言ステートメントはブロックで許可されていますが、埋め込みステートメントとして使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="51045-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="51045-195">ローカル変数の宣言</span><span class="sxs-lookup"><span data-stu-id="51045-195">Local variable declarations</span></span>

<span data-ttu-id="51045-196">*Local_variable_declaration*は、1つ以上のローカル変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="51045-196">A *local_variable_declaration* declares one or more local variables.</span></span>

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

<span data-ttu-id="51045-197">*Local_variable_declaration*の*local_variable_type*は、宣言によって導入された変数の型を直接指定します。または、初期化子に基づいて型を推論する必要があることを `var` の識別子と共に示します。</span><span class="sxs-lookup"><span data-stu-id="51045-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="51045-198">型の後に*local_variable_declarator*のリストが続き、それぞれに新しい変数が導入されています。</span><span class="sxs-lookup"><span data-stu-id="51045-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="51045-199">*Local_variable_declarator*は、変数に名前を付けた*識別子*と、必要に応じて "`=`" トークン、および変数の初期値を指定する*local_variable_initializer*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="51045-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="51045-200">ローカル変数宣言のコンテキストでは、識別子 var はコンテキストキーワード ([キーワード](lexical-structure.md#keywords)) として機能します。*Local_variable_type*が `var` として指定され、`var` という名前の型がスコープ内にない場合、宣言は暗黙的に型指定された***ローカル変数宣言***であり、その型は、関連付けられた初期化子式の型から推論されます。</span><span class="sxs-lookup"><span data-stu-id="51045-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="51045-201">暗黙的に型指定されるローカル変数宣言には、次の制限があります。</span><span class="sxs-lookup"><span data-stu-id="51045-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="51045-202">*Local_variable_declaration*に複数の*local_variable_declarator*を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="51045-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="51045-203">*Local_variable_declarator*には、 *local_variable_initializer*を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="51045-204">*Local_variable_initializer*は*式*である必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="51045-205">初期化子*式*は、コンパイル時の型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="51045-206">初期化子*式*は、宣言された変数自体を参照できません</span><span class="sxs-lookup"><span data-stu-id="51045-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="51045-207">暗黙的に型指定された不適切なローカル変数宣言の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="51045-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="51045-208">ローカル変数の値は、 *simple_name* ([簡易名](expressions.md#simple-names)) を使用して式で取得され、ローカル変数の値は*代入*[演算子 (代入演算子](expressions.md#assignment-operators)) を使用して変更されます。</span><span class="sxs-lookup"><span data-stu-id="51045-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="51045-209">ローカル変数は、値が取得される場所ごとに、確実に割り当てられる必要があります ([明確な代入](variables.md#definite-assignment))。</span><span class="sxs-lookup"><span data-stu-id="51045-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="51045-210">*Local_variable_declaration*で宣言されたローカル変数のスコープは、宣言が発生するブロックです。</span><span class="sxs-lookup"><span data-stu-id="51045-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="51045-211">ローカル変数の*local_variable_declarator*の前にあるテキスト位置でローカル変数を参照すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="51045-212">ローカル変数のスコープ内では、同じ名前を持つ別のローカル変数または定数を宣言するコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="51045-213">複数の変数を宣言するローカル変数宣言は、同じ型の単一の変数の複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="51045-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="51045-214">さらに、ローカル変数宣言内の変数初期化子は、宣言の直後に挿入される代入ステートメントと完全に一致します。</span><span class="sxs-lookup"><span data-stu-id="51045-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="51045-215">例</span><span class="sxs-lookup"><span data-stu-id="51045-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="51045-216">はに正確に対応します。</span><span class="sxs-lookup"><span data-stu-id="51045-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="51045-217">暗黙的に型指定されたローカル変数宣言では、宣言されるローカル変数の型は、変数の初期化に使用される式の型と同じになります。</span><span class="sxs-lookup"><span data-stu-id="51045-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="51045-218">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="51045-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="51045-219">上記の暗黙的に型指定されたローカル変数宣言は、明示的に型指定された次の宣言とまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="51045-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="51045-220">ローカル定数宣言</span><span class="sxs-lookup"><span data-stu-id="51045-220">Local constant declarations</span></span>

<span data-ttu-id="51045-221">*Local_constant_declaration*は、1つ以上のローカル定数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="51045-221">A *local_constant_declaration* declares one or more local constants.</span></span>

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="51045-222">*Local_constant_declaration*の*型*は、宣言によって導入される定数の型を指定します。</span><span class="sxs-lookup"><span data-stu-id="51045-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="51045-223">型の後に*constant_declarator*のリストが続き、それぞれに新しい定数が導入されています。</span><span class="sxs-lookup"><span data-stu-id="51045-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="51045-224">*Constant_declarator*は、定数に名前を付け、その後に "`=`" トークンを続け、その後に定数の値を指定する*constant_expression* ([定数式](expressions.md#constant-expressions)) を指定する*識別子*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="51045-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="51045-225">ローカル定数宣言の*型*と*constant_expression*は、定数メンバー宣言 ([定数](classes.md#constants)) と同じ規則に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="51045-226">ローカル定数の値は、 *simple_name* ([簡易名](expressions.md#simple-names)) を使用して式で取得されます。</span><span class="sxs-lookup"><span data-stu-id="51045-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="51045-227">ローカル定数のスコープは、宣言が発生するブロックです。</span><span class="sxs-lookup"><span data-stu-id="51045-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="51045-228">*Constant_declarator*の前にあるテキスト位置でローカル定数を参照すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="51045-229">ローカル定数のスコープ内では、同じ名前を持つ別のローカル変数または定数を宣言するコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="51045-230">複数の定数を宣言するローカル定数宣言は、同じ型を持つ単一定数の複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="51045-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="51045-231">式ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-231">Expression statements</span></span>

<span data-ttu-id="51045-232">*Expression_statement*は、指定された式を評価します。</span><span class="sxs-lookup"><span data-stu-id="51045-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="51045-233">式によって計算された値 (存在する場合) は破棄されます。</span><span class="sxs-lookup"><span data-stu-id="51045-233">The value computed by the expression, if any, is discarded.</span></span>

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

<span data-ttu-id="51045-234">すべての式がステートメントとして許可されているわけではありません。</span><span class="sxs-lookup"><span data-stu-id="51045-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="51045-235">特に、や`x + y` `x == 1`などの式では、値を計算するだけで (破棄されます)、ステートメントとして使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="51045-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="51045-236">*Expression_statement*を実行すると、含まれている式が評価され、 *expression_statement*の終点に制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="51045-237">*Expression_statement*に到達できる場合、 *expression_statement*のエンドポイントに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="51045-238">選択ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-238">Selection statements</span></span>

<span data-ttu-id="51045-239">選択ステートメントでは、いくつかの式の値に基づいて、実行に使用できるステートメントをいくつか選択します。</span><span class="sxs-lookup"><span data-stu-id="51045-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="51045-240">If ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-240">The if statement</span></span>

<span data-ttu-id="51045-241">ステートメント`if`は、ブール式の値に基づいて実行するステートメントを選択します。</span><span class="sxs-lookup"><span data-stu-id="51045-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="51045-242">構文で許可されている、前`if`に最も近い構文にパートが関連付けられています。`else`</span><span class="sxs-lookup"><span data-stu-id="51045-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="51045-243">そのため、 `if`</span><span class="sxs-lookup"><span data-stu-id="51045-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="51045-244">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="51045-244">is equivalent to</span></span>
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

<span data-ttu-id="51045-245">`if`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-246">*Boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。</span><span class="sxs-lookup"><span data-stu-id="51045-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="51045-247">ブール式`true`がの場合、制御は最初の埋め込みステートメントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="51045-248">コントロールがそのステートメントの終点に到達した場合、制御は`if`ステートメントの終了点に移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="51045-249">ブール式がを`false` `else`生成し、パーツが存在する場合、制御は2番目の埋め込みステートメントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="51045-250">コントロールがそのステートメントの終点に到達した場合、制御は`if`ステートメントの終了点に移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="51045-251">ブール式がを生成`false`し、 `else`パーツが存在しない場合、制御は`if`ステートメントの終了点に移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="51045-252">ステートメントが到達可能で、 `if`ブール式が定数値`if` `false`を持たない場合は、ステートメントの最初の埋め込みステートメントに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="51045-253">ステートメントが到達可能で、 `if`ブール式が定数値`true`を持たない`if`場合は、ステートメントの2番目の埋め込みステートメント (存在する場合) に到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="51045-254">少なくとも1つ`if`の埋め込みステートメントの終点に到達できる場合、ステートメントの終了位置に到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="51045-255">また、 `if`ステートメントが到達可能で、 `if`ブール式が`else`定数値`true`を持たない場合は、部分を持たないステートメントの終点に到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="51045-256">Switch ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-256">The switch statement</span></span>

<span data-ttu-id="51045-257">Switch ステートメントは、switch 式の値に対応するスイッチラベルが関連付けられているステートメントリストの実行を選択します。</span><span class="sxs-lookup"><span data-stu-id="51045-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

<span data-ttu-id="51045-258">*Switch_statement*は、キーワード `switch`、その後にかっこで囲まれた式 (switch 式と呼ばれます)、 *switch_block*の順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="51045-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="51045-259">*Switch_block*は、中かっこで囲まれた0個以上の*switch_section*s で構成されます。</span><span class="sxs-lookup"><span data-stu-id="51045-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="51045-260">各*switch_section*は、1つ以上の*switch_label*s の後に*statement_list* ([ステートメントリスト](statements.md#statement-lists)) で構成されます。</span><span class="sxs-lookup"><span data-stu-id="51045-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="51045-261">`switch`ステートメントの***管理型***は、switch 式によって設定されます。</span><span class="sxs-lookup"><span data-stu-id="51045-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="51045-262">スイッチ式の型が `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`bool`、`char`、0、または*enum_type*のいずれかの型に対応する null 許容型である場合は、は、2 ステートメントの管理型です。</span><span class="sxs-lookup"><span data-stu-id="51045-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="51045-263">それ以外の場合、ユーザー定義の暗黙的な変換 ([ユーザー定義の変換](conversions.md#user-defined-conversions)) は、switch 式の型から、次のいずれかの管理型に`sbyte`する必要`short`が`ushort`あります。、 `byte`、、`int` 、、`char`、 、`long` 、、`string`、または。これらの型のいずれかに対応する null 許容型。 `ulong` `uint`</span><span class="sxs-lookup"><span data-stu-id="51045-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="51045-264">それ以外の場合、このような暗黙的な変換が存在しない場合、または複数の暗黙的な変換が存在する場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="51045-265">各`case`ラベルの定数式は、 `switch`ステートメントの管理型に暗黙的に変換可能な ([暗黙の変換](conversions.md#implicit-conversions)) 値を示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="51045-266">`case` 同じ`switch`ステートメント内の2つ以上のラベルで同じ定数値が指定されている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="51045-267">Switch ステートメントには、最大`default`で1つのラベルを設定できます。</span><span class="sxs-lookup"><span data-stu-id="51045-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="51045-268">`switch`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-269">Switch 式が評価され、管理型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="51045-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="51045-270">同じ`case` `case`ステートメントのラベルに指定されているいずれかの定数が switch 式の値と等しい場合は、一致するラベルの後にあるステートメントリストに制御が移ります。 `switch`</span><span class="sxs-lookup"><span data-stu-id="51045-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="51045-271">同じ`case` `default` `default`ステートメントのラベルに指定されている定数が switch 式の値と等しい場合、およびラベルが存在する場合、制御は次のようなステートメントの一覧に移ります。 `switch`タイトル.</span><span class="sxs-lookup"><span data-stu-id="51045-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="51045-272">同じ`case` `default` `switch`ステートメントのラベルに指定されている定数が switch 式の値と等しい場合、ラベルが存在しない場合、制御はステートメントのエンドポイントに転送されます。 `switch`</span><span class="sxs-lookup"><span data-stu-id="51045-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="51045-273">Switch セクションのステートメントリストの終点に到達できる場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="51045-274">これは "フォールスルー" ルールと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="51045-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="51045-275">例</span><span class="sxs-lookup"><span data-stu-id="51045-275">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
<span data-ttu-id="51045-276">は、到達可能なエンドポイントを持つ switch セクションがないため、有効です。</span><span class="sxs-lookup"><span data-stu-id="51045-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="51045-277">C やとC++は異なり、switch セクションの実行は、次の switch セクションに "フォールスルー" することはできません。例</span><span class="sxs-lookup"><span data-stu-id="51045-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
<span data-ttu-id="51045-278">コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-278">results in a compile-time error.</span></span> <span data-ttu-id="51045-279">Switch セクションの実行後に別の switch セクションを実行する場合は、明示的`goto case`なまたは`goto default`ステートメントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

<span data-ttu-id="51045-280">*Switch_section*では、複数のラベルを使用できます。</span><span class="sxs-lookup"><span data-stu-id="51045-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="51045-281">例</span><span class="sxs-lookup"><span data-stu-id="51045-281">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
<span data-ttu-id="51045-282">が有効です。</span><span class="sxs-lookup"><span data-stu-id="51045-282">is valid.</span></span> <span data-ttu-id="51045-283">この例では、ラベル `case 2:`、`default:` は同じ*switch_section*の一部であるため、"フォールスルー" ルールに違反しません。</span><span class="sxs-lookup"><span data-stu-id="51045-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="51045-284">"フォールスルーなし" ルールは、C で発生する一般的なバグクラスを防ぎC++ 、 `break`ステートメントが誤って省略された場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="51045-285">また、このルールにより、ステートメントの動作に影響を`switch`与えずに、ステートメントの switch セクションを任意に再配置できます。</span><span class="sxs-lookup"><span data-stu-id="51045-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="51045-286">たとえば、上記の`switch`ステートメントのセクションは、ステートメントの動作に影響を与えずに元に戻すことができます。</span><span class="sxs-lookup"><span data-stu-id="51045-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

<span data-ttu-id="51045-287">Switch セクションのステートメントの一覧は`break`、通常、 `goto case`、、または`goto default`ステートメントで終了しますが、ステートメントリストの終点を表示するコンストラクトは使用できません。</span><span class="sxs-lookup"><span data-stu-id="51045-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="51045-288">たとえば、ブール式`while` `true`によって制御されるステートメントは、エンドポイントに達しないことがわかっています。</span><span class="sxs-lookup"><span data-stu-id="51045-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="51045-289">同様に、 `throw`また`return`はステートメントは、常にコントロールを別の場所に転送し、エンドポイントに到達しないようにします。</span><span class="sxs-lookup"><span data-stu-id="51045-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="51045-290">したがって、次の例は有効です。</span><span class="sxs-lookup"><span data-stu-id="51045-290">Thus, the following example is valid:</span></span>
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

<span data-ttu-id="51045-291">`switch`ステートメントの管理型は、型`string`にすることができます。</span><span class="sxs-lookup"><span data-stu-id="51045-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="51045-292">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="51045-292">For example:</span></span>
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

<span data-ttu-id="51045-293">文字列等値演算子 ([文字列等値](expressions.md#string-equality-operators)演算子) `switch`と同様に、ステートメントでは大文字と小文字が区別され、switch 式の`case`文字列がラベル定数と完全に一致する場合にのみ、指定された switch セクションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="51045-294">`switch`ステートメントの管理型が`string`の場合、値`null`は case ラベル定数として許可されます。</span><span class="sxs-lookup"><span data-stu-id="51045-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="51045-295">*Switch_block*の*statement_list*には、宣言ステートメント ([宣言ステートメント](statements.md#declaration-statements)) を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="51045-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="51045-296">Switch ブロックで宣言されたローカル変数または定数のスコープは、スイッチブロックです。</span><span class="sxs-lookup"><span data-stu-id="51045-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="51045-297">`switch`ステートメントが到達可能で、少なくとも次のいずれかの条件に該当する場合は、特定の switch セクションのステートメントリストに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="51045-298">スイッチ式は、非定数値です。</span><span class="sxs-lookup"><span data-stu-id="51045-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="51045-299">Switch 式は、switch セクションの`case`ラベルに一致する定数値です。</span><span class="sxs-lookup"><span data-stu-id="51045-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="51045-300">スイッチ式がどのラベルとも`case`一致しない定数値です。 switch セクションには`default`ラベルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="51045-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="51045-301">Switch セクションのスイッチラベルが、到達可能`goto case`なまたは`goto default`ステートメントによって参照されています。</span><span class="sxs-lookup"><span data-stu-id="51045-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="51045-302">次のいずれかの`switch`条件に該当する場合は、ステートメントの終了位置に到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="51045-303">ステートメントには、 `switch`ステートメント`break`を終了する到達可能なステートメントが含まれています。 `switch`</span><span class="sxs-lookup"><span data-stu-id="51045-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="51045-304">ステートメントが到達可能で、スイッチ式が非定数値であり、ラベルが存在しません`default`。 `switch`</span><span class="sxs-lookup"><span data-stu-id="51045-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="51045-305">ステートメントに到達できます。 switch 式は、どのラベルにも`case`一致しない定数値です。ラベルは存在しません。 `default` `switch`</span><span class="sxs-lookup"><span data-stu-id="51045-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="51045-306">繰り返しステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-306">Iteration statements</span></span>

<span data-ttu-id="51045-307">繰り返しステートメントでは、埋め込みステートメントを繰り返し実行します。</span><span class="sxs-lookup"><span data-stu-id="51045-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="51045-308">While ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-308">The while statement</span></span>

<span data-ttu-id="51045-309">ステートメント`while`は、条件付きで埋め込みステートメントを0回以上実行します。</span><span class="sxs-lookup"><span data-stu-id="51045-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="51045-310">`while`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-311">*Boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。</span><span class="sxs-lookup"><span data-stu-id="51045-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="51045-312">ブール式`true`がの場合、コントロールは埋め込みステートメントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="51045-313">コントロールが埋め込みステートメントの終点に到達した場合 ( `continue`ステートメントの実行から)、 `while`ステートメントの先頭に制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="51045-314">ブール式`false`がの場合、制御は`while`ステートメントの終了点に移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="51045-315">ステートメント`while`の埋め込みステートメント内では`break` 、ステートメント ([break ステートメント](statements.md#the-break-statement)) を使用して、 `while`ステートメントの終了位置に制御を移すことができます (したがって、埋め込みステートメントの反復処理を終了します)。`continue`ステートメント ([continue ステートメント](statements.md#the-continue-statement)) を使用して、埋め込みステートメントのエンドポイントに制御を移すことができます (したがって`while` 、ステートメントの別の反復処理を実行します)。</span><span class="sxs-lookup"><span data-stu-id="51045-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="51045-316">ステートメントが到達可能で`while` 、ブール式が定数`while`値`false`を持たない場合は、ステートメントの埋め込みステートメントに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="51045-317">次のいずれかの`while`条件に該当する場合は、ステートメントの終了位置に到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="51045-318">ステートメントには、 `while`ステートメント`break`を終了する到達可能なステートメントが含まれています。 `while`</span><span class="sxs-lookup"><span data-stu-id="51045-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="51045-319">ステートメントが到達可能で、ブール式に定数値`true`がありません。 `while`</span><span class="sxs-lookup"><span data-stu-id="51045-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="51045-320">Do ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-320">The do statement</span></span>

<span data-ttu-id="51045-321">ステートメント`do`は、条件付きで埋め込みステートメントを1回以上実行します。</span><span class="sxs-lookup"><span data-stu-id="51045-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="51045-322">`do`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-323">コントロールは、埋め込みステートメントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="51045-324">コントロールが埋め込みステートメントの終点に到達すると (場合によっては `continue` ステートメントの実行から)、 *boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。</span><span class="sxs-lookup"><span data-stu-id="51045-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="51045-325">ブール式`true`がの場合、制御は`do`ステートメントの先頭に移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="51045-326">それ以外の場合、制御は`do`ステートメントのエンドポイントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="51045-327">ステートメント`do`の埋め込みステートメント内では`break` 、ステートメント ([break ステートメント](statements.md#the-break-statement)) を使用して、 `do`ステートメントの終了位置に制御を移すことができます (したがって、埋め込みステートメントの反復処理を終了します)。`continue`ステートメント ([continue ステートメント](statements.md#the-continue-statement)) を使用して、埋め込みステートメントのエンドポイントに制御を移すことができます。</span><span class="sxs-lookup"><span data-stu-id="51045-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="51045-328">ステートメントに到達できる`do` `do`場合、ステートメントの埋め込みステートメントに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="51045-329">次のいずれかの`do`条件に該当する場合は、ステートメントの終了位置に到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="51045-330">ステートメントには、 `do`ステートメント`break`を終了する到達可能なステートメントが含まれています。 `do`</span><span class="sxs-lookup"><span data-stu-id="51045-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="51045-331">埋め込みステートメントの終点に到達でき、ブール式に定数値が指定`true`されていません。</span><span class="sxs-lookup"><span data-stu-id="51045-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="51045-332">For ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-332">The for statement</span></span>

<span data-ttu-id="51045-333">ステートメント`for`は初期化式のシーケンスを評価した後、条件が true の場合、埋め込みステートメントを繰り返し実行し、反復式のシーケンスを評価します。</span><span class="sxs-lookup"><span data-stu-id="51045-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

<span data-ttu-id="51045-334">*For_initializer*(存在する場合) は、コンマで区切られた*local_variable_declaration* ([ローカル変数宣言](statements.md#local-variable-declarations)) または*statement_expression*s ([式ステートメント](statements.md#expression-statements)) のリストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="51045-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="51045-335">*For_initializer*によって宣言されたローカル変数のスコープは、変数の*local_variable_declarator*から始まり、埋め込みステートメントの末尾まで拡張されます。</span><span class="sxs-lookup"><span data-stu-id="51045-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="51045-336">スコープには、 *for_condition*と*for_iterator*が含まれます。</span><span class="sxs-lookup"><span data-stu-id="51045-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="51045-337">*For_condition*(存在する場合) は、 *boolean_expression* ([ブール式](expressions.md#boolean-expressions)) である必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="51045-338">*For_iterator*(存在する場合) は、コンマで区切られた*Statement_expression*s ([式ステートメント](statements.md#expression-statements)) のリストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="51045-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="51045-339">For ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-340">*For_initializer*が存在する場合、変数初期化子またはステートメント式は、記述された順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="51045-341">このステップは1回だけ実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-341">This step is only performed once.</span></span>
*  <span data-ttu-id="51045-342">*For_condition*が存在する場合は、評価されます。</span><span class="sxs-lookup"><span data-stu-id="51045-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="51045-343">*For_condition*が存在しない場合、または評価結果が-1 @no__t の場合、コントロールは埋め込みステートメントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="51045-344">コントロールが埋め込みステートメントの終点に到達した場合 (`continue` ステートメントの実行など)、 *for_iterator*の式 (場合によっては) が順番に評価された後、次のように、別の反復処理が実行されます。上記の手順での*for_condition*の評価。</span><span class="sxs-lookup"><span data-stu-id="51045-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="51045-345">*For_condition*が存在し、評価結果が-1 @no__t の場合、制御は `for` ステートメントのエンドポイントに移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="51045-346">@No__t-0 ステートメントの埋め込みステートメント内では、`break` ステートメント ([break ステートメント](statements.md#the-break-statement)) を使用して、`for` ステートメントの終点に制御を移すことができます (したがって、埋め込みステートメントの反復処理を終了します)。また、`continue` ステートメント ([Continue ステートメント](statements.md#the-continue-statement)) を使用して、埋め込みステートメントのエンドポイントに制御を移すことができます (つまり、 *for_iterator*を実行し、 *for_condition*から始まる `for` ステートメントの別の反復処理を実行します)。</span><span class="sxs-lookup"><span data-stu-id="51045-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="51045-347">次のいずれかに`for`該当する場合は、ステートメントの埋め込みステートメントに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="51045-348">@No__t-0 ステートメントに到達可能で、 *for_condition*が存在しません。</span><span class="sxs-lookup"><span data-stu-id="51045-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="51045-349">@No__t-0 ステートメントに到達可能であり、 *for_condition*が存在し、定数値 `false` が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="51045-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="51045-350">次のいずれかの`for`条件に該当する場合は、ステートメントの終了位置に到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="51045-351">ステートメントには、 `for`ステートメント`break`を終了する到達可能なステートメントが含まれています。 `for`</span><span class="sxs-lookup"><span data-stu-id="51045-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="51045-352">@No__t-0 ステートメントに到達可能であり、 *for_condition*が存在し、定数値 `true` が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="51045-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="51045-353">Foreach ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-353">The foreach statement</span></span>

<span data-ttu-id="51045-354">ステートメント`foreach`は、コレクションの各要素に対して埋め込みステートメントを実行して、コレクションの要素を列挙します。</span><span class="sxs-lookup"><span data-stu-id="51045-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="51045-355">ステートメントの*型*と*識別子* `foreach`は、ステートメントの***繰り返し変数***を宣言します。</span><span class="sxs-lookup"><span data-stu-id="51045-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="51045-356">@No__t 0 の識別子が*local_variable_type*として指定され、`var` という名前の型がスコープ内にない場合、反復変数は***暗黙的に型指定***された反復変数と呼ばれ、その型は @no__t の要素型になります。ステートメント。以下のように指定します。</span><span class="sxs-lookup"><span data-stu-id="51045-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="51045-357">繰り返し変数は、埋め込みステートメントの上にあるスコープを持つ読み取り専用のローカル変数に対応します。</span><span class="sxs-lookup"><span data-stu-id="51045-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="51045-358">反復変数は、 `foreach`ステートメントの実行中に、反復処理が現在実行されているコレクション要素を表します。</span><span class="sxs-lookup"><span data-stu-id="51045-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="51045-359">埋め込みステートメントが反復変数を変更しようとし`++`た場合 (代入`--`演算子または演算子を使用)、 `ref`または反復変数をパラメーターまたは`out`パラメーターとして渡すと、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="51045-360">以下では、 `IEnumerable`を簡潔`IEnumerator`にするため`IEnumerable<T>`に`IEnumerator<T>` 、、、およびは、名前空間`System.Collections`と`System.Collections.Generic`の対応する型を参照しています。</span><span class="sxs-lookup"><span data-stu-id="51045-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="51045-361">Foreach ステートメントのコンパイル時の処理では、最初に、式の***コレクション型***、***列挙子の型***、および***要素の型***を決定します。</span><span class="sxs-lookup"><span data-stu-id="51045-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="51045-362">この決定は次のように行われることになります。</span><span class="sxs-lookup"><span data-stu-id="51045-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="51045-363">式の型`X`が配列型である場合は、から`IEnumerable`インターフェイスへの暗黙の`X`参照変換が行われ`System.Array`ます (以降はこのインターフェイスが実装されます)。</span><span class="sxs-lookup"><span data-stu-id="51045-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="51045-364">***コレクション型***は`IEnumerable`インターフェイス、***列挙子型***はインターフェイス、 `IEnumerator` ***要素型***は配列型`X`の要素型です。</span><span class="sxs-lookup"><span data-stu-id="51045-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="51045-365">式の型`X`が `dynamic`の場合は、*式*から`IEnumerable`インターフェイスへの暗黙的な変換 ([暗黙の動的変換](conversions.md#implicit-dynamic-conversions)) が存在します。</span><span class="sxs-lookup"><span data-stu-id="51045-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="51045-366">***コレクション型***は`IEnumerable`インターフェイスで、***列挙子の型***は`IEnumerator`インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="51045-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="51045-367">@No__t 0 の識別子が*local_variable_type*として指定されている場合は、***要素の型***が 3 @no__t になります。それ以外の場合は、`object` になります。</span><span class="sxs-lookup"><span data-stu-id="51045-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="51045-368">それ以外の場合は、 `X`型に適切`GetEnumerator`なメソッドがあるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="51045-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="51045-369">`X` 識別子`GetEnumerator`を使用して型引数を指定せずに、型に対してメンバーの参照を実行します。</span><span class="sxs-lookup"><span data-stu-id="51045-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="51045-370">メンバー参照によって一致が生成されない場合、またはあいまいさが生成される場合、またはメソッドグループではない一致が生成される場合は、次に示すように、列挙可能なインターフェイスを確認してください。</span><span class="sxs-lookup"><span data-stu-id="51045-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="51045-371">メンバー参照によってメソッドグループ以外のものが生成された場合、または一致するものがない場合は、警告を発行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="51045-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="51045-372">結果のメソッドグループと空の引数リストを使用して、オーバーロードの解決を実行します。</span><span class="sxs-lookup"><span data-stu-id="51045-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="51045-373">オーバーロードの解決によって適用できないメソッドが発生した場合、あいまいさが生じる場合、または結果が1つのベストメソッドであり、そのメソッドが静的であるかどうかにかかわらず、次に示すように、列挙可能なインターフェイスを確認してください。</span><span class="sxs-lookup"><span data-stu-id="51045-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="51045-374">オーバーロードの解決によって明確なパブリックインスタンスメソッド以外のものが生成される場合、または該当するメソッドがない場合は、警告を発行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="51045-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="51045-375">メソッドの戻り値`E`の型がクラス、構造体、またはインターフェイス型ではない場合は、エラーが生成され、それ以上の手順は実行されません。 `GetEnumerator`</span><span class="sxs-lookup"><span data-stu-id="51045-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="51045-376">メンバー参照は、識別子`E` `Current`と型引数を指定せずにに対して実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="51045-377">メンバー参照が一致しない場合、結果がエラーになる場合、または読み取りを許可するパブリックインスタンスプロパティ以外の結果である場合は、エラーが生成され、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="51045-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="51045-378">メンバー参照は、識別子`E` `MoveNext`と型引数を指定せずにに対して実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="51045-379">メンバー参照が一致しない場合、結果がエラーになる場合、またはメソッドグループ以外のすべての結果である場合は、エラーが生成され、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="51045-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="51045-380">オーバーロードの解決は、空の引数リストを持つメソッドグループで実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="51045-381">オーバーロードの解決によって適用されるメソッドがない場合、あいまいさが生じるか、または1つのベストメソッドになりますが、そのメソッドは静的で`bool`もパブリックでもありません。また、戻り値の型が存在しない場合は、エラーが生成され、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="51045-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="51045-382">***コレクション型***は`X`で、***列挙子の型***は`E`で、***要素の型***は`Current`プロパティの型です。</span><span class="sxs-lookup"><span data-stu-id="51045-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="51045-383">それ以外の場合は、列挙可能なインターフェイスがあるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="51045-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="51045-384">`Ti`から`dynamic` `T` `T` `Ti`への暗黙的な変換が行われているすべての型の中で、がではなく、他のすべての型が`IEnumerable<Ti>` `X`から`IEnumerable<T>`へ`IEnumerator<T>` `IEnumerable<T>`の暗黙の型変換では、コレクション型はインターフェイス、列挙子型はインターフェイス、要素型はです。 `IEnumerable<Ti>` `T`.</span><span class="sxs-lookup"><span data-stu-id="51045-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="51045-385">それ以外の場合、このような型`T`が複数存在すると、エラーが生成され、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="51045-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="51045-386">それ以外の場合`X` 、から`System.Collections.IEnumerable`インターフェイスへの暗黙的な変換がある場合 ***、コレクション型***はこのインターフェイス、***列挙子型***はインターフェイス`System.Collections.IEnumerator`、***要素型***はです。 `object`.</span><span class="sxs-lookup"><span data-stu-id="51045-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="51045-387">それ以外の場合は、エラーが生成され、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="51045-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="51045-388">上記の手順が成功すると、コレクション型`C`、列挙子型`E` 、および要素型`T`が明確に生成されます。</span><span class="sxs-lookup"><span data-stu-id="51045-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="51045-389">フォームの foreach ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="51045-390">は次のように展開されます。</span><span class="sxs-lookup"><span data-stu-id="51045-390">is then expanded to:</span></span>
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

<span data-ttu-id="51045-391">変数`e`は、式`x` 、埋め込みステートメント、またはプログラムのその他のソースコードからは参照できないか、アクセスできません。</span><span class="sxs-lookup"><span data-stu-id="51045-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="51045-392">埋め込み`v`ステートメントでは、変数は読み取り専用です。</span><span class="sxs-lookup"><span data-stu-id="51045-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="51045-393">@No__t-1 (要素型) から `V` (foreach ステートメントの*local_variable_type* ) への明示的な変換 ([明示的](conversions.md#explicit-conversions)な変換) が行われていない場合は、エラーが生成され、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="51045-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="51045-394">に`x`値`null`がある場合は、実行時にがスロー`System.NullReferenceException`されます。</span><span class="sxs-lookup"><span data-stu-id="51045-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="51045-395">実装では、特定の foreach ステートメントを異なる方法で実装することができます。たとえば、パフォーマンス上の理由から、動作が上記の拡張と一致している場合に限ります。</span><span class="sxs-lookup"><span data-stu-id="51045-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="51045-396">While ループ内の @no__t 0 の位置は、 *embedded_statement*で発生している匿名関数によってキャプチャされる方法にとって重要です。</span><span class="sxs-lookup"><span data-stu-id="51045-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="51045-397">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="51045-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="51045-398">が`v` while ループの外側で宣言されている場合、すべてのイテレーション間で共有され、for ループの後の値が最終的`13`な値になります。これ`f`は、の呼び出しの結果です。</span><span class="sxs-lookup"><span data-stu-id="51045-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="51045-399">各反復処理には独自の変数`v`があるため、最初のイテレーションでによって`f`キャプチャされた値`7`は、出力される値を保持し続けます。</span><span class="sxs-lookup"><span data-stu-id="51045-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="51045-400">(注: の以前のC#バージョン`v`は、while ループの外側で宣言されています)。</span><span class="sxs-lookup"><span data-stu-id="51045-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="51045-401">Finally ブロックの本体は、次の手順に従って構築されます。</span><span class="sxs-lookup"><span data-stu-id="51045-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="51045-402">から`E` インターフェイス`System.IDisposable`への暗黙的な変換がある場合は、</span><span class="sxs-lookup"><span data-stu-id="51045-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="51045-403">が`E` null 非許容の値型である場合、finally 句は次のようなセマンティックに拡張されます。</span><span class="sxs-lookup"><span data-stu-id="51045-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="51045-404">それ以外の場合は、finally 句がに相当するセマンティックに拡張されます。</span><span class="sxs-lookup"><span data-stu-id="51045-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="51045-405">が値型`E`の場合、または値型にインスタンス化された型パラメーターの場合、へ`e`の`System.IDisposable`キャストによってボックス化が発生することはありません。</span><span class="sxs-lookup"><span data-stu-id="51045-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="51045-406">それ以外の`E`場合、が sealed 型の場合、finally 句は空のブロックに展開されます。</span><span class="sxs-lookup"><span data-stu-id="51045-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="51045-407">それ以外の場合、finally 句は次のように展開されます。</span><span class="sxs-lookup"><span data-stu-id="51045-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="51045-408">ローカル変数`d`は、ユーザーコードから参照できないか、ユーザーコードからアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="51045-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="51045-409">特に、スコープに finally ブロックが含まれている他の変数と競合しません。</span><span class="sxs-lookup"><span data-stu-id="51045-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="51045-410">配列の要素を`foreach`走査する順序は次のとおりです。1次元配列の要素の場合、インデックス順にインデックスを作成 `0`し、インデックスで終了`Length - 1`します。</span><span class="sxs-lookup"><span data-stu-id="51045-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="51045-411">多次元配列の場合は、要素が走査されて、右端の次元のインデックスが最初に増加し、次に左の次元になるようになります。</span><span class="sxs-lookup"><span data-stu-id="51045-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="51045-412">次の例では、要素の順序で2次元配列の各値を出力します。</span><span class="sxs-lookup"><span data-stu-id="51045-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
<span data-ttu-id="51045-413">生成される出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="51045-413">The output produced is as follows:</span></span>
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="51045-414">この例では、</span><span class="sxs-lookup"><span data-stu-id="51045-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="51045-415">の`n`型は、の`numbers`要素型`int`であると推論されます。</span><span class="sxs-lookup"><span data-stu-id="51045-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="51045-416">ジャンプ ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-416">Jump statements</span></span>

<span data-ttu-id="51045-417">ジャンプステートメントが無条件で制御を転送します。</span><span class="sxs-lookup"><span data-stu-id="51045-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="51045-418">ジャンプステートメントが制御を転送する場所は、ジャンプステートメントの***ターゲット***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="51045-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="51045-419">ジャンプステートメントがブロック内で発生し、そのジャンプステートメントの対象がそのブロックの外側にある場合、ジャンプステートメントはブロックを***終了***すると言います。</span><span class="sxs-lookup"><span data-stu-id="51045-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="51045-420">ジャンプステートメントは、ブロックから制御を移すことができますが、制御をブロックに移すことはできません。</span><span class="sxs-lookup"><span data-stu-id="51045-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="51045-421">ジャンプステートメントの実行は、介在`try`するステートメントが存在すると複雑になります。</span><span class="sxs-lookup"><span data-stu-id="51045-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="51045-422">このような`try`ステートメントが存在しない場合、ジャンプステートメントは無条件でジャンプステートメントからターゲットに制御を転送します。</span><span class="sxs-lookup"><span data-stu-id="51045-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="51045-423">このような中間`try`ステートメントが存在する場合、実行はより複雑になります。</span><span class="sxs-lookup"><span data-stu-id="51045-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="51045-424">ジャンプステートメントによって、関連付け`try`られ`finally`たブロックがある1つ以上のブロック`finally`が終了した`try`場合、最初にコントロールが最も内側のステートメントのブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="51045-425">コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="51045-426">このプロセスは、 `finally` `try`すべてのステートメントのブロックが実行されるまで繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="51045-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="51045-427">この例では、</span><span class="sxs-lookup"><span data-stu-id="51045-427">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
<span data-ttu-id="51045-428">2 `finally` つ`try`のステートメントに関連付けられているブロックは、ジャンプステートメントのターゲットに制御が転送される前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="51045-429">生成される出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="51045-429">The output produced is as follows:</span></span>
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="51045-430">Break ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-430">The break statement</span></span>

<span data-ttu-id="51045-431">ステートメント`break`は、 `switch`最も近い、 `while` `foreach` 、、 、`for`またはステートメントを終了します。 `do`</span><span class="sxs-lookup"><span data-stu-id="51045-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="51045-432">`break`ステートメントのターゲットは、最も近い`while`外側`switch` `do`の、、、 `for`、または`foreach`ステートメントの終点です。</span><span class="sxs-lookup"><span data-stu-id="51045-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="51045-433">`switch`ステートメントが、`do`、、、または`foreach`ステートメントで囲まれていない場合、コンパイル時エラーが発生します。 `for` `while` `break`</span><span class="sxs-lookup"><span data-stu-id="51045-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="51045-434">複数`switch`の`while` `foreach` `break` 、 、、、またはステートメントが入れ子になっている場合、ステートメントは最も内側のステートメントにのみ適用されます。`do` `for`</span><span class="sxs-lookup"><span data-stu-id="51045-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="51045-435">複数の入れ子レベルで制御を転送する`goto`には、ステートメント ([goto ステートメント](statements.md#the-goto-statement)) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="51045-436">ステートメント`break`でブロックを終了`finally`することはできません ([try ステートメント](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="51045-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="51045-437">ステートメントが`finally`ブロック内にある場合`break` 、ステートメントのターゲットは同じ`finally`ブロック内になければなりません。それ以外の場合は、コンパイル時エラーが発生します。 `break`</span><span class="sxs-lookup"><span data-stu-id="51045-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="51045-438">`break`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-439">ステートメントが`break` 、関連付けられ`try` `finally`た`finally`ブロックを持つ1つ以上のブロックを終了した場合、 `try`最初にコントロールが最も内側のステートメントのブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="51045-440">コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="51045-441">このプロセスは、 `finally` `try`すべてのステートメントのブロックが実行されるまで繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="51045-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="51045-442">制御は、 `break`ステートメントのターゲットに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="51045-443">ステートメントは`break`無条件で制御を別の場所に転送するの`break`で、ステートメントのエンドポイントに到達できません。</span><span class="sxs-lookup"><span data-stu-id="51045-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="51045-444">Continue ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-444">The continue statement</span></span>

<span data-ttu-id="51045-445">ステートメント`continue`は、 `while`最も近い`do` `foreach` 、、、またはステートメントの新しい反復処理を開始します。 `for`</span><span class="sxs-lookup"><span data-stu-id="51045-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="51045-446">`continue`ステートメントの対象は、最も近い外側`while`の、 `do` `for`、、または`foreach`ステートメントの埋め込みステートメントの終点です。</span><span class="sxs-lookup"><span data-stu-id="51045-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="51045-447">`continue`ステートメントが`while`、 `do`、、または`foreach`ステートメントで囲まれていない場合、コンパイル時エラーが発生します。 `for`</span><span class="sxs-lookup"><span data-stu-id="51045-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="51045-448">`while`複数の`do` 、、`continue` 、また`foreach`はステートメントが相互に入れ子になっている場合、ステートメントは最も内側のステートメントにのみ適用されます。 `for`</span><span class="sxs-lookup"><span data-stu-id="51045-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="51045-449">複数の入れ子レベルで制御を転送する`goto`には、ステートメント ([goto ステートメント](statements.md#the-goto-statement)) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="51045-450">ステートメント`continue`でブロックを終了`finally`することはできません ([try ステートメント](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="51045-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="51045-451">ステートメントが`finally`ブロック内にある場合`continue` 、ステートメントのターゲットは同じ`finally`ブロック内になければなりません。それ以外の場合は、コンパイル時エラーが発生します。 `continue`</span><span class="sxs-lookup"><span data-stu-id="51045-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="51045-452">`continue`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-453">ステートメントが`continue` 、関連付けられ`try` `finally`た`finally`ブロックを持つ1つ以上のブロックを終了した場合、 `try`最初にコントロールが最も内側のステートメントのブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="51045-454">コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="51045-455">このプロセスは、 `finally` `try`すべてのステートメントのブロックが実行されるまで繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="51045-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="51045-456">制御は、 `continue`ステートメントのターゲットに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="51045-457">ステートメントは`continue`無条件で制御を別の場所に転送するの`continue`で、ステートメントのエンドポイントに到達できません。</span><span class="sxs-lookup"><span data-stu-id="51045-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="51045-458">GoTo ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-458">The goto statement</span></span>

<span data-ttu-id="51045-459">ステートメント`goto`は、ラベルによってマークされているステートメントに制御を移します。</span><span class="sxs-lookup"><span data-stu-id="51045-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="51045-460">`goto` *識別子*ステートメントのターゲットは、指定されたラベルを持つラベル付きステートメントです。</span><span class="sxs-lookup"><span data-stu-id="51045-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="51045-461">指定した名前のラベルが現在の関数メンバーに存在しない場合、または`goto`ステートメントがラベルのスコープ内にない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="51045-462">この規則により、ステートメントを`goto`使用して、入れ子になったスコープから制御を移すことができますが、入れ子になったスコープには移動できません。</span><span class="sxs-lookup"><span data-stu-id="51045-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="51045-463">この例では、</span><span class="sxs-lookup"><span data-stu-id="51045-463">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
<span data-ttu-id="51045-464">ステートメント`goto`は、入れ子になったスコープから制御を移すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="51045-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="51045-465">`goto case`ステートメントのターゲットは、指定された定数値を持つ`switch`ラベルを`case`含む、すぐ外側のステートメント ([switch ステートメント](statements.md#the-switch-statement)) のステートメントリストです。</span><span class="sxs-lookup"><span data-stu-id="51045-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="51045-466">@No__t-0 ステートメントが `switch` ステートメントで囲まれていない場合、最も近い最も近い `switch` ステートメントの管理型に*constant_expression*が暗黙的に変換 ([暗黙の変換](conversions.md#implicit-conversions)) されない場合、または最も近い`switch` ステートメントに、指定された定数値を持つ @no__t 6 のラベルが含まれていません。コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="51045-467">`goto default`ステートメントの対象となるのは、ラベルを`default`含むすぐ外側`switch`のステートメント ([switch ステートメント](statements.md#the-switch-statement)) のステートメントリストです。</span><span class="sxs-lookup"><span data-stu-id="51045-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="51045-468">ステートメントが`switch`ステートメントで囲まれていない場合、または最も近い`switch`外側のステートメントに`default`ラベルが含まれていない場合は、コンパイル時エラーが発生します。 `goto default`</span><span class="sxs-lookup"><span data-stu-id="51045-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="51045-469">ステートメント`goto`でブロックを終了`finally`することはできません ([try ステートメント](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="51045-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="51045-470">ステートメントが`finally`ブロック内にある場合`goto` 、ステートメントのターゲットは同じ`finally`ブロック内に存在する必要があります。そうでない場合は、コンパイル時エラーが発生します。 `goto`</span><span class="sxs-lookup"><span data-stu-id="51045-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="51045-471">`goto`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-472">ステートメントが`goto` 、関連付けられ`try` `finally`た`finally`ブロックを持つ1つ以上のブロックを終了した場合、 `try`最初にコントロールが最も内側のステートメントのブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="51045-473">コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="51045-474">このプロセスは、 `finally` `try`すべてのステートメントのブロックが実行されるまで繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="51045-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="51045-475">制御は、 `goto`ステートメントのターゲットに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="51045-476">ステートメントは`goto`無条件で制御を別の場所に転送するの`goto`で、ステートメントのエンドポイントに到達できません。</span><span class="sxs-lookup"><span data-stu-id="51045-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="51045-477">Return ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-477">The return statement</span></span>

<span data-ttu-id="51045-478">ステートメント`return`は、 `return`ステートメントが存在する関数の現在の呼び出し元に制御を返します。</span><span class="sxs-lookup"><span data-stu-id="51045-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="51045-479">`set` `add` `void`式が指定されていないステートメントは、値を計算しない関数メンバーでのみ使用できます。つまり、結果の型([メソッド本体](classes.md#method-body))を持つメソッド、プロパティまたはインデクサーの`return`アクセサー、イベント`remove`のアクセサー、インスタンスコンストラクター、静的コンストラクター、またはデストラクター。</span><span class="sxs-lookup"><span data-stu-id="51045-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="51045-480">式を持つ`get` ステートメントは、値を計算する関数メンバー、つまり、void以外の結果型、プロパティまたはインデクサーのアクセサー、またはユーザー定義の演算子を持つメソッドを使用する場合にのみ`return`使用できます。</span><span class="sxs-lookup"><span data-stu-id="51045-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="51045-481">暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) は、式の型から、含んでいる関数メンバーの戻り値の型に存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="51045-482">Return ステートメントは、匿名関数式 ([匿名関数式](expressions.md#anonymous-function-expressions)) の本体で使用することもできます。また、これらの関数にどの変換が存在するかを判断するためにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="51045-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="51045-483">`return`ステートメントが`finally`ブロック ([try ステートメント](statements.md#the-try-statement)) に含まれる場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="51045-484">`return`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-485">`return`ステートメントが式を指定した場合、式が評価され、結果の値は暗黙的な変換によって、含んでいる関数の戻り値の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="51045-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="51045-486">変換の結果は、関数によって生成される結果値になります。</span><span class="sxs-lookup"><span data-stu-id="51045-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="51045-487">`try` `catch` `try` `finally` `finally`ステートメントが1つ以上のまたはブロックに関連付けられたブロックによって囲まれている場合は、最初にコントロールが最も内側のステートメントのブロックに転送されます。 `return`</span><span class="sxs-lookup"><span data-stu-id="51045-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="51045-488">コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="51045-489">このプロセスは、 `finally`外側`try`のすべてのステートメントのブロックが実行されるまで繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="51045-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="51045-490">含んでいる関数が非同期関数でない場合、制御は、含まれている関数の呼び出し元に、結果値 (存在する場合) と共に返されます。</span><span class="sxs-lookup"><span data-stu-id="51045-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="51045-491">含まれている関数が非同期関数の場合、コントロールは現在の呼び出し元に返され、結果値 (存在する場合) は、「([列挙子インターフェイス](classes.md#enumerator-interfaces))」で説明されているように、返されるタスクに記録されます。</span><span class="sxs-lookup"><span data-stu-id="51045-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="51045-492">ステートメントは`return`無条件で制御を別の場所に転送するの`return`で、ステートメントのエンドポイントに到達できません。</span><span class="sxs-lookup"><span data-stu-id="51045-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="51045-493">Throw ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-493">The throw statement</span></span>

<span data-ttu-id="51045-494">ステートメント`throw`が例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="51045-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="51045-495">式を含むステートメントでは、式を評価することによって生成される値がスローされます。 `throw`</span><span class="sxs-lookup"><span data-stu-id="51045-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="51045-496">式は、クラス`System.Exception`型の値を表す必要があります。これは、またはサブクラスを持つ`System.Exception`型パラメーター型から`System.Exception` 、有効な基本クラスとして派生するクラス型の値を表します。</span><span class="sxs-lookup"><span data-stu-id="51045-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="51045-497">式の評価によって`null` `System.NullReferenceException`が生成される場合は、代わりにがスローされます。</span><span class="sxs-lookup"><span data-stu-id="51045-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="51045-498">式が指定されていない`catch` `catch`ステートメントは、ブロック内でのみ使用できます。その場合、ステートメントは、現在そのブロックによって処理されている例外を再スローします。 `throw`</span><span class="sxs-lookup"><span data-stu-id="51045-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="51045-499">ステートメントは`throw`無条件で制御を別の場所に転送するの`throw`で、ステートメントのエンドポイントに到達できません。</span><span class="sxs-lookup"><span data-stu-id="51045-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="51045-500">例外がスローされると、その例外を処理できる`catch`外側`try`のステートメント内の最初の句に制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="51045-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="51045-501">適切な例外ハンドラーに制御を転送するポイントにスローされた例外の発生点から実行されるプロセスは、***例外の伝達***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="51045-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="51045-502">例外の伝達は、例外に一致する`catch`句が見つかるまで、次の手順を繰り返し評価することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="51045-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="51045-503">この説明では、最初に***スローポイント***が例外がスローされる場所です。</span><span class="sxs-lookup"><span data-stu-id="51045-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="51045-504">現在の関数メンバーでは、 `try`スローポイントを囲む各ステートメントが検査されます。</span><span class="sxs-lookup"><span data-stu-id="51045-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="51045-505">ステートメント`S`ごとに、最も内側`try`のステートメントで始まり、最も外側`try`のステートメントで終了すると、次の手順が評価されます。</span><span class="sxs-lookup"><span data-stu-id="51045-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="51045-506">`try`の`catch` `catch`ブロックがスローポイントを囲む場合、S に1つ以上の句が含まれている場合は、「」で指定されている規則に従って、例外に適したハンドラーを検索するために、表示順に句が調べられます。 `S`[Try ステートメントの](statements.md#the-try-statement)セクションです。</span><span class="sxs-lookup"><span data-stu-id="51045-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="51045-507">一致`catch`する句が見つかった場合は、その`catch`句のブロックに制御を移すことによって、例外の伝達が完了します。</span><span class="sxs-lookup"><span data-stu-id="51045-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="51045-508">それ以外の場合`try` 、ブロック`catch`またはの`S`ブロックがスローポイントを`finally`囲む`S`場合、およびにブロックがある場合は`finally` 、制御がブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="51045-509">ブロックで`finally`別の例外がスローされた場合、現在の例外の処理は終了します。</span><span class="sxs-lookup"><span data-stu-id="51045-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="51045-510">それ以外の場合、コントロールが`finally`ブロックの終点に到達すると、現在の例外の処理が続行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="51045-511">現在の関数呼び出しで例外ハンドラーが見つからなかった場合は、関数の呼び出しが終了し、次のいずれかが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="51045-512">現在の関数が非同期でない場合、上記の手順は関数の呼び出し元に対して、関数メンバーが呼び出されたステートメントに対応する throw ポイントを使用して繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="51045-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="51045-513">現在の関数が非同期でタスクを返す場合、例外は return タスクに記録されます。これは、「[列挙子インターフェイス](classes.md#enumerator-interfaces)」で説明されているように、エラーまたはキャンセル状態になります。</span><span class="sxs-lookup"><span data-stu-id="51045-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="51045-514">現在の関数が async および void を返す場合、現在のスレッドの同期コンテキストは、「列挙可能な[インターフェイス](classes.md#enumerable-interfaces)」で説明されているように通知されます。</span><span class="sxs-lookup"><span data-stu-id="51045-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="51045-515">例外処理によって、現在のスレッドのすべての関数メンバー呼び出しが終了し、そのスレッドに例外のハンドラーがないことが示された場合、そのスレッド自体は終了します。</span><span class="sxs-lookup"><span data-stu-id="51045-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="51045-516">このような終了の影響は、実装によって定義されます。</span><span class="sxs-lookup"><span data-stu-id="51045-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="51045-517">Try ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-517">The try statement</span></span>

<span data-ttu-id="51045-518">ステートメント`try`は、ブロックの実行中に発生した例外をキャッチするためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="51045-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="51045-519">さらに`try` 、ステートメントを使用すると、コントロールがステートメントから出たときに常に実行されるコードブロックを`try`指定できます。</span><span class="sxs-lookup"><span data-stu-id="51045-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

<span data-ttu-id="51045-520">ステートメントには、次の`try` 3 つの形式があります。</span><span class="sxs-lookup"><span data-stu-id="51045-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="51045-521">ブロック`try`の後に1つ`catch`以上のブロックが続きます。</span><span class="sxs-lookup"><span data-stu-id="51045-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="51045-522">ブロック`try`の後`finally`にブロックが続く。</span><span class="sxs-lookup"><span data-stu-id="51045-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="51045-523">ブロック`try`の後に1つ`catch`以上のブロックが続き`finally` 、その後にブロックが続きます。</span><span class="sxs-lookup"><span data-stu-id="51045-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="51045-524">@No__t-0 句で*exception_specifier*が指定 @no__t されている場合、型は、`System.Exception` から派生した型、または有効な基本クラスとして `System.Exception` (またはサブクラス) を持つ型パラメーター型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="51045-525">@No__t-0 句が*識別子*を持つ*exception_specifier*の両方を指定すると、指定された名前と型の***例外変数***が宣言されます。</span><span class="sxs-lookup"><span data-stu-id="51045-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="51045-526">例外変数は、 `catch`句を超えてスコープを持つローカル変数に対応します。</span><span class="sxs-lookup"><span data-stu-id="51045-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="51045-527">*Exception_filter*と*block*の実行中、例外変数は現在処理中の例外を表します。</span><span class="sxs-lookup"><span data-stu-id="51045-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="51045-528">明確な割り当てチェックの目的では、例外変数はスコープ全体で確実に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="51045-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="51045-529">句に`catch`例外変数名が含まれていない限り、フィルターおよび`catch`ブロック内の例外オブジェクトにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="51045-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="51045-530">*Exception_specifier*を指定しない @no__t 0 句は、general `catch` 句と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="51045-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="51045-531">一部のプログラミング言語は、から`System.Exception`派生したオブジェクトとして表現できない例外をサポートする場合がありますが、このような例外はコードによってC#生成されることはありません。</span><span class="sxs-lookup"><span data-stu-id="51045-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="51045-532">一般的`catch`な句は、このような例外をキャッチするために使用できます。</span><span class="sxs-lookup"><span data-stu-id="51045-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="51045-533">したがって、一般的`catch`な句は、型`System.Exception`を指定するものとは意味が異なります。つまり、前者は他の言語からも例外をキャッチする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="51045-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="51045-534">例外のハンドラーを見つけるために、 `catch`句は構文の順序で検証されます。</span><span class="sxs-lookup"><span data-stu-id="51045-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="51045-535">句に型が指定されていても例外フィルターがない場合は、同じ`try`ステートメント内`catch`の後の句で、その型と同じか、またはその型から派生した型を指定するコンパイル時エラーになります。 `catch`</span><span class="sxs-lookup"><span data-stu-id="51045-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="51045-536">句に`catch`型が指定されておらず、フィルターもない場合`catch`は、その`try`ステートメントの最後の句である必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="51045-537">ブロック内では`throw` 、式のないステートメント ([throw ステートメント](statements.md#the-throw-statement)) を使用して、 `catch`ブロックでキャッチされた例外を再スローできます。 `catch`</span><span class="sxs-lookup"><span data-stu-id="51045-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="51045-538">例外変数への代入では、再スローされた例外は変更されません。</span><span class="sxs-lookup"><span data-stu-id="51045-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="51045-539">この例では、</span><span class="sxs-lookup"><span data-stu-id="51045-539">In the example</span></span>
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
<span data-ttu-id="51045-540">メソッド`F`は、例外をキャッチし、いくつかの診断情報をコンソールに書き込み、例外変数を変更して、例外を再スローします。</span><span class="sxs-lookup"><span data-stu-id="51045-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="51045-541">再スローされる例外は元の例外なので、生成される出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="51045-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```console
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="51045-542">現在の例外を再スローする`e`のではなく、最初の catch ブロックがスローされた場合、生成される出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="51045-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```console
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="51045-543">`break`、、また`continue`は`goto`ステートメントが`finally`ブロックの外部で制御を転送する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="51045-544">`break` `finally` 、 、`continue`または`goto`ステートメントがブロック内にある場合、ステートメントのターゲットは同じブロック内になければなりません。それ以外の場合は、`finally`コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="51045-545">`return`ステートメントが`finally`ブロック内で発生する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="51045-546">`try`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-547">コントロールは`try`ブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="51045-548">コントロールが`try`ブロックの終点に到達した場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="51045-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="51045-549">ステートメントにブロックがある場合は、ブロックが実行されます。`finally` `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="51045-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="51045-550">制御は、 `try`ステートメントのエンドポイントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="51045-551">ブロックの実行中に例外が`try`ステートメントに反映される場合は、次のようになります。 `try`</span><span class="sxs-lookup"><span data-stu-id="51045-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="51045-552">句`catch` (存在する場合) は、例外に適したハンドラーを見つけるために、表示順に調べられます。</span><span class="sxs-lookup"><span data-stu-id="51045-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="51045-553">`catch`句が型を指定しない場合、または例外の種類または例外の種類の基本型を指定する場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="51045-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="51045-554">句で`catch`例外変数が宣言されている場合、例外オブジェクトは例外変数に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="51045-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="51045-555">句で`catch`例外フィルターを宣言すると、フィルターが評価されます。</span><span class="sxs-lookup"><span data-stu-id="51045-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="51045-556">と評価`false`された場合、catch 句は一致しないため、適切なハンドラーの後続`catch`の句の中で検索が続行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="51045-557">それ以外の`catch`場合、句は一致と見なされ、制御は一致`catch`するブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="51045-558">コントロールが`catch`ブロックの終点に到達した場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="51045-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="51045-559">ステートメントにブロックがある場合は、ブロックが実行されます。`finally` `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="51045-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="51045-560">制御は、 `try`ステートメントのエンドポイントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="51045-561">ブロックの実行中に例外が`try`ステートメントに反映される場合は、次のようになります。 `catch`</span><span class="sxs-lookup"><span data-stu-id="51045-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="51045-562">ステートメントにブロックがある場合は、ブロックが実行されます。`finally` `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="51045-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="51045-563">例外は、次の外側`try`のステートメントに反映されます。</span><span class="sxs-lookup"><span data-stu-id="51045-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="51045-564">ステートメントに`try` `catch`句が含まれていない`catch`場合、または句が例外に一致しない場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="51045-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="51045-565">ステートメントにブロックがある場合は、ブロックが実行されます。`finally` `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="51045-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="51045-566">例外は、次の外側`try`のステートメントに反映されます。</span><span class="sxs-lookup"><span data-stu-id="51045-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="51045-567">コントロールがステートメントを`finally` `try`離れると、常にブロックのステートメントが実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="51045-568">`break`これは`continue` `try` 、、、 `return` 、またはステートメントを実行した結果として、通常の実行の結果として制御転送が行われるか、または、例外が`goto`諸表.</span><span class="sxs-lookup"><span data-stu-id="51045-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="51045-569">`finally`ブロックの実行中に例外がスローされ、同じ finally ブロック内でキャッチされない場合、例外は次の外側`try`のステートメントに反映されます。</span><span class="sxs-lookup"><span data-stu-id="51045-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="51045-570">別の例外が伝達の処理中であった場合、その例外は失われます。</span><span class="sxs-lookup"><span data-stu-id="51045-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="51045-571">例外を反映するプロセスについては、 `throw`ステートメントの説明 ([throw ステートメント](statements.md#the-throw-statement)) でさらに説明します。</span><span class="sxs-lookup"><span data-stu-id="51045-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="51045-572">ステートメント`try`に到達できる`try`場合`try` 、ステートメントのブロックに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="51045-573">ステートメントに到達できる`try`場合`try` 、ステートメントのブロックに到達できます。`catch`</span><span class="sxs-lookup"><span data-stu-id="51045-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="51045-574">ステートメント`finally`に到達できる`try`場合`try` 、ステートメントのブロックに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="51045-575">次の両方に該当`try`する場合は、ステートメントの終了位置に到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="51045-576">`try`ブロックの終点に到達可能であるか、少なくとも1つ`catch`のブロックのエンドポイントに到達できる。</span><span class="sxs-lookup"><span data-stu-id="51045-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="51045-577">ブロックが`finally`存在する場合は、 `finally`ブロックのエンドポイントに到達できます。</span><span class="sxs-lookup"><span data-stu-id="51045-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="51045-578">Checked ステートメントと unchecked ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-578">The checked and unchecked statements</span></span>

<span data-ttu-id="51045-579">`checked` および`unchecked`ステートメントは、整数型の算術演算および変換の***オーバーフローチェックコンテキスト***を制御するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="51045-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="51045-580">ステートメントにより、*ブロック*内のすべての式が checked `unchecked`コンテキストで評価され、ステートメントによってブロック内のすべての式が unchecked コンテキストで評価されます。 `checked`</span><span class="sxs-lookup"><span data-stu-id="51045-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="51045-581">`checked` `unchecked` ステートメントとステートメントは、式ではなくブロックで動作する点を除いて、and 演算子 ([checked および unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) とまったく同じです。 `checked` `unchecked`</span><span class="sxs-lookup"><span data-stu-id="51045-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="51045-582">Lock ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-582">The lock statement</span></span>

<span data-ttu-id="51045-583">ステートメント`lock`は、指定されたオブジェクトの相互排他ロックを取得し、ステートメントを実行してから、ロックを解放します。</span><span class="sxs-lookup"><span data-stu-id="51045-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="51045-584">@No__t-0 ステートメントの式は、 *reference_type*であることがわかっている型の値を示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="51045-585">@No__t-1 ステートメントの式に対しては、暗黙的なボックス変換 ([ボックス](conversions.md#boxing-conversions)化変換) は行われません。したがって、式が*value_type*の値を示すためにコンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51045-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="51045-586">フォーム`lock`のステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="51045-587">ここで `x` は*reference_type*の式であり、とまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="51045-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
<span data-ttu-id="51045-588">ただし、`x` が評価されるのは 1 回だけです。</span><span class="sxs-lookup"><span data-stu-id="51045-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="51045-589">相互排他ロックが保持されている間、同じ実行スレッドで実行されているコードもロックを取得して解放できます。</span><span class="sxs-lookup"><span data-stu-id="51045-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="51045-590">ただし、他のスレッドで実行されているコードは、ロックが解除されるまでロックを取得できません。</span><span class="sxs-lookup"><span data-stu-id="51045-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="51045-591">静的`System.Type`データへのアクセスを同期するためにオブジェクトをロックすることは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="51045-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="51045-592">他のコードは同じ型をロックし、デッドロックが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="51045-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="51045-593">より適切な方法は、プライベート静的オブジェクトをロックすることによって、静的データへのアクセスを同期することです。</span><span class="sxs-lookup"><span data-stu-id="51045-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="51045-594">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="51045-594">For example:</span></span>
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a><span data-ttu-id="51045-595">using ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-595">The using statement</span></span>

<span data-ttu-id="51045-596">ステートメント`using`は、1つまたは複数のリソースを取得し、ステートメントを実行してから、リソースを破棄します。</span><span class="sxs-lookup"><span data-stu-id="51045-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="51045-597">***リソース***とは、を実装`System.IDisposable`するクラスまたは構造体です。これには、という名前`Dispose`のパラメーターなしのメソッドが1つ含まれます。</span><span class="sxs-lookup"><span data-stu-id="51045-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="51045-598">リソースを使用しているコードは`Dispose` 、リソースが不要になったことを示すためにを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="51045-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="51045-599">が`Dispose`呼び出されていない場合は、ガベージコレクションの結果として、最終的に自動破棄が行われます。</span><span class="sxs-lookup"><span data-stu-id="51045-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="51045-600">*Resource_acquisition*の形式が*local_variable_declaration*の場合、 *local_variable_declaration*の型は `dynamic` であるか、または暗黙的に `System.IDisposable` に変換できる型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="51045-601">*Resource_acquisition*の形式が*expression*の場合、この式は、暗黙的に `System.IDisposable` に変換可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="51045-602">*Resource_acquisition*で宣言されたローカル変数は読み取り専用であり、初期化子を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="51045-603">埋め込みステートメントがこれらの`++`ローカル変数 (代入`--`演算子または演算子を使用して) を変更しようとした場合、コンパイル時エラーが発生した場合`out`は、それらの変数のアドレスを受け取るか、またはパラメーターとして`ref`渡します。</span><span class="sxs-lookup"><span data-stu-id="51045-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="51045-604">`using`ステートメントは、取得、使用、および破棄という3つの部分に変換されます。</span><span class="sxs-lookup"><span data-stu-id="51045-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="51045-605">リソースの使用は、句を`try` `finally`含むステートメントで暗黙的に囲まれます。</span><span class="sxs-lookup"><span data-stu-id="51045-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="51045-606">この`finally`句はリソースを破棄します。</span><span class="sxs-lookup"><span data-stu-id="51045-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="51045-607">リソースが取得された場合、へ`Dispose`の呼び出しは行われず、例外はスローされません。 `null`</span><span class="sxs-lookup"><span data-stu-id="51045-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="51045-608">リソースの種類`dynamic`がである場合は、使用前に変換が成功したことを確認する`IDisposable`ために、暗黙的な動的変換 (暗黙の[動的](conversions.md#implicit-dynamic-conversions)変換) によって取得時に動的に変換されます。処分.</span><span class="sxs-lookup"><span data-stu-id="51045-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="51045-609">フォーム`using`のステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="51045-610">考えられる3つの展開のいずれかに対応します。</span><span class="sxs-lookup"><span data-stu-id="51045-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="51045-611">が`ResourceType` null 非許容の値型である場合、拡張は</span><span class="sxs-lookup"><span data-stu-id="51045-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="51045-612">それ以外の`ResourceType`場合、が null 許容値型または以外`dynamic`の参照型である場合、展開は</span><span class="sxs-lookup"><span data-stu-id="51045-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="51045-613">それ以外の`ResourceType`場合`dynamic`、がの場合、拡張は</span><span class="sxs-lookup"><span data-stu-id="51045-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

<span data-ttu-id="51045-614">どちらの展開でも`resource` 、埋め込みステートメント`d`では変数が読み取り専用になり、埋め込みステートメントでは変数にアクセスできず、非表示になります。</span><span class="sxs-lookup"><span data-stu-id="51045-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="51045-615">実装では、指定された using ステートメントを異なる方法で実装することができます。たとえば、パフォーマンス上の理由から、動作が上記の拡張と一致している場合に限ります。</span><span class="sxs-lookup"><span data-stu-id="51045-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="51045-616">フォーム`using`のステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="51045-617">では、3つの展開が可能です。</span><span class="sxs-lookup"><span data-stu-id="51045-617">has the same three possible expansions.</span></span> <span data-ttu-id="51045-618">この場合`ResourceType` 、は、暗黙的にのコンパイル時の型`expression`です (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="51045-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="51045-619">それ以外の`IDisposable`場合は、 `ResourceType`インターフェイス自体がとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="51045-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="51045-620">`resource`変数は、埋め込みステートメントではアクセスできず、非表示になります。</span><span class="sxs-lookup"><span data-stu-id="51045-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="51045-621">*Resource_acquisition*が*local_variable_declaration*の形式を取る場合、特定の種類のリソースを複数取得することができます。</span><span class="sxs-lookup"><span data-stu-id="51045-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="51045-622">フォーム`using`のステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="51045-623">は、入れ子になっ`using`たステートメントのシーケンスとまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="51045-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="51045-624">次の例では、と`log.txt`いう名前のファイルを作成し、ファイルに2行のテキストを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="51045-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="51045-625">この例では、同じファイルを読み取り用に開き、含まれているテキスト行をコンソールにコピーします。</span><span class="sxs-lookup"><span data-stu-id="51045-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

<span data-ttu-id="51045-626">クラスと`TextWriter` `TextReader`クラスは`IDisposable`インターフェイスを実装するため、この例`using`ではステートメントを使用して、書き込み操作または読み取り操作の後で、基になるファイルが適切に閉じられるようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="51045-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="51045-627">Yield ステートメント</span><span class="sxs-lookup"><span data-stu-id="51045-627">The yield statement</span></span>

<span data-ttu-id="51045-628">この`yield`ステートメントは、反復子オブジェクト ([列挙子オブジェクト](classes.md#enumerator-objects)) または反復子の列挙可能なオブジェクト ([列挙可能オブジェクト](classes.md#enumerable-objects))に値を生成したり、反復処理の終了を通知したりするために、反復子ブロック ([ブロック](statements.md#blocks)) で使用されます。</span><span class="sxs-lookup"><span data-stu-id="51045-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="51045-629">`yield`は予約語ではありません。または`return` `break`キーワードの直前に使用された場合にのみ、特別な意味を持ちます。</span><span class="sxs-lookup"><span data-stu-id="51045-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="51045-630">他のコンテキストで`yield`は、を識別子として使用できます。</span><span class="sxs-lookup"><span data-stu-id="51045-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="51045-631">次に示すように、ステートメント`yield`を表示できる場所にはいくつかの制限があります。</span><span class="sxs-lookup"><span data-stu-id="51045-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="51045-632">@No__t-0 ステートメント (いずれかの形式) が*method_body*、 *operator_body* 、または*accessor_body*の外部に表示される場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="51045-633">`yield`ステートメント (いずれかの形式) が匿名関数内に出現する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="51045-634">ステートメントが`yield` ステートメント`try`の`finally`句に記述されている場合は、コンパイル時にエラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="51045-635">`yield return`ステートメントが`try` 任意`catch`の句を含むステートメント内の任意の場所に出現する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51045-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="51045-636">次の例では、ステートメントの`yield`有効な使用方法と無効な使用方法を示します。</span><span class="sxs-lookup"><span data-stu-id="51045-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

<span data-ttu-id="51045-637">暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の`yield return`変換) は、ステートメント内の式の型から、反復子の yield 型 ([yield 型](classes.md#yield-type)) に存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="51045-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="51045-638">`yield return`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-639">ステートメントで指定された式が評価され、暗黙的に yield 型に変換され`Current` 、列挙子オブジェクトのプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="51045-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="51045-640">反復子ブロックの実行が中断されています。</span><span class="sxs-lookup"><span data-stu-id="51045-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="51045-641">ステートメントが1つ`try`以上のブロック内にある場合、 `finally`関連付けられたブロックは現時点では実行されません。 `yield return`</span><span class="sxs-lookup"><span data-stu-id="51045-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="51045-642">列挙`MoveNext`子オブジェクトのメソッドは、 `true`その呼び出し元にを返します。これは、列挙子オブジェクトが次の項目に正常に進んだことを示します。</span><span class="sxs-lookup"><span data-stu-id="51045-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="51045-643">次に列挙子オブジェクトの`MoveNext`メソッドを呼び出すと、最後に中断された位置から反復子ブロックの実行が再開されます。</span><span class="sxs-lookup"><span data-stu-id="51045-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="51045-644">`yield break`ステートメントは次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="51045-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="51045-645">`finally` `try` `try`ステートメントが、関連付けられた`finally`ブロックがある1つ以上のブロックによって囲まれている場合、最初にコントロールが最も内側のステートメントのブロックに転送されます。 `yield break`</span><span class="sxs-lookup"><span data-stu-id="51045-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="51045-646">コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="51045-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="51045-647">このプロセスは、 `finally`外側`try`のすべてのステートメントのブロックが実行されるまで繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="51045-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="51045-648">コントロールは、反復子ブロックの呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="51045-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="51045-649">これは、列挙`MoveNext`子オブジェクト`Dispose`のメソッドまたはメソッドのいずれかです。</span><span class="sxs-lookup"><span data-stu-id="51045-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="51045-650">ステートメントは`yield break`無条件で制御を別の場所に転送するの`yield break`で、ステートメントのエンドポイントに到達できません。</span><span class="sxs-lookup"><span data-stu-id="51045-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
