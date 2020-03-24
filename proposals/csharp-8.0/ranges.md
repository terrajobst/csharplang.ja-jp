---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122949"
---
# <a name="ranges"></a><span data-ttu-id="b780b-101">範囲</span><span class="sxs-lookup"><span data-stu-id="b780b-101">Ranges</span></span>

## <a name="summary"></a><span data-ttu-id="b780b-102">要約</span><span class="sxs-lookup"><span data-stu-id="b780b-102">Summary</span></span>

<span data-ttu-id="b780b-103">この機能は、`System.Index` と `System.Range` オブジェクトの構築を可能にし、実行時にそれらを使用してコレクションのインデックス作成とスライスを行うことができる2つの新しい演算子を提供することです。</span><span class="sxs-lookup"><span data-stu-id="b780b-103">This feature is about delivering two new operators that allow constructing `System.Index` and `System.Range` objects, and using them to index/slice collections at runtime.</span></span>

## <a name="overview"></a><span data-ttu-id="b780b-104">概要</span><span class="sxs-lookup"><span data-stu-id="b780b-104">Overview</span></span>

### <a name="well-known-types-and-members"></a><span data-ttu-id="b780b-105">既知の型とメンバー</span><span class="sxs-lookup"><span data-stu-id="b780b-105">Well-known types and members</span></span>

<span data-ttu-id="b780b-106">`System.Index` と `System.Range`に新しい構文形式を使用するには、使用される構文形式に応じて、新しい既知の型とメンバーが必要になることがあります。</span><span class="sxs-lookup"><span data-stu-id="b780b-106">To use the new syntactic forms for `System.Index` and `System.Range`, new well-known types and members may be necessary, depending on which syntactic forms are used.</span></span>

<span data-ttu-id="b780b-107">"Hat" 演算子 (`^`) を使用するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="b780b-107">To use the "hat" operator (`^`), the following is required</span></span>

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

<span data-ttu-id="b780b-108">`System.Index` 型を配列要素アクセスの引数として使用するには、次のメンバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="b780b-108">To use the `System.Index` type as an argument in an array element access, the following member is required:</span></span>

```csharp
int System.Index.GetOffset(int length);
```

<span data-ttu-id="b780b-109">`System.Range` の `..` 構文には、`System.Range` の種類と、次のメンバーの1つ以上が必要です。</span><span class="sxs-lookup"><span data-stu-id="b780b-109">The `..` syntax for `System.Range` will require the `System.Range` type, as well as one or more of the following members:</span></span>

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

<span data-ttu-id="b780b-110">`..` 構文では、どちらの場合も、いずれの引数も指定できません。</span><span class="sxs-lookup"><span data-stu-id="b780b-110">The `..` syntax allows for either, both, or none of its arguments to be absent.</span></span> <span data-ttu-id="b780b-111">引数の数に関係なく、`Range` 構文を使用するには、`Range` コンストラクターが常に十分です。</span><span class="sxs-lookup"><span data-stu-id="b780b-111">Regardless of the number of arguments, the `Range` constructor is always sufficient for using the `Range` syntax.</span></span> <span data-ttu-id="b780b-112">ただし、他のメンバーのいずれかが存在し、1つ以上の `..` 引数が不足している場合は、適切なメンバーを置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="b780b-112">However, if any of the other members are present and one or more of the `..` arguments are missing, the appropriate member may be substituted.</span></span>

<span data-ttu-id="b780b-113">最後に、配列要素のアクセス式で使用される `System.Range` 型の値については、次のメンバーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b780b-113">Finally, for a value of type `System.Range` to be used in an array element access expression, the following member must be present:</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a><span data-ttu-id="b780b-114">System.string</span><span class="sxs-lookup"><span data-stu-id="b780b-114">System.Index</span></span>

<span data-ttu-id="b780b-115">C#では、最終的にコレクションのインデックスを作成する方法はありませんが、ほとんどのインデクサーは "from start" の概念を使用するか、"length-i" 式を実行します。</span><span class="sxs-lookup"><span data-stu-id="b780b-115">C# has no way of indexing a collection from the end, but rather most indexers use the "from start" notion, or do a "length - i" expression.</span></span> <span data-ttu-id="b780b-116">"終了" を意味する新しいインデックス式が導入されています。</span><span class="sxs-lookup"><span data-stu-id="b780b-116">We introduce a new Index expression that means "from the end".</span></span> <span data-ttu-id="b780b-117">この機能により、新しい単項プレフィックス "hat" 演算子が導入されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-117">The feature will introduce a new unary prefix "hat" operator.</span></span> <span data-ttu-id="b780b-118">1つのオペランドは `System.Int32`に変換可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="b780b-118">Its single operand must be convertible to `System.Int32`.</span></span> <span data-ttu-id="b780b-119">このメソッドは、適切な `System.Index` ファクトリメソッド呼び出しに下げられます。</span><span class="sxs-lookup"><span data-stu-id="b780b-119">It will be lowered into the appropriate `System.Index` factory method call.</span></span>

