---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2019
ms.locfileid: "79484034"
---

# <a name="discriminated-unions--enum-class"></a><span data-ttu-id="3d6fd-101">判別共用体/`enum class`</span><span class="sxs-lookup"><span data-stu-id="3d6fd-101">Discriminated unions / `enum class`</span></span>

<span data-ttu-id="3d6fd-102">`enum class`es は、型宣言の新しい種類であり、判別共用体と呼ばれることもあります。ここでは、各インスタンスで型が一覧表示され、各インスタンスが重複していないことがわかります。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-102">`enum class`es are a new kind of type declaration, sometimes referred to as discriminated unions, where each every possible instance the type is listed, and each instance is non-overlapping.</span></span>

<span data-ttu-id="3d6fd-103">`enum class` は、次の構文を使用して定義されます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-103">An `enum class` is defined using the following syntax:</span></span>

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

<span data-ttu-id="3d6fd-104">構文例:</span><span class="sxs-lookup"><span data-stu-id="3d6fd-104">Sample syntax:</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a><span data-ttu-id="3d6fd-105">Semantics</span><span class="sxs-lookup"><span data-stu-id="3d6fd-105">Semantics</span></span>

<span data-ttu-id="3d6fd-106">`enum class` 定義は、ルート型を定義します。これは、`enum class` 宣言と同じ名前の抽象クラスであり、メンバーのセットであり、それぞれがルート型のサブタイプである型を持ちます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-106">An `enum class` definition defines a root type, which is an abstract class of the same name as the `enum class` declaration, and a set of members, each of which has a type which is a subtype of the root type.</span></span> <span data-ttu-id="3d6fd-107">複数の `partial enum class` 定義がある場合、すべてのメンバーは列挙型クラス定義のメンバーと見なされます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-107">If there are multiple `partial enum class` definitions, all members will be considered members of the enum class definition.</span></span> <span data-ttu-id="3d6fd-108">ユーザー定義の抽象クラス定義とは異なり、`enum class` のルート型は既定で部分的で、既定の*プライベート*パラメーターのないコンストラクターを持つように定義されています。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-108">Unlike a user-defined abstract class definition, the `enum class` root type is partial by default and defined to have a default *private* parameter-less constructor.</span></span>

<span data-ttu-id="3d6fd-109">ルート型は部分抽象クラスとして定義されるため、*ルート型*の部分定義も追加される可能性があることに注意してください。クラス本体の標準的な構文形式を使用できます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-109">Note that, since the root type is defined to be a partial abstract class, partial definitions of the *root type* may also be added, where standard syntax forms for a class body are allowed.</span></span>
<span data-ttu-id="3d6fd-110">ただし、型を `enum class` メンバーとして指定されたものとは別に、任意の宣言のルート型から直接継承することはできません。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-110">However, no types may directly inherit from the root type in any declaration, aside from those specified as `enum class` members.</span></span> <span data-ttu-id="3d6fd-111">また、ルート型に対してユーザー定義のコンストラクターを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-111">In addition, no user-defined constructors are permitted for the root type.</span></span>

<span data-ttu-id="3d6fd-112">`enum class` メンバー宣言には、次の4種類があります。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-112">There are four kinds of `enum class` member declarations:</span></span>

* <span data-ttu-id="3d6fd-113">単純なクラスメンバー</span><span class="sxs-lookup"><span data-stu-id="3d6fd-113">simple class members</span></span>

* <span data-ttu-id="3d6fd-114">複合クラスメンバー</span><span class="sxs-lookup"><span data-stu-id="3d6fd-114">complex class members</span></span>

* <span data-ttu-id="3d6fd-115">`enum class` メンバー</span><span class="sxs-lookup"><span data-stu-id="3d6fd-115">`enum class` members</span></span>

* <span data-ttu-id="3d6fd-116">値のメンバー。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-116">value members.</span></span>

### <a name="simple-class-members"></a><span data-ttu-id="3d6fd-117">単純なクラスメンバー</span><span class="sxs-lookup"><span data-stu-id="3d6fd-117">Simple class members</span></span>

