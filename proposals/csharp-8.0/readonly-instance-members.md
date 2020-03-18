---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79484370"
---
# <a name="readonly-instance-members"></a><span data-ttu-id="3e567-101">Readonly インスタンスのメンバー</span><span class="sxs-lookup"><span data-stu-id="3e567-101">Readonly Instance Members</span></span>

<span data-ttu-id="3e567-102">Championed の問題: <https://github.com/dotnet/csharplang/issues/1710></span><span class="sxs-lookup"><span data-stu-id="3e567-102">Championed Issue: <https://github.com/dotnet/csharplang/issues/1710></span></span>

## <a name="summary"></a><span data-ttu-id="3e567-103">まとめ</span><span class="sxs-lookup"><span data-stu-id="3e567-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3e567-104">インスタンスメンバーが状態を変更しないように指定するのと同じ `readonly struct` 方法で、構造体で個別のインスタンスメンバーを指定する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="3e567-104">Provide a way to specify individual instance members on a struct do not modify state, in the same way that `readonly struct` specifies no instance members modify state.</span></span>

<span data-ttu-id="3e567-105">`readonly instance member`! = `pure instance member`であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3e567-105">It is worth noting that `readonly instance member` != `pure instance member`.</span></span> <span data-ttu-id="3e567-106">`pure` インスタンスメンバーは、状態が変更されないことを保証します。</span><span class="sxs-lookup"><span data-stu-id="3e567-106">A `pure` instance member guarantees no state will be modified.</span></span> <span data-ttu-id="3e567-107">`readonly` インスタンスメンバーは、インスタンスの状態が変更されないことのみを保証します。</span><span class="sxs-lookup"><span data-stu-id="3e567-107">A `readonly` instance member only guarantees that instance state will not be modified.</span></span>

<span data-ttu-id="3e567-108">`readonly struct` のインスタンスメンバーはすべて、暗黙的に `readonly instance members`と見なすことができます。</span><span class="sxs-lookup"><span data-stu-id="3e567-108">All instance members on a `readonly struct` could be considered implicitly `readonly instance members`.</span></span> <span data-ttu-id="3e567-109">非 readonly 構造体で宣言された明示的な `readonly instance members` は、同じ方法で動作します。</span><span class="sxs-lookup"><span data-stu-id="3e567-109">Explicit `readonly instance members` declared on non-readonly structs would behave in the same manner.</span></span> <span data-ttu-id="3e567-110">たとえば、インスタンスメンバー (現在のインスタンスまたはインスタンスのフィールド) を呼び出したが、それ自体が非 readonly であった場合でも、非表示のコピーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3e567-110">For example, they would still create hidden copies if you called an instance member (on the current instance or on a field of the instance) which was itself not-readonly.</span></span>

## <a name="motivation"></a><span data-ttu-id="3e567-111">目的</span><span class="sxs-lookup"><span data-stu-id="3e567-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3e567-112">現在、ユーザーは `readonly struct` 型を作成できます。これにより、コンパイラは、すべてのフィールドが readonly であることを強制します。また、拡張によって、インスタンスメンバーが状態を変更することはありません。</span><span class="sxs-lookup"><span data-stu-id="3e567-112">Today, users have the ability to create `readonly struct` types which the compiler enforces that all fields are readonly (and by extension, that no instance members modify the state).</span></span> <span data-ttu-id="3e567-113">ただし、アクセス可能なフィールドを公開する既存の API がある場合や、変化するメンバーと変化しないメンバーが混在している場合があります。</span><span class="sxs-lookup"><span data-stu-id="3e567-113">However, there are some scenarios where you have an existing API that exposes accessible fields or that has a mix of mutating and non-mutating members.</span></span> <span data-ttu-id="3e567-114">このような状況では、型を `readonly` としてマークすることはできません (これは互換性に影響する変更です)。</span><span class="sxs-lookup"><span data-stu-id="3e567-114">Under these circumstances, you cannot mark the type as `readonly` (it would be a breaking change).</span></span>

