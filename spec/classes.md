---
ms.openlocfilehash: af7af574814dc04ee3ece0396b7ae5f86b3ec8eb
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229782"
---
# <a name="classes"></a><span data-ttu-id="be588-101">クラス</span><span class="sxs-lookup"><span data-stu-id="be588-101">Classes</span></span>

<span data-ttu-id="be588-102">クラスは、データ メンバー (定数とフィールド)、関数メンバー (メソッド、プロパティ、イベント、インデクサー、演算子、インスタンス コンス トラクター、デストラクター、静的コンス トラクター)、および入れ子にされた型を含む可能性のあるデータ構造です。</span><span class="sxs-lookup"><span data-stu-id="be588-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="be588-103">クラス型では、継承、派生クラスが基底クラスを拡張および限定するためのメカニズムをサポートします。</span><span class="sxs-lookup"><span data-stu-id="be588-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="be588-104">クラス宣言</span><span class="sxs-lookup"><span data-stu-id="be588-104">Class declarations</span></span>

<span data-ttu-id="be588-105">A *class_declaration*は、 *type_declaration* ([入力宣言](namespaces.md#type-declarations)) 新しいクラスを宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="be588-106">A *class_declaration*オプションのセットから成る*属性*([属性](attributes.md)) オプションのセットと、その後*class_modifier*s ([修飾子をクラス](classes.md#class-modifiers)) と省略可能なその後`partial`の後に、キーワード修飾子`class`と*識別子*続く、クラスの名前を示す、省略可能な*type_parameter_list* ([パラメーター入力](classes.md#type-parameters)) と省略可能なその後*class_base*仕様 ([クラスの基本仕様](classes.md#class-base-specification)) オプションのセットと、その後*type_parameter_constraints_clause*s ([パラメーターの制約入力](classes.md#type-parameter-constraints))、その後に、 *class_body* ([クラス本体](classes.md#class-body))、セミコロンで必要に応じてその後にします。</span><span class="sxs-lookup"><span data-stu-id="be588-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="be588-107">クラス宣言を提供できません*type_parameter_constraints_clause*s も用意されていない限り、 *type_parameter_list*します。</span><span class="sxs-lookup"><span data-stu-id="be588-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="be588-108">クラスの宣言を提供する、 *type_parameter_list*は、***ジェネリック クラス宣言***します。</span><span class="sxs-lookup"><span data-stu-id="be588-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="be588-109">さらに、ジェネリック クラス宣言またはジェネリック構造体宣言内で入れ子になったクラス自体がジェネリック クラス宣言では、含んでいる型の型パラメーターを指定して構築された型を作成する必要がありますので。</span><span class="sxs-lookup"><span data-stu-id="be588-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="be588-110">クラスの修飾子</span><span class="sxs-lookup"><span data-stu-id="be588-110">Class modifiers</span></span>

<span data-ttu-id="be588-111">A *class_declaration*クラス修飾子のシーケンスを必要に応じて含めることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="be588-112">同じ修飾子をクラス宣言内で複数回のコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="be588-113">`new`修飾子を入れ子になったクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="be588-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="be588-114">」の説明に従って、クラスが、同じ名前で、継承されたメンバーを非表示にするを指定します、 [new 修飾子](classes.md#the-new-modifier)します。</span><span class="sxs-lookup"><span data-stu-id="be588-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="be588-115">コンパイル時エラーには、`new`修飾子を入れ子になったクラス宣言ではないクラス宣言に表示されます。</span><span class="sxs-lookup"><span data-stu-id="be588-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="be588-116">`public`、 `protected`、 `internal`、および`private`修飾子は、クラスのアクセシビリティを制御します。</span><span class="sxs-lookup"><span data-stu-id="be588-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="be588-117">クラス宣言が発生したコンテキストに応じてこれらの修飾子の一部は使用できません ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="be588-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="be588-118">`abstract`、`sealed`と`static`修飾子は次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="be588-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="be588-119">抽象クラス</span><span class="sxs-lookup"><span data-stu-id="be588-119">Abstract classes</span></span>

<span data-ttu-id="be588-120">`abstract`修飾子を使用して、クラスが完了したと基本クラスとしてのみ使用するものであることを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="be588-121">抽象クラスは、次の方法で非抽象クラスとは異なります。</span><span class="sxs-lookup"><span data-stu-id="be588-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="be588-122">抽象クラスは、直接インスタンス化できないし、コンパイル時エラーに使用する、`new`抽象クラスでの演算子。</span><span class="sxs-lookup"><span data-stu-id="be588-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="be588-123">変数と値がコンパイル時の型が抽象クラスを含めることは可能ですが、このような変数と値は必ずしもなります`null`または抽象型から派生した非抽象クラスのインスタンスへの参照が含まれています。</span><span class="sxs-lookup"><span data-stu-id="be588-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="be588-124">抽象クラスが許可されている (ただし必要ありません) に抽象メンバーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="be588-125">抽象クラスをシールドすることはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="be588-126">非抽象クラスは抽象クラスから派生するときに、非抽象クラスは、これらの抽象メンバーをオーバーライドするため、すべての継承抽象メンバーの実際の実装を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="be588-127">例</span><span class="sxs-lookup"><span data-stu-id="be588-127">In the example</span></span>
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
<span data-ttu-id="be588-128">抽象クラス`A`抽象メソッドを導入`F`します。</span><span class="sxs-lookup"><span data-stu-id="be588-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="be588-129">クラス`B`その他の方法が導入されました`G`の実装を提供しないので`F`、`B`する必要がありますも抽象として宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="be588-130">クラス`C`オーバーライド`F`し、実際の実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="be588-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="be588-131">抽象メンバーがないので`C`、`C`許可しました (ただし必要ありません) は非抽象にします。</span><span class="sxs-lookup"><span data-stu-id="be588-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="be588-132">シール クラス</span><span class="sxs-lookup"><span data-stu-id="be588-132">Sealed classes</span></span>

<span data-ttu-id="be588-133">`sealed`修飾子がクラスからの派生を防ぐために使用します。</span><span class="sxs-lookup"><span data-stu-id="be588-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="be588-134">シール クラスが別のクラスの基底クラスとして指定されている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="be588-135">シール クラスには、抽象クラスにもできません。</span><span class="sxs-lookup"><span data-stu-id="be588-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="be588-136">`sealed`修飾子は、主に、意図しない派生を防ぐために使用しますが、特定のランタイム最適化こともできます。</span><span class="sxs-lookup"><span data-stu-id="be588-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="be588-137">具体的には、シール クラス、派生クラスが含まれないことがわかっているため、シール クラスのインスタンス上の仮想関数メンバーの呼び出しを非仮想呼び出しに変換することができます。</span><span class="sxs-lookup"><span data-stu-id="be588-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="be588-138">静的クラス</span><span class="sxs-lookup"><span data-stu-id="be588-138">Static classes</span></span>

<span data-ttu-id="be588-139">`static`として宣言するクラスをマークする修飾子を使用する***静的クラス***します。</span><span class="sxs-lookup"><span data-stu-id="be588-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="be588-140">静的クラスはインスタンス化できない型として使用することはできません、および静的メンバーのみを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="be588-141">静的クラスは、拡張メソッドの宣言を含めることができますのみ ([拡張メソッド](classes.md#extension-methods))。</span><span class="sxs-lookup"><span data-stu-id="be588-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="be588-142">静的クラスの宣言では、次の制限を受けます。</span><span class="sxs-lookup"><span data-stu-id="be588-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="be588-143">静的クラスが含まない場合があります、`sealed`または`abstract`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="be588-144">ただし、静的クラスをインスタンス化されたかから派生することはできません、ために動作するシールと abstract の両方の場合とに注意してください。</span><span class="sxs-lookup"><span data-stu-id="be588-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="be588-145">静的クラスが含まない場合があります、 *class_base*仕様 ([クラスの基本仕様](classes.md#class-base-specification)) し、基底クラスまたは実装されたインターフェイスのリストを明示的に指定できません。</span><span class="sxs-lookup"><span data-stu-id="be588-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="be588-146">静的クラスは、型から暗黙的に継承`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="be588-147">静的クラスは静的メンバーのみを含むことができます ([静的メンバーとインスタンス メンバー](classes.md#static-and-instance-members))。</span><span class="sxs-lookup"><span data-stu-id="be588-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="be588-148">定数と入れ子にされた型が静的メンバーとして分類されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="be588-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="be588-149">静的クラスを持つメンバーを含めることはできません`protected`または`protected internal`アクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="be588-150">これらの制限に違反するコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="be588-151">静的クラスには、インスタンス コンス トラクターがありません。</span><span class="sxs-lookup"><span data-stu-id="be588-151">A static class has no instance constructors.</span></span> <span data-ttu-id="be588-152">インスタンス コンス トラクター、静的クラスでない既定のインスタンス コンス トラクターを宣言することはできません ([既定のコンス トラクター](classes.md#default-constructors)) は、静的クラスを提供します。</span><span class="sxs-lookup"><span data-stu-id="be588-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="be588-153">静的クラスのメンバーが自動的に静的でないし、メンバーの宣言が明示的に含める必要があります、 `static` (定数と入れ子にされた型) を除く修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="be588-154">クラスは、外部の静的クラス内で入れ子になって、ときに、入れ子になったクラスは静的クラス明示的に含まれていない限り、`static`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="be588-155">__静的クラスの型を参照します。__</span><span class="sxs-lookup"><span data-stu-id="be588-155">__Referencing static class types__</span></span>

<span data-ttu-id="be588-156">A *namespace_or_type_name* ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names)) で許可されている場合は、静的クラスを参照</span><span class="sxs-lookup"><span data-stu-id="be588-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="be588-157">*Namespace_or_type_name*は、`T`で、 *namespace_or_type_name*フォームの`T.I`、または</span><span class="sxs-lookup"><span data-stu-id="be588-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="be588-158">*Namespace_or_type_name*は、`T`で、 *typeof_expression* ([引数リスト](expressions.md#argument-lists)1) フォームの`typeof(T)`します。</span><span class="sxs-lookup"><span data-stu-id="be588-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="be588-159">A *primary_expression* ([関数メンバー](expressions.md#function-members)) で許可されている場合は、静的クラスを参照</span><span class="sxs-lookup"><span data-stu-id="be588-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="be588-160">*Primary_expression*は、`E`で、 *member_access* ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) フォームの`E.I`します。</span><span class="sxs-lookup"><span data-stu-id="be588-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="be588-161">他のコンテキストで静的クラスを参照すると、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="be588-162">エラー構成要素の型の基本クラスとして使用する静的クラスにはたとえば、([入れ子になった型](classes.md#nested-types)) のメンバー、ジェネリック型引数、または型パラメーターの制約。</span><span class="sxs-lookup"><span data-stu-id="be588-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="be588-163">同様に、配列型、ポインター型で静的クラスは使用できません、 `new` 、キャスト式、式、`is`式、`as`式、`sizeof`式、または既定値の式。</span><span class="sxs-lookup"><span data-stu-id="be588-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="be588-164">Partial 識別子</span><span class="sxs-lookup"><span data-stu-id="be588-164">Partial modifier</span></span>

<span data-ttu-id="be588-165">`partial`修飾子を示すために使用されるこの*class_declaration*部分型の宣言です。</span><span class="sxs-lookup"><span data-stu-id="be588-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="be588-166">指定された規則に従って、それを囲む名前空間または型の宣言内で同じ名前の複数の部分型の宣言は、フォームの 1 つの型宣言に結合、[部分型](classes.md#partial-types)します。</span><span class="sxs-lookup"><span data-stu-id="be588-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="be588-167">プログラムのテキストのセグメントを個別に分散クラスの宣言は、これらのセグメントを生成または別のコンテキスト内に保持する場合役立ちます。</span><span class="sxs-lookup"><span data-stu-id="be588-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="be588-168">たとえば、クラス宣言の 1 つの部分可能性がありますコンピューターによって生成されると、一方は手動で作成した他の。</span><span class="sxs-lookup"><span data-stu-id="be588-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="be588-169">2 つのテキストの分離は、他の更新と衝突する 1 つを更新プログラムを抑止します。</span><span class="sxs-lookup"><span data-stu-id="be588-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="be588-170">型パラメーター</span><span class="sxs-lookup"><span data-stu-id="be588-170">Type parameters</span></span>

<span data-ttu-id="be588-171">型パラメーターは、構築された型を作成する指定された型引数のプレース ホルダーを示す単純な識別子です。</span><span class="sxs-lookup"><span data-stu-id="be588-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="be588-172">型パラメーターは、後で提供される型の正式なプレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="be588-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="be588-173">一方、型引数 ([引数を入力](types.md#type-arguments)) が構築された型が作成されるときに、型パラメーターを置き換える実際の型。</span><span class="sxs-lookup"><span data-stu-id="be588-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="be588-174">クラス宣言で型パラメーターごとの宣言領域に名前を定義します ([宣言](basic-concepts.md#declarations)) そのクラスの。</span><span class="sxs-lookup"><span data-stu-id="be588-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="be588-175">そのため、別の型パラメーターと同じ名前を持つことはできませんまたはメンバーがそのクラスで宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="be588-176">型パラメーターには、型自体と同じ名前を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="be588-177">クラスの基本仕様</span><span class="sxs-lookup"><span data-stu-id="be588-177">Class base specification</span></span>

<span data-ttu-id="be588-178">クラス宣言を含めることができます、 *class_base*クラスとインターフェイスの直接基底クラスを定義する仕様 ([インターフェイス](interfaces.md)) 直接クラスによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="be588-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="be588-179">クラス宣言で指定された基本クラスは、構築済みクラス型を指定できます ([構築型](types.md#constructed-types))。</span><span class="sxs-lookup"><span data-stu-id="be588-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="be588-180">スコープ内の型パラメーターを伴うことができますが、基底クラスは、独自の型パラメーターにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="be588-181">基底クラス</span><span class="sxs-lookup"><span data-stu-id="be588-181">Base classes</span></span>

<span data-ttu-id="be588-182">ときに、 *class_type*に含まれている、 *class_base*、宣言されたクラスの直接の基本クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="be588-183">クラス宣言がにない場合*class_base*、または、 *class_base*リストのみインターフェイスの種類、直接基底クラスがあると見なされます`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="be588-184">」の説明に従って、クラスの直接の基本クラスからの継承メンバー[継承](classes.md#inheritance)します。</span><span class="sxs-lookup"><span data-stu-id="be588-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="be588-185">例</span><span class="sxs-lookup"><span data-stu-id="be588-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="be588-186">クラス`A`の直接基底クラスと呼ばれます`B`、および`B`から派生する言います`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="be588-187">`A`は直接基底クラスを明示的に指定、その直接の基本クラスが暗黙的に`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="be588-188">構築済みクラス型では、ジェネリック クラスの宣言で基底クラスが指定されている場合、構築された型の基本クラスを取得、ごとに置き換えることによって*type_parameter*対応する基本クラスの宣言で*type_argument*構築された型のです。</span><span class="sxs-lookup"><span data-stu-id="be588-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="be588-189">ジェネリック クラス宣言</span><span class="sxs-lookup"><span data-stu-id="be588-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="be588-190">構築された型の基本クラス`G<int>`なります`B<string,int[]>`します。</span><span class="sxs-lookup"><span data-stu-id="be588-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="be588-191">クラス型の直接の基本クラスは、少なくとも、クラス型自体と同程度にアクセスである必要があります ([アクセシビリティ ドメイン](basic-concepts.md#accessibility-domains))。</span><span class="sxs-lookup"><span data-stu-id="be588-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="be588-192">などのコンパイル時エラーは、`public`クラスから派生する、`private`または`internal`クラス。</span><span class="sxs-lookup"><span data-stu-id="be588-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="be588-193">クラス型の直接の基本クラスでは、次の種類のいずれかのある必要がありますいない: `System.Array`、 `System.Delegate`、 `System.MulticastDelegate`、 `System.Enum`、または`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="be588-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="be588-194">ジェネリック クラス宣言をさらに、使用することはできません`System.Attribute`直接的または間接的な基底クラスとして。</span><span class="sxs-lookup"><span data-stu-id="be588-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="be588-195">直接基底クラスの指定の意味を決定する際に`A`クラスの`B`、直接の基本クラスの`B`は一時的にあると見なされます`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="be588-196">基底クラスの指定の意味が再帰的にことはできません、直感的にこれにより、それ自体に依存します。</span><span class="sxs-lookup"><span data-stu-id="be588-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="be588-197">例:</span><span class="sxs-lookup"><span data-stu-id="be588-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="be588-198">以降、基底クラスの指定でのエラーでは、`A<C.B>`の直接の基本クラス`C`と見なされます`object`、ため、(のルールによって[Namespace と型の名前](basic-concepts.md#namespace-and-type-names))`C`と見なされないメンバーを持つ`B`します。</span><span class="sxs-lookup"><span data-stu-id="be588-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="be588-199">クラス型の基底クラスは、直接基底クラスとその基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="be588-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="be588-200">つまり、基本クラスのセットは、直接基底クラスの関係の推移閉包です。</span><span class="sxs-lookup"><span data-stu-id="be588-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="be588-201">上記の基本クラスの例を参照する`B`は`A`と`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="be588-202">例</span><span class="sxs-lookup"><span data-stu-id="be588-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="be588-203">基本クラス`D<int>`は`C<int[]>`、 `B<IComparable<int[]>>`、 `A`、および`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="be588-204">クラスを除く`object`、すべてのクラス型が正確に 1 つの直接基底クラス。</span><span class="sxs-lookup"><span data-stu-id="be588-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="be588-205">`object`クラスの直接基底クラスを持たないし、他のすべてのクラスの最終的な基底クラスです。</span><span class="sxs-lookup"><span data-stu-id="be588-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="be588-206">クラスと`B`クラスから派生する`A`、コンパイル時エラーには`A`に依存する`B`します。</span><span class="sxs-lookup"><span data-stu-id="be588-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="be588-207">クラス***に直接依存***(あれば) 直接基本クラスと***に直接依存***はすぐに入れ子になっている (あれば) クラス。</span><span class="sxs-lookup"><span data-stu-id="be588-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="be588-208">この定義を指定するには、クラスが依存しているクラスの完全なセットがの再帰的、および推移的なクロージャ、***に直接依存***リレーションシップ。</span><span class="sxs-lookup"><span data-stu-id="be588-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="be588-209">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="be588-210">誤ったクラスは、自身に依存しているためです。</span><span class="sxs-lookup"><span data-stu-id="be588-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="be588-211">例では、同様に、</span><span class="sxs-lookup"><span data-stu-id="be588-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="be588-212">クラスは、循環的自体に依存するためにはエラーです。</span><span class="sxs-lookup"><span data-stu-id="be588-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="be588-213">例では、最後に、</span><span class="sxs-lookup"><span data-stu-id="be588-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="be588-214">ため、コンパイル時エラー結果`A`に依存`B.C`(直接基底クラス) に依存する`B`(すぐ外側のクラス) に依存する循環的`A`。</span><span class="sxs-lookup"><span data-stu-id="be588-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="be588-215">クラスは、内部に入れ子になっているクラスに依存しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="be588-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="be588-216">例</span><span class="sxs-lookup"><span data-stu-id="be588-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="be588-217">`B` 依存`A`(ため`A`は、直接基底クラスとそのすぐ外側のクラスの両方) が`A`に依存しない`B`(ため`B`は基底クラスでも、外側のクラスの`A`).</span><span class="sxs-lookup"><span data-stu-id="be588-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="be588-218">そのため、この例では有効です。</span><span class="sxs-lookup"><span data-stu-id="be588-218">Thus, the example is valid.</span></span>

<span data-ttu-id="be588-219">派生することはできません、`sealed`クラス。</span><span class="sxs-lookup"><span data-stu-id="be588-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="be588-220">例</span><span class="sxs-lookup"><span data-stu-id="be588-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="be588-221">クラス`B`しようとするから派生するために、エラーでは、`sealed`クラス`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="be588-222">インターフェイスの実装</span><span class="sxs-lookup"><span data-stu-id="be588-222">Interface implementations</span></span>

<span data-ttu-id="be588-223">A *class_base*仕様は場合、クラスが言います、特定のインターフェイス型を直接実装するインターフェイスの型の一覧を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="be588-224">インターフェイスの実装については、説明でさらに[インターフェイスの実装](interfaces.md#interface-implementations)します。</span><span class="sxs-lookup"><span data-stu-id="be588-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="be588-225">型パラメーターの制約</span><span class="sxs-lookup"><span data-stu-id="be588-225">Type parameter constraints</span></span>

<span data-ttu-id="be588-226">ジェネリック型、メソッドの宣言を含めることで型パラメーターの制約を指定できます必要に応じて*type_parameter_constraints_clause*秒。</span><span class="sxs-lookup"><span data-stu-id="be588-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="be588-227">各*type_parameter_constraints_clause*トークンから成る`where`後に、型パラメーターの名前の後にコロンとその型パラメーターの制約の一覧。</span><span class="sxs-lookup"><span data-stu-id="be588-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="be588-228">最大で 1 つできます`where`型パラメーターごとに、句、`where`句は、任意の順序で表示できます。</span><span class="sxs-lookup"><span data-stu-id="be588-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="be588-229">ように、`get`と`set`プロパティのアクセサー内のトークン、`where`トークンは、キーワードではありません。</span><span class="sxs-lookup"><span data-stu-id="be588-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="be588-230">指定された制約の一覧を`where`句は、次のコンポーネントのいずれかをこの順序で含めることができます。 1 つの主な制約、1 つまたは複数のセカンダリ制約、およびコンス トラクターの制約`new()`します。</span><span class="sxs-lookup"><span data-stu-id="be588-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="be588-231">主な制約は、クラス型を指定できますまたは***参照型制約***`class`または***値の型の制約*** `struct`。</span><span class="sxs-lookup"><span data-stu-id="be588-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="be588-232">セカンダリの制約として使用できる、 *type_parameter*または*interface_type*します。</span><span class="sxs-lookup"><span data-stu-id="be588-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="be588-233">参照型の制約では、型パラメーターを使用する型引数は参照型である必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="be588-234">すべてのクラス型、インターフェイス型、デリゲート型、配列型、および (下記を参照) のように、参照型に既知の型パラメーターは、この制約を満たしています。</span><span class="sxs-lookup"><span data-stu-id="be588-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="be588-235">値型の制約では、型パラメーターを使用する型引数は null 非許容値型である必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="be588-236">すべての null 非許容の構造体型、列挙型、および値型の制約を持つ型パラメーターは、この制約を満たしています。</span><span class="sxs-lookup"><span data-stu-id="be588-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="be588-237">値型、null 許容型に分類されますが ([null 許容型](types.md#nullable-types)) 値の型の制約を満たしていません。</span><span class="sxs-lookup"><span data-stu-id="be588-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="be588-238">値型の制約を持つ型パラメーターも含めることはできません、 *constructor_constraint*します。</span><span class="sxs-lookup"><span data-stu-id="be588-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="be588-239">ポインター型では、型引数を許可しないと、参照型または値型のいずれかの制約を満たすためには考慮されません。</span><span class="sxs-lookup"><span data-stu-id="be588-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="be588-240">クラス型、インターフェイス型または型パラメーター制約とは、その型は最小限に抑える「ベース型」型パラメーターに使用されるすべての型引数をサポートする必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="be588-241">構築された型またはジェネリック メソッドを使用すると、型引数がコンパイル時に型パラメーターに対する制約に照らしてチェックします。</span><span class="sxs-lookup"><span data-stu-id="be588-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="be588-242">指定された型引数で説明する条件を満たす必要があります[制約条件を満たす](types.md#satisfying-constraints)します。</span><span class="sxs-lookup"><span data-stu-id="be588-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="be588-243">A *class_type*制約は、次の規則を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="be588-244">種類は、クラス型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-244">The type must be a class type.</span></span>
*  <span data-ttu-id="be588-245">型でなければなりません`sealed`します。</span><span class="sxs-lookup"><span data-stu-id="be588-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="be588-246">型は、次の種類のいずれかを指定しない必要があります: `System.Array`、 `System.Delegate`、 `System.Enum`、または`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="be588-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="be588-247">型でなければなりません`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-247">The type must not be `object`.</span></span> <span data-ttu-id="be588-248">すべての型が派生するため`object`、このような制約は効果がありませんが、許可された場合。</span><span class="sxs-lookup"><span data-stu-id="be588-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="be588-249">特定の型パラメーターを最大で 1 つの制約は、クラス型であることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="be588-250">として指定された型、 *interface_type*制約は、次の規則を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="be588-251">型はインターフェイス型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="be588-252">複数回に指定されていない型もする必要がありますを指定した`where`句。</span><span class="sxs-lookup"><span data-stu-id="be588-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="be588-253">いずれの場合も、制約は、関連付けられている型または構築された型の一部として、メソッドの宣言の型パラメーターのいずれかが含まれることができ、宣言された型が含まれることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="be588-254">型パラメーターの制約は、少なくともにアクセスする必要がありますを指定したクラスまたはインターフェイスの型 ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints)) ジェネリック型またはメソッドが宣言されているとします。</span><span class="sxs-lookup"><span data-stu-id="be588-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="be588-255">として指定された型、 *type_parameter*制約は、次の規則を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="be588-256">種類は、型パラメーターである必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="be588-257">複数回に指定されていない型もする必要がありますを指定した`where`句。</span><span class="sxs-lookup"><span data-stu-id="be588-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="be588-258">さらにはならないサイクルによって定義されている推移的な関係の依存関係のある、型パラメーターの依存関係グラフ。</span><span class="sxs-lookup"><span data-stu-id="be588-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="be588-259">場合、型パラメーター`T`型パラメーターを制約として使用`S`し`S`***異なります***`T`します。</span><span class="sxs-lookup"><span data-stu-id="be588-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="be588-260">場合、型パラメーター`S`型パラメーターに依存`T`と`T`型パラメーターに依存`U`し`S`***異なります*** `U`。</span><span class="sxs-lookup"><span data-stu-id="be588-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="be588-261">この関係がある、(直接または間接的に)、それ自体に依存する型パラメーターのコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="be588-262">すべての制約は、依存する型パラメーターの間で一貫性のあるである必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="be588-263">場合のパラメーターを入力`S`型パラメーターに依存`T`し。</span><span class="sxs-lookup"><span data-stu-id="be588-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="be588-264">`T` 値型の制約が必要ありません。</span><span class="sxs-lookup"><span data-stu-id="be588-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="be588-265">それ以外の場合、`T`これは実質的にシール`S`と同じ型にすることが強制されます`T`、2 つの型パラメーターでは必要はありません。</span><span class="sxs-lookup"><span data-stu-id="be588-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="be588-266">場合`S`しの値の型の制約を持つ`T`は必要ありません、 *class_type*制約。</span><span class="sxs-lookup"><span data-stu-id="be588-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="be588-267">場合`S`が、 *class_type*制約`A`と`T`が、 *class_type*制約`B`恒等変換が存在する必要がありますまたは暗黙的な参照変換から`A`に`B`から暗黙の参照変換または`B`に`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="be588-268">場合`S`型パラメーターにも依存`U`と`U`が、 *class_type*制約`A`と`T`が、 *class_type*制約`B`恒等変換またはからの暗黙的な参照変換がある必要がありますし、`A`に`B`から暗黙の参照変換または`B`に`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="be588-269">それが有効で`S`が値型の制約と`T`参照型の制約にします。</span><span class="sxs-lookup"><span data-stu-id="be588-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="be588-270">これを効果的に制限`T`型に`System.Object`、 `System.ValueType`、 `System.Enum`、および任意のインターフェイス型。</span><span class="sxs-lookup"><span data-stu-id="be588-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="be588-271">場合、`where`句型パラメーターにはにコンス トラクターの制約が含まれています (フォームを持つ`new()`) を使用することは、`new`演算子、型のインスタンスを作成する ([オブジェクト作成式](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="be588-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="be588-272">使用する型引数が値型の制約またはコンス トラクターの制約 (参照型パラメーターまたは型パラメーターでは、コンス トラクター制約する必要があります (このコンス トラクターは任意の値型に暗黙的に存在する) のパブリック コンス トラクターを指定すること[パラメーターの制約入力](classes.md#type-parameter-constraints)詳細については)。</span><span class="sxs-lookup"><span data-stu-id="be588-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="be588-273">制約の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="be588-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="be588-274">次の例は、型パラメーターの依存関係グラフで循環が発生しているために、エラーには。</span><span class="sxs-lookup"><span data-stu-id="be588-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="be588-275">次の例では、その他の無効な状況を示します。</span><span class="sxs-lookup"><span data-stu-id="be588-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="be588-276">***実質的な基本クラス***型パラメーターの`T`が次のように定義されています。</span><span class="sxs-lookup"><span data-stu-id="be588-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="be588-277">場合`T`primary 制約がない場合や、その効果的な基底クラスの型パラメーター制約が`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="be588-278">場合`T`値型の制約、その効果的な基底クラスは`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="be588-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="be588-279">場合`T`が、 *class_type*制約`C`が*type_parameter*制約、その効果的な基底クラスは`C`します。</span><span class="sxs-lookup"><span data-stu-id="be588-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="be588-280">場合`T`にない*class_type*制約が、1 つまたは複数*type_parameter*制約、その効果的な基底クラスは、最も内側の型 ([リフトの変換演算子](conversions.md#lifted-conversion-operators)) の有効な基本クラスのセットでその*type_parameter*制約。</span><span class="sxs-lookup"><span data-stu-id="be588-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="be588-281">一貫性規則では、このような最も内側の型が存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="be588-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="be588-282">場合`T`両方を持つ、 *class_type*制約と 1 つ以上*type_parameter*制約、その効果的な基底クラスは、最も内側の型 ([リフトの変換演算子](conversions.md#lifted-conversion-operators)) から成るセットで、 *class_type*の制約`T`の効果的な基底クラスとその*type_parameter*制約。</span><span class="sxs-lookup"><span data-stu-id="be588-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="be588-283">一貫性規則では、このような最も内側の型が存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="be588-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="be588-284">場合`T`を持ち、参照型の制約はいいえ*class_type*制約、その効果的な基底クラスは`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="be588-285">T に制約がある場合、これらの規則の目的で`V`つまり、 *value_type*、代わりに最も固有の基本型の`V`は、 *class_type*します。</span><span class="sxs-lookup"><span data-stu-id="be588-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="be588-286">これは、明示的に特定の制約では発生しないことができますが、ジェネリック メソッドの制約は、オーバーライドするメソッドの宣言または明示的なインターフェイス メソッドの実装によって暗黙的に継承されたときに発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="be588-287">これらの規則は、有効な基本クラスが常にあることを確認、 *class_type*します。</span><span class="sxs-lookup"><span data-stu-id="be588-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="be588-288">***有効なインターフェイス セット***型パラメーターの`T`が次のように定義されています。</span><span class="sxs-lookup"><span data-stu-id="be588-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="be588-289">場合`T`にない*secondary_constraints*、その有効なインターフェイス セットが空です。</span><span class="sxs-lookup"><span data-stu-id="be588-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="be588-290">場合`T`が*interface_type*制約がない*type_parameter*制約、その有効なインターフェイス セットが一連の*interface_type*制約。</span><span class="sxs-lookup"><span data-stu-id="be588-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="be588-291">場合`T`にない*interface_type*制約が、 *type_parameter*制約、その有効なインターフェイス セットがの有効なインターフェイスのセットの和集合の*種類パラメーター*制約。</span><span class="sxs-lookup"><span data-stu-id="be588-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="be588-292">場合`T`両方*interface_type*制約と*type_parameter*制約、その有効なインターフェイス セットがそのセットの和集合*interface_type*制約との有効なインターフェイス セットをその*type_parameter*制約。</span><span class="sxs-lookup"><span data-stu-id="be588-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="be588-293">型パラメーターが***参照型に既知の***参照型の制約があるかどうか、または、有効な基本クラスが`object`または`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="be588-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="be588-294">制約付きの型パラメーターの型の値は、制約によって暗黙的にインスタンス メンバーへのアクセスに使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="be588-295">例</span><span class="sxs-lookup"><span data-stu-id="be588-295">In the example</span></span>
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
<span data-ttu-id="be588-296">メソッド`IPrintable`上で直接呼び出すことができます`x`ため`T`が常に実装するために制約`IPrintable`します。</span><span class="sxs-lookup"><span data-stu-id="be588-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="be588-297">クラス本体</span><span class="sxs-lookup"><span data-stu-id="be588-297">Class body</span></span>

<span data-ttu-id="be588-298">*Class_body*クラスのクラスのメンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="be588-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="be588-299">部分型</span><span class="sxs-lookup"><span data-stu-id="be588-299">Partial types</span></span>

<span data-ttu-id="be588-300">型宣言を複数に分割できるようにする***部分型の宣言***します。</span><span class="sxs-lookup"><span data-stu-id="be588-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="be588-301">型宣言は、当該プログラムのコンパイル時と実行時の処理の残りの部分の中に 1 つの宣言として扱われます、このセクションでは、規則に従っての部分から構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="be588-302">A *class_declaration*、 *struct_declaration*または*interface_declaration*が含まれる場合は、部分型の宣言を表す、`partial`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="be588-303">`partial` キーワードでないし、キーワードのいずれかの直前に表示される場合、修飾子としてのみ機能`class`、`struct`または`interface`型宣言、または型の前に`void`メソッドの宣言でします。</span><span class="sxs-lookup"><span data-stu-id="be588-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="be588-304">他のコンテキストで、通常の識別子として使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="be588-305">部分型の宣言の各部分を含める必要があります、`partial`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="be588-306">同じ名前を指定する必要があり、同じ名前空間または型の他の部分宣言で宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="be588-307">`partial`修飾子は、型宣言の追加部分が別の場所に存在してもこのようなその他の部分が存在することが必須ではないことを示します。 1 つの宣言を持つ型を含めることは、`partial`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="be588-308">1 つの型宣言に、コンパイル時に、パーツをマージできるように、部分型のすべての部分を一緒にコンパイルする必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="be588-309">具体的には、部分型には既にコンパイルされている型を拡張することはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="be588-310">使用して複数の部分に入れ子にされた型を宣言することがあります、`partial`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="be588-311">通常、含んでいる型が宣言を使用して`partial`良好であり、入れ子にされた型の各部分を含んでいる型の別の部分で宣言したので。</span><span class="sxs-lookup"><span data-stu-id="be588-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="be588-312">`partial`デリゲートまたは列挙型の宣言に修飾子は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="be588-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="be588-313">属性</span><span class="sxs-lookup"><span data-stu-id="be588-313">Attributes</span></span>

<span data-ttu-id="be588-314">部分型の属性は、各部分の属性を不特定の順序で組み合わせることで決定されます。</span><span class="sxs-lookup"><span data-stu-id="be588-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="be588-315">属性は、複数の部分に配置する場合は、型の属性を複数回指定すると同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="be588-316">たとえば、2 つの部分があると:</span><span class="sxs-lookup"><span data-stu-id="be588-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="be588-317">などの宣言に同等です。</span><span class="sxs-lookup"><span data-stu-id="be588-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="be588-318">型パラメーターの属性は、同様の方法で結合します。</span><span class="sxs-lookup"><span data-stu-id="be588-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="be588-319">修飾子</span><span class="sxs-lookup"><span data-stu-id="be588-319">Modifiers</span></span>

<span data-ttu-id="be588-320">部分型の宣言がアクセシビリティの指定が含まれてとき (、 `public`、 `protected`、 `internal`、および`private`修飾子)、アクセシビリティの仕様が含まれるその他のすべての部分に同意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="be588-321">アクセシビリティの指定を含む、部分的な型の部分がない場合、型が指定された適切な既定のアクセシビリティ ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="be588-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="be588-322">入れ子にされた型の 1 つまたは複数の部分宣言が含まれる場合、`new`修飾子は、入れ子にされた型が継承されたメンバーを非表示にする場合に警告が報告されません ([継承による隠ぺい](basic-concepts.md#hiding-through-inheritance))。</span><span class="sxs-lookup"><span data-stu-id="be588-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="be588-323">クラスの 1 つまたは複数の部分宣言が含まれる場合、`abstract`修飾子、クラスが抽象と見なされます ([抽象クラス](classes.md#abstract-classes))。</span><span class="sxs-lookup"><span data-stu-id="be588-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="be588-324">それ以外の場合、クラスは、非抽象と見なされます。</span><span class="sxs-lookup"><span data-stu-id="be588-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="be588-325">クラスの 1 つまたは複数の部分宣言が含まれる場合、`sealed`修飾子、クラスがシールと見なされます ([クラスをシール](classes.md#sealed-classes))。</span><span class="sxs-lookup"><span data-stu-id="be588-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="be588-326">クラスを考慮する場合は、封印されていません。</span><span class="sxs-lookup"><span data-stu-id="be588-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="be588-327">クラスを abstract と sealed の両方ができないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="be588-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="be588-328">ときに、`unsafe`その特定の部分は、unsafe コンテキストと見なされるのみ、部分型の宣言に修飾子が使用されます ([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="be588-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="be588-329">型パラメーターと制約</span><span class="sxs-lookup"><span data-stu-id="be588-329">Type parameters and constraints</span></span>

<span data-ttu-id="be588-330">ジェネリック型が複数の部分で宣言されている場合、型パラメーターが各部分に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="be588-331">各部分は、順番、型パラメーターのと同じ数と型パラメーターごとに、同じ名前が必要です。</span><span class="sxs-lookup"><span data-stu-id="be588-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="be588-332">部分的なジェネリック型の宣言が制約が含まれる場合 (`where`句)、制約は制約を含むその他のすべての部分と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="be588-333">具体的には、制約が含まれる各の部分は、型パラメーターの同じセットに対して制約を持つ必要があり、型パラメーターごとのプライマリ、セカンダリ、およびコンス トラクター制約必要がありますと同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="be588-334">制約の 2 つのセットは、同じメンバーが含まれている場合に相当します。</span><span class="sxs-lookup"><span data-stu-id="be588-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="be588-335">部分的なジェネリック型の一部は、型パラメーターの制約がないを指定します、パラメーターと見なされる型は無制限です。</span><span class="sxs-lookup"><span data-stu-id="be588-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="be588-336">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-336">The example</span></span>
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
<span data-ttu-id="be588-337">正しい効果的に制約 (最初の 2 つ) を含む部分が同じセカンダリをプライマリのセットと同じ型パラメーターでは、一連のコンス トラクターの制約を指定するためです。</span><span class="sxs-lookup"><span data-stu-id="be588-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="be588-338">基底クラス</span><span class="sxs-lookup"><span data-stu-id="be588-338">Base class</span></span>

<span data-ttu-id="be588-339">部分クラス宣言には、基底クラスの指定が含まれています。 基底クラスの指定を含むその他のすべての部分を持つことに同意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="be588-340">部分クラスの一部に基底クラスの指定が含まれていない場合、基本クラスは次のようになります。 `System.Object` ([基底クラス](classes.md#base-classes))。</span><span class="sxs-lookup"><span data-stu-id="be588-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="be588-341">基底インターフェイス</span><span class="sxs-lookup"><span data-stu-id="be588-341">Base interfaces</span></span>

<span data-ttu-id="be588-342">複数の部分で宣言された型の基本インターフェイスのセットは、各部分で指定された基本インターフェイスの和集合です。</span><span class="sxs-lookup"><span data-stu-id="be588-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="be588-343">各部分では、1 回にという名前のみ、特定の基本インターフェイスも可能性がありますが、同じ基本インターフェイスの名前を複数の部分のことができます。</span><span class="sxs-lookup"><span data-stu-id="be588-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="be588-344">任意の基本インターフェイスのメンバーの実装の 1 つだけあります。</span><span class="sxs-lookup"><span data-stu-id="be588-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="be588-345">例</span><span class="sxs-lookup"><span data-stu-id="be588-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="be588-346">クラスの基本インターフェイスのセット`C`は`IA`、 `IB`、および`IC`します。</span><span class="sxs-lookup"><span data-stu-id="be588-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="be588-347">通常、各部分はその部分で宣言されたインターフェイスの実装を提供します。ただし、これは要件ではありません。</span><span class="sxs-lookup"><span data-stu-id="be588-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="be588-348">一部は、別の部分で宣言されたインターフェイスの実装を提供できます。</span><span class="sxs-lookup"><span data-stu-id="be588-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="be588-349">メンバー</span><span class="sxs-lookup"><span data-stu-id="be588-349">Members</span></span>

<span data-ttu-id="be588-350">部分メソッドを除く ([部分メソッド](classes.md#partial-methods))、複数の部分で宣言された型のメンバーのセットは、各部分で宣言されたメンバーのセットの和集合だけです。</span><span class="sxs-lookup"><span data-stu-id="be588-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="be588-351">型宣言のすべての部分の本文が同じ宣言領域を共有 ([宣言](basic-concepts.md#declarations))、および各メンバーのスコープ ([スコープ](basic-concepts.md#scopes)) に、すべての部分の本文を拡張します。</span><span class="sxs-lookup"><span data-stu-id="be588-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="be588-352">任意のメンバーのアクセシビリティ ドメインは、外側の型のすべての部分を常に含まれます`private` 1 つの部分で宣言されたメンバーは、別の部分から自由にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="be588-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="be588-353">そのメンバーが持つ型でない限り、型の 1 つ以上の部分で同じメンバーを宣言すると、コンパイル時エラーは、`partial`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="be588-354">型内のメンバーの順序が、c# のコードをほとんど重要では、他の言語や環境とやり取りするときに大きな可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="be588-355">このような場合は、複数の部分で宣言された型内のメンバーの順序は定義されません。</span><span class="sxs-lookup"><span data-stu-id="be588-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="be588-356">部分メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-356">Partial methods</span></span>

<span data-ttu-id="be588-357">部分メソッドは、型宣言の 1 つの部分で定義されているし、別に実装できます。</span><span class="sxs-lookup"><span data-stu-id="be588-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="be588-358">実装は省略可能です。部分メソッドを実装部分がない場合、すべての呼び出しと部分メソッド宣言は、部分の組み合わせから作成される型の宣言から削除されます。</span><span class="sxs-lookup"><span data-stu-id="be588-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="be588-359">部分メソッドは、アクセス修飾子を定義することはできませんが、暗黙的に`private`します。</span><span class="sxs-lookup"><span data-stu-id="be588-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="be588-360">戻り値の型である必要があります`void`、そのパラメーターを持つことができません、`out`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="be588-361">識別子`partial`直前に表示される場合にのみは、特殊なメソッドの宣言でキーワードとして認識、`void`型。 それ以外の場合、通常の識別子として使用できますが。</span><span class="sxs-lookup"><span data-stu-id="be588-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="be588-362">部分メソッドは、インターフェイス メソッドを明示的に実装できません。</span><span class="sxs-lookup"><span data-stu-id="be588-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="be588-363">部分メソッド宣言の 2 つの種類があります。メソッド宣言の本体がセミコロンの場合は、宣言と呼ばれます、***部分メソッドの宣言を定義する***します。</span><span class="sxs-lookup"><span data-stu-id="be588-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="be588-364">として本文を指定した場合、*ブロック*、宣言はモード、***実装部分メソッド宣言***します。</span><span class="sxs-lookup"><span data-stu-id="be588-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="be588-365">型宣言の部分があります指定されたシグネチャを持つ部分メソッド宣言を定義する 1 つだけと実装、特定のシグネチャを持つ部分メソッド宣言は 1 つのみです。</span><span class="sxs-lookup"><span data-stu-id="be588-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="be588-366">部分メソッドの実装宣言が指定された、対応する部分メソッド宣言の定義が存在する必要がありますとして宣言する必要がありますと一致する場合は、次に指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="be588-367">宣言は、(が必ずしも同じ順序で)、同じ修飾子を設定する必要があります、メソッド名、型パラメーターの数、およびパラメーターの数。</span><span class="sxs-lookup"><span data-stu-id="be588-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="be588-368">対応するパラメーターの宣言では、(が必ずしも同じ順序で)、同じ修飾子を設定する必要がありますと同じ型 (型パラメーター名の相違) を除く。</span><span class="sxs-lookup"><span data-stu-id="be588-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="be588-369">宣言内の対応する型パラメーター (型パラメーター名の相違) を除くと同じ制約があります。</span><span class="sxs-lookup"><span data-stu-id="be588-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="be588-370">部分メソッドの実装宣言は、対応する部分メソッドの定義宣言と同じ部分に表示できます。</span><span class="sxs-lookup"><span data-stu-id="be588-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="be588-371">部分メソッドが定義するだけでは、オーバー ロードの解決に関与します。</span><span class="sxs-lookup"><span data-stu-id="be588-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="be588-372">したがって、実装宣言が指定されているかどうかを示す invocation 式は、部分メソッドの呼び出しを解決します。</span><span class="sxs-lookup"><span data-stu-id="be588-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="be588-373">部分メソッドは常に返すため`void`、このような呼び出しの式の式ステートメントは常になります。</span><span class="sxs-lookup"><span data-stu-id="be588-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="be588-374">さらに、部分メソッドは暗黙的にあるため`private`、このようなステートメントは常に内で部分メソッドを宣言する型宣言の部分の 1 つ発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="be588-375">部分型の宣言の一部に特定の部分メソッドの実装宣言が含まれていない場合、それを呼び出す任意の式ステートメントは結合型の宣言から削除だけです。</span><span class="sxs-lookup"><span data-stu-id="be588-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="be588-376">したがって、呼び出しなど、式の構成の式には、実行時に影響はありません。</span><span class="sxs-lookup"><span data-stu-id="be588-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="be588-377">部分メソッド自体も削除され、結合型の宣言のメンバーはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="be588-378">特定の部分メソッドの実装宣言がある場合は、部分メソッドの呼び出しが保持されます。</span><span class="sxs-lookup"><span data-stu-id="be588-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="be588-379">部分メソッドでは、次を除く部分メソッドの実装宣言のようなメソッドの宣言に上昇。</span><span class="sxs-lookup"><span data-stu-id="be588-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="be588-380">`partial`修飾子が含まれていません。</span><span class="sxs-lookup"><span data-stu-id="be588-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="be588-381">結果として得られるメソッドの宣言内の属性は、不特定の順序で、定義と実装の部分メソッド宣言の結合の属性です。</span><span class="sxs-lookup"><span data-stu-id="be588-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="be588-382">重複は削除されません。</span><span class="sxs-lookup"><span data-stu-id="be588-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="be588-383">結果として得られるメソッドの宣言のパラメーターの属性は、不特定の順序で、定義と実装の部分メソッド宣言の対応するパラメーターの結合の属性です。</span><span class="sxs-lookup"><span data-stu-id="be588-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="be588-384">重複は削除されません。</span><span class="sxs-lookup"><span data-stu-id="be588-384">Duplicates are not removed.</span></span>

<span data-ttu-id="be588-385">部分メソッド M の実装宣言がなく、定義宣言を指定した場合、次の制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="be588-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="be588-386">メソッドにデリゲートを作成すると、コンパイル時エラー ([デリゲート作成式](expressions.md#delegate-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="be588-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="be588-387">参照すると、コンパイル時エラー`M`式ツリー型に変換される匿名関数の内側 ([式ツリー型への匿名関数の変換の評価](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="be588-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="be588-388">式の呼び出しの一部として発生する`M`確実な割り当ての状態には影響しません ([確実な代入](variables.md#definite-assignment))、コンパイル時エラーにすることになります。</span><span class="sxs-lookup"><span data-stu-id="be588-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="be588-389">`M` アプリケーションのエントリ ポイントにすることはできません ([アプリケーションの起動](basic-concepts.md#application-startup))。</span><span class="sxs-lookup"><span data-stu-id="be588-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="be588-390">部分メソッドは、ツールによって生成されるものなど、別の部分の動作をカスタマイズする型宣言の 1 つの部分を許可する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="be588-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="be588-391">次の部分クラス宣言を検討してください。</span><span class="sxs-lookup"><span data-stu-id="be588-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="be588-392">他の部分でもないこのクラスがコンパイルされると、部分メソッドの定義宣言とその呼び出しは削除され、結果の結合されたクラス宣言は、次と同等になります。</span><span class="sxs-lookup"><span data-stu-id="be588-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="be588-393">別の部分が与えられている、ただし、部分メソッドの実装宣言を提供するものとします。</span><span class="sxs-lookup"><span data-stu-id="be588-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="be588-394">結果の結合されたクラス宣言は次と同等になります。</span><span class="sxs-lookup"><span data-stu-id="be588-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="be588-395">名前のバインド</span><span class="sxs-lookup"><span data-stu-id="be588-395">Name binding</span></span>

<span data-ttu-id="be588-396">拡張可能な型の各部分は、同じ名前空間内で宣言する必要があります、パーツは通常、別の名前空間宣言内で書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="be588-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="be588-397">したがって、異なる`using`ディレクティブ ([ディレクティブを使用して](namespaces.md#using-directives)) の各部分に存在する場合があります。</span><span class="sxs-lookup"><span data-stu-id="be588-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="be588-398">単純な名前を解釈するときに ([型推論](expressions.md#type-inference)) のみ 1 つのパーツ内で、`using`その一部を囲む名前空間宣言のディレクティブと見なされます。</span><span class="sxs-lookup"><span data-stu-id="be588-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="be588-399">これにより、さまざまな部分で異なる意味を持つ同じ識別子可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="be588-400">クラス メンバー</span><span class="sxs-lookup"><span data-stu-id="be588-400">Class members</span></span>

<span data-ttu-id="be588-401">クラスのメンバーで導入されたメンバーで構成されているその*class_member_declaration*s およびメンバーは、直接基底クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="be588-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="be588-402">クラス型のメンバーは、次のカテゴリに分類されます。</span><span class="sxs-lookup"><span data-stu-id="be588-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="be588-403">クラスに関連付けられている定数の値を表す定数 ([定数](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="be588-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="be588-404">クラスの変数であるフィールド ([フィールド](classes.md#fields))。</span><span class="sxs-lookup"><span data-stu-id="be588-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="be588-405">メソッドで、計算とクラスによって実行できるアクションの実装 ([メソッド](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="be588-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="be588-406">プロパティを定義する特性をという名前と読み取りと書き込みのような特性に関連付けられているアクション ([プロパティ](classes.md#properties))。</span><span class="sxs-lookup"><span data-stu-id="be588-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="be588-407">クラスによって生成される通知を定義するには、イベント ([イベント](classes.md#events))。</span><span class="sxs-lookup"><span data-stu-id="be588-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="be588-408">同じ方法でインデックスを作成するクラスのインスタンスが許可されるインデクサーは、配列として (構文) ([インデクサー](classes.md#indexers))。</span><span class="sxs-lookup"><span data-stu-id="be588-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="be588-409">クラスのインスタンスに適用できる式の演算子の定義の演算子 ([演算子](classes.md#operators))。</span><span class="sxs-lookup"><span data-stu-id="be588-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="be588-410">インスタンス クラスのインスタンスを初期化するために必要なアクションを実装するコンス トラクター ([インスタンス コンス トラクター](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="be588-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="be588-411">デストラクターで、クラスのインスタンスが完全に破棄される前に実行されるアクションを実装 ([デストラクター](classes.md#destructors))。</span><span class="sxs-lookup"><span data-stu-id="be588-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="be588-412">クラス自体を初期化するために必要なアクションを実装する静的コンス トラクター ([静的コンス トラクター](classes.md#static-constructors))。</span><span class="sxs-lookup"><span data-stu-id="be588-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="be588-413">種類は、クラスに対してローカルな型を表す ([入れ子になった型](classes.md#nested-types))。</span><span class="sxs-lookup"><span data-stu-id="be588-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="be588-414">実行可能コードを含むことのできるメンバーと総称されます、*関数メンバー*のクラス型。</span><span class="sxs-lookup"><span data-stu-id="be588-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="be588-415">クラス型の関数メンバーは、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンス コンス トラクター、デストラクター、およびそのクラス型の静的コンス トラクターです。</span><span class="sxs-lookup"><span data-stu-id="be588-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="be588-416">A *class_declaration*新しい宣言領域を作成します ([宣言](basic-concepts.md#declarations))、および*class_member_declaration*ですぐに格納されている、*クラス_declaration*この宣言領域に新しいメンバーを導入します。</span><span class="sxs-lookup"><span data-stu-id="be588-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="be588-417">次の規則に適用されます*class_member_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="be588-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="be588-418">インスタンス コンス トラクター、デストラクター、および静的コンス トラクターは、すぐ外側のクラスと同じ名前が必要です。</span><span class="sxs-lookup"><span data-stu-id="be588-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="be588-419">その他のすべてのメンバーのすぐ外側のクラスの名前とは異なる名前が必要です。</span><span class="sxs-lookup"><span data-stu-id="be588-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="be588-420">定数、フィールド、プロパティ、イベント、または型の名前は、同じクラスで宣言されているその他のすべてのメンバーの名前とは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="be588-421">メソッドの名前は、他のすべての非-のメソッドの名前、同じクラスで宣言されているとは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="be588-422">さらに、署名 ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading)) のメソッドと同じクラスで宣言されている他のすべてのメソッドのシグネチャは異なるし、同じクラスで宣言されている 2 つの方法でのみ異なるシグネチャがない可能性がありますによって`ref`と`out`します。</span><span class="sxs-lookup"><span data-stu-id="be588-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="be588-423">インスタンス コンス トラクターのシグネチャは、同じクラスで宣言されている他のすべてのインスタンス コンス トラクターのシグネチャと異なる必要があり、同じクラスで宣言されている 2 つのコンス トラクターでのみが異なるシグネチャがない可能性があります`ref`と`out`します。</span><span class="sxs-lookup"><span data-stu-id="be588-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="be588-424">インデクサーのシグネチャは、同じクラスで宣言されている他のすべてのインデクサーの署名とは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="be588-425">演算子のシグネチャは、同じクラスで宣言されているその他のすべての演算子のシグネチャとは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="be588-426">クラス型の継承されたメンバー ([継承](classes.md#inheritance)) クラスの宣言領域の一部ではないです。</span><span class="sxs-lookup"><span data-stu-id="be588-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="be588-427">したがって、派生クラスは (これは有効で、継承されたメンバーを非表示になります)、継承されたメンバーと同じ名前またはシグネチャを持つメンバーを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="be588-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="be588-428">インスタンスの型</span><span class="sxs-lookup"><span data-stu-id="be588-428">The instance type</span></span>

<span data-ttu-id="be588-429">各クラスの宣言が関連付けられたバインドされた型 ([バインドされ、型がバインドされていない](types.md#bound-and-unbound-types))、***インスタンス タイプに***します。</span><span class="sxs-lookup"><span data-stu-id="be588-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="be588-430">構築された型を作成して、ジェネリック クラス宣言のインスタンスの型の形式が ([構築型](types.md#constructed-types))、型宣言からで指定された型の各引数が、対応する入力パラメーター。</span><span class="sxs-lookup"><span data-stu-id="be588-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="be588-431">インスタンスの型は、型パラメーターを使用するためにのみ使用できます、型パラメーターがスコープにあります。つまり、クラス宣言内で。</span><span class="sxs-lookup"><span data-stu-id="be588-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="be588-432">インスタンスの型の型は、`this`クラス宣言内で記述されたコード。</span><span class="sxs-lookup"><span data-stu-id="be588-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="be588-433">非ジェネリック クラス、インスタンスの型は、宣言されたクラスだけです。</span><span class="sxs-lookup"><span data-stu-id="be588-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="be588-434">インスタンスの型といくつかのクラス宣言を次に示します。</span><span class="sxs-lookup"><span data-stu-id="be588-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="be588-435">構築された型のメンバー</span><span class="sxs-lookup"><span data-stu-id="be588-435">Members of constructed types</span></span>

<span data-ttu-id="be588-436">構築された型の非継承メンバーが、それぞれに置き換えることによって取得した*type_parameter* 、対応するメンバーの宣言で*type_argument*構築された型のです。</span><span class="sxs-lookup"><span data-stu-id="be588-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="be588-437">切り替え処理は、型の宣言の意味に基づいており、単なるテキストの置換ではありません。</span><span class="sxs-lookup"><span data-stu-id="be588-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="be588-438">たとえば、ジェネリック クラス宣言について考えます。</span><span class="sxs-lookup"><span data-stu-id="be588-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="be588-439">構築された型`Gen<int[],IComparable<string>>`に次のメンバーがあります。</span><span class="sxs-lookup"><span data-stu-id="be588-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="be588-440">メンバーの型`a`ジェネリック クラス宣言で`Gen`は"の 2 次元配列`T`"ため、メンバーの型`a`"の1次元配列の2次元配列をは、上記の構築された型`int`"、または`int[,][]`します。</span><span class="sxs-lookup"><span data-stu-id="be588-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="be588-441">インスタンス関数メンバーの種類内`this`インスタンスの型は、([インスタンス型](classes.md#the-instance-type)) 含む宣言のです。</span><span class="sxs-lookup"><span data-stu-id="be588-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="be588-442">ジェネリック クラスのすべてのメンバーは、直接、または構築された型の一部として、外側のクラスからの型パラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="be588-443">構築された型の特定が閉じられたときに ([オープンとクローズ型](types.md#open-and-closed-types)) が使用される実行時には、型パラメーターの使用は、構築された型に指定された実際の型引数に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="be588-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="be588-444">例:</span><span class="sxs-lookup"><span data-stu-id="be588-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="be588-445">継承</span><span class="sxs-lookup"><span data-stu-id="be588-445">Inheritance</span></span>

<span data-ttu-id="be588-446">クラス***継承***直接基底クラス型のメンバー。</span><span class="sxs-lookup"><span data-stu-id="be588-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="be588-447">継承では、クラスが暗黙的にインスタンス コンス トラクター、デストラクター、および基底クラスの静的コンス トラクターを除く、直接基底クラス型のすべてのメンバーを格納することを意味します。</span><span class="sxs-lookup"><span data-stu-id="be588-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="be588-448">継承する場合は、いくつかの重要な側面は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="be588-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="be588-449">継承は推移的です。</span><span class="sxs-lookup"><span data-stu-id="be588-449">Inheritance is transitive.</span></span> <span data-ttu-id="be588-450">場合`C`から派生`B`、および`B`から派生`A`、し`C`で宣言されたメンバーを継承`B`で宣言されたメンバーと`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="be588-451">派生クラスでは、その直接の基本クラスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="be588-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="be588-452">派生クラスは、継承するメンバーに新しいメンバーを追加できますが、継承されたメンバーの定義を削除することはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="be588-453">インスタンス コンス トラクター、デストラクター、および静的コンス トラクターは継承されませんが、宣言されたアクセシビリティに関係なく、他のすべてのメンバーは、([メンバー アクセス](basic-concepts.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="be588-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="be588-454">ただし、によって宣言されたアクセシビリティ、継承されたメンバーができない派生クラスでアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="be588-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="be588-455">派生クラスは***を非表示に***([継承によって非表示](basic-concepts.md#hiding-through-inheritance)) 同じ名前またはシグネチャを持つ新しいメンバーを宣言することで、継承されたメンバー。</span><span class="sxs-lookup"><span data-stu-id="be588-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="be588-456">そのメンバーは削除継承されたメンバーが隠ぺいされてただしは、単に、そのメンバーは派生クラスから直接アクセスできません。</span><span class="sxs-lookup"><span data-stu-id="be588-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="be588-457">クラスのインスタンスには、一連クラスとその基本クラスでは、暗黙的な変換で宣言されているすべてのインスタンス フィールドにはが含まれています ([暗黙の参照変換](conversions.md#implicit-reference-conversions)) クラスの派生型からその基底クラス型のいずれかに存在します。</span><span class="sxs-lookup"><span data-stu-id="be588-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="be588-458">したがって、いくつかの派生クラスのインスタンスへの参照は、その基本クラスのいずれかのインスタンスへの参照として処理できます。</span><span class="sxs-lookup"><span data-stu-id="be588-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="be588-459">クラスは、仮想メソッド、プロパティ、およびインデクサーを宣言し、派生クラスは、これらの関数メンバーの実装をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="be588-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="be588-460">これにより、クラス メンバー関数の呼び出しによって実行されたアクションは、その関数のメンバーが呼び出されるインスタンスの実行時の型によって異なります、ポリモーフィックな動作が発生することができます。</span><span class="sxs-lookup"><span data-stu-id="be588-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="be588-461">構築済みクラス型の継承されたメンバーは、直接の基本クラス型のメンバー ([基底クラス](classes.md#base-classes))、対応する型の各インスタンスに対して構築された型の型引数を代入することによってこれが見つかった内のパラメーター、 *class_base*仕様。</span><span class="sxs-lookup"><span data-stu-id="be588-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="be588-462">さらに、これらのメンバーがそれぞれの置換によって変換されます*type_parameter* 、対応するメンバーの宣言で*type_argument*の*class_base*指定。</span><span class="sxs-lookup"><span data-stu-id="be588-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="be588-463">上記の例では、構築された型で`D<int>`非継承メンバーを持つ`public int G(string s)`型引数を代入することによって取得`int`、型パラメーターに対して`T`。</span><span class="sxs-lookup"><span data-stu-id="be588-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="be588-464">`D<int>` クラス宣言から継承されたメンバーがあります`B`します。</span><span class="sxs-lookup"><span data-stu-id="be588-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="be588-465">この継承されたメンバーを最初に基本クラスの型を決定することで決定`B<int[]>`の`D<int>`に置き換えることによって`int`の`T`基底クラスの指定で`B<T[]>`します。</span><span class="sxs-lookup"><span data-stu-id="be588-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="be588-466">次に、型引数として、 `B`、`int[]`を置き換える`U`で`public U F(long index)`、継承されたメンバーを生成`public int[] F(long index)`します。</span><span class="sxs-lookup"><span data-stu-id="be588-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="be588-467">New 修飾子</span><span class="sxs-lookup"><span data-stu-id="be588-467">The new modifier</span></span>

<span data-ttu-id="be588-468">A *class_member_declaration*継承されたメンバーと同じ名前またはシグネチャを持つメンバーを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="be588-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="be588-469">この場合、派生クラスのメンバーといいます***を非表示に***基底クラスのメンバー。</span><span class="sxs-lookup"><span data-stu-id="be588-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="be588-470">継承されたメンバーを非表示で、エラーはありませんが、コンパイラ警告を発し。</span><span class="sxs-lookup"><span data-stu-id="be588-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="be588-471">警告を抑制するには、派生クラスのメンバーの宣言を含めることができます、`new`修飾子を派生メンバーでは、基本メンバーを非表示にするものであるかを示します。</span><span class="sxs-lookup"><span data-stu-id="be588-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="be588-472">このトピックの説明でさらに[継承による隠ぺい](basic-concepts.md#hiding-through-inheritance)します。</span><span class="sxs-lookup"><span data-stu-id="be588-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="be588-473">場合、`new`継承されたメンバーを隠ぺいしない宣言に修飾子を含めると、それに対応する警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="be588-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="be588-474">削除することによってこの警告が抑制され、`new`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="be588-475">アクセス修飾子</span><span class="sxs-lookup"><span data-stu-id="be588-475">Access modifiers</span></span>

<span data-ttu-id="be588-476">A *class_member_declaration*宣言されたアクセシビリティの 5 つの可能な種類のいずれかを持つことができます ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility)): `public`、 `protected internal`、 `protected`、 `internal`、または`private`します。</span><span class="sxs-lookup"><span data-stu-id="be588-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="be588-477">を除き、`protected internal`を 1 つ以上のアクセス修飾子を指定すると、コンパイル エラーは、の組み合わせ。</span><span class="sxs-lookup"><span data-stu-id="be588-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="be588-478">ときに、 *class_member_declaration* 、アクセス修飾子を含まない`private`と見なされます。</span><span class="sxs-lookup"><span data-stu-id="be588-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="be588-479">構成要素型</span><span class="sxs-lookup"><span data-stu-id="be588-479">Constituent types</span></span>

<span data-ttu-id="be588-480">メンバーの宣言で使用される型は、そのメンバーの構成要素の型と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="be588-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="be588-481">使用可能な構成要素である型は、定数、フィールド、プロパティ、イベント、またはインデクサーの型、演算子、またはメソッドの戻り値の型およびメソッド、インデクサー、演算子、またはインスタンスのコンス トラクターのパラメーターの型。</span><span class="sxs-lookup"><span data-stu-id="be588-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="be588-482">メンバーの構成要素の型は、少なくともそのメンバー自体と同程度にアクセスである必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="be588-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="be588-483">静的メンバーとインスタンス メンバー</span><span class="sxs-lookup"><span data-stu-id="be588-483">Static and instance members</span></span>

<span data-ttu-id="be588-484">クラスのメンバーは、いずれかの***静的メンバー***または***インスタンス メンバー***します。</span><span class="sxs-lookup"><span data-stu-id="be588-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="be588-485">一般に、静的メンバーとしてクラス型とオブジェクト (クラス型のインスタンス) に属するものとしてインスタンス メンバーに属していると考えると便利です。</span><span class="sxs-lookup"><span data-stu-id="be588-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="be588-486">フィールド、メソッド、プロパティ、イベント、演算子、またはコンス トラクターの宣言が含まれています、`static`修飾子は、静的メンバーを宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="be588-487">さらに、定数または型の宣言には、静的メンバーを暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="be588-488">静的メンバーには、次の特性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="be588-489">静的メンバー`M`で言及されている、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`、`E`含まれる型を表す必要があります`M`します。</span><span class="sxs-lookup"><span data-stu-id="be588-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="be588-490">コンパイル時エラーには`E`インスタンスを表すとします。</span><span class="sxs-lookup"><span data-stu-id="be588-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="be588-491">静的フィールドは、閉じられた特定のクラス型のすべてのインスタンスで共有する 1 つだけの記憶域の場所を識別します。</span><span class="sxs-lookup"><span data-stu-id="be588-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="be588-492">閉じられた特定のクラス型のインスタンスの数が作成されては、静的フィールドの 1 つだけコピーします。</span><span class="sxs-lookup"><span data-stu-id="be588-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="be588-493">静的な関数メンバー (メソッド、プロパティ、イベント、演算子、またはコンス トラクター) が特定のインスタンスで動作しないを参照すると、コンパイル時エラー`this`でこのような関数メンバー。</span><span class="sxs-lookup"><span data-stu-id="be588-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="be588-494">フィールド、メソッド、プロパティ、イベント、インデクサー、コンス トラクターまたはデストラクターの宣言が含まれていない場合、`static`修飾子は、インスタンス メンバーを宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="be588-495">(インスタンス メンバーと呼ぶことが、非静的メンバーです。)インスタンス メンバーには、次の特性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="be588-496">インスタンス メンバーと`M`で言及されている、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`、 `E` を含む型のインスタンスを表す必要があります`M`.</span><span class="sxs-lookup"><span data-stu-id="be588-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="be588-497">バインディング時のエラーには`E`型を表すとします。</span><span class="sxs-lookup"><span data-stu-id="be588-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="be588-498">クラスのすべてのインスタンスには、別のクラスのすべてのインスタンス フィールドのセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="be588-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="be588-499">インスタンス関数メンバー (メソッド、プロパティ、インデクサー、インスタンス コンス トラクターまたはデストラクター) は、クラスの特定のインスタンスを操作し、としてこのインスタンスにアクセスできる`this`([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="be588-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="be588-500">次の例は、静的にアクセスするためのルールとインスタンス メンバーを示しています。</span><span class="sxs-lookup"><span data-stu-id="be588-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="be588-501">`F`メソッドは、インスタンス関数メンバーの表示を*simple_name* ([簡易名](expressions.md#simple-names)) インスタンスのメンバーと静的メンバーの両方にアクセスするために使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="be588-502">`G`メソッドは、静的な関数メンバーであるを通じてインスタンス メンバーにアクセスすると、コンパイル時エラーを表示、 *simple_name*します。</span><span class="sxs-lookup"><span data-stu-id="be588-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="be588-503">`Main`メソッドを示していますが、 *member_access* ([メンバー アクセス](expressions.md#member-access)) インスタンス、インスタンス メンバーにアクセスする必要があります、静的メンバーは、型を使ってアクセスする必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="be588-504">入れ子にされた型</span><span class="sxs-lookup"><span data-stu-id="be588-504">Nested types</span></span>

<span data-ttu-id="be588-505">クラスまたは構造体宣言内で宣言された型が呼び出される、***入れ子にされた型***します。</span><span class="sxs-lookup"><span data-stu-id="be588-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="be588-506">コンパイル単位または名前空間内で宣言された型が呼び出される、***に入れ子にされた型***します。</span><span class="sxs-lookup"><span data-stu-id="be588-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="be588-507">例</span><span class="sxs-lookup"><span data-stu-id="be588-507">In the example</span></span>
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
<span data-ttu-id="be588-508">クラス`B`クラス内で宣言されているため、入れ子にされた型は、 `A`、およびクラス`A`に入れ子にされた型は、コンパイル単位内で宣言されているためです。</span><span class="sxs-lookup"><span data-stu-id="be588-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="be588-509">完全修飾名</span><span class="sxs-lookup"><span data-stu-id="be588-509">Fully qualified name</span></span>

<span data-ttu-id="be588-510">完全修飾名 ([完全修飾名](basic-concepts.md#fully-qualified-names)) 入れ子にされた型は`S.N`場所`S`でどの種類の型の完全修飾名は、`N`は宣言されています。</span><span class="sxs-lookup"><span data-stu-id="be588-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="be588-511">アクセシビリティの宣言</span><span class="sxs-lookup"><span data-stu-id="be588-511">Declared accessibility</span></span>

<span data-ttu-id="be588-512">非入れ子にされた型を持つことができます`public`または`internal`アクセシビリティで宣言されていて、`internal`既定でアクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="be588-513">入れ子にされた型はさらにそれを含む型は、クラスまたは構造体かどうかに応じて、宣言されたアクセシビリティの 1 つまたは複数の形式を別途、これらの種類の宣言されたアクセシビリティを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="be588-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="be588-514">クラスで宣言されている入れ子にされた型が宣言されたアクセシビリティの 5 つの形式のいずれかを持つことができます (`public`、 `protected internal`、 `protected`、 `internal`、または`private`) と、他のクラス メンバーのように既定値は`private`宣言アクセシビリティ。</span><span class="sxs-lookup"><span data-stu-id="be588-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="be588-515">構造体で宣言されている入れ子にされた型が宣言されたアクセシビリティの 3 つの形式のいずれかを持つことができます (`public`、 `internal`、または`private`) と、他の構造体のメンバーのように既定値は`private`アクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="be588-516">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-516">The example</span></span>
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
<span data-ttu-id="be588-517">プライベートの入れ子になったクラスを宣言して`Node`します。</span><span class="sxs-lookup"><span data-stu-id="be588-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="be588-518">非表示</span><span class="sxs-lookup"><span data-stu-id="be588-518">Hiding</span></span>

<span data-ttu-id="be588-519">入れ子にされた型の非表示にすることがあります ([名前の隠ぺい](basic-concepts.md#name-hiding)) ベースのメンバー。</span><span class="sxs-lookup"><span data-stu-id="be588-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="be588-520">`new`修飾子が入れ子にされた型の宣言で許可されるため、非表示を明示的に表すことができます。</span><span class="sxs-lookup"><span data-stu-id="be588-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="be588-521">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-521">The example</span></span>
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
<span data-ttu-id="be588-522">入れ子になったクラスを示しています。`M`メソッドを非表示に`M`で定義されている`Base`します。</span><span class="sxs-lookup"><span data-stu-id="be588-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="be588-523">このアクセス</span><span class="sxs-lookup"><span data-stu-id="be588-523">this access</span></span>

<span data-ttu-id="be588-524">入れ子にされた型とそれを含む型にに関して特別なリレーションシップがありません*this_access* ([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="be588-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="be588-525">具体的には、`this`内での入れ子にされた型には使用できません、包含する型のインスタンス メンバーを参照してください。</span><span class="sxs-lookup"><span data-stu-id="be588-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="be588-526">入れ子にされた型がそれを含む型のインスタンス メンバーへのアクセスを必要がある場合、アクセスを提供することで提供できます、`this`の入れ子にされた型のコンス トラクター引数として含んでいる型のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="be588-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="be588-527">次の例</span><span class="sxs-lookup"><span data-stu-id="be588-527">The following example</span></span>
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
<span data-ttu-id="be588-528">この方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="be588-528">shows this technique.</span></span> <span data-ttu-id="be588-529">インスタンス`C`のインスタンスを作成します`Nested`独自渡します`this`に`Nested`のコンス トラクターへの後続のアクセスを提供するために`C`のインスタンス メンバーです。</span><span class="sxs-lookup"><span data-stu-id="be588-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="be588-530">含んでいる型のプライベート、プロテクト メンバーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="be588-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="be588-531">入れ子にされた型を持つ、包含する型のメンバーを含む、包含する型にアクセスできるメンバーのすべてにアクセスするが`private`と`protected`アクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="be588-532">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-532">The example</span></span>
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
<span data-ttu-id="be588-533">クラスを示しています`C`入れ子になったクラスを格納している`Nested`します。</span><span class="sxs-lookup"><span data-stu-id="be588-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="be588-534">内で`Nested`、メソッド`G`静的メソッドを呼び出して`F`で定義されている`C`、および`F`宣言されたアクセシビリティをプライベートにします。</span><span class="sxs-lookup"><span data-stu-id="be588-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="be588-535">入れ子にされた型は、それを含む型の基本型で定義されている保護されたメンバーもアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="be588-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="be588-536">例</span><span class="sxs-lookup"><span data-stu-id="be588-536">In the example</span></span>
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
<span data-ttu-id="be588-537">入れ子になったクラス`Derived.Nested`プロテクト メソッドにアクセスする`F`で定義されている`Derived`の基本クラス、`Base`によりのインスタンスを介して呼び出す`Derived`します。</span><span class="sxs-lookup"><span data-stu-id="be588-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="be588-538">ジェネリック クラスの入れ子にされた型</span><span class="sxs-lookup"><span data-stu-id="be588-538">Nested types in generic classes</span></span>

<span data-ttu-id="be588-539">ジェネリック クラス宣言は、入れ子になった型宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="be588-540">外側のクラスの型パラメーターは、入れ子にされた型で使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="be588-541">入れ子になった型宣言は、入れ子にされた型にのみ適用される追加の型パラメーターを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="be588-542">ジェネリック クラス宣言内に含まれるすべての型宣言は、暗黙的にジェネリック型の宣言です。</span><span class="sxs-lookup"><span data-stu-id="be588-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="be588-543">ジェネリック型の中で入れ子になった型への参照を書き込むときに、型引数を含む、コンテナーの構築された型が指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="be588-544">ただしから外側のクラス内で入れ子にされた型は修飾しないで使用できる;外側のクラスのインスタンスの型は、入れ子にされた型を構築するときに暗黙的に使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="be588-545">次の例は、作成から構築された型を参照する 3 つの正しい方法を示しています。 `Inner`; 最初の 2 つは等価です。</span><span class="sxs-lookup"><span data-stu-id="be588-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="be588-546">不適切なプログラミング スタイルでは、入れ子にされた型の型パラメーター メンバーを非表示にしたり、外側の型で宣言されたパラメーターを入力します。</span><span class="sxs-lookup"><span data-stu-id="be588-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="be588-547">予約済みのメンバー名</span><span class="sxs-lookup"><span data-stu-id="be588-547">Reserved member names</span></span>

<span data-ttu-id="be588-548">C# 実行時の基本的な実装をプロパティ、イベント、またはインデクサーでは、各ソース メンバー宣言を容易に実装は、メンバーの宣言、名前、およびその型の種類に基づく 2 つのメソッド シグネチャを予約する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="be588-549">基になるランタイムの実装を行わない場合でも、これらのいずれかに一致するシグネチャを持つメンバーには、署名が予約されています。 を宣言するプログラムのコンパイル時エラーがこれらの予約を使用します。</span><span class="sxs-lookup"><span data-stu-id="be588-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="be588-550">予約済みの名前は宣言されていない、したがって、メンバーの参照に参加していません。</span><span class="sxs-lookup"><span data-stu-id="be588-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="be588-551">ただし、宣言のシグネチャが継承に参加して予約済みのメソッドの関連 ([継承](classes.md#inheritance)) と非表示にできると、`new`修飾子 ([new 修飾子](classes.md#the-new-modifier))。</span><span class="sxs-lookup"><span data-stu-id="be588-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="be588-552">これらの名前の予約には、次の 3 つの目的があります。</span><span class="sxs-lookup"><span data-stu-id="be588-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="be588-553">Get のメソッド名として通常の識別子を使用するか、c# 言語機能にアクセスを設定するには、基になる実装をできるようにします。</span><span class="sxs-lookup"><span data-stu-id="be588-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="be588-554">その他の言語の get メソッド名として通常の識別子を使用して相互運用または c# 言語機能にアクセスを設定できるようにします。</span><span class="sxs-lookup"><span data-stu-id="be588-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="be588-555">ソースが 1 つの準拠コンパイラによって受け入れられるようにがによって許可される、ことで予約されているメンバーの仕様名一貫性のあるすべての c# 実装でします。</span><span class="sxs-lookup"><span data-stu-id="be588-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="be588-556">デストラクターの宣言 ([デストラクター](classes.md#destructors)) もシグネチャを予約できると、([デストラクター予約されているメンバーの名前](classes.md#member-names-reserved-for-destructors))。</span><span class="sxs-lookup"><span data-stu-id="be588-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="be588-557">プロパティ用に予約されているメンバー名</span><span class="sxs-lookup"><span data-stu-id="be588-557">Member names reserved for properties</span></span>

<span data-ttu-id="be588-558">プロパティの`P`([プロパティ](classes.md#properties)) 型の`T`、次のシグネチャは予約されています。</span><span class="sxs-lookup"><span data-stu-id="be588-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="be588-559">プロパティが読み取り専用または書き込み専用の場合でも、両方の署名が予約されています。</span><span class="sxs-lookup"><span data-stu-id="be588-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="be588-560">例</span><span class="sxs-lookup"><span data-stu-id="be588-560">In the example</span></span>
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
<span data-ttu-id="be588-561">クラス`A`読み取り専用プロパティを定義する`P`、シグネチャが予約`get_P`と`set_P`メソッド。</span><span class="sxs-lookup"><span data-stu-id="be588-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="be588-562">クラス`B`から派生した`A`両方のこれらの予約済みの署名を非表示にします。</span><span class="sxs-lookup"><span data-stu-id="be588-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="be588-563">この例では、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-563">The example produces the output:</span></span>
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="be588-564">イベント用に予約されているメンバー名</span><span class="sxs-lookup"><span data-stu-id="be588-564">Member names reserved for events</span></span>

<span data-ttu-id="be588-565">イベントの`E`([イベント](classes.md#events)) デリゲート型の`T`、次のシグネチャは予約されています。</span><span class="sxs-lookup"><span data-stu-id="be588-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="be588-566">インデクサー用に予約されているメンバー名</span><span class="sxs-lookup"><span data-stu-id="be588-566">Member names reserved for indexers</span></span>

<span data-ttu-id="be588-567">インデクサーの ([インデクサー](classes.md#indexers)) 型の`T`パラメーター リストで`L`、次のシグネチャは予約されています。</span><span class="sxs-lookup"><span data-stu-id="be588-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="be588-568">インデクサーは読み取り専用または書き込み専用で場合でも、両方の署名が予約されています。</span><span class="sxs-lookup"><span data-stu-id="be588-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="be588-569">さらに、メンバー名`Item`は予約されています。</span><span class="sxs-lookup"><span data-stu-id="be588-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="be588-570">デストラクターの予約済みのメンバー名</span><span class="sxs-lookup"><span data-stu-id="be588-570">Member names reserved for destructors</span></span>

<span data-ttu-id="be588-571">デストラクターを含むクラスの ([デストラクター](classes.md#destructors))、次のシグネチャは予約されています。</span><span class="sxs-lookup"><span data-stu-id="be588-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="be588-572">定数</span><span class="sxs-lookup"><span data-stu-id="be588-572">Constants</span></span>

<span data-ttu-id="be588-573">A***定数***定数値を表すクラスのメンバーである: コンパイル時に計算できる値。</span><span class="sxs-lookup"><span data-stu-id="be588-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="be588-574">A *constant_declaration*指定された型の 1 つまたは複数の定数が導入されています。</span><span class="sxs-lookup"><span data-stu-id="be588-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="be588-575">A *constant_declaration*のセットを含めることができます*属性*([属性](attributes.md))、`new`修飾子 ([new 修飾子](classes.md#the-new-modifier))、有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))。</span><span class="sxs-lookup"><span data-stu-id="be588-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="be588-576">属性と修飾子で宣言されたメンバーのすべてに適用されます、 *constant_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="be588-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="be588-577">定数は、静的メンバーと見なされる場合でも、 *constant_declaration*が必要し、は、どちらも、`static`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="be588-578">同じ修飾子を定数宣言内で複数回のエラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="be588-579">*型*の*constant_declaration*宣言によって導入されるメンバーの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="be588-580">型がのリストが続く*constant_declarator*s、新しいメンバーは、それぞれが導入されています。</span><span class="sxs-lookup"><span data-stu-id="be588-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="be588-581">A *constant_declarator*から成る、*識別子*後に、メンバーの名前を示す、"`=`"トークン、その後に、 *constant_expression* ([定数式](expressions.md#constant-expressions)) のメンバーの値を提供します。</span><span class="sxs-lookup"><span data-stu-id="be588-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="be588-582">*型*定数で指定された宣言である必要があります`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、`float`、 `double`、 `decimal`、 `bool`、 `string`、 *enum_type*、または*reference_type*します。</span><span class="sxs-lookup"><span data-stu-id="be588-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="be588-583">各*constant_expression*対象の型の場合、または暗黙的な変換によって対象の型に変換できる型の値を生成する必要があります ([暗黙的な変換](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="be588-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="be588-584">*型*定数は、少なくとも定数自体と同程度にアクセスする必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="be588-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="be588-585">定数の値が使用する式で取得した、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバー アクセス](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="be588-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="be588-586">定数できます自体の一部を*constant_expression*します。</span><span class="sxs-lookup"><span data-stu-id="be588-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="be588-587">したがってが必要な構成要素で定数を使用すること、 *constant_expression*します。</span><span class="sxs-lookup"><span data-stu-id="be588-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="be588-588">このような構造の例として、`case`ラベル、`goto case`ステートメント、`enum`メンバーの宣言、属性、およびその他の定数の宣言。</span><span class="sxs-lookup"><span data-stu-id="be588-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="be588-589">」の説明に従って[定数式](expressions.md#constant-expressions)、 *constant_expression*コンパイル時に完全に評価される式を指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="be588-590">以降の null 以外の値を作成する唯一の方法、 *reference_type*以外`string`が適用される、`new`演算子、しているので、`new`で演算子は使用できません、 *constant_式*の定数にのみ指定できる値*reference_type*以外 s`string`は`null`します。</span><span class="sxs-lookup"><span data-stu-id="be588-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="be588-591">定数値のシンボル名が必要なときにその値の型は、定数宣言で許可されていませんが、またはによってコンパイル時に値を計算できない場合、 *constant_expression*、 `readonly` (フィールド[読み取り専用フィールド](classes.md#readonly-fields)) 代わりに使用される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="be588-592">複数の定数を宣言する定数宣言では、属性、修飾子、および型の単一の定数の複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="be588-593">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="be588-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="be588-594">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="be588-595">定数は、循環依存関係がない限り、同じプログラム内の他の定数に依存して許可されます。</span><span class="sxs-lookup"><span data-stu-id="be588-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="be588-596">コンパイラは、適切な順序で定数の宣言を評価する自動的に配置します。</span><span class="sxs-lookup"><span data-stu-id="be588-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="be588-597">例</span><span class="sxs-lookup"><span data-stu-id="be588-597">In the example</span></span>
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
<span data-ttu-id="be588-598">コンパイラは最初に評価`A.Y`を評価し、 `B.Z`、最後に評価および`A.X`、値を生成`10`、`11`と`12`します。</span><span class="sxs-lookup"><span data-stu-id="be588-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="be588-599">定数宣言可能性があります、他のプログラムから定数によって異なりますが、このような依存関係は一方向にのみです。</span><span class="sxs-lookup"><span data-stu-id="be588-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="be588-600">場合に、上記の例を参照する`A`と`B`宣言されている別のプログラムは可能性があります`A.X`に依存する`B.Z`が`B.Z`に同時にではなく依存でしたし`A.Y`します。</span><span class="sxs-lookup"><span data-stu-id="be588-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="be588-601">フィールド</span><span class="sxs-lookup"><span data-stu-id="be588-601">Fields</span></span>

<span data-ttu-id="be588-602">A***フィールド***オブジェクトまたはクラスに関連付けられている変数を表すメンバーします。</span><span class="sxs-lookup"><span data-stu-id="be588-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="be588-603">A *field_declaration*指定された型の 1 つまたは複数のフィールドが導入されています。</span><span class="sxs-lookup"><span data-stu-id="be588-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="be588-604">A *field_declaration*のセットを含めることができます*属性*([属性](attributes.md))、`new`修飾子 ([new 修飾子](classes.md#the-new-modifier))、有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、および`static`修飾子 ([静的およびインスタンス フィールド](classes.md#static-and-instance-fields))。</span><span class="sxs-lookup"><span data-stu-id="be588-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="be588-605">さらに、 *field_declaration*含めることができます、`readonly`修飾子 ([読み取り専用フィールド](classes.md#readonly-fields)) または`volatile`修飾子 ([Volatile フィールド](classes.md#volatile-fields)) 両方ではないです。</span><span class="sxs-lookup"><span data-stu-id="be588-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="be588-606">属性と修飾子で宣言されたメンバーのすべてに適用されます、 *field_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="be588-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="be588-607">同じ修飾子をフィールド宣言内で複数回のエラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="be588-608">*型*の*field_declaration*宣言によって導入されるメンバーの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="be588-609">型がのリストが続く*variable_declarator*s、新しいメンバーは、それぞれが導入されています。</span><span class="sxs-lookup"><span data-stu-id="be588-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="be588-610">A *variable_declarator*から成る、*識別子*必要に応じて、そのメンバーの名前を示す、"`=`"トークンと*variable_initializer* ([変数の初期化子](classes.md#variable-initializers)) により、そのメンバーの初期値。</span><span class="sxs-lookup"><span data-stu-id="be588-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="be588-611">*型*フィールドは少なくともフィールド自体と同程度にアクセスできる必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="be588-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="be588-612">フィールドの値が使用する式で取得した、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバー アクセス](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="be588-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="be588-613">使用して読み取り専用以外のフィールドの値を変更、*割り当て*([代入演算子](expressions.md#assignment-operators))。</span><span class="sxs-lookup"><span data-stu-id="be588-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="be588-614">読み取り専用以外のフィールドの値が取得および変更を使用して後置インクリメントとデクリメント演算子を指定できます ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)) とは、前置インクリメントとデクリメント演算子 ([プレフィックスインクリメントおよびデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="be588-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="be588-615">複数のフィールドを宣言するフィールドの宣言は、属性、修飾子、および型の単一のフィールドの複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="be588-616">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="be588-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="be588-617">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="be588-618">静的およびインスタンスのフィールド</span><span class="sxs-lookup"><span data-stu-id="be588-618">Static and instance fields</span></span>

<span data-ttu-id="be588-619">フィールド宣言が含まれています、`static`修飾子、宣言によって導入されるフィールドは***静的フィールド***します。</span><span class="sxs-lookup"><span data-stu-id="be588-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="be588-620">ない場合`static`修飾子が存在する場合、フィールド、宣言によって導入される***インスタンス フィールド***します。</span><span class="sxs-lookup"><span data-stu-id="be588-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="be588-621">静的フィールドおよびインスタンス フィールドは、いくつかの種類の変数の 2 つ ([変数](variables.md)) で c# の場合は、サポートされているし、も参照されているとして***静的変数***と***インスタンス変数***、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="be588-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="be588-622">静的フィールドは、特定のインスタンスの一部ではありません。代わりに、クローズ型のすべてのインスタンス間で共有されます ([オープンとクローズ型](types.md#open-and-closed-types))。</span><span class="sxs-lookup"><span data-stu-id="be588-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="be588-623">閉じられたクラス型のインスタンスの数が作成されて、関連付けられているアプリケーション ドメインを静的フィールドの 1 つだけのコピーがあります。</span><span class="sxs-lookup"><span data-stu-id="be588-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="be588-624">例:</span><span class="sxs-lookup"><span data-stu-id="be588-624">For example:</span></span>
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

<span data-ttu-id="be588-625">インスタンス フィールドは、インスタンスに属しています。</span><span class="sxs-lookup"><span data-stu-id="be588-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="be588-626">具体的には、クラスのすべてのインスタンスには、そのクラスのすべてのインスタンス フィールドの個別のセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="be588-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="be588-627">フィールドが参照されている場合、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`場合は、`M`静的フィールド、`E`含まれる型を表す必要があります`M`、場合`M`インスタンス フィールドの場合は、電子メールに含まれる型のインスタンスを表す必要があります`M`します。</span><span class="sxs-lookup"><span data-stu-id="be588-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="be588-628">静的な違いは、インスタンス メンバーの説明とでさらに[静的メンバーとインスタンス メンバー](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="be588-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="be588-629">読み取り専用フィールド</span><span class="sxs-lookup"><span data-stu-id="be588-629">Readonly fields</span></span>

<span data-ttu-id="be588-630">ときに、 *field_declaration*が含まれています、`readonly`修飾子、宣言によって導入されるフィールドは***読み取り専用フィールド***します。</span><span class="sxs-lookup"><span data-stu-id="be588-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="be588-631">読み取り専用フィールドへの直接代入のみで、インスタンス コンス トラクターまたは同じクラスの静的コンス トラクターまたは宣言の一部として発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="be588-632">(読み取り専用フィールドはで割り当てられるを複数回これらのコンテキスト。)具体的には、直接割り当てを`readonly`フィールドは、次のコンテキストでのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="be588-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="be588-633">*Variable_declarator*フィールドを導入する (を含めることによって、 *variable_initializer*宣言内)。</span><span class="sxs-lookup"><span data-stu-id="be588-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="be588-634">インスタンス フィールドのフィールド宣言を含むクラスのインスタンス コンス トラクターで、静的フィールド、フィールド宣言を含むクラスの静的コンス トラクター内。</span><span class="sxs-lookup"><span data-stu-id="be588-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="be588-635">これらは、渡すコンテキストのみでも、`readonly`フィールドとして、`out`または`ref`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="be588-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="be588-636">割り当てる、`readonly`フィールドまたはとして渡す、`out`または`ref`他のコンテキストでのパラメーターは、コンパイル時エラー。</span><span class="sxs-lookup"><span data-stu-id="be588-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="be588-637">静的読み取り専用フィールドを使用して、定数</span><span class="sxs-lookup"><span data-stu-id="be588-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="be588-638">A`static readonly`フィールドが便利な定数値のシンボル名が必要な場合、値の型では使用できません、`const`宣言、コンパイル時に値を計算できない場合またはします。</span><span class="sxs-lookup"><span data-stu-id="be588-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="be588-639">例</span><span class="sxs-lookup"><span data-stu-id="be588-639">In the example</span></span>
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
<span data-ttu-id="be588-640">`Black`、 `White`、 `Red`、 `Green`、および`Blue`メンバーとして宣言できません`const`メンバーのため、コンパイル時にその値を計算することはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="be588-641">ただし、宣言する`static readonly`ほぼ同じ結果が代わりにします。</span><span class="sxs-lookup"><span data-stu-id="be588-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="be588-642">定数と静的読み取り専用フィールドのバージョン管理</span><span class="sxs-lookup"><span data-stu-id="be588-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="be588-643">定数と読み取り専用フィールドは、異なるバイナリのバージョン管理のセマンティクスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="be588-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="be588-644">コンパイル時に、定数の値を取得する式は、定数を参照する場合が、式は、読み取り専用フィールドを参照する場合、フィールドの値は実行時まで取得されません。</span><span class="sxs-lookup"><span data-stu-id="be588-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="be588-645">2 つのプログラムで構成されるアプリケーションを検討してください。</span><span class="sxs-lookup"><span data-stu-id="be588-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="be588-646">`Program1`と`Program2`名前空間が個別にコンパイルされる 2 つのプログラムを示します。</span><span class="sxs-lookup"><span data-stu-id="be588-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="be588-647">`Program1.Utils.X`によって値の出力は、静的読み取り専用フィールドとして宣言されている、`Console.WriteLine`ステートメントがコンパイル時に、不明はではなく、実行時に取得されます。</span><span class="sxs-lookup"><span data-stu-id="be588-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="be588-648">そのため場合の値`X`が変更されたと`Program1`が再コンパイルされる、`Console.WriteLine`ステートメントの出力は、新しい値いて`Program2`再コンパイルはありません。</span><span class="sxs-lookup"><span data-stu-id="be588-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="be588-649">ただし、 `X` 、定数の値`X`時に取得`Program2`がコンパイルされるとでの変更による影響を受けるに残る`Program1`まで`Program2`が再コンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="be588-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="be588-650">Volatile フィールド</span><span class="sxs-lookup"><span data-stu-id="be588-650">Volatile fields</span></span>

<span data-ttu-id="be588-651">ときに、 *field_declaration*が含まれています、`volatile`修飾子、宣言によって導入されるフィールドは***volatile フィールド***します。</span><span class="sxs-lookup"><span data-stu-id="be588-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="be588-652">非 volatile フィールドの場合は、命令を並べ替える最適化手法をによって提供される同期しないフィールドにアクセスするマルチ スレッド プログラムで、予期しない結果を生じることができます、 *lock_statement* ([Lock ステートメント](statements.md#the-lock-statement))。</span><span class="sxs-lookup"><span data-stu-id="be588-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="be588-653">コンパイラによって、実行時のシステムまたはハードウェアは、これらの最適化を実行できます。</span><span class="sxs-lookup"><span data-stu-id="be588-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="be588-654">Volatile フィールドは、このような並べ替えの最適化が制限されます。</span><span class="sxs-lookup"><span data-stu-id="be588-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="be588-655">Volatile フィールドの読み取りと呼ばれる、***の volatile 読み取り***します。</span><span class="sxs-lookup"><span data-stu-id="be588-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="be588-656">揮発性の読み取りが「セマンティクスを取得します。」つまり、メモリへの参照を命令シーケンスで、その後に発生する前に発生するが保証されます。</span><span class="sxs-lookup"><span data-stu-id="be588-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="be588-657">Volatile フィールドの書き込みと呼ばれる、 ***volatile 書き込み***します。</span><span class="sxs-lookup"><span data-stu-id="be588-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="be588-658">揮発性の書き込みが「セマンティクスをリリースします。」つまり、命令シーケンスで書き込み命令の前にメモリ参照の後に発生するが保証されます。</span><span class="sxs-lookup"><span data-stu-id="be588-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="be588-659">これらの制限により、すべてのスレッドが、他のスレッドによって実行された volatile の書き込みを、実行された順序で観察することが保証されます。</span><span class="sxs-lookup"><span data-stu-id="be588-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="be588-660">準拠した実装は、実行のすべてのスレッドから参照できる volatile 書き込みの順序付け単一合計を提供する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="be588-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="be588-661">Volatile フィールドの種類は、次のいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="be588-662">A *reference_type*します。</span><span class="sxs-lookup"><span data-stu-id="be588-662">A *reference_type*.</span></span>
*  <span data-ttu-id="be588-663">型`byte`、 `sbyte`、 `short`、 `ushort`、 `int`、 `uint`、 `char`、 `float`、 `bool`、 `System.IntPtr`、または` System.UIntPtr`します。</span><span class="sxs-lookup"><span data-stu-id="be588-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or` System.UIntPtr`.</span></span>
*  <span data-ttu-id="be588-664">*Enum_type*の基本型を列挙型を持つ`byte`、 `sbyte`、 `short`、 `ushort`、 `int`、または`uint`します。</span><span class="sxs-lookup"><span data-stu-id="be588-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="be588-665">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-665">The example</span></span>
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
<span data-ttu-id="be588-666">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="be588-666">produces the output:</span></span>
```
result = 143
```

<span data-ttu-id="be588-667">この例では、メソッドで`Main`メソッドを実行する新しいスレッドが開始`Thread2`します。</span><span class="sxs-lookup"><span data-stu-id="be588-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="be588-668">このメソッドは、非揮発性という名前のフィールドに値を格納`result`、し格納`true`volatile フィールドで`finished`します。</span><span class="sxs-lookup"><span data-stu-id="be588-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="be588-669">フィールドのメイン スレッドが待機する`finished`に設定する`true`、フィールドを読み取ります`result`します。</span><span class="sxs-lookup"><span data-stu-id="be588-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="be588-670">`finished`は宣言されています`volatile`、メイン スレッドが値を読み取る必要があります`143`フィールドから`result`します。</span><span class="sxs-lookup"><span data-stu-id="be588-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="be588-671">場合、フィールド`finished`宣言されなかった`volatile`にストアの許容されることになります`result`に格納した後、メイン スレッドに表示される`finished`、ため、値を読み取るメイン スレッドの`0`から、フィールド`result`します。</span><span class="sxs-lookup"><span data-stu-id="be588-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="be588-672">宣言する`finished`として、`volatile`フィールドは、このような不整合を防ぎます。</span><span class="sxs-lookup"><span data-stu-id="be588-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="be588-673">フィールドの初期化</span><span class="sxs-lookup"><span data-stu-id="be588-673">Field initialization</span></span>

<span data-ttu-id="be588-674">フィールドの初期値は静的フィールドまたはインスタンス フィールドであるかどうか、既定値 ([既定値](variables.md#default-values)) のフィールドの型。</span><span class="sxs-lookup"><span data-stu-id="be588-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="be588-675">この既定の初期化が発生し、フィールドはありません「初期化されていない」したがって前に、フィールドの値を観察することはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="be588-676">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-676">The example</span></span>
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
<span data-ttu-id="be588-677">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="be588-677">produces the output</span></span>
```
b = False, i = 0
```
<span data-ttu-id="be588-678">`b`と`i`はどちらも自動的に既定値に初期化します。</span><span class="sxs-lookup"><span data-stu-id="be588-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="be588-679">変数の初期化子</span><span class="sxs-lookup"><span data-stu-id="be588-679">Variable initializers</span></span>

<span data-ttu-id="be588-680">フィールドの宣言を含めることができます*variable_initializer*秒。</span><span class="sxs-lookup"><span data-stu-id="be588-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="be588-681">静的フィールドは、変数の初期化子はクラスの初期化中に実行された代入ステートメントに対応します。</span><span class="sxs-lookup"><span data-stu-id="be588-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="be588-682">インスタンス フィールド、変数初期化子に対応クラスのインスタンスが作成されるときに実行されるステートメントを代入します。</span><span class="sxs-lookup"><span data-stu-id="be588-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="be588-683">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-683">The example</span></span>
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
<span data-ttu-id="be588-684">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="be588-684">produces the output</span></span>
```
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="be588-685">への代入`x`が発生した静的フィールド初期化子の実行と、割り当てを`i`と`s`インスタンス フィールド初期化子の実行時に発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="be588-686">説明されている既定値の初期化[フィールドの初期化](classes.md#field-initialization)変数初期化子を持つフィールドを含め、すべてのフィールドに発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="be588-687">したがって、クラスが初期化されると、そのクラス内のすべての静的フィールドは、まず、既定値に初期化し、静的フィールド初期化子は、テキストの順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="be588-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="be588-688">同様に、クラスのインスタンスが作成されると、そのインスタンス内のすべてのインスタンス フィールドは、まず、既定値に初期化し、インスタンス フィールド初期化子がテキストの順序で実行します。</span><span class="sxs-lookup"><span data-stu-id="be588-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="be588-689">既定値の状態で見られる変数初期化子を持つ静的フィールドのことができます。</span><span class="sxs-lookup"><span data-stu-id="be588-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="be588-690">ただし、これは、スタイルの問題として強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="be588-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="be588-691">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-691">The example</span></span>
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
<span data-ttu-id="be588-692">この現象が発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-692">exhibits this behavior.</span></span> <span data-ttu-id="be588-693">循環定義に関係なく、b、プログラムが無効です。</span><span class="sxs-lookup"><span data-stu-id="be588-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="be588-694">出力になります</span><span class="sxs-lookup"><span data-stu-id="be588-694">It results in the output</span></span>
```
a = 1, b = 2
```
<span data-ttu-id="be588-695">ため、静的フィールド`a`と`b`に初期化されます`0`(の既定値`int`)、初期化子が実行される前にします。</span><span class="sxs-lookup"><span data-stu-id="be588-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="be588-696">ときに初期化子`a`の値を実行します。 `b` 0 の場合は、ので`a`に初期化されます`1`します。</span><span class="sxs-lookup"><span data-stu-id="be588-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="be588-697">ときに初期化子`b`の値を実行します。`a`は既に`1`、ので`b`に初期化されます`2`します。</span><span class="sxs-lookup"><span data-stu-id="be588-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="be588-698">静的フィールドの初期化</span><span class="sxs-lookup"><span data-stu-id="be588-698">Static field initialization</span></span>

<span data-ttu-id="be588-699">クラスの静的フィールドの変数初期化子は、一連のクラス宣言に表示されるテキストの順序で実行される割り当てに対応します。</span><span class="sxs-lookup"><span data-stu-id="be588-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="be588-700">場合、静的コンス トラクター ([静的コンス トラクター](classes.md#static-constructors)) が存在する静的フィールド初期化子の実行をクラスでは、その静的コンス トラクターを実行する前に、すぐに発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="be588-701">それ以外の場合、静的フィールド初期化子は、そのクラスの静的フィールドの最初に使用する前に、実装に依存する時に実行されます。</span><span class="sxs-lookup"><span data-stu-id="be588-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="be588-702">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-702">The example</span></span>
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
<span data-ttu-id="be588-703">いずれか、出力を生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-703">might produce either the output:</span></span>
```
Init A
Init B
1 1
```
<span data-ttu-id="be588-704">または、出力:</span><span class="sxs-lookup"><span data-stu-id="be588-704">or the output:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="be588-705">の実行`X`の初期化子と`Y`の初期化子は、任意の順序で発生する可能性があります。 これらのフィールドへの参照が発生する限定されます。</span><span class="sxs-lookup"><span data-stu-id="be588-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="be588-706">ただしでは、例を示します。</span><span class="sxs-lookup"><span data-stu-id="be588-706">However, in the example:</span></span>
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
<span data-ttu-id="be588-707">出力があります。</span><span class="sxs-lookup"><span data-stu-id="be588-707">the output must be:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="be588-708">ため、静的コンス トラクターが実行する場合の規則 (で定義されている[静的コンス トラクター](classes.md#static-constructors)) を提供`B`の静的コンス トラクター (したがって`B`の静的フィールド初期化子)の前に実行する必要があります`A`の静的コンス トラクター、フィールド初期化子。</span><span class="sxs-lookup"><span data-stu-id="be588-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="be588-709">インスタンス フィールドの初期化</span><span class="sxs-lookup"><span data-stu-id="be588-709">Instance field initialization</span></span>

<span data-ttu-id="be588-710">クラスのインスタンス フィールド変数初期化子の割り当て時にインスタンス コンス トラクターのいずれかのエントリにすぐに実行されるシーケンスに対応 ([コンス トラクター初期化子](classes.md#constructor-initializers)) そのクラスの。</span><span class="sxs-lookup"><span data-stu-id="be588-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="be588-711">変数の初期化子は、クラス宣言に表示されるテキストの順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="be588-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="be588-712">クラスのインスタンスの作成および初期化プロセスの詳細については[インスタンス コンス トラクター](classes.md#instance-constructors)します。</span><span class="sxs-lookup"><span data-stu-id="be588-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="be588-713">インスタンス フィールドの変数の初期化子は、作成中のインスタンスを参照できません。</span><span class="sxs-lookup"><span data-stu-id="be588-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="be588-714">したがって、参照すると、コンパイル時エラーが`this`変数の初期化子ではによる任意のインスタンス メンバーを参照する変数の初期化子のコンパイル時エラー、 *simple_name*します。</span><span class="sxs-lookup"><span data-stu-id="be588-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="be588-715">例</span><span class="sxs-lookup"><span data-stu-id="be588-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="be588-716">変数の初期化子`y`作成中のインスタンスのメンバーを参照しているため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="be588-717">メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-717">Methods</span></span>

<span data-ttu-id="be588-718">"***メソッド***" は、オブジェクトまたはクラスによって実行可能な計算またはアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="be588-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="be588-719">メソッドを使用して宣言*method_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="be588-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="be588-720">A *method_declaration*のセットを含めることができます*属性*([属性](attributes.md)) と有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、 `new` ([new 修飾子](classes.md#the-new-modifier))、 `static` ([静的およびインスタンス メソッド](classes.md#static-and-instance-methods))、 `virtual` ([仮想メソッド](classes.md#virtual-methods))、 `override` ([メソッドをオーバーライド](classes.md#override-methods))、 `sealed` ([メソッドをシール](classes.md#sealed-methods))、 `abstract` ([抽象メソッド](classes.md#abstract-methods))、および`extern` ([外部メソッド](classes.md#external-methods)) 修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="be588-721">宣言には、有効な修飾子の組み合わせは、次のすべてに該当する場合。</span><span class="sxs-lookup"><span data-stu-id="be588-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="be588-722">宣言には、有効なアクセス修飾子の組み合わせが含まれています ([アクセス修飾子](classes.md#access-modifiers))。</span><span class="sxs-lookup"><span data-stu-id="be588-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="be588-723">宣言は含まれません同じ修飾子は、複数回。</span><span class="sxs-lookup"><span data-stu-id="be588-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="be588-724">宣言では、次の修飾子の 1 つだけ含まれています: `static`、 `virtual`、および`override`します。</span><span class="sxs-lookup"><span data-stu-id="be588-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="be588-725">宣言では、次の修飾子の 1 つだけ含まれています:`new`と`override`します。</span><span class="sxs-lookup"><span data-stu-id="be588-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="be588-726">宣言が含まれている場合、`abstract`修飾子、宣言は次の修飾子のいずれかに含まれません: `static`、 `virtual`、`sealed`または`extern`します。</span><span class="sxs-lookup"><span data-stu-id="be588-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="be588-727">宣言が含まれている場合、`private`修飾子、宣言は次の修飾子のいずれかに含まれません: `virtual`、 `override`、または`abstract`します。</span><span class="sxs-lookup"><span data-stu-id="be588-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="be588-728">宣言が含まれている場合、`sealed`修飾子、宣言も含まれています、`override`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="be588-729">宣言が含まれている場合、`partial`修飾子は、その後は、次の修飾子のいずれかに含まれません: `new`、 `public`、 `protected`、 `internal`、 `private`、 `virtual`、 `sealed`、 `override`、 `abstract`、または`extern`します。</span><span class="sxs-lookup"><span data-stu-id="be588-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="be588-730">メソッドを持つ、`async`修飾子は、非同期関数でありで説明されている規則に従って[非同期関数](classes.md#async-functions)します。</span><span class="sxs-lookup"><span data-stu-id="be588-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="be588-731">*Return_type*メソッドの宣言が計算され、メソッドによって返される値の型を指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="be588-732">*Return_type*は`void`メソッドが値を返さない場合。</span><span class="sxs-lookup"><span data-stu-id="be588-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="be588-733">宣言が含まれている場合、`partial`修飾子は、その戻り値の型である必要があります`void`します。</span><span class="sxs-lookup"><span data-stu-id="be588-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="be588-734">*Member_name*メソッドの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="be588-735">メソッドが明示的なインターフェイス メンバーの実装でない限り ([明示的なインターフェイス メンバーの実装](interfaces.md#explicit-interface-member-implementations))、 *member_name*は単に、*識別子*します。</span><span class="sxs-lookup"><span data-stu-id="be588-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="be588-736">明示的なインターフェイス メンバーの実装、 *member_name*から成る、 *interface_type*後に、"`.`"と*識別子*します。</span><span class="sxs-lookup"><span data-stu-id="be588-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="be588-737">省略可能な*type_parameter_list*メソッドの型パラメーターを指定します ([パラメーター入力](classes.md#type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="be588-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="be588-738">場合、 *type_parameter_list*メソッドが指定されて、***ジェネリック メソッド***します。</span><span class="sxs-lookup"><span data-stu-id="be588-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="be588-739">メソッドの場合、`extern`修飾子、 *type_parameter_list*は指定できません。</span><span class="sxs-lookup"><span data-stu-id="be588-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="be588-740">省略可能な*formal_parameter_list*メソッドのパラメーターを指定します ([メソッド パラメーター](classes.md#method-parameters))。</span><span class="sxs-lookup"><span data-stu-id="be588-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="be588-741">省略可能な*type_parameter_constraints_clause*は個々 の型パラメーターの制約を指定する ([パラメーターの制約入力](classes.md#type-parameter-constraints)) 場合にのみ指定可能性があります、 *type_parameter_リスト*が指定されても、メソッドがないと、`override`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="be588-742">*Return_type*および各で参照される型の*formal_parameter_list*メソッドは少なくともメソッド自体と同程度にアクセスできる必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="be588-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="be588-743">*Method_body*がいずれかをセミコロン、***ステートメント本体***または***式本体***します。</span><span class="sxs-lookup"><span data-stu-id="be588-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="be588-744">ステートメント本体から成る、*ブロック*メソッドが呼び出されたときに実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="be588-745">式の本体から成る`=>`続けて、*式*およびセミコロンが、メソッドが呼び出されたときに実行する 1 つの式を表します。</span><span class="sxs-lookup"><span data-stu-id="be588-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="be588-746">`abstract`と`extern`、メソッド、 *method_body*セミコロンだけで構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="be588-747">`partial`メソッド、 *method_body*のセミコロン、ブロックの本体または式の本体のいずれかで構成されている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="be588-748">他のすべてのメソッドに対して、 *method_body*はブロック本体または式の本体。</span><span class="sxs-lookup"><span data-stu-id="be588-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="be588-749">場合、 *method_body* 、宣言が含まれていない可能性がありますし、セミコロンから成る、`async`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="be588-750">名前、型パラメーター リストおよびメソッドの仮パラメーター リストの定義、署名 ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading)) メソッドの。</span><span class="sxs-lookup"><span data-stu-id="be588-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="be588-751">具体的には、メソッドのシグネチャは、その名前、型パラメーターと数、修飾子、およびその仮パラメーターの型の数で構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="be588-752">このため、その名前ではなく、メソッドの型引数リストの序数位置を使用して、仮パラメーターの型で使用されるメソッドの型パラメーターが識別されます。戻り値の型はメソッドのシグネチャの一部でないでも、型パラメーターまたは仮パラメーターの名前です。</span><span class="sxs-lookup"><span data-stu-id="be588-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="be588-753">メソッドの名前は、他のすべての非-のメソッドの名前、同じクラスで宣言されているとは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="be588-754">さらに、メソッドのシグネチャは、同じクラスで宣言されている他のすべてのメソッドのシグネチャと異なる必要があり、同じクラスで宣言されている 2 つの方法でのみが異なるシグネチャがない可能性があります`ref`と`out`します。</span><span class="sxs-lookup"><span data-stu-id="be588-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="be588-755">メソッドの*type_parameter*全体のスコープでは、 *method_declaration*でそのスコープ全体でフォームの種類に使用できると*return_type*、 *method_body*、および*type_parameter_constraints_clause*s ではなく*属性*します。</span><span class="sxs-lookup"><span data-stu-id="be588-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="be588-756">すべての仮パラメーターと型パラメーターには、異なる名前がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="be588-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="be588-757">メソッドのパラメーター</span><span class="sxs-lookup"><span data-stu-id="be588-757">Method parameters</span></span>

<span data-ttu-id="be588-758">メソッドのパラメーターは、存在する場合、メソッドの宣言されます*formal_parameter_list*します。</span><span class="sxs-lookup"><span data-stu-id="be588-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="be588-759">1 つまたは複数コンマ区切りのパラメーターのうち、最後にのみがあります、仮パラメーター リストで構成されます、*括ら*します。</span><span class="sxs-lookup"><span data-stu-id="be588-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="be588-760">A *fixed_parameter*オプションのセットから成る*属性*([属性](attributes.md))、省略可能な`ref`、`out`または`this`修飾子、*型*、*識別子*と省略可能な*default_argument*します。</span><span class="sxs-lookup"><span data-stu-id="be588-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="be588-761">各*fixed_parameter*指定した名前で指定された型のパラメーターを宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="be588-762">`this`修飾子は、拡張メソッドとしてメソッドを指定しは静的メソッドの最初のパラメーターでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="be588-763">拡張メソッドはさらに「[拡張メソッド](classes.md#extension-methods)します。</span><span class="sxs-lookup"><span data-stu-id="be588-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="be588-764">A *fixed_parameter*で、 *default_argument*と呼ばれますが、***省略可能なパラメーター***であるのに対し、 *fixed_parameter* なし*default_argument*は、***必須パラメーター***します。</span><span class="sxs-lookup"><span data-stu-id="be588-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="be588-765">省略可能なパラメーターの後に必要なパラメーターが表示されない、 *formal_parameter_list*します。</span><span class="sxs-lookup"><span data-stu-id="be588-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="be588-766">A`ref`または`out`パラメーターを含めることはできません、 *default_argument*します。</span><span class="sxs-lookup"><span data-stu-id="be588-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="be588-767">*式*で、 *default_argument*次のいずれかを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="be588-768">*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="be588-768">a *constant_expression*</span></span>
*  <span data-ttu-id="be588-769">式形式の`new S()`場所`S`は値型です。</span><span class="sxs-lookup"><span data-stu-id="be588-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="be588-770">式形式の`default(S)`場所`S`は値型です。</span><span class="sxs-lookup"><span data-stu-id="be588-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="be588-771">*式*id 列またはパラメーターの型を null 許容型の変換によって暗黙的に変換できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="be588-772">部分メソッドの実装宣言で省略可能なパラメーターが発生するかどうか ([部分メソッド](classes.md#partial-methods))、明示的なインターフェイス メンバーの実装 ([明示的なインターフェイス メンバーの実装](interfaces.md#explicit-interface-member-implementations)) または、パラメーターを 1 つのインデクサーの宣言 ([インデクサー](classes.md#indexers)) これらのメンバーは、引数を省略を許可する方法では呼び出されませんので、コンパイラは警告を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="be588-773">A*括ら*オプションのセットから成る*属性*([属性](attributes.md))、`params`修飾子、 *array_type*、および*識別子*します。</span><span class="sxs-lookup"><span data-stu-id="be588-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="be588-774">パラメーター配列は、指定した名前で指定された配列型の 1 つのパラメーターを宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="be588-775">*Array_type*配列パラメーターの 1 次元配列型である必要があります ([配列型](arrays.md#array-types))。</span><span class="sxs-lookup"><span data-stu-id="be588-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="be588-776">メソッドの呼び出しでは、パラメーター配列を指定するには、指定された配列型の 1 つの引数を許可するか、0 個以上の引数を指定する配列要素型のようになります。</span><span class="sxs-lookup"><span data-stu-id="be588-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="be588-777">パラメーター配列の詳細については[パラメーター配列](classes.md#parameter-arrays)します。</span><span class="sxs-lookup"><span data-stu-id="be588-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="be588-778">A*括ら*、オプションのパラメーターの後に発生する可能性がありますが、既定値は--の引数の省略を含めることはできません、*括ら*空の配列の作成時に代わりになります。</span><span class="sxs-lookup"><span data-stu-id="be588-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="be588-779">次の例は、さまざまな種類のパラメーターを示しています。</span><span class="sxs-lookup"><span data-stu-id="be588-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="be588-780">*Formal_parameter_list*の`M`、`i`必要な ref パラメーターでは、`d`が必要な値を持つパラメーター、 `b`、 `s`、`o`と`t`パラメーターが省略可能な値と`a`はパラメーター配列です。</span><span class="sxs-lookup"><span data-stu-id="be588-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="be588-781">メソッドの宣言では、パラメーター、型パラメーターとローカル変数を別の宣言領域を作成します。</span><span class="sxs-lookup"><span data-stu-id="be588-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="be588-782">ローカル変数宣言と型パラメーター リストと、メソッドの仮パラメーター リストで、この宣言領域に名前が導入された、*ブロック*メソッドの。</span><span class="sxs-lookup"><span data-stu-id="be588-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="be588-783">メソッドの宣言領域に同じ名前の 2 つのメンバーのエラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="be588-784">メソッドの宣言領域と同じ名前の要素を格納する入れ子になった宣言領域のローカル変数の宣言領域のエラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="be588-785">メソッドの呼び出し ([メソッドの呼び出し](expressions.md#method-invocations)) その呼び出しに固有のコピーが作成、仮パラメーターと、メソッドのローカル変数と呼び出しの引数リストの値または変数の参照に割り当てられます、仮パラメーターを新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="be588-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="be588-786">内で、*ブロック*でそれらの識別子で、メソッドの仮パラメーターを参照できる*simple_name*式 ([簡易名](expressions.md#simple-names))。</span><span class="sxs-lookup"><span data-stu-id="be588-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="be588-787">仮パラメーターの 4 つの種類があります。</span><span class="sxs-lookup"><span data-stu-id="be588-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="be588-788">値パラメーターは、修飾子なしで宣言されます。</span><span class="sxs-lookup"><span data-stu-id="be588-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="be588-789">宣言されるパラメーターを参照して、`ref`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="be588-790">宣言される出力パラメーター、`out`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="be588-791">宣言されるパラメーター配列、`params`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="be588-792">」の説明に従って[シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading)、`ref`と`out`修飾子はメソッドのシグネチャの一部が、`params`修飾子はありません。</span><span class="sxs-lookup"><span data-stu-id="be588-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="be588-793">値パラメーター</span><span class="sxs-lookup"><span data-stu-id="be588-793">Value parameters</span></span>

<span data-ttu-id="be588-794">修飾子なしで宣言されたパラメーターは、値パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="be588-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="be588-795">値を持つパラメーターは、メソッドの呼び出しで指定された対応する引数からその初期値を取得するローカル変数に対応します。</span><span class="sxs-lookup"><span data-stu-id="be588-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="be588-796">メソッドの呼び出しで、対応する引数が暗黙的に変換できる式を指定する必要があります仮パラメーターに値を持つパラメーターがある場合 ([暗黙的な変換](conversions.md#implicit-conversions)) 仮パラメーターの型にします。</span><span class="sxs-lookup"><span data-stu-id="be588-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="be588-797">値を持つパラメーターに新しい値を割り当てるメソッドが許可されます。</span><span class="sxs-lookup"><span data-stu-id="be588-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="be588-798">このような割り当て、value パラメーターで表されるローカル ストレージの場所にのみ影響: メソッドの呼び出しで指定された実際の引数には影響があるありません。</span><span class="sxs-lookup"><span data-stu-id="be588-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="be588-799">参照パラメーター</span><span class="sxs-lookup"><span data-stu-id="be588-799">Reference parameters</span></span>

<span data-ttu-id="be588-800">宣言したパラメーターを`ref`修飾子は、参照パラメーター。</span><span class="sxs-lookup"><span data-stu-id="be588-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="be588-801">値パラメーターの場合とは異なり、参照パラメーターは、新しい記憶域の場所を作成できません。</span><span class="sxs-lookup"><span data-stu-id="be588-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="be588-802">代わりに、参照パラメーターは、メソッドの呼び出しで引数として指定された変数と同じストレージ場所を表します。</span><span class="sxs-lookup"><span data-stu-id="be588-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="be588-803">メソッドの呼び出しで対応する引数がキーワードで構成される必要があります仮パラメーターは、参照パラメーターが、`ref`続けて、 *variable_reference* ([を決定するための厳密な規則確実な代入](variables.md#precise-rules-for-determining-definite-assignment)) の仮パラメーターと同じ型。</span><span class="sxs-lookup"><span data-stu-id="be588-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="be588-804">参照パラメーターとして渡す前に、変数を間違いなく割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="be588-805">メソッド内の参照パラメーターは常と見なされます明示的に代入します。</span><span class="sxs-lookup"><span data-stu-id="be588-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="be588-806">反復子として宣言されたメソッド ([反復子](classes.md#iterators)) 参照パラメーターを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="be588-807">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-807">The example</span></span>
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
<span data-ttu-id="be588-808">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="be588-808">produces the output</span></span>
```
i = 2, j = 1
```

<span data-ttu-id="be588-809">呼び出しの`Swap`で`Main`、`x`表します`i`と`y`表します`j`。</span><span class="sxs-lookup"><span data-stu-id="be588-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="be588-810">このため、呼び出しは、の値を交換`i`と`j`します。</span><span class="sxs-lookup"><span data-stu-id="be588-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="be588-811">メソッドでは複数の名前が同じ記憶域の場所を表すことが参照パラメーターを受け取る。</span><span class="sxs-lookup"><span data-stu-id="be588-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="be588-812">例</span><span class="sxs-lookup"><span data-stu-id="be588-812">In the example</span></span>
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
<span data-ttu-id="be588-813">呼び出し`F`で`G`への参照を渡す`s`両方の`a`と`b`します。</span><span class="sxs-lookup"><span data-stu-id="be588-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="be588-814">したがって、その呼び出しでは、名前の`s`、 `a`、および`b`すべて、同じストレージの場所を参照し、すべての 3 つの割り当ては、インスタンス フィールドを変更`s`します。</span><span class="sxs-lookup"><span data-stu-id="be588-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="be588-815">出力パラメーター</span><span class="sxs-lookup"><span data-stu-id="be588-815">Output parameters</span></span>

<span data-ttu-id="be588-816">宣言したパラメーター、`out`修飾子は、出力パラメーター。</span><span class="sxs-lookup"><span data-stu-id="be588-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="be588-817">参照パラメーターと同様に、出力パラメーターには新しい記憶域の場所は作成できません。</span><span class="sxs-lookup"><span data-stu-id="be588-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="be588-818">代わりに、出力パラメーターは、メソッドの呼び出しで引数として指定された変数と同じストレージ場所を表します。</span><span class="sxs-lookup"><span data-stu-id="be588-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="be588-819">メソッドの呼び出しで対応する引数がキーワードで構成される必要があります仮パラメーターが出力パラメーターの場合は、`out`続けて、 *variable_reference* ([を決定するための厳密な規則確実な代入](variables.md#precise-rules-for-determining-definite-assignment)) の仮パラメーターと同じ型。</span><span class="sxs-lookup"><span data-stu-id="be588-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="be588-820">変数必要がありますいないに明示的に代入、出力パラメーターとして渡すことが、次の変数が出力パラメーターとして渡された呼び出し、変数と見なされます明示的に代入します。</span><span class="sxs-lookup"><span data-stu-id="be588-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="be588-821">メソッド内と同じように、ローカル変数、出力パラメーターは最初と見なされる未割り当ての値を使用する前に間違いなく割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="be588-822">メソッドが戻る前に、メソッドのすべての出力パラメーターを間違いなく割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="be588-823">部分メソッドとして宣言されたメソッド ([部分メソッド](classes.md#partial-methods)) または反復子 ([反復子](classes.md#iterators)) パラメーターの出力があることはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="be588-824">出力パラメーターは、複数の戻り値を生成するメソッドで通常使用されます。</span><span class="sxs-lookup"><span data-stu-id="be588-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="be588-825">例:</span><span class="sxs-lookup"><span data-stu-id="be588-825">For example:</span></span>
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

<span data-ttu-id="be588-826">この例では、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-826">The example produces the output:</span></span>
```
c:\Windows\System\
hello.txt
```

<span data-ttu-id="be588-827">なお、`dir`と`name`に渡す前に変数が割り当て済みにすることができます`SplitPath`、いると見なされる次の呼び出しが明示的に代入するとします。</span><span class="sxs-lookup"><span data-stu-id="be588-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="be588-828">パラメーター配列</span><span class="sxs-lookup"><span data-stu-id="be588-828">Parameter arrays</span></span>

<span data-ttu-id="be588-829">宣言したパラメーターを`params`修飾子は、パラメーター配列。</span><span class="sxs-lookup"><span data-stu-id="be588-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="be588-830">仮パラメーター リストには、パラメーター配列が含まれている場合、一覧の最後のパラメーターがあり、1 次元配列型でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="be588-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="be588-831">たとえば、型`string[]`と`string[][]`パラメーター配列の型が、型として使用できる`string[,]`ことはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="be588-832">結合することはできません、`params`修飾子を使用して修飾子`ref`と`out`します。</span><span class="sxs-lookup"><span data-stu-id="be588-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="be588-833">パラメーター配列は、メソッドの呼び出しで 2 つの方法のいずれかで指定する引数を許可します。</span><span class="sxs-lookup"><span data-stu-id="be588-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="be588-834">引数の指定されたパラメーター配列は、暗黙的に変換する 1 つの式を指定できます ([暗黙的な変換](conversions.md#implicit-conversions)) に、パラメーター配列の型。</span><span class="sxs-lookup"><span data-stu-id="be588-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="be588-835">この場合、パラメーター配列は、値を持つパラメーターとまったく同じように機能します。</span><span class="sxs-lookup"><span data-stu-id="be588-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="be588-836">または、呼び出しはそれぞれの引数が、暗黙的に変換する式は、パラメーター配列の 0 個以上の引数を指定できます ([暗黙的な変換](conversions.md#implicit-conversions)) パラメーターの配列の要素の型にします。</span><span class="sxs-lookup"><span data-stu-id="be588-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="be588-837">ここでは、呼び出し引数の数に対応する長さを持つパラメーターの配列型のインスタンスを作成します。 指定された引数に値を使用して、配列インスタンスの要素を初期化し、実際に新しく作成された配列のインスタンスを使用引数。</span><span class="sxs-lookup"><span data-stu-id="be588-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="be588-838">を除き、呼び出しの可変個の引数を許可すると、パラメーター配列に値を持つパラメーターとまったく同じです ([パラメーターの値](classes.md#value-parameters)) 同じ型。</span><span class="sxs-lookup"><span data-stu-id="be588-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="be588-839">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-839">The example</span></span>
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
<span data-ttu-id="be588-840">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="be588-840">produces the output</span></span>
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="be588-841">最初の呼び出し`F`配列を渡すだけ`a`値パラメーターとして。</span><span class="sxs-lookup"><span data-stu-id="be588-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="be588-842">2 番目の呼び出しの`F`4 つの要素が自動的に作成`int[]`の特定の要素の値と配列のインスタンスの値を持つパラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="be588-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="be588-843">3 つ目の呼び出しでは同様に、 `F` 0 要素を作成します`int[]`値パラメーターとしてそのインスタンスを渡します。</span><span class="sxs-lookup"><span data-stu-id="be588-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="be588-844">2 番目と 3 番目の呼び出しは、書き込みを正確に同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="be588-845">パラメーター配列を持つメソッドを標準形式で、または拡張形式では、適用可能なオーバー ロードの解決を実行するときに ([適用可能な関数メンバー](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="be588-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="be588-846">メソッドの拡張の形式は、通常、メソッドの形式が適用されない場合にのみ、および拡張形式と同じシグネチャを持つ、該当するメソッドが同じ型で既に宣言されていない場合にのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="be588-847">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-847">The example</span></span>
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
<span data-ttu-id="be588-848">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="be588-848">produces the output</span></span>
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="be588-849">例では、パラメーター配列を持つメソッドの拡張可能な形式の 2 つ既に含まれているクラスの通常のメソッドとして。</span><span class="sxs-lookup"><span data-stu-id="be588-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="be588-850">そのため、これらの拡張形式はオーバー ロードの解決を実行するときに考慮されませんし、最初と 3 番目のメソッドの呼び出しでは、通常のメソッドが選択します。</span><span class="sxs-lookup"><span data-stu-id="be588-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="be588-851">クラスは、パラメーター配列を持つメソッドを宣言して、それはも通常のメソッドとして展開されたフォームの一部を含めるには珍しくありません。</span><span class="sxs-lookup"><span data-stu-id="be588-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="be588-852">により、配列の割り当てを回避することは、パラメーター配列を持つメソッドの拡張の形式のときに発生するインスタンスが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="be588-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="be588-853">パラメーター配列の型の場合は`object[]`、通常、メソッドの形式と単一の拡張形式の間で発生する潜在的なあいまいさ`object`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="be588-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="be588-854">あいまいさの理由は、`object[]`型に暗黙的に変換自体が`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="be588-855">あいまいさも問題はありません、ただし、必要な場合は、キャストを挿入することで解決できるためです。</span><span class="sxs-lookup"><span data-stu-id="be588-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="be588-856">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-856">The example</span></span>
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
<span data-ttu-id="be588-857">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="be588-857">produces the output</span></span>
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="be588-858">最初と最後の呼び出しで`F`、通常の形式`F`はパラメーターの型に引数の型からの暗黙的な変換が存在するために適用 (型の両方が`object[]`)。</span><span class="sxs-lookup"><span data-stu-id="be588-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="be588-859">したがって、オーバー ロードの解決は通常の形式を選択します。 `F`、通常の値のパラメーターとして渡される引数、と。</span><span class="sxs-lookup"><span data-stu-id="be588-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="be588-860">2 番目と 3 番目の呼び出し、通常の形式で`F`暗黙的な変換が存在しないため、引数の型からパラメーターの型には適用できません (型`object`型に暗黙的に変換できない`object[]`)。</span><span class="sxs-lookup"><span data-stu-id="be588-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="be588-861">ただし、拡張されたフォームの`F`はオーバー ロードの解決で選択されているので、該当します。</span><span class="sxs-lookup"><span data-stu-id="be588-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="be588-862">その結果、1 つの要素`object[]`、呼び出しによって作成し、配列の 1 つの要素が指定された引数の値で初期化されます (への参照をそれ自体が、 `object[]`)。</span><span class="sxs-lookup"><span data-stu-id="be588-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="be588-863">静的メソッドとインスタンス メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-863">Static and instance methods</span></span>

<span data-ttu-id="be588-864">メソッドの宣言が含まれています、`static`修飾子、メソッドは静的メソッドをいいます。</span><span class="sxs-lookup"><span data-stu-id="be588-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="be588-865">ない場合`static`修飾子が存在する、メソッドはインスタンス メソッドと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="be588-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="be588-866">静的メソッドは、特定のインスタンスでは動作せずを参照すると、コンパイル時エラー`this`で静的メソッド。</span><span class="sxs-lookup"><span data-stu-id="be588-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="be588-867">インスタンス メソッドは、クラスの特定のインスタンスを操作し、そのインスタンスとしてアクセスできます`this`([このアクセス](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="be588-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="be588-868">メソッドが参照されている場合、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`場合は、`M`静的メソッドでは、 `E` を含む型を表す必要があります`M`、場合`M`、インスタンス メソッドである`E`含まれる型のインスタンスを表す必要があります`M`します。</span><span class="sxs-lookup"><span data-stu-id="be588-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="be588-869">静的な違いは、インスタンス メンバーの説明とでさらに[静的メンバーとインスタンス メンバー](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="be588-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="be588-870">仮想メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-870">Virtual methods</span></span>

<span data-ttu-id="be588-871">インスタンス メソッドの宣言が含まれています、`virtual`修飾子、メソッドは仮想メソッドをいいます。</span><span class="sxs-lookup"><span data-stu-id="be588-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="be588-872">ない場合`virtual`修飾子が存在する、メソッドは、非仮想メソッドと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="be588-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="be588-873">非仮想メソッドの実装は、バリアントではありません。内のクラスのインスタンスでメソッドが呼び出されるかどうか、実装は同じ宣言されているか、派生クラスのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="be588-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="be588-874">これに対し、仮想メソッドの実装は、派生クラスで置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="be588-875">継承された仮想メソッドの実装に取り替えるのプロセスと呼びます***オーバーライド***メソッド ([メソッドをオーバーライド](classes.md#override-methods))。</span><span class="sxs-lookup"><span data-stu-id="be588-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="be588-876">仮想メソッドの呼び出しで、***実行時の型***その呼び出しが対象のインスタンスの場所は、実際のメソッドの実装を呼び出すを決定します。</span><span class="sxs-lookup"><span data-stu-id="be588-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="be588-877">非仮想メソッドの呼び出しで、***コンパイル時の型***インスタンスの決定要因は、します。</span><span class="sxs-lookup"><span data-stu-id="be588-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="be588-878">正確には、メソッドがという名前で`N`が呼び出されると、引数リスト`A`コンパイル時の型を持つインスタンスで`C`と実行時の型`R`(場所`R`か`C`派生したクラスまたは`C`)、呼び出しは次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="be588-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="be588-879">オーバー ロードの解決を適用する最初に、 `C`、 `N`、および`A`、特定の方法を選択する`M`で宣言および継承されるメソッドのセットから`C`します。</span><span class="sxs-lookup"><span data-stu-id="be588-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="be588-880">「[メソッドの呼び出し](expressions.md#method-invocations)します。</span><span class="sxs-lookup"><span data-stu-id="be588-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="be588-881">その後、if`M`非仮想メソッドでは、`M`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="be588-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="be588-882">それ以外の場合、`M`は仮想メソッド、および、最派生実装の`M`を`R`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="be588-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="be588-883">宣言またはクラスによって継承されている仮想メソッドのそれぞれについて、存在、***最も派生実装***そのクラスに対してメソッドの。</span><span class="sxs-lookup"><span data-stu-id="be588-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="be588-884">仮想メソッドの最多派生実装`M`クラスに関して`R`は次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="be588-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="be588-885">場合`R`、導入が含まれています。`virtual`の宣言`M`、これは、最派生実装の`M`します。</span><span class="sxs-lookup"><span data-stu-id="be588-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="be588-886">の場合`R`が含まれています、`override`の`M`、これは、最派生実装の`M`します。</span><span class="sxs-lookup"><span data-stu-id="be588-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="be588-887">それ以外の場合の実装の派生を最大限に`M`を`R`は、最派生実装のと同じ`M`の直接の基本クラスに対して`R`。</span><span class="sxs-lookup"><span data-stu-id="be588-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="be588-888">次の例は、仮想および非仮想メソッドの違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="be588-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="be588-889">例では、`A`非仮想メソッドを導入`F`と仮想メソッド`G`します。</span><span class="sxs-lookup"><span data-stu-id="be588-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="be588-890">クラスは、`B`新しい非仮想メソッドが導入されています`F`、そのため、継承を非表示`F`、継承されたメソッドもオーバーライドと`G`します。</span><span class="sxs-lookup"><span data-stu-id="be588-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="be588-891">この例では、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-891">The example produces the output:</span></span>
```
A.F
B.F
B.G
B.G
```

<span data-ttu-id="be588-892">注意ステートメント`a.G()`呼び出す`B.G`ではなく、`A.G`します。</span><span class="sxs-lookup"><span data-stu-id="be588-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="be588-893">これは、実行時の型のインスタンスのためです (これは`B`)、インスタンスのコンパイル時型ではありません (これは`A`) を呼び出す実際のメソッドの実装を決定します。</span><span class="sxs-lookup"><span data-stu-id="be588-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="be588-894">継承されたメソッドを非表示には、メソッドが許可されている、ために、同じシグネチャを持つ複数の仮想メソッドを含むクラス可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="be588-895">最派生メソッドを除くすべてが非表示のため、あいまいさの問題は発生このしません。</span><span class="sxs-lookup"><span data-stu-id="be588-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="be588-896">例</span><span class="sxs-lookup"><span data-stu-id="be588-896">In the example</span></span>
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
<span data-ttu-id="be588-897">`C`と`D`クラスに同じシグネチャを持つ 2 つの仮想メソッドを含めます。導入された、1 つ`A`で導入された、1 つ`C`します。</span><span class="sxs-lookup"><span data-stu-id="be588-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="be588-898">導入されたメソッド`C`から継承されたメソッドを非表示に`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="be588-899">オーバーライド宣言つまり`D`で導入されたメソッドをオーバーライド`C`のことはできませんと`D`で導入されたメソッドをオーバーライドする`A`。</span><span class="sxs-lookup"><span data-stu-id="be588-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="be588-900">この例では、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-900">The example produces the output:</span></span>
```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="be588-901">インスタンスへのアクセスによって非表示の仮想メソッドを呼び出すことは`D`小から派生した型、メソッドが非表示されません。</span><span class="sxs-lookup"><span data-stu-id="be588-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="be588-902">メソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="be588-902">Override methods</span></span>

<span data-ttu-id="be588-903">インスタンス メソッドの宣言が含まれています、`override`修飾子、メソッドはモード、***メソッドをオーバーライドして***します。</span><span class="sxs-lookup"><span data-stu-id="be588-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="be588-904">オーバーライド メソッドでは、同じシグネチャを持つ継承された仮想メソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="be588-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="be588-905">仮想メソッドの宣言には新しいメソッドが導入されていますが、オーバーライド メソッドの宣言では、そのメソッドの新しい実装を提供することで既存の継承された仮想メソッドを特殊化します。</span><span class="sxs-lookup"><span data-stu-id="be588-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="be588-906">によってオーバーライドされたメソッド、`override`宣言と呼ばれる、***基本メソッドをオーバーライド***します。</span><span class="sxs-lookup"><span data-stu-id="be588-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="be588-907">オーバーライド メソッド`M`クラスで宣言されている`C`、オーバーライドされる基本メソッドはの各基本クラス型を調べることによって決まります`C`の直接基底クラスの型と、`C`進みながら連続します。直接の基本クラスの型が指定された基本クラス型であるアクセス可能なメソッドは、少なくとも 1 つまでと同じシグネチャを持つ`M`型引数の置換後にします。</span><span class="sxs-lookup"><span data-stu-id="be588-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="be588-908">オーバーライドされる基本メソッドを検索するためには、メソッドと見なされますがある場合は、アクセス`public`である場合、`protected`である場合、`protected internal`である場合または`internal`として同じプログラム内で宣言されていると`C`します。</span><span class="sxs-lookup"><span data-stu-id="be588-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="be588-909">上書きの宣言の場合は true、次のすべての場合を除いて、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="be588-910">前述のように配置されている、オーバーライドされる基本メソッドは使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="be588-911">このようなオーバーライドされる基本メソッドは 1 つだけです。</span><span class="sxs-lookup"><span data-stu-id="be588-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="be588-912">この制限は、基本クラスの型が構築された型、型引数の置換により 2 つのメソッドのシグネチャと同じ場合にのみ影響です。</span><span class="sxs-lookup"><span data-stu-id="be588-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="be588-913">オーバーライドされる基本メソッドが仮想、抽象型の場合、またはメソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="be588-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="be588-914">つまり、オーバーライドされる基本メソッドは、静的または非仮想にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="be588-915">オーバーライドの基本メソッドは、シール メソッドではありません。</span><span class="sxs-lookup"><span data-stu-id="be588-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="be588-916">オーバーライド メソッドとオーバーライドされる基本メソッドと同じ戻り値型であります。</span><span class="sxs-lookup"><span data-stu-id="be588-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="be588-917">オーバーライドされる基本メソッドとオーバーライド宣言は、同じの宣言されたアクセシビリティを持ちます。</span><span class="sxs-lookup"><span data-stu-id="be588-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="be588-918">つまり、オーバーライド宣言は、仮想メソッドのアクセシビリティを変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="be588-919">ただし、オーバーライドされる基本メソッドの内部が保護されており、オーバーライド メソッド、オーバーライド メソッドを含むアセンブリが宣言されているよりもに、別のアセンブリで宣言されている場合は、アクセシビリティを保護されなければなりません。</span><span class="sxs-lookup"><span data-stu-id="be588-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="be588-920">オーバーライドの宣言では、型パラメーター制約句が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="be588-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="be588-921">代わりに、制約は、オーバーライドされる基本メソッドから継承されます。</span><span class="sxs-lookup"><span data-stu-id="be588-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="be588-922">オーバーライドされたメソッドの型パラメーターの制約を型引数は、継承された制約に置き換えることができますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="be588-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="be588-923">これは、法的なは、値の型または sealed 型など、明示的に指定されている制約につながります。</span><span class="sxs-lookup"><span data-stu-id="be588-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="be588-924">次の例では、ジェネリック クラスのオーバーライドの規則のしくみを示しています。</span><span class="sxs-lookup"><span data-stu-id="be588-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="be588-925">オーバーライド宣言は、オーバーライドされる基本メソッドを使用して、アクセスできる、 *base_access* ([Base アクセス](expressions.md#base-access))。</span><span class="sxs-lookup"><span data-stu-id="be588-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="be588-926">例</span><span class="sxs-lookup"><span data-stu-id="be588-926">In the example</span></span>
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
<span data-ttu-id="be588-927">`base.PrintFields()`で呼び出し`B`呼び出す、`PrintFields`メソッドで宣言`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="be588-928">A *base_access*仮想呼び出しメカニズムを無効にし、単に基本のメソッドを非仮想メソッドとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="be588-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="be588-929">呼び出す必要がある`B`されて書き込まれる`((A)this).PrintFields()`、再帰的に存在するかを呼び出す、`PrintFields`メソッドで宣言`B`で宣言されている 1 つではない`A`、ため`PrintFields`仮想の実行時の型と`((A)this)`は`B`します。</span><span class="sxs-lookup"><span data-stu-id="be588-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="be588-930">含めることによってのみ、`override`修飾子は、メソッドは、別のメソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="be588-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="be588-931">その他のすべてのケースで、継承されたメソッドとして同じシグネチャを持つメソッドは単に継承されたメソッドを隠ぺいします。</span><span class="sxs-lookup"><span data-stu-id="be588-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="be588-932">例</span><span class="sxs-lookup"><span data-stu-id="be588-932">In the example</span></span>
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
<span data-ttu-id="be588-933">`F`メソッド`B`は含まれません、`override`修飾子を上書きしませんし、`F`メソッド`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="be588-934">代わりに、`F`メソッド`B`でメソッドを非表示に`A`、宣言が含まれていないために、警告が報告されると、`new`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="be588-935">例</span><span class="sxs-lookup"><span data-stu-id="be588-935">In the example</span></span>
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
<span data-ttu-id="be588-936">`F`メソッド`B`仮想を非表示に`F`から継承されたメソッド`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="be588-937">以降、新しい`F`で`B`プライベート アクセスは、そのスコープには、のクラスの本文にはのみが含まれます。`B`には拡張されませんと`C`します。</span><span class="sxs-lookup"><span data-stu-id="be588-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="be588-938">宣言ではそのため、`F`で`C`上書きが許可されている、`F`から継承された`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="be588-939">シール メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-939">Sealed methods</span></span>

<span data-ttu-id="be588-940">インスタンス メソッドの宣言が含まれています、`sealed`修飾子、メソッドがあると言うこと、***メソッドをシール***します。</span><span class="sxs-lookup"><span data-stu-id="be588-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="be588-941">インスタンス メソッドの宣言が含まれている場合、`sealed`修飾子も含める必要があります、`override`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="be588-942">使用、`sealed`修飾子からさらに、メソッドをオーバーライドする派生クラスをできないようにします。</span><span class="sxs-lookup"><span data-stu-id="be588-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="be588-943">例</span><span class="sxs-lookup"><span data-stu-id="be588-943">In the example</span></span>
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
<span data-ttu-id="be588-944">クラスは、 `B` 2 つのメソッドのオーバーライドは、:`F`メソッドを持つ、`sealed`修飾子と`G`メソッドにはないです。</span><span class="sxs-lookup"><span data-stu-id="be588-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="be588-945">`B`使用が、シールドの`modifier`防止`C`をさらにオーバーライド`F`します。</span><span class="sxs-lookup"><span data-stu-id="be588-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="be588-946">抽象メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-946">Abstract methods</span></span>

<span data-ttu-id="be588-947">インスタンス メソッドの宣言が含まれています、`abstract`修飾子、メソッドがあると言うこと、***抽象メソッド***します。</span><span class="sxs-lookup"><span data-stu-id="be588-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="be588-948">抽象メソッドは暗黙的にも仮想メソッドの修飾子を持つことはできません`virtual`します。</span><span class="sxs-lookup"><span data-stu-id="be588-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="be588-949">抽象メソッドの宣言では、新しい仮想メソッドが導入されていますが、そのメソッドの実装は提供されません。</span><span class="sxs-lookup"><span data-stu-id="be588-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="be588-950">代わりに、非抽象派生クラスでは、そのメソッドをオーバーライドすることで独自の実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="be588-951">抽象メソッドが実際の実装を提供しないため、 *method_body*の抽象メソッドをセミコロンの構成だけです。</span><span class="sxs-lookup"><span data-stu-id="be588-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="be588-952">抽象メソッドの宣言は抽象クラスでのみ許可されます ([抽象クラス](classes.md#abstract-classes))。</span><span class="sxs-lookup"><span data-stu-id="be588-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="be588-953">例</span><span class="sxs-lookup"><span data-stu-id="be588-953">In the example</span></span>
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
<span data-ttu-id="be588-954">`Shape`クラス自体を描画できる幾何学的な図形オブジェクトの抽象概念を定義します。</span><span class="sxs-lookup"><span data-stu-id="be588-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="be588-955">`Paint`意味のある既定の実装がないため、抽象メソッドです。</span><span class="sxs-lookup"><span data-stu-id="be588-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="be588-956">`Ellipse`と`Box`クラスは具象`Shape`実装します。</span><span class="sxs-lookup"><span data-stu-id="be588-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="be588-957">オーバーライドする必要がこれらのクラスは、非抽象であるため、`Paint`メソッドと、実際の実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="be588-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="be588-958">コンパイル時エラーには、 *base_access* ([Base アクセス](expressions.md#base-access)) を抽象メソッドを参照します。</span><span class="sxs-lookup"><span data-stu-id="be588-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="be588-959">例</span><span class="sxs-lookup"><span data-stu-id="be588-959">In the example</span></span>
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
<span data-ttu-id="be588-960">コンパイル時エラーが報告された、`base.F()`呼び出し抽象メソッドを参照しているためです。</span><span class="sxs-lookup"><span data-stu-id="be588-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="be588-961">抽象メソッドの宣言は、仮想メソッドをオーバーライド許可されています。</span><span class="sxs-lookup"><span data-stu-id="be588-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="be588-962">これにより、派生クラスでメソッドの再実装を強制する抽象クラスとメソッドの元の実装を使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="be588-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="be588-963">例</span><span class="sxs-lookup"><span data-stu-id="be588-963">In the example</span></span>
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
<span data-ttu-id="be588-964">クラス`A`仮想メソッド、クラスを宣言`B`抽象メソッドとクラスを使用してこのメソッドをオーバーライド`C`独自の実装を提供する抽象メソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="be588-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="be588-965">外部メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-965">External methods</span></span>

<span data-ttu-id="be588-966">メソッドの宣言が含まれています、`extern`修飾子、メソッドがあると言うこと、***外部メソッド***します。</span><span class="sxs-lookup"><span data-stu-id="be588-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="be588-967">外部メソッドは、通常 c# 以外の言語を使用して外部で実装されます。</span><span class="sxs-lookup"><span data-stu-id="be588-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="be588-968">外部メソッドの宣言は、実際の実装を提供するため、 *method_body*の外部メソッドをセミコロンの構成だけです。</span><span class="sxs-lookup"><span data-stu-id="be588-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="be588-969">外部メソッドのジェネリックできない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-969">An external method may not be generic.</span></span>

<span data-ttu-id="be588-970">`extern`修飾子が組み合わせてで通常使用される、`DllImport`属性 ([と Win32 の COM コンポーネントとの相互運用](attributes.md#interoperation-with-com-and-win32-components))、外部メソッドを Dll (ダイナミック リンク ライブラリ) によって実装することができます。</span><span class="sxs-lookup"><span data-stu-id="be588-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="be588-971">実行環境では、外部メソッドの実装を提供できる、他のメカニズムをサポートできます。</span><span class="sxs-lookup"><span data-stu-id="be588-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="be588-972">外部メソッドが含まれる場合、`DllImport`属性、メソッドの宣言を含める必要がありますも、`static`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="be588-973">この例の使用、`extern`修飾子と`DllImport`属性。</span><span class="sxs-lookup"><span data-stu-id="be588-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="be588-974">部分メソッド (まとめ)</span><span class="sxs-lookup"><span data-stu-id="be588-974">Partial methods (recap)</span></span>

<span data-ttu-id="be588-975">メソッドの宣言が含まれています、`partial`修飾子、メソッドがあると言うこと、***部分メソッド***します。</span><span class="sxs-lookup"><span data-stu-id="be588-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="be588-976">部分メソッドは、部分型のメンバーとしてのみ宣言できます ([部分型](classes.md#partial-types))、およびいくつかの制限を受けます。</span><span class="sxs-lookup"><span data-stu-id="be588-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="be588-977">部分メソッドはさらに「[部分メソッド](classes.md#partial-methods)します。</span><span class="sxs-lookup"><span data-stu-id="be588-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="be588-978">拡張メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-978">Extension methods</span></span>

<span data-ttu-id="be588-979">メソッドの最初のパラメーターが含まれる場合、`this`修飾子、メソッドがあると言うこと、***拡張メソッド***します。</span><span class="sxs-lookup"><span data-stu-id="be588-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="be588-980">拡張メソッドは、非ジェネリック、入れ子になった非静的クラスでのみ宣言できます。</span><span class="sxs-lookup"><span data-stu-id="be588-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="be588-981">拡張メソッドの最初のパラメーターを持たない修飾子以外`this`パラメーターの型がポインター型にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="be588-982">2 つの拡張メソッドを宣言する静的クラスの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="be588-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="be588-983">拡張メソッドは、通常の静的メソッドです。</span><span class="sxs-lookup"><span data-stu-id="be588-983">An extension method is a regular static method.</span></span> <span data-ttu-id="be588-984">さらに、その外側の静的クラスは、スコープ内では、拡張メソッド呼び出すことができますのインスタンス メソッドの呼び出し構文を使用して ([拡張メソッド呼び出し](expressions.md#extension-method-invocations))、最初の引数として受信者式を使用します。</span><span class="sxs-lookup"><span data-stu-id="be588-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="be588-985">次のプログラムでは、上で宣言された拡張メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="be588-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="be588-986">`Slice`メソッドは、の使用、 `string[]`、および`ToInt32`メソッドで使用`string`拡張メソッドとして宣言されているため、します。</span><span class="sxs-lookup"><span data-stu-id="be588-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="be588-987">プログラムの意味では、次を使用して、通常の静的メソッドの呼び出しと同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="be588-988">メソッドの本体</span><span class="sxs-lookup"><span data-stu-id="be588-988">Method body</span></span>

<span data-ttu-id="be588-989">*Method_body*ブロック本体を式の本体またはセミコロンのいずれかのメソッドの宣言がで構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="be588-990">***型の結果***メソッドが`void`戻り値の型がある場合`void`、メソッドは非同期と戻り値の型である場合または`System.Threading.Tasks.Task`します。</span><span class="sxs-lookup"><span data-stu-id="be588-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="be588-991">戻り値の型、および非同期メソッドの戻り値の型の結果型では、非同期メソッドの結果型は、それ以外の場合、`System.Threading.Tasks.Task<T>`は`T`します。</span><span class="sxs-lookup"><span data-stu-id="be588-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="be588-992">メソッドにある場合、`void`結果の種類とブロックの本体、`return`ステートメント ([return ステートメント](statements.md#the-return-statement)) ブロックが式を指定することできません。</span><span class="sxs-lookup"><span data-stu-id="be588-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="be588-993">Void メソッドのブロックの実行が正常に完了するかどうか (つまり、メソッド本体の末尾からのフロー制御) メソッドが、現在の呼び出し元に単に返します。</span><span class="sxs-lookup"><span data-stu-id="be588-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="be588-994">メソッドにある場合、`void`結果および式、式の本文`E`必要があります、 *statement_expression*、本文がフォームのブロックの本体とまったく同じ`{ E; }`。</span><span class="sxs-lookup"><span data-stu-id="be588-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="be588-995">メソッドが void 以外の結果の型を持つ、ブロック、本体の各`return`ブロックのステートメントでは、結果の型に暗黙的に変換される式を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="be588-996">値を返すメソッドのブロックの本体のエンドポイントが到達可能な場合があります。</span><span class="sxs-lookup"><span data-stu-id="be588-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="be588-997">つまり、ブロックの本体で値を返すメソッドでは、コントロールは許可されていません、メソッド本体の末尾に到達します。</span><span class="sxs-lookup"><span data-stu-id="be588-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="be588-998">メソッドが void 以外の結果型は、式の本体と式は、結果の型に暗黙的に変換可能である必要があります、本文がフォームのブロックの本体と同じ`{ return E; }`します。</span><span class="sxs-lookup"><span data-stu-id="be588-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="be588-999">例</span><span class="sxs-lookup"><span data-stu-id="be588-999">In the example</span></span>
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
<span data-ttu-id="be588-1000">値を返す`F`メソッドは、制御は、メソッド本体の最後にフローできるため、コンパイル時エラーが発生結果します。</span><span class="sxs-lookup"><span data-stu-id="be588-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="be588-1001">`G`と`H`メソッドは、すべての実行可能なパスが戻り値を指定する、return ステートメントで終了するので、正しい。</span><span class="sxs-lookup"><span data-stu-id="be588-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="be588-1002">`I`の本体は等価ですが 1 つの戻りステートメントだけでステートメント ブロックされるため、メソッドです。</span><span class="sxs-lookup"><span data-stu-id="be588-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="be588-1003">メソッドのオーバーロード</span><span class="sxs-lookup"><span data-stu-id="be588-1003">Method overloading</span></span>

<span data-ttu-id="be588-1004">メソッドのオーバー ロード解決規則が記載されて[型推論](expressions.md#type-inference)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="be588-1005">プロパティ</span><span class="sxs-lookup"><span data-stu-id="be588-1005">Properties</span></span>

<span data-ttu-id="be588-1006">A***プロパティ***オブジェクトまたはクラスの特性へのアクセスを提供するメンバーします。</span><span class="sxs-lookup"><span data-stu-id="be588-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="be588-1007">プロパティの例では、顧客の名前、ウィンドウのキャプションのフォントのサイズの文字列の長さを含めるし、具合です。</span><span class="sxs-lookup"><span data-stu-id="be588-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="be588-1008">プロパティは、フィールドの自然な拡張機能、両方を関連する型を持つメンバーを指定し、フィールドとプロパティにアクセスするための構文は同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="be588-1009">ただし、フィールドとは異なり、プロパティは格納場所を表しません。</span><span class="sxs-lookup"><span data-stu-id="be588-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="be588-1010">その代わりに、プロパティには、値の読み取りまたは書き込みの際に実行されるステートメントを指定する "***アクセサー***" があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="be588-1011">プロパティはオブジェクトの属性の書き込みと読み取りにアクションを関連付けるためのメカニズムを提供するためさらに、それらを計算するには、このような属性を許可します。</span><span class="sxs-lookup"><span data-stu-id="be588-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="be588-1012">プロパティを使用して宣言*property_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="be588-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="be588-1013">A *property_declaration*のセットを含めることができます*属性*([属性](attributes.md)) と有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、 `new` ([new 修飾子](classes.md#the-new-modifier))、 `static` ([静的およびインスタンス メソッド](classes.md#static-and-instance-methods))、 `virtual` ([仮想メソッド](classes.md#virtual-methods))、 `override` ([メソッドをオーバーライド](classes.md#override-methods))、 `sealed` ([メソッドをシール](classes.md#sealed-methods))、 `abstract` ([抽象メソッド](classes.md#abstract-methods))、および`extern` ([外部メソッド](classes.md#external-methods)) 修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="be588-1014">プロパティ宣言では、メソッドの宣言と同じ規則 ([メソッド](classes.md#methods)) 修飾子の組み合わせが無効です。</span><span class="sxs-lookup"><span data-stu-id="be588-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="be588-1015">*型*プロパティの宣言は、宣言によって導入されるプロパティの型を指定します、 *member_name*プロパティの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="be588-1016">プロパティが、明示的なインターフェイス メンバーの実装でない限り、 *member_name*は単に、*識別子*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="be588-1017">明示的なインターフェイス メンバーの実装 ([明示的なインターフェイス メンバーの実装](interfaces.md#explicit-interface-member-implementations))、 *member_name*から成る、 *interface_type*後に、"`.`"および*識別子*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="be588-1018">*型*プロパティは少なくともプロパティ自体と同程度にアクセスできる必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="be588-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="be588-1019">A *property_body*ので構成するか、***アクセサー本体***または***式本体***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="be588-1020">アクセサーの本体で*accessor_declarations*、これで囲む必要があります"`{`「と」`}`"トークンは、アクセサーを宣言する ([アクセサー](classes.md#accessors)) プロパティの。</span><span class="sxs-lookup"><span data-stu-id="be588-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="be588-1021">アクセサーは、読み取りと書き込みのプロパティに関連付けられている実行可能ステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="be588-1022">構成される式の本体`=>`続けて、*式*`E`セミコロンはステートメント本体内でまったく同じですが、 `{ get { return E; } }`、ことができますのみため、get アクセス操作子のみを指定するにはプロパティは、get アクセス操作子の結果は 1 つの式で指定した場合。</span><span class="sxs-lookup"><span data-stu-id="be588-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="be588-1023">A *property_initializer*を自動的に実装されたプロパティ用にのみ与えられます ([自動実装プロパティ](classes.md#automatically-implemented-properties))、し、このような基になるフィールドの初期化プロパティで指定された値、*式*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="be588-1024">プロパティにアクセスするための構文と同じではフィールドが、プロパティは変数としては分類されません。</span><span class="sxs-lookup"><span data-stu-id="be588-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="be588-1025">したがって、としてプロパティを渡すことはできません、`ref`または`out`引数。</span><span class="sxs-lookup"><span data-stu-id="be588-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="be588-1026">プロパティの宣言が含まれています、`extern`修飾子は、このプロパティがあると、***外部プロパティ***。</span><span class="sxs-lookup"><span data-stu-id="be588-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="be588-1027">外部プロパティの宣言は、それぞれの実際の実装を提供するため、 *accessor_declarations*をセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="be588-1028">静的およびインスタンスのプロパティ</span><span class="sxs-lookup"><span data-stu-id="be588-1028">Static and instance properties</span></span>

<span data-ttu-id="be588-1029">プロパティの宣言が含まれています、`static`修飾子は、このプロパティがあると、***静的プロパティ***。</span><span class="sxs-lookup"><span data-stu-id="be588-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="be588-1030">ない場合`static`修飾子が存在する、このプロパティにすると、***インスタンス プロパティ***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="be588-1031">静的プロパティは、特定のインスタンスに関連付けられていないを参照すると、コンパイル時エラー`this`静的プロパティのアクセサーでします。</span><span class="sxs-lookup"><span data-stu-id="be588-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="be588-1032">インスタンス プロパティは、クラスのインスタンスに関連付けられていると、そのインスタンスとしてアクセスできます`this`([このアクセス](expressions.md#this-access)) でそのプロパティのアクセサー。</span><span class="sxs-lookup"><span data-stu-id="be588-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="be588-1033">プロパティが参照されている場合、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`場合は、`M`静的なプロパティは、 `E` を含む型を表す必要があります`M`、場合`M`インスタンス プロパティは、電子メールに含まれる型のインスタンスを表す必要があります`M`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="be588-1034">静的な違いは、インスタンス メンバーの説明とでさらに[静的メンバーとインスタンス メンバー](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="be588-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="be588-1035">アクセサー</span><span class="sxs-lookup"><span data-stu-id="be588-1035">Accessors</span></span>

<span data-ttu-id="be588-1036">*Accessor_declarations*プロパティの読み取りと書き込みのプロパティに関連付けられている実行可能なステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="be588-1037">アクセサーの宣言から成る、 *get_accessor_declaration*、 *set_accessor_declaration*、またはその両方です。</span><span class="sxs-lookup"><span data-stu-id="be588-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="be588-1038">各アクセサーの宣言は、トークンの`get`または`set`続く省略可能な*accessor_modifier*と*accessor_body*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="be588-1039">使用*accessor_modifier*s は、次の制限によって管理されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="be588-1040">*Accessor_modifier*インターフェイスまたは明示的なインターフェイス メンバーの実装で使用できません。</span><span class="sxs-lookup"><span data-stu-id="be588-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="be588-1041">プロパティまたはインデクサーを持たない`override`修飾子、 *accessor_modifier*プロパティまたはインデクサーの両方がある場合にのみ許可されて、`get`と`set`アクセサー、およびうちの 1 つでのみ許可し、アクセサー。</span><span class="sxs-lookup"><span data-stu-id="be588-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="be588-1042">プロパティまたはインデクサーを含む、`override`修飾子をアクセサーが一致する必要があります、 *accessor_modifier*いずれかオーバーライドされるアクセサーの場合は、します。</span><span class="sxs-lookup"><span data-stu-id="be588-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="be588-1043">*Accessor_modifier*プロパティまたはインデクサー自体の宣言されたアクセシビリティよりも厳密に制限は、アクセシビリティを宣言する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="be588-1044">正確には。</span><span class="sxs-lookup"><span data-stu-id="be588-1044">To be precise:</span></span>
   * <span data-ttu-id="be588-1045">プロパティまたはインデクサーの宣言されたアクセシビリティを持つかどうか`public`、 *accessor_modifier*にある場合も`protected internal`、 `internal`、 `protected`、または`private`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="be588-1046">プロパティまたはインデクサーの宣言されたアクセシビリティを持つかどうか`protected internal`、 *accessor_modifier*にある場合も`internal`、 `protected`、または`private`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="be588-1047">プロパティまたはインデクサーの宣言されたアクセシビリティを持つかどうか`internal`または`protected`、 *accessor_modifier*あります`private`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="be588-1048">プロパティまたはインデクサーの宣言されたアクセシビリティを持つかどうか`private`、 *accessor_modifier*使用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="be588-1049">`abstract`と`extern`プロパティ、 *accessor_body*指定された各アクセサーは、セミコロンだけです。</span><span class="sxs-lookup"><span data-stu-id="be588-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="be588-1050">各非 abstract、extern 以外のプロパティがあります*accessor_body*セミコロンをする場合は、***プロパティが自動的に実装***([自動実装プロパティ](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="be588-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="be588-1051">自動的に実装されたプロパティを get アクセサーが必要です。</span><span class="sxs-lookup"><span data-stu-id="be588-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="be588-1052">その他の非 abstract、extern 以外、任意のプロパティのアクセサーに、 *accessor_body*は、*ブロック*対応するアクセサーが呼び出されたときに実行されるステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="be588-1053">A`get`アクセサーはプロパティの型の値を返すパラメーターなしのメソッドに相当します。</span><span class="sxs-lookup"><span data-stu-id="be588-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="be588-1054">代入のターゲット プロパティが式で参照されている場合を除き、`get`プロパティの値を計算する、プロパティのアクセサーが呼び出されます ([式の値](expressions.md#values-of-expressions))。</span><span class="sxs-lookup"><span data-stu-id="be588-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="be588-1055">本文を`get`アクセサーは値を返すの規則に従う必要がありますで説明したメソッド[メソッド本体](classes.md#method-body)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="be588-1056">具体的には、すべて`return`ステートメントの本体で、`get`アクセサーはプロパティの型に暗黙的に変換される式を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="be588-1057">さらのエンドポイントを`get`アクセサーに到達できないする必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="be588-1058">A`set`アクセサーはプロパティの型の 1 つの値を持つパラメーターを持つメソッドに相当し、`void`型を返します。</span><span class="sxs-lookup"><span data-stu-id="be588-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="be588-1059">暗黙のパラメーターを`set`アクセサーの名前は常に`value`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="be588-1060">プロパティが割り当ての対象として参照されている場合 ([代入演算子](expressions.md#assignment-operators))、またはのオペランドとして`++`または`--`([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)、 [前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))、`set`引数にアクセサーが呼び出されます (値が代入の右側にあるかのオペランドの`++`または`--`演算子) を新しい値を提供します ([単純な代入](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="be588-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="be588-1061">本文を`set`アクセサーがの規則に従う必要があります`void`で説明したメソッド[メソッド本体](classes.md#method-body)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="be588-1062">具体的には、`return`内のステートメント、`set`式を指定するアクセサーの本体は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="be588-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="be588-1063">`set`アクセサーがという名前のパラメーターを暗黙的に`value`は、ローカル変数または定数宣言でのコンパイル時エラーです、`set`名前がアクセサー。</span><span class="sxs-lookup"><span data-stu-id="be588-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="be588-1064">有無に基づいて、`get`と`set`アクセサー、プロパティは次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="be588-1065">両方が含まれるプロパティを`get`アクセサーと`set`アクセサーはモード、***読み取り/書き込み***プロパティ。</span><span class="sxs-lookup"><span data-stu-id="be588-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="be588-1066">のみを持つプロパティを`get`アクセサーはモード、***読み取り専用***プロパティ。</span><span class="sxs-lookup"><span data-stu-id="be588-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="be588-1067">割り当ての対象となる読み取り専用プロパティのコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="be588-1068">のみを持つプロパティを`set`アクセサーはモード、***書き込み専用***プロパティ。</span><span class="sxs-lookup"><span data-stu-id="be588-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="be588-1069">ただし、代入式のターゲットとして式の中で、書き込み専用プロパティを参照すると、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="be588-1070">例</span><span class="sxs-lookup"><span data-stu-id="be588-1070">In the example</span></span>
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
<span data-ttu-id="be588-1071">`Button`コントロール宣言パブリック`Caption`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="be588-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="be588-1072">`get`のアクセサー、`Caption`プロパティがプライベートに格納された文字列を返します`caption`フィールド。</span><span class="sxs-lookup"><span data-stu-id="be588-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="be588-1073">`set`アクセサーは、新しい値を格納する場合は、新しい値が現在の値と異なる場合にチェックし、コントロールを再描画します。</span><span class="sxs-lookup"><span data-stu-id="be588-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="be588-1074">多くの場合、プロパティには、上記のパターンに従います。`get`アクセサーはプライベート フィールドに格納されている値だけを返します、`set`アクセサーは、そのプライベート フィールドを変更し、オブジェクトの状態を完全に更新するために必要な追加の操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="be588-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="be588-1075">指定された、`Button`上記のクラス、例を次の使用、`Caption`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="be588-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="be588-1076">ここでは、`set`アクセサーがプロパティの値を割り当てることによって呼び出されると、`get`アクセサーは、式でプロパティを参照することによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="be588-1077">`get`と`set`プロパティのアクセサーが個別のメンバーでないし、プロパティのアクセサーを個別に宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="be588-1078">そのため、読み取り/書き込みプロパティの 2 つのアクセサーが異なるアクセシビリティを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="be588-1079">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-1079">The example</span></span>
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
<span data-ttu-id="be588-1080">1 つの読み取り/書き込みプロパティを宣言していません。</span><span class="sxs-lookup"><span data-stu-id="be588-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="be588-1081">読み取り専用のいずれか、同じ名前の 2 つのプロパティを宣言ではなく、書き込み専用の 1 つ。</span><span class="sxs-lookup"><span data-stu-id="be588-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="be588-1082">同じクラスで宣言されている 2 つのメンバーは、同じ名前を持つことはできません、ため、例ではコンパイル時エラーが発生、します。</span><span class="sxs-lookup"><span data-stu-id="be588-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="be588-1083">派生クラスでは、継承されたプロパティと同じ名前でプロパティを宣言して、派生プロパティの読み取りと書き込みの両方に関して、継承されたプロパティを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="be588-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="be588-1084">例</span><span class="sxs-lookup"><span data-stu-id="be588-1084">In the example</span></span>
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
<span data-ttu-id="be588-1085">`P`プロパティ`B`を非表示に、`P`プロパティ`A`の読み取りと書き込みの両方に対して。</span><span class="sxs-lookup"><span data-stu-id="be588-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="be588-1086">そのため、ステートメントで</span><span class="sxs-lookup"><span data-stu-id="be588-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="be588-1087">割り当てを`b.P`以降、読み取り専用、報告されることをコンパイル時エラーが発生`P`プロパティ`B`書き込み専用の表示と非`P`プロパティ`A`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="be588-1088">ただし、キャストを隠しへのアクセスに使用できること`P`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="be588-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="be588-1089">パブリックのフィールドとは異なりは、プロパティは、オブジェクトの内部状態とそのパブリック インターフェイスとの間の分離を提供します。</span><span class="sxs-lookup"><span data-stu-id="be588-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="be588-1090">この例を検討してください。</span><span class="sxs-lookup"><span data-stu-id="be588-1090">Consider the example:</span></span>
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

<span data-ttu-id="be588-1091">ここでは、`Label`クラスを使用して 2 つ`int`フィールド、`x`と`y`、その場所を格納します。</span><span class="sxs-lookup"><span data-stu-id="be588-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="be588-1092">パブリックには、場所は両方として公開されている、`X`と`Y`プロパティとして、`Location`型のプロパティ`Point`。</span><span class="sxs-lookup"><span data-stu-id="be588-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="be588-1093">将来のバージョンの場合、`Label`と場所を格納する方が便利になります、`Point`内部的には、変更できるクラスのパブリック インターフェイスの影響を与えずに。</span><span class="sxs-lookup"><span data-stu-id="be588-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="be588-1094">`x`と`y`されて代わりに`public readonly`フィールド、それはされているを変更すること、`Label`クラス。</span><span class="sxs-lookup"><span data-stu-id="be588-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="be588-1095">プロパティから状態を公開することは必ずしもフィールドを直接公開するよりも効率が落ちるではありません。</span><span class="sxs-lookup"><span data-stu-id="be588-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="be588-1096">具体的には、プロパティは、非仮想少量のコードのみを含む、ときに実行環境がアクセサーへの呼び出し、アクセサーの実際のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="be588-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="be588-1097">このプロセスと呼びます***インライン展開***、およびフィールドへのアクセス、ほど効率的プロパティへのアクセスは、まだプロパティの柔軟性の向上が保持されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="be588-1098">以降の呼び出しを`get`アクセサーは概念的には、フィールドの値を読み取るのと同じですが、プログラミング スタイルは不適切と見なされます`get`アクセサーを観測可能な副作用があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="be588-1099">例</span><span class="sxs-lookup"><span data-stu-id="be588-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="be588-1100">値、`Next`プロパティは、プロパティにアクセスした回数によって異なります。</span><span class="sxs-lookup"><span data-stu-id="be588-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="be588-1101">したがって、監視可能な側効果を生成するプロパティにアクセスする、プロパティを代わりに、メソッドとして実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="be588-1102">「副作用」の規則`get`アクセサーは、ことを意味しない`get`を単にフィールドに格納されている値を返すアクセサーを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="be588-1103">実際、`get`アクセサーは多くの場合、複数のフィールドへのアクセスやメソッドの呼び出しによってプロパティの値を計算します。</span><span class="sxs-lookup"><span data-stu-id="be588-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="be588-1104">ただし、適切にデザインされた`get`アクセサーは、オブジェクトの状態の監視可能な変更を引き起こす操作を行いません。</span><span class="sxs-lookup"><span data-stu-id="be588-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="be588-1105">最初に参照される時点まで、リソースの初期化を遅延プロパティを使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="be588-1106">例:</span><span class="sxs-lookup"><span data-stu-id="be588-1106">For example:</span></span>
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

<span data-ttu-id="be588-1107">`Console`クラスには、3 つのプロパティが含まれています。 `In`、 `Out`、および`Error`、標準入力、出力、およびデバイスのエラーをそれぞれ表すです。</span><span class="sxs-lookup"><span data-stu-id="be588-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="be588-1108">これらのメンバーのプロパティとして公開することで、`Console`クラスは実際に使用されるまで初期化を遅らせることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="be588-1109">たとえば、最初に参照するときとき、`Out`に示すように、プロパティ</span><span class="sxs-lookup"><span data-stu-id="be588-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="be588-1110">基になる`TextWriter`出力デバイスを作成します。</span><span class="sxs-lookup"><span data-stu-id="be588-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="be588-1111">アプリケーションへの参照がない場合は、`In`と`Error`それらのデバイス プロパティ、そのオブジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="be588-1112">自動実装プロパティ</span><span class="sxs-lookup"><span data-stu-id="be588-1112">Automatically implemented properties</span></span>

<span data-ttu-id="be588-1113">自動的に実装されたプロパティ (または***自動プロパティ***省略形)、セミコロン専用のアクセサー本体を持つ非抽象 extern 以外のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="be588-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="be588-1114">自動プロパティを使用して、get アクセサーがあります、set アクセサーを持つことができます必要に応じて。</span><span class="sxs-lookup"><span data-stu-id="be588-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="be588-1115">プロパティを指定するには、自動的に実装されたプロパティとして、非表示のバッキング フィールドが自動的には、プロパティの使用可能なとアクセサーが読み取りし、そのバッキング フィールドへの書き込みのために実装されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="be588-1116">バッキング フィールドと見なされます自動プロパティには、set アクセサーがなければ、 `readonly` ([読み取り専用フィールド](classes.md#readonly-fields))。</span><span class="sxs-lookup"><span data-stu-id="be588-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="be588-1117">同じように、`readonly`フィールドを getter 専用自動プロパティ割り当てることもできますを外側のクラスのコンス トラクターの本体でします。</span><span class="sxs-lookup"><span data-stu-id="be588-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="be588-1118">このような割り当ては、プロパティの読み取り専用のバッキング フィールドに直接割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="be588-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="be588-1119">自動プロパティことができます、 *property_initializer*、として、バッキング フィールドに直接適用、 *variable_initializer* ([変数初期化子](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="be588-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="be588-1120">次の例では:</span><span class="sxs-lookup"><span data-stu-id="be588-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="be588-1121">次の宣言と同等です。</span><span class="sxs-lookup"><span data-stu-id="be588-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="be588-1122">次の例では:</span><span class="sxs-lookup"><span data-stu-id="be588-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="be588-1123">次の宣言と同等です。</span><span class="sxs-lookup"><span data-stu-id="be588-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="be588-1124">コンス トラクター内で発生したため、読み取り専用フィールドへの割り当てが、有効なことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="be588-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="be588-1125">ユーザー補助</span><span class="sxs-lookup"><span data-stu-id="be588-1125">Accessibility</span></span>

<span data-ttu-id="be588-1126">アクセサーがある場合、 *accessor_modifier*、アクセシビリティ ドメイン ([アクセシビリティ ドメイン](basic-concepts.md#accessibility-domains)) は、アクセサーの決まりますの宣言されたアクセシビリティを使用して、 *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="be588-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="be588-1127">アクセサーがない場合、 *accessor_modifier*アクセサーのアクセシビリティ ドメインは、プロパティまたはインデクサーの宣言されたアクセシビリティから決定されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="be588-1128">有無、 *accessor_modifier*メンバー検索には影響せず ([演算子](expressions.md#operators)) オーバー ロードの解決または ([オーバー ロードの解決](expressions.md#overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="be588-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="be588-1129">プロパティまたはインデクサーに修飾子のプロパティまたはインデクサーは、バインドのアクセス権のコンテキストに関係なく常に判別します。</span><span class="sxs-lookup"><span data-stu-id="be588-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="be588-1130">特定のプロパティまたはインデクサーが選択されていると、その使用法が有効であるかを判断する関連する特定のアクセサーのアクセシビリティ ドメインが使用されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="be588-1131">値として使用する場合 ([式の値](expressions.md#values-of-expressions))、`get`アクセサーが存在し、アクセスできるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="be588-1132">単純な代入のターゲットとして使用する場合 ([単純な代入](expressions.md#simple-assignment))、`set`アクセサーが存在し、アクセスできるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="be588-1133">複合代入のターゲットとして使用するかどうかは ([複合代入](expressions.md#compound-assignment))、またはの対象として、`++`または`--`演算子 ([関数メンバー](expressions.md#function-members).9、 [Invocation 式](expressions.md#invocation-expressions)) の両方を`get`アクセサーと`set`アクセサーが存在し、アクセスできるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="be588-1134">次の例では、プロパティで`A.Text`プロパティによって隠されている`B.Text`のみのコンテキストであっても、`set`アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="be588-1135">プロパティとは異なり、`B.Count`がクラスにアクセスできない`M`ため、アクセス可能なプロパティ`A.Count`代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="be588-1136">インターフェイスを実装するために使用されるアクセサーがない可能性があります、 *accessor_modifier*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="be588-1137">他のアクセサーを宣言することも 1 つのアクセサーを使用するインターフェイスを実装すると、専用の場合、 *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="be588-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="be588-1138">封印されて、仮想、オーバーライド、および抽象プロパティ アクセサー</span><span class="sxs-lookup"><span data-stu-id="be588-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="be588-1139">A`virtual`プロパティ宣言では、プロパティのアクセサーが仮想であることを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="be588-1140">`virtual`修飾子は読み取り/書き込みプロパティの両方のアクセサーに適用されます: 読み取り/書き込みプロパティの 1 つだけのアクセサーは、仮想にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="be588-1141">`abstract`プロパティ宣言は、プロパティのアクセサーが仮想では、アクセサーの実際の実装は提供されないことを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="be588-1142">代わりに、非抽象派生クラスでは、プロパティをオーバーライドすることで、アクセサーの独自の実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="be588-1143">抽象プロパティの宣言のアクセサーは、実際の実装を提供するため、 *accessor_body*セミコロンだけで構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="be588-1144">両方が含まれるプロパティの宣言、`abstract`と`override`修飾子は、プロパティは抽象であり、基本プロパティをオーバーライドすることを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="be588-1145">このようなプロパティのアクセサーは、抽象もあります。</span><span class="sxs-lookup"><span data-stu-id="be588-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="be588-1146">抽象プロパティの宣言は抽象クラスでのみ許可されます ([抽象クラス](classes.md#abstract-classes))。仮想継承されたプロパティのアクセサーを指定するプロパティの宣言を含めることによって派生クラスでオーバーライドできます、`override`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="be588-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="be588-1147">これと呼ばれますが、***プロパティの宣言をオーバーライドする***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="be588-1148">オーバーライドするプロパティの宣言では、新しいプロパティを宣言していません。</span><span class="sxs-lookup"><span data-stu-id="be588-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="be588-1149">代わりに、既存の仮想プロパティのアクセサーの実装を特化するだけです。</span><span class="sxs-lookup"><span data-stu-id="be588-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="be588-1150">オーバーライドするプロパティの宣言では、継承されるプロパティと正確な同じアクセシビリティ修飾子、型、および名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="be588-1151">場合 (つまり、継承されたプロパティが読み取り専用または書き込み専用である場合)、継承されたプロパティが 1 つのアクセサーだけをオーバーライドするプロパティは、そのアクセサーだけを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="be588-1152">場合 (つまり、継承されたプロパティが読み取り/書き込みである場合)、継承されたプロパティが両方のアクセサーを含む、オーバーライドするプロパティは、アクセサーが 1 つまたは両方のアクセサーのいずれかを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="be588-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="be588-1153">オーバーライドするプロパティの宣言を含めることができます、`sealed`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="be588-1154">この修飾子を使用すると、派生クラスがさらに、プロパティをオーバーライドできなくなります。</span><span class="sxs-lookup"><span data-stu-id="be588-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="be588-1155">封印されたプロパティのアクセサーはシールされてもいます。</span><span class="sxs-lookup"><span data-stu-id="be588-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="be588-1156">宣言と呼び出しの相違点を除いて、構文、仮想、sealed、オーバーライド、および抽象アクセサーが仮想、シール、override キーワードと抽象メソッドとまったく同じ動作です。</span><span class="sxs-lookup"><span data-stu-id="be588-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="be588-1157">具体的には、規則が記載[仮想メソッド](classes.md#virtual-methods)、[メソッドをオーバーライド](classes.md#override-methods)、[メソッドをシール](classes.md#sealed-methods)、および[抽象メソッド](classes.md#abstract-methods)適用としてアクセサーは、対応する形式のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="be588-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="be588-1158">A`get`アクセサーはプロパティの型と、包含するプロパティと同じ修飾子の値を返すパラメーターなしのメソッドに相当します。</span><span class="sxs-lookup"><span data-stu-id="be588-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="be588-1159">A`set`アクセサーはプロパティの型の 1 つの値を持つパラメーターを持つメソッドに相当する`void`種類、およびそれを含むプロパティと同じ修飾子を返します。</span><span class="sxs-lookup"><span data-stu-id="be588-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="be588-1160">例</span><span class="sxs-lookup"><span data-stu-id="be588-1160">In the example</span></span>
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
<span data-ttu-id="be588-1161">`X` 仮想の読み取り専用プロパティは、`Y`仮想の読み取り/書き込みプロパティと`Z`は抽象の読み取り/書き込みプロパティです。</span><span class="sxs-lookup"><span data-stu-id="be588-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="be588-1162">`Z`抽象クラスでは、含んでいるクラス`A`する必要がありますも抽象として宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="be588-1163">派生したクラス`A`次に示します。</span><span class="sxs-lookup"><span data-stu-id="be588-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="be588-1164">ここでは、宣言の`X`、 `Y`、および`Z`プロパティの宣言をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="be588-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="be588-1165">各プロパティの宣言は、アクセシビリティ修飾子、型、および対応する継承されたプロパティの名前を正確に一致します。</span><span class="sxs-lookup"><span data-stu-id="be588-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="be588-1166">`get`のアクセサー`X`と`set`のアクセサー`Y`を使用して、`base`キーワードを使って、継承されたアクセサーにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="be588-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="be588-1167">宣言`Z`両方の抽象アクセサーのオーバーライド: そのため、未処理の抽象関数のメンバーではありません`B`と`B`非抽象クラスにすることが許可されています。</span><span class="sxs-lookup"><span data-stu-id="be588-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="be588-1168">プロパティとして宣言されている場合、 `override`、オーバーライドされるアクセサーをオーバーライドするコードにアクセスできる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="be588-1169">さらに、プロパティまたはインデクサー自体の両方のおよびアクセサーの宣言されたアクセシビリティでは、オーバーライドされたメンバーおよびアクセサーの一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="be588-1170">例:</span><span class="sxs-lookup"><span data-stu-id="be588-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="be588-1171">イベント</span><span class="sxs-lookup"><span data-stu-id="be588-1171">Events</span></span>

<span data-ttu-id="be588-1172">***イベント***オブジェクトや通知を提供するクラスのメンバーであります。</span><span class="sxs-lookup"><span data-stu-id="be588-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="be588-1173">クライアントは、イベントの実行可能コードを追加指定することによって***イベント ハンドラー***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="be588-1174">使用してイベントが宣言された*event_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="be588-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="be588-1175">*Event_declaration*のセットを含めることができます*属性*([属性](attributes.md)) と有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、 `new` ([new 修飾子](classes.md#the-new-modifier))、 `static` ([静的およびインスタンス メソッド](classes.md#static-and-instance-methods))、 `virtual` ([仮想メソッド](classes.md#virtual-methods))、 `override` ([メソッドをオーバーライド](classes.md#override-methods))、 `sealed` ([メソッドをシール](classes.md#sealed-methods))、 `abstract` ([抽象メソッド](classes.md#abstract-methods))、および`extern` ([外部メソッド](classes.md#external-methods)) 修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="be588-1176">イベント宣言では、メソッドの宣言と同じ規則 ([メソッド](classes.md#methods)) 修飾子の組み合わせが無効です。</span><span class="sxs-lookup"><span data-stu-id="be588-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="be588-1177">*型*イベントの宣言がある必要があります、 *delegate_type* ([参照型](types.md#reference-types))、および*delegate_type*以上である必要がありますとしてイベント自体としてアクセスできるように ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="be588-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="be588-1178">イベントの宣言を含めることができます*event_accessor_declarations*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="be588-1179">ただし、そうでない場合、extern 以外の場合の非抽象イベントをコンパイラによってに自動的に ([フィールドのようなイベント](classes.md#field-like-events)) は、extern イベント アクセサーが外部から提供されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="be588-1180">イベントの宣言を省略した*event_accessor_declarations* 1 つまたは複数のイベントを定義します: のごとに 1 つ、 *variable_declarator*秒。</span><span class="sxs-lookup"><span data-stu-id="be588-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="be588-1181">属性と修飾子などで宣言されたメンバーのすべてに適用されます、 *event_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="be588-1182">コンパイル時エラーには、 *event_declaration*両方を含める、`abstract`修飾子と中かっこで区切られた*event_accessor_declarations*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="be588-1183">イベントの宣言が含まれています、`extern`修飾子は、イベントはモード、***外部イベント***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="be588-1184">両方を追加するエラーです、外部のイベント宣言は実際の実装を提供しないため、`extern`修飾子と*event_accessor_declarations*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="be588-1185">コンパイル時エラーには、 *variable_declarator*イベント宣言での`abstract`または`external`修飾子を含める、 *variable_initializer*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="be588-1186">イベントの左側のオペランドとして使用できる、`+=`と`-=`演算子 ([イベント割り当て](expressions.md#event-assignment))。</span><span class="sxs-lookup"><span data-stu-id="be588-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="be588-1187">これらの演算子を使用、それぞれにイベント ハンドラーをアタッチするか、イベントからイベント ハンドラーを削除して、イベントのアクセス修飾子は、このような操作が許可されているコンテキストを制御します。</span><span class="sxs-lookup"><span data-stu-id="be588-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="be588-1188">`+=`と`-=`はイベント、外部コードを宣言する型の外部イベントが許可されている唯一の操作できます追加し、イベントのハンドラーを削除するが、ことはできません、他の方法で取得またはイベントの基になるリストを変更ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="be588-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="be588-1189">フォームの操作で`x += y`または`x -= y`、`x`イベントは、の宣言を含む型の外部参照が行わ`x`、操作の結果は、型を持つ`void`(直す必要はありません型`x`の値を持つ`x`代入された後)。</span><span class="sxs-lookup"><span data-stu-id="be588-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="be588-1190">このルールは、イベントの基になるデリゲートを直接調べることから、外部コードを禁止します。</span><span class="sxs-lookup"><span data-stu-id="be588-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="be588-1191">次の例のインスタンスにイベント ハンドラーをアタッチする方法を示しています、`Button`クラス。</span><span class="sxs-lookup"><span data-stu-id="be588-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="be588-1192">ここでは、`LoginDialog`インスタンス コンス トラクターでは、2 つ作成されます`Button`インスタンスし、イベント ハンドラーをアタッチします、`Click`イベント。</span><span class="sxs-lookup"><span data-stu-id="be588-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="be588-1193">フィールドのようなイベント</span><span class="sxs-lookup"><span data-stu-id="be588-1193">Field-like events</span></span>

<span data-ttu-id="be588-1194">クラスまたはイベントの宣言を含む構造体のプログラム テキスト内のフィールドのような特定のイベントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="be588-1195">この方法で使用する、イベントすることはできません`abstract`または`extern`、明示的に含めることはできませんと*event_accessor_declarations*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="be588-1196">このような場合は、フィールドを許容する任意のコンテキストで使用できます。</span><span class="sxs-lookup"><span data-stu-id="be588-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="be588-1197">フィールドには、デリゲートが含まれています ([デリゲート](delegates.md)) これは、イベントに追加されているイベント ハンドラーの一覧を指します。</span><span class="sxs-lookup"><span data-stu-id="be588-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="be588-1198">イベント ハンドラーが追加されていない場合、フィールドが含まれます`null`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="be588-1199">例</span><span class="sxs-lookup"><span data-stu-id="be588-1199">In the example</span></span>
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
<span data-ttu-id="be588-1200">`Click` 内のフィールドとして提供される、`Button`クラス。</span><span class="sxs-lookup"><span data-stu-id="be588-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="be588-1201">例に示すようフィールド検査、変更、および使用できるデリゲートの呼び出し式で。</span><span class="sxs-lookup"><span data-stu-id="be588-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="be588-1202">`OnClick`メソッドで、`Button`クラスが「生成」、`Click`イベント。</span><span class="sxs-lookup"><span data-stu-id="be588-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="be588-1203">イベントを発生させるという概念は、イベントによって表されるデリゲートの呼び出しとまったく同じです。したがって、イベントを発生させるための特殊な言語コンストラクトはありません。</span><span class="sxs-lookup"><span data-stu-id="be588-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="be588-1204">デリゲートの呼び出しは、デリゲートが null でないことを確認するチェックに続くことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="be588-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="be588-1205">宣言の外側、`Button`クラス、`Click`のみの左側にあるメンバーを使用できます、`+=`と`-=`の演算子</span><span class="sxs-lookup"><span data-stu-id="be588-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="be588-1206">呼び出しリストにデリゲートを追加する`Click`イベント、および</span><span class="sxs-lookup"><span data-stu-id="be588-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="be588-1207">呼び出しリストからデリゲートを削除する`Click`イベント。</span><span class="sxs-lookup"><span data-stu-id="be588-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="be588-1208">フィールドのようにイベントをコンパイルするときに、コンパイラは自動的に、デリゲートを保持するストレージを作成し、追加または削除するデリゲート フィールドのイベント ハンドラーをイベントのアクセサーを作成します。</span><span class="sxs-lookup"><span data-stu-id="be588-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="be588-1209">追加と削除操作はスレッド セーフであると可能性があります (ただしする必要はありません) 中に実行するロックを保持 ([lock ステートメント](statements.md#the-lock-statement)) インスタンスのイベントを格納するオブジェクト、または型のオブジェクト ([Anonymousオブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) の静的イベント。</span><span class="sxs-lookup"><span data-stu-id="be588-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="be588-1210">したがって、インスタンス イベントの形式の宣言:</span><span class="sxs-lookup"><span data-stu-id="be588-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="be588-1211">等価のものにコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="be588-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="be588-1212">クラス内で`X`への参照`Ev`の左側にある、`+=`と`-=`演算子の追加が発生して remove アクセサーを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="be588-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="be588-1213">その他のすべての参照を`Ev`隠しフィールドを参照にコンパイルされます`__Ev`代わりに ([メンバー アクセス](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="be588-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="be588-1214">名前"`__Ev`"は任意です。 非表示フィールドがあって任意の名前または名前のないすべての。</span><span class="sxs-lookup"><span data-stu-id="be588-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="be588-1215">イベント アクセサー</span><span class="sxs-lookup"><span data-stu-id="be588-1215">Event accessors</span></span>

<span data-ttu-id="be588-1216">イベントの宣言は通常省略*event_accessor_declarations*、`Button`上記の例です。</span><span class="sxs-lookup"><span data-stu-id="be588-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="be588-1217">そのため、1 つの状況としてには、イベントごとに 1 つのフィールドのストレージ コストが許容されない場合が含まれます。</span><span class="sxs-lookup"><span data-stu-id="be588-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="be588-1218">このような場合は、クラスを含めることができます*event_accessor_declarations*およびイベント ハンドラーの一覧を格納するため、プライベート メカニズムを使用します。</span><span class="sxs-lookup"><span data-stu-id="be588-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="be588-1219">*Event_accessor_declarations*イベントの追加と削除イベント ハンドラーに関連付けられている実行可能なステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="be588-1220">アクセサーの宣言から成る、 *add_accessor_declaration*と*remove_accessor_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="be588-1221">各アクセサーの宣言は、トークンの`add`または`remove`続けて、*ブロック*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="be588-1222">*ブロック*に関連付けられている、 *add_accessor_declaration*イベント ハンドラーを追加すると、ときに実行するステートメントを指定します、*ブロック*に関連付けられています。*remove_accessor_declaration*イベント ハンドラーが削除されるときに実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="be588-1223">各*add_accessor_declaration*と*remove_accessor_declaration*イベントの種類の 1 つの値を持つパラメーターを持つメソッドに対応し、`void`型を返します。</span><span class="sxs-lookup"><span data-stu-id="be588-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="be588-1224">イベント アクセサーの暗黙のパラメーターの名前は`value`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="be588-1225">イベントはイベント割り当てで使用する適切なイベントのアクセサーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="be588-1226">具体的には、代入演算子がある場合`+=`代入演算子は、add アクセサーを使用すると、 `-=` remove アクセサーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="be588-1227">いずれの場合も、代入演算子の右側のオペランドは、イベント アクセサーに引数として使用されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="be588-1228">ブロック、 *add_accessor_declaration*または*remove_accessor_declaration*の規則に従う必要があります`void`で説明したメソッド[メソッド本体](classes.md#method-body)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="be588-1229">具体的には、`return`式を指定するこのようなブロック内のステートメントは許可されていません。</span><span class="sxs-lookup"><span data-stu-id="be588-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="be588-1230">イベント アクセサーは、という名前のパラメーターを暗黙的にため`value`、その名前を持つイベント アクセサーでローカル変数または定数が宣言されているは、コンパイル時エラー。</span><span class="sxs-lookup"><span data-stu-id="be588-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="be588-1231">例</span><span class="sxs-lookup"><span data-stu-id="be588-1231">In the example</span></span>
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
<span data-ttu-id="be588-1232">`Control`クラスは、イベント用内部ストレージ メカニズムを実装します。</span><span class="sxs-lookup"><span data-stu-id="be588-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="be588-1233">`AddEventHandler`メソッドは、キーを持つデリゲート値を関連付けます、`GetEventHandler`メソッドは、キーに関連付けられているデリゲートを返します、`RemoveEventHandler`メソッドは、指定したイベントのイベント ハンドラーとしてデリゲートを削除します。</span><span class="sxs-lookup"><span data-stu-id="be588-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="be588-1234">多くの場合、基になるストレージ メカニズムは関連付けるためのコストはありませんように作られています、`null`キーを持つ値を委任し、未処理のイベント ストレージ消費しないためです。</span><span class="sxs-lookup"><span data-stu-id="be588-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="be588-1235">静的およびインスタンスのイベント</span><span class="sxs-lookup"><span data-stu-id="be588-1235">Static and instance events</span></span>

<span data-ttu-id="be588-1236">イベントの宣言が含まれています、`static`修飾子は、イベントはモード、***静的イベント***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="be588-1237">ない場合`static`修飾子が存在する、イベントはモード、***インスタンス イベント***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="be588-1238">静的イベントは、特定のインスタンスに関連付けられていないを参照すると、コンパイル時エラー`this`静的イベントのアクセサーでします。</span><span class="sxs-lookup"><span data-stu-id="be588-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="be588-1239">インスタンス イベントは、クラスの特定のインスタンスに関連付けられ、としてこのインスタンスにアクセスできる`this`([このアクセス](expressions.md#this-access)) でそのイベントのアクセサー。</span><span class="sxs-lookup"><span data-stu-id="be588-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="be588-1240">イベントが参照されている場合、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`E.M`場合は、`M`静的なイベントは、 `E` を含む型を表す必要があります`M`、場合`M`インスタンス イベントは、電子メールに含まれる型のインスタンスを表す必要があります`M`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="be588-1241">静的な違いは、インスタンス メンバーの説明とでさらに[静的メンバーとインスタンス メンバー](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="be588-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="be588-1242">封印されて、仮想、オーバーライド、および抽象イベント アクセサー</span><span class="sxs-lookup"><span data-stu-id="be588-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="be588-1243">A`virtual`イベント宣言では、そのイベントのアクセサーが仮想であることを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="be588-1244">`virtual`修飾子は、イベントの両方のアクセサーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="be588-1245">`abstract`イベント宣言は、イベントのアクセサーが仮想では、アクセサーの実際の実装は提供されないことを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="be588-1246">代わりに、非抽象派生クラスでは、イベントをオーバーライドすることで、アクセサーの独自の実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="be588-1247">抽象イベントの宣言は実際の実装を提供しないため、中かっこで区切られたを提供できません*event_accessor_declarations*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="be588-1248">両方が含まれるイベントの宣言、`abstract`と`override`修飾子は、イベントは抽象であり、基本イベントをオーバーライドを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="be588-1249">このようなイベントのアクセサーは、抽象もあります。</span><span class="sxs-lookup"><span data-stu-id="be588-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="be588-1250">抽象イベントの宣言は抽象クラスでのみ許可されます ([抽象クラス](classes.md#abstract-classes))。</span><span class="sxs-lookup"><span data-stu-id="be588-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="be588-1251">継承された仮想イベントのアクセサーを指定するイベントの宣言を含めることによって派生クラスでオーバーライドできます、`override`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="be588-1252">これと呼ばれますが、***イベントの宣言をオーバーライドする***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="be588-1253">オーバーライドするイベントの宣言では、新しいイベントを宣言していません。</span><span class="sxs-lookup"><span data-stu-id="be588-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="be588-1254">代わりに、既存の仮想イベントのアクセサーの実装を特化するだけです。</span><span class="sxs-lookup"><span data-stu-id="be588-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="be588-1255">オーバーライドするイベントの宣言では、上書きされるイベントとして正確な同じアクセシビリティ修飾子、型、および名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="be588-1256">オーバーライドするイベントの宣言を含めることができます、`sealed`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="be588-1257">この修飾子を使用すると、派生クラスがさらに、イベントをオーバーライドできなくなります。</span><span class="sxs-lookup"><span data-stu-id="be588-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="be588-1258">封印されたイベントのアクセサーはシールされてもいます。</span><span class="sxs-lookup"><span data-stu-id="be588-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="be588-1259">コンパイル時エラーをオーバーライドする側のイベントの宣言を含めるには、`new`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="be588-1260">宣言と呼び出しの相違点を除いて、構文、仮想、sealed、オーバーライド、および抽象アクセサーが仮想、シール、override キーワードと抽象メソッドとまったく同じ動作です。</span><span class="sxs-lookup"><span data-stu-id="be588-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="be588-1261">具体的には、規則が記載[仮想メソッド](classes.md#virtual-methods)、[メソッドをオーバーライド](classes.md#override-methods)、[メソッドをシール](classes.md#sealed-methods)、および[抽象メソッド](classes.md#abstract-methods)適用としてアクセサーは、対応する形式のメソッドをでした。</span><span class="sxs-lookup"><span data-stu-id="be588-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="be588-1262">各アクセサーは、イベントの種類の 1 つの値を持つパラメーターを持つメソッドに相当する`void`種類、および、含んでいるイベントと同じ修飾子を返します。</span><span class="sxs-lookup"><span data-stu-id="be588-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="be588-1263">インデクサー</span><span class="sxs-lookup"><span data-stu-id="be588-1263">Indexers</span></span>

<span data-ttu-id="be588-1264">***インデクサー***は、配列と同じ方法でインデックスを作成するオブジェクトのメンバーです。</span><span class="sxs-lookup"><span data-stu-id="be588-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="be588-1265">使用してインデクサーを宣言*indexer_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="be588-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="be588-1266">*Indexer_declaration*のセットを含めることができます*属性*([属性](attributes.md)) と有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、 `new` ([new 修飾子](classes.md#the-new-modifier))、 `virtual` ([仮想メソッド](classes.md#virtual-methods))、 `override` ([メソッドをオーバーライド](classes.md#override-methods))、 `sealed` ([メソッドをシール](classes.md#sealed-methods))、 `abstract` ([抽象メソッド](classes.md#abstract-methods))、および`extern`([外部メソッド](classes.md#external-methods)) 修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="be588-1267">インデクサーの宣言では、メソッドの宣言と同じ規則 ([メソッド](classes.md#methods)) 有効な修飾子の組み合わせ、static 修飾子が 1 つの例外は許可されていません、インデクサーの宣言。</span><span class="sxs-lookup"><span data-stu-id="be588-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="be588-1268">修飾子`virtual`、 `override`、および`abstract`は 1 つのケースを除く相互に排他的です。</span><span class="sxs-lookup"><span data-stu-id="be588-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="be588-1269">`abstract`と`override`抽象インデクサーが仮想の 1 つをオーバーライドするために、修飾子をまとめて使用することです。</span><span class="sxs-lookup"><span data-stu-id="be588-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="be588-1270">*型*インデクサーの宣言は、宣言によって導入されるインデクサーの要素の型を指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="be588-1271">インデクサーは、明示的なインターフェイス メンバーの実装、しない限り、*型*キーワードの後に`this`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="be588-1272">明示的なインターフェイス メンバーの実装、*型*が続く、 *interface_type*、"`.`"、およびキーワード`this`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="be588-1273">他のメンバーとは異なりインデクサーにはユーザー定義の名前がありません。</span><span class="sxs-lookup"><span data-stu-id="be588-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="be588-1274">*Formal_parameter_list*インデクサーのパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="be588-1275">メソッドに対応するインデクサーの仮パラメーター リスト ([メソッドのパラメーター](classes.md#method-parameters)) には少なくとも 1 つのパラメーターを指定することを除いて、および、`ref`と`out`パラメーター修飾子は許可されていません.</span><span class="sxs-lookup"><span data-stu-id="be588-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="be588-1276">*型*で参照される型のそれぞれのインデクサーの*formal_parameter_list*少なくとも、インデクサー自体と同程度にアクセスできる必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="be588-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="be588-1277">*Indexer_body*ので構成するか、***アクセサー本体***または***式本体***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="be588-1278">アクセサーの本体で*accessor_declarations*、これで囲む必要があります"`{`「と」`}`"トークンは、アクセサーを宣言する ([アクセサー](classes.md#accessors)) プロパティの。</span><span class="sxs-lookup"><span data-stu-id="be588-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="be588-1279">アクセサーは、読み取りと書き込みのプロパティに関連付けられている実行可能ステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="be588-1280">構成される式の本体"`=>`"の後に式`E`セミコロンはステートメント本体内でまったく同じですが、 `{ get { return E; } }`、ことができますのみため、インデクサーの get アクセス操作子のみを指定する、get アクセス操作子の結果が1 つの式で指定されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="be588-1281">インデクサーの要素にアクセスするための構文と同じでは配列要素が、インデクサーの要素は変数としては分類されません。</span><span class="sxs-lookup"><span data-stu-id="be588-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="be588-1282">したがってとしてインデクサーの要素を渡すことはできません、`ref`または`out`引数。</span><span class="sxs-lookup"><span data-stu-id="be588-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="be588-1283">インデクサーの仮パラメーター リストは、署名を定義します ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading)) のインデクサー。</span><span class="sxs-lookup"><span data-stu-id="be588-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="be588-1284">具体的には、インデクサーのシグネチャは、その仮パラメーターの型と数で構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="be588-1285">要素型と仮パラメーターの名前は、インデクサーのシグネチャの一部ではないです。</span><span class="sxs-lookup"><span data-stu-id="be588-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="be588-1286">インデクサーのシグネチャは、同じクラスで宣言されている他のすべてのインデクサーの署名とは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="be588-1287">インデクサーとプロパティは、概念的によく似ていますが、次の点で異なります。</span><span class="sxs-lookup"><span data-stu-id="be588-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="be588-1288">プロパティは、インデクサーは、そのシグネチャで識別される一方、名前によって識別されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="be588-1289">プロパティを通じてアクセス、 *simple_name* ([簡易名](expressions.md#simple-names)) または*member_access* ([メンバー アクセス](expressions.md#member-access)) であるのに対し、インデクサー要素は、 *element_access* ([インデクサー アクセス](expressions.md#indexer-access))。</span><span class="sxs-lookup"><span data-stu-id="be588-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="be588-1290">プロパティを`static`メンバー、インデクサーはインスタンス メンバーでは常にします。</span><span class="sxs-lookup"><span data-stu-id="be588-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="be588-1291">A`get`一方、プロパティのアクセサーがパラメーターなしのメソッドに対応する`get`インデクサーのアクセサーは、インデクサーと同じ仮パラメーター リストを持つメソッドに相当します。</span><span class="sxs-lookup"><span data-stu-id="be588-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="be588-1292">A`set`という名前の 1 つのパラメーターのプロパティのアクセサーがメソッドに対応`value`であるのに対し、`set`インデクサーのアクセサーは、インデクサー、および追加のパラメーターとして同じ仮パラメーター リストを持つメソッドに相当名前付き`value`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="be588-1293">インデクサー パラメーターと同じ名前でローカル変数を宣言するインデクサーのアクセサーのコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="be588-1294">構文を使用して継承されたプロパティにアクセスをオーバーライドするプロパティの宣言で`base.P`ここで、`P`プロパティの名前です。</span><span class="sxs-lookup"><span data-stu-id="be588-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="be588-1295">構文を使用して、オーバーライドするインデクサーの宣言から継承されたインデクサーにアクセスは`base[E]`ここで、`E`式のコンマ区切り一覧を示します。</span><span class="sxs-lookup"><span data-stu-id="be588-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="be588-1296">「自動的に実装されたインデクサー」の概念はありません。</span><span class="sxs-lookup"><span data-stu-id="be588-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="be588-1297">セミコロンのアクセサーを持つ非抽象、非外部のインデクサーにエラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="be588-1298">これらの違いとは別にすべてのルールが定義されている[アクセサー](classes.md#accessors)と[自動実装プロパティ](classes.md#automatically-implemented-properties)インデクサー アクセサーもプロパティ アクセサーに適用します。</span><span class="sxs-lookup"><span data-stu-id="be588-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="be588-1299">インデクサーの宣言が含まれています、`extern`修飾子は、インデクサーはモード、***外部インデクサー***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="be588-1300">外部のインデクサーの宣言は、それぞれの実際の実装を提供するため、 *accessor_declarations*をセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="be588-1301">次の例の宣言を`BitArray`ビット配列内の個々 のビットにアクセスするためのインデクサーを実装するクラス。</span><span class="sxs-lookup"><span data-stu-id="be588-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="be588-1302">インスタンス、`BitArray`クラスは、対応するよりも大幅に削減のメモリを消費`bool[]`(前者の各値の代わりに 1 ビットのみが占めるため、後者の 1 バイト) と同じ操作、ようになりますが、`bool[]`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="be588-1303">次`CountPrimes`クラスで使用する`BitArray`と 1 と指定された最大値の間の素数の数を計算する従来の「エラトステネス」アルゴリズム。</span><span class="sxs-lookup"><span data-stu-id="be588-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="be588-1304">なお、構文の要素にアクセスするため、`BitArray`が正確に同じである、`bool[]`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="be588-1305">次の例では、2 つのパラメーターを持つインデクサーを持つ 26 \* 10 グリッド クラスを示します。</span><span class="sxs-lookup"><span data-stu-id="be588-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="be588-1306">大文字または小文字の文字 A ~ Z の範囲の最初のパラメーターが必要し、する 0 ~ 9 の範囲の整数である 2 つ目が必要です。</span><span class="sxs-lookup"><span data-stu-id="be588-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="be588-1307">インデクサーのオーバー ロード</span><span class="sxs-lookup"><span data-stu-id="be588-1307">Indexer overloading</span></span>

<span data-ttu-id="be588-1308">インデクサーのオーバー ロード解決規則が記載されて[型推論](expressions.md#type-inference)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="be588-1309">演算子</span><span class="sxs-lookup"><span data-stu-id="be588-1309">Operators</span></span>

<span data-ttu-id="be588-1310">***演算子***は、クラスのインスタンスに適用できる式の演算子の意味を定義するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="be588-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="be588-1311">演算子を使用して宣言*operator_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="be588-1311">Operators are declared using *operator_declaration*s:</span></span>

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

<span data-ttu-id="be588-1312">オーバー ロードされた演算子の 3 つのカテゴリがあります。単項演算子 ([単項演算子](classes.md#unary-operators))、二項演算子 ([二項演算子](classes.md#binary-operators))、および変換演算子 ([変換演算子](classes.md#conversion-operators))。</span><span class="sxs-lookup"><span data-stu-id="be588-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="be588-1313">*Operator_body*がいずれかをセミコロン、***ステートメント本体***または***式本体***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="be588-1314">ステートメント本体から成る、*ブロック*演算子が呼び出されたときに実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="be588-1315">*ブロック*値を返すための規則に準拠している必要がありますで説明したメソッド[メソッド本体](classes.md#method-body)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="be588-1316">式の本体から成る`=>`式およびセミコロンが続くし、演算子が呼び出されたときに実行する 1 つの式を表します。</span><span class="sxs-lookup"><span data-stu-id="be588-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="be588-1317">`extern` 、演算子、 *operator_body*セミコロンだけで構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="be588-1318">その他のすべての演算子、 *operator_body*はブロック本体または式の本体。</span><span class="sxs-lookup"><span data-stu-id="be588-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="be588-1319">すべての演算子の宣言に、次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="be588-1320">演算子の宣言は、両方を含める必要があります、`public`と`static`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="be588-1321">演算子のパラメーターはパラメーターの値である必要があります ([パラメーターの値](variables.md#value-parameters))。</span><span class="sxs-lookup"><span data-stu-id="be588-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="be588-1322">演算子の宣言を指定すると、コンパイル時エラー`ref`または`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="be588-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="be588-1323">演算子のシグネチャ ([単項演算子](classes.md#unary-operators)、[二項演算子](classes.md#binary-operators)、[変換演算子](classes.md#conversion-operators)) で宣言されているその他のすべての演算子のシグネチャが異なる場合、同じクラスです。</span><span class="sxs-lookup"><span data-stu-id="be588-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="be588-1324">演算子の宣言で参照されるすべての型は、少なくとも、演算子自体と同程度にアクセスである必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="be588-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="be588-1325">演算子の宣言内で複数回、同じ修飾子のエラーになります。</span><span class="sxs-lookup"><span data-stu-id="be588-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="be588-1326">各演算子のカテゴリは、次のセクションで説明したように、制限を課しています。</span><span class="sxs-lookup"><span data-stu-id="be588-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="be588-1327">他のメンバーのような基底クラスで宣言されている演算子は、派生クラスによって継承されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="be588-1328">演算子の宣言は、常に、クラスまたは構造体への参加、演算子のシグネチャ、演算子が宣言されている必要があるために、派生クラスで宣言された基本クラスで宣言されている演算子が非表示にするオペレーターのことはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="be588-1329">つまり、`new`修飾子が、必要なことはありませんし、ためでは使用できない、演算子の宣言。</span><span class="sxs-lookup"><span data-stu-id="be588-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="be588-1330">単項および二項演算子の追加情報が記載されて[演算子](expressions.md#operators)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="be588-1331">変換演算子の追加情報が記載されて[ユーザー定義の変換](conversions.md#user-defined-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="be588-1332">単項演算子</span><span class="sxs-lookup"><span data-stu-id="be588-1332">Unary operators</span></span>

<span data-ttu-id="be588-1333">単項演算子の宣言に、次の規則が適用されます、`T`はクラスまたは演算子の宣言を含む構造体のインスタンスの型を示します。</span><span class="sxs-lookup"><span data-stu-id="be588-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="be588-1334">単項`+`、 `-`、 `!`、または`~`演算子は、型の 1 つのパラメーターを受け取る必要があります`T`または`T?`任意の型を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="be588-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="be588-1335">単項`++`または`--`演算子は、型の 1 つのパラメーターを受け取る必要があります`T`または`T?`から同じ型または型が派生を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="be588-1336">単項`true`または`false`演算子は、型の 1 つのパラメーターを受け取る必要があります`T`または`T?`と型を返す必要があります`bool`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="be588-1337">単項演算子のシグネチャは、演算子のトークンで構成されます (`+`、 `-`、 `!`、 `~`、 `++`、 `--`、 `true`、または`false`) と 1 つの仮パラメーターの型。</span><span class="sxs-lookup"><span data-stu-id="be588-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="be588-1338">戻り値の型は、単項演算子のシグネチャの一部でないも仮パラメーターの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="be588-1339">`true`と`false`単項演算子には、ペアで宣言が必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="be588-1340">クラスがこれらの演算子の 1 つも、もう一方を宣言せずに宣言する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="be588-1341">`true`と`false`演算子の詳細については[ユーザー定義の条件付き論理演算子](expressions.md#user-defined-conditional-logical-operators)と[ブール式](expressions.md#boolean-expressions)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="be588-1342">次の例では、実装とのそれ以降の使用を示しています。`operator ++`整数ベクター クラス。</span><span class="sxs-lookup"><span data-stu-id="be588-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="be588-1343">演算子のメソッドが後置インクリメントと同じように、オペランドに 1 を追加することによって生成された値を返す方法に注意してくださいし、前置デクリメント演算子 ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators))、前置インクリメントとデクリメント演算子 ([前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="be588-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="be588-1344">異なり C++ では、このメソッド必要がありますいない、オペランドの値を直接変更します。</span><span class="sxs-lookup"><span data-stu-id="be588-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="be588-1345">実際には、オペランドの値を変更すると、後置のインクリメント演算子の標準のセマンティクスが違反していました。</span><span class="sxs-lookup"><span data-stu-id="be588-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="be588-1346">バイナリ演算子</span><span class="sxs-lookup"><span data-stu-id="be588-1346">Binary operators</span></span>

<span data-ttu-id="be588-1347">二項演算子の宣言に、次の規則が適用されます、`T`はクラスまたは演算子の宣言を含む構造体のインスタンスの型を示します。</span><span class="sxs-lookup"><span data-stu-id="be588-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="be588-1348">バイナリの非シフト演算子は、型である必要がありますが少なくとも 1 つ、2 つのパラメーターを受け取る必要があります`T`または`T?`、任意の型を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="be588-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="be588-1349">バイナリ`<<`または`>>`演算子の型である必要があります、2 つのパラメーターを受け取る必要があります`T`または`T?`2 番目の型である必要がありますと`int`または`int?`、任意の型を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="be588-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="be588-1350">二項演算子のシグネチャは、演算子のトークンで構成されます (`+`、 `-`、 `*`、 `/`、 `%`、 `&`、 `|`、 `^`、 `<<`、 `>>`、`==`、 `!=`、 `>`、 `<`、 `>=`、または`<=`) と 2 つの仮パラメーターの型。</span><span class="sxs-lookup"><span data-stu-id="be588-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="be588-1351">戻り値の型と仮パラメーターの名前は、二項演算子のシグネチャの一部ではありません。</span><span class="sxs-lookup"><span data-stu-id="be588-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="be588-1352">特定のバイナリ演算子では、ペアで宣言が必要です。</span><span class="sxs-lookup"><span data-stu-id="be588-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="be588-1353">すべてのペアのいずれかの演算子の宣言では、ペアの他の演算子の宣言に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="be588-1354">2 つの演算子の宣言は同じ戻り値の型と各パラメーターの型が同じである場合と一致します。</span><span class="sxs-lookup"><span data-stu-id="be588-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="be588-1355">次の演算子には、ペアで宣言が必要です。</span><span class="sxs-lookup"><span data-stu-id="be588-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="be588-1356">`operator ==` および `operator !=`</span><span class="sxs-lookup"><span data-stu-id="be588-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="be588-1357">`operator >` および `operator <`</span><span class="sxs-lookup"><span data-stu-id="be588-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="be588-1358">`operator >=` および `operator <=`</span><span class="sxs-lookup"><span data-stu-id="be588-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="be588-1359">変換演算子</span><span class="sxs-lookup"><span data-stu-id="be588-1359">Conversion operators</span></span>

<span data-ttu-id="be588-1360">変換演算子の宣言が導入されています、***ユーザー定義の変換***([ユーザー定義の変換](conversions.md#user-defined-conversions)) 定義済みの明示的および暗黙的な変換を強化します。</span><span class="sxs-lookup"><span data-stu-id="be588-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="be588-1361">変換演算子の宣言を含む、`implicit`キーワードには、ユーザー定義の暗黙的な変換が導入されています。</span><span class="sxs-lookup"><span data-stu-id="be588-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="be588-1362">暗黙的な変換は、さまざまな関数メンバーの呼び出し、式のキャスト、代入などの状況で発生することができます。</span><span class="sxs-lookup"><span data-stu-id="be588-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="be588-1363">詳細についてはこの[暗黙的な変換](conversions.md#implicit-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="be588-1364">変換演算子の宣言を含む、`explicit`キーワードには、ユーザー定義の明示的な変換が導入されています。</span><span class="sxs-lookup"><span data-stu-id="be588-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="be588-1365">明示的な変換は、キャスト式で発生しの詳細については[明示的な変換](conversions.md#explicit-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="be588-1366">変換演算子は、戻り値の型変換演算子によって示される、ターゲットの型に、変換演算子のパラメーターの型によって示される、ソースの種類からに変換します。</span><span class="sxs-lookup"><span data-stu-id="be588-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="be588-1367">指定したソースの種類`S`ターゲットの種類と`T`場合は、`S`または`T`は null 許容型は、ように`S0`と`T0`それ以外の場合、基になる型を参照してください`S0`と`T0`は等しい`S`と`T`それぞれします。</span><span class="sxs-lookup"><span data-stu-id="be588-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="be588-1368">ソースの種類からの変換を宣言するクラスまたは構造体が許可されている`S`をターゲットの型`T`次のすべてに当てはまる場合にのみ。</span><span class="sxs-lookup"><span data-stu-id="be588-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="be588-1369">`S0` `T0`はさまざまな種類。</span><span class="sxs-lookup"><span data-stu-id="be588-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="be588-1370">いずれか`S0`または`T0`演算子宣言が行われるクラスまたは構造体の型です。</span><span class="sxs-lookup"><span data-stu-id="be588-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="be588-1371">どちらも`S0`も`T0`は、 *interface_type*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="be588-1372">ユーザー定義の変換を除く、変換が存在しないから`S`に`T`またはから`T`に`S`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="be588-1373">これらの規則のためには、任意の型パラメーターに関連付けられている`S`または`T`が、他の種類、継承関係とパラメーターが無視されるこれらの型に対する制約のない一意の型と見なされます。</span><span class="sxs-lookup"><span data-stu-id="be588-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="be588-1374">例</span><span class="sxs-lookup"><span data-stu-id="be588-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="be588-1375">最初の 2 つの演算子の宣言は許可のため[インデクサー](classes.md#indexers).3`T`と`int`と`string`それぞれ一意の型とは関係がないと見なされます。</span><span class="sxs-lookup"><span data-stu-id="be588-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="be588-1376">しかし、3 番目の演算子がエラー`C<T>`の基本クラスは、`D<T>`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="be588-1377">2 番目のルールに従っている変換演算子は、または演算子が宣言されているクラスまたは構造体の型から変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="be588-1378">たとえば、クラスまたは構造体の型は`C`から変換を定義する`C`に`int`との間`int`に`C`がからではなく`int`に`bool`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="be588-1379">定義済みの変換を直接再定義することはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="be588-1380">したがって、変換演算子がからまたは変換を許可されていません`object`間明示的および暗黙的な変換が既に存在するため`object`およびその他のすべての型。</span><span class="sxs-lookup"><span data-stu-id="be588-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="be588-1381">同様に、ソースでも、変換のターゲットの種類できます、その他の基本型変換は既に存在しため。</span><span class="sxs-lookup"><span data-stu-id="be588-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="be588-1382">ただし、特定の型引数の定義済みの変換として既に存在する変換を指定するジェネリック型で演算子を宣言することはできます。</span><span class="sxs-lookup"><span data-stu-id="be588-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="be588-1383">例</span><span class="sxs-lookup"><span data-stu-id="be588-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="be588-1384">入力すると`object`がの型引数として指定されて`T`、2 番目の演算子が既に存在する変換を宣言します (、暗黙的なため明示的でも任意の型を型から変換が存在する`object`)。</span><span class="sxs-lookup"><span data-stu-id="be588-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="be588-1385">2 種類の定義済みの変換が存在する場合、それらの型の間でユーザー定義の変換は無視されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="be588-1386">具体的には、次のように使用します。</span><span class="sxs-lookup"><span data-stu-id="be588-1386">Specifically:</span></span>

*  <span data-ttu-id="be588-1387">場合、定義済みの暗黙的な変換 ([暗黙的な変換](conversions.md#implicit-conversions)) 型からが存在する`S`入力`T`、すべてのユーザー定義の変換 (暗黙的または明示的な) から`S`に`T`は無視されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="be588-1388">定義済みの明示的な変換の場合 ([明示的な変換](conversions.md#explicit-conversions)) 型から存在する`S`入力`T`、ユーザーによる明示的な変換から`S`に`T`は無視されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="be588-1389">さらには。</span><span class="sxs-lookup"><span data-stu-id="be588-1389">Furthermore:</span></span>

<span data-ttu-id="be588-1390">場合`T`インターフェイス型は、ユーザー定義からの暗黙的な変換は、`S`に`T`は無視されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="be588-1391">それ以外の場合、ユーザー定義の暗黙的な変換から`S`に`T`と見なされます。</span><span class="sxs-lookup"><span data-stu-id="be588-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="be588-1392">すべての種類が`object`、によって演算子が宣言されている、`Convertible<T>`上記の型が定義済みの変換と競合しません。</span><span class="sxs-lookup"><span data-stu-id="be588-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="be588-1393">例:</span><span class="sxs-lookup"><span data-stu-id="be588-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="be588-1394">ただし、型の`object`、定義済みの変換には、すべてのケースが 1 つのユーザー定義の変換が非表示にします。</span><span class="sxs-lookup"><span data-stu-id="be588-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="be588-1395">ユーザー定義の変換からの変換は許可されません*interface_type*秒。</span><span class="sxs-lookup"><span data-stu-id="be588-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="be588-1396">具体的には、この制限により、ユーザー定義の変換が発生しないことに変換するときに、 *interface_type*、ことへの変換、 *interface_type*場合にのみ成功オブジェクト実際に変換される、指定した実装*interface_type*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="be588-1397">変換演算子のシグネチャは、ソースの種類とターゲットの種類で構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="be588-1398">(対象の戻り値の型は、署名に参加しているメンバーの唯一の形式であるに注意してください)。`implicit`または`explicit`変換演算子の分類が演算子のシグネチャの一部ではありません。</span><span class="sxs-lookup"><span data-stu-id="be588-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="be588-1399">したがって、クラスまたは構造体を宣言できません両方、`implicit`と`explicit`を同じソースとターゲットの型変換演算子。</span><span class="sxs-lookup"><span data-stu-id="be588-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="be588-1400">一般に、ユーザー定義の暗黙的な変換は例外をスローしないと、情報が失われることはありませんするように設計する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="be588-1401">ユーザー定義の変換は、(たとえば、元の引数が範囲外です) ために、例外の上昇を与えることが場合はその変換 (上位ビットが破棄) などの情報の損失は、明示的な変換として定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="be588-1402">例</span><span class="sxs-lookup"><span data-stu-id="be588-1402">In the example</span></span>
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
<span data-ttu-id="be588-1403">変換`Digit`に`byte`決してに例外がスローまたはについてがからの変換を失うので、暗黙的に`byte`に`Digit`以降明示的`Digit`の使用可能なサブセットを表すことができますのみ値を`byte`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="be588-1404">インスタンス コンス トラクター</span><span class="sxs-lookup"><span data-stu-id="be588-1404">Instance constructors</span></span>

<span data-ttu-id="be588-1405">"***インスタンス コンストラクター***" は、クラスのインスタンスを初期化するために必要なアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="be588-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="be588-1406">使用してインスタンス コンス トラクターが宣言された*constructor_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="be588-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="be588-1407">A *constructor_declaration*のセットを含めることができます*属性*([属性](attributes.md))、有効な 4 つのアクセス修飾子の組み合わせ ([アクセス修飾子](classes.md#access-modifiers))、および`extern`([外部メソッド](classes.md#external-methods)) 修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="be588-1408">複数回、同じ修飾子を含めるには、コンス トラクターの宣言は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="be588-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="be588-1409">*識別子*の*constructor_declarator*インスタンス コンス トラクターが宣言されているクラスの名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="be588-1410">その他の任意の名前を指定すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="be588-1411">省略可能な*formal_parameter_list*インスタンスのコンス トラクターと同じ規則に従って、 *formal_parameter_list*メソッドの ([メソッド](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="be588-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="be588-1412">仮パラメーター リストは、署名を定義します ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading))、インスタンス コンス トラクターのというプロセスを管理オーバー ロードの解決 ([型推論](expressions.md#type-inference)) 特定の選択呼び出しのインスタンス コンス トラクターです。</span><span class="sxs-lookup"><span data-stu-id="be588-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="be588-1413">参照される型の各、 *formal_parameter_list*インスタンスのコンス トラクターは、少なくともコンス トラクター自体と同程度にアクセスする必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="be588-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="be588-1414">省略可能な*constructor_initializer*で指定されたステートメントを実行する前に呼び出すための別のインスタンス コンス トラクターを指定します、 *constructor_body*のこのインスタンスのコンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="be588-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="be588-1415">詳細についてはこの[コンス トラクター初期化子](classes.md#constructor-initializers)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="be588-1416">コンス トラクターの宣言が含まれています、`extern`修飾子は、コンス トラクターはモード、***コンス トラクターの外部***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="be588-1417">外部コンス トラクターの宣言は、実際の実装を提供しないため、その*constructor_body*をセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="be588-1418">その他のすべてのコンス トラクター、 *constructor_body*から成る、*ブロック*クラスの新しいインスタンスを初期化するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="be588-1419">これは正確に対応して、*ブロック*を持つインスタンス メソッドの`void`型を返す ([メソッド本体](classes.md#method-body))。</span><span class="sxs-lookup"><span data-stu-id="be588-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="be588-1420">インスタンス コンス トラクターは継承されません。</span><span class="sxs-lookup"><span data-stu-id="be588-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="be588-1421">したがって、クラスでは、実際には、クラスで宣言されている以外のインスタンス コンス トラクターがありません。</span><span class="sxs-lookup"><span data-stu-id="be588-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="be588-1422">既定のインスタンス コンス トラクターが自動的に提供されるクラスにインスタンス コンス トラクターの宣言が含まれていない場合 ([既定のコンス トラクター](classes.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="be588-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="be588-1423">インスタンス コンス トラクターを呼び出す*object_creation_expression*s ([オブジェクト作成式](expressions.md#object-creation-expressions)) および ~ *constructor_initializer*秒。</span><span class="sxs-lookup"><span data-stu-id="be588-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="be588-1424">Constructor Initializers (コンストラクター初期化子)</span><span class="sxs-lookup"><span data-stu-id="be588-1424">Constructor initializers</span></span>

<span data-ttu-id="be588-1425">すべてのインスタンス コンス トラクター (クラスを除く、 `object`) 暗黙的に別のインスタンス コンス トラクターの呼び出しをする直前に含める、 *constructor_body*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="be588-1426">暗黙的に呼び出すコンス トラクターはによって決定されます、 *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="be588-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="be588-1427">フォームのインスタンス コンス トラクター初期化子`base(argument_list)`または`base()`により呼び出される直接の基本クラスからインスタンス コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="be588-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="be588-1428">使用してそのコンス トラクターを選択*argument_list*場合存在し、オーバー ロード解決規則[オーバー ロードの解決](expressions.md#overload-resolution)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="be588-1429">直接の基本クラスに含まれているすべてのアクセス可能なインスタンス コンス トラクターまたは既定のコンス トラクターの候補のインスタンス コンス トラクターのセットで構成されます ([既定のコンス トラクター](classes.md#default-constructors)) にインスタンス コンス トラクターが宣言されていない場合、直接の基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="be588-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="be588-1430">このセットが空の場合、または 1 つの最適なインスタンス コンス トラクターを識別できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="be588-1431">フォームのインスタンス コンス トラクター初期化子`this(argument-list)`または`this()`自体を呼び出すクラスのインスタンス コンス トラクターを発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="be588-1432">使用して、コンス トラクターを選択*argument_list*場合存在し、オーバー ロード解決規則[オーバー ロードの解決](expressions.md#overload-resolution)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="be588-1433">インスタンス コンス トラクターの候補のセットは、クラス自体で宣言されているすべてのアクセス可能なインスタンス コンス トラクターで構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="be588-1434">このセットが空の場合、または 1 つの最適なインスタンス コンス トラクターを識別できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="be588-1435">インスタンス コンス トラクターの宣言には、コンス トラクター自体を呼び出すコンス トラクター初期化子が含まれている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="be588-1436">インスタンス コンス トラクターは、フォームのコンス トラクター初期化子、コンス トラクター初期化子を持たない場合`base()`が暗黙的に指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="be588-1437">したがって、インスタンス コンス トラクターの宣言フォームの</span><span class="sxs-lookup"><span data-stu-id="be588-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="be588-1438">まったく同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="be588-1439">によって指定されたパラメーターのスコープ、 *formal_parameter_list*宣言には、インスタンス コンス トラクターの宣言のコンス トラクター初期化子が含まれています。</span><span class="sxs-lookup"><span data-stu-id="be588-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="be588-1440">そのため、コンス トラクター初期化子はコンス トラクターのパラメーターにアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="be588-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="be588-1441">例:</span><span class="sxs-lookup"><span data-stu-id="be588-1441">For example:</span></span>
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

<span data-ttu-id="be588-1442">インスタンス コンス トラクターの初期化子は、作成中のインスタンスにアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="be588-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="be588-1443">そのため、コンパイル時のエラーを参照するが`this`コンス トラクター初期化子の引数式、としては、による任意のインスタンス メンバーを参照する引数式のコンパイル時エラー、 *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="be588-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="be588-1444">インスタンス変数初期化子</span><span class="sxs-lookup"><span data-stu-id="be588-1444">Instance variable initializers</span></span>

<span data-ttu-id="be588-1445">インスタンス コンス トラクターを持たないコンス トラクター初期化子、またはフォームのコンス トラクター初期化子があると`base(...)`、そのコンス トラクターが暗黙的に指定された初期化を実行、 *variable_initializer*の sインスタンス フィールドは、そのクラスで宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="be588-1446">これは、一連のコンス トラクターに、直接基底クラスのコンス トラクターの暗黙的な呼び出しの前に入ったときにすぐに実行される割り当てに対応します。</span><span class="sxs-lookup"><span data-stu-id="be588-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="be588-1447">変数の初期化子は、クラス宣言に表示されるテキストの順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="be588-1448">コンス トラクターの実行</span><span class="sxs-lookup"><span data-stu-id="be588-1448">Constructor execution</span></span>

<span data-ttu-id="be588-1449">変数初期化子は代入ステートメントに変換され、基底クラスのインスタンス コンス トラクターの呼び出しの前にこれらの代入ステートメントが実行されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="be588-1450">この順序により、すべてのインスタンス フィールドは、そのインスタンスへのアクセス権を持つすべてのステートメントが実行される前に、変数初期化子で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="be588-1451">例を示します</span><span class="sxs-lookup"><span data-stu-id="be588-1451">Given the example</span></span>
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
<span data-ttu-id="be588-1452">ときに`new B()`のインスタンスを作成するために使用`B`、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```
x = 1, y = 0
```

<span data-ttu-id="be588-1453">値`x`1 は、基底クラスのインスタンス コンス トラクターが呼び出される前に、変数の初期化子が実行されるためです。</span><span class="sxs-lookup"><span data-stu-id="be588-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="be588-1454">ただしの値`y`は 0 です (既定値の`int`) ためへの割り当てを`y`基底クラスのコンス トラクターから制御が戻た後までは実行されません。</span><span class="sxs-lookup"><span data-stu-id="be588-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="be588-1455">インスタンス変数初期化子とコンス トラクター初期化子、ステートメントの前に自動的に挿入されると考えると便利だと、 *constructor_body*します。</span><span class="sxs-lookup"><span data-stu-id="be588-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="be588-1456">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-1456">The example</span></span>
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
<span data-ttu-id="be588-1457">いくつかの変数初期化子; が含まれています両方のフォームのコンス トラクター初期化子も含まれています (`base`と`this`)。</span><span class="sxs-lookup"><span data-stu-id="be588-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="be588-1458">例は、次のコードに対応が各コメントが (自動的に挿入されたコンス トラクターの呼び出しに使用する構文が有効でないが、メカニズムを説明するためにのみ機能します)、自動的に挿入されたステートメントを示します。</span><span class="sxs-lookup"><span data-stu-id="be588-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="be588-1459">既定のコンストラクター</span><span class="sxs-lookup"><span data-stu-id="be588-1459">Default constructors</span></span>

<span data-ttu-id="be588-1460">クラスにインスタンス コンス トラクターの宣言が含まれていない場合は、既定のインスタンス コンス トラクターが自動的に提供します。</span><span class="sxs-lookup"><span data-stu-id="be588-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="be588-1461">既定のコンス トラクターは、直接基底クラスのパラメーターなしのコンス トラクターを呼び出すだけです。</span><span class="sxs-lookup"><span data-stu-id="be588-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="be588-1462">抽象クラスで宣言されたアクセシビリティの既定のコンス トラクターは保護されています。</span><span class="sxs-lookup"><span data-stu-id="be588-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="be588-1463">それ以外の場合、既定のコンス トラクターの宣言されたアクセシビリティがパブリックです。</span><span class="sxs-lookup"><span data-stu-id="be588-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="be588-1464">したがって、既定のコンス トラクターは常にフォームの</span><span class="sxs-lookup"><span data-stu-id="be588-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="be588-1465">または</span><span class="sxs-lookup"><span data-stu-id="be588-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="be588-1466">場所`C`クラスの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="be588-1467">オーバー ロードの解決が基底クラスのコンス トラクター初期化子に一意の最適な候補を特定できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="be588-1468">例</span><span class="sxs-lookup"><span data-stu-id="be588-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="be588-1469">クラスにインスタンス コンス トラクターの宣言が含まれていないために、既定のコンス トラクターが提供されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="be588-1470">したがって、例では、とまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="be588-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="be588-1471">プライベート コンス トラクター</span><span class="sxs-lookup"><span data-stu-id="be588-1471">Private constructors</span></span>

<span data-ttu-id="be588-1472">クラスと`T`プライベート インスタンス コンス トラクターのみを宣言して、プログラム テキストの外側のクラスのことはできません`T`から派生する`T`または直接のインスタンスを作成する`T`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="be588-1473">したがって、クラスが静的メンバーのみが含まれています、インスタンス化するためのものでない場合、空のプライベート インスタンス コンス トラクターを追加することはインスタンス化されないようにします。</span><span class="sxs-lookup"><span data-stu-id="be588-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="be588-1474">例:</span><span class="sxs-lookup"><span data-stu-id="be588-1474">For example:</span></span>
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

<span data-ttu-id="be588-1475">`Trig`クラスは、関連するメソッドと定数、グループが、インスタンス化するものではありません。</span><span class="sxs-lookup"><span data-stu-id="be588-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="be588-1476">そのため、1 つの空のプライベート インスタンス コンス トラクターを宣言します。</span><span class="sxs-lookup"><span data-stu-id="be588-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="be588-1477">既定のコンス トラクターの自動生成を抑制するには、少なくとも 1 つのインスタンス コンス トラクターを宣言しなければなりません。</span><span class="sxs-lookup"><span data-stu-id="be588-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="be588-1478">省略可能なインスタンス コンス トラクターのパラメーター</span><span class="sxs-lookup"><span data-stu-id="be588-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="be588-1479">`this(...)`オプションのインスタンス コンス トラクターのパラメーターを実装するために、コンス トラクター初期化子の形式はオーバー ロードと組み合わせて使用一般的です。</span><span class="sxs-lookup"><span data-stu-id="be588-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="be588-1480">例</span><span class="sxs-lookup"><span data-stu-id="be588-1480">In the example</span></span>
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
<span data-ttu-id="be588-1481">最初の 2 つのインスタンス コンス トラクターは、だけで欠けている引数の既定値を指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="be588-1482">両方を使用して、`this(...)`を 3 つ目の新しいインスタンスを初期化中のインスタンス コンス トラクターを呼び出すコンス トラクター初期化子。</span><span class="sxs-lookup"><span data-stu-id="be588-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="be588-1483">省略可能なコンス トラクターのパラメーターの意味になります。</span><span class="sxs-lookup"><span data-stu-id="be588-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="be588-1484">静的コンストラクター</span><span class="sxs-lookup"><span data-stu-id="be588-1484">Static constructors</span></span>

<span data-ttu-id="be588-1485">A***静的コンス トラクター***が閉じられたクラス型の初期化に必要なアクションを実装するメンバー。</span><span class="sxs-lookup"><span data-stu-id="be588-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="be588-1486">使用して静的コンス トラクターが宣言された*static_constructor_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="be588-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="be588-1487">A *static_constructor_declaration*のセットを含めることができます*属性*([属性](attributes.md)) および`extern`修飾子 ([の外部メソッド](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="be588-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="be588-1488">*識別子*の*static_constructor_declaration*静的コンス トラクターが宣言されているクラスの名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="be588-1489">その他の任意の名前を指定すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="be588-1490">静的コンス トラクターの宣言が含まれています、`extern`修飾子は、静的コンス トラクターはモード、***外部の静的コンス トラクター***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="be588-1491">外部の静的コンス トラクターの宣言は、実際の実装を提供しないため、その*static_constructor_body*をセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="be588-1492">その他のすべての静的コンス トラクター宣言、 *static_constructor_body*から成る、*ブロック*クラスを初期化するために実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="be588-1493">これには対応、 *method_body*して静的メソッドの`void`型を返す ([メソッド本体](classes.md#method-body))。</span><span class="sxs-lookup"><span data-stu-id="be588-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="be588-1494">静的コンス トラクターは継承されませんし、直接呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="be588-1495">閉じられたクラス型の静的コンス トラクターは、特定のアプリケーション ドメインで最大で 1 回実行します。</span><span class="sxs-lookup"><span data-stu-id="be588-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="be588-1496">静的コンス トラクターの実行は、次のイベントの最初のアプリケーション ドメイン内に発生するによってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="be588-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="be588-1497">クラス型のインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="be588-1498">クラス型の静的メンバーのいずれかが参照されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="be588-1499">クラスが含まれている場合、`Main`メソッド ([アプリケーションの起動](basic-concepts.md#application-startup)) で、どの実行が開始される、静的コンス トラクターの前にそのクラスが実行される、`Main`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="be588-1500">最初の静的フィールドの新しいセットで閉じられたクラスの新しい型を初期化するために ([静的およびインスタンス フィールド](classes.md#static-and-instance-fields)) 特定のクローズ型を作成します。</span><span class="sxs-lookup"><span data-stu-id="be588-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="be588-1501">静的フィールドの各はその既定値に初期化 ([既定値](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="be588-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="be588-1502">次に、静的フィールド初期化子 ([静的フィールドの初期化](classes.md#static-field-initialization)) それらの静的フィールドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="be588-1503">最後に、静的コンス トラクターが実行されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="be588-1504">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-1504">The example</span></span>
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
<span data-ttu-id="be588-1505">出力を生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1505">must produce the output:</span></span>
```
Init A
A.F
Init B
B.F
```
<span data-ttu-id="be588-1506">の実行`A`の静的コンス トラクターがへの呼び出しによってトリガーされる`A.F`の実行と`B`の静的コンス トラクターがへの呼び出しによってトリガーされる`B.F`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="be588-1507">既定値の状態で見られる変数初期化子を含む静的フィールドの循環依存関係を構築することになります。</span><span class="sxs-lookup"><span data-stu-id="be588-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="be588-1508">例では、</span><span class="sxs-lookup"><span data-stu-id="be588-1508">The example</span></span>
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
<span data-ttu-id="be588-1509">この例では、次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1509">produces the output</span></span>
```
X = 1, Y = 2
```

<span data-ttu-id="be588-1510">実行する、`Main`メソッドをシステムを初めて実行の初期化子`B.Y`クラスを前に、`B`の静的コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="be588-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="be588-1511">`Y`初期化子によって`A`のために実行する静的コンス トラクターの値`A.X`参照されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="be588-1512">静的コンス トラクター `A`の値を計算に続いて `X`の既定値はフェッチそうすることで `Y`ゼロであります。</span><span class="sxs-lookup"><span data-stu-id="be588-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="be588-1513">`A.X` そのため 1 に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="be588-1514">実行中のプロセス`A`静的フィールド初期化子と静的コンス トラクターの後が完了したらの初期値の計算に返す `Y`これは 2 となります。</span><span class="sxs-lookup"><span data-stu-id="be588-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="be588-1515">実行時のチェック制約を使用してコンパイル時にチェックすることはできませんが、型パラメーターに適用する便利な場所は、静的コンス トラクターは、それぞれのクローズ構築されたクラス型の 1 回だけ実行は、ため、([型パラメーター制約](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="be588-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="be588-1516">たとえば、次の種類は、型引数が列挙型を強制するのに静的コンス トラクターを使用します。</span><span class="sxs-lookup"><span data-stu-id="be588-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="be588-1517">デストラクター</span><span class="sxs-lookup"><span data-stu-id="be588-1517">Destructors</span></span>

<span data-ttu-id="be588-1518">A***デストラクター***はクラスのインスタンスを消滅させるために必要なアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="be588-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="be588-1519">使用して、デストラクターを宣言、 *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="be588-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="be588-1520">A *destructor_declaration*のセットを含めることができます*属性*([属性](attributes.md))。</span><span class="sxs-lookup"><span data-stu-id="be588-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="be588-1521">*識別子*の*destructor_declaration*デストラクターが宣言されているクラスの名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="be588-1522">その他の任意の名前を指定すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="be588-1523">デストラクターの宣言が含まれています、`extern`修飾子は、デストラクターはモード、***外部デストラクター***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="be588-1524">外部デストラクターの宣言は、実際の実装を提供しないため、その*destructor_body*をセミコロンで構成されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="be588-1525">その他のすべてのデストラクターの*destructor_body*から成る、*ブロック*クラスのインスタンスを破棄するために実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="be588-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="be588-1526">A *destructor_body*に対応して、 *method_body*を持つインスタンス メソッドの`void`型を返す ([メソッド本体](classes.md#method-body))。</span><span class="sxs-lookup"><span data-stu-id="be588-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="be588-1527">デストラクターは継承されません。</span><span class="sxs-lookup"><span data-stu-id="be588-1527">Destructors are not inherited.</span></span> <span data-ttu-id="be588-1528">したがって、クラスでは、そのクラスで宣言されたもの以外のデストラクターがありません。</span><span class="sxs-lookup"><span data-stu-id="be588-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="be588-1529">デストラクターは、パラメーターがない必要があることはできません、オーバー ロードされたクラスに参照できるように、最大で 1 つのデストラクター。</span><span class="sxs-lookup"><span data-stu-id="be588-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="be588-1530">デストラクターは、自動的に呼び出され、明示的に呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="be588-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="be588-1531">インスタンスは、そのインスタンスを使用するには、どのコードが不要になったときに破壊の対象になります。</span><span class="sxs-lookup"><span data-stu-id="be588-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="be588-1532">インスタンスのデストラクターの実行のインスタンスが破壊の対象になると、いつでも発生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="be588-1533">順番に、そのインスタンスの継承チェーンでデストラクターが呼び出さインスタンスを破棄すると、ときにほとんどから派生する最低派生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="be588-1534">デストラクターは、任意のスレッドで実行する場合があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="be588-1535">タイミングとデストラクターの実行方法を制御するルールの詳細な説明については、[自動メモリ管理](basic-concepts.md#automatic-memory-management)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="be588-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="be588-1536">この例の出力</span><span class="sxs-lookup"><span data-stu-id="be588-1536">The output of the example</span></span>
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
<span data-ttu-id="be588-1537">is</span><span class="sxs-lookup"><span data-stu-id="be588-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="be588-1538">デストラクターは、継承チェーンでは、順序で呼び出されるのでほとんどから派生する最低派生します。</span><span class="sxs-lookup"><span data-stu-id="be588-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="be588-1539">デストラクターが仮想メソッドをオーバーライドすることによって実装される`Finalize`で`System.Object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="be588-1540">このメソッドをオーバーライドまたは (またはそのオーバーライド) を呼び出す c# プログラムは許可されていません直接します。</span><span class="sxs-lookup"><span data-stu-id="be588-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="be588-1541">たとえば、プログラム</span><span class="sxs-lookup"><span data-stu-id="be588-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="be588-1542">2 つのエラーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="be588-1542">contains two errors.</span></span>

<span data-ttu-id="be588-1543">コンパイラは、このメソッドをおよび、上書きがまったく存在しないかのように動作します。</span><span class="sxs-lookup"><span data-stu-id="be588-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="be588-1544">したがって、このプログラム:</span><span class="sxs-lookup"><span data-stu-id="be588-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="be588-1545">有効で非表示になりますが、メソッドに示すように、`System.Object`の`Finalize`メソッド。</span><span class="sxs-lookup"><span data-stu-id="be588-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="be588-1546">動作の詳細については、デストラクターから例外がスローされるを参照してください[例外の処理方法](exceptions.md#how-exceptions-are-handled)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="be588-1547">反復子</span><span class="sxs-lookup"><span data-stu-id="be588-1547">Iterators</span></span>

<span data-ttu-id="be588-1548">関数メンバー ([関数メンバー](expressions.md#function-members)) 反復子ブロックを使用して実装 ([ブロック](statements.md#blocks)) と呼ばれますが、***反復子***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="be588-1549">反復子ブロックは関数に対応するメンバーの戻り値の型は列挙子インターフェイスの 1 つとして、関数メンバーの本文として使用可能性があります ([列挙子インターフェイス](classes.md#enumerator-interfaces)) または列挙可能なインターフェイスの 1 つ ([列挙可能なインターフェイス](classes.md#enumerable-interfaces))。</span><span class="sxs-lookup"><span data-stu-id="be588-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="be588-1550">として発生する可能性が、 *method_body*、 *operator_body*または*accessor_body*イベント、インスタンス コンス トラクター、静的コンス トラクターおよびデストラクターがすることはできませんが、反復子として実装されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="be588-1551">反復子ブロックを使用して関数のメンバーが実装されている場合は、いずれかを指定するメンバー関数の仮パラメーター リストのコンパイル時エラー`ref`または`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="be588-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="be588-1552">列挙子インターフェイス</span><span class="sxs-lookup"><span data-stu-id="be588-1552">Enumerator interfaces</span></span>

<span data-ttu-id="be588-1553">***列挙子インターフェイス***非ジェネリック インターフェイスは`System.Collections.IEnumerator`ジェネリック インターフェイスのすべてのインスタンス化と`System.Collections.Generic.IEnumerator<T>`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="be588-1554">説明を簡潔にするため、この章ではこれらのインターフェイスとして参照されます`IEnumerator`と`IEnumerator<T>`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="be588-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="be588-1555">列挙可能なインターフェイス</span><span class="sxs-lookup"><span data-stu-id="be588-1555">Enumerable interfaces</span></span>

<span data-ttu-id="be588-1556">***列挙可能なインターフェイス***非ジェネリック インターフェイスは`System.Collections.IEnumerable`ジェネリック インターフェイスのすべてのインスタンス化と`System.Collections.Generic.IEnumerable<T>`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="be588-1557">説明を簡潔にするため、この章ではこれらのインターフェイスとして参照されます`IEnumerable`と`IEnumerable<T>`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="be588-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="be588-1558">Yield 型</span><span class="sxs-lookup"><span data-stu-id="be588-1558">Yield type</span></span>

<span data-ttu-id="be588-1559">反復子は、すべて同じ型の値のシーケンスを生成します。</span><span class="sxs-lookup"><span data-stu-id="be588-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="be588-1560">この型が呼び出される、***型を生成***の反復子。</span><span class="sxs-lookup"><span data-stu-id="be588-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="be588-1561">返す反復子の yield 型`IEnumerator`または`IEnumerable`は`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="be588-1562">返す反復子の yield 型`IEnumerator<T>`または`IEnumerable<T>`は`T`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="be588-1563">列挙子オブジェクト</span><span class="sxs-lookup"><span data-stu-id="be588-1563">Enumerator objects</span></span>

<span data-ttu-id="be588-1564">反復子ブロックを使用して列挙子インターフェイス型を返す関数のメンバーが実装されている関数メンバーを呼び出しても反復子ブロックのコードはすぐに実行されません。</span><span class="sxs-lookup"><span data-stu-id="be588-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="be588-1565">代わりに、***列挙子オブジェクト***が作成され、返されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="be588-1566">このオブジェクトは、反復子ブロックで指定されたコードをカプセル化し、反復子ブロック内のコードの実行時に、列挙子オブジェクトの`MoveNext`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="be588-1567">列挙子オブジェクトには、次の特徴があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="be588-1568">実装`IEnumerator`と`IEnumerator<T>`ここで、`T`反復子の yield 型です。</span><span class="sxs-lookup"><span data-stu-id="be588-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="be588-1569">このクラスは、`System.IDisposable` を実装します。</span><span class="sxs-lookup"><span data-stu-id="be588-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="be588-1570">引数の値のコピーの初期化 (ある場合) および関数メンバーに渡されるインスタンスの値。</span><span class="sxs-lookup"><span data-stu-id="be588-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="be588-1571">4 つの潜在的な状態が***する前に***、***を実行している***、***中断***、および***後***、最初に、***する前に***状態。</span><span class="sxs-lookup"><span data-stu-id="be588-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="be588-1572">列挙子オブジェクトは、通常、反復子ブロック内のコードをカプセル化し、列挙子のインターフェイスを実装する列挙子のコンパイラによって生成されたクラスのインスタンスが他のメソッドの実装が可能です。</span><span class="sxs-lookup"><span data-stu-id="be588-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="be588-1573">場合は、列挙子クラスは、コンパイラによって生成される、そのクラスは入れ子になった、直接的または間接的に、関数メンバーを含むクラスで private のアクセシビリティがおよびコンパイラを使用するために予約された名前になります ([識別子](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="be588-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="be588-1574">列挙子オブジェクトには、上記で指定したものより多くのインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="be588-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="be588-1575">次のセクションでは、説明の正確な動作、 `MoveNext`、`Current`と`Dispose`のメンバー、`IEnumerable`と`IEnumerable<T>`インターフェイスの列挙子オブジェクトによって提供される実装です。</span><span class="sxs-lookup"><span data-stu-id="be588-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="be588-1576">列挙子オブジェクトはサポートされないことに注意してください、`IEnumerator.Reset`メソッド。</span><span class="sxs-lookup"><span data-stu-id="be588-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="be588-1577">このメソッドを呼び出すと、`System.NotSupportedException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="be588-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="be588-1578">MoveNext メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-1578">The MoveNext method</span></span>

<span data-ttu-id="be588-1579">`MoveNext`列挙子オブジェクトのメソッドは、反復子ブロックのコードをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="be588-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="be588-1580">呼び出す、`MoveNext`メソッド、反復子ブロックとセット内のコードの実行、`Current`として適切な列挙子オブジェクトのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="be588-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="be588-1581">実行した正確な動作`MoveNext`列挙子オブジェクトの状態によって異なるとき`MoveNext`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="be588-1582">列挙子オブジェクトの状態が場合***する前に***呼び出すと、 `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="be588-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="be588-1583">状態を変更して***を実行している***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="be588-1584">パラメーターを初期化します (など`this`) 引数の値とインスタンス値の列挙子オブジェクトが初期化されたときに保存するには、反復子ブロックの。</span><span class="sxs-lookup"><span data-stu-id="be588-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="be588-1585">(後述) の実行が中断されるまでは、反復子ブロックを最初から実行します。</span><span class="sxs-lookup"><span data-stu-id="be588-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="be588-1586">列挙子オブジェクトの状態が場合***を実行している***、呼び出しの結果`MoveNext`が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="be588-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="be588-1587">列挙子オブジェクトの状態が場合***中断***呼び出すと、 `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="be588-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="be588-1588">状態を変更して***を実行している***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="be588-1589">最後に、反復子ブロックの実行が中断したときに保存する値には、すべてのローカル変数とパラメーター (これを含む) の値を復元します。</span><span class="sxs-lookup"><span data-stu-id="be588-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="be588-1590">これらの変数によって参照される任意のオブジェクトの内容は、MoveNext の以前の呼び出し以降に変更された可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="be588-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="be588-1591">直後に、次の反復子ブロックの実行を再開、`yield return`ステートメントを実行の中断の原因となった、(後述) の実行が中断されるまで継続します。</span><span class="sxs-lookup"><span data-stu-id="be588-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="be588-1592">列挙子オブジェクトの状態が場合***後***呼び出すと、`MoveNext`返します`false`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="be588-1593">ときに`MoveNext`反復子ブロックを実行します。 4 つの方法で実行を中断できます。によって、`yield return`ステートメントにより、`yield break`ステートメントでは、例外と、反復子ブロックの最後の発生によってがスローされ、反復子ブロックの外部で伝達されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="be588-1594">ときに、`yield return`ステートメントが見つかりました ([yield ステートメント](statements.md#the-yield-statement))。</span><span class="sxs-lookup"><span data-stu-id="be588-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="be588-1595">ステートメントで指定された式が評価、暗黙的に、yield 型に変換されに割り当てられている、`Current`列挙子オブジェクトのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="be588-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="be588-1596">反復子本体の実行が中断されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="be588-1597">すべてのローカル変数とパラメーターの値 (など`this`) のこの場所は、保存は`yield return`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="be588-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="be588-1598">場合、`yield return`内で 1 つまたは複数のステートメントが`try`ブロック、関連付けられている`finally`ブロックは、この時点では実行されません。</span><span class="sxs-lookup"><span data-stu-id="be588-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="be588-1599">列挙子オブジェクトの状態に変更***中断***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="be588-1600">`MoveNext`メソッドを返します。`true`イテレーションは、次の値に正常に進んだことを示す、呼び出し元にします。</span><span class="sxs-lookup"><span data-stu-id="be588-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="be588-1601">ときに、`yield break`ステートメントが見つかりました ([yield ステートメント](statements.md#the-yield-statement))。</span><span class="sxs-lookup"><span data-stu-id="be588-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="be588-1602">場合、`yield break`内で 1 つまたは複数のステートメントが`try`ブロック、関連付けられている`finally`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="be588-1603">列挙子オブジェクトの状態に変更***後***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="be588-1604">`MoveNext`メソッドを返します。`false`イテレーションが完了したことを示す、呼び出し元にします。</span><span class="sxs-lookup"><span data-stu-id="be588-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="be588-1605">ときに、反復子本体の末尾が発生しました。</span><span class="sxs-lookup"><span data-stu-id="be588-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="be588-1606">列挙子オブジェクトの状態に変更***後***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="be588-1607">`MoveNext`メソッドを返します。`false`イテレーションが完了したことを示す、呼び出し元にします。</span><span class="sxs-lookup"><span data-stu-id="be588-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="be588-1608">ときに例外がスローされ、反復子ブロックの外部で伝達します。</span><span class="sxs-lookup"><span data-stu-id="be588-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="be588-1609">適切な`finally`反復子本体のブロックが例外の反映によって実行されるは。</span><span class="sxs-lookup"><span data-stu-id="be588-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="be588-1610">列挙子オブジェクトの状態に変更***後***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="be588-1611">例外の反映の呼び出し元は引き続き、`MoveNext`メソッド。</span><span class="sxs-lookup"><span data-stu-id="be588-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="be588-1612">現在のプロパティ</span><span class="sxs-lookup"><span data-stu-id="be588-1612">The Current property</span></span>

<span data-ttu-id="be588-1613">列挙子オブジェクトの`Current`によってプロパティの影響を受ける`yield return`反復子ブロック内のステートメント。</span><span class="sxs-lookup"><span data-stu-id="be588-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="be588-1614">列挙子オブジェクトの場合、***中断***状態では、値`Current`以前の呼び出しによって設定された値は、`MoveNext`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="be588-1615">列挙子オブジェクトの場合、***する前に***、***を実行している***、または***後***へのアクセスの結果が示す`Current`が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="be588-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="be588-1616">以外の入力を yield を使った反復子のため`object`へのアクセス結果`Current`で列挙子オブジェクトの`IEnumerable`へのアクセスに対応する実装`Current`で列挙子オブジェクトの`IEnumerator<T>`実装と、その結果をキャスト`object`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="be588-1617">Dispose メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-1617">The Dispose method</span></span>

<span data-ttu-id="be588-1618">`Dispose`メソッドを使用列挙子オブジェクトを導入することで、イテレーションのクリーンアップを***後***状態。</span><span class="sxs-lookup"><span data-stu-id="be588-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="be588-1619">列挙子オブジェクトの状態が場合***する前に***呼び出すと、`Dispose`状態を変更して***後***。</span><span class="sxs-lookup"><span data-stu-id="be588-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="be588-1620">列挙子オブジェクトの状態が場合***を実行している***、呼び出しの結果`Dispose`が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="be588-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="be588-1621">列挙子オブジェクトの状態が場合***中断***呼び出すと、 `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="be588-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="be588-1622">状態を変更して***を実行している***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="be588-1623">いずれかの finally ブロックを実行、最後に実行された場合、`yield return`ステートメントが、`yield break`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="be588-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="be588-1624">これによって、例外がスローされ、反復子本体の外部で伝達する場合、列挙子オブジェクトの状態に設定されて***後***の呼び出し元に例外が伝達されると、`Dispose`メソッド。</span><span class="sxs-lookup"><span data-stu-id="be588-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="be588-1625">状態を変更して***後***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="be588-1626">列挙子オブジェクトの状態が場合***後***呼び出すと、`Dispose`効力はなくなりました。</span><span class="sxs-lookup"><span data-stu-id="be588-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="be588-1627">列挙可能なオブジェクト</span><span class="sxs-lookup"><span data-stu-id="be588-1627">Enumerable objects</span></span>

<span data-ttu-id="be588-1628">反復子ブロックを使用して、列挙可能なインターフェイスの型を返す関数のメンバーが実装されている関数メンバーを呼び出しても反復子ブロックのコードはすぐに実行されません。</span><span class="sxs-lookup"><span data-stu-id="be588-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="be588-1629">代わりに、***列挙可能なオブジェクト***が作成され、返されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="be588-1630">列挙可能なオブジェクトの`GetEnumerator`メソッドを返します、反復子ブロックで指定された列挙子オブジェクトをコードをカプセル化して、反復子ブロック内のコードの実行が発生したときに列挙子オブジェクトの`MoveNext`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="be588-1631">列挙可能なオブジェクトには、次の特徴があります。</span><span class="sxs-lookup"><span data-stu-id="be588-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="be588-1632">実装`IEnumerable`と`IEnumerable<T>`ここで、`T`反復子の yield 型です。</span><span class="sxs-lookup"><span data-stu-id="be588-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="be588-1633">引数の値のコピーの初期化 (ある場合) および関数メンバーに渡されるインスタンスの値。</span><span class="sxs-lookup"><span data-stu-id="be588-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="be588-1634">列挙可能なオブジェクトは、通常の反復子ブロック内のコードをカプセル化し、列挙可能なインターフェイスを実装するための列挙可能なクラスがコンパイラによって生成されたインスタンスが、他のメソッドの実装が可能です。</span><span class="sxs-lookup"><span data-stu-id="be588-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="be588-1635">場合は、列挙可能なクラスは、コンパイラによって生成される、そのクラスが入れ子にする、直接または間接的には、関数メンバーを含むクラスで private のアクセシビリティがおよびコンパイラを使用するために予約された名前になります ([識別子](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="be588-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="be588-1636">列挙可能なオブジェクトは、上記で指定したものよりもいないインターフェイスを実装することができます。</span><span class="sxs-lookup"><span data-stu-id="be588-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="be588-1637">具体的には、列挙可能なオブジェクトも実装`IEnumerator`と`IEnumerator<T>`、列挙型と列挙子の両方として機能するようにします。</span><span class="sxs-lookup"><span data-stu-id="be588-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="be588-1638">このような実装では、最初の時間、列挙可能なオブジェクトの`GetEnumerator`メソッドが呼び出される列挙可能なオブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="be588-1639">列挙可能なオブジェクトの後に呼び出さ`GetEnumerator`列挙可能なオブジェクトのコピーを返す場合は、します。</span><span class="sxs-lookup"><span data-stu-id="be588-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="be588-1640">したがって、各エラーは、列挙子が独自の状態と、1 つの列挙子で変更が別に影響することはありません返されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="be588-1641">GetEnumerator メソッド</span><span class="sxs-lookup"><span data-stu-id="be588-1641">The GetEnumerator method</span></span>

<span data-ttu-id="be588-1642">列挙可能なオブジェクトの実装を提供する、`GetEnumerator`のメソッド、`IEnumerable`と`IEnumerable<T>`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="be588-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="be588-1643">2 つ`GetEnumerator`メソッドを取得し、使用可能な列挙子オブジェクトを取得する一般的な実装を共有します。</span><span class="sxs-lookup"><span data-stu-id="be588-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="be588-1644">列挙子オブジェクトが引数の値で初期化され、インスタンスの列挙可能なオブジェクトが初期化されますが、それ以外の場合に保存された値で説明されているように機能[列挙子オブジェクト](classes.md#enumerator-objects)します。</span><span class="sxs-lookup"><span data-stu-id="be588-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="be588-1645">実装例</span><span class="sxs-lookup"><span data-stu-id="be588-1645">Implementation example</span></span>

<span data-ttu-id="be588-1646">このセクションでは、標準 c# コンストラクトの観点からの反復子の実装について説明します。</span><span class="sxs-lookup"><span data-stu-id="be588-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="be588-1647">ここで説明されている実装 Microsoft c# コンパイラで使用される同じ原則に基づいていますが、規制に準拠した実装または使用可能な 1 つだけではではありません。</span><span class="sxs-lookup"><span data-stu-id="be588-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="be588-1648">次`Stack<T>`クラスが実装するその`GetEnumerator`メソッド、反復子を使用します。</span><span class="sxs-lookup"><span data-stu-id="be588-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="be588-1649">反復子は、上から下へのスタックの要素を列挙します。</span><span class="sxs-lookup"><span data-stu-id="be588-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="be588-1650">`GetEnumerator`メソッドは、次に示すように、反復子ブロックでコードをカプセル化する列挙子のコンパイラによって生成されたクラスのインスタンス化に変換できます。</span><span class="sxs-lookup"><span data-stu-id="be588-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="be588-1651">反復子ブロック内のコードをステート マシンに変換されに配置前の変換で、`MoveNext`列挙子クラスのメソッド。</span><span class="sxs-lookup"><span data-stu-id="be588-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="be588-1652">さらに、ローカル変数`i`の呼び出し間での存在を続行できるように、列挙子オブジェクトのフィールドになって`MoveNext`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="be588-1653">次の例では、整数 1 ~ 10 の単純な掛け算表を印刷します。</span><span class="sxs-lookup"><span data-stu-id="be588-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="be588-1654">`FromTo`メソッドの例では列挙可能なオブジェクトを返し、反復子を使用して実装されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="be588-1655">`FromTo`メソッドは、次に示すように、反復子ブロック内のコードをカプセル化するための列挙可能なクラスがコンパイラによって生成されたインスタンス化に変換できます。</span><span class="sxs-lookup"><span data-stu-id="be588-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="be588-1656">列挙可能なクラスでは、列挙可能なインターフェイスと列挙型および列挙子の両方として機能できるように、列挙子インターフェイスの両方を実装します。</span><span class="sxs-lookup"><span data-stu-id="be588-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="be588-1657">最初に、`GetEnumerator`メソッドが呼び出される列挙可能なオブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="be588-1658">列挙可能なオブジェクトの後に呼び出さ`GetEnumerator`列挙可能なオブジェクトのコピーを返す場合は、します。</span><span class="sxs-lookup"><span data-stu-id="be588-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="be588-1659">したがって、各エラーは、列挙子が独自の状態と、1 つの列挙子で変更が別に影響することはありません返されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="be588-1660">`Interlocked.CompareExchange`メソッドを使用して、スレッド セーフ操作を確認します。</span><span class="sxs-lookup"><span data-stu-id="be588-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="be588-1661">`from`と`to`パラメーターは列挙可能なクラスのフィールドに変換されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="be588-1662">`from`反復子ブロックでは、追加で変更が`__from`に指定された初期値を保持するフィールドが導入されました`from`各列挙子にします。</span><span class="sxs-lookup"><span data-stu-id="be588-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="be588-1663">`MoveNext`メソッドがスローされます、`InvalidOperationException`ときに呼び出された場合`__state`は`0`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="be588-1664">これで最初に呼び出さず、列挙子オブジェクトとしての列挙可能なオブジェクトの使用に対して保護`GetEnumerator`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="be588-1665">次の例では、単純なツリーのクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="be588-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="be588-1666">`Tree<T>`クラスが実装するその`GetEnumerator`メソッド、反復子を使用します。</span><span class="sxs-lookup"><span data-stu-id="be588-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="be588-1667">反復子は、infix ツリーの要素を列挙します。</span><span class="sxs-lookup"><span data-stu-id="be588-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="be588-1668">`GetEnumerator`メソッドは、次に示すように、反復子ブロックでコードをカプセル化する列挙子のコンパイラによって生成されたクラスのインスタンス化に変換できます。</span><span class="sxs-lookup"><span data-stu-id="be588-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="be588-1669">使用されるコンパイラによって生成された一時、`foreach`にステートメントが無効になる、`__left`と`__right`列挙子オブジェクトのフィールド。</span><span class="sxs-lookup"><span data-stu-id="be588-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="be588-1670">`__state`列挙子オブジェクトのフィールドは、慎重に更新できるように、正しい`Dispose()`メソッドが正しく呼び出される場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="be588-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="be588-1671">翻訳された単純なコードを記述することはできませんに注意してください。`foreach`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="be588-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="be588-1672">非同期関数</span><span class="sxs-lookup"><span data-stu-id="be588-1672">Async functions</span></span>

<span data-ttu-id="be588-1673">メソッド ([メソッド](classes.md#methods)) または匿名関数 ([匿名関数式](expressions.md#anonymous-function-expressions)) で、`async`修飾子と呼ばれる、***非同期関数***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="be588-1674">一般に、用語***async***任意の種類を持つ関数の記述に使用される、`async`修飾子。</span><span class="sxs-lookup"><span data-stu-id="be588-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="be588-1675">いずれかを指定する非同期関数の仮パラメーター リストと、コンパイル時エラー`ref`または`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="be588-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="be588-1676">*Return_type* 、非同期のメソッドはいずれかのことがあります`void`または***タスクの種類***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="be588-1677">タスクの種類は`System.Threading.Tasks.Task`から型を構築および`System.Threading.Tasks.Task<T>`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="be588-1678">説明を簡潔にするため、この章ではこれらの型として参照されます`Task`と`Task<T>`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="be588-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="be588-1679">タスク型を返す非同期メソッドは、タスクを返すと言います。</span><span class="sxs-lookup"><span data-stu-id="be588-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="be588-1680">タスクの種類の正確な定義が定義されている、実装が、言語の観点から、タスクの型がのいずれか、不完全な状態が成功したか、エラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="be588-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="be588-1681">エラーが発生したタスクは、関連する例外を記録します。</span><span class="sxs-lookup"><span data-stu-id="be588-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="be588-1682">成功、`Task<T>`型の結果を記録`T`します。</span><span class="sxs-lookup"><span data-stu-id="be588-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="be588-1683">タスクの種類は待機可能なとのオペランドは、そのため、await 式 ([Await 式](expressions.md#await-expressions))。</span><span class="sxs-lookup"><span data-stu-id="be588-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="be588-1684">非同期関数の呼び出しが評価を停止することでは、await 式 ([Await 式](expressions.md#await-expressions)) 本体内にします。</span><span class="sxs-lookup"><span data-stu-id="be588-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="be588-1685">評価を中断時点では後で再開が await 式で、***再開デリゲート***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="be588-1686">再開のデリゲートは型`System.Action`、非同期関数の呼び出しの評価が箇所の await 式から再開されるメソッドが呼び出されたときにします。</span><span class="sxs-lookup"><span data-stu-id="be588-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="be588-1687">***現在の呼び出し元***非同期関数の呼び出しは最初の呼び出し元関数の呼び出しは中断されていることはない場合、または再開デリゲートの最新の呼び出し元それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="be588-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="be588-1688">非同期タスクを返す関数の評価</span><span class="sxs-lookup"><span data-stu-id="be588-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="be588-1689">タスクを返す非同期関数の呼び出しにより、生成するタスクが返される型のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="be588-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="be588-1690">これと呼ばれますが、***タスクを返す***の async 関数。</span><span class="sxs-lookup"><span data-stu-id="be588-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="be588-1691">タスクは、不完全な状態の初期段階です。</span><span class="sxs-lookup"><span data-stu-id="be588-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="be588-1692">非同期関数の本体は評価されます (await 式に到達) によって中断されていたか、またはが終了するまでタスクを返すと、呼び出し元にコントロールのポイントが返されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="be588-1693">非同期関数の本体が終了すると、タスクを返すに不完全な状態から移動されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="be588-1694">Return ステートメントまたは本体の末尾に到達の結果として、関数本体が終了すると、成功の状態に、戻り値のタスクで、結果の値が記録されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="be588-1695">キャッチされない例外の結果として、関数本体が終了しているかどうか ([throw ステートメント](statements.md#the-throw-statement)) エラーが発生した状態に戻り、タスクに例外が記録されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="be588-1696">Async void を返す関数の評価</span><span class="sxs-lookup"><span data-stu-id="be588-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="be588-1697">非同期関数の戻り値の型がある場合`void`、次のように、上記とは異なる評価。関数が完了と、現在のスレッドの例外に伝える代わりにタスクが返されないため***同期コンテキスト***します。</span><span class="sxs-lookup"><span data-stu-id="be588-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="be588-1698">同期コンテキストの正確な定義は実装に依存しますが、表現である"where"現在のスレッドが実行されているのです。</span><span class="sxs-lookup"><span data-stu-id="be588-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="be588-1699">同期コンテキストは、async void を返す関数の評価の開始が正常に完了すると、またはキャッチされない例外がスローされると、ときに通知されます。</span><span class="sxs-lookup"><span data-stu-id="be588-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="be588-1700">これにより、コンテキスト、その下にある数の void を返す非同期関数が実行されているを追跡して、それらから例外を伝達する方法を決定できます。</span><span class="sxs-lookup"><span data-stu-id="be588-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