<span data-ttu-id="3d6fd-118">単純クラスメンバーの宣言では、同じ名前を使用して、入れ子になった新しい "record" クラス (このドキュメントで意図的に残されています) を定義します。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-118">A simple class member declaration defines a new nested "record" class (intentionally left undefined in this document) with the same name.</span></span> <span data-ttu-id="3d6fd-119">入れ子になったクラスは、ルート型から継承されます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-119">The nested class inherits from the root type.</span></span>

<span data-ttu-id="3d6fd-120">上記のサンプルコードを指定すると、</span><span class="sxs-lookup"><span data-stu-id="3d6fd-120">Given the sample code above,</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

<span data-ttu-id="3d6fd-121">`enum class` 宣言には、次の宣言と等価なセマンティクスがあります。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-121">the `enum class` declaration has semantics equivalent to the following declaration</span></span>

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a><span data-ttu-id="3d6fd-122">複合クラスメンバー</span><span class="sxs-lookup"><span data-stu-id="3d6fd-122">Complex class members</span></span>

<span data-ttu-id="3d6fd-123">また、クラス宣言全体を `enum class` 宣言の下に入れ子にすることもできます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-123">You can also nest an entire class declaration below an `enum class` declaration.</span></span> <span data-ttu-id="3d6fd-124">これは、ルート型の入れ子になったクラスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-124">It will be treated as a nested class of the root type.</span></span> <span data-ttu-id="3d6fd-125">構文では任意のクラス宣言を使用できますが、複合クラスメンバーは `enum class` 宣言を含む直接のを継承する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-125">The syntax allows any class declaration, but it is required for the complex class member to inherit from the direct containing `enum class` declaration.</span></span> 

### <a name="enum-class-members"></a><span data-ttu-id="3d6fd-126">`enum class` メンバー</span><span class="sxs-lookup"><span data-stu-id="3d6fd-126">`enum class` members</span></span>

<span data-ttu-id="3d6fd-127">`enum classes` は相互に入れ子にすることができます。例:</span><span class="sxs-lookup"><span data-stu-id="3d6fd-127">`enum classes` can be nested under each other, e.g.</span></span>

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

<span data-ttu-id="3d6fd-128">これは、最上位レベルの `enum class`のセマンティクスとほぼ同じですが、入れ子になった列挙型クラスが入れ子になったルート型を定義し、入れ子になった列挙型クラスの下のすべてのものが、最上位レベルのルート型ではなく、入れ子になったルート型のサブタイプである点が異なります。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-128">This is almost identical to the semantics of a top-level `enum class`, except that the nested enum class defines a nested root type, and everything below the nested enum class is a subtype of the nested root type, instead of the top-level root type.</span></span>

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

### <a name="value-members"></a><span data-ttu-id="3d6fd-129">値のメンバー</span><span class="sxs-lookup"><span data-stu-id="3d6fd-129">Value members</span></span>

<span data-ttu-id="3d6fd-130">`enum classes` には、値メンバーを含めることもできます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-130">`enum classes` can also contain value members.</span></span> <span data-ttu-id="3d6fd-131">値メンバーはルート型のパブリックな get 専用静的プロパティを定義します。これはルート型も返します。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-131">Value members define public get-only static properties on the root type that also return the root type, e.g.</span></span>

```C#
enum class Color
{
    Red,
    Green
}
```

<span data-ttu-id="3d6fd-132">プロパティはと等価です。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-132">has properties equivalent to</span></span>

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

<span data-ttu-id="3d6fd-133">完全なセマンティクスは実装の詳細と見なされますが、各プロパティに対して1つの一意のインスタンスが返され、同じインスタンスが繰り返し呼び出し時に返されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-133">The complete semantics are considered an implementation detail, but it is guaranteed that one unique instance will be returned for each property, and the same instance will be returned on repeated invocations.</span></span>


### <a name="switch-expression-and-patterns"></a><span data-ttu-id="3d6fd-134">式とパターンの切り替え</span><span class="sxs-lookup"><span data-stu-id="3d6fd-134">Switch expression and patterns</span></span>