<span data-ttu-id="3e567-115">これは通常、`in` パラメーターの場合を除き、あまり影響を与えません。</span><span class="sxs-lookup"><span data-stu-id="3e567-115">This normally doesn't have much impact, except in the case of `in` parameters.</span></span> <span data-ttu-id="3e567-116">読み取り専用でない構造体に対して `in` パラメーターを使用すると、呼び出しによって内部状態が変更されないことを保証できないため、コンパイラはインスタンスメンバーの呼び出しごとにパラメーターのコピーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3e567-116">With `in` parameters for non-readonly structs, the compiler will make a copy of the parameter for each instance member invocation, since it cannot guarantee that the invocation does not modify internal state.</span></span> <span data-ttu-id="3e567-117">これにより、構造体を値によって直接渡された場合よりも、多数のコピーとパフォーマンスが悪化する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3e567-117">This can lead to a multitude of copies and worse overall performance than if you had just passed the struct directly by value.</span></span> <span data-ttu-id="3e567-118">例については、 [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)で次のコードを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3e567-118">For an example, see this code on [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span></span>

<span data-ttu-id="3e567-119">非表示のコピーが発生する可能性があるその他のシナリオとして、`static readonly fields` と `literals`があります。</span><span class="sxs-lookup"><span data-stu-id="3e567-119">Some other scenarios where hidden copies can occur include `static readonly fields` and `literals`.</span></span> <span data-ttu-id="3e567-120">将来サポートされている場合、`blittable constants` は同じボートで終了します。つまり、構造体が `readonly`としてマークされていない場合、すべてのユーザーが (インスタンスメンバーの呼び出し時に) 完全コピーを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="3e567-120">If they are supported in the future, `blittable constants` would end up in the same boat; that is they all currently necessitate a full copy (on instance member invocation) if the struct is not marked `readonly`.</span></span>

## <a name="design"></a><span data-ttu-id="3e567-121">デザイン</span><span class="sxs-lookup"><span data-stu-id="3e567-121">Design</span></span>
[design]: #design

<span data-ttu-id="3e567-122">インスタンスメンバーが `readonly` であることをユーザーが指定し、インスタンスの状態を変更しないようにします (もちろん、コンパイラによって実行されるすべての適切な検証を使用します)。</span><span class="sxs-lookup"><span data-stu-id="3e567-122">Allow a user to specify that an instance member is, itself, `readonly` and does not modify the state of the instance (with all the appropriate verification done by the compiler, of course).</span></span> <span data-ttu-id="3e567-123">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3e567-123">For example:</span></span>

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

<span data-ttu-id="3e567-124">Readonly は、アクセサーで `this` が変換されないことを示すために、プロパティアクセサーに適用できます。</span><span class="sxs-lookup"><span data-stu-id="3e567-124">Readonly can be applied to property accessors to indicate that `this` will not be mutated in the accessor.</span></span> <span data-ttu-id="3e567-125">次の例では、これらのアクセサーはメンバーフィールドの状態を変更しますが、メンバーフィールドの値は変更しないため、readonly setter を持ちます。</span><span class="sxs-lookup"><span data-stu-id="3e567-125">The following examples have readonly setters because those accessors modify the state of member field, but do not modify the value of that member field.</span></span>

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

<span data-ttu-id="3e567-126">プロパティの構文に `readonly` が適用されている場合は、すべてのアクセサーが `readonly`ことを意味します。</span><span class="sxs-lookup"><span data-stu-id="3e567-126">When `readonly` is applied to the property syntax, it means that all accessors are `readonly`.</span></span>

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

<span data-ttu-id="3e567-127">Readonly は、包含する型を変化させないアクセサーにのみ適用できます。</span><span class="sxs-lookup"><span data-stu-id="3e567-127">Readonly can only be applied to accessors which do not mutate the containing type.</span></span>

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

<span data-ttu-id="3e567-128">Readonly は、一部の自動実装プロパティに適用できますが、意味のある効果はありません。</span><span class="sxs-lookup"><span data-stu-id="3e567-128">Readonly can be applied to some auto-implemented properties, but it won't have a meaningful effect.</span></span> <span data-ttu-id="3e567-129">コンパイラは、`readonly` キーワードが存在するかどうかにかかわらず、自動実装されたすべての getter を readonly として扱います。</span><span class="sxs-lookup"><span data-stu-id="3e567-129">The compiler will treat all auto-implemented getters as readonly whether or not the `readonly` keyword is present.</span></span>

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