<span data-ttu-id="b780b-120">次の追加の構文形式を使用して、 *unary_expression*の文法を拡張します。</span><span class="sxs-lookup"><span data-stu-id="b780b-120">We augment the grammar for *unary_expression* with the following additional syntax form:</span></span>

```antlr
unary_expression
    : '^' unary_expression
    ;
```

<span data-ttu-id="b780b-121">*End 演算子の index*を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b780b-121">We call this the *index from end* operator.</span></span> <span data-ttu-id="b780b-122">終了演算子*の*定義済みインデックスは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b780b-122">The predefined *index from end* operators are as follows:</span></span>

```csharp
    System.Index operator ^(int fromEnd);
```

<span data-ttu-id="b780b-123">この演算子の動作は、0以上の入力値に対してのみ定義されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-123">The behavior of this operator is only defined for input values greater than or equal to zero.</span></span>

<span data-ttu-id="b780b-124">例 :</span><span class="sxs-lookup"><span data-stu-id="b780b-124">Examples:</span></span>

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a><span data-ttu-id="b780b-125">System.string</span><span class="sxs-lookup"><span data-stu-id="b780b-125">System.Range</span></span>

<span data-ttu-id="b780b-126">C#には、コレクションの "範囲" または "スライス" にアクセスする構文的な方法はありません。</span><span class="sxs-lookup"><span data-stu-id="b780b-126">C# has no syntactic way to access "ranges" or "slices" of collections.</span></span> <span data-ttu-id="b780b-127">通常、ユーザーは、メモリのスライスをフィルター処理したり操作したりするための複雑な構造を実装したり、`list.Skip(5).Take(2)`のような LINQ メソッドを活用したりすることが求められます。</span><span class="sxs-lookup"><span data-stu-id="b780b-127">Usually users are forced to implement complex structures to filter/operate on slices of memory, or resort to LINQ methods like `list.Skip(5).Take(2)`.</span></span> <span data-ttu-id="b780b-128">`System.Span<T>` とその他の類似する型を追加することで、このような操作を言語/ランタイムのより深いレベルでサポートし、インターフェイスを統合することが重要になります。</span><span class="sxs-lookup"><span data-stu-id="b780b-128">With the addition of `System.Span<T>` and other similar types, it becomes more important to have this kind of operation supported on a deeper level in the language/runtime, and have the interface unified.</span></span>

<span data-ttu-id="b780b-129">この言語では、新しい範囲演算子 `x..y`が導入されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-129">The language will introduce a new range operator `x..y`.</span></span> <span data-ttu-id="b780b-130">2つの式を受け取るバイナリ挿入演算子です。</span><span class="sxs-lookup"><span data-stu-id="b780b-130">It is a binary infix operator that accepts two expressions.</span></span> <span data-ttu-id="b780b-131">どちらのオペランドも省略できます (以下の例を参照)。 `System.Index`に変換可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="b780b-131">Either operand can be omitted (examples below), and they have to be convertible to `System.Index`.</span></span> <span data-ttu-id="b780b-132">このメソッドは、適切な `System.Range` ファクトリメソッド呼び出しに下げられます。</span><span class="sxs-lookup"><span data-stu-id="b780b-132">It will be lowered to the appropriate `System.Range` factory method call.</span></span>

<span data-ttu-id="b780b-133">C# *Multiplicative_expression*の文法規則を次のように置き換えます (新しい優先順位を導入するため)。</span><span class="sxs-lookup"><span data-stu-id="b780b-133">We replace the C# grammar rules for *multiplicative_expression* with the following (in order to introduce a new precedence level):</span></span>

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

<span data-ttu-id="b780b-134">*範囲演算子*のすべての形式の優先順位は同じです。</span><span class="sxs-lookup"><span data-stu-id="b780b-134">All forms of the *range operator* have the same precedence.</span></span> <span data-ttu-id="b780b-135">この新しい優先順位グループは、*単項演算子*よりも小さく、 *mulitiplicative 算術演算子*よりも高くなっています。</span><span class="sxs-lookup"><span data-stu-id="b780b-135">This new precedence group is lower than the *unary operators* and higher than the *mulitiplicative arithmetic operators*.</span></span>

<span data-ttu-id="b780b-136">`..` 演算子を*範囲演算子*と呼びます。</span><span class="sxs-lookup"><span data-stu-id="b780b-136">We call the `..` operator the *range operator*.</span></span> <span data-ttu-id="b780b-137">組み込みの範囲演算子は、次の形式の組み込み演算子の呼び出しに対応するように、ほぼ理解できます。</span><span class="sxs-lookup"><span data-stu-id="b780b-137">The built-in range operator can roughly be understood to correspond to the invocation of a built-in operator of this form:</span></span>

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

<span data-ttu-id="b780b-138">例 :</span><span class="sxs-lookup"><span data-stu-id="b780b-138">Examples:</span></span>

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

