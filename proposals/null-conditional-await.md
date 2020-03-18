---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483494"
---
# <a name="null-conditional-await"></a><span data-ttu-id="d84ec-101">null 条件 await</span><span class="sxs-lookup"><span data-stu-id="d84ec-101">null-conditional await</span></span>

* <span data-ttu-id="d84ec-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="d84ec-102">[x] Proposed</span></span>
* <span data-ttu-id="d84ec-103">[] プロトタイプ: なし</span><span class="sxs-lookup"><span data-stu-id="d84ec-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="d84ec-104">[] の実装: なし</span><span class="sxs-lookup"><span data-stu-id="d84ec-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="d84ec-105">[] 仕様: 開始しました。以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d84ec-105">[ ] Specification: Started, below</span></span>

## <a name="summary"></a><span data-ttu-id="d84ec-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="d84ec-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d84ec-107">`await? e`形式の式をサポートします。これは、null 以外の場合に `e` を待機します。それ以外の場合は `null`になります。</span><span class="sxs-lookup"><span data-stu-id="d84ec-107">Support an expression of the form `await? e`, which awaits `e` if it is non-null, otherwise it results in `null`.</span></span>

## <a name="motivation"></a><span data-ttu-id="d84ec-108">目的</span><span class="sxs-lookup"><span data-stu-id="d84ec-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d84ec-109">これは一般的なコーディングパターンであり、この機能は、既存の null を反映する演算子と null 合体演算子によって優れた相乗効果が得られます。</span><span class="sxs-lookup"><span data-stu-id="d84ec-109">This is a common coding pattern, and this feature would have nice synergy with the existing null-propagating and null-coalescing operators.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d84ec-110">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="d84ec-110">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d84ec-111">*Await_expression*の新しいフォームを追加します。</span><span class="sxs-lookup"><span data-stu-id="d84ec-111">We add a new form of the *await_expression*:</span></span>

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

<span data-ttu-id="d84ec-112">Null 条件 `await` 演算子は、そのオペランドが null 以外の場合にのみオペランドを待機します。</span><span class="sxs-lookup"><span data-stu-id="d84ec-112">The null-conditional `await` operator awaits its operand only if that operand is non-null.</span></span> <span data-ttu-id="d84ec-113">それ以外の場合は、演算子を適用した結果が null になります。</span><span class="sxs-lookup"><span data-stu-id="d84ec-113">Otherwise the result of applying the operator is null.</span></span>

<span data-ttu-id="d84ec-114">結果の型は、 [null 条件演算子のルール](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator)を使用して計算されます。</span><span class="sxs-lookup"><span data-stu-id="d84ec-114">The type of the result is computed using the [rules for the null-conditional operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span></span>

> <span data-ttu-id="d84ec-115">**注:** `e` が `Task`型の場合、`await? e;` は `e` が `null`されている場合は何も行いません。 `e` ない場合は、`null`待機します。</span><span class="sxs-lookup"><span data-stu-id="d84ec-115">**NOTE:** If `e` is of type `Task`, then `await? e;` would do nothing if `e` is `null`, and await `e` if it is not `null`.</span></span>
>
> <span data-ttu-id="d84ec-116">`e` が `Task<K>` 型の場合、`K` は値型であるため、`await? e` は `K?`型の値を生成します。</span><span class="sxs-lookup"><span data-stu-id="d84ec-116">If `e` is of type `Task<K>` where `K` is a value type, then `await? e` would yield a value of type `K?`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="d84ec-117">短所</span><span class="sxs-lookup"><span data-stu-id="d84ec-117">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d84ec-118">他の言語機能と同様に、この機能の恩恵を受けるプログラムのC#本文に対して、さらに複雑な言語を端的にする必要があるかどうかを質問する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d84ec-118">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d84ec-119">代替</span><span class="sxs-lookup"><span data-stu-id="d84ec-119">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d84ec-120">いくつかの定型コードが必要ですが、この演算子の使用は、多くの場合、`(e == null) ? null : await e` や `if (e != null) await e`のようなステートメントに置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="d84ec-120">Although it requires some boilerplate code, uses of this operator can often be replaced by an expression something like `(e == null) ? null : await e` or a statement like `if (e != null) await e`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d84ec-121">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="d84ec-121">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="d84ec-122">[] には LDM レビューが必要です</span><span class="sxs-lookup"><span data-stu-id="d84ec-122">[ ] Requires LDM review</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d84ec-123">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="d84ec-123">Design meetings</span></span>

<span data-ttu-id="d84ec-124">[なし] :</span><span class="sxs-lookup"><span data-stu-id="d84ec-124">None.</span></span>
