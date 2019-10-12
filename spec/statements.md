---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704038"
---
# <a name="statements"></a>ステートメント

C#には、さまざまなステートメントが用意されています。 これらのステートメントのほとんどは、C およびC++でプログラミングされている開発者にとってはよく知られています。

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

*Embedded_statement*非終端要素は、他のステートメント内に出現するステートメントに使用されます。 *ステートメント*ではなく*embedded_statement*を使用すると、これらのコンテキストで宣言ステートメントとラベル付きステートメントの使用が除外されます。 例
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
`if` ステートメントでは、if 分岐の*ステートメント*ではなく*embedded_statement*が必要になるため、コンパイル時エラーが発生します。 このコードが許可された場合、 `i`変数は宣言されますが、使用することはできません。 ただし、の宣言をブロックに`i`配置すると、この例は有効です。

## <a name="end-points-and-reachability"></a>エンドポイントと到達可能性

すべてのステートメントに***エンドポイント***があります。 直感的に言うと、ステートメントの終了点は、ステートメントの直後にある場所です。 複合ステートメント (埋め込みステートメントを含むステートメント) の実行ルールでは、コントロールが埋め込みステートメントのエンドポイントに到達したときに実行されるアクションを指定します。 たとえば、コントロールがブロック内のステートメントの終了位置に到達すると、コントロールはブロック内の次のステートメントに転送されます。

実行によってステートメントに到達できる場合は、ステートメントが***到達***可能であると言います。 逆に、ステートメントが実行される可能性がない場合、ステートメントは "***到達不能***" と呼ばれます。

この例では、
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
の2番目`Console.WriteLine`の呼び出しは、ステートメントが実行される可能性がないため、到達できません。

ステートメントに到達できないとコンパイラが判断した場合、警告が報告されます。 具体的には、ステートメントに到達できない場合のエラーではありません。

