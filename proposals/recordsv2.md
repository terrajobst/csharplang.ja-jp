---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/09/2019
ms.locfileid: "79483926"
---

# <a name="records-v2"></a><span data-ttu-id="d1c4a-101">レコード v2</span><span class="sxs-lookup"><span data-stu-id="d1c4a-101">Records v2</span></span>

<span data-ttu-id="d1c4a-102">これまで、データの操作を可能にする機能として、レコードについて考えました。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-102">In the past we've thought about records as a feature to enable working with data.</span></span>

<span data-ttu-id="d1c4a-103">"データの操作" は多数のファセットを持つ大規模なグループであるため、それぞれを個別に調べることが重要です。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-103">"Working with data" is a big group with a number of facets, so it may be interesting to look at each in isolation.</span></span> <span data-ttu-id="d1c4a-104">まず、今日のレコードの例とその欠点を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-104">Let's start by looking at an example of records today and some of its drawbacks.</span></span>

<span data-ttu-id="d1c4a-105">たとえば、次のようにして、簡単なレコードを定義します。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-105">For instance, a simple record would be defined today as follows</span></span>

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

<span data-ttu-id="d1c4a-106">使用量が読み取られます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-106">and the usage would read</span></span>

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

<span data-ttu-id="d1c4a-107">このコードには、次のような大きな利点があります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-107">There are significant advantages in this code:</span></span>

1. <span data-ttu-id="d1c4a-108">定義はバージョンの回復性であり、プロパティを簡単に追加または移動できます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-108">The definition is version resilient, properties can easily be added or moved</span></span>
2. <span data-ttu-id="d1c4a-109">プロパティは任意の順序で設定できます。また、初期化内の名前は常にアクセサーと一致します。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-109">Properties can be set in any order, and the names in the initialization always match the accessors</span></span>
3. <span data-ttu-id="d1c4a-110">既定値を持つプロパティは単にスキップできます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-110">Properties with default values can simply be skipped</span></span>

<span data-ttu-id="d1c4a-111">最初の欠点は、プロパティが変更可能である必要があることです。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-111">The first flaw is that the properties must now be mutable.</span></span>

# <a name="mutability"></a><span data-ttu-id="d1c4a-112">可能性</span><span class="sxs-lookup"><span data-stu-id="d1c4a-112">Mutability</span></span>

<span data-ttu-id="d1c4a-113">ここでは、オブジェクト初期化C#子で `readonly` メンバーを設定する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-113">What we'd like is for C# to provide a way to set a `readonly` member in object initializers.</span></span>
<span data-ttu-id="d1c4a-114">この初期化を念頭に置いて設計されていない型もあるため、オプトインすることもできます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-114">Since some types may not have been designed with this initialization in mind, we'd also like it to be opt-in.</span></span>

<span data-ttu-id="d1c4a-115">提案されたソリューションは、プロパティとフィールドに適用できる新しい修飾子 `initonly`です。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-115">The proposed solution is a new modifier, `initonly`, that can be applied to properties and fields:</span></span>

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="d1c4a-116">この codegen は非常に簡単です。読み取り専用フィールドを設定するだけです。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-116">The codegen for this is surprisingly straight forward: we just set the readonly field.</span></span>
<span data-ttu-id="d1c4a-117">具体的には、下げられたプロパティは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-117">Specifically, the lowered properties would look like:</span></span>

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

<span data-ttu-id="d1c4a-118">CLR では、読み取り専用フィールドの設定は検証不可能ですが、unsafe は考慮されません。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-118">The CLR considers setting readonly fields to be unverifiable, but not unsafe.</span></span> <span data-ttu-id="d1c4a-119">より高度な検証をサポートするために、次の規則が提案されています。読み取り専用フィールドは、`initonly` メソッド内、または CLR スタック上の新しいオブジェクトでのみ変更でき、ストアまたはメソッドの呼び出しによって公開されていません。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-119">To support a more advanced verifier, the following rule is proposed: a readonly field can be modified only inside `initonly` methods, or on a new object that is on the CLR stack and has not been published via a store or method call.</span></span>

<span data-ttu-id="d1c4a-120">これにより、`UserInfo` の例の中でも、さまざまな問題を解決する必要がありますが、複雑で不安定な出力方法は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-120">This should solve many of the problems with mutability in the `UserInfo` example, while not requiring complicated or brittle emit strategies.</span></span> <span data-ttu-id="d1c4a-121">ただし、不変性によって新しい問題が発生します。変更されたオブジェクトを簡単に構築できます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-121">However, immutability does present a new problem: easily constructing an object with changes.</span></span>

# <a name="with-ing"></a><span data-ttu-id="d1c4a-122">With-ing</span><span class="sxs-lookup"><span data-stu-id="d1c4a-122">With-ing</span></span>

