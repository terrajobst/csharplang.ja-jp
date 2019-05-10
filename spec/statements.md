---
ms.openlocfilehash: 8f9551b9e7f70379836c23a60f0d37dc02f8e18e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488811"
---
# <a name="statements"></a><span data-ttu-id="e1c3d-101">ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-101">Statements</span></span>

<span data-ttu-id="e1c3d-102">C# には、さまざまなステートメントが用意されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-102">C# provides a variety of statements.</span></span> <span data-ttu-id="e1c3d-103">これらのステートメントのほとんどは、C および C++ でプログラミングする開発者にとって馴染み深いになります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

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

<span data-ttu-id="e1c3d-104">*Embedded_statement*非終端要素は他のステートメント内に表示されるステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="e1c3d-105">使用*embedded_statement*なく*ステートメント*宣言ステートメントとラベル付きステートメントでこれらのコンテキストの使用は含まれません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="e1c3d-106">例では、</span><span class="sxs-lookup"><span data-stu-id="e1c3d-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="e1c3d-107">ため、コンパイル時エラーの結果、`if`ステートメントが必要です、 *embedded_statement*なく*ステートメント*場合にそのブランチ。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="e1c3d-108">このコードが許可された場合、変数`i`は宣言されますが、使用することはありませんでした。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="e1c3d-109">ただし、配置している`i`の例が有効では、ブロックで宣言します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="e1c3d-110">エンドポイントと到達可能性</span><span class="sxs-lookup"><span data-stu-id="e1c3d-110">End points and reachability</span></span>

<span data-ttu-id="e1c3d-111">すべてのステートメントが、***終点***します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="e1c3d-112">直感的な用語では、ステートメントの終了点は、ステートメントの直後に場所です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="e1c3d-113">複合ステートメント (埋め込みステートメントを含んでいるステートメント) の実行のルールは、コントロールが埋め込みステートメントの終点に達したときに行われるアクションを指定します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="e1c3d-114">たとえば、コントロールには、ブロック内のステートメントの終了点に達すると、ブロックに次のステートメントに制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="e1c3d-115">ステートメントを実行してに可能性があることができます、ステートメントがあるとは***到達***します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="e1c3d-116">逆に、ステートメントが実行される可能性がない場合、ステートメントと呼ばれます***到達できない***します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="e1c3d-117">例</span><span class="sxs-lookup"><span data-stu-id="e1c3d-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="e1c3d-118">2 番目の呼び出しの`Console.WriteLine`ステートメントが実行される可能性がないために到達できません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="e1c3d-119">ステートメントに到達できることをコンパイラが判断した場合、警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="e1c3d-120">具体的にはエラーではなく到達できないステートメントにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="e1c3d-121">特定のステートメントまたはエンドポイントが到達可能かどうかを確認するのには、コンパイラは、各ステートメントに対して定義されている到達可能性の規則に従ってフロー分析を行います。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="e1c3d-122">フロー解析では、定数式の値を考慮に入れます ([定数式](expressions.md#constant-expressions)) ステートメントの動作を制御するが、非定数式で使用できる値は考慮されません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="e1c3d-123">つまり、制御フロー分析のために、指定された型の非定数式がその型のすべての値を持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="e1c3d-124">例</span><span class="sxs-lookup"><span data-stu-id="e1c3d-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="e1c3d-125">ブール式、`if`ために、ステートメントが定数式のオペランドは両方とも、`==`演算子は定数。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="e1c3d-126">定数式は、コンパイル時に評価されるとは、値を生成`false`、`Console.WriteLine`呼び出しは到達不能と見なされます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="e1c3d-127">ただし場合、`i`ローカル変数にするのには</span><span class="sxs-lookup"><span data-stu-id="e1c3d-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="e1c3d-128">`Console.WriteLine`実際が実行されない場合でも呼び出しが到達可能が考慮されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="e1c3d-129">*ブロック*関数のメンバーは常と見なされます到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="e1c3d-130">ブロック内の各ステートメントの到達可能性の規則を連続して評価するには、任意のステートメントの到達可能性を判断できます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="e1c3d-131">例</span><span class="sxs-lookup"><span data-stu-id="e1c3d-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="e1c3d-132">2 番目の到達可能性`Console.WriteLine`は次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="e1c3d-133">最初の`Console.WriteLine`式ステートメントに到達できるためのブロック、`F`メソッドに到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="e1c3d-134">1 つ目の終了点`Console.WriteLine`そのステートメントに到達するために、式のステートメントに到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="e1c3d-135">`if`ステートメントは、最初の最後のポイントため到達`Console.WriteLine`式ステートメントに到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="e1c3d-136">2 番目の`Console.WriteLine`式ステートメントに到達できるためのブール式、`if`ステートメントには、定数の値はありません。`false`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="e1c3d-137">2 つの状況に到達可能で、ステートメントの終了点のコンパイル時エラーがあります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="e1c3d-138">`switch`ステートメントでは、次の switch セクションに「フォール スルー」する switch セクションが許可されていませんはの switch セクションに到達可能で、ステートメント リストの終点のコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="e1c3d-139">示す値では通常このエラーが発生した場合、`break`ステートメントがありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="e1c3d-140">最後の位置に到達可能で値を計算する関数メンバーのブロックのコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="e1c3d-141">示す値を通常は、このエラーが発生した場合、`return`ステートメントがありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="e1c3d-142">ブロック</span><span class="sxs-lookup"><span data-stu-id="e1c3d-142">Blocks</span></span>

