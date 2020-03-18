---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2019
ms.locfileid: "79483902"
---
# <a name="primary-constructors"></a><span data-ttu-id="c52b3-101">プライマリコンストラクター</span><span class="sxs-lookup"><span data-stu-id="c52b3-101">Primary constructors</span></span>

* <span data-ttu-id="c52b3-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="c52b3-102">[x] Proposed</span></span>
* <span data-ttu-id="c52b3-103">[] プロトタイプ: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="c52b3-103">[ ] Prototype: Not started</span></span>
* <span data-ttu-id="c52b3-104">[] の実装: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="c52b3-104">[ ] Implementation: Not started</span></span>
* <span data-ttu-id="c52b3-105">[] 仕様: 開始されていません</span><span class="sxs-lookup"><span data-stu-id="c52b3-105">[ ] Specification: Not started</span></span>

## <a name="summary"></a><span data-ttu-id="c52b3-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="c52b3-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="c52b3-107">クラスはパラメーターリストを持つことができ、その場合、基底クラスの指定は引数リストを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-107">Classes can have a parameter list, and when they do, their base class specification can have an argument list.</span></span>
<span data-ttu-id="c52b3-108">プライマリコンストラクターのパラメーターは、クラス宣言全体を通じてスコープ内にあり、関数メンバーまたは匿名関数によってキャプチャされると、クラスにプライベートフィールドとして格納されます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-108">Primary constructor parameters are in scope throughout the class declaration, and if they are captured by a function member or anonymous function, they are stored as private fields in the class.</span></span>

## <a name="motivation"></a><span data-ttu-id="c52b3-109">目的</span><span class="sxs-lookup"><span data-stu-id="c52b3-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="c52b3-110">プログラム初期化コードに多くの定型句があることは一般的です。</span><span class="sxs-lookup"><span data-stu-id="c52b3-110">It is common to have a lot of boilerplate in program initialization code.</span></span> <span data-ttu-id="c52b3-111">一般的なケースでは、特定のデータ `x` が何度も記述されています。</span><span class="sxs-lookup"><span data-stu-id="c52b3-111">In the general case, a given piece of data `x` is mentioned many times:</span></span>

- <span data-ttu-id="c52b3-112">プライベートフィールドとして `_x`</span><span class="sxs-lookup"><span data-stu-id="c52b3-112">As a private field `_x`</span></span>
- <span data-ttu-id="c52b3-113">コンストラクターに `x` パラメーターとして</span><span class="sxs-lookup"><span data-stu-id="c52b3-113">As a parameter `x` to a constructor</span></span>
- <span data-ttu-id="c52b3-114">コンストラクターのパラメーターからのフィールドの割り当て `_x = x;`</span><span class="sxs-lookup"><span data-stu-id="c52b3-114">In an assignment `_x = x;` of the field from the parameter in the constructor</span></span>
- <span data-ttu-id="c52b3-115">プロパティとして `X`</span><span class="sxs-lookup"><span data-stu-id="c52b3-115">As a property `X`</span></span>
- <span data-ttu-id="c52b3-116">プロパティ set アクセス操作子 `x = value;`</span><span class="sxs-lookup"><span data-stu-id="c52b3-116">In the property setter `x = value;`</span></span>
- <span data-ttu-id="c52b3-117">プロパティの getter `return x;`</span><span class="sxs-lookup"><span data-stu-id="c52b3-117">In the property getter `return x;`</span></span>

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

<span data-ttu-id="c52b3-118">検証や計算を必要としないプロパティの場合は、自動プロパティを使用して面倒な作業を減らすことができます。これにより、プロパティの明示的なバッキングフィールドを宣言する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-118">For properties that don't require validation or computation, the tedium can be reduced using auto-properties, thus cutting out the need to declare an explicit backing field for the property.</span></span> <span data-ttu-id="c52b3-119">ただし、プロパティが自動プロパティで提供されるもの以外のロジックの種類を必要とする場合は、上記の方法をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="c52b3-119">But if your property requires any sort of logic beyond what an auto-property provides, the above is the best you an do.</span></span>

<span data-ttu-id="c52b3-120">プライマリコンストラクターでは、コンストラクターの引数をクラスのスコープ内に直接配置することによってオーバーヘッドを軽減し、バッキングフィールドを明示的に宣言する必要が習得ます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-120">Primary constructors instead reduce the overhead by putting constructor arguments directly in scope throughout the class, again obviating the need to explicitly declare a backing field.</span></span> <span data-ttu-id="c52b3-121">したがって、上記の例は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-121">Thus, the above example would become:</span></span>

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

