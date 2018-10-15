# <a name="enums"></a>列挙体

***列挙型***は個別の値の型です ([値の型](types.md#value-types)) 一連の名前付き定数を宣言します。

例では、

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

という名前の列挙型を宣言します`Color`メンバーと`Red`、 `Green`、および`Blue`します。

## <a name="enum-declarations"></a>列挙型の宣言

列挙型の宣言は、新しい列挙型を宣言します。 列挙型の宣言がキーワードで始まり`enum`名前、アクセシビリティ、基になる型、および列挙型のメンバーを定義します。

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

各列挙型が、対応する整数型と呼ばれる、***基になる型***の列挙型。 この基になる型は、列挙体で定義されているすべての列挙子の値を表現できる必要があります。 列挙型の宣言は、基になる型に明示的に宣言可能性があります`byte`、 `sbyte`、 `short`、 `ushort`、 `int`、 `uint`、`long`または`ulong`します。 なお`char`を基になる型として使用することはできません。 列挙型の宣言を基になる型を明示的に宣言しないが、基になる型の`int`します。

例では、

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

基になる型の列挙型を宣言します`long`します。 開発者は、基になる型を使用することができます`long`などの範囲内にある値の使用を有効にするには、`long`の範囲内にありませんが、 `int`、または将来には、このオプションを保持するためにします。

## <a name="enum-modifiers"></a>列挙型修飾子

*Enum_declaration*列挙型修飾子のシーケンスを必要に応じて含めることができます。

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

同じ修飾子を列挙型の宣言内で複数回のコンパイル時エラーになります。

列挙型の宣言の修飾子がクラス宣言のものと同じ意味を持ちます ([修飾子をクラス](classes.md#class-modifiers))。 ただしを`abstract`と`sealed`修飾子は、enum 宣言では許可されていません。 列挙型は abstract にすることはできません、派生は許可されていません。

## <a name="enum-members"></a>列挙型メンバー

列挙型の宣言の本体は、列挙型の名前付き定数である、0 個以上の列挙型メンバーを定義します。 2 つの列挙型メンバーには、同じ名前を持つことができます。

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

各列挙型のメンバーでは、関連付けられている定数値があります。 この値の型は、列挙型の基になる型です。 各列挙型メンバーの定数値は、列挙型の基になる型の範囲でなければなりません。 例では、

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

ため、コンパイル時エラー結果定数値`-1`、 `-2`、および`-3`の基になる整数型の範囲にない`uint`します。

複数の列挙型メンバーには、同じ関連付けられた値を共有できます。 例では、

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

どの 2 つの列挙型メンバー - 列挙型を示します`Blue`と`Max`--が同じ関連付けられている値。

列挙型のメンバーに関連付けられた値は、暗黙的または明示的に割り当てられます。 列挙型メンバーの宣言がある場合、 *constant_expression*列挙型の基になる型に暗黙的に変換、定数式の値に初期化子は列挙型のメンバーに関連する値。 列挙型メンバーの宣言に初期化子があるない場合、暗黙的に、次のように、関連付けられた値が設定されます。

*  列挙型メンバーが最初に列挙型で宣言された列挙型のメンバーである場合は、関連する値は 0 です。
*  それ以外の場合、列挙型のメンバーに関連付けられた値は、1 つの直前の列挙型のメンバーに関連付けられた値を増やすことで取得されます。 この増加する値は、基になる型で表現できる値の範囲内である必要があります、それ以外の場合、コンパイル時エラーが発生します。

例では、

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

列挙型メンバーの名前と関連付けられている値を出力します。 出力は次のようになります。

```
Red = 0
Green = 10
Blue = 11
```

次の場合。

*  列挙メンバー `Red` (初期化子を持たない列挙型の最初のメンバーであるため) を 0 以外の値を自動的に割り当てられます
*  列挙メンバー`Green`値が明示的に指定`10`;
*  列挙型のメンバーと`Blue`直前のメンバーより 1 大きい値を自動的に割り当てられます。

列挙型のメンバーに関連付けられた値よりも直接的または間接的に関連付けられている列挙型メンバーの値を使用します。 この循環制限以外、列挙型メンバーの初期化子は、テキストの位置に関係なく、他のメンバー初期化子を自由に参照します。 列挙型メンバー初期化子内では他の列挙型メンバーを参照するときのキャストは必要ありませんように他の列挙型メンバーの値を基になる型の型を持つものとして扱われます常に。

例では、

```csharp
enum Circular
{
    A = B,
    B
}
```

コンパイル時エラーのため、結果の宣言`A`と`B`が循環します。 `A` 依存`B`明示的と`B`異なります`A`暗黙的にします。

列挙型メンバーの名前し、クラス内のフィールドとほぼ同じ方法でスコープします。 列挙型のメンバーのスコープは、その列挙型の本体です。 そのスコープでは、単純な名前を指定して列挙型のメンバーを参照できます。 その他のすべてのコードからその列挙型の名前を持つ列挙型のメンバーの名前を修飾する必要があります。 列挙型のメンバーには、任意の宣言されたアクセシビリティはありません。--列挙型のメンバーにアクセスできるは、それを含む列挙型がアクセス可能な場合。

## <a name="the-systemenum-type"></a>System.Enum 型

型`System.Enum`(これは distinct とは異なる列挙型の基になる型から) すべての列挙型の抽象基本クラスから継承したメンバーは、`System.Enum`は任意の列挙型で使用できます。 ボックス化変換 ([ボックス化変換](types.md#boxing-conversions)) から任意の列挙型に存在する`System.Enum`、ボックス化変換 ([ボックス化解除変換](types.md#unboxing-conversions)) から存在する`System.Enum`任意の列挙型にします。

なお`System.Enum`自体ではありません、 *enum_type*します。 *Class_type*すべてから*enum_type*s が派生します。 型`System.Enum`型から継承`System.ValueType`([、System.ValueType 型](types.md#the-systemvaluetype-type)) であり、さらに、型から継承`object`します。 実行時の型の値に`System.Enum`できる`null`または任意の列挙型のボックス化された値への参照。

## <a name="enum-values-and-operations"></a>列挙型の値、および操作

各列挙型は、別個の型を定義します。明示的な列挙値の型変換を ([列挙値の明示的な変換](conversions.md#explicit-enumeration-conversions)) 列挙型と整数型、または 2 つの列挙型間の変換が必要です。 列挙型上で実行できる値のセットは、その列挙型のメンバーでは制限されません。 具体的には、enum の基になる型の任意の値は、列挙型にキャストできるし、その列挙型の個別の有効な値は。

列挙型のメンバー、列挙型の型である (その他の列挙型メンバーの初期化子内を除く: を参照してください[列挙型メンバー](enums.md#enum-members))。 列挙型のメンバーの値が列挙型で宣言された`E`関連付けられている値を持つ`v`は`(E)v`します。

列挙型の値では、次の演算子を使用できます: `==`、 `!=`、 `<`、 `>`、 `<=`、 `>=` ([列挙型の比較演算子](expressions.md#enumeration-comparison-operators))、バイナリ`+`([加算演算子](expressions.md#addition-operator))、バイナリ`-`([減算演算子](expressions.md#subtraction-operator))、 `^`、 `&`、 `|` ([論理列挙型演算子](expressions.md#enumeration-logical-operators))、 `~` ([ビットごとの補数演算子](expressions.md#bitwise-complement-operator))、`++`と`--`([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)と[前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。

すべての列挙型が自動的にクラスから派生`System.Enum`(さらから派生するには、`System.ValueType`と`object`)。 したがって、このクラスの継承されたメソッドとプロパティは、列挙型の値で使用できます。
