---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616140"
---
# <a name="types"></a><span data-ttu-id="8d05f-101">種類</span><span class="sxs-lookup"><span data-stu-id="8d05f-101">Types</span></span>

<span data-ttu-id="8d05f-102">C#言語の型は、***値型***と***参照型***の2つの主なカテゴリに分類されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-102">The types of the C# language are divided into two main categories: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="8d05f-103">値型と参照型は、どちらも1つ以上の***型パラメーター***を受け取る***ジェネリック型***にすることができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-103">Both value types and reference types may be ***generic types***, which take one or more ***type parameters***.</span></span> <span data-ttu-id="8d05f-104">型パラメーターは、値型と参照型の両方を指定できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-104">Type parameters can designate both value types and reference types.</span></span>

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

<span data-ttu-id="8d05f-105">型の最後のカテゴリであるポインターは、アンセーフコードでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-105">The final category of types, pointers, is available only in unsafe code.</span></span> <span data-ttu-id="8d05f-106">これについては、「[ポインター型](unsafe-code.md#pointer-types)」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-106">This is discussed further in [Pointer types](unsafe-code.md#pointer-types).</span></span>

<span data-ttu-id="8d05f-107">値型は参照型とは異なり、値型の変数はデータを直接含んでいるのに対し、参照型の変数はデータへの***参照***を格納します。後者は***オブジェクト***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-107">Value types differ from reference types in that variables of the value types directly contain their data, whereas variables of the reference types store ***references*** to their data, the latter being known as ***objects***.</span></span> <span data-ttu-id="8d05f-108">参照型を使用すると、2つの変数が同じオブジェクトを参照する可能性があります。したがって、ある変数に対する操作が、もう一方の変数によって参照されるオブジェクトに影響を与える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-108">With reference types, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="8d05f-109">値型の場合、それぞれの変数にはデータの独自のコピーがあり、一方の変数に対する操作がもう一方に影響を与えることはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-109">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span>

<span data-ttu-id="8d05f-110">C#の型システムは、任意の型の値をオブジェクトとして扱うことができるように統合されています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-110">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="8d05f-111">C# における型はすべて、直接的または間接的に `object` クラス型から派生し、`object` はすべての型の究極の基底クラスです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-111">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="8d05f-112">参照型の値は、値を単純に `object` 型としてみなすことによってオブジェクトとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-112">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="8d05f-113">値型の値は、ボックス化とボックス化解除の操作 ([ボックス化およびボックス化解除](types.md#boxing-and-unboxing)) を実行することで、オブジェクトとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-113">Values of value types are treated as objects by performing boxing and unboxing operations ([Boxing and unboxing](types.md#boxing-and-unboxing)).</span></span>

## <a name="value-types"></a><span data-ttu-id="8d05f-114">値型</span><span class="sxs-lookup"><span data-stu-id="8d05f-114">Value types</span></span>

<span data-ttu-id="8d05f-115">値型は、構造体型または列挙型のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-115">A value type is either a struct type or an enumeration type.</span></span> <span data-ttu-id="8d05f-116">C#***単純型***と呼ばれる定義済みの構造体型のセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-116">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="8d05f-117">単純型は、予約語によって識別されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-117">The simple types are identified through reserved words.</span></span>

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

<span data-ttu-id="8d05f-118">参照型の変数とは異なり、値型の変数には、値の型が null 許容型である場合にのみ `null` 値を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-118">Unlike a variable of a reference type, a variable of a value type can contain the value `null` only if the value type is a nullable type.</span></span>  <span data-ttu-id="8d05f-119">Null 非許容の値型ごとに、同じ値のセットと `null`値を示す、対応する null 許容値型があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-119">For every non-nullable value type there is a corresponding nullable value type denoting the same set of values plus the value `null`.</span></span>

<span data-ttu-id="8d05f-120">値型の変数への代入では、割り当てられている値のコピーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-120">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="8d05f-121">これは、参照によって識別されるオブジェクトではなく、参照をコピーする参照型の変数への代入とは異なります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-121">This differs from assignment to a variable of a reference type, which copies the reference but not the object identified by the reference.</span></span>

### <a name="the-systemvaluetype-type"></a><span data-ttu-id="8d05f-122">ValueType 型</span><span class="sxs-lookup"><span data-stu-id="8d05f-122">The System.ValueType type</span></span>

<span data-ttu-id="8d05f-123">すべての値型はクラス `System.ValueType`から暗黙的に継承します。このクラスはクラス `object`から継承されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-123">All value types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="8d05f-124">型を値型から派生させることはできません。したがって、値型は暗黙的にシールされます ([シールクラス](classes.md#sealed-classes))。</span><span class="sxs-lookup"><span data-stu-id="8d05f-124">It is not possible for any type to derive from a value type, and value types are thus implicitly sealed ([Sealed classes](classes.md#sealed-classes)).</span></span>

<span data-ttu-id="8d05f-125">`System.ValueType` はそれ自体が*value_type*ではないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-125">Note that `System.ValueType` is not itself a *value_type*.</span></span> <span data-ttu-id="8d05f-126">代わりに、すべての*value_type*が自動的に派生する*class_type*です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-126">Rather, it is a *class_type* from which all *value_type*s are automatically derived.</span></span>

### <a name="default-constructors"></a><span data-ttu-id="8d05f-127">既定のコンストラクター</span><span class="sxs-lookup"><span data-stu-id="8d05f-127">Default constructors</span></span>

<span data-ttu-id="8d05f-128">すべての値型は、***既定のコンストラクター***と呼ばれる、パラメーターなしのパブリックなインスタンスコンストラクターを暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-128">All value types implicitly declare a public parameterless instance constructor called the ***default constructor***.</span></span> <span data-ttu-id="8d05f-129">既定のコンストラクターは、値型の***既定値***として認識される、ゼロで初期化されたインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-129">The default constructor returns a zero-initialized instance known as the ***default value*** for the value type:</span></span>

*  <span data-ttu-id="8d05f-130">すべての*simple_type*s について、既定値は、すべてのゼロのビットパターンによって生成される値です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-130">For all *simple_type*s, the default value is the value produced by a bit pattern of all zeros:</span></span>
    * <span data-ttu-id="8d05f-131">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`の場合、既定値は `0`です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-131">For `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, and `ulong`, the default value is `0`.</span></span>
    * <span data-ttu-id="8d05f-132">`char`の場合、既定値は `'\x0000'`です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-132">For `char`, the default value is `'\x0000'`.</span></span>
    * <span data-ttu-id="8d05f-133">`float`の場合、既定値は `0.0f`です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-133">For `float`, the default value is `0.0f`.</span></span>
    * <span data-ttu-id="8d05f-134">`double`の場合、既定値は `0.0d`です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-134">For `double`, the default value is `0.0d`.</span></span>
    * <span data-ttu-id="8d05f-135">`decimal`の場合、既定値は `0.0m`です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-135">For `decimal`, the default value is `0.0m`.</span></span>
    * <span data-ttu-id="8d05f-136">`bool`の場合、既定値は `false`です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-136">For `bool`, the default value is `false`.</span></span>
*  <span data-ttu-id="8d05f-137">*Enum_type* `E`の場合、既定値は `0``E`型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-137">For an *enum_type* `E`, the default value is `0`, converted to the type `E`.</span></span>
*  <span data-ttu-id="8d05f-138">*Struct_type*の既定値は、すべての値の型フィールドを既定値に設定し、すべての参照型フィールドを `null`に設定することによって生成される値です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-138">For a *struct_type*, the default value is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>
*  <span data-ttu-id="8d05f-139">*Nullable_type*の場合、既定値は `HasValue` プロパティが false で、`Value` プロパティが定義されていないインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-139">For a *nullable_type* the default value is an instance for which the `HasValue` property is false and the `Value` property is undefined.</span></span> <span data-ttu-id="8d05f-140">既定値は null 許容型の***null 値***とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-140">The default value is also known as the ***null value*** of the nullable type.</span></span>

<span data-ttu-id="8d05f-141">他のインスタンスコンストラクターと同様に、値型の既定のコンストラクターは `new` 演算子を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-141">Like any other instance constructor, the default constructor of a value type is invoked using the `new` operator.</span></span> <span data-ttu-id="8d05f-142">効率上の理由から、実装によってコンストラクター呼び出しが生成されるようにするための要件はありません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-142">For efficiency reasons, this requirement is not intended to actually have the implementation generate a constructor call.</span></span> <span data-ttu-id="8d05f-143">次の例では、変数 `i` と `j` がどちらも0に初期化されています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-143">In the example below, variables `i` and `j` are both initialized to zero.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

<span data-ttu-id="8d05f-144">すべての値型には暗黙的にパラメーターなしのインスタンスコンストラクターがあるため、パラメーターなしのコンストラクターの明示的な宣言を struct 型に含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-144">Because every value type implicitly has a public parameterless instance constructor, it is not possible for a struct type to contain an explicit declaration of a parameterless constructor.</span></span> <span data-ttu-id="8d05f-145">ただし、構造体型では、パラメーター化されたインスタンスコンストラクター ([コンストラクター](structs.md#constructors)) を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-145">A struct type is however permitted to declare parameterized instance constructors ([Constructors](structs.md#constructors)).</span></span>

### <a name="struct-types"></a><span data-ttu-id="8d05f-146">構造体の型</span><span class="sxs-lookup"><span data-stu-id="8d05f-146">Struct types</span></span>

<span data-ttu-id="8d05f-147">構造体型は、定数、フィールド、メソッド、プロパティ、インデクサー、演算子、インスタンスコンストラクター、静的コンストラクター、および入れ子にされた型を宣言できる値型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-147">A struct type is a value type that can declare constants, fields, methods, properties, indexers, operators, instance constructors, static constructors, and nested types.</span></span> <span data-ttu-id="8d05f-148">構造体型の宣言については、「 [struct 宣言](structs.md#struct-declarations)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-148">The declaration of struct types is described in [Struct declarations](structs.md#struct-declarations).</span></span>

### <a name="simple-types"></a><span data-ttu-id="8d05f-149">単純型</span><span class="sxs-lookup"><span data-stu-id="8d05f-149">Simple types</span></span>

<span data-ttu-id="8d05f-150">C#***単純型***と呼ばれる定義済みの構造体型のセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-150">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="8d05f-151">単純型は予約語によって識別されますが、これらの予約語は、次の表で説明するように、`System` 名前空間の定義済みの構造体型のエイリアスにすぎません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-151">The simple types are identified through reserved words, but these reserved words are simply aliases for predefined struct types in the `System` namespace, as described in the table below.</span></span>


| <span data-ttu-id="8d05f-152">__予約語__</span><span class="sxs-lookup"><span data-stu-id="8d05f-152">__Reserved word__</span></span> | <span data-ttu-id="8d05f-153">__エイリアスを持つ型__</span><span class="sxs-lookup"><span data-stu-id="8d05f-153">__Aliased type__</span></span> |
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

<span data-ttu-id="8d05f-154">単純型は構造体型のエイリアスであるため、すべての単純型にメンバーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-154">Because a simple type aliases a struct type, every simple type has members.</span></span> <span data-ttu-id="8d05f-155">たとえば、`int` には `System.Int32` で宣言されたメンバーと `System.Object`から継承されたメンバーがあり、次のステートメントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-155">For example, `int` has the members declared in `System.Int32` and the members inherited from `System.Object`, and the following statements are permitted:</span></span>

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

<span data-ttu-id="8d05f-156">単純型は、ある追加の操作を許可している点で、他の構造体型とは異なります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-156">The simple types differ from other struct types in that they permit certain additional operations:</span></span>

*  <span data-ttu-id="8d05f-157">ほとんどの単純型では、*リテラル*([リテラル](lexical-structure.md#literals)) を記述することによって値を作成できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-157">Most simple types permit values to be created by writing *literals* ([Literals](lexical-structure.md#literals)).</span></span> <span data-ttu-id="8d05f-158">たとえば、`123` は `int` 型のリテラルで、`'a'` は `char`型のリテラルです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-158">For example, `123` is a literal of type `int` and `'a'` is a literal of type `char`.</span></span> <span data-ttu-id="8d05f-159">C#は、一般に構造体型のリテラルのプロビジョニングを行いません。また、他の構造体型の既定以外の値は、最終的にこれらの構造体型のインスタンスコンストラクターを使用して作成されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-159">C# makes no provision for literals of struct types in general, and non-default values of other struct types are ultimately always created through instance constructors of those struct types.</span></span>
*  <span data-ttu-id="8d05f-160">式のオペランドがすべて単純型定数である場合、コンパイラはコンパイル時に式を評価することができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-160">When the operands of an expression are all simple type constants, it is possible for the compiler to evaluate the expression at compile-time.</span></span> <span data-ttu-id="8d05f-161">このような式は、 *constant_expression* ([定数式](expressions.md#constant-expressions)) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-161">Such an expression is known as a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)).</span></span> <span data-ttu-id="8d05f-162">他の構造体型によって定義された演算子を含む式は、定数式とは見なされません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-162">Expressions involving operators defined by other struct types are not considered to be constant expressions.</span></span>
*  <span data-ttu-id="8d05f-163">`const` 宣言を使用すると、単純型 ([定数](classes.md#constants)) の定数を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-163">Through `const` declarations it is possible to declare constants of the simple types ([Constants](classes.md#constants)).</span></span> <span data-ttu-id="8d05f-164">他の構造体型の定数を使用することはできませんが、`static readonly` のフィールドでも同様の効果が得られます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-164">It is not possible to have constants of other struct types, but a similar effect is provided by `static readonly` fields.</span></span>
*  <span data-ttu-id="8d05f-165">単純型を含む変換は、他の構造体型によって定義された変換演算子の評価に参加できますが、ユーザー定義の変換演算子は、別のユーザー定義演算子の評価に参加することはできません ([の評価ユーザー定義の変換](conversions.md#evaluation-of-user-defined-conversions))。</span><span class="sxs-lookup"><span data-stu-id="8d05f-165">Conversions involving simple types can participate in evaluation of conversion operators defined by other struct types, but a user-defined conversion operator can never participate in evaluation of another user-defined operator ([Evaluation of user-defined conversions](conversions.md#evaluation-of-user-defined-conversions)).</span></span>

### <a name="integral-types"></a><span data-ttu-id="8d05f-166">整数型</span><span class="sxs-lookup"><span data-stu-id="8d05f-166">Integral types</span></span>

<span data-ttu-id="8d05f-167">C#では、`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`の9つの整数型がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-167">C# supports nine integral types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, and `char`.</span></span> <span data-ttu-id="8d05f-168">整数型には、次のサイズと値の範囲があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-168">The integral types have the following sizes and ranges of values:</span></span>

*  <span data-ttu-id="8d05f-169">`sbyte` 型は、-128 ~ 127 の値を持つ符号付き8ビット整数を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-169">The `sbyte` type represents signed 8-bit integers with values between -128 and 127.</span></span>
*  <span data-ttu-id="8d05f-170">`byte` 型は、0 ~ 255 の値を持つ符号なし8ビット整数を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-170">The `byte` type represents unsigned 8-bit integers with values between 0 and 255.</span></span>
*  <span data-ttu-id="8d05f-171">`short` 型は、-32768 ~ 32767 の値を持つ符号付き16ビット整数を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-171">The `short` type represents signed 16-bit integers with values between -32768 and 32767.</span></span>
*  <span data-ttu-id="8d05f-172">`ushort` 型は、0 ~ 65535 の値を持つ符号なし16ビット整数を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-172">The `ushort` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span>
*  <span data-ttu-id="8d05f-173">`int` 型は、-2147483648 から2147483647までの値を持つ符号付き32ビット整数を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-173">The `int` type represents signed 32-bit integers with values between -2147483648 and 2147483647.</span></span>
*  <span data-ttu-id="8d05f-174">`uint` 型は、0 ~ 4294967295 の値を持つ符号なし32ビット整数を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-174">The `uint` type represents unsigned 32-bit integers with values between 0 and 4294967295.</span></span>
*  <span data-ttu-id="8d05f-175">`long` 型は、-9223372036854775808 ~ 9223372036854775807 の値を持つ符号付き64ビット整数を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-175">The `long` type represents signed 64-bit integers with values between -9223372036854775808 and 9223372036854775807.</span></span>
*  <span data-ttu-id="8d05f-176">`ulong` 型は、0 ~ 18446744073709551615 の値を持つ符号なし64ビット整数を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-176">The `ulong` type represents unsigned 64-bit integers with values between 0 and 18446744073709551615.</span></span>
*  <span data-ttu-id="8d05f-177">`char` 型は、0 ~ 65535 の値を持つ符号なし16ビット整数を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-177">The `char` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span> <span data-ttu-id="8d05f-178">`char` 型に使用できる値のセットは、Unicode 文字セットに対応しています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-178">The set of possible values for the `char` type corresponds to the Unicode character set.</span></span> <span data-ttu-id="8d05f-179">`char` には `ushort`と同じ表現がありますが、1つの型で許可されているすべての操作が他方で許可されているわけではありません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-179">Although `char` has the same representation as `ushort`, not all operations permitted on one type are permitted on the other.</span></span>

<span data-ttu-id="8d05f-180">整数型の単項演算子および二項演算子は、常に、符号付き32ビット精度、符号なし32ビット精度、符号付き64ビット精度、符号なし64ビット精度で動作します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-180">The integral-type unary and binary operators always operate with signed 32-bit precision, unsigned 32-bit precision, signed 64-bit precision, or unsigned 64-bit precision:</span></span>

*  <span data-ttu-id="8d05f-181">単項演算子 `+` と `~` 演算子の場合、オペランドは `T`型に変換されます。ここで、`T` は、オペランドのすべての使用可能な値を完全に表すことができる `int`、`uint`、`long`、および `ulong` の最初のものです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-181">For the unary `+` and `~` operators, the operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="8d05f-182">この操作は `T`型の有効桁数を使用して実行され、結果の型は `T`になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-182">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>
*  <span data-ttu-id="8d05f-183">単項 `-` 演算子では、オペランドが型 `T`に変換されます。ここで、`T` は、オペランドのすべての可能な値を完全に表すことができる `int` および `long` の最初のものです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-183">For the unary `-` operator, the operand is converted to type `T`, where `T` is the first of `int` and `long` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="8d05f-184">この操作は `T`型の有効桁数を使用して実行され、結果の型は `T`になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-184">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span> <span data-ttu-id="8d05f-185">単項 `-` 演算子を `ulong`型のオペランドに適用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-185">The unary `-` operator cannot be applied to operands of type `ulong`.</span></span>
*  <span data-ttu-id="8d05f-186">バイナリ `+`、`-`、`*`、`/`、`%`、`&`、`^`、`|`、`==`、`!=`、`>`、`<`の各演算子では、オペランドは `T`型に変換されます。ここで、`T` は、両方のオペランドのすべての可能な値を完全に表すことができる `int`、`uint`、`long`、および `ulong` の最初のものです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-186">For the binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, and `<=` operators, the operands are converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of both operands.</span></span> <span data-ttu-id="8d05f-187">この操作は `T`型の有効桁数を使用して実行され、結果の型は `T` になります (または、関係演算子の `bool` ます)。</span><span class="sxs-lookup"><span data-stu-id="8d05f-187">The operation is then performed using the precision of type `T`, and the type of the result is `T` (or `bool` for the relational operators).</span></span> <span data-ttu-id="8d05f-188">1つのオペランドを `long` 型にし、もう一方のオペランドを二項演算子で `ulong` 型にすることは許可されていません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-188">It is not permitted for one operand to be of type `long` and the other to be of type `ulong` with the binary operators.</span></span>
*  <span data-ttu-id="8d05f-189">二項 `<<` および `>>` 演算子の場合、左オペランドは `T`型に変換されます。ここで `T` は、オペランドのすべての可能な値を完全に表すことができる `int`、`uint`、`long`、および `ulong` の最初のものです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-189">For the binary `<<` and `>>` operators, the left operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="8d05f-190">この操作は `T`型の有効桁数を使用して実行され、結果の型は `T`になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-190">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>

<span data-ttu-id="8d05f-191">`char` 型は整数型として分類されますが、他の整数型とは次の2つの点で異なります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-191">The `char` type is classified as an integral type, but it differs from the other integral types in two ways:</span></span>

*  <span data-ttu-id="8d05f-192">他の型から `char` 型へと暗黙的に変換することはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-192">There are no implicit conversions from other types to the `char` type.</span></span> <span data-ttu-id="8d05f-193">特に、`sbyte`、`byte`、および `ushort` 型には、`char` 型を使用して完全に表現できる値の範囲がある場合でも、`sbyte`、`byte`、または `ushort` から `char` への暗黙的な変換は存在しません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-193">In particular, even though the `sbyte`, `byte`, and `ushort` types have ranges of values that are fully representable using the `char` type, implicit conversions from `sbyte`, `byte`, or `ushort` to `char` do not exist.</span></span>
*  <span data-ttu-id="8d05f-194">`char` 型の定数は、 *character_literal*s として記述するか、 *integer_literal*を `char`型へのキャストと組み合わせて使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-194">Constants of the `char` type must be written as *character_literal*s or as *integer_literal*s in combination with a cast to type `char`.</span></span> <span data-ttu-id="8d05f-195">たとえば、`(char)10` は `'\x000A'` と同じです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-195">For example, `(char)10` is the same as `'\x000A'`.</span></span>

<span data-ttu-id="8d05f-196">`checked` および `unchecked` の演算子とステートメントを使用して、整数型の算術演算および変換 ([checked および unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) のオーバーフローチェックを制御します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-196">The `checked` and `unchecked` operators and statements are used to control overflow checking for integral-type arithmetic operations and conversions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span> <span data-ttu-id="8d05f-197">`checked` コンテキストでは、オーバーフローによってコンパイル時エラーが生成されるか、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-197">In a `checked` context, an overflow produces a compile-time error or causes a `System.OverflowException` to be thrown.</span></span> <span data-ttu-id="8d05f-198">`unchecked` のコンテキストでは、オーバーフローは無視され、変換先の型に収まらない上位ビットはすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-198">In an `unchecked` context, overflows are ignored and any high-order bits that do not fit in the destination type are discarded.</span></span>

### <a name="floating-point-types"></a><span data-ttu-id="8d05f-199">浮動小数点型</span><span class="sxs-lookup"><span data-stu-id="8d05f-199">Floating point types</span></span>

<span data-ttu-id="8d05f-200">C#では、`float` と `double`の2つの浮動小数点型がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-200">C# supports two floating point types: `float` and `double`.</span></span> <span data-ttu-id="8d05f-201">`float` 型と `double` 型は、32ビットの単精度と64ビットの倍精度の IEEE 754 形式を使用して表されます。これにより、次の値のセットが提供されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-201">The `float` and `double` types are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats, which provide the following sets of values:</span></span>

*  <span data-ttu-id="8d05f-202">正のゼロと負の0。</span><span class="sxs-lookup"><span data-stu-id="8d05f-202">Positive zero and negative zero.</span></span> <span data-ttu-id="8d05f-203">ほとんどの場合、正のゼロと負の0は単純な値ゼロと同じように動作しますが、特定の操作で2つの ([除算演算子](expressions.md#division-operator)) が区別されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-203">In most situations, positive zero and negative zero behave identically as the simple value zero, but certain operations distinguish between the two ([Division operator](expressions.md#division-operator)).</span></span>
*  <span data-ttu-id="8d05f-204">正の無限大と負の無限大。</span><span class="sxs-lookup"><span data-stu-id="8d05f-204">Positive infinity and negative infinity.</span></span> <span data-ttu-id="8d05f-205">無限大は、0以外の数値を0で除算するなどの操作によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-205">Infinities are produced by such operations as dividing a non-zero number by zero.</span></span> <span data-ttu-id="8d05f-206">たとえば、`1.0 / 0.0` は正の無限大を生成し、`-1.0 / 0.0` は負の無限大を生成します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-206">For example, `1.0 / 0.0` yields positive infinity, and `-1.0 / 0.0` yields negative infinity.</span></span>
*  <span data-ttu-id="8d05f-207">非***数***の値。多くの場合、NaN が省略されています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-207">The ***Not-a-Number*** value, often abbreviated NaN.</span></span> <span data-ttu-id="8d05f-208">Nan は、0除算などの無効な浮動小数点演算によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-208">NaNs are produced by invalid floating-point operations, such as dividing zero by zero.</span></span>
*  <span data-ttu-id="8d05f-209">`s` が1または-1 で、`m` および `e` が特定の浮動小数点型によって決定される、`s * m * 2^e`形式の0以外の値の有限のセット。 `float`の場合は、`0 < m < 2^24` の場合は `-149 <= e <= 104`、`double`、`0 < m < 2^53` および `-1075 <= e <= 970`。</span><span class="sxs-lookup"><span data-stu-id="8d05f-209">The finite set of non-zero values of the form `s * m * 2^e`, where `s` is 1 or -1, and `m` and `e` are determined by the particular floating-point type: For `float`, `0 < m < 2^24` and `-149 <= e <= 104`, and for `double`, `0 < m < 2^53` and `-1075 <= e <= 970`.</span></span> <span data-ttu-id="8d05f-210">非正規化された浮動小数点数は、0以外の有効な値と見なされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-210">Denormalized floating-point numbers are considered valid non-zero values.</span></span>

<span data-ttu-id="8d05f-211">`float` 型は、有効桁数が7桁の、約 `1.5 * 10^-45` から `3.4 * 10^38` までの範囲の値を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-211">The `float` type can represent values ranging from approximately `1.5 * 10^-45` to `3.4 * 10^38` with a precision of 7 digits.</span></span>

<span data-ttu-id="8d05f-212">`double` 型は、15-16 桁の有効桁数で、約 `5.0 * 10^-324` から `1.7 × 10^308` までの範囲の値を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-212">The `double` type can represent values ranging from approximately `5.0 * 10^-324` to `1.7 × 10^308` with a precision of 15-16 digits.</span></span>

<span data-ttu-id="8d05f-213">二項演算子のオペランドの1つが浮動小数点型の場合、もう一方のオペランドは整数型または浮動小数点型である必要があり、演算は次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-213">If one of the operands of a binary operator is of a floating-point type, then the other operand must be of an integral type or a floating-point type, and the operation is evaluated as follows:</span></span>

*  <span data-ttu-id="8d05f-214">オペランドの1つが整数型の場合、そのオペランドはもう一方のオペランドの浮動小数点型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-214">If one of the operands is of an integral type, then that operand is converted to the floating-point type of the other operand.</span></span>
*  <span data-ttu-id="8d05f-215">次に、いずれかのオペランドが `double`型である場合、もう一方のオペランドが `double`に変換され、少なくとも `double` 範囲と有効桁数を使用して演算が実行され、結果の型が `double` (または関係演算子の `bool`) になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-215">Then, if either of the operands is of type `double`, the other operand is converted to `double`, the operation is performed using at least `double` range and precision, and the type of the result is `double` (or `bool` for the relational operators).</span></span>
*  <span data-ttu-id="8d05f-216">それ以外の場合は、少なくとも `float` 範囲と有効桁数を使用して演算が実行され、結果の型が `float` ます (または、関係演算子の `bool`)。</span><span class="sxs-lookup"><span data-stu-id="8d05f-216">Otherwise, the operation is performed using at least `float` range and precision, and the type of the result is `float` (or `bool` for the relational operators).</span></span>

<span data-ttu-id="8d05f-217">代入演算子を含む浮動小数点演算子は、例外を生成しません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-217">The floating-point operators, including the assignment operators, never produce exceptions.</span></span> <span data-ttu-id="8d05f-218">次に示すように、例外的な状況では、浮動小数点演算ではゼロ、無限大、または NaN が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-218">Instead, in exceptional situations, floating-point operations produce zero, infinity, or NaN, as described below:</span></span>

*  <span data-ttu-id="8d05f-219">浮動小数点演算の結果が変換先の形式に対して小さすぎる場合、演算の結果は正の0または負の0になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-219">If the result of a floating-point operation is too small for the destination format, the result of the operation becomes positive zero or negative zero.</span></span>
*  <span data-ttu-id="8d05f-220">浮動小数点演算の結果が変換先の形式に対して大きすぎる場合、演算の結果は正の無限大または負の無限大になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-220">If the result of a floating-point operation is too large for the destination format, the result of the operation becomes positive infinity or negative infinity.</span></span>
*  <span data-ttu-id="8d05f-221">浮動小数点演算が無効な場合、演算の結果は NaN になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-221">If a floating-point operation is invalid, the result of the operation becomes NaN.</span></span>
*  <span data-ttu-id="8d05f-222">浮動小数点演算の一方または両方のオペランドが NaN の場合、演算の結果は NaN になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-222">If one or both operands of a floating-point operation is NaN, the result of the operation becomes NaN.</span></span>

<span data-ttu-id="8d05f-223">浮動小数点演算は、演算の結果の型よりも高い精度で実行される場合があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-223">Floating-point operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="8d05f-224">たとえば、一部のハードウェアアーキテクチャでは、`double` 型よりも範囲と有効桁数が大きい "拡張" または "long double" 浮動小数点型がサポートされており、このより高い有効桁数の型を使用してすべての浮動小数点演算が暗黙的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-224">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `double` type, and implicitly perform all floating-point operations using this higher precision type.</span></span> <span data-ttu-id="8d05f-225">このようなハードウェアアーキテクチャは、低精度で浮動小数点演算を実行し、パフォーマンスと精度の両方をプランするための実装を必要とするのでC#はなく、高いパフォーマンスを実現することだけが可能です。すべての浮動小数点演算に使用されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-225">Only at excessive cost in performance can such hardware architectures be made to perform floating-point operations with less precision, and rather than require an implementation to forfeit both performance and precision, C# allows a higher precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="8d05f-226">より正確な結果を提供する以外にも、測定可能な効果はほとんどありません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-226">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="8d05f-227">ただし、`x * y / z`の形式の式では、乗算によって `double` 範囲外の結果が生成されますが、その後の除算では `double` 範囲に一時的な結果が返されます。これは、式がで評価されるという事実です。範囲の形式が大きいと、無限大ではなく、有限の結果が生成される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-227">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `double` range, but the subsequent division brings the temporary result back into the `double` range, the fact that the expression is evaluated in a higher range format may cause a finite result to be produced instead of an infinity.</span></span>

### <a name="the-decimal-type"></a><span data-ttu-id="8d05f-228">Decimal 型</span><span class="sxs-lookup"><span data-stu-id="8d05f-228">The decimal type</span></span>

<span data-ttu-id="8d05f-229">`decimal` 型は 128 ビットのデータ型で、財務や通貨の計算に適しています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-229">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span> <span data-ttu-id="8d05f-230">`decimal` 型は、`1.0 * 10^-28` から `7.9 * 10^28` 約28-29 の範囲の値を表すことができます。有効桁数はです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-230">The `decimal` type can represent values ranging from `1.0 * 10^-28` to approximately `7.9 * 10^28` with 28-29 significant digits.</span></span>

<span data-ttu-id="8d05f-231">`decimal` 型の有限の値のセットは `(-1)^s * c * 10^-e`形式です。符号 `s` は0または1であり、`c` 係数は `0 <= *c* < 2^96`によって指定され、小数点以下桁数は `e` になります。`decimal` 型は、符号付きの0、無限大、または NaN のをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-231">The finite set of values of type `decimal` are of the form `(-1)^s * c * 10^-e`, where the sign `s` is 0 or 1, the coefficient `c` is given by `0 <= *c* < 2^96`, and the scale `e` is such that `0 <= e <= 28`.The `decimal` type does not support signed zeros, infinities, or NaN's.</span></span> <span data-ttu-id="8d05f-232">`decimal` は、10の累乗によってスケーリングされた96ビット整数として表されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-232">A `decimal` is represented as a 96-bit integer scaled by a power of ten.</span></span> <span data-ttu-id="8d05f-233">`1.0m`未満の絶対値を持つ `decimal`の場合、値は28桁の小数点以下の桁数に完全になりますが、それ以上はありません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-233">For `decimal`s with an absolute value less than `1.0m`, the value is exact to the 28th decimal place, but no further.</span></span> <span data-ttu-id="8d05f-234">`1.0m`以上の絶対値を持つ `decimal`s では、値は28桁または29桁になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-234">For `decimal`s with an absolute value greater than or equal to `1.0m`, the value is exact to 28 or 29 digits.</span></span> <span data-ttu-id="8d05f-235">データ型の `float` と `double` とは対照的に、0.1 のような10進数の小数部は、`decimal` 表現で正確に表すことができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-235">Contrary to the `float` and `double` data types, decimal fractional numbers such as 0.1 can be represented exactly in the `decimal` representation.</span></span> <span data-ttu-id="8d05f-236">`float` と `double` 表現では、多くの場合、このような数値は無限の分数になるため、これらの表現が丸められると、丸め誤差が発生しやすくなります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-236">In the `float` and `double` representations, such numbers are often infinite fractions, making those representations more prone to round-off errors.</span></span>

<span data-ttu-id="8d05f-237">二項演算子のオペランドの1つが `decimal`型である場合、もう一方のオペランドは整数型または `decimal`型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-237">If one of the operands of a binary operator is of type `decimal`, then the other operand must be of an integral type or of type `decimal`.</span></span> <span data-ttu-id="8d05f-238">整数型のオペランドが存在する場合は、演算が実行される前に `decimal` に変換されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-238">If an integral type operand is present, it is converted to `decimal` before the operation is performed.</span></span>

<span data-ttu-id="8d05f-239">`decimal` 型の値に対する演算の結果は、正確な結果 (各演算子に対して定義されているように、小数点以下桁数を保持します) を計算してから、その表現に合わせて丸めた結果になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-239">The result of an operation on values of type `decimal` is that which would result from calculating an exact result (preserving scale, as defined for each operator) and then rounding to fit the representation.</span></span> <span data-ttu-id="8d05f-240">結果は、最も近い表現可能な値に丸められます。また、結果が2つの表現可能な値に均等に近い場合は、最下位の桁に偶数の数値が含まれる値になります (これは "銀行型丸め" と呼ばれます)。</span><span class="sxs-lookup"><span data-stu-id="8d05f-240">Results are rounded to the nearest representable value, and, when a result is equally close to two representable values, to the value that has an even number in the least significant digit position (this is known as "banker's rounding").</span></span> <span data-ttu-id="8d05f-241">ゼロの結果は、常に0の符号と小数点以下桁数が0になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-241">A zero result always has a sign of 0 and a scale of 0.</span></span>

<span data-ttu-id="8d05f-242">10進数の算術演算で、絶対値が絶対値で `5 * 10^-29` 以下の値が生成された場合、演算の結果はゼロになります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-242">If a decimal arithmetic operation produces a value less than or equal to `5 * 10^-29` in absolute value, the result of the operation becomes zero.</span></span> <span data-ttu-id="8d05f-243">`decimal` 算術演算によって、`decimal` 形式に対して大きすぎる結果が生成されると、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-243">If a `decimal` arithmetic operation produces a result that is too large for the `decimal` format, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="8d05f-244">`decimal` 型は、より精度が高く、浮動小数点型よりも範囲が小さくなっています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-244">The `decimal` type has greater precision but smaller range than the floating-point types.</span></span> <span data-ttu-id="8d05f-245">したがって、浮動小数点型から `decimal` への変換ではオーバーフロー例外が発生する可能性があり、`decimal` から浮動小数点型への変換によって精度が失われる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-245">Thus, conversions from the floating-point types to `decimal` might produce overflow exceptions, and conversions from `decimal` to the floating-point types might cause loss of precision.</span></span> <span data-ttu-id="8d05f-246">このような理由から、浮動小数点型と `decimal`の間に暗黙的な変換は存在せず、明示的なキャストを行わないと、浮動小数点と `decimal` のオペランドを同じ式に混在させることはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-246">For these reasons, no implicit conversions exist between the floating-point types and `decimal`, and without explicit casts, it is not possible to mix floating-point and `decimal` operands in the same expression.</span></span>

### <a name="the-bool-type"></a><span data-ttu-id="8d05f-247">Bool 型</span><span class="sxs-lookup"><span data-stu-id="8d05f-247">The bool type</span></span>

<span data-ttu-id="8d05f-248">`bool` 型はブール値の論理数を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-248">The `bool` type represents boolean logical quantities.</span></span> <span data-ttu-id="8d05f-249">`bool` 型の有効な値は `true` と `false`です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-249">The possible values of type `bool` are `true` and `false`.</span></span>

<span data-ttu-id="8d05f-250">`bool` とその他の型の間に標準変換は存在しません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-250">No standard conversions exist between `bool` and other types.</span></span> <span data-ttu-id="8d05f-251">特に、`bool` 型は、整数型とは区別されていますが、整数値の代わりに `bool` 値を使用することはできません。その逆も同様です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-251">In particular, the `bool` type is distinct and separate from the integral types, and a `bool` value cannot be used in place of an integral value, and vice versa.</span></span>

<span data-ttu-id="8d05f-252">C とC++言語では、ゼロの整数または浮動小数点値、または null ポインターをブール値 `false`、0以外の整数または浮動小数点値、または null 以外のポインターをブール値 `true`に変換できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-252">In the C and C++ languages, a zero integral or floating-point value, or a null pointer can be converted to the boolean value `false`, and a non-zero integral or floating-point value, or a non-null pointer can be converted to the boolean value `true`.</span></span> <span data-ttu-id="8d05f-253">でC#は、整数または浮動小数点値を明示的に0に比較するか、オブジェクト参照を `null`に明示的に比較することで、このような変換が行われます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-253">In C#, such conversions are accomplished by explicitly comparing an integral or floating-point value to zero, or by explicitly comparing an object reference to `null`.</span></span>

### <a name="enumeration-types"></a><span data-ttu-id="8d05f-254">列挙型</span><span class="sxs-lookup"><span data-stu-id="8d05f-254">Enumeration types</span></span>

<span data-ttu-id="8d05f-255">列挙型は、名前付き定数を持つ別個の型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-255">An enumeration type is a distinct type with named constants.</span></span> <span data-ttu-id="8d05f-256">すべての列挙型には基になる型があり、`byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`のいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-256">Every enumeration type has an underlying type, which must be `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="8d05f-257">列挙型の値のセットは、基になる型の値のセットと同じです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-257">The set of values of the enumeration type is the same as the set of values of the underlying type.</span></span> <span data-ttu-id="8d05f-258">列挙型の値は、名前付き定数の値に制限されません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-258">Values of the enumeration type are not restricted to the values of the named constants.</span></span> <span data-ttu-id="8d05f-259">列挙型は列挙宣言 (列挙型[宣言](enums.md#enum-declarations)) を使用して定義されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-259">Enumeration types are defined through enumeration declarations ([Enum declarations](enums.md#enum-declarations)).</span></span>

### <a name="nullable-types"></a><span data-ttu-id="8d05f-260">Null 許容型</span><span class="sxs-lookup"><span data-stu-id="8d05f-260">Nullable types</span></span>

<span data-ttu-id="8d05f-261">Null 許容型は、***基になる型***のすべての値と追加の null 値を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-261">A nullable type can represent all values of its ***underlying type*** plus an additional null value.</span></span> <span data-ttu-id="8d05f-262">Null 許容型は `T?`書き込まれます。 `T` は基になる型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-262">A nullable type is written `T?`, where `T` is the underlying type.</span></span> <span data-ttu-id="8d05f-263">この構文は `System.Nullable<T>`の短縮形であり、2つの形式を区別して使用できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-263">This syntax is shorthand for `System.Nullable<T>`, and the two forms can be used interchangeably.</span></span>

<span data-ttu-id="8d05f-264">逆に、 ***null 非許容の値型***は、`System.Nullable<T>` 以外の任意の値型と、その短縮形 `T?` (任意の `T`) と、null 非許容の値型 (つまり、`struct` を持つ任意の型パラメーター) に制限されている任意の型パラメーターです。制約)。</span><span class="sxs-lookup"><span data-stu-id="8d05f-264">A ***non-nullable value type*** conversely is any value type other than `System.Nullable<T>` and its shorthand `T?` (for any `T`), plus any type parameter that is constrained to be a non-nullable value type (that is, any type parameter with a `struct` constraint).</span></span> <span data-ttu-id="8d05f-265">`System.Nullable<T>` 型は、`T` ([型パラメーター制約](classes.md#type-parameter-constraints)) の値型の制約を指定します。これは、null 許容型の基になる型が null 非許容の値型であることを意味します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-265">The `System.Nullable<T>` type specifies the value type constraint for `T` ([Type parameter constraints](classes.md#type-parameter-constraints)), which means that the underlying type of a nullable type can be any non-nullable value type.</span></span> <span data-ttu-id="8d05f-266">Null 許容型の基になる型を null 許容型または参照型にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-266">The underlying type of a nullable type cannot be a nullable type or a reference type.</span></span> <span data-ttu-id="8d05f-267">たとえば、`int??` と `string?` は無効な型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-267">For example, `int??` and `string?` are invalid types.</span></span>

<span data-ttu-id="8d05f-268">Null 許容型 `T?` のインスタンスには、次の2つのパブリック読み取り専用プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-268">An instance of a nullable type `T?` has two public read-only properties:</span></span>

*  <span data-ttu-id="8d05f-269">型の `HasValue` プロパティ `bool`</span><span class="sxs-lookup"><span data-stu-id="8d05f-269">A `HasValue` property of type `bool`</span></span>
*  <span data-ttu-id="8d05f-270">型の `Value` プロパティ `T`</span><span class="sxs-lookup"><span data-stu-id="8d05f-270">A `Value` property of type `T`</span></span>

<span data-ttu-id="8d05f-271">`HasValue` が true であるインスタンスは、null 以外であると言います。</span><span class="sxs-lookup"><span data-stu-id="8d05f-271">An instance for which `HasValue` is true is said to be non-null.</span></span> <span data-ttu-id="8d05f-272">Null 以外のインスタンスには既知の値が含まれており、`Value` その値を返します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-272">A non-null instance contains a known value and `Value` returns that value.</span></span>

<span data-ttu-id="8d05f-273">`HasValue` が false であるインスタンスは、null と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-273">An instance for which `HasValue` is false is said to be null.</span></span> <span data-ttu-id="8d05f-274">Null インスタンスには未定義の値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-274">A null instance has an undefined value.</span></span> <span data-ttu-id="8d05f-275">Null インスタンスの `Value` を読み取ろうとすると、`System.InvalidOperationException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-275">Attempting to read the `Value` of a null instance causes a `System.InvalidOperationException` to be thrown.</span></span> <span data-ttu-id="8d05f-276">Null 許容インスタンスの `Value` プロパティにアクセスするプロセスを、***ラップ***解除と呼びます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-276">The process of accessing the `Value` property of a nullable instance is referred to as ***unwrapping***.</span></span>

<span data-ttu-id="8d05f-277">既定のコンストラクターに加えて、すべての null 許容型 `T?` には、`T`型の単一の引数を受け取るパブリックコンストラクターがあります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-277">In addition to the default constructor, every nullable type `T?` has a public constructor that takes a single argument of type `T`.</span></span> <span data-ttu-id="8d05f-278">`T`型の値 `x` を指定した場合、フォームのコンストラクターが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-278">Given a value `x` of type `T`, a constructor invocation of the form</span></span>

```csharp
new T?(x)
```
<span data-ttu-id="8d05f-279">`Value` プロパティが `x`される `T?` の null 以外のインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-279">creates a non-null instance of `T?` for which the `Value` property is `x`.</span></span> <span data-ttu-id="8d05f-280">指定された値の null 許容型の null 以外のインスタンスを作成するプロセスを、***ラップ***と呼びます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-280">The process of creating a non-null instance of a nullable type for a given value is referred to as ***wrapping***.</span></span>

<span data-ttu-id="8d05f-281">暗黙の型変換は、`null` リテラルから、`T?` ([Null リテラル変換](conversions.md#null-literal-conversions)) と `T` から `T?` ([暗黙の null 許容型変換](conversions.md#implicit-nullable-conversions)) から使用できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-281">Implicit conversions are available from the `null` literal to `T?` ([Null literal conversions](conversions.md#null-literal-conversions)) and from `T` to `T?` ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)).</span></span>

## <a name="reference-types"></a><span data-ttu-id="8d05f-282">参照型</span><span class="sxs-lookup"><span data-stu-id="8d05f-282">Reference types</span></span>

<span data-ttu-id="8d05f-283">参照型は、クラス型、インターフェイス型、配列型、またはデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-283">A reference type is a class type, an interface type, an array type, or a delegate type.</span></span>

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

<span data-ttu-id="8d05f-284">参照型の値は、型の***インスタンス***への参照 (後者は***オブジェクト***と呼ばれます) です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-284">A reference type value is a reference to an ***instance*** of the type, the latter known as an ***object***.</span></span> <span data-ttu-id="8d05f-285">`null` 特別な値は、すべての参照型と互換性があり、インスタンスが存在しないことを示します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-285">The special value `null` is compatible with all reference types and indicates the absence of an instance.</span></span>

### <a name="class-types"></a><span data-ttu-id="8d05f-286">クラス型</span><span class="sxs-lookup"><span data-stu-id="8d05f-286">Class types</span></span>

<span data-ttu-id="8d05f-287">クラス型は、データメンバー (定数とフィールド)、関数メンバー (メソッド、プロパティ、イベント、インデクサー、演算子、インスタンスコンストラクター、デストラクターおよび静的コンストラクター)、および入れ子にされた型を含むデータ構造を定義します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-287">A class type defines a data structure that contains data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="8d05f-288">クラス型は、派生クラスが基本クラスを拡張および特殊化できる機構である継承をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-288">Class types support inheritance, a mechanism whereby derived classes can extend and specialize base classes.</span></span> <span data-ttu-id="8d05f-289">クラス型のインスタンスは、 *object_creation_expression*s ([オブジェクト作成式](expressions.md#object-creation-expressions)) を使用して作成されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-289">Instances of class types are created using *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>

<span data-ttu-id="8d05f-290">クラス型については、「[クラス](classes.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-290">Class types are described in [Classes](classes.md).</span></span>

<span data-ttu-id="8d05f-291">次の表で説明するように、 C#定義済みの特定のクラス型は言語で特別な意味を持ちます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-291">Certain predefined class types have special meaning in the C# language, as described in the table below.</span></span>


| <span data-ttu-id="8d05f-292">__クラスの型__</span><span class="sxs-lookup"><span data-stu-id="8d05f-292">__Class type__</span></span>     | <span data-ttu-id="8d05f-293">__説明__</span><span class="sxs-lookup"><span data-stu-id="8d05f-293">__Description__</span></span>                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | <span data-ttu-id="8d05f-294">他のすべての型の最終的な基底クラス。</span><span class="sxs-lookup"><span data-stu-id="8d05f-294">The ultimate base class of all other types.</span></span> <span data-ttu-id="8d05f-295">[オブジェクトの種類を](types.md#the-object-type)参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-295">See [The object type](types.md#the-object-type).</span></span> | 
| `System.String`    | <span data-ttu-id="8d05f-296">C#言語の文字列型。</span><span class="sxs-lookup"><span data-stu-id="8d05f-296">The string type of the C# language.</span></span> <span data-ttu-id="8d05f-297">[文字列型を](types.md#the-string-type)参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-297">See [The string type](types.md#the-string-type).</span></span>         |
| `System.ValueType` | <span data-ttu-id="8d05f-298">すべての値型の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="8d05f-298">The base class of all value types.</span></span> <span data-ttu-id="8d05f-299">[ValueType 型を](types.md#the-systemvaluetype-type)参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-299">See [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>          |
| `System.Enum`      | <span data-ttu-id="8d05f-300">すべての列挙型の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="8d05f-300">The base class of all enum types.</span></span> <span data-ttu-id="8d05f-301">「[列挙](enums.md)型」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-301">See [Enums](enums.md).</span></span>              |
| `System.Array`     | <span data-ttu-id="8d05f-302">すべての配列型の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="8d05f-302">The base class of all array types.</span></span> <span data-ttu-id="8d05f-303">「[配列](arrays.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-303">See [Arrays](arrays.md).</span></span>             |
| `System.Delegate`  | <span data-ttu-id="8d05f-304">すべてのデリゲート型の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="8d05f-304">The base class of all delegate types.</span></span> <span data-ttu-id="8d05f-305">「[デリゲート](delegates.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-305">See [Delegates](delegates.md).</span></span>          |
| `System.Exception` | <span data-ttu-id="8d05f-306">すべての例外の種類の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="8d05f-306">The base class of all exception types.</span></span> <span data-ttu-id="8d05f-307">「[例外](exceptions.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-307">See [Exceptions](exceptions.md).</span></span>         |

### <a name="the-object-type"></a><span data-ttu-id="8d05f-308">オブジェクト型</span><span class="sxs-lookup"><span data-stu-id="8d05f-308">The object type</span></span>

<span data-ttu-id="8d05f-309">`object` クラス型は、他のすべての型の最終的な基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-309">The `object` class type is the ultimate base class of all other types.</span></span> <span data-ttu-id="8d05f-310">内のすべてC#の型は、`object` クラス型から直接または間接的に派生します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-310">Every type in C# directly or indirectly derives from the `object` class type.</span></span>

<span data-ttu-id="8d05f-311">キーワード `object` は、定義済みのクラス `System.Object`のエイリアスにすぎません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-311">The keyword `object` is simply an alias for the predefined class `System.Object`.</span></span>

### <a name="the-dynamic-type"></a><span data-ttu-id="8d05f-312">dynamic 型</span><span class="sxs-lookup"><span data-stu-id="8d05f-312">The dynamic type</span></span>

<span data-ttu-id="8d05f-313">`dynamic` 型 (`object`など) は任意のオブジェクトを参照できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-313">The `dynamic` type, like `object`, can reference any object.</span></span> <span data-ttu-id="8d05f-314">`dynamic`型の式に演算子を適用すると、その解決はプログラムが実行されるまで延期されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-314">When operators are applied to expressions of type `dynamic`, their resolution is deferred until the program is run.</span></span> <span data-ttu-id="8d05f-315">したがって、演算子を参照先のオブジェクトに合法的に適用できない場合、コンパイル中にエラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-315">Thus, if the operator cannot legally be applied to the referenced object, no error is given during compilation.</span></span> <span data-ttu-id="8d05f-316">代わりに、実行時に演算子の解決が失敗すると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-316">Instead an exception will be thrown when resolution of the operator fails at run-time.</span></span>

<span data-ttu-id="8d05f-317">その目的は動的バインドを許可することです。これについては、「[動的バインド](expressions.md#dynamic-binding)」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-317">Its purpose is to allow dynamic binding, which is described in detail in [Dynamic binding](expressions.md#dynamic-binding).</span></span>

<span data-ttu-id="8d05f-318">`dynamic` は、次の点を除けば `object` と同一であると見なされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-318">`dynamic` is considered identical to `object` except in the following respects:</span></span>

*  <span data-ttu-id="8d05f-319">`dynamic` 型の式に対する演算は動的にバインドできます ([動的バインド](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="8d05f-319">Operations on expressions of type `dynamic` can be dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span>
*  <span data-ttu-id="8d05f-320">型の推定 ([型の推論](expressions.md#type-inference)) では、両方が候補である場合、`object` よりも `dynamic` 優先されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-320">Type inference ([Type inference](expressions.md#type-inference)) will prefer `dynamic` over `object` if both are candidates.</span></span>

<span data-ttu-id="8d05f-321">この等価性により、次のものが保持されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-321">Because of this equivalence, the following holds:</span></span>

*  <span data-ttu-id="8d05f-322">`object` と `dynamic`間の暗黙的な id 変換と、`dynamic` をに置き換えるときに同じである構築された型の間では、`object`</span><span class="sxs-lookup"><span data-stu-id="8d05f-322">There is an implicit identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing `dynamic` with `object`</span></span>
*  <span data-ttu-id="8d05f-323">`object` との間の暗黙の型変換と明示的な変換は、`dynamic`との間でも適用されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-323">Implicit and explicit conversions to and from `object` also apply to and from `dynamic`.</span></span>
*  <span data-ttu-id="8d05f-324">`dynamic` を `object` に置き換える場合と同じメソッドシグネチャは、同じシグネチャと見なされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-324">Method signatures that are the same when replacing `dynamic` with `object` are considered the same signature</span></span>
*  <span data-ttu-id="8d05f-325">`dynamic` 型は、実行時に `object` と区別できません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-325">The type `dynamic` is indistinguishable from `object` at run-time.</span></span>
*  <span data-ttu-id="8d05f-326">`dynamic` 型の式は、***動的な式***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-326">An expression of the type `dynamic` is referred to as a ***dynamic expression***.</span></span>

### <a name="the-string-type"></a><span data-ttu-id="8d05f-327">文字列型</span><span class="sxs-lookup"><span data-stu-id="8d05f-327">The string type</span></span>

<span data-ttu-id="8d05f-328">`string` 型は、`object`から直接継承するシールクラス型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-328">The `string` type is a sealed class type that inherits directly from `object`.</span></span> <span data-ttu-id="8d05f-329">`string` クラスのインスタンスは、Unicode 文字列を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-329">Instances of the `string` class represent Unicode character strings.</span></span>

<span data-ttu-id="8d05f-330">`string` 型の値は、文字列リテラル ([文字列リテラル](lexical-structure.md#string-literals)) として書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-330">Values of the `string` type can be written as string literals ([String literals](lexical-structure.md#string-literals)).</span></span>

<span data-ttu-id="8d05f-331">キーワード `string` は、定義済みのクラス `System.String`のエイリアスにすぎません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-331">The keyword `string` is simply an alias for the predefined class `System.String`.</span></span>

### <a name="interface-types"></a><span data-ttu-id="8d05f-332">インターフェイス型</span><span class="sxs-lookup"><span data-stu-id="8d05f-332">Interface types</span></span>

<span data-ttu-id="8d05f-333">インターフェイスはコントラクトを定義します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-333">An interface defines a contract.</span></span> <span data-ttu-id="8d05f-334">インターフェイスを実装するクラスまたは構造体は、コントラクトに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-334">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="8d05f-335">インターフェイスは複数の基本インターフェイスから継承でき、クラスまたは構造体は複数のインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-335">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="8d05f-336">インターフェイス型については、「[インターフェイス](interfaces.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-336">Interface types are described in [Interfaces](interfaces.md).</span></span>

### <a name="array-types"></a><span data-ttu-id="8d05f-337">配列型</span><span class="sxs-lookup"><span data-stu-id="8d05f-337">Array types</span></span>

<span data-ttu-id="8d05f-338">配列は、計算されたインデックスを通じてアクセスされる0個以上の変数を含むデータ構造です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-338">An array is a data structure that contains zero or more variables which are accessed through computed indices.</span></span> <span data-ttu-id="8d05f-339">配列に含まれる変数は、配列の要素とも呼ばれ、すべて同じ型であり、この型は配列の要素型と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-339">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="8d05f-340">配列型については、「[配列](arrays.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-340">Array types are described in [Arrays](arrays.md).</span></span>

### <a name="delegate-types"></a><span data-ttu-id="8d05f-341">デリゲート型</span><span class="sxs-lookup"><span data-stu-id="8d05f-341">Delegate types</span></span>

<span data-ttu-id="8d05f-342">デリゲートは、1つ以上のメソッドを参照するデータ構造体です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-342">A delegate is a data structure that refers to one or more methods.</span></span> <span data-ttu-id="8d05f-343">インスタンスメソッドの場合は、対応するオブジェクトインスタンスも参照します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-343">For instance methods, it also refers to their corresponding object instances.</span></span>

<span data-ttu-id="8d05f-344">C またはC++のデリゲートに最も近いものは関数ポインターですが、関数ポインターで参照できるのは静的関数だけですが、デリゲートは静的メソッドとインスタンスメソッドの両方を参照できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-344">The closest equivalent of a delegate in C or C++ is a function pointer, but whereas a function pointer can only reference static functions, a delegate can reference both static and instance methods.</span></span> <span data-ttu-id="8d05f-345">後者の場合、デリゲートは、メソッドのエントリポイントへの参照だけでなく、メソッドを呼び出すオブジェクトインスタンスへの参照も格納します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-345">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance on which to invoke the method.</span></span>

<span data-ttu-id="8d05f-346">デリゲート型については、「[デリゲート](delegates.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-346">Delegate types are described in [Delegates](delegates.md).</span></span>

## <a name="boxing-and-unboxing"></a><span data-ttu-id="8d05f-347">ボックス化とボックス化解除</span><span class="sxs-lookup"><span data-stu-id="8d05f-347">Boxing and unboxing</span></span>

<span data-ttu-id="8d05f-348">ボックス化とボックス化解除の概念はC#、型システムの中心となるものです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-348">The concept of boxing and unboxing is central to C#'s type system.</span></span> <span data-ttu-id="8d05f-349">*Value_type*s と*reference_type*s の間のブリッジを提供します。これにより、 *value_type*の任意の値を型 `object`との間で変換することが許可されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-349">It provides a bridge between *value_type*s and *reference_type*s by permitting any value of a *value_type* to be converted to and from type `object`.</span></span> <span data-ttu-id="8d05f-350">ボックス化とボックス化解除を使用すると、型システムの統一されたビューを使用して、任意の型の値を最終的にオブジェクトとして扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-350">Boxing and unboxing enables a unified view of the type system wherein a value of any type can ultimately be treated as an object.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="8d05f-351">ボックス化変換</span><span class="sxs-lookup"><span data-stu-id="8d05f-351">Boxing conversions</span></span>

<span data-ttu-id="8d05f-352">ボックス化変換は、 *value_type*を暗黙的に*reference_type*に変換することを許可します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-352">A boxing conversion permits a *value_type* to be implicitly converted to a *reference_type*.</span></span> <span data-ttu-id="8d05f-353">次のボックス化変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-353">The following boxing conversions exist:</span></span>

*  <span data-ttu-id="8d05f-354">任意の*value_type*から型 `object`にします。</span><span class="sxs-lookup"><span data-stu-id="8d05f-354">From any *value_type* to the type `object`.</span></span>
*  <span data-ttu-id="8d05f-355">任意の*value_type*から型 `System.ValueType`にします。</span><span class="sxs-lookup"><span data-stu-id="8d05f-355">From any *value_type* to the type `System.ValueType`.</span></span>
*  <span data-ttu-id="8d05f-356">任意の*non_nullable_value_type*から、 *value_type*によって実装されている任意の*interface_type*に。</span><span class="sxs-lookup"><span data-stu-id="8d05f-356">From any *non_nullable_value_type* to any *interface_type* implemented by the *value_type*.</span></span>
*  <span data-ttu-id="8d05f-357">任意の*nullable_type*から、基になる*nullable_type*の型によって実装されている任意の*interface_type*にします。</span><span class="sxs-lookup"><span data-stu-id="8d05f-357">From any *nullable_type* to any *interface_type* implemented by the underlying type of the *nullable_type*.</span></span>
*  <span data-ttu-id="8d05f-358">任意の*enum_type*から型 `System.Enum`にします。</span><span class="sxs-lookup"><span data-stu-id="8d05f-358">From any *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="8d05f-359">基になる*enum_type*を持つ任意の*nullable_type*から `System.Enum`型にします。</span><span class="sxs-lookup"><span data-stu-id="8d05f-359">From any *nullable_type* with an underlying *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="8d05f-360">実行時に、型パラメーターからの暗黙的な変換が、値型から参照型 ([型パラメーター](conversions.md#implicit-conversions-involving-type-parameters)を使用する暗黙的な変換) に変換される場合は、ボックス化変換として実行されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-360">Note that an implicit conversion from a type parameter will be executed as a boxing conversion if at run-time it ends up converting from a value type to a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span>

<span data-ttu-id="8d05f-361">*Non_nullable_value_type*の値をボックス化するには、オブジェクトインスタンスを割り当て、そのインスタンスに*non_nullable_value_type*値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="8d05f-361">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *non_nullable_value_type* value into that instance.</span></span>

<span data-ttu-id="8d05f-362">*Nullable_type*の値をボックス化すると、`null` 値 (`HasValue` が `false`) の場合は null 参照が生成され、それ以外の場合は、基になる値のラップ解除とボックス化の結果になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-362">Boxing a value of a *nullable_type* produces a null reference if it is the `null` value (`HasValue` is `false`), or the result of unwrapping and boxing the underlying value otherwise.</span></span>

<span data-ttu-id="8d05f-363">*Non_nullable_value_type*の値をボックス化する実際のプロセスは、次のように宣言されているかのように動作するジェネリック***ボックス化クラス***の存在を練りすることによって説明します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-363">The actual process of boxing a value of a *non_nullable_value_type* is best explained by imagining the existence of a generic ***boxing class***, which behaves as if it were declared as follows:</span></span>

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

<span data-ttu-id="8d05f-364">`T` 型の値 `v` のボックス化は、式 `new Box<T>(v)`を実行し、結果のインスタンスを `object`型の値として返すようになりました。</span><span class="sxs-lookup"><span data-stu-id="8d05f-364">Boxing of a value `v` of type `T` now consists of executing the expression `new Box<T>(v)`, and returning the resulting instance as a value of type `object`.</span></span> <span data-ttu-id="8d05f-365">したがって、ステートメント</span><span class="sxs-lookup"><span data-stu-id="8d05f-365">Thus, the statements</span></span>
```csharp
int i = 123;
object box = i;
```
<span data-ttu-id="8d05f-366">概念的に対応</span><span class="sxs-lookup"><span data-stu-id="8d05f-366">conceptually correspond to</span></span>
```csharp
int i = 123;
object box = new Box<int>(i);
```

<span data-ttu-id="8d05f-367">上記の `Box<T>` のようなボックス化クラスは実際には存在せず、ボックス化された値の動的な型は実際にはクラス型ではありません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-367">A boxing class like `Box<T>` above doesn't actually exist and the dynamic type of a boxed value isn't actually a class type.</span></span> <span data-ttu-id="8d05f-368">代わりに、型 `T` のボックス化された値には動的な型 `T`があり、`is` 演算子を使用した動的な型チェックでは、単純に型 `T`を参照できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-368">Instead, a boxed value of type `T` has the dynamic type `T`, and a dynamic type check using the `is` operator can simply reference type `T`.</span></span> <span data-ttu-id="8d05f-369">たとえば、オブジェクトに適用された</span><span class="sxs-lookup"><span data-stu-id="8d05f-369">For example,</span></span>
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
<span data-ttu-id="8d05f-370">は、文字列 "`Box contains an int`" をコンソールに出力します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-370">will output the string "`Box contains an int`" on the console.</span></span>

<span data-ttu-id="8d05f-371">ボックス化変換は、ボックス化された値のコピーを作成することを意味します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-371">A boxing conversion implies making a copy of the value being boxed.</span></span> <span data-ttu-id="8d05f-372">これは、 *reference_type*から `object`型への変換とは異なります。この場合、値は引き続き同じインスタンスを参照し、`object`より弱い派生型と見なされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-372">This is different from a conversion of a *reference_type* to type `object`, in which the value continues to reference the same instance and simply is regarded as the less derived type `object`.</span></span> <span data-ttu-id="8d05f-373">たとえば、次のように宣言したとします。</span><span class="sxs-lookup"><span data-stu-id="8d05f-373">For example, given the declaration</span></span>
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
<span data-ttu-id="8d05f-374">次のステートメント</span><span class="sxs-lookup"><span data-stu-id="8d05f-374">the following statements</span></span>
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
<span data-ttu-id="8d05f-375">は、値10をコンソールに出力します。これは、`p` を `box` に割り当てたときに暗黙的なボックス化操作によって `p` の値がコピーされるためです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-375">will output the value 10 on the console because the implicit boxing operation that occurs in the assignment of `p` to `box` causes the value of `p` to be copied.</span></span> <span data-ttu-id="8d05f-376">`p` と `box` が同じインスタンスを参照するため、代わりに `Point` `class` として宣言されています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-376">Had `Point` been declared a `class` instead, the value 20 would be output because `p` and `box` would reference the same instance.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="8d05f-377">ボックス化解除</span><span class="sxs-lookup"><span data-stu-id="8d05f-377">Unboxing conversions</span></span>

<span data-ttu-id="8d05f-378">アンボックス変換は、 *reference_type*を明示的に*value_type*に変換することを許可します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-378">An unboxing conversion permits a *reference_type* to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="8d05f-379">次のボックス化解除変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-379">The following unboxing conversions exist:</span></span>

*  <span data-ttu-id="8d05f-380">型から任意の*value_type*に `object` ます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-380">From the type `object` to any *value_type*.</span></span>
*  <span data-ttu-id="8d05f-381">型から任意の*value_type*に `System.ValueType` ます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-381">From the type `System.ValueType` to any *value_type*.</span></span>
*  <span data-ttu-id="8d05f-382">任意の*interface_type*から、 *interface_type*を実装する任意の*non_nullable_value_type*に。</span><span class="sxs-lookup"><span data-stu-id="8d05f-382">From any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span>
*  <span data-ttu-id="8d05f-383">任意の*interface_type*から、基になる型が*interface_type*を実装している任意の*nullable_type* 。</span><span class="sxs-lookup"><span data-stu-id="8d05f-383">From any *interface_type* to any *nullable_type* whose underlying type implements the *interface_type*.</span></span>
*  <span data-ttu-id="8d05f-384">型から任意の*enum_type*に `System.Enum` ます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-384">From the type `System.Enum` to any *enum_type*.</span></span>
*  <span data-ttu-id="8d05f-385">型から、基になる*enum_type*を持つ任意の*nullable_type*に `System.Enum` ます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-385">From the type `System.Enum` to any *nullable_type* with an underlying *enum_type*.</span></span>
*  <span data-ttu-id="8d05f-386">実行時に参照型から値型に変換すると ([明示的な動的変換](conversions.md#explicit-dynamic-conversions))、型パラメーターへの明示的な変換は、アンボックス変換として実行されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-386">Note that an explicit conversion to a type parameter will be executed as an unboxing conversion if at run-time it ends up converting from a reference type to a value type ([Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)).</span></span>

<span data-ttu-id="8d05f-387">*Non_nullable_value_type*へのボックス化解除操作では、最初に、オブジェクトインスタンスが指定された*non_nullable_value_type*のボックス化された値であることを確認し、次にインスタンスから値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="8d05f-387">An unboxing operation to a *non_nullable_value_type* consists of first checking that the object instance is a boxed value of the given *non_nullable_value_type*, and then copying the value out of the instance.</span></span>

<span data-ttu-id="8d05f-388">*Nullable_type*にボックス化を解除すると、ソースオペランドが `null`場合は*nullable_type*の null 値が生成されます。それ以外の場合は、オブジェクトインスタンスを*nullable_type*の基になる型にボックス化解除した結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-388">Unboxing to a *nullable_type* produces the null value of the *nullable_type* if the source operand is `null`, or the wrapped result of unboxing the object instance to the underlying type of the *nullable_type* otherwise.</span></span>

<span data-ttu-id="8d05f-389">前のセクションで説明した架空のボックス化クラスを参照し、オブジェクト `box` から*value_type* `T` へのアンボックス変換は、`((Box<T>)box).value`式を実行することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-389">Referring to the imaginary boxing class described in the previous section, an unboxing conversion of an object `box` to a *value_type* `T` consists of executing the expression `((Box<T>)box).value`.</span></span> <span data-ttu-id="8d05f-390">したがって、ステートメント</span><span class="sxs-lookup"><span data-stu-id="8d05f-390">Thus, the statements</span></span>
```csharp
object box = 123;
int i = (int)box;
```
<span data-ttu-id="8d05f-391">概念的に対応</span><span class="sxs-lookup"><span data-stu-id="8d05f-391">conceptually correspond to</span></span>
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

<span data-ttu-id="8d05f-392">指定された*non_nullable_value_type*へのアンボックス変換が実行時に成功するようにするには、ソースオペランドの値が、その*non_nullable_value_type*のボックス化された値への参照である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-392">For an unboxing conversion to a given *non_nullable_value_type* to succeed at run-time, the value of the source operand must be a reference to a boxed value of that *non_nullable_value_type*.</span></span> <span data-ttu-id="8d05f-393">ソースオペランドが `null`場合、`System.NullReferenceException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-393">If the source operand is `null`, a `System.NullReferenceException` is thrown.</span></span> <span data-ttu-id="8d05f-394">ソースオペランドが互換性のないオブジェクトへの参照である場合は、`System.InvalidCastException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-394">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="8d05f-395">指定された*nullable_type*へのアンボックス変換が実行時に成功するようにするには、source オペランドの値が `null` か、または*nullable_type*の基になる*non_nullable_value_type*のボックス化された値への参照である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-395">For an unboxing conversion to a given *nullable_type* to succeed at run-time, the value of the source operand must be either `null` or a reference to a boxed value of the underlying *non_nullable_value_type* of the *nullable_type*.</span></span> <span data-ttu-id="8d05f-396">ソースオペランドが互換性のないオブジェクトへの参照である場合は、`System.InvalidCastException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-396">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="8d05f-397">構築された型</span><span class="sxs-lookup"><span data-stu-id="8d05f-397">Constructed types</span></span>

<span data-ttu-id="8d05f-398">ジェネリック型の宣言自体は、***型引数***を適用することによって、さまざまな型を形成するための "ブループリント" として使用される、バインドされていない***ジェネリック型***を表します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-398">A generic type declaration, by itself, denotes an ***unbound generic type*** that is used as a "blueprint" to form many different types, by way of applying ***type arguments***.</span></span> <span data-ttu-id="8d05f-399">型引数は、ジェネリック型の名前の直後に山かっこ (`<` と `>`) で記述されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-399">The type arguments are written within angle brackets (`<` and `>`) immediately following the name of the generic type.</span></span> <span data-ttu-id="8d05f-400">少なくとも1つの型引数を含む型は、構築された***型***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-400">A type that includes at least one type argument is called a ***constructed type***.</span></span> <span data-ttu-id="8d05f-401">構築された型は、型名を表示できる言語のほとんどの場所で使用できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-401">A constructed type can be used in most places in the language in which a type name can appear.</span></span> <span data-ttu-id="8d05f-402">バインドされていないジェネリック型は、 *typeof_expression* ([typeof 演算子](expressions.md#the-typeof-operator)) 内でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-402">An unbound generic type can only be used within a *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

<span data-ttu-id="8d05f-403">構築された型は、単純な名前 ([簡易名](expressions.md#simple-names)) として式で使用することも、メンバー ([メンバーアクセス](expressions.md#member-access)) にアクセスするときに使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-403">Constructed types can also be used in expressions as simple names ([Simple names](expressions.md#simple-names)) or when accessing a member ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="8d05f-404">*Namespace_or_type_name*が評価されると、正しい数の型パラメーターを持つジェネリック型だけが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-404">When a *namespace_or_type_name* is evaluated, only generic types with the correct number of type parameters are considered.</span></span> <span data-ttu-id="8d05f-405">したがって、型の型パラメーターの数が異なる限り、同じ識別子を使用して異なる型を識別することができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-405">Thus, it is possible to use the same identifier to identify different types, as long as the types have different numbers of type parameters.</span></span> <span data-ttu-id="8d05f-406">これは、ジェネリッククラスと非ジェネリッククラスを同じプログラムに混在させる場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-406">This is useful when mixing generic and non-generic classes in the same program:</span></span>

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

<span data-ttu-id="8d05f-407">型パラメーターが直接指定されていない場合でも、 *type_name*は構築された型を識別することがあります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-407">A *type_name* might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="8d05f-408">これは、ジェネリッククラス宣言内で型が入れ子になっている場合に発生する可能性があり、包含する宣言のインスタンス型は、名前参照 ([ジェネリッククラスの入れ子](classes.md#nested-types-in-generic-classes)にされた型) に暗黙的に使用されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-408">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup ([Nested types in generic classes](classes.md#nested-types-in-generic-classes)):</span></span>

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

<span data-ttu-id="8d05f-409">アンセーフコードでは、構築された型を*unmanaged_type* ([ポインター型](unsafe-code.md#pointer-types)) として使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-409">In unsafe code, a constructed type cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

### <a name="type-arguments"></a><span data-ttu-id="8d05f-410">型引数</span><span class="sxs-lookup"><span data-stu-id="8d05f-410">Type arguments</span></span>

<span data-ttu-id="8d05f-411">型引数リストの各引数は単なる*型*です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-411">Each argument in a type argument list is simply a *type*.</span></span>

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

<span data-ttu-id="8d05f-412">アンセーフコード ([unsafe コード](unsafe-code.md)) では、 *type_argument*をポインター型にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-412">In unsafe code ([Unsafe code](unsafe-code.md)), a *type_argument* may not be a pointer type.</span></span> <span data-ttu-id="8d05f-413">それぞれの型引数は、対応する型パラメーター ([型パラメーターの制約](classes.md#type-parameter-constraints)) に対する制約を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-413">Each type argument must satisfy any constraints on the corresponding type parameter ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>

### <a name="open-and-closed-types"></a><span data-ttu-id="8d05f-414">Open 型と closed 型</span><span class="sxs-lookup"><span data-stu-id="8d05f-414">Open and closed types</span></span>

<span data-ttu-id="8d05f-415">すべての型は、***オープン型***または***閉じら***れた型として分類できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-415">All types can be classified as either ***open types*** or ***closed types***.</span></span> <span data-ttu-id="8d05f-416">オープン型は、型パラメーターを含む型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-416">An open type is a type that involves type parameters.</span></span> <span data-ttu-id="8d05f-417">具体的には次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-417">More specifically:</span></span>

*  <span data-ttu-id="8d05f-418">型パラメーターは、オープン型を定義します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-418">A type parameter defines an open type.</span></span>
*  <span data-ttu-id="8d05f-419">配列型は、要素型がオープン型である場合にのみ、オープン型になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-419">An array type is an open type if and only if its element type is an open type.</span></span>
*  <span data-ttu-id="8d05f-420">構築された型は、型引数の1つ以上がオープン型である場合にのみ、オープン型になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-420">A constructed type is an open type if and only if one or more of its type arguments is an open type.</span></span> <span data-ttu-id="8d05f-421">構築された入れ子になった型は、その型引数の1つ以上がオープン型である場合にのみ、オープン型になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-421">A constructed nested type is an open type if and only if one or more of its type arguments or the type arguments of its containing type(s) is an open type.</span></span>

<span data-ttu-id="8d05f-422">閉じられた型は、オープン型ではない型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-422">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="8d05f-423">実行時には、ジェネリック型宣言内のすべてのコードが、ジェネリック宣言に型引数を適用することによって作成されたクローズ構築型のコンテキストで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-423">At run-time, all of the code within a generic type declaration is executed in the context of a closed constructed type that was created by applying type arguments to the generic declaration.</span></span> <span data-ttu-id="8d05f-424">ジェネリック型内の各型パラメーターは、特定の実行時の型にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-424">Each type parameter within the generic type is bound to a particular run-time type.</span></span> <span data-ttu-id="8d05f-425">すべてのステートメントおよび式の実行時処理は常に閉じられた型で発生し、オープン型はコンパイル時の処理中にのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-425">The run-time processing of all statements and expressions always occurs with closed types, and open types occur only during compile-time processing.</span></span>

<span data-ttu-id="8d05f-426">閉じられた構築型にはそれぞれ、静的な変数のセットがあります。これは、その他の閉じた構築型とは共有されません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-426">Each closed constructed type has its own set of static variables, which are not shared with any other closed constructed types.</span></span> <span data-ttu-id="8d05f-427">オープン型は実行時に存在しないため、オープン型に関連付けられた静的変数はありません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-427">Since an open type does not exist at run-time, there are no static variables associated with an open type.</span></span> <span data-ttu-id="8d05f-428">2つの閉じられた構築型は、同じ非バインドジェネリック型から構築され、それらに対応する型引数が同じ型である場合、同じ型になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-428">Two closed constructed types are the same type if they are constructed from the same unbound generic type, and their corresponding type arguments are the same type.</span></span>

### <a name="bound-and-unbound-types"></a><span data-ttu-id="8d05f-429">バインドおよびバインド解除された型</span><span class="sxs-lookup"><span data-stu-id="8d05f-429">Bound and unbound types</span></span>

<span data-ttu-id="8d05f-430">バインドされていない***型***とは、非ジェネリック型またはバインドされていないジェネリック型を指します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-430">The term ***unbound type*** refers to a non-generic type or an unbound generic type.</span></span> <span data-ttu-id="8d05f-431">"バインドされた***型***" とは、非ジェネリック型または構築された型を指します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-431">The term ***bound type*** refers to a non-generic type or a constructed type.</span></span>

<span data-ttu-id="8d05f-432">バインドされていない型は、型宣言によって宣言されたエンティティを参照します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-432">An unbound type refers to the entity declared by a type declaration.</span></span> <span data-ttu-id="8d05f-433">バインドされていないジェネリック型はそれ自体が型ではなく、変数、引数、または戻り値の型として、または基本型として使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-433">An unbound generic type is not itself a type, and cannot be used as the type of a variable, argument or return value, or as a base type.</span></span> <span data-ttu-id="8d05f-434">バインドされていないジェネリック型を参照できる唯一のコンストラクトは、`typeof` 式 ([typeof 演算子](expressions.md#the-typeof-operator)) です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-434">The only construct in which an unbound generic type can be referenced is the `typeof` expression ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

### <a name="satisfying-constraints"></a><span data-ttu-id="8d05f-435">制約を満たす</span><span class="sxs-lookup"><span data-stu-id="8d05f-435">Satisfying constraints</span></span>

<span data-ttu-id="8d05f-436">構築された型またはジェネリックメソッドが参照されるたびに、指定された型引数は、ジェネリック型またはジェネリックメソッド ([型パラメーターの制約](classes.md#type-parameter-constraints)) で宣言された型パラメーターの制約に照らしてチェックされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-436">Whenever a constructed type or generic method is referenced, the supplied type arguments are checked against the type parameter constraints declared on the generic type or method ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="8d05f-437">`where` 句ごとに、名前付きの型パラメーターに対応する型引数 `A` が、次のように各制約に対してチェックされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-437">For each `where` clause, the type argument `A` that corresponds to the named type parameter is checked against each constraint as follows:</span></span>

*  <span data-ttu-id="8d05f-438">制約がクラス型、インターフェイス型、または型パラメーターである場合、`C` は、制約に含まれる型パラメーターの代わりに指定された型引数を持つ制約を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-438">If the constraint is a class type, an interface type, or a type parameter, let `C` represent that constraint with the supplied type arguments substituted for any type parameters that appear in the constraint.</span></span> <span data-ttu-id="8d05f-439">制約を満たすには、型 `A` が、次のいずれかによって `C` 型に変換可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-439">To satisfy the constraint, it must be the case that type `A` is convertible to type `C` by one of the following:</span></span>
    * <span data-ttu-id="8d05f-440">Id 変換 ([id 変換](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="8d05f-440">An identity conversion ([Identity conversion](conversions.md#identity-conversion))</span></span>
    * <span data-ttu-id="8d05f-441">暗黙の参照変換 ([暗黙的な参照](conversions.md#implicit-reference-conversions)変換)</span><span class="sxs-lookup"><span data-stu-id="8d05f-441">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
    * <span data-ttu-id="8d05f-442">型 A が null 非許容の値型である場合、ボックス化変換 ([ボックス](conversions.md#boxing-conversions)化変換)。</span><span class="sxs-lookup"><span data-stu-id="8d05f-442">A boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)), provided that type A is a non-nullable value type.</span></span>
    * <span data-ttu-id="8d05f-443">型パラメーター `A` から `C`への暗黙的な参照、ボックス化、または型パラメーターの変換。</span><span class="sxs-lookup"><span data-stu-id="8d05f-443">An implicit reference, boxing or type parameter conversion from a type parameter `A` to `C`.</span></span>
*  <span data-ttu-id="8d05f-444">制約が参照型制約 (`class`) の場合、型 `A` は次のいずれかを満たしている必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-444">If the constraint is the reference type constraint (`class`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="8d05f-445">`A` は、インターフェイス型、クラス型、デリゲート型、または配列型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-445">`A` is an interface type, class type, delegate type or array type.</span></span> <span data-ttu-id="8d05f-446">`System.ValueType` と `System.Enum` は、この制約を満たす参照型であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-446">Note that `System.ValueType` and `System.Enum` are reference types that satisfy this constraint.</span></span>
    * <span data-ttu-id="8d05f-447">`A` は、参照型 ([型パラメーターの制約](classes.md#type-parameter-constraints)) であることがわかっている型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-447">`A` is a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="8d05f-448">制約が値型制約 (`struct`) の場合、型 `A` は次のいずれかを満たしている必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-448">If the constraint is the value type constraint (`struct`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="8d05f-449">`A` は構造体型または列挙型ですが、null 許容型ではありません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-449">`A` is a struct type or enum type, but not a nullable type.</span></span> <span data-ttu-id="8d05f-450">`System.ValueType` と `System.Enum` は、この制約を満たしていない参照型であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-450">Note that `System.ValueType` and `System.Enum` are reference types that do not satisfy this constraint.</span></span>
    * <span data-ttu-id="8d05f-451">`A` は、値型の制約 ([型パラメーターの制約](classes.md#type-parameter-constraints)) を持つ型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-451">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="8d05f-452">制約がコンストラクターの制約 `new()`の場合は、型 `A` を `abstract` せずに、パブリックなパラメーターなしのコンストラクターを持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-452">If the constraint is the constructor constraint `new()`, the type `A` must not be `abstract` and must have a public parameterless constructor.</span></span> <span data-ttu-id="8d05f-453">これは、次のいずれかに該当する場合に満たされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-453">This is satisfied if one of the following is true:</span></span>
    * <span data-ttu-id="8d05f-454">すべての値型にはパブリックな既定のコンストラクター ([既定のコンストラクター](types.md#default-constructors)) があるため、`A` は値型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-454">`A` is a value type, since all value types have a public default constructor ([Default constructors](types.md#default-constructors)).</span></span>
    * <span data-ttu-id="8d05f-455">`A` は、コンストラクターの制約 ([型パラメーターの制約](classes.md#type-parameter-constraints)) を持つ型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-455">`A` is a type parameter having the constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="8d05f-456">`A` は、値型の制約 ([型パラメーターの制約](classes.md#type-parameter-constraints)) を持つ型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="8d05f-456">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="8d05f-457">`A` は `abstract` ないクラスであり、パラメーターを持たない明示的に宣言された `public` コンストラクターを含んでいます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-457">`A` is a class that is not `abstract` and contains an explicitly declared `public` constructor with no parameters.</span></span>
    * <span data-ttu-id="8d05f-458">`A` が `abstract` ではなく、既定のコンストラクター ([既定のコンストラクター](classes.md#default-constructors)) を持っています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-458">`A` is not `abstract` and has a default constructor ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="8d05f-459">指定された型引数によって1つ以上の型パラメーターの制約が満たされない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-459">A compile-time error occurs if one or more of a type parameter's constraints are not satisfied by the given type arguments.</span></span>

<span data-ttu-id="8d05f-460">型パラメーターは継承されないため、制約は継承されません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-460">Since type parameters are not inherited, constraints are never inherited either.</span></span> <span data-ttu-id="8d05f-461">次の例では、`T` が基本クラス `B<T>`によって課される制約を満たすように、型パラメーター `T` に対して制約を指定する必要があり `D`。</span><span class="sxs-lookup"><span data-stu-id="8d05f-461">In the example below, `D` needs to specify the constraint on its type parameter `T` so that `T` satisfies the constraint imposed by the base class `B<T>`.</span></span> <span data-ttu-id="8d05f-462">一方、`List<T>` は `T`に `IEnumerable` を実装しているため、クラス `E` では制約を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-462">In contrast, class `E` need not specify a constraint, because `List<T>` implements `IEnumerable` for any `T`.</span></span>

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a><span data-ttu-id="8d05f-463">型パラメーター</span><span class="sxs-lookup"><span data-stu-id="8d05f-463">Type parameters</span></span>

<span data-ttu-id="8d05f-464">型パラメーターは、実行時にパラメーターがバインドされる値型または参照型を指定する識別子です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-464">A type parameter is an identifier designating a value type or reference type that the parameter is bound to at run-time.</span></span>

```antlr
type_parameter
    : identifier
    ;
```

<span data-ttu-id="8d05f-465">型パラメーターは、さまざまな異なる実際の型引数を使用してインスタンス化できるため、型パラメーターには、他の型とは少し異なる操作と制限があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-465">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types.</span></span> <span data-ttu-id="8d05f-466">次の設定があります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-466">These include:</span></span>

*  <span data-ttu-id="8d05f-467">型パラメーターを直接使用して基底クラス ([基底クラス](classes.md#base-class)) またはインターフェイス ([バリアント型パラメーターリスト](interfaces.md#variant-type-parameter-lists)) を宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-467">A type parameter cannot be used directly to declare a base class ([Base class](classes.md#base-class)) or interface ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)).</span></span>
*  <span data-ttu-id="8d05f-468">型パラメーターに対するメンバー参照の規則は、型パラメーターに適用される制約によって異なります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-468">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="8d05f-469">これらの詳細については、「[メンバー検索](expressions.md#member-lookup)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-469">They are detailed in [Member lookup](expressions.md#member-lookup).</span></span>
*  <span data-ttu-id="8d05f-470">型パラメーターに使用できる変換は、型パラメーターに適用される制約によって異なります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-470">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="8d05f-471">これらの詳細については、型パラメーターと[明示的な動的変換](conversions.md#explicit-dynamic-conversions)を[含む暗黙の型変換](conversions.md#implicit-conversions-involving-type-parameters)について説明します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-471">They are detailed in [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions).</span></span>
*  <span data-ttu-id="8d05f-472">型パラメーターが参照型であることがわかっている場合を除き、リテラル `null` を型パラメーターによって指定された型に変換することはできません ([型パラメーター](conversions.md#implicit-conversions-involving-type-parameters)を使用する暗黙的な変換)。</span><span class="sxs-lookup"><span data-stu-id="8d05f-472">The literal `null` cannot be converted to a type given by a type parameter, except if the type parameter is known to be a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span> <span data-ttu-id="8d05f-473">ただし、代わりに `default` 式 ([既定値の式](expressions.md#default-value-expressions)) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-473">However, a `default` expression ([Default value expressions](expressions.md#default-value-expressions)) can be used instead.</span></span> <span data-ttu-id="8d05f-474">さらに、型パラメーターによって指定された型の値は、型パラメーターに値型の制約がない限り、`==` と `!=` ([参照型の等値演算子](expressions.md#reference-type-equality-operators)) を使用して、`null` と比較できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-474">In addition, a value with a type given by a type parameter can be compared with `null` using `==` and `!=` ([Reference type equality operators](expressions.md#reference-type-equality-operators)) unless the type parameter has the value type constraint.</span></span>
*  <span data-ttu-id="8d05f-475">`new` 式 ([オブジェクト作成式](expressions.md#object-creation-expressions)) は、型パラメーターが*constructor_constraint*または値型の制約 ([型パラメーターの制約](classes.md#type-parameter-constraints)) によって制約されている場合にのみ、型パラメーターと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-475">A `new` expression ([Object creation expressions](expressions.md#object-creation-expressions)) can only be used with a type parameter if the type parameter is constrained by a *constructor_constraint* or the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="8d05f-476">型パラメーターは、属性内の任意の場所で使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-476">A type parameter cannot be used anywhere within an attribute.</span></span>
*  <span data-ttu-id="8d05f-477">静的メンバーまたは入れ子にされた型を識別するために、型パラメーターをメンバーアクセス ([メンバーアクセス](expressions.md#member-access)) または型名 ([名前空間と型名](basic-concepts.md#namespace-and-type-names)) で使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-477">A type parameter cannot be used in a member access ([Member access](expressions.md#member-access)) or type name ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) to identify a static member or a nested type.</span></span>
*  <span data-ttu-id="8d05f-478">アンセーフコードでは、型パラメーターを*unmanaged_type* ([ポインター型](unsafe-code.md#pointer-types)) として使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-478">In unsafe code, a type parameter cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

<span data-ttu-id="8d05f-479">型として、型パラメーターは純粋にコンパイル時の構成要素です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-479">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="8d05f-480">実行時に、各型パラメーターは、ジェネリック型宣言に型引数を指定して指定されたランタイム型にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-480">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic type declaration.</span></span> <span data-ttu-id="8d05f-481">したがって、型パラメーターを使用して宣言された変数の型は、実行時にクローズ構築型 ([オープン型およびクローズ型](types.md#open-and-closed-types)) になります。</span><span class="sxs-lookup"><span data-stu-id="8d05f-481">Thus, the type of a variable declared with a type parameter will, at run-time, be a closed constructed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="8d05f-482">型パラメーターを含むすべてのステートメントおよび式の実行時の実行では、そのパラメーターの型引数として指定された実際の型を使用します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-482">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>

## <a name="expression-tree-types"></a><span data-ttu-id="8d05f-483">式ツリー型</span><span class="sxs-lookup"><span data-stu-id="8d05f-483">Expression tree types</span></span>

<span data-ttu-id="8d05f-484">***式ツリー***では、ラムダ式を実行可能コードではなくデータ構造として表すことができます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-484">***Expression trees*** permit lambda expressions to be represented as data structures instead of executable code.</span></span> <span data-ttu-id="8d05f-485">式ツリーは、`System.Linq.Expressions.Expression<D>`形式の***式ツリー型***の値です。 `D` は任意のデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-485">Expression trees are values of ***expression tree types*** of the form `System.Linq.Expressions.Expression<D>`, where `D` is any delegate type.</span></span> <span data-ttu-id="8d05f-486">この仕様の残りの部分では、短縮形 `Expression<D>`を使用してこれらの型を参照します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-486">For the remainder of this specification we will refer to these types using the shorthand `Expression<D>`.</span></span>

<span data-ttu-id="8d05f-487">ラムダ式から `D`デリゲート型への変換が存在する場合、式ツリー型 `Expression<D>`にも変換されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-487">If a conversion exists from a lambda expression to a delegate type `D`, a conversion also exists to the expression tree type `Expression<D>`.</span></span> <span data-ttu-id="8d05f-488">ラムダ式からデリゲート型への変換では、ラムダ式の実行可能コードを参照するデリゲートが生成されますが、式ツリー型への変換では、ラムダ式の式ツリー表現が作成されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-488">Whereas the conversion of a lambda expression to a delegate type generates a delegate that references executable code for the lambda expression, conversion to an expression tree type creates an expression tree representation of the lambda expression.</span></span>

<span data-ttu-id="8d05f-489">式ツリーは、ラムダ式の効率的なインメモリデータ表現であり、ラムダ式の構造を透過的かつ明示的にします。</span><span class="sxs-lookup"><span data-stu-id="8d05f-489">Expression trees are efficient in-memory data representations of lambda expressions and make the structure of the lambda expression transparent and explicit.</span></span>

<span data-ttu-id="8d05f-490">デリゲート型 `D`と同様に、`Expression<D>` は `D`のパラメーターと戻り値の型と同じように扱われます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-490">Just like a delegate type `D`, `Expression<D>` is said to have parameter and return types, which are the same as those of `D`.</span></span>

<span data-ttu-id="8d05f-491">次の例は、ラムダ式を実行可能コードおよび式ツリーとして表しています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-491">The following example represents a lambda expression both as executable code and as an expression tree.</span></span> <span data-ttu-id="8d05f-492">`Func<int,int>`への変換が存在するため、`Expression<Func<int,int>>`にも変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-492">Because a conversion exists to `Func<int,int>`, a conversion also exists to `Expression<Func<int,int>>`:</span></span>

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

<span data-ttu-id="8d05f-493">これらの代入の後、デリゲート `del` は `x + 1`を返すメソッドを参照し、式ツリー `exp` は式 `x => x + 1`を記述するデータ構造を参照します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-493">Following these assignments, the delegate `del` references a method that returns `x + 1`, and the expression tree `exp` references a data structure that describes the expression `x => x + 1`.</span></span>

<span data-ttu-id="8d05f-494">ジェネリック型 `Expression<D>` の正確な定義と、ラムダ式が式ツリー型に変換されるときに式ツリーを構築するための正確な規則は、両方ともこの仕様の範囲外です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-494">The exact definition of the generic type `Expression<D>` as well as the precise rules for constructing an expression tree when a lambda expression is converted to an expression tree type, are both outside the scope of this specification.</span></span>

<span data-ttu-id="8d05f-495">明示するには、次の2つのことが重要です。</span><span class="sxs-lookup"><span data-stu-id="8d05f-495">Two things are important to make explicit:</span></span>

*  <span data-ttu-id="8d05f-496">すべてのラムダ式を式ツリーに変換することはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-496">Not all lambda expressions can be converted to expression trees.</span></span> <span data-ttu-id="8d05f-497">たとえば、ステートメント本体を含むラムダ式や、代入式を含むラムダ式を表すことはできません。</span><span class="sxs-lookup"><span data-stu-id="8d05f-497">For instance, lambda expressions with statement bodies, and lambda expressions containing assignment expressions cannot be represented.</span></span> <span data-ttu-id="8d05f-498">このような場合は、変換はまだ存在しますが、コンパイル時に失敗します。</span><span class="sxs-lookup"><span data-stu-id="8d05f-498">In these cases, a conversion still exists, but will fail at compile-time.</span></span> <span data-ttu-id="8d05f-499">これらの例外の詳細については、「[匿名関数変換](conversions.md#anonymous-function-conversions)」を参考にしてください。</span><span class="sxs-lookup"><span data-stu-id="8d05f-499">These exceptions are detailed in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>
*   <span data-ttu-id="8d05f-500">`Expression<D>` には、`D`型のデリゲートを生成するインスタンスメソッド `Compile` が用意されています。</span><span class="sxs-lookup"><span data-stu-id="8d05f-500">`Expression<D>` offers an instance method `Compile` which produces a delegate of type `D`:</span></span>

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    <span data-ttu-id="8d05f-501">このデリゲートを呼び出すと、式ツリーによって表されるコードが実行されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-501">Invoking this delegate causes the code represented by the expression tree to be executed.</span></span> <span data-ttu-id="8d05f-502">したがって、上記の定義を指定した場合、del と del2 は同等であり、次の2つのステートメントは同じ効果を持ちます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-502">Thus, given the definitions above, del and del2 are equivalent, and the following two statements will have the same effect:</span></span>

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    <span data-ttu-id="8d05f-503">このコードを実行すると、`i1` と `i2` の両方に `2`値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="8d05f-503">After executing this code,  `i1` and `i2` will both have the value `2`.</span></span>