<span data-ttu-id="c52b3-122">この例では、プライマリコンストラクターによって、`x` の名前付きエンティティの数が3から2に減り、`_x` バッキングフィールドが習得されます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-122">In this example, the primary constructor reduces the number of named entities for `x` from three to two, obviating the `_x` backing field.</span></span> <span data-ttu-id="c52b3-123">このメソッドは、3つのメンバー宣言 (プロパティ宣言自体のみを保持) のうちの2つを削除し、`x`/`_x`/`X` の合計数を8から5に減らします。</span><span class="sxs-lookup"><span data-stu-id="c52b3-123">It removes two out of three member declarations (keeping only the property declaration itself), and reduces the total number of mentions of `x`/`_x`/`X` from eight to five.</span></span>


## <a name="detailed-design"></a><span data-ttu-id="c52b3-124">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="c52b3-124">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="c52b3-125">クラスにはパラメーターリストを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-125">Classes can have a parameter list:</span></span>

``` c#
public class C(int i, string s)
{
    ...
}
```

<span data-ttu-id="c52b3-126">パラメーターリストを使用すると、クラス自体と同じアクセシビリティで、クラスに対してコンストラクターが暗黙的に宣言されます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-126">The parameter list causes a constructor to be implicitly declared for the class, with the same accessibility as the class itself.</span></span>

``` c#
new C(5, "Hello");
```

<span data-ttu-id="c52b3-127">プライマリコンストラクターのパラメーターは、クラス本体全体でスコープ内にあります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-127">Primary constructor parameters are in scope throughout the class body.</span></span> <span data-ttu-id="c52b3-128">関数メンバーまたは匿名関数によってキャプチャされた場合は、クラスのプライベートフィールドとして格納されます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-128">If they are captured by a function member or anonymous function, they become stored as private fields in the class.</span></span> <span data-ttu-id="c52b3-129">初期化中にのみ使用される場合は、オブジェクトに格納されません。</span><span class="sxs-lookup"><span data-stu-id="c52b3-129">If they are only used during initialization they will not be stored in the object.</span></span>

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

<span data-ttu-id="c52b3-130">プライマリコンストラクターを持つクラスに基底クラスの指定がある場合、そのクラスは引数リストを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-130">If a class with a primary constructor has a base class specification, that one can have an argument list.</span></span> <span data-ttu-id="c52b3-131">これは、暗黙的に宣言されたコンストラクターの `base(...)` 初期化子の引数リストとして機能します。</span><span class="sxs-lookup"><span data-stu-id="c52b3-131">This serves as the argument list to a `base(...)` initializer of the implicitly declared constructor.</span></span> <span data-ttu-id="c52b3-132">引数リストが指定されていない場合は、空のものが想定されます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-132">If no argument list is provided, an empty one is assumed.</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
<span data-ttu-id="c52b3-133">クラスは、明示的に定義されたコンストラクターも持つことができますが、これらはすべて `this(...)` 初期化子を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-133">The class can have explicitly defined constructors as well, but those all have to use a `this(...)` initializer.</span></span> <span data-ttu-id="c52b3-134">これにより、新しいインスタンスが構築されるときに、常にプライマリコンストラクターが呼び出されるようになります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-134">This ensures that the primary constructor is always called when a new instance is constructed.</span></span>

<span data-ttu-id="c52b3-135">クラス本体内のすべての初期化子は、生成されたコンストラクター内の割り当てになります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-135">All initializers in the class body will become assignments in the generated constructor.</span></span> <span data-ttu-id="c52b3-136">これは、他のクラスとは異なり、初期化子は、前ではなく、基本コンストラクターが呼び出され*た後*に実行されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="c52b3-136">This means that, unlike other classes, initializers will run *after* the base constructor has been invoked, not before.</span></span> <span data-ttu-id="c52b3-137">また、生成されたクラスには、メンバーによってキャプチャされたプライマリコンストラクターパラメーターを格納するために生成されたプライベートフィールドを初期化するための割り当てが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-137">In addition, the generated class will contain assignments to initialize any private fields that were generated to store primary constructor parameters that were captured by members.</span></span> <span data-ttu-id="c52b3-138">これらのメンバーは、ラムダ式のクロージャと同様の方法で、パラメーターの代わりにプライベートフィールドを使用するように書き換えられます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-138">Those members are rewritten to use the private field instead of the parameter in a manner similar to closures for lambda expressions.</span></span> <span data-ttu-id="c52b3-139">生成されたプライマリフィールドが最初に初期化された後、初期化子によって生成された割り当てがクラス内の外観の順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-139">The generated primary fields are initialized first, and then the initializer-generated assignments are executed in the order of appearance in the class.</span></span>

