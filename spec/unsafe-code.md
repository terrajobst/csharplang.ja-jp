# <a name="unsafe-code"></a>アンセーフ コード

前の章で定義されているように、core、c# 言語でのデータ型としてのポインターの省略、C および C++ とは大きく異なります。 代わりに、c# には、参照、およびガベージ コレクターによって管理されているオブジェクトを作成することができます。 このデザインは、その他の機能を組み合わせることによって、c# は、C や C++ よりもはるかにより安全な言語にします。 Core c# 言語で単にことはできません、初期化されていない変数、「未解決」のポインターまたは配列の境界を越えてのインデックスを作成する式。 バグの全体カテゴリがちな C および C++ プログラムのため削除します。

C または C++ で実際上すべてのポインター型のコンス トラクター (C#)、対応する参照型には、それでも、状況は、ポインター型へのアクセスが必要になります。 たとえば、基になるオペレーティング システムとやり取り、メモリ マップト デバイスへのアクセスやタイム クリティカルなアルゴリズムを実装できない可能性があります不可能または非現実的ポインターへのアクセスなし。 このニーズに対処する c# 機能を提供します、書き込む***アンセーフ コード***します。

安全でないコードを宣言し、ポインターと、変数のアドレスを整数型の間の変換を実行する、ポインターに使用してなどです。 アンセーフ コードを記述することは、ある意味で、c# プログラム内での C コードの記述と似ています。

アンセーフ コードは、実際には、開発者とユーザーの両方の観点からの「安全」機能です。 修飾子の付いた、アンセーフ コードを明確にマークする必要があります`unsafe`開発者の安全でない機能を誤って、使用可能性があることはできませんし、実行エンジンは信頼されていない環境でアンセーフ コードを実行できないようにするため、します。

## <a name="unsafe-contexts"></a>Unsafe コンテキスト

C# の安全でない機能は、unsafe コンテキストでのみ使用できます。 Unsafe コンテキストを導入、`unsafe`または使用することによって、型またはメンバーの宣言に修飾子を*unsafe_statement*:

*  クラス、構造体、インターフェイス、またはデリゲートの宣言を含めることができます、`unsafe`修飾子でその型の宣言 (クラス、構造体、またはインターフェイスの本文を含む) の全体的なテキスト範囲の場合は、unsafe コンテキストと見なされます。
*  フィールド、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンス コンス トラクター、デストラクター、または静的コンス トラクターの宣言を含めることができます、`unsafe`修飾子でそのメンバーの宣言の全体的なテキスト範囲の場合は、安全でないです。コンテキスト。
*  *Unsafe_statement* 、unsafe コンテキスト内で使用できるように、*ブロック*します。 全体的なテキスト範囲の関連付けられている*ブロック*は unsafe コンテキストと見なされます。

関連付けられている文法は、以下に示します。

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

例

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

`unsafe`構造体の宣言で指定した修飾子により、unsafe コンテキストに構造体の宣言の全体的なテキスト範囲。 したがって、宣言することは、`Left`と`Right`ポインター型にするフィールド。 上記の例も記述できます。

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

ここでは、`unsafe`フィールドの宣言に修飾子がそれらの宣言に、unsafe コンテキストと見なされます。

したがって、ポインター型の使用を許可する、unsafe コンテキストを確立する以外、`unsafe`修飾子は、型またはメンバーに影響を与えません。 例

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

`unsafe`修飾子を`F`メソッド`A`のテキストの範囲が表示されるだけ`F`になる、unsafe コンテキストの言語の安全でない機能を使用できます。 オーバーライドで`F`で`B`、再指定する必要はありません、`unsafe`修飾子--しない限り、もちろん、`F`メソッド`B`安全でない機能にアクセスする必要自体。

ポインター型がメソッドのシグネチャの一部である場合は、状況は若干異なります

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

ここでは、ため`F`の署名には、ポインター型が含まれていることができますのみ記述する必要が、unsafe コンテキストでします。 ただし、unsafe コンテキストは、いずれかを行うクラス全体、安全でないでは、によって発生する可能性`A`、またはを含めることによって、`unsafe`メソッドの宣言に修飾子の場合と同様`B`します。

## <a name="pointer-types"></a>ポインター型

Unsafe コンテキストで、*型*([型](types.md)) 可能性があります、 *pointer_type*と同様に、 *value_type*または*reference_type*. ただし、 *pointer_type*でも使用できます、`typeof`式 ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions))、unsafe コンテキストの外部でそのための使用は安全でないです。

```antlr
type_unsafe
    : pointer_type
    ;
```

A *pointer_type*として書き込まれますが、 *unmanaged_type*またはキーワード`void`、その後に、`*`トークン。

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

前に指定された型、`*`ポインターの型が呼び出される、***参照先の型***ポインター型のです。 ポインター型の値が指す変数の型を表します。

(参照型の値) の参照とは異なり、ポインターは、ガベージ コレクターによって追跡されません--ガベージ コレクターがポインターをポイントするデータに関する知識を持たない。 参照を指すポインターをこの理由は許可されていませんまたは構造体への参照を格納するいると、ポインターの参照先の型である必要があります、 *unmanaged_type*します。

