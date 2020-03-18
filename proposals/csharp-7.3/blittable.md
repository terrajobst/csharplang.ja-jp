---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483602"
---
# <a name="unmanaged-type-constraint"></a><span data-ttu-id="9b1ce-101">アンマネージ型制約</span><span class="sxs-lookup"><span data-stu-id="9b1ce-101">Unmanaged type constraint</span></span>

## <a name="summary"></a><span data-ttu-id="9b1ce-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="9b1ce-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="9b1ce-103">アンマネージ制約機能は、言語仕様の "アンマネージ型" C#と呼ばれる型のクラスに言語の適用を提供します。 これは、参照型ではなく、入れ子の任意のレベルの参照型フィールドを含まない型として、セクション18.2 で定義されています。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-103">The unmanaged constraint feature will give language enforcement to the class of types known as "unmanaged types" in the C# language spec.  This is defined in section 18.2 as a type which is not a reference type and doesn't contain reference type fields at any level of nesting.</span></span>  

## <a name="motivation"></a><span data-ttu-id="9b1ce-104">目的</span><span class="sxs-lookup"><span data-stu-id="9b1ce-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="9b1ce-105">主な動機は、で低レベルのC#相互運用コードを簡単に作成できるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-105">The primary motivation is to make it easier to author low level interop code in C#.</span></span> <span data-ttu-id="9b1ce-106">アンマネージ型は相互運用コードの中核となる構成要素の1つですが、ジェネリックのサポートがないため、すべてのアンマネージ型で再利用可能なルーチンを作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-106">Unmanaged types are one of the core building blocks for interop code, yet the lack of support in generics makes it impossible to create re-usable routines across all unmanaged types.</span></span> <span data-ttu-id="9b1ce-107">代わりに、開発者はライブラリ内のすべてのアンマネージ型に対して同じボイラープレートコードを作成することが強制されます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-107">Instead developers are forced to author the same boiler plate code for every unmanaged type in their library:</span></span>

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

<span data-ttu-id="9b1ce-108">この種類のシナリオを有効にするには、言語に新しい制約を導入します。アンマネージ:</span><span class="sxs-lookup"><span data-stu-id="9b1ce-108">To enable this type of scenario the language will be introducing a new constraint: unmanaged:</span></span>

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

<span data-ttu-id="9b1ce-109">この制約は、 C#言語仕様のアンマネージ型定義に適合する型によってのみ満たすことができます。これを調べる別の方法として、型がアンマネージ制約を満たしていて、ポインターとして使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-109">This constraint can only be met by types which fit into the unmanaged type definition in the C# language spec. Another way of looking at it is that a type satisfies the unmanaged constraint iff it can also be used as a pointer.</span></span> 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

<span data-ttu-id="9b1ce-110">アンマネージ制約がある型パラメーターは、アンマネージ型に使用できるすべての機能 (ポインター、固定など) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-110">Type parameters with the unmanaged constraint can use all the features available to unmanaged types: pointers, fixed, etc ...</span></span> 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

<span data-ttu-id="9b1ce-111">この制約により、構造化データとバイトストリームの間で効率的な変換を行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-111">This constraint will also make it possible to have efficient conversions between structured data and streams of bytes.</span></span> <span data-ttu-id="9b1ce-112">これは、ネットワークスタックとシリアル化層で一般的な操作です。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-112">This is an operation that is common in networking stacks and serialization layers:</span></span>

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

