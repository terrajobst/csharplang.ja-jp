---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488841"
---
# <a name="arrays"></a><span data-ttu-id="7c83d-101">配列</span><span class="sxs-lookup"><span data-stu-id="7c83d-101">Arrays</span></span>

<span data-ttu-id="7c83d-102">配列は、さまざまな算出されたインデックスを介してアクセスする変数を含むデータ構造です。</span><span class="sxs-lookup"><span data-stu-id="7c83d-102">An array is a data structure that contains a number of variables which are accessed through computed indices.</span></span> <span data-ttu-id="7c83d-103">配列の要素とも呼ばれます。 配列に含まれる変数はすべて、同じ型と、この型には、配列の要素の型が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-103">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="7c83d-104">配列には、配列の各要素に関連付けられているインデックスの数を決定するランクがあります。</span><span class="sxs-lookup"><span data-stu-id="7c83d-104">An array has a rank which determines the number of indices associated with each array element.</span></span> <span data-ttu-id="7c83d-105">配列のランクは、配列のディメンションであるとも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-105">The rank of an array is also referred to as the dimensions of the array.</span></span> <span data-ttu-id="7c83d-106">ランクが 1 の配列と呼ばれる、 ***1 次元配列***します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-106">An array with a rank of one is called a ***single-dimensional array***.</span></span> <span data-ttu-id="7c83d-107">配列と呼ばれる 1 つより大きいランクを持つ、***多次元配列***します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-107">An array with a rank greater than one is called a ***multi-dimensional array***.</span></span> <span data-ttu-id="7c83d-108">特定のサイズが設定された多次元配列は、2 次元配列や 3 次元の配列とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-108">Specific sized multi-dimensional arrays are often referred to as two-dimensional arrays, three-dimensional arrays, and so on.</span></span>

<span data-ttu-id="7c83d-109">配列の各次元が、関連付けられた長さはゼロに整数の数より大きいか等しいです。</span><span class="sxs-lookup"><span data-stu-id="7c83d-109">Each dimension of an array has an associated length which is an integral number greater than or equal to zero.</span></span> <span data-ttu-id="7c83d-110">次元の長さは、配列の型の一部ではありませんが、配列型のインスタンスが実行時に作成されたときに確立されます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-110">The dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run-time.</span></span> <span data-ttu-id="7c83d-111">次元の長さは、その次元のインデックスの有効な範囲を決定します。長さのディメンションの`N`、インデックスの範囲は`0`に`N - 1`包括的です。</span><span class="sxs-lookup"><span data-stu-id="7c83d-111">The length of a dimension determines the valid range of indices for that dimension: For a dimension of length `N`, indices can range from `0` to `N - 1` inclusive.</span></span> <span data-ttu-id="7c83d-112">配列内の要素の合計数は、配列の各次元の長さの製品です。</span><span class="sxs-lookup"><span data-stu-id="7c83d-112">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="7c83d-113">1 つ以上の配列の次元の長さ 0 の場合、配列は空にすることはできます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-113">If one or more of the dimensions of an array have a length of zero, the array is said to be empty.</span></span>

<span data-ttu-id="7c83d-114">配列の要素型には、配列型を含む任意の型を指定できます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-114">The element type of an array can be any type, including an array type.</span></span>

## <a name="array-types"></a><span data-ttu-id="7c83d-115">配列型</span><span class="sxs-lookup"><span data-stu-id="7c83d-115">Array types</span></span>

<span data-ttu-id="7c83d-116">配列型として書き込まれます、 *non_array_type* 1 つまたは複数続く*rank_specifier*: %s</span><span class="sxs-lookup"><span data-stu-id="7c83d-116">An array type is written as a *non_array_type* followed by one or more *rank_specifier*s:</span></span>

```antlr
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
```

<span data-ttu-id="7c83d-117">A *non_array_type*は any*型*いない自体は、 *array_type*します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-117">A *non_array_type* is any *type* that is not itself an *array_type*.</span></span>

<span data-ttu-id="7c83d-118">配列型のランクが左端で指定された*rank_specifier*で、 *array_type*:A *rank_specifier*配列が 1 を加えた数のランクを持つ配列であることを示します"`,`"のトークン、 *rank_specifier*します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-118">The rank of an array type is given by the leftmost *rank_specifier* in the *array_type*: A *rank_specifier* indicates that the array is an array with a rank of one plus the number of "`,`" tokens in the *rank_specifier*.</span></span>

