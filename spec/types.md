---
ms.openlocfilehash: a28397b1ce97dbead6d5014e2b20e108a1018502
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488791"
---
# <a name="types"></a><span data-ttu-id="01ae4-101">型</span><span class="sxs-lookup"><span data-stu-id="01ae4-101">Types</span></span>

<span data-ttu-id="01ae4-102">C# 言語の型は、2 つの主なカテゴリに分類されます。***値の型***と***参照型***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-102">The types of the C# language are divided into two main categories: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="01ae4-103">値型と参照型の両方があります***ジェネリック型***、これは、1 つ以上時間がかかる***パラメーター入力***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-103">Both value types and reference types may be ***generic types***, which take one or more ***type parameters***.</span></span> <span data-ttu-id="01ae4-104">型パラメーターでは、両方の値の型を指定でき、型を参照することができます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-104">Type parameters can designate both value types and reference types.</span></span>

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

<span data-ttu-id="01ae4-105">型、ポインターの最終的なカテゴリは、アンセーフ コードでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-105">The final category of types, pointers, is available only in unsafe code.</span></span> <span data-ttu-id="01ae4-106">これについては説明でさらに[ポインター型](unsafe-code.md#pointer-types)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-106">This is discussed further in [Pointer types](unsafe-code.md#pointer-types).</span></span>

<span data-ttu-id="01ae4-107">値の型とは異なりは参照型で値型の変数は、データを直接格納変数参照の種類のストアは***参照***として認識されている後者、データに***オブジェクト***.</span><span class="sxs-lookup"><span data-stu-id="01ae4-107">Value types differ from reference types in that variables of the value types directly contain their data, whereas variables of the reference types store ***references*** to their data, the latter being known as ***objects***.</span></span> <span data-ttu-id="01ae4-108">参照型では 2 つの変数が同じオブジェクトを参照できるため、他の変数によって参照されるオブジェクトに影響を与える 1 つの変数に対する操作。</span><span class="sxs-lookup"><span data-stu-id="01ae4-108">With reference types, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="01ae4-109">値型の場合は、各変数が独自のデータのコピーにあり、操作に影響を与えるもう 1 つのことはできません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-109">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span>

<span data-ttu-id="01ae4-110">C#型システムは任意の型の値をオブジェクトとして処理できるように統合されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-110">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="01ae4-111">C# における型はすべて、直接的または間接的に `object` クラス型から派生し、`object` はすべての型の究極の基底クラスです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-111">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="01ae4-112">参照型の値は、値を単純に `object` 型としてみなすことによってオブジェクトとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-112">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="01ae4-113">値型の値がボックス化とボックス化解除操作を実行することによってオブジェクトとして扱われます ([ボックス化とボックス化解除](types.md#boxing-and-unboxing))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-113">Values of value types are treated as objects by performing boxing and unboxing operations ([Boxing and unboxing](types.md#boxing-and-unboxing)).</span></span>

## <a name="value-types"></a><span data-ttu-id="01ae4-114">値型</span><span class="sxs-lookup"><span data-stu-id="01ae4-114">Value types</span></span>

<span data-ttu-id="01ae4-115">値型は、構造体の型または列挙型のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-115">A value type is either a struct type or an enumeration type.</span></span> <span data-ttu-id="01ae4-116">C# と呼ばれる定義済みの構造体の型のセットを提供する、***単純型***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-116">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="01ae4-117">単純型は、予約語によって特定されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-117">The simple types are identified through reserved words.</span></span>

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

<span data-ttu-id="01ae4-118">参照型の変数とは異なり、値型の変数には、値も含めることができます`null`値の型が null 許容型である場合にのみです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-118">Unlike a variable of a reference type, a variable of a value type can contain the value `null` only if the value type is a nullable type.</span></span>  <span data-ttu-id="01ae4-119">すべての null 非許容値型の値と値の同じセットを示す対応する null 許容値型は`null`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-119">For every non-nullable value type there is a corresponding nullable value type denoting the same set of values plus the value `null`.</span></span>

<span data-ttu-id="01ae4-120">値型の変数への代入では、割り当てられている値のコピーを作成します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-120">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="01ae4-121">これは、コピー、参照が参照によって識別されるオブジェクトではなく、参照型の変数に割り当てと異なります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-121">This differs from assignment to a variable of a reference type, which copies the reference but not the object identified by the reference.</span></span>

### <a name="the-systemvaluetype-type"></a><span data-ttu-id="01ae4-122">System.ValueType 型</span><span class="sxs-lookup"><span data-stu-id="01ae4-122">The System.ValueType type</span></span>

<span data-ttu-id="01ae4-123">すべての値型がクラスから暗黙的に継承`System.ValueType`であり、さらに、クラスを継承`object`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-123">All value types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="01ae4-124">任意の種類、値型から派生することはできませんし、値の型が暗黙的にシールしたがって ([クラスをシール](classes.md#sealed-classes))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-124">It is not possible for any type to derive from a value type, and value types are thus implicitly sealed ([Sealed classes](classes.md#sealed-classes)).</span></span>

<span data-ttu-id="01ae4-125">なお`System.ValueType`自体ではありません、 *value_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-125">Note that `System.ValueType` is not itself a *value_type*.</span></span> <span data-ttu-id="01ae4-126">*Class_type*すべてから*value_type*s が自動的に派生します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-126">Rather, it is a *class_type* from which all *value_type*s are automatically derived.</span></span>

### <a name="default-constructors"></a><span data-ttu-id="01ae4-127">既定のコンストラクター</span><span class="sxs-lookup"><span data-stu-id="01ae4-127">Default constructors</span></span>

<span data-ttu-id="01ae4-128">すべての値型が暗黙的に呼び出されるパラメーターなしのパブリック インスタンス コンス トラクターを宣言、***既定のコンス トラクター***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-128">All value types implicitly declare a public parameterless instance constructor called the ***default constructor***.</span></span> <span data-ttu-id="01ae4-129">既定のコンス トラクターと呼ばれる、ゼロ初期化のインスタンスを返します、***既定値***値の型。</span><span class="sxs-lookup"><span data-stu-id="01ae4-129">The default constructor returns a zero-initialized instance known as the ***default value*** for the value type:</span></span>

*  <span data-ttu-id="01ae4-130">すべての*simple_type*s、既定値はすべてゼロのビット パターンで生成される値。</span><span class="sxs-lookup"><span data-stu-id="01ae4-130">For all *simple_type*s, the default value is the value produced by a bit pattern of all zeros:</span></span>
    * <span data-ttu-id="01ae4-131">`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、および`ulong`、既定値は`0`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-131">For `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, and `ulong`, the default value is `0`.</span></span>
    * <span data-ttu-id="01ae4-132">`char`、既定値は`'\x0000'`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-132">For `char`, the default value is `'\x0000'`.</span></span>
    * <span data-ttu-id="01ae4-133">`float`、既定値は`0.0f`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-133">For `float`, the default value is `0.0f`.</span></span>
    * <span data-ttu-id="01ae4-134">`double`、既定値は`0.0d`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-134">For `double`, the default value is `0.0d`.</span></span>
    * <span data-ttu-id="01ae4-135">`decimal`、既定値は`0.0m`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-135">For `decimal`, the default value is `0.0m`.</span></span>
    * <span data-ttu-id="01ae4-136">`bool`、既定値は`false`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-136">For `bool`, the default value is `false`.</span></span>
*  <span data-ttu-id="01ae4-137">*Enum_type* `E`、既定値は`0`型に変換された`E`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-137">For an *enum_type* `E`, the default value is `0`, converted to the type `E`.</span></span>
*  <span data-ttu-id="01ae4-138">*Struct_type*、既定値は、すべての値型フィールドが既定値をすべて参照型フィールドに設定された値`null`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-138">For a *struct_type*, the default value is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>
*  <span data-ttu-id="01ae4-139">*Nullable_type*既定値は、対象のインスタンス、`HasValue`プロパティが false と`Value`プロパティが定義されていません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-139">For a *nullable_type* the default value is an instance for which the `HasValue` property is false and the `Value` property is undefined.</span></span> <span data-ttu-id="01ae4-140">既定値とも呼ばれますが、 ***null 値***null 許容型のです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-140">The default value is also known as the ***null value*** of the nullable type.</span></span>

<span data-ttu-id="01ae4-141">使用して値型の既定のコンス トラクターを呼び出すその他のインスタンス コンスのように、`new`演算子。</span><span class="sxs-lookup"><span data-stu-id="01ae4-141">Like any other instance constructor, the default constructor of a value type is invoked using the `new` operator.</span></span> <span data-ttu-id="01ae4-142">効率性の理由から、この要件はコンス トラクターの呼び出しを生成する実装が実際にあるありません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-142">For efficiency reasons, this requirement is not intended to actually have the implementation generate a constructor call.</span></span> <span data-ttu-id="01ae4-143">変数の下の例では`i`と`j`両方は 0 に初期化します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-143">In the example below, variables `i` and `j` are both initialized to zero.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

<span data-ttu-id="01ae4-144">すべての値型に暗黙的には、パラメーターなしのパブリック インスタンス コンス トラクターがあるために、パラメーターなしのコンス トラクターの明示的な宣言を格納する構造体の型のことはできません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-144">Because every value type implicitly has a public parameterless instance constructor, it is not possible for a struct type to contain an explicit declaration of a parameterless constructor.</span></span> <span data-ttu-id="01ae4-145">パラメーター化されたインスタンス コンス トラクターを宣言する構造体の型が許可されているただし ([コンス トラクター](structs.md#constructors))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-145">A struct type is however permitted to declare parameterized instance constructors ([Constructors](structs.md#constructors)).</span></span>

### <a name="struct-types"></a><span data-ttu-id="01ae4-146">構造体の型</span><span class="sxs-lookup"><span data-stu-id="01ae4-146">Struct types</span></span>

<span data-ttu-id="01ae4-147">構造体の型とは、定数、フィールド、メソッド、プロパティ、インデクサー、演算子、インスタンス コンス トラクター、静的コンス トラクター、および入れ子にされた型を宣言する値型です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-147">A struct type is a value type that can declare constants, fields, methods, properties, indexers, operators, instance constructors, static constructors, and nested types.</span></span> <span data-ttu-id="01ae4-148">構造体型の宣言については、「[構造体の宣言](structs.md#struct-declarations)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-148">The declaration of struct types is described in [Struct declarations](structs.md#struct-declarations).</span></span>

### <a name="simple-types"></a><span data-ttu-id="01ae4-149">単純型</span><span class="sxs-lookup"><span data-stu-id="01ae4-149">Simple types</span></span>

<span data-ttu-id="01ae4-150">C# と呼ばれる定義済みの構造体の型のセットを提供する、***単純型***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-150">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="01ae4-151">予約語を使って単純型が識別されますが、これらの予約語は定義済みの構造体の型に対する単なるエイリアス、`System`名前空間では、次の表で説明されているとします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-151">The simple types are identified through reserved words, but these reserved words are simply aliases for predefined struct types in the `System` namespace, as described in the table below.</span></span>


| <span data-ttu-id="01ae4-152">__予約語__</span><span class="sxs-lookup"><span data-stu-id="01ae4-152">__Reserved word__</span></span> | <span data-ttu-id="01ae4-153">__エイリアスの型__</span><span class="sxs-lookup"><span data-stu-id="01ae4-153">__Aliased type__</span></span> |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

<span data-ttu-id="01ae4-154">単純型のエイリアスは、構造体型、ため、すべての単純型はメンバーがあります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-154">Because a simple type aliases a struct type, every simple type has members.</span></span> <span data-ttu-id="01ae4-155">たとえば、`int`で宣言されたメンバーが含まれて`System.Int32`から継承したメンバーと`System.Object`と、次のステートメントが許可されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-155">For example, `int` has the members declared in `System.Int32` and the members inherited from `System.Object`, and the following statements are permitted:</span></span>

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

<span data-ttu-id="01ae4-156">単純型は、ある追加の操作を許可している点で、他の構造体型とは異なります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-156">The simple types differ from other struct types in that they permit certain additional operations:</span></span>

*  <span data-ttu-id="01ae4-157">ほとんどの単純型を記述して作成する値を許可する*リテラル*([リテラル](lexical-structure.md#literals))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-157">Most simple types permit values to be created by writing *literals* ([Literals](lexical-structure.md#literals)).</span></span> <span data-ttu-id="01ae4-158">たとえば、`123`型のリテラルは、`int`と`'a'`型のリテラルは、`char`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-158">For example, `123` is a literal of type `int` and `'a'` is a literal of type `char`.</span></span> <span data-ttu-id="01ae4-159">C# の場合は、構造体の型のリテラルをプロビジョニングする一般に、他の構造体の型の既定以外の値が常にこれらの構造体の型のインスタンス コンス トラクターを通じて作成最終的に。</span><span class="sxs-lookup"><span data-stu-id="01ae4-159">C# makes no provision for literals of struct types in general, and non-default values of other struct types are ultimately always created through instance constructors of those struct types.</span></span>
*  <span data-ttu-id="01ae4-160">式のオペランドがすべて単純型の定数の場合は、コンパイラはコンパイル時に式の評価になります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-160">When the operands of an expression are all simple type constants, it is possible for the compiler to evaluate the expression at compile-time.</span></span> <span data-ttu-id="01ae4-161">このような式と呼ばれる、 *constant_expression* ([定数式](expressions.md#constant-expressions))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-161">Such an expression is known as a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)).</span></span> <span data-ttu-id="01ae4-162">その他の構造体の型で定義された演算子を含む式は定数式と見なされません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-162">Expressions involving operators defined by other struct types are not considered to be constant expressions.</span></span>
*  <span data-ttu-id="01ae4-163">を通じて`const`単純型の定数を宣言することは、宣言 ([定数](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-163">Through `const` declarations it is possible to declare constants of the simple types ([Constants](classes.md#constants)).</span></span> <span data-ttu-id="01ae4-164">その他の構造体の型の定数を持つことはありませんが、同様の効果がによって提供される`static readonly`フィールド。</span><span class="sxs-lookup"><span data-stu-id="01ae4-164">It is not possible to have constants of other struct types, but a similar effect is provided by `static readonly` fields.</span></span>
*  <span data-ttu-id="01ae4-165">単純型を使用する変換は、他の構造体型で定義された変換演算子の評価に参加できますが、ユーザー定義変換演算子がもう 1 つのユーザー定義演算子の評価に参加できることはありません ([の評価ユーザー定義の変換](conversions.md#evaluation-of-user-defined-conversions))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-165">Conversions involving simple types can participate in evaluation of conversion operators defined by other struct types, but a user-defined conversion operator can never participate in evaluation of another user-defined operator ([Evaluation of user-defined conversions](conversions.md#evaluation-of-user-defined-conversions)).</span></span>

### <a name="integral-types"></a><span data-ttu-id="01ae4-166">整数型</span><span class="sxs-lookup"><span data-stu-id="01ae4-166">Integral types</span></span>

<span data-ttu-id="01ae4-167">C# では、9 つの整数型: `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、および`char`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-167">C# supports nine integral types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, and `char`.</span></span> <span data-ttu-id="01ae4-168">整数型では、次のサイズと値の範囲があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-168">The integral types have the following sizes and ranges of values:</span></span>

*  <span data-ttu-id="01ae4-169">`sbyte`符号付き 8 ビット整数-128 から 127 まで値の型を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-169">The `sbyte` type represents signed 8-bit integers with values between -128 and 127.</span></span>
*  <span data-ttu-id="01ae4-170">`byte`型が符号なし 8 ビット整数 0 ~ 255 の範囲の値を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-170">The `byte` type represents unsigned 8-bit integers with values between 0 and 255.</span></span>
*  <span data-ttu-id="01ae4-171">`short`署名された-32768 から 32767 の値を 16 ビット整数の型を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-171">The `short` type represents signed 16-bit integers with values between -32768 and 32767.</span></span>
*  <span data-ttu-id="01ae4-172">`ushort`型が符号なし 16 ビット整数 0 ~ 65535 の値を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-172">The `ushort` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span>
*  <span data-ttu-id="01ae4-173">`int`符号付き 32 ビット整数-2147483648 から 2147483647 までの値の型を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-173">The `int` type represents signed 32-bit integers with values between -2147483648 and 2147483647.</span></span>
*  <span data-ttu-id="01ae4-174">`uint`型は、0 ~ 4294967295 の値の符号なし 32 ビット整数を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-174">The `uint` type represents unsigned 32-bit integers with values between 0 and 4294967295.</span></span>
*  <span data-ttu-id="01ae4-175">`long`符号付き 64 ビット整数-9223372036854775808 から 9223372036854775807 まで値の型を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-175">The `long` type represents signed 64-bit integers with values between -9223372036854775808 and 9223372036854775807.</span></span>
*  <span data-ttu-id="01ae4-176">`ulong`型が符号なし 64 ビット整数 0 から 18446744073709551615 までの値を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-176">The `ulong` type represents unsigned 64-bit integers with values between 0 and 18446744073709551615.</span></span>
*  <span data-ttu-id="01ae4-177">`char`型が符号なし 16 ビット整数 0 ~ 65535 の値を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-177">The `char` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span> <span data-ttu-id="01ae4-178">使用できる値のセット、`char`種類は、Unicode 文字セットに対応します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-178">The set of possible values for the `char` type corresponds to the Unicode character set.</span></span> <span data-ttu-id="01ae4-179">`char`と同じ表現を持つ`ushort`、他の 1 つの型で許可されているすべての操作が許可されています。</span><span class="sxs-lookup"><span data-stu-id="01ae4-179">Although `char` has the same representation as `ushort`, not all operations permitted on one type are permitted on the other.</span></span>

<span data-ttu-id="01ae4-180">整数型の単項および二項演算子は、常に符号付き 32 ビットの有効桁数、符号なし 32 ビットの有効桁数、符号付き 64 ビットの有効桁数、または符号なし 64 ビットの精度の動作します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-180">The integral-type unary and binary operators always operate with signed 32-bit precision, unsigned 32-bit precision, signed 64-bit precision, or unsigned 64-bit precision:</span></span>

*  <span data-ttu-id="01ae4-181">単項`+`と`~`演算子、オペランドを型に変換されます`T`ここで、`T`の最初の`int`、 `uint`、 `long`、および`ulong`すべてを完全にはオペランドの値。</span><span class="sxs-lookup"><span data-stu-id="01ae4-181">For the unary `+` and `~` operators, the operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="01ae4-182">操作が型の有効桁数を使用して実行し、 `T`、および結果の型は`T`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-182">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>
*  <span data-ttu-id="01ae4-183">単項`-`演算子、オペランドを型に変換されます`T`ここで、`T`の最初の`int`と`long`オペランドのすべての値を表す完全ことができます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-183">For the unary `-` operator, the operand is converted to type `T`, where `T` is the first of `int` and `long` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="01ae4-184">操作が型の有効桁数を使用して実行し、 `T`、および結果の型は`T`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-184">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span> <span data-ttu-id="01ae4-185">単項`-`演算子は、型のオペランドに適用することはできません`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-185">The unary `-` operator cannot be applied to operands of type `ulong`.</span></span>
*  <span data-ttu-id="01ae4-186">バイナリの`+`、 `-`、 `*`、 `/`、 `%`、 `&`、 `^`、 `|`、 `==`、 `!=`、 `>`、 `<`、 `>=`、および`<=`演算子、オペランドを型に変換されます`T`ここで、`T`の最初の`int`、 `uint`、 `long`、および`ulong`すべてを表す完全両方のオペランドの値。</span><span class="sxs-lookup"><span data-stu-id="01ae4-186">For the binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, and `<=` operators, the operands are converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of both operands.</span></span> <span data-ttu-id="01ae4-187">操作が型の有効桁数を使用して実行し、 `T`、および結果の型は`T`(または`bool`関係演算子)。</span><span class="sxs-lookup"><span data-stu-id="01ae4-187">The operation is then performed using the precision of type `T`, and the type of the result is `T` (or `bool` for the relational operators).</span></span> <span data-ttu-id="01ae4-188">1 つのオペランドの型は許可されていません`long`およびその他の型`ulong`二項演算子でします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-188">It is not permitted for one operand to be of type `long` and the other to be of type `ulong` with the binary operators.</span></span>
*  <span data-ttu-id="01ae4-189">バイナリの`<<`と`>>`演算子、左のオペランドを型に変換されます`T`ここで、`T`の最初の`int`、 `uint`、 `long`、および`ulong`すべてを完全にはオペランドの値。</span><span class="sxs-lookup"><span data-stu-id="01ae4-189">For the binary `<<` and `>>` operators, the left operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="01ae4-190">操作が型の有効桁数を使用して実行し、 `T`、および結果の型は`T`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-190">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>

<span data-ttu-id="01ae4-191">`char`型が整数型に分類されますが、2 つの方法で他の整数型とは異なります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-191">The `char` type is classified as an integral type, but it differs from the other integral types in two ways:</span></span>

*  <span data-ttu-id="01ae4-192">他の型から暗黙的な変換がない、`char`型。</span><span class="sxs-lookup"><span data-stu-id="01ae4-192">There are no implicit conversions from other types to the `char` type.</span></span> <span data-ttu-id="01ae4-193">具体的には、場合でも、 `sbyte`、 `byte`、および`ushort`型を使用して完全に表現できる値の範囲がある、`char`から暗黙的な変換を入力します`sbyte`、 `byte`、または`ushort`に。`char`はありません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-193">In particular, even though the `sbyte`, `byte`, and `ushort` types have ranges of values that are fully representable using the `char` type, implicit conversions from `sbyte`, `byte`, or `ushort` to `char` do not exist.</span></span>
*  <span data-ttu-id="01ae4-194">定数、`char`として型を記述する必要があります*character_literal*s または*integer_literal*型へのキャストと組み合わせて s`char`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-194">Constants of the `char` type must be written as *character_literal*s or as *integer_literal*s in combination with a cast to type `char`.</span></span> <span data-ttu-id="01ae4-195">たとえば、`(char)10` は `'\x000A'` と同じです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-195">For example, `(char)10` is the same as `'\x000A'`.</span></span>

<span data-ttu-id="01ae4-196">`checked`と`unchecked`演算子とステートメントは、整数型の算術演算および変換に対するオーバーフロー チェックの制御に使用されます ([checked と unchecked 演算子](expressions.md#the-checked-and-unchecked-operators))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-196">The `checked` and `unchecked` operators and statements are used to control overflow checking for integral-type arithmetic operations and conversions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span> <span data-ttu-id="01ae4-197">`checked`コンテキスト、オーバーフローがコンパイル時エラーを生成またはにより、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-197">In a `checked` context, an overflow produces a compile-time error or causes a `System.OverflowException` to be thrown.</span></span> <span data-ttu-id="01ae4-198">`unchecked`コンテキストはオーバーフローは無視され、変換先の型に適合しない任意の上位ビットは破棄されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-198">In an `unchecked` context, overflows are ignored and any high-order bits that do not fit in the destination type are discarded.</span></span>

### <a name="floating-point-types"></a><span data-ttu-id="01ae4-199">浮動小数点型</span><span class="sxs-lookup"><span data-stu-id="01ae4-199">Floating point types</span></span>

<span data-ttu-id="01ae4-200">C# には、2 つがサポートしている浮動小数点型:`float`と`double`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-200">C# supports two floating point types: `float` and `double`.</span></span> <span data-ttu-id="01ae4-201">`float`と`double`形式を使用して、32 ビット単精度と 64 ビット倍精度 IEEE 754、次の値のセットを提供する型が表されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-201">The `float` and `double` types are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats, which provide the following sets of values:</span></span>

*  <span data-ttu-id="01ae4-202">正および負のゼロ。</span><span class="sxs-lookup"><span data-stu-id="01ae4-202">Positive zero and negative zero.</span></span> <span data-ttu-id="01ae4-203">ほとんどの場合、正のゼロと負のゼロ動作は同じように、単純な値のゼロが区別して 2 つの特定の操作 ([除算演算子](expressions.md#division-operator))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-203">In most situations, positive zero and negative zero behave identically as the simple value zero, but certain operations distinguish between the two ([Division operator](expressions.md#division-operator)).</span></span>
*  <span data-ttu-id="01ae4-204">正の無限大と負の無限大。</span><span class="sxs-lookup"><span data-stu-id="01ae4-204">Positive infinity and negative infinity.</span></span> <span data-ttu-id="01ae4-205">無限大は、0 以外の数値をゼロ除算などの操作によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-205">Infinities are produced by such operations as dividing a non-zero number by zero.</span></span> <span data-ttu-id="01ae4-206">たとえば、`1.0 / 0.0`正の無限大と`-1.0 / 0.0`と負の無限大。</span><span class="sxs-lookup"><span data-stu-id="01ae4-206">For example, `1.0 / 0.0` yields positive infinity, and `-1.0 / 0.0` yields negative infinity.</span></span>
*  <span data-ttu-id="01ae4-207">***Not 非数***値、NaN の多くの場合省略されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-207">The ***Not-a-Number*** value, often abbreviated NaN.</span></span> <span data-ttu-id="01ae4-208">Nan は、0 を 0 除算などの無効な浮動小数点演算によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-208">NaNs are produced by invalid floating-point operations, such as dividing zero by zero.</span></span>
*  <span data-ttu-id="01ae4-209">フォームの値が 0 以外の有限のセット`s * m * 2^e`ここで、 `s` 1 または-1 の場合と`m`と`e`は、特定の浮動小数点型によって決まります。`float`、`0 < m < 2^24`と`-149 <= e <= 104`、および`double`、`0 < m < 2^53`と`1075 <= e <= 970`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-209">The finite set of non-zero values of the form `s * m * 2^e`, where `s` is 1 or -1, and `m` and `e` are determined by the particular floating-point type: For `float`, `0 < m < 2^24` and `-149 <= e <= 104`, and for `double`, `0 < m < 2^53` and `1075 <= e <= 970`.</span></span> <span data-ttu-id="01ae4-210">非正規化浮動小数点数は、有効な値が 0 以外と見なされます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-210">Denormalized floating-point numbers are considered valid non-zero values.</span></span>

<span data-ttu-id="01ae4-211">`float`型が約範囲の値を表す`1.5 * 10^-45`に`3.4 * 10^38`7 桁の有効桁数を持つ。</span><span class="sxs-lookup"><span data-stu-id="01ae4-211">The `float` type can represent values ranging from approximately `1.5 * 10^-45` to `3.4 * 10^38` with a precision of 7 digits.</span></span>

<span data-ttu-id="01ae4-212">`double`型が約範囲の値を表す`5.0 * 10^-324`に`1.7 × 10^308`15 ~ 16 桁の有効桁数を持つ。</span><span class="sxs-lookup"><span data-stu-id="01ae4-212">The `double` type can represent values ranging from approximately `5.0 * 10^-324` to `1.7 × 10^308` with a precision of 15-16 digits.</span></span>

<span data-ttu-id="01ae4-213">二項演算子のオペランドの 1 つの浮動小数点型の場合、し、もう一方のオペランドが整数型または浮動小数点型のある必要があり、操作は次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-213">If one of the operands of a binary operator is of a floating-point type, then the other operand must be of an integral type or a floating-point type, and the operation is evaluated as follows:</span></span>

*  <span data-ttu-id="01ae4-214">整数型のオペランドのいずれかの場合は、そのオペランドがもう一方のオペランドの浮動小数点型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-214">If one of the operands is of an integral type, then that operand is converted to the floating-point type of the other operand.</span></span>
*  <span data-ttu-id="01ae4-215">型のオペランドのいずれかの場合は、 `double`、もう一方のオペランドに変換`double`、少なくともを使用して、操作を実行`double`範囲と有効桁数、および結果の型は`double`(または`bool`の関係演算子)。</span><span class="sxs-lookup"><span data-stu-id="01ae4-215">Then, if either of the operands is of type `double`, the other operand is converted to `double`, the operation is performed using at least `double` range and precision, and the type of the result is `double` (or `bool` for the relational operators).</span></span>
*  <span data-ttu-id="01ae4-216">それ以外の場合、操作を使用して実行少なくとも`float`範囲と有効桁数、および結果の型は`float`(または`bool`関係演算子)。</span><span class="sxs-lookup"><span data-stu-id="01ae4-216">Otherwise, the operation is performed using at least `float` range and precision, and the type of the result is `float` (or `bool` for the relational operators).</span></span>

<span data-ttu-id="01ae4-217">浮動小数点の演算子、代入演算子を含む例外が発生することはありません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-217">The floating-point operators, including the assignment operators, never produce exceptions.</span></span> <span data-ttu-id="01ae4-218">代わりに、例外的な状況は、浮動小数点演算が生成 0、無限大、または NaN の場合、以下に示すよう。</span><span class="sxs-lookup"><span data-stu-id="01ae4-218">Instead, in exceptional situations, floating-point operations produce zero, infinity, or NaN, as described below:</span></span>

*  <span data-ttu-id="01ae4-219">浮動小数点演算の結果が変換先の形式に対して小さすぎる場合は、操作の結果は正の値 0 になるか、負のゼロ。</span><span class="sxs-lookup"><span data-stu-id="01ae4-219">If the result of a floating-point operation is too small for the destination format, the result of the operation becomes positive zero or negative zero.</span></span>
*  <span data-ttu-id="01ae4-220">浮動小数点演算の結果が大きすぎて変換先の形式の場合、操作の結果は正の無限大または負の無限大になります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-220">If the result of a floating-point operation is too large for the destination format, the result of the operation becomes positive infinity or negative infinity.</span></span>
*  <span data-ttu-id="01ae4-221">浮動小数点演算が有効でない場合、操作の結果は NaN になります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-221">If a floating-point operation is invalid, the result of the operation becomes NaN.</span></span>
*  <span data-ttu-id="01ae4-222">浮動小数点演算のオペランドの一方または両方が NaN の場合は、操作の結果は NaN になります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-222">If one or both operands of a floating-point operation is NaN, the result of the operation becomes NaN.</span></span>

<span data-ttu-id="01ae4-223">浮動小数点演算は、操作の結果の型よりも高い精度で実行できます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-223">Floating-point operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="01ae4-224">一部のハードウェア アーキテクチャでの大きい範囲とより有効桁数を持つ「拡張」または"long double"浮動小数点型のサポートなど、 `double` 「」暗黙的にこのより高い有効桁数型を使用してすべての浮動小数点演算を実行します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-224">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `double` type, and implicitly perform all floating-point operations using this higher precision type.</span></span> <span data-ttu-id="01ae4-225">過剰なパフォーマンス コストでのみこのようなハードウェア アーキテクチャにできる、低精度の浮動小数点演算を実行して、パフォーマンスと精度の両方を犠牲にする必要があるのではなく c# を使用するより高い有効桁数の型すべての浮動小数点演算に使用されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-225">Only at excessive cost in performance can such hardware architectures be made to perform floating-point operations with less precision, and rather than require an implementation to forfeit both performance and precision, C# allows a higher precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="01ae4-226">正確な結果を提供する以外はこのことはほとんどありません、測定可能な影響を与えます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-226">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="01ae4-227">ただし、フォームの式で`x * y / z`乗算が外部にある結果を生成します、`double`範囲が、後続の除算に一時的な結果を表示、`double`式であるという事実の範囲評価上の範囲で形式、無限値ではなく生成する有限の結果が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-227">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `double` range, but the subsequent division brings the temporary result back into the `double` range, the fact that the expression is evaluated in a higher range format may cause a finite result to be produced instead of an infinity.</span></span>

### <a name="the-decimal-type"></a><span data-ttu-id="01ae4-228">10 進数型</span><span class="sxs-lookup"><span data-stu-id="01ae4-228">The decimal type</span></span>

<span data-ttu-id="01ae4-229">`decimal` 型は 128 ビットのデータ型で、財務や通貨の計算に適しています。</span><span class="sxs-lookup"><span data-stu-id="01ae4-229">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span> <span data-ttu-id="01ae4-230">`decimal`型は、範囲の値を表すことができます`1.0 * 10^-28`に約`7.9 * 10^28`28 ~ 29 桁を使用します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-230">The `decimal` type can represent values ranging from `1.0 * 10^-28` to approximately `7.9 * 10^28` with 28-29 significant digits.</span></span>

<span data-ttu-id="01ae4-231">型の値の有限のセット`decimal`の形式は`(-1)^s * c * 10^-e`ここで、符号`s`が 0 または 1、係数`c`で指定されます`0 <= *c* < 2^96`、および小数点以下桁数`e`はように`0 <= e <= 28`します。`decimal`符号付きのゼロ、無限大、または NaN の型をサポートしません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-231">The finite set of values of type `decimal` are of the form `(-1)^s * c * 10^-e`, where the sign `s` is 0 or 1, the coefficient `c` is given by `0 <= *c* < 2^96`, and the scale `e` is such that `0 <= e <= 28`.The `decimal` type does not support signed zeros, infinities, or NaN's.</span></span> <span data-ttu-id="01ae4-232">A`decimal`は 10 の累乗によってスケーリング 96 ビット整数として表されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-232">A `decimal` is represented as a 96-bit integer scaled by a power of ten.</span></span> <span data-ttu-id="01ae4-233">`decimal`絶対値を使用してより小さい`1.0m`値は 28 小数点に正確ながありません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-233">For `decimal`s with an absolute value less than `1.0m`, the value is exact to the 28th decimal place, but no further.</span></span> <span data-ttu-id="01ae4-234">`decimal`絶対値より大きいまたは等しいを`1.0m`値が 28 または 29 桁までは正確です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-234">For `decimal`s with an absolute value greater than or equal to `1.0m`, the value is exact to 28 or 29 digits.</span></span> <span data-ttu-id="01ae4-235">Contrary、`float`と`double`データ型、0.1 などの 10 進数の小数値で正確に表すことができます、`decimal`表現。</span><span class="sxs-lookup"><span data-stu-id="01ae4-235">Contrary to the `float` and `double` data types, decimal fractional numbers such as 0.1 can be represented exactly in the `decimal` representation.</span></span> <span data-ttu-id="01ae4-236">`float`と`double`表現、このような数値は多くの場合、無限の小数できるため、表現の丸めを受けやすいエラー。</span><span class="sxs-lookup"><span data-stu-id="01ae4-236">In the `float` and `double` representations, such numbers are often infinite fractions, making those representations more prone to round-off errors.</span></span>

<span data-ttu-id="01ae4-237">型のかどうかは、二項演算子のオペランドの 1 つ`decimal`、もう一方のオペランドが整数型または型にする必要があります`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-237">If one of the operands of a binary operator is of type `decimal`, then the other operand must be of an integral type or of type `decimal`.</span></span> <span data-ttu-id="01ae4-238">整数型のオペランドが存在する場合に変換されます`decimal`操作を実行する前にします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-238">If an integral type operand is present, it is converted to `decimal` before the operation is performed.</span></span>

<span data-ttu-id="01ae4-239">型の値に演算の結果`decimal`から正確な結果 (維持拡張、オペレーターごとに定義されている) を計算して、表現に合わせて丸め処理し、なることができます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-239">The result of an operation on values of type `decimal` is that which would result from calculating an exact result (preserving scale, as defined for each operator) and then rounding to fit the representation.</span></span> <span data-ttu-id="01ae4-240">結果が丸められる、最も近い表現可能な値は、し、結果が均等に最下位ビットが 0 (これと呼ばれる「銀行型丸め」) である値に 2 つの表現可能な値。</span><span class="sxs-lookup"><span data-stu-id="01ae4-240">Results are rounded to the nearest representable value, and, when a result is equally close to two representable values, to the value that has an even number in the least significant digit position (this is known as "banker's rounding").</span></span> <span data-ttu-id="01ae4-241">0 個の結果は、0 の符号とスケールは 0 を常に持ちます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-241">A zero result always has a sign of 0 and a scale of 0.</span></span>

<span data-ttu-id="01ae4-242">10 進数の算術演算の値以下と等しいかそれよりも場合`5 * 10^-29`絶対値で操作の結果はゼロになります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-242">If a decimal arithmetic operation produces a value less than or equal to `5 * 10^-29` in absolute value, the result of the operation becomes zero.</span></span> <span data-ttu-id="01ae4-243">場合、`decimal`算術演算が大きすぎて結果の生成、`decimal`形式、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-243">If a `decimal` arithmetic operation produces a result that is too large for the `decimal` format, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="01ae4-244">`decimal`型がより高い精度浮動小数点型よりも、小さい範囲です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-244">The `decimal` type has greater precision but smaller range than the floating-point types.</span></span> <span data-ttu-id="01ae4-245">浮動小数点型から変換したがって、`decimal`オーバーフロー例外、および変換から生じる可能性があります`decimal`浮動小数点型の有効桁数の損失を伴う可能性があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-245">Thus, conversions from the floating-point types to `decimal` might produce overflow exceptions, and conversions from `decimal` to the floating-point types might cause loss of precision.</span></span> <span data-ttu-id="01ae4-246">これらの理由から、暗黙的な変換が存在しない浮動小数点型の間と`decimal`、明示的なキャストしないことはできません浮動小数点を混在させると、`decimal`同じ式のオペランド。</span><span class="sxs-lookup"><span data-stu-id="01ae4-246">For these reasons, no implicit conversions exist between the floating-point types and `decimal`, and without explicit casts, it is not possible to mix floating-point and `decimal` operands in the same expression.</span></span>

### <a name="the-bool-type"></a><span data-ttu-id="01ae4-247">Bool 型</span><span class="sxs-lookup"><span data-stu-id="01ae4-247">The bool type</span></span>

<span data-ttu-id="01ae4-248">`bool`型がブール論理の数量を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-248">The `bool` type represents boolean logical quantities.</span></span> <span data-ttu-id="01ae4-249">型の有効な値`bool`は`true`と`false`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-249">The possible values of type `bool` are `true` and `false`.</span></span>

<span data-ttu-id="01ae4-250">間の標準変換が存在しない`bool`および他の型。</span><span class="sxs-lookup"><span data-stu-id="01ae4-250">No standard conversions exist between `bool` and other types.</span></span> <span data-ttu-id="01ae4-251">具体的には、`bool`型は別に整数の型から独立して、`bool`整数値では、代わりに、その逆の値は使用できません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-251">In particular, the `bool` type is distinct and separate from the integral types, and a `bool` value cannot be used in place of an integral value, and vice versa.</span></span>

<span data-ttu-id="01ae4-252">C および C++ の言語で、ゼロ整数または浮動小数点の値または null ポインターをブール値に変換できる`false`、0 以外の整数または浮動小数点値または null 以外のポインターは、ブール値に変換できると`true`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-252">In the C and C++ languages, a zero integral or floating-point value, or a null pointer can be converted to the boolean value `false`, and a non-zero integral or floating-point value, or a non-null pointer can be converted to the boolean value `true`.</span></span> <span data-ttu-id="01ae4-253">C# で、このような変換を 0、整数または浮動小数点値を明示的に比較することで、または明示的にオブジェクト参照を比較することで行われます`null`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-253">In C#, such conversions are accomplished by explicitly comparing an integral or floating-point value to zero, or by explicitly comparing an object reference to `null`.</span></span>

### <a name="enumeration-types"></a><span data-ttu-id="01ae4-254">列挙型</span><span class="sxs-lookup"><span data-stu-id="01ae4-254">Enumeration types</span></span>

<span data-ttu-id="01ae4-255">列挙型は、名前付き定数を使用して別個の型です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-255">An enumeration type is a distinct type with named constants.</span></span> <span data-ttu-id="01ae4-256">すべての列挙型がある必要があります、基になる型`byte`、 `sbyte`、 `short`、 `ushort`、 `int`、 `uint`、`long`または`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-256">Every enumeration type has an underlying type, which must be `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="01ae4-257">列挙型の値のセットでは、基になる型の値のセットと同じです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-257">The set of values of the enumeration type is the same as the set of values of the underlying type.</span></span> <span data-ttu-id="01ae4-258">列挙型の値では、名前付き定数の値に限定されません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-258">Values of the enumeration type are not restricted to the values of the named constants.</span></span> <span data-ttu-id="01ae4-259">列挙型が列挙体の宣言で定義されている ([列挙型宣言](enums.md#enum-declarations))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-259">Enumeration types are defined through enumeration declarations ([Enum declarations](enums.md#enum-declarations)).</span></span>

### <a name="nullable-types"></a><span data-ttu-id="01ae4-260">Null 許容型</span><span class="sxs-lookup"><span data-stu-id="01ae4-260">Nullable types</span></span>

<span data-ttu-id="01ae4-261">Null 許容型のすべての値を表すことができます、***基になる型***さらに null 値を追加します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-261">A nullable type can represent all values of its ***underlying type*** plus an additional null value.</span></span> <span data-ttu-id="01ae4-262">Null 許容型が書き込まれる`T?`ここで、`T`は、基になる型です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-262">A nullable type is written `T?`, where `T` is the underlying type.</span></span> <span data-ttu-id="01ae4-263">この構文の短縮形は、 `System.Nullable<T>`、し、2 つの形式は同じ意味で使用されることができます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-263">This syntax is shorthand for `System.Nullable<T>`, and the two forms can be used interchangeably.</span></span>

<span data-ttu-id="01ae4-264">A ***null 非許容値型***以外の任意の値型を逆には`System.Nullable<T>`とその省略`T?`(いずれかの`T`)、さらに (つまり、いずれかの null 非許容値型に制約されている任意の型パラメーターパラメーターの入力を`struct`制約)。</span><span class="sxs-lookup"><span data-stu-id="01ae4-264">A ***non-nullable value type*** conversely is any value type other than `System.Nullable<T>` and its shorthand `T?` (for any `T`), plus any type parameter that is constrained to be a non-nullable value type (that is, any type parameter with a `struct` constraint).</span></span> <span data-ttu-id="01ae4-265">`System.Nullable<T>`型の値の型の制約を指定する`T`([パラメーターの制約入力](classes.md#type-parameter-constraints))、null 許容型の基になる型は任意の null 非許容値型であることを意味します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-265">The `System.Nullable<T>` type specifies the value type constraint for `T` ([Type parameter constraints](classes.md#type-parameter-constraints)), which means that the underlying type of a nullable type can be any non-nullable value type.</span></span> <span data-ttu-id="01ae4-266">Null 許容型の基になる型は、null 許容型または参照型にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-266">The underlying type of a nullable type cannot be a nullable type or a reference type.</span></span> <span data-ttu-id="01ae4-267">たとえば、`int??`と`string?`種類が無効です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-267">For example, `int??` and `string?` are invalid types.</span></span>

<span data-ttu-id="01ae4-268">Null 許容型のインスタンス`T?`は 2 つのパブリックな読み取り専用プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-268">An instance of a nullable type `T?` has two public read-only properties:</span></span>

*  <span data-ttu-id="01ae4-269">A`HasValue`型のプロパティ `bool`</span><span class="sxs-lookup"><span data-stu-id="01ae4-269">A `HasValue` property of type `bool`</span></span>
*  <span data-ttu-id="01ae4-270">A`Value`型のプロパティ `T`</span><span class="sxs-lookup"><span data-stu-id="01ae4-270">A `Value` property of type `T`</span></span>

<span data-ttu-id="01ae4-271">インスタンス`HasValue`が true は null 以外と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-271">An instance for which `HasValue` is true is said to be non-null.</span></span> <span data-ttu-id="01ae4-272">Null 以外のインスタンスには、既知の値が含まれていますと`Value`その値を返します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-272">A non-null instance contains a known value and `Value` returns that value.</span></span>

<span data-ttu-id="01ae4-273">インスタンス`HasValue`は、false は null と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-273">An instance for which `HasValue` is false is said to be null.</span></span> <span data-ttu-id="01ae4-274">Null インスタンスには、未定義の値があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-274">A null instance has an undefined value.</span></span> <span data-ttu-id="01ae4-275">読み取ろうとした、`Value`の null インスタンスにより、`System.InvalidOperationException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-275">Attempting to read the `Value` of a null instance causes a `System.InvalidOperationException` to be thrown.</span></span> <span data-ttu-id="01ae4-276">アクセスするプロセス、 `Value` null 許容型のインスタンスのプロパティと呼びます***ラップ解除***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-276">The process of accessing the `Value` property of a nullable instance is referred to as ***unwrapping***.</span></span>

<span data-ttu-id="01ae4-277">既定のコンス トラクター、すべての null 許容型だけでなく`T?`型の 1 つの引数を受け取るパブリック コンス トラクターを持つ`T`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-277">In addition to the default constructor, every nullable type `T?` has a public constructor that takes a single argument of type `T`.</span></span> <span data-ttu-id="01ae4-278">値が指定される`x`型の`T`フォームのコンス トラクターの呼び出し</span><span class="sxs-lookup"><span data-stu-id="01ae4-278">Given a value `x` of type `T`, a constructor invocation of the form</span></span>

```csharp
new T?(x)
```
<span data-ttu-id="01ae4-279">null 以外のインスタンスを作成します`T?`を`Value`プロパティは`x`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-279">creates a non-null instance of `T?` for which the `Value` property is `x`.</span></span> <span data-ttu-id="01ae4-280">指定した値と呼びますの null 許容型の null 以外のインスタンスを作成するプロセス***ラッピング***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-280">The process of creating a non-null instance of a nullable type for a given value is referred to as ***wrapping***.</span></span>

<span data-ttu-id="01ae4-281">暗黙的な変換はから利用可能な`null`リテラルを`T?`([Null リテラル変換](conversions.md#null-literal-conversions)) との間`T`に`T?`([暗黙的な null 許容変換](conversions.md#implicit-nullable-conversions)).</span><span class="sxs-lookup"><span data-stu-id="01ae4-281">Implicit conversions are available from the `null` literal to `T?` ([Null literal conversions](conversions.md#null-literal-conversions)) and from `T` to `T?` ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)).</span></span>

## <a name="reference-types"></a><span data-ttu-id="01ae4-282">参照型</span><span class="sxs-lookup"><span data-stu-id="01ae4-282">Reference types</span></span>

<span data-ttu-id="01ae4-283">参照型は、クラス型、インターフェイス型、配列型、またはデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-283">A reference type is a class type, an interface type, an array type, or a delegate type.</span></span>

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;

delegate_type
    : type_name
    ;
```

<span data-ttu-id="01ae4-284">参照型の値はへの参照、***インスタンス***として知ら、型の***オブジェクト***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-284">A reference type value is a reference to an ***instance*** of the type, the latter known as an ***object***.</span></span> <span data-ttu-id="01ae4-285">特殊な値`null`すべての参照型と互換性が、インスタンスのないことを示します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-285">The special value `null` is compatible with all reference types and indicates the absence of an instance.</span></span>

### <a name="class-types"></a><span data-ttu-id="01ae4-286">クラス型</span><span class="sxs-lookup"><span data-stu-id="01ae4-286">Class types</span></span>

<span data-ttu-id="01ae4-287">クラス型では、データ メンバー (定数とフィールド)、関数メンバー (メソッド、プロパティ、イベント、インデクサー、演算子、インスタンス コンス トラクター、デストラクター、静的コンス トラクター) と入れ子にされた型を含むデータ構造を定義します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-287">A class type defines a data structure that contains data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="01ae4-288">クラス型では、継承、派生クラスが基底クラスを拡張および限定するためのメカニズムをサポートします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-288">Class types support inheritance, a mechanism whereby derived classes can extend and specialize base classes.</span></span> <span data-ttu-id="01ae4-289">使用してクラス型のインスタンスが作成された*object_creation_expression*s ([オブジェクト作成式](expressions.md#object-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-289">Instances of class types are created using *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>

<span data-ttu-id="01ae4-290">クラス型が記載されて[クラス](classes.md)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-290">Class types are described in [Classes](classes.md).</span></span>

<span data-ttu-id="01ae4-291">特定の種類の定義済みのクラスは、次の表に示すように、c# 言語では、特別な意味を持ちます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-291">Certain predefined class types have special meaning in the C# language, as described in the table below.</span></span>


| <span data-ttu-id="01ae4-292">__クラス型__</span><span class="sxs-lookup"><span data-stu-id="01ae4-292">__Class type__</span></span>     | <span data-ttu-id="01ae4-293">__説明__</span><span class="sxs-lookup"><span data-stu-id="01ae4-293">__Description__</span></span>                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | <span data-ttu-id="01ae4-294">その他のすべての種類の最終的な基本クラス。</span><span class="sxs-lookup"><span data-stu-id="01ae4-294">The ultimate base class of all other types.</span></span> <span data-ttu-id="01ae4-295">参照してください[オブジェクトの種類](types.md#the-object-type)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-295">See [The object type](types.md#the-object-type).</span></span> | 
| `System.String`    | <span data-ttu-id="01ae4-296">C# 言語の文字列型。</span><span class="sxs-lookup"><span data-stu-id="01ae4-296">The string type of the C# language.</span></span> <span data-ttu-id="01ae4-297">参照してください[文字列型](types.md#the-string-type)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-297">See [The string type](types.md#the-string-type).</span></span>         |
| `System.ValueType` | <span data-ttu-id="01ae4-298">すべての値型の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="01ae4-298">The base class of all value types.</span></span> <span data-ttu-id="01ae4-299">参照してください[、System.ValueType 型](types.md#the-systemvaluetype-type)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-299">See [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>          |
| `System.Enum`      | <span data-ttu-id="01ae4-300">すべての列挙型の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="01ae4-300">The base class of all enum types.</span></span> <span data-ttu-id="01ae4-301">参照してください[列挙型](enums.md)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-301">See [Enums](enums.md).</span></span>              |
| `System.Array`     | <span data-ttu-id="01ae4-302">すべての配列型の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="01ae4-302">The base class of all array types.</span></span> <span data-ttu-id="01ae4-303">「[配列](arrays.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="01ae4-303">See [Arrays](arrays.md).</span></span>             |
| `System.Delegate`  | <span data-ttu-id="01ae4-304">すべてのデリゲート型の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="01ae4-304">The base class of all delegate types.</span></span> <span data-ttu-id="01ae4-305">参照してください[デリゲート](delegates.md)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-305">See [Delegates](delegates.md).</span></span>          |
| `System.Exception` | <span data-ttu-id="01ae4-306">すべての例外の種類の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="01ae4-306">The base class of all exception types.</span></span> <span data-ttu-id="01ae4-307">参照してください[例外](exceptions.md)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-307">See [Exceptions](exceptions.md).</span></span>         |

### <a name="the-object-type"></a><span data-ttu-id="01ae4-308">オブジェクトの種類</span><span class="sxs-lookup"><span data-stu-id="01ae4-308">The object type</span></span>

<span data-ttu-id="01ae4-309">`object`クラス型が他のすべての型の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="01ae4-309">The `object` class type is the ultimate base class of all other types.</span></span> <span data-ttu-id="01ae4-310">すべての型 (C#) で直接的または間接的に派生から、`object`クラス型。</span><span class="sxs-lookup"><span data-stu-id="01ae4-310">Every type in C# directly or indirectly derives from the `object` class type.</span></span>

<span data-ttu-id="01ae4-311">キーワード`object`は定義済みのクラスのエイリアスにすぎません`System.Object`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-311">The keyword `object` is simply an alias for the predefined class `System.Object`.</span></span>

### <a name="the-dynamic-type"></a><span data-ttu-id="01ae4-312">動的な型</span><span class="sxs-lookup"><span data-stu-id="01ae4-312">The dynamic type</span></span>

<span data-ttu-id="01ae4-313">`dynamic`入力と、このような`object`、任意のオブジェクトを参照できます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-313">The `dynamic` type, like `object`, can reference any object.</span></span> <span data-ttu-id="01ae4-314">型の式に演算子を適用する`dynamic`、その解決方法は、プログラムが実行されるまでに遅延します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-314">When operators are applied to expressions of type `dynamic`, their resolution is deferred until the program is run.</span></span> <span data-ttu-id="01ae4-315">そのため、オペレーターは、参照先オブジェクトに適用することはできません合法的に場合、エラーを指定しないコンパイル時にします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-315">Thus, if the operator cannot legally be applied to the referenced object, no error is given during compilation.</span></span> <span data-ttu-id="01ae4-316">代わりに実行時に、演算子の解決が失敗したときに例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-316">Instead an exception will be thrown when resolution of the operator fails at run-time.</span></span>

<span data-ttu-id="01ae4-317">その目的はで詳しく説明されている、動的バインドを許可するのには、[動的バインド](expressions.md#dynamic-binding)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-317">Its purpose is to allow dynamic binding, which is described in detail in [Dynamic binding](expressions.md#dynamic-binding).</span></span>

<span data-ttu-id="01ae4-318">`dynamic` 同じと見なされます`object`次の点を除きます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-318">`dynamic` is considered identical to `object` except in the following respects:</span></span>

*  <span data-ttu-id="01ae4-319">型の式に対して操作`dynamic`動的にバインドすることができます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-319">Operations on expressions of type `dynamic` can be dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span>
*  <span data-ttu-id="01ae4-320">型の推論 ([型推論](expressions.md#type-inference)) が優先されます`dynamic`経由で`object`両方の候補にする場合。</span><span class="sxs-lookup"><span data-stu-id="01ae4-320">Type inference ([Type inference](expressions.md#type-inference)) will prefer `dynamic` over `object` if both are candidates.</span></span>

<span data-ttu-id="01ae4-321">この同等性のため、次の処理が保持されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-321">Because of this equivalence, the following holds:</span></span>

*  <span data-ttu-id="01ae4-322">間の暗黙的な id 変換が`object`と`dynamic`、および置換するときに、同じ構築された型の間で`dynamic`で `object`</span><span class="sxs-lookup"><span data-stu-id="01ae4-322">There is an implicit identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing `dynamic` with `object`</span></span>
*  <span data-ttu-id="01ae4-323">明示的および暗黙的な変換との間`object`との間にも適用`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-323">Implicit and explicit conversions to and from `object` also apply to and from `dynamic`.</span></span>
*  <span data-ttu-id="01ae4-324">置換するときに、同じメソッド シグネチャ`dynamic`で`object`は同じシグネチャと見なされます</span><span class="sxs-lookup"><span data-stu-id="01ae4-324">Method signatures that are the same when replacing `dynamic` with `object` are considered the same signature</span></span>
*  <span data-ttu-id="01ae4-325">型`dynamic`は区別できません`object`実行時にします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-325">The type `dynamic` is indistinguishable from `object` at run-time.</span></span>
*  <span data-ttu-id="01ae4-326">型の式`dynamic`と呼ばれますが、***動的な式***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-326">An expression of the type `dynamic` is referred to as a ***dynamic expression***.</span></span>

### <a name="the-string-type"></a><span data-ttu-id="01ae4-327">文字列型</span><span class="sxs-lookup"><span data-stu-id="01ae4-327">The string type</span></span>

<span data-ttu-id="01ae4-328">`string`型がシール クラス型から直接継承される`object`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-328">The `string` type is a sealed class type that inherits directly from `object`.</span></span> <span data-ttu-id="01ae4-329">インスタンス、`string`クラスは Unicode 文字の文字列を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-329">Instances of the `string` class represent Unicode character strings.</span></span>

<span data-ttu-id="01ae4-330">値、`string`型は文字列リテラルとして記述できます ([文字列リテラル](lexical-structure.md#string-literals))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-330">Values of the `string` type can be written as string literals ([String literals](lexical-structure.md#string-literals)).</span></span>

<span data-ttu-id="01ae4-331">キーワード`string`は定義済みのクラスのエイリアスにすぎません`System.String`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-331">The keyword `string` is simply an alias for the predefined class `System.String`.</span></span>

### <a name="interface-types"></a><span data-ttu-id="01ae4-332">インターフェイス型</span><span class="sxs-lookup"><span data-stu-id="01ae4-332">Interface types</span></span>

<span data-ttu-id="01ae4-333">インターフェイスは、コントラクトを定義します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-333">An interface defines a contract.</span></span> <span data-ttu-id="01ae4-334">クラスまたはインターフェイスを実装する構造体は、そのコントラクトに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-334">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="01ae4-335">複数の基底インターフェイスから継承でき、クラスまたは構造体には、複数のインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-335">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="01ae4-336">インターフェイスの種類が記載されて[インターフェイス](interfaces.md)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-336">Interface types are described in [Interfaces](interfaces.md).</span></span>

### <a name="array-types"></a><span data-ttu-id="01ae4-337">配列型</span><span class="sxs-lookup"><span data-stu-id="01ae4-337">Array types</span></span>

<span data-ttu-id="01ae4-338">配列は、算出されたインデックスを介してアクセスされる 0 個以上の変数を含むデータ構造です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-338">An array is a data structure that contains zero or more variables which are accessed through computed indices.</span></span> <span data-ttu-id="01ae4-339">配列の要素とも呼ばれます。 配列に含まれる変数はすべて、同じ型と、この型には、配列の要素の型が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-339">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="01ae4-340">配列型が記載されて[配列](arrays.md)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-340">Array types are described in [Arrays](arrays.md).</span></span>

### <a name="delegate-types"></a><span data-ttu-id="01ae4-341">デリゲート型</span><span class="sxs-lookup"><span data-stu-id="01ae4-341">Delegate types</span></span>

<span data-ttu-id="01ae4-342">デリゲートは、1 つまたは複数のメソッドを参照するデータ構造です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-342">A delegate is a data structure that refers to one or more methods.</span></span> <span data-ttu-id="01ae4-343">インスタンスのメソッドも参照、対応するオブジェクトのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="01ae4-343">For instance methods, it also refers to their corresponding object instances.</span></span>

<span data-ttu-id="01ae4-344">C または C++ では、デリゲートの最も近いは、関数ポインターが関数ポインターは、静的関数を参照できるのみ、一方デリゲートは両方の静的な参照しインスタンス メソッド。</span><span class="sxs-lookup"><span data-stu-id="01ae4-344">The closest equivalent of a delegate in C or C++ is a function pointer, but whereas a function pointer can only reference static functions, a delegate can reference both static and instance methods.</span></span> <span data-ttu-id="01ae4-345">後者の場合、デリゲートは、メソッドのエントリ ポイントへの参照だけでなく、メソッドの呼び出し元となるオブジェクトのインスタンスへの参照を格納します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-345">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance on which to invoke the method.</span></span>

<span data-ttu-id="01ae4-346">デリゲート型が記載されて[デリゲート](delegates.md)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-346">Delegate types are described in [Delegates](delegates.md).</span></span>

## <a name="boxing-and-unboxing"></a><span data-ttu-id="01ae4-347">ボックス化とボックス化解除</span><span class="sxs-lookup"><span data-stu-id="01ae4-347">Boxing and unboxing</span></span>

<span data-ttu-id="01ae4-348">ボックス化とボックス化解除の概念が中心となるC#のシステムを入力します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-348">The concept of boxing and unboxing is central to C#'s type system.</span></span> <span data-ttu-id="01ae4-349">間のブリッジを提供*value_type*s と*reference_type*s のすべての値を許可、 *value_type*型との間に変換する`object`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-349">It provides a bridge between *value_type*s and *reference_type*s by permitting any value of a *value_type* to be converted to and from type `object`.</span></span> <span data-ttu-id="01ae4-350">ボックス化とボックス化解除は、任意の型の値を最終的として処理できるオブジェクトの型システムの統合ビューを使用できます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-350">Boxing and unboxing enables a unified view of the type system wherein a value of any type can ultimately be treated as an object.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="01ae4-351">ボックス化変換</span><span class="sxs-lookup"><span data-stu-id="01ae4-351">Boxing conversions</span></span>

<span data-ttu-id="01ae4-352">ボックス化変換では、 *value_type*に暗黙的に変換する、 *reference_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-352">A boxing conversion permits a *value_type* to be implicitly converted to a *reference_type*.</span></span> <span data-ttu-id="01ae4-353">次のボックス化変換は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-353">The following boxing conversions exist:</span></span>

*  <span data-ttu-id="01ae4-354">いずれかから*value_type*型に`object`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-354">From any *value_type* to the type `object`.</span></span>
*  <span data-ttu-id="01ae4-355">いずれかから*value_type*型に`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-355">From any *value_type* to the type `System.ValueType`.</span></span>
*  <span data-ttu-id="01ae4-356">いずれかから*non_nullable_value_type*いずれかに*interface_type*によって実装される、 *value_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-356">From any *non_nullable_value_type* to any *interface_type* implemented by the *value_type*.</span></span>
*  <span data-ttu-id="01ae4-357">いずれかから*nullable_type*いずれかに*interface_type*の基になる型によって実装される、 *nullable_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-357">From any *nullable_type* to any *interface_type* implemented by the underlying type of the *nullable_type*.</span></span>
*  <span data-ttu-id="01ae4-358">いずれかから*enum_type*型に`System.Enum`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-358">From any *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="01ae4-359">いずれかから*nullable_type*基になると*enum_type*型に`System.Enum`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-359">From any *nullable_type* with an underlying *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="01ae4-360">実行時に最終的には値型から参照型に変換する場合に、型パラメーターからの暗黙的な変換、ボックス化変換として実行されることに注意してください ([型パラメーターを使用する暗黙的な変換](conversions.md#implicit-conversions-involving-type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-360">Note that an implicit conversion from a type parameter will be executed as a boxing conversion if at run-time it ends up converting from a value type to a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span>

<span data-ttu-id="01ae4-361">値をボックス化、 *non_nullable_value_type*オブジェクト インスタンスの割り当てとコピーから成る、 *non_nullable_value_type*値をそのインスタンスにします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-361">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *non_nullable_value_type* value into that instance.</span></span>

<span data-ttu-id="01ae4-362">値をボックス化、 *nullable_type*である場合は、null 参照を生成、`null`値 (`HasValue`は`false`)、またはラップの解除とそれ以外の場合、基になる値をボックス化の結果。</span><span class="sxs-lookup"><span data-stu-id="01ae4-362">Boxing a value of a *nullable_type* produces a null reference if it is the `null` value (`HasValue` is `false`), or the result of unwrapping and boxing the underlying value otherwise.</span></span>

<span data-ttu-id="01ae4-363">実際の値をボックス化のプロセスを*non_nullable_value_type*を仮定すると、汎用の存在をわかりやすく説明***ボックス化クラス***、次のように宣言された場合と同様にします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-363">The actual process of boxing a value of a *non_nullable_value_type* is best explained by imagining the existence of a generic ***boxing class***, which behaves as if it were declared as follows:</span></span>

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

<span data-ttu-id="01ae4-364">値のボックス化`v`型の`T`式を実行するようになりましたは`new Box<T>(v)`、型の値として生成されたインスタンスを返すと`object`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-364">Boxing of a value `v` of type `T` now consists of executing the expression `new Box<T>(v)`, and returning the resulting instance as a value of type `object`.</span></span> <span data-ttu-id="01ae4-365">そのため、ステートメント</span><span class="sxs-lookup"><span data-stu-id="01ae4-365">Thus, the statements</span></span>
```csharp
int i = 123;
object box = i;
```
<span data-ttu-id="01ae4-366">概念的に対応しています</span><span class="sxs-lookup"><span data-stu-id="01ae4-366">conceptually correspond to</span></span>
```csharp
int i = 123;
object box = new Box<int>(i);
```

<span data-ttu-id="01ae4-367">ようにボックス化クラス`Box<T>`以上が存在しない実際にボックス化された値の動的な型がクラス型では実際にはありません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-367">A boxing class like `Box<T>` above doesn't actually exist and the dynamic type of a boxed value isn't actually a class type.</span></span> <span data-ttu-id="01ae4-368">代わりに、型のボックス化された値`T`動的な型を持つ`T`を使用して動的な型チェックと、`is`演算子は単に型を参照`T`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-368">Instead, a boxed value of type `T` has the dynamic type `T`, and a dynamic type check using the `is` operator can simply reference type `T`.</span></span> <span data-ttu-id="01ae4-369">例えば以下のようにします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-369">For example,</span></span>
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
<span data-ttu-id="01ae4-370">文字列の出力は"`Box contains an int`"コンソール。</span><span class="sxs-lookup"><span data-stu-id="01ae4-370">will output the string "`Box contains an int`" on the console.</span></span>

<span data-ttu-id="01ae4-371">ボックス化変換では、ボックス化される値のコピーを意味します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-371">A boxing conversion implies making a copy of the value being boxed.</span></span> <span data-ttu-id="01ae4-372">これは、別の変換から、 *reference_type*入力`object`、値引き続き同じインスタンスを参照すると、単に弱い派生型と見なさ`object`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-372">This is different from a conversion of a *reference_type* to type `object`, in which the value continues to reference the same instance and simply is regarded as the less derived type `object`.</span></span> <span data-ttu-id="01ae4-373">たとえば、宣言について考えます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-373">For example, given the declaration</span></span>
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
<span data-ttu-id="01ae4-374">次のステートメント</span><span class="sxs-lookup"><span data-stu-id="01ae4-374">the following statements</span></span>
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
<span data-ttu-id="01ae4-375">に、コンソールで、値 10 が出力の割り当てで発生する暗黙的なボックス化操作`p`に`box`の値と、`p`をコピーします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-375">will output the value 10 on the console because the implicit boxing operation that occurs in the assignment of `p` to `box` causes the value of `p` to be copied.</span></span> <span data-ttu-id="01ae4-376">`Point`されて宣言されている、`class`代わりに、値 20 出力されるため、`p`と`box`同じインスタンスを参照します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-376">Had `Point` been declared a `class` instead, the value 20 would be output because `p` and `box` would reference the same instance.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="01ae4-377">ボックス化解除変換</span><span class="sxs-lookup"><span data-stu-id="01ae4-377">Unboxing conversions</span></span>

<span data-ttu-id="01ae4-378">ボックス化解除の変換では、 *reference_type*に明示的に変換する、 *value_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-378">An unboxing conversion permits a *reference_type* to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="01ae4-379">次のボックス化解除変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-379">The following unboxing conversions exist:</span></span>

*  <span data-ttu-id="01ae4-380">型から`object`いずれかに*value_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-380">From the type `object` to any *value_type*.</span></span>
*  <span data-ttu-id="01ae4-381">型から`System.ValueType`いずれかに*value_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-381">From the type `System.ValueType` to any *value_type*.</span></span>
*  <span data-ttu-id="01ae4-382">いずれかから*interface_type*いずれかに*non_nullable_value_type*を実装する、 *interface_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-382">From any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span>
*  <span data-ttu-id="01ae4-383">いずれかから*interface_type*いずれかに*nullable_type*基になる型を実装して、 *interface_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-383">From any *interface_type* to any *nullable_type* whose underlying type implements the *interface_type*.</span></span>
*  <span data-ttu-id="01ae4-384">型から`System.Enum`いずれかに*enum_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-384">From the type `System.Enum` to any *enum_type*.</span></span>
*  <span data-ttu-id="01ae4-385">型から`System.Enum`いずれかに*nullable_type*基になると*enum_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-385">From the type `System.Enum` to any *nullable_type* with an underlying *enum_type*.</span></span>
*  <span data-ttu-id="01ae4-386">実行時に最終的には、参照型から値型に変換する場合に、型パラメーターに明示的な変換、アンボックス変換として実行されることに注意してください ([動的の明示的な変換](conversions.md#explicit-dynamic-conversions))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-386">Note that an explicit conversion to a type parameter will be executed as an unboxing conversion if at run-time it ends up converting from a reference type to a value type ([Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)).</span></span>

<span data-ttu-id="01ae4-387">ボックス化解除の操作を*non_nullable_value_type*オブジェクト インスタンスのボックス化された値は、最初に確認から成る、指定された*non_nullable_value_type*、しの値をコピーして、インスタンス。</span><span class="sxs-lookup"><span data-stu-id="01ae4-387">An unboxing operation to a *non_nullable_value_type* consists of first checking that the object instance is a boxed value of the given *non_nullable_value_type*, and then copying the value out of the instance.</span></span>

<span data-ttu-id="01ae4-388">ボックス化解除、 *nullable_type*の null 値を生成、 *nullable_type*ソース オペランドの場合`null`、またはオブジェクト インスタンスの基になる型をボックス化解除のラップされた結果、*nullable_type*それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="01ae4-388">Unboxing to a *nullable_type* produces the null value of the *nullable_type* if the source operand is `null`, or the wrapped result of unboxing the object instance to the underlying type of the *nullable_type* otherwise.</span></span>

<span data-ttu-id="01ae4-389">オブジェクトのアンボックス変換、前のセクションで説明されている仮想のボックス化クラスを参照する`box`を*value_type* `T`式を実行から成る`((Box<T>)box).value`。</span><span class="sxs-lookup"><span data-stu-id="01ae4-389">Referring to the imaginary boxing class described in the previous section, an unboxing conversion of an object `box` to a *value_type* `T` consists of executing the expression `((Box<T>)box).value`.</span></span> <span data-ttu-id="01ae4-390">そのため、ステートメント</span><span class="sxs-lookup"><span data-stu-id="01ae4-390">Thus, the statements</span></span>
```csharp
object box = 123;
int i = (int)box;
```
<span data-ttu-id="01ae4-391">概念的に対応しています</span><span class="sxs-lookup"><span data-stu-id="01ae4-391">conceptually correspond to</span></span>
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

<span data-ttu-id="01ae4-392">ボックス化解除変換の指定された*non_nullable_value_type*ソース オペランドの値が実行時に成功するをボックス化された値への参照をある必要があります*non_nullable_value_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-392">For an unboxing conversion to a given *non_nullable_value_type* to succeed at run-time, the value of the source operand must be a reference to a boxed value of that *non_nullable_value_type*.</span></span> <span data-ttu-id="01ae4-393">ソース オペランドの場合`null`、`System.NullReferenceException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-393">If the source operand is `null`, a `System.NullReferenceException` is thrown.</span></span> <span data-ttu-id="01ae4-394">ソース オペランドが、互換性のないオブジェクトへの参照の場合、`System.InvalidCastException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-394">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="01ae4-395">ボックス化解除変換の指定された*nullable_type*に成功すると、実行時に、ソース オペランドの値はいずれかのことがあります`null`または基になるのボックス化された値への参照*non_nullable_value_type*の*nullable_type*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-395">For an unboxing conversion to a given *nullable_type* to succeed at run-time, the value of the source operand must be either `null` or a reference to a boxed value of the underlying *non_nullable_value_type* of the *nullable_type*.</span></span> <span data-ttu-id="01ae4-396">ソース オペランドが、互換性のないオブジェクトへの参照の場合、`System.InvalidCastException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-396">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="01ae4-397">構築された型</span><span class="sxs-lookup"><span data-stu-id="01ae4-397">Constructed types</span></span>

<span data-ttu-id="01ae4-398">ジェネリック型の宣言では、単独で表します、***ジェネリック型にバインドされていない***の適用を使用して、さまざまな種類のフォームに「ブルー プリント」として使用される***引数を入力***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-398">A generic type declaration, by itself, denotes an ***unbound generic type*** that is used as a "blueprint" to form many different types, by way of applying ***type arguments***.</span></span> <span data-ttu-id="01ae4-399">型引数は山かっこ内に書き込まれます (`<`と`>`) すぐに次のジェネリック型の名前。</span><span class="sxs-lookup"><span data-stu-id="01ae4-399">The type arguments are written within angle brackets (`<` and `>`) immediately following the name of the generic type.</span></span> <span data-ttu-id="01ae4-400">少なくとも 1 つの型引数を含む型が呼び出される、***構築型***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-400">A type that includes at least one type argument is called a ***constructed type***.</span></span> <span data-ttu-id="01ae4-401">構築された型は、型名の表示に使用できる言語のほとんどの場所で使用できます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-401">A constructed type can be used in most places in the language in which a type name can appear.</span></span> <span data-ttu-id="01ae4-402">バインドされていないのジェネリック型は内でのみ使用できます、 *typeof_expression* ([typeof 演算子](expressions.md#the-typeof-operator))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-402">An unbound generic type can only be used within a *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

<span data-ttu-id="01ae4-403">単純な名前として式で構築された型は使用もできます ([簡易名](expressions.md#simple-names)) メンバーにアクセスするとき、または ([メンバー アクセス](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-403">Constructed types can also be used in expressions as simple names ([Simple names](expressions.md#simple-names)) or when accessing a member ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="01ae4-404">ときに、 *namespace_or_type_name*と見なされますパラメーター型の正しい数と種類の評価された、汎用のみです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-404">When a *namespace_or_type_name* is evaluated, only generic types with the correct number of type parameters are considered.</span></span> <span data-ttu-id="01ae4-405">そのため、型がある型パラメーターの数が異なる限り、さまざまな種類を識別するために同じ識別子を使用することです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-405">Thus, it is possible to use the same identifier to identify different types, as long as the types have different numbers of type parameters.</span></span> <span data-ttu-id="01ae4-406">これは、機能は、同じプログラム内でジェネリックと非ジェネリックのクラスが混在する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-406">This is useful when mixing generic and non-generic classes in the same program:</span></span>

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

<span data-ttu-id="01ae4-407">A *type_name*型パラメーターを直接指定しない場合でも、構築された型を識別する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-407">A *type_name* might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="01ae4-408">これは、問題は、型がジェネリック クラス宣言の中で入れ子になった、外側の宣言のインスタンスの型が暗黙的に名前参照の使用に発生することができます ([ジェネリック クラスの型を入れ子になった](classes.md#nested-types-in-generic-classes))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-408">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup ([Nested types in generic classes](classes.md#nested-types-in-generic-classes)):</span></span>

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

<span data-ttu-id="01ae4-409">アンセーフ コードで構築された型として使用できません、 *unmanaged_type* ([ポインター型](unsafe-code.md#pointer-types))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-409">In unsafe code, a constructed type cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

### <a name="type-arguments"></a><span data-ttu-id="01ae4-410">型引数</span><span class="sxs-lookup"><span data-stu-id="01ae4-410">Type arguments</span></span>

<span data-ttu-id="01ae4-411">型引数リストの各引数は単に、*型*します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-411">Each argument in a type argument list is simply a *type*.</span></span>

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

<span data-ttu-id="01ae4-412">安全でないコード ([アンセーフ コード](unsafe-code.md))、 *type_argument*ポインター型ができない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-412">In unsafe code ([Unsafe code](unsafe-code.md)), a *type_argument* may not be a pointer type.</span></span> <span data-ttu-id="01ae4-413">各型引数の型パラメーターの対応する、制約を満たす必要があります ([パラメーターの制約入力](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-413">Each type argument must satisfy any constraints on the corresponding type parameter ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>

### <a name="open-and-closed-types"></a><span data-ttu-id="01ae4-414">オープン型</span><span class="sxs-lookup"><span data-stu-id="01ae4-414">Open and closed types</span></span>

<span data-ttu-id="01ae4-415">すべての種類は、いずれかに分類できます***の種類を開く***または***クローズ型***します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-415">All types can be classified as either ***open types*** or ***closed types***.</span></span> <span data-ttu-id="01ae4-416">オープン型は、型パラメーターを含む型です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-416">An open type is a type that involves type parameters.</span></span> <span data-ttu-id="01ae4-417">具体的には。</span><span class="sxs-lookup"><span data-stu-id="01ae4-417">More specifically:</span></span>

*  <span data-ttu-id="01ae4-418">型パラメーターは、オープン型を定義します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-418">A type parameter defines an open type.</span></span>
*  <span data-ttu-id="01ae4-419">要素の型がオープン型場合にのみ、配列型はオープン型が。</span><span class="sxs-lookup"><span data-stu-id="01ae4-419">An array type is an open type if and only if its element type is an open type.</span></span>
*  <span data-ttu-id="01ae4-420">構築された型はオープン型がオープン型の 1 つ以上の型引数の場合にのみ</span><span class="sxs-lookup"><span data-stu-id="01ae4-420">A constructed type is an open type if and only if one or more of its type arguments is an open type.</span></span> <span data-ttu-id="01ae4-421">構築された入れ子にされた型はオープン型がオープン型の 1 つ以上の型引数または外側の型の型引数の場合にのみ</span><span class="sxs-lookup"><span data-stu-id="01ae4-421">A constructed nested type is an open type if and only if one or more of its type arguments or the type arguments of its containing type(s) is an open type.</span></span>

<span data-ttu-id="01ae4-422">閉じている型は、オープン型ではない型です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-422">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="01ae4-423">実行時にには、ジェネリック宣言する型引数を適用することによって作成された構築されたクローズ型のコンテキストでのジェネリック型の宣言内でコードをすべて実行されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-423">At run-time, all of the code within a generic type declaration is executed in the context of a closed constructed type that was created by applying type arguments to the generic declaration.</span></span> <span data-ttu-id="01ae4-424">ジェネリック型内の各型パラメーターは、特定の実行時の型にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-424">Each type parameter within the generic type is bound to a particular run-time type.</span></span> <span data-ttu-id="01ae4-425">すべてのステートメントと式の実行時の処理は、常には、クローズ型が発生し、コンパイル時にのみ発生するオープン型が処理します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-425">The run-time processing of all statements and expressions always occurs with closed types, and open types occur only during compile-time processing.</span></span>

<span data-ttu-id="01ae4-426">構築されたクローズ型をそれぞれが、独自の静的変数は、クローズ構築型と共有されていないセット。</span><span class="sxs-lookup"><span data-stu-id="01ae4-426">Each closed constructed type has its own set of static variables, which are not shared with any other closed constructed types.</span></span> <span data-ttu-id="01ae4-427">実行時にオープン型が存在しないために、オープン型に関連付けられている静的変数はありません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-427">Since an open type does not exist at run-time, there are no static variables associated with an open type.</span></span> <span data-ttu-id="01ae4-428">2 つのクローズ構築型は、同じバインドされていないジェネリック型から構築されると、対応する型引数は、同じ型である場合、同じ型です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-428">Two closed constructed types are the same type if they are constructed from the same unbound generic type, and their corresponding type arguments are the same type.</span></span>

### <a name="bound-and-unbound-types"></a><span data-ttu-id="01ae4-429">バインドとバインドされていない型</span><span class="sxs-lookup"><span data-stu-id="01ae4-429">Bound and unbound types</span></span>

<span data-ttu-id="01ae4-430">用語***型にバインドされていない***非ジェネリック型または非バインドのジェネリック型を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-430">The term ***unbound type*** refers to a non-generic type or an unbound generic type.</span></span> <span data-ttu-id="01ae4-431">用語***型にバインドされている***非ジェネリック型または構築された型を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-431">The term ***bound type*** refers to a non-generic type or a constructed type.</span></span>

<span data-ttu-id="01ae4-432">バインドされていない型は、型宣言で宣言されたエンティティを指します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-432">An unbound type refers to the entity declared by a type declaration.</span></span> <span data-ttu-id="01ae4-433">非バインドのジェネリック型自体ではなく、型と、変数、引数または戻り値の型として、または基本データ型として使用できません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-433">An unbound generic type is not itself a type, and cannot be used as the type of a variable, argument or return value, or as a base type.</span></span> <span data-ttu-id="01ae4-434">バインドされていないジェネリック型を参照できる唯一のコンストラクトを`typeof`式 ([typeof 演算子](expressions.md#the-typeof-operator))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-434">The only construct in which an unbound generic type can be referenced is the `typeof` expression ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

### <a name="satisfying-constraints"></a><span data-ttu-id="01ae4-435">制約の充足</span><span class="sxs-lookup"><span data-stu-id="01ae4-435">Satisfying constraints</span></span>

<span data-ttu-id="01ae4-436">構築された型またはジェネリック メソッドを参照すると、指定された型引数はジェネリック型またはメソッドで宣言された型パラメーターの制約に照らしてチェック ([パラメーターの制約入力](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-436">Whenever a constructed type or generic method is referenced, the supplied type arguments are checked against the type parameter constraints declared on the generic type or method ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="01ae4-437">各`where`句では、型引数`A`名前付きに対応する型パラメーターは、次のように制約に照らしてチェックします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-437">For each `where` clause, the type argument `A` that corresponds to the named type parameter is checked against each constraint as follows:</span></span>

*  <span data-ttu-id="01ae4-438">クラス型、インターフェイス型または型パラメーターの制約である場合、`C`制約に表示される任意の型パラメーターに指定された型引数の制約が置き換えことを表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-438">If the constraint is a class type, an interface type, or a type parameter, let `C` represent that constraint with the supplied type arguments substituted for any type parameters that appear in the constraint.</span></span> <span data-ttu-id="01ae4-439">制約を満たす必要がありますは、型`A`は型に変換`C`次のいずれか。</span><span class="sxs-lookup"><span data-stu-id="01ae4-439">To satisfy the constraint, it must be the case that type `A` is convertible to type `C` by one of the following:</span></span>
    * <span data-ttu-id="01ae4-440">Id 変換 ([恒等変換](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="01ae4-440">An identity conversion ([Identity conversion](conversions.md#identity-conversion))</span></span>
    * <span data-ttu-id="01ae4-441">暗黙の参照変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="01ae4-441">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
    * <span data-ttu-id="01ae4-442">ボックス化変換 ([ボックス化変換](conversions.md#boxing-conversions))、その型 A は、null 非許容値型。</span><span class="sxs-lookup"><span data-stu-id="01ae4-442">A boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)), provided that type A is a non-nullable value type.</span></span>
    * <span data-ttu-id="01ae4-443">型パラメーターから暗黙の参照、ボックス化、または型パラメーター変換`A`に`C`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-443">An implicit reference, boxing or type parameter conversion from a type parameter `A` to `C`.</span></span>
*  <span data-ttu-id="01ae4-444">制約が参照型の制約である場合 (`class`)、型`A`次のいずれかを満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-444">If the constraint is the reference type constraint (`class`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="01ae4-445">`A` インターフェイス型、クラス型、デリゲート型または配列型。</span><span class="sxs-lookup"><span data-stu-id="01ae4-445">`A` is an interface type, class type, delegate type or array type.</span></span> <span data-ttu-id="01ae4-446">なお`System.ValueType`と`System.Enum`はこの制約を満たす参照型です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-446">Note that `System.ValueType` and `System.Enum` are reference types that satisfy this constraint.</span></span>
    * <span data-ttu-id="01ae4-447">`A` 参照型があることがわかっている型パラメーターは、([パラメーターの制約入力](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-447">`A` is a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="01ae4-448">制約が値型の制約である場合 (`struct`)、型`A`次のいずれかを満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-448">If the constraint is the value type constraint (`struct`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="01ae4-449">`A` 構造体の型または列挙型を null 許容型ではなくです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-449">`A` is a struct type or enum type, but not a nullable type.</span></span> <span data-ttu-id="01ae4-450">なお`System.ValueType`と`System.Enum`はこの制約を満たしていない参照型です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-450">Note that `System.ValueType` and `System.Enum` are reference types that do not satisfy this constraint.</span></span>
    * <span data-ttu-id="01ae4-451">`A` 値型の制約を持つ型パラメーター ([パラメーターの制約入力](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-451">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="01ae4-452">制約は、コンス トラクター制約が場合`new()`、型`A`必要があります`abstract`パブリック パラメーターなしのコンス トラクターが必要とします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-452">If the constraint is the constructor constraint `new()`, the type `A` must not be `abstract` and must have a public parameterless constructor.</span></span> <span data-ttu-id="01ae4-453">これが満たされるは、次のいずれかが true の場合。</span><span class="sxs-lookup"><span data-stu-id="01ae4-453">This is satisfied if one of the following is true:</span></span>
    * <span data-ttu-id="01ae4-454">`A` 値の型は、すべての値型がパブリックの既定のコンス トラクターがあるため ([既定のコンス トラクター](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-454">`A` is a value type, since all value types have a public default constructor ([Default constructors](types.md#default-constructors)).</span></span>
    * <span data-ttu-id="01ae4-455">`A` コンス トラクター制約を持つ型パラメーター ([パラメーターの制約入力](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-455">`A` is a type parameter having the constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="01ae4-456">`A` 値型の制約を持つ型パラメーター ([パラメーターの制約入力](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-456">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="01ae4-457">`A` ないクラス`abstract`明示的に宣言が含まれている`public`パラメーターなしコンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="01ae4-457">`A` is a class that is not `abstract` and contains an explicitly declared `public` constructor with no parameters.</span></span>
    * <span data-ttu-id="01ae4-458">`A` ない`abstract`既定のコンス トラクター ([既定のコンス トラクター](classes.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-458">`A` is not `abstract` and has a default constructor ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="01ae4-459">指定された型引数によって 1 つ以上の型パラメーターの制約が満たされない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-459">A compile-time error occurs if one or more of a type parameter's constraints are not satisfied by the given type arguments.</span></span>

<span data-ttu-id="01ae4-460">制約はありませんので、型パラメーターは継承されず、いずれかを継承します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-460">Since type parameters are not inherited, constraints are never inherited either.</span></span> <span data-ttu-id="01ae4-461">次の例で`D`、型パラメーターに制約を指定する必要がある`T`ように`T`基底クラスによって課される制約を満たす`B<T>`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-461">In the example below, `D` needs to specify the constraint on its type parameter `T` so that `T` satisfies the constraint imposed by the base class `B<T>`.</span></span> <span data-ttu-id="01ae4-462">これに対し、クラス`E`ために、制約を指定する必要があります`List<T>`実装`IEnumerable`は`T`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-462">In contrast, class `E` need not specify a constraint, because `List<T>` implements `IEnumerable` for any `T`.</span></span>

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a><span data-ttu-id="01ae4-463">型パラメーター</span><span class="sxs-lookup"><span data-stu-id="01ae4-463">Type parameters</span></span>

<span data-ttu-id="01ae4-464">型パラメーターでは、値型またはパラメーターは、実行時にバインドされている参照型を指定する識別子です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-464">A type parameter is an identifier designating a value type or reference type that the parameter is bound to at run-time.</span></span>

```antlr
type_parameter
    : identifier
    ;
```

<span data-ttu-id="01ae4-465">型パラメーターは、多くのさまざまな実際の型引数でインスタンス化することができます、ために、型パラメーターには、若干異なる操作とその他の型よりも制限があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-465">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types.</span></span> <span data-ttu-id="01ae4-466">不足している機能には次が含まれます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-466">These include:</span></span>

*  <span data-ttu-id="01ae4-467">型パラメーターを使用して直接基底クラスを宣言することはできません ([基本クラス](classes.md#base-class)) またはインターフェイス ([バリアント型パラメーター リスト](interfaces.md#variant-type-parameter-lists))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-467">A type parameter cannot be used directly to declare a base class ([Base class](classes.md#base-class)) or interface ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)).</span></span>
*  <span data-ttu-id="01ae4-468">パラメーターは、存在する場合、制約とは異なります。 型のメンバー検索の規則は、型パラメーターに適用されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-468">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="01ae4-469">詳細については、[メンバー ルックアップ](expressions.md#member-lookup)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-469">They are detailed in [Member lookup](expressions.md#member-lookup).</span></span>
*  <span data-ttu-id="01ae4-470">使用可能な変換型パラメーターは、存在する場合、制約に依存しているは、型パラメーターに適用されます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-470">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="01ae4-471">詳細については、[型パラメーターを使用する暗黙的な変換](conversions.md#implicit-conversions-involving-type-parameters)と[動的の明示的な変換](conversions.md#explicit-dynamic-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-471">They are detailed in [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions).</span></span>
*  <span data-ttu-id="01ae4-472">リテラル`null`型パラメーターが参照型であることがわかっている場合を除き、型パラメーターで指定する型に変換できません ([型パラメーターを使用する暗黙的な変換](conversions.md#implicit-conversions-involving-type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-472">The literal `null` cannot be converted to a type given by a type parameter, except if the type parameter is known to be a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span> <span data-ttu-id="01ae4-473">ただし、`default`式 ([既定の値式](expressions.md#default-value-expressions)) 代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-473">However, a `default` expression ([Default value expressions](expressions.md#default-value-expressions)) can be used instead.</span></span> <span data-ttu-id="01ae4-474">さらに、型パラメーターで指定した型の値と比較できる`null`を使用して`==`と`!=`([参照型の等値演算子](expressions.md#reference-type-equality-operators)) 型パラメーターに値型の制約がない限り、します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-474">In addition, a value with a type given by a type parameter can be compared with `null` using `==` and `!=` ([Reference type equality operators](expressions.md#reference-type-equality-operators)) unless the type parameter has the value type constraint.</span></span>
*  <span data-ttu-id="01ae4-475">A`new`式 ([オブジェクト作成式](expressions.md#object-creation-expressions)) によって、型パラメーターが制約される場合のみの型パラメーターに使用できます、 *constructor_constraint*または値制約 (を入力します。[パラメーターの制約入力](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-475">A `new` expression ([Object creation expressions](expressions.md#object-creation-expressions)) can only be used with a type parameter if the type parameter is constrained by a *constructor_constraint* or the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="01ae4-476">型パラメーターは、属性内の任意の場所で使用できません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-476">A type parameter cannot be used anywhere within an attribute.</span></span>
*  <span data-ttu-id="01ae4-477">メンバー アクセスでは、型パラメーターを使用できません ([メンバー アクセス](expressions.md#member-access)) または型の名前 ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names)) 静的メンバーまたは入れ子にされた型を識別するためにします。</span><span class="sxs-lookup"><span data-stu-id="01ae4-477">A type parameter cannot be used in a member access ([Member access](expressions.md#member-access)) or type name ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) to identify a static member or a nested type.</span></span>
*  <span data-ttu-id="01ae4-478">としてアンセーフ コードは、型パラメーターを使用することはできません、 *unmanaged_type* ([ポインター型](unsafe-code.md#pointer-types))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-478">In unsafe code, a type parameter cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

<span data-ttu-id="01ae4-479">型パラメーター、型では、純粋なコンパイル時の構成要素があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-479">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="01ae4-480">実行時に、それぞれの型パラメーターは、ジェネリック型の宣言する型引数を指定することによって指定された実行時の型にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-480">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic type declaration.</span></span> <span data-ttu-id="01ae4-481">したがって、変数の型を宣言で型パラメーターは、実行時に、クローズ構築型になります ([オープンとクローズ型](types.md#open-and-closed-types))。</span><span class="sxs-lookup"><span data-stu-id="01ae4-481">Thus, the type of a variable declared with a type parameter will, at run-time, be a closed constructed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="01ae4-482">すべての型パラメーターを含む式とステートメントの実行時の実行は、そのパラメーターの型引数として渡された実際の型を使用します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-482">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>

## <a name="expression-tree-types"></a><span data-ttu-id="01ae4-483">式ツリー型</span><span class="sxs-lookup"><span data-stu-id="01ae4-483">Expression tree types</span></span>

<span data-ttu-id="01ae4-484">***式ツリー***実行可能コードではなく、データ構造として表されるラムダ式を許可します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-484">***Expression trees*** permit lambda expressions to be represented as data structures instead of executable code.</span></span> <span data-ttu-id="01ae4-485">式ツリーは値の***式ツリー型***フォームの`System.Linq.Expressions.Expression<D>`ここで、`D`はデリゲート型。</span><span class="sxs-lookup"><span data-stu-id="01ae4-485">Expression trees are values of ***expression tree types*** of the form `System.Linq.Expressions.Expression<D>`, where `D` is any delegate type.</span></span> <span data-ttu-id="01ae4-486">短縮形を使用してこれらの型をこの仕様の残りの部分参照`Expression<D>`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-486">For the remainder of this specification we will refer to these types using the shorthand `Expression<D>`.</span></span>

<span data-ttu-id="01ae4-487">変換が存在するかどうか、ラムダ式からデリゲート型に`D`、変換は、式ツリー型にも存在する`Expression<D>`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-487">If a conversion exists from a lambda expression to a delegate type `D`, a conversion also exists to the expression tree type `Expression<D>`.</span></span> <span data-ttu-id="01ae4-488">ラムダ式をデリゲート型の変換では、ラムダ式の実行可能コードを参照するデリゲートを生成する一方、式ツリー型への変換は、ラムダ式の式ツリー表現を作成します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-488">Whereas the conversion of a lambda expression to a delegate type generates a delegate that references executable code for the lambda expression, conversion to an expression tree type creates an expression tree representation of the lambda expression.</span></span>

<span data-ttu-id="01ae4-489">式ツリーは、ラムダ式の効率的なメモリ内データ表現および透過的と明示的なラムダ式の構造を作成します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-489">Expression trees are efficient in-memory data representations of lambda expressions and make the structure of the lambda expression transparent and explicit.</span></span>

<span data-ttu-id="01ae4-490">デリゲート型と同様に`D`、`Expression<D>`パラメーターと戻り値の型があるといいますのものと同じである`D`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-490">Just like a delegate type `D`, `Expression<D>` is said to have parameter and return types, which are the same as those of `D`.</span></span>

<span data-ttu-id="01ae4-491">次の例では、実行可能コードと、式ツリーの両方に、ラムダ式を表します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-491">The following example represents a lambda expression both as executable code and as an expression tree.</span></span> <span data-ttu-id="01ae4-492">変換が存在するため`Func<int,int>`、変換が存在する`Expression<Func<int,int>>`:</span><span class="sxs-lookup"><span data-stu-id="01ae4-492">Because a conversion exists to `Func<int,int>`, a conversion also exists to `Expression<Func<int,int>>`:</span></span>

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

<span data-ttu-id="01ae4-493">次のデリゲートのこれらの割り当て`del`を返すメソッドを参照`x + 1`、式ツリーと`exp`式を記述するデータ構造を参照`x => x + 1`。</span><span class="sxs-lookup"><span data-stu-id="01ae4-493">Following these assignments, the delegate `del` references a method that returns `x + 1`, and the expression tree `exp` references a data structure that describes the expression `x => x + 1`.</span></span>

<span data-ttu-id="01ae4-494">ジェネリック型の正確な定義`Expression<D>`ラムダ式を式ツリー型に変換する場合は、式ツリーを構築するための正確なルールとは、どちらもこの仕様の範囲外です。</span><span class="sxs-lookup"><span data-stu-id="01ae4-494">The exact definition of the generic type `Expression<D>` as well as the precise rules for constructing an expression tree when a lambda expression is converted to an expression tree type, are both outside the scope of this specification.</span></span>

<span data-ttu-id="01ae4-495">2 つのことを明示する重要なのとおりです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-495">Two things are important to make explicit:</span></span>

*  <span data-ttu-id="01ae4-496">すべてのラムダ式は、式ツリーに変換できます。</span><span class="sxs-lookup"><span data-stu-id="01ae4-496">Not all lambda expressions can be converted to expression trees.</span></span> <span data-ttu-id="01ae4-497">たとえば、ステートメントの本体でラムダ式と代入式を含むラムダ式を表示することはできません。</span><span class="sxs-lookup"><span data-stu-id="01ae4-497">For instance, lambda expressions with statement bodies, and lambda expressions containing assignment expressions cannot be represented.</span></span> <span data-ttu-id="01ae4-498">このような場合は、変換が存在しますはコンパイル時に失敗します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-498">In these cases, a conversion still exists, but will fail at compile-time.</span></span> <span data-ttu-id="01ae4-499">これらの例外の詳細については[匿名関数の変換](conversions.md#anonymous-function-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-499">These exceptions are detailed in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>
*   <span data-ttu-id="01ae4-500">`Expression<D>` インスタンス メソッドを提供しています`Compile`型のデリゲートを生成する`D`:。</span><span class="sxs-lookup"><span data-stu-id="01ae4-500">`Expression<D>` offers an instance method `Compile` which produces a delegate of type `D`:</span></span>

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    <span data-ttu-id="01ae4-501">このデリゲートの呼び出しが実行される式ツリーによって表されるコードです。</span><span class="sxs-lookup"><span data-stu-id="01ae4-501">Invoking this delegate causes the code represented by the expression tree to be executed.</span></span> <span data-ttu-id="01ae4-502">したがって、上記の定義を指定するには、del と del2 はそれと同等と、次の 2 つのステートメントは同じ効果があります。</span><span class="sxs-lookup"><span data-stu-id="01ae4-502">Thus, given the definitions above, del and del2 are equivalent, and the following two statements will have the same effect:</span></span>

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    <span data-ttu-id="01ae4-503">このコードを実行した後`i1`と`i2`どちらにも値`2`します。</span><span class="sxs-lookup"><span data-stu-id="01ae4-503">After executing this code,  `i1` and `i2` will both have the value `2`.</span></span>

