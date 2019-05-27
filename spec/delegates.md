---
ms.openlocfilehash: 994b22f5375d57cfc4c7537c64345a27ddf3e416
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193886"
---
# <a name="delegates"></a><span data-ttu-id="eb037-101">デリゲート</span><span class="sxs-lookup"><span data-stu-id="eb037-101">Delegates</span></span>

<span data-ttu-id="eb037-102">デリゲートがシナリオの他の言語を有効にする、C、pascal 形式、および Modula--などの関数ポインターとしました。</span><span class="sxs-lookup"><span data-stu-id="eb037-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="eb037-103">ただし、C++ の関数ポインターとは異なりデリゲートはオブジェクト指向では完全に、C++ メンバー関数へのポインターとは異なり、デリゲートにはオブジェクトのインスタンスとメソッドの両方をカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="eb037-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="eb037-104">デリゲートの宣言は、クラスから派生したクラスを定義します。`System.Delegate`します。</span><span class="sxs-lookup"><span data-stu-id="eb037-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="eb037-105">デリゲートのインスタンスでは、呼び出し可能なエンティティと呼ばれますがそれぞれ 1 つまたは複数のメソッドの一覧は、呼び出しリストをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="eb037-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="eb037-106">インスタンスのメソッドを呼び出し可能なエンティティから成るインスタンスとそのインスタンスのメソッド。</span><span class="sxs-lookup"><span data-stu-id="eb037-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="eb037-107">静的メソッドの呼び出し可能なエンティティは、メソッドだけで構成されます。</span><span class="sxs-lookup"><span data-stu-id="eb037-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="eb037-108">適切な一連の引数を含むデリゲート インスタンスを呼び出すと、指定した引数のセットを呼び出すときに、デリゲートの呼び出し可能なエンティティ。</span><span class="sxs-lookup"><span data-stu-id="eb037-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="eb037-109">Delegate インスタンスの興味深く有用なプロパティを把握したりしない気にカプセル化します。 メソッドのクラスこれらのメソッドを互換性のあることが重要な ([デリゲートの宣言](delegates.md#delegate-declarations)) で、デリゲートの型。</span><span class="sxs-lookup"><span data-stu-id="eb037-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="eb037-110">これは、ため、デリゲートを「匿名」の呼び出しに最適です。</span><span class="sxs-lookup"><span data-stu-id="eb037-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="eb037-111">デリゲートの宣言</span><span class="sxs-lookup"><span data-stu-id="eb037-111">Delegate declarations</span></span>

