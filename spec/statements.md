---
ms.openlocfilehash: 8f9551b9e7f70379836c23a60f0d37dc02f8e18e
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229857"
---
# <a name="statements"></a>ステートメント

C# には、さまざまなステートメントが用意されています。 これらのステートメントのほとんどは、C および C++ でプログラミングする開発者にとって馴染み深いになります。

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

*Embedded_statement*非終端要素は他のステートメント内に表示されるステートメントを使用します。 使用*embedded_statement*なく*ステートメント*宣言ステートメントとラベル付きステートメントでこれらのコンテキストの使用は含まれません。 例では、
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
ため、コンパイル時エラーの結果、`if`ステートメントが必要です、 *embedded_statement*なく*ステートメント*場合にそのブランチ。 このコードが許可された場合、変数`i`は宣言されますが、使用することはありませんでした。 ただし、配置している`i`の例が有効では、ブロックで宣言します。

## <a name="end-points-and-reachability"></a>エンドポイントと到達可能性

すべてのステートメントが、***終点***します。 直感的な用語では、ステートメントの終了点は、ステートメントの直後に場所です。 複合ステートメント (埋め込みステートメントを含んでいるステートメント) の実行のルールは、コントロールが埋め込みステートメントの終点に達したときに行われるアクションを指定します。 たとえば、コントロールには、ブロック内のステートメントの終了点に達すると、ブロックに次のステートメントに制御が移ります。

ステートメントを実行してに可能性があることができます、ステートメントがあるとは***到達***します。 逆に、ステートメントが実行される可能性がない場合、ステートメントと呼ばれます***到達できない***します。

例
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
2 番目の呼び出しの`Console.WriteLine`ステートメントが実行される可能性がないために到達できません。

ステートメントに到達できることをコンパイラが判断した場合、警告が報告されます。 具体的にはエラーではなく到達できないステートメントにすることをお勧めします。