<span data-ttu-id="c52b3-140">上記の例では、クラス宣言の効果は次のように書き換えられます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-140">For the above example, the effect of the class declaration is as if rewritten like this:</span></span>

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

<span data-ttu-id="c52b3-141">キャプチャには、ラムダ式によってローカル変数をキャプチャする場合と同様の制限があります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-141">The capture has similar restrictions to the capture of local variables by lambda expressions.</span></span> <span data-ttu-id="c52b3-142">たとえば、`ref` パラメーターと `out` パラメーターは、プライマリコンストラクターで許可されますが、メンバー本体をキャプチャすることはできません。</span><span class="sxs-lookup"><span data-stu-id="c52b3-142">For instance, `ref` and `out` parameters are allowed in primary constructors, but cannot be captured my member bodies.</span></span>


## <a name="drawbacks"></a><span data-ttu-id="c52b3-143">短所</span><span class="sxs-lookup"><span data-stu-id="c52b3-143">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="c52b3-144">重要度の大まかな順序。</span><span class="sxs-lookup"><span data-stu-id="c52b3-144">In rough order of significance.</span></span>

* <span data-ttu-id="c52b3-145">この提案では、位置指定レコードに対しても提示された構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="c52b3-145">The proposal uses syntax that has also been proposed for positional records.</span></span> <span data-ttu-id="c52b3-146">両方の機能が必要な場合は、いくつかの設備が必要です。</span><span class="sxs-lookup"><span data-stu-id="c52b3-146">If we desire both features, some accommodation is required.</span></span> <span data-ttu-id="c52b3-147">例:</span><span class="sxs-lookup"><span data-stu-id="c52b3-147">E.g.</span></span> <span data-ttu-id="c52b3-148">レコードの `data` 修飾子が指定されました。</span><span class="sxs-lookup"><span data-stu-id="c52b3-148">a `data` modifier on records has been proposed.</span></span>
* <span data-ttu-id="c52b3-149">構築されたオブジェクトの割り当てサイズは、クラスのフルテキストに基づいてプライマリコンストラクターパラメーターのフィールドを割り当てるかどうかをコンパイラが決定するため、あまり明確ではありません。</span><span class="sxs-lookup"><span data-stu-id="c52b3-149">The allocation size of constructed objects is less obvious, as the compiler determines whether to allocate a field for a primary constructor parameter based on the full text of the class.</span></span> <span data-ttu-id="c52b3-150">このリスクは、ラムダ式による変数の暗黙的なキャプチャに似ています。</span><span class="sxs-lookup"><span data-stu-id="c52b3-150">This risk is similar to the implicit capture of variables by lambda expressions.</span></span>
* <span data-ttu-id="c52b3-151">一般的な誘惑 (または偶発的なパターン) は、複数の継承レベルで "同じ" パラメーターをキャプチャする場合があります。これは、基底クラスで保護されたフィールドを明示的に割り当てて、重複する割り当てを行うのではなく、コンストラクターチェーンが渡されるためです。オブジェクトの同じデータに対して。</span><span class="sxs-lookup"><span data-stu-id="c52b3-151">A common temptation (or accidental pattern) might be to capture the "same" parameter at multiple levels of inheritance as it is passed up the constructor chain instead of explicitly allotting it a protected field at the base class, leading to duplicated allocations for the same data in objects.</span></span> <span data-ttu-id="c52b3-152">これは、自動プロパティを使用して自動プロパティをオーバーライドする現在のリスクとよく似ています。</span><span class="sxs-lookup"><span data-stu-id="c52b3-152">This is very similar to today's risk of overriding auto-properties with auto-properties.</span></span> 
* <span data-ttu-id="c52b3-153">前述のように、通常はコンストラクター本体で表現される追加のロジックを使用する場所はありません。</span><span class="sxs-lookup"><span data-stu-id="c52b3-153">As proposed above, there is no place for additional logic that might usually expressed in constructor bodies.</span></span> <span data-ttu-id="c52b3-154">以下の "プライマリコンストラクター本体" 拡張機能は、それに対応しています。</span><span class="sxs-lookup"><span data-stu-id="c52b3-154">The "Primary constructor bodies" extension below addresses that.</span></span>
* <span data-ttu-id="c52b3-155">提案されたように、実行順序のセマンティクスは、通常のコンストラクターとはわずかに異なります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-155">As proposed, execution order semantics are subtly different than with ordinary constructors.</span></span> <span data-ttu-id="c52b3-156">これは、一部の拡張機能の提案 (特に "プライマリコンストラクター本体") を犠牲にする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-156">This could probably be remedied, but at the cost of some of the extension proposals (notably "Primary constructor bodies").</span></span>
* <span data-ttu-id="c52b3-157">この提案は、単一のコンストラクターをプライマリとして指定できる場合にのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="c52b3-157">The proposal only works when a single constructor can be designated primary.</span></span>
* <span data-ttu-id="c52b3-158">クラスとプライマリコンストラクターのアクセシビリティを個別に設定する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="c52b3-158">There is no way to have separate accessibility of the class and the primary constructor.</span></span> <span data-ttu-id="c52b3-159">たとえば、パブリックコンストラクターがすべて、必要となる1つのプライベート "build-all" コンストラクターにデリゲートする場合などです。</span><span class="sxs-lookup"><span data-stu-id="c52b3-159">For instance, in situations where public constructors all delegate to one private "build-it-all" constructor that would be needed.</span></span> <span data-ttu-id="c52b3-160">必要に応じて、後で構文を提示することもできます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-160">If necessary, syntax could be proposed for that later.</span></span>