<span data-ttu-id="d1c4a-123">不変性を使用してプログラミングする場合は、オブジェクトを直接変更するのではなく、変更を加えたコピーを作成することによって、オブジェクトを変更します。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-123">When programming with immutability, making changes to an object is done by constructing a copy with changes instead of making the changes directly on the object.</span></span> <span data-ttu-id="d1c4a-124">残念ながら、現在のスタイルのレコードを使用しC#ていても、でこれを行うための便利な方法はありません。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-124">Unfortunately, there's no convenient way to do this in C#, even with current-style records.</span></span> <span data-ttu-id="d1c4a-125">以前は、この機能を実装するレコードに対して、自動生成された "With" メソッドが提供されることが提案されています。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-125">It's been previously proposed that some kind of autogenerated "With" method be provided for records that implements that functionality.</span></span> <span data-ttu-id="d1c4a-126">このようなメカニズムがある場合は、`initonly` メンバーと連携することが重要です。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-126">If we have such a mechanism, it's important that it work with `initonly` members.</span></span> <span data-ttu-id="d1c4a-127">これを実現するために、オブジェクト初期化子に似た `with` 式を追加することを提案しています。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-127">To achieve this, it's proposed that we add a `with` expression, analogous to an object initializer.</span></span> <span data-ttu-id="d1c4a-128">使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-128">Sample usage would be as follows:</span></span>

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

<span data-ttu-id="d1c4a-129">結果として得られる `newUserName` オブジェクトは `userInfo`のコピーであり、`Username` は "angocke" に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-129">The resulting `newUserName` object would be a copy of `userInfo`, with `Username` set to "angocke".</span></span>
<span data-ttu-id="d1c4a-130">`with` 式の codegen も、オブジェクト初期化子に似ています。新しいオブジェクトが構築され、`initonly` `Username` setter がメソッド本体で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-130">The codegen on the `with` expression would also be similar to the object initializer: a new object is constructed, and then the `initonly` `Username` setter would be called in the method body.</span></span>

<span data-ttu-id="d1c4a-131">もちろん、ここでの違いは、構築される新しいオブジェクトが単純な新しいオブジェクト作成ではなく、元のオブジェクトと重複していることです。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-131">Of course, the difference here is that the new object being constructed is not a simple new object creation, it is a duplicate of the original object.</span></span> <span data-ttu-id="d1c4a-132">この機能を提供するには、オブジェクトに、重複するオブジェクトを提供する "With constructor" が用意されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-132">To provide this functionality, we require that the object provide a "With constructor" that provides a duplicate object.</span></span> <span data-ttu-id="d1c4a-133">サンプルの `With` コンストラクターは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-133">A sample `With` constructor would look like:</span></span>

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

<span data-ttu-id="d1c4a-134">特に、`with` 式では、オブジェクト初期化子と同様に `initonly` メンバーが設定されるため、検証をサポートするには、`initonly` メンバーが設定される前にオブジェクトが公開されていないことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-134">Notably, the `with` expression will set `initonly` members, just like the object initializer, so to support verification we must ensure that the object cannot have been published before the `initonly` members are set.</span></span> <span data-ttu-id="d1c4a-135">これを適用するには、`WithConstructor` 属性 (またはそれと同等の構文) によって、メソッドに新しい規則が適用されます。すべての return ステートメントには、オブジェクト初期化子と共に、場合によってはオブジェクト作成式が直接含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-135">To enforce this, the `WithConstructor` attribute (or equivalent syntax) will enforce a new rule for the method: all return statements must directly contain an object creation expression, possibly with an object initializer.</span></span>

<span data-ttu-id="d1c4a-136">`With` コンストラクターで検証が必要な場合、ユーザーはその検証を行うコンストラクターを導入することができます。たとえば、</span><span class="sxs-lookup"><span data-stu-id="d1c4a-136">If the `With` constructor requires validation, the user may introduce a constructor to do that validation, e.g.</span></span>

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

<span data-ttu-id="d1c4a-137">`With` に関連付けられた複雑さの最後の部分は継承です。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-137">The last piece of complexity associated with `With` is inheritance.</span></span> <span data-ttu-id="d1c4a-138">レコードが拡張可能な場合は、サブクラスの新しい `With` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-138">If your record is extensible, you will need to provide a new `With` for the subclass.</span></span> <span data-ttu-id="d1c4a-139">これは、次のように実現できます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-139">This can be achieved as follows:</span></span>

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

<span data-ttu-id="d1c4a-140">ここでもう1つ複雑な点に注意してください。派生型で `With` コンストラクターをオーバーライドするには、オーバーライドで共変の戻り値の型もサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-140">Note one additional piece of complexity here: in order to override the `With` constructor with the derived type the language will also need to support covariant return types in overrides.</span></span>
<span data-ttu-id="d1c4a-141">この[機能については、](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)既に別の提案があります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-141">There is already a separate proposal for this feature [here](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).</span></span>