<span data-ttu-id="7c83d-119">配列型の要素型が、左端を削除した結果を型*rank_specifier*:</span><span class="sxs-lookup"><span data-stu-id="7c83d-119">The element type of an array type is the type that results from deleting the leftmost *rank_specifier*:</span></span>

*  <span data-ttu-id="7c83d-120">フォームの配列型`T[R]`配列のランクを持つ`R`と非配列要素型を`T`します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-120">An array type of the form `T[R]` is an array with rank `R` and a non-array element type `T`.</span></span>
*  <span data-ttu-id="7c83d-121">フォームの配列型`T[R][R1]...[Rn]`配列のランクを持つ`R`と要素型`T[R1]...[Rn]`します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-121">An array type of the form `T[R][R1]...[Rn]` is an array with rank `R` and an element type `T[R1]...[Rn]`.</span></span>

<span data-ttu-id="7c83d-122">実際には、 *rank_specifier*s は、非配列の最後の要素型の前に右に左から読み取られます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-122">In effect, the *rank_specifier*s are read from left to right before the final non-array element type.</span></span> <span data-ttu-id="7c83d-123">型`int[][,,][,]`の 2 次元の配列の 3 次元の配列の 1 次元配列は、`int`します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-123">The type `int[][,,][,]` is a single-dimensional array of three-dimensional arrays of two-dimensional arrays of `int`.</span></span>

<span data-ttu-id="7c83d-124">実行時に、配列型の値を指定できます`null`またはその配列型のインスタンスへの参照。</span><span class="sxs-lookup"><span data-stu-id="7c83d-124">At run-time, a value of an array type can be `null` or a reference to an instance of that array type.</span></span>

### <a name="the-systemarray-type"></a><span data-ttu-id="7c83d-125">System.Array 型</span><span class="sxs-lookup"><span data-stu-id="7c83d-125">The System.Array type</span></span>

<span data-ttu-id="7c83d-126">型`System.Array`はすべての配列型の抽象基本型です。</span><span class="sxs-lookup"><span data-stu-id="7c83d-126">The type `System.Array` is the abstract base type of all array types.</span></span> <span data-ttu-id="7c83d-127">暗黙の参照変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions)) から任意の配列型に存在する`System.Array`、および明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) が存在します。`System.Array`任意の配列型にします。</span><span class="sxs-lookup"><span data-stu-id="7c83d-127">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from any array type to `System.Array`, and an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `System.Array` to any array type.</span></span> <span data-ttu-id="7c83d-128">なお`System.Array`自体ではありません、 *array_type*します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-128">Note that `System.Array` is not itself an *array_type*.</span></span> <span data-ttu-id="7c83d-129">*Class_type*すべてから*array_type*s が派生します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-129">Rather, it is a *class_type* from which all *array_type*s are derived.</span></span>

<span data-ttu-id="7c83d-130">実行時の型の値に`System.Array`できる`null`または任意の配列型のインスタンスへの参照。</span><span class="sxs-lookup"><span data-stu-id="7c83d-130">At run-time, a value of type `System.Array` can be `null` or a reference to an instance of any array type.</span></span>

### <a name="arrays-and-the-generic-ilist-interface"></a><span data-ttu-id="7c83d-131">配列とジェネリックの IList インターフェイス</span><span class="sxs-lookup"><span data-stu-id="7c83d-131">Arrays and the generic IList interface</span></span>

