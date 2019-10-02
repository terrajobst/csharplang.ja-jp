---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704026"
---
# <a name="structs"></a>構造体

構造体は、データメンバーと関数メンバーを含むことができるデータ構造を表すという点でクラスに似ています。 ただし、クラスとは異なり、構造体は値型であり、ヒープ割り当ては必要ありません。 構造体型の変数は、構造体のデータを直接格納します。一方、クラス型の変数には、データへの参照 (後者はオブジェクトと呼ばれます) が含まれます。

構造体は、値セマンティクスを持つ小規模なデータ構造に特に便利です。 構造体の主な例としては、複素数、座標系のポイント、ディクショナリのキーと値のペアなどがあります。 これらのデータ構造にとって重要なのは、データメンバーが少ないこと、継承または参照 id を使用する必要がないこと、および割り当てによって参照の代わりに値がコピーされる値のセマンティクスを使用して便宜的に実装できることです。

「[単純型](types.md#simple-types)」で説明されているようC#に、によって提供される単純型 (`int`、`double`、`bool` など) は、実際にはすべての構造体型です。 これらの定義済みの型が構造体であるのと同様に、構造体と演算子のオーバーロードを使用して、 C#新しい "プリミティブ" 型を言語で実装することもできます。 このような型の2つの例については、この章の最後 ([構造体の例](structs.md#struct-examples)) を参照してください。

## <a name="struct-declarations"></a>構造体の宣言

*Struct_declaration*は、新しい構造体を宣言する*type_declaration* ([型宣言](namespaces.md#type-declarations)) です。

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

*Struct_declaration*は、省略可能な*属性*のセット ([属性](attributes.md)) の後にオプションの一連の*struct_modifier*s ([struct 修飾子](structs.md#struct-modifiers)) が続き、その後に省略可能な `partial` 修飾子が続きます。キーワード `struct` と、構造体に名前を付け、その後に省略可能な*type_parameter_list*仕様 ([型パラメーター](classes.md#type-parameters)) を指定する*識別子*、およびその後に省略可能な*struct_interfaces*仕様 ([Partial修飾子](structs.md#partial-modifier)) の後に省略可能な*type_parameter_constraints_clause*s 仕様 ([型パラメーターの制約](classes.md#type-parameter-constraints)) を指定し、続けて*struct_body* ([構造体の本体](structs.md#struct-body)) を入力します。その後にセミコロンを続けます。

### <a name="struct-modifiers"></a>Struct 修飾子

*Struct_declaration*には、必要に応じて構造体の修飾子のシーケンスを含めることができます。

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

同じ修飾子が struct 宣言で複数回出現する場合、コンパイル時エラーになります。

構造体宣言の修飾子は、クラス宣言 ([クラス宣言](classes.md#class-declarations)) と同じ意味を持ちます。

### <a name="partial-modifier"></a>Partial 修飾子

@No__t-0 修飾子は、この*struct_declaration*が部分型の宣言であることを示します。 外側の名前空間または型宣言内で同じ名前を持つ複数の部分構造体宣言を組み合わせると、[部分型](classes.md#partial-types)で指定された規則に従って1つの構造体宣言が形成されます。

### <a name="struct-interfaces"></a>構造体のインターフェイス

Struct 宣言には*struct_interfaces*仕様を含めることができます。この場合、構造体は、指定されたインターフェイス型を直接実装すると言います。

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

インターフェイスの実装については、「[インターフェイスの実装](interfaces.md#interface-implementations)」で詳しく説明します。

### <a name="struct-body"></a>構造体の本体

構造体の*struct_body*は、構造体のメンバーを定義します。

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>構造体のメンバー

構造体のメンバーは、 *struct_member_declaration*によって導入されたメンバーと、その @no__t 型から継承されたメンバーで構成されます-1。

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

[クラスと構造体の違い](structs.md#class-and-struct-differences)については、[反復子](classes.md#iterators)を通じて[クラスメンバー](classes.md#class-members)に指定されたクラスメンバーの説明も、構造体のメンバーに適用されます。

## <a name="class-and-struct-differences"></a>クラスと構造体の違い

構造体は、いくつかの重要な点でクラスとは異なります。

*  構造体は値型 ([値のセマンティクス](structs.md#value-semantics)) です。
*  すべての構造体型は、クラス `System.ValueType` ([継承](structs.md#inheritance)) を暗黙的に継承します。
*  構造体型の変数への代入では、代入される値のコピーが作成されます ([代入](structs.md#assignment))。
*  構造体の既定値は、すべての値の型フィールドを既定値に設定し、すべての参照型フィールドを `null` ([既定値](structs.md#default-values)) に設定することによって生成される値です。
*  ボックス化およびボックス化解除の操作は、構造体型と `object` ([ボックス化およびボックス化解除](structs.md#boxing-and-unboxing)) の間の変換に使用されます。
*  @No__t-0 の意味は、構造体 ([このアクセス](expressions.md#this-access)) では異なります。
*  構造体のインスタンスフィールド宣言には、変数初期化子 ([フィールド初期化子](structs.md#field-initializers)) を含めることはできません。
*  構造体は、パラメーターなしのインスタンスコンストラクター ([コンストラクター](structs.md#constructors)) の宣言を許可されていません。
*  構造体はデストラクター ([デストラクター](structs.md#destructors)) の宣言を許可されていません。

### <a name="value-semantics"></a>値のセマンティクス

構造体は値型 (値[型](types.md#value-types)) であり、値のセマンティクスがあると言います。 一方、クラスは参照型 ([参照型](types.md#reference-types)) であり、参照セマンティクスがあると言います。

構造体型の変数は、構造体のデータを直接格納します。一方、クラス型の変数には、データへの参照 (後者はオブジェクトと呼ばれます) が含まれます。 構造体 `B` に @no__t 型のインスタンスフィールドが含まれており、`A` が構造体型である場合、`A` は `B` または @no__t から構築された型に依存します。 構造体 `X` は、`X` が @no__t 型のインスタンスフィールドを格納している場合は、構造体 `Y`***に直接依存***します。 この定義により、構造体が依存する構造体の完全なセットは、リレーションシップ***に直接依存***します。  次に例を示します。
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
`Node` に独自の型のインスタンスフィールドが含まれているため、エラーが発生します。  別の例
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
は、`A`、`B`、@no__t の各型が互いに依存しているため、エラーになります。

クラスを使用すると、2つの変数が同じオブジェクトを参照する可能性があるため、1つの変数に対する操作が、もう一方の変数によって参照されるオブジェクトに影響を与える可能性があります。 構造体を使用すると、変数にはそれぞれ独自のデータコピーが含まれます (`ref` および `out` パラメーター変数の場合を除きます)。また、1つの変数に対して操作を行うことはできません。 さらに、構造体は参照型ではないため、構造体型の値を @no__t 0 にすることはできません。

宣言を指定します。
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
コード片
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
値 `10` に出力します。 @No__t-0 から `b` への割り当てによって値のコピーが作成されるため、`b` は `a.x` への割り当ての影響を受けません。 @No__t-0 はクラスとして宣言されていましたが、`a` と `b` が同じオブジェクトを参照するため、出力は-1 @no__t ます。

### <a name="inheritance"></a>継承

すべての構造体型は、クラス `System.ValueType` から暗黙的に継承します。このクラスは、クラス `object` から継承されます。 構造体の宣言では実装されたインターフェイスのリストを指定できますが、構造体の宣言で基底クラスを指定することはできません。

構造体型は抽象ではなく、常に暗黙的にシールされます。 したがって、`abstract` および `sealed` 修飾子は、構造体宣言では許可されません。

継承は構造体ではサポートされていないため、構造体メンバーの宣言されたアクセシビリティを @no__t 0 または `protected internal` にすることはできません。

構造体の関数メンバーを @no__t 0 または `virtual` にすることはできません。また、`override` 修飾子は、`System.ValueType` から継承されたメソッドをオーバーライドする場合にのみ使用できます。

### <a name="assignment"></a>代入

構造体型の変数への代入では、割り当てられている値のコピーが作成されます。 これは、参照によって識別されるオブジェクトではなく、参照をコピーするクラス型の変数への代入とは異なります。

代入と同様に、構造体が値パラメーターとして渡される場合や、関数メンバーの結果として返される場合は、構造体のコピーが作成されます。 構造体は、`ref` または `out` パラメーターを使用して関数メンバーに参照渡しできます。

構造体のプロパティまたはインデクサーが割り当てのターゲットである場合、プロパティまたはインデクサーアクセスに関連付けられたインスタンス式は、変数として分類される必要があります。 インスタンス式が値として分類されている場合は、コンパイル時エラーが発生します。 詳細については、「[単純な割り当て](expressions.md#simple-assignment)」を参照してください。

### <a name="default-values"></a>既定の値

「[既定値](variables.md#default-values)」で説明されているように、いくつかの種類の変数は、作成時に自動的に既定値に初期化されます。 クラス型およびその他の参照型の変数の場合、この既定値は `null` です。 ただし、構造体は値型であり、0 @no__t できないため、構造体の既定値は、すべての値の型フィールドを既定値に設定し、すべての参照型フィールドを `null` に設定することによって生成される値です。

上で宣言された @no__t 0 構造体を参照しています。例
```csharp
Point[] a = new Point[100];
```
配列内の各 `Point` を、`x` および `y` フィールドを0に設定することによって生成される値に初期化します。

構造体の既定値は、構造体の既定のコンストラクターによって返される値に対応します ([既定のコンストラクター](types.md#default-constructors))。 クラスとは異なり、構造体では、パラメーターなしのインスタンスコンストラクターを宣言することはできません。 代わりに、すべての構造体には、暗黙的にパラメーターなしのインスタンスコンストラクターがあります。このコンストラクターは、すべての値型フィールドを既定値に設定し、すべての参照型フィールドを `null` に設定した結果の値を常に返します。

構造体は、既定の初期化状態が有効な状態であると見なすように設計する必要があります。 この例では、
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
ユーザー定義のインスタンスコンストラクターは、明示的に呼び出された場合にのみ null 値を保護します。 @No__t 0 の変数が既定値の初期化の対象となる場合、`key` フィールドと `value` フィールドは null になり、構造体はこの状態を処理できるように準備する必要があります。

### <a name="boxing-and-unboxing"></a>ボックス化とボックス化解除

クラス型の値は、コンパイル時に別の型として参照を扱うことによって、型 `object` またはクラスによって実装されるインターフェイス型に変換できます。 同様に、型 `object` またはインターフェイス型の値は、参照を変更せずにクラス型に変換できます (この場合は、実行時の型チェックが必要です)。

構造体は参照型ではないため、これらの操作は構造体型に対して異なる方法で実装されます。 構造体型の値が型 `object` または構造体によって実装されるインターフェイス型に変換されると、ボックス化操作が行われます。 同様に、型 `object` またはインターフェイス型の値が構造体型に変換されると、ボックス化解除操作が行われます。 クラス型に対する同じ操作との主な違いは、ボックス化とボックス化解除によって、ボックス化されたインスタンスの内外に構造体の値がコピーされることです。 したがって、ボックス化またはボックス化解除の後に、ボックス化解除された構造体に加えられた変更は、ボックス化された構造体に反映されません。

@No__t-0 (`Equals`、`GetHashCode`、または `ToString` など) から継承された仮想メソッドをオーバーライドする場合、構造体型のインスタンスを介して仮想メソッドを呼び出すと、ボックス化が発生しません。 これは、構造体が型パラメーターとして使用されていて、型パラメーター型のインスタンスを通じて呼び出しが行われる場合にも当てはまります。 以下に例を示します。
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

プログラムの出力は次のようになります。
```console
1
2
3
```

@No__t-0 で副作用が発生するのは不適切なスタイルですが、この例では `x.ToString()` の3回の呼び出しに対してボックス化が実行されていないことを示しています。

同様に、制約された型パラメーターのメンバーにアクセスするときに、ボックス化が暗黙的に行われることはありません。 たとえば、@no__t 0 のインターフェイスに、値を変更するために使用できるメソッド `Increment` が含まれているとします。 @No__t-0 が制約として使用されている場合、`Increment` メソッドの実装は、`Increment` が呼び出された変数への参照を使用して呼び出され、ボックス化されたコピーではありません。

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

@No__t-0 の最初の呼び出しでは、変数 `x` の値が変更されます。 これは `Increment` への2回目の呼び出しと同じではありません。この場合、`x` のボックス化されたコピーの値が変更されます。 このため、プログラムの出力は次のようになります。
```console
0
1
1
```

ボックス化とボックス化解除の詳細については、「[ボックス化とボックス化解除](types.md#boxing-and-unboxing)」を参照してください。

### <a name="meaning-of-this"></a>意味

クラスのインスタンスコンストラクターまたはインスタンス関数メンバー内では、@no__t 0 が値として分類されます。 したがって、`this` を使用して、関数メンバーが呼び出されたインスタンスを参照することはできますが、クラスの関数メンバーの `this` に割り当てることはできません。

構造体のインスタンスコンストラクター内で `this` は構造体型の `out` パラメーターに対応します。また、構造体のインスタンス関数メンバー内では、@no__t は、構造体型の `ref` パラメーターに対応します。 どちらの場合も、@no__t 0 は変数として分類され、`this` に代入するか、これを `ref` または `out` パラメーターとして渡すことによって、関数メンバーが呼び出された構造体全体を変更することができます。

### <a name="field-initializers"></a>フィールド初期化子

「[既定値](structs.md#default-values)」で説明されているように、構造体の既定値は、すべての値の型フィールドを既定値に設定し、すべての参照型フィールドを `null` に設定した結果の値で構成されます。 このため、構造体では、インスタンスフィールドの宣言に変数初期化子を含めることはできません。 この制限は、インスタンスフィールドにのみ適用されます。 構造体の静的フィールドには、変数初期化子を含めることができます。

例
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
インスタンスフィールドの宣言に変数初期化子が含まれているため、エラーが発生しています。

### <a name="constructors"></a>コンストラクター

クラスとは異なり、構造体では、パラメーターなしのインスタンスコンストラクターを宣言することはできません。 代わりに、すべての構造体には、暗黙的にパラメーターなしのインスタンスコンストラクターがあります。このコンストラクターは、すべての値型フィールドを既定値に設定し、すべての参照型フィールドを null ([既定のコンストラクター](types.md#default-constructors)) に設定した結果の値を常に返します。 構造体は、パラメーターを持つインスタンスコンストラクターを宣言できます。 次に例を示します。
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

上記の宣言を指定した場合、ステートメントは
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
どちらも、`x`、@no__t が0に初期化された @no__t 0 を作成します。

構造体インスタンスコンストラクターには、フォームのコンストラクター初期化子を含めることはできません `base(...)`。

Struct インスタンスコンストラクターがコンストラクター初期化子を指定していない場合、@no__t 0 変数は構造体型の `out` パラメーターに対応し、`out` パラメーターと同様に、`this` は確実に代入される必要があります ([明確な代入](variables.md#definite-assignment)) を指定します。 Struct インスタンスコンストラクターでコンストラクター初期化子が指定されている場合、@no__t 0 変数は構造体型の `ref` パラメーターに対応し、`ref` パラメーターと同様に、`this` はコンストラクター本体のエントリで確実に割り当てられていると見なされます. 次に示すインスタンスコンストラクターの実装について考えてみましょう。
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

構築されている構造体のすべてのフィールドが確実に代入されるまで、インスタンスメンバー関数 (プロパティ `X` および `Y` を含む) を呼び出すことはできません。 唯一の例外には、自動的に実装されるプロパティ ([自動的に実装](classes.md#automatically-implemented-properties)されるプロパティ) が含まれます。 明確な代入規則 ([単純代入式](variables.md#simple-assignment-expressions)) は、その構造体型のインスタンスコンストラクター内の構造体型の自動プロパティへの割り当てを明示的に除外します。このような代入は、隠ぺいされたの割り当てと見なされます。自動プロパティのバッキングフィールド。 したがって、次のものを使用できます。

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a>デストラクター

構造体は、デストラクターの宣言を許可されていません。

### <a name="static-constructors"></a>静的コンストラクター

構造体の静的コンストラクターは、クラスの場合と同じ規則のほとんどに従います。 構造体型の静的コンストラクターの実行は、アプリケーションドメイン内で発生する次のイベントの最初のイベントによってトリガーされます。

*  構造体型の静的メンバーが参照されています。
*  構造体型の明示的に宣言されたコンストラクターが呼び出されます。

構造体型の既定値 ([既定値](structs.md#default-values)) を作成しても、静的コンストラクターはトリガーされません。 (例として、配列内の要素の初期値が挙げられます)。

## <a name="struct-examples"></a>構造体の例

次に、`struct` 型を使用して、定義済みの言語の型と同様に使用できる型を作成する2つの大きな例を示しますが、セマンティクスは変更されています。

### <a name="database-integer-type"></a>データベースの整数型

次の @no__t 0 構造体は、`int` 型の値の完全なセットと、不明な値を示す追加の状態を表すことができる整数型を実装します。 これらの特性を持つ型は、データベースで一般的に使用されます。

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a>データベースのブール型

次の @no__t 0 構造体は、3つの値を持つ論理型を実装します。 この型に指定できる値は、0、`DBBool.False`、および `DBBool.Null` の @no__t です。ここで、`Null` のメンバーは不明な値を示します。 このような3つの値の論理型は、データベースで一般的に使用されます。

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
