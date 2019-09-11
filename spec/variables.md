---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876797"
---
# <a name="variables"></a>変数

変数は、ストレージの場所を表します。 すべての変数には、変数に格納できる値を決定する型があります。 C#はタイプセーフな言語であり、コンパイラC#は変数に格納されている値が常に適切な型であることを保証します。 変数の値は、代入によって、または演算子`++`と`--`演算子を使用して変更できます。

値を取得する前に、変数を***確実***に代入 ([明確な代入](variables.md#definite-assignment)) する必要があります。

以下のセクションで説明するように、変数は***最初に割り当てら***れるか、***最初***に割り当てが解除されます。 最初に割り当てられた変数には、明確に定義された初期値があり、常に割り当てられていると見なされます。 初期値が割り当てられていない変数には初期値がありません。 最初に割り当てられていない変数を特定の場所で確実に割り当てられるようにするには、その場所に至る可能性のあるすべての実行パスで、変数への代入を行う必要があります。

## <a name="variable-categories"></a>変数のカテゴリ

C#変数の7つのカテゴリ (静的変数、インスタンス変数、配列要素、値パラメーター、参照パラメーター、出力パラメーター、およびローカル変数) を定義します。 以下のセクションでは、これらの各カテゴリについて説明します。

この例では、
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
`x`は静的`y`変数、はインスタンス`v[0]`変数、は配列要素`a` 、は`c`値パラメーター `b` 、は参照パラメーター、は出力パラメーター `i` 、はローカル変数です.

### <a name="static-variables"></a>静的変数

`static`修飾子を使用して宣言されたフィールドは、***静的変数***と呼ばれます。 静的変数は、それを含んでいる型に対して静的コンストラクター ([静的](classes.md#static-constructors)コンストラクター) を実行する前に存在し、関連付けられているアプリケーションドメインが存在しなくなったときには存在しなくなります。

静的変数の初期値は、変数の型の既定値 ([既定](variables.md#default-values)値) です。

確実な代入のチェックのために、静的変数は最初に割り当てられたものと見なされます。

### <a name="instance-variables"></a>インスタンス変数

修飾子を`static`指定せずに宣言されたフィールドは、***インスタンス変数***と呼ばれます。

#### <a name="instance-variables-in-classes"></a>クラスのインスタンス変数

クラスのインスタンス変数は、そのクラスの新しいインスタンスが作成されるときに存在し、そのインスタンスへの参照がなく、インスタンスのデストラクター (存在する場合) が実行されたときに存在しなくなります。

クラスのインスタンス変数の初期値は、変数の型の既定値 ([既定](variables.md#default-values)値) です。

明確な代入を確認するために、クラスのインスタンス変数は最初に割り当てられたものと見なされます。

#### <a name="instance-variables-in-structs"></a>構造体のインスタンス変数

構造体のインスタンス変数の有効期間は、それが属する構造体変数とまったく同じです。 つまり、構造体型の変数が存在する場合、または存在しなくなった場合は、構造体のインスタンス変数を取得します。

構造体のインスタンス変数の初期割り当ての状態は、それを含んでいる構造体の変数と同じです。 つまり、構造体変数が最初に割り当てられていると見なされると、そのインスタンス変数もあります。また、struct 変数が最初に割り当てられていないと見なされると、そのインスタンス変数は同様に割り当て解除されます。

### <a name="array-elements"></a>配列要素

配列の要素は、配列インスタンスの作成時に存在し、その配列インスタンスへの参照がない場合は存在しなくなります。

配列の各要素の初期値は、配列要素の型の既定値 ([既定](variables.md#default-values)値) です。

明確な割り当てチェックのために、配列要素は最初に割り当てられたと見なされます。

### <a name="value-parameters"></a>値パラメーター

または`ref` `out`修飾子を指定せずに宣言されたパラメーターは、***値パラメーター***です。

値パラメーターは、関数メンバー (メソッド、インスタンスコンストラクター、アクセサー、または演算子) の呼び出し時、またはパラメーターが属する匿名関数の呼び出し時に存在し、呼び出しで指定された引数の値で初期化されます。 通常、値パラメーターは、関数メンバーまたは匿名関数を返したときに存在しなくなります。 ただし、値パラメーターが匿名関数 ([匿名関数式](expressions.md#anonymous-function-expressions)) によってキャプチャされる場合、その匿名関数から作成されたデリゲートまたは式ツリーがガベージコレクションの対象になるまで、その有効期間は少なくともになります。

明確な割り当てチェックのために、値パラメーターは最初に割り当てられたと見なされます。

### <a name="reference-parameters"></a>参照パラメーター

修飾子を`ref`使用して宣言されたパラメーターは***参照パラメーター***です。

参照パラメーターでは、新しいストレージの場所は作成されません。 代わりに、参照パラメーターは、関数メンバーまたは匿名関数呼び出しで引数として指定された変数と同じ格納場所を表します。 したがって、参照パラメーターの値は、常に基になる変数と同じになります。

参照パラメーターには、次の明確な代入規則が適用されます。 「[出力パラメーター](variables.md#output-parameters)」で説明されている出力パラメーターのさまざまな規則に注意してください。

*  変数は、関数メンバーまたはデリゲート呼び出しで参照パラメーターとして渡す前に、確実に代入 ([明確な代入](variables.md#definite-assignment)) する必要があります。
*  関数メンバーまたは匿名関数内では、参照パラメーターは最初に割り当てられたと見なされます。

インスタンスメソッドまたは構造体型のインスタンスアクセサー内では`this` 、キーワードは構造体型の参照パラメーターとまったく同じように動作します ([このアクセス](expressions.md#this-access))。

### <a name="output-parameters"></a>出力パラメーター

`out`修飾子を使用して宣言されたパラメーターは、***出力パラメーター***です。

出力パラメーターでは、新しいストレージの場所は作成されません。 代わりに、出力パラメーターは、関数メンバーまたはデリゲート呼び出しの引数として指定された変数と同じ格納場所を表します。 したがって、出力パラメーターの値は、常に基になる変数と同じになります。

出力パラメーターには、次の明確な代入規則が適用されます。 参照パラメーターについては、「[参照パラメーター](variables.md#reference-parameters)」で説明しているさまざまな規則に注意してください。

*  変数は、関数メンバーまたはデリゲート呼び出しの出力パラメーターとして渡す前に、確実に割り当てる必要はありません。
*  関数メンバーの通常の完了またはデリゲート呼び出しの後に、出力パラメーターとして渡された各変数は、その実行パスで割り当てられていると見なされます。
*  関数メンバーまたは匿名関数内では、出力パラメーターは最初に割り当てられていないと見なされます。
*  関数メンバーまたは匿名関数のすべての出力パラメーターは、関数メンバーまたは匿名関数が正常に返される前に、確実に代入される必要があります ([明確な代入](variables.md#definite-assignment))。

構造体型のインスタンスコンストラクター内では、 `this`キーワードは構造体型の出力パラメーターとして正確に動作します ([このアクセス](expressions.md#this-access))。

### <a name="local-variables"></a>ローカル変数

***ローカル変数***は、 *local_variable_declaration*によって宣言されます。これは、*ブロック*、 *for_statement*、 *switch_statement* 、または*using_statement*で発生する可能性があります。または、 *foreach_statement*または*try_statement*の*specific_catch_clause* 。

ローカル変数の有効期間は、ストレージが予約されていることが保証されるプログラム実行の部分です。 この有効期間は、少なくともの entry から、それが関連付けられている*block*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*にまで及びます。その*block*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*の実行は、どのような形で終了します。 (囲まれた*ブロック*を入力するか、メソッドを呼び出すと、現在の*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または specific_ の実行が中断されますが、終了することはありません。 *catch_clause*)ローカル変数が匿名関数 (キャプチャされた[外部変数](expressions.md#captured-outer-variables)) によってキャプチャされる場合、その有効期間は、匿名関数から作成されたデリゲートまたは式ツリーと、キャプチャされた変数は、ガベージコレクションの対象になります。

親*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*が再帰的に入力されると、ローカル変数の新しいインスタンスが作成されます。時間とその*local_variable_initializer*がある場合は、毎回評価されます。

*Local_variable_declaration*によって導入されるローカル変数は自動的に初期化されないため、既定値はありません。 明確な割り当てチェックの目的で、 *local_variable_declaration*によって導入されたローカル変数は、最初に未割り当てと見なされます。 *Local_variable_declaration*には、 *local_variable_initializer*を含めることができます。この場合、変数は初期化式 ([宣言ステートメント](variables.md#declaration-statements)) の後にのみ確実に割り当てられたと見なされます。

*Local_variable_declaration*によって導入されるローカル変数のスコープ内では、そのローカル変数を*local_variable_declarator*の前にあるテキスト位置で参照するコンパイル時エラーになります。 ローカル変数宣言が暗黙的 ([ローカル変数宣言](statements.md#local-variable-declarations)) である場合は、 *local_variable_declarator*内で変数を参照することもエラーになります。

*Foreach_statement*または*specific_catch_clause*によって導入されたローカル変数は、そのスコープ全体で確実に割り当てられていると見なされます。

ローカル変数の実際の有効期間は実装に依存します。 たとえば、コンパイラは、ブロック内のローカル変数が、そのブロックの一部にのみ使用されることを静的に判断する場合があります。 この分析を使用すると、コンパイラは、変数のストレージの有効期間が、それを含んでいるブロックよりも短くなるようなコードを生成できます。

ローカル参照変数によって参照されるストレージは、ローカル参照変数の有効期間 ([自動メモリ管理](basic-concepts.md#automatic-memory-management)) とは無関係に再利用されます。

## <a name="default-values"></a>既定の値

次のカテゴリの変数は、自動的に既定値に初期化されます。

*  静的変数
*  クラスインスタンスのインスタンス変数。
*  配列要素。

変数の既定値は、変数の型によって異なり、次のように決定されます。

*  *Value_type*の変数の既定値は、 *value_type*の既定のコンストラクター ([既定のコンストラクター](types.md#default-constructors)) によって計算された値と同じです。
*  *Reference_type*の変数の場合、既定値は`null`です。

既定値への初期化は、通常、メモリマネージャーまたはガベージコレクターが、使用のために割り当てられる前にすべてのビットゼロにメモリを初期化することによって行われます。 このため、null 参照を表すためにすべて-bit-0 を使用すると便利です。

## <a name="definite-assignment"></a>明確な代入

関数メンバーの実行可能コード内の特定の場所では、コンパイラが特定の静的フロー分析 ([明確な代入を決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) によって、変数が確実に代入される場合、変数は***確実に割り当てら***れると言われます。が自動的に初期化されたか、少なくとも1つの割り当ての対象になっています。 非公式に言うと、明確な割り当ての規則は次のとおりです。

*  最初に割り当てられた変数 ([最初に割り当てられた変数](variables.md#initially-assigned-variables)) は、常に代入されていると見なされます。
*  最初に割り当てられていない変数 ([最初に割り当て](variables.md#initially-unassigned-variables)られていない変数) は、その場所を指すすべての実行パスに次のうち少なくとも1つが含まれている場合、特定の場所で確実に割り当てられたと見なされます。
    * 単純な代入 ([単純な代入](expressions.md#simple-assignment))。変数は左オペランドです。
    * 出力パラメーターとして変数を渡す呼び出し式 ([呼び出し式](expressions.md#invocation-expressions)) またはオブジェクト作成式 ([オブジェクト作成式](expressions.md#object-creation-expressions))。
    * ローカル変数の場合は、変数初期化子を含むローカル変数宣言 ([ローカル変数宣言](statements.md#local-variable-declarations))。

上記の非公式な規則の基になる正式な仕様については、[最初に割り当てら](variables.md#initially-assigned-variables)れた変数、[初期未](variables.md#initially-unassigned-variables)割り当ての変数、[明確な割り当てを決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)について説明します。

*Struct_type*変数のインスタンス変数の明示的な割り当ての状態は、まとめて、個別に追跡されます。 上記の規則に加えて、 *struct_type*変数とそのインスタンス変数には次の規則が適用されます。

*  インスタンス変数は、含まれている*struct_type*変数が確実に代入されたと見なされる場合、確実に割り当てられたと見なされます。
*  *Struct_type*変数は、各インスタンス変数が確実に割り当てられたと見なされる場合、確実に割り当てられたと見なされます。

明確な代入は、次のコンテキストの要件です。

*  変数は、値が取得される場所ごとに確実に割り当てられる必要があります。 これにより、未定義の値が発生することはありません。 式での変数の出現は、の場合を除き、変数の値を取得すると見なされます。
    * 変数は、単純な代入の左オペランドです。
    * 変数は、出力パラメーターとして渡されます。
    * 変数は*struct_type*変数で、メンバーアクセスの左オペランドとして発生します。
*  変数は、参照パラメーターとして渡される場所ごとに確実に代入する必要があります。 これにより、呼び出される関数メンバーは最初に割り当てられた参照パラメーターを考慮することができます。
*  関数メンバーのすべての出力パラメーターは、関数メンバーがを返す各場所 (ステートメントを`return`使用するか、関数メンバー本体の末尾に到達するまで) に確実に代入する必要があります。 これにより、関数メンバーが出力パラメーターで未定義の値を返さないようにすることができます。これにより、コンパイラは、変数への代入と同等の出力パラメーターとして変数を受け取る関数メンバーの呼び出しを考慮することができます。
*  Struct_type `this` instance コンストラクターの変数は、そのインスタンスコンストラクターが返す各場所で、確実に割り当てられる必要があります。

### <a name="initially-assigned-variables"></a>最初に割り当てられた変数

次のカテゴリの変数が最初に割り当てられたものとして分類されます。

*  静的変数
*  クラスインスタンスのインスタンス変数。
*  最初に割り当てられた構造体変数のインスタンス変数。
*  配列要素。
*  値パラメーター。
*  参照パラメーター。
*  `catch` 句`foreach`またはステートメントで宣言された変数。

### <a name="initially-unassigned-variables"></a>初期未割り当ての変数

次のカテゴリの変数は、最初に未割り当てとして分類されます。

*  初期割り当てられていない構造体変数のインスタンス変数。
*  出力パラメーター (構造体`this`インスタンスコンストラクターの変数を含む)。
*  ローカル変数 ( `catch`句`foreach`またはステートメントで宣言されているものを除く)。

### <a name="precise-rules-for-determining-definite-assignment"></a>明確な代入を決定するための正確な規則

使用される変数が確実に代入されることを判断するために、コンパイラは、このセクションで説明したものと同等のプロセスを使用する必要があります。

コンパイラは、最初に割り当てられていない変数を1つ以上持つ各関数メンバーの本体を処理します。 最初に割り当てられていない変数*v*について、コンパイラは、関数メンバーの次の各点で*v*の***確実な割り当て状態***を判断します。

*  各ステートメントの先頭にある
*  各ステートメントのエンドポイント ([エンドポイントと到達可能性](statements.md#end-points-and-reachability))
*  別のステートメントまたはステートメントの終了位置に制御を転送する各弧
*  各式の先頭にある
*  各式の最後

*V*の明確な割り当ての状態は、次のいずれかになります。

*  確実に割り当てられます。 これは、可能なすべての制御フローで、 *v*に値が割り当てられていることを示します。
*  確実には割り当てられません。 型`bool`の式の最後にある変数の状態では、明示的に代入されていない変数の状態は、次のいずれかのサブ状態に分類される場合があります。
    * True 式の後に確実に割り当てられます。 この状態は、ブール式が true と評価された場合に*v*が確実に代入されることを示しますが、ブール式が false と評価された場合には必ずしも割り当てられるとは限りません。
    * False 式の後に確実に割り当てられます。 この状態は、ブール式が false と評価された場合に*v*が確実に割り当てられることを示しますが、ブール式が true と評価された場合に必ずしも割り当てられるとは限りません。

次の規則は、変数*v*の状態が各場所でどのように決定されるかを制御します。

#### <a name="general-rules-for-statements"></a>ステートメントの一般的な規則

*  *v*は、関数メンバー本体の先頭では確実に割り当てられません。
*  *v*は、到達できないステートメントの先頭に確実に割り当てられます。
*  他のステートメントの先頭にある*v*の明確な割り当ての状態は、そのステートメントの先頭を対象とするすべての制御フロー転送で*v*の確実な割り当て状態を確認することによって決定されます。 このようなすべての制御フロー転送で*v*が確実に割り当てられている場合は、ステートメントの先頭に*v*が確実に割り当てられます。 可能な制御フロー転送のセットは、ステートメントの到達可能性 ([エンドポイントと到達可能性](statements.md#end-points-and-reachability)) をチェックするのと同じ方法で決定されます。
*  ブロック`using` `lock` `foreach` `checked`の終点`for`での v の明確な割り当ての状態は、、、、、、、、、、またはです。 `unchecked` `if` `while` `do` `switch`ステートメントは、そのステートメントのエンドポイントを対象とするすべての制御フロー転送で*v*の確実な割り当て状態を確認することによって決定されます。 このようなすべての制御フロー転送で*v*が確実に割り当てられている場合は、ステートメントの終了時に*v*が確実に割り当てられます。 それ以外*v*は、ステートメントのエンドポイントでは確実に割り当てられません。 可能な制御フロー転送のセットは、ステートメントの到達可能性 ([エンドポイントと到達可能性](statements.md#end-points-and-reachability)) をチェックするのと同じ方法で決定されます。

#### <a name="block-statements-checked-and-unchecked-statements"></a>ブロックステートメント、checked、および unchecked ステートメント

ブロック内のステートメントリストの最初のステートメント (または、ステートメントリストが空の場合は、ブロックの終了点) への、制御転送での*v*の確実な割り当て状態は、ブロックの前の*v*の明確な代入ステートメントと同じです。、 `checked`、また`unchecked`はステートメント。

#### <a name="expression-statements"></a>式ステートメント

式ステートメントの*stmt*には、式*expr*で構成されます。

*  *v*は、 *stmt*の先頭にある*expr*の先頭で同じ明確な割り当て状態を持ちます。
*  *Expr*の終わりに*v*が明示的に割り当てられている場合は、 *stmt*の終点に確実に代入されます。それ以外これは、 *stmt*のエンドポイントでは確実に割り当てられません。

#### <a name="declaration-statements"></a>宣言ステートメント

*  *Stmt*が初期化子を含まない宣言ステートメントである場合、 *v*は stmt の先頭にある*stmt*の終了時点で同じ明確な割り当て状態に*なります。*
*  *Stmt*が初期化子を持つ宣言ステートメントである場合、 *v*の明確な割り当ての状態は、 *stmt*がステートメントリストであるかのように決定されます。これには、初期化子を持つ宣言ごとに1つの代入ステートメントがあります (宣言)。

#### <a name="if-statements"></a>If ステートメント

次の形式のステートメントのstmtの`if`場合:
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v*は、 *stmt*の先頭にある*expr*の先頭で同じ明確な割り当て状態を持ちます。
*  *Expr*の最後に*v*が明示的に割り当てられている場合、 *then_stmt*に制御フロー転送が割り当てられ、else 句が存在しない場合は、 *else_stmt*または*stmt*の終了ポイントのいずれかに確実に代入されます。
*  *Expr*の終わりに "true 式の後に明示的に代入されました" という状態が*v*にある場合は、制御フロー転送で*then_stmt*に確実に割り当てられ、else_ への制御フロー転送では確実に割り当てられません。else 句が*ない場合は、stmt の*終了位置に stmt またはを指定します。
*  *Expr*の終わりに、 *v*が "false 式の後に確実に代入されました" という状態がある場合は、 *else_stmt*への制御フロー転送で確実に割り当てられ、then_stmt への制御フロー転送では確実に割り当てられません。. これは、 *then_stmt*のエンドポイントで確実に割り当てられている場合にのみ、 *stmt*のエンドポイントで確実に割り当てられます。
*  それ以外の場合、 *v*は、制御フロー転送で*then_stmt*または*else_stmt*のいずれかに割り当てられているとは限りません。また、else 句がない場合は、 *stmt*のエンドポイントにも代入されません。

#### <a name="switch-statements"></a>Switch ステートメント

制御式`switch` *expr*を持つステートメントの*stmt*で、次のように指定します。

*  *Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭の*v*の状態と同じです。
*  到達可能なスイッチブロックステートメントリストへの制御フロー転送の*v*の明示的な割り当て状態は、 *expr*の最後に*v*を指定した場合と同じです。

#### <a name="while-statements"></a>While ステートメント

次の形式のステートメントのstmtの`while`場合:
```csharp
while ( expr ) while_body
```

*  *v*は、 *stmt*の先頭にある*expr*の先頭で同じ明確な割り当て状態を持ちます。
*  *Expr*の最後に*v*が明示的に割り当てられている場合、明示的には制御フロー転送で*while_body*に割り当てられ、終了ポイントは*stmt*に割り当てられます。
*  *Expr*の終わりに "true 式の後に明示的に代入されました" という状態*がある場合*、明示的に割り当てられますが、 *while_body*への制御フロー転送は確実に代入されますが、 *stmt*のエンドポイントでは確実に割り当てられません。
*  *Expr*の終わりに*v*が "false 式の後に確実に代入されました" という状態がある場合は、明示的に制御フロー転送で*stmt*の終了ポイントに割り当てられますが、その間の制御フロー転送では確実に割り当てられません。 *本文 (_c)*

#### <a name="do-statements"></a>Do ステートメント

次の形式のステートメントのstmtの`do`場合:
```csharp
do do_body while ( expr ) ;
```

*  *v*は、stmt の先頭から*do_body*への制御フロー転送において、 *stmt*の先頭と同じ*ように明確*な割り当て状態になります。
*  *v*は、 *do_body*の終点と同様に、 *expr*の先頭に同じ明確な割り当て状態を持ちます。
*  *Expr*の最後に*v*が明示的に割り当てられている場合は、制御フロー転送で*stmt*の終点に確実に代入されます。
*  *Expr*の最後に、"false 式の後に確実に代入されました" という状態が*v*にある場合は、明示的に制御フロー転送で*stmt*の終点に割り当てられます。

#### <a name="for-statements"></a>For ステートメント

次の形式のステートメント`for`に対して明確な代入をチェックします。
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
は、ステートメントが書き込まれたかのように実行されます。
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

*For_condition* `for`がステートメントから省略されている場合は、上記の展開で*for_condition*がに`true`置き換えられたかのように、明確な割り当ての評価が行われます。

#### <a name="break-continue-and-goto-statements"></a>Break、continue、および goto ステートメント

、 `break` 、または`goto`ステートメントによって発生する制御フロー転送の v の明確な割り当て状態は、ステートメントの先頭にある v の明確な代入の状態と同じです。 `continue`

#### <a name="throw-statements"></a>Throw ステートメント

フォームのステートメントの*stmt*の場合
```csharp
throw expr ;
```

*Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭にある*v*の明確な割り当て状態と同じです。

#### <a name="return-statements"></a>Return ステートメント

フォームのステートメントの*stmt*の場合
```csharp
return expr ;
```

*  *Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭にある*v*の明確な割り当て状態と同じです。
*  *V*が出力パラメーターの場合は、次のいずれかが確実に割り当てられている必要があります。
    * after *expr*
    * また`finally`は、 `try` ステートメントを`return`囲むまたは`try` のブロック-の末尾にあります。 `catch` - `finally` - `finally`

次の形式のステートメントの stmt の場合:
```csharp
return ;
```

*  *V*が出力パラメーターの場合は、次のいずれかが確実に割り当てられている必要があります。
    * *stmt*の前
    * また`finally`は、 `try` ステートメントを`return`囲むまたは`try` のブロック-の末尾にあります。 `catch` - `finally` - `finally`

#### <a name="try-catch-statements"></a>Try-catch ステートメント

次の形式のステートメントの*stmt*の場合:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  *Try_block*の開始時の*v*の明確な割り当て状態は、 *stmt*の開始時の*v*の明確な割り当て状態と同じです。
*  *Catch_block_i*の開始時の*v*の明確な割り当ての状態 (任意の*i*) は、 *stmt*の先頭にある*v*の明確な割り当て状態と同じです。
*  *Try_block*のエンドポイント*で v が*確実に割り当てられている場合は、 *stmt*のエンドポイントでの*v*の明確な割り当て状態が確実に割り当てられます (1 から n まで*のすべての* *catch_block_i* )。).

#### <a name="try-finally-statements"></a>Try-finally ステートメント

次の形式のステートメントのstmtの`try`場合:
```csharp
try try_block finally finally_block
```

*  *Try_block*の開始時の*v*の明確な割り当て状態は、 *stmt*の開始時の*v*の明確な割り当て状態と同じです。
*  *Finally_block*の開始時の*v*の明確な割り当て状態は、 *stmt*の開始時の*v*の明確な割り当て状態と同じです。
*  次のいずれかが true の場合にのみ (およびの場合のみ)、 *stmt*のエンドポイントでの*v*の明確な割り当て状態が確実に割り当てられます。
    * *v*は*try_block*のエンドポイントで確実に割り当てられます。
    * *v*は*finally_block*のエンドポイントで確実に割り当てられます。

制御フロー `goto`転送 (ステートメントなど) が*try_block*内で開始され、 *try_block*の外部で終了した場合、v がである場合は、その制御フロー転送で*v*が明示的に割り当てられていると見なされます。*finally_block*のエンドポイントで確実に割り当てられます。 (これは、この制御フロー転送の別の理由で*v*が確実に割り当てられている場合にだけではありません)。

#### <a name="try-catch-finally-statements"></a>Try-catch ステートメント

次の形式の`try` `catch` - ステートメント`finally`の-明確な代入分析。
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
ステートメントがステートメントを囲む`try` `finally` `try` - ステートメント-であるかのように実行されます。 `catch`
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

次の例では、 `try`ステートメントのさまざまなブロック ([try ステートメント](statements.md#the-try-statement)) が確実な代入にどのように影響するかを示します。
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

#### <a name="foreach-statements"></a>Foreach ステートメント

次の形式のステートメントのstmtの`foreach`場合:
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  *Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭の*v*の状態と同じです。
*  *Embedded_statement*または*stmt*の終点に対する制御フロー転送の*v*の明確な割り当て状態は、 *expr*の最後の*v*の状態と同じです。

#### <a name="using-statements"></a>ステートメントの使用

次の形式のステートメントのstmtの`using`場合:
```csharp
using ( resource_acquisition ) embedded_statement
```

*  *Resource_acquisition*の開始時の*v*の明確な割り当て状態は、 *stmt*の先頭の*v*の状態と同じです。
*  *Embedded_statement*への制御フロー転送での*v*の明確な割り当て状態は、 *resource_acquisition*の最後の*v*の状態と同じです。

#### <a name="lock-statements"></a>Lock ステートメント

次の形式のステートメントのstmtの`lock`場合:
```csharp
lock ( expr ) embedded_statement
```

*  *Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭の*v*の状態と同じです。
*  *Embedded_statement*への制御フロー転送での*v*の明確な割り当て状態は、 *expr*の最後の*v*の状態と同じです。

#### <a name="yield-statements"></a>Yield ステートメント

次の形式のステートメントのstmtの`yield return`場合:
```csharp
yield return expr ;
```

*  *Expr*の先頭での*v*の明確な割り当ての状態は、 *stmt*の先頭の*v*の状態と同じです。
*  *Stmt*の終了時の*v*の明確な割り当て状態は、 *expr*の最後の*v*の状態と同じです。
*  ステートメント`yield break`は、確実な割り当て状態には影響しません。

#### <a name="general-rules-for-simple-expressions"></a>単純式の一般的な規則

リテラル ([リテラル](expressions.md#literals))、単純な名前 ([簡易名](expressions.md#simple-names))、メンバーアクセス式 ([メンバーアクセス](expressions.md#member-access))、インデックス付けされていないベースアクセス式 ([ベースアクセス](expressions.md#base-access)) `typeof`には、次の規則が適用されます。式 ([typeof 演算子](expressions.md#the-typeof-operator))、既定値の式 ([既定値の式](expressions.md#default-value-expressions)) `nameof` 、および[式 (値の式)](expressions.md#nameof-expressions)。

*  このような式の最後にある*v*の明確な割り当ての状態は、式の先頭にある*v*の明確な代入の状態と同じです。

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>埋め込み式を使用した式の一般的な規則

次の規則は、かっこで囲まれた式 ([かっこで囲ま](expressions.md#parenthesized-expressions)れた式)、要素アクセス式 ([要素アクセス](expressions.md#element-access))、インデックスを使用した基本アクセス式 ([ベースアクセス](expressions.md#base-access))、増分、およびの各式に適用されます。デクリメント式 ([後置インクリメントおよびデクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、[前置インクリメント演算子とデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))、キャスト式`~`([キャスト式](expressions.md#cast-expressions)) `+`、 `-`単項`*`演算子、式、バイナリ`+` `-` 、`*`、 `/` 、、`%`、、、、、、、 `<<` `>>` `<` `<=` `>` `>=``==` 、`!=`、 、`as`、 、、`^`式([算術演算子](expressions.md#arithmetic-operators)、[シフト演算子](expressions.md#shift-operators)、関係、および型のテスト) `|` `&` `is` [演算子](expressions.md#relational-and-type-testing-operators)、[論理演算子](expressions.md#logical-operators))、複合代入式 ([複合代入](expressions.md#compound-assignment))、 `checked`および`unchecked`式 ([checked および unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) に加えて、配列とデリゲート作成式 ([新しい演算子](expressions.md#the-new-operator))。

これらの各式には、無条件で決まった順序で評価される1つ以上のサブ式があります。 たとえば、二項`%`演算子は演算子の左辺、次に右辺を評価します。 インデックス作成操作によって、インデックス付きの式が評価され、左から右に順番に各インデックス式が評価されます。 式の*expr*では、サブ式*e1, e2,..., eN*がこの順序で評価されます。

*  *E1*の開始時の*v*の明確な割り当ての状態は、 *expr*の先頭での確定代入の状態と同じです。
*  *Ei*の先頭における*v*の明確な割り当ての状態 (*i*が1を超える) は、前のサブ式の最後にある明確な代入の状態と同じです。
*  *Expr*の最後における*v*の明確な割り当ての状態は、 *eN*の終わりにある明確な代入の状態と同じです。

#### <a name="invocation-expressions-and-object-creation-expressions"></a>呼び出し式とオブジェクト作成式

次の形式の呼び出し式の*expr* 。
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
または、次の形式のオブジェクト作成式。
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  呼び出し式の場合、 *primary_expression*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の状態と同じです。
*  呼び出し式の場合、 *arg1*の前の*v*の明確な割り当て状態は、 *primary_expression*後の*v*の状態と同じになります。
*  オブジェクト作成式の場合、 *arg1*より前の*v*の明確な割り当ての状態は、 *expr*より前の*v*の状態と同じになります。
*  各引数*argi*に対して、 *argi* after の明示*代入の状態*は、正規`ref`表現の規則によって決定`out`され、または修飾子は無視されます。
*  各引数*argi*が1より大きい場合、 *argi*前の*v*の明確な割り当ての状態は、前*の引数の*後の*v*の状態と同じ*になります*。
*  引数のいずれかで変数*v*が`out`引数として渡された場合 (つまり`out v`、形式の引数)、 *v* after *expr*の状態は確実に代入されます。 それ以外*v* after *expr*の状態は、 *argN*の後の*v*の状態と同じです。
*  配列初期化子 ([配列作成式](expressions.md#array-creation-expressions))、オブジェクト初期化子 ([オブジェクト初期化](expressions.md#object-initializers)子)、コレクション初期化子 ([コレクション初期化子](expressions.md#collection-initializers))、および匿名オブジェクト初期化子 ([匿名オブジェクトの作成) の場合式](expressions.md#anonymous-object-creation-expressions)) の場合、明確な割り当ての状態は、これらの構成要素がに関して定義されている展開によって決定されます。

#### <a name="simple-assignment-expressions"></a>単純な代入式

式*expr*の形式`w = expr_rhs`は次のとおりです。

*  *Expr_rhs*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の明確な割り当て状態と同じです。
*  *V* after *expr*の明確な割り当て状態は、次のように決定されます。
   * *W*が*v*と同じ変数の場合、 *v* after *expr*の明確な代入の状態は確実に代入されます。
   * それ以外の場合、割り当てが構造体型のインスタンスコンストラクター内で行われる場合、 *w*がプロパティアクセスである場合、構築されるインスタンスに自動的に実装されたプロパティ*P*が指定され、 *v*が非表示のバッキングフィールドになります。*P*の場合、 *v* after *expr*の明確な割り当て状態は確実に代入されます。
   * それ以外の場合、 *v* after *expr*の明示代入の状態は、 *expr_rhs*後の*v*の明確な代入の状態と同じになります。

#### <a name="-conditional-and-expressions"></a>& & (条件式および式式)

式*expr*の形式`expr_first && expr_second`は次のとおりです。

*  *Expr_first*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の明確な割り当て状態と同じです。
*  *Expr_first*の状態が明示的に割り当てられている場合、または "true 式の後に確実に割り当てら*れた"* 場合は、 *expr_second*より前の*v*の明確な割り当て状態が確実に割り当てられます。 それ以外の場合は、確実に割り当てられません。
*  *V* after *expr*の明確な割り当て状態は、次のように決定されます。
    * *Expr_first* `false`が値を持つ定数式である場合、 *v* after *expr*の明示的な代入の状態は、 *expr_first*の*v*の明示的な代入の状態と同じになります。
    * それ以外の場合、 *expr_first*後の*v*の状態が確実に割り当てられると、 *v* after *expr*の状態は確実に代入されます。
    * それ以外の場合、 *v* after *expr_second*の状態が確実に割り当てられ、 *expr_first*後の状態が "false 式の後に確実に割り当てられました" になると、 *v* *after* *expr*の状態は確実になります。担当.
    * それ以外の場合、 *v* after *expr_second*が明示的に割り当てられているか、"true 式の後に確実に割り当てられました" の場合、 *v* after *expr*の状態は "true expression の後に確実に割り当てられます" になります。
    * それ以外の場合は、 *expr_first*後の*v*の状態が "false expression の後に確実に割り当てられました" で、 *expr_second*が "false *expression* *の後に明示的に割り当てられました" という状態になります。expr*は、"false 式の後に確実に代入されます" です。
    * それ以外の場合、 *v* after *expr*の状態は確実に代入されません。

この例では、
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
変数`i`は、 `if`ステートメントの埋め込みステートメントの1つでは確実に代入されますが、他方には存在しないと見なされます。 `if`メソッド`i` `(i = y)`のステートメントでは、最初の埋め込みステートメントで変数が確実に代入されます。これは、式の実行が常にこの埋め込みステートメントの実行に先行するからです。 `F` これに対して、 `i`変数は、2番目の埋め込みステートメントで`x >= 0`は確実に割り当てられません。 `i`これは、false がテストされ、変数が割り当て解除されるためです。

#### <a name="-conditional-or-expressions"></a>||(条件式または) 式

式*expr*の形式`expr_first || expr_second`は次のとおりです。

*  *Expr_first*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の明確な割り当て状態と同じです。
*  *Expr_first*の状態が明示的に割り当てられている場合、または "false 式の後に確実に割り当てら*れた"* 場合は、 *expr_second*より前の*v*の明確な割り当て状態が確実に割り当てられます。 それ以外の場合は、確実に割り当てられません。
*  *V*の明示代入ステートメントは、*次の方法で決定され*ます。
    * *Expr_first* `true`が値を持つ定数式である場合、 *v* after *expr*の明示的な代入の状態は、 *expr_first*の*v*の明示的な代入の状態と同じになります。
    * それ以外の場合、 *expr_first*後の*v*の状態が確実に割り当てられると、 *v* after *expr*の状態は確実に代入されます。
    * それ以外の場合、 *v* after *expr_second*の状態が確実に割り当てられ、 *expr_first*後の*v*の状態が "true expression の後に確実に割り当てられました" である場合、 *v* after *expr*の状態は確実になります。担当.
    * それ以外の場合、 *v* after *expr_second*の状態が確実に割り当てられているか、"false 式の後に確実に代入されました" の場合、 *v* after *expr*の状態は "false 式の後に確実に割り当てられます" になります。
    * それ以外の場合は、 *expr_first*後の*v*の状態が "true expression の後に確実に割り当てられました" で、 *expr_second*が "true *expression の後*に*明示的に*割り当てられています" という状態になります。は "true 式の後に確実に代入されました" です。
    * それ以外の場合、 *v* after *expr*の状態は確実に代入されません。

この例では、
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
変数`i`は、 `if`ステートメントの埋め込みステートメントの1つでは確実に代入されますが、他方には存在しないと見なされます。 `if`メソッド`i` `(i = y)`のステートメントでは、2番目の embedded ステートメントで変数が確実に代入されます。これは、式の実行が常にこの埋め込みステートメントの実行に先行するためです。 `G` これに対して、 `i`変数は、最初の埋め込みステートメントでは確実`x >= 0`に割り当てられません。これは`i` 、true がテストされている可能性があり、その結果、変数が割り当て解除されるためです。

#### <a name="-logical-negation-expressions"></a>! (論理否定) 式

式*expr*の形式`! expr_operand`は次のとおりです。

*  *Expr_operand*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の明確な割り当て状態と同じです。
*  *V* after *expr*の明確な割り当て状態は、次のように決定されます。
    * \* Expr_operand * の後の*v*の状態が確実に割り当てられている場合、 *v* after *expr*の状態は確実に代入されます。
    * \* Expr_operand * の後の*v*の状態が確実に割り当てられていない場合、 *v* after *expr*の状態は確実に代入されません。
    * \* Expr_operand * 後の*v*の状態が "false 式の後に確実に割り当てられました" の場合、 *v* after *expr*の状態は "true expression の後に確実に割り当てられます" になります。
    * \* Expr_operand * 後の*v*の状態が "true expression の後に確実に割り当てられました" の場合、 *v* after *expr*の状態は、"false 式の後に確実に割り当てられます" になります。

#### <a name="-null-coalescing-expressions"></a>?? (null 合体) 式

式*expr*の形式`expr_first ?? expr_second`は次のとおりです。

*  *Expr_first*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の明確な割り当て状態と同じです。
*  *Expr_second*より前の*v*の明確な割り当ての状態は、 *expr_first*の後の*v*の明確な割り当て状態と同じです。
*  *V*の明示代入ステートメントは、*次の方法で決定され*ます。
    * *Expr_first*が定数式 ([定数式](expressions.md#constant-expressions)) で、値が null の場合、 *v* after *expr*の状態は、 *expr_second*の後の*v*の状態と同じになります。
*  それ以外の場合、 *v* after *expr*の状態は、 *expr_first*の後の*v*の明確な割り当て状態と同じになります。

#### <a name="-conditional-expressions"></a>?: (条件) 式

式*expr*の形式`expr_cond ? expr_true : expr_false`は次のとおりです。

*  *Expr_cond*より前の*v*の明確な割り当て状態は、 *expr*より前の*v*の状態と同じです。
*  次のいずれかに当てはまる場合にのみ、 *expr_true*より前の*v*の明確な割り当て状態が確実に割り当てられます。
    * *expr_cond*は、値を持つ定数式です。`false`
    * *expr_cond*後の*v*の状態は、確実に割り当てられるか、"true 式の後に確実に代入されました" です。
*  次のいずれかに当てはまる場合にのみ、 *expr_false*より前の*v*の明確な割り当て状態が確実に割り当てられます。
    * *expr_cond*は、値を持つ定数式です。`true`
*  *expr_cond*後の*v*の状態は、確実に割り当てられるか、"false 式の後に確実に代入されました" です。
*  *V* after *expr*の明確な割り当て状態は、次のように決定されます。
    * `true` *Expr_cond*が値を持つ定数式 ([定数式](expressions.md#constant-expressions)) である場合、 *v* after *expr*の状態は、 *expr_true*の後の*v*の状態と同じになります。
    * それ以外の場合、 *expr_cond*が値`false`を持つ定数式 ([定数式](expressions.md#constant-expressions)) である場合、 *v* after *expr*の状態は、 *expr_false*の後の*v*の状態と同じになります。
    * それ以外の場合、 *expr_true*後の*v*の状態が確実に割り当てられ、 *expr_false*後の*v*の状態が確実に割り当てられると、 *v* after *expr*の状態は確実に代入されます。
    * それ以外の場合、 *v* after *expr*の状態は確実に代入されません。

#### <a name="anonymous-functions"></a>匿名関数

本文 (*ブロック*または*式*) の*本体*を持つ*lambda_expression*または*anonymous_method_expression* *expr*の場合:

*  *本体*より前の外部変数*v*の明確な割り当ての状態は、 *expr*より前の*v*の状態と同じです。 つまり、外部変数の明確な割り当て状態は、匿名関数のコンテキストから継承されます。
*  外部変数*v*の明示的な代入の状態は、 *expr*より前の*v*の状態と同じ*です。*

例
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
では、匿名関数が宣言`max`されている場所が確実に割り当てられないため、コンパイル時エラーが生成されます。 例
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
では、匿名関数のへ`n`の代入が匿名関数の外部の確実な`n`割り当て状態に影響を与えないため、コンパイル時エラーが生成されます。

## <a name="variable-references"></a>変数参照

*Variable_reference*は、変数として分類される*式*です。 *Variable_reference*は、現在の値を取得し、新しい値を格納するためにアクセスできるストレージの場所を表します。

```antlr
variable_reference
    : expression
    ;
```

C およびC++では、 *variable_reference*は*左辺*値と呼ばれます。

## <a name="atomicity-of-variable-references"></a>変数参照の原子性

`bool`次のデータ型の読み取りと書き込みは`int` `uint` `short` `ushort` `byte`、atomic: `char`、、 `sbyte` 、`float`、、、、、、およびの各参照型です。 また、前の一覧の基になる型を持つ列挙型の読み取りと書き込みもアトミックです。 `long` 、`ulong`、 、`decimal`など、他の型の読み取りと書き込みは、ユーザー定義型と同様にアトミックであるとは限りません。 `double` この目的のために設計されたライブラリ関数とは別に、インクリメントまたはデクリメントの場合など、アトミック読み取り/変更/書き込みの保証はありません。