<span data-ttu-id="b780b-139">さらに、多次元署名に対して整数とインデックスを混在させるためにオーバーロードする必要がないように、`System.Index` は `System.Int32`からの暗黙の型変換を必要とします。</span><span class="sxs-lookup"><span data-stu-id="b780b-139">Moreover, `System.Index` should have an implicit conversion from `System.Int32`, in order not to need to overload for mixing integers and indexes over multi-dimensional signatures.</span></span>

## <a name="adding-index-and-range-support-to-existing-library-types"></a><span data-ttu-id="b780b-140">既存のライブラリの種類へのインデックスと範囲のサポートの追加</span><span class="sxs-lookup"><span data-stu-id="b780b-140">Adding Index and Range support to existing library types</span></span>

### <a name="implicit-index-support"></a><span data-ttu-id="b780b-141">暗黙のインデックスのサポート</span><span class="sxs-lookup"><span data-stu-id="b780b-141">Implicit Index support</span></span>

<span data-ttu-id="b780b-142">この言語では、次の条件を満たす型の `Index` 型の単一のパラメーターを持つインスタンスインデクサーメンバーが提供されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-142">The language will provide an instance indexer member with a single parameter of type `Index` for types which meet the following criteria:</span></span>

- <span data-ttu-id="b780b-143">種類は不可能です。</span><span class="sxs-lookup"><span data-stu-id="b780b-143">The type is Countable.</span></span>
- <span data-ttu-id="b780b-144">型には、引数として1つの `int` を受け取る、アクセス可能なインスタンスインデクサーがあります。</span><span class="sxs-lookup"><span data-stu-id="b780b-144">The type has an accessible instance indexer which takes a single `int` as the argument.</span></span>
- <span data-ttu-id="b780b-145">この型には、最初のパラメーターとして `Index` を受け取るアクセス可能なインスタンスインデクサーがありません。</span><span class="sxs-lookup"><span data-stu-id="b780b-145">The type does not have an accessible instance indexer which takes an `Index` as the first parameter.</span></span> <span data-ttu-id="b780b-146">`Index` は唯一のパラメーターであるか、またはその他のパラメーターは省略可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="b780b-146">The `Index` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="b780b-147">型は、`Length` という名前のプロパティがある場合、またはアクセス可能な getter と戻り値の型が `int`の `Count` である場合に、***不可能***になります。</span><span class="sxs-lookup"><span data-stu-id="b780b-147">A type is ***Countable*** if it  has a property named `Length` or `Count` with an accessible getter and a return type of `int`.</span></span> <span data-ttu-id="b780b-148">この言語では、このプロパティを使用して `Index` 型の式を式の位置で `int` に変換することができます。 `Index` 型を使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b780b-148">The language can make use of this property to convert an expression of type `Index` into an `int` at the point of the expression without the need to use the type `Index` at all.</span></span> <span data-ttu-id="b780b-149">`Length` と `Count` の両方が存在する場合は、`Length` が優先されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-149">In case both `Length` and `Count` are present, `Length` will be preferred.</span></span> <span data-ttu-id="b780b-150">わかりやすくするために、提案では、`Count` または `Length`を表すために `Length` という名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="b780b-150">For simplicity going forward, the proposal will use the name `Length` to represent `Count` or `Length`.</span></span>

<span data-ttu-id="b780b-151">このような型の場合、言語は `T this[Index index]` フォームのインデクサーメンバーがあるかのように動作します。 `T` は、`ref` スタイルの注釈を含む `int` ベースのインデクサーの戻り値の型です。</span><span class="sxs-lookup"><span data-stu-id="b780b-151">For such types, the language will act as if there is an indexer member of the form `T this[Index index]` where `T` is the return type of the `int` based indexer including any `ref` style annotations.</span></span> <span data-ttu-id="b780b-152">新しいメンバーには、`int` インデクサーと一致するアクセシビリティを持つメンバーと同じ `get` と `set` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-152">The new member will have the same `get` and `set` members with matching accessibility as the `int` indexer.</span></span> 

<span data-ttu-id="b780b-153">`Index` 型の引数を `int` に変換し、`int` ベースのインデクサーへの呼び出しを出力することによって、新しいインデクサーが実装されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-153">The new indexer will be implemented by converting the argument of type `Index` into an `int` and emitting a call to the `int` based indexer.</span></span> <span data-ttu-id="b780b-154">説明のために、`receiver[expr]`の例を使用してみましょう。</span><span class="sxs-lookup"><span data-stu-id="b780b-154">For discussion purposes, let's use the example of `receiver[expr]`.</span></span> <span data-ttu-id="b780b-155">`expr` から `int` への変換は次のように行われます。</span><span class="sxs-lookup"><span data-stu-id="b780b-155">The conversion of `expr` to `int` will occur as follows:</span></span>