<span data-ttu-id="eb037-112">A *delegate_declaration*は、 *type_declaration* ([入力宣言](namespaces.md#type-declarations)) 新しいデリゲート型を宣言します。</span><span class="sxs-lookup"><span data-stu-id="eb037-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

<span data-ttu-id="eb037-113">同じ修飾子をデリゲート宣言内で複数回のコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="eb037-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="eb037-114">`new`修飾子がデリゲートでのみ許可されている別の種類内で宣言されている場合、そのようなデリゲートを指定しますを非表示に継承されたメンバーを同じ名前で」の説明に従って[new 修飾子](classes.md#the-new-modifier)します。</span><span class="sxs-lookup"><span data-stu-id="eb037-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="eb037-115">`public`、 `protected`、 `internal`、および`private`修飾子はデリゲート型のアクセシビリティを制御します。</span><span class="sxs-lookup"><span data-stu-id="eb037-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="eb037-116">デリゲートの宣言が発生したコンテキストに応じてこれらの修飾子の一部は使用できません ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="eb037-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="eb037-117">デリゲートの型名が*識別子*します。</span><span class="sxs-lookup"><span data-stu-id="eb037-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="eb037-118">省略可能な*formal_parameter_list* 、デリゲートのパラメーターを指定し、 *return_type*デリゲートの戻り値の型を示します。</span><span class="sxs-lookup"><span data-stu-id="eb037-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="eb037-119">省略可能な*variant_type_parameter_list* ([バリアント型パラメーター リスト](interfaces.md#variant-type-parameter-lists))、デリゲート自体の型パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="eb037-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="eb037-120">デリゲート型の戻り値の型はいずれかである必要があります`void`、または出力セーフ ([差異の安全性](interfaces.md#variance-safety))。</span><span class="sxs-lookup"><span data-stu-id="eb037-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="eb037-121">デリゲート型のすべての正式なパラメーター型は、入力の安全なである必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb037-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="eb037-122">また、`out`または`ref`パラメーターの型は出力セーフにもあります。</span><span class="sxs-lookup"><span data-stu-id="eb037-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="eb037-123">注も`out`パラメーターを基になる実行プラットフォームの制限により、入力セーフである必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb037-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="eb037-124">C# のデリゲート型がそれに相当しない構造的に等しい名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="eb037-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="eb037-125">具体的には、同じパラメーターを持つ 2 つの異なるデリゲート型では、一覧表示し、型が異なるデリゲート型と見なされますを返します。</span><span class="sxs-lookup"><span data-stu-id="eb037-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="eb037-126">ただし、2 つの異なるが、構造的に等しいデリゲート型のインスタンスが等しいと見なさ比較可能性があります ([デリゲート等値演算子](expressions.md#delegate-equality-operators))。</span><span class="sxs-lookup"><span data-stu-id="eb037-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="eb037-127">例:</span><span class="sxs-lookup"><span data-stu-id="eb037-127">For example:</span></span>

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

<span data-ttu-id="eb037-128">メソッド`A.M1`と`B.M1`デリゲート型と互換性のある`D1`と`D2`があるため、型とパラメーター リストが同じ戻り値ただし、これらのデリゲート型が解除されるため、2 つのさまざまな種類を、。交換できます。</span><span class="sxs-lookup"><span data-stu-id="eb037-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="eb037-129">メソッド`B.M2`、 `B.M3`、および`B.M4`デリゲート型と互換性がない`D1`と`D2`異なる戻り値の型またはパラメーター リストがあるため、します。</span><span class="sxs-lookup"><span data-stu-id="eb037-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="eb037-130">その他のジェネリック型の宣言のように構築されたデリゲート型を作成する型引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb037-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="eb037-131">パラメーターの型と構築されたデリゲート型の戻り値の型は、構築されたデリゲート型の対応する型引数をデリゲートの宣言で型パラメーターごとの代入することによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="eb037-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="eb037-132">結果として得られる戻り値の型とパラメーターの型は、構築されたデリゲート型と互換性のあるどのような方法の決定に使用されます。</span><span class="sxs-lookup"><span data-stu-id="eb037-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="eb037-133">例:</span><span class="sxs-lookup"><span data-stu-id="eb037-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="eb037-134">メソッド`X.F`デリゲート型と互換性が`Predicate<int>`メソッドと`X.G`デリゲート型と互換性が`Predicate<string>`します。</span><span class="sxs-lookup"><span data-stu-id="eb037-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="eb037-135">デリゲート型を宣言する唯一の方法を使用して、 *delegate_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="eb037-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="eb037-136">デリゲートの型から派生したクラス型は、`System.Delegate`します。</span><span class="sxs-lookup"><span data-stu-id="eb037-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="eb037-137">デリゲート型は暗黙的に`sealed`ので、任意の型をデリゲート型から派生することはできません。</span><span class="sxs-lookup"><span data-stu-id="eb037-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="eb037-138">デリゲート以外のクラス型を派生させるにはも`System.Delegate`します。</span><span class="sxs-lookup"><span data-stu-id="eb037-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="eb037-139">なお`System.Delegate`デリゲートの型自体は、すべてのデリゲート型の派生元クラス型です。</span><span class="sxs-lookup"><span data-stu-id="eb037-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="eb037-140">C# のデリゲートの特別な構文を提供しますインスタンス化および呼び出し。</span><span class="sxs-lookup"><span data-stu-id="eb037-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="eb037-141">インスタンス化を除き、クラスまたはクラスのインスタンスに適用できるすべての操作も適用できますデリゲート クラスまたはインスタンスにそれぞれします。</span><span class="sxs-lookup"><span data-stu-id="eb037-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="eb037-142">特にのメンバーにアクセスすることは、`System.Delegate`通常メンバー アクセス構文を使用しました。</span><span class="sxs-lookup"><span data-stu-id="eb037-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="eb037-143">デリゲートのインスタンスによってカプセル化されるメソッドのセットには、呼び出しリストは呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="eb037-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="eb037-144">デリゲートのインスタンスが作成されたとき ([デリゲートの互換性](delegates.md#delegate-compatibility)) 1 つのメソッドから、そのメソッドをカプセル化し、呼び出しリストには、1 つのエントリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="eb037-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="eb037-145">ただし、2 つの null でないデリゲート インスタンスを組み合わせると、それぞれの呼び出しリストは--の順序で連結左のオペランドから - 右側のオペランドを 2 つ以上のエントリが含まれる新しい呼び出しリストを形成します。</span><span class="sxs-lookup"><span data-stu-id="eb037-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="eb037-146">バイナリを使用してデリゲートを組み合わせる`+`([加算演算子](expressions.md#addition-operator)) と`+=`演算子 ([複合代入](expressions.md#compound-assignment))。</span><span class="sxs-lookup"><span data-stu-id="eb037-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="eb037-147">デリゲートは、バイナリを使用して、デリゲートの組み合わせから削除できる`-`([減算演算子](expressions.md#subtraction-operator)) と`-=`演算子 ([複合代入](expressions.md#compound-assignment))。</span><span class="sxs-lookup"><span data-stu-id="eb037-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="eb037-148">デリゲートが等しいかどうかを比較できます ([デリゲート等値演算子](expressions.md#delegate-equality-operators))。</span><span class="sxs-lookup"><span data-stu-id="eb037-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="eb037-149">次の例は、いくつかのデリゲートのインスタンスを作成し、対応する呼び出しリストします。</span><span class="sxs-lookup"><span data-stu-id="eb037-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

<span data-ttu-id="eb037-150">ときに`cd1`と`cd2`はそれぞれ 1 つのメソッドをカプセル化をインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="eb037-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="eb037-151">ときに`cd3`は 2 つのメソッドの呼び出しリストをインスタンス化が`M1`と`M2`点で、注文します。</span><span class="sxs-lookup"><span data-stu-id="eb037-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="eb037-152">`cd4`呼び出しリストが含まれています`M1`、 `M2`、および`M1`点で、注文します。</span><span class="sxs-lookup"><span data-stu-id="eb037-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="eb037-153">最後に、`cd5`の呼び出しリストが含まれています`M1`、 `M2`、 `M1`、 `M1`、および`M2`点で、注文します。</span><span class="sxs-lookup"><span data-stu-id="eb037-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="eb037-154">(場合によっては、削除も) のデリゲートを組み合わせることの例については、次を参照してください。[デリゲート呼び出し](delegates.md#delegate-invocation)します。</span><span class="sxs-lookup"><span data-stu-id="eb037-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="eb037-155">デリゲートの互換性</span><span class="sxs-lookup"><span data-stu-id="eb037-155">Delegate compatibility</span></span>

<span data-ttu-id="eb037-156">メソッドまたはデリゲート`M`は***互換性のある***デリゲート型で`D`次のすべてに該当する場合。</span><span class="sxs-lookup"><span data-stu-id="eb037-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="eb037-157">`D` `M`パラメーター、および内の各パラメーターの個数が同じ`D`同じ`ref`または`out`との対応するパラメーター修飾子`M`します。</span><span class="sxs-lookup"><span data-stu-id="eb037-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="eb037-158">各値パラメーターの (パラメーターなしで`ref`または`out`修飾子)、id 変換 ([恒等変換](conversions.md#identity-conversion)) または暗黙的な参照変換 ([の暗黙の参照変換](conversions.md#implicit-reference-conversions))パラメーター型から存在`D`で対応するパラメーターの型に`M`します。</span><span class="sxs-lookup"><span data-stu-id="eb037-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="eb037-159">各`ref`または`out`パラメーター、パラメーター入力`D`でパラメーターの型と同じです`M`します。</span><span class="sxs-lookup"><span data-stu-id="eb037-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="eb037-160">戻り値の型からの id 列または暗黙的な参照変換が存在する`M`の戻り値の型を`D`します。</span><span class="sxs-lookup"><span data-stu-id="eb037-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="eb037-161">デリゲートのインスタンス化</span><span class="sxs-lookup"><span data-stu-id="eb037-161">Delegate instantiation</span></span>

<span data-ttu-id="eb037-162">によって、デリゲートのインスタンスが作成された、 *delegate_creation_expression* ([デリゲート作成式](expressions.md#delegate-creation-expressions)) またはデリゲート型に変換します。</span><span class="sxs-lookup"><span data-stu-id="eb037-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="eb037-163">新しく作成されたデリゲートのインスタンスは、いずれかを参照します。</span><span class="sxs-lookup"><span data-stu-id="eb037-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="eb037-164">参照される静的メソッド、 *delegate_creation_expression*、または</span><span class="sxs-lookup"><span data-stu-id="eb037-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="eb037-165">ターゲット オブジェクト (することはできませんが`null`) インスタンス メソッドで参照されていると、 *delegate_creation_expression*、または</span><span class="sxs-lookup"><span data-stu-id="eb037-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="eb037-166">他のデリゲート。</span><span class="sxs-lookup"><span data-stu-id="eb037-166">Another delegate.</span></span>

<span data-ttu-id="eb037-167">例:</span><span class="sxs-lookup"><span data-stu-id="eb037-167">For example:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

<span data-ttu-id="eb037-168">インスタンス化すると、同じターゲット オブジェクトとメソッドを常にデリゲート インスタンス参照してください。</span><span class="sxs-lookup"><span data-stu-id="eb037-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="eb037-169">2 つのデリゲートを組み合わせると、または 1 つは、別の独自の呼び出しリスト持つ新しいデリゲート結果から削除する場合に、注意してください。結合または削除のデリゲートの呼び出しリストが変更されません。</span><span class="sxs-lookup"><span data-stu-id="eb037-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="eb037-170">デリゲートの呼び出し</span><span class="sxs-lookup"><span data-stu-id="eb037-170">Delegate invocation</span></span>

<span data-ttu-id="eb037-171">C#、デリゲートの呼び出しの特別な構文を提供します。</span><span class="sxs-lookup"><span data-stu-id="eb037-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="eb037-172">呼び出しリスト持つにはには、1 つのエントリが含まれています。 null でないデリゲート インスタンスが呼び出されると、同じ引数が与えられているし、参照先と同じ値を返しますでは、1 つのメソッドが呼び出されますメソッドにします。</span><span class="sxs-lookup"><span data-stu-id="eb037-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="eb037-173">(を参照してください[デリゲートの呼び出し](expressions.md#delegate-invocations)デリゲートの呼び出しの詳細についてはします)。そのメソッドを直接呼び出した場合とに、デリゲートが呼び出されるメソッドに例外 catch 句の検索を続行場合は、このようなデリゲートの呼び出し中に例外が発生して、その例外が呼び出されたメソッド内でキャッチされない、委任するメソッドと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="eb037-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="eb037-174">順序で同期的に、呼び出しリストのメソッド呼び出すことによって、呼び出しリスト持つにはには、複数のエントリが含まれています。 デリゲートのインスタンスの呼び出しが行われます。</span><span class="sxs-lookup"><span data-stu-id="eb037-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="eb037-175">そのために呼び出される各メソッドには、同じように、デリゲート インスタンスに指定された引数のセットが渡されます。</span><span class="sxs-lookup"><span data-stu-id="eb037-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="eb037-176">このようなデリゲートの呼び出しには、参照パラメーターが含まれている場合 ([パラメーターを参照して](classes.md#reference-parameters))、同じ変数への参照で各メソッドの呼び出しが行われます呼び出しリスト内の 1 つのメソッドでは、その変数への変更になります。呼び出しリスト メソッドをさらに表示されます。</span><span class="sxs-lookup"><span data-stu-id="eb037-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="eb037-177">デリゲートの呼び出しには、出力パラメーターまたは戻り値が含まれている場合、最終的な値は、一覧の最後のデリゲートの呼び出しから取得されます。</span><span class="sxs-lookup"><span data-stu-id="eb037-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="eb037-178">場合は、このようなデリゲートの呼び出しの処理中に例外が発生して、その例外が呼び出されたメソッド内でキャッチされない、デリゲートを呼び出したメソッドで例外 catch 句の検索を続行して、メソッドの後の部分呼び出しリストには呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="eb037-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="eb037-179">値が null の結果の種類の例外では、デリゲート インスタンスを起動しようとしています。`System.NullReferenceException`します。</span><span class="sxs-lookup"><span data-stu-id="eb037-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="eb037-180">次の例では、インスタンス化、結合、削除、およびデリゲートを呼び出す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="eb037-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

<span data-ttu-id="eb037-181">ステートメントに示すように`cd3 += cd1;`デリゲートで使用できる呼び出しリストを複数回です。</span><span class="sxs-lookup"><span data-stu-id="eb037-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="eb037-182">この場合は、単に呼び出されます 1 回出現ごと。</span><span class="sxs-lookup"><span data-stu-id="eb037-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="eb037-183">このなどの呼び出しリストをそのデリゲートを削除すると、呼び出しリストで、最後に見つかったが実際に削除します。</span><span class="sxs-lookup"><span data-stu-id="eb037-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="eb037-184">最後のステートメントの実行前にすぐに`cd3 -= cd1;`、デリゲート`cd3`空の呼び出しリストを参照します。</span><span class="sxs-lookup"><span data-stu-id="eb037-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="eb037-185">空のリストからデリゲートを削除する (または空のリストから、存在しないデリゲートを削除する) をしようとしていますが、エラーではありません。</span><span class="sxs-lookup"><span data-stu-id="eb037-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="eb037-186">生成される出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="eb037-186">The output produced is:</span></span>

```
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