特定のステートメントまたはエンドポイントが到達可能かどうかを確認するのには、コンパイラは、各ステートメントに対して定義されている到達可能性の規則に従ってフロー分析を行います。 フロー解析では、定数式の値を考慮に入れます ([定数式](expressions.md#constant-expressions)) ステートメントの動作を制御するが、非定数式で使用できる値は考慮されません。 つまり、制御フロー分析のために、指定された型の非定数式がその型のすべての値を持つと見なされます。

例
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
ブール式、`if`ために、ステートメントが定数式のオペランドは両方とも、`==`演算子は定数。 定数式は、コンパイル時に評価されるとは、値を生成`false`、`Console.WriteLine`呼び出しは到達不能と見なされます。 ただし場合、`i`ローカル変数にするのには
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine`実際が実行されない場合でも呼び出しが到達可能が考慮されます。

*ブロック*関数のメンバーは常と見なされます到達します。 ブロック内の各ステートメントの到達可能性の規則を連続して評価するには、任意のステートメントの到達可能性を判断できます。

例
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
2 番目の到達可能性`Console.WriteLine`は次のように決定されます。

*  最初の`Console.WriteLine`式ステートメントに到達できるためのブロック、`F`メソッドに到達します。
*  1 つ目の終了点`Console.WriteLine`そのステートメントに到達するために、式のステートメントに到達します。
*  `if`ステートメントは、最初の最後のポイントため到達`Console.WriteLine`式ステートメントに到達します。
*  2 番目の`Console.WriteLine`式ステートメントに到達できるためのブール式、`if`ステートメントには、定数の値はありません。`false`します。

2 つの状況に到達可能で、ステートメントの終了点のコンパイル時エラーがあります。

*  `switch`ステートメントでは、次の switch セクションに「フォール スルー」する switch セクションが許可されていませんはの switch セクションに到達可能で、ステートメント リストの終点のコンパイル時エラーです。 示す値では通常このエラーが発生した場合、`break`ステートメントがありません。
*  最後の位置に到達可能で値を計算する関数メンバーのブロックのコンパイル時エラーになります。 示す値を通常は、このエラーが発生した場合、`return`ステートメントがありません。

## <a name="blocks"></a>ブロック

"*ブロック*" を使用すると、1 つのステートメントしか使用できないコンテキストで複数のステートメントを記述できます。

```antlr
block
    : '{' statement_list? '}'
    ;
```

A*ブロック*省略可能なから成る*statement_list* ([ステートメントが一覧表示](statements.md#statement-lists)) 中かっこで囲まれている。 ステートメントの一覧を省略した場合、ブロックは空にすると言います。

ブロックは、宣言ステートメントを含めることができます ([宣言ステートメント](statements.md#declaration-statements))。 ローカル変数または定数のスコープ、ブロックで宣言されているブロックです。

ブロックは、次のように実行されます。

*  ブロックが空の場合、ブロックの終了ポイントに制御が移ります。
*  ブロックが空でない場合は、ステートメントの一覧に制御が移ります。 コントロールが、ステートメント リストの最後のポイントに達すると、コントロールは、ブロックの終了ポイントに転送されます。

ブロックのステートメントの一覧は、到達可能なブロック自体が到達可能な場合です。

ブロックが空の場合、またはステートメント リストのエンドポイントが到達可能な場合、ブロックの終了点が到達可能です。

A*ブロック*1 つまたは複数を格納している`yield`ステートメント ([yield ステートメント](statements.md#the-yield-statement))、反復子ブロックと呼ばれます。 反復子ブロックは、反復子としての関数メンバーを実装するために使用 ([反復子](classes.md#iterators))。 反復子ブロックにいくつか追加の制限が適用されます。

*  コンパイル時エラーには、`return`反復子ブロックに表示されるステートメント (が`yield return`ステートメントは許可されます)。
*  Unsafe コンテキストを格納する反復子ブロックのコンパイル時エラーです ([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts))。 反復子ブロックは、その宣言が、unsafe コンテキストで入れ子になった場合でも常に、安全なコンテキストを定義します。

### <a name="statement-lists"></a>ステートメントの一覧

A***ステートメント リスト***シーケンスで記述された 1 つまたは複数のステートメントで構成されます。 発生するステートメント リスト*ブロック*s ([ブロック](statements.md#blocks)) し、 *switch_block*s ([switch ステートメント](statements.md#the-switch-statement))。

```antlr
statement_list
    : statement+
    ;
```

ステートメントの一覧は、最初のステートメントに制御を転送することによって実行されます。 コントロールが、ステートメントの終了点に達すると、制御は次のステートメントに移ります。 コントロールが最後のステートメントの終了点に達すると、コントロールは、ステートメント リストのエンドポイントに転送されます。

ステートメントの一覧内のステートメントは、到達できるは、次の少なくとも 1 つが true の場合です。

*  ステートメントが最初のステートメントと、ステートメント リスト自体に到達します。
*  前のステートメントの終了点に到達します。
*  ステートメントはラベル付きステートメントであり、到達可能で、ラベルが参照されている`goto`ステートメント。

一覧の最後のステートメントの終了点に到達できる場合、ステートメント リストの終点に到達します。

## <a name="the-empty-statement"></a>空のステートメント

*Empty_statement*何も行われません。

```antlr
empty_statement
    : ';'
    ;
```

空のステートメントは、操作がないコンテキストで実行するステートメントが必要になるときに使用されます。

空のステートメントの実行は、単に、ステートメントの終了点にコントロールを転送します。 したがって、空のステートメントの終了点は、到達可能な空のステートメントに到達できる場合は。

書き込み時に空のステートメントを使用できます、`while`本体のステートメント。
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

空のステートメントを使用して、終了直前にラベルを宣言することも、"`}`"ブロックの。
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>ラベル付きステートメント

A *labeled_statement*ラベルによってプレフィックス指定するステートメントを許可します。 ラベル付きステートメントはブロックでは、許可されますが、埋め込みステートメントとしては使用できません。

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

ラベル付きステートメントで指定された名前のラベルを宣言して、*識別子*します。 ラベルのスコープは、ブロック全体を入れ子になったブロックを含め、ラベルが宣言されています。 重複するスコープが同じ名前の 2 つのラベルのコンパイル時エラーになります。

ラベルはから参照できる`goto`ステートメント ([goto ステートメント](statements.md#the-goto-statement)) ラベルのスコープ内で。 つまり、`goto`ステートメント ブロック内で、ブロック、もののブロックにないコントロールを転送できます。

ラベルは、独自の宣言領域があるし、他の識別子に干渉することはできません。 例では、
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
有効であり、名前を使用して`x`パラメーターとラベルの両方として。

ラベル付きステートメントの実行は、ラベルを次のステートメントの実行に正確に対応します。

ラベル付きステートメントに到達できるは、ラベルが、到達可能で参照されている場合にだけでなく通常の制御フローによって提供される到達可能性、`goto`ステートメント。 (例外。場合、`goto`内のステートメントは、`try`を含む、`finally`ブロック、およびラベル付きステートメントが範囲外です、`try`との終点、`finally`ブロックにアクセスし、ラベル付きステートメントにから到達できません`goto`ステートメントです)。

## <a name="declaration-statements"></a>宣言ステートメント

A *declaration_statement*ローカル変数または定数を宣言します。 宣言ステートメントはブロックでは、許可されますが、埋め込みステートメントとしては使用できません。

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>ローカル変数の宣言

A *local_variable_declaration* 1 つまたは複数のローカル変数を宣言します。

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

*Local_variable_type*の*local_variable_declaration*か、直接、宣言によって導入される変数の型を指定しますまたは、識別子を持つことを示します`var`です初期化子に基づいて型を推論する必要があります。 型がのリストが続く*local_variable_declarator*s、新しい変数をそれぞれが導入されています。 A *local_variable_declarator*から成る、*識別子*必要に応じて、変数の名前を示す、"`=`"トークンと*local_variable_initializer*変数の初期値を提供します。

コンテキスト キーワードとして、ローカル変数宣言のコンテキストで識別子 var が機能します ([キーワード](lexical-structure.md#keywords))。ときに、 *local_variable_type*として指定されて`var`とという名前のない型`var`は宣言は、スコープ内、***ローカル変数宣言を暗黙的に型指定された***型があります。関連付け初期化子式の型から推論されます。 暗黙的に型指定されたローカル変数の宣言は、次の制限が適用されます。

*  *Local_variable_declaration*複数を含めることはできません*local_variable_declarator*秒。
*  *Local_variable_declarator*含める必要があります、 *local_variable_initializer*します。
*  *Local_variable_initializer*必要があります、*式*します。
*  初期化子*式*コンパイル時の型があります。
*  初期化子*式*自体、宣言された変数は参照できません

正しくない暗黙的に型指定されたローカル変数宣言の例を次に示します。

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

ローカル変数の値が使用する式で取得した、 *simple_name* ([簡易名](expressions.md#simple-names)) を使用してローカル変数の値を変更し、*割り当て*([代入演算子](expressions.md#assignment-operators))。 ローカル変数を明示的に代入する必要があります ([確実な代入](variables.md#definite-assignment)) 各場所、その値を取得します。

宣言されたローカル変数のスコープを*local_variable_declaration*ブロックで宣言が発生します。 前にあるテキストの位置にローカル変数を参照するとエラーには、 *local_variable_declarator*のローカル変数。 ローカル変数のスコープ内では、別のローカル変数や定数を同じ名前で宣言すると、コンパイル時エラーを勧めします。

複数の変数を宣言して、ローカル変数の宣言は、同じ型を持つ 1 つの変数の複数の宣言と同じです。 さらに、ローカル変数宣言で変数の初期化子は、宣言の直後に挿入されている代入ステートメントに正確に対応します。

例では、
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
対応します。
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

暗黙的に型指定されたローカル変数宣言で宣言されているローカル変数の型を表示して、変数を初期化するために使用する式の型と同じであります。 例:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

上、暗黙的に型指定されたローカル変数宣言は、明示的に型指定された次の宣言にまったく同じです。
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>ローカル定数宣言

A *local_constant_declaration* 1 つまたは複数のローカル定数を宣言します。

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

*型*の*local_constant_declaration*宣言によって導入される定数の型を指定します。 型がのリストが続く*constant_declarator*s、新しい定数をそれぞれが導入されています。 A *constant_declarator*から成る、*識別子*でその名前、定数の後に、"`=`"トークン、その後に、 *constant_expression* ([定数式](expressions.md#constant-expressions))、定数の値を提供します。

*型*と*constant_expression*ローカル定数宣言の定数メンバー宣言のものと同じ規則に従う必要があります ([定数](classes.md#constants))。

ローカル定数の値が使用する式で取得した、 *simple_name* ([簡易名](expressions.md#simple-names))。

ローカル定数のスコープは、ブロックの宣言が発生します。 前にあるテキストの位置でのローカル定数を参照するとエラーにはその*constant_declarator*します。 ローカル定数のスコープ内では、別のローカル変数や定数を同じ名前で宣言すると、コンパイル時エラーを勧めします。

複数の定数を宣言するためのローカル定数宣言では、単一の定数のと同じ型の複数の宣言に相当します。

## <a name="expression-statements"></a>式ステートメント

*Expression_statement*指定された式を評価します。 値は、式で計算、存在する場合は破棄されます。

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

ステートメントとしては、すべての式を使用します。 など、特定の式で`x + y`と`x == 1`ことだけです (これは破棄されます) 値を計算する、ステートメントとしては使用できません。

実行、 *expression_statement*内の式を評価しの終点に制御を転送、 *expression_statement*します。 終点を*expression_statement*が到達可能な場合は、その*expression_statement*に到達します。

## <a name="selection-statements"></a>選択ステートメント

選択ステートメントでは、いくつかの式の値に基づいて、実行可能ステートメントの数値の 1 つを選択します。

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If ステートメント

`if`ステートメントは、ブール式の値に基づいて実行するステートメントを選択します。

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

`else`パーツは構文的に最も近い先行関連付け`if`構文で許可されています。 つまり、`if`形式のステートメント
```csharp
if (x) if (y) F(); else G();
```
上記の式は、次の式と同じです。
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

`if`ステートメントが次のように実行されます。

*  *Boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。
*  ブール式の結果が場合`true`、埋め込みの最初のステートメントに制御が移ります。 コントロールがの終了ポイントに転送されるコントロールがそのステートメントの終了点に達すると、`if`ステートメント。
*  ブール式の結果が場合`false`場合に、`else`部分が存在する、2 番目の埋め込みステートメントに制御が移ります。 コントロールがの終了ポイントに転送されるコントロールがそのステートメントの終了点に達すると、`if`ステートメント。
*  ブール式の結果が場合`false`場合に、`else`部分が存在しないの終点に制御が移ります、`if`ステートメント。

最初のステートメントの埋め込み、`if`ステートメントに到達できる場合、`if`ステートメントに到達でき、ブール型の式が定数の値を持たない`false`します。

2 番目のステートメントの埋め込み、`if`ステートメントで存在する場合の指定が到達可能な場合、`if`ステートメントに到達でき、ブール型の式が定数の値を持たない`true`します。

終点を`if`ステートメントが到達可能な場合、その埋め込みステートメントの少なくとも 1 つの終点に到達します。 末尾がさらに、ポイント、`if`ステートメントなしで`else`部品に到達できる場合、`if`ステートメントに到達でき、ブール型の式が定数の値を持たない`true`。

### <a name="the-switch-statement"></a>Switch ステートメント

Switch ステートメントは、実行の switch 式の値に対応するスイッチが関連付けられているラベルを持つステートメントの一覧を選択します。

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

A *switch_statement*キーワードから成る`switch`の後にかっこで囲まれた式が (switch 式と呼ばれます)、その後、 *switch_block*します。 *Switch_block* 0 個以上から成る*switch_section*s、中かっこで囲みます。 各*switch_section* 1 つまたは複数から成る*switch_label*秒後に、 *statement_list* ([ステートメントが一覧表示](statements.md#statement-lists))。

***の種類を制御する***の`switch`ステートメントは、switch 式で確立されます。

*  Switch 式の型が場合`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `bool`、 `char`、 `string`、または、 *enum_type*のかどうかは、これらの型のいずれかに対応する null 許容型では、管理方法はまたはの入力、`switch`ステートメント。
*  それ以外の場合、1 つだけユーザー定義の暗黙の変換 ([ユーザー定義の変換](conversions.md#user-defined-conversions)) switch 式の型から型を制御する次の候補のいずれかに存在する必要があります: `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `string`、またはそれらの型のいずれかに対応する null 許容型。
*  それ以外の場合、このような暗黙的な変換が存在しないか、このような 1 つの暗黙的な変換が存在する数より多い場合、コンパイル時エラーが発生します。

それぞれの定数式`case`ラベルは、暗黙的に変換する値を表す必要があります ([暗黙的な変換](conversions.md#implicit-conversions)) の管理の型を`switch`ステートメント。 2 つ以上の場合、コンパイル時エラーが発生した`case`同じラベル`switch`ステートメントで同じ定数値を指定します。

最大で 1 つできます`default`switch ステートメントでラベル。

A`switch`ステートメントが次のように実行されます。

*  Switch 式が評価され、管理の型に変換します。
*  定数のいずれかが指定されている場合、`case`同じラベル`switch`ステートメントが switch 式の値と等しく、一致する、次のステートメントの一覧に制御が移ります`case`ラベル。
*  場合に指定された定数のいずれも`case`同じラベル`switch`ステートメントは、switch 式の値と等しい場合に、`default`ラベルが存在する、ステートメント リストの次に制御が移ります、 `default`ラベル。
*  場合に指定された定数のいずれも`case`同じラベル`switch`ステートメントは、switch 式の値と等しいされていない場合`default`ラベルが存在するの終点に制御が移ります、`switch`ステートメント。

Switch セクションのステートメント リストのエンドポイントが到達可能な場合は、コンパイル時エラーが発生します。 これは、「フォール スルー」ルールと呼ばれます。 例では、
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
switch セクションに到達可能なエンドポイントがあるないため、このプロパティの値は有効です。 C や C++ とは異なり、switch セクションの実行は許可されていません「フォール スルー」に次の switch セクションを例では、
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
コンパイル時エラーが発生します。 Switch セクションの実行が明示的なもう 1 つの switch セクションの実行後に、`goto case`または`goto default`ステートメントを使用する必要があります。
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

複数のラベルが許可されている、 *switch_section*します。 例では、
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
有効です。 この例ではためにを「フォール スルー」ルールに違反しないラベル`case 2:`と`default:`同じの一部である*switch_section*します。

「フォール スルー」ルールが C および C++ で発生するバグの一般的なクラスと`break`ステートメントが誤ってを省略するとします。 さらに、このルールでは、switch セクションにより、`switch`ステートメントにステートメントの動作に影響を与えずに任意に置くことです。 セクションなど、`switch`上記のステートメントは、ステートメントの動作に影響を与えずに元に戻すことができます。
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

Switch セクションのステートメントの一覧は、通常で終わる、 `break`、 `goto case`、または`goto default`ステートメントが到達できないステートメント リストの最後のポイントを表示する構成要素を許可します。 たとえば、`while`ブール式で制御ステートメント`true`エンドポイントに正常には到達しません。 同様に、`throw`または`return`ステートメントが常に別の場所に制御を転送し、その終点に到達しません。 したがって、次の例では有効です。
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

管理の型を`switch`ステートメントは、型、`string`します。 例:
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

などの文字列の等値演算子 ([文字列等値演算子](expressions.md#string-equality-operators))、`switch`ステートメントが大文字小文字を区別し、switch 式の文字列が完全に一致する場合にのみに指定されたスイッチ セクションを実行する`case`ラベル定数。

管理方法が入力すると、`switch`ステートメントが`string`、値`null`case ラベル定数として許可されます。

*Statement_list*の s を*switch_block*宣言ステートメントを含めることができます ([宣言ステートメント](statements.md#declaration-statements))。 ローカル変数または定数のスコープの switch ブロックで宣言されている、スイッチ ブロックです。

Switch セクションのステートメントの一覧に到達できる場合、`switch`ステートメントに到達し、次の少なくとも 1 つが true:

*  Switch 式は、非定数値です。
*  Switch 式が定数の値と一致する、 `case` switch セクション内のラベル。
*  Switch 式は定数値と一致しない`case`ラベル、および switch セクションが含まれています、`default`ラベル。
*  Switch セクションの switch ラベルは、到達によって参照される`goto case`または`goto default`ステートメント。

終点を`switch`ステートメントに到達できるは、次の少なくとも 1 つが true の場合。

*  `switch`ステートメントが含まれていますが、到達可能な`break`を終了させるステートメント、`switch`ステートメント。
*  `switch`ステートメントに到達、switch 式は非定数値、および no`default`ラベルが存在します。
*  `switch`ステートメントに到達、switch 式は定数値と一致しない`case`ラベル、および no`default`ラベルが存在します。

## <a name="iteration-statements"></a>繰り返しステートメント

繰り返しステートメントでは、埋め込みステートメントを繰り返し実行します。

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>While ステートメント

`while`ステートメントは条件付きで 0 回以上の埋め込みステートメントを実行します。

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

A`while`ステートメントが次のように実行されます。

*  *Boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。
*  ブール式の結果が場合`true`、埋め込みステートメントに制御が移ります。 コントロールが埋め込みステートメントの終了点に達すると (通常の実行から、`continue`ステートメント) の先頭に制御が移ります、`while`ステートメント。
*  ブール式の結果が場合`false`の終点に制御が移ります、`while`ステートメント。

内の埋め込みステートメントを`while`ステートメントを`break`ステートメント ([break ステートメント](statements.md#the-break-statement)) の終点に制御を転送できる、 `while` (埋め込みの繰り返しを終了するステートメントステートメントの場合)、および`continue`ステートメント ([continue ステートメント](statements.md#the-continue-statement)) に埋め込みステートメントの終了ポイントに制御を移すことができます (の別のイテレーションを実行するため、`while`ステートメント)。

埋め込みステートメントを`while`ステートメントに到達できる場合、`while`ステートメントに到達でき、ブール型の式が定数の値を持たない`false`します。

終点を`while`ステートメントに到達できるは、次の少なくとも 1 つが true の場合。

*  `while`ステートメントが含まれていますが、到達可能な`break`を終了させるステートメント、`while`ステートメント。
*  `while`ステートメントに到達でき、ブール型の式が定数の値を持たない`true`します。

### <a name="the-do-statement"></a>Do ステートメント

`do`ステートメントは条件付きで 1 回以上の埋め込みステートメントを実行します。

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

A`do`ステートメントが次のように実行されます。

*  埋め込みステートメントには、制御が移ります。
*  コントロールが埋め込みステートメントの終了点に達すると (通常の実行から、`continue`ステートメント)、 *boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。 ブール式の結果が場合`true`の先頭に制御が移ります、`do`ステートメント。 終点に制御が移ります、それ以外の場合、`do`ステートメント。

内の埋め込みステートメントを`do`ステートメントを`break`ステートメント ([break ステートメント](statements.md#the-break-statement)) の終点に制御を転送できる、 `do` (埋め込みの繰り返しを終了するステートメントステートメントの場合)、および`continue`ステートメント ([continue ステートメント](statements.md#the-continue-statement)) に埋め込みステートメントの終了ポイントに制御を移すことができます。

埋め込みステートメントを`do`ステートメントに到達できる場合、`do`ステートメントに到達します。

終点を`do`ステートメントに到達できるは、次の少なくとも 1 つが true の場合。

*  `do`ステートメントが含まれていますが、到達可能な`break`を終了させるステートメント、`do`ステートメント。
*  埋め込みステートメントの終了ポイントに到達でき、ブール型の式が定数の値を持たない`true`します。

### <a name="the-for-statement"></a>For ステートメント

`for`ステートメント一連の初期化式を評価し、条件が true の場合、繰り返し埋め込みステートメントを実行し、一連の反復式を評価します。

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

*For_initializer*存在する場合は、いずれかで構成されています、 *local_variable_declaration* ([ローカル変数宣言](statements.md#local-variable-declarations)) の一覧または*statement_式*s ([式ステートメント](statements.md#expression-statements)) コンマで区切られました。 宣言されたローカル変数のスコープを*for_initializer*から始まり、 *local_variable_declarator*変数の埋め込みステートメントの最後までを対象とします。 スコープに含まれる、 *for_condition*と*for_iterator*します。

*For_condition*がある場合があります、 *boolean_expression* ([ブール式](expressions.md#boolean-expressions))。

*For_iterator*存在する場合は、一覧から成る*statement_expression*s ([式ステートメント](statements.md#expression-statements)) コンマで区切られました。

次のように FOR ステートメントを実行します。

*  場合、 *for_initializer*が存在する場合は、変数の初期化子またはステートメントの式が記述されている順序で実行されます。 この手順は一度だけ実行します。
*  場合、 *for_condition*が存在する場合は、評価されます。
*  場合、 *for_condition*が存在するか、評価が得られます`true`、埋め込みステートメントに制御が移ります。 コントロールが埋め込みステートメントの終了点に達すると (通常の実行から、`continue`ステートメント) の式、 *for_iterator*シーケンスで、いずれかが評価され、その他のイテレーションは場合は、以降の評価では、実行、 *for_condition*前の手順でします。
*  場合、 *for_condition*が存在し、評価が得られます`false`の終点に制御が移ります、`for`ステートメント。

内の埋め込みステートメントを`for`ステートメントを`break`ステートメント ([break ステートメント](statements.md#the-break-statement)) の終点に制御を転送できる、 `for` (埋め込みの繰り返しを終了するステートメントステートメントの場合)、および`continue`ステートメント ([continue ステートメント](statements.md#the-continue-statement)) に埋め込みステートメントの終了ポイントに制御を移すことができます (実行するため、 *for_iterator*と別のイテレーションを実行する、`for`ステートメントでは、以降では、 *for_condition*)。

埋め込みステートメントを`for`ステートメントに到達できるは、次のいずれかが true の場合。

*  `for`ステートメントに到達しないと*for_condition*が存在します。
*  `for`ステートメントに到達し、 *for_condition*が存在し、定数の値を持たない`false`します。

終点を`for`ステートメントに到達できるは、次の少なくとも 1 つが true の場合。

*  `for`ステートメントが含まれていますが、到達可能な`break`を終了させるステートメント、`for`ステートメント。
*  `for`ステートメントに到達し、 *for_condition*が存在し、定数の値を持たない`true`します。

### <a name="the-foreach-statement"></a>Foreach ステートメント

`foreach`ステートメントは、コレクションの各要素に対して埋め込みステートメントを実行し、コレクションの要素を列挙します。

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

*型*と*識別子*の`foreach`ステートメントの宣言、***反復変数***ステートメントの。 場合、`var`として識別子が指定されて、 *local_variable_type*、という名前のない型と`var`がスコープ内の反復変数と呼ばれます、***暗黙的に型指定された繰り返し変数***、その型がの要素の型にして、`foreach`ステートメントは、以下のようです。 反復変数は、埋め込みステートメントを拡張するスコープを持つ読み取り専用のローカル変数に対応します。 実行中に、`foreach`ステートメントでは、繰り返し変数は、イテレーションが現在実行中のコレクションの要素を表します。 埋め込みステートメントを繰り返し変数を変更しようとすると、コンパイル時エラーが発生します (割り当てまたは`++`と`--`演算子) として繰り返し変数を渡すことも、`ref`または`out`パラメーター。

次に、簡潔にするため、 `IEnumerable`、 `IEnumerator`、`IEnumerable<T>`と`IEnumerator<T>`名前空間に対応する型を参照してください`System.Collections`と`System.Collections.Generic`します。

Foreach ステートメントのコンパイル時の処理が最初に決定します、***コレクション型***、***列挙子の型***と***要素型***の式。 この決定は、次のように処理されます。

*  場合、型`X`の*式*が配列型からの暗黙的な参照変換は`X`を`IEnumerable`インターフェイス (ため`System.Array`このインターフェイスを実装)。 ***コレクション型***は、`IEnumerable`インターフェイスを***列挙子の型***は、`IEnumerator`インターフェイスと***要素型***の要素の型が、配列型`X`します。
*  場合、型`X`の*式*は`dynamic`からの暗黙的な変換は*式*を`IEnumerable`インターフェイス ([暗黙的な動的変換](conversions.md#implicit-dynamic-conversions))。 ***コレクション型***は、`IEnumerable`インターフェイスと***列挙子の型***は、`IEnumerator`インターフェイス。 場合、`var`として識別子が指定されて、 *local_variable_type* 、***要素型***は`dynamic`、それ以外の場合は`object`します。
*  それ以外の場合、確認するかどうか、型`X`が適切な`GetEnumerator`メソッド。
   * 型のメンバーの参照を実行`X`識別子を持つ`GetEnumerator`と型引数はありません。 メンバーの検索に一致を生成しない、または、あいまいさを生成します。 または、メソッド グループではない一致は、以下に示すは列挙可能なインターフェイスを確認します。 メンバー検索は何も、メソッド グループ、または一致するものを除くが生成される場合に、警告を発行することをお勧めします。
   * メソッドの結果として得られるグループと、空の引数リストを使用するオーバー ロードの解決を実行します。 適切なメソッドのオーバー ロードの解決結果、か、あいまいな結果しますが、メソッドは、以下に示すように列挙可能なインターフェイスの静的またはパブリックではありませんのいずれかのチェックを 1 つの最適な方法で結果します。 オーバー ロードの解決は何も明確なパブリック インスタンス メソッドまたは適切なメソッドを除くが生成される場合に、警告を発行することをお勧めします。
   * 戻り値の型の場合`E`の`GetEnumerator`メソッドがクラスではない、構造体またはインターフェイス型、エラーが生成および以降の手順は実行されません。
   * メンバー参照がで実行される`E`識別子を持つ`Current`と型引数はありません。 メンバー検索の一致結果は、結果は、エラーまたは結果が読み取りを許可されているパブリック インスタンス プロパティ以外のすべて、エラーが生成され、以降の手順は実行されません。
   * メンバー参照がで実行される`E`識別子を持つ`MoveNext`と型引数はありません。 メンバー検索の一致結果は、結果は、エラーに対して、または、結果は、メソッド グループ以外のすべての場合は、エラーが発生し、以降の手順は行われません。
   * オーバー ロードの解決は、空の引数リストを持つメソッド グループで実行されます。 いない適用可能なメソッド、あいまいな結果または 1 つの最適な方法が、そのメソッドのオーバー ロード解決の結果は静的であるか、パブリックではありません、またはその戻り値の型でない場合`bool`あり、エラーが生成されるため、以降の手順は実行されません。
   * ***コレクション型***は`X`、***列挙子の型***は`E`、および***要素型***の種類です、`Current`プロパティ。

*  それ以外の場合、列挙可能なインターフェイスを確認します。
   * すべての種類のデータの場合`Ti`が暗黙的に変換`X`に`IEnumerable<Ti>`、一意の型がある`T`ように`T`ない`dynamic`と他のすべての`Ti`が、暗黙的な変換`IEnumerable<T>`に`IEnumerable<Ti>`、***コレクション型***インターフェイス`IEnumerable<T>`、***列挙子の型***インターフェイス`IEnumerator<T>`、および***要素型***は`T`します。
   * それ以外の場合、このような 1 つ以上の型がある場合`T`エラーが発生し、以降の手順は実行されません。
   * それ以外の場合からの暗黙的な変換がある場合`X`を`System.Collections.IEnumerable`、インターフェイス、***コレクション型***はこのインターフェイスは、***列挙子の型***インターフェイスは、`System.Collections.IEnumerator`、および***要素型***は`object`します。
   * それ以外の場合、エラーが発生し、以降の手順は実行されません。

上記の手順では、成功した場合、明確に、コレクションの型を生成`C`、列挙子の型`E`および要素の型`T`します。 フォームの foreach ステートメント
```csharp
foreach (V v in x) embedded_statement
```
を拡張されます。
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

変数`e`に表示されるか、式にアクセス`x`埋め込みステートメントまたはプログラムの他のソース コード。 変数`v`は埋め込みステートメントでは読み取り専用です。 明示的な変換がない場合 ([明示的な変換](conversions.md#explicit-conversions)) から`T`(要素の型) に`V`(、 *local_variable_type* foreach ステートメント)、エラーが生成されますおよびそれ以上の手順は実行されません。 場合`x`、値を持つ`null`、`System.NullReferenceException`が実行時にスローされます。

上記の拡張と一貫した動作であれば、パフォーマンス上の理由などの異なる方法では、特定の foreach ステートメントを実装するために実装が許可されます。

配置`v`while 内でループがで発生しているすべての匿名関数がキャプチャ時に重要ですが、 *embedded_statement*します。

例:
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
場合`v`while の外部で宣言されたループでは、すべてのイテレーションと後に、その値の間で共有は、ループは、最終的な値になります`13`はどのような呼び出し`f`印刷します。 代わりに、各反復処理に独自の変数があるため`v`、によってキャプチャされた、1 つ`f`最初の値を保持するイテレーションは引き続き`7`、印刷される内容であります。 (注: 以前のバージョンの C# 宣言`v`while の外側のループします)。

本体、ブロックが最後に、次の手順に従って構築されます。

*  暗黙的な変換がある場合`E`を`System.IDisposable`し、インターフェイス
   *  場合`E`null 非許容値型は、finally 句は、相当する意味的に拡張されます。

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  それ以外の場合、finally 句は、相当する意味的に拡張されます。

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   場合を除く`E`値の型または値の型のキャストをインスタンス化される型パラメーターは、`e`に`System.IDisposable`が発生するボックス化は発生しません。

*  の場合`E`シールされた型である、finally 句が空のブロックに拡張されます。

   ```csharp
   finally {
   }
   ```

*  それ以外の場合、finally 句に拡張されます。

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   ローカル変数`d`に表示されるか、ユーザーのコードからアクセスします。 具体的には、競合しない他の変数がスコープに含まれると、finally ブロックします。

順序`foreach`配列の要素の走査、次に示します。1 次元配列の要素は、インデックスの昇順で走査するは、インデックスから始まって `0`インデックスで終わる`Length - 1`します。 多次元配列は、右端の次元のインデックスが増加してから、左側に、[次へ] の左側ディメンションにするように、要素が走査されます。

次の例は、要素の順序で、2 次元の配列内の各値を印刷します。
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
生成される出力は次のとおりです。
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

例
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
型`n`推論されます`int`の要素型`numbers`します。

## <a name="jump-statements"></a>ジャンプ ステートメント

ジャンプ ステートメントは、コントロールを無条件で転送します。

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

ジャンプ ステートメントが制御を転送する場所が呼び出された、***ターゲット***ジャンプ ステートメントの。

ジャンプ ステートメントがブロック内で発生し、そのブロックの外側、ジャンプ ステートメントの対象では、ジャンプ ステートメントと言います***終了***ブロックします。 ジャンプ ステートメント ブロックからコントロールを譲渡、中には、ブロックにコントロールを移動ができることはありません。

介在するのかどうか、ジャンプ ステートメントの実行は複雑`try`ステートメント。 このようながない場合は、`try`ジャンプ ステートメントのステートメント、無条件で転送コントロール ジャンプ ステートメントからそのターゲットにします。 このような仲介が`try`ステートメントの実行がより複雑な。 ジャンプ ステートメントは、1 つまたは複数が終了した場合`try`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。 コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。 までこのプロセスが繰り返されます、`finally`ブロックすべての介在する`try`ステートメントが実行されています。

例
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
`finally`ブロックに関連付けられている 2 つ`try`コントロールがジャンプ ステートメントのターゲットに転送される前にステートメントが実行されます。

生成される出力は次のとおりです。
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Break ステートメント

`break`ステートメント終了囲む最も近い`switch`、 `while`、 `do`、 `for`、または`foreach`ステートメント。

```antlr
break_statement
    : 'break' ';'
    ;
```

ターゲットを`break`ステートメントは外側の終点`switch`、 `while`、 `do`、 `for`、または`foreach`ステートメント。 場合、`break`ステートメントがで囲まれていない、 `switch`、 `while`、 `do`、 `for`、または`foreach`ステートメントでは、コンパイル時エラーが発生します。

ときに複数`switch`、 `while`、 `do`、 `for`、または`foreach`ステートメントが、互いに入れ子になった、`break`ステートメントは、最も内側のステートメントにのみ適用されます。 複数の入れ子レベルの制御を転送する、`goto`ステートメント ([goto ステートメント](statements.md#the-goto-statement)) 使用する必要があります。

A`break`ステートメントが終了することはできません、`finally`ブロック ([try ステートメント](statements.md#the-try-statement))。 ときに、`break`ステートメント内で発生、`finally`のターゲットをブロック、`break`ステートメント内で同じでなければなりません`finally`ブロックは、それ以外の場合、コンパイル時エラーが発生します。

A`break`ステートメントが次のように実行されます。

*  場合、`break`ステートメントは、1 つまたは複数が終了した`try`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。 コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。 までこのプロセスが繰り返されます、`finally`ブロックすべての介在する`try`ステートメントが実行されています。
*  対象に制御が移ります、`break`ステートメント。

`break`ステートメント無条件に制御を転送他の場所での終了点を`break`ステートメントが到達可能ではありません。

### <a name="the-continue-statement"></a>Continue ステートメント

`continue`ステートメントが最も近い外側の新しいイテレーションを開始`while`、 `do`、 `for`、または`foreach`ステートメント。

```antlr
continue_statement
    : 'continue' ';'
    ;
```

ターゲットを`continue`ステートメントは外側の埋め込みステートメントの終了点`while`、 `do`、 `for`、または`foreach`ステートメント。 場合、`continue`ステートメントがで囲まれていない、 `while`、 `do`、 `for`、または`foreach`ステートメントでは、コンパイル時エラーが発生します。

ときに複数`while`、 `do`、 `for`、または`foreach`ステートメントが、互いに入れ子になった、`continue`ステートメントは、最も内側のステートメントにのみ適用されます。 複数の入れ子レベルの制御を転送する、`goto`ステートメント ([goto ステートメント](statements.md#the-goto-statement)) 使用する必要があります。

A`continue`ステートメントが終了することはできません、`finally`ブロック ([try ステートメント](statements.md#the-try-statement))。 ときに、`continue`ステートメント内で発生、`finally`のターゲットをブロック、`continue`ステートメント内で同じでなければなりません`finally`ブロックは、それ以外の場合、コンパイル時エラーが発生します。

A`continue`ステートメントが次のように実行されます。

*  場合、`continue`ステートメントは、1 つまたは複数が終了した`try`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。 コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。 までこのプロセスが繰り返されます、`finally`ブロックすべての介在する`try`ステートメントが実行されています。
*  対象に制御が移ります、`continue`ステートメント。

`continue`ステートメント無条件に制御を転送他の場所での終了点を`continue`ステートメントが到達可能ではありません。

### <a name="the-goto-statement"></a>GoTo ステートメント

`goto`ステートメントはラベルでマークされているステートメントに制御を転送します。

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

ターゲットを`goto`*識別子*ステートメントは、特定のラベルとラベル付きステートメント。 現在の関数のメンバーでは、指定した名前のラベルが存在しない場合、または場合、`goto`ステートメントは、ラベルのスコープ内で、コンパイル時エラーが発生します。 このルールの使用を許可する、`goto`ステートメントを入れ子になったスコープではなく、入れ子になったスコープ外の制御を転送します。 例
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
`goto`ステートメントを使用して、入れ子になったスコープ外の制御を転送します。

ターゲットを`goto case`ステートメントがステートメントの一覧で、すぐ外側`switch`ステートメント ([switch ステートメント](statements.md#the-switch-statement)) が含まれています、`case`特定の定数値を持つラベル。 場合、`goto case`ステートメントがで囲まれていない、`switch`ステートメントでは場合、 *constant_expression*は暗黙的に変換できません ([暗黙的な変換](conversions.md#implicit-conversions)) の管理の型をそれを囲む最も近い`switch`ステートメントでは、場合、または最も近い外側`switch`ステートメントを含まない、`case`コンパイル時エラーが発生した特定の定数値でラベルを付けます。

ターゲットを`goto default`ステートメントがステートメントの一覧で、すぐ外側`switch`ステートメント ([switch ステートメント](statements.md#the-switch-statement)) が含まれています、`default`ラベル。 場合、`goto default`ステートメントがで囲まれていない、`switch`ステートメントでは、場合囲む最も近い`switch`ステートメントを含まない、`default`ラベル付け、コンパイル時エラーが発生します。

A`goto`ステートメントが終了することはできません、`finally`ブロック ([try ステートメント](statements.md#the-try-statement))。 ときに、`goto`ステートメント内で発生、`finally`のターゲットをブロック、`goto`ステートメント内で同じでなければなりません`finally`ブロック、またはそれ以外の場合、コンパイル時エラーが発生しました。

A`goto`ステートメントが次のように実行されます。

*  場合、`goto`ステートメントは、1 つまたは複数が終了した`try`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。 コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。 までこのプロセスが繰り返されます、`finally`ブロックすべての介在する`try`ステートメントが実行されています。
*  対象に制御が移ります、`goto`ステートメント。

`goto`ステートメント無条件に制御を転送他の場所での終了点を`goto`ステートメントが到達可能ではありません。

### <a name="the-return-statement"></a>Return ステートメント

`return`ステートメントに制御を関数の現在の呼び出し元を返します、`return`ステートメントが表示されます。

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

A`return`式ステートメントは、結果の型を持つメソッドは、値を計算しない関数メンバーでのみ使用できます ([メソッド本体](classes.md#method-body)) `void`、`set`プロパティのアクセサーまたはインデクサー、`add`と`remove`イベント、インスタンス コンストラクター、静的コンストラクターまたはデストラクターのアクセサー。

A`return`式とステートメントは、void 以外の結果型を持つメソッドは、値を計算する関数のメンバーでのみ使用できます、`get`プロパティまたはインデクサー、またはユーザー定義演算子のアクセサー。 暗黙的な変換 ([暗黙的な変換](conversions.md#implicit-conversions)) 含んでいる関数メンバーの戻り値の型を式の型から存在する必要があります。

戻り値のステートメントは、匿名関数式の本文にも使用できます ([匿名関数式](expressions.md#anonymous-function-expressions))、どの変換がこれらの関数の存在の決定に参加します。

コンパイル時エラーには、`return`に表示されるステートメントを`finally`ブロック ([try ステートメント](statements.md#the-try-statement))。

A`return`ステートメントが次のように実行されます。

*  場合、`return`ステートメント、式を指定する式が評価され、結果の値は、暗黙的な変換で含まれる関数の戻り値の型に変換されます。 変換の結果では、関数によって生成される結果値になります。
*  場合、`return`ステートメントが 1 つまたは複数で囲まれた`try`または`catch`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。 コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。 までこのプロセスが繰り返されます、`finally`外側のすべてのブロック`try`ステートメントが実行されています。
*  含まれる関数は、非同期関数ではありません、存在する場合、制御が、結果の値と共に含まれる関数の呼び出し元に返されます。
*  親関数が非同期関数で、現在の呼び出し元に制御が返されますされ、結果の値、存在する場合に記録されますタスクを返す」の説明に従って場合 ([列挙子インターフェイス](classes.md#enumerator-interfaces))。

`return`ステートメント無条件に制御を転送他の場所での終了点を`return`ステートメントが到達可能ではありません。

### <a name="the-throw-statement"></a>Throw ステートメント

`throw`ステートメントが例外をスローします。

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

A`throw`式を持つステートメント、式を評価することによって生成された値をスローします。 式は、クラス型の値を表す必要があります`System.Exception`から派生したクラス型の`System.Exception`または型パラメーターの型を持つの`System.Exception`(またはそのサブクラス)、有効な基本クラスとして。 式の評価が生成される場合`null`、`System.NullReferenceException`が代わりにスローされます。

A`throw`式ステートメントでのみ使用できます、`catch`ブロック、そのステートメントが再処理をしている現在の例外をスロー後者`catch`ブロックします。

`throw`ステートメント無条件に制御を転送他の場所での終了点を`throw`ステートメントが到達可能ではありません。

最初に制御が移りますが、例外がスローされたときに`catch`句で、それを囲む`try`例外を処理できるステートメント。 適切な例外ハンドラーに制御を転送するポイントにスローされる例外のポイントから実行されるプロセスと呼びます***例外の反映***します。 まで、次の手順を繰り返し評価するは、例外の反映、`catch`例外に一致する句が見つかりました。 この説明で、***スロー ポイント***で例外がスローされる場所は、最初にします。

*  現在の関数メンバーの各`try`ステートメントを囲むスロー ポイントが調べられます。 各ステートメントに対して`S`最も内側以降の`try`ステートメントと、最も外側で終了するまで`try`ステートメントでは、次の手順が評価されます。

   * 場合、`try`ブロック`S`スロー ポイントを囲む S が 1 つまたは複数`catch`句、`catch`句は、適切なハンドラーに指定された規則に従って、例外を検索する外観の順序でチェックされますセクション[try ステートメント](statements.md#the-try-statement)します。 一致する場合`catch`句が見つかると、そのブロックに制御を転送して例外の反映が完了した`catch`句。

   * の場合、`try`ブロックまたは`catch`のブロック`S`スロー ポイントを囲む場合`S`が、`finally`に転送されるブロック、制御、`finally`ブロックします。 場合、`finally`ブロックは、別の例外をスロー、現在の例外の処理が終了します。 それ以外の場合、コントロールの終了ポイントに達したら、`finally`ブロック、現在の例外の処理が続行します。

*  例外ハンドラーが現在の関数の呼び出しでない場合は、関数の呼び出しが終了し、次のいずれか。

   * 現在の関数が非同期以外の場合は、上記の手順が、関数の呼び出し元の関数メンバーの呼び出し元のステートメントに対応するスロー ポイントに繰り返されます。

   * 現在の関数は、async とタスクを返すには、例外は」の説明に従ってエラーが発生したか取り消された状態には、戻り値のタスクに記録されます。[列挙子インターフェイス](classes.md#enumerator-interfaces)します。

   * 現在のスレッドの同期コンテキストを通知する」の説明に従って、現在の関数が非同期と void を返す場合は、[列挙可能なインターフェイス](classes.md#enumerable-interfaces)します。

*  例外の処理には、現在のスレッドですべての関数メンバーの呼び出しが終了する場合、スレッドに、例外のハンドラーがないことを示すし、スレッドが自体が終了します。 このような終了の影響は、実装定義です。

## <a name="the-try-statement"></a>Try ステートメント

`try`ステートメント ブロックの実行中に発生する例外をキャッチするためのメカニズムを提供します。 さらに、`try`ステートメントでは、コントロールが離れるときに常に実行されるコードのブロックを指定する機能、`try`ステートメント。

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

次の 3 つの可能な形式としては、`try`ステートメント。

*  A`try`ブロックでは、1 つまたは複数続く`catch`ブロックします。
*  A`try`ブロックが続く、`finally`ブロックします。
*  A`try`ブロックでは、1 つまたは複数続く`catch`ブロックが続く、`finally`ブロックします。

ときに、`catch`句を指定します、 *exception_specifier*、型でなければなりません`System.Exception`から派生した型`System.Exception`または型パラメーターの型を持つ`System.Exception`(またはそのサブクラス)、効果的なとして基本クラスです。

ときに、`catch`句では、両方を指定します、 *exception_specifier*で、*識別子*、***例外変数***指定した名前と型の宣言します。 例外変数は、そのスコープでローカル変数に対応、`catch`句。 実行中に、 *exception_filter*と*ブロック*、例外変数は、現在処理中の例外を表します。 確実な代入をチェックのために、例外変数は間違いなく全体のスコープに割り当てられていると見なされます。

しない限り、`catch`句にはで例外変数名が含まれて、フィルターで例外オブジェクトにアクセスすることはできませんと`catch`ブロックします。

A`catch`句が指定されていない、 *exception_specifier* 、一般的なと呼びます`catch`句。

一部のプログラミング言語から派生したオブジェクトとして表現ではない例外をサポート可能性があります`System.Exception`、C# コードで、このような例外を生成しない可能性があります。 一般的な`catch`このような例外をキャッチする句を使用することがあります。 したがって、一般的な`catch`句は、種類を指定する 1 つから意味の異なる`System.Exception`前者可能性がありますも例外をキャッチする他の言語で、します。

例外のハンドラーを見つけるために`catch`句は、構文の順序でチェックされます。 場合、`catch`句には例外フィルターなしの型を指定します、それ以降のコンパイル時エラーが`catch`で同じ句`try`ステートメントを入力すると、同じかから派生する型を指定します。 場合、`catch`句型を持たないとフィルターなしを指定、最後にある必要があります`catch`句を`try`ステートメント。

内で、`catch`ブロック、`throw`ステートメント ([throw ステートメント](statements.md#the-throw-statement)) 式を指定しないと、によってキャッチされた例外を再スローに使用できる、`catch`ブロックします。 例外変数への割り当てでは、再スローされる例外は変更されません。

例
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
メソッド`F`例外をキャッチ、診断情報をコンソールに出力、例外変数を変更し、例外が再度スローします。 再スローされる例外には、元の例外があるため、生成される出力です。
```
Exception in F: G
Exception in Main: G
```

最初の catch ブロックをスローしていた場合`e`現在の例外をスローするには、代わりに生成される出力は次のようになります。
```csharp
Exception in F: G
Exception in Main: F
```

コンパイル時エラーには、 `break`、 `continue`、または`goto`ステートメントの制御を転送する、`finally`ブロックします。 ときに、 `break`、 `continue`、または`goto`でステートメントが発生した、`finally`ステートメントのターゲット ブロックが同じでなければなりません`finally`ブロック、またはそれ以外の場合、コンパイル時エラーが発生しました。

コンパイル時エラーには、`return`ステートメントで発生する、`finally`ブロックします。

A`try`ステートメントが次のように実行されます。

*  制御が移ります、`try`ブロックします。
*  コントロールの終了点に達すると、`try`ブロック。
   *  場合、`try`ステートメントには、`finally`ブロック、`finally`ブロックが実行されます。
   *  終点に制御が移ります、`try`ステートメント。

*  例外が伝達される場合、`try`ステートメントの実行中に、`try`ブロック。
   *  `catch`句は、存在する場合、適切な例外ハンドラーを検索する外観の順序でチェックされます。 場合、`catch`句は、型を指定していないまたは例外の種類または例外の種類の基本型を指定します。
      *  場合、`catch`句例外変数を宣言して、例外オブジェクトが例外変数に割り当てられます。
      *  場合、`catch`句は、例外フィルターを宣言して、フィルターが評価されます。 評価されると、 `false`catch 句は、一致ではない場合、いずれかで検索を続行する後続`catch`の適切なハンドラー句。
      *  それ以外の場合、`catch`句は、一致と見なされ、照合に制御が移ります`catch`ブロックします。
      *  コントロールの終了点に達すると、`catch`ブロック。
         * 場合、`try`ステートメントには、`finally`ブロック、`finally`ブロックが実行されます。
         * 終点に制御が移ります、`try`ステートメント。
      *  例外が伝達される場合、`try`ステートメントの実行中に、`catch`ブロック。
         *  場合、`try`ステートメントには、`finally`ブロック、`finally`ブロックが実行されます。
         *  [次へ] の外側に、例外が伝達される`try`ステートメント。
   *  場合、`try`ステートメントが no`catch`句しない場合または`catch`句に一致する例外。
      *  場合、`try`ステートメントには、`finally`ブロック、`finally`ブロックが実行されます。
      *  [次へ] の外側に、例外が伝達される`try`ステートメント。

ステートメントを`finally`ブロックは、コントロールが離れるときに常に実行を`try`ステートメント。 コントロールの転送を実行した結果として、通常の実行の結果として発生するかどうかこれは true、 `break`、 `continue`、 `goto`、または`return`ステートメント、または例外の反映の結果として、 `try`ステートメント。

実行中に例外がスローされた場合、`finally`ブロックし、キャッチされない例外の反映 [次へ] の外側に同じ finally ブロック内で`try`ステートメント。 別の例外の反映中だった場合は、その例外は失われます。 例外を伝達するプロセスについては、説明の説明にさらに、`throw`ステートメント ([throw ステートメント](statements.md#the-throw-statement))。

`try`のブロックを`try`ステートメントに到達できる場合、`try`ステートメントに到達します。

A`catch`のブロックを`try`ステートメントに到達できる場合、`try`ステートメントに到達します。

`finally`のブロックを`try`ステートメントに到達できる場合、`try`ステートメントに到達します。

終点を`try`ステートメントに到達できるは、次の両方に該当する場合。

*  終点、`try`ブロックに到達できないか、末尾が少なくとも 1 つのポイント`catch`ブロックに到達します。
*  場合、`finally`ブロックが存在する場合は、終点、`finally`ブロックに到達します。

## <a name="the-checked-and-unchecked-statements"></a>Checked と unchecked ステートメント

`checked`と`unchecked`ステートメントは制御を使用して、***オーバーフロー チェック コンテキスト***整数型の算術演算と変換します。

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

`checked`ステートメントは、すべての式、*ブロック*checked コンテキストで評価されると、`unchecked`ステートメントは、すべての式、*ブロック*で評価される、unchecked コンテキスト。

`checked`と`unchecked`ステートメントは同等では、`checked`と`unchecked`演算子 ([checked と unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) 式ではなくブロック上で動作する点を除いて、.

## <a name="the-lock-statement"></a>Lock ステートメント

`lock`ステートメント、特定のオブジェクトの相互排他ロックが取得、ステートメントを実行、ロックを解放します。

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

式を`lock`ステートメントがある既知の型の値を表す必要があります、 *reference_type*します。 暗黙的なボックス化変換なし ([ボックス化変換](conversions.md#boxing-conversions)) の式が実行されるまで、`lock`ステートメント、およびそのため、式の値を表すことのコンパイル時エラー、 *value_type*.

A`lock`形式のステートメント
```csharp
lock (x) ...
```
場所`x`の式を指定する*reference_type*とまったく同じです
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
ただし、`x` が評価されるのは 1 回だけです。

相互排他ロックが保持されている間、同じ実行スレッドで実行されるコードできますも入手して、ロックを解放します。 ただし、他のスレッドで実行されるコードは、ロックが解放されるまで、ロックの取得からブロックされます。

ロック`System.Type`静的データへのアクセスを同期するためにオブジェクトはお勧めしません。 他のコードは、デッドロックが発生することができますが、同じ型にロックがあります。 プライベート静的オブジェクトをロックして静的なデータへのアクセスを同期することをお勧めします。 例:
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

## <a name="the-using-statement"></a>using ステートメント

`using`ステートメントが 1 つまたは複数のリソースを取得し、ステートメントを実行およびリソースを破棄します。

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

A***リソース***クラスまたは構造体を実装する`System.IDisposable`、という名前の 1 つのパラメーターなしのメソッドを含む`Dispose`します。 リソースを使用してコードを呼び出すことができます`Dispose`をリソースが不要になったことを示します。 場合`Dispose`は呼び出されません、ガベージ コレクションの結果として自動的に破棄が最終的が発生します。

場合のフォーム*resource_acquisition*は*local_variable_declaration*の型、 *local_variable_declaration*いずれかである必要があります`dynamic`または型暗黙的に変換できる`System.IDisposable`します。 場合のフォーム*resource_acquisition*は*式*この式は、暗黙的に変換可能である必要がありますし、`System.IDisposable`します。

宣言されたローカル変数、 *resource_acquisition*は読み取り専用であり、初期化子を含める必要があります。 コンパイル時エラーが発生する場合は、埋め込みステートメントが、これらのローカル変数を変更しようとしています。 (割り当てまたは`++`と`--`演算子)、それらのアドレスを取得またはとして渡したり`ref`または`out`パラメーター。

A`using`ステートメントは 3 つの部分に変換されます。 取得、使用状況、および破棄します。 リソースの使用量が暗黙的にで囲まれている、`try`ステートメントを含む、`finally`句。 これは、`finally`句は、リソースを破棄します。 場合、`null`のリソースを取得すると、なしの呼び出しを`Dispose`が行われると例外はスローされません。 型の場合は、リソース`dynamic`動的の暗黙的な変換を動的に変換されます ([動的の暗黙的な変換](conversions.md#implicit-dynamic-conversions)) に`IDisposable`変換があることを確認するには取得中に使用および破棄する前に成功します。

A`using`形式のステートメント
```csharp
using (ResourceType resource = expression) statement
```
次の 3 つの可能な展開のいずれかに対応します。 ときに`ResourceType`null 非許容値型では、拡張が、
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

それ以外の場合、when`ResourceType`が null 許容値型または参照型以外の`dynamic`、拡張
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

それ以外の場合、when`ResourceType`は`dynamic`拡張が、
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

どちらの展開、`resource`変数には、埋め込みのステートメントでは、読み取り専用と`d`変数で、アクセス不可能と非表示は、埋め込みステートメントにします。

上記の拡張と一貫した動作であれば、パフォーマンス上の理由などの異なる方法では、特定の using ステートメントを実装するために実装が許可されます。

A`using`形式のステートメント
```csharp
using (expression) statement
```
同じ 3 つの可能な拡張があります。 ここで`ResourceType`のコンパイル時の型を暗黙的には、`expression`があるいずれかの場合。 それ以外の場合、インターフェイス`IDisposable`自体として提供される、`ResourceType`します。 `resource`変数で、アクセス不可能と非表示は、埋め込みステートメントにします。

ときに、 *resource_acquisition*の形式を*local_variable_declaration*、指定された型の複数のリソースを取得することができます。 A`using`形式のステートメント
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
シーケンスに正確に同等の入れ子になって`using`ステートメント。
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

次の例は、という名前のファイルを作成します。`log.txt`し、ファイルに 2 行のテキストを書き込みます。 読み取り用にその同じファイルを開きし、コンソールに含まれている行のテキストをコピーします。
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

以降、`TextWriter`と`TextReader`クラスで実装、`IDisposable`例を使用できるインターフェイス、`using`ステートメントを基になるファイルを次の書き込み正しく閉じることを確認または読み取り操作。

## <a name="the-yield-statement"></a>Yield ステートメント

`yield`反復子ブロックでは、ステートメントを使用してください ([ブロック](statements.md#blocks))、列挙子オブジェクトに値を生成する ([列挙子オブジェクト](classes.md#enumerator-objects)) または列挙可能なオブジェクト ([の列挙可能なオブジェクト。](classes.md#enumerable-objects))反復子のまたはイテレーションの終了を通知します。

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` 予約語です。直前に使用する場合にのみ、特別な意味を`return`または`break`キーワード。 他のコンテキストで`yield`識別子として使用できます。

いくつかの制限がある場所について、`yield`次」の説明に従って、ステートメントを表示できます。

*  コンパイル時エラーには、 `yield` (いずれかの形式) のステートメントの外側に表示する、 *method_body*、 *operator_body*または*accessor_body*
*  コンパイル時エラーには、 `yield` (のいずれかの形式) ステートメント、匿名関数内に表示します。
*  コンパイル時エラーには、 `yield` (いずれかの形式) のステートメントに表示される、`finally`の句、`try`ステートメント。
*  コンパイル時エラーには、`yield return`どこにでも表示するステートメントを`try`ステートメントを含む`catch`句。

次の例のいくつかの有効および無効な使用`yield`ステートメント。

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

暗黙的な変換 ([暗黙的な変換](conversions.md#implicit-conversions)) 内の式の型から存在する必要があります、 `yield return` yield 型ステートメント ([型を生成](classes.md#yield-type)) 反復子の。

A`yield return`ステートメントが次のように実行されます。

*  ステートメントで指定された式が評価、暗黙的に、yield 型に変換されに割り当てられている、`Current`列挙子オブジェクトのプロパティ。
*  反復子ブロックの実行が中断されます。 場合、`yield return`内で 1 つまたは複数のステートメントが`try`ブロック、関連付けられている`finally`ブロックは、この時点では実行されません。
*  `MoveNext`列挙子オブジェクトのメソッドを返します`true`列挙子オブジェクトは、次の項目に正常に進んだことを示す、呼び出し元にします。

次の列挙子オブジェクトの呼び出し`MoveNext`メソッドは、前回中断された場所からの反復子ブロックの実行を再開します。

A`yield break`ステートメントが次のように実行されます。

*  場合、`yield break`ステートメントが 1 つまたは複数で囲まれた`try`のブロックに関連付けられている`finally`、コントロール ブロックが最初に転送、`finally`最も内側のブロック`try`ステートメント。 コントロールの終了点に達すると、`finally`に転送されるブロック、制御、 `finally` [次へ] を囲むブロック`try`ステートメント。 までこのプロセスが繰り返されます、`finally`外側のすべてのブロック`try`ステートメントが実行されています。
*  コントロールは、反復子ブロックの呼び出し元に返されます。 いずれかになります、`MoveNext`メソッドまたは`Dispose`列挙子オブジェクトのメソッド。

`yield break`ステートメント無条件に制御を転送他の場所での終了点を`yield break`ステートメントが到達可能ではありません。
