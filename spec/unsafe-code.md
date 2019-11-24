---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310365"
---
# <a name="unsafe-code"></a>アンセーフ コード

コアC#言語は、前の章で定義されているように、 C++ C と、データ型としてのポインターの省略によって異なります。 代わりに、 C#には、ガベージコレクターによって管理されるオブジェクトを作成するための参照と機能が用意されています。 この設計は、他の機能と組み合わせC#て、C またはC++よりもはるかに安全な言語になります。 コアC#言語では、初期化されていない変数、"ぶら下がり" ポインター、または配列がその境界を越えてインデックスを作成する式を持つことはできません。 このため、C とC++プログラムが定期的に消滅するバグのカテゴリ全体が除外されます。

これに対して、C のすべてのC++ポインター型の構造体やC#、に対応する参照型がありますが、ポインター型へのアクセスが必要になる状況もあります。 たとえば、基になるオペレーティングシステムとのやり取り、メモリマップトデバイスへのアクセス、または時間の重要なアルゴリズムの実装は、ポインターにアクセスしなくても不可能であるか、実用的でない可能性があります。 このニーズに対処するC#ために、には***unsafe コード***を記述する機能が用意されています。

アンセーフコードでは、ポインターを宣言して操作したり、ポインターと整数型の間の変換を実行したり、変数のアドレスを取得したりすることができます。 わかりやすいように、unsafe コードを記述することは、 C#プログラム内で C コードを記述することとよく似ています。

アンセーフコードは、実際には開発者とユーザーの両方から見た "安全な" 機能です。 アンセーフコードは修飾子 `unsafe`で明確にマークする必要があるため、開発者は安全でない機能を誤って使用することはできません。また、信頼できない環境で安全でないコードを実行できないようにするために、実行エンジンが動作します。

## <a name="unsafe-contexts"></a>Unsafe コンテキスト

のC#安全でない機能は、unsafe コンテキストでのみ使用できます。 Unsafe コンテキストは、型またはメンバーの宣言に `unsafe` 修飾子を含めるか、 *unsafe_statement*を採用することによって導入されます。

*  クラス、構造体、インターフェイス、またはデリゲートの宣言には `unsafe` 修飾子を含めることができます。この場合、その型宣言のテキスト範囲全体 (クラス、構造体、またはインターフェイスの本体を含む) は、unsafe コンテキストと見なされます。
*  フィールド、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンスコンストラクター、デストラクター、または静的コンストラクターの宣言には、`unsafe` 修飾子を含めることができます。この場合、そのメンバー宣言のテキスト範囲全体が unsafe コンテキストと見なされます。
*  *Unsafe_statement*を使用すると、*ブロック*内で unsafe コンテキストを使用できます。 関連付けられた*ブロック*のテキスト範囲全体は、unsafe コンテキストと見なされます。

関連付けられている文法の生産は次のとおりです。

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

この例では、

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

構造体の宣言で指定された `unsafe` 修飾子によって、構造体の宣言のテキスト範囲全体が unsafe コンテキストになります。 したがって、`Left` と `Right` のフィールドをポインター型として宣言できます。 上記の例は、次のように記述することもできます。

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

ここでは、フィールド宣言の `unsafe` 修飾子によって、これらの宣言が安全でないコンテキストと見なされます。

Unsafe コンテキストを確立する以外に、ポインター型の使用を許可する以外は、`unsafe` 修飾子が型またはメンバーに影響を与えることはありません。 この例では、

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

`A` の `F` メソッドの `unsafe` 修飾子を使用すると、単に `F` のテキスト範囲が安全ではなくなり、その言語の安全でない機能を使用できるようになります。 `B`での `F` のオーバーライドでは、`unsafe` 修飾子を再指定する必要はありません。もちろん、`B` 内の `F` メソッドは、安全でない機能へのアクセスを必要とします。

ポインター型がメソッドのシグネチャの一部である場合、状況は若干異なります。

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

ここでは `F`のシグネチャにポインター型が含まれているため、unsafe コンテキストでのみ書き込むことができます。 ただし、unsafe コンテキストを導入するには、`A`の場合と同様に、クラス全体を安全に作成するか、`B`の場合と同様にメソッド宣言に `unsafe` 修飾子を含めることができます。

## <a name="pointer-types"></a>ポインター型

Unsafe コンテキストでは、*型*([型](types.md)) は、 *pointer_type*だけでなく、 *value_type*または*reference_type*でもかまいません。 ただし、 *pointer_type*は、unsafe コンテキストの外部で `typeof` 式 ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) で使用されることもあります。このような使用方法は安全ではありません。

```antlr
type_unsafe
    : pointer_type
    ;
```

*Pointer_type*は、 *unmanaged_type*またはキーワード `void`として書き込まれ、その後に `*` トークンが続きます。

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

ポインター型の `*` の前に指定された型は、ポインター型の指示***型***と呼ばれます。 ポインター型の値が指す変数の型を表します。

参照 (参照型の値) とは異なり、ポインターはガベージコレクターによって追跡されません。ガベージコレクターは、ポインターと、ポインターが指し示すデータを認識しません。 このため、ポインターは参照を含む構造体への参照または参照を含む構造体へのポインターを指すことができず、ポインターの参照型は*unmanaged_type*である必要があります。

