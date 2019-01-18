---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229785"
---
# <a name="arrays"></a>配列

配列は、さまざまな算出されたインデックスを介してアクセスする変数を含むデータ構造です。 配列の要素とも呼ばれます。 配列に含まれる変数はすべて、同じ型と、この型には、配列の要素の型が呼び出されます。

配列には、配列の各要素に関連付けられているインデックスの数を決定するランクがあります。 配列のランクは、配列のディメンションであるとも呼ばれます。 ランクが 1 の配列と呼ばれる、 ***1 次元配列***します。 配列と呼ばれる 1 つより大きいランクを持つ、***多次元配列***します。 特定のサイズが設定された多次元配列は、2 次元配列や 3 次元の配列とも呼ばれます。

配列の各次元が、関連付けられた長さはゼロに整数の数より大きいか等しいです。 次元の長さは、配列の型の一部ではありませんが、配列型のインスタンスが実行時に作成されたときに確立されます。 次元の長さは、その次元のインデックスの有効な範囲を決定します。長さのディメンションの`N`、インデックスの範囲は`0`に`N - 1`包括的です。 配列内の要素の合計数は、配列の各次元の長さの製品です。 1 つ以上の配列の次元の長さ 0 の場合、配列は空にすることはできます。

配列の要素型には、配列型を含む任意の型を指定できます。

## <a name="array-types"></a>配列型

配列型として書き込まれます、 *non_array_type* 1 つまたは複数続く*rank_specifier*: %s

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

A *non_array_type*は any*型*いない自体は、 *array_type*します。

配列型のランクが左端で指定された*rank_specifier*で、 *array_type*:A *rank_specifier*配列が 1 を加えた数のランクを持つ配列であることを示します"`,`"のトークン、 *rank_specifier*します。

配列型の要素型が、左端を削除した結果を型*rank_specifier*:

*  フォームの配列型`T[R]`配列のランクを持つ`R`と非配列要素型を`T`します。
*  フォームの配列型`T[R][R1]...[Rn]`配列のランクを持つ`R`と要素型`T[R1]...[Rn]`します。

実際には、 *rank_specifier*s は、非配列の最後の要素型の前に右に左から読み取られます。 型`int[][,,][,]`の 2 次元の配列の 3 次元の配列の 1 次元配列は、`int`します。

実行時に、配列型の値を指定できます`null`またはその配列型のインスタンスへの参照。

### <a name="the-systemarray-type"></a>System.Array 型

型`System.Array`はすべての配列型の抽象基本型です。 暗黙の参照変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions)) から任意の配列型に存在する`System.Array`、および明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) が存在します。`System.Array`任意の配列型にします。 なお`System.Array`自体ではありません、 *array_type*します。 *Class_type*すべてから*array_type*s が派生します。

実行時の型の値に`System.Array`できる`null`または任意の配列型のインスタンスへの参照。

### <a name="arrays-and-the-generic-ilist-interface"></a>配列とジェネリックの IList インターフェイス

