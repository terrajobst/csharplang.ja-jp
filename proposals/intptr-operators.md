---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483548"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a><span data-ttu-id="c34d2-101">`System.IntPtr` と `System.UIntPtr` では、演算子を公開する必要があります</span><span class="sxs-lookup"><span data-stu-id="c34d2-101">Operators should be exposed for `System.IntPtr` and `System.UIntPtr`</span></span>

* <span data-ttu-id="c34d2-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="c34d2-102">[x] Proposed</span></span>
* <span data-ttu-id="c34d2-103">[] プロトタイプ: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="c34d2-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="c34d2-104">[] の実装: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="c34d2-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="c34d2-105">[] 仕様: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="c34d2-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="c34d2-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="c34d2-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="c34d2-107">CLR では、`System.IntPtr` 型と `System.UIntPtr` 型 (`native int`) の一連の演算子がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="c34d2-107">The CLR supports a set of operators for the `System.IntPtr` and `System.UIntPtr` types (`native int`).</span></span> <span data-ttu-id="c34d2-108">これらの演算子は、共通言語基盤仕様 (`ECMA-335`) の `III.1.5` で確認できます。</span><span class="sxs-lookup"><span data-stu-id="c34d2-108">These operators can be seen in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="c34d2-109">ただし、これらの演算子はでC#はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="c34d2-109">However, these operators are not supported by C#.</span></span>

<span data-ttu-id="c34d2-110">`System.IntPtr` と `System.UIntPtr`でサポートされている演算子の完全なセットに対しては、言語サポートを提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c34d2-110">Language support should be provided for the full set of operators supported by `System.IntPtr` and `System.UIntPtr`.</span></span> <span data-ttu-id="c34d2-111">これらの演算子には、`Add`、`Divide`、`Multiply`、`Remainder`、`Subtract`、`Negate`、`Equals`、`Compare`、`And`、`Not`、`Or`、`XOr`、`ShiftLeft`、`ShiftRight`があります。</span><span class="sxs-lookup"><span data-stu-id="c34d2-111">These operators are: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span></span>

## <a name="motivation"></a><span data-ttu-id="c34d2-112">目的</span><span class="sxs-lookup"><span data-stu-id="c34d2-112">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="c34d2-113">現在、ユーザーは、さまざまC#なツールやフレームワーク (`Xamarin`、`.NET Core`、`Mono`など) を使用して、複数のプラットフォームを対象とするアプリケーションを簡単に作成できます。</span><span class="sxs-lookup"><span data-stu-id="c34d2-113">Today, users can easily write C# applications targeting multiple platforms using various tools and frameworks, such as: `Xamarin`, `.NET Core`, `Mono`, etc...</span></span>

<span data-ttu-id="c34d2-114">クロスプラットフォームコードを記述する場合は、特定の方法で特定のターゲットプラットフォームと対話する相互運用コードを記述することが必要になることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="c34d2-114">When writing cross-platform code, it is often necessary to write interop code that interacts with a particular target platform in a specific manner.</span></span> <span data-ttu-id="c34d2-115">これには、グラフィックスコードの作成、システム API の呼び出し、または既存のネイティブライブラリとの対話が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c34d2-115">This could include writing graphics code, calling some System API, or interacting with an existing native library.</span></span>

<span data-ttu-id="c34d2-116">この相互運用コードでは、多くの場合、ハンドル、アンマネージメモリ、またはプラットフォーム固有のサイズの整数だけを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c34d2-116">This interop code often has to deal with handles, unmanaged memory, or even just platform-specific sized integers.</span></span>

<span data-ttu-id="c34d2-117">ランタイムは、`native int` (`System.IntPtr`) プリミティブ型と `native unsigned int` (`System.UIntPtr`) プリミティブ型で使用できる演算子のセットを定義することで、こののサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="c34d2-117">The runtime provides support for this by defining a set of operators that can be used on the `native int` (`System.IntPtr`) and `native unsigned int` (`System.UIntPtr`) primitive types.</span></span>

<span data-ttu-id="c34d2-118">C#ではこれらの演算子がサポートされていないため、ユーザーは問題を回避する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c34d2-118">C# has never supported these operators and so users have to work around the issue.</span></span> <span data-ttu-id="c34d2-119">多くの場合、コードの複雑さが増し、コードの保守性が低下します。</span><span class="sxs-lookup"><span data-stu-id="c34d2-119">This often increases code complexity and lowers code maintainability.</span></span>

<span data-ttu-id="c34d2-120">そのため、これらの要件をより適切にサポートするために、言語を進めるためにこれらの演算子のサポートを開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c34d2-120">As such, the language should begin to support these operators to help advance the language to better support these requirements.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="c34d2-121">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="c34d2-121">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="c34d2-122">サポートされている演算子の完全なセットは、共通言語基盤仕様 (`ECMA-335`) の `III.1.5` で定義されています。</span><span class="sxs-lookup"><span data-stu-id="c34d2-122">The full set of operators supported are defined in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="c34d2-123">この仕様については、「 [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c34d2-123">The specification is available here: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span></span>