*Unmanaged_type*はない任意の型は、 *reference_type*または構築された型、およびが含まれていない*reference_type*または任意のレベルの型フィールドを構築入れ子にします。 つまり、 *unmanaged_type*は、次の 1 つです。

*  `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、 `decimal`、または`bool`します。
*  すべて*enum_type*します。
*  すべて*pointer_type*します。
*  ユーザー定義*struct_type*構築された型ではないのフィールドを含む*unmanaged_type*にのみです。

直感的なルールを混在ポインターと参照は、ポインターを含む参照 (オブジェクト) の参照が許可されますが、参照を格納するポインターの参照は許可されていませんが。

ポインター型の例をいくつかは、次の表で与えられます。

| __例__ | __説明__                               |
|-------------|-----------------------------------------------|
| `byte*`     | ポインター `byte`                             |
| `char*`     | ポインター `char`                             |
| `int**`     | ポインターへのポインター `int`                   |
| `int*[]`    | ポインターの 1 次元配列 `int` |
| `void*`     | 不明な型へのポインター                       |

特定の実装では、すべてのポインター型は、同じサイズと形式が必要です。

C および C++、c# では、同じ宣言で複数のポインターが宣言されている場合とは異なり、`*`は各ポインター名のプレフィックスなどの区切り記号としてではなく基になる型にのみ、と共に書き込まれます。 次に例を示します。

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

型のポインターの値`T*`型の変数のアドレスを表す`T`します。 ポインター間接演算子`*`([ポインターの間接参照](unsafe-code.md#pointer-indirection)) この変数にアクセスするために使用する可能性があります。 たとえば、変数がある`P`型の`int*`、式`*P`表します、`int`変数に含まれるアドレスにある`P`。

オブジェクト参照のようなポインターである可能性があります`null`します。 間接演算子を適用すること、`null`実装定義の動作の結果のポインター。 値を持つポインター`null`はすべてのビットが 0 で表されます。

`void*`型が不明な型へのポインターを表します。 型のポインターに間接演算子は適用できません参照先の型が不明なため`void*`もこのようなポインターですべての算術演算を実行することができます。 ただし、型のポインター`void*`他のポインター型 (その逆) にキャストすることができます。

ポインター型は、別の種類のカテゴリです。 継承しないポインター型の参照型と値型とは異なり`object`ポインター型間で変換が存在しないと、`object`します。 具体的には、ボックス化とボックス化解除 ([ボックス化とボックス化解除](types.md#boxing-and-unboxing)) ポインターはサポートされていません。 ただし、変換は、異なるポインター型間で、ポインター型と整数型の間に許可されます。 「[ポインター変換](unsafe-code.md#pointer-conversions)します。

A *pointer_type*型引数として使用することはできません ([構築型](types.md#constructed-types))、や型推論 ([型推論](expressions.md#type-inference)) であると推論されたジェネリック メソッドの呼び出しが失敗した、引数がポインター型を入力します。

A *pointer_type* volatile フィールドの型として使用することがあります ([Volatile フィールド](classes.md#volatile-fields))。

としてのポインターを渡すことができますが`ref`または`out`パラメーター、呼び出されたメソッドが戻るときに存在しないローカル変数またはその固定オブジェクトを指すポインターがない場合もあるため、未定義の動作が生じることを設定します。ポイントを使用すると、修正が不要になった。 例えば:

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

メソッドは、いくつかの型の値を返すことができ、その型のポインターであることができます。 たとえば、ポインターの連続したシーケンスに指定されている場合`int`s、そのシーケンスの要素の数とその他の`int`値、次のメソッドは、一致が発生した場合、その順序でその値のアドレスを返しますを返しますそれ以外の場合。`null`:

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

Unsafe コンテキストでは、いくつかの構成要素はポインターに対する操作のために使用できます。

*  `*`ポインターの間接参照を実行する演算子を使用できます ([ポインターの間接参照](unsafe-code.md#pointer-indirection))。
*  `->`ポインターを通じて構造体のメンバーにアクセスする演算子を使用できます ([ポインターのメンバー アクセス](unsafe-code.md#pointer-member-access))。
*  `[]`演算子はポインターのインデックスを使用できる可能性があります ([ポインターの要素へのアクセス](unsafe-code.md#pointer-element-access))。
*  `&`変数のアドレスを取得する演算子を使用できます ([address-of 演算子](unsafe-code.md#the-address-of-operator))。
*  `++`と`--`にポインターをインクリメントおよびデクリメント演算子を使用する ([ポインターのインクリメントとデクリメント](unsafe-code.md#pointer-increment-and-decrement))。
*  `+`と`-`ポインターの算術演算を実行する演算子を使用する ([ポインターの算術演算](unsafe-code.md#pointer-arithmetic))。
*  `==`、 `!=`、 `<`、 `>`、 `<=`、および`=>`ポインターを比較する演算子を使用する ([ポインター比較](unsafe-code.md#pointer-comparison))。
*  `stackalloc`演算子を使用する呼び出しスタックからメモリを割り当てる ([固定サイズ バッファー](unsafe-code.md#fixed-size-buffers))。
*  `fixed`ステートメントは、一時的にそのアドレスを取得するために変数を解決するために使用可能性があります ([fixed ステートメント](unsafe-code.md#the-fixed-statement))。

## <a name="fixed-and-moveable-variables"></a>固定と移動可能変数

Address-of 演算子 ([address-of 演算子](unsafe-code.md#the-address-of-operator)) および`fixed`ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) 2 つのカテゴリ変数に分割: ***の変数を固定***と***移動可能変数***します。

固定変数は、ガベージ コレクターの操作によって影響を受けません記憶域の場所に存在します。 (固定変数の例は、ローカル変数、パラメーターの値、およびポインターの逆参照によって作成された変数含まれます)。その一方で、移動可能な変数は、再配置やガベージ コレクターによって破棄対象である記憶域の場所に存在します。 (移動可能な変数の例に、オブジェクトと配列の要素にあるフィールドが含める)。

`&`演算子 ([address-of 演算子](unsafe-code.md#the-address-of-operator)) に制限なしに取得する固定の変数のアドレスを許可します。 ただし、移動可能な変数は再配置やガベージ コレクターによって破棄されるため、移動可能な変数のアドレスのみを使って取得できます、`fixed`ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement))、およびそのアドレスその期間に対してのみ有効なまま`fixed`ステートメント。

正確には、固定の変数は、次のいずれか。

*  変数の結果から、 *simple_name* ([簡易名](expressions.md#simple-names)) 匿名関数で変数をキャプチャしない限り、ローカル変数または値を持つパラメーターを参照します。
*  変数の結果から、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`V.I`ここで、`V`の固定変数、 *struct_type*。
*  変数の結果から、 *pointer_indirection_expression* ([ポインターの間接参照](unsafe-code.md#pointer-indirection)) フォームの`*P`、 *pointer_member_access* ([ポインターのメンバー アクセス](unsafe-code.md#pointer-member-access)) フォームの`P->I`、または*pointer_element_access* ([ポインターの要素へのアクセス](unsafe-code.md#pointer-element-access)) フォームの`P[E]`します。

その他のすべての変数は、移動可能な変数として分類されます。

静的フィールドが移動可能な変数として分類されることに注意してください。 また、`ref`または`out`パラメーターは、パラメーターの指定された引数が固定の変数である場合でも、移動可能変数として分類されます。 最後に、ポインターを逆参照によって生成された変数は常に固定変数として分類することに注意してください。

## <a name="pointer-conversions"></a>ポインター変換

Unsafe コンテキストでは、使用可能な暗黙的な変換のセット ([暗黙的な変換](conversions.md#implicit-conversions)) が拡張され、次の暗黙的なポインター変換。

*  いずれかから*pointer_type*型に`void*`します。
*  `null`いずれかにリテラル*pointer_type*します。

さらに、使用できる明示的な変換のセットを unsafe コンテキストで ([明示的な変換](conversions.md#explicit-conversions)) が拡張され、次の明示的なポインター変換。

*  いずれかから*pointer_type*他*pointer_type*します。
*  `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、または`ulong`いずれかに*pointer_type*します。
*  いずれかから*pointer_type*に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、または`ulong`します。

