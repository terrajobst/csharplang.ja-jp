---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483554"
---
# <a name="compiler-intrinsics"></a><span data-ttu-id="0d2ea-101">コンパイラの組み込み</span><span class="sxs-lookup"><span data-stu-id="0d2ea-101">Compiler Intrinsics</span></span>

## <a name="summary"></a><span data-ttu-id="0d2ea-102">まとめ</span><span class="sxs-lookup"><span data-stu-id="0d2ea-102">Summary</span></span>

<span data-ttu-id="0d2ea-103">この提案は、現在は効率的にアクセスできない、または `ldftn`、`ldvirtftn`、`ldtoken`、および `calli`の下位レベルの IL オペコードを公開する言語コンストラクトを提供します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-103">This proposal provides language constructs that expose low level IL opcodes that cannot currently be accessed efficiently, or at all: `ldftn`, `ldvirtftn`, `ldtoken` and `calli`.</span></span> <span data-ttu-id="0d2ea-104">これらの低レベルのオペコードは、高パフォーマンスのコードでは重要であり、開発者はそれらにアクセスするための効率的な方法を必要とします。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-104">These low level opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="0d2ea-105">目的</span><span class="sxs-lookup"><span data-stu-id="0d2ea-105">Motivation</span></span>

<span data-ttu-id="0d2ea-106">この機能の動機と背景については、次の問題で説明されています (この機能が実装される可能性があります)。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span> 

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="0d2ea-107">この代替設計の提案は、@msjabby によって元の提案のプロトタイプの実装を確認した後、重要なコードベース全体で使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-107">This alternate design proposal comes after reviewing a prototype implementation of the original proposal by @msjabby as well as the use throughout a significant code base.</span></span> <span data-ttu-id="0d2ea-108">この設計は、@mjsabby、@tmat および @jkotasからの重要な入力で行われました。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-108">This design was done with significant input from @mjsabby, @tmat and @jkotas.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="0d2ea-109">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="0d2ea-109">Detailed Design</span></span> 

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="0d2ea-110">ターゲットメソッドへのアドレスを許可する</span><span class="sxs-lookup"><span data-stu-id="0d2ea-110">Allow address of to target methods</span></span>

<span data-ttu-id="0d2ea-111">アドレス式の引数として、メソッドグループが許可されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-111">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="0d2ea-112">このような式の型が `void*`されます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-112">The type of such an expression will be `void*`.</span></span> 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

<span data-ttu-id="0d2ea-113">ここではデリゲート変換がないため、メソッドグループのメンバーをフィルター処理するためのメカニズムは、静的/インスタンスアクセスだけです。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-113">Given there is no delegate conversion here the only mechanism for filtering members in the method group is by static / instance access.</span></span> <span data-ttu-id="0d2ea-114">メンバーを区別できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-114">If that cannot distinguish the members then a compile time error will occur.</span></span>

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

<span data-ttu-id="0d2ea-115">このコンテキストの addressof 式は、次の方法で実装されます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-115">The addressof expression in this context will be implemented in the following manner:</span></span>

- <span data-ttu-id="0d2ea-116">ldftn: メソッドが非仮想の場合。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-116">ldftn: when the method is non-virtual.</span></span>
- <span data-ttu-id="0d2ea-117">ldvirtftn: メソッドが仮想である場合。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-117">ldvirtftn: when the method is virtual.</span></span>

<span data-ttu-id="0d2ea-118">この機能の制限:</span><span class="sxs-lookup"><span data-stu-id="0d2ea-118">Restrictions of this feature:</span></span>

