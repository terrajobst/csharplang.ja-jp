---
ms.openlocfilehash: a8ad8a8b3eda1d00fa745bd92e4371eacc36b79f
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229737"
---
# <a name="attributes"></a>属性

C# 言語の多くは、プログラマは、プログラムで定義されたエンティティの宣言型の情報を指定できます。 修飾して、クラスのメソッドのアクセシビリティを指定するなど、 *method_modifier*s `public`、 `protected`、 `internal`、および`private`します。

C# では、新しい種類の宣言型の情報を作成***属性***します。 プログラマは、さまざまなプログラム エンティティに属性を追加し、実行時環境での属性情報を取得します。 たとえば、フレームワークは、定義、`HelpAttribute`属性にこれらのプログラム要素からそのドキュメントへのマッピングを提供する (クラスとメソッド) などの特定のプログラム要素に配置できます。

属性は属性クラスの宣言によって定義されます ([属性クラス](attributes.md#attribute-classes))、位置指定引数と名前付きパラメーターである可能性があります ([位置指定パラメーターと名前付きパラメーター](attributes.md#positional-and-named-parameters))。 属性の仕様を使用して c# プログラム内のエンティティに関連付けられている属性 ([属性の指定](attributes.md#attribute-specification))、属性のインスタンスとして実行時に取得することができます ([属性インスタンス](attributes.md#attribute-instances))。

## <a name="attribute-classes"></a>属性クラス

抽象クラスから派生するクラスを`System.Attribute`、直接的または間接的にするかどうかは、***属性クラス***します。 属性クラスの宣言定義の種類を新しい***属性***宣言でに配置することができます。 サフィックスを持つ属性クラスの名前は慣例により、`Attribute`します。 属性の使用は可能性があります含めるか、このサフィックスを省略します。

### <a name="attribute-usage"></a>属性の使用方法

属性`AttributeUsage`([、AttributeUsage 属性](attributes.md#the-attributeusage-attribute)) 属性クラスの使用方法について説明するために使用します。

`AttributeUsage` 位置指定パラメーターがあります ([位置指定パラメーターと名前付きパラメーター](attributes.md#positional-and-named-parameters)) 宣言を使用できますの種類を指定する属性クラスを有効にします。 例では、

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

という名前の属性クラスを定義します`SimpleAttribute`上に配置する*class_declaration*s と*interface_declaration*にのみです。 例では、

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

いくつか示されて、`Simple`属性。 この属性は名前で定義されて`SimpleAttribute`、この属性を使用する場合、`Attribute`サフィックスは省略できます。 その結果、短い名前`Simple`します。 したがって、上記の例は、次の意味と同等です。

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

`AttributeUsage` 名前付きパラメーターがあります ([位置指定パラメーターと名前付きパラメーター](attributes.md#positional-and-named-parameters)) と呼ばれる`AllowMultiple`、特定のエンティティの属性を複数回指定できるかどうかを示します。 場合`AllowMultiple`属性クラスが true の場合は、その属性クラス、***属性を複数使用クラス***エンティティで複数回指定できます。 場合`AllowMultiple`属性クラスは常に false かは指定しませんし、その属性クラスは、***単一使用属性クラス***エンティティに最大で 1 回指定できます。

例では、

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

という名前の属性を複数使用クラスを定義します。`AuthorAttribute`します。 例では、

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

2 つの用途をクラス宣言を示しています、`Author`属性。

`AttributeUsage` 呼ばれる別の名前付きパラメーターを持つ`Inherited`、基底クラスで指定する場合は、属性がその基底クラスから派生したクラスによって継承もかどうかを示します。 場合`Inherited`属性クラスが true の場合、その属性が継承されます。 場合`Inherited`属性クラスは常に false し、その属性は継承されません。 指定されていない場合、既定値は true です。

属性クラス`X`がない、`AttributeUsage`ように、属性がアタッチされています。

```csharp
using System;

class X: Attribute {...}
```

次のと同等です。

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a>位置指定引数と名前付きパラメーター

属性クラスが持つことができます***位置指定パラメーター***と***名前付きパラメーター***します。 属性クラスの各パブリック インスタンス コンス トラクターは、その属性クラスの位置指定パラメーターの有効なシーケンスを定義します。 各静的でないパブリックの読み取り/書き込みフィールドと、属性クラスのプロパティは、属性クラスの名前付きパラメーターを定義します。

例では、

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

という名前の属性クラスを定義します`HelpAttribute`1 つの位置指定パラメーターを持つ`url`と 1 つの名前付きパラメーター`Topic`します。 Non-static および public プロパティが`Url`読み取り/書き込みではないために、名前付きのパラメーターを定義しません。

この属性クラスを次のように使用する可能性があります。

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a>属性パラメーターの型

属性クラスの位置と名前付きパラメーターの型に制限されます、***属性パラメーター型***は。

*  次の種類のいずれか: `bool`、 `byte`、 `char`、 `double`、 `float`、 `int`、 `long`、 `sbyte`、 `short`、 `string`、 `uint`、 `ulong`、`ushort`.
*  型 `object`。
*  型 `System.Type`。
*  パブリック アクセシビリティし、では、入れ子になっている (該当する場合) の型もパブリック アクセシビリティを持つ列挙型が提供される ([属性の指定](attributes.md#attribute-specification))。
*  上記の種類の 1 次元配列。
*  コンス トラクター引数またはこれらの型の 1 つがパブリック フィールドは、属性の指定で位置指定か名前付きパラメーターとして使用できません。

## <a name="attribute-specification"></a>属性の指定

***属性の指定***はアプリケーションを以前に定義された属性の宣言。 属性は、宣言で指定されている追加の宣言情報です。 属性は、グローバル スコープ (を含んでいるアセンブリまたはモジュールに属性を指定) に指定することができます、 *type_declaration*s ([入力宣言](namespaces.md#type-declarations))、 *class_member_declaration*s ([パラメーターの制約入力](classes.md#type-parameter-constraints))、 *interface_member_declaration*s ([インターフェイスのメンバー](interfaces.md#interface-members))、 *struct_member_declaration*s ([構造体のメンバー](structs.md#struct-members))、 *enum_member_declaration*s ([列挙型メンバー](enums.md#enum-members))、 *accessor_declarations* ([アクセサー](classes.md#accessors))、 *event_accessor_declarations* ([フィールドのようなイベント](classes.md#field-like-events))、および*formal_parameter_list*s ([メソッド パラメーター](classes.md#method-parameters))。

属性で指定***属性セクション***します。 属性のセクションでは、1 つまたは複数の属性のコンマ区切りのリストを囲む角かっこのペアで構成されます。 属性は、このようなリストで指定し、同じプログラム エンティティにアタッチされているセクションでは、順序の配置の順序は大きくありません。 属性の指定、 `[A][B]`、 `[B][A]`、 `[A,B]`、および`[B,A]`は同等です。

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

属性から成る、 *attribute_name*と、省略可能な引数と名前付き引数のリスト。 名前付き引数の前に、位置指定引数 (ある場合)。 位置指定引数から成る、 *attribute_argument_expression*; 名前付き引数を名の後に続く、等号 (=) から成る、 *attribute_argument_expression*であり、まとめて、単純な代入と同じ規則によって制限されます。 名前付き引数の順序は大きくありません。

*Attribute_name*属性クラスを識別します。 場合のフォーム*attribute_name*は*type_name*この名前は、属性クラスに参照する必要があります。 それ以外の場合は、コンパイル時のエラーが発生します。 例では、

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

結果の使用を試むため、コンパイル時エラー`Class1`クラス属性として`Class1`属性クラスではありません。

特定のコンテキストでは、1 つ以上のターゲットの属性の指定を許可します。 含めることによって、プログラムがターゲットを明示的に指定する*attribute_target_specifier*します。 グローバル レベルでは、属性を配置すると、 *global_attribute_target_specifier*が必要です。 その他のすべての場所で適切な既定値が適用されるが*attribute_target_specifier*を確認したり、または特定のあいまいな場合の既定値を上書きする (またはだけあいまいでない場合の既定値を確認する) に使用することができます。 したがって、通常、 *attribute_target_specifier*グローバル レベルを除く s を省略できます。 あいまいな可能性のあるコンテキストは、次のように解決されます。

*  グローバル スコープで指定された属性は、ターゲット アセンブリまたは対象のモジュールに適用できます。 既定値が存在しない、このコンテキストのため、 *attribute_target_specifier*が常に、このコンテキストで必要です。 有無、 `assembly` *attribute_target_specifier*属性がターゲットに適用されることを示しますアセンブリ以外の存在、 `module` *attribute_target_specifier* 。ターゲット モジュールに属性が適用されることを示します。
*  デリゲート宣言で指定された属性は、宣言されているデリゲートまたはその戻り値のいずれかに適用できます。 ない場合、 *attribute_target_specifier*デリゲートに属性が適用されます。 有無、 `type` *attribute_target_specifier* ; デリゲートに属性が適用されることを示しますの存在、 `return` *attribute_target_specifier*戻り値に属性が適用されることを示します。
*  メソッド宣言で指定された属性は、宣言されているメソッドまたはその戻り値のいずれかに適用できます。 ない場合、 *attribute_target_specifier*メソッドに属性が適用されます。 有無、 `method` *attribute_target_specifier*メソッドに属性が適用されることを示しますの存在、 `return` *attribute_target_specifier*を示します戻り値に属性が適用されます。
*  演算子の宣言で指定された属性は、宣言されている演算子またはその戻り値のいずれかに適用できます。 ない場合、 *attribute_target_specifier*、演算子、属性が適用されます。 有無、 `method` *attribute_target_specifier*には、演算子、属性が適用されることを示しますの存在、 `return` *attribute_target_specifier*戻り値に属性が適用されることを示します。
*  イベント アクセサーを省略するイベントの宣言で指定された属性では、宣言されるイベント、関連するフィールド (イベントが抽象でない場合)、または関連付けられている追加の適用でき、メソッドを削除することができます。 ない場合、 *attribute_target_specifier*イベントに属性が適用されます。 有無、 `event` *attribute_target_specifier*イベントに属性が適用されることを示しますの存在、 `field` *attribute_target_specifier*を示しますフィールドに属性が適用されます。かどうか、 `method` *attribute_target_specifier*メソッドに属性が適用されることを示します。
*  プロパティまたはインデクサーの宣言の get アクセサーの宣言で指定された属性は、関連付けられているメソッドまたはその戻り値のいずれかに適用できます。 ない場合、 *attribute_target_specifier*メソッドに属性が適用されます。 有無、 `method` *attribute_target_specifier*メソッドに属性が適用されることを示しますの存在、 `return` *attribute_target_specifier*を示します戻り値に属性が適用されます。
*  プロパティまたはインデクサーの宣言の set アクセサーに指定された属性は、関連付けられているメソッドまたはその唯一のパラメーターのいずれかに適用できます。 ない場合、 *attribute_target_specifier*メソッドに属性が適用されます。 有無、 `method` *attribute_target_specifier*メソッドに属性が適用されることを示しますの存在、 `param` *attribute_target_specifier*を示しますパラメーターの場合に、属性が適用されます。有無、 `return` *attribute_target_specifier*属性は、戻り値に適用されることを示します。
*  イベントの宣言は、関連付けられているメソッドまたはその唯一のパラメーターのいずれかを適用できます add または remove アクセサーの宣言で指定された属性です。 ない場合、 *attribute_target_specifier*メソッドに属性が適用されます。 有無、 `method` *attribute_target_specifier*メソッドに属性が適用されることを示しますの存在、 `param` *attribute_target_specifier*を示しますパラメーターの場合に、属性が適用されます。有無、 `return` *attribute_target_specifier*属性は、戻り値に適用されることを示します。

他のコンテキストを含めることで、 *attribute_target_specifier*は許可されているが不要な。 たとえば、クラス宣言を含めるか、指定子を省略`type`:

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

無効なを指定するとエラーには*attribute_target_specifier*します。 指定子、`param`クラス宣言では使用できません。

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

サフィックスを持つ属性クラスの名前は慣例により、`Attribute`します。 *Attribute_name*フォームの*type_name*を含めるかこのサフィックスを省略することがあります。 このサフィックスの有無にかかわらず、属性クラスが見つかった場合、あいまいさが存在して、コンパイル時エラーが発生します。 場合、 *attribute_name*が入力されているように、右端の*識別子*逐語的識別子 ([識別子](lexical-structure.md#identifiers))、サフィックスが付いていない属性だけが一致すると、その後あいまいさを解決できるようにします。 例では、

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

2 つの属性という名前のクラスを示しています`X`と`XAttribute`します。 属性`[X]`いずれかを参照しているため、あいまいになりますが`X`または`XAttribute`します。 逐語的識別子を使用すると、このようなまれなケースで指定する正確なインテントができます。 属性`[XAttribute]`あいまいでない (がという名前の属性クラスがあったかどうか`XAttributeAttribute`!)。 場合クラスの宣言`X`が削除され、両方の属性がという名前の属性クラスを参照して`XAttribute`、次のようにします。

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

同じエンティティについて単一使用属性クラスを複数回使用すると、コンパイル時エラーになります。 例では、

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

結果の使用を試むため、コンパイル時エラー `HelpString`、単一使用属性クラスである、2 回以上の宣言で`Class1`します。

式`E`は、 *attribute_argument_expression*のすべての次のステートメントに該当する場合。

*  型`E`属性パラメーター型は、([属性パラメーター型](attributes.md#attribute-parameter-types))。
*  コンパイル時の値に`E`次のいずれかに解決できます。
   * 定数値。
   * `System.Type` オブジェクト。
   * 1 次元配列*attribute_argument_expression*秒。

例:

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

A *typeof_expression* ([typeof 演算子](expressions.md#the-typeof-operator)) 属性引数の式は、非ジェネリック型、構築されたクローズ型、または、バインドされていないジェネリック型を参照できますが、これは参照できません、使用します。型を開きます。 これは、コンパイル時に式を解決できることを確認します。

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a>属性インスタンス

***属性インスタンス***は実行時に属性を表すインスタンスです。 属性は、位置指定引数は、属性クラスで定義されているし、名前付き引数。 属性インスタンスは、位置指定引数と名前付き引数を使用して初期化される属性クラスのインスタンスです。

属性インスタンスの取得には、次のセクションで説明した、コンパイル時と実行時の両方の処理が含まれます。

### <a name="compilation-of-an-attribute"></a>属性のコンパイル

コンパイル、*属性*属性クラスを使用して`T`、 *positional_argument_list* `P`と*named_argument_list* `N`、次の手順で構成されます。

*  コンパイル時の処理手順をコンパイルするため、 *object_creation_expression*フォームの`new T(P)`します。 次の手順は、コンパイル時エラーが発生するか、インスタンス コンス トラクターを判断`C`で`T`は、実行時に呼び出すことができます。
*  場合`C`パブリック アクセシビリティを持たない場合、コンパイル時エラーが発生します。
*  各*named_argument* `Arg`で`N`:
   * ように`Name`する、*識別子*の*named_argument* `Arg`します。
   * `Name` 非静的な読み取り/書き込みのパブリック フィールドまたはプロパティを識別する必要があります`T`します。 場合`T`コンパイル時エラーが発生し、そのようなフィールドまたはプロパティを持ちます。
*  属性の実行時インスタンスを作成して、次の情報を保持します属性クラス`T`、インスタンス コンス トラクター`C`で`T`、 *positional_argument_list* `P` 。および*named_argument_list* `N`します。

### <a name="run-time-retrieval-of-an-attribute-instance"></a>属性インスタンスの実行時の取得

コンパイル、*属性*属性クラスを生成`T`、インスタンス コンス トラクター`C`で`T`、 *positional_argument_list* `P`、および*named_argument_list* `N`します。 この情報を指定するには、属性インスタンスが、次の手順を使用して実行時に取得できます。

*  実行時の処理手順を実行するため、 *object_creation_expression*フォームの`new T(P)`、インスタンス コンス トラクターを使用して`C`コンパイル時に決定されます。 次の手順は、例外、またはインスタンスを作成します`O`の`T`します。
*  各*named_argument* `Arg`で`N`の順序で。
   * ように`Name`する、*識別子*の*named_argument* `Arg`します。 場合`Name`で静的でないパブリックの読み取り/書き込みフィールドまたはプロパティを識別しないいない`O`例外がスローされます。
   * ように`Value`評価の結果である、 *attribute_argument_expression*の`Arg`します。
   * 場合`Name`でフィールドを識別する`O`にこのフィールドを設定`Value`します。
   * それ以外の場合、`Name`でプロパティを識別します`O`します。 このプロパティを設定`Value`します。
   * 結果は`O`、属性クラスのインスタンス`T`で初期化されました、 *positional_argument_list* `P`と*named_argument_list*`N`.

## <a name="reserved-attributes"></a>予約済みの属性

属性の数が少ないでは、何らかの方法で、言語に影響します。 これらの属性は次のとおりです。

*  `System.AttributeUsageAttribute` ([、AttributeUsage 属性](attributes.md#the-attributeusage-attribute)) を使用して、属性クラスを使用できる方法について説明します。
*  `System.Diagnostics.ConditionalAttribute` ([、条件付き属性](attributes.md#the-conditional-attribute))、これは条件付きメソッドの定義に使用します。
*  `System.ObsoleteAttribute` ([The Obsolete 属性](attributes.md#the-obsolete-attribute))、不使用とメンバーを示すために使用します。
*  `System.Runtime.CompilerServices.CallerLineNumberAttribute`、`System.Runtime.CompilerServices.CallerFilePathAttribute`と`System.Runtime.CompilerServices.CallerMemberNameAttribute`([呼び出し元情報属性](attributes.md#caller-info-attributes))、省略可能なパラメーターを呼び出し元のコンテキストに関する情報を入力に使用されます。

### <a name="the-attributeusage-attribute"></a>AttributeUsage 属性

属性`AttributeUsage`属性クラスを使用できる方法を説明するために使用します。

装飾したクラス、`AttributeUsage`属性から派生しなければなりません`System.Attribute`、直接または間接的にします。 それ以外の場合は、コンパイル時のエラーが発生します。

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a>条件付き属性

属性`Conditional`の定義を有効に***条件付きメソッド***と***条件付き属性クラス***します。

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a>条件付きメソッド

修飾されたメソッド、`Conditional`属性は、条件付きメソッド。 `Conditional`属性は、条件付きコンパイル シンボルをテストして条件を示します。 条件付きメソッドの呼び出しをに含まれるか、または省略すると、呼び出しの時点でこのシンボルが定義されているかどうかによって異なります。 呼び出しが含まれています。 シンボルが定義されている場合それ以外の場合、(受信者の評価と呼び出しのパラメーターを含む) の呼び出しは省略されます。

条件付きメソッドは、次の制限を受けます。

*  条件付きメソッドのメソッドである必要があります、 *class_declaration*または*struct_declaration*します。 コンパイル時エラーが発生します、`Conditional`インターフェイス宣言のメソッドに属性を指定します。
*  条件付きメソッドの戻り値の型があります`void`します。
*  条件付きメソッドがでマークされていない必要があります、`override`修飾子。 条件付きメソッドをマークすることがあります、`virtual`修飾子、ただしします。 このようなメソッドのオーバーライドは暗黙的に条件付きとでない、明示的にマークする必要があります、`Conditional`属性。
*  条件付きメソッドは、インターフェイス メソッドの実装があります。 それ以外の場合は、コンパイル時のエラーが発生します。

さらに、コンパイル時エラーが発生する条件付きメソッドで使用する場合、 *delegate_creation_expression*します。 例では、

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

宣言`Class1.M`条件付きメソッドとして。 `Class2``Test`メソッドは、このメソッドを呼び出します。 条件付きコンパイル シンボル以降`DEBUG`が定義されている場合は場合、`Class2.Test`が呼び出されると、それが呼び出す`M`します。 場合、シンボル`DEBUG`が定義されていない、し`Class2.Test`呼び出さない`Class1.M`します。

呼び出しの時点の条件付きコンパイル シンボルを含めるか、条件付きメソッドの呼び出しの除外を制御することに注意してください。 例

ファイル`class1.cs`:

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

ファイル`class2.cs`:

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

ファイル`class3.cs`:

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

クラスは、`Class2`と`Class3`条件付きメソッドの呼び出しを含む各`Class1.F`、かどうかに基づく条件付きである`DEBUG`が定義されています。 コンテキストでこのシンボルが定義されているため`Class2`なく`Class3`への呼び出し`F`で`Class2`への呼び出し中に、含まれている`F`で`Class3`を省略するとします。

継承チェーンで条件付きメソッドを使用してわかりにくいことができます。 条件付きメソッドを呼び出し`base`、フォームの`base.M`、通常の条件付きメソッドの呼び出し規則が適用されます。 例

ファイル`class1.cs`:

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

ファイル`class2.cs`:

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

ファイル`class3.cs`:

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

`Class2` 呼び出しを含み、`M`基底クラスで定義されています。 この呼び出しを省略したは、基本のメソッドでは、シンボルの有無に基づく条件付き理由`DEBUG`は定義されていません。 したがって、メソッドは、コンソールに書き込みます"`Class2.M executed`"のみです。 賢く利用*pp_declaration*s は、このような問題を排除できます。

#### <a name="conditional-attribute-classes"></a>条件付き属性クラス

属性クラス ([属性クラス](attributes.md#attribute-classes)) 1 つまたは複数で修飾された`Conditional`属性は、***条件付き属性クラス***します。 宣言されている条件付きコンパイル シンボルを使用した条件付き属性クラスが関連付けられているなりますその`Conditional`属性。 この例では:

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

宣言`TestAttribute`条件付きコンパイル シンボルに関連付けられた条件属性をクラスとして`ALPHA`と`BETA`します。

属性の仕様 ([属性の指定](attributes.md#attribute-specification)) の条件付き属性が含まれる 1 つ以上の関連付けられている条件付きコンパイル シンボルが定義されている場合、仕様の時点でそれ以外の場合、属性指定は省略されます。

仕様の時点で条件付きコンパイル シンボルでの追加または条件付き属性クラスの属性の指定の除外を制御することに注意してください。 重要です。 例

ファイル`test.cs`:

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

ファイル`class1.cs`:

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

ファイル`class2.cs`:

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

クラスは、`Class1`と`Class2`は属性で修飾された各`Test`、かどうかに基づく条件付きである`DEBUG`が定義されています。 コンテキストでこのシンボルが定義されているため`Class1`なく`Class2`の仕様、`Test`属性`Class1`の仕様の中に、含まれている、`Test`属性を`Class2`を省略するとします。

### <a name="the-obsolete-attribute"></a>Obsolete 属性

属性`Obsolete`使用できなくする型の型とメンバーをマークするために使用します。

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

プログラムを使用して、型またはメンバーで装飾したかどうか、`Obsolete`属性に、コンパイラは、警告またはエラーを発行します。 エラー パラメーターが指定されていない場合、または error パラメーターを指定し、値を持つ場合に、コンパイラが警告を発行する具体的には、`false`します。 エラー パラメーターを指定し、値を持つ場合、コンパイラがエラーを発行`true`します。

例

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

クラスは、`A`で修飾されたが、`Obsolete`属性。 各使用`A`で`Main`で指定されたメッセージを含む警告結果は、"このクラスは廃止されています。クラス B 代わりに使用します。"

### <a name="caller-info-attributes"></a>呼び出し元情報属性

ログ記録やレポートなどの目的で、関数メンバー呼び出し元のコードに関する特定のコンパイル時情報を取得するのに便利ですがあります。 呼び出し元情報属性は、このような情報を透過的に渡す方法を提供します。

オプションのパラメーターは、呼び出し元情報属性のいずれかで注釈が付け、呼び出しに対応する引数を省略することは必ずしも代わりに使用する既定のパラメーター値。 代わりに、指定した呼び出し元のコンテキストに関する情報を使用できる場合、その情報は、引数の値として渡されます。

例:

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

呼び出し`Log()`引数なしで印刷には、呼び出しの行の数とファイルのパスとを呼び出しが発生したメンバーの名前。

呼び出し元情報属性は、デリゲートの宣言を含む省略可能なパラメーターなど、どこで実行できます。 ただし、特定の呼び出し元情報属性では、パラメーターの型に代入された値から暗黙的な変換が必ずようにができる属性は、パラメーターの型に制限があります。

同じ呼び出し元情報属性を定義して、部分メソッド宣言の一部を実装する両方のパラメーターがエラーになります。 実装の一部でのみ発生している呼び出し元情報属性は無視されますが、定義の一部で呼び出し元情報属性のみが適用されます。

呼び出し元情報は、オーバー ロードの解決には影響しません。 オーバー ロードの解決が省略されたその他の省略可能なパラメーターは無視されますが、同じ方法でこれらのパラメーターを無視する属性付きの省略可能なパラメーターは、呼び出し元のソース コードから省略されますが、([オーバー ロードの解決](expressions.md#overload-resolution))。

関数がソース コードで明示的に呼び出されるときにのみ、呼び出し元情報が置き換えられます。 親の暗黙的なコンス トラクターの呼び出しなど暗黙的な呼び出しでは、ソースの場所がないと、呼び出し元情報は置き換えられません。 また、動的にバインドされている呼び出しに呼び出し元情報は置き換えられません。 ときに、呼び出し元情報をこのような場合で属性付きのパラメーターを省略すると、パラメーターの指定した既定値が代わりに使用されます。

クエリ式は例外です。 これらは構文の拡張と見なされ、呼び出し、呼び出し元情報属性を持つオプション パラメーターを省略するを展開する場合、呼び出し元情報が置き換えられます。 使用されている場所では、呼び出しはから生成されたクエリ句の場所です。

次の順序で優先設定されているパラメーターを 1 つ以上の呼び出し元情報属性が指定されている場合: `CallerLineNumber`、 `CallerFilePath`、`CallerMemberName`します。

#### <a name="the-callerlinenumber-attribute"></a>CallerLineNumber 属性

`System.Runtime.CompilerServices.CallerLineNumberAttribute`は省略可能なパラメーターで許可されている標準の暗黙的な変換がある場合に ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) 定数の値から`int.MaxValue`パラメーターの型にします。 これにより、エラーが発生せず、その値になるまでの任意の負でない行番号を渡すことができます。

ソース コード内の場所から関数の呼び出しは省略可能なパラメーターを省略する場合、 `CallerLineNumberAttribute`、その場所の行番号を表す数値リテラルが既定のパラメーター値ではなく、呼び出しに引数として使用されます。

呼び出しは、複数の行にまたがっている場合、選択した行は実装依存を使用します。

行番号の影響を受ける可能性がありますので注意`#line`ディレクティブ ([ディレクティブの行](lexical-structure.md#line-directives))。

#### <a name="the-callerfilepath-attribute"></a>CallerFilePath 属性

`System.Runtime.CompilerServices.CallerFilePathAttribute`は省略可能なパラメーターで許可されている標準の暗黙的な変換がある場合に ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) から`string`パラメーターの型にします。

ソース コード内の場所から関数の呼び出しは省略可能なパラメーターを省略する場合、 `CallerFilePathAttribute`、その場所のファイル パスを表す文字列リテラルが既定のパラメーター値ではなく、呼び出しに引数として使用されます。

ファイルのパスの形式は、実装によって異なります。

ファイルのパスはによって影響を受ける可能性がありますので注意`#line`ディレクティブ ([ディレクティブの行](lexical-structure.md#line-directives))。

#### <a name="the-callermembername-attribute"></a>CallerMemberName 属性

`System.Runtime.CompilerServices.CallerMemberNameAttribute`は省略可能なパラメーターで許可されている標準の暗黙的な変換がある場合に ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) から`string`パラメーターの型にします。

ソース コードでは省略可能なパラメーターを省略自体またはその戻り値の型、パラメーターまたは型パラメーターで、関数メンバーの本文内または属性内の場所から関数の呼び出しが関数メンバーに適用されている場合、 `CallerMemberNameAttribute`、そのメンバーの名前を表す文字列リテラルは、パラメーターの既定値ではなく、呼び出しの引数として使用されます。

ジェネリック メソッド内で発生する呼び出しは、型パラメーター リストなし、メソッド名のみを使用します。

呼び出しで明示的なインターフェイス メンバーの実装内で発生する、上記のインターフェイス修飾なし、メソッド名自体は使用されます。

プロパティまたはイベントのアクセサー内で発生するための呼び出し、メンバー名を使用するプロパティまたはイベント自体は。

インデクサーのアクセサー内で発生する呼び出しでは、使用されるメンバーの名前は、によって指定された、 `IndexerNameAttribute` ([、IndexerName 属性](attributes.md#the-indexername-attribute)) 存在する場合は、インデクサーのメンバーまたは既定の名前で`Item`それ以外の場合。

呼び出しで、メンバーのインスタンス コンス トラクター、静的コンス トラクター、デストラクター、および演算子の宣言内で発生するは、使用される名前は、実装によって異なります。

## <a name="attributes-for-interoperation"></a>相互運用の属性

メモ:このセクションでは、c# の Microsoft .NET の実装にのみ適用されます。

### <a name="interoperation-with-com-and-win32-components"></a>COM および Win32 コンポーネントとの相互運用

.NET ランタイムでは、COM と Win32 の Dll を使用して記述されたコンポーネントと相互運用する c# プログラムを有効にする属性の数が多いを提供します。 たとえば、`DllImport`で属性を使用できます、`static extern`に Win32 DLL に含まれるメソッドの実装があることを示す方法。 これらの属性がある、`System.Runtime.InteropServices`名前空間、およびこれらの属性の詳細なドキュメントが、.NET ランタイムのドキュメントが見つかりました。

### <a name="interoperation-with-other-net-languages"></a>他の .NET 言語との相互運用

#### <a name="the-indexername-attribute"></a>IndexerName 属性

インデクサーはインデックス付きのプロパティを使用して .NET で実装され、名前 .NET メタデータであります。 ない場合は`IndexerName`属性は、インデクサーでは、その名前存在`Item`既定で使用されます。 `IndexerName`属性により、開発者はこの既定をオーバーライドし、別の名前を指定します。

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