- <span data-ttu-id="b780b-156">引数が `^expr2` の形式で、`expr2` の型が `int`場合、その引数は `receiver.Length - expr2`に変換されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-156">When the argument is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="b780b-157">それ以外の場合は、`expr.GetOffset(receiver.Length)`として変換されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-157">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="b780b-158">これにより、開発者は、既存の型で `Index` 機能を使用して、変更を加える必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="b780b-158">This allows for developers to use the `Index` feature on existing types without the need for modification.</span></span> <span data-ttu-id="b780b-159">例 :</span><span class="sxs-lookup"><span data-stu-id="b780b-159">For example:</span></span>

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

<span data-ttu-id="b780b-160">`receiver` と `Length` の式は、副作用が1回だけ実行されるように、必要に応じて書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="b780b-160">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="b780b-161">例 :</span><span class="sxs-lookup"><span data-stu-id="b780b-161">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="b780b-162">このコードは、"Get Length 3" を出力します。</span><span class="sxs-lookup"><span data-stu-id="b780b-162">This code will print "Get Length 3".</span></span>

### <a name="implicit-range-support"></a><span data-ttu-id="b780b-163">暗黙的な範囲のサポート</span><span class="sxs-lookup"><span data-stu-id="b780b-163">Implicit Range support</span></span>

<span data-ttu-id="b780b-164">この言語では、次の条件を満たす型の `Range` 型の単一のパラメーターを持つインスタンスインデクサーメンバーが提供されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-164">The language will provide an instance indexer member with a single parameter of type `Range` for types which meet the following criteria:</span></span>

- <span data-ttu-id="b780b-165">種類は不可能です。</span><span class="sxs-lookup"><span data-stu-id="b780b-165">The type is Countable.</span></span>
- <span data-ttu-id="b780b-166">型には、`int`型の2つのパラメーターを持つ `Slice` という名前のアクセス可能なメンバーがあります。</span><span class="sxs-lookup"><span data-stu-id="b780b-166">The type has an accessible member named `Slice` which has two parameters of type `int`.</span></span>
- <span data-ttu-id="b780b-167">この型には、最初のパラメーターとして1つの `Range` を受け取るインスタンスインデクサーがありません。</span><span class="sxs-lookup"><span data-stu-id="b780b-167">The type does not have an instance indexer which takes a single `Range` as the first parameter.</span></span> <span data-ttu-id="b780b-168">`Range` は唯一のパラメーターであるか、またはその他のパラメーターは省略可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="b780b-168">The `Range` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="b780b-169">このような型の場合、言語は `T this[Range range]` フォームのインデクサーメンバーがあるかのようにバインドされます。 `T` は `Slice` メソッドの戻り値の型であり、`ref` スタイルの注釈が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b780b-169">For such types, the language will bind as if there is an indexer member of the form `T this[Range range]` where `T` is the return type of the `Slice` method including any `ref` style annotations.</span></span> <span data-ttu-id="b780b-170">また、新しいメンバーのアクセシビリティは、`Slice`と一致します。</span><span class="sxs-lookup"><span data-stu-id="b780b-170">The new member will also have matching accessibility with `Slice`.</span></span> 

<span data-ttu-id="b780b-171">`Range` ベースのインデクサーが `receiver`という名前の式でバインドされている場合、`Range` 式を2つの値に変換し、その後 `Slice` メソッドに渡されることで、このインデクサーは減少します。</span><span class="sxs-lookup"><span data-stu-id="b780b-171">When the `Range` based indexer is bound on an expression named `receiver`, it will be lowered by converting the `Range` expression into two values that are then passed to the `Slice` method.</span></span> <span data-ttu-id="b780b-172">説明のために、`receiver[expr]`の例を使用してみましょう。</span><span class="sxs-lookup"><span data-stu-id="b780b-172">For discussion purposes, let's use the example of `receiver[expr]`.</span></span>

