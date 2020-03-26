---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281971"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="dbe2e-101">ターゲット型の `new` 式</span><span class="sxs-lookup"><span data-stu-id="dbe2e-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="dbe2e-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="dbe2e-102">[x] Proposed</span></span>
* <span data-ttu-id="dbe2e-103">[x][プロトタイプ](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="dbe2e-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="dbe2e-104">[] の実装</span><span class="sxs-lookup"><span data-stu-id="dbe2e-104">[ ] Implementation</span></span>
* <span data-ttu-id="dbe2e-105">[] 仕様</span><span class="sxs-lookup"><span data-stu-id="dbe2e-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="dbe2e-106">要約</span><span class="sxs-lookup"><span data-stu-id="dbe2e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="dbe2e-107">型がわかっている場合は、コンストラクターの型指定を必要としません。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="dbe2e-108">目的</span><span class="sxs-lookup"><span data-stu-id="dbe2e-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="dbe2e-109">型を複製せずにフィールドの初期化を許可します。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="dbe2e-110">使用法から推論できる場合は、型の省略を許可します。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="dbe2e-111">型のスペルを修正せずにオブジェクトをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="specification"></a><span data-ttu-id="dbe2e-112">仕様</span><span class="sxs-lookup"><span data-stu-id="dbe2e-112">Specification</span></span>
[design]: #detailed-design

<span data-ttu-id="dbe2e-113">*Object_creation_expression*の*target_typed_new*新しい構文形式が受け入れられます。この*型*は省略可能です。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-113">A new syntactic form, *target_typed_new* of the *object_creation_expression* is accepted in which the *type* is optional.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

<span data-ttu-id="dbe2e-114">*Target_typed_new*式に型がありません。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-114">A *target_typed_new* expression does not have a type.</span></span> <span data-ttu-id="dbe2e-115">ただし、新しい*オブジェクト作成変換*は、 *target_typed_new*からすべての型に存在する式からの暗黙的な変換です。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-115">However, there is a new *object creation conversion* that is an implicit conversion from expression, that exists from a *target_typed_new* to every type.</span></span>

<span data-ttu-id="dbe2e-116">`T`対象の型を指定すると、`T` が `System.Nullable`のインスタンスである場合、型 `T0` は `T`基になる型になります。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-116">Given a target type `T`, the type `T0` is `T`'s underlying type if `T` is an instance of `System.Nullable`.</span></span> <span data-ttu-id="dbe2e-117">それ以外の場合は `T0` が `T`ます。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-117">Otherwise `T0` is `T`.</span></span> <span data-ttu-id="dbe2e-118">型 `T` に変換される*target_typed_new*式の意味は、型として `T0` を指定する、対応する*object_creation_expression*の意味と同じです。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-118">The meaning of a *target_typed_new* expression that is converted to the type `T` is the same as the meaning of a corresponding *object_creation_expression* that specifies `T0` as the type.</span></span>

<span data-ttu-id="dbe2e-119">単項演算子または二項演算子のオペランドとして*target_typed_new*が使用されている場合、または*オブジェクトの作成変換*の対象とならない場所で使用されている場合は、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-119">It is a compile-time error if a *target_typed_new* is used as an operand of a unary or binary operator, or if it is used where it is not subject to an *object creation conversion*.</span></span>

> <span data-ttu-id="dbe2e-120">**懸案事項を開く:** デリゲートとタプルをターゲットタイプとして許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-120">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="dbe2e-121">上記の規則には、デリゲート (参照型) と組 (構造体型) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-121">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="dbe2e-122">どちらの型も構築可能ですが、型が inferable の場合は、匿名関数またはタプルリテラルを使用できます。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-122">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="dbe2e-123">その他</span><span class="sxs-lookup"><span data-stu-id="dbe2e-123">Miscellaneous</span></span>

<span data-ttu-id="dbe2e-124">仕様の結果は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-124">The following are consequences of the specification:</span></span>

- <span data-ttu-id="dbe2e-125">`throw new()` が許可されています (ターゲットの種類は `System.Exception`)</span><span class="sxs-lookup"><span data-stu-id="dbe2e-125">`throw new()` is allowed (the target type is `System.Exception`)</span></span>
- <span data-ttu-id="dbe2e-126">ターゲット型の `new` は、バイナリ演算子では使用できません。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-126">Target-typed `new` is not allowed with binary operators.</span></span>
- <span data-ttu-id="dbe2e-127">対象となる型がない場合は、単項演算子、`foreach`のコレクションです。 `using`の場合、`await` 式では、匿名型のプロパティ (`new { Prop = new() }`)、`lock` ステートメント、`sizeof`演算子のオペランドとして、LINQ クエリ内では、`fixed` 演算子の左オペランドとして、動的にディスパッチされる操作 (`new().field`) において、ステートメントを使用して、ステートメントに含まれます。,  ...`someDynamic.Method(new())``is``??`</span><span class="sxs-lookup"><span data-stu-id="dbe2e-127">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>
- <span data-ttu-id="dbe2e-128">また、`ref`として許可されていません。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-128">It is also disallowed as a `ref`.</span></span>
- <span data-ttu-id="dbe2e-129">次の種類の型は、変換のターゲットとして許可されていません。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-129">The following kinds of types are not permitted as targets of the conversion</span></span>
  - <span data-ttu-id="dbe2e-130">**列挙型:** `new()` は動作します (`new Enum()` が既定値を提供するために動作します) が、列挙型にコンストラクターがないため `new(1)` は機能しません。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-130">**Enum types:** `new()` will work (as `new Enum()` works to give the default value), but `new(1)` will not work as enum types do not have a constructor.</span></span>
  - <span data-ttu-id="dbe2e-131">**インターフェイスの種類:** これは、COM 型の対応する作成式と同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-131">**Interface types:** This would work the same as the corresponding creation expression for COM types.</span></span>
  - <span data-ttu-id="dbe2e-132">**配列型:** 配列の長さを指定するには、特別な構文が必要です。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-132">**Array types:** arrays need a special syntax to provide the length.</span></span>    
  - <span data-ttu-id="dbe2e-133">**動的:** `new dynamic()`は許可されていないため、`dynamic` を対象の型として `new()` することはできません。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-133">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>
  - <span data-ttu-id="dbe2e-134">**タプル:** これらは、基になる型を使用したオブジェクトの作成と同じ意味を持ちます。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-134">**tuples:** These have the same meaning as an object creation using the underlying type.</span></span>
  - <span data-ttu-id="dbe2e-135">*Object_creation_expression*で許可されていないその他のすべての型も、ポインター型など、除外されます。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-135">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>   

## <a name="drawbacks"></a><span data-ttu-id="dbe2e-136">短所</span><span class="sxs-lookup"><span data-stu-id="dbe2e-136">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="dbe2e-137">互換性に影響する変更の新しいカテゴリを作成するために、ターゲット型の `new` にはいくつかの問題がありましたが、既に `null` と `default`を使用していますが、これは重大な問題ではありませんでした。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-137">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="dbe2e-138">代替</span><span class="sxs-lookup"><span data-stu-id="dbe2e-138">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="dbe2e-139">フィールドの初期化時に型引数が長すぎることに関する苦情のほとんどは、型自体で*はない型*引数に関するものであり、`new Dictionary(...)` (または同様) のような型引数だけを推論し、引数またはコレクション初期化子からローカルに型引数を推論することもできます。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-139">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="dbe2e-140">疑問がある場合</span><span class="sxs-lookup"><span data-stu-id="dbe2e-140">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="dbe2e-141">式ツリーで使用できないようにする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-141">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="dbe2e-142">番号</span><span class="sxs-lookup"><span data-stu-id="dbe2e-142">(no)</span></span>
- <span data-ttu-id="dbe2e-143">機能が `dynamic` の引数とどのように連動するか。</span><span class="sxs-lookup"><span data-stu-id="dbe2e-143">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="dbe2e-144">(特別な処理はありません)</span><span class="sxs-lookup"><span data-stu-id="dbe2e-144">(no special treatment)</span></span>
- <span data-ttu-id="dbe2e-145">IntelliSense での `new()`の使用方法</span><span class="sxs-lookup"><span data-stu-id="dbe2e-145">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="dbe2e-146">(1 つのターゲット型がある場合のみ)</span><span class="sxs-lookup"><span data-stu-id="dbe2e-146">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="dbe2e-147">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="dbe2e-147">Design meetings</span></span>

- [<span data-ttu-id="dbe2e-148">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="dbe2e-148">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="dbe2e-149">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="dbe2e-149">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="dbe2e-150">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="dbe2e-150">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="dbe2e-151">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="dbe2e-151">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="dbe2e-152">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="dbe2e-152">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [<span data-ttu-id="dbe2e-153">LDM-2020-03-25</span><span class="sxs-lookup"><span data-stu-id="dbe2e-153">LDM-2020-03-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