<span data-ttu-id="3e567-130">Readonly は、フィールドに似たイベントではなく、手動で実装されたイベントに適用できます。</span><span class="sxs-lookup"><span data-stu-id="3e567-130">Readonly can be applied to manually-implemented events, but not field-like events.</span></span> <span data-ttu-id="3e567-131">個別のイベントアクセサー (追加/削除) に Readonly を適用することはできません。</span><span class="sxs-lookup"><span data-stu-id="3e567-131">Readonly cannot be applied to individual event accessors (add/remove).</span></span>

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

<span data-ttu-id="3e567-132">その他の構文例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3e567-132">Some other syntax examples:</span></span>

* <span data-ttu-id="3e567-133">式の形式のメンバー: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span><span class="sxs-lookup"><span data-stu-id="3e567-133">Expression bodied members: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span></span>
* <span data-ttu-id="3e567-134">ジェネリック制約: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span><span class="sxs-lookup"><span data-stu-id="3e567-134">Generic constraints: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span></span>

<span data-ttu-id="3e567-135">コンパイラは、通常どおりにインスタンスメンバーを出力し、さらに、インスタンスメンバーが状態を変更しないことを示すコンパイラ認識属性を出力します。</span><span class="sxs-lookup"><span data-stu-id="3e567-135">The compiler would emit the instance member, as usual, and would additionally emit a compiler recognized attribute indicating that the instance member does not modify state.</span></span> <span data-ttu-id="3e567-136">これにより、非表示の `this` パラメーターが `ref T`ではなく `in T` になります。</span><span class="sxs-lookup"><span data-stu-id="3e567-136">This effectively causes the hidden `this` parameter to become `in T` instead of `ref T`.</span></span>

<span data-ttu-id="3e567-137">これにより、コンパイラがコピーを作成しなくても、ユーザーは安全にインスタンスメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="3e567-137">This would allow the user to safely call said instance method without the compiler needing to make a copy.</span></span>

<span data-ttu-id="3e567-138">制限事項は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="3e567-138">The restrictions would include:</span></span>

* <span data-ttu-id="3e567-139">`readonly` 修飾子は、静的メソッド、コンストラクター、またはデストラクターには適用できません。</span><span class="sxs-lookup"><span data-stu-id="3e567-139">The `readonly` modifier cannot be applied to static methods, constructors or destructors.</span></span>
* <span data-ttu-id="3e567-140">`readonly` 修飾子はデリゲートに適用できません。</span><span class="sxs-lookup"><span data-stu-id="3e567-140">The `readonly` modifier cannot be applied to delegates.</span></span>
* <span data-ttu-id="3e567-141">`readonly` 修飾子は、クラスまたはインターフェイスのメンバーには適用できません。</span><span class="sxs-lookup"><span data-stu-id="3e567-141">The `readonly` modifier cannot be applied to members of class or interface.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="3e567-142">短所</span><span class="sxs-lookup"><span data-stu-id="3e567-142">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="3e567-143">現在、`readonly struct` メソッドと同じ欠点があります。</span><span class="sxs-lookup"><span data-stu-id="3e567-143">Same drawbacks as exist with `readonly struct` methods today.</span></span> <span data-ttu-id="3e567-144">一部のコードでは、非表示のコピーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3e567-144">Certain code may still cause hidden copies.</span></span>

## <a name="notes"></a><span data-ttu-id="3e567-145">メモ</span><span class="sxs-lookup"><span data-stu-id="3e567-145">Notes</span></span>
[notes]: #notes

<span data-ttu-id="3e567-146">属性または別のキーワードを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="3e567-146">Using an attribute or another keyword may also be possible.</span></span>

<span data-ttu-id="3e567-147">この提案は、`functional purity` または `constant expressions`(またはその両方) に関連しています (ただし、これらには既存の提案がいくつかあります)。</span><span class="sxs-lookup"><span data-stu-id="3e567-147">This proposal is somewhat related to (but is more a subset of) `functional purity` and/or `constant expressions`, both of which have had some existing proposals.</span></span>