<span data-ttu-id="b780b-173">`Slice` の最初の引数は、次のように、型指定された式を次のように変換することによって取得します。</span><span class="sxs-lookup"><span data-stu-id="b780b-173">The first argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="b780b-174">`expr` の形式が `expr1..expr2` (`expr2` は省略可能) で、`expr1` の型が `int`の場合、`expr1`として出力されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-174">When `expr` is of the form `expr1..expr2` (where `expr2` can be omitted) and `expr1` has type `int`, then it will be emitted as `expr1`.</span></span>
- <span data-ttu-id="b780b-175">`expr` が `^expr1..expr2` 形式 (`expr2` を省略できる) の場合、`receiver.Length - expr1`として出力されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-175">When `expr` is of the form `^expr1..expr2` (where `expr2` can be omitted), then it will be emitted as `receiver.Length - expr1`.</span></span>
- <span data-ttu-id="b780b-176">`expr` が `..expr2` 形式 (`expr2` を省略できる) の場合、`0`として出力されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-176">When `expr` is of the form `..expr2` (where `expr2` can be omitted), then it will be emitted as `0`.</span></span>
- <span data-ttu-id="b780b-177">それ以外の場合は、`expr.Start.GetOffset(receiver.Length)`として出力されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-177">Otherwise, it will be emitted as `expr.Start.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="b780b-178">この値は、2番目の `Slice` 引数の計算で再利用されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-178">This value will be re-used in the calculation of the second `Slice` argument.</span></span> <span data-ttu-id="b780b-179">この操作を実行すると、`start`と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="b780b-179">When doing so it will be referred to as `start`.</span></span> <span data-ttu-id="b780b-180">`Slice` の2番目の引数は、次のように、型指定された式を次のように変換することによって取得します。</span><span class="sxs-lookup"><span data-stu-id="b780b-180">The second argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="b780b-181">`expr` の形式が `expr1..expr2` (`expr1` は省略可能) で、`expr2` の型が `int`の場合、`expr2 - start`として出力されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-181">When `expr` is of the form `expr1..expr2` (where `expr1` can be omitted) and `expr2` has type `int`, then it will be emitted as `expr2 - start`.</span></span>
- <span data-ttu-id="b780b-182">`expr` が `expr1..^expr2` 形式 (`expr1` を省略できる) の場合、`(receiver.Length - expr2) - start`として出力されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-182">When `expr` is of the form `expr1..^expr2` (where `expr1` can be omitted), then it will be emitted as `(receiver.Length - expr2) - start`.</span></span>
- <span data-ttu-id="b780b-183">`expr` が `expr1..` 形式 (`expr1` を省略できる) の場合、`receiver.Length - start`として出力されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-183">When `expr` is of the form `expr1..` (where `expr1` can be omitted), then it will be emitted as `receiver.Length - start`.</span></span>
- <span data-ttu-id="b780b-184">それ以外の場合は、`expr.End.GetOffset(receiver.Length) - start`として出力されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-184">Otherwise, it will be emitted as `expr.End.GetOffset(receiver.Length) - start`.</span></span>

<span data-ttu-id="b780b-185">`receiver`、`Length`、および `expr` 式は、副作用が1回だけ実行されるようにするために必要に応じて書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="b780b-185">The `receiver`, `Length`, and `expr` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="b780b-186">例 :</span><span class="sxs-lookup"><span data-stu-id="b780b-186">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

<span data-ttu-id="b780b-187">このコードは、"Get Length 2" を出力します。</span><span class="sxs-lookup"><span data-stu-id="b780b-187">This code will print "Get Length 2".</span></span>

<span data-ttu-id="b780b-188">この言語では、次の既知の型が特別なケースになります。</span><span class="sxs-lookup"><span data-stu-id="b780b-188">The language will special case the following known types:</span></span> 

- <span data-ttu-id="b780b-189">`string`: `Slice`の代わりに `Substring` メソッドが使用されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-189">`string`: the method `Substring` will be used instead of `Slice`.</span></span>
- <span data-ttu-id="b780b-190">`array`: `Slice`の代わりに `System.Reflection.CompilerServices.GetSubArray` メソッドが使用されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-190">`array`: the method `System.Reflection.CompilerServices.GetSubArray` will be used instead of `Slice`.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b780b-191">代替</span><span class="sxs-lookup"><span data-stu-id="b780b-191">Alternatives</span></span>

<span data-ttu-id="b780b-192">新しい演算子 (`^` と `..`) は構文砂糖です。</span><span class="sxs-lookup"><span data-stu-id="b780b-192">The new operators (`^` and `..`) are syntactic sugar.</span></span> <span data-ttu-id="b780b-193">この機能は `System.Index` および `System.Range` ファクトリメソッドを明示的に呼び出すことによって実装できますが、多くの定型コードが生成され、エクスペリエンスはにくいになります。</span><span class="sxs-lookup"><span data-stu-id="b780b-193">The functionality can be implemented by explicit calls to `System.Index` and `System.Range` factory methods, but it will result in a lot more boilerplate code, and the experience will be unintuitive.</span></span>

## <a name="il-representation"></a><span data-ttu-id="b780b-194">IL 表現</span><span class="sxs-lookup"><span data-stu-id="b780b-194">IL Representation</span></span>

<span data-ttu-id="b780b-195">これら2つの演算子は、通常のインデクサー/メソッド呼び出しに対して低下しますが、それ以降のコンパイラレイヤーは変更されません。</span><span class="sxs-lookup"><span data-stu-id="b780b-195">These two operators will be lowered to regular indexer/method calls, with no change in subsequent compiler layers.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="b780b-196">実行時の動作</span><span class="sxs-lookup"><span data-stu-id="b780b-196">Runtime behavior</span></span>