- <span data-ttu-id="0d2ea-119">インスタンスメソッドは、値に対して呼び出し式を使用する場合にのみ指定できます</span><span class="sxs-lookup"><span data-stu-id="0d2ea-119">Instance methods can only be specified when using an invocation expression on a value</span></span>
- <span data-ttu-id="0d2ea-120">ローカル関数は `&`では使用できません。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-120">Local functions cannot be used in `&`.</span></span> <span data-ttu-id="0d2ea-121">これらのメソッドの実装の詳細は、言語によって意図的に指定されていません。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-121">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="0d2ea-122">これには、静的であるかインスタンスであるか、またはそれらが出力されるシグネチャが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-122">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="handleof"></a><span data-ttu-id="0d2ea-123">handleof</span><span class="sxs-lookup"><span data-stu-id="0d2ea-123">handleof</span></span>

<span data-ttu-id="0d2ea-124">`handleof` コンテキストキーワードは、`ldtoken` 命令を使用して、フィールド、メンバー、または型を、それと等価な `RuntimeHandle` 型に変換します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-124">The `handleof` contextual keyword will translate a field, member or type into their equivalent `RuntimeHandle` type using the `ldtoken` instruction.</span></span> <span data-ttu-id="0d2ea-125">式の正確な型は、`handleof`内の名前の種類によって異なります。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-125">The exact type of the expression will depend on the kind of the name in `handleof`:</span></span>

- <span data-ttu-id="0d2ea-126">フィールド: `RuntimeFieldHandle`</span><span class="sxs-lookup"><span data-stu-id="0d2ea-126">field: `RuntimeFieldHandle`</span></span>
- <span data-ttu-id="0d2ea-127">種類: `RuntimeTypeHandle`</span><span class="sxs-lookup"><span data-stu-id="0d2ea-127">type: `RuntimeTypeHandle`</span></span>
- <span data-ttu-id="0d2ea-128">メソッド: `RuntimeMethodHandle`</span><span class="sxs-lookup"><span data-stu-id="0d2ea-128">method: `RuntimeMethodHandle`</span></span>

<span data-ttu-id="0d2ea-129">`handleof` する引数は `nameof`と同じです。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-129">The arguments to `handleof` are identical to `nameof`.</span></span> <span data-ttu-id="0d2ea-130">これは、簡易名、修飾名、メンバーアクセス、指定されたメンバーを持つ基本アクセス、または指定されたメンバーを使用したこのアクセスである必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-130">It must be a simple name, qualified name, member access, base access with a specified member, or this access with a specified member.</span></span> <span data-ttu-id="0d2ea-131">引数の式はコード定義を識別しますが、評価されることはありません。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-131">The argument expression identifies a code definition, but it is never evaluated.</span></span>

<span data-ttu-id="0d2ea-132">`handleof` 式は実行時に評価され、`RuntimeHandle`の戻り値の型があります。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-132">The `handleof` expression is evaluated at runtime and has a return type of `RuntimeHandle`.</span></span> <span data-ttu-id="0d2ea-133">安全なコードや安全ではないコードで実行できます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-133">This can be executed in safe code as well as unsafe.</span></span> 

``` 
RuntimeHandle stringHandle = handleof(string);
```

<span data-ttu-id="0d2ea-134">この機能の制限:</span><span class="sxs-lookup"><span data-stu-id="0d2ea-134">Restrictions of this feature:</span></span>

- <span data-ttu-id="0d2ea-135">プロパティは `handleof` 式では使用できません。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-135">Properties cannot be used in a `handleof` expression.</span></span>
- <span data-ttu-id="0d2ea-136">スコープに既存の `handleof` 名がある場合、`handleof` 式は使用できません。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-136">The `handleof` expression cannot be used when there is an existing `handleof` name in scope.</span></span> <span data-ttu-id="0d2ea-137">たとえば、型、名前空間などです。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-137">For example a type, namespace, etc ...</span></span>

### <a name="calli"></a><span data-ttu-id="0d2ea-138">呼び出し</span><span class="sxs-lookup"><span data-stu-id="0d2ea-138">calli</span></span>