特定のステートメントまたはエンドポイントに到達できるかどうかを判断するために、コンパイラは、ステートメントごとに定義された到達可能性規則に従ってフロー分析を実行します。 フロー分析では、ステートメントの動作を制御する定数式 ([定数式](expressions.md#constant-expressions)) の値が考慮されますが、非定数式で使用できる値は考慮されません。 つまり、制御フロー分析の目的で、指定された型の非定数式は、その型の可能な値を持つと見なされます。

この例では、
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
`if`ステートメントのブール式は、 `==`演算子の両方のオペランドが定数であるため、定数式です。 定数式がコンパイル時に評価され、値`false` `Console.WriteLine`が生成されると、呼び出しは到達不能と見なされます。 ただし、が`i`ローカル変数に変更された場合は、
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine`呼び出しは到達可能と見なされますが、実際には実行されません。

関数メンバーの*ブロック*は常に到達可能と見なされます。 ブロック内の各ステートメントの到達可能性規則を連続して評価することにより、特定のステートメントの到達可能性を特定できます。

この例では、
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
2番目`Console.WriteLine`の到達可能性は、次のように決定されます。

*  最初`Console.WriteLine`の式ステートメントに到達できるのは、 `F`メソッドのブロックに到達可能であるためです。
*  最初`Console.WriteLine`の式ステートメントの終点に到達できるのは、そのステートメントに到達可能であるためです。
*  `if` 最初`Console.WriteLine`の式ステートメントの終点に到達可能であるため、ステートメントに到達できます。
*  ステートメントのブール式に定数値が指定`Console.WriteLine` `false`されていないため、2番目の式ステートメントに到達できます。 `if`

ステートメントのエンドポイントが到達可能になるには、コンパイル時にエラーが発生するという2つの状況があります。

*  `switch`ステートメントでは、switch セクションが次の switch セクションに "フォールスルー" されることは許可されていないため、switch セクションのステートメントリストのエンドポイントが到達可能であると、コンパイル時にエラーが発生します。 このエラーが発生した場合は、通常、 `break`ステートメントが存在しないことを示しています。
*  これは、到達可能な値を計算する関数メンバーのブロックのエンドポイントに対するコンパイル時のエラーです。 このエラーが発生した場合は、通常、 `return`ステートメントが存在しないことを示しています。

## <a name="blocks"></a>ブロック

"*ブロック*" を使用すると、1 つのステートメントしか使用できないコンテキストで複数のステートメントを記述できます。

```antlr
block
    : '{' statement_list? '}'
    ;
```

*ブロック*は、省略可能な*statement_list* ([ステートメントリスト](statements.md#statement-lists)) で構成され、中かっこで囲まれています。 ステートメントリストが省略されている場合、ブロックは空であると言われます。

ブロックには、宣言ステートメント ([宣言ステートメント](statements.md#declaration-statements)) を含めることができます。 ブロックで宣言されているローカル変数または定数のスコープは、ブロックです。

ブロックは次のように実行されます。

*  ブロックが空の場合、制御はブロックの終点に転送されます。
*  ブロックが空でない場合、制御はステートメントリストに転送されます。 コントロールがステートメントリストの終点に到達した場合、制御はブロックの終点に移ります。

ブロック自体に到達可能な場合、ブロックのステートメントリストに到達できます。

ブロックが空の場合、またはステートメントリストの終点に到達できる場合は、ブロックの終点に到達できます。

1つ`yield`以上のステートメントを含むブロック ([yield ステートメント](statements.md#the-yield-statement)) は、反復子ブロックと呼ばれます。 反復子ブロックは、関数メンバーを反復子 ([反復子](classes.md#iterators)) として実装するために使用されます。 反復子ブロックには、いくつかの追加の制限が適用されます。

*  `return`ステートメントが反復子ブロックに出現する場合、コンパイル時エラーになります (ただし`yield return` 、ステートメントは許可されます)。
*  反復子ブロックに unsafe コンテキスト ([unsafe](unsafe-code.md#unsafe-contexts)コンテキスト) が含まれていると、コンパイル時にエラーになります。 反復子ブロックは、その宣言が unsafe コンテキストで入れ子になっている場合でも、常に安全なコンテキストを定義します。

### <a name="statement-lists"></a>ステートメントの一覧

***ステートメントリスト***は、順番に記述された1つ以上のステートメントで構成されます。 ステートメントリストは、*ブロック*s ([ブロック](statements.md#blocks)) と*switch_block*s ([switch ステートメント](statements.md#the-switch-statement)) で発生します。

```antlr
statement_list
    : statement+
    ;
```

ステートメントリストを実行するには、最初のステートメントに制御を移します。 コントロールがステートメントの終点に到達した場合、制御は次のステートメントに移ります。 コントロールが最後のステートメントの終点に達した場合、制御はステートメントリストのエンドポイントに移ります。

次のいずれかの条件に該当する場合は、ステートメントリスト内のステートメントに到達できます。

*  ステートメントが最初のステートメントであり、ステートメントリスト自体に到達可能である。
*  先行するステートメントの終点に到達できる。
*  ステートメントがラベル付きステートメントであり、到達可能`goto`なステートメントによってラベルが参照されています。

リスト内の最後のステートメントの終点に到達できる場合は、ステートメントリストの終点に到達できます。

## <a name="the-empty-statement"></a>空のステートメント

*Empty_statement*は何も行いません。

```antlr
empty_statement
    : ';'
    ;
```

ステートメントが必要なコンテキストで実行する操作がない場合は、空のステートメントが使用されます。

空のステートメントを実行すると、単に制御がステートメントのエンドポイントに転送されます。 空のステートメントに到達できる場合、空のステートメントのエンドポイントに到達できるようになります。

空のステートメントは、本文が null の`while`ステートメントを記述するときに使用できます。
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

また、空のステートメントを使用して、ブロックの終わりの "`}`" の直前にラベルを宣言することもできます。
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>ラベル付きステートメント

*Labeled_statement*を使用すると、ステートメントの先頭にラベルを付けることができます。 ラベル付きステートメントはブロックで許可されますが、埋め込みステートメントとして使用することはできません。

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

ラベル付きステートメントは、*識別子*によって指定された名前を持つラベルを宣言します。 ラベルのスコープは、入れ子になったブロックを含め、ラベルが宣言されているすべてのブロックです。 同じ名前の2つのラベルが重複するスコープを持つ場合、コンパイル時エラーになります。

ラベルは、ラベルのスコープ`goto`内のステートメント ([goto ステートメント](statements.md#the-goto-statement)) から参照できます。 つまり`goto` 、ステートメントは、ブロック内およびブロック内で制御を転送できますが、ブロックには移動できません。

ラベルには独自の宣言領域があり、他の識別子に干渉することはありません。 例
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
は有効で、パラメーターと`x`ラベルの両方として名前を使用します。

ラベル付きステートメントの実行は、ラベルに続くステートメントの実行に正確に対応します。

ラベルが到達可能`goto`なステートメントによって参照されている場合、通常の制御フローによって提供される到達可能性に加えて、ラベル付きステートメントに到達できるようになります。 例外的`try` `try` `finally`ステートメントがブロックを`finally`含む内にあり、ラベルが付けられたステートメントがの外側にあり、ブロックの終点に到達できない場合、ラベルが付けられたステートメントはから到達できません。 `goto`この`goto`ステートメントです。)

## <a name="declaration-statements"></a>宣言ステートメント

*Declaration_statement*は、ローカル変数または定数を宣言します。 宣言ステートメントはブロックで許可されていますが、埋め込みステートメントとして使用することはできません。

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>ローカル変数の宣言

*Local_variable_declaration*は、1つ以上のローカル変数を宣言します。

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

*Local_variable_declaration*の*local_variable_type*は、宣言によって導入された変数の型を直接指定します。または、初期化子に基づいて型を推論する必要があることを `var` の識別子と共に示します。 型の後に*local_variable_declarator*のリストが続き、それぞれに新しい変数が導入されています。 *Local_variable_declarator*は、変数に名前を付けた*識別子*と、必要に応じて "`=`" トークン、および変数の初期値を指定する*local_variable_initializer*で構成されます。

ローカル変数宣言のコンテキストでは、識別子 var はコンテキストキーワード ([キーワード](lexical-structure.md#keywords)) として機能します。*Local_variable_type*が `var` として指定され、`var` という名前の型がスコープ内にない場合、宣言は暗黙的に型指定された***ローカル変数宣言***であり、その型は、関連付けられた初期化子式の型から推論されます。 暗黙的に型指定されるローカル変数宣言には、次の制限があります。

*  *Local_variable_declaration*に複数の*local_variable_declarator*を含めることはできません。
*  *Local_variable_declarator*には、 *local_variable_initializer*を含める必要があります。
*  *Local_variable_initializer*は*式*である必要があります。
*  初期化子*式*は、コンパイル時の型である必要があります。
*  初期化子*式*は、宣言された変数自体を参照できません

暗黙的に型指定された不適切なローカル変数宣言の例を次に示します。

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

ローカル変数の値は、 *simple_name* ([簡易名](expressions.md#simple-names)) を使用して式で取得され、ローカル変数の値は*代入*[演算子 (代入演算子](expressions.md#assignment-operators)) を使用して変更されます。 ローカル変数は、値が取得される場所ごとに、確実に割り当てられる必要があります ([明確な代入](variables.md#definite-assignment))。

*Local_variable_declaration*で宣言されたローカル変数のスコープは、宣言が発生するブロックです。 ローカル変数の*local_variable_declarator*の前にあるテキスト位置でローカル変数を参照すると、エラーになります。 ローカル変数のスコープ内では、同じ名前を持つ別のローカル変数または定数を宣言するコンパイル時エラーになります。

複数の変数を宣言するローカル変数宣言は、同じ型の単一の変数の複数の宣言と同じです。 さらに、ローカル変数宣言内の変数初期化子は、宣言の直後に挿入される代入ステートメントと完全に一致します。

例
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
はに正確に対応します。
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

暗黙的に型指定されたローカル変数宣言では、宣言されるローカル変数の型は、変数の初期化に使用される式の型と同じになります。 以下に例を示します。
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

上記の暗黙的に型指定されたローカル変数宣言は、明示的に型指定された次の宣言とまったく同じです。
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>ローカル定数宣言

*Local_constant_declaration*は、1つ以上のローカル定数を宣言します。

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

*Local_constant_declaration*の*型*は、宣言によって導入される定数の型を指定します。 型の後に*constant_declarator*のリストが続き、それぞれに新しい定数が導入されています。 *Constant_declarator*は、定数に名前を付け、その後に "`=`" トークンを続け、その後に定数の値を指定する*constant_expression* ([定数式](expressions.md#constant-expressions)) を指定する*識別子*で構成されます。

ローカル定数宣言の*型*と*constant_expression*は、定数メンバー宣言 ([定数](classes.md#constants)) と同じ規則に従う必要があります。

ローカル定数の値は、 *simple_name* ([簡易名](expressions.md#simple-names)) を使用して式で取得されます。

ローカル定数のスコープは、宣言が発生するブロックです。 *Constant_declarator*の前にあるテキスト位置でローカル定数を参照すると、エラーになります。 ローカル定数のスコープ内では、同じ名前を持つ別のローカル変数または定数を宣言するコンパイル時エラーになります。

複数の定数を宣言するローカル定数宣言は、同じ型を持つ単一定数の複数の宣言と同じです。

## <a name="expression-statements"></a>式ステートメント

*Expression_statement*は、指定された式を評価します。 式によって計算された値 (存在する場合) は破棄されます。

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

すべての式がステートメントとして許可されているわけではありません。 特に、や`x + y` `x == 1`などの式では、値を計算するだけで (破棄されます)、ステートメントとして使用することはできません。

*Expression_statement*を実行すると、含まれている式が評価され、 *expression_statement*の終点に制御が移ります。 *Expression_statement*に到達できる場合、 *expression_statement*のエンドポイントに到達できます。

## <a name="selection-statements"></a>選択ステートメント

選択ステートメントでは、いくつかの式の値に基づいて、実行に使用できるステートメントをいくつか選択します。

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If ステートメント

ステートメント`if`は、ブール式の値に基づいて実行するステートメントを選択します。

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

構文で許可されている、前`if`に最も近い構文にパートが関連付けられています。`else` そのため、 `if`
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

`if`ステートメントは次のように実行されます。

*  *Boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。
*  ブール式`true`がの場合、制御は最初の埋め込みステートメントに転送されます。 コントロールがそのステートメントの終点に到達した場合、制御は`if`ステートメントの終了点に移ります。
*  ブール式がを`false` `else`生成し、パーツが存在する場合、制御は2番目の埋め込みステートメントに転送されます。 コントロールがそのステートメントの終点に到達した場合、制御は`if`ステートメントの終了点に移ります。
*  ブール式がを生成`false`し、 `else`パーツが存在しない場合、制御は`if`ステートメントの終了点に移ります。

ステートメントが到達可能で、 `if`ブール式が定数値`if` `false`を持たない場合は、ステートメントの最初の埋め込みステートメントに到達できます。

ステートメントが到達可能で、 `if`ブール式が定数値`true`を持たない`if`場合は、ステートメントの2番目の埋め込みステートメント (存在する場合) に到達できます。

少なくとも1つ`if`の埋め込みステートメントの終点に到達できる場合、ステートメントの終了位置に到達できます。 また、 `if`ステートメントが到達可能で、 `if`ブール式が`else`定数値`true`を持たない場合は、部分を持たないステートメントの終点に到達できます。

### <a name="the-switch-statement"></a>Switch ステートメント

Switch ステートメントは、switch 式の値に対応するスイッチラベルが関連付けられているステートメントリストの実行を選択します。

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

*Switch_statement*は、キーワード `switch`、その後にかっこで囲まれた式 (switch 式と呼ばれます)、 *switch_block*の順で構成されます。 *Switch_block*は、中かっこで囲まれた0個以上の*switch_section*s で構成されます。 各*switch_section*は、1つ以上の*switch_label*s の後に*statement_list* ([ステートメントリスト](statements.md#statement-lists)) で構成されます。

`switch`ステートメントの***管理型***は、switch 式によって設定されます。

*  スイッチ式の型が `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`bool`、`char`、0、または*enum_type*のいずれかの型に対応する null 許容型である場合は、は、2 ステートメントの管理型です。
*  それ以外の場合、ユーザー定義の暗黙的な変換 ([ユーザー定義の変換](conversions.md#user-defined-conversions)) は、switch 式の型から、次のいずれかの管理型に`sbyte`する必要`short`が`ushort`あります。、 `byte`、、`int` 、、`char`、 、`long` 、、`string`、または。これらの型のいずれかに対応する null 許容型。 `ulong` `uint`
*  それ以外の場合、このような暗黙的な変換が存在しない場合、または複数の暗黙的な変換が存在する場合は、コンパイル時エラーが発生します。

各`case`ラベルの定数式は、 `switch`ステートメントの管理型に暗黙的に変換可能な ([暗黙の変換](conversions.md#implicit-conversions)) 値を示す必要があります。 `case` 同じ`switch`ステートメント内の2つ以上のラベルで同じ定数値が指定されている場合、コンパイル時エラーが発生します。

Switch ステートメントには、最大`default`で1つのラベルを設定できます。

`switch`ステートメントは次のように実行されます。

*  Switch 式が評価され、管理型に変換されます。
*  同じ`case` `case`ステートメントのラベルに指定されているいずれかの定数が switch 式の値と等しい場合は、一致するラベルの後にあるステートメントリストに制御が移ります。 `switch`
*  同じ`case` `default` `default`ステートメントのラベルに指定されている定数が switch 式の値と等しい場合、およびラベルが存在する場合、制御は次のようなステートメントの一覧に移ります。 `switch`タイトル.
*  同じ`case` `default` `switch`ステートメントのラベルに指定されている定数が switch 式の値と等しい場合、ラベルが存在しない場合、制御はステートメントのエンドポイントに転送されます。 `switch`

Switch セクションのステートメントリストの終点に到達できる場合、コンパイル時エラーが発生します。 これは "フォールスルー" ルールと呼ばれます。 例
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
は、到達可能なエンドポイントを持つ switch セクションがないため、有効です。 C やとC++は異なり、switch セクションの実行は、次の switch セクションに "フォールスルー" することはできません。例
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
コンパイル時エラーが発生します。 Switch セクションの実行後に別の switch セクションを実行する場合は、明示的`goto case`なまたは`goto default`ステートメントを使用する必要があります。
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

*Switch_section*では、複数のラベルを使用できます。 例
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
が有効です。 この例では、ラベル `case 2:`、`default:` は同じ*switch_section*の一部であるため、"フォールスルー" ルールに違反しません。

"フォールスルーなし" ルールは、C で発生する一般的なバグクラスを防ぎC++ 、 `break`ステートメントが誤って省略された場合に発生します。 また、このルールにより、ステートメントの動作に影響を`switch`与えずに、ステートメントの switch セクションを任意に再配置できます。 たとえば、上記の`switch`ステートメントのセクションは、ステートメントの動作に影響を与えずに元に戻すことができます。
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

Switch セクションのステートメントの一覧は`break`、通常、 `goto case`、、または`goto default`ステートメントで終了しますが、ステートメントリストの終点を表示するコンストラクトは使用できません。 たとえば、ブール式`while` `true`によって制御されるステートメントは、エンドポイントに達しないことがわかっています。 同様に、 `throw`また`return`はステートメントは、常にコントロールを別の場所に転送し、エンドポイントに到達しないようにします。 したがって、次の例は有効です。
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

`switch`ステートメントの管理型は、型`string`にすることができます。 以下に例を示します。
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

文字列等値演算子 ([文字列等値](expressions.md#string-equality-operators)演算子) `switch`と同様に、ステートメントでは大文字と小文字が区別され、switch 式の`case`文字列がラベル定数と完全に一致する場合にのみ、指定された switch セクションが実行されます。

`switch`ステートメントの管理型が`string`の場合、値`null`は case ラベル定数として許可されます。

*Switch_block*の*statement_list*には、宣言ステートメント ([宣言ステートメント](statements.md#declaration-statements)) を含めることができます。 Switch ブロックで宣言されたローカル変数または定数のスコープは、スイッチブロックです。

`switch`ステートメントが到達可能で、少なくとも次のいずれかの条件に該当する場合は、特定の switch セクションのステートメントリストに到達できます。

*  スイッチ式は、非定数値です。
*  Switch 式は、switch セクションの`case`ラベルに一致する定数値です。
*  スイッチ式がどのラベルとも`case`一致しない定数値です。 switch セクションには`default`ラベルが含まれています。
*  Switch セクションのスイッチラベルが、到達可能`goto case`なまたは`goto default`ステートメントによって参照されています。

次のいずれかの`switch`条件に該当する場合は、ステートメントの終了位置に到達できます。

*  ステートメントには、 `switch`ステートメント`break`を終了する到達可能なステートメントが含まれています。 `switch`
*  ステートメントが到達可能で、スイッチ式が非定数値であり、ラベルが存在しません`default`。 `switch`
*  ステートメントに到達できます。 switch 式は、どのラベルにも`case`一致しない定数値です。ラベルは存在しません。 `default` `switch`

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

ステートメント`while`は、条件付きで埋め込みステートメントを0回以上実行します。

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

`while`ステートメントは次のように実行されます。

*  *Boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。
*  ブール式`true`がの場合、コントロールは埋め込みステートメントに転送されます。 コントロールが埋め込みステートメントの終点に到達した場合 ( `continue`ステートメントの実行から)、 `while`ステートメントの先頭に制御が移ります。
*  ブール式`false`がの場合、制御は`while`ステートメントの終了点に移ります。

ステートメント`while`の埋め込みステートメント内では`break` 、ステートメント ([break ステートメント](statements.md#the-break-statement)) を使用して、 `while`ステートメントの終了位置に制御を移すことができます (したがって、埋め込みステートメントの反復処理を終了します)。`continue`ステートメント ([continue ステートメント](statements.md#the-continue-statement)) を使用して、埋め込みステートメントのエンドポイントに制御を移すことができます (したがって`while` 、ステートメントの別の反復処理を実行します)。

ステートメントが到達可能で`while` 、ブール式が定数`while`値`false`を持たない場合は、ステートメントの埋め込みステートメントに到達できます。

次のいずれかの`while`条件に該当する場合は、ステートメントの終了位置に到達できます。

*  ステートメントには、 `while`ステートメント`break`を終了する到達可能なステートメントが含まれています。 `while`
*  ステートメントが到達可能で、ブール式に定数値`true`がありません。 `while`

### <a name="the-do-statement"></a>Do ステートメント

ステートメント`do`は、条件付きで埋め込みステートメントを1回以上実行します。

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

`do`ステートメントは次のように実行されます。

*  コントロールは、埋め込みステートメントに転送されます。
*  コントロールが埋め込みステートメントの終点に到達すると (場合によっては `continue` ステートメントの実行から)、 *boolean_expression* ([ブール式](expressions.md#boolean-expressions)) が評価されます。 ブール式`true`がの場合、制御は`do`ステートメントの先頭に移ります。 それ以外の場合、制御は`do`ステートメントのエンドポイントに転送されます。

ステートメント`do`の埋め込みステートメント内では`break` 、ステートメント ([break ステートメント](statements.md#the-break-statement)) を使用して、 `do`ステートメントの終了位置に制御を移すことができます (したがって、埋め込みステートメントの反復処理を終了します)。`continue`ステートメント ([continue ステートメント](statements.md#the-continue-statement)) を使用して、埋め込みステートメントのエンドポイントに制御を移すことができます。

ステートメントに到達できる`do` `do`場合、ステートメントの埋め込みステートメントに到達できます。

次のいずれかの`do`条件に該当する場合は、ステートメントの終了位置に到達できます。

*  ステートメントには、 `do`ステートメント`break`を終了する到達可能なステートメントが含まれています。 `do`
*  埋め込みステートメントの終点に到達でき、ブール式に定数値が指定`true`されていません。

### <a name="the-for-statement"></a>For ステートメント

ステートメント`for`は初期化式のシーケンスを評価した後、条件が true の場合、埋め込みステートメントを繰り返し実行し、反復式のシーケンスを評価します。

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

*For_initializer*(存在する場合) は、コンマで区切られた*local_variable_declaration* ([ローカル変数宣言](statements.md#local-variable-declarations)) または*statement_expression*s ([式ステートメント](statements.md#expression-statements)) のリストで構成されます。 *For_initializer*によって宣言されたローカル変数のスコープは、変数の*local_variable_declarator*から始まり、埋め込みステートメントの末尾まで拡張されます。 スコープには、 *for_condition*と*for_iterator*が含まれます。

*For_condition*(存在する場合) は、 *boolean_expression* ([ブール式](expressions.md#boolean-expressions)) である必要があります。

*For_iterator*(存在する場合) は、コンマで区切られた*Statement_expression*s ([式ステートメント](statements.md#expression-statements)) のリストで構成されます。

For ステートメントは次のように実行されます。

*  *For_initializer*が存在する場合、変数初期化子またはステートメント式は、記述された順序で実行されます。 このステップは1回だけ実行されます。
*  *For_condition*が存在する場合は、評価されます。
*  *For_condition*が存在しない場合、または評価結果が-1 @no__t の場合、コントロールは埋め込みステートメントに転送されます。 コントロールが埋め込みステートメントの終点に到達した場合 (`continue` ステートメントの実行など)、 *for_iterator*の式 (場合によっては) が順番に評価された後、次のように、別の反復処理が実行されます。上記の手順での*for_condition*の評価。
*  *For_condition*が存在し、評価結果が-1 @no__t の場合、制御は `for` ステートメントのエンドポイントに移ります。

@No__t-0 ステートメントの埋め込みステートメント内では、`break` ステートメント ([break ステートメント](statements.md#the-break-statement)) を使用して、`for` ステートメントの終点に制御を移すことができます (したがって、埋め込みステートメントの反復処理を終了します)。また、`continue` ステートメント ([Continue ステートメント](statements.md#the-continue-statement)) を使用して、埋め込みステートメントのエンドポイントに制御を移すことができます (つまり、 *for_iterator*を実行し、 *for_condition*から始まる `for` ステートメントの別の反復処理を実行します)。

次のいずれかに`for`該当する場合は、ステートメントの埋め込みステートメントに到達できます。

*  @No__t-0 ステートメントに到達可能で、 *for_condition*が存在しません。
*  @No__t-0 ステートメントに到達可能であり、 *for_condition*が存在し、定数値 `false` が指定されていません。

次のいずれかの`for`条件に該当する場合は、ステートメントの終了位置に到達できます。

*  ステートメントには、 `for`ステートメント`break`を終了する到達可能なステートメントが含まれています。 `for`
*  @No__t-0 ステートメントに到達可能であり、 *for_condition*が存在し、定数値 `true` が指定されていません。

### <a name="the-foreach-statement"></a>Foreach ステートメント

ステートメント`foreach`は、コレクションの各要素に対して埋め込みステートメントを実行して、コレクションの要素を列挙します。

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

ステートメントの*型*と*識別子* `foreach`は、ステートメントの***繰り返し変数***を宣言します。 @No__t 0 の識別子が*local_variable_type*として指定され、`var` という名前の型がスコープ内にない場合、反復変数は***暗黙的に型指定***された反復変数と呼ばれ、その型は @no__t の要素型になります。ステートメント。以下のように指定します。 繰り返し変数は、埋め込みステートメントの上にあるスコープを持つ読み取り専用のローカル変数に対応します。 反復変数は、 `foreach`ステートメントの実行中に、反復処理が現在実行されているコレクション要素を表します。 埋め込みステートメントが反復変数を変更しようとし`++`た場合 (代入`--`演算子または演算子を使用)、 `ref`または反復変数をパラメーターまたは`out`パラメーターとして渡すと、コンパイル時にエラーが発生します。

以下では、 `IEnumerable`を簡潔`IEnumerator`にするため`IEnumerable<T>`に`IEnumerator<T>` 、、、およびは、名前空間`System.Collections`と`System.Collections.Generic`の対応する型を参照しています。

Foreach ステートメントのコンパイル時の処理では、最初に、式の***コレクション型***、***列挙子の型***、および***要素の型***を決定します。 この決定は次のように行われることになります。

*  式の型`X`が配列型である場合は、から`IEnumerable`インターフェイスへの暗黙の`X`参照変換が行われ`System.Array`ます (以降はこのインターフェイスが実装されます)。 ***コレクション型***は`IEnumerable`インターフェイス、***列挙子型***はインターフェイス、 `IEnumerator` ***要素型***は配列型`X`の要素型です。
*  式の型`X`が `dynamic`の場合は、*式*から`IEnumerable`インターフェイスへの暗黙的な変換 ([暗黙の動的変換](conversions.md#implicit-dynamic-conversions)) が存在します。 ***コレクション型***は`IEnumerable`インターフェイスで、***列挙子の型***は`IEnumerator`インターフェイスです。 @No__t 0 の識別子が*local_variable_type*として指定されている場合は、***要素の型***が 3 @no__t になります。それ以外の場合は、`object` になります。
*  それ以外の場合は、 `X`型に適切`GetEnumerator`なメソッドがあるかどうかを確認します。
   * `X` 識別子`GetEnumerator`を使用して型引数を指定せずに、型に対してメンバーの参照を実行します。 メンバー参照によって一致が生成されない場合、またはあいまいさが生成される場合、またはメソッドグループではない一致が生成される場合は、次に示すように、列挙可能なインターフェイスを確認してください。 メンバー参照によってメソッドグループ以外のものが生成された場合、または一致するものがない場合は、警告を発行することをお勧めします。
   * 結果のメソッドグループと空の引数リストを使用して、オーバーロードの解決を実行します。 オーバーロードの解決によって適用できないメソッドが発生した場合、あいまいさが生じる場合、または結果が1つのベストメソッドであり、そのメソッドが静的であるかどうかにかかわらず、次に示すように、列挙可能なインターフェイスを確認してください。 オーバーロードの解決によって明確なパブリックインスタンスメソッド以外のものが生成される場合、または該当するメソッドがない場合は、警告を発行することをお勧めします。
   * メソッドの戻り値`E`の型がクラス、構造体、またはインターフェイス型ではない場合は、エラーが生成され、それ以上の手順は実行されません。 `GetEnumerator`
   * メンバー参照は、識別子`E` `Current`と型引数を指定せずにに対して実行されます。 メンバー参照が一致しない場合、結果がエラーになる場合、または読み取りを許可するパブリックインスタンスプロパティ以外の結果である場合は、エラーが生成され、それ以上の手順は実行されません。
   * メンバー参照は、識別子`E` `MoveNext`と型引数を指定せずにに対して実行されます。 メンバー参照が一致しない場合、結果がエラーになる場合、またはメソッドグループ以外のすべての結果である場合は、エラーが生成され、それ以上の手順は実行されません。
   * オーバーロードの解決は、空の引数リストを持つメソッドグループで実行されます。 オーバーロードの解決によって適用されるメソッドがない場合、あいまいさが生じるか、または1つのベストメソッドになりますが、そのメソッドは静的で`bool`もパブリックでもありません。また、戻り値の型が存在しない場合は、エラーが生成され、それ以上の手順は実行されません。
   * ***コレクション型***は`X`で、***列挙子の型***は`E`で、***要素の型***は`Current`プロパティの型です。

*  それ以外の場合は、列挙可能なインターフェイスがあるかどうかを確認します。
   * `Ti`から`dynamic` `T` `T` `Ti`への暗黙的な変換が行われているすべての型の中で、がではなく、他のすべての型が`IEnumerable<Ti>` `X`から`IEnumerable<T>`へ`IEnumerator<T>` `IEnumerable<T>`の暗黙の型変換では、コレクション型はインターフェイス、列挙子型はインターフェイス、要素型はです。 `IEnumerable<Ti>` `T`.
   * それ以外の場合、このような型`T`が複数存在すると、エラーが生成され、それ以上の手順は実行されません。
   * それ以外の場合`X` 、から`System.Collections.IEnumerable`インターフェイスへの暗黙的な変換がある場合 ***、コレクション型***はこのインターフェイス、***列挙子型***はインターフェイス`System.Collections.IEnumerator`、***要素型***はです。 `object`.
   * それ以外の場合は、エラーが生成され、それ以上の手順は実行されません。

上記の手順が成功すると、コレクション型`C`、列挙子型`E` 、および要素型`T`が明確に生成されます。 フォームの foreach ステートメント
```csharp
foreach (V v in x) embedded_statement
```
は次のように展開されます。
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

変数`e`は、式`x` 、埋め込みステートメント、またはプログラムのその他のソースコードからは参照できないか、アクセスできません。 埋め込み`v`ステートメントでは、変数は読み取り専用です。 @No__t-1 (要素型) から `V` (foreach ステートメントの*local_variable_type* ) への明示的な変換 ([明示的](conversions.md#explicit-conversions)な変換) が行われていない場合は、エラーが生成され、それ以上の手順は実行されません。 に`x`値`null`がある場合は、実行時にがスロー`System.NullReferenceException`されます。

実装では、特定の foreach ステートメントを異なる方法で実装することができます。たとえば、パフォーマンス上の理由から、動作が上記の拡張と一致している場合に限ります。

While ループ内の @no__t 0 の位置は、 *embedded_statement*で発生している匿名関数によってキャプチャされる方法にとって重要です。

以下に例を示します。
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
が`v` while ループの外側で宣言されている場合、すべてのイテレーション間で共有され、for ループの後の値が最終的`13`な値になります。これ`f`は、の呼び出しの結果です。 各反復処理には独自の変数`v`があるため、最初のイテレーションでによって`f`キャプチャされた値`7`は、出力される値を保持し続けます。 (注: の以前のC#バージョン`v`は、while ループの外側で宣言されています)。

Finally ブロックの本体は、次の手順に従って構築されます。

*  から`E` インターフェイス`System.IDisposable`への暗黙的な変換がある場合は、
   *  が`E` null 非許容の値型である場合、finally 句は次のようなセマンティックに拡張されます。

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  それ以外の場合は、finally 句がに相当するセマンティックに拡張されます。

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   が値型`E`の場合、または値型にインスタンス化された型パラメーターの場合、へ`e`の`System.IDisposable`キャストによってボックス化が発生することはありません。

*  それ以外の`E`場合、が sealed 型の場合、finally 句は空のブロックに展開されます。

   ```csharp
   finally {
   }
   ```

*  それ以外の場合、finally 句は次のように展開されます。

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   ローカル変数`d`は、ユーザーコードから参照できないか、ユーザーコードからアクセスできません。 特に、スコープに finally ブロックが含まれている他の変数と競合しません。

配列の要素を`foreach`走査する順序は次のとおりです。1次元配列の要素の場合、インデックス順にインデックスを作成 `0`し、インデックスで終了`Length - 1`します。 多次元配列の場合は、要素が走査されて、右端の次元のインデックスが最初に増加し、次に左の次元になるようになります。

次の例では、要素の順序で2次元配列の各値を出力します。
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
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

この例では、
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
の`n`型は、の`numbers`要素型`int`であると推論されます。

## <a name="jump-statements"></a>ジャンプ ステートメント

ジャンプステートメントが無条件で制御を転送します。

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

ジャンプステートメントが制御を転送する場所は、ジャンプステートメントの***ターゲット***と呼ばれます。

ジャンプステートメントがブロック内で発生し、そのジャンプステートメントの対象がそのブロックの外側にある場合、ジャンプステートメントはブロックを***終了***すると言います。 ジャンプステートメントは、ブロックから制御を移すことができますが、制御をブロックに移すことはできません。

ジャンプステートメントの実行は、介在`try`するステートメントが存在すると複雑になります。 このような`try`ステートメントが存在しない場合、ジャンプステートメントは無条件でジャンプステートメントからターゲットに制御を転送します。 このような中間`try`ステートメントが存在する場合、実行はより複雑になります。 ジャンプステートメントによって、関連付け`try`られ`finally`たブロックがある1つ以上のブロック`finally`が終了した`try`場合、最初にコントロールが最も内側のステートメントのブロックに転送されます。 コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。 このプロセスは、 `finally` `try`すべてのステートメントのブロックが実行されるまで繰り返されます。

この例では、
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
2 `finally` つ`try`のステートメントに関連付けられているブロックは、ジャンプステートメントのターゲットに制御が転送される前に実行されます。

生成される出力は次のとおりです。
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Break ステートメント

ステートメント`break`は、 `switch`最も近い、 `while` `foreach` 、、 、`for`またはステートメントを終了します。 `do`

```antlr
break_statement
    : 'break' ';'
    ;
```

`break`ステートメントのターゲットは、最も近い`while`外側`switch` `do`の、、、 `for`、または`foreach`ステートメントの終点です。 `switch`ステートメントが、`do`、、、または`foreach`ステートメントで囲まれていない場合、コンパイル時エラーが発生します。 `for` `while` `break`

複数`switch`の`while` `foreach` `break` 、 、、、またはステートメントが入れ子になっている場合、ステートメントは最も内側のステートメントにのみ適用されます。`do` `for` 複数の入れ子レベルで制御を転送する`goto`には、ステートメント ([goto ステートメント](statements.md#the-goto-statement)) を使用する必要があります。

ステートメント`break`でブロックを終了`finally`することはできません ([try ステートメント](statements.md#the-try-statement))。 ステートメントが`finally`ブロック内にある場合`break` 、ステートメントのターゲットは同じ`finally`ブロック内になければなりません。それ以外の場合は、コンパイル時エラーが発生します。 `break`

`break`ステートメントは次のように実行されます。

*  ステートメントが`break` 、関連付けられ`try` `finally`た`finally`ブロックを持つ1つ以上のブロックを終了した場合、 `try`最初にコントロールが最も内側のステートメントのブロックに転送されます。 コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。 このプロセスは、 `finally` `try`すべてのステートメントのブロックが実行されるまで繰り返されます。
*  制御は、 `break`ステートメントのターゲットに転送されます。

ステートメントは`break`無条件で制御を別の場所に転送するの`break`で、ステートメントのエンドポイントに到達できません。

### <a name="the-continue-statement"></a>Continue ステートメント

ステートメント`continue`は、 `while`最も近い`do` `foreach` 、、、またはステートメントの新しい反復処理を開始します。 `for`

```antlr
continue_statement
    : 'continue' ';'
    ;
```

`continue`ステートメントの対象は、最も近い外側`while`の、 `do` `for`、、または`foreach`ステートメントの埋め込みステートメントの終点です。 `continue`ステートメントが`while`、 `do`、、または`foreach`ステートメントで囲まれていない場合、コンパイル時エラーが発生します。 `for`

`while`複数の`do` 、、`continue` 、また`foreach`はステートメントが相互に入れ子になっている場合、ステートメントは最も内側のステートメントにのみ適用されます。 `for` 複数の入れ子レベルで制御を転送する`goto`には、ステートメント ([goto ステートメント](statements.md#the-goto-statement)) を使用する必要があります。

ステートメント`continue`でブロックを終了`finally`することはできません ([try ステートメント](statements.md#the-try-statement))。 ステートメントが`finally`ブロック内にある場合`continue` 、ステートメントのターゲットは同じ`finally`ブロック内になければなりません。それ以外の場合は、コンパイル時エラーが発生します。 `continue`

`continue`ステートメントは次のように実行されます。

*  ステートメントが`continue` 、関連付けられ`try` `finally`た`finally`ブロックを持つ1つ以上のブロックを終了した場合、 `try`最初にコントロールが最も内側のステートメントのブロックに転送されます。 コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。 このプロセスは、 `finally` `try`すべてのステートメントのブロックが実行されるまで繰り返されます。
*  制御は、 `continue`ステートメントのターゲットに転送されます。

ステートメントは`continue`無条件で制御を別の場所に転送するの`continue`で、ステートメントのエンドポイントに到達できません。

### <a name="the-goto-statement"></a>GoTo ステートメント

ステートメント`goto`は、ラベルによってマークされているステートメントに制御を移します。

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

`goto` *識別子*ステートメントのターゲットは、指定されたラベルを持つラベル付きステートメントです。 指定した名前のラベルが現在の関数メンバーに存在しない場合、または`goto`ステートメントがラベルのスコープ内にない場合は、コンパイル時エラーが発生します。 この規則により、ステートメントを`goto`使用して、入れ子になったスコープから制御を移すことができますが、入れ子になったスコープには移動できません。 この例では、
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
ステートメント`goto`は、入れ子になったスコープから制御を移すために使用されます。

`goto case`ステートメントのターゲットは、指定された定数値を持つ`switch`ラベルを`case`含む、すぐ外側のステートメント ([switch ステートメント](statements.md#the-switch-statement)) のステートメントリストです。 @No__t-0 ステートメントが `switch` ステートメントで囲まれていない場合、最も近い最も近い `switch` ステートメントの管理型に*constant_expression*が暗黙的に変換 ([暗黙の変換](conversions.md#implicit-conversions)) されない場合、または最も近い`switch` ステートメントに、指定された定数値を持つ @no__t 6 のラベルが含まれていません。コンパイル時のエラーが発生します。

`goto default`ステートメントの対象となるのは、ラベルを`default`含むすぐ外側`switch`のステートメント ([switch ステートメント](statements.md#the-switch-statement)) のステートメントリストです。 ステートメントが`switch`ステートメントで囲まれていない場合、または最も近い`switch`外側のステートメントに`default`ラベルが含まれていない場合は、コンパイル時エラーが発生します。 `goto default`

ステートメント`goto`でブロックを終了`finally`することはできません ([try ステートメント](statements.md#the-try-statement))。 ステートメントが`finally`ブロック内にある場合`goto` 、ステートメントのターゲットは同じ`finally`ブロック内に存在する必要があります。そうでない場合は、コンパイル時エラーが発生します。 `goto`

`goto`ステートメントは次のように実行されます。

*  ステートメントが`goto` 、関連付けられ`try` `finally`た`finally`ブロックを持つ1つ以上のブロックを終了した場合、 `try`最初にコントロールが最も内側のステートメントのブロックに転送されます。 コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。 このプロセスは、 `finally` `try`すべてのステートメントのブロックが実行されるまで繰り返されます。
*  制御は、 `goto`ステートメントのターゲットに転送されます。

ステートメントは`goto`無条件で制御を別の場所に転送するの`goto`で、ステートメントのエンドポイントに到達できません。

### <a name="the-return-statement"></a>Return ステートメント

ステートメント`return`は、 `return`ステートメントが存在する関数の現在の呼び出し元に制御を返します。

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

`set` `add` `void`式が指定されていないステートメントは、値を計算しない関数メンバーでのみ使用できます。つまり、結果の型([メソッド本体](classes.md#method-body))を持つメソッド、プロパティまたはインデクサーの`return`アクセサー、イベント`remove`のアクセサー、インスタンスコンストラクター、静的コンストラクター、またはデストラクター。

式を持つ`get` ステートメントは、値を計算する関数メンバー、つまり、void以外の結果型、プロパティまたはインデクサーのアクセサー、またはユーザー定義の演算子を持つメソッドを使用する場合にのみ`return`使用できます。 暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) は、式の型から、含んでいる関数メンバーの戻り値の型に存在する必要があります。

Return ステートメントは、匿名関数式 ([匿名関数式](expressions.md#anonymous-function-expressions)) の本体で使用することもできます。また、これらの関数にどの変換が存在するかを判断するためにも使用できます。

`return`ステートメントが`finally`ブロック ([try ステートメント](statements.md#the-try-statement)) に含まれる場合、コンパイル時エラーになります。

`return`ステートメントは次のように実行されます。

*  `return`ステートメントが式を指定した場合、式が評価され、結果の値は暗黙的な変換によって、含んでいる関数の戻り値の型に変換されます。 変換の結果は、関数によって生成される結果値になります。
*  `try` `catch` `try` `finally` `finally`ステートメントが1つ以上のまたはブロックに関連付けられたブロックによって囲まれている場合は、最初にコントロールが最も内側のステートメントのブロックに転送されます。 `return` コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。 このプロセスは、 `finally`外側`try`のすべてのステートメントのブロックが実行されるまで繰り返されます。
*  含んでいる関数が非同期関数でない場合、制御は、含まれている関数の呼び出し元に、結果値 (存在する場合) と共に返されます。
*  含まれている関数が非同期関数の場合、コントロールは現在の呼び出し元に返され、結果値 (存在する場合) は、「([列挙子インターフェイス](classes.md#enumerator-interfaces))」で説明されているように、返されるタスクに記録されます。

ステートメントは`return`無条件で制御を別の場所に転送するの`return`で、ステートメントのエンドポイントに到達できません。

### <a name="the-throw-statement"></a>Throw ステートメント

ステートメント`throw`が例外をスローします。

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

式を含むステートメントでは、式を評価することによって生成される値がスローされます。 `throw` 式は、クラス`System.Exception`型の値を表す必要があります。これは、またはサブクラスを持つ`System.Exception`型パラメーター型から`System.Exception` 、有効な基本クラスとして派生するクラス型の値を表します。 式の評価によって`null` `System.NullReferenceException`が生成される場合は、代わりにがスローされます。

式が指定されていない`catch` `catch`ステートメントは、ブロック内でのみ使用できます。その場合、ステートメントは、現在そのブロックによって処理されている例外を再スローします。 `throw`

ステートメントは`throw`無条件で制御を別の場所に転送するの`throw`で、ステートメントのエンドポイントに到達できません。

例外がスローされると、その例外を処理できる`catch`外側`try`のステートメント内の最初の句に制御が移ります。 適切な例外ハンドラーに制御を転送するポイントにスローされた例外の発生点から実行されるプロセスは、***例外の伝達***と呼ばれます。 例外の伝達は、例外に一致する`catch`句が見つかるまで、次の手順を繰り返し評価することで構成されます。 この説明では、最初に***スローポイント***が例外がスローされる場所です。

*  現在の関数メンバーでは、 `try`スローポイントを囲む各ステートメントが検査されます。 ステートメント`S`ごとに、最も内側`try`のステートメントで始まり、最も外側`try`のステートメントで終了すると、次の手順が評価されます。

   * `try`の`catch` `catch`ブロックがスローポイントを囲む場合、S に1つ以上の句が含まれている場合は、「」で指定されている規則に従って、例外に適したハンドラーを検索するために、表示順に句が調べられます。 `S`[Try ステートメントの](statements.md#the-try-statement)セクションです。 一致`catch`する句が見つかった場合は、その`catch`句のブロックに制御を移すことによって、例外の伝達が完了します。

   * それ以外の場合`try` 、ブロック`catch`またはの`S`ブロックがスローポイントを`finally`囲む`S`場合、およびにブロックがある場合は`finally` 、制御がブロックに転送されます。 ブロックで`finally`別の例外がスローされた場合、現在の例外の処理は終了します。 それ以外の場合、コントロールが`finally`ブロックの終点に到達すると、現在の例外の処理が続行されます。

*  現在の関数呼び出しで例外ハンドラーが見つからなかった場合は、関数の呼び出しが終了し、次のいずれかが発生します。

   * 現在の関数が非同期でない場合、上記の手順は関数の呼び出し元に対して、関数メンバーが呼び出されたステートメントに対応する throw ポイントを使用して繰り返されます。

   * 現在の関数が非同期でタスクを返す場合、例外は return タスクに記録されます。これは、「[列挙子インターフェイス](classes.md#enumerator-interfaces)」で説明されているように、エラーまたはキャンセル状態になります。

   * 現在の関数が async および void を返す場合、現在のスレッドの同期コンテキストは、「列挙可能な[インターフェイス](classes.md#enumerable-interfaces)」で説明されているように通知されます。

*  例外処理によって、現在のスレッドのすべての関数メンバー呼び出しが終了し、そのスレッドに例外のハンドラーがないことが示された場合、そのスレッド自体は終了します。 このような終了の影響は、実装によって定義されます。

## <a name="the-try-statement"></a>Try ステートメント

ステートメント`try`は、ブロックの実行中に発生した例外をキャッチするためのメカニズムを提供します。 さらに`try` 、ステートメントを使用すると、コントロールがステートメントから出たときに常に実行されるコードブロックを`try`指定できます。

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

ステートメントには、次の`try` 3 つの形式があります。

*  ブロック`try`の後に1つ`catch`以上のブロックが続きます。
*  ブロック`try`の後`finally`にブロックが続く。
*  ブロック`try`の後に1つ`catch`以上のブロックが続き`finally` 、その後にブロックが続きます。

@No__t-0 句で*exception_specifier*が指定 @no__t されている場合、型は、`System.Exception` から派生した型、または有効な基本クラスとして `System.Exception` (またはサブクラス) を持つ型パラメーター型である必要があります。

@No__t-0 句が*識別子*を持つ*exception_specifier*の両方を指定すると、指定された名前と型の***例外変数***が宣言されます。 例外変数は、 `catch`句を超えてスコープを持つローカル変数に対応します。 *Exception_filter*と*block*の実行中、例外変数は現在処理中の例外を表します。 明確な割り当てチェックの目的では、例外変数はスコープ全体で確実に割り当てられていると見なされます。

句に`catch`例外変数名が含まれていない限り、フィルターおよび`catch`ブロック内の例外オブジェクトにアクセスすることはできません。

*Exception_specifier*を指定しない @no__t 0 句は、general `catch` 句と呼ばれます。

一部のプログラミング言語は、から`System.Exception`派生したオブジェクトとして表現できない例外をサポートする場合がありますが、このような例外はコードによってC#生成されることはありません。 一般的`catch`な句は、このような例外をキャッチするために使用できます。 したがって、一般的`catch`な句は、型`System.Exception`を指定するものとは意味が異なります。つまり、前者は他の言語からも例外をキャッチする可能性があります。

例外のハンドラーを見つけるために、 `catch`句は構文の順序で検証されます。 句に型が指定されていても例外フィルターがない場合は、同じ`try`ステートメント内`catch`の後の句で、その型と同じか、またはその型から派生した型を指定するコンパイル時エラーになります。 `catch` 句に`catch`型が指定されておらず、フィルターもない場合`catch`は、その`try`ステートメントの最後の句である必要があります。

ブロック内では`throw` 、式のないステートメント ([throw ステートメント](statements.md#the-throw-statement)) を使用して、 `catch`ブロックでキャッチされた例外を再スローできます。 `catch` 例外変数への代入では、再スローされた例外は変更されません。

この例では、
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
メソッド`F`は、例外をキャッチし、いくつかの診断情報をコンソールに書き込み、例外変数を変更して、例外を再スローします。 再スローされる例外は元の例外なので、生成される出力は次のようになります。
```console
Exception in F: G
Exception in Main: G
```

現在の例外を再スローする`e`のではなく、最初の catch ブロックがスローされた場合、生成される出力は次のようになります。
```console
Exception in F: G
Exception in Main: F
```

`break`、、また`continue`は`goto`ステートメントが`finally`ブロックの外部で制御を転送する場合、コンパイル時エラーになります。 `break` `finally` 、 、`continue`または`goto`ステートメントがブロック内にある場合、ステートメントのターゲットは同じブロック内になければなりません。それ以外の場合は、`finally`コンパイル時エラーが発生します。

`return`ステートメントが`finally`ブロック内で発生する場合、コンパイル時エラーになります。

`try`ステートメントは次のように実行されます。

*  コントロールは`try`ブロックに転送されます。
*  コントロールが`try`ブロックの終点に到達した場合は、次のようになります。
   *  ステートメントにブロックがある場合は、ブロックが実行されます。`finally` `finally` `try`
   *  制御は、 `try`ステートメントのエンドポイントに転送されます。

*  ブロックの実行中に例外が`try`ステートメントに反映される場合は、次のようになります。 `try`
   *  句`catch` (存在する場合) は、例外に適したハンドラーを見つけるために、表示順に調べられます。 `catch`句が型を指定しない場合、または例外の種類または例外の種類の基本型を指定する場合は、次のようになります。
      *  句で`catch`例外変数が宣言されている場合、例外オブジェクトは例外変数に割り当てられます。
      *  句で`catch`例外フィルターを宣言すると、フィルターが評価されます。 と評価`false`された場合、catch 句は一致しないため、適切なハンドラーの後続`catch`の句の中で検索が続行されます。
      *  それ以外の`catch`場合、句は一致と見なされ、制御は一致`catch`するブロックに転送されます。
      *  コントロールが`catch`ブロックの終点に到達した場合は、次のようになります。
         * ステートメントにブロックがある場合は、ブロックが実行されます。`finally` `finally` `try`
         * 制御は、 `try`ステートメントのエンドポイントに転送されます。
      *  ブロックの実行中に例外が`try`ステートメントに反映される場合は、次のようになります。 `catch`
         *  ステートメントにブロックがある場合は、ブロックが実行されます。`finally` `finally` `try`
         *  例外は、次の外側`try`のステートメントに反映されます。
   *  ステートメントに`try` `catch`句が含まれていない`catch`場合、または句が例外に一致しない場合は、次のようになります。
      *  ステートメントにブロックがある場合は、ブロックが実行されます。`finally` `finally` `try`
      *  例外は、次の外側`try`のステートメントに反映されます。

コントロールがステートメントを`finally` `try`離れると、常にブロックのステートメントが実行されます。 `break`これは`continue` `try` 、、、 `return` 、またはステートメントを実行した結果として、通常の実行の結果として制御転送が行われるか、または、例外が`goto`諸表.

`finally`ブロックの実行中に例外がスローされ、同じ finally ブロック内でキャッチされない場合、例外は次の外側`try`のステートメントに反映されます。 別の例外が伝達の処理中であった場合、その例外は失われます。 例外を反映するプロセスについては、 `throw`ステートメントの説明 ([throw ステートメント](statements.md#the-throw-statement)) でさらに説明します。

ステートメント`try`に到達できる`try`場合`try` 、ステートメントのブロックに到達できます。

ステートメントに到達できる`try`場合`try` 、ステートメントのブロックに到達できます。`catch`

ステートメント`finally`に到達できる`try`場合`try` 、ステートメントのブロックに到達できます。

次の両方に該当`try`する場合は、ステートメントの終了位置に到達できます。

*  `try`ブロックの終点に到達可能であるか、少なくとも1つ`catch`のブロックのエンドポイントに到達できる。
*  ブロックが`finally`存在する場合は、 `finally`ブロックのエンドポイントに到達できます。

## <a name="the-checked-and-unchecked-statements"></a>Checked ステートメントと unchecked ステートメント

`checked` および`unchecked`ステートメントは、整数型の算術演算および変換の***オーバーフローチェックコンテキスト***を制御するために使用されます。

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

ステートメントにより、*ブロック*内のすべての式が checked `unchecked`コンテキストで評価され、ステートメントによってブロック内のすべての式が unchecked コンテキストで評価されます。 `checked`

`checked` `unchecked` ステートメントとステートメントは、式ではなくブロックで動作する点を除いて、and 演算子 ([checked および unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) とまったく同じです。 `checked` `unchecked`

## <a name="the-lock-statement"></a>Lock ステートメント

ステートメント`lock`は、指定されたオブジェクトの相互排他ロックを取得し、ステートメントを実行してから、ロックを解放します。

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

@No__t-0 ステートメントの式は、 *reference_type*であることがわかっている型の値を示す必要があります。 @No__t-1 ステートメントの式に対しては、暗黙的なボックス変換 ([ボックス](conversions.md#boxing-conversions)化変換) は行われません。したがって、式が*value_type*の値を示すためにコンパイル時にエラーが発生します。

フォーム`lock`のステートメント
```csharp
lock (x) ...
```
ここで `x` は*reference_type*の式であり、とまったく同じです。
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

相互排他ロックが保持されている間、同じ実行スレッドで実行されているコードもロックを取得して解放できます。 ただし、他のスレッドで実行されているコードは、ロックが解除されるまでロックを取得できません。

静的`System.Type`データへのアクセスを同期するためにオブジェクトをロックすることは推奨されません。 他のコードは同じ型をロックし、デッドロックが発生する可能性があります。 より適切な方法は、プライベート静的オブジェクトをロックすることによって、静的データへのアクセスを同期することです。 以下に例を示します。
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

ステートメント`using`は、1つまたは複数のリソースを取得し、ステートメントを実行してから、リソースを破棄します。

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

***リソース***とは、を実装`System.IDisposable`するクラスまたは構造体です。これには、という名前`Dispose`のパラメーターなしのメソッドが1つ含まれます。 リソースを使用しているコードは`Dispose` 、リソースが不要になったことを示すためにを呼び出すことができます。 が`Dispose`呼び出されていない場合は、ガベージコレクションの結果として、最終的に自動破棄が行われます。

*Resource_acquisition*の形式が*local_variable_declaration*の場合、 *local_variable_declaration*の型は `dynamic` であるか、または暗黙的に `System.IDisposable` に変換できる型である必要があります。 *Resource_acquisition*の形式が*expression*の場合、この式は、暗黙的に `System.IDisposable` に変換可能である必要があります。

*Resource_acquisition*で宣言されたローカル変数は読み取り専用であり、初期化子を含める必要があります。 埋め込みステートメントがこれらの`++`ローカル変数 (代入`--`演算子または演算子を使用して) を変更しようとした場合、コンパイル時エラーが発生した場合`out`は、それらの変数のアドレスを受け取るか、またはパラメーターとして`ref`渡します。

`using`ステートメントは、取得、使用、および破棄という3つの部分に変換されます。 リソースの使用は、句を`try` `finally`含むステートメントで暗黙的に囲まれます。 この`finally`句はリソースを破棄します。 リソースが取得された場合、へ`Dispose`の呼び出しは行われず、例外はスローされません。 `null` リソースの種類`dynamic`がである場合は、使用前に変換が成功したことを確認する`IDisposable`ために、暗黙的な動的変換 (暗黙の[動的](conversions.md#implicit-dynamic-conversions)変換) によって取得時に動的に変換されます。処分.

フォーム`using`のステートメント
```csharp
using (ResourceType resource = expression) statement
```
考えられる3つの展開のいずれかに対応します。 が`ResourceType` null 非許容の値型である場合、拡張は
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

それ以外の`ResourceType`場合、が null 許容値型または以外`dynamic`の参照型である場合、展開は
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

それ以外の`ResourceType`場合`dynamic`、がの場合、拡張は
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

どちらの展開でも`resource` 、埋め込みステートメント`d`では変数が読み取り専用になり、埋め込みステートメントでは変数にアクセスできず、非表示になります。

実装では、指定された using ステートメントを異なる方法で実装することができます。たとえば、パフォーマンス上の理由から、動作が上記の拡張と一致している場合に限ります。

フォーム`using`のステートメント
```csharp
using (expression) statement
```
では、3つの展開が可能です。 この場合`ResourceType` 、は、暗黙的にのコンパイル時の型`expression`です (存在する場合)。 それ以外の`IDisposable`場合は、 `ResourceType`インターフェイス自体がとして使用されます。 `resource`変数は、埋め込みステートメントではアクセスできず、非表示になります。

*Resource_acquisition*が*local_variable_declaration*の形式を取る場合、特定の種類のリソースを複数取得することができます。 フォーム`using`のステートメント
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
は、入れ子になっ`using`たステートメントのシーケンスとまったく同じです。
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

次の例では、と`log.txt`いう名前のファイルを作成し、ファイルに2行のテキストを書き込みます。 この例では、同じファイルを読み取り用に開き、含まれているテキスト行をコンソールにコピーします。
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

クラスと`TextWriter` `TextReader`クラスは`IDisposable`インターフェイスを実装するため、この例`using`ではステートメントを使用して、書き込み操作または読み取り操作の後で、基になるファイルが適切に閉じられるようにすることができます。

## <a name="the-yield-statement"></a>Yield ステートメント

この`yield`ステートメントは、反復子オブジェクト ([列挙子オブジェクト](classes.md#enumerator-objects)) または反復子の列挙可能なオブジェクト ([列挙可能オブジェクト](classes.md#enumerable-objects))に値を生成したり、反復処理の終了を通知したりするために、反復子ブロック ([ブロック](statements.md#blocks)) で使用されます。

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield`は予約語ではありません。または`return` `break`キーワードの直前に使用された場合にのみ、特別な意味を持ちます。 他のコンテキストで`yield`は、を識別子として使用できます。

次に示すように、ステートメント`yield`を表示できる場所にはいくつかの制限があります。

*  @No__t-0 ステートメント (いずれかの形式) が*method_body*、 *operator_body* 、または*accessor_body*の外部に表示される場合、コンパイル時エラーになります。
*  `yield`ステートメント (いずれかの形式) が匿名関数内に出現する場合、コンパイル時エラーになります。
*  ステートメントが`yield` ステートメント`try`の`finally`句に記述されている場合は、コンパイル時にエラーになります。
*  `yield return`ステートメントが`try` 任意`catch`の句を含むステートメント内の任意の場所に出現する場合、コンパイル時エラーになります。

次の例では、ステートメントの`yield`有効な使用方法と無効な使用方法を示します。

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

暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の`yield return`変換) は、ステートメント内の式の型から、反復子の yield 型 ([yield 型](classes.md#yield-type)) に存在する必要があります。

`yield return`ステートメントは次のように実行されます。

*  ステートメントで指定された式が評価され、暗黙的に yield 型に変換され`Current` 、列挙子オブジェクトのプロパティに割り当てられます。
*  反復子ブロックの実行が中断されています。 ステートメントが1つ`try`以上のブロック内にある場合、 `finally`関連付けられたブロックは現時点では実行されません。 `yield return`
*  列挙`MoveNext`子オブジェクトのメソッドは、 `true`その呼び出し元にを返します。これは、列挙子オブジェクトが次の項目に正常に進んだことを示します。

次に列挙子オブジェクトの`MoveNext`メソッドを呼び出すと、最後に中断された位置から反復子ブロックの実行が再開されます。

`yield break`ステートメントは次のように実行されます。

*  `finally` `try` `try`ステートメントが、関連付けられた`finally`ブロックがある1つ以上のブロックによって囲まれている場合、最初にコントロールが最も内側のステートメントのブロックに転送されます。 `yield break` コントロールが`finally`ブロックの終点に到達した場合、コントロールは次の外側`try`の`finally`ステートメントのブロックに転送されます。 このプロセスは、 `finally`外側`try`のすべてのステートメントのブロックが実行されるまで繰り返されます。
*  コントロールは、反復子ブロックの呼び出し元に返されます。 これは、列挙`MoveNext`子オブジェクト`Dispose`のメソッドまたはメソッドのいずれかです。

ステートメントは`yield break`無条件で制御を別の場所に転送するの`yield break`で、ステートメントのエンドポイントに到達できません。
