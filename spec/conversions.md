# <a name="conversions"></a><span data-ttu-id="f7a0d-101">変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-101">Conversions</span></span>

<span data-ttu-id="f7a0d-102">A***変換***式を特定の型として扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-102">A ***conversion*** enables an expression to be treated as being of a particular type.</span></span> <span data-ttu-id="f7a0d-103">変換を別の種類を持つものとして扱う特定の型の式が発生する可能性があります。 または型を取得する型のない式があります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-103">A conversion may cause an expression of a given type to be treated as having a different type, or it may cause an expression without a type to get a type.</span></span> <span data-ttu-id="f7a0d-104">変換***暗黙的な***または***明示的な***、および明示的なキャストが必要かどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-104">Conversions can be ***implicit*** or ***explicit***, and this determines whether an explicit cast is required.</span></span> <span data-ttu-id="f7a0d-105">型からの変換、`int`入力`long`が暗黙的な型の式をその`int`型として暗黙的に処理できる`long`。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-105">For instance, the conversion from type `int` to type `long` is implicit, so expressions of type `int` can implicitly be treated as type `long`.</span></span> <span data-ttu-id="f7a0d-106">型からの逆の変換`long`入力`int`は明示的なため、明示的なキャストが必要です。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-106">The opposite conversion, from type `long` to type `int`, is explicit and so an explicit cast is required.</span></span>

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

<span data-ttu-id="f7a0d-107">一部の変換は言語によって定義されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-107">Some conversions are defined by the language.</span></span> <span data-ttu-id="f7a0d-108">プログラムは、独自の型変換を定義も可能性があります ([ユーザー定義の変換](conversions.md#user-defined-conversions))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-108">Programs may also define their own conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

## <a name="implicit-conversions"></a><span data-ttu-id="f7a0d-109">暗黙の変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-109">Implicit conversions</span></span>

<span data-ttu-id="f7a0d-110">次の変換は、暗黙的な変換として分類されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-110">The following conversions are classified as implicit conversions:</span></span>

*  <span data-ttu-id="f7a0d-111">恒等変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-111">Identity conversions</span></span>
*  <span data-ttu-id="f7a0d-112">暗黙的な数値変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-112">Implicit numeric conversions</span></span>
*  <span data-ttu-id="f7a0d-113">列挙体の暗黙的な変換です。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-113">Implicit enumeration conversions.</span></span>
*  <span data-ttu-id="f7a0d-114">Null 許容型の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-114">Implicit nullable conversions</span></span>
*  <span data-ttu-id="f7a0d-115">Null リテラルの変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-115">Null literal conversions</span></span>
*  <span data-ttu-id="f7a0d-116">暗黙的な参照変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-116">Implicit reference conversions</span></span>
*  <span data-ttu-id="f7a0d-117">ボックス化変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-117">Boxing conversions</span></span>
*  <span data-ttu-id="f7a0d-118">動的の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-118">Implicit dynamic conversions</span></span>
*  <span data-ttu-id="f7a0d-119">定数式が暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-119">Implicit constant expression conversions</span></span>
*  <span data-ttu-id="f7a0d-120">ユーザー定義の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-120">User-defined implicit conversions</span></span>
*  <span data-ttu-id="f7a0d-121">匿名関数の変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-121">Anonymous function conversions</span></span>
*  <span data-ttu-id="f7a0d-122">メソッド グループ変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-122">Method group conversions</span></span>