<span data-ttu-id="0d2ea-139">コンパイラは、`.calli` 命令に効率的に変換する新しい型の `extern` 関数のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-139">The compiler will add support for a new type of `extern` function that efficiently translates into a `.calli` instruction.</span></span> <span data-ttu-id="0d2ea-140">Extern 属性は、次の図形の属性でマークされます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-140">The extern attribute will be marked with an attribute of the following shape:</span></span>

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

<span data-ttu-id="0d2ea-141">これにより、開発者は次の形式でメソッドを定義できます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-141">This allows developers to define methods in the following form:</span></span>

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

<span data-ttu-id="0d2ea-142">`CallIndirect` 属性が適用されているメソッドに関する制限事項を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-142">Restrictions on the method which has the `CallIndirect` attribute applied:</span></span>

- <span data-ttu-id="0d2ea-143">`DllImport` 属性を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-143">Cannot have a `DllImport` attribute.</span></span>
- <span data-ttu-id="0d2ea-144">をジェネリックにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-144">Cannot be generic.</span></span>

## <a name="open-issues"></a><span data-ttu-id="0d2ea-145">懸案事項を開く</span><span class="sxs-lookup"><span data-stu-id="0d2ea-145">Open Issues</span></span>

### <a name="callingconvention"></a><span data-ttu-id="0d2ea-146">CallingConvention</span><span class="sxs-lookup"><span data-stu-id="0d2ea-146">CallingConvention</span></span>

<span data-ttu-id="0d2ea-147">設計された `CallIndirectAttribute` は、マネージ呼び出し規約のエントリを持たない `CallingConvention` 列挙型を使用します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-147">The `CallIndirectAttribute` as designed uses the `CallingConvention` enum which lacks an entry for managed calling conventions.</span></span> <span data-ttu-id="0d2ea-148">列挙型は、この呼び出し規約を含めるように拡張する必要があります。または、属性で別の方法を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-148">The enum either needs to be extended to include this calling convention or the attribute needs to take a different approach.</span></span>

## <a name="considerations"></a><span data-ttu-id="0d2ea-149">考慮事項</span><span class="sxs-lookup"><span data-stu-id="0d2ea-149">Considerations</span></span>

### <a name="disambiguating-method-groups"></a><span data-ttu-id="0d2ea-150">明確化メソッドグループ</span><span class="sxs-lookup"><span data-stu-id="0d2ea-150">Disambiguating method groups</span></span>

<span data-ttu-id="0d2ea-151">ここでは、アドレス式に渡されるメソッドグループを明確に区別できるようにする機能について説明しました。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-151">There was some discussion around features that would make it easier to disambiguate method groups passed to an address-of expression.</span></span> <span data-ttu-id="0d2ea-152">たとえば、構文に署名要素を追加する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-152">For instance potentially adding signature elements to the syntax:</span></span>

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

<span data-ttu-id="0d2ea-153">これは、説得力のあるケースを作成できなかったため、または単純な構文がここで想定されていたため、拒否されました。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-153">This was rejected because a compelling case could not be made nor could a simple syntax be envisioned here.</span></span> <span data-ttu-id="0d2ea-154">また、非常に簡単に前進することもできます。単純に明確な別のC#メソッドを定義し、コードを使用して目的の関数を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-154">Also there is a fairly straight forward work around: simple define another method that is unambiguous and uses C# code to call into the desired function.</span></span> 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

<span data-ttu-id="0d2ea-155">これは、`static` ローカル関数が言語に入る場合にもより簡単になります。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-155">This becomes even simpler if `static` local functions enter the language.</span></span> <span data-ttu-id="0d2ea-156">次に、あいまいなアドレス操作を使用したのと同じ関数で、回避策を定義できます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-156">Then the work around could be defined in the same function that used the ambiguous address-of operation:</span></span>

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a><span data-ttu-id="0d2ea-157">LoadTypeTokenInt32</span><span class="sxs-lookup"><span data-stu-id="0d2ea-157">LoadTypeTokenInt32</span></span>