- <span data-ttu-id="b780b-197">コンパイラでは、配列や文字列などの組み込み型のインデクサーを最適化し、適切な既存のメソッドへのインデックス作成を下げることができます。</span><span class="sxs-lookup"><span data-stu-id="b780b-197">Compiler can optimize indexers for built-in types like arrays and strings, and lower the indexing to the appropriate existing methods.</span></span>
- <span data-ttu-id="b780b-198">負の値を使用して構築された場合、`System.Index` はをスローします。</span><span class="sxs-lookup"><span data-stu-id="b780b-198">`System.Index` will throw if constructed with a negative value.</span></span>
- <span data-ttu-id="b780b-199">`^0` はスローしませんが、指定されたコレクションまたは列挙型の長さに変換されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-199">`^0` does not throw, but it translates to the length of the collection/enumerable it is supplied to.</span></span>
- <span data-ttu-id="b780b-200">`Range.All` は `0..^0`と意味的に同等であり、これらのインデックスに分解ことができます。</span><span class="sxs-lookup"><span data-stu-id="b780b-200">`Range.All` is semantically equivalent to `0..^0`, and can be deconstructed to these indices.</span></span>

## <a name="considerations"></a><span data-ttu-id="b780b-201">考慮事項</span><span class="sxs-lookup"><span data-stu-id="b780b-201">Considerations</span></span>

### <a name="detect-indexable-based-on-icollection"></a><span data-ttu-id="b780b-202">ICollection に基づいてインデックス可能の検出</span><span class="sxs-lookup"><span data-stu-id="b780b-202">Detect Indexable based on ICollection</span></span>

<span data-ttu-id="b780b-203">この動作のインスピレーションは、コレクション初期化子でした。</span><span class="sxs-lookup"><span data-stu-id="b780b-203">The inspiration for this behavior was collection initializers.</span></span> <span data-ttu-id="b780b-204">型の構造体を使用して、機能が選択されたことを伝えます。</span><span class="sxs-lookup"><span data-stu-id="b780b-204">Using the structure of a type to convey that it had opted into a feature.</span></span> <span data-ttu-id="b780b-205">コレクション初期化子型の場合は、インターフェイス `IEnumerable` (非ジェネリック) を実装することによって、機能を選択できます。</span><span class="sxs-lookup"><span data-stu-id="b780b-205">In the case of collection initializers types can opt into the feature by implementing the interface `IEnumerable` (non generic).</span></span>

<span data-ttu-id="b780b-206">この提案では、最初に、型がインデックス可能と見なされるために `ICollection` を実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b780b-206">This proposal initially required that types implement `ICollection` in order to qualify as Indexable.</span></span> <span data-ttu-id="b780b-207">ただし、次のような特殊なケースが必要でした。</span><span class="sxs-lookup"><span data-stu-id="b780b-207">That required a number of special cases though:</span></span>

- <span data-ttu-id="b780b-208">`ref struct`: これらのインターフェイスを実装することはできません `Span<T>` のような型は、インデックス/範囲のサポートに適しています。</span><span class="sxs-lookup"><span data-stu-id="b780b-208">`ref struct`: these cannot implement interfaces yet types like `Span<T>` are ideal for index / range support.</span></span> 
- <span data-ttu-id="b780b-209">`string`: `ICollection` を実装せず、`interface` に大きなコストを追加します。</span><span class="sxs-lookup"><span data-stu-id="b780b-209">`string`: does not implement `ICollection` and adding that `interface` has a large cost.</span></span>

<span data-ttu-id="b780b-210">このため、キーの種類をサポートするには、特別な大文字と小文字の区別が既に必要です。</span><span class="sxs-lookup"><span data-stu-id="b780b-210">This means to support key types special casing is already needed.</span></span> <span data-ttu-id="b780b-211">`string` の特殊な大文字と小文字の区別は、他の領域 (`foreach`、定数など) での言語の方が興味深いものではありません。`ref struct` の特殊な大文字と小文字の区別は、型のクラス全体に対して特殊な大文字と小文字が区別されるため、より詳細になります。</span><span class="sxs-lookup"><span data-stu-id="b780b-211">The special casing of `string` is less interesting as the language does this in other areas (`foreach` lowering, constants, etc ...). The special casing of `ref struct` is more concerning as it's special casing an entire class of types.</span></span> <span data-ttu-id="b780b-212">戻り値の型が `int`の `Count` という名前のプロパティがあるだけであれば、インデックスを作成できます。</span><span class="sxs-lookup"><span data-stu-id="b780b-212">They get labeled as Indexable if they simply have a property named `Count` with a return type of `int`.</span></span> 

<span data-ttu-id="b780b-213">この設計は、戻り値の型が `int` の `Length`  / プロパティ `Count`を持つ任意の型がインデックス可能であるということを考慮しています。</span><span class="sxs-lookup"><span data-stu-id="b780b-213">After consideration the design was normalized to say that any type which has a property `Count` / `Length` with a return type of `int` is Indexable.</span></span> <span data-ttu-id="b780b-214">これにより、`string` と配列の場合でも、特殊な大文字と小文字がすべて削除されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-214">That removes all special casing, even for `string` and arrays.</span></span>

### <a name="detect-just-count"></a><span data-ttu-id="b780b-215">カウントのみ検出</span><span class="sxs-lookup"><span data-stu-id="b780b-215">Detect just Count</span></span>