<span data-ttu-id="9b1ce-113">このようなルーチンは、コンパイル時には安全であり、割り当てが provably されるため、便利です。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-113">Such routines are advantageous because they are provably safe at compile time and allocation free.</span></span>  <span data-ttu-id="9b1ce-114">現在、相互運用の作成者はこれを行うことはできません (パフォーマンスが重要なレイヤーにある場合でも)。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-114">Interop authors today can not do this (even though it's at a layer where perf is critical).</span></span>  <span data-ttu-id="9b1ce-115">代わりに、値が正しく管理されていないことを確認するために、高コストのランタイムチェックを含むルーチンの割り当てに依存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-115">Instead they need to rely on allocating routines that have expensive runtime checks to verify values are correctly unmanaged.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="9b1ce-116">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="9b1ce-116">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="9b1ce-117">この言語では、`unmanaged`という名前の新しい制約が導入されます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-117">The language will introduce a new constraint named `unmanaged`.</span></span> <span data-ttu-id="9b1ce-118">この制約を満たすためには、型は構造体である必要があり、型のすべてのフィールドは次のいずれかのカテゴリに分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-118">In order to satisfy this constraint a type must be a struct and all the fields of the type must fall into one of the following categories:</span></span>

- <span data-ttu-id="9b1ce-119">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`の種類があります。`bool``IntPtr``UIntPtr`</span><span class="sxs-lookup"><span data-stu-id="9b1ce-119">Have the type `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` or `UIntPtr`.</span></span>
- <span data-ttu-id="9b1ce-120">任意の `enum` の種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-120">Be any `enum` type.</span></span>
- <span data-ttu-id="9b1ce-121">ポインター型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-121">Be a pointer type.</span></span>
- <span data-ttu-id="9b1ce-122">`unmanaged` 制約を satsifies ユーザー定義構造体であること。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-122">Be a user defined struct that satsifies the `unmanaged` constraint.</span></span>

<span data-ttu-id="9b1ce-123">自動実装されたプロパティなど、コンパイラによって生成されるインスタンスフィールドも、これらの制約を満たしている必要があります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-123">Compiler generated instance fields, such as those backing auto-implemented properties, must also meet these constraints.</span></span> 

<span data-ttu-id="9b1ce-124">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-124">For example:</span></span>

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

<span data-ttu-id="9b1ce-125">`unmanaged` 制約は、`struct`、`class`、または `new()`と組み合わせることはできません。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-125">The `unmanaged` constraint cannot be combined with `struct`, `class` or `new()`.</span></span> <span data-ttu-id="9b1ce-126">この制限は、`unmanaged` が `struct` を意味するため、その他の制約は意味を持たないからです。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-126">This restriction derives from the fact that `unmanaged` implies `struct` hence the other constraints do not make sense.</span></span>

<span data-ttu-id="9b1ce-127">`unmanaged` 制約は、CLR によって適用されるのではなく、言語によってのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-127">The `unmanaged` constraint is not enforced by CLR, only by the language.</span></span> <span data-ttu-id="9b1ce-128">他の言語による mis の使用を防ぐために、この制約を持つメソッドは mod 要求によって保護されます。これにより、アンマネージ型ではない型引数が他の言語で使用されるのを防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-128">To prevent mis-use by other languages, methods which have this constraint will be protected by a mod-req. This will prevent other languages from using type arguments which are not unmanaged types.</span></span>

<span data-ttu-id="9b1ce-129">制約内の `unmanaged` トークンはキーワードでもコンテキストキーワードでもありません。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-129">The token `unmanaged` in the constraint is not a keyword, nor a contextual keyword.</span></span> <span data-ttu-id="9b1ce-130">代わりに、その場所で評価され、次のいずれかの方法で `var` のようになります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-130">Instead it is like `var` in that it is evaluated at that location and will either:</span></span>

- <span data-ttu-id="9b1ce-131">`unmanaged`という名前のユーザー定義または参照型にバインドします。これは、他の名前付きの型制約が処理されるのと同じように扱われます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-131">Bind to user defined or referenced type named `unmanaged`: This will be treated just as any other named type constraint is treated.</span></span> 
- <span data-ttu-id="9b1ce-132">No type にバインドする: これは、`unmanaged` 制約として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-132">Bind to no type: This will be interpreted as the `unmanaged` constraint.</span></span>

<span data-ttu-id="9b1ce-133">`unmanaged` という名前の型があり、現在のコンテキストに限定せずに使用できる場合、`unmanaged` 制約を使用する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-133">In the case there is a type named `unmanaged` and it is available without qualification in the current context, then there will be no way to use the `unmanaged` constraint.</span></span> <span data-ttu-id="9b1ce-134">これは、機能 `var` を囲む規則と、同じ名前のユーザー定義型に似ています。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-134">This parallels the rules surrounding the feature `var` and user defined types of the same name.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="9b1ce-135">短所</span><span class="sxs-lookup"><span data-stu-id="9b1ce-135">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="9b1ce-136">この機能の主な欠点は、少数の開発者が提供することです。通常は、低レベルのライブラリ作成者またはフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-136">The primary drawback of this feature is that it serves a small number of developers: typically low level library authors or frameworks.</span></span>  <span data-ttu-id="9b1ce-137">そのため、少数の開発者にとって貴重な言語の時間が費やされています。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-137">Hence it's spending precious language time for a small number of developers.</span></span> 

<span data-ttu-id="9b1ce-138">ただし、多くの場合、これらのフレームワークは .NET アプリケーションの大半の基礎となります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-138">Yet these frameworks are often the basis for the majority of .NET applications out there.</span></span>  <span data-ttu-id="9b1ce-139">そのため、このレベルではパフォーマンスと正確性が優先され、.NET エコシステムに波及効果を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-139">Hence performance / correctness wins at this level can have a ripple effect on the .NET ecosystem.</span></span>  <span data-ttu-id="9b1ce-140">これにより、対象ユーザーが制限されていても、この機能を考慮する価値があります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-140">This makes the feature worth considering even with the limited audience.</span></span>

## <a name="alternatives"></a><span data-ttu-id="9b1ce-141">代替</span><span class="sxs-lookup"><span data-stu-id="9b1ce-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="9b1ce-142">考慮すべき選択肢がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-142">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="9b1ce-143">現状では、この機能は独自のメリットには合わないため、開発者は暗黙的なオプトイン動作を引き続き使用します。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-143">The status quo:  The feature is not justified on its own merits and developers continue to use the implicit opt in behavior.</span></span>

## <a name="questions"></a><span data-ttu-id="9b1ce-144">疑問がある場合</span><span class="sxs-lookup"><span data-stu-id="9b1ce-144">Questions</span></span>
[quesions]: #questions

### <a name="metadata-representation"></a><span data-ttu-id="9b1ce-145">メタデータ表現</span><span class="sxs-lookup"><span data-stu-id="9b1ce-145">Metadata Representation</span></span>

<span data-ttu-id="9b1ce-146">F#言語では、署名ファイル内の制約がエンコードC#されます。これは、その表現を再利用できないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-146">The F# language encodes the constraint in the signature file which means C# cannot re-use their representation.</span></span> <span data-ttu-id="9b1ce-147">この制約には、新しい属性を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-147">A new attribute will need to be chosen for this constraint.</span></span> <span data-ttu-id="9b1ce-148">さらに、この制約を持つメソッドは、mod 要求によって保護されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-148">Additionally a method which has this constraint must be protected by a mod-req.</span></span>

### <a name="blittable-vs-unmanaged"></a><span data-ttu-id="9b1ce-149">Blittable とアンマネージ</span><span class="sxs-lookup"><span data-stu-id="9b1ce-149">Blittable vs. Unmanaged</span></span>
<span data-ttu-id="9b1ce-150">このF#言語には、アンマネージキーワードを使用する非常に[よく似た機能](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints)があります。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-150">The F# language has a very [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) which uses the keyword unmanaged.</span></span> <span data-ttu-id="9b1ce-151">Blittable 名は、Midori の使用から取得されます。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-151">The blittable name comes from the use in Midori.</span></span>  <span data-ttu-id="9b1ce-152">ここで優先順位を指定し、代わりにアンマネージを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-152">May want to look to precedence here and use unmanaged instead.</span></span> 

<span data-ttu-id="9b1ce-153">**解決策**アンマネージドを使用することを決定する言語</span><span class="sxs-lookup"><span data-stu-id="9b1ce-153">**Resolution** The language decide to use unmanaged</span></span> 

### <a name="verifier"></a><span data-ttu-id="9b1ce-154">検証ツール</span><span class="sxs-lookup"><span data-stu-id="9b1ce-154">Verifier</span></span>

<span data-ttu-id="9b1ce-155">ジェネリック型パラメーターへのポインターの使用を理解するために、検証ツール/ランタイムを更新する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-155">Does the verifier / runtime need to be updated to understand the use of pointers to generic type parameters?</span></span>  <span data-ttu-id="9b1ce-156">または、変更せずにそのまま機能しますか。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-156">Or can it simply work as is without changes?</span></span>

<span data-ttu-id="9b1ce-157">**解決策**変更は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-157">**Resolution** No changes needed.</span></span> <span data-ttu-id="9b1ce-158">すべてのポインター型は単に検証できません。</span><span class="sxs-lookup"><span data-stu-id="9b1ce-158">All pointer types are simply unverifiable.</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="9b1ce-159">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="9b1ce-159">Design meetings</span></span>

<span data-ttu-id="9b1ce-160">該当なし</span><span class="sxs-lookup"><span data-stu-id="9b1ce-160">n/a</span></span>
