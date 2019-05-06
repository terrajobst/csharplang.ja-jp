---
ms.openlocfilehash: a8ad8a8b3eda1d00fa745bd92e4371eacc36b79f
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229737"
---
# <a name="attributes"></a><span data-ttu-id="19388-101">属性</span><span class="sxs-lookup"><span data-stu-id="19388-101">Attributes</span></span>

<span data-ttu-id="19388-102">C# 言語の多くは、プログラマは、プログラムで定義されたエンティティの宣言型の情報を指定できます。</span><span class="sxs-lookup"><span data-stu-id="19388-102">Much of the C# language enables the programmer to specify declarative information about the entities defined in the program.</span></span> <span data-ttu-id="19388-103">修飾して、クラスのメソッドのアクセシビリティを指定するなど、 *method_modifier*s `public`、 `protected`、 `internal`、および`private`します。</span><span class="sxs-lookup"><span data-stu-id="19388-103">For example, the accessibility of a method in a class is specified by decorating it with the *method_modifier*s `public`, `protected`, `internal`, and `private`.</span></span>

<span data-ttu-id="19388-104">C# では、新しい種類の宣言型の情報を作成***属性***します。</span><span class="sxs-lookup"><span data-stu-id="19388-104">C# enables programmers to invent new kinds of declarative information, called ***attributes***.</span></span> <span data-ttu-id="19388-105">プログラマは、さまざまなプログラム エンティティに属性を追加し、実行時環境での属性情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="19388-105">Programmers can then attach attributes to various program entities, and retrieve attribute information in a run-time environment.</span></span> <span data-ttu-id="19388-106">たとえば、フレームワークは、定義、`HelpAttribute`属性にこれらのプログラム要素からそのドキュメントへのマッピングを提供する (クラスとメソッド) などの特定のプログラム要素に配置できます。</span><span class="sxs-lookup"><span data-stu-id="19388-106">For instance, a framework might define a `HelpAttribute` attribute that can be placed on certain program elements (such as classes and methods) to provide a mapping from those program elements to their documentation.</span></span>