<span data-ttu-id="b780b-216">`Count` または `Length` のプロパティ名を検出すると、設計が少し複雑になります。</span><span class="sxs-lookup"><span data-stu-id="b780b-216">Detecting on the property names `Count` or `Length` does complicate the design a bit.</span></span> <span data-ttu-id="b780b-217">1つだけを選択するだけでは、多数の型を除外した場合には不十分です。</span><span class="sxs-lookup"><span data-stu-id="b780b-217">Picking just one to standardize though is not sufficient as it ends up excluding a large number of types:</span></span>

- <span data-ttu-id="b780b-218">`Length`の使用: システムコレクションとサブ名前空間のすべてのコレクションを除外します。</span><span class="sxs-lookup"><span data-stu-id="b780b-218">Use `Length`: excludes pretty much every collection in System.Collections and sub-namespaces.</span></span> <span data-ttu-id="b780b-219">これらは `ICollection` から派生する傾向があるため、長さが `Count` を優先します。</span><span class="sxs-lookup"><span data-stu-id="b780b-219">Those tend to derive from `ICollection` and hence prefer `Count` over length.</span></span>
- <span data-ttu-id="b780b-220">`Count`を使用: `string`、配列、`Span<T>`、およびほとんどの `ref struct` ベースの型を除外します。</span><span class="sxs-lookup"><span data-stu-id="b780b-220">Use `Count`: excludes `string`, arrays, `Span<T>` and most `ref struct` based types</span></span>

<span data-ttu-id="b780b-221">インデックス可能な型の初期検出に対する余分な複雑さは、他の側面での単純化によって上回るされます。</span><span class="sxs-lookup"><span data-stu-id="b780b-221">The extra complication on the initial detection of Indexable types is outweighed by its simplification in other aspects.</span></span>

### <a name="choice-of-slice-as-a-name"></a><span data-ttu-id="b780b-222">名前としてのスライスの選択</span><span class="sxs-lookup"><span data-stu-id="b780b-222">Choice of Slice as a name</span></span>

<span data-ttu-id="b780b-223">.NET でのスライススタイル操作の事実上の標準名として `Slice` 名前が選択されました。</span><span class="sxs-lookup"><span data-stu-id="b780b-223">The name `Slice` was chosen as it's the de-facto standard name for slice style operations in .NET.</span></span> <span data-ttu-id="b780b-224">Netcoreapp 2.1 以降では、すべてのスパンスタイルの種類でスライス操作に `Slice` 名前が使用されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-224">Starting with netcoreapp2.1 all span style types use the name `Slice` for slicing operations.</span></span> <span data-ttu-id="b780b-225">Netcoreapp 2.1 より前の例では、スライスの例はありません。</span><span class="sxs-lookup"><span data-stu-id="b780b-225">Prior to netcoreapp2.1 there really aren't any examples of slicing to look to for an example.</span></span> <span data-ttu-id="b780b-226">`List<T>`、`ArraySegment<T>`、`SortedList<T>` などの型は、スライスには理想的ですが、型が追加されたときの概念は存在しませんでした。</span><span class="sxs-lookup"><span data-stu-id="b780b-226">Types like `List<T>`, `ArraySegment<T>`, `SortedList<T>` would've been ideal for slicing but the concept didn't exist when types were added.</span></span> 

<span data-ttu-id="b780b-227">そのため、唯一の例として、名前として選択された `Slice` ます。</span><span class="sxs-lookup"><span data-stu-id="b780b-227">Thus, `Slice` being the sole example, it was chosen as the name.</span></span>

### <a name="index-target-type-conversion"></a><span data-ttu-id="b780b-228">インデックスターゲット型の変換</span><span class="sxs-lookup"><span data-stu-id="b780b-228">Index target type conversion</span></span>

<span data-ttu-id="b780b-229">インデクサー式で `Index` 変換を表示するもう1つの方法は、ターゲット型の変換です。</span><span class="sxs-lookup"><span data-stu-id="b780b-229">Another way to view the `Index` transformation in an indexer expression is as a target type conversion.</span></span> <span data-ttu-id="b780b-230">`return_type this[Index]`フォームのメンバーであるかのようにバインドするのではなく、変換先の型指定された変換を `int`に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="b780b-230">Instead of binding as if there is a member of the form `return_type this[Index]`, the language instead assigns a target typed conversion to `int`.</span></span> 

<span data-ttu-id="b780b-231">この概念は、不可能型のすべてのメンバーアクセスに一般化されている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b780b-231">This concept could be generalized to all member access on Countable types.</span></span> <span data-ttu-id="b780b-232">`Index` 型の式がインスタンスメンバー呼び出しの引数として使用され、受信側が不可能である場合、式は `int`に変換先の型を変換します。</span><span class="sxs-lookup"><span data-stu-id="b780b-232">Whenever an expression with type `Index` is used as an argument to an instance member invocation and the receiver is Countable then the expression will have a target type conversion to `int`.</span></span> <span data-ttu-id="b780b-233">この変換に適用できるメンバー呼び出しには、メソッド、インデクサー、プロパティ、拡張メソッドなどがあります。受信者が存在しないため、コンストラクターのみが除外されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-233">The member invocations applicable for this conversion include methods, indexers, properties, extension methods, etc ... Only constructors are excluded as they have no receiver.</span></span> 

