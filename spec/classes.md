---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703983"
---
# <a name="classes"></a><span data-ttu-id="88a1a-101">クラス</span><span class="sxs-lookup"><span data-stu-id="88a1a-101">Classes</span></span>

<span data-ttu-id="88a1a-102">クラスは、データメンバー (定数とフィールド)、関数メンバー (メソッド、プロパティ、イベント、インデクサー、演算子、インスタンスコンストラクター、デストラクターと静的コンストラクター)、および入れ子にされた型を含むことができるデータ構造です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="88a1a-103">クラス型は、派生クラスが基底クラスを拡張および特殊化できる機構である継承をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="88a1a-104">クラス宣言</span><span class="sxs-lookup"><span data-stu-id="88a1a-104">Class declarations</span></span>

<span data-ttu-id="88a1a-105">*Class_declaration*は、新しいクラスを宣言する*type_declaration* ([型宣言](namespaces.md#type-declarations)) です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="88a1a-106">*Class_declaration*は、省略可能な*属性*のセット ([属性](attributes.md))、オプションの一連の*class_modifier*s ([クラス修飾子](classes.md#class-modifiers))、オプションの `partial` 修飾子、その後に続くキーワードで構成されます。`class` と、クラスに*名前を付け*、省略可能な*type_parameter_list* ([型パラメーター](classes.md#type-parameters))、省略可能な*class_base*仕様 (クラスの[基本仕様](classes.md#class-base-specification))、その後に続けて、省略可能な*type_parameter_constraints_clause*s ([型パラメーター制約](classes.md#type-parameter-constraints)) のセットで、その後に*class_body* ([クラス本体](classes.md#class-body)) が続き、その後にセミコロンが続いています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="88a1a-107">クラス宣言では、 *type_parameter_list*も指定しない限り、 *type_parameter_constraints_clause*を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="88a1a-108">*Type_parameter_list*を提供するクラス宣言は、***ジェネリッククラス宣言***です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="88a1a-109">また、ジェネリッククラスの宣言またはジェネリック構造体の宣言内で入れ子になっているクラスは、それ自体がジェネリッククラスの宣言です。これは、包含する型の型パラメーターを指定して構築された型を作成する必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="88a1a-110">クラス修飾子</span><span class="sxs-lookup"><span data-stu-id="88a1a-110">Class modifiers</span></span>

<span data-ttu-id="88a1a-111">*Class_declaration*には、必要に応じてクラス修飾子のシーケンスを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

<span data-ttu-id="88a1a-112">同じ修飾子がクラス宣言に複数回出現する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="88a1a-113">修飾子`new`は、入れ子になったクラスで許可されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="88a1a-114">このメソッドは、[新しい修飾子](classes.md#the-new-modifier)で説明されているように、クラスが同じ名前で継承されたメンバーを非表示にすることを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="88a1a-115">入れ子になったクラス宣言ではない`new`クラス宣言に修飾子が表示される場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="88a1a-116">、 、、および`public`の`private`各修飾子は、クラスのアクセシビリティを制御します。 `protected` `internal`</span><span class="sxs-lookup"><span data-stu-id="88a1a-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="88a1a-117">クラス宣言が発生したコンテキストによっては、これらの修飾子の一部が許可されない場合があります ([アクセシビリティは宣言](basic-concepts.md#declared-accessibility)されています)。</span><span class="sxs-lookup"><span data-stu-id="88a1a-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="88a1a-118">、、 `sealed` および`static`修飾子については、次のセクションで説明します。 `abstract`</span><span class="sxs-lookup"><span data-stu-id="88a1a-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="88a1a-119">抽象クラス</span><span class="sxs-lookup"><span data-stu-id="88a1a-119">Abstract classes</span></span>

<span data-ttu-id="88a1a-120">`abstract`修飾子は、クラスが不完全であること、および基底クラスとしてのみ使用されることを示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="88a1a-121">抽象クラスは、次のように、非抽象クラスとは異なります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="88a1a-122">抽象クラスを直接インスタンス化することはできません。抽象クラスで`new`演算子を使用する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="88a1a-123">コンパイル時の型が抽象型である変数と値を使用することもできますが、このような変数`null`と値は必ずしも抽象型から派生した非抽象クラスのインスタンスへの参照であるか、または含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="88a1a-124">抽象クラスは、抽象メンバーを含むことができます (必須ではありません)。</span><span class="sxs-lookup"><span data-stu-id="88a1a-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="88a1a-125">抽象クラスをシールドにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="88a1a-126">非抽象クラスが抽象クラスから派生した場合、非抽象クラスには、継承されたすべての抽象メンバーの実際の実装が含まれている必要があります。これにより、抽象メンバーはオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="88a1a-127">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-127">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
<span data-ttu-id="88a1a-128">抽象クラス`A`には、抽象メソッド`F`が導入されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="88a1a-129">クラス`B`には追加の`G`メソッドが導入されていますが`F`、 `B`の実装を提供していないため、抽象としても宣言する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="88a1a-130">クラス`C`は`F`をオーバーライドし、実際の実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="88a1a-131">に`C`は抽象メンバーが存在しない`C`ため、を非抽象にすることができます (必須ではありません)。</span><span class="sxs-lookup"><span data-stu-id="88a1a-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="88a1a-132">シールクラス</span><span class="sxs-lookup"><span data-stu-id="88a1a-132">Sealed classes</span></span>

<span data-ttu-id="88a1a-133">`sealed`修飾子は、クラスからの派生を防ぐために使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="88a1a-134">シールクラスが別のクラスの基底クラスとして指定されている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="88a1a-135">シールクラスを抽象クラスにすることもできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="88a1a-136">`sealed`修飾子は、意図しない派生を防ぐために主に使用されますが、特定の実行時の最適化を有効にすることもできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="88a1a-137">特に、シールクラスは派生クラスを持たないことがわかっているため、シールされたクラスインスタンスでの仮想関数メンバー呼び出しを非仮想呼び出しに変換することができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="88a1a-138">静的クラス</span><span class="sxs-lookup"><span data-stu-id="88a1a-138">Static classes</span></span>

<span data-ttu-id="88a1a-139">修飾子は、***静的クラス***として宣言されているクラスをマークするために使用されます。 `static`</span><span class="sxs-lookup"><span data-stu-id="88a1a-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="88a1a-140">静的クラスはインスタンス化できません。型として使用することはできず、静的メンバーのみを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="88a1a-141">拡張メソッド ([拡張メソッド](classes.md#extension-methods)) の宣言を含めることができるのは、静的クラスだけです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="88a1a-142">静的クラスの宣言には、次の制限があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="88a1a-143">静的クラスには、または`sealed` `abstract`修飾子を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="88a1a-144">ただし、静的クラスはインスタンス化することも、から派生することもできないため、シールドと抽象の両方として動作します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="88a1a-145">静的クラスに*class_base* Specification ([クラスの基本仕様](classes.md#class-base-specification)) を含めることはできません。また、基底クラスまたは実装されたインターフェイスのリストを明示的に指定することもできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="88a1a-146">静的クラスは、型`object`を暗黙的に継承します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="88a1a-147">静的クラスには、静的メンバー ([静的メンバーとインスタンスメンバー](classes.md#static-and-instance-members)) のみを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="88a1a-148">定数および入れ子にされた型は、静的メンバーとして分類されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="88a1a-149">静的クラスは、アクセシビリティを持つ`protected`メンバー `protected internal`または宣言されたアクセシビリティを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="88a1a-150">これらの制限に違反すると、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="88a1a-151">静的クラスには、インスタンスコンストラクターがありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-151">A static class has no instance constructors.</span></span> <span data-ttu-id="88a1a-152">静的クラスでインスタンスコンストラクターを宣言することはできません。また、静的クラスに既定のインスタンスコンストラクター ([既定のコンストラクター](classes.md#default-constructors)) は用意されていません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="88a1a-153">静的クラスのメンバーは自動的に静的ではなく、メンバー宣言に修飾子を`static`明示的に含める必要があります (定数と入れ子にされた型を除く)。</span><span class="sxs-lookup"><span data-stu-id="88a1a-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="88a1a-154">クラスが静的外部クラス内で入れ子になっている場合、入れ子になったクラスは、明示的に`static`修飾子が含まれていない限り、静的クラスではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="88a1a-155">__静的クラス型の参照__</span><span class="sxs-lookup"><span data-stu-id="88a1a-155">__Referencing static class types__</span></span>

<span data-ttu-id="88a1a-156">*Namespace_or_type_name* ([名前空間と型名](basic-concepts.md#namespace-and-type-names)) は、静的クラスを参照することが許可されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="88a1a-157">*Namespace_or_type_name*は、`T.I` の形式の*namespace_or_type_name*の `T` です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="88a1a-158">*Namespace_or_type_name*は、`typeof(T)` の形式の*typeof_expression* ([引数リスト](expressions.md#argument-lists)1) の `T` です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="88a1a-159">*Primary_expression* ([関数メンバー](expressions.md#function-members)) は、静的クラスを参照することが許可されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="88a1a-160">*Primary_expression*は、フォーム`E` のmember_access([動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))の`E.I`です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="88a1a-161">それ以外のコンテキストでは、静的クラスを参照するコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="88a1a-162">たとえば、静的クラスを基底クラス、構成型 (入れ子にされた[型](classes.md#nested-types))、ジェネリック型引数、または型パラメーター制約として使用すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="88a1a-163">同様に、静的クラスは、配列型、ポインター型`new` 、式、キャスト式`is` 、式`sizeof` 、 `as`式、式、または既定値の式では使用できません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="88a1a-164">Partial 修飾子</span><span class="sxs-lookup"><span data-stu-id="88a1a-164">Partial modifier</span></span>

<span data-ttu-id="88a1a-165">@No__t-0 修飾子は、この*class_declaration*が部分的な型宣言であることを示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="88a1a-166">外側の名前空間または型宣言内で同じ名前を持つ複数の部分型宣言を組み合わせると、[部分](classes.md#partial-types)型で指定された規則に従って1つの型宣言が形成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="88a1a-167">クラスの宣言をプログラムテキストの個別のセグメントに分散させると、これらのセグメントが異なるコンテキストで生成または管理される場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="88a1a-168">たとえば、クラス宣言の一部はコンピューターによって生成される場合がありますが、もう一方の部分は手動で作成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="88a1a-169">2つのテキストを分離することで、1つの更新がもう一方の更新と競合するのを防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="88a1a-170">型パラメーター</span><span class="sxs-lookup"><span data-stu-id="88a1a-170">Type parameters</span></span>

<span data-ttu-id="88a1a-171">型パラメーターは、構築された型を作成するために提供される型引数のプレースホルダーを示す単純な識別子です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="88a1a-172">型パラメーターは、後で提供される型の正式なプレースホルダーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="88a1a-173">これに対して、型引数 ([型引数](types.md#type-arguments)) は、構築された型が作成されるときに、型パラメーターの代わりに使用される実際の型です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

<span data-ttu-id="88a1a-174">クラス宣言の各型パラメーターは、そのクラスの宣言空間 ([宣言](basic-concepts.md#declarations)) に名前を定義します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="88a1a-175">そのため、別の型パラメーターまたはそのクラスで宣言されたメンバーと同じ名前を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="88a1a-176">型パラメーターには、型自体と同じ名前を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="88a1a-177">クラスの基本指定</span><span class="sxs-lookup"><span data-stu-id="88a1a-177">Class base specification</span></span>

<span data-ttu-id="88a1a-178">クラス宣言には、クラスの直接基底クラスと、クラスによって直接実装されたインターフェイス ([インターフェイス](interfaces.md)) を定義する*class_base*仕様を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

<span data-ttu-id="88a1a-179">クラス宣言で指定される基底クラスは、構築されたクラス型 ([構築型](types.md#constructed-types)) にすることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="88a1a-180">基底クラスは、独自の型パラメーターにすることはできませんが、スコープ内の型パラメーターを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="88a1a-181">基底クラス</span><span class="sxs-lookup"><span data-stu-id="88a1a-181">Base classes</span></span>

<span data-ttu-id="88a1a-182">*Class_type*が*class_base*に含まれている場合は、宣言されているクラスの直接基底クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="88a1a-183">クラス宣言に*class_base*がない場合、または*class_base*がインターフェイス型のみを一覧表示する場合、直接の基底クラスは `object` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="88a1a-184">クラスは、「[継承](classes.md#inheritance)」で説明されているように、直接基底クラスからメンバーを継承します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="88a1a-185">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="88a1a-186">クラス`A`は、の`B`直接基底クラスと呼ばれ、から`B` `A`派生すると言います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="88a1a-187">で`A`は直接基底クラスが明示的に指定されていないため`object`、直接基底クラスは暗黙的に指定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="88a1a-188">構築されたクラス型の場合、基底クラスがジェネリッククラス宣言で指定されている場合、基底クラスの宣言の各*type_parameter*に対して、対応する type_argument を使用して、構築された型の基底クラスを取得します。構築された型の。</span><span class="sxs-lookup"><span data-stu-id="88a1a-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="88a1a-189">指定されたジェネリッククラス宣言</span><span class="sxs-lookup"><span data-stu-id="88a1a-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="88a1a-190">構築さ`G<int>` `B<string,int[]>`れた型の基本クラスは、です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="88a1a-191">クラス型の直接基底クラスは、少なくともクラス型自体 ([アクセシビリティドメイン](basic-concepts.md#accessibility-domains)) と同じようにアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="88a1a-192">たとえば、 `public`クラスがクラス`private`または`internal`クラスから派生する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="88a1a-193">クラス型の直接基底クラスは`System.Array` `System.MulticastDelegate`、 `System.Delegate`、、、 `System.Enum`、または`System.ValueType`のいずれの型でもない必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="88a1a-194">さらに、ジェネリッククラス宣言では`System.Attribute` 、を直接または間接基底クラスとして使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="88a1a-195">クラス`A` `B` `object`の直接的な基底クラスの指定の意味を判断する際、の直接の基底クラスはとして一時的に使用されます。 `B`</span><span class="sxs-lookup"><span data-stu-id="88a1a-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="88a1a-196">直感的に言えば、基底クラスの指定の意味は、それ自体に再帰的に依存することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="88a1a-197">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="88a1a-198">がエラーになっています。基底`A<C.B>`クラスの指定では`C` `object`、の直接基底クラスはと見なされます。したがって、([名前空間と型名](basic-concepts.md#namespace-and-type-names)の規則によって) メンバー `C` `B`を持つとは見なされません.</span><span class="sxs-lookup"><span data-stu-id="88a1a-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="88a1a-199">クラス型の基底クラスは、直接基底クラスとその基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="88a1a-200">つまり、基本クラスのセットは、直接基底クラスのリレーションシップの推移的なクロージャです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="88a1a-201">上の例を参照してください。の`B`基本`A`クラス`object`はとです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="88a1a-202">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="88a1a-203">`D<int>`の基本クラスは`C<int[]>`、 `B<IComparable<int[]>>` `object`、、、およびです。 `A`</span><span class="sxs-lookup"><span data-stu-id="88a1a-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="88a1a-204">クラス`object`を除き、すべてのクラス型には、直接基底クラスが1つだけあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="88a1a-205">クラス`object`は、直接基底クラスを持たず、他のすべてのクラスの最終的な基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="88a1a-206">クラスがクラス`A`から`A` `B`派生した場合、はに依存するのコンパイル時エラーになります。 `B`</span><span class="sxs-lookup"><span data-stu-id="88a1a-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="88a1a-207">クラスは、直接基底クラス (存在する場合)***に依存***し、そのクラスがすぐに入れ子になっているクラス***に直接依存***します (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="88a1a-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="88a1a-208">この定義により、クラスが依存するクラスの完全なセットは、リレーションシップ***に直接依存***しているの再帰的および推移的なクロージャです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="88a1a-209">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="88a1a-210">は、クラスがそれ自体に依存しているため、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="88a1a-211">同様に、例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="88a1a-212">クラスが循環的に依存しているため、エラーが発生しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="88a1a-213">最後に、例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="88a1a-214">は (直接的な`A`基底クラス`B` ) に依存し`B.C`ています。これは、循環的に依存して`A`いる (すぐ外側のクラス) に依存しているため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="88a1a-215">クラスは、その中に入れ子になっているクラスに依存しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="88a1a-216">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="88a1a-217">`B``A`はに`A`依存しますが (は、直接の基底クラスとそのすぐ外側の`A`クラス) に依存しませ`B`んが、(は基底クラスでも外側の`B` `A`クラスでもないため)に依存しません。).</span><span class="sxs-lookup"><span data-stu-id="88a1a-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="88a1a-218">したがって、この例は有効です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-218">Thus, the example is valid.</span></span>

<span data-ttu-id="88a1a-219">`sealed`クラスから派生することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="88a1a-220">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="88a1a-221">クラス`B`は`sealed`クラスから派生しようとしているため、エラーが発生しています。`A`</span><span class="sxs-lookup"><span data-stu-id="88a1a-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="88a1a-222">インターフェイスの実装</span><span class="sxs-lookup"><span data-stu-id="88a1a-222">Interface implementations</span></span>

<span data-ttu-id="88a1a-223">*Class_base*仕様には、インターフェイス型のリストを含めることができます。この場合、クラスは、指定されたインターフェイス型を直接実装すると言います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="88a1a-224">インターフェイスの実装については、「[インターフェイスの実装](interfaces.md#interface-implementations)」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="88a1a-225">型パラメーターの制約</span><span class="sxs-lookup"><span data-stu-id="88a1a-225">Type parameter constraints</span></span>

<span data-ttu-id="88a1a-226">ジェネリック型およびメソッドの宣言では、 *type_parameter_constraints_clause*s を含めることによって、必要に応じて型パラメーターの制約を指定できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

<span data-ttu-id="88a1a-227">各*type_parameter_constraints_clause*は、トークン `where`、その後に型パラメーターの名前、コロンとその型パラメーターの制約のリストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="88a1a-228">型パラメーターごとに1つ`where`の句を指定できます。 `where`また、句は任意の順序で一覧表示できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="88a1a-229">プロパティアクセサー `set`のおよびトークンと同様に、トークンはキーワードではありません。`where` `get`</span><span class="sxs-lookup"><span data-stu-id="88a1a-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="88a1a-230">`where`句で指定される制約の一覧には、次のいずれかのコンポーネントを含めることができます。この順序は、単一のプライマリ制約、1つ以上`new()`のセカンダリ制約、およびコンストラクター制約です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="88a1a-231">プライマリ制約には、クラス型、***参照型制約*** `class` 、または***値型の制約*** `struct`を指定できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="88a1a-232">セカンダリ制約には、 *type_parameter*または*interface_type*を指定できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="88a1a-233">参照型の制約は、型パラメーターに使用される型引数が参照型である必要があることを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="88a1a-234">すべてのクラス型、インターフェイス型、デリゲート型、配列型、および参照型として知られる型パラメーターは、この制約を満たしています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="88a1a-235">値型の制約は、型パラメーターに使用される型引数が null 非許容の値型である必要があることを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="88a1a-236">Null 非許容の構造体型、列挙型、および値型の制約を持つ型パラメーターは、この制約を満たしています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="88a1a-237">値型として分類されていますが、null 許容型 ([Null 許容](types.md#nullable-types)型) は値型の制約を満たしていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="88a1a-238">値型の制約を持つ型パラメーターにも、 *constructor_constraint*を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="88a1a-239">ポインター型を型引数にすることはできません。また、参照型または値型の制約を満たしているとは見なされません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="88a1a-240">制約がクラス型、インターフェイス型、または型パラメーターである場合、その型は、その型パラメーターに使用されるすべての型引数がをサポートする必要がある最小の "基本型" を指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="88a1a-241">構築された型またはジェネリックメソッドを使用する場合は、コンパイル時に型パラメーターの制約に対して型引数がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="88a1a-242">指定された型引数は、「[制約を満たす](types.md#satisfying-constraints)」で説明されている条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="88a1a-243">*Class_type*制約は、次の規則を満たしている必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="88a1a-244">型はクラス型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-244">The type must be a class type.</span></span>
*  <span data-ttu-id="88a1a-245">型をにすること`sealed`はできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="88a1a-246">型`System.Array`には`System.Delegate` `System.ValueType`、、、、またはのいずれかを指定することはできません。 `System.Enum`</span><span class="sxs-lookup"><span data-stu-id="88a1a-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="88a1a-247">型をにすること`object`はできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-247">The type must not be `object`.</span></span> <span data-ttu-id="88a1a-248">すべての型はから`object`派生するため、このような制約は、許可されている場合は効果がありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="88a1a-249">特定の型パラメーターに対して最大1つの制約をクラス型にすることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="88a1a-250">*Interface_type*制約として指定する型は、次の規則を満たしている必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="88a1a-251">型はインターフェイス型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="88a1a-252">特定`where`の句で1つの型を複数回指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="88a1a-253">どちらの場合も、制約には、関連付けられている型またはメソッドの宣言の型パラメーターを構築型の一部として含めることができ、宣言された型を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="88a1a-254">型パラメーターの制約として指定されたクラスまたはインターフェイスの型は、宣言するジェネリック型またはメソッドと同じように、少なくともアクセス可能な ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints)) 必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="88a1a-255">*Type_parameter*制約として指定する型は、次の規則を満たしている必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="88a1a-256">型は型パラメーターでなければなりません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="88a1a-257">特定`where`の句で1つの型を複数回指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="88a1a-258">さらに、型パラメーターの依存関係グラフに循環がない必要があります。依存関係は、によって定義される推移的な関係です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="88a1a-259">`T`型パラメーター `S` `S`が型パラメーターの制約として使用されている場合、はに依存します。 `T`</span><span class="sxs-lookup"><span data-stu-id="88a1a-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="88a1a-260">`S` `U` `T` `T` `U`型パラメーターが型パラメーターに依存し、型パラメーターに依存している場合、はに依存します。`S`</span><span class="sxs-lookup"><span data-stu-id="88a1a-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="88a1a-261">この関係により、型パラメーターが自身に依存している (直接または間接的に) ことを前提としたコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="88a1a-262">制約は、依存する型パラメーターの間で一貫している必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="88a1a-263">型パラメーター `S`が型パラメーター `T`に依存する場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="88a1a-264">`T`値型の制約を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="88a1a-265">それ以外`T`の場合、は`S`実質的にシールされるため、と`T`同じ型である必要があり、2つの型パラメーターが不要になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="88a1a-266">@No__t-0 に値の型の制約がある場合は、`T` に*class_type*制約を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="88a1a-267">@No__t-0 に*class_type* @no__t 制約が指定されていて、`T` に*class_type* @no__t 制約がある場合は、`A` から @no__t への暗黙的な参照変換またはからの暗黙の参照変換を行う必要があります。`B` を `A` にします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="88a1a-268">@No__t-0 も型パラメーター `U` に依存し、`U` には*class_type* @no__t 制約があります。また、`T` には、 *class_type*制約があります。 @no__t には、id 変換または暗黙の参照変換が必要 `A``B` または 0 から 1 への暗黙の参照変換。</span><span class="sxs-lookup"><span data-stu-id="88a1a-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="88a1a-269">は、値型`S`の`T`制約を持ち、参照型の制約を持つに有効です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="88a1a-270">実質的に`T`は`System.Object` `System.ValueType` 、`System.Enum`、、、および任意のインターフェイス型に制限されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="88a1a-271">型パラメーターの`new()` `new` 句にコンストラクターの制約 (フォームがある) が含まれている場合は、演算子を使用して、型のインスタンス ([オブジェクト作成式](expressions.md#object-creation-expressions)) を作成することができます。`where`</span><span class="sxs-lookup"><span data-stu-id="88a1a-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="88a1a-272">コンストラクター制約のある型パラメーターに使用されるすべての型引数には、パブリックなパラメーターなしのコンストラクターが必要です (このコンストラクターは、任意の値型に対して暗黙的に存在します)。または、値型の制約またはコンストラクターの制約を持つ型パラメーターである必要があります (「詳細については、 [「パラメーターの制約](classes.md#type-parameter-constraints)」を入力してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="88a1a-273">制約の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-273">The following are examples of constraints:</span></span>
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

<span data-ttu-id="88a1a-274">次の例では、型パラメーターの依存関係グラフで循環が発生するため、エラーが発生しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="88a1a-275">次の例は、その他の無効な状況を示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-275">The following examples illustrate additional invalid situations:</span></span>
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

<span data-ttu-id="88a1a-276">型パラメーター `T`の***有効な基本クラス***は、次のように定義されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="88a1a-277">に`T` primary 制約または型パラメーター制約がない場合、その有効な`object`基本クラスはです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="88a1a-278">に`T`値型の制約がある場合、その有効な`System.ValueType`基本クラスはです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="88a1a-279">@No__t-0 の場合、 *class_type*制約 `C` ですが、 *type_parameter*制約はありません。その有効な基本クラスは `C` です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="88a1a-280">@No__t-0 に*class_type*制約がなく、1つ以上の*type_parameter*制約がある場合、その有効な基本クラスは、その種類の有効な基本クラスのセットに含まれる最も内側の型 (リフトされた[変換演算子](conversions.md#lifted-conversion-operators)) になります。 *パラメーター*制約。</span><span class="sxs-lookup"><span data-stu-id="88a1a-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="88a1a-281">一貫性規則により、このような最も包含型が存在することが保証されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="88a1a-282">@No__t-0 に*class_type*制約と1つ以上の*type_parameter*制約の両方が含まれている場合、その有効な基本クラスは、 *class_type*で構成されているセット内の最も包含型 (リフトされた[変換演算子](conversions.md#lifted-conversion-operators)) になります。`T` の制約と、その*type_parameter*制約の有効な基本クラス。</span><span class="sxs-lookup"><span data-stu-id="88a1a-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="88a1a-283">一貫性規則により、このような最も包含型が存在することが保証されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="88a1a-284">@No__t-0 に参照型の制約があり、 *class_type*の制約がない場合、その有効な基本クラスは `object` です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="88a1a-285">これらのルールでは、T に*value_type*である @no__t 0 という制約がある場合は、 *class_type*である @no__t の最も限定的な基本データ型であるを使用します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="88a1a-286">これは明示的に指定された制約では発生しませんが、ジェネリックメソッドの制約が、オーバーライドするメソッドの宣言またはインターフェイスメソッドの明示的な実装によって暗黙的に継承される場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="88a1a-287">これらの規則は、有効な基本クラスが常に*class_type*であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="88a1a-288">型パラメーター `T`の***有効なインターフェイスセット***は、次のように定義されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="88a1a-289">@No__t-0 に*secondary_constraints*がない場合、その有効なインターフェイスセットは空になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="88a1a-290">@No__t-0 の場合、 *interface_type*制約はありますが、 *type_parameter*制約はありません。その有効なインターフェイスセットは、一連の*interface_type*制約です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="88a1a-291">@No__t-0 に*interface_type*制約がなく、 *type_parameter*制約がある場合、その有効なインターフェイスセットは、その*type_parameter*制約の有効なインターフェイスセットの和集合になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="88a1a-292">@No__t-0 に*interface_type*制約と*type_parameter*制約の両方が含まれている場合、その有効なインターフェイスセットは、 *interface_type*制約のセットとその*type_parameter*の有効なインターフェイスセットの和集合になります。constraints.</span><span class="sxs-lookup"><span data-stu-id="88a1a-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="88a1a-293">参照型の制約がある場合、またはその有効な基本クラスがまたは`System.ValueType`でない`object`場合、型パラメーターは***参照型で***あると認識されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="88a1a-294">制約付きの型パラメーター型の値を使用して、制約によって暗黙的に指定されたインスタンスメンバーにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="88a1a-295">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-295">In the example</span></span>
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
<span data-ttu-id="88a1a-296">の`IPrintable`メソッドは、常にを実装`x` `IPrintable`する`T`ように制約されているため、で直接呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="88a1a-297">クラス本体</span><span class="sxs-lookup"><span data-stu-id="88a1a-297">Class body</span></span>

<span data-ttu-id="88a1a-298">クラスの*class_body*は、そのクラスのメンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="88a1a-299">部分型</span><span class="sxs-lookup"><span data-stu-id="88a1a-299">Partial types</span></span>

<span data-ttu-id="88a1a-300">型宣言は、複数の***部分型宣言***にまたがって分割できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="88a1a-301">型宣言は、このセクションの規則に従ってパートから構築されます。とは、プログラムのコンパイル時および実行時の処理中に、単一の宣言として扱われます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="88a1a-302">*Class_declaration*、 *struct_declaration* 、または*interface_declaration*は、`partial` 修飾子が含まれている場合、部分型宣言を表します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="88a1a-303">`partial`はキーワードで`class`はなく、キーワード`struct`の1つの直前、また`interface`は型宣言内、またはメソッド宣言内の型`void`の前に出現する場合にのみ、修飾子として機能します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="88a1a-304">その他のコンテキストでは、通常の識別子として使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="88a1a-305">部分型の宣言の各部分には、 `partial`修飾子を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="88a1a-306">同じ名前を持ち、他の部分と同じ名前空間または型宣言で宣言されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="88a1a-307">修飾子`partial`は、型宣言の追加部分が別の場所に存在する可能性があることを示しますが、そのような追加の部分の存在は要件ではありません`partial` 。修飾子を含む1つの宣言を持つ型に対して有効です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="88a1a-308">部分型のすべての部分を一緒にコンパイルして、コンパイル時にパートを1つの型宣言にマージできるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="88a1a-309">部分型では、コンパイル済みの型を拡張することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="88a1a-310">入れ子になった型は、 `partial`修飾子を使用して複数の部分で宣言できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="88a1a-311">通常、含んでいる型はを`partial`使用して宣言され、入れ子にされた型の各部分は、含んでいる型の別の部分で宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="88a1a-312">修飾子`partial`は、デリゲートまたは列挙型の宣言では許可されていません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="88a1a-313">属性</span><span class="sxs-lookup"><span data-stu-id="88a1a-313">Attributes</span></span>

<span data-ttu-id="88a1a-314">部分型の属性は、各部分の属性を指定しない順序で組み合わせることによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="88a1a-315">属性が複数の部分に配置されている場合は、その型に対して属性を複数回指定することと同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="88a1a-316">たとえば、次の2つの部分があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="88a1a-317">は、次のような宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="88a1a-318">型パラメーターの属性は、同様の方法で結合されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="88a1a-319">修飾子</span><span class="sxs-lookup"><span data-stu-id="88a1a-319">Modifiers</span></span>

<span data-ttu-id="88a1a-320">部分型の宣言に`public`アクセシビリティの指定 ( `internal`、 `protected`、、および`private`修飾子) が含まれている場合、アクセシビリティの仕様を含むその他すべての部分に同意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="88a1a-321">部分型の一部にアクセシビリティの指定が含まれていない場合、型には、適切な既定のアクセシビリティ (宣言された[アクセシビリティ](basic-concepts.md#declared-accessibility)) が与えられます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="88a1a-322">入れ子になった型の1つ以上の部分宣言`new`に修飾子が含まれている場合、入れ子にされた型が継承されたメンバーを隠ぺいする ([継承によって隠ぺい](basic-concepts.md#hiding-through-inheritance)される) 場合、警告は報告されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="88a1a-323">クラスの1つ以上の部分宣言に`abstract`修飾子が含まれている場合、クラスは抽象 ([抽象クラス](classes.md#abstract-classes)) と見なされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="88a1a-324">それ以外の場合、クラスは非抽象であると見なされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="88a1a-325">クラスの1つ以上の部分宣言に`sealed`修飾子が含まれている場合、クラスは sealed ([シールクラス](classes.md#sealed-classes)) と見なされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="88a1a-326">それ以外の場合、クラスはシールされていないと見なされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="88a1a-327">クラスに abstract と sealed の両方を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="88a1a-328">部分型宣言で修飾子を使用すると、その特定の部分だけがunsafeコンテキスト([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts))と見なされ`unsafe`ます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="88a1a-329">型パラメーターと制約</span><span class="sxs-lookup"><span data-stu-id="88a1a-329">Type parameters and constraints</span></span>

<span data-ttu-id="88a1a-330">ジェネリック型が複数の部分で宣言されている場合、各部分は型パラメーターを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="88a1a-331">各部分には、同じ数の型パラメーターと、それぞれの型パラメーターに対して同じ名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="88a1a-332">部分的なジェネリック型の宣言に制約`where` (句) が含まれている場合、制約は、制約を含むその他すべての部分と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="88a1a-333">具体的には、制約を含む各部分は、同じ型パラメーターのセットに対する制約を持つ必要があります。また、各型パラメーターには、primary、secondary、およびコンストラクターの各制約のセットが同等である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="88a1a-334">同じメンバーが含まれている場合、2つの制約セットは同等です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="88a1a-335">部分的なジェネリック型の一部で型パラメーターの制約が指定されていない場合、型パラメーターは制約なしと見なされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="88a1a-336">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-336">The example</span></span>
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
<span data-ttu-id="88a1a-337">は、制約を含む部分 (最初の2つ) が、同じ型パラメーターのセットに対して同じ primary、secondary、およびコンストラクターの制約のセットを効果的に指定するため、正しいです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="88a1a-338">基本クラス</span><span class="sxs-lookup"><span data-stu-id="88a1a-338">Base class</span></span>

<span data-ttu-id="88a1a-339">部分クラスの宣言に基底クラスの指定が含まれている場合、基底クラスの指定を含む他のすべての部分に同意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="88a1a-340">部分クラスの一部に基底クラスの指定が含まれていない場合、 `System.Object`基本クラスは ([基底クラス](classes.md#base-classes)) になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="88a1a-341">基本インターフェイス</span><span class="sxs-lookup"><span data-stu-id="88a1a-341">Base interfaces</span></span>

<span data-ttu-id="88a1a-342">複数の部分で宣言された型の基本インターフェイスのセットは、各部分で指定された基本インターフェイスの和集合です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="88a1a-343">特定の基本インターフェイスには、各部分で1回だけ名前を付けることができますが、複数の部分で同じ基本インターフェイスに名前を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="88a1a-344">特定の基本インターフェイスのメンバーの実装は1つだけ必要です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="88a1a-345">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="88a1a-346">クラス`C`の基本インターフェイスのセットは`IA`、 `IB`、、および`IC`です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="88a1a-347">通常、各部分は、その部分で宣言されたインターフェイスの実装を提供します。ただし、これは要件ではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="88a1a-348">パートは、別の部分で宣言されたインターフェイスの実装を提供する場合があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-348">A part may provide the implementation for an interface declared on a different part:</span></span>
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a><span data-ttu-id="88a1a-349">メンバー</span><span class="sxs-lookup"><span data-stu-id="88a1a-349">Members</span></span>

<span data-ttu-id="88a1a-350">部分メソッド ([部分メソッド](classes.md#partial-methods)) を除き、複数の部分で宣言された型のメンバーのセットは、単に各部分で宣言されたメンバーのセットの和集合になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="88a1a-351">型宣言のすべての部分の本体は同じ宣言空間 ([宣言](basic-concepts.md#declarations)) を共有し、各メンバー ([スコープ](basic-concepts.md#scopes)) のスコープはすべての部分の本体に拡張されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="88a1a-352">メンバーのアクセシビリティドメインには、それを囲む型のすべての部分が常に含まれます。ある`private`パートで宣言されたメンバーは、別のパートから自由にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="88a1a-353">型の複数の部分で同じメンバーを宣言する場合、そのメンバーが`partial`修飾子を持つ型である場合を除き、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

<span data-ttu-id="88a1a-354">型内のメンバーの順序付けは、コードにC#とってはあまり重要ではありませんが、他の言語や環境とのやり取りには大きな意味があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="88a1a-355">このような場合、複数の部分で宣言された型内のメンバーの順序は未定義です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="88a1a-356">部分メソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-356">Partial methods</span></span>

<span data-ttu-id="88a1a-357">部分メソッドは、型宣言の1つの部分で定義し、別ので実装することができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="88a1a-358">実装は省略可能です。部分メソッドを実装している部分がない場合は、部分メソッドの宣言とその部分のすべての呼び出しが、その部分の組み合わせによって生成される型宣言から削除されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="88a1a-359">部分メソッドでは、アクセス修飾子は定義でき`private`ませんが、暗黙的に指定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="88a1a-360">戻り値の型は`void`である必要があり、パラメーター `out`に修飾子を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="88a1a-361">識別子`partial`は、型の`void`直前に出現する場合にのみ、メソッド宣言内で特殊なキーワードとして認識されます。それ以外の場合は、通常の識別子として使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="88a1a-362">部分メソッドは、インターフェイスメソッドを明示的に実装することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="88a1a-363">部分メソッド宣言には、次の2種類があります。メソッド宣言の本体がセミコロンである場合、宣言は***部分メソッド宣言を定義***すると言います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="88a1a-364">本文が*ブロック*として指定されている場合、宣言は***部分メソッド宣言の実装***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="88a1a-365">型宣言の一部では、特定のシグネチャを持つ部分メソッド宣言を1つだけ定義できます。また、特定のシグネチャを持つ実装部分メソッド宣言は1つだけです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="88a1a-366">実装部分メソッド宣言が指定されている場合は、対応する部分メソッド宣言を定義する必要があり、宣言は次のように指定されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="88a1a-367">宣言には、同じ修飾子 (必ずしも同じ順序ではありません)、メソッド名、型パラメーターの数、およびパラメーターの数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="88a1a-368">宣言内の対応するパラメーターは、同じ修飾子を持つ必要があります (必ずしも同じ順序ではありません)。また、型パラメーター名のモジュロが異なる場合もあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="88a1a-369">宣言内の対応する型パラメーターは、同じ制約を持つ必要があります (型パラメーター名の剰余の違い)。</span><span class="sxs-lookup"><span data-stu-id="88a1a-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="88a1a-370">部分メソッド宣言の実装は、対応する部分メソッド宣言を定義するのと同じ部分に記述できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="88a1a-371">オーバーロードの解決に参加するのは、部分メソッドの定義のみです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="88a1a-372">したがって、実装宣言が指定されているかどうかにかかわらず、呼び出し式は部分メソッドの呼び出しに解決される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="88a1a-373">部分メソッドは常にを`void`返すため、このような呼び出し式は常に式ステートメントになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="88a1a-374">さらに、部分メソッドは暗黙的`private`に使用されるため、このようなステートメントは、部分メソッドが宣言されている型宣言のいずれかの部分で常に発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="88a1a-375">部分型宣言の一部に、特定の部分メソッドの実装宣言が含まれていない場合、その部分メソッドを呼び出す式ステートメントは、結合された型宣言から単純に削除されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="88a1a-376">したがって、構成式を含む呼び出し式は、実行時に効果がありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="88a1a-377">部分メソッド自体も削除され、結合された型宣言のメンバーにはなりません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="88a1a-378">実装宣言が特定の部分メソッドに存在する場合は、部分メソッドの呼び出しが保持されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="88a1a-379">部分メソッドは、次の点を除いて、部分メソッド宣言の実装に似たメソッド宣言になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="88a1a-380">`partial`修飾子は含まれていません</span><span class="sxs-lookup"><span data-stu-id="88a1a-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="88a1a-381">結果として得られるメソッドの宣言の属性は、定義の属性と実装部分メソッドの宣言を、指定されていない順序で結合したものです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="88a1a-382">重複は削除されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="88a1a-383">結果として得られるメソッド宣言のパラメーターの属性は、定義の対応するパラメーターの属性と、部分メソッドの実装を指定しない順序での実装の属性を組み合わせたものです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="88a1a-384">重複は削除されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-384">Duplicates are not removed.</span></span>

<span data-ttu-id="88a1a-385">実装宣言ではなく、定義宣言が部分メソッド M に対して指定されている場合、次の制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="88a1a-386">メソッドへのデリゲート ([デリゲート作成式](expressions.md#delegate-creation-expressions)) を作成するためのコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="88a1a-387">式ツリー型に変換される匿名関数`M`内で参照するコンパイル時エラーです ([式ツリー型への匿名関数変換の評価](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="88a1a-388">の`M`呼び出しの一部として出現する式は、確実な代入の状態 ([明確な代入](variables.md#definite-assignment)) には影響しないため、コンパイル時エラーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="88a1a-389">`M`アプリケーションのエントリポイントにすることはできません ([アプリケーションの起動](basic-concepts.md#application-startup))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="88a1a-390">部分メソッドは、1つの型宣言の一部を使用して、ツールによって生成された別のパートの動作をカスタマイズできるようにする場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="88a1a-391">次の部分クラス宣言について考えてみます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-391">Consider the following partial class declaration:</span></span>
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

<span data-ttu-id="88a1a-392">このクラスが他の部分なしでコンパイルされた場合、定義部分メソッド宣言とその呼び出しは削除され、結果として得られるクラス宣言は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

<span data-ttu-id="88a1a-393">ただし、部分メソッドの実装宣言を提供する別のパートが指定されているとします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

<span data-ttu-id="88a1a-394">次に、結果として得られる結合クラスの宣言は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a><span data-ttu-id="88a1a-395">名前のバインド</span><span class="sxs-lookup"><span data-stu-id="88a1a-395">Name binding</span></span>

<span data-ttu-id="88a1a-396">拡張可能な型の各部分は同じ名前空間内で宣言する必要がありますが、通常、各部分は異なる名前空間宣言内に記述されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="88a1a-397">したがって、 `using`各部分に異なるディレクティブ ([ディレクティブを使用](namespaces.md#using-directives)) が存在する場合があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="88a1a-398">1つの部分で単純な名前 ([型の推定](expressions.md#type-inference)) を`using`解釈する場合、その部分を囲む名前空間宣言のディレクティブだけが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="88a1a-399">これにより、異なる部分で異なる意味を持つ同じ識別子が生成される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-399">This may result in the same identifier having different meanings in different parts:</span></span>
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a><span data-ttu-id="88a1a-400">クラス メンバー</span><span class="sxs-lookup"><span data-stu-id="88a1a-400">Class members</span></span>

<span data-ttu-id="88a1a-401">クラスのメンバーは、その*class_member_declaration*によって導入されたメンバーと、直接基底クラスから継承されたメンバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

<span data-ttu-id="88a1a-402">クラス型のメンバーは、次のカテゴリに分類されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="88a1a-403">定数。クラス ([定数](classes.md#constants)) に関連付けられている定数値を表します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="88a1a-404">フィールド。クラス ([フィールド](classes.md#fields)) の変数です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="88a1a-405">メソッド。クラスによって実行できる計算とアクションを実装します ([メソッド](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="88a1a-406">プロパティ。名前付きの特性、およびこれらの特性 ([プロパティ](classes.md#properties)) の読み取りと書き込みに関連するアクションを定義します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="88a1a-407">イベント。クラス ([イベント](classes.md#events)) によって生成される通知を定義します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="88a1a-408">インデクサーは、クラスのインスタンスが配列 ([インデクサー](classes.md#indexers)) と同じ方法で (構文的に) インデックスを作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="88a1a-409">演算子。クラスのインスタンスに適用できる式演算子を定義します ([演算子](classes.md#operators))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="88a1a-410">クラスのインスタンスを初期化するために必要なアクションを実装するインスタンスコンストラクター ([インスタンスコンストラクター](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="88a1a-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="88a1a-411">デストラクターは、クラスのインスタンスが完全に破棄 ([デストラクター](classes.md#destructors)) される前に実行されるアクションを実装します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="88a1a-412">静的コンストラクター。クラス自体を初期化するために必要なアクション ([静的コンストラクター](classes.md#static-constructors)) を実装します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="88a1a-413">型。クラス (入れ子にされた[型](classes.md#nested-types)) に対してローカルな型を表します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="88a1a-414">実行可能コードを含むことができるメンバーは、クラス型の*関数メンバー*と総称されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="88a1a-415">クラス型の関数メンバーは、そのクラス型のメソッド、プロパティ、イベント、インデクサー、演算子、インスタンスコンストラクター、デストラクター、および静的コンストラクターです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="88a1a-416">*Class_declaration*は新しい宣言空間 ([宣言](basic-concepts.md#declarations)) を作成し、 *class_declaration*にすぐに含まれる*class_member_declaration*は、この宣言空間に新しいメンバーを導入します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="88a1a-417">*Class_member_declaration*s には、次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="88a1a-418">インスタンスコンストラクター、デストラクター、および静的コンストラクターには、すぐ外側のクラスと同じ名前を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="88a1a-419">他のすべてのメンバーには、すぐに外側のクラスの名前とは異なる名前を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="88a1a-420">定数、フィールド、プロパティ、イベント、または型の名前は、同じクラスで宣言されている他のすべてのメンバーの名前と異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="88a1a-421">メソッドの名前は、同じクラスで宣言されている他のすべての非メソッドの名前と異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="88a1a-422">さらに、メソッドのシグネチャ ([シグネチャとオーバーロード](basic-concepts.md#signatures-and-overloading)) は、同じクラス内で宣言されている他のすべてのメソッドのシグネチャと異なる必要があります。また、同じクラスで宣言され`ref`た2つのメソッドは、とだけが異なるシグネチャを持つことができません。`out`.</span><span class="sxs-lookup"><span data-stu-id="88a1a-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="88a1a-423">インスタンスコンストラクターのシグネチャは、同じクラスで宣言されている他のすべてのインスタンスコンストラクターのシグネチャと異なる必要があります。また、同じクラスで宣言された`ref` 2 `out`つのコンストラクターは、とだけが異なるシグネチャを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="88a1a-424">インデクサーのシグネチャは、同じクラスで宣言されている他のすべてのインデクサーのシグネチャと異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="88a1a-425">演算子のシグネチャは、同じクラスで宣言されている他のすべての演算子のシグネチャと異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="88a1a-426">クラス型 ([継承](classes.md#inheritance)) の継承されたメンバーは、クラスの宣言空間の一部ではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="88a1a-427">したがって、派生クラスでは、継承されたメンバーと同じ名前またはシグネチャを持つメンバーを宣言できます (これにより、継承されたメンバーは非表示になります)。</span><span class="sxs-lookup"><span data-stu-id="88a1a-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="88a1a-428">インスタンスの種類</span><span class="sxs-lookup"><span data-stu-id="88a1a-428">The instance type</span></span>

<span data-ttu-id="88a1a-429">各クラス宣言には、バインドされた型 (バインドされた型とバインドされていない[型](types.md#bound-and-unbound-types))、***インスタンス型***があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="88a1a-430">ジェネリッククラス宣言では、型宣言から構築された型 ([構築](types.md#constructed-types)型) を作成し、指定された各型引数を対応する型パラメーターとして、インスタンス型を作成します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="88a1a-431">インスタンス型は型パラメーターを使用するため、型パラメーターがスコープ内にある場合にのみ使用できます。つまり、クラス宣言の内部にあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="88a1a-432">インスタンス型は、クラス宣言内`this`で記述されたコードのの型です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="88a1a-433">非ジェネリッククラスの場合、インスタンスの型は単に宣言されたクラスになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="88a1a-434">次に、いくつかのクラス宣言とそのインスタンス型を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="88a1a-435">構築された型のメンバー</span><span class="sxs-lookup"><span data-stu-id="88a1a-435">Members of constructed types</span></span>

<span data-ttu-id="88a1a-436">構築された型の非継承メンバーは、メンバー宣言内の各*type_parameter* (構築された型の対応する*type_argument* ) に対して、置換によって取得されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="88a1a-437">置換プロセスは、型宣言のセマンティックの意味に基づいており、単なるテキスト置換ではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="88a1a-438">たとえば、ジェネリッククラス宣言があるとします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="88a1a-439">構築され`Gen<int[],IComparable<string>>`た型には、次のメンバーがあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="88a1a-440">ジェネリッククラス宣言`Gen`内の`a`メンバーの型は " `T`2 次元配列" です。そのため、上記の構築された`a`型のメンバーの型は、次の"1次元配列の2次元配列`int`"、また`int[,][]`は。</span><span class="sxs-lookup"><span data-stu-id="88a1a-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="88a1a-441">インスタンス関数のメンバー内では、 `this`の型は、包含する宣言のインスタンスの型 ([インスタンス型](classes.md#the-instance-type)) です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="88a1a-442">ジェネリッククラスのすべてのメンバーは、外側の任意のクラスの型パラメーターを直接または構築された型の一部として使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="88a1a-443">実行時に特定のクローズ構築型 ([オープン型およびクローズ型](types.md#open-and-closed-types)) を使用する場合は、型パラメーターを使用するたびに、構築された型に渡される実際の型引数に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="88a1a-444">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-444">For example:</span></span>
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a><span data-ttu-id="88a1a-445">継承</span><span class="sxs-lookup"><span data-stu-id="88a1a-445">Inheritance</span></span>

<span data-ttu-id="88a1a-446">クラスは、その直接の基底クラス型のメンバーを***継承***します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="88a1a-447">継承とは、基底クラスのインスタンスコンストラクター、デストラクター、および静的コンストラクターを除く、クラスに直接基底クラス型のすべてのメンバーが暗黙的に含まれることを意味します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="88a1a-448">継承の重要な側面は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="88a1a-449">継承は推移的です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-449">Inheritance is transitive.</span></span> <span data-ttu-id="88a1a-450">が`C`から`B` `C` `A`派生し`B`ており、がから`A`派生している場合、は、で宣言されたメンバーと、で宣言されたメンバーを継承します。 `B`</span><span class="sxs-lookup"><span data-stu-id="88a1a-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="88a1a-451">派生クラスは、その直接の基底クラスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="88a1a-452">派生クラスは、継承するメンバーに新しいメンバーを追加できますが、継承されたメンバーの定義を削除することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="88a1a-453">インスタンスコンストラクター、デストラクター、および静的コンストラクターは継承されませんが、宣言されたアクセシビリティ ([メンバーアクセス](basic-concepts.md#member-access)) に関係なく、他のすべてのメンバーはになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="88a1a-454">ただし、宣言されたアクセシビリティによっては、継承されたメンバーが派生クラスでアクセスできない場合があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="88a1a-455">派生クラスは、同じ名前またはシグネチャを持つ新しいメンバーを宣言することで、継承されたメンバー***を隠ぺい (*** [隠ぺい](basic-concepts.md#hiding-through-inheritance)) できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="88a1a-456">ただし、継承されたメンバーを非表示にしても、そのメンバーは削除されません。つまり、派生クラスを通じてそのメンバーに直接アクセスできないだけです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="88a1a-457">クラスのインスタンスには、クラスとその基本クラスで宣言されたすべてのインスタンスフィールドのセットが含まれています。また、派生クラス型から基本クラス型への暗黙的な変換 ([暗黙的な参照変換](conversions.md#implicit-reference-conversions)) も存在します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="88a1a-458">したがって、派生クラスのインスタンスへの参照は、その基底クラスのインスタンスへの参照として処理できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="88a1a-459">クラスは、仮想メソッド、プロパティ、インデクサーを宣言でき、派生クラスはこれらの関数メンバーの実装をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="88a1a-460">これにより、クラスは、関数メンバー呼び出しによって実行されるアクションが、その関数メンバーの呼び出しに使用されるインスタンスのランタイム型によって異なる場合に、ポリモーフィックな動作を実現できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="88a1a-461">構築されたクラス型の継承メンバーは、直接の基底クラス型 ([基底クラス](classes.md#base-classes)) のメンバーです。これは、の*対応する型パラメーターが出現するたびに、構築された型の型引数を置き換えることによって検出されます。class_base*の仕様。</span><span class="sxs-lookup"><span data-stu-id="88a1a-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="88a1a-462">これらのメンバーは、メンバー宣言内の*type_parameter*ごとに、 *class_base*仕様の対応する*type_argument*によって、置換によって変換されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

<span data-ttu-id="88a1a-463">上の例では、構築さ`D<int>`れた型には、 `public int G(string s)`型パラメーター `T`の型引数`int`を置き換えることによって取得される非継承メンバーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="88a1a-464">`D<int>`には、クラス宣言`B`から継承されたメンバーもあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="88a1a-465">この継承されたメンバーは、基本クラスの指定`B<int[]>` `B<T[]>`で`D<int>`をに`int` `T`代入することによって、最初にの基底クラスの型を決定することによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="88a1a-466">`B`次に`U` `public U F(long index)`、の型引数として、 `public int[] F(long index)`はのに置き換えられ、継承されたメンバーが生成されます。`int[]`</span><span class="sxs-lookup"><span data-stu-id="88a1a-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="88a1a-467">新しい修飾子</span><span class="sxs-lookup"><span data-stu-id="88a1a-467">The new modifier</span></span>

<span data-ttu-id="88a1a-468">*Class_member_declaration*は、継承されたメンバーと同じ名前またはシグネチャを持つメンバーを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="88a1a-469">この場合、派生クラスのメンバーは、基底クラスのメンバーを***非表示***にすると言います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="88a1a-470">継承されたメンバーを非表示にすることは、エラーとは見なされませんが、コンパイラによって警告が発行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="88a1a-471">警告を抑制するために、派生クラスメンバーの宣言に修飾子を`new`含めて、派生メンバーが基本メンバーを非表示にすることを示すことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="88a1a-472">このトピックの詳細については、「[継承による非表示](basic-concepts.md#hiding-through-inheritance)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="88a1a-473">継承されたメンバーを非表示にしない宣言に修飾子が含まれている場合、その効果に対する警告が発行されます。`new`</span><span class="sxs-lookup"><span data-stu-id="88a1a-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="88a1a-474">この警告は、修飾子を削除`new`することによって抑制されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="88a1a-475">アクセス修飾子</span><span class="sxs-lookup"><span data-stu-id="88a1a-475">Access modifiers</span></span>

<span data-ttu-id="88a1a-476">*Class_member_declaration*は、宣言された[アクセシビリティ (@no__t](basic-concepts.md#declared-accessibility)-2、`protected internal`、`protected`、`internal`、または @no__t) の5種類のうちいずれか1つを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="88a1a-477">`protected internal`この組み合わせを除き、複数のアクセス修飾子を指定するコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="88a1a-478">*Class_member_declaration*にアクセス修飾子が含まれていない場合、`private` が想定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="88a1a-479">構成型</span><span class="sxs-lookup"><span data-stu-id="88a1a-479">Constituent types</span></span>

<span data-ttu-id="88a1a-480">メンバーの宣言で使用される型は、そのメンバーの構成型と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="88a1a-481">使用可能な構成型は、定数、フィールド、プロパティ、イベント、またはインデクサーの型、メソッドまたは演算子の戻り値の型、およびメソッド、インデクサー、演算子、またはインスタンスコンストラクターのパラメーターの型です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="88a1a-482">メンバーの構成型は、少なくともそのメンバー自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="88a1a-483">静的メンバーとインスタンスメンバー</span><span class="sxs-lookup"><span data-stu-id="88a1a-483">Static and instance members</span></span>

<span data-ttu-id="88a1a-484">クラスのメンバーは、***静的メンバー***または***インスタンスメンバー***です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="88a1a-485">一般に、静的メンバーは、オブジェクト (クラス型のインスタンス) に属しているクラス型およびインスタンスメンバーに属していると考えると便利です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="88a1a-486">フィールド、メソッド、プロパティ、イベント、演算子、またはコンストラクターの宣言に`static`修飾子が含まれている場合は、静的メンバーを宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="88a1a-487">また、定数または型の宣言は、暗黙的に静的メンバーを宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="88a1a-488">静的メンバーには次の特性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="88a1a-489">静的メンバー `M` が、`E.M` の形式の*member_access* ([メンバーアクセス](expressions.md#member-access)) で参照されている場合、`E` は `M` を含む型を示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="88a1a-490">は、インスタンスを表すために、 `E`のコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="88a1a-491">静的フィールドは、指定された closed クラス型のすべてのインスタンスによって共有されるストレージの場所を1つだけ識別します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="88a1a-492">指定されたクローズクラス型のインスタンスの数に関係なく、静的フィールドのコピーは1つしか存在しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="88a1a-493">静的関数メンバー (メソッド、プロパティ、イベント、演算子、またはコンストラクター) は、特定のインスタンスでは動作せず、このような関数メンバー `this`で参照するコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="88a1a-494">フィールド、メソッド、プロパティ、イベント、インデクサー、コンストラクター、またはデストラクターの宣言に`static`修飾子が含まれていない場合は、インスタンスメンバーが宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="88a1a-495">(インスタンスメンバーは非静的メンバーと呼ばれることもあります)。インスタンスメンバーには次の特性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="88a1a-496">インスタンスメンバー `M` が、`E.M` の形式の*member_access* ([メンバーアクセス](expressions.md#member-access)) で参照されている場合、`E` は `M` を含む型のインスタンスを示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="88a1a-497">これは、が型を示すため`E`のバインド時エラーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="88a1a-498">クラスのすべてのインスタンスには、クラスのすべてのインスタンスフィールドの個別のセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="88a1a-499">インスタンス関数メンバー (メソッド、プロパティ、インデクサー、インスタンスコンストラクター、またはデストラクター) は、クラスの特定のインスタンスで動作します。このインスタンスには`this` 、([このアクセス](expressions.md#this-access)) としてアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="88a1a-500">次の例は、静的メンバーとインスタンスメンバーにアクセスするための規則を示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-500">The following example illustrates the rules for accessing static and instance members:</span></span>
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

<span data-ttu-id="88a1a-501">@No__t 0 メソッドは、インスタンス関数メンバーで*simple_name* ([簡易名](expressions.md#simple-names)) を使用して、インスタンスメンバーと静的メンバーの両方にアクセスできることを示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="88a1a-502">@No__t 0 メソッドは、静的関数メンバーで、 *simple_name*を介してインスタンスメンバーにアクセスするときにコンパイル時にエラーになることを示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="88a1a-503">@No__t 0 メソッドは、 *member_access* ([メンバーアクセス](expressions.md#member-access)) で、インスタンスメンバーにインスタンスからアクセスする必要があり、静的メンバーには型を使用してアクセスする必要があることを示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="88a1a-504">入れ子にされた型</span><span class="sxs-lookup"><span data-stu-id="88a1a-504">Nested types</span></span>

<span data-ttu-id="88a1a-505">クラスまたは構造体の宣言内で宣言された型は、入れ子にされた***型***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="88a1a-506">コンパイル単位または名前空間内で宣言された型は、入れ子になってい***ない型***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="88a1a-507">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-507">In the example</span></span>
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
<span data-ttu-id="88a1a-508">クラスはクラス`A`内で宣言されているため、入れ子に`A`された型です。クラスはコンパイル単位内で宣言されているため、入れ子になっていない型です。 `B`</span><span class="sxs-lookup"><span data-stu-id="88a1a-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="88a1a-509">完全修飾名</span><span class="sxs-lookup"><span data-stu-id="88a1a-509">Fully qualified name</span></span>

<span data-ttu-id="88a1a-510">入れ子にされた型の完全修飾名 ([完全修飾](basic-concepts.md#fully-qualified-names)名`S.N` ) `S`はです。ここで、は、型`N`が宣言されている型の完全修飾名です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="88a1a-511">アクセシビリティの宣言</span><span class="sxs-lookup"><span data-stu-id="88a1a-511">Declared accessibility</span></span>

<span data-ttu-id="88a1a-512">入れ子になっていない`public`型`internal`は、アクセシビリティを`internal`持つことも宣言することもでき、既定ではアクセシビリティが宣言されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="88a1a-513">入れ子にされた型には、これらの形式のアクセシビリティを含めることができます。また、包含する型がクラスまたは構造体であるかどうかによって、宣言されたアクセシビリティの1つ以上の追加の形式を使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="88a1a-514">クラスで宣言された入れ子にされた型は、5つの形式の`public`アクセシビリティ`protected internal`( `protected`、 `internal`、、 `private`、または) を持つことができ、 `private`他のクラスメンバーと同様に既定で宣言されます。アクセシビリティ.</span><span class="sxs-lookup"><span data-stu-id="88a1a-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="88a1a-515">構造体で宣言された入れ子にされた型は、3つの形式`public`の`internal`アクセシビリティ ( `private`、、または) を持つことができ`private` 、他の構造体のメンバーと同様に、既定ではアクセシビリティが宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="88a1a-516">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-516">The example</span></span>
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
<span data-ttu-id="88a1a-517">入れ子になったプライベート`Node`クラスを宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="88a1a-518">非表示</span><span class="sxs-lookup"><span data-stu-id="88a1a-518">Hiding</span></span>

<span data-ttu-id="88a1a-519">入れ子にされた型では、基本メンバーを非表示にすることができます ([名前の非](basic-concepts.md#name-hiding)表示)。</span><span class="sxs-lookup"><span data-stu-id="88a1a-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="88a1a-520">修飾子`new`は入れ子になった型宣言で許可されているため、非表示を明示的に表すことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="88a1a-521">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-521">The example</span></span>
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
<span data-ttu-id="88a1a-522">で`M` `M` 定義さ`Base`れているメソッドを非表示にする、入れ子になったクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="88a1a-523">このアクセス権</span><span class="sxs-lookup"><span data-stu-id="88a1a-523">this access</span></span>

<span data-ttu-id="88a1a-524">入れ子にされた型とそれを含んでいる型には、 *this_access* ([このアクセス](expressions.md#this-access)) に関して特別な関係はありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="88a1a-525">具体的に`this`は、入れ子にされた型内では、包含する型のインスタンスメンバーを参照するために使用できません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="88a1a-526">入れ子にされた型が、それを含んでいる型のインスタンスメンバーにアクセスする必要がある場合`this`は、入れ子にされた型のコンストラクター引数として、包含する型のインスタンスのを指定することにより、アクセスを提供できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="88a1a-527">次の例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-527">The following example</span></span>
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
<span data-ttu-id="88a1a-528">この手法を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-528">shows this technique.</span></span> <span data-ttu-id="88a1a-529">の`C`インスタンスは、の`Nested`インスタンスを作成し、その`this`インスタンス`Nested`のインスタンスメンバーへ`C`の後続のアクセスを提供するために独自のコンストラクターを渡します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="88a1a-530">包含する型のプライベートメンバーとプロテクトメンバーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="88a1a-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="88a1a-531">入れ子になった型は、その型にアクセスできるすべてのメンバーにアクセスできます。これには、アクセシビリティを`private`持つ`protected`包含型のメンバーも含まれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="88a1a-532">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-532">The example</span></span>
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
<span data-ttu-id="88a1a-533">入れ子になっ`C`たクラス`Nested`を含むクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="88a1a-534">内`Nested`では、 `G`メソッドはで定義`F`され`C`た静的`F`メソッドを呼び出し、プライベートで宣言されたアクセシビリティを持ちます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="88a1a-535">入れ子になった型は、それを含んでいる型の基本型で定義されているプロテクトメンバーにもアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="88a1a-536">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-536">In the example</span></span>
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
<span data-ttu-id="88a1a-537">入れ子になっ`Derived.Nested`たクラスは、 `F`の`Base` `Derived`インスタンス`Derived`を介してを呼び出すことによって、の基本クラスで定義されているプロテクトメソッドにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="88a1a-538">ジェネリッククラスの入れ子にされた型</span><span class="sxs-lookup"><span data-stu-id="88a1a-538">Nested types in generic classes</span></span>

<span data-ttu-id="88a1a-539">ジェネリッククラスの宣言には、入れ子にされた型宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="88a1a-540">外側のクラスの型パラメーターは、入れ子にされた型内で使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="88a1a-541">入れ子になった型宣言には、入れ子になった型にのみ適用される追加の型パラメーターを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="88a1a-542">ジェネリッククラスの宣言内に含まれるすべての型宣言は、暗黙的にジェネリック型宣言になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="88a1a-543">ジェネリック型内で入れ子にされた型への参照を書き込む場合は、その型引数を含む構築型の型に名前を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="88a1a-544">ただし、外側のクラス内では、入れ子にされた型は修飾なしで使用できます。外側のクラスのインスタンスの型は、入れ子にされた型を構築するときに暗黙的に使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="88a1a-545">次の例は、から`Inner`作成された構築済みの型を参照するための3つの異なる正しい方法を示しています。最初の2つは等価です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

<span data-ttu-id="88a1a-546">無効なプログラミングスタイルですが、入れ子にされた型の型パラメーターは、外側の型で宣言されたメンバーまたは型パラメーターを隠ぺいすることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="88a1a-547">予約されたメンバー名</span><span class="sxs-lookup"><span data-stu-id="88a1a-547">Reserved member names</span></span>

<span data-ttu-id="88a1a-548">基になるC#ランタイム実装を容易にするために、プロパティ、イベント、またはインデクサーであるソースメンバーの宣言ごとに、実装は、メンバー宣言の種類、名前、および型に基づいて、2つのメソッドシグネチャを予約する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="88a1a-549">基になるランタイム実装でこれらの予約が使用されていない場合でも、シグネチャがこれらの予約済み署名のいずれかに一致するメンバーを宣言するプログラムでは、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="88a1a-550">予約された名前は宣言を導入しないため、メンバーの検索には関与しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="88a1a-551">ただし、宣言に関連付けられた予約済みメソッドシグネチャは継承 ([継承](classes.md#inheritance)) に参加し、 `new`修飾子 ([new 修飾子](classes.md#the-new-modifier)) で非表示にすることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="88a1a-552">これらの名前の予約は、次の3つの目的で機能します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="88a1a-553">基になる実装が、 C#言語機能への get または set アクセスのために、通常の識別子をメソッド名として使用できるようにする場合は。</span><span class="sxs-lookup"><span data-stu-id="88a1a-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="88a1a-554">C#言語機能へのアクセスを取得または設定するためのメソッド名として、他の言語が通常の識別子を使用して相互運用できるようにする場合は。</span><span class="sxs-lookup"><span data-stu-id="88a1a-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="88a1a-555">1つの準拠コンパイラによって受け入れられるソースが、すべてC#の実装で一貫して予約されたメンバー名の詳細を持つようにするために、別のコンパイラによって受け入れられるようにします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="88a1a-556">デストラクター ([デストラクター](classes.md#destructors)) の宣言によって、シグネチャも予約されます (デストラクター用に予約された[メンバー名](classes.md#member-names-reserved-for-destructors))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="88a1a-557">プロパティ用に予約されたメンバー名</span><span class="sxs-lookup"><span data-stu-id="88a1a-557">Member names reserved for properties</span></span>

<span data-ttu-id="88a1a-558">型`P` の`T`プロパティ ([プロパティ](classes.md#properties)) の場合、次のシグネチャが予約されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="88a1a-559">どちらの署名も、プロパティが読み取り専用または書き込み専用であっても予約されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="88a1a-560">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-560">In the example</span></span>
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
<span data-ttu-id="88a1a-561">クラス`A`は、読み取り専用プロパティ`P`を定義します。これに`get_P`より`set_P` 、メソッドとメソッドの署名が予約されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="88a1a-562">クラス`B`はから`A`派生し、これらの予約済み署名の両方を非表示にします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="88a1a-563">この例では、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-563">The example produces the output:</span></span>
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="88a1a-564">イベント用に予約されたメンバー名</span><span class="sxs-lookup"><span data-stu-id="88a1a-564">Member names reserved for events</span></span>

<span data-ttu-id="88a1a-565">デリゲート型`E` のイベント([イベント](classes.md#events))の場合、次のシグネチャが予約されてい`T`ます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="88a1a-566">インデクサーに予約されたメンバー名</span><span class="sxs-lookup"><span data-stu-id="88a1a-566">Member names reserved for indexers</span></span>

<span data-ttu-id="88a1a-567">パラメーターリスト`T` を持つ型のインデクサー([インデクサー](classes.md#indexers))の場合、次のシグネチャが予約されてい`L`ます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="88a1a-568">インデクサーが読み取り専用または書き込み専用の場合でも、両方の署名が予約されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="88a1a-569">さらに、メンバー `Item`名は予約されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="88a1a-570">デストラクター用に予約されたメンバー名</span><span class="sxs-lookup"><span data-stu-id="88a1a-570">Member names reserved for destructors</span></span>

<span data-ttu-id="88a1a-571">デストラクター ([デストラクター](classes.md#destructors)) を含むクラスでは、次のシグネチャが予約されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="88a1a-572">定数</span><span class="sxs-lookup"><span data-stu-id="88a1a-572">Constants</span></span>

<span data-ttu-id="88a1a-573">***定数***は、定数値を表すクラスメンバーです。この値は、コンパイル時に計算できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="88a1a-574">*Constant_declaration*は、指定された型の1つ以上の定数を導入します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="88a1a-575">*Constant_declaration*には、一連の*属性*([属性](attributes.md))、@no__t 3 修飾子 ([新しい修飾子](classes.md#the-new-modifier))、および4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="88a1a-576">属性と修飾子は、 *constant_declaration*によって宣言されたすべてのメンバーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="88a1a-577">定数は静的メンバーと見なされますが、 *constant_declaration*修飾子を必要とすることも、許可 @no__t することもできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="88a1a-578">同じ修飾子が定数宣言で複数回出現する場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="88a1a-579">*Constant_declaration*の*型*は、宣言によって導入されるメンバーの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="88a1a-580">型の後に*constant_declarator*のリストが続き、それぞれに新しいメンバーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="88a1a-581">*Constant_declarator*は、メンバーに名前を付け、その後に "`=`" トークンを続け、その後にメンバーの値を指定する*constant_expression* ([定数式](expressions.md#constant-expressions)) を付けた*識別子*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="88a1a-582">定数宣言で指定された*型*は、`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、1、`char`、0、4、 *enum_type*、または reference_ である必要があります。 *「* 」と入力します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="88a1a-583">各*constant_expression*は、暗黙的な変換 ([暗黙的](conversions.md#implicit-conversions)な変換) によって対象の型に変換できる、ターゲット型または型の値を生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="88a1a-584">定数の*型*は、少なくとも定数自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="88a1a-585">定数の値は、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバーアクセス](expressions.md#member-access)) を使用して式で取得されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="88a1a-586">定数は、 *constant_expression*に参加することができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="88a1a-587">したがって、定数は、 *constant_expression*を必要とするすべてのコンストラクトで使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="88a1a-588">このような構成体`case`の例`goto case`とし`enum`ては、ラベル、ステートメント、メンバー宣言、属性、およびその他の定数宣言があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="88a1a-589">「[定数式](expressions.md#constant-expressions)」で説明されているように、 *constant_expression*はコンパイル時に完全に評価できる式です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="88a1a-590">@No__t-1 以外の*reference_type*の null 以外の値を作成する唯一の方法は、`new` 演算子を適用することであり、`new` 演算子は*constant_expression*では許可されていないため、の*定数に使用できる値は*`string` 以外の reference_type は `null` です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="88a1a-591">定数値のシンボル名が必要なときに、その値の型が定数宣言で許可されていない場合、またはコンパイル時に*constant_expression*によって値を計算できない場合は、`readonly` フィールド ([Readonly フィールド](classes.md#readonly-fields)) を指定できます。代わりに、を使用してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="88a1a-592">複数の定数を宣言する定数宣言は、同じ属性、修飾子、および型を持つ単一の定数の複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="88a1a-593">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="88a1a-594">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="88a1a-595">依存関係が循環的な性質を持たない限り、定数は同じプログラム内の他の定数に依存することができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="88a1a-596">コンパイラは、定数宣言を適切な順序で評価するように自動的に配置します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="88a1a-597">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-597">In the example</span></span>
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
<span data-ttu-id="88a1a-598">コンパイラは、まず`A.Y`を評価し`B.Z`、次に評価`A.X`して、最後に`11`評価し`12`、値`10`、、およびを生成します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="88a1a-599">定数宣言は、他のプログラムの定数に依存する場合がありますが、このような依存関係は一方向でしか使用できません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="88a1a-600">前の例を参照して`A` 、 `B`とが別々のプログラムで宣言されている場合は、 `B.Z`がに`B.Z`依存している可能性`A.X`が`A.Y`ありますが、同時にに依存することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="88a1a-601">フィールド</span><span class="sxs-lookup"><span data-stu-id="88a1a-601">Fields</span></span>

<span data-ttu-id="88a1a-602">***フィールド***は、オブジェクトまたはクラスに関連付けられた変数を表すメンバーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="88a1a-603">*Field_declaration*は、指定された型の1つ以上のフィールドを導入します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="88a1a-604">*Field_declaration*には、一連の*属性*([属性](attributes.md))、@no__t 3 修飾子 ([新しい修飾子](classes.md#the-new-modifier))、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせ、および `static` 修飾子 ([静的フィールドおよびインスタンスフィールド](classes.md#static-and-instance-fields))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="88a1a-605">また、 *field_declaration*には、`readonly` 修飾子 ([Readonly フィールド](classes.md#readonly-fields)) または `volatile` 修飾子 ([Volatile フィールド](classes.md#volatile-fields)) を含めることができますが、両方を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="88a1a-606">属性と修飾子は、 *field_declaration*によって宣言されたすべてのメンバーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="88a1a-607">同じ修飾子がフィールド宣言に複数回出現する場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="88a1a-608">*Field_declaration*の*型*は、宣言によって導入されるメンバーの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="88a1a-609">型の後に*variable_declarator*のリストが続き、それぞれに新しいメンバーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="88a1a-610">*Variable_declarator*は、そのメンバーに名前を付けた*識別子*で構成され、必要に応じて、そのメンバーの初期値を指定する "`=`" トークンと*variable_initializer* ([変数初期化子](classes.md#variable-initializers)) を続けます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="88a1a-611">フィールドの*型*は、少なくともフィールド自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="88a1a-612">フィールドの値は、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバーアクセス](expressions.md#member-access)) を使用して式で取得されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="88a1a-613">非読み取り専用フィールドの値は、*代入*[演算子 (代入演算子](expressions.md#assignment-operators)) を使用して変更されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="88a1a-614">非 readonly フィールドの値は、後置インクリメント演算子とデクリメント演算子 ([後置インクリメントおよびデクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)) と前置インクリメントおよびデクリメント演算子 ([前置インクリメントおよびデクリメント) を使用して取得および変更できます。演算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="88a1a-615">複数のフィールドを宣言するフィールド宣言は、同じ属性、修飾子、および型を持つ1つのフィールドの複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="88a1a-616">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="88a1a-617">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="88a1a-618">静的フィールドとインスタンスフィールド</span><span class="sxs-lookup"><span data-stu-id="88a1a-618">Static and instance fields</span></span>

<span data-ttu-id="88a1a-619">フィールド宣言に`static`修飾子が含まれている場合、宣言によって導入されるフィールドは***静的フィールド***です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="88a1a-620">修飾子が`static`存在しない場合、宣言によって導入されるフィールドは***インスタンスフィールド***になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="88a1a-621">C#静的フィールドおよびインスタンスフィールドは、によってサポートされるいくつかの種類の変数 ([変数](variables.md)) のうち、それぞれ***静的変数***と***インスタンス変数***として参照される場合があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="88a1a-622">静的フィールドは、特定のインスタンスの一部ではありません。代わりに、閉じられた型のすべてのインスタンス間で共有されます ([オープン型およびクローズ型](types.md#open-and-closed-types))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="88a1a-623">クローズされたクラス型のインスタンスの数に関係なく、関連付けられているアプリケーションドメインの静的フィールドのコピーは1つしかありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="88a1a-624">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-624">For example:</span></span>
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

<span data-ttu-id="88a1a-625">インスタンスフィールドはインスタンスに属します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="88a1a-626">具体的には、クラスのすべてのインスタンスには、そのクラスのすべてのインスタンスフィールドのセットが個別に含まれています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="88a1a-627">フィールドが `E.M` の形式の*member_access* ([メンバーアクセス](expressions.md#member-access)) で参照されている場合、`M` が静的フィールドである場合、`E` は `M` を含む型を示す必要があり、`M` がインスタンスフィールドの場合、E は、を含む型のインスタンスを示す必要があります。`M`。</span><span class="sxs-lookup"><span data-stu-id="88a1a-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="88a1a-628">静的メンバーとインスタンスメンバーの違いについては、「[静的メンバーとインスタンス](classes.md#static-and-instance-members)メンバー」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="88a1a-629">読み取り専用フィールド</span><span class="sxs-lookup"><span data-stu-id="88a1a-629">Readonly fields</span></span>

<span data-ttu-id="88a1a-630">*Field_declaration*に `readonly` 修飾子が含まれている場合、宣言によって導入されるフィールドは***読み取り専用フィールド***です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="88a1a-631">読み取り専用フィールドへの直接割り当ては、その宣言の一部として、または同じクラスのインスタンスコンストラクターまたは静的コンストラクターでのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="88a1a-632">(読み取り専用フィールドは、これらのコンテキストで複数回割り当てることができます)。具体的には、 `readonly`フィールドへの直接割り当ては、次のコンテキストでのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="88a1a-633">(宣言に*variable_initializer*を含めることにより) フィールドを導入する*variable_declarator* 。</span><span class="sxs-lookup"><span data-stu-id="88a1a-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="88a1a-634">インスタンスフィールドの場合、フィールド宣言を含むクラスのインスタンスコンストラクター。静的フィールドの場合は、フィールド宣言を含むクラスの静的コンストラクター。</span><span class="sxs-lookup"><span data-stu-id="88a1a-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="88a1a-635">また、フィールドを`readonly`パラメーターまたは`ref`パラメーターとして渡すことができるのは、これらのコンテキストだけです。 `out`</span><span class="sxs-lookup"><span data-stu-id="88a1a-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="88a1a-636">`readonly`フィールドに割り当てたり、他のコンテキストでパラメーター `out`または`ref`パラメーターとして渡したりしようとすると、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="88a1a-637">静的読み取り専用フィールドを定数に使用する</span><span class="sxs-lookup"><span data-stu-id="88a1a-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="88a1a-638">フィールドは、定数値のシンボル名が必要なときに、値の型が`const`宣言で許可されていない場合、またはコンパイル時に値を計算できない場合に便利です。 `static readonly`</span><span class="sxs-lookup"><span data-stu-id="88a1a-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="88a1a-639">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-639">In the example</span></span>
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
<span data-ttu-id="88a1a-640">`Black` `const` `White` 、、`Green`、、および`Blue`の各メンバーは、コンパイル時に値を計算できないため、メンバーとして宣言することはできません。 `Red`</span><span class="sxs-lookup"><span data-stu-id="88a1a-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="88a1a-641">ただし、それら`static readonly`を宣言すると、ほとんど同じ効果があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="88a1a-642">定数と静的な読み取り専用フィールドのバージョン管理</span><span class="sxs-lookup"><span data-stu-id="88a1a-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="88a1a-643">定数および読み取り専用フィールドには、異なるバイナリバージョン管理セマンティクスがあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="88a1a-644">式が定数を参照する場合、定数の値はコンパイル時に取得されますが、式が読み取り専用フィールドを参照する場合、フィールドの値は実行時まで取得されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="88a1a-645">2つの独立したプログラムで構成されるアプリケーションを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="88a1a-645">Consider an application that consists of two separate programs:</span></span>
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

<span data-ttu-id="88a1a-646">名前`Program1`空間`Program2`と名前空間は、個別にコンパイルされる2つのプログラムを表します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="88a1a-647">は`Program1.Utils.X` 、静的な読み取り専用フィールドとして宣言され`Console.WriteLine`ているため、コンパイル時には、ステートメントによって出力される値は認識されませんが、実行時に取得されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="88a1a-648">したがって`X` 、の値を変更して再コンパイル`Console.WriteLine`すると`Program1` 、が再コンパイルされてい`Program2`ない場合でも、ステートメントは新しい値を出力します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="88a1a-649">ただし、 `X`が定数であった場合、の`X`値はコンパイル時`Program2`に取得され、が再コンパイルさ`Program1`れるまで`Program2`の変更による影響を受けません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="88a1a-650">Volatile フィールド</span><span class="sxs-lookup"><span data-stu-id="88a1a-650">Volatile fields</span></span>

<span data-ttu-id="88a1a-651">*Field_declaration*に `volatile` 修飾子が含まれている場合、その宣言によって導入されるフィールドは***volatile フィールド***です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="88a1a-652">非 volatile フィールドの場合、命令を並べ替える最適化手法は、予期しない、予測できない結果になる可能性があります。マルチスレッドプログラムでは、 *lock_statement*によって提供される[もの (lock ステートメント](statements.md#the-lock-statement))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="88a1a-653">これらの最適化は、コンパイラ、ランタイムシステム、またはハードウェアによって実行できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="88a1a-654">Volatile フィールドの場合、このような並べ替えの最適化は制限されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="88a1a-655">Volatile フィールドの読み取りは、 ***volatile 読み取り***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="88a1a-656">揮発性読み取りには "取得セマンティクス" があります。つまり、命令シーケンス内で発生したメモリへの参照の前に発生することが保証されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="88a1a-657">Volatile フィールドの書き込みは、 ***volatile 書き込み***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="88a1a-658">Volatile 書き込みには "リリースセマンティクス" があります。つまり、命令シーケンスで書き込み命令より前のメモリ参照の後に発生することは保証されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="88a1a-659">これらの制限により、すべてのスレッドが、他のスレッドによって実行された volatile の書き込みを、実行された順序で観察することが保証されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="88a1a-660">すべての実行スレッドから見たように、volatile 書き込みの単一の合計順序を指定するために、準拠している実装は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="88a1a-661">Volatile フィールドの型は、次のいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="88a1a-662">*Reference_type*。</span><span class="sxs-lookup"><span data-stu-id="88a1a-662">A *reference_type*.</span></span>
*  <span data-ttu-id="88a1a-663">`byte`型、、`float`、、、、、、、、または。`System.UIntPtr` `sbyte` `short` `ushort` `int` `uint` `char` `bool` `System.IntPtr`</span><span class="sxs-lookup"><span data-stu-id="88a1a-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="88a1a-664">@No__t-1、`sbyte`、`short`、`ushort`、`int`、または `uint` の enum 基本型を持つ*enum_type* 。</span><span class="sxs-lookup"><span data-stu-id="88a1a-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="88a1a-665">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-665">The example</span></span>
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
<span data-ttu-id="88a1a-666">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-666">produces the output:</span></span>
```console
result = 143
```

<span data-ttu-id="88a1a-667">この例`Main`では、メソッドはメソッド`Thread2`を実行する新しいスレッドを開始します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="88a1a-668">このメソッドは、という`result`非 volatile フィールドに値を格納し、volatile フィールド`finished`にを格納`true`します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="88a1a-669">メインスレッドは、フィールド`finished`がに`true`設定されるのを待機してから`result`、フィールドを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="88a1a-670">が`finished`宣言`volatile`されているため、メインスレッドはフィールド`143` `result`から値を読み取る必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="88a1a-671">`finished`フィールドが宣言`volatile` `0`されていない場合は、ストアがに`finished`格納されてからメインスレッドから参照されるようにすることができます。したがって、メインスレッドは、 `result`フィールド`result`。</span><span class="sxs-lookup"><span data-stu-id="88a1a-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="88a1a-672">`finished` を`volatile`フィールドとして宣言すると、そのような不整合を防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="88a1a-673">フィールドの初期化</span><span class="sxs-lookup"><span data-stu-id="88a1a-673">Field initialization</span></span>

<span data-ttu-id="88a1a-674">フィールドの初期値は、静的フィールドであるか、インスタンスフィールドであるかにかかわらず、フィールドの型の既定値 ([既定](variables.md#default-values)値) になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="88a1a-675">この既定の初期化が発生する前にフィールドの値を確認することはできず、フィールドは "初期化されていません" にはなりません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="88a1a-676">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-676">The example</span></span>
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
<span data-ttu-id="88a1a-677">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-677">produces the output</span></span>
```console
b = False, i = 0
```
<span data-ttu-id="88a1a-678">と`b`は`i`両方とも既定値に自動的に初期化されるためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="88a1a-679">変数初期化子</span><span class="sxs-lookup"><span data-stu-id="88a1a-679">Variable initializers</span></span>

<span data-ttu-id="88a1a-680">フィールド宣言には*variable_initializer*s を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="88a1a-681">静的フィールドの場合、変数初期化子は、クラスの初期化中に実行される代入ステートメントに対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="88a1a-682">インスタンスフィールドの場合、変数初期化子は、クラスのインスタンスが作成されるときに実行される代入ステートメントに対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="88a1a-683">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-683">The example</span></span>
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
<span data-ttu-id="88a1a-684">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-684">produces the output</span></span>
```console
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="88a1a-685">静的フィールド初期化子`x`が実行されるときにが割り当てら`i`れ`s` 、インスタンスフィールド初期化子が実行されるときにが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="88a1a-686">「[フィールドの初期化](classes.md#field-initialization)」で説明されている既定値の初期化は、変数初期化子を持つフィールドを含むすべてのフィールドに対して行われます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="88a1a-687">したがって、クラスが初期化されると、そのクラスのすべての静的フィールドが最初に既定値に初期化され、次に静的フィールド初期化子がテキスト順に実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="88a1a-688">同様に、クラスのインスタンスが作成されると、そのインスタンス内のすべてのインスタンスフィールドが最初に既定値に初期化され、次に、インスタンスフィールド初期化子がテキスト順に実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="88a1a-689">変数初期化子を持つ静的フィールドを既定値の状態で観察することができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="88a1a-690">ただし、スタイルの問題としては、この方法は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="88a1a-691">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-691">The example</span></span>
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
<span data-ttu-id="88a1a-692">この動作を行います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-692">exhibits this behavior.</span></span> <span data-ttu-id="88a1a-693">A と b の循環定義にかかわらず、プログラムは有効です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="88a1a-694">結果が出力されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-694">It results in the output</span></span>
```console
a = 1, b = 2
```
<span data-ttu-id="88a1a-695">静的フィールド`a`および`b`は、初期化子が`0`実行される前に`int`(の既定値) に初期化されるためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="88a1a-696">の`a`初期化子が実行`a`されると、 `b`の値が0になり、が`1`に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="88a1a-697">の`b`初期化子を実行すると、の`a`値は`1`既にに`b`なります。 `2`したがって、はに初期化されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="88a1a-698">静的フィールドの初期化</span><span class="sxs-lookup"><span data-stu-id="88a1a-698">Static field initialization</span></span>

<span data-ttu-id="88a1a-699">クラスの静的フィールド変数初期化子は、クラス宣言に出現するテキスト順に実行される割り当てのシーケンスに対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="88a1a-700">静的コンストラクター ([静的](classes.md#static-constructors)コンストラクター) がクラスに存在する場合、静的コンストラクターを実行する直前に静的フィールド初期化子が実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="88a1a-701">それ以外の場合、静的フィールド初期化子は、そのクラスの静的フィールドを最初に使用する前に、実装に依存する時刻に実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="88a1a-702">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-702">The example</span></span>
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="88a1a-703">では、次の出力が生成される場合があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-703">might produce either the output:</span></span>
```console
Init A
Init B
1 1
```
<span data-ttu-id="88a1a-704">または、次のように出力します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-704">or the output:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="88a1a-705">の初期化子と`X` `Y`の初期化子の実行は、どちらの順序でも発生する可能性があるため、これらのフィールドへの参照の前にのみ発生するように制約されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="88a1a-706">ただし、この例では次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-706">However, in the example:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="88a1a-707">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-707">the output must be:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="88a1a-708">静的コンストラクターを実行するときの規則は ([静的コンストラクター](classes.md#static-constructors)で定義されて`B`いるように)、静的`B`コンストラクター (つまり、静的フィールド初期化子`A`) は、の前に実行する必要があります。コンストラクターとフィールド初期化子。</span><span class="sxs-lookup"><span data-stu-id="88a1a-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="88a1a-709">インスタンスフィールドの初期化</span><span class="sxs-lookup"><span data-stu-id="88a1a-709">Instance field initialization</span></span>

<span data-ttu-id="88a1a-710">クラスのインスタンスフィールド変数初期化子は、そのクラスのインスタンスコンストラクター ([コンストラクター初期化子](classes.md#constructor-initializers)) のいずれかにエントリしたときに直ちに実行される割り当てのシーケンスに対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="88a1a-711">変数初期化子は、クラス宣言に含まれるテキスト順に実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="88a1a-712">クラスインスタンスの作成と初期化のプロセスについては、「[インスタンスコンストラクター](classes.md#instance-constructors)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="88a1a-713">インスタンスフィールドの変数初期化子では、作成されるインスタンスを参照できません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="88a1a-714">このため、変数初期化子で `this` を参照すると、コンパイル時にエラーが発生します。これは、変数初期化子が*simple_name*を介して任意のインスタンスメンバーを参照するためのコンパイル時エラーであるためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="88a1a-715">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="88a1a-716">の`y`変数初期化子は、作成されるインスタンスのメンバーを参照するため、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="88a1a-717">メソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-717">Methods</span></span>

<span data-ttu-id="88a1a-718">"***メソッド***" は、オブジェクトまたはクラスによって実行可能な計算またはアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="88a1a-719">メソッドは*method_declaration*s を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-719">Methods are declared using *method_declaration*s:</span></span>

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="88a1a-720">*Method_declaration*には、*属性*のセット ([属性](attributes.md)) と、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers))、@no__t 4 ([新しい修飾子](classes.md#the-new-modifier))、`static` ([Static および instance) の有効な組み合わせを含めることができます。メソッド](classes.md#static-and-instance-methods))、@no__t 8 ([仮想メソッド](classes.md#virtual-methods))、0 ([オーバーライドメソッド](classes.md#override-methods))、2 ([シールメソッド](classes.md#sealed-methods))、4 ([抽象メソッド](classes.md#abstract-methods))、および 6 ([外部メソッド](classes.md#external-methods)) 修飾子。</span><span class="sxs-lookup"><span data-stu-id="88a1a-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="88a1a-721">次のすべての条件を満たす場合、宣言には修飾子の有効な組み合わせがあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="88a1a-722">この宣言には、アクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせが含まれています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="88a1a-723">宣言に、同じ修飾子が複数回含まれていません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="88a1a-724">宣言には`static`、、 `virtual`、および`override`の修飾子が1つだけ含まれています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="88a1a-725">宣言には、 `new`と`override`の修飾子のうち、最大1つしか含まれていません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="88a1a-726">宣言`abstract`に修飾子が含まれている場合、宣言には`static`、 `virtual`、、 `sealed`または`extern`の各修飾子は含まれません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="88a1a-727">宣言に`private`修飾子が含まれている場合、宣言には`virtual`、 `override`、、または`abstract`の各修飾子は含まれません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="88a1a-728">宣言に`sealed`修飾子が含まれている場合は、宣言に`override`修飾子も含まれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="88a1a-729">宣言`partial`に修飾子が含まれている場合は`private` `internal` `public` `override` 、、、 `protected`、 `new` `sealed`、、、、の各修飾子は含まれません。 `virtual`、 `abstract`、また`extern`は。</span><span class="sxs-lookup"><span data-stu-id="88a1a-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="88a1a-730">`async`修飾子を持つメソッドは非同期関数で、「 [async functions](classes.md#async-functions)」で説明されている規則に従います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="88a1a-731">メソッド*宣言の種類によっ*て、計算され、メソッドによって返される値の型が指定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="88a1a-732">メソッドが値を返さない場合、`void`*になります*。</span><span class="sxs-lookup"><span data-stu-id="88a1a-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="88a1a-733">宣言に`partial`修飾子が含まれている場合、戻り値の`void`型はである必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="88a1a-734">*Member_name*は、メソッドの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="88a1a-735">メソッドが明示的なインターフェイスメンバーの実装 ([明示的なインターフェイスメンバーの実装](interfaces.md#explicit-interface-member-implementations)) でない限り、 *member_name*は単なる*識別子*です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="88a1a-736">明示的なインターフェイスメンバーの実装では、 *member_name*は、 *interface_type*の後に "`.`" と*識別子*を続けたもので構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="88a1a-737">省略可能な*type_parameter_list*は、メソッド ([型パラメーター](classes.md#type-parameters)) の型パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="88a1a-738">*Type_parameter_list*が指定されている場合、メソッドは***ジェネリックメソッド***です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="88a1a-739">メソッドに @no__t 0 修飾子がある場合、 *type_parameter_list*を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="88a1a-740">省略可能な*formal_parameter_list*は、メソッドのパラメーター ([メソッドパラメーター](classes.md#method-parameters)) を指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="88a1a-741">省略可能な*type_parameter_constraints_clause*s は、個々の型パラメーター ([型パラメーター制約](classes.md#type-parameter-constraints)) に対して制約を指定し、 *type_parameter_list*が指定されている場合にのみ指定できます。また、メソッドには、`override` 修飾子。</span><span class="sxs-lookup"><span data-stu-id="88a1a-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="88a1a-742">メソッドの*formal_parameter_list*で参照される各型*は、少なく*ともメソッド自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="88a1a-743">*Method_body*は、セミコロン、***ステートメント本体***、または式の***本体***です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="88a1a-744">ステートメントの本体は、メソッドが呼び出されたときに実行するステートメントを指定する*ブロック*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="88a1a-745">式の`=>`本体は、*式*とセミコロンで構成され、その後にメソッドが呼び出されたときに実行する1つの式を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="88a1a-746">@No__t-0 および `extern` メソッドの場合、 *method_body*はセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="88a1a-747">@No__t 0 のメソッドの場合、 *method_body*は、セミコロン、ブロック本体、または式の本体で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="88a1a-748">その他のすべてのメソッドでは、 *method_body*はブロック本体または式の本体です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="88a1a-749">*Method_body*がセミコロンで構成されている場合、宣言には `async` 修飾子を含めることができません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="88a1a-750">メソッドの名前、型パラメーターリスト、およびメソッドの仮パラメーターリストは、メソッドのシグネチャ (シグネチャ[とオーバーロード](basic-concepts.md#signatures-and-overloading)) を定義します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="88a1a-751">具体的には、メソッドのシグネチャは、その名前、型パラメーターの数、およびその仮パラメーターの数、修飾子、および型で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="88a1a-752">このため、仮パラメーターの型で発生するメソッドの型パラメーターは、その名前ではなく、メソッドの型引数リスト内の序数位置によって識別されます。戻り値の型は、メソッドのシグネチャの一部ではなく、型パラメーターまたは仮パラメーターの名前でもありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="88a1a-753">メソッドの名前は、同じクラスで宣言されている他のすべての非メソッドの名前と異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="88a1a-754">また、メソッドのシグネチャは、同じクラスで宣言されている他のすべてのメソッドのシグネチャとは異なる必要があります。また、同じクラスで宣言された`ref` 2 `out`つのメソッドは、とだけが異なるシグネチャを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="88a1a-755">このメソッドの*type_parameter*は、 *method_declaration*全体を通じてスコープ内にあります。また、このメソッドを使用*して、* type_parameter_constraints_clause、 *method_body*、s でそのスコープ全体の型を形成することができますが、*属性*内。</span><span class="sxs-lookup"><span data-stu-id="88a1a-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="88a1a-756">すべての仮パラメーターと型パラメーターには、異なる名前を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="88a1a-757">メソッドパラメーター</span><span class="sxs-lookup"><span data-stu-id="88a1a-757">Method parameters</span></span>

<span data-ttu-id="88a1a-758">メソッドのパラメーター (存在する場合) は、メソッドの*formal_parameter_list*によって宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

<span data-ttu-id="88a1a-759">仮パラメーターリストは、最後のパラメーターだけが*parameter_array*である、1つ以上のコンマ区切りのパラメーターで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="88a1a-760">*Fixed_parameter*は、省略可能な*属性*のセット ([属性](attributes.md))、オプションの `ref`、@no__t 4 または `this` 修飾子、*型*、*識別子*、および省略可能な*default_argument*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="88a1a-761">各*fixed_parameter*は、指定された名前を使用して、指定された型のパラメーターを宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="88a1a-762">修飾子`this`は、メソッドを拡張メソッドとして指定し、静的メソッドの最初のパラメーターでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="88a1a-763">拡張メソッドの詳細については、「[拡張メソッド](classes.md#extension-methods)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="88a1a-764">*Default_argument*を持つ*fixed_parameter*は***省略可能なパラメーター***として知られていますが、 *default_argument*のない*fixed_parameter*は***必須パラメーター***です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="88a1a-765">必須パラメーターは、 *formal_parameter_list*の省略可能なパラメーターの後に指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="88a1a-766">@No__t-0 または `out` のパラメーターに*default_argument*を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="88a1a-767">*Default_argument*の*式*は、次のいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="88a1a-768">*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="88a1a-768">a *constant_expression*</span></span>
*  <span data-ttu-id="88a1a-769">`new S()` フォーム`S`の式 (は値型)</span><span class="sxs-lookup"><span data-stu-id="88a1a-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="88a1a-770">`default(S)` フォーム`S`の式 (は値型)</span><span class="sxs-lookup"><span data-stu-id="88a1a-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="88a1a-771">*式*は、id または null 許容型からパラメーターの型への変換によって、暗黙的に変換可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="88a1a-772">部分メソッド宣言の実装 ([部分メソッド](classes.md#partial-methods))、明示的なインターフェイスメンバーの実装 ([明示的なインターフェイスメンバーの実装](interfaces.md#explicit-interface-member-implementations))、または単一パラメーターインデクサー宣言 ([インデクサー](classes.md#indexers)) は、引数を省略できるようにするためにこれらのメンバーを呼び出すことはできないため、コンパイラは警告を出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="88a1a-773">*Parameter_array*は、省略可能な*属性*のセット ([属性](attributes.md))、`params` 修飾子、 *array_type*、および*識別子*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="88a1a-774">パラメーター配列は、指定された名前を持つ指定された配列型の1つのパラメーターを宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="88a1a-775">パラメーター配列の*array_type*は、1次元配列型 ([配列型](arrays.md#array-types)) である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="88a1a-776">メソッド呼び出しでは、パラメーター配列は、指定された配列型の1つの引数を指定するか、配列要素型の0個以上の引数を指定できるようにします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="88a1a-777">パラメーター配列の詳細については、「[パラメーター配列](classes.md#parameter-arrays)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="88a1a-778">*Parameter_array*は省略可能なパラメーターの後に発生する可能性がありますが、既定値を持つことはできません。 *parameter_array*の引数を省略すると、代わりに空の配列が作成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="88a1a-779">次の例は、さまざまな種類のパラメーターを示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-779">The following example illustrates different kinds of parameters:</span></span>
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

<span data-ttu-id="88a1a-780">@No__t- *1 の場合、@no__t*は必須の ref パラメーター、`d` は必須の値パラメーター、`b`、`s`、`o`、`t` は省略可能な値パラメーターで、`a` はパラメーター配列です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="88a1a-781">メソッド宣言は、パラメーター、型パラメーター、およびローカル変数に対して個別の宣言領域を作成します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="88a1a-782">名前は、型パラメーターリストとメソッドの仮パラメーターリスト、およびメソッドの*ブロック*内のローカル変数宣言によって、この宣言領域に導入されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="88a1a-783">メソッド宣言空間の2つのメンバーが同じ名前を持つ場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="88a1a-784">メソッド宣言領域と入れ子になった宣言空間のローカル変数宣言領域が同じ名前の要素を含む場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="88a1a-785">メソッドの呼び出し ([メソッド呼び出し](expressions.md#method-invocations)) によって、メソッドの仮パラメーターとローカル変数の、その呼び出しに固有のコピーが作成され、呼び出しの引数リストによって、新しく作成された仮引数に値または変数参照が割り当てられます。パラメータ.</span><span class="sxs-lookup"><span data-stu-id="88a1a-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="88a1a-786">メソッドの*ブロック*内では、 *Simple_name*式 ([簡易名](expressions.md#simple-names)) の識別子によって仮パラメーターを参照できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="88a1a-787">仮引数には4種類あります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="88a1a-788">値パラメーター。修飾子なしで宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="88a1a-789">参照パラメーター。 `ref`修飾子を使用して宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="88a1a-790">出力パラメーター。 `out`修飾子を使用して宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="88a1a-791">パラメーター配列。 `params`修飾子を使用して宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="88a1a-792">「[シグネチャとオーバーロード](basic-concepts.md#signatures-and-overloading) `ref` 」で説明され`out`ているように、修飾子と修飾子はメソッドの`params`シグネチャの一部ですが、修飾子は含まれません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="88a1a-793">値パラメーター</span><span class="sxs-lookup"><span data-stu-id="88a1a-793">Value parameters</span></span>

<span data-ttu-id="88a1a-794">修飾子を指定せずに宣言されたパラメーターは、値パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="88a1a-795">値パラメーターは、メソッドの呼び出しで指定された対応する引数から初期値を取得するローカル変数に対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="88a1a-796">仮パラメーターが値パラメーターの場合、メソッド呼び出しの対応する引数は、暗黙的に変換可能な (暗黙の型[変換](conversions.md#implicit-conversions)) 式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="88a1a-797">メソッドは、値パラメーターに新しい値を割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="88a1a-798">このような割り当ては、値パラメーターによって表されるローカルストレージの場所にのみ影響します。メソッドの呼び出しで指定された実際の引数には影響しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="88a1a-799">参照パラメーター</span><span class="sxs-lookup"><span data-stu-id="88a1a-799">Reference parameters</span></span>

<span data-ttu-id="88a1a-800">修飾子を`ref`使用して宣言されたパラメーターは参照パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="88a1a-801">値パラメーターとは異なり、参照パラメーターは新しいストレージの場所を作成しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="88a1a-802">代わりに、参照パラメーターは、メソッド呼び出しで引数として指定された変数と同じ格納場所を表します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="88a1a-803">仮パラメーターが参照パラメーターである場合、メソッド呼び出しの対応する引数は、キーワード `ref` の後に、 *variable_reference* (明確な[割り当てを決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) で構成される必要があります。仮パラメーター。</span><span class="sxs-lookup"><span data-stu-id="88a1a-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="88a1a-804">参照パラメーターとして渡すには、変数を確実に代入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="88a1a-805">メソッド内では、参照パラメーターは常に確実に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="88a1a-806">反復子 ([反復子](classes.md#iterators)) として宣言されたメソッドに参照パラメーターを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="88a1a-807">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-807">The example</span></span>
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
<span data-ttu-id="88a1a-808">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-808">produces the output</span></span>
```console
i = 2, j = 1
```

<span data-ttu-id="88a1a-809">で`Swap` `x` `i`のの呼び出しの場合、は`y` と`j`を表します。 `Main`</span><span class="sxs-lookup"><span data-stu-id="88a1a-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="88a1a-810">したがって、呼び出しはと`i` `j`の値を交換した場合の効果があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="88a1a-811">参照パラメーターを受け取るメソッドでは、複数の名前が同じストレージの場所を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="88a1a-812">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-812">In the example</span></span>
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
<span data-ttu-id="88a1a-813">`F`のの呼び出しで`G`は、と`s` `a`の両方のへの参照が渡されます`b`。</span><span class="sxs-lookup"><span data-stu-id="88a1a-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="88a1a-814">そのため、この呼び出しでは、 `s`、 `a`、および`b`はすべて同じストレージの場所を参照し、3つの代入はすべてインスタンス`s`フィールドを変更します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="88a1a-815">出力パラメーター</span><span class="sxs-lookup"><span data-stu-id="88a1a-815">Output parameters</span></span>

<span data-ttu-id="88a1a-816">`out`修飾子を使用して宣言されたパラメーターは、出力パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="88a1a-817">参照パラメーターと同様に、出力パラメーターは新しいストレージの場所を作成しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="88a1a-818">代わりに、出力パラメーターは、メソッド呼び出しで引数として指定された変数と同じ格納場所を表します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="88a1a-819">仮パラメーターが出力パラメーターの場合、メソッド呼び出しの対応する引数は、variable_reference の @no__t キーワードで構成されている必要があります。その後、次のように同じ型の ([明確な割り当てを決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) を指定します。仮パラメーター。</span><span class="sxs-lookup"><span data-stu-id="88a1a-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="88a1a-820">変数は出力パラメーターとして渡す前に明示的に割り当てられている必要はありませんが、変数が出力パラメーターとして渡された場合の呼び出しに従うと、変数は確実に代入されたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="88a1a-821">ローカル変数と同じように、メソッド内では、出力パラメーターはまず未割り当てと見なされ、その値を使用する前に確実に代入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="88a1a-822">メソッドが返される前に、メソッドのすべての出力パラメーターを確実に代入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="88a1a-823">部分メソッド ([部分メソッド](classes.md#partial-methods)) または反復子 ([反復](classes.md#iterators)子) として宣言されたメソッドは、出力パラメーターを持つことができません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="88a1a-824">出力パラメーターは、通常、複数の戻り値を生成するメソッドで使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="88a1a-825">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-825">For example:</span></span>
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

<span data-ttu-id="88a1a-826">この例では、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-826">The example produces the output:</span></span>
```console
c:\Windows\System\
hello.txt
```

<span data-ttu-id="88a1a-827">変数と`dir` `name`変数は、に`SplitPath`渡す前に割り当てを解除することができ、呼び出しの後に確実に割り当てられることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="88a1a-828">パラメーター配列</span><span class="sxs-lookup"><span data-stu-id="88a1a-828">Parameter arrays</span></span>

<span data-ttu-id="88a1a-829">修飾子を`params`使用して宣言されたパラメーターは、パラメーター配列です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="88a1a-830">仮パラメーターリストにパラメーター配列が含まれている場合は、リストの最後のパラメーターである必要があり、それは1次元配列型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="88a1a-831">たとえば、型と`string[]` `string[][]`型はパラメーター配列の型として使用できますが、型`string[,]`は使用できません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="88a1a-832">`params`修飾子`ref`と修飾子`out`を組み合わせることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="88a1a-833">パラメーター配列では、メソッド呼び出しの2つの方法のいずれかで引数を指定できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="88a1a-834">パラメーター配列に指定する引数には、暗黙的に変換可能な (暗黙の[変換](conversions.md#implicit-conversions)) 1 つの式をパラメーター配列型に指定できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="88a1a-835">この場合、パラメーター配列は、値パラメーターとまったく同様に動作します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="88a1a-836">また、呼び出しでは、パラメーター配列に0個以上の引数を指定できます。各引数は、暗黙的に変換可能な式 ([暗黙的な変換](conversions.md#implicit-conversions)) で、パラメーター配列の要素型になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="88a1a-837">この場合、呼び出しは、引数の数に対応する長さを持つパラメーター配列型のインスタンスを作成し、指定された引数値を使用して配列インスタンスの要素を初期化し、新しく作成された配列インスタンスを実際のとして使用します。引数.</span><span class="sxs-lookup"><span data-stu-id="88a1a-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="88a1a-838">呼び出しで可変個の引数を許可する場合を除き、パラメーター配列は、同じ型の値パラメーター ([値](classes.md#value-parameters)パラメーター) と厳密に等価です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="88a1a-839">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-839">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
<span data-ttu-id="88a1a-840">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-840">produces the output</span></span>
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="88a1a-841">の最初の`F`呼び出しでは、単`a`に配列を値パラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="88a1a-842">の2番目`F`の呼び出しは、指定さ`int[]`れた要素の値を持つ4つの要素を自動的に作成し、その配列インスタンスを値パラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="88a1a-843">同様に、の3番`F`目の呼び出しでは`int[]` 、ゼロ要素を作成し、そのインスタンスを値パラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="88a1a-844">2番目と3番目の呼び出しは、書き込みとまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="88a1a-845">オーバーロードの解決を実行する場合、パラメーター配列を持つメソッドは、通常の形式または拡張された形式 ([適用可能な関数メンバー](expressions.md#applicable-function-member)) のいずれかに適用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="88a1a-846">メソッドの拡張形式は、メソッドの通常の形式が適用されない場合にのみ使用できます。また、展開されたフォームと同じシグネチャを持つ適用可能なメソッドが、同じ型で宣言されていない場合にのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="88a1a-847">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-847">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
<span data-ttu-id="88a1a-848">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-848">produces the output</span></span>
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="88a1a-849">この例では、パラメーター配列を使用するメソッドの拡張された2つの形式は、通常のメソッドとしてクラスに既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="88a1a-850">これらの拡張されたフォームは、オーバーロードの解決を実行するときには考慮されません。そのため、最初と3番目のメソッドの呼び出しでは、通常のメソッドを選択します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="88a1a-851">クラスがパラメーター配列を使用してメソッドを宣言する場合、一部の拡張されたフォームも通常のメソッドとして含めることは珍しくありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="88a1a-852">これにより、パラメーター配列を持つメソッドの拡張された形式が呼び出されたときに発生する配列インスタンスの割り当てを回避できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="88a1a-853">パラメーター配列の型がの場合、 `object[]`通常の形式のメソッドと1つ`object`のパラメーターの使用可能な形式との間であいまいさが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="88a1a-854">あいまいさの理由は、自体`object[]`が型`object`に暗黙的に変換できることです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="88a1a-855">ただし、あいまいさは、必要に応じてキャストを挿入することによって解決できるため、問題ありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="88a1a-856">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-856">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
<span data-ttu-id="88a1a-857">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-857">produces the output</span></span>
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="88a1a-858">の最初と最後の`F`呼び出しでは、引数の型からパラメーターの型への暗黙的な変換が存在するため (両方とも型`object[]`)、通常の`F`形式が適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="88a1a-859">したがって、オーバーロードの解決では通常`F`の形式のが選択され、引数は通常の値パラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="88a1a-860">2番目と3番目の呼び出しでは`F` 、引数の型からパラメーターの型への暗黙的な変換が存在しないため`object` 、通常の形式のが`object[]`適用されません (型に暗黙的に変換することはできません)。</span><span class="sxs-lookup"><span data-stu-id="88a1a-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="88a1a-861">ただし、の拡張形式は`F`適用可能なので、オーバーロードの解決によって選択されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="88a1a-862">その結果、1つの要素`object[]`が呼び出しによって作成され、配列の1つの要素が、指定された引数の値 (自体が`object[]`への参照) で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="88a1a-863">静的メソッドとインスタンス メソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-863">Static and instance methods</span></span>

<span data-ttu-id="88a1a-864">メソッドの宣言に`static`修飾子が含まれている場合、そのメソッドは静的メソッドと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="88a1a-865">修飾子が`static`存在しない場合、メソッドはインスタンスメソッドと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="88a1a-866">静的メソッドは、特定のインスタンスでは動作せず、静的メソッド`this`で参照するコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="88a1a-867">インスタンスメソッドは、クラスの特定のインスタンスに対して動作し、そのインスタンスには`this` ([このアクセス](expressions.md#this-access)) としてアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="88a1a-868">@No__t-2 の形式の*member_access* ([メンバーアクセス](expressions.md#member-access)) でメソッドが参照されている場合、`M` が静的メソッドである場合、`E` は `M` を含む型を表している必要があり、`M` がインスタンスメソッドの場合は、@no__t が型のインスタンスを示す必要があります。@no__t 8 を含む。</span><span class="sxs-lookup"><span data-stu-id="88a1a-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="88a1a-869">静的メンバーとインスタンスメンバーの違いについては、「[静的メンバーとインスタンス](classes.md#static-and-instance-members)メンバー」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="88a1a-870">仮想メソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-870">Virtual methods</span></span>

<span data-ttu-id="88a1a-871">インスタンスメソッドの宣言に`virtual`修飾子が含まれている場合、そのメソッドは仮想メソッドと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="88a1a-872">修飾子が`virtual`存在しない場合、メソッドは非仮想メソッドと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="88a1a-873">非仮想メソッドの実装は不変です。実装は、メソッドが宣言されているクラスのインスタンスまたは派生クラスのインスタンスで呼び出されているかどうかと同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="88a1a-874">これに対し、仮想メソッドの実装は、派生クラスによって置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="88a1a-875">継承された仮想メソッドの実装を置き換えるプロセスは、そのメソッドの***オーバーライド***([メソッドのオーバーライド](classes.md#override-methods)) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="88a1a-876">仮想メソッドの呼び出しでは、呼び出し元のインスタンスの***実行時の型***によって、呼び出す実際のメソッドの実装が決まります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="88a1a-877">非仮想メソッドの呼び出しでは、インスタンスの***コンパイル時の型***が決定要因になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="88a1a-878">正確に言うと、という名前`N`のメソッドが、コンパイル時`A`の型`R` `C`と`C`実行時の型`R`を持つインスタンスの引数リストを使用して呼び出された場合 (は、またはクラスが派生したクラスです。から`C`)、呼び出しは次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="88a1a-879">最初に、オーバーロードの解決は`C`、 `N`、および`A`に適用され、で`M`宣言され、によって`C`継承されるメソッドのセットから特定のメソッドを選択します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="88a1a-880">これについては、「[メソッドの呼び出し](expressions.md#method-invocations)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="88a1a-881">次に、 `M`が非仮想`M`メソッドである場合、が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="88a1a-882">それ以外`M`の場合、は仮想メソッドであり、に`R`対する`M`の最も派生した実装が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="88a1a-883">クラスによって宣言または継承されているすべての仮想メソッドについて、そのクラスに対してメソッドの***最も派生***した実装が存在します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="88a1a-884">`M` クラス`R`に対して最も派生する仮想メソッドの実装は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="88a1a-885">に`R` `virtual`の`M`導入宣言が含まれている場合、これはの最も派生された実装です。 `M`</span><span class="sxs-lookup"><span data-stu-id="88a1a-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="88a1a-886">それ以外の`R`場合、 `override`に`M`が含まれている場合は、これ`M`がの最も派生された実装になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="88a1a-887">それ以外の場合、に対し`M`てを使用`R`したの最も派生する実装は、 `M`の`R`直接の基底クラスに対するの最も派生した実装と同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="88a1a-888">次の例では、仮想メソッドと非仮想メソッドの違いについて説明します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

<span data-ttu-id="88a1a-889">この例では`A` 、によって、非`F`仮想メソッドと仮想`G`メソッドが導入されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="88a1a-890">クラス`B`は、新しい非仮想メソッド`F`を導入し、継承`F`されたを非表示にし、 `G`継承されたメソッドもオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="88a1a-891">この例では、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-891">The example produces the output:</span></span>
```console
A.F
B.F
B.G
B.G
```

<span data-ttu-id="88a1a-892">ではなく`a.G()` `B.G` 、`A.G`ステートメントが呼び出されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="88a1a-893">これは、インスタンスのランタイム型 ( `B`) がインスタンスのコンパイル時の型 ( `A`) ではなく、呼び出す実際のメソッドの実装を決定するためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="88a1a-894">メソッドは継承されたメソッドを隠すことができるため、クラスに同じシグネチャを持つ複数の仮想メソッドを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="88a1a-895">これにより、すべての派生メソッドが非表示になるため、あいまいさの問題はありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="88a1a-896">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-896">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
<span data-ttu-id="88a1a-897">クラス`C` と`D`クラスには、同じシグネチャを持つ2つの仮想メソッドが含まれています。によっ`A`て導入されたものと`C`、によって導入されたもの。</span><span class="sxs-lookup"><span data-stu-id="88a1a-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="88a1a-898">によって導入`C`されたメソッドは`A`、から継承されたメソッドを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="88a1a-899">したがって、のオーバーライド宣言`D`はによって`C`導入されたメソッドをオーバーライド`D`するため、で導入`A`されたメソッドをでオーバーライドすることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="88a1a-900">この例では、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-900">The example produces the output:</span></span>
```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="88a1a-901">メソッドが非表示にならない派生型を使用してのインスタンスに`D`アクセスすることによって、非表示の仮想メソッドを呼び出すことができることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="88a1a-902">オーバーライドメソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-902">Override methods</span></span>

<span data-ttu-id="88a1a-903">インスタンスメソッドの宣言に`override`修飾子が含まれている場合、メソッドは***オーバーライドメソッド***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="88a1a-904">オーバーライドメソッドは、同じシグネチャを持つ継承された仮想メソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="88a1a-905">仮想メソッドの宣言には新しいメソッドが導入されていますが、オーバーライド メソッドの宣言では、そのメソッドの新しい実装を提供することで既存の継承された仮想メソッドを特殊化します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="88a1a-906">`override`宣言によってオーバーライドされるメソッドは、オーバーライドされた***基本メソッド***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="88a1a-907">クラス`M` `C` `C`で宣言されたオーバーライドメソッドの場合、オーバーライドされた基本メソッドは、の基底クラス型のそれぞれを調べることによって決定されます。これは、の直接基底クラス型から始まり、それぞれ連続して続行されます。 `C`直接基底クラスの型。指定した基底クラスの型では、少なくと`M`も1つのアクセス可能なメソッドが配置されます。このメソッドには、型引数の置換後のシグネチャがあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="88a1a-908">オーバーライドされた基本メソッドを検索するために、メソッドは、存在する場合`public`は、そのメソッド`protected`にアクセスできる`protected internal`と見なされ`C`ます (の`internal`場合)。また、と同じプログラム内で宣言されている場合もあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="88a1a-909">次のすべてがオーバーライド宣言に当てはまる場合を除き、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="88a1a-910">オーバーライドされた基本メソッドは、前述のように配置できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="88a1a-911">このようなオーバーライドされた基本メソッドが1つだけあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="88a1a-912">この制限は、基底クラスの型が構築された型である場合にのみ有効です。型引数を代入すると、2つのメソッドのシグネチャが同じになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="88a1a-913">オーバーライドされた基本メソッドは、virtual、abstract、または override メソッドです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="88a1a-914">言い換えると、オーバーライドされた基本メソッドを静的または非仮想にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="88a1a-915">オーバーライドされた基本メソッドは、シールメソッドではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="88a1a-916">オーバーライドメソッドとオーバーライドされた基本メソッドの戻り値の型は同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="88a1a-917">オーバーライド宣言とオーバーライドされた基本メソッドに、同じアクセシビリティが宣言されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="88a1a-918">つまり、オーバーライド宣言では、仮想メソッドのアクセシビリティを変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="88a1a-919">ただし、オーバーライドされた基本メソッドが内部で保護されていて、オーバーライドメソッドを含むアセンブリとは別のアセンブリで宣言されている場合は、オーバーライドメソッドで宣言されたアクセシビリティを保護する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="88a1a-920">オーバーライド宣言で、型パラメーターの制約句が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="88a1a-921">代わりに、制約はオーバーライドされた基本メソッドから継承されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="88a1a-922">オーバーライドされたメソッドの型パラメーターである制約は、継承された制約の型引数によって置き換えられる可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="88a1a-923">これは、値型やシール型など、明示的に指定された場合には無効な制約につながる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="88a1a-924">次の例は、ジェネリッククラスでのオーバーライド規則のしくみを示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

<span data-ttu-id="88a1a-925">オーバーライド宣言は、 *base_access* ([ベースアクセス](expressions.md#base-access)) を使用してオーバーライドされた基本メソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="88a1a-926">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-926">In the example</span></span>
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
<span data-ttu-id="88a1a-927">の`base.PrintFields()` `PrintFields` 呼び出しは`A`、で宣言されたメソッドを呼び出します。 `B`</span><span class="sxs-lookup"><span data-stu-id="88a1a-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="88a1a-928">*Base_access*は、仮想呼び出し機構を無効にし、単に基本メソッドを非仮想メソッドとして扱います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="88a1a-929">の呼び出し`B`が書き込ま`A` `PrintFields` `PrintFields` `B`れている場合は、で宣言されたメソッドではなく、で宣言されたメソッドを再帰的に呼び出します。これは、が仮想であり、の実行時の型であるためです。 `((A)this).PrintFields()` `((A)this)` が`B`です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="88a1a-930">`override`修飾子を含めることによってのみ、メソッドが別のメソッドをオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="88a1a-931">それ以外の場合、継承されたメソッドと同じシグネチャを持つメソッドは、継承されたメソッドを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="88a1a-932">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-932">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
<span data-ttu-id="88a1a-933">`A` `F`の`F` `override`メソッドに修飾子が含まれていないため、のメソッドはオーバーライドされません。 `B`</span><span class="sxs-lookup"><span data-stu-id="88a1a-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="88a1a-934">代わりに、の`F` `B`メソッドはの`A`メソッドを非表示にします。また、宣言に`new`修飾子が含まれていないため、警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="88a1a-935">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-935">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
<span data-ttu-id="88a1a-936">の`F`メソッドは`B` 、 `F` から`A`継承された仮想メソッドを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="88a1a-937">の新しい`F` `B`はプライベートアクセス`C`があるため、そのスコープにはのクラス本体のみが含まれ、はには拡張され`B`ません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="88a1a-938">したがって、のの`F` `C`宣言では、から`A`継承`F`されたをオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="88a1a-939">シールメソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-939">Sealed methods</span></span>

<span data-ttu-id="88a1a-940">インスタンスメソッドの宣言に`sealed`修飾子が含まれている場合、そのメソッドは***sealed メソッド***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="88a1a-941">インスタンスメソッドの`sealed`宣言に修飾子が含まれている場合は、 `override`修飾子も含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="88a1a-942">`sealed`修飾子を使用すると、派生クラスでメソッドをさらにオーバーライドできなくなります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="88a1a-943">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-943">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
<span data-ttu-id="88a1a-944">クラス`B`には、 `sealed`修飾子`F` を`G`持つメソッドと、ではないメソッドの2つのオーバーライドメソッドが用意されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="88a1a-945">`B`では、sealed `modifier`を使用することにより、をさらにオーバーライド`F`することはできませ`C`ん。</span><span class="sxs-lookup"><span data-stu-id="88a1a-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="88a1a-946">抽象メソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-946">Abstract methods</span></span>

<span data-ttu-id="88a1a-947">インスタンスメソッドの宣言に`abstract`修飾子が含まれている場合、そのメソッドは***抽象メソッド***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="88a1a-948">抽象メソッドは暗黙的に仮想メソッドでもありますが、修飾子`virtual`を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="88a1a-949">抽象メソッドの宣言では、新しい仮想メソッドが導入されていますが、そのメソッドの実装は提供していません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="88a1a-950">代わりに、抽象でない派生クラスは、そのメソッドをオーバーライドすることによって独自の実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="88a1a-951">抽象メソッドは実際の実装を提供しないため、抽象メソッドの*method_body*は、単純にセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="88a1a-952">抽象メソッドの宣言は、抽象クラス ([抽象クラス](classes.md#abstract-classes)) でのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="88a1a-953">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-953">In the example</span></span>
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
<span data-ttu-id="88a1a-954">クラス`Shape`は、それ自体を描画できる幾何学図形オブジェクトの抽象概念を定義します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="88a1a-955">有意`Paint`な既定の実装がないため、メソッドは abstract です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="88a1a-956">クラス`Ellipse`と`Box`クラスは、 `Shape`具象実装です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="88a1a-957">これらのクラスは非抽象であるため、 `Paint`メソッドをオーバーライドし、実際の実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="88a1a-958">*Base_access* ([ベースアクセス](expressions.md#base-access)) が抽象メソッドを参照する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="88a1a-959">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-959">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
<span data-ttu-id="88a1a-960">`base.F()`呼び出しでは抽象メソッドを参照しているため、コンパイル時エラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="88a1a-961">抽象メソッドの宣言では、仮想メソッドをオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="88a1a-962">これにより、抽象クラスは派生クラスのメソッドを強制的に再実装できるようになり、メソッドの元の実装を使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="88a1a-963">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-963">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
<span data-ttu-id="88a1a-964">クラス`A`は仮想メソッドを宣言し`B` 、クラスは抽象メソッドを使用してこの`C`メソッドをオーバーライドし、クラスは抽象メソッドをオーバーライドして独自の実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="88a1a-965">外部メソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-965">External methods</span></span>

<span data-ttu-id="88a1a-966">メソッドの宣言に`extern`修飾子が含まれている場合、そのメソッドは***外部メソッド***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="88a1a-967">外部メソッドは、通常以外C#の言語を使用して、外部で実装されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="88a1a-968">外部メソッドの宣言は実際の実装を提供しないため、外部メソッドの*method_body*は、単純にセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="88a1a-969">外部メソッドはジェネリックではない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-969">An external method may not be generic.</span></span>

<span data-ttu-id="88a1a-970">修飾子は、通常、 `DllImport`属性 ([COM コンポーネントおよび Win32 コンポーネントとの相互運用](attributes.md#interoperation-with-com-and-win32-components)) と共に使用して、外部メソッドを dll (ダイナミックリンクライブラリ) で実装できるようにします。 `extern`</span><span class="sxs-lookup"><span data-stu-id="88a1a-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="88a1a-971">実行環境では、外部メソッドの実装を提供できる他のメカニズムがサポートされている場合があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="88a1a-972">外部メソッドに`DllImport`属性が含まれている場合は、メソッドの宣言`static`に修飾子も含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="88a1a-973">この例では、 `extern`修飾子`DllImport`と属性の使用方法を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a><span data-ttu-id="88a1a-974">部分メソッド (要約)</span><span class="sxs-lookup"><span data-stu-id="88a1a-974">Partial methods (recap)</span></span>

<span data-ttu-id="88a1a-975">メソッドの宣言に`partial`修飾子が含まれている場合、そのメソッドは***部分メソッド***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="88a1a-976">部分メソッドは、部分型 ([部分型](classes.md#partial-types)) のメンバーとしてのみ宣言でき、いくつかの制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="88a1a-977">部分メソッドの詳細については、[部分メソッド](classes.md#partial-methods)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="88a1a-978">拡張メソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-978">Extension methods</span></span>

<span data-ttu-id="88a1a-979">メソッドの最初のパラメーターに`this`修飾子が含まれている場合、そのメソッドは***拡張メソッド***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="88a1a-980">拡張メソッドは、非ジェネリックで入れ子にされていない静的クラスでのみ宣言できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="88a1a-981">拡張メソッドの最初のパラメーターは、以外`this`の修飾子を持つことはできません。また、パラメーターの型をポインター型にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="88a1a-982">2つの拡張メソッドを宣言する静的クラスの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-982">The following is an example of a static class that declares two extension methods:</span></span>
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

<span data-ttu-id="88a1a-983">拡張メソッドは、通常の静的メソッドです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-983">An extension method is a regular static method.</span></span> <span data-ttu-id="88a1a-984">また、外側の静的クラスがスコープ内にある場合、最初の引数としてレシーバー式を使用して、インスタンスメソッドの呼び出し構文 ([拡張メソッドの呼び出し](expressions.md#extension-method-invocations)) を使用して拡張メソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="88a1a-985">次のプログラムでは、上記で宣言した拡張メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-985">The following program uses the extension methods declared above:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

<span data-ttu-id="88a1a-986">メソッドは、 `string[]`で使用できます`string`。メソッドは、拡張メソッドとして宣言されているため、で使用できます。`ToInt32` `Slice`</span><span class="sxs-lookup"><span data-stu-id="88a1a-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="88a1a-987">プログラムの意味は、通常の静的メソッド呼び出しを使用した次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a><span data-ttu-id="88a1a-988">メソッド本体</span><span class="sxs-lookup"><span data-stu-id="88a1a-988">Method body</span></span>

<span data-ttu-id="88a1a-989">メソッド宣言の*method_body*は、ブロック本体、式本体、またはセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="88a1a-990">戻り値の型が`void`の場合`void` 、またはメソッドが非同期で戻り値の型が`System.Threading.Tasks.Task`の場合、メソッドの結果の***型***はです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="88a1a-991">それ以外の場合、非同期でないメソッドの結果の型は戻り値の型になり、戻り値の型`System.Threading.Tasks.Task<T>`を持つ非同期メソッドの結果の型は`T`になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="88a1a-992">メソッドの`void`結果型とブロック本体がある場合、 `return`ブロック内のステートメント ([return ステートメント](statements.md#the-return-statement)) で式を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="88a1a-993">Void メソッドのブロックの実行が正常に完了した場合 (つまり、制御フローがメソッド本体の末尾から離れた場合)、そのメソッドは単に現在の呼び出し元に戻ります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="88a1a-994">メソッドに @no__t 0 の結果と式の本体が含まれている場合、式 `E` は*statement_expression*である必要があり、本文は `{ E; }` の形式のブロック本体とまったく同じになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="88a1a-995">メソッドに void 以外の結果型とブロック本体がある場合、ブロック内`return`の各ステートメントでは、結果型に暗黙的に変換できる式を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="88a1a-996">値を返すメソッドのブロック本体のエンドポイントに到達できません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="88a1a-997">つまり、ブロック本体を持つ値を返すメソッドでは、コントロールはメソッド本体の末尾から制御できません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="88a1a-998">メソッドに void 以外の結果型と式本体がある場合、式は結果型に暗黙的に変換できる必要があり、本文はフォーム`{ return E; }`のブロック本体とまったく同じになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="88a1a-999">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-999">In the example</span></span>
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
<span data-ttu-id="88a1a-1000">コントロールはメソッド本体`F`の末尾から制御を渡すことができるため、値を返すメソッドはコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="88a1a-1001">すべて`G`の`H`実行パスが戻り値を指定する return ステートメントで終了するので、メソッドとメソッドは正しいです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="88a1a-1002">`I`メソッドは、その本体が1つの return ステートメントだけを含むステートメントブロックと同じであるため、正しいです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="88a1a-1003">メソッドのオーバーロード</span><span class="sxs-lookup"><span data-stu-id="88a1a-1003">Method overloading</span></span>

<span data-ttu-id="88a1a-1004">メソッドのオーバーロードの解決規則については、 [「型の推定](expressions.md#type-inference)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="88a1a-1005">Properties</span><span class="sxs-lookup"><span data-stu-id="88a1a-1005">Properties</span></span>

<span data-ttu-id="88a1a-1006">***プロパティ***は、オブジェクトまたはクラスの特性へのアクセスを提供するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="88a1a-1007">プロパティの例としては、文字列の長さ、フォントのサイズ、ウィンドウのキャプション、顧客名などがあります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="88a1a-1008">プロパティはフィールドの自然な拡張機能であり、どちらも、関連付けられた型を持つ名前付きのメンバーであり、フィールドとプロパティにアクセスするための構文は同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="88a1a-1009">ただし、フィールドとは異なり、プロパティは格納場所を表しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="88a1a-1010">その代わりに、プロパティには、値の読み取りまたは書き込みの際に実行されるステートメントを指定する "***アクセサー***" があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="88a1a-1011">そのため、プロパティは、アクションをオブジェクトの属性の読み取りと書き込みに関連付けるメカニズムを提供します。さらに、このような属性を計算することもできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="88a1a-1012">プロパティは*property_declaration*s を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1012">Properties are declared using *property_declaration*s:</span></span>

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

<span data-ttu-id="88a1a-1013">*Property_declaration*には、*属性*のセット ([属性](attributes.md)) と、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers))、@no__t 4 ([新しい修飾子](classes.md#the-new-modifier))、`static` ([Static および instance) の有効な組み合わせを含めることができます。メソッド](classes.md#static-and-instance-methods))、@no__t 8 ([仮想メソッド](classes.md#virtual-methods))、0 ([オーバーライドメソッド](classes.md#override-methods))、2 ([シールメソッド](classes.md#sealed-methods))、4 ([抽象メソッド](classes.md#abstract-methods))、および 6 ([外部メソッド](classes.md#external-methods)) 修飾子。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="88a1a-1014">プロパティ宣言は、修飾子の有効な組み合わせに関して、メソッド宣言 ([メソッド](classes.md#methods)) と同じ規則に従います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="88a1a-1015">プロパティ宣言の*型*は、宣言によって導入されるプロパティの型を指定し、 *member_name*はプロパティの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="88a1a-1016">プロパティが明示的なインターフェイスメンバーの実装でない限り、 *member_name*は単なる*識別子*です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="88a1a-1017">明示的なインターフェイスメンバーの実装 ([明示的なインターフェイスメンバー](interfaces.md#explicit-interface-member-implementations)の実装) の場合、 *member_name*は、 *interface_type*の後に "`.`" と*識別子*を続けたもので構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="88a1a-1018">プロパティの*型*は、少なくともプロパティ自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="88a1a-1019">*Property_body*は、***アクセサー本体***または***式の本体***で構成されている場合があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="88a1a-1020">アクセサー本体では、 *accessor_declarations*を "`{`" および "@no__t" トークンで囲む必要があり、プロパティのアクセサー ([アクセサー](classes.md#accessors)) を宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="88a1a-1021">アクセサーは、プロパティの読み取りと書き込みに関連付けられた実行可能なステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="88a1a-1022">で構成される式`=>`本体の後に*式* `E`とセミコロンが続く場合は、ステートメント本体`{ get { return E; } }`とまったく同じになります。したがって、次の結果が返される getter のみのプロパティを指定する場合にのみ使用できます。getter は、1つの式で指定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="88a1a-1023">*Property_initializer*は、自動的に実装されたプロパティ ([自動的に実装](classes.md#automatically-implemented-properties)されるプロパティ) に対してのみ指定でき、式によって指定された値を使用して、このようなプロパティの基になるフィールドを初期化します。.</span><span class="sxs-lookup"><span data-stu-id="88a1a-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="88a1a-1024">プロパティにアクセスするための構文は、フィールドの場合と同じですが、プロパティは変数として分類されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="88a1a-1025">したがって、プロパティを`ref`または`out`引数として渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="88a1a-1026">プロパティの宣言に`extern`修飾子が含まれている場合、プロパティは***外部プロパティ***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="88a1a-1027">外部プロパティの宣言は実際の実装を提供しないため、各*accessor_declarations*はセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="88a1a-1028">静的プロパティとインスタンスプロパティ</span><span class="sxs-lookup"><span data-stu-id="88a1a-1028">Static and instance properties</span></span>

<span data-ttu-id="88a1a-1029">プロパティの宣言に`static`修飾子が含まれている場合、プロパティは***静的プロパティ***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="88a1a-1030">修飾子が`static`存在しない場合、プロパティは***インスタンスプロパティ***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="88a1a-1031">静的プロパティは特定のインスタンスに関連付けられておらず、静的プロパティのアクセサー `this`で参照するコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="88a1a-1032">インスタンスプロパティは、クラスの特定のインスタンスに関連付けられます。このインスタンスには`this` 、そのプロパティのアクセサーで ([このアクセス](expressions.md#this-access)) としてアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="88a1a-1033">@No__t-2 の形式の*member_access* ([メンバーアクセス](expressions.md#member-access)) でプロパティが参照されている場合、`M` が静的プロパティである場合、`E` は `M` を含む型を示す必要があり、`M` がインスタンスプロパティの場合、E は型のインスタンスを示す必要があります。`M` を含む。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="88a1a-1034">静的メンバーとインスタンスメンバーの違いについては、「[静的メンバーとインスタンス](classes.md#static-and-instance-members)メンバー」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="88a1a-1035">アクセス</span><span class="sxs-lookup"><span data-stu-id="88a1a-1035">Accessors</span></span>

<span data-ttu-id="88a1a-1036">プロパティの*accessor_declarations*は、そのプロパティの読み取りと書き込みに関連付けられた実行可能なステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="88a1a-1037">アクセサー宣言は、 *get_accessor_declaration*、 *set_accessor_declaration*、またはその両方で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="88a1a-1038">各アクセサー宣言は、`get` または `set` の後に省略可能な*accessor_modifier*と*accessor_body*を指定したトークンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="88a1a-1039">*Accessor_modifier*s の使用には、次の制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="88a1a-1040">インターフェイスまたは明示的なインターフェイスメンバーの実装で*accessor_modifier*を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="88a1a-1041">@No__t-0 修飾子を持たないプロパティまたはインデクサーの場合、 *accessor_modifier*は、プロパティまたはインデクサーに @no__t と `set` の両方のアクセサーがある場合にのみ許可されます。その後、これらのアクセサーのいずれかでのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="88a1a-1042">@No__t 0 修飾子を含むプロパティまたはインデクサーの場合、アクセサーは、オーバーライドされるアクセサーの*accessor_modifier*(存在する場合) と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="88a1a-1043">*Accessor_modifier*は、プロパティまたはインデクサー自体の宣言されたアクセシビリティより厳密に制限されたアクセシビリティを宣言する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="88a1a-1044">正確である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1044">To be precise:</span></span>
   * <span data-ttu-id="88a1a-1045">プロパティまたはインデクサーに `public` のアクセシビリティが宣言されている場合、 *accessor_modifier*は `protected internal`、`internal`、`protected`、または `private` のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="88a1a-1046">プロパティまたはインデクサーに `protected internal` のアクセシビリティが宣言されている場合、 *accessor_modifier*は `internal`、`protected`、または `private` のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="88a1a-1047">プロパティまたはインデクサーに `internal` または `protected` のアクセシビリティが宣言されている場合、 *accessor_modifier*は `private` である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="88a1a-1048">プロパティまたはインデクサーに `private` のアクセシビリティが宣言されている場合、 *accessor_modifier*は使用できません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="88a1a-1049">@No__t-0 および `extern` プロパティの場合、指定された各アクセサーの*accessor_body*は、単純にセミコロンになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="88a1a-1050">非抽象の非 extern プロパティは、各*accessor_body*をセミコロンにすることができます。この場合、自動的***に実装***されるプロパティ ([自動的に実装](classes.md#automatically-implemented-properties)されるプロパティ) です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="88a1a-1051">自動的に実装されるプロパティには、少なくとも get アクセサーが必要です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="88a1a-1052">他の非抽象、非 extern プロパティのアクセサーの場合、 *accessor_body*は、対応するアクセサーが呼び出されたときに実行されるステートメントを指定する*ブロック*です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="88a1a-1053">アクセサー `get`は、プロパティの型の戻り値を持つパラメーターなしのメソッドに対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="88a1a-1054">代入の対象として、プロパティが式`get`で参照されている場合は、プロパティのアクセサーが呼び出され、プロパティ ([式の値](expressions.md#values-of-expressions)) の値が計算されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="88a1a-1055">`get`アクセサーの本体は、「[メソッド本体](classes.md#method-body)」で説明されている値を返すメソッドの規則に準拠している必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="88a1a-1056">特に、 `get`アクセサー `return`の本体内のすべてのステートメントでは、プロパティの型に暗黙的に変換できる式を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="88a1a-1057">さらに、 `get`アクセサーのエンドポイントに到達できないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="88a1a-1058">アクセサー `set`は、プロパティ型`void`の単一の値パラメーターと戻り値の型を持つメソッドに対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="88a1a-1059">`set`アクセサーの暗黙的なパラメーターには、常`value`にという名前が付けられます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="88a1a-1060">プロパティが代入 ([代入演算子](expressions.md#assignment-operators)) のターゲットとして参照されている場合、または`++`また`--`は ([後置インクリメントおよびデクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、[前置インクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)と前置デクリメント演算子) のオペランドとして参照されている場合、アクセサーは、新しい値 ([単純な代入](expressions.md#simple-assignment)) を提供する引数 (代入の右辺また`++`は or `--`演算子のオペランドの値を持つ) を使用して呼び出されます。 `set`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="88a1a-1061">`set`アクセサーの本体は、「[メソッド本体](classes.md#method-body)」で説明`void`されているメソッドの規則に準拠している必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="88a1a-1062">特に、 `return` `set`アクセサー本体のステートメントでは、式を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="88a1a-1063">アクセサーにはという名前`value`のパラメーターが暗黙的に含まれているため、 `set`アクセサー内のローカル変数または定数宣言でその名前が使用される場合、コンパイル時エラーになります。 `set`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="88a1a-1064">アクセサー `get` と`set`アクセサーの有無に基づいて、プロパティは次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="88a1a-1065">`get` アクセサー`set`とアクセサーの両方を含むプロパティは、***読み取り/書き込み***プロパティと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="88a1a-1066">`get`アクセサーだけを持つプロパティは、***読み取り***専用プロパティと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="88a1a-1067">読み取り専用プロパティが割り当てのターゲットである場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="88a1a-1068">`set`アクセサーだけを持つプロパティは、***書き込み専用***のプロパティと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="88a1a-1069">代入の対象となる場合を除き、式の書き込み専用プロパティを参照するコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="88a1a-1070">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1070">In the example</span></span>
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
<span data-ttu-id="88a1a-1071">コントロール`Button`は、パブリック`Caption`プロパティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="88a1a-1072">プロパティのアクセサーは`get` 、プライベート`caption`フィールドに格納されている文字列を返します。 `Caption`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="88a1a-1073">アクセサー `set`は、新しい値が現在の値と異なるかどうかを確認し、存在する場合は新しい値を格納して、コントロールを再描画します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="88a1a-1074">多くの場合、プロパティは上記のパターンに従います。アクセサー `get`は、プライベートフィールドに格納されている値を返す`set`だけで、アクセサーはそのプライベートフィールドを変更し、オブジェクトの状態を完全に更新するために必要な追加のアクションを実行します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="88a1a-1075">上記の`Caption`クラスを使用した場合、プロパティの使用例を次に示します。 `Button`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="88a1a-1076">ここでは、プロパティに値を割り当てることによって`get` アクセサーが呼び出され、式のプロパティを参照することによってアクセサーが呼び出されます。`set`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="88a1a-1077">プロパティの`set`アクセサーとアクセサーは、個別のメンバーではなく、プロパティのアクセサーを個別に宣言することはできません。`get`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="88a1a-1078">そのため、読み取り/書き込みプロパティの2つのアクセサーが異なるアクセシビリティを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="88a1a-1079">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-1079">The example</span></span>
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
<span data-ttu-id="88a1a-1080">は、1つの読み取り/書き込みプロパティを宣言しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="88a1a-1081">代わりに、同じ名前の2つのプロパティを宣言します。1つは読み取り専用で、もう1つは書き込み専用です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="88a1a-1082">同じクラスで宣言された2つのメンバーは同じ名前を持つことができないため、この例ではコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="88a1a-1083">派生クラスで、継承されたプロパティと同じ名前のプロパティが宣言されている場合、派生プロパティは、読み取りと書き込みの両方に関して継承されたプロパティを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="88a1a-1084">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1084">In the example</span></span>
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
<span data-ttu-id="88a1a-1085">の`P` `P` `A`プロパティは、読み取りと書き込みの両方に関して、のプロパティを非表示にします。 `B`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="88a1a-1086">そのため、ステートメントでは、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="88a1a-1087">の値を`b.P`代入すると、コンパイル時エラーが報告されます。の読み取り`P`専用プロパティ`B`は、の`A`書き込み専用`P`プロパティを非表示にするためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="88a1a-1088">ただし、キャストを使用して非表示`P`のプロパティにアクセスできることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="88a1a-1089">パブリックフィールドとは異なり、プロパティによって、オブジェクトの内部状態とそのパブリックインターフェイスが分離されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="88a1a-1090">例を考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1090">Consider the example:</span></span>
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="88a1a-1091">ここでは`Label` 、クラスは`int` 2 つ`x`の`y`フィールドとを使用して、その場所を格納します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="88a1a-1092">`X`この場所は、 `Y`およびプロパティ`Location`として、および型`Point`のプロパティとして公開されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="88a1a-1093">の将来の`Label`バージョンでは、 `Point`内部的にとして場所を格納する方が便利な場合があります。この変更は、クラスのパブリックインターフェイスに影響を与えることなく行うことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="88a1a-1094">では`x` なく`public readonly`フィールドがあったので、このような変更を`Label`クラスに加えることはできませんでした。 `y`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="88a1a-1095">プロパティを通じて状態を公開することは、フィールドを直接公開するよりも効率が低いとは限りません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="88a1a-1096">特に、プロパティが非仮想で、少量のコードのみが含まれている場合、実行環境では、アクセサーの呼び出しをアクセサーの実際のコードに置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="88a1a-1097">このプロセスは***インライン展開***と呼ばれ、フィールドアクセスと同じようにプロパティへのアクセスが可能になり、プロパティの柔軟性が向上します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="88a1a-1098">`get`アクセサーの呼び出しは、概念的にはフィールドの値の読み取りと同等であるため、 `get`アクセサーが観察可能な副作用を持つように、不適切なプログラミングスタイルと見なされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="88a1a-1099">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="88a1a-1100">`Next`プロパティの値は、プロパティが以前にアクセスされた回数によって異なります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="88a1a-1101">したがって、プロパティにアクセスすると、監視可能な副作用が生成されます。プロパティは、代わりにメソッドとして実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="88a1a-1102">アクセサーに対する`get` "副作用なし" の規則は、フィールドに`get`格納されている値だけを返すようにアクセサーを常に記述することを意味するわけではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="88a1a-1103">実際、 `get`アクセサーは、複数のフィールドにアクセスしたりメソッドを呼び出したりすることによって、多くの場合、プロパティの値を計算します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="88a1a-1104">ただし、適切にデザイン`get`されたアクセサーでは、オブジェクトの状態に対して監視可能な変更を引き起こすアクションは実行されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="88a1a-1105">プロパティを使用して、リソースが最初に参照されるまで、リソースの初期化を遅延させることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="88a1a-1106">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1106">For example:</span></span>
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

<span data-ttu-id="88a1a-1107">クラス`Console`には、、、 `In`および`Out`と`Error`いう3つのプロパティが含まれています。これらのプロパティは、標準入力、出力、およびエラーデバイスをそれぞれ表します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="88a1a-1108">これらのメンバーをプロパティとして`Console`公開することにより、クラスは実際に使用されるまで初期化を遅延させることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="88a1a-1109">たとえば、最初にプロパティを参照`Out`したときに、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="88a1a-1110">出力デバイス`TextWriter`の基になるが作成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="88a1a-1111">ただし、アプリケーションがプロパティ`In`と`Error`プロパティを参照しない場合、これらのデバイスのオブジェクトは作成されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="88a1a-1112">自動的に実装されたプロパティ</span><span class="sxs-lookup"><span data-stu-id="88a1a-1112">Automatically implemented properties</span></span>

<span data-ttu-id="88a1a-1113">自動的に実装されるプロパティ (または short の***自動プロパティ***) は、セミコロン専用のアクセサー本体を持つ非抽象非 extern プロパティです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="88a1a-1114">自動プロパティには get アクセサーが必要であり、オプションで set アクセサーを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="88a1a-1115">プロパティが自動的に実装されるプロパティとして指定されている場合は、非表示のバッキングフィールドがプロパティで自動的に使用できるようになります。アクセサーは、そのバッキングフィールドに対して読み取りおよび書き込みを行うために実装されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="88a1a-1116">自動プロパティに set アクセサーがない場合、バッキングフィールドは ( `readonly` [読み取り専用フィールド](classes.md#readonly-fields)) と見なされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="88a1a-1117">`readonly`フィールドと同様に、getter のみの自動プロパティも、外側のクラスのコンストラクターの本体でに割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="88a1a-1118">この割り当ては、プロパティの readonly バッキングフィールドに直接割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="88a1a-1119">自動プロパティには、必要に応じて*property_initializer*を含めることができます。これは、 *variable_initializer* ([変数初期化子](classes.md#variable-initializers)) としてバッキングフィールドに直接適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="88a1a-1120">次のような例です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="88a1a-1121">は、次の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="88a1a-1122">次のような例です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="88a1a-1123">は、次の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1123">is equivalent to the following declaration:</span></span>
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

<span data-ttu-id="88a1a-1124">読み取り専用フィールドへの割り当ては、コンストラクター内で行われるため、有効です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="88a1a-1125">ユーザー補助</span><span class="sxs-lookup"><span data-stu-id="88a1a-1125">Accessibility</span></span>

<span data-ttu-id="88a1a-1126">アクセサーに*accessor_modifier*がある場合、アクセサーのアクセシビリティドメイン ([アクセシビリティ](basic-concepts.md#accessibility-domains)ドメイン) は、 *accessor_modifier*の宣言されたアクセシビリティを使用して決定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="88a1a-1127">アクセサーに*accessor_modifier*がない場合、アクセサーのアクセシビリティドメインは、プロパティまたはインデクサーの宣言されたアクセシビリティから決まります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="88a1a-1128">*Accessor_modifier*が存在しても、メンバー参照 ([演算子](expressions.md#operators)) やオーバーロードの解決 ([オーバーロードの解決](expressions.md#overload-resolution)) には影響しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="88a1a-1129">プロパティまたはインデクサーの修飾子は、アクセスのコンテキストに関係なく、バインド先のプロパティまたはインデクサーを常に決定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="88a1a-1130">特定のプロパティまたはインデクサーが選択されると、その使用法が有効かどうかを判断するために、関連する特定のアクセサーのアクセシビリティドメインが使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="88a1a-1131">使用量が値 ([式の値](expressions.md#values-of-expressions)) `get`である場合は、アクセサーが存在し、アクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="88a1a-1132">単純な代入 ([単純な代入](expressions.md#simple-assignment)) `set`の対象として使用する場合は、アクセサーが存在し、アクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="88a1a-1133">使用量が複合代入 ([複合代入](expressions.md#compound-assignment)) の`++`ターゲットであるか、または`--`演算子 ([関数メンバー](expressions.md#function-members).9、[呼び出し式](expressions.md#invocation-expressions)) `get`のターゲットとして使用されている場合、アクセサーとアクセサー `set`は存在し、アクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="88a1a-1134">次の例`A.Text`では、 `set`アクセサーだけが呼び出されて`B.Text`いるコンテキストでも、プロパティはプロパティによって非表示になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="88a1a-1135">これに対して、 `B.Count`プロパティはクラス`M`にアクセスできないため、代わりに`A.Count`アクセス可能なプロパティが使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

<span data-ttu-id="88a1a-1136">インターフェイスを実装するために使用されるアクセサーは、 *accessor_modifier*を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="88a1a-1137">インターフェイスを実装するために使用されるアクセサーが1つだけの場合、もう一方のアクセサーは*accessor_modifier*を使用して宣言できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="88a1a-1138">仮想、シール、オーバーライド、および抽象プロパティアクセサー</span><span class="sxs-lookup"><span data-stu-id="88a1a-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="88a1a-1139">プロパティ`virtual`の宣言では、プロパティのアクセサーが仮想であることを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="88a1a-1140">修飾子`virtual`は、読み取り/書き込みプロパティの両方のアクセサーに適用されます。読み取り/書き込みプロパティの1つのアクセサーだけを仮想にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="88a1a-1141">`abstract`プロパティの宣言では、プロパティのアクセサーが仮想であることを指定しますが、アクセサーの実際の実装は提供しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="88a1a-1142">代わりに、抽象でない派生クラスは、プロパティをオーバーライドすることによって、アクセサーの独自の実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="88a1a-1143">抽象プロパティ宣言のアクセサーは実際の実装を提供しないため、その*accessor_body*は単純にセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="88a1a-1144">修飾子と`abstract` `override`修飾子の両方を含むプロパティ宣言では、プロパティが abstract で、基本プロパティがオーバーライドされることを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="88a1a-1145">このようなプロパティのアクセサーも抽象型です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="88a1a-1146">抽象プロパティ宣言は、抽象クラス ([抽象クラス](classes.md#abstract-classes)) でのみ許可されます。継承された仮想プロパティのアクセサーは、 `override`ディレクティブを指定するプロパティ宣言を含めることによって、派生クラスでオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="88a1a-1147">これは、オーバーライドする***プロパティの宣言***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="88a1a-1148">オーバーライドするプロパティの宣言で、新しいプロパティが宣言されていません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="88a1a-1149">代わりに、既存の仮想プロパティのアクセサーの実装を単に特殊化します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="88a1a-1150">オーバーライドするプロパティの宣言では、継承されたプロパティとまったく同じアクセシビリティ修飾子、型、および名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="88a1a-1151">継承されたプロパティにアクセサーが1つしかない場合 (つまり、継承されたプロパティが読み取り専用または書き込み専用の場合)、オーバーライドする側のプロパティにはそのアクセサーだけを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="88a1a-1152">継承されたプロパティに両方のアクセサーが含まれている場合 (つまり、継承されたプロパティが読み取り/書き込みの場合)、オーバーライドする側のプロパティには、1つのアクセサーまたは両方のアクセサーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="88a1a-1153">オーバーライドするプロパティの宣言には`sealed` 、修飾子を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="88a1a-1154">この修飾子を使用すると、派生クラスでプロパティをさらにオーバーライドできなくなります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="88a1a-1155">シールされたプロパティのアクセサーもシールされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="88a1a-1156">宣言と呼び出しの構文の違いを除けば、virtual、sealed、override、および abstract の各アクセサーは、仮想、シール、オーバーライド、抽象メソッドとまったく同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="88a1a-1157">具体的には、「[仮想メソッド](classes.md#virtual-methods)」、 [「オーバーライドメソッド](classes.md#override-methods)」、「[シールメソッド](classes.md#sealed-methods)」、および「[抽象メソッド](classes.md#abstract-methods)」で説明されている規則は、アクセサーが対応する形式のメソッドである場合と同様に適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="88a1a-1158">アクセサー `get`は、プロパティの型の戻り値と、それを含むプロパティと同じ修飾子を持つパラメーターなしのメソッドに対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="88a1a-1159">アクセサー `set`は、プロパティ型`void`の単一の値パラメーター、戻り値の型、およびそれを含むプロパティと同じ修飾子を持つメソッドに対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="88a1a-1160">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1160">In the example</span></span>
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
<span data-ttu-id="88a1a-1161">`X`は仮想読み取り専用プロパティで、 `Y`は仮想読み取り/書き込み`Z`プロパティで、は抽象読み取り/書き込みプロパティです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="88a1a-1162">は`Z`抽象であるため、含ん`A`でいるクラスも abstract として宣言されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="88a1a-1163">から`A`派生するクラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1163">A class that derives from `A` is show below:</span></span>
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

<span data-ttu-id="88a1a-1164">ここでは、、 `X` `Y`、および`Z`の宣言は、プロパティ宣言をオーバーライドしています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="88a1a-1165">各プロパティ宣言は、対応する継承されたプロパティのアクセシビリティ修飾子、型、および名前と完全に一致します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="88a1a-1166">`base`の`get` アクセサー`X` と`set`のアクセサーは、キーワードを使用して、継承されたアクセサーにアクセスします。`Y`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="88a1a-1167">の宣言は`Z` 、両方の抽象アクセサーをオーバーライドします。したがって、に`B`は未処理の`B`抽象関数メンバーはなく、非抽象クラスであることが許可されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="88a1a-1168">プロパティがとして宣言さ`override`れている場合、オーバーライドされたアクセサーには、オーバーライドする側のコードからアクセスできる必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="88a1a-1169">さらに、プロパティまたはインデクサー自体とアクセサーの両方について宣言されたアクセシビリティは、オーバーライドされるメンバーとアクセサーのアクセシビリティと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="88a1a-1170">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1170">For example:</span></span>
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a><span data-ttu-id="88a1a-1171">イベント</span><span class="sxs-lookup"><span data-stu-id="88a1a-1171">Events</span></span>

<span data-ttu-id="88a1a-1172">***イベント***は、オブジェクトまたはクラスが通知を提供できるようにするメンバーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="88a1a-1173">クライアントは、***イベントハンドラー***を指定することによって、イベントの実行可能コードを添付できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="88a1a-1174">イベントは*event_declaration*s を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1174">Events are declared using *event_declaration*s:</span></span>

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

<span data-ttu-id="88a1a-1175">*Event_declaration*には、*属性*のセット ([属性](attributes.md)) と、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers))、@no__t 4 ([新しい修飾子](classes.md#the-new-modifier))、`static` ([Static および instance) の有効な組み合わせを含めることができます。メソッド](classes.md#static-and-instance-methods))、@no__t 8 ([仮想メソッド](classes.md#virtual-methods))、0 ([オーバーライドメソッド](classes.md#override-methods))、2 ([シールメソッド](classes.md#sealed-methods))、4 ([抽象メソッド](classes.md#abstract-methods))、および 6 ([外部メソッド](classes.md#external-methods)) 修飾子。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="88a1a-1176">イベント宣言には、修飾子の有効な組み合わせに関して、メソッド宣言 ([メソッド](classes.md#methods)) と同じ規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="88a1a-1177">イベント宣言の*型*は*delegate_type* ([参照型](types.md#reference-types)) である必要があります。また、 *delegate_type*は、少なくともイベント自体 ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints)) と同様にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="88a1a-1178">イベント宣言には、 *event_accessor_declarations*を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="88a1a-1179">ただし、非 extern の非抽象イベントの場合は、コンパイラによって自動的に提供されます ([フィールドに似たイベント](classes.md#field-like-events))。extern イベントの場合、アクセサーは外部に提供されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="88a1a-1180">*Event_accessor_declarations*を省略するイベント宣言では、1つ以上のイベント ( *variable_declarator*のそれぞれに1つ) を定義します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="88a1a-1181">属性と修飾子は、このような*event_declaration*によって宣言されたすべてのメンバーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="88a1a-1182">Event_declaration 修飾子と中かっこ区切りの*event_accessor_declarations*の @no__t 両方が含まれているのコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="88a1a-1183">イベント宣言に`extern`修飾子が含まれている場合、イベントは***外部イベント***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="88a1a-1184">外部イベント宣言は実際の実装を提供しないため、@no__t 0 修飾子と*event_accessor_declarations*の両方を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="88a1a-1185">@No__t-1 または @no__t 修飾子を持つイベント宣言の*variable_declarator*に*variable_initializer*を含めると、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="88a1a-1186">イベントは、 `+=`および`-=`演算子 ([イベント割り当て](expressions.md#event-assignment)) の左側のオペランドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="88a1a-1187">これらの演算子は、イベントからイベントハンドラーを削除するために、またはイベントからイベントハンドラーを削除するために、それぞれ使用されます。また、イベントのアクセス修飾子は、そのような操作が許可されているコンテキストを制御します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="88a1a-1188">`+=` と`-=`は、イベントを宣言する型以外のイベントで許可される唯一の操作であるため、外部コードはイベントのハンドラーを追加および削除できますが、それ以外のイベントの一覧を取得または変更することはできません。ハンドラー.</span><span class="sxs-lookup"><span data-stu-id="88a1a-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="88a1a-1189">`x += y`または`x` `x` `void`の形式の操作では、がイベントであり、の宣言を含む型の外で参照が行われる場合、操作の結果には型が含まれます ( `x -= y`の`x`型。代入後のの`x`値を使用します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="88a1a-1190">この規則は、外部コードがイベントの基になるデリゲートを間接的に調べることを禁止します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="88a1a-1191">次の例は、イベントハンドラーを`Button`クラスのインスタンスにアタッチする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

<span data-ttu-id="88a1a-1192">ここでは`LoginDialog` 、インスタンスコンストラクターは`Button` 2 つのインスタンスを作成し`Click` 、イベントにイベントハンドラーをアタッチします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="88a1a-1193">フィールドに似たイベント</span><span class="sxs-lookup"><span data-stu-id="88a1a-1193">Field-like events</span></span>

<span data-ttu-id="88a1a-1194">イベントの宣言を含むクラスまたは構造体のプログラムテキスト内では、フィールドのように特定のイベントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="88a1a-1195">この方法で使用するには、イベントを @no__t 0 または `extern` にすることはできません。また、 *event_accessor_declarations*を明示的に含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="88a1a-1196">このようなイベントは、フィールドを許可する任意のコンテキストで使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="88a1a-1197">フィールドには、イベントに追加されたイベントハンドラーのリストを参照するデリゲート ([デリゲート](delegates.md)) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="88a1a-1198">イベントハンドラーが追加されていない場合、 `null`フィールドにはが含まれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="88a1a-1199">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1199">In the example</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
<span data-ttu-id="88a1a-1200">`Click`は、 `Button`クラス内のフィールドとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="88a1a-1201">この例で示すように、フィールドは、デリゲート呼び出し式で検査、変更、および使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="88a1a-1202">クラスのメソッド`OnClick`は、イベントを`Click` "発生" させます。 `Button`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="88a1a-1203">イベントを発生させるという概念は、イベントによって表されるデリゲートの呼び出しとまったく同じです。したがって、イベントを発生させるための特殊な言語コンストラクトはありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="88a1a-1204">デリゲート呼び出しの前に、デリゲートが null でないことを確認するチェックが付いていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="88a1a-1205">`Button`クラスの宣言の外側では、 `Click`メンバーは、のように、 `+=`および`-=`演算子の左辺でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="88a1a-1206">`Click`イベントの呼び出しリストにデリゲートを追加します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="88a1a-1207">これにより、 `Click`イベントの呼び出しリストからデリゲートが削除されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="88a1a-1208">フィールドに似たイベントをコンパイルすると、コンパイラは、デリゲートを保持するためのストレージを自動的に作成し、デリゲートフィールドに対してイベントハンドラーを追加または削除するイベントのアクセサーを作成します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="88a1a-1209">追加と削除の操作は、スレッドセーフであり、インスタンスイベントの格納オブジェクトに対してロックを保持する ([ロックステートメント](statements.md#the-lock-statement)) か、または型オブジェクト ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) を保持している間に、実行する必要があります (ただし、必須ではありません)。静的イベントの場合。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="88a1a-1210">したがって、次のような形式のインスタンスイベント宣言があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="88a1a-1211">は、次のものに相当するものにコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1211">will be compiled to something equivalent to:</span></span>
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
<span data-ttu-id="88a1a-1212">クラス`X`内で、 `Ev` `+=` および`-=`演算子の左辺のを参照すると、add アクセサーと remove アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="88a1a-1213">へ`Ev`の他のすべての参照は、非表示`__Ev`フィールドを参照するようにコンパイルされます ([メンバーアクセス](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="88a1a-1214">"`__Ev`" という名前は任意です。非表示フィールドには任意の名前を付けることも、名前をまったく指定しないこともできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="88a1a-1215">イベント アクセサー</span><span class="sxs-lookup"><span data-stu-id="88a1a-1215">Event accessors</span></span>

<span data-ttu-id="88a1a-1216">イベント宣言は、通常、上の `Button` の例のように、 *event_accessor_declarations*を省略しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="88a1a-1217">これを行う1つの状況として、イベントごとに1つのフィールドのストレージコストが許容されない場合が挙げられます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="88a1a-1218">このような場合は、クラスに*event_accessor_declarations*を含めて、イベントハンドラーのリストを格納するためのプライベートメカニズムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="88a1a-1219">イベントの*event_accessor_declarations*は、イベントハンドラーの追加と削除に関連付けられた実行可能なステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="88a1a-1220">アクセサー宣言は、 *add_accessor_declaration*と*remove_accessor_declaration*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="88a1a-1221">各アクセサー宣言は、トークン`add`または`remove`それに続く*ブロック*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="88a1a-1222">*Add_accessor_declaration*に関連付けられている*ブロック*は、イベントハンドラーが追加されたときに実行するステートメントを指定します。また、 *remove_accessor_declaration*に関連付けられている*ブロック*には、実行するステートメントを指定します。イベントハンドラーが削除されたとき。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="88a1a-1223">各*add_accessor_declaration*と*remove_accessor_declaration*は、イベントの種類の1つの値パラメーターと @no__t 2 つの戻り値の型を持つメソッドに対応しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="88a1a-1224">イベントアクセサーの暗黙のパラメーターにはと`value`いう名前が付けられます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="88a1a-1225">イベントの割り当てでイベントが使用される場合は、適切なイベントアクセサーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="88a1a-1226">具体的には`+=` 、代入演算子がの場合、add アクセサーが使用され、代入演算子が`-=`の場合は remove アクセサーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="88a1a-1227">どちらの場合も、代入演算子の右側のオペランドは、イベントアクセサーの引数として使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="88a1a-1228">*Add_accessor_declaration*または*remove_accessor_declaration*のブロックは、「[メソッド本体](classes.md#method-body)」で説明されている @no__t 2 つのメソッドの規則に従っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="88a1a-1229">特に、 `return`このようなブロック内のステートメントでは、式を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="88a1a-1230">イベントアクセサーはという名前`value`のパラメーターを暗黙的に持つため、イベントアクセサーで宣言されたローカル変数または定数がその名前を持つようにすると、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="88a1a-1231">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1231">In the example</span></span>
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
<span data-ttu-id="88a1a-1232">クラス`Control`は、イベントの内部ストレージ機構を実装します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="88a1a-1233">メソッド`AddEventHandler`は、デリゲート値をキーに関連付けます。 `GetEventHandler`メソッドは、現在キーに関連付けられている`RemoveEventHandler`デリゲートを返し、メソッドは、指定されたイベントのイベントハンドラーとしてデリゲートを削除します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="88a1a-1234">おそらく、基になるストレージメカニズムは、デリゲート値を`null`キーに関連付けてもコストが発生しないように設計されているため、未処理のイベントはストレージを使用しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="88a1a-1235">静的イベントとインスタンスイベント</span><span class="sxs-lookup"><span data-stu-id="88a1a-1235">Static and instance events</span></span>

<span data-ttu-id="88a1a-1236">イベント宣言に`static`修飾子が含まれている場合、イベントは***静的イベント***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="88a1a-1237">修飾子が`static`存在しない場合、イベントは***インスタンスイベント***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="88a1a-1238">静的イベントは特定のインスタンスに関連付けられておらず、静的イベントのアクセサー `this`で参照するコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="88a1a-1239">インスタンスイベントは、クラスの特定のインスタンスに関連付けられます。このインスタンスには`this` 、そのイベントのアクセサーで ([このアクセス](expressions.md#this-access)) としてアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="88a1a-1240">@No__t-2 の形式の*member_access* ([メンバーアクセス](expressions.md#member-access)) でイベントが参照されている場合、`M` が静的イベントである場合、`E` は `M` を含む型を示す必要があり、`M` がインスタンスイベントの場合、E は、を含む型のインスタンスを示す必要があります。`M`。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="88a1a-1241">静的メンバーとインスタンスメンバーの違いについては、「[静的メンバーとインスタンス](classes.md#static-and-instance-members)メンバー」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="88a1a-1242">仮想、シール、オーバーライド、および抽象イベントアクセサー</span><span class="sxs-lookup"><span data-stu-id="88a1a-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="88a1a-1243">イベント`virtual`宣言は、そのイベントのアクセサーが仮想であることを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="88a1a-1244">修飾子`virtual`は、イベントの両方のアクセサーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="88a1a-1245">イベント`abstract`宣言では、イベントのアクセサーが仮想であることを指定しますが、アクセサーの実際の実装は提供しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="88a1a-1246">代わりに、非抽象派生クラスは、イベントをオーバーライドすることによって、アクセサーの独自の実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="88a1a-1247">抽象イベント宣言は実際の実装を提供しないため、中かっこで区切られた*event_accessor_declarations*を提供することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="88a1a-1248">修飾子と`abstract` `override`修飾子の両方を含むイベント宣言は、イベントが抽象であり、基本イベントをオーバーライドすることを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="88a1a-1249">このようなイベントのアクセサーも抽象的なものです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="88a1a-1250">抽象イベント宣言は、抽象クラス ([抽象クラス](classes.md#abstract-classes)) でのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="88a1a-1251">継承された仮想イベントのアクセサーは、 `override`修飾子を指定するイベント宣言を含めることによって、派生クラスでオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="88a1a-1252">これは、オーバーライドする***イベント宣言***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="88a1a-1253">オーバーライドするイベント宣言では、新しいイベントが宣言されていません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="88a1a-1254">代わりに、既存の仮想イベントのアクセサーの実装を単に特殊化します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="88a1a-1255">オーバーライドするイベント宣言では、オーバーライドされたイベントとまったく同じアクセシビリティ修飾子、型、および名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="88a1a-1256">オーバーライドするイベント宣言には、 `sealed`修飾子を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="88a1a-1257">この修飾子を使用すると、派生クラスでイベントをさらにオーバーライドできなくなります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="88a1a-1258">シールされたイベントのアクセサーもシールされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="88a1a-1259">オーバーライドするイベント宣言に`new`修飾子が含まれていると、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="88a1a-1260">宣言と呼び出しの構文の違いを除けば、virtual、sealed、override、および abstract の各アクセサーは、仮想、シール、オーバーライド、抽象メソッドとまったく同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="88a1a-1261">具体的には、「[仮想メソッド](classes.md#virtual-methods)」、 [「オーバーライドメソッド](classes.md#override-methods)」、「[シールメソッド](classes.md#sealed-methods)」、および「[抽象メソッド](classes.md#abstract-methods)」で説明されている規則は、アクセサーが対応するフォームのメソッドである場合と同様に適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="88a1a-1262">各アクセサーは、イベントの種類の単一の値パラメーター、 `void`戻り値の型、および親イベントと同じ修飾子を持つメソッドに対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="88a1a-1263">インデクサー</span><span class="sxs-lookup"><span data-stu-id="88a1a-1263">Indexers</span></span>

<span data-ttu-id="88a1a-1264">***インデクサー***は、配列と同じ方法でオブジェクトのインデックスを作成できるようにするメンバーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="88a1a-1265">インデクサーは*indexer_declaration*s を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1265">Indexers are declared using *indexer_declaration*s:</span></span>

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

<span data-ttu-id="88a1a-1266">*Indexer_declaration*には、*属性*のセット ([属性](attributes.md)) と、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせ、@no__t 4 ([新しい修飾子](classes.md#the-new-modifier))、`virtual` ([仮想メソッド) が含まれる場合があります。](classes.md#virtual-methods))、`override` ([オーバーライドメソッド](classes.md#override-methods))、@no__t 10 ([シールメソッド](classes.md#sealed-methods))、2 ([抽象メソッド](classes.md#abstract-methods))、および 4 ([外部メソッド](classes.md#external-methods)) 修飾子。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="88a1a-1267">インデクサー宣言では、修飾子の有効な組み合わせに関して、メソッド宣言 ([メソッド](classes.md#methods)) と同じ規則が適用されます。一方、インデクサー宣言では static 修飾子は許可されないという例外があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="88a1a-1268">修飾子`virtual` `abstract` 、 `override`、およびは、1つの場合を除いて、相互に排他的です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="88a1a-1269">`abstract` および`override`修飾子を一緒に使用して、抽象インデクサーが仮想1をオーバーライドできるようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="88a1a-1270">インデクサー宣言の*型*は、宣言によって導入されるインデクサーの要素の種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="88a1a-1271">インデクサーが明示的なインターフェイスメンバーの実装でない限り、*型*の後に`this`キーワードが続きます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="88a1a-1272">明示的なインターフェイスメンバーの実装では、*型*の後に*interface_type*、"`.`"、キーワード `this` が続きます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="88a1a-1273">他のメンバーとは異なり、インデクサーにはユーザー定義の名前がありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="88a1a-1274">*Formal_parameter_list*は、インデクサーのパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="88a1a-1275">インデクサーの仮パラメーターリストは、少なく`ref`とも1つのパラメーターを指定する必要があり、および`out`パラメーター修飾子が許可されていない点を除いて、メソッドのパラメーター ([メソッドパラメーター](classes.md#method-parameters)) に対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="88a1a-1276">インデクサーの*型*と、 *formal_parameter_list*で参照される各型は、少なくともインデクサー自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="88a1a-1277">*Indexer_body*は、***アクセサー本体***または***式の本体***で構成されている場合があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="88a1a-1278">アクセサー本体では、 *accessor_declarations*を "`{`" および "@no__t" トークンで囲む必要があり、プロパティのアクセサー ([アクセサー](classes.md#accessors)) を宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="88a1a-1279">アクセサーは、プロパティの読み取りと書き込みに関連付けられた実行可能なステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="88a1a-1280">"`=>`" の後に式`E`とセミコロンが続く式本体は、ステートメント本体`{ get { return E; } }`とまったく同じであり、そのため、getter の結果がである getter のみのインデクサーを指定するためにのみ使用できます。1つの式によって指定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="88a1a-1281">インデクサー要素にアクセスするための構文は、配列要素の構文と同じですが、インデクサー要素は変数として分類されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="88a1a-1282">このため、インデクサー要素を引数`ref`または`out`引数として渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="88a1a-1283">インデクサーの仮パラメーターリストでは、インデクサーの署名 ([シグネチャとオーバーロード](basic-concepts.md#signatures-and-overloading)) を定義します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="88a1a-1284">具体的には、インデクサーのシグネチャは、その仮パラメーターの数と型で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="88a1a-1285">仮パラメーターの要素の型と名前は、インデクサーのシグネチャの一部ではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="88a1a-1286">インデクサーのシグネチャは、同じクラスで宣言されている他のすべてのインデクサーのシグネチャと異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="88a1a-1287">インデクサーとプロパティは概念とよく似ていますが、次の点が異なります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="88a1a-1288">プロパティは名前で識別されますが、インデクサーはそのシグネチャによって識別されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="88a1a-1289">プロパティは、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバーアクセス](expressions.md#member-access)) を介してアクセスされます。一方、インデクサー要素には、 *element_access* ([インデクサーアクセス](expressions.md#indexer-access)) を介してアクセスします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="88a1a-1290">プロパティは`static`メンバーにすることができますが、インデクサーは常にインスタンスメンバーである必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="88a1a-1291">プロパティの`get`アクセサーは、パラメーターのないメソッドに対応します。一方、インデクサーのアクセサーは、インデクサーと同じ仮パラメーターリストを持つメソッドに対応します。 `get`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="88a1a-1292">プロパティ`set`のアクセサーは、という名前`value`の1つのパラメーターを持つメソッドに`set`対応します。一方、インデクサーのアクセサーは、インデクサーと同じ仮パラメーターリストと追加パラメーターを持つメソッドに対応します。名前`value`付き。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="88a1a-1293">インデクサーアクセサーがインデクサーパラメーターと同じ名前を持つローカル変数を宣言する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="88a1a-1294">オーバーライドするプロパティの宣言では、という構文`base.P`を使用して、継承されたプロパティにアクセスします。ここ`P`で、はプロパティ名です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="88a1a-1295">オーバーライドするインデクサーの宣言では、構文`base[E]`を使用して、継承されたインデクサーにアクセスします。ここ`E`で、は式のコンマ区切りリストです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="88a1a-1296">"自動的に実装されたインデクサー" の概念はありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="88a1a-1297">セミコロン (;) 以外の非抽象インデクサーを使用すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="88a1a-1298">これらの違いを除けば、[アクセサー](classes.md#accessors)と[自動的に実装](classes.md#automatically-implemented-properties)されるプロパティで定義されているすべての規則は、インデクサーアクセサーおよびプロパティアクセサーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="88a1a-1299">インデクサー宣言に`extern`修飾子が含まれている場合、インデクサーは***外部インデクサー***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="88a1a-1300">外部インデクサー宣言は実際の実装を提供しないため、各*accessor_declarations*はセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="88a1a-1301">次の例では`BitArray` 、ビット配列内の個々のビットにアクセスするためのインデクサーを実装するクラスを宣言しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

<span data-ttu-id="88a1a-1302">`BitArray`クラスのインスタンスは、対応`bool[]`するよりもかなり少ないメモリを消費し`bool[]`ます (前者の各値は、後者の1バイトではなく1ビットのみを占有するため)。ただし、と同じ操作を許可します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="88a1a-1303">次`CountPrimes`のクラスは、 `BitArray`と従来の "エラトステネス" アルゴリズムを使用して、1から指定された最大値までの primes 数を計算します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

<span data-ttu-id="88a1a-1304">の`BitArray`要素にアクセスするための構文は、 `bool[]`の場合とまったく同じであることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="88a1a-1305">次の例は、2つのパラメーターを持つインデクサーを持つ 26 \* 10 grid クラスを示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="88a1a-1306">最初のパラメーターは、A ~ Z の範囲の大文字または小文字にする必要があり、2番目のパラメーターは0-9 の範囲の整数である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a><span data-ttu-id="88a1a-1307">インデクサーのオーバーロード</span><span class="sxs-lookup"><span data-stu-id="88a1a-1307">Indexer overloading</span></span>

<span data-ttu-id="88a1a-1308">インデクサーのオーバーロードの解決規則については、 [「型の推定](expressions.md#type-inference)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="88a1a-1309">演算子</span><span class="sxs-lookup"><span data-stu-id="88a1a-1309">Operators</span></span>

<span data-ttu-id="88a1a-1310">***演算子***は、クラスのインスタンスに適用できる式演算子の意味を定義するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="88a1a-1311">演算子は*operator_declaration*s を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1311">Operators are declared using *operator_declaration*s:</span></span>

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="88a1a-1312">オーバーロードされた演算子には、次の3つのカテゴリがあります。単項演算子 ([単項](classes.md#unary-operators)演算子)、二項演算子 ([二項](classes.md#binary-operators)演算子)、および変換演算子 ([変換演算子](classes.md#conversion-operators))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="88a1a-1313">*Operator_body*は、セミコロン、***ステートメント本体***、または式の***本体***です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="88a1a-1314">ステートメントの本体は、*ブロック*で構成されます。これは、演算子が呼び出されたときに実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="88a1a-1315">*ブロック*は、「[メソッド本体](classes.md#method-body)」で説明されている値を返すメソッドの規則に準拠している必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="88a1a-1316">式の`=>`本体は、式とセミコロンで構成され、その後に演算子が呼び出されたときに実行する1つの式を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="88a1a-1317">@No__t 0 演算子の場合、 *operator_body*はセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="88a1a-1318">その他のすべての演算子では、 *operator_body*はブロック本体または式の本体です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="88a1a-1319">すべての演算子宣言には、次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="88a1a-1320">演算子宣言には、 `public` `static`と修飾子の両方を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="88a1a-1321">演算子のパラメーターは、値パラメーター ([値パラメーター](variables.md#value-parameters)) である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="88a1a-1322">演算子宣言でまたは`ref` `out`パラメーターを指定する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="88a1a-1323">演算子のシグネチャ ([単項演算子](classes.md#unary-operators)、[二項演算子](classes.md#binary-operators)、[変換演算子](classes.md#conversion-operators)) は、同じクラスで宣言されている他のすべての演算子のシグネチャとは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="88a1a-1324">演算子宣言で参照されるすべての型は、少なくとも演算子自体 ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints)) と同じようにアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="88a1a-1325">同じ修飾子が演算子宣言で複数回出現する場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="88a1a-1326">各演算子カテゴリでは、次のセクションで説明するように、追加の制限が課せられます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="88a1a-1327">他のメンバーと同様に、基底クラスで宣言された演算子は、派生クラスによって継承されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="88a1a-1328">演算子宣言は、演算子のシグネチャに参加するように宣言されているクラスまたは構造体を常に必要とするため、派生クラスで宣言された演算子が基底クラスで宣言された演算子を非表示にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="88a1a-1329">したがって、 `new`演算子の宣言では、修飾子は不要であり、許可されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="88a1a-1330">単項演算子と二項演算子の詳細については、「[演算子](expressions.md#operators)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="88a1a-1331">変換演算子の追加情報については、「[ユーザー定義変換](conversions.md#user-defined-conversions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="88a1a-1332">単項演算子</span><span class="sxs-lookup"><span data-stu-id="88a1a-1332">Unary operators</span></span>

<span data-ttu-id="88a1a-1333">次の規則は、単項演算子の宣言`T`に適用されます。は、演算子宣言を含むクラスまたは構造体のインスタンス型を表します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="88a1a-1334">単項`+` `-` `~` `T` 、、 `T?` 、または演算子は、型または型の1つのパラメーターを受け取る必要があり、任意の型を返すことができます。 `!`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="88a1a-1335">単項`++` or `--`演算子は、型または`T?`型`T`の1つのパラメーターを受け取る必要があり、同じ型またはその派生型を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="88a1a-1336">単項`true` or `false`演算子は、型`T`または`T?`型の1つのパラメーターを`bool`受け取る必要があり、型を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="88a1a-1337">`+`単項演算子のシグネチャは、演算子トークン ( `-` `!`、、、 `true` `--` `~` `++`、、、、または`false`) と、単一の仮パラメーターの型で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="88a1a-1338">戻り値の型は単項演算子のシグネチャの一部ではなく、仮パラメーターの名前でもありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="88a1a-1339">`true` および`false`の単項演算子には、ペアごとの宣言が必要です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="88a1a-1340">クラスでこれらの演算子のいずれかが宣言されていない場合、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="88a1a-1341">および演算子については、[ユーザー定義の条件付き論理演算子](expressions.md#user-defined-conditional-logical-operators)と[ブール式](expressions.md#boolean-expressions)でさらに説明します。 `false` `true`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="88a1a-1342">次の例は、整数 vector クラスの実装`operator ++`とその後の使用方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

<span data-ttu-id="88a1a-1343">演算子メソッドは、後置インクリメント演算子と前置デクリメント演算子 ([後置](expressions.md#postfix-increment-and-decrement-operators)インクリメント演算子およびデクリメント演算子) と同様に、オペランドに1を加算することによって生成される値を返す方法に注意してください。前置インクリメント演算子と前置デクリメント演算子 ([前置インクリメント演算子とデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="88a1a-1344">とはC++異なり、このメソッドでは、オペランドの値を直接変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="88a1a-1345">実際、オペランドの値を変更すると、後置インクリメント演算子の標準セマンティクスに違反することになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="88a1a-1346">バイナリ演算子</span><span class="sxs-lookup"><span data-stu-id="88a1a-1346">Binary operators</span></span>

<span data-ttu-id="88a1a-1347">次の規則は、二項演算子の宣言に`T`適用されます。は、演算子宣言を含むクラスまたは構造体のインスタンス型を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="88a1a-1348">バイナリ以外のシフト演算子は、2つのパラメーターを受け取る必要があります。少なく`T`と`T?`も1つは型またはを持つ必要があり、任意の型を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="88a1a-1349">`<<`バイナリまたは`>>`演算子は、2つのパラメーターを受け取る必要があります`T` 。最初のパラメーターの型はまたは`int` `T?`である必要があり、2番目のパラメーターは型または`int?`型である必要があり、任意の型を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="88a1a-1350">`+`二項演算子のシグネチャは、演算子トークン ( `%` `*`、 `-` `/` `&` 、、`>>`、、、、、、、) で構成されます。 `|` `^` `<<``==` 、`!=`、 、`<`、、または`<=`) と、2つの仮パラメーターの型。 `>=` `>`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="88a1a-1351">戻り値の型と仮パラメーターの名前は、二項演算子のシグネチャの一部ではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="88a1a-1352">特定の二項演算子には、ペアごとの宣言が必要です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="88a1a-1353">ペアのいずれかの演算子を宣言する場合は、そのペアのもう一方の演算子の宣言が一致している必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="88a1a-1354">2つの演算子宣言は、同じ戻り値の型を持ち、各パラメーターの型が同じである場合に一致します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="88a1a-1355">次の演算子では、ペアごとの宣言が必要です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="88a1a-1356">`operator ==` および `operator !=`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="88a1a-1357">`operator >` および `operator <`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="88a1a-1358">`operator >=` および `operator <=`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="88a1a-1359">変換演算子</span><span class="sxs-lookup"><span data-stu-id="88a1a-1359">Conversion operators</span></span>

<span data-ttu-id="88a1a-1360">変換演算子の宣言では、定義済みの暗黙的な変換と明示的な変換を補強する、***ユーザー定義の変換***([ユーザー定義](conversions.md#user-defined-conversions)の変換) が導入されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="88a1a-1361">キーワードを`implicit`含む変換演算子の宣言では、ユーザー定義の暗黙的な変換が導入されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="88a1a-1362">暗黙の型変換は、関数メンバー呼び出し、キャスト式、代入など、さまざまな状況で発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="88a1a-1363">この詳細については、「[暗黙的な変換](conversions.md#implicit-conversions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="88a1a-1364">キーワードを`explicit`含む変換演算子の宣言では、ユーザー定義の明示的な変換が導入されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="88a1a-1365">明示的な変換は、キャスト式で発生する可能性があり、[明示的な変換](conversions.md#explicit-conversions)で詳しく説明されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="88a1a-1366">変換演算子は、変換演算子のパラメーター型で示される変換元の型を変換演算子の戻り値の型で指定されたターゲットの型に変換します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="88a1a-1367">指定されたソース`S`型およびターゲット`T`型の`S`場合`T` 、またはが null `S0`許容`T0`型の場合は、その基に`T0`なる型をおよびで参照します。それ以外`S0`の場合は、それぞれと`S` `T`が等しくなります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="88a1a-1368">クラスまたは構造体は、次のすべてに該当する場合`S`にのみ、ソース`T`型からターゲット型への変換を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="88a1a-1369">`S0`と`T0`の型は異なります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="88a1a-1370">`S0` また`T0`はは、演算子の宣言が行われるクラスまたは構造体の型です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="88a1a-1371">@No__t-0 も `T0` も*interface_type*ではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="88a1a-1372">ユーザー定義の変換を除外`S`する場合、から`T`への変換または`T`から`S`への変換は存在しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="88a1a-1373">これらの規則のために、または`S` `T`に関連付けられている型パラメーターは、他の型との継承関係がない一意の型と見なされます。これらの型パラメーターに対する制約は無視されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="88a1a-1374">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="88a1a-1375">最初の2つの演算子宣言が許可されるのは、[インデクサー](classes.md#indexers).3 `T` `int` `string`の目的で、とはそれぞれ、リレーションシップのない一意の型と見なされるためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="88a1a-1376">ただし、3番目の演算子はエラー `C<T>`です。これは、 `D<T>`がの基本クラスであるためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="88a1a-1377">2番目のルールでは、変換演算子は、演算子が宣言されているクラスまたは構造体型との間で変換を行う必要があることに従います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="88a1a-1378">たとえば、クラスまたは`C`構造体の型でからへの`C`変換とから`C` `int`への`int`変換を定義することはでき`int` `bool`ますが、からへの変換は定義できません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="88a1a-1379">定義済みの変換を直接再定義することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="88a1a-1380">したがって、と他のすべての型の間`object` `object`に暗黙的な変換と明示的な変換が既に存在するため、変換演算子はまたはからへの変換を行うことができません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="88a1a-1381">同様に、変換元と変換先の両方の型を他の型の基本型にすることはできません。これは、変換が既に存在するためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="88a1a-1382">ただし、特定の型引数に対して、事前定義された変換として既に存在する変換を指定する場合は、ジェネリック型に対して演算子を宣言することができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="88a1a-1383">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="88a1a-1384">型がの`T`型引数として指定されている場合、2番目の演算子は、既に存在する変換を宣言します (暗黙的な変換で`object`もあり、明示的には、任意の型から型への変換が存在します)。 `object`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="88a1a-1385">2つの型の間に事前定義された変換が存在する場合、これらの型間のユーザー定義の変換はすべて無視されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="88a1a-1386">具体的な内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1386">Specifically:</span></span>

*  <span data-ttu-id="88a1a-1387">定義済みの暗黙的な変換 ([暗黙](conversions.md#implicit-conversions)の変換) が`S`型から型`T`に存在する場合、からへ`T`のすべての`S`ユーザー定義変換 (暗黙的または明示的な変換) は無視されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="88a1a-1388">定義済みの明示的な変換 ([明示的な変換](conversions.md#explicit-conversions)) が`S`型から`T`型に存在する場合、からへ`S` `T`のユーザー定義の明示的な変換はすべて無視されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="88a1a-1389">さら</span><span class="sxs-lookup"><span data-stu-id="88a1a-1389">Furthermore:</span></span>

<span data-ttu-id="88a1a-1390">が`T`インターフェイス型の場合、からへ`T`の`S`ユーザー定義の暗黙的な変換は無視されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="88a1a-1391">それ以外の場合、からへ`S` `T`のユーザー定義の暗黙的な変換は引き続き考慮されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="88a1a-1392">すべての型`object`に対して、上記の`Convertible<T>`型によって宣言された演算子は、事前に定義された変換と競合しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="88a1a-1393">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="88a1a-1394">ただし、型`object`の場合、事前定義された変換は、常に1つのユーザー定義変換を非表示にします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="88a1a-1395">ユーザー定義の変換では、または*interface_type*s への変換は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="88a1a-1396">特に、この制限により、 *interface_type*への変換時にユーザー定義の変換が発生しないようにします。*また、変換*対象のオブジェクトが実際に指定された*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="88a1a-1397">変換演算子のシグネチャは、変換元の型と変換先の型で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="88a1a-1398">(これは、戻り値の型がシグネチャに参加する唯一の形式のメンバーであることに注意してください)。変換`implicit`演算子`explicit`の or 分類は、演算子のシグネチャの一部ではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="88a1a-1399">したがって、クラスまたは構造体は、 `implicit` `explicit`との変換演算子の両方を同じソースとターゲットの型で宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="88a1a-1400">一般に、ユーザー定義の暗黙の変換は、例外をスローせず、情報を失わないように設計する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="88a1a-1401">ユーザー定義の変換によって例外が発生する可能性がある場合 (たとえば、ソース引数が範囲外の場合) や情報が失われた場合 (上位ビットを破棄する場合など) は、その変換を明示的な変換として定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="88a1a-1402">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1402">In the example</span></span>
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
<span data-ttu-id="88a1a-1403">から`Digit` `Digit`へ`byte`の変換は、例外をスローしたり情報を失ったりすることはない`byte`ため、暗黙的です`Digit`が、からへの変換は、可能なのサブセットのみを表すことができるので明示的に変換されます。の`byte`値。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="88a1a-1404">インスタンスコンストラクター</span><span class="sxs-lookup"><span data-stu-id="88a1a-1404">Instance constructors</span></span>

<span data-ttu-id="88a1a-1405">"***インスタンス コンストラクター***" は、クラスのインスタンスを初期化するために必要なアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="88a1a-1406">インスタンスコンストラクターは*constructor_declaration*s を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="88a1a-1407">*Constructor_declaration*には、*属性*のセット ([属性](attributes.md))、4つのアクセス修飾子 ([アクセス修飾子](classes.md#access-modifiers)) の有効な組み合わせ、および `extern` ([外部メソッド](classes.md#external-methods)) 修飾子を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="88a1a-1408">コンストラクター宣言では、同じ修飾子を複数回含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="88a1a-1409">*Constructor_declarator*の*識別子*は、インスタンスコンストラクターが宣言されているクラスに名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="88a1a-1410">他の名前が指定されている場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="88a1a-1411">インスタンスコンストラクターの省略可能な*formal_parameter_list*には、メソッドの*formal_parameter_list*と同じ規則が適用されます ([メソッド](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="88a1a-1412">仮パラメーターリストでは、インスタンスコンストラクターの署名 ([シグネチャとオーバーロード](basic-concepts.md#signatures-and-overloading)) を定義し、オーバーロードの解決 ([型の推定](expressions.md#type-inference)) が呼び出しで特定のインスタンスコンストラクターを選択するプロセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="88a1a-1413">インスタンスコンストラクターの*formal_parameter_list*で参照される各型は、少なくともコンストラクター自体と同じようにアクセス可能である必要があります ([アクセシビリティの制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="88a1a-1414">省略可能な*constructor_initializer*は、このインスタンスコンストラクターの*constructor_body*で指定されたステートメントを実行する前に呼び出す別のインスタンスコンストラクターを指定します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="88a1a-1415">これについては、「[コンストラクター初期化子](classes.md#constructor-initializers)」で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="88a1a-1416">コンストラクターの宣言に`extern`修飾子が含まれている場合、コンストラクターは***外部コンストラクター***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="88a1a-1417">外部コンストラクターの宣言は実際の実装を提供しないため、 *constructor_body*はセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="88a1a-1418">他のすべてのコンストラクターでは、 *constructor_body*は、クラスの新しいインスタンスを初期化するステートメントを指定する*ブロック*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="88a1a-1419">これは、 `void`戻り値の型 ([メソッド本体](classes.md#method-body)) を持つインスタンスメソッドの*ブロック*に正確に対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="88a1a-1420">インスタンスコンストラクターは継承されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="88a1a-1421">したがって、クラスには、クラスで実際に宣言されているコンストラクター以外のインスタンスコンストラクターがありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="88a1a-1422">クラスにインスタンスコンストラクターの宣言が含まれていない場合は、既定のインスタンスコンストラクターが自動的に提供されます ([既定のコンストラクター](classes.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="88a1a-1423">インスタンスコンストラクターは、 *object_creation_expression*s ([オブジェクト作成式](expressions.md#object-creation-expressions)) と*constructor_initializer*s によって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="88a1a-1424">Constructor Initializers (コンストラクター初期化子)</span><span class="sxs-lookup"><span data-stu-id="88a1a-1424">Constructor initializers</span></span>

<span data-ttu-id="88a1a-1425">すべてのインスタンスコンストラクター (クラス `object` の場合を除く) には、 *constructor_body*の直前に別のインスタンスコンストラクターを呼び出すことが暗黙的に含まれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="88a1a-1426">暗黙的に呼び出すコンストラクターは、 *constructor_initializer*によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="88a1a-1427">または`base(argument_list)` `base()`の形式のインスタンスコンストラクター初期化子が呼び出されると、直接基底クラスのインスタンスコンストラクターが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="88a1a-1428">このコンストラクターは、 *argument_list*が存在する場合は、[オーバーロード解決](expressions.md#overload-resolution)のオーバーロード解決規則を使用して選択されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="88a1a-1429">候補インスタンスコンストラクターのセットは、直接基底クラスに含まれるすべてのアクセス可能なインスタンスコンストラクター、または、直接基底クラスでインスタンスコンストラクターが宣言されていない場合は既定のコンストラクター ([既定のコンストラクター](classes.md#default-constructors)) で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="88a1a-1430">このセットが空の場合、または1つの最適なインスタンスコンストラクターを識別できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="88a1a-1431">または`this(argument-list)` `this()`の形式のインスタンスコンストラクター初期化子が呼び出されると、クラス自体のインスタンスコンストラクターが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="88a1a-1432">コンストラクターは、 *argument_list*が存在する場合は、[オーバーロード解決](expressions.md#overload-resolution)のオーバーロード解決規則を使用して選択されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="88a1a-1433">候補インスタンスコンストラクターのセットは、クラス自体で宣言されたすべてのアクセス可能なインスタンスコンストラクターで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="88a1a-1434">このセットが空の場合、または1つの最適なインスタンスコンストラクターを識別できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="88a1a-1435">インスタンスコンストラクターの宣言にコンストラクター自体を呼び出すコンストラクター初期化子が含まれている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="88a1a-1436">インスタンスコンストラクターにコンストラクター初期化子がない場合は、フォーム`base()`のコンストラクター初期化子が暗黙的に指定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="88a1a-1437">そのため、フォームのインスタンスコンストラクター宣言</span><span class="sxs-lookup"><span data-stu-id="88a1a-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="88a1a-1438">はとまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="88a1a-1439">インスタンスコンストラクター宣言の*formal_parameter_list*によって指定されたパラメーターのスコープには、その宣言のコンストラクター初期化子が含まれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="88a1a-1440">したがって、コンストラクターの初期化子は、コンストラクターのパラメーターにアクセスすることが許可されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="88a1a-1441">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1441">For example:</span></span>
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

<span data-ttu-id="88a1a-1442">インスタンスコンストラクターの初期化子が、作成されているインスタンスにアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="88a1a-1443">したがって、コンストラクター初期化子の引数式で `this` を参照すると、コンパイル時にエラーになります。これは、引数式が*simple_name*を介して任意のインスタンスメンバーを参照する場合のコンパイル時エラーであるということです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="88a1a-1444">インスタンス変数初期化子</span><span class="sxs-lookup"><span data-stu-id="88a1a-1444">Instance variable initializers</span></span>

<span data-ttu-id="88a1a-1445">インスタンスコンストラクターにコンストラクター初期化子がない場合、または `base(...)` という形式のコンストラクター初期化子がある場合、そのコンストラクターは、で宣言されたインスタンスフィールドの*variable_initializer*s によって指定された初期化を暗黙的に実行します。クラス。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="88a1a-1446">これは、コンストラクターへのエントリと、直接基底クラスのコンストラクターの暗黙的な呼び出しの直前に実行される割り当てのシーケンスに対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="88a1a-1447">変数初期化子は、クラス宣言に含まれるテキスト順に実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="88a1a-1448">コンストラクターの実行</span><span class="sxs-lookup"><span data-stu-id="88a1a-1448">Constructor execution</span></span>

<span data-ttu-id="88a1a-1449">変数初期化子は代入ステートメントに変換され、これらの代入ステートメントは、基底クラスのインスタンスコンストラクターが呼び出される前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="88a1a-1450">この順序により、インスタンスにアクセスできるステートメントが実行される前に、すべてのインスタンスフィールドが変数初期化子によって初期化されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="88a1a-1451">例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1451">Given the example</span></span>
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
<span data-ttu-id="88a1a-1452">を`new B()`使用しての`B`インスタンスを作成すると、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```console
x = 1, y = 0
```

<span data-ttu-id="88a1a-1453">の`x`値は1です。これは、基底クラスのインスタンスコンストラクターが呼び出される前に変数初期化子が実行されるためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="88a1a-1454">ただし、の`y`値は 0 ( `int`の既定値) です。これは、へ`y`の代入が、基底クラスのコンストラクターが返されるまで実行されないためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="88a1a-1455">インスタンス変数初期化子とコンストラクター初期化子は、 *constructor_body*の前に自動的に挿入されるステートメントと考えると便利です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="88a1a-1456">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-1456">The example</span></span>
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
<span data-ttu-id="88a1a-1457">複数の変数初期化子が含まれています。また、両方の形式 (`base`と`this`) のコンストラクター初期化子も含まれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="88a1a-1458">この例は、以下に示すコードに対応しています。各コメントは、自動的に挿入されたステートメントを示します (自動的に挿入されたコンストラクターの呼び出しに使用される構文は有効ではありませんが、単に機構を示すためにのみ機能します)。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a><span data-ttu-id="88a1a-1459">既定のコンストラクター</span><span class="sxs-lookup"><span data-stu-id="88a1a-1459">Default constructors</span></span>

<span data-ttu-id="88a1a-1460">クラスにインスタンスコンストラクターの宣言が含まれていない場合は、既定のインスタンスコンストラクターが自動的に指定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="88a1a-1461">この既定のコンストラクターは、単に直接基底クラスのパラメーターなしのコンストラクターを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="88a1a-1462">クラスが abstract の場合、既定のコンストラクターに対して宣言されたアクセシビリティは保護されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="88a1a-1463">それ以外の場合、既定のコンストラクターに対して宣言されたアクセシビリティは public になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="88a1a-1464">したがって、既定のコンストラクターは常にフォームになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="88a1a-1465">または</span><span class="sxs-lookup"><span data-stu-id="88a1a-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="88a1a-1466">ここ`C`で、はクラスの名前です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="88a1a-1467">オーバーロードの解決で基底クラスのコンストラクターの初期化子に最適な一意の候補を特定できない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="88a1a-1468">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="88a1a-1469">クラスにはインスタンスコンストラクターの宣言が含まれていないため、既定のコンストラクターが用意されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="88a1a-1470">したがって、この例はとまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="88a1a-1471">プライベートコンストラクター</span><span class="sxs-lookup"><span data-stu-id="88a1a-1471">Private constructors</span></span>

<span data-ttu-id="88a1a-1472">クラス`T`がプライベートインスタンスコンストラクターのみを宣言する場合、の`T` `T`プログラムテキストの外部のクラスは、またはの`T`インスタンスを直接作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="88a1a-1473">したがって、クラスに静的メンバーのみが含まれ、インスタンス化されない場合は、空のプライベートインスタンスコンストラクターを追加すると、インスタンス化ができなくなります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="88a1a-1474">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1474">For example:</span></span>
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

<span data-ttu-id="88a1a-1475">クラス`Trig`は、関連するメソッドと定数をグループ化しますが、インスタンス化するためのものではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="88a1a-1476">したがって、1つの空のプライベートインスタンスコンストラクターを宣言します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="88a1a-1477">既定のコンストラクターが自動生成されないようにするには、少なくとも1つのインスタンスコンストラクターを宣言する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="88a1a-1478">省略可能なインスタンスコンストラクターパラメーター</span><span class="sxs-lookup"><span data-stu-id="88a1a-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="88a1a-1479">コンストラクター `this(...)`初期化子の形式は、一般に、省略可能なインスタンスコンストラクターパラメーターを実装するためにオーバーロードと組み合わせて使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="88a1a-1480">この例では、</span><span class="sxs-lookup"><span data-stu-id="88a1a-1480">In the example</span></span>
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
<span data-ttu-id="88a1a-1481">最初の2つのインスタンスコンストラクターは、不足している引数の既定値を提供するだけです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="88a1a-1482">どちらも、 `this(...)`コンストラクター初期化子を使用して3番目のインスタンスコンストラクターを呼び出します。このコンストラクターは、実際に新しいインスタンスを初期化する処理を行います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="88a1a-1483">これは、省略可能なコンストラクターパラメーターの効果です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="88a1a-1484">静的コンストラクター</span><span class="sxs-lookup"><span data-stu-id="88a1a-1484">Static constructors</span></span>

<span data-ttu-id="88a1a-1485">***静的コンストラクター***は、closed クラス型を初期化するために必要なアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="88a1a-1486">静的コンストラクターは*static_constructor_declaration*s を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="88a1a-1487">*Static_constructor_declaration*には、一連の*属性*([属性](attributes.md)) と `extern` 修飾子 ([外部メソッド](classes.md#external-methods)) を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="88a1a-1488">*Static_constructor_declaration*の*識別子*は、静的コンストラクターが宣言されているクラスに名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="88a1a-1489">他の名前が指定されている場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="88a1a-1490">静的コンストラクターの宣言に`extern`修飾子が含まれている場合、静的コンストラクターは外部の***静的コンストラクター***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="88a1a-1491">外部の静的コンストラクター宣言は実際の実装を提供しないため、 *static_constructor_body*はセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="88a1a-1492">その他のすべての静的コンストラクター宣言では、 *static_constructor_body*は、クラスを初期化するために実行するステートメントを指定する*ブロック*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="88a1a-1493">これは、@no__t 1 つの戻り値の型 ([メソッド本体](classes.md#method-body)) を持つ静的メソッドの*method_body*に相当します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="88a1a-1494">静的コンストラクターは継承されず、直接呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="88a1a-1495">Closed クラス型の静的コンストラクターは、特定のアプリケーションドメインで1回だけ実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="88a1a-1496">静的コンストラクターの実行は、アプリケーションドメイン内で発生する次のイベントの最初のイベントによってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="88a1a-1497">クラス型のインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="88a1a-1498">クラス型のいずれかの静的メンバーが参照されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="88a1a-1499">クラスに、実行を`Main`開始するメソッド ([アプリケーションのスタートアップ](basic-concepts.md#application-startup)) が含まれている場合、 `Main`メソッドが呼び出される前に、そのクラスの静的コンストラクターが実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="88a1a-1500">クローズしたクラスの新しい型を初期化するには、まず、その特定のクローズされた型の新しい静的フィールド ([静的フィールドとインスタンスフィールド](classes.md#static-and-instance-fields)) のセットを作成します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="88a1a-1501">各静的フィールドは、既定値 ([既定値](variables.md#default-values)) に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="88a1a-1502">次に、静的フィールドの初期化子 (静的フィールドの[初期化](classes.md#static-field-initialization)) を静的フィールドに対して実行します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="88a1a-1503">最後に、静的コンストラクターが実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="88a1a-1504">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-1504">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
<span data-ttu-id="88a1a-1505">出力を生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1505">must produce the output:</span></span>
```console
Init A
A.F
Init B
B.F
```
<span data-ttu-id="88a1a-1506">の`A` `B`呼び出しによっての静的コンストラクターの実行がトリガーされるため、の呼び出しによっての`B.F`静的コンストラクターの実行がトリガーされます。 `A.F`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="88a1a-1507">変数初期化子を持つ静的フィールドを既定値の状態で観察できるようにする、循環依存関係を構築することができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="88a1a-1508">例</span><span class="sxs-lookup"><span data-stu-id="88a1a-1508">The example</span></span>
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
<span data-ttu-id="88a1a-1509">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1509">produces the output</span></span>
```console
X = 1, Y = 2
```

<span data-ttu-id="88a1a-1510">`Main`メソッドを実行するために、システムはまず、クラス`B.Y` `B`の静的コンストラクターの前に、の初期化子を実行します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="88a1a-1511">`Y``A`の`A.X`値が参照されているため、の初期化子によって静的コンストラクターが実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="88a1a-1512">さらに、の `A`静的コンストラクターは、の `X`値の計算を続行します。これにより、の `Y`既定値 (0) がフェッチされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="88a1a-1513">`A.X`この場合、は1に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="88a1a-1514">次に、の`A`静的フィールド初期化子と静的コンストラクターを実行するプロセスが完了し、の `Y`初期値の計算に戻ります。結果は2になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="88a1a-1515">静的コンストラクターは、終了した構築済みのクラス型ごとに1回だけ実行されるため、制約 ([型パラメーター制約](classes.md#type-parameter-constraints)) を使用してコンパイル時にチェックできない型パラメーターに対してランタイムチェックを強制するための便利な場所です.</span><span class="sxs-lookup"><span data-stu-id="88a1a-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="88a1a-1516">たとえば、次の型では、静的コンストラクターを使用して、型引数が列挙型であることを強制しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a><span data-ttu-id="88a1a-1517">デストラクター</span><span class="sxs-lookup"><span data-stu-id="88a1a-1517">Destructors</span></span>

<span data-ttu-id="88a1a-1518">***デストラクター***は、クラスのインスタンスを消滅させるために必要なアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="88a1a-1519">デストラクターは*destructor_declaration*を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1519">A destructor is declared using a *destructor_declaration*:</span></span>

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="88a1a-1520">*Destructor_declaration*には、一連の*属性*([属性](attributes.md)) を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="88a1a-1521">*Destructor_declaration*の*識別子*は、デストラクターが宣言されているクラスに名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="88a1a-1522">他の名前が指定されている場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="88a1a-1523">デストラクター宣言に`extern`修飾子が含まれている場合、デストラクターは***外部デストラクター***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="88a1a-1524">外部デストラクター宣言は実際の実装を提供しないため、 *destructor_body*はセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="88a1a-1525">その他のすべてのデストラクターでは、 *destructor_body*は、クラスのインスタンスを破棄するために実行するステートメントを指定する*ブロック*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="88a1a-1526">*Destructor_body*は、@no__t 2 つの戻り値の型 ([メソッド本体](classes.md#method-body)) を持つインスタンスメソッドの*method_body*に正確に対応します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="88a1a-1527">デストラクターは継承されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1527">Destructors are not inherited.</span></span> <span data-ttu-id="88a1a-1528">したがって、クラスには、そのクラスで宣言されている可能性のあるデストラクターがありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="88a1a-1529">デストラクターにはパラメーターが必要ないため、オーバーロードすることはできないため、クラスは最大で1つのデストラクターを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="88a1a-1530">デストラクターは自動的に呼び出されるため、明示的に呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="88a1a-1531">インスタンスをコードで使用することができなくなった場合、インスタンスは破棄の対象になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="88a1a-1532">インスタンスのデストラクターの実行は、インスタンスが破棄の対象になった後、いつでも発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="88a1a-1533">インスタンスが破棄されされると、そのインスタンスの継承チェーン内のデストラクターは、ほとんどの派生から最小の派生まで、順に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="88a1a-1534">デストラクターは、任意のスレッドで実行できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="88a1a-1535">デストラクターを実行するタイミングと方法を制御する規則の詳細については、「[自動メモリ管理](basic-concepts.md#automatic-memory-management)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="88a1a-1536">例の出力</span><span class="sxs-lookup"><span data-stu-id="88a1a-1536">The output of the example</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
<span data-ttu-id="88a1a-1537">is</span><span class="sxs-lookup"><span data-stu-id="88a1a-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="88a1a-1538">継承チェーン内のデストラクターは、ほとんどの派生から最小の派生から順に呼び出されるためです。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="88a1a-1539">デストラクターは、の`Finalize` `System.Object`仮想メソッドをオーバーライドすることによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="88a1a-1540">C#プログラムでは、このメソッドのオーバーライドまたは呼び出し (またはそのオーバーライド) を直接行うことはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="88a1a-1541">たとえば、プログラム</span><span class="sxs-lookup"><span data-stu-id="88a1a-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="88a1a-1542">2つのエラーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1542">contains two errors.</span></span>

<span data-ttu-id="88a1a-1543">コンパイラは、このメソッドとオーバーライドがまったく存在しないかのように動作します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="88a1a-1544">そのため、このプログラムは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="88a1a-1545">は有効で、表示されるメソッド`System.Object`は`Finalize` 、のメソッドを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="88a1a-1546">デストラクターから例外がスローされた場合の動作の詳細については、「[例外の処理方法](exceptions.md#how-exceptions-are-handled)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="88a1a-1547">Iterators</span><span class="sxs-lookup"><span data-stu-id="88a1a-1547">Iterators</span></span>

<span data-ttu-id="88a1a-1548">反復子ブロック ([ブロック](statements.md#blocks)) を使用して実装される関数メンバー ([関数メンバー](expressions.md#function-members)) は、***反復子***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="88a1a-1549">対応する関数メンバーの戻り値の型が列挙子インターフェイス ([列挙子](classes.md#enumerator-interfaces)インターフェイス) または列挙可能なインターフェイス (列挙可能な[インターフェイス](classes.md#enumerable-interfaces)) の1つである限り、反復子ブロックは関数メンバーの本体として使用できます.</span><span class="sxs-lookup"><span data-stu-id="88a1a-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="88a1a-1550">これは、 *method_body*、 *operator_body* 、または*accessor_body*として発生する可能性があります。一方、イベント、インスタンスコンストラクター、静的コンストラクター、およびデストラクターを反復子として実装することはできません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="88a1a-1551">関数メンバーが反復子ブロックを使用して実装されている場合、関数メンバーの仮パラメーターリストで`ref`または`out`パラメーターを指定すると、コンパイル時にエラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="88a1a-1552">列挙子インターフェイス</span><span class="sxs-lookup"><span data-stu-id="88a1a-1552">Enumerator interfaces</span></span>

<span data-ttu-id="88a1a-1553">***列挙子インターフェイス***は、非ジェネリックインターフェイス`System.Collections.IEnumerator`と、ジェネリックインターフェイス`System.Collections.Generic.IEnumerator<T>`のすべてのインスタンス化です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="88a1a-1554">簡潔にするために、この章では、これらのインターフェイス`IEnumerator`を`IEnumerator<T>`それぞれおよびとして参照しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="88a1a-1555">列挙可能なインターフェイス</span><span class="sxs-lookup"><span data-stu-id="88a1a-1555">Enumerable interfaces</span></span>

<span data-ttu-id="88a1a-1556">***列挙***可能なインターフェイスは、非ジェネリックインターフェイス`System.Collections.IEnumerable`と、ジェネリックインターフェイス`System.Collections.Generic.IEnumerable<T>`のすべてのインスタンス化です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="88a1a-1557">簡潔にするために、この章では、これらのインターフェイス`IEnumerable`を`IEnumerable<T>`それぞれおよびとして参照しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="88a1a-1558">Yield 型</span><span class="sxs-lookup"><span data-stu-id="88a1a-1558">Yield type</span></span>

<span data-ttu-id="88a1a-1559">反復子は、すべて同じ型の値のシーケンスを生成します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="88a1a-1560">この型は、反復子の***yield 型***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="88a1a-1561">`IEnumerator` また`IEnumerable`はを返す反復子の yield 型。 `object`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="88a1a-1562">`IEnumerator<T>` また`IEnumerable<T>`はを返す反復子の yield 型。 `T`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="88a1a-1563">列挙子オブジェクト</span><span class="sxs-lookup"><span data-stu-id="88a1a-1563">Enumerator objects</span></span>

<span data-ttu-id="88a1a-1564">列挙子インターフェイス型を返す関数メンバーが反復子ブロックを使用して実装されている場合、関数メンバーを呼び出すと反復子ブロックのコードはすぐには実行されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="88a1a-1565">代わりに、***列挙子オブジェクト***が作成され、返されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="88a1a-1566">このオブジェクトは、反復子ブロックで指定されたコードをカプセル化し、列挙子オブジェクトの`MoveNext`メソッドが呼び出されたときに反復子ブロック内のコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="88a1a-1567">列挙子オブジェクトには、次の特性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="88a1a-1568">`T`と`IEnumerator` を実装`IEnumerator<T>`します。ここで、は反復子の yield 型です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="88a1a-1569">このクラスは、`System.IDisposable` を実装します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="88a1a-1570">引数値 (存在する場合) と、関数メンバーに渡されるインスタンス値のコピーを使用して初期化されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="88a1a-1571">これには、"***前***"、"***実行中***"、"***中断***"、および "***後***" の4つの状態があり、最初は "***前***" の状態になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="88a1a-1572">列挙子オブジェクトは、通常、コンパイラによって生成される列挙子クラスのインスタンスであり、反復子ブロックのコードをカプセル化し、列挙子インターフェイスを実装しますが、その他の実装方法も可能です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="88a1a-1573">列挙子クラスがコンパイラによって生成される場合、そのクラスは、関数メンバーを含むクラスで直接的または間接的に入れ子にされ、プライベートアクセシビリティを持ち、コンパイラで使用するために予約された名前 ([識別子](lexical-structure.md#identifiers)) になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="88a1a-1574">列挙子オブジェクトには、上で指定したものよりも多くのインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="88a1a-1575">次のセクションでは、列挙子オブジェクト`MoveNext`に`Current`よって`Dispose`提供される`IEnumerable`および`IEnumerable<T>`インターフェイス実装の、、の各メンバーの正確な動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="88a1a-1576">列挙子オブジェクトはメソッドを`IEnumerator.Reset`サポートしていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="88a1a-1577">このメソッドを呼び出す`System.NotSupportedException`と、がスローされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="88a1a-1578">MoveNext メソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-1578">The MoveNext method</span></span>

<span data-ttu-id="88a1a-1579">列挙`MoveNext`子オブジェクトのメソッドは、反復子ブロックのコードをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="88a1a-1580">メソッドを`MoveNext`呼び出すと、反復子ブロック内のコード`Current`が実行され、必要に応じて列挙子オブジェクトのプロパティが設定されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="88a1a-1581">によって`MoveNext`実行される正確なアクションは、が呼び出され`MoveNext`たときの列挙子オブジェクトの状態によって異なります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="88a1a-1582">列挙子オブジェクトの状態が***以前***である場合は`MoveNext`、次を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="88a1a-1583">状態を***実行中***に変更します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="88a1a-1584">反復子オブジェクトが初期化`this`されたときに保存された引数値とインスタンス値に対して、反復子ブロックのパラメーター (を含む) を初期化します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="88a1a-1585">次に示すように、実行が中断されるまで、最初から反復子ブロックを実行します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="88a1a-1586">列挙子オブジェクトの状態が [***実行中***] の場合、呼び出し`MoveNext`の結果は指定されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="88a1a-1587">列挙子オブジェクトの状態が***中断***されている`MoveNext`場合は、次を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="88a1a-1588">状態を***実行中***に変更します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="88a1a-1589">すべてのローカル変数とパラメーター (this を含む) の値を、反復子ブロックの実行が最後に中断されたときに保存された値に復元します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="88a1a-1590">これらの変数によって参照されるオブジェクトの内容は、MoveNext の前の呼び出し以降に変更されている可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="88a1a-1591">実行の中断の原因となったステートメント`yield return`の直後に反復子ブロックの実行を再開し、次に説明するように実行が中断されるまで続行します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="88a1a-1592">列挙子オブジェクトの状態がの***後***にある場合`MoveNext` 、 `false`を呼び出すと、が返されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="88a1a-1593">が`MoveNext`反復子ブロックを実行すると、次の4つの方法で実行が中断される可能性があります。ステートメントでは、ステートメントによって、反復子ブロックの終わりを検出し、例外がスローされ、反復子ブロックの外に伝達されます。 `yield break` `yield return`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="88a1a-1594">ステートメントが検出された場合 ([yield ステートメント](statements.md#the-yield-statement)): `yield return`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="88a1a-1595">ステートメントで指定された式が評価され、暗黙的に yield 型に変換され`Current` 、列挙子オブジェクトのプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="88a1a-1596">反復子本体の実行は中断されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="88a1a-1597">`this`この`yield return`ステートメントの場所と同様に、すべてのローカル変数とパラメーター (を含む) の値が保存されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="88a1a-1598">ステートメントが1つ`try`以上のブロック内にある場合、 `finally`関連付けられたブロックは現時点では実行されません。 `yield return`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="88a1a-1599">列挙子オブジェクトの状態が "***中断***" に変更されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="88a1a-1600">メソッド`MoveNext`は、 `true`呼び出し元に戻り、反復処理が次の値に正常に進んだことを示します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="88a1a-1601">ステートメントが検出された場合 ([yield ステートメント](statements.md#the-yield-statement)): `yield break`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="88a1a-1602">ステートメントが1つ`try`以上のブロック内にある場合は`finally` 、関連付けられているブロックが実行されます。 `yield break`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="88a1a-1603">列挙子オブジェクトの状態が***after***に変更されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="88a1a-1604">メソッド`MoveNext`は、 `false`反復処理が完了したことを示す呼び出し元に戻ります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="88a1a-1605">反復子本体の終わりが見つかった場合:</span><span class="sxs-lookup"><span data-stu-id="88a1a-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="88a1a-1606">列挙子オブジェクトの状態が***after***に変更されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="88a1a-1607">メソッド`MoveNext`は、 `false`反復処理が完了したことを示す呼び出し元に戻ります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="88a1a-1608">例外がスローされ、反復子ブロックの外に伝達される場合:</span><span class="sxs-lookup"><span data-stu-id="88a1a-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="88a1a-1609">反復`finally`子本体内の適切なブロックは、例外の反映によって実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="88a1a-1610">列挙子オブジェクトの状態が***after***に変更されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="88a1a-1611">例外の伝達は、 `MoveNext`メソッドの呼び出し元に続きます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="88a1a-1612">現在のプロパティ</span><span class="sxs-lookup"><span data-stu-id="88a1a-1612">The Current property</span></span>

<span data-ttu-id="88a1a-1613">列挙子オブジェクトの`Current`プロパティは、反復`yield return`子ブロック内のステートメントの影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="88a1a-1614">列挙子オブジェクトが***中断***状態にある場合、の値は`Current` 、の前`MoveNext`の呼び出しで設定された値になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="88a1a-1615">列挙子オブジェクトが [***前***]、[***実行中***]、または [***後***] の`Current`状態にある場合、へのアクセスの結果は指定されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="88a1a-1616">以外`object`の yield 型を持つ反復子の場合、列挙子オブジェクト`Current`の`IEnumerable`実装を通じてにアクセスする`Current`と、その結果、列挙`IEnumerator<T>`子オブジェクトのを実装し、結果を`object`にキャストします。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="88a1a-1617">Dispose メソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-1617">The Dispose method</span></span>

<span data-ttu-id="88a1a-1618">メソッドは、列挙子オブジェクトを after 状態にすることによって、反復処理をクリーンアップするために使用されます。 `Dispose`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="88a1a-1619">列挙子オブジェクトの状態が***以前***である場合、 `Dispose`を呼び出すと状態がに変更さ***れます。***</span><span class="sxs-lookup"><span data-stu-id="88a1a-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="88a1a-1620">列挙子オブジェクトの状態が [***実行中***] の場合、呼び出し`Dispose`の結果は指定されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="88a1a-1621">列挙子オブジェクトの状態が***中断***されている`Dispose`場合は、次を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="88a1a-1622">状態を***実行中***に変更します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="88a1a-1623">最後に実行`yield return`されたステートメントがステートメントで`yield break`あったかのように、finally ブロックを実行します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="88a1a-1624">これによって例外がスローされ、反復子本体の外に伝達される場合、列挙子オブジェクトの状態は***after***に設定され、例外は`Dispose`メソッドの呼び出し元に反映されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="88a1a-1625">状態をの***後***に変更します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="88a1a-1626">列挙子オブジェクトの状態がの***後***である場合`Dispose` 、を呼び出すことによる影響はありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="88a1a-1627">列挙可能なオブジェクト</span><span class="sxs-lookup"><span data-stu-id="88a1a-1627">Enumerable objects</span></span>

<span data-ttu-id="88a1a-1628">列挙可能なインターフェイス型を返す関数メンバーが反復子ブロックを使用して実装されている場合、関数メンバーを呼び出すと、反復子ブロックのコードはすぐには実行されません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="88a1a-1629">代わりに、列挙可能な***オブジェクト***が作成されて返されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="88a1a-1630">列挙可能なオブジェクト`GetEnumerator`のメソッドは、反復子ブロックで指定されたコードをカプセル化する列挙子オブジェクトを返します。反復子オブジェクトの`MoveNext`メソッドが呼び出されると、反復子ブロック内のコードが実行されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="88a1a-1631">列挙可能なオブジェクトには、次の特性があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="88a1a-1632">`T`と`IEnumerable` を実装`IEnumerable<T>`します。ここで、は反復子の yield 型です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="88a1a-1633">引数値 (存在する場合) と、関数メンバーに渡されるインスタンス値のコピーを使用して初期化されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="88a1a-1634">列挙可能なオブジェクトは、通常、コンパイラによって生成される列挙可能なクラスのインスタンスであり、反復子ブロックのコードをカプセル化し、列挙可能なインターフェイスを実装しますが、その他の実装方法も可能です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="88a1a-1635">列挙可能なクラスがコンパイラによって生成される場合、そのクラスは、関数メンバーを含むクラスで直接的または間接的に入れ子にされ、プライベートアクセシビリティを持ち、コンパイラで使用するために予約された名前 ([識別子](lexical-structure.md#identifiers)) になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="88a1a-1636">列挙可能なオブジェクトは、上で指定したものよりも多くのインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="88a1a-1637">特に、列挙可能なオブジェクトはと`IEnumerator` `IEnumerator<T>`を実装し、列挙可能なオブジェクトと列挙子の両方として機能できるようにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="88a1a-1638">この種類の実装では、列挙可能なオブジェクトの`GetEnumerator`メソッドが初めて呼び出されるときに、列挙可能なオブジェクト自体が返されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="88a1a-1639">列挙可能なオブジェクト`GetEnumerator`の後続の呼び出し (存在する場合) は、列挙可能なオブジェクトのコピーを返します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="88a1a-1640">したがって、返される各列挙子には独自の状態があり、1つの列挙子の変更は別の列挙子に影響しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="88a1a-1641">GetEnumerator メソッド</span><span class="sxs-lookup"><span data-stu-id="88a1a-1641">The GetEnumerator method</span></span>

<span data-ttu-id="88a1a-1642">列挙可能なオブジェクトは、インターフェイス`GetEnumerator` `IEnumerable`と`IEnumerable<T>`インターフェイスのメソッドの実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="88a1a-1643">2つ`GetEnumerator`のメソッドは、使用可能な列挙子オブジェクトを取得して返す共通の実装を共有します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="88a1a-1644">列挙子オブジェクトは、列挙可能なオブジェクトが初期化されたときに保存された引数値とインスタンス値を使用して初期化されますが、それ以外の場合は、列挙子オブジェクトは「 [enumerator オブジェクト](classes.md#enumerator-objects)」の説明に従って機能します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="88a1a-1645">実装の例</span><span class="sxs-lookup"><span data-stu-id="88a1a-1645">Implementation example</span></span>

<span data-ttu-id="88a1a-1646">このセクションでは、標準的C#なコンストラクトに関して、反復子の実装について説明します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="88a1a-1647">ここで説明する実装は、Microsoft C#コンパイラで使用されるのと同じ原則に基づいていますが、必須の実装であるか、または可能な唯一の方法ではありません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="88a1a-1648">次`Stack<T>`のクラスは、 `GetEnumerator`反復子を使用してメソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="88a1a-1649">反復子は、スタックの要素を上から下の順に列挙します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

<span data-ttu-id="88a1a-1650">メソッド`GetEnumerator`は、次に示すように、反復子ブロックのコードをカプセル化する、コンパイラによって生成された列挙子クラスのインスタンス化に変換できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="88a1a-1651">前の変換では、反復子ブロック内のコードはステートマシンに変換され、列挙`MoveNext`子クラスのメソッドに配置されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="88a1a-1652">さらに、ローカル変数`i`は列挙子オブジェクトのフィールドに変換されるので、の呼び出しの`MoveNext`間に引き続き存在することができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="88a1a-1653">次の例では、1 ~ 10 の整数の単純な乗算テーブルを出力します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="88a1a-1654">この`FromTo`例のメソッドは、列挙可能なオブジェクトを返し、反復子を使用して実装されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="88a1a-1655">メソッド`FromTo`は、次に示すように、反復子ブロックのコードをカプセル化する、コンパイラによって生成される列挙可能なクラスのインスタンス化に変換できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="88a1a-1656">列挙可能なクラスは、列挙可能なインターフェイスと列挙子インターフェイスの両方を実装します。これにより、列挙可能なと列挙子の両方として機能できるようになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="88a1a-1657">`GetEnumerator`メソッドが初めて呼び出されたときに、列挙可能なオブジェクト自体が返されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="88a1a-1658">列挙可能なオブジェクト`GetEnumerator`の後続の呼び出し (存在する場合) は、列挙可能なオブジェクトのコピーを返します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="88a1a-1659">したがって、返される各列挙子には独自の状態があり、1つの列挙子の変更は別の列挙子に影響しません。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="88a1a-1660">メソッド`Interlocked.CompareExchange`は、スレッドセーフな操作を確実に行うために使用されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="88a1a-1661">`from` および`to`パラメーターは、列挙可能なクラスのフィールドに変換されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="88a1a-1662">は`from`反復子ブロックで変更されるため、 `__from`各列挙子のに`from`指定された初期値を保持するために追加のフィールドが導入されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="88a1a-1663">がの`InvalidOperationException`ときに `MoveNext` `__state`呼び出された場合、メソッドはをスローします。`0`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="88a1a-1664">これにより、最初にを呼び出す`GetEnumerator`ことなく、列挙可能なオブジェクトを列挙子オブジェクトとして使用することが防止されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="88a1a-1665">次の例は、単純なツリークラスを示しています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="88a1a-1666">クラス`Tree<T>`は、反復`GetEnumerator`子を使用してメソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="88a1a-1667">反復子は、ツリーの要素を挿入順序で列挙します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="88a1a-1668">メソッド`GetEnumerator`は、次に示すように、反復子ブロックのコードをカプセル化する、コンパイラによって生成された列挙子クラスのインスタンス化に変換できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="88a1a-1669">`foreach`ステートメントで使用されるコンパイラによって生成され`__left`た`__right`一時要素は、列挙子オブジェクトのフィールドとフィールドにリフトされます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="88a1a-1670">列挙`__state`子オブジェクトのフィールドは、例外がスローされた`Dispose()`場合に適切なメソッドが正しく呼び出されるように、慎重に更新されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="88a1a-1671">単純な`foreach`ステートメントでは、翻訳されたコードを記述できないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="88a1a-1672">非同期関数</span><span class="sxs-lookup"><span data-stu-id="88a1a-1672">Async functions</span></span>

<span data-ttu-id="88a1a-1673">修飾子を持つメソッド ([メソッド](classes.md#methods)) または匿名関数 ([匿名関数式](expressions.md#anonymous-function-expressions)) は、非同期関数と呼ばれます。 `async`</span><span class="sxs-lookup"><span data-stu-id="88a1a-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="88a1a-1674">一般に、 `async`修飾子を持つ任意の種類の関数を記述するには、 ***async***という用語を使用します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="88a1a-1675">`ref`または`out`パラメーターを指定する非同期関数の仮パラメーターリストでは、コンパイル時にエラーになります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="88a1a-1676">非同期*メソッドの種類は、@no__t* -1 または***タスクの種類***のいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="88a1a-1677">タスクの種類は`System.Threading.Tasks.Task` 、から`System.Threading.Tasks.Task<T>`構築された型です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="88a1a-1678">簡潔にするために、この章では、これらの型`Task`は`Task<T>`それぞれおよびとして参照されています。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="88a1a-1679">タスクの種類を返す非同期メソッドは、タスクを返すと言います。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="88a1a-1680">タスクの種類の正確な定義は実装で定義されていますが、言語の観点から見ると、タスクの種類は [未完了]、[成功]、または [失敗] のいずれかの状態になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="88a1a-1681">エラーが発生したタスクは、関連する例外を記録します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="88a1a-1682">Succeeded `Task<T>`は、型`T`の結果を記録します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="88a1a-1683">タスクの種類は待機可能であるため、await 式 ([await 式](expressions.md#await-expressions)) のオペランドにすることができます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="88a1a-1684">非同期関数呼び出しでは、その本体で await 式 ([await 式](expressions.md#await-expressions)) を使って評価を中断できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="88a1a-1685">評価は、***再開デリゲート***によって、中断 await 式の時点で後で再開される場合があります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="88a1a-1686">再開デリゲートは型`System.Action`であり、呼び出されると、非同期関数呼び出しの評価は、中断した await 式から再開されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="88a1a-1687">非同期関数呼び出しの***現在の呼び出し***元は、関数呼び出しが中断されていない場合は元の呼び出し元、それ以外の場合は再開デリゲートの最新の呼び出し元です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="88a1a-1688">タスクを返す非同期関数の評価</span><span class="sxs-lookup"><span data-stu-id="88a1a-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="88a1a-1689">タスクを返す非同期関数を呼び出すと、返されたタスクの型のインスタンスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="88a1a-1690">これは、async 関数の***return タスク***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="88a1a-1691">タスクは、最初は不完全な状態です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="88a1a-1692">非同期関数本体は、(await 式に到達することによって) 中断されるか終了するまで評価されます。また、制御が戻りタスクと共に呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="88a1a-1693">非同期関数の本体が終了すると、返されるタスクは不完全な状態から移動します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="88a1a-1694">関数本体が return ステートメントまたは本体の末尾に到達した結果として終了した場合、結果の値は、succeeded タスクに記録されます。これは succeeded 状態になります。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="88a1a-1695">キャッチされていない例外 ([throw ステートメント](statements.md#the-throw-statement)) の結果として関数本体が終了した場合、例外はエラー状態になるリターンタスクに記録されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="88a1a-1696">Void を返す非同期関数の評価</span><span class="sxs-lookup"><span data-stu-id="88a1a-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="88a1a-1697">非同期関数の戻り値の型が`void`の場合、次のように評価が上記と異なります。タスクが返されないため、関数は、現在のスレッドの***同期コンテキスト***に対して、完了と例外を通信します。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="88a1a-1698">同期コンテキストの正確な定義は実装に依存しますが、現在のスレッドが実行されている "where" の表現です。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="88a1a-1699">Void を返す非同期関数の評価が開始されるか、正常に完了するか、キャッチされていない例外がスローされると、同期コンテキストに通知されます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="88a1a-1700">これにより、コンテキストは、その下で実行されている void を返す非同期関数の数を追跡し、その中で例外を伝達する方法を決定できます。</span><span class="sxs-lookup"><span data-stu-id="88a1a-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