*Unmanaged_type*は、 *reference_type*または構築された型ではない任意の型であり、 *reference_type*または構築された型フィールドを入れ子の任意のレベルで含むことはできません。 つまり、 *unmanaged_type*は次のいずれかになります。

*  `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`。
*  任意の*enum_type*。
*  任意の*pointer_type*。
*  構築された型ではなく*unmanaged_type*s のフィールドのみを含むユーザー定義*struct_type* 。

ポインターと参照を混在させる直感的なルールとして、参照 (オブジェクト) の referents にはポインターを含めることができますが、ポインターの referents には参照を含めることはできません。

次の表に、ポインター型の例をいくつか示します。

| __例__ | __説明__                               |
|-------------|-----------------------------------------------|
| `byte*`     | `byte` へのポインター                             |
| `char*`     | `char` へのポインター                             |
| `int**`     | `int` へのポインターへのポインター                   |
| `int*[]`    | `int` へのポインターの1次元配列 |
| `void*`     | 不明な型へのポインター                       |

特定の実装では、すべてのポインター型のサイズと表現が同じである必要があります。

C およびとC++は異なり、同じ宣言で複数のポインターが宣言さC#れている場合、`*` は、各ポインター名のプレフィックスなどとしてではなく、基になる型のみと共に書き込まれます。 次に例を示します。

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

