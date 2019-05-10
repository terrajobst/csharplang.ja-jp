---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488774"
---
# <a name="interfaces"></a><span data-ttu-id="2a12a-101">インターフェイス</span><span class="sxs-lookup"><span data-stu-id="2a12a-101">Interfaces</span></span>

<span data-ttu-id="2a12a-102">インターフェイスは、コントラクトを定義します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-102">An interface defines a contract.</span></span> <span data-ttu-id="2a12a-103">クラスまたはインターフェイスを実装する構造体は、そのコントラクトに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-103">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="2a12a-104">複数の基底インターフェイスから継承でき、クラスまたは構造体には、複数のインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-104">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="2a12a-105">インターフェイスは、メソッド、プロパティ、イベント、およびインデクサーに含めることができます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-105">Interfaces can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="2a12a-106">インターフェイス自体では、定義するメンバーの実装は提供されません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-106">The interface itself does not provide implementations for the members that it defines.</span></span> <span data-ttu-id="2a12a-107">インターフェイスには、クラスまたはインターフェイスを実装する構造体を渡す必要があるメンバーだけを指定します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-107">The interface merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

## <a name="interface-declarations"></a><span data-ttu-id="2a12a-108">インターフェイスの宣言</span><span class="sxs-lookup"><span data-stu-id="2a12a-108">Interface declarations</span></span>