<span data-ttu-id="0d2ea-158">コンパイル時にメタデータトークンを `int` 値として読み込むことが許可された元の提案。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-158">The original proposal allowed for metadata tokens to be loaded as `int` values at compile time.</span></span> <span data-ttu-id="0d2ea-159">基本的には、`handleof` と同じ引数を持つ `tokenof` を持ちますが、コンパイル時に `int` 定数に評価されます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-159">Essentially have `tokenof` that has the same arguments as `handleof` but is evaluated at compile time to an `int` constant.</span></span> 

<span data-ttu-id="0d2ea-160">これは、IL の書き換え (.NET には多くの場合) で大きな問題が発生するため、拒否されました。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-160">This was rejected as it causes significant problem for IL rewrites (of which .NET has many).</span></span> <span data-ttu-id="0d2ea-161">このような再作成者は、多くの場合、これらの値を無効にできるような方法でメタデータテーブルを操作します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-161">Such rewriters often manipulate the metadata tables in a way that could invalidate these values.</span></span> <span data-ttu-id="0d2ea-162">単純な `int` 値として格納されている場合、このような再作成者がこれらの値を更新するための適切な方法はありません。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-162">There is no reasonable way for such rewriters to update these values when they are stored as simple `int` values.</span></span>

<span data-ttu-id="0d2ea-163">メタデータエントリの非透過ハンドルを持つ基礎となる概念は、ランタイムチームによって引き続き調査されます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-163">The underlying idea of having an opaque handle for metadata entries will continue to be explored by the runtime team.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="0d2ea-164">将来の注意事項</span><span class="sxs-lookup"><span data-stu-id="0d2ea-164">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="0d2ea-165">静的ローカル関数</span><span class="sxs-lookup"><span data-stu-id="0d2ea-165">static local functions</span></span>

<span data-ttu-id="0d2ea-166">これは、ローカル関数で `static` 修飾子を許可する[提案](https://github.com/dotnet/csharplang/issues/1565)を指します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-166">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="0d2ea-167">このような関数は、ソースコードで正確なシグネチャを指定した `static` として出力されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-167">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="0d2ea-168">このような関数は、ローカル関数の現在の問題を含んでいないため、`&` の有効な引数である必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-168">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today.</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="0d2ea-169">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="0d2ea-169">NativeCallableAttribute</span></span>

<span data-ttu-id="0d2ea-170">CLR には、ネイティブコードから直接呼び出すことができる方法でマネージメソッドを出力できる機能があります。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-170">The CLR has a feature that allows for managed methods to be emitted in such a way that they are directly callable from native code.</span></span> <span data-ttu-id="0d2ea-171">これを行うには、メソッドに `NativeCallableAttribute` を追加します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-171">This is done by adding the `NativeCallableAttribute` to methods.</span></span> <span data-ttu-id="0d2ea-172">このようなメソッドはネイティブコードからのみ呼び出すことができるため、シグネチャには blittable 型のみを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-172">Such a method is only callable from native code and hence must contain only blittable types in the signature.</span></span> <span data-ttu-id="0d2ea-173">マネージコードからを呼び出すと、ランタイムエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-173">Calling from managed code results in a runtime error.</span></span> 

<span data-ttu-id="0d2ea-174">この機能は、次のように、この提案に適しています。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-174">This feature would pattern well with this proposal as it would allow:</span></span>

- <span data-ttu-id="0d2ea-175">マネージコードで定義されている関数をネイティブコードに渡すことは、マネージコードまたはネイティブコードのオーバーヘッドを発生させることなく、(のアドレスを介して) 関数ポインターとして使用します。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-175">Passing a function defined in managed code to native code as a function pointer (via address-of) with no overhead in managed or native code.</span></span> 
- <span data-ttu-id="0d2ea-176">ランタイムでは、コンパイル時に呼び出されないように、マネージコードでこのような関数にサイトエラーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0d2ea-176">Runtime can introduce use site errors for such functions in managed code to prevent them from being invoked at compile time.</span></span>




