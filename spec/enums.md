---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703964"
---
# <a name="enums"></a>列挙体

***列挙型***は、名前付き定数のセットを宣言する個別の値型 ([値型](types.md#value-types)) です。

例

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

`Red`、`Green`、および `Blue`のメンバーを持つ `Color` という名前の列挙型を宣言します。

## <a name="enum-declarations"></a>列挙型の宣言

列挙型宣言は、新しい列挙型を宣言します。 列挙型宣言は、キーワード `enum`で始まり、名前、アクセシビリティ、基になる型、および列挙型のメンバーを定義します。

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

各列挙型には、列挙型の***基になる型***と呼ばれる対応する整数型があります。 この基になる型は、列挙体で定義されているすべての列挙子の値を表すことができる必要があります。 列挙型宣言は、`byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`の基になる型を明示的に宣言できます。 `char` は、基になる型として使用できないことに注意してください。 基になる型を明示的に宣言しない列挙型宣言には、`int`の基になる型があります。

例

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

基になる型が `long`の列挙型を宣言します。 開発者は、例に示すように、基になる型の `long`を使用して、`long` の範囲内の値の使用を有効にしたり、`int`の範囲外にしたり、今後このオプションを保持したりすることができます。

## <a name="enum-modifiers"></a>列挙修飾子

*Enum_declaration*には、必要に応じて列挙修飾子のシーケンスを含めることができます。

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

同じ修飾子が列挙型宣言で複数回出現する場合、コンパイル時エラーになります。

列挙型宣言の修飾子は、クラス宣言 ([クラス修飾子](classes.md#class-modifiers)) と同じ意味を持ちます。 ただし、enum 宣言では、`abstract` 修飾子と `sealed` 修飾子は使用できません。 列挙型を抽象にすることはできません。また、派生を許可しません。

## <a name="enum-members"></a>列挙型メンバー

列挙型宣言の本体では、列挙型の名前付き定数である0個以上の列挙型メンバーを定義します。 2つの列挙メンバーが同じ名前を持つことはできません。

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

各列挙型メンバーには、定数値が関連付けられています。 この値の型は、それを含む列挙型の基になる型です。 各列挙型メンバーの定数値は、列挙型の基になる型の範囲内である必要があります。 例

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

`-1`、`-2`、および `-3` 定数値が基になる整数型 `uint`の範囲内にないため、コンパイル時エラーが発生します。

複数の列挙メンバーが同じ関連する値を共有する場合があります。 例

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

2つの列挙メンバー--`Blue` と `Max` に同じ値が関連付けられている列挙体を示します。

列挙メンバーの関連する値は、暗黙的または明示的に割り当てられます。 列挙型メンバーの宣言に*constant_expression*初期化子がある場合、その定数式の値は列挙型の基になる型に暗黙的に変換され、列挙型メンバーの値になります。 列挙型メンバーの宣言に初期化子がない場合、次のように、そのメンバーに関連付けられている値が暗黙的に設定されます。

*  列挙型のメンバーが列挙型で宣言された最初の列挙型のメンバーである場合、それに関連付けられた値は0になります。
*  それ以外の場合、列挙型メンバーの関連する値を取得するには、直前の列挙型メンバーの関連する値を1つ増やします。 この増加した値は、基になる型で表すことができる値の範囲内である必要があります。それ以外の場合は、コンパイル時にエラーが発生します。

例

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

列挙メンバー名とそれに関連付けられている値を出力します。 出力は次のようになります。

```console
Red = 0
Green = 10
Blue = 11
```

次の理由が考えられます。

*  列挙型メンバー `Red` には、初期化子がなく、最初の列挙メンバーであるため、値0が自動的に割り当てられます。
*  列挙型のメンバー `Green` には、明示的に値 `10`が指定されます。
*  列挙型メンバー `Blue` には、その前にあるメンバーよりも大きい値が自動的に割り当てられます。

列挙型メンバーに関連付けられた値は、それ自体に関連付けられている列挙型メンバーの値を直接または間接的に使用することはできません。 この循環制限以外に、列挙型メンバー初期化子は、テキストの位置に関係なく、他の列挙型メンバーの初期化子を自由に参照できます。 列挙メンバー初期化子内では、他の列挙型メンバーの値は、他の列挙型のメンバーを参照するときにキャストが不要になるように、その基になる型の型として常に処理されます。

例

```csharp
enum Circular
{
    A = B,
    B
}
```

`A` と `B` の宣言が循環しているため、コンパイル時エラーが発生します。 `A` は `B` に明示的に依存し、`B` は暗黙的に `A` に依存します。

列挙型メンバーには、クラス内のフィールドに厳密に似た方法で名前が付けられます。 列挙型メンバーのスコープは、それを含んでいる列挙型の本体です。 そのスコープ内では、列挙型のメンバーを単純な名前で参照できます。 他のすべてのコードからは、列挙型メンバーの名前が列挙型の名前で修飾されている必要があります。 列挙型メンバーは、宣言されたアクセシビリティを持っていません。列挙型メンバーには、その列挙型がアクセス可能な場合にアクセスできます。

## <a name="the-systemenum-type"></a>System.enum 型

`System.Enum` 型は、すべての列挙型の抽象基本クラス (これは列挙型の基になる型とは異なります) であり、`System.Enum` から継承されたメンバーは任意の列挙型で使用できます。 任意の列挙型から `System.Enum`へのボックス変換 ([ボックス](types.md#boxing-conversions)化変換) が存在し、`System.Enum` から任意の列挙型へのアンボックス変換 ([ボックス](types.md#unboxing-conversions)化変換) が存在します。

`System.Enum` は、それ自体が*enum_type*ではないことに注意してください。 これは、すべての*enum_type*の派生元である*class_type*です。 型 `System.Enum` `System.ValueType` (system.string[型](types.md#the-systemvaluetype-type)) から継承されます。この型は `object`型から継承されます。 実行時には、`System.Enum` 型の値を `null` するか、任意の列挙型のボックス化された値への参照を指定できます。

## <a name="enum-values-and-operations"></a>列挙値と操作

それぞれの列挙型は、個別の型を定義します。列挙型と整数型の間、または2つの列挙型間で変換を行うには、明示的な列挙変換 ([明示的な列挙](conversions.md#explicit-enumeration-conversions)変換) が必要です。 列挙型が受け取ることができる値のセットは、列挙型のメンバーによって制限されません。 特に、列挙型の基になる型の任意の値を列挙型にキャストできます。これは、その列挙型の個別の有効な値です。

列挙型メンバーには、それを含む列挙型の型があります (他の列挙型メンバーの初期化子内を除き[ます)。列挙型](enums.md#enum-members)メンバーを参照してください。 Enum 型で宣言された列挙型メンバーの値が、関連付けられている値 `v` `E` `(E)v`です。

列挙型の値には、次の演算子を使用できます。 `==`、`!=`、`<`、`>`、`<=`、`>=` ([列挙比較演算子](expressions.md#enumeration-comparison-operators))、バイナリ `+` ([加算演算子](expressions.md#addition-operator))、バイナリ `-` ([減算演算子](expressions.md#subtraction-operator))、`^`、`&`、`|` ([列挙論理演算子](expressions.md#enumeration-logical-operators))、`~` ([ビットごとの補数演算子](expressions.md#bitwise-complement-operator))、`++` `--` ([後置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、[前置インクリメント演算子と前置デクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。

すべての列挙型は、クラス `System.Enum` (つまり、`System.ValueType` と `object`から派生します) から自動的に派生します。 したがって、このクラスの継承されたメソッドとプロパティは、列挙型の値に対して使用できます。