<span data-ttu-id="f7a0d-123">暗黙的な変換は、さまざまな関数メンバーの呼び出しをなどの状況で発生することができます ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))、キャスト式 ([キャスト式](expressions.md#cast-expressions))、割り当て ([代入演算子](expressions.md#assignment-operators))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-123">Implicit conversions can occur in a variety of situations, including function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), and assignments ([Assignment operators](expressions.md#assignment-operators)).</span></span>

<span data-ttu-id="f7a0d-124">定義済みの暗黙的な変換では、常に成功しがスローされる例外は発生しません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-124">The pre-defined implicit conversions always succeed and never cause exceptions to be thrown.</span></span> <span data-ttu-id="f7a0d-125">適切にデザインされたユーザー定義の暗黙的な変換では、これらの特性もを示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-125">Properly designed user-defined implicit conversions should exhibit these characteristics as well.</span></span>

<span data-ttu-id="f7a0d-126">型の変換のための`object`と`dynamic`同等と見なされます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-126">For the purposes of conversion, the types `object` and `dynamic` are considered equivalent.</span></span>

<span data-ttu-id="f7a0d-127">ただし、動的な変換 ([動的の暗黙的な変換](conversions.md#implicit-dynamic-conversions)と[動的の明示的な変換](conversions.md#explicit-dynamic-conversions)) 型の式にのみ適用されます`dynamic`([動的な型](types.md#the-dynamic-type)).</span><span class="sxs-lookup"><span data-stu-id="f7a0d-127">However, dynamic conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)) apply only to expressions of type `dynamic` ([The dynamic type](types.md#the-dynamic-type)).</span></span>

### <a name="identity-conversion"></a><span data-ttu-id="f7a0d-128">Id 変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-128">Identity conversion</span></span>

<span data-ttu-id="f7a0d-129">Id 変換は、任意の型から同じ型に変換します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-129">An identity conversion converts from any type to the same type.</span></span> <span data-ttu-id="f7a0d-130">この変換には、その型に変換可能であるエンティティを既に必要な型を持つことが言えますようにが存在します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-130">This conversion exists such that an entity that already has a required type can be said to be convertible to that type.</span></span>

*  <span data-ttu-id="f7a0d-131">Id の間で変換があるオブジェクトと動的同等と見なされるため、`object`と`dynamic`、間のすべての出現を置換するときに、同じ構築された型および`dynamic`で`object`。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-131">Because object and dynamic are considered equivalent there is an identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing all occurrences of `dynamic` with `object`.</span></span>

### <a name="implicit-numeric-conversions"></a><span data-ttu-id="f7a0d-132">暗黙的な数値変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-132">Implicit numeric conversions</span></span>

<span data-ttu-id="f7a0d-133">暗黙的な数値変換は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-133">The implicit numeric conversions are:</span></span>

*  <span data-ttu-id="f7a0d-134">`sbyte`に`short`、 `int`、 `long`、 `float`、 `double`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-134">From `sbyte` to `short`, `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-135">`byte`に`short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `float`、 `double`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-135">From `byte` to `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-136">`short`に`int`、 `long`、 `float`、 `double`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-136">From `short` to `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-137">`ushort`に`int`、 `uint`、 `long`、 `ulong`、 `float`、 `double`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-137">From `ushort` to `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-138">`int`に`long`、 `float`、 `double`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-138">From `int` to `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-139">`uint`に`long`、 `ulong`、 `float`、 `double`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-139">From `uint` to `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-140">`long`に`float`、 `double`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-140">From `long` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-141">`ulong`に`float`、 `double`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-141">From `ulong` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-142">`char`に`ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `float`、 `double`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-142">From `char` to `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-143">`float`に`double`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-143">From `float` to `double`.</span></span>

<span data-ttu-id="f7a0d-144">変換`int`、 `uint`、 `long`、または`ulong`に`float`との間`long`または`ulong`に`double`により、有効桁数が失われる可能性がありますが、1 桁の損失しない原因は。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-144">Conversions from `int`, `uint`, `long`, or `ulong` to `float` and from `long` or `ulong` to `double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="f7a0d-145">その他の暗黙的な数値変換情報は失われません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-145">The other implicit numeric conversions never lose any information.</span></span>

<span data-ttu-id="f7a0d-146">暗黙的な変換がない、`char`型であるために、その他の整数型の値は自動的に変換されない、`char`型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-146">There are no implicit conversions to the `char` type, so values of the other integral types do not automatically convert to the `char` type.</span></span>

### <a name="implicit-enumeration-conversions"></a><span data-ttu-id="f7a0d-147">列挙体の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-147">Implicit enumeration conversions</span></span>

<span data-ttu-id="f7a0d-148">列挙体の暗黙的な変換を許可、 *decimal_integer_literal* `0`いずれかに変換する*enum_type*しに*nullable_type*が基になる型は、 *enum_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-148">An implicit enumeration conversion permits the *decimal_integer_literal* `0` to be converted to any *enum_type* and to any *nullable_type* whose underlying type is an *enum_type*.</span></span> <span data-ttu-id="f7a0d-149">後者の場合、変換は、基に変換することによって評価*enum_type*結果をラップし、([null 許容型](types.md#nullable-types))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-149">In the latter case the conversion is evaluated by converting to the underlying *enum_type* and wrapping the result ([Nullable types](types.md#nullable-types)).</span></span>

### <a name="implicit-interpolated-string-conversions"></a><span data-ttu-id="f7a0d-150">補間文字列の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-150">Implicit interpolated string conversions</span></span>

<span data-ttu-id="f7a0d-151">暗黙の補間文字列変換により、 *interpolated_string_expression* ([文字列補間](expressions.md#interpolated-strings)) に変換する`System.IFormattable`または`System.FormattableString`(を実装する`System.IFormattable`).</span><span class="sxs-lookup"><span data-stu-id="f7a0d-151">An implicit interpolated string conversion permits an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)) to be converted to `System.IFormattable` or `System.FormattableString` (which implements `System.IFormattable`).</span></span>

<span data-ttu-id="f7a0d-152">この変換が適用されるときに文字列値はありません補間文字列から構成されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-152">When this conversion is applied a string value is not composed from the interpolated string.</span></span> <span data-ttu-id="f7a0d-153">代わりに、インスタンスの`System.FormattableString`作成は、その詳細に説明[文字列補間](expressions.md#interpolated-strings)します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-153">Instead an instance of `System.FormattableString` is created, as further described in [Interpolated strings](expressions.md#interpolated-strings).</span></span>

### <a name="implicit-nullable-conversions"></a><span data-ttu-id="f7a0d-154">Null 許容型の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-154">Implicit nullable conversions</span></span>

<span data-ttu-id="f7a0d-155">Null 非許容値型を操作する定義済みの暗黙的な変換は、これらの型の null 許容の形式でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-155">Predefined implicit conversions that operate on non-nullable value types can also be used with nullable forms of those types.</span></span> <span data-ttu-id="f7a0d-156">定義済みの暗黙的な id と null 非許容値型から変換する数値変換の各`S`null 非許容値型に`T`、次の暗黙的な null 許容型の変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-156">For each of the predefined implicit identity and numeric conversions that convert from a non-nullable value type `S` to a non-nullable value type `T`, the following implicit nullable conversions exist:</span></span>

*  <span data-ttu-id="f7a0d-157">暗黙的な変換`S?`に`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-157">An implicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="f7a0d-158">暗黙的な変換`S`に`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-158">An implicit conversion from `S` to `T?`.</span></span>

<span data-ttu-id="f7a0d-159">Null 許容型の暗黙的な変換の評価がから基になる変換に基づく`S`に`T`次のように進みます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-159">Evaluation of an implicit nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="f7a0d-160">場合は、null 許容型の変換から`S?`に`T?`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-160">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="f7a0d-161">元の値が null の場合 (`HasValue`プロパティが false)、結果は、型の null 値`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-161">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="f7a0d-162">ラップ解除とそれ以外の場合、変換の評価は`S?`に`S`からの変換を基になると、その後`S`に`T`、その後に、ラップ ([null 許容型](types.md#nullable-types))から`T`に`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-162">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping ([Nullable types](types.md#nullable-types)) from `T` to `T?`.</span></span>

*  <span data-ttu-id="f7a0d-163">場合は、null 許容型の変換から`S`に`T?`から基になる変換と変換の評価は`S`に`T`からの折り返しを続けて`T`に`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-163">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>

### <a name="null-literal-conversions"></a><span data-ttu-id="f7a0d-164">Null リテラルの変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-164">Null literal conversions</span></span>

<span data-ttu-id="f7a0d-165">暗黙的な変換が存在する、`null`リテラルを任意の null 許容型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-165">An implicit conversion exists from the `null` literal to any nullable type.</span></span> <span data-ttu-id="f7a0d-166">この変換には、null 値が生成されます ([null 許容型](types.md#nullable-types)) の指定された null 許容型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-166">This conversion produces the null value ([Nullable types](types.md#nullable-types)) of the given nullable type.</span></span>

### <a name="implicit-reference-conversions"></a><span data-ttu-id="f7a0d-167">暗黙的な参照変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-167">Implicit reference conversions</span></span>

<span data-ttu-id="f7a0d-168">暗黙の参照変換は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-168">The implicit reference conversions are:</span></span>

*  <span data-ttu-id="f7a0d-169">いずれかから*reference_type*に`object`と`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-169">From any *reference_type* to `object` and `dynamic`.</span></span>
*  <span data-ttu-id="f7a0d-170">いずれかから*class_type* `S`いずれかに*class_type* `T`に用意されている`S`から派生`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-170">From any *class_type* `S` to any *class_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="f7a0d-171">いずれかから*class_type* `S`いずれかに*interface_type* `T`に用意されている`S`実装`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-171">From any *class_type* `S` to any *interface_type* `T`, provided `S` implements `T`.</span></span>
*  <span data-ttu-id="f7a0d-172">いずれかから*interface_type* `S`いずれかに*interface_type* `T`に用意されている`S`から派生`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-172">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="f7a0d-173">*Array_type* `S`要素型が`SE`を*array_type* `T`要素型が`TE`true は、次のすべての提供。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-173">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="f7a0d-174">`S` `T`要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-174">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="f7a0d-175">つまり、`S`と`T`同じ次元数を持ちます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-175">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="f7a0d-176">両方`SE`と`TE`は*reference_type*秒。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-176">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="f7a0d-177">暗黙の参照変換が存在する`SE`に`TE`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-177">An implicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="f7a0d-178">いずれかから*array_type*に`System.Array`とインターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-178">From any *array_type* to `System.Array` and the interfaces it implements.</span></span>
*  <span data-ttu-id="f7a0d-179">1 次元配列型から`S[]`に`System.Collections.Generic.IList<T>`とからの暗黙的な id または参照変換が提供される、基本インターフェイス`S`に`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-179">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an implicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="f7a0d-180">いずれかから*delegate_type*に`System.Delegate`とインターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-180">From any *delegate_type* to `System.Delegate` and the interfaces it implements.</span></span>
*  <span data-ttu-id="f7a0d-181">いずれかに null リテラルから*reference_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-181">From the null literal to any *reference_type*.</span></span>
*  <span data-ttu-id="f7a0d-182">いずれかから*reference_type*を*reference_type* `T`への暗黙的な id または参照変換がある場合、 *reference_type* `T0`と`T0`が恒等変換を`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-182">From any *reference_type* to a *reference_type* `T` if it has an implicit identity or reference conversion to a *reference_type* `T0` and `T0` has an identity conversion to `T`.</span></span>
*  <span data-ttu-id="f7a0d-183">いずれかから*reference_type* 、インターフェイスまたはデリゲート型に`T`インターフェイスまたはデリゲート型への暗黙的な id または参照変換がある場合`T0`と`T0`(の差異に変換できるは[分散変換](interfaces.md#variance-conversion)) に`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-183">From any *reference_type* to an interface or delegate type `T` if it has an implicit identity or reference conversion to an interface or delegate type `T0` and `T0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `T`.</span></span>
*  <span data-ttu-id="f7a0d-184">使用する暗黙的な変換では、参照型にすることがわかっているパラメーターを入力します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-184">Implicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="f7a0d-185">参照してください[型パラメーターを使用する暗黙的な変換](conversions.md#implicit-conversions-involving-type-parameters)型パラメーターを使用する暗黙的な変換の詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-185">See [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) for more details on implicit conversions involving type parameters.</span></span>

<span data-ttu-id="f7a0d-186">暗黙の参照変換が変換の間で*reference_type*を常に成功することが保証され、そのため、実行時のチェックは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-186">The implicit reference conversions are those conversions between *reference_type*s that can be proven to always succeed, and therefore require no checks at run-time.</span></span>

<span data-ttu-id="f7a0d-187">参照変換、暗黙的または明示的には、変換されるオブジェクトの参照 id を変更することはありません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-187">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="f7a0d-188">つまり、参照変換は、参照の種類を変更することが、変更しません、型または参照されるオブジェクトの値。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-188">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="f7a0d-189">ボックス化変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-189">Boxing conversions</span></span>

<span data-ttu-id="f7a0d-190">ボックス化変換では、 *value_type*参照型に暗黙的に変換します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-190">A boxing conversion permits a *value_type* to be implicitly converted to a reference type.</span></span> <span data-ttu-id="f7a0d-191">いずれかからボックス化変換が存在する*non_nullable_value_type*に`object`と`dynamic`を`System.ValueType`しに*interface_type*によって実装される、 *non_nullable_value_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-191">A boxing conversion exists from any *non_nullable_value_type* to `object` and `dynamic`, to `System.ValueType` and to any *interface_type* implemented by the *non_nullable_value_type*.</span></span> <span data-ttu-id="f7a0d-192">さらに、 *enum_type*型に変換できる`System.Enum`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-192">Furthermore an *enum_type* can be converted to the type `System.Enum`.</span></span>

<span data-ttu-id="f7a0d-193">ボックス化変換が存在する、 *nullable_type* 、参照型をボックス化変換場合にのみが存在する、基礎となる*non_nullable_value_type*参照型にします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-193">A boxing conversion exists from a *nullable_type* to a reference type, if and only if a boxing conversion exists from the underlying *non_nullable_value_type* to the reference type.</span></span>

<span data-ttu-id="f7a0d-194">値の型がインターフェイス型にボックス化変換`I`かどうかは、インターフェイス型にボックス化変換`I0`と`I0`が恒等変換を`I`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-194">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="f7a0d-195">値の型がインターフェイス型にボックス化変換`I`、インターフェイスまたはデリゲート型にボックス化変換がある場合`I0`と`I0`差異に変換できるは ([分散変換](interfaces.md#variance-conversion))に`I`.</span><span class="sxs-lookup"><span data-stu-id="f7a0d-195">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface or delegate type `I0` and `I0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `I`.</span></span>

<span data-ttu-id="f7a0d-196">値をボックス化、 *non_nullable_value_type*オブジェクト インスタンスの割り当てとコピーから成る、 *value_type*値をそのインスタンスにします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-196">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *value_type* value into that instance.</span></span> <span data-ttu-id="f7a0d-197">構造体は、型にボックス化される`System.ValueType`すべての構造体の基本クラスでは、([継承](structs.md#inheritance))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-197">A struct can be boxed to the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="f7a0d-198">値をボックス化、 *nullable_type*次のように進みます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-198">Boxing a value of a *nullable_type* proceeds as follows:</span></span>

*  <span data-ttu-id="f7a0d-199">元の値が null の場合 (`HasValue`プロパティが false)、対象の型の参照を null になります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-199">If the source value is null (`HasValue` property is false), the result is a null reference of the target type.</span></span>
*  <span data-ttu-id="f7a0d-200">それ以外の場合、結果は、ボックス化されたへの参照を`T`ラップの解除と、元の値をボックス化によって生成されました。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-200">Otherwise, the result is a reference to a boxed `T` produced by unwrapping and boxing the source value.</span></span>

<span data-ttu-id="f7a0d-201">ボックス化変換の詳細については[ボックス化変換](types.md#boxing-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-201">Boxing conversions are described further in [Boxing conversions](types.md#boxing-conversions).</span></span>

### <a name="implicit-dynamic-conversions"></a><span data-ttu-id="f7a0d-202">動的の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-202">Implicit dynamic conversions</span></span>

<span data-ttu-id="f7a0d-203">型の式から動的の暗黙的な変換が存在する`dynamic`任意の型を`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-203">An implicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="f7a0d-204">変換が動的にバインドされている ([動的バインド](expressions.md#dynamic-binding))、実行時に式の実行時の型からの暗黙的な変換が求められますことを意味する`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-204">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an implicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="f7a0d-205">変換が見つからない場合は、実行時に例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-205">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="f7a0d-206">この暗黙の変換は、の先頭でアドバイスを一見違反に注意してください。[暗黙的な変換](conversions.md#implicit-conversions)暗黙的な変換では、例外は発生しません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-206">Note that this implicit conversion seemingly violates the advice in the beginning of [Implicit conversions](conversions.md#implicit-conversions) that an implicit conversion should never cause an exception.</span></span> <span data-ttu-id="f7a0d-207">ただしない自体には、変換が、*検索*の変換、例外が発生しました。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-207">However it is not the conversion itself, but the *finding* of the conversion that causes the exception.</span></span> <span data-ttu-id="f7a0d-208">実行時の例外のリスクは、動的バインドの使用に伴うです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-208">The risk of run-time exceptions is inherent in the use of dynamic binding.</span></span> <span data-ttu-id="f7a0d-209">変換の動的バインドが望ましくない場合、式最初に変換できる`object`とし、目的の型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-209">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="f7a0d-210">次の例は、動的の暗黙的な変換を示しています。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-210">The following example illustrates implicit dynamic conversions:</span></span>

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

<span data-ttu-id="f7a0d-211">割り当て`s2`と`i`両方で操作のバインディングが実行時まで中断されている、動的な暗黙的な変換を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-211">The assignments to `s2` and `i` both employ implicit dynamic conversions, where the binding of the operations is suspended until run-time.</span></span> <span data-ttu-id="f7a0d-212">実行時の型から実行時に、暗黙的な変換のシーク`d`  --  `string` --ターゲットの型にします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-212">At run-time, implicit conversions are sought from the run-time type of `d` -- `string` -- to the target type.</span></span> <span data-ttu-id="f7a0d-213">変換が検出された`string`には`int`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-213">A conversion is found to `string` but not to `int`.</span></span>

### <a name="implicit-constant-expression-conversions"></a><span data-ttu-id="f7a0d-214">定数式が暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-214">Implicit constant expression conversions</span></span>

<span data-ttu-id="f7a0d-215">定数式が暗黙的な変換を次の変換を許可します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-215">An implicit constant expression conversion permits the following conversions:</span></span>

*  <span data-ttu-id="f7a0d-216">A *constant_expression* ([定数式](expressions.md#constant-expressions)) 型の`int`型に変換できます`sbyte`、 `byte`、 `short`、 `ushort`、 `uint`、または`ulong`の値を提供、 *constant_expression*が変換先の型の範囲内です。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-216">A *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) of type `int` can be converted to type `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the *constant_expression* is within the range of the destination type.</span></span>
*  <span data-ttu-id="f7a0d-217">A *constant_expression*型の`long`型に変換できます`ulong`の値を提供、 *constant_expression*が負でないです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-217">A *constant_expression* of type `long` can be converted to type `ulong`, provided the value of the *constant_expression* is not negative.</span></span>

### <a name="implicit-conversions-involving-type-parameters"></a><span data-ttu-id="f7a0d-218">型パラメーターを伴う暗黙の変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-218">Implicit conversions involving type parameters</span></span>

<span data-ttu-id="f7a0d-219">次の暗黙的な変換が特定の型パラメーターの存在`T`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-219">The following implicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="f7a0d-220">`T`効果的な基底クラスに`C`から`T`の任意の基本クラスを`C`との間`T`によって実装されるインターフェイスを任意に`C`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-220">From `T` to its effective base class `C`, from `T` to any base class of `C`, and from `T` to any interface implemented by `C`.</span></span> <span data-ttu-id="f7a0d-221">At 実行時に、if`T`値の型は、ボックス化変換と変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-221">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="f7a0d-222">それ以外の場合、変換は暗黙的な参照変換または id 変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-222">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="f7a0d-223">`T`インターフェイス型に`I`で`T`の有効なインターフェイスのセットとの間`T`の任意の基本インターフェイスを`I`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-223">From `T` to an interface type `I` in `T`'s effective interface set and from `T` to any base interface of `I`.</span></span> <span data-ttu-id="f7a0d-224">At 実行時に、if`T`値の型は、ボックス化変換と変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-224">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="f7a0d-225">それ以外の場合、変換は暗黙的な参照変換または id 変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-225">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="f7a0d-226">`T`型パラメーターに`U`に用意されている`T`異なります`U`([パラメーターの制約入力](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-226">From `T` to a type parameter `U`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="f7a0d-227">実行時に、if`U`値の型には`T`と`U`必ずしも同じ型では、変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-227">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="f7a0d-228">の場合`T`値の型がボックス化変換と変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-228">Otherwise, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="f7a0d-229">それ以外の場合、変換は暗黙的な参照変換または id 変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-229">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="f7a0d-230">Null リテラルをから`T`に用意されている`T`参照型があることがわかっています。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-230">From the null literal to `T`, provided `T` is known to be a reference type.</span></span>
*  <span data-ttu-id="f7a0d-231">`T`参照型に`I`参照型への暗黙的な変換があるかどうかは`S0`と`S0`が恒等変換を`S`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-231">From `T` to a reference type `I` if it has an implicit conversion to a reference type `S0` and `S0` has an identity conversion to `S`.</span></span> <span data-ttu-id="f7a0d-232">実行時に、変換はへの変換と同様な実行`S0`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-232">At run-time the conversion is executed the same way as the conversion to `S0`.</span></span>
*  <span data-ttu-id="f7a0d-233">`T`インターフェイス型に`I`インターフェイスまたはデリゲート型への暗黙的な変換がある場合`I0`と`I0`差異に変換できるに`I`([分散変換](interfaces.md#variance-conversion)).</span><span class="sxs-lookup"><span data-stu-id="f7a0d-233">From `T` to an interface type `I` if it has an implicit conversion to an interface or delegate type `I0` and `I0` is variance-convertible to `I` ([Variance conversion](interfaces.md#variance-conversion)).</span></span> <span data-ttu-id="f7a0d-234">At 実行時に、if`T`値の型は、ボックス化変換と変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-234">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="f7a0d-235">それ以外の場合、変換は暗黙的な参照変換または id 変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-235">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="f7a0d-236">場合`T`参照型がわかっている ([パラメーターの制約入力](classes.md#type-parameter-constraints))、上記の変換はすべて暗黙的な参照変換として分類 ([暗黙の参照変換](conversions.md#implicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-236">If `T` is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)), the conversions above are all classified as implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="f7a0d-237">場合`T`は上記の変換は参照型であるとわかっていない、ボックス化変換に分類されます ([ボックス化変換](conversions.md#boxing-conversions))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-237">If `T` is not known to be a reference type, the conversions above are classified as boxing conversions ([Boxing conversions](conversions.md#boxing-conversions)).</span></span>

### <a name="user-defined-implicit-conversions"></a><span data-ttu-id="f7a0d-238">ユーザー定義の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-238">User-defined implicit conversions</span></span>

<span data-ttu-id="f7a0d-239">省略可能な標準的な暗黙的な変換、省略可能な別の標準的な暗黙的な変換の後に、暗黙的な変換のユーザー定義演算子の実行後に、ユーザー定義の暗黙的な変換で構成されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-239">A user-defined implicit conversion consists of an optional standard implicit conversion, followed by execution of a user-defined implicit conversion operator, followed by another optional standard implicit conversion.</span></span> <span data-ttu-id="f7a0d-240">ユーザー定義の暗黙的な変換を評価するための正確な規則については、「[暗黙的な変換のユーザー定義の処理](conversions.md#processing-of-user-defined-implicit-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-240">The exact rules for evaluating user-defined implicit conversions are described in [Processing of user-defined implicit conversions](conversions.md#processing-of-user-defined-implicit-conversions).</span></span>

### <a name="anonymous-function-conversions-and-method-group-conversions"></a><span data-ttu-id="f7a0d-241">匿名関数の変換とメソッド グループ変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-241">Anonymous function conversions and method group conversions</span></span>

<span data-ttu-id="f7a0d-242">匿名関数とメソッドのグループ自体の型がありませんが、デリゲート型または式ツリー型に暗黙的に変換可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-242">Anonymous functions and method groups do not have types in and of themselves, but may be implicitly converted to delegate types or expression tree types.</span></span> <span data-ttu-id="f7a0d-243">匿名関数の変換がで詳しく説明されている[匿名関数の変換](conversions.md#anonymous-function-conversions)とで、メソッド グループ変換[メソッド グループ変換](conversions.md#method-group-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-243">Anonymous function conversions are described in more detail in [Anonymous function conversions](conversions.md#anonymous-function-conversions) and method group conversions in [Method group conversions](conversions.md#method-group-conversions).</span></span>

## <a name="explicit-conversions"></a><span data-ttu-id="f7a0d-244">明示的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-244">Explicit conversions</span></span>

<span data-ttu-id="f7a0d-245">次の変換は、明示的な変換として分類されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-245">The following conversions are classified as explicit conversions:</span></span>

*  <span data-ttu-id="f7a0d-246">すべての暗黙的な変換です。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-246">All implicit conversions.</span></span>
*  <span data-ttu-id="f7a0d-247">明示的な数値変換します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-247">Explicit numeric conversions.</span></span>
*  <span data-ttu-id="f7a0d-248">明示的な列挙値の変換。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-248">Explicit enumeration conversions.</span></span>
*  <span data-ttu-id="f7a0d-249">明示的な null 許容変換します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-249">Explicit nullable conversions.</span></span>
*  <span data-ttu-id="f7a0d-250">明示的な参照変換します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-250">Explicit reference conversions.</span></span>
*  <span data-ttu-id="f7a0d-251">変換の明示的なインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-251">Explicit interface conversions.</span></span>
*  <span data-ttu-id="f7a0d-252">ボックス化解除変換します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-252">Unboxing conversions.</span></span>
*  <span data-ttu-id="f7a0d-253">明示的な動的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-253">Explicit dynamic conversions</span></span>
*  <span data-ttu-id="f7a0d-254">ユーザー定義の明示的な変換です。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-254">User-defined explicit conversions.</span></span>

<span data-ttu-id="f7a0d-255">明示的な変換はキャスト式で実行できます ([キャスト式](expressions.md#cast-expressions))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-255">Explicit conversions can occur in cast expressions ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="f7a0d-256">明示的な変換のセットには、すべての暗黙的な変換が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-256">The set of explicit conversions includes all implicit conversions.</span></span> <span data-ttu-id="f7a0d-257">これは、冗長なキャスト式が許可されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-257">This means that redundant cast expressions are allowed.</span></span>

<span data-ttu-id="f7a0d-258">暗黙的な変換ではない明示的な変換は、明示的なメリットを十分にさまざまな種類のドメイン間で変換を常に成功を証明することはできません、情報が失われる可能性があることがわかっている変換および変換表記法。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-258">The explicit conversions that are not implicit conversions are conversions that cannot be proven to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit explicit notation.</span></span>

### <a name="explicit-numeric-conversions"></a><span data-ttu-id="f7a0d-259">明示的な数値変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-259">Explicit numeric conversions</span></span>

<span data-ttu-id="f7a0d-260">明示的な数値変換は、変換を*numeric_type*間*numeric_type*を暗黙的な数値変換 ([暗黙的な数値変換](conversions.md#implicit-numeric-conversions))既に存在しません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-260">The explicit numeric conversions are the conversions from a *numeric_type* to another *numeric_type* for which an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) does not already exist:</span></span>

*  <span data-ttu-id="f7a0d-261">`sbyte`に`byte`、 `ushort`、 `uint`、 `ulong`、または`char`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-261">From `sbyte` to `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="f7a0d-262">`byte`に`sbyte`と`char`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-262">From `byte` to `sbyte` and `char`.</span></span>
*  <span data-ttu-id="f7a0d-263">`short`に`sbyte`、 `byte`、 `ushort`、 `uint`、 `ulong`、または`char`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-263">From `short` to `sbyte`, `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="f7a0d-264">`ushort`に`sbyte`、 `byte`、 `short`、または`char`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-264">From `ushort` to `sbyte`, `byte`, `short`, or `char`.</span></span>
*  <span data-ttu-id="f7a0d-265">`int`に`sbyte`、 `byte`、 `short`、 `ushort`、 `uint`、 `ulong`、または`char`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-265">From `int` to `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="f7a0d-266">`uint`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、または`char`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-266">From `uint` to `sbyte`, `byte`, `short`, `ushort`, `int`, or `char`.</span></span>
*  <span data-ttu-id="f7a0d-267">`long`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `ulong`、または`char`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-267">From `long` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="f7a0d-268">`ulong`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、または`char`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-268">From `ulong` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `char`.</span></span>
*  <span data-ttu-id="f7a0d-269">`char`に`sbyte`、 `byte`、または`short`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-269">From `char` to `sbyte`, `byte`, or `short`.</span></span>
*  <span data-ttu-id="f7a0d-270">`float`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-270">From `float` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-271">`double`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-271">From `double` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-272">`decimal`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、または`double`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-272">From `decimal` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `double`.</span></span>

<span data-ttu-id="f7a0d-273">いずれかから変換することは常に明示的な変換では、すべての明示的および暗黙的な数値変換が含まれる、ため*numeric_type*他*numeric_type*キャスト式 (を使用します。[キャスト式](expressions.md#cast-expressions))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-273">Because the explicit conversions include all implicit and explicit numeric conversions, it is always possible to convert from any *numeric_type* to any other *numeric_type* using a cast expression ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="f7a0d-274">明示的な数値変換では、情報が失われる可能性がありますかがスローされる例外が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-274">The explicit numeric conversions possibly lose information or possibly cause exceptions to be thrown.</span></span> <span data-ttu-id="f7a0d-275">明示的な数値変換は、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-275">An explicit numeric conversion is processed as follows:</span></span>

*  <span data-ttu-id="f7a0d-276">整数型から別の整数型への変換では、処理は、オーバーフロー チェック コンテキストに依存 ([checked と unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) 移動変換では、配置します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-276">For a conversion from an integral type to another integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="f7a0d-277">`checked`コンテキスト、ソース オペランドの値が変換先の型の範囲内にもスローする場合、変換に成功した、`System.OverflowException`ソース オペランドの値が変換先の型の範囲外にある場合。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-277">In a `checked` context, the conversion succeeds if the value of the source operand is within the range of the destination type, but throws a `System.OverflowException` if the value of the source operand is outside the range of the destination type.</span></span>
    * <span data-ttu-id="f7a0d-278">`unchecked`コンテキスト変換常に成功し、次のように進みます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-278">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="f7a0d-279">変換元の型が変換先の型より大きい場合、変換元の値はその "余分な" 最上位ビットを破棄することで切り詰められます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-279">If the source type is larger than the destination type, then the source value is truncated by discarding its "extra" most significant bits.</span></span> <span data-ttu-id="f7a0d-280">結果は変換先の型の値として扱われます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-280">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="f7a0d-281">変換元の型が変換先の型より小さい場合、変換元の値は変換先の型と同じサイズになるように符号かゼロが拡張されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-281">If the source type is smaller than the destination type, then the source value is either sign-extended or zero-extended so that it is the same size as the destination type.</span></span> <span data-ttu-id="f7a0d-282">変換元の型に符号が付いている場合は符号拡張が利用され、符号が付いていない場合はゼロ拡張が利用されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-282">Sign-extension is used if the source type is signed; zero-extension is used if the source type is unsigned.</span></span> <span data-ttu-id="f7a0d-283">結果は変換先の型の値として扱われます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-283">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="f7a0d-284">変換元の型が変換先の型と同じサイズの場合、変換元の値は変換先の型の値として扱われます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-284">If the source type is the same size as the destination type, then the source value is treated as a value of the destination type.</span></span>
*  <span data-ttu-id="f7a0d-285">変換の`decimal`、整数型にソース値が最も近い整数値を 0 方向に丸められ、この整数値の変換の結果になります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-285">For a conversion from `decimal` to an integral type, the source value is rounded towards zero to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="f7a0d-286">結果の整数値が変換先の型の範囲外の場合、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-286">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="f7a0d-287">変換の`float`または`double`を整数型、処理は、オーバーフロー チェック コンテキストに依存 ([checked と unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) 変換が、配置。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-287">For a conversion from `float` or `double` to an integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="f7a0d-288">`checked`コンテキスト、変換処理次のようにします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-288">In a `checked` context, the conversion proceeds as follows:</span></span>
        * <span data-ttu-id="f7a0d-289">オペランドの値が NaN、無限、またはの場合、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-289">If the value of the operand is NaN or infinite, a `System.OverflowException` is thrown.</span></span>
        * <span data-ttu-id="f7a0d-290">それ以外の場合、ソース オペランドは最も近い整数値を 0 方向に丸められます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-290">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="f7a0d-291">場合、この整数値を変換先の型の範囲内でこの値は変換の結果です。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-291">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="f7a0d-292">それ以外の場合は、`System.OverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-292">Otherwise, a `System.OverflowException` is thrown.</span></span>
    * <span data-ttu-id="f7a0d-293">`unchecked`コンテキスト変換常に成功し、次のように進みます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-293">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="f7a0d-294">オペランドの値が NaN、無限の変換の結果は、変換先の型の値が指定されていない場合。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-294">If the value of the operand is NaN or infinite, the result of the conversion is an unspecified value of the destination type.</span></span>
        * <span data-ttu-id="f7a0d-295">それ以外の場合、ソース オペランドは最も近い整数値を 0 方向に丸められます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-295">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="f7a0d-296">場合、この整数値を変換先の型の範囲内でこの値は変換の結果です。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-296">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="f7a0d-297">それ以外の場合、変換の結果は、先の型の値は指定されません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-297">Otherwise, the result of the conversion is an unspecified value of the destination type.</span></span>
*  <span data-ttu-id="f7a0d-298">変換の`double`に`float`、`double`値に丸められます、最も近い`float`値。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-298">For a conversion from `double` to `float`, the `double` value is rounded to the nearest `float` value.</span></span> <span data-ttu-id="f7a0d-299">場合、`double`として表す値が小さすぎる、`float`結果が正のゼロまたは負のゼロ。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-299">If the `double` value is too small to represent as a `float`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="f7a0d-300">場合、`double`として表す値が大きすぎて、`float`結果は正の無限大または負の無限大になります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-300">If the `double` value is too large to represent as a `float`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="f7a0d-301">場合、`double`値が NaN の結果は、NaN ではも場合、します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-301">If the `double` value is NaN, the result is also NaN.</span></span>
*  <span data-ttu-id="f7a0d-302">変換の`float`または`double`に`decimal`、ソース値に変換されます`decimal`表現し、必要な場合、28 小数点後近似値に丸められます ([10 進数型](types.md#the-decimal-type)).</span><span class="sxs-lookup"><span data-stu-id="f7a0d-302">For a conversion from `float` or `double` to `decimal`, the source value is converted to `decimal` representation and rounded to the nearest number after the 28th decimal place if required ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="f7a0d-303">ソース値が小さすぎてとして表すかどうか、`decimal`結果はゼロになります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-303">If the source value is too small to represent as a `decimal`, the result becomes zero.</span></span> <span data-ttu-id="f7a0d-304">場合、元の値が NaN の場合、無限大、または大きすぎてとして表す、 `decimal`、`System.OverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-304">If the source value is NaN, infinity, or too large to represent as a `decimal`, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="f7a0d-305">変換の`decimal`に`float`または`double`、`decimal`値に丸められます、最も近い`double`または`float`値。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-305">For a conversion from `decimal` to `float` or `double`, the `decimal` value is rounded to the nearest `double` or `float` value.</span></span> <span data-ttu-id="f7a0d-306">この変換は、有効桁数を失う可能性があります、中には、ことはありませんがスローされる例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-306">While this conversion may lose precision, it never causes an exception to be thrown.</span></span>

### <a name="explicit-enumeration-conversions"></a><span data-ttu-id="f7a0d-307">明示的な列挙値の変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-307">Explicit enumeration conversions</span></span>

<span data-ttu-id="f7a0d-308">明示的な列挙値の変換は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-308">The explicit enumeration conversions are:</span></span>

*  <span data-ttu-id="f7a0d-309">`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、または`decimal`任意に*enum_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-309">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal` to any *enum_type*.</span></span>
*  <span data-ttu-id="f7a0d-310">いずれかから*enum_type*に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、または`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-310">From any *enum_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="f7a0d-311">いずれかから*enum_type*他*enum_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-311">From any *enum_type* to any other *enum_type*.</span></span>

<span data-ttu-id="f7a0d-312">2 つの型の間の明示的な列挙型変換が任意参加を扱うことにより処理される*enum_type*を基になる型として*enum_type*、してから、暗黙的または明示的なを実行します。結果として得られる型間の数値変換します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-312">An explicit enumeration conversion between two types is processed by treating any participating *enum_type* as the underlying type of that *enum_type*, and then performing an implicit or explicit numeric conversion between the resulting types.</span></span> <span data-ttu-id="f7a0d-313">たとえば、 *enum_type* `E`での種類を基になると`int`からの変換`E`に`byte`明示的な数値変換として処理されます ([明示的な数値変換](conversions.md#explicit-numeric-conversions)) から`int`に`byte`からの変換と`byte`に`E`暗黙的な数値変換として処理されます ([暗黙的な数値変換](conversions.md#implicit-numeric-conversions))`byte`に`int`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-313">For example, given an *enum_type* `E` with and underlying type of `int`, a conversion from `E` to `byte` is processed as an explicit numeric conversion ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from `int` to `byte`, and a conversion from `byte` to `E` is processed as an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) from `byte` to `int`.</span></span>

### <a name="explicit-nullable-conversions"></a><span data-ttu-id="f7a0d-314">明示的な null 許容型の変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-314">Explicit nullable conversions</span></span>

<span data-ttu-id="f7a0d-315">***明示的な null 許容変換***定義済みの型の null 許容の形式でも使用する null 非許容値型を操作する明示的な変換を許可します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-315">***Explicit nullable conversions*** permit predefined explicit conversions that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="f7a0d-316">Null 非許容値型から変換する定義済みの明示的な変換の各`S`null 非許容値型に`T`([恒等変換](conversions.md#identity-conversion)、 [暗黙的な数値変換](conversions.md#implicit-numeric-conversions)、[列挙体の暗黙的な変換](conversions.md#implicit-enumeration-conversions)、[明示的な数値変換](conversions.md#explicit-numeric-conversions)、および[列挙値の明示的な変換](conversions.md#explicit-enumeration-conversions))、以下null 許容型の変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-316">For each of the predefined explicit conversions that convert from a non-nullable value type `S` to a non-nullable value type `T` ([Identity conversion](conversions.md#identity-conversion), [Implicit numeric conversions](conversions.md#implicit-numeric-conversions), [Implicit enumeration conversions](conversions.md#implicit-enumeration-conversions), [Explicit numeric conversions](conversions.md#explicit-numeric-conversions), and [Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)), the following nullable conversions exist:</span></span>

*  <span data-ttu-id="f7a0d-317">明示的な変換を`S?`に`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-317">An explicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="f7a0d-318">明示的な変換を`S`に`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-318">An explicit conversion from `S` to `T?`.</span></span>
*  <span data-ttu-id="f7a0d-319">明示的な変換を`S?`に`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-319">An explicit conversion from `S?` to `T`.</span></span>

<span data-ttu-id="f7a0d-320">Null 許容型の変換の評価がから基になる変換に基づく`S`に`T`次のように進みます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-320">Evaluation of a nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="f7a0d-321">場合は、null 許容型の変換から`S?`に`T?`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-321">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="f7a0d-322">元の値が null の場合 (`HasValue`プロパティが false)、結果は、型の null 値`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-322">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="f7a0d-323">ラップ解除とそれ以外の場合、変換の評価は`S?`に`S`からの変換を基になると、その後`S`に`T`、その後に、ラップをから`T`に`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-323">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="f7a0d-324">場合は、null 許容型の変換から`S`に`T?`から基になる変換と変換の評価は`S`に`T`からの折り返しを続けて`T`に`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-324">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="f7a0d-325">場合は、null 許容型の変換から`S?`に`T`からラップ解除すると、変換の評価は`S?`に`S`から基になる変換`S`に`T`。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-325">If the nullable conversion is from `S?` to `T`, the conversion is evaluated as an unwrapping from `S?` to `S` followed by the underlying conversion from `S` to `T`.</span></span>

<span data-ttu-id="f7a0d-326">値の場合、null 許容値のラップを解除しようとするは例外をスローことに注意してください。`null`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-326">Note that an attempt to unwrap a nullable value will throw an exception if the value is `null`.</span></span>

### <a name="explicit-reference-conversions"></a><span data-ttu-id="f7a0d-327">明示的な参照変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-327">Explicit reference conversions</span></span>

<span data-ttu-id="f7a0d-328">明示的な参照変換は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-328">The explicit reference conversions are:</span></span>

*  <span data-ttu-id="f7a0d-329">`object`と`dynamic`他*reference_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-329">From `object` and `dynamic` to any other *reference_type*.</span></span>
*  <span data-ttu-id="f7a0d-330">いずれかから*class_type* `S`いずれかに*class_type* `T`に用意されている`S`の基本クラスである`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-330">From any *class_type* `S` to any *class_type* `T`, provided `S` is a base class of `T`.</span></span>
*  <span data-ttu-id="f7a0d-331">いずれかから*class_type* `S`いずれかに*interface_type* `T`に用意されている`S`シールおよび提供されない`S`実装しない`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-331">From any *class_type* `S` to any *interface_type* `T`, provided `S` is not sealed and provided `S` does not implement `T`.</span></span>
*  <span data-ttu-id="f7a0d-332">いずれかから*interface_type* `S`いずれかに*class_type* `T`に用意されている`T`シールまたは指定されていない`T`実装`S`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-332">From any *interface_type* `S` to any *class_type* `T`, provided `T` is not sealed or provided `T` implements `S`.</span></span>
*  <span data-ttu-id="f7a0d-333">いずれかから*interface_type* `S`いずれかに*interface_type* `T`に用意されている`S`から派生していない`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-333">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is not derived from `T`.</span></span>
*  <span data-ttu-id="f7a0d-334">*Array_type* `S`要素型が`SE`を*array_type* `T`要素型が`TE`true は、次のすべての提供。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-334">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="f7a0d-335">`S` `T`要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-335">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="f7a0d-336">つまり、`S`と`T`同じ次元数を持ちます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-336">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="f7a0d-337">両方`SE`と`TE`は*reference_type*秒。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-337">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="f7a0d-338">明示的な参照変換が存在する`SE`に`TE`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-338">An explicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="f7a0d-339">`System.Array`いずれかに、インターフェイスを実装および*array_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-339">From `System.Array` and the interfaces it implements to any *array_type*.</span></span>
*  <span data-ttu-id="f7a0d-340">1 次元配列型から`S[]`に`System.Collections.Generic.IList<T>`とからの明示的な参照変換が提供される、基本インターフェイス`S`に`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-340">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an explicit reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="f7a0d-341">`System.Collections.Generic.IList<S>` 1 次元の配列型をその基本インターフェイスと`T[]`からの明示的な id または参照変換はその`S`に`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-341">From `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]`, provided that there is an explicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="f7a0d-342">`System.Delegate`いずれかに、インターフェイスを実装および*delegate_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-342">From `System.Delegate` and the interfaces it implements to any *delegate_type*.</span></span>
*  <span data-ttu-id="f7a0d-343">参照型への参照型から`T`参照型への明示的な参照変換があるかどうかは`T0`と`T0`が恒等変換`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-343">From a reference type to a reference type `T` if it has an explicit reference conversion to a reference type `T0` and `T0` has an identity conversion `T`.</span></span>
*  <span data-ttu-id="f7a0d-344">インターフェイスまたはデリゲートの型への参照型から`T`インターフェイスまたはデリゲート型への明示的な参照変換がある場合`T0`、`T0`差異に変換できるに`T`または`T`は変換できる分散`T0`([分散変換](interfaces.md#variance-conversion))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-344">From a reference type to an interface or delegate type `T` if it has an explicit reference conversion to an interface or delegate type `T0` and either `T0` is variance-convertible to `T` or `T` is variance-convertible to `T0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>
*  <span data-ttu-id="f7a0d-345">`D<S1...Sn>`に`D<T1...Tn>`場所`D<X1...Xn>`が汎用デリゲート型では、`D<S1...Sn>`と互換性があるかと同じでない`D<T1...Tn>`、およびそれぞれの型パラメーター`Xi`の`D`次の保留。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-345">From `D<S1...Sn>` to `D<T1...Tn>` where `D<X1...Xn>` is a generic delegate type, `D<S1...Sn>` is not compatible with or identical to `D<T1...Tn>`, and for each type parameter `Xi` of `D` the following holds:</span></span>
    * <span data-ttu-id="f7a0d-346">場合`Xi`しバリアントでは`Si`ヲェヒェケェ ・`Ti`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-346">If `Xi` is invariant, then `Si` is identical to `Ti`.</span></span>
    * <span data-ttu-id="f7a0d-347">場合`Xi`共変性を持つ、暗黙的または明示的な id または参照からの変換は`Si`に`Ti`。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-347">If `Xi` is covariant, then there is an implicit or explicit identity or reference conversion from `Si` to `Ti`.</span></span>
    * <span data-ttu-id="f7a0d-348">場合`Xi`は反変、`Si`と`Ti`は同一と両方の参照型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-348">If `Xi` is contravariant, then `Si` and `Ti` are either identical or both reference types.</span></span>
*  <span data-ttu-id="f7a0d-349">使用する明示的な変換では、参照型にすることがわかっているパラメーターを入力します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-349">Explicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="f7a0d-350">型パラメーターを使用する明示的な変換の詳細については、次を参照してください。[型パラメーターを使用する明示的な変換](conversions.md#explicit-conversions-involving-type-parameters)します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-350">For more details on explicit conversions involving type parameters, see [Explicit conversions involving type parameters](conversions.md#explicit-conversions-involving-type-parameters).</span></span>

<span data-ttu-id="f7a0d-351">明示的な参照変換は、実行時のチェックが正しいことを確認を必要とする参照型の間で変換のことです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-351">The explicit reference conversions are those conversions between reference-types that require run-time checks to ensure they are correct.</span></span>

<span data-ttu-id="f7a0d-352">明示的な参照変換を実行時に正常にするには、ソース オペランドの値がある必要があります`null`、またはソース オペランドによって参照されるオブジェクトの実際の型の暗黙的な参照によって変換先の型に変換できる型でなければなりません変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions)) またはボックス化変換 ([ボックス化変換](conversions.md#boxing-conversions))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-352">For an explicit reference conversion to succeed at run-time, the value of the source operand must be `null`, or the actual type of the object referenced by the source operand must be a type that can be converted to the destination type by an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)).</span></span> <span data-ttu-id="f7a0d-353">明示的な参照変換に失敗した場合、`System.InvalidCastException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-353">If an explicit reference conversion fails, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="f7a0d-354">参照変換、暗黙的または明示的には、変換されるオブジェクトの参照 id を変更することはありません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-354">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="f7a0d-355">つまり、参照変換は、参照の種類を変更することが、変更しません、型または参照されるオブジェクトの値。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-355">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="f7a0d-356">ボックス化解除変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-356">Unboxing conversions</span></span>

<span data-ttu-id="f7a0d-357">ボックス化解除の変換では参照型に明示的に変換する、 *value_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-357">An unboxing conversion permits a reference type to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="f7a0d-358">型からボックス化解除の変換が存在する`object`、`dynamic`と`System.ValueType`いずれかに*non_nullable_value_type*、いずれかから*interface_type*いずれかに*non_nullable_value_type*を実装する、 *interface_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-358">An unboxing conversion exists from the types `object`, `dynamic` and `System.ValueType` to any *non_nullable_value_type*, and from any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span> <span data-ttu-id="f7a0d-359">さらに入力`System.Enum`いずれかにボックス化解除できます*enum_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-359">Furthermore type `System.Enum` can be unboxed to any *enum_type*.</span></span>

<span data-ttu-id="f7a0d-360">参照型をボックス化解除の変換が存在する、 *nullable_type* 、基になる参照型のアンボックス変換が存在する場合は*non_nullable_value_type*の*nullable_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-360">An unboxing conversion exists from a reference type to a *nullable_type* if an unboxing conversion exists from the reference type to the underlying *non_nullable_value_type* of the *nullable_type*.</span></span>

<span data-ttu-id="f7a0d-361">値型`S`がインターフェイス型型からをボックス化解除変換`I`インターフェイス型型からのボックス化解除の変換があるかどうかは`I0`と`I0`が恒等変換を`I`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-361">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="f7a0d-362">値型`S`がインターフェイス型型からをボックス化解除変換`I`インターフェイスまたはデリゲートの型からボックス化解除の変換がある場合`I0`、`I0`差異に変換できるに`I`または`I`差異に変換できるに`I0`([分散変換](interfaces.md#variance-conversion))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-362">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface or delegate type `I0` and either `I0` is variance-convertible to `I` or `I` is variance-convertible to `I0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>

<span data-ttu-id="f7a0d-363">ボックス化解除の操作は、オブジェクトのインスタンスのボックス化された値は、最初に確認の指定された*value_type*、し、そのインスタンスから値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-363">An unboxing operation consists of first checking that the object instance is a boxed value of the given *value_type*, and then copying the value out of the instance.</span></span> <span data-ttu-id="f7a0d-364">Null 参照がボックス化解除、 *nullable_type*の null 値を生成、 *nullable_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-364">Unboxing a null reference to a *nullable_type* produces the null value of the *nullable_type*.</span></span> <span data-ttu-id="f7a0d-365">構造体の型からボックス化が解除できます`System.ValueType`すべての構造体の基本クラスでは、([継承](structs.md#inheritance))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-365">A struct can be unboxed from the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="f7a0d-366">ボックス化解除変換の詳細については[ボックス化解除変換](types.md#unboxing-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-366">Unboxing conversions are described further in [Unboxing conversions](types.md#unboxing-conversions).</span></span>

### <a name="explicit-dynamic-conversions"></a><span data-ttu-id="f7a0d-367">明示的な動的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-367">Explicit dynamic conversions</span></span>

<span data-ttu-id="f7a0d-368">型の式から動的の明示的な変換が存在する`dynamic`任意の型を`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-368">An explicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="f7a0d-369">変換が動的にバインドされている ([動的バインド](expressions.md#dynamic-binding))、つまり、実行時に式の実行時の型からの明示的な変換が求められますこと`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-369">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an explicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="f7a0d-370">変換が見つからない場合は、実行時に例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-370">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="f7a0d-371">変換の動的バインドが望ましくない場合、式最初に変換できる`object`とし、目的の型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-371">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="f7a0d-372">次のクラスが定義されていると仮定します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-372">Assume the following class is defined:</span></span>
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

<span data-ttu-id="f7a0d-373">次の例は、明示的な動的な変換を示しています。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-373">The following example illustrates explicit dynamic conversions:</span></span>
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

<span data-ttu-id="f7a0d-374">最適な変換`o`に`C`が記載された明示的な参照変換をコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-374">The best conversion of `o` to `C` is found at compile-time to be an explicit reference conversion.</span></span> <span data-ttu-id="f7a0d-375">に、実行時に、これが失敗した`"1"`ファクトでは、`C`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-375">This fails at run-time, because `"1"` is not in fact a `C`.</span></span> <span data-ttu-id="f7a0d-376">変換`d`に`C`実行時、ユーザーがの実行時の型からの変換を定義するが、明示的な動的変換としてがただし、中断されて`d`  --  `string` --に`C`が見つかると、成功したとします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-376">The conversion of `d` to `C` however, as an explicit dynamic conversion, is suspended to run-time, where a user defined conversion from the run-time type of `d` -- `string` -- to `C` is found, and succeeds.</span></span>

### <a name="explicit-conversions-involving-type-parameters"></a><span data-ttu-id="f7a0d-377">型パラメーターを使用する明示的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-377">Explicit conversions involving type parameters</span></span>

<span data-ttu-id="f7a0d-378">次の明示的な変換が指定された型パラメーターの存在`T`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-378">The following explicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="f7a0d-379">有効な基本クラスから`C`の`T`に`T`との任意の基本クラスから`C`に`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-379">From the effective base class `C` of `T` to `T` and from any base class of `C` to `T`.</span></span> <span data-ttu-id="f7a0d-380">実行時に、if`T`値型である、変換、アンボックス変換として実行します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-380">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="f7a0d-381">それ以外の場合、変換は、明示的な参照変換または id 変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-381">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="f7a0d-382">任意のインターフェイス型から`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-382">From any interface type to `T`.</span></span> <span data-ttu-id="f7a0d-383">実行時に、if`T`値型である、変換、アンボックス変換として実行します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-383">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="f7a0d-384">それ以外の場合、変換は、明示的な参照変換または id 変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-384">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="f7a0d-385">`T`いずれかに*interface_type* `I`既にからの暗黙的な変換がない`T`に`I`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-385">From `T` to any *interface_type* `I` provided there is not already an implicit conversion from `T` to `I`.</span></span> <span data-ttu-id="f7a0d-386">実行時に、if`T`値型である明示的な参照変換の後にボックス化変換と変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-386">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion followed by an explicit reference conversion.</span></span> <span data-ttu-id="f7a0d-387">それ以外の場合、変換は、明示的な参照変換または id 変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-387">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="f7a0d-388">型パラメーターから`U`に`T`に用意されている`T`異なります`U`([パラメーターの制約入力](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-388">From a type parameter `U` to `T`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="f7a0d-389">実行時に、if`U`値の型には`T`と`U`必ずしも同じ型では、変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-389">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="f7a0d-390">の場合`T`値型である、変換、アンボックス変換として実行します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-390">Otherwise, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="f7a0d-391">それ以外の場合、変換は、明示的な参照変換または id 変換として実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-391">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="f7a0d-392">場合`T`はわかっていますが、参照型である、上記の変換はすべてとして分類明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-392">If `T` is known to be a reference type, the conversions above are all classified as explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="f7a0d-393">場合`T`は上記の変換は参照型であるとわかっていない、ボックス化解除変換に分類されます ([ボックス化解除変換](conversions.md#unboxing-conversions))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-393">If `T` is not known to be a reference type, the conversions above are classified as unboxing conversions ([Unboxing conversions](conversions.md#unboxing-conversions)).</span></span>

<span data-ttu-id="f7a0d-394">上記の規則は、制約のない型パラメーターから、インターフェイスではない型への直接明示的な変換許可されていません驚くべきことが必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-394">The above rules do not permit a direct explicit conversion from an unconstrained type parameter to a non-interface type, which might be surprising.</span></span> <span data-ttu-id="f7a0d-395">このルールの理由は、このような変換をオフのセマンティクスを混乱を避けるためです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-395">The reason for this rule is to prevent confusion and make the semantics of such conversions clear.</span></span> <span data-ttu-id="f7a0d-396">たとえば、次のような宣言があるとします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-396">For example, consider the following declaration:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

<span data-ttu-id="f7a0d-397">場合の直接の明示的な変換`t`に`int`が許可されているいずれかが簡単に期待する`X<int>.F(7)`を返します `7L`。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-397">If the direct explicit conversion of `t` to `int` were permitted, one might easily expect that `X<int>.F(7)` would return `7L`.</span></span> <span data-ttu-id="f7a0d-398">ただし、設定されませんでしたが、標準の数値の変換は、バインド時に数値型がわかっている場合にのみと見なされるためです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-398">However, it would not, because the standard numeric conversions are only considered when the types are known to be numeric at binding-time.</span></span> <span data-ttu-id="f7a0d-399">セマンティクスを作成するには明確で上記の例では、代わりに記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-399">In order to make the semantics clear, the above example must instead be written:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

<span data-ttu-id="f7a0d-400">このコードはコンパイルされますが、実行時`X<int>.F(7)`ボックス化された後の実行時に、例外をスローしは`int`に直接変換することはできません、`long`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-400">This code will now compile but executing `X<int>.F(7)` would then throw an exception at run-time, since a boxed `int` cannot be converted directly to a `long`.</span></span>

### <a name="user-defined-explicit-conversions"></a><span data-ttu-id="f7a0d-401">ユーザー定義の明示的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-401">User-defined explicit conversions</span></span>

<span data-ttu-id="f7a0d-402">ユーザー定義の明示的な変換は、省略可能な標準的な明示的な変換後に省略可能な別の標準的な明示的な変換の後に、ユーザー定義の暗黙的または明示的な変換演算子の実行で構成されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-402">A user-defined explicit conversion consists of an optional standard explicit conversion, followed by execution of a user-defined implicit or explicit conversion operator, followed by another optional standard explicit conversion.</span></span> <span data-ttu-id="f7a0d-403">ユーザー定義の明示的な変換を評価するための正確な規則については、「[明示的な変換のユーザー定義の処理](conversions.md#processing-of-user-defined-explicit-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-403">The exact rules for evaluating user-defined explicit conversions are described in [Processing of user-defined explicit conversions](conversions.md#processing-of-user-defined-explicit-conversions).</span></span>

## <a name="standard-conversions"></a><span data-ttu-id="f7a0d-404">標準変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-404">Standard conversions</span></span>

<span data-ttu-id="f7a0d-405">標準の変換は、ユーザー定義の変換の一部として発生する変換の定義済みです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-405">The standard conversions are those pre-defined conversions that can occur as part of a user-defined conversion.</span></span>

### <a name="standard-implicit-conversions"></a><span data-ttu-id="f7a0d-406">標準の暗黙的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-406">Standard implicit conversions</span></span>

<span data-ttu-id="f7a0d-407">次の暗黙的な変換は、標準の暗黙的な変換として分類されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-407">The following implicit conversions are classified as standard implicit conversions:</span></span>

*  <span data-ttu-id="f7a0d-408">恒等変換 ([恒等変換](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="f7a0d-408">Identity conversions ([Identity conversion](conversions.md#identity-conversion))</span></span>
*  <span data-ttu-id="f7a0d-409">暗黙的な数値変換 ([暗黙的な数値変換](conversions.md#implicit-numeric-conversions))</span><span class="sxs-lookup"><span data-stu-id="f7a0d-409">Implicit numeric conversions ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions))</span></span>
*  <span data-ttu-id="f7a0d-410">Null 許容型の暗黙的な変換 ([null 許容型の暗黙的な変換](conversions.md#implicit-nullable-conversions))</span><span class="sxs-lookup"><span data-stu-id="f7a0d-410">Implicit nullable conversions ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions))</span></span>
*  <span data-ttu-id="f7a0d-411">暗黙的な参照変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="f7a0d-411">Implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
*  <span data-ttu-id="f7a0d-412">ボックス化変換 ([ボックス化変換](conversions.md#boxing-conversions))</span><span class="sxs-lookup"><span data-stu-id="f7a0d-412">Boxing conversions ([Boxing conversions](conversions.md#boxing-conversions))</span></span>
*  <span data-ttu-id="f7a0d-413">定数式が暗黙的な変換 ([動的の暗黙的な変換](conversions.md#implicit-dynamic-conversions))</span><span class="sxs-lookup"><span data-stu-id="f7a0d-413">Implicit constant expression conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions))</span></span>
*  <span data-ttu-id="f7a0d-414">型パラメーターを伴う暗黙の変換 ([型パラメーターを使用する暗黙的な変換](conversions.md#implicit-conversions-involving-type-parameters))</span><span class="sxs-lookup"><span data-stu-id="f7a0d-414">Implicit conversions involving type parameters ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters))</span></span>

<span data-ttu-id="f7a0d-415">具体的には、標準の暗黙的な変換は、ユーザー定義の暗黙的な変換を除外します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-415">The standard implicit conversions specifically exclude user-defined implicit conversions.</span></span>

### <a name="standard-explicit-conversions"></a><span data-ttu-id="f7a0d-416">標準の明示的な変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-416">Standard explicit conversions</span></span>

<span data-ttu-id="f7a0d-417">標準の明示的な変換は、すべての標準的な暗黙的な変換と反対の標準的な暗黙的な変換が存在する明示的な変換のサブセットです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-417">The standard explicit conversions are all standard implicit conversions plus the subset of the explicit conversions for which an opposite standard implicit conversion exists.</span></span> <span data-ttu-id="f7a0d-418">つまり、標準の暗黙的な変換が存在する場合、型から`A`型に`B`、型から標準の明示的な変換が存在し、`A`入力`B`型との間`B`入力`A`。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-418">In other words, if a standard implicit conversion exists from a type `A` to a type `B`, then a standard explicit conversion exists from type `A` to type `B` and from type `B` to type `A`.</span></span>

## <a name="user-defined-conversions"></a><span data-ttu-id="f7a0d-419">ユーザー定義の変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-419">User-defined conversions</span></span>

<span data-ttu-id="f7a0d-420">C# では、事前に定義された明示的および暗黙的な変換を拡張して***ユーザー定義の変換***します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-420">C# allows the pre-defined implicit and explicit conversions to be augmented by ***user-defined conversions***.</span></span> <span data-ttu-id="f7a0d-421">ユーザー定義の変換が変換演算子を宣言することで導入されました ([変換演算子](classes.md#conversion-operators)) クラスと構造体の型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-421">User-defined conversions are introduced by declaring conversion operators ([Conversion operators](classes.md#conversion-operators)) in class and struct types.</span></span>

### <a name="permitted-user-defined-conversions"></a><span data-ttu-id="f7a0d-422">ユーザー定義の変換を許可</span><span class="sxs-lookup"><span data-stu-id="f7a0d-422">Permitted user-defined conversions</span></span>

<span data-ttu-id="f7a0d-423">C# でのみ特定のユーザー定義変換を宣言します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-423">C# permits only certain user-defined conversions to be declared.</span></span> <span data-ttu-id="f7a0d-424">具体的には、既存の暗黙的または明示的な変換を再定義することはできません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-424">In particular, it is not possible to redefine an already existing implicit or explicit conversion.</span></span>

<span data-ttu-id="f7a0d-425">指定したソースの種類`S`ターゲットの種類と`T`場合は、`S`または`T`は null 許容型は、ように`S0`と`T0`それ以外の場合、基になる型を参照してください`S0`と`T0`は等しい`S`と`T`それぞれします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-425">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="f7a0d-426">ソースの種類からの変換を宣言するクラスまたは構造体が許可されている`S`をターゲットの型`T`次のすべてに当てはまる場合にのみ。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-426">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="f7a0d-427">`S0` `T0`はさまざまな種類。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-427">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="f7a0d-428">いずれか`S0`または`T0`演算子宣言が行われるクラスまたは構造体の型です。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-428">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="f7a0d-429">どちらも`S0`も`T0`は、 *interface_type*します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-429">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="f7a0d-430">ユーザー定義の変換を除く、変換が存在しないから`S`に`T`またはから`T`に`S`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-430">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="f7a0d-431">ユーザー定義の変換に適用される制限については、説明でさらに[変換演算子](classes.md#conversion-operators)します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-431">The restrictions that apply to user-defined conversions are discussed further in [Conversion operators](classes.md#conversion-operators).</span></span>

### <a name="lifted-conversion-operators"></a><span data-ttu-id="f7a0d-432">リフト変換演算子</span><span class="sxs-lookup"><span data-stu-id="f7a0d-432">Lifted conversion operators</span></span>

<span data-ttu-id="f7a0d-433">Null 非許容値型に変換するユーザー定義の変換演算子を指定された`S`null 非許容値型に`T`、***変換演算子のリフト***から変換が存在する`S?`に`T?`.</span><span class="sxs-lookup"><span data-stu-id="f7a0d-433">Given a user-defined conversion operator that converts from a non-nullable value type `S` to a non-nullable value type `T`, a ***lifted conversion operator*** exists that converts from `S?` to `T?`.</span></span> <span data-ttu-id="f7a0d-434">ラップ解除、このリフト変換演算子を実行します`S?`に`S`からユーザー定義の変換後に`S`に`T`からの折り返しを続けて`T`に`T?`点を除き、null値を持つ`S?`値は null に直接変換`T?`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-434">This lifted conversion operator performs an unwrapping from `S?` to `S` followed by the user-defined conversion from `S` to `T` followed by a wrapping from `T` to `T?`, except that a null valued `S?` converts directly to a null valued `T?`.</span></span>

<span data-ttu-id="f7a0d-435">リフト変換演算子が、その基になるユーザー定義の変換演算子として同じ暗黙的または明示的な分類します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-435">A lifted conversion operator has the same implicit or explicit classification as its underlying user-defined conversion operator.</span></span> <span data-ttu-id="f7a0d-436">「ユーザー定義の変換」は、両方の使用に適用期間は、ユーザー定義し、リフト変換演算子。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-436">The term "user-defined conversion" applies to the use of both user-defined and lifted conversion operators.</span></span>

### <a name="evaluation-of-user-defined-conversions"></a><span data-ttu-id="f7a0d-437">ユーザー定義の変換の評価</span><span class="sxs-lookup"><span data-stu-id="f7a0d-437">Evaluation of user-defined conversions</span></span>

<span data-ttu-id="f7a0d-438">ユーザー定義の変換と呼ばれる、その型からの値の変換、***ソースの種類***、という名前の別の型に、***ターゲットの種類***します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-438">A user-defined conversion converts a value from its type, called the ***source type***, to another type, called the ***target type***.</span></span> <span data-ttu-id="f7a0d-439">ユーザー定義の変換の評価を確認する方法に中央揃え、***最も具体的***特定のソースとターゲットの種類のユーザー定義変換演算子。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-439">Evaluation of a user-defined conversion centers on finding the ***most specific*** user-defined conversion operator for the particular source and target types.</span></span> <span data-ttu-id="f7a0d-440">この決定は、いくつかの手順に分かれています。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-440">This determination is broken into several steps:</span></span>

*  <span data-ttu-id="f7a0d-441">クラスと元のユーザー定義の変換演算子を考慮する構造体のセットを検索します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-441">Finding the set of classes and structs from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="f7a0d-442">このセットは、ソースの種類とその基本クラスとターゲットの種類 (クラスと構造体のみがユーザー定義の演算子を宣言でき、クラス以外の型が基底クラスを含むなしの暗黙の想定) をその基本クラスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-442">This set consists of the source type and its base classes and the target type and its base classes (with the implicit assumptions that only classes and structs can declare user-defined operators, and that non-class types have no base classes).</span></span> <span data-ttu-id="f7a0d-443">ソースまたはターゲットのいずれかの型がある場合、この手順の目的で、 *nullable_type*、その基になる型は代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-443">For the purposes of this step, if either the source or target type is a *nullable_type*, their underlying type is used instead.</span></span>
*  <span data-ttu-id="f7a0d-444">型が設定されるユーザー定義して、リフト変換演算子の決定に適用されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-444">From that set of types, determining which user-defined and lifted conversion operators are applicable.</span></span> <span data-ttu-id="f7a0d-445">変換演算子を適用する場合にの標準変換を実行する場合があります ([標準変換](conversions.md#standard-conversions)) オペランドのソースの種類からは、演算子の種類で標準変換を実行できる必要があります対象の型に演算子の結果の型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-445">For a conversion operator to be applicable, it must be possible to perform a standard conversion ([Standard conversions](conversions.md#standard-conversions)) from the source type to the operand type of the operator, and it must be possible to perform a standard conversion from the result type of the operator to the target type.</span></span>
*  <span data-ttu-id="f7a0d-446">該当するユーザー定義演算子のセットからには、演算子が明確に最も固有を決定します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-446">From the set of applicable user-defined operators, determining which operator is unambiguously the most specific.</span></span> <span data-ttu-id="f7a0d-447">一般的な用語では、最も固有の演算子はオペランドの型がソースの種類に「最も近い」で、結果の型は、ターゲット型に「最も近い」演算子が。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-447">In general terms, the most specific operator is the operator whose operand type is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="f7a0d-448">ユーザー定義変換演算子は、リフト変換演算子より優先されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-448">User-defined conversion operators are preferred over lifted conversion operators.</span></span> <span data-ttu-id="f7a0d-449">最も固有のユーザー定義変換演算子を確立するための正確な規則は、次のセクションで定義されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-449">The exact rules for establishing the most specific user-defined conversion operator are defined in the following sections.</span></span>

<span data-ttu-id="f7a0d-450">特定のユーザー定義の変換演算子が識別されると、ユーザー定義の変換の実際の実行に最大 3 つの手順が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-450">Once a most specific user-defined conversion operator has been identified, the actual execution of the user-defined conversion involves up to three steps:</span></span>

*  <span data-ttu-id="f7a0d-451">最初に、必要な場合は、ソースの種類から、またはユーザー定義の変換の変換演算子のオペランドの型への標準変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-451">First, if required, performing a standard conversion from the source type to the operand type of the user-defined or lifted conversion operator.</span></span>
*  <span data-ttu-id="f7a0d-452">次に、変換を実行するユーザー定義またはリフトの変換演算子を呼び出しています。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-452">Next, invoking the user-defined or lifted conversion operator to perform the conversion.</span></span>
*  <span data-ttu-id="f7a0d-453">最後に、必要な場合、またはユーザー定義の変換の変換演算子の結果の型から対象の型への標準変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-453">Finally, if required, performing a standard conversion from the result type of the user-defined or lifted conversion operator to the target type.</span></span>

<span data-ttu-id="f7a0d-454">ユーザー定義変換しないの評価には、1 つ以上のユーザー定義またはリフトの変換演算子が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-454">Evaluation of a user-defined conversion never involves more than one user-defined or lifted conversion operator.</span></span> <span data-ttu-id="f7a0d-455">つまり、型から変換`S`を入力する`T`からユーザー定義の変換は実行しない最初`S`に`X`からユーザー定義の変換を実行し、`X`に`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-455">In other words, a conversion from type `S` to type `T` will never first execute a user-defined conversion from `S` to `X` and then execute a user-defined conversion from `X` to `T`.</span></span>

<span data-ttu-id="f7a0d-456">ユーザー定義の暗黙的または明示的な変換の評価の正確な定義は、次のセクションで提供されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-456">Exact definitions of evaluation of user-defined implicit or explicit conversions are given in the following sections.</span></span> <span data-ttu-id="f7a0d-457">定義の次の用語を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-457">The definitions make use of the following terms:</span></span>

*  <span data-ttu-id="f7a0d-458">場合、標準の暗黙的な変換 ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) 型からが存在する`A`型`B`、どちらの場合と`A`も`B`は*interface_type*s し`A`と呼ばれます***に包含***`B`と`B`といいます***encompass*** `A`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-458">If a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) exists from a type `A` to a type `B`, and if neither `A` nor `B` are *interface_type*s, then `A` is said to be ***encompassed by*** `B`, and `B` is said to ***encompass*** `A`.</span></span>
*  <span data-ttu-id="f7a0d-459">***最も外側の型***一連の型では、セット内の他のすべての型を包含している 1 つの型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-459">The ***most encompassing type*** in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="f7a0d-460">その他のすべての型を包含する 1 つの型の場合セットに最も外側の型がありません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-460">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="f7a0d-461">最も外側の型は、セットの「最大」の型をより直感的な用語で、先の他の種類ごとに暗黙的に変換できる 1 つの型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-461">In more intuitive terms, the most encompassing type is the "largest" type in the set—the one type to which each of the other types can be implicitly converted.</span></span>
*  <span data-ttu-id="f7a0d-462">***最も内側の型***一連の型では、セット内の他のすべての種類に包含されている 1 つの型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-462">The ***most encompassed type*** in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="f7a0d-463">他のすべての型によって 1 つの型が含まれていない場合、セットが最も内側の型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-463">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="f7a0d-464">直感的な用語は、最も内側の型は、セット内の「最小」の型などの他の種類ごとに暗黙的に変換できる 1 つの型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-464">In more intuitive terms, the most encompassed type is the "smallest" type in the set—the one type that can be implicitly converted to each of the other types.</span></span>

### <a name="processing-of-user-defined-implicit-conversions"></a><span data-ttu-id="f7a0d-465">暗黙的な変換のユーザー定義の処理</span><span class="sxs-lookup"><span data-stu-id="f7a0d-465">Processing of user-defined implicit conversions</span></span>

<span data-ttu-id="f7a0d-466">ユーザー定義型から暗黙的な変換`S`を入力する`T`ように処理されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-466">A user-defined implicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="f7a0d-467">種類を特定する`S0`と`T0`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-467">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="f7a0d-468">場合`S`または`T`は null 許容型は、`S0`と`T0`の基になる型をそれ以外の場合は`S0`と`T0`と等しい`S`と`T`それぞれします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-468">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="f7a0d-469">型のセットを見つける`D`、どのユーザー定義の変換演算子を考慮します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-469">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="f7a0d-470">このセットから成る`S0`(場合`S0`がクラスまたは構造体) の基本クラス`S0`(場合`S0`クラスは、)、および`T0`(場合`T0`がクラスまたは構造体)。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-470">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), and `T0` (if `T0` is a class or struct).</span></span>
*  <span data-ttu-id="f7a0d-471">該当するユーザー定義およびリフト変換演算子のセットを見つける`U`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-471">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="f7a0d-472">このセットは、クラスまたは構造体で宣言されているユーザー定義およびリフトの暗黙的な変換演算子で構成されます`D`包括的な型から変換する`S`型によって包含`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-472">This set consists of the user-defined and lifted implicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing `S` to a type encompassed by `T`.</span></span> <span data-ttu-id="f7a0d-473">場合`U`は空の場合、変換が定義されていると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-473">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="f7a0d-474">特定のソースの種類を検索`SX`、演算子の`U`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-474">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="f7a0d-475">いずれかの演算子の`U`から変換`S`、し`SX`は`S`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-475">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="f7a0d-476">それ以外の場合、`SX`の演算子のソースの種類の結合されたセット内の最も内側の型は、`U`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-476">Otherwise, `SX` is the most encompassed type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="f7a0d-477">型が見つからない最も内側の 1 つだけの場合に、変換があいまいなをコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-477">If exactly one most encompassed type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="f7a0d-478">最も固有のターゲット型を見つける`TX`、演算子の`U`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-478">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="f7a0d-479">いずれかの演算子の場合`U`変換`T`、し`TX`は`T`。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-479">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="f7a0d-480">それ以外の場合、`TX`で一連の演算子のターゲット タイプの最も外側の型は、`U`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-480">Otherwise, `TX` is the most encompassing type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="f7a0d-481">正確に 1 つの最も外側の型が見つからない場合、変換はあいまいですしとコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-481">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="f7a0d-482">最も固有の変換演算子を検索します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-482">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="f7a0d-483">場合`U`から変換する 1 つだけのユーザー定義変換演算子が含まれています`SX`に`TX`、これは、最も固有の変換演算子。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-483">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="f7a0d-484">の場合`U`から変換する 1 つだけのリフト変換演算子が含まれています`SX`に`TX`、これは、最も固有の変換演算子。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-484">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="f7a0d-485">それ以外の場合、変換はあいまいですし、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-485">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="f7a0d-486">最後に、変換が適用されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-486">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="f7a0d-487">場合`S`ない`SX`から、標準、暗黙的な変換し、`S`に`SX`が実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-487">If `S` is not `SX`, then a standard implicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="f7a0d-488">変換する最も固有の変換演算子が呼び出される`SX`に`TX`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-488">The most specific conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="f7a0d-489">場合`TX`ない`T`から、標準、暗黙的な変換し、`TX`に`T`が実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-489">If `TX` is not `T`, then a standard implicit conversion from `TX` to `T` is performed.</span></span>

### <a name="processing-of-user-defined-explicit-conversions"></a><span data-ttu-id="f7a0d-490">明示的な変換のユーザー定義の処理</span><span class="sxs-lookup"><span data-stu-id="f7a0d-490">Processing of user-defined explicit conversions</span></span>

<span data-ttu-id="f7a0d-491">型からユーザー定義の明示的な変換`S`入力`T`ように処理されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-491">A user-defined explicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="f7a0d-492">種類を特定する`S0`と`T0`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-492">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="f7a0d-493">場合`S`または`T`は null 許容型は、`S0`と`T0`の基になる型をそれ以外の場合は`S0`と`T0`と等しい`S`と`T`それぞれします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-493">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="f7a0d-494">型のセットを見つける`D`、どのユーザー定義の変換演算子を考慮します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-494">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="f7a0d-495">このセットから成る`S0`(場合`S0`がクラスまたは構造体) の基本クラス`S0`(場合`S0`クラスは、)、 `T0` (場合`T0`がクラスまたは構造体)、およびの基本クラス`T0`(場合`T0`クラスです)。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-495">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), `T0` (if `T0` is a class or struct), and the base classes of `T0` (if `T0` is a class).</span></span>
*  <span data-ttu-id="f7a0d-496">該当するユーザー定義およびリフト変換演算子のセットを見つける`U`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-496">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="f7a0d-497">このセットは、ユーザー定義およびリフト暗黙的または明示的な変換演算子は、クラスまたは構造体で宣言されている`D`を包括的な型から変換またはによって包含`S`を包括的なまたはに包含型`T`.</span><span class="sxs-lookup"><span data-stu-id="f7a0d-497">This set consists of the user-defined and lifted implicit or explicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing or encompassed by `S` to a type encompassing or encompassed by `T`.</span></span> <span data-ttu-id="f7a0d-498">場合`U`は空の場合、変換が定義されていると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-498">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="f7a0d-499">特定のソースの種類を検索`SX`、演算子の`U`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-499">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="f7a0d-500">いずれかの演算子の`U`から変換`S`、し`SX`は`S`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-500">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="f7a0d-501">それ以外の場合、いずれかの演算子の`U`包含する型から変換`S`、し`SX`これらの演算子のソースの種類の結合されたセット内の最も内側にある型です。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-501">Otherwise, if any of the operators in `U` convert from types that encompass `S`, then `SX` is the most encompassed type in the combined set of source types of those operators.</span></span> <span data-ttu-id="f7a0d-502">型が見つからない、最も内側の場合に、変換があいまいなをコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-502">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="f7a0d-503">それ以外の場合、`SX`の演算子のソースの種類の結合されたセット内の最も外側の型は、`U`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-503">Otherwise, `SX` is the most encompassing type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="f7a0d-504">正確に 1 つの最も外側の型が見つからない場合、変換はあいまいですしとコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-504">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="f7a0d-505">最も固有のターゲット型を見つける`TX`、演算子の`U`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-505">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="f7a0d-506">いずれかの演算子の場合`U`変換`T`、し`TX`は`T`。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-506">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="f7a0d-507">それ以外の場合、いずれかの演算子の`U`によって包含する型に変換`T`、し`TX`はこれらの演算子のターゲット型の結合されたセット内の最も外側の型。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-507">Otherwise, if any of the operators in `U` convert to types that are encompassed by `T`, then `TX` is the most encompassing type in the combined set of target types of those operators.</span></span> <span data-ttu-id="f7a0d-508">正確に 1 つの最も外側の型が見つからない場合、変換はあいまいですしとコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-508">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="f7a0d-509">それ以外の場合、`TX`で一連の演算子のターゲット タイプの最も内側の型は、`U`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-509">Otherwise, `TX` is the most encompassed type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="f7a0d-510">型が見つからない、最も内側の場合に、変換があいまいなをコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-510">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="f7a0d-511">最も固有の変換演算子を検索します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-511">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="f7a0d-512">場合`U`から変換する 1 つだけのユーザー定義変換演算子が含まれています`SX`に`TX`、これは、最も固有の変換演算子。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-512">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="f7a0d-513">の場合`U`から変換する 1 つだけのリフト変換演算子が含まれています`SX`に`TX`、これは、最も固有の変換演算子。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-513">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="f7a0d-514">それ以外の場合、変換はあいまいですし、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-514">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="f7a0d-515">最後に、変換が適用されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-515">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="f7a0d-516">場合`S`ない`SX`から標準の明示的な変換、`S`に`SX`が実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-516">If `S` is not `SX`, then a standard explicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="f7a0d-517">変換する最も固有のユーザー定義変換演算子が呼び出される`SX`に`TX`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-517">The most specific user-defined conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="f7a0d-518">場合`TX`ない`T`から標準の明示的な変換、`TX`に`T`が実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-518">If `TX` is not `T`, then a standard explicit conversion from `TX` to `T` is performed.</span></span>

## <a name="anonymous-function-conversions"></a><span data-ttu-id="f7a0d-519">匿名関数の変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-519">Anonymous function conversions</span></span>

<span data-ttu-id="f7a0d-520">*Anonymous_method_expression*または*lambda_expression*匿名関数として分類されます ([匿名関数式](expressions.md#anonymous-function-expressions))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-520">An *anonymous_method_expression* or *lambda_expression* is classified as an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="f7a0d-521">式は、型がありませんが、互換性のあるデリゲート型または式ツリー型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-521">The expression does not have a type but can be implicitly converted to a compatible delegate type or expression tree type.</span></span> <span data-ttu-id="f7a0d-522">具体的には、匿名関数`F`デリゲート型と互換性が`D`提供します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-522">Specifically, an anonymous function `F` is compatible with a delegate type `D` provided:</span></span>

*  <span data-ttu-id="f7a0d-523">場合`F`が含まれています、 *anonymous_function_signature*、し`D`と`F`同じ数のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-523">If `F` contains an *anonymous_function_signature*, then `D` and `F` have the same number of parameters.</span></span>
*  <span data-ttu-id="f7a0d-524">場合`F`が含まれていない、 *anonymous_function_signature*、し`D`のパラメーターなしに限り、あらゆる種類の 0 個以上のパラメーターがあります。`D`が、`out`パラメーター修飾子。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-524">If `F` does not contain an *anonymous_function_signature*, then `D` may have zero or more parameters of any type, as long as no parameter of `D` has the `out` parameter modifier.</span></span>
*  <span data-ttu-id="f7a0d-525">場合`F`、明示的に型指定されたパラメーターのリスト内の各パラメーターを持つ`D`の対応するパラメーターとして、同じ型および修飾子を持つ`F`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-525">If `F` has an explicitly typed parameter list, each parameter in `D` has the same type and modifiers as the corresponding parameter in `F`.</span></span>
*  <span data-ttu-id="f7a0d-526">場合`F`、暗黙的に型指定されたパラメーター リストを持つ`D`にない`ref`または`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-526">If `F` has an implicitly typed parameter list, `D` has no `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="f7a0d-527">場合の本文`F`が、式で、かつ`D`が、`void`型を返すまたは`F`は async と`D`戻り値の型を持つ`Task`、その後の各パラメーター`F`の型を指定、対応するパラメーターの`D`の本文`F`有効な式です (wrt[式](expressions.md)) として許可されている場合、 *statement_expression* ([式ステートメント](statements.md#expression-statements))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-527">If the body of `F` is an expression, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that would be permitted as a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>
*  <span data-ttu-id="f7a0d-528">場合の本文`F`がステートメント ブロックで、かつ`D`が、`void`型を返すまたは`F`は async と`D`戻り値の型を持つ`Task`、その後の各パラメーター`F`の型を指定対応するパラメーター`D`の本文`F`は有効なステートメント ブロックです (wrt[ブロック](statements.md#blocks)) いない`return`ステートメント式を指定します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-528">If the body of `F` is a statement block, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) in which no `return` statement specifies an expression.</span></span>
*  <span data-ttu-id="f7a0d-529">場合の本文`F`式であると*か*`F`は非同期ではないと`D`非 void の戻り値の型を持つ`T`、*または*`F`は async と`D`戻り値の型を持つ`Task<T>`、その後の各パラメーター`F`の対応するパラメーターの型が指定`D`の本文`F`有効な式です (wrt [式](expressions.md)) に暗黙的に変換可能`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-529">If the body of `F` is an expression, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that is implicitly convertible to `T`.</span></span>
*  <span data-ttu-id="f7a0d-530">場合の本文`F`ステートメント ブロックと*か*`F`は非同期ではないと`D`非 void の戻り値の型を持つ`T`、*または*`F`は async と`D`戻り値の型を持つ`Task<T>`、その後の各パラメーター`F`の対応するパラメーターの型が指定`D`の本文`F`は有効なステートメント ブロックです (wrt[ブロック](statements.md#blocks)) 各が到達可能な終了点で`return`ステートメントは、暗黙的に変換できる式を指定します。`T`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-530">If the body of `F` is a statement block, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) with a non-reachable end point in which each `return` statement specifies an expression that is implicitly convertible to `T`.</span></span>

<span data-ttu-id="f7a0d-531">このセクションで、簡潔さを優先するためにタスクの種類の短い形式を使用して`Task`と`Task<T>`([非同期関数](classes.md#async-functions))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-531">For the purpose of brevity, this section uses the short form for the task types `Task` and `Task<T>` ([Async functions](classes.md#async-functions)).</span></span>

<span data-ttu-id="f7a0d-532">ラムダ式`F`式ツリー型と互換性が`Expression<D>`場合`F`デリゲート型と互換性が`D`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-532">A lambda expression `F` is compatible with an expression tree type `Expression<D>` if `F` is compatible with the delegate type `D`.</span></span> <span data-ttu-id="f7a0d-533">匿名メソッド、ラムダ式のみをこの適用されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-533">Note that this does not apply to anonymous methods, only lambda expressions.</span></span>

<span data-ttu-id="f7a0d-534">特定のラムダ式は、式ツリー型に変換できません。場合でも、変換*が存在する*コンパイル時に失敗します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-534">Certain lambda expressions cannot be converted to expression tree types: Even though the conversion *exists*, it fails at compile-time.</span></span> <span data-ttu-id="f7a0d-535">これは、場合は、ラムダ式。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-535">This is the case if the lambda expression:</span></span>

*  <span data-ttu-id="f7a0d-536">*ブロック*本文</span><span class="sxs-lookup"><span data-stu-id="f7a0d-536">Has a *block* body</span></span>
*  <span data-ttu-id="f7a0d-537">単純型または複合代入演算子が含まれています</span><span class="sxs-lookup"><span data-stu-id="f7a0d-537">Contains simple or compound assignment operators</span></span>
*  <span data-ttu-id="f7a0d-538">動的にバインドされている式が含まれています</span><span class="sxs-lookup"><span data-stu-id="f7a0d-538">Contains a dynamically bound expression</span></span>
*  <span data-ttu-id="f7a0d-539">非同期には</span><span class="sxs-lookup"><span data-stu-id="f7a0d-539">Is async</span></span>

<span data-ttu-id="f7a0d-540">次の例は、汎用デリゲート型を使用して、`Func<A,R>`型の引数を受け取る関数を表す`A`型の値を返す`R`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-540">The examples that follow use a generic delegate type `Func<A,R>` which represents a function that takes an argument of type `A` and returns a value of type `R`:</span></span>
```csharp
delegate R Func<A,R>(A arg);
```

<span data-ttu-id="f7a0d-541">割り当て</span><span class="sxs-lookup"><span data-stu-id="f7a0d-541">In the assignments</span></span>
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
<span data-ttu-id="f7a0d-542">各匿名関数のパラメーターと戻り値の型は、匿名関数が割り当てられている変数の型から決まります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-542">the parameter and return types of each anonymous function are determined from the type of the variable to which the anonymous function is assigned.</span></span>

<span data-ttu-id="f7a0d-543">最初の割り当てが正常に匿名関数をデリゲート型に変換`Func<int,int>`ため、`x`型が指定`int`、`x+1`型に暗黙的に変換できる有効な式です`int`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-543">The first assignment successfully converts the anonymous function to the delegate type `Func<int,int>` because, when `x` is given type `int`, `x+1` is a valid expression that is implicitly convertible to type `int`.</span></span>

<span data-ttu-id="f7a0d-544">同様に、2 つ目の割り当てを正常に変換関数を匿名デリゲートの型に`Func<int,double>`ため、結果の`x+1`(型の`int`) 型に暗黙的に変換できる`double`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-544">Likewise, the second assignment successfully converts the anonymous function to the delegate type `Func<int,double>` because the result of `x+1` (of type `int`) is implicitly convertible to type `double`.</span></span>

<span data-ttu-id="f7a0d-545">しかし、3 番目の割り当てが、コンパイル時エラー、`x`型が指定`double`の結果`x+1`(型の`double`) 型に暗黙的に変換可能でない`int`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-545">However, the third assignment is a compile-time error because, when `x` is given type `double`, the result of `x+1` (of type `double`) is not implicitly convertible to type `int`.</span></span>

<span data-ttu-id="f7a0d-546">4 番目の割り当てが正常に匿名の非同期関数をデリゲート型に変換`Func<int, Task<int>>`ため、結果の`x+1`(型の`int`) 結果の型に暗黙的に変換できる`int`タスクの種類の`Task<int>`.</span><span class="sxs-lookup"><span data-stu-id="f7a0d-546">The fourth assignment successfully converts the anonymous async function to the delegate type `Func<int, Task<int>>` because the result of `x+1` (of type `int`) is implicitly convertible to the result type `int` of the task type `Task<int>`.</span></span>

<span data-ttu-id="f7a0d-547">匿名関数はオーバー ロードの解決に影響を与える可能性があり、型の推定に参加します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-547">Anonymous functions may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="f7a0d-548">参照してください[関数メンバー](expressions.md#function-members)の詳細。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-548">See [Function members](expressions.md#function-members) for further details.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a><span data-ttu-id="f7a0d-549">デリゲート型への匿名関数の変換の評価</span><span class="sxs-lookup"><span data-stu-id="f7a0d-549">Evaluation of anonymous function conversions to delegate types</span></span>

<span data-ttu-id="f7a0d-550">匿名関数のデリゲート型への変換では、匿名関数とキャプチャされた外部変数の評価時にアクティブになっている一連の (場合によっては空) を参照するデリゲート インスタンスを生成します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-550">Conversion of an anonymous function to a delegate type produces a delegate instance which references the anonymous function and the (possibly empty) set of captured outer variables that are active at the time of the evaluation.</span></span> <span data-ttu-id="f7a0d-551">デリゲートが呼び出されたときに、匿名関数の本体が実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-551">When the delegate is invoked, the body of the anonymous function is executed.</span></span> <span data-ttu-id="f7a0d-552">デリゲートによって参照される外部変数をキャプチャのセットを使用して、本体内のコードが実行されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-552">The code in the body is executed using the set of captured outer variables referenced by the delegate.</span></span>

<span data-ttu-id="f7a0d-553">匿名関数によって生成されたデリゲートの呼び出しリストには、1 つのエントリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-553">The invocation list of a delegate produced from an anonymous function contains a single entry.</span></span> <span data-ttu-id="f7a0d-554">正確なターゲット オブジェクトと、デリゲートのターゲット メソッドが指定されていません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-554">The exact target object and target method of the delegate are unspecified.</span></span> <span data-ttu-id="f7a0d-555">具体的には、指定されていないかどうか、デリゲートのターゲット オブジェクトは、 `null`、`this`外側の関数メンバー、またはその他のオブジェクトの値。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-555">In particular, it is unspecified whether the target object of the delegate is `null`, the `this` value of the enclosing function member, or some other object.</span></span>

<span data-ttu-id="f7a0d-556">同じデリゲート型への外部変数のキャプチャされたインスタンスの同じ (場合によっては空) のセットの意味的に同一の匿名関数の変換は許可されている (ただし必要ありません) を同じデリゲート インスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-556">Conversions of semantically identical anonymous functions with the same (possibly empty) set of captured outer variable instances to the same delegate types are permitted (but not required) to return the same delegate instance.</span></span> <span data-ttu-id="f7a0d-557">匿名関数の実行、すべてのケースで、表示されるのと同じ引数を指定したのと同じ効果を意味する意味的に同一の用語がここで使用されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-557">The term semantically identical is used here to mean that execution of the anonymous functions will, in all cases, produce the same effects given the same arguments.</span></span> <span data-ttu-id="f7a0d-558">このルールは、最適化するのには、次のようなコードを許可します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-558">This rule permits code such as the following to be optimized.</span></span>

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

<span data-ttu-id="f7a0d-559">2 つの関数を匿名デリゲートを同じ (空) と匿名関数は意味が同じであるため、キャプチャされた外部変数の設定があるため、コンパイラがデリゲートに同じターゲット メソッドを参照してください。 許可されています。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-559">Since the two anonymous function delegates have the same (empty) set of captured outer variables, and since the anonymous functions are semantically identical, the compiler is permitted to have the delegates refer to the same target method.</span></span> <span data-ttu-id="f7a0d-560">実際には、コンパイラは、両方の匿名関数式から、まったく同じデリゲート インスタンスを返すに許可されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-560">Indeed, the compiler is permitted to return the very same delegate instance from both anonymous function expressions.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a><span data-ttu-id="f7a0d-561">式ツリー型への匿名関数の変換の評価</span><span class="sxs-lookup"><span data-stu-id="f7a0d-561">Evaluation of anonymous function conversions to expression tree types</span></span>

<span data-ttu-id="f7a0d-562">匿名関数の式ツリー型への変換、式ツリーを生成します ([式ツリー型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-562">Conversion of an anonymous function to an expression tree type produces an expression tree ([Expression tree types](types.md#expression-tree-types)).</span></span> <span data-ttu-id="f7a0d-563">正確には、匿名関数の変換の評価は、匿名関数自体の構造を表すオブジェクトの構造の構築につながります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-563">More precisely, evaluation of the anonymous function conversion leads to the construction of an object structure that represents the structure of the anonymous function itself.</span></span> <span data-ttu-id="f7a0d-564">それを作成するための正確なプロセスと同様に、式ツリーの厳密な構造は、実装定義です。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-564">The precise structure of the expression tree, as well as the exact process for creating it, are implementation defined.</span></span>

### <a name="implementation-example"></a><span data-ttu-id="f7a0d-565">実装例</span><span class="sxs-lookup"><span data-stu-id="f7a0d-565">Implementation example</span></span>

<span data-ttu-id="f7a0d-566">このセクションでは、その他の c# コンストラクトの観点から匿名関数の変換の実装について説明します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-566">This section describes a possible implementation of anonymous function conversions in terms of other C# constructs.</span></span> <span data-ttu-id="f7a0d-567">ここで説明されている実装 Microsoft c# コンパイラで使用される同じ原則に基づいていますが、1 つだけのことも、必須の実装ではではありません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-567">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation, nor is it the only one possible.</span></span> <span data-ttu-id="f7a0d-568">ごく簡単に、この仕様の範囲外の正確なセマンティクスは、式ツリーへの変換を紹介します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-568">It only briefly mentions conversions to expression trees, as their exact semantics are outside the scope of this specification.</span></span>

<span data-ttu-id="f7a0d-569">このセクションの残りの部分では、異なる特性を持つ匿名関数を含むコードのいくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-569">The remainder of this section gives several examples of code that contains anonymous functions with different characteristics.</span></span> <span data-ttu-id="f7a0d-570">それぞれの例については、のみ c# の他のコンストラクトを使用するコードに対応する翻訳が提供されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-570">For each example, a corresponding translation to code that uses only other C# constructs is provided.</span></span> <span data-ttu-id="f7a0d-571">識別子の例についてで`D`次のデリゲート型で表すと見なされます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-571">In the examples, the identifier `D` is assumed by represent the following delegate type:</span></span>
```csharp
public delegate void D();
```

<span data-ttu-id="f7a0d-572">匿名関数の最も単純な形式は、外部変数をキャプチャしません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-572">The simplest form of an anonymous function is one that captures no outer variables:</span></span>
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

<span data-ttu-id="f7a0d-573">これは、匿名関数のコードが置かれているコンパイラによって生成された静的メソッドを参照するデリゲートのインスタンス化に書き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-573">This can be translated to a delegate instantiation that references a compiler generated static method in which the code of the anonymous function is placed:</span></span>
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

<span data-ttu-id="f7a0d-574">匿名関数がのインスタンス メンバーを参照する次の例では、 `this`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-574">In the following example, the anonymous function references instance members of `this`:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

<span data-ttu-id="f7a0d-575">これは、匿名関数のコードを含むコンパイラによって生成されたインスタンス メソッドに書き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-575">This can be translated to a compiler generated instance method containing the code of the anonymous function:</span></span>
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

<span data-ttu-id="f7a0d-576">この例では、匿名関数は、ローカル変数をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-576">In this example, the anonymous function captures a local variable:</span></span>
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

<span data-ttu-id="f7a0d-577">ローカル変数の有効期間を少なくとも、関数を匿名デリゲートの有効期間まで延長する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-577">The lifetime of the local variable must now be extended to at least the lifetime of the anonymous function delegate.</span></span> <span data-ttu-id="f7a0d-578">これは、コンパイラによって生成されたクラスのフィールドに、ローカル変数を「ホイスト」で実現できます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-578">This can be achieved by "hoisting" the local variable into a field of a compiler generated class.</span></span> <span data-ttu-id="f7a0d-579">ローカル変数のインスタンス化 ([ローカル変数のインスタンス化](expressions.md#instantiation-of-local-variables))、コンパイラによって生成されたクラスのインスタンス内のフィールドへのアクセスに対応するローカル変数にアクセスするのインスタンスを作成するのには対応し、コンパイラによって生成されたクラスです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-579">Instantiation of the local variable ([Instantiation of local variables](expressions.md#instantiation-of-local-variables)) then corresponds to creating an instance of the compiler generated class, and accessing the local variable corresponds to accessing a field in the instance of the compiler generated class.</span></span> <span data-ttu-id="f7a0d-580">さらに、匿名関数には、コンパイラによって生成されたクラスのインスタンス メソッドがようになります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-580">Furthermore, the anonymous function becomes an instance method of the compiler generated class:</span></span>
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

<span data-ttu-id="f7a0d-581">最後に、キャプチャの関数は次の匿名`this`と有効期間の異なる 2 つのローカル変数。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-581">Finally, the following anonymous function captures `this` as well as two local variables with different lifetimes:</span></span>
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

<span data-ttu-id="f7a0d-582">ここでは、コンパイラによって生成されたクラスを作成する for each ステートメントで、さまざまな要素でローカル変数は、独立した有効期間を持つことができますをどのローカル変数がキャプチャをブロックします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-582">Here, a compiler generated class is created for each statement block in which locals are captured such that the locals in the different blocks can have independent lifetimes.</span></span> <span data-ttu-id="f7a0d-583">インスタンス`__Locals2`、inner ステートメント ブロックのコンパイラによって生成されたクラスには、ローカル変数が含まれています。`z`のインスタンスを参照するフィールドと`__Locals1`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-583">An instance of `__Locals2`, the compiler generated class for the inner statement block, contains the local variable `z` and a field that references an instance of `__Locals1`.</span></span>  <span data-ttu-id="f7a0d-584">インスタンス`__Locals1`、外側のステートメント ブロックのコンパイラによって生成されたクラスには、ローカル変数が含まれています。`y`を参照するフィールドと`this`外側の関数メンバーの。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-584">An instance of `__Locals1`, the compiler generated class for the outer statement block, contains the local variable `y` and a field that references `this` of the enclosing function member.</span></span> <span data-ttu-id="f7a0d-585">アクセスには、これらのデータ構造ですべてのキャプチャされた外部変数のインスタンスを通じて`__Local2`、および匿名関数のコードはそのクラスのインスタンス メソッドとして実装できます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-585">With these data structures it is possible to reach all captured outer variables through an instance of `__Local2`, and the code of the anonymous function can thus be implemented as an instance method of that class.</span></span>

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

<span data-ttu-id="f7a0d-586">ここで適用されるローカル変数をキャプチャする同じ手法は、匿名関数を式ツリーに変換する場合にも使用できます。コンパイラによって生成されたオブジェクトへの参照は、式ツリーに格納できるし、これらのオブジェクトのフィールドにアクセスすると、ローカル変数へのアクセスを表すことができます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-586">The same technique applied here to capture local variables can also be used when converting anonymous functions to expression trees: References to the compiler generated objects can be stored in the expression tree, and access to the local variables can be represented as field accesses on these objects.</span></span> <span data-ttu-id="f7a0d-587">このアプローチの利点は、デリゲート、式ツリーの間で共有する「リフト」ローカル変数でできることです。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-587">The advantage of this approach is that it allows the "lifted" local variables to be shared between delegates and expression trees.</span></span>

## <a name="method-group-conversions"></a><span data-ttu-id="f7a0d-588">メソッド グループ変換</span><span class="sxs-lookup"><span data-stu-id="f7a0d-588">Method group conversions</span></span>

<span data-ttu-id="f7a0d-589">暗黙的な変換 ([暗黙的な変換](conversions.md#implicit-conversions))、メソッド グループからが存在する ([式の分類](expressions.md#expression-classifications)) 互換性のあるデリゲート型にします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-589">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from a method group ([Expression classifications](expressions.md#expression-classifications)) to a compatible delegate type.</span></span> <span data-ttu-id="f7a0d-590">デリゲート型を指定して`D`と式`E`メソッド グループに分類されるからの暗黙的な変換が存在する`E`に`D`場合`E`はその通常の形式 (該当する少なくとも 1 つのメソッドが含まれています[適用可能な関数メンバー](expressions.md#applicable-function-member))、引数リストへの修飾子をパラメーターの型の使用して構築された`D`以下で説明するようにします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-590">Given a delegate type `D` and an expression `E` that is classified as a method group, an implicit conversion exists from `E` to `D` if `E` contains at least one method that is applicable in its normal form ([Applicable function member](expressions.md#applicable-function-member)) to an argument list constructed by use of the parameter types and modifiers of `D`, as described in the following.</span></span>

<span data-ttu-id="f7a0d-591">メソッド グループからの変換のコンパイル時のアプリケーション`E`をデリゲート型に`D`については、次で説明します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-591">The compile-time application of a conversion from a method group `E` to a delegate type `D` is described in the following.</span></span> <span data-ttu-id="f7a0d-592">なおからの暗黙的な変換が存在する`E`に`D`変換のコンパイル時のアプリケーションがエラーなしに成功するには保証されません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-592">Note that the existence of an implicit conversion from `E` to `D` does not guarantee that the compile-time application of the conversion will succeed without error.</span></span>

*  <span data-ttu-id="f7a0d-593">1 つのメソッド`M`が選択されているメソッドの呼び出しに対応する ([メソッドの呼び出し](expressions.md#method-invocations)) フォームの`E(A)`、次の変更。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-593">A single method `M` is selected corresponding to a method invocation ([Method invocations](expressions.md#method-invocations)) of the form `E(A)`, with the following modifications:</span></span>
    * <span data-ttu-id="f7a0d-594">引数リスト`A`式、変数として、型および修飾子を使用して、各分類済みの一覧を示します (`ref`または`out`) に対応するパラメーターの*formal_parameter_list* の`D`.</span><span class="sxs-lookup"><span data-stu-id="f7a0d-594">The argument list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref` or `out`) of the corresponding parameter in the *formal_parameter_list* of `D`.</span></span>
    * <span data-ttu-id="f7a0d-595">候補となるメソッドと見なされますが、標準形式で適用されるメソッドのみ ([適用可能な関数メンバー](expressions.md#applicable-function-member))、拡張形式でのみ適用されません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-595">The candidate methods considered are only those methods that are applicable in their normal form ([Applicable function member](expressions.md#applicable-function-member)), not those applicable only in their expanded form.</span></span>
*  <span data-ttu-id="f7a0d-596">場合、アルゴリズムの[メソッドの呼び出し](expressions.md#method-invocations)コンパイル時エラーが発生し、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-596">If the algorithm of [Method invocations](expressions.md#method-invocations) produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="f7a0d-597">それ以外の場合、アルゴリズムが最適な 1 つのメソッドが生成されます`M`同じ同数のパラメーターを持つ`D`と変換が存在すると見なされます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-597">Otherwise the algorithm produces a single best method `M` having the same number of parameters as `D` and the conversion is considered to exist.</span></span>
*  <span data-ttu-id="f7a0d-598">選択したメソッド`M`互換である必要があります ([デリゲートの互換性](delegates.md#delegate-compatibility)) デリゲート型と`D`、またはそれ以外の場合、コンパイル時エラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-598">The selected method `M` must be compatible ([Delegate compatibility](delegates.md#delegate-compatibility)) with the delegate type `D`, or otherwise, a compile-time error occurs.</span></span>
*  <span data-ttu-id="f7a0d-599">場合、選択したメソッド`M`、インスタンス メソッドに関連付けられたインスタンス式である`E`デリゲートのターゲット オブジェクトを決定します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-599">If the selected method `M` is an instance method, the instance expression associated with `E` determines the target object of the delegate.</span></span>
*  <span data-ttu-id="f7a0d-600">インスタンス式でメンバー アクセスによって表される拡張メソッドを選択したメソッド M には、そのインスタンス式は、デリゲートの対象オブジェクトを決定します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-600">If the selected method M is an extension method which is denoted by means of a member access on an instance expression, that instance expression determines the target object of the delegate.</span></span>
*  <span data-ttu-id="f7a0d-601">変換の結果は、型の値 `D`、新しく作成されたデリゲート メソッドとターゲットの選択したオブジェクトを指す namely します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-601">The result of the conversion is a value of type `D`, namely a newly created delegate that refers to the selected method and target object.</span></span>
*  <span data-ttu-id="f7a0d-602">このプロセスが拡張メソッド、デリゲートの作成を簡単になることができる場合のアルゴリズム[メソッドの呼び出し](expressions.md#method-invocations)インスタンス メソッドの検索に失敗したが、処理の呼び出しに成功すると、`E(A)`拡張機能としてメソッドの呼び出し ([拡張メソッド呼び出し](expressions.md#extension-method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-602">Note that this process can lead to the creation of a delegate to an extension method, if the algorithm of [Method invocations](expressions.md#method-invocations) fails to find an instance method but succeeds in processing the invocation of `E(A)` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="f7a0d-603">こうして作成したデリゲートでは、拡張メソッドと、最初の引数をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-603">A delegate thus created captures the extension method as well as its first argument.</span></span>

<span data-ttu-id="f7a0d-604">次の例では、メソッド グループ変換を示しています。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-604">The following example demonstrates method group conversions:</span></span>
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

<span data-ttu-id="f7a0d-605">代入`d1`メソッド グループを暗黙的に変換します`F`型の値に`D1`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-605">The assignment to `d1` implicitly converts the method group `F` to a value of type `D1`.</span></span>

<span data-ttu-id="f7a0d-606">代入`d2`を弱い派生 (反変) パラメーターの型を持つメソッドにデリゲートを作成することはしより (共変性) の戻り値の型を派生する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-606">The assignment to `d2` shows how it is possible to create a delegate to a method that has less derived (contravariant) parameter types and a more derived (covariant) return type.</span></span>

<span data-ttu-id="f7a0d-607">代入`d3`表示変換が存在しないかどうか、メソッドは使用できません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-607">The assignment to `d3` shows how no conversion exists if the method is not applicable.</span></span>

<span data-ttu-id="f7a0d-608">代入`d4`メソッドが、標準形式で適用する必要がありますの方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-608">The assignment to `d4` shows how the method must be applicable in its normal form.</span></span>

<span data-ttu-id="f7a0d-609">代入`d5`参照型に対してのみ異なるデリゲートとメソッドのパラメーターと戻り値の型を許可する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-609">The assignment to `d5` shows how parameter and return types of the delegate and method are allowed to differ only for reference types.</span></span>

<span data-ttu-id="f7a0d-610">すべて、他の明示的および暗黙的な変換と明示的にメソッド グループ変換を実行するキャスト演算子を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-610">As with all other implicit and explicit conversions, the cast operator can be used to explicitly perform a method group conversion.</span></span> <span data-ttu-id="f7a0d-611">例では、そのため、</span><span class="sxs-lookup"><span data-stu-id="f7a0d-611">Thus, the example</span></span>
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
<span data-ttu-id="f7a0d-612">代わりに書き込まれる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-612">could instead be written</span></span>
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

<span data-ttu-id="f7a0d-613">メソッド グループは、オーバー ロードの解決に影響を与えるし、型の推定に参加可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-613">Method groups may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="f7a0d-614">参照してください[関数メンバー](expressions.md#function-members)の詳細。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-614">See [Function members](expressions.md#function-members) for further details.</span></span>

<span data-ttu-id="f7a0d-615">メソッド グループ変換の実行時の評価は、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-615">The run-time evaluation of a method group conversion proceeds as follows:</span></span>

*  <span data-ttu-id="f7a0d-616">関連付けられたインスタンス式からデリゲートのターゲット オブジェクトを確認するコンパイル時に選択されているメソッドがインスタンス メソッド、またはインスタンス メソッドとしてアクセスされる拡張メソッドは、 `E`:</span><span class="sxs-lookup"><span data-stu-id="f7a0d-616">If the method selected at compile-time is an instance method, or it is an extension method which is accessed as an instance method, the target object of the delegate is determined from the instance expression associated with `E`:</span></span>
    * <span data-ttu-id="f7a0d-617">インスタンス式が評価されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-617">The instance expression is evaluated.</span></span> <span data-ttu-id="f7a0d-618">この評価は、例外を発生させ、その後の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-618">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="f7a0d-619">インスタンス式がある場合、 *reference_type*、ターゲット オブジェクトをインスタンス式で計算された値になります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-619">If the instance expression is of a *reference_type*, the value computed by the instance expression becomes the target object.</span></span> <span data-ttu-id="f7a0d-620">選択したメソッドがインスタンス メソッドと、ターゲット オブジェクトがかどうか`null`、`System.NullReferenceException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-620">If the selected method is an instance method and the target object is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="f7a0d-621">インスタンス式がある場合、 *value_type*、ボックス化操作 ([ボックス化変換](types.md#boxing-conversions)) は、オブジェクトに値を変換する実行し、このオブジェクトがターゲット オブジェクトになります。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-621">If the instance expression is of a *value_type*, a boxing operation ([Boxing conversions](types.md#boxing-conversions)) is performed to convert the value to an object, and this object becomes the target object.</span></span>
*  <span data-ttu-id="f7a0d-622">それ以外の場合、選択したメソッドは静的メソッドの呼び出しの一部ですが、デリゲートのターゲット オブジェクトと`null`します。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-622">Otherwise the selected method is part of a static method call, and the target object of the delegate is `null`.</span></span>
*  <span data-ttu-id="f7a0d-623">デリゲート型の新しいインスタンス`D`が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-623">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="f7a0d-624">新しいインスタンスを割り当てることができる十分なメモリがない場合、`System.OutOfMemoryException`がスローされます、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-624">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="f7a0d-625">新しいデリゲート インスタンスがコンパイル時に決定されたメソッドへの参照で初期化され、上、ターゲット オブジェクトへの参照が計算されます。</span><span class="sxs-lookup"><span data-stu-id="f7a0d-625">The new delegate instance is initialized with a reference to the method that was determined at compile-time and a reference to the target object computed above.</span></span>
