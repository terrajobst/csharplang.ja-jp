---
ms.openlocfilehash: 4faef9a12bdff54fa59a55a0206fa72bda4ea585
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704060"
---
# <a name="unsafe-code"></a>アンセーフ コード

コアC#言語は、前の章で定義されているように、 C++ C と、データ型としてのポインターの省略によって異なります。 代わりに、 C#には、ガベージコレクターによって管理されるオブジェクトを作成するための参照と機能が用意されています。 この設計は、他の機能と組み合わせC#て、C またはC++よりもはるかに安全な言語になります。 コアC#言語では、初期化されていない変数、"ぶら下がり" ポインター、または配列がその境界を越えてインデックスを作成する式を持つことはできません。 このため、C とC++プログラムが定期的に消滅するバグのカテゴリ全体が除外されます。

これに対して、C のすべてのC++ポインター型の構造体やC#、に対応する参照型がありますが、ポインター型へのアクセスが必要になる状況もあります。 たとえば、基になるオペレーティングシステムとのやり取り、メモリマップトデバイスへのアクセス、または時間の重要なアルゴリズムの実装は、ポインターにアクセスしなくても不可能であるか、実用的でない可能性があります。 このニーズに対処するC#ために、には***unsafe コード***を記述する機能が用意されています。

アンセーフコードでは、ポインターを宣言して操作したり、ポインターと整数型の間の変換を実行したり、変数のアドレスを取得したりすることができます。 わかりやすいように、unsafe コードを記述することは、 C#プログラム内で C コードを記述することとよく似ています。

アンセーフコードは、実際には開発者とユーザーの両方から見た "安全な" 機能です。 アンセーフコードは、修飾子 @no__t 0 に設定する必要があります。そのため、開発者は安全でない機能を誤って使用することはできません。また、安全でないコードを信頼されていない環境で実行できないように実行エンジンが動作します。

## <a name="unsafe-contexts"></a>Unsafe コンテキスト

のC#安全でない機能は、unsafe コンテキストでのみ使用できます。 Unsafe コンテキストは、型またはメンバーの宣言に @no__t 0 修飾子を含めるか、 *unsafe_statement*を使って追加することによって導入されます。

*  クラス、構造体、インターフェイス、またはデリゲートの宣言には @no__t 0 修飾子を含めることができます。この場合、その型宣言のテキスト範囲全体 (クラス、構造体、またはインターフェイスの本体を含む) は、安全でないコンテキストと見なされます。
*  フィールド、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンスコンストラクター、デストラクター、または静的コンストラクターの宣言には @no__t 0 修飾子を含めることができます。この場合、そのメンバー宣言のテキスト範囲全体が unsafe コンテキストと見なされます。
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

構造体の宣言で指定された @no__t 0 修飾子は、構造体宣言のテキスト範囲全体が unsafe コンテキストになります。 したがって、`Left` および `Right` フィールドをポインター型として宣言できます。 上記の例は、次のように記述することもできます。

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

ここでは、フィールド宣言の @no__t 0 修飾子を使用すると、これらの宣言が安全でないコンテキストと見なされます。

Unsafe コンテキストを確立する以外に、ポインター型の使用を許可する以外に、@no__t 0 修飾子は型またはメンバーに影響を与えません。 この例では、

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

`A` の `F` メソッドの @no__t 0 修飾子を使用すると、単に `F` のテキスト範囲が安全でないコンテキストになり、その言語の安全でない機能が使用される可能性があります。 @No__t-1 の `F` のオーバーライドでは、`unsafe` 修飾子を再指定する必要はありません。ただし、もちろん、@no__t 内の @no__t 3 のメソッドでは、安全でない機能へのアクセスが必要になります。

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

ここでは、`F` のシグネチャにポインター型が含まれているため、unsafe コンテキストでのみ書き込むことができます。 ただし、unsafe コンテキストを導入するには、@no__t 0 の場合と同様に、クラス全体を安全ではないようにするか、`B` の場合と同様にメソッド宣言に `unsafe` 修飾子を含めることができます。

## <a name="pointer-types"></a>ポインター型

Unsafe コンテキストでは、*型*([型](types.md)) は*pointer_type*だけでなく、 *value_type*または*reference_type*でもかまいません。 ただし、unsafe コンテキストの外部で `typeof` 式 ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) に*pointer_type*を使用することもできます。このような使用方法は安全ではありません。

```antlr
type_unsafe
    : pointer_type
    ;
```

*Pointer_type*は、 *unmanaged_type*またはキーワード `void`、その後に `*` トークンとして書き込まれます。

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

参照 (参照型の値) とは異なり、ポインターはガベージコレクターによって追跡されません。ガベージコレクターは、ポインターと、ポインターが指し示すデータを認識しません。 このため、ポインターは参照または参照を含む構造体へのポインターを指すことができず、ポインターの参照型は*unmanaged_type*である必要があります。

