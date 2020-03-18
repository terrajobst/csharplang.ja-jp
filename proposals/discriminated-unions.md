---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2019
ms.locfileid: "79484034"
---

# <a name="discriminated-unions--enum-class"></a>判別共用体/`enum class`

`enum class`es は、型宣言の新しい種類であり、判別共用体と呼ばれることもあります。ここでは、各インスタンスで型が一覧表示され、各インスタンスが重複していないことがわかります。

`enum class` は、次の構文を使用して定義されます。

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

構文例:

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a>Semantics

`enum class` 定義は、ルート型を定義します。これは、`enum class` 宣言と同じ名前の抽象クラスであり、メンバーのセットであり、それぞれがルート型のサブタイプである型を持ちます。 複数の `partial enum class` 定義がある場合、すべてのメンバーは列挙型クラス定義のメンバーと見なされます。 ユーザー定義の抽象クラス定義とは異なり、`enum class` のルート型は既定で部分的で、既定の*プライベート*パラメーターのないコンストラクターを持つように定義されています。

ルート型は部分抽象クラスとして定義されるため、*ルート型*の部分定義も追加される可能性があることに注意してください。クラス本体の標準的な構文形式を使用できます。
ただし、型を `enum class` メンバーとして指定されたものとは別に、任意の宣言のルート型から直接継承することはできません。 また、ルート型に対してユーザー定義のコンストラクターを使用することはできません。

`enum class` メンバー宣言には、次の4種類があります。

* 単純なクラスメンバー

* 複合クラスメンバー

* `enum class` メンバー

* 値のメンバー。

### <a name="simple-class-members"></a>単純なクラスメンバー

単純クラスメンバーの宣言では、同じ名前を使用して、入れ子になった新しい "record" クラス (このドキュメントで意図的に残されています) を定義します。 入れ子になったクラスは、ルート型から継承されます。

上記のサンプルコードを指定すると、

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

`enum class` 宣言には、次の宣言と等価なセマンティクスがあります。

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a>複合クラスメンバー

また、クラス宣言全体を `enum class` 宣言の下に入れ子にすることもできます。 これは、ルート型の入れ子になったクラスとして扱われます。 構文では任意のクラス宣言を使用できますが、複合クラスメンバーは `enum class` 宣言を含む直接のを継承する必要があります。 

### <a name="enum-class-members"></a>`enum class` メンバー

`enum classes` は相互に入れ子にすることができます。例:

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

これは、最上位レベルの `enum class`のセマンティクスとほぼ同じですが、入れ子になった列挙型クラスが入れ子になったルート型を定義し、入れ子になった列挙型クラスの下のすべてのものが、最上位レベルのルート型ではなく、入れ子になったルート型のサブタイプである点が異なります。

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a>値のメンバー

`enum classes` には、値メンバーを含めることもできます。 値メンバーはルート型のパブリックな get 専用静的プロパティを定義します。これはルート型も返します。

```C#
enum class Color
{
    Red,
    Green
}
```

プロパティはと等価です。

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

完全なセマンティクスは実装の詳細と見なされますが、各プロパティに対して1つの一意のインスタンスが返され、同じインスタンスが繰り返し呼び出し時に返されることが保証されます。


### <a name="switch-expression-and-patterns"></a>式とパターンの切り替え

パターンマッチングに対して提案された調整と、`enum classes`を処理する switch 式があります。 Switch 式では、変数パターンを使用して型を既に一致させることができますが、現在のところ、参照型では、switch 式に含まれるスイッチアームのセットは完全と見なされません。ただし、引数の静的な型またはサブタイプと照合する場合は除きます。

Switch 式は変更されます。これにより、`enum class` のルート型が switch 式の引数の静的な型であり、列挙型のすべてのメンバーに一致するパターンのセットがある場合、スイッチは完全に見なされます。

値メンバーは定数ではなく、新しい静的な型を定義しないため、現在、パターンに一致させることはできません。 これを可能にするために、定数パターンの構文を使用する新しいパターンを追加して、`enum class` 値メンバーとの照合を許可します。 一致が成功するように定義されているのは、パターンに一致する引数と `enum class` 値メンバーから返された値が等価である場合のみです。ただし、実装ではこのチェックを実行する必要はありません。


## <a name="open-questions"></a>質問を開く

- [] `enum class` メンバーに関して共通型アルゴリズムは何を意味しますか。 この有効なコードですか?
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- [] 値メンバーのためだけに新しいパターンを追加すると、より多くの値が渡されているように見えます。 意味のある、より一般的なバージョンの構築はありますか。
    - [] 値メンバーも、次の理由により、入れ子になった並列クラスの構築に適切にマップされません。

- [] `enum class` 静的な型が定数時間であることが保証されている引数に対して切り替えます。

- [] Switch 式で `enum class`を完了と見なさないようにする方法はありますか。 `virtual`のプレフィックス

- [] `enum class`で許可する必要がある修飾子を指定してください。