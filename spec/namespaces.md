# <a name="namespaces"></a>名前空間

C# プログラムの名前空間を使用して構成します。 名前空間は、"external"組織のシステムおよびプログラムは、「内部」組織システムとして使用されます-その他のプログラムに公開されているプログラム要素を表すため。

Using ディレクティブ ([ディレクティブを使用して](namespaces.md#using-directives)) 名前空間の使用を容易に提供されます。

## <a name="compilation-units"></a>コンパイル単位

A *compilation_unit*ソース ファイルの全体的な構造を定義します。 0 個以上のコンパイル単位で構成されます*using_directive*s が 0 個以上続く*global_attributes* 0 個以上続く*namespace_member_declaration*s.

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

C# プログラムは 1 つまたは複数のコンパイル単位で、別のソース ファイルに含まれている各。 C# プログラムがコンパイルされると、すべてのコンパイル単位が一度に処理されます。 そのため、コンパイル単位は互いに依存して、可能性があります、循環形式で。

*Using_directive*コンパイル ユニットに影響の s、 *global_attributes*と*namespace_member_declaration*、そのコンパイル ユニットの s があるない影響その他のコンパイル ユニット。

*Global_attributes* ([属性](attributes.md)) コンパイル単位のターゲット アセンブリとモジュール属性の指定を許可します。 アセンブリとモジュールは、型の物理的なコンテナーとして機能します。 いくつかの物理的に独立したモジュールのアセンブリがあります。

*Namespace_member_declaration*プログラムのコンパイル単位ごとの s がグローバル名前空間と呼ばれる 1 つの宣言領域にメンバーを投稿します。 例:

ファイル`A.cs`:
```csharp
class A {}
```

ファイル`B.cs`:
```csharp
class B {}
```

この場合、完全修飾名を持つ 2 つのクラスを宣言する 1 つのグローバル名前空間を 2 つのコンパイル単位が投稿`A`と`B`します。 同じ宣言領域に 2 つのコンパイル単位がドキュメントに投稿されているとエラーと同じ名前のメンバーの宣言をそれぞれに含まれる場合。

## <a name="namespace-declarations"></a>名前空間の宣言

A *namespace_declaration*キーワードから成る`namespace`、名前空間の名前と本文で後に、必要に応じて後にセミコロンが。

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

A *namespace_declaration*で最上位レベルの宣言と発生する可能性があります、 *compilation_unit*または別のメンバーの宣言として*namespace_declaration*します。 ときに、 *namespace_declaration*で最上位レベルの宣言と発生する、 *compilation_unit*、名前空間はグローバル名前空間のメンバーになります。 ときに、 *namespace_declaration*別内で発生*namespace_declaration*、内部の名前空間が外側の名前空間のメンバーになります。 どちらの場合、名前空間の名前が含まれる名前空間内で一意であります。

名前空間は、暗黙的に`public`と名前空間の宣言は、アクセス修飾子を含めることはできません。

内で、 *namespace_body*、省略可能な*using_directive*s が他の名前空間、型を修飾名ではなく直接参照できるように、メンバーの名前をインポートします。 省略可能な*namespace_member_declaration*s が、名前空間の宣言領域にメンバーを投稿します。 なおすべて*using_directive*s は、メンバー宣言の前に表示する必要があります。

*Qualified_identifier*の*namespace_declaration* 、単一の識別子またはシーケンスで区切られた識別子の場合があります"`.`"トークンです。 後者の形式は、構文的に複数の名前空間宣言を入れ子せず、入れ子になった名前空間を定義するプログラムを許可します。 例えば以下のようにします。

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
同じ意味には
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

名前空間は拡張可能なと同じ完全修飾名で 2 つの名前空間宣言が同じ宣言領域に投稿 ([宣言](basic-concepts.md#declarations))。 例
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
ここで完全修飾の名前を持つ 2 つのクラスを宣言する、同じ宣言領域に 2 つの名前空間宣言が貢献`N1.N2.A`と`N1.N2.B`します。 2 つの宣言は、同じ宣言領域に追加、ためにされていた場合は、各エラーには、同じ名前のメンバーの宣言が含まれています。

## <a name="extern-aliases"></a>Extern エイリアス

*Extern_alias_directive*名前空間のエイリアスとして機能する識別子が導入されています。 エイリアスの名前空間の仕様では、プログラムのソース コードの外部にあるし、別名の名前空間の入れ子になった名前空間にも適用されます。

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

スコープ、 *extern_alias_directive*にわたる、 *using_directive*s、 *global_attributes*と*namespace_member_declaration*s がすぐに親のコンパイル単位または名前空間本文。

含むコンパイル単位または名前空間本体内で、 *extern_alias_directive*で導入された識別子、 *extern_alias_directive*エイリアスの名前空間を参照するために使用できます。 コンパイル時エラーには、*識別子*という単語を`global`します。

*Extern_alias_directive*エイリアス使用可能な特定のコンパイル単位または名前空間本体の内部は、基になる宣言領域に新しいメンバーは含まれませんが、します。 つまり、 *extern_alias_directive* 、推移的ではありませんが、コンパイル単位または名前空間本文のみが発生するのではなく、影響を与えます。

次のプログラムを宣言し、2 つの extern エイリアスを使用して`X`と`Y`、それぞれの個別の名前空間階層のルートを表します。
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

プログラムは、extern の存在のエイリアスを宣言します`X`と`Y`がエイリアスの実際の定義は、外部プログラムにします。 同じ名前を持つ`N.B`クラスとして参照できます`X.N.B`と`Y.N.B`、または、名前空間エイリアス修飾子を使って`X::N.B`と`Y::N.B`します。 エラーは、プログラムは、外部定義が提供 extern エイリアスを宣言する場合に発生しません。

## <a name="using-directives"></a>using ディレクティブ

***Using ディレクティブ***名前空間および他の名前空間で定義された型の使用を容易にします。 ディレクティブへの影響の名前解決プロセスを使用して*namespace_or_type_name*s ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names)) と*simple_name*s ([簡易名](expressions.md#simple-names))、ディレクティブを使用して、宣言とは異なり、新しいメンバーのコンパイル単位または使用される名前空間の基になる宣言スペースには影響しません。

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

A *using_alias_directive* ([Using エイリアス ディレクティブ](namespaces.md#using-alias-directives)) 名前空間または型のエイリアスが導入されています。

A *using_namespace_directive* ([名前空間ディレクティブを使用して](namespaces.md#using-namespace-directives)) 名前空間の型のメンバーをインポートします。

A *using_static_directive* ([Using static ディレクティブ](namespaces.md#using-static-directives))、入れ子にされた型と型の静的メンバーをインポートします。

スコープを*using_directive*にわたる、 *namespace_member_declaration*s がすぐに親のコンパイル単位または名前空間本文。 スコープを*using_directive*具体的には、ピアは含まれません*using_directive*秒。 したがって、ピア*using_directive*s が、互いに影響しませんし、記述されている順序が重要ではありません。

### <a name="using-alias-directives"></a>Using エイリアス ディレクティブ

A *using_alias_directive*名前空間またはすぐ外側のコンパイル単位または名前空間の本文内の型のエイリアスとして機能する識別子が導入されています。

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

メンバー宣言が含まれるコンパイル単位または名前空間本文内で、 *using_alias_directive*で導入された識別子、 *using_alias_directive*使用できる参照を特定名前空間または型。 例:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

上記のメンバー宣言内で、`N3`名前空間、`A`の別名です`N1.N2.A`、クラス`N3.B`クラスから派生`N1.N2.A`します。 別名を作成して、同じ効果を取得できる`R`の`N1.N2`し、参照する`R.A`:
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

*識別子*の*using_alias_directive*コンパイル単位またはすぐに含む名前空間の宣言領域内で一意である必要があります、 *using_alias_directive*. 例:
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

上記`N3`メンバーが既に含まれています`A`ので、コンパイル時エラーには、 *using_alias_directive*その識別子を使用します。 同様に、2 つ以上のコンパイル時エラーが*using_alias_directive*同じコンパイル単位または名前空間に同じ名前のエイリアスを宣言する本体で s。

A *using_alias_directive*エイリアス使用可能な特定のコンパイル単位または名前空間本体の内部は、基になる宣言領域に新しいメンバーは含まれませんが、します。 つまり、 *using_alias_directive*は推移的ではありませんが、コンパイル単位または名前空間本文のみが発生するのではなく影響を与えます。 例
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
スコープ、 *using_alias_directive*導入する`R`のみが含まれる名前空間のメンバー宣言に対する拡張ように`R`が 2 つ目の名前空間宣言で不明です。 ただし、配置、 *using_alias_directive*含むコンパイル単位が両方の名前空間宣言内で使用可能になるエイリアス。
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

通常のメンバーと同様で導入された名前*using_alias_directive*s は入れ子になったスコープの同じ名前のメンバーによって非表示にします。 例
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
参照を`R.A`の宣言で`B`ので、コンパイル時エラーが発生`R`を指す`N3.R`ではなく、`N1.N2`します。

順序*using_alias_directive*有意性、およびの解像度を持たない s が書き込まれる、 *namespace_or_type_name*によって参照される、 *using_alias_directive*は影響されません、 *using_alias_directive*自体またはその他*using_directive*コンパイル単位または名前空間の本体は、コンテナーで s。 つまり、 *namespace_or_type_name*の*using_alias_directive*がコンパイル単位または名前空間の本体は、すぐに親があるないかのように解決される*using_directive*秒。 A *using_alias_directive*しかし、影響を受ける*extern_alias_directive*コンパイル単位または名前空間の本体は、コンテナー内。 例
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
最後の*using_alias_directive*最初に影響を受けないため、コンパイル時エラー結果*using_alias_directive*します。 最初の*using_alias_directive* extern エイリアスのスコープから、エラーが発生しない`E`が含まれています、 *using_alias_directive*します。

A *using_alias_directive*任意の名前空間またはが表示される名前空間を含む、型の別名を作成し、任意の名前空間または型は、その名前空間内に入れ子にします。

名前空間または型にエイリアスを使ってアクセスするには、その宣言された名前をその名前空間または型へのアクセスとまったく同じ結果が得られます。 たとえば、
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
名前`N1.N2.A`、 `R1.N2.A`、および`R2.A`の完全修飾名がクラスを同等とすべて参照`N1.N2.A`します。

別名を使用して構築されたクローズ型の名前をことができますが、型引数を指定せずに、バインドされていないジェネリック型の宣言の名前をことはできません。 例:
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a>名前空間ディレクティブを使用します。

A *using_namespace_directive*修飾なしで使用するには、各型の識別子を有効にするとすぐにそれを囲むコンパイル単位または名前空間の本体に含まれている名前空間型をインポートします。

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

メンバー宣言が含まれるコンパイル単位または名前空間本文内で、 *using_namespace_directive*、指定した名前空間に含まれる型を直接参照できます。 例:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

上記のメンバー宣言内で、`N3`名前空間、型のメンバーの`N1.N2`直接利用して、クラス`N3.B`クラスから派生`N1.N2.A`します。

A *using_namespace_directive*指定した名前空間に含まれる型をインポートしますが、具体的には入れ子になった名前空間をインポートしません。 例
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
*using_namespace_directive*に含まれている型をインポート`N1`で入れ子になった名前空間ではありませんが、`N1`します。 参照をそのため、`N2.A`の宣言で`B`メンバーが指定されていないため、コンパイル時エラー結果`N2`スコープ内にあります。

異なり、 *using_alias_directive*、 *using_namespace_directive*の識別子が外側のコンパイル単位または名前空間の本文内で既に定義されている型をインポートすることがあります。 によって名前が実際には、インポート、 *using_namespace_directive*外側のコンパイル単位または名前空間の本体で似た名前のメンバーでは表示されません。 例:
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

ここでは、メンバー宣言内で、`N3`名前空間、`A`を指す`N3.A`なく`N1.N2.A`します。

によって、1 つ以上の名前空間または型がインポートするときに*using_namespace_directive*s または*using_static_directive*同じコンパイル単位または名前空間の本体では、同じ名前でへの参照型を含めることがその名前として、 *type_name*あいまいと見なされます。 例
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
両方`N1`と`N2`、メンバーを含んで`A`、ため`N3`を参照する、両方をインポート`A`で`N3`はコンパイル時エラーです。 このような状況で、競合はへの参照の修飾を使用するか解決できる`A`、または導入することで、 *using_alias_directive*特定を取得する`A`します。 例:
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

さらに、インポートすると 1 つ以上の名前空間または型で*using_namespace_directive*s または*using_static_directive*同じコンパイル単位または名前空間の本体では、型またはメンバーを含めることが、同じ名前の参照としてその名前に、 *simple_name*あいまいと見なされます。 例
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
`N1` 型のメンバーが含まれています`A`、および`C`静的フィールドが含まれています`A`、ため`N2`を参照する、両方をインポート`A`として、 *simple_name*あいまいな、コンパイル時にエラーがあります。 

ように、 *using_alias_directive*、 *using_namespace_directive*コンパイル単位または名前空間の基になる宣言領域に新しいメンバーは含まれませんが、代わりにのみ影響しますコンパイル単位または名前空間の本体が表示されます。

*Namespace_name*によって参照される、 *using_namespace_directive*と同じ方法で解決されて、 *namespace_or_type_name*によって参照される、 *using_alias_directive*します。 したがって、 *using_namespace_directive*同じコンパイル単位または名前空間の本体で互いには影響しませんし、任意の順序で書き込むことができます。

### <a name="using-static-directives"></a>Using static ディレクティブ

A *using_static_directive*入れ子にされた型とする各メンバーと型の識別子を有効にする静的メンバーをすぐにそれを囲むコンパイル単位または名前空間の本体に型宣言に直接含まれているインポート修飾なしで使用します。

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

メンバー宣言が含まれるコンパイル単位または名前空間本文内で、 *using_static_directive*型と静的メンバー (拡張メソッド) を除くの宣言に直接含まれている、アクセス可能な入れ子にします指定した型を直接参照できます。 例:

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

上記のメンバー宣言内で、`N2`名前空間、静的メンバーと入れ子にされた型の`N1.A`直接使用できますが、およびそのため、メソッド`N`両方を参照することは、`B`と`M`のメンバー`N1.A`.

A *using_static_directive*具体的には、静的メソッドとして直接拡張メソッドをインポートできませんが、拡張メソッド呼び出しの使用可能になります ([拡張メソッド呼び出し](expressions.md#extension-method-invocations))。 例

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
*using_static_directive*拡張メソッドをインポート`M`に含まれている`N1.A`、拡張メソッドとしてのみです。 したがって、最初に参照される`M`の本体で`B.N`メンバーが指定されていないため、コンパイル時エラー結果`M`スコープ内にあります。

A *using_static_directive*のみをインポート メンバーと型指定された型で直接宣言された基本クラスで宣言されたメンバーと型ではありません。

TODO:例

複数のあいまいさ*using_namespace_directives*と*using_static_directives*は、後ほど[名前空間ディレクティブを使用して](namespaces.md#using-namespace-directives)します。

## <a name="namespace-members"></a>Namespace メンバー

A *namespace_member_declaration*か、 *namespace_declaration* ([Namespace 宣言](namespaces.md#namespace-declarations)) または*type_declaration* ([入力宣言](namespaces.md#type-declarations))。

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

コンパイル単位または名前空間の本文を含めることができます*namespace_member_declaration*と、このような宣言は、新しいメンバーを含むコンパイル単位または名前空間の本体の基になる宣言領域を投稿します。

## <a name="type-declarations"></a>型の宣言

A *type_declaration*は、 *class_declaration* ([クラス クラスの宣言](classes.md#class-declarations))、 *struct_declaration* ([構造体宣言](structs.md#struct-declarations))、 *interface_declaration* ([インターフェイスの宣言](interfaces.md#interface-declarations))、 *enum_declaration* ([列挙型宣言](enums.md#enum-declarations))、または*delegate_declaration* ([デリゲートの宣言](delegates.md#delegate-declarations))。

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

A *type_declaration*コンパイル単位内の最上位レベルの宣言、または名前空間、クラスまたは構造体内でメンバーの宣言として発生することができます。

型の型宣言`T`新しく宣言された型の完全修飾名が単には、コンパイル単位内の最上位レベルの宣言と発生`T`します。 型の型宣言`T`クラスまたは構造体、新しく宣言された型の完全修飾名が、名前空間内で発生`N.T`ここで、`N`含んでいる名前空間、クラスまたは構造体の完全修飾の名前を指定します。

クラス内で宣言された型または構造体には、入れ子にされた型が呼び出された ([入れ子になった型](classes.md#nested-types))。

使用できるアクセス修飾子と型の宣言の既定のアクセスを宣言が行われるコンテキストに依存 ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))。

*  コンパイル単位または名前空間で宣言された型を持つことができます`public`または`internal`アクセスします。 既定値は`internal`アクセスします。
*  クラスで宣言された型を持つことができます`public`、 `protected internal`、 `protected`、 `internal`、または`private`アクセスします。 既定値は`private`アクセスします。
*  構造体で宣言された型を持つことができます`public`、 `internal`、または`private`アクセスします。 既定値は`private`アクセスします。

## <a name="namespace-alias-qualifiers"></a>Namespace エイリアス修飾子

***名前空間エイリアス修飾子***`::`型名の検索が新しい型とメンバーの導入によって影響を受けるいないことを保証することになります。 名前空間エイリアス修飾子は、左辺と右辺の識別子と呼ばれる 2 つの識別子の間で常に表示されます。 通常とは異なり`.`修飾子の左側の識別子、`::`修飾子は、参照が、extern、またはエイリアスを使用してとしてのみです。

A *qualified_alias_member*が次のように定義されています。

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

A *qualified_alias_member*として使用できる、 *namespace_or_type_name* ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names)) またはの左辺のオペランドとして、 *member_access*([メンバー アクセス](expressions.md#member-access))。

A *qualified_alias_member*が 2 つの形式のいずれか。

*  `N::I<A1, ..., Ak>`、、`N`と`I`、識別子を表すと`<A1, ..., Ak>`が型引数リスト。 (`K`は常に少なくとも 1 つです)。
*  `N::I`、、`N`と`I`識別子を表します。 (この場合、 `K` 0 と見なされます)。

意味、この表記を使用して、 *qualified_alias_member*は次のように決定されます。

*  場合`N`識別子`global`、グローバル名前空間を検索し、 `I`:
   * グローバル名前空間には、名前空間が含まれている場合 `I`と`K`0 の場合は、次に、 *qualified_alias_member*その名前空間を参照します。
   * それ以外の場合、グローバル名前空間には、という名前の非ジェネリック型が含まれている場合 `I`と`K`0 の場合は、次に、 *qualified_alias_member*はその型を表します。
   * それ以外の場合、グローバル名前空間には、という名前の型が含まれている場合 `I`を持つ`K` パラメーター、入力、 *qualified_alias_member*は特定の型引数を使用して構築する型を表します。
   * それ以外の場合、 *qualified_alias_member*は未定義となり、コンパイル時エラーが発生します。

*  それ以外の場合、名前空間の宣言で始まる ([Namespace 宣言](namespaces.md#namespace-declarations)) 含まれている、 *qualified_alias_member* (ある場合)、それを囲む各名前空間宣言で継続的(ある場合) を含むコンパイル単位で終わる、 *qualified_alias_member*エンティティが見つかるまで、次の手順が評価されます。

   * 名前空間の宣言またはコンパイル ユニットに含まれる場合、 *using_alias_directive*を関連付ける`N`型を持つ、 *qualified_alias_member*が定義されていないと、コンパイル時エラーが発生します。
   * それ以外の場合、名前空間の宣言またはコンパイル ユニットに含まれる場合、 *extern_alias_directive*または*using_alias_directive*を関連付ける`N`名前空間、しに。
     * 名前空間に関連付けられている場合`N`という名前空間が含まれています `I`と`K`0 の場合は、次に、 *qualified_alias_member*その名前空間を参照します。
     * それ以外の場合、名前空間に関連付けられている場合`N`という名前の非ジェネリック型が含まれています `I`と`K`0 の場合は、次に、 *qualified_alias_member*はその型を表します。
     * それ以外の場合、名前空間に関連付けられている場合`N`という名前の型が含まれています `I`を持つ`K` パラメーターを入力し、 *qualified_alias_member*を持つ型を構築するには指定された型引数。
     * それ以外の場合、 *qualified_alias_member*は未定義となり、コンパイル時エラーが発生します。
*  それ以外の場合、 *qualified_alias_member*は未定義となり、コンパイル時エラーが発生します。

型を参照するエイリアスで名前空間エイリアス修飾子を使用して、コンパイル時エラーが発生に注意してください。 いる場合に、id をメモ `N`は`global`、使用してエイリアスに関連付けることがある場合でも、グローバル名前空間で検索を実行し、`global`型または名前空間。

### <a name="uniqueness-of-aliases"></a>エイリアスの一意性

各コンパイル単位と名前空間の本体がの extern エイリアスを別の宣言領域とエイリアスを使用します。 そのため、エイリアスを使用してまたは extern エイリアスの名前は、extern エイリアスのセット内で一意である必要があります、すぐに親のコンパイル単位または名前空間の本体で宣言されたエイリアスを使用して、エイリアスが型または名前空間の長さと同じ名前を持つことができますはt はでのみ使用、`::`修飾子。

例
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
名前`A`ため、2 つ目の名前空間の 2 つの可能性のある意味を持つクラスにも`A`を使用して、およびエイリアス`A`スコープ内にあります。 このため、使用`A`修飾名で`A.Stream`があいまいであり、コンパイル時エラーが発生します。 ただしの使用`A`で、`::`修飾子エラーではないため、`A`名前空間の別名としてのみを検索します。