最後に、標準の暗黙的な変換のセットを unsafe コンテキストで ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) 次のポインター変換が含まれています。

*  いずれかから*pointer_type*型に`void*`します。

2 つのポインター型間の変換は、実際のポインター値を変更することはありません。 つまり、1 つのポインター型から別の変換は、ポインターによって指定された基になるアドレスへの影響を与えません。

Pointed-to 型に対して、結果のポインターが正しくアラインされない場合、1 つのポインター型は、変換するときに、動作がいる場合、結果を逆参照します。 一般に、「正しくアライン」という概念は推移的な: 型へのポインターの場合`A`型へのポインターが正しくアライン`B`であり、さらの型へのポインターの配置が正しく`C`、型へのポインター、`A`型へのポインターが正しくアライン`C`します。

次のさまざまな型へのポインターを使用して 1 つの型を持つ変数にアクセスする場合を考慮してください。

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

ポインター型が変換される場合、バイトへのポインター変数のアドレス指定された最下位バイトを結果のポイント。 一連の変数のサイズまで、結果のインクリメントでは、その変数の残りのバイトへのポインターを生成します。 たとえば、次のメソッドは、double 8 バイトの各 16 進数の値として表示します。

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

もちろん、生成される出力はエンディアンに依存します。

ポインターと整数間のマッピングでは、実装定義です。 ただし、32 の * とリニア アドレス空間と 64 ビット CPU アーキテクチャ、整数型のポインターの変換通常動作への変換とまったく同じように`uint`または`ulong`にそれぞれ、または整数型の値。

### <a name="pointer-arrays"></a>ポインターの配列

Unsafe コンテキストでは、ポインターの配列を構築することができます。 ポインターの配列では、その他の配列型に適用される変換の一部のみ使用できます。

