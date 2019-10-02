---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704096"
---
# <a name="delegates"></a><span data-ttu-id="f4700-101">デリゲート</span><span class="sxs-lookup"><span data-stu-id="f4700-101">Delegates</span></span>

<span data-ttu-id="f4700-102">デリゲートを使用するとC++、、Pascal、Modula などの他の言語が関数ポインターでアドレス指定されるシナリオを実現できます。</span><span class="sxs-lookup"><span data-stu-id="f4700-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="f4700-103">ただしC++ 、関数ポインターとは異なり、デリゲートは完全なオブジェクト指向C++であり、メンバー関数へのポインターとは異なり、デリゲートはオブジェクトインスタンスとメソッドの両方をカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="f4700-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="f4700-104">デリゲート宣言では、クラスから派生したクラス `System.Delegate` を定義します。</span><span class="sxs-lookup"><span data-stu-id="f4700-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="f4700-105">デリゲートインスタンスは、1つまたは複数のメソッドのリストである呼び出しリストをカプセル化します。各メソッドは、呼び出し可能なエンティティと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="f4700-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="f4700-106">インスタンスメソッドの場合、呼び出し可能なエンティティは、インスタンスと、そのインスタンスのメソッドで構成されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="f4700-107">静的メソッドの場合、呼び出し可能なエンティティはメソッドだけで構成されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="f4700-108">適切な引数のセットを使用してデリゲートインスタンスを呼び出すと、指定された一連の引数を使用してデリゲートの呼び出し可能なエンティティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="f4700-109">デリゲートインスタンスの興味深い便利なプロパティは、カプセル化するメソッドのクラスについて関知しないことです。これらのメソッドは、デリゲートの型と互換性がある ([デリゲート宣言](delegates.md#delegate-declarations)) ことだけが重要です。</span><span class="sxs-lookup"><span data-stu-id="f4700-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="f4700-110">これにより、デリゲートは "匿名" 呼び出しに適しています。</span><span class="sxs-lookup"><span data-stu-id="f4700-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="f4700-111">デリゲート宣言</span><span class="sxs-lookup"><span data-stu-id="f4700-111">Delegate declarations</span></span>

<span data-ttu-id="f4700-112">*Delegate_declaration*は、新しいデリゲート型を宣言する*type_declaration* ([型宣言](namespaces.md#type-declarations)) です。</span><span class="sxs-lookup"><span data-stu-id="f4700-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

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

<span data-ttu-id="f4700-113">デリゲート宣言で同じ修飾子が複数回出現する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="f4700-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="f4700-114">@No__t-0 修飾子は、別の型で宣言されたデリゲートでのみ許可されます。この場合、[新しい修飾子](classes.md#the-new-modifier)で説明されているように、このようなデリゲートは、継承されたメンバーを同じ名前で隠ぺいすることを指定します。</span><span class="sxs-lookup"><span data-stu-id="f4700-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="f4700-115">@No__t-0、`protected`、`internal`、および `private` の各修飾子は、デリゲート型のアクセシビリティを制御します。</span><span class="sxs-lookup"><span data-stu-id="f4700-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="f4700-116">デリゲート宣言が発生したコンテキストによっては、これらの修飾子の一部が許可されない場合があります ([アクセシビリティの宣言](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="f4700-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="f4700-117">デリゲートの型名は*identifier*です。</span><span class="sxs-lookup"><span data-stu-id="f4700-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="f4700-118">省略可能な*formal_parameter_list*は、デリゲートのパラメーターを指定し、[*週*] はデリゲートの戻り値の型を示します。</span><span class="sxs-lookup"><span data-stu-id="f4700-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="f4700-119">省略可能な*variant_type_parameter_list* ([バリアント型パラメーターリスト](interfaces.md#variant-type-parameter-lists)) は、デリゲート自体の型パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="f4700-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="f4700-120">デリゲート型の戻り値の型は、`void` または出力セーフ (差異の[安全性](interfaces.md#variance-safety)) のいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="f4700-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="f4700-121">デリゲート型のすべての仮パラメーター型は、入力セーフである必要があります。</span><span class="sxs-lookup"><span data-stu-id="f4700-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="f4700-122">また、`out` または `ref` パラメーター型も、出力セーフである必要があります。</span><span class="sxs-lookup"><span data-stu-id="f4700-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="f4700-123">基になる実行プラットフォームの制限により、`out` のパラメーターも入力セーフである必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f4700-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="f4700-124">のC#デリゲート型は同じ名前であり、構造的に同等ではありません。</span><span class="sxs-lookup"><span data-stu-id="f4700-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="f4700-125">具体的には、同じパラメーターリストと戻り値の型を持つ2つの異なるデリゲート型は、異なるデリゲート型と見なされます。</span><span class="sxs-lookup"><span data-stu-id="f4700-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="f4700-126">ただし、2つの異なるが構造的に等価なデリゲート型のインスタンスは、等しいと見なされる場合があります ([デリゲート等値演算子](expressions.md#delegate-equality-operators))。</span><span class="sxs-lookup"><span data-stu-id="f4700-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="f4700-127">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f4700-127">For example:</span></span>

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

<span data-ttu-id="f4700-128">@No__t-0 および `B.M1` のメソッドは、同じ戻り値の型とパラメーターリストを持っているため、`D1` と `D2` の両方のデリゲート型と互換性があります。ただし、これらのデリゲート型は2つの異なる型であるため、交換することはできません。</span><span class="sxs-lookup"><span data-stu-id="f4700-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="f4700-129">@No__t-0、`B.M3`、`B.M4` の各メソッドは、異なる戻り値の型またはパラメーターリストを持っているため、デリゲート型 `D1` および `D2` と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="f4700-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="f4700-130">他のジェネリック型宣言と同様に、構築されたデリゲート型を作成するには、型引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f4700-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="f4700-131">構築されたデリゲート型のパラメーターの型と戻り値の型は、デリゲート宣言の型パラメーターごとに、構築されたデリゲート型の対応する型引数に代入することによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="f4700-132">結果の戻り値の型とパラメーターの型は、構築されたデリゲート型と互換性のあるメソッドを決定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="f4700-133">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f4700-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="f4700-134">メソッド `X.F` はデリゲート型 `Predicate<int>` と互換性があり、メソッド `X.G` はデリゲート型 `Predicate<string>` と互換性があります。</span><span class="sxs-lookup"><span data-stu-id="f4700-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="f4700-135">デリゲート型を宣言する唯一の方法は、 *delegate_declaration*を使用することです。</span><span class="sxs-lookup"><span data-stu-id="f4700-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="f4700-136">デリゲート型は `System.Delegate` から派生したクラス型です。</span><span class="sxs-lookup"><span data-stu-id="f4700-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="f4700-137">デリゲート型は暗黙的に @no__t 0 であるため、デリゲート型から型を派生させることはできません。</span><span class="sxs-lookup"><span data-stu-id="f4700-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="f4700-138">また、`System.Delegate` から非デリゲートクラス型を派生させることもできません。</span><span class="sxs-lookup"><span data-stu-id="f4700-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="f4700-139">@No__t-0 はそれ自体がデリゲート型ではないことに注意してください。これは、すべてのデリゲート型の派生元であるクラス型です。</span><span class="sxs-lookup"><span data-stu-id="f4700-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="f4700-140">C#デリゲートのインスタンス化と呼び出しのための特殊な構文を提供します。</span><span class="sxs-lookup"><span data-stu-id="f4700-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="f4700-141">インスタンス化を除き、クラスまたはクラスのインスタンスに適用できる操作は、それぞれデリゲートクラスまたはインスタンスに適用することもできます。</span><span class="sxs-lookup"><span data-stu-id="f4700-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="f4700-142">特に、通常のメンバーアクセス構文を使用して `System.Delegate` 型のメンバーにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="f4700-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="f4700-143">デリゲートインスタンスによってカプセル化されるメソッドのセットは、呼び出しリストと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="f4700-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="f4700-144">デリゲートインスタンスが1つのメソッドから作成 ([デリゲート互換性](delegates.md#delegate-compatibility)) されると、そのメソッドがカプセル化され、その呼び出しリストにはエントリが1つだけ含まれます。</span><span class="sxs-lookup"><span data-stu-id="f4700-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="f4700-145">ただし、null 以外の2つのデリゲートインスタンスを結合すると、2つ以上のエントリを含む新しい呼び出しリストを形成するために、その呼び出しリストが連結されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="f4700-146">デリゲートは、バイナリ `+` ([加算演算子](expressions.md#addition-operator)) と `+=` 演算子 ([複合代入](expressions.md#compound-assignment)) を使用して結合されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="f4700-147">デリゲートは、バイナリ `-` ([減算演算子](expressions.md#subtraction-operator)) と `-=` 演算子 ([複合代入](expressions.md#compound-assignment)) を使用して、デリゲートの組み合わせから削除できます。</span><span class="sxs-lookup"><span data-stu-id="f4700-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="f4700-148">デリゲートは、等価性 ([デリゲート等値演算子](expressions.md#delegate-equality-operators)) を比較できます。</span><span class="sxs-lookup"><span data-stu-id="f4700-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="f4700-149">次の例は、さまざまなデリゲートのインスタンス化と、それらに対応する呼び出しリストを示しています。</span><span class="sxs-lookup"><span data-stu-id="f4700-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

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

<span data-ttu-id="f4700-150">@No__t-0 および `cd2` がインスタンス化されると、それぞれが1つのメソッドをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="f4700-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="f4700-151">@No__t-0 がインスタンス化されると、その順序で `M1` と @no__t 2 の2つのメソッドの呼び出しリストがあります。</span><span class="sxs-lookup"><span data-stu-id="f4700-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="f4700-152">`cd4` の呼び出しリストには、その順序で `M1`、`M2`、および `M1` が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f4700-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="f4700-153">最後に、`cd5` の呼び出しリストには、その順序で `M1`、`M2`、`M1`、`M1`、および `M2` が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f4700-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="f4700-154">デリゲートの結合 (および削除) のその他の例については、「[デリゲートの呼び出し](delegates.md#delegate-invocation)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f4700-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="f4700-155">デリゲートの互換性</span><span class="sxs-lookup"><span data-stu-id="f4700-155">Delegate compatibility</span></span>

<span data-ttu-id="f4700-156">次のすべての条件に該当する場合、メソッドまたはデリゲート `M` はデリゲート @no__t 型と***互換性***があります。</span><span class="sxs-lookup"><span data-stu-id="f4700-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="f4700-157">`D` および `M` のパラメーター数は同じで、`D` の各パラメーターには、`M` の対応するパラメーターと同じ `ref` または `out` の修飾子が指定されています。</span><span class="sxs-lookup"><span data-stu-id="f4700-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="f4700-158">各値パラメーター (@no__t 0 または `out` 修飾子のないパラメーター) については、`D` のパラメーターの型から、id 変換 ([id 変換](conversions.md#identity-conversion)) または暗黙の参照変換 ([暗黙的な参照](conversions.md#implicit-reference-conversions)変換) が存在します。`M` の対応するパラメーターの型。</span><span class="sxs-lookup"><span data-stu-id="f4700-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="f4700-159">@No__t-0 または `out` の各パラメーターについて、`D` のパラメーターの型は `M` のパラメーターの型と同じです。</span><span class="sxs-lookup"><span data-stu-id="f4700-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="f4700-160">@No__t-0 の戻り値の型から `D` の戻り値の型への id または暗黙の参照変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="f4700-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="f4700-161">デリゲートのインスタンス化</span><span class="sxs-lookup"><span data-stu-id="f4700-161">Delegate instantiation</span></span>

<span data-ttu-id="f4700-162">デリゲートのインスタンスは、 *delegate_creation_expression* ([デリゲート作成式](expressions.md#delegate-creation-expressions)) またはデリゲート型への変換によって作成されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="f4700-163">新しく作成されたデリゲートインスタンスは、次のいずれかを参照します。</span><span class="sxs-lookup"><span data-stu-id="f4700-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="f4700-164">*Delegate_creation_expression*で参照される静的メソッド。</span><span class="sxs-lookup"><span data-stu-id="f4700-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="f4700-165">*Delegate_creation_expression*で参照されるターゲットオブジェクト (@no__t 0 にすることはできません) とインスタンスメソッド。</span><span class="sxs-lookup"><span data-stu-id="f4700-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="f4700-166">別のデリゲート。</span><span class="sxs-lookup"><span data-stu-id="f4700-166">Another delegate.</span></span>

<span data-ttu-id="f4700-167">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f4700-167">For example:</span></span>

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

<span data-ttu-id="f4700-168">インスタンス化されると、デリゲートインスタンスは常に同じターゲットオブジェクトとメソッドを参照します。</span><span class="sxs-lookup"><span data-stu-id="f4700-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="f4700-169">2つのデリゲートが結合されている場合、または1つが別のデリゲートから削除された場合は、新しいデリゲートの結果が独自の呼び出しリストになります。結合または削除されたデリゲートの呼び出しリストは変更されません。</span><span class="sxs-lookup"><span data-stu-id="f4700-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="f4700-170">デリゲートの呼び出し</span><span class="sxs-lookup"><span data-stu-id="f4700-170">Delegate invocation</span></span>

<span data-ttu-id="f4700-171">C#デリゲートを呼び出すための特別な構文を提供します。</span><span class="sxs-lookup"><span data-stu-id="f4700-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="f4700-172">呼び出しリストに1つのエントリが含まれている null 以外のデリゲートインスタンスが呼び出されると、指定されたのと同じ引数を使用して1つのメソッドが呼び出され、参照先のメソッドと同じ値が返されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="f4700-173">(デリゲート呼び出しの詳細については、「[デリゲートの呼び出し](expressions.md#delegate-invocations)」を参照してください)。このようなデリゲートの呼び出し中に例外が発生し、その例外が呼び出されたメソッド内でキャッチされない場合、そのメソッドが直接を呼び出したかのように、デリゲートを呼び出したメソッドで例外の catch 句の検索が続行されます。デリゲートが参照されたメソッド。</span><span class="sxs-lookup"><span data-stu-id="f4700-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="f4700-174">呼び出しリストに複数のエントリが含まれているデリゲートインスタンスの呼び出しは、呼び出しリスト内の各メソッドを同期的に順番に呼び出すことによって続行されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="f4700-175">を呼び出す各メソッドには、デリゲートインスタンスに指定されたものと同じ引数セットが渡されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="f4700-176">このようなデリゲート呼び出しに参照パラメーター ([参照パラメーター](classes.md#reference-parameters)) が含まれている場合、各メソッド呼び出しは同じ変数への参照を使用して発生します。呼び出しリストの1つのメソッドによってその変数に加えられた変更は、呼び出しリストの下位にあるメソッドから参照できます。</span><span class="sxs-lookup"><span data-stu-id="f4700-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="f4700-177">デリゲート呼び出しに出力パラメーターまたは戻り値が含まれている場合、最終的な値はリスト内の最後のデリゲートの呼び出しから取得されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="f4700-178">このようなデリゲートの呼び出しの処理中に例外が発生し、呼び出されたメソッド内で例外がキャッチされない場合は、デリゲートを呼び出したメソッドで例外の catch 句を検索し、すべてのメソッドをさらに下に移動します。呼び出しリストは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="f4700-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="f4700-179">値が null のデリゲートインスタンスを呼び出そうとすると、`System.NullReferenceException` 型の例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="f4700-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="f4700-180">次の例は、デリゲートのインスタンス化、結合、削除、および呼び出しを行う方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="f4700-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

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

<span data-ttu-id="f4700-181">ステートメント `cd3 += cd1;` に示されているように、デリゲートは呼び出しリスト内に複数回存在できます。</span><span class="sxs-lookup"><span data-stu-id="f4700-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="f4700-182">この場合、単に1回だけ呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f4700-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="f4700-183">このような呼び出しリストでは、そのデリゲートが削除されると、呼び出しリストで最後に出現したものが実際に削除されたものになります。</span><span class="sxs-lookup"><span data-stu-id="f4700-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="f4700-184">最後のステートメントを実行する直前 (`cd3 -= cd1;`)、デリゲート `cd3` は空の呼び出しリストを参照します。</span><span class="sxs-lookup"><span data-stu-id="f4700-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="f4700-185">空のリストからデリゲートを削除しようとしている (または、空でないリストから存在しないデリゲートを削除する) と、エラーにはなりません。</span><span class="sxs-lookup"><span data-stu-id="f4700-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="f4700-186">生成される出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f4700-186">The output produced is:</span></span>

```console
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