## <a name="alternatives"></a><span data-ttu-id="c52b3-161">代替</span><span class="sxs-lookup"><span data-stu-id="c52b3-161">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="c52b3-162">詳細については、完全な位置指定レコードを代わりに使用することも、プライマリコンストラクターと共存させることもできます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-162">Full-on positional records may be an alternative, or may coexist with primary constructors, depending on the specifics.</span></span> <span data-ttu-id="c52b3-163">これにより、より*少ない*省略形で*より多く*のシナリオを実現できます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-163">They would allow for *more* abbreviation in a *smaller* number of scenarios.</span></span> <span data-ttu-id="c52b3-164">そのため、どちらも役に立つ可能性がありますが、両方を使用することは、互いに多少の統合が可能でない限り、過剰になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-164">So both are potentially useful, but having both may be overkill, unless they can be somewhat neatly integrated with each other.</span></span>


## <a name="possible-extensions"></a><span data-ttu-id="c52b3-165">拡張の可能性</span><span class="sxs-lookup"><span data-stu-id="c52b3-165">Possible extensions</span></span>
[extensions]: #possible-extensions

<span data-ttu-id="c52b3-166">これらは、それと共に検討される可能性があるコア提案のバリエーションや追加、または有用であると思われる場合は後の段階で使用されます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-166">These are variations or additions to the core proposal that may be considered in conjunction with it, or at a later stage if deemed useful.</span></span>

### <a name="primary-constructor-bodies"></a><span data-ttu-id="c52b3-167">プライマリコンストラクター本体</span><span class="sxs-lookup"><span data-stu-id="c52b3-167">Primary constructor bodies</span></span>

<span data-ttu-id="c52b3-168">多くの場合、コンストラクター自体には、初期化子として表現できないパラメーター検証ロジックや、その他の重要な初期化コードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c52b3-168">Constructors themselves often contain parameter validation logic or other nontrivial initialization code that cannot be expressed as initializers.</span></span>

<span data-ttu-id="c52b3-169">プライマリコンストラクターを拡張して、ステートメントブロックをクラス本体に直接表示できるようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-169">Primary constructors could be extended to allow statement blocks to appear directly in the class body.</span></span> <span data-ttu-id="c52b3-170">これらのステートメントは、生成されたコンストラクターに挿入されます。これらのステートメントは初期化の初期化の間に出現します。そのため、初期化子と共に実行されます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-170">Those statements would be inserted in the generated constructor at the point where they appear between initializing assignments, and would thus be executed interspersed with initializers.</span></span> <span data-ttu-id="c52b3-171">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c52b3-171">For instance:</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a><span data-ttu-id="c52b3-172">初期化子フィールドおよび初期化子関数</span><span class="sxs-lookup"><span data-stu-id="c52b3-172">Initializer fields and initializer functions</span></span>

<span data-ttu-id="c52b3-173">プライマリコンストラクターを持つクラスでは、ローカル変数やローカル関数のように、アクセシビリティ修飾子のないフィールド宣言とメソッド宣言を検討できます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-173">In a class with a primary constructor we could consider field and method declarations without accessibility modifiers to be more like local variables and local functions:</span></span>

