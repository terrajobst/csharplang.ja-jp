---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108371"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="54fc5-101">ターゲット型の `new` 式</span><span class="sxs-lookup"><span data-stu-id="54fc5-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="54fc5-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="54fc5-102">[x] Proposed</span></span>
* <span data-ttu-id="54fc5-103">[x][プロトタイプ](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="54fc5-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="54fc5-104">[] の実装</span><span class="sxs-lookup"><span data-stu-id="54fc5-104">[ ] Implementation</span></span>
* <span data-ttu-id="54fc5-105">[] 仕様</span><span class="sxs-lookup"><span data-stu-id="54fc5-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="54fc5-106">要約</span><span class="sxs-lookup"><span data-stu-id="54fc5-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="54fc5-107">型がわかっている場合は、コンストラクターの型指定を必要としません。</span><span class="sxs-lookup"><span data-stu-id="54fc5-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="54fc5-108">目的</span><span class="sxs-lookup"><span data-stu-id="54fc5-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="54fc5-109">型を複製せずにフィールドの初期化を許可します。</span><span class="sxs-lookup"><span data-stu-id="54fc5-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="54fc5-110">使用法から推論できる場合は、型の省略を許可します。</span><span class="sxs-lookup"><span data-stu-id="54fc5-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="54fc5-111">型のスペルを修正せずにオブジェクトをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="54fc5-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="54fc5-112">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="54fc5-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="54fc5-113">*Object_creation_expression*構文は、かっこが存在する場合に*型*を省略可能にするように変更されます。</span><span class="sxs-lookup"><span data-stu-id="54fc5-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="54fc5-114">これは、 *anonymous_object_creation_expression*のあいまいさを解決するために必要です。</span><span class="sxs-lookup"><span data-stu-id="54fc5-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="54fc5-115">ターゲット型の `new` は、任意の型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="54fc5-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="54fc5-116">そのため、オーバーロードの解決には関与しません。</span><span class="sxs-lookup"><span data-stu-id="54fc5-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="54fc5-117">これは主に、予期しない重大な変更を回避するためのものです。</span><span class="sxs-lookup"><span data-stu-id="54fc5-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="54fc5-118">引数リストと初期化子式は、型が決定された後にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="54fc5-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="54fc5-119">式の型は、次のいずれかである必要があるターゲット型から推論されます。</span><span class="sxs-lookup"><span data-stu-id="54fc5-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="54fc5-120">**任意の構造体型**(タプル型を含む)</span><span class="sxs-lookup"><span data-stu-id="54fc5-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="54fc5-121">**任意の参照型**(デリゲート型を含む)</span><span class="sxs-lookup"><span data-stu-id="54fc5-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="54fc5-122">コンストラクターまたは `struct` 制約を持つ**任意の型パラメーター**</span><span class="sxs-lookup"><span data-stu-id="54fc5-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="54fc5-123">次の例外があります。</span><span class="sxs-lookup"><span data-stu-id="54fc5-123">with the following exceptions:</span></span>

- <span data-ttu-id="54fc5-124">**列挙型:** すべての列挙型に定数ゼロが含まれていないため、明示的な enum メンバーを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="54fc5-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="54fc5-125">**インターフェイス型:** これはニッチ機能であり、型を明示的に言及することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="54fc5-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="54fc5-126">**配列型:** 配列の長さを指定するには、特別な構文が必要です。</span><span class="sxs-lookup"><span data-stu-id="54fc5-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="54fc5-127">**動的:** `new dynamic()`は許可されていないため、`dynamic` を対象の型として `new()` することはできません。</span><span class="sxs-lookup"><span data-stu-id="54fc5-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="54fc5-128">*Object_creation_expression*で許可されていないその他のすべての型も、ポインター型など、除外されます。</span><span class="sxs-lookup"><span data-stu-id="54fc5-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="54fc5-129">対象の型が null 許容の値型である場合、対象の型指定された `new` は、null 許容型ではなく、基になる型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="54fc5-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="54fc5-130">**懸案事項を開く:** デリゲートとタプルをターゲットタイプとして許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="54fc5-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="54fc5-131">上記の規則には、デリゲート (参照型) と組 (構造体型) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="54fc5-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="54fc5-132">どちらの型も構築可能ですが、型が inferable の場合は、匿名関数またはタプルリテラルを使用できます。</span><span class="sxs-lookup"><span data-stu-id="54fc5-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