<span data-ttu-id="d1c4a-142">**デメリット**</span><span class="sxs-lookup"><span data-stu-id="d1c4a-142">**Drawbacks**</span></span>

- <span data-ttu-id="d1c4a-143">`WithConstructor`s 内のすべての return ステートメントに新しいオブジェクト式を含めることは、制限がありません。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-143">Making all return statements in `WithConstructor`s contain new object expressions is restrictive.</span></span>
  <span data-ttu-id="d1c4a-144">これは、新しいオブジェクトがメソッドをエスケープしないようにするフロー分析によって軽減される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-144">This could be possibly be mitigated by flow analysis that ensures the new object doesn't escape the method</span></span>
- <span data-ttu-id="d1c4a-145">コンパイラのテクニックを使用したオーバーライドの分散をサポートするには、スタブメソッドが必要です。これにより、継承の深さで quadratically が拡張されます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-145">Supporting variance in overrides through compiler tricks will require stub methods, which will grow quadratically with the inheritance depth.</span></span> <span data-ttu-id="d1c4a-146">スタブメソッドの必要性は、シグネチャを厳密に一致させるランタイム要件によるものです。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-146">The need for a stub method is due to a runtime requirement that override signatures match exactly.</span></span> <span data-ttu-id="d1c4a-147">ランタイム要件が緩和の場合、スタブメソッドはまったく必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-147">If the runtime requirement were loosened, the stub methods would not be required at all.</span></span>
- <span data-ttu-id="d1c4a-148">フォーム `Type(Type original)` のチェーンコンストラクターを使用すると、パターンを使用するためにそのコンストラクターを効果的に予約できます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-148">Using chained constructors of the form `Type(Type original)` effectively reserves that constructor for the use of the pattern.</span></span> <span data-ttu-id="d1c4a-149">コンストラクターのセマンティクスは一意であるため、名前を変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-149">Since constructors have unique semantics and cannot be re-named this could be limiting.</span></span>


## <a name="wrapping-it-all-up-records"></a><span data-ttu-id="d1c4a-150">すべてをラップする: レコード</span><span class="sxs-lookup"><span data-stu-id="d1c4a-150">Wrapping it all up: Records</span></span>

<span data-ttu-id="d1c4a-151">上記の機能を使用すると、以前は非常に難しいプログラミングのスタイルを実現できます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-151">The above features enable a style of programming that was very difficult before.</span></span> <span data-ttu-id="d1c4a-152">しかし、新しい機能を使用する場合でも、非常に冗長になり、すべての注釈に注釈を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-152">But even with the new features it could be quite verbose and error prone to annotate everything yourself.</span></span> <span data-ttu-id="d1c4a-153">また、既に記述されている Equals や GetHashCode のようないくつかの項目もありますが、手間がかかります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-153">There are also a few items, like Equals and GetHashCode, which can already be written today, it's just laborious.</span></span>
<span data-ttu-id="d1c4a-154">さらに、これらの新しいプリミティブの上に等価性を実装する場合の大きな欠点として、構造の等価性は、新しいデータを追加するときにデータ型によって変わることがありますが、手動で処理する場合は、これらの問題が同期されない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-154">Moreover, a significant flaw in implementing equality on top of these new primitives is that structural equality is something that should change with your data type as new data is added, but when handling it manually it is likely that these things can get out of sync.</span></span>

<span data-ttu-id="d1c4a-155">そのため、新しい機能をC#提供するのではなく、レコードの新しい構文をサポートするように提案されていますが、既定値を設定し、レコードで使用するように設計されたコードを生成することもできます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-155">Therefore, it is proposed that C# support new syntax for records, not for providing new features, but for setting defaults and generating code designed for use in records.</span></span> <span data-ttu-id="d1c4a-156">構文の例は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-156">Example syntax would look like</span></span>

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="d1c4a-157">このクラス用に生成されたコードは、すべてのパブリックフィールドと自動プロパティをレコードの構造メンバーとして扱うことになります。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-157">The generated code for this class would regard all public fields and auto-properties as structural members of the record.</span></span> <span data-ttu-id="d1c4a-158">レコードメンバーは、メンバーの追加または除外に使用できる新しい `RecordMember(bool)` 属性を使用してカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-158">Record members could be customized using a new `RecordMember(bool)` attribute that could be used to either include or exclude members.</span></span> <span data-ttu-id="d1c4a-159">レコードメンバーは既定で `initonly` され、レコードメンバーに基づいて、クラスに対して等値が自動生成されます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-159">Record members would be `initonly` by default and equality would be autogenerated for the class based on the record members.</span></span> <span data-ttu-id="d1c4a-160">どの時点でも、これらのメンバーの動作は、ソースで宣言するだけでカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-160">At any point the behavior of these members could be customized simply by declaring them in source.</span></span> <span data-ttu-id="d1c4a-161">ユーザーが記述した実装は、すべてのパターン使用法で既定の実装を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-161">The user-written implementation would replace the default implementation in all pattern usage.</span></span>

