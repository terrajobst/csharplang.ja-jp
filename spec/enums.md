---
ms.openlocfilehash: c3b716e6eb331be2ee33fffeb42c1e2406cd3a5c
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229814"
---
# <a name="enums"></a><span data-ttu-id="ca1e8-101">列挙体</span><span class="sxs-lookup"><span data-stu-id="ca1e8-101">Enums</span></span>

<span data-ttu-id="ca1e8-102">***列挙型***は個別の値の型です ([値の型](types.md#value-types)) 一連の名前付き定数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="ca1e8-103">例では、</span><span class="sxs-lookup"><span data-stu-id="ca1e8-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="ca1e8-104">という名前の列挙型を宣言します`Color`メンバーと`Red`、 `Green`、および`Blue`します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="ca1e8-105">列挙型の宣言</span><span class="sxs-lookup"><span data-stu-id="ca1e8-105">Enum declarations</span></span>

<span data-ttu-id="ca1e8-106">列挙型の宣言は、新しい列挙型を宣言します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="ca1e8-107">列挙型の宣言がキーワードで始まり`enum`名前、アクセシビリティ、基になる型、および列挙型のメンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="ca1e8-108">各列挙型が、対応する整数型と呼ばれる、***基になる型***の列挙型。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="ca1e8-109">この基になる型は、列挙体で定義されているすべての列挙子の値を表現できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="ca1e8-110">列挙型の宣言は、基になる型に明示的に宣言可能性があります`byte`、 `sbyte`、 `short`、 `ushort`、 `int`、 `uint`、`long`または`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="ca1e8-111">なお`char`を基になる型として使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="ca1e8-112">列挙型の宣言を基になる型を明示的に宣言しないが、基になる型の`int`します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="ca1e8-113">例では、</span><span class="sxs-lookup"><span data-stu-id="ca1e8-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="ca1e8-114">基になる型の列挙型を宣言します`long`します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="ca1e8-115">開発者は、基になる型を使用することができます`long`などの範囲内にある値の使用を有効にするには、`long`の範囲内にありませんが、 `int`、または将来には、このオプションを保持するためにします。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="ca1e8-116">列挙型修飾子</span><span class="sxs-lookup"><span data-stu-id="ca1e8-116">Enum modifiers</span></span>

<span data-ttu-id="ca1e8-117">*Enum_declaration*列挙型修飾子のシーケンスを必要に応じて含めることができます。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="ca1e8-118">同じ修飾子を列挙型の宣言内で複数回のコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="ca1e8-119">列挙型の宣言の修飾子がクラス宣言のものと同じ意味を持ちます ([修飾子をクラス](classes.md#class-modifiers))。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="ca1e8-120">ただしを`abstract`と`sealed`修飾子は、enum 宣言では許可されていません。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="ca1e8-121">列挙型は abstract にすることはできません、派生は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="ca1e8-122">列挙型メンバー</span><span class="sxs-lookup"><span data-stu-id="ca1e8-122">Enum members</span></span>

<span data-ttu-id="ca1e8-123">列挙型の宣言の本体は、列挙型の名前付き定数である、0 個以上の列挙型メンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="ca1e8-124">2 つの列挙型メンバーには、同じ名前を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="ca1e8-125">各列挙型のメンバーでは、関連付けられている定数値があります。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="ca1e8-126">この値の型は、列挙型の基になる型です。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="ca1e8-127">各列挙型メンバーの定数値は、列挙型の基になる型の範囲でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="ca1e8-128">例では、</span><span class="sxs-lookup"><span data-stu-id="ca1e8-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="ca1e8-129">ため、コンパイル時エラー結果定数値`-1`、 `-2`、および`-3`の基になる整数型の範囲にない`uint`します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="ca1e8-130">複数の列挙型メンバーには、同じ関連付けられた値を共有できます。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="ca1e8-131">例では、</span><span class="sxs-lookup"><span data-stu-id="ca1e8-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="ca1e8-132">どの 2 つの列挙型メンバー - 列挙型を示します`Blue`と`Max`--が同じ関連付けられている値。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="ca1e8-133">列挙型のメンバーに関連付けられた値は、暗黙的または明示的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="ca1e8-134">列挙型メンバーの宣言がある場合、 *constant_expression*列挙型の基になる型に暗黙的に変換、定数式の値に初期化子は列挙型のメンバーに関連する値。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="ca1e8-135">列挙型メンバーの宣言に初期化子があるない場合、暗黙的に、次のように、関連付けられた値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="ca1e8-136">列挙型メンバーが最初に列挙型で宣言された列挙型のメンバーである場合は、関連する値は 0 です。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="ca1e8-137">それ以外の場合、列挙型のメンバーに関連付けられた値は、1 つの直前の列挙型のメンバーに関連付けられた値を増やすことで取得されます。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="ca1e8-138">この増加する値は、基になる型で表現できる値の範囲内である必要があります、それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="ca1e8-139">例では、</span><span class="sxs-lookup"><span data-stu-id="ca1e8-139">The example</span></span>

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

<span data-ttu-id="ca1e8-140">列挙型メンバーの名前と関連付けられている値を出力します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="ca1e8-141">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-141">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="ca1e8-142">次の場合。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-142">for the following reasons:</span></span>

*  <span data-ttu-id="ca1e8-143">列挙メンバー `Red` (初期化子を持たない列挙型の最初のメンバーであるため) を 0 以外の値を自動的に割り当てられます</span><span class="sxs-lookup"><span data-stu-id="ca1e8-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="ca1e8-144">列挙メンバー`Green`値が明示的に指定`10`;</span><span class="sxs-lookup"><span data-stu-id="ca1e8-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="ca1e8-145">列挙型のメンバーと`Blue`直前のメンバーより 1 大きい値を自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="ca1e8-146">列挙型のメンバーに関連付けられた値よりも直接的または間接的に関連付けられている列挙型メンバーの値を使用します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="ca1e8-147">この循環制限以外、列挙型メンバーの初期化子は、テキストの位置に関係なく、他のメンバー初期化子を自由に参照します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="ca1e8-148">列挙型メンバー初期化子内では他の列挙型メンバーを参照するときのキャストは必要ありませんように他の列挙型メンバーの値を基になる型の型を持つものとして扱われます常に。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="ca1e8-149">例では、</span><span class="sxs-lookup"><span data-stu-id="ca1e8-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="ca1e8-150">コンパイル時エラーのため、結果の宣言`A`と`B`が循環します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="ca1e8-151">`A` 依存`B`明示的と`B`異なります`A`暗黙的にします。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="ca1e8-152">列挙型メンバーの名前し、クラス内のフィールドとほぼ同じ方法でスコープします。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="ca1e8-153">列挙型のメンバーのスコープは、その列挙型の本体です。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="ca1e8-154">そのスコープでは、単純な名前を指定して列挙型のメンバーを参照できます。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="ca1e8-155">その他のすべてのコードからその列挙型の名前を持つ列挙型のメンバーの名前を修飾する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="ca1e8-156">列挙型のメンバーには、任意の宣言されたアクセシビリティはありません。--列挙型のメンバーにアクセスできるは、それを含む列挙型がアクセス可能な場合。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="ca1e8-157">System.Enum 型</span><span class="sxs-lookup"><span data-stu-id="ca1e8-157">The System.Enum type</span></span>

<span data-ttu-id="ca1e8-158">型`System.Enum`(これは distinct とは異なる列挙型の基になる型から) すべての列挙型の抽象基本クラスから継承したメンバーは、`System.Enum`は任意の列挙型で使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="ca1e8-159">ボックス化変換 ([ボックス化変換](types.md#boxing-conversions)) から任意の列挙型に存在する`System.Enum`、ボックス化変換 ([ボックス化解除変換](types.md#unboxing-conversions)) から存在する`System.Enum`任意の列挙型にします。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="ca1e8-160">なお`System.Enum`自体ではありません、 *enum_type*します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="ca1e8-161">*Class_type*すべてから*enum_type*s が派生します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="ca1e8-162">型`System.Enum`型から継承`System.ValueType`([、System.ValueType 型](types.md#the-systemvaluetype-type)) であり、さらに、型から継承`object`します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="ca1e8-163">実行時の型の値に`System.Enum`できる`null`または任意の列挙型のボックス化された値への参照。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="ca1e8-164">列挙型の値、および操作</span><span class="sxs-lookup"><span data-stu-id="ca1e8-164">Enum values and operations</span></span>

<span data-ttu-id="ca1e8-165">各列挙型は、別個の型を定義します。明示的な列挙値の型変換を ([列挙値の明示的な変換](conversions.md#explicit-enumeration-conversions)) 列挙型と整数型、または 2 つの列挙型間の変換が必要です。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="ca1e8-166">列挙型上で実行できる値のセットは、その列挙型のメンバーでは制限されません。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="ca1e8-167">具体的には、enum の基になる型の任意の値は、列挙型にキャストできるし、その列挙型の個別の有効な値は。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="ca1e8-168">列挙型のメンバー、列挙型の型である (その他の列挙型メンバーの初期化子内を除く: を参照してください[列挙型メンバー](enums.md#enum-members))。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="ca1e8-169">列挙型のメンバーの値が列挙型で宣言された`E`関連付けられている値を持つ`v`は`(E)v`します。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="ca1e8-170">列挙型の値では、次の演算子を使用できます: `==`、 `!=`、 `<`、 `>`、 `<=`、 `>=`  ([列挙型の比較演算子](expressions.md#enumeration-comparison-operators))、バイナリ`+`  ([加算演算子](expressions.md#addition-operator))、バイナリ`-`  ([減算演算子](expressions.md#subtraction-operator))、 `^`、 `&`、 `|`  ([列挙体の論理演算子](expressions.md#enumeration-logical-operators))、 `~`  ([ビットごとの補数演算子](expressions.md#bitwise-complement-operator))、`++`と`--` ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)と[前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="ca1e8-171">すべての列挙型が自動的にクラスから派生`System.Enum`(さらから派生するには、`System.ValueType`と`object`)。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="ca1e8-172">したがって、このクラスの継承されたメソッドとプロパティは、列挙型の値で使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca1e8-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