<span data-ttu-id="2a12a-109">*Interface_declaration*は、 *type_declaration* ([入力宣言](namespaces.md#type-declarations)) 新しいインターフェイスの型を宣言します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-109">An *interface_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new interface type.</span></span>

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

<span data-ttu-id="2a12a-110">*Interface_declaration*オプションのセットから成る*属性*([属性](attributes.md)) オプションのセットと、その後*interface_modifier*s ([修飾子をインターフェイス](interfaces.md#interface-modifiers)) と省略可能なその後`partial`の後に、キーワード修飾子`interface`と*識別子*インターフェイスの名前を示す続けて、省略可能な*variant_type_parameter_list*仕様 ([バリアント型パラメーター リスト](interfaces.md#variant-type-parameter-lists)) と省略可能なその後*interface_base*仕様 ([基本インターフェイス](interfaces.md#base-interfaces)) と省略可能なその後*type_parameter_constraints_clause*s 仕様 ([パラメーターの制約入力](classes.md#type-parameter-constraints))、続けて、 *interface_body* ([インターフェイス本体](interfaces.md#interface-body))、セミコロンで必要に応じてその後にします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-110">An *interface_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *interface_modifier*s ([Interface modifiers](interfaces.md#interface-modifiers)), followed by an optional `partial` modifier, followed by the keyword `interface` and an *identifier* that names the interface, followed by an optional *variant_type_parameter_list* specification ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)), followed by an optional *interface_base* specification ([Base interfaces](interfaces.md#base-interfaces)), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by an *interface_body* ([Interface body](interfaces.md#interface-body)), optionally followed by a semicolon.</span></span>

### <a name="interface-modifiers"></a><span data-ttu-id="2a12a-111">修飾子をインターフェイスします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-111">Interface modifiers</span></span>

<span data-ttu-id="2a12a-112">*Interface_declaration*インターフェイス修飾子のシーケンスを必要に応じて含めることができます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-112">An *interface_declaration* may optionally include a sequence of interface modifiers:</span></span>

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

<span data-ttu-id="2a12a-113">同じ修飾子をインターフェイスの宣言内で複数回のコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-113">It is a compile-time error for the same modifier to appear multiple times in an interface declaration.</span></span>

<span data-ttu-id="2a12a-114">`new`修飾子がクラス内で定義されているインターフェイスでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-114">The `new` modifier is only permitted on interfaces defined within a class.</span></span> <span data-ttu-id="2a12a-115">指定します、インターフェイスが、同じ名前で、継承されたメンバーを非表示にする」の説明に従って[new 修飾子](classes.md#the-new-modifier)します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-115">It specifies that the interface hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="2a12a-116">`public`、 `protected`、 `internal`、および`private`修飾子は、インターフェイスのアクセシビリティを制御します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the interface.</span></span> <span data-ttu-id="2a12a-117">インターフェイスの宣言が発生したコンテキストに応じてこれらの修飾子の一部のみできない場合があります ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="2a12a-117">Depending on the context in which the interface declaration occurs, only some of these modifiers may be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="2a12a-118">Partial 識別子</span><span class="sxs-lookup"><span data-stu-id="2a12a-118">Partial modifier</span></span>

<span data-ttu-id="2a12a-119">`partial`修飾子には、ことを示しますこの*interface_declaration*部分型の宣言です。</span><span class="sxs-lookup"><span data-stu-id="2a12a-119">The `partial` modifier indicates that this *interface_declaration* is a partial type declaration.</span></span> <span data-ttu-id="2a12a-120">指定された規則に従って、それを囲む名前空間または型の宣言内に同じ名前を持つ複数の部分的なインターフェイス宣言は、フォームの 1 つのインターフェイスの宣言に結合、[部分型](classes.md#partial-types)します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-120">Multiple partial interface declarations with the same name within an enclosing namespace or type declaration combine to form one interface declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="variant-type-parameter-lists"></a><span data-ttu-id="2a12a-121">バリアント型パラメーター リスト</span><span class="sxs-lookup"><span data-stu-id="2a12a-121">Variant type parameter lists</span></span>

<span data-ttu-id="2a12a-122">バリアント型パラメーター リストは、インターフェイスとデリゲート型でのみ実行できます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-122">Variant type parameter lists can only occur on interface and delegate types.</span></span> <span data-ttu-id="2a12a-123">通常の違い*type_parameter_list*s は省略可能な*variance_annotation*型パラメーターごとにします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-123">The difference from ordinary *type_parameter_list*s is the optional *variance_annotation* on each type parameter.</span></span>

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

<span data-ttu-id="2a12a-124">分散注釈が場合`out`、型パラメーターはモード***共変***します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-124">If the variance annotation is `out`, the type parameter is said to be ***covariant***.</span></span> <span data-ttu-id="2a12a-125">分散注釈が場合`in`、型パラメーターはモード***反変***します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-125">If the variance annotation is `in`, the type parameter is said to be ***contravariant***.</span></span> <span data-ttu-id="2a12a-126">分散注釈がない場合は、型パラメーターと呼ばれます***インバリアント***します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-126">If there is no variance annotation, the type parameter is said to be ***invariant***.</span></span>

<span data-ttu-id="2a12a-127">例</span><span class="sxs-lookup"><span data-stu-id="2a12a-127">In the example</span></span>
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
<span data-ttu-id="2a12a-128">`X` 共変性を持つ`Y`は反変、`Z`バリアントではありません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-128">`X` is covariant, `Y` is contravariant and `Z` is invariant.</span></span>

#### <a name="variance-safety"></a><span data-ttu-id="2a12a-129">差異の安全性</span><span class="sxs-lookup"><span data-stu-id="2a12a-129">Variance safety</span></span>

<span data-ttu-id="2a12a-130">型の型パラメーター リスト内の分散注釈の発生は、型宣言内で種類できますが発生した場所を制限します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-130">The occurrence of variance annotations in the type parameter list of a type restricts the places where types can occur within the type declaration.</span></span>

<span data-ttu-id="2a12a-131">型`T`は***出力の安全でない***次のいずれかが保持している場合。</span><span class="sxs-lookup"><span data-stu-id="2a12a-131">A type `T` is ***output-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="2a12a-132">`T` 反変の型パラメーターには</span><span class="sxs-lookup"><span data-stu-id="2a12a-132">`T` is a contravariant type parameter</span></span>
*  <span data-ttu-id="2a12a-133">`T` 出力の安全でない要素の型の配列型には</span><span class="sxs-lookup"><span data-stu-id="2a12a-133">`T` is an array type with an output-unsafe element type</span></span>
*  <span data-ttu-id="2a12a-134">`T` インターフェイスまたはデリゲート型`S<A1,...,Ak>`ジェネリック型から構築された`S<X1,...,Xk>`少なくとも 1 つの where`Ai`次のいずれかを保持します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-134">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="2a12a-135">`Xi` 共変またはインバリアントと`Ai`は出力の安全でないです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-135">`Xi` is covariant or invariant and `Ai` is output-unsafe.</span></span>
   * <span data-ttu-id="2a12a-136">`Xi` 反変か不変条件と`Ai`入力セーフです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-136">`Xi` is contravariant or invariant and `Ai` is input-safe.</span></span>
   
<span data-ttu-id="2a12a-137">型`T`は***入力の安全でない***次のいずれかが保持している場合。</span><span class="sxs-lookup"><span data-stu-id="2a12a-137">A type `T` is ***input-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="2a12a-138">`T` 共変の型パラメーターには</span><span class="sxs-lookup"><span data-stu-id="2a12a-138">`T` is a covariant type parameter</span></span>
*  <span data-ttu-id="2a12a-139">`T` 入力の安全でない要素の型の配列型には</span><span class="sxs-lookup"><span data-stu-id="2a12a-139">`T` is an array type with an input-unsafe element type</span></span>
*  <span data-ttu-id="2a12a-140">`T` インターフェイスまたはデリゲート型`S<A1,...,Ak>`ジェネリック型から構築された`S<X1,...,Xk>`少なくとも 1 つの where`Ai`次のいずれかを保持します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-140">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="2a12a-141">`Xi` 共変またはインバリアントと`Ai`は入力の安全でないです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-141">`Xi` is covariant or invariant and `Ai` is input-unsafe.</span></span>
   * <span data-ttu-id="2a12a-142">`Xi` 反変か不変条件と`Ai`は出力の安全でないです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-142">`Xi` is contravariant or invariant and `Ai` is output-unsafe.</span></span>

<span data-ttu-id="2a12a-143">直感的な出力の安全でない型が、出力位置で禁止されているし、入力位置に、入力の安全でない型は禁止されています。</span><span class="sxs-lookup"><span data-stu-id="2a12a-143">Intuitively, an output-unsafe type is prohibited in an output position, and an input-unsafe type is prohibited in an input position.</span></span>

<span data-ttu-id="2a12a-144">型が***出力セーフ***出力 unsafe でない場合、***入力の安全な***ない入力の安全でない場合。</span><span class="sxs-lookup"><span data-stu-id="2a12a-144">A type is ***output-safe*** if it is not output-unsafe, and ***input-safe*** if it is not input-unsafe.</span></span>

#### <a name="variance-conversion"></a><span data-ttu-id="2a12a-145">差異の変換</span><span class="sxs-lookup"><span data-stu-id="2a12a-145">Variance conversion</span></span>

<span data-ttu-id="2a12a-146">分散注釈では、インターフェイスとデリゲート型に (ただし、まだタイプ セーフ) より緩やかな変換を提供します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-146">The purpose of variance annotations is to provide for more lenient (but still type safe) conversions to interface and delegate types.</span></span> <span data-ttu-id="2a12a-147">この暗黙の定義を終了します ([暗黙的な変換](conversions.md#implicit-conversions)) と明示的な変換 ([明示的な変換](conversions.md#explicit-conversions)) ことを次のように定義されている分散でもの概念の使用します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-147">To this end the definitions of implicit ([Implicit conversions](conversions.md#implicit-conversions)) and explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) make use of the notion of variance-convertibility, which is defined as follows:</span></span>

<span data-ttu-id="2a12a-148">型`T<A1,...,An>`分散変換できる型に`T<B1,...,Bn>`場合`T`バリアント型パラメーターでは、インターフェイスまたはデリゲート型宣言`T<X1,...,Xn>`、および各バリアント型パラメーターの`Xi`次のいずれか保持されています。</span><span class="sxs-lookup"><span data-stu-id="2a12a-148">A type `T<A1,...,An>` is variance-convertible to a type `T<B1,...,Bn>` if `T` is either an interface or a delegate type declared with the variant type parameters `T<X1,...,Xn>`, and for each variant type parameter `Xi` one of the following holds:</span></span>

*  <span data-ttu-id="2a12a-149">`Xi` 共変およびから暗黙の参照または id 変換が存在する`Ai`に `Bi`</span><span class="sxs-lookup"><span data-stu-id="2a12a-149">`Xi` is covariant and an implicit reference or identity conversion exists from `Ai` to `Bi`</span></span>
*  <span data-ttu-id="2a12a-150">`Xi` 反変、暗黙的な参照またはから id 変換が存在する`Bi`に `Ai`</span><span class="sxs-lookup"><span data-stu-id="2a12a-150">`Xi` is contravariant and an implicit reference or identity conversion exists from `Bi` to `Ai`</span></span>
*  <span data-ttu-id="2a12a-151">`Xi` 不変性は、id からの変換が存在する`Ai`に `Bi`</span><span class="sxs-lookup"><span data-stu-id="2a12a-151">`Xi` is invariant and an identity conversion exists from `Ai` to `Bi`</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="2a12a-152">基底インターフェイス</span><span class="sxs-lookup"><span data-stu-id="2a12a-152">Base interfaces</span></span>

<span data-ttu-id="2a12a-153">インターフェイスと呼ばれる、0 個以上のインターフェイス型から継承できます、***基底インターフェイスの明示的な***のインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="2a12a-153">An interface can inherit from zero or more interface types, which are called the ***explicit base interfaces*** of the interface.</span></span> <span data-ttu-id="2a12a-154">インターフェイスの 1 つまたは複数の明示的な基本インターフェイスの場合、そのインターフェイスの宣言インターフェイス識別子の後にコロンとコンマ区切り基底インターフェイスの種類の一覧。</span><span class="sxs-lookup"><span data-stu-id="2a12a-154">When an interface has one or more explicit base interfaces, then in the declaration of that interface, the interface identifier is followed by a colon and a comma separated list of base interface types.</span></span>

```antlr
interface_base
    : ':' interface_type_list
    ;
```

<span data-ttu-id="2a12a-155">ジェネリック型の宣言に基本インターフェイスの明示的な宣言を取得し、それぞれを代入して構築されたインターフェイス型の場合、明示的な基本インターフェイスが形成されます*type_parameter*基本インターフェイスで対応する宣言*type_argument*構築された型のです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-155">For a constructed interface type, the explicit base interfaces are formed by taking the explicit base interface declarations on the generic type declaration, and substituting, for each *type_parameter* in the base interface declaration, the corresponding *type_argument* of the constructed type.</span></span>

<span data-ttu-id="2a12a-156">インターフェイスの明示的な基本インターフェイスは、少なくともインターフェイス自体と同程度にアクセスである必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="2a12a-156">The explicit base interfaces of an interface must be at least as accessible as the interface itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span> <span data-ttu-id="2a12a-157">たとえば、指定すると、コンパイル時エラーが、`private`または`internal`インターフェイスで、 *interface_base*の`public`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="2a12a-157">For example, it is a compile-time error to specify a `private` or `internal` interface in the *interface_base* of a `public` interface.</span></span>

<span data-ttu-id="2a12a-158">直接または間接的には、それ自体から継承するインターフェイスのコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-158">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>

<span data-ttu-id="2a12a-159">***基本インターフェイス***のインターフェイスは、明示的な基本インターフェイスとその基本インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-159">The ***base interfaces*** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="2a12a-160">つまり、基本インターフェイスのセットは、明示的な基本インターフェイス、明示的な基本インターフェイス、およびなどの完全な推移閉包です。</span><span class="sxs-lookup"><span data-stu-id="2a12a-160">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="2a12a-161">インターフェイスは、その基本インターフェイスのすべてのメンバーを継承します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-161">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="2a12a-162">例</span><span class="sxs-lookup"><span data-stu-id="2a12a-162">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="2a12a-163">インターフェイスの基本`IComboBox`は`IControl`、 `ITextBox`、および`IListBox`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-163">the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="2a12a-164">つまり、`IComboBox`上記のインターフェイス メンバーを継承する`SetText`と`SetItems`だけでなく`Paint`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-164">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="2a12a-165">インターフェイスのすべての基本インターフェイスは出力セーフである必要があります ([差異の安全性](interfaces.md#variance-safety))。</span><span class="sxs-lookup"><span data-stu-id="2a12a-165">Every base interface of an interface must be output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span> <span data-ttu-id="2a12a-166">クラスまたは構造体も暗黙的にインターフェイスを実装するには、すべてのインターフェイスの基本インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-166">A class or struct that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

### <a name="interface-body"></a><span data-ttu-id="2a12a-167">インターフェイスの本文</span><span class="sxs-lookup"><span data-stu-id="2a12a-167">Interface body</span></span>

<span data-ttu-id="2a12a-168">*Interface_body*インターフェイスのインターフェイスのメンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-168">The *interface_body* of an interface defines the members of the interface.</span></span>

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a><span data-ttu-id="2a12a-169">インターフェイスのメンバー</span><span class="sxs-lookup"><span data-stu-id="2a12a-169">Interface members</span></span>

<span data-ttu-id="2a12a-170">インターフェイスのメンバーが基底インターフェイスから継承されたメンバーとメンバーがインターフェイス自体で宣言します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-170">The members of an interface are the members inherited from the base interfaces and the members declared by the interface itself.</span></span>

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

<span data-ttu-id="2a12a-171">インターフェイスの宣言には、0 個以上のメンバーを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-171">An interface declaration may declare zero or more members.</span></span> <span data-ttu-id="2a12a-172">インターフェイスのメンバーは、メソッド、プロパティ、イベント、またはインデクサーである必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-172">The members of an interface must be methods, properties, events, or indexers.</span></span> <span data-ttu-id="2a12a-173">インターフェイスは、定数、フィールド、演算子、インスタンス コンス トラクター、デストラクター、または、型を含めることはできません。 また、インターフェイスは、任意の種類の静的メンバーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-173">An interface cannot contain constants, fields, operators, instance constructors, destructors, or types, nor can an interface contain static members of any kind.</span></span>

<span data-ttu-id="2a12a-174">すべてのインターフェイス メンバーは、暗黙的にパブリック アクセスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-174">All interface members implicitly have public access.</span></span> <span data-ttu-id="2a12a-175">インターフェイス メンバーの宣言に修飾子を含めるのコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-175">It is a compile-time error for interface member declarations to include any modifiers.</span></span> <span data-ttu-id="2a12a-176">具体的には、修飾子を使用してインターフェイス メンバーを宣言することはできません`abstract`、 `public`、 `protected`、 `internal`、 `private`、 `virtual`、 `override`、または`static`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-176">In particular, interfaces members cannot be declared with the modifiers `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="2a12a-177">例では、</span><span class="sxs-lookup"><span data-stu-id="2a12a-177">The example</span></span>
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
<span data-ttu-id="2a12a-178">メンバーの可能な種類の 1 つずつ含むインターフェイスを宣言します。メソッド、プロパティ、イベント、およびインデクサーです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-178">declares an interface that contains one each of the possible kinds of members: A method, a property, an event, and an indexer.</span></span>

<span data-ttu-id="2a12a-179">*Interface_declaration*新しい宣言領域を作成します ([宣言](basic-concepts.md#declarations))、および*interface_member_declaration*ですぐに格納されている*interface_declaration*この宣言領域に新しいメンバーを導入します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-179">An *interface_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *interface_member_declaration*s immediately contained by the *interface_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="2a12a-180">次の規則に適用されます*interface_member_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="2a12a-180">The following rules apply to *interface_member_declaration*s:</span></span>

*  <span data-ttu-id="2a12a-181">メソッドの名前は、すべてのプロパティと同じインターフェイスで宣言されたイベントの名前とは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-181">The name of a method must differ from the names of all properties and events declared in the same interface.</span></span> <span data-ttu-id="2a12a-182">さらに、署名 ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading)) のメソッドと同じインターフェイスで宣言されている他のすべてのメソッドのシグネチャは異なるし、同じインターフェイスで宣言されている 2 つの方法で署名がない可能性がありますがのみが異なる`ref`と`out`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-182">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same interface, and two methods declared in the same interface may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="2a12a-183">プロパティまたはイベントの名前は、同じインターフェイスで宣言されているその他のすべてのメンバーの名前とは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-183">The name of a property or event must differ from the names of all other members declared in the same interface.</span></span>
*  <span data-ttu-id="2a12a-184">インデクサーのシグネチャは、同じインターフェイスで宣言されている他のすべてのインデクサーの署名とは異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-184">The signature of an indexer must differ from the signatures of all other indexers declared in the same interface.</span></span>

<span data-ttu-id="2a12a-185">インターフェイスの継承されたメンバーは、具体的には含まれないインターフェイスの宣言領域です。</span><span class="sxs-lookup"><span data-stu-id="2a12a-185">The inherited members of an interface are specifically not part of the declaration space of the interface.</span></span> <span data-ttu-id="2a12a-186">したがって、インターフェイスは継承されたメンバーと同じ名前またはシグネチャを持つメンバーを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-186">Thus, an interface is allowed to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="2a12a-187">この問題が発生すると、派生インターフェイスのメンバーは、基本インターフェイスのメンバーを非表示にすると言います。</span><span class="sxs-lookup"><span data-stu-id="2a12a-187">When this occurs, the derived interface member is said to hide the base interface member.</span></span> <span data-ttu-id="2a12a-188">継承されたメンバーを非表示で、エラーはありませんが、コンパイラ警告を発し。</span><span class="sxs-lookup"><span data-stu-id="2a12a-188">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="2a12a-189">警告を抑制するには、派生インターフェイス メンバーの宣言を含める必要があります、`new`修飾子を派生メンバーでは、基本メンバーを非表示にするものであるかを示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-189">To suppress the warning, the declaration of the derived interface member must include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="2a12a-190">このトピックの説明でさらに[継承による隠ぺい](basic-concepts.md#hiding-through-inheritance)します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-190">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="2a12a-191">場合、`new`継承されたメンバーを隠ぺいしない宣言に修飾子を含めると、警告は、その結果を発行します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-191">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning is issued to that effect.</span></span> <span data-ttu-id="2a12a-192">削除することによってこの警告が抑制され、`new`修飾子。</span><span class="sxs-lookup"><span data-stu-id="2a12a-192">This warning is suppressed by removing the `new` modifier.</span></span>

<span data-ttu-id="2a12a-193">なおクラスでメンバー`object`厳密に言うと、任意のインターフェイスのメンバーは使用されません ([インターフェイスのメンバー](interfaces.md#interface-members))。</span><span class="sxs-lookup"><span data-stu-id="2a12a-193">Note that the members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="2a12a-194">ただし、クラスでメンバー`object`は任意のインターフェイス型でメンバーの検索で利用できます ([メンバー ルックアップ](expressions.md#member-lookup))。</span><span class="sxs-lookup"><span data-stu-id="2a12a-194">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="interface-methods"></a><span data-ttu-id="2a12a-195">インターフェイス メソッド</span><span class="sxs-lookup"><span data-stu-id="2a12a-195">Interface methods</span></span>

<span data-ttu-id="2a12a-196">使用してインターフェイス メソッドが宣言された*interface_method_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="2a12a-196">Interface methods are declared using *interface_method_declaration*s:</span></span>

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

<span data-ttu-id="2a12a-197">*属性*、 *return_type*、*識別子*、および*formal_parameter_list*のインターフェイスのメソッドの宣言が同じであります。クラスでメソッドの宣言のものと意味 ([メソッド](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="2a12a-197">The *attributes*, *return_type*, *identifier*, and *formal_parameter_list* of an interface method declaration have the same meaning as those of a method declaration in a class ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="2a12a-198">メソッドの本体を指定する、インターフェイス メソッド宣言は許可されていませんし、宣言したがって常にはセミコロンで終わます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-198">An interface method declaration is not permitted to specify a method body, and the declaration therefore always ends with a semicolon.</span></span>

<span data-ttu-id="2a12a-199">各インターフェイス メソッドの仮パラメーターの型は、入力の安全なをする必要があります ([差異の安全性](interfaces.md#variance-safety))、戻り値の型は、いずれかを指定する必要があります`void`または出力セーフ。</span><span class="sxs-lookup"><span data-stu-id="2a12a-199">Each formal parameter type of an interface method must be input-safe ([Variance safety](interfaces.md#variance-safety)), and the return type must be either `void` or output-safe.</span></span> <span data-ttu-id="2a12a-200">さらに、各クラス型制約、インターフェイス型制約、およびメソッドの型パラメーターの型パラメーターの制約は、入力の安全なをする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-200">Furthermore, each class type constraint, interface type constraint and type parameter constraint on any type parameter of the method must be input-safe.</span></span>

<span data-ttu-id="2a12a-201">これらのルールにより、共変のインターフェイスの使用状況を反変、タイプ セーフまたはします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-201">These rules ensure that any covariant or contravariant usage of the interface remains type-safe.</span></span> <span data-ttu-id="2a12a-202">例えば以下のようにします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-202">For example,</span></span>
```csharp
interface I<out T> { void M<U>() where U : T; }
```
<span data-ttu-id="2a12a-203">無効ための使用状況`T`で型パラメーター制約として`U`入力セーフではありません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-203">is illegal because the usage of `T` as a type parameter constraint on `U` is not input-safe.</span></span>

<span data-ttu-id="2a12a-204">配置でこの制限はなかったことが可能に次のようにタイプ セーフに違反します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-204">Were this restriction not in place it would be possible to violate type safety in the following manner:</span></span>
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
<span data-ttu-id="2a12a-205">これは、実際に呼び出しを`C.M<E>`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-205">This is actually a call to `C.M<E>`.</span></span> <span data-ttu-id="2a12a-206">呼び出しである必要がありますが、その`E`から派生`D`ので、ここでタイプ セーフに違反することです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-206">But that call requires that `E` derive from `D`, so type safety would be violated here.</span></span>

### <a name="interface-properties"></a><span data-ttu-id="2a12a-207">インターフェイスのプロパティ</span><span class="sxs-lookup"><span data-stu-id="2a12a-207">Interface properties</span></span>

<span data-ttu-id="2a12a-208">使用してインターフェイスのプロパティが宣言された*interface_property_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="2a12a-208">Interface properties are declared using *interface_property_declaration*s:</span></span>

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

<span data-ttu-id="2a12a-209">*属性*、*型*、および*識別子*、インターフェイス プロパティ宣言のクラスでのプロパティの宣言と同じ意味がある ([プロパティ](classes.md#properties))。</span><span class="sxs-lookup"><span data-stu-id="2a12a-209">The *attributes*, *type*, and *identifier* of an interface property declaration have the same meaning as those of a property declaration in a class ([Properties](classes.md#properties)).</span></span>

<span data-ttu-id="2a12a-210">インターフェイス プロパティ宣言のアクセサーがクラスのプロパティ宣言のアクセサーに対応 ([アクセサー](classes.md#accessors)) ただし、アクセサーの本体は、セミコロンに常にあります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-210">The accessors of an interface property declaration correspond to the accessors of a class property declaration ([Accessors](classes.md#accessors)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="2a12a-211">したがって、アクセサーは、プロパティが読み取り/書き込み、読み取り専用または書き込み専用のかどうかを示すだけです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-211">Thus, the accessors simply indicate whether the property is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="2a12a-212">インターフェイス プロパティの型は、get アクセサーがある場合の出力セーフである必要があり、入力の安全な set アクセサーがある場合があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-212">The type of an interface property must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-events"></a><span data-ttu-id="2a12a-213">インターフェイスのイベント</span><span class="sxs-lookup"><span data-stu-id="2a12a-213">Interface events</span></span>

<span data-ttu-id="2a12a-214">インターフェイス イベント宣言を使用して*interface_event_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="2a12a-214">Interface events are declared using *interface_event_declaration*s:</span></span>

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

<span data-ttu-id="2a12a-215">*属性*、*型*、および*識別子*インターフェイス イベント宣言には、クラスでイベントの宣言のものと同じ意味を持ちます ([イベント](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="2a12a-215">The *attributes*, *type*, and *identifier* of an interface event declaration have the same meaning as those of an event declaration in a class ([Events](classes.md#events)).</span></span>

<span data-ttu-id="2a12a-216">インターフェイス イベントの種類は、入力の安全なである必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-216">The type of an interface event must be input-safe.</span></span>

### <a name="interface-indexers"></a><span data-ttu-id="2a12a-217">インターフェイスのインデクサー</span><span class="sxs-lookup"><span data-stu-id="2a12a-217">Interface indexers</span></span>

<span data-ttu-id="2a12a-218">インターフェイスのインデクサーを宣言*interface_indexer_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="2a12a-218">Interface indexers are declared using *interface_indexer_declaration*s:</span></span>

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

<span data-ttu-id="2a12a-219">*属性*、*型*、および*formal_parameter_list*インターフェイスのインデクサーの宣言のクラス (でインデクサーの宣言のと同じ意味があります。[インデクサー](classes.md#indexers))。</span><span class="sxs-lookup"><span data-stu-id="2a12a-219">The *attributes*, *type*, and *formal_parameter_list* of an interface indexer declaration have the same meaning as those of an indexer declaration in a class ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="2a12a-220">インターフェイスのインデクサーの宣言のアクセサーがクラスのインデクサーの宣言のアクセサーに対応 ([インデクサー](classes.md#indexers)) ただし、アクセサーの本体は、セミコロンに常にあります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-220">The accessors of an interface indexer declaration correspond to the accessors of a class indexer declaration ([Indexers](classes.md#indexers)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="2a12a-221">したがって、アクセサーは、インデクサーが読み取り/書き込み、読み取り専用または書き込み専用のかどうかを示すだけです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-221">Thus, the accessors simply indicate whether the indexer is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="2a12a-222">インターフェイスのインデクサーのすべての正式なパラメーター型は、入力の安全なである必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-222">All the formal parameter types of an interface indexer must be input-safe .</span></span> <span data-ttu-id="2a12a-223">さらに、すべて`out`または`ref`仮パラメーターの型は出力セーフにもあります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-223">In addition, any `out` or `ref` formal parameter types must also be output-safe.</span></span> <span data-ttu-id="2a12a-224">注も`out`パラメーターを基になる実行プラットフォームの制限により、入力セーフである必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-224">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="2a12a-225">インターフェイスのインデクサーの型は、get アクセサーがある場合の出力セーフである必要があり、入力の安全な set アクセサーがある場合があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-225">The type of an interface indexer must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-member-access"></a><span data-ttu-id="2a12a-226">インターフェイス メンバーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="2a12a-226">Interface member access</span></span>

<span data-ttu-id="2a12a-227">インターフェイス メンバーはメンバー アクセスを通じてアクセス ([メンバー アクセス](expressions.md#member-access)) アクセスおよびインデクサー アクセス ([インデクサー アクセス](expressions.md#indexer-access)) 形式の式`I.M`と`I[A]`ここで、 `I`インターフェイス型は、`M`メソッド、プロパティ、またはそのインターフェイス型のイベントと`A`インデクサーの引数リストです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-227">Interface members are accessed through member access ([Member access](expressions.md#member-access)) and indexer access ([Indexer access](expressions.md#indexer-access)) expressions of the form `I.M` and `I[A]`, where `I` is an interface type, `M` is a method, property, or event of that interface type, and `A` is an indexer argument list.</span></span>

<span data-ttu-id="2a12a-228">厳密にはインターフェイスの単一継承 (継承チェーン内の各インターフェイスは、正確に 0 個または 1 つの直接基底インターフェイスを持つ)、メンバー検索の効果 ([メンバー ルックアップ](expressions.md#member-lookup))、メソッドの呼び出し ([メソッドの呼び出し](expressions.md#method-invocations))、およびインデクサー アクセス ([インデクサー アクセス](expressions.md#indexer-access)) ルールはまったく同じクラスと構造体です。メンバーの非表示には、同じ名前またはシグネチャを持つ派生メンバーを少なくする強い派生します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-228">For interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effects of the member lookup ([Member lookup](expressions.md#member-lookup)), method invocation ([Method invocations](expressions.md#method-invocations)), and indexer access ([Indexer access](expressions.md#indexer-access)) rules are exactly the same as for classes and structs: More derived members hide less derived members with the same name or signature.</span></span> <span data-ttu-id="2a12a-229">ただし、多重継承インターフェイスでは、2 つに、あいまいさが発生することがまたは以上の関連付けられていない基本インターフェイスが同じ名前またはシグネチャを持つメンバーを宣言します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-229">However, for multiple-inheritance interfaces, ambiguities can occur when two or more unrelated base interfaces declare members with the same name or signature.</span></span> <span data-ttu-id="2a12a-230">このセクションでは、このような状況のいくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-230">This section shows several examples of such situations.</span></span> <span data-ttu-id="2a12a-231">すべてのケースで、あいまいさを解決するのには明示的なキャストを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-231">In all cases, explicit casts can be used to resolve the ambiguities.</span></span>

<span data-ttu-id="2a12a-232">例</span><span class="sxs-lookup"><span data-stu-id="2a12a-232">In the example</span></span>
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
<span data-ttu-id="2a12a-233">最初の 2 つのステートメントのコンパイル時エラーが発生するため、メンバー検索 ([メンバー検索](expressions.md#member-lookup)) の`Count`で`IListCounter`があいまいです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-233">the first two statements cause compile-time errors because the member lookup ([Member lookup](expressions.md#member-lookup)) of `Count` in `IListCounter` is ambiguous.</span></span> <span data-ttu-id="2a12a-234">キャストすることで、あいまいさが解決されるように、例に示すように、`x`基底インターフェイスの適切な型にします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-234">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="2a12a-235">このようなキャストには実行時のコストが必要ない、コンパイル時に弱い派生型として、インスタンスを表示するだけで構成されます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-235">Such casts have no run-time costs—they merely consist of viewing the instance as a less derived type at compile-time.</span></span>

<span data-ttu-id="2a12a-236">例</span><span class="sxs-lookup"><span data-stu-id="2a12a-236">In the example</span></span>
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
<span data-ttu-id="2a12a-237">呼び出し`n.Add(1)`選択`IInteger.Add`のオーバー ロード解決規則を適用することで[オーバー ロードの解決](expressions.md#overload-resolution)します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-237">the invocation `n.Add(1)` selects `IInteger.Add` by applying the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="2a12a-238">同様に、呼び出し`n.Add(1.0)`選択`IDouble.Add`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-238">Similarly the invocation `n.Add(1.0)` selects `IDouble.Add`.</span></span> <span data-ttu-id="2a12a-239">明示的なキャストが挿入されると、メソッドとなるに 1 つのみの候補があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-239">When explicit casts are inserted, there is only one candidate method, and thus no ambiguity.</span></span>

<span data-ttu-id="2a12a-240">例</span><span class="sxs-lookup"><span data-stu-id="2a12a-240">In the example</span></span>
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
<span data-ttu-id="2a12a-241">`IBase.F`によって隠されているメンバー、`ILeft.F`メンバー。</span><span class="sxs-lookup"><span data-stu-id="2a12a-241">the `IBase.F` member is hidden by the `ILeft.F` member.</span></span> <span data-ttu-id="2a12a-242">呼び出し`d.F(1)`ため選択`ILeft.F`場合でも、`IBase.F`を通じて潜在顧客のアクセス パスで非表示にしないよう`IRight`。</span><span class="sxs-lookup"><span data-stu-id="2a12a-242">The invocation `d.F(1)` thus selects `ILeft.F`, even though `IBase.F` appears to not be hidden in the access path that leads through `IRight`.</span></span>

<span data-ttu-id="2a12a-243">多重継承インターフェイスで非表示の直感的なルールは、これには単純です。メンバーが任意のアクセス パスで非表示の場合は、すべてのアクセス パスで表示されません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-243">The intuitive rule for hiding in multiple-inheritance interfaces is simply this: If a member is hidden in any access path, it is hidden in all access paths.</span></span> <span data-ttu-id="2a12a-244">からのアクセス パス`IDerived`に`ILeft`に`IBase`を非表示に`IBase.F`からのアクセス パスで、メンバーは非表示も`IDerived`に`IRight`に`IBase`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-244">Because the access path from `IDerived` to `ILeft` to `IBase` hides `IBase.F`, the member is also hidden in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

## <a name="fully-qualified-interface-member-names"></a><span data-ttu-id="2a12a-245">完全修飾インターフェイス メンバーの名前</span><span class="sxs-lookup"><span data-stu-id="2a12a-245">Fully qualified interface member names</span></span>

<span data-ttu-id="2a12a-246">インターフェイス メンバーはによって呼ばその***完全修飾名***します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-246">An interface member is sometimes referred to by its ***fully qualified name***.</span></span> <span data-ttu-id="2a12a-247">インターフェイス メンバーの完全修飾名をメンバーが宣言されている、続けて、ドット、後に、メンバーの名前のインターフェイスの名前で構成されます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-247">The fully qualified name of an interface member consists of the name of the interface in which the member is declared, followed by a dot, followed by the name of the member.</span></span> <span data-ttu-id="2a12a-248">メンバーの完全修飾名では、メンバーが宣言されているインターフェイスを参照します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-248">The fully qualified name of a member references the interface in which the member is declared.</span></span> <span data-ttu-id="2a12a-249">たとえば、宣言があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-249">For example, given the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
<span data-ttu-id="2a12a-250">完全修飾名`Paint`は`IControl.Paint`との完全修飾名`SetText`は`ITextBox.SetText`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-250">the fully qualified name of `Paint` is `IControl.Paint` and the fully qualified name of `SetText` is `ITextBox.SetText`.</span></span>

<span data-ttu-id="2a12a-251">上記の例ではないから参照できる`Paint`として`ITextBox.Paint`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-251">In the example above, it is not possible to refer to `Paint` as `ITextBox.Paint`.</span></span>

<span data-ttu-id="2a12a-252">インターフェイスは、名前空間の一部が、インターフェイス メンバーの完全修飾名には、名前空間の名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2a12a-252">When an interface is part of a namespace, the fully qualified name of an interface member includes the namespace name.</span></span> <span data-ttu-id="2a12a-253">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-253">For example</span></span>
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

<span data-ttu-id="2a12a-254">ここでは、の完全修飾名、`Clone`メソッドは`System.ICloneable.Clone`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-254">Here, the fully qualified name of the `Clone` method is `System.ICloneable.Clone`.</span></span>

## <a name="interface-implementations"></a><span data-ttu-id="2a12a-255">インターフェイスの実装</span><span class="sxs-lookup"><span data-stu-id="2a12a-255">Interface implementations</span></span>

<span data-ttu-id="2a12a-256">クラスと構造体でインターフェイスを実装することがあります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-256">Interfaces may be implemented by classes and structs.</span></span> <span data-ttu-id="2a12a-257">クラスまたは構造体を直接インターフェイスを実装するかを示す、クラスまたは構造体の基底クラス リストにインターフェイス識別子が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-257">To indicate that a class or struct directly implements an interface, the interface identifier is included in the base class list of the class or struct.</span></span> <span data-ttu-id="2a12a-258">例:</span><span class="sxs-lookup"><span data-stu-id="2a12a-258">For example:</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

<span data-ttu-id="2a12a-259">クラスまたは直接も直接インターフェイスを実装する構造体はインターフェイスの基本インターフェイスのすべて暗黙的に実装します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-259">A class or struct that directly implements an interface also directly implements all of the interface's base interfaces implicitly.</span></span> <span data-ttu-id="2a12a-260">これは、クラスまたは構造体により明示的に基底クラス リスト内のすべての基底インターフェイスが表示されない場合でも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-260">This is true even if the class or struct doesn't explicitly list all base interfaces in the base class list.</span></span> <span data-ttu-id="2a12a-261">例:</span><span class="sxs-lookup"><span data-stu-id="2a12a-261">For example:</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

<span data-ttu-id="2a12a-262">ここでは、クラス`TextBox`両方を実装`IControl`と`ITextBox`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-262">Here, class `TextBox` implements both `IControl` and `ITextBox`.</span></span>

<span data-ttu-id="2a12a-263">クラスと`C`C から派生したすべてのクラス、インターフェイスを実装するインターフェイスを暗黙的に実装も直接します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-263">When a class `C` directly implements an interface, all classes derived from C also implement the interface implicitly.</span></span> <span data-ttu-id="2a12a-264">クラス宣言で指定された基本インターフェイスが構築されたインターフェイス型を指定できます ([構築型](types.md#constructed-types))。</span><span class="sxs-lookup"><span data-stu-id="2a12a-264">The base interfaces specified in a class declaration can be constructed interface types ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="2a12a-265">スコープ内の型パラメーターを伴うことができますが、基本インターフェイスは、独自の型パラメーターにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-265">A base interface cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span> <span data-ttu-id="2a12a-266">次のコードは、クラスの実装および構築された型を拡張する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="2a12a-266">The following code illustrates how a class can implement and extend constructed types:</span></span>
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

<span data-ttu-id="2a12a-267">ジェネリック クラス宣言の基本インターフェイスは、一意性の規則を満たす必要があります[実装されたインターフェイスの一意性](interfaces.md#uniqueness-of-implemented-interfaces)します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-267">The base interfaces of a generic class declaration must satisfy the uniqueness rule described in [Uniqueness of implemented interfaces](interfaces.md#uniqueness-of-implemented-interfaces).</span></span>

### <a name="explicit-interface-member-implementations"></a><span data-ttu-id="2a12a-268">明示的なインターフェイス メンバーの実装</span><span class="sxs-lookup"><span data-stu-id="2a12a-268">Explicit interface member implementations</span></span>

<span data-ttu-id="2a12a-269">クラスまたは構造体を宣言できますインターフェイスを実装するために、***明示的なインターフェイス メンバーの実装***します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-269">For purposes of implementing interfaces, a class or struct may declare ***explicit interface member implementations***.</span></span> <span data-ttu-id="2a12a-270">明示的なインターフェイス メンバーの実装は、完全修飾インターフェイス メンバー名を参照するメソッド、プロパティ、イベント、またはインデクサーの宣言です。</span><span class="sxs-lookup"><span data-stu-id="2a12a-270">An explicit interface member implementation is a method, property, event, or indexer declaration that references a fully qualified interface member name.</span></span> <span data-ttu-id="2a12a-271">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-271">For example</span></span>
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

<span data-ttu-id="2a12a-272">ここで`IDictionary<int,T>.this`と`IDictionary<int,T>.Add`明示的なインターフェイスは、メンバーの実装です。</span><span class="sxs-lookup"><span data-stu-id="2a12a-272">Here `IDictionary<int,T>.this` and `IDictionary<int,T>.Add` are explicit interface member implementations.</span></span>

<span data-ttu-id="2a12a-273">場合によっては、インターフェイス メンバーの名前のインターフェイス メンバーの場合可能性がありますが実装される明示的なインターフェイス メンバーの実装を使用して、実装するクラスの適切なしない場合があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-273">In some cases, the name of an interface member may not be appropriate for the implementing class, in which case the interface member may be implemented using explicit interface member implementation.</span></span> <span data-ttu-id="2a12a-274">ファイル アブストラクションを実装するクラスの実装は可能性があります、`Close`メンバー関数をファイル リソースが解放の効果があり、実装、`Dispose`のメソッド、`IDisposable`インターフェイスの明示的なインターフェイスを使用します。メンバーの実装:</span><span class="sxs-lookup"><span data-stu-id="2a12a-274">A class implementing a file abstraction, for example, would likely implement a `Close` member function that has the effect of releasing the file resource, and implement the `Dispose` method of the `IDisposable` interface using explicit interface member implementation:</span></span>
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

<span data-ttu-id="2a12a-275">メソッドの呼び出し、プロパティ アクセス、またはインデクサー アクセスで、完全修飾名を使って明示的なインターフェイス メンバーの実装にアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-275">It is not possible to access an explicit interface member implementation through its fully qualified name in a method invocation, property access, or indexer access.</span></span> <span data-ttu-id="2a12a-276">明示的なインターフェイス メンバーの実装では、インターフェイス インスタンスからのみアクセスできるし、そのメンバーの名前だけで参照される場合。</span><span class="sxs-lookup"><span data-stu-id="2a12a-276">An explicit interface member implementation can only be accessed through an interface instance, and is in that case referenced simply by its member name.</span></span>

<span data-ttu-id="2a12a-277">アクセス修飾子を含めるには、明示的なインターフェイス メンバーの実装と、コンパイル時エラーし、修飾子を含めると、コンパイル時エラー `abstract`、 `virtual`、 `override`、または`static`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-277">It is a compile-time error for an explicit interface member implementation to include access modifiers, and it is a compile-time error to include the modifiers `abstract`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="2a12a-278">明示的なインターフェイス メンバーの実装では、他のメンバーとさまざまなアクセシビリティの特性があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-278">Explicit interface member implementations have different accessibility characteristics than other members.</span></span> <span data-ttu-id="2a12a-279">明示的なインターフェイス メンバーの実装がメソッドの呼び出しまたはプロパティ アクセスで、完全修飾名からアクセスできることはありませんのでは意味がプライベートにあります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-279">Because explicit interface member implementations are never accessible through their fully qualified name in a method invocation or a property access, they are in a sense private.</span></span> <span data-ttu-id="2a12a-280">ただし、インターフェイス インスタンスを通じてアクセスできる、ので意味もパブリックでは。</span><span class="sxs-lookup"><span data-stu-id="2a12a-280">However, since they can be accessed through an interface instance, they are in a sense also public.</span></span>

<span data-ttu-id="2a12a-281">明示的なインターフェイス メンバーの実装では、2 つの主な目的を果たします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-281">Explicit interface member implementations serve two primary purposes:</span></span>

*  <span data-ttu-id="2a12a-282">明示的なインターフェイス メンバーの実装がクラスまたは構造体のインスタンスを通じてアクセス可能でないため、クラスまたは構造体のパブリック インターフェイスから除外するインターフェイスの実装が可能です。</span><span class="sxs-lookup"><span data-stu-id="2a12a-282">Because explicit interface member implementations are not accessible through class or struct instances, they allow interface implementations to be excluded from the public interface of a class or struct.</span></span> <span data-ttu-id="2a12a-283">これは、クラスの場合に特に役立ちます。 または構造体は、そのクラスまたは構造体のコンシューマーに関係のないのは内部インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-283">This is particularly useful when a class or struct implements an internal interface that is of no interest to a consumer of that class or struct.</span></span>
*  <span data-ttu-id="2a12a-284">明示的なインターフェイス メンバーの実装では、同じシグネチャを持つインターフェイス メンバーのあいまいさ排除を許可します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-284">Explicit interface member implementations allow disambiguation of interface members with the same signature.</span></span> <span data-ttu-id="2a12a-285">明示的なインターフェイス メンバーの実装を含まないことはできませんクラスまたは構造体のさまざまな実装のインターフェイスのメンバーは同じシグネチャを持つことはできません、クラスまたは構造体の可能な任意の実装としての型を取得するにはまったく同じシグネチャではなく、さまざまなインターフェイス メンバーの型を返します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-285">Without explicit interface member implementations it would be impossible for a class or struct to have different implementations of interface members with the same signature and return type, as would it be impossible for a class or struct to have any implementation at all of interface members with the same signature but with different return types.</span></span>

<span data-ttu-id="2a12a-286">明示的なインターフェイス メンバーの実装を有効にするには、クラスまたは構造体する必要がありますインターフェイスの名前、その基底クラス リストが完全修飾名、型、およびパラメーターの型と正確に一致明示的なインターフェイス メンバーのメンバーを含む実装です。</span><span class="sxs-lookup"><span data-stu-id="2a12a-286">For an explicit interface member implementation to be valid, the class or struct must name an interface in its base class list that contains a member whose fully qualified name, type, and parameter types exactly match those of the explicit interface member implementation.</span></span> <span data-ttu-id="2a12a-287">したがって、次のクラスで</span><span class="sxs-lookup"><span data-stu-id="2a12a-287">Thus, in the following class</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
<span data-ttu-id="2a12a-288">宣言`IComparable.CompareTo`ため、コンパイル時エラー結果`IComparable`の基底クラス リストにも記載されていない`Shape`の基本インターフェイスでないと`ICloneable`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-288">the declaration of `IComparable.CompareTo` results in a compile-time error because `IComparable` is not listed in the base class list of `Shape` and is not a base interface of `ICloneable`.</span></span> <span data-ttu-id="2a12a-289">同様に、宣言内</span><span class="sxs-lookup"><span data-stu-id="2a12a-289">Likewise, in the declarations</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
<span data-ttu-id="2a12a-290">宣言`ICloneable.Clone`で`Ellipse`ため、コンパイル時エラー結果`ICloneable`の基底クラス リストで明示的ににない`Ellipse`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-290">the declaration of `ICloneable.Clone` in `Ellipse` results in a compile-time error because `ICloneable` is not explicitly listed in the base class list of `Ellipse`.</span></span>

<span data-ttu-id="2a12a-291">インターフェイス メンバーの完全修飾名には、メンバーが宣言されたインターフェイスを参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-291">The fully qualified name of an interface member must reference the interface in which the member was declared.</span></span> <span data-ttu-id="2a12a-292">したがって、宣言内</span><span class="sxs-lookup"><span data-stu-id="2a12a-292">Thus, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
<span data-ttu-id="2a12a-293">明示的なインターフェイス メンバーの実装の`Paint`として記述する必要があります`IControl.Paint`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-293">the explicit interface member implementation of `Paint` must be written as `IControl.Paint`.</span></span>

### <a name="uniqueness-of-implemented-interfaces"></a><span data-ttu-id="2a12a-294">実装されたインターフェイスの一意性</span><span class="sxs-lookup"><span data-stu-id="2a12a-294">Uniqueness of implemented interfaces</span></span>

<span data-ttu-id="2a12a-295">ジェネリック型の宣言によって実装されるインターフェイスは、すべての可能な構築された型に対して一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-295">The interfaces implemented by a generic type declaration must remain unique for all possible constructed types.</span></span> <span data-ttu-id="2a12a-296">この規則がないできなくなる正しいメソッドを呼び出して構築された特定の種類を特定できます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-296">Without this rule, it would be impossible to determine the correct method to call for certain constructed types.</span></span> <span data-ttu-id="2a12a-297">たとえば、次のように書き込まれるジェネリック クラス宣言が許可されたとします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-297">For example, suppose a generic class declaration were permitted to be written as follows:</span></span>
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

<span data-ttu-id="2a12a-298">これは許可された、できなくなる場合は、次を実行するコードを判断すること。</span><span class="sxs-lookup"><span data-stu-id="2a12a-298">Were this permitted, it would be impossible to determine which code to execute in the following case:</span></span>
```csharp
I<int> x = new X<int,int>();
x.F();
```

<span data-ttu-id="2a12a-299">インターフェイスのジェネリック型の宣言の一覧が有効であるを確認するのには、次の手順が実行されます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-299">To determine if the interface list of a generic type declaration is valid, the following steps are performed:</span></span>

*  <span data-ttu-id="2a12a-300">ように`L`ジェネリック クラス、構造体、またはインターフェイスの宣言で直接指定するインターフェイスのリストである`C`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-300">Let `L` be the list of interfaces directly specified in a generic class, struct, or interface declaration `C`.</span></span>
*  <span data-ttu-id="2a12a-301">追加`L`任意の基本インターフェイスのインターフェイスを既に`L`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-301">Add to `L` any base interfaces of the interfaces already in `L`.</span></span>
*  <span data-ttu-id="2a12a-302">すべての重複を削除`L`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-302">Remove any duplicates from `L`.</span></span>
*  <span data-ttu-id="2a12a-303">可能性のあるすべての構築型から作成された場合`C`は、型引数に置き換え後`L`、2 つのインターフェイスで`L`は同じでの宣言では、`C`が無効です。</span><span class="sxs-lookup"><span data-stu-id="2a12a-303">If any possible constructed type created from `C` would, after type arguments are substituted into `L`, cause two interfaces in `L` to be identical, then the declaration of `C` is invalid.</span></span> <span data-ttu-id="2a12a-304">すべての可能な構築された型を決定するときに、宣言の制約は考慮されません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-304">Constraint declarations are not considered when determining all possible constructed types.</span></span>

<span data-ttu-id="2a12a-305">クラス宣言で`X`インターフェイス リストの上`L`から成る`I<U>`と`I<V>`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-305">In the class declaration `X` above, the interface list `L` consists of `I<U>` and `I<V>`.</span></span> <span data-ttu-id="2a12a-306">いずれかを持つ型が構築されたため、宣言は無効です`U`と`V`これら 2 つのインターフェイスと同じ型にすることによって、同じ型の中します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-306">The declaration is invalid because any constructed type with `U` and `V` being the same type would cause these two interfaces to be identical types.</span></span>

<span data-ttu-id="2a12a-307">別の継承レベルを統一する指定されたインターフェイスのことができます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-307">It is possible for interfaces specified at different inheritance levels to unify:</span></span>
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

<span data-ttu-id="2a12a-308">このコードは有効な場合でも`Derived<U,V>`両方を実装`I<U>`と`I<V>`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-308">This code is valid even though `Derived<U,V>` implements both `I<U>` and `I<V>`.</span></span> <span data-ttu-id="2a12a-309">コード</span><span class="sxs-lookup"><span data-stu-id="2a12a-309">The code</span></span>
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
<span data-ttu-id="2a12a-310">メソッドを呼び出します`Derived`、ため`Derived<int,int>`効果的に再実装`I<int>`([インターフェイスの再実装](interfaces.md#interface-re-implementation))。</span><span class="sxs-lookup"><span data-stu-id="2a12a-310">invokes the method in `Derived`, since `Derived<int,int>` effectively re-implements `I<int>` ([Interface re-implementation](interfaces.md#interface-re-implementation)).</span></span>

### <a name="implementation-of-generic-methods"></a><span data-ttu-id="2a12a-311">ジェネリック メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="2a12a-311">Implementation of generic methods</span></span>

<span data-ttu-id="2a12a-312">ジェネリック メソッドを暗黙的にメソッドを実装するインターフェイス、メソッド型パラメーターごとにする必要がありますと同等の両方の宣言で (任意のインターフェイス型のパラメーターが適切な型引数に置き換えられます) 後で指定される制約メソッド型パラメーターは、左から右に序数の位置によって識別されます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-312">When a generic method implicitly implements an interface method, the constraints given for each method type parameter must be equivalent in both declarations (after any interface type parameters are replaced with the appropriate type arguments), where method type parameters are identified by ordinal positions, left to right.</span></span>

<span data-ttu-id="2a12a-313">ジェネリック メソッドは、インターフェイス メソッドを明示的に実装する、ただし、制約は許可されませんメソッドの実装にします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-313">When a generic method explicitly implements an interface method, however, no constraints are allowed on the implementing method.</span></span> <span data-ttu-id="2a12a-314">代わりに、制約がインターフェイス メソッドから継承します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-314">Instead, the constraints are inherited from the interface method</span></span>

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

<span data-ttu-id="2a12a-315">メソッド`C.F<T>`は暗黙的に実装`I<object,C,string>.F<T>`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-315">The method `C.F<T>` implicitly implements `I<object,C,string>.F<T>`.</span></span> <span data-ttu-id="2a12a-316">この場合、`C.F<T>`がないために必要な (も許可されている)、制約を指定する`T:object`ため`object`暗黙的な型のすべてのパラメーターの制約します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-316">In this case, `C.F<T>` is not required (nor permitted) to specify the constraint `T:object` since `object` is an implicit constraint on all type parameters.</span></span> <span data-ttu-id="2a12a-317">メソッド`C.G<T>`は暗黙的に実装`I<object,C,string>.G<T>`制約に合わせてインターフェイスで、インターフェイスの型パラメーターは、対応する型引数に置き換えられます後ためです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-317">The method `C.G<T>` implicitly implements `I<object,C,string>.G<T>` because the constraints match those in the interface, after the interface type parameters are replaced with the corresponding type arguments.</span></span> <span data-ttu-id="2a12a-318">メソッドの制約`C.H<T>`、sealed 型であるために、エラー (`string`ここで) 制約として使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-318">The constraint for method `C.H<T>` is an error because sealed types (`string` in this case) cannot be used as constraints.</span></span> <span data-ttu-id="2a12a-319">制約を省略すると、暗黙的なインターフェイス メソッドの実装の制約と一致する必要があるためエラーがあります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-319">Omitting the constraint would also be an error since constraints of implicit interface method implementations are required to match.</span></span> <span data-ttu-id="2a12a-320">そのため、暗黙的に実装することはできません`I<object,C,string>.H<T>`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-320">Thus, it is impossible to implicitly implement `I<object,C,string>.H<T>`.</span></span> <span data-ttu-id="2a12a-321">このインターフェイス メソッドは、明示的なインターフェイス メンバーの実装を使用してのみ実装できます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-321">This interface method can only be implemented using an explicit interface member implementation:</span></span>
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

<span data-ttu-id="2a12a-322">この例では、明示的なインターフェイス メンバーの実装は、厳密に弱い制約を持つパブリック メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-322">In this example, the explicit interface member implementation invokes a public method having strictly weaker constraints.</span></span> <span data-ttu-id="2a12a-323">なおから代入された`t`に`s`以降が正しく`T`の制約を継承`T:string`この制約はソース コードで表現できる場合でも。</span><span class="sxs-lookup"><span data-stu-id="2a12a-323">Note that the assignment from `t` to `s` is valid since `T` inherits a constraint of `T:string`, even though this constraint is not expressible in source code.</span></span>

### <a name="interface-mapping"></a><span data-ttu-id="2a12a-324">インターフェイスの割り当て</span><span class="sxs-lookup"><span data-stu-id="2a12a-324">Interface mapping</span></span>

<span data-ttu-id="2a12a-325">クラスまたは構造体には、クラスまたは構造体の基底クラス リストに記載されているインターフェイスのすべてのメンバーの実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-325">A class or struct must provide implementations of all members of the interfaces that are listed in the base class list of the class or struct.</span></span> <span data-ttu-id="2a12a-326">実装するクラスまたは構造体でインターフェイス メンバーの実装を検索するプロセスと呼びます***インターフェイス マップ***します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-326">The process of locating implementations of interface members in an implementing class or struct is known as ***interface mapping***.</span></span>

<span data-ttu-id="2a12a-327">クラスまたは構造体のインターフェイス マップ`C`の基底クラス リストで指定された各インターフェイスの各メンバーの実装を探します`C`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-327">Interface mapping for a class or struct `C` locates an implementation for each member of each interface specified in the base class list of `C`.</span></span> <span data-ttu-id="2a12a-328">特定のインターフェイス メンバーの実装`I.M`ここで、`I`をインターフェイスには、メンバー`M`が宣言されている場合は、各クラスまたは構造体を調べることで決定されます`S`以降の`C`と連続する各基底クラスの繰り返し`C`一致が見つかるまで。</span><span class="sxs-lookup"><span data-stu-id="2a12a-328">The implementation of a particular interface member `I.M`, where `I` is the interface in which the member `M` is declared, is determined by examining each class or struct `S`, starting with `C` and repeating for each successive base class of `C`, until a match is located:</span></span>

*  <span data-ttu-id="2a12a-329">場合`S`と一致する明示的なインターフェイス メンバーの実装の宣言を含む`I`と`M`、このメンバーは、実装の`I.M`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-329">If `S` contains a declaration of an explicit interface member implementation that matches `I` and `M`, then this member is the implementation of `I.M`.</span></span>
*  <span data-ttu-id="2a12a-330">の場合`S`と一致する非静的なパブリック メンバーの宣言を含む`M`、このメンバーは、実装の`I.M`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-330">Otherwise, if `S` contains a declaration of a non-static public member that matches `M`, then this member is the implementation of `I.M`.</span></span> <span data-ttu-id="2a12a-331">メンバーの実装は、1 つのメンバーの一致よりも多く指定がない場合`I.M`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-331">If more than one member matches, it is unspecified which member is the implementation of `I.M`.</span></span> <span data-ttu-id="2a12a-332">このような状況はできる場合にのみ発生`S`が構築された型をジェネリック型で宣言されている 2 つのメンバーが異なるシグネチャを持つが、型引数を指定すること、署名は同じです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-332">This situation can only occur if `S` is a constructed type where the two members as declared in the generic type have different signatures, but the type arguments make their signatures identical.</span></span>

<span data-ttu-id="2a12a-333">コンパイル時エラーは、実装は、の基底クラス リストで指定されたすべてのインターフェイスのすべてのメンバーが見つからない場合に発生します。`C`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-333">A compile-time error occurs if implementations cannot be located for all members of all interfaces specified in the base class list of `C`.</span></span> <span data-ttu-id="2a12a-334">インターフェイスのメンバーが基底インターフェイスから継承されたメンバーを含めることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2a12a-334">Note that the members of an interface include those members that are inherited from base interfaces.</span></span>

<span data-ttu-id="2a12a-335">インターフェイス マップ、クラス メンバーの目的で`A`インターフェイスのメンバーと一致する`B`とき。</span><span class="sxs-lookup"><span data-stu-id="2a12a-335">For purposes of interface mapping, a class member `A` matches an interface member `B` when:</span></span>

*  <span data-ttu-id="2a12a-336">`A` `B`メソッド、および、名前、種類、および仮パラメーター リスト`A`と`B`は同じです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-336">`A` and `B` are methods, and the name, type, and formal parameter lists of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="2a12a-337">`A` `B` 、プロパティ、名前および種類の`A`と`B`同じですと`A`と同じアクセサーを持つ`B`(`A`明示的なインターフェイスでない場合は、追加のアクセサーを持つことができますメンバーの実装の場合)。</span><span class="sxs-lookup"><span data-stu-id="2a12a-337">`A` and `B` are properties, the name and type of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>
*  <span data-ttu-id="2a12a-338">`A` `B`イベントは、名前と種類の`A`と`B`は同じです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-338">`A` and `B` are events, and the name and type of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="2a12a-339">`A` `B`インデクサーでは、型と仮パラメーター リストは`A`と`B`同じですと`A`と同じアクセサーを持つ`B`(`A`でない場合は、追加のアクセサーを持つことができます、明示的なインターフェイス メンバーの実装)。</span><span class="sxs-lookup"><span data-stu-id="2a12a-339">`A` and `B` are indexers, the type and formal parameter lists of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>

<span data-ttu-id="2a12a-340">インターフェイス マップのアルゴリズムの主な特性は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-340">Notable implications of the interface mapping algorithm are:</span></span>

*  <span data-ttu-id="2a12a-341">明示的なインターフェイス メンバーの実装は、同じクラスまたは構造体の他のメンバーに優先される、インターフェイス メンバーを実装するクラスまたは構造体のメンバーを決定するときにします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-341">Explicit interface member implementations take precedence over other members in the same class or struct when determining the class or struct member that implements an interface member.</span></span>
*  <span data-ttu-id="2a12a-342">非パブリックも静的でもないメンバーは、インターフェイスのマッピングに参加します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-342">Neither non-public nor static members participate in interface mapping.</span></span>

<span data-ttu-id="2a12a-343">例</span><span class="sxs-lookup"><span data-stu-id="2a12a-343">In the example</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
<span data-ttu-id="2a12a-344">`ICloneable.Clone`のメンバー`C`の実装になります`Clone`で`ICloneable`理由は、明示的なインターフェイス メンバーの実装が他のメンバーより優先順位。</span><span class="sxs-lookup"><span data-stu-id="2a12a-344">the `ICloneable.Clone` member of `C` becomes the implementation of `Clone` in `ICloneable` because explicit interface member implementations take precedence over other members.</span></span>

<span data-ttu-id="2a12a-345">同じ名前、種類、およびパラメーターの型を持つメンバーを格納している以上のインターフェイスをクラスまたは構造体は 2 つを実装している場合は、1 つのクラスまたは構造体メンバー上にこれらのインターフェイス メンバーの各をマップすること勧めします。</span><span class="sxs-lookup"><span data-stu-id="2a12a-345">If a class or struct implements two or more interfaces containing a member with the same name, type, and parameter types, it is possible to map each of those interface members onto a single class or struct member.</span></span> <span data-ttu-id="2a12a-346">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-346">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

<span data-ttu-id="2a12a-347">ここでは、`Paint`の両方のメソッド`IControl`と`IForm`にマップされて、`Paint`メソッド`Page`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-347">Here, the `Paint` methods of both `IControl` and `IForm` are mapped onto the `Paint` method in `Page`.</span></span> <span data-ttu-id="2a12a-348">もちろんも 2 つのメソッドの別の明示的なインターフェイス メンバーの実装を含めることは可能です。</span><span class="sxs-lookup"><span data-stu-id="2a12a-348">It is of course also possible to have separate explicit interface member implementations for the two methods.</span></span>

<span data-ttu-id="2a12a-349">クラスまたは構造体は、非表示のメンバーを格納しているインターフェイスを実装する場合は、明示的なインターフェイス メンバーの実装を通じて、一部のメンバーとは限りません実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-349">If a class or struct implements an interface that contains hidden members, then some members must necessarily be implemented through explicit interface member implementations.</span></span> <span data-ttu-id="2a12a-350">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-350">For example</span></span>
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

<span data-ttu-id="2a12a-351">このインターフェイスの実装に少なくとも 1 つの明示的なインターフェイス メンバーの実装では、必要し、なります。 形式は次のいずれか</span><span class="sxs-lookup"><span data-stu-id="2a12a-351">An implementation of this interface would require at least one explicit interface member implementation, and would take one of the following forms</span></span>
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

<span data-ttu-id="2a12a-352">クラスは、同じ基本インターフェイスを持つ複数のインターフェイスを実装するときに、基本インターフェイスの 1 つだけの実装があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-352">When a class implements multiple interfaces that have the same base interface, there can be only one implementation of the base interface.</span></span> <span data-ttu-id="2a12a-353">例</span><span class="sxs-lookup"><span data-stu-id="2a12a-353">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
<span data-ttu-id="2a12a-354">別の実装を持つことはできません、`IControl`基底クラス リストで、名前付き、`IControl`によって継承`ITextBox`、および`IControl`によって継承`IListBox`。</span><span class="sxs-lookup"><span data-stu-id="2a12a-354">it is not possible to have separate implementations for the `IControl` named in the base class list, the `IControl` inherited by `ITextBox`, and the `IControl` inherited by `IListBox`.</span></span> <span data-ttu-id="2a12a-355">実際には、これらのインターフェイスに別々 の id の概念はありません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-355">Indeed, there is no notion of a separate identity for these interfaces.</span></span> <span data-ttu-id="2a12a-356">実装ではなく、`ITextBox`と`IListBox`の同じ実装を共有`IControl`、および`ComboBox`は単に 3 つのインターフェイスを実装すると見なされます`IControl`、 `ITextBox`、および`IListBox`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-356">Rather, the implementations of `ITextBox` and `IListBox` share the same implementation of `IControl`, and `ComboBox` is simply considered to implement three interfaces, `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="2a12a-357">基底クラスのメンバーは、インターフェイスのマッピングに参加します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-357">The members of a base class participate in interface mapping.</span></span> <span data-ttu-id="2a12a-358">例</span><span class="sxs-lookup"><span data-stu-id="2a12a-358">In the example</span></span>
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
<span data-ttu-id="2a12a-359">メソッド`F`で`Class1`で使用されて`Class2`の実装の`Interface1`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-359">the method `F` in `Class1` is used in `Class2`'s implementation of `Interface1`.</span></span>

### <a name="interface-implementation-inheritance"></a><span data-ttu-id="2a12a-360">インターフェイス実装の継承</span><span class="sxs-lookup"><span data-stu-id="2a12a-360">Interface implementation inheritance</span></span>

<span data-ttu-id="2a12a-361">クラスは、その基本クラスによって提供されるすべてのインターフェイスの実装を継承します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-361">A class inherits all interface implementations provided by its base classes.</span></span>

<span data-ttu-id="2a12a-362">せずに明示的に***再実装する***インターフェイス、派生クラス何らかの方法で変更できませんその基本クラスから継承するインターフェイス マップ。</span><span class="sxs-lookup"><span data-stu-id="2a12a-362">Without explicitly ***re-implementing*** an interface, a derived class cannot in any way alter the interface mappings it inherits from its base classes.</span></span> <span data-ttu-id="2a12a-363">たとえば、宣言内</span><span class="sxs-lookup"><span data-stu-id="2a12a-363">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
<span data-ttu-id="2a12a-364">`Paint`メソッド`TextBox`を非表示に、`Paint`メソッド`Control`のマッピングは変更されませんが、`Control.Paint`に`IControl.Paint`への呼び出しと`Paint`クラスとインターフェイスのインスタンスが、次の効果します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-364">the `Paint` method in `TextBox` hides the `Paint` method in `Control`, but it does not alter the mapping of `Control.Paint` onto `IControl.Paint`, and calls to `Paint` through class instances and interface instances will have the following effects</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

<span data-ttu-id="2a12a-365">ただし、インターフェイス メソッドがクラスでの仮想メソッドにマッピングされると、仮想メソッドをオーバーライドし、インターフェイスの実装を変更する派生クラスです。</span><span class="sxs-lookup"><span data-stu-id="2a12a-365">However, when an interface method is mapped onto a virtual method in a class, it is possible for derived classes to override the virtual method and alter the implementation of the interface.</span></span> <span data-ttu-id="2a12a-366">たとえば、上記の宣言の書き換え</span><span class="sxs-lookup"><span data-stu-id="2a12a-366">For example, rewriting the declarations above to</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
<span data-ttu-id="2a12a-367">次の効果は観察されたようになりました</span><span class="sxs-lookup"><span data-stu-id="2a12a-367">the following effects will now be observed</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

<span data-ttu-id="2a12a-368">明示的なインターフェイス メンバーの実装を virtual と宣言することはできません、ために、明示的なインターフェイス メンバーの実装をオーバーライドすることはできません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-368">Since explicit interface member implementations cannot be declared virtual, it is not possible to override an explicit interface member implementation.</span></span> <span data-ttu-id="2a12a-369">ただし、ことは、別のメソッドを呼び出す明示的なインターフェイス メンバーの実装は有効ですし、オーバーライドするクラスを派生する他のメソッドを許可する仮想宣言することができます、します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-369">However, it is perfectly valid for an explicit interface member implementation to call another method, and that other method can be declared virtual to allow derived classes to override it.</span></span> <span data-ttu-id="2a12a-370">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-370">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

<span data-ttu-id="2a12a-371">ここから派生したクラス`Control`の実装を特化できる`IControl.Paint`オーバーライドすることで、`PaintControl`メソッド。</span><span class="sxs-lookup"><span data-stu-id="2a12a-371">Here, classes derived from `Control` can specialize the implementation of `IControl.Paint` by overriding the `PaintControl` method.</span></span>

### <a name="interface-re-implementation"></a><span data-ttu-id="2a12a-372">インターフェイスの再実装</span><span class="sxs-lookup"><span data-stu-id="2a12a-372">Interface re-implementation</span></span>

<span data-ttu-id="2a12a-373">インターフェイスの実装を継承するクラスが許可されている***再実装***基底クラス リストに含めることによってインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="2a12a-373">A class that inherits an interface implementation is permitted to ***re-implement*** the interface by including it in the base class list.</span></span>

<span data-ttu-id="2a12a-374">インターフェイスの再実装では、まったく同じインターフェイス マッピング規則として、インターフェイスの初期の実装に従います。</span><span class="sxs-lookup"><span data-stu-id="2a12a-374">A re-implementation of an interface follows exactly the same interface mapping rules as an initial implementation of an interface.</span></span> <span data-ttu-id="2a12a-375">そのため、継承されたインターフェイスの割り当ては、インターフェイスの再実装用に確立されたインターフェイス マップに一切影響を与えません。</span><span class="sxs-lookup"><span data-stu-id="2a12a-375">Thus, the inherited interface mapping has no effect whatsoever on the interface mapping established for the re-implementation of the interface.</span></span> <span data-ttu-id="2a12a-376">たとえば、宣言内</span><span class="sxs-lookup"><span data-stu-id="2a12a-376">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
<span data-ttu-id="2a12a-377">事実を`Control`マップ`IControl.Paint`に`Control.IControl.Paint`で再実装には影響しません`MyControl`、マップを`IControl.Paint`に`MyControl.Paint`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-377">the fact that `Control` maps `IControl.Paint` onto `Control.IControl.Paint` doesn't affect the re-implementation in `MyControl`, which maps `IControl.Paint` onto `MyControl.Paint`.</span></span>

<span data-ttu-id="2a12a-378">パブリック メンバーの宣言と明示的なインターフェイスの継承されたメンバーの宣言が再実装されたインターフェイスのインターフェイス マップ プロセスに参加を継承します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-378">Inherited public member declarations and inherited explicit interface member declarations participate in the interface mapping process for re-implemented interfaces.</span></span> <span data-ttu-id="2a12a-379">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-379">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

<span data-ttu-id="2a12a-380">ここでは、の実装`IMethods`で`Derived`にインターフェイス メソッドのマップ`Derived.F`、 `Base.IMethods.G`、 `Derived.IMethods.H`、および`Base.I`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-380">Here, the implementation of `IMethods` in `Derived` maps the interface methods onto `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, and `Base.I`.</span></span>

<span data-ttu-id="2a12a-381">クラス インターフェイスを実装するときに、暗黙的にもすべてのインターフェイスの基本インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-381">When a class implements an interface, it implicitly also implements all of that interface's base interfaces.</span></span> <span data-ttu-id="2a12a-382">同様に、インターフェイスの再実装も暗黙的にすべてのインターフェイスの基本インターフェイスの再実装できます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-382">Likewise, a re-implementation of an interface is also implicitly a re-implementation of all of the interface's base interfaces.</span></span> <span data-ttu-id="2a12a-383">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-383">For example</span></span>
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

<span data-ttu-id="2a12a-384">ここでは、の再実装`IDerived`も再実装`IBase`マッピング`IBase.F`に`D.F`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-384">Here, the re-implementation of `IDerived` also re-implements `IBase`, mapping `IBase.F` onto `D.F`.</span></span>

### <a name="abstract-classes-and-interfaces"></a><span data-ttu-id="2a12a-385">抽象クラスとインターフェイス</span><span class="sxs-lookup"><span data-stu-id="2a12a-385">Abstract classes and interfaces</span></span>

<span data-ttu-id="2a12a-386">抽象クラスは、非抽象クラスと同様に、クラスの基底クラス リストに記載されているインターフェイスのすべてのメンバーの実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a12a-386">Like a non-abstract class, an abstract class must provide implementations of all members of the interfaces that are listed in the base class list of the class.</span></span> <span data-ttu-id="2a12a-387">ただし、抽象クラスは、インターフェイス メソッドを抽象メソッドにマップする許可されます。</span><span class="sxs-lookup"><span data-stu-id="2a12a-387">However, an abstract class is permitted to map interface methods onto abstract methods.</span></span> <span data-ttu-id="2a12a-388">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-388">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

<span data-ttu-id="2a12a-389">ここでは、の実装`IMethods`マップ`F`と`G`を抽象メソッドは、上にから派生した非抽象クラスでオーバーライドする必要があります`C`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-389">Here, the implementation of `IMethods` maps `F` and `G` onto abstract methods, which must be overridden in non-abstract classes that derive from `C`.</span></span>

<span data-ttu-id="2a12a-390">明示的なインターフェイス メンバーの実装を抽象にすることはできませんが、明示的なインターフェイス メンバーの実装が抽象メソッドの呼び出しを許可はもちろんことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2a12a-390">Note that explicit interface member implementations cannot be abstract, but explicit interface member implementations are of course permitted to call abstract methods.</span></span> <span data-ttu-id="2a12a-391">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-391">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

<span data-ttu-id="2a12a-392">ここでは、非抽象クラスから派生した`C`をオーバーライドする必要があります`FF`と`GG`、したがっての実際の実装を提供する`IMethods`します。</span><span class="sxs-lookup"><span data-stu-id="2a12a-392">Here, non-abstract classes that derive from `C` would be required to override `FF` and `GG`, thus providing the actual implementation of `IMethods`.</span></span>
