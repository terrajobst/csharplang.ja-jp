---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484082"
---
# <a name="default-interface-methods"></a><span data-ttu-id="0031c-101">既定のインターフェイスメソッド</span><span class="sxs-lookup"><span data-stu-id="0031c-101">default interface methods</span></span>

* <span data-ttu-id="0031c-102">[x] が提案されています</span><span class="sxs-lookup"><span data-stu-id="0031c-102">[x] Proposed</span></span>
* <span data-ttu-id="0031c-103">[] プロトタイプ:[実行](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)中</span><span class="sxs-lookup"><span data-stu-id="0031c-103">[ ] Prototype: [In progress](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span></span>
* <span data-ttu-id="0031c-104">[] の実装: なし</span><span class="sxs-lookup"><span data-stu-id="0031c-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="0031c-105">[] 仕様: 進行中、以下</span><span class="sxs-lookup"><span data-stu-id="0031c-105">[ ] Specification: In progress, below</span></span>

## <a name="summary"></a><span data-ttu-id="0031c-106">まとめ</span><span class="sxs-lookup"><span data-stu-id="0031c-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="0031c-107">_仮想拡張メソッド_のサポートを追加します。具体的な実装を持つインターフェイスにメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="0031c-107">Add support for _virtual extension methods_ - methods in interfaces with concrete implementations.</span></span> <span data-ttu-id="0031c-108">このようなインターフェイスを実装するクラスまたは構造体は、インターフェイスメソッドに対して、クラスまたは構造体によって実装されるか、基本クラスまたはインターフェイスから継承される、_最も限定的_な実装を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-108">A class or struct that implements such an interface is required to have a single _most specific_ implementation for the interface method, either implemented by the class or struct, or inherited from its base classes or interfaces.</span></span> <span data-ttu-id="0031c-109">仮想拡張メソッドを使用すると、API 作成者は、そのインターフェイスの既存の実装との間でソースまたはバイナリの互換性を損なうことなく、将来のバージョンでインターフェイスにメソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-109">Virtual extension methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>

<span data-ttu-id="0031c-110">これらは、Java の["既定のメソッド"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)に似ています。</span><span class="sxs-lookup"><span data-stu-id="0031c-110">These are similar to Java's ["Default Methods"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html).</span></span>

<span data-ttu-id="0031c-111">(考えられる実装手法に基づく) この機能では、CLI/CLR で対応するサポートが必要です。</span><span class="sxs-lookup"><span data-stu-id="0031c-111">(Based on the likely implementation technique) this feature requires corresponding support in the CLI/CLR.</span></span> <span data-ttu-id="0031c-112">この機能を利用するプログラムは、以前のバージョンのプラットフォームでは実行できません。</span><span class="sxs-lookup"><span data-stu-id="0031c-112">Programs that take advantage of this feature cannot run on earlier versions of the platform.</span></span>

## <a name="motivation"></a><span data-ttu-id="0031c-113">目的</span><span class="sxs-lookup"><span data-stu-id="0031c-113">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="0031c-114">この機能の主な動機は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="0031c-114">The principal motivations for this feature are</span></span>

- <span data-ttu-id="0031c-115">既定のインターフェイスメソッドを使用すると、API 作成者は、そのインターフェイスの既存の実装とソースまたはバイナリの互換性を損なうことなく、将来のバージョンでインターフェイスにメソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-115">Default interface methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>
- <span data-ttu-id="0031c-116">この機能をC#使用すると、は、同様の機能をサポートする[Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)と[iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)を対象とする api と相互運用できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-116">The feature enables C# to interoperate with APIs targeting [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) and [iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), which support similar features.</span></span>
- <span data-ttu-id="0031c-117">結局のところ、既定のインターフェイス実装を追加すると、"特徴" 言語機能 (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>) の要素が提供されます。</span><span class="sxs-lookup"><span data-stu-id="0031c-117">As it turns out, adding default interface implementations provides the elements of the "traits" language feature (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span></span> <span data-ttu-id="0031c-118">特徴は、強力なプログラミング手法 (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>) であることが実証されています。</span><span class="sxs-lookup"><span data-stu-id="0031c-118">Traits have proven to be a powerful programming technique (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span></span>

## <a name="detailed-design"></a><span data-ttu-id="0031c-119">詳細なデザイン</span><span class="sxs-lookup"><span data-stu-id="0031c-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="0031c-120">インターフェイスの構文は、を許可するように拡張されています。</span><span class="sxs-lookup"><span data-stu-id="0031c-120">The syntax for an interface is extended to permit</span></span>

- <span data-ttu-id="0031c-121">定数、演算子、静的コンストラクター、および入れ子にされた型を宣言するメンバー宣言。</span><span class="sxs-lookup"><span data-stu-id="0031c-121">member declarations that declare constants, operators, static constructors, and nested types;</span></span>
- <span data-ttu-id="0031c-122">メソッド、インデクサー、プロパティ、またはイベントアクセサーの*本体*(つまり、"既定" の実装)。</span><span class="sxs-lookup"><span data-stu-id="0031c-122">a *body* for a method or indexer, property, or event accessor (that is, a "default" implementation);</span></span>
- <span data-ttu-id="0031c-123">静的フィールド、メソッド、プロパティ、インデクサー、およびイベントを宣言するメンバー宣言</span><span class="sxs-lookup"><span data-stu-id="0031c-123">member declarations that declare static fields, methods, properties, indexers, and events;</span></span>
- <span data-ttu-id="0031c-124">明示的なインターフェイス実装構文を使用したメンバー宣言そして</span><span class="sxs-lookup"><span data-stu-id="0031c-124">member declarations using the explicit interface implementation syntax; and</span></span>
- <span data-ttu-id="0031c-125">明示的なアクセス修飾子 (既定のアクセスは `public`)。</span><span class="sxs-lookup"><span data-stu-id="0031c-125">Explicit access modifiers (the default access is `public`).</span></span>

<span data-ttu-id="0031c-126">本体を持つメンバーは、オーバーライドする実装を提供しないクラスと構造体のメソッドに対して、"既定の" 実装を提供するインターフェイスを許可します。</span><span class="sxs-lookup"><span data-stu-id="0031c-126">Members with bodies permit the interface to provide a "default" implementation for the method in classes and structs that do not provide an overriding implementation.</span></span>

<span data-ttu-id="0031c-127">インターフェイスには、インスタンスの状態を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="0031c-127">Interfaces may not contain instance state.</span></span> <span data-ttu-id="0031c-128">静的フィールドが許可されるようになりましたが、インスタンス フィールドはインターフェイスでは許可されません。</span><span class="sxs-lookup"><span data-stu-id="0031c-128">While static fields are now permitted, instance fields are not permitted in interfaces.</span></span> <span data-ttu-id="0031c-129">インスタンスの自動プロパティは、非表示フィールドを暗黙的に宣言するため、インターフェイスではサポートされません。</span><span class="sxs-lookup"><span data-stu-id="0031c-129">Instance auto-properties are not supported in interfaces, as they would implicitly declare a hidden field.</span></span>

<span data-ttu-id="0031c-130">静的メソッドとプライベートメソッドでは、インターフェイスのパブリック API を実装するために使用される、役に立つリファクタリングとコードの編成が可能です。</span><span class="sxs-lookup"><span data-stu-id="0031c-130">Static and private methods permit useful refactoring and organization of code used to implement the interface's public API.</span></span>

<span data-ttu-id="0031c-131">インターフェイスのメソッドオーバーライドでは、明示的なインターフェイスの実装構文を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-131">A method override in an interface must use the explicit interface implementation syntax.</span></span>

<span data-ttu-id="0031c-132">*Variance_annotation*で宣言された型パラメーターのスコープ内で、クラス型、構造体型、または列挙型を宣言すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="0031c-132">It is an error to declare a class type, struct type, or enum type within the scope of a type parameter that was declared with a *variance_annotation*.</span></span>  <span data-ttu-id="0031c-133">たとえば、次の `C` の宣言はエラーになります。</span><span class="sxs-lookup"><span data-stu-id="0031c-133">For example, the declaration of `C` below is an error.</span></span>

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a><span data-ttu-id="0031c-134">インターフェイスの具象メソッド</span><span class="sxs-lookup"><span data-stu-id="0031c-134">Concrete methods in interfaces</span></span>

<span data-ttu-id="0031c-135">この機能の最も単純な形式は、インターフェイスで*具象メソッド*を宣言する機能です。これは、本体を持つメソッドです。</span><span class="sxs-lookup"><span data-stu-id="0031c-135">The simplest form of this feature is the ability to declare a *concrete method* in an interface, which is a method with a body.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

<span data-ttu-id="0031c-136">このインターフェイスを実装するクラスは、具象メソッドを実装する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="0031c-136">A class that implements this interface need not implement its concrete method.</span></span>

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

<span data-ttu-id="0031c-137">クラス `C` 内の `IA.M` の最終的なオーバーライドは、`IA`で宣言さ `M` 具象メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0031c-137">The final override for `IA.M` in class `C` is the concrete method `M` declared in `IA`.</span></span> <span data-ttu-id="0031c-138">クラスは、そのインターフェイスからメンバーを継承しないことに注意してください。この機能によって変更されることはありません。</span><span class="sxs-lookup"><span data-stu-id="0031c-138">Note that a class does not inherit members from its interfaces; that is not changed by this feature:</span></span>

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

<span data-ttu-id="0031c-139">インターフェイスのインスタンスメンバー内では、`this` には外側のインターフェイスの型があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-139">Within an instance member of an interface, `this` has the type of the enclosing interface.</span></span>

### <a name="modifiers-in-interfaces"></a><span data-ttu-id="0031c-140">インターフェイス内の修飾子</span><span class="sxs-lookup"><span data-stu-id="0031c-140">Modifiers in interfaces</span></span>

<span data-ttu-id="0031c-141">インターフェイスの構文は、メンバーに対して修飾子を許可するためには緩和されています。</span><span class="sxs-lookup"><span data-stu-id="0031c-141">The syntax for an interface is relaxed to permit modifiers on its members.</span></span> <span data-ttu-id="0031c-142">`private`、`protected`、`internal`、`public`、`virtual`、`abstract`、`sealed`、`static`、`extern`、`partial`を使用できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-142">The following are permitted: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`, and `partial`.</span></span>

> <span data-ttu-id="0031c-143">***TODO***: 他の修飾子が存在するかどうかを確認してください。</span><span class="sxs-lookup"><span data-stu-id="0031c-143">***TODO***: check what other modifiers exist.</span></span>

<span data-ttu-id="0031c-144">宣言が本体を含むインターフェイスメンバーは、`sealed` または `private` 修飾子が使用されていない限り、`virtual` メンバーです。</span><span class="sxs-lookup"><span data-stu-id="0031c-144">An interface member whose declaration includes a body is a `virtual` member unless the `sealed` or `private` modifier is used.</span></span> <span data-ttu-id="0031c-145">`virtual` 修飾子は、暗黙的に `virtual`される関数メンバーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-145">The `virtual` modifier may be used on a function member that would otherwise be implicitly `virtual`.</span></span> <span data-ttu-id="0031c-146">同様に、本体のないインターフェイスメンバーの既定値は `abstract` ですが、修飾子を明示的に指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="0031c-146">Similarly, although `abstract` is the default on interface members without bodies, that modifier may be given explicitly.</span></span> <span data-ttu-id="0031c-147">非仮想メンバーは、`sealed` キーワードを使用して宣言できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-147">A non-virtual member may be declared using the `sealed` keyword.</span></span>

<span data-ttu-id="0031c-148">`private` またはインターフェイスの `sealed` 関数メンバーが本文を持たない場合はエラーになります。</span><span class="sxs-lookup"><span data-stu-id="0031c-148">It is an error for a `private` or `sealed` function member of an interface to have no body.</span></span> <span data-ttu-id="0031c-149">`private` 関数メンバーに修飾子 `sealed`を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="0031c-149">A `private` function member may not have the modifier `sealed`.</span></span>

<span data-ttu-id="0031c-150">アクセス修飾子は、許可されているすべての種類のメンバーのインターフェイスメンバーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-150">Access modifiers may be used on interface members of all kinds of members that are permitted.</span></span> <span data-ttu-id="0031c-151">`public` アクセスレベルは既定値ですが、明示的に指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="0031c-151">The access level `public` is the default but it may be given explicitly.</span></span>

> <span data-ttu-id="0031c-152">***懸案事項を開く:***`protected` や `internal`などのアクセス修飾子の正確な意味と、これらの宣言 (派生インターフェイスの場合) または実装しない宣言 (インターフェイスを実装するクラス) を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-152">***Open Issue:*** We need to specify the precise meaning of the access modifiers such as `protected` and `internal`, and which declarations do and do not override them (in a derived interface) or implement them (in a class that implements the interface).</span></span>

<span data-ttu-id="0031c-153">インターフェイスは、入れ子にされた型、メソッド、インデクサー、プロパティ、イベント、および静的コンストラクターを含む `static` メンバーを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-153">Interfaces may declare `static` members, including nested types, methods, indexers, properties, events, and static constructors.</span></span> <span data-ttu-id="0031c-154">すべてのインターフェイスメンバーの既定のアクセスレベルは `public`です。</span><span class="sxs-lookup"><span data-stu-id="0031c-154">The default access level for all interface members is `public`.</span></span>

<span data-ttu-id="0031c-155">インターフェイスでは、インスタンスコンストラクター、デストラクター、またはフィールドを宣言できません。</span><span class="sxs-lookup"><span data-stu-id="0031c-155">Interfaces may not declare instance constructors, destructors, or fields.</span></span>

> <span data-ttu-id="0031c-156">***終了済みの問題:*** インターフェイスで演算子宣言を許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-156">***Closed Issue:*** Should operator declarations be permitted in an interface?</span></span> <span data-ttu-id="0031c-157">変換演算子ではありませんが、他の演算子についてはどうでしょうか。</span><span class="sxs-lookup"><span data-stu-id="0031c-157">Probably not conversion operators, but what about others?</span></span> <span data-ttu-id="0031c-158">***Decision***: 変換演算子、等値演算子、および非等値演算子を*除き*、演算子を使用できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-158">***Decision***: Operators are permitted *except* for conversion, equality, and inequality operators.</span></span>

> <span data-ttu-id="0031c-159">***終了済みの問題:*** 基底インターフェイスのメンバーを非表示にするインターフェイスメンバー宣言では、`new` 許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-159">***Closed Issue:*** Should `new` be permitted on interface member declarations that hide members from base interfaces?</span></span> <span data-ttu-id="0031c-160">***決定***: はい。</span><span class="sxs-lookup"><span data-stu-id="0031c-160">***Decision***: Yes.</span></span>

> <span data-ttu-id="0031c-161">***終了済みの問題:*** 現在、インターフェイスまたはそのメンバーの `partial` は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="0031c-161">***Closed Issue:*** We do not currently permit `partial` on an interface or its members.</span></span> <span data-ttu-id="0031c-162">これには別の提案が必要です。</span><span class="sxs-lookup"><span data-stu-id="0031c-162">That would require a separate proposal.</span></span> <span data-ttu-id="0031c-163">***決定***: はい。</span><span class="sxs-lookup"><span data-stu-id="0031c-163">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a><span data-ttu-id="0031c-164">インターフェイスでのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="0031c-164">Overrides in interfaces</span></span>

<span data-ttu-id="0031c-165">オーバーライド宣言 (つまり、`override` 修飾子を含む) を使用すると、プログラマは、コンパイラまたはランタイムがそれ以外の方法で仮想メンバーを見つけることができないようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="0031c-165">Override declarations (i.e. those containing the `override` modifier) allow the programmer to provide a most specific implementation of a virtual member in an interface where the compiler or runtime would not otherwise find one.</span></span> <span data-ttu-id="0031c-166">また、スーパーインターフェイスの抽象メンバーを、派生インターフェイスの既定のメンバーに変えることもできます。</span><span class="sxs-lookup"><span data-stu-id="0031c-166">It also allows turning an abstract member from a super-interface into a default member in a derived interface.</span></span> <span data-ttu-id="0031c-167">オーバーライド宣言では、インターフェイス名で宣言を修飾することによって、特定の基底インターフェイスメソッドを*明示的*にオーバーライドできます (この場合、アクセス修飾子は許可されません)。</span><span class="sxs-lookup"><span data-stu-id="0031c-167">An override declaration is permitted to *explicitly* override a particular base interface method by qualifying the declaration with the interface name (no access modifier is permitted in this case).</span></span> <span data-ttu-id="0031c-168">暗黙のオーバーライドは許可されていません。</span><span class="sxs-lookup"><span data-stu-id="0031c-168">Implicit overrides are not permitted.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

<span data-ttu-id="0031c-169">インターフェイスのオーバーライド宣言を `sealed`として宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="0031c-169">Override declarations in interfaces may not be declared `sealed`.</span></span>

<span data-ttu-id="0031c-170">インターフェイスのパブリック `virtual` 関数メンバーは、派生インターフェイスで明示的にオーバーライドすることができます (最初にメソッドを宣言したインターフェイス型でオーバーライド宣言内の名前を修飾し、アクセス修飾子を省略します)。</span><span class="sxs-lookup"><span data-stu-id="0031c-170">Public `virtual` function members in an interface may be overridden in a derived interface explicitly (by qualifying the name in the override declaration with the interface type that originally declared the method, and omitting an access modifier).</span></span>

<span data-ttu-id="0031c-171">インターフェイス内の `virtual` 関数メンバーは、派生インターフェイスで明示的に (暗黙的にではなく) オーバーライドできます。また、`public` されていないメンバーは、明示的に (暗黙的にではなく) クラスまたは構造体にのみ実装できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-171">`virtual` function members in an interface may only be overridden explicitly (not implicitly) in derived interfaces, and members that are not `public` may only be implemented in a class or struct explicitly (not implicitly).</span></span> <span data-ttu-id="0031c-172">どちらの場合も、オーバーライドまたは実装されたメンバーは、オーバーライドされた場所に*アクセス*できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-172">In either case, the overridden or implemented member must be *accessible* where it is overridden.</span></span>

### <a name="reabstraction"></a><span data-ttu-id="0031c-173">再抽象化</span><span class="sxs-lookup"><span data-stu-id="0031c-173">Reabstraction</span></span>

<span data-ttu-id="0031c-174">インターフェイスで宣言された仮想 (具象) メソッドは、派生インターフェイスで抽象になるようにオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="0031c-174">A virtual (concrete) method declared in an interface may be overridden to be abstract in a derived interface</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

<span data-ttu-id="0031c-175">`abstract` 修飾子は、`IB.M` の宣言 (インターフェイスの既定値) には必要ありませんが、オーバーライド宣言では明示的にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="0031c-175">The `abstract` modifier is not required in the declaration of `IB.M` (that is the default in interfaces), but it is probably good practice to be explicit in an override declaration.</span></span>

<span data-ttu-id="0031c-176">これは、メソッドの既定の実装が不適切で、実装クラスによってより適切な実装を提供する必要がある派生インターフェイスで役立ちます。</span><span class="sxs-lookup"><span data-stu-id="0031c-176">This is useful in derived interfaces where the default implementation of a method is inappropriate and a more appropriate implementation should be provided by implementing classes.</span></span>

> <span data-ttu-id="0031c-177">***懸案事項を開く:*** 再抽象化を許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-177">***Open Issue:*** Should reabstraction be permitted?</span></span>

### <a name="the-most-specific-override-rule"></a><span data-ttu-id="0031c-178">最も限定的な上書きルール</span><span class="sxs-lookup"><span data-stu-id="0031c-178">The most specific override rule</span></span>

<span data-ttu-id="0031c-179">すべてのインターフェイスとクラスは、型またはその直接および間接インターフェイスに表示されるオーバーライドのすべての仮想メンバーに対して、*最も限定的なオーバーライド*を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-179">We require that every interface and class have a *most specific override* for every virtual member among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="0031c-180">*最も具体的*なオーバーライドは、他のすべての上書きよりも固有の一意のオーバーライドです。</span><span class="sxs-lookup"><span data-stu-id="0031c-180">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="0031c-181">オーバーライドがない場合、メンバー自体は最も具体的なオーバーライドと見なされます。</span><span class="sxs-lookup"><span data-stu-id="0031c-181">If there is no override, the member itself is considered the most specific override.</span></span>

<span data-ttu-id="0031c-182">`M1` が型 `T1`で宣言されていて、`M2` が型 `T2`で宣言されている場合は、1つのオーバーライド `M1` が別のオーバーライドよりも*具体的*に `M2` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="0031c-182">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>

1. <span data-ttu-id="0031c-183">`T1` には、直接または間接のインターフェイスの `T2` が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0031c-183">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
2. <span data-ttu-id="0031c-184">`T2` はインターフェイス型ですが、`T1` はインターフェイス型ではありません。</span><span class="sxs-lookup"><span data-stu-id="0031c-184">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="0031c-185">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="0031c-185">For example:</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

<span data-ttu-id="0031c-186">最も限定的なオーバーライド規則では、競合が発生した時点でプログラマが明示的に競合を解決します (つまり、ダイヤモンドの継承によって生じるあいまいさが生じます)。</span><span class="sxs-lookup"><span data-stu-id="0031c-186">The most specific override rule ensures that a conflict (i.e. an ambiguity arising from diamond inheritance) is resolved explicitly by the programmer at the point where the conflict arises.</span></span>

<span data-ttu-id="0031c-187">インターフェイスでは明示的な抽象オーバーライドがサポートされているため、クラスでもこれを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="0031c-187">Because we support explicit abstract overrides in interfaces, we could do so in classes as well</span></span>

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> <span data-ttu-id="0031c-188">***懸案事項を開く***: クラスの明示的なインターフェイス抽象オーバーライドをサポートする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-188">***Open issue***: should we support explicit interface abstract overrides in classes?</span></span>

<span data-ttu-id="0031c-189">さらに、クラス宣言内で、一部のインターフェイスメソッドの最も限定的なオーバーライドが、インターフェイスで宣言された抽象オーバーライドである場合は、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="0031c-189">In addition, it is an error if in a class declaration the most specific override of some interface method is an abstract override that was declared in an interface.</span></span> <span data-ttu-id="0031c-190">これは、新しい用語を使用した既存のルール反映先です。</span><span class="sxs-lookup"><span data-stu-id="0031c-190">This is an existing rule restated using the new terminology.</span></span>

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

<span data-ttu-id="0031c-191">インターフェイスで宣言された仮想プロパティは、1つのインターフェイスで `get` アクセサーに対して最も限定的なオーバーライドを持つことができます。また、別のインターフェイスの `set` アクセサーに対して最も具体的なオーバーライドを行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="0031c-191">It is possible for a virtual property declared in an interface to have a most specific override for its `get` accessor in one interface and a most specific override for its `set` accessor in a different interface.</span></span> <span data-ttu-id="0031c-192">これは、*最も限定的な上書き*ルールの違反と見なされます。</span><span class="sxs-lookup"><span data-stu-id="0031c-192">This is considered a violation of the *most specific override* rule.</span></span>

### <a name="static-and-private-methods"></a><span data-ttu-id="0031c-193">`static` メソッドおよび `private` メソッド</span><span class="sxs-lookup"><span data-stu-id="0031c-193">`static` and `private` methods</span></span>

<span data-ttu-id="0031c-194">インターフェイスには実行可能コードが含まれている可能性があるため、共通のコードをプライベートメソッドと静的メソッドに抽象すると便利です。</span><span class="sxs-lookup"><span data-stu-id="0031c-194">Because interfaces may now contain executable code, it is useful to abstract common code into private and static methods.</span></span> <span data-ttu-id="0031c-195">これらはインターフェイスで許可されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="0031c-195">We now permit these in interfaces.</span></span>

> <span data-ttu-id="0031c-196">***終了***済みの問題: プライベートメソッドをサポートする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-196">***Closed issue***: Should we support private methods?</span></span> <span data-ttu-id="0031c-197">静的メソッドをサポートする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-197">Should we support static methods?</span></span> <span data-ttu-id="0031c-198">**決定: はい**</span><span class="sxs-lookup"><span data-stu-id="0031c-198">**Decision: YES**</span></span>

> <span data-ttu-id="0031c-199">***懸案事項を開く***: インターフェイスメソッドが `protected` または `internal` またはその他のアクセスを許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-199">***Open issue***: should we permit interface methods to be `protected` or `internal` or other access?</span></span> <span data-ttu-id="0031c-200">その場合、どのような意味があるのでしょうか。</span><span class="sxs-lookup"><span data-stu-id="0031c-200">If so, what are the semantics?</span></span> <span data-ttu-id="0031c-201">既定では `virtual` ますか?</span><span class="sxs-lookup"><span data-stu-id="0031c-201">Are they `virtual` by default?</span></span> <span data-ttu-id="0031c-202">そうでない場合、非仮想化を行う方法はありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-202">If so, is there a way to make them non-virtual?</span></span>

> <span data-ttu-id="0031c-203">***未解決の問題***: 静的メソッドをサポートしている場合は、(静的) 演算子をサポートする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-203">***Open issue***: If we support static methods, should we support (static) operators?</span></span>

### <a name="base-interface-invocations"></a><span data-ttu-id="0031c-204">基本インターフェイスの呼び出し</span><span class="sxs-lookup"><span data-stu-id="0031c-204">Base interface invocations</span></span>

<span data-ttu-id="0031c-205">既定のメソッドを使用してインターフェイスから派生した型のコードでは、そのインターフェイスの "基本" 実装を明示的に呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="0031c-205">Code in a type that derives from an interface with a default method can explicitly invoke that interface's "base" implementation.</span></span>

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

<span data-ttu-id="0031c-206">インスタンス (非静的) メソッドは、`base(Type).M`構文を使用して名前を付けることによって、直接基底インターフェイスのアクセス可能なインスタンスメソッドの実装を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="0031c-206">An instance (nonstatic) method is permitted to invoke the implementation of an accessible instance method in a direct base interface nonvirtually by naming it using the syntax `base(Type).M`.</span></span> <span data-ttu-id="0031c-207">これは、ダイヤモンド継承によって提供される必要があるオーバーライドが、1つの特定の基本実装に委任することによって解決される場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="0031c-207">This is useful when an override that is required to be provided due to diamond inheritance is resolved by delegating to one particular base implementation.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

<span data-ttu-id="0031c-208">構文 `base(Type).M`を使用して `virtual` または `abstract` のメンバーにアクセスする場合、`Type` には `M`に対して*最も固有*の一意のオーバーライドが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-208">When a `virtual` or `abstract` member is accessed using the syntax `base(Type).M`, it is required that `Type` contains a unique *most specific override* for `M`.</span></span>

### <a name="binding-base-clauses"></a><span data-ttu-id="0031c-209">基本句のバインド</span><span class="sxs-lookup"><span data-stu-id="0031c-209">Binding base clauses</span></span>

<span data-ttu-id="0031c-210">インターフェイスに型が含まれるようになりました。</span><span class="sxs-lookup"><span data-stu-id="0031c-210">Interfaces now contain types.</span></span>  <span data-ttu-id="0031c-211">これらの型は、基本のインターフェイスとしてベース句で使用できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-211">These types may be used in the base clause as base interfaces.</span></span>  <span data-ttu-id="0031c-212">ベース句をバインドするときに、これらの型をバインドする基本インターフェイスのセットを知る必要がある場合があります (たとえば、参照して保護されたアクセスを解決する場合)。</span><span class="sxs-lookup"><span data-stu-id="0031c-212">When binding a base clause, we may need to know the set of base interfaces to bind those types (e.g. to lookup in them and to resolve protected access).</span></span>  <span data-ttu-id="0031c-213">そのため、インターフェイスのベース句の意味が循環的に定義されています。</span><span class="sxs-lookup"><span data-stu-id="0031c-213">The meaning of an interface's base clause is thus circularly defined.</span></span>  <span data-ttu-id="0031c-214">サイクルを中断するには、クラスに対して既に設定されている同様の規則に対応する新しい言語規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="0031c-214">To break the cycle, we add a new language rules corresponding to a similar rule already in place for classes.</span></span>

<span data-ttu-id="0031c-215">インターフェイスの*interface_base*の意味を判断する際、基本インターフェイスは一時的に空であると見なされます。</span><span class="sxs-lookup"><span data-stu-id="0031c-215">While determining the meaning of the *interface_base* of an interface, the base interfaces are temporarily assumed to be empty.</span></span> <span data-ttu-id="0031c-216">直感的に言えば、ベース句の意味を再帰的に依存させることはできません。</span><span class="sxs-lookup"><span data-stu-id="0031c-216">Intuitively this ensures that the meaning of a base clause cannot recursively depend on itself.</span></span> 

<span data-ttu-id="0031c-217">**ここでは、次の規則を使用しました。**</span><span class="sxs-lookup"><span data-stu-id="0031c-217">**We used to have the following rules:**</span></span>

<span data-ttu-id="0031c-218">"クラス B がクラス A から派生した場合、が B に依存している場合は、コンパイル時にエラーが発生します。クラスは、直接基底クラス (存在する場合)**に依存**し、そのクラスがすぐに入れ子になっている~~**クラス**~~**に直接依存**します (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="0031c-218">"When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the ~~**class**~~ within which it is immediately nested (if any).</span></span> <span data-ttu-id="0031c-219">この定義により、クラスが依存する~~**クラス**~~の完全なセットは、リレーションシップ**に直接依存**する再帰的および推移的なクロージャです。 "</span><span class="sxs-lookup"><span data-stu-id="0031c-219">Given this definition, the complete set of ~~**classes**~~ upon which a class depends is the reflexive and transitive closure of the **directly depends on** relationship."</span></span>

<span data-ttu-id="0031c-220">インターフェイスがそれ自体から直接または間接的に継承する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="0031c-220">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>
<span data-ttu-id="0031c-221">インターフェイスの**基本インターフェイス**は、明示的な基本インターフェイスとその基本インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="0031c-221">The **base interfaces** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="0031c-222">つまり、基本インターフェイスのセットは、明示的な基本インターフェイス、その明示的な基本インターフェイスなどの完全な推移的なクロージャです。</span><span class="sxs-lookup"><span data-stu-id="0031c-222">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span>

<span data-ttu-id="0031c-223">**次のように調整します。**</span><span class="sxs-lookup"><span data-stu-id="0031c-223">**We are adjusting them as follows:**</span></span>

<span data-ttu-id="0031c-224">クラス B がクラス A から派生した場合、A が B に依存していると、コンパイル時にエラーになります。クラスは、直接の基底クラス (存在する場合)**に依存**し、すぐに入れ子になっている _**型**_ **に直接**依存します (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="0031c-224">When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the _**type**_ within which it is immediately nested (if any).</span></span>

<span data-ttu-id="0031c-225">インターフェイス IB によって IA が拡張されると、IA が IB に依存している場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="0031c-225">When an interface IB extends an interface IA, it is a compile-time error for IA to depend on IB.</span></span> <span data-ttu-id="0031c-226">インターフェイスは、直接の基本インターフェイス (存在する場合)**に依存**し、直接的に入れ子になっている型 (存在する場合)**に**依存します。</span><span class="sxs-lookup"><span data-stu-id="0031c-226">An interface **directly depends on** its direct base interfaces (if any) and **directly depends on** the type within which it is immediately nested (if any).</span></span>

<span data-ttu-id="0031c-227">これらの定義を指定すると、型が依存する**型**の完全なセットは、リレーションシップ**に直接依存**します。</span><span class="sxs-lookup"><span data-stu-id="0031c-227">Given these definitions, the complete set of **types** upon which a type depends is the reflexive and transitive closure of the **directly depends on** relationship.</span></span>

### <a name="effect-on-existing-programs"></a><span data-ttu-id="0031c-228">既存のプログラムへの影響</span><span class="sxs-lookup"><span data-stu-id="0031c-228">Effect on existing programs</span></span>

<span data-ttu-id="0031c-229">ここに示すルールは、既存のプログラムの意味に影響を与えないことを目的としています。</span><span class="sxs-lookup"><span data-stu-id="0031c-229">The rules presented here are intended to have no effect on the meaning of existing programs.</span></span>

<span data-ttu-id="0031c-230">例 1:</span><span class="sxs-lookup"><span data-stu-id="0031c-230">Example 1:</span></span>

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

<span data-ttu-id="0031c-231">例 2:</span><span class="sxs-lookup"><span data-stu-id="0031c-231">Example 2:</span></span>

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

<span data-ttu-id="0031c-232">同じ規則によって、既定のインターフェイスメソッドを含む同様の状況に似た結果が得られます。</span><span class="sxs-lookup"><span data-stu-id="0031c-232">The same rules give similar results to the analogous situation involving default interface methods:</span></span>

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> <span data-ttu-id="0031c-233">***終了***済みの問題: これが仕様の意図した結果であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0031c-233">***Closed issue***: confirm that this is an intended consequence of the specification.</span></span> <span data-ttu-id="0031c-234">**決定: はい**</span><span class="sxs-lookup"><span data-stu-id="0031c-234">**Decision: YES**</span></span>

### <a name="runtime-method-resolution"></a><span data-ttu-id="0031c-235">ランタイムメソッドの解決</span><span class="sxs-lookup"><span data-stu-id="0031c-235">Runtime method resolution</span></span>

> <span data-ttu-id="0031c-236">***終了済みの問題:*** この仕様では、インターフェイスの既定のメソッドの面でのランタイムメソッドの解決アルゴリズムを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-236">***Closed Issue:*** The spec should describe the runtime method resolution algorithm in the face of interface default methods.</span></span> <span data-ttu-id="0031c-237">セマンティクスと言語のセマンティクスが一致していることを確認する必要があります。たとえば、宣言されたメソッドが、`internal` メソッドをオーバーライドしたり実装したりしないようにします。</span><span class="sxs-lookup"><span data-stu-id="0031c-237">We need to ensure that the semantics are consistent with the language semantics, e.g. which declared methods do and do not override or implement an `internal` method.</span></span>

### <a name="clr-support-api"></a><span data-ttu-id="0031c-238">CLR サポート API</span><span class="sxs-lookup"><span data-stu-id="0031c-238">CLR support API</span></span>

<span data-ttu-id="0031c-239">コンパイラがこの機能をサポートするランタイムに対してコンパイルされているかどうかを検出するために、このようなランタイムのライブラリは <https://github.com/dotnet/corefx/issues/17116>で説明されている API を介してその事実を提供するように変更されます。</span><span class="sxs-lookup"><span data-stu-id="0031c-239">In order for compilers to detect when they are compiling for a runtime that supports this feature, libraries for such runtimes are modified to advertise that fact through the API discussed in <https://github.com/dotnet/corefx/issues/17116>.</span></span> <span data-ttu-id="0031c-240">次のように追加します。</span><span class="sxs-lookup"><span data-stu-id="0031c-240">We add</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> <span data-ttu-id="0031c-241">***懸案事項を開く***: *CLR*機能の最適な名前</span><span class="sxs-lookup"><span data-stu-id="0031c-241">***Open issue***: Is that the best name for the *CLR* feature?</span></span> <span data-ttu-id="0031c-242">CLR 機能は、それだけではありません (たとえば、緩和されの保護制約、インターフェイスでのオーバーライドのサポートなど)。</span><span class="sxs-lookup"><span data-stu-id="0031c-242">The CLR feature does much more than just that (e.g. relaxes protection constraints, supports overrides in interfaces, etc).</span></span> <span data-ttu-id="0031c-243">"インターフェイス内の具象メソッド" や "特徴" などを呼び出す必要があるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="0031c-243">Perhaps it should be called something like "concrete methods in interfaces", or "traits"?</span></span>

### <a name="further-areas-to-be-specified"></a><span data-ttu-id="0031c-244">指定する領域の追加</span><span class="sxs-lookup"><span data-stu-id="0031c-244">Further areas to be specified</span></span>

- <span data-ttu-id="0031c-245">[] 既定のインターフェイスメソッドとオーバーライドを既存のインターフェイスに追加することによって発生するソースとバイナリの互換性の影響の種類をカタログ化すると便利です。</span><span class="sxs-lookup"><span data-stu-id="0031c-245">[ ] It would be useful to catalog the kinds of source and binary compatibility effects caused by adding default interface methods and overrides to existing interfaces.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="0031c-246">短所</span><span class="sxs-lookup"><span data-stu-id="0031c-246">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="0031c-247">この提案には、CLR 仕様の調整された更新が必要です (インターフェイスとメソッドの解決法の具象メソッドをサポートするため)。</span><span class="sxs-lookup"><span data-stu-id="0031c-247">This proposal requires a coordinated update to the CLR specification (to support concrete methods in interfaces and method resolution).</span></span> <span data-ttu-id="0031c-248">そのため、非常に "高額" なので、CLR の変更が必要になることも予想される他の機能と組み合わせて使用する価値があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-248">It is therefore fairly "expensive" and it may be worth doing in combination with other features that we also anticipate would require CLR changes.</span></span>

## <a name="alternatives"></a><span data-ttu-id="0031c-249">代替</span><span class="sxs-lookup"><span data-stu-id="0031c-249">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="0031c-250">[なし] :</span><span class="sxs-lookup"><span data-stu-id="0031c-250">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="0031c-251">未解決の質問</span><span class="sxs-lookup"><span data-stu-id="0031c-251">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="0031c-252">未解決の質問は、上記の提案の中でも呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="0031c-252">Open questions are called out throughout the proposal, above.</span></span>
- <span data-ttu-id="0031c-253">開いている質問の一覧については、「<https://github.com/dotnet/csharplang/issues/406>」も参照してください。</span><span class="sxs-lookup"><span data-stu-id="0031c-253">See also <https://github.com/dotnet/csharplang/issues/406> for a list of open questions.</span></span>
- <span data-ttu-id="0031c-254">詳細な仕様では、呼び出される正確なメソッドを選択するために、実行時に使用される解決機構を記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-254">The detailed specification must describe the resolution mechanism used at runtime to select the precise method to be invoked.</span></span>
- <span data-ttu-id="0031c-255">新しいコンパイラによって生成され、以前のコンパイラによって使用されるメタデータの相互作用は、詳しく説明する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-255">The interaction of metadata produced by new compilers and consumed by older compilers needs to be worked out in detail.</span></span> <span data-ttu-id="0031c-256">たとえば、以前のコンパイラによってコンパイルされたときに、そのインターフェイスを実装する既存のクラスを中断するために、使用するメタデータ表現によってインターフェイスに既定の実装が追加されないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-256">For example, we need to ensure that the metadata representation that we use does not cause the addition of a default implementation in an interface to break an existing class that implements that interface when compiled by an older compiler.</span></span> <span data-ttu-id="0031c-257">これは、使用できるメタデータ表現に影響を与える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-257">This may affect the metadata representation that we can use.</span></span>
- <span data-ttu-id="0031c-258">この設計では、他の言語と他の言語の既存のコンパイラとの相互運用を検討する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-258">The design must consider interoperation with other languages and existing compilers for other languages.</span></span>

## <a name="resolved-questions"></a><span data-ttu-id="0031c-259">解決済みの質問</span><span class="sxs-lookup"><span data-stu-id="0031c-259">Resolved Questions</span></span>

### <a name="abstract-override"></a><span data-ttu-id="0031c-260">抽象オーバーライド</span><span class="sxs-lookup"><span data-stu-id="0031c-260">Abstract Override</span></span>

<span data-ttu-id="0031c-261">以前のドラフト仕様には、継承されたメソッドを "再 abstract" する機能が含まれていました。</span><span class="sxs-lookup"><span data-stu-id="0031c-261">The earlier draft spec contained the ability to "reabstract" an inherited method:</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

<span data-ttu-id="0031c-262">2017-03-20 のメモでは、このことを許可しないことにしました。</span><span class="sxs-lookup"><span data-stu-id="0031c-262">My notes for 2017-03-20 showed that we decided not to allow this.</span></span> <span data-ttu-id="0031c-263">ただし、少なくとも2つのユースケースがあります。</span><span class="sxs-lookup"><span data-stu-id="0031c-263">However, there are at least two use cases for it:</span></span>

1. <span data-ttu-id="0031c-264">この機能の一部のユーザーが相互運用を期待する Java Api は、この機能に依存しています。</span><span class="sxs-lookup"><span data-stu-id="0031c-264">The Java APIs, with which some users of this feature hope to interoperate, depend on this facility.</span></span>
2. <span data-ttu-id="0031c-265">この点からの*特徴*によるプログラミング。</span><span class="sxs-lookup"><span data-stu-id="0031c-265">Programming with *traits* benefits from this.</span></span> <span data-ttu-id="0031c-266">Reabstraction は、"特徴" 言語機能 (https://en.wikipedia.org/wiki/Trait_(computer_programming))の要素の1つです。</span><span class="sxs-lookup"><span data-stu-id="0031c-266">Reabstraction is one of the elements of the "traits" language feature (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span></span> <span data-ttu-id="0031c-267">クラスでは、次のものを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-267">The following is permitted with classes:</span></span>

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

<span data-ttu-id="0031c-268">残念ながら、このコードは、許可されている場合を除き、インターフェイス (特徴) のセットとしてリファクタリングすることはできません。</span><span class="sxs-lookup"><span data-stu-id="0031c-268">Unfortunately this code cannot be refactored as a set of interfaces (traits) unless this is permitted.</span></span> <span data-ttu-id="0031c-269">このような場合は、 *jared の原則*により、これを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-269">By the *Jared principle of greed*, it should be permitted.</span></span>

> <span data-ttu-id="0031c-270">***終了済みの問題:*** 再抽象化を許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-270">***Closed issue:*** Should reabstraction be permitted?</span></span> <span data-ttu-id="0031c-271">イエスメモが間違っています。</span><span class="sxs-lookup"><span data-stu-id="0031c-271">[YES] My notes were wrong.</span></span> <span data-ttu-id="0031c-272">[LDM note](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)は、インターフェイスで reabstraction が許可されていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="0031c-272">The [LDM notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) say that reabstraction is permitted in an interface.</span></span> <span data-ttu-id="0031c-273">クラスにありません。</span><span class="sxs-lookup"><span data-stu-id="0031c-273">Not in a class.</span></span>

### <a name="virtual-modifier-vs-sealed-modifier"></a><span data-ttu-id="0031c-274">仮想修飾子と Sealed 修飾子</span><span class="sxs-lookup"><span data-stu-id="0031c-274">Virtual Modifier vs Sealed Modifier</span></span>

<span data-ttu-id="0031c-275">[Aleksey Tsingz](https://github.com/AlekseyTs)から:</span><span class="sxs-lookup"><span data-stu-id="0031c-275">From [Aleksey Tsingauz](https://github.com/AlekseyTs):</span></span>

> <span data-ttu-id="0031c-276">インターフェイスメンバーの一部を禁止する理由がない限り、修飾子を明示的に宣言することにしました。</span><span class="sxs-lookup"><span data-stu-id="0031c-276">We decided to allow modifiers explicitly stated on interface members, unless there is a reason to disallow some of them.</span></span> <span data-ttu-id="0031c-277">これにより、仮想修飾子に関する興味深い質問が発生します。</span><span class="sxs-lookup"><span data-stu-id="0031c-277">This brings an interesting question around virtual modifier.</span></span> <span data-ttu-id="0031c-278">既定の実装のメンバーで必要になるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="0031c-278">Should it be required on members with default implementation?</span></span>
>
> <span data-ttu-id="0031c-279">次のようなことが考えられます。</span><span class="sxs-lookup"><span data-stu-id="0031c-279">We could say that:</span></span>
>
> - <span data-ttu-id="0031c-280">実装がなく、仮想と sealed のどちらも指定されていない場合、メンバーは abstract であると想定されます。</span><span class="sxs-lookup"><span data-stu-id="0031c-280">if there is no implementation and neither virtual, nor sealed are specified, we assume the member is abstract.</span></span>
> - <span data-ttu-id="0031c-281">実装があり、abstract と sealed のどちらも指定されていない場合は、メンバーが virtual であると想定します。</span><span class="sxs-lookup"><span data-stu-id="0031c-281">if there is an implementation and neither abstract, nor sealed are specified, we assume the member is virtual.</span></span>
> - <span data-ttu-id="0031c-282">シール修飾子は、メソッドを仮想でも abstract にもできないようにするために必要です。</span><span class="sxs-lookup"><span data-stu-id="0031c-282">sealed modifier is required to make a method neither virtual, nor abstract.</span></span>
>
> <span data-ttu-id="0031c-283">または、仮想メンバーに仮想修飾子が必要であるということも考えられます。</span><span class="sxs-lookup"><span data-stu-id="0031c-283">Alternatively, we could say that virtual modifier is required for a virtual member.</span></span> <span data-ttu-id="0031c-284">つまり、仮想修飾子で明示的にマークされていない実装を持つメンバーがある場合は、仮想でも抽象でもありません。</span><span class="sxs-lookup"><span data-stu-id="0031c-284">I.e, if there is a member with implementation not explicitly marked with virtual modifier, it is neither virtual, nor abstract.</span></span> <span data-ttu-id="0031c-285">この方法では、メソッドをクラスからインターフェイスに移動したときに、より優れたエクスペリエンスが得られる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-285">This approach might provide better experience when a method is moved from a class to an interface:</span></span>
>
> - <span data-ttu-id="0031c-286">抽象メソッドは抽象のままです。</span><span class="sxs-lookup"><span data-stu-id="0031c-286">an abstract method stays abstract.</span></span>
> - <span data-ttu-id="0031c-287">仮想メソッドは仮想のままです。</span><span class="sxs-lookup"><span data-stu-id="0031c-287">a virtual method stays virtual.</span></span>
> - <span data-ttu-id="0031c-288">修飾子のないメソッドは、仮想でも abstract にもとどまりません。</span><span class="sxs-lookup"><span data-stu-id="0031c-288">a method without any modifier stays neither virtual, nor abstract.</span></span>
> - <span data-ttu-id="0031c-289">シールされた修飾子は、オーバーライドではないメソッドには適用できません。</span><span class="sxs-lookup"><span data-stu-id="0031c-289">sealed modifier cannot be applied to a method that is not an override.</span></span>
>
> <span data-ttu-id="0031c-290">どう思いますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-290">What do you think?</span></span>

> <span data-ttu-id="0031c-291">***終了済みの問題:*** 具象メソッド (実装を含む) を暗黙的に `virtual`する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-291">***Closed Issue:*** Should a concrete method (with implementation) be implicitly `virtual`?</span></span> <span data-ttu-id="0031c-292">イエス</span><span class="sxs-lookup"><span data-stu-id="0031c-292">[YES]</span></span>

<span data-ttu-id="0031c-293">***決定:*** LDM 2017-04-05 で作成されました。</span><span class="sxs-lookup"><span data-stu-id="0031c-293">***Decisions:*** Made in the LDM 2017-04-05:</span></span>

1. <span data-ttu-id="0031c-294">非仮想は、`sealed` または `private`を通じて明示的に表現する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-294">non-virtual should be explicitly expressed through `sealed` or `private`.</span></span>
2. <span data-ttu-id="0031c-295">`sealed` は、インターフェイスインスタンスのメンバーを非仮想ボディで作成するためのキーワードです。</span><span class="sxs-lookup"><span data-stu-id="0031c-295">`sealed` is the keyword to make interface instance members with bodies non-virtual</span></span>
3. <span data-ttu-id="0031c-296">インターフェイス内のすべての修飾子を許可します。</span><span class="sxs-lookup"><span data-stu-id="0031c-296">We want to allow all modifiers in interfaces</span></span>  
4. <span data-ttu-id="0031c-297">インターフェイスメンバーの既定のアクセシビリティは、入れ子になった型を含むパブリックです。</span><span class="sxs-lookup"><span data-stu-id="0031c-297">Default accessibility for interface members is public, including nested types</span></span>
5. <span data-ttu-id="0031c-298">インターフェイスのプライベート関数メンバーは暗黙的にシールされているため、`sealed` は使用できません。</span><span class="sxs-lookup"><span data-stu-id="0031c-298">private function members in interfaces are implicitly sealed, and `sealed` is not permitted on them.</span></span>
6. <span data-ttu-id="0031c-299">プライベートクラス (インターフェイス内) は許可されており、シールすることができます。これは、シールされたクラスの意味でシールされていることを意味します。</span><span class="sxs-lookup"><span data-stu-id="0031c-299">Private classes (in interfaces) are permitted and can be sealed, and that means sealed in the class sense of sealed.</span></span>
7. <span data-ttu-id="0031c-300">適切な提案がない場合でも、インターフェイスまたはメンバーでは部分的な使用は許可されません。</span><span class="sxs-lookup"><span data-stu-id="0031c-300">Absent a good proposal, partial is still not allowed on interfaces or their members.</span></span>

### <a name="binary-compatibility-1"></a><span data-ttu-id="0031c-301">バイナリ互換性1</span><span class="sxs-lookup"><span data-stu-id="0031c-301">Binary Compatibility 1</span></span>

<span data-ttu-id="0031c-302">ライブラリが既定の実装を提供する場合</span><span class="sxs-lookup"><span data-stu-id="0031c-302">When a library provides a default implementation</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

<span data-ttu-id="0031c-303">`C` での `I1.M` の実装は `I1.M`ことを理解しています。</span><span class="sxs-lookup"><span data-stu-id="0031c-303">We understand that the implementation of `I1.M` in `C` is `I1.M`.</span></span> <span data-ttu-id="0031c-304">`I2` を含むアセンブリが次のように変更され、再コンパイルされた場合</span><span class="sxs-lookup"><span data-stu-id="0031c-304">What if the assembly containing `I2` is changed as follows and recompiled</span></span>

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

<span data-ttu-id="0031c-305">ただし `C` は再コンパイルされません。</span><span class="sxs-lookup"><span data-stu-id="0031c-305">but `C` is not recompiled.</span></span> <span data-ttu-id="0031c-306">プログラムを実行するとどうなりますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-306">What happens when the program is run?</span></span> <span data-ttu-id="0031c-307">`(C as I1).M()` の呼び出し</span><span class="sxs-lookup"><span data-stu-id="0031c-307">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="0031c-308">実行 `I1.M`</span><span class="sxs-lookup"><span data-stu-id="0031c-308">Runs `I1.M`</span></span>
2. <span data-ttu-id="0031c-309">実行 `I2.M`</span><span class="sxs-lookup"><span data-stu-id="0031c-309">Runs `I2.M`</span></span>
3. <span data-ttu-id="0031c-310">何らかの種類のランタイムエラーをスローします。</span><span class="sxs-lookup"><span data-stu-id="0031c-310">Throws some kind of runtime error</span></span>

<span data-ttu-id="0031c-311">***決定:*** 2017-04-11: 実行時に明確に最も限定的なオーバーライドである `I2.M`を実行します。</span><span class="sxs-lookup"><span data-stu-id="0031c-311">***Decision:*** Made 2017-04-11: Runs `I2.M`, which is the unambiguously most specific override at runtime.</span></span>

### <a name="event-accessors-closed"></a><span data-ttu-id="0031c-312">イベントアクセサー (終了)</span><span class="sxs-lookup"><span data-stu-id="0031c-312">Event accessors (closed)</span></span>

> <span data-ttu-id="0031c-313">***終了済みの問題:***"区分" というイベントを上書きできますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-313">***Closed Issue:*** Can an event be overridden "piecewise"?</span></span>

<span data-ttu-id="0031c-314">次の場合を考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0031c-314">Consider this case:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

<span data-ttu-id="0031c-315">このイベントの "部分的な" 実装は許可されていません。これは、クラスのように、イベント宣言の構文ではアクセサーが1つしか許可されていないためです。両方 (またはどちらも) を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-315">This "partial" implementation of the event is not permitted because, as in a class, the syntax for an event declaration does not permit only one accessor; both (or neither) must be provided.</span></span> <span data-ttu-id="0031c-316">次のように、構文の abstract remove アクセサーを、本文がないことによって暗黙的に抽象化することを許可することで、同じことを実現できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-316">You could accomplish the same thing by permitting the abstract remove accessor in the syntax to be implicitly abstract by the absence of a body:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

<span data-ttu-id="0031c-317">*これは新しい (提案された) 構文*であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0031c-317">Note that *this is a new (proposed) syntax*.</span></span> <span data-ttu-id="0031c-318">現在の文法では、イベントアクセサーには必須の本文があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-318">In the current grammar, event accessors have a mandatory body.</span></span>

> <span data-ttu-id="0031c-319">***終了済みの問題:*** イベントアクセサーは、本体の省略によって (暗黙的に) 抽象化できます。これは、インターフェイスとプロパティアクセサーのメソッドが (暗黙的に) 本体の省略によって抽象化される方法と同じです。</span><span class="sxs-lookup"><span data-stu-id="0031c-319">***Closed Issue:*** Can an event accessor be (implicitly) abstract by the omission of a body, similarly to the way that methods in interfaces and property accessors are (implicitly) abstract by the omission of a body?</span></span>

<span data-ttu-id="0031c-320">***決定:*** (2017-04-18) いいえ。イベント宣言には、具象アクセサー (またはその両方) が必要です。</span><span class="sxs-lookup"><span data-stu-id="0031c-320">***Decision:*** (2017-04-18) No, event declarations require both concrete accessors (or neither).</span></span>

### <a name="reabstraction-in-a-class-closed"></a><span data-ttu-id="0031c-321">クラス (closed) での再抽象化</span><span class="sxs-lookup"><span data-stu-id="0031c-321">Reabstraction in a Class (closed)</span></span>

<span data-ttu-id="0031c-322">***終了済みの問題:*** これが許可されていることを確認する必要があります (それ以外の場合、既定の実装を追加すると、互換性に影響する変更になります)。</span><span class="sxs-lookup"><span data-stu-id="0031c-322">***Closed Issue:*** We should confirm that this is permitted (otherwise adding a default implementation would be a breaking change):</span></span>

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

<span data-ttu-id="0031c-323">***決定:*** (2017-04-18) はい。インターフェイスメンバーの宣言に本体を追加して、C を中断することはできません。</span><span class="sxs-lookup"><span data-stu-id="0031c-323">***Decision:*** (2017-04-18) Yes, adding a body to an interface member declaration shouldn't break C.</span></span>

### <a name="sealed-override-closed"></a><span data-ttu-id="0031c-324">シールされたオーバーライド (終了)</span><span class="sxs-lookup"><span data-stu-id="0031c-324">Sealed Override (closed)</span></span>

<span data-ttu-id="0031c-325">前の質問では、`sealed` 修飾子をインターフェイスの `override` に適用できることを暗黙的に想定しています。</span><span class="sxs-lookup"><span data-stu-id="0031c-325">The previous question implicitly assumes that the `sealed` modifier can be applied to an `override` in an interface.</span></span> <span data-ttu-id="0031c-326">これはドラフト仕様と矛盾しています。</span><span class="sxs-lookup"><span data-stu-id="0031c-326">This contradicts the draft specification.</span></span> <span data-ttu-id="0031c-327">上書きの封印を許可しますか?</span><span class="sxs-lookup"><span data-stu-id="0031c-327">Do we want to permit sealing an override?</span></span> <span data-ttu-id="0031c-328">封印のソースとバイナリの互換性の影響を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-328">Source and binary compatibility effects of sealing should be considered.</span></span>

> <span data-ttu-id="0031c-329">***終了済みの問題:*** 上書きの封印を許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-329">***Closed Issue:*** Should we permit sealing an override?</span></span>

<span data-ttu-id="0031c-330">***決定:*** (2017-04-18) インターフェイスのオーバーライドで `sealed` できないようにします。</span><span class="sxs-lookup"><span data-stu-id="0031c-330">***Decision:*** (2017-04-18) Let's not allowed `sealed` on overrides in interfaces.</span></span> <span data-ttu-id="0031c-331">インターフェイスメンバーに対して `sealed` を使用する唯一の方法は、初期宣言で非仮想にすることです。</span><span class="sxs-lookup"><span data-stu-id="0031c-331">The only use of `sealed` on interface members is to make them non-virtual in their initial declaration.</span></span>

### <a name="diamond-inheritance-and-classes-closed"></a><span data-ttu-id="0031c-332">ダイヤモンド継承とクラス (閉じている)</span><span class="sxs-lookup"><span data-stu-id="0031c-332">Diamond inheritance and classes (closed)</span></span>

<span data-ttu-id="0031c-333">提案のドラフトでは、ひし形の継承シナリオで、クラスがインターフェイスのオーバーライドに優先されます。</span><span class="sxs-lookup"><span data-stu-id="0031c-333">The draft of the proposal prefers class overrides to interface overrides in diamond inheritance scenarios:</span></span>

> <span data-ttu-id="0031c-334">すべてのインターフェイスとクラスは、型またはその直接および間接インターフェイスに表示されるオーバーライドのすべてのインターフェイスメソッドに対して、*最も限定的なオーバーライド*を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-334">We require that every interface and class have a *most specific override* for every interface method among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="0031c-335">*最も具体的*なオーバーライドは、他のすべての上書きよりも固有の一意のオーバーライドです。</span><span class="sxs-lookup"><span data-stu-id="0031c-335">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="0031c-336">オーバーライドがない場合、メソッド自体は最も限定的なオーバーライドと見なされます。</span><span class="sxs-lookup"><span data-stu-id="0031c-336">If there is no override, the method itself is considered the most specific override.</span></span>
>
> <span data-ttu-id="0031c-337">`M1` が型 `T1`で宣言されていて、`M2` が型 `T2`で宣言されている場合は、1つのオーバーライド `M1` が別のオーバーライドよりも*具体的*に `M2` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="0031c-337">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>
>
> 1. <span data-ttu-id="0031c-338">`T1` には、直接または間接のインターフェイスの `T2` が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0031c-338">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
> 2. <span data-ttu-id="0031c-339">`T2` はインターフェイス型ですが、`T1` はインターフェイス型ではありません。</span><span class="sxs-lookup"><span data-stu-id="0031c-339">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="0031c-340">このシナリオは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="0031c-340">The scenario is this</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

<span data-ttu-id="0031c-341">この動作を確認する必要があります (または他の方法を決定します)。</span><span class="sxs-lookup"><span data-stu-id="0031c-341">We should confirm this behavior (or decide otherwise)</span></span>

> <span data-ttu-id="0031c-342">***終了済みの問題:*** 上記のドラフト仕様を、混合クラスおよびインターフェイス (クラスはインターフェイスよりも優先されます) に適用される*ほとんどの特定のオーバーライド*について確認します。</span><span class="sxs-lookup"><span data-stu-id="0031c-342">***Closed Issue:*** Confirm the draft spec, above, for *most specific override* as it applies to mixed classes and interfaces (a class takes priority over an interface).</span></span> <span data-ttu-id="0031c-343">[https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0031c-343">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span></span>

### <a name="interface-methods-vs-structs-closed"></a><span data-ttu-id="0031c-344">インターフェイスメソッドと構造体 (closed)</span><span class="sxs-lookup"><span data-stu-id="0031c-344">Interface methods vs structs (closed)</span></span>

<span data-ttu-id="0031c-345">既定のインターフェイスメソッドと構造体の間には、よくある相互作用があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-345">There are some unfortunate interactions between default interface methods and structs.</span></span>

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

<span data-ttu-id="0031c-346">インターフェイスメンバーは継承されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0031c-346">Note that interface members are not inherited:</span></span>

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

<span data-ttu-id="0031c-347">そのため、クライアントは、インターフェイスメソッドを呼び出すように構造体をボックスにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-347">Consequently, the client must box the struct to invoke interface methods</span></span>

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

<span data-ttu-id="0031c-348">このようにボックス化すると、`struct` の種類の主な利点が損なわれます。</span><span class="sxs-lookup"><span data-stu-id="0031c-348">Boxing in this way defeats the principal benefits of a `struct` type.</span></span> <span data-ttu-id="0031c-349">さらに、変異メソッドは、構造体のボックス化された*コピー*で動作しているため、明らかに効果がありません。</span><span class="sxs-lookup"><span data-stu-id="0031c-349">Moreover, any mutation methods will have no apparent effect, because they are operating on a *boxed copy* of the struct:</span></span>

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> <span data-ttu-id="0031c-350">***終了済みの問題:*** これについては、次のことを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="0031c-350">***Closed Issue:*** What can we do about this:</span></span>
>
> 1. <span data-ttu-id="0031c-351">`struct` が既定の実装を継承することを禁止します。</span><span class="sxs-lookup"><span data-stu-id="0031c-351">Forbid a `struct` from inheriting a default implementation.</span></span> <span data-ttu-id="0031c-352">すべてのインターフェイスメソッドは、`struct`で abstract として扱われます。</span><span class="sxs-lookup"><span data-stu-id="0031c-352">All interface methods would be treated as abstract in a `struct`.</span></span> <span data-ttu-id="0031c-353">その後、後でより適切に動作させる方法を決定するために時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="0031c-353">Then we may take time later to decide how to make it work better.</span></span>
> 2. <span data-ttu-id="0031c-354">ボックス化を回避する何らかの種類のコード生成方法を考えてください。</span><span class="sxs-lookup"><span data-stu-id="0031c-354">Come up with some kind of code generation strategy that avoids boxing.</span></span> <span data-ttu-id="0031c-355">`IB.Increment`のようなメソッドの内部では、`this` の型は `IB`に制約された型パラメーターに類似している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-355">Inside a method like `IB.Increment`, the type of `this` would perhaps be akin to a type parameter constrained to `IB`.</span></span> <span data-ttu-id="0031c-356">これと共に、呼び出し元でのボックス化を避けるために、非抽象メソッドはインターフェイスから継承されます。</span><span class="sxs-lookup"><span data-stu-id="0031c-356">In conjunction with that, to avoid boxing in the caller, non-abstract methods would be inherited from interfaces.</span></span> <span data-ttu-id="0031c-357">これにより、コンパイラと CLR の実装が大幅に増加する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-357">This may increase compiler and CLR implementation work substantially.</span></span>
> 3. <span data-ttu-id="0031c-358">心配せず、wart として残してください。</span><span class="sxs-lookup"><span data-stu-id="0031c-358">Not worry about it and just leave it as a wart.</span></span>
> 4. <span data-ttu-id="0031c-359">その他のアイデア</span><span class="sxs-lookup"><span data-stu-id="0031c-359">Other ideas?</span></span>

<span data-ttu-id="0031c-360">***決定:*** 心配せず、wart として残してください。</span><span class="sxs-lookup"><span data-stu-id="0031c-360">***Decision:*** Not worry about it and just leave it as a wart.</span></span> <span data-ttu-id="0031c-361">[https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0031c-361">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span></span>

### <a name="base-interface-invocations-closed"></a><span data-ttu-id="0031c-362">基本インターフェイス呼び出し (closed)</span><span class="sxs-lookup"><span data-stu-id="0031c-362">Base interface invocations (closed)</span></span>

<span data-ttu-id="0031c-363">ドラフト仕様は、Java: `Interface.base.M()`によって使用される基本インターフェイスの呼び出しの構文を提案します。</span><span class="sxs-lookup"><span data-stu-id="0031c-363">The draft spec suggests a syntax for base interface invocations inspired by Java: `Interface.base.M()`.</span></span> <span data-ttu-id="0031c-364">少なくとも最初のプロトタイプについて、構文を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-364">We need to select a syntax, at least for the initial prototype.</span></span> <span data-ttu-id="0031c-365">お気に入りは `base<Interface>.M()`。</span><span class="sxs-lookup"><span data-stu-id="0031c-365">My favorite is `base<Interface>.M()`.</span></span>

> <span data-ttu-id="0031c-366">***終了済みの問題:*** 基本メンバー呼び出しの構文は何ですか。</span><span class="sxs-lookup"><span data-stu-id="0031c-366">***Closed Issue:*** What is the syntax for a base member invocation?</span></span>

<span data-ttu-id="0031c-367">***決定:*** 構文は `base(Interface).M()`です。</span><span class="sxs-lookup"><span data-stu-id="0031c-367">***Decision:*** The syntax is `base(Interface).M()`.</span></span> <span data-ttu-id="0031c-368">[https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0031c-368">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span></span> <span data-ttu-id="0031c-369">このため、という名前のインターフェイスは基本インターフェイスである必要がありますが、直接の基本インターフェイスである必要はありません。</span><span class="sxs-lookup"><span data-stu-id="0031c-369">The interface so named must be a base interface, but does not need to be a direct base interface.</span></span>

> <span data-ttu-id="0031c-370">***懸案事項を開く:*** 基底インターフェイスの呼び出しをクラスメンバーで許可する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-370">***Open Issue:*** Should base interface invocations be permitted in class members?</span></span>

<span data-ttu-id="0031c-371">***決定***: はい。</span><span class="sxs-lookup"><span data-stu-id="0031c-371">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a><span data-ttu-id="0031c-372">パブリックでないインターフェイスメンバーのオーバーライド (closed)</span><span class="sxs-lookup"><span data-stu-id="0031c-372">Overriding non-public interface members (closed)</span></span>

<span data-ttu-id="0031c-373">インターフェイスでは、基本インターフェイスの非パブリックメンバーは `override` 修飾子を使用してオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="0031c-373">In an interface, non-public members from base interfaces are overridden using the `override` modifier.</span></span> <span data-ttu-id="0031c-374">メンバーを含むインターフェイスに名前を明示的にオーバーライドする場合、アクセス修飾子は省略されます。</span><span class="sxs-lookup"><span data-stu-id="0031c-374">If it is an "explicit" override that names the interface containing the member, the access modifier is omitted.</span></span>

> <span data-ttu-id="0031c-375">***終了済みの問題:*** インターフェイスに名前が付けられていない "暗黙" のオーバーライドである場合、アクセス修飾子は一致している必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-375">***Closed Issue:*** If it is an "implicit" override that does not name the interface, does the access modifier have to match?</span></span>

<span data-ttu-id="0031c-376">***決定:*** 暗黙的にオーバーライドできるのはパブリックメンバーだけで、アクセスは一致している必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-376">***Decision:*** Only public members may be implicitly overridden, and the access must match.</span></span> <span data-ttu-id="0031c-377">[https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0031c-377">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

> <span data-ttu-id="0031c-378">***懸案事項を開く:***`override void IB.M() {}`などの明示的なオーバーライドでは、アクセス修飾子が必須、省略可能、または省略されているかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="0031c-378">***Open Issue:*** Is the access modifier required, optional, or omitted on an explicit override such as `override void IB.M() {}`?</span></span>

> <span data-ttu-id="0031c-379">***懸案事項を開く:***`void IB.M() {}`などの明示的なオーバーライドでは `override` 必須、省略可能、または省略されていますか?</span><span class="sxs-lookup"><span data-stu-id="0031c-379">***Open Issue:*** Is `override` required, optional, or omitted on an explicit override such as `void IB.M() {}`?</span></span>

<span data-ttu-id="0031c-380">1つは、クラスにパブリックでないインターフェイスメンバーを実装する方法ですか。</span><span class="sxs-lookup"><span data-stu-id="0031c-380">How does one implement a non-public interface member in a class?</span></span> <span data-ttu-id="0031c-381">明示的に実行する必要があるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="0031c-381">Perhaps it must be done explicitly?</span></span>

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> <span data-ttu-id="0031c-382">***終了済みの問題:*** 1つは、クラスにパブリックでないインターフェイスメンバーを実装する方法ですか。</span><span class="sxs-lookup"><span data-stu-id="0031c-382">***Closed Issue:*** How does one implement a non-public interface member in a class?</span></span>

<span data-ttu-id="0031c-383">***決定:*** 非パブリックインターフェイスメンバーを明示的に実装することはできません。</span><span class="sxs-lookup"><span data-stu-id="0031c-383">***Decision:*** You can only implement non-public interface members explicitly.</span></span> <span data-ttu-id="0031c-384">[https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0031c-384">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

<span data-ttu-id="0031c-385">***Decision***: インターフェイスメンバーに `override` キーワードは使用できません。</span><span class="sxs-lookup"><span data-stu-id="0031c-385">***Decision***: No `override` keyword permitted on interface members.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a><span data-ttu-id="0031c-386">バイナリ互換性 2 (終了)</span><span class="sxs-lookup"><span data-stu-id="0031c-386">Binary Compatibility 2 (closed)</span></span>

<span data-ttu-id="0031c-387">それぞれの型が別のアセンブリにある次のコードについて考えてみます。</span><span class="sxs-lookup"><span data-stu-id="0031c-387">Consider the following code in which each type is in a separate assembly</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

<span data-ttu-id="0031c-388">`C` での `I1.M` の実装は `I2.M`ことを理解しています。</span><span class="sxs-lookup"><span data-stu-id="0031c-388">We understand that the implementation of `I1.M` in `C` is `I2.M`.</span></span> <span data-ttu-id="0031c-389">`I3` を含むアセンブリが次のように変更され、再コンパイルされた場合</span><span class="sxs-lookup"><span data-stu-id="0031c-389">What if the assembly containing `I3` is changed as follows and recompiled</span></span>

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

<span data-ttu-id="0031c-390">ただし `C` は再コンパイルされません。</span><span class="sxs-lookup"><span data-stu-id="0031c-390">but `C` is not recompiled.</span></span> <span data-ttu-id="0031c-391">プログラムを実行するとどうなりますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-391">What happens when the program is run?</span></span> <span data-ttu-id="0031c-392">`(C as I1).M()` の呼び出し</span><span class="sxs-lookup"><span data-stu-id="0031c-392">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="0031c-393">実行 `I1.M`</span><span class="sxs-lookup"><span data-stu-id="0031c-393">Runs `I1.M`</span></span>
2. <span data-ttu-id="0031c-394">実行 `I2.M`</span><span class="sxs-lookup"><span data-stu-id="0031c-394">Runs `I2.M`</span></span>
3. <span data-ttu-id="0031c-395">実行 `I3.M`</span><span class="sxs-lookup"><span data-stu-id="0031c-395">Runs `I3.M`</span></span>
4. <span data-ttu-id="0031c-396">2または 3 (決定的)</span><span class="sxs-lookup"><span data-stu-id="0031c-396">Either 2 or 3, deterministically</span></span>
5. <span data-ttu-id="0031c-397">何らかの種類のランタイム例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="0031c-397">Throws some kind of runtime exception</span></span>

<span data-ttu-id="0031c-398">***Decision***: 例外をスローします (5)。</span><span class="sxs-lookup"><span data-stu-id="0031c-398">***Decision***: Throw an exception (5).</span></span> <span data-ttu-id="0031c-399">[https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0031c-399">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span></span>

### <a name="permit-partial-in-interface-closed"></a><span data-ttu-id="0031c-400">インターフェイスで `partial` を許可しますか?</span><span class="sxs-lookup"><span data-stu-id="0031c-400">Permit `partial` in interface?</span></span> <span data-ttu-id="0031c-401">開閉</span><span class="sxs-lookup"><span data-stu-id="0031c-401">(closed)</span></span>

<span data-ttu-id="0031c-402">抽象クラスの使用方法に似た方法でインターフェイスが使用されている場合は、それらを `partial`宣言すると便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-402">Given that interfaces may be used in ways analogous to the way abstract classes are used, it may be useful to declare them `partial`.</span></span> <span data-ttu-id="0031c-403">これは、ジェネレーターの面で特に便利です。</span><span class="sxs-lookup"><span data-stu-id="0031c-403">This would be particularly useful in the face of generators.</span></span>

> <span data-ttu-id="0031c-404">***提案:*** インターフェイスおよびインターフェイスのメンバーが `partial`として宣言されていない可能性のある言語制限を削除します。</span><span class="sxs-lookup"><span data-stu-id="0031c-404">***Proposal:*** Remove the language restriction that interfaces and members of interfaces may not be declared `partial`.</span></span>

<span data-ttu-id="0031c-405">***決定***: はい。</span><span class="sxs-lookup"><span data-stu-id="0031c-405">***Decision***: Yes.</span></span> <span data-ttu-id="0031c-406">[https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0031c-406">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span></span>

### <a name="main-in-an-interface-closed"></a><span data-ttu-id="0031c-407">インターフェイスで `Main` しますか?</span><span class="sxs-lookup"><span data-stu-id="0031c-407">`Main` in an interface?</span></span> <span data-ttu-id="0031c-408">開閉</span><span class="sxs-lookup"><span data-stu-id="0031c-408">(closed)</span></span>

> <span data-ttu-id="0031c-409">***懸案事項を開く:*** インターフェイス内の `static Main` メソッドは、プログラムのエントリポイントになる候補ですか。</span><span class="sxs-lookup"><span data-stu-id="0031c-409">***Open Issue:*** Is a `static Main` method in an interface a candidate to be the program's entry point?</span></span>

<span data-ttu-id="0031c-410">***決定***: はい。</span><span class="sxs-lookup"><span data-stu-id="0031c-410">***Decision***: Yes.</span></span> <span data-ttu-id="0031c-411">[https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0031c-411">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span></span>

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a><span data-ttu-id="0031c-412">パブリックな非仮想メソッドをサポートすることを確認する (終了)</span><span class="sxs-lookup"><span data-stu-id="0031c-412">Confirm intent to support public non-virtual methods (closed)</span></span>

<span data-ttu-id="0031c-413">インターフェイスで非仮想パブリックメソッドを許可するかどうかを確認 (または反転) できますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-413">Can we please confirm (or reverse) our decision to permit non-virtual public methods in an interface?</span></span>

```csharp
interface IA
{
    public sealed void M() { }
}
```

> <span data-ttu-id="0031c-414">***半終了した問題:*** (2017-04-18) は役に立つと思われますが、それに戻ります。</span><span class="sxs-lookup"><span data-stu-id="0031c-414">***Semi-Closed Issue:*** (2017-04-18) We think it is going to be useful, but will come back to it.</span></span> <span data-ttu-id="0031c-415">これは、メンタルモデルのトリップブロックです。</span><span class="sxs-lookup"><span data-stu-id="0031c-415">This is a mental model tripping block.</span></span>

<span data-ttu-id="0031c-416">***決定***: はい。</span><span class="sxs-lookup"><span data-stu-id="0031c-416">***Decision***: Yes.</span></span> <span data-ttu-id="0031c-417">[https://login.microsoftonline.com/consumers/](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>)</span><span class="sxs-lookup"><span data-stu-id="0031c-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span></span>

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a><span data-ttu-id="0031c-418">インターフェイス内の `override` は新しいメンバーを導入しますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-418">Does an `override` in an interface introduce a new member?</span></span> <span data-ttu-id="0031c-419">開閉</span><span class="sxs-lookup"><span data-stu-id="0031c-419">(closed)</span></span>

<span data-ttu-id="0031c-420">オーバーライド宣言に新しいメンバーが導入されているかどうかを確認するには、いくつかの方法があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-420">There are a few ways to observe whether an override declaration introduces a new member or not.</span></span>

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> <span data-ttu-id="0031c-421">***懸案事項を開く:*** インターフェイス内のオーバーライド宣言によって新しいメンバーが導入されるか。</span><span class="sxs-lookup"><span data-stu-id="0031c-421">***Open Issue:*** Does an override declaration in an interface introduce a new member?</span></span> <span data-ttu-id="0031c-422">開閉</span><span class="sxs-lookup"><span data-stu-id="0031c-422">(closed)</span></span>

<span data-ttu-id="0031c-423">クラスでは、オーバーライドするメソッドはいくつかの意味で "visible" になります。</span><span class="sxs-lookup"><span data-stu-id="0031c-423">In a class, an overriding method is "visible" in some senses.</span></span> <span data-ttu-id="0031c-424">たとえば、パラメーターの名前は、オーバーライドされたメソッドのパラメーターの名前よりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="0031c-424">For example, the names of its parameters take precedence over the names of parameters in the overridden method.</span></span> <span data-ttu-id="0031c-425">常に最も限定的なオーバーライドがあるため、インターフェイスでその動作を複製できる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-425">It may be possible to duplicate that behavior in interfaces, as there is always a most specific override.</span></span> <span data-ttu-id="0031c-426">しかし、その動作を複製する必要はありますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-426">But do we want to duplicate that behavior?</span></span>

<span data-ttu-id="0031c-427">また、オーバーライドメソッドを "オーバーライド" できますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-427">Also, it is possible to "override" an override method?</span></span> <span data-ttu-id="0031c-428">議論</span><span class="sxs-lookup"><span data-stu-id="0031c-428">[Moot]</span></span>

<span data-ttu-id="0031c-429">***Decision***: インターフェイスメンバーに `override` キーワードは使用できません。</span><span class="sxs-lookup"><span data-stu-id="0031c-429">***Decision***: No `override` keyword permitted on interface members.</span></span> <span data-ttu-id="0031c-430">[https://login.microsoftonline.com/consumers/](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>)</span><span class="sxs-lookup"><span data-stu-id="0031c-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span></span>

### <a name="properties-with-a-private-accessor-closed"></a><span data-ttu-id="0031c-431">プライベートアクセサーを持つプロパティ (closed)</span><span class="sxs-lookup"><span data-stu-id="0031c-431">Properties with a private accessor (closed)</span></span>

<span data-ttu-id="0031c-432">プライベートメンバーは仮想ではなく、仮想とプライベートの組み合わせは許可されていません。</span><span class="sxs-lookup"><span data-stu-id="0031c-432">We say that private members are not virtual, and the combination of virtual and private is disallowed.</span></span> <span data-ttu-id="0031c-433">しかし、プライベートアクセサーを持つプロパティについてはどうでしょうか。</span><span class="sxs-lookup"><span data-stu-id="0031c-433">But what about a property with a private accessor?</span></span>

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

<span data-ttu-id="0031c-434">これは可能ですか?</span><span class="sxs-lookup"><span data-stu-id="0031c-434">Is this allowed?</span></span> <span data-ttu-id="0031c-435">ここでは `set` アクセサー `virtual` ですか?</span><span class="sxs-lookup"><span data-stu-id="0031c-435">Is the `set` accessor here `virtual` or not?</span></span> <span data-ttu-id="0031c-436">アクセス可能な場所で上書きできますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-436">Can it be overridden where it is accessible?</span></span> <span data-ttu-id="0031c-437">次のは、暗黙的に `get` アクセサーのみを実装しますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-437">Does the following implicitly implement only the `get` accessor?</span></span>

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="0031c-438">IA が原因で、次のようなエラーが発生することがあります。P. set は仮想ではなく、アクセスできないためにも設定されていますか。</span><span class="sxs-lookup"><span data-stu-id="0031c-438">Is the following presumably an error because IA.P.set isn't virtual and also because it isn't accessible?</span></span>

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="0031c-439">***Decision***: 最初の例は有効ですが、最後の例は有効ではありません。</span><span class="sxs-lookup"><span data-stu-id="0031c-439">***Decision***: The first example looks valid, while the last does not.</span></span> <span data-ttu-id="0031c-440">これは、でC#既に動作していると同様に解決されます。</span><span class="sxs-lookup"><span data-stu-id="0031c-440">This is resolved analogously to how it already works in C#.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a><span data-ttu-id="0031c-441">基本インターフェイス呼び出し、round 2 (closed)</span><span class="sxs-lookup"><span data-stu-id="0031c-441">Base Interface Invocations, round 2 (closed)</span></span>

<span data-ttu-id="0031c-442">基本呼び出しを処理する方法に対する以前の "解決策" は、実際には十分な表現力を提供しません。</span><span class="sxs-lookup"><span data-stu-id="0031c-442">Our previous "resolution" to how to handle base invocations doesn't actually provide sufficient expressiveness.</span></span> <span data-ttu-id="0031c-443">Java とは異なり、 C#と CLR では、メソッド宣言を含むインターフェイスと呼び出す実装の場所の両方を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-443">It turns out that in C# and the CLR, unlike Java, you need to specify both the interface containing the method declaration and the location of the implementation you want to invoke.</span></span>

<span data-ttu-id="0031c-444">ここでは、インターフェイスの基本呼び出しに対して次の構文を提案します。</span><span class="sxs-lookup"><span data-stu-id="0031c-444">I propose the following syntax for base calls in interfaces.</span></span> <span data-ttu-id="0031c-445">私は気に入っていませんが、どのような構文を表現できる必要があるかを示しています。</span><span class="sxs-lookup"><span data-stu-id="0031c-445">I’m not in love with it, but it illustrates what any syntax must be able to express:</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

<span data-ttu-id="0031c-446">あいまいさがない場合は、簡単に記述できます。</span><span class="sxs-lookup"><span data-stu-id="0031c-446">If there is no ambiguity, you can write it more simply</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

<span data-ttu-id="0031c-447">または</span><span class="sxs-lookup"><span data-stu-id="0031c-447">Or</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

<span data-ttu-id="0031c-448">または</span><span class="sxs-lookup"><span data-stu-id="0031c-448">Or</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

<span data-ttu-id="0031c-449">***Decision***: `base(N.I1<T>).M(s)`に対して決定されます。呼び出しバインドがある場合は、後で問題が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-449">***Decision***: Decided on `base(N.I1<T>).M(s)`, conceding that if we have an invocation binding there may be problem here later on.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a><span data-ttu-id="0031c-450">構造体が既定のメソッドを実装していない場合の警告</span><span class="sxs-lookup"><span data-stu-id="0031c-450">Warning for struct not implementing default method?</span></span> <span data-ttu-id="0031c-451">開閉</span><span class="sxs-lookup"><span data-stu-id="0031c-451">(closed)</span></span>

<span data-ttu-id="0031c-452">@vancem アサートするには、インターフェイスからメソッドの実装を継承する場合でも、値型の宣言がインターフェイスメソッドのオーバーライドに失敗した場合に警告を生成することを真剣に検討する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-452">@vancem asserts that we should seriously consider producing a warning if a value type declaration fails to override some interface method, even if it would inherit an implementation of that method from an interface.</span></span> <span data-ttu-id="0031c-453">これは、ボックス化と損なうの制約付き呼び出しを引き起こすためです。</span><span class="sxs-lookup"><span data-stu-id="0031c-453">Because it causes boxing and undermines constrained calls.</span></span>

<span data-ttu-id="0031c-454">***決定***: これは、アナライザーにより適しているように見えます。</span><span class="sxs-lookup"><span data-stu-id="0031c-454">***Decision***: This seems like something more suited for an analyzer.</span></span> <span data-ttu-id="0031c-455">また、この警告は、既定のインターフェイスメソッドが呼び出されず、ボックス化が行われない場合でも起動されるため、この警告が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0031c-455">It also seems like this warning could be noisy, since it would fire even if the default interface method is never called and no boxing will ever occur.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a><span data-ttu-id="0031c-456">インターフェイス静的コンストラクター (closed)</span><span class="sxs-lookup"><span data-stu-id="0031c-456">Interface static constructors (closed)</span></span>

<span data-ttu-id="0031c-457">インターフェイスの静的コンストラクターを実行する場合</span><span class="sxs-lookup"><span data-stu-id="0031c-457">When are interface static constructors run?</span></span>  <span data-ttu-id="0031c-458">現在の CLI ドラフトでは、最初の静的メソッドまたはフィールドにアクセスしたときに発生することが提案されています。</span><span class="sxs-lookup"><span data-stu-id="0031c-458">The current CLI draft proposes that it occurs when the first static method or field is accessed.</span></span> <span data-ttu-id="0031c-459">これらのいずれも存在しない場合、実行されない可能性がありますか?</span><span class="sxs-lookup"><span data-stu-id="0031c-459">If there are neither of those then it might never be run??</span></span>

<span data-ttu-id="0031c-460">[2018-10-09 CLR チームは、valuetypes に対して行うことをミラー化する (各インスタンスメソッドへのアクセスに対しては、.cctor チェック) を提案します。]</span><span class="sxs-lookup"><span data-stu-id="0031c-460">[2018-10-09 The CLR team proposes "Going to mirror what we do for valuetypes (cctor check on access to each instance method)"]</span></span>

<span data-ttu-id="0031c-461">***Decision***: 静的コンストラクターは、インスタンスメソッドへのエントリでも実行されます。この場合、静的コンストラクターが `beforefieldinit`されていないと、最初の静的フィールドにアクセスする前に静的コンストラクターが実行されます。</span><span class="sxs-lookup"><span data-stu-id="0031c-461">***Decision***: Static constructors are also run on entry to instance methods, if the static constructor was not `beforefieldinit`, in which case static constructors are run before access to the first static field.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a><span data-ttu-id="0031c-462">会議のデザイン</span><span class="sxs-lookup"><span data-stu-id="0031c-462">Design meetings</span></span>

<span data-ttu-id="0031c-463">[2017-03-08 Ldm 会議ノート](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 ldm のノート](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 Meeting "既定のインターフェイスメソッドに対する CLR の動作"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 Ldm 会議のメモ](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 ldm 会議](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)の[メモ
2017-04-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) LDM 会議のメモ
2017-04-19 [ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)会議のメモ
[2017-05-17 ldm 会議のメモ](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 ldm のメモ](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
2017-06-14 [ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-11-14 ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)会議メモの会議ノート
[2018-10-17 ldm 会議](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)ノート</span><span class="sxs-lookup"><span data-stu-id="0031c-463">[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)
[2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
[2017-06-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span></span>
