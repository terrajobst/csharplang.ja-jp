---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229606"
---
# <a name="structs"></a>構造体

データ メンバーおよび関数メンバーを格納できるデータ構造を表しているという点では、構造体をクラスに似ています。 ただし、クラスと異なり、構造体は値型で、ヒープ割り当ては必要ありません。 構造体の型の変数は、クラス型の変数にデータをオブジェクトとして知らへの参照が含まれていますが、構造体のデータを直接格納します。

構造体は、値セマンティクスを持つ小規模なデータ構造に特に便利です。 構造体の主な例としては、複素数、座標系のポイント、ディクショナリのキーと値のペアなどがあります。 これらのデータ構造のキーは必要があるいくつかのデータ メンバー、継承や参照の id の使用が必要としないことと、簡単に実装できる値セマンティクスを使用して、割り当てが、参照の代わりに値をコピーします。

」の説明に従って[単純型](types.md#simple-types)、単純型など c# の場合は、によって提供される`int`、 `double`、および`bool`、実際にはすべての構造体型。 これらの定義済みの型が構造体であるのと同様にも構造体と c# 言語で新しい「プリミティブ」型を実装するためにオーバー ロードする演算子を使用可能です。 この章の最後にこのような型の 2 つの例が与えられます ([構造体の例](structs.md#struct-examples))。

## <a name="struct-declarations"></a>構造体の宣言

A *struct_declaration*は、 *type_declaration* ([入力宣言](namespaces.md#type-declarations)) 新しい構造体を宣言します。

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

A *struct_declaration*オプションのセットから成る*属性*([属性](attributes.md)) オプションのセットと、その後*struct_modifier*s ([構造体修飾子](structs.md#struct-modifiers)) と省略可能なその後`partial`の後に、キーワード修飾子`struct`と*識別子*後に、構造体の名前を示す、省略可能な*type_parameter_list*仕様 ([パラメーター入力](classes.md#type-parameters)) と省略可能なその後*struct_interfaces*仕様 ([Partial 修飾子](structs.md#partial-modifier))) と省略可能なその後*type_parameter_constraints_clause*s 仕様 ([パラメーターの制約入力](classes.md#type-parameter-constraints))、その後に、 *struct_body* ([構造体の本体](structs.md#struct-body))、セミコロンで必要に応じてその後にします。

### <a name="struct-modifiers"></a>構造体の修飾子

A *struct_declaration*構造体の修飾子のシーケンスを必要に応じて含めることができます。

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

同じ修飾子を構造体宣言内で複数回のコンパイル時エラーになります。

構造体の宣言の修飾子がクラス宣言のものと同じ意味を持ちます ([クラス クラスの宣言](classes.md#class-declarations))。

### <a name="partial-modifier"></a>Partial 識別子

`partial`修飾子には、ことを示しますこの*struct_declaration*部分型の宣言です。 指定された規則に従って、それを囲む名前空間または型の宣言内に同じ名前を持つ複数の部分的な構造体宣言は、1 つの構造体の宣言を形成する結合、[部分型](classes.md#partial-types)します。

### <a name="struct-interfaces"></a>構造体のインターフェイス

構造体の宣言を含めることができます、 *struct_interfaces*仕様、構造体のケースが特定のインターフェイス型を直接実装すると呼ばれます。

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

インターフェイスの実装については、説明でさらに[インターフェイスの実装](interfaces.md#interface-implementations)します。

### <a name="struct-body"></a>構造体の本文

*Struct_body*構造体の構造体のメンバーを定義します。

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>構造体のメンバー

構造体のメンバーで導入されたメンバーで構成されているその*struct_member_declaration*s およびメンバーの型から継承`System.ValueType`します。

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

記載の相違点を除いて[クラスと構造体の違い](structs.md#class-and-struct-differences)で提供されるクラスのメンバーの説明については、[クラス メンバー](classes.md#class-members)を通じて[反復子](classes.md#iterators)構造体に適用されますメンバーでも使用します。

## <a name="class-and-struct-differences"></a>クラスと構造体の違い

構造体は、いくつかの重要な方法でクラスとは異なります。

*  構造体は、値型 ([値セマンティクス](structs.md#value-semantics))。
*  すべての構造体型は暗黙的にクラスから継承`System.ValueType`([継承](structs.md#inheritance))。
*  構造体の型の変数への代入が割り当てられている値のコピーを作成します ([割り当て](structs.md#assignment))。
*  構造体の既定値は、すべての値型フィールドが既定値をすべて参照型フィールドに設定された値`null`([既定値](structs.md#default-values))。
*  ボックス化とボックス化解除の操作を使用して、構造体型の間で変換および`object`([ボックス化とボックス化解除](structs.md#boxing-and-unboxing))。
*  意味`this`は構造体の異なる ([このアクセス](expressions.md#this-access))。
*  変数の初期化子を含めるには構造体のインスタンス フィールドの宣言は許可されていません ([フィールド初期化子](structs.md#field-initializers))。
*  パラメーターなしインターフェイス コンス トラクターを宣言する構造体は許可されていません ([コンス トラクター](structs.md#constructors))。
*  構造体がデストラクターを宣言することはできない ([デストラクター](structs.md#destructors))。

### <a name="value-semantics"></a>値のセマンティクス

構造体は、値型 ([値の型](types.md#value-types)) と値のセマンティクスを持つユーザーと呼びます。 一方で、クラスでは、参照型 ([参照型](types.md#reference-types)) と、参照セマンティクスを持つと言います。

構造体の型の変数は、クラス型の変数にデータをオブジェクトとして知らへの参照が含まれていますが、構造体のデータを直接格納します。 構造体と`B`型のインスタンス フィールドが含まれています`A`と`A`、構造体型は、コンパイル時エラーには`A`に依存する`B`から構築された型または`B`します。 構造体`X`***に直接依存***構造体`Y`場合`X`型のインスタンス フィールドを含む`Y`します。 この定義を指定するには、構造体が依存している構造体の完全なセットが推移的閉包、***に直接依存***リレーションシップ。  次に例を示します。
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
エラーは、ため`Node`独自の型のインスタンス フィールドが含まれています。  別の例
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
ために、エラーの種類ごと`A`、`B`と`C`互いに依存しています。

クラスは 2 つの変数が同じオブジェクトを参照できるため、他の変数によって参照されるオブジェクトに影響を与える 1 つの変数に対する操作。 構造体では、各変数がある独自のデータのコピー (の場合を除く`ref`と`out`パラメーター変数) とに影響を与える、他のいずれかの操作にことはできません。 さらに、構造体は参照型ではないため、ことはできませんを構造体の型の値の`null`します。

宣言について考えます。
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
コード フラグメントは、
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
値を出力します`10`します。 割り当て`a`に`b`、値のコピーを作成および`b`はそのためへの割り当てによって影響を受けません`a.x`。 `Point`されて代わりに出力になりますが、クラスとして宣言すると、`100`ため`a`と`b`同じオブジェクトを参照します。

### <a name="inheritance"></a>継承

すべての構造体型は暗黙的にクラスから継承`System.ValueType`であり、さらに、クラスを継承`object`します。 構造体の宣言が実装されるインターフェイスの一覧を指定できますが、基底クラスを指定する構造体の宣言のことはできません。

構造体の型は、抽象されないしは常に暗黙的にシールします。 `abstract`と`sealed`構造体の宣言で 修飾子は使用できないためです。

構造体メンバーの宣言されたアクセシビリティをすることはできませんので、構造体の継承がサポートされていない`protected`または`protected internal`します。

構造体の関数メンバーにすることはできません`abstract`または`virtual`、および`override`のみから継承されたメソッドをオーバーライドするのには、修飾子を使用してください。`System.ValueType`します。

### <a name="assignment"></a>代入

構造体の型の変数への代入では、割り当てられている値のコピーを作成します。 これは、コピー、参照が参照によって識別されるオブジェクトではなく、クラスの型の変数に割り当てと異なります。

構造体の値を持つパラメーターとして渡されるまたは関数メンバーの結果として返されるときに、割り当てと同様に、構造体のコピーが作成されます。 構造体を使用して関数のメンバーへの参照で渡すことが、`ref`または`out`パラメーター。

プロパティまたはインデクサーの構造体には、代入式のターゲットがある場合は、プロパティまたはインデクサーのアクセスに関連付けられたインスタンス式を変数として分類される必要があります。 インスタンス式が値として分類は、コンパイル時エラーが発生します。 さらに詳しく記載されて[単純な代入](expressions.md#simple-assignment)します。

### <a name="default-values"></a>既定の値

」の説明に従って[既定値](variables.md#default-values)、ユーザーが作成されるときでいくつかの種類の変数は、既定値に自動的に初期化します。 クラスの型および他の参照型の変数は、この既定値は`null`します。 ただし、構造体は、値の型にできないため`null`、構造体の既定値は、すべての値型フィールドが既定値をすべて参照型フィールドに設定された値`null`します。

参照する、`Point`上、例では、構造体で宣言されました。
```csharp
Point[] a = new Point[100];
```
それぞれを初期化します`Point`設定された値の配列内の`x`と`y`フィールドをゼロにします。

構造体の既定値は、構造体の既定のコンス トラクターによって返される値に対応 ([既定のコンス トラクター](types.md#default-constructors))。 クラスとは異なり、パラメーターなしコンス トラクターを宣言する構造体は許可されていません。 暗黙的に常に結果が既定値をすべて参照型フィールドを値型のすべてのフィールドを設定の値を返すパラメーターなしインターフェイス コンス トラクターを代わりに、各構造体`null`します。

構造体を設計すると、既定の初期化状態に有効な状態を考慮する必要があります。 例
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
ユーザー定義のインスタンス コンス トラクターのみ呼び出される場所に明示的に null 値を防ぎます。 場合で、`KeyValuePair`変数は、既定値の初期化の対象は、`key`と`value`フィールドが null をして、構造体は、この状態を処理するために準備する必要があります。

### <a name="boxing-and-unboxing"></a>ボックス化とボックス化解除

クラス型の値を型に変換できます`object`またはコンパイル時に別の種類として、参照を扱うだけで、クラスによって実装されているインターフェイス型。 型の値を同様に、`object`またはインターフェイス型の値は、参照を変更することがなくクラス型に変換できます (ただしもちろん実行時の型チェックが必要なこの場合)。

構造体が参照型ではないために、構造体の型をこれらの操作が異なる方法で実装されます。 構造体の型の値を型に変換するときに`object`または構造体によって実装されるインターフェイス型にボックス化操作が実行されます。 同様に、型の値と`object`またはインターフェイス型の値が構造体の型に変換、ボックス化解除の操作が行われています。 クラス型に対して同じ操作からの重要な違いは、ボックス化とボックス化解除がコピーされる構造体の値にするか、ボックス化されたインスタンスからです。 そのため、ボックス化およびボックス化解除操作では、次のボックス化解除された構造体に加えられた変更は反映されませんでボックス化された構造体。

構造体の型がから継承された仮想メソッドをオーバーライドするときに`System.Object`(など`Equals`、 `GetHashCode`、または`ToString`)、構造体の型のインスタンス経由で仮想メソッドの呼び出しが発生するボックス化は発生しません。 これは、構造体は型パラメーターとして使用され、型パラメーターの型のインスタンスを通じて、呼び出しが発生した場合でも当てはまります。 例:
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

プログラムの出力は次のとおりです。
```
1
2
3
```

不適切なスタイルの`ToString`副作用がある、この例の 3 つの呼び出しのボックス化が発生しなかったこと`x.ToString()`します。

同様に、決して暗黙的にボックス化は、制約付きの型パラメーターのメンバーにアクセスするときに発生します。 たとえば、インターフェイス`ICounter`メソッドを含む`Increment`値の変更に使用できます。 場合`ICounter`の実装、制約として使用、`Increment`メソッドを呼び出すと、変数への参照を`Increment`で、ボックス化されたコピーではありませんが呼び出されました。

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

最初の呼び出し`Increment`変数に値を変更します`x`します。 これは、2 番目の呼び出しに相当しない`Increment`、ボックス化されたコピーの値が変更されます`x`します。 そのため、プログラムの出力は次のとおりです。
```
0
1
1
```

ボックス化とボックス化解除の詳細については、[ボックス化とボックス化解除](types.md#boxing-and-unboxing)を参照してください。

### <a name="meaning-of-this"></a>この意味

インスタンス コンス トラクターまたはクラスの関数メンバーのインスタンス内で`this`値として分類されます。 そのため、while `this` 、インスタンスを参照するために使用できますを関数メンバーの呼び出しのことはできませんに割り当てる`this`クラスの関数メンバーで。

構造体のインスタンス コンス トラクター内で`this`に対応、`out`パラメーターと、構造体のインスタンス関数メンバー内で、構造体型の`this`に対応、`ref`構造体の型のパラメーター。 どちらの場合も、`this`変数として分類されますを割り当てることによって呼び出された関数のメンバーを構造体全体を変更することも、`this`として渡すことにより、`ref`または`out`パラメーター。

### <a name="field-initializers"></a>フィールド初期化子

」の説明に従って[既定値](structs.md#default-values)、構造体の既定値が既定値をすべて参照型フィールドを値型のすべてのフィールドを設定から結果の値から成る`null`します。 このため、構造体を変数初期化子を含めるインスタンス フィールドの宣言は許可されません。 この制限は、インスタンス フィールドにのみ適用されます。 構造体の静的フィールドは、変数の初期化子を含める許可されます。

例では、
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
エラーが、インスタンス フィールドの宣言が変数初期化子が含まれるためです。

### <a name="constructors"></a>コンストラクター

クラスとは異なり、パラメーターなしコンス トラクターを宣言する構造体は許可されていません。 暗黙的に常に値型のすべてのフィールドが既定値をすべて参照型フィールドを null に設定から結果値を返すパラメーターなしインターフェイス コンス トラクターを代わりに、各構造体 ([既定のコンス トラクター](types.md#default-constructors)). 構造体には、パラメーターを持つインスタンス コンス トラクターを宣言できます。 次に例を示します。
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

上記の宣言ステートメント
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
両方を作成、`Point`で`x`と`y`0 に初期化します。

構造体のインスタンス コンス トラクターは、フォームのコンス トラクター初期化子を含めるには許可されていません`base(...)`します。

構造体のインスタンス コンス トラクターは、コンス トラクター初期化子を指定しない場合、`this`変数に対応して、`out`類似しています、構造体の型のパラメーター、`out`パラメーター、 `this` (を間違いなく割り当てする必要があります[確実な代入](variables.md#definite-assignment)) が、コンス トラクターを返すすべての場所にします。 構造体のインスタンス コンス トラクターは、コンス トラクター初期化子を指定する場合、`this`変数に対応して、`ref`類似しています、構造体の型のパラメーターを`ref`パラメーター、`this`で確実に割り当てられていると見なされますコンス トラクター本体のエントリ。 インスタンス コンス トラクターの実装を検討してください。
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

インスタンス メンバー関数 (プロパティの set アクセサーを含む`X`と`Y`) 構築される構造体のすべてのフィールドが必ず割り当てられるまでに呼び出すことができます。 唯一の例外が自動的に実装されたプロパティが含まれます ([自動実装プロパティ](classes.md#automatically-implemented-properties))。 確実な代入ルール ([単純な代入式](variables.md#simple-assignment-expressions)) 具体的には、インスタンス コンス トラクター内でその構造体の型の構造体型の自動プロパティへの割り当てを除外します。 このような割り当ては、確実なと見なされます。自動プロパティの非表示のバッキング フィールドの割り当て。 したがって、次は使用できます。

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

構造体は、デストラクターを宣言は許可されません。

### <a name="static-constructors"></a>静的コンストラクター

構造体の静的コンス トラクターでは、ほとんどのクラスと同じ規則に従います。 構造体の型の静的コンス トラクターの実行は、次のイベントの最初のアプリケーション ドメイン内に発生するによってトリガーされます。

*  構造体の型の静的メンバーが参照されます。
*  構造体の型の明示的に宣言されたコンス トラクターが呼び出されます。

既定値の作成 ([既定値](structs.md#default-values)) 構造体の型が開始されない、静的コンス トラクター。 (この例は、配列内の要素の初期値です)。

## <a name="struct-examples"></a>構造体の例

使用して 2 つの重要な例を次に示します`struct`同様に、言語が変更されたセマンティクスを持つ定義済みの型を使用できる型を作成する型。

### <a name="database-integer-type"></a>データベースの整数型

`DBInt`下構造体の値の完全なセットを表すことのできる整数型の実装、`int`型宣言および不明な値を示す追加の状態。 これらの特性を持つ型がデータベースでよく使用されます。

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

`DBBool`下構造体は、3 値論理の一種を実装します。 この型の値には`DBBool.True`、 `DBBool.False`、および`DBBool.Null`、場所、`Null`メンバーが不明な値を示します。 このような 3 つの値の論理型は、データベースでよく使用されます。

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
