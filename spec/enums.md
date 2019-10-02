---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703964"
---
# <a name="enums"></a><span data-ttu-id="17cfa-101">列挙体</span><span class="sxs-lookup"><span data-stu-id="17cfa-101">Enums</span></span>

<span data-ttu-id="17cfa-102">***列挙型***は、名前付き定数のセットを宣言する個別の値型 ([値型](types.md#value-types)) です。</span><span class="sxs-lookup"><span data-stu-id="17cfa-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="17cfa-103">例</span><span class="sxs-lookup"><span data-stu-id="17cfa-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="17cfa-104">`Color` という名前の列挙型を宣言し、メンバー `Red`、`Green`、および `Blue` を指定します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="17cfa-105">列挙型の宣言</span><span class="sxs-lookup"><span data-stu-id="17cfa-105">Enum declarations</span></span>

<span data-ttu-id="17cfa-106">列挙型宣言は、新しい列挙型を宣言します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="17cfa-107">列挙型宣言は、キーワード `enum` で始まり、名前、アクセシビリティ、基になる型、および列挙型のメンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

<span data-ttu-id="17cfa-108">各列挙型には、列挙型の***基になる型***と呼ばれる対応する整数型があります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="17cfa-109">この基になる型は、列挙体で定義されているすべての列挙子の値を表すことができる必要があります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="17cfa-110">列挙型宣言では、基になる型 `byte`、`sbyte`、`short`、`ushort`、@no__t 4、`uint`、`long`、または `ulong` が明示的に宣言される場合があります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="17cfa-111">@No__t-0 は基になる型として使用できないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="17cfa-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="17cfa-112">基になる型を明示的に宣言しない列挙型宣言では、基になる型が `int` になります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="17cfa-113">例</span><span class="sxs-lookup"><span data-stu-id="17cfa-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="17cfa-114">基になる型が `long` の列挙型を宣言します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="17cfa-115">開発者は、例に示すように、基になる型 `long` を使用して、@no__t の範囲内の値の使用を有効にすることができます。たとえば、`int` の範囲に含まれていない値を使用できます。また、今後このオプションを保持することもできます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="17cfa-116">列挙修飾子</span><span class="sxs-lookup"><span data-stu-id="17cfa-116">Enum modifiers</span></span>

<span data-ttu-id="17cfa-117">*Enum_declaration*には、必要に応じて列挙修飾子のシーケンスを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="17cfa-118">同じ修飾子が列挙型宣言で複数回出現する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="17cfa-119">列挙型宣言の修飾子は、クラス宣言 ([クラス修飾子](classes.md#class-modifiers)) と同じ意味を持ちます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="17cfa-120">ただし、enum 宣言では、`abstract` および `sealed` 修飾子は使用できません。</span><span class="sxs-lookup"><span data-stu-id="17cfa-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="17cfa-121">列挙型を抽象にすることはできません。また、派生を許可しません。</span><span class="sxs-lookup"><span data-stu-id="17cfa-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="17cfa-122">列挙型メンバー</span><span class="sxs-lookup"><span data-stu-id="17cfa-122">Enum members</span></span>

<span data-ttu-id="17cfa-123">列挙型宣言の本体では、列挙型の名前付き定数である0個以上の列挙型メンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="17cfa-124">2つの列挙メンバーが同じ名前を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="17cfa-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="17cfa-125">各列挙型メンバーには、定数値が関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="17cfa-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="17cfa-126">この値の型は、それを含む列挙型の基になる型です。</span><span class="sxs-lookup"><span data-stu-id="17cfa-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="17cfa-127">各列挙型メンバーの定数値は、列挙型の基になる型の範囲内である必要があります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="17cfa-128">例</span><span class="sxs-lookup"><span data-stu-id="17cfa-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="17cfa-129">定数値 `-1`、`-2`、`-3` が基になる整数 @no__t 型の範囲内にないため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="17cfa-130">複数の列挙メンバーが同じ関連する値を共有する場合があります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="17cfa-131">例</span><span class="sxs-lookup"><span data-stu-id="17cfa-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="17cfa-132">2つの列挙メンバー (`Blue` と `Max`) に同じ値が関連付けられている列挙体を示します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="17cfa-133">列挙メンバーの関連する値は、暗黙的または明示的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="17cfa-134">列挙型メンバーの宣言に*constant_expression*初期化子がある場合、その定数式の値は列挙型の基になる型に暗黙的に変換され、列挙型メンバーの値になります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="17cfa-135">列挙型メンバーの宣言に初期化子がない場合、次のように、そのメンバーに関連付けられている値が暗黙的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="17cfa-136">列挙型のメンバーが列挙型で宣言された最初の列挙型のメンバーである場合、それに関連付けられた値は0になります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="17cfa-137">それ以外の場合、列挙型メンバーの関連する値を取得するには、直前の列挙型メンバーの関連する値を1つ増やします。</span><span class="sxs-lookup"><span data-stu-id="17cfa-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="17cfa-138">この増加した値は、基になる型で表すことができる値の範囲内である必要があります。それ以外の場合は、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="17cfa-139">例</span><span class="sxs-lookup"><span data-stu-id="17cfa-139">The example</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

<span data-ttu-id="17cfa-140">列挙メンバー名とそれに関連付けられている値を出力します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="17cfa-141">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-141">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="17cfa-142">次の理由が考えられます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-142">for the following reasons:</span></span>

*  <span data-ttu-id="17cfa-143">列挙型のメンバー `Red` には、初期化子がなく、最初の列挙型のメンバーであるため、値0が自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="17cfa-144">列挙型のメンバー `Green` に明示的に値 `10` を指定します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="17cfa-145">列挙型のメンバー `Blue` には、その前にあるメンバーよりも大きい値が自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="17cfa-146">列挙型メンバーに関連付けられた値は、それ自体に関連付けられている列挙型メンバーの値を直接または間接的に使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="17cfa-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="17cfa-147">この循環制限以外に、列挙型メンバー初期化子は、テキストの位置に関係なく、他の列挙型メンバーの初期化子を自由に参照できます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="17cfa-148">列挙メンバー初期化子内では、他の列挙型メンバーの値は、他の列挙型のメンバーを参照するときにキャストが不要になるように、その基になる型の型として常に処理されます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="17cfa-149">例</span><span class="sxs-lookup"><span data-stu-id="17cfa-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="17cfa-150">`A` と `B` の宣言が循環しているため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="17cfa-151">`A` は `B` に明示的に依存し、@no__t は暗黙的に @no__t に依存します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="17cfa-152">列挙型メンバーには、クラス内のフィールドに厳密に似た方法で名前が付けられます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="17cfa-153">列挙型メンバーのスコープは、それを含んでいる列挙型の本体です。</span><span class="sxs-lookup"><span data-stu-id="17cfa-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="17cfa-154">そのスコープ内では、列挙型のメンバーを単純な名前で参照できます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="17cfa-155">他のすべてのコードからは、列挙型メンバーの名前が列挙型の名前で修飾されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="17cfa-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="17cfa-156">列挙型メンバーは、宣言されたアクセシビリティを持っていません。列挙型メンバーには、その列挙型がアクセス可能な場合にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="17cfa-157">System.enum 型</span><span class="sxs-lookup"><span data-stu-id="17cfa-157">The System.Enum type</span></span>

<span data-ttu-id="17cfa-158">型 `System.Enum` は、すべての列挙型の抽象基本クラス (これは列挙型の基になる型とは異なります) であり、`System.Enum` から継承されたメンバーは任意の列挙型で使用できます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="17cfa-159">任意の列挙型から `System.Enum` へのボックス変換 ([ボックス](types.md#boxing-conversions)化変換) が存在します。また、`System.Enum` から任意の列挙型へのボックス化変換 ([アンボックス](types.md#unboxing-conversions)変換) が存在します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="17cfa-160">@No__t-0 はそれ自体が*enum_type*ではないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="17cfa-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="17cfa-161">むしろ、すべての*enum_type*が派生している*class_type*です。</span><span class="sxs-lookup"><span data-stu-id="17cfa-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="17cfa-162">@No__t-0 の型は、型 `System.ValueType` (system.string[型](types.md#the-systemvaluetype-type)) から継承され、その後、型 `object` から継承されます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="17cfa-163">実行時には `System.Enum` 型の値を `null`、または任意の列挙型のボックス化された値への参照を指定できます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="17cfa-164">列挙値と操作</span><span class="sxs-lookup"><span data-stu-id="17cfa-164">Enum values and operations</span></span>

<span data-ttu-id="17cfa-165">それぞれの列挙型は、個別の型を定義します。列挙型と整数型の間、または2つの列挙型間で変換を行うには、明示的な列挙変換 ([明示的な列挙](conversions.md#explicit-enumeration-conversions)変換) が必要です。</span><span class="sxs-lookup"><span data-stu-id="17cfa-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="17cfa-166">列挙型が受け取ることができる値のセットは、列挙型のメンバーによって制限されません。</span><span class="sxs-lookup"><span data-stu-id="17cfa-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="17cfa-167">特に、列挙型の基になる型の任意の値を列挙型にキャストできます。これは、その列挙型の個別の有効な値です。</span><span class="sxs-lookup"><span data-stu-id="17cfa-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="17cfa-168">列挙型メンバーには、それを含む列挙型の型があります (他の列挙型メンバーの初期化子内を除き[ます)。列挙型](enums.md#enum-members)メンバーを参照してください。</span><span class="sxs-lookup"><span data-stu-id="17cfa-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="17cfa-169">列挙型として宣言された列挙型メンバーの値が、関連付けられている値 `v` である場合は、-1 が `(E)v` @no__t ます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="17cfa-170">次の演算子は、列挙型の値に対して使用できます: @no__t 0、`!=`、`<`、`>`、`<=`、`>=` @ no__t ([列挙比較演算子](expressions.md#enumeration-comparison-operators))、バイナリ `+` @ No__t ([加算演算子](expressions.md#addition-operator))、バイナリ 1 @ no__t-12 ([減算演算子](expressions.md#subtraction-operator))、4、5、6 @ no__t-17 ([列挙論理演算子](expressions.md#enumeration-logical-operators))、9 @ No__t-20 ([ビットごとの補数演算子](expressions.md#bitwise-complement-operator))、2 と 3 @ no__t-24 ([後置インクリメント)およびデクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)と[前置インクリメント演算子と前置デクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="17cfa-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="17cfa-171">すべての列挙型は、自動的にクラスから派生します `System.Enum` (つまり、`System.ValueType` と `object`) から派生します。</span><span class="sxs-lookup"><span data-stu-id="17cfa-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="17cfa-172">したがって、このクラスの継承されたメソッドとプロパティは、列挙型の値に対して使用できます。</span><span class="sxs-lookup"><span data-stu-id="17cfa-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