*Unmanaged_type*は、 *reference_type*または構築された型ではない任意の型であり、 *reference_type*または構築された型のフィールドは入れ子のレベルには含まれません。 言い換えると、 *unmanaged_type*は次のいずれかになります。

*  `sbyte`、`byte`、`short`、`ushort`、@no__t 4、`uint`、`long`、`ulong`、`char`、`float`、0、1、または 2。
*  任意の*enum_type*。
*  任意の*pointer_type*。
*  構築された型ではなく、 *unmanaged_type*s のフィールドのみを含むユーザー定義*struct_type* 。

ポインターと参照を混在させる直感的なルールとして、参照 (オブジェクト) の referents にはポインターを含めることができますが、ポインターの referents には参照を含めることはできません。

次の表に、ポインター型の例をいくつか示します。

| __例__ | __[説明]__                               |
|-------------|-----------------------------------------------|
| `byte*`     | @No__t へのポインター-0                             |
| `char*`     | @No__t へのポインター-0                             |
| `int**`     | @No__t へのポインターへのポインター-0                   |
| `int*[]`    | @No__t へのポインターの1次元配列-0 |
| `void*`     | 不明な型へのポインター                       |

特定の実装では、すべてのポインター型のサイズと表現が同じである必要があります。

同じ宣言C#でC++複数のポインターが宣言されている場合、C ととは異なり、`*` は基になる型だけと共に書き込まれ、各ポインター名にプレフィックスなどとして記述されることはありません。 次に例を示します。

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

