---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703983"
---
# <a name="classes"></a>クラス

クラスは、データメンバー (定数とフィールド)、関数メンバー (メソッド、プロパティ、イベント、インデクサー、演算子、インスタンスコンストラクター、デストラクターと静的コンストラクター)、および入れ子にされた型を含むことができるデータ構造です。 クラス型は、派生クラスが基底クラスを拡張および特殊化できる機構である継承をサポートしています。

## <a name="class-declarations"></a>クラス宣言

*Class_declaration*は、新しいクラスを宣言する*type_declaration* ([型宣言](namespaces.md#type-declarations)) です。

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

*Class_declaration*は、省略可能な*属性*のセット ([属性](attributes.md))、オプションの一連の*class_modifier*s ([クラス修飾子](classes.md#class-modifiers))、オプションの `partial` 修飾子、その後に省略*可能な* *type_parameter_list* ([型パラメーター](classes.md#type-parameters))、オプションの class_base の指定 (クラスの基本仕様) *、省略可能*なの仕様 (クラスの[基本仕様](classes.md#class-base-specification)) の順で構成さ `class` れます。省略可能な*type_parameter_constraints_clause*s ([型パラメーター制約](classes.md#type-parameter-constraints)) のセットで、その後に*class_body* ([クラス本体](classes.md#class-body)) が続きます。その後にセミコロンが続きます。

クラス宣言は、 *type_parameter_list*を提供する場合を除き、 *type_parameter_constraints_clause*s を指定することはできません。

*Type_parameter_list*を提供するクラス宣言は、***ジェネリッククラス宣言***です。 また、ジェネリッククラスの宣言またはジェネリック構造体の宣言内で入れ子になっているクラスは、それ自体がジェネリッククラスの宣言です。これは、包含する型の型パラメーターを指定して構築された型を作成する必要があるためです。

### <a name="class-modifiers"></a>クラス修飾子

*Class_declaration*には、必要に応じてクラス修飾子のシーケンスを含めることができます。

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

同じ修飾子がクラス宣言に複数回出現する場合、コンパイル時エラーになります。

`new` 修飾子は、入れ子になったクラスで許可されています。 このメソッドは、[新しい修飾子](classes.md#the-new-modifier)で説明されているように、クラスが同じ名前で継承されたメンバーを非表示にすることを指定します。 入れ子になったクラス宣言ではないクラス宣言に `new` 修飾子が表示される場合、コンパイル時エラーになります。

`public`、`protected`、`internal`、および `private` 修飾子は、クラスのアクセシビリティを制御します。 クラス宣言が発生したコンテキストによっては、これらの修飾子の一部が許可されない場合があります ([アクセシビリティは宣言](basic-concepts.md#declared-accessibility)されています)。

`abstract`、`sealed`、および `static` 修飾子については、次のセクションで説明します。

#### <a name="abstract-classes"></a>抽象クラス

`abstract` 修飾子は、クラスが不完全であること、および基底クラスとしてのみ使用されることを示すために使用されます。 抽象クラスは、次のように、非抽象クラスとは異なります。

*  抽象クラスを直接インスタンス化することはできません。抽象クラスで `new` 演算子を使用する場合、コンパイル時エラーになります。 コンパイル時の型が抽象である変数と値を持つことはできますが、このような変数と値は必ずしも `null` であるか、抽象型から派生した非抽象クラスのインスタンスへの参照を含んでいる必要があります。
*  抽象クラスは、抽象メンバーを含むことができます (必須ではありません)。
*  抽象クラスをシールドにすることはできません。

非抽象クラスが抽象クラスから派生した場合、非抽象クラスには、継承されたすべての抽象メンバーの実際の実装が含まれている必要があります。これにより、抽象メンバーはオーバーライドされます。 この例では、
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
抽象クラス `A` には、`F`抽象メソッドが導入されています。 クラス `B` には、追加のメソッド `G`が導入されていますが、`F`の実装が提供されていないため、`B` も abstract として宣言する必要があります。 クラス `C` は `F` をオーバーライドし、実際の実装を提供します。 `C`には抽象メンバーがないため、`C` は非抽象にすることができます (必須ではありません)。

#### <a name="sealed-classes"></a>シールクラス

`sealed` 修飾子は、クラスからの派生を防ぐために使用されます。 シールクラスが別のクラスの基底クラスとして指定されている場合、コンパイル時エラーが発生します。

シールクラスを抽象クラスにすることもできません。

`sealed` 修飾子は、意図しない派生を防ぐために主に使用されますが、特定の実行時の最適化を有効にすることもできます。 特に、シールクラスは派生クラスを持たないことがわかっているため、シールされたクラスインスタンスでの仮想関数メンバー呼び出しを非仮想呼び出しに変換することができます。

#### <a name="static-classes"></a>静的クラス

`static` 修飾子は、クラスが***静的クラス***として宣言されていることを示すために使用されます。 静的クラスはインスタンス化できません。型として使用することはできず、静的メンバーのみを含めることができます。 拡張メソッド ([拡張メソッド](classes.md#extension-methods)) の宣言を含めることができるのは、静的クラスだけです。

静的クラスの宣言には、次の制限があります。

*  静的クラスには、`sealed` または `abstract` 修飾子を含めることはできません。 ただし、静的クラスはインスタンス化することも、から派生することもできないため、シールドと抽象の両方として動作します。
*  静的クラスに*class_base*仕様 ([クラスの基本仕様](classes.md#class-base-specification)) を含めることはできません。また、基底クラスまたは実装されたインターフェイスのリストを明示的に指定することもできません。 静的クラスは、型 `object`から暗黙的に継承します。
*  静的クラスには、静的メンバー ([静的メンバーとインスタンスメンバー](classes.md#static-and-instance-members)) のみを含めることができます。 定数および入れ子にされた型は、静的メンバーとして分類されることに注意してください。
*  静的クラスには、`protected` または `protected internal` によって宣言されたアクセシビリティを持つメンバーを含めることはできません。

これらの制限に違反すると、コンパイル時にエラーが発生します。

静的クラスには、インスタンスコンストラクターがありません。 静的クラスでインスタンスコンストラクターを宣言することはできません。また、静的クラスに既定のインスタンスコンストラクター ([既定のコンストラクター](classes.md#default-constructors)) は用意されていません。

静的クラスのメンバーは自動的に静的ではなく、メンバー宣言には `static` 修飾子を明示的に含める必要があります (定数と入れ子にされた型を除く)。 クラスが静的外部クラス内で入れ子になっている場合、入れ子になったクラスは、明示的に `static` 修飾子を含んでいない限り、静的クラスにはなりません。

__静的クラス型の参照__

では、 *namespace_or_type_name* ([名前空間と型名](basic-concepts.md#namespace-and-type-names)) に静的クラスを参照することが許可されています。

*  *Namespace_or_type_name*は `T.I`フォームの*namespace_or_type_name*の `T` です。
*  *Namespace_or_type_name*は、`typeof(T)`フォームの*typeof_expression* ([引数リスト](expressions.md#argument-lists)1) の `T` です。

*Primary_expression* ([関数メンバー](expressions.md#function-members)) は、静的クラスを参照することが許可されています。

*  *Primary_expression*は、フォーム`E` のmember_access([動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))の`E.I`です。

それ以外のコンテキストでは、静的クラスを参照するコンパイル時エラーになります。 たとえば、静的クラスを基底クラス、構成型 (入れ子にされた[型](classes.md#nested-types))、ジェネリック型引数、または型パラメーター制約として使用すると、エラーになります。 同様に、静的クラスは、配列型、ポインター型、`new` 式、キャスト式、`is` 式、`as` 式、`sizeof` 式、または既定値式では使用できません。

### <a name="partial-modifier"></a>Partial 修飾子

この*class_declaration*が部分型の宣言であることを示すには、`partial` 修飾子を使用します。 外側の名前空間または型宣言内で同じ名前を持つ複数の部分型宣言を組み合わせると、[部分](classes.md#partial-types)型で指定された規則に従って1つの型宣言が形成されます。

クラスの宣言をプログラムテキストの個別のセグメントに分散させると、これらのセグメントが異なるコンテキストで生成または管理される場合に便利です。 たとえば、クラス宣言の一部はコンピューターによって生成される場合がありますが、もう一方の部分は手動で作成されます。 2つのテキストを分離することで、1つの更新がもう一方の更新と競合するのを防ぐことができます。

### <a name="type-parameters"></a>型パラメーター

型パラメーターは、構築された型を作成するために提供される型引数のプレースホルダーを示す単純な識別子です。 型パラメーターは、後で提供される型の正式なプレースホルダーです。 これに対して、型引数 ([型引数](types.md#type-arguments)) は、構築された型が作成されるときに、型パラメーターの代わりに使用される実際の型です。

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

クラス宣言の各型パラメーターは、そのクラスの宣言空間 ([宣言](basic-concepts.md#declarations)) に名前を定義します。 そのため、別の型パラメーターまたはそのクラスで宣言されたメンバーと同じ名前を持つことはできません。 型パラメーターには、型自体と同じ名前を指定することはできません。

### <a name="class-base-specification"></a>クラスの基本指定

クラス宣言には*class_base*の仕様を含めることができます。この仕様では、クラスの直接的な基本クラスと、クラスによって直接実装されたインターフェイス ([インターフェイス](interfaces.md)) が定義されています。

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

クラス宣言で指定される基底クラスは、構築されたクラス型 ([構築型](types.md#constructed-types)) にすることができます。 基底クラスは、独自の型パラメーターにすることはできませんが、スコープ内の型パラメーターを含めることができます。

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>基底クラス

*Class_base*に*class_type*が含まれている場合は、宣言されているクラスの直接基底クラスを指定します。 クラス宣言に*class_base*がない場合、または*class_base*がインターフェイス型のみを一覧表示する場合は、直接基底クラスが `object`と見なされます。 クラスは、「[継承](classes.md#inheritance)」で説明されているように、直接基底クラスからメンバーを継承します。

この例では、
```csharp
class A {}

class B: A {}
```
クラス `A` は `B`の直接基底クラスと呼ばれ、`B` は `A`から派生したものと呼ばれます。 `A` は直接基底クラスを明示的に指定していないため、その直接の基底クラスは暗黙的に `object`ます。

構築されたクラス型の場合、基底クラスがジェネリッククラス宣言で指定されている場合、基本クラスの宣言の各*type_parameter*に対して、構築された型の基底クラスが、構築された型の対応する*type_argument*として取得されます。 指定されたジェネリッククラス宣言
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
構築された型 `G<int>` の基本クラスは `B<string,int[]>`ます。

クラス型の直接基底クラスは、少なくともクラス型自体 ([アクセシビリティドメイン](basic-concepts.md#accessibility-domains)) と同じようにアクセス可能である必要があります。 たとえば、`public` クラスが `private` または `internal` クラスから派生する場合、コンパイル時エラーになります。

クラス型の直接基底クラスは、`System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`、または `System.ValueType`のいずれの型でもない必要があります。 さらに、ジェネリッククラスの宣言では、`System.Attribute` を直接または間接的な基底クラスとして使用することはできません。

クラス `B`の直接的な基底クラスの指定 `A` の意味を判断する際、`B` の直接の基底クラスは一時的に `object`と見なされます。 直感的に言えば、基底クラスの指定の意味は、それ自体に再帰的に依存することはできません。 次に例を示します。
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
は、基底クラスの仕様では、`C` の直接基底クラスが `object`と見なされ、そのため ([名前空間と型名](basic-concepts.md#namespace-and-type-names)の規則によって) `C` にメンバー `B`があるとは見なされない `A<C.B>`、エラーになります。

クラス型の基底クラスは、直接基底クラスとその基本クラスです。 つまり、基本クラスのセットは、直接基底クラスのリレーションシップの推移的なクロージャです。 上の例を参照して、`B` の基本クラスは `A` および `object`です。 この例では、
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
`D<int>` の基本クラスは、`C<int[]>`、`B<IComparable<int[]>>`、`A`、および `object`です。

クラス `object`を除き、すべてのクラス型には、直接基底クラスが1つだけあります。 `object` クラスには、直接の基底クラスはなく、他のすべてのクラスの最終的な基本クラスです。

クラス `B` クラス `A`から派生した場合、`A` が `B`に依存していると、コンパイル時にエラーになります。 クラスは、直接基底クラス (存在する場合)***に依存***し、そのクラスがすぐに入れ子になっているクラス***に直接依存***します (存在する場合)。 この定義により、クラスが依存するクラスの完全なセットは、リレーションシップ***に直接依存***しているの再帰的および推移的なクロージャです。

例
```csharp
class A: A {}
```
は、クラスがそれ自体に依存しているため、エラーになります。 同様に、例を次に示します。
```csharp
class A: B {}
class B: C {}
class C: A {}
```
クラスが循環的に依存しているため、エラーが発生しています。 最後に、例を示します。
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
`A` は `B.C` (直接のクラス) に依存しているため、コンパイル時エラーが発生します。これは、循環的に `A`に依存している `B` (すぐ外側のクラス) に依存します。

クラスは、その中に入れ子になっているクラスに依存しないことに注意してください。 この例では、
```csharp
class A
{
    class B: A {}
}
```
`B` は `A` に依存します (`A` は直接の基底クラスであり、そのすぐ外側のクラスでもあるため)。ただし、`A` は `B` に依存しません (`B` は基本クラスでも外側のクラスでもありません)。`A` したがって、この例は有効です。

`sealed` クラスから派生させることはできません。 この例では、
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
クラス `B` は、`sealed` クラス `A`からの派生を試行しているため、エラーになっています。

#### <a name="interface-implementations"></a>インターフェイスの実装

*Class_base*の指定には、インターフェイス型のリストを含めることができます。この場合、クラスは、指定されたインターフェイス型を直接実装すると言います。 インターフェイスの実装については、「[インターフェイスの実装](interfaces.md#interface-implementations)」で詳しく説明します。

### <a name="type-parameter-constraints"></a>型パラメーターの制約

ジェネリック型およびメソッドの宣言では、 *type_parameter_constraints_clause*s を含めることにより、必要に応じて型パラメーターの制約を指定できます。

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

各*type_parameter_constraints_clause*は、トークン `where`で構成され、その後に型パラメーターの名前が続き、その後にコロンとその型パラメーターの制約のリストが続きます。 型パラメーターごとに1つの `where` 句を指定できます。また、`where` 句は任意の順序で一覧表示できます。 プロパティアクセサーの `get` および `set` トークンと同様、`where` トークンはキーワードではありません。

`where` 句に指定されている制約の一覧には、次のいずれかのコンポーネントを含めることができます。この順序では、1つのプライマリ制約、1つ以上のセカンダリ制約、コンストラクター制約、`new()`です。

プライマリ制約は、クラス型、***参照型の制約***`class`、または***値型の制約***`struct`にすることができます。 セカンダリ制約は、 *type_parameter*または*interface_type*にすることができます。

参照型の制約は、型パラメーターに使用される型引数が参照型である必要があることを指定します。 すべてのクラス型、インターフェイス型、デリゲート型、配列型、および参照型として知られる型パラメーターは、この制約を満たしています。

値型の制約は、型パラメーターに使用される型引数が null 非許容の値型である必要があることを指定します。 Null 非許容の構造体型、列挙型、および値型の制約を持つ型パラメーターは、この制約を満たしています。 値型として分類されていますが、null 許容型 ([Null 許容](types.md#nullable-types)型) は値型の制約を満たしていないことに注意してください。 値型の制約を持つ型パラメーターには、 *constructor_constraint*を指定することもできません。

ポインター型を型引数にすることはできません。また、参照型または値型の制約を満たしているとは見なされません。

制約がクラス型、インターフェイス型、または型パラメーターである場合、その型は、その型パラメーターに使用されるすべての型引数がをサポートする必要がある最小の "基本型" を指定します。 構築された型またはジェネリックメソッドを使用する場合は、コンパイル時に型パラメーターの制約に対して型引数がチェックされます。 指定された型引数は、「[制約を満たす](types.md#satisfying-constraints)」で説明されている条件を満たす必要があります。

*Class_type*制約は、次の規則を満たしている必要があります。

*  型はクラス型である必要があります。
*  型を `sealed`にすることはできません。
*  型は、`System.Array`、`System.Delegate`、`System.Enum`、または `System.ValueType`のいずれの型でもない必要があります。
*  型を `object`にすることはできません。 すべての型は `object`から派生するため、このような制約は、許可されている場合は効果がありません。
*  特定の型パラメーターに対して最大1つの制約をクラス型にすることができます。

*Interface_type*制約として指定する型は、次の規則を満たしている必要があります。

*  型はインターフェイス型である必要があります。
*  指定された `where` 句では、型を複数回指定することはできません。

どちらの場合も、制約には、関連付けられている型またはメソッドの宣言の型パラメーターを構築型の一部として含めることができ、宣言された型を含めることができます。

型パラメーターの制約として指定されたクラスまたはインターフェイスの型は、宣言するジェネリック型またはメソッドと同じように、少なくともアクセス可能な ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints)) 必要があります。

*Type_parameter*制約として指定する型は、次の規則を満たしている必要があります。

*  型は型パラメーターでなければなりません。
*  指定された `where` 句では、型を複数回指定することはできません。

さらに、型パラメーターの依存関係グラフに循環がない必要があります。依存関係は、によって定義される推移的な関係です。

*  型パラメーター `T` が型パラメーターの制約として使用されている場合 `S` `S` は `T`***に依存***します。
*  型パラメーター `S` が型パラメーター `T` に依存し、`T` が型 `U` パラメーターに依存している場合、`S` は `U`***に依存***します。

この関係により、型パラメーターが自身に依存している (直接または間接的に) ことを前提としたコンパイル時エラーになります。

制約は、依存する型パラメーターの間で一貫している必要があります。 型パラメーター `S` が型パラメーターに依存する場合 `T` 次のようになります。

*  `T` には値型の制約を指定できません。 それ以外の場合、`T` は実質的にシールされるため、`S` は `T`と同じ型である必要があります。これにより、2つの型パラメーターは不要になります。
*  `S` に値型の制約がある場合は、`T` に*class_type*制約を指定することはできません。
*  `S` に*class_type*の `A` 制約があり、`T` に*class_type*の制約がある場合は、`B` から `A` への id 変換または暗黙の参照変換が `B` になる必要があります。`B``A`
*  `S` 型パラメーター `U` にも依存し、`U` に*class_type*の制約があり、`A` に *`T`* の制約がある場合は、class_type から `B` への id 変換または暗黙の参照変換が `A` になる必要があります。`B``B``A`

`S` が値型の制約を持ち、`T` 参照型の制約を持つようにするために有効です。 これにより、`System.Object`、`System.ValueType`、`System.Enum`、および任意のインターフェイス型に `T` が制限されます。

型パラメーターの`where` `new()` 句にコンストラクターの制約 (フォームがある) が含まれている場合は、演算子を使用して、型のインスタンス (`new`オブジェクト作成式[) を作成することができます。](expressions.md#object-creation-expressions) コンストラクター制約のある型パラメーターに使用されるすべての型引数には、パブリックなパラメーターなしのコンストラクターが必要です (このコンストラクターは、任意の値型に[対して暗黙的](classes.md#type-parameter-constraints)に存在します)。または、値型の制約またはコンストラクターの制約を持つ型パラメーターであること

制約の例を次に示します。
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

次の例では、型パラメーターの依存関係グラフで循環が発生するため、エラーが発生しています。
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

次の例は、その他の無効な状況を示しています。
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

`T` 型パラメーターの***有効な基本クラス***は次のように定義されています。

*  `T` に primary 制約または型パラメーター制約がない場合、その有効な基本クラスは `object`です。
*  `T` に値型の制約がある場合、その有効な基本クラスは `System.ValueType`です。
*  `T` に*class_type* `C` 制約はありますが*type_parameter*制約はありませんが、その有効な基本クラスは `C`です。
*  `T` に*class_type*制約がなく、1つ以上の*type_parameter*制約がある場合、その有効な基本クラスは、その*type_parameter*制約の有効な基本クラスのセットに含まれる最も内側の型 (リフトされた[変換演算子](conversions.md#lifted-conversion-operators)) になります。 一貫性規則により、このような最も包含型が存在することが保証されます。
*  `T` に*class_type*制約と1つ以上の*type_parameter*制約の両方がある場合、その有効な基本クラスは、`T` の*class_type*制約とその*type_parameter*制約の有効な基本クラスで構成される、セット内の最も包含型 (リフトされた[変換演算子](conversions.md#lifted-conversion-operators)) です。 一貫性規則により、このような最も包含型が存在することが保証されます。
*  `T` が参照型の制約を持ち、 *class_type*の制約がない場合、その有効な基本クラスは `object`ます。

これらの規則の目的のために、T に*value_type*の制約 `V` がある場合は、代わりにを使用して、 *class_type*である `V` の最も限定的な基本型を使用します。 これは明示的に指定された制約では発生しませんが、ジェネリックメソッドの制約が、オーバーライドするメソッドの宣言またはインターフェイスメソッドの明示的な実装によって暗黙的に継承される場合に発生する可能性があります。

これらの規則は、有効な基本クラスが常に*class_type*であることを確認します。

`T` 型パラメーターの***有効なインターフェイスセット***は、次のように定義されます。

*  `T` に*secondary_constraints*がない場合、その有効なインターフェイスセットは空になります。
*  `T` *interface_type*に制約があり、 *type_parameter*制約がない場合、その有効なインターフェイスセットは*interface_type*制約のセットになります。
*  `T` に*interface_type*制約がなく、 *type_parameter*制約がある場合、その有効なインターフェイスセットは、その*type_parameter*制約の有効なインターフェイスセットの和集合になります。
*  `T` に*interface_type*の制約と*type_parameter*の制約の両方がある場合、その有効なインターフェイスセットは、一連の*interface_type*制約とその*type_parameter*制約の有効なインターフェイスセットの和集合になります。

参照型の制約がある場合、またはその有効な基本クラスが `object` または `System.ValueType`でない場合、型パラメーターは***参照型で***あると認識されます。

制約付きの型パラメーター型の値を使用して、制約によって暗黙的に指定されたインスタンスメンバーにアクセスできます。 この例では、
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
`IPrintable` のメソッドは、`x` で直接呼び出すことができます。 `T` は常に `IPrintable`を実装するように制約されているためです。

### <a name="class-body"></a>クラス本体

クラスの*class_body*は、そのクラスのメンバーを定義します。

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>部分型

型宣言は、複数の***部分型宣言***にまたがって分割できます。 型宣言は、このセクションの規則に従ってパートから構築されます。とは、プログラムのコンパイル時および実行時の処理中に、単一の宣言として扱われます。

*Class_declaration*、 *struct_declaration*または*interface_declaration*は、`partial` 修飾子が含まれている場合、部分型宣言を表します。 `partial` はキーワードではなく、修飾子としてのみ機能します。これは、`class`キーワードの1つの直前、型宣言内、`struct` または `interface`、またはメソッド宣言内の型 `void` の前に記述されている場合にのみ機能します。 その他のコンテキストでは、通常の識別子として使用できます。

部分型の宣言の各部分には、`partial` 修飾子を含める必要があります。 同じ名前を持ち、他の部分と同じ名前空間または型宣言で宣言されている必要があります。 `partial` 修飾子は、型宣言の追加部分が別の場所に存在する可能性があることを示しますが、そのような追加の部分の存在は要件ではありません。これは、1つの宣言で `partial` 修飾子を含む型に対して有効です。

部分型のすべての部分を一緒にコンパイルして、コンパイル時にパートを1つの型宣言にマージできるようにする必要があります。 部分型では、コンパイル済みの型を拡張することはできません。

入れ子になった型は、`partial` 修飾子を使用して複数の部分で宣言できます。 通常、含んでいる型は `partial` を使用して宣言され、入れ子にされた型の各部分は、含んでいる型の別の部分で宣言されます。

`partial` 修飾子は、デリゲートまたは列挙型の宣言では許可されていません。

### <a name="attributes"></a>属性

部分型の属性は、各部分の属性を指定しない順序で組み合わせることによって決定されます。 属性が複数の部分に配置されている場合は、その型に対して属性を複数回指定することと同じです。 たとえば、次の2つの部分があります。

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
は、次のような宣言と同じです。
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

型パラメーターの属性は、同様の方法で結合されます。

### <a name="modifiers"></a>修飾子

部分型の宣言にアクセシビリティの仕様 (`public`、`protected`、`internal`、および `private` の修飾子) が含まれている場合、アクセシビリティの仕様を含むその他すべての部分に同意する必要があります。 部分型の一部にアクセシビリティの指定が含まれていない場合、型には、適切な既定のアクセシビリティ (宣言された[アクセシビリティ](basic-concepts.md#declared-accessibility)) が与えられます。

入れ子になった型の1つ以上の部分宣言に `new` 修飾子が含まれる場合、入れ子にされた型が継承されたメンバーを隠ぺいする ([継承によって隠ぺい](basic-concepts.md#hiding-through-inheritance)される) 場合、警告は報告されません。

クラスの1つ以上の部分宣言に `abstract` 修飾子が含まれている場合、クラスは抽象 ([抽象クラス](classes.md#abstract-classes)) と見なされます。 それ以外の場合、クラスは非抽象であると見なされます。

クラスの1つ以上の部分宣言に `sealed` 修飾子が含まれている場合、クラスは sealed ([sealed クラス](classes.md#sealed-classes)) と見なされます。 それ以外の場合、クラスはシールされていないと見なされます。

クラスに abstract と sealed の両方を指定することはできません。

部分型宣言で修飾子を使用すると、その特定の部分だけがunsafeコンテキスト(`unsafe`Unsafe コンテキスト[)と見なされ](unsafe-code.md#unsafe-contexts)ます。

### <a name="type-parameters-and-constraints"></a>型パラメーターと制約

ジェネリック型が複数の部分で宣言されている場合、各部分は型パラメーターを指定する必要があります。 各部分には、同じ数の型パラメーターと、それぞれの型パラメーターに対して同じ名前を指定する必要があります。

部分的なジェネリック型の宣言に制約 (`where` 句) が含まれている場合、制約は、制約を含むその他すべての部分と一致する必要があります。 具体的には、制約を含む各部分は、同じ型パラメーターのセットに対する制約を持つ必要があります。また、各型パラメーターには、primary、secondary、およびコンストラクターの各制約のセットが同等である必要があります。 同じメンバーが含まれている場合、2つの制約セットは同等です。 部分的なジェネリック型の一部で型パラメーターの制約が指定されていない場合、型パラメーターは制約なしと見なされます。

例
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
は、制約を含む部分 (最初の2つ) が、同じ型パラメーターのセットに対して同じ primary、secondary、およびコンストラクターの制約のセットを効果的に指定するため、正しいです。

### <a name="base-class"></a>基底クラス

部分クラスの宣言に基底クラスの指定が含まれている場合、基底クラスの指定を含む他のすべての部分に同意する必要があります。 部分クラスの一部に基底クラスの指定が含まれていない場合、基本クラスは `System.Object` ([基底クラス](classes.md#base-classes)) になります。

### <a name="base-interfaces"></a>基本インターフェイス

複数の部分で宣言された型の基本インターフェイスのセットは、各部分で指定された基本インターフェイスの和集合です。 特定の基本インターフェイスには、各部分で1回だけ名前を付けることができますが、複数の部分で同じ基本インターフェイスに名前を付けることができます。 特定の基本インターフェイスのメンバーの実装は1つだけ必要です。

この例では、
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
クラス `C` の基本インターフェイスのセットは、`IA`、`IB`、および `IC`です。

通常、各部分は、その部分で宣言されたインターフェイスの実装を提供します。ただし、これは要件ではありません。 パートは、別の部分で宣言されたインターフェイスの実装を提供する場合があります。
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a>メンバー

部分メソッド ([部分メソッド](classes.md#partial-methods)) を除き、複数の部分で宣言された型のメンバーのセットは、単に各部分で宣言されたメンバーのセットの和集合になります。 型宣言のすべての部分の本体は同じ宣言空間 ([宣言](basic-concepts.md#declarations)) を共有し、各メンバー ([スコープ](basic-concepts.md#scopes)) のスコープはすべての部分の本体に拡張されます。 メンバーのアクセシビリティドメインには、それを囲む型のすべての部分が常に含まれます。あるパートで宣言された `private` メンバーは、別のパートから自由にアクセスできます。 型の複数の部分で同じメンバーを宣言する場合、そのメンバーが `partial` 修飾子を持つ型である場合を除き、コンパイル時にエラーが発生します。

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

型内のメンバーの順序付けは、コードにC#とってはあまり重要ではありませんが、他の言語や環境とのやり取りには大きな意味があります。 このような場合、複数の部分で宣言された型内のメンバーの順序は未定義です。

### <a name="partial-methods"></a>部分メソッド

部分メソッドは、型宣言の1つの部分で定義し、別ので実装することができます。 実装は省略可能です。部分メソッドを実装している部分がない場合は、部分メソッドの宣言とその部分のすべての呼び出しが、その部分の組み合わせによって生成される型宣言から削除されます。

部分メソッドはアクセス修飾子を定義できませんが、暗黙的に `private`ます。 戻り値の型は `void`である必要があり、そのパラメーターには `out` 修飾子を指定できません。 識別子 `partial` は、`void` 型の直前に出現する場合にのみ、メソッド宣言で特別なキーワードとして認識されます。それ以外の場合は、通常の識別子として使用できます。 部分メソッドは、インターフェイスメソッドを明示的に実装することはできません。

部分メソッド宣言には2種類あります。メソッド宣言の本体がセミコロンの場合、宣言は***部分メソッド宣言の定義***と呼ばれます。 本文が*ブロック*として指定されている場合、宣言は***部分メソッド宣言の実装***と呼ばれます。 型宣言の一部では、特定のシグネチャを持つ部分メソッド宣言を1つだけ定義できます。また、特定のシグネチャを持つ実装部分メソッド宣言は1つだけです。 実装部分メソッド宣言が指定されている場合は、対応する部分メソッド宣言を定義する必要があり、宣言は次のように指定されている必要があります。

* 宣言には、同じ修飾子 (必ずしも同じ順序ではありません)、メソッド名、型パラメーターの数、およびパラメーターの数を指定する必要があります。
* 宣言内の対応するパラメーターは、同じ修飾子を持つ必要があります (必ずしも同じ順序ではありません)。また、型パラメーター名のモジュロが異なる場合もあります。
* 宣言内の対応する型パラメーターは、同じ制約を持つ必要があります (型パラメーター名の剰余の違い)。

部分メソッド宣言の実装は、対応する部分メソッド宣言を定義するのと同じ部分に記述できます。

オーバーロードの解決に参加するのは、部分メソッドの定義のみです。 したがって、実装宣言が指定されているかどうかにかかわらず、呼び出し式は部分メソッドの呼び出しに解決される可能性があります。 部分メソッドは常に `void`を返すため、このような呼び出し式は常に式ステートメントになります。 さらに、部分メソッドは暗黙的に `private`されるため、このようなステートメントは、部分メソッドが宣言されている型宣言のいずれかの部分で常に発生します。

部分型宣言の一部に、特定の部分メソッドの実装宣言が含まれていない場合、その部分メソッドを呼び出す式ステートメントは、結合された型宣言から単純に削除されます。 したがって、構成式を含む呼び出し式は、実行時に効果がありません。 部分メソッド自体も削除され、結合された型宣言のメンバーにはなりません。

実装宣言が特定の部分メソッドに存在する場合は、部分メソッドの呼び出しが保持されます。 部分メソッドは、次の点を除いて、部分メソッド宣言の実装に似たメソッド宣言になります。

* `partial` 修飾子は含まれていません
* 結果として得られるメソッドの宣言の属性は、定義の属性と実装部分メソッドの宣言を、指定されていない順序で結合したものです。 重複は削除されません。
* 結果として得られるメソッド宣言のパラメーターの属性は、定義の対応するパラメーターの属性と、部分メソッドの実装を指定しない順序での実装の属性を組み合わせたものです。 重複は削除されません。

実装宣言ではなく、定義宣言が部分メソッド M に対して指定されている場合、次の制限が適用されます。

* メソッドへのデリゲート ([デリゲート作成式](expressions.md#delegate-creation-expressions)) を作成するためのコンパイル時エラーです。
* 式ツリー型に変換される匿名関数内の `M` を参照するコンパイル時エラーです ([式ツリー型への匿名関数変換の評価](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types))。
* `M` の呼び出しの一部として出現する式は、確実な代入の状態 ([明確な代入](variables.md#definite-assignment)) には影響しません。これにより、コンパイル時エラーが発生する可能性があります。
* `M` アプリケーションのエントリポイントにすることはできません ([アプリケーションの起動](basic-concepts.md#application-startup))。

部分メソッドは、1つの型宣言の一部を使用して、ツールによって生成された別のパートの動作をカスタマイズできるようにする場合に便利です。 次の部分クラス宣言について考えてみます。
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

このクラスが他の部分なしでコンパイルされた場合、定義部分メソッド宣言とその呼び出しは削除され、結果として得られるクラス宣言は次のようになります。
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

ただし、部分メソッドの実装宣言を提供する別のパートが指定されているとします。
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

次に、結果として得られる結合クラスの宣言は、次のようになります。
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a>名前のバインド

拡張可能な型の各部分は同じ名前空間内で宣言する必要がありますが、通常、各部分は異なる名前空間宣言内に記述されます。 したがって、各部分に異なる `using` ディレクティブ ([ディレクティブを使用](namespaces.md#using-directives)) が存在する場合があります。 1つの部分で単純な名前 ([型の推定](expressions.md#type-inference)) を解釈する場合、その部分を囲む名前空間宣言の `using` ディレクティブだけが考慮されます。 これにより、異なる部分で異なる意味を持つ同じ識別子が生成される可能性があります。
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a>クラス メンバー

クラスのメンバーは、その*class_member_declaration*によって導入されたメンバーと、直接基底クラスから継承されたメンバーで構成されます。

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

クラス型のメンバーは、次のカテゴリに分類されます。

*  定数。クラス ([定数](classes.md#constants)) に関連付けられている定数値を表します。
*  フィールド。クラス ([フィールド](classes.md#fields)) の変数です。
*  メソッド。クラスによって実行できる計算とアクションを実装します ([メソッド](classes.md#methods))。
*  プロパティ。名前付きの特性、およびこれらの特性 ([プロパティ](classes.md#properties)) の読み取りと書き込みに関連するアクションを定義します。
*  イベント。クラス ([イベント](classes.md#events)) によって生成される通知を定義します。
*  インデクサーは、クラスのインスタンスが配列 ([インデクサー](classes.md#indexers)) と同じ方法で (構文的に) インデックスを作成できるようにします。
*  演算子。クラスのインスタンスに適用できる式演算子を定義します ([演算子](classes.md#operators))。
*  クラスのインスタンスを初期化するために必要なアクションを実装するインスタンスコンストラクター ([インスタンスコンストラクター](classes.md#instance-constructors))
*  デストラクターは、クラスのインスタンスが完全に破棄 ([デストラクター](classes.md#destructors)) される前に実行されるアクションを実装します。
*  静的コンストラクター。クラス自体を初期化するために必要なアクション ([静的コンストラクター](classes.md#static-constructors)) を実装します。
*  型。クラス (入れ子にされた[型](classes.md#nested-types)) に対してローカルな型を表します。

実行可能コードを含むことができるメンバーは、クラス型の*関数メンバー*と総称されます。 クラス型の関数メンバーは、そのクラス型のメソッド、プロパティ、イベント、インデクサー、演算子、インスタンスコンストラクター、デストラクター、および静的コンストラクターです。

*Class_declaration*によって新しい宣言空間 ([宣言](basic-concepts.md#declarations)) が作成され、 *class_declaration*にすぐに含まれる*class_member_declaration*が、この宣言空間に新しいメンバーを導入します。 *Class_member_declaration*s には、次の規則が適用されます。

*  インスタンスコンストラクター、デストラクター、および静的コンストラクターには、すぐ外側のクラスと同じ名前を付ける必要があります。 他のすべてのメンバーには、すぐに外側のクラスの名前とは異なる名前を付ける必要があります。
*  定数、フィールド、プロパティ、イベント、または型の名前は、同じクラスで宣言されている他のすべてのメンバーの名前と異なる必要があります。
*  メソッドの名前は、同じクラスで宣言されている他のすべての非メソッドの名前と異なる必要があります。 さらに、メソッドのシグネチャ ([シグネチャとオーバーロード](basic-concepts.md#signatures-and-overloading)) は、同じクラスで宣言されている他のすべてのメソッドのシグネチャと異なる必要があります。また、同じクラスで宣言された2つのメソッドは、`ref` と `out`だけが異なるシグネチャを持つことはできません。
*  インスタンスコンストラクターのシグネチャは、同じクラスで宣言されている他のすべてのインスタンスコンストラクターのシグネチャと異なる必要があります。また、同じクラスで宣言された2つのコンストラクターは、`ref` と `out`のみが異なるシグネチャを持つことはできません。
*  インデクサーのシグネチャは、同じクラスで宣言されている他のすべてのインデクサーのシグネチャと異なる必要があります。
*  演算子のシグネチャは、同じクラスで宣言されている他のすべての演算子のシグネチャと異なる必要があります。

クラス型 ([継承](classes.md#inheritance)) の継承されたメンバーは、クラスの宣言空間の一部ではありません。 したがって、派生クラスでは、継承されたメンバーと同じ名前またはシグネチャを持つメンバーを宣言できます (これにより、継承されたメンバーは非表示になります)。

### <a name="the-instance-type"></a>インスタンスの種類

各クラス宣言には、バインドされた型 (バインドされた型とバインドされていない[型](types.md#bound-and-unbound-types))、***インスタンス型***があります。 ジェネリッククラス宣言では、型宣言から構築された型 ([構築](types.md#constructed-types)型) を作成し、指定された各型引数を対応する型パラメーターとして、インスタンス型を作成します。 インスタンス型は型パラメーターを使用するため、型パラメーターがスコープ内にある場合にのみ使用できます。つまり、クラス宣言の内部にあります。 インスタンス型は、クラス宣言内で記述されたコードの `this` の型です。 非ジェネリッククラスの場合、インスタンスの型は単に宣言されたクラスになります。 次に、いくつかのクラス宣言とそのインスタンス型を示します。 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>構築された型のメンバー

構築された型の非継承メンバーは、メンバー宣言内の各*type_parameter*に対して、構築された型の対応する*type_argument*に代入することによって取得されます。 置換プロセスは、型宣言のセマンティックの意味に基づいており、単なるテキスト置換ではありません。

たとえば、ジェネリッククラス宣言があるとします。
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
構築された型 `Gen<int[],IComparable<string>>` には、次のメンバーがあります。
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

ジェネリッククラス宣言のメンバー `a` の型 `Gen` は "`T`の2次元配列" です。そのため、上記の構築された型のメンバー `a` の型は、「`int`の1次元配列の2次元配列」、または「`int[,][]`」です。

インスタンス関数のメンバー内では、`this` の型は、包含する宣言のインスタンスの型 ([インスタンス型](classes.md#the-instance-type)) です。

ジェネリッククラスのすべてのメンバーは、外側の任意のクラスの型パラメーターを直接または構築された型の一部として使用できます。 実行時に特定のクローズ構築型 ([オープン型およびクローズ型](types.md#open-and-closed-types)) を使用する場合は、型パラメーターを使用するたびに、構築された型に渡される実際の型引数に置き換えられます。 例 :
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a>継承

クラスは、その直接の基底クラス型のメンバーを***継承***します。 継承とは、基底クラスのインスタンスコンストラクター、デストラクター、および静的コンストラクターを除く、クラスに直接基底クラス型のすべてのメンバーが暗黙的に含まれることを意味します。 継承の重要な側面は次のとおりです。

*  継承は推移的です。 `C` が `B`から派生し、`B` が `A`から派生している場合、`C` で宣言されたメンバーと `B` で宣言されているメンバーが継承されます。`A`
*  派生クラスは、その直接の基底クラスを拡張します。 派生クラスは、継承するメンバーに新しいメンバーを追加できますが、継承されたメンバーの定義を削除することはできません。
*  インスタンスコンストラクター、デストラクター、および静的コンストラクターは継承されませんが、宣言されたアクセシビリティ ([メンバーアクセス](basic-concepts.md#member-access)) に関係なく、他のすべてのメンバーはになります。 ただし、宣言されたアクセシビリティによっては、継承されたメンバーが派生クラスでアクセスできない場合があります。
*  派生クラスは、同じ名前またはシグネチャを持つ新しいメンバーを宣言することで、継承されたメンバー***を隠ぺい (*** [隠ぺい](basic-concepts.md#hiding-through-inheritance)) できます。 ただし、継承されたメンバーを非表示にしても、そのメンバーは削除されません。つまり、派生クラスを通じてそのメンバーに直接アクセスできないだけです。
*  クラスのインスタンスには、クラスとその基本クラスで宣言されたすべてのインスタンスフィールドのセットが含まれています。また、派生クラス型から基本クラス型への暗黙的な変換 ([暗黙的な参照変換](conversions.md#implicit-reference-conversions)) も存在します。 したがって、派生クラスのインスタンスへの参照は、その基底クラスのインスタンスへの参照として処理できます。
*  クラスは、仮想メソッド、プロパティ、インデクサーを宣言でき、派生クラスはこれらの関数メンバーの実装をオーバーライドできます。 これにより、クラスは、関数メンバー呼び出しによって実行されるアクションが、その関数メンバーの呼び出しに使用されるインスタンスのランタイム型によって異なる場合に、ポリモーフィックな動作を実現できます。

構築されたクラス型の継承されたメンバーは、直接的な基底クラス型 ([基底クラス](classes.md#base-classes)) のメンバーです。これは、 *class_base*仕様の対応する型パラメーターが出現するたびに、構築された型の型引数を置き換えることによって検出されます。 これらのメンバーは、メンバー宣言内の各*type_parameter*に対して、 *class_base*仕様の対応する*type_argument*という置換によって変換されます。

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

上の例では、構築された型 `D<int>` には、型パラメーター `T`の型引数 `int` を置き換えることによって取得される非継承メンバー `public int G(string s)` が含まれています。 `D<int>` には、`B`クラス宣言から継承されたメンバーもあります。 この継承されたメンバーは、基本クラスの指定 `B<T[]>`の `T` に `int` を置き換えることによって、`D<int>` の基底クラス型 `B<int[]>` を最初に決定することによって決定されます。 次に、`B`の型引数として、`int[]` が `public U F(long index)`の `U` に置き換えられ、継承されたメンバー `public int[] F(long index)`が生成されます。

### <a name="the-new-modifier"></a>新しい修飾子

*Class_member_declaration*は、継承されたメンバーと同じ名前またはシグネチャを持つメンバーを宣言できます。 この場合、派生クラスのメンバーは、基底クラスのメンバーを***非表示***にすると言います。 継承されたメンバーを非表示にすることは、エラーとは見なされませんが、コンパイラによって警告が発行されます。 警告を抑制するには、派生クラスメンバーの宣言に、派生メンバーが基本メンバーを非表示にすることを示す `new` 修飾子を含めることができます。 このトピックの詳細については、「[継承による非表示](basic-concepts.md#hiding-through-inheritance)」を参照してください。

継承されたメンバーを非表示にしない宣言に `new` 修飾子が含まれている場合、その効果に対する警告が発行されます。 この警告は、`new` 修飾子を削除することによって抑制されます。

### <a name="access-modifiers"></a>アクセス修飾子

*Class_member_declaration*には、`public`、`protected internal`、`protected`、`internal`、または `private`という5種類の宣言され[たアクセシビリティの](basic-concepts.md#declared-accessibility)うちいずれか1つを使用できます。 `protected internal` の組み合わせを除き、複数のアクセス修飾子を指定するコンパイル時エラーになります。 *Class_member_declaration*にアクセス修飾子が含まれていない場合、`private` が想定されます。

### <a name="constituent-types"></a>構成型

メンバーの宣言で使用される型は、そのメンバーの構成型と呼ばれます。 使用可能な構成型は、定数、フィールド、プロパティ、イベント、またはインデクサーの型、メソッドまたは演算子の戻り値の型、およびメソッド、インデクサー、演算子、またはインスタンスコンストラクターのパラメーターの型です。 メンバーの構成型は、少なくともそのメンバー自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。

### <a name="static-and-instance-members"></a>静的メンバーとインスタンスメンバー

クラスのメンバーは、***静的メンバー***または***インスタンスメンバー***です。 一般に、静的メンバーは、オブジェクト (クラス型のインスタンス) に属しているクラス型およびインスタンスメンバーに属していると考えると便利です。

フィールド、メソッド、プロパティ、イベント、演算子、またはコンストラクターの宣言に `static` 修飾子が含まれている場合は、静的メンバーを宣言します。 また、定数または型の宣言は、暗黙的に静的メンバーを宣言します。 静的メンバーには次の特性があります。

*  静的メンバー `M` が `E.M`フォームの*member_access* ([メンバーアクセス](expressions.md#member-access)) で参照されている場合、`E` は `M`を含む型を示す必要があります。 `E` がインスタンスを示す場合、コンパイル時エラーになります。
*  静的フィールドは、指定された closed クラス型のすべてのインスタンスによって共有されるストレージの場所を1つだけ識別します。 指定されたクローズクラス型のインスタンスの数に関係なく、静的フィールドのコピーは1つしか存在しません。
*  静的関数メンバー (メソッド、プロパティ、イベント、演算子、またはコンストラクター) は、特定のインスタンスでは動作しません。このような関数メンバーの `this` を参照するには、コンパイル時エラーになります。

フィールド、メソッド、プロパティ、イベント、インデクサー、コンストラクター、またはデストラクターの宣言に `static` 修飾子が含まれていない場合、インスタンスメンバーが宣言されます。 (インスタンスメンバーは非静的メンバーと呼ばれることもあります)。インスタンスメンバーには次の特性があります。

*  インスタンスメンバー `M` が `E.M`フォームの*member_access* ([メンバーアクセス](expressions.md#member-access)) で参照されている場合、`E` は `M`を含む型のインスタンスを示す必要があります。 `E` が型を表している場合、バインド時エラーになります。
*  クラスのすべてのインスタンスには、クラスのすべてのインスタンスフィールドの個別のセットが含まれています。
*  インスタンス関数メンバー (メソッド、プロパティ、インデクサー、インスタンスコンストラクター、またはデストラクター) は、クラスの特定のインスタンスで動作し、このインスタンスには `this` ([このアクセス](expressions.md#this-access)) としてアクセスできます。

次の例は、静的メンバーとインスタンスメンバーにアクセスするための規則を示しています。
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

`F` メソッドは、インスタンス関数メンバーで*simple_name* ([簡易名](expressions.md#simple-names)) を使用して、インスタンスメンバーと静的メンバーの両方にアクセスできることを示しています。 `G` メソッドは、静的関数メンバーで、 *simple_name*を介してインスタンスメンバーにアクセスするためのコンパイル時エラーであることを示しています。 `Main` メソッドは、 *member_access* ([メンバーアクセス](expressions.md#member-access)) でインスタンスメンバーにインスタンスを介してアクセスする必要があり、静的メンバーには型を使用してアクセスする必要があることを示しています。

### <a name="nested-types"></a>入れ子にされた型

クラスまたは構造体の宣言内で宣言された型は、入れ子にされた***型***と呼ばれます。 コンパイル単位または名前空間内で宣言された型は、入れ子になってい***ない型***と呼ばれます。

この例では、
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
クラス `B` はクラス `A`内で宣言されているため、入れ子にされた型です。クラス `A` はコンパイル単位内で宣言されているので、入れ子になっていない型です。

#### <a name="fully-qualified-name"></a>完全修飾名

入れ子にされた型の完全修飾名 ([完全修飾](basic-concepts.md#fully-qualified-names)名) は `S.N` です。 `S` は、`N` が宣言されている型の完全修飾名です。

#### <a name="declared-accessibility"></a>アクセシビリティの宣言

入れ子になっていない型は、`public` または `internal` 宣言されたアクセシビリティを持つことができ、既定では `internal` として宣言されています。 入れ子にされた型には、これらの形式のアクセシビリティを含めることができます。また、包含する型がクラスまたは構造体であるかどうかによって、宣言されたアクセシビリティの1つ以上の追加の形式を使用できます。

*  クラス内で宣言されている入れ子にされた型は、5つの形式のアクセシビリティ (`public`、`protected internal`、`protected`、`internal`、または `private`) を持つことができ、他のクラスメンバーと同様に、既定では `private` 宣言されたアクセシビリティになります。
*  構造体で宣言されている入れ子にされた型は、3つの形式のアクセシビリティ (`public`、`internal`、または `private`) を持つことができ、他の構造体のメンバーと同様に、既定では `private` 宣言されたアクセシビリティを持つことになります。

例
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
入れ子になったプライベートクラス `Node`を宣言します。

#### <a name="hiding"></a>非表示

入れ子にされた型では、基本メンバーを非表示にすることができます ([名前の非](basic-concepts.md#name-hiding)表示)。 `new` 修飾子は入れ子になった型宣言で許可されているため、非表示を明示的に表すことができます。 例
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
`Base`で定義されている `M` メソッドを非表示にする `M` 入れ子になったクラスを示します。

#### <a name="this-access"></a>このアクセス権

入れ子にされた型とそれを含んでいる型には、 *this_access* ([このアクセス](expressions.md#this-access)) に関して特別な関係はありません。 具体的には、入れ子にされた型内の `this` は、それを含んでいる型のインスタンスメンバーを参照するために使用することはできません。 入れ子にされた型が、それを含んでいる型のインスタンスメンバーにアクセスする必要がある場合は、入れ子にされた型のコンストラクター引数として、包含する型のインスタンスの `this` を指定することにより、アクセスを提供できます。 次の例では、
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
この手法を示します。 `C` のインスタンスは `Nested` のインスタンスを作成し、その独自の `this` を `Nested`のコンストラクターに渡して、後続の `C`のインスタンスメンバーへのアクセスを提供します。

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>包含する型のプライベートメンバーとプロテクトメンバーへのアクセス

入れ子になった型は、含まれている型にアクセスできるすべてのメンバーにアクセスできます。これには、`private` があり、`protected` 宣言されたアクセシビリティを含む型のメンバーも含まれます。 例
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
入れ子になったクラス `Nested`を含む `C` クラスを表示します。 `Nested`内では、メソッドは `C`で定義されている `F` 静的メソッドを呼び出し `G`、プライベートで宣言されたアクセシビリティを `F` します。

入れ子になった型は、それを含んでいる型の基本型で定義されているプロテクトメンバーにもアクセスできます。 この例では、
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
入れ子になったクラスは、`Derived`のインスタンスを介してを呼び出すことによって、`Derived`の基本 `Base`クラスで定義されている保護されたメソッド `F` `Derived.Nested` アクセスします。

#### <a name="nested-types-in-generic-classes"></a>ジェネリッククラスの入れ子にされた型

ジェネリッククラスの宣言には、入れ子にされた型宣言を含めることができます。 外側のクラスの型パラメーターは、入れ子にされた型内で使用できます。 入れ子になった型宣言には、入れ子になった型にのみ適用される追加の型パラメーターを含めることができます。

ジェネリッククラスの宣言内に含まれるすべての型宣言は、暗黙的にジェネリック型宣言になります。 ジェネリック型内で入れ子にされた型への参照を書き込む場合は、その型引数を含む構築型の型に名前を付ける必要があります。 ただし、外側のクラス内では、入れ子にされた型は修飾なしで使用できます。外側のクラスのインスタンスの型は、入れ子にされた型を構築するときに暗黙的に使用できます。 次の例は、`Inner`から作成された構築済みの型を参照するための3つの異なる正しい方法を示しています。最初の2つは同等です。
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

無効なプログラミングスタイルですが、入れ子にされた型の型パラメーターは、外側の型で宣言されたメンバーまたは型パラメーターを隠ぺいすることができます。
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>予約されたメンバー名

基になるC#ランタイム実装を容易にするために、プロパティ、イベント、またはインデクサーであるソースメンバーの宣言ごとに、実装は、メンバー宣言の種類、名前、および型に基づいて、2つのメソッドシグネチャを予約する必要があります。 基になるランタイム実装でこれらの予約が使用されていない場合でも、シグネチャがこれらの予約済み署名のいずれかに一致するメンバーを宣言するプログラムでは、コンパイル時エラーになります。

予約された名前は宣言を導入しないため、メンバーの検索には関与しません。 ただし、宣言に関連付けられた予約済みメソッドシグネチャは継承 ([継承](classes.md#inheritance)) に参加し、`new` 修飾子 ([新しい修飾子](classes.md#the-new-modifier)) で非表示にすることができます。

これらの名前の予約は、次の3つの目的で機能します。

*  基になる実装が、 C#言語機能への get または set アクセスのために、通常の識別子をメソッド名として使用できるようにする場合は。
*  C#言語機能へのアクセスを取得または設定するためのメソッド名として、他の言語が通常の識別子を使用して相互運用できるようにする場合は。
*  1つの準拠コンパイラによって受け入れられるソースが、すべてC#の実装で一貫して予約されたメンバー名の詳細を持つようにするために、別のコンパイラによって受け入れられるようにします。

デストラクター ([デストラクター](classes.md#destructors)) の宣言によって、シグネチャも予約されます (デストラクター用に予約された[メンバー名](classes.md#member-names-reserved-for-destructors))。

#### <a name="member-names-reserved-for-properties"></a>プロパティ用に予約されたメンバー名

`T`型のプロパティ `P` ([プロパティ](classes.md#properties)) の場合、次のシグネチャが予約されています。

```csharp
T get_P();
void set_P(T value);
```

どちらの署名も、プロパティが読み取り専用または書き込み専用であっても予約されています。

この例では、
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
読み取り専用プロパティ `P`を定義し、`get_P` および `set_P` メソッドの署名を予約 `A` クラス。 クラス `B` `A` から派生し、これらの予約済み署名の両方を非表示にします。 この例では、次の出力が生成されます。
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>イベント用に予約されたメンバー名

デリゲート型`E` のイベント([イベント](classes.md#events))の場合、次のシグネチャが予約されてい`T`ます。
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>インデクサーに予約されたメンバー名

パラメーターリスト[ を持つ型のインデクサー(](classes.md#indexers)インデクサー`T`)の場合、次のシグネチャが予約されてい`L`ます。
```csharp
T get_Item(L);
void set_Item(L, T value);
```

インデクサーが読み取り専用または書き込み専用の場合でも、両方の署名が予約されます。

さらに、メンバー名 `Item` は予約されています。

#### <a name="member-names-reserved-for-destructors"></a>デストラクター用に予約されたメンバー名

デストラクター ([デストラクター](classes.md#destructors)) を含むクラスでは、次のシグネチャが予約されています。
```csharp
void Finalize();
```

## <a name="constants"></a>定数

***定数***は、定数値を表すクラスメンバーです。この値は、コンパイル時に計算できます。 *Constant_declaration*は、指定された型の1つ以上の定数を導入します。

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

*Constant_declaration*には、一連の*属性*([属性](attributes.md))、`new` 修飾子 ([新しい修飾子](classes.md#the-new-modifier))、および4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせを含めることができます。 属性と修飾子は、 *constant_declaration*によって宣言されたすべてのメンバーに適用されます。 定数は静的メンバーと見なされますが、 *constant_declaration*では、`static` 修飾子を必要とすることも許可することもありません。 同じ修飾子が定数宣言で複数回出現する場合、エラーになります。

*Constant_declaration*の*型*は、宣言によって導入されるメンバーの型を指定します。 型には*constant_declarator*のリストが続き、それぞれに新しいメンバーが導入されています。 *Constant_declarator*は、メンバーに*名前を付け*、その後に "`=`" トークンを続け、その後にメンバーの値を指定する*constant_expression* ([定数式](expressions.md#constant-expressions)) で構成されます。

定数宣言で指定される*型*は、`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、または *`bool`である*必要が*あります。* `string` 各*constant_expression*は、暗黙的な変換 ([暗黙的](conversions.md#implicit-conversions)な変換) によって対象の型に変換できる、ターゲット型または型の値を生成する必要があります。

定数の*型*は、少なくとも定数自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。

定数の値は、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバーアクセス](expressions.md#member-access)) を使用して式で取得されます。

定数は、 *constant_expression*に参加させることができます。 したがって、定数は、 *constant_expression*を必要とするすべてのコンストラクトで使用できます。 このような構成体の例としては、`case` ラベル、`goto case` ステートメント、`enum` メンバー宣言、属性、その他の定数宣言などがあります。

「[定数式](expressions.md#constant-expressions)」で説明されているように、 *constant_expression*は、コンパイル時に完全に評価できる式です。 `string` 以外の*reference_type*の null 以外の値を作成する唯一の方法は、`new` 演算子を適用することです。また、`new` 演算子は*constant_expression*では許可されていないため、reference_type 以外の *`string` の定数*で使用できる値は `null`だけです。

定数値のシンボリック名が必要であるが、その値の型が定数宣言で許可されていない場合、またはコンパイル時に*constant_expression*によって値を計算できない場合は、代わりに `readonly` フィールド ([Readonly フィールド](classes.md#readonly-fields)) を使用できます。

複数の定数を宣言する定数宣言は、同じ属性、修飾子、および型を持つ単一の定数の複数の宣言と同じです。 次に例を示します。
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
上記の式は、次の式と同じです。
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

依存関係が循環的な性質を持たない限り、定数は同じプログラム内の他の定数に依存することができます。 コンパイラは、定数宣言を適切な順序で評価するように自動的に配置します。 この例では、
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
コンパイラは最初に `A.Y`を評価した後、`B.Z`を評価し、最後に `A.X`を評価して、`10`、`11`、および `12`の値を生成します。 定数宣言は、他のプログラムの定数に依存する場合がありますが、このような依存関係は一方向でしか使用できません。 上記の例を参照して `A` と `B` が個別のプログラムで宣言されている場合は、`A.X` が `B.Z`に依存していても、`B.Z` に同時に依存していない可能性があります。`A.Y`

## <a name="fields"></a>フィールド

***フィールド***は、オブジェクトまたはクラスに関連付けられた変数を表すメンバーです。 *Field_declaration*には、指定された型の1つ以上のフィールドが導入されています。

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

*Field_declaration*には、一連の*属性*([属性](attributes.md))、`new` 修飾子 ([新しい修飾子](classes.md#the-new-modifier))、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせ、および `static` 修飾子 ([静的およびインスタンスフィールド](classes.md#static-and-instance-fields)) を含めることができます。 また、 *field_declaration*には、`readonly` 修飾子 ([Readonly フィールド](classes.md#readonly-fields)) または `volatile` 修飾子 ([Volatile フィールド](classes.md#volatile-fields)) を含めることができますが、両方を含めることはできません。 属性と修飾子は、 *field_declaration*によって宣言されたすべてのメンバーに適用されます。 同じ修飾子がフィールド宣言に複数回出現する場合、エラーになります。

*Field_declaration*の*型*は、宣言によって導入されるメンバーの型を指定します。 型には*variable_declarator*のリストが続き、それぞれに新しいメンバーが導入されています。 *Variable_declarator*は、そのメンバーに名前を付け、その後に "`=`" トークンと、そのメンバーの初期値を指定する*variable_initializer* ([変数初期化子](classes.md#variable-initializers)) を指定する*識別子*で構成されます。

フィールドの*型*は、少なくともフィールド自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。

フィールドの値は、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバーアクセス](expressions.md#member-access)) を使用して式で取得されます。 非読み取り専用フィールドの値は、*代入*[演算子 (代入演算子](expressions.md#assignment-operators)) を使用して変更されます。 非 readonly フィールドの値は、後置インクリメント演算子とデクリメント演算子 ([後置](expressions.md#postfix-increment-and-decrement-operators)インクリメント演算子およびデクリメント演算子) と前置インクリメント演算子 (前置インクリメント演算子および前置[デクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)) を使用して取得および変更できます。

複数のフィールドを宣言するフィールド宣言は、同じ属性、修飾子、および型を持つ1つのフィールドの複数の宣言と同じです。 次に例を示します。
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
上記の式は、次の式と同じです。
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a>静的フィールドとインスタンスフィールド

フィールド宣言に `static` 修飾子が含まれている場合、宣言によって導入されるフィールドは***静的フィールド***です。 `static` 修飾子が存在しない場合、宣言によって導入されるフィールドは***インスタンスフィールド***になります。 C#静的フィールドおよびインスタンスフィールドは、によってサポートされるいくつかの種類の変数 ([変数](variables.md)) のうち、それぞれ***静的変数***と***インスタンス変数***として参照される場合があります。

静的フィールドは、特定のインスタンスの一部ではありません。代わりに、閉じられた型のすべてのインスタンス間で共有されます ([オープン型およびクローズ型](types.md#open-and-closed-types))。 クローズされたクラス型のインスタンスの数に関係なく、関連付けられているアプリケーションドメインの静的フィールドのコピーは1つしかありません。

例 :
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

インスタンスフィールドはインスタンスに属します。 具体的には、クラスのすべてのインスタンスには、そのクラスのすべてのインスタンスフィールドのセットが個別に含まれています。

フィールドが `E.M`フォームの*member_access* ([メンバーアクセス](expressions.md#member-access)) で参照されている場合、`M` が静的フィールドである場合、`E` は `M`を含む型を表している必要があります。また、`M` がインスタンスフィールドの場合、E は `M`を含む型のインスタンスを示す必要があります。

静的メンバーとインスタンスメンバーの違いについては、「[静的メンバーとインスタンス](classes.md#static-and-instance-members)メンバー」で詳しく説明します。

### <a name="readonly-fields"></a>読み取り専用フィールド

*Field_declaration*に `readonly` 修飾子が含まれている場合、宣言によって導入されるフィールドは***読み取り専用フィールド***です。 読み取り専用フィールドへの直接割り当ては、その宣言の一部として、または同じクラスのインスタンスコンストラクターまたは静的コンストラクターでのみ発生します。 (読み取り専用フィールドは、これらのコンテキストで複数回割り当てることができます)。具体的には、`readonly` フィールドへの直接割り当ては、次のコンテキストでのみ許可されます。

*  (宣言に*variable_initializer*を含めることによって) フィールドが導入されている*variable_declarator* 。
*  インスタンスフィールドの場合、フィールド宣言を含むクラスのインスタンスコンストラクター。静的フィールドの場合は、フィールド宣言を含むクラスの静的コンストラクター。 これらは、`readonly` フィールドを `out` または `ref` パラメーターとして渡すことができる唯一のコンテキストでもあります。

`readonly` フィールドに割り当てたり、他のコンテキストで `out` または `ref` パラメーターとして渡したりしようとすると、コンパイル時エラーになります。

#### <a name="using-static-readonly-fields-for-constants"></a>静的読み取り専用フィールドを定数に使用する

`static readonly` フィールドは、定数値のシンボル名が必要なときに、値の型が `const` 宣言で許可されていない場合、またはコンパイル時に値を計算できない場合に便利です。 この例では、
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
`Black`、`White`、`Red`、`Green`、および `Blue` のメンバーは、コンパイル時に値を計算できないため、`const` メンバーとして宣言することはできません。 ただし、これらの `static readonly` 宣言することは、ほとんど同じ効果を持ちます。

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>定数と静的な読み取り専用フィールドのバージョン管理

定数および読み取り専用フィールドには、異なるバイナリバージョン管理セマンティクスがあります。 式が定数を参照する場合、定数の値はコンパイル時に取得されますが、式が読み取り専用フィールドを参照する場合、フィールドの値は実行時まで取得されません。 2つの独立したプログラムで構成されるアプリケーションを考えてみましょう。
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

`Program1` と `Program2` の名前空間は、個別にコンパイルされる2つのプログラムを表します。 `Program1.Utils.X` が静的な読み取り専用フィールドとして宣言されているため、`Console.WriteLine` ステートメントによって出力される値はコンパイル時には認識されませんが、実行時に取得されます。 したがって、`X` の値が変更され `Program1` が再コンパイルされた場合、`Program2` が再コンパイルされない場合でも、`Console.WriteLine` ステートメントによって新しい値が出力されます。 ただし、`X` が定数であった場合、`X` の値は `Program2` のコンパイル時に取得され、`Program2` が再コンパイルされるまで `Program1` の変更による影響を受けません。

### <a name="volatile-fields"></a>Volatile フィールド

*Field_declaration*に `volatile` 修飾子が含まれている場合、その宣言によって導入されるフィールドは***volatile フィールド***です。

非 volatile フィールドの場合、命令の順序を変更する最適化手法は、予期しない、予測できない結果になる可能性があります。これは、 *lock_statement* ([lock ステートメント](statements.md#the-lock-statement)) によって提供されるような同期を行わずにフィールドにアクセスするマルチスレッドプログラムです。 これらの最適化は、コンパイラ、ランタイムシステム、またはハードウェアによって実行できます。 Volatile フィールドの場合、このような並べ替えの最適化は制限されます。

*  Volatile フィールドの読み取りは、 ***volatile 読み取り***と呼ばれます。 揮発性読み取りには "取得セマンティクス" があります。つまり、命令シーケンス内で発生したメモリへの参照の前に発生することが保証されます。
*  Volatile フィールドの書き込みは、 ***volatile 書き込み***と呼ばれます。 Volatile 書き込みには "リリースセマンティクス" があります。つまり、命令シーケンスで書き込み命令より前のメモリ参照の後に発生することは保証されます。

これらの制限により、すべてのスレッドが、他のスレッドによって実行された volatile の書き込みを、実行された順序で観察することが保証されます。 すべての実行スレッドから見たように、volatile 書き込みの単一の合計順序を指定するために、準拠している実装は必要ありません。 Volatile フィールドの型は、次のいずれかである必要があります。

*  *Reference_type*。
*  型 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`char`、`float`、`bool`、`System.IntPtr`、または `System.UIntPtr`。
*  `byte`、`sbyte`、`short`、`ushort`、`int`、または `uint`の enum 基本型を持つ*enum_type* 。

例
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
この例では、次のように出力されます。
```console
result = 143
```

この例では、メソッド `Main` `Thread2`メソッドを実行する新しいスレッドを開始します。 このメソッドは、値を `result`という非 volatile フィールドに格納し、`true` を volatile フィールド `finished`に格納します。 メインスレッドは、フィールド `finished` が `true`に設定されるのを待機し、`result`フィールドを読み取ります。 `finished` は `volatile`として宣言されているため、メインスレッドは、フィールド `result`から `143` 値を読み取る必要があります。 フィールド `finished` が `volatile``result` として宣言されていない場合、ストアが `finished`する前にメインスレッドに表示されるようにすることができます。したがって、メインスレッドは `0` フィールドからの値を読み取ることができます。`result` `finished` を `volatile` フィールドとして宣言すると、そのような不整合を防ぐことができます。

### <a name="field-initialization"></a>フィールドの初期化

フィールドの初期値は、静的フィールドであるか、インスタンスフィールドであるかにかかわらず、フィールドの型の既定値 ([既定](variables.md#default-values)値) になります。 この既定の初期化が発生する前にフィールドの値を確認することはできず、フィールドは "初期化されていません" にはなりません。 例
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
この例では、次のように出力されます。
```console
b = False, i = 0
```
`b` と `i` は両方とも既定値に自動的に初期化されるためです。

### <a name="variable-initializers"></a>変数初期化子

フィールド宣言には*variable_initializer*s を含めることができます。 静的フィールドの場合、変数初期化子は、クラスの初期化中に実行される代入ステートメントに対応します。 インスタンスフィールドの場合、変数初期化子は、クラスのインスタンスが作成されるときに実行される代入ステートメントに対応します。

例
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
この例では、次のように出力されます。
```console
x = 1.4142135623731, i = 100, s = Hello
```
`x` への代入は、静的フィールド初期化子が実行され、`i` に割り当てられ、インスタンスフィールド初期化子の実行時に `s` 発生する場合に発生します。

「[フィールドの初期化](classes.md#field-initialization)」で説明されている既定値の初期化は、変数初期化子を持つフィールドを含むすべてのフィールドに対して行われます。 したがって、クラスが初期化されると、そのクラスのすべての静的フィールドが最初に既定値に初期化され、次に静的フィールド初期化子がテキスト順に実行されます。 同様に、クラスのインスタンスが作成されると、そのインスタンス内のすべてのインスタンスフィールドが最初に既定値に初期化され、次に、インスタンスフィールド初期化子がテキスト順に実行されます。

変数初期化子を持つ静的フィールドを既定値の状態で観察することができます。 ただし、スタイルの問題としては、この方法は推奨されません。 例
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
この動作を行います。 A と b の循環定義にかかわらず、プログラムは有効です。 結果が出力されます。
```console
a = 1, b = 2
```
静的フィールド `a` と `b` は、初期化子が実行される前に `0` (`int`の既定値) に初期化されるためです。 `a` の初期化子が実行されると、`b` の値が0になり、`a` が `1`に初期化されます。 `b` の初期化子を実行すると、`a` の値は既に `1`ため、`b` は `2`に初期化されます。

#### <a name="static-field-initialization"></a>静的フィールドの初期化

クラスの静的フィールド変数初期化子は、クラス宣言に出現するテキスト順に実行される割り当てのシーケンスに対応します。 静的コンストラクター ([静的](classes.md#static-constructors)コンストラクター) がクラスに存在する場合、静的コンストラクターを実行する直前に静的フィールド初期化子が実行されます。 それ以外の場合、静的フィールド初期化子は、そのクラスの静的フィールドを最初に使用する前に、実装に依存する時刻に実行されます。 例
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
では、次の出力が生成される場合があります。
```console
Init A
Init B
1 1
```
または、次のように出力します。
```console
Init B
Init A
1 1
```
`X`の初期化子と `Y`の初期化子の実行は、どちらの順序でも発生する可能性があります。これらのフィールドへの参照の前にのみ発生するように制限されています。 ただし、この例では次のようになります。
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
出力は次のようになります。
```console
Init B
Init A
1 1
```
静的コンストラクターを実行するときの規則 ([静的コンストラクター](classes.md#static-constructors)で定義されているように) は、`B`の静的コンストラクター (したがって `B`の静的フィールド初期化子) を `A`の静的コンストラクターとフィールド初期化子の前に実行する必要があるためです。

#### <a name="instance-field-initialization"></a>インスタンスフィールドの初期化

クラスのインスタンスフィールド変数初期化子は、そのクラスのインスタンスコンストラクター ([コンストラクター初期化子](classes.md#constructor-initializers)) のいずれかにエントリしたときに直ちに実行される割り当てのシーケンスに対応します。 変数初期化子は、クラス宣言に含まれるテキスト順に実行されます。 クラスインスタンスの作成と初期化のプロセスについては、「[インスタンスコンストラクター](classes.md#instance-constructors)」を参照してください。

インスタンスフィールドの変数初期化子では、作成されるインスタンスを参照できません。 このため、変数初期化子で `this` を参照するにはコンパイル時エラーが発生します。これは、変数初期化子が*simple_name*を通じて任意のインスタンスメンバーを参照するためのコンパイル時エラーであるためです。 この例では、
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
`y` の変数初期化子は、作成されるインスタンスのメンバーを参照するため、コンパイル時エラーになります。

## <a name="methods"></a>メソッド

"***メソッド***" は、オブジェクトまたはクラスによって実行可能な計算またはアクションを実装するメンバーです。 メソッドは*method_declaration*s を使用して宣言されます。

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

*Method_declaration*には、*属性*([属性](attributes.md)) のセットと、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせ、`new` ([新しい修飾子](classes.md#the-new-modifier))、`static` ([静的メソッドとインスタンスメソッド](classes.md#static-and-instance-methods))、`virtual` ([仮想メソッド](classes.md#virtual-methods))、`override` ([オーバーライドメソッド](classes.md#override-methods))、([シールメソッド](classes.md#sealed-methods))、`sealed` ([抽象メソッド](classes.md#abstract-methods))、および `abstract` ([外部メソッド](classes.md#external-methods)) 修飾子を含めることができます。`extern`

次のすべての条件を満たす場合、宣言には修飾子の有効な組み合わせがあります。

*  この宣言には、アクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせが含まれています。
*  宣言に、同じ修飾子が複数回含まれていません。
*  宣言には、`static`、`virtual`、および `override`の修飾子が1つだけ含まれています。
*  宣言には、`new` と `override`の修飾子が1つだけ含まれています。
*  宣言に `abstract` 修飾子が含まれている場合、宣言には、`static`、`virtual`、`sealed` または `extern`の修飾子は含まれません。
*  宣言に `private` 修飾子が含まれている場合、宣言には、`virtual`、`override`、または `abstract`の各修飾子は含まれません。
*  宣言に `sealed` 修飾子が含まれている場合、宣言には `override` 修飾子も含まれます。
*  宣言に `partial` 修飾子が含まれている場合は、`new`、`public`、`protected`、`internal`、`private`、`virtual`、`sealed`、`override`、`abstract`、`extern`の修飾子は含まれません。

`async` 修飾子を持つメソッドは、非同期関数であり、「 [async 関数](classes.md#async-functions)」で説明されている規則に従います。

メソッド宣言の*return_type*は、計算され、メソッドによって返される値の型を指定します。 メソッドが値を返さない場合、 *return_type*は `void` です。 宣言に `partial` 修飾子が含まれている場合は、戻り値の型が `void`である必要があります。

*Member_name*は、メソッドの名前を指定します。 メソッドが明示的なインターフェイスメンバーの実装 ([明示的なインターフェイスメンバー](interfaces.md#explicit-interface-member-implementations)の実装) でない限り、 *member_name*は単なる*識別子*です。 明示的なインターフェイスメンバーの実装では、 *member_name*は*interface_type*の後に "`.`" と*識別子*が続きます。

省略可能な*type_parameter_list*は、メソッド ([型パラメーター](classes.md#type-parameters)) の型パラメーターを指定します。 *Type_parameter_list*が指定されている場合、メソッドは***ジェネリックメソッド***です。 メソッドに `extern` 修飾子がある場合、 *type_parameter_list*を指定することはできません。

省略可能な*formal_parameter_list*は、メソッドのパラメーター ([メソッドパラメーター](classes.md#method-parameters)) を指定します。

省略可能な*type_parameter_constraints_clause*s では、個々の型パラメーター ([型パラメーター制約](classes.md#type-parameter-constraints)) に対して制約を指定し、 *type_parameter_list*が指定されている場合にのみ指定できます。また、メソッドには `override` 修飾子がありません。

メソッドの*formal_parameter_list*で参照される*return_type*と各型は、少なくともメソッド自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。

*Method_body*は、セミコロン、***ステートメント本体***、または式の***本体***です。 ステートメントの本体は、メソッドが呼び出されたときに実行するステートメントを指定する*ブロック*で構成されます。 式の本体は、`=>` の後に*式*とセミコロンが続く形式で構成され、メソッドが呼び出されたときに実行する1つの式を示します。 

`abstract` および `extern` メソッドの場合、 *method_body*は単にセミコロンで構成されます。 `partial` メソッドの場合、 *method_body*は、セミコロン、ブロック本体、または式の本体で構成されます。 その他のすべてのメソッドでは、 *method_body*はブロック本体または式本体のいずれかになります。

*Method_body*がセミコロンで構成されている場合、宣言に `async` 修飾子が含まれていない可能性があります。

メソッドの名前、型パラメーターリスト、およびメソッドの仮パラメーターリストは、メソッドのシグネチャ (シグネチャ[とオーバーロード](basic-concepts.md#signatures-and-overloading)) を定義します。 具体的には、メソッドのシグネチャは、その名前、型パラメーターの数、およびその仮パラメーターの数、修飾子、および型で構成されます。 このため、仮パラメーターの型で発生するメソッドの型パラメーターは、その名前ではなく、メソッドの型引数リスト内の序数位置によって識別されます。戻り値の型は、メソッドのシグネチャの一部ではなく、型パラメーターまたは仮パラメーターの名前でもありません。

メソッドの名前は、同じクラスで宣言されている他のすべての非メソッドの名前と異なる必要があります。 さらに、メソッドのシグネチャは、同じクラス内で宣言されている他のすべてのメソッドのシグネチャとは異なる必要があります。また、同じクラスで宣言された2つのメソッドは、`ref` と `out`だけが異なるシグネチャを持つことはできません。

メソッドの*type_parameter*は*method_declaration*全体を通じてスコープ内にあり、 *return_type*、 *method_body*、 *type_parameter_constraints_clause*s で、*属性*ではなく、そのスコープ全体の型を形成するために使用できます。

すべての仮パラメーターと型パラメーターには、異なる名前を付ける必要があります。

### <a name="method-parameters"></a>メソッドパラメーター

メソッドのパラメーター (存在する場合) は、メソッドの*formal_parameter_list*によって宣言されます。

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

仮パラメーターリストは1つ以上のコンマ区切りのパラメーターで構成され、最後のパラメーターだけが*parameter_array*になる可能性があります。

*Fixed_parameter*は、省略可能な*属性*のセット ([属性](attributes.md))、省略可能な `ref`、`out` または `this` 修飾子、*型*、*識別子*、および省略可能な*default_argument*で構成されます。 各*fixed_parameter*は、指定された名前を使用して、指定された型のパラメーターを宣言します。 `this` 修飾子は、メソッドを拡張メソッドとして指定し、静的メソッドの最初のパラメーターでのみ使用できます。 拡張メソッドの詳細については、「[拡張メソッド](classes.md#extension-methods)」を参照してください。

*Default_argument*を持つ*fixed_parameter*は***省略可能なパラメーター***として知られていますが、 *default_argument*のない*fixed_parameter*は***必須パラメーター***です。 必須パラメーターは、 *formal_parameter_list*内の省略可能なパラメーターの後に指定することはできません。

`ref` または `out` パラメーターに*default_argument*を指定することはできません。 *Default_argument*内の*式*は、次のいずれかである必要があります。

*  *constant_expression*
*  `S` が値型である `new S()` フォームの式
*  `S` が値型である `default(S)` フォームの式

*式*は、id または null 許容型からパラメーターの型への変換によって、暗黙的に変換可能である必要があります。

部分メソッド宣言の実装 ([部分メソッド](classes.md#partial-methods))、明示的なインターフェイスメンバーの実装 ([明示的なインターフェイスメンバーの実装](interfaces.md#explicit-interface-member-implementations))、または単一パラメーターインデクサー宣言 ([インデクサー](classes.md#indexers)) で、省略可能なパラメーターが発生した場合、コンパイラは警告を出する必要があります。これらのメンバーは、引数を省略できるようには呼び出されないためです。

*Parameter_array*は、省略可能な*属性*のセット ([属性](attributes.md))、`params` 修飾子、 *array_type*、および*識別子*で構成されます。 パラメーター配列は、指定された名前を持つ指定された配列型の1つのパラメーターを宣言します。 パラメーター配列の*array_type*は、1次元配列型 ([配列型](arrays.md#array-types)) である必要があります。 メソッド呼び出しでは、パラメーター配列は、指定された配列型の1つの引数を指定するか、配列要素型の0個以上の引数を指定できるようにします。 パラメーター配列の詳細については、「[パラメーター配列](classes.md#parameter-arrays)」を参照してください。

*Parameter_array*は省略可能なパラメーターの後に発生することがありますが、既定値を持つことはできません。 *parameter_array*の引数を省略すると、代わりに空の配列が作成されます。

次の例は、さまざまな種類のパラメーターを示しています。
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

`M`の*formal_parameter_list*では、`i` は必須の ref パラメーターです。 `d` は必須の値パラメーター、`b`、`s`、`o` であり、`t` はパラメーター配列です。`a`

メソッド宣言は、パラメーター、型パラメーター、およびローカル変数に対して個別の宣言領域を作成します。 名前は、型パラメーターリストとメソッドの仮パラメーターリスト、およびメソッドの*ブロック*内のローカル変数宣言によって、この宣言領域に導入されます。 メソッド宣言空間の2つのメンバーが同じ名前を持つ場合、エラーになります。 メソッド宣言領域と入れ子になった宣言空間のローカル変数宣言領域が同じ名前の要素を含む場合、エラーになります。

メソッドの呼び出し ([メソッド呼び出し](expressions.md#method-invocations)) によって、メソッドの仮パラメーターとローカル変数の、その呼び出しに固有のコピーが作成され、呼び出しの引数リストによって、新しく作成された仮パラメーターに値または変数参照が割り当てられます。 メソッドの*ブロック*内では、 *Simple_name*式 ([簡易名](expressions.md#simple-names)) の識別子によって仮パラメーターを参照できます。

仮引数には4種類あります。

*  値パラメーター。修飾子なしで宣言されます。
*  参照パラメーター。 `ref` 修飾子を使用して宣言されます。
*  出力パラメーター。 `out` 修飾子を使用して宣言されます。
*  パラメーター配列。 `params` 修飾子を使用して宣言されます。

「[シグネチャとオーバーロード](basic-concepts.md#signatures-and-overloading)」で説明されているように、`ref` 修飾子と `out` 修飾子はメソッドのシグネチャの一部ですが、`params` 修飾子は含まれません。

#### <a name="value-parameters"></a>値パラメーター

修飾子を指定せずに宣言されたパラメーターは、値パラメーターです。 値パラメーターは、メソッドの呼び出しで指定された対応する引数から初期値を取得するローカル変数に対応します。

仮パラメーターが値パラメーターの場合、メソッド呼び出しの対応する引数は、暗黙的に変換可能な (暗黙の型[変換](conversions.md#implicit-conversions)) 式である必要があります。

メソッドは、値パラメーターに新しい値を割り当てることができます。 このような割り当ては、値パラメーターによって表されるローカルストレージの場所にのみ影響します。メソッドの呼び出しで指定された実際の引数には影響しません。

#### <a name="reference-parameters"></a>参照パラメーター

`ref` 修飾子で宣言されたパラメーターは参照パラメーターです。 値パラメーターとは異なり、参照パラメーターは新しいストレージの場所を作成しません。 代わりに、参照パラメーターは、メソッド呼び出しで引数として指定された変数と同じ格納場所を表します。

仮パラメーターが参照パラメーターである場合、メソッド呼び出しの対応する引数は、キーワード `ref` の後に、仮パラメーターと同じ型の*variable_reference* ([明確な代入を決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) が含まれている必要があります。 参照パラメーターとして渡すには、変数を確実に代入する必要があります。

メソッド内では、参照パラメーターは常に確実に割り当てられていると見なされます。

反復子 ([反復子](classes.md#iterators)) として宣言されたメソッドに参照パラメーターを含めることはできません。

例
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
この例では、次のように出力されます。
```console
i = 2, j = 1
```

`Main`で `Swap` を呼び出す場合、`x` は `i` を表し、`y` は `j`を表します。 このため、呼び出しは `i` と `j`の値をスワップする効果を持ちます。

参照パラメーターを受け取るメソッドでは、複数の名前が同じストレージの場所を表すことができます。 この例では、
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
`G` で `F` を呼び出すと、`a` と `b`の両方に対して `s` への参照が渡されます。 そのため、この呼び出しでは、`s`、`a`、および `b` の名前はすべて同じストレージの場所を参照し、3つの割り当てはすべて、インスタンスフィールド `s`を変更します。

#### <a name="output-parameters"></a>出力パラメーター

`out` 修飾子で宣言されたパラメーターは出力パラメーターです。 参照パラメーターと同様に、出力パラメーターは新しいストレージの場所を作成しません。 代わりに、出力パラメーターは、メソッド呼び出しで引数として指定された変数と同じ格納場所を表します。

仮パラメーターが出力パラメーターの場合、メソッド呼び出しの対応する引数は、キーワード `out` の後に、仮パラメーターと同じ型の*variable_reference* ([明確な代入を決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) を指定する必要があります。 変数は出力パラメーターとして渡す前に明示的に割り当てられている必要はありませんが、変数が出力パラメーターとして渡された場合の呼び出しに従うと、変数は確実に代入されたと見なされます。

ローカル変数と同じように、メソッド内では、出力パラメーターはまず未割り当てと見なされ、その値を使用する前に確実に代入する必要があります。

メソッドが返される前に、メソッドのすべての出力パラメーターを確実に代入する必要があります。

部分メソッド ([部分メソッド](classes.md#partial-methods)) または反復子 ([反復](classes.md#iterators)子) として宣言されたメソッドは、出力パラメーターを持つことができません。

出力パラメーターは、通常、複数の戻り値を生成するメソッドで使用されます。 例 :
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

この例では、次の出力が生成されます。
```console
c:\Windows\System\
hello.txt
```

`dir` 変数と `name` 変数は、`SplitPath`に渡す前に割り当てを解除することができ、呼び出し後に確実に割り当てられることに注意してください。

#### <a name="parameter-arrays"></a>パラメーター配列

`params` 修飾子で宣言されたパラメーターはパラメーター配列です。 仮パラメーターリストにパラメーター配列が含まれている場合は、リストの最後のパラメーターである必要があり、それは1次元配列型である必要があります。 たとえば、型 `string[]` と `string[][]` はパラメーター配列の型として使用できますが、`string[,]` 型は使用できません。 `params` 修飾子と修飾子 `ref` および `out`を組み合わせることはできません。

パラメーター配列では、メソッド呼び出しの2つの方法のいずれかで引数を指定できます。

*  パラメーター配列に指定する引数には、暗黙的に変換可能な (暗黙の[変換](conversions.md#implicit-conversions)) 1 つの式をパラメーター配列型に指定できます。 この場合、パラメーター配列は、値パラメーターとまったく同様に動作します。
*  また、呼び出しでは、パラメーター配列に0個以上の引数を指定できます。各引数は、暗黙的に変換可能な式 ([暗黙的な変換](conversions.md#implicit-conversions)) で、パラメーター配列の要素型になります。 この場合、呼び出しは、引数の数に対応する長さを持つパラメーター配列型のインスタンスを作成し、指定された引数値を使用して配列インスタンスの要素を初期化し、新しく作成された配列インスタンスを実際のとして使用します。引数.

呼び出しで可変個の引数を許可する場合を除き、パラメーター配列は、同じ型の値パラメーター ([値](classes.md#value-parameters)パラメーター) と厳密に等価です。

例
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
この例では、次のように出力されます。
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

`F` の最初の呼び出しでは、単に配列 `a` を値パラメーターとして渡します。 `F` の2回目の呼び出しでは、指定された要素の値を持つ4つの要素 `int[]` が自動的に作成され、その配列インスタンスが値パラメーターとして渡されます。 同様に、`F` の3回目の呼び出しでは、ゼロ要素 `int[]` を作成し、そのインスタンスを値パラメーターとして渡します。 2番目と3番目の呼び出しは、書き込みとまったく同じです。
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

オーバーロードの解決を実行する場合、パラメーター配列を持つメソッドは、通常の形式または拡張された形式 ([適用可能な関数メンバー](expressions.md#applicable-function-member)) のいずれかに適用できます。 メソッドの拡張形式は、メソッドの通常の形式が適用されない場合にのみ使用できます。また、展開されたフォームと同じシグネチャを持つ適用可能なメソッドが、同じ型で宣言されていない場合にのみ使用できます。

例
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
この例では、次のように出力されます。
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

この例では、パラメーター配列を使用するメソッドの拡張された2つの形式は、通常のメソッドとしてクラスに既に含まれています。 これらの拡張されたフォームは、オーバーロードの解決を実行するときには考慮されません。そのため、最初と3番目のメソッドの呼び出しでは、通常のメソッドを選択します。 クラスがパラメーター配列を使用してメソッドを宣言する場合、一部の拡張されたフォームも通常のメソッドとして含めることは珍しくありません。 これにより、パラメーター配列を持つメソッドの拡張された形式が呼び出されたときに発生する配列インスタンスの割り当てを回避できます。

パラメーター配列の型が `object[]`場合、通常の形式のメソッドと1つの `object` パラメーターの使用可能な形式との間であいまいさが発生する可能性があります。 あいまいさの理由は、`object[]` 自体が型 `object`に暗黙的に変換できることです。 ただし、あいまいさは、必要に応じてキャストを挿入することによって解決できるため、問題ありません。

例
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
この例では、次のように出力されます。
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

`F`の最初の呼び出しと最後の呼び出しでは、引数の型からパラメーターの型への暗黙的な変換が存在するため (両方とも `object[]`型)、`F` の通常の形式が適用されます。 したがって、オーバーロードの解決では、通常の形式の `F`が選択され、引数は通常の値パラメーターとして渡されます。 2番目と3番目の呼び出しでは、引数の型からパラメーターの型への暗黙的な変換が存在しないため、通常の形式の `F` は適用できません (型 `object` を型 `object[]`に暗黙的に変換することはできません)。 ただし、拡張された形式の `F` は適用可能なので、オーバーロードの解決によって選択されます。 その結果、1要素の `object[]` が呼び出しによって作成され、配列の1つの要素が、指定された引数の値 (それ自体が `object[]`への参照) で初期化されます。

### <a name="static-and-instance-methods"></a>静的メソッドとインスタンス メソッド

メソッドの宣言に `static` 修飾子が含まれている場合、そのメソッドは静的メソッドと呼ばれます。 `static` 修飾子が存在しない場合、メソッドはインスタンスメソッドと呼ばれます。

静的メソッドは、特定のインスタンスでは動作せず、静的メソッドで `this` を参照するコンパイル時エラーです。

インスタンスメソッドは、クラスの特定のインスタンスに対して動作し、そのインスタンスには `this` ([このアクセス](expressions.md#this-access)) としてアクセスできます。

メソッドが `E.M`フォームの*member_access* ([メンバーアクセス](expressions.md#member-access)) で参照されている場合、`M` が静的メソッドである場合、`E` は `M`を含む型を表している必要があり、`M` がインスタンスメソッドである場合、`E` は `M`を含む型のインスタンスを示す必要があります。

静的メンバーとインスタンスメンバーの違いについては、「[静的メンバーとインスタンス](classes.md#static-and-instance-members)メンバー」で詳しく説明します。

### <a name="virtual-methods"></a>仮想メソッド

インスタンスメソッドの宣言に `virtual` 修飾子が含まれている場合、そのメソッドは仮想メソッドと呼ばれます。 `virtual` 修飾子が存在しない場合、メソッドは非仮想メソッドと呼ばれます。

非仮想メソッドの実装は不変です。実装は、メソッドが宣言されているクラスのインスタンスまたは派生クラスのインスタンスで呼び出されているかどうかによって異なります。 これに対し、仮想メソッドの実装は、派生クラスによって置き換えることができます。 継承された仮想メソッドの実装を置き換えるプロセスは、そのメソッドの***オーバーライド***([メソッドのオーバーライド](classes.md#override-methods)) と呼ばれます。

仮想メソッドの呼び出しでは、呼び出し元のインスタンスの***実行時の型***によって、呼び出す実際のメソッドの実装が決まります。 非仮想メソッドの呼び出しでは、インスタンスの***コンパイル時の型***が決定要因になります。 正確に言えば、`N` という名前のメソッドが、コンパイル時の型 `C` で、実行時の型 `R` (`R` が `C` または `C`から派生したクラス) であるインスタンスに対して `A` 引数リストを使用して呼び出されると、呼び出しは次のように処理されます。

*  1つ目は、オーバーロードの解決を `C`、`N`、および `A`に適用して、で宣言され、`C`によって継承されるメソッドのセットから特定のメソッド `M` を選択することです。 これについては、「[メソッドの呼び出し](expressions.md#method-invocations)」を参照してください。
*  次に、`M` が非仮想メソッドの場合、`M` が呼び出されます。
*  それ以外の場合、`M` は仮想メソッドであり、`R` に関して最も派生する `M` の実装が呼び出されます。

クラスによって宣言または継承されているすべての仮想メソッドについて、そのクラスに対してメソッドの***最も派生***した実装が存在します。 クラス `R` に対して `M` 仮想メソッドの最も派生する実装は、次のように決定されます。

*  `R` に `M`の導入 `virtual` 宣言が含まれている場合、これは `M`の最も派生された実装です。
*  それ以外の場合、`R` に `M`の `override` が含まれている場合、これは `M`の最も派生された実装です。
*  それ以外の場合、`R` に関して最も派生した `M` の実装は、`R`の直接基底クラスに対する `M` の最も派生した実装と同じです。

次の例では、仮想メソッドと非仮想メソッドの違いについて説明します。
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

この例では、`A` に、仮想メソッド以外の `F` と仮想メソッド `G`が導入されています。 クラス `B` は、新しい非仮想メソッド `F`を導入し、継承された `F`を非表示にします。また、継承されたメソッド `G`もオーバーライドします。 この例では、次の出力が生成されます。
```console
A.F
B.F
B.G
B.G
```

ステートメント `a.G()` が `A.G`ではなく `B.G`を呼び出すことに注意してください。 これは、インスタンス (`A`) のコンパイル時の型ではなく、インスタンスの実行時の型 (`B`) が、呼び出す実際のメソッドの実装を決定するためです。

メソッドは継承されたメソッドを隠すことができるため、クラスに同じシグネチャを持つ複数の仮想メソッドを含めることができます。 これにより、すべての派生メソッドが非表示になるため、あいまいさの問題はありません。 この例では、
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
`C` クラスと `D` クラスには、`A` によって導入されたものと `C`によって導入されたものの2つの仮想メソッドが含まれています。 `C` によって導入されたメソッドは、`A`から継承されたメソッドを非表示にします。 したがって、`D` のオーバーライド宣言は、`C`によって導入されたメソッドよりも優先されるため、`A`によって導入されたメソッドを `D` でオーバーライドすることはできません。 この例では、次の出力が生成されます。
```console
B.F
B.F
D.F
D.F
```

メソッドが非表示にならない派生型を使用して `D` のインスタンスにアクセスすることによって、非表示の仮想メソッドを呼び出すことができることに注意してください。

### <a name="override-methods"></a>オーバーライドメソッド

インスタンスメソッドの宣言に `override` 修飾子が含まれている場合、メソッドは***オーバーライドメソッド***と呼ばれます。 オーバーライドメソッドは、同じシグネチャを持つ継承された仮想メソッドをオーバーライドします。 仮想メソッドの宣言には新しいメソッドが導入されていますが、オーバーライド メソッドの宣言では、そのメソッドの新しい実装を提供することで既存の継承された仮想メソッドを特殊化します。

`override` 宣言によってオーバーライドされるメソッドは、オーバーライドされた***基本メソッド***と呼ばれます。 オーバーライドメソッド `M` クラス `C`で宣言されている場合、オーバーライドされた基本メソッドは、1つ以上の基底クラス型の `C``C` を検査することによって決定されます。これは、指定された基底クラス型で、型引数の代入後に `M` と同じシグネチャを持つアクセス可能なメソッドが少なくとも1つ存在します オーバーライドされた基本メソッドを検索するために、メソッドは、`public`されている場合はアクセス可能と見なされます (`protected`されている場合は、`protected internal`されていない場合、`C`と同じプログラム内で `internal` および宣言されている場合)。

次のすべてがオーバーライド宣言に当てはまる場合を除き、コンパイル時エラーが発生します。

*  オーバーライドされた基本メソッドは、前述のように配置できます。
*  このようなオーバーライドされた基本メソッドが1つだけあります。 この制限は、基底クラスの型が構築された型である場合にのみ有効です。型引数を代入すると、2つのメソッドのシグネチャが同じになります。
*  オーバーライドされた基本メソッドは、virtual、abstract、または override メソッドです。 言い換えると、オーバーライドされた基本メソッドを静的または非仮想にすることはできません。
*  オーバーライドされた基本メソッドは、シールメソッドではありません。
*  オーバーライドメソッドとオーバーライドされた基本メソッドの戻り値の型は同じです。
*  オーバーライド宣言とオーバーライドされた基本メソッドに、同じアクセシビリティが宣言されています。 つまり、オーバーライド宣言では、仮想メソッドのアクセシビリティを変更することはできません。 ただし、オーバーライドされた基本メソッドが内部で保護されていて、オーバーライドメソッドを含むアセンブリとは別のアセンブリで宣言されている場合は、オーバーライドメソッドで宣言されたアクセシビリティを保護する必要があります。
*  オーバーライド宣言で、型パラメーターの制約句が指定されていません。 代わりに、制約はオーバーライドされた基本メソッドから継承されます。 オーバーライドされたメソッドの型パラメーターである制約は、継承された制約の型引数によって置き換えられる可能性があることに注意してください。 これは、値型やシール型など、明示的に指定された場合には無効な制約につながる可能性があります。

次の例は、ジェネリッククラスでのオーバーライド規則のしくみを示しています。
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

オーバーライド宣言は、 *base_access* ([ベースアクセス](expressions.md#base-access)) を使用して、オーバーライドされた基本メソッドにアクセスできます。 この例では、
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
`B` での `base.PrintFields()` の呼び出しでは、`A`で宣言された `PrintFields` メソッドが呼び出されます。 *Base_access*は、仮想呼び出し機構を無効にし、単に基本メソッドを非仮想メソッドとして扱います。 `B` の呼び出しが `((A)this).PrintFields()`書き込まれている場合、`A`が仮想であり、`PrintFields` の実行時の型が `((A)this)` であるため、`B`で宣言されたメソッドではなく `B`で宣言されている `PrintFields` メソッドを再帰的に呼び出します。

`override` 修飾子を含めることによってのみ、メソッドが別のメソッドをオーバーライドできます。 それ以外の場合、継承されたメソッドと同じシグネチャを持つメソッドは、継承されたメソッドを非表示にします。 この例では、
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
`B` の `F` メソッドには、`override` 修飾子が含まれていないため、`A`の `F` メソッドはオーバーライドされません。 代わりに、`B` の `F` メソッドによって `A`のメソッドが非表示になり、宣言に `new` 修飾子が含まれていないため、警告が報告されます。

この例では、
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
`B` の `F` メソッドは、`A`から継承された仮想 `F` メソッドを非表示にします。 `B` の新しい `F` にはプライベートアクセスがあるため、そのスコープには `B` のクラス本体のみが含まれ、`C`には拡張されません。 したがって、`C` 内の `F` の宣言は、`A`から継承された `F` をオーバーライドできます。

### <a name="sealed-methods"></a>シールメソッド

インスタンスメソッドの宣言に `sealed` 修飾子が含まれている場合、そのメソッドは***sealed メソッド***と呼ばれます。 インスタンスメソッドの宣言に `sealed` 修飾子が含まれている場合は、`override` 修飾子も含める必要があります。 `sealed` 修飾子を使用すると、派生クラスでメソッドをさらにオーバーライドできなくなります。

この例では、
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
クラス `B` には、2つのオーバーライドメソッドが用意されています。 `sealed` 修飾子を持つ `F` メソッドと、それ以外の `G` メソッドです。 シールされた `modifier` を使用 `B`と、`F`をさらにオーバーライドできなく `C` なります。

### <a name="abstract-methods"></a>抽象メソッド

インスタンスメソッドの宣言に `abstract` 修飾子が含まれている場合、そのメソッドは***抽象メソッド***と呼ばれます。 抽象メソッドは暗黙的に仮想メソッドでもありますが、修飾子 `virtual`を持つことはできません。

抽象メソッドの宣言では、新しい仮想メソッドが導入されていますが、そのメソッドの実装は提供していません。 代わりに、抽象でない派生クラスは、そのメソッドをオーバーライドすることによって独自の実装を提供する必要があります。 抽象メソッドは実際の実装を提供しないため、抽象メソッドの*method_body*は単純にセミコロンで構成されます。

抽象メソッドの宣言は、抽象クラス ([抽象クラス](classes.md#abstract-classes)) でのみ許可されます。

この例では、
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
`Shape` クラスは、それ自体を描画できる幾何学図形オブジェクトの抽象概念を定義します。 `Paint` メソッドは abstract です。これは、意味のある既定の実装が存在しないためです。 `Ellipse` クラスと `Box` クラスは、具体的な `Shape` 実装です。 これらのクラスは非抽象であるため、`Paint` メソッドをオーバーライドし、実際の実装を提供する必要があります。

*Base_access* ([ベースアクセス](expressions.md#base-access)) が抽象メソッドを参照する場合、コンパイル時エラーになります。 この例では、
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
`base.F()` の呼び出しでは、抽象メソッドを参照しているため、コンパイル時エラーが報告されます。

抽象メソッドの宣言では、仮想メソッドをオーバーライドできます。 これにより、抽象クラスは派生クラスのメソッドを強制的に再実装できるようになり、メソッドの元の実装を使用できなくなります。 この例では、
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
クラス `A` は仮想メソッドを宣言し、クラス `B` は抽象メソッドを使用してこのメソッドをオーバーライドし、クラス `C` は抽象メソッドをオーバーライドして独自の実装を提供します。

### <a name="external-methods"></a>外部メソッド

メソッドの宣言に `extern` 修飾子が含まれている場合、そのメソッドは***外部メソッド***と呼ばれます。 外部メソッドは、通常以外C#の言語を使用して、外部で実装されます。 外部メソッドの宣言は実際の実装を提供しないため、外部メソッドの*method_body*は単純にセミコロンで構成されます。 外部メソッドはジェネリックではない可能性があります。

`extern` 修飾子は通常、`DllImport` 属性 ([COM コンポーネントおよび Win32 コンポーネントとの相互運用](attributes.md#interoperation-with-com-and-win32-components)) と共に使用され、外部メソッドを Dll (ダイナミックリンクライブラリ) によって実装できます。 実行環境では、外部メソッドの実装を提供できる他のメカニズムがサポートされている場合があります。

外部メソッドに `DllImport` 属性が含まれている場合、メソッドの宣言にも `static` 修飾子を含める必要があります。 この例では、`extern` 修飾子と `DllImport` 属性の使用方法を示します。
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a>部分メソッド (要約)

メソッドの宣言に `partial` 修飾子が含まれている場合、そのメソッドは***部分メソッド***と呼ばれます。 部分メソッドは、部分型 ([部分型](classes.md#partial-types)) のメンバーとしてのみ宣言でき、いくつかの制限が適用されます。 部分メソッドの詳細については、[部分メソッド](classes.md#partial-methods)を参照してください。

### <a name="extension-methods"></a>拡張メソッド

メソッドの最初のパラメーターに `this` 修飾子が含まれている場合、そのメソッドは***拡張メソッド***と呼ばれます。 拡張メソッドは、非ジェネリックで入れ子にされていない静的クラスでのみ宣言できます。 拡張メソッドの最初のパラメーターには、`this`以外の修飾子を指定できません。また、パラメーターの型をポインター型にすることはできません。

2つの拡張メソッドを宣言する静的クラスの例を次に示します。
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

拡張メソッドは、通常の静的メソッドです。 また、外側の静的クラスがスコープ内にある場合、最初の引数としてレシーバー式を使用して、インスタンスメソッドの呼び出し構文 ([拡張メソッドの呼び出し](expressions.md#extension-method-invocations)) を使用して拡張メソッドを呼び出すことができます。

次のプログラムでは、上記で宣言した拡張メソッドを使用します。
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

`Slice` メソッドは `string[]`で使用でき、`ToInt32` メソッドは、拡張メソッドとして宣言されているため、`string`で使用できます。 プログラムの意味は、通常の静的メソッド呼び出しを使用した次のようになります。
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a>メソッド本体

メソッド宣言の*method_body*は、ブロック本体、式本体、またはセミコロンで構成されます。

戻り値の型が `void`の場合、またはメソッドが非同期で戻り値の型が `System.Threading.Tasks.Task`場合は、メソッドの***結果型***が `void` ます。 それ以外の場合、非同期メソッド以外のメソッドの結果型は戻り値の型になり、戻り値の型が `System.Threading.Tasks.Task<T>` の非同期メソッドの結果の型は `T`になります。

メソッドに `void` の結果型とブロック本体がある場合、ブロック内の `return` ステートメント ([return ステートメント](statements.md#the-return-statement)) で式を指定することはできません。 Void メソッドのブロックの実行が正常に完了した場合 (つまり、制御フローがメソッド本体の末尾から離れた場合)、そのメソッドは単に現在の呼び出し元に戻ります。
    
メソッドに `void` の結果と式の本体が含まれている場合、`E` 式は*statement_expression*である必要があり、本文は `{ E; }`フォームのブロック本体とまったく同じになります。
    
メソッドに void 以外の結果型とブロック本体がある場合、ブロック内の各 `return` ステートメントで、結果型に暗黙的に変換できる式を指定する必要があります。 値を返すメソッドのブロック本体のエンドポイントに到達できません。 つまり、ブロック本体を持つ値を返すメソッドでは、コントロールはメソッド本体の末尾から制御できません。
    
メソッドが void 以外の結果型と式本体を持つ場合、式は結果型に暗黙的に変換できる必要があり、本文は `{ return E; }`フォームのブロック本体とまったく同じになります。
    
この例では、
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
コントロールはメソッド本体の末尾から制御できるため、値を返す `F` メソッドはコンパイル時エラーになります。 使用可能なすべての実行パスが戻り値を指定する return ステートメントで終了するため、`G` および `H` メソッドは正しいです。 `I` メソッドが正しいのは、その本体が1つの return ステートメントだけを含むステートメントブロックと同じであるためです。

### <a name="method-overloading"></a>メソッドのオーバーロード

メソッドのオーバーロードの解決規則については、 [「型の推定](expressions.md#type-inference)」を参照してください。

## <a name="properties"></a>プロパティ

***プロパティ***は、オブジェクトまたはクラスの特性へのアクセスを提供するメンバーです。 プロパティの例としては、文字列の長さ、フォントのサイズ、ウィンドウのキャプション、顧客名などがあります。 プロパティはフィールドの自然な拡張機能であり、どちらも、関連付けられた型を持つ名前付きのメンバーであり、フィールドとプロパティにアクセスするための構文は同じです。 ただし、フィールドとは異なり、プロパティは格納場所を表しません。 その代わりに、プロパティには、値の読み取りまたは書き込みの際に実行されるステートメントを指定する "***アクセサー***" があります。 そのため、プロパティは、アクションをオブジェクトの属性の読み取りと書き込みに関連付けるメカニズムを提供します。さらに、このような属性を計算することもできます。

プロパティは*property_declaration*s を使用して宣言されます。

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

*Property_declaration*には、*属性*([属性](attributes.md)) のセットと、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせ、`new` ([新しい修飾子](classes.md#the-new-modifier))、`static` ([静的メソッドとインスタンスメソッド](classes.md#static-and-instance-methods))、`virtual` ([仮想メソッド](classes.md#virtual-methods))、`override` ([オーバーライドメソッド](classes.md#override-methods))、([シールメソッド](classes.md#sealed-methods))、`sealed` ([抽象メソッド](classes.md#abstract-methods))、および `abstract` ([外部メソッド](classes.md#external-methods)) 修飾子を含めることができます。`extern`

プロパティ宣言は、修飾子の有効な組み合わせに関して、メソッド宣言 ([メソッド](classes.md#methods)) と同じ規則に従います。

プロパティ宣言の*型*は、宣言によって導入されるプロパティの型を指定し、 *member_name*はプロパティの名前を指定します。 プロパティが明示的なインターフェイスメンバーの実装でない限り、 *member_name*は単なる*識別子*です。 明示的なインターフェイスメンバーの実装 ([明示的なインターフェイスメンバー](interfaces.md#explicit-interface-member-implementations)の実装) の場合、 *member_name*は*interface_type*の後に "`.`" と*識別子*が続く形式で構成されます。

プロパティの*型*は、少なくともプロパティ自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。

*Property_body*は、***アクセサー本体***または***式の本体***で構成されている場合があります。 アクセサー本体で、 *accessor_declarations*を "`{`" および "`}`" トークンで囲む必要がある場合は、プロパティのアクセサー ([アクセサー](classes.md#accessors)) を宣言します。 アクセサーは、プロパティの読み取りと書き込みに関連付けられた実行可能なステートメントを指定します。

`=>` で構成され、その後に*式*`E` が続く式本体と、セミコロンはステートメント本体 `{ get { return E; } }`と完全に等価であり、そのため、getter の結果が1つの式によって指定される getter のみのプロパティを指定する場合にのみ使用できます。

*Property_initializer*は、自動的に実装されるプロパティ ([自動的に実装](classes.md#automatically-implemented-properties)されるプロパティ) に対してのみ指定でき、そのようなプロパティの基になるフィールドは、*式*によって指定された値で初期化されます。

プロパティにアクセスするための構文は、フィールドの場合と同じですが、プロパティは変数として分類されません。 したがって、プロパティを `ref` または `out` 引数として渡すことはできません。

プロパティの宣言に `extern` 修飾子が含まれている場合、プロパティは***外部プロパティ***と呼ばれます。 外部プロパティの宣言は実際の実装を提供しないため、各*accessor_declarations*はセミコロンで構成されます。

### <a name="static-and-instance-properties"></a>静的プロパティとインスタンスプロパティ

プロパティの宣言に `static` 修飾子が含まれている場合、プロパティは***静的プロパティ***と呼ばれます。 `static` 修飾子が存在しない場合、プロパティは***インスタンスプロパティ***と呼ばれます。

静的プロパティは特定のインスタンスに関連付けられていません。静的なプロパティのアクセサーで `this` を参照するには、コンパイル時エラーになります。

インスタンスプロパティは、クラスの特定のインスタンスに関連付けられます。このインスタンスには、そのプロパティのアクセサーで `this` ([このアクセス](expressions.md#this-access)) としてアクセスできます。

プロパティが `E.M`フォームの*member_access* ([メンバーアクセス](expressions.md#member-access)) で参照されている場合、`M` が静的プロパティである場合、`E` は `M`を含む型を示す必要があります。また、`M` がインスタンスプロパティの場合、E は `M`を含む型のインスタンスを示す必要があります。

静的メンバーとインスタンスメンバーの違いについては、「[静的メンバーとインスタンス](classes.md#static-and-instance-members)メンバー」で詳しく説明します。

### <a name="accessors"></a>アクセス

プロパティの*accessor_declarations*は、そのプロパティの読み取りと書き込みに関連付けられた実行可能なステートメントを指定します。

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

アクセサー宣言は、 *get_accessor_declaration*、 *set_accessor_declaration*、またはその両方で構成されます。 各アクセサー宣言は、トークン `get` または `set` の後に省略可能な*accessor_modifier*と*accessor_body*で構成されます。

*Accessor_modifier*s の使用には、次の制限が適用されます。

*  *Accessor_modifier*は、インターフェイスまたは明示的なインターフェイスメンバーの実装では使用できません。
*  `override` 修飾子を持たないプロパティまたはインデクサーの場合、 *accessor_modifier*は、プロパティまたはインデクサーに `get` と `set` の両方のアクセサーが含まれている場合にのみ許可されます。その後、これらのアクセサーのいずれかでのみ許可されます。
*  `override` 修飾子を含むプロパティまたはインデクサーの場合、アクセサーはオーバーライドされるアクセサーの*accessor_modifier*と一致する必要があります。
*  *Accessor_modifier*は、プロパティまたはインデクサー自体の宣言されたアクセシビリティより厳密に制限されたアクセシビリティを宣言する必要があります。 正確である必要があります。
   * プロパティまたはインデクサーに `public`のアクセシビリティが宣言されている場合、 *accessor_modifier*は `protected internal`、`internal`、`protected`、`private`のいずれかになります。
   * プロパティまたはインデクサーに `protected internal`のアクセシビリティが宣言されている場合、 *accessor_modifier*は `internal`、`protected`、または `private`のいずれかになります。
   * プロパティまたはインデクサーに `internal` または `protected`の宣言されたアクセシビリティがある場合、 *accessor_modifier*を `private`する必要があります。
   * プロパティまたはインデクサーに `private`のアクセシビリティが宣言されている場合、 *accessor_modifier*を使用することはできません。

`abstract` および `extern` プロパティの場合、指定された各アクセサーの*accessor_body*は、単純にセミコロンになります。 非抽象型の非 extern プロパティは、各*accessor_body*をセミコロンにすることができます。この場合、自動的***に実装***されるプロパティ ([自動的に実装](classes.md#automatically-implemented-properties)されるプロパティ) です。 自動的に実装されるプロパティには、少なくとも get アクセサーが必要です。 他の非抽象、非 extern プロパティのアクセサーの場合、 *accessor_body*は、対応するアクセサーが呼び出されたときに実行されるステートメントを指定する*ブロック*です。

`get` アクセサーは、プロパティの型の戻り値を持つパラメーターなしのメソッドに対応します。 代入の対象として、プロパティが式で参照されている場合は、プロパティの `get` アクセサーが呼び出され、プロパティ ([式の値](expressions.md#values-of-expressions)) の値が計算されます。 `get` アクセサーの本体は、「[メソッド本体](classes.md#method-body)」で説明されている値を返すメソッドの規則に準拠している必要があります。 特に、`get` アクセサーの本体にあるすべての `return` ステートメントでは、プロパティの型に暗黙的に変換できる式を指定する必要があります。 さらに、`get` アクセサーのエンドポイントに到達できないようにする必要があります。

`set` アクセサーは、プロパティ型の単一の値パラメーターと `void` の戻り値の型を持つメソッドに対応します。 `set` アクセサーの暗黙的なパラメーターには、常に `value`という名前が付けられます。 プロパティが代入式のターゲットとして参照されている場合 ([代入演算子](expressions.md#assignment-operators))、または、`++` または `--` のオペランド ([後置インクリメントおよびデクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、[前置インクリメントおよびデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)) として、`set` アクセサーは、新しい値 ([単純な代入](expressions.md#simple-assignment)) を提供する引数 (代入の右辺の値または `++` または `--` 演算子のオペランド) を使用して呼び出されます。 `set` アクセサーの本体は、「[メソッド本体](classes.md#method-body)」で説明されている `void` メソッドの規則に準拠している必要があります。 特に、`set` アクセサー本体の `return` ステートメントでは、式を指定することはできません。 `set` アクセサーには `value`という名前のパラメーターが暗黙的に含まれているため、`set` アクセサー内のローカル変数または定数宣言でその名前が使用される場合、コンパイル時エラーになります。

`get` と `set` のアクセサーの有無に基づいて、プロパティは次のように分類されます。

*  `get` アクセサーと `set` アクセサーの両方を含むプロパティは、***読み取り/書き込み***プロパティと呼ばれます。
*  `get` アクセサーだけを持つプロパティは、***読み取り***専用プロパティと呼ばれます。 読み取り専用プロパティが割り当てのターゲットである場合、コンパイル時エラーになります。
*  `set` のアクセサーだけを持つプロパティは、***書き込み専用***のプロパティと呼ばれます。 代入の対象となる場合を除き、式の書き込み専用プロパティを参照するコンパイル時エラーになります。

この例では、
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
`Button` コントロールは、パブリック `Caption` プロパティを宣言します。 `Caption` プロパティの `get` アクセサーは、private `caption` フィールドに格納されている文字列を返します。 `set` アクセサーは、新しい値が現在の値と異なるかどうかを確認します。存在する場合は、新しい値を格納し、コントロールを再描画します。 プロパティは、多くの場合、上記のパターンに従います。 `get` アクセサーは、プライベートフィールドに格納されている値を返すだけで、`set` アクセサーはそのプライベートフィールドを変更し、オブジェクトの状態を完全に更新するために必要な追加のアクションを実行します。

上記の `Button` クラスを使用した場合、`Caption` プロパティの使用例を次に示します。
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

ここでは、プロパティに値を割り当てることによって `set` アクセサーが呼び出され、式のプロパティを参照することによって `get` アクセサーが呼び出されます。

プロパティの `get` アクセサーと `set` アクセサーは、個別のメンバーではなく、プロパティのアクセサーを個別に宣言することはできません。 そのため、読み取り/書き込みプロパティの2つのアクセサーが異なるアクセシビリティを持つことはできません。 例
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
は、1つの読み取り/書き込みプロパティを宣言しません。 代わりに、同じ名前の2つのプロパティを宣言します。1つは読み取り専用で、もう1つは書き込み専用です。 同じクラスで宣言された2つのメンバーは同じ名前を持つことができないため、この例ではコンパイル時エラーが発生します。

派生クラスで、継承されたプロパティと同じ名前のプロパティが宣言されている場合、派生プロパティは、読み取りと書き込みの両方に関して継承されたプロパティを非表示にします。 この例では、
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
`B` の `P` プロパティは、読み取りと書き込みの両方に対して `A` の `P` プロパティを非表示にします。 そのため、ステートメントでは、
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
`b.P` への割り当てにより、コンパイル時エラーが報告されます。これは `B` の読み取り専用 `P` プロパティによって `A`の書き込み専用 `P` プロパティが非表示になるためです。 ただし、キャストを使用して非表示の `P` プロパティにアクセスできることに注意してください。

パブリックフィールドとは異なり、プロパティによって、オブジェクトの内部状態とそのパブリックインターフェイスが分離されます。 例を考えてみましょう。
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

ここでは、`Label` クラスは、`x` と `y`の2つの `int` フィールドを使用して、その場所を格納します。 この場所は、`X` と `Y` プロパティの両方と `Point`型の `Location` プロパティとして公開されています。 `Label`の将来のバージョンでは、場所を `Point` として内部に格納する方が便利な場合があります。この変更は、クラスのパブリックインターフェイスに影響を与えることなく行うことができます。
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

`public readonly` フィールドではなく `x` と `y` があったので、`Label` クラスに変更を加えることはできませんでした。

プロパティを通じて状態を公開することは、フィールドを直接公開するよりも効率が低いとは限りません。 特に、プロパティが非仮想で、少量のコードのみが含まれている場合、実行環境では、アクセサーの呼び出しをアクセサーの実際のコードに置き換えることができます。 このプロセスは***インライン展開***と呼ばれ、フィールドアクセスと同じようにプロパティへのアクセスが可能になり、プロパティの柔軟性が向上します。

`get` アクセサーを呼び出すことは、概念的にはフィールドの値を読み取ることと同じであるため、`get` アクセサーが監視可能な副作用を持つような不適切なプログラミングスタイルと見なされます。 この例では、
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
`Next` プロパティの値は、プロパティが以前にアクセスされた回数によって異なります。 したがって、プロパティにアクセスすると、監視可能な副作用が生成されます。プロパティは、代わりにメソッドとして実装する必要があります。

`get` アクセサーに対する "副作用なし" の規則は、`get` アクセサーが常にフィールドに格納されている値を返すように記述する必要があるという意味ではありません。 実際には、複数のフィールドにアクセスしたりメソッドを呼び出したりすることによって、多くの場合、`get` アクセサーがプロパティの値を計算します。 ただし、適切にデザインされた `get` アクセサーは、オブジェクトの状態の監視可能な変更の原因となるアクションを実行しません。

プロパティを使用して、リソースが最初に参照されるまで、リソースの初期化を遅延させることができます。 例 :
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

`Console` クラスには、標準入力、出力、およびエラーデバイスをそれぞれ表す、`In`、`Out`、および `Error`という3つのプロパティが含まれています。 これらのメンバーをプロパティとして公開することによって、実際に使用されるまで、`Console` クラスは初期化を遅延させることができます。 たとえば、最初に `Out` プロパティを参照したときに、次のようになります。
```csharp
Console.Out.WriteLine("hello, world");
```
出力デバイスの基になる `TextWriter` が作成されます。 ただし、アプリケーションが `In` プロパティと `Error` プロパティを参照していない場合、それらのデバイスのオブジェクトは作成されません。

### <a name="automatically-implemented-properties"></a>自動的に実装されたプロパティ

自動的に実装されるプロパティ (または short の***自動プロパティ***) は、セミコロン専用のアクセサー本体を持つ非抽象非 extern プロパティです。 自動プロパティには get アクセサーが必要であり、オプションで set アクセサーを持つことができます。

プロパティが自動的に実装されるプロパティとして指定されている場合は、非表示のバッキングフィールドがプロパティで自動的に使用できるようになります。アクセサーは、そのバッキングフィールドに対して読み取りおよび書き込みを行うために実装されます。 自動プロパティに set アクセサーがない場合、バッキングフィールドは `readonly` ([読み取り専用フィールド](classes.md#readonly-fields)) と見なされます。 `readonly` フィールドと同様に、getter のみの自動プロパティも、外側のクラスのコンストラクターの本体でに割り当てることができます。 この割り当ては、プロパティの readonly バッキングフィールドに直接割り当てられます。

自動プロパティには、必要に応じて、 *variable_initializer* ([変数初期化子](classes.md#variable-initializers)) としてバッキングフィールドに直接適用される*property_initializer*を含めることができます。

次のような例です。
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
は、次の宣言と同じです。
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

次のような例です。
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
は、次の宣言と同じです。
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

読み取り専用フィールドへの割り当ては、コンストラクター内で行われるため、有効です。


### <a name="accessibility"></a>ユーザー補助

アクセサーに*accessor_modifier*がある場合、アクセサーのアクセシビリティドメイン ([アクセシビリティ](basic-concepts.md#accessibility-domains)ドメイン) は、 *accessor_modifier*の宣言されたアクセシビリティを使用して決定されます。 アクセサーに*accessor_modifier*がない場合、アクセサーのアクセシビリティドメインは、プロパティまたはインデクサーの宣言されたアクセシビリティから決まります。

*Accessor_modifier*が存在しても、メンバー参照 ([演算子](expressions.md#operators)) やオーバーロードの解決 ([オーバーロードの解決](expressions.md#overload-resolution)) には影響しません。 プロパティまたはインデクサーの修飾子は、アクセスのコンテキストに関係なく、バインド先のプロパティまたはインデクサーを常に決定します。

特定のプロパティまたはインデクサーが選択されると、その使用法が有効かどうかを判断するために、関連する特定のアクセサーのアクセシビリティドメインが使用されます。

*  使用法が値 ([式の値](expressions.md#values-of-expressions)) である場合は、`get` アクセサーが存在し、アクセス可能である必要があります。
*  単純な代入 ([単純な代入](expressions.md#simple-assignment)) の対象として使用する場合は、`set` アクセサーが存在し、アクセス可能である必要があります。
*  使用法が複合代入 ([複合代入](expressions.md#compound-assignment)) のターゲットである場合、または `++` または `--` 演算子 ([関数メンバー](expressions.md#function-members).9、[呼び出し式](expressions.md#invocation-expressions)) のターゲットとして使用されている場合は、`get` アクセサーと `set` アクセサーの両方が存在し、アクセス可能である必要があります。

次の例では、プロパティ `A.Text` は、`set` アクセサーのみが呼び出されているコンテキストでも、プロパティ `B.Text`によって非表示になります。 これに対し、プロパティ `B.Count` はクラス `M`にアクセスできないため、代わりにアクセス可能なプロパティ `A.Count` が使用されます。

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

インターフェイスを実装するために使用されるアクセサーには、 *accessor_modifier*がない可能性があります。 インターフェイスを実装するために使用されるアクセサーが1つだけの場合、もう一方のアクセサーは*accessor_modifier*を使用して宣言できます。
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>仮想、シール、オーバーライド、および抽象プロパティアクセサー

プロパティの宣言 `virtual` は、プロパティのアクセサーが仮想であることを指定します。 `virtual` 修飾子は、読み取り/書き込みプロパティの両方のアクセサーに適用されます。読み取り/書き込みプロパティの1つのアクセサーだけを仮想にすることはできません。

`abstract` プロパティの宣言では、プロパティのアクセサーが仮想であることを指定しますが、アクセサーの実際の実装は提供しません。 代わりに、抽象でない派生クラスは、プロパティをオーバーライドすることによって、アクセサーの独自の実装を提供する必要があります。 抽象プロパティ宣言のアクセサーは実際の実装を提供しないため、 *accessor_body*は単にセミコロンで構成されます。

`abstract` 修飾子と `override` 修飾子の両方を含むプロパティ宣言では、プロパティが abstract であり、基本プロパティがオーバーライドされることを指定します。 このようなプロパティのアクセサーも抽象型です。

抽象プロパティ宣言は、抽象クラス ([抽象クラス](classes.md#abstract-classes)) でのみ許可されます。継承された仮想プロパティのアクセサーは、`override` ディレクティブを指定するプロパティ宣言を含めることによって、派生クラスでオーバーライドできます。 これは、オーバーライドする***プロパティの宣言***と呼ばれます。 オーバーライドするプロパティの宣言で、新しいプロパティが宣言されていません。 代わりに、既存の仮想プロパティのアクセサーの実装を単に特殊化します。

オーバーライドするプロパティの宣言では、継承されたプロパティとまったく同じアクセシビリティ修飾子、型、および名前を指定する必要があります。 継承されたプロパティにアクセサーが1つしかない場合 (つまり、継承されたプロパティが読み取り専用または書き込み専用の場合)、オーバーライドする側のプロパティにはそのアクセサーだけを含める必要があります。 継承されたプロパティに両方のアクセサーが含まれている場合 (つまり、継承されたプロパティが読み取り/書き込みの場合)、オーバーライドする側のプロパティには、1つのアクセサーまたは両方のアクセサーを含めることができます。

オーバーライドするプロパティの宣言には、`sealed` 修飾子を含めることができます。 この修飾子を使用すると、派生クラスでプロパティをさらにオーバーライドできなくなります。 シールされたプロパティのアクセサーもシールされます。

宣言と呼び出しの構文の違いを除けば、virtual、sealed、override、および abstract の各アクセサーは、仮想、シール、オーバーライド、抽象メソッドとまったく同じように動作します。 具体的には、「[仮想メソッド](classes.md#virtual-methods)」、 [「オーバーライドメソッド](classes.md#override-methods)」、「[シールメソッド](classes.md#sealed-methods)」、および「[抽象メソッド](classes.md#abstract-methods)」で説明されている規則は、アクセサーが対応する形式のメソッドである場合と同様に適用されます。

*  `get` アクセサーは、プロパティの型の戻り値と、それを含むプロパティと同じ修飾子を持つパラメーターなしのメソッドに対応します。
*  `set` アクセサーは、プロパティ型の単一の値パラメーター、`void` の戻り値の型、および格納しているプロパティと同じ修飾子を持つメソッドに対応します。

この例では、
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
`X` は仮想読み取り専用プロパティ、`Y` は仮想読み取り/書き込みプロパティ、`Z` は抽象読み取り/書き込みプロパティです。 `Z` は抽象であるため、含んでいるクラス `A` も abstract として宣言する必要があります。

`A` から派生するクラスを次に示します。
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

ここでは、`X`、`Y`、および `Z` の宣言は、プロパティ宣言をオーバーライドしています。 各プロパティ宣言は、対応する継承されたプロパティのアクセシビリティ修飾子、型、および名前と完全に一致します。 `X` の `get` アクセサーと `Y` の `set` アクセサーは、`base` キーワードを使用して、継承されたアクセサーにアクセスします。 `Z` の宣言は、両方の抽象アクセサーをオーバーライドします。したがって、`B`には未処理の抽象関数メンバーはなく、`B` は非抽象クラスにすることが許可されています。

プロパティが `override`として宣言されている場合、オーバーライドされたアクセサーには、オーバーライドする側のコードからアクセスできる必要があります。 さらに、プロパティまたはインデクサー自体とアクセサーの両方について宣言されたアクセシビリティは、オーバーライドされるメンバーとアクセサーのアクセシビリティと一致する必要があります。 例 :
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a>イベント

***イベント***は、オブジェクトまたはクラスが通知を提供できるようにするメンバーです。 クライアントは、***イベントハンドラー***を指定することによって、イベントの実行可能コードを添付できます。

イベントは*event_declaration*s を使用して宣言されます。

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

*Event_declaration*には、*属性*([属性](attributes.md)) のセットと、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせ、`new` ([新しい修飾子](classes.md#the-new-modifier))、`static` ([静的メソッドとインスタンスメソッド](classes.md#static-and-instance-methods))、`virtual` ([仮想メソッド](classes.md#virtual-methods))、`override` ([オーバーライドメソッド](classes.md#override-methods))、([シールメソッド](classes.md#sealed-methods))、`sealed` ([抽象メソッド](classes.md#abstract-methods))、および `abstract` ([外部メソッド](classes.md#external-methods)) 修飾子を含めることができます。`extern`

イベント宣言には、修飾子の有効な組み合わせに関して、メソッド宣言 ([メソッド](classes.md#methods)) と同じ規則が適用されます。

イベント宣言の*型*は、 *delegate_type* ([参照型](types.md#reference-types)) である必要があります。また、 *delegate_type*は、少なくともイベント自体 ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints)) と同様にアクセス可能である必要があります。

イベント宣言には、 *event_accessor_declarations*を含めることができます。 ただし、非 extern の非抽象イベントの場合は、コンパイラによって自動的に提供されます ([フィールドに似たイベント](classes.md#field-like-events))。extern イベントの場合、アクセサーは外部に提供されます。

*Event_accessor_declarations*を省略するイベント宣言は、1つ以上のイベント ( *variable_declarator*のそれぞれに1つ) を定義します。 属性と修飾子は、このような*event_declaration*によって宣言されたすべてのメンバーに適用されます。

*Event_declaration*に `abstract` 修飾子と中かっこで区切られた*event_accessor_declarations*の両方を含めると、コンパイル時にエラーが発生します。

イベント宣言に `extern` 修飾子が含まれている場合、イベントは***外部イベント***と呼ばれます。 外部イベント宣言は実際の実装を提供しないため、`extern` 修飾子と*event_accessor_declarations*の両方を含めることはできません。

`abstract` または `external` 修飾子を持つイベント宣言の*variable_declarator*が*variable_initializer*を含むようにすると、コンパイル時にエラーが発生します。

イベントは、`+=` 演算子と `-=` 演算子 ([イベント割り当て](expressions.md#event-assignment)) の左側のオペランドとして使用できます。 これらの演算子は、イベントからイベントハンドラーを削除するために、またはイベントからイベントハンドラーを削除するために、それぞれ使用されます。また、イベントのアクセス修飾子は、そのような操作が許可されているコンテキストを制御します。

`+=` と `-=` は、イベントを宣言する型以外のイベントで許可される唯一の操作であるため、外部コードはイベントのハンドラーを追加および削除できますが、それ以外のイベントハンドラーの一覧を取得または変更することはできません。

`x += y` または `x -= y`の形式の操作では、`x` がイベントであり、`x`の宣言を含む型の外で参照が行われた場合、演算の結果には `void` 型 (`x`の型ではなく、代入後の `x` の値) が含まれます。 この規則は、外部コードがイベントの基になるデリゲートを間接的に調べることを禁止します。

次の例は、イベントハンドラーが `Button` クラスのインスタンスにアタッチされる方法を示しています。
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

ここでは、`LoginDialog` インスタンスコンストラクターが2つの `Button` インスタンスを作成し、イベントハンドラーを `Click` イベントにアタッチします。

### <a name="field-like-events"></a>フィールドに似たイベント

イベントの宣言を含むクラスまたは構造体のプログラムテキスト内では、フィールドのように特定のイベントを使用できます。 このように使用するには、イベントを `abstract` または `extern`してはならず、 *event_accessor_declarations*を明示的に含めることはできません。 このようなイベントは、フィールドを許可する任意のコンテキストで使用できます。 フィールドには、イベントに追加されたイベントハンドラーのリストを参照するデリゲート ([デリゲート](delegates.md)) が含まれています。 イベントハンドラーが追加されていない場合、フィールドには `null`が含まれます。

この例では、
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
`Click` は、`Button` クラス内のフィールドとして使用されます。 この例で示すように、フィールドは、デリゲート呼び出し式で検査、変更、および使用できます。 `Button` クラスの `OnClick` メソッドは、`Click` イベントを発生させます。 イベントを発生させるという概念は、イベントによって表されるデリゲートの呼び出しとまったく同じです。したがって、イベントを発生させるための特殊な言語コンストラクトはありません。 デリゲート呼び出しの前に、デリゲートが null でないことを確認するチェックが付いていることに注意してください。

`Button` クラスの宣言の外側では、`Click` メンバーは、`+=` および `-=` 演算子の左側でのみ使用できます。
```csharp
b.Click += new EventHandler(...);
```
デリゲートを `Click` イベントの呼び出しリストに追加します。
```csharp
b.Click -= new EventHandler(...);
```
`Click` イベントの呼び出しリストからデリゲートを削除します。

フィールドに似たイベントをコンパイルすると、コンパイラは、デリゲートを保持するためのストレージを自動的に作成し、デリゲートフィールドに対してイベントハンドラーを追加または削除するイベントのアクセサーを作成します。 追加操作と削除操作は、スレッドセーフであり、インスタンスイベントの格納オブジェクトに対してロック ([lock ステートメント](statements.md#the-lock-statement)) を保持しているとき、または静的イベントの型オブジェクト ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) に対して実行する必要があります (ただし、必須ではありません)。

したがって、次のような形式のインスタンスイベント宣言があります。
```csharp
class X
{
    public event D Ev;
}
```
は、次のものに相当するものにコンパイルされます。
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
クラス `X`内では、`+=` および `-=` 演算子の左側にある `Ev` への参照によって、add アクセサーと remove アクセサーが呼び出されます。 `Ev` へのその他のすべての参照は、非表示フィールド `__Ev` ([メンバーアクセス](expressions.md#member-access)) を参照するようにコンパイルされます。 "`__Ev`" という名前は任意です。隠しフィールドには、任意の名前を付けることも、名前をまったく指定しないこともできます。

### <a name="event-accessors"></a>イベント アクセサー

イベント宣言は、通常、上記の `Button` の例のように、 *event_accessor_declarations*を省略します。 これを行う1つの状況として、イベントごとに1つのフィールドのストレージコストが許容されない場合が挙げられます。 このような場合、クラスには*event_accessor_declarations*を追加し、イベントハンドラーのリストを格納するためのプライベート機構を使用できます。

イベントの*event_accessor_declarations*では、イベントハンドラーの追加と削除に関連付けられた実行可能なステートメントを指定します。

アクセサー宣言は、 *add_accessor_declaration*と*remove_accessor_declaration*で構成されます。 各アクセサー宣言は `add` または `remove` の後に*ブロック*が続くトークンで構成されます。 *Add_accessor_declaration*に関連付けられた*ブロック*は、イベントハンドラーが追加されたときに実行するステートメントを指定します。また、 *remove_accessor_declaration*に関連付けられている*ブロック*には、イベントハンドラーが削除されたときに実行するステートメントを指定します。

各*add_accessor_declaration*と*remove_accessor_declaration*は、イベントの種類の単一の値パラメーターと `void` の戻り値の型を持つメソッドに対応します。 イベントアクセサーの暗黙のパラメーターには `value`という名前が付けられます。 イベントの割り当てでイベントが使用される場合は、適切なイベントアクセサーが使用されます。 具体的には、代入演算子が `+=` 場合、add アクセサーが使用され、代入演算子が `-=` 場合は、remove アクセサーが使用されます。 どちらの場合も、代入演算子の右側のオペランドは、イベントアクセサーの引数として使用されます。 *Add_accessor_declaration*または*remove_accessor_declaration*のブロックは、「[メソッド本体](classes.md#method-body)」で説明されている `void` メソッドの規則に準拠している必要があります。 特に、このようなブロック内の `return` ステートメントでは、式を指定することはできません。

イベントアクセサーには `value`という名前のパラメーターが暗黙的に含まれるため、イベントアクセサーで宣言されたローカル変数または定数がその名前を持つようにすると、コンパイル時エラーになります。

この例では、
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
`Control` クラスは、イベントの内部ストレージ機構を実装します。 `AddEventHandler` メソッドはデリゲート値をキーに関連付け、`GetEventHandler` メソッドは、現在キーに関連付けられているデリゲートを返します。また、`RemoveEventHandler` メソッドは、指定されたイベントのイベントハンドラーとしてデリゲートを削除します。 たとえば、基になるストレージメカニズムは、`null` デリゲート値をキーに関連付けてもコストが発生しないように設計されているため、未処理のイベントはストレージを使用しません。

### <a name="static-and-instance-events"></a>静的イベントとインスタンスイベント

イベント宣言に `static` 修飾子が含まれている場合、イベントは***静的イベント***と呼ばれます。 `static` 修飾子が存在しない場合、イベントは***インスタンスイベント***と呼ばれます。

静的イベントは特定のインスタンスに関連付けられておらず、静的イベントのアクセサーで `this` を参照するコンパイル時エラーです。

インスタンスイベントは、クラスの特定のインスタンスに関連付けられます。このインスタンスには、そのイベントのアクセサーで `this` ([このアクセス](expressions.md#this-access)) としてアクセスできます。

フォーム `E.M`の*member_access* ([メンバーアクセス](expressions.md#member-access)) でイベントが参照されている場合、`M` が静的イベントである場合、`E` は `M`を含む型を示す必要があり、`M` がインスタンスイベントの場合、E は `M`を含む型のインスタンスを示す必要があります。

静的メンバーとインスタンスメンバーの違いについては、「[静的メンバーとインスタンス](classes.md#static-and-instance-members)メンバー」で詳しく説明します。

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>仮想、シール、オーバーライド、および抽象イベントアクセサー

`virtual` イベント宣言は、そのイベントのアクセサーが仮想であることを指定します。 `virtual` 修飾子は、イベントの両方のアクセサーに適用されます。

`abstract` イベント宣言は、イベントのアクセサーが仮想であることを指定しますが、アクセサーの実際の実装は提供しません。 代わりに、非抽象派生クラスは、イベントをオーバーライドすることによって、アクセサーの独自の実装を提供する必要があります。 抽象イベント宣言は実際の実装を提供しないため、中かっこで区切られた*event_accessor_declarations*を提供することはできません。

`abstract` と `override` の両方の修飾子を含むイベント宣言は、イベントが抽象であり、基本イベントをオーバーライドすることを指定します。 このようなイベントのアクセサーも抽象的なものです。

抽象イベント宣言は、抽象クラス ([抽象クラス](classes.md#abstract-classes)) でのみ許可されます。

継承された仮想イベントのアクセサーは、`override` 修飾子を指定するイベント宣言を含めることによって、派生クラスでオーバーライドできます。 これは、オーバーライドする***イベント宣言***と呼ばれます。 オーバーライドするイベント宣言では、新しいイベントが宣言されていません。 代わりに、既存の仮想イベントのアクセサーの実装を単に特殊化します。

オーバーライドするイベント宣言では、オーバーライドされたイベントとまったく同じアクセシビリティ修飾子、型、および名前を指定する必要があります。

オーバーライドするイベント宣言には、`sealed` 修飾子を含めることができます。 この修飾子を使用すると、派生クラスでイベントをさらにオーバーライドできなくなります。 シールされたイベントのアクセサーもシールされます。

オーバーライドするイベント宣言に `new` 修飾子を含めると、コンパイル時にエラーが発生します。

宣言と呼び出しの構文の違いを除けば、virtual、sealed、override、および abstract の各アクセサーは、仮想、シール、オーバーライド、抽象メソッドとまったく同じように動作します。 具体的には、「[仮想メソッド](classes.md#virtual-methods)」、 [「オーバーライドメソッド](classes.md#override-methods)」、「[シールメソッド](classes.md#sealed-methods)」、および「[抽象メソッド](classes.md#abstract-methods)」で説明されている規則は、アクセサーが対応するフォームのメソッドである場合と同様に適用されます。 各アクセサーは、イベントの種類の単一の値パラメーター、`void` の戻り値の型、および含んでいるイベントと同じ修飾子を持つメソッドに対応します。

## <a name="indexers"></a>インデクサー

***インデクサー***は、配列と同じ方法でオブジェクトのインデックスを作成できるようにするメンバーです。 インデクサーは*indexer_declaration*s を使用して宣言されます。

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

*Indexer_declaration*には、一連の*属性*([属性](attributes.md)) と、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers))、`new` ([新しい修飾子](classes.md#the-new-modifier))、`virtual` ([仮想メソッド](classes.md#virtual-methods))、`override` ([オーバーライドメソッド](classes.md#override-methods))、`sealed` ([シールメソッド](classes.md#sealed-methods))、`abstract` ([抽象メソッド](classes.md#abstract-methods))、および `extern` ([外部メソッド](classes.md#external-methods)) 修飾子の有効な組み合わせを含めることができます。

インデクサー宣言では、修飾子の有効な組み合わせに関して、メソッド宣言 ([メソッド](classes.md#methods)) と同じ規則が適用されます。一方、インデクサー宣言では static 修飾子は許可されないという例外があります。

修飾子 `virtual`、`override`、および `abstract` は、1つの場合を除き、相互に排他的です。 Abstract インデクサーが仮想1をオーバーライドできるように、`abstract` 修飾子と `override` 修飾子を一緒に使用することができます。

インデクサー宣言の*型*は、宣言によって導入されるインデクサーの要素の種類を指定します。 インデクサーが明示的なインターフェイスメンバーの実装でない限り、*型*の後にキーワード `this`が続きます。 明示的なインターフェイスメンバーの実装では、*型*の後に*interface_type*、"`.`"、およびキーワード `this`が続きます。 他のメンバーとは異なり、インデクサーにはユーザー定義の名前がありません。

*Formal_parameter_list*は、インデクサーのパラメーターを指定します。 インデクサーの仮パラメーターリストは、メソッドのパラメーター ([メソッドパラメーター](classes.md#method-parameters)) と対応しています。ただし、パラメーターを少なくとも1つ指定する必要があります。また、`ref` および `out` パラメーター修飾子は使用できません。

インデクサーの*型*と*formal_parameter_list*で参照される各型は、少なくとも、インデクサー自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。

*Indexer_body*は、***アクセサー本体***または***式の本体***で構成されている場合があります。 アクセサー本体で、 *accessor_declarations*を "`{`" および "`}`" トークンで囲む必要がある場合は、プロパティのアクセサー ([アクセサー](classes.md#accessors)) を宣言します。 アクセサーは、プロパティの読み取りと書き込みに関連付けられた実行可能なステートメントを指定します。

"`=>`" の後に式 `E` が続く式本体と、セミコロンはステートメント本体 `{ get { return E; } }`とまったく同じであり、そのため、getter の結果が1つの式で指定される getter のみのインデクサーを指定する場合にのみ使用できます。

インデクサー要素にアクセスするための構文は、配列要素の構文と同じですが、インデクサー要素は変数として分類されません。 このため、インデクサー要素を `ref` または `out` 引数として渡すことはできません。

インデクサーの仮パラメーターリストでは、インデクサーの署名 ([シグネチャとオーバーロード](basic-concepts.md#signatures-and-overloading)) を定義します。 具体的には、インデクサーのシグネチャは、その仮パラメーターの数と型で構成されます。 仮パラメーターの要素の型と名前は、インデクサーのシグネチャの一部ではありません。

インデクサーのシグネチャは、同じクラスで宣言されている他のすべてのインデクサーのシグネチャと異なる必要があります。

インデクサーとプロパティは概念とよく似ていますが、次の点が異なります。

*  プロパティは名前で識別されますが、インデクサーはそのシグネチャによって識別されます。
*  プロパティは、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバーアクセス](expressions.md#member-access)) を介してアクセスされます。一方、インデクサー要素は*element_access* ([インデクサーアクセス](expressions.md#indexer-access)) を介してアクセスされます。
*  プロパティは `static` メンバーにすることができますが、インデクサーは常にインスタンスメンバーになります。
*  プロパティの `get` アクセサーは、パラメーターのないメソッドに対応します。一方、インデクサーの `get` アクセサーは、インデクサーと同じ仮パラメーターリストを持つメソッドに対応します。
*  プロパティの `set` アクセサーは、`value`という名前の1つのパラメーターを持つメソッドに対応します。一方、インデクサーの `set` アクセサーは、インデクサーと同じ仮パラメーターリストと、`value`という名前の追加パラメーターを持つメソッドに対応します。
*  インデクサーアクセサーがインデクサーパラメーターと同じ名前を持つローカル変数を宣言する場合、コンパイル時エラーになります。
*  オーバーライドするプロパティの宣言では、`base.P`構文を使用して、継承されたプロパティにアクセスします。 `P` はプロパティ名です。 オーバーライドするインデクサーの宣言では、`base[E]`構文を使用して、継承されたインデクサーにアクセスします。 `E` は、コンマで区切られた式のリストです。
*  "自動的に実装されたインデクサー" の概念はありません。 セミコロン (;) 以外の非抽象インデクサーを使用すると、エラーになります。

これらの違いを除けば、[アクセサー](classes.md#accessors)と[自動的に実装](classes.md#automatically-implemented-properties)されるプロパティで定義されているすべての規則は、インデクサーアクセサーおよびプロパティアクセサーに適用されます。

インデクサー宣言に `extern` 修飾子が含まれている場合、インデクサーは***外部インデクサー***と呼ばれます。 外部インデクサー宣言は実際の実装を提供しないため、各*accessor_declarations*はセミコロンで構成されます。

次の例では、ビット配列内の個々のビットにアクセスするためのインデクサーを実装する `BitArray` クラスを宣言しています。
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

`BitArray` クラスのインスタンスは、対応する `bool[]` よりもかなり少ないメモリを消費します (前者の各値は、後者の1バイトではなく1ビットのみを占有するため)。ただし、`bool[]`と同じ操作を許可します。

次の `CountPrimes` クラスは、`BitArray` とクラシック "エラトステネス" アルゴリズムを使用して、1から指定された最大値までの primes 数を計算します。
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

`BitArray` の要素にアクセスするための構文は、`bool[]`の場合とまったく同じであることに注意してください。

次の例は、2つのパラメーターを持つインデクサーを持つ 26 * 10 grid クラスを示しています。 最初のパラメーターは、A ~ Z の範囲の大文字または小文字にする必要があり、2番目のパラメーターは0-9 の範囲の整数である必要があります。

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a>インデクサーのオーバーロード

インデクサーのオーバーロードの解決規則については、 [「型の推定](expressions.md#type-inference)」を参照してください。

## <a name="operators"></a>演算子

***演算子***は、クラスのインスタンスに適用できる式演算子の意味を定義するメンバーです。 演算子は*operator_declaration*s を使用して宣言されます。

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

オーバーロード可能な演算子には、単項演算子 ([単項演算子](classes.md#unary-operators))、二項演算子 ([二項](classes.md#binary-operators)演算子)、および変換演算子 ([変換演算子](classes.md#conversion-operators)) の3つのカテゴリがあります。

*Operator_body*は、セミコロン、***ステートメント本体***、または式の***本体***です。 ステートメントの本体は、*ブロック*で構成されます。これは、演算子が呼び出されたときに実行するステートメントを指定します。 *ブロック*は、「[メソッド本体](classes.md#method-body)」で説明されている値を返すメソッドの規則に準拠している必要があります。 式の本体は `=>` で構成され、その後に式とセミコロンが続き、演算子が呼び出されたときに実行する1つの式を示します。

`extern` 演算子の場合、 *operator_body*はセミコロンで構成されます。 その他のすべての演算子では、 *operator_body*はブロック本体または式の本体です。

すべての演算子宣言には、次の規則が適用されます。

*  演算子の宣言には、`public` と `static` 修飾子の両方を含める必要があります。
*  演算子のパラメーターは、値パラメーター ([値パラメーター](variables.md#value-parameters)) である必要があります。 演算子宣言で `ref` パラメーターまたは `out` パラメーターを指定する場合、コンパイル時エラーになります。
*  演算子のシグネチャ ([単項演算子](classes.md#unary-operators)、[二項演算子](classes.md#binary-operators)、[変換演算子](classes.md#conversion-operators)) は、同じクラスで宣言されている他のすべての演算子のシグネチャとは異なる必要があります。
*  演算子宣言で参照されるすべての型は、少なくとも演算子自体 ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints)) と同じようにアクセス可能である必要があります。
*  同じ修飾子が演算子宣言で複数回出現する場合、エラーになります。

各演算子カテゴリでは、次のセクションで説明するように、追加の制限が課せられます。

他のメンバーと同様に、基底クラスで宣言された演算子は、派生クラスによって継承されます。 演算子宣言は、演算子のシグネチャに参加するように宣言されているクラスまたは構造体を常に必要とするため、派生クラスで宣言された演算子が基底クラスで宣言された演算子を非表示にすることはできません。 したがって、演算子の宣言では、`new` 修飾子は不要であり、許可されません。

単項演算子と二項演算子の詳細については、「[演算子](expressions.md#operators)」を参照してください。

変換演算子の追加情報については、「[ユーザー定義変換](conversions.md#user-defined-conversions)」を参照してください。

### <a name="unary-operators"></a>単項演算子

次の規則は単項演算子の宣言に適用されます。 `T` は、演算子宣言を含むクラスまたは構造体のインスタンス型を表します。

*  単項 `+`、`-`、`!`、または `~` 演算子は、型 `T` または `T?` の1つのパラメーターを受け取る必要があり、任意の型を返すことができます。
*  単項 `++` または `--` 演算子は `T` または `T?` 型の1つのパラメーターを受け取る必要があり、同じ型またはそれから派生した型を返す必要があります。
*  単項 `true` または `false` 演算子は、型 `T` または `T?` の1つのパラメーターを受け取る必要があり、型 `bool`を返す必要があります。

単項演算子のシグネチャは、演算子トークン (`+`、`-`、`!`、`~`、`++`、`--`、`true`、または `false`) と、単一の仮パラメーターの型で構成されます。 戻り値の型は単項演算子のシグネチャの一部ではなく、仮パラメーターの名前でもありません。

`true` および `false` の単項演算子には、ペアごとの宣言が必要です。 クラスでこれらの演算子のいずれかが宣言されていない場合、コンパイル時にエラーが発生します。 `true` 演算子と `false` 演算子については、[ユーザー定義の条件付き論理演算子](expressions.md#user-defined-conditional-logical-operators)と[ブール式](expressions.md#boolean-expressions)を参照してください。

次の例は、整数 vector クラスの `operator ++` の実装とその後の使用法を示しています。
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

演算子メソッドが、オペランドに1を加算することによって生成される値を返す方法に注意してください。後置インクリメント演算子とデクリメント演算子 ([後置インクリメント](expressions.md#postfix-increment-and-decrement-operators)演算子およびデクリメント演算子) と同様、前置インクリメント演算子と前置デクリメント演算子 ([前置インクリメント演算子およびデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)) と同様です。 とはC++異なり、このメソッドでは、オペランドの値を直接変更する必要はありません。 実際、オペランドの値を変更すると、後置インクリメント演算子の標準セマンティクスに違反することになります。

### <a name="binary-operators"></a>バイナリ演算子

次の規則は、二項演算子の宣言に適用されます。ここで `T` は、演算子宣言を含むクラスまたは構造体のインスタンス型を表します。

*  二項非シフト演算子は、2つのパラメーターを受け取る必要があります。少なくとも1つは型 `T` または `T?`である必要があり、任意の型を返すことができます。
*  バイナリ `<<` または `>>` 演算子は、2つのパラメーターを受け取る必要があります。1つ目のパラメーターの型は `T` または `T?` で、2番目のパラメーターの型は `int` または `int?`である必要があり、任意の型を返すことができます。

二項演算子の署名は、演算子トークン (`+`、`-`、`*`、`/`、`%`、`&`、`|`、`^`、`<<`、`>>`、`==`、`!=`) と、2つの仮パラメーターの型で構成されますが、`>``<``>=``<=` 戻り値の型と仮パラメーターの名前は、二項演算子のシグネチャの一部ではありません。

特定の二項演算子には、ペアごとの宣言が必要です。 ペアのいずれかの演算子を宣言する場合は、そのペアのもう一方の演算子の宣言が一致している必要があります。 2つの演算子宣言は、同じ戻り値の型を持ち、各パラメーターの型が同じである場合に一致します。 次の演算子では、ペアごとの宣言が必要です。

*  `operator ==` および `operator !=`
*  `operator >` および `operator <`
*  `operator >=` および `operator <=`

### <a name="conversion-operators"></a>変換演算子

変換演算子の宣言では、定義済みの暗黙的な変換と明示的な変換を補強する、***ユーザー定義の変換***([ユーザー定義](conversions.md#user-defined-conversions)の変換) が導入されます。

`implicit` キーワードを含む変換演算子の宣言では、ユーザー定義の暗黙的な変換が導入されます。 暗黙の型変換は、関数メンバー呼び出し、キャスト式、代入など、さまざまな状況で発生する可能性があります。 この詳細については、「[暗黙的な変換](conversions.md#implicit-conversions)」を参照してください。

`explicit` キーワードを含む変換演算子の宣言では、ユーザー定義の明示的な変換が導入されます。 明示的な変換は、キャスト式で発生する可能性があり、[明示的な変換](conversions.md#explicit-conversions)で詳しく説明されています。

変換演算子は、変換演算子のパラメーター型で示される変換元の型を変換演算子の戻り値の型で指定されたターゲットの型に変換します。

指定されたソース型 `S` とターゲット型 `T`の場合、`S` または `T` が null 許容型である場合は、`S0` と `T0` がその基になる型を参照するようにします。それ以外の場合、`S0` と `T0` はそれぞれ `S` と `T` になります。 クラスまたは構造体は、次のすべての条件を満たす場合にのみ、ソース `S` 型からターゲット `T` 型への変換を宣言することができます。

*  `S0` と `T0` の種類は異なります。
*  `S0` または `T0` は、演算子の宣言が行われるクラスまたは構造体の型です。
*  `S0` も `T0` も*interface_type*ではありません。
*  ユーザー定義の変換を除外すると、`S` から `T` または `T` から `S`への変換は存在しません。

これらの規則のために、`S` または `T` に関連付けられている型パラメーターは、他の型との継承関係がない一意の型と見なされます。これらの型パラメーターに対する制約は無視されます。

この例では、
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
最初の2つの演算子宣言が許可されるのは、[インデクサー](classes.md#indexers).3、`T` および `int` と `string` がそれぞれ、リレーションシップのない一意の型と見なされるためです。 ただし、3番目の演算子はエラーになります。これは `C<T>` が `D<T>`の基本クラスであるためです。

2番目のルールでは、変換演算子は、演算子が宣言されているクラスまたは構造体型との間で変換を行う必要があることに従います。 たとえば、クラスまたは構造体の型 `C` は、`C` から `int` への変換と `int` から `C`への変換を定義できますが、`int` から `bool`への変換は定義できません。

定義済みの変換を直接再定義することはできません。 したがって、`object` と他のすべての型の間に暗黙的な変換と明示的な変換が既に存在しているため、変換演算子をまたは `object` に変換することはできません。 同様に、変換元と変換先の両方の型を他の型の基本型にすることはできません。これは、変換が既に存在するためです。

ただし、特定の型引数に対して、事前定義された変換として既に存在する変換を指定する場合は、ジェネリック型に対して演算子を宣言することができます。 この例では、
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
型 `object` が `T`の型引数として指定されている場合、2番目の演算子は、既に存在する変換 (暗黙的な変換でもあり、明示的には、任意の型から `object`型への変換) を宣言します。

2つの型の間に事前定義された変換が存在する場合、これらの型間のユーザー定義の変換はすべて無視されます。 具体的な内容は次のとおりです。

*  定義済みの暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) が型 `S` から `T`型に存在する場合、`S` から `T` へのすべてのユーザー定義変換 (暗黙的または明示的) は無視されます。
*  定義済みの明示的な変換 ([明示](conversions.md#explicit-conversions)的な変換) が型 `S` から型 `T`に存在する場合、`S` から `T` へのユーザー定義の明示的な変換はすべて無視されます。 さらに、

`T` がインターフェイス型の場合、`S` から `T` へのユーザー定義の暗黙的な変換は無視されます。

それ以外の場合、`S` から `T` へのユーザー定義の暗黙的な変換は引き続き考慮されます。

すべての型に `object`ますが、上の `Convertible<T>` 型で宣言された演算子は、事前に定義された変換と競合しません。 例 :
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

ただし、型 `object`の場合、事前定義された変換によって、すべてのケースで1つのユーザー定義変換が非表示になります。

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

ユーザー定義の変換では、から*interface_type*への変換は許可されていません。 特に、この制限により、 *interface_type*への変換時にユーザー定義の変換が発生しないようにし、変換されるオブジェクトが実際に指定の*interface_type*を実装する場合にのみ、 *interface_type*への変換が成功するようにします。

変換演算子のシグネチャは、変換元の型と変換先の型で構成されます。 (これは、戻り値の型がシグネチャに参加する唯一の形式のメンバーであることに注意してください)。変換演算子の `implicit` または `explicit` 分類は、演算子のシグネチャの一部ではありません。 したがって、クラスまたは構造体は、`implicit` と、同じソースとターゲットの型を持つ `explicit` 変換演算子の両方を宣言することはできません。

一般に、ユーザー定義の暗黙の変換は、例外をスローせず、情報を失わないように設計する必要があります。 ユーザー定義の変換によって例外が発生する可能性がある場合 (たとえば、ソース引数が範囲外の場合) や情報が失われた場合 (上位ビットを破棄する場合など) は、その変換を明示的な変換として定義する必要があります。

この例では、
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
`Digit` から `byte` への変換は、例外をスローしたり情報を失ったりすることはないため、暗黙的に行われますが、`Digit` は `byte`の使用可能な値のサブセットのみを表すことができるため、`byte` から `Digit` への変換は明示的です。

## <a name="instance-constructors"></a>インスタンスコンストラクター

"***インスタンス コンストラクター***" は、クラスのインスタンスを初期化するために必要なアクションを実装するメンバーです。 インスタンスコンストラクターは*constructor_declaration*s を使用して宣言されています。

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

*Constructor_declaration*には、*属性*のセット ([属性](attributes.md))、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせ、および `extern` ([外部メソッド](classes.md#external-methods)) 修飾子を含めることができます。 コンストラクター宣言では、同じ修飾子を複数回含めることはできません。

*Constructor_declarator*の*識別子*は、インスタンスコンストラクターが宣言されているクラスの名前を指定する必要があります。 他の名前が指定されている場合は、コンパイル時エラーが発生します。

インスタンスコンストラクターのオプションの*formal_parameter_list*には、[メソッド (メソッド](classes.md#methods)) の*formal_parameter_list*と同じ規則が適用されます。 仮パラメーターリストでは、インスタンスコンストラクターの署名 ([シグネチャとオーバーロード](basic-concepts.md#signatures-and-overloading)) を定義し、オーバーロードの解決 ([型の推定](expressions.md#type-inference)) が呼び出しで特定のインスタンスコンストラクターを選択するプロセスを制御します。

インスタンスコンストラクターの*formal_parameter_list*で参照される各型は、少なくともコンストラクター自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。

省略可能な*constructor_initializer*は、このインスタンスコンストラクターの*constructor_body*で指定されたステートメントを実行する前に呼び出す別のインスタンスコンストラクターを指定します。 これについては、「[コンストラクター初期化子](classes.md#constructor-initializers)」で詳しく説明します。

コンストラクターの宣言に `extern` 修飾子が含まれている場合、コンストラクターは***外部コンストラクター***と呼ばれます。 外部コンストラクターの宣言は実際の実装を提供しないため、 *constructor_body*はセミコロンで構成されます。 他のすべてのコンストラクターについては、 *constructor_body*は、クラスの新しいインスタンスを初期化するステートメントを指定する*ブロック*で構成されます。 これは、`void` 戻り値の型 ([メソッド本体](classes.md#method-body)) を持つインスタンスメソッドの*ブロック*に正確に対応します。

インスタンスコンストラクターは継承されません。 したがって、クラスには、クラスで実際に宣言されているコンストラクター以外のインスタンスコンストラクターがありません。 クラスにインスタンスコンストラクターの宣言が含まれていない場合は、既定のインスタンスコンストラクターが自動的に提供されます ([既定のコンストラクター](classes.md#default-constructors))。

インスタンスコンストラクターは*object_creation_expression*s ([オブジェクト作成式](expressions.md#object-creation-expressions)) および*constructor_initializer*によって呼び出されます。

### <a name="constructor-initializers"></a>Constructor Initializers (コンストラクター初期化子)

すべてのインスタンスコンストラクター (クラス `object`の場合を除く) には、 *constructor_body*の直前に別のインスタンスコンストラクターを呼び出すことが暗黙的に含まれます。 暗黙的に呼び出すコンストラクターは、 *constructor_initializer*によって決定されます。

*  `base(argument_list)` または `base()` フォームのインスタンスコンストラクター初期化子によって、直接の基底クラスのインスタンスコンストラクターが呼び出されます。 このコンストラクターは、 *argument_list*存在する場合は、[オーバーロード解決](expressions.md#overload-resolution)のオーバーロード解決規則を使用して選択されます。 候補インスタンスコンストラクターのセットは、直接基底クラスに含まれるすべてのアクセス可能なインスタンスコンストラクター、または、直接基底クラスでインスタンスコンストラクターが宣言されていない場合は既定のコンストラクター ([既定のコンストラクター](classes.md#default-constructors)) で構成されます。 このセットが空の場合、または1つの最適なインスタンスコンストラクターを識別できない場合は、コンパイル時エラーが発生します。
*  `this(argument-list)` または `this()` フォームのインスタンスコンストラクター初期化子によって、クラス自体のインスタンスコンストラクターが呼び出されます。 コンストラクターは、 *argument_list*存在する場合は、[オーバーロード解決](expressions.md#overload-resolution)のオーバーロード解決規則を使用して選択されます。 候補インスタンスコンストラクターのセットは、クラス自体で宣言されたすべてのアクセス可能なインスタンスコンストラクターで構成されます。 このセットが空の場合、または1つの最適なインスタンスコンストラクターを識別できない場合は、コンパイル時エラーが発生します。 インスタンスコンストラクターの宣言にコンストラクター自体を呼び出すコンストラクター初期化子が含まれている場合、コンパイル時エラーが発生します。

インスタンスコンストラクターにコンストラクター初期化子がない場合は、`base()` フォームのコンストラクター初期化子が暗黙的に指定されます。 そのため、フォームのインスタンスコンストラクター宣言
```csharp
C(...) {...}
```
はとまったく同じです。
```csharp
C(...): base() {...}
```

インスタンスコンストラクター宣言の*formal_parameter_list*によって指定されたパラメーターのスコープには、その宣言のコンストラクター初期化子が含まれます。 したがって、コンストラクターの初期化子は、コンストラクターのパラメーターにアクセスすることが許可されます。 例 :
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

インスタンスコンストラクターの初期化子が、作成されているインスタンスにアクセスできません。 したがって、コンストラクター初期化子の引数式で `this` を参照するにはコンパイル時エラーになります。これは、引数式が*simple_name*を通じてインスタンスメンバーを参照する場合のコンパイル時エラーであるということです。

### <a name="instance-variable-initializers"></a>インスタンス変数初期化子

インスタンスコンストラクターにコンストラクター初期化子がない場合、または `base(...)`フォームのコンストラクター初期化子が存在する場合、そのコンストラクターはクラスで宣言されたインスタンスフィールドの*variable_initializer*s によって指定された初期化を暗黙的に実行します。 これは、コンストラクターへのエントリと、直接基底クラスのコンストラクターの暗黙的な呼び出しの直前に実行される割り当てのシーケンスに対応します。 変数初期化子は、クラス宣言に含まれるテキスト順に実行されます。

### <a name="constructor-execution"></a>コンストラクターの実行

変数初期化子は代入ステートメントに変換され、これらの代入ステートメントは、基底クラスのインスタンスコンストラクターが呼び出される前に実行されます。 この順序により、インスタンスにアクセスできるステートメントが実行される前に、すべてのインスタンスフィールドが変数初期化子によって初期化されます。

例を次に示します。
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
`B`のインスタンスを作成するために `new B()` を使用すると、次の出力が生成されます。
```console
x = 1, y = 0
```

`x` の値は1です。これは、基底クラスのインスタンスコンストラクターが呼び出される前に変数初期化子が実行されるためです。 ただし、`y` の値は 0 (`int`の既定値) です。 `y` への割り当ては、基底クラスのコンストラクターが返されるまで実行されないためです。

インスタンス変数初期化子とコンストラクター初期化子は、 *constructor_body*の前に自動的に挿入されるステートメントと考えると便利です。 例
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
複数の変数初期化子が含まれています。また、両方の形式 (`base` と `this`) のコンストラクター初期化子も含まれています。 この例は、以下に示すコードに対応しています。各コメントは、自動的に挿入されたステートメントを示します (自動的に挿入されたコンストラクターの呼び出しに使用される構文は有効ではありませんが、単に機構を示すためにのみ機能します)。

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a>既定のコンストラクター

クラスにインスタンスコンストラクターの宣言が含まれていない場合は、既定のインスタンスコンストラクターが自動的に指定されます。 この既定のコンストラクターは、単に直接基底クラスのパラメーターなしのコンストラクターを呼び出します。 クラスが abstract の場合、既定のコンストラクターに対して宣言されたアクセシビリティは保護されます。 それ以外の場合、既定のコンストラクターに対して宣言されたアクセシビリティは public になります。 したがって、既定のコンストラクターは常にフォームになります。

```csharp
protected C(): base() {}
```
、または
```csharp
public C(): base() {}
```
ここで `C` はクラスの名前です。 オーバーロードの解決で基底クラスのコンストラクターの初期化子に最適な一意の候補を特定できない場合、コンパイル時エラーが発生します。

この例では、
```csharp
class Message
{
    object sender;
    string text;
}
```
クラスにはインスタンスコンストラクターの宣言が含まれていないため、既定のコンストラクターが用意されています。 したがって、この例はとまったく同じです。
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>プライベートコンストラクター

クラス `T` がプライベートインスタンスコンストラクターのみを宣言する場合、`T` のプログラムテキストの外部のクラスは `T` から派生したり、`T`のインスタンスを直接作成したりすることはできません。 したがって、クラスに静的メンバーのみが含まれ、インスタンス化されない場合は、空のプライベートインスタンスコンストラクターを追加すると、インスタンス化ができなくなります。 例 :
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

`Trig` クラスは、関連するメソッドと定数をグループ化しますが、インスタンス化するためのものではありません。 したがって、1つの空のプライベートインスタンスコンストラクターを宣言します。 既定のコンストラクターが自動生成されないようにするには、少なくとも1つのインスタンスコンストラクターを宣言する必要があります。

### <a name="optional-instance-constructor-parameters"></a>省略可能なインスタンスコンストラクターパラメーター

コンストラクター初期化子の `this(...)` 形式は、一般に、省略可能なインスタンスコンストラクターパラメーターを実装するためにオーバーロードと組み合わせて使用されます。 この例では、
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
最初の2つのインスタンスコンストラクターは、不足している引数の既定値を提供するだけです。 どちらも、`this(...)` コンストラクター初期化子を使用して3番目のインスタンスコンストラクターを呼び出します。これにより、実際には、新しいインスタンスを初期化する作業が行われます。 これは、省略可能なコンストラクターパラメーターの効果です。
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>静的コンストラクター

***静的コンストラクター***は、closed クラス型を初期化するために必要なアクションを実装するメンバーです。 静的コンストラクターは*static_constructor_declaration*s を使用して宣言されます。

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

*Static_constructor_declaration*には、一連の*属性*([属性](attributes.md)) と `extern` 修飾子 ([外部メソッド](classes.md#external-methods)) を含めることができます。

*Static_constructor_declaration*の*識別子*は、静的コンストラクターが宣言されているクラスの名前を指定する必要があります。 他の名前が指定されている場合は、コンパイル時エラーが発生します。

静的コンストラクターの宣言に `extern` 修飾子が含まれている場合、静的コンストラクターは***外部の静的コンストラクター***と呼ばれます。 外部の静的コンストラクター宣言は実際の実装を提供しないため、 *static_constructor_body*はセミコロンで構成されます。 その他のすべての静的コンストラクター宣言では、 *static_constructor_body*は、クラスを初期化するために実行するステートメントを指定する*ブロック*で構成されます。 これは、`void` 戻り値の型 ([メソッド本体](classes.md#method-body)) を持つ静的メソッドの*method_body*に相当します。

静的コンストラクターは継承されず、直接呼び出すことはできません。

Closed クラス型の静的コンストラクターは、特定のアプリケーションドメインで1回だけ実行されます。 静的コンストラクターの実行は、アプリケーションドメイン内で発生する次のイベントの最初のイベントによってトリガーされます。

*  クラス型のインスタンスが作成されます。
*  クラス型のいずれかの静的メンバーが参照されています。

実行を開始する `Main` メソッド ([アプリケーションのスタートアップ](basic-concepts.md#application-startup)) がクラスに含まれている場合、そのクラスの静的コンストラクターは、`Main` メソッドが呼び出される前に実行されます。

クローズしたクラスの新しい型を初期化するには、まず、その特定のクローズされた型の新しい静的フィールド ([静的フィールドとインスタンスフィールド](classes.md#static-and-instance-fields)) のセットを作成します。 各静的フィールドは、既定値 ([既定値](variables.md#default-values)) に初期化されます。 次に、静的フィールドの初期化子 (静的フィールドの[初期化](classes.md#static-field-initialization)) を静的フィールドに対して実行します。 最後に、静的コンストラクターが実行されます。

例
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
出力を生成する必要があります。
```console
Init A
A.F
Init B
B.F
```
`A`の静的コンストラクターの実行は `A.F`への呼び出しによってトリガーされるため、`B`の静的コンストラクターの実行は `B.F`への呼び出しによってトリガーされます。

変数初期化子を持つ静的フィールドを既定値の状態で観察できるようにする、循環依存関係を構築することができます。

例
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
この例では、次のように出力されます。
```console
X = 1, Y = 2
```

`Main` メソッドを実行するために、システムはまず、クラス `B`の静的コンストラクターの前に `B.Y`の初期化子を実行します。 `A.X` の値が参照されているため、`Y`の初期化子によって `A`の静的コンストラクターが実行されます。  `A` の静的コンストラクターは、 `X`の値の計算を続行します。これにより、 `Y`の既定値 (0) がフェッチされます。 したがって `A.X` が1に初期化されます。 `A`の静的フィールド初期化子と静的コンストラクターを実行するプロセスが完了し、 `Y`の初期値の計算に戻ります。結果は2になります。

静的コンストラクターは、終了した構築済みのクラス型ごとに1回だけ実行されるため、制約 ([型パラメーターの制約](classes.md#type-parameter-constraints)) によってコンパイル時にチェックできない型パラメーターに対してランタイムチェックを適用するのに便利な場所です。 たとえば、次の型では、静的コンストラクターを使用して、型引数が列挙型であることを強制しています。
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a>デストラクター

***デストラクター***は、クラスのインスタンスを消滅させるために必要なアクションを実装するメンバーです。 デストラクターは*destructor_declaration*を使用して宣言されます。

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

*Destructor_declaration*には、一連の*属性*([属性](attributes.md)) を含めることができます。

*Destructor_declaration*の*識別子*は、デストラクターが宣言されているクラスの名前を指定する必要があります。 他の名前が指定されている場合は、コンパイル時エラーが発生します。

デストラクター宣言に `extern` 修飾子が含まれている場合、デストラクターは***外部デストラクター***と呼ばれます。 外部デストラクター宣言は実際の実装を提供しないため、 *destructor_body*はセミコロンで構成されます。 その他のすべてのデストラクターでは、 *destructor_body*は、クラスのインスタンスを破棄するために実行するステートメントを指定する*ブロック*で構成されます。 *Destructor_body*は、`void` の戻り値の型 ([メソッド本体](classes.md#method-body)) を持つインスタンスメソッドの*method_body*に正確に対応します。

デストラクターは継承されません。 したがって、クラスには、そのクラスで宣言されている可能性のあるデストラクターがありません。

デストラクターにはパラメーターが必要ないため、オーバーロードすることはできないため、クラスは最大で1つのデストラクターを持つことができます。

デストラクターは自動的に呼び出されるため、明示的に呼び出すことはできません。 インスタンスをコードで使用することができなくなった場合、インスタンスは破棄の対象になります。 インスタンスのデストラクターの実行は、インスタンスが破棄の対象になった後、いつでも発生する可能性があります。 インスタンスが破棄されされると、そのインスタンスの継承チェーン内のデストラクターは、ほとんどの派生から最小の派生まで、順に呼び出されます。 デストラクターは、任意のスレッドで実行できます。 デストラクターを実行するタイミングと方法を制御する規則の詳細については、「[自動メモリ管理](basic-concepts.md#automatic-memory-management)」を参照してください。

例の出力
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
is
```
B's destructor
A's destructor
```
継承チェーン内のデストラクターは、ほとんどの派生から最小の派生から順に呼び出されるためです。

デストラクターは、`System.Object`で `Finalize` 仮想メソッドをオーバーライドすることによって実装されます。 C#プログラムでは、このメソッドのオーバーライドまたは呼び出し (またはそのオーバーライド) を直接行うことはできません。 たとえば、プログラム
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
2つのエラーが含まれています。

コンパイラは、このメソッドとオーバーライドがまったく存在しないかのように動作します。 そのため、このプログラムは次のようになります。
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
が有効であり、表示されているメソッドが `System.Object`の `Finalize` メソッドを非表示にします。

デストラクターから例外がスローされた場合の動作の詳細については、「[例外の処理方法](exceptions.md#how-exceptions-are-handled)」を参照してください。

## <a name="iterators"></a>反復子

反復子ブロック ([ブロック](statements.md#blocks)) を使用して実装される関数メンバー ([関数メンバー](expressions.md#function-members)) は、***反復子***と呼ばれます。

対応する関数メンバーの戻り値の型が列挙子インターフェイス ([列挙子](classes.md#enumerator-interfaces)インターフェイス) の1つであるか、または列挙可能なインターフェイス (列挙可能な[インターフェイス](classes.md#enumerable-interfaces)) の1つである限り、反復子ブロックは関数メンバーの本体として使用できます。 これは、 *method_body*、 *operator_body*または*accessor_body*として発生する可能性があります。一方、イベント、インスタンスコンストラクター、静的コンストラクター、およびデストラクターを反復子として実装することはできません。

関数メンバーが反復子ブロックを使用して実装されている場合、関数メンバーの仮パラメーターリストで `ref` または `out` パラメーターを指定すると、コンパイル時エラーになります。

### <a name="enumerator-interfaces"></a>列挙子インターフェイス

***列挙子インターフェイス***は、非ジェネリックインターフェイス `System.Collections.IEnumerator` と、ジェネリックインターフェイス `System.Collections.Generic.IEnumerator<T>`のすべてのインスタンス化です。 簡潔にするために、この章では、これらのインターフェイスをそれぞれ `IEnumerator` と `IEnumerator<T>`として参照しています。

### <a name="enumerable-interfaces"></a>列挙可能なインターフェイス

***列挙***可能なインターフェイスは、非ジェネリックインターフェイス `System.Collections.IEnumerable` と、ジェネリックインターフェイス `System.Collections.Generic.IEnumerable<T>`のすべてのインスタンス化です。 簡潔にするために、この章では、これらのインターフェイスをそれぞれ `IEnumerable` と `IEnumerable<T>`として参照しています。

### <a name="yield-type"></a>Yield 型

反復子は、すべて同じ型の値のシーケンスを生成します。 この型は、反復子の***yield 型***と呼ばれます。

*  `IEnumerator` を返す反復子の yield 型、または `IEnumerable` が `object`である。
*  `IEnumerator<T>` を返す反復子の yield 型、または `IEnumerable<T>` が `T`である。

### <a name="enumerator-objects"></a>列挙子オブジェクト

列挙子インターフェイス型を返す関数メンバーが反復子ブロックを使用して実装されている場合、関数メンバーを呼び出すと反復子ブロックのコードはすぐには実行されません。 代わりに、***列挙子オブジェクト***が作成され、返されます。 このオブジェクトは、反復子ブロックで指定されたコードをカプセル化し、列挙子オブジェクトの `MoveNext` メソッドが呼び出されたときに反復子ブロック内のコードを実行します。 列挙子オブジェクトには、次の特性があります。

*  `IEnumerator` と `IEnumerator<T>`を実装します。 `T` は反復子の yield 型です。
*  このクラスは、`System.IDisposable` を実装します。
*  引数値 (存在する場合) と、関数メンバーに渡されるインスタンス値のコピーを使用して初期化されます。
*  これには、"***前***"、"***実行中***"、"***中断***"、および "***後***" の4つの状態があり、最初は "***前***" の状態になります。

列挙子オブジェクトは、通常、コンパイラによって生成される列挙子クラスのインスタンスであり、反復子ブロックのコードをカプセル化し、列挙子インターフェイスを実装しますが、その他の実装方法も可能です。 列挙子クラスがコンパイラによって生成される場合、そのクラスは、関数メンバーを含むクラスで直接的または間接的に入れ子にされ、プライベートアクセシビリティを持ち、コンパイラで使用するために予約された名前 ([識別子](lexical-structure.md#identifiers)) になります。

列挙子オブジェクトには、上で指定したものよりも多くのインターフェイスを実装できます。

次のセクションでは、列挙子オブジェクトによって提供される `IEnumerable` および `IEnumerable<T>` インターフェイスの実装の `MoveNext`、`Current`、および `Dispose` メンバーの正確な動作について説明します。

列挙子オブジェクトは、`IEnumerator.Reset` メソッドをサポートしていないことに注意してください。 このメソッドを呼び出すと `System.NotSupportedException` がスローされます。

#### <a name="the-movenext-method"></a>MoveNext メソッド

列挙子オブジェクトの `MoveNext` メソッドは、反復子ブロックのコードをカプセル化します。 `MoveNext` メソッドを呼び出すと、反復子ブロック内のコードが実行され、必要に応じて列挙子オブジェクトの `Current` プロパティが設定されます。 `MoveNext` によって実行される正確なアクションは、`MoveNext` が呼び出されたときの列挙子オブジェクトの状態によって異なります。

*  列挙子オブジェクトの状態が***以前***である場合は、`MoveNext`を呼び出します。
   * 状態を***実行中***に変更します。
   * 反復子オブジェクトが初期化されたときに保存された引数値とインスタンス値に対して、反復子ブロックのパラメーター (`this`を含む) を初期化します。
   * 次に示すように、実行が中断されるまで、最初から反復子ブロックを実行します。
*  列挙子オブジェクトの状態が [***実行中***] の場合、`MoveNext` を呼び出した結果は指定されません。
*  列挙子オブジェクトの状態が***中断***されている場合は `MoveNext`を呼び出します。
   * 状態を***実行中***に変更します。
   * すべてのローカル変数とパラメーター (this を含む) の値を、反復子ブロックの実行が最後に中断されたときに保存された値に復元します。 これらの変数によって参照されるオブジェクトの内容は、MoveNext の前の呼び出し以降に変更されている可能性があることに注意してください。
   * 実行の中断の原因となった `yield return` ステートメントのすぐ後に反復子ブロックの実行を再開し、実行が中断されるまで続行します (後述)。
*  列挙子オブジェクトの状態がの***後***にある場合は、`MoveNext` を呼び出すと `false`が返されます。


`MoveNext` が反復子ブロックを実行すると、4つの方法で実行を中断できます。これには、`yield return` ステートメント、`yield break` ステートメント、反復子ブロックの終わりを検出し、例外がスローされ、反復子ブロックの外に伝達されます。

*  `yield return` ステートメントが発生したとき ([yield ステートメント](statements.md#the-yield-statement)):
   * ステートメントで指定された式が評価され、暗黙的に yield 型に変換され、列挙子オブジェクトの `Current` プロパティに割り当てられます。
   * 反復子本体の実行は中断されています。 この `yield return` ステートメントの場所と同様に、すべてのローカル変数とパラメーター (`this`を含む) の値が保存されます。 `yield return` ステートメントが1つ以上の `try` ブロック内にある場合、関連付けられた `finally` ブロックは現時点では実行されません。
   * 列挙子オブジェクトの状態が "***中断***" に変更されます。
   * `MoveNext` メソッドは、`true` を呼び出し元に返します。これは、反復処理が次の値に正常に進んだことを示します。
*  `yield break` ステートメントが発生したとき ([yield ステートメント](statements.md#the-yield-statement)):
   * `yield break` ステートメントが1つ以上の `try` ブロック内にある場合は、関連付けられた `finally` ブロックが実行されます。
   * 列挙子オブジェクトの状態が***after***に変更されます。
   * `MoveNext` メソッドは、反復処理が完了したことを示す `false` を呼び出し元に返します。
*  反復子本体の終わりが見つかった場合:
   * 列挙子オブジェクトの状態が***after***に変更されます。
   * `MoveNext` メソッドは、反復処理が完了したことを示す `false` を呼び出し元に返します。
*  例外がスローされ、反復子ブロックの外に伝達される場合:
   * 反復子本体内の適切な `finally` ブロックは、例外の反映によって実行されます。
   * 列挙子オブジェクトの状態が***after***に変更されます。
   * 例外の伝達は、`MoveNext` メソッドの呼び出し元に続きます。

#### <a name="the-current-property"></a>現在のプロパティ

列挙子オブジェクトの `Current` プロパティは、反復子ブロック内の `yield return` ステートメントの影響を受けます。

列挙子オブジェクトが***中断***状態にある場合、`Current` の値は、前に `MoveNext`を呼び出したときに設定された値になります。 列挙子オブジェクトが [***前***]、[***実行中***]、または [***後***] の状態にある場合、`Current` にアクセスした結果は指定されません。

`object`以外の yield 型を持つ反復子の場合、列挙子オブジェクトの `IEnumerable` 実装を通じて `Current` にアクセスしたときの結果は、列挙子オブジェクトの `IEnumerator<T>` 実装を通じて `Current` にアクセスし、結果を `object`にキャストすることと対応しています。

#### <a name="the-dispose-method"></a>Dispose メソッド

`Dispose` メソッドは、列挙子オブジェクトを***after***状態にすることによって、反復処理をクリーンアップするために使用されます。

*  列挙子オブジェクトの状態が***以前***の場合、 ***`Dispose` を呼び***出すと、状態がに変更されます。
*  列挙子オブジェクトの状態が [***実行中***] の場合、`Dispose` を呼び出した結果は指定されません。
*  列挙子オブジェクトの状態が***中断***されている場合は `Dispose`を呼び出します。
   * 状態を***実行中***に変更します。
   * 最後に実行された `yield return` ステートメントが `yield break` ステートメントであったかのように、finally ブロックを実行します。 これによって例外がスローされ、反復子本体の外に伝達される場合、列挙子オブジェクトの状態が***after***に設定され、例外が `Dispose` メソッドの呼び出し元に反映されます。
   * 状態をの***後***に変更します。
*  列挙子オブジェクトの状態がの***後***にある場合、`Dispose` を呼び出すことはできません。

### <a name="enumerable-objects"></a>列挙可能なオブジェクト

列挙可能なインターフェイス型を返す関数メンバーが反復子ブロックを使用して実装されている場合、関数メンバーを呼び出すと、反復子ブロックのコードはすぐには実行されません。 代わりに、列挙可能な***オブジェクト***が作成されて返されます。 列挙可能なオブジェクトの `GetEnumerator` メソッドは、反復子ブロックで指定されたコードをカプセル化する列挙子オブジェクトを返します。反復子オブジェクトの `MoveNext` メソッドが呼び出されると、反復子ブロック内のコードが実行されます。 列挙可能なオブジェクトには、次の特性があります。

*  `IEnumerable` と `IEnumerable<T>`を実装します。 `T` は反復子の yield 型です。
*  引数値 (存在する場合) と、関数メンバーに渡されるインスタンス値のコピーを使用して初期化されます。

列挙可能なオブジェクトは、通常、コンパイラによって生成される列挙可能なクラスのインスタンスであり、反復子ブロックのコードをカプセル化し、列挙可能なインターフェイスを実装しますが、その他の実装方法も可能です。 列挙可能なクラスがコンパイラによって生成される場合、そのクラスは、関数メンバーを含むクラスで直接的または間接的に入れ子にされ、プライベートアクセシビリティを持ち、コンパイラで使用するために予約された名前 ([識別子](lexical-structure.md#identifiers)) になります。

列挙可能なオブジェクトは、上で指定したものよりも多くのインターフェイスを実装できます。 特に、列挙可能なオブジェクトは `IEnumerator` と `IEnumerator<T>`を実装して、列挙可能と列挙子の両方として機能できるようにすることもできます。 この種類の実装では、列挙可能なオブジェクトの `GetEnumerator` メソッドが初めて呼び出されるときに、列挙可能なオブジェクト自体が返されます。 その後、列挙可能なオブジェクトの `GetEnumerator`を呼び出すと、列挙可能なオブジェクトのコピーが返されます。 したがって、返される各列挙子には独自の状態があり、1つの列挙子の変更は別の列挙子に影響しません。

#### <a name="the-getenumerator-method"></a>GetEnumerator メソッド

列挙可能なオブジェクトは、`IEnumerable` および `IEnumerable<T>` インターフェイスの `GetEnumerator` メソッドの実装を提供します。 2つの `GetEnumerator` メソッドは、使用可能な列挙子オブジェクトを取得して返す共通の実装を共有します。 列挙子オブジェクトは、列挙可能なオブジェクトが初期化されたときに保存された引数値とインスタンス値を使用して初期化されますが、それ以外の場合は、列挙子オブジェクトは「 [enumerator オブジェクト](classes.md#enumerator-objects)」の説明に従って機能します。

### <a name="implementation-example"></a>実装の例

このセクションでは、標準的C#なコンストラクトに関して、反復子の実装について説明します。 ここで説明する実装は、Microsoft C#コンパイラで使用されるのと同じ原則に基づいていますが、必須の実装であるか、または可能な唯一の方法ではありません。

次の `Stack<T>` クラスは、反復子を使用して、その `GetEnumerator` メソッドを実装します。 反復子は、スタックの要素を上から下の順に列挙します。

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

`GetEnumerator` メソッドは、次に示すように、反復子ブロックのコードをカプセル化する、コンパイラによって生成される列挙子クラスのインスタンス化に変換できます。

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

前の変換では、反復子ブロック内のコードはステートマシンに変換され、enumerator クラスの `MoveNext` メソッドに配置されます。 さらに、ローカル変数 `i` が列挙子オブジェクトのフィールドに変換されるため、`MoveNext`の呼び出しの間に存在し続けることができます。

次の例では、1 ~ 10 の整数の単純な乗算テーブルを出力します。 この例の `FromTo` メソッドは、列挙可能なオブジェクトを返し、反復子を使用して実装されます。

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

`FromTo` メソッドは、次に示すように、反復子ブロックのコードをカプセル化する、コンパイラによって生成される列挙可能なクラスのインスタンス化に変換できます。

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

列挙可能なクラスは、列挙可能なインターフェイスと列挙子インターフェイスの両方を実装します。これにより、列挙可能なと列挙子の両方として機能できるようになります。 `GetEnumerator` メソッドが初めて呼び出されたときに、列挙可能なオブジェクト自体が返されます。 その後、列挙可能なオブジェクトの `GetEnumerator`を呼び出すと、列挙可能なオブジェクトのコピーが返されます。 したがって、返される各列挙子には独自の状態があり、1つの列挙子の変更は別の列挙子に影響しません。 `Interlocked.CompareExchange` メソッドは、スレッドセーフな操作を確実に行うために使用されます。

`from` パラメーターと `to` パラメーターは、列挙可能なクラスのフィールドに変換されます。 反復子ブロックで `from` が変更されるため、各列挙子の `from` に与えられた初期値を保持するために、追加の `__from` フィールドが導入されます。

`MoveNext` メソッドは、`__state` が `0`ときに呼び出されると、`InvalidOperationException` をスローします。 これにより、最初に `GetEnumerator`を呼び出すことなく、列挙可能なオブジェクトを列挙子オブジェクトとして使用することが防止されます。

次の例は、単純なツリークラスを示しています。 `Tree<T>` クラスは、反復子を使用して、その `GetEnumerator` メソッドを実装します。 反復子は、ツリーの要素を挿入順序で列挙します。

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

`GetEnumerator` メソッドは、次に示すように、反復子ブロックのコードをカプセル化する、コンパイラによって生成される列挙子クラスのインスタンス化に変換できます。

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

`foreach` ステートメントで使用されるコンパイラによって生成された一時要素は、列挙子オブジェクトの `__left` および `__right` フィールドにリフトされます。 列挙子オブジェクトの `__state` フィールドは、例外がスローされた場合に正しい `Dispose()` メソッドが正しく呼び出されるように、慎重に更新されます。 単純な `foreach` ステートメントを使用して、翻訳したコードを記述することはできないことに注意してください。

## <a name="async-functions"></a>非同期関数

`async` 修飾子を持つメソッド ([メソッド](classes.md#methods)) または匿名関数 ([匿名関数式](expressions.md#anonymous-function-expressions)) は、***非同期関数***と呼ばれます。 一般に、 ***async***という用語は、`async` 修飾子を持つ任意の種類の関数を記述するために使用されます。

非同期関数の仮パラメーターリストで `ref` または `out` パラメーターを指定すると、コンパイル時にエラーになります。

非同期メソッドの*return_type*は `void` または***タスクの種類***である必要があります。 タスクの種類は `System.Threading.Tasks.Task`、`System.Threading.Tasks.Task<T>`から構築された型です。 簡潔にするために、この章では、これらの型はそれぞれ `Task` と `Task<T>`として参照されています。 タスクの種類を返す非同期メソッドは、タスクを返すと言います。

タスクの種類の正確な定義は実装で定義されていますが、言語の観点から見ると、タスクの種類は [未完了]、[成功]、または [失敗] のいずれかの状態になります。 エラーが発生したタスクは、関連する例外を記録します。 Succeeded `Task<T>` は `T`型の結果を記録します。 タスクの種類は待機可能であるため、await 式 ([await 式](expressions.md#await-expressions)) のオペランドにすることができます。

非同期関数呼び出しでは、その本体で await 式 ([await 式](expressions.md#await-expressions)) を使って評価を中断できます。 評価は、***再開デリゲート***によって、中断 await 式の時点で後で再開される場合があります。 再開デリゲートの型は `System.Action`であり、呼び出されると、非同期関数呼び出しの評価は、中断した await 式から再開されます。 非同期関数呼び出しの***現在の呼び出し***元は、関数呼び出しが中断されていない場合は元の呼び出し元、それ以外の場合は再開デリゲートの最新の呼び出し元です。

### <a name="evaluation-of-a-task-returning-async-function"></a>タスクを返す非同期関数の評価

タスクを返す非同期関数を呼び出すと、返されたタスクの型のインスタンスが生成されます。 これは、async 関数の***return タスク***と呼ばれます。 タスクは、最初は不完全な状態です。

非同期関数本体は、(await 式に到達することによって) 中断されるか終了するまで評価されます。また、制御が戻りタスクと共に呼び出し元に返されます。

非同期関数の本体が終了すると、返されるタスクは不完全な状態から移動します。

*  関数本体が return ステートメントまたは本体の末尾に到達した結果として終了した場合、結果の値は、succeeded タスクに記録されます。これは succeeded 状態になります。
*  キャッチされていない例外 ([throw ステートメント](statements.md#the-throw-statement)) の結果として関数本体が終了した場合、例外はエラー状態になるリターンタスクに記録されます。

### <a name="evaluation-of-a-void-returning-async-function"></a>Void を返す非同期関数の評価

非同期関数の戻り値の型が `void`場合、次のように評価が異なります。タスクが返されないため、関数は、現在のスレッドの***同期コンテキスト***に対して完了と例外を通信します。 同期コンテキストの正確な定義は実装に依存しますが、現在のスレッドが実行されている "where" の表現です。 Void を返す非同期関数の評価が開始されるか、正常に完了するか、キャッチされていない例外がスローされると、同期コンテキストに通知されます。

これにより、コンテキストは、その下で実行されている void を返す非同期関数の数を追跡し、その中で例外を伝達する方法を決定できます。
