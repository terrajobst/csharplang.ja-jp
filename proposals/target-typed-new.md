---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483530"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="4b934-101">ターゲット型の `new` 式</span><span class="sxs-lookup"><span data-stu-id="4b934-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="4b934-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="4b934-102">[x] Proposed</span></span>
* <span data-ttu-id="4b934-103">[x][プロトタイプ](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="4b934-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="4b934-104">[] の実装</span><span class="sxs-lookup"><span data-stu-id="4b934-104">[ ] Implementation</span></span>
* <span data-ttu-id="4b934-105">[] 仕様</span><span class="sxs-lookup"><span data-stu-id="4b934-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="4b934-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="4b934-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4b934-107">型がわかっている場合は、コンストラクターの型指定を必要としません。</span><span class="sxs-lookup"><span data-stu-id="4b934-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="4b934-108">目的</span><span class="sxs-lookup"><span data-stu-id="4b934-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="4b934-109">型を複製せずにフィールドの初期化を許可します。</span><span class="sxs-lookup"><span data-stu-id="4b934-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
<span data-ttu-id="4b934-110">使用法から推論できる場合は、型の省略を許可します。</span><span class="sxs-lookup"><span data-stu-id="4b934-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
<span data-ttu-id="4b934-111">型のスペルを修正せずにオブジェクトをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="4b934-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a><span data-ttu-id="4b934-112">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="4b934-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="4b934-113">*Object_creation_expression*構文は、かっこが存在する場合に*型*を省略可能にするように変更されます。</span><span class="sxs-lookup"><span data-stu-id="4b934-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="4b934-114">これは、 *anonymous_object_creation_expression*のあいまいさを解決するために必要です。</span><span class="sxs-lookup"><span data-stu-id="4b934-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
<span data-ttu-id="4b934-115">ターゲット型の `new` は、任意の型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="4b934-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="4b934-116">そのため、オーバーロードの解決には関与しません。</span><span class="sxs-lookup"><span data-stu-id="4b934-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="4b934-117">これは主に、予期しない重大な変更を回避するためのものです。</span><span class="sxs-lookup"><span data-stu-id="4b934-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="4b934-118">引数リストと初期化子式は、型が決定された後にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="4b934-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="4b934-119">式の型は、次のいずれかである必要があるターゲット型から推論されます。</span><span class="sxs-lookup"><span data-stu-id="4b934-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="4b934-120">**任意の構造体型**</span><span class="sxs-lookup"><span data-stu-id="4b934-120">**Any struct type**</span></span>
- <span data-ttu-id="4b934-121">**任意の参照型**</span><span class="sxs-lookup"><span data-stu-id="4b934-121">**Any reference type**</span></span>
- <span data-ttu-id="4b934-122">コンストラクターまたは `struct` 制約を持つ**任意の型パラメーター**</span><span class="sxs-lookup"><span data-stu-id="4b934-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="4b934-123">次の例外があります。</span><span class="sxs-lookup"><span data-stu-id="4b934-123">with the following exceptions:</span></span>

- <span data-ttu-id="4b934-124">**列挙型:** すべての列挙型に定数ゼロが含まれていないため、明示的な enum メンバーを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="4b934-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="4b934-125">**インターフェイス型:** これはニッチ機能であり、型を明示的に言及することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="4b934-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="4b934-126">**配列型:** 配列の長さを指定するには、特別な構文が必要です。</span><span class="sxs-lookup"><span data-stu-id="4b934-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="4b934-127">**構造体の既定のコンストラクター**: この規則により、すべてのプリミティブ型とほとんどの値の型が除外されます。</span><span class="sxs-lookup"><span data-stu-id="4b934-127">**Struct default constructor**: this rules out all primitive types and most value types.</span></span> <span data-ttu-id="4b934-128">このような型の既定値を使用する場合は、代わりに `default` を記述できます。</span><span class="sxs-lookup"><span data-stu-id="4b934-128">If you wanted to use the default value of such types you could write `default` instead.</span></span>

<span data-ttu-id="4b934-129">*Object_creation_expression*で許可されていないその他のすべての型も、ポインター型など、除外されます。</span><span class="sxs-lookup"><span data-stu-id="4b934-129">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

> <span data-ttu-id="4b934-130">**懸案事項を開く:** デリゲートとタプルをターゲットタイプとして許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="4b934-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="4b934-131">上記の規則には、デリゲート (参照型) と組 (構造体型) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="4b934-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="4b934-132">どちらの型も構築可能ですが、型が inferable の場合は、匿名関数またはタプルリテラルを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4b934-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> <span data-ttu-id="4b934-133">**懸案事項を開く:** `Exception` をターゲットの種類として `throw new()` できるようにする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="4b934-133">**Open Issue:** should we allow `throw new()` with `Exception` as the target-type?</span></span>

<span data-ttu-id="4b934-134">現在 `throw null` していますが、`throw default` はありません (ただし、同じ効果があります)。</span><span class="sxs-lookup"><span data-stu-id="4b934-134">We have `throw null` today, but not `throw default` (though it would have the same effect).</span></span> <span data-ttu-id="4b934-135">一方、`throw new()` は、`throw new Exception(...)`の短縮形として実際に役立つ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4b934-135">On the other hand, `throw new()` could be actually useful as a shorthand for `throw new Exception(...)`.</span></span> <span data-ttu-id="4b934-136">現在の仕様によって既に許可されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4b934-136">Note that it is already allowed by the current specification.</span></span> <span data-ttu-id="4b934-137">`Exception` は参照型であり、throw ステートメントの仕様では、式が `Exception`に変換されることを示します。</span><span class="sxs-lookup"><span data-stu-id="4b934-137">`Exception` is a reference type, and the specification for the throw statement says that the expression is converted to `Exception`.</span></span>

> <span data-ttu-id="4b934-138">**懸案事項を開く:** ユーザー定義の比較演算子と算術演算子を使用して、ターゲット型の `new` の使用を許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="4b934-138">**Open Issue:** should we allow usages of a target-typed `new` with user-defined comparison and arithmetic operators?</span></span>

<span data-ttu-id="4b934-139">比較のために、`default` でサポートされるのは、等値 (ユーザー定義および組み込み) 演算子だけです。</span><span class="sxs-lookup"><span data-stu-id="4b934-139">For comparison, `default` only supports equality (user-defined and built-in) operators.</span></span> <span data-ttu-id="4b934-140">`new()` のために他の演算子をサポートすることは理にかなっていますか。</span><span class="sxs-lookup"><span data-stu-id="4b934-140">Would it make sense to support other operators for `new()` as well?</span></span>

## <a name="drawbacks"></a><span data-ttu-id="4b934-141">短所</span><span class="sxs-lookup"><span data-stu-id="4b934-141">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="4b934-142">[なし] :</span><span class="sxs-lookup"><span data-stu-id="4b934-142">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="4b934-143">代替</span><span class="sxs-lookup"><span data-stu-id="4b934-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="4b934-144">フィールドの初期化時に型引数が長すぎることに関する苦情のほとんどは、型自体で*はない型*引数に関するものであり、`new Dictionary(...)` (または同様) のような型引数だけを推論し、引数またはコレクション初期化子からローカルに型引数を推論することもできます。</span><span class="sxs-lookup"><span data-stu-id="4b934-144">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="4b934-145">疑問がある場合</span><span class="sxs-lookup"><span data-stu-id="4b934-145">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="4b934-146">式ツリーで使用できないようにする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="4b934-146">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="4b934-147">番号</span><span class="sxs-lookup"><span data-stu-id="4b934-147">(no)</span></span>
- <span data-ttu-id="4b934-148">機能が `dynamic` の引数とどのように連動するか。</span><span class="sxs-lookup"><span data-stu-id="4b934-148">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="4b934-149">(特別な処理はありません)</span><span class="sxs-lookup"><span data-stu-id="4b934-149">(no special treatment)</span></span>
- <span data-ttu-id="4b934-150">IntelliSense での `new()`の使用方法</span><span class="sxs-lookup"><span data-stu-id="4b934-150">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="4b934-151">(1 つのターゲット型がある場合のみ)</span><span class="sxs-lookup"><span data-stu-id="4b934-151">(only when there is a single target-type)</span></span>
## <a name="design-meetings"></a><span data-ttu-id="4b934-152">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="4b934-152">Design meetings</span></span>

- [<span data-ttu-id="4b934-153">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="4b934-153">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