*  暗黙的な参照変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions)) いずれかから*array_type*に`System.Array`も実装するインターフェイスは、ポインターの配列に適用されます。 配列の要素にアクセスしようとするただし、`System.Array`またはインターフェイスの実装は、例外が実行時に、ポインター型に変換できない`object`します。
*  暗黙的および明示的な参照変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions)、[明示的な参照変換](conversions.md#explicit-reference-conversions)) 1 次元配列型から`S[]`に`System.Collections.Generic.IList<T>`とジェネリックの基本インターフェイスは、型の引数としてポインター型を使用することはできませんし、ポインター型から非ポインター型への変換がないために、ポインターの配列に適用しません。
*  明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) から`System.Array`いずれかに、インターフェイスを実装および*array_type*ポインターの配列に適用されます。
*  明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) から`System.Collections.Generic.IList<S>`1 次元の配列型をその基本インターフェイスと`T[]`ポインター型にすることはできませんので、ポインターの配列には適用されません使用される引数を入力として、ポインター型から非ポインター型への変換はありません。

これらの制限を意味するの拡張、`foreach`で説明されているステートメントで配列に対する[foreach ステートメント](statements.md#the-foreach-statement)ポインターの配列には適用できません。 代わりに、フォームの foreach ステートメント

```csharp
foreach (V v in x) embedded_statement
```

場所の種類`x`形式の配列型は、 `T[,,...,]`、`N`ディメンションから 1 を引いた数と`T`または`V`ポインター型は、次のように for ループで入れ子になったを使用して展開します。

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

変数`a`、 `i0`、 `i1`,...,`iN`を表示またはアクセスではない`x`または*embedded_statement*またはプログラムの他のソース コード。 変数`v`は埋め込みステートメントでは読み取り専用です。 明示的な変換がない場合 ([ポインター変換](unsafe-code.md#pointer-conversions)) から`T`(要素の型) に`V`あり、エラーが生成されるため、以降の手順は実行されません。 場合`x`、値を持つ`null`、`System.NullReferenceException`が実行時にスローされます。

## <a name="pointers-in-expressions"></a>式のポインター

Unsafe コンテキストでは、式は、ポインター型の結果を生成可能性がありますが、unsafe コンテキストの外側はポインター型の式のコンパイル時エラー。 正確には、unsafe コンテキストの外部コンパイル時エラーが発生する場合は、すべて*simple_name* ([簡易名](expressions.md#simple-names))、 *member_access* ([メンバーへのアクセス](expressions.md#member-access))、 *invocation_expression* ([Invocation 式](expressions.md#invocation-expressions))、または*element_access* ([要素へのアクセス](expressions.md#element-access)) ポインター型です。

Unsafe コンテキストで、 *primary_no_array_creation_expression* ([一次式](expressions.md#primary-expressions)) と*unary_expression* ([の単項演算子](expressions.md#unary-operators))運用では、次の追加の構成要素を許可します。

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

これらのコンストラクトは、次のセクションで説明します。 優先順位と安全でない演算子の結合規則を文法で暗黙的に指定するとします。

### <a name="pointer-indirection"></a>ポインターの間接参照

A *pointer_indirection_expression*アスタリスクで構成されます (`*`) 後に、 *unary_expression*します。

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

単項`*`演算子はポインターの間接参照を表し、ポインターが指す変数を取得するために使用します。 評価結果`*P`ここで、`P`ポインター型の式は、 `T*`、型の変数は、`T`します。 単項を適用すると、コンパイル時エラー`*`型の式に演算子`void*`またはポインター型のない式にします。

単項を適用する効果`*`演算子を`null`ポインターは、実装定義です。 具体的には、この操作でスローされる保証はありません、`System.NullReferenceException`します。

無効な値はポインターでは、単項の動作に割り当てられている場合`*`演算子は定義されていません。 単項ポインターを逆参照に無効な値の間で`*`演算子が不適切な配置の種類が指すアドレス (例を参照してください[ポインター変換](unsafe-code.md#pointer-conversions)) と後に変数のアドレス、存続期間が終了します。

形式の式を評価することによって確実な代入分析のために、変数が生成される`*P`は最初に割り当てられていると見なされます ([変数を最初に割り当てられた](variables.md#initially-assigned-variables))。

### <a name="pointer-member-access"></a>ポインターのメンバー アクセス

A *pointer_member_access*から成る、 *primary_expression*、その後に、"`->`"トークン、その後に、*識別子*と省略可能な*type_argument_list*します。

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

フォームのポインターのメンバー アクセスで`P->I`、`P`以外のポインター型の式を指定する必要があります`void*`、および`I`型のアクセス可能なメンバーを表す必要があります`P`ポイント。

フォームのポインターのメンバー アクセス`P->I`とまったく同じ評価`(*P).I`します。 ポインター間接演算子の説明については (`*`) を参照してください[ポインターの間接参照](unsafe-code.md#pointer-indirection)します。 メンバー アクセス演算子の説明については (`.`) を参照してください[メンバー アクセス](expressions.md#member-access)。

例

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

`->`フィールドにアクセスし、ポインターを介して構造体のメソッドを呼び出す演算子を使用します。 操作`P->I`とまったく同じ`(*P).I`、`Main`メソッドも同じように記述できます。

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a>ポインターの要素へのアクセス

A *pointer_element_access*から成る、 *primary_no_array_creation_expression*で囲まれた式の後に"`[`「と」`]`"。

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

ポインターの要素には、フォームへのアクセスで`P[E]`、`P`以外のポインター型の式を指定する必要があります`void*`、および`E`に暗黙的に変換できる式を指定する必要があります`int`、 `uint`、 `long`、または`ulong`します。

ポインターの要素には、フォームへのアクセス`P[E]`とまったく同じ評価`*(P + E)`します。 ポインター間接演算子の説明については (`*`) を参照してください[ポインターの間接参照](unsafe-code.md#pointer-indirection)します。 ポインターの加算演算子の説明については (`+`) を参照してください[ポインターの算術演算](unsafe-code.md#pointer-arithmetic)します。

例

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

ポインターの要素へのアクセスが内で文字バッファーを初期化するために使用される、`for`ループします。 操作`P[E]`とまったく同じ`*(P + E)`例では、同じように記述できます。

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

ポインターの要素アクセス演算子は範囲外のチェックせずエラーと、動作にアクセスする場合、要素が定義されている範囲外です。 これは、C および C++ と同じです。

### <a name="the-address-of-operator"></a>Address-of 演算子

*Addressof_expression*アンパサンドで構成されます (`&`) 後に、 *unary_expression*します。

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

式が指定`E`一種の`T`固定変数として分類されます ([固定属性と移動可能変数](unsafe-code.md#fixed-and-moveable-variables))、コンストラクト`&E`によって指定される変数のアドレスを計算`E`. 結果の型が`T*`値として分類されます。 コンパイル時エラーが発生します`E`場合、変数として分類されない`E`は読み取り専用のローカル変数として分類または`E`移動可能な変数を表します。 最後の場合、fixed ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement))、変数のアドレスを取得する前に、変数を一時的に「修正」するために使用できます。 説明したように[メンバー アクセス](expressions.md#member-access)のインスタンス コンス トラクターまたは構造体を定義するクラスの静的コンス トラクターの外部、`readonly`フィールドに、そのフィールドは変数ではなく、値と見なされます。 そのため、そのアドレスを取得することはできません。 同様に、定数のアドレスを取得することはできません。

`&`演算子が明示的に代入が次の引数を必要としない、`&`操作、演算子が適用される変数と見なされます、操作が発生する実行パスで明示的に代入します。 変数の適切な初期化を確認するプログラマの責任は実際にこのような状況を実行をお勧めします。

例

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

`i` 明示的に代入と見なされます、`&i`初期化するために使用される操作`p`します。 代入`*p`有効な初期化`i`が、この初期化を含めることは、プログラマの責任と割り当てが削除された場合はコンパイル時エラーが発生しません。

確実な代入ルール、`&`演算子存在することでローカル変数の冗長な初期化を回避できます。 たとえば、外部 Api の多くはその API によって格納される構造へのポインターです。 このような Api の呼び出しは通常、構造体のローカル変数のアドレスを渡すし、規則がない構造体変数の冗長な初期化が必要になります。

### <a name="pointer-increment-and-decrement"></a>ポインターのインクリメントとデクリメント

Unsafe コンテキストで、`++`と`--`演算子 ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)と[前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)) ポインターに適用できます除くすべての型の変数`void*`します。 したがって、すべてのポインター型の`T*`、次の演算子が暗黙的に定義されています。

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

演算子と同じ結果を生成する`x + 1`と`x - 1`、それぞれ ([ポインターの算術演算](unsafe-code.md#pointer-arithmetic))。 つまり、型のポインター変数の`T*`、`++`演算子を追加`sizeof(T)`、変数に含まれるアドレスを`--`演算子が減算`sizeof(T)`変数に含まれるアドレスから。

ポインターのインクリメントまたはデクリメント演算がポインター型のドメインをオーバーフロー、実装定義になりますが、例外は生成されません。

### <a name="pointer-arithmetic"></a>ポインターの算術演算

Unsafe コンテキストでは、`+`と`-`演算子 ([加算演算子](expressions.md#addition-operator)と[減算演算子](expressions.md#subtraction-operator)) を除くすべてのポインター型の値に適用できる`void*`します。 したがって、すべてのポインター型の`T*`、次の演算子が暗黙的に定義されています。

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

式が指定`P`ポインター型の`T*`と式`N`型の`int`、 `uint`、 `long`、または`ulong`、式`P + N`と`N + P`コンピューティング、型のポインター値`T*`を加算した結果を`N * sizeof(T)`によって指定されたアドレスに`P`します。 式では同様に、`P - N`型のポインター値を計算`T*`を減算した結果の`N * sizeof(T)`によって指定されたアドレスから`P`します。

2 つの式を指定された`P`と`Q`、ポインター型の`T*`、式`P - Q`で指定されたアドレスの差を計算`P`と`Q`でその違いを除算し、`sizeof(T)`. 結果の型は常に`long`します。 実際には、`P - Q`として計算されます`((long)(P) - (long)(Q)) / sizeof(T)`します。

例えば:

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

これには、出力が生成されます。

```
p - q = -14
q - p = 14
```

ポインターの算術演算がポインター型のドメインをオーバーフローした場合は、実装で定義された方法で、結果が切り捨てられますが、例外は生成されません。

### <a name="pointer-comparison"></a>ポインターの比較

Unsafe コンテキストで、 `==`、 `!=`、 `<`、 `>`、 `<=`、および`=>`演算子 ([関係式と型検査演算子](expressions.md#relational-and-type-testing-operators)) すべての値に適用できますポインターの型。 ポインターの比較演算子は次のとおりです。

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

ポインター型からの暗黙的な変換が存在するので、`void*`型、任意のポインター型のオペランドは、これらの演算子を使用して比較できます。 比較演算子は、符号なし整数として 2 つのオペランドで指定されるアドレスを比較します。

### <a name="the-sizeof-operator"></a>Sizeof 演算子

`sizeof`演算子は、指定した型の変数によって占有されるバイト数を返します。 オペランドとして指定された型`sizeof`必要があります、 *unmanaged_type* ([ポインター型](unsafe-code.md#pointer-types))。

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

結果、`sizeof`演算子は、型の値を`int`します。 特定の組み込み型の場合、`sizeof`演算子は、次の表に示すように、定数値を生成します。


| __式__   | __結果__ |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

その他の種類の結果を`sizeof`演算子の実装で定義および定数ではなく、値として分類されます。

メンバーが構造体にパックされている順序では、指定されていません。

配置のためにある可能性がありますしない埋め込み構造体、内の構造体の先頭に、構造体の最後に名前付き。 埋め込み文字として使用されるビットの内容は不確定です。

構造体型のオペランドに適用する場合の埋め込みを含む、その型の変数のバイト数の合計数になります。

## <a name="the-fixed-statement"></a>Fixed ステートメント

Unsafe コンテキストで、 *embedded_statement* ([ステートメント](statements.md)) 運用により、追加の構築、`fixed`ステートメントでは、移動可能な変数を「修正」するために使用するよう、ステートメントの実行中にアドレスが定数のままになります。

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

各*fixed_pointer_declarator*のローカル変数を宣言して、指定された*pointer_type* 、対応することによって計算されたアドレスでそのローカル変数を初期化*fixed_pointer_initializer*します。 宣言されたローカル変数、`fixed`ステートメントがいずれかでアクセス可能な*fixed_pointer_initializer*秒と、その変数の宣言の右側に発生している、 *embedded_statement*の`fixed`ステートメント。 宣言されたローカル変数、`fixed`ステートメントは読み取り専用と見なされます。 埋め込みステートメントをこのローカル変数を変更しようとすると、コンパイル時エラーが発生します (割り当てまたは`++`と`--`演算子) として渡すか、`ref`または`out`パラメーター。

A *fixed_pointer_initializer*次のいずれかを指定できます。

*  トークン"`&`"続けて、 *variable_reference* ([確実な代入を決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) 移動可能な変数に ([固定属性と移動可能変数](unsafe-code.md#fixed-and-moveable-variables))アンマネージ型の`T`、型指定された`T*`指定されたポインター型に暗黙的に変換されて、`fixed`ステートメント。 この場合、初期化子は、指定された変数のアドレスを計算し、変数の間の固定アドレスで維持することが保証されます、`fixed`ステートメント。
*  式、 *array_type*アンマネージ型の要素を含む`T`、型指定された`T*`指定されたポインター型に暗黙的に変換できる、`fixed`ステートメント。 この場合、初期化子は、配列の最初の要素のアドレスを計算し、配列全体の期間の固定アドレスで維持することが保証されます、`fixed`ステートメント。 配列式が null の場合、または配列の要素が 0 の場合は、初期化子は、アドレスに値が 0 を計算します。
*  型の式`string`、型指定された`char*`指定されたポインター型に暗黙的に変換されて、`fixed`ステートメント。 この場合、初期化子は、文字列の最初の文字のアドレスを計算し、文字列全体の期間の固定アドレスで維持することが保証されます、`fixed`ステートメント。 動作、`fixed`ステートメントは、実装で定義された文字列式が null の場合。
*  A *simple_name*または*member_access*指定された固定サイズ バッファーのメンバーの型が指定されたポインター型に暗黙的に変換、移動可能な変数の固定サイズ バッファーのメンバーを参照します。`fixed`ステートメント。 この場合、初期化子が固定サイズ バッファーの最初の要素へのポインターを計算 ([式で固定サイズ バッファー](unsafe-code.md#fixed-size-buffers-in-expressions))、固定サイズ バッファー、の間の固定アドレスで維持することが保証されます`fixed`ステートメント。

によって計算された各アドレスに対して、 *fixed_pointer_initializer* 、`fixed`ステートメントにより再配置やの間、ガベージ コレクターによって破棄される、アドレスによって参照される変数がありませんが、`fixed`ステートメント。 によってアドレスが計算された場合など、 *fixed_pointer_initializer*オブジェクトのフィールドまたは配列インスタンスの要素を参照して、`fixed`ステートメントでは、オブジェクト インスタンスが再配置されないことが保証されますかステートメントの有効期間中に破棄します。

ポインターがによって作成されたことを確認するプログラマの役目です`fixed`ステートメントは、これらのステートメントの実行終了後は保持されません。 ポインターが作成した場合など、`fixed`ステートメントは、外部 Api に渡されるはプログラマの Api がこれらのポインターのメモリを保持しないようにする必要があります。

(ため、それらを移動することはできません) 固定オブジェクト ヒープの断片化が発生する可能性があります。 そのため、絶対に必要な場合にのみオブジェクトは固定する必要があり、復元可能なの最短時間に対してのみです。

例では、

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

いくつかの用途を示します、`fixed`ステートメント。 最初のステートメントを修正し、静的フィールドのアドレスを取得、2 番目のステートメントを修正インスタンス フィールドのアドレスを取得と 3 番目のステートメントを修正し、配列の要素のアドレスを取得します。 各ケースにされていると、正規表現を使用するとエラー`&`演算子ので、変数がすべて移動可能な変数として分類されます。

4 番目`fixed`ステートメント上の例で、3 番目に同様の結果を生成します。

この例の`fixed`ステートメント使用`string`:

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

インデックスから始まって増加のインデックス順で unsafe コンテキストでは 1 次元配列の配列要素が格納されている`0`インデックスで終わる`Length - 1`します。 多次元配列、右端の次元のインデックスを最初に、増加するように要素が格納された配列の次次ディメンション、という具合に左、左。 内で、`fixed`ポインターを取得するステートメント`p`配列インスタンスへ`a`、ポインター値から`p`に`p + a.Length - 1`配列内の要素のアドレスを表します。 同様に、範囲変数`p[0]`に`p[a.Length - 1]`実際の配列要素を表します。 配列が格納されている方法を指定するには、扱うことができます任意の次元の配列線形ものとして。

例えば:

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

これには、出力が生成されます。

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

例

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

`fixed`ステートメントを使用して配列を修正するため、そのアドレスは、ポインターを受け取るメソッドに渡すことができます。

次に例を示します。

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

fixed ステートメントは、そのアドレスをポインターとして使用できるように、構造体の固定サイズ バッファーの修正に使用されます。

A`char*`修正は文字列のインスタンスは常に null で終わる文字列へのポインターによって生成された値。 ポインターを取得する固定ステートメント内で`p`文字列インスタンスを`s`、ポインター値から`p`に`p + s.Length - 1`、文字列、およびポインターの値の文字のアドレスを表す`p + s.Length`常に null 文字を指します (値を持つ文字`'\0'`)。

固定されたポインターを通じてマネージ型のオブジェクトを変更するには、未定義の動作の結果ことができます。 たとえば、文字列は変更できないため、固定文字列を指すポインターにより参照されている文字が変更されていないことを確認するはプログラマの役目は。

文字列の null の自動終了は、"C"スタイルの文字列を期待する外部 Api を呼び出すときに便利です。 ただし、文字列のインスタンスが null 文字を許可されたことに注意してください。 Null で終わるとして扱われた場合に、このような null 文字が存在する場合、文字列は切り詰められて表示されます`char*`します。

## <a name="fixed-size-buffers"></a>固定サイズ バッファー

固定サイズ バッファーを使用して、構造体のメンバーとして「C スタイル」行で配列を宣言しは主にアンマネージ Api とのやり取りに使用します。

### <a name="fixed-size-buffer-declarations"></a>固定サイズ バッファーの宣言

A***固定サイズ バッファー***記憶域の指定された型の変数の固定長バッファーを表すメンバーします。 固定サイズ バッファーの宣言では、指定された要素型の 1 つまたは複数の固定サイズ バッファーについて説明します。 固定サイズ バッファーは構造体の宣言でのみ許可されますが、unsafe コンテキストでのみ発生することができます ([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts))。

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

固定サイズ バッファーの宣言は、一連の属性を含めることができます ([属性](attributes.md))、`new`修飾子 ([修飾子](classes.md#modifiers))、有効な 4 つのアクセス修飾子の組み合わせ ([型パラメーターや制約](classes.md#type-parameters-and-constraints)) および`unsafe`修飾子 ([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts))。 属性と修飾子は、すべての固定サイズ バッファーの宣言で宣言されたメンバーに適用されます。 同じ修飾子を固定サイズ バッファーの宣言内で複数回のエラーになります。

固定サイズ バッファーの宣言を含めることはできません、`static`修飾子。

バッファーの固定サイズ バッファーの宣言の要素型では、宣言によって導入された、バッファーの要素の型を指定します。 バッファー要素の型は、定義済みの型のいずれかを指定する必要があります`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、または`bool`します。

バッファー要素の型には、新しいメンバーを追加の固定サイズ バッファーの宣言子のリストが続きます。 固定サイズ バッファーの宣言子から成る識別子で囲まれた定数式の後に、メンバーの名前を示す`[`と`]`トークンです。 定数式は、その固定サイズ バッファーの宣言子によって導入されるメンバー内の要素の数を表しています。 定数式の型は、型に暗黙的に変換可能である必要があります`int`値は 0 以外の正の整数である必要があります。

固定サイズ バッファーの要素にメモリ内で順番にレイアウトすることが保証されます。

複数の固定サイズ バッファーを宣言する固定サイズ バッファーの宣言は、同じ属性および要素型を持つ 1 つの固定サイズ バッファーの宣言の複数の宣言と同じです。 次に例を示します。

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

上記の式は、次の式と同じです。

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a>式で固定サイズ バッファー

メンバー ルックアップ ([演算子](expressions.md#operators)) 固定サイズのバッファーのメンバーがフィールドのメンバーの検索とまったく同じように進みます。

固定サイズ バッファーを使用する式で参照できます、 *simple_name* ([型推論](expressions.md#type-inference)) または*member_access* ([コンパイル時のチェック動的なオーバー ロードの解決](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。

形式のメンバー アクセスと同じ効果は、固定サイズ バッファーのメンバーは、簡易名として参照された場合、`this.I`ここで、`I`は固定サイズ バッファーのメンバーです。

形式のメンバー アクセスで`E.I`場合は、`E`は構造体の型とメンバーを参照`I`構造体の型が固定サイズ メンバーを識別に`E.I`は分類および次のように評価されます。

*  場合、式`E.I`は発生しません、unsafe コンテキストでは、コンパイル時エラーが発生します。
*  場合`E`コンパイル時エラーが発生した、値として分類されます。
*  の場合`E`移動可能な変数は、([固定属性と移動可能変数](unsafe-code.md#fixed-and-moveable-variables)) と式`E.I`でない、 *fixed_pointer_initializer* ([固定ステートメント](unsafe-code.md#the-fixed-statement))、コンパイル時エラーが発生します。
*  それ以外の場合、`E`固定変数参照式の結果は固定サイズ バッファーのメンバーの最初の要素へのポインターと`I`で`E`します。 結果は型`S*`ここで、`S`の要素の型は、 `I`、値として分類されます。

固定サイズ バッファーの後続の要素は、最初の要素からポインター操作を使用してアクセスできます。 、配列へのアクセスとは異なり、固定サイズ バッファーの要素へのアクセスは、安全でない操作は、し、範囲のチェックではありません。

次の例では、宣言し、固定サイズ バッファーのメンバーを持つ構造体を使用します。

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a>確実な代入をチェック

固定サイズ バッファーは確実な代入をチェック対象になりません ([確実な代入](variables.md#definite-assignment)) と確実な代入の構造体の型の変数のチェックの目的での固定サイズ バッファーのメンバーは無視されます。

固定サイズ バッファーのメンバーの最も外側にある含む構造体変数は、静的変数、クラスのインスタンス、または配列要素のインスタンス変数が、固定サイズ バッファーの要素は、既定値に自動的に初期化 ([既定値](variables.md#default-values))。 その他のすべての場合は、固定サイズ バッファーの初期コンテンツは定義されません。

## <a name="stack-allocation"></a>スタック割り当て

Unsafe コンテキストでは、ローカル変数宣言 ([ローカル変数宣言](statements.md#local-variable-declarations)) が、呼び出し履歴からメモリを割り当てられます。 スタック割り当ての初期化子を含めることができます。

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type*新しく割り当てられた場所に格納される項目の種類を示す、*式*これらの項目の数を示します。 これらの必要な割り当てサイズを指定、まとめて扱わします。 として項目の数を指定すると、コンパイル時エラーがスタック割り当てのサイズは、負の値にすることはできません、ため、 *constant_expression*負の値に評価されます。

フォームのスタック割り当ての初期化子`stackalloc T[E]`必要があります`T`アンマネージ型にする ([ポインター型](unsafe-code.md#pointer-types)) と`E`型の式を指定する`int`します。 構造`E * sizeof(T)`呼び出しからのバイトがスタックが作成され、型のポインターを返します`T*`、新しく割り当てられたブロックにします。 場合`E`負の値が、動作は未定義です。 場合`E`0、し、割り当てが行われていないと返されるポインターは、実装定義です。 特定のサイズのブロックを割り当てることができる十分なメモリがない場合、`System.StackOverflowException`がスローされます。

新しく割り当てられたメモリの内容は定義されません。

スタック割り当ての初期化子で許可されない`catch`または`finally`ブロック ([try ステートメント](statements.md#the-try-statement))。

使用して割り当てられたメモリを明示的に解放する方法はありません`stackalloc`します。 その関数のメンバーが返されるときに、関数メンバーの実行中に作成されたすべてのスタックが割り当てられたメモリ ブロックが自動的に破棄されます。 これに対応して、`alloca`関数では、C および C++ の実装では一般的な拡張機能。

例

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

`stackalloc`で初期化子が使用される、`IntToString`スタックに 16 文字のバッファーを割り当てるためのメソッド。 バッファーは、メソッドが戻るときに自動的に破棄されます。

## <a name="dynamic-memory-allocation"></a>動的メモリの割り当て

を除き、`stackalloc`演算子 (C#) は用意されていません定義済みの構造には、ガベージ収集されたメモリを管理します。 そのようなサービスは通常のクラス ライブラリをサポートすることによって提供されるまたは、基になるオペレーティング システムから直接インポートします。 たとえば、`Memory`次に示すクラスは可能性があります、基になるオペレーティング システムのヒープ関数を c# からアクセスする方法を示しています。

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

使用する例を`Memory`クラスのとおりです。

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

例では、割り当ての 256 バイトのメモリを`Memory.Alloc`0 から 255 までの値でメモリ ブロックを初期化します。 次に、256 の要素のバイト配列を使用して`Memory.Copy`バイト配列にメモリ ブロックの内容をコピーします。 メモリ ブロックが解放を使用して最後に、`Memory.Free`し、バイト配列の内容は、コンソールに出力します。
