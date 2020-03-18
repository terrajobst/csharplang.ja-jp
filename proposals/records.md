---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2019
ms.locfileid: "79483878"
---
# <a name="records"></a>records

* [x] が提案されています
* [] プロトタイプ:[完了](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)
* [] の実装:[実行](https://github.com/dotnet/roslyn/BRANCH_NAME)中
* [] 仕様: ドラフト仕様を囲みます

## <a name="summary"></a>まとめ
[summary]: #summary

レコードは、クラス型および構造体型C#の新しい単純な宣言形式で、多数の単純な機能の利点を組み合わせることができます。 ここでは、新しい機能 (呼び出し元のパラメーターと *-expression*s) について説明し、レコード宣言の構文とセマンティクスを指定して、いくつかの例を示します。


## <a name="motivation"></a>目的
[motivation]: #motivation

のC#型宣言の多くは、型指定されたデータの集約コレクションよりもわずかです。 残念ながら、このような型を宣言するには、大量の定型コードが必要です。 *レコード*は、集計のメンバーと追加のコード、または通常の定型句からの偏差 (存在する場合) を記述することによって、データ型を宣言するためのメカニズムを提供します。

以下の[例](#examples)を参照してください。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a>呼び出し元-受信側パラメーター

現在、メソッドパラメーターの*既定の引数*はである必要があります 
- *定数式*です。もしくは
- `S` が値型の `new S()` 形式の式。もしくは
- `S` が値型である `default(S)` フォームの式

これを拡張して次のものを追加します。
- フォームの式 `this.Identifier`

この新しい形式は、*呼び出し元と受信側の既定の引数*と呼ばれ、次のすべてが満たされている場合にのみ使用できます。
- このメソッドが表示されるメソッドは、インスタンスメソッドです。そして
- 式 `this.Identifier`、それを囲む型のインスタンスメンバーにバインドされます。これは、フィールドまたはプロパティである必要があります。そして
- バインド先のメンバー (およびプロパティである場合は `get` アクセサー) は、少なくともメソッドと同じようにアクセスできます。そして
- `this.Identifier` の型は、id または null 許容型からパラメーターの型への変換によって暗黙的に変換できます (これは*既定の引数*に対する既存の制約です)。

*呼び出し元の既定の引数*を使用して、対応する省略可能なパラメーターの関数メンバーを呼び出すことで引数を省略した場合、受信側のメンバーの値は暗黙的に渡されます。 

> **設計メモ**: 呼び出し元のパラメーターの主な理由は、 *with 式*をサポートするためです。 これは、次のようにメソッドを宣言できるということです。
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> 次のように使用します。
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> 既存の `Point` と同じように新しい `Point` を作成し `X` の値を変更した場合。
> 
> 呼び出し元のパラメーターがサポートされている場合に、*式*の構文形式を追加する必要があるかどうかにかかわらず、未解決の質問になります。したがって、 *with 式* *に加え*て*ではなく、これを*行うことができます。

- []**オープン懸案事項**:*呼び出し元の既定の引数*が他の引数に対して評価される順序は何ですか? 指定されていない場合は、

### <a name="with-expressions"></a>式の使用

新しい式の形式が提案されます。

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

`with` トークンは、新しい状況依存のキーワードです。

*With_initializer*の左側の各*識別子*は、アクセス可能なインスタンスフィールドまたは*with_expression*の*primary_expression*の型のプロパティにバインドする必要があります。 指定された*with_expression*のこれらの識別子の間に重複する名前がない可能性があります。

フォームの*with_expression*

> *e1* `with` `{`*識別子* = *e2*,... `}`

は、フォームの呼び出しとして扱われます。

> *e1*`.With(`*identifier2*`:` *e2*,...`)`

ここでは、`With` という名前の各メソッドに対して、 *e1*のアクセス可能なインスタンスメンバーである場合、 *identifier2*を、そのメソッドの最初のパラメーターの名前として選択します。このメソッドは、*識別子*にバインドされたインスタンスフィールドまたはプロパティと同じメンバーである呼び出し側のパラメーターを持ちます。 そのようなパラメーターを特定できない場合は、メソッドが考慮から除外されます。 呼び出されるメソッドは、オーバーロードの解決によって残りの候補の中から選択されます。

> **設計メモ**: 呼び出し元のパラメーターを指定すると、この特殊な構文形式を使用しなくても、*式*の多くの利点が得られます。 このため、必要かどうかを検討しています。 主な利点は、パラメーターの名前ではなく、フィールドとプロパティの名前を使用してプログラムをプログラミングできることです。 この方法では、読みやすさとツールの品質の両方が向上します (たとえば、 *with_expression*の識別子に対する "定義への移動" は、メソッドパラメーターではなく、プロパティに移動します)。

- []**未解決の問題**: 拡張メソッドをサポートするには、この説明を変更する必要があります。

### <a name="pattern-matching"></a>パターン一致

`Deconstruct` の仕様とパターンマッチングとの関係については、[パターンマッチングの仕様](csharp-8.0/patterns.md#positional-pattern)を参照してください。

> **設計メモ**: ここで指定されているコンパイラで生成された `Deconstruct` と、パターンマッチングの仕様 (レコードの宣言)
> ```cs
> public class Point(int X, int Y);
> ```
> では、次のように位置パターン一致がサポートされます。
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a>レコード型の宣言

`class` または `struct` 宣言の構文は、値パラメーターをサポートするために拡張されています。パラメーターは、型のプロパティになります。

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> **設計**上の注意: レコードの種類は、クラス本体で明示的に宣言されたメンバーを必要としない場合に便利です。そのため、宣言の構文を変更して、本文が単にセミコロンになるようにします。

レコード*パラメーター*を使用して宣言されたクラス (構造体) はレコード*クラス*(*レコード構造体*) と呼ばれ、そのいずれかが*レコード型*です。

- [] **Open issue**: レコード型宣言内に表示できるように、文法に*primary_constructor_body*を含める必要があります。
- []**未解決の問題**: パラメーター名の名前の競合ルールは何ですか? 1つの型パラメーターまたは別の*レコードパラメーター*との競合は許可されていません。
- []**未解決の問題**: レコードパラメーターのスコープを指定する必要があります。 使用できる場所 インスタンスフィールド初期化子と*primary_constructor_body*少なくともであるとします。
- [] **Open issue**: レコードの種類の宣言を部分的に使用できますか? その場合、各部分でパラメーターを繰り返す必要がありますか。

#### <a name="members-of-a-record-type"></a>レコードの種類のメンバー

レコード型には、*クラス本体*で宣言されたメンバーに加えて、次の追加メンバーがあります。

##### <a name="primary-constructor"></a>プライマリコンストラクター

レコード型には、型宣言の値パラメーターに対応するシグネチャを持つ `public` コンストラクターがあります。 これは、型の*プライマリコンストラクター*と呼ばれ、暗黙的に宣言された*既定のコンストラクター*は抑制されます。

実行時のプライマリコンストラクター

* 値パラメーターに対応するプロパティについて、コンパイラによって生成されるバッキングフィールドを初期化します (これらのプロパティがコンパイラによって指定されている場合)。[「1.1.2」を参照してください](#1.1.2))。そうしたら
* *クラス本体*に出現するインスタンスフィールド初期化子を実行します。それから
* 基底クラスのコンストラクターを呼び出します。
    * *Record_base_arguments*に引数がある場合は、これらの引数を使用してオーバーロードの解決によって選択された基本コンストラクターが呼び出されます。
    * それ以外の場合は、引数なしで基底コンストラクターが呼び出されます。
* 各*primary_constructor_body*の本文をソースの順序で実行します。

- []**未解決の問題**: 特にパーシャルのコンパイル単位全体で、その順序を指定する必要があります。
- []**未解決の問題**: 明示的に宣言されたすべてのコンストラクターがプライマリコンストラクターにチェーンされる必要があることを指定する必要があります。
- [] **Open issue**: プライマリコンストラクターでアクセス修飾子を変更することが許可されているかどうか。
- [] **Open issue**: レコード構造体では、レコードパラメーターがないというエラーが発生します。

##### <a name="primary-constructor-body"></a>プライマリコンストラクター本体

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

*Primary_constructor_body*は、レコード型の宣言内でのみ使用できます。 *Primary_constructor_body*の*識別子*は、宣言されているレコードの種類に名前を指定する必要があります。

*Primary_constructor_body*は、自身でメンバーを宣言するのではなく、プログラマがレコード型のプライマリコンストラクターの属性を提供し、そのアクセスを指定する方法です。 また、レコードの種類のインスタンスが構築されたときに実行されるコードを追加することもできます。

- []**未解決の問題**: 構造体の既定のコンストラクターがこれをバイパスすることに注意してください。
- []**未解決の問題**: 初期化の実行順序を指定する必要があります。
- []**オープン問題**: レコード型以外の宣言で (属性や修飾子を使用せずに) *primary_constructor_body*のような処理を許可し、インスタンスフィールド初期化子のコードと同じように扱う必要がありますか。

##### <a name="properties"></a>Properties

レコード型宣言の各レコードパラメーターには、対応する `public` プロパティメンバーがあります。このメンバーの名前と型は、値パラメーターの宣言から取得されます。 この名前は、 *record_property_name*の*識別子*、存在する場合は*record_parameter*の識別子、それ以外の場合は*の識別子です*。 `get` アクセサーを持つ具象 (つまり非抽象) パブリックプロパティが明示的に宣言または継承されていない場合は、コンパイラによって次のように生成されます。

* *レコード構造体*または `sealed`*レコードクラス*の場合:
 * `private` `readonly` フィールドは、`readonly` プロパティのバッキングフィールドとして生成されます。 この値は、対応するプライマリコンストラクターパラメーターの値を使用して構築時に初期化されます。
 * バッキングフィールドの値を返すために、プロパティの `get` アクセサーが実装されています。
 * 各 "一致する" 継承された仮想プロパティの `get` アクセサーはオーバーライドされます。

> **デザインメモ**: つまり、基本クラスを拡張する場合、またはレコードパラメーターと同じ名前と型を持つパブリック抽象プロパティを宣言するインターフェイスを実装する場合は、そのプロパティがオーバーライドまたは実装されます。

- [] **Open issue**: 明示的に宣言されている場合、プロパティのアクセス修飾子を変更できるようにする必要がありますか。
- []**未解決の問題**: プロパティのフィールドを置き換えることができるかどうか。

##### <a name="object-methods"></a>オブジェクト メソッド

*レコード構造体*または `sealed`*レコードクラス*では、ユーザーが指定しない限り、`object.GetHashCode()` および `object.Equals(object)` メソッドの実装がコンパイラによって生成されます。

- []**未解決の問題**: 実装を正確に指定する必要があります。
- []**未解決の問題**: レコードの種類に `IEquatable<T>` インターフェイスを追加し、実装が提供されることを指定する必要があります。
- []**未解決の問題**: すべての `IEquatable<T>.Equals`を実装するように指定する必要もあります。
- []**オープン問題**: レコードの継承の面で、Equals の問題の解決方法を正確に指定する必要があります。具体的には、対称、推移、再帰などの等値メソッドを生成する方法を示します。
- []**未解決の問題**: レコードの種類の `operator ==` と `operator !=` を実装することが提案されています。
- []**未解決の問題**: `object.ToString`の実装を自動生成しますか?

##### `Deconstruct`

レコード型には、ユーザーによって署名が指定されていない限り、コンパイラによって生成される `public` メソッド `void Deconstruct` があります。 各パラメーターは、レコード型の対応するパラメーターと同じ名前および型の `out` パラメーターです。 コンパイラによって提供されるこのメソッドの実装では、各 `out` パラメーターに対応するプロパティの値を割り当てる必要があります。

`Deconstruct`のセマンティクスについては[、パターンマッチングの仕様](csharp-8.0/patterns.md#positional-pattern)を参照してください。

##### <a name="with-method"></a>`With` メソッド

`With` という名前のユーザー宣言メンバーが宣言されていない限り、レコード型には、その戻り値の型がレコード型自体である `With` という名前のコンパイラが提供するメソッドがあり、これらのパラメーターがレコード型宣言に出現する順序で各*レコードパラメーター*に対応する1つの値パラメーターが含まれています。 各パラメーターには、対応するプロパティの*呼び出し元と受信*側の既定の引数が必要です。

`abstract` レコードクラスでは、コンパイラによって提供される `With` メソッドは abstract です。 レコード構造体または sealed レコードクラスでは、コンパイラによって提供される `With` メソッドが `sealed`ます。 それ以外の場合、コンパイラによって提供される `With` メソッドは ' 仮想であり、その実装では、パラメーターを引数として使用してプライマリコンストラクターを呼び出し、パラメーターから新しいインスタンスを作成し、その新しいインスタンスを返すことによって生成される新しいインスタンスを返す必要があります。

- []**オープンの問題**: オーバーライドする条件、または実装されたインターフェイスから継承された仮想 `With` メソッドまたは `With` メソッドを実装する条件の下でも指定する必要があります。
- []**未解決の問題**: 非仮想 `With` メソッドを継承するとどうなるかを説明する必要があります。

> **設計メモ**: レコードの種類は既定では不変であるため、`With` メソッドを使用すると、既存のインスタンスと同じで、新しい値を指定したプロパティを持つ新しいインスタンスを作成することができます。 たとえば、次のように指定します。
> ```cs
> public class Point(int X, int Y);
> ```
> コンパイラによって指定されたメンバーが存在します
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> これにより、レコードの種類の変数が有効になります。
> ```cs
> var p = new Point(3, 4);
> ```
> 1つ以上のプロパティが異なるインスタンスに置き換える場合
> ```cs
>     p = p.With(X: 5);
> ```
> これは、 *with_expression*を使用して表すこともできます。
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a>例

#### <a name="compatibility-of-record-types"></a>レコードの種類の互換性

プログラマはレコードの種類の宣言にメンバーを追加できるので、多くの場合、既存のクライアントに影響を与えずにレコードの要素のセットを変更することができます。 たとえば、レコード型の初期バージョンが指定されているとします。

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

レコード型の新しい要素は、バイナリまたはソースの互換性に影響を与えることなく、型の次のリビジョンで方に追加できます。

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a>レコード構造体の例

このレコード構造体

```cs
public struct Pair(object First, object Second);
```

このコードに変換されます

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- [] **Open issue**: Equals (pair other) の実装がペアのパブリックメンバーである必要がありますか?
- []**未解決の懸案事項**: 継承の場合、この `Equals` の実装は対称ではありません。
- [] **Open issue**: レコードが `operator ==` を宣言し、`operator !=`しますか?

> **設計メモ**: 1 つのレコードの種類は別のレコード型から継承でき、この `Equals` の実装は対称ではないため、正しくありません。 ここでは、次のように等価性を実装することを提案します。
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> 派生レコードは `override EqualityContract`します。 代替手段として、継承を制限する方が適しています。

#### <a name="sealed-record-example"></a>シールされたレコードの例

このシールされたレコードクラス

```cs
public sealed class Student(string Name, decimal Gpa);
```

は、次のコードに変換されます。

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a>abstract record クラスの例

この抽象レコードクラス

```cs
public abstract class Person(string Name);
```

は、次のコードに変換されます。

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a>抽象レコードとシールレコードの結合

上記の抽象レコードクラス `Person`、この sealed レコードクラスを指定します。

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

は、次のコードに変換されます。

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

他の言語機能と同様に、この機能の恩恵を受けるプログラムのC#本文に対して、さらに複雑な言語を端的にする必要があるかどうかを質問する必要があります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

6では、*プライマリコンストラクター*の追加をC#検討しています。 この提案と同じ構文サーフェイスが利用されていますが、レコードによって提供される利点が不足していることがわかりました。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

オープンな質問は、提案の本文に表示されます。

## <a name="design-meetings"></a>会議のデザイン

TBD