<span data-ttu-id="19388-107">属性は属性クラスの宣言によって定義されます ([属性クラス](attributes.md#attribute-classes))、位置指定引数と名前付きパラメーターである可能性があります ([位置指定パラメーターと名前付きパラメーター](attributes.md#positional-and-named-parameters))。</span><span class="sxs-lookup"><span data-stu-id="19388-107">Attributes are defined through the declaration of attribute classes ([Attribute classes](attributes.md#attribute-classes)), which may have positional and named parameters ([Positional and named parameters](attributes.md#positional-and-named-parameters)).</span></span> <span data-ttu-id="19388-108">属性の仕様を使用して C# プログラム内のエンティティに関連付けられている属性 ([属性の指定](attributes.md#attribute-specification))、属性のインスタンスとして実行時に取得することができます ([属性インスタンス](attributes.md#attribute-instances))。</span><span class="sxs-lookup"><span data-stu-id="19388-108">Attributes are attached to entities in a C# program using attribute specifications ([Attribute specification](attributes.md#attribute-specification)), and can be retrieved at run-time as attribute instances ([Attribute instances](attributes.md#attribute-instances)).</span></span>

## <a name="attribute-classes"></a><span data-ttu-id="19388-109">属性クラス</span><span class="sxs-lookup"><span data-stu-id="19388-109">Attribute classes</span></span>

<span data-ttu-id="19388-110">抽象クラスから派生するクラスを`System.Attribute`、直接的または間接的にするかどうかは、***属性クラス***します。</span><span class="sxs-lookup"><span data-stu-id="19388-110">A class that derives from the abstract class `System.Attribute`, whether directly or indirectly, is an ***attribute class***.</span></span> <span data-ttu-id="19388-111">属性クラスの宣言定義の種類を新しい***属性***宣言でに配置することができます。</span><span class="sxs-lookup"><span data-stu-id="19388-111">The declaration of an attribute class defines a new kind of ***attribute*** that can be placed on a declaration.</span></span> <span data-ttu-id="19388-112">サフィックスを持つ属性クラスの名前は慣例により、`Attribute`します。</span><span class="sxs-lookup"><span data-stu-id="19388-112">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="19388-113">属性の使用は可能性があります含めるか、このサフィックスを省略します。</span><span class="sxs-lookup"><span data-stu-id="19388-113">Uses of an attribute may either include or omit this suffix.</span></span>

### <a name="attribute-usage"></a><span data-ttu-id="19388-114">属性の使用方法</span><span class="sxs-lookup"><span data-stu-id="19388-114">Attribute usage</span></span>

<span data-ttu-id="19388-115">属性`AttributeUsage`([、AttributeUsage 属性](attributes.md#the-attributeusage-attribute)) 属性クラスの使用方法について説明するために使用します。</span><span class="sxs-lookup"><span data-stu-id="19388-115">The attribute `AttributeUsage` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)) is used to describe how an attribute class can be used.</span></span>

<span data-ttu-id="19388-116">`AttributeUsage` 位置指定パラメーターがあります ([位置指定パラメーターと名前付きパラメーター](attributes.md#positional-and-named-parameters)) 宣言を使用できますの種類を指定する属性クラスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="19388-116">`AttributeUsage` has a positional parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) that enables an attribute class to specify the kinds of declarations on which it can be used.</span></span> <span data-ttu-id="19388-117">例では、</span><span class="sxs-lookup"><span data-stu-id="19388-117">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

<span data-ttu-id="19388-118">という名前の属性クラスを定義します`SimpleAttribute`上に配置する*class_declaration*s と*interface_declaration*にのみです。</span><span class="sxs-lookup"><span data-stu-id="19388-118">defines an attribute class named `SimpleAttribute` that can be placed on *class_declaration*s and *interface_declaration*s only.</span></span> <span data-ttu-id="19388-119">例では、</span><span class="sxs-lookup"><span data-stu-id="19388-119">The example</span></span>

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

<span data-ttu-id="19388-120">いくつか示されて、`Simple`属性。</span><span class="sxs-lookup"><span data-stu-id="19388-120">shows several uses of the `Simple` attribute.</span></span> <span data-ttu-id="19388-121">この属性は名前で定義されて`SimpleAttribute`、この属性を使用する場合、`Attribute`サフィックスは省略できます。 その結果、短い名前`Simple`します。</span><span class="sxs-lookup"><span data-stu-id="19388-121">Although this attribute is defined with the name `SimpleAttribute`, when this attribute is used, the `Attribute` suffix may be omitted, resulting in the short name `Simple`.</span></span> <span data-ttu-id="19388-122">したがって、上記の例は、次の意味と同等です。</span><span class="sxs-lookup"><span data-stu-id="19388-122">Thus, the example above is semantically equivalent to the following:</span></span>

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

<span data-ttu-id="19388-123">`AttributeUsage` 名前付きパラメーターがあります ([位置指定パラメーターと名前付きパラメーター](attributes.md#positional-and-named-parameters)) と呼ばれる`AllowMultiple`、特定のエンティティの属性を複数回指定できるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="19388-123">`AttributeUsage` has a named parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) called `AllowMultiple`, which indicates whether the attribute can be specified more than once for a given entity.</span></span> <span data-ttu-id="19388-124">場合`AllowMultiple`属性クラスが true の場合は、その属性クラス、***属性を複数使用クラス***エンティティで複数回指定できます。</span><span class="sxs-lookup"><span data-stu-id="19388-124">If `AllowMultiple` for an attribute class is true, then that attribute class is a ***multi-use attribute class***, and can be specified more than once on an entity.</span></span> <span data-ttu-id="19388-125">場合`AllowMultiple`属性クラスは常に false かは指定しませんし、その属性クラスは、***単一使用属性クラス***エンティティに最大で 1 回指定できます。</span><span class="sxs-lookup"><span data-stu-id="19388-125">If `AllowMultiple` for an attribute class is false or it is unspecified, then that attribute class is a ***single-use attribute class***, and can be specified at most once on an entity.</span></span>

<span data-ttu-id="19388-126">例では、</span><span class="sxs-lookup"><span data-stu-id="19388-126">The example</span></span>

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

<span data-ttu-id="19388-127">という名前の属性を複数使用クラスを定義します。`AuthorAttribute`します。</span><span class="sxs-lookup"><span data-stu-id="19388-127">defines a multi-use attribute class named `AuthorAttribute`.</span></span> <span data-ttu-id="19388-128">例では、</span><span class="sxs-lookup"><span data-stu-id="19388-128">The example</span></span>

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

<span data-ttu-id="19388-129">2 つの用途をクラス宣言を示しています、`Author`属性。</span><span class="sxs-lookup"><span data-stu-id="19388-129">shows a class declaration with two uses of the `Author` attribute.</span></span>

<span data-ttu-id="19388-130">`AttributeUsage` 呼ばれる別の名前付きパラメーターを持つ`Inherited`、基底クラスで指定する場合は、属性がその基底クラスから派生したクラスによって継承もかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="19388-130">`AttributeUsage` has another named parameter called `Inherited`, which indicates whether the attribute, when specified on a base class, is also inherited by classes that derive from that base class.</span></span> <span data-ttu-id="19388-131">場合`Inherited`属性クラスが true の場合、その属性が継承されます。</span><span class="sxs-lookup"><span data-stu-id="19388-131">If `Inherited` for an attribute class is true, then that attribute is inherited.</span></span> <span data-ttu-id="19388-132">場合`Inherited`属性クラスは常に false し、その属性は継承されません。</span><span class="sxs-lookup"><span data-stu-id="19388-132">If `Inherited` for an attribute class is false then that attribute is not inherited.</span></span> <span data-ttu-id="19388-133">指定されていない場合、既定値は true です。</span><span class="sxs-lookup"><span data-stu-id="19388-133">If it is unspecified, its default value is true.</span></span>

<span data-ttu-id="19388-134">属性クラス`X`がない、`AttributeUsage`ように、属性がアタッチされています。</span><span class="sxs-lookup"><span data-stu-id="19388-134">An attribute class `X` not having an `AttributeUsage` attribute attached to it, as in</span></span>

```csharp
using System;

class X: Attribute {...}
```

<span data-ttu-id="19388-135">次のと同等です。</span><span class="sxs-lookup"><span data-stu-id="19388-135">is equivalent to the following:</span></span>

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a><span data-ttu-id="19388-136">位置指定引数と名前付きパラメーター</span><span class="sxs-lookup"><span data-stu-id="19388-136">Positional and named parameters</span></span>

<span data-ttu-id="19388-137">属性クラスが持つことができます***位置指定パラメーター***と***名前付きパラメーター***します。</span><span class="sxs-lookup"><span data-stu-id="19388-137">Attribute classes can have ***positional parameters*** and ***named parameters***.</span></span> <span data-ttu-id="19388-138">属性クラスの各パブリック インスタンス コンストラクターは、その属性クラスの位置指定パラメーターの有効なシーケンスを定義します。</span><span class="sxs-lookup"><span data-stu-id="19388-138">Each public instance constructor for an attribute class defines a valid sequence of positional parameters for that attribute class.</span></span> <span data-ttu-id="19388-139">各静的でないパブリックの読み取り/書き込みフィールドと、属性クラスのプロパティは、属性クラスの名前付きパラメーターを定義します。</span><span class="sxs-lookup"><span data-stu-id="19388-139">Each non-static public read-write field and property for an attribute class defines a named parameter for the attribute class.</span></span>

<span data-ttu-id="19388-140">例では、</span><span class="sxs-lookup"><span data-stu-id="19388-140">The example</span></span>

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

<span data-ttu-id="19388-141">という名前の属性クラスを定義します`HelpAttribute`1 つの位置指定パラメーターを持つ`url`と 1 つの名前付きパラメーター`Topic`します。</span><span class="sxs-lookup"><span data-stu-id="19388-141">defines an attribute class named `HelpAttribute` that has one positional parameter, `url`, and one named parameter, `Topic`.</span></span> <span data-ttu-id="19388-142">Non-static および public プロパティが`Url`読み取り/書き込みではないために、名前付きのパラメーターを定義しません。</span><span class="sxs-lookup"><span data-stu-id="19388-142">Although it is non-static and public, the property `Url` does not define a named parameter, since it is not read-write.</span></span>

<span data-ttu-id="19388-143">この属性クラスを次のように使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="19388-143">This attribute class might be used as follows:</span></span>

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

### <a name="attribute-parameter-types"></a><span data-ttu-id="19388-144">属性パラメーターの型</span><span class="sxs-lookup"><span data-stu-id="19388-144">Attribute parameter types</span></span>

<span data-ttu-id="19388-145">属性クラスの位置と名前付きパラメーターの型に制限されます、***属性パラメーター型***は。</span><span class="sxs-lookup"><span data-stu-id="19388-145">The types of positional and named parameters for an attribute class are limited to the ***attribute parameter types***, which are:</span></span>

*  <span data-ttu-id="19388-146">次の種類のいずれか: `bool`、 `byte`、 `char`、 `double`、 `float`、 `int`、 `long`、 `sbyte`、 `short`、 `string`、 `uint`、 `ulong`、`ushort`.</span><span class="sxs-lookup"><span data-stu-id="19388-146">One of the following types: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span></span>
*  <span data-ttu-id="19388-147">型 `object`。</span><span class="sxs-lookup"><span data-stu-id="19388-147">The type `object`.</span></span>
*  <span data-ttu-id="19388-148">型 `System.Type`。</span><span class="sxs-lookup"><span data-stu-id="19388-148">The type `System.Type`.</span></span>
*  <span data-ttu-id="19388-149">パブリック アクセシビリティし、では、入れ子になっている (該当する場合) の型もパブリック アクセシビリティを持つ列挙型が提供される ([属性の指定](attributes.md#attribute-specification))。</span><span class="sxs-lookup"><span data-stu-id="19388-149">An enum type, provided it has public accessibility and the types in which it is nested (if any) also have public accessibility ([Attribute specification](attributes.md#attribute-specification)).</span></span>
*  <span data-ttu-id="19388-150">上記の種類の 1 次元配列。</span><span class="sxs-lookup"><span data-stu-id="19388-150">Single-dimensional arrays of the above types.</span></span>
*  <span data-ttu-id="19388-151">コンストラクター引数またはこれらの型の 1 つがパブリック フィールドは、属性の指定で位置指定か名前付きパラメーターとして使用できません。</span><span class="sxs-lookup"><span data-stu-id="19388-151">A constructor argument or public field which does not have one of these types, cannot be used as a positional or named parameter in an attribute specification.</span></span>

## <a name="attribute-specification"></a><span data-ttu-id="19388-152">属性の指定</span><span class="sxs-lookup"><span data-stu-id="19388-152">Attribute specification</span></span>

<span data-ttu-id="19388-153">***属性の指定***はアプリケーションを以前に定義された属性の宣言。</span><span class="sxs-lookup"><span data-stu-id="19388-153">***Attribute specification*** is the application of a previously defined attribute to a declaration.</span></span> <span data-ttu-id="19388-154">属性は、宣言で指定されている追加の宣言情報です。</span><span class="sxs-lookup"><span data-stu-id="19388-154">An attribute is a piece of additional declarative information that is specified for a declaration.</span></span> <span data-ttu-id="19388-155">属性は、グローバル スコープ (を含んでいるアセンブリまたはモジュールに属性を指定) に指定することができます、 *type_declaration*s ([入力宣言](namespaces.md#type-declarations))、 *class_member_declaration*s ([パラメーターの制約入力](classes.md#type-parameter-constraints))、 *interface_member_declaration*s ([インターフェイスのメンバー](interfaces.md#interface-members))、 *struct_member_declaration*s ([構造体のメンバー](structs.md#struct-members))、 *enum_member_declaration*s ([列挙型メンバー](enums.md#enum-members))、 *accessor_declarations* ([アクセサー](classes.md#accessors))、 *event_accessor_declarations* ([フィールドのようなイベント](classes.md#field-like-events))、および*formal_parameter_list*s ([メソッド パラメーター](classes.md#method-parameters))。</span><span class="sxs-lookup"><span data-stu-id="19388-155">Attributes can be specified at global scope (to specify attributes on the containing assembly or module) and for *type_declaration*s ([Type declarations](namespaces.md#type-declarations)), *class_member_declaration*s ([Type parameter constraints](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([Interface members](interfaces.md#interface-members)), *struct_member_declaration*s ([Struct members](structs.md#struct-members)), *enum_member_declaration*s ([Enum members](enums.md#enum-members)), *accessor_declarations* ([Accessors](classes.md#accessors)), *event_accessor_declarations* ([Field-like events](classes.md#field-like-events)), and *formal_parameter_list*s ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="19388-156">属性で指定***属性セクション***します。</span><span class="sxs-lookup"><span data-stu-id="19388-156">Attributes are specified in ***attribute sections***.</span></span> <span data-ttu-id="19388-157">属性のセクションでは、1 つまたは複数の属性のコンマ区切りのリストを囲む角かっこのペアで構成されます。</span><span class="sxs-lookup"><span data-stu-id="19388-157">An attribute section consists of a pair of square brackets, which surround a comma-separated list of one or more attributes.</span></span> <span data-ttu-id="19388-158">属性は、このようなリストで指定し、同じプログラム エンティティにアタッチされているセクションでは、順序の配置の順序は大きくありません。</span><span class="sxs-lookup"><span data-stu-id="19388-158">The order in which attributes are specified in such a list, and the order in which sections attached to the same program entity are arranged, is not significant.</span></span> <span data-ttu-id="19388-159">属性の指定、 `[A][B]`、 `[B][A]`、 `[A,B]`、および`[B,A]`は同等です。</span><span class="sxs-lookup"><span data-stu-id="19388-159">For instance, the attribute specifications `[A][B]`, `[B][A]`, `[A,B]`, and `[B,A]` are equivalent.</span></span>

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

<span data-ttu-id="19388-160">属性から成る、 *attribute_name*と、省略可能な引数と名前付き引数のリスト。</span><span class="sxs-lookup"><span data-stu-id="19388-160">An attribute consists of an *attribute_name* and an optional list of positional and named arguments.</span></span> <span data-ttu-id="19388-161">名前付き引数の前に、位置指定引数 (ある場合)。</span><span class="sxs-lookup"><span data-stu-id="19388-161">The positional arguments (if any) precede the named arguments.</span></span> <span data-ttu-id="19388-162">位置指定引数から成る、 *attribute_argument_expression*; 名前付き引数を名の後に続く、等号 (=) から成る、 *attribute_argument_expression*であり、まとめて、単純な代入と同じ規則によって制限されます。</span><span class="sxs-lookup"><span data-stu-id="19388-162">A positional argument consists of an *attribute_argument_expression*; a named argument consists of a name, followed by an equal sign, followed by an *attribute_argument_expression*, which, together, are constrained by the same rules as simple assignment.</span></span> <span data-ttu-id="19388-163">名前付き引数の順序は大きくありません。</span><span class="sxs-lookup"><span data-stu-id="19388-163">The order of named arguments is not significant.</span></span>

<span data-ttu-id="19388-164">*Attribute_name*属性クラスを識別します。</span><span class="sxs-lookup"><span data-stu-id="19388-164">The *attribute_name* identifies an attribute class.</span></span> <span data-ttu-id="19388-165">場合のフォーム*attribute_name*は*type_name*この名前は、属性クラスに参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="19388-165">If the form of *attribute_name* is *type_name* then this name must refer to an attribute class.</span></span> <span data-ttu-id="19388-166">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="19388-166">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="19388-167">例では、</span><span class="sxs-lookup"><span data-stu-id="19388-167">The example</span></span>

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

<span data-ttu-id="19388-168">結果の使用を試むため、コンパイル時エラー`Class1`クラス属性として`Class1`属性クラスではありません。</span><span class="sxs-lookup"><span data-stu-id="19388-168">results in a compile-time error because it attempts to use `Class1` as an attribute class when `Class1` is not an attribute class.</span></span>

<span data-ttu-id="19388-169">特定のコンテキストでは、1 つ以上のターゲットの属性の指定を許可します。</span><span class="sxs-lookup"><span data-stu-id="19388-169">Certain contexts permit the specification of an attribute on more than one target.</span></span> <span data-ttu-id="19388-170">含めることによって、プログラムがターゲットを明示的に指定する*attribute_target_specifier*します。</span><span class="sxs-lookup"><span data-stu-id="19388-170">A program can explicitly specify the target by including an *attribute_target_specifier*.</span></span> <span data-ttu-id="19388-171">グローバル レベルでは、属性を配置すると、 *global_attribute_target_specifier*が必要です。</span><span class="sxs-lookup"><span data-stu-id="19388-171">When an attribute is placed at the global level, a *global_attribute_target_specifier* is required.</span></span> <span data-ttu-id="19388-172">その他のすべての場所で適切な既定値が適用されるが*attribute_target_specifier*を確認したり、または特定のあいまいな場合の既定値を上書きする (またはだけあいまいでない場合の既定値を確認する) に使用することができます。</span><span class="sxs-lookup"><span data-stu-id="19388-172">In all other locations, a reasonable default is applied, but an *attribute_target_specifier* can be used to affirm or override the default in certain ambiguous cases (or to just affirm the default in non-ambiguous cases).</span></span> <span data-ttu-id="19388-173">したがって、通常、 *attribute_target_specifier*グローバル レベルを除く s を省略できます。</span><span class="sxs-lookup"><span data-stu-id="19388-173">Thus, typically, *attribute_target_specifier*s can be omitted except at the global level.</span></span> <span data-ttu-id="19388-174">あいまいな可能性のあるコンテキストは、次のように解決されます。</span><span class="sxs-lookup"><span data-stu-id="19388-174">The potentially ambiguous contexts are resolved as follows:</span></span>

*  <span data-ttu-id="19388-175">グローバル スコープで指定された属性は、ターゲット アセンブリまたは対象のモジュールに適用できます。</span><span class="sxs-lookup"><span data-stu-id="19388-175">An attribute specified at global scope can apply either to the target assembly or the target module.</span></span> <span data-ttu-id="19388-176">既定値が存在しない、このコンテキストのため、 *attribute_target_specifier*が常に、このコンテキストで必要です。</span><span class="sxs-lookup"><span data-stu-id="19388-176">No default exists for this context, so an *attribute_target_specifier* is always required in this context.</span></span> <span data-ttu-id="19388-177">有無、 `assembly` *attribute_target_specifier*属性がターゲットに適用されることを示しますアセンブリ以外の存在、 `module` *attribute_target_specifier* 。ターゲット モジュールに属性が適用されることを示します。</span><span class="sxs-lookup"><span data-stu-id="19388-177">The presence of the `assembly` *attribute_target_specifier* indicates that the attribute applies to the target assembly; the presence of the `module` *attribute_target_specifier* indicates that the attribute applies to the target module.</span></span>
*  <span data-ttu-id="19388-178">デリゲート宣言で指定された属性は、宣言されているデリゲートまたはその戻り値のいずれかに適用できます。</span><span class="sxs-lookup"><span data-stu-id="19388-178">An attribute specified on a delegate declaration can apply either to the delegate being declared or to its return value.</span></span> <span data-ttu-id="19388-179">ない場合、 *attribute_target_specifier*デリゲートに属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-179">In the absence of an *attribute_target_specifier*, the attribute applies to the delegate.</span></span> <span data-ttu-id="19388-180">有無、 `type` *attribute_target_specifier* ; デリゲートに属性が適用されることを示しますの存在、 `return` *attribute_target_specifier*戻り値に属性が適用されることを示します。</span><span class="sxs-lookup"><span data-stu-id="19388-180">The presence of the `type` *attribute_target_specifier* indicates that the attribute applies to the delegate; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="19388-181">メソッド宣言で指定された属性は、宣言されているメソッドまたはその戻り値のいずれかに適用できます。</span><span class="sxs-lookup"><span data-stu-id="19388-181">An attribute specified on a method declaration can apply either to the method being declared or to its return value.</span></span> <span data-ttu-id="19388-182">ない場合、 *attribute_target_specifier*メソッドに属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-182">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="19388-183">有無、 `method` *attribute_target_specifier*メソッドに属性が適用されることを示しますの存在、 `return` *attribute_target_specifier*を示します戻り値に属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-183">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="19388-184">演算子の宣言で指定された属性は、宣言されている演算子またはその戻り値のいずれかに適用できます。</span><span class="sxs-lookup"><span data-stu-id="19388-184">An attribute specified on an operator declaration can apply either to the operator being declared or to its return value.</span></span> <span data-ttu-id="19388-185">ない場合、 *attribute_target_specifier*、演算子、属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-185">In the absence of an *attribute_target_specifier*, the attribute applies to the operator.</span></span> <span data-ttu-id="19388-186">有無、 `method` *attribute_target_specifier*には、演算子、属性が適用されることを示しますの存在、 `return` *attribute_target_specifier*戻り値に属性が適用されることを示します。</span><span class="sxs-lookup"><span data-stu-id="19388-186">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the operator; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="19388-187">イベント アクセサーを省略するイベントの宣言で指定された属性では、宣言されるイベント、関連するフィールド (イベントが抽象でない場合)、または関連付けられている追加の適用でき、メソッドを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="19388-187">An attribute specified on an event declaration that omits event accessors can apply to the event being declared, to the associated field (if the event is not abstract), or to the associated add and remove methods.</span></span> <span data-ttu-id="19388-188">ない場合、 *attribute_target_specifier*イベントに属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-188">In the absence of an *attribute_target_specifier*, the attribute applies to the event.</span></span> <span data-ttu-id="19388-189">有無、 `event` *attribute_target_specifier*イベントに属性が適用されることを示しますの存在、 `field` *attribute_target_specifier*を示しますフィールドに属性が適用されます。かどうか、 `method` *attribute_target_specifier*メソッドに属性が適用されることを示します。</span><span class="sxs-lookup"><span data-stu-id="19388-189">The presence of the `event` *attribute_target_specifier* indicates that the attribute applies to the event; the presence of the `field` *attribute_target_specifier* indicates that the attribute applies to the field; and the presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the methods.</span></span>
*  <span data-ttu-id="19388-190">プロパティまたはインデクサーの宣言の get アクセサーの宣言で指定された属性は、関連付けられているメソッドまたはその戻り値のいずれかに適用できます。</span><span class="sxs-lookup"><span data-stu-id="19388-190">An attribute specified on a get accessor declaration for a property or indexer declaration can apply either to the associated method or to its return value.</span></span> <span data-ttu-id="19388-191">ない場合、 *attribute_target_specifier*メソッドに属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-191">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="19388-192">有無、 `method` *attribute_target_specifier*メソッドに属性が適用されることを示しますの存在、 `return` *attribute_target_specifier*を示します戻り値に属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-192">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="19388-193">プロパティまたはインデクサーの宣言の set アクセサーに指定された属性は、関連付けられているメソッドまたはその唯一のパラメーターのいずれかに適用できます。</span><span class="sxs-lookup"><span data-stu-id="19388-193">An attribute specified on a set accessor for a property or indexer declaration can apply either to the associated method or to its lone implicit parameter.</span></span> <span data-ttu-id="19388-194">ない場合、 *attribute_target_specifier*メソッドに属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-194">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="19388-195">有無、 `method` *attribute_target_specifier*メソッドに属性が適用されることを示しますの存在、 `param` *attribute_target_specifier*を示しますパラメーターの場合に、属性が適用されます。有無、 `return` *attribute_target_specifier*属性は、戻り値に適用されることを示します。</span><span class="sxs-lookup"><span data-stu-id="19388-195">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="19388-196">イベントの宣言は、関連付けられているメソッドまたはその唯一のパラメーターのいずれかを適用できます add または remove アクセサーの宣言で指定された属性です。</span><span class="sxs-lookup"><span data-stu-id="19388-196">An attribute specified on an add or remove accessor declaration for an event declaration can apply either to the associated method or to its lone parameter.</span></span> <span data-ttu-id="19388-197">ない場合、 *attribute_target_specifier*メソッドに属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-197">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="19388-198">有無、 `method` *attribute_target_specifier*メソッドに属性が適用されることを示しますの存在、 `param` *attribute_target_specifier*を示しますパラメーターの場合に、属性が適用されます。有無、 `return` *attribute_target_specifier*属性は、戻り値に適用されることを示します。</span><span class="sxs-lookup"><span data-stu-id="19388-198">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>

<span data-ttu-id="19388-199">他のコンテキストを含めることで、 *attribute_target_specifier*は許可されているが不要な。</span><span class="sxs-lookup"><span data-stu-id="19388-199">In other contexts, inclusion of an *attribute_target_specifier* is permitted but unnecessary.</span></span> <span data-ttu-id="19388-200">たとえば、クラス宣言を含めるか、指定子を省略`type`:</span><span class="sxs-lookup"><span data-stu-id="19388-200">For instance, a class declaration may either include or omit the specifier `type`:</span></span>

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

<span data-ttu-id="19388-201">無効なを指定するとエラーには*attribute_target_specifier*します。</span><span class="sxs-lookup"><span data-stu-id="19388-201">It is an error to specify an invalid *attribute_target_specifier*.</span></span> <span data-ttu-id="19388-202">指定子、`param`クラス宣言では使用できません。</span><span class="sxs-lookup"><span data-stu-id="19388-202">For instance, the specifier `param` cannot be used on a class declaration:</span></span>

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

<span data-ttu-id="19388-203">サフィックスを持つ属性クラスの名前は慣例により、`Attribute`します。</span><span class="sxs-lookup"><span data-stu-id="19388-203">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="19388-204">*Attribute_name*フォームの*type_name*を含めるかこのサフィックスを省略することがあります。</span><span class="sxs-lookup"><span data-stu-id="19388-204">An *attribute_name* of the form *type_name* may either include or omit this suffix.</span></span> <span data-ttu-id="19388-205">このサフィックスの有無にかかわらず、属性クラスが見つかった場合、あいまいさが存在して、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="19388-205">If an attribute class is found both with and without this suffix, an ambiguity is present, and a compile-time error results.</span></span> <span data-ttu-id="19388-206">場合、 *attribute_name*が入力されているように、右端の*識別子*逐語的識別子 ([識別子](lexical-structure.md#identifiers))、サフィックスが付いていない属性だけが一致すると、その後あいまいさを解決できるようにします。</span><span class="sxs-lookup"><span data-stu-id="19388-206">If the *attribute_name* is spelled such that its right-most *identifier* is a verbatim identifier ([Identifiers](lexical-structure.md#identifiers)), then only an attribute without a suffix is matched, thus enabling such an ambiguity to be resolved.</span></span> <span data-ttu-id="19388-207">例では、</span><span class="sxs-lookup"><span data-stu-id="19388-207">The example</span></span>

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

<span data-ttu-id="19388-208">2 つの属性という名前のクラスを示しています`X`と`XAttribute`します。</span><span class="sxs-lookup"><span data-stu-id="19388-208">shows two attribute classes named `X` and `XAttribute`.</span></span> <span data-ttu-id="19388-209">属性`[X]`いずれかを参照しているため、あいまいになりますが`X`または`XAttribute`します。</span><span class="sxs-lookup"><span data-stu-id="19388-209">The attribute `[X]` is ambiguous, since it could refer to either `X` or `XAttribute`.</span></span> <span data-ttu-id="19388-210">逐語的識別子を使用すると、このようなまれなケースで指定する正確なインテントができます。</span><span class="sxs-lookup"><span data-stu-id="19388-210">Using a verbatim identifier allows the exact intent to be specified in such rare cases.</span></span> <span data-ttu-id="19388-211">属性`[XAttribute]`あいまいでない (がという名前の属性クラスがあったかどうか`XAttributeAttribute`!)。</span><span class="sxs-lookup"><span data-stu-id="19388-211">The attribute `[XAttribute]` is not ambiguous (although it would be if there was an attribute class named `XAttributeAttribute`!).</span></span> <span data-ttu-id="19388-212">場合クラスの宣言`X`が削除され、両方の属性がという名前の属性クラスを参照して`XAttribute`、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="19388-212">If the declaration for class `X` is removed, then both attributes refer to the attribute class named `XAttribute`, as follows:</span></span>

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

<span data-ttu-id="19388-213">同じエンティティについて単一使用属性クラスを複数回使用すると、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="19388-213">It is a compile-time error to use a single-use attribute class more than once on the same entity.</span></span> <span data-ttu-id="19388-214">例では、</span><span class="sxs-lookup"><span data-stu-id="19388-214">The example</span></span>

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

<span data-ttu-id="19388-215">結果の使用を試むため、コンパイル時エラー `HelpString`、単一使用属性クラスである、2 回以上の宣言で`Class1`します。</span><span class="sxs-lookup"><span data-stu-id="19388-215">results in a compile-time error because it attempts to use `HelpString`, which is a single-use attribute class, more than once on the declaration of `Class1`.</span></span>

<span data-ttu-id="19388-216">式`E`は、 *attribute_argument_expression*のすべての次のステートメントに該当する場合。</span><span class="sxs-lookup"><span data-stu-id="19388-216">An expression `E` is an *attribute_argument_expression* if all of the following statements are true:</span></span>

*  <span data-ttu-id="19388-217">型`E`属性パラメーター型は、([属性パラメーター型](attributes.md#attribute-parameter-types))。</span><span class="sxs-lookup"><span data-stu-id="19388-217">The type of `E` is an attribute parameter type ([Attribute parameter types](attributes.md#attribute-parameter-types)).</span></span>
*  <span data-ttu-id="19388-218">コンパイル時の値に`E`次のいずれかに解決できます。</span><span class="sxs-lookup"><span data-stu-id="19388-218">At compile-time, the value of `E` can be resolved to one of the following:</span></span>
   * <span data-ttu-id="19388-219">定数値。</span><span class="sxs-lookup"><span data-stu-id="19388-219">A constant value.</span></span>
   * <span data-ttu-id="19388-220">`System.Type` オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="19388-220">A `System.Type` object.</span></span>
   * <span data-ttu-id="19388-221">1 次元配列*attribute_argument_expression*秒。</span><span class="sxs-lookup"><span data-stu-id="19388-221">A one-dimensional array of *attribute_argument_expression*s.</span></span>

<span data-ttu-id="19388-222">例:</span><span class="sxs-lookup"><span data-stu-id="19388-222">For example:</span></span>

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

<span data-ttu-id="19388-223">A *typeof_expression* ([typeof 演算子](expressions.md#the-typeof-operator)) 属性引数の式は、非ジェネリック型、構築されたクローズ型、または、バインドされていないジェネリック型を参照できますが、これは参照できません、使用します。型を開きます。</span><span class="sxs-lookup"><span data-stu-id="19388-223">A *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)) used as an attribute argument expression can reference a non-generic type, a closed constructed type, or an unbound generic type, but it cannot reference an open type.</span></span> <span data-ttu-id="19388-224">これは、コンパイル時に式を解決できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="19388-224">This is to ensure that the expression can be resolved at compile-time.</span></span>

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

## <a name="attribute-instances"></a><span data-ttu-id="19388-225">属性インスタンス</span><span class="sxs-lookup"><span data-stu-id="19388-225">Attribute instances</span></span>

<span data-ttu-id="19388-226">***属性インスタンス***は実行時に属性を表すインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="19388-226">An ***attribute instance*** is an instance that represents an attribute at run-time.</span></span> <span data-ttu-id="19388-227">属性は、位置指定引数は、属性クラスで定義されているし、名前付き引数。</span><span class="sxs-lookup"><span data-stu-id="19388-227">An attribute is defined with an attribute class, positional arguments, and named arguments.</span></span> <span data-ttu-id="19388-228">属性インスタンスは、位置指定引数と名前付き引数を使用して初期化される属性クラスのインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="19388-228">An attribute instance is an instance of the attribute class that is initialized with the positional and named arguments.</span></span>

<span data-ttu-id="19388-229">属性インスタンスの取得には、次のセクションで説明した、コンパイル時と実行時の両方の処理が含まれます。</span><span class="sxs-lookup"><span data-stu-id="19388-229">Retrieval of an attribute instance involves both compile-time and run-time processing, as described in the following sections.</span></span>

### <a name="compilation-of-an-attribute"></a><span data-ttu-id="19388-230">属性のコンパイル</span><span class="sxs-lookup"><span data-stu-id="19388-230">Compilation of an attribute</span></span>

<span data-ttu-id="19388-231">コンパイル、*属性*属性クラスを使用して`T`、 *positional_argument_list* `P`と*named_argument_list* `N`、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="19388-231">The compilation of an *attribute* with attribute class `T`, *positional_argument_list* `P` and *named_argument_list* `N`, consists of the following steps:</span></span>

*  <span data-ttu-id="19388-232">コンパイル時の処理手順をコンパイルするため、 *object_creation_expression*フォームの`new T(P)`します。</span><span class="sxs-lookup"><span data-stu-id="19388-232">Follow the compile-time processing steps for compiling an *object_creation_expression* of the form `new T(P)`.</span></span> <span data-ttu-id="19388-233">次の手順は、コンパイル時エラーが発生するか、インスタンス コンストラクターを判断`C`で`T`は、実行時に呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="19388-233">These steps either result in a compile-time error, or determine an instance constructor `C` on `T` that can be invoked at run-time.</span></span>
*  <span data-ttu-id="19388-234">場合`C`パブリック アクセシビリティを持たない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="19388-234">If `C` does not have public accessibility, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="19388-235">各*named_argument* `Arg`で`N`:</span><span class="sxs-lookup"><span data-stu-id="19388-235">For each *named_argument* `Arg` in `N`:</span></span>
   * <span data-ttu-id="19388-236">ように`Name`する、*識別子*の*named_argument* `Arg`します。</span><span class="sxs-lookup"><span data-stu-id="19388-236">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span>
   * <span data-ttu-id="19388-237">`Name` 非静的な読み取り/書き込みのパブリック フィールドまたはプロパティを識別する必要があります`T`します。</span><span class="sxs-lookup"><span data-stu-id="19388-237">`Name` must identify a non-static read-write public field or property on `T`.</span></span> <span data-ttu-id="19388-238">場合`T`コンパイル時エラーが発生し、そのようなフィールドまたはプロパティを持ちます。</span><span class="sxs-lookup"><span data-stu-id="19388-238">If `T` has no such field or property, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="19388-239">属性の実行時インスタンスを作成して、次の情報を保持します属性クラス`T`、インスタンス コンストラクター`C`で`T`、 *positional_argument_list* `P` 。および*named_argument_list* `N`します。</span><span class="sxs-lookup"><span data-stu-id="19388-239">Keep the following information for run-time instantiation of the attribute: the attribute class `T`, the instance constructor `C` on `T`, the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

### <a name="run-time-retrieval-of-an-attribute-instance"></a><span data-ttu-id="19388-240">属性インスタンスの実行時の取得</span><span class="sxs-lookup"><span data-stu-id="19388-240">Run-time retrieval of an attribute instance</span></span>

<span data-ttu-id="19388-241">コンパイル、*属性*属性クラスを生成`T`、インスタンス コンストラクター`C`で`T`、 *positional_argument_list* `P`、および*named_argument_list* `N`します。</span><span class="sxs-lookup"><span data-stu-id="19388-241">Compilation of an *attribute* yields an attribute class `T`, an instance constructor `C` on `T`, a *positional_argument_list* `P`, and a *named_argument_list* `N`.</span></span> <span data-ttu-id="19388-242">この情報を指定するには、属性インスタンスが、次の手順を使用して実行時に取得できます。</span><span class="sxs-lookup"><span data-stu-id="19388-242">Given this information, an attribute instance can be retrieved at run-time using the following steps:</span></span>

*  <span data-ttu-id="19388-243">実行時の処理手順を実行するため、 *object_creation_expression*フォームの`new T(P)`、インスタンス コンストラクターを使用して`C`コンパイル時に決定されます。</span><span class="sxs-lookup"><span data-stu-id="19388-243">Follow the run-time processing steps for executing an *object_creation_expression* of the form `new T(P)`, using the instance constructor `C` as determined at compile-time.</span></span> <span data-ttu-id="19388-244">次の手順は、例外、またはインスタンスを作成します`O`の`T`します。</span><span class="sxs-lookup"><span data-stu-id="19388-244">These steps either result in an exception, or produce an instance `O` of `T`.</span></span>
*  <span data-ttu-id="19388-245">各*named_argument* `Arg`で`N`の順序で。</span><span class="sxs-lookup"><span data-stu-id="19388-245">For each *named_argument* `Arg` in `N`, in order:</span></span>
   * <span data-ttu-id="19388-246">ように`Name`する、*識別子*の*named_argument* `Arg`します。</span><span class="sxs-lookup"><span data-stu-id="19388-246">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span> <span data-ttu-id="19388-247">場合`Name`で静的でないパブリックの読み取り/書き込みフィールドまたはプロパティを識別しないいない`O`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="19388-247">If `Name` does not identify a non-static public read-write field or property on `O`, then an exception is thrown.</span></span>
   * <span data-ttu-id="19388-248">ように`Value`評価の結果である、 *attribute_argument_expression*の`Arg`します。</span><span class="sxs-lookup"><span data-stu-id="19388-248">Let `Value` be the result of evaluating the *attribute_argument_expression* of `Arg`.</span></span>
   * <span data-ttu-id="19388-249">場合`Name`でフィールドを識別する`O`にこのフィールドを設定`Value`します。</span><span class="sxs-lookup"><span data-stu-id="19388-249">If `Name` identifies a field on `O`, then set this field to `Value`.</span></span>
   * <span data-ttu-id="19388-250">それ以外の場合、`Name`でプロパティを識別します`O`します。</span><span class="sxs-lookup"><span data-stu-id="19388-250">Otherwise, `Name` identifies a property on `O`.</span></span> <span data-ttu-id="19388-251">このプロパティを設定`Value`します。</span><span class="sxs-lookup"><span data-stu-id="19388-251">Set this property to `Value`.</span></span>
   * <span data-ttu-id="19388-252">結果は`O`、属性クラスのインスタンス`T`で初期化されました、 *positional_argument_list* `P`と*named_argument_list*`N`.</span><span class="sxs-lookup"><span data-stu-id="19388-252">The result is `O`, an instance of the attribute class `T` that has been initialized with the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

## <a name="reserved-attributes"></a><span data-ttu-id="19388-253">予約済みの属性</span><span class="sxs-lookup"><span data-stu-id="19388-253">Reserved attributes</span></span>

<span data-ttu-id="19388-254">属性の数が少ないでは、何らかの方法で、言語に影響します。</span><span class="sxs-lookup"><span data-stu-id="19388-254">A small number of attributes affect the language in some way.</span></span> <span data-ttu-id="19388-255">これらの属性は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="19388-255">These attributes include:</span></span>

*  <span data-ttu-id="19388-256">`System.AttributeUsageAttribute` ([、AttributeUsage 属性](attributes.md#the-attributeusage-attribute)) を使用して、属性クラスを使用できる方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="19388-256">`System.AttributeUsageAttribute` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)), which is used to describe the ways in which an attribute class can be used.</span></span>
*  <span data-ttu-id="19388-257">`System.Diagnostics.ConditionalAttribute` ([、条件付き属性](attributes.md#the-conditional-attribute))、これは条件付きメソッドの定義に使用します。</span><span class="sxs-lookup"><span data-stu-id="19388-257">`System.Diagnostics.ConditionalAttribute` ([The Conditional attribute](attributes.md#the-conditional-attribute)), which is used to define conditional methods.</span></span>
*  <span data-ttu-id="19388-258">`System.ObsoleteAttribute` ([The Obsolete 属性](attributes.md#the-obsolete-attribute))、不使用とメンバーを示すために使用します。</span><span class="sxs-lookup"><span data-stu-id="19388-258">`System.ObsoleteAttribute` ([The Obsolete attribute](attributes.md#the-obsolete-attribute)), which is used to mark a member as obsolete.</span></span>
*  <span data-ttu-id="19388-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`、`System.Runtime.CompilerServices.CallerFilePathAttribute`と`System.Runtime.CompilerServices.CallerMemberNameAttribute`([呼び出し元情報属性](attributes.md#caller-info-attributes))、省略可能なパラメーターを呼び出し元のコンテキストに関する情報を入力に使用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` and `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller info attributes](attributes.md#caller-info-attributes)), which are used to supply information about the calling context to optional parameters.</span></span>

### <a name="the-attributeusage-attribute"></a><span data-ttu-id="19388-260">AttributeUsage 属性</span><span class="sxs-lookup"><span data-stu-id="19388-260">The AttributeUsage attribute</span></span>

<span data-ttu-id="19388-261">属性`AttributeUsage`属性クラスを使用できる方法を説明するために使用します。</span><span class="sxs-lookup"><span data-stu-id="19388-261">The attribute `AttributeUsage` is used to describe the manner in which the attribute class can be used.</span></span>

<span data-ttu-id="19388-262">装飾したクラス、`AttributeUsage`属性から派生しなければなりません`System.Attribute`、直接または間接的にします。</span><span class="sxs-lookup"><span data-stu-id="19388-262">A class that is decorated with the `AttributeUsage` attribute must derive from `System.Attribute`, either directly or indirectly.</span></span> <span data-ttu-id="19388-263">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="19388-263">Otherwise, a compile-time error occurs.</span></span>

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

### <a name="the-conditional-attribute"></a><span data-ttu-id="19388-264">条件付き属性</span><span class="sxs-lookup"><span data-stu-id="19388-264">The Conditional attribute</span></span>

<span data-ttu-id="19388-265">属性`Conditional`の定義を有効に***条件付きメソッド***と***条件付き属性クラス***します。</span><span class="sxs-lookup"><span data-stu-id="19388-265">The attribute `Conditional` enables the definition of ***conditional methods*** and ***conditional attribute classes***.</span></span>

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

#### <a name="conditional-methods"></a><span data-ttu-id="19388-266">条件付きメソッド</span><span class="sxs-lookup"><span data-stu-id="19388-266">Conditional methods</span></span>

<span data-ttu-id="19388-267">修飾されたメソッド、`Conditional`属性は、条件付きメソッド。</span><span class="sxs-lookup"><span data-stu-id="19388-267">A method decorated with the `Conditional` attribute is a conditional method.</span></span> <span data-ttu-id="19388-268">`Conditional`属性は、条件付きコンパイル シンボルをテストして条件を示します。</span><span class="sxs-lookup"><span data-stu-id="19388-268">The `Conditional` attribute indicates a condition by testing a conditional compilation symbol.</span></span> <span data-ttu-id="19388-269">条件付きメソッドの呼び出しをに含まれるか、または省略すると、呼び出しの時点でこのシンボルが定義されているかどうかによって異なります。</span><span class="sxs-lookup"><span data-stu-id="19388-269">Calls to a conditional method are either included or omitted depending on whether this symbol is defined at the point of the call.</span></span> <span data-ttu-id="19388-270">呼び出しが含まれています。 シンボルが定義されている場合それ以外の場合、(受信者の評価と呼び出しのパラメーターを含む) の呼び出しは省略されます。</span><span class="sxs-lookup"><span data-stu-id="19388-270">If the symbol is defined, the call is included; otherwise, the call (including evaluation of the receiver and parameters of the call) is omitted.</span></span>

<span data-ttu-id="19388-271">条件付きメソッドは、次の制限を受けます。</span><span class="sxs-lookup"><span data-stu-id="19388-271">A conditional method is subject to the following restrictions:</span></span>

*  <span data-ttu-id="19388-272">条件付きメソッドのメソッドである必要があります、 *class_declaration*または*struct_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="19388-272">The conditional method must be a method in a *class_declaration* or *struct_declaration*.</span></span> <span data-ttu-id="19388-273">コンパイル時エラーが発生します、`Conditional`インターフェイス宣言のメソッドに属性を指定します。</span><span class="sxs-lookup"><span data-stu-id="19388-273">A compile-time error occurs if the `Conditional` attribute is specified on a method in an interface declaration.</span></span>
*  <span data-ttu-id="19388-274">条件付きメソッドの戻り値の型があります`void`します。</span><span class="sxs-lookup"><span data-stu-id="19388-274">The conditional method must have a return type of `void`.</span></span>
*  <span data-ttu-id="19388-275">条件付きメソッドがでマークされていない必要があります、`override`修飾子。</span><span class="sxs-lookup"><span data-stu-id="19388-275">The conditional method must not be marked with the `override` modifier.</span></span> <span data-ttu-id="19388-276">条件付きメソッドをマークすることがあります、`virtual`修飾子、ただしします。</span><span class="sxs-lookup"><span data-stu-id="19388-276">A conditional method may be marked with the `virtual` modifier, however.</span></span> <span data-ttu-id="19388-277">このようなメソッドのオーバーライドは暗黙的に条件付きとでない、明示的にマークする必要があります、`Conditional`属性。</span><span class="sxs-lookup"><span data-stu-id="19388-277">Overrides of such a method are implicitly conditional, and must not be explicitly marked with a `Conditional` attribute.</span></span>
*  <span data-ttu-id="19388-278">条件付きメソッドは、インターフェイス メソッドの実装があります。</span><span class="sxs-lookup"><span data-stu-id="19388-278">The conditional method must not be an implementation of an interface method.</span></span> <span data-ttu-id="19388-279">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="19388-279">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="19388-280">さらに、コンパイル時エラーが発生する条件付きメソッドで使用する場合、 *delegate_creation_expression*します。</span><span class="sxs-lookup"><span data-stu-id="19388-280">In addition, a compile-time error occurs if a conditional method is used in a *delegate_creation_expression*.</span></span> <span data-ttu-id="19388-281">例では、</span><span class="sxs-lookup"><span data-stu-id="19388-281">The example</span></span>

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

<span data-ttu-id="19388-282">宣言`Class1.M`条件付きメソッドとして。</span><span class="sxs-lookup"><span data-stu-id="19388-282">declares `Class1.M` as a conditional method.</span></span> <span data-ttu-id="19388-283">`Class2``Test`メソッドは、このメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="19388-283">`Class2`'s `Test` method calls this method.</span></span> <span data-ttu-id="19388-284">条件付きコンパイル シンボル以降`DEBUG`が定義されている場合は場合、`Class2.Test`が呼び出されると、それが呼び出す`M`します。</span><span class="sxs-lookup"><span data-stu-id="19388-284">Since the conditional compilation symbol `DEBUG` is defined, if `Class2.Test` is called, it will call `M`.</span></span> <span data-ttu-id="19388-285">場合、シンボル`DEBUG`が定義されていない、し`Class2.Test`呼び出さない`Class1.M`します。</span><span class="sxs-lookup"><span data-stu-id="19388-285">If the symbol `DEBUG` had not been defined, then `Class2.Test` would not call `Class1.M`.</span></span>

<span data-ttu-id="19388-286">呼び出しの時点の条件付きコンパイル シンボルを含めるか、条件付きメソッドの呼び出しの除外を制御することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="19388-286">It is important to note that the inclusion or exclusion of a call to a conditional method is controlled by the conditional compilation symbols at the point of the call.</span></span> <span data-ttu-id="19388-287">例</span><span class="sxs-lookup"><span data-stu-id="19388-287">In the example</span></span>

<span data-ttu-id="19388-288">ファイル`class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="19388-288">File `class1.cs`:</span></span>

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

<span data-ttu-id="19388-289">ファイル`class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="19388-289">File `class2.cs`:</span></span>

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

<span data-ttu-id="19388-290">ファイル`class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="19388-290">File `class3.cs`:</span></span>

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

<span data-ttu-id="19388-291">クラスは、`Class2`と`Class3`条件付きメソッドの呼び出しを含む各`Class1.F`、かどうかに基づく条件付きである`DEBUG`が定義されています。</span><span class="sxs-lookup"><span data-stu-id="19388-291">the classes `Class2` and `Class3` each contain calls to the conditional method `Class1.F`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="19388-292">コンテキストでこのシンボルが定義されているため`Class2`なく`Class3`への呼び出し`F`で`Class2`への呼び出し中に、含まれている`F`で`Class3`を省略するとします。</span><span class="sxs-lookup"><span data-stu-id="19388-292">Since this symbol is defined in the context of `Class2` but not `Class3`, the call to `F` in `Class2` is included, while the call to `F` in `Class3` is omitted.</span></span>

<span data-ttu-id="19388-293">継承チェーンで条件付きメソッドを使用してわかりにくいことができます。</span><span class="sxs-lookup"><span data-stu-id="19388-293">The use of conditional methods in an inheritance chain can be confusing.</span></span> <span data-ttu-id="19388-294">条件付きメソッドを呼び出し`base`、フォームの`base.M`、通常の条件付きメソッドの呼び出し規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-294">Calls made to a conditional method through `base`, of the form `base.M`, are subject to the normal conditional method call rules.</span></span> <span data-ttu-id="19388-295">例</span><span class="sxs-lookup"><span data-stu-id="19388-295">In the example</span></span>

<span data-ttu-id="19388-296">ファイル`class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="19388-296">File `class1.cs`:</span></span>

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

<span data-ttu-id="19388-297">ファイル`class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="19388-297">File `class2.cs`:</span></span>

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

<span data-ttu-id="19388-298">ファイル`class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="19388-298">File `class3.cs`:</span></span>

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

<span data-ttu-id="19388-299">`Class2` 呼び出しを含み、`M`基底クラスで定義されています。</span><span class="sxs-lookup"><span data-stu-id="19388-299">`Class2` includes a call to the `M` defined in its base class.</span></span> <span data-ttu-id="19388-300">この呼び出しを省略したは、基本のメソッドでは、シンボルの有無に基づく条件付き理由`DEBUG`は定義されていません。</span><span class="sxs-lookup"><span data-stu-id="19388-300">This call is omitted because the base method is conditional based on the presence of the symbol `DEBUG`, which is undefined.</span></span> <span data-ttu-id="19388-301">したがって、メソッドは、コンソールに書き込みます"`Class2.M executed`"のみです。</span><span class="sxs-lookup"><span data-stu-id="19388-301">Thus, the method writes to the console "`Class2.M executed`" only.</span></span> <span data-ttu-id="19388-302">賢く利用*pp_declaration*s は、このような問題を排除できます。</span><span class="sxs-lookup"><span data-stu-id="19388-302">Judicious use of *pp_declaration*s can eliminate such problems.</span></span>

#### <a name="conditional-attribute-classes"></a><span data-ttu-id="19388-303">条件付き属性クラス</span><span class="sxs-lookup"><span data-stu-id="19388-303">Conditional attribute classes</span></span>

<span data-ttu-id="19388-304">属性クラス ([属性クラス](attributes.md#attribute-classes)) 1 つまたは複数で修飾された`Conditional`属性は、***条件付き属性クラス***します。</span><span class="sxs-lookup"><span data-stu-id="19388-304">An attribute class ([Attribute classes](attributes.md#attribute-classes)) decorated with one or more `Conditional` attributes is a ***conditional attribute class***.</span></span> <span data-ttu-id="19388-305">宣言されている条件付きコンパイル シンボルを使用した条件付き属性クラスが関連付けられているなりますその`Conditional`属性。</span><span class="sxs-lookup"><span data-stu-id="19388-305">A conditional attribute class is thus associated with the conditional compilation symbols declared in its `Conditional` attributes.</span></span> <span data-ttu-id="19388-306">この例では:</span><span class="sxs-lookup"><span data-stu-id="19388-306">This example:</span></span>

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

<span data-ttu-id="19388-307">宣言`TestAttribute`条件付きコンパイル シンボルに関連付けられた条件属性をクラスとして`ALPHA`と`BETA`します。</span><span class="sxs-lookup"><span data-stu-id="19388-307">declares `TestAttribute` as a conditional attribute class associated with the conditional compilations symbols `ALPHA` and `BETA`.</span></span>

<span data-ttu-id="19388-308">属性の仕様 ([属性の指定](attributes.md#attribute-specification)) の条件付き属性が含まれる 1 つ以上の関連付けられている条件付きコンパイル シンボルが定義されている場合、仕様の時点でそれ以外の場合、属性指定は省略されます。</span><span class="sxs-lookup"><span data-stu-id="19388-308">Attribute specifications ([Attribute specification](attributes.md#attribute-specification)) of a conditional attribute are included if one or more of its associated conditional compilation symbols is defined at the point of specification, otherwise the attribute specification is omitted.</span></span>

<span data-ttu-id="19388-309">仕様の時点で条件付きコンパイル シンボルでの追加または条件付き属性クラスの属性の指定の除外を制御することに注意してください。 重要です。</span><span class="sxs-lookup"><span data-stu-id="19388-309">It is important to note that the inclusion or exclusion of an attribute specification of a conditional attribute class is controlled by the conditional compilation symbols at the point of the specification.</span></span> <span data-ttu-id="19388-310">例</span><span class="sxs-lookup"><span data-stu-id="19388-310">In the example</span></span>

<span data-ttu-id="19388-311">ファイル`test.cs`:</span><span class="sxs-lookup"><span data-stu-id="19388-311">File `test.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

<span data-ttu-id="19388-312">ファイル`class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="19388-312">File `class1.cs`:</span></span>

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

<span data-ttu-id="19388-313">ファイル`class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="19388-313">File `class2.cs`:</span></span>

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

<span data-ttu-id="19388-314">クラスは、`Class1`と`Class2`は属性で修飾された各`Test`、かどうかに基づく条件付きである`DEBUG`が定義されています。</span><span class="sxs-lookup"><span data-stu-id="19388-314">the classes `Class1` and `Class2` are each decorated with attribute `Test`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="19388-315">コンテキストでこのシンボルが定義されているため`Class1`なく`Class2`の仕様、`Test`属性`Class1`の仕様の中に、含まれている、`Test`属性を`Class2`を省略するとします。</span><span class="sxs-lookup"><span data-stu-id="19388-315">Since this symbol is defined in the context of `Class1` but not `Class2`, the specification of the `Test` attribute on `Class1` is included, while the specification of the `Test` attribute on `Class2` is omitted.</span></span>

### <a name="the-obsolete-attribute"></a><span data-ttu-id="19388-316">Obsolete 属性</span><span class="sxs-lookup"><span data-stu-id="19388-316">The Obsolete attribute</span></span>

<span data-ttu-id="19388-317">属性`Obsolete`使用できなくする型の型とメンバーをマークするために使用します。</span><span class="sxs-lookup"><span data-stu-id="19388-317">The attribute `Obsolete` is used to mark types and members of types that should no longer be used.</span></span>

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

<span data-ttu-id="19388-318">プログラムを使用して、型またはメンバーで装飾したかどうか、`Obsolete`属性に、コンパイラは、警告またはエラーを発行します。</span><span class="sxs-lookup"><span data-stu-id="19388-318">If a program uses a type or member that is decorated with the `Obsolete` attribute, the compiler issues a warning or an error.</span></span> <span data-ttu-id="19388-319">エラー パラメーターが指定されていない場合、または error パラメーターを指定し、値を持つ場合に、コンパイラが警告を発行する具体的には、`false`します。</span><span class="sxs-lookup"><span data-stu-id="19388-319">Specifically, the compiler issues a warning if no error parameter is provided, or if the error parameter is provided and has the value `false`.</span></span> <span data-ttu-id="19388-320">エラー パラメーターを指定し、値を持つ場合、コンパイラがエラーを発行`true`します。</span><span class="sxs-lookup"><span data-stu-id="19388-320">The compiler issues an error if the error parameter is specified and has the value `true`.</span></span>

<span data-ttu-id="19388-321">例</span><span class="sxs-lookup"><span data-stu-id="19388-321">In the example</span></span>

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

<span data-ttu-id="19388-322">クラスは、`A`で修飾されたが、`Obsolete`属性。</span><span class="sxs-lookup"><span data-stu-id="19388-322">the class `A` is decorated with the `Obsolete` attribute.</span></span> <span data-ttu-id="19388-323">各使用`A`で`Main`で指定されたメッセージを含む警告結果は、"このクラスは廃止されています。クラス B 代わりに使用します。"</span><span class="sxs-lookup"><span data-stu-id="19388-323">Each use of `A` in `Main` results in a warning that includes the specified message, "This class is obsolete; use class B instead."</span></span>

### <a name="caller-info-attributes"></a><span data-ttu-id="19388-324">呼び出し元情報属性</span><span class="sxs-lookup"><span data-stu-id="19388-324">Caller info attributes</span></span>

<span data-ttu-id="19388-325">ログ記録やレポートなどの目的で、関数メンバー呼び出し元のコードに関する特定のコンパイル時情報を取得するのに便利ですがあります。</span><span class="sxs-lookup"><span data-stu-id="19388-325">For purposes such as logging and reporting, it is sometimes useful for a function member to obtain certain compile-time information about the calling code.</span></span> <span data-ttu-id="19388-326">呼び出し元情報属性は、このような情報を透過的に渡す方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="19388-326">The caller info attributes provide a way to pass such information transparently.</span></span>

<span data-ttu-id="19388-327">オプションのパラメーターは、呼び出し元情報属性のいずれかで注釈が付け、呼び出しに対応する引数を省略することは必ずしも代わりに使用する既定のパラメーター値。</span><span class="sxs-lookup"><span data-stu-id="19388-327">When an optional parameter is annotated with one of the caller info attributes, omitting the corresponding argument in a call does not necessarily cause the default parameter value to be substituted.</span></span> <span data-ttu-id="19388-328">代わりに、指定した呼び出し元のコンテキストに関する情報を使用できる場合、その情報は、引数の値として渡されます。</span><span class="sxs-lookup"><span data-stu-id="19388-328">Instead, if the specified information about the calling context is available, that information will be passed as the argument value.</span></span>

<span data-ttu-id="19388-329">例:</span><span class="sxs-lookup"><span data-stu-id="19388-329">For example:</span></span>

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

<span data-ttu-id="19388-330">呼び出し`Log()`引数なしで印刷には、呼び出しの行の数とファイルのパスとを呼び出しが発生したメンバーの名前。</span><span class="sxs-lookup"><span data-stu-id="19388-330">A call to `Log()` with no arguments would print the line number and file path of the call, as well as the name of the member within which the call occurred.</span></span>

<span data-ttu-id="19388-331">呼び出し元情報属性は、デリゲートの宣言を含む省略可能なパラメーターなど、どこで実行できます。</span><span class="sxs-lookup"><span data-stu-id="19388-331">Caller info attributes can occur on optional parameters anywhere, including in delegate declarations.</span></span> <span data-ttu-id="19388-332">ただし、特定の呼び出し元情報属性では、パラメーターの型に代入された値から暗黙的な変換が必ずようにができる属性は、パラメーターの型に制限があります。</span><span class="sxs-lookup"><span data-stu-id="19388-332">However, the specific caller info attributes have restrictions on the types of the parameters they can attribute, so that there will always be an implicit conversion from a substituted value to the parameter type.</span></span>

<span data-ttu-id="19388-333">同じ呼び出し元情報属性を定義して、部分メソッド宣言の一部を実装する両方のパラメーターがエラーになります。</span><span class="sxs-lookup"><span data-stu-id="19388-333">It is an error to have the same caller info attribute on a parameter of both the defining and implementing part of a partial method declaration.</span></span> <span data-ttu-id="19388-334">実装の一部でのみ発生している呼び出し元情報属性は無視されますが、定義の一部で呼び出し元情報属性のみが適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-334">Only caller info attributes in the defining part are applied, whereas caller info attributes occurring only in the implementing part are ignored.</span></span>

<span data-ttu-id="19388-335">呼び出し元情報は、オーバー ロードの解決には影響しません。</span><span class="sxs-lookup"><span data-stu-id="19388-335">Caller information does not affect overload resolution.</span></span> <span data-ttu-id="19388-336">オーバー ロードの解決が省略されたその他の省略可能なパラメーターは無視されますが、同じ方法でこれらのパラメーターを無視する属性付きの省略可能なパラメーターは、呼び出し元のソース コードから省略されますが、([オーバー ロードの解決](expressions.md#overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="19388-336">As the attributed optional parameters are still omitted from the source code of the caller, overload resolution ignores those parameters in the same way it ignores other omitted optional parameters ([Overload resolution](expressions.md#overload-resolution)).</span></span>

<span data-ttu-id="19388-337">関数がソース コードで明示的に呼び出されるときにのみ、呼び出し元情報が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="19388-337">Caller information is only substituted when a function is explicitly invoked in source code.</span></span> <span data-ttu-id="19388-338">親の暗黙的なコンストラクターの呼び出しなど暗黙的な呼び出しでは、ソースの場所がないと、呼び出し元情報は置き換えられません。</span><span class="sxs-lookup"><span data-stu-id="19388-338">Implicit invocations such as implicit parent constructor calls do not have a source location and will not substitute caller information.</span></span> <span data-ttu-id="19388-339">また、動的にバインドされている呼び出しに呼び出し元情報は置き換えられません。</span><span class="sxs-lookup"><span data-stu-id="19388-339">Also, calls that are dynamically bound will not substitute caller information.</span></span> <span data-ttu-id="19388-340">ときに、呼び出し元情報をこのような場合で属性付きのパラメーターを省略すると、パラメーターの指定した既定値が代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-340">When a caller info attributed parameter is omitted in such cases, the specified default value of the parameter is used instead.</span></span>

<span data-ttu-id="19388-341">クエリ式は例外です。</span><span class="sxs-lookup"><span data-stu-id="19388-341">One exception is query-expressions.</span></span> <span data-ttu-id="19388-342">これらは構文の拡張と見なされ、呼び出し、呼び出し元情報属性を持つオプション パラメーターを省略するを展開する場合、呼び出し元情報が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="19388-342">These are considered syntactic expansions, and if the calls they expand to omit optional parameters with caller info attributes, caller information will be substituted.</span></span> <span data-ttu-id="19388-343">使用されている場所では、呼び出しはから生成されたクエリ句の場所です。</span><span class="sxs-lookup"><span data-stu-id="19388-343">The location used is the location of the query clause which the call was generated from.</span></span>

<span data-ttu-id="19388-344">次の順序で優先設定されているパラメーターを 1 つ以上の呼び出し元情報属性が指定されている場合: `CallerLineNumber`、 `CallerFilePath`、`CallerMemberName`します。</span><span class="sxs-lookup"><span data-stu-id="19388-344">If more than one caller info attribute is specified on a given parameter, they are preferred in the following order: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span></span>

#### <a name="the-callerlinenumber-attribute"></a><span data-ttu-id="19388-345">CallerLineNumber 属性</span><span class="sxs-lookup"><span data-stu-id="19388-345">The CallerLineNumber attribute</span></span>

<span data-ttu-id="19388-346">`System.Runtime.CompilerServices.CallerLineNumberAttribute`は省略可能なパラメーターで許可されている標準の暗黙的な変換がある場合に ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) 定数の値から`int.MaxValue`パラメーターの型にします。</span><span class="sxs-lookup"><span data-stu-id="19388-346">The `System.Runtime.CompilerServices.CallerLineNumberAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from the constant value `int.MaxValue` to the parameter's type.</span></span> <span data-ttu-id="19388-347">これにより、エラーが発生せず、その値になるまでの任意の負でない行番号を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="19388-347">This ensures that any non-negative line number up to that value can be passed without error.</span></span>

<span data-ttu-id="19388-348">ソース コード内の場所から関数の呼び出しは省略可能なパラメーターを省略する場合、 `CallerLineNumberAttribute`、その場所の行番号を表す数値リテラルが既定のパラメーター値ではなく、呼び出しに引数として使用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-348">If a function invocation from a location in source code omits an optional parameter with the `CallerLineNumberAttribute`, then a numeric literal representing that location's line number is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="19388-349">呼び出しは、複数の行にまたがっている場合、選択した行は実装依存を使用します。</span><span class="sxs-lookup"><span data-stu-id="19388-349">If the invocation spans multiple lines, the line chosen is implementation-dependent.</span></span>

<span data-ttu-id="19388-350">行番号の影響を受ける可能性がありますので注意`#line`ディレクティブ ([ディレクティブの行](lexical-structure.md#line-directives))。</span><span class="sxs-lookup"><span data-stu-id="19388-350">Note that the line number may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callerfilepath-attribute"></a><span data-ttu-id="19388-351">CallerFilePath 属性</span><span class="sxs-lookup"><span data-stu-id="19388-351">The CallerFilePath attribute</span></span>

<span data-ttu-id="19388-352">`System.Runtime.CompilerServices.CallerFilePathAttribute`は省略可能なパラメーターで許可されている標準の暗黙的な変換がある場合に ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) から`string`パラメーターの型にします。</span><span class="sxs-lookup"><span data-stu-id="19388-352">The `System.Runtime.CompilerServices.CallerFilePathAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="19388-353">ソース コード内の場所から関数の呼び出しは省略可能なパラメーターを省略する場合、 `CallerFilePathAttribute`、その場所のファイル パスを表す文字列リテラルが既定のパラメーター値ではなく、呼び出しに引数として使用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-353">If a function invocation from a location in source code omits an optional parameter with the `CallerFilePathAttribute`, then a string literal representing that location's file path is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="19388-354">ファイルのパスの形式は、実装によって異なります。</span><span class="sxs-lookup"><span data-stu-id="19388-354">The format of the file path is implementation-dependent.</span></span>

<span data-ttu-id="19388-355">ファイルのパスはによって影響を受ける可能性がありますので注意`#line`ディレクティブ ([ディレクティブの行](lexical-structure.md#line-directives))。</span><span class="sxs-lookup"><span data-stu-id="19388-355">Note that the file path may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callermembername-attribute"></a><span data-ttu-id="19388-356">CallerMemberName 属性</span><span class="sxs-lookup"><span data-stu-id="19388-356">The CallerMemberName attribute</span></span>

<span data-ttu-id="19388-357">`System.Runtime.CompilerServices.CallerMemberNameAttribute`は省略可能なパラメーターで許可されている標準の暗黙的な変換がある場合に ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) から`string`パラメーターの型にします。</span><span class="sxs-lookup"><span data-stu-id="19388-357">The `System.Runtime.CompilerServices.CallerMemberNameAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="19388-358">ソース コードでは省略可能なパラメーターを省略自体またはその戻り値の型、パラメーターまたは型パラメーターで、関数メンバーの本文内または属性内の場所から関数の呼び出しが関数メンバーに適用されている場合、 `CallerMemberNameAttribute`、そのメンバーの名前を表す文字列リテラルは、パラメーターの既定値ではなく、呼び出しの引数として使用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-358">If a function invocation from a location within the body of a function member or within an attribute applied to the function member itself or its return type, parameters or type parameters in source code omits an optional parameter with the `CallerMemberNameAttribute`, then a string literal representing the name of that member is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="19388-359">ジェネリック メソッド内で発生する呼び出しは、型パラメーター リストなし、メソッド名のみを使用します。</span><span class="sxs-lookup"><span data-stu-id="19388-359">For invocations that occur within generic methods, only the method name itself is used, without the type parameter list.</span></span>

<span data-ttu-id="19388-360">呼び出しで明示的なインターフェイス メンバーの実装内で発生する、上記のインターフェイス修飾なし、メソッド名自体は使用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-360">For invocations that occur within explicit interface member implementations, only the method name itself is used, without the preceding interface qualification.</span></span>

<span data-ttu-id="19388-361">プロパティまたはイベントのアクセサー内で発生するための呼び出し、メンバー名を使用するプロパティまたはイベント自体は。</span><span class="sxs-lookup"><span data-stu-id="19388-361">For invocations that occur within property or event accessors, the member name used is that of the property or event itself.</span></span>

<span data-ttu-id="19388-362">インデクサーのアクセサー内で発生する呼び出しでは、使用されるメンバーの名前は、によって指定された、 `IndexerNameAttribute` ([、IndexerName 属性](attributes.md#the-indexername-attribute)) 存在する場合は、インデクサーのメンバーまたは既定の名前で`Item`それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="19388-362">For invocations that occur within indexer accessors, the member name used is that supplied by an `IndexerNameAttribute` ([The IndexerName attribute](attributes.md#the-indexername-attribute)) on the indexer member, if present, or the default name `Item` otherwise.</span></span>

<span data-ttu-id="19388-363">呼び出しで、メンバーのインスタンス コンストラクター、静的コンストラクター、デストラクター、および演算子の宣言内で発生するは、使用される名前は、実装によって異なります。</span><span class="sxs-lookup"><span data-stu-id="19388-363">For invocations that occur within declarations of instance constructors, static constructors, destructors and operators the member name used is implementation-dependent.</span></span>

## <a name="attributes-for-interoperation"></a><span data-ttu-id="19388-364">相互運用の属性</span><span class="sxs-lookup"><span data-stu-id="19388-364">Attributes for Interoperation</span></span>

<span data-ttu-id="19388-365">メモ:このセクションでは、C# の Microsoft .NET の実装にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-365">Note: This section is applicable only to the Microsoft .NET implementation of C#.</span></span>

### <a name="interoperation-with-com-and-win32-components"></a><span data-ttu-id="19388-366">COM および Win32 コンポーネントとの相互運用</span><span class="sxs-lookup"><span data-stu-id="19388-366">Interoperation with COM and Win32 components</span></span>

<span data-ttu-id="19388-367">.NET ランタイムでは、COM と Win32 の Dll を使用して記述されたコンポーネントと相互運用する C# プログラムを有効にする属性の数が多いを提供します。</span><span class="sxs-lookup"><span data-stu-id="19388-367">The .NET run-time provides a large number of attributes that enable C# programs to interoperate with components written using COM and Win32 DLLs.</span></span> <span data-ttu-id="19388-368">たとえば、`DllImport`で属性を使用できます、`static extern`に Win32 DLL に含まれるメソッドの実装があることを示す方法。</span><span class="sxs-lookup"><span data-stu-id="19388-368">For example, the `DllImport` attribute can be used on a `static extern` method to indicate that the implementation of the method is to be found in a Win32 DLL.</span></span> <span data-ttu-id="19388-369">これらの属性がある、`System.Runtime.InteropServices`名前空間、およびこれらの属性の詳細なドキュメントが、.NET ランタイムのドキュメントが見つかりました。</span><span class="sxs-lookup"><span data-stu-id="19388-369">These attributes are found in the `System.Runtime.InteropServices` namespace, and detailed documentation for these attributes is found in the .NET runtime documentation.</span></span>

### <a name="interoperation-with-other-net-languages"></a><span data-ttu-id="19388-370">他の .NET 言語との相互運用</span><span class="sxs-lookup"><span data-stu-id="19388-370">Interoperation with other .NET languages</span></span>

#### <a name="the-indexername-attribute"></a><span data-ttu-id="19388-371">IndexerName 属性</span><span class="sxs-lookup"><span data-stu-id="19388-371">The IndexerName attribute</span></span>

<span data-ttu-id="19388-372">インデクサーはインデックス付きのプロパティを使用して .NET で実装され、名前 .NET メタデータであります。</span><span class="sxs-lookup"><span data-stu-id="19388-372">Indexers are implemented in .NET using indexed properties, and have a name in the .NET metadata.</span></span> <span data-ttu-id="19388-373">ない場合は`IndexerName`属性は、インデクサーでは、その名前存在`Item`既定で使用されます。</span><span class="sxs-lookup"><span data-stu-id="19388-373">If no `IndexerName` attribute is present for an indexer, then the name `Item` is used by default.</span></span> <span data-ttu-id="19388-374">`IndexerName`属性により、開発者はこの既定をオーバーライドし、別の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="19388-374">The `IndexerName` attribute enables a developer to override this default and specify a different name.</span></span>

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