<span data-ttu-id="d1c4a-162">継承の面での等価性は複雑ですが、[他のレコードの提案](records.md)では適切に解決されているように見えます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-162">Note that equality in the face of inheritance is complex, but seems to have been adequately solved in the [other records proposal](records.md).</span></span>

## <a name="primary-constructors"></a><span data-ttu-id="d1c4a-163">プライマリコンストラクター</span><span class="sxs-lookup"><span data-stu-id="d1c4a-163">Primary constructors</span></span>

<span data-ttu-id="d1c4a-164">前のレコードの提案には、型自体のパラメーターリストの新しい構文も含まれています。例:</span><span class="sxs-lookup"><span data-stu-id="d1c4a-164">Previous record proposal have also included a new syntax for a parameter list on the type itself, e.g.</span></span>

```C#
class Point(int X, int Y);
```

<span data-ttu-id="d1c4a-165">新しい設計では、パラメーターリストは、レコードと完全C#に統合される可能性がある直交特徴です。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-165">In the new design, the parameter list would be an orthogonal C# feature, which could be cleanly integrated with records.</span></span> <span data-ttu-id="d1c4a-166">プライマリコンストラクターがレコードに含まれている場合、パブリックフィールドや自動プロパティと同様に、新しい既定値が設定されます。プライマリコンストラクターのパラメーターは、同じ名前のパブリックレコードメンバープロパティを生成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-166">If a primary constructor is included in a record, it would have new defaults, just like public fields and auto-properties: the parameters in the primary constructor would be used to generate public record-member properties with the same name.</span></span> <span data-ttu-id="d1c4a-167">また、プライマリコンストラクターを使用して、deconstructor を自動生成できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-167">In addition, the primary constructor could now be used to auto-generate a deconstructor.</span></span>

<span data-ttu-id="d1c4a-168">たとえば、次のレコードのプライマリコンストラクターがあるとします。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-168">For example, the following record with a primary constructor</span></span>

```C#
data class Point(int X, int Y);
```

<span data-ttu-id="d1c4a-169">はと同じです。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-169">would be equivalent to</span></span>

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

<span data-ttu-id="d1c4a-170">また、上記の最終的な生成結果は、</span><span class="sxs-lookup"><span data-stu-id="d1c4a-170">and the final generation of the above would be</span></span>

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

<span data-ttu-id="d1c4a-171">プライマリコンストラクターを使用するデータクラスについては、他にもいくつかの情報を取得しました。生成されたプロテクトコンストラクター内でプライマリフィールドを設定する代わりに、プライマリコンストラクターにデリゲートします。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-171">Note that we've taken one other piece of information into account for a data class with a primary constructor: instead of setting the primary fields inside the generated protected constructor, we delegate to the primary constructor.</span></span> <span data-ttu-id="d1c4a-172">Point クラスに別の非プライマリレコードメンバーがある場合 (例:</span><span class="sxs-lookup"><span data-stu-id="d1c4a-172">If the Point class had another non-primary record member, e.g.</span></span>

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

<span data-ttu-id="d1c4a-173">これにより、生成された保護コンストラクターが次のように変更されます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-173">then that would change the generated protected constructor as follows:</span></span>

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

<span data-ttu-id="d1c4a-174">特に、プライマリコンストラクターを使用したレコードの継承については、何を行うかについては説明しません。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-174">Notably, this doesn't answer what to do about inheritance of records with primary constructors.</span></span> <span data-ttu-id="d1c4a-175">たとえば、</span><span class="sxs-lookup"><span data-stu-id="d1c4a-175">For instance,</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

<span data-ttu-id="d1c4a-176">任意の方法で解決するのではなく、より明示的な方法では、パラメーターリストを基本リストと共に指定する必要があります。たとえば、</span><span class="sxs-lookup"><span data-stu-id="d1c4a-176">Rather than resolving in an arbitrary manner, a more explicit approach could require that a parameter list be provided with the base list, e.g.</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

<span data-ttu-id="d1c4a-177">基本リストのパラメーターリストは、生成されたプライマリコンストラクター内の `base` の呼び出しに適用されます。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-177">The parameter list in the base list would then be applied to a `base` call in the generated primary constructor:</span></span>

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

<span data-ttu-id="d1c4a-178">1つのプライマリコンストラクターがレコードの外部にあるということは、引き続き、今後の提案のために開いています。</span><span class="sxs-lookup"><span data-stu-id="d1c4a-178">As for what a primary constructor could mean outside of a record, that is still open to further proposal.</span></span>
