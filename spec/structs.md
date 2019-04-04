---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229606"
---
# <a name="structs"></a><span data-ttu-id="ec473-101">構造体</span><span class="sxs-lookup"><span data-stu-id="ec473-101">Structs</span></span>

<span data-ttu-id="ec473-102">データ メンバーおよび関数メンバーを格納できるデータ構造を表しているという点では、構造体をクラスに似ています。</span><span class="sxs-lookup"><span data-stu-id="ec473-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="ec473-103">ただし、クラスと異なり、構造体は値型で、ヒープ割り当ては必要ありません。</span><span class="sxs-lookup"><span data-stu-id="ec473-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="ec473-104">構造体の型の変数は、クラス型の変数にデータをオブジェクトとして知らへの参照が含まれていますが、構造体のデータを直接格納します。</span><span class="sxs-lookup"><span data-stu-id="ec473-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="ec473-105">構造体は、値セマンティクスを持つ小規模なデータ構造に特に便利です。</span><span class="sxs-lookup"><span data-stu-id="ec473-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="ec473-106">構造体の主な例としては、複素数、座標系のポイント、ディクショナリのキーと値のペアなどがあります。</span><span class="sxs-lookup"><span data-stu-id="ec473-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="ec473-107">これらのデータ構造のキーは必要があるいくつかのデータ メンバー、継承や参照の id の使用が必要としないことと、簡単に実装できる値セマンティクスを使用して、割り当てが、参照の代わりに値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="ec473-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="ec473-108">」の説明に従って[単純型](types.md#simple-types)、単純型など c# の場合は、によって提供される`int`、 `double`、および`bool`、実際にはすべての構造体型。</span><span class="sxs-lookup"><span data-stu-id="ec473-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="ec473-109">これらの定義済みの型が構造体であるのと同様にも構造体と c# 言語で新しい「プリミティブ」型を実装するためにオーバー ロードする演算子を使用可能です。</span><span class="sxs-lookup"><span data-stu-id="ec473-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="ec473-110">この章の最後にこのような型の 2 つの例が与えられます ([構造体の例](structs.md#struct-examples))。</span><span class="sxs-lookup"><span data-stu-id="ec473-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="ec473-111">構造体の宣言</span><span class="sxs-lookup"><span data-stu-id="ec473-111">Struct declarations</span></span>

<span data-ttu-id="ec473-112">A *struct_declaration*は、 *type_declaration* ([入力宣言](namespaces.md#type-declarations)) 新しい構造体を宣言します。</span><span class="sxs-lookup"><span data-stu-id="ec473-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="ec473-113">A *struct_declaration*オプションのセットから成る*属性*([属性](attributes.md)) オプションのセットと、その後*struct_modifier*s ([構造体修飾子](structs.md#struct-modifiers)) と省略可能なその後`partial`の後に、キーワード修飾子`struct`と*識別子*後に、構造体の名前を示す、省略可能な*type_parameter_list*仕様 ([パラメーター入力](classes.md#type-parameters)) と省略可能なその後*struct_interfaces*仕様 ([Partial 修飾子](structs.md#partial-modifier))) と省略可能なその後*type_parameter_constraints_clause*s 仕様 ([パラメーターの制約入力](classes.md#type-parameter-constraints))、その後に、 *struct_body* ([構造体の本体](structs.md#struct-body))、セミコロンで必要に応じてその後にします。</span><span class="sxs-lookup"><span data-stu-id="ec473-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="ec473-114">構造体の修飾子</span><span class="sxs-lookup"><span data-stu-id="ec473-114">Struct modifiers</span></span>

<span data-ttu-id="ec473-115">A *struct_declaration*構造体の修飾子のシーケンスを必要に応じて含めることができます。</span><span class="sxs-lookup"><span data-stu-id="ec473-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

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

<span data-ttu-id="ec473-116">同じ修飾子を構造体宣言内で複数回のコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="ec473-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="ec473-117">構造体の宣言の修飾子がクラス宣言のものと同じ意味を持ちます ([クラス クラスの宣言](classes.md#class-declarations))。</span><span class="sxs-lookup"><span data-stu-id="ec473-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="ec473-118">Partial 識別子</span><span class="sxs-lookup"><span data-stu-id="ec473-118">Partial modifier</span></span>

<span data-ttu-id="ec473-119">`partial`修飾子には、ことを示しますこの*struct_declaration*部分型の宣言です。</span><span class="sxs-lookup"><span data-stu-id="ec473-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="ec473-120">指定された規則に従って、それを囲む名前空間または型の宣言内に同じ名前を持つ複数の部分的な構造体宣言は、1 つの構造体の宣言を形成する結合、[部分型](classes.md#partial-types)します。</span><span class="sxs-lookup"><span data-stu-id="ec473-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="ec473-121">構造体のインターフェイス</span><span class="sxs-lookup"><span data-stu-id="ec473-121">Struct interfaces</span></span>

<span data-ttu-id="ec473-122">構造体の宣言を含めることができます、 *struct_interfaces*仕様、構造体のケースが特定のインターフェイス型を直接実装すると呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="ec473-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="ec473-123">インターフェイスの実装については、説明でさらに[インターフェイスの実装](interfaces.md#interface-implementations)します。</span><span class="sxs-lookup"><span data-stu-id="ec473-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="ec473-124">構造体の本文</span><span class="sxs-lookup"><span data-stu-id="ec473-124">Struct body</span></span>

<span data-ttu-id="ec473-125">*Struct_body*構造体の構造体のメンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="ec473-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="ec473-126">構造体のメンバー</span><span class="sxs-lookup"><span data-stu-id="ec473-126">Struct members</span></span>

<span data-ttu-id="ec473-127">構造体のメンバーで導入されたメンバーで構成されているその*struct_member_declaration*s およびメンバーの型から継承`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

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

<span data-ttu-id="ec473-128">記載の相違点を除いて[クラスと構造体の違い](structs.md#class-and-struct-differences)で提供されるクラスのメンバーの説明については、[クラス メンバー](classes.md#class-members)を通じて[反復子](classes.md#iterators)構造体に適用されますメンバーでも使用します。</span><span class="sxs-lookup"><span data-stu-id="ec473-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="ec473-129">クラスと構造体の違い</span><span class="sxs-lookup"><span data-stu-id="ec473-129">Class and struct differences</span></span>

<span data-ttu-id="ec473-130">構造体は、いくつかの重要な方法でクラスとは異なります。</span><span class="sxs-lookup"><span data-stu-id="ec473-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="ec473-131">構造体は、値型 ([値セマンティクス](structs.md#value-semantics))。</span><span class="sxs-lookup"><span data-stu-id="ec473-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="ec473-132">すべての構造体型は暗黙的にクラスから継承`System.ValueType`([継承](structs.md#inheritance))。</span><span class="sxs-lookup"><span data-stu-id="ec473-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="ec473-133">構造体の型の変数への代入が割り当てられている値のコピーを作成します ([割り当て](structs.md#assignment))。</span><span class="sxs-lookup"><span data-stu-id="ec473-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="ec473-134">構造体の既定値は、すべての値型フィールドが既定値をすべて参照型フィールドに設定された値`null`([既定値](structs.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="ec473-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="ec473-135">ボックス化とボックス化解除の操作を使用して、構造体型の間で変換および`object`([ボックス化とボックス化解除](structs.md#boxing-and-unboxing))。</span><span class="sxs-lookup"><span data-stu-id="ec473-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="ec473-136">意味`this`は構造体の異なる ([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="ec473-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="ec473-137">変数の初期化子を含めるには構造体のインスタンス フィールドの宣言は許可されていません ([フィールド初期化子](structs.md#field-initializers))。</span><span class="sxs-lookup"><span data-stu-id="ec473-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="ec473-138">パラメーターなしインターフェイス コンス トラクターを宣言する構造体は許可されていません ([コンス トラクター](structs.md#constructors))。</span><span class="sxs-lookup"><span data-stu-id="ec473-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="ec473-139">構造体がデストラクターを宣言することはできない ([デストラクター](structs.md#destructors))。</span><span class="sxs-lookup"><span data-stu-id="ec473-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="ec473-140">値のセマンティクス</span><span class="sxs-lookup"><span data-stu-id="ec473-140">Value semantics</span></span>

<span data-ttu-id="ec473-141">構造体は、値型 ([値の型](types.md#value-types)) と値のセマンティクスを持つユーザーと呼びます。</span><span class="sxs-lookup"><span data-stu-id="ec473-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="ec473-142">一方で、クラスでは、参照型 ([参照型](types.md#reference-types)) と、参照セマンティクスを持つと言います。</span><span class="sxs-lookup"><span data-stu-id="ec473-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="ec473-143">構造体の型の変数は、クラス型の変数にデータをオブジェクトとして知らへの参照が含まれていますが、構造体のデータを直接格納します。</span><span class="sxs-lookup"><span data-stu-id="ec473-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="ec473-144">構造体と`B`型のインスタンス フィールドが含まれています`A`と`A`、構造体型は、コンパイル時エラーには`A`に依存する`B`から構築された型または`B`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="ec473-145">構造体`X`***に直接依存***構造体`Y`場合`X`型のインスタンス フィールドを含む`Y`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="ec473-146">この定義を指定するには、構造体が依存している構造体の完全なセットが推移的閉包、***に直接依存***リレーションシップ。</span><span class="sxs-lookup"><span data-stu-id="ec473-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="ec473-147">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ec473-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="ec473-148">エラーは、ため`Node`独自の型のインスタンス フィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ec473-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="ec473-149">別の例</span><span class="sxs-lookup"><span data-stu-id="ec473-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="ec473-150">ために、エラーの種類ごと`A`、`B`と`C`互いに依存しています。</span><span class="sxs-lookup"><span data-stu-id="ec473-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="ec473-151">クラスは 2 つの変数が同じオブジェクトを参照できるため、他の変数によって参照されるオブジェクトに影響を与える 1 つの変数に対する操作。</span><span class="sxs-lookup"><span data-stu-id="ec473-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="ec473-152">構造体では、各変数がある独自のデータのコピー (の場合を除く`ref`と`out`パラメーター変数) とに影響を与える、他のいずれかの操作にことはできません。</span><span class="sxs-lookup"><span data-stu-id="ec473-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="ec473-153">さらに、構造体は参照型ではないため、ことはできませんを構造体の型の値の`null`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="ec473-154">宣言について考えます。</span><span class="sxs-lookup"><span data-stu-id="ec473-154">Given the declaration</span></span>
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
<span data-ttu-id="ec473-155">コード フラグメントは、</span><span class="sxs-lookup"><span data-stu-id="ec473-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="ec473-156">値を出力します`10`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-156">outputs the value `10`.</span></span> <span data-ttu-id="ec473-157">割り当て`a`に`b`、値のコピーを作成および`b`はそのためへの割り当てによって影響を受けません`a.x`。</span><span class="sxs-lookup"><span data-stu-id="ec473-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="ec473-158">`Point`されて代わりに出力になりますが、クラスとして宣言すると、`100`ため`a`と`b`同じオブジェクトを参照します。</span><span class="sxs-lookup"><span data-stu-id="ec473-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="ec473-159">継承</span><span class="sxs-lookup"><span data-stu-id="ec473-159">Inheritance</span></span>

<span data-ttu-id="ec473-160">すべての構造体型は暗黙的にクラスから継承`System.ValueType`であり、さらに、クラスを継承`object`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="ec473-161">構造体の宣言が実装されるインターフェイスの一覧を指定できますが、基底クラスを指定する構造体の宣言のことはできません。</span><span class="sxs-lookup"><span data-stu-id="ec473-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="ec473-162">構造体の型は、抽象されないしは常に暗黙的にシールします。</span><span class="sxs-lookup"><span data-stu-id="ec473-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="ec473-163">`abstract`と`sealed`構造体の宣言で 修飾子は使用できないためです。</span><span class="sxs-lookup"><span data-stu-id="ec473-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="ec473-164">構造体メンバーの宣言されたアクセシビリティをすることはできませんので、構造体の継承がサポートされていない`protected`または`protected internal`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="ec473-165">構造体の関数メンバーにすることはできません`abstract`または`virtual`、および`override`のみから継承されたメソッドをオーバーライドするのには、修飾子を使用してください。`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="ec473-166">代入</span><span class="sxs-lookup"><span data-stu-id="ec473-166">Assignment</span></span>

<span data-ttu-id="ec473-167">構造体の型の変数への代入では、割り当てられている値のコピーを作成します。</span><span class="sxs-lookup"><span data-stu-id="ec473-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="ec473-168">これは、コピー、参照が参照によって識別されるオブジェクトではなく、クラスの型の変数に割り当てと異なります。</span><span class="sxs-lookup"><span data-stu-id="ec473-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="ec473-169">構造体の値を持つパラメーターとして渡されるまたは関数メンバーの結果として返されるときに、割り当てと同様に、構造体のコピーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="ec473-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="ec473-170">構造体を使用して関数のメンバーへの参照で渡すことが、`ref`または`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="ec473-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="ec473-171">プロパティまたはインデクサーの構造体には、代入式のターゲットがある場合は、プロパティまたはインデクサーのアクセスに関連付けられたインスタンス式を変数として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="ec473-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="ec473-172">インスタンス式が値として分類は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ec473-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="ec473-173">さらに詳しく記載されて[単純な代入](expressions.md#simple-assignment)します。</span><span class="sxs-lookup"><span data-stu-id="ec473-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="ec473-174">既定の値</span><span class="sxs-lookup"><span data-stu-id="ec473-174">Default values</span></span>

<span data-ttu-id="ec473-175">」の説明に従って[既定値](variables.md#default-values)、ユーザーが作成されるときでいくつかの種類の変数は、既定値に自動的に初期化します。</span><span class="sxs-lookup"><span data-stu-id="ec473-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="ec473-176">クラスの型および他の参照型の変数は、この既定値は`null`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="ec473-177">ただし、構造体は、値の型にできないため`null`、構造体の既定値は、すべての値型フィールドが既定値をすべて参照型フィールドに設定された値`null`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="ec473-178">参照する、`Point`上、例では、構造体で宣言されました。</span><span class="sxs-lookup"><span data-stu-id="ec473-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="ec473-179">それぞれを初期化します`Point`設定された値の配列内の`x`と`y`フィールドをゼロにします。</span><span class="sxs-lookup"><span data-stu-id="ec473-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="ec473-180">構造体の既定値は、構造体の既定のコンス トラクターによって返される値に対応 ([既定のコンス トラクター](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="ec473-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="ec473-181">クラスとは異なり、パラメーターなしコンス トラクターを宣言する構造体は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="ec473-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="ec473-182">暗黙的に常に結果が既定値をすべて参照型フィールドを値型のすべてのフィールドを設定の値を返すパラメーターなしインターフェイス コンス トラクターを代わりに、各構造体`null`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="ec473-183">構造体を設計すると、既定の初期化状態に有効な状態を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ec473-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="ec473-184">例</span><span class="sxs-lookup"><span data-stu-id="ec473-184">In the example</span></span>
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
<span data-ttu-id="ec473-185">ユーザー定義のインスタンス コンス トラクターのみ呼び出される場所に明示的に null 値を防ぎます。</span><span class="sxs-lookup"><span data-stu-id="ec473-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="ec473-186">場合で、`KeyValuePair`変数は、既定値の初期化の対象は、`key`と`value`フィールドが null をして、構造体は、この状態を処理するために準備する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ec473-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="ec473-187">ボックス化とボックス化解除</span><span class="sxs-lookup"><span data-stu-id="ec473-187">Boxing and unboxing</span></span>

<span data-ttu-id="ec473-188">クラス型の値を型に変換できます`object`またはコンパイル時に別の種類として、参照を扱うだけで、クラスによって実装されているインターフェイス型。</span><span class="sxs-lookup"><span data-stu-id="ec473-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="ec473-189">型の値を同様に、`object`またはインターフェイス型の値は、参照を変更することがなくクラス型に変換できます (ただしもちろん実行時の型チェックが必要なこの場合)。</span><span class="sxs-lookup"><span data-stu-id="ec473-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="ec473-190">構造体が参照型ではないために、構造体の型をこれらの操作が異なる方法で実装されます。</span><span class="sxs-lookup"><span data-stu-id="ec473-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="ec473-191">構造体の型の値を型に変換するときに`object`または構造体によって実装されるインターフェイス型にボックス化操作が実行されます。</span><span class="sxs-lookup"><span data-stu-id="ec473-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="ec473-192">同様に、型の値と`object`またはインターフェイス型の値が構造体の型に変換、ボックス化解除の操作が行われています。</span><span class="sxs-lookup"><span data-stu-id="ec473-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="ec473-193">クラス型に対して同じ操作からの重要な違いは、ボックス化とボックス化解除がコピーされる構造体の値にするか、ボックス化されたインスタンスからです。</span><span class="sxs-lookup"><span data-stu-id="ec473-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="ec473-194">そのため、ボックス化およびボックス化解除操作では、次のボックス化解除された構造体に加えられた変更は反映されませんでボックス化された構造体。</span><span class="sxs-lookup"><span data-stu-id="ec473-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="ec473-195">構造体の型がから継承された仮想メソッドをオーバーライドするときに`System.Object`(など`Equals`、 `GetHashCode`、または`ToString`)、構造体の型のインスタンス経由で仮想メソッドの呼び出しが発生するボックス化は発生しません。</span><span class="sxs-lookup"><span data-stu-id="ec473-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="ec473-196">これは、構造体は型パラメーターとして使用され、型パラメーターの型のインスタンスを通じて、呼び出しが発生した場合でも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="ec473-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="ec473-197">例:</span><span class="sxs-lookup"><span data-stu-id="ec473-197">For example:</span></span>
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

<span data-ttu-id="ec473-198">プログラムの出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ec473-198">The output of the program is:</span></span>
```
1
2
3
```

<span data-ttu-id="ec473-199">不適切なスタイルの`ToString`副作用がある、この例の 3 つの呼び出しのボックス化が発生しなかったこと`x.ToString()`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="ec473-200">同様に、決して暗黙的にボックス化は、制約付きの型パラメーターのメンバーにアクセスするときに発生します。</span><span class="sxs-lookup"><span data-stu-id="ec473-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="ec473-201">たとえば、インターフェイス`ICounter`メソッドを含む`Increment`値の変更に使用できます。</span><span class="sxs-lookup"><span data-stu-id="ec473-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="ec473-202">場合`ICounter`の実装、制約として使用、`Increment`メソッドを呼び出すと、変数への参照を`Increment`で、ボックス化されたコピーではありませんが呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="ec473-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

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

<span data-ttu-id="ec473-203">最初の呼び出し`Increment`変数に値を変更します`x`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="ec473-204">これは、2 番目の呼び出しに相当しない`Increment`、ボックス化されたコピーの値が変更されます`x`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="ec473-205">そのため、プログラムの出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ec473-205">Thus, the output of the program is:</span></span>
```
0
1
1
```

<span data-ttu-id="ec473-206">ボックス化とボックス化解除の詳細については、[ボックス化とボックス化解除](types.md#boxing-and-unboxing)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ec473-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="ec473-207">この意味</span><span class="sxs-lookup"><span data-stu-id="ec473-207">Meaning of this</span></span>

<span data-ttu-id="ec473-208">インスタンス コンス トラクターまたはクラスの関数メンバーのインスタンス内で`this`値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="ec473-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="ec473-209">そのため、while `this` 、インスタンスを参照するために使用できますを関数メンバーの呼び出しのことはできませんに割り当てる`this`クラスの関数メンバーで。</span><span class="sxs-lookup"><span data-stu-id="ec473-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="ec473-210">構造体のインスタンス コンス トラクター内で`this`に対応、`out`パラメーターと、構造体のインスタンス関数メンバー内で、構造体型の`this`に対応、`ref`構造体の型のパラメーター。</span><span class="sxs-lookup"><span data-stu-id="ec473-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="ec473-211">どちらの場合も、`this`変数として分類されますを割り当てることによって呼び出された関数のメンバーを構造体全体を変更することも、`this`として渡すことにより、`ref`または`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="ec473-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="ec473-212">フィールド初期化子</span><span class="sxs-lookup"><span data-stu-id="ec473-212">Field initializers</span></span>

<span data-ttu-id="ec473-213">」の説明に従って[既定値](structs.md#default-values)、構造体の既定値が既定値をすべて参照型フィールドを値型のすべてのフィールドを設定から結果の値から成る`null`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="ec473-214">このため、構造体を変数初期化子を含めるインスタンス フィールドの宣言は許可されません。</span><span class="sxs-lookup"><span data-stu-id="ec473-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="ec473-215">この制限は、インスタンス フィールドにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="ec473-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="ec473-216">構造体の静的フィールドは、変数の初期化子を含める許可されます。</span><span class="sxs-lookup"><span data-stu-id="ec473-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="ec473-217">例では、</span><span class="sxs-lookup"><span data-stu-id="ec473-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="ec473-218">エラーが、インスタンス フィールドの宣言が変数初期化子が含まれるためです。</span><span class="sxs-lookup"><span data-stu-id="ec473-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="ec473-219">コンストラクター</span><span class="sxs-lookup"><span data-stu-id="ec473-219">Constructors</span></span>

<span data-ttu-id="ec473-220">クラスとは異なり、パラメーターなしコンス トラクターを宣言する構造体は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="ec473-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="ec473-221">暗黙的に常に値型のすべてのフィールドが既定値をすべて参照型フィールドを null に設定から結果値を返すパラメーターなしインターフェイス コンス トラクターを代わりに、各構造体 ([既定のコンス トラクター](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="ec473-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="ec473-222">構造体には、パラメーターを持つインスタンス コンス トラクターを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="ec473-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="ec473-223">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ec473-223">For example</span></span>
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

<span data-ttu-id="ec473-224">上記の宣言ステートメント</span><span class="sxs-lookup"><span data-stu-id="ec473-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="ec473-225">両方を作成、`Point`で`x`と`y`0 に初期化します。</span><span class="sxs-lookup"><span data-stu-id="ec473-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="ec473-226">構造体のインスタンス コンス トラクターは、フォームのコンス トラクター初期化子を含めるには許可されていません`base(...)`します。</span><span class="sxs-lookup"><span data-stu-id="ec473-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="ec473-227">構造体のインスタンス コンス トラクターは、コンス トラクター初期化子を指定しない場合、`this`変数に対応して、`out`類似しています、構造体の型のパラメーター、`out`パラメーター、 `this` (を間違いなく割り当てする必要があります[確実な代入](variables.md#definite-assignment)) が、コンス トラクターを返すすべての場所にします。</span><span class="sxs-lookup"><span data-stu-id="ec473-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="ec473-228">構造体のインスタンス コンス トラクターは、コンス トラクター初期化子を指定する場合、`this`変数に対応して、`ref`類似しています、構造体の型のパラメーターを`ref`パラメーター、`this`で確実に割り当てられていると見なされますコンス トラクター本体のエントリ。</span><span class="sxs-lookup"><span data-stu-id="ec473-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="ec473-229">インスタンス コンス トラクターの実装を検討してください。</span><span class="sxs-lookup"><span data-stu-id="ec473-229">Consider the instance constructor implementation below:</span></span>
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

<span data-ttu-id="ec473-230">インスタンス メンバー関数 (プロパティの set アクセサーを含む`X`と`Y`) 構築される構造体のすべてのフィールドが必ず割り当てられるまでに呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="ec473-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="ec473-231">唯一の例外が自動的に実装されたプロパティが含まれます ([自動実装プロパティ](classes.md#automatically-implemented-properties))。</span><span class="sxs-lookup"><span data-stu-id="ec473-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="ec473-232">確実な代入ルール ([単純な代入式](variables.md#simple-assignment-expressions)) 具体的には、インスタンス コンス トラクター内でその構造体の型の構造体型の自動プロパティへの割り当てを除外します。 このような割り当ては、確実なと見なされます。自動プロパティの非表示のバッキング フィールドの割り当て。</span><span class="sxs-lookup"><span data-stu-id="ec473-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="ec473-233">したがって、次は使用できます。</span><span class="sxs-lookup"><span data-stu-id="ec473-233">Thus, the following is allowed:</span></span>

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

### <a name="destructors"></a><span data-ttu-id="ec473-234">デストラクター</span><span class="sxs-lookup"><span data-stu-id="ec473-234">Destructors</span></span>

<span data-ttu-id="ec473-235">構造体は、デストラクターを宣言は許可されません。</span><span class="sxs-lookup"><span data-stu-id="ec473-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="ec473-236">静的コンストラクター</span><span class="sxs-lookup"><span data-stu-id="ec473-236">Static constructors</span></span>

<span data-ttu-id="ec473-237">構造体の静的コンス トラクターでは、ほとんどのクラスと同じ規則に従います。</span><span class="sxs-lookup"><span data-stu-id="ec473-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="ec473-238">構造体の型の静的コンス トラクターの実行は、次のイベントの最初のアプリケーション ドメイン内に発生するによってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="ec473-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="ec473-239">構造体の型の静的メンバーが参照されます。</span><span class="sxs-lookup"><span data-stu-id="ec473-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="ec473-240">構造体の型の明示的に宣言されたコンス トラクターが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ec473-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="ec473-241">既定値の作成 ([既定値](structs.md#default-values)) 構造体の型が開始されない、静的コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="ec473-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="ec473-242">(この例は、配列内の要素の初期値です)。</span><span class="sxs-lookup"><span data-stu-id="ec473-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="ec473-243">構造体の例</span><span class="sxs-lookup"><span data-stu-id="ec473-243">Struct examples</span></span>

<span data-ttu-id="ec473-244">使用して 2 つの重要な例を次に示します`struct`同様に、言語が変更されたセマンティクスを持つ定義済みの型を使用できる型を作成する型。</span><span class="sxs-lookup"><span data-stu-id="ec473-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="ec473-245">データベースの整数型</span><span class="sxs-lookup"><span data-stu-id="ec473-245">Database integer type</span></span>

<span data-ttu-id="ec473-246">`DBInt`下構造体の値の完全なセットを表すことのできる整数型の実装、`int`型宣言および不明な値を示す追加の状態。</span><span class="sxs-lookup"><span data-stu-id="ec473-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="ec473-247">これらの特性を持つ型がデータベースでよく使用されます。</span><span class="sxs-lookup"><span data-stu-id="ec473-247">A type with these characteristics is commonly used in databases.</span></span>

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

### <a name="database-boolean-type"></a><span data-ttu-id="ec473-248">データベースのブール型</span><span class="sxs-lookup"><span data-stu-id="ec473-248">Database boolean type</span></span>

<span data-ttu-id="ec473-249">`DBBool`下構造体は、3 値論理の一種を実装します。</span><span class="sxs-lookup"><span data-stu-id="ec473-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="ec473-250">この型の値には`DBBool.True`、 `DBBool.False`、および`DBBool.Null`、場所、`Null`メンバーが不明な値を示します。</span><span class="sxs-lookup"><span data-stu-id="ec473-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="ec473-251">このような 3 つの値の論理型は、データベースでよく使用されます。</span><span class="sxs-lookup"><span data-stu-id="ec473-251">Such three-valued logical types are commonly used in databases.</span></span>

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