<span data-ttu-id="b780b-234">ターゲット型の変換は、`Index`の型を持つ任意の式に対して次のように実装されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-234">The target type conversion will be implemented as follows for any expression which has a type of `Index`.</span></span> <span data-ttu-id="b780b-235">説明のために、`receiver[expr]`の例を使用します。</span><span class="sxs-lookup"><span data-stu-id="b780b-235">For discussion purposes lets use the example of `receiver[expr]`:</span></span>

- <span data-ttu-id="b780b-236">`expr` が `^expr2` フォームで、`expr2` の種類が `int`場合、`receiver.Length - expr2`に変換されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-236">When `expr` is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="b780b-237">それ以外の場合は、`expr.GetOffset(receiver.Length)`として変換されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-237">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="b780b-238">`receiver` と `Length` の式は、副作用が1回だけ実行されるように、必要に応じて書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="b780b-238">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="b780b-239">例 :</span><span class="sxs-lookup"><span data-stu-id="b780b-239">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="b780b-240">このコードは、"Get Length 3" を出力します。</span><span class="sxs-lookup"><span data-stu-id="b780b-240">This code will print "Get Length 3".</span></span> 

<span data-ttu-id="b780b-241">この機能は、インデックスを表すパラメーターを持つ任意のメンバーに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="b780b-241">This feature would be beneficial to any member which had a parameter that represented an index.</span></span> <span data-ttu-id="b780b-242">たとえば、「 `List<T>.InsertAt` 」のように指定します。</span><span class="sxs-lookup"><span data-stu-id="b780b-242">For example `List<T>.InsertAt`.</span></span> <span data-ttu-id="b780b-243">また、式がインデックス作成に使用されるかどうかについてのガイダンスを言語で提供できないため、混乱が生じる可能性もあります。</span><span class="sxs-lookup"><span data-stu-id="b780b-243">This also has the potential for confusion as the language can't give any guidance as to whether or not an expression is meant for indexing.</span></span> <span data-ttu-id="b780b-244">不可能型でメンバーを呼び出すときに、すべての `Index` 式を `int` に変換することができます。</span><span class="sxs-lookup"><span data-stu-id="b780b-244">All it can do is convert any `Index` expression to `int` when invoking a member on a Countable type.</span></span> 

<span data-ttu-id="b780b-245">制限:</span><span class="sxs-lookup"><span data-stu-id="b780b-245">Restrictions:</span></span>

- <span data-ttu-id="b780b-246">この変換は、`Index` 型の式がメンバーの引数として直接指定されている場合にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="b780b-246">This conversion is only applicable when the expression with type `Index` is directly an argument to the member.</span></span> <span data-ttu-id="b780b-247">入れ子になった式には適用されません。</span><span class="sxs-lookup"><span data-stu-id="b780b-247">It would not apply to any nested expressions.</span></span>

## <a name="decisions-made-during-implementation"></a><span data-ttu-id="b780b-248">実装時に行われる決定</span><span class="sxs-lookup"><span data-stu-id="b780b-248">Decisions made during implementation</span></span>

- <span data-ttu-id="b780b-249">パターン内のすべてのメンバーはインスタンスメンバーである必要があります</span><span class="sxs-lookup"><span data-stu-id="b780b-249">All members in the pattern must be instance members</span></span>
- <span data-ttu-id="b780b-250">長さのメソッドが見つかり、戻り値の型が正しくない場合は、カウントの検索を続行します。</span><span class="sxs-lookup"><span data-stu-id="b780b-250">If a Length method is found but it has the wrong return type, continue looking for Count</span></span>
- <span data-ttu-id="b780b-251">インデックスパターンに使用されるインデクサーには、int パラメーターを1つだけ指定しなければなりません。</span><span class="sxs-lookup"><span data-stu-id="b780b-251">The indexer used for the Index pattern must have exactly one int parameter</span></span>
- <span data-ttu-id="b780b-252">範囲パターンに使用するスライスメソッドには、2つの int パラメーターを指定しなければなりません。</span><span class="sxs-lookup"><span data-stu-id="b780b-252">The Slice method used for the Range pattern must have exactly two int parameters</span></span>
- <span data-ttu-id="b780b-253">パターンメンバーを検索する場合は、構築されたメンバーではなく、元の定義を探します。</span><span class="sxs-lookup"><span data-stu-id="b780b-253">When looking for the pattern members, we look for original definitions, not constructed members</span></span>

## <a name="design-meetings"></a><span data-ttu-id="b780b-254">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="b780b-254">Design Meetings</span></span>

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a><span data-ttu-id="b780b-255">疑問がある場合</span><span class="sxs-lookup"><span data-stu-id="b780b-255">Questions</span></span>