* <span data-ttu-id="c34d2-124">これらの演算子の概要については、以下で簡単に説明します。</span><span class="sxs-lookup"><span data-stu-id="c34d2-124">A summary of the operators is provided below for convenience.</span></span>
* <span data-ttu-id="c34d2-125">CLI 仕様で定義されている検証不可能な演算子は記載されていないため、現在この提案に含まれていません (ただし、これらも考慮する価値があります)。</span><span class="sxs-lookup"><span data-stu-id="c34d2-125">The unverifiable operators defined by the CLI spec are not listed and are not currently part of this proposal (although it may be worth considering these as well).</span></span>
* <span data-ttu-id="c34d2-126">キーワード (`nint` や `nuint`など) を指定したり、`System.IntPtr` と `System.UIntPtr` (0n など) に対してリテラルを宣言する方法を提供したりすることは、この提案には含まれていません (ただし、これらも考慮する価値があります)。</span><span class="sxs-lookup"><span data-stu-id="c34d2-126">Providing a keyword (such as `nint` and `nuint`) nor providing a way to for literals to be declared for `System.IntPtr` and `System.UIntPtr` (such as 0n) is not part of this proposal (although it may be worth considering these as well).</span></span>

### <a name="unary-plus-operator"></a><span data-ttu-id="c34d2-127">単項プラス演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-127">Unary Plus Operator</span></span>

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a><span data-ttu-id="c34d2-128">単項マイナス演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-128">Unary Minus Operator</span></span>

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a><span data-ttu-id="c34d2-129">ビットごとの補数演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-129">Bitwise Complement Operator</span></span>

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a><span data-ttu-id="c34d2-130">キャスト演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-130">Cast Operators</span></span>

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a><span data-ttu-id="c34d2-131">乗算演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-131">Multiplication Operator</span></span>

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a><span data-ttu-id="c34d2-132">除算演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-132">Division Operator</span></span>

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a><span data-ttu-id="c34d2-133">剰余演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-133">Remainder Operator</span></span>

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a><span data-ttu-id="c34d2-134">加算演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-134">Addition Operator</span></span>

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a><span data-ttu-id="c34d2-135">減算演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-135">Subtraction Operator</span></span>

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a><span data-ttu-id="c34d2-136">シフト演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-136">Shift Operators</span></span>

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a><span data-ttu-id="c34d2-137">整数の比較演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-137">Integer Comparison Operators</span></span>

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a><span data-ttu-id="c34d2-138">整数の論理演算子</span><span class="sxs-lookup"><span data-stu-id="c34d2-138">Integer Logical Operators</span></span>

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a><span data-ttu-id="c34d2-139">短所</span><span class="sxs-lookup"><span data-stu-id="c34d2-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="c34d2-140">これらの演算子の実際の使用は、下位レベルのライブラリまたは相互運用コードを記述しているエンドユーザーに対しては、小規模である場合があります。</span><span class="sxs-lookup"><span data-stu-id="c34d2-140">The actual use of these operators may be small and limited to end-users who are writing lower level libraries or interop code.</span></span> <span data-ttu-id="c34d2-141">ほとんどのエンドユーザーは、ネイティブサイズの整数、ハンドル、および相互運用コードが抽象化されている下位レベルのライブラリ自体を使用している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c34d2-141">Most end-users would likely be consuming these lower level libraries themselves which would have the native sized integers, handles, and interop code abstracted away.</span></span> <span data-ttu-id="c34d2-142">そのため、オペレーター自体が必要になることはありません。</span><span class="sxs-lookup"><span data-stu-id="c34d2-142">As such, they would not have need of the operators themselves.</span></span>

## <a name="alternatives"></a><span data-ttu-id="c34d2-143">代替</span><span class="sxs-lookup"><span data-stu-id="c34d2-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="c34d2-144">必要な演算子を実装している場合は、直接 IL に記述します。</span><span class="sxs-lookup"><span data-stu-id="c34d2-144">Have the framework implement the required operators by writing them directly in IL.</span></span> <span data-ttu-id="c34d2-145">さらに、ランタイムは、フレームワークによって定義された演算子の組み込みサポートを提供するため、最終的なパフォーマンスをより適切に最適化することができます。</span><span class="sxs-lookup"><span data-stu-id="c34d2-145">Additionally, the runtime could provide intrinsic support for the operators defined by the framework, so as to better optimize the end performance.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="c34d2-146">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="c34d2-146">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="c34d2-147">設計のどの部分も未定ですか。</span><span class="sxs-lookup"><span data-stu-id="c34d2-147">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="c34d2-148">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="c34d2-148">Design meetings</span></span>

<span data-ttu-id="c34d2-149">この提案に影響を与えるデザインメモにリンクし、それぞれの変更内容について1つの文で説明します。</span><span class="sxs-lookup"><span data-stu-id="c34d2-149">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>