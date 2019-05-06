---
ms.openlocfilehash: b7bb7dd575d9e2e6d5dd85bdd3e535411e29fcf4
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229745"
---
# <a name="variables"></a>変数

変数は、記憶域の場所を表します。 すべての変数ができる値を指定する型の変数に格納します。 C# は、タイプ セーフな言語と C# コンパイラでは、変数に格納されている値は常に適切な型のことが保証されます。 割り当てまたはを使用して変数の値を変更できる、`++`と`--`演算子。

変数である必要があります***確実に代入***([確実な代入](variables.md#definite-assignment)) 前に、その値を取得できます。

いずれかの変数は、次のセクションで説明した、***最初に割り当てられた***または***最初に割り当てられていない***します。 最初に割り当てられている変数は、明確に定義された初期値を備え、明示的に代入すると考えられます。 最初に未割り当ての変数には、初期値はありません。 特定の場所に明示的に代入して考慮することを最初に割り当てられていない変数、その位置に至るすべての可能な実行パスで、変数への代入があります。

## <a name="variable-categories"></a>変数のカテゴリ

C# 7 つのカテゴリの変数を定義します。 静的変数、インスタンス変数、配列の要素、パラメーターの値、参照パラメーター、出力パラメーター、およびローカル変数。 次のセクションでは、これらの各カテゴリについて説明します。

例
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
`x` 静的変数、`y`インスタンス変数は、`v[0]`配列要素である`a`値パラメーターでは、`b`参照パラメーターは、`c`は出力パラメーターと`i`はローカル変数です。

### <a name="static-variables"></a>静的変数

宣言されたフィールド、`static`修飾子と呼ばれる、***静的変数***します。 静的変数は、静的コンストラクターの実行前に存在するように、([静的コンストラクター](classes.md#static-constructors)) その親の種類、および関連付けられているアプリケーション ドメインが存在しなくなるときに存在する停止します。

静的変数の初期値は、既定値 ([既定値](variables.md#default-values)) の変数の型。

確実な代入をチェックのために、静的変数は最初に割り当てられていると見なされます。

### <a name="instance-variables"></a>インスタンス変数

なしで宣言されたフィールド、`static`修飾子と呼ばれる、***インスタンス変数***します。

#### <a name="instance-variables-in-classes"></a>クラスのインスタンス変数

クラスのインスタンス変数は、そのクラスの新しいインスタンスが作成され、そのインスタンスへの参照がないと、インスタンスのデストラクター (存在する場合) が実行時に存在しなくなったときに、存在するように得られます。

クラスのインスタンス変数の初期値は、既定値 ([既定値](variables.md#default-values)) の変数の型。

クラスのインスタンス変数は、確実な代入をチェックするために、最初に割り当てられていると見なされます。

#### <a name="instance-variables-in-structs"></a>構造体にインスタンス変数

構造体のインスタンス変数には、正確に同じ有効期間が所属する構造体変数としてがあります。 つまり、構造体の型の変数にまたは場合、存在しなくなりますすぎる構造体のインスタンス変数の操作を行います。

構造体のインスタンス変数の初期の割り当ての状態は、構造体の変数の場合と同じです。 つまり、構造体変数が最初と見なされるタイミング割り当てられると、これがそのインスタンス変数と構造体変数が最初に割り当てられていないと見なされると、そのインスタンス変数が同様に割り当てられています。

### <a name="array-elements"></a>配列の要素

配列の要素は、配列インスタンスの作成時に存在することになるし、その配列インスタンスへの参照がない場合に存在しなくなります。

各配列の要素の初期値は、既定値 ([既定値](variables.md#default-values)) の配列の要素の型。

配列の要素は、確実な代入をチェックするために、最初に割り当てられていると見なされます。

### <a name="value-parameters"></a>値パラメーター

パラメーターなしで宣言された、`ref`または`out`修飾子は、***値パラメーター***。

関数メンバー (メソッド、インスタンス コンストラクター、アクセサー、または演算子) または匿名関数の呼び出し時に存在するように値を持つパラメーターには、パラメーターが属していると、呼び出しで指定された引数の値で初期化されます。 値を持つパラメーターは、通常、関数メンバーまたは匿名関数が戻るときに削除されます。 ただし、値パラメーターが匿名関数によってキャプチャされたかどうか ([匿名関数式](expressions.md#anonymous-function-expressions))、その有効期間をデリゲートには少なくとも拡張または作成の匿名関数式ツリーが対象であります。ガベージ コレクション。

値を持つパラメーターは、確実な代入をチェックするために、最初に割り当てられていると見なされます。

### <a name="reference-parameters"></a>参照パラメーター

宣言したパラメーター、`ref`修飾子は、***パラメーター参照***します。

参照パラメーターは、新しい記憶域の場所を作成できません。 代わりに、参照パラメーターは、関数メンバーまたは匿名関数の呼び出しで引数として指定された変数と同じストレージ場所を表します。 したがって、参照パラメーターの値は常に、基になる変数と同じです。

次の確実な代入ルールは、参照パラメーターに適用されます。 説明されている出力パラメーターのさまざまな規則に注意してください[出力パラメーター](variables.md#output-parameters)します。

*  変数を明示的に代入する必要があります ([確実な代入](variables.md#definite-assignment)) 関数のメンバーまたはデリゲートの呼び出しで参照パラメーターとして渡す前にします。
*  関数メンバーまたは匿名関数の中で参照パラメーターが最初に割り当てられていると見なされます。

インスタンス メソッドまたはインスタンス アクセサー、構造体型の内、`this`キーワードは構造体の型の参照パラメーターとまったく同じ動作 ([このアクセス](expressions.md#this-access))。

### <a name="output-parameters"></a>出力パラメーター

宣言したパラメーター、`out`修飾子は、***出力パラメーター***します。

出力パラメーターは、新しい記憶域の場所を作成できません。 代わりに、出力パラメーターは、関数のメンバーまたはデリゲートの呼び出しで引数として指定された変数と同じストレージ場所を表します。 したがって、出力パラメーターの値は常に、基になる変数と同じです。

次の確実な代入ルールは、出力パラメーターに適用されます。 説明されている参照パラメーターのさまざまな規則に注意してください[パラメーターを参照して](variables.md#reference-parameters)します。

*  変数が必要ないに明示的に代入関数メンバーの出力パラメーターとして渡すことができますが、デリゲートの呼び出しまたは。
*  関数メンバーまたはデリゲートの呼び出しが正常に完了した後、その実行パスと見なされます、出力パラメーターとして渡された各変数が割り当てられます。
*  関数メンバーまたは匿名関数の中で、出力パラメーターが最初に割り当てられていないと見なされます。
*  関数のメンバーまたは匿名関数のすべての出力パラメーターを明示的に代入する必要があります ([確実な代入](variables.md#definite-assignment)) 関数の前にメンバーまたは匿名関数が通常どおり戻る。

構造体の型のインスタンス コンストラクター内で、`this`キーワードは構造体の型の出力パラメーターとまったく同じ動作 ([このアクセス](expressions.md#this-access))。

### <a name="local-variables"></a>ローカル変数

A***ローカル変数***で宣言されて、 *local_variable_declaration*で発生する可能性があります、*ブロック*、 *for_statement*、 *switch_statement*または*using_statement*または、 *foreach_statement*または*specific_catch_clause*をの *。try_statement*します。

ローカル変数の有効期間は、プログラムの実行を予約する記憶域を保証する部分です。 この有効期間を拡張のエントリから、少なくとも、*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*それが関連付けられている、その実行まで*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*任意の方法で終了します。 (入力、囲まれた*ブロック*メソッドを呼び出すが、中断、終わらない、現在の実行または*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*)。匿名関数でローカル変数をキャプチャするかどうか ([外部変数をキャプチャ](expressions.md#captured-outer-variables))、その有効期間に付属するその他のオブジェクトと共に、匿名関数から作成されたデリゲートまたは式ツリーまで少なくとも拡張キャプチャされた変数への参照はガベージ コレクションの対象とします。

場合、親*ブロック*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*、または*specific_catch_clause*入力が再帰的に、ローカル変数の新しいインスタンスのたびが作成し、その*local_variable_initializer*、いずれかが評価される場合、各時間です。

導入されたローカル変数、 *local_variable_declaration*が自動的に初期化されていないと、そのため、既定値がありません。 確実な代入をチェックするためには、ローカル変数がで導入された、 *local_variable_declaration*は最初に割り当てられていないと見なされます。 A *local_variable_declaration*含めることができます、 *local_variable_initializer*、その場合、変数と見なされます明示的に代入、初期化式の後にのみ ([宣言ステートメント](variables.md#declaration-statements))。

導入されたローカル変数のスコープ内で、 *local_variable_declaration*、前にあるテキストの位置でそのローカル変数を参照すると、コンパイル時エラー、 *local_variable_declarator*. ローカル変数宣言に暗黙の型がある場合 ([ローカル変数の宣言](statements.md#local-variable-declarations))、内の変数に参照するとエラーもその*local_variable_declarator*します。

導入されたローカル変数、 *foreach_statement*または*specific_catch_clause*全体のスコープに確実に割り当てられていると見なされます。

ローカル変数の実際の有効期間は、実装によって異なります。 たとえば、コンパイラ可能性があります静的に決定できるブロック内のローカル変数は、そのブロックの小さな部分にのみ使用します。 この分析を使用して、コンパイラはその包含ブロックよりも短い有効期間を持つ変数の記憶域になるコードを生成可能性があります。

ローカル参照変数によって参照されるストレージはローカル参照変数の有効期間とは独立して再利用 ([自動メモリ管理](basic-concepts.md#automatic-memory-management))。

## <a name="default-values"></a>既定の値

次のカテゴリの変数は、既定値に自動的に初期化されます。

*  静的変数
*  クラスのインスタンスのインスタンス変数。
*  配列の要素。

変数の既定値は、変数の型に依存し、次のように決定されます。

*  変数の*value_type*、既定値はによって計算された値と同じ、 *value_type*の既定のコンストラクター ([既定のコンストラクター](types.md#default-constructors))。
*  変数の*reference_type*、既定値は`null`します。

メモリ マネージャーによって既定値に初期化が普通または使用に割り当てられる前に、ガベージ コレクターがすべてのビットが 0 にメモリを初期化します。 このため、null 参照を表すすべてのビットが 0 を使用すると便利です。

## <a name="definite-assignment"></a>確実な代入

関数メンバーの実行可能コード内の指定した位置に変数があると言います***確実に代入***、特定の静的フロー解析でコンパイラを証明できるかどうか ([確実なを決定するための厳密な規則割り当て](variables.md#precise-rules-for-determining-definite-assignment))、変数が自動的に初期化されるまたは少なくとも 1 つの割り当ての対象になっています。 確実な代入ルールは、します非公式説明すると。

*  最初に割り当て済みの変数 ([変数を最初に割り当てられた](variables.md#initially-assigned-variables)) 確実に割り当てられていると考えられます。
*  最初に未割り当ての変数を ([変数を最初に割り当てられていない](variables.md#initially-unassigned-variables)) と見なされます確実に割り当てられている特定の場所にその場所につながるすべての可能な実行パスでは、次の少なくとも 1 つ含まれている場合。
    * 単純な代入 ([単純な代入](expressions.md#simple-assignment)) で、変数は、左側のオペランド。
    * Invocation 式 ([Invocation 式](expressions.md#invocation-expressions)) またはオブジェクトの作成式 ([オブジェクト作成式](expressions.md#object-creation-expressions))、出力パラメーターとして、変数に合格します。
    * ローカル変数、ローカル変数宣言 ([ローカル変数宣言](statements.md#local-variable-declarations)) 変数の初期化子が含まれます。

上記の非公式なルールを基になる正式な仕様については、「[変数最初に割り当てられている](variables.md#initially-assigned-variables)、[変数最初に割り当てられていない](variables.md#initially-unassigned-variables)、および[を決定するための厳密な規則確実な代入](variables.md#precise-rules-for-determining-definite-assignment)します。

インスタンス変数の確実な割り当ての状態、 *struct_type*変数が全体としても個別に追跡されます。 上記の規則に、次の規則にも適用されます*struct_type*変数とそのインスタンス変数。

*  インスタンス変数と見なされます確実に割り当てられている場合はそれを含んでいる*struct_type*変数が確実に割り当てられていると見なされます。
*  A *struct_type*変数と見なされます確実に割り当てられている場合は確実に割り当てられているそれぞれのインスタンス変数と見なされます。

確実な代入では、次のコンテキストでの要件を示します。

*  変数は、その値が取得される各場所では明示的に代入する必要があります。 これにより、未定義の値が発生しません。 式内の変数の出現が場合を除き、変数の値を取得すると見なされます
    * 変数は、単純な割り当ての左オペランドです。
    * 変数が出力パラメーターとして渡されたか、
    * 変数は、 *struct_type*変数とメンバー アクセスの左のオペランドとして発生します。
*  変数は、明示的に参照パラメーターとして渡されますが、それぞれの場所に代入する必要があります。 これにより、呼び出される関数メンバーが最初に割り当てられている参照パラメーターを検討できます。
*  関数メンバーを返しますの各場所で明示的関数メンバーのすべての出力パラメーターに代入する必要があります (を通じて、`return`ステートメントまたは実行関数メンバーの本文の終わりに達するまで)。 これにより、関数メンバー未定義の値に返しません、出力パラメーター、変数への代入と等価の出力パラメーターとして変数を受け取る関数メンバーの呼び出しを考慮するコンパイラが有効にします。
*  `this`の変数を*struct_type*インスタンス コンストラクターをそのコンストラクターを返しますの各場所で間違いなく割り当てる必要があります。

### <a name="initially-assigned-variables"></a>最初に割り当てられた変数

次のカテゴリの変数は、初期割り当てられている分類されます。

*  静的変数
*  クラスのインスタンスのインスタンス変数。
*  最初に割り当てられている構造体の変数のインスタンス変数。
*  配列の要素。
*  値のパラメーター。
*  参照パラメーター。
*  宣言された変数を`catch`句または`foreach`ステートメント。

### <a name="initially-unassigned-variables"></a>最初に割り当てられていない変数

次のカテゴリの変数は、初期代入なしに分類されます。

*  最初に割り当てられていない構造体の変数のインスタンス変数。
*  出力パラメーター、`this`構造体のインスタンス コンストラクターの変数。
*  宣言されたローカル変数を除く、`catch`句または`foreach`ステートメント。

### <a name="precise-rules-for-determining-definite-assignment"></a>確実な代入を決定するための正確な規則

を使用する各変数を明示的に代入を判断するために、コンパイラは、このセクションで説明したように相当するプロセスを使用する必要があります。

コンパイラは、最初に割り当てられていない 1 つまたは複数の変数を持つ各関数メンバーの本文を処理します。 最初に割り当てられていない各変数の*v*、コンパイラが判断を***確実な代入状態***の*v*関数メンバーの次の点の各。

*  各ステートメントの先頭に
*  最後の時点で ([エンドポイントと到達可能性](statements.md#end-points-and-reachability)) の各ステートメント
*  それぞれの円弧の制御を移す別のステートメントまたはステートメントの終了点
*  各式の先頭に
*  各式の最後に

確実な代入状態*v*いずれかになります。

*  明示的に代入します。 この時点ですべての可能な制御フローのことを示しますこれは、 *v*に値が代入されています。
*  間違いなく割り当てられています。 型の式の最後に変数の状態の`bool`、間違いなく月が割り当てられていない (ただし、必ずしも) 変数の状態は、次のサブ状態のいずれかに分類されます。
    * 間違いなく true 式の後に割り当てられます。 この状態では、ことを示します*v*ブール式が true として評価は、ブール式が false と評価された場合とは限りません割り当てられていない場合は確実に代入します。
    * False の式の後に確実に代入します。 この状態では、ことを示します*v*ブール式が false として評価は、ブール式が true と評価された場合とは限りません割り当てられていない場合は確実に代入します。

次の規則が、どのように変数の状態*v*は場所ごとに決定されます。

#### <a name="general-rules-for-statements"></a>ステートメントの一般的な規則

*  *v*関数メンバーの本文の先頭に確実に代入できません。
*  *v*が確実に到達不可能なステートメントの先頭に代入します。
*  確実な代入状態*v*の確実な割り当ての状態をチェックして、他のステートメントの先頭に決定されます*v*の先頭を対象とするすべてのコントロール フロー転送ステートメント。 場合 (および場合にのみ) *v*し、割り当て、このようなすべてのコントロールのフローの転送に間違いなく*v*が確実に割り当て、ステートメントの先頭にします。 ステートメントの到達可能性のチェックと同じ方法で移動する可能性のある制御フローのセットが決定されます ([エンドポイントと到達可能性](statements.md#end-points-and-reachability))。
*  確実な代入状態*v*ブロックの終了点で`checked`、 `unchecked`、 `if`、 `while`、 `do`、 `for`、 `foreach`、 `lock`、 `using`、または`switch`の確実な割り当ての状態を確認してステートメントが決定されます*v*をそのステートメントの終点を対象とするすべてのコントロール フロー転送します。 場合*v*が確実に割り当て、このようなすべてのコントロールのフローの転送で、 *v*が確実に割り当て、ステートメントの終了時点。 それ以外の場合。*v*ステートメントの終了時点で確実に代入できません。 ステートメントの到達可能性のチェックと同じ方法で移動する可能性のある制御フローのセットが決定されます ([エンドポイントと到達可能性](statements.md#end-points-and-reachability))。

#### <a name="block-statements-checked-and-unchecked-statements"></a>オンになっている、ブロックのステートメントと unchecked ステートメント

確実な代入状態*v*ブロックにおけるステートメント リストの最初のステートメント (またはステートメントの一覧が空の場合は、ブロックの終了点) の転送では、コントロールがの確実な代入ステートメントと同じ*v* 、ブロックの前に`checked`、または`unchecked`ステートメント。

#### <a name="expression-statements"></a>式ステートメント

式ステートメントの*stmt*式で構成される*expr*:

*  *v*の先頭に同じ状態の確実な代入*expr*の先頭として*stmt*します。
*  場合*v*の最後に確実に割り当てられている場合*expr*、間違いなくの終了ポイントに割り当てられている*stmt*それの終点で間違いなく割り当てがありません*stmt*します。

#### <a name="declaration-statements"></a>宣言ステートメント

*  場合*stmt*宣言のステートメントは、初期化子のない、 *v*の終了時点で確実な代入状態の同じ*stmt* の先頭として*stmt*します。
*  場合*stmt*と初期化子、宣言ステートメントは次の明確な割り当ての状態は、 *v*決定場合と*stmt*が 1 つの割り当てと、ステートメントの一覧(宣言) の順序で初期化子による各宣言ステートメントです。

#### <a name="if-statements"></a>場合ステートメント

`if`ステートメント*stmt*の形式。
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v*の先頭に同じ状態の確実な代入*expr*の先頭として*stmt*します。
*  場合*v*の最後に明示的に代入が*expr*、確実に制御フローの移動で割り当てられているし、 *then_stmt*とするか、 *else_stmt*またはのエンドポイントに*stmt* else 句がない場合。
*  場合*v*の末尾に「間違いなく true 式の後に割り当て済み」の状態を持つ*expr*、確実に制御フローの移動で割り当てられているし、 *then_stmt*、および not間違いなくいずれかを制御フローの移動で割り当てられている*else_stmt*またはのエンドポイントに*stmt* else 句がない場合。
*  場合*v*の末尾に「を代入式が false の後に」の状態を持つ*expr*、確実に制御フローの移動で割り当てられているし、 *else_stmt*、および not制御フローの移動の代入*then_stmt*します。 間違いなくのエンドポイントに割り当てられている*stmt*間違いなくのエンドポイントに割り当てられている場合にのみ*then_stmt*します。
*  それ以外の場合、 *v*はいないと見なされるいずれかを制御フローの転送を確実に割り当てられた、 *then_stmt*または*else_stmt*、またはのエンドポイントに*stmt* else 句がない場合。

#### <a name="switch-statements"></a>Switch ステートメント

`switch`ステートメント*stmt*制御式と*expr*:

*  確実な代入状態*v*の先頭に*expr*の状態と同じでは、 *v*の先頭に*stmt*します。
*  確実な代入状態*v*到達可能なスイッチ ブロック ステートメントの一覧への転送では、制御フローでが確実な割り当ての状態のと同じ*v*の最後に*expr*.

#### <a name="while-statements"></a>While ステートメント

`while`ステートメント*stmt*の形式。
```csharp
while ( expr ) while_body
```

*  *v*の先頭に同じ状態の確実な代入*expr*の先頭として*stmt*します。
*  場合*v*の最後に明示的に代入が*expr*、確実に制御フローの移動で割り当てられているし、 *while_body*と終点を*stmt*します。
*  場合*v*の末尾に「間違いなく true 式の後に割り当て済み」の状態を持つ*expr*、確実に制御フローの移動で割り当てられているし、 *while_body*がありませんエンドポイントで確実に代入*stmt*します。
*  場合*v*の末尾に「を代入式が false の後に」の状態を持つ*expr*、間違いなくの終了点への制御フローの移動で割り当てられているし、 *stmt*、への制御フローの移動では間違いなく割り当てられていませんが、 *while_body*します。

#### <a name="do-statements"></a>Do ステートメント

`do`ステートメント*stmt*の形式。
```csharp
do do_body while ( expr ) ;
```

*  *v*の先頭からの制御フローの移動で同じ状態の確実な代入*stmt*に*do_body*の先頭として*stmt*します。
*  *v*の先頭に同じ状態の確実な代入*expr*の終了時点として*do_body*します。
*  場合*v*の最後に明示的に代入が*expr*、間違いなくの終了点への制御フローの移動で割り当てられているし、 *stmt*します。
*  場合*v*の末尾に「を代入式が false の後に」の状態を持つ*expr*、間違いなくの終了点への制御フローの移動で割り当てられているし、 *stmt*.

#### <a name="for-statements"></a>ステートメント

確実な代入のチェック、`for`形式のステートメント。
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
ステートメントが記述された場合とが行われます。
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

場合、 *for_condition*から省略すると、`for`ステートメント、し、確実な代入処理の進行状況の評価として*for_condition*に置き換えられました`true`で上記の拡張.

#### <a name="break-continue-and-goto-statements"></a>中断、続行、および goto ステートメント

確実な代入状態*v*による制御フローの転送、 `break`、 `continue`、または`goto`ステートメントが確実な割り当ての状態のと同じ*v*で、ステートメントの先頭。

#### <a name="throw-statements"></a>Throw ステートメント

ステートメントの*stmt*のフォーム
```csharp
throw expr ;
```

確実な代入状態*v*の先頭に*expr*の確実な割り当ての状態と同じ*v*の先頭に*stmt*.

#### <a name="return-statements"></a>Return ステートメント

ステートメントの*stmt*のフォーム
```csharp
return expr ;
```

*  確実な代入状態*v*の先頭に*expr*の確実な割り当ての状態と同じ*v*の先頭に*stmt*.
*  場合*v*が出力パラメーターでは、間違いなく割り当てる必要がありますか。
    * 後*expr*
    * またはの最後に、`finally`のブロックを`try` - `finally`または`try` - `catch` - `finally`を囲む、`return`ステートメント。

形式のステートメント stmt:
```csharp
return ;
```

*  場合*v*が出力パラメーターでは、間違いなく割り当てる必要がありますか。
    * 前に*stmt*
    * またはの最後に、`finally`のブロックを`try` - `finally`または`try` - `catch` - `finally`を囲む、`return`ステートメント。

#### <a name="try-catch-statements"></a>Try-catch ステートメント

ステートメントの*stmt*の形式。
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  確実な代入状態*v*の先頭に*try_block*の確実な割り当ての状態と同じ*v*の先頭に*stmt*.
*  確実な代入状態*v*の先頭に*catch_block_i* (いずれかの*は*) の明確な割り当ての状態と同じ*v*の先頭に*stmt*します。
*  確実な代入状態*v*のエンドポイントで*stmt*は確実に割り当てられている場合 (および場合にのみ) *v*のエンドポイントで確実に代入が*try_block* 、毎回*catch_block_i* (のすべて*は*1 ~ *n*)。

#### <a name="try-finally-statements"></a>Try finally ステートメント

`try`ステートメント*stmt*の形式。
```csharp
try try_block finally finally_block
```

*  確実な代入状態*v*の先頭に*try_block*の確実な割り当ての状態と同じ*v*の先頭に*stmt*.
*  確実な代入状態*v*の先頭に*finally_block*の確実な割り当ての状態と同じ*v*の先頭に*stmt*.
*  確実な代入状態*v*のエンドポイントで*stmt*は確実に割り当てられている場合 (および場合にのみ)、次の少なくとも 1 つが true:
    * *v*のエンドポイントで確実に代入が*try_block*
    * *v*のエンドポイントで確実に代入が*finally_block*

制御フローの転送の場合 (など、`goto`ステートメント) が行われた内で開始*try_block*、以外の終了と*try_block*、し*v*も場合、その制御フローの転送を確実に割り当てられていると見なされます*v*のエンドポイントで確実に代入が*finally_block*します。 (これは、場合にのみではありません — 場合*v*が間違いなくこの制御フローの転送時に別の理由を割り当てられて明示的に代入みなされますし、)。

#### <a name="try-catch-finally-statements"></a>Try – catch – finally ステートメント

分析を確実な代入を`try` - `catch` - `finally`形式のステートメント。
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
場合、ステートメントが同じように行われますが、 `try` - `finally`ステートメントを囲む、 `try` - `catch`ステートメント。
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

次の例で方法の異なるブロック、`try`ステートメント ([try ステートメント](statements.md#the-try-statement)) に影響する確実な代入です。
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

`foreach`ステートメント*stmt*の形式。
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  確実な代入状態*v*の先頭に*expr*の状態と同じでは、 *v*の先頭に*stmt*します。
*  確実な代入状態*v*への制御フローの移動で*embedded_statement*またはの終点を*stmt*の状態と同じでは、 *v*の最後に*expr*します。

#### <a name="using-statements"></a>ステートメントを使用します。

`using`ステートメント*stmt*の形式。
```csharp
using ( resource_acquisition ) embedded_statement
```

*  確実な代入状態*v*の先頭に*resource_acquisition*の状態と同じでは、 *v*の先頭に*stmt*.
*  確実な代入状態*v*への制御フローの移動で*embedded_statement*の状態と同じでは、 *v*の最後に*resource_買収*します。

#### <a name="lock-statements"></a>Lock ステートメント

`lock`ステートメント*stmt*の形式。
```csharp
lock ( expr ) embedded_statement
```

*  確実な代入状態*v*の先頭に*expr*の状態と同じでは、 *v*の先頭に*stmt*します。
*  確実な代入状態*v*への制御フローの移動で*embedded_statement*の状態と同じでは、 *v*の最後に*expr*.

#### <a name="yield-statements"></a>Yield ステートメント

`yield return`ステートメント*stmt*の形式。
```csharp
yield return expr ;
```

*  確実な代入状態*v*の先頭に*expr*の状態と同じでは、 *v*の先頭に*stmt*します。
*  確実な代入状態*v*の最後に*stmt*の状態と同じでは、 *v*の最後に*expr*します。
*  A`yield break`ステートメントが確実な代入の状態に影響を与えません。

#### <a name="general-rules-for-simple-expressions"></a>単純式での一般的な規則

これらの種類の式に、次の規則が適用されますリテラル ([リテラル](expressions.md#literals))、単純な名前 ([簡易名](expressions.md#simple-names))、メンバー アクセス式 ([メンバー アクセス](expressions.md#member-access))、。インデックスのないベース アクセス式 ([Base アクセス](expressions.md#base-access))、`typeof`式 ([typeof 演算子](expressions.md#the-typeof-operator))、既定の値式 ([既定の値式](expressions.md#default-value-expressions)) と`nameof`式 ([Nameof 式](expressions.md#nameof-expressions))。

*  確実な代入状態*v*はの確実な割り当ての状態と同じような式の末尾に*v*式の先頭にします。

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>埋め込み式を持つ式の一般的な規則

これらの種類の式に、次の規則が適用されます: かっこで囲まれた式 ([かっこで囲まれた式](expressions.md#parenthesized-expressions))、要素アクセス式 ([要素へのアクセス](expressions.md#element-access))、基本アクセスを含む式インデックスの作成 ([Base アクセス](expressions.md#base-access))、インクリメントおよびデクリメント式 ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、[前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))、キャスト式 ([キャスト式](expressions.md#cast-expressions))、単項`+`、 `-`、 `~`、`*`式、バイナリ`+`、 `-`、 `*`、`/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`、 `|`、`^`式 ([算術演算子](expressions.md#arithmetic-operators)、[シフト演算子](expressions.md#shift-operators)、[関係式と型検査演算子](expressions.md#relational-and-type-testing-operators)、[論理演算子](expressions.md#logical-operators))、複合代入式 ([複合代入](expressions.md#compound-assignment))、`checked`と`unchecked`式 ([checked と unchecked演算子](expressions.md#the-checked-and-unchecked-operators))、さらに配列とデリゲート作成式 ([new 演算子](expressions.md#the-new-operator))。

それぞれの式は、無条件に一定の順序で評価される 1 つまたは複数のサブ式があります。 たとえば、バイナリ`%`演算子を評価し、演算子の左側、右側にあります。 インデックス作成操作では、インデックス付きの式を評価しの各インデックス式、順序は左から右に評価し、します。 式の*expr*、サブ式を持つ*e1、e2、…、eN*、その順序で評価されます。

*  確実な代入状態*v*の先頭に*e1*の先頭に明確な割り当ての状態と同じ*expr*します。
*  確実な代入状態*v*の先頭に*ei* (*は*1 より大きい) 以前のサブ式の末尾に明確な割り当ての状態の場合と同じです。
*  確実な代入状態*v*の最後に*expr*の最後に明確な割り当ての状態と同じ*eN*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Invocation 式とオブジェクト作成式

Invocation 式の*expr*の形式。
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
または、フォームのオブジェクト作成式:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Invocation 式での確実な割り当ての状態の*v*する前に*primary_expression*の状態と同じでは、 *v*する前に*expr*.
*  Invocation 式での確実な割り当ての状態の*v*する前に*arg1*の状態と同じでは、 *v*後*primary_expression*.
*  オブジェクト作成式での確実な割り当ての状態を*v*する前に*arg1*の状態と同じでは、 *v*する前に*expr*します。
*  各引数の*argi*の確実な代入状態*v*後*argi*は無視して、通常の式の規則によって決まります`ref`または`out`修飾子。
*  各引数の*argi*は*は*1 の確実な割り当ての状態よりも大きい*v*する前に*argi*の状態の場合と同じです*v*前後*arg*します。
*  場合、変数*v*として渡される、`out`引数 (つまり、フォームの引数`out v`)、引数は、次の状態のいずれかで*v*後*expr*間違いなく割り当てられます。 それ以外の場合。状態*v*後*expr*の状態と同じでは、 *v*後*argN*します。
*  配列初期化子の ([配列作成式](expressions.md#array-creation-expressions))、オブジェクト初期化子 ([オブジェクト初期化子](expressions.md#object-initializers))、コレクション初期化子 ([コレクション初期化子](expressions.md#collection-initializers)) と匿名オブジェクト初期化子 ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions))、これらのコンストラクトがの観点で定義されている拡張によって明確な割り当ての状態が決定されます。

#### <a name="simple-assignment-expressions"></a>単純な代入式

式の*expr*フォームの`w = expr_rhs`:

*  確実な代入状態*v*する前に*expr_rhs*の確実な割り当ての状態と同じ*v*する前に*expr*します。
*  確実な代入状態*v*後*expr*によって決定されます。
   * 場合*w*として変数が同じ*v*の確実な代入状態*v*後*expr*は確実に代入します。
   * 場合、構造体の型のインスタンス コンストラクター内で、割り当てが行われた場合*w* 、自動的に実装されたプロパティを指定するプロパティへのアクセスは、 *P*で構築されるインスタンス*v*の非表示のバッキング フィールドは、 *P*の確実な代入状態*v*後*expr*は間違いなく割り当てられます。
   * それ以外の場合の確実な代入状態*v*後*expr*の確実な割り当ての状態と同じ*v*後*expr_rhs*します。

#### <a name="-conditional-and-expressions"></a>& & (AND 条件付き) 式

式の*expr*フォームの`expr_first && expr_second`:

*  確実な代入状態*v*する前に*expr_first*の確実な割り当ての状態と同じ*v*する前に*expr*します。
*  確実な代入状態*v*する前に*expr_second*場合が確実に割り当ての状態*v*後*expr_first*いずれかです明示的に代入または「間違いなく true 式の後に割り当てられている」します。 それ以外の場合、これは間違いなく割り当てられません。
*  確実な代入状態*v*後*expr*によって決定されます。
    * 場合*expr_first*の定数式の値では、`false`の確実な代入状態*v*後*expr*確実な代入と同じです状態の*v*後*expr_first*します。
    * の場合の状態*v*後*expr_first*が確実に割り当て、次の状態*v*後*expr*は確実に代入します。
    * の場合の状態*v*後*expr_second*が確実に割り当て、および状態の*v*後*expr_first*は"間違いなく割り当てられた式が false の後に"、その後の状態*v*後*expr*は確実に代入します。
    * の場合、状態の*v*後*expr_second*が確実に割り当てまたは「明示的に代入式が true の後に」後の状態*v*後*expr* 「は確実に代入式の true」です。
    * の場合、状態の*v*後*expr_first* 「明示的に代入式が false の後に」、"と"の状態は、 *v*後*expr_second* 「明示的に代入式が false の後に」、その後の状態は、 *v*後*expr* 「は確実に代入式が false の後に」。
    * それ以外の場合、状態の*v*後*expr*が確実に割り当てできません。

例
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
変数`i`の埋め込みステートメントのいずれかで確実に割り当てられていると見なされます、`if`ステートメントが他にない。 `if`メソッドでステートメント`F`、変数`i`ために埋め込みの最初のステートメントで間違いなく割り当てられた式の実行`(i = y)`この埋め込みステートメントの実行の前に必ずします。 これに対して、変数`i`が間違いなく割り当てられていない 2 番目の埋め込みステートメントの後`x >= 0`可能性がありますの検査が false の場合、変数に`i`未割り当て。

#### <a name="-conditional-or-expressions"></a>||(条件付き OR) 式

式の*expr*フォームの`expr_first || expr_second`:

*  確実な代入状態*v*する前に*expr_first*の確実な割り当ての状態と同じ*v*する前に*expr*します。
*  確実な代入状態*v*する前に*expr_second*場合が確実に割り当ての状態*v*後*expr_first*いずれかです明示的に代入または「false 式の後に間違いなく割り当て済み」です。 それ以外の場合、これは間違いなく割り当てられません。
*  確実な代入ステートメントの*v*後*expr*によって決定されます。
    * 場合*expr_first*の定数式の値では、`true`の確実な代入状態*v*後*expr*確実な代入と同じです状態の*v*後*expr_first*します。
    * の場合の状態*v*後*expr_first*が確実に割り当て、次の状態*v*後*expr*は確実に代入します。
    * の場合の状態*v*後*expr_second*が確実に割り当て、および状態の*v*後*expr_first*は"間違いなく割り当てられている場合は true。 式の後に"、その後の状態*v*後*expr*は確実に代入します。
    * の場合、状態の*v*後*expr_second*が確実に割り当てまたは「明示的に代入式が false の後に」後の状態*v* 後*expr* 「は確実に代入式が false の後に」。
    * の場合、状態の*v*後*expr_first* 「明示的に代入式の true」、"と"の状態は、 *v*後*expr_second*「明示的に代入式の true」、その後の状態は、 *v*後*expr* 「は確実に代入式の true」です。
    * それ以外の場合、状態の*v*後*expr*が確実に割り当てできません。

例
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
変数`i`の埋め込みステートメントのいずれかで確実に割り当てられていると見なされます、`if`ステートメントが他にない。 `if`メソッドでステートメント`G`、変数`i`ために 2 番目の埋め込みステートメントで間違いなく割り当てられた式の実行`(i = y)`この埋め込みステートメントの実行の前に必ずします。 これに対して、変数`i`が間違いなく割り当てられていない最初の埋め込みステートメントの後`x >= 0`可能性がありますの検査が true の場合、変数に`i`未割り当て。

#### <a name="-logical-negation-expressions"></a>! (論理否定) 式

式の*expr*フォームの`! expr_operand`:

*  確実な代入状態*v*する前に*expr_operand*の確実な割り当ての状態と同じ*v*する前に*expr*します。
*  確実な代入状態*v*後*expr*によって決定されます。
    * 場合の状態*v*後 * expr_operand * が確実に割り当て、次の状態*v*後*expr*は確実に代入します。
    * 場合の状態*v*後 * expr_operand * が間違いなく割り当てられていない、状態の*v*後*expr*が確実に割り当てできません。
    * 場合の状態*v*後 * expr_operand *「明示的に代入式が false の後に」、その後の状態は、 *v*後*expr* "は確実に代入後 true式"です。
    * 場合の状態*v*後 * expr_operand *"は true 式の後には割り当て間違いなく、"次の状態は、 *v*後*expr* "は確実に代入後 false。式"です。

#### <a name="-null-coalescing-expressions"></a>?? 式の (null 合体演算子)

式の*expr*フォームの`expr_first ?? expr_second`:

*  確実な代入状態*v*する前に*expr_first*の確実な割り当ての状態と同じ*v*する前に*expr*します。
*  確実な代入状態*v*する前に*expr_second*の確実な割り当ての状態と同じ*v*後*expr_first*します。
*  確実な代入ステートメントの*v*後*expr*によって決定されます。
    * 場合*expr_first*定数式です ([定数式](expressions.md#constant-expressions))、null 値を持つ、状態の*v*後*expr*は同じ状態と*v*後*expr_second*します。
*  それ以外の場合、状態の*v*後*expr*の確実な代入状態と同じでは、 *v*後*expr_first*。

#### <a name="-conditional-expressions"></a>?: (条件) 式

式の*expr*フォームの`expr_cond ? expr_true : expr_false`:

*  確実な代入状態*v*する前に*expr_cond*の状態と同じでは、 *v*する前に*expr*します。
*  確実な代入状態*v*する前に*expr_true*次のいずれかを保持する場合にのみが確実に割り当て。
    * *expr_cond*値を持つ定数式です `false`
    * 状態*v*後*expr_cond*が明示的に代入するか、「間違いなく true 式の後に割り当てられている」。
*  確実な代入状態*v*する前に*expr_false*次のいずれかを保持する場合にのみが確実に割り当て。
    * *expr_cond*値を持つ定数式です `true`
*  状態*v*後*expr_cond*が明示的に代入するか、「を代入式が false の後に」。
*  確実な代入状態*v*後*expr*によって決定されます。
    * 場合*expr_cond*定数式です ([定数式](expressions.md#constant-expressions)) 値を持つ`true`の状態、 *v*後*expr*状態と同じでは、 *v*後*expr_true*します。
    * の場合*expr_cond*定数式です ([定数式](expressions.md#constant-expressions)) 値を持つ`false`の状態、 *v*後*expr*の状態と同じでは、 *v*後*expr_false*します。
    * の場合、状態の*v*後*expr_true*が確実に割り当ての状態と*v*後*expr_false*は間違いなく状態、割り当てられた*v*後*expr*は確実に代入します。
    * それ以外の場合、状態の*v*後*expr*が確実に割り当てできません。

#### <a name="anonymous-functions"></a>匿名関数

*Lambda_expression*または*anonymous_method_expression* *expr*の本文 (か*ブロック*または*式*)*本文*:

*  外部変数の確実な代入状態*v*する前に*本文*の状態と同じでは、 *v*する前に*expr*します。 つまり、外部変数の状態を確実な代入では、匿名関数のコンテキストから継承されます。
*  外部変数の確実な代入状態*v*後*expr*の状態と同じでは、 *v*する前に*expr*します。

例では、
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
以降のコンパイル時エラーが生成されます`max`が間違いなく割り当てられていない匿名関数が宣言されています。 例では、
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
代入後も、コンパイル時エラーが生成されます`n`匿名関数に効力はなくなりましたの確実な代入状態`n`匿名関数の外側です。

## <a name="variable-references"></a>変数参照

A *variable_reference*は、*式*変数として分類します。 A *variable_reference*を現在の値をフェッチして新しい値を格納する両方にアクセスできる記憶域の場所を表します。

```antlr
variable_reference
    : expression
    ;
```

C および C++ で、 *variable_reference*と呼ばれますが、*左辺値*。

## <a name="atomicity-of-variable-references"></a>変数参照の原子性

次のデータ型の読み取りや書き込みはアトミック: `bool`、 `char`、 `byte`、 `sbyte`、 `short`、 `ushort`、 `uint`、 `int`、`float`と参照型。 さらに、上記の一覧では、基になる型と列挙型の読み取りや書き込みもアトミックです。 など、他の型の読み書き`long`、 `ulong`、 `double`、および`decimal`とユーザー定義の型がアトミックである保証されません。 目的のために設計されたライブラリ関数を除けば、インクリメントまたはデクリメントの場合など、分割不可能な読み取り/変更/書き込みの保証はありません。