<span data-ttu-id="e1c3d-143">"*ブロック*" を使用すると、1 つのステートメントしか使用できないコンテキストで複数のステートメントを記述できます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="e1c3d-144">A*ブロック*省略可能なから成る*statement_list* ([ステートメントが一覧表示](statements.md#statement-lists)) 中かっこで囲まれている。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="e1c3d-145">ステートメントの一覧を省略した場合、ブロックは空にすると言います。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="e1c3d-146">ブロックは、宣言ステートメントを含めることができます ([宣言ステートメント](statements.md#declaration-statements))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="e1c3d-147">ローカル変数または定数のスコープ、ブロックで宣言されているブロックです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="e1c3d-148">ブロックは、次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-149">ブロックが空の場合、ブロックの終了ポイントに制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="e1c3d-150">ブロックが空でない場合は、ステートメントの一覧に制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="e1c3d-151">コントロールが、ステートメント リストの最後のポイントに達すると、コントロールは、ブロックの終了ポイントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="e1c3d-152">ブロックのステートメントの一覧は、到達可能なブロック自体が到達可能な場合です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="e1c3d-153">ブロックが空の場合、またはステートメント リストのエンドポイントが到達可能な場合、ブロックの終了点が到達可能です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="e1c3d-154">A*ブロック*1 つまたは複数を格納している`yield`ステートメント ([yield ステートメント](statements.md#the-yield-statement))、反復子ブロックと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="e1c3d-155">反復子ブロックは、反復子としての関数メンバーを実装するために使用 ([反復子](classes.md#iterators))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="e1c3d-156">反復子ブロックにいくつか追加の制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="e1c3d-157">コンパイル時エラーには、`return`反復子ブロックに表示されるステートメント (が`yield return`ステートメントは許可されます)。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="e1c3d-158">Unsafe コンテキストを格納する反復子ブロックのコンパイル時エラーです ([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="e1c3d-159">反復子ブロックは、その宣言が、unsafe コンテキストで入れ子になった場合でも常に、安全なコンテキストを定義します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="e1c3d-160">ステートメントの一覧</span><span class="sxs-lookup"><span data-stu-id="e1c3d-160">Statement lists</span></span>

<span data-ttu-id="e1c3d-161">A***ステートメント リスト***シーケンスで記述された 1 つまたは複数のステートメントで構成されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="e1c3d-162">発生するステートメント リスト*ブロック*s ([ブロック](statements.md#blocks)) し、 *switch_block*s ([switch ステートメント](statements.md#the-switch-statement))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="e1c3d-163">ステートメントの一覧は、最初のステートメントに制御を転送することによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="e1c3d-164">コントロールが、ステートメントの終了点に達すると、制御は次のステートメントに移ります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="e1c3d-165">コントロールが最後のステートメントの終了点に達すると、コントロールは、ステートメント リストのエンドポイントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="e1c3d-166">ステートメントの一覧内のステートメントは、到達できるは、次の少なくとも 1 つが true の場合です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="e1c3d-167">ステートメントが最初のステートメントと、ステートメント リスト自体に到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="e1c3d-168">前のステートメントの終了点に到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="e1c3d-169">ステートメントはラベル付きステートメントであり、到達可能で、ラベルが参照されている`goto`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="e1c3d-170">一覧の最後のステートメントの終了点に到達できる場合、ステートメント リストの終点に到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="e1c3d-171">空のステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-171">The empty statement</span></span>

<span data-ttu-id="e1c3d-172">*Empty_statement*何も行われません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="e1c3d-173">空のステートメントは、操作がないコンテキストで実行するステートメントが必要になるときに使用されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="e1c3d-174">空のステートメントの実行は、単に、ステートメントの終了点にコントロールを転送します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="e1c3d-175">したがって、空のステートメントの終了点は、到達可能な空のステートメントに到達できる場合は。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="e1c3d-176">書き込み時に空のステートメントを使用できます、`while`本体のステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="e1c3d-177">空のステートメントを使用して、終了直前にラベルを宣言することも、"`}`"ブロックの。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="e1c3d-178">ラベル付きステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-178">Labeled statements</span></span>

<span data-ttu-id="e1c3d-179">A *labeled_statement*ラベルによってプレフィックス指定するステートメントを許可します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="e1c3d-180">ラベル付きステートメントはブロックでは、許可されますが、埋め込みステートメントとしては使用できません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="e1c3d-181">ラベル付きステートメントで指定された名前のラベルを宣言して、*識別子*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="e1c3d-182">ラベルのスコープは、ブロック全体を入れ子になったブロックを含め、ラベルが宣言されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="e1c3d-183">重複するスコープが同じ名前の 2 つのラベルのコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="e1c3d-184">ラベルはから参照できる`goto`ステートメント ([goto ステートメント](statements.md#the-goto-statement)) ラベルのスコープ内で。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="e1c3d-185">つまり、`goto`ステートメント ブロック内で、ブロック、もののブロックにないコントロールを転送できます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="e1c3d-186">ラベルは、独自の宣言領域があるし、他の識別子に干渉することはできません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="e1c3d-187">例では、</span><span class="sxs-lookup"><span data-stu-id="e1c3d-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="e1c3d-188">有効であり、名前を使用して`x`パラメーターとラベルの両方として。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="e1c3d-189">ラベル付きステートメントの実行は、ラベルを次のステートメントの実行に正確に対応します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="e1c3d-190">ラベル付きステートメントに到達できるは、ラベルが、到達可能で参照されている場合にだけでなく通常の制御フローによって提供される到達可能性、`goto`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="e1c3d-191">(例外。場合、`goto`内のステートメントは、`try`を含む、`finally`ブロック、およびラベル付きステートメントが範囲外です、`try`との終点、`finally`ブロックにアクセスし、ラベル付きステートメントにから到達できません`goto`ステートメントです)。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="e1c3d-192">宣言ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-192">Declaration statements</span></span>

<span data-ttu-id="e1c3d-193">A *declaration_statement*ローカル変数または定数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="e1c3d-194">宣言ステートメントはブロックでは、許可されますが、埋め込みステートメントとしては使用できません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="e1c3d-195">ローカル変数の宣言</span><span class="sxs-lookup"><span data-stu-id="e1c3d-195">Local variable declarations</span></span>

<span data-ttu-id="e1c3d-196">A *local_variable_declaration* 1 つまたは複数のローカル変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-196">A *local_variable_declaration* declares one or more local variables.</span></span>

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

<span data-ttu-id="e1c3d-197">*Local_variable_type*の*local_variable_declaration*か、直接、宣言によって導入される変数の型を指定しますまたは、識別子を持つことを示します`var`です初期化子に基づいて型を推論する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="e1c3d-198">型がのリストが続く*local_variable_declarator*s、新しい変数をそれぞれが導入されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="e1c3d-199">A *local_variable_declarator*から成る、*識別子*必要に応じて、変数の名前を示す、"`=`"トークンと*local_variable_initializer*変数の初期値を提供します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="e1c3d-200">コンテキスト キーワードとして、ローカル変数宣言のコンテキストで識別子 var が機能します ([キーワード](lexical-structure.md#keywords))。ときに、 *local_variable_type*として指定されて`var`とという名前のない型`var`は宣言は、スコープ内、***ローカル変数宣言を暗黙的に型指定された***型があります。関連付け初期化子式の型から推論されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="e1c3d-201">暗黙的に型指定されたローカル変数の宣言は、次の制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="e1c3d-202">*Local_variable_declaration*複数を含めることはできません*local_variable_declarator*秒。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="e1c3d-203">*Local_variable_declarator*含める必要があります、 *local_variable_initializer*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="e1c3d-204">*Local_variable_initializer*必要があります、*式*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="e1c3d-205">初期化子*式*コンパイル時の型があります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="e1c3d-206">初期化子*式*自体、宣言された変数は参照できません</span><span class="sxs-lookup"><span data-stu-id="e1c3d-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="e1c3d-207">正しくない暗黙的に型指定されたローカル変数宣言の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="e1c3d-208">ローカル変数の値が使用する式で取得した、 *simple_name* ([簡易名](expressions.md#simple-names)) を使用してローカル変数の値を変更し、*割り当て*([代入演算子](expressions.md#assignment-operators))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="e1c3d-209">ローカル変数を明示的に代入する必要があります ([確実な代入](variables.md#definite-assignment)) 各場所、その値を取得します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="e1c3d-210">宣言されたローカル変数のスコープを*local_variable_declaration*ブロックで宣言が発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="e1c3d-211">前にあるテキストの位置にローカル変数を参照するとエラーには、 *local_variable_declarator*のローカル変数。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="e1c3d-212">ローカル変数のスコープ内では、別のローカル変数や定数を同じ名前で宣言すると、コンパイル時エラーを勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="e1c3d-213">複数の変数を宣言して、ローカル変数の宣言は、同じ型を持つ 1 つの変数の複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="e1c3d-214">さらに、ローカル変数宣言で変数の初期化子は、宣言の直後に挿入されている代入ステートメントに正確に対応します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="e1c3d-215">例では、</span><span class="sxs-lookup"><span data-stu-id="e1c3d-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="e1c3d-216">対応します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="e1c3d-217">暗黙的に型指定されたローカル変数宣言で宣言されているローカル変数の型を表示して、変数を初期化するために使用する式の型と同じであります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="e1c3d-218">例:</span><span class="sxs-lookup"><span data-stu-id="e1c3d-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="e1c3d-219">上、暗黙的に型指定されたローカル変数宣言は、明示的に型指定された次の宣言にまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="e1c3d-220">ローカル定数宣言</span><span class="sxs-lookup"><span data-stu-id="e1c3d-220">Local constant declarations</span></span>

<span data-ttu-id="e1c3d-221">A *local_constant_declaration* 1 つまたは複数のローカル定数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-221">A *local_constant_declaration* declares one or more local constants.</span></span>

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

<span data-ttu-id="e1c3d-222">*型*の*local_constant_declaration*宣言によって導入される定数の型を指定します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="e1c3d-223">型がのリストが続く*constant_declarator*s、新しい定数をそれぞれが導入されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="e1c3d-224">A *constant_declarator*から成る、*識別子*でその名前、定数の後に、"`=`"トークン、その後に、 *constant_expression* ([定数式](expressions.md#constant-expressions))、定数の値を提供します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="e1c3d-225">*型*と*constant_expression*ローカル定数宣言の定数メンバー宣言のものと同じ規則に従う必要があります ([定数](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="e1c3d-226">ローカル定数の値が使用する式で取得した、 *simple_name* ([簡易名](expressions.md#simple-names))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="e1c3d-227">ローカル定数のスコープは、ブロックの宣言が発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="e1c3d-228">前にあるテキストの位置でのローカル定数を参照するとエラーにはその*constant_declarator*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="e1c3d-229">ローカル定数のスコープ内では、別のローカル変数や定数を同じ名前で宣言すると、コンパイル時エラーを勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="e1c3d-230">複数の定数を宣言するためのローカル定数宣言では、単一の定数のと同じ型の複数の宣言に相当します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="e1c3d-231">式ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-231">Expression statements</span></span>

<span data-ttu-id="e1c3d-232">*Expression_statement*指定された式を評価します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="e1c3d-233">値は、式で計算、存在する場合は破棄されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-233">The value computed by the expression, if any, is discarded.</span></span>

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

<span data-ttu-id="e1c3d-234">ステートメントとしては、すべての式を使用します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="e1c3d-235">など、特定の式で`x + y`と`x == 1`ことだけです (これは破棄されます) 値を計算する、ステートメントとしては使用できません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="e1c3d-236">実行、 *expression_statement*内の式を評価しの終点に制御を転送、 *expression_statement*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="e1c3d-237">終点を*expression_statement*が到達可能な場合は、その*expression_statement*に到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="e1c3d-238">選択ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-238">Selection statements</span></span>

<span data-ttu-id="e1c3d-239">選択ステートメントでは、いくつかの式の値に基づいて、実行可能ステートメントの数値の 1 つを選択します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="e1c3d-240">If ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-240">The if statement</span></span>

<span data-ttu-id="e1c3d-241">`if`ステートメントは、ブール式の値に基づいて実行するステートメントを選択します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="e1c3d-242">`else`パーツは構文的に最も近い先行関連付け`if`構文で許可されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="e1c3d-243">つまり、`if`形式のステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="e1c3d-244">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-244">is equivalent to</span></span>
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

<span data-ttu-id="e1c3d-245">`if`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-246">*Boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="e1c3d-247">ブール式の結果が場合`true`、埋め込みの最初のステートメントに制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="e1c3d-248">コントロールがの終了ポイントに転送されるコントロールがそのステートメントの終了点に達すると、`if`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="e1c3d-249">ブール式の結果が場合`false`場合に、`else`部分が存在する、2 番目の埋め込みステートメントに制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="e1c3d-250">コントロールがの終了ポイントに転送されるコントロールがそのステートメントの終了点に達すると、`if`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="e1c3d-251">ブール式の結果が場合`false`場合に、`else`部分が存在しないの終点に制御が移ります、`if`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="e1c3d-252">最初のステートメントの埋め込み、`if`ステートメントに到達できる場合、`if`ステートメントに到達でき、ブール型の式が定数の値を持たない`false`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="e1c3d-253">2 番目のステートメントの埋め込み、`if`ステートメントで存在する場合の指定が到達可能な場合、`if`ステートメントに到達でき、ブール型の式が定数の値を持たない`true`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="e1c3d-254">終点を`if`ステートメントが到達可能な場合、その埋め込みステートメントの少なくとも 1 つの終点に到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="e1c3d-255">末尾がさらに、ポイント、`if`ステートメントなしで`else`部品に到達できる場合、`if`ステートメントに到達でき、ブール型の式が定数の値を持たない`true`。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="e1c3d-256">Switch ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-256">The switch statement</span></span>

<span data-ttu-id="e1c3d-257">Switch ステートメントは、実行の switch 式の値に対応するスイッチが関連付けられているラベルを持つステートメントの一覧を選択します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

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

<span data-ttu-id="e1c3d-258">A *switch_statement*キーワードから成る`switch`の後にかっこで囲まれた式が (switch 式と呼ばれます)、その後、 *switch_block*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="e1c3d-259">*Switch_block* 0 個以上から成る*switch_section*s、中かっこで囲みます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="e1c3d-260">各*switch_section* 1 つまたは複数から成る*switch_label*秒後に、 *statement_list* ([ステートメントが一覧表示](statements.md#statement-lists))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="e1c3d-261">***の種類を制御する***の`switch`ステートメントは、switch 式で確立されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="e1c3d-262">Switch 式の型が場合`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `bool`、 `char`、 `string`、または、 *enum_type*のかどうかは、これらの型のいずれかに対応する null 許容型では、管理方法はまたはの入力、`switch`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="e1c3d-263">それ以外の場合、1 つだけユーザー定義の暗黙の変換 ([ユーザー定義の変換](conversions.md#user-defined-conversions)) switch 式の型から型を制御する次の候補のいずれかに存在する必要があります: `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `string`、またはそれらの型のいずれかに対応する null 許容型。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="e1c3d-264">それ以外の場合、このような暗黙的な変換が存在しないか、このような 1 つの暗黙的な変換が存在する数より多い場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="e1c3d-265">それぞれの定数式`case`ラベルは、暗黙的に変換する値を表す必要があります ([暗黙的な変換](conversions.md#implicit-conversions)) の管理の型を`switch`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="e1c3d-266">2 つ以上の場合、コンパイル時エラーが発生した`case`同じラベル`switch`ステートメントで同じ定数値を指定します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="e1c3d-267">最大で 1 つできます`default`switch ステートメントでラベル。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="e1c3d-268">A`switch`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-269">Switch 式が評価され、管理の型に変換します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="e1c3d-270">定数のいずれかが指定されている場合、`case`同じラベル`switch`ステートメントが switch 式の値と等しく、一致する、次のステートメントの一覧に制御が移ります`case`ラベル。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="e1c3d-271">場合に指定された定数のいずれも`case`同じラベル`switch`ステートメントは、switch 式の値と等しい場合に、`default`ラベルが存在する、ステートメント リストの次に制御が移ります、 `default`ラベル。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="e1c3d-272">場合に指定された定数のいずれも`case`同じラベル`switch`ステートメントは、switch 式の値と等しいされていない場合`default`ラベルが存在するの終点に制御が移ります、`switch`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="e1c3d-273">Switch セクションのステートメント リストのエンドポイントが到達可能な場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="e1c3d-274">これは、「フォール スルー」ルールと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="e1c3d-275">例では、</span><span class="sxs-lookup"><span data-stu-id="e1c3d-275">The example</span></span>
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
<span data-ttu-id="e1c3d-276">switch セクションに到達可能なエンドポイントがあるないため、このプロパティの値は有効です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="e1c3d-277">C や C++ とは異なり、switch セクションの実行は許可されていません「フォール スルー」に次の switch セクションを例では、</span><span class="sxs-lookup"><span data-stu-id="e1c3d-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
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
<span data-ttu-id="e1c3d-278">コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-278">results in a compile-time error.</span></span> <span data-ttu-id="e1c3d-279">Switch セクションの実行が明示的なもう 1 つの switch セクションの実行後に、`goto case`または`goto default`ステートメントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
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

<span data-ttu-id="e1c3d-280">複数のラベルが許可されている、 *switch_section*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="e1c3d-281">例では、</span><span class="sxs-lookup"><span data-stu-id="e1c3d-281">The example</span></span>
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
<span data-ttu-id="e1c3d-282">有効です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-282">is valid.</span></span> <span data-ttu-id="e1c3d-283">この例ではためにを「フォール スルー」ルールに違反しないラベル`case 2:`と`default:`同じの一部である*switch_section*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="e1c3d-284">「フォール スルー」ルールが C および C++ で発生するバグの一般的なクラスと`break`ステートメントが誤ってを省略するとします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="e1c3d-285">さらに、このルールでは、switch セクションにより、`switch`ステートメントにステートメントの動作に影響を与えずに任意に置くことです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="e1c3d-286">セクションなど、`switch`上記のステートメントは、ステートメントの動作に影響を与えずに元に戻すことができます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
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

<span data-ttu-id="e1c3d-287">Switch セクションのステートメントの一覧は、通常で終わる、 `break`、 `goto case`、または`goto default`ステートメントが到達できないステートメント リストの最後のポイントを表示する構成要素を許可します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="e1c3d-288">たとえば、`while`ブール式で制御ステートメント`true`エンドポイントに正常には到達しません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="e1c3d-289">同様に、`throw`または`return`ステートメントが常に別の場所に制御を転送し、その終点に到達しません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="e1c3d-290">したがって、次の例では有効です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-290">Thus, the following example is valid:</span></span>
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

<span data-ttu-id="e1c3d-291">管理の型を`switch`ステートメントは、型、`string`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="e1c3d-292">例:</span><span class="sxs-lookup"><span data-stu-id="e1c3d-292">For example:</span></span>
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

<span data-ttu-id="e1c3d-293">などの文字列の等値演算子 ([文字列等値演算子](expressions.md#string-equality-operators))、`switch`ステートメントが大文字小文字を区別し、switch 式の文字列が完全に一致する場合にのみに指定されたスイッチ セクションを実行する`case`ラベル定数。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="e1c3d-294">管理方法が入力すると、`switch`ステートメントが`string`、値`null`case ラベル定数として許可されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="e1c3d-295">*Statement_list*の s を*switch_block*宣言ステートメントを含めることができます ([宣言ステートメント](statements.md#declaration-statements))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="e1c3d-296">ローカル変数または定数のスコープの switch ブロックで宣言されている、スイッチ ブロックです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="e1c3d-297">Switch セクションのステートメントの一覧に到達できる場合、`switch`ステートメントに到達し、次の少なくとも 1 つが true:</span><span class="sxs-lookup"><span data-stu-id="e1c3d-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="e1c3d-298">Switch 式は、非定数値です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="e1c3d-299">Switch 式が定数の値と一致する、 `case` switch セクション内のラベル。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="e1c3d-300">Switch 式は定数値と一致しない`case`ラベル、および switch セクションが含まれています、`default`ラベル。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="e1c3d-301">Switch セクションの switch ラベルは、到達によって参照される`goto case`または`goto default`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="e1c3d-302">終点を`switch`ステートメントに到達できるは、次の少なくとも 1 つが true の場合。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="e1c3d-303">`switch`ステートメントが含まれていますが、到達可能な`break`を終了させるステートメント、`switch`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="e1c3d-304">`switch`ステートメントに到達、switch 式は非定数値、および no`default`ラベルが存在します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="e1c3d-305">`switch`ステートメントに到達、switch 式は定数値と一致しない`case`ラベル、および no`default`ラベルが存在します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="e1c3d-306">繰り返しステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-306">Iteration statements</span></span>

<span data-ttu-id="e1c3d-307">繰り返しステートメントでは、埋め込みステートメントを繰り返し実行します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="e1c3d-308">While ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-308">The while statement</span></span>

<span data-ttu-id="e1c3d-309">`while`ステートメントは条件付きで 0 回以上の埋め込みステートメントを実行します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="e1c3d-310">A`while`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-311">*Boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="e1c3d-312">ブール式の結果が場合`true`、埋め込みステートメントに制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="e1c3d-313">コントロールが埋め込みステートメントの終了点に達すると (通常の実行から、`continue`ステートメント) の先頭に制御が移ります、`while`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="e1c3d-314">ブール式の結果が場合`false`の終点に制御が移ります、`while`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="e1c3d-315">内の埋め込みステートメントを`while`ステートメントを`break`ステートメント ([break ステートメント](statements.md#the-break-statement)) の終点に制御を転送できる、 `while` (埋め込みの繰り返しを終了するステートメントステートメントの場合)、および`continue`ステートメント ([continue ステートメント](statements.md#the-continue-statement)) に埋め込みステートメントの終了ポイントに制御を移すことができます (の別のイテレーションを実行するため、`while`ステートメント)。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="e1c3d-316">埋め込みステートメントを`while`ステートメントに到達できる場合、`while`ステートメントに到達でき、ブール型の式が定数の値を持たない`false`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="e1c3d-317">終点を`while`ステートメントに到達できるは、次の少なくとも 1 つが true の場合。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="e1c3d-318">`while`ステートメントが含まれていますが、到達可能な`break`を終了させるステートメント、`while`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="e1c3d-319">`while`ステートメントに到達でき、ブール型の式が定数の値を持たない`true`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="e1c3d-320">Do ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-320">The do statement</span></span>

<span data-ttu-id="e1c3d-321">`do`ステートメントは条件付きで 1 回以上の埋め込みステートメントを実行します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="e1c3d-322">A`do`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-323">埋め込みステートメントには、制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="e1c3d-324">コントロールが埋め込みステートメントの終了点に達すると (通常の実行から、`continue`ステートメント)、 *boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="e1c3d-325">ブール式の結果が場合`true`の先頭に制御が移ります、`do`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="e1c3d-326">終点に制御が移ります、それ以外の場合、`do`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="e1c3d-327">内の埋め込みステートメントを`do`ステートメントを`break`ステートメント ([break ステートメント](statements.md#the-break-statement)) の終点に制御を転送できる、 `do` (埋め込みの繰り返しを終了するステートメントステートメントの場合)、および`continue`ステートメント ([continue ステートメント](statements.md#the-continue-statement)) に埋め込みステートメントの終了ポイントに制御を移すことができます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="e1c3d-328">埋め込みステートメントを`do`ステートメントに到達できる場合、`do`ステートメントに到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="e1c3d-329">終点を`do`ステートメントに到達できるは、次の少なくとも 1 つが true の場合。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="e1c3d-330">`do`ステートメントが含まれていますが、到達可能な`break`を終了させるステートメント、`do`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="e1c3d-331">埋め込みステートメントの終了ポイントに到達でき、ブール型の式が定数の値を持たない`true`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="e1c3d-332">For ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-332">The for statement</span></span>

<span data-ttu-id="e1c3d-333">`for`ステートメント一連の初期化式を評価し、条件が true の場合、繰り返し埋め込みステートメントを実行し、一連の反復式を評価します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

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

<span data-ttu-id="e1c3d-334">*For_initializer*存在する場合は、いずれかで構成されています、 *local_variable_declaration* ([ローカル変数宣言](statements.md#local-variable-declarations)) の一覧または*statement_式*s ([式ステートメント](statements.md#expression-statements)) コンマで区切られました。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="e1c3d-335">宣言されたローカル変数のスコープを*for_initializer*から始まり、 *local_variable_declarator*変数の埋め込みステートメントの最後までを対象とします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="e1c3d-336">スコープに含まれる、 *for_condition*と*for_iterator*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="e1c3d-337">*For_condition*がある場合があります、 *boolean_expression* ([ブール式](expressions.md#boolean-expressions))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="e1c3d-338">*For_iterator*存在する場合は、一覧から成る*statement_expression*s ([式ステートメント](statements.md#expression-statements)) コンマで区切られました。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="e1c3d-339">次のように FOR ステートメントを実行します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-340">場合、 *for_initializer*が存在する場合は、変数の初期化子またはステートメントの式が記述されている順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="e1c3d-341">この手順は一度だけ実行します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-341">This step is only performed once.</span></span>
*  <span data-ttu-id="e1c3d-342">場合、 *for_condition*が存在する場合は、評価されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="e1c3d-343">場合、 *for_condition*が存在するか、評価が得られます`true`、埋め込みステートメントに制御が移ります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="e1c3d-344">コントロールが埋め込みステートメントの終了点に達すると (通常の実行から、`continue`ステートメント) の式、 *for_iterator*シーケンスで、いずれかが評価され、その他のイテレーションは場合は、以降の評価では、実行、 *for_condition*前の手順でします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="e1c3d-345">場合、 *for_condition*が存在し、評価が得られます`false`の終点に制御が移ります、`for`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="e1c3d-346">内の埋め込みステートメントを`for`ステートメントを`break`ステートメント ([break ステートメント](statements.md#the-break-statement)) の終点に制御を転送できる、 `for` (埋め込みの繰り返しを終了するステートメントステートメントの場合)、および`continue`ステートメント ([continue ステートメント](statements.md#the-continue-statement)) に埋め込みステートメントの終了ポイントに制御を移すことができます (実行するため、 *for_iterator*と別のイテレーションを実行する、`for`ステートメントでは、以降では、 *for_condition*)。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="e1c3d-347">埋め込みステートメントを`for`ステートメントに到達できるは、次のいずれかが true の場合。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="e1c3d-348">`for`ステートメントに到達しないと*for_condition*が存在します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="e1c3d-349">`for`ステートメントに到達し、 *for_condition*が存在し、定数の値を持たない`false`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="e1c3d-350">終点を`for`ステートメントに到達できるは、次の少なくとも 1 つが true の場合。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="e1c3d-351">`for`ステートメントが含まれていますが、到達可能な`break`を終了させるステートメント、`for`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="e1c3d-352">`for`ステートメントに到達し、 *for_condition*が存在し、定数の値を持たない`true`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="e1c3d-353">Foreach ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-353">The foreach statement</span></span>

<span data-ttu-id="e1c3d-354">`foreach`ステートメントは、コレクションの各要素に対して埋め込みステートメントを実行し、コレクションの要素を列挙します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="e1c3d-355">*型*と*識別子*の`foreach`ステートメントの宣言、***反復変数***ステートメントの。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="e1c3d-356">場合、`var`として識別子が指定されて、 *local_variable_type*、という名前のない型と`var`がスコープ内の反復変数と呼ばれます、***暗黙的に型指定された繰り返し変数***、その型がの要素の型にして、`foreach`ステートメントは、以下のようです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="e1c3d-357">反復変数は、埋め込みステートメントを拡張するスコープを持つ読み取り専用のローカル変数に対応します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="e1c3d-358">実行中に、`foreach`ステートメントでは、繰り返し変数は、イテレーションが現在実行中のコレクションの要素を表します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="e1c3d-359">埋め込みステートメントを繰り返し変数を変更しようとすると、コンパイル時エラーが発生します (割り当てまたは`++`と`--`演算子) として繰り返し変数を渡すことも、`ref`または`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="e1c3d-360">次に、簡潔にするため、 `IEnumerable`、 `IEnumerator`、`IEnumerable<T>`と`IEnumerator<T>`名前空間に対応する型を参照してください`System.Collections`と`System.Collections.Generic`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="e1c3d-361">Foreach ステートメントのコンパイル時の処理が最初に決定します、***コレクション型***、***列挙子の型***と***要素型***の式。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="e1c3d-362">この決定は、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="e1c3d-363">場合、型`X`の*式*が配列型からの暗黙的な参照変換は`X`を`IEnumerable`インターフェイス (ため`System.Array`このインターフェイスを実装)。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="e1c3d-364">***コレクション型***は、`IEnumerable`インターフェイスを***列挙子の型***は、`IEnumerator`インターフェイスと***要素型***の要素の型が、配列型`X`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="e1c3d-365">場合、型`X`の*式*は`dynamic`からの暗黙的な変換は*式*を`IEnumerable`インターフェイス ([暗黙的な動的変換](conversions.md#implicit-dynamic-conversions))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="e1c3d-366">***コレクション型***は、`IEnumerable`インターフェイスと***列挙子の型***は、`IEnumerator`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="e1c3d-367">場合、`var`として識別子が指定されて、 *local_variable_type* 、***要素型***は`dynamic`、それ以外の場合は`object`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="e1c3d-368">それ以外の場合、確認するかどうか、型`X`が適切な`GetEnumerator`メソッド。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="e1c3d-369">型のメンバーの参照を実行`X`識別子を持つ`GetEnumerator`と型引数はありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="e1c3d-370">メンバーの検索に一致を生成しない、または、あいまいさを生成します。 または、メソッド グループではない一致は、以下に示すは列挙可能なインターフェイスを確認します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="e1c3d-371">メンバー検索は何も、メソッド グループ、または一致するものを除くが生成される場合に、警告を発行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="e1c3d-372">メソッドの結果として得られるグループと、空の引数リストを使用するオーバー ロードの解決を実行します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="e1c3d-373">適切なメソッドのオーバー ロードの解決結果、か、あいまいな結果しますが、メソッドは、以下に示すように列挙可能なインターフェイスの静的またはパブリックではありませんのいずれかのチェックを 1 つの最適な方法で結果します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="e1c3d-374">オーバー ロードの解決は何も明確なパブリック インスタンス メソッドまたは適切なメソッドを除くが生成される場合に、警告を発行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="e1c3d-375">戻り値の型の場合`E`の`GetEnumerator`メソッドがクラスではない、構造体またはインターフェイス型、エラーが生成および以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="e1c3d-376">メンバー参照がで実行される`E`識別子を持つ`Current`と型引数はありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="e1c3d-377">メンバー検索の一致結果は、結果は、エラーまたは結果が読み取りを許可されているパブリック インスタンス プロパティ以外のすべて、エラーが生成され、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="e1c3d-378">メンバー参照がで実行される`E`識別子を持つ`MoveNext`と型引数はありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="e1c3d-379">メンバー検索の一致結果は、結果は、エラーに対して、または、結果は、メソッド グループ以外のすべての場合は、エラーが発生し、以降の手順は行われません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="e1c3d-380">オーバー ロードの解決は、空の引数リストを持つメソッド グループで実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="e1c3d-381">いない適用可能なメソッド、あいまいな結果または 1 つの最適な方法が、そのメソッドのオーバー ロード解決の結果は静的であるか、パブリックではありません、またはその戻り値の型でない場合`bool`あり、エラーが生成されるため、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="e1c3d-382">***コレクション型***は`X`、***列挙子の型***は`E`、および***要素型***の種類です、`Current`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="e1c3d-383">それ以外の場合、列挙可能なインターフェイスを確認します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="e1c3d-384">すべての種類のデータの場合`Ti`が暗黙的に変換`X`に`IEnumerable<Ti>`、一意の型がある`T`ように`T`ない`dynamic`と他のすべての`Ti`が、暗黙的な変換`IEnumerable<T>`に`IEnumerable<Ti>`、***コレクション型***インターフェイス`IEnumerable<T>`、***列挙子の型***インターフェイス`IEnumerator<T>`、および***要素型***は`T`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="e1c3d-385">それ以外の場合、このような 1 つ以上の型がある場合`T`エラーが発生し、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="e1c3d-386">それ以外の場合からの暗黙的な変換がある場合`X`を`System.Collections.IEnumerable`、インターフェイス、***コレクション型***はこのインターフェイスは、***列挙子の型***インターフェイスは、`System.Collections.IEnumerator`、および***要素型***は`object`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="e1c3d-387">それ以外の場合、エラーが発生し、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="e1c3d-388">上記の手順では、成功した場合、明確に、コレクションの型を生成`C`、列挙子の型`E`および要素の型`T`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="e1c3d-389">フォームの foreach ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="e1c3d-390">を拡張されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-390">is then expanded to:</span></span>
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

<span data-ttu-id="e1c3d-391">変数`e`に表示されるか、式にアクセス`x`埋め込みステートメントまたはプログラムの他のソース コード。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="e1c3d-392">変数`v`は埋め込みステートメントでは読み取り専用です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="e1c3d-393">明示的な変換がない場合 ([明示的な変換](conversions.md#explicit-conversions)) から`T`(要素の型) に`V`(、 *local_variable_type* foreach ステートメント)、エラーが生成されますおよびそれ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="e1c3d-394">場合`x`、値を持つ`null`、`System.NullReferenceException`が実行時にスローされます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="e1c3d-395">上記の拡張と一貫した動作であれば、パフォーマンス上の理由などの異なる方法では、特定の foreach ステートメントを実装するために実装が許可されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="e1c3d-396">配置`v`while 内でループがで発生しているすべての匿名関数がキャプチャ時に重要ですが、 *embedded_statement*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="e1c3d-397">例:</span><span class="sxs-lookup"><span data-stu-id="e1c3d-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="e1c3d-398">場合`v`while の外部で宣言されたループでは、すべてのイテレーションと後に、その値の間で共有は、ループは、最終的な値になります`13`はどのような呼び出し`f`印刷します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="e1c3d-399">代わりに、各反復処理に独自の変数があるため`v`、によってキャプチャされた、1 つ`f`最初の値を保持するイテレーションは引き続き`7`、印刷される内容であります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="e1c3d-400">(注: 以前のバージョンの c# 宣言`v`while の外側のループします)。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="e1c3d-401">本体、ブロックが最後に、次の手順に従って構築されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="e1c3d-402">暗黙的な変換がある場合`E`を`System.IDisposable`し、インターフェイス</span><span class="sxs-lookup"><span data-stu-id="e1c3d-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="e1c3d-403">場合`E`null 非許容値型は、finally 句は、相当する意味的に拡張されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="e1c3d-404">それ以外の場合、finally 句は、相当する意味的に拡張されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="e1c3d-405">場合を除く`E`値の型または値の型のキャストをインスタンス化される型パラメーターは、`e`に`System.IDisposable`が発生するボックス化は発生しません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="e1c3d-406">の場合`E`シールされた型である、finally 句が空のブロックに拡張されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="e1c3d-407">それ以外の場合、finally 句に拡張されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="e1c3d-408">ローカル変数`d`に表示されるか、ユーザーのコードからアクセスします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="e1c3d-409">具体的には、競合しない他の変数がスコープに含まれると、finally ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="e1c3d-410">順序`foreach`配列の要素の走査、次に示します。1 次元配列の要素は、インデックスの昇順で走査するは、インデックスから始まって `0`インデックスで終わる`Length - 1`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="e1c3d-411">多次元配列は、右端の次元のインデックスが増加してから、左側に、[次へ] の左側ディメンションにするように、要素が走査されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="e1c3d-412">次の例は、要素の順序で、2 次元の配列内の各値を印刷します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
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
<span data-ttu-id="e1c3d-413">生成される出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="e1c3d-414">例</span><span class="sxs-lookup"><span data-stu-id="e1c3d-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="e1c3d-415">型`n`推論されます`int`の要素型`numbers`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="e1c3d-416">ジャンプ ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-416">Jump statements</span></span>

<span data-ttu-id="e1c3d-417">ジャンプ ステートメントは、コントロールを無条件で転送します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="e1c3d-418">ジャンプ ステートメントが制御を転送する場所が呼び出された、***ターゲット***ジャンプ ステートメントの。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="e1c3d-419">ジャンプ ステートメントがブロック内で発生し、そのブロックの外側、ジャンプ ステートメントの対象では、ジャンプ ステートメントと言います***終了***ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="e1c3d-420">ジャンプ ステートメント ブロックからコントロールを譲渡、中には、ブロックにコントロールを移動ができることはありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="e1c3d-421">介在するのかどうか、ジャンプ ステートメントの実行は複雑`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="e1c3d-422">このようながない場合は、`try`ジャンプ ステートメントのステートメント、無条件で転送コントロール ジャンプ ステートメントからそのターゲットにします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="e1c3d-423">このような仲介が`try`ステートメントの実行がより複雑な。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="e1c3d-424">ジャンプ ステートメントは、1 つまたは複数が終了した場合`try`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="e1c3d-425">コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="e1c3d-426">までこのプロセスが繰り返されます、`finally`ブロックすべての介在する`try`ステートメントが実行されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="e1c3d-427">例</span><span class="sxs-lookup"><span data-stu-id="e1c3d-427">In the example</span></span>
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
<span data-ttu-id="e1c3d-428">`finally`ブロックに関連付けられている 2 つ`try`コントロールがジャンプ ステートメントのターゲットに転送される前にステートメントが実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="e1c3d-429">生成される出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="e1c3d-430">Break ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-430">The break statement</span></span>

<span data-ttu-id="e1c3d-431">`break`ステートメント終了囲む最も近い`switch`、 `while`、 `do`、 `for`、または`foreach`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="e1c3d-432">ターゲットを`break`ステートメントは外側の終点`switch`、 `while`、 `do`、 `for`、または`foreach`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="e1c3d-433">場合、`break`ステートメントがで囲まれていない、 `switch`、 `while`、 `do`、 `for`、または`foreach`ステートメントでは、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="e1c3d-434">ときに複数`switch`、 `while`、 `do`、 `for`、または`foreach`ステートメントが、互いに入れ子になった、`break`ステートメントは、最も内側のステートメントにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="e1c3d-435">複数の入れ子レベルの制御を転送する、`goto`ステートメント ([goto ステートメント](statements.md#the-goto-statement)) 使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="e1c3d-436">A`break`ステートメントが終了することはできません、`finally`ブロック ([try ステートメント](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="e1c3d-437">ときに、`break`ステートメント内で発生、`finally`のターゲットをブロック、`break`ステートメント内で同じでなければなりません`finally`ブロックは、それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="e1c3d-438">A`break`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-439">場合、`break`ステートメントは、1 つまたは複数が終了した`try`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="e1c3d-440">コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="e1c3d-441">までこのプロセスが繰り返されます、`finally`ブロックすべての介在する`try`ステートメントが実行されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="e1c3d-442">対象に制御が移ります、`break`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="e1c3d-443">`break`ステートメント無条件に制御を転送他の場所での終了点を`break`ステートメントが到達可能ではありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="e1c3d-444">Continue ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-444">The continue statement</span></span>

<span data-ttu-id="e1c3d-445">`continue`ステートメントが最も近い外側の新しいイテレーションを開始`while`、 `do`、 `for`、または`foreach`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="e1c3d-446">ターゲットを`continue`ステートメントは外側の埋め込みステートメントの終了点`while`、 `do`、 `for`、または`foreach`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="e1c3d-447">場合、`continue`ステートメントがで囲まれていない、 `while`、 `do`、 `for`、または`foreach`ステートメントでは、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="e1c3d-448">ときに複数`while`、 `do`、 `for`、または`foreach`ステートメントが、互いに入れ子になった、`continue`ステートメントは、最も内側のステートメントにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="e1c3d-449">複数の入れ子レベルの制御を転送する、`goto`ステートメント ([goto ステートメント](statements.md#the-goto-statement)) 使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="e1c3d-450">A`continue`ステートメントが終了することはできません、`finally`ブロック ([try ステートメント](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="e1c3d-451">ときに、`continue`ステートメント内で発生、`finally`のターゲットをブロック、`continue`ステートメント内で同じでなければなりません`finally`ブロックは、それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="e1c3d-452">A`continue`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-453">場合、`continue`ステートメントは、1 つまたは複数が終了した`try`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="e1c3d-454">コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="e1c3d-455">までこのプロセスが繰り返されます、`finally`ブロックすべての介在する`try`ステートメントが実行されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="e1c3d-456">対象に制御が移ります、`continue`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="e1c3d-457">`continue`ステートメント無条件に制御を転送他の場所での終了点を`continue`ステートメントが到達可能ではありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="e1c3d-458">GoTo ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-458">The goto statement</span></span>

<span data-ttu-id="e1c3d-459">`goto`ステートメントはラベルでマークされているステートメントに制御を転送します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="e1c3d-460">ターゲットを`goto`*識別子*ステートメントは、特定のラベルとラベル付きステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="e1c3d-461">現在の関数のメンバーでは、指定した名前のラベルが存在しない場合、または場合、`goto`ステートメントは、ラベルのスコープ内で、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="e1c3d-462">このルールの使用を許可する、`goto`ステートメントを入れ子になったスコープではなく、入れ子になったスコープ外の制御を転送します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="e1c3d-463">例</span><span class="sxs-lookup"><span data-stu-id="e1c3d-463">In the example</span></span>
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
<span data-ttu-id="e1c3d-464">`goto`ステートメントを使用して、入れ子になったスコープ外の制御を転送します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="e1c3d-465">ターゲットを`goto case`ステートメントがステートメントの一覧で、すぐ外側`switch`ステートメント ([switch ステートメント](statements.md#the-switch-statement)) が含まれています、`case`特定の定数値を持つラベル。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="e1c3d-466">場合、`goto case`ステートメントがで囲まれていない、`switch`ステートメントでは場合、 *constant_expression*は暗黙的に変換できません ([暗黙的な変換](conversions.md#implicit-conversions)) の管理の型をそれを囲む最も近い`switch`ステートメントでは、場合、または最も近い外側`switch`ステートメントを含まない、`case`コンパイル時エラーが発生した特定の定数値でラベルを付けます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="e1c3d-467">ターゲットを`goto default`ステートメントがステートメントの一覧で、すぐ外側`switch`ステートメント ([switch ステートメント](statements.md#the-switch-statement)) が含まれています、`default`ラベル。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="e1c3d-468">場合、`goto default`ステートメントがで囲まれていない、`switch`ステートメントでは、場合囲む最も近い`switch`ステートメントを含まない、`default`ラベル付け、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="e1c3d-469">A`goto`ステートメントが終了することはできません、`finally`ブロック ([try ステートメント](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="e1c3d-470">ときに、`goto`ステートメント内で発生、`finally`のターゲットをブロック、`goto`ステートメント内で同じでなければなりません`finally`ブロック、またはそれ以外の場合、コンパイル時エラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="e1c3d-471">A`goto`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-472">場合、`goto`ステートメントは、1 つまたは複数が終了した`try`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="e1c3d-473">コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="e1c3d-474">までこのプロセスが繰り返されます、`finally`ブロックすべての介在する`try`ステートメントが実行されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="e1c3d-475">対象に制御が移ります、`goto`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="e1c3d-476">`goto`ステートメント無条件に制御を転送他の場所での終了点を`goto`ステートメントが到達可能ではありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="e1c3d-477">Return ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-477">The return statement</span></span>

<span data-ttu-id="e1c3d-478">`return`ステートメントに制御を関数の現在の呼び出し元を返します、`return`ステートメントが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="e1c3d-479">A`return`式ステートメントは、結果の型を持つメソッドは、値を計算しない関数メンバーでのみ使用できます ([メソッド本体](classes.md#method-body)) `void`、`set`プロパティのアクセサーまたはインデクサー、`add`と`remove`イベント、インスタンス コンス トラクター、静的コンス トラクターまたはデストラクターのアクセサー。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="e1c3d-480">A`return`式とステートメントは、void 以外の結果型を持つメソッドは、値を計算する関数のメンバーでのみ使用できます、`get`プロパティまたはインデクサー、またはユーザー定義演算子のアクセサー。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="e1c3d-481">暗黙的な変換 ([暗黙的な変換](conversions.md#implicit-conversions)) 含んでいる関数メンバーの戻り値の型を式の型から存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="e1c3d-482">戻り値のステートメントは、匿名関数式の本文にも使用できます ([匿名関数式](expressions.md#anonymous-function-expressions))、どの変換がこれらの関数の存在の決定に参加します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="e1c3d-483">コンパイル時エラーには、`return`に表示されるステートメントを`finally`ブロック ([try ステートメント](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="e1c3d-484">A`return`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-485">場合、`return`ステートメント、式を指定する式が評価され、結果の値は、暗黙的な変換で含まれる関数の戻り値の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="e1c3d-486">変換の結果では、関数によって生成される結果値になります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="e1c3d-487">場合、`return`ステートメントが 1 つまたは複数で囲まれた`try`または`catch`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="e1c3d-488">コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="e1c3d-489">までこのプロセスが繰り返されます、`finally`外側のすべてのブロック`try`ステートメントが実行されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="e1c3d-490">含まれる関数は、非同期関数ではありません、存在する場合、制御が、結果の値と共に含まれる関数の呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="e1c3d-491">親関数が非同期関数で、現在の呼び出し元に制御が返されますされ、結果の値、存在する場合に記録されますタスクを返す」の説明に従って場合 ([列挙子インターフェイス](classes.md#enumerator-interfaces))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="e1c3d-492">`return`ステートメント無条件に制御を転送他の場所での終了点を`return`ステートメントが到達可能ではありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="e1c3d-493">Throw ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-493">The throw statement</span></span>

<span data-ttu-id="e1c3d-494">`throw`ステートメントが例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="e1c3d-495">A`throw`式を持つステートメント、式を評価することによって生成された値をスローします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="e1c3d-496">式は、クラス型の値を表す必要があります`System.Exception`から派生したクラス型の`System.Exception`または型パラメーターの型を持つの`System.Exception`(またはそのサブクラス)、有効な基本クラスとして。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="e1c3d-497">式の評価が生成される場合`null`、`System.NullReferenceException`が代わりにスローされます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="e1c3d-498">A`throw`式ステートメントでのみ使用できます、`catch`ブロック、そのステートメントが再処理をしている現在の例外をスロー後者`catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="e1c3d-499">`throw`ステートメント無条件に制御を転送他の場所での終了点を`throw`ステートメントが到達可能ではありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="e1c3d-500">最初に制御が移りますが、例外がスローされたときに`catch`句で、それを囲む`try`例外を処理できるステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="e1c3d-501">適切な例外ハンドラーに制御を転送するポイントにスローされる例外のポイントから実行されるプロセスと呼びます***例外の反映***します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="e1c3d-502">まで、次の手順を繰り返し評価するは、例外の反映、`catch`例外に一致する句が見つかりました。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="e1c3d-503">この説明で、***スロー ポイント***で例外がスローされる場所は、最初にします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="e1c3d-504">現在の関数メンバーの各`try`ステートメントを囲むスロー ポイントが調べられます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="e1c3d-505">各ステートメントに対して`S`最も内側以降の`try`ステートメントと、最も外側で終了するまで`try`ステートメントでは、次の手順が評価されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="e1c3d-506">場合、`try`ブロック`S`スロー ポイントを囲む S が 1 つまたは複数`catch`句、`catch`句は、適切なハンドラーに指定された規則に従って、例外を検索する外観の順序でチェックされますセクション[try ステートメント](statements.md#the-try-statement)します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="e1c3d-507">一致する場合`catch`句が見つかると、そのブロックに制御を転送して例外の反映が完了した`catch`句。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="e1c3d-508">の場合、`try`ブロックまたは`catch`のブロック`S`スロー ポイントを囲む場合`S`が、`finally`に転送されるブロック、制御、`finally`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="e1c3d-509">場合、`finally`ブロックは、別の例外をスロー、現在の例外の処理が終了します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="e1c3d-510">それ以外の場合、コントロールの終了ポイントに達したら、`finally`ブロック、現在の例外の処理が続行します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="e1c3d-511">例外ハンドラーが現在の関数の呼び出しでない場合は、関数の呼び出しが終了し、次のいずれか。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="e1c3d-512">現在の関数が非同期以外の場合は、上記の手順が、関数の呼び出し元の関数メンバーの呼び出し元のステートメントに対応するスロー ポイントに繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="e1c3d-513">現在の関数は、async とタスクを返すには、例外は」の説明に従ってエラーが発生したか取り消された状態には、戻り値のタスクに記録されます。[列挙子インターフェイス](classes.md#enumerator-interfaces)します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="e1c3d-514">現在のスレッドの同期コンテキストを通知する」の説明に従って、現在の関数が非同期と void を返す場合は、[列挙可能なインターフェイス](classes.md#enumerable-interfaces)します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="e1c3d-515">例外の処理には、現在のスレッドですべての関数メンバーの呼び出しが終了する場合、スレッドに、例外のハンドラーがないことを示すし、スレッドが自体が終了します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="e1c3d-516">このような終了の影響は、実装定義です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="e1c3d-517">Try ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-517">The try statement</span></span>

<span data-ttu-id="e1c3d-518">`try`ステートメント ブロックの実行中に発生する例外をキャッチするためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="e1c3d-519">さらに、`try`ステートメントでは、コントロールが離れるときに常に実行されるコードのブロックを指定する機能、`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

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

<span data-ttu-id="e1c3d-520">次の 3 つの可能な形式としては、`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="e1c3d-521">A`try`ブロックでは、1 つまたは複数続く`catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="e1c3d-522">A`try`ブロックが続く、`finally`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="e1c3d-523">A`try`ブロックでは、1 つまたは複数続く`catch`ブロックが続く、`finally`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="e1c3d-524">ときに、`catch`句を指定します、 *exception_specifier*、型でなければなりません`System.Exception`から派生した型`System.Exception`または型パラメーターの型を持つ`System.Exception`(またはそのサブクラス)、効果的なとして基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="e1c3d-525">ときに、`catch`句では、両方を指定します、 *exception_specifier*で、*識別子*、***例外変数***指定した名前と型の宣言します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="e1c3d-526">例外変数は、そのスコープでローカル変数に対応、`catch`句。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="e1c3d-527">実行中に、 *exception_filter*と*ブロック*、例外変数は、現在処理中の例外を表します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="e1c3d-528">確実な代入をチェックのために、例外変数は間違いなく全体のスコープに割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="e1c3d-529">しない限り、`catch`句にはで例外変数名が含まれて、フィルターで例外オブジェクトにアクセスすることはできませんと`catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="e1c3d-530">A`catch`句が指定されていない、 *exception_specifier* 、一般的なと呼びます`catch`句。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="e1c3d-531">一部のプログラミング言語から派生したオブジェクトとして表現ではない例外をサポート可能性があります`System.Exception`、c# コードで、このような例外を生成しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="e1c3d-532">一般的な`catch`このような例外をキャッチする句を使用することがあります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="e1c3d-533">したがって、一般的な`catch`句は、種類を指定する 1 つから意味の異なる`System.Exception`前者可能性がありますも例外をキャッチする他の言語で、します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="e1c3d-534">例外のハンドラーを見つけるために`catch`句は、構文の順序でチェックされます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="e1c3d-535">場合、`catch`句には例外フィルターなしの型を指定します、それ以降のコンパイル時エラーが`catch`で同じ句`try`ステートメントを入力すると、同じかから派生する型を指定します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="e1c3d-536">場合、`catch`句型を持たないとフィルターなしを指定、最後にある必要があります`catch`句を`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="e1c3d-537">内で、`catch`ブロック、`throw`ステートメント ([throw ステートメント](statements.md#the-throw-statement)) 式を指定しないと、によってキャッチされた例外を再スローに使用できる、`catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="e1c3d-538">例外変数への割り当てでは、再スローされる例外は変更されません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="e1c3d-539">例</span><span class="sxs-lookup"><span data-stu-id="e1c3d-539">In the example</span></span>
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
<span data-ttu-id="e1c3d-540">メソッド`F`例外をキャッチ、診断情報をコンソールに出力、例外変数を変更し、例外が再度スローします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="e1c3d-541">再スローされる例外には、元の例外があるため、生成される出力です。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="e1c3d-542">最初の catch ブロックをスローしていた場合`e`現在の例外をスローするには、代わりに生成される出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```csharp
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="e1c3d-543">コンパイル時エラーには、 `break`、 `continue`、または`goto`ステートメントの制御を転送する、`finally`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="e1c3d-544">ときに、 `break`、 `continue`、または`goto`でステートメントが発生した、`finally`ステートメントのターゲット ブロックが同じでなければなりません`finally`ブロック、またはそれ以外の場合、コンパイル時エラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="e1c3d-545">コンパイル時エラーには、`return`ステートメントで発生する、`finally`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="e1c3d-546">A`try`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-547">制御が移ります、`try`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="e1c3d-548">コントロールの終了点に達すると、`try`ブロック。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="e1c3d-549">場合、`try`ステートメントには、`finally`ブロック、`finally`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="e1c3d-550">終点に制御が移ります、`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="e1c3d-551">例外が伝達される場合、`try`ステートメントの実行中に、`try`ブロック。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="e1c3d-552">`catch`句は、存在する場合、適切な例外ハンドラーを検索する外観の順序でチェックされます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="e1c3d-553">場合、`catch`句は、型を指定していないまたは例外の種類または例外の種類の基本型を指定します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="e1c3d-554">場合、`catch`句例外変数を宣言して、例外オブジェクトが例外変数に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="e1c3d-555">場合、`catch`句は、例外フィルターを宣言して、フィルターが評価されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="e1c3d-556">評価されると、 `false`catch 句は、一致ではない場合、いずれかで検索を続行する後続`catch`の適切なハンドラー句。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="e1c3d-557">それ以外の場合、`catch`句は、一致と見なされ、照合に制御が移ります`catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="e1c3d-558">コントロールの終了点に達すると、`catch`ブロック。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="e1c3d-559">場合、`try`ステートメントには、`finally`ブロック、`finally`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="e1c3d-560">終点に制御が移ります、`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="e1c3d-561">例外が伝達される場合、`try`ステートメントの実行中に、`catch`ブロック。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="e1c3d-562">場合、`try`ステートメントには、`finally`ブロック、`finally`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="e1c3d-563">[次へ] の外側に、例外が伝達される`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="e1c3d-564">場合、`try`ステートメントが no`catch`句しない場合または`catch`句に一致する例外。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="e1c3d-565">場合、`try`ステートメントには、`finally`ブロック、`finally`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="e1c3d-566">[次へ] の外側に、例外が伝達される`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="e1c3d-567">ステートメントを`finally`ブロックは、コントロールが離れるときに常に実行を`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="e1c3d-568">コントロールの転送を実行した結果として、通常の実行の結果として発生するかどうかこれは true、 `break`、 `continue`、 `goto`、または`return`ステートメント、または例外の反映の結果として、 `try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="e1c3d-569">実行中に例外がスローされた場合、`finally`ブロックし、キャッチされない例外の反映 [次へ] の外側に同じ finally ブロック内で`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="e1c3d-570">別の例外の反映中だった場合は、その例外は失われます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="e1c3d-571">例外を伝達するプロセスについては、説明の説明にさらに、`throw`ステートメント ([throw ステートメント](statements.md#the-throw-statement))。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="e1c3d-572">`try`のブロックを`try`ステートメントに到達できる場合、`try`ステートメントに到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="e1c3d-573">A`catch`のブロックを`try`ステートメントに到達できる場合、`try`ステートメントに到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="e1c3d-574">`finally`のブロックを`try`ステートメントに到達できる場合、`try`ステートメントに到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="e1c3d-575">終点を`try`ステートメントに到達できるは、次の両方に該当する場合。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="e1c3d-576">終点、`try`ブロックに到達できないか、末尾が少なくとも 1 つのポイント`catch`ブロックに到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="e1c3d-577">場合、`finally`ブロックが存在する場合は、終点、`finally`ブロックに到達します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="e1c3d-578">Checked と unchecked ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-578">The checked and unchecked statements</span></span>

<span data-ttu-id="e1c3d-579">`checked`と`unchecked`ステートメントは制御を使用して、***オーバーフロー チェック コンテキスト***整数型の算術演算と変換します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="e1c3d-580">`checked`ステートメントは、すべての式、*ブロック*checked コンテキストで評価されると、`unchecked`ステートメントは、すべての式、*ブロック*で評価される、unchecked コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="e1c3d-581">`checked`と`unchecked`ステートメントは同等では、`checked`と`unchecked`演算子 ([checked と unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) 式ではなくブロック上で動作する点を除いて、.</span><span class="sxs-lookup"><span data-stu-id="e1c3d-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="e1c3d-582">Lock ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-582">The lock statement</span></span>

<span data-ttu-id="e1c3d-583">`lock`ステートメント、特定のオブジェクトの相互排他ロックが取得、ステートメントを実行、ロックを解放します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="e1c3d-584">式を`lock`ステートメントがある既知の型の値を表す必要があります、 *reference_type*します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="e1c3d-585">暗黙的なボックス化変換なし ([ボックス化変換](conversions.md#boxing-conversions)) の式が実行されるまで、`lock`ステートメント、およびそのため、式の値を表すことのコンパイル時エラー、 *value_type*.</span><span class="sxs-lookup"><span data-stu-id="e1c3d-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="e1c3d-586">A`lock`形式のステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="e1c3d-587">場所`x`の式を指定する*reference_type*とまったく同じです</span><span class="sxs-lookup"><span data-stu-id="e1c3d-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
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
<span data-ttu-id="e1c3d-588">ただし、`x` が評価されるのは 1 回だけです。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="e1c3d-589">相互排他ロックが保持されている間、同じ実行スレッドで実行されるコードできますも入手して、ロックを解放します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="e1c3d-590">ただし、他のスレッドで実行されるコードは、ロックが解放されるまで、ロックの取得からブロックされます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="e1c3d-591">ロック`System.Type`静的データへのアクセスを同期するためにオブジェクトはお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="e1c3d-592">他のコードは、デッドロックが発生することができますが、同じ型にロックがあります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="e1c3d-593">プライベート静的オブジェクトをロックして静的なデータへのアクセスを同期することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="e1c3d-594">例:</span><span class="sxs-lookup"><span data-stu-id="e1c3d-594">For example:</span></span>
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

## <a name="the-using-statement"></a><span data-ttu-id="e1c3d-595">using ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-595">The using statement</span></span>

<span data-ttu-id="e1c3d-596">`using`ステートメントが 1 つまたは複数のリソースを取得し、ステートメントを実行およびリソースを破棄します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="e1c3d-597">A***リソース***クラスまたは構造体を実装する`System.IDisposable`、という名前の 1 つのパラメーターなしのメソッドを含む`Dispose`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="e1c3d-598">リソースを使用してコードを呼び出すことができます`Dispose`をリソースが不要になったことを示します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="e1c3d-599">場合`Dispose`は呼び出されません、ガベージ コレクションの結果として自動的に破棄が最終的が発生します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="e1c3d-600">場合のフォーム*resource_acquisition*は*local_variable_declaration*の型、 *local_variable_declaration*いずれかである必要があります`dynamic`または型暗黙的に変換できる`System.IDisposable`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="e1c3d-601">場合のフォーム*resource_acquisition*は*式*この式は、暗黙的に変換可能である必要がありますし、`System.IDisposable`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="e1c3d-602">宣言されたローカル変数、 *resource_acquisition*は読み取り専用であり、初期化子を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="e1c3d-603">コンパイル時エラーが発生する場合は、埋め込みステートメントが、これらのローカル変数を変更しようとしています。 (割り当てまたは`++`と`--`演算子)、それらのアドレスを取得またはとして渡したり`ref`または`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="e1c3d-604">A`using`ステートメントは 3 つの部分に変換されます。 取得、使用状況、および破棄します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="e1c3d-605">リソースの使用量が暗黙的にで囲まれている、`try`ステートメントを含む、`finally`句。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="e1c3d-606">これは、`finally`句は、リソースを破棄します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="e1c3d-607">場合、`null`のリソースを取得すると、なしの呼び出しを`Dispose`が行われると例外はスローされません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="e1c3d-608">型の場合は、リソース`dynamic`動的の暗黙的な変換を動的に変換されます ([動的の暗黙的な変換](conversions.md#implicit-dynamic-conversions)) に`IDisposable`変換があることを確認するには取得中に使用および破棄する前に成功します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="e1c3d-609">A`using`形式のステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="e1c3d-610">次の 3 つの可能な展開のいずれかに対応します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="e1c3d-611">ときに`ResourceType`null 非許容値型では、拡張が、</span><span class="sxs-lookup"><span data-stu-id="e1c3d-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
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

<span data-ttu-id="e1c3d-612">それ以外の場合、when`ResourceType`が null 許容値型または参照型以外の`dynamic`、拡張</span><span class="sxs-lookup"><span data-stu-id="e1c3d-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="e1c3d-613">それ以外の場合、when`ResourceType`は`dynamic`拡張が、</span><span class="sxs-lookup"><span data-stu-id="e1c3d-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="e1c3d-614">どちらの展開、`resource`変数には、埋め込みのステートメントでは、読み取り専用と`d`変数で、アクセス不可能と非表示は、埋め込みステートメントにします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="e1c3d-615">上記の拡張と一貫した動作であれば、パフォーマンス上の理由などの異なる方法では、特定の using ステートメントを実装するために実装が許可されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="e1c3d-616">A`using`形式のステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="e1c3d-617">同じ 3 つの可能な拡張があります。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-617">has the same three possible expansions.</span></span> <span data-ttu-id="e1c3d-618">ここで`ResourceType`のコンパイル時の型を暗黙的には、`expression`があるいずれかの場合。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="e1c3d-619">それ以外の場合、インターフェイス`IDisposable`自体として提供される、`ResourceType`します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="e1c3d-620">`resource`変数で、アクセス不可能と非表示は、埋め込みステートメントにします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="e1c3d-621">ときに、 *resource_acquisition*の形式を*local_variable_declaration*、指定された型の複数のリソースを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="e1c3d-622">A`using`形式のステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="e1c3d-623">シーケンスに正確に同等の入れ子になって`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="e1c3d-624">次の例は、という名前のファイルを作成します。`log.txt`し、ファイルに 2 行のテキストを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="e1c3d-625">読み取り用にその同じファイルを開きし、コンソールに含まれている行のテキストをコピーします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
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

<span data-ttu-id="e1c3d-626">以降、`TextWriter`と`TextReader`クラスで実装、`IDisposable`例を使用できるインターフェイス、`using`ステートメントを基になるファイルを次の書き込み正しく閉じることを確認または読み取り操作。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="e1c3d-627">Yield ステートメント</span><span class="sxs-lookup"><span data-stu-id="e1c3d-627">The yield statement</span></span>

<span data-ttu-id="e1c3d-628">`yield`反復子ブロックでは、ステートメントを使用してください ([ブロック](statements.md#blocks))、列挙子オブジェクトに値を生成する ([列挙子オブジェクト](classes.md#enumerator-objects)) または列挙可能なオブジェクト ([の列挙可能なオブジェクト。](classes.md#enumerable-objects))反復子のまたはイテレーションの終了を通知します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="e1c3d-629">`yield` 予約語です。直前に使用する場合にのみ、特別な意味を`return`または`break`キーワード。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="e1c3d-630">他のコンテキストで`yield`識別子として使用できます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="e1c3d-631">いくつかの制限がある場所について、`yield`次」の説明に従って、ステートメントを表示できます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="e1c3d-632">コンパイル時エラーには、 `yield` (いずれかの形式) のステートメントの外側に表示する、 *method_body*、 *operator_body*または*accessor_body*</span><span class="sxs-lookup"><span data-stu-id="e1c3d-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="e1c3d-633">コンパイル時エラーには、 `yield` (のいずれかの形式) ステートメント、匿名関数内に表示します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="e1c3d-634">コンパイル時エラーには、 `yield` (いずれかの形式) のステートメントに表示される、`finally`の句、`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="e1c3d-635">コンパイル時エラーには、`yield return`どこにでも表示するステートメントを`try`ステートメントを含む`catch`句。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="e1c3d-636">次の例のいくつかの有効および無効な使用`yield`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

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

<span data-ttu-id="e1c3d-637">暗黙的な変換 ([暗黙的な変換](conversions.md#implicit-conversions)) 内の式の型から存在する必要があります、 `yield return` yield 型ステートメント ([型を生成](classes.md#yield-type)) 反復子の。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="e1c3d-638">A`yield return`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-639">ステートメントで指定された式が評価、暗黙的に、yield 型に変換されに割り当てられている、`Current`列挙子オブジェクトのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="e1c3d-640">反復子ブロックの実行が中断されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="e1c3d-641">場合、`yield return`内で 1 つまたは複数のステートメントが`try`ブロック、関連付けられている`finally`ブロックは、この時点では実行されません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="e1c3d-642">`MoveNext`列挙子オブジェクトのメソッドを返します`true`列挙子オブジェクトは、次の項目に正常に進んだことを示す、呼び出し元にします。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="e1c3d-643">次の列挙子オブジェクトの呼び出し`MoveNext`メソッドは、前回中断された場所からの反復子ブロックの実行を再開します。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="e1c3d-644">A`yield break`ステートメントが次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="e1c3d-645">場合、`yield break`ステートメントが 1 つまたは複数で囲まれた`try`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="e1c3d-646">コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="e1c3d-647">までこのプロセスが繰り返されます、`finally`外側のすべてのブロック`try`ステートメントが実行されています。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="e1c3d-648">コントロールは、反復子ブロックの呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="e1c3d-649">いずれかになります、`MoveNext`メソッドまたは`Dispose`列挙子オブジェクトのメソッド。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="e1c3d-650">`yield break`ステートメント無条件に制御を転送他の場所での終了点を`yield break`ステートメントが到達可能ではありません。</span><span class="sxs-lookup"><span data-stu-id="e1c3d-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
