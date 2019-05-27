---
ms.openlocfilehash: 917e2f1e196013f85eefbda21fb3d717cc681084
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193907"
---
# <a name="classes"></a>クラス

クラスは、データ メンバー (定数とフィールド)、関数メンバー (メソッド、プロパティ、イベント、インデクサー、演算子、インスタンス コンス トラクター、デストラクター、静的コンス トラクター)、および入れ子にされた型を含む可能性のあるデータ構造です。 クラス型では、継承、派生クラスが基底クラスを拡張および限定するためのメカニズムをサポートします。

## <a name="class-declarations"></a>クラス宣言

A *class_declaration*は、 *type_declaration* ([入力宣言](namespaces.md#type-declarations)) 新しいクラスを宣言します。

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

A *class_declaration*オプションのセットから成る*属性*([属性](attributes.md)) オプションのセットと、その後*class_modifier*s ([修飾子をクラス](classes.md#class-modifiers)) と省略可能なその後`partial`の後に、キーワード修飾子`class`と*識別子*続く、クラスの名前を示す、省略可能な*type_parameter_list* ([パラメーター入力](classes.md#type-parameters)) と省略可能なその後*class_base*仕様 ([クラスの基本仕様](classes.md#class-base-specification)) オプションのセットと、その後*type_parameter_constraints_clause*s ([パラメーターの制約入力](classes.md#type-parameter-constraints))、その後に、 *class_body* ([クラス本体](classes.md#class-body))、セミコロンで必要に応じてその後にします。

クラス宣言を提供できません*type_parameter_constraints_clause*s も用意されていない限り、 *type_parameter_list*します。

クラスの宣言を提供する、 *type_parameter_list*は、***ジェネリック クラス宣言***します。 さらに、ジェネリック クラス宣言またはジェネリック構造体宣言内で入れ子になったクラス自体がジェネリック クラス宣言では、含んでいる型の型パラメーターを指定して構築された型を作成する必要がありますので。

### <a name="class-modifiers"></a>クラスの修飾子

A *class_declaration*クラス修飾子のシーケンスを必要に応じて含めることができます。

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

同じ修飾子をクラス宣言内で複数回のコンパイル時エラーになります。

`new`修飾子を入れ子になったクラスを使用します。 」の説明に従って、クラスが、同じ名前で、継承されたメンバーを非表示にするを指定します、 [new 修飾子](classes.md#the-new-modifier)します。 コンパイル時エラーには、`new`修飾子を入れ子になったクラス宣言ではないクラス宣言に表示されます。

`public`、 `protected`、 `internal`、および`private`修飾子は、クラスのアクセシビリティを制御します。 クラス宣言が発生したコンテキストに応じてこれらの修飾子の一部は使用できません ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))。

`abstract`、`sealed`と`static`修飾子は次のセクションで説明します。

#### <a name="abstract-classes"></a>抽象クラス

`abstract`修飾子を使用して、クラスが完了したと基本クラスとしてのみ使用するものであることを指定します。 抽象クラスは、次の方法で非抽象クラスとは異なります。

*  抽象クラスは、直接インスタンス化できないし、コンパイル時エラーに使用する、`new`抽象クラスでの演算子。 変数と値がコンパイル時の型が抽象クラスを含めることは可能ですが、このような変数と値は必ずしもなります`null`または抽象型から派生した非抽象クラスのインスタンスへの参照が含まれています。
*  抽象クラスが許可されている (ただし必要ありません) に抽象メンバーを含めることができます。
*  抽象クラスをシールドすることはできません。

非抽象クラスは抽象クラスから派生するときに、非抽象クラスは、これらの抽象メンバーをオーバーライドするため、すべての継承抽象メンバーの実際の実装を含める必要があります。 例
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
抽象クラス`A`抽象メソッドを導入`F`します。 クラス`B`その他の方法が導入されました`G`の実装を提供しないので`F`、`B`する必要がありますも抽象として宣言します。 クラス`C`オーバーライド`F`し、実際の実装を提供します。 抽象メンバーがないので`C`、`C`許可しました (ただし必要ありません) は非抽象にします。

#### <a name="sealed-classes"></a>シール クラス

`sealed`修飾子がクラスからの派生を防ぐために使用します。 シール クラスが別のクラスの基底クラスとして指定されている場合、コンパイル時エラーが発生します。

シール クラスには、抽象クラスにもできません。

`sealed`修飾子は、主に、意図しない派生を防ぐために使用しますが、特定のランタイム最適化こともできます。 具体的には、シール クラス、派生クラスが含まれないことがわかっているため、シール クラスのインスタンス上の仮想関数メンバーの呼び出しを非仮想呼び出しに変換することができます。

#### <a name="static-classes"></a>静的クラス

`static`として宣言するクラスをマークする修飾子を使用する***静的クラス***します。 静的クラスはインスタンス化できない型として使用することはできません、および静的メンバーのみを含めることができます。 静的クラスは、拡張メソッドの宣言を含めることができますのみ ([拡張メソッド](classes.md#extension-methods))。

静的クラスの宣言では、次の制限を受けます。

*  静的クラスが含まない場合があります、`sealed`または`abstract`修飾子。 ただし、静的クラスをインスタンス化されたかから派生することはできません、ために動作するシールと abstract の両方の場合とに注意してください。
*  静的クラスが含まない場合があります、 *class_base*仕様 ([クラスの基本仕様](classes.md#class-base-specification)) し、基底クラスまたは実装されたインターフェイスのリストを明示的に指定できません。 静的クラスは、型から暗黙的に継承`object`します。
*  静的クラスは静的メンバーのみを含むことができます ([静的メンバーとインスタンス メンバー](classes.md#static-and-instance-members))。 定数と入れ子にされた型が静的メンバーとして分類されることに注意してください。
*  静的クラスを持つメンバーを含めることはできません`protected`または`protected internal`アクセシビリティを宣言します。

これらの制限に違反するコンパイル時エラーになります。

静的クラスには、インスタンス コンス トラクターがありません。 インスタンス コンス トラクター、静的クラスでない既定のインスタンス コンス トラクターを宣言することはできません ([既定のコンス トラクター](classes.md#default-constructors)) は、静的クラスを提供します。

静的クラスのメンバーが自動的に静的でないし、メンバーの宣言が明示的に含める必要があります、 `static` (定数と入れ子にされた型) を除く修飾子。 クラスは、外部の静的クラス内で入れ子になって、ときに、入れ子になったクラスは静的クラス明示的に含まれていない限り、`static`修飾子。

__静的クラスの型を参照します。__

A *namespace_or_type_name* ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names)) で許可されている場合は、静的クラスを参照

*  *Namespace_or_type_name*は、`T`で、 *namespace_or_type_name*フォームの`T.I`、または
*  *Namespace_or_type_name*は、`T`で、 *typeof_expression* ([引数リスト](expressions.md#argument-lists)1) フォームの`typeof(T)`します。

A *primary_expression* ([関数メンバー](expressions.md#function-members)) で許可されている場合は、静的クラスを参照

*  *Primary_expression*は、`E`で、 *member_access* ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) フォームの`E.I`します。

他のコンテキストで静的クラスを参照すると、コンパイル時エラーになります。 エラー構成要素の型の基本クラスとして使用する静的クラスにはたとえば、([入れ子になった型](classes.md#nested-types)) のメンバー、ジェネリック型引数、または型パラメーターの制約。 同様に、配列型、ポインター型で静的クラスは使用できません、 `new` 、キャスト式、式、`is`式、`as`式、`sizeof`式、または既定値の式。

### <a name="partial-modifier"></a>Partial 識別子

`partial`修飾子を示すために使用されるこの*class_declaration*部分型の宣言です。 指定された規則に従って、それを囲む名前空間または型の宣言内で同じ名前の複数の部分型の宣言は、フォームの 1 つの型宣言に結合、[部分型](classes.md#partial-types)します。

プログラムのテキストのセグメントを個別に分散クラスの宣言は、これらのセグメントを生成または別のコンテキスト内に保持する場合役立ちます。 たとえば、クラス宣言の 1 つの部分可能性がありますコンピューターによって生成されると、一方は手動で作成した他の。 2 つのテキストの分離は、他の更新と衝突する 1 つを更新プログラムを抑止します。

### <a name="type-parameters"></a>型パラメーター

型パラメーターは、構築された型を作成する指定された型引数のプレース ホルダーを示す単純な識別子です。 型パラメーターは、後で提供される型の正式なプレース ホルダーです。 一方、型引数 ([引数を入力](types.md#type-arguments)) が構築された型が作成されるときに、型パラメーターを置き換える実際の型。

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

クラス宣言で型パラメーターごとの宣言領域に名前を定義します ([宣言](basic-concepts.md#declarations)) そのクラスの。 そのため、別の型パラメーターと同じ名前を持つことはできませんまたはメンバーがそのクラスで宣言します。 型パラメーターには、型自体と同じ名前を持つことはできません。

### <a name="class-base-specification"></a>クラスの基本仕様

クラス宣言を含めることができます、 *class_base*クラスとインターフェイスの直接基底クラスを定義する仕様 ([インターフェイス](interfaces.md)) 直接クラスによって実装されます。

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

クラス宣言で指定された基本クラスは、構築済みクラス型を指定できます ([構築型](types.md#constructed-types))。 スコープ内の型パラメーターを伴うことができますが、基底クラスは、独自の型パラメーターにすることはできません。

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>基底クラス

ときに、 *class_type*に含まれている、 *class_base*、宣言されたクラスの直接の基本クラスを指定します。 クラス宣言がにない場合*class_base*、または、 *class_base*リストのみインターフェイスの種類、直接基底クラスがあると見なされます`object`します。 」の説明に従って、クラスの直接の基本クラスからの継承メンバー[継承](classes.md#inheritance)します。

例
```csharp
class A {}

class B: A {}
```
クラス`A`の直接基底クラスと呼ばれます`B`、および`B`から派生する言います`A`します。 `A`は直接基底クラスを明示的に指定、その直接の基本クラスが暗黙的に`object`します。

構築済みクラス型では、ジェネリック クラスの宣言で基底クラスが指定されている場合、構築された型の基本クラスを取得、ごとに置き換えることによって*type_parameter*対応する基本クラスの宣言で*type_argument*構築された型のです。 ジェネリック クラス宣言
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
構築された型の基本クラス`G<int>`なります`B<string,int[]>`します。

クラス型の直接の基本クラスは、少なくとも、クラス型自体と同程度にアクセスである必要があります ([アクセシビリティ ドメイン](basic-concepts.md#accessibility-domains))。 などのコンパイル時エラーは、`public`クラスから派生する、`private`または`internal`クラス。

クラス型の直接の基本クラスでは、次の種類のいずれかのある必要がありますいない: `System.Array`、 `System.Delegate`、 `System.MulticastDelegate`、 `System.Enum`、または`System.ValueType`します。 ジェネリック クラス宣言をさらに、使用することはできません`System.Attribute`直接的または間接的な基底クラスとして。

直接基底クラスの指定の意味を決定する際に`A`クラスの`B`、直接の基本クラスの`B`は一時的にあると見なされます`object`します。 基底クラスの指定の意味が再帰的にことはできません、直感的にこれにより、それ自体に依存します。 例:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
以降、基底クラスの指定でのエラーでは、`A<C.B>`の直接の基本クラス`C`と見なされます`object`、ため、(のルールによって[Namespace と型の名前](basic-concepts.md#namespace-and-type-names))`C`と見なされないメンバーを持つ`B`します。

クラス型の基底クラスは、直接基底クラスとその基本クラスです。 つまり、基本クラスのセットは、直接基底クラスの関係の推移閉包です。 上記の基本クラスの例を参照する`B`は`A`と`object`します。 例
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
基本クラス`D<int>`は`C<int[]>`、 `B<IComparable<int[]>>`、 `A`、および`object`します。

クラスを除く`object`、すべてのクラス型が正確に 1 つの直接基底クラス。 `object`クラスの直接基底クラスを持たないし、他のすべてのクラスの最終的な基底クラスです。

クラスと`B`クラスから派生する`A`、コンパイル時エラーには`A`に依存する`B`します。 クラス***に直接依存***(あれば) 直接基本クラスと***に直接依存***はすぐに入れ子になっている (あれば) クラス。 この定義を指定するには、クラスが依存しているクラスの完全なセットがの再帰的、および推移的なクロージャ、***に直接依存***リレーションシップ。

例では、
```csharp
class A: A {}
```
誤ったクラスは、自身に依存しているためです。 例では、同様に、
```csharp
class A: B {}
class B: C {}
class C: A {}
```
クラスは、循環的自体に依存するためにはエラーです。 例では、最後に、
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
ため、コンパイル時エラー結果`A`に依存`B.C`(直接基底クラス) に依存する`B`(すぐ外側のクラス) に依存する循環的`A`。

クラスは、内部に入れ子になっているクラスに依存しないことに注意してください。 例
```csharp
class A
{
    class B: A {}
}
```
`B` 依存`A`(ため`A`は、直接基底クラスとそのすぐ外側のクラスの両方) が`A`に依存しない`B`(ため`B`は基底クラスでも、外側のクラスの`A`). そのため、この例では有効です。

派生することはできません、`sealed`クラス。 例
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
クラス`B`しようとするから派生するために、エラーでは、`sealed`クラス`A`します。

#### <a name="interface-implementations"></a>インターフェイスの実装

A *class_base*仕様は場合、クラスが言います、特定のインターフェイス型を直接実装するインターフェイスの型の一覧を含めることができます。 インターフェイスの実装については、説明でさらに[インターフェイスの実装](interfaces.md#interface-implementations)します。

### <a name="type-parameter-constraints"></a>型パラメーターの制約

ジェネリック型、メソッドの宣言を含めることで型パラメーターの制約を指定できます必要に応じて*type_parameter_constraints_clause*秒。

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

各*type_parameter_constraints_clause*トークンから成る`where`後に、型パラメーターの名前の後にコロンとその型パラメーターの制約の一覧。 最大で 1 つできます`where`型パラメーターごとに、句、`where`句は、任意の順序で表示できます。 ように、`get`と`set`プロパティのアクセサー内のトークン、`where`トークンは、キーワードではありません。

指定された制約の一覧を`where`句は、次のコンポーネントのいずれかをこの順序で含めることができます。 1 つの主な制約、1 つまたは複数のセカンダリ制約、およびコンス トラクターの制約`new()`します。

主な制約は、クラス型を指定できますまたは***参照型制約***`class`または***値の型の制約*** `struct`。 セカンダリの制約として使用できる、 *type_parameter*または*interface_type*します。

参照型の制約では、型パラメーターを使用する型引数は参照型である必要がありますを指定します。 すべてのクラス型、インターフェイス型、デリゲート型、配列型、および (下記を参照) のように、参照型に既知の型パラメーターは、この制約を満たしています。

値型の制約では、型パラメーターを使用する型引数は null 非許容値型である必要がありますを指定します。 すべての null 非許容の構造体型、列挙型、および値型の制約を持つ型パラメーターは、この制約を満たしています。 値型、null 許容型に分類されますが ([null 許容型](types.md#nullable-types)) 値の型の制約を満たしていません。 値型の制約を持つ型パラメーターも含めることはできません、 *constructor_constraint*します。

ポインター型では、型引数を許可しないと、参照型または値型のいずれかの制約を満たすためには考慮されません。

クラス型、インターフェイス型または型パラメーター制約とは、その型は最小限に抑える「ベース型」型パラメーターに使用されるすべての型引数をサポートする必要がありますを指定します。 構築された型またはジェネリック メソッドを使用すると、型引数がコンパイル時に型パラメーターに対する制約に照らしてチェックします。 指定された型引数で説明する条件を満たす必要があります[制約条件を満たす](types.md#satisfying-constraints)します。

A *class_type*制約は、次の規則を満たす必要があります。

*  種類は、クラス型である必要があります。
*  型でなければなりません`sealed`します。
*  型は、次の種類のいずれかを指定しない必要があります: `System.Array`、 `System.Delegate`、 `System.Enum`、または`System.ValueType`します。
*  型でなければなりません`object`します。 すべての型が派生するため`object`、このような制約は効果がありませんが、許可された場合。
*  特定の型パラメーターを最大で 1 つの制約は、クラス型であることができます。

として指定された型、 *interface_type*制約は、次の規則を満たす必要があります。

*  型はインターフェイス型である必要があります。
*  複数回に指定されていない型もする必要がありますを指定した`where`句。

いずれの場合も、制約は、関連付けられている型または構築された型の一部として、メソッドの宣言の型パラメーターのいずれかが含まれることができ、宣言された型が含まれることができます。

型パラメーターの制約は、少なくともにアクセスする必要がありますを指定したクラスまたはインターフェイスの型 ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints)) ジェネリック型またはメソッドが宣言されているとします。

として指定された型、 *type_parameter*制約は、次の規則を満たす必要があります。

*  種類は、型パラメーターである必要があります。
*  複数回に指定されていない型もする必要がありますを指定した`where`句。

さらにはならないサイクルによって定義されている推移的な関係の依存関係のある、型パラメーターの依存関係グラフ。

*  場合、型パラメーター`T`型パラメーターを制約として使用`S`し`S`***異なります***`T`します。
*  場合、型パラメーター`S`型パラメーターに依存`T`と`T`型パラメーターに依存`U`し`S`***異なります*** `U`。

この関係がある、(直接または間接的に)、それ自体に依存する型パラメーターのコンパイル時エラーになります。

すべての制約は、依存する型パラメーターの間で一貫性のあるである必要があります。 場合のパラメーターを入力`S`型パラメーターに依存`T`し。

*  `T` 値型の制約が必要ありません。 それ以外の場合、`T`これは実質的にシール`S`と同じ型にすることが強制されます`T`、2 つの型パラメーターでは必要はありません。
*  場合`S`しの値の型の制約を持つ`T`は必要ありません、 *class_type*制約。
*  場合`S`が、 *class_type*制約`A`と`T`が、 *class_type*制約`B`恒等変換が存在する必要がありますまたは暗黙的な参照変換から`A`に`B`から暗黙の参照変換または`B`に`A`します。
*  場合`S`型パラメーターにも依存`U`と`U`が、 *class_type*制約`A`と`T`が、 *class_type*制約`B`恒等変換またはからの暗黙的な参照変換がある必要がありますし、`A`に`B`から暗黙の参照変換または`B`に`A`します。

それが有効で`S`が値型の制約と`T`参照型の制約にします。 これを効果的に制限`T`型に`System.Object`、 `System.ValueType`、 `System.Enum`、および任意のインターフェイス型。

場合、`where`句型パラメーターにはにコンス トラクターの制約が含まれています (フォームを持つ`new()`) を使用することは、`new`演算子、型のインスタンスを作成する ([オブジェクト作成式](expressions.md#object-creation-expressions)). 使用する型引数が値型の制約またはコンス トラクターの制約 (参照型パラメーターまたは型パラメーターでは、コンス トラクター制約する必要があります (このコンス トラクターは任意の値型に暗黙的に存在する) のパブリック コンス トラクターを指定すること[パラメーターの制約入力](classes.md#type-parameter-constraints)詳細については)。

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

次の例は、型パラメーターの依存関係グラフで循環が発生しているために、エラーには。
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

次の例では、その他の無効な状況を示します。
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

***実質的な基本クラス***型パラメーターの`T`が次のように定義されています。

*  場合`T`primary 制約がない場合や、その効果的な基底クラスの型パラメーター制約が`object`します。
*  場合`T`値型の制約、その効果的な基底クラスは`System.ValueType`します。
*  場合`T`が、 *class_type*制約`C`が*type_parameter*制約、その効果的な基底クラスは`C`します。
*  場合`T`にない*class_type*制約が、1 つまたは複数*type_parameter*制約、その効果的な基底クラスは、最も内側の型 ([リフトの変換演算子](conversions.md#lifted-conversion-operators)) の有効な基本クラスのセットでその*type_parameter*制約。 一貫性規則では、このような最も内側の型が存在することを確認します。
*  場合`T`両方を持つ、 *class_type*制約と 1 つ以上*type_parameter*制約、その効果的な基底クラスは、最も内側の型 ([リフトの変換演算子](conversions.md#lifted-conversion-operators)) から成るセットで、 *class_type*の制約`T`の効果的な基底クラスとその*type_parameter*制約。 一貫性規則では、このような最も内側の型が存在することを確認します。
*  場合`T`を持ち、参照型の制約はいいえ*class_type*制約、その効果的な基底クラスは`object`します。

T に制約がある場合、これらの規則の目的で`V`つまり、 *value_type*、代わりに最も固有の基本型の`V`は、 *class_type*します。 これは、明示的に特定の制約では発生しないことができますが、ジェネリック メソッドの制約は、オーバーライドするメソッドの宣言または明示的なインターフェイス メソッドの実装によって暗黙的に継承されたときに発生する可能性があります。

これらの規則は、有効な基本クラスが常にあることを確認、 *class_type*します。

***有効なインターフェイス セット***型パラメーターの`T`が次のように定義されています。

*  場合`T`にない*secondary_constraints*、その有効なインターフェイス セットが空です。
*  場合`T`が*interface_type*制約がない*type_parameter*制約、その有効なインターフェイス セットが一連の*interface_type*制約。
*  場合`T`にない*interface_type*制約が、 *type_parameter*制約、その有効なインターフェイス セットがの有効なインターフェイスのセットの和集合の*種類パラメーター*制約。
*  場合`T`両方*interface_type*制約と*type_parameter*制約、その有効なインターフェイス セットがそのセットの和集合*interface_type*制約との有効なインターフェイス セットをその*type_parameter*制約。

型パラメーターが***参照型に既知の***参照型の制約があるかどうか、または、有効な基本クラスが`object`または`System.ValueType`します。

制約付きの型パラメーターの型の値は、制約によって暗黙的にインスタンス メンバーへのアクセスに使用できます。 例
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
メソッド`IPrintable`上で直接呼び出すことができます`x`ため`T`が常に実装するために制約`IPrintable`します。

### <a name="class-body"></a>クラス本体

*Class_body*クラスのクラスのメンバーを定義します。

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>部分型

型宣言を複数に分割できるようにする***部分型の宣言***します。 型宣言は、当該プログラムのコンパイル時と実行時の処理の残りの部分の中に 1 つの宣言として扱われます、このセクションでは、規則に従っての部分から構成されます。

A *class_declaration*、 *struct_declaration*または*interface_declaration*が含まれる場合は、部分型の宣言を表す、`partial`修飾子。 `partial` キーワードでないし、キーワードのいずれかの直前に表示される場合、修飾子としてのみ機能`class`、`struct`または`interface`型宣言、または型の前に`void`メソッドの宣言でします。 他のコンテキストで、通常の識別子として使用できます。

部分型の宣言の各部分を含める必要があります、`partial`修飾子。 同じ名前を指定する必要があり、同じ名前空間または型の他の部分宣言で宣言します。 `partial`修飾子は、型宣言の追加部分が別の場所に存在してもこのようなその他の部分が存在することが必須ではないことを示します。 1 つの宣言を持つ型を含めることは、`partial`修飾子。

1 つの型宣言に、コンパイル時に、パーツをマージできるように、部分型のすべての部分を一緒にコンパイルする必要があります。 具体的には、部分型には既にコンパイルされている型を拡張することはできません。

使用して複数の部分に入れ子にされた型を宣言することがあります、`partial`修飾子。 通常、含んでいる型が宣言を使用して`partial`良好であり、入れ子にされた型の各部分を含んでいる型の別の部分で宣言したので。

`partial`デリゲートまたは列挙型の宣言に修飾子は許可されていません。

### <a name="attributes"></a>属性

部分型の属性は、各部分の属性を不特定の順序で組み合わせることで決定されます。 属性は、複数の部分に配置する場合は、型の属性を複数回指定すると同じです。 たとえば、2 つの部分があると:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
などの宣言に同等です。
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

型パラメーターの属性は、同様の方法で結合します。

### <a name="modifiers"></a>修飾子

部分型の宣言がアクセシビリティの指定が含まれてとき (、 `public`、 `protected`、 `internal`、および`private`修飾子)、アクセシビリティの仕様が含まれるその他のすべての部分に同意する必要があります。 アクセシビリティの指定を含む、部分的な型の部分がない場合、型が指定された適切な既定のアクセシビリティ ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))。

入れ子にされた型の 1 つまたは複数の部分宣言が含まれる場合、`new`修飾子は、入れ子にされた型が継承されたメンバーを非表示にする場合に警告が報告されません ([継承による隠ぺい](basic-concepts.md#hiding-through-inheritance))。

クラスの 1 つまたは複数の部分宣言が含まれる場合、`abstract`修飾子、クラスが抽象と見なされます ([抽象クラス](classes.md#abstract-classes))。 それ以外の場合、クラスは、非抽象と見なされます。

クラスの 1 つまたは複数の部分宣言が含まれる場合、`sealed`修飾子、クラスがシールと見なされます ([クラスをシール](classes.md#sealed-classes))。 クラスを考慮する場合は、封印されていません。

クラスを abstract と sealed の両方ができないことに注意してください。

ときに、`unsafe`その特定の部分は、unsafe コンテキストと見なされるのみ、部分型の宣言に修飾子が使用されます ([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts))。

### <a name="type-parameters-and-constraints"></a>型パラメーターと制約

ジェネリック型が複数の部分で宣言されている場合、型パラメーターが各部分に記述する必要があります。 各部分は、順番、型パラメーターのと同じ数と型パラメーターごとに、同じ名前が必要です。

部分的なジェネリック型の宣言が制約が含まれる場合 (`where`句)、制約は制約を含むその他のすべての部分と一致する必要があります。 具体的には、制約が含まれる各の部分は、型パラメーターの同じセットに対して制約を持つ必要があり、型パラメーターごとのプライマリ、セカンダリ、およびコンス トラクター制約必要がありますと同じです。 制約の 2 つのセットは、同じメンバーが含まれている場合に相当します。 部分的なジェネリック型の一部は、型パラメーターの制約がないを指定します、パラメーターと見なされる型は無制限です。

例では、
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
正しい効果的に制約 (最初の 2 つ) を含む部分が同じセカンダリをプライマリのセットと同じ型パラメーターでは、一連のコンス トラクターの制約を指定するためです。

### <a name="base-class"></a>基底クラス

部分クラス宣言には、基底クラスの指定が含まれています。 基底クラスの指定を含むその他のすべての部分を持つことに同意する必要があります。 部分クラスの一部に基底クラスの指定が含まれていない場合、基本クラスは次のようになります。 `System.Object` ([基底クラス](classes.md#base-classes))。

### <a name="base-interfaces"></a>基底インターフェイス

複数の部分で宣言された型の基本インターフェイスのセットは、各部分で指定された基本インターフェイスの和集合です。 各部分では、1 回にという名前のみ、特定の基本インターフェイスも可能性がありますが、同じ基本インターフェイスの名前を複数の部分のことができます。 任意の基本インターフェイスのメンバーの実装の 1 つだけあります。

例
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
クラスの基本インターフェイスのセット`C`は`IA`、 `IB`、および`IC`します。

通常、各部分はその部分で宣言されたインターフェイスの実装を提供します。ただし、これは要件ではありません。 一部は、別の部分で宣言されたインターフェイスの実装を提供できます。
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

部分メソッドを除く ([部分メソッド](classes.md#partial-methods))、複数の部分で宣言された型のメンバーのセットは、各部分で宣言されたメンバーのセットの和集合だけです。 型宣言のすべての部分の本文が同じ宣言領域を共有 ([宣言](basic-concepts.md#declarations))、および各メンバーのスコープ ([スコープ](basic-concepts.md#scopes)) に、すべての部分の本文を拡張します。 任意のメンバーのアクセシビリティ ドメインは、外側の型のすべての部分を常に含まれます`private` 1 つの部分で宣言されたメンバーは、別の部分から自由にアクセスします。 そのメンバーが持つ型でない限り、型の 1 つ以上の部分で同じメンバーを宣言すると、コンパイル時エラーは、`partial`修飾子。

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

型内のメンバーの順序が、c# のコードをほとんど重要では、他の言語や環境とやり取りするときに大きな可能性があります。 このような場合は、複数の部分で宣言された型内のメンバーの順序は定義されません。

### <a name="partial-methods"></a>部分メソッド

部分メソッドは、型宣言の 1 つの部分で定義されているし、別に実装できます。 実装は省略可能です。部分メソッドを実装部分がない場合、すべての呼び出しと部分メソッド宣言は、部分の組み合わせから作成される型の宣言から削除されます。

部分メソッドは、アクセス修飾子を定義することはできませんが、暗黙的に`private`します。 戻り値の型である必要があります`void`、そのパラメーターを持つことができません、`out`修飾子。 識別子`partial`直前に表示される場合にのみは、特殊なメソッドの宣言でキーワードとして認識、`void`型。 それ以外の場合、通常の識別子として使用できますが。 部分メソッドは、インターフェイス メソッドを明示的に実装できません。

部分メソッド宣言の 2 つの種類があります。メソッド宣言の本体がセミコロンの場合は、宣言と呼ばれます、***部分メソッドの宣言を定義する***します。 として本文を指定した場合、*ブロック*、宣言はモード、***実装部分メソッド宣言***します。 型宣言の部分があります指定されたシグネチャを持つ部分メソッド宣言を定義する 1 つだけと実装、特定のシグネチャを持つ部分メソッド宣言は 1 つのみです。 部分メソッドの実装宣言が指定された、対応する部分メソッド宣言の定義が存在する必要がありますとして宣言する必要がありますと一致する場合は、次に指定します。

* 宣言は、(が必ずしも同じ順序で)、同じ修飾子を設定する必要があります、メソッド名、型パラメーターの数、およびパラメーターの数。
* 対応するパラメーターの宣言では、(が必ずしも同じ順序で)、同じ修飾子を設定する必要がありますと同じ型 (型パラメーター名の相違) を除く。
* 宣言内の対応する型パラメーター (型パラメーター名の相違) を除くと同じ制約があります。

部分メソッドの実装宣言は、対応する部分メソッドの定義宣言と同じ部分に表示できます。

部分メソッドが定義するだけでは、オーバー ロードの解決に関与します。 したがって、実装宣言が指定されているかどうかを示す invocation 式は、部分メソッドの呼び出しを解決します。 部分メソッドは常に返すため`void`、このような呼び出しの式の式ステートメントは常になります。 さらに、部分メソッドは暗黙的にあるため`private`、このようなステートメントは常に内で部分メソッドを宣言する型宣言の部分の 1 つ発生します。

部分型の宣言の一部に特定の部分メソッドの実装宣言が含まれていない場合、それを呼び出す任意の式ステートメントは結合型の宣言から削除だけです。 したがって、呼び出しなど、式の構成の式には、実行時に影響はありません。 部分メソッド自体も削除され、結合型の宣言のメンバーはできません。

特定の部分メソッドの実装宣言がある場合は、部分メソッドの呼び出しが保持されます。 部分メソッドでは、次を除く部分メソッドの実装宣言のようなメソッドの宣言に上昇。

* `partial`修飾子が含まれていません。
* 結果として得られるメソッドの宣言内の属性は、不特定の順序で、定義と実装の部分メソッド宣言の結合の属性です。 重複は削除されません。
* 結果として得られるメソッドの宣言のパラメーターの属性は、不特定の順序で、定義と実装の部分メソッド宣言の対応するパラメーターの結合の属性です。 重複は削除されません。

部分メソッド M の実装宣言がなく、定義宣言を指定した場合、次の制限が適用されます。

* メソッドにデリゲートを作成すると、コンパイル時エラー ([デリゲート作成式](expressions.md#delegate-creation-expressions))。
* 参照すると、コンパイル時エラー`M`式ツリー型に変換される匿名関数の内側 ([式ツリー型への匿名関数の変換の評価](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types))。
* 式の呼び出しの一部として発生する`M`確実な割り当ての状態には影響しません ([確実な代入](variables.md#definite-assignment))、コンパイル時エラーにすることになります。
* `M` アプリケーションのエントリ ポイントにすることはできません ([アプリケーションの起動](basic-concepts.md#application-startup))。

部分メソッドは、ツールによって生成されるものなど、別の部分の動作をカスタマイズする型宣言の 1 つの部分を許可する場合に便利です。 次の部分クラス宣言を検討してください。
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

他の部分でもないこのクラスがコンパイルされると、部分メソッドの定義宣言とその呼び出しは削除され、結果の結合されたクラス宣言は、次と同等になります。
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

別の部分が与えられている、ただし、部分メソッドの実装宣言を提供するものとします。
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

結果の結合されたクラス宣言は次と同等になります。
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

拡張可能な型の各部分は、同じ名前空間内で宣言する必要があります、パーツは通常、別の名前空間宣言内で書き込まれます。 したがって、異なる`using`ディレクティブ ([ディレクティブを使用して](namespaces.md#using-directives)) の各部分に存在する場合があります。 単純な名前を解釈するときに ([型推論](expressions.md#type-inference)) のみ 1 つのパーツ内で、`using`その一部を囲む名前空間宣言のディレクティブと見なされます。 これにより、さまざまな部分で異なる意味を持つ同じ識別子可能性があります。
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

クラスのメンバーで導入されたメンバーで構成されているその*class_member_declaration*s およびメンバーは、直接基底クラスから継承します。

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

*  クラスに関連付けられている定数の値を表す定数 ([定数](classes.md#constants))。
*  クラスの変数であるフィールド ([フィールド](classes.md#fields))。
*  メソッドで、計算とクラスによって実行できるアクションの実装 ([メソッド](classes.md#methods))。
*  プロパティを定義する特性をという名前と読み取りと書き込みのような特性に関連付けられているアクション ([プロパティ](classes.md#properties))。
*  クラスによって生成される通知を定義するには、イベント ([イベント](classes.md#events))。
*  同じ方法でインデックスを作成するクラスのインスタンスが許可されるインデクサーは、配列として (構文) ([インデクサー](classes.md#indexers))。
*  クラスのインスタンスに適用できる式の演算子の定義の演算子 ([演算子](classes.md#operators))。
*  インスタンス クラスのインスタンスを初期化するために必要なアクションを実装するコンス トラクター ([インスタンス コンス トラクター](classes.md#instance-constructors))
*  デストラクターで、クラスのインスタンスが完全に破棄される前に実行されるアクションを実装 ([デストラクター](classes.md#destructors))。
*  クラス自体を初期化するために必要なアクションを実装する静的コンス トラクター ([静的コンス トラクター](classes.md#static-constructors))。
*  種類は、クラスに対してローカルな型を表す ([入れ子になった型](classes.md#nested-types))。

実行可能コードを含むことのできるメンバーと総称されます、*関数メンバー*のクラス型。 クラス型の関数メンバーは、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンス コンス トラクター、デストラクター、およびそのクラス型の静的コンス トラクターです。

A *class_declaration*新しい宣言領域を作成します ([宣言](basic-concepts.md#declarations))、および*class_member_declaration*ですぐに格納されている、*クラス_declaration*この宣言領域に新しいメンバーを導入します。 次の規則に適用されます*class_member_declaration*: %s

*  インスタンス コンス トラクター、デストラクター、および静的コンス トラクターは、すぐ外側のクラスと同じ名前が必要です。 その他のすべてのメンバーのすぐ外側のクラスの名前とは異なる名前が必要です。
*  定数、フィールド、プロパティ、イベント、または型の名前は、同じクラスで宣言されているその他のすべてのメンバーの名前とは異なる必要があります。
*  メソッドの名前は、他のすべての非-のメソッドの名前、同じクラスで宣言されているとは異なる必要があります。 さらに、署名 ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading)) のメソッドと同じクラスで宣言されている他のすべてのメソッドのシグネチャは異なるし、同じクラスで宣言されている 2 つの方法でのみ異なるシグネチャがない可能性がありますによって`ref`と`out`します。
*  インスタンス コンス トラクターのシグネチャは、同じクラスで宣言されている他のすべてのインスタンス コンス トラクターのシグネチャと異なる必要があり、同じクラスで宣言されている 2 つのコンス トラクターでのみが異なるシグネチャがない可能性があります`ref`と`out`します。
*  インデクサーのシグネチャは、同じクラスで宣言されている他のすべてのインデクサーの署名とは異なる必要があります。
*  演算子のシグネチャは、同じクラスで宣言されているその他のすべての演算子のシグネチャとは異なる必要があります。

クラス型の継承されたメンバー ([継承](classes.md#inheritance)) クラスの宣言領域の一部ではないです。 したがって、派生クラスは (これは有効で、継承されたメンバーを非表示になります)、継承されたメンバーと同じ名前またはシグネチャを持つメンバーを宣言できます。

### <a name="the-instance-type"></a>インスタンスの型

各クラスの宣言が関連付けられたバインドされた型 ([バインドされ、型がバインドされていない](types.md#bound-and-unbound-types))、***インスタンス タイプに***します。 構築された型を作成して、ジェネリック クラス宣言のインスタンスの型の形式が ([構築型](types.md#constructed-types))、型宣言からで指定された型の各引数が、対応する入力パラメーター。 インスタンスの型は、型パラメーターを使用するためにのみ使用できます、型パラメーターがスコープにあります。つまり、クラス宣言内で。 インスタンスの型の型は、`this`クラス宣言内で記述されたコード。 非ジェネリック クラス、インスタンスの型は、宣言されたクラスだけです。 インスタンスの型といくつかのクラス宣言を次に示します。 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>構築された型のメンバー

構築された型の非継承メンバーが、それぞれに置き換えることによって取得した*type_parameter* 、対応するメンバーの宣言で*type_argument*構築された型のです。 切り替え処理は、型の宣言の意味に基づいており、単なるテキストの置換ではありません。

たとえば、ジェネリック クラス宣言について考えます。
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
構築された型`Gen<int[],IComparable<string>>`に次のメンバーがあります。
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

メンバーの型`a`ジェネリック クラス宣言で`Gen`は"の 2 次元配列`T`"ため、メンバーの型`a`"の1次元配列の2次元配列をは、上記の構築された型`int`"、または`int[,][]`します。

インスタンス関数メンバーの種類内`this`インスタンスの型は、([インスタンス型](classes.md#the-instance-type)) 含む宣言のです。

ジェネリック クラスのすべてのメンバーは、直接、または構築された型の一部として、外側のクラスからの型パラメーターを使用できます。 構築された型の特定が閉じられたときに ([オープンとクローズ型](types.md#open-and-closed-types)) が使用される実行時には、型パラメーターの使用は、構築された型に指定された実際の型引数に置き換えられます。 例:
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

クラス***継承***直接基底クラス型のメンバー。 継承では、クラスが暗黙的にインスタンス コンス トラクター、デストラクター、および基底クラスの静的コンス トラクターを除く、直接基底クラス型のすべてのメンバーを格納することを意味します。 継承する場合は、いくつかの重要な側面は次のとおりです。

*  継承は推移的です。 場合`C`から派生`B`、および`B`から派生`A`、し`C`で宣言されたメンバーを継承`B`で宣言されたメンバーと`A`します。
*  派生クラスでは、その直接の基本クラスを拡張します。 派生クラスは、継承するメンバーに新しいメンバーを追加できますが、継承されたメンバーの定義を削除することはできません。
*  インスタンス コンス トラクター、デストラクター、および静的コンス トラクターは継承されませんが、宣言されたアクセシビリティに関係なく、他のすべてのメンバーは、([メンバー アクセス](basic-concepts.md#member-access))。 ただし、によって宣言されたアクセシビリティ、継承されたメンバーができない派生クラスでアクセスできます。
*  派生クラスは***を非表示に***([継承によって非表示](basic-concepts.md#hiding-through-inheritance)) 同じ名前またはシグネチャを持つ新しいメンバーを宣言することで、継承されたメンバー。 そのメンバーは削除継承されたメンバーが隠ぺいされてただしは、単に、そのメンバーは派生クラスから直接アクセスできません。
*  クラスのインスタンスには、一連クラスとその基本クラスでは、暗黙的な変換で宣言されているすべてのインスタンス フィールドにはが含まれています ([暗黙の参照変換](conversions.md#implicit-reference-conversions)) クラスの派生型からその基底クラス型のいずれかに存在します。 したがって、いくつかの派生クラスのインスタンスへの参照は、その基本クラスのいずれかのインスタンスへの参照として処理できます。
*  クラスは、仮想メソッド、プロパティ、およびインデクサーを宣言し、派生クラスは、これらの関数メンバーの実装をオーバーライドできます。 これにより、クラス メンバー関数の呼び出しによって実行されたアクションは、その関数のメンバーが呼び出されるインスタンスの実行時の型によって異なります、ポリモーフィックな動作が発生することができます。

構築済みクラス型の継承されたメンバーは、直接の基本クラス型のメンバー ([基底クラス](classes.md#base-classes))、対応する型の各インスタンスに対して構築された型の型引数を代入することによってこれが見つかった内のパラメーター、 *class_base*仕様。 さらに、これらのメンバーがそれぞれの置換によって変換されます*type_parameter* 、対応するメンバーの宣言で*type_argument*の*class_base*指定。

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

上記の例では、構築された型で`D<int>`非継承メンバーを持つ`public int G(string s)`型引数を代入することによって取得`int`、型パラメーターに対して`T`。 `D<int>` クラス宣言から継承されたメンバーがあります`B`します。 この継承されたメンバーを最初に基本クラスの型を決定することで決定`B<int[]>`の`D<int>`に置き換えることによって`int`の`T`基底クラスの指定で`B<T[]>`します。 次に、型引数として、 `B`、`int[]`を置き換える`U`で`public U F(long index)`、継承されたメンバーを生成`public int[] F(long index)`します。

### <a name="the-new-modifier"></a>New 修飾子

A *class_member_declaration*継承されたメンバーと同じ名前またはシグネチャを持つメンバーを宣言できます。 この場合、派生クラスのメンバーといいます***を非表示に***基底クラスのメンバー。 継承されたメンバーを非表示で、エラーはありませんが、コンパイラ警告を発し。 警告を抑制するには、派生クラスのメンバーの宣言を含めることができます、`new`修飾子を派生メンバーでは、基本メンバーを非表示にするものであるかを示します。 このトピックの説明でさらに[継承による隠ぺい](basic-concepts.md#hiding-through-inheritance)します。

場合、`new`継承されたメンバーを隠ぺいしない宣言に修飾子を含めると、それに対応する警告が表示されます。 削除することによってこの警告が抑制され、`new`修飾子。

### <a name="access-modifiers"></a>アクセス修飾子

A *class_member_declaration*宣言されたアクセシビリティの 5 つの可能な種類のいずれかを持つことができます ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility)): `public`、 `protected internal`、 `protected`、 `internal`、または`private`します。 を除き、`protected internal`を 1 つ以上のアクセス修飾子を指定すると、コンパイル エラーは、の組み合わせ。 ときに、 *class_member_declaration* 、アクセス修飾子を含まない`private`と見なされます。

### <a name="constituent-types"></a>構成要素型

メンバーの宣言で使用される型は、そのメンバーの構成要素の型と呼ばれます。 使用可能な構成要素である型は、定数、フィールド、プロパティ、イベント、またはインデクサーの型、演算子、またはメソッドの戻り値の型およびメソッド、インデクサー、演算子、またはインスタンスのコンス トラクターのパラメーターの型。 メンバーの構成要素の型は、少なくともそのメンバー自体と同程度にアクセスである必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。

### <a name="static-and-instance-members"></a>静的メンバーとインスタンス メンバー

クラスのメンバーは、いずれかの***静的メンバー***または***インスタンス メンバー***します。 一般に、静的メンバーとしてクラス型とオブジェクト (クラス型のインスタンス) に属するものとしてインスタンス メンバーに属していると考えると便利です。

フィールド、メソッド、プロパティ、イベント、演算子、またはコンス トラクターの宣言が含まれています、`static`修飾子は、静的メンバーを宣言します。 さらに、定数または型の宣言には、静的メンバーを暗黙的に宣言します。 静的メンバーには、次の特性があります。

*  静的メンバー`M`で言及されている、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`、`E`含まれる型を表す必要があります`M`します。 コンパイル時エラーには`E`インスタンスを表すとします。
*  静的フィールドは、閉じられた特定のクラス型のすべてのインスタンスで共有する 1 つだけの記憶域の場所を識別します。 閉じられた特定のクラス型のインスタンスの数が作成されては、静的フィールドの 1 つだけコピーします。
*  静的な関数メンバー (メソッド、プロパティ、イベント、演算子、またはコンス トラクター) が特定のインスタンスで動作しないを参照すると、コンパイル時エラー`this`でこのような関数メンバー。

フィールド、メソッド、プロパティ、イベント、インデクサー、コンス トラクターまたはデストラクターの宣言が含まれていない場合、`static`修飾子は、インスタンス メンバーを宣言します。 (インスタンス メンバーと呼ぶことが、非静的メンバーです。)インスタンス メンバーには、次の特性があります。

*  インスタンス メンバーと`M`で言及されている、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`、 `E` を含む型のインスタンスを表す必要があります`M`. バインディング時のエラーには`E`型を表すとします。
*  クラスのすべてのインスタンスには、別のクラスのすべてのインスタンス フィールドのセットが含まれています。
*  インスタンス関数メンバー (メソッド、プロパティ、インデクサー、インスタンス コンス トラクターまたはデストラクター) は、クラスの特定のインスタンスを操作し、としてこのインスタンスにアクセスできる`this`([このアクセス](expressions.md#this-access))。

次の例は、静的にアクセスするためのルールとインスタンス メンバーを示しています。
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

`F`メソッドは、インスタンス関数メンバーの表示を*simple_name* ([簡易名](expressions.md#simple-names)) インスタンスのメンバーと静的メンバーの両方にアクセスするために使用できます。 `G`メソッドは、静的な関数メンバーであるを通じてインスタンス メンバーにアクセスすると、コンパイル時エラーを表示、 *simple_name*します。 `Main`メソッドを示していますが、 *member_access* ([メンバー アクセス](expressions.md#member-access)) インスタンス、インスタンス メンバーにアクセスする必要があります、静的メンバーは、型を使ってアクセスする必要があります。

### <a name="nested-types"></a>入れ子にされた型

クラスまたは構造体宣言内で宣言された型が呼び出される、***入れ子にされた型***します。 コンパイル単位または名前空間内で宣言された型が呼び出される、***に入れ子にされた型***します。

例
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
クラス`B`クラス内で宣言されているため、入れ子にされた型は、 `A`、およびクラス`A`に入れ子にされた型は、コンパイル単位内で宣言されているためです。

#### <a name="fully-qualified-name"></a>完全修飾名

完全修飾名 ([完全修飾名](basic-concepts.md#fully-qualified-names)) 入れ子にされた型は`S.N`場所`S`でどの種類の型の完全修飾名は、`N`は宣言されています。

#### <a name="declared-accessibility"></a>アクセシビリティの宣言

非入れ子にされた型を持つことができます`public`または`internal`アクセシビリティで宣言されていて、`internal`既定でアクセシビリティを宣言します。 入れ子にされた型はさらにそれを含む型は、クラスまたは構造体かどうかに応じて、宣言されたアクセシビリティの 1 つまたは複数の形式を別途、これらの種類の宣言されたアクセシビリティを持つことができます。

*  クラスで宣言されている入れ子にされた型が宣言されたアクセシビリティの 5 つの形式のいずれかを持つことができます (`public`、 `protected internal`、 `protected`、 `internal`、または`private`) と、他のクラス メンバーのように既定値は`private`宣言アクセシビリティ。
*  構造体で宣言されている入れ子にされた型が宣言されたアクセシビリティの 3 つの形式のいずれかを持つことができます (`public`、 `internal`、または`private`) と、他の構造体のメンバーのように既定値は`private`アクセシビリティを宣言します。

例では、
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
プライベートの入れ子になったクラスを宣言して`Node`します。

#### <a name="hiding"></a>非表示

入れ子にされた型の非表示にすることがあります ([名前の隠ぺい](basic-concepts.md#name-hiding)) ベースのメンバー。 `new`修飾子が入れ子にされた型の宣言で許可されるため、非表示を明示的に表すことができます。 例では、
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
入れ子になったクラスを示しています。`M`メソッドを非表示に`M`で定義されている`Base`します。

#### <a name="this-access"></a>このアクセス

入れ子にされた型とそれを含む型にに関して特別なリレーションシップがありません*this_access* ([このアクセス](expressions.md#this-access))。 具体的には、`this`内での入れ子にされた型には使用できません、包含する型のインスタンス メンバーを参照してください。 入れ子にされた型がそれを含む型のインスタンス メンバーへのアクセスを必要がある場合、アクセスを提供することで提供できます、`this`の入れ子にされた型のコンス トラクター引数として含んでいる型のインスタンス。 次の例
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
この方法を示しています。 インスタンス`C`のインスタンスを作成します`Nested`独自渡します`this`に`Nested`のコンス トラクターへの後続のアクセスを提供するために`C`のインスタンス メンバーです。

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>含んでいる型のプライベート、プロテクト メンバーへのアクセス

入れ子にされた型を持つ、包含する型のメンバーを含む、包含する型にアクセスできるメンバーのすべてにアクセスするが`private`と`protected`アクセシビリティを宣言します。 例では、
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
クラスを示しています`C`入れ子になったクラスを格納している`Nested`します。 内で`Nested`、メソッド`G`静的メソッドを呼び出して`F`で定義されている`C`、および`F`宣言されたアクセシビリティをプライベートにします。

入れ子にされた型は、それを含む型の基本型で定義されている保護されたメンバーもアクセスできます。 例
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
入れ子になったクラス`Derived.Nested`プロテクト メソッドにアクセスする`F`で定義されている`Derived`の基本クラス、`Base`によりのインスタンスを介して呼び出す`Derived`します。

#### <a name="nested-types-in-generic-classes"></a>ジェネリック クラスの入れ子にされた型

ジェネリック クラス宣言は、入れ子になった型宣言を含めることができます。 外側のクラスの型パラメーターは、入れ子にされた型で使用できます。 入れ子になった型宣言は、入れ子にされた型にのみ適用される追加の型パラメーターを含めることができます。

ジェネリック クラス宣言内に含まれるすべての型宣言は、暗黙的にジェネリック型の宣言です。 ジェネリック型の中で入れ子になった型への参照を書き込むときに、型引数を含む、コンテナーの構築された型が指定する必要があります。 ただしから外側のクラス内で入れ子にされた型は修飾しないで使用できる;外側のクラスのインスタンスの型は、入れ子にされた型を構築するときに暗黙的に使用できます。 次の例は、作成から構築された型を参照する 3 つの正しい方法を示しています。 `Inner`; 最初の 2 つは等価です。
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

不適切なプログラミング スタイルでは、入れ子にされた型の型パラメーター メンバーを非表示にしたり、外側の型で宣言されたパラメーターを入力します。
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>予約済みのメンバー名

C# 実行時の基本的な実装をプロパティ、イベント、またはインデクサーでは、各ソース メンバー宣言を容易に実装は、メンバーの宣言、名前、およびその型の種類に基づく 2 つのメソッド シグネチャを予約する必要があります。 基になるランタイムの実装を行わない場合でも、これらのいずれかに一致するシグネチャを持つメンバーには、署名が予約されています。 を宣言するプログラムのコンパイル時エラーがこれらの予約を使用します。

予約済みの名前は宣言されていない、したがって、メンバーの参照に参加していません。 ただし、宣言のシグネチャが継承に参加して予約済みのメソッドの関連 ([継承](classes.md#inheritance)) と非表示にできると、`new`修飾子 ([new 修飾子](classes.md#the-new-modifier))。

これらの名前の予約には、次の 3 つの目的があります。

*  Get のメソッド名として通常の識別子を使用するか、c# 言語機能にアクセスを設定するには、基になる実装をできるようにします。
*  その他の言語の get メソッド名として通常の識別子を使用して相互運用または c# 言語機能にアクセスを設定できるようにします。
*  ソースが 1 つの準拠コンパイラによって受け入れられるようにがによって許可される、ことで予約されているメンバーの仕様名一貫性のあるすべての c# 実装でします。

デストラクターの宣言 ([デストラクター](classes.md#destructors)) もシグネチャを予約できると、([デストラクター予約されているメンバーの名前](classes.md#member-names-reserved-for-destructors))。

#### <a name="member-names-reserved-for-properties"></a>プロパティ用に予約されているメンバー名

プロパティの`P`([プロパティ](classes.md#properties)) 型の`T`、次のシグネチャは予約されています。

```csharp
T get_P();
void set_P(T value);
```

プロパティが読み取り専用または書き込み専用の場合でも、両方の署名が予約されています。

例
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
クラス`A`読み取り専用プロパティを定義する`P`、シグネチャが予約`get_P`と`set_P`メソッド。 クラス`B`から派生した`A`両方のこれらの予約済みの署名を非表示にします。 この例では、出力が生成されます。
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>イベント用に予約されているメンバー名

イベントの`E`([イベント](classes.md#events)) デリゲート型の`T`、次のシグネチャは予約されています。
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>インデクサー用に予約されているメンバー名

インデクサーの ([インデクサー](classes.md#indexers)) 型の`T`パラメーター リストで`L`、次のシグネチャは予約されています。
```csharp
T get_Item(L);
void set_Item(L, T value);
```

インデクサーは読み取り専用または書き込み専用で場合でも、両方の署名が予約されています。

さらに、メンバー名`Item`は予約されています。

#### <a name="member-names-reserved-for-destructors"></a>デストラクターの予約済みのメンバー名

デストラクターを含むクラスの ([デストラクター](classes.md#destructors))、次のシグネチャは予約されています。
```csharp
void Finalize();
```

## <a name="constants"></a>定数

A***定数***定数値を表すクラスのメンバーである: コンパイル時に計算できる値。 A *constant_declaration*指定された型の 1 つまたは複数の定数が導入されています。

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

A *constant_declaration*のセットを含めることができます*属性*([属性](attributes.md))、`new`修飾子 ([new 修飾子](classes.md#the-new-modifier))、有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))。 属性と修飾子で宣言されたメンバーのすべてに適用されます、 *constant_declaration*します。 定数は、静的メンバーと見なされる場合でも、 *constant_declaration*が必要し、は、どちらも、`static`修飾子。 同じ修飾子を定数宣言内で複数回のエラーになります。

*型*の*constant_declaration*宣言によって導入されるメンバーの種類を指定します。 型がのリストが続く*constant_declarator*s、新しいメンバーは、それぞれが導入されています。 A *constant_declarator*から成る、*識別子*後に、メンバーの名前を示す、"`=`"トークン、その後に、 *constant_expression* ([定数式](expressions.md#constant-expressions)) のメンバーの値を提供します。

*型*定数で指定された宣言である必要があります`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、`float`、 `double`、 `decimal`、 `bool`、 `string`、 *enum_type*、または*reference_type*します。 各*constant_expression*対象の型の場合、または暗黙的な変換によって対象の型に変換できる型の値を生成する必要があります ([暗黙的な変換](conversions.md#implicit-conversions))。

*型*定数は、少なくとも定数自体と同程度にアクセスする必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。

定数の値が使用する式で取得した、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバー アクセス](expressions.md#member-access))。

定数できます自体の一部を*constant_expression*します。 したがってが必要な構成要素で定数を使用すること、 *constant_expression*します。 このような構造の例として、`case`ラベル、`goto case`ステートメント、`enum`メンバーの宣言、属性、およびその他の定数の宣言。

」の説明に従って[定数式](expressions.md#constant-expressions)、 *constant_expression*コンパイル時に完全に評価される式を指定します。 以降の null 以外の値を作成する唯一の方法、 *reference_type*以外`string`が適用される、`new`演算子、しているので、`new`で演算子は使用できません、 *constant_式*の定数にのみ指定できる値*reference_type*以外 s`string`は`null`します。

定数値のシンボル名が必要なときにその値の型は、定数宣言で許可されていませんが、またはによってコンパイル時に値を計算できない場合、 *constant_expression*、 `readonly` (フィールド[読み取り専用フィールド](classes.md#readonly-fields)) 代わりに使用される可能性があります。

複数の定数を宣言する定数宣言では、属性、修飾子、および型の単一の定数の複数の宣言と同じです。 次に例を示します。
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

定数は、循環依存関係がない限り、同じプログラム内の他の定数に依存して許可されます。 コンパイラは、適切な順序で定数の宣言を評価する自動的に配置します。 例
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
コンパイラは最初に評価`A.Y`を評価し、 `B.Z`、最後に評価および`A.X`、値を生成`10`、`11`と`12`します。 定数宣言可能性があります、他のプログラムから定数によって異なりますが、このような依存関係は一方向にのみです。 場合に、上記の例を参照する`A`と`B`宣言されている別のプログラムは可能性があります`A.X`に依存する`B.Z`が`B.Z`に同時にではなく依存でしたし`A.Y`します。

## <a name="fields"></a>フィールド

A***フィールド***オブジェクトまたはクラスに関連付けられている変数を表すメンバーします。 A *field_declaration*指定された型の 1 つまたは複数のフィールドが導入されています。

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

A *field_declaration*のセットを含めることができます*属性*([属性](attributes.md))、`new`修飾子 ([new 修飾子](classes.md#the-new-modifier))、有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、および`static`修飾子 ([静的およびインスタンス フィールド](classes.md#static-and-instance-fields))。 さらに、 *field_declaration*含めることができます、`readonly`修飾子 ([読み取り専用フィールド](classes.md#readonly-fields)) または`volatile`修飾子 ([Volatile フィールド](classes.md#volatile-fields)) 両方ではないです。 属性と修飾子で宣言されたメンバーのすべてに適用されます、 *field_declaration*します。 同じ修飾子をフィールド宣言内で複数回のエラーになります。

*型*の*field_declaration*宣言によって導入されるメンバーの種類を指定します。 型がのリストが続く*variable_declarator*s、新しいメンバーは、それぞれが導入されています。 A *variable_declarator*から成る、*識別子*必要に応じて、そのメンバーの名前を示す、"`=`"トークンと*variable_initializer* ([変数の初期化子](classes.md#variable-initializers)) により、そのメンバーの初期値。

*型*フィールドは少なくともフィールド自体と同程度にアクセスできる必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。

フィールドの値が使用する式で取得した、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバー アクセス](expressions.md#member-access))。 使用して読み取り専用以外のフィールドの値を変更、*割り当て*([代入演算子](expressions.md#assignment-operators))。 読み取り専用以外のフィールドの値が取得および変更を使用して後置インクリメントとデクリメント演算子を指定できます ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)) とは、前置インクリメントとデクリメント演算子 ([プレフィックスインクリメントおよびデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。

複数のフィールドを宣言するフィールドの宣言は、属性、修飾子、および型の単一のフィールドの複数の宣言と同じです。 次に例を示します。
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

### <a name="static-and-instance-fields"></a>静的およびインスタンスのフィールド

フィールド宣言が含まれています、`static`修飾子、宣言によって導入されるフィールドは***静的フィールド***します。 ない場合`static`修飾子が存在する場合、フィールド、宣言によって導入される***インスタンス フィールド***します。 静的フィールドおよびインスタンス フィールドは、いくつかの種類の変数の 2 つ ([変数](variables.md)) で c# の場合は、サポートされているし、も参照されているとして***静的変数***と***インスタンス変数***、それぞれします。

静的フィールドは、特定のインスタンスの一部ではありません。代わりに、クローズ型のすべてのインスタンス間で共有されます ([オープンとクローズ型](types.md#open-and-closed-types))。 閉じられたクラス型のインスタンスの数が作成されて、関連付けられているアプリケーション ドメインを静的フィールドの 1 つだけのコピーがあります。

例:
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

インスタンス フィールドは、インスタンスに属しています。 具体的には、クラスのすべてのインスタンスには、そのクラスのすべてのインスタンス フィールドの個別のセットが含まれています。

フィールドが参照されている場合、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`場合は、`M`静的フィールド、`E`含まれる型を表す必要があります`M`、場合`M`インスタンス フィールドの場合は、電子メールに含まれる型のインスタンスを表す必要があります`M`します。

静的な違いは、インスタンス メンバーの説明とでさらに[静的メンバーとインスタンス メンバー](classes.md#static-and-instance-members)。

### <a name="readonly-fields"></a>読み取り専用フィールド

ときに、 *field_declaration*が含まれています、`readonly`修飾子、宣言によって導入されるフィールドは***読み取り専用フィールド***します。 読み取り専用フィールドへの直接代入のみで、インスタンス コンス トラクターまたは同じクラスの静的コンス トラクターまたは宣言の一部として発生します。 (読み取り専用フィールドはで割り当てられるを複数回これらのコンテキスト。)具体的には、直接割り当てを`readonly`フィールドは、次のコンテキストでのみ許可されます。

*  *Variable_declarator*フィールドを導入する (を含めることによって、 *variable_initializer*宣言内)。
*  インスタンス フィールドのフィールド宣言を含むクラスのインスタンス コンス トラクターで、静的フィールド、フィールド宣言を含むクラスの静的コンス トラクター内。 これらは、渡すコンテキストのみでも、`readonly`フィールドとして、`out`または`ref`パラメーター。

割り当てる、`readonly`フィールドまたはとして渡す、`out`または`ref`他のコンテキストでのパラメーターは、コンパイル時エラー。

#### <a name="using-static-readonly-fields-for-constants"></a>静的読み取り専用フィールドを使用して、定数

A`static readonly`フィールドが便利な定数値のシンボル名が必要な場合、値の型では使用できません、`const`宣言、コンパイル時に値を計算できない場合またはします。 例
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
`Black`、 `White`、 `Red`、 `Green`、および`Blue`メンバーとして宣言できません`const`メンバーのため、コンパイル時にその値を計算することはできません。 ただし、宣言する`static readonly`ほぼ同じ結果が代わりにします。

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>定数と静的読み取り専用フィールドのバージョン管理

定数と読み取り専用フィールドは、異なるバイナリのバージョン管理のセマンティクスを持ちます。 コンパイル時に、定数の値を取得する式は、定数を参照する場合が、式は、読み取り専用フィールドを参照する場合、フィールドの値は実行時まで取得されません。 2 つのプログラムで構成されるアプリケーションを検討してください。
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

`Program1`と`Program2`名前空間が個別にコンパイルされる 2 つのプログラムを示します。 `Program1.Utils.X`によって値の出力は、静的読み取り専用フィールドとして宣言されている、`Console.WriteLine`ステートメントがコンパイル時に、不明はではなく、実行時に取得されます。 そのため場合の値`X`が変更されたと`Program1`が再コンパイルされる、`Console.WriteLine`ステートメントの出力は、新しい値いて`Program2`再コンパイルはありません。 ただし、 `X` 、定数の値`X`時に取得`Program2`がコンパイルされるとでの変更による影響を受けるに残る`Program1`まで`Program2`が再コンパイルされます。

### <a name="volatile-fields"></a>Volatile フィールド

ときに、 *field_declaration*が含まれています、`volatile`修飾子、宣言によって導入されるフィールドは***volatile フィールド***します。

非 volatile フィールドの場合は、命令を並べ替える最適化手法をによって提供される同期しないフィールドにアクセスするマルチ スレッド プログラムで、予期しない結果を生じることができます、 *lock_statement* ([Lock ステートメント](statements.md#the-lock-statement))。 コンパイラによって、実行時のシステムまたはハードウェアは、これらの最適化を実行できます。 Volatile フィールドは、このような並べ替えの最適化が制限されます。

*  Volatile フィールドの読み取りと呼ばれる、***の volatile 読み取り***します。 揮発性の読み取りが「セマンティクスを取得します。」つまり、メモリへの参照を命令シーケンスで、その後に発生する前に発生するが保証されます。
*  Volatile フィールドの書き込みと呼ばれる、 ***volatile 書き込み***します。 揮発性の書き込みが「セマンティクスをリリースします。」つまり、命令シーケンスで書き込み命令の前にメモリ参照の後に発生するが保証されます。

これらの制限により、すべてのスレッドが、他のスレッドによって実行された volatile の書き込みを、実行された順序で観察することが保証されます。 準拠した実装は、実行のすべてのスレッドから参照できる volatile 書き込みの順序付け単一合計を提供する必要はありません。 Volatile フィールドの種類は、次のいずれかである必要があります。

*  A *reference_type*します。
*  型`byte`、 `sbyte`、 `short`、 `ushort`、 `int`、 `uint`、 `char`、 `float`、 `bool`、 `System.IntPtr`、または`System.UIntPtr`します。
*  *Enum_type*の基本型を列挙型を持つ`byte`、 `sbyte`、 `short`、 `ushort`、 `int`、または`uint`します。

例では、
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
```
result = 143
```

この例では、メソッドで`Main`メソッドを実行する新しいスレッドが開始`Thread2`します。 このメソッドは、非揮発性という名前のフィールドに値を格納`result`、し格納`true`volatile フィールドで`finished`します。 フィールドのメイン スレッドが待機する`finished`に設定する`true`、フィールドを読み取ります`result`します。 `finished`は宣言されています`volatile`、メイン スレッドが値を読み取る必要があります`143`フィールドから`result`します。 場合、フィールド`finished`宣言されなかった`volatile`にストアの許容されることになります`result`に格納した後、メイン スレッドに表示される`finished`、ため、値を読み取るメイン スレッドの`0`から、フィールド`result`します。 宣言する`finished`として、`volatile`フィールドは、このような不整合を防ぎます。

### <a name="field-initialization"></a>フィールドの初期化

フィールドの初期値は静的フィールドまたはインスタンス フィールドであるかどうか、既定値 ([既定値](variables.md#default-values)) のフィールドの型。 この既定の初期化が発生し、フィールドはありません「初期化されていない」したがって前に、フィールドの値を観察することはできません。 例では、
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
```
b = False, i = 0
```
`b`と`i`はどちらも自動的に既定値に初期化します。

### <a name="variable-initializers"></a>変数の初期化子

フィールドの宣言を含めることができます*variable_initializer*秒。 静的フィールドは、変数の初期化子はクラスの初期化中に実行された代入ステートメントに対応します。 インスタンス フィールド、変数初期化子に対応クラスのインスタンスが作成されるときに実行されるステートメントを代入します。

例では、
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
```
x = 1.4142135623731, i = 100, s = Hello
```
への代入`x`が発生した静的フィールド初期化子の実行と、割り当てを`i`と`s`インスタンス フィールド初期化子の実行時に発生します。

説明されている既定値の初期化[フィールドの初期化](classes.md#field-initialization)変数初期化子を持つフィールドを含め、すべてのフィールドに発生します。 したがって、クラスが初期化されると、そのクラス内のすべての静的フィールドは、まず、既定値に初期化し、静的フィールド初期化子は、テキストの順序で実行されます。 同様に、クラスのインスタンスが作成されると、そのインスタンス内のすべてのインスタンス フィールドは、まず、既定値に初期化し、インスタンス フィールド初期化子がテキストの順序で実行します。

既定値の状態で見られる変数初期化子を持つ静的フィールドのことができます。 ただし、これは、スタイルの問題として強くお勧めします。 例では、
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
この現象が発生します。 循環定義に関係なく、b、プログラムが無効です。 出力になります
```
a = 1, b = 2
```
ため、静的フィールド`a`と`b`に初期化されます`0`(の既定値`int`)、初期化子が実行される前にします。 ときに初期化子`a`の値を実行します。 `b` 0 の場合は、ので`a`に初期化されます`1`します。 ときに初期化子`b`の値を実行します。`a`は既に`1`、ので`b`に初期化されます`2`します。

#### <a name="static-field-initialization"></a>静的フィールドの初期化

クラスの静的フィールドの変数初期化子は、一連のクラス宣言に表示されるテキストの順序で実行される割り当てに対応します。 場合、静的コンス トラクター ([静的コンス トラクター](classes.md#static-constructors)) が存在する静的フィールド初期化子の実行をクラスでは、その静的コンス トラクターを実行する前に、すぐに発生します。 それ以外の場合、静的フィールド初期化子は、そのクラスの静的フィールドの最初に使用する前に、実装に依存する時に実行されます。 例では、
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
いずれか、出力を生成する可能性があります。
```
Init A
Init B
1 1
```
または、出力:
```
Init B
Init A
1 1
```
の実行`X`の初期化子と`Y`の初期化子は、任意の順序で発生する可能性があります。 これらのフィールドへの参照が発生する限定されます。 ただしでは、例を示します。
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
出力があります。
```
Init B
Init A
1 1
```
ため、静的コンス トラクターが実行する場合の規則 (で定義されている[静的コンス トラクター](classes.md#static-constructors)) を提供`B`の静的コンス トラクター (したがって`B`の静的フィールド初期化子)の前に実行する必要があります`A`の静的コンス トラクター、フィールド初期化子。

#### <a name="instance-field-initialization"></a>インスタンス フィールドの初期化

クラスのインスタンス フィールド変数初期化子の割り当て時にインスタンス コンス トラクターのいずれかのエントリにすぐに実行されるシーケンスに対応 ([コンス トラクター初期化子](classes.md#constructor-initializers)) そのクラスの。 変数の初期化子は、クラス宣言に表示されるテキストの順序で実行されます。 クラスのインスタンスの作成および初期化プロセスの詳細については[インスタンス コンス トラクター](classes.md#instance-constructors)します。

インスタンス フィールドの変数の初期化子は、作成中のインスタンスを参照できません。 したがって、参照すると、コンパイル時エラーが`this`変数の初期化子ではによる任意のインスタンス メンバーを参照する変数の初期化子のコンパイル時エラー、 *simple_name*します。 例
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
変数の初期化子`y`作成中のインスタンスのメンバーを参照しているため、コンパイル時エラーが発生します。

## <a name="methods"></a>メソッド

"***メソッド***" は、オブジェクトまたはクラスによって実行可能な計算またはアクションを実装するメンバーです。 メソッドを使用して宣言*method_declaration*: %s

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

A *method_declaration*のセットを含めることができます*属性*([属性](attributes.md)) と有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、 `new` ([new 修飾子](classes.md#the-new-modifier))、 `static` ([静的およびインスタンス メソッド](classes.md#static-and-instance-methods))、 `virtual` ([仮想メソッド](classes.md#virtual-methods))、 `override` ([メソッドをオーバーライド](classes.md#override-methods))、 `sealed` ([メソッドをシール](classes.md#sealed-methods))、 `abstract` ([抽象メソッド](classes.md#abstract-methods))、および`extern` ([外部メソッド](classes.md#external-methods)) 修飾子。

宣言には、有効な修飾子の組み合わせは、次のすべてに該当する場合。

*  宣言には、有効なアクセス修飾子の組み合わせが含まれています ([アクセス修飾子](classes.md#access-modifiers))。
*  宣言は含まれません同じ修飾子は、複数回。
*  宣言では、次の修飾子の 1 つだけ含まれています: `static`、 `virtual`、および`override`します。
*  宣言では、次の修飾子の 1 つだけ含まれています:`new`と`override`します。
*  宣言が含まれている場合、`abstract`修飾子、宣言は次の修飾子のいずれかに含まれません: `static`、 `virtual`、`sealed`または`extern`します。
*  宣言が含まれている場合、`private`修飾子、宣言は次の修飾子のいずれかに含まれません: `virtual`、 `override`、または`abstract`します。
*  宣言が含まれている場合、`sealed`修飾子、宣言も含まれています、`override`修飾子。
*  宣言が含まれている場合、`partial`修飾子は、その後は、次の修飾子のいずれかに含まれません: `new`、 `public`、 `protected`、 `internal`、 `private`、 `virtual`、 `sealed`、 `override`、 `abstract`、または`extern`します。

メソッドを持つ、`async`修飾子は、非同期関数でありで説明されている規則に従って[非同期関数](classes.md#async-functions)します。

*Return_type*メソッドの宣言が計算され、メソッドによって返される値の型を指定します。 *Return_type*は`void`メソッドが値を返さない場合。 宣言が含まれている場合、`partial`修飾子は、その戻り値の型である必要があります`void`します。

*Member_name*メソッドの名前を指定します。 メソッドが明示的なインターフェイス メンバーの実装でない限り ([明示的なインターフェイス メンバーの実装](interfaces.md#explicit-interface-member-implementations))、 *member_name*は単に、*識別子*します。 明示的なインターフェイス メンバーの実装、 *member_name*から成る、 *interface_type*後に、"`.`"と*識別子*します。

省略可能な*type_parameter_list*メソッドの型パラメーターを指定します ([パラメーター入力](classes.md#type-parameters))。 場合、 *type_parameter_list*メソッドが指定されて、***ジェネリック メソッド***します。 メソッドの場合、`extern`修飾子、 *type_parameter_list*は指定できません。

省略可能な*formal_parameter_list*メソッドのパラメーターを指定します ([メソッド パラメーター](classes.md#method-parameters))。

省略可能な*type_parameter_constraints_clause*は個々 の型パラメーターの制約を指定する ([パラメーターの制約入力](classes.md#type-parameter-constraints)) 場合にのみ指定可能性があります、 *type_parameter_リスト*が指定されても、メソッドがないと、`override`修飾子。

*Return_type*および各で参照される型の*formal_parameter_list*メソッドは少なくともメソッド自体と同程度にアクセスできる必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints)).

*Method_body*がいずれかをセミコロン、***ステートメント本体***または***式本体***します。 ステートメント本体から成る、*ブロック*メソッドが呼び出されたときに実行するステートメントを指定します。 式の本体から成る`=>`続けて、*式*およびセミコロンが、メソッドが呼び出されたときに実行する 1 つの式を表します。 

`abstract`と`extern`、メソッド、 *method_body*セミコロンだけで構成されます。 `partial`メソッド、 *method_body*のセミコロン、ブロックの本体または式の本体のいずれかで構成されている可能性があります。 他のすべてのメソッドに対して、 *method_body*はブロック本体または式の本体。

場合、 *method_body* 、宣言が含まれていない可能性がありますし、セミコロンから成る、`async`修飾子。

名前、型パラメーター リストおよびメソッドの仮パラメーター リストの定義、署名 ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading)) メソッドの。 具体的には、メソッドのシグネチャは、その名前、型パラメーターと数、修飾子、およびその仮パラメーターの型の数で構成されます。 このため、その名前ではなく、メソッドの型引数リストの序数位置を使用して、仮パラメーターの型で使用されるメソッドの型パラメーターが識別されます。戻り値の型はメソッドのシグネチャの一部でないでも、型パラメーターまたは仮パラメーターの名前です。

メソッドの名前は、他のすべての非-のメソッドの名前、同じクラスで宣言されているとは異なる必要があります。 さらに、メソッドのシグネチャは、同じクラスで宣言されている他のすべてのメソッドのシグネチャと異なる必要があり、同じクラスで宣言されている 2 つの方法でのみが異なるシグネチャがない可能性があります`ref`と`out`します。

メソッドの*type_parameter*全体のスコープでは、 *method_declaration*でそのスコープ全体でフォームの種類に使用できると*return_type*、 *method_body*、および*type_parameter_constraints_clause*s ではなく*属性*します。

すべての仮パラメーターと型パラメーターには、異なる名前がある場合があります。

### <a name="method-parameters"></a>メソッドのパラメーター

メソッドのパラメーターは、存在する場合、メソッドの宣言されます*formal_parameter_list*します。

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

1 つまたは複数コンマ区切りのパラメーターのうち、最後にのみがあります、仮パラメーター リストで構成されます、*括ら*します。

A *fixed_parameter*オプションのセットから成る*属性*([属性](attributes.md))、省略可能な`ref`、`out`または`this`修飾子、*型*、*識別子*と省略可能な*default_argument*します。 各*fixed_parameter*指定した名前で指定された型のパラメーターを宣言します。 `this`修飾子は、拡張メソッドとしてメソッドを指定しは静的メソッドの最初のパラメーターでのみ使用できます。 拡張メソッドはさらに「[拡張メソッド](classes.md#extension-methods)します。

A *fixed_parameter*で、 *default_argument*と呼ばれますが、***省略可能なパラメーター***であるのに対し、 *fixed_parameter* なし*default_argument*は、***必須パラメーター***します。 省略可能なパラメーターの後に必要なパラメーターが表示されない、 *formal_parameter_list*します。

A`ref`または`out`パラメーターを含めることはできません、 *default_argument*します。 *式*で、 *default_argument*次のいずれかを指定する必要があります。

*  *constant_expression*
*  式形式の`new S()`場所`S`は値型です。
*  式形式の`default(S)`場所`S`は値型です。

*式*id 列またはパラメーターの型を null 許容型の変換によって暗黙的に変換できる必要があります。

部分メソッドの実装宣言で省略可能なパラメーターが発生するかどうか ([部分メソッド](classes.md#partial-methods))、明示的なインターフェイス メンバーの実装 ([明示的なインターフェイス メンバーの実装](interfaces.md#explicit-interface-member-implementations)) または、パラメーターを 1 つのインデクサーの宣言 ([インデクサー](classes.md#indexers)) これらのメンバーは、引数を省略を許可する方法では呼び出されませんので、コンパイラは警告を表示する必要があります。

A*括ら*オプションのセットから成る*属性*([属性](attributes.md))、`params`修飾子、 *array_type*、および*識別子*します。 パラメーター配列は、指定した名前で指定された配列型の 1 つのパラメーターを宣言します。 *Array_type*配列パラメーターの 1 次元配列型である必要があります ([配列型](arrays.md#array-types))。 メソッドの呼び出しでは、パラメーター配列を指定するには、指定された配列型の 1 つの引数を許可するか、0 個以上の引数を指定する配列要素型のようになります。 パラメーター配列の詳細については[パラメーター配列](classes.md#parameter-arrays)します。

A*括ら*、オプションのパラメーターの後に発生する可能性がありますが、既定値は--の引数の省略を含めることはできません、*括ら*空の配列の作成時に代わりになります。

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

*Formal_parameter_list*の`M`、`i`必要な ref パラメーターでは、`d`が必要な値を持つパラメーター、 `b`、 `s`、`o`と`t`パラメーターが省略可能な値と`a`はパラメーター配列です。

メソッドの宣言では、パラメーター、型パラメーターとローカル変数を別の宣言領域を作成します。 ローカル変数宣言と型パラメーター リストと、メソッドの仮パラメーター リストで、この宣言領域に名前が導入された、*ブロック*メソッドの。 メソッドの宣言領域に同じ名前の 2 つのメンバーのエラーになります。 メソッドの宣言領域と同じ名前の要素を格納する入れ子になった宣言領域のローカル変数の宣言領域のエラーになります。

メソッドの呼び出し ([メソッドの呼び出し](expressions.md#method-invocations)) その呼び出しに固有のコピーが作成、仮パラメーターと、メソッドのローカル変数と呼び出しの引数リストの値または変数の参照に割り当てられます、仮パラメーターを新しく作成します。 内で、*ブロック*でそれらの識別子で、メソッドの仮パラメーターを参照できる*simple_name*式 ([簡易名](expressions.md#simple-names))。

仮パラメーターの 4 つの種類があります。

*  値パラメーターは、修飾子なしで宣言されます。
*  宣言されるパラメーターを参照して、`ref`修飾子。
*  宣言される出力パラメーター、`out`修飾子。
*  宣言されるパラメーター配列、`params`修飾子。

」の説明に従って[シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading)、`ref`と`out`修飾子はメソッドのシグネチャの一部が、`params`修飾子はありません。

#### <a name="value-parameters"></a>値パラメーター

修飾子なしで宣言されたパラメーターは、値パラメーターです。 値を持つパラメーターは、メソッドの呼び出しで指定された対応する引数からその初期値を取得するローカル変数に対応します。

メソッドの呼び出しで、対応する引数が暗黙的に変換できる式を指定する必要があります仮パラメーターに値を持つパラメーターがある場合 ([暗黙的な変換](conversions.md#implicit-conversions)) 仮パラメーターの型にします。

値を持つパラメーターに新しい値を割り当てるメソッドが許可されます。 このような割り当て、value パラメーターで表されるローカル ストレージの場所にのみ影響: メソッドの呼び出しで指定された実際の引数には影響があるありません。

#### <a name="reference-parameters"></a>参照パラメーター

宣言したパラメーターを`ref`修飾子は、参照パラメーター。 値パラメーターの場合とは異なり、参照パラメーターは、新しい記憶域の場所を作成できません。 代わりに、参照パラメーターは、メソッドの呼び出しで引数として指定された変数と同じストレージ場所を表します。

メソッドの呼び出しで対応する引数がキーワードで構成される必要があります仮パラメーターは、参照パラメーターが、`ref`続けて、 *variable_reference* ([を決定するための厳密な規則確実な代入](variables.md#precise-rules-for-determining-definite-assignment)) の仮パラメーターと同じ型。 参照パラメーターとして渡す前に、変数を間違いなく割り当てる必要があります。

メソッド内の参照パラメーターは常と見なされます明示的に代入します。

反復子として宣言されたメソッド ([反復子](classes.md#iterators)) 参照パラメーターを含めることはできません。

例では、
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
```
i = 2, j = 1
```

呼び出しの`Swap`で`Main`、`x`表します`i`と`y`表します`j`。 このため、呼び出しは、の値を交換`i`と`j`します。

メソッドでは複数の名前が同じ記憶域の場所を表すことが参照パラメーターを受け取る。 例
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
呼び出し`F`で`G`への参照を渡す`s`両方の`a`と`b`します。 したがって、その呼び出しでは、名前の`s`、 `a`、および`b`すべて、同じストレージの場所を参照し、すべての 3 つの割り当ては、インスタンス フィールドを変更`s`します。

#### <a name="output-parameters"></a>出力パラメーター

宣言したパラメーター、`out`修飾子は、出力パラメーター。 参照パラメーターと同様に、出力パラメーターには新しい記憶域の場所は作成できません。 代わりに、出力パラメーターは、メソッドの呼び出しで引数として指定された変数と同じストレージ場所を表します。

メソッドの呼び出しで対応する引数がキーワードで構成される必要があります仮パラメーターが出力パラメーターの場合は、`out`続けて、 *variable_reference* ([を決定するための厳密な規則確実な代入](variables.md#precise-rules-for-determining-definite-assignment)) の仮パラメーターと同じ型。 変数必要がありますいないに明示的に代入、出力パラメーターとして渡すことが、次の変数が出力パラメーターとして渡された呼び出し、変数と見なされます明示的に代入します。

メソッド内と同じように、ローカル変数、出力パラメーターは最初と見なされる未割り当ての値を使用する前に間違いなく割り当てる必要があります。

メソッドが戻る前に、メソッドのすべての出力パラメーターを間違いなく割り当てる必要があります。

部分メソッドとして宣言されたメソッド ([部分メソッド](classes.md#partial-methods)) または反復子 ([反復子](classes.md#iterators)) パラメーターの出力があることはできません。

出力パラメーターは、複数の戻り値を生成するメソッドで通常使用されます。 例:
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

この例では、出力が生成されます。
```
c:\Windows\System\
hello.txt
```

なお、`dir`と`name`に渡す前に変数が割り当て済みにすることができます`SplitPath`、いると見なされる次の呼び出しが明示的に代入するとします。

#### <a name="parameter-arrays"></a>パラメーター配列

宣言したパラメーターを`params`修飾子は、パラメーター配列。 仮パラメーター リストには、パラメーター配列が含まれている場合、一覧の最後のパラメーターがあり、1 次元配列型でなければなりません。 たとえば、型`string[]`と`string[][]`パラメーター配列の型が、型として使用できる`string[,]`ことはできません。 結合することはできません、`params`修飾子を使用して修飾子`ref`と`out`します。

パラメーター配列は、メソッドの呼び出しで 2 つの方法のいずれかで指定する引数を許可します。

*  引数の指定されたパラメーター配列は、暗黙的に変換する 1 つの式を指定できます ([暗黙的な変換](conversions.md#implicit-conversions)) に、パラメーター配列の型。 この場合、パラメーター配列は、値を持つパラメーターとまったく同じように機能します。
*  または、呼び出しはそれぞれの引数が、暗黙的に変換する式は、パラメーター配列の 0 個以上の引数を指定できます ([暗黙的な変換](conversions.md#implicit-conversions)) パラメーターの配列の要素の型にします。 ここでは、呼び出し引数の数に対応する長さを持つパラメーターの配列型のインスタンスを作成します。 指定された引数に値を使用して、配列インスタンスの要素を初期化し、実際に新しく作成された配列のインスタンスを使用引数。

を除き、呼び出しの可変個の引数を許可すると、パラメーター配列に値を持つパラメーターとまったく同じです ([パラメーターの値](classes.md#value-parameters)) 同じ型。

例では、
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
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

最初の呼び出し`F`配列を渡すだけ`a`値パラメーターとして。 2 番目の呼び出しの`F`4 つの要素が自動的に作成`int[]`の特定の要素の値と配列のインスタンスの値を持つパラメーターとして渡します。 3 つ目の呼び出しでは同様に、 `F` 0 要素を作成します`int[]`値パラメーターとしてそのインスタンスを渡します。 2 番目と 3 番目の呼び出しは、書き込みを正確に同じです。
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

パラメーター配列を持つメソッドを標準形式で、または拡張形式では、適用可能なオーバー ロードの解決を実行するときに ([適用可能な関数メンバー](expressions.md#applicable-function-member))。 メソッドの拡張の形式は、通常、メソッドの形式が適用されない場合にのみ、および拡張形式と同じシグネチャを持つ、該当するメソッドが同じ型で既に宣言されていない場合にのみ使用できます。

例では、
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
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

例では、パラメーター配列を持つメソッドの拡張可能な形式の 2 つ既に含まれているクラスの通常のメソッドとして。 そのため、これらの拡張形式はオーバー ロードの解決を実行するときに考慮されませんし、最初と 3 番目のメソッドの呼び出しでは、通常のメソッドが選択します。 クラスは、パラメーター配列を持つメソッドを宣言して、それはも通常のメソッドとして展開されたフォームの一部を含めるには珍しくありません。 により、配列の割り当てを回避することは、パラメーター配列を持つメソッドの拡張の形式のときに発生するインスタンスが呼び出されます。

パラメーター配列の型の場合は`object[]`、通常、メソッドの形式と単一の拡張形式の間で発生する潜在的なあいまいさ`object`パラメーター。 あいまいさの理由は、`object[]`型に暗黙的に変換自体が`object`します。 あいまいさも問題はありません、ただし、必要な場合は、キャストを挿入することで解決できるためです。

例では、
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
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

最初と最後の呼び出しで`F`、通常の形式`F`はパラメーターの型に引数の型からの暗黙的な変換が存在するために適用 (型の両方が`object[]`)。 したがって、オーバー ロードの解決は通常の形式を選択します。 `F`、通常の値のパラメーターとして渡される引数、と。 2 番目と 3 番目の呼び出し、通常の形式で`F`暗黙的な変換が存在しないため、引数の型からパラメーターの型には適用できません (型`object`型に暗黙的に変換できない`object[]`)。 ただし、拡張されたフォームの`F`はオーバー ロードの解決で選択されているので、該当します。 その結果、1 つの要素`object[]`、呼び出しによって作成し、配列の 1 つの要素が指定された引数の値で初期化されます (への参照をそれ自体が、 `object[]`)。

### <a name="static-and-instance-methods"></a>静的メソッドとインスタンス メソッド

メソッドの宣言が含まれています、`static`修飾子、メソッドは静的メソッドをいいます。 ない場合`static`修飾子が存在する、メソッドはインスタンス メソッドと呼ばれます。

静的メソッドは、特定のインスタンスでは動作せずを参照すると、コンパイル時エラー`this`で静的メソッド。

インスタンス メソッドは、クラスの特定のインスタンスを操作し、そのインスタンスとしてアクセスできます`this`([このアクセス](expressions.md#this-access))。

メソッドが参照されている場合、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`場合は、`M`静的メソッドでは、 `E` を含む型を表す必要があります`M`、場合`M`、インスタンス メソッドである`E`含まれる型のインスタンスを表す必要があります`M`します。

静的な違いは、インスタンス メンバーの説明とでさらに[静的メンバーとインスタンス メンバー](classes.md#static-and-instance-members)。

### <a name="virtual-methods"></a>仮想メソッド

インスタンス メソッドの宣言が含まれています、`virtual`修飾子、メソッドは仮想メソッドをいいます。 ない場合`virtual`修飾子が存在する、メソッドは、非仮想メソッドと呼ばれます。

非仮想メソッドの実装は、バリアントではありません。内のクラスのインスタンスでメソッドが呼び出されるかどうか、実装は同じ宣言されているか、派生クラスのインスタンス。 これに対し、仮想メソッドの実装は、派生クラスで置き換えることができます。 継承された仮想メソッドの実装に取り替えるのプロセスと呼びます***オーバーライド***メソッド ([メソッドをオーバーライド](classes.md#override-methods))。

仮想メソッドの呼び出しで、***実行時の型***その呼び出しが対象のインスタンスの場所は、実際のメソッドの実装を呼び出すを決定します。 非仮想メソッドの呼び出しで、***コンパイル時の型***インスタンスの決定要因は、します。 正確には、メソッドがという名前で`N`が呼び出されると、引数リスト`A`コンパイル時の型を持つインスタンスで`C`と実行時の型`R`(場所`R`か`C`派生したクラスまたは`C`)、呼び出しは次のように処理されます。

*  オーバー ロードの解決を適用する最初に、 `C`、 `N`、および`A`、特定の方法を選択する`M`で宣言および継承されるメソッドのセットから`C`します。 「[メソッドの呼び出し](expressions.md#method-invocations)します。
*  その後、if`M`非仮想メソッドでは、`M`が呼び出されます。
*  それ以外の場合、`M`は仮想メソッド、および、最派生実装の`M`を`R`が呼び出されます。

宣言またはクラスによって継承されている仮想メソッドのそれぞれについて、存在、***最も派生実装***そのクラスに対してメソッドの。 仮想メソッドの最多派生実装`M`クラスに関して`R`は次のように決定されます。

*  場合`R`、導入が含まれています。`virtual`の宣言`M`、これは、最派生実装の`M`します。
*  の場合`R`が含まれています、`override`の`M`、これは、最派生実装の`M`します。
*  それ以外の場合の実装の派生を最大限に`M`を`R`は、最派生実装のと同じ`M`の直接の基本クラスに対して`R`。

次の例は、仮想および非仮想メソッドの違いを示しています。
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

例では、`A`非仮想メソッドを導入`F`と仮想メソッド`G`します。 クラスは、`B`新しい非仮想メソッドが導入されています`F`、そのため、継承を非表示`F`、継承されたメソッドもオーバーライドと`G`します。 この例では、出力が生成されます。
```
A.F
B.F
B.G
B.G
```

注意ステートメント`a.G()`呼び出す`B.G`ではなく、`A.G`します。 これは、実行時の型のインスタンスのためです (これは`B`)、インスタンスのコンパイル時型ではありません (これは`A`) を呼び出す実際のメソッドの実装を決定します。

継承されたメソッドを非表示には、メソッドが許可されている、ために、同じシグネチャを持つ複数の仮想メソッドを含むクラス可能性があります。 最派生メソッドを除くすべてが非表示のため、あいまいさの問題は発生このしません。 例
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
`C`と`D`クラスに同じシグネチャを持つ 2 つの仮想メソッドを含めます。導入された、1 つ`A`で導入された、1 つ`C`します。 導入されたメソッド`C`から継承されたメソッドを非表示に`A`します。 オーバーライド宣言つまり`D`で導入されたメソッドをオーバーライド`C`のことはできませんと`D`で導入されたメソッドをオーバーライドする`A`。 この例では、出力が生成されます。
```
B.F
B.F
D.F
D.F
```

インスタンスへのアクセスによって非表示の仮想メソッドを呼び出すことは`D`小から派生した型、メソッドが非表示されません。

### <a name="override-methods"></a>メソッドをオーバーライドします。

インスタンス メソッドの宣言が含まれています、`override`修飾子、メソッドはモード、***メソッドをオーバーライドして***します。 オーバーライド メソッドでは、同じシグネチャを持つ継承された仮想メソッドをオーバーライドします。 仮想メソッドの宣言には新しいメソッドが導入されていますが、オーバーライド メソッドの宣言では、そのメソッドの新しい実装を提供することで既存の継承された仮想メソッドを特殊化します。

によってオーバーライドされたメソッド、`override`宣言と呼ばれる、***基本メソッドをオーバーライド***します。 オーバーライド メソッド`M`クラスで宣言されている`C`、オーバーライドされる基本メソッドはの各基本クラス型を調べることによって決まります`C`の直接基底クラスの型と、`C`進みながら連続します。直接の基本クラスの型が指定された基本クラス型であるアクセス可能なメソッドは、少なくとも 1 つまでと同じシグネチャを持つ`M`型引数の置換後にします。 オーバーライドされる基本メソッドを検索するためには、メソッドと見なされますがある場合は、アクセス`public`である場合、`protected`である場合、`protected internal`である場合または`internal`として同じプログラム内で宣言されていると`C`します。

上書きの宣言の場合は true、次のすべての場合を除いて、コンパイル時エラーが発生します。

*  前述のように配置されている、オーバーライドされる基本メソッドは使用できます。
*  このようなオーバーライドされる基本メソッドは 1 つだけです。 この制限は、基本クラスの型が構築された型、型引数の置換により 2 つのメソッドのシグネチャと同じ場合にのみ影響です。
*  オーバーライドされる基本メソッドが仮想、抽象型の場合、またはメソッドをオーバーライドします。 つまり、オーバーライドされる基本メソッドは、静的または非仮想にすることはできません。
*  オーバーライドの基本メソッドは、シール メソッドではありません。
*  オーバーライド メソッドとオーバーライドされる基本メソッドと同じ戻り値型であります。
*  オーバーライドされる基本メソッドとオーバーライド宣言は、同じの宣言されたアクセシビリティを持ちます。 つまり、オーバーライド宣言は、仮想メソッドのアクセシビリティを変更することはできません。 ただし、オーバーライドされる基本メソッドの内部が保護されており、オーバーライド メソッド、オーバーライド メソッドを含むアセンブリが宣言されているよりもに、別のアセンブリで宣言されている場合は、アクセシビリティを保護されなければなりません。
*  オーバーライドの宣言では、型パラメーター制約句が指定されていません。 代わりに、制約は、オーバーライドされる基本メソッドから継承されます。 オーバーライドされたメソッドの型パラメーターの制約を型引数は、継承された制約に置き換えることができますに注意してください。 これは、法的なは、値の型または sealed 型など、明示的に指定されている制約につながります。

次の例では、ジェネリック クラスのオーバーライドの規則のしくみを示しています。
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

オーバーライド宣言は、オーバーライドされる基本メソッドを使用して、アクセスできる、 *base_access* ([Base アクセス](expressions.md#base-access))。 例
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
`base.PrintFields()`で呼び出し`B`呼び出す、`PrintFields`メソッドで宣言`A`します。 A *base_access*仮想呼び出しメカニズムを無効にし、単に基本のメソッドを非仮想メソッドとして扱われます。 呼び出す必要がある`B`されて書き込まれる`((A)this).PrintFields()`、再帰的に存在するかを呼び出す、`PrintFields`メソッドで宣言`B`で宣言されている 1 つではない`A`、ため`PrintFields`仮想の実行時の型と`((A)this)`は`B`します。

含めることによってのみ、`override`修飾子は、メソッドは、別のメソッドをオーバーライドします。 その他のすべてのケースで、継承されたメソッドとして同じシグネチャを持つメソッドは単に継承されたメソッドを隠ぺいします。 例
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
`F`メソッド`B`は含まれません、`override`修飾子を上書きしませんし、`F`メソッド`A`します。 代わりに、`F`メソッド`B`でメソッドを非表示に`A`、宣言が含まれていないために、警告が報告されると、`new`修飾子。

例
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
`F`メソッド`B`仮想を非表示に`F`から継承されたメソッド`A`します。 以降、新しい`F`で`B`プライベート アクセスは、そのスコープには、のクラスの本文にはのみが含まれます。`B`には拡張されませんと`C`します。 宣言ではそのため、`F`で`C`上書きが許可されている、`F`から継承された`A`します。

### <a name="sealed-methods"></a>シール メソッド

インスタンス メソッドの宣言が含まれています、`sealed`修飾子、メソッドがあると言うこと、***メソッドをシール***します。 インスタンス メソッドの宣言が含まれている場合、`sealed`修飾子も含める必要があります、`override`修飾子。 使用、`sealed`修飾子からさらに、メソッドをオーバーライドする派生クラスをできないようにします。

例
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
クラスは、 `B` 2 つのメソッドのオーバーライドは、:`F`メソッドを持つ、`sealed`修飾子と`G`メソッドにはないです。 `B`使用が、シールドの`modifier`防止`C`をさらにオーバーライド`F`します。

### <a name="abstract-methods"></a>抽象メソッド

インスタンス メソッドの宣言が含まれています、`abstract`修飾子、メソッドがあると言うこと、***抽象メソッド***します。 抽象メソッドは暗黙的にも仮想メソッドの修飾子を持つことはできません`virtual`します。

抽象メソッドの宣言では、新しい仮想メソッドが導入されていますが、そのメソッドの実装は提供されません。 代わりに、非抽象派生クラスでは、そのメソッドをオーバーライドすることで独自の実装を提供する必要があります。 抽象メソッドが実際の実装を提供しないため、 *method_body*の抽象メソッドをセミコロンの構成だけです。

抽象メソッドの宣言は抽象クラスでのみ許可されます ([抽象クラス](classes.md#abstract-classes))。

例
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
`Shape`クラス自体を描画できる幾何学的な図形オブジェクトの抽象概念を定義します。 `Paint`意味のある既定の実装がないため、抽象メソッドです。 `Ellipse`と`Box`クラスは具象`Shape`実装します。 オーバーライドする必要がこれらのクラスは、非抽象であるため、`Paint`メソッドと、実際の実装を提供します。

コンパイル時エラーには、 *base_access* ([Base アクセス](expressions.md#base-access)) を抽象メソッドを参照します。 例
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
コンパイル時エラーが報告された、`base.F()`呼び出し抽象メソッドを参照しているためです。

抽象メソッドの宣言は、仮想メソッドをオーバーライド許可されています。 これにより、派生クラスでメソッドの再実装を強制する抽象クラスとメソッドの元の実装を使用できなくなります。 例
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
クラス`A`仮想メソッド、クラスを宣言`B`抽象メソッドとクラスを使用してこのメソッドをオーバーライド`C`独自の実装を提供する抽象メソッドをオーバーライドします。

### <a name="external-methods"></a>外部メソッド

メソッドの宣言が含まれています、`extern`修飾子、メソッドがあると言うこと、***外部メソッド***します。 外部メソッドは、通常 c# 以外の言語を使用して外部で実装されます。 外部メソッドの宣言は、実際の実装を提供するため、 *method_body*の外部メソッドをセミコロンの構成だけです。 外部メソッドのジェネリックできない可能性があります。

`extern`修飾子が組み合わせてで通常使用される、`DllImport`属性 ([と Win32 の COM コンポーネントとの相互運用](attributes.md#interoperation-with-com-and-win32-components))、外部メソッドを Dll (ダイナミック リンク ライブラリ) によって実装することができます。 実行環境では、外部メソッドの実装を提供できる、他のメカニズムをサポートできます。

外部メソッドが含まれる場合、`DllImport`属性、メソッドの宣言を含める必要がありますも、`static`修飾子。 この例の使用、`extern`修飾子と`DllImport`属性。
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

### <a name="partial-methods-recap"></a>部分メソッド (まとめ)

メソッドの宣言が含まれています、`partial`修飾子、メソッドがあると言うこと、***部分メソッド***します。 部分メソッドは、部分型のメンバーとしてのみ宣言できます ([部分型](classes.md#partial-types))、およびいくつかの制限を受けます。 部分メソッドはさらに「[部分メソッド](classes.md#partial-methods)します。

### <a name="extension-methods"></a>拡張メソッド

メソッドの最初のパラメーターが含まれる場合、`this`修飾子、メソッドがあると言うこと、***拡張メソッド***します。 拡張メソッドは、非ジェネリック、入れ子になった非静的クラスでのみ宣言できます。 拡張メソッドの最初のパラメーターを持たない修飾子以外`this`パラメーターの型がポインター型にすることはできません。

2 つの拡張メソッドを宣言する静的クラスの例を次に示します。
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

拡張メソッドは、通常の静的メソッドです。 さらに、その外側の静的クラスは、スコープ内では、拡張メソッド呼び出すことができますのインスタンス メソッドの呼び出し構文を使用して ([拡張メソッド呼び出し](expressions.md#extension-method-invocations))、最初の引数として受信者式を使用します。

次のプログラムでは、上で宣言された拡張メソッドを使用します。
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

`Slice`メソッドは、の使用、 `string[]`、および`ToInt32`メソッドで使用`string`拡張メソッドとして宣言されているため、します。 プログラムの意味では、次を使用して、通常の静的メソッドの呼び出しと同じです。
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

### <a name="method-body"></a>メソッドの本体

*Method_body*ブロック本体を式の本体またはセミコロンのいずれかのメソッドの宣言がで構成されます。

***型の結果***メソッドが`void`戻り値の型がある場合`void`、メソッドは非同期と戻り値の型である場合または`System.Threading.Tasks.Task`します。 戻り値の型、および非同期メソッドの戻り値の型の結果型では、非同期メソッドの結果型は、それ以外の場合、`System.Threading.Tasks.Task<T>`は`T`します。

メソッドにある場合、`void`結果の種類とブロックの本体、`return`ステートメント ([return ステートメント](statements.md#the-return-statement)) ブロックが式を指定することできません。 Void メソッドのブロックの実行が正常に完了するかどうか (つまり、メソッド本体の末尾からのフロー制御) メソッドが、現在の呼び出し元に単に返します。
    
メソッドにある場合、`void`結果および式、式の本文`E`必要があります、 *statement_expression*、本文がフォームのブロックの本体とまったく同じ`{ E; }`。
    
メソッドが void 以外の結果の型を持つ、ブロック、本体の各`return`ブロックのステートメントでは、結果の型に暗黙的に変換される式を指定する必要があります。 値を返すメソッドのブロックの本体のエンドポイントが到達可能な場合があります。 つまり、ブロックの本体で値を返すメソッドでは、コントロールは許可されていません、メソッド本体の末尾に到達します。
    
メソッドが void 以外の結果型は、式の本体と式は、結果の型に暗黙的に変換可能である必要があります、本文がフォームのブロックの本体と同じ`{ return E; }`します。
    
例
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
値を返す`F`メソッドは、制御は、メソッド本体の最後にフローできるため、コンパイル時エラーが発生結果します。 `G`と`H`メソッドは、すべての実行可能なパスが戻り値を指定する、return ステートメントで終了するので、正しい。 `I`の本体は等価ですが 1 つの戻りステートメントだけでステートメント ブロックされるため、メソッドです。

### <a name="method-overloading"></a>メソッドのオーバーロード

メソッドのオーバー ロード解決規則が記載されて[型推論](expressions.md#type-inference)します。

## <a name="properties"></a>プロパティ

A***プロパティ***オブジェクトまたはクラスの特性へのアクセスを提供するメンバーします。 プロパティの例では、顧客の名前、ウィンドウのキャプションのフォントのサイズの文字列の長さを含めるし、具合です。 プロパティは、フィールドの自然な拡張機能、両方を関連する型を持つメンバーを指定し、フィールドとプロパティにアクセスするための構文は同じです。 ただし、フィールドとは異なり、プロパティは格納場所を表しません。 その代わりに、プロパティには、値の読み取りまたは書き込みの際に実行されるステートメントを指定する "***アクセサー***" があります。 プロパティはオブジェクトの属性の書き込みと読み取りにアクションを関連付けるためのメカニズムを提供するためさらに、それらを計算するには、このような属性を許可します。

プロパティを使用して宣言*property_declaration*: %s

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

A *property_declaration*のセットを含めることができます*属性*([属性](attributes.md)) と有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、 `new` ([new 修飾子](classes.md#the-new-modifier))、 `static` ([静的およびインスタンス メソッド](classes.md#static-and-instance-methods))、 `virtual` ([仮想メソッド](classes.md#virtual-methods))、 `override` ([メソッドをオーバーライド](classes.md#override-methods))、 `sealed` ([メソッドをシール](classes.md#sealed-methods))、 `abstract` ([抽象メソッド](classes.md#abstract-methods))、および`extern` ([外部メソッド](classes.md#external-methods)) 修飾子。

プロパティ宣言では、メソッドの宣言と同じ規則 ([メソッド](classes.md#methods)) 修飾子の組み合わせが無効です。

*型*プロパティの宣言は、宣言によって導入されるプロパティの型を指定します、 *member_name*プロパティの名前を指定します。 プロパティが、明示的なインターフェイス メンバーの実装でない限り、 *member_name*は単に、*識別子*します。 明示的なインターフェイス メンバーの実装 ([明示的なインターフェイス メンバーの実装](interfaces.md#explicit-interface-member-implementations))、 *member_name*から成る、 *interface_type*後に、"`.`"および*識別子*します。

*型*プロパティは少なくともプロパティ自体と同程度にアクセスできる必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。

A *property_body*ので構成するか、***アクセサー本体***または***式本体***します。 アクセサーの本体で*accessor_declarations*、これで囲む必要があります"`{`「と」`}`"トークンは、アクセサーを宣言する ([アクセサー](classes.md#accessors)) プロパティの。 アクセサーは、読み取りと書き込みのプロパティに関連付けられている実行可能ステートメントを指定します。

構成される式の本体`=>`続けて、*式*`E`セミコロンはステートメント本体内でまったく同じですが、 `{ get { return E; } }`、ことができますのみため、get アクセス操作子のみを指定するにはプロパティは、get アクセス操作子の結果は 1 つの式で指定した場合。

A *property_initializer*を自動的に実装されたプロパティ用にのみ与えられます ([自動実装プロパティ](classes.md#automatically-implemented-properties))、し、このような基になるフィールドの初期化プロパティで指定された値、*式*します。

プロパティにアクセスするための構文と同じではフィールドが、プロパティは変数としては分類されません。 したがって、としてプロパティを渡すことはできません、`ref`または`out`引数。

プロパティの宣言が含まれています、`extern`修飾子は、このプロパティがあると、***外部プロパティ***。 外部プロパティの宣言は、それぞれの実際の実装を提供するため、 *accessor_declarations*をセミコロンで構成されます。

### <a name="static-and-instance-properties"></a>静的およびインスタンスのプロパティ

プロパティの宣言が含まれています、`static`修飾子は、このプロパティがあると、***静的プロパティ***。 ない場合`static`修飾子が存在する、このプロパティにすると、***インスタンス プロパティ***します。

静的プロパティは、特定のインスタンスに関連付けられていないを参照すると、コンパイル時エラー`this`静的プロパティのアクセサーでします。

インスタンス プロパティは、クラスのインスタンスに関連付けられていると、そのインスタンスとしてアクセスできます`this`([このアクセス](expressions.md#this-access)) でそのプロパティのアクセサー。

プロパティが参照されている場合、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`場合は、`M`静的なプロパティは、 `E` を含む型を表す必要があります`M`、場合`M`インスタンス プロパティは、電子メールに含まれる型のインスタンスを表す必要があります`M`します。

静的な違いは、インスタンス メンバーの説明とでさらに[静的メンバーとインスタンス メンバー](classes.md#static-and-instance-members)。

### <a name="accessors"></a>アクセサー

*Accessor_declarations*プロパティの読み取りと書き込みのプロパティに関連付けられている実行可能なステートメントを指定します。

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

アクセサーの宣言から成る、 *get_accessor_declaration*、 *set_accessor_declaration*、またはその両方です。 各アクセサーの宣言は、トークンの`get`または`set`続く省略可能な*accessor_modifier*と*accessor_body*します。

使用*accessor_modifier*s は、次の制限によって管理されます。

*  *Accessor_modifier*インターフェイスまたは明示的なインターフェイス メンバーの実装で使用できません。
*  プロパティまたはインデクサーを持たない`override`修飾子、 *accessor_modifier*プロパティまたはインデクサーの両方がある場合にのみ許可されて、`get`と`set`アクセサー、およびうちの 1 つでのみ許可し、アクセサー。
*  プロパティまたはインデクサーを含む、`override`修飾子をアクセサーが一致する必要があります、 *accessor_modifier*いずれかオーバーライドされるアクセサーの場合は、します。
*  *Accessor_modifier*プロパティまたはインデクサー自体の宣言されたアクセシビリティよりも厳密に制限は、アクセシビリティを宣言する必要があります。 正確には。
   * プロパティまたはインデクサーの宣言されたアクセシビリティを持つかどうか`public`、 *accessor_modifier*にある場合も`protected internal`、 `internal`、 `protected`、または`private`します。
   * プロパティまたはインデクサーの宣言されたアクセシビリティを持つかどうか`protected internal`、 *accessor_modifier*にある場合も`internal`、 `protected`、または`private`します。
   * プロパティまたはインデクサーの宣言されたアクセシビリティを持つかどうか`internal`または`protected`、 *accessor_modifier*あります`private`します。
   * プロパティまたはインデクサーの宣言されたアクセシビリティを持つかどうか`private`、 *accessor_modifier*使用可能性があります。

`abstract`と`extern`プロパティ、 *accessor_body*指定された各アクセサーは、セミコロンだけです。 各非 abstract、extern 以外のプロパティがあります*accessor_body*セミコロンをする場合は、***プロパティが自動的に実装***([自動実装プロパティ](classes.md#automatically-implemented-properties)). 自動的に実装されたプロパティを get アクセサーが必要です。 その他の非 abstract、extern 以外、任意のプロパティのアクセサーに、 *accessor_body*は、*ブロック*対応するアクセサーが呼び出されたときに実行されるステートメントを指定します。

A`get`アクセサーはプロパティの型の値を返すパラメーターなしのメソッドに相当します。 代入のターゲット プロパティが式で参照されている場合を除き、`get`プロパティの値を計算する、プロパティのアクセサーが呼び出されます ([式の値](expressions.md#values-of-expressions))。 本文を`get`アクセサーは値を返すの規則に従う必要がありますで説明したメソッド[メソッド本体](classes.md#method-body)します。 具体的には、すべて`return`ステートメントの本体で、`get`アクセサーはプロパティの型に暗黙的に変換される式を指定する必要があります。 さらのエンドポイントを`get`アクセサーに到達できないする必要があります。

A`set`アクセサーはプロパティの型の 1 つの値を持つパラメーターを持つメソッドに相当し、`void`型を返します。 暗黙のパラメーターを`set`アクセサーの名前は常に`value`します。 プロパティが割り当ての対象として参照されている場合 ([代入演算子](expressions.md#assignment-operators))、またはのオペランドとして`++`または`--`([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、 [前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))、`set`引数にアクセサーが呼び出されます (値が代入の右側にあるかのオペランドの`++`または`--`演算子) を新しい値を提供します ([単純な代入](expressions.md#simple-assignment))。 本文を`set`アクセサーがの規則に従う必要があります`void`で説明したメソッド[メソッド本体](classes.md#method-body)します。 具体的には、`return`内のステートメント、`set`式を指定するアクセサーの本体は許可されていません。 `set`アクセサーがという名前のパラメーターを暗黙的に`value`は、ローカル変数または定数宣言でのコンパイル時エラーです、`set`名前がアクセサー。

有無に基づいて、`get`と`set`アクセサー、プロパティは次のように分類されます。

*  両方が含まれるプロパティを`get`アクセサーと`set`アクセサーはモード、***読み取り/書き込み***プロパティ。
*  のみを持つプロパティを`get`アクセサーはモード、***読み取り専用***プロパティ。 割り当ての対象となる読み取り専用プロパティのコンパイル時エラーになります。
*  のみを持つプロパティを`set`アクセサーはモード、***書き込み専用***プロパティ。 ただし、代入式のターゲットとして式の中で、書き込み専用プロパティを参照すると、コンパイル時エラーになります。

例
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
`Button`コントロール宣言パブリック`Caption`プロパティ。 `get`のアクセサー、`Caption`プロパティがプライベートに格納された文字列を返します`caption`フィールド。 `set`アクセサーは、新しい値を格納する場合は、新しい値が現在の値と異なる場合にチェックし、コントロールを再描画します。 多くの場合、プロパティには、上記のパターンに従います。`get`アクセサーはプライベート フィールドに格納されている値だけを返します、`set`アクセサーは、そのプライベート フィールドを変更し、オブジェクトの状態を完全に更新するために必要な追加の操作を実行します。

指定された、`Button`上記のクラス、例を次の使用、`Caption`プロパティ。
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

ここでは、`set`アクセサーがプロパティの値を割り当てることによって呼び出されると、`get`アクセサーは、式でプロパティを参照することによって呼び出されます。

`get`と`set`プロパティのアクセサーが個別のメンバーでないし、プロパティのアクセサーを個別に宣言することはできません。 そのため、読み取り/書き込みプロパティの 2 つのアクセサーが異なるアクセシビリティを持つことはできません。 例では、
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
1 つの読み取り/書き込みプロパティを宣言していません。 読み取り専用のいずれか、同じ名前の 2 つのプロパティを宣言ではなく、書き込み専用の 1 つ。 同じクラスで宣言されている 2 つのメンバーは、同じ名前を持つことはできません、ため、例ではコンパイル時エラーが発生、します。

派生クラスでは、継承されたプロパティと同じ名前でプロパティを宣言して、派生プロパティの読み取りと書き込みの両方に関して、継承されたプロパティを非表示にします。 例
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
`P`プロパティ`B`を非表示に、`P`プロパティ`A`の読み取りと書き込みの両方に対して。 そのため、ステートメントで
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
割り当てを`b.P`以降、読み取り専用、報告されることをコンパイル時エラーが発生`P`プロパティ`B`書き込み専用の表示と非`P`プロパティ`A`します。 ただし、キャストを隠しへのアクセスに使用できること`P`プロパティ。

パブリックのフィールドとは異なりは、プロパティは、オブジェクトの内部状態とそのパブリック インターフェイスとの間の分離を提供します。 この例を検討してください。
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

ここでは、`Label`クラスを使用して 2 つ`int`フィールド、`x`と`y`、その場所を格納します。 パブリックには、場所は両方として公開されている、`X`と`Y`プロパティとして、`Location`型のプロパティ`Point`。 将来のバージョンの場合、`Label`と場所を格納する方が便利になります、`Point`内部的には、変更できるクラスのパブリック インターフェイスの影響を与えずに。
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

`x`と`y`されて代わりに`public readonly`フィールド、それはされているを変更すること、`Label`クラス。

プロパティから状態を公開することは必ずしもフィールドを直接公開するよりも効率が落ちるではありません。 具体的には、プロパティは、非仮想少量のコードのみを含む、ときに実行環境がアクセサーへの呼び出し、アクセサーの実際のコードに置き換えます。 このプロセスと呼びます***インライン展開***、およびフィールドへのアクセス、ほど効率的プロパティへのアクセスは、まだプロパティの柔軟性の向上が保持されます。

以降の呼び出しを`get`アクセサーは概念的には、フィールドの値を読み取るのと同じですが、プログラミング スタイルは不適切と見なされます`get`アクセサーを観測可能な副作用があります。 例
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
値、`Next`プロパティは、プロパティにアクセスした回数によって異なります。 したがって、監視可能な側効果を生成するプロパティにアクセスする、プロパティを代わりに、メソッドとして実装する必要があります。

「副作用」の規則`get`アクセサーは、ことを意味しない`get`を単にフィールドに格納されている値を返すアクセサーを記述する必要があります。 実際、`get`アクセサーは多くの場合、複数のフィールドへのアクセスやメソッドの呼び出しによってプロパティの値を計算します。 ただし、適切にデザインされた`get`アクセサーは、オブジェクトの状態の監視可能な変更を引き起こす操作を行いません。

最初に参照される時点まで、リソースの初期化を遅延プロパティを使用できます。 例:
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

`Console`クラスには、3 つのプロパティが含まれています。 `In`、 `Out`、および`Error`、標準入力、出力、およびデバイスのエラーをそれぞれ表すです。 これらのメンバーのプロパティとして公開することで、`Console`クラスは実際に使用されるまで初期化を遅らせることができます。 たとえば、最初に参照するときとき、`Out`に示すように、プロパティ
```csharp
Console.Out.WriteLine("hello, world");
```
基になる`TextWriter`出力デバイスを作成します。 アプリケーションへの参照がない場合は、`In`と`Error`それらのデバイス プロパティ、そのオブジェクトが作成されます。

### <a name="automatically-implemented-properties"></a>自動実装プロパティ

自動的に実装されたプロパティ (または***自動プロパティ***省略形)、セミコロン専用のアクセサー本体を持つ非抽象 extern 以外のプロパティです。 自動プロパティを使用して、get アクセサーがあります、set アクセサーを持つことができます必要に応じて。

プロパティを指定するには、自動的に実装されたプロパティとして、非表示のバッキング フィールドが自動的には、プロパティの使用可能なとアクセサーが読み取りし、そのバッキング フィールドへの書き込みのために実装されます。 バッキング フィールドと見なされます自動プロパティには、set アクセサーがなければ、 `readonly` ([読み取り専用フィールド](classes.md#readonly-fields))。 同じように、`readonly`フィールドを getter 専用自動プロパティ割り当てることもできますを外側のクラスのコンス トラクターの本体でします。 このような割り当ては、プロパティの読み取り専用のバッキング フィールドに直接割り当てられます。

自動プロパティことができます、 *property_initializer*、として、バッキング フィールドに直接適用、 *variable_initializer* ([変数初期化子](classes.md#variable-initializers)).

次のような例です。
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
次の宣言と同等です。
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
次の宣言と同等です。
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

コンス トラクター内で発生したため、読み取り専用フィールドへの割り当てが、有効なことに注意してください。


### <a name="accessibility"></a>ユーザー補助

アクセサーがある場合、 *accessor_modifier*、アクセシビリティ ドメイン ([アクセシビリティ ドメイン](basic-concepts.md#accessibility-domains)) は、アクセサーの決まりますの宣言されたアクセシビリティを使用して、 *accessor_modifier*. アクセサーがない場合、 *accessor_modifier*アクセサーのアクセシビリティ ドメインは、プロパティまたはインデクサーの宣言されたアクセシビリティから決定されます。

有無、 *accessor_modifier*メンバー検索には影響せず ([演算子](expressions.md#operators)) オーバー ロードの解決または ([オーバー ロードの解決](expressions.md#overload-resolution))。 プロパティまたはインデクサーに修飾子のプロパティまたはインデクサーは、バインドのアクセス権のコンテキストに関係なく常に判別します。

特定のプロパティまたはインデクサーが選択されていると、その使用法が有効であるかを判断する関連する特定のアクセサーのアクセシビリティ ドメインが使用されます。

*  値として使用する場合 ([式の値](expressions.md#values-of-expressions))、`get`アクセサーが存在し、アクセスできるようにする必要があります。
*  単純な代入のターゲットとして使用する場合 ([単純な代入](expressions.md#simple-assignment))、`set`アクセサーが存在し、アクセスできるようにする必要があります。
*  複合代入のターゲットとして使用するかどうかは ([複合代入](expressions.md#compound-assignment))、またはの対象として、`++`または`--`演算子 ([関数メンバー](expressions.md#function-members).9、 [Invocation 式](expressions.md#invocation-expressions)) の両方を`get`アクセサーと`set`アクセサーが存在し、アクセスできるようにする必要があります。

次の例では、プロパティで`A.Text`プロパティによって隠されている`B.Text`のみのコンテキストであっても、`set`アクセサーが呼び出されます。 プロパティとは異なり、`B.Count`がクラスにアクセスできない`M`ため、アクセス可能なプロパティ`A.Count`代わりに使用されます。

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

インターフェイスを実装するために使用されるアクセサーがない可能性があります、 *accessor_modifier*します。 他のアクセサーを宣言することも 1 つのアクセサーを使用するインターフェイスを実装すると、専用の場合、 *accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>封印されて、仮想、オーバーライド、および抽象プロパティ アクセサー

A`virtual`プロパティ宣言では、プロパティのアクセサーが仮想であることを指定します。 `virtual`修飾子は読み取り/書き込みプロパティの両方のアクセサーに適用されます: 読み取り/書き込みプロパティの 1 つだけのアクセサーは、仮想にすることはできません。

`abstract`プロパティ宣言は、プロパティのアクセサーが仮想では、アクセサーの実際の実装は提供されないことを指定します。 代わりに、非抽象派生クラスでは、プロパティをオーバーライドすることで、アクセサーの独自の実装を提供する必要があります。 抽象プロパティの宣言のアクセサーは、実際の実装を提供するため、 *accessor_body*セミコロンだけで構成されます。

両方が含まれるプロパティの宣言、`abstract`と`override`修飾子は、プロパティは抽象であり、基本プロパティをオーバーライドすることを指定します。 このようなプロパティのアクセサーは、抽象もあります。

抽象プロパティの宣言は抽象クラスでのみ許可されます ([抽象クラス](classes.md#abstract-classes))。仮想継承されたプロパティのアクセサーを指定するプロパティの宣言を含めることによって派生クラスでオーバーライドできます、`override`ディレクティブ。 これと呼ばれますが、***プロパティの宣言をオーバーライドする***します。 オーバーライドするプロパティの宣言では、新しいプロパティを宣言していません。 代わりに、既存の仮想プロパティのアクセサーの実装を特化するだけです。

オーバーライドするプロパティの宣言では、継承されるプロパティと正確な同じアクセシビリティ修飾子、型、および名前を指定する必要があります。 場合 (つまり、継承されたプロパティが読み取り専用または書き込み専用である場合)、継承されたプロパティが 1 つのアクセサーだけをオーバーライドするプロパティは、そのアクセサーだけを含める必要があります。 場合 (つまり、継承されたプロパティが読み取り/書き込みである場合)、継承されたプロパティが両方のアクセサーを含む、オーバーライドするプロパティは、アクセサーが 1 つまたは両方のアクセサーのいずれかを含めることができます。

オーバーライドするプロパティの宣言を含めることができます、`sealed`修飾子。 この修飾子を使用すると、派生クラスがさらに、プロパティをオーバーライドできなくなります。 封印されたプロパティのアクセサーはシールされてもいます。

宣言と呼び出しの相違点を除いて、構文、仮想、sealed、オーバーライド、および抽象アクセサーが仮想、シール、override キーワードと抽象メソッドとまったく同じ動作です。 具体的には、規則が記載[仮想メソッド](classes.md#virtual-methods)、[メソッドをオーバーライド](classes.md#override-methods)、[メソッドをシール](classes.md#sealed-methods)、および[抽象メソッド](classes.md#abstract-methods)適用としてアクセサーは、対応する形式のメソッドです。

*  A`get`アクセサーはプロパティの型と、包含するプロパティと同じ修飾子の値を返すパラメーターなしのメソッドに相当します。
*  A`set`アクセサーはプロパティの型の 1 つの値を持つパラメーターを持つメソッドに相当する`void`種類、およびそれを含むプロパティと同じ修飾子を返します。

例
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
`X` 仮想の読み取り専用プロパティは、`Y`仮想の読み取り/書き込みプロパティと`Z`は抽象の読み取り/書き込みプロパティです。 `Z`抽象クラスでは、含んでいるクラス`A`する必要がありますも抽象として宣言します。

派生したクラス`A`次に示します。
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

ここでは、宣言の`X`、 `Y`、および`Z`プロパティの宣言をオーバーライドします。 各プロパティの宣言は、アクセシビリティ修飾子、型、および対応する継承されたプロパティの名前を正確に一致します。 `get`のアクセサー`X`と`set`のアクセサー`Y`を使用して、`base`キーワードを使って、継承されたアクセサーにアクセスします。 宣言`Z`両方の抽象アクセサーのオーバーライド: そのため、未処理の抽象関数のメンバーではありません`B`と`B`非抽象クラスにすることが許可されています。

プロパティとして宣言されている場合、 `override`、オーバーライドされるアクセサーをオーバーライドするコードにアクセスできる必要があります。 さらに、プロパティまたはインデクサー自体の両方のおよびアクセサーの宣言されたアクセシビリティでは、オーバーライドされたメンバーおよびアクセサーの一致する必要があります。 例:
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

***イベント***オブジェクトや通知を提供するクラスのメンバーであります。 クライアントは、イベントの実行可能コードを追加指定することによって***イベント ハンドラー***します。

使用してイベントが宣言された*event_declaration*: %s

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

*Event_declaration*のセットを含めることができます*属性*([属性](attributes.md)) と有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、 `new` ([new 修飾子](classes.md#the-new-modifier))、 `static` ([静的およびインスタンス メソッド](classes.md#static-and-instance-methods))、 `virtual` ([仮想メソッド](classes.md#virtual-methods))、 `override` ([メソッドをオーバーライド](classes.md#override-methods))、 `sealed` ([メソッドをシール](classes.md#sealed-methods))、 `abstract` ([抽象メソッド](classes.md#abstract-methods))、および`extern` ([外部メソッド](classes.md#external-methods)) 修飾子。

イベント宣言では、メソッドの宣言と同じ規則 ([メソッド](classes.md#methods)) 修飾子の組み合わせが無効です。

*型*イベントの宣言がある必要があります、 *delegate_type* ([参照型](types.md#reference-types))、および*delegate_type*以上である必要がありますとしてイベント自体としてアクセスできるように ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。

イベントの宣言を含めることができます*event_accessor_declarations*します。 ただし、そうでない場合、extern 以外の場合の非抽象イベントをコンパイラによってに自動的に ([フィールドのようなイベント](classes.md#field-like-events)) は、extern イベント アクセサーが外部から提供されます。

イベントの宣言を省略した*event_accessor_declarations* 1 つまたは複数のイベントを定義します: のごとに 1 つ、 *variable_declarator*秒。 属性と修飾子などで宣言されたメンバーのすべてに適用されます、 *event_declaration*します。

コンパイル時エラーには、 *event_declaration*両方を含める、`abstract`修飾子と中かっこで区切られた*event_accessor_declarations*します。

イベントの宣言が含まれています、`extern`修飾子は、イベントはモード、***外部イベント***します。 両方を追加するエラーです、外部のイベント宣言は実際の実装を提供しないため、`extern`修飾子と*event_accessor_declarations*します。

コンパイル時エラーには、 *variable_declarator*イベント宣言での`abstract`または`external`修飾子を含める、 *variable_initializer*します。

イベントの左側のオペランドとして使用できる、`+=`と`-=`演算子 ([イベント割り当て](expressions.md#event-assignment))。 これらの演算子を使用、それぞれにイベント ハンドラーをアタッチするか、イベントからイベント ハンドラーを削除して、イベントのアクセス修飾子は、このような操作が許可されているコンテキストを制御します。

`+=`と`-=`はイベント、外部コードを宣言する型の外部イベントが許可されている唯一の操作できます追加し、イベントのハンドラーを削除するが、ことはできません、他の方法で取得またはイベントの基になるリストを変更ハンドラー。

フォームの操作で`x += y`または`x -= y`、`x`イベントは、の宣言を含む型の外部参照が行わ`x`、操作の結果は、型を持つ`void`(直す必要はありません型`x`の値を持つ`x`代入された後)。 このルールは、イベントの基になるデリゲートを直接調べることから、外部コードを禁止します。

次の例のインスタンスにイベント ハンドラーをアタッチする方法を示しています、`Button`クラス。
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

ここでは、`LoginDialog`インスタンス コンス トラクターでは、2 つ作成されます`Button`インスタンスし、イベント ハンドラーをアタッチします、`Click`イベント。

### <a name="field-like-events"></a>フィールドのようなイベント

クラスまたはイベントの宣言を含む構造体のプログラム テキスト内のフィールドのような特定のイベントを使用できます。 この方法で使用する、イベントすることはできません`abstract`または`extern`、明示的に含めることはできませんと*event_accessor_declarations*します。 このような場合は、フィールドを許容する任意のコンテキストで使用できます。 フィールドには、デリゲートが含まれています ([デリゲート](delegates.md)) これは、イベントに追加されているイベント ハンドラーの一覧を指します。 イベント ハンドラーが追加されていない場合、フィールドが含まれます`null`します。

例
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
`Click` 内のフィールドとして提供される、`Button`クラス。 例に示すようフィールド検査、変更、および使用できるデリゲートの呼び出し式で。 `OnClick`メソッドで、`Button`クラスが「生成」、`Click`イベント。 イベントを発生させるという概念は、イベントによって表されるデリゲートの呼び出しとまったく同じです。したがって、イベントを発生させるための特殊な言語コンストラクトはありません。 デリゲートの呼び出しは、デリゲートが null でないことを確認するチェックに続くことに注意してください。

宣言の外側、`Button`クラス、`Click`のみの左側にあるメンバーを使用できます、`+=`と`-=`の演算子
```csharp
b.Click += new EventHandler(...);
```
呼び出しリストにデリゲートを追加する`Click`イベント、および
```csharp
b.Click -= new EventHandler(...);
```
呼び出しリストからデリゲートを削除する`Click`イベント。

フィールドのようにイベントをコンパイルするときに、コンパイラは自動的に、デリゲートを保持するストレージを作成し、追加または削除するデリゲート フィールドのイベント ハンドラーをイベントのアクセサーを作成します。 追加と削除操作はスレッド セーフであると可能性があります (ただしする必要はありません) 中に実行するロックを保持 ([lock ステートメント](statements.md#the-lock-statement)) インスタンスのイベントを格納するオブジェクト、または型のオブジェクト ([Anonymousオブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) の静的イベント。

したがって、インスタンス イベントの形式の宣言:
```csharp
class X
{
    public event D Ev;
}
```
等価のものにコンパイルされます。
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
クラス内で`X`への参照`Ev`の左側にある、`+=`と`-=`演算子の追加が発生して remove アクセサーを呼び出すことができます。 その他のすべての参照を`Ev`隠しフィールドを参照にコンパイルされます`__Ev`代わりに ([メンバー アクセス](expressions.md#member-access))。 名前"`__Ev`"は任意です。 非表示フィールドがあって任意の名前または名前のないすべての。

### <a name="event-accessors"></a>イベント アクセサー

イベントの宣言は通常省略*event_accessor_declarations*、`Button`上記の例です。 そのため、1 つの状況としてには、イベントごとに 1 つのフィールドのストレージ コストが許容されない場合が含まれます。 このような場合は、クラスを含めることができます*event_accessor_declarations*およびイベント ハンドラーの一覧を格納するため、プライベート メカニズムを使用します。

*Event_accessor_declarations*イベントの追加と削除イベント ハンドラーに関連付けられている実行可能なステートメントを指定します。

アクセサーの宣言から成る、 *add_accessor_declaration*と*remove_accessor_declaration*します。 各アクセサーの宣言は、トークンの`add`または`remove`続けて、*ブロック*します。 *ブロック*に関連付けられている、 *add_accessor_declaration*イベント ハンドラーを追加すると、ときに実行するステートメントを指定します、*ブロック*に関連付けられています。*remove_accessor_declaration*イベント ハンドラーが削除されるときに実行するステートメントを指定します。

各*add_accessor_declaration*と*remove_accessor_declaration*イベントの種類の 1 つの値を持つパラメーターを持つメソッドに対応し、`void`型を返します。 イベント アクセサーの暗黙のパラメーターの名前は`value`します。 イベントはイベント割り当てで使用する適切なイベントのアクセサーが使用されます。 具体的には、代入演算子がある場合`+=`代入演算子は、add アクセサーを使用すると、 `-=` remove アクセサーが使用されます。 いずれの場合も、代入演算子の右側のオペランドは、イベント アクセサーに引数として使用されます。 ブロック、 *add_accessor_declaration*または*remove_accessor_declaration*の規則に従う必要があります`void`で説明したメソッド[メソッド本体](classes.md#method-body)します。 具体的には、`return`式を指定するこのようなブロック内のステートメントは許可されていません。

イベント アクセサーは、という名前のパラメーターを暗黙的にため`value`、その名前を持つイベント アクセサーでローカル変数または定数が宣言されているは、コンパイル時エラー。

例
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
`Control`クラスは、イベント用内部ストレージ メカニズムを実装します。 `AddEventHandler`メソッドは、キーを持つデリゲート値を関連付けます、`GetEventHandler`メソッドは、キーに関連付けられているデリゲートを返します、`RemoveEventHandler`メソッドは、指定したイベントのイベント ハンドラーとしてデリゲートを削除します。 多くの場合、基になるストレージ メカニズムは関連付けるためのコストはありませんように作られています、`null`キーを持つ値を委任し、未処理のイベント ストレージ消費しないためです。

### <a name="static-and-instance-events"></a>静的およびインスタンスのイベント

イベントの宣言が含まれています、`static`修飾子は、イベントはモード、***静的イベント***します。 ない場合`static`修飾子が存在する、イベントはモード、***インスタンス イベント***します。

静的イベントは、特定のインスタンスに関連付けられていないを参照すると、コンパイル時エラー`this`静的イベントのアクセサーでします。

インスタンス イベントは、クラスの特定のインスタンスに関連付けられ、としてこのインスタンスにアクセスできる`this`([このアクセス](expressions.md#this-access)) でそのイベントのアクセサー。

イベントが参照されている場合、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`場合は、`M`静的なイベントは、 `E` を含む型を表す必要があります`M`、場合`M`インスタンス イベントは、電子メールに含まれる型のインスタンスを表す必要があります`M`します。

静的な違いは、インスタンス メンバーの説明とでさらに[静的メンバーとインスタンス メンバー](classes.md#static-and-instance-members)。

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>封印されて、仮想、オーバーライド、および抽象イベント アクセサー

A`virtual`イベント宣言では、そのイベントのアクセサーが仮想であることを指定します。 `virtual`修飾子は、イベントの両方のアクセサーに適用されます。

`abstract`イベント宣言は、イベントのアクセサーが仮想では、アクセサーの実際の実装は提供されないことを指定します。 代わりに、非抽象派生クラスでは、イベントをオーバーライドすることで、アクセサーの独自の実装を提供する必要があります。 抽象イベントの宣言は実際の実装を提供しないため、中かっこで区切られたを提供できません*event_accessor_declarations*します。

両方が含まれるイベントの宣言、`abstract`と`override`修飾子は、イベントは抽象であり、基本イベントをオーバーライドを指定します。 このようなイベントのアクセサーは、抽象もあります。

抽象イベントの宣言は抽象クラスでのみ許可されます ([抽象クラス](classes.md#abstract-classes))。

継承された仮想イベントのアクセサーを指定するイベントの宣言を含めることによって派生クラスでオーバーライドできます、`override`修飾子。 これと呼ばれますが、***イベントの宣言をオーバーライドする***します。 オーバーライドするイベントの宣言では、新しいイベントを宣言していません。 代わりに、既存の仮想イベントのアクセサーの実装を特化するだけです。

オーバーライドするイベントの宣言では、上書きされるイベントとして正確な同じアクセシビリティ修飾子、型、および名前を指定する必要があります。

オーバーライドするイベントの宣言を含めることができます、`sealed`修飾子。 この修飾子を使用すると、派生クラスがさらに、イベントをオーバーライドできなくなります。 封印されたイベントのアクセサーはシールされてもいます。

コンパイル時エラーをオーバーライドする側のイベントの宣言を含めるには、`new`修飾子。

宣言と呼び出しの相違点を除いて、構文、仮想、sealed、オーバーライド、および抽象アクセサーが仮想、シール、override キーワードと抽象メソッドとまったく同じ動作です。 具体的には、規則が記載[仮想メソッド](classes.md#virtual-methods)、[メソッドをオーバーライド](classes.md#override-methods)、[メソッドをシール](classes.md#sealed-methods)、および[抽象メソッド](classes.md#abstract-methods)適用としてアクセサーは、対応する形式のメソッドをでした。 各アクセサーは、イベントの種類の 1 つの値を持つパラメーターを持つメソッドに相当する`void`種類、および、含んでいるイベントと同じ修飾子を返します。

## <a name="indexers"></a>インデクサー

***インデクサー***は、配列と同じ方法でインデックスを作成するオブジェクトのメンバーです。 使用してインデクサーを宣言*indexer_declaration*: %s

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

*Indexer_declaration*のセットを含めることができます*属性*([属性](attributes.md)) と有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、 `new` ([new 修飾子](classes.md#the-new-modifier))、 `virtual` ([仮想メソッド](classes.md#virtual-methods))、 `override` ([メソッドをオーバーライド](classes.md#override-methods))、 `sealed` ([メソッドをシール](classes.md#sealed-methods))、 `abstract` ([抽象メソッド](classes.md#abstract-methods))、および`extern`([外部メソッド](classes.md#external-methods)) 修飾子。

インデクサーの宣言では、メソッドの宣言と同じ規則 ([メソッド](classes.md#methods)) 有効な修飾子の組み合わせ、static 修飾子が 1 つの例外は許可されていません、インデクサーの宣言。

修飾子`virtual`、 `override`、および`abstract`は 1 つのケースを除く相互に排他的です。 `abstract`と`override`抽象インデクサーが仮想の 1 つをオーバーライドするために、修飾子をまとめて使用することです。

*型*インデクサーの宣言は、宣言によって導入されるインデクサーの要素の型を指定します。 インデクサーは、明示的なインターフェイス メンバーの実装、しない限り、*型*キーワードの後に`this`します。 明示的なインターフェイス メンバーの実装、*型*が続く、 *interface_type*、"`.`"、およびキーワード`this`します。 他のメンバーとは異なりインデクサーにはユーザー定義の名前がありません。

*Formal_parameter_list*インデクサーのパラメーターを指定します。 メソッドに対応するインデクサーの仮パラメーター リスト ([メソッドのパラメーター](classes.md#method-parameters)) には少なくとも 1 つのパラメーターを指定することを除いて、および、`ref`と`out`パラメーター修飾子は許可されていません.

*型*で参照される型のそれぞれのインデクサーの*formal_parameter_list*少なくとも、インデクサー自体と同程度にアクセスできる必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints)).

*Indexer_body*ので構成するか、***アクセサー本体***または***式本体***します。 アクセサーの本体で*accessor_declarations*、これで囲む必要があります"`{`「と」`}`"トークンは、アクセサーを宣言する ([アクセサー](classes.md#accessors)) プロパティの。 アクセサーは、読み取りと書き込みのプロパティに関連付けられている実行可能ステートメントを指定します。

構成される式の本体"`=>`"の後に式`E`セミコロンはステートメント本体内でまったく同じですが、 `{ get { return E; } }`、ことができますのみため、インデクサーの get アクセス操作子のみを指定する、get アクセス操作子の結果が1 つの式で指定されます。

インデクサーの要素にアクセスするための構文と同じでは配列要素が、インデクサーの要素は変数としては分類されません。 したがってとしてインデクサーの要素を渡すことはできません、`ref`または`out`引数。

インデクサーの仮パラメーター リストは、署名を定義します ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading)) のインデクサー。 具体的には、インデクサーのシグネチャは、その仮パラメーターの型と数で構成されます。 要素型と仮パラメーターの名前は、インデクサーのシグネチャの一部ではないです。

インデクサーのシグネチャは、同じクラスで宣言されている他のすべてのインデクサーの署名とは異なる必要があります。

インデクサーとプロパティは、概念的によく似ていますが、次の点で異なります。

*  プロパティは、インデクサーは、そのシグネチャで識別される一方、名前によって識別されます。
*  プロパティを通じてアクセス、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバー アクセス](expressions.md#member-access)) であるのに対し、インデクサー要素は、 *element_access* ([インデクサー アクセス](expressions.md#indexer-access))。
*  プロパティを`static`メンバー、インデクサーはインスタンス メンバーでは常にします。
*  A`get`一方、プロパティのアクセサーがパラメーターなしのメソッドに対応する`get`インデクサーのアクセサーは、インデクサーと同じ仮パラメーター リストを持つメソッドに相当します。
*  A`set`という名前の 1 つのパラメーターのプロパティのアクセサーがメソッドに対応`value`であるのに対し、`set`インデクサーのアクセサーは、インデクサー、および追加のパラメーターとして同じ仮パラメーター リストを持つメソッドに相当名前付き`value`します。
*  インデクサー パラメーターと同じ名前でローカル変数を宣言するインデクサーのアクセサーのコンパイル時エラーになります。
*  構文を使用して継承されたプロパティにアクセスをオーバーライドするプロパティの宣言で`base.P`ここで、`P`プロパティの名前です。 構文を使用して、オーバーライドするインデクサーの宣言から継承されたインデクサーにアクセスは`base[E]`ここで、`E`式のコンマ区切り一覧を示します。
*  「自動的に実装されたインデクサー」の概念はありません。 セミコロンのアクセサーを持つ非抽象、非外部のインデクサーにエラーになります。

これらの違いとは別にすべてのルールが定義されている[アクセサー](classes.md#accessors)と[自動実装プロパティ](classes.md#automatically-implemented-properties)インデクサー アクセサーもプロパティ アクセサーに適用します。

インデクサーの宣言が含まれています、`extern`修飾子は、インデクサーはモード、***外部インデクサー***します。 外部のインデクサーの宣言は、それぞれの実際の実装を提供するため、 *accessor_declarations*をセミコロンで構成されます。

次の例の宣言を`BitArray`ビット配列内の個々 のビットにアクセスするためのインデクサーを実装するクラス。
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

インスタンス、`BitArray`クラスは、対応するよりも大幅に削減のメモリを消費`bool[]`(前者の各値の代わりに 1 ビットのみが占めるため、後者の 1 バイト) と同じ操作、ようになりますが、`bool[]`します。

次`CountPrimes`クラスで使用する`BitArray`と 1 と指定された最大値の間の素数の数を計算する従来の「エラトステネス」アルゴリズム。
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

なお、構文の要素にアクセスするため、`BitArray`が正確に同じである、`bool[]`します。

次の例では、2 つのパラメーターを持つインデクサーを持つ 26 * 10 グリッド クラスを示します。 大文字または小文字の文字 A ~ Z の範囲の最初のパラメーターが必要し、する 0 ~ 9 の範囲の整数である 2 つ目が必要です。

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

### <a name="indexer-overloading"></a>インデクサーのオーバー ロード

インデクサーのオーバー ロード解決規則が記載されて[型推論](expressions.md#type-inference)します。

## <a name="operators"></a>演算子

***演算子***は、クラスのインスタンスに適用できる式の演算子の意味を定義するメンバーです。 演算子を使用して宣言*operator_declaration*: %s

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

オーバー ロードされた演算子の 3 つのカテゴリがあります。単項演算子 ([単項演算子](classes.md#unary-operators))、二項演算子 ([二項演算子](classes.md#binary-operators))、および変換演算子 ([変換演算子](classes.md#conversion-operators))。

*Operator_body*がいずれかをセミコロン、***ステートメント本体***または***式本体***します。 ステートメント本体から成る、*ブロック*演算子が呼び出されたときに実行するステートメントを指定します。 *ブロック*値を返すための規則に準拠している必要がありますで説明したメソッド[メソッド本体](classes.md#method-body)します。 式の本体から成る`=>`式およびセミコロンが続くし、演算子が呼び出されたときに実行する 1 つの式を表します。

`extern` 、演算子、 *operator_body*セミコロンだけで構成されます。 その他のすべての演算子、 *operator_body*はブロック本体または式の本体。

すべての演算子の宣言に、次の規則が適用されます。

*  演算子の宣言は、両方を含める必要があります、`public`と`static`修飾子。
*  演算子のパラメーターはパラメーターの値である必要があります ([パラメーターの値](variables.md#value-parameters))。 演算子の宣言を指定すると、コンパイル時エラー`ref`または`out`パラメーター。
*  演算子のシグネチャ ([単項演算子](classes.md#unary-operators)、[二項演算子](classes.md#binary-operators)、[変換演算子](classes.md#conversion-operators)) で宣言されているその他のすべての演算子のシグネチャが異なる場合、同じクラスです。
*  演算子の宣言で参照されるすべての型は、少なくとも、演算子自体と同程度にアクセスである必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。
*  演算子の宣言内で複数回、同じ修飾子のエラーになります。

各演算子のカテゴリは、次のセクションで説明したように、制限を課しています。

他のメンバーのような基底クラスで宣言されている演算子は、派生クラスによって継承されます。 演算子の宣言は、常に、クラスまたは構造体への参加、演算子のシグネチャ、演算子が宣言されている必要があるために、派生クラスで宣言された基本クラスで宣言されている演算子が非表示にするオペレーターのことはできません。 つまり、`new`修飾子が、必要なことはありませんし、ためでは使用できない、演算子の宣言。

単項および二項演算子の追加情報が記載されて[演算子](expressions.md#operators)します。

変換演算子の追加情報が記載されて[ユーザー定義の変換](conversions.md#user-defined-conversions)します。

### <a name="unary-operators"></a>単項演算子

単項演算子の宣言に、次の規則が適用されます、`T`はクラスまたは演算子の宣言を含む構造体のインスタンスの型を示します。

*  単項`+`、 `-`、 `!`、または`~`演算子は、型の 1 つのパラメーターを受け取る必要があります`T`または`T?`任意の型を返すことができます。
*  単項`++`または`--`演算子は、型の 1 つのパラメーターを受け取る必要があります`T`または`T?`から同じ型または型が派生を返す必要があります。
*  単項`true`または`false`演算子は、型の 1 つのパラメーターを受け取る必要があります`T`または`T?`と型を返す必要があります`bool`します。

単項演算子のシグネチャは、演算子のトークンで構成されます (`+`、 `-`、 `!`、 `~`、 `++`、 `--`、 `true`、または`false`) と 1 つの仮パラメーターの型。 戻り値の型は、単項演算子のシグネチャの一部でないも仮パラメーターの名前を指定します。

`true`と`false`単項演算子には、ペアで宣言が必要があります。 クラスがこれらの演算子の 1 つも、もう一方を宣言せずに宣言する場合、コンパイル時エラーが発生します。 `true`と`false`演算子の詳細については[ユーザー定義の条件付き論理演算子](expressions.md#user-defined-conditional-logical-operators)と[ブール式](expressions.md#boolean-expressions)します。

次の例では、実装とのそれ以降の使用を示しています。`operator ++`整数ベクター クラス。
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

演算子のメソッドが後置インクリメントと同じように、オペランドに 1 を追加することによって生成された値を返す方法に注意してくださいし、前置デクリメント演算子 ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators))、前置インクリメントとデクリメント演算子 ([前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。 異なり C++ では、このメソッド必要がありますいない、オペランドの値を直接変更します。 実際には、オペランドの値を変更すると、後置のインクリメント演算子の標準のセマンティクスが違反していました。

### <a name="binary-operators"></a>バイナリ演算子

二項演算子の宣言に、次の規則が適用されます、`T`はクラスまたは演算子の宣言を含む構造体のインスタンスの型を示します。

*  バイナリの非シフト演算子は、型である必要がありますが少なくとも 1 つ、2 つのパラメーターを受け取る必要があります`T`または`T?`、任意の型を返すことができます。
*  バイナリ`<<`または`>>`演算子の型である必要があります、2 つのパラメーターを受け取る必要があります`T`または`T?`2 番目の型である必要がありますと`int`または`int?`、任意の型を返すことができます。

二項演算子のシグネチャは、演算子のトークンで構成されます (`+`、 `-`、 `*`、 `/`、 `%`、 `&`、 `|`、 `^`、 `<<`、 `>>`、`==`、 `!=`、 `>`、 `<`、 `>=`、または`<=`) と 2 つの仮パラメーターの型。 戻り値の型と仮パラメーターの名前は、二項演算子のシグネチャの一部ではありません。

特定のバイナリ演算子では、ペアで宣言が必要です。 すべてのペアのいずれかの演算子の宣言では、ペアの他の演算子の宣言に一致する必要があります。 2 つの演算子の宣言は同じ戻り値の型と各パラメーターの型が同じである場合と一致します。 次の演算子には、ペアで宣言が必要です。

*  `operator ==` および `operator !=`
*  `operator >` および `operator <`
*  `operator >=` および `operator <=`

### <a name="conversion-operators"></a>変換演算子

変換演算子の宣言が導入されています、***ユーザー定義の変換***([ユーザー定義の変換](conversions.md#user-defined-conversions)) 定義済みの明示的および暗黙的な変換を強化します。

変換演算子の宣言を含む、`implicit`キーワードには、ユーザー定義の暗黙的な変換が導入されています。 暗黙的な変換は、さまざまな関数メンバーの呼び出し、式のキャスト、代入などの状況で発生することができます。 詳細についてはこの[暗黙的な変換](conversions.md#implicit-conversions)します。

変換演算子の宣言を含む、`explicit`キーワードには、ユーザー定義の明示的な変換が導入されています。 明示的な変換は、キャスト式で発生しの詳細については[明示的な変換](conversions.md#explicit-conversions)します。

変換演算子は、戻り値の型変換演算子によって示される、ターゲットの型に、変換演算子のパラメーターの型によって示される、ソースの種類からに変換します。

指定したソースの種類`S`ターゲットの種類と`T`場合は、`S`または`T`は null 許容型は、ように`S0`と`T0`それ以外の場合、基になる型を参照してください`S0`と`T0`は等しい`S`と`T`それぞれします。 ソースの種類からの変換を宣言するクラスまたは構造体が許可されている`S`をターゲットの型`T`次のすべてに当てはまる場合にのみ。

*  `S0` `T0`はさまざまな種類。
*  いずれか`S0`または`T0`演算子宣言が行われるクラスまたは構造体の型です。
*  どちらも`S0`も`T0`は、 *interface_type*します。
*  ユーザー定義の変換を除く、変換が存在しないから`S`に`T`またはから`T`に`S`します。

これらの規則のためには、任意の型パラメーターに関連付けられている`S`または`T`が、他の種類、継承関係とパラメーターが無視されるこれらの型に対する制約のない一意の型と見なされます。

例
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
最初の 2 つの演算子の宣言は許可のため[インデクサー](classes.md#indexers).3`T`と`int`と`string`それぞれ一意の型とは関係がないと見なされます。 しかし、3 番目の演算子がエラー`C<T>`の基本クラスは、`D<T>`します。

2 番目のルールに従っている変換演算子は、または演算子が宣言されているクラスまたは構造体の型から変換する必要があります。 たとえば、クラスまたは構造体の型は`C`から変換を定義する`C`に`int`との間`int`に`C`がからではなく`int`に`bool`します。

定義済みの変換を直接再定義することはできません。 したがって、変換演算子がからまたは変換を許可されていません`object`間明示的および暗黙的な変換が既に存在するため`object`およびその他のすべての型。 同様に、ソースでも、変換のターゲットの種類できます、その他の基本型変換は既に存在しため。

ただし、特定の型引数の定義済みの変換として既に存在する変換を指定するジェネリック型で演算子を宣言することはできます。 例
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
入力すると`object`がの型引数として指定されて`T`、2 番目の演算子が既に存在する変換を宣言します (、暗黙的なため明示的でも任意の型を型から変換が存在する`object`)。

2 種類の定義済みの変換が存在する場合、それらの型の間でユーザー定義の変換は無視されます。 具体的には、次のように使用します。

*  場合、定義済みの暗黙的な変換 ([暗黙的な変換](conversions.md#implicit-conversions)) 型からが存在する`S`入力`T`、すべてのユーザー定義の変換 (暗黙的または明示的な) から`S`に`T`は無視されます。
*  定義済みの明示的な変換の場合 ([明示的な変換](conversions.md#explicit-conversions)) 型から存在する`S`入力`T`、ユーザーによる明示的な変換から`S`に`T`は無視されます。 さらには。

場合`T`インターフェイス型は、ユーザー定義からの暗黙的な変換は、`S`に`T`は無視されます。

それ以外の場合、ユーザー定義の暗黙的な変換から`S`に`T`と見なされます。

すべての種類が`object`、によって演算子が宣言されている、`Convertible<T>`上記の型が定義済みの変換と競合しません。 例:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

ただし、型の`object`、定義済みの変換には、すべてのケースが 1 つのユーザー定義の変換が非表示にします。

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

ユーザー定義の変換からの変換は許可されません*interface_type*秒。 具体的には、この制限により、ユーザー定義の変換が発生しないことに変換するときに、 *interface_type*、ことへの変換、 *interface_type*場合にのみ成功オブジェクト実際に変換される、指定した実装*interface_type*します。

変換演算子のシグネチャは、ソースの種類とターゲットの種類で構成されます。 (対象の戻り値の型は、署名に参加しているメンバーの唯一の形式であるに注意してください)。`implicit`または`explicit`変換演算子の分類が演算子のシグネチャの一部ではありません。 したがって、クラスまたは構造体を宣言できません両方、`implicit`と`explicit`を同じソースとターゲットの型変換演算子。

一般に、ユーザー定義の暗黙的な変換は例外をスローしないと、情報が失われることはありませんするように設計する必要があります。 ユーザー定義の変換は、(たとえば、元の引数が範囲外です) ために、例外の上昇を与えることが場合はその変換 (上位ビットが破棄) などの情報の損失は、明示的な変換として定義する必要があります。

例
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
変換`Digit`に`byte`決してに例外がスローまたはについてがからの変換を失うので、暗黙的に`byte`に`Digit`以降明示的`Digit`の使用可能なサブセットを表すことができますのみ値を`byte`します。

## <a name="instance-constructors"></a>インスタンス コンス トラクター

"***インスタンス コンストラクター***" は、クラスのインスタンスを初期化するために必要なアクションを実装するメンバーです。 使用してインスタンス コンス トラクターが宣言された*constructor_declaration*: %s

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

A *constructor_declaration*のセットを含めることができます*属性*([属性](attributes.md))、有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、および`extern`([外部メソッド](classes.md#external-methods)) 修飾子。 複数回、同じ修飾子を含めるには、コンス トラクターの宣言は許可されていません。

*識別子*の*constructor_declarator*インスタンス コンス トラクターが宣言されているクラスの名前を指定する必要があります。 その他の任意の名前を指定すると、コンパイル時エラーが発生します。

省略可能な*formal_parameter_list*インスタンスのコンス トラクターと同じ規則に従って、 *formal_parameter_list*メソッドの ([メソッド](classes.md#methods))。 仮パラメーター リストは、署名を定義します ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading))、インスタンス コンス トラクターのというプロセスを管理オーバー ロードの解決 ([型推論](expressions.md#type-inference)) 特定の選択呼び出しのインスタンス コンス トラクターです。

参照される型の各、 *formal_parameter_list*インスタンスのコンス トラクターは、少なくともコンス トラクター自体と同程度にアクセスする必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。

省略可能な*constructor_initializer*で指定されたステートメントを実行する前に呼び出すための別のインスタンス コンス トラクターを指定します、 *constructor_body*のこのインスタンスのコンス トラクター。 詳細についてはこの[コンス トラクター初期化子](classes.md#constructor-initializers)します。

コンス トラクターの宣言が含まれています、`extern`修飾子は、コンス トラクターはモード、***コンス トラクターの外部***します。 外部コンス トラクターの宣言は、実際の実装を提供しないため、その*constructor_body*をセミコロンで構成されます。 その他のすべてのコンス トラクター、 *constructor_body*から成る、*ブロック*クラスの新しいインスタンスを初期化するステートメントを指定します。 これは正確に対応して、*ブロック*を持つインスタンス メソッドの`void`型を返す ([メソッド本体](classes.md#method-body))。

インスタンス コンス トラクターは継承されません。 したがって、クラスでは、実際には、クラスで宣言されている以外のインスタンス コンス トラクターがありません。 既定のインスタンス コンス トラクターが自動的に提供されるクラスにインスタンス コンス トラクターの宣言が含まれていない場合 ([既定のコンス トラクター](classes.md#default-constructors))。

インスタンス コンス トラクターを呼び出す*object_creation_expression*s ([オブジェクト作成式](expressions.md#object-creation-expressions)) および ~ *constructor_initializer*秒。

### <a name="constructor-initializers"></a>Constructor Initializers (コンストラクター初期化子)

すべてのインスタンス コンス トラクター (クラスを除く、 `object`) 暗黙的に別のインスタンス コンス トラクターの呼び出しをする直前に含める、 *constructor_body*します。 暗黙的に呼び出すコンス トラクターはによって決定されます、 *constructor_initializer*:

*  フォームのインスタンス コンス トラクター初期化子`base(argument_list)`または`base()`により呼び出される直接の基本クラスからインスタンス コンス トラクター。 使用してそのコンス トラクターを選択*argument_list*場合存在し、オーバー ロード解決規則[オーバー ロードの解決](expressions.md#overload-resolution)します。 直接の基本クラスに含まれているすべてのアクセス可能なインスタンス コンス トラクターまたは既定のコンス トラクターの候補のインスタンス コンス トラクターのセットで構成されます ([既定のコンス トラクター](classes.md#default-constructors)) にインスタンス コンス トラクターが宣言されていない場合、直接の基本クラスです。 このセットが空の場合、または 1 つの最適なインスタンス コンス トラクターを識別できない場合は、コンパイル時エラーが発生します。
*  フォームのインスタンス コンス トラクター初期化子`this(argument-list)`または`this()`自体を呼び出すクラスのインスタンス コンス トラクターを発生します。 使用して、コンス トラクターを選択*argument_list*場合存在し、オーバー ロード解決規則[オーバー ロードの解決](expressions.md#overload-resolution)します。 インスタンス コンス トラクターの候補のセットは、クラス自体で宣言されているすべてのアクセス可能なインスタンス コンス トラクターで構成されます。 このセットが空の場合、または 1 つの最適なインスタンス コンス トラクターを識別できない場合は、コンパイル時エラーが発生します。 インスタンス コンス トラクターの宣言には、コンス トラクター自体を呼び出すコンス トラクター初期化子が含まれている場合、コンパイル時エラーが発生します。

インスタンス コンス トラクターは、フォームのコンス トラクター初期化子、コンス トラクター初期化子を持たない場合`base()`が暗黙的に指定します。 したがって、インスタンス コンス トラクターの宣言フォームの
```csharp
C(...) {...}
```
まったく同じです。
```csharp
C(...): base() {...}
```

によって指定されたパラメーターのスコープ、 *formal_parameter_list*宣言には、インスタンス コンス トラクターの宣言のコンス トラクター初期化子が含まれています。 そのため、コンス トラクター初期化子はコンス トラクターのパラメーターにアクセスを許可します。 例:
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

インスタンス コンス トラクターの初期化子は、作成中のインスタンスにアクセスできません。 そのため、コンパイル時のエラーを参照するが`this`コンス トラクター初期化子の引数式、としては、による任意のインスタンス メンバーを参照する引数式のコンパイル時エラー、 *simple_name*.

### <a name="instance-variable-initializers"></a>インスタンス変数初期化子

インスタンス コンス トラクターを持たないコンス トラクター初期化子、またはフォームのコンス トラクター初期化子があると`base(...)`、そのコンス トラクターが暗黙的に指定された初期化を実行、 *variable_initializer*の sインスタンス フィールドは、そのクラスで宣言します。 これは、一連のコンス トラクターに、直接基底クラスのコンス トラクターの暗黙的な呼び出しの前に入ったときにすぐに実行される割り当てに対応します。 変数の初期化子は、クラス宣言に表示されるテキストの順序で実行されます。

### <a name="constructor-execution"></a>コンス トラクターの実行

変数初期化子は代入ステートメントに変換され、基底クラスのインスタンス コンス トラクターの呼び出しの前にこれらの代入ステートメントが実行されます。 この順序により、すべてのインスタンス フィールドは、そのインスタンスへのアクセス権を持つすべてのステートメントが実行される前に、変数初期化子で初期化されます。

例を示します
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
ときに`new B()`のインスタンスを作成するために使用`B`、次の出力が生成されます。
```
x = 1, y = 0
```

値`x`1 は、基底クラスのインスタンス コンス トラクターが呼び出される前に、変数の初期化子が実行されるためです。 ただしの値`y`は 0 です (既定値の`int`) ためへの割り当てを`y`基底クラスのコンス トラクターから制御が戻た後までは実行されません。

インスタンス変数初期化子とコンス トラクター初期化子、ステートメントの前に自動的に挿入されると考えると便利だと、 *constructor_body*します。 例では、
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
いくつかの変数初期化子; が含まれています両方のフォームのコンス トラクター初期化子も含まれています (`base`と`this`)。 例は、次のコードに対応が各コメントが (自動的に挿入されたコンス トラクターの呼び出しに使用する構文が有効でないが、メカニズムを説明するためにのみ機能します)、自動的に挿入されたステートメントを示します。

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

クラスにインスタンス コンス トラクターの宣言が含まれていない場合は、既定のインスタンス コンス トラクターが自動的に提供します。 既定のコンス トラクターは、直接基底クラスのパラメーターなしのコンス トラクターを呼び出すだけです。 抽象クラスで宣言されたアクセシビリティの既定のコンス トラクターは保護されています。 それ以外の場合、既定のコンス トラクターの宣言されたアクセシビリティがパブリックです。 したがって、既定のコンス トラクターは常にフォームの

```csharp
protected C(): base() {}
```
または
```csharp
public C(): base() {}
```
場所`C`クラスの名前を指定します。 オーバー ロードの解決が基底クラスのコンス トラクター初期化子に一意の最適な候補を特定できない場合は、コンパイル時エラーが発生します。

例
```csharp
class Message
{
    object sender;
    string text;
}
```
クラスにインスタンス コンス トラクターの宣言が含まれていないために、既定のコンス トラクターが提供されます。 したがって、例では、とまったく同じです。
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>プライベート コンス トラクター

クラスと`T`プライベート インスタンス コンス トラクターのみを宣言して、プログラム テキストの外側のクラスのことはできません`T`から派生する`T`または直接のインスタンスを作成する`T`します。 したがって、クラスが静的メンバーのみが含まれています、インスタンス化するためのものでない場合、空のプライベート インスタンス コンス トラクターを追加することはインスタンス化されないようにします。 例:
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

`Trig`クラスは、関連するメソッドと定数、グループが、インスタンス化するものではありません。 そのため、1 つの空のプライベート インスタンス コンス トラクターを宣言します。 既定のコンス トラクターの自動生成を抑制するには、少なくとも 1 つのインスタンス コンス トラクターを宣言しなければなりません。

### <a name="optional-instance-constructor-parameters"></a>省略可能なインスタンス コンス トラクターのパラメーター

`this(...)`オプションのインスタンス コンス トラクターのパラメーターを実装するために、コンス トラクター初期化子の形式はオーバー ロードと組み合わせて使用一般的です。 例
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
最初の 2 つのインスタンス コンス トラクターは、だけで欠けている引数の既定値を指定します。 両方を使用して、`this(...)`を 3 つ目の新しいインスタンスを初期化中のインスタンス コンス トラクターを呼び出すコンス トラクター初期化子。 省略可能なコンス トラクターのパラメーターの意味になります。
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>静的コンストラクター

A***静的コンス トラクター***が閉じられたクラス型の初期化に必要なアクションを実装するメンバー。 使用して静的コンス トラクターが宣言された*static_constructor_declaration*: %s

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

A *static_constructor_declaration*のセットを含めることができます*属性*([属性](attributes.md)) および`extern`修飾子 ([の外部メソッド](classes.md#external-methods)).

*識別子*の*static_constructor_declaration*静的コンス トラクターが宣言されているクラスの名前を指定する必要があります。 その他の任意の名前を指定すると、コンパイル時エラーが発生します。

静的コンス トラクターの宣言が含まれています、`extern`修飾子は、静的コンス トラクターはモード、***外部の静的コンス トラクター***します。 外部の静的コンス トラクターの宣言は、実際の実装を提供しないため、その*static_constructor_body*をセミコロンで構成されます。 その他のすべての静的コンス トラクター宣言、 *static_constructor_body*から成る、*ブロック*クラスを初期化するために実行するステートメントを指定します。 これには対応、 *method_body*して静的メソッドの`void`型を返す ([メソッド本体](classes.md#method-body))。

静的コンス トラクターは継承されませんし、直接呼び出すことはできません。

閉じられたクラス型の静的コンス トラクターは、特定のアプリケーション ドメインで最大で 1 回実行します。 静的コンス トラクターの実行は、次のイベントの最初のアプリケーション ドメイン内に発生するによってトリガーされます。

*  クラス型のインスタンスが作成されます。
*  クラス型の静的メンバーのいずれかが参照されます。

クラスが含まれている場合、`Main`メソッド ([アプリケーションの起動](basic-concepts.md#application-startup)) で、どの実行が開始される、静的コンス トラクターの前にそのクラスが実行される、`Main`メソッドが呼び出されます。

最初の静的フィールドの新しいセットで閉じられたクラスの新しい型を初期化するために ([静的およびインスタンス フィールド](classes.md#static-and-instance-fields)) 特定のクローズ型を作成します。 静的フィールドの各はその既定値に初期化 ([既定値](variables.md#default-values))。 次に、静的フィールド初期化子 ([静的フィールドの初期化](classes.md#static-field-initialization)) それらの静的フィールドが実行されます。 最後に、静的コンス トラクターが実行されます。

例では、
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
```
Init A
A.F
Init B
B.F
```
の実行`A`の静的コンス トラクターがへの呼び出しによってトリガーされる`A.F`の実行と`B`の静的コンス トラクターがへの呼び出しによってトリガーされる`B.F`します。

既定値の状態で見られる変数初期化子を含む静的フィールドの循環依存関係を構築することになります。

例では、
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
```
X = 1, Y = 2
```

実行する、`Main`メソッドをシステムを初めて実行の初期化子`B.Y`クラスを前に、`B`の静的コンス トラクター。 `Y`初期化子によって`A`のために実行する静的コンス トラクターの値`A.X`参照されます。 静的コンス トラクター `A`の値を計算に続いて `X`の既定値はフェッチそうすることで `Y`ゼロであります。 `A.X` そのため 1 に初期化されます。 実行中のプロセス`A`静的フィールド初期化子と静的コンス トラクターの後が完了したらの初期値の計算に返す `Y`これは 2 となります。

実行時のチェック制約を使用してコンパイル時にチェックすることはできませんが、型パラメーターに適用する便利な場所は、静的コンス トラクターは、それぞれのクローズ構築されたクラス型の 1 回だけ実行は、ため、([型パラメーター制約](classes.md#type-parameter-constraints))。 たとえば、次の種類は、型引数が列挙型を強制するのに静的コンス トラクターを使用します。
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

A***デストラクター***はクラスのインスタンスを消滅させるために必要なアクションを実装するメンバーです。 使用して、デストラクターを宣言、 *destructor_declaration*:

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

A *destructor_declaration*のセットを含めることができます*属性*([属性](attributes.md))。

*識別子*の*destructor_declaration*デストラクターが宣言されているクラスの名前を指定する必要があります。 その他の任意の名前を指定すると、コンパイル時エラーが発生します。

デストラクターの宣言が含まれています、`extern`修飾子は、デストラクターはモード、***外部デストラクター***します。 外部デストラクターの宣言は、実際の実装を提供しないため、その*destructor_body*をセミコロンで構成されます。 その他のすべてのデストラクターの*destructor_body*から成る、*ブロック*クラスのインスタンスを破棄するために実行するステートメントを指定します。 A *destructor_body*に対応して、 *method_body*を持つインスタンス メソッドの`void`型を返す ([メソッド本体](classes.md#method-body))。

デストラクターは継承されません。 したがって、クラスでは、そのクラスで宣言されたもの以外のデストラクターがありません。

デストラクターは、パラメーターがない必要があることはできません、オーバー ロードされたクラスに参照できるように、最大で 1 つのデストラクター。

デストラクターは、自動的に呼び出され、明示的に呼び出すことはできません。 インスタンスは、そのインスタンスを使用するには、どのコードが不要になったときに破壊の対象になります。 インスタンスのデストラクターの実行のインスタンスが破壊の対象になると、いつでも発生します。 順番に、そのインスタンスの継承チェーンでデストラクターが呼び出さインスタンスを破棄すると、ときにほとんどから派生する最低派生します。 デストラクターは、任意のスレッドで実行する場合があります。 タイミングとデストラクターの実行方法を制御するルールの詳細な説明については、次を参照してください。[自動メモリ管理](basic-concepts.md#automatic-memory-management)します。

この例の出力
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
デストラクターは、継承チェーンでは、順序で呼び出されるのでほとんどから派生する最低派生します。

デストラクターが仮想メソッドをオーバーライドすることによって実装される`Finalize`で`System.Object`します。 このメソッドをオーバーライドまたは (またはそのオーバーライド) を呼び出す c# プログラムは許可されていません直接します。 たとえば、プログラム
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
2 つのエラーが含まれています。

コンパイラは、このメソッドをおよび、上書きがまったく存在しないかのように動作します。 したがって、このプログラム:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
有効で非表示になりますが、メソッドに示すように、`System.Object`の`Finalize`メソッド。

動作の詳細については、デストラクターから例外がスローされるを参照してください[例外の処理方法](exceptions.md#how-exceptions-are-handled)します。

## <a name="iterators"></a>反復子

関数メンバー ([関数メンバー](expressions.md#function-members)) 反復子ブロックを使用して実装 ([ブロック](statements.md#blocks)) と呼ばれますが、***反復子***します。

反復子ブロックは関数に対応するメンバーの戻り値の型は列挙子インターフェイスの 1 つとして、関数メンバーの本文として使用可能性があります ([列挙子インターフェイス](classes.md#enumerator-interfaces)) または列挙可能なインターフェイスの 1 つ ([列挙可能なインターフェイス](classes.md#enumerable-interfaces))。 として発生する可能性が、 *method_body*、 *operator_body*または*accessor_body*イベント、インスタンス コンス トラクター、静的コンス トラクターおよびデストラクターがすることはできませんが、反復子として実装されます。

反復子ブロックを使用して関数のメンバーが実装されている場合は、いずれかを指定するメンバー関数の仮パラメーター リストのコンパイル時エラー`ref`または`out`パラメーター。

### <a name="enumerator-interfaces"></a>列挙子インターフェイス

***列挙子インターフェイス***非ジェネリック インターフェイスは`System.Collections.IEnumerator`ジェネリック インターフェイスのすべてのインスタンス化と`System.Collections.Generic.IEnumerator<T>`します。 説明を簡潔にするため、この章ではこれらのインターフェイスとして参照されます`IEnumerator`と`IEnumerator<T>`、それぞれします。

### <a name="enumerable-interfaces"></a>列挙可能なインターフェイス

***列挙可能なインターフェイス***非ジェネリック インターフェイスは`System.Collections.IEnumerable`ジェネリック インターフェイスのすべてのインスタンス化と`System.Collections.Generic.IEnumerable<T>`します。 説明を簡潔にするため、この章ではこれらのインターフェイスとして参照されます`IEnumerable`と`IEnumerable<T>`、それぞれします。

### <a name="yield-type"></a>Yield 型

反復子は、すべて同じ型の値のシーケンスを生成します。 この型が呼び出される、***型を生成***の反復子。

*  返す反復子の yield 型`IEnumerator`または`IEnumerable`は`object`します。
*  返す反復子の yield 型`IEnumerator<T>`または`IEnumerable<T>`は`T`します。

### <a name="enumerator-objects"></a>列挙子オブジェクト

反復子ブロックを使用して列挙子インターフェイス型を返す関数のメンバーが実装されている関数メンバーを呼び出しても反復子ブロックのコードはすぐに実行されません。 代わりに、***列挙子オブジェクト***が作成され、返されます。 このオブジェクトは、反復子ブロックで指定されたコードをカプセル化し、反復子ブロック内のコードの実行時に、列挙子オブジェクトの`MoveNext`メソッドが呼び出されます。 列挙子オブジェクトには、次の特徴があります。

*  実装`IEnumerator`と`IEnumerator<T>`ここで、`T`反復子の yield 型です。
*  このクラスは、`System.IDisposable` を実装します。
*  引数の値のコピーの初期化 (ある場合) および関数メンバーに渡されるインスタンスの値。
*  4 つの潜在的な状態が***する前に***、***を実行している***、***中断***、および***後***、最初に、***する前に***状態。

列挙子オブジェクトは、通常、反復子ブロック内のコードをカプセル化し、列挙子のインターフェイスを実装する列挙子のコンパイラによって生成されたクラスのインスタンスが他のメソッドの実装が可能です。 場合は、列挙子クラスは、コンパイラによって生成される、そのクラスは入れ子になった、直接的または間接的に、関数メンバーを含むクラスで private のアクセシビリティがおよびコンパイラを使用するために予約された名前になります ([識別子](lexical-structure.md#identifiers)).

列挙子オブジェクトには、上記で指定したものより多くのインターフェイスを実装できます。

次のセクションでは、説明の正確な動作、 `MoveNext`、`Current`と`Dispose`のメンバー、`IEnumerable`と`IEnumerable<T>`インターフェイスの列挙子オブジェクトによって提供される実装です。

列挙子オブジェクトはサポートされないことに注意してください、`IEnumerator.Reset`メソッド。 このメソッドを呼び出すと、`System.NotSupportedException`がスローされます。

#### <a name="the-movenext-method"></a>MoveNext メソッド

`MoveNext`列挙子オブジェクトのメソッドは、反復子ブロックのコードをカプセル化します。 呼び出す、`MoveNext`メソッド、反復子ブロックとセット内のコードの実行、`Current`として適切な列挙子オブジェクトのプロパティ。 実行した正確な動作`MoveNext`列挙子オブジェクトの状態によって異なるとき`MoveNext`が呼び出されます。

*  列挙子オブジェクトの状態が場合***する前に***呼び出すと、 `MoveNext`:
   * 状態を変更して***を実行している***します。
   * パラメーターを初期化します (など`this`) 引数の値とインスタンス値の列挙子オブジェクトが初期化されたときに保存するには、反復子ブロックの。
   * (後述) の実行が中断されるまでは、反復子ブロックを最初から実行します。
*  列挙子オブジェクトの状態が場合***を実行している***、呼び出しの結果`MoveNext`が指定されていません。
*  列挙子オブジェクトの状態が場合***中断***呼び出すと、 `MoveNext`:
   * 状態を変更して***を実行している***します。
   * 最後に、反復子ブロックの実行が中断したときに保存する値には、すべてのローカル変数とパラメーター (これを含む) の値を復元します。 これらの変数によって参照される任意のオブジェクトの内容は、MoveNext の以前の呼び出し以降に変更された可能性がありますに注意してください。
   * 直後に、次の反復子ブロックの実行を再開、`yield return`ステートメントを実行の中断の原因となった、(後述) の実行が中断されるまで継続します。
*  列挙子オブジェクトの状態が場合***後***呼び出すと、`MoveNext`返します`false`します。


ときに`MoveNext`反復子ブロックを実行します。 4 つの方法で実行を中断できます。によって、`yield return`ステートメントにより、`yield break`ステートメントでは、例外と、反復子ブロックの最後の発生によってがスローされ、反復子ブロックの外部で伝達されます。

*  ときに、`yield return`ステートメントが見つかりました ([yield ステートメント](statements.md#the-yield-statement))。
   * ステートメントで指定された式が評価、暗黙的に、yield 型に変換されに割り当てられている、`Current`列挙子オブジェクトのプロパティ。
   * 反復子本体の実行が中断されます。 すべてのローカル変数とパラメーターの値 (など`this`) のこの場所は、保存は`yield return`ステートメント。 場合、`yield return`内で 1 つまたは複数のステートメントが`try`ブロック、関連付けられている`finally`ブロックは、この時点では実行されません。
   * 列挙子オブジェクトの状態に変更***中断***します。
   * `MoveNext`メソッドを返します。`true`イテレーションは、次の値に正常に進んだことを示す、呼び出し元にします。
*  ときに、`yield break`ステートメントが見つかりました ([yield ステートメント](statements.md#the-yield-statement))。
   * 場合、`yield break`内で 1 つまたは複数のステートメントが`try`ブロック、関連付けられている`finally`ブロックが実行されます。
   * 列挙子オブジェクトの状態に変更***後***します。
   * `MoveNext`メソッドを返します。`false`イテレーションが完了したことを示す、呼び出し元にします。
*  ときに、反復子本体の末尾が発生しました。
   * 列挙子オブジェクトの状態に変更***後***します。
   * `MoveNext`メソッドを返します。`false`イテレーションが完了したことを示す、呼び出し元にします。
*  ときに例外がスローされ、反復子ブロックの外部で伝達します。
   * 適切な`finally`反復子本体のブロックが例外の反映によって実行されるは。
   * 列挙子オブジェクトの状態に変更***後***します。
   * 例外の反映の呼び出し元は引き続き、`MoveNext`メソッド。

#### <a name="the-current-property"></a>現在のプロパティ

列挙子オブジェクトの`Current`によってプロパティの影響を受ける`yield return`反復子ブロック内のステートメント。

列挙子オブジェクトの場合、***中断***状態では、値`Current`以前の呼び出しによって設定された値は、`MoveNext`します。 列挙子オブジェクトの場合、***する前に***、***を実行している***、または***後***へのアクセスの結果が示す`Current`が指定されていません。

以外の入力を yield を使った反復子のため`object`へのアクセス結果`Current`で列挙子オブジェクトの`IEnumerable`へのアクセスに対応する実装`Current`で列挙子オブジェクトの`IEnumerator<T>`実装と、その結果をキャスト`object`します。

#### <a name="the-dispose-method"></a>Dispose メソッド

`Dispose`メソッドを使用列挙子オブジェクトを導入することで、イテレーションのクリーンアップを***後***状態。

*  列挙子オブジェクトの状態が場合***する前に***呼び出すと、`Dispose`状態を変更して***後***。
*  列挙子オブジェクトの状態が場合***を実行している***、呼び出しの結果`Dispose`が指定されていません。
*  列挙子オブジェクトの状態が場合***中断***呼び出すと、 `Dispose`:
   * 状態を変更して***を実行している***します。
   * いずれかの finally ブロックを実行、最後に実行された場合、`yield return`ステートメントが、`yield break`ステートメント。 これによって、例外がスローされ、反復子本体の外部で伝達する場合、列挙子オブジェクトの状態に設定されて***後***の呼び出し元に例外が伝達されると、`Dispose`メソッド。
   * 状態を変更して***後***します。
*  列挙子オブジェクトの状態が場合***後***呼び出すと、`Dispose`効力はなくなりました。

### <a name="enumerable-objects"></a>列挙可能なオブジェクト

反復子ブロックを使用して、列挙可能なインターフェイスの型を返す関数のメンバーが実装されている関数メンバーを呼び出しても反復子ブロックのコードはすぐに実行されません。 代わりに、***列挙可能なオブジェクト***が作成され、返されます。 列挙可能なオブジェクトの`GetEnumerator`メソッドを返します、反復子ブロックで指定された列挙子オブジェクトをコードをカプセル化して、反復子ブロック内のコードの実行が発生したときに列挙子オブジェクトの`MoveNext`メソッドが呼び出されます。 列挙可能なオブジェクトには、次の特徴があります。

*  実装`IEnumerable`と`IEnumerable<T>`ここで、`T`反復子の yield 型です。
*  引数の値のコピーの初期化 (ある場合) および関数メンバーに渡されるインスタンスの値。

列挙可能なオブジェクトは、通常の反復子ブロック内のコードをカプセル化し、列挙可能なインターフェイスを実装するための列挙可能なクラスがコンパイラによって生成されたインスタンスが、他のメソッドの実装が可能です。 場合は、列挙可能なクラスは、コンパイラによって生成される、そのクラスが入れ子にする、直接または間接的には、関数メンバーを含むクラスで private のアクセシビリティがおよびコンパイラを使用するために予約された名前になります ([識別子](lexical-structure.md#identifiers)).

列挙可能なオブジェクトは、上記で指定したものよりもいないインターフェイスを実装することができます。 具体的には、列挙可能なオブジェクトも実装`IEnumerator`と`IEnumerator<T>`、列挙型と列挙子の両方として機能するようにします。 このような実装では、最初の時間、列挙可能なオブジェクトの`GetEnumerator`メソッドが呼び出される列挙可能なオブジェクトが返されます。 列挙可能なオブジェクトの後に呼び出さ`GetEnumerator`列挙可能なオブジェクトのコピーを返す場合は、します。 したがって、各エラーは、列挙子が独自の状態と、1 つの列挙子で変更が別に影響することはありません返されます。

#### <a name="the-getenumerator-method"></a>GetEnumerator メソッド

列挙可能なオブジェクトの実装を提供する、`GetEnumerator`のメソッド、`IEnumerable`と`IEnumerable<T>`インターフェイス。 2 つ`GetEnumerator`メソッドを取得し、使用可能な列挙子オブジェクトを取得する一般的な実装を共有します。 列挙子オブジェクトが引数の値で初期化され、インスタンスの列挙可能なオブジェクトが初期化されますが、それ以外の場合に保存された値で説明されているように機能[列挙子オブジェクト](classes.md#enumerator-objects)します。

### <a name="implementation-example"></a>実装例

このセクションでは、標準 c# コンストラクトの観点からの反復子の実装について説明します。 ここで説明されている実装 Microsoft c# コンパイラで使用される同じ原則に基づいていますが、規制に準拠した実装または使用可能な 1 つだけではではありません。

次`Stack<T>`クラスが実装するその`GetEnumerator`メソッド、反復子を使用します。 反復子は、上から下へのスタックの要素を列挙します。

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

`GetEnumerator`メソッドは、次に示すように、反復子ブロックでコードをカプセル化する列挙子のコンパイラによって生成されたクラスのインスタンス化に変換できます。

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

反復子ブロック内のコードをステート マシンに変換されに配置前の変換で、`MoveNext`列挙子クラスのメソッド。 さらに、ローカル変数`i`の呼び出し間での存在を続行できるように、列挙子オブジェクトのフィールドになって`MoveNext`します。

次の例では、整数 1 ~ 10 の単純な掛け算表を印刷します。 `FromTo`メソッドの例では列挙可能なオブジェクトを返し、反復子を使用して実装されます。

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

`FromTo`メソッドは、次に示すように、反復子ブロック内のコードをカプセル化するための列挙可能なクラスがコンパイラによって生成されたインスタンス化に変換できます。

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

列挙可能なクラスでは、列挙可能なインターフェイスと列挙型および列挙子の両方として機能できるように、列挙子インターフェイスの両方を実装します。 最初に、`GetEnumerator`メソッドが呼び出される列挙可能なオブジェクトが返されます。 列挙可能なオブジェクトの後に呼び出さ`GetEnumerator`列挙可能なオブジェクトのコピーを返す場合は、します。 したがって、各エラーは、列挙子が独自の状態と、1 つの列挙子で変更が別に影響することはありません返されます。 `Interlocked.CompareExchange`メソッドを使用して、スレッド セーフ操作を確認します。

`from`と`to`パラメーターは列挙可能なクラスのフィールドに変換されます。 `from`反復子ブロックでは、追加で変更が`__from`に指定された初期値を保持するフィールドが導入されました`from`各列挙子にします。

`MoveNext`メソッドがスローされます、`InvalidOperationException`ときに呼び出された場合`__state`は`0`します。 これで最初に呼び出さず、列挙子オブジェクトとしての列挙可能なオブジェクトの使用に対して保護`GetEnumerator`します。

次の例では、単純なツリーのクラスを示します。 `Tree<T>`クラスが実装するその`GetEnumerator`メソッド、反復子を使用します。 反復子は、infix ツリーの要素を列挙します。

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

`GetEnumerator`メソッドは、次に示すように、反復子ブロックでコードをカプセル化する列挙子のコンパイラによって生成されたクラスのインスタンス化に変換できます。

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

使用されるコンパイラによって生成された一時、`foreach`にステートメントが無効になる、`__left`と`__right`列挙子オブジェクトのフィールド。 `__state`列挙子オブジェクトのフィールドは、慎重に更新できるように、正しい`Dispose()`メソッドが正しく呼び出される場合は、例外がスローされます。 翻訳された単純なコードを記述することはできませんに注意してください。`foreach`ステートメント。

## <a name="async-functions"></a>非同期関数

メソッド ([メソッド](classes.md#methods)) または匿名関数 ([匿名関数式](expressions.md#anonymous-function-expressions)) で、`async`修飾子と呼ばれる、***非同期関数***します。 一般に、用語***async***任意の種類を持つ関数の記述に使用される、`async`修飾子。

いずれかを指定する非同期関数の仮パラメーター リストと、コンパイル時エラー`ref`または`out`パラメーター。

*Return_type* 、非同期のメソッドはいずれかのことがあります`void`または***タスクの種類***します。 タスクの種類は`System.Threading.Tasks.Task`から型を構築および`System.Threading.Tasks.Task<T>`します。 説明を簡潔にするため、この章ではこれらの型として参照されます`Task`と`Task<T>`、それぞれします。 タスク型を返す非同期メソッドは、タスクを返すと言います。

タスクの種類の正確な定義が定義されている、実装が、言語の観点から、タスクの型がのいずれか、不完全な状態が成功したか、エラーが発生しました。 エラーが発生したタスクは、関連する例外を記録します。 成功、`Task<T>`型の結果を記録`T`します。 タスクの種類は待機可能なとのオペランドは、そのため、await 式 ([Await 式](expressions.md#await-expressions))。

非同期関数の呼び出しが評価を停止することでは、await 式 ([Await 式](expressions.md#await-expressions)) 本体内にします。 評価を中断時点では後で再開が await 式で、***再開デリゲート***します。 再開のデリゲートは型`System.Action`、非同期関数の呼び出しの評価が箇所の await 式から再開されるメソッドが呼び出されたときにします。 ***現在の呼び出し元***非同期関数の呼び出しは最初の呼び出し元関数の呼び出しは中断されていることはない場合、または再開デリゲートの最新の呼び出し元それ以外の場合。

### <a name="evaluation-of-a-task-returning-async-function"></a>非同期タスクを返す関数の評価

タスクを返す非同期関数の呼び出しにより、生成するタスクが返される型のインスタンス。 これと呼ばれますが、***タスクを返す***の async 関数。 タスクは、不完全な状態の初期段階です。

非同期関数の本体は評価されます (await 式に到達) によって中断されていたか、またはが終了するまでタスクを返すと、呼び出し元にコントロールのポイントが返されます。

非同期関数の本体が終了すると、タスクを返すに不完全な状態から移動されます。

*  Return ステートメントまたは本体の末尾に到達の結果として、関数本体が終了すると、成功の状態に、戻り値のタスクで、結果の値が記録されます。
*  キャッチされない例外の結果として、関数本体が終了しているかどうか ([throw ステートメント](statements.md#the-throw-statement)) エラーが発生した状態に戻り、タスクに例外が記録されます。

### <a name="evaluation-of-a-void-returning-async-function"></a>Async void を返す関数の評価

非同期関数の戻り値の型がある場合`void`、次のように、上記とは異なる評価。関数が完了と、現在のスレッドの例外に伝える代わりにタスクが返されないため***同期コンテキスト***します。 同期コンテキストの正確な定義は実装に依存しますが、表現である"where"現在のスレッドが実行されているのです。 同期コンテキストは、async void を返す関数の評価の開始が正常に完了すると、またはキャッチされない例外がスローされると、ときに通知されます。

これにより、コンテキスト、その下にある数の void を返す非同期関数が実行されているを追跡して、それらから例外を伝達する方法を決定できます。