* <span data-ttu-id="c52b3-174">プライマリコンストラクターのパラメーターと同様に、"初期化子フィールド" は、関数メンバーで使用されていた場合にのみ実際のプライベートフィールドにキャプチャされます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-174">Just like primary constructor parameters the "initializer fields" would only be captured into an actual private field if they were used in function members.</span></span>
* <span data-ttu-id="c52b3-175">"初期化子関数" は、プライマリコンストラクターのパラメーターと初期化子フィールドが他の関数メンバーで使用されていた場合にのみ、それらをキャプチャすることを検討します。</span><span class="sxs-lookup"><span data-stu-id="c52b3-175">The "initializer functions" would only be considered to capture primary constructor parameters and initializer fields if they were themselves used in other function members.</span></span> <span data-ttu-id="c52b3-176">キャプチャされていない場合は、ローカル関数のように、より最適化された方法で生成される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-176">If not captured, they could be generated in a more optimal fashion, like local functions.</span></span>
* <span data-ttu-id="c52b3-177">プライマリコンストラクターパラメーターと同様に、メンバーアクセスでは使用できませんが、単純な名前としてのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-177">Just like primary constructor parameters they would not be available via member access, but only as a simple name.</span></span>

<span data-ttu-id="c52b3-178">これは、初期化にのみ関連する一時変数およびヘルパー関数に使用できます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-178">This could be used for temporary variables and helper functions that are only relevant to initialization:</span></span>

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

<span data-ttu-id="c52b3-179">特に、アクセシビリティ修飾子がない場合は、`private`を意味します。</span><span class="sxs-lookup"><span data-stu-id="c52b3-179">This may be too subtle, especially since the absence of accessibility modifiers elsewhere simply means `private`.</span></span> 

### <a name="initializer-statements"></a><span data-ttu-id="c52b3-180">初期化子ステートメント</span><span class="sxs-lookup"><span data-stu-id="c52b3-180">Initializer statements</span></span>

<span data-ttu-id="c52b3-181">上の例と拡張機能の組み合わせでは、単にステートメントをクラス本体で直接許可するだけです。</span><span class="sxs-lookup"><span data-stu-id="c52b3-181">A radical combination of the above to extensions would be to simply allow statements directly in the class body.</span></span> <span data-ttu-id="c52b3-182">このようなステートメントは、上記で示したように、厳密に指定されたコンストラクター本体とまったく同じですが、`{ }`で囲む必要がない点が異なります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-182">Such statements are exactly as the interspersed constructor bodies proposed above, except they don't need to be enclosed in `{ }`.</span></span> <span data-ttu-id="c52b3-183">これを十分に活用するには、"ローカル" 変数およびヘルパー関数をクラスの最上位レベルで表現する必要もあります。これについては、上記の拡張機能の "初期化子フィールドと初期化子関数" で調査します。</span><span class="sxs-lookup"><span data-stu-id="c52b3-183">For this to be sufficiently useful, "local" variables and helper functions would need to also be expressible at the top level of the class, in the manner explored in the extension "Initializer fields and initializer functions" above.</span></span>


### <a name="member-access"></a><span data-ttu-id="c52b3-184">メンバー アクセス。</span><span class="sxs-lookup"><span data-stu-id="c52b3-184">Member access</span></span>

<span data-ttu-id="c52b3-185">コア提案では、プライマリコンストラクターパラメーターを、単純な名前としてのみ参照できるパラメーターとして扱います。</span><span class="sxs-lookup"><span data-stu-id="c52b3-185">The core proposal treats primary constructor parameters as parameters that can only be referred as simple names.</span></span> <span data-ttu-id="c52b3-186">別の方法としては、フィールドとして生成されない場合*でも*、メンバーアクセスを使用して、プライベートフィールドであるかのように参照できるようにする方法があります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-186">An alternative is to allow them to be referenced as if they were private fields, i.e. with a member access, *even* if they are sometimes not generated as fields.</span></span> <span data-ttu-id="c52b3-187">これにより、ローカル変数によってシャドウされた場合に `this.x` として参照したり、`other.x`とは異なるインスタンスからアクセスしたりできます。</span><span class="sxs-lookup"><span data-stu-id="c52b3-187">This would allow them to be referenced as `this.x` when shadowed by local variables, and accessed from a different instance as `other.x`.</span></span>

<span data-ttu-id="c52b3-188">"初期化子フィールドおよび初期化子関数" 拡張機能に適用されている場合、通常のプライベートメンバーとは異なる程度になります。</span><span class="sxs-lookup"><span data-stu-id="c52b3-188">If applied to the "initializer fields and initializer functions" extension this would also reduce the degree to which those were different from ordinary private members.</span></span> <span data-ttu-id="c52b3-189">唯一の違いは、初期化中に使用された場合にのみ、コンパイラがオブジェクトから除外できることです。</span><span class="sxs-lookup"><span data-stu-id="c52b3-189">The only difference would then be that the compiler is free to elide them from the object if only used during initialization.</span></span>