型 `T*` を持つポインターの値は、型 `T` の変数のアドレスを表します。 ポインター間接演算子 `*` ([ポインター間接](unsafe-code.md#pointer-indirection)参照) を使用してこの変数にアクセスできます。 たとえば、型 `int*` の変数 `P` の場合、式 `*P` は @no__t に含まれるアドレスで見つかった @no__t 3 変数を示します。

オブジェクト参照と同様に、ポインターは @no__t 0 になることがあります。 @No__t 0 ポインターに間接演算子を適用すると、実装定義の動作になります。 値が 0 @no__t のポインターは、すべて-ビット-ゼロで表されます。

@No__t-0 型は、不明な型へのポインターを表します。 参照型は不明なので、`void*` 型のポインターに間接演算子を適用することはできません。また、このようなポインターに対して算術演算を実行することもできません。 ただし、`void*` 型のポインターは、他のポインター型 (およびその逆) にキャストできます。

ポインター型は、型の別のカテゴリです。 参照型と値型とは異なり、ポインター型は `object` から継承せず、ポインター型と `object` の間の変換は存在しません。 特に、ボックス化とボックス化解除 ([ボックス化と](types.md#boxing-and-unboxing)ボックス化解除) は、ポインターではサポートされていません。 ただし、異なるポインター型と、ポインター型と整数型の間で変換が許可されています。 これについては、「[ポインター変換](unsafe-code.md#pointer-conversions)」を参照してください。

*Pointer_type*を型引数 (構築された[型](types.md#constructed-types)) として使用することはできません。型の推定 (型の[推定](expressions.md#type-inference)) は、型引数がポインター型であると推論されたジェネリックメソッド呼び出しで失敗します。

*Pointer_type*は、volatile フィールド ([volatile フィールド](classes.md#volatile-fields)) の型として使用できます。

ポインターは `ref` または `out` のパラメーターとして渡すことができますが、これを行うと、未定義の動作が発生する可能性があります。これは、呼び出されたメソッドが返されるときに存在しなくなったローカル変数、またはポイントに使用された固定オブジェクトを指すようにポインターを設定できるためです。は修正されていません。 以下に例を示します。

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

メソッドは、何らかの型の値を返すことができ、その型はポインターにすることができます。 たとえば、連続する一連の @no__t 0、そのシーケンスの要素数、およびその他の `int` 値へのポインターを指定した場合、次のメソッドは、一致が発生した場合にその値のアドレスをそのシーケンスで返します。それ以外の場合は `null` を返します。

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

*  @No__t-0 演算子は、ポインターの間接参照 ([ポインター間接](unsafe-code.md#pointer-indirection)参照) を実行するために使用できます。
*  @No__t-0 演算子は、ポインター ([ポインターメンバーアクセス](unsafe-code.md#pointer-member-access)) を介して構造体のメンバーにアクセスするために使用できます。
*  @No__t-0 演算子は、ポインター ([ポインター要素アクセス](unsafe-code.md#pointer-element-access)) にインデックスを設定するために使用できます。
*  @No__t-0 演算子は、変数のアドレス ([アドレス演算子](unsafe-code.md#the-address-of-operator)) を取得するために使用できます。
*  @No__t-0 および `--` 演算子は、ポインターのインクリメントとデクリメント ([ポインターのインクリメントとデクリメント](unsafe-code.md#pointer-increment-and-decrement)) に使用できます。
*  @No__t-0 および `-` 演算子は、ポインターの算術演算 ([ポインター演算](unsafe-code.md#pointer-arithmetic)) を実行するために使用できます。
*  @No__t-0、`!=`、`<`、`>`、`<=`、および `=>` の各演算子を使用して、ポインター ([ポインター比較](unsafe-code.md#pointer-comparison)) を比較できます。
*  @No__t-0 演算子を使用すると、呼び出し履歴 ([固定サイズバッファー](unsafe-code.md#fixed-size-buffers)) からメモリを割り当てることができます。
*  @No__t-0 ステートメントを使用して変数を一時的に修正し、そのアドレスを取得できるようにすることができます ([fixed ステートメント](unsafe-code.md#the-fixed-statement))。

## <a name="fixed-and-moveable-variables"></a>固定変数と移動可能変数

アドレス演算子 ([アドレス演算子](unsafe-code.md#the-address-of-operator)) と `fixed` ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) は、変数を2つのカテゴリに分割します。***変数***と移動可能な***変数***を修正します。

固定変数は、ガベージコレクターの操作の影響を受けないストレージの場所に存在します。 (固定変数の例としては、ローカル変数、値パラメーター、およびポインターの逆参照によって作成される変数などがあります)。一方、移動可能な変数は、ガベージコレクターによって再配置または破棄される可能性があるストレージの場所に存在します。 移動可能な変数の例としては、オブジェクト内のフィールドや配列の要素などがあります。

@No__t-0 演算子 ([アドレス演算子](unsafe-code.md#the-address-of-operator)) を使用すると、固定変数のアドレスを制限なく取得できます。 ただし、移動可能な変数はガベージコレクターによって再配置または破棄される可能性があるため、移動可能な変数のアドレスは、@no__t 0 のステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) を使用してのみ取得できます。このアドレスは、`fixed` ステートメントの期間。

正確に言うと、固定変数は次のいずれかになります。

*  変数が匿名関数によってキャプチャされていない限り、ローカル変数または値パラメーターを参照する*simple_name* ([簡易名](expressions.md#simple-names)) によって生成される変数。
*  @No__t-2 の形式の*member_access* ([メンバーアクセス](expressions.md#member-access)) によって生成される変数。 `V` は、 *struct_type*の固定変数です。
*  @No__t-2、@no__t フォームの*pointer_member_access* ([ポインターメンバーアクセス](unsafe-code.md#pointer-member-access))、または*pointer_element_access*の形式の*pointer_indirection_expression* ([ポインターの間接](unsafe-code.md#pointer-indirection)参照) によって生成される変数。[ポインター要素へのアクセス](unsafe-code.md#pointer-element-access))`P[E]` の形式。

その他のすべての変数は、移動可能な変数として分類されます。

静的フィールドは移動可能な変数として分類されることに注意してください。 また、パラメーターに指定された引数が固定変数の場合でも、`ref` または `out` パラメーターは移動可能な変数として分類されることに注意してください。 最後に、ポインターを逆参照することによって生成される変数は、常に固定変数として分類されることに注意してください。

## <a name="pointer-conversions"></a>ポインター変換

Unsafe コンテキストでは、次の暗黙的なポインター変換を含むように、使用可能な暗黙の変換 ([暗黙の変換](conversions.md#implicit-conversions)) のセットが拡張されます。

*  任意の*pointer_type*から型 `void*` になります。
*  @No__t-0 リテラルから任意の*pointer_type*になります。

また、unsafe コンテキストでは、次の明示的なポインター変換を含むように、使用可能な明示的な変換 ([明示的な変換](conversions.md#explicit-conversions)) のセットが拡張されます。

*  任意の*pointer_type*から他の*pointer_type*に。
*  @No__t-0、`byte`、`short`、`ushort`、`int`、`uint`、`long`、または `ulong` を任意の*pointer_type*にします。
*  任意の*pointer_type*から `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、または `ulong`。

最後に、unsafe コンテキストでは、標準の暗黙的な変換 ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) のセットに次のポインター変換が含まれています。

*  任意の*pointer_type*から型 `void*` になります。

2つのポインター型の間の変換では、実際のポインター値が変更されることはありません。 つまり、あるポインター型から別のポインター型への変換は、ポインターによって指定された基になるアドレスには影響しません。

あるポインター型が別のポインター型に変換されると、結果のポインターがポイント先の型に対して適切にアラインされていない場合、結果が逆参照されても動作は未定義になります。 一般に、"適切にアラインされた" 概念は推移的であり、`A` 型へのポインターが @no__t 型へのポインターに対して正しく調整されている場合 (つまり、型 `C` へのポインターに対して適切にアラインされた場合)、型 `A` へのポインターは正しくアラインされます型 @no__t へのポインター-4。

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

ポインターと整数の間のマッピングは、実装によって定義されます。 ただし、リニアアドレス空間を使用する 32 * および64ビットの CPU アーキテクチャでは、通常、整数型との間のポインターの変換は、整数型との間で `uint` または @no__t 値の変換と同じように動作します。

### <a name="pointer-arrays"></a>ポインター配列

Unsafe コンテキストでは、ポインターの配列を構築できます。 ポインター配列では、他の配列型に適用される変換の一部のみが許可されます。

*  任意の*array_type*から `System.Array` への暗黙的な参照変換 ([暗黙の参照](conversions.md#implicit-reference-conversions)変換) と、それが実装するインターフェイスもポインター配列に適用されます。 ただし、ポインター型は `object` に変換できないため、`System.Array` またはそれが実装するインターフェイスを介して配列要素にアクセスしようとすると、実行時に例外が発生します。
*  1次元配列型から @no__t `S[]` への暗黙的な参照変換と明示的な[参照](conversions.md#explicit-reference-conversions)変換 ([暗黙](conversions.md#implicit-reference-conversions)の参照変換) は、ポインター配列には適用されません。ポインター型は型引数として使用できないため、ポインター型から非ポインター型への変換はありません。
*  @No__t-1*からの明示*的な参照変換 ([明示的な参照](conversions.md#explicit-reference-conversions)変換) と、それが実装するインターフェイスは、ポインター配列に適用されます。
*  ポインター型を型引数として使用することはできないため、`System.Collections.Generic.IList<S>` とその基本インターフェイスから1次元配列 @no__t 型への明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) は、ポインター配列には適用されません。ポインター型から非ポインター型への変換は行われません。

これらの制限は、 [foreach ステートメント](statements.md#the-foreach-statement)で記述された配列に対する `foreach` ステートメントの展開をポインター配列に適用できないことを意味します。 代わりに、フォームの foreach ステートメント

```csharp
foreach (V v in x) embedded_statement
```

@no__t 0 の型がフォームの配列型である場合 `T[,,...,]`、`N` は次元の数-@no__t 1、@no__t がポインター型で、次のように入れ子になった for ループを使用して展開されます。

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

変数 `a`、`i0`、`i1`、...、`iN` は、`x`、 *embedded_statement* 、またはプログラムのその他のソースコードからは参照できないか、アクセスできません。 埋め込み`v`ステートメントでは、変数は読み取り専用です。 @No__t-1 (要素型) から `V` への明示的な変換 ([ポインター変換](unsafe-code.md#pointer-conversions)) が行われていない場合は、エラーが生成され、それ以上の手順は実行されません。 に`x`値`null`がある場合は、実行時にがスロー`System.NullReferenceException`されます。

## <a name="pointers-in-expressions"></a>式におけるポインター

Unsafe コンテキストでは、式はポインター型の結果を生成する可能性がありますが、unsafe コンテキストの外部では、式がポインター型である場合のコンパイル時エラーになります。 厳密に言うと、unsafe コンテキストの外部では、 *simple_name* ([simple names](expressions.md#simple-names))、 *member_access* ([メンバーアクセス](expressions.md#member-access))、 *invocation_expression* ([呼び出し式](expressions.md#invocation-expressions))*のいずれかに該当する場合、コンパイル時エラーが発生します。element_access* ([要素アクセス](expressions.md#element-access)) はポインター型です。

Unsafe コンテキストでは、 *primary_no_array_creation_expression* ([プライマリ式](expressions.md#primary-expressions)) と*unary_expression* ([単項演算子](expressions.md#unary-operators)) の各生産で、次の追加の構成体が許可されます。

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

単項 `*` 演算子はポインターの間接参照を表し、ポインターが指す変数を取得するために使用されます。 @No__t-0 を評価した結果 (`P` はポインター型の式 `T*`) は `T` 型の変数です。 単項 `*` 演算子を、`void*` 型の式、またはポインター型ではない式に適用するコンパイル時エラーです。

単項 `*` 演算子を @no__t のポインターに適用した場合の効果は、実装によって定義されます。 特に、この操作で `System.NullReferenceException` がスローされる保証はありません。

ポインターに無効な値が割り当てられている場合、単項 `*` 演算子の動作は定義されていません。 単項 `*` 演算子によってポインターを逆参照するための無効な値の中には、ポインターが指す型に対して不適切にアラインされたアドレス (「[ポインターの変換](unsafe-code.md#pointer-conversions)」の例を参照) と、有効期間が終了した後の変数のアドレスがあります。

明確な代入分析を目的として、`*P` という形式の式を評価することによって生成される変数は、最初に割り当てられた変数 ([最初に割り当てられた変数](variables.md#initially-assigned-variables)) と見なされます。

### <a name="pointer-member-access"></a>ポインターメンバーアクセス

*Pointer_member_access*は、 *primary_expression*、その後に "`->`" トークン、*識別子*と省略可能な*type_argument_list*で構成されます。

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

@No__t-0 の形式のポインターメンバーアクセスでは、`P` は `void*` 以外のポインター型の式である必要があります。また、`I` は、@no__t 4 ポイントがある型のアクセス可能なメンバーを示す必要があります。

@No__t-0 という形式のポインターメンバーアクセスは `(*P).I` と正確に評価されます。 ポインター間接演算子 (@no__t 0) の説明については、「[ポインターの間接](unsafe-code.md#pointer-indirection)参照」を参照してください。 メンバーアクセス演算子 (`.`) の説明については、「[メンバーアクセス](expressions.md#member-access)」を参照してください。

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

`->` 演算子は、フィールドにアクセスし、ポインターを介して構造体のメソッドを呼び出すために使用されます。 操作 `P->I` は `(*P).I` とまったく同じであるため、`Main` メソッドも同様に記述されている可能性があります。

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

@No__t-0 の形式のポインター要素アクセスでは、`P` は `void*` 以外のポインター型の式である必要があります。また、`E` は、`int`、`uint`、`long`、または @no__t に暗黙的に変換できる式である必要があります。

@No__t-0 という形式のポインター要素へのアクセスは、`*(P + E)` と正確に評価されます。 ポインター間接演算子 (@no__t 0) の説明については、「[ポインターの間接](unsafe-code.md#pointer-indirection)参照」を参照してください。 ポインター加算演算子 (@no__t 0) の説明については、「[ポインター演算](unsafe-code.md#pointer-arithmetic)」を参照してください。

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

ポインター要素アクセスは、@no__t 0 のループで文字バッファーを初期化するために使用されます。 操作 `P[E]` は `*(P + E)` とまったく同じであるため、この例は同様に記述できます。

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

*Addressof_expression*は、アンパサンド (`&`) の後に*unary_expression*を付けたもので構成されます。

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

式 `T` の @no__t 型であり、固定変数 (固定変数と移動可能な[変数](unsafe-code.md#fixed-and-moveable-variables)) として分類されている場合は、`&E` が指定された変数のアドレスが `E` によって計算されます。 結果の型は `T*` で、値として分類されます。 @No__t-1 が読み取り専用のローカル変数として分類されている場合、または @no__t が移動可能な変数を表している場合、`E` が変数として分類されていないと、コンパイル時にエラーが発生します。 最後の例では、fixed ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) を使用して、そのアドレスを取得する前に変数を一時的に "修正" できます。 「[メンバーアクセス](expressions.md#member-access)」に示されているように、`readonly` フィールドを定義する構造体またはクラスのインスタンスコンストラクターまたは静的コンストラクターの外側では、そのフィールドは変数ではなく値と見なされます。 そのため、そのアドレスを取得することはできません。 同様に、定数のアドレスを取得することはできません。

@No__t-0 演算子では、引数が確実に代入される必要はありませんが、`&` の操作の後に、演算子が適用される変数は、操作が行われる実行パスで確実に割り当てられていると見なされます。 プログラマは、変数の正しい初期化がこの状況で実際に行われるようにする必要があります。

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

`i` は、`p` の初期化に使用される `&i` の操作に従って確実に割り当てられていると見なされます。 @No__t-0 への割り当ては、`i` を初期化しますが、この初期化はプログラマが行う必要があります。割り当てが削除された場合、コンパイル時のエラーは発生しません。

@No__t-0 演算子の明確な割り当て規則は、ローカル変数の冗長な初期化を回避できるようにするために存在します。 たとえば、多くの外部 Api は、API によって入力された構造体へのポインターを受け取ります。 このような Api の呼び出しは、通常、ローカルの構造体変数のアドレスを渡し、規則がないと、構造体変数の冗長な初期化が必要になります。

### <a name="pointer-increment-and-decrement"></a>ポインターのインクリメントとデクリメント

Unsafe コンテキストでは、`++` および `--` 演算子 ([後置インクリメントおよびデクリメント](expressions.md#postfix-increment-and-decrement-operators)演算子、[前置インクリメントおよびデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)) を、`void*` を除くすべての型のポインター変数に適用できます。 したがって、`T*` のすべてのポインター型に対して、次の演算子が暗黙的に定義されます。

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

これらの演算子は、`x + 1` および `x - 1` と同じ結果を生成します ([ポインター演算](unsafe-code.md#pointer-arithmetic))。 つまり、`T*` 型のポインター変数の場合、`++` 演算子は変数に格納されているアドレスに `sizeof(T)` を加算し、`--` 演算子は変数に格納されているアドレスから `sizeof(T)` を減算します。

ポインターインクリメントまたはデクリメント操作がポインター型のドメインをオーバーフローした場合、結果は実装定義になりますが、例外は生成されません。

### <a name="pointer-arithmetic"></a>ポインター算術

Unsafe コンテキストでは、`+` および `-` 演算子 ([加算演算子](expressions.md#addition-operator)および[減算演算子](expressions.md#subtraction-operator)) を、`void*` を除くすべてのポインター型の値に適用できます。 したがって、`T*` のすべてのポインター型に対して、次の演算子が暗黙的に定義されます。

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

@No__t-1 のポインター型の 0 @no__t 式が指定されていて、`int`、`uint`、`long`、または @no__t の型の式 `N` である場合、式 `P + N` と @no__t は、`T*`0 をアドレスに追加した結果として返される型のポインター値を計算します。1 によって指定されます。 同様に、式 `P - N` は、`P` によって指定されたアドレスから `N * sizeof(T)` を減算した結果 @no__t 型のポインター値を計算します。

2つの式を指定すると、@no__t 0 と `Q` のポインター型 `T*` の場合、式 `P - Q` によって指定されたアドレスの差が `P` と `Q` によって計算され、その差が `sizeof(T)` で除算されます。 結果の型は常に `long` です。 実際には、`P - Q` は `((long)(P) - (long)(Q)) / sizeof(T)` として計算されます。

以下に例を示します。

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

Unsafe コンテキストでは、@no__t 0、`!=`、`<`、`>`、`<=`、および `=>` 演算子 ([関係演算子と型検査演算子](expressions.md#relational-and-type-testing-operators)) をすべてのポインター型の値に適用できます。 ポインター比較演算子は次のとおりです。

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

ポインター型から @no__t 0 型への暗黙的な変換が存在するため、ポインター型のオペランドは、これらの演算子を使用して比較できます。 比較演算子は、2つのオペランドによって指定されたアドレスを符号なし整数として比較します。

### <a name="the-sizeof-operator"></a>Sizeof 演算子

`sizeof` は、指定された型の変数が占有しているバイト数を返します。 @No__t-0 のオペランドとして指定された型は、 *unmanaged_type* ([ポインター型](unsafe-code.md#pointer-types)) である必要があります。

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

@No__t-0 演算子の結果は `int` 型の値になります。 定義済みの型によっては、次の表に示すように、@no__t 0 演算子によって定数値が生成されます。


| __[式]__   | __結果__ |
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

それ以外のすべての型では、`sizeof` 演算子の結果は実装によって定義され、定数ではなく値として分類されます。

メンバーを構造体にパックする順序は指定されていません。

配置のために、構造体の先頭に名前のない埋め込み、構造体の内部、および構造体の末尾に存在する可能性があります。 埋め込みとして使用されるビットの内容は不確定です。

構造体型のオペランドに適用された場合、結果は、その型の変数に含まれるバイト数の合計 (埋め込みを含む) になります。

## <a name="the-fixed-statement"></a>Fixed ステートメント

Unsafe コンテキストでは、 *embedded_statement* ([ステートメント](statements.md)) の実稼働では、追加の構成体である `fixed` ステートメントを使用できます。これは、移動可能な変数を "修正" するために使用されます。これは、ステートメントの実行中は、そのアドレスが一定のままであることを示します.

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

各*fixed_pointer_declarator*は、指定された*pointer_type*のローカル変数を宣言し、対応する*fixed_pointer_initializer*によって計算されたアドレスを使用してローカル変数を初期化します。 @No__t-0 ステートメントで宣言されたローカル変数には、その変数の宣言の右側に出現するすべての*fixed_pointer_initializer*と、`fixed` ステートメントの*embedded_statement*でアクセスできます。 @No__t-0 ステートメントで宣言されたローカル変数は読み取り専用と見なされます。 埋め込みステートメントがこのローカル変数を変更しようとした場合、コンパイル時にエラーが発生します (代入または `++` および `--` 演算子を使用) か、`ref` または `out` パラメーターとして渡します。

*Fixed_pointer_initializer*には、次のいずれかを指定できます。

*  "@No__t-0" の後に、アンマネージ型の移動可能な変数 ([固定および](unsafe-code.md#fixed-and-moveable-variables)移動可能な変数) に対して、型 `T*` が指定されている場合、その後に*variable_reference* ([明確な割り当てを決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) が @no__t ます。`fixed` ステートメントで指定されたポインター型に暗黙的に変換できます。 この場合、初期化子は指定された変数のアドレスを計算します。変数は、`fixed` ステートメントの実行中、固定アドレスで保持されることが保証されます。
*  型 @no__t @no__t が指定されているアンマネージ型の要素を含む*array_type*の式は、`fixed` ステートメントで指定されたポインター型に暗黙的に変換できます。 この場合、初期化子は配列内の最初の要素のアドレスを計算します。配列全体は、`fixed` ステートメントの実行中に固定アドレスに保持されることが保証されます。 配列式が null の場合、または配列の要素がゼロの場合、初期化子は0と等しいアドレスを計算します。
*  型 `char*` を指定した @no__t 型の式は、`fixed` ステートメントで指定されたポインター型に暗黙的に変換できます。 この場合、初期化子は文字列内の最初の文字のアドレスを計算します。また、文字列全体は、`fixed` ステートメントの実行中に固定アドレスで保持されることが保証されます。 文字列式が null の場合、`fixed` ステートメントの動作は実装によって定義されます。
*  固定サイズバッファーのメンバーの型が、`fixed` ステートメントで指定されたポインター型に暗黙的に変換できる場合、 *simple_name*または*member_access*は移動可能な変数の固定サイズバッファーメンバーを参照します。 この場合、初期化子は、固定サイズバッファー ([式の固定サイズ](unsafe-code.md#fixed-size-buffers-in-expressions)バッファー) の最初の要素へのポインターを計算します。固定サイズバッファーは、`fixed` ステートメントの実行中に固定アドレスで保持されることが保証されます。

*Fixed_pointer_initializer*によって計算された各アドレスに対して、`fixed` ステートメントでは、そのアドレスによって参照される変数が、`fixed` ステートメントの実行中にガベージコレクターによって再配置または破棄されることを保証します。 たとえば、 *fixed_pointer_initializer*によって計算されたアドレスがオブジェクトのフィールドまたは配列インスタンスの要素を参照している場合、`fixed` ステートメントによって、親オブジェクトのインスタンスが再配置または破棄されないことが保証されます。ステートメントの有効期間。

@No__t-0 ステートメントで作成されたポインターがこれらのステートメントの実行後も保持されないようにするのは、プログラマの責任です。 たとえば、`fixed` ステートメントによって作成されたポインターが外部 Api に渡される場合、Api がこれらのポインターのメモリを保持しないようにするのはプログラマの責任です。

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

上の例の4番目の `fixed` ステートメントでは、3番目のと同様の結果が生成されます。

@No__t-0 ステートメントのこの例では、`string` を使用します。

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

Unsafe コンテキストでは、1次元配列の配列要素は、インデックス `0` からインデックス `Length - 1` で終わるインデックスの順に増加して格納されます。 多次元配列の場合は、配列要素が格納されます。これにより、右端の次元のインデックスが最初に増加し、次に左の次元になるようになります。 配列 @no__t インスタンスに対して-1 @no__t ポインターを取得する @no__t 0 ステートメント内では、`p` から `p + a.Length - 1` までの範囲のポインター値は、配列内の要素のアドレスを表します。 同様に、`p[0]` から `p[a.Length - 1]` までの範囲の変数は、実際の配列要素を表します。 配列を格納する方法を考えると、任意の次元の配列を線形として扱うことができます。

以下に例を示します。

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

配列を修正するには、@no__t 0 ステートメントを使用します。これにより、ポインターを受け取るメソッドにそのアドレスを渡すことができます。

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

文字列インスタンスを修正することによって生成される @no__t 0 値は、常に null で終わる文字列を指します。 ポインターを取得する固定ステートメント内では、-0 @no__t 文字列インスタンス `s` の場合、@no__t ~ @no__t 2 の範囲のポインター値は文字列内の文字のアドレスを表し、ポインター値 @no__t は常に null 文字を指します (値が `'\0'`) の文字。

固定ポインターを使用してマネージ型のオブジェクトを変更すると、未定義の動作が発生する可能性があります。 たとえば、文字列は不変であるため、プログラマは、固定文字列へのポインターで参照される文字が変更されないようにする必要があります。

文字列の自動 null 終了は、"C スタイル" の文字列を必要とする外部 Api を呼び出すときに特に便利です。 ただし、文字列インスタンスには null 文字を含めることが許可されていることに注意してください。 このような null 文字が存在する場合、null で終わる @no__t 0 として処理されると、文字列は切り捨てられます。

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

固定サイズのバッファー宣言には、一連の属性 ([属性](attributes.md))、@no__t 1 つの修飾子 ([修飾子](classes.md#modifiers))、4つのアクセス修飾子 ([型パラメーターと制約](classes.md#type-parameters-and-constraints)) の有効な組み合わせ、および `unsafe` 修飾子 (Unsafe) を含めることができます。[コンテキスト](unsafe-code.md#unsafe-contexts))。 属性と修飾子は、固定サイズのバッファー宣言によって宣言されたすべてのメンバーに適用されます。 固定サイズのバッファー宣言で同じ修飾子が複数回出現する場合、エラーになります。

固定サイズのバッファー宣言には、@no__t 0 修飾子を含めることはできません。

固定サイズバッファーの宣言のバッファー要素型は、宣言によって導入されるバッファーの要素の種類を指定します。 バッファー要素の型は、定義済みの型 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、0、または 1 のいずれかである必要があります。

バッファー要素型の後に固定サイズバッファー宣言子のリストが続き、それぞれに新しいメンバーが導入されています。 固定サイズバッファー宣言子は、メンバーに名前を付け、その後に `[` および `]` トークンで囲まれた定数式で構成されます。 定数式は、固定サイズバッファー宣言子によって導入されたメンバー内の要素の数を表します。 定数式の型は `int` 型に暗黙的に変換できる必要があり、値は0でない正の整数である必要があります。

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

固定サイズバッファーのメンバーが単純名として参照されている場合、その効果は `this.I` という形式のメンバーアクセスと同じになります。 `I` は固定サイズバッファーのメンバーです。

@No__t-0 の形式のメンバーアクセスでは、`E` が構造体型であり、その構造体型で @no__t のメンバー参照が固定サイズのメンバーを識別する場合、`E.I` は次のように分類されたを評価します。

*  式 `E.I` が unsafe コンテキストで発生しない場合、コンパイル時エラーが発生します。
*  @No__t-0 が値として分類されている場合は、コンパイル時エラーが発生します。
*  それ以外の場合、`E` が移動可能な変数 ([固定変数および](unsafe-code.md#fixed-and-moveable-variables)移動可能変数) で、式 @no__t が*fixed_pointer_initializer* ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) ではない場合、コンパイル時エラーが発生します。
*  それ以外の場合、`E` は固定変数を参照し、式の結果は `E` の固定サイズバッファーメンバー `I` の最初の要素へのポインターになります。 結果の型は `S*` です。ここで `S` は `I` の要素の型で、は値として分類されます。

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

固定サイズバッファーのメンバーの最も外側に含まれる構造体変数が、静的変数、クラスインスタンスのインスタンス変数、または配列要素の場合、固定サイズバッファーの要素は自動的に既定値に初期化されます (既定値)。[値](variables.md#default-values))。 それ以外の場合は、固定サイズバッファーの初期コンテンツは未定義です。

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

*Unmanaged_type*は、新しく割り当てられた場所に格納される項目の種類を示し、*式*はこれらの項目の数を示します。 これらは共に、必要な割り当てサイズを指定します。 スタック割り当てのサイズを負の値にすることはできません。そのため、項目数を*constant_expression*として指定すると、負の値に評価されます。

@No__t-0 の形式のスタック割り当て初期化子は、`T` がアンマネージ型 ([ポインター型](unsafe-code.md#pointer-types)) であり、`E` が @no__t 型の式である必要があります。 このコンストラクトは、呼び出し履歴から `E * sizeof(T)` バイトを割り当て、@no__t 型のポインターを新しく割り当てられたブロックに返します。 @No__t-0 が負の値の場合、動作は定義されていません。 @No__t-0 が0の場合、割り当ては行われず、返されるポインターは実装定義になります。 指定されたサイズのブロックを割り当てるために十分なメモリがない場合は、`System.StackOverflowException` がスローされます。

新しく割り当てられたメモリの内容は未定義です。

スタック割り当て初期化子は、`catch` または `finally` ブロック ([try ステートメント](statements.md#the-try-statement)) では許可されていません。

@No__t-0 を使用して割り当てられたメモリを明示的に解放する方法はありません。 関数メンバーの実行中に作成されたすべてのスタック割り当てメモリブロックは、その関数メンバーがを返すと自動的に破棄されます。 これは、C および実装で一般的に検出される拡張機能であるC++ @no__t 0 関数に対応しています。

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

@no__t 0 初期化子は、16文字のバッファーをスタックに割り当てるために、`IntToString` メソッドで使用されます。 メソッドがを返した場合、バッファーは自動的に破棄されます。

## <a name="dynamic-memory-allocation"></a>動的メモリ割り当て

@No__t-0 演算子を除き、はC# 、ガベージコレクションではないメモリを管理するための定義済みの構成体を提供しません。 通常、このようなサービスは、サポートクラスライブラリによって提供されるか、基になるオペレーティングシステムから直接インポートされます。 たとえば、次の `Memory` クラスは、基になるオペレーティングシステムのヒープ関数がからC#アクセスされる方法を示しています。

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

@No__t-0 クラスを使用する例を次に示します。

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

この例では、`Memory.Alloc` を使用して256バイトのメモリを割り当て、0から255までの値を使用してメモリブロックを初期化します。 次に、256要素のバイト配列を割り当て、`Memory.Copy` を使用してメモリブロックの内容をバイト配列にコピーします。 最後に、`Memory.Free` を使用してメモリブロックが解放され、バイト配列の内容がコンソールに出力されます。