型 `T*` を持つポインターの値は、`T`型の変数のアドレスを表します。 ポインター間接演算子 `*` ([ポインター間接](unsafe-code.md#pointer-indirection)参照) を使用して、この変数にアクセスできます。 たとえば、`int*`型の変数 `P` が指定されている場合、`*P` 式は `P`に含まれるアドレスで見つかった `int` 変数を表します。

オブジェクト参照と同様に、ポインターを `null`こともできます。 間接演算子を `null` ポインターに適用すると、実装定義の動作になります。 `null` 値を持つポインターは、すべて-ビット-ゼロで表されます。

`void*` 型は、不明な型へのポインターを表します。 参照型は不明なので、間接演算子は `void*`型のポインターには適用できません。また、このようなポインターに対して算術演算を実行することもできません。 ただし、`void*` 型のポインターは、他のポインター型 (およびその逆) にキャストできます。

ポインター型は、型の別のカテゴリです。 参照型と値型とは異なり、ポインター型は `object` から継承せず、ポインター型と `object`の間に変換は存在しません。 特に、ボックス化とボックス化解除 ([ボックス化と](types.md#boxing-and-unboxing)ボックス化解除) は、ポインターではサポートされていません。 ただし、異なるポインター型と、ポインター型と整数型の間で変換が許可されています。 これについては、「[ポインター変換](unsafe-code.md#pointer-conversions)」を参照してください。

*Pointer_type*を型引数 (構築された[型](types.md#constructed-types)) として使用することはできません。型の推定 (型の[推定](expressions.md#type-inference)) は、型引数がポインター型であると推論されたジェネリックメソッド呼び出しで失敗します。

*Pointer_type*は、volatile フィールド ([volatile フィールド](classes.md#volatile-fields)) の型として使用できます。

ポインターは `ref` パラメーターまたは `out` パラメーターとして渡すことができますが、これを行うと、未定義の動作が発生する可能性があります。これは、呼び出されたメソッドから制御が戻ったとき、またはポイントに使用された固定オブジェクトが解決されなくなったときに、現在は存在しないローカル変数を指すようにポインター 例 :

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

メソッドは、何らかの型の値を返すことができ、その型はポインターにすることができます。 たとえば、連続した `int`のシーケンス、そのシーケンスの要素数、およびその他の `int` 値へのポインターを指定した場合、次のメソッドは、一致が発生した場合にその値のアドレスをそのシーケンスで返します。それ以外の場合は `null`を返します。

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

Unsafe コンテキストでは、ポインターを操作するためにいくつかの構造体を使用できます。

*  `*` 演算子は、ポインターの間接参照 ([ポインター間接](unsafe-code.md#pointer-indirection)参照) を実行するために使用できます。
*  `->` 演算子は、ポインター ([ポインターメンバーアクセス](unsafe-code.md#pointer-member-access)) を介して構造体のメンバーにアクセスするために使用できます。
*  `[]` 演算子は、ポインター ([ポインター要素アクセス](unsafe-code.md#pointer-element-access)) にインデックスを設定するために使用できます。
*  `&` 演算子は、変数のアドレス ([アドレス演算子](unsafe-code.md#the-address-of-operator)) を取得するために使用できます。
*  `++` 演算子と `--` 演算子を使用すると、ポインターのインクリメントとデクリメント ([ポインターのインクリメントとデクリメント](unsafe-code.md#pointer-increment-and-decrement)) を行うことができます。
*  `+` 演算子と `-` 演算子を使用して、ポインターの算術演算 ([ポインター演算](unsafe-code.md#pointer-arithmetic)) を実行できます。
*  `==`、`!=`、`<`、`>`、`<=`、および `=>` の各演算子を使用して、ポインター ([ポインター比較](unsafe-code.md#pointer-comparison)) を比較できます。
*  `stackalloc` 演算子を使用すると、呼び出し履歴 ([固定サイズバッファー](unsafe-code.md#fixed-size-buffers)) からメモリを割り当てることができます。
*  `fixed` ステートメントを使用して変数を一時的に修正し、そのアドレスを取得できるようにすることができます ([fixed ステートメント](unsafe-code.md#the-fixed-statement))。

## <a name="fixed-and-moveable-variables"></a>固定変数と移動可能変数

アドレス演算子 ([アドレス演算子](unsafe-code.md#the-address-of-operator)) と `fixed` ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) は、変数を***固定変数***と移動可能***変数***の2つのカテゴリに分割します。

固定変数は、ガベージコレクターの操作の影響を受けないストレージの場所に存在します。 (固定変数の例としては、ローカル変数、値パラメーター、およびポインターの逆参照によって作成される変数などがあります)。一方、移動可能な変数は、ガベージコレクターによって再配置または破棄される可能性があるストレージの場所に存在します。 移動可能な変数の例としては、オブジェクト内のフィールドや配列の要素などがあります。

`&` 演算子 ([アドレス演算子](unsafe-code.md#the-address-of-operator)) を使用すると、固定変数のアドレスを制限なく取得できます。 ただし、移動可能な変数はガベージコレクターによって再配置または破棄される可能性があるため、移動可能な変数のアドレスは `fixed` ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) を使用してのみ取得できます。このアドレスは、その `fixed` ステートメントの実行中にのみ有効です。

正確に言うと、固定変数は次のいずれかになります。

*  変数が匿名関数によってキャプチャされていない限り、ローカル変数または値パラメーターを参照する*simple_name* ([簡易名](expressions.md#simple-names)) によって生成される変数。
*  フォーム `V.I`の*member_access* ([メンバーアクセス](expressions.md#member-access)) によって生成される変数。 `V` は*struct_type*の固定変数です。
*  フォーム `*P`の*pointer_indirection_expression* (ポインターの[間接](unsafe-code.md#pointer-indirection)数)、`P->I`フォームの*pointer_member_access* ([ポインターメンバーアクセス](unsafe-code.md#pointer-member-access))、またはフォーム pointer_element_access の *`P[E]`* ([ポインター要素アクセス](unsafe-code.md#pointer-element-access)) によって生成される変数。

その他のすべての変数は、移動可能な変数として分類されます。

静的フィールドは移動可能な変数として分類されることに注意してください。 また、パラメーターに指定された引数が固定変数の場合でも、`ref` または `out` パラメーターは移動可能な変数として分類されることに注意してください。 最後に、ポインターを逆参照することによって生成される変数は、常に固定変数として分類されることに注意してください。

## <a name="pointer-conversions"></a>ポインター変換

Unsafe コンテキストでは、次の暗黙的なポインター変換を含むように、使用可能な暗黙の変換 ([暗黙の変換](conversions.md#implicit-conversions)) のセットが拡張されます。

*  任意の*pointer_type*から型 `void*`にします。
*  `null` リテラルから任意の*pointer_type*にします。

また、unsafe コンテキストでは、次の明示的なポインター変換を含むように、使用可能な明示的な変換 ([明示的な変換](conversions.md#explicit-conversions)) のセットが拡張されます。

*  任意の*pointer_type*から他の*pointer_type*に。
*  `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong` を任意の*pointer_type*に対して行います。
*  任意の*pointer_type*から `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`になります。

最後に、unsafe コンテキストでは、標準の暗黙的な変換 ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) のセットに次のポインター変換が含まれています。

*  任意の*pointer_type*から型 `void*`にします。

2つのポインター型の間の変換では、実際のポインター値が変更されることはありません。 つまり、あるポインター型から別のポインター型への変換は、ポインターによって指定された基になるアドレスには影響しません。

あるポインター型が別のポインター型に変換されると、結果のポインターがポイント先の型に対して適切にアラインされていない場合、結果が逆参照されても動作は未定義になります。 一般に、"適切にアラインされた" 概念は推移的であり、`A` 型へのポインターが `C``B`型へのポインターに対して正しく調整されている場合、型へのポインターが正しく固定されている場合は `A` 型へのポインターが正しく調整さ `C`れます。

次の例では、ある型を持つ変数が、別の型へのポインターを介してアクセスされているとします。

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

ポインター型が byte へのポインターに変換されると、結果は変数の最小のアドレス指定バイトを指します。 結果の連続したインクリメント (変数のサイズまで) によって、その変数の残りのバイトへのポインターが生成されます。 たとえば、次のメソッドは、2つの8バイトを16進数値として表示します。

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

もちろん、生成される出力は、エンディアンに依存します。

ポインターと整数の間のマッピングは、実装によって定義されます。 ただし、リニアアドレス空間を使用する 32 * および64ビットの CPU アーキテクチャでは、通常、整数型との間のポインターの変換は、整数型との間で `uint` または `ulong` 値の変換と同じように動作します。

### <a name="pointer-arrays"></a>ポインター配列

Unsafe コンテキストでは、ポインターの配列を構築できます。 ポインター配列では、他の配列型に適用される変換の一部のみが許可されます。

*  任意の*array_type*から `System.Array` への暗黙的な参照変換 ([暗黙の参照](conversions.md#implicit-reference-conversions)変換) と、それが実装するインターフェイスもポインター配列に適用されます。 ただし、`System.Array` またはそれが実装するインターフェイスを介して配列要素にアクセスしようとすると、ポインター型が `object`に変換できなくなるため、実行時に例外が発生します。
*  ポインター型を型引数として使用することはできず、ポインター型から非ポインター型への変換もないため、単次元配列 `S[]` 型から `System.Collections.Generic.IList<T>` への暗黙的な参照変換と明示的な参照変換 ([暗黙の参照](conversions.md#implicit-reference-conversions)変換、[明示的な参照](conversions.md#explicit-reference-conversions)変換) は、ポインター配列には適用されません。
*  `System.Array` からの明示的な参照変換 ([明示的 array_type な参照変換](conversions.md#explicit-reference-conversions)) と、それが実装するインターフェイスは、ポインター配列に適用されます。
*  ポインター型を型引数として使用することはできず、ポインター型から非ポインター型への変換もないため、`System.Collections.Generic.IList<S>` からの明示的な参照変換 (明示的な参照変換) と、その基本インターフェイスから1次元配列 `T[]` 型への明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) は、ポインター配列には適用されません。

これらの制限は、 [foreach ステートメント](statements.md#the-foreach-statement)で記述された配列に対する `foreach` ステートメントの展開をポインター配列に適用できないことを意味します。 代わりに、フォームの foreach ステートメント

```csharp
foreach (V v in x) embedded_statement
```

`x` の型が `T[,,...,]`フォームの配列型である場合、`N` は次元の数-1、`T` または `V` はポインター型であり、入れ子になった for ループを使用して次のように展開されます。

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

変数 `a`、`i0`、`i1`、...、`iN` は、`x`、またはプログラムの*embedded_statement*またはその他のソースコードからは参照できないか、アクセスできません。 埋め込みステートメントでは `v` 変数は読み取り専用です。 `T` (要素型) から `V`への明示的な変換 ([ポインター変換](unsafe-code.md#pointer-conversions)) がない場合は、エラーが生成され、それ以上の手順は実行されません。 `x` の値が `null`場合は、実行時に `System.NullReferenceException` がスローされます。

## <a name="pointers-in-expressions"></a>式におけるポインター

Unsafe コンテキストでは、式はポインター型の結果を生成する可能性がありますが、unsafe コンテキストの外部では、式がポインター型である場合のコンパイル時エラーになります。 厳密に言うと、unsafe コンテキストの外部では、 *simple_name* ([単純名](expressions.md#simple-names))、 *member_access* ([メンバーアクセス](expressions.md#member-access))、 *invocation_expression* ([呼び出し式](expressions.md#invocation-expressions))、または*element_access* ([要素アクセス](expressions.md#element-access)) がポインター型である場合、コンパイル時エラーが発生します。

Unsafe コンテキストでは、 *primary_no_array_creation_expression* ([プライマリ式](expressions.md#primary-expressions)) と*unary_expression* ([単項演算子](expressions.md#unary-operators)) の生産によって、次の追加の構成体が許可されます。

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

これらの構成要素については、次のセクションで説明します。 Unsafe 演算子の優先順位と結合規則は、文法によって暗黙的に示されます。

### <a name="pointer-indirection"></a>ポインターの間接参照

*Pointer_indirection_expression*は、アスタリスク (`*`) とそれに続く*unary_expression*で構成されます。

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

単項 `*` 演算子はポインターの間接参照を表し、ポインターが指す変数を取得するために使用されます。 `*P`を評価した結果 `P` はポインター型 `T*`の式であり、`T`型の変数です。 単項 `*` 演算子を、`void*` 型の式、またはポインター型ではない式に適用する場合は、コンパイル時エラーになります。

単項 `*` 演算子を `null` ポインターに適用した場合の効果は、実装によって定義されます。 特に、この操作によって `System.NullReferenceException`がスローされる保証はありません。

ポインターに無効な値が割り当てられている場合、単項 `*` 演算子の動作は定義されていません。 単項 `*` 演算子によってポインターを逆参照するための無効な値の中には、指す型に対して不適切にアラインされたアドレス (「[ポインターの変換](unsafe-code.md#pointer-conversions)」の例を参照) と、有効期間の終了後の変数のアドレスがあります。

明確な代入分析の目的では、`*P` フォームの式を評価することによって生成される変数は、最初に割り当てられた変数 ([最初に割り当てられた変数](variables.md#initially-assigned-variables)) と見なされます。

### <a name="pointer-member-access"></a>ポインターメンバーアクセス

*Pointer_member_access*は、 *primary_expression*、その後に "`->`" トークン、*識別子*と省略可能な*type_argument_list*で構成されます。

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

`P->I`フォームのポインターメンバーアクセスでは、`P` は `void*`以外のポインター型の式である必要があり、`I` は `P` が指す型のアクセス可能なメンバーを示す必要があります。

`P->I` フォームのポインターメンバーアクセスは `(*P).I`として正確に評価されます。 ポインター間接演算子 (`*`) の説明については、「[ポインターの間接](unsafe-code.md#pointer-indirection)参照」を参照してください。 メンバーアクセス演算子 (`.`) の説明については、「[メンバーアクセス](expressions.md#member-access)」を参照してください。

この例では、

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

`->` 演算子は、フィールドにアクセスし、ポインターを介して構造体のメソッドを呼び出すために使用されます。 `P->I` の操作は `(*P).I`とまったく同じであるため、`Main` メソッドも同様に記述されている可能性があります。

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

### <a name="pointer-element-access"></a>ポインター要素へのアクセス

*Pointer_element_access*は、 *primary_no_array_creation_expression*の後に "`[`" と "`]`" で囲まれた式で構成されます。

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

`P[E]`フォームのポインター要素アクセスでは、`P` は `void*`以外のポインター型の式である必要があり、`E` は `int`、`uint`、`long`、または `ulong`に暗黙的に変換できる式である必要があります。

`P[E]` フォームに対するポインター要素のアクセスは `*(P + E)`として正確に評価されます。 ポインター間接演算子 (`*`) の説明については、「[ポインターの間接](unsafe-code.md#pointer-indirection)参照」を参照してください。 ポインター加算演算子 (`+`) の説明については、「[ポインター演算](unsafe-code.md#pointer-arithmetic)」を参照してください。

この例では、

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

ポインター要素アクセスは、`for` ループ内の文字バッファーを初期化するために使用されます。 `P[E]` の操作は `*(P + E)`とまったく同じであるため、例も同様に記述できます。

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

ポインター要素アクセス演算子は、範囲外のエラーを確認しません。また、範囲外の要素にアクセスする場合の動作は定義されていません。 これは、C およびC++と同じです。

### <a name="the-address-of-operator"></a>アドレス演算子

*Addressof_expression*は、アンパサンド (`&`) の後に続く*unary_expression*で構成されます。

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

式 `E` が型 `T` であり、固定変数 (固定変数および移動可能な[変数](unsafe-code.md#fixed-and-moveable-variables)) として分類されている場合、コンストラクト `&E` は `E`によって指定された変数のアドレスを計算します。 結果の型は `T*`、値として分類されます。 コンパイル時にエラーが発生するのは、`E` が変数として分類されていない場合、`E` が読み取り専用のローカル変数として分類されている場合、または `E` が移動可能な変数を表している場合です。 最後の例では、fixed ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) を使用して、そのアドレスを取得する前に変数を一時的に "修正" できます。 「[メンバーアクセス](expressions.md#member-access)」に示されているように、`readonly` フィールドを定義する構造体またはクラスのインスタンスコンストラクターまたは静的コンストラクターの外側では、そのフィールドは変数ではなく値と見なされます。 そのため、そのアドレスを取得することはできません。 同様に、定数のアドレスを取得することはできません。

`&` 演算子では、引数が確実に代入される必要はありませんが、`&` 操作の後、演算子が適用される変数は、操作が行われる実行パスで確実に割り当てられていると見なされます。 プログラマは、変数の正しい初期化がこの状況で実際に行われるようにする必要があります。

この例では、

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

`i` は、`p`の初期化に使用される `&i` 操作の後に確実に割り当てられていると見なされます。 `*p` に適用される割り当ては `i`を初期化しますが、この初期化を含めることはプログラマの責任であり、割り当てが削除された場合、コンパイル時のエラーは発生しません。

`&` 演算子の明確な割り当て規則は、ローカル変数の冗長な初期化を回避できるようにするために存在します。 たとえば、多くの外部 Api は、API によって入力された構造体へのポインターを受け取ります。 このような Api の呼び出しは、通常、ローカルの構造体変数のアドレスを渡し、規則がないと、構造体変数の冗長な初期化が必要になります。

### <a name="pointer-increment-and-decrement"></a>ポインターのインクリメントとデクリメント

Unsafe コンテキストでは、`++` 演算子と `--` 演算子 ([後置インクリメントおよびデクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、[前置インクリメントおよびデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)) を、`void*`以外のすべての型のポインター変数に適用できます。 したがって、`T*`のすべてのポインター型に対して、次の演算子が暗黙的に定義されます。

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

これらの演算子は、`x + 1` と `x - 1`([ポインター演算](unsafe-code.md#pointer-arithmetic)) と同じ結果を生成します。 つまり、`T*`型のポインター変数の場合、`++` 演算子は変数に格納されているアドレスに `sizeof(T)` を追加し、`--` 演算子は変数に格納されているアドレスから `sizeof(T)` を減算します。

ポインターインクリメントまたはデクリメント操作がポインター型のドメインをオーバーフローした場合、結果は実装定義になりますが、例外は生成されません。

### <a name="pointer-arithmetic"></a>ポインター算術

Unsafe コンテキストでは、`+` 演算子と `-` 演算子 ([加算演算子](expressions.md#addition-operator)および[減算演算子](expressions.md#subtraction-operator)) を、`void*`を除くすべてのポインター型の値に適用できます。 したがって、`T*`のすべてのポインター型に対して、次の演算子が暗黙的に定義されます。

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

`T*` ポインター型の式 `P` と、`int`、`uint`、`long`、または `ulong`型の式 `N` を指定すると、`P + N` によって指定されたアドレスに `N + P` を追加した結果として生成される `T*` 型のポインター値を計算 `N * sizeof(T)` ます。`P` 同様に、式 `P - N` は `P`によって指定されたアドレスから `N * sizeof(T)` を減算した結果として得られる `T*` 型のポインター値を計算します。

`T*`ポインター型の2つの式 `P` と `Q`を指定すると、式 `P - Q` は `P` と `Q` によって指定されたアドレスの差を計算し、その差を `sizeof(T)`で除算します。 結果の型は常に `long`です。 実際には、`P - Q` は `((long)(P) - (long)(Q)) / sizeof(T)`として計算されます。

例 :

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

次のような出力が生成されます。

```console
p - q = -14
q - p = 14
```

ポインターの算術演算がポインター型のドメインにオーバーフローした場合、結果は実装定義の形式で切り捨てられますが、例外は生成されません。

### <a name="pointer-comparison"></a>ポインターの比較

Unsafe コンテキストでは、`==`、`!=`、`<`、`>`、`<=`、および `=>` 演算子 ([関係演算子と型テスト演算子](expressions.md#relational-and-type-testing-operators)) をすべてのポインター型の値に適用できます。 ポインター比較演算子は次のとおりです。

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

ポインター型から `void*` 型への暗黙的な変換が存在するため、ポインター型のオペランドは、これらの演算子を使用して比較できます。 比較演算子は、2つのオペランドによって指定されたアドレスを符号なし整数として比較します。

### <a name="the-sizeof-operator"></a>Sizeof 演算子

`sizeof` は、指定された型の変数が占有しているバイト数を返します。 `sizeof` するオペランドとして指定された型は、 *unmanaged_type* ([ポインター型](unsafe-code.md#pointer-types)) である必要があります。

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

`sizeof` 演算子の結果は `int`型の値になります。 定義済みの型によっては、次の表に示すように、`sizeof` 演算子によって定数値が生成されます。


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

それ以外の型の場合、`sizeof` 演算子の結果は実装によって定義され、定数ではなく値として分類されます。

メンバーを構造体にパックする順序は指定されていません。

配置のために、構造体の先頭に名前のない埋め込み、構造体の内部、および構造体の末尾に存在する可能性があります。 埋め込みとして使用されるビットの内容は不確定です。

構造体型のオペランドに適用された場合、結果は、その型の変数に含まれるバイト数の合計 (埋め込みを含む) になります。

## <a name="the-fixed-statement"></a>Fixed ステートメント

Unsafe コンテキストでは、 *embedded_statement* ([ステートメント](statements.md)) の実稼働では、追加の構成体である `fixed` ステートメントを使用できます。これは、移動可能な変数を "修正" するために使用されます。これは、ステートメントの実行中は、そのアドレスが変わらないようにします。

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

各*fixed_pointer_declarator*は、指定された*pointer_type*のローカル変数を宣言し、対応する*fixed_pointer_initializer*によって計算されたアドレスを使用してローカル変数を初期化します。 `fixed` ステートメントで宣言されたローカル変数には、その変数の宣言の右側に出現するすべての*fixed_pointer_initializer*と、`fixed` ステートメントの*embedded_statement*でアクセスできます。 `fixed` ステートメントで宣言されたローカル変数は読み取り専用と見なされます。 埋め込みステートメントがこのローカル変数を変更しようとした場合 (代入演算子または `++` 演算子と `--` 演算子を使用)、または `ref` または `out` パラメーターとして渡すと、コンパイル時エラーが発生します。

*Fixed_pointer_initializer*には、次のいずれかを指定できます。

*  型 `T*` が `fixed` ステートメントで指定されたポインター型に暗黙的に変換可能である場合、トークン "`&`" の後に、アンマネージ型の移動可能な変数 ([固定変数および](unsafe-code.md#fixed-and-moveable-variables)移動可能な変数) への*variable_reference* ([明確な代入を決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) が `T`ます。 この場合、初期化子は指定された変数のアドレスを計算します。変数は、`fixed` ステートメントの実行中、固定アドレスで保持されることが保証されます。
*  型 `T*` が `fixed` ステートメントで指定されたポインター型に暗黙的に変換可能である場合、アンマネージ型 `T`の要素を含む*array_type*の式。 この場合、初期化子は配列内の最初の要素のアドレスを計算します。配列全体は、`fixed` ステートメントの実行中に固定アドレスに保持されることが保証されます。 配列式が null の場合、または配列の要素がゼロの場合、初期化子は0と等しいアドレスを計算します。
*  型 `char*` が `fixed` ステートメントで指定されたポインター型に暗黙的に変換できる場合は、型 `string`の式。 この場合、初期化子は文字列内の最初の文字のアドレスを計算し、文字列全体は、`fixed` ステートメントの実行中は固定アドレスに保持されることが保証されます。 文字列式が null の場合、`fixed` ステートメントの動作は実装によって定義されます。
*  固定サイズバッファーのメンバーの型が、`fixed` ステートメントで指定されたポインター型に暗黙的に変換できる場合、移動可能な変数の固定サイズバッファーメンバーを参照する*simple_name*または*member_access* 。 この場合、初期化子は、固定サイズバッファー ([式の固定サイズ](unsafe-code.md#fixed-size-buffers-in-expressions)バッファー) の最初の要素へのポインターを計算します。固定サイズバッファーは、`fixed` ステートメントの実行中、固定アドレスで保持されることが保証されます。

*Fixed_pointer_initializer*によって計算された各アドレスに対して、`fixed` ステートメントによって、アドレスによって参照される変数が、`fixed` ステートメントの実行中にガベージコレクターによって再配置または破棄されることがなくなります。 たとえば、 *fixed_pointer_initializer*によって計算されたアドレスがオブジェクトのフィールドまたは配列インスタンスの要素を参照している場合、`fixed` ステートメントでは、ステートメントの有効期間中に、それを含んでいるオブジェクトのインスタンスが再配置または破棄されないことが保証されます。

`fixed` ステートメントによって作成されたポインターがこれらのステートメントの実行後も保持されないようにするのは、プログラマの責任です。 たとえば、`fixed` ステートメントによって作成されたポインターが外部 Api に渡される場合、Api がこれらのポインターのメモリを保持しないようにするのはプログラマの責任です。

固定オブジェクトを使用すると、ヒープの断片化が発生する可能性があります (移動できないため)。 そのため、オブジェクトは、絶対に必要な場合にのみ修正してから、できるだけ最短の時間だけ使用するようにしてください。

例

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

`fixed` ステートメントのいくつかの使用方法を示します。 最初のステートメントは、静的フィールドのアドレスを修正して取得します。2番目のステートメントは、インスタンスフィールドのアドレスを修正して取得し、3番目のステートメントは配列要素のアドレスを修正して取得します。 どちらの場合も、変数はすべて移動可能な変数として分類されるため、通常の `&` 演算子を使用するとエラーになります。

上記の例の4番目の `fixed` ステートメントでは、3番目のと同様の結果が生成されます。

この `fixed` ステートメントの例では、`string`を使用します。

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

Unsafe コンテキストでは、1次元配列の配列要素が、インデックス `0` から始まり、インデックス `Length - 1`で終わるインデックスの順序で格納されます。 多次元配列の場合は、配列要素が格納されます。これにより、右端の次元のインデックスが最初に増加し、次に左の次元になるようになります。 配列 `a`インスタンスへのポインター `p` を取得する `fixed` ステートメント内では、`p` から `p + a.Length - 1` の範囲のポインター値は、配列内の要素のアドレスを表します。 同様に、`p[0]` から `p[a.Length - 1]` までの変数は、実際の配列要素を表します。 配列を格納する方法を考えると、任意の次元の配列を線形として扱うことができます。

例 :

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

次のような出力が生成されます。

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

この例では、

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

配列を修正するには、`fixed` ステートメントを使用します。これにより、ポインターを受け取るメソッドにそのアドレスを渡すことができます。

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

fixed ステートメントは、そのアドレスをポインターとして使用できるように、構造体の固定サイズバッファーを修正するために使用されます。

文字列インスタンスを修正することによって生成される `char*` 値は、常に null で終わる文字列を指します。 文字列インスタンス `s`に `p` ポインターを取得する fixed ステートメント内では、`p` から `p + s.Length - 1` までのポインター値が文字列内の文字のアドレスを表し、ポインター値が常に null 文字 (値が `p + s.Length` の文字) を指しています。`'\0'`

固定ポインターを使用してマネージ型のオブジェクトを変更すると、未定義の動作が発生する可能性があります。 たとえば、文字列は不変であるため、プログラマは、固定文字列へのポインターで参照される文字が変更されないようにする必要があります。

文字列の自動 null 終了は、"C スタイル" の文字列を必要とする外部 Api を呼び出すときに特に便利です。 ただし、文字列インスタンスには null 文字を含めることが許可されていることに注意してください。 このような null 文字が存在する場合、null で終わる `char*`として扱われると、文字列は切り捨てられます。

## <a name="fixed-size-buffers"></a>固定サイズ バッファー

固定サイズバッファーは、"C スタイル" のインライン配列を構造体のメンバーとして宣言するために使用されます。これは、主にアンマネージ Api とのやり取りに役立ちます。

### <a name="fixed-size-buffer-declarations"></a>固定サイズバッファーの宣言

***固定サイズバッファー***は、指定された型の変数の固定長バッファーのストレージを表すメンバーです。 固定サイズバッファーの宣言では、特定の要素の型の1つ以上の固定サイズのバッファーが導入されます。 固定サイズバッファーは、構造体宣言でのみ許可され、unsafe コンテキスト ([unsafe コンテキスト](unsafe-code.md#unsafe-contexts)) でのみ使用できます。

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

固定サイズのバッファー宣言には、一連の属性 ([属性](attributes.md))、`new` 修飾子 ([修飾子](classes.md#modifiers))、4つのアクセス修飾子 ([型パラメーターと制約](classes.md#type-parameters-and-constraints)) の有効な組み合わせ、および `unsafe` 修飾子 ([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts)) を含めることができます。 属性と修飾子は、固定サイズのバッファー宣言によって宣言されたすべてのメンバーに適用されます。 固定サイズのバッファー宣言で同じ修飾子が複数回出現する場合、エラーになります。

固定サイズのバッファー宣言では、`static` 修飾子を含めることはできません。

固定サイズバッファーの宣言のバッファー要素型は、宣言によって導入されるバッファーの要素の種類を指定します。 バッファー要素の型は、定義済みの型 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`bool`のいずれかである必要があります。

バッファー要素型の後に固定サイズバッファー宣言子のリストが続き、それぞれに新しいメンバーが導入されています。 固定サイズバッファーの宣言子は、メンバーに名前を付け、その後に `[` と `]` トークンで囲まれた定数式で構成されます。 定数式は、固定サイズバッファー宣言子によって導入されたメンバー内の要素の数を表します。 定数式の型は `int`型に暗黙的に変換できる必要があり、値は0でない正の整数である必要があります。

固定サイズバッファーの要素は、メモリに順番に配置されることが保証されます。

複数の固定サイズバッファーを宣言する固定サイズのバッファー宣言は、同じ属性と要素の型を持つ単一の固定サイズのバッファー宣言の複数の宣言と同じです。 次に例を示します。

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

### <a name="fixed-size-buffers-in-expressions"></a>式でのサイズバッファーの固定

固定サイズのバッファーメンバーのメンバー参照 ([演算子](expressions.md#operators)) は、フィールドのメンバー参照とまったく同じように処理します。

固定サイズバッファーは、 *simple_name* ([型の推論](expressions.md#type-inference)) または*member_access* ([動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) を使用して、式で参照できます。

固定サイズバッファーのメンバーが単純名として参照されている場合、その効果は `this.I`フォームのメンバーアクセスと同じになります。 `I` は固定サイズバッファーのメンバーです。

`E.I`フォームのメンバーアクセスでは、`E` が構造体型であり、その構造体型で `I` のメンバー参照が固定サイズのメンバーを識別する場合、`E.I` は次のように分類されたを評価します。

*  `E.I` 式が unsafe コンテキストで発生しない場合、コンパイル時エラーが発生します。
*  `E` が値として分類されている場合は、コンパイル時エラーが発生します。
*  それ以外の場合、`E` が移動可能な変数 ([固定変数および](unsafe-code.md#fixed-and-moveable-variables)移動可能変数) であり、`E.I` 式が*fixed_pointer_initializer* ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) ではない場合、コンパイル時エラーが発生します。
*  それ以外の場合、`E` は固定変数を参照し、式の結果は `E`で `I` 固定サイズバッファーメンバーの最初の要素へのポインターになります。 結果は `S*`型になります。 `S` は `I`の要素型で、は値として分類されます。

固定サイズバッファーの後続の要素は、最初の要素からのポインター操作を使用してアクセスできます。 配列へのアクセスとは異なり、固定サイズバッファーの要素へのアクセスは安全でない操作であり、範囲チェックされません。

次の例では、固定サイズのバッファーメンバーを持つ構造体を宣言して使用します。

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

### <a name="definite-assignment-checking"></a>明確な代入のチェック

固定サイズバッファーは、明示代入のチェック ([明確な代入](variables.md#definite-assignment)) の対象にはなりません。固定サイズバッファーのメンバーは、構造体型の変数の明確な割り当てチェックを目的として無視されます。

固定サイズバッファーのメンバーの最も外側に含まれる構造体変数が、静的変数、クラスインスタンスのインスタンス変数、または配列要素の場合、固定サイズバッファーの要素は既定値 ([既定値](variables.md#default-values)) に自動的に初期化されます。 それ以外の場合は、固定サイズバッファーの初期コンテンツは未定義です。

## <a name="stack-allocation"></a>スタック割り当て

Unsafe コンテキストでは、ローカル変数宣言 ([ローカル変数宣言](statements.md#local-variable-declarations)) に、呼び出し履歴からメモリを割り当てるスタック割り当て初期化子を含めることができます。

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type*は、新しく割り当てられた場所に格納される項目の種類を示し、*式*はこれらの項目の数を示します。 これらは共に、必要な割り当てサイズを指定します。 スタック割り当てのサイズを負の値にすることはできません。そのため、項目数を負の値に評価される*constant_expression*として指定すると、コンパイル時エラーになります。

`stackalloc T[E]` フォームのスタック割り当て初期化子は、アンマネージ型 ([ポインター型](unsafe-code.md#pointer-types)) であること、および `int`型の式であることを `E` `T` 必要があります。 このコンストラクトは、呼び出し履歴から `E * sizeof(T)` バイトを割り当て、新しく割り当てられたブロックに `T*`型のポインターを返します。 `E` が負の値の場合、動作は定義されていません。 `E` がゼロの場合、割り当ては行われず、返されるポインターは実装定義になります。 指定されたサイズのブロックを割り当てるために十分なメモリがない場合は、`System.StackOverflowException` がスローされます。

新しく割り当てられたメモリの内容は未定義です。

スタック割り当て初期化子は、`catch` または `finally` ブロック ([try ステートメント](statements.md#the-try-statement)) では許可されていません。

`stackalloc`を使用して割り当てられたメモリを明示的に解放する方法はありません。 関数メンバーの実行中に作成されたすべてのスタック割り当てメモリブロックは、その関数メンバーがを返すと自動的に破棄されます。 これは、C およびC++実装で一般的に検出される拡張機能である `alloca` 関数に対応しています。

この例では、

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

`stackalloc` 初期化子は、スタックに16文字のバッファーを割り当てるために、`IntToString` メソッドで使用されます。 メソッドがを返した場合、バッファーは自動的に破棄されます。

## <a name="dynamic-memory-allocation"></a>動的メモリ割り当て

`stackalloc` 演算子を除き、はC# 、ガベージコレクションではないメモリを管理するための定義済みの構成体を提供しません。 通常、このようなサービスは、サポートクラスライブラリによって提供されるか、基になるオペレーティングシステムから直接インポートされます。 たとえば、次の `Memory` クラスは、基になるオペレーティングシステムのヒープ関数がからC#アクセスされる方法を示しています。

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

`Memory` クラスを使用する例を次に示します。

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

この例では、`Memory.Alloc` を介して256バイトのメモリを割り当て、0から255までの値を使用してメモリブロックを初期化します。 次に、256要素のバイト配列を割り当て、`Memory.Copy` を使用してメモリブロックの内容をバイト配列にコピーします。 最後に、`Memory.Free` を使用してメモリブロックが解放され、バイト配列の内容がコンソールに出力されます。