1 次元配列`T[]`インターフェイスを実装する`System.Collections.Generic.IList<T>`(`IList<T>`略して) とその基本インターフェイスです。 したがってからの暗黙的な変換がある`T[]`に`IList<T>`とその基本インターフェイスです。 さらからの暗黙的な参照変換がある場合`S`に`T`し`S[]`実装`IList<T>`から暗黙の参照変換があると`S[]`に`IList<T>`およびその基本インターフェイス ([暗黙の参照変換](conversions.md#implicit-reference-conversions))。 明示的な参照変換がある場合`S`に`T`からの明示的な参照変換は`S[]`に`IList<T>`とその基本インターフェイス ([明示的な参照変換](conversions.md#explicit-reference-conversions)). 例:
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

割り当て`lst2 = oa1`以降からの変換、コンパイル時エラーが発生`object[]`に`IList<string>`はいない暗黙的な明示的な変換をします。 キャスト`(IList<string>)oa1`が以降の実行時にスローされる例外が発生`oa1`参照、`object[]`および not、`string[]`します。 ただし、キャスト`(IList<string>)oa2`からスローされる例外は発生しません`oa2`参照、`string[]`します。

暗黙的または明示的な参照変換が存在する場合は`S[]`に`IList<T>`からの明示的な参照変換も`IList<T>`その基本インターフェイスと`S[]`([明示的な参照変換](conversions.md#explicit-reference-conversions))。

配列型`S[]`実装`IList<T>`、実装されたインターフェイスのメンバーの一部の例外をスローする可能性があります。 インターフェイスの実装の動作の詳細についてはこの仕様の範囲外です。

## <a name="array-creation"></a>配列の作成

配列のインスタンスがによって作成された*array_creation_expression*s ([配列作成式](expressions.md#array-creation-expressions)) フィールド、ローカル変数の宣言が含まれるかを*array_initializer*([配列初期化子](arrays.md#array-initializers))。

配列インスタンスが作成されるが確立されているランクと各次元の長さと、インスタンスの有効期間にわたって一定です。 つまり、既存の配列インスタンスのランクを変更することはできませんも、そのディメンションのサイズを変更することはできます。

配列型の配列インスタンスは常にです。 `System.Array`型が抽象型をインスタンス化することはできません。

によって作成された配列の要素*array_creation_expression*s は必ずが既定値に初期化 ([既定値](variables.md#default-values))。

## <a name="array-element-access"></a>配列要素へのアクセス

配列の要素が使用してアクセス*element_access*式 ([配列アクセス](expressions.md#array-access)) フォームの`A[I1, I2, ..., In]`ここで、`A`配列型とそれぞれの式を指定`Ix`が、型の式`int`、 `uint`、 `long`、 `ulong`、または 1 つ以上のこれらの型に暗黙的に変換できます。 配列要素へのアクセスの結果は、変数、インデックスによって選択されている配列要素つまりです。

使用して、配列の要素を列挙することができます、`foreach`ステートメント ([foreach ステートメント](statements.md#the-foreach-statement))。

## <a name="array-members"></a>配列メンバー

すべての配列型で宣言されたメンバーの継承、`System.Array`型。

## <a name="array-covariance"></a>配列の共変性

任意の 2 つの*reference_type*s`A`と`B`暗黙の参照変換の場合は、([暗黙の参照変換](conversions.md#implicit-reference-conversions)) または明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) から存在する`A`に`B`、配列型から、同じ変換が存在し、`A[R]`配列型に`B[R]`ここで、`R`は any指定された*rank_specifier* (ただし、配列の両方で同じ型)。 このリレーションシップと呼ばれる***配列の共変性***します。 配列の共変性であることを意味、配列型の値を`A[R]`実際には、配列型のインスタンスへの参照があります`B[R]`から暗黙の参照変換が存在する限り、`B`に`A`します。

参照型の配列の要素への代入には、配列の共変性によりにより許可されている型の配列の要素に割り当てられている値が実際には、実行時チェックが含まれます ([単純な代入](expressions.md#simple-assignment))。 例:
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

代入`array[i]`で、`Fill`メソッドは、実行時のチェックを実行できるようにするによって参照されるオブジェクトを暗黙的に含まれます`value`か`null`の実際の要素の型と互換性があるインスタンスまたは`array`. `Main`の最初の 2 つの呼び出し`Fill`失敗すると、3 つ目の呼び出しの原因が、`System.ArrayTypeMismatchException`への最初の割り当てを実行したときにスローされる`array[i]`。 例外が発生するため、ボックス化された`int`で格納することはできません、`string`配列。

配列の共変性は起こりませんの配列に*value_type*秒。 たとえば、変換が存在しないこと、`int[]`として扱う場合に、`object[]`します。

## <a name="array-initializers"></a>配列初期化子

フィールドの宣言で配列初期化子を指定することがあります ([フィールド](classes.md#fields))、ローカル変数の宣言 ([ローカル変数宣言](statements.md#local-variable-declarations)) を配列作成式 ([配列の作成式](expressions.md#array-creation-expressions))。

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

配列初期化子で囲まれた変数の初期化子のシーケンスから成る"`{`「と」`}`「トークンし、で区切られた」`,`"トークンです。 各変数の初期化子が式または、多次元配列、入れ子になった配列初期化子。

配列初期化子が使用されているコンテキストでは、初期化される配列の型を決定します。 配列作成式で配列型はすぐに、初期化子の前または配列初期化子式から推論されます。 フィールドまたは変数宣言では、配列型は、フィールドまたは宣言される変数の型です。 配列初期化子で使用する場合、フィールドまたは変数の宣言など。
```csharp
int[] a = {0, 2, 4, 6, 8};
```
等価な配列作成式を簡略にすることをお勧めします。
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

1 次元配列では、配列初期化子必要がありますの配列の要素の型と互換性のある代入である、式のシーケンスで構成されます。 式は、昇順に並べ替え、インデックス 0 位置にある要素で始まる配列の要素を初期化します。 配列初期化子式の数は、作成される配列インスタンスの長さを決定します。 たとえば、上記の配列初期化子を作成します、`int[]`長さ 5 のインスタンスとし、次の値を使用して、インスタンスを初期化します。
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

多次元配列、配列初期化子は、配列内のディメンションとしての入れ子のレベル数だけが必要です。 最も外側の入れ子レベルが最も左にあるディメンションに対応し、最も内側の入れ子レベルが最も右にあるディメンションに対応します。 配列の各次元の長さは配列初期化子に対応する入れ子のレベルにある要素の数によって決まります。 入れ子になった配列初期化子の場合、それぞれは、要素の数は同じレベルの他の配列初期化子と同じである必要があります。 例:
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
左端のディメンションの 5 つの長さと右端のディメンションの 2 つの長さでは、2 次元の配列を作成します。
```csharp
int[,] b = new int[5, 2];
```
初期化します、配列インスタンスで、次の値。
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

長さがゼロで、右端以外のディメンションを指定すると、後続のディメンションが長さがゼロにもと見なされます。 例:
```csharp
int[,] c = {};
```
長さが 0 の左端と右端のディメンションでは、2 次元の配列を作成します。
```csharp
int[,] c = new int[0, 0];
```

配列作成式には、明示的な次元の長さと配列初期化子の両方が含まれている場合は、長さは定数式である必要があり、各入れ子のレベルにある要素の数が、対応する次元の長さと一致する必要があります。 次にいくつかの例を示します。
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

ここでは、初期化子`y`ディメンションの長さの式の定数、および初期化子がないため、コンパイル時エラー結果`z`のため、コンパイル時エラーの結果の長さと要素の数、初期化子が一致しません。
