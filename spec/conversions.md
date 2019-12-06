---
ms.openlocfilehash: 4d6d28a3127bc701867afe157aa5496377a06f69
ms.sourcegitcommit: 63d276488c9770a565fd787020783ffc1d2af9d6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868005"
---
# <a name="conversions"></a><span data-ttu-id="87a1f-101">変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-101">Conversions</span></span>

<span data-ttu-id="87a1f-102">***変換***を使用すると、式を特定の型として扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-102">A ***conversion*** enables an expression to be treated as being of a particular type.</span></span> <span data-ttu-id="87a1f-103">変換によって、指定された型の式が別の型として処理されるか、型のない式が型を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-103">A conversion may cause an expression of a given type to be treated as having a different type, or it may cause an expression without a type to get a type.</span></span> <span data-ttu-id="87a1f-104">変換は***暗黙的***または***明示的***に行うことができます。これにより、明示的なキャストが必要かどうかが決まります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-104">Conversions can be ***implicit*** or ***explicit***, and this determines whether an explicit cast is required.</span></span> <span data-ttu-id="87a1f-105">たとえば、型 `int` から型 `long` への変換は暗黙的に行われるため、型 `int` の式は暗黙的に型 `long`として扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-105">For instance, the conversion from type `int` to type `long` is implicit, so expressions of type `int` can implicitly be treated as type `long`.</span></span> <span data-ttu-id="87a1f-106">型 `long` から `int`型への逆の変換は明示的であるため、明示的なキャストが必要です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-106">The opposite conversion, from type `long` to type `int`, is explicit and so an explicit cast is required.</span></span>

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

<span data-ttu-id="87a1f-107">一部の変換は、言語によって定義されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-107">Some conversions are defined by the language.</span></span> <span data-ttu-id="87a1f-108">プログラムでは、独自の変換 ([ユーザー定義の変換](conversions.md#user-defined-conversions)) を定義することもできます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-108">Programs may also define their own conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

## <a name="implicit-conversions"></a><span data-ttu-id="87a1f-109">暗黙の変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-109">Implicit conversions</span></span>

<span data-ttu-id="87a1f-110">次の変換は、暗黙的な変換として分類されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-110">The following conversions are classified as implicit conversions:</span></span>

*  <span data-ttu-id="87a1f-111">Id 変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-111">Identity conversions</span></span>
*  <span data-ttu-id="87a1f-112">暗黙の数値変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-112">Implicit numeric conversions</span></span>
*  <span data-ttu-id="87a1f-113">暗黙的な列挙変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-113">Implicit enumeration conversions</span></span>
*  <span data-ttu-id="87a1f-114">暗黙的な挿入文字列の変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-114">Implicit interpolated string conversions</span></span>
*  <span data-ttu-id="87a1f-115">暗黙の null 許容型変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-115">Implicit nullable conversions</span></span>
*  <span data-ttu-id="87a1f-116">Null リテラル変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-116">Null literal conversions</span></span>
*  <span data-ttu-id="87a1f-117">暗黙の参照変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-117">Implicit reference conversions</span></span>
*  <span data-ttu-id="87a1f-118">ボックス化変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-118">Boxing conversions</span></span>
*  <span data-ttu-id="87a1f-119">暗黙の動的変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-119">Implicit dynamic conversions</span></span>
*  <span data-ttu-id="87a1f-120">暗黙の定数式の変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-120">Implicit constant expression conversions</span></span>
*  <span data-ttu-id="87a1f-121">ユーザー定義の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-121">User-defined implicit conversions</span></span>
*  <span data-ttu-id="87a1f-122">匿名関数の変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-122">Anonymous function conversions</span></span>
*  <span data-ttu-id="87a1f-123">メソッドグループの変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-123">Method group conversions</span></span>