<span data-ttu-id="3d6fd-135">パターンマッチングに対して提案された調整と、`enum classes`を処理する switch 式があります。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-135">There are some proposed adjustments to pattern matching and the switch expression to handle `enum classes`.</span></span> <span data-ttu-id="3d6fd-136">Switch 式では、変数パターンを使用して型を既に一致させることができますが、現在のところ、参照型では、switch 式に含まれるスイッチアームのセットは完全と見なされません。ただし、引数の静的な型またはサブタイプと照合する場合は除きます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-136">Switch expressions can already match types through the variable pattern, but for currently for reference types, no set of switch arms in the switch expression are considered complete, except for matching against the static type of the argument, or a subtype.</span></span>

<span data-ttu-id="3d6fd-137">Switch 式は変更されます。これにより、`enum class` のルート型が switch 式の引数の静的な型であり、列挙型のすべてのメンバーに一致するパターンのセットがある場合、スイッチは完全に見なされます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-137">Switch expressions would be changed such that, if the root type of an `enum class` is the static type of the argument to the switch expression, and there is a set of patterns matching all members of the enum, then the switch will be considered exhaustive.</span></span>

<span data-ttu-id="3d6fd-138">値メンバーは定数ではなく、新しい静的な型を定義しないため、現在、パターンに一致させることはできません。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-138">Since value members are not constants and do not define new static types, they currently cannot be matched by pattern.</span></span> <span data-ttu-id="3d6fd-139">これを可能にするために、定数パターンの構文を使用する新しいパターンを追加して、`enum class` 値メンバーとの照合を許可します。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-139">To make this possible, a new pattern using the constant pattern syntax will be added to allow match against `enum class` value members.</span></span> <span data-ttu-id="3d6fd-140">一致が成功するように定義されているのは、パターンに一致する引数と `enum class` 値メンバーから返された値が等価である場合のみです。ただし、実装ではこのチェックを実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-140">The match is defined to succeed if and only if the argument to the pattern match and the value returned by the `enum class` value member would be reference equal, although the implementation is not required to perform this check.</span></span>


## <a name="open-questions"></a><span data-ttu-id="3d6fd-141">質問を開く</span><span class="sxs-lookup"><span data-stu-id="3d6fd-141">Open questions</span></span>

- <span data-ttu-id="3d6fd-142">[] `enum class` メンバーに関して共通型アルゴリズムは何を意味しますか。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-142">[ ] What does the common type algorithm say about `enum class` members?</span></span> <span data-ttu-id="3d6fd-143">この有効なコードですか?</span><span class="sxs-lookup"><span data-stu-id="3d6fd-143">Is this valid code?</span></span>
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- <span data-ttu-id="3d6fd-144">[] 値メンバーのためだけに新しいパターンを追加すると、より多くの値が渡されているように見えます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-144">[ ] Adding a new pattern just for value members seems heavy handed.</span></span> <span data-ttu-id="3d6fd-145">意味のある、より一般的なバージョンの構築はありますか。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-145">Is there a more general version construction that makes sense?</span></span>
    - <span data-ttu-id="3d6fd-146">[] 値メンバーも、次の理由により、入れ子になった並列クラスの構築に適切にマップされません。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-146">[ ] Value members also do not map well to a parallel nested class construction because of this</span></span>

- <span data-ttu-id="3d6fd-147">[] `enum class` 静的な型が定数時間であることが保証されている引数に対して切り替えます。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-147">[ ] Is switching against an argument with an `enum class` static type guaranteed to be constant-time?</span></span>

- <span data-ttu-id="3d6fd-148">[] Switch 式で `enum class`を完了と見なさないようにする方法はありますか。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-148">[ ] Should there be a way to make `enum class`es not be considered complete in the switch expression?</span></span> <span data-ttu-id="3d6fd-149">`virtual`のプレフィックス</span><span class="sxs-lookup"><span data-stu-id="3d6fd-149">Prefix with `virtual`?</span></span>

- <span data-ttu-id="3d6fd-150">[] `enum class`で許可する必要がある修飾子を指定してください。</span><span class="sxs-lookup"><span data-stu-id="3d6fd-150">[ ] What modifiers should be permitted on `enum class`?</span></span>