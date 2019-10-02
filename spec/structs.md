---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704026"
---
# <a name="structs"></a><span data-ttu-id="51963-101">構造体</span><span class="sxs-lookup"><span data-stu-id="51963-101">Structs</span></span>

<span data-ttu-id="51963-102">構造体は、データメンバーと関数メンバーを含むことができるデータ構造を表すという点でクラスに似ています。</span><span class="sxs-lookup"><span data-stu-id="51963-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="51963-103">ただし、クラスとは異なり、構造体は値型であり、ヒープ割り当ては必要ありません。</span><span class="sxs-lookup"><span data-stu-id="51963-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="51963-104">構造体型の変数は、構造体のデータを直接格納します。一方、クラス型の変数には、データへの参照 (後者はオブジェクトと呼ばれます) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="51963-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="51963-105">構造体は、値セマンティクスを持つ小規模なデータ構造に特に便利です。</span><span class="sxs-lookup"><span data-stu-id="51963-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="51963-106">構造体の主な例としては、複素数、座標系のポイント、ディクショナリのキーと値のペアなどがあります。</span><span class="sxs-lookup"><span data-stu-id="51963-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="51963-107">これらのデータ構造にとって重要なのは、データメンバーが少ないこと、継承または参照 id を使用する必要がないこと、および割り当てによって参照の代わりに値がコピーされる値のセマンティクスを使用して便宜的に実装できることです。</span><span class="sxs-lookup"><span data-stu-id="51963-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="51963-108">「[単純型](types.md#simple-types)」で説明されているようC#に、によって提供される単純型 (`int`、`double`、`bool` など) は、実際にはすべての構造体型です。</span><span class="sxs-lookup"><span data-stu-id="51963-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="51963-109">これらの定義済みの型が構造体であるのと同様に、構造体と演算子のオーバーロードを使用して、 C#新しい "プリミティブ" 型を言語で実装することもできます。</span><span class="sxs-lookup"><span data-stu-id="51963-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="51963-110">このような型の2つの例については、この章の最後 ([構造体の例](structs.md#struct-examples)) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="51963-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="51963-111">構造体の宣言</span><span class="sxs-lookup"><span data-stu-id="51963-111">Struct declarations</span></span>

<span data-ttu-id="51963-112">*Struct_declaration*は、新しい構造体を宣言する*type_declaration* ([型宣言](namespaces.md#type-declarations)) です。</span><span class="sxs-lookup"><span data-stu-id="51963-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="51963-113">*Struct_declaration*は、省略可能な*属性*のセット ([属性](attributes.md)) の後にオプションの一連の*struct_modifier*s ([struct 修飾子](structs.md#struct-modifiers)) が続き、その後に省略可能な `partial` 修飾子が続きます。キーワード `struct` と、構造体に名前を付け、その後に省略可能な*type_parameter_list*仕様 ([型パラメーター](classes.md#type-parameters)) を指定する*識別子*、およびその後に省略可能な*struct_interfaces*仕様 ([Partial修飾子](structs.md#partial-modifier)) の後に省略可能な*type_parameter_constraints_clause*s 仕様 ([型パラメーターの制約](classes.md#type-parameter-constraints)) を指定し、続けて*struct_body* ([構造体の本体](structs.md#struct-body)) を入力します。その後にセミコロンを続けます。</span><span class="sxs-lookup"><span data-stu-id="51963-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="51963-114">Struct 修飾子</span><span class="sxs-lookup"><span data-stu-id="51963-114">Struct modifiers</span></span>

<span data-ttu-id="51963-115">*Struct_declaration*には、必要に応じて構造体の修飾子のシーケンスを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="51963-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

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

<span data-ttu-id="51963-116">同じ修飾子が struct 宣言で複数回出現する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51963-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="51963-117">構造体宣言の修飾子は、クラス宣言 ([クラス宣言](classes.md#class-declarations)) と同じ意味を持ちます。</span><span class="sxs-lookup"><span data-stu-id="51963-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="51963-118">Partial 修飾子</span><span class="sxs-lookup"><span data-stu-id="51963-118">Partial modifier</span></span>

<span data-ttu-id="51963-119">@No__t-0 修飾子は、この*struct_declaration*が部分型の宣言であることを示します。</span><span class="sxs-lookup"><span data-stu-id="51963-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="51963-120">外側の名前空間または型宣言内で同じ名前を持つ複数の部分構造体宣言を組み合わせると、[部分型](classes.md#partial-types)で指定された規則に従って1つの構造体宣言が形成されます。</span><span class="sxs-lookup"><span data-stu-id="51963-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="51963-121">構造体のインターフェイス</span><span class="sxs-lookup"><span data-stu-id="51963-121">Struct interfaces</span></span>

<span data-ttu-id="51963-122">Struct 宣言には*struct_interfaces*仕様を含めることができます。この場合、構造体は、指定されたインターフェイス型を直接実装すると言います。</span><span class="sxs-lookup"><span data-stu-id="51963-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="51963-123">インターフェイスの実装については、「[インターフェイスの実装](interfaces.md#interface-implementations)」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="51963-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="51963-124">構造体の本体</span><span class="sxs-lookup"><span data-stu-id="51963-124">Struct body</span></span>

<span data-ttu-id="51963-125">構造体の*struct_body*は、構造体のメンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="51963-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="51963-126">構造体のメンバー</span><span class="sxs-lookup"><span data-stu-id="51963-126">Struct members</span></span>

<span data-ttu-id="51963-127">構造体のメンバーは、 *struct_member_declaration*によって導入されたメンバーと、その @no__t 型から継承されたメンバーで構成されます-1。</span><span class="sxs-lookup"><span data-stu-id="51963-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

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

<span data-ttu-id="51963-128">[クラスと構造体の違い](structs.md#class-and-struct-differences)については、[反復子](classes.md#iterators)を通じて[クラスメンバー](classes.md#class-members)に指定されたクラスメンバーの説明も、構造体のメンバーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="51963-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="51963-129">クラスと構造体の違い</span><span class="sxs-lookup"><span data-stu-id="51963-129">Class and struct differences</span></span>

<span data-ttu-id="51963-130">構造体は、いくつかの重要な点でクラスとは異なります。</span><span class="sxs-lookup"><span data-stu-id="51963-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="51963-131">構造体は値型 ([値のセマンティクス](structs.md#value-semantics)) です。</span><span class="sxs-lookup"><span data-stu-id="51963-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="51963-132">すべての構造体型は、クラス `System.ValueType` ([継承](structs.md#inheritance)) を暗黙的に継承します。</span><span class="sxs-lookup"><span data-stu-id="51963-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="51963-133">構造体型の変数への代入では、代入される値のコピーが作成されます ([代入](structs.md#assignment))。</span><span class="sxs-lookup"><span data-stu-id="51963-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="51963-134">構造体の既定値は、すべての値の型フィールドを既定値に設定し、すべての参照型フィールドを `null` ([既定値](structs.md#default-values)) に設定することによって生成される値です。</span><span class="sxs-lookup"><span data-stu-id="51963-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="51963-135">ボックス化およびボックス化解除の操作は、構造体型と `object` ([ボックス化およびボックス化解除](structs.md#boxing-and-unboxing)) の間の変換に使用されます。</span><span class="sxs-lookup"><span data-stu-id="51963-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="51963-136">@No__t-0 の意味は、構造体 ([このアクセス](expressions.md#this-access)) では異なります。</span><span class="sxs-lookup"><span data-stu-id="51963-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="51963-137">構造体のインスタンスフィールド宣言には、変数初期化子 ([フィールド初期化子](structs.md#field-initializers)) を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="51963-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="51963-138">構造体は、パラメーターなしのインスタンスコンストラクター ([コンストラクター](structs.md#constructors)) の宣言を許可されていません。</span><span class="sxs-lookup"><span data-stu-id="51963-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="51963-139">構造体はデストラクター ([デストラクター](structs.md#destructors)) の宣言を許可されていません。</span><span class="sxs-lookup"><span data-stu-id="51963-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="51963-140">値のセマンティクス</span><span class="sxs-lookup"><span data-stu-id="51963-140">Value semantics</span></span>

<span data-ttu-id="51963-141">構造体は値型 (値[型](types.md#value-types)) であり、値のセマンティクスがあると言います。</span><span class="sxs-lookup"><span data-stu-id="51963-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="51963-142">一方、クラスは参照型 ([参照型](types.md#reference-types)) であり、参照セマンティクスがあると言います。</span><span class="sxs-lookup"><span data-stu-id="51963-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="51963-143">構造体型の変数は、構造体のデータを直接格納します。一方、クラス型の変数には、データへの参照 (後者はオブジェクトと呼ばれます) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="51963-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="51963-144">構造体 `B` に @no__t 型のインスタンスフィールドが含まれており、`A` が構造体型である場合、`A` は `B` または @no__t から構築された型に依存します。</span><span class="sxs-lookup"><span data-stu-id="51963-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="51963-145">構造体 `X` は、`X` が @no__t 型のインスタンスフィールドを格納している場合は、構造体 `Y`***に直接依存***します。</span><span class="sxs-lookup"><span data-stu-id="51963-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="51963-146">この定義により、構造体が依存する構造体の完全なセットは、リレーションシップ***に直接依存***します。</span><span class="sxs-lookup"><span data-stu-id="51963-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="51963-147">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="51963-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="51963-148">`Node` に独自の型のインスタンスフィールドが含まれているため、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51963-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="51963-149">別の例</span><span class="sxs-lookup"><span data-stu-id="51963-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="51963-150">は、`A`、`B`、@no__t の各型が互いに依存しているため、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="51963-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="51963-151">クラスを使用すると、2つの変数が同じオブジェクトを参照する可能性があるため、1つの変数に対する操作が、もう一方の変数によって参照されるオブジェクトに影響を与える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="51963-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="51963-152">構造体を使用すると、変数にはそれぞれ独自のデータコピーが含まれます (`ref` および `out` パラメーター変数の場合を除きます)。また、1つの変数に対して操作を行うことはできません。</span><span class="sxs-lookup"><span data-stu-id="51963-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="51963-153">さらに、構造体は参照型ではないため、構造体型の値を @no__t 0 にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="51963-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="51963-154">宣言を指定します。</span><span class="sxs-lookup"><span data-stu-id="51963-154">Given the declaration</span></span>
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
<span data-ttu-id="51963-155">コード片</span><span class="sxs-lookup"><span data-stu-id="51963-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="51963-156">値 `10` に出力します。</span><span class="sxs-lookup"><span data-stu-id="51963-156">outputs the value `10`.</span></span> <span data-ttu-id="51963-157">@No__t-0 から `b` への割り当てによって値のコピーが作成されるため、`b` は `a.x` への割り当ての影響を受けません。</span><span class="sxs-lookup"><span data-stu-id="51963-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="51963-158">@No__t-0 はクラスとして宣言されていましたが、`a` と `b` が同じオブジェクトを参照するため、出力は-1 @no__t ます。</span><span class="sxs-lookup"><span data-stu-id="51963-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="51963-159">継承</span><span class="sxs-lookup"><span data-stu-id="51963-159">Inheritance</span></span>

<span data-ttu-id="51963-160">すべての構造体型は、クラス `System.ValueType` から暗黙的に継承します。このクラスは、クラス `object` から継承されます。</span><span class="sxs-lookup"><span data-stu-id="51963-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="51963-161">構造体の宣言では実装されたインターフェイスのリストを指定できますが、構造体の宣言で基底クラスを指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="51963-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="51963-162">構造体型は抽象ではなく、常に暗黙的にシールされます。</span><span class="sxs-lookup"><span data-stu-id="51963-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="51963-163">したがって、`abstract` および `sealed` 修飾子は、構造体宣言では許可されません。</span><span class="sxs-lookup"><span data-stu-id="51963-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="51963-164">継承は構造体ではサポートされていないため、構造体メンバーの宣言されたアクセシビリティを @no__t 0 または `protected internal` にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="51963-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="51963-165">構造体の関数メンバーを @no__t 0 または `virtual` にすることはできません。また、`override` 修飾子は、`System.ValueType` から継承されたメソッドをオーバーライドする場合にのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="51963-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="51963-166">代入</span><span class="sxs-lookup"><span data-stu-id="51963-166">Assignment</span></span>

<span data-ttu-id="51963-167">構造体型の変数への代入では、割り当てられている値のコピーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="51963-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="51963-168">これは、参照によって識別されるオブジェクトではなく、参照をコピーするクラス型の変数への代入とは異なります。</span><span class="sxs-lookup"><span data-stu-id="51963-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="51963-169">代入と同様に、構造体が値パラメーターとして渡される場合や、関数メンバーの結果として返される場合は、構造体のコピーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="51963-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="51963-170">構造体は、`ref` または `out` パラメーターを使用して関数メンバーに参照渡しできます。</span><span class="sxs-lookup"><span data-stu-id="51963-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="51963-171">構造体のプロパティまたはインデクサーが割り当てのターゲットである場合、プロパティまたはインデクサーアクセスに関連付けられたインスタンス式は、変数として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="51963-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="51963-172">インスタンス式が値として分類されている場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="51963-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="51963-173">詳細については、「[単純な割り当て](expressions.md#simple-assignment)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="51963-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="51963-174">既定の値</span><span class="sxs-lookup"><span data-stu-id="51963-174">Default values</span></span>

<span data-ttu-id="51963-175">「[既定値](variables.md#default-values)」で説明されているように、いくつかの種類の変数は、作成時に自動的に既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="51963-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="51963-176">クラス型およびその他の参照型の変数の場合、この既定値は `null` です。</span><span class="sxs-lookup"><span data-stu-id="51963-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="51963-177">ただし、構造体は値型であり、0 @no__t できないため、構造体の既定値は、すべての値の型フィールドを既定値に設定し、すべての参照型フィールドを `null` に設定することによって生成される値です。</span><span class="sxs-lookup"><span data-stu-id="51963-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="51963-178">上で宣言された @no__t 0 構造体を参照しています。例</span><span class="sxs-lookup"><span data-stu-id="51963-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="51963-179">配列内の各 `Point` を、`x` および `y` フィールドを0に設定することによって生成される値に初期化します。</span><span class="sxs-lookup"><span data-stu-id="51963-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="51963-180">構造体の既定値は、構造体の既定のコンストラクターによって返される値に対応します ([既定のコンストラクター](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="51963-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="51963-181">クラスとは異なり、構造体では、パラメーターなしのインスタンスコンストラクターを宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="51963-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="51963-182">代わりに、すべての構造体には、暗黙的にパラメーターなしのインスタンスコンストラクターがあります。このコンストラクターは、すべての値型フィールドを既定値に設定し、すべての参照型フィールドを `null` に設定した結果の値を常に返します。</span><span class="sxs-lookup"><span data-stu-id="51963-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="51963-183">構造体は、既定の初期化状態が有効な状態であると見なすように設計する必要があります。</span><span class="sxs-lookup"><span data-stu-id="51963-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="51963-184">この例では、</span><span class="sxs-lookup"><span data-stu-id="51963-184">In the example</span></span>
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
<span data-ttu-id="51963-185">ユーザー定義のインスタンスコンストラクターは、明示的に呼び出された場合にのみ null 値を保護します。</span><span class="sxs-lookup"><span data-stu-id="51963-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="51963-186">@No__t 0 の変数が既定値の初期化の対象となる場合、`key` フィールドと `value` フィールドは null になり、構造体はこの状態を処理できるように準備する必要があります。</span><span class="sxs-lookup"><span data-stu-id="51963-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="51963-187">ボックス化とボックス化解除</span><span class="sxs-lookup"><span data-stu-id="51963-187">Boxing and unboxing</span></span>

<span data-ttu-id="51963-188">クラス型の値は、コンパイル時に別の型として参照を扱うことによって、型 `object` またはクラスによって実装されるインターフェイス型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="51963-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="51963-189">同様に、型 `object` またはインターフェイス型の値は、参照を変更せずにクラス型に変換できます (この場合は、実行時の型チェックが必要です)。</span><span class="sxs-lookup"><span data-stu-id="51963-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="51963-190">構造体は参照型ではないため、これらの操作は構造体型に対して異なる方法で実装されます。</span><span class="sxs-lookup"><span data-stu-id="51963-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="51963-191">構造体型の値が型 `object` または構造体によって実装されるインターフェイス型に変換されると、ボックス化操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="51963-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="51963-192">同様に、型 `object` またはインターフェイス型の値が構造体型に変換されると、ボックス化解除操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="51963-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="51963-193">クラス型に対する同じ操作との主な違いは、ボックス化とボックス化解除によって、ボックス化されたインスタンスの内外に構造体の値がコピーされることです。</span><span class="sxs-lookup"><span data-stu-id="51963-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="51963-194">したがって、ボックス化またはボックス化解除の後に、ボックス化解除された構造体に加えられた変更は、ボックス化された構造体に反映されません。</span><span class="sxs-lookup"><span data-stu-id="51963-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="51963-195">@No__t-0 (`Equals`、`GetHashCode`、または `ToString` など) から継承された仮想メソッドをオーバーライドする場合、構造体型のインスタンスを介して仮想メソッドを呼び出すと、ボックス化が発生しません。</span><span class="sxs-lookup"><span data-stu-id="51963-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="51963-196">これは、構造体が型パラメーターとして使用されていて、型パラメーター型のインスタンスを通じて呼び出しが行われる場合にも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="51963-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="51963-197">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="51963-197">For example:</span></span>
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

<span data-ttu-id="51963-198">プログラムの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="51963-198">The output of the program is:</span></span>
```console
1
2
3
```

<span data-ttu-id="51963-199">@No__t-0 で副作用が発生するのは不適切なスタイルですが、この例では `x.ToString()` の3回の呼び出しに対してボックス化が実行されていないことを示しています。</span><span class="sxs-lookup"><span data-stu-id="51963-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="51963-200">同様に、制約された型パラメーターのメンバーにアクセスするときに、ボックス化が暗黙的に行われることはありません。</span><span class="sxs-lookup"><span data-stu-id="51963-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="51963-201">たとえば、@no__t 0 のインターフェイスに、値を変更するために使用できるメソッド `Increment` が含まれているとします。</span><span class="sxs-lookup"><span data-stu-id="51963-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="51963-202">@No__t-0 が制約として使用されている場合、`Increment` メソッドの実装は、`Increment` が呼び出された変数への参照を使用して呼び出され、ボックス化されたコピーではありません。</span><span class="sxs-lookup"><span data-stu-id="51963-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

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

<span data-ttu-id="51963-203">@No__t-0 の最初の呼び出しでは、変数 `x` の値が変更されます。</span><span class="sxs-lookup"><span data-stu-id="51963-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="51963-204">これは `Increment` への2回目の呼び出しと同じではありません。この場合、`x` のボックス化されたコピーの値が変更されます。</span><span class="sxs-lookup"><span data-stu-id="51963-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="51963-205">このため、プログラムの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="51963-205">Thus, the output of the program is:</span></span>
```console
0
1
1
```

<span data-ttu-id="51963-206">ボックス化とボックス化解除の詳細については、「[ボックス化とボックス化解除](types.md#boxing-and-unboxing)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="51963-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="51963-207">意味</span><span class="sxs-lookup"><span data-stu-id="51963-207">Meaning of this</span></span>

<span data-ttu-id="51963-208">クラスのインスタンスコンストラクターまたはインスタンス関数メンバー内では、@no__t 0 が値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="51963-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="51963-209">したがって、`this` を使用して、関数メンバーが呼び出されたインスタンスを参照することはできますが、クラスの関数メンバーの `this` に割り当てることはできません。</span><span class="sxs-lookup"><span data-stu-id="51963-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="51963-210">構造体のインスタンスコンストラクター内で `this` は構造体型の `out` パラメーターに対応します。また、構造体のインスタンス関数メンバー内では、@no__t は、構造体型の `ref` パラメーターに対応します。</span><span class="sxs-lookup"><span data-stu-id="51963-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="51963-211">どちらの場合も、@no__t 0 は変数として分類され、`this` に代入するか、これを `ref` または `out` パラメーターとして渡すことによって、関数メンバーが呼び出された構造体全体を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="51963-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="51963-212">フィールド初期化子</span><span class="sxs-lookup"><span data-stu-id="51963-212">Field initializers</span></span>

<span data-ttu-id="51963-213">「[既定値](structs.md#default-values)」で説明されているように、構造体の既定値は、すべての値の型フィールドを既定値に設定し、すべての参照型フィールドを `null` に設定した結果の値で構成されます。</span><span class="sxs-lookup"><span data-stu-id="51963-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="51963-214">このため、構造体では、インスタンスフィールドの宣言に変数初期化子を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="51963-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="51963-215">この制限は、インスタンスフィールドにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="51963-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="51963-216">構造体の静的フィールドには、変数初期化子を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="51963-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="51963-217">例</span><span class="sxs-lookup"><span data-stu-id="51963-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="51963-218">インスタンスフィールドの宣言に変数初期化子が含まれているため、エラーが発生しています。</span><span class="sxs-lookup"><span data-stu-id="51963-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="51963-219">コンストラクター</span><span class="sxs-lookup"><span data-stu-id="51963-219">Constructors</span></span>

<span data-ttu-id="51963-220">クラスとは異なり、構造体では、パラメーターなしのインスタンスコンストラクターを宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="51963-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="51963-221">代わりに、すべての構造体には、暗黙的にパラメーターなしのインスタンスコンストラクターがあります。このコンストラクターは、すべての値型フィールドを既定値に設定し、すべての参照型フィールドを null ([既定のコンストラクター](types.md#default-constructors)) に設定した結果の値を常に返します。</span><span class="sxs-lookup"><span data-stu-id="51963-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="51963-222">構造体は、パラメーターを持つインスタンスコンストラクターを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="51963-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="51963-223">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="51963-223">For example</span></span>
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

<span data-ttu-id="51963-224">上記の宣言を指定した場合、ステートメントは</span><span class="sxs-lookup"><span data-stu-id="51963-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="51963-225">どちらも、`x`、@no__t が0に初期化された @no__t 0 を作成します。</span><span class="sxs-lookup"><span data-stu-id="51963-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="51963-226">構造体インスタンスコンストラクターには、フォームのコンストラクター初期化子を含めることはできません `base(...)`。</span><span class="sxs-lookup"><span data-stu-id="51963-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="51963-227">Struct インスタンスコンストラクターがコンストラクター初期化子を指定していない場合、@no__t 0 変数は構造体型の `out` パラメーターに対応し、`out` パラメーターと同様に、`this` は確実に代入される必要があります ([明確な代入](variables.md#definite-assignment)) を指定します。</span><span class="sxs-lookup"><span data-stu-id="51963-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="51963-228">Struct インスタンスコンストラクターでコンストラクター初期化子が指定されている場合、@no__t 0 変数は構造体型の `ref` パラメーターに対応し、`ref` パラメーターと同様に、`this` はコンストラクター本体のエントリで確実に割り当てられていると見なされます.</span><span class="sxs-lookup"><span data-stu-id="51963-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="51963-229">次に示すインスタンスコンストラクターの実装について考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="51963-229">Consider the instance constructor implementation below:</span></span>
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

<span data-ttu-id="51963-230">構築されている構造体のすべてのフィールドが確実に代入されるまで、インスタンスメンバー関数 (プロパティ `X` および `Y` を含む) を呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="51963-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="51963-231">唯一の例外には、自動的に実装されるプロパティ ([自動的に実装](classes.md#automatically-implemented-properties)されるプロパティ) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="51963-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="51963-232">明確な代入規則 ([単純代入式](variables.md#simple-assignment-expressions)) は、その構造体型のインスタンスコンストラクター内の構造体型の自動プロパティへの割り当てを明示的に除外します。このような代入は、隠ぺいされたの割り当てと見なされます。自動プロパティのバッキングフィールド。</span><span class="sxs-lookup"><span data-stu-id="51963-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="51963-233">したがって、次のものを使用できます。</span><span class="sxs-lookup"><span data-stu-id="51963-233">Thus, the following is allowed:</span></span>

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

### <a name="destructors"></a><span data-ttu-id="51963-234">デストラクター</span><span class="sxs-lookup"><span data-stu-id="51963-234">Destructors</span></span>

<span data-ttu-id="51963-235">構造体は、デストラクターの宣言を許可されていません。</span><span class="sxs-lookup"><span data-stu-id="51963-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="51963-236">静的コンストラクター</span><span class="sxs-lookup"><span data-stu-id="51963-236">Static constructors</span></span>

<span data-ttu-id="51963-237">構造体の静的コンストラクターは、クラスの場合と同じ規則のほとんどに従います。</span><span class="sxs-lookup"><span data-stu-id="51963-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="51963-238">構造体型の静的コンストラクターの実行は、アプリケーションドメイン内で発生する次のイベントの最初のイベントによってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="51963-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="51963-239">構造体型の静的メンバーが参照されています。</span><span class="sxs-lookup"><span data-stu-id="51963-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="51963-240">構造体型の明示的に宣言されたコンストラクターが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="51963-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="51963-241">構造体型の既定値 ([既定値](structs.md#default-values)) を作成しても、静的コンストラクターはトリガーされません。</span><span class="sxs-lookup"><span data-stu-id="51963-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="51963-242">(例として、配列内の要素の初期値が挙げられます)。</span><span class="sxs-lookup"><span data-stu-id="51963-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="51963-243">構造体の例</span><span class="sxs-lookup"><span data-stu-id="51963-243">Struct examples</span></span>

<span data-ttu-id="51963-244">次に、`struct` 型を使用して、定義済みの言語の型と同様に使用できる型を作成する2つの大きな例を示しますが、セマンティクスは変更されています。</span><span class="sxs-lookup"><span data-stu-id="51963-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="51963-245">データベースの整数型</span><span class="sxs-lookup"><span data-stu-id="51963-245">Database integer type</span></span>

<span data-ttu-id="51963-246">次の @no__t 0 構造体は、`int` 型の値の完全なセットと、不明な値を示す追加の状態を表すことができる整数型を実装します。</span><span class="sxs-lookup"><span data-stu-id="51963-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="51963-247">これらの特性を持つ型は、データベースで一般的に使用されます。</span><span class="sxs-lookup"><span data-stu-id="51963-247">A type with these characteristics is commonly used in databases.</span></span>

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

### <a name="database-boolean-type"></a><span data-ttu-id="51963-248">データベースのブール型</span><span class="sxs-lookup"><span data-stu-id="51963-248">Database boolean type</span></span>

<span data-ttu-id="51963-249">次の @no__t 0 構造体は、3つの値を持つ論理型を実装します。</span><span class="sxs-lookup"><span data-stu-id="51963-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="51963-250">この型に指定できる値は、0、`DBBool.False`、および `DBBool.Null` の @no__t です。ここで、`Null` のメンバーは不明な値を示します。</span><span class="sxs-lookup"><span data-stu-id="51963-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="51963-251">このような3つの値の論理型は、データベースで一般的に使用されます。</span><span class="sxs-lookup"><span data-stu-id="51963-251">Such three-valued logical types are commonly used in databases.</span></span>

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