<span data-ttu-id="87a1f-124">暗黙の型変換は、関数メンバー呼び出し ([動的なオーバーロード解決のコンパイル時のチェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))、キャスト式 ([キャスト式](expressions.md#cast-expressions))、代入 ([代入演算子](expressions.md#assignment-operators)) など、さまざまな状況で発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-124">Implicit conversions can occur in a variety of situations, including function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), and assignments ([Assignment operators](expressions.md#assignment-operators)).</span></span>

<span data-ttu-id="87a1f-125">定義済みの暗黙的な変換は常に成功し、例外がスローされることはありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-125">The pre-defined implicit conversions always succeed and never cause exceptions to be thrown.</span></span> <span data-ttu-id="87a1f-126">適切に設計されたユーザー定義の暗黙的な変換でも、これらの特性が示されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-126">Properly designed user-defined implicit conversions should exhibit these characteristics as well.</span></span>

<span data-ttu-id="87a1f-127">変換のために、型 `object` と `dynamic` は同等と見なされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-127">For the purposes of conversion, the types `object` and `dynamic` are considered equivalent.</span></span>

<span data-ttu-id="87a1f-128">ただし、動的変換 ([暗黙の動的](conversions.md#implicit-dynamic-conversions)変換と[明示的な動的](conversions.md#explicit-dynamic-conversions)変換) は `dynamic` 型の式 ([動的型](types.md#the-dynamic-type)) にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-128">However, dynamic conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)) apply only to expressions of type `dynamic` ([The dynamic type](types.md#the-dynamic-type)).</span></span>

### <a name="identity-conversion"></a><span data-ttu-id="87a1f-129">Id 変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-129">Identity conversion</span></span>

<span data-ttu-id="87a1f-130">Id 変換は、任意の型から同じ型に変換します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-130">An identity conversion converts from any type to the same type.</span></span> <span data-ttu-id="87a1f-131">この変換は、既に必要な型を持つエンティティをその型に変換できるようにするために存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-131">This conversion exists such that an entity that already has a required type can be said to be convertible to that type.</span></span>

*  <span data-ttu-id="87a1f-132">`object` と `dynamic` は同等と見なされるため、`object` と `dynamic`間の id 変換と、`dynamic` のすべての出現箇所を `object`に置き換える場合の構築された型の間の id 変換があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-132">Because `object` and `dynamic` are considered equivalent there is an identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing all occurrences of `dynamic` with `object`.</span></span>

### <a name="implicit-numeric-conversions"></a><span data-ttu-id="87a1f-133">暗黙の数値変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-133">Implicit numeric conversions</span></span>

<span data-ttu-id="87a1f-134">暗黙的な数値変換は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-134">The implicit numeric conversions are:</span></span>

*  <span data-ttu-id="87a1f-135">`sbyte` から `short`、`int`、`long`、`float`、`double`、`decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-135">From `sbyte` to `short`, `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-136">`byte` から `short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`、`decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-136">From `byte` to `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-137">`short` から `int`、`long`、`float`、`double`、`decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-137">From `short` to `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-138">`ushort` から `int`、`uint`、`long`、`ulong`、`float`、`double`、`decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-138">From `ushort` to `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-139">`int` から `long`、`float`、`double`、または `decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-139">From `int` to `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-140">`uint` から `long`、`ulong`、`float`、`double`、`decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-140">From `uint` to `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-141">`long` から `float`、`double`、または `decimal`。</span><span class="sxs-lookup"><span data-stu-id="87a1f-141">From `long` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-142">`ulong` から `float`、`double`、または `decimal`。</span><span class="sxs-lookup"><span data-stu-id="87a1f-142">From `ulong` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-143">`char` から `ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`、`decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-143">From `char` to `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-144">`float` から `double`。</span><span class="sxs-lookup"><span data-stu-id="87a1f-144">From `float` to `double`.</span></span>

<span data-ttu-id="87a1f-145">`int`、`uint`、`long`、または `ulong` から `float` への変換または `long` への変換は、精度の低下を招く可能性がありますが、マグニチュードが失われることはありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-145">Conversions from `int`, `uint`, `long`, or `ulong` to `float` and from `long` or `ulong` to `double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="87a1f-146">その他の暗黙的な数値変換では、情報が失われることはありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-146">The other implicit numeric conversions never lose any information.</span></span>

<span data-ttu-id="87a1f-147">`char` 型への暗黙的な変換は行われないため、他の整数型の値は `char` 型に自動的に変換されません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-147">There are no implicit conversions to the `char` type, so values of the other integral types do not automatically convert to the `char` type.</span></span>

### <a name="implicit-enumeration-conversions"></a><span data-ttu-id="87a1f-148">暗黙的な列挙変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-148">Implicit enumeration conversions</span></span>

<span data-ttu-id="87a1f-149">暗黙的な列挙型変換では、 *decimal_integer_literal* `0` を任意の*enum_type*に変換し、基になる型が*enum_type*であるすべての*nullable_type*に変換できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-149">An implicit enumeration conversion permits the *decimal_integer_literal* `0` to be converted to any *enum_type* and to any *nullable_type* whose underlying type is an *enum_type*.</span></span> <span data-ttu-id="87a1f-150">後者の場合、変換は、基になる*enum_type*に変換し、結果をラップすることによって評価されます ([null 許容型](types.md#nullable-types))。</span><span class="sxs-lookup"><span data-stu-id="87a1f-150">In the latter case the conversion is evaluated by converting to the underlying *enum_type* and wrapping the result ([Nullable types](types.md#nullable-types)).</span></span>

### <a name="implicit-interpolated-string-conversions"></a><span data-ttu-id="87a1f-151">暗黙的な挿入文字列の変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-151">Implicit interpolated string conversions</span></span>

<span data-ttu-id="87a1f-152">暗黙的な補間文字列変換は、 *interpolated_string_expression* (挿入[文字列](expressions.md#interpolated-strings)) を `System.IFormattable` または `System.FormattableString` (`System.IFormattable`を実装) に変換することを許可します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-152">An implicit interpolated string conversion permits an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)) to be converted to `System.IFormattable` or `System.FormattableString` (which implements `System.IFormattable`).</span></span>

<span data-ttu-id="87a1f-153">この変換が適用された場合、文字列値は挿入文字列からは構成されません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-153">When this conversion is applied a string value is not composed from the interpolated string.</span></span> <span data-ttu-id="87a1f-154">代わりに、「挿入[文字列](expressions.md#interpolated-strings)」で詳しく説明されているように、`System.FormattableString` のインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-154">Instead an instance of `System.FormattableString` is created, as further described in [Interpolated strings](expressions.md#interpolated-strings).</span></span>

### <a name="implicit-nullable-conversions"></a><span data-ttu-id="87a1f-155">暗黙の null 許容型変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-155">Implicit nullable conversions</span></span>

<span data-ttu-id="87a1f-156">Null 非許容の値型に対して動作する定義済みの暗黙的な変換は、これらの型の null 許容形式でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-156">Predefined implicit conversions that operate on non-nullable value types can also be used with nullable forms of those types.</span></span> <span data-ttu-id="87a1f-157">Null 非許容の値型から null 非許容の値 `T`型に変換する、定義済みの暗黙的な id および数値変換については、次の暗黙的な null 許容の変換が存在します。 `S`</span><span class="sxs-lookup"><span data-stu-id="87a1f-157">For each of the predefined implicit identity and numeric conversions that convert from a non-nullable value type `S` to a non-nullable value type `T`, the following implicit nullable conversions exist:</span></span>

*  <span data-ttu-id="87a1f-158">`S?` から `T?`への暗黙的な変換です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-158">An implicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="87a1f-159">`S` から `T?`への暗黙的な変換です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-159">An implicit conversion from `S` to `T?`.</span></span>

<span data-ttu-id="87a1f-160">`S` から `T` への基になる変換に基づく null 許容型の暗黙的な変換の評価は次のように行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-160">Evaluation of an implicit nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="87a1f-161">Null 許容型変換が `S?` から `T?`の場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-161">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="87a1f-162">ソース値が null (`HasValue` プロパティが false) の場合、結果は `T?`型の null 値になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-162">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="87a1f-163">それ以外の場合、変換は `S?` から `S`へのラップ解除として評価された後、`S` から `T`への基になる変換と、`T` から `T?`へのラップ ([Null 許容型](types.md#nullable-types)) が行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-163">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping ([Nullable types](types.md#nullable-types)) from `T` to `T?`.</span></span>

*  <span data-ttu-id="87a1f-164">Null 許容型変換が `S` から `T?`に対して行われる場合、変換は `S` から `T` の基になる変換として評価され、その後、`T` から `T?`へのラップが行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-164">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>

### <a name="null-literal-conversions"></a><span data-ttu-id="87a1f-165">Null リテラル変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-165">Null literal conversions</span></span>

<span data-ttu-id="87a1f-166">`null` リテラルから null 許容型への暗黙的な変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-166">An implicit conversion exists from the `null` literal to any nullable type.</span></span> <span data-ttu-id="87a1f-167">この変換では、null 許容型の null 値 (null[許容型](types.md#nullable-types)) が生成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-167">This conversion produces the null value ([Nullable types](types.md#nullable-types)) of the given nullable type.</span></span>

### <a name="implicit-reference-conversions"></a><span data-ttu-id="87a1f-168">暗黙の参照変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-168">Implicit reference conversions</span></span>

<span data-ttu-id="87a1f-169">暗黙の参照変換は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-169">The implicit reference conversions are:</span></span>

*  <span data-ttu-id="87a1f-170">任意の*reference_type*から `object` して `dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-170">From any *reference_type* to `object` and `dynamic`.</span></span>
*  <span data-ttu-id="87a1f-171">任意の*class_type* `S` から任意の*class_type* `T`への `S` は、`T`から派生しています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-171">From any *class_type* `S` to any *class_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="87a1f-172">任意の*class_type* `S` から、`S` が `T`を実装している*interface_type* `T`。</span><span class="sxs-lookup"><span data-stu-id="87a1f-172">From any *class_type* `S` to any *interface_type* `T`, provided `S` implements `T`.</span></span>
*  <span data-ttu-id="87a1f-173">任意の*interface_type* `S` から任意の*interface_type* `T`への `S` は、`T`から派生しています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-173">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="87a1f-174">要素型が `T` の*array_type* `S` から、次のすべてが満たされていれば、要素型が `TE`の*array_type*に `SE` ます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-174">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="87a1f-175">`S` と `T` は要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-175">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="87a1f-176">言い換えると、`S` と `T` の次元数が同じになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-176">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="87a1f-177">`SE` と `TE` は両方とも*reference_type*です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-177">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="87a1f-178">`SE` から `TE`への暗黙的な参照変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-178">An implicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="87a1f-179">任意の*array_type*から `System.Array` とそれが実装するインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="87a1f-179">From any *array_type* to `System.Array` and the interfaces it implements.</span></span>
*  <span data-ttu-id="87a1f-180">`S` から `T`への暗黙的な id または参照の変換がある場合は、1次元配列型から `System.Collections.Generic.IList<T>` とその基本インターフェイスを `S[]` します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-180">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an implicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="87a1f-181">任意の*delegate_type*から `System.Delegate` とそれが実装するインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="87a1f-181">From any *delegate_type* to `System.Delegate` and the interfaces it implements.</span></span>
*  <span data-ttu-id="87a1f-182">Null リテラルから任意の*reference_type*にします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-182">From the null literal to any *reference_type*.</span></span>
*  <span data-ttu-id="87a1f-183">*Reference_type* `T0` への暗黙的な id または参照の変換がある場合は、任意の*reference_type*から*reference_type* `T`、`T0` には `T`への id 変換が含まれています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-183">From any *reference_type* to a *reference_type* `T` if it has an implicit identity or reference conversion to a *reference_type* `T0` and `T0` has an identity conversion to `T`.</span></span>
*  <span data-ttu-id="87a1f-184">インターフェイスまたはデリゲート型への暗黙的な id または参照の変換がある場合は、任意の*reference_type*からインターフェイスまたはデリゲート型に `T`、`T0` および `T0` が `T`への分散変換可能 ([変位変換](interfaces.md#variance-conversion)) になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-184">From any *reference_type* to an interface or delegate type `T` if it has an implicit identity or reference conversion to an interface or delegate type `T0` and `T0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `T`.</span></span>
*  <span data-ttu-id="87a1f-185">参照型として認識されている型パラメーターを使用する暗黙的な変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-185">Implicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="87a1f-186">型パラメーターを使用する暗黙的な変換の詳細については、「[型パラメーターに関連する暗黙的な変換](conversions.md#implicit-conversions-involving-type-parameters)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-186">See [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) for more details on implicit conversions involving type parameters.</span></span>

<span data-ttu-id="87a1f-187">暗黙の参照変換とは*reference_type*の間の変換であり、常に成功することが証明されるため、実行時のチェックは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-187">The implicit reference conversions are those conversions between *reference_type*s that can be proven to always succeed, and therefore require no checks at run-time.</span></span>

<span data-ttu-id="87a1f-188">暗黙的または明示的な参照変換では、変換するオブジェクトの参照 id は変更されません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-188">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="87a1f-189">つまり、参照変換によって参照の型が変更されても、参照先のオブジェクトの型または値が変更されることはありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-189">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="87a1f-190">ボックス化変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-190">Boxing conversions</span></span>

<span data-ttu-id="87a1f-191">ボックス化変換は、 *value_type*を参照型に暗黙的に変換することを許可します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-191">A boxing conversion permits a *value_type* to be implicitly converted to a reference type.</span></span> <span data-ttu-id="87a1f-192">ボックス化変換は、すべての*non_nullable_value_type*から `object` と `dynamic`に、 *interface_type*によって実装されている任意の*non_nullable_value_type*に対して `System.ValueType` およびに対して行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-192">A boxing conversion exists from any *non_nullable_value_type* to `object` and `dynamic`, to `System.ValueType` and to any *interface_type* implemented by the *non_nullable_value_type*.</span></span> <span data-ttu-id="87a1f-193">さらに、 *enum_type*は `System.Enum`型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-193">Furthermore an *enum_type* can be converted to the type `System.Enum`.</span></span>

<span data-ttu-id="87a1f-194">基になる*non_nullable_value_type*から参照型へのボックス変換が存在する場合にのみ、 *nullable_type*から参照型へのボックス変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-194">A boxing conversion exists from a *nullable_type* to a reference type, if and only if a boxing conversion exists from the underlying *non_nullable_value_type* to the reference type.</span></span>

<span data-ttu-id="87a1f-195">値型には、インターフェイス型へのボックス変換が `I0` ある場合は `I` インターフェイス型へのボックス変換があり、`I0` には `I`への id 変換が含まれています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-195">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="87a1f-196">値型には、インターフェイス型またはデリゲート `I0` 型へのボックス変換がある場合は `I` インターフェイス型へのボックス変換があり、`I0` は `I`への変位変換 ([変位変換](interfaces.md#variance-conversion)) があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-196">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface or delegate type `I0` and `I0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `I`.</span></span>

<span data-ttu-id="87a1f-197">*Non_nullable_value_type*の値のボックス化では、オブジェクトインスタンスを割り当て、そのインスタンスに*value_type*値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-197">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *value_type* value into that instance.</span></span> <span data-ttu-id="87a1f-198">構造体は、すべての構造体 ([継承](structs.md#inheritance)) の基本クラスであるため `System.ValueType`型にボックス化できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-198">A struct can be boxed to the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="87a1f-199">*Nullable_type*の値をボックス化すると、次のように処理されるようになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-199">Boxing a value of a *nullable_type* proceeds as follows:</span></span>

*  <span data-ttu-id="87a1f-200">ソース値が null (`HasValue` プロパティが false) の場合、結果はターゲット型の null 参照になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-200">If the source value is null (`HasValue` property is false), the result is a null reference of the target type.</span></span>
*  <span data-ttu-id="87a1f-201">それ以外の場合、結果は、ソース値のラップ解除とボックス化によって生成されるボックス化された `T` への参照になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-201">Otherwise, the result is a reference to a boxed `T` produced by unwrapping and boxing the source value.</span></span>

<span data-ttu-id="87a1f-202">ボックス化変換の詳細については、「[ボックス化変換](types.md#boxing-conversions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-202">Boxing conversions are described further in [Boxing conversions](types.md#boxing-conversions).</span></span>

### <a name="implicit-dynamic-conversions"></a><span data-ttu-id="87a1f-203">暗黙の動的変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-203">Implicit dynamic conversions</span></span>

<span data-ttu-id="87a1f-204">`dynamic` 型の式から `T`任意の型への暗黙の動的変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-204">An implicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="87a1f-205">変換は動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。つまり、`T`する式の実行時の型から、実行時に暗黙的な変換がシークされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-205">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an implicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="87a1f-206">変換が見つからない場合は、実行時例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-206">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="87a1f-207">この暗黙的な変換は、暗黙的な変換では例外が発生しないと[いう暗黙の変換の開始](conversions.md#implicit-conversions)時のアドバイスには違反していることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-207">Note that this implicit conversion seemingly violates the advice in the beginning of [Implicit conversions](conversions.md#implicit-conversions) that an implicit conversion should never cause an exception.</span></span> <span data-ttu-id="87a1f-208">ただし、これは変換自体ではなく、例外を発生させる変換の*検索*です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-208">However it is not the conversion itself, but the *finding* of the conversion that causes the exception.</span></span> <span data-ttu-id="87a1f-209">実行時例外のリスクは、動的バインドの使用に固有のものです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-209">The risk of run-time exceptions is inherent in the use of dynamic binding.</span></span> <span data-ttu-id="87a1f-210">変換の動的バインドが望ましくない場合は、式を最初に `object`に変換し、次に目的の型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-210">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="87a1f-211">暗黙の動的変換の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-211">The following example illustrates implicit dynamic conversions:</span></span>

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

<span data-ttu-id="87a1f-212">`s2` と `i` の割り当てには、暗黙的な動的変換が使用されます。この場合、操作のバインドは実行時まで中断されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-212">The assignments to `s2` and `i` both employ implicit dynamic conversions, where the binding of the operations is suspended until run-time.</span></span> <span data-ttu-id="87a1f-213">実行時には、暗黙的な変換は、`d` -- `string` の実行時の型から対象の型にシークされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-213">At run-time, implicit conversions are sought from the run-time type of `d` -- `string` -- to the target type.</span></span> <span data-ttu-id="87a1f-214">`string` への変換は見つかりましたが、`int`はありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-214">A conversion is found to `string` but not to `int`.</span></span>

### <a name="implicit-constant-expression-conversions"></a><span data-ttu-id="87a1f-215">暗黙の定数式の変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-215">Implicit constant expression conversions</span></span>

<span data-ttu-id="87a1f-216">暗黙の定数式の変換では、次の変換が許可されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-216">An implicit constant expression conversion permits the following conversions:</span></span>

*  <span data-ttu-id="87a1f-217">`int` 型の*constant_expression* ([定数式](expressions.md#constant-expressions)) は、 *`short`* の値が変換先の型の範囲内にある場合に、型 `sbyte`、`byte`、`ushort`、`uint`、`ulong`、または constant_expression に変換できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-217">A *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) of type `int` can be converted to type `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the *constant_expression* is within the range of the destination type.</span></span>
*  <span data-ttu-id="87a1f-218">*Constant_expression*の値が負でない場合は、型 `long` の*constant_expression*を `ulong`型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-218">A *constant_expression* of type `long` can be converted to type `ulong`, provided the value of the *constant_expression* is not negative.</span></span>

### <a name="implicit-conversions-involving-type-parameters"></a><span data-ttu-id="87a1f-219">型パラメーターを含む暗黙の型変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-219">Implicit conversions involving type parameters</span></span>

<span data-ttu-id="87a1f-220">指定された型パラメーター `T`には、次の暗黙的な変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-220">The following implicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="87a1f-221">`T` から、有効な基本クラス `C`、`T` から `C`の任意の基底クラス、`T` によって実装されている任意のインターフェイスに `C`ます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-221">From `T` to its effective base class `C`, from `T` to any base class of `C`, and from `T` to any interface implemented by `C`.</span></span> <span data-ttu-id="87a1f-222">実行時に `T` が値型の場合、変換はボックス化変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-222">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="87a1f-223">それ以外の場合、暗黙的な参照変換または id 変換として変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-223">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="87a1f-224">`T` から、`T`の有効なインターフェイスセット内の `I` インターフェイス型と、`T` から `I`の任意の基本インターフェイスになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-224">From `T` to an interface type `I` in `T`'s effective interface set and from `T` to any base interface of `I`.</span></span> <span data-ttu-id="87a1f-225">実行時に `T` が値型の場合、変換はボックス化変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-225">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="87a1f-226">それ以外の場合、暗黙的な参照変換または id 変換として変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-226">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="87a1f-227">`T` から型パラメーター `U`には、`T` が `U` ([型パラメーターの制約](classes.md#type-parameter-constraints)) に依存しています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-227">From `T` to a type parameter `U`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="87a1f-228">実行時に `U` が値型の場合、`T` と `U` は必ず同じ型であり、変換は実行されません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-228">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="87a1f-229">それ以外の場合、`T` が値型の場合、変換はボックス化変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-229">Otherwise, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="87a1f-230">それ以外の場合、暗黙的な参照変換または id 変換として変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-230">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="87a1f-231">Null リテラルから `T`に、指定された `T` は参照型であることがわかっています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-231">From the null literal to `T`, provided `T` is known to be a reference type.</span></span>
*  <span data-ttu-id="87a1f-232">参照型への暗黙的な変換が含まれている場合、`T` から参照型への変換が `I` 場合 `S0` および `S0` には `S`への id 変換が含まれます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-232">From `T` to a reference type `I` if it has an implicit conversion to a reference type `S0` and `S0` has an identity conversion to `S`.</span></span> <span data-ttu-id="87a1f-233">実行時には、`S0`への変換と同じ方法で変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-233">At run-time the conversion is executed the same way as the conversion to `S0`.</span></span>
*  <span data-ttu-id="87a1f-234">インターフェイス型またはデリゲート `I0` 型への暗黙的な変換が含まれている場合は、`T` からインターフェイス型に `I` します。また、`I0` は `I` ([変位変換](interfaces.md#variance-conversion)) に変換可能です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-234">From `T` to an interface type `I` if it has an implicit conversion to an interface or delegate type `I0` and `I0` is variance-convertible to `I` ([Variance conversion](interfaces.md#variance-conversion)).</span></span> <span data-ttu-id="87a1f-235">実行時に `T` が値型の場合、変換はボックス化変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-235">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="87a1f-236">それ以外の場合、暗黙的な参照変換または id 変換として変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-236">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="87a1f-237">`T` が参照型 ([型パラメーターの制約](classes.md#type-parameter-constraints)) であることがわかっている場合、上記の変換はすべて暗黙の参照変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions)) として分類されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-237">If `T` is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)), the conversions above are all classified as implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="87a1f-238">`T` が参照型でないことがわかっている場合、上記の変換はボックス化変換 ([ボックス化変換](conversions.md#boxing-conversions)) として分類されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-238">If `T` is not known to be a reference type, the conversions above are classified as boxing conversions ([Boxing conversions](conversions.md#boxing-conversions)).</span></span>

### <a name="user-defined-implicit-conversions"></a><span data-ttu-id="87a1f-239">ユーザー定義の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-239">User-defined implicit conversions</span></span>

<span data-ttu-id="87a1f-240">ユーザー定義の暗黙の変換は、省略可能な標準の暗黙的な変換で構成され、その後にユーザー定義の暗黙的な変換演算子の実行が続き、その後に省略可能な標準の暗黙的な変換が続きます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-240">A user-defined implicit conversion consists of an optional standard implicit conversion, followed by execution of a user-defined implicit conversion operator, followed by another optional standard implicit conversion.</span></span> <span data-ttu-id="87a1f-241">ユーザー定義の暗黙の変換を評価するための正確な規則については、「[ユーザー定義の暗黙的な変換の処理](conversions.md#processing-of-user-defined-implicit-conversions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-241">The exact rules for evaluating user-defined implicit conversions are described in [Processing of user-defined implicit conversions](conversions.md#processing-of-user-defined-implicit-conversions).</span></span>

### <a name="anonymous-function-conversions-and-method-group-conversions"></a><span data-ttu-id="87a1f-242">匿名関数の変換とメソッドグループの変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-242">Anonymous function conversions and method group conversions</span></span>

<span data-ttu-id="87a1f-243">匿名関数とメソッドグループには、それ自体の型はありませんが、デリゲート型または式ツリー型に暗黙的に変換される場合があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-243">Anonymous functions and method groups do not have types in and of themselves, but may be implicitly converted to delegate types or expression tree types.</span></span> <span data-ttu-id="87a1f-244">匿名関数の変換の詳細については、「[匿名関数の変換](conversions.md#anonymous-function-conversions)」および「メソッドグループの変換」の「メソッドグループの変換」を参照し[てください。](conversions.md#method-group-conversions)</span><span class="sxs-lookup"><span data-stu-id="87a1f-244">Anonymous function conversions are described in more detail in [Anonymous function conversions](conversions.md#anonymous-function-conversions) and method group conversions in [Method group conversions](conversions.md#method-group-conversions).</span></span>

## <a name="explicit-conversions"></a><span data-ttu-id="87a1f-245">明示的な変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-245">Explicit conversions</span></span>

<span data-ttu-id="87a1f-246">次の変換は、明示的な変換として分類されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-246">The following conversions are classified as explicit conversions:</span></span>

*  <span data-ttu-id="87a1f-247">すべての暗黙的な変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-247">All implicit conversions.</span></span>
*  <span data-ttu-id="87a1f-248">明示的な数値変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-248">Explicit numeric conversions.</span></span>
*  <span data-ttu-id="87a1f-249">明示的な列挙変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-249">Explicit enumeration conversions.</span></span>
*  <span data-ttu-id="87a1f-250">明示的な null 許容型変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-250">Explicit nullable conversions.</span></span>
*  <span data-ttu-id="87a1f-251">明示的な参照変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-251">Explicit reference conversions.</span></span>
*  <span data-ttu-id="87a1f-252">明示的なインターフェイス変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-252">Explicit interface conversions.</span></span>
*  <span data-ttu-id="87a1f-253">変換のボックス化を解除します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-253">Unboxing conversions.</span></span>
*  <span data-ttu-id="87a1f-254">明示的な動的変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-254">Explicit dynamic conversions</span></span>
*  <span data-ttu-id="87a1f-255">ユーザー定義の明示的な変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-255">User-defined explicit conversions.</span></span>

<span data-ttu-id="87a1f-256">明示的な変換は、キャスト式 ([キャスト式](expressions.md#cast-expressions)) で行うことができます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-256">Explicit conversions can occur in cast expressions ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="87a1f-257">明示的な変換のセットには、すべての暗黙的な変換が含まれます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-257">The set of explicit conversions includes all implicit conversions.</span></span> <span data-ttu-id="87a1f-258">これは、冗長なキャスト式が許可されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-258">This means that redundant cast expressions are allowed.</span></span>

<span data-ttu-id="87a1f-259">暗黙的な変換ではない明示的な変換とは、常に成功することがわかっていることがわかっている変換、情報を失う可能性がある変換、および明示的に異なる型のドメイン間での変換です。表し.</span><span class="sxs-lookup"><span data-stu-id="87a1f-259">The explicit conversions that are not implicit conversions are conversions that cannot be proven to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit explicit notation.</span></span>

### <a name="explicit-numeric-conversions"></a><span data-ttu-id="87a1f-260">明示的な数値変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-260">Explicit numeric conversions</span></span>

<span data-ttu-id="87a1f-261">明示的な数値変換は、 *numeric_type*から別の*numeric_type*への変換であり、暗黙的な数値変換 ([暗黙的な数値](conversions.md#implicit-numeric-conversions)変換) はまだ存在しません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-261">The explicit numeric conversions are the conversions from a *numeric_type* to another *numeric_type* for which an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) does not already exist:</span></span>

*  <span data-ttu-id="87a1f-262">`sbyte` から `byte`、`ushort`、`uint`、`ulong`、`char`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-262">From `sbyte` to `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="87a1f-263">`byte` から `sbyte` と `char`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-263">From `byte` to `sbyte` and `char`.</span></span>
*  <span data-ttu-id="87a1f-264">`short` から `sbyte`、`byte`、`ushort`、`uint`、`ulong`、`char`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-264">From `short` to `sbyte`, `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="87a1f-265">`ushort` から `sbyte`、`byte`、`short`、または `char`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-265">From `ushort` to `sbyte`, `byte`, `short`, or `char`.</span></span>
*  <span data-ttu-id="87a1f-266">`int` から `sbyte`、`byte`、`short`、`ushort`、`uint`、`ulong`、`char`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-266">From `int` to `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="87a1f-267">`uint` から `sbyte`、`byte`、`short`、`ushort`、`int`、`char`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-267">From `uint` to `sbyte`, `byte`, `short`, `ushort`, `int`, or `char`.</span></span>
*  <span data-ttu-id="87a1f-268">`long` から `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`ulong`、`char`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-268">From `long` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="87a1f-269">`ulong` から `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`char`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-269">From `ulong` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `char`.</span></span>
*  <span data-ttu-id="87a1f-270">`char` から `sbyte`、`byte`、または `short`。</span><span class="sxs-lookup"><span data-stu-id="87a1f-270">From `char` to `sbyte`, `byte`, or `short`.</span></span>
*  <span data-ttu-id="87a1f-271">`float` から `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-271">From `float` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-272">`double` から `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-272">From `double` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-273">`decimal` から `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-273">From `decimal` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `double`.</span></span>

<span data-ttu-id="87a1f-274">明示的な変換には暗黙的な数値変換と明示的な数値変換がすべて含まれているため、キャスト式 ([cast 式](expressions.md#cast-expressions)) を使用して任意の*numeric_type*から他の任意の*numeric_type*に変換することは常に可能です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-274">Because the explicit conversions include all implicit and explicit numeric conversions, it is always possible to convert from any *numeric_type* to any other *numeric_type* using a cast expression ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="87a1f-275">明示的な数値変換によって情報が失われるか、例外がスローされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-275">The explicit numeric conversions possibly lose information or possibly cause exceptions to be thrown.</span></span> <span data-ttu-id="87a1f-276">明示的な数値変換は、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-276">An explicit numeric conversion is processed as follows:</span></span>

*  <span data-ttu-id="87a1f-277">整数型から別の整数型への変換では、変換が行われるオーバーフローチェックコンテキスト ([checked および unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) によって処理が異なります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-277">For a conversion from an integral type to another integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="87a1f-278">`checked` のコンテキストでは、ソースオペランドの値が変換先の型の範囲内にある場合、変換は成功しますが、ソースオペランドの値が変換先の型の範囲外である場合は、`System.OverflowException` をスローします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-278">In a `checked` context, the conversion succeeds if the value of the source operand is within the range of the destination type, but throws a `System.OverflowException` if the value of the source operand is outside the range of the destination type.</span></span>
    * <span data-ttu-id="87a1f-279">`unchecked` のコンテキストでは、変換は常に成功し、次のように進行します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-279">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="87a1f-280">変換元の型が変換先の型より大きい場合、変換元の値はその "余分な" 最上位ビットを破棄することで切り詰められます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-280">If the source type is larger than the destination type, then the source value is truncated by discarding its "extra" most significant bits.</span></span> <span data-ttu-id="87a1f-281">結果は変換先の型の値として扱われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-281">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="87a1f-282">変換元の型が変換先の型より小さい場合、変換元の値は変換先の型と同じサイズになるように符号かゼロが拡張されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-282">If the source type is smaller than the destination type, then the source value is either sign-extended or zero-extended so that it is the same size as the destination type.</span></span> <span data-ttu-id="87a1f-283">変換元の型に符号が付いている場合は符号拡張が利用され、符号が付いていない場合はゼロ拡張が利用されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-283">Sign-extension is used if the source type is signed; zero-extension is used if the source type is unsigned.</span></span> <span data-ttu-id="87a1f-284">結果は変換先の型の値として扱われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-284">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="87a1f-285">変換元の型が変換先の型と同じサイズの場合、変換元の値は変換先の型の値として扱われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-285">If the source type is the same size as the destination type, then the source value is treated as a value of the destination type.</span></span>
*  <span data-ttu-id="87a1f-286">`decimal` から整数型への変換では、変換元の値が、最も近い整数値に0方向に丸められます。この整数値は変換の結果になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-286">For a conversion from `decimal` to an integral type, the source value is rounded towards zero to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="87a1f-287">結果の整数値が変換先の型の範囲外になると、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-287">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="87a1f-288">`float` または `double` から整数型への変換の場合、処理は、変換が行われるオーバーフローチェックコンテキスト ([checked および unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) に依存します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-288">For a conversion from `float` or `double` to an integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="87a1f-289">`checked` のコンテキストでは、変換は次のように行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-289">In a `checked` context, the conversion proceeds as follows:</span></span>
        * <span data-ttu-id="87a1f-290">オペランドの値が NaN または無限の場合は、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-290">If the value of the operand is NaN or infinite, a `System.OverflowException` is thrown.</span></span>
        * <span data-ttu-id="87a1f-291">それ以外の場合、ソースオペランドは0方向に最も近い整数値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-291">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="87a1f-292">この整数値が変換先の型の範囲内にある場合、この値は変換の結果になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-292">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="87a1f-293">それ以外の場合は、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-293">Otherwise, a `System.OverflowException` is thrown.</span></span>
    * <span data-ttu-id="87a1f-294">`unchecked` のコンテキストでは、変換は常に成功し、次のように進行します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-294">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="87a1f-295">オペランドの値が NaN または無限の場合、変換の結果は、変換先の型の指定されていない値になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-295">If the value of the operand is NaN or infinite, the result of the conversion is an unspecified value of the destination type.</span></span>
        * <span data-ttu-id="87a1f-296">それ以外の場合、ソースオペランドは0方向に最も近い整数値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-296">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="87a1f-297">この整数値が変換先の型の範囲内にある場合、この値は変換の結果になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-297">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="87a1f-298">それ以外の場合、変換の結果は、変換先の型の指定されていない値になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-298">Otherwise, the result of the conversion is an unspecified value of the destination type.</span></span>
*  <span data-ttu-id="87a1f-299">`double` から `float`への変換では、`double` 値は最も近い `float` 値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-299">For a conversion from `double` to `float`, the `double` value is rounded to the nearest `float` value.</span></span> <span data-ttu-id="87a1f-300">`double` 値が小さすぎて `float`として表現できない場合、結果は正のゼロまたは負のゼロになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-300">If the `double` value is too small to represent as a `float`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="87a1f-301">`double` 値が大きすぎて `float`として表現できない場合、結果は正の無限大または負の無限大になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-301">If the `double` value is too large to represent as a `float`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="87a1f-302">`double` 値が NaN の場合、結果も NaN になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-302">If the `double` value is NaN, the result is also NaN.</span></span>
*  <span data-ttu-id="87a1f-303">`float` または `double` から `decimal`への変換では、変換元の値が `decimal` 表現に変換され、必要に応じて28進数の小数点以下の最も近い数値に丸められます ([10 進数型](types.md#the-decimal-type))。</span><span class="sxs-lookup"><span data-stu-id="87a1f-303">For a conversion from `float` or `double` to `decimal`, the source value is converted to `decimal` representation and rounded to the nearest number after the 28th decimal place if required ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="87a1f-304">ソース値が小さすぎて `decimal`として表現できない場合、結果は0になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-304">If the source value is too small to represent as a `decimal`, the result becomes zero.</span></span> <span data-ttu-id="87a1f-305">ソース値が NaN、無限大、または大きすぎて `decimal`として表すことができない場合は、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-305">If the source value is NaN, infinity, or too large to represent as a `decimal`, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="87a1f-306">`decimal` から `float` または `double`への変換では、`decimal` 値は最も近い `double` または `float` 値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-306">For a conversion from `decimal` to `float` or `double`, the `decimal` value is rounded to the nearest `double` or `float` value.</span></span> <span data-ttu-id="87a1f-307">この変換によって精度が失われる場合がありますが、例外がスローされることはありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-307">While this conversion may lose precision, it never causes an exception to be thrown.</span></span>

### <a name="explicit-enumeration-conversions"></a><span data-ttu-id="87a1f-308">明示的な列挙変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-308">Explicit enumeration conversions</span></span>

<span data-ttu-id="87a1f-309">明示的な列挙変換は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-309">The explicit enumeration conversions are:</span></span>

*  <span data-ttu-id="87a1f-310">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal` を任意の*enum_type*にします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-310">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal` to any *enum_type*.</span></span>
*  <span data-ttu-id="87a1f-311">任意の*enum_type*から `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-311">From any *enum_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="87a1f-312">任意の*enum_type*から他の*enum_type*に。</span><span class="sxs-lookup"><span data-stu-id="87a1f-312">From any *enum_type* to any other *enum_type*.</span></span>

<span data-ttu-id="87a1f-313">2つの型の間の明示的な列挙型変換は、参加している*enum_type*を*enum_type*の基になる型として扱い、結果の型間で暗黙的または明示的な数値変換を実行することによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-313">An explicit enumeration conversion between two types is processed by treating any participating *enum_type* as the underlying type of that *enum_type*, and then performing an implicit or explicit numeric conversion between the resulting types.</span></span> <span data-ttu-id="87a1f-314">たとえば、および基になる型が `int`の*enum_type* `E` 場合、`E` から `byte` への変換は、`int` から `byte`への明示的な数値変換 ([明示的](conversions.md#explicit-numeric-conversions)な数値変換) として処理され、`byte` から `E` への変換は暗黙的な数値変換 ([暗黙的](conversions.md#implicit-numeric-conversions)な数値変換) として処理されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-314">For example, given an *enum_type* `E` with and underlying type of `int`, a conversion from `E` to `byte` is processed as an explicit numeric conversion ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from `int` to `byte`, and a conversion from `byte` to `E` is processed as an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) from `byte` to `int`.</span></span>

### <a name="explicit-nullable-conversions"></a><span data-ttu-id="87a1f-315">明示的な null 許容変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-315">Explicit nullable conversions</span></span>

<span data-ttu-id="87a1f-316">***明示的な null 許容***型変換では、null 非許容の値型に対して動作する定義済みの明示的な変換が許可され、これらの型の null 許容形式でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-316">***Explicit nullable conversions*** permit predefined explicit conversions that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="87a1f-317">Null 非許容値 `S` 型から `T` null 非許容の値型への変換を行う定義済みの明示的な変換 ([id 変換](conversions.md#identity-conversion)、[暗黙的な数値変換](conversions.md#implicit-numeric-conversions)、[暗黙的な列挙変換](conversions.md#implicit-enumeration-conversions)、[明示的な数値変換](conversions.md#explicit-numeric-conversions)、明示的な[列挙](conversions.md#explicit-enumeration-conversions)変換) には、次の null 許容変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-317">For each of the predefined explicit conversions that convert from a non-nullable value type `S` to a non-nullable value type `T` ([Identity conversion](conversions.md#identity-conversion), [Implicit numeric conversions](conversions.md#implicit-numeric-conversions), [Implicit enumeration conversions](conversions.md#implicit-enumeration-conversions), [Explicit numeric conversions](conversions.md#explicit-numeric-conversions), and [Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)), the following nullable conversions exist:</span></span>

*  <span data-ttu-id="87a1f-318">`S?` から `T?`への明示的な変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-318">An explicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="87a1f-319">`S` から `T?`への明示的な変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-319">An explicit conversion from `S` to `T?`.</span></span>
*  <span data-ttu-id="87a1f-320">`S?` から `T`への明示的な変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-320">An explicit conversion from `S?` to `T`.</span></span>

<span data-ttu-id="87a1f-321">`S` から `T` への基になる変換に基づく null 許容型変換の評価は次のように行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-321">Evaluation of a nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="87a1f-322">Null 許容型変換が `S?` から `T?`の場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-322">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="87a1f-323">ソース値が null (`HasValue` プロパティが false) の場合、結果は `T?`型の null 値になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-323">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="87a1f-324">それ以外の場合、変換は `S?` から `S`へのラップ解除として評価された後、`S` から `T`への基になる変換と、その後に `T` から `T?`へのラップが行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-324">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="87a1f-325">Null 許容型変換が `S` から `T?`に対して行われる場合、変換は `S` から `T` の基になる変換として評価され、その後、`T` から `T?`へのラップが行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-325">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="87a1f-326">Null 許容型変換が `S?` から `T`に対して行われる場合、変換は `S?` から `S` のラップ解除として評価され、その後に `S` から `T`への基になる変換が行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-326">If the nullable conversion is from `S?` to `T`, the conversion is evaluated as an unwrapping from `S?` to `S` followed by the underlying conversion from `S` to `T`.</span></span>

<span data-ttu-id="87a1f-327">Null 許容値のラップを解除しようとすると、値が `null`場合に例外がスローされることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-327">Note that an attempt to unwrap a nullable value will throw an exception if the value is `null`.</span></span>

### <a name="explicit-reference-conversions"></a><span data-ttu-id="87a1f-328">明示的な参照変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-328">Explicit reference conversions</span></span>

<span data-ttu-id="87a1f-329">明示的な参照変換は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-329">The explicit reference conversions are:</span></span>

*  <span data-ttu-id="87a1f-330">`object` から、他の*reference_type*に `dynamic` します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-330">From `object` and `dynamic` to any other *reference_type*.</span></span>
*  <span data-ttu-id="87a1f-331">任意の*class_type* `S` から任意の*class_type* `T`への `S` は、`T`の基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-331">From any *class_type* `S` to any *class_type* `T`, provided `S` is a base class of `T`.</span></span>
*  <span data-ttu-id="87a1f-332">任意の*class_type* `S` から任意の*interface_type* `T`への `S` はシールされていませんが、`S` は実装されていません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-332">From any *class_type* `S` to any *interface_type* `T`, provided `S` is not sealed and provided `S` does not implement `T`.</span></span>
*  <span data-ttu-id="87a1f-333">任意の*interface_type* `T`*class_type* `S` から、`T` を実装 `T` がシールされていない、または指定されている場合は、`S`を実装します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-333">From any *interface_type* `S` to any *class_type* `T`, provided `T` is not sealed or provided `T` implements `S`.</span></span>
*  <span data-ttu-id="87a1f-334">任意の*interface_type* `S` から、`S` は `T`から派生したものではなく、任意の*interface_type* `T`になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-334">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is not derived from `T`.</span></span>
*  <span data-ttu-id="87a1f-335">要素型が `T` の*array_type* `S` から、次のすべてが満たされていれば、要素型が `TE`の*array_type*に `SE` ます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-335">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="87a1f-336">`S` と `T` は要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-336">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="87a1f-337">言い換えると、`S` と `T` の次元数が同じになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-337">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="87a1f-338">`SE` と `TE` は両方とも*reference_type*です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-338">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="87a1f-339">`SE` から `TE`への明示的な参照変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-339">An explicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="87a1f-340">`System.Array` と、それが実装するインターフェイスから、任意の*array_type*にします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-340">From `System.Array` and the interfaces it implements to any *array_type*.</span></span>
*  <span data-ttu-id="87a1f-341">`S` から `T`への明示的な参照変換がある場合は、1次元配列型から `System.Collections.Generic.IList<T>` とその基本インターフェイスを `S[]` します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-341">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an explicit reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="87a1f-342">`S` から `T`への明示的な id または参照の変換がある場合は、`System.Collections.Generic.IList<S>` とその基本インターフェイスから、`T[]`にします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-342">From `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]`, provided that there is an explicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="87a1f-343">`System.Delegate` と、それが実装するインターフェイスから、任意の*delegate_type*にします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-343">From `System.Delegate` and the interfaces it implements to any *delegate_type*.</span></span>
*  <span data-ttu-id="87a1f-344">参照型から参照型への明示的な参照変換がある場合は `T` 参照型から参照型への明示的な参照変換があり、`T0` には `T`の id 変換がある場合は `T0`。</span><span class="sxs-lookup"><span data-stu-id="87a1f-344">From a reference type to a reference type `T` if it has an explicit reference conversion to a reference type `T0` and `T0` has an identity conversion `T`.</span></span>
*  <span data-ttu-id="87a1f-345">参照型からインターフェイスまたはデリゲート型への明示的な参照変換が含まれている場合 `T`、その型がインターフェイスまたはデリゲート `T0` 型への明示的な参照変換を持っている場合、または `T0` が `T` に変換可能であるか `T` に変換可能かどうかを示します ([分散変換](interfaces.md#variance-conversion))。</span><span class="sxs-lookup"><span data-stu-id="87a1f-345">From a reference type to an interface or delegate type `T` if it has an explicit reference conversion to an interface or delegate type `T0` and either `T0` is variance-convertible to `T` or `T` is variance-convertible to `T0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>
*  <span data-ttu-id="87a1f-346">`D<S1...Sn>` から `D<T1...Tn>` `D<X1...Xn>` が汎用デリゲート型である場合、`D<S1...Sn>` は `D<T1...Tn>`との互換性がないか、`Xi` の型パラメーター `D` ごとに次のようになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-346">From `D<S1...Sn>` to `D<T1...Tn>` where `D<X1...Xn>` is a generic delegate type, `D<S1...Sn>` is not compatible with or identical to `D<T1...Tn>`, and for each type parameter `Xi` of `D` the following holds:</span></span>
    * <span data-ttu-id="87a1f-347">`Xi` が不変の場合、`Si` は `Ti`と同じになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-347">If `Xi` is invariant, then `Si` is identical to `Ti`.</span></span>
    * <span data-ttu-id="87a1f-348">`Xi` が共変の場合は、`Si` から `Ti`への暗黙的または明示的な id または参照の変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-348">If `Xi` is covariant, then there is an implicit or explicit identity or reference conversion from `Si` to `Ti`.</span></span>
    * <span data-ttu-id="87a1f-349">`Xi` が反変の場合、`Si` と `Ti` は両方とも同じか、または両方の参照型になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-349">If `Xi` is contravariant, then `Si` and `Ti` are either identical or both reference types.</span></span>
*  <span data-ttu-id="87a1f-350">参照型として認識されている型パラメーターを使用する明示的な変換。</span><span class="sxs-lookup"><span data-stu-id="87a1f-350">Explicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="87a1f-351">型パラメーターを使用する明示的な変換の詳細については、「型パラメーターを使用した[明示的な変換](conversions.md#explicit-conversions-involving-type-parameters)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-351">For more details on explicit conversions involving type parameters, see [Explicit conversions involving type parameters](conversions.md#explicit-conversions-involving-type-parameters).</span></span>

<span data-ttu-id="87a1f-352">明示的な参照変換とは、それらが正しいことを確認するためにランタイムチェックを必要とする参照型間の変換です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-352">The explicit reference conversions are those conversions between reference-types that require run-time checks to ensure they are correct.</span></span>

<span data-ttu-id="87a1f-353">実行時に明示的な参照変換を成功させるには、ソースオペランドの値が `null`であるか、またはソースオペランドによって参照されるオブジェクトの実際の型が、暗黙的な参照変換 ([暗黙の参照](conversions.md#implicit-reference-conversions)変換) またはボックス化変換 ([ボックス](conversions.md#boxing-conversions)化変換) によって対象の型に変換できる型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-353">For an explicit reference conversion to succeed at run-time, the value of the source operand must be `null`, or the actual type of the object referenced by the source operand must be a type that can be converted to the destination type by an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)).</span></span> <span data-ttu-id="87a1f-354">明示的な参照変換が失敗すると、`System.InvalidCastException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-354">If an explicit reference conversion fails, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="87a1f-355">暗黙的または明示的な参照変換では、変換するオブジェクトの参照 id は変更されません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-355">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="87a1f-356">つまり、参照変換によって参照の型が変更されても、参照先のオブジェクトの型または値が変更されることはありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-356">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="87a1f-357">ボックス化解除</span><span class="sxs-lookup"><span data-stu-id="87a1f-357">Unboxing conversions</span></span>

<span data-ttu-id="87a1f-358">ボックス化解除変換は、参照型を明示的に*value_type*に変換することを許可します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-358">An unboxing conversion permits a reference type to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="87a1f-359">ボックス化解除変換は、型 `object`、`dynamic` および `System.ValueType` から任意の*non_nullable_value_type*に、任意の*interface_type*から*non_nullable_value_type*を実装する任意の*interface_type*に変換されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-359">An unboxing conversion exists from the types `object`, `dynamic` and `System.ValueType` to any *non_nullable_value_type*, and from any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span> <span data-ttu-id="87a1f-360">さらに、型 `System.Enum` を任意の*enum_type*にボックス化解除できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-360">Furthermore type `System.Enum` can be unboxed to any *enum_type*.</span></span>

<span data-ttu-id="87a1f-361">参照型から*nullable_type*の基になる*non_nullable_value_type*にアンボックス変換が存在する場合、参照型から*nullable_type*へのアンボックス変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-361">An unboxing conversion exists from a reference type to a *nullable_type* if an unboxing conversion exists from the reference type to the underlying *non_nullable_value_type* of the *nullable_type*.</span></span>

<span data-ttu-id="87a1f-362">インターフェイス `I0` 型からのアンボックス変換があり、`I0` が `I`への id 変換を持っている場合、`S` 値型には、インターフェイス型から `I` のボックス化変換があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-362">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="87a1f-363">インターフェイス型またはデリゲート `I0` 型からのアンボックス変換がある場合、または `I0` が `I` に対して分散変換可能であるか、`I` に変換可能かどうか `I0` ([変位変換](interfaces.md#variance-conversion)) である場合、値型 `S` は、インターフェイス型からのボックス化変換を `I` ます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-363">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface or delegate type `I0` and either `I0` is variance-convertible to `I` or `I` is variance-convertible to `I0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>

<span data-ttu-id="87a1f-364">ボックス化解除操作は、最初にオブジェクトインスタンスが、指定された*value_type*のボックス化された値であることを確認し、次にインスタンスから値をコピーすることで構成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-364">An unboxing operation consists of first checking that the object instance is a boxed value of the given *value_type*, and then copying the value out of the instance.</span></span> <span data-ttu-id="87a1f-365">*Nullable_type*への null 参照のボックス化を解除すると、 *nullable_type*の null 値が生成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-365">Unboxing a null reference to a *nullable_type* produces the null value of the *nullable_type*.</span></span> <span data-ttu-id="87a1f-366">構造体は、すべての構造体 ([継承](structs.md#inheritance)) の基本クラスであるため、`System.ValueType`型からのボックス化を解除できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-366">A struct can be unboxed from the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="87a1f-367">ボックス化解除変換の詳細については、「[ボックス化変換](types.md#unboxing-conversions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-367">Unboxing conversions are described further in [Unboxing conversions](types.md#unboxing-conversions).</span></span>

### <a name="explicit-dynamic-conversions"></a><span data-ttu-id="87a1f-368">明示的な動的変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-368">Explicit dynamic conversions</span></span>

<span data-ttu-id="87a1f-369">明示的な動的変換は、`dynamic` 型の式から `T`任意の型に存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-369">An explicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="87a1f-370">変換は動的にバインドされます ([動的バインド](expressions.md#dynamic-binding))。これは、`T`する式の実行時の型から、実行時に明示的な変換が行われることを意味します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-370">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an explicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="87a1f-371">変換が見つからない場合は、実行時例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-371">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="87a1f-372">変換の動的バインドが望ましくない場合は、式を最初に `object`に変換し、次に目的の型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-372">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="87a1f-373">次のクラスが定義されているとします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-373">Assume the following class is defined:</span></span>
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

<span data-ttu-id="87a1f-374">明示的な動的変換の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-374">The following example illustrates explicit dynamic conversions:</span></span>
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

<span data-ttu-id="87a1f-375">`o` から `C` への最適な変換は、明示的な参照変換としてコンパイル時に検出されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-375">The best conversion of `o` to `C` is found at compile-time to be an explicit reference conversion.</span></span> <span data-ttu-id="87a1f-376">これは、実際には `"1"` が `C`ではないため、実行時に失敗します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-376">This fails at run-time, because `"1"` is not in fact a `C`.</span></span> <span data-ttu-id="87a1f-377">ただし、`d` から `C` への変換は、明示的な動的変換として実行時に中断されます。実行時には、`d` -- の実行時の型から `string` へのユーザー定義の変換が検出され、成功します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-377">The conversion of `d` to `C` however, as an explicit dynamic conversion, is suspended to run-time, where a user defined conversion from the run-time type of `d` -- `string` -- to `C` is found, and succeeds.</span></span>

### <a name="explicit-conversions-involving-type-parameters"></a><span data-ttu-id="87a1f-378">型パラメーターを含む明示的な変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-378">Explicit conversions involving type parameters</span></span>

<span data-ttu-id="87a1f-379">指定された型パラメーター `T`には、次の明示的な変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-379">The following explicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="87a1f-380">`T` の有効な基本クラス `C` から、`C` の任意の基底クラスから `T`に `T` します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-380">From the effective base class `C` of `T` to `T` and from any base class of `C` to `T`.</span></span> <span data-ttu-id="87a1f-381">実行時に `T` が値型の場合、変換はボックス化解除変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-381">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="87a1f-382">それ以外の場合は、明示的な参照変換または id 変換として変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-382">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="87a1f-383">任意のインターフェイス型から `T`します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-383">From any interface type to `T`.</span></span> <span data-ttu-id="87a1f-384">実行時に `T` が値型の場合、変換はボックス化解除変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-384">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="87a1f-385">それ以外の場合は、明示的な参照変換または id 変換として変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-385">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="87a1f-386">`T` から `I`への暗黙的な変換は行われていませんが、指定されたすべての*interface_type* `I` に `T` ます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-386">From `T` to any *interface_type* `I` provided there is not already an implicit conversion from `T` to `I`.</span></span> <span data-ttu-id="87a1f-387">実行時に `T` が値型の場合、変換はボックス化変換として実行され、その後に明示的な参照変換が行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-387">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion followed by an explicit reference conversion.</span></span> <span data-ttu-id="87a1f-388">それ以外の場合は、明示的な参照変換または id 変換として変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-388">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="87a1f-389">型パラメーターから `T`への `U` は、`T` が `U` ([型パラメーターの制約](classes.md#type-parameter-constraints)) に依存しています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-389">From a type parameter `U` to `T`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="87a1f-390">実行時に `U` が値型の場合、`T` と `U` は必ず同じ型であり、変換は実行されません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-390">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="87a1f-391">それ以外の場合、`T` が値型の場合、変換はボックス化解除変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-391">Otherwise, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="87a1f-392">それ以外の場合は、明示的な参照変換または id 変換として変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-392">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="87a1f-393">`T` が参照型であることがわかっている場合、上記の変換はすべて、明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) として分類されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-393">If `T` is known to be a reference type, the conversions above are all classified as explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="87a1f-394">`T` が参照型でないことがわかっている場合、上記の変換は、ボックス化解除変換 ([ボックス化解除](conversions.md#unboxing-conversions)) として分類されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-394">If `T` is not known to be a reference type, the conversions above are classified as unboxing conversions ([Unboxing conversions](conversions.md#unboxing-conversions)).</span></span>

<span data-ttu-id="87a1f-395">上記のルールでは、制約のない型パラメーターから非インターフェイス型への直接明示的な変換は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-395">The above rules do not permit a direct explicit conversion from an unconstrained type parameter to a non-interface type, which might be surprising.</span></span> <span data-ttu-id="87a1f-396">このルールの理由は、混乱を防ぎ、そのような変換のセマンティクスを明確にするためです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-396">The reason for this rule is to prevent confusion and make the semantics of such conversions clear.</span></span> <span data-ttu-id="87a1f-397">たとえば、次のような宣言があるとします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-397">For example, consider the following declaration:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

<span data-ttu-id="87a1f-398">`t` から `int` への直接的な明示的な変換が許可されている場合は、`X<int>.F(7)` が `7L`を返すことが簡単であると考えられます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-398">If the direct explicit conversion of `t` to `int` were permitted, one might easily expect that `X<int>.F(7)` would return `7L`.</span></span> <span data-ttu-id="87a1f-399">ただし、標準の数値変換は、バインド時に型が数値であることがわかっている場合にのみ考慮されるため、そうではありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-399">However, it would not, because the standard numeric conversions are only considered when the types are known to be numeric at binding-time.</span></span> <span data-ttu-id="87a1f-400">このようなセマンティクスを明確にするために、上記の例を記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-400">In order to make the semantics clear, the above example must instead be written:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

<span data-ttu-id="87a1f-401">このコードはコンパイルされるようになりましたが、実行時に `X<int>.F(7)` を実行すると、ボックス化された `int` を `long`に直接変換できないため、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-401">This code will now compile but executing `X<int>.F(7)` would then throw an exception at run-time, since a boxed `int` cannot be converted directly to a `long`.</span></span>

### <a name="user-defined-explicit-conversions"></a><span data-ttu-id="87a1f-402">ユーザー定義の明示的な変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-402">User-defined explicit conversions</span></span>

<span data-ttu-id="87a1f-403">ユーザー定義の明示的な変換は、省略可能な標準の明示的な変換で構成され、その後にユーザー定義の暗黙的または明示的な変換演算子を実行した後に、別のオプションで標準の明示的な変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-403">A user-defined explicit conversion consists of an optional standard explicit conversion, followed by execution of a user-defined implicit or explicit conversion operator, followed by another optional standard explicit conversion.</span></span> <span data-ttu-id="87a1f-404">ユーザー定義の明示的な変換を評価するための正確な規則については、「[ユーザー定義の明示的な変換の処理](conversions.md#processing-of-user-defined-explicit-conversions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-404">The exact rules for evaluating user-defined explicit conversions are described in [Processing of user-defined explicit conversions](conversions.md#processing-of-user-defined-explicit-conversions).</span></span>

## <a name="standard-conversions"></a><span data-ttu-id="87a1f-405">標準変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-405">Standard conversions</span></span>

<span data-ttu-id="87a1f-406">標準変換は、ユーザー定義の変換の一部として発生する可能性のある定義済みの変換です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-406">The standard conversions are those pre-defined conversions that can occur as part of a user-defined conversion.</span></span>

### <a name="standard-implicit-conversions"></a><span data-ttu-id="87a1f-407">標準の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-407">Standard implicit conversions</span></span>

<span data-ttu-id="87a1f-408">次の暗黙の変換は、標準の暗黙的な変換として分類されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-408">The following implicit conversions are classified as standard implicit conversions:</span></span>

*  <span data-ttu-id="87a1f-409">Id 変換 ([id 変換](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="87a1f-409">Identity conversions ([Identity conversion](conversions.md#identity-conversion))</span></span>
*  <span data-ttu-id="87a1f-410">暗黙の数値変換 ([暗黙的な数値変換](conversions.md#implicit-numeric-conversions))</span><span class="sxs-lookup"><span data-stu-id="87a1f-410">Implicit numeric conversions ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions))</span></span>
*  <span data-ttu-id="87a1f-411">暗黙の null 許容変換 (暗黙の null[許容](conversions.md#implicit-nullable-conversions)変換)</span><span class="sxs-lookup"><span data-stu-id="87a1f-411">Implicit nullable conversions ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions))</span></span>
*  <span data-ttu-id="87a1f-412">暗黙の参照変換 ([暗黙的な参照変換](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="87a1f-412">Implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
*  <span data-ttu-id="87a1f-413">ボックス化変換 ([ボックス化変換](conversions.md#boxing-conversions))</span><span class="sxs-lookup"><span data-stu-id="87a1f-413">Boxing conversions ([Boxing conversions](conversions.md#boxing-conversions))</span></span>
*  <span data-ttu-id="87a1f-414">暗黙の定数式の変換 ([暗黙の動的変換](conversions.md#implicit-dynamic-conversions))</span><span class="sxs-lookup"><span data-stu-id="87a1f-414">Implicit constant expression conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions))</span></span>
*  <span data-ttu-id="87a1f-415">型パラメーターを含む暗黙の型変換 ([型パラメーターを含む暗黙](conversions.md#implicit-conversions-involving-type-parameters)の型変換)</span><span class="sxs-lookup"><span data-stu-id="87a1f-415">Implicit conversions involving type parameters ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters))</span></span>

<span data-ttu-id="87a1f-416">標準の暗黙の変換は、明示的にユーザー定義の暗黙的な変換を除外します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-416">The standard implicit conversions specifically exclude user-defined implicit conversions.</span></span>

### <a name="standard-explicit-conversions"></a><span data-ttu-id="87a1f-417">標準の明示的な変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-417">Standard explicit conversions</span></span>

<span data-ttu-id="87a1f-418">標準の明示的な変換は、すべての標準の暗黙的な変換に加え、逆の標準の暗黙的な変換が存在する明示的な変換のサブセットを加えたものです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-418">The standard explicit conversions are all standard implicit conversions plus the subset of the explicit conversions for which an opposite standard implicit conversion exists.</span></span> <span data-ttu-id="87a1f-419">言い換えると、標準の暗黙的な変換が型 `A` から `B`型に存在する場合は、型 `A` から型 `B` および型の `B` への標準の明示的な変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-419">In other words, if a standard implicit conversion exists from a type `A` to a type `B`, then a standard explicit conversion exists from type `A` to type `B` and from type `B` to type `A`.</span></span>

## <a name="user-defined-conversions"></a><span data-ttu-id="87a1f-420">ユーザー定義の変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-420">User-defined conversions</span></span>

<span data-ttu-id="87a1f-421">C#定義済みの暗黙的な変換と明示的な変換を、***ユーザー定義の変換***によって拡張できるようにします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-421">C# allows the pre-defined implicit and explicit conversions to be augmented by ***user-defined conversions***.</span></span> <span data-ttu-id="87a1f-422">ユーザー定義の変換は、クラスおよび構造体型の変換演算子 ([変換演算子](classes.md#conversion-operators)) を宣言することによって導入されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-422">User-defined conversions are introduced by declaring conversion operators ([Conversion operators](classes.md#conversion-operators)) in class and struct types.</span></span>

### <a name="permitted-user-defined-conversions"></a><span data-ttu-id="87a1f-423">許可されたユーザー定義変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-423">Permitted user-defined conversions</span></span>

<span data-ttu-id="87a1f-424">C#特定のユーザー定義変換のみを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-424">C# permits only certain user-defined conversions to be declared.</span></span> <span data-ttu-id="87a1f-425">特に、既存の暗黙的または明示的な変換を再定義することはできません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-425">In particular, it is not possible to redefine an already existing implicit or explicit conversion.</span></span>

<span data-ttu-id="87a1f-426">指定されたソース型 `S` とターゲット型 `T`の場合、`S` または `T` が null 許容型である場合は、`S0` と `T0` がその基になる型を参照するようにします。それ以外の場合、`S0` と `T0` はそれぞれ `S` と `T` になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-426">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="87a1f-427">クラスまたは構造体は、次のすべての条件を満たす場合にのみ、ソース `S` 型からターゲット `T` 型への変換を宣言することができます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-427">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="87a1f-428">`S0` と `T0` の種類は異なります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-428">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="87a1f-429">`S0` または `T0` は、演算子の宣言が行われるクラスまたは構造体の型です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-429">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="87a1f-430">`S0` も `T0` も*interface_type*ではありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-430">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="87a1f-431">ユーザー定義の変換を除外すると、`S` から `T` または `T` から `S`への変換は存在しません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-431">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="87a1f-432">ユーザー定義の変換に適用される制限については、「[変換演算子](classes.md#conversion-operators)」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-432">The restrictions that apply to user-defined conversions are discussed further in [Conversion operators](classes.md#conversion-operators).</span></span>

### <a name="lifted-conversion-operators"></a><span data-ttu-id="87a1f-433">リフト変換演算子</span><span class="sxs-lookup"><span data-stu-id="87a1f-433">Lifted conversion operators</span></span>

<span data-ttu-id="87a1f-434">Null 非許容の値 `S` 型から null 非許容の値 `T`型に変換するユーザー定義の変換演算子を指定した場合、`S?` から `T?`に変換するリフトされた***変換演算子***が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-434">Given a user-defined conversion operator that converts from a non-nullable value type `S` to a non-nullable value type `T`, a ***lifted conversion operator*** exists that converts from `S?` to `T?`.</span></span> <span data-ttu-id="87a1f-435">このリフトされた変換演算子は、`S?` から `S` へのラップ解除を実行します。その後、`S` から `T` へのユーザー定義変換と、`T` から `T?`へのラップが実行されます。ただし、null 値 `S?` は null 値 `T?`に直接変換されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-435">This lifted conversion operator performs an unwrapping from `S?` to `S` followed by the user-defined conversion from `S` to `T` followed by a wrapping from `T` to `T?`, except that a null valued `S?` converts directly to a null valued `T?`.</span></span>

<span data-ttu-id="87a1f-436">リフトされた変換演算子には、基になるユーザー定義変換演算子と同じ暗黙的または明示的な分類があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-436">A lifted conversion operator has the same implicit or explicit classification as its underlying user-defined conversion operator.</span></span> <span data-ttu-id="87a1f-437">"ユーザー定義変換" という用語は、ユーザー定義変換演算子とリフト変換演算子の両方を使用する場合に適用されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-437">The term "user-defined conversion" applies to the use of both user-defined and lifted conversion operators.</span></span>

### <a name="evaluation-of-user-defined-conversions"></a><span data-ttu-id="87a1f-438">ユーザー定義変換の評価</span><span class="sxs-lookup"><span data-stu-id="87a1f-438">Evaluation of user-defined conversions</span></span>

<span data-ttu-id="87a1f-439">ユーザー定義の変換は、***ソース型***と呼ばれる型の値を、***ターゲット型***と呼ばれる別の型に変換します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-439">A user-defined conversion converts a value from its type, called the ***source type***, to another type, called the ***target type***.</span></span> <span data-ttu-id="87a1f-440">ユーザー定義の変換の評価では、特定のソースとターゲットの種類に対して***最も限定的***なユーザー定義変換演算子が検索されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-440">Evaluation of a user-defined conversion centers on finding the ***most specific*** user-defined conversion operator for the particular source and target types.</span></span> <span data-ttu-id="87a1f-441">この決定は、次のいくつかの手順に分かれています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-441">This determination is broken into several steps:</span></span>

*  <span data-ttu-id="87a1f-442">ユーザー定義の変換演算子が考慮されるクラスと構造体のセットを検索します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-442">Finding the set of classes and structs from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="87a1f-443">このセットは、ソースの型とその基本クラス、およびターゲットの型とその基本クラスで構成されます (クラスと構造体のみがユーザー定義の演算子を宣言できること、および非クラス型には基底クラスがないという暗黙の想定があります)。</span><span class="sxs-lookup"><span data-stu-id="87a1f-443">This set consists of the source type and its base classes and the target type and its base classes (with the implicit assumptions that only classes and structs can declare user-defined operators, and that non-class types have no base classes).</span></span> <span data-ttu-id="87a1f-444">この手順では、ソースまたはターゲットのどちらかの型が*nullable_type*の場合、基になる型が代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-444">For the purposes of this step, if either the source or target type is a *nullable_type*, their underlying type is used instead.</span></span>
*  <span data-ttu-id="87a1f-445">この一連の型から、適用可能なユーザー定義およびリフト変換演算子を決定します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-445">From that set of types, determining which user-defined and lifted conversion operators are applicable.</span></span> <span data-ttu-id="87a1f-446">変換演算子を適用できるようにするには、ソース型から演算子のオペランド型への標準変換 ([標準](conversions.md#standard-conversions)変換) を実行できる必要があります。また、演算子の結果型からターゲット型への標準変換を実行できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-446">For a conversion operator to be applicable, it must be possible to perform a standard conversion ([Standard conversions](conversions.md#standard-conversions)) from the source type to the operand type of the operator, and it must be possible to perform a standard conversion from the result type of the operator to the target type.</span></span>
*  <span data-ttu-id="87a1f-447">適用可能なユーザー定義演算子のセットから、明確に特定できる演算子を決定します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-447">From the set of applicable user-defined operators, determining which operator is unambiguously the most specific.</span></span> <span data-ttu-id="87a1f-448">一般的に、最も具体的な演算子は、変換元の型に対してオペランドの型が "最も近い" 演算子で、結果の型が対象の型に "最も近い" になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-448">In general terms, the most specific operator is the operator whose operand type is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="87a1f-449">ユーザー定義の変換演算子は、リフト変換演算子よりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-449">User-defined conversion operators are preferred over lifted conversion operators.</span></span> <span data-ttu-id="87a1f-450">特定のユーザー定義変換演算子を設定するための正確な規則は、次のセクションで定義されています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-450">The exact rules for establishing the most specific user-defined conversion operator are defined in the following sections.</span></span>

<span data-ttu-id="87a1f-451">最も限定的なユーザー定義変換演算子が特定されたら、ユーザー定義変換の実際の実行では、次の3つの手順が必要になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-451">Once a most specific user-defined conversion operator has been identified, the actual execution of the user-defined conversion involves up to three steps:</span></span>

*  <span data-ttu-id="87a1f-452">最初に、必要に応じて、変換元の型からユーザー定義またはリフトされた変換演算子のオペランドの型への標準変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-452">First, if required, performing a standard conversion from the source type to the operand type of the user-defined or lifted conversion operator.</span></span>
*  <span data-ttu-id="87a1f-453">次に、ユーザー定義変換演算子またはリフト変換演算子を呼び出して変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-453">Next, invoking the user-defined or lifted conversion operator to perform the conversion.</span></span>
*  <span data-ttu-id="87a1f-454">最後に、必要に応じて、ユーザー定義またはリフトされた変換演算子の結果型から対象の型への標準変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-454">Finally, if required, performing a standard conversion from the result type of the user-defined or lifted conversion operator to the target type.</span></span>

<span data-ttu-id="87a1f-455">ユーザー定義の変換の評価では、複数のユーザー定義変換演算子またはリフト変換演算子は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-455">Evaluation of a user-defined conversion never involves more than one user-defined or lifted conversion operator.</span></span> <span data-ttu-id="87a1f-456">言い換えると、型 `S` から型 `T` への変換では、最初に `S` から `X` へのユーザー定義変換が実行され、次に `X` から `T`へのユーザー定義変換が実行されることはありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-456">In other words, a conversion from type `S` to type `T` will never first execute a user-defined conversion from `S` to `X` and then execute a user-defined conversion from `X` to `T`.</span></span>

<span data-ttu-id="87a1f-457">ユーザー定義の暗黙的または明示的な変換の評価の正確な定義については、次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-457">Exact definitions of evaluation of user-defined implicit or explicit conversions are given in the following sections.</span></span> <span data-ttu-id="87a1f-458">定義では、次の用語が使用されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-458">The definitions make use of the following terms:</span></span>

*  <span data-ttu-id="87a1f-459">標準の暗黙的な変換 ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) が型 `A` から `B`型に存在する場合、`A` も `B` も\*interface_type\*\*\*\*に含ま***れていないと、`A` が `B`***を包含し\*\*\*ていると言います。</span><span class="sxs-lookup"><span data-stu-id="87a1f-459">If a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) exists from a type `A` to a type `B`, and if neither `A` nor `B` are *interface_type*s, then `A` is said to be ***encompassed by*** `B`, and `B` is said to ***encompass*** `A`.</span></span>
*  <span data-ttu-id="87a1f-460">型のセットの中で***最も外側***にある型は、セット内の他のすべての型を含む1つの型です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-460">The ***most encompassing type*** in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="87a1f-461">1つの型に他のすべての型が含まれていない場合、そのセットには最も外側の型がありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-461">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="87a1f-462">より直観的な用語では、最も外側の型は、セット内の "最大" 型です。これは、他の型を暗黙的に変換できる1つの型です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-462">In more intuitive terms, the most encompassing type is the "largest" type in the set—the one type to which each of the other types can be implicitly converted.</span></span>
*  <span data-ttu-id="87a1f-463">型のセットの中で***最も内側***にある型は、セット内の他のすべての型に包含されている型です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-463">The ***most encompassed type*** in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="87a1f-464">他のすべての型に包含されている型がない場合は、そのセットに最も包含されていない型はありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-464">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="87a1f-465">より直観的な用語では、最も包含されている型は、セット内の "最小" 型です。これは、他の型に暗黙的に変換できる1つの型です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-465">In more intuitive terms, the most encompassed type is the "smallest" type in the set—the one type that can be implicitly converted to each of the other types.</span></span>

### <a name="processing-of-user-defined-implicit-conversions"></a><span data-ttu-id="87a1f-466">ユーザー定義の暗黙的な変換の処理</span><span class="sxs-lookup"><span data-stu-id="87a1f-466">Processing of user-defined implicit conversions</span></span>

<span data-ttu-id="87a1f-467">型 `S` から型 `T` へのユーザー定義の暗黙的な変換は、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-467">A user-defined implicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="87a1f-468">`S0` と `T0`の種類を決定します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-468">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="87a1f-469">`S` または `T` が null 許容型である場合、`S0` と `T0` はその基になる型です。それ以外の場合は、`S0` と `T0` がそれぞれ `S` と `T` になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-469">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="87a1f-470">ユーザー定義の変換演算子が考慮される型のセット (`D`) を検索します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-470">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="87a1f-471">このセットは、`S0` (`S0` がクラスまたは構造体である場合)、`S0` の基本クラス (`S0` がクラスの場合)、`T0` (`T0` がクラスまたは構造体の場合) で構成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-471">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), and `T0` (if `T0` is a class or struct).</span></span>
*  <span data-ttu-id="87a1f-472">適用可能なユーザー定義およびリフト変換演算子のセットを検索し、`U`します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-472">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="87a1f-473">このセットは、`S` を含む型から `T`に含まれる型への変換を行う `D` のクラスまたは構造体によって宣言された、ユーザー定義の、リフトされた暗黙の変換演算子で構成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-473">This set consists of the user-defined and lifted implicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing `S` to a type encompassed by `T`.</span></span> <span data-ttu-id="87a1f-474">`U` が空の場合、変換は定義されていないため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-474">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="87a1f-475">`U`内の演算子の最も具体的なソースの種類である `SX`を検索します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-475">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="87a1f-476">`U` 内のいずれかの演算子が `S`から変換される場合、`SX` は `S`ます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-476">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="87a1f-477">それ以外の場合、`SX` は `U`内の演算子のソースの種類の組み合わせにおいて最も包含されている型です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-477">Otherwise, `SX` is the most encompassed type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="87a1f-478">最も包含されている型が1つだけ見つからない場合、変換はあいまいで、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-478">If exactly one most encompassed type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="87a1f-479">`U`内の演算子の最も具体的な対象の型である `TX`を検索します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-479">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="87a1f-480">`U` のいずれかの演算子が `T`に変換される場合、`TX` は `T`ます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-480">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="87a1f-481">それ以外の場合、`TX` は、`U`内の演算子の対象の型を組み合わせたセット内で最も外側にある型になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-481">Otherwise, `TX` is the most encompassing type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="87a1f-482">最も外側の型が1つだけ見つからない場合、変換はあいまいで、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-482">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="87a1f-483">最も具体的な変換演算子を見つけます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-483">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="87a1f-484">`SX` から `TX`に変換するユーザー定義の変換演算子を1つだけ含む `U` 場合は、これが最も具体的な変換演算子です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-484">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="87a1f-485">それ以外の `U` 場合、`SX` から `TX`に変換するリフト変換演算子が1つだけ含まれている場合は、これが最も具体的な変換演算子になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-485">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="87a1f-486">それ以外の場合、変換はあいまいで、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-486">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="87a1f-487">最後に、変換を適用します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-487">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="87a1f-488">`S` が `SX`ない場合は、`S` から `SX` への標準の暗黙的な変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-488">If `S` is not `SX`, then a standard implicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="87a1f-489">`SX` から `TX`に変換するために、最も具体的な変換演算子が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-489">The most specific conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="87a1f-490">`TX` が `T`ない場合は、`TX` から `T` への標準の暗黙的な変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-490">If `TX` is not `T`, then a standard implicit conversion from `TX` to `T` is performed.</span></span>

### <a name="processing-of-user-defined-explicit-conversions"></a><span data-ttu-id="87a1f-491">ユーザー定義の明示的な変換の処理</span><span class="sxs-lookup"><span data-stu-id="87a1f-491">Processing of user-defined explicit conversions</span></span>

<span data-ttu-id="87a1f-492">`S` 型から `T` 型へのユーザー定義の明示的な変換は、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-492">A user-defined explicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="87a1f-493">`S0` と `T0`の種類を決定します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-493">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="87a1f-494">`S` または `T` が null 許容型である場合、`S0` と `T0` はその基になる型です。それ以外の場合は、`S0` と `T0` がそれぞれ `S` と `T` になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-494">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="87a1f-495">ユーザー定義の変換演算子が考慮される型のセット (`D`) を検索します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-495">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="87a1f-496">このセットは、`S0` (`S0` がクラスまたは構造体である場合)、`S0` の基本クラス (`S0` がクラスの場合)、`T0` (`T0` がクラスまたは構造体の場合)、`T0` の基本クラス (`T0` がクラスの場合) で構成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-496">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), `T0` (if `T0` is a class or struct), and the base classes of `T0` (if `T0` is a class).</span></span>
*  <span data-ttu-id="87a1f-497">適用可能なユーザー定義およびリフト変換演算子のセットを検索し、`U`します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-497">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="87a1f-498">このセットは、`S` によって包含または包含されている型を `T`によって包含または包含されている型に変換する、`D` のクラスまたは構造体によって宣言された、ユーザー定義の、リフトされた暗黙的または明示的な変換演算子で構成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-498">This set consists of the user-defined and lifted implicit or explicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing or encompassed by `S` to a type encompassing or encompassed by `T`.</span></span> <span data-ttu-id="87a1f-499">`U` が空の場合、変換は定義されていないため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-499">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="87a1f-500">`U`内の演算子の最も具体的なソースの種類である `SX`を検索します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-500">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="87a1f-501">`U` 内のいずれかの演算子が `S`から変換される場合、`SX` は `S`ます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-501">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="87a1f-502">それ以外の場合、`U` 内のいずれかの演算子が `S`を含む型から変換される場合、`SX` は、それらの演算子のソース型の組み合わせで最も包含されている型になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-502">Otherwise, if any of the operators in `U` convert from types that encompass `S`, then `SX` is the most encompassed type in the combined set of source types of those operators.</span></span> <span data-ttu-id="87a1f-503">最も包含されていない型が見つからない場合は、変換があいまいになり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-503">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="87a1f-504">それ以外の場合、`SX` は `U`内の演算子のソースの種類の組み合わせにおいて最も外側の型になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-504">Otherwise, `SX` is the most encompassing type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="87a1f-505">最も外側の型が1つだけ見つからない場合、変換はあいまいで、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-505">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="87a1f-506">`U`内の演算子の最も具体的な対象の型である `TX`を検索します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-506">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="87a1f-507">`U` のいずれかの演算子が `T`に変換される場合、`TX` は `T`ます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-507">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="87a1f-508">それ以外の場合、`U` 内のいずれかの演算子が `T`によって包含されている型に変換される場合、`TX` は、それらの演算子の対象となる型の組み合わせのセット内で最も外側の型になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-508">Otherwise, if any of the operators in `U` convert to types that are encompassed by `T`, then `TX` is the most encompassing type in the combined set of target types of those operators.</span></span> <span data-ttu-id="87a1f-509">最も外側の型が1つだけ見つからない場合、変換はあいまいで、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-509">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="87a1f-510">それ以外の場合、`TX` は、`U`内の演算子の対象となる型の組み合わせで最も包含される型です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-510">Otherwise, `TX` is the most encompassed type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="87a1f-511">最も包含されていない型が見つからない場合は、変換があいまいになり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-511">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="87a1f-512">最も具体的な変換演算子を見つけます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-512">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="87a1f-513">`SX` から `TX`に変換するユーザー定義の変換演算子を1つだけ含む `U` 場合は、これが最も具体的な変換演算子です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-513">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="87a1f-514">それ以外の `U` 場合、`SX` から `TX`に変換するリフト変換演算子が1つだけ含まれている場合は、これが最も具体的な変換演算子になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-514">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="87a1f-515">それ以外の場合、変換はあいまいで、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-515">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="87a1f-516">最後に、変換を適用します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-516">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="87a1f-517">`S` が `SX`ない場合は、`S` から `SX` への標準の明示的な変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-517">If `S` is not `SX`, then a standard explicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="87a1f-518">`SX` から `TX`に変換するために、ユーザー定義の最も限定的な変換演算子が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-518">The most specific user-defined conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="87a1f-519">`TX` が `T`ない場合は、`TX` から `T` への標準の明示的な変換が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-519">If `TX` is not `T`, then a standard explicit conversion from `TX` to `T` is performed.</span></span>

## <a name="anonymous-function-conversions"></a><span data-ttu-id="87a1f-520">匿名関数の変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-520">Anonymous function conversions</span></span>

<span data-ttu-id="87a1f-521">*Anonymous_method_expression*または*lambda_expression*は、匿名関数 ([匿名関数式](expressions.md#anonymous-function-expressions)) として分類されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-521">An *anonymous_method_expression* or *lambda_expression* is classified as an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="87a1f-522">式に型がありませんが、互換性のあるデリゲート型または式ツリー型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-522">The expression does not have a type but can be implicitly converted to a compatible delegate type or expression tree type.</span></span> <span data-ttu-id="87a1f-523">具体的には、匿名関数 `F` は、指定された `D` デリゲート型と互換性があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-523">Specifically, an anonymous function `F` is compatible with a delegate type `D` provided:</span></span>

*  <span data-ttu-id="87a1f-524">`F` に*anonymous_function_signature*が含まれている場合は、`D` と `F` のパラメーターの数が同じになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-524">If `F` contains an *anonymous_function_signature*, then `D` and `F` have the same number of parameters.</span></span>
*  <span data-ttu-id="87a1f-525">`F` に*anonymous_function_signature*が含まれていない場合、`D` のパラメーターに `out` parameter 修飾子が指定されていない限り、`D` には任意の型の0個以上のパラメーターを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-525">If `F` does not contain an *anonymous_function_signature*, then `D` may have zero or more parameters of any type, as long as no parameter of `D` has the `out` parameter modifier.</span></span>
*  <span data-ttu-id="87a1f-526">`F` に明示的に型指定されたパラメーターリストがある場合、`D` 内の各パラメーターの型と修飾子は `F`内の対応するパラメーターと同じになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-526">If `F` has an explicitly typed parameter list, each parameter in `D` has the same type and modifiers as the corresponding parameter in `F`.</span></span>
*  <span data-ttu-id="87a1f-527">`F` に暗黙的に型指定されたパラメーターリストがある場合、`D` には `ref` または `out` パラメーターがありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-527">If `F` has an implicitly typed parameter list, `D` has no `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="87a1f-528">`F` の本体が式であり、`D` が `void` の戻り値の型であるか、または `F` が非同期であり、`D` の戻り値の型が `Task`である場合、`F` の本体*は、`D`(* [式ステートメント](statements.md#expression-statements)) として許可される有効な式 (wrt[式](expressions.md)) になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-528">If the body of `F` is an expression, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that would be permitted as a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>
*  <span data-ttu-id="87a1f-529">`F` の本体がステートメントブロックであり、`D` が `void` の戻り値の型を持っている場合、または `F` が非同期で、`D` の戻り値の型が `Task`の場合、`F` の本体は有効なステートメントブロック (wrt [block](statements.md#blocks)) で、`D`ステートメントで式が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-529">If the body of `F` is a statement block, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) in which no `return` statement specifies an expression.</span></span>
*  <span data-ttu-id="87a1f-530">`F` の本体が式であり *、`F` が*非同期ではなく、`D` の戻り値の `T`型が void でない場合、*または*`F` が async で、`D` が戻り値の型 `Task<T>`の場合、`F` の本体は、暗黙的に `D`に変換できる有効な式 (wrt[式](expressions.md)) になります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-530">If the body of `F` is an expression, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that is implicitly convertible to `T`.</span></span>
*  <span data-ttu-id="87a1f-531">`F` の本体がステートメントブロックで、`F` が非同期*ではなく*、`D` が void 以外の戻り値の型 `T`の場合、*または*`F` が async で `D` が戻り値の型 `Task<T>`である場合、`F` の各パラメーターに `D`の対応するパラメーターの型が指定されている場合、`F` の本体は到達できないエンドポイントを持つ有効なステートメントブロック (wrt [block](statements.md#blocks)) です。この場合、各 `return` ステートメントは、暗黙的に変換可能な式を指定します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-531">If the body of `F` is a statement block, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) with a non-reachable end point in which each `return` statement specifies an expression that is implicitly convertible to `T`.</span></span>

<span data-ttu-id="87a1f-532">簡潔にするために、このセクションでは、タスクの種類 `Task` および `Task<T>` ([非同期関数](classes.md#async-functions)) に短い形式を使用します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-532">For the purpose of brevity, this section uses the short form for the task types `Task` and `Task<T>` ([Async functions](classes.md#async-functions)).</span></span>

<span data-ttu-id="87a1f-533">ラムダ式 `F` は、`F` がデリゲート型 `D`と互換性がある場合に `Expression<D>` 式ツリー型と互換性があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-533">A lambda expression `F` is compatible with an expression tree type `Expression<D>` if `F` is compatible with the delegate type `D`.</span></span> <span data-ttu-id="87a1f-534">これは匿名メソッドには適用されず、ラムダ式にのみ適用されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-534">Note that this does not apply to anonymous methods, only lambda expressions.</span></span>

<span data-ttu-id="87a1f-535">特定のラムダ式は、式のツリー型に変換できません。変換が*存在*する場合でも、コンパイル時に失敗します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-535">Certain lambda expressions cannot be converted to expression tree types: Even though the conversion *exists*, it fails at compile-time.</span></span> <span data-ttu-id="87a1f-536">ラムダ式の場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-536">This is the case if the lambda expression:</span></span>

*  <span data-ttu-id="87a1f-537">*ブロック*本体がある</span><span class="sxs-lookup"><span data-stu-id="87a1f-537">Has a *block* body</span></span>
*  <span data-ttu-id="87a1f-538">単純型または複合型の代入演算子を含んでいます</span><span class="sxs-lookup"><span data-stu-id="87a1f-538">Contains simple or compound assignment operators</span></span>
*  <span data-ttu-id="87a1f-539">動的にバインドされた式を含む</span><span class="sxs-lookup"><span data-stu-id="87a1f-539">Contains a dynamically bound expression</span></span>
*  <span data-ttu-id="87a1f-540">非同期</span><span class="sxs-lookup"><span data-stu-id="87a1f-540">Is async</span></span>

<span data-ttu-id="87a1f-541">次の例では、汎用デリゲート `Func<A,R>` 型を使用します。これは、型 `A` の引数を受け取り、`R`型の値を返す関数を表します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-541">The examples that follow use a generic delegate type `Func<A,R>` which represents a function that takes an argument of type `A` and returns a value of type `R`:</span></span>
```csharp
delegate R Func<A,R>(A arg);
```

<span data-ttu-id="87a1f-542">割り当て</span><span class="sxs-lookup"><span data-stu-id="87a1f-542">In the assignments</span></span>
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
<span data-ttu-id="87a1f-543">各匿名関数のパラメーターと戻り値の型は、匿名関数が割り当てられている変数の型によって決まります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-543">the parameter and return types of each anonymous function are determined from the type of the variable to which the anonymous function is assigned.</span></span>

<span data-ttu-id="87a1f-544">最初の代入は、匿名関数をデリゲート `Func<int,int>` 型に正常に変換します。これは、`x` に型 `int`が指定されている場合、`x+1` は型 `int`に暗黙的に変換できる有効な式であるためです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-544">The first assignment successfully converts the anonymous function to the delegate type `Func<int,int>` because, when `x` is given type `int`, `x+1` is a valid expression that is implicitly convertible to type `int`.</span></span>

<span data-ttu-id="87a1f-545">同様に、2番目の代入は、匿名関数をデリゲート `Func<int,double>` 型に正常に変換します。これは、`x+1` (型 `int`) の結果が暗黙的に型 `double`に変換可能であるためです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-545">Likewise, the second assignment successfully converts the anonymous function to the delegate type `Func<int,double>` because the result of `x+1` (of type `int`) is implicitly convertible to type `double`.</span></span>

<span data-ttu-id="87a1f-546">ただし、3番目の代入はコンパイル時エラーになります。これは、`x` に型 `double`が指定されている場合、`x+1` (型 `double`) の結果が型 `int`に暗黙的に変換可能ではないためです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-546">However, the third assignment is a compile-time error because, when `x` is given type `double`, the result of `x+1` (of type `double`) is not implicitly convertible to type `int`.</span></span>

<span data-ttu-id="87a1f-547">4番目の代入は、匿名の非同期関数をデリゲート型 `Func<int, Task<int>>` に正常に変換します。これは、`x+1` (型 `int`) の結果が、タスクの種類 `Task<int>`の `int` 結果の型に暗黙的に変換できるためです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-547">The fourth assignment successfully converts the anonymous async function to the delegate type `Func<int, Task<int>>` because the result of `x+1` (of type `int`) is implicitly convertible to the result type `int` of the task type `Task<int>`.</span></span>

<span data-ttu-id="87a1f-548">匿名関数は、オーバーロードの解決に影響を与える可能性があり、型の推定に関与します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-548">Anonymous functions may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="87a1f-549">詳細については、「[関数メンバー](expressions.md#function-members) 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-549">See [Function members](expressions.md#function-members) for further details.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a><span data-ttu-id="87a1f-550">デリゲート型への匿名関数変換の評価</span><span class="sxs-lookup"><span data-stu-id="87a1f-550">Evaluation of anonymous function conversions to delegate types</span></span>

<span data-ttu-id="87a1f-551">匿名関数をデリゲート型に変換すると、匿名関数を参照するデリゲートインスタンスと、評価時にアクティブであるキャプチャされた外部変数の (空の場合もありますが) セットが生成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-551">Conversion of an anonymous function to a delegate type produces a delegate instance which references the anonymous function and the (possibly empty) set of captured outer variables that are active at the time of the evaluation.</span></span> <span data-ttu-id="87a1f-552">デリゲートが呼び出されると、匿名関数の本体が実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-552">When the delegate is invoked, the body of the anonymous function is executed.</span></span> <span data-ttu-id="87a1f-553">本文内のコードは、デリゲートによって参照されるキャプチャされた外部変数のセットを使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-553">The code in the body is executed using the set of captured outer variables referenced by the delegate.</span></span>

<span data-ttu-id="87a1f-554">匿名関数から生成されたデリゲートの呼び出しリストには、1つのエントリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-554">The invocation list of a delegate produced from an anonymous function contains a single entry.</span></span> <span data-ttu-id="87a1f-555">デリゲートの正確なターゲットオブジェクトとターゲットメソッドは指定されていません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-555">The exact target object and target method of the delegate are unspecified.</span></span> <span data-ttu-id="87a1f-556">特に、デリゲートのターゲットオブジェクトが `null`、外側の関数メンバーの `this` 値、またはその他のオブジェクトであるかどうかは指定されていません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-556">In particular, it is unspecified whether the target object of the delegate is `null`, the `this` value of the enclosing function member, or some other object.</span></span>

<span data-ttu-id="87a1f-557">同じ (場合によっては空の) キャプチャされた外部変数インスタンスを同じデリゲート型に対して同じ匿名関数から変換すると、同じデリゲートインスタンスを返すことができます (ただし、必須ではありません)。</span><span class="sxs-lookup"><span data-stu-id="87a1f-557">Conversions of semantically identical anonymous functions with the same (possibly empty) set of captured outer variable instances to the same delegate types are permitted (but not required) to return the same delegate instance.</span></span> <span data-ttu-id="87a1f-558">ここでは、意味的に同じ用語が使用されています。これは、匿名関数の実行によって、同じ引数を指定した場合と同じ効果が生成されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-558">The term semantically identical is used here to mean that execution of the anonymous functions will, in all cases, produce the same effects given the same arguments.</span></span> <span data-ttu-id="87a1f-559">この規則は、次のようなコードを最適化することを許可します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-559">This rule permits code such as the following to be optimized.</span></span>

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

<span data-ttu-id="87a1f-560">2つの匿名関数デリゲートは、キャプチャされた外部変数と同じ (空の) セットを持つため、匿名関数は意味が同じであるため、コンパイラはデリゲートが同じターゲットメソッドを参照することを許可します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-560">Since the two anonymous function delegates have the same (empty) set of captured outer variables, and since the anonymous functions are semantically identical, the compiler is permitted to have the delegates refer to the same target method.</span></span> <span data-ttu-id="87a1f-561">実際には、コンパイラは、両方の匿名関数式からまったく同じデリゲートインスタンスを返すことができます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-561">Indeed, the compiler is permitted to return the very same delegate instance from both anonymous function expressions.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a><span data-ttu-id="87a1f-562">式ツリー型への匿名関数変換の評価</span><span class="sxs-lookup"><span data-stu-id="87a1f-562">Evaluation of anonymous function conversions to expression tree types</span></span>

<span data-ttu-id="87a1f-563">匿名関数から式ツリー型への変換では、式ツリー ([式ツリー型](types.md#expression-tree-types)) が生成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-563">Conversion of an anonymous function to an expression tree type produces an expression tree ([Expression tree types](types.md#expression-tree-types)).</span></span> <span data-ttu-id="87a1f-564">より正確には、匿名関数の変換を評価すると、匿名関数自体の構造を表すオブジェクト構造の構築につながります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-564">More precisely, evaluation of the anonymous function conversion leads to the construction of an object structure that represents the structure of the anonymous function itself.</span></span> <span data-ttu-id="87a1f-565">式ツリーの正確な構造と、それを作成するための正確なプロセスは、実装が定義されています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-565">The precise structure of the expression tree, as well as the exact process for creating it, are implementation defined.</span></span>

### <a name="implementation-example"></a><span data-ttu-id="87a1f-566">実装例</span><span class="sxs-lookup"><span data-stu-id="87a1f-566">Implementation example</span></span>

<span data-ttu-id="87a1f-567">このセクションでは、他C#の構造体の観点から、匿名関数の変換を実装する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-567">This section describes a possible implementation of anonymous function conversions in terms of other C# constructs.</span></span> <span data-ttu-id="87a1f-568">ここで説明する実装は、Microsoft C#コンパイラで使用されているのと同じ原則に基づいていますが、必須の実装ではなく、可能な唯一の方法でもあります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-568">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation, nor is it the only one possible.</span></span> <span data-ttu-id="87a1f-569">ここでは、式ツリーへの変換について簡単に説明します。正確なセマンティクスは、この仕様の範囲外です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-569">It only briefly mentions conversions to expression trees, as their exact semantics are outside the scope of this specification.</span></span>

<span data-ttu-id="87a1f-570">このセクションの残りの部分では、さまざまな特性を持つ匿名関数を含むコードの例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-570">The remainder of this section gives several examples of code that contains anonymous functions with different characteristics.</span></span> <span data-ttu-id="87a1f-571">各例では、他のC#コンストラクトのみを使用するコードへの対応する変換が提供されています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-571">For each example, a corresponding translation to code that uses only other C# constructs is provided.</span></span> <span data-ttu-id="87a1f-572">この例では、識別子 `D` は、次のデリゲート型を表すことによって想定されています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-572">In the examples, the identifier `D` is assumed by represent the following delegate type:</span></span>
```csharp
public delegate void D();
```

<span data-ttu-id="87a1f-573">匿名関数の最も単純な形式は、外部変数をキャプチャする関数です。</span><span class="sxs-lookup"><span data-stu-id="87a1f-573">The simplest form of an anonymous function is one that captures no outer variables:</span></span>
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

<span data-ttu-id="87a1f-574">これは、匿名関数のコードが配置される、コンパイラによって生成された静的メソッドを参照するデリゲートのインスタンス化に変換できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-574">This can be translated to a delegate instantiation that references a compiler generated static method in which the code of the anonymous function is placed:</span></span>
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

<span data-ttu-id="87a1f-575">次の例では、匿名関数は `this`のインスタンスメンバーを参照します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-575">In the following example, the anonymous function references instance members of `this`:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

<span data-ttu-id="87a1f-576">これは、匿名関数のコードを含む、コンパイラによって生成されるインスタンスメソッドに変換できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-576">This can be translated to a compiler generated instance method containing the code of the anonymous function:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

<span data-ttu-id="87a1f-577">この例では、匿名関数はローカル変数をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-577">In this example, the anonymous function captures a local variable:</span></span>
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

<span data-ttu-id="87a1f-578">ローカル変数の有効期間は、少なくとも匿名関数デリゲートの有効期間に拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-578">The lifetime of the local variable must now be extended to at least the lifetime of the anonymous function delegate.</span></span> <span data-ttu-id="87a1f-579">これは、コンパイラによって生成されるクラスのフィールドにローカル変数を "hoisting" することによって実現できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-579">This can be achieved by "hoisting" the local variable into a field of a compiler generated class.</span></span> <span data-ttu-id="87a1f-580">ローカル変数 ([ローカル変数のインスタンス化](expressions.md#instantiation-of-local-variables)) のインスタンス化は、コンパイラによって生成されたクラスのインスタンスの作成に対応し、ローカル変数へのアクセスは、コンパイラによって生成されるクラスのインスタンス内のフィールドへのアクセスに対応します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-580">Instantiation of the local variable ([Instantiation of local variables](expressions.md#instantiation-of-local-variables)) then corresponds to creating an instance of the compiler generated class, and accessing the local variable corresponds to accessing a field in the instance of the compiler generated class.</span></span> <span data-ttu-id="87a1f-581">さらに、匿名関数は、コンパイラによって生成されるクラスのインスタンスメソッドになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-581">Furthermore, the anonymous function becomes an instance method of the compiler generated class:</span></span>
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

<span data-ttu-id="87a1f-582">最後に、次の匿名関数は、`this`、および有効期間が異なる2つのローカル変数をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-582">Finally, the following anonymous function captures `this` as well as two local variables with different lifetimes:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

<span data-ttu-id="87a1f-583">ここでは、別のブロックのローカル変数が独立した有効期間を持つことができるように、ローカルがキャプチャされた各ステートメントブロックに対して、コンパイラによって生成されるクラスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-583">Here, a compiler generated class is created for each statement block in which locals are captured such that the locals in the different blocks can have independent lifetimes.</span></span> <span data-ttu-id="87a1f-584">`__Locals2`のインスタンス (内部ステートメントブロックに対してコンパイラによって生成されるクラス) には、ローカル変数 `z` と `__Locals1`のインスタンスを参照するフィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-584">An instance of `__Locals2`, the compiler generated class for the inner statement block, contains the local variable `z` and a field that references an instance of `__Locals1`.</span></span>  <span data-ttu-id="87a1f-585">`__Locals1`のインスタンス (外側のステートメントブロックに対してコンパイラによって生成されるクラス) には、ローカル変数 `y` と、外側の関数メンバーの `this` を参照するフィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-585">An instance of `__Locals1`, the compiler generated class for the outer statement block, contains the local variable `y` and a field that references `this` of the enclosing function member.</span></span> <span data-ttu-id="87a1f-586">これらのデータ構造を使用すると、`__Local2`のインスタンスを介してキャプチャされたすべての外部変数にアクセスできます。そのため、匿名関数のコードは、そのクラスのインスタンスメソッドとして実装できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-586">With these data structures it is possible to reach all captured outer variables through an instance of `__Local2`, and the code of the anonymous function can thus be implemented as an instance method of that class.</span></span>

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

<span data-ttu-id="87a1f-587">ここでは、ローカル変数をキャプチャするために使用するのと同じ手法を、匿名関数を式ツリーに変換するときにも使用できます。コンパイラによって生成されたオブジェクトへの参照は式ツリーに格納でき、ローカル変数へのアクセスは次のようになります。これらのオブジェクトに対するフィールドアクセスとして表されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-587">The same technique applied here to capture local variables can also be used when converting anonymous functions to expression trees: References to the compiler generated objects can be stored in the expression tree, and access to the local variables can be represented as field accesses on these objects.</span></span> <span data-ttu-id="87a1f-588">この方法の利点は、"リフトされた" ローカル変数をデリゲートと式ツリーの間で共有できることです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-588">The advantage of this approach is that it allows the "lifted" local variables to be shared between delegates and expression trees.</span></span>

## <a name="method-group-conversions"></a><span data-ttu-id="87a1f-589">メソッドグループの変換</span><span class="sxs-lookup"><span data-stu-id="87a1f-589">Method group conversions</span></span>

<span data-ttu-id="87a1f-590">メソッドグループ ([式の分類](expressions.md#expression-classifications)) から互換性のあるデリゲート型への暗黙の変換 ([暗黙](conversions.md#implicit-conversions)の変換) が存在します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-590">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from a method group ([Expression classifications](expressions.md#expression-classifications)) to a compatible delegate type.</span></span> <span data-ttu-id="87a1f-591">デリゲート型 `D` と、メソッドグループとして分類される式 `E` が指定されている場合、次に示すように、`E` のパラメーターの型と修飾子を使用して構築された引数リストに、`D`が通常の形式 ([適用可能な関数メンバー](expressions.md#applicable-function-member)) に適用可能なメソッドが少なくとも1つ含まれている場合は、`D` `E` から</span><span class="sxs-lookup"><span data-stu-id="87a1f-591">Given a delegate type `D` and an expression `E` that is classified as a method group, an implicit conversion exists from `E` to `D` if `E` contains at least one method that is applicable in its normal form ([Applicable function member](expressions.md#applicable-function-member)) to an argument list constructed by use of the parameter types and modifiers of `D`, as described in the following.</span></span>

<span data-ttu-id="87a1f-592">メソッドグループからデリゲート `D` 型への変換のコンパイル時アプリケーション `E` については、次の説明を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-592">The compile-time application of a conversion from a method group `E` to a delegate type `D` is described in the following.</span></span> <span data-ttu-id="87a1f-593">`E` から `D` への暗黙的な変換が存在しても、変換のコンパイル時のアプリケーションがエラーなしで成功するとは限りません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-593">Note that the existence of an implicit conversion from `E` to `D` does not guarantee that the compile-time application of the conversion will succeed without error.</span></span>

*  <span data-ttu-id="87a1f-594">フォーム `E(A)`のメソッド呼び出し ([メソッド](expressions.md#method-invocations)の呼び出し) に対応する1つのメソッド `M` が選択されていますが、次のように変更されています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-594">A single method `M` is selected corresponding to a method invocation ([Method invocations](expressions.md#method-invocations)) of the form `E(A)`, with the following modifications:</span></span>
    * <span data-ttu-id="87a1f-595">引数リスト `A` は式のリストであり、それぞれが変数として分類され、`D`の*formal_parameter_list*内の対応するパラメーターの型と修飾子 (`ref` または `out`) を使用します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-595">The argument list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref` or `out`) of the corresponding parameter in the *formal_parameter_list* of `D`.</span></span>
    * <span data-ttu-id="87a1f-596">考慮される候補メソッドは、通常の形式 ([適用可能な関数メンバー](expressions.md#applicable-function-member)) で適用可能なメソッドのみであり、拡張フォームにのみ適用されるものではありません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-596">The candidate methods considered are only those methods that are applicable in their normal form ([Applicable function member](expressions.md#applicable-function-member)), not those applicable only in their expanded form.</span></span>
*  <span data-ttu-id="87a1f-597">[メソッド呼び出し](expressions.md#method-invocations)のアルゴリズムによってエラーが発生した場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-597">If the algorithm of [Method invocations](expressions.md#method-invocations) produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="87a1f-598">それ以外の場合、`D` と同じ数のパラメーターを持ち、変換が存在していると見なされる `M`、1つの最適なメソッドが生成されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-598">Otherwise the algorithm produces a single best method `M` having the same number of parameters as `D` and the conversion is considered to exist.</span></span>
*  <span data-ttu-id="87a1f-599">選択されたメソッド `M` は、デリゲート型 `D`と互換性がある ([デリゲート互換性](delegates.md#delegate-compatibility)) 必要があります。そうでない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-599">The selected method `M` must be compatible ([Delegate compatibility](delegates.md#delegate-compatibility)) with the delegate type `D`, or otherwise, a compile-time error occurs.</span></span>
*  <span data-ttu-id="87a1f-600">選択したメソッド `M` がインスタンスメソッドの場合、`E` に関連付けられたインスタンス式によって、デリゲートのターゲットオブジェクトが決定されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-600">If the selected method `M` is an instance method, the instance expression associated with `E` determines the target object of the delegate.</span></span>
*  <span data-ttu-id="87a1f-601">選択したメソッド M が、インスタンス式でのメンバーアクセスによって示される拡張メソッドである場合、そのインスタンス式によって、デリゲートのターゲットオブジェクトが決定されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-601">If the selected method M is an extension method which is denoted by means of a member access on an instance expression, that instance expression determines the target object of the delegate.</span></span>
*  <span data-ttu-id="87a1f-602">変換の結果は `D`型の値になります。つまり、選択したメソッドとターゲットオブジェクトを参照する新しく作成されたデリゲートです。</span><span class="sxs-lookup"><span data-stu-id="87a1f-602">The result of the conversion is a value of type `D`, namely a newly created delegate that refers to the selected method and target object.</span></span>
*  <span data-ttu-id="87a1f-603">[メソッド呼び出し](expressions.md#method-invocations)のアルゴリズムがインスタンスメソッドを検出できず、拡張メソッドの呼び出し ([拡張メソッド](expressions.md#extension-method-invocations)の呼び出し) として `E(A)` の呼び出しの処理に成功する場合は、このプロセスによって、拡張メソッドへのデリゲートの作成につながる可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-603">Note that this process can lead to the creation of a delegate to an extension method, if the algorithm of [Method invocations](expressions.md#method-invocations) fails to find an instance method but succeeds in processing the invocation of `E(A)` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="87a1f-604">このように作成されたデリゲートは、拡張メソッドとその最初の引数をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="87a1f-604">A delegate thus created captures the extension method as well as its first argument.</span></span>

<span data-ttu-id="87a1f-605">次の例は、メソッドグループの変換を示しています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-605">The following example demonstrates method group conversions:</span></span>
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

<span data-ttu-id="87a1f-606">`d1` への代入は、メソッドグループ `F` を `D1`型の値に暗黙的に変換します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-606">The assignment to `d1` implicitly converts the method group `F` to a value of type `D1`.</span></span>

<span data-ttu-id="87a1f-607">`d2` への代入は、弱い派生型 (反変) のパラメーター型と、より派生した (共変の) 戻り値の型を持つメソッドへのデリゲートを作成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-607">The assignment to `d2` shows how it is possible to create a delegate to a method that has less derived (contravariant) parameter types and a more derived (covariant) return type.</span></span>

<span data-ttu-id="87a1f-608">`d3` への代入は、メソッドが適用されない場合に変換が存在しないことを示します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-608">The assignment to `d3` shows how no conversion exists if the method is not applicable.</span></span>

<span data-ttu-id="87a1f-609">`d4` への割り当ては、メソッドを通常の形式で適用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-609">The assignment to `d4` shows how the method must be applicable in its normal form.</span></span>

<span data-ttu-id="87a1f-610">`d5` への代入は、デリゲートとメソッドのパラメーターと戻り値の型が参照型に対してのみ異なることを許可する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="87a1f-610">The assignment to `d5` shows how parameter and return types of the delegate and method are allowed to differ only for reference types.</span></span>

<span data-ttu-id="87a1f-611">他の暗黙的な変換および明示的な変換と同様に、cast 演算子を使用してメソッドグループの変換を明示的に実行できます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-611">As with all other implicit and explicit conversions, the cast operator can be used to explicitly perform a method group conversion.</span></span> <span data-ttu-id="87a1f-612">この例では、</span><span class="sxs-lookup"><span data-stu-id="87a1f-612">Thus, the example</span></span>
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
<span data-ttu-id="87a1f-613">書き込みが可能</span><span class="sxs-lookup"><span data-stu-id="87a1f-613">could instead be written</span></span>
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

<span data-ttu-id="87a1f-614">メソッドグループは、オーバーロードの解決に影響を与える可能性があり、型の推定に関与します。</span><span class="sxs-lookup"><span data-stu-id="87a1f-614">Method groups may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="87a1f-615">詳細については、「[関数メンバー](expressions.md#function-members) 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="87a1f-615">See [Function members](expressions.md#function-members) for further details.</span></span>

<span data-ttu-id="87a1f-616">メソッドグループの変換の実行時の評価は、次のように行われます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-616">The run-time evaluation of a method group conversion proceeds as follows:</span></span>

*  <span data-ttu-id="87a1f-617">コンパイル時に選択されたメソッドがインスタンスメソッドである場合、またはインスタンスメソッドとしてアクセスされる拡張メソッドである場合は、デリゲートのターゲットオブジェクトは `E`に関連付けられたインスタンス式から決定されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-617">If the method selected at compile-time is an instance method, or it is an extension method which is accessed as an instance method, the target object of the delegate is determined from the instance expression associated with `E`:</span></span>
    * <span data-ttu-id="87a1f-618">インスタンス式が評価されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-618">The instance expression is evaluated.</span></span> <span data-ttu-id="87a1f-619">この評価によって例外が発生した場合、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-619">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="87a1f-620">インスタンス式が*reference_type*の場合、インスタンス式によって計算された値が対象オブジェクトになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-620">If the instance expression is of a *reference_type*, the value computed by the instance expression becomes the target object.</span></span> <span data-ttu-id="87a1f-621">選択したメソッドがインスタンスメソッドで、ターゲットオブジェクトが `null`場合、`System.NullReferenceException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-621">If the selected method is an instance method and the target object is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="87a1f-622">インスタンス式が*value_type*の場合は、値をオブジェクトに変換するボックス化操作 ([ボックス化変換](types.md#boxing-conversions)) が実行され、このオブジェクトが対象オブジェクトになります。</span><span class="sxs-lookup"><span data-stu-id="87a1f-622">If the instance expression is of a *value_type*, a boxing operation ([Boxing conversions](types.md#boxing-conversions)) is performed to convert the value to an object, and this object becomes the target object.</span></span>
*  <span data-ttu-id="87a1f-623">それ以外の場合は、選択したメソッドが静的メソッド呼び出しの一部になり、デリゲートのターゲットオブジェクトが `null`されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-623">Otherwise the selected method is part of a static method call, and the target object of the delegate is `null`.</span></span>
*  <span data-ttu-id="87a1f-624">`D` デリゲート型の新しいインスタンスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-624">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="87a1f-625">新しいインスタンスの割り当てに使用できるメモリが不足している場合は、`System.OutOfMemoryException` がスローされ、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="87a1f-625">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="87a1f-626">新しいデリゲートインスタンスは、コンパイル時に決定されたメソッドへの参照と、上記で計算されたターゲットオブジェクトへの参照を使用して初期化されます。</span><span class="sxs-lookup"><span data-stu-id="87a1f-626">The new delegate instance is initialized with a reference to the method that was determined at compile-time and a reference to the target object computed above.</span></span>