<span data-ttu-id="7c83d-132">1 次元配列`T[]`インターフェイスを実装する`System.Collections.Generic.IList<T>`(`IList<T>`略して) とその基本インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="7c83d-132">A one-dimensional array `T[]` implements the interface `System.Collections.Generic.IList<T>` (`IList<T>` for short) and its base interfaces.</span></span> <span data-ttu-id="7c83d-133">したがってからの暗黙的な変換がある`T[]`に`IList<T>`とその基本インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="7c83d-133">Accordingly, there is an implicit conversion from `T[]` to `IList<T>` and its base interfaces.</span></span> <span data-ttu-id="7c83d-134">さらからの暗黙的な参照変換がある場合`S`に`T`し`S[]`実装`IList<T>`から暗黙の参照変換があると`S[]`に`IList<T>`およびその基本インターフェイス ([暗黙の参照変換](conversions.md#implicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="7c83d-134">In addition, if there is an implicit reference conversion from `S` to `T` then `S[]` implements `IList<T>` and there is an implicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="7c83d-135">明示的な参照変換がある場合`S`に`T`からの明示的な参照変換は`S[]`に`IList<T>`とその基本インターフェイス ([明示的な参照変換](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="7c83d-135">If there is an explicit reference conversion from `S` to `T` then there is an explicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="7c83d-136">例えば:</span><span class="sxs-lookup"><span data-stu-id="7c83d-136">For example:</span></span>
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

<span data-ttu-id="7c83d-137">割り当て`lst2 = oa1`以降からの変換、コンパイル時エラーが発生`object[]`に`IList<string>`はいない暗黙的な明示的な変換をします。</span><span class="sxs-lookup"><span data-stu-id="7c83d-137">The assignment `lst2 = oa1` generates a compile-time error since the conversion from `object[]` to `IList<string>` is an explicit conversion, not implicit.</span></span> <span data-ttu-id="7c83d-138">キャスト`(IList<string>)oa1`が以降の実行時にスローされる例外が発生`oa1`参照、`object[]`および not、`string[]`します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-138">The cast `(IList<string>)oa1` will cause an exception to be thrown at run-time since `oa1` references an `object[]` and not a `string[]`.</span></span> <span data-ttu-id="7c83d-139">ただし、キャスト`(IList<string>)oa2`からスローされる例外は発生しません`oa2`参照、`string[]`します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-139">However the cast `(IList<string>)oa2` will not cause an exception to be thrown since `oa2` references a `string[]`.</span></span>

<span data-ttu-id="7c83d-140">暗黙的または明示的な参照変換が存在する場合は`S[]`に`IList<T>`からの明示的な参照変換も`IList<T>`その基本インターフェイスと`S[]`([明示的な参照変換](conversions.md#explicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="7c83d-140">Whenever there is an implicit or explicit reference conversion from `S[]` to `IList<T>`, there is also an explicit reference conversion from `IList<T>` and its base interfaces to `S[]` ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span>

<span data-ttu-id="7c83d-141">配列型`S[]`実装`IList<T>`、実装されたインターフェイスのメンバーの一部の例外をスローする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7c83d-141">When an array type `S[]` implements `IList<T>`, some of the members of the implemented interface may throw exceptions.</span></span> <span data-ttu-id="7c83d-142">インターフェイスの実装の動作の詳細についてはこの仕様の範囲外です。</span><span class="sxs-lookup"><span data-stu-id="7c83d-142">The precise behavior of the implementation of the interface is beyond the scope of this specification.</span></span>

## <a name="array-creation"></a><span data-ttu-id="7c83d-143">配列の作成</span><span class="sxs-lookup"><span data-stu-id="7c83d-143">Array creation</span></span>

<span data-ttu-id="7c83d-144">配列のインスタンスがによって作成された*array_creation_expression*s ([配列作成式](expressions.md#array-creation-expressions)) フィールド、ローカル変数の宣言が含まれるかを*array_initializer*([配列初期化子](arrays.md#array-initializers))。</span><span class="sxs-lookup"><span data-stu-id="7c83d-144">Array instances are created by *array_creation_expression*s ([Array creation expressions](expressions.md#array-creation-expressions)) or by field or local variable declarations that include an *array_initializer* ([Array initializers](arrays.md#array-initializers)).</span></span>

<span data-ttu-id="7c83d-145">配列インスタンスが作成されるが確立されているランクと各次元の長さと、インスタンスの有効期間にわたって一定です。</span><span class="sxs-lookup"><span data-stu-id="7c83d-145">When an array instance is created, the rank and length of each dimension are established and then remain constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="7c83d-146">つまり、既存の配列インスタンスのランクを変更することはできませんも、そのディメンションのサイズを変更することはできます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-146">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

<span data-ttu-id="7c83d-147">配列型の配列インスタンスは常にです。</span><span class="sxs-lookup"><span data-stu-id="7c83d-147">An array instance is always of an array type.</span></span> <span data-ttu-id="7c83d-148">`System.Array`型が抽象型をインスタンス化することはできません。</span><span class="sxs-lookup"><span data-stu-id="7c83d-148">The `System.Array` type is an abstract type that cannot be instantiated.</span></span>

<span data-ttu-id="7c83d-149">によって作成された配列の要素*array_creation_expression*s は必ずが既定値に初期化 ([既定値](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="7c83d-149">Elements of arrays created by *array_creation_expression*s are always initialized to their default value ([Default values](variables.md#default-values)).</span></span>

## <a name="array-element-access"></a><span data-ttu-id="7c83d-150">配列要素へのアクセス</span><span class="sxs-lookup"><span data-stu-id="7c83d-150">Array element access</span></span>

<span data-ttu-id="7c83d-151">配列の要素が使用してアクセス*element_access*式 ([配列アクセス](expressions.md#array-access)) フォームの`A[I1, I2, ..., In]`ここで、`A`配列型とそれぞれの式を指定`Ix`が、型の式`int`、 `uint`、 `long`、 `ulong`、または 1 つ以上のこれらの型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-151">Array elements are accessed using *element_access* expressions ([Array access](expressions.md#array-access)) of the form `A[I1, I2, ..., In]`, where `A` is an expression of an array type and each `Ix` is an expression of type `int`, `uint`, `long`, `ulong`, or can be implicitly converted to one or more of these types.</span></span> <span data-ttu-id="7c83d-152">配列要素へのアクセスの結果は、変数、インデックスによって選択されている配列要素つまりです。</span><span class="sxs-lookup"><span data-stu-id="7c83d-152">The result of an array element access is a variable, namely the array element selected by the indices.</span></span>

<span data-ttu-id="7c83d-153">使用して、配列の要素を列挙することができます、`foreach`ステートメント ([foreach ステートメント](statements.md#the-foreach-statement))。</span><span class="sxs-lookup"><span data-stu-id="7c83d-153">The elements of an array can be enumerated using a `foreach` statement ([The foreach statement](statements.md#the-foreach-statement)).</span></span>

## <a name="array-members"></a><span data-ttu-id="7c83d-154">配列メンバー</span><span class="sxs-lookup"><span data-stu-id="7c83d-154">Array members</span></span>

<span data-ttu-id="7c83d-155">すべての配列型で宣言されたメンバーの継承、`System.Array`型。</span><span class="sxs-lookup"><span data-stu-id="7c83d-155">Every array type inherits the members declared by the `System.Array` type.</span></span>

## <a name="array-covariance"></a><span data-ttu-id="7c83d-156">配列の共変性</span><span class="sxs-lookup"><span data-stu-id="7c83d-156">Array covariance</span></span>

<span data-ttu-id="7c83d-157">任意の 2 つの*reference_type*s`A`と`B`暗黙の参照変換の場合は、([暗黙の参照変換](conversions.md#implicit-reference-conversions)) または明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) から存在する`A`に`B`、配列型から、同じ変換が存在し、`A[R]`配列型に`B[R]`ここで、`R`は any指定された*rank_specifier* (ただし、配列の両方で同じ型)。</span><span class="sxs-lookup"><span data-stu-id="7c83d-157">For any two *reference_type*s `A` and `B`, if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `A` to `B`, then the same reference conversion also exists from the array type `A[R]` to the array type `B[R]`, where `R` is any given *rank_specifier* (but the same for both array types).</span></span> <span data-ttu-id="7c83d-158">このリレーションシップと呼ばれる***配列の共変性***します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-158">This relationship is known as ***array covariance***.</span></span> <span data-ttu-id="7c83d-159">配列の共変性であることを意味、配列型の値を`A[R]`実際には、配列型のインスタンスへの参照があります`B[R]`から暗黙の参照変換が存在する限り、`B`に`A`します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-159">Array covariance in particular means that a value of an array type `A[R]` may actually be a reference to an instance of an array type `B[R]`, provided an implicit reference conversion exists from `B` to `A`.</span></span>

<span data-ttu-id="7c83d-160">参照型の配列の要素への代入には、配列の共変性によりにより許可されている型の配列の要素に割り当てられている値が実際には、実行時チェックが含まれます ([単純な代入](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="7c83d-160">Because of array covariance, assignments to elements of reference type arrays include a run-time check which ensures that the value being assigned to the array element is actually of a permitted type ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="7c83d-161">例:</span><span class="sxs-lookup"><span data-stu-id="7c83d-161">For example:</span></span>
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

<span data-ttu-id="7c83d-162">代入`array[i]`で、`Fill`メソッドは、実行時のチェックを実行できるようにするによって参照されるオブジェクトを暗黙的に含まれます`value`か`null`の実際の要素の型と互換性があるインスタンスまたは`array`.</span><span class="sxs-lookup"><span data-stu-id="7c83d-162">The assignment to `array[i]` in the `Fill` method implicitly includes a run-time check which ensures that the object referenced by `value` is either `null` or an instance that is compatible with the actual element type of `array`.</span></span> <span data-ttu-id="7c83d-163">`Main`の最初の 2 つの呼び出し`Fill`失敗すると、3 つ目の呼び出しの原因が、`System.ArrayTypeMismatchException`への最初の割り当てを実行したときにスローされる`array[i]`。</span><span class="sxs-lookup"><span data-stu-id="7c83d-163">In `Main`, the first two invocations of `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` to be thrown upon executing the first assignment to `array[i]`.</span></span> <span data-ttu-id="7c83d-164">例外が発生するため、ボックス化された`int`で格納することはできません、`string`配列。</span><span class="sxs-lookup"><span data-stu-id="7c83d-164">The exception occurs because a boxed `int` cannot be stored in a `string` array.</span></span>

<span data-ttu-id="7c83d-165">配列の共変性は起こりませんの配列に*value_type*秒。</span><span class="sxs-lookup"><span data-stu-id="7c83d-165">Array covariance specifically does not extend to arrays of *value_type*s.</span></span> <span data-ttu-id="7c83d-166">たとえば、変換が存在しないこと、`int[]`として扱う場合に、`object[]`します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-166">For example, no conversion exists that permits an `int[]` to be treated as an `object[]`.</span></span>

## <a name="array-initializers"></a><span data-ttu-id="7c83d-167">配列初期化子</span><span class="sxs-lookup"><span data-stu-id="7c83d-167">Array initializers</span></span>

<span data-ttu-id="7c83d-168">フィールドの宣言で配列初期化子を指定することがあります ([フィールド](classes.md#fields))、ローカル変数の宣言 ([ローカル変数宣言](statements.md#local-variable-declarations)) を配列作成式 ([配列の作成式](expressions.md#array-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="7c83d-168">Array initializers may be specified in field declarations ([Fields](classes.md#fields)), local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), and array creation expressions ([Array creation expressions](expressions.md#array-creation-expressions)):</span></span>

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="7c83d-169">配列初期化子で囲まれた変数の初期化子のシーケンスから成る"`{`「と」`}`「トークンし、で区切られた」`,`"トークンです。</span><span class="sxs-lookup"><span data-stu-id="7c83d-169">An array initializer consists of a sequence of variable initializers, enclosed by "`{`" and "`}`" tokens and separated by "`,`" tokens.</span></span> <span data-ttu-id="7c83d-170">各変数の初期化子が式または、多次元配列、入れ子になった配列初期化子。</span><span class="sxs-lookup"><span data-stu-id="7c83d-170">Each variable initializer is an expression or, in the case of a multi-dimensional array, a nested array initializer.</span></span>

<span data-ttu-id="7c83d-171">配列初期化子が使用されているコンテキストでは、初期化される配列の型を決定します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-171">The context in which an array initializer is used determines the type of the array being initialized.</span></span> <span data-ttu-id="7c83d-172">配列作成式で配列型はすぐに、初期化子の前または配列初期化子式から推論されます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-172">In an array creation expression, the array type immediately precedes the initializer, or is inferred from the expressions in the array initializer.</span></span> <span data-ttu-id="7c83d-173">フィールドまたは変数宣言では、配列型は、フィールドまたは宣言される変数の型です。</span><span class="sxs-lookup"><span data-stu-id="7c83d-173">In a field or variable declaration, the array type is the type of the field or variable being declared.</span></span> <span data-ttu-id="7c83d-174">配列初期化子で使用する場合、フィールドまたは変数の宣言など。</span><span class="sxs-lookup"><span data-stu-id="7c83d-174">When an array initializer is used in a field or variable declaration, such as:</span></span>
```csharp
int[] a = {0, 2, 4, 6, 8};
```
<span data-ttu-id="7c83d-175">等価な配列作成式を簡略にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="7c83d-175">it is simply shorthand for an equivalent array creation expression:</span></span>
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

<span data-ttu-id="7c83d-176">1 次元配列では、配列初期化子必要がありますの配列の要素の型と互換性のある代入である、式のシーケンスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-176">For a single-dimensional array, the array initializer must consist of a sequence of expressions that are assignment compatible with the element type of the array.</span></span> <span data-ttu-id="7c83d-177">式は、昇順に並べ替え、インデックス 0 位置にある要素で始まる配列の要素を初期化します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-177">The expressions initialize array elements in increasing order, starting with the element at index zero.</span></span> <span data-ttu-id="7c83d-178">配列初期化子式の数は、作成される配列インスタンスの長さを決定します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-178">The number of expressions in the array initializer determines the length of the array instance being created.</span></span> <span data-ttu-id="7c83d-179">たとえば、上記の配列初期化子を作成します、`int[]`長さ 5 のインスタンスとし、次の値を使用して、インスタンスを初期化します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-179">For example, the array initializer above creates an `int[]` instance of length 5 and then initializes the instance with the following values:</span></span>
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

<span data-ttu-id="7c83d-180">多次元配列、配列初期化子は、配列内のディメンションとしての入れ子のレベル数だけが必要です。</span><span class="sxs-lookup"><span data-stu-id="7c83d-180">For a multi-dimensional array, the array initializer must have as many levels of nesting as there are dimensions in the array.</span></span> <span data-ttu-id="7c83d-181">最も外側の入れ子レベルが最も左にあるディメンションに対応し、最も内側の入れ子レベルが最も右にあるディメンションに対応します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-181">The outermost nesting level corresponds to the leftmost dimension and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="7c83d-182">配列の各次元の長さは配列初期化子に対応する入れ子のレベルにある要素の数によって決まります。</span><span class="sxs-lookup"><span data-stu-id="7c83d-182">The length of each dimension of the array is determined by the number of elements at the corresponding nesting level in the array initializer.</span></span> <span data-ttu-id="7c83d-183">入れ子になった配列初期化子の場合、それぞれは、要素の数は同じレベルの他の配列初期化子と同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="7c83d-183">For each nested array initializer, the number of elements must be the same as the other array initializers at the same level.</span></span> <span data-ttu-id="7c83d-184">例:</span><span class="sxs-lookup"><span data-stu-id="7c83d-184">The example:</span></span>
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
<span data-ttu-id="7c83d-185">左端のディメンションの 5 つの長さと右端のディメンションの 2 つの長さでは、2 次元の配列を作成します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-185">creates a two-dimensional array with a length of five for the leftmost dimension and a length of two for the rightmost dimension:</span></span>
```csharp
int[,] b = new int[5, 2];
```
<span data-ttu-id="7c83d-186">初期化します、配列インスタンスで、次の値。</span><span class="sxs-lookup"><span data-stu-id="7c83d-186">and then initializes the array instance with the following values:</span></span>
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

<span data-ttu-id="7c83d-187">長さがゼロで、右端以外のディメンションを指定すると、後続のディメンションが長さがゼロにもと見なされます。</span><span class="sxs-lookup"><span data-stu-id="7c83d-187">If a dimension other than the rightmost is given with length zero, the subsequent dimensions are assumed to also have length zero.</span></span> <span data-ttu-id="7c83d-188">例:</span><span class="sxs-lookup"><span data-stu-id="7c83d-188">The example:</span></span>
```csharp
int[,] c = {};
```
<span data-ttu-id="7c83d-189">長さが 0 の左端と右端のディメンションでは、2 次元の配列を作成します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-189">creates a two-dimensional array with a length of zero for both the leftmost and the rightmost dimension:</span></span>
```csharp
int[,] c = new int[0, 0];
```

<span data-ttu-id="7c83d-190">配列作成式には、明示的な次元の長さと配列初期化子の両方が含まれている場合は、長さは定数式である必要があり、各入れ子のレベルにある要素の数が、対応する次元の長さと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7c83d-190">When an array creation expression includes both explicit dimension lengths and an array initializer, the lengths must be constant expressions and the number of elements at each nesting level must match the corresponding dimension length.</span></span> <span data-ttu-id="7c83d-191">次にいくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="7c83d-191">Here are some examples:</span></span>
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

<span data-ttu-id="7c83d-192">ここでは、初期化子`y`ディメンションの長さの式の定数、および初期化子がないため、コンパイル時エラー結果`z`のため、コンパイル時エラーの結果の長さと要素の数、初期化子が一致しません。</span><span class="sxs-lookup"><span data-stu-id="7c83d-192">Here, the initializer for `y` results in a compile-time error because the dimension length expression is not a constant, and the initializer for `z` results in a compile-time error because the length and the number of elements in the initializer do not agree.</span></span>
