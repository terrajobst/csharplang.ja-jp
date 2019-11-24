---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704004"
---
# <a name="basic-concepts"></a><span data-ttu-id="8672b-101">基本的な概念</span><span class="sxs-lookup"><span data-stu-id="8672b-101">Basic concepts</span></span>

## <a name="application-startup"></a><span data-ttu-id="8672b-102">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="8672b-102">Application Startup</span></span>

<span data-ttu-id="8672b-103">***エントリポイント***を持つアセンブリは、***アプリケーション***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8672b-103">An assembly that has an ***entry point*** is called an ***application***.</span></span> <span data-ttu-id="8672b-104">アプリケーションを実行すると、新しい***アプリケーションドメイン***が作成されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-104">When an application is run, a new ***application domain*** is created.</span></span> <span data-ttu-id="8672b-105">アプリケーションの複数の異なるインスタンス化が同時に同じコンピューター上に存在し、それぞれに独自のアプリケーションドメインが存在する場合があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-105">Several different instantiations of an application may exist on the same machine at the same time, and each has its own application domain.</span></span>

<span data-ttu-id="8672b-106">アプリケーションドメインは、アプリケーションの状態のコンテナーとして機能することによって、アプリケーションの分離を可能にします。</span><span class="sxs-lookup"><span data-stu-id="8672b-106">An application domain enables application isolation by acting as a container for application state.</span></span> <span data-ttu-id="8672b-107">アプリケーションドメインは、アプリケーションで定義されている型とそれが使用するクラスライブラリのコンテナーおよび境界として機能します。</span><span class="sxs-lookup"><span data-stu-id="8672b-107">An application domain acts as a container and boundary for the types defined in the application and the class libraries it uses.</span></span> <span data-ttu-id="8672b-108">1つのアプリケーションドメインに読み込まれる型は、別のアプリケーションドメインに読み込まれる同じ型とは異なり、オブジェクトのインスタンスはアプリケーションドメイン間で直接共有されません。</span><span class="sxs-lookup"><span data-stu-id="8672b-108">Types loaded into one application domain are distinct from the same type loaded into another application domain, and instances of objects are not directly shared between application domains.</span></span> <span data-ttu-id="8672b-109">たとえば、各アプリケーションドメインには、これらの型の静的変数の独自のコピーがあり、型の静的コンストラクターはアプリケーションドメインごとに1回だけ実行されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-109">For instance, each application domain has its own copy of static variables for these types, and a static constructor for a type is run at most once per application domain.</span></span> <span data-ttu-id="8672b-110">実装には、アプリケーションドメインの作成と破棄を行うための実装固有のポリシーやメカニズムを無料で用意しています。</span><span class="sxs-lookup"><span data-stu-id="8672b-110">Implementations are free to provide implementation-specific policy or mechanisms for the creation and destruction of application domains.</span></span>

<span data-ttu-id="8672b-111">***アプリケーションの起動***は、実行環境が、アプリケーションのエントリポイントと呼ばれる指定されたメソッドを呼び出すと発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-111">***Application startup*** occurs when the execution environment calls a designated method, which is referred to as the application's entry point.</span></span> <span data-ttu-id="8672b-112">このエントリポイントメソッドは常に `Main`という名前で、次のいずれかのシグネチャを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-112">This entry point method is always named `Main`, and can have one of the following signatures:</span></span>

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

<span data-ttu-id="8672b-113">示されているように、エントリポイントは必要に応じて `int` 値を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-113">As shown, the entry point may optionally return an `int` value.</span></span> <span data-ttu-id="8672b-114">この戻り値は、アプリケーションの終了 ([アプリケーションの終了](basic-concepts.md#application-termination)) で使用されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-114">This return value is used in application termination ([Application termination](basic-concepts.md#application-termination)).</span></span>

<span data-ttu-id="8672b-115">エントリポイントには、必要に応じて、1つの仮パラメーターを指定できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-115">The entry point may optionally have one formal parameter.</span></span> <span data-ttu-id="8672b-116">パラメーターには任意の名前を指定できますが、パラメーターの型は `string[]`である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-116">The parameter may have any name, but the type of the parameter must be `string[]`.</span></span> <span data-ttu-id="8672b-117">仮パラメーターが存在する場合、実行環境は、アプリケーションの起動時に指定されたコマンドライン引数を含む `string[]` 引数を作成して渡します。</span><span class="sxs-lookup"><span data-stu-id="8672b-117">If the formal parameter is present, the execution environment creates and passes a `string[]` argument containing the command-line arguments that were specified when the application was started.</span></span> <span data-ttu-id="8672b-118">`string[]` 引数が null になることはありませんが、コマンドライン引数が指定されていない場合、長さが0になることがあります。</span><span class="sxs-lookup"><span data-stu-id="8672b-118">The `string[]` argument is never null, but it may have a length of zero if no command-line arguments were specified.</span></span>

<span data-ttu-id="8672b-119">でC#はメソッドのオーバーロードがサポートされているため、クラスまたは構造体には、それぞれ異なるシグネチャを持つメソッドの複数の定義を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-119">Since C# supports method overloading, a class or struct may contain multiple definitions of some method, provided each has a different signature.</span></span> <span data-ttu-id="8672b-120">ただし、1つのプログラム内では、クラスまたは構造体に、アプリケーションのエントリポイントとして使用されることを修飾する `Main` と呼ばれる複数のメソッドを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-120">However, within a single program, no class or struct may contain more than one method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="8672b-121">ただし、複数のパラメーターがある場合、またはその唯一のパラメーターが型 `string[]`以外の場合は、`Main` の他のオーバーロードされたバージョンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-121">Other overloaded versions of `Main` are permitted, however, provided they have more than one parameter, or their only parameter is other than type `string[]`.</span></span>

<span data-ttu-id="8672b-122">アプリケーションは、複数のクラスまたは構造体で構成できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-122">An application can be made up of multiple classes or structs.</span></span> <span data-ttu-id="8672b-123">これらのクラスまたは構造体の1つに、アプリケーションのエントリポイントとして使用することが定義されている `Main` という名前のメソッドを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-123">It is possible for more than one of these classes or structs to contain a method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="8672b-124">このような場合は、外部機構 (コマンドラインコンパイラオプションなど) を使用して、これらの `Main` メソッドのいずれかをエントリポイントとして選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-124">In such cases, an external mechanism (such as a command-line compiler option) must be used to select one of these `Main` methods as the entry point.</span></span>

<span data-ttu-id="8672b-125">でC#は、すべてのメソッドをクラスまたは構造体のメンバーとして定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-125">In C#, every method must be defined as a member of a class or struct.</span></span> <span data-ttu-id="8672b-126">通常、メソッドの宣言されたアクセシビリティ ([アクセシビリティ](basic-concepts.md#declared-accessibility)) は、宣言で指定されたアクセス修飾子 ([アクセス](classes.md#access-modifiers)修飾子) によって決まります。同様に、型の宣言されたアクセシビリティは、宣言で指定されたアクセス修飾子によって決まります。</span><span class="sxs-lookup"><span data-stu-id="8672b-126">Ordinarily, the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of a method is determined by the access modifiers ([Access modifiers](classes.md#access-modifiers)) specified in its declaration, and similarly the declared accessibility of a type is determined by the access modifiers specified in its declaration.</span></span> <span data-ttu-id="8672b-127">特定の型の特定のメソッドを呼び出し可能にするためには、型とメンバーの両方にアクセスできる必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-127">In order for a given method of a given type to be callable, both the type and the member must be accessible.</span></span> <span data-ttu-id="8672b-128">ただし、アプリケーションのエントリポイントは特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="8672b-128">However, the application entry point is a special case.</span></span> <span data-ttu-id="8672b-129">具体的には、実行環境は、宣言されたアクセシビリティに関係なく、それを囲む型宣言のアクセシビリティに関係なく、アプリケーションのエントリポイントにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="8672b-129">Specifically, the execution environment can access the application's entry point regardless of its declared accessibility and regardless of the declared accessibility of its enclosing type declarations.</span></span>

<span data-ttu-id="8672b-130">アプリケーションエントリポイントメソッドがジェネリッククラス宣言に含まれていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-130">The application entry point method may not be in a generic class declaration.</span></span>

<span data-ttu-id="8672b-131">それ以外の点では、エントリポイントメソッドはエントリポイントではないもののように動作します。</span><span class="sxs-lookup"><span data-stu-id="8672b-131">In all other respects, entry point methods behave like those that are not entry points.</span></span>

## <a name="application-termination"></a><span data-ttu-id="8672b-132">アプリケーションの終了</span><span class="sxs-lookup"><span data-stu-id="8672b-132">Application termination</span></span>

<span data-ttu-id="8672b-133">***アプリケーションの終了***では、実行環境に制御が戻ります。</span><span class="sxs-lookup"><span data-stu-id="8672b-133">***Application termination*** returns control to the execution environment.</span></span>

<span data-ttu-id="8672b-134">アプリケーションの***エントリポイント***メソッドの戻り値の型が `int`場合、返される値はアプリケーションの***終了ステータスコード***として機能します。</span><span class="sxs-lookup"><span data-stu-id="8672b-134">If the return type of the application's ***entry point*** method is `int`, the value returned serves as the application's ***termination status code***.</span></span> <span data-ttu-id="8672b-135">このコードの目的は、実行環境への成功または失敗の通信を許可することです。</span><span class="sxs-lookup"><span data-stu-id="8672b-135">The purpose of this code is to allow communication of success or failure to the execution environment.</span></span>

<span data-ttu-id="8672b-136">エントリポイントメソッドの戻り値の型が `void`である場合、そのメソッドを終了する右中かっこ (`}`) に到達するか、式のない `return` ステートメントを実行すると、終了ステータスコード `0`が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-136">If the return type of the entry point method is `void`, reaching the right brace (`}`) which terminates that method, or executing a `return` statement that has no expression, results in a termination status code of `0`.</span></span>

<span data-ttu-id="8672b-137">アプリケーションが終了する前は、ガベージコレクションされていないすべてのオブジェクトのデストラクターが呼び出されます。ただし、そのようなクリーンアップが抑制されている場合 (ライブラリメソッド `GC.SuppressFinalize`の呼び出しによって) は呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="8672b-137">Prior to an application's termination, destructors for all of its objects that have not yet been garbage collected are called, unless such cleanup has been suppressed (by a call to the library method `GC.SuppressFinalize`, for example).</span></span>

## <a name="declarations"></a><span data-ttu-id="8672b-138">宣言</span><span class="sxs-lookup"><span data-stu-id="8672b-138">Declarations</span></span>

<span data-ttu-id="8672b-139">C#プログラムの宣言は、プログラムの構成要素を定義します。</span><span class="sxs-lookup"><span data-stu-id="8672b-139">Declarations in a C# program define the constituent elements of the program.</span></span> <span data-ttu-id="8672b-140">C#プログラムは名前空間 ([名前空間](namespaces.md)) を使用して編成され、型宣言と入れ子になった名前空間宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-140">C# programs are organized using namespaces ([Namespaces](namespaces.md)), which can contain type declarations and nested namespace declarations.</span></span> <span data-ttu-id="8672b-141">型宣言 ([型宣言](namespaces.md#type-declarations)) は、クラス ([クラス](classes.md))、構造体 ([構造体](structs.md))、インターフェイス ([インターフェイス](interfaces.md))、列挙型 ([列挙](enums.md)型)、およびデリゲート ([デリゲート](delegates.md)) を定義するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-141">Type declarations ([Type declarations](namespaces.md#type-declarations)) are used to define classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md)), enums ([Enums](enums.md)), and delegates ([Delegates](delegates.md)).</span></span> <span data-ttu-id="8672b-142">型宣言で許可されるメンバーの種類は、型宣言の形式によって異なります。</span><span class="sxs-lookup"><span data-stu-id="8672b-142">The kinds of members permitted in a type declaration depend on the form of the type declaration.</span></span> <span data-ttu-id="8672b-143">たとえば、クラス宣言には、定数 ([定数](classes.md#constants))、フィールド ([フィールド](classes.md#fields))、メソッド ([メソッド](classes.md#methods))、プロパティ ([プロパティ](classes.md#properties))、イベント ([イベント](classes.md#events))、インデクサー ([インデクサー](classes.md#indexers))、演算子 ([演算子](classes.md#operators))、インスタンスコンストラクター ([インスタンスコンストラクター](classes.md#instance-constructors))、静的コンストラクター ([静的コンストラクター](classes.md#static-constructors))、デストラクター ([デストラクター](classes.md#destructors))、および入れ子にされた型 (入れ子にされた[型](classes.md#nested-types)) の宣言を含める</span><span class="sxs-lookup"><span data-stu-id="8672b-143">For instance, class declarations can contain declarations for constants ([Constants](classes.md#constants)), fields ([Fields](classes.md#fields)), methods ([Methods](classes.md#methods)), properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), indexers ([Indexers](classes.md#indexers)), operators ([Operators](classes.md#operators)), instance constructors ([Instance constructors](classes.md#instance-constructors)), static constructors ([Static constructors](classes.md#static-constructors)), destructors ([Destructors](classes.md#destructors)), and nested types ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="8672b-144">宣言は、宣言が属する宣言***空間***内の名前を定義します。</span><span class="sxs-lookup"><span data-stu-id="8672b-144">A declaration defines a name in the ***declaration space*** to which the declaration belongs.</span></span> <span data-ttu-id="8672b-145">オーバーロードされたメンバー ([シグネチャおよびオーバーロード](basic-concepts.md#signatures-and-overloading)) を除き、宣言空間で同じ名前のメンバーを導入する2つ以上の宣言があると、コンパイル時にエラーになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-145">Except for overloaded members ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)), it is a compile-time error to have two or more declarations that introduce members with the same name in a declaration space.</span></span> <span data-ttu-id="8672b-146">宣言空間には、同じ名前を持つ異なる種類のメンバーを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-146">It is never possible for a declaration space to contain different kinds of members with the same name.</span></span> <span data-ttu-id="8672b-147">たとえば、宣言空間には、同じ名前のフィールドとメソッドを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-147">For example, a declaration space can never contain a field and a method by the same name.</span></span>

<span data-ttu-id="8672b-148">次に示すように、さまざまな種類の宣言スペースがあります。</span><span class="sxs-lookup"><span data-stu-id="8672b-148">There are several different types of declaration spaces, as described in the following.</span></span>

*  <span data-ttu-id="8672b-149">プログラムのすべてのソースファイル内で、を囲む*namespace_declaration*を持たない*namespace_member_declaration*は、***グローバル宣言空間***と呼ばれる1つの結合された宣言空間のメンバーになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-149">Within all source files of a program, *namespace_member_declaration*s with no enclosing *namespace_declaration* are members of a single combined declaration space called the ***global declaration space***.</span></span>
*  <span data-ttu-id="8672b-150">プログラムのすべてのソースファイル内で、 *namespace_declaration*内の*namespace_member_declaration*は、完全修飾された同じ名前空間名を持つ1つの組み合わせの宣言空間のメンバーになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-150">Within all source files of a program, *namespace_member_declaration*s within *namespace_declaration*s that have the same fully qualified namespace name are members of a single combined declaration space.</span></span>
*  <span data-ttu-id="8672b-151">クラス、構造体、またはインターフェイスの宣言ごとに、新しい宣言領域が作成されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-151">Each class, struct, or interface declaration creates a new declaration space.</span></span> <span data-ttu-id="8672b-152">名前は、 *class_member_declaration*s、 *struct_member_declaration*s、 *interface_member_declaration*s、または*type_parameter*s を介してこの宣言領域に導入されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-152">Names are introduced into this declaration space through *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, or *type_parameter*s.</span></span> <span data-ttu-id="8672b-153">オーバーロードされたインスタンスコンストラクター宣言と静的コンストラクター宣言を除き、クラスまたは構造体には、クラスまたは構造体と同じ名前を持つメンバー宣言を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-153">Except for overloaded instance constructor declarations and static constructor declarations, a class or struct cannot contain a member declaration with the same name as the class or struct.</span></span> <span data-ttu-id="8672b-154">クラス、構造体、またはインターフェイスは、オーバーロードされたメソッドとインデクサーの宣言を許可します。</span><span class="sxs-lookup"><span data-stu-id="8672b-154">A class, struct, or interface permits the declaration of overloaded methods and indexers.</span></span> <span data-ttu-id="8672b-155">さらに、クラスまたは構造体は、オーバーロードされたインスタンスコンストラクターと演算子の宣言を許可します。</span><span class="sxs-lookup"><span data-stu-id="8672b-155">Furthermore, a class or struct permits the declaration of overloaded instance constructors and operators.</span></span> <span data-ttu-id="8672b-156">たとえば、クラス、構造体、またはインターフェイスには、同じ名前を持つ複数のメソッド宣言が含まれている場合があります。これらのメソッド宣言がシグネチャ ([シグネチャとオーバーロード](basic-concepts.md#signatures-and-overloading)) で異なる場合に限ります。</span><span class="sxs-lookup"><span data-stu-id="8672b-156">For example, a class, struct, or interface may contain multiple method declarations with the same name, provided these method declarations differ in their signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)).</span></span> <span data-ttu-id="8672b-157">基底クラスはクラスの宣言空間に関与しないことに注意してください。基本インターフェイスは、インターフェイスの宣言空間に関与しません。</span><span class="sxs-lookup"><span data-stu-id="8672b-157">Note that base classes do not contribute to the declaration space of a class, and base interfaces do not contribute to the declaration space of an interface.</span></span> <span data-ttu-id="8672b-158">したがって、派生クラスまたはインターフェイスは、継承されたメンバーと同じ名前を持つメンバーを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-158">Thus, a derived class or interface is allowed to declare a member with the same name as an inherited member.</span></span> <span data-ttu-id="8672b-159">このようなメンバーは、継承されたメンバーを***非表示***にすると言います。</span><span class="sxs-lookup"><span data-stu-id="8672b-159">Such a member is said to ***hide*** the inherited member.</span></span>
*  <span data-ttu-id="8672b-160">各デリゲート宣言によって、新しい宣言空間が作成されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-160">Each delegate declaration creates a new declaration space.</span></span> <span data-ttu-id="8672b-161">名前は、仮パラメーター (*fixed_parameter*s および*parameter_array*s) と*type_parameter*s を使用して、この宣言領域に導入されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-161">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span>
*  <span data-ttu-id="8672b-162">列挙型の宣言ごとに、新しい宣言空間が作成されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-162">Each enumeration declaration creates a new declaration space.</span></span> <span data-ttu-id="8672b-163">名前は、 *enum_member_declarations*によってこの宣言領域に導入されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-163">Names are introduced into this declaration space through *enum_member_declarations*.</span></span>
*  <span data-ttu-id="8672b-164">各メソッド宣言、インデクサー宣言、演算子宣言、インスタンスコンストラクター宣言、および匿名関数は、***ローカル変数宣言空間***と呼ばれる新しい宣言領域を作成します。</span><span class="sxs-lookup"><span data-stu-id="8672b-164">Each method declaration, indexer declaration, operator declaration, instance constructor declaration and anonymous function creates a new declaration space called a ***local variable declaration space***.</span></span> <span data-ttu-id="8672b-165">名前は、仮パラメーター (*fixed_parameter*s および*parameter_array*s) と*type_parameter*s を使用して、この宣言領域に導入されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-165">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span> <span data-ttu-id="8672b-166">関数メンバーまたは匿名関数 (存在する場合) の本体は、ローカル変数宣言領域内で入れ子になっていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="8672b-166">The body of the function member or anonymous function, if any, is considered to be nested within the local variable declaration space.</span></span> <span data-ttu-id="8672b-167">ローカル変数宣言空間と入れ子になったローカル変数宣言空間が同じ名前の要素を含むようにすると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-167">It is an error for a local variable declaration space and a nested local variable declaration space to contain elements with the same name.</span></span> <span data-ttu-id="8672b-168">したがって、入れ子になった宣言領域内では、ローカル変数または定数を、外側の宣言空間内のローカル変数または定数と同じ名前で宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-168">Thus, within a nested declaration space it is not possible to declare a local variable or constant with the same name as a local variable or constant in an enclosing declaration space.</span></span> <span data-ttu-id="8672b-169">2つの宣言空間に、同じ名前の要素を含めることができます。ただし、宣言領域に他方の要素が含まれている必要はありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-169">It is possible for two declaration spaces to contain elements with the same name as long as neither declaration space contains the other.</span></span>
*  <span data-ttu-id="8672b-170">各*ブロック*または*switch_block* 、および*for*、 *foreach* 、および*using*ステートメントでは、ローカル変数とローカル定数のローカル変数宣言空間が作成されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-170">Each *block* or *switch_block* , as well as a *for*, *foreach* and *using* statement, creates a local variable declaration space for local variables and local constants .</span></span> <span data-ttu-id="8672b-171">名前は*local_variable_declaration*s および*local_constant_declaration*s を介してこの宣言領域に導入されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-171">Names are introduced into this declaration space through *local_variable_declaration*s and *local_constant_declaration*s.</span></span> <span data-ttu-id="8672b-172">関数メンバーまたは匿名関数の本体内またはその内部で発生するブロックは、それらのパラメーターの関数によって宣言されたローカル変数宣言領域内で入れ子になっています。</span><span class="sxs-lookup"><span data-stu-id="8672b-172">Note that blocks that occur as or within the body of a function member or anonymous function are nested within the local variable declaration space declared by those functions for their parameters.</span></span> <span data-ttu-id="8672b-173">したがって、ローカル変数を持つメソッドや、同じ名前のパラメーターを使用すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-173">Thus it is an error to have e.g. a method with a local variable and a parameter of the same name.</span></span>
*  <span data-ttu-id="8672b-174">各*ブロック*または*switch_block*は、ラベル用に個別の宣言領域を作成します。</span><span class="sxs-lookup"><span data-stu-id="8672b-174">Each *block* or *switch_block* creates a separate declaration space for labels.</span></span> <span data-ttu-id="8672b-175">名前は*labeled_statement*s を介してこの宣言領域に導入され、名前は*goto_statement*s を介して参照されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-175">Names are introduced into this declaration space through *labeled_statement*s, and the names are referenced through *goto_statement*s.</span></span> <span data-ttu-id="8672b-176">ブロックの***ラベル宣言領域***には、入れ子になったブロックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8672b-176">The ***label declaration space*** of a block includes any nested blocks.</span></span> <span data-ttu-id="8672b-177">したがって、入れ子になったブロック内では、外側のブロックでラベルと同じ名前のラベルを宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-177">Thus, within a nested block it is not possible to declare a label with the same name as a label in an enclosing block.</span></span>

<span data-ttu-id="8672b-178">名前が宣言される文字列の順序は、一般に意味がありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-178">The textual order in which names are declared is generally of no significance.</span></span> <span data-ttu-id="8672b-179">特に、名前空間、定数、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンスコンストラクター、デストラクター、静的コンストラクター、および型の宣言と使用については、テキストの順序は重要ではありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-179">In particular, textual order is not significant for the declaration and use of namespaces, constants, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors, and types.</span></span> <span data-ttu-id="8672b-180">宣言の順序は、次の点で重要です。</span><span class="sxs-lookup"><span data-stu-id="8672b-180">Declaration order is significant in the following ways:</span></span>

*  <span data-ttu-id="8672b-181">フィールド宣言とローカル変数宣言の宣言順序によって、初期化子 (存在する場合) が実行される順序が決まります。</span><span class="sxs-lookup"><span data-stu-id="8672b-181">Declaration order for field declarations and local variable declarations determines the order in which their initializers (if any) are executed.</span></span>
*  <span data-ttu-id="8672b-182">ローカル変数は、使用する前に定義する必要があります ([スコープ](basic-concepts.md#scopes))。</span><span class="sxs-lookup"><span data-stu-id="8672b-182">Local variables must be defined before they are used ([Scopes](basic-concepts.md#scopes)).</span></span>
*  <span data-ttu-id="8672b-183">列挙メンバー宣言 ([列挙メンバー](enums.md#enum-members)) の宣言順序は、 *constant_expression*値を省略した場合に重要になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-183">Declaration order for enum member declarations ([Enum members](enums.md#enum-members)) is significant when *constant_expression* values are omitted.</span></span>

<span data-ttu-id="8672b-184">名前空間の宣言領域は "open 終了" であり、同じ完全修飾名を持つ2つの名前空間宣言は、同じ宣言空間に関与します。</span><span class="sxs-lookup"><span data-stu-id="8672b-184">The declaration space of a namespace is "open ended", and two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="8672b-185">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8672b-185">For example</span></span>
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

<span data-ttu-id="8672b-186">上記の2つの名前空間宣言は同じ宣言領域に寄与します。この例では、完全修飾名を持つ2つのクラスを宣言し `Megacorp.Data.Customer` および `Megacorp.Data.Order`ます。</span><span class="sxs-lookup"><span data-stu-id="8672b-186">The two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Megacorp.Data.Customer` and `Megacorp.Data.Order`.</span></span> <span data-ttu-id="8672b-187">2つの宣言は同じ宣言領域に関与するため、それぞれに同じ名前のクラスの宣言が含まれていると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-187">Because the two declarations contribute to the same declaration space, it would have caused a compile-time error if each contained a declaration of a class with the same name.</span></span>

<span data-ttu-id="8672b-188">前述のように、ブロックの宣言空間には、入れ子になったブロックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8672b-188">As specified above, the declaration space of a block includes any nested blocks.</span></span> <span data-ttu-id="8672b-189">したがって、次の例では、`F` メソッドと `G` メソッドによってコンパイル時エラーが発生します。これは、名前 `i` が外側のブロックで宣言され、内部ブロックで再宣言できないためです。</span><span class="sxs-lookup"><span data-stu-id="8672b-189">Thus, in the following example, the `F` and `G` methods result in a compile-time error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="8672b-190">ただし、2つの `i`が入れ子になっていない個別のブロックで宣言されているため、`H` メソッドと `I` メソッドは有効です。</span><span class="sxs-lookup"><span data-stu-id="8672b-190">However, the `H` and `I` methods are valid since the two `i`'s are declared in separate non-nested blocks.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a><span data-ttu-id="8672b-191">メンバー</span><span class="sxs-lookup"><span data-stu-id="8672b-191">Members</span></span>

<span data-ttu-id="8672b-192">名前空間と型には***メンバー***があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-192">Namespaces and types have ***members***.</span></span> <span data-ttu-id="8672b-193">エンティティのメンバーは、エンティティへの参照で始まる修飾名を使用し、その後に "`.`" トークンを続け、その後にメンバーの名前を指定することで、一般に使用できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-193">The members of an entity are generally available through the use of a qualified name that starts with a reference to the entity, followed by a "`.`" token, followed by the name of the member.</span></span>

<span data-ttu-id="8672b-194">型のメンバーは、型宣言で宣言されているか、型の基底クラスから***継承***されています。</span><span class="sxs-lookup"><span data-stu-id="8672b-194">Members of a type are either declared in the type declaration or ***inherited*** from the base class of the type.</span></span> <span data-ttu-id="8672b-195">型が基本クラスから継承する場合、インスタンスコンストラクター、デストラクター、および静的コンストラクターを除く、基本クラスのすべてのメンバーが派生型のメンバーになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-195">When a type inherits from a base class, all members of the base class, except instance constructors, destructors and static constructors, become members of the derived type.</span></span> <span data-ttu-id="8672b-196">基底クラスのメンバーに対して宣言されたアクセシビリティでは、メンバーが継承されるかどうかは制御されません。継承は、インスタンスコンストラクター、静的コンストラクター、またはデストラクターではない任意のメンバーに及びます。</span><span class="sxs-lookup"><span data-stu-id="8672b-196">The declared accessibility of a base class member does not control whether the member is inherited—inheritance extends to any member that isn't an instance constructor, static constructor, or destructor.</span></span> <span data-ttu-id="8672b-197">ただし、継承されたメンバーは、アクセシビリティが宣言されている ([アクセシビリティが宣言](basic-concepts.md#declared-accessibility)されている) か、型自体の宣言によって非表示になっている (継承によって[隠ぺい](basic-concepts.md#hiding-through-inheritance)されている) ため、派生型ではアクセスできない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-197">However, an inherited member may not be accessible in a derived type, either because of its declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) or because it is hidden by a declaration in the type itself ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

### <a name="namespace-members"></a><span data-ttu-id="8672b-198">名前空間のメンバー</span><span class="sxs-lookup"><span data-stu-id="8672b-198">Namespace members</span></span>

<span data-ttu-id="8672b-199">外側の名前空間を持たない名前空間と型は、***グローバル名前空間***のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-199">Namespaces and types that have no enclosing namespace are members of the ***global namespace***.</span></span> <span data-ttu-id="8672b-200">これは、グローバル宣言領域で宣言されている名前に直接対応します。</span><span class="sxs-lookup"><span data-stu-id="8672b-200">This corresponds directly to the names declared in the global declaration space.</span></span>

<span data-ttu-id="8672b-201">名前空間内で宣言された名前空間と型は、その名前空間のメンバーになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-201">Namespaces and types declared within a namespace are members of that namespace.</span></span> <span data-ttu-id="8672b-202">これは、名前空間の宣言空間で宣言されている名前に直接対応します。</span><span class="sxs-lookup"><span data-stu-id="8672b-202">This corresponds directly to the names declared in the declaration space of the namespace.</span></span>

<span data-ttu-id="8672b-203">名前空間には、アクセス制限がありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-203">Namespaces have no access restrictions.</span></span> <span data-ttu-id="8672b-204">プライベート、プロテクト、または内部の名前空間を宣言することはできません。また、名前空間名には常にパブリックにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="8672b-204">It is not possible to declare private, protected, or internal namespaces, and namespace names are always publicly accessible.</span></span>

### <a name="struct-members"></a><span data-ttu-id="8672b-205">構造体のメンバー</span><span class="sxs-lookup"><span data-stu-id="8672b-205">Struct members</span></span>

<span data-ttu-id="8672b-206">構造体のメンバーは、構造体で宣言されたメンバーと、構造体の直接基底クラス `System.ValueType` から継承されたメンバー、および間接基底クラス `object`です。</span><span class="sxs-lookup"><span data-stu-id="8672b-206">The members of a struct are the members declared in the struct and the members inherited from the struct's direct base class `System.ValueType` and the indirect base class `object`.</span></span>

<span data-ttu-id="8672b-207">単純型のメンバーは、単純型によってエイリアス化された構造体型のメンバーに直接対応します。</span><span class="sxs-lookup"><span data-stu-id="8672b-207">The members of a simple type correspond directly to the members of the struct type aliased by the simple type:</span></span>

*  <span data-ttu-id="8672b-208">`sbyte` のメンバーは、`System.SByte` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-208">The members of `sbyte` are the members of the `System.SByte` struct.</span></span>
*  <span data-ttu-id="8672b-209">`byte` のメンバーは、`System.Byte` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-209">The members of `byte` are the members of the `System.Byte` struct.</span></span>
*  <span data-ttu-id="8672b-210">`short` のメンバーは、`System.Int16` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-210">The members of `short` are the members of the `System.Int16` struct.</span></span>
*  <span data-ttu-id="8672b-211">`ushort` のメンバーは、`System.UInt16` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-211">The members of `ushort` are the members of the `System.UInt16` struct.</span></span>
*  <span data-ttu-id="8672b-212">`int` のメンバーは、`System.Int32` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-212">The members of `int` are the members of the `System.Int32` struct.</span></span>
*  <span data-ttu-id="8672b-213">`uint` のメンバーは、`System.UInt32` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-213">The members of `uint` are the members of the `System.UInt32` struct.</span></span>
*  <span data-ttu-id="8672b-214">`long` のメンバーは、`System.Int64` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-214">The members of `long` are the members of the `System.Int64` struct.</span></span>
*  <span data-ttu-id="8672b-215">`ulong` のメンバーは、`System.UInt64` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-215">The members of `ulong` are the members of the `System.UInt64` struct.</span></span>
*  <span data-ttu-id="8672b-216">`char` のメンバーは、`System.Char` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-216">The members of `char` are the members of the `System.Char` struct.</span></span>
*  <span data-ttu-id="8672b-217">`float` のメンバーは、`System.Single` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-217">The members of `float` are the members of the `System.Single` struct.</span></span>
*  <span data-ttu-id="8672b-218">`double` のメンバーは、`System.Double` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-218">The members of `double` are the members of the `System.Double` struct.</span></span>
*  <span data-ttu-id="8672b-219">`decimal` のメンバーは、`System.Decimal` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-219">The members of `decimal` are the members of the `System.Decimal` struct.</span></span>
*  <span data-ttu-id="8672b-220">`bool` のメンバーは、`System.Boolean` 構造体のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-220">The members of `bool` are the members of the `System.Boolean` struct.</span></span>

### <a name="enumeration-members"></a><span data-ttu-id="8672b-221">列挙型のメンバー</span><span class="sxs-lookup"><span data-stu-id="8672b-221">Enumeration members</span></span>

<span data-ttu-id="8672b-222">列挙体のメンバーは、列挙体で宣言された定数と、列挙体の direct 基底クラス `System.Enum` から継承されたメンバー、および `System.ValueType` と `object`の間接的な基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="8672b-222">The members of an enumeration are the constants declared in the enumeration and the members inherited from the enumeration's direct base class `System.Enum` and the indirect base classes `System.ValueType` and `object`.</span></span>

### <a name="class-members"></a><span data-ttu-id="8672b-223">クラス メンバー</span><span class="sxs-lookup"><span data-stu-id="8672b-223">Class members</span></span>

<span data-ttu-id="8672b-224">クラスのメンバーは、クラスで宣言されたメンバーと、基本クラスから継承されたメンバー (基底クラスを持たないクラス `object` を除く) です。</span><span class="sxs-lookup"><span data-stu-id="8672b-224">The members of a class are the members declared in the class and the members inherited from the base class (except for class `object` which has no base class).</span></span> <span data-ttu-id="8672b-225">基本クラスから継承されたメンバーには、基本クラスの定数、フィールド、メソッド、プロパティ、イベント、インデクサー、演算子、および型が含まれますが、インスタンスコンストラクター、デストラクター、および基底クラスの静的コンストラクターは含まれません。</span><span class="sxs-lookup"><span data-stu-id="8672b-225">The members inherited from the base class include the constants, fields, methods, properties, events, indexers, operators, and types of the base class, but not the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="8672b-226">基底クラスのメンバーは、アクセシビリティに関係なく継承されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-226">Base class members are inherited without regard to their accessibility.</span></span>

<span data-ttu-id="8672b-227">クラス宣言には、定数、フィールド、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンスコンストラクター、デストラクター、静的コンストラクター、および型の宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-227">A class declaration may contain declarations of constants, fields, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors and types.</span></span>

<span data-ttu-id="8672b-228">`object` と `string` のメンバーは、そのエイリアスを持つクラス型のメンバーに直接対応します。</span><span class="sxs-lookup"><span data-stu-id="8672b-228">The members of `object` and `string` correspond directly to the members of the class types they alias:</span></span>

*  <span data-ttu-id="8672b-229">`object` のメンバーは、`System.Object` クラスのメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-229">The members of `object` are the members of the `System.Object` class.</span></span>
*  <span data-ttu-id="8672b-230">`string` のメンバーは、`System.String` クラスのメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-230">The members of `string` are the members of the `System.String` class.</span></span>

### <a name="interface-members"></a><span data-ttu-id="8672b-231">インターフェイスのメンバー</span><span class="sxs-lookup"><span data-stu-id="8672b-231">Interface members</span></span>

<span data-ttu-id="8672b-232">インターフェイスのメンバーは、インターフェイス内で宣言されたメンバーと、インターフェイスのすべての基本インターフェイスで宣言されています。</span><span class="sxs-lookup"><span data-stu-id="8672b-232">The members of an interface are the members declared in the interface and in all base interfaces of the interface.</span></span> <span data-ttu-id="8672b-233">`object` クラスのメンバーは、厳密に言えば、インターフェイス ([インターフェイスメンバー](interfaces.md#interface-members)) のメンバーではありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-233">The members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="8672b-234">ただし、クラス `object` のメンバーは、任意のインターフェイス型 ([メンバー参照](expressions.md#member-lookup)) でメンバー参照を介して使用できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-234">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="array-members"></a><span data-ttu-id="8672b-235">配列のメンバー</span><span class="sxs-lookup"><span data-stu-id="8672b-235">Array members</span></span>

<span data-ttu-id="8672b-236">配列のメンバーは、クラス `System.Array`から継承されたメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-236">The members of an array are the members inherited from class `System.Array`.</span></span>

### <a name="delegate-members"></a><span data-ttu-id="8672b-237">デリゲートメンバー</span><span class="sxs-lookup"><span data-stu-id="8672b-237">Delegate members</span></span>

<span data-ttu-id="8672b-238">デリゲートのメンバーは、クラス `System.Delegate`から継承されたメンバーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-238">The members of a delegate are the members inherited from class `System.Delegate`.</span></span>

## <a name="member-access"></a><span data-ttu-id="8672b-239">メンバー アクセス。</span><span class="sxs-lookup"><span data-stu-id="8672b-239">Member access</span></span>

<span data-ttu-id="8672b-240">メンバーの宣言によって、メンバーアクセスを制御できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-240">Declarations of members allow control over member access.</span></span> <span data-ttu-id="8672b-241">メンバーのアクセシビリティは、メンバーのアクセシビリティ ([宣言](basic-concepts.md#declared-accessibility)されたアクセシビリティ) によって、すぐに含まれる型のアクセシビリティ (存在する場合) と組み合わせて確立されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-241">The accessibility of a member is established by the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of the member combined with the accessibility of the immediately containing type, if any.</span></span>

<span data-ttu-id="8672b-242">特定のメンバーへのアクセスが許可されている場合、そのメンバーは***アクセス可能***であると言われます。</span><span class="sxs-lookup"><span data-stu-id="8672b-242">When access to a particular member is allowed, the member is said to be ***accessible***.</span></span> <span data-ttu-id="8672b-243">逆に、特定のメンバーへのアクセスが許可されていない場合、そのメンバーはアクセス***不可能***と見なされます。</span><span class="sxs-lookup"><span data-stu-id="8672b-243">Conversely, when access to a particular member is disallowed, the member is said to be ***inaccessible***.</span></span> <span data-ttu-id="8672b-244">メンバーへのアクセスが許可されるのは、アクセスが行われるテキストの場所が、メンバーのアクセシビリティドメイン ([アクセシビリティ](basic-concepts.md#accessibility-domains)ドメイン) に含まれている場合です。</span><span class="sxs-lookup"><span data-stu-id="8672b-244">Access to a member is permitted when the textual location in which the access takes place is included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>

### <a name="declared-accessibility"></a><span data-ttu-id="8672b-245">アクセシビリティの宣言</span><span class="sxs-lookup"><span data-stu-id="8672b-245">Declared accessibility</span></span>

<span data-ttu-id="8672b-246">メンバーに対して宣言された***アクセシビリティ***は、次のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-246">The ***declared accessibility*** of a member can be one of the following:</span></span>

*  <span data-ttu-id="8672b-247">Public。メンバー宣言に `public` 修飾子を含めることによって選択されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-247">Public, which is selected by including a `public` modifier in the member declaration.</span></span> <span data-ttu-id="8672b-248">`public` の直感的な意味は、"アクセスが制限されていません" です。</span><span class="sxs-lookup"><span data-stu-id="8672b-248">The intuitive meaning of `public` is "access not limited".</span></span>
*  <span data-ttu-id="8672b-249">Protected。メンバー宣言に `protected` 修飾子を含めることによって選択されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-249">Protected, which is selected by including a `protected` modifier in the member declaration.</span></span> <span data-ttu-id="8672b-250">`protected` の直感的な意味は、"含まれるクラスに対してアクセスが制限されているか、そのクラスから派生した型" です。</span><span class="sxs-lookup"><span data-stu-id="8672b-250">The intuitive meaning of `protected` is "access limited to the containing class or types derived from the containing class".</span></span>
*  <span data-ttu-id="8672b-251">Internal。メンバー宣言に `internal` 修飾子を含めることによって選択されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-251">Internal, which is selected by including an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="8672b-252">`internal` の直感的な意味は、"このプログラムに対するアクセスが制限されています" ということです。</span><span class="sxs-lookup"><span data-stu-id="8672b-252">The intuitive meaning of `internal` is "access limited to this program".</span></span>
*  <span data-ttu-id="8672b-253">Protected internal (protected または internal)。メンバー宣言に `protected` と `internal` 修飾子の両方を含めることによって選択されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-253">Protected internal (meaning protected or internal), which is selected by including both a `protected` and an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="8672b-254">`protected internal` の直感的な意味は、"このプログラムにはアクセスが制限されます。または、包含クラスから派生した型" です。</span><span class="sxs-lookup"><span data-stu-id="8672b-254">The intuitive meaning of `protected internal` is "access limited to this program or types derived from the containing class".</span></span>
*  <span data-ttu-id="8672b-255">Private。メンバー宣言に `private` 修飾子を含めることによって選択されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-255">Private, which is selected by including a `private` modifier in the member declaration.</span></span> <span data-ttu-id="8672b-256">`private` の直感的な意味は、"含まれる型に制限されたアクセス" です。</span><span class="sxs-lookup"><span data-stu-id="8672b-256">The intuitive meaning of `private` is "access limited to the containing type".</span></span>

<span data-ttu-id="8672b-257">メンバー宣言が行われるコンテキストによっては、特定の種類のアクセシビリティのみが許可されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-257">Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted.</span></span> <span data-ttu-id="8672b-258">さらに、メンバー宣言にアクセス修飾子が含まれていない場合、宣言が実行されるコンテキストによって、既定で宣言されたアクセシビリティが決まります。</span><span class="sxs-lookup"><span data-stu-id="8672b-258">Furthermore, when a member declaration does not include any access modifiers, the context in which the declaration takes place determines the default declared accessibility.</span></span>

*  <span data-ttu-id="8672b-259">名前空間は、宣言されたアクセシビリティを暗黙的に `public` します。</span><span class="sxs-lookup"><span data-stu-id="8672b-259">Namespaces implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="8672b-260">名前空間宣言でアクセス修飾子を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-260">No access modifiers are allowed on namespace declarations.</span></span>
*  <span data-ttu-id="8672b-261">コンパイル単位または名前空間で宣言された型は、`public` または `internal` 宣言されたアクセシビリティを持つことができ、宣言されたアクセシビリティを `internal` に既定値を持つ</span><span class="sxs-lookup"><span data-stu-id="8672b-261">Types declared in compilation units or namespaces can have `public` or `internal` declared accessibility and default to `internal` declared accessibility.</span></span>
*  <span data-ttu-id="8672b-262">クラスメンバーは、宣言されたアクセシビリティの5種類のいずれかを持つことができ、既定で宣言されたアクセシビリティ `private` に設定できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-262">Class members can have any of the five kinds of declared accessibility and default to `private` declared accessibility.</span></span> <span data-ttu-id="8672b-263">(クラスのメンバーとして宣言された型は、5種類のアクセシビリティを持つことができますが、名前空間のメンバーとして宣言された型は、`public` または `internal` 宣言されたアクセシビリティのみを持つことができます)。</span><span class="sxs-lookup"><span data-stu-id="8672b-263">(Note that a type declared as a member of a class can have any of the five kinds of declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="8672b-264">構造体メンバーは、`public`、`internal`、または `private` として宣言されたアクセシビリティを持つことができ、構造体は暗黙的にシールされるため、宣言されたアクセシビリティを既定で `private`</span><span class="sxs-lookup"><span data-stu-id="8672b-264">Struct members can have `public`, `internal`, or `private` declared accessibility and default to `private` declared accessibility because structs are implicitly sealed.</span></span> <span data-ttu-id="8672b-265">構造体で導入された構造体のメンバー (その構造体によって継承されない) は、`protected` または `protected internal` によって宣言されたアクセシビリティを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-265">Struct members introduced in a struct (that is, not inherited by that struct) cannot have `protected` or `protected internal` declared accessibility.</span></span> <span data-ttu-id="8672b-266">(構造体のメンバーとして宣言された型は、`public`、`internal`、または `private` アクセシビリティを宣言できますが、名前空間のメンバーとして宣言されている型は、`public` または `internal` のアクセシビリティのみを持つことができます)。</span><span class="sxs-lookup"><span data-stu-id="8672b-266">(Note that a type declared as a member of a struct can have `public`, `internal`, or `private` declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="8672b-267">インターフェイスのメンバーは、宣言されたアクセシビリティを暗黙的に `public` します。</span><span class="sxs-lookup"><span data-stu-id="8672b-267">Interface members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="8672b-268">インターフェイスメンバー宣言でアクセス修飾子を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-268">No access modifiers are allowed on interface member declarations.</span></span>
*  <span data-ttu-id="8672b-269">列挙型のメンバーは、宣言されたアクセシビリティを暗黙的に `public` します。</span><span class="sxs-lookup"><span data-stu-id="8672b-269">Enumeration members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="8672b-270">列挙メンバー宣言でアクセス修飾子を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-270">No access modifiers are allowed on enumeration member declarations.</span></span>

### <a name="accessibility-domains"></a><span data-ttu-id="8672b-271">アクセシビリティドメイン</span><span class="sxs-lookup"><span data-stu-id="8672b-271">Accessibility domains</span></span>

<span data-ttu-id="8672b-272">メンバーの***アクセシビリティドメイン***は、プログラムテキストの中で、メンバーへのアクセスが許可されている (不整合の可能性がある) セクションで構成されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-272">The ***accessibility domain*** of a member consists of the (possibly disjoint) sections of program text in which access to the member is permitted.</span></span> <span data-ttu-id="8672b-273">メンバーのアクセシビリティドメインを定義する目的では、メンバーが型の中で宣言されていない場合は***最上位レベル***と呼ばれ、別の型で宣言されている場合はメンバーが***入れ子になっ***ていると言います。</span><span class="sxs-lookup"><span data-stu-id="8672b-273">For purposes of defining the accessibility domain of a member, a member is said to be ***top-level*** if it is not declared within a type, and a member is said to be ***nested*** if it is declared within another type.</span></span> <span data-ttu-id="8672b-274">さらに、プログラムの***プログラムテキスト***は、プログラムのすべてのソースファイルに含まれるすべてのプログラムテキストとして定義されます。また、型のプログラムテキストは、その型の*type_declaration*s に含まれるすべてのプログラムテキストとして定義されます (場合によっては、型の中で入れ子になっている型を含みます)。</span><span class="sxs-lookup"><span data-stu-id="8672b-274">Furthermore, the ***program text*** of a program is defined as all program text contained in all source files of the program, and the program text of a type is defined as all program text contained in the *type_declaration*s of that type (including, possibly, types that are nested within the type).</span></span>

<span data-ttu-id="8672b-275">定義済みの型 (`object`、`int`、`double`など) のアクセシビリティドメインは無制限です。</span><span class="sxs-lookup"><span data-stu-id="8672b-275">The accessibility domain of a predefined type (such as `object`, `int`, or `double`) is unlimited.</span></span>

<span data-ttu-id="8672b-276">プログラム `P` で宣言されている最上位レベルのバインドさ[れてい](types.md#bound-and-unbound-types)ない型 `T` のアクセシビリティドメインは、次のように定義されています。</span><span class="sxs-lookup"><span data-stu-id="8672b-276">The accessibility domain of a top-level unbound type `T` ([Bound and unbound types](types.md#bound-and-unbound-types)) that is declared in a program `P` is defined as follows:</span></span>

*  <span data-ttu-id="8672b-277">宣言された `T` のアクセシビリティが `public`場合、`T` のアクセシビリティドメインは `P` のプログラムテキストと `P`を参照するすべてのプログラムになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-277">If the declared accessibility of `T` is `public`, the accessibility domain of `T` is the program text of `P` and any program that references `P`.</span></span>
*  <span data-ttu-id="8672b-278">`T` に対して宣言されているアクセシビリティが `internal` の場合、`T` のアクセシビリティ ドメインは `P` のプログラム テキストになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-278">If the declared accessibility of `T` is `internal`, the accessibility domain of `T` is the program text of `P`.</span></span>

<span data-ttu-id="8672b-279">これらの定義から、最上位レベルのバインド解除された型のアクセシビリティドメインは、その型が宣言されているプログラムのプログラムテキスト以上であることに従います。</span><span class="sxs-lookup"><span data-stu-id="8672b-279">From these definitions it follows that the accessibility domain of a top-level unbound type is always at least the program text of the program in which that type is declared.</span></span>

<span data-ttu-id="8672b-280">構築された型 `T<A1, ..., An>` のアクセシビリティドメインは、バインドされていないジェネリック型 `T` のアクセシビリティドメインと、`A1, ..., An`型引数のアクセシビリティドメインの積集合です。</span><span class="sxs-lookup"><span data-stu-id="8672b-280">The accessibility domain for a constructed type `T<A1, ..., An>` is the intersection of the accessibility domain of the unbound generic type `T` and the accessibility domains of the type arguments `A1, ..., An`.</span></span>

<span data-ttu-id="8672b-281">入れ子になったメンバーのアクセシビリティドメインは、プログラム内で `T` 型で宣言されている `M` `P` 次のように定義されます (`M` 自体が型である場合もあります)。</span><span class="sxs-lookup"><span data-stu-id="8672b-281">The accessibility domain of a nested member `M` declared in a type `T` within a program `P` is defined as follows (noting that `M` itself may possibly be a type):</span></span>

*  <span data-ttu-id="8672b-282">`M` に対して宣言されたアクセシビリティが `public` の場合、`M` のアクセシビリティ ドメインは `T` のアクセシビリティ ドメインになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-282">If the declared accessibility of `M` is `public`, the accessibility domain of `M` is the accessibility domain of `T`.</span></span>
*  <span data-ttu-id="8672b-283">宣言された `M` のアクセシビリティが `protected internal`場合は、`P` のプログラムテキストと `T`から派生した任意の型のプログラムテキストの和集合に `D` します。これは `P`の外部で宣言されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-283">If the declared accessibility of `M` is `protected internal`, let `D` be the union of the program text of `P` and the program text of any type derived from `T`, which is declared outside `P`.</span></span> <span data-ttu-id="8672b-284">`M` のアクセシビリティドメインは、`T` のアクセシビリティドメインと `D`の共通部分です。</span><span class="sxs-lookup"><span data-stu-id="8672b-284">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="8672b-285">宣言された `M` のアクセシビリティが `protected`場合は、`T` のプログラムテキストと `T`から派生した任意の型のプログラムテキストの和集合を `D` ます。</span><span class="sxs-lookup"><span data-stu-id="8672b-285">If the declared accessibility of `M` is `protected`, let `D` be the union of the program text of `T` and the program text of any type derived from `T`.</span></span> <span data-ttu-id="8672b-286">`M` のアクセシビリティドメインは、`T` のアクセシビリティドメインと `D`の共通部分です。</span><span class="sxs-lookup"><span data-stu-id="8672b-286">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="8672b-287">`M` に対して宣言されているアクセシビリティが `internal` の場合、`M` のアクセシビリティ ドメインは、`T` のアクセシビリティ ドメインと `P` のプログラム テキストとの積集合になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-287">If the declared accessibility of `M` is `internal`, the accessibility domain of `M` is the intersection of the accessibility domain of `T` with the program text of `P`.</span></span>
*  <span data-ttu-id="8672b-288">`M` に対して宣言されているアクセシビリティが `private` の場合、`M` のアクセシビリティ ドメインは `T` のプログラム テキストになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-288">If the declared accessibility of `M` is `private`, the accessibility domain of `M` is the program text of `T`.</span></span>

<span data-ttu-id="8672b-289">これらの定義から、入れ子になったメンバーのアクセシビリティドメインは、メンバーが宣言されている型のプログラムテキスト以上であることに従います。</span><span class="sxs-lookup"><span data-stu-id="8672b-289">From these definitions it follows that the accessibility domain of a nested member is always at least the program text of the type in which the member is declared.</span></span> <span data-ttu-id="8672b-290">さらに、メンバーのアクセシビリティドメインは、メンバーが宣言されている型のアクセシビリティドメインを超えないようにします。</span><span class="sxs-lookup"><span data-stu-id="8672b-290">Furthermore, it follows that the accessibility domain of a member is never more inclusive than the accessibility domain of the type in which the member is declared.</span></span>

<span data-ttu-id="8672b-291">直感的な用語では、型またはメンバーの `M` にアクセスするときに、アクセスが許可されていることを確認するために次の手順が評価されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-291">In intuitive terms, when a type or member `M` is accessed, the following steps are evaluated to ensure that the access is permitted:</span></span>

*  <span data-ttu-id="8672b-292">1つ目の方法として `M` が (コンパイル単位または名前空間ではなく) 型の中で宣言されている場合、その型にアクセスできないと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-292">First, if `M` is declared within a type (as opposed to a compilation unit or a namespace), a compile-time error occurs if that type is not accessible.</span></span>
*  <span data-ttu-id="8672b-293">`M` が `public`場合は、アクセスが許可されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-293">Then, if `M` is `public`, the access is permitted.</span></span>
*  <span data-ttu-id="8672b-294">それ以外の場合、`M` が `protected internal`場合、`M` が宣言されているプログラム内でアクセスが発生した場合、または `M` が宣言され、派生クラスの型 ([インスタンスメンバーの保護されたアクセス](basic-concepts.md#protected-access-for-instance-members)) を介して実行されるクラスの内部にある場合は、アクセスが許可されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-294">Otherwise, if `M` is `protected internal`, the access is permitted if it occurs within the program in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="8672b-295">それ以外の場合、`M` が `protected`場合、`M` が宣言されているクラス内でアクセスが発生した場合、または `M` が宣言されていて、派生クラスの型 ([インスタンスメンバーの保護されたアクセス](basic-concepts.md#protected-access-for-instance-members)) を介して実行されるクラスの内部にある場合は、アクセスが許可されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-295">Otherwise, if `M` is `protected`, the access is permitted if it occurs within the class in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="8672b-296">それ以外の場合、`M` が `internal`場合、`M` が宣言されているプログラム内でアクセスが行われると、アクセスが許可されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-296">Otherwise, if `M` is `internal`, the access is permitted if it occurs within the program in which `M` is declared.</span></span>
*  <span data-ttu-id="8672b-297">それ以外の場合、`M` が `private`場合、`M` が宣言されている型内でアクセスが許可されていると、アクセスが許可されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-297">Otherwise, if `M` is `private`, the access is permitted if it occurs within the type in which `M` is declared.</span></span>
*  <span data-ttu-id="8672b-298">それ以外の場合は、型またはメンバーにアクセスできないため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-298">Otherwise, the type or member is inaccessible, and a compile-time error occurs.</span></span>

<span data-ttu-id="8672b-299">この例では、</span><span class="sxs-lookup"><span data-stu-id="8672b-299">In the example</span></span>
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
<span data-ttu-id="8672b-300">クラスとメンバーには、次のアクセシビリティドメインがあります。</span><span class="sxs-lookup"><span data-stu-id="8672b-300">the classes and members have the following accessibility domains:</span></span>

*  <span data-ttu-id="8672b-301">`A` と `A.X` のアクセシビリティドメインは無制限です。</span><span class="sxs-lookup"><span data-stu-id="8672b-301">The accessibility domain of `A` and `A.X` is unlimited.</span></span>
*  <span data-ttu-id="8672b-302">`A.Y`、`B`、`B.X`、`B.Y`、`B.C`、`B.C.X`、`B.C.Y` のアクセシビリティドメインは、格納しているプログラムのプログラムテキストです。</span><span class="sxs-lookup"><span data-stu-id="8672b-302">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the program text of the containing program.</span></span>
*  <span data-ttu-id="8672b-303">`A.Z` のアクセシビリティドメインは、`A`のプログラムテキストです。</span><span class="sxs-lookup"><span data-stu-id="8672b-303">The accessibility domain of `A.Z` is the program text of `A`.</span></span>
*  <span data-ttu-id="8672b-304">`B.Z` と `B.D` のアクセシビリティドメインは `B`のプログラムテキストであり、`B.C` および `B.D`のプログラムテキストを含みます。</span><span class="sxs-lookup"><span data-stu-id="8672b-304">The accessibility domain of `B.Z` and `B.D` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="8672b-305">`B.C.Z` のアクセシビリティドメインは、`B.C`のプログラムテキストです。</span><span class="sxs-lookup"><span data-stu-id="8672b-305">The accessibility domain of `B.C.Z` is the program text of `B.C`.</span></span>
*  <span data-ttu-id="8672b-306">`B.D.X` と `B.D.Y` のアクセシビリティドメインは `B`のプログラムテキストであり、`B.C` および `B.D`のプログラムテキストを含みます。</span><span class="sxs-lookup"><span data-stu-id="8672b-306">The accessibility domain of `B.D.X` and `B.D.Y` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="8672b-307">`B.D.Z` のアクセシビリティドメインは、`B.D`のプログラムテキストです。</span><span class="sxs-lookup"><span data-stu-id="8672b-307">The accessibility domain of `B.D.Z` is the program text of `B.D`.</span></span>

<span data-ttu-id="8672b-308">例に示すように、メンバーのアクセシビリティドメインは、それを含んでいる型のアクセシビリティドメインよりも大きくなることはありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-308">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="8672b-309">たとえば、すべての `X` メンバーがパブリックに宣言されたアクセシビリティを持つ場合でも、それ `A.X` 以外のすべてのメンバーには、包含する型によって制約されるアクセシビリティドメインがあります。</span><span class="sxs-lookup"><span data-stu-id="8672b-309">For example, even though all `X` members have public declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="8672b-310">「[メンバー](basic-concepts.md#members)」で説明したように、インスタンスコンストラクター、デストラクター、および静的コンストラクターを除く、基底クラスのすべてのメンバーは、派生型によって継承されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-310">As described in [Members](basic-concepts.md#members), all members of a base class, except for instance constructors, destructors and static constructors, are inherited by derived types.</span></span> <span data-ttu-id="8672b-311">これには、基底クラスのプライベートメンバーも含まれます。</span><span class="sxs-lookup"><span data-stu-id="8672b-311">This includes even private members of a base class.</span></span> <span data-ttu-id="8672b-312">ただし、プライベートメンバーのアクセシビリティドメインには、メンバーが宣言されている型のプログラムテキストのみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8672b-312">However, the accessibility domain of a private member includes only the program text of the type in which the member is declared.</span></span> <span data-ttu-id="8672b-313">この例では、</span><span class="sxs-lookup"><span data-stu-id="8672b-313">In the example</span></span>
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
<span data-ttu-id="8672b-314">`B` クラスは、`A` クラスからプライベートメンバー `x` を継承します。</span><span class="sxs-lookup"><span data-stu-id="8672b-314">the `B` class inherits the private member `x` from the `A` class.</span></span> <span data-ttu-id="8672b-315">メンバーはプライベートであるため、`A`の*class_body*内でのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="8672b-315">Because the member is private, it is only accessible within the *class_body* of `A`.</span></span> <span data-ttu-id="8672b-316">したがって、`b.x` へのアクセスは `A.F` メソッドで成功しますが、`B.F` メソッドでは失敗します。</span><span class="sxs-lookup"><span data-stu-id="8672b-316">Thus, the access to `b.x` succeeds in the `A.F` method, but fails in the `B.F` method.</span></span>

### <a name="protected-access-for-instance-members"></a><span data-ttu-id="8672b-317">インスタンスメンバーの保護されたアクセス</span><span class="sxs-lookup"><span data-stu-id="8672b-317">Protected access for instance members</span></span>

<span data-ttu-id="8672b-318">`protected` インスタンスメンバーが宣言されているクラスのプログラムテキストの外部でアクセスされ、`protected internal` インスタンスメンバーが宣言されているプログラムのプログラムテキストの外部にアクセスする場合、アクセスは、宣言されているクラスから派生したクラス宣言内で実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-318">When a `protected` instance member is accessed outside the program text of the class in which it is declared, and when a `protected internal` instance member is accessed outside the program text of the program in which it is declared, the access must take place within a class declaration that derives from the class in which it is declared.</span></span> <span data-ttu-id="8672b-319">さらに、その派生クラス型のインスタンス、またはそれから構築されたクラス型を使用してアクセスする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-319">Furthermore, the access is required to take place through an instance of that derived class type or a class type constructed from it.</span></span> <span data-ttu-id="8672b-320">この制限により、メンバーが同じ基本クラスから継承されている場合でも、1つの派生クラスが他の派生クラスのプロテクトメンバーにアクセスするのを防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-320">This restriction prevents one derived class from accessing protected members of other derived classes, even when the members are inherited from the same base class.</span></span>

<span data-ttu-id="8672b-321">`M`保護されたインスタンスメンバーを宣言する基底クラスとして `B` し、`B`から派生したクラスに `D` できるようにします。</span><span class="sxs-lookup"><span data-stu-id="8672b-321">Let `B` be a base class that declares a protected instance member `M`, and let `D` be a class that derives from `B`.</span></span> <span data-ttu-id="8672b-322">`D`の*class_body*内で `M` へのアクセスには、次のいずれかの形式を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-322">Within the *class_body* of `D`, access to `M` can take one of the following forms:</span></span>

*  <span data-ttu-id="8672b-323">`M`フォームの修飾されていない*type_name*または*primary_expression* 。</span><span class="sxs-lookup"><span data-stu-id="8672b-323">An unqualified *type_name* or *primary_expression* of the form `M`.</span></span>
*  <span data-ttu-id="8672b-324">フォーム `E.M`の*primary_expression* 。 `E` の型が `T` であるか、`T`から派生したクラス (`T` がクラス型 `D`、またはから構築されたクラス型である場合) `D`</span><span class="sxs-lookup"><span data-stu-id="8672b-324">A *primary_expression* of the form `E.M`, provided the type of `E` is `T` or a class derived from `T`, where `T` is the class type `D`, or a class type constructed from `D`</span></span>
*  <span data-ttu-id="8672b-325">フォーム `base.M`の*primary_expression* 。</span><span class="sxs-lookup"><span data-stu-id="8672b-325">A *primary_expression* of the form `base.M`.</span></span>

<span data-ttu-id="8672b-326">派生クラスは、これらの形式のアクセスに加えて、 *constructor_initializer*内の基底クラスの保護されたインスタンスコンストラクターにアクセスできます ([コンストラクター初期化子](classes.md#constructor-initializers))。</span><span class="sxs-lookup"><span data-stu-id="8672b-326">In addition to these forms of access, a derived class can access a protected instance constructor of a base class in a *constructor_initializer* ([Constructor initializers](classes.md#constructor-initializers)).</span></span>

<span data-ttu-id="8672b-327">この例では、</span><span class="sxs-lookup"><span data-stu-id="8672b-327">In the example</span></span>
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
<span data-ttu-id="8672b-328">`A`内では、`A` と `B`の両方のインスタンスを介して `x` にアクセスすることができます。どちらの場合も、アクセスは `A` のインスタンスまたは `A`から派生したクラスによって行われるためです。</span><span class="sxs-lookup"><span data-stu-id="8672b-328">within `A`, it is possible to access `x` through instances of both `A` and `B`, since in either case the access takes place through an instance of `A` or a class derived from `A`.</span></span> <span data-ttu-id="8672b-329">ただし `B`内では、`A` は `B`から派生していないため、`A`のインスタンスを介して `x` にアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-329">However, within `B`, it is not possible to access `x` through an instance of `A`, since `A` does not derive from `B`.</span></span>

<span data-ttu-id="8672b-330">この例では、</span><span class="sxs-lookup"><span data-stu-id="8672b-330">In the example</span></span>
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
<span data-ttu-id="8672b-331">`x` に対する3つの割り当ては、ジェネリック型から構築されたクラス型のインスタンスを通じてすべての処理が行われるため、許可されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-331">the three assignments to `x` are permitted because they all take place through instances of class types constructed from the generic type.</span></span>

### <a name="accessibility-constraints"></a><span data-ttu-id="8672b-332">アクセシビリティの制約</span><span class="sxs-lookup"><span data-stu-id="8672b-332">Accessibility constraints</span></span>

<span data-ttu-id="8672b-333">C#言語のいくつかの構造体は、少なくともメンバーまたは別の型***としてアクセスできる***型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-333">Several constructs in the C# language require a type to be ***at least as accessible as*** a member or another type.</span></span> <span data-ttu-id="8672b-334">`T` のアクセシビリティドメインが `M`のアクセシビリティドメインのスーパーセットである場合、`T` 型は少なくともメンバーまたは型 `M` と同じようにアクセス可能であると言います。</span><span class="sxs-lookup"><span data-stu-id="8672b-334">A type `T` is said to be at least as accessible as a member or type `M` if the accessibility domain of `T` is a superset of the accessibility domain of `M`.</span></span> <span data-ttu-id="8672b-335">つまり、`M` にアクセスできるすべてのコンテキストで `T` にアクセスできる場合、`T` は少なくとも `M` と同じようにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="8672b-335">In other words, `T` is at least as accessible as `M` if `T` is accessible in all contexts in which `M` is accessible.</span></span>

<span data-ttu-id="8672b-336">次のアクセシビリティ制約が存在します。</span><span class="sxs-lookup"><span data-stu-id="8672b-336">The following accessibility constraints exist:</span></span>

*  <span data-ttu-id="8672b-337">クラスの型の直接基底クラスは、少なくとも、クラスの型自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-337">The direct base class of a class type must be at least as accessible as the class type itself.</span></span>
*  <span data-ttu-id="8672b-338">インターフェイスの型の明示的な基本インターフェイスは、少なくとも、インターフェイスの型自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-338">The explicit base interfaces of an interface type must be at least as accessible as the interface type itself.</span></span>
*  <span data-ttu-id="8672b-339">デリゲート型の戻り値の型およびパラメーターの型は、少なくとも、デリゲート型自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-339">The return type and parameter types of a delegate type must be at least as accessible as the delegate type itself.</span></span>
*  <span data-ttu-id="8672b-340">定数の型は、少なくとも定数自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-340">The type of a constant must be at least as accessible as the constant itself.</span></span>
*  <span data-ttu-id="8672b-341">フィールドの型は、少なくともフィールド自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-341">The type of a field must be at least as accessible as the field itself.</span></span>
*  <span data-ttu-id="8672b-342">メソッドの戻り値の型およびパラメーターの型は、少なくとも、メソッド自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-342">The return type and parameter types of a method must be at least as accessible as the method itself.</span></span>
*  <span data-ttu-id="8672b-343">プロパティの型は、少なくともプロパティ自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-343">The type of a property must be at least as accessible as the property itself.</span></span>
*  <span data-ttu-id="8672b-344">イベントの型は、少なくともイベント自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-344">The type of an event must be at least as accessible as the event itself.</span></span>
*  <span data-ttu-id="8672b-345">インデクサーの型とパラメーターの型は、少なくとも、インデクサー自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-345">The type and parameter types of an indexer must be at least as accessible as the indexer itself.</span></span>
*  <span data-ttu-id="8672b-346">演算子の戻り値の型とパラメーターの型は、少なくとも、演算子自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-346">The return type and parameter types of an operator must be at least as accessible as the operator itself.</span></span>
*  <span data-ttu-id="8672b-347">インスタンスコンストラクターのパラメーターの型は、少なくともインスタンスコンストラクター自体と同じようにアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-347">The parameter types of an instance constructor must be at least as accessible as the instance constructor itself.</span></span>

<span data-ttu-id="8672b-348">この例では、</span><span class="sxs-lookup"><span data-stu-id="8672b-348">In the example</span></span>
```csharp
class A {...}

public class B: A {...}
```
<span data-ttu-id="8672b-349">`B` クラスは、`A` に少なくとも `B`としてアクセスできないため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-349">the `B` class results in a compile-time error because `A` is not at least as accessible as `B`.</span></span>

<span data-ttu-id="8672b-350">同様に、例では</span><span class="sxs-lookup"><span data-stu-id="8672b-350">Likewise, in the example</span></span>
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
<span data-ttu-id="8672b-351">`B` の `H` メソッドは、戻り値の型 `A` がメソッドとは少なくともアクセス可能ではないため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-351">the `H` method in `B` results in a compile-time error because the return type `A` is not at least as accessible as the method.</span></span>

## <a name="signatures-and-overloading"></a><span data-ttu-id="8672b-352">署名とオーバーロード</span><span class="sxs-lookup"><span data-stu-id="8672b-352">Signatures and overloading</span></span>

<span data-ttu-id="8672b-353">メソッド、インスタンスコンストラクター、インデクサー、および演算子は、***シグネチャ***によって特徴付けられます。</span><span class="sxs-lookup"><span data-stu-id="8672b-353">Methods, instance constructors, indexers, and operators are characterized by their ***signatures***:</span></span>

*  <span data-ttu-id="8672b-354">メソッドのシグネチャは、メソッドの名前、型パラメーターの数、それぞれの仮パラメーターの型と種類 (値、参照、または出力) で構成されます。順序は左から右になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-354">The signature of a method consists of the name of the method, the number of type parameters and the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="8672b-355">このため、仮パラメーターの型で発生するメソッドの型パラメーターは、その名前ではなく、メソッドの型引数リスト内の序数位置によって識別されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-355">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.</span></span> <span data-ttu-id="8672b-356">メソッドのシグネチャには、戻り値の型、右端のパラメーターに指定できる `params` 修飾子、および省略可能な型パラメーター制約は含まれません。</span><span class="sxs-lookup"><span data-stu-id="8672b-356">The signature of a method specifically does not include the return type, the `params` modifier that may be specified for the right-most parameter, nor the optional type parameter constraints.</span></span>
*  <span data-ttu-id="8672b-357">インスタンスコンストラクターのシグネチャは、それぞれの仮パラメーターの型と種類 (値、参照、または出力) で構成され、左から右の順序で考慮されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-357">The signature of an instance constructor consists of the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="8672b-358">インスタンスコンストラクターのシグネチャには、右端のパラメーターに指定できる `params` 修飾子が明示的に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="8672b-358">The signature of an instance constructor specifically does not include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="8672b-359">インデクサーのシグネチャは、それぞれの仮パラメーターの型で構成され、左から右の順序で考慮されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-359">The signature of an indexer consists of the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="8672b-360">インデクサーのシグネチャには、特に要素の型は含まれません。また、右端のパラメーターに指定できる `params` 修飾子も含まれません。</span><span class="sxs-lookup"><span data-stu-id="8672b-360">The signature of an indexer specifically does not include the element type, nor does it include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="8672b-361">演算子のシグネチャは、演算子の名前と、それぞれの仮パラメーターの型で構成されます。順序は左から右になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-361">The signature of an operator consists of the name of the operator and the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="8672b-362">演算子のシグネチャには、結果の型は含まれません。</span><span class="sxs-lookup"><span data-stu-id="8672b-362">The signature of an operator specifically does not include the result type.</span></span>

<span data-ttu-id="8672b-363">シグネチャは、クラス、構造体、およびインターフェイスのメンバーを***オーバーロード***するための有効なメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="8672b-363">Signatures are the enabling mechanism for ***overloading*** of members in classes, structs, and interfaces:</span></span>

*  <span data-ttu-id="8672b-364">メソッドのオーバーロードにより、クラス、構造体、またはインターフェイスは、シグネチャがそのクラス、構造体、またはインターフェイス内で一意である場合に、同じ名前の複数のメソッドを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-364">Overloading of methods permits a class, struct, or interface to declare multiple methods with the same name, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="8672b-365">インスタンスコンストラクターのオーバーロードにより、クラスまたは構造体は、そのクラスまたは構造体内で一意のシグネチャを持つ場合に、複数のインスタンスコンストラクターを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-365">Overloading of instance constructors permits a class or struct to declare multiple instance constructors, provided their signatures are unique within that class or struct.</span></span>
*  <span data-ttu-id="8672b-366">インデクサーをオーバーロードすると、クラス、構造体、またはインターフェイスは、そのクラス、構造体、またはインターフェイス内で一意のシグネチャを持つことができるので、複数のインデクサーを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-366">Overloading of indexers permits a class, struct, or interface to declare multiple indexers, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="8672b-367">演算子のオーバーロードにより、クラスまたは構造体は、そのクラスまたは構造体内で一意のシグネチャを持つ場合、同じ名前の複数の演算子を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-367">Overloading of operators permits a class or struct to declare multiple operators with the same name, provided their signatures are unique within that class or struct.</span></span>

<span data-ttu-id="8672b-368">`out` および `ref` パラメーター修飾子はシグネチャの一部と見なされますが、1つの型で宣言されたメンバーは、`ref` と `out`だけでシグネチャが異なることはありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-368">Although `out` and `ref` parameter modifiers are considered part of a signature, members declared in a single type cannot differ in signature solely by `ref` and `out`.</span></span> <span data-ttu-id="8672b-369">2つのメンバーが同じ型で宣言されている場合に、`out` 修飾子を持つ両方のメソッドのすべてのパラメーターが `ref` 修飾子に変更された場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-369">A compile-time error occurs if two members are declared in the same type with signatures that would be the same if all parameters in both methods with `out` modifiers were changed to `ref` modifiers.</span></span> <span data-ttu-id="8672b-370">署名の一致 (たとえば、非表示またはオーバーライド) のその他の目的では、`ref` と `out` はシグネチャの一部と見なされ、相互に一致しません。</span><span class="sxs-lookup"><span data-stu-id="8672b-370">For other purposes of signature matching (e.g., hiding or overriding), `ref` and `out` are considered part of the signature and do not match each other.</span></span> <span data-ttu-id="8672b-371">(この制限は、 C#プログラムを共通言語基盤 (CLI) で実行するために簡単に変換できるようにすることです。これにより、`ref` と `out`のみが異なるメソッドを定義することはできません)。</span><span class="sxs-lookup"><span data-stu-id="8672b-371">(This restriction is to allow C#  programs to be easily translated to run on the Common Language Infrastructure (CLI), which does not provide a way to define methods that differ solely in `ref` and `out`.)</span></span>

<span data-ttu-id="8672b-372">署名のために、型 `object` と `dynamic` は同じものと見なされます。</span><span class="sxs-lookup"><span data-stu-id="8672b-372">For the purposes of signatures, the types `object` and `dynamic` are considered the same.</span></span> <span data-ttu-id="8672b-373">したがって、1つの型で宣言されたメンバーは、`object` と `dynamic`だけでシグネチャが異なることがあります。</span><span class="sxs-lookup"><span data-stu-id="8672b-373">Members declared in a single type can therefore not differ in signature solely by `object` and `dynamic`.</span></span>

<span data-ttu-id="8672b-374">次の例は、オーバーロードされたメソッド宣言のセットとそのシグネチャを示しています。</span><span class="sxs-lookup"><span data-stu-id="8672b-374">The following example shows a set of overloaded method declarations along with their signatures.</span></span>
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

<span data-ttu-id="8672b-375">`ref` および `out` パラメーター修飾子 ([メソッドパラメーター](classes.md#method-parameters)) は、シグネチャの一部であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8672b-375">Note that any `ref` and `out` parameter modifiers ([Method parameters](classes.md#method-parameters)) are part of a signature.</span></span> <span data-ttu-id="8672b-376">したがって、`F(int)` と `F(ref int)` は一意の署名です。</span><span class="sxs-lookup"><span data-stu-id="8672b-376">Thus, `F(int)` and `F(ref int)` are unique signatures.</span></span> <span data-ttu-id="8672b-377">ただし、`F(ref int)` と `F(out int)` は、`ref` と `out`のみが異なるため、同じインターフェイス内で宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-377">However, `F(ref int)` and `F(out int)` cannot be declared within the same interface because their signatures differ solely by `ref` and `out`.</span></span> <span data-ttu-id="8672b-378">また、戻り値の型と `params` 修飾子はシグネチャの一部ではないことに注意してください。したがって、戻り値の型、または `params` 修飾子の包含または除外に基づいてのみオーバーロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-378">Also, note that the return type and the `params` modifier are not part of a signature, so it is not possible to overload solely based on return type or on the inclusion or exclusion of the `params` modifier.</span></span> <span data-ttu-id="8672b-379">そのため、上記で特定されたメソッド `F(int)` および `F(params string[])` の宣言では、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-379">As such, the declarations of the methods `F(int)` and `F(params string[])` identified above result in a compile-time error.</span></span>

## <a name="scopes"></a><span data-ttu-id="8672b-380">スコープ</span><span class="sxs-lookup"><span data-stu-id="8672b-380">Scopes</span></span>

<span data-ttu-id="8672b-381">名前の***スコープ***はプログラムテキストの領域であり、名前を修飾することなく、名前で宣言されたエンティティを参照できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-381">The ***scope*** of a name is the region of program text within which it is possible to refer to the entity declared by the name without qualification of the name.</span></span> <span data-ttu-id="8672b-382">スコープは***入れ子***にすることができます。また、内側のスコープは、外側のスコープから名前の意味を再宣言することができます (ただし、入れ子になったブロック内の[宣言](basic-concepts.md#declarations)によって課される制限はなく、外側のブロック内のローカル変数と同じ名前のローカル変数を宣言することはできません)。</span><span class="sxs-lookup"><span data-stu-id="8672b-382">Scopes can be ***nested***, and an inner scope may redeclare the meaning of a name from an outer scope (this does not, however, remove the restriction imposed by [Declarations](basic-concepts.md#declarations) that within a nested block it is not possible to declare a local variable with the same name as a local variable in an enclosing block).</span></span> <span data-ttu-id="8672b-383">外側のスコープの名前は、内側のスコープによってカバーされるプログラムテキストの領域で***非表示***になると言い、外部名へのアクセスは、名前を修飾することでのみ可能です。</span><span class="sxs-lookup"><span data-stu-id="8672b-383">The name from the outer scope is then said to be ***hidden*** in the region of program text covered by the inner scope, and access to the outer name is only possible by qualifying the name.</span></span>

*  <span data-ttu-id="8672b-384">*Namespace_declaration*を含まない*namespace_member_declaration* ([名前空間メンバー](namespaces.md#namespace-members)) によって宣言された名前空間メンバーのスコープは、プログラムテキスト全体です。</span><span class="sxs-lookup"><span data-stu-id="8672b-384">The scope of a namespace member declared by a *namespace_member_declaration* ([Namespace members](namespaces.md#namespace-members)) with no enclosing *namespace_declaration* is the entire program text.</span></span>
*  <span data-ttu-id="8672b-385">完全修飾名が `N` の*namespace_declaration*内の*namespace_member_declaration*によって宣言された名前空間メンバーのスコープは、完全修飾名が namespace_declaration であるか `N` で始まり、その後にピリオドが続くすべての *`N`* の*namespace_body*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-385">The scope of a namespace member declared by a *namespace_member_declaration* within a *namespace_declaration* whose fully qualified name is `N` is the *namespace_body* of every *namespace_declaration* whose fully qualified name is `N` or starts with `N`, followed by a period.</span></span>
*  <span data-ttu-id="8672b-386">*Extern_alias_directive*によって定義された名前のスコープは、その直後にコンパイル単位または名前空間の本体が含まれている*using_directive*s、 *global_attributes*および*namespace_member_declaration*を超えています。</span><span class="sxs-lookup"><span data-stu-id="8672b-386">The scope of name defined by an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="8672b-387">*Extern_alias_directive*は、基になる宣言領域に新しいメンバーを提供しません。</span><span class="sxs-lookup"><span data-stu-id="8672b-387">An *extern_alias_directive* does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="8672b-388">つまり、 *extern_alias_directive*は推移的ではありませんが、は、それが発生するコンパイル単位または名前空間の本文にのみ影響します。</span><span class="sxs-lookup"><span data-stu-id="8672b-388">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>
*  <span data-ttu-id="8672b-389">*Using_directive*によって定義またはインポートされた名前のスコープ ([ディレクティブを使用](namespaces.md#using-directives)) は、 *using_directive*が発生する*compilation_unit*または*namespace_body*の*namespace_member_declaration*を超えています。</span><span class="sxs-lookup"><span data-stu-id="8672b-389">The scope of a name defined or imported by a *using_directive* ([Using directives](namespaces.md#using-directives)) extends over the *namespace_member_declaration*s of the *compilation_unit* or *namespace_body* in which the *using_directive* occurs.</span></span> <span data-ttu-id="8672b-390">*Using_directive*は、0個以上の名前空間、型、またはメンバーの名前を特定の*compilation_unit*または*namespace_body*内で使用できるようにすることはできますが、基になる宣言領域に新しいメンバーを追加することはできません。</span><span class="sxs-lookup"><span data-stu-id="8672b-390">A *using_directive* may make zero or more namespace, type or member names available within a particular *compilation_unit* or *namespace_body*, but does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="8672b-391">つまり、 *using_directive*は推移的ではなく、それが発生する*compilation_unit*または*namespace_body*のみに影響します。</span><span class="sxs-lookup"><span data-stu-id="8672b-391">In other words, a *using_directive* is not transitive but rather affects only the *compilation_unit* or *namespace_body* in which it occurs.</span></span>
*  <span data-ttu-id="8672b-392">*Class_declaration* ([クラス宣言](classes.md#class-declarations)) の*type_parameter_list*によって宣言された型パラメーターのスコープは、その*class_body*の*class_base*、 *type_parameter_constraints_clause*s、および*class_declaration*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-392">The scope of a type parameter declared by a *type_parameter_list* on a *class_declaration* ([Class declarations](classes.md#class-declarations)) is the *class_base*, *type_parameter_constraints_clause*s, and *class_body* of that *class_declaration*.</span></span>
*  <span data-ttu-id="8672b-393">*Struct_declaration* ([構造体宣言](structs.md#struct-declarations)) の*type_parameter_list*によって宣言された型パラメーターのスコープは、その*struct_body*の*struct_interfaces*、 *type_parameter_constraints_clause*s、および*struct_declaration*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-393">The scope of a type parameter declared by a *type_parameter_list* on a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)) is the *struct_interfaces*, *type_parameter_constraints_clause*s, and *struct_body* of that *struct_declaration*.</span></span>
*  <span data-ttu-id="8672b-394">*Interface_declaration* ([インターフェイス宣言](interfaces.md#interface-declarations)) の*type_parameter_list*によって宣言された型パラメーターのスコープは、その*interface_body*の*interface_base*、 *type_parameter_constraints_clause*s、および*interface_declaration*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-394">The scope of a type parameter declared by a *type_parameter_list* on an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)) is the *interface_base*, *type_parameter_constraints_clause*s, and *interface_body* of that *interface_declaration*.</span></span>
*  <span data-ttu-id="8672b-395">*Delegate_declaration* ([デリゲート宣言](delegates.md#delegate-declarations)) の*type_parameter_list*によって宣言された型パラメーターのスコープは、その*type_parameter_constraints_clause*の*return_type*、 *formal_parameter_list*、および*delegate_declaration*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-395">The scope of a type parameter declared by a *type_parameter_list* on a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)) is the *return_type*, *formal_parameter_list*, and *type_parameter_constraints_clause*s of that *delegate_declaration*.</span></span>
*  <span data-ttu-id="8672b-396">*Class_member_declaration* ([クラス本体](classes.md#class-body)) によって宣言されるメンバーのスコープは、宣言が行われる*class_body*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-396">The scope of a member declared by a *class_member_declaration* ([Class body](classes.md#class-body)) is the *class_body* in which the declaration occurs.</span></span> <span data-ttu-id="8672b-397">さらに、クラスメンバーのスコープは、メンバーのアクセシビリティドメイン ([アクセシビリティ](basic-concepts.md#accessibility-domains)ドメイン) に含まれている派生クラスの*class_body*まで拡張されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-397">In addition, the scope of a class member extends to the *class_body* of those derived classes that are included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>
*  <span data-ttu-id="8672b-398">*Struct_member_declaration* ([構造体メンバー](structs.md#struct-members)) によって宣言されるメンバーのスコープは、宣言が行われる*struct_body*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-398">The scope of a member declared by a *struct_member_declaration* ([Struct members](structs.md#struct-members)) is the *struct_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="8672b-399">*Enum_member_declaration* ([列挙型メンバー](enums.md#enum-members)) によって宣言されたメンバーのスコープは、宣言が行われる*enum_body*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-399">The scope of a member declared by an *enum_member_declaration*  ([Enum members](enums.md#enum-members)) is the *enum_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="8672b-400">*Method_declaration* ([メソッド](classes.md#methods)) で宣言されたパラメーターのスコープは、その*method_declaration*の*method_body*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-400">The scope of a parameter declared in a *method_declaration* ([Methods](classes.md#methods)) is the *method_body* of that *method_declaration*.</span></span>
*  <span data-ttu-id="8672b-401">*Indexer_declaration* ([インデクサー](classes.md#indexers)) で宣言されたパラメーターのスコープは、その*indexer_declaration*の*accessor_declarations*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-401">The scope of a parameter declared in an *indexer_declaration* ([Indexers](classes.md#indexers)) is the *accessor_declarations* of that *indexer_declaration*.</span></span>
*  <span data-ttu-id="8672b-402">*Operator_declaration* ([Operators](classes.md#operators)) で宣言されたパラメーターのスコープは、その*operator_declaration*の*ブロック*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-402">The scope of a parameter declared in an *operator_declaration* ([Operators](classes.md#operators)) is the *block* of that *operator_declaration*.</span></span>
*  <span data-ttu-id="8672b-403">*Constructor_declaration* ([インスタンスコンストラクター](classes.md#instance-constructors)) で宣言されたパラメーターのスコープは、その*constructor_declaration*の*constructor_initializer*および*ブロック*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-403">The scope of a parameter declared in a *constructor_declaration* ([Instance constructors](classes.md#instance-constructors)) is the *constructor_initializer* and *block* of that *constructor_declaration*.</span></span>
*  <span data-ttu-id="8672b-404">*Lambda_expression* ([匿名関数式](expressions.md#anonymous-function-expressions)) で宣言されたパラメーターのスコープは、その*anonymous_function_body*です*lambda_expression*</span><span class="sxs-lookup"><span data-stu-id="8672b-404">The scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression*</span></span>
*  <span data-ttu-id="8672b-405">*Anonymous_method_expression* ([匿名関数式](expressions.md#anonymous-function-expressions)) で宣言されたパラメーターのスコープは、その*anonymous_method_expression*の*ブロック*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-405">The scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>
*  <span data-ttu-id="8672b-406">*Labeled_statement* ([ラベル付きステートメント](statements.md#labeled-statements)) で宣言されたラベルのスコープは、宣言が発生する*ブロック*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-406">The scope of a label declared in a *labeled_statement* ([Labeled statements](statements.md#labeled-statements)) is the *block* in which the declaration occurs.</span></span>
*  <span data-ttu-id="8672b-407">*Local_variable_declaration* ([ローカル変数宣言](statements.md#local-variable-declarations)) で宣言されたローカル変数のスコープは、宣言が発生するブロックです。</span><span class="sxs-lookup"><span data-stu-id="8672b-407">The scope of a local variable declared in a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) is the block in which the declaration occurs.</span></span>
*  <span data-ttu-id="8672b-408">`switch` ステートメント ([switch ステートメント](statements.md#the-switch-statement)) の*switch_block*で宣言されたローカル変数のスコープは*switch_block*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-408">The scope of a local variable declared in a *switch_block* of a `switch` statement ([The switch statement](statements.md#the-switch-statement)) is the *switch_block*.</span></span>
*  <span data-ttu-id="8672b-409">`for` ステートメント ([for ステートメント](statements.md#the-for-statement)) の*for_initializer*で宣言されたローカル変数のスコープは、 *for_initializer*、 *for_condition*、 *for_iterator*、および `for` ステートメントの含まれている*ステートメント*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-409">The scope of a local variable declared in a *for_initializer* of a `for` statement ([The for statement](statements.md#the-for-statement)) is the *for_initializer*, the *for_condition*, the *for_iterator*, and the contained *statement* of the `for` statement.</span></span>
*  <span data-ttu-id="8672b-410">*Local_constant_declaration* ([ローカル定数宣言](statements.md#local-constant-declarations)) で宣言されたローカル定数のスコープは、宣言が発生するブロックです。</span><span class="sxs-lookup"><span data-stu-id="8672b-410">The scope of a local constant declared in a *local_constant_declaration* ([Local constant declarations](statements.md#local-constant-declarations)) is the block in which the declaration occurs.</span></span> <span data-ttu-id="8672b-411">*Constant_declarator*の前にあるテキストの位置でローカル定数を参照するコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="8672b-411">It is a compile-time error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span>
*  <span data-ttu-id="8672b-412">*Foreach_statement*、 *using_statement*、 *lock_statement*または*query_expression*の一部として宣言された変数のスコープは、指定されたコンストラクトの展開によって決まります。</span><span class="sxs-lookup"><span data-stu-id="8672b-412">The scope of a variable declared as part of a *foreach_statement*, *using_statement*, *lock_statement* or *query_expression* is determined by the expansion of the given construct.</span></span>

<span data-ttu-id="8672b-413">名前空間、クラス、構造体、または列挙型のメンバーのスコープ内では、メンバーの宣言の前にあるテキスト位置でメンバーを参照することができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-413">Within the scope of a namespace, class, struct, or enumeration member it is possible to refer to the member in a textual position that precedes the declaration of the member.</span></span> <span data-ttu-id="8672b-414">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8672b-414">For example</span></span>
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
<span data-ttu-id="8672b-415">ここでは、`F` が宣言される前に `i` を参照することが有効です。</span><span class="sxs-lookup"><span data-stu-id="8672b-415">Here, it is valid for `F` to refer to `i` before it is declared.</span></span>

<span data-ttu-id="8672b-416">ローカル変数のスコープ内では、ローカル変数の*local_variable_declarator*の前にあるテキスト位置でローカル変数を参照するコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-416">Within the scope of a local variable, it is a compile-time error to refer to the local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="8672b-417">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8672b-417">For example</span></span>
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

<span data-ttu-id="8672b-418">上記の `F` メソッドでは、`i` への最初の代入は、外側のスコープで宣言されたフィールドを参照しません。</span><span class="sxs-lookup"><span data-stu-id="8672b-418">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="8672b-419">代わりに、ローカル変数を参照するので、コンパイル時エラーが発生します。これは、変数の宣言の前にあるためです。</span><span class="sxs-lookup"><span data-stu-id="8672b-419">Rather, it refers to the local variable and it results in a compile-time error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="8672b-420">`G` メソッドでは、`j` の宣言の初期化子で `j` を使用することは、 *local_variable_declarator*の前に使用されていないため、有効です。</span><span class="sxs-lookup"><span data-stu-id="8672b-420">In the `G` method, the use of `j` in the initializer for the declaration of `j` is valid because the use does not precede the *local_variable_declarator*.</span></span> <span data-ttu-id="8672b-421">`H` メソッドでは、後続の*local_variable_declarator*は、同じ*local_variable_declaration*内の以前の*local_variable_declarator*で宣言されたローカル変数を正しく参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-421">In the `H` method, a subsequent *local_variable_declarator* correctly refers to a local variable declared in an earlier *local_variable_declarator* within the same *local_variable_declaration*.</span></span>

<span data-ttu-id="8672b-422">ローカル変数のスコープ規則は、式のコンテキストで使用される名前の意味が常にブロック内で同じであることを保証するように設計されています。</span><span class="sxs-lookup"><span data-stu-id="8672b-422">The scoping rules for local variables are designed to guarantee that the meaning of a name used in an expression context is always the same within a block.</span></span> <span data-ttu-id="8672b-423">ローカル変数のスコープがその宣言からブロックの末尾まで拡張される場合、上記の例では、最初の代入がインスタンス変数に割り当てられ、2番目の代入がローカル変数に代入されますが、これはおそらく次のようになります。ブロックのステートメントが後で再配置された場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-423">If the scope of a local variable were to extend only from its declaration to the end of the block, then in the example above, the first assignment would assign to the instance variable and the second assignment would assign to the local variable, possibly leading to compile-time errors if the statements of the block were later to be rearranged.</span></span>

<span data-ttu-id="8672b-424">ブロック内の名前の意味は、名前が使用されているコンテキストによって異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-424">The meaning of a name within a block may differ based on the context in which the name is used.</span></span> <span data-ttu-id="8672b-425">この例では、</span><span class="sxs-lookup"><span data-stu-id="8672b-425">In the example</span></span>
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
<span data-ttu-id="8672b-426">`A` 名前は、クラス `A`を参照するために、式のコンテキストで使用され、ローカル変数 `A` と型コンテキストで参照されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-426">the name `A` is used in an expression context to refer to the local variable `A` and in a type context to refer to the class `A`.</span></span>

### <a name="name-hiding"></a><span data-ttu-id="8672b-427">名前の非表示</span><span class="sxs-lookup"><span data-stu-id="8672b-427">Name hiding</span></span>

<span data-ttu-id="8672b-428">エンティティのスコープには、通常、エンティティの宣言空間よりも多くのプログラムテキストが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8672b-428">The scope of an entity typically encompasses more program text than the declaration space of the entity.</span></span> <span data-ttu-id="8672b-429">特に、エンティティのスコープには、同じ名前のエンティティを含む新しい宣言スペースを導入する宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-429">In particular, the scope of an entity may include declarations that introduce new declaration spaces containing entities of the same name.</span></span> <span data-ttu-id="8672b-430">このような宣言を行うと、元のエンティティが***非表示***になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-430">Such declarations cause the original entity to become ***hidden***.</span></span> <span data-ttu-id="8672b-431">逆に、エンティティは非表示になっている場合は***表示***されると言います。</span><span class="sxs-lookup"><span data-stu-id="8672b-431">Conversely, an entity is said to be ***visible*** when it is not hidden.</span></span>

<span data-ttu-id="8672b-432">名前の非表示は、スコープが入れ子になっている場合と、スコープが継承によって重複している場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-432">Name hiding occurs when scopes overlap through nesting and when scopes overlap through inheritance.</span></span> <span data-ttu-id="8672b-433">2種類の非表示の特性については、次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="8672b-433">The characteristics of the two types of hiding are described in the following sections.</span></span>

#### <a name="hiding-through-nesting"></a><span data-ttu-id="8672b-434">非表示 (入れ子による)</span><span class="sxs-lookup"><span data-stu-id="8672b-434">Hiding through nesting</span></span>

<span data-ttu-id="8672b-435">入れ子による名前の隠ぺいは、名前空間内に名前空間または型を入れ子にした結果として発生することがあります。これは、クラスまたは構造体で型を入れ子にした結果として、パラメーターとローカル変数の宣言の結果として発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-435">Name hiding through nesting can occur as a result of nesting namespaces or types within namespaces, as a result of nesting types within classes or structs, and as a result of parameter and local variable declarations.</span></span>

<span data-ttu-id="8672b-436">この例では、</span><span class="sxs-lookup"><span data-stu-id="8672b-436">In the example</span></span>
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
<span data-ttu-id="8672b-437">`F` メソッドでは、インスタンス変数 `i` はローカル変数 `i`によって非表示にされますが、`G` メソッド内では、`i` は引き続きインスタンス変数を参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-437">within the `F` method, the instance variable `i` is hidden by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

<span data-ttu-id="8672b-438">内部スコープ内の名前が外側のスコープ内の名前を非表示にすると、その名前のすべてのオーバーロードされた出現が非表示になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-438">When a name in an inner scope hides a name in an outer scope, it hides all overloaded occurrences of that name.</span></span> <span data-ttu-id="8672b-439">この例では、</span><span class="sxs-lookup"><span data-stu-id="8672b-439">In the example</span></span>
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
<span data-ttu-id="8672b-440">呼び出し `F(1)` は `Inner` で宣言された `F` を呼び出します。これは、`F` のすべての外部オカレンスが内部宣言によって非表示になるためです。</span><span class="sxs-lookup"><span data-stu-id="8672b-440">the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="8672b-441">同じ理由から、`F("Hello")` を呼び出すと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-441">For the same reason, the call `F("Hello")` results in a compile-time error.</span></span>

#### <a name="hiding-through-inheritance"></a><span data-ttu-id="8672b-442">隠ぺい (継承による)</span><span class="sxs-lookup"><span data-stu-id="8672b-442">Hiding through inheritance</span></span>

<span data-ttu-id="8672b-443">継承による名前の隠ぺいは、クラスまたは構造体が基底クラスから継承された名前を再宣言するときに発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-443">Name hiding through inheritance occurs when classes or structs redeclare names that were inherited from base classes.</span></span> <span data-ttu-id="8672b-444">この種類の名前の隠ぺいは、次のいずれかの形式になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-444">This type of name hiding takes one of the following forms:</span></span>

*  <span data-ttu-id="8672b-445">クラスまたは構造体で導入された定数、フィールド、プロパティ、イベント、または型は、同じ名前を持つすべての基底クラスメンバーを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="8672b-445">A constant, field, property, event, or type introduced in a class or struct hides all base class members with the same name.</span></span>
*  <span data-ttu-id="8672b-446">クラスまたは構造体で導入されたメソッドは、同じ名前を持つすべての非メソッド基底クラスメンバーと、同じシグネチャ (メソッド名とパラメーター数、修飾子、型) を持つすべての基底クラスメソッドを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="8672b-446">A method introduced in a class or struct hides all non-method base class members with the same name, and all base class methods with the same signature (method name and parameter count, modifiers, and types).</span></span>
*  <span data-ttu-id="8672b-447">クラスまたは構造体で導入されたインデクサーは、同じシグネチャ (パラメーター数および型) を持つすべての基底クラスインデクサーを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="8672b-447">An indexer introduced in a class or struct hides all base class indexers with the same signature (parameter count and types).</span></span>

<span data-ttu-id="8672b-448">演算子宣言 ([演算子](classes.md#operators)) を制御する規則により、派生クラスは、基底クラスで演算子と同じシグネチャを持つ演算子を宣言できなくなります。</span><span class="sxs-lookup"><span data-stu-id="8672b-448">The rules governing operator declarations ([Operators](classes.md#operators)) make it impossible for a derived class to declare an operator with the same signature as an operator in a base class.</span></span> <span data-ttu-id="8672b-449">したがって、演算子は相互に隠ぺいすることはありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-449">Thus, operators never hide one another.</span></span>

<span data-ttu-id="8672b-450">外側のスコープから名前を非表示にする場合とは対照的に、継承されたスコープからアクセス可能な名前を非表示にすると、警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-450">Contrary to hiding a name from an outer scope, hiding an accessible name from an inherited scope causes a warning to be reported.</span></span> <span data-ttu-id="8672b-451">この例では、</span><span class="sxs-lookup"><span data-stu-id="8672b-451">In the example</span></span>
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
<span data-ttu-id="8672b-452">`Derived` の `F` を宣言すると、警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-452">the declaration of `F` in `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="8672b-453">継承された名前を非表示にすることは、特にエラーではありません。これは、基本クラスの個別の進化を防ぐためです。</span><span class="sxs-lookup"><span data-stu-id="8672b-453">Hiding an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="8672b-454">たとえば、より新しいバージョンの `Base` では、以前のバージョンのクラスには存在しない `F` メソッドが導入されたため、上記の状況が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-454">For example, the above situation might have come about because a later version of `Base` introduced an `F` method that wasn't present in an earlier version of the class.</span></span> <span data-ttu-id="8672b-455">上記の状況でエラーが発生した場合、個別にバージョン管理されたクラスライブラリの基底クラスに変更が加えられると、派生クラスが無効になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-455">Had the above situation been an error, then any change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="8672b-456">継承された名前を非表示にすることによって発生する警告は、`new` 修飾子を使用することによって削除できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-456">The warning caused by hiding an inherited name can be eliminated through use of the `new` modifier:</span></span>
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

<span data-ttu-id="8672b-457">`new` 修飾子は `Derived` 内の `F` が "new" であり、継承されたメンバーを非表示にすることを意図していることを示します。</span><span class="sxs-lookup"><span data-stu-id="8672b-457">The `new` modifier indicates that the `F` in `Derived` is "new", and that it is indeed intended to hide the inherited member.</span></span>

<span data-ttu-id="8672b-458">新しいメンバーの宣言は、継承されたメンバーを新しいメンバーのスコープ内でのみ非表示にします。</span><span class="sxs-lookup"><span data-stu-id="8672b-458">A declaration of a new member hides an inherited member only within the scope of the new member.</span></span>

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

<span data-ttu-id="8672b-459">上の例では、`Derived` 内の `F` の宣言によって `Base`から継承された `F` が非表示になりますが、`F` の新しい `Derived` にはプライベートアクセスがあるため、そのスコープは `MoreDerived`には拡張されません。</span><span class="sxs-lookup"><span data-stu-id="8672b-459">In the example above, the declaration of `F` in `Derived` hides the `F` that was inherited from `Base`, but since the new `F` in `Derived` has private access, its scope does not extend to `MoreDerived`.</span></span> <span data-ttu-id="8672b-460">したがって、`MoreDerived.G` 内の `F()` 呼び出しは有効で、`Base.F`を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8672b-460">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span>

## <a name="namespace-and-type-names"></a><span data-ttu-id="8672b-461">名前空間と型名</span><span class="sxs-lookup"><span data-stu-id="8672b-461">Namespace and type names</span></span>

<span data-ttu-id="8672b-462">プログラム内のいくつかのコンテキストでは、 *namespace_name*または type_name を指定する必要があります。 C#</span><span class="sxs-lookup"><span data-stu-id="8672b-462">Several contexts in a C# program require a *namespace_name* or a *type_name* to be specified.</span></span>

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

<span data-ttu-id="8672b-463">*Namespace_name*は、名前空間を参照する*namespace_or_type_name*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-463">A *namespace_name* is a *namespace_or_type_name* that refers to a namespace.</span></span> <span data-ttu-id="8672b-464">以下で説明するように、 *namespace_name*の*namespace_or_type_name*は名前空間を参照する必要があり、それ以外の場合はコンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-464">Following resolution as described below, the *namespace_or_type_name* of a *namespace_name* must refer to a namespace, or otherwise a compile-time error occurs.</span></span> <span data-ttu-id="8672b-465">型[引数 (型引数)](types.md#type-arguments)を*namespace_name*に含めることはできません (型引数を持つことができるのは型だけです)。</span><span class="sxs-lookup"><span data-stu-id="8672b-465">No type arguments ([Type arguments](types.md#type-arguments)) can be present in a *namespace_name* (only types can have type arguments).</span></span>

<span data-ttu-id="8672b-466">*Type_name*は、型を参照する*namespace_or_type_name*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-466">A *type_name* is a *namespace_or_type_name* that refers to a type.</span></span> <span data-ttu-id="8672b-467">以下で説明するように、 *type_name*の*namespace_or_type_name*は型を参照する必要があり、それ以外の場合はコンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-467">Following resolution as described below, the *namespace_or_type_name* of a *type_name* must refer to a type, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="8672b-468">*Namespace_or_type_name*が修飾エイリアスメンバーである場合は、「[名前空間エイリアス修飾子](namespaces.md#namespace-alias-qualifiers)」で説明されているように意味があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-468">If the *namespace_or_type_name* is a qualified-alias-member its meaning is as described in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span> <span data-ttu-id="8672b-469">それ以外の場合、 *namespace_or_type_name*には次の4つの形式があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-469">Otherwise, a *namespace_or_type_name* has one of four forms:</span></span>

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

<span data-ttu-id="8672b-470">`I` が1つの識別子であり、`N` は*namespace_or_type_name*であり、`<A1, ..., Ak>` は省略可能な*type_argument_list*です。</span><span class="sxs-lookup"><span data-stu-id="8672b-470">where `I` is a single identifier, `N` is a *namespace_or_type_name* and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="8672b-471">*Type_argument_list*が指定されていない場合は、`k` を0にすることを検討してください。</span><span class="sxs-lookup"><span data-stu-id="8672b-471">When no *type_argument_list* is specified, consider `k` to be zero.</span></span>

<span data-ttu-id="8672b-472">*Namespace_or_type_name*の意味は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-472">The meaning of a *namespace_or_type_name* is determined as follows:</span></span>

*   <span data-ttu-id="8672b-473">*Namespace_or_type_name*の形式が `I` または `I<A1, ..., Ak>`の場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-473">If the *namespace_or_type_name* is of the form `I` or of the form `I<A1, ..., Ak>`:</span></span>
    * <span data-ttu-id="8672b-474">`K` がゼロで、 *namespace_or_type_name*がジェネリックメソッド宣言 ([メソッド](classes.md#methods)) 内に存在し、その宣言に `I`という名前の型パラメーター ([型](classes.md#type-parameters)パラメーター) が含まれている場合、 *namespace_or_type_name*はその型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-474">If `K` is zero and the *namespace_or_type_name* appears within a generic method declaration ([Methods](classes.md#methods)) and if that declaration includes a type parameter ([Type parameters](classes.md#type-parameters)) with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
    * <span data-ttu-id="8672b-475">それ以外の場合、 *namespace_or_type_name*が型宣言内に出現する場合は、各インスタンス型 `T` ([インスタンス型](classes.md#the-instance-type)) に対して、その型宣言のインスタンス型から開始し、外側の各クラスまたは構造体宣言 (存在する場合) のインスタンス型を続行します。</span><span class="sxs-lookup"><span data-stu-id="8672b-475">Otherwise, if the *namespace_or_type_name* appears within a type declaration, then for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of that type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
        * <span data-ttu-id="8672b-476">`K` がゼロで、`T` の宣言に `I`という名前の型パラメーターが含まれている場合、 *namespace_or_type_name*はその型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-476">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
        * <span data-ttu-id="8672b-477">それ以外の場合、 *namespace_or_type_name*が型宣言の本体に含まれていて、`T` またはその基本型のいずれかに、名前 `I` および `K`の型パラメーターを持つ入れ子になったアクセス可能な型が含まれている場合、 * * は、指定された型引数を使用して構築されたその型を参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-477">Otherwise, if the *namespace_or_type_name* appears within the body of the type declaration, and `T` or any of its base types contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="8672b-478">このような型が複数ある場合は、より多くの派生型で宣言された型が選択されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-478">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="8672b-479">非型メンバー (定数、フィールド、メソッド、プロパティ、インデクサー、演算子、インスタンスコンストラクター、デストラクター、および静的コンストラクター) と、型パラメーターの数が異なる型メンバーは、 *namespace_or_type_name*の意味を判断する際に無視されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8672b-479">Note that non-type members (constants, fields, methods, properties, indexers, operators, instance constructors, destructors, and static constructors) and type members with a different number of type parameters are ignored when determining the meaning of the *namespace_or_type_name*.</span></span>
    * <span data-ttu-id="8672b-480">前の手順が正常に完了しなかった場合は、各名前空間 `N`、 *namespace_or_type_name*が発生する名前空間から開始し、外側の名前空間 (存在する場合) を続けて、グローバル名前空間で終わるまで、次の手順はエンティティが見つかるまで評価されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-480">If the previous steps were unsuccessful then, for each namespace `N`, starting with the namespace in which the *namespace_or_type_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
        * <span data-ttu-id="8672b-481">`K` がゼロで、`I` が `N`内の名前空間の名前である場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-481">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
            * <span data-ttu-id="8672b-482">*Namespace_or_type_name*が発生した場所が `N` の名前空間宣言によって囲まれていて、名前空間に名前空間または型が関連付けられている*extern_alias_directive*または*using_alias_directive*が名前空間宣言に含まれている場合、 * `I`* はあいまいで、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-482">If the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="8672b-483">それ以外の場合、 *namespace_or_type_name*は `N`内の `I` という名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-483">Otherwise, the *namespace_or_type_name* refers to the namespace named `I` in `N`.</span></span>
        * <span data-ttu-id="8672b-484">それ以外の場合、`N` に名前 `I` と `K` 型パラメーターを持つアクセス可能な型が含まれている場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-484">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
            * <span data-ttu-id="8672b-485">`K` がゼロで、 *namespace_or_type_name*が発生した場所が `N` の名前空間宣言によって囲まれており、名前空間を名前空間または型に関連付ける*extern_alias_directive*または*using_alias_directive*が名前空間宣言に含まれている場合、 * `I`* はあいまいで、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-485">If `K` is zero and the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="8672b-486">それ以外の場合、 *namespace_or_type_name*は、指定された型引数を使用して構築された型を参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-486">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
        * <span data-ttu-id="8672b-487">それ以外の場合、 *namespace_or_type_name*が発生した場所が `N`の名前空間宣言によって囲まれている場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-487">Otherwise, if the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
            * <span data-ttu-id="8672b-488">`K` がゼロで、名前空間の宣言に、名前 `I` をインポートした名前空間または型に関連付ける*extern_alias_directive*または*using_alias_directive*が含まれている場合、 *namespace_or_type_name*はその名前空間または型を参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-488">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *namespace_or_type_name* refers to that namespace or type.</span></span>
            * <span data-ttu-id="8672b-489">それ以外の場合、 *using_namespace_directive*s および名前空間宣言の*using_alias_directive*によってインポートされた名前空間と型の宣言に、name `I` および `K`の型パラメーターを持つアクセス可能な型が1つだけ含まれる場合、 * * は、指定された型引数を使用して構築されたその型を参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-489">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain exactly one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
            * <span data-ttu-id="8672b-490">それ以外の場合、 *using_namespace_directive*s および名前空間宣言の*using_alias_directive*によってインポートされた名前空間と型の宣言に、名前 `I` および `K`の型パラメーターを持つアクセス可能な型が複数含まれていると、 * * があいまいになり、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-490">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain more than one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* is ambiguous and an error occurs.</span></span>
    * <span data-ttu-id="8672b-491">それ以外の場合、 *namespace_or_type_name*は定義されていないため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-491">Otherwise, the *namespace_or_type_name* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="8672b-492">それ以外の場合、 *namespace_or_type_name*はフォーム `N.I` または `N.I<A1, ..., Ak>`の形式になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-492">Otherwise, the *namespace_or_type_name* is of the form `N.I` or of the form `N.I<A1, ..., Ak>`.</span></span> <span data-ttu-id="8672b-493">`N` は、まず*namespace_or_type_name*として解決されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-493">`N` is first resolved as a *namespace_or_type_name*.</span></span> <span data-ttu-id="8672b-494">`N` の解決に失敗した場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-494">If the resolution of `N` is not successful, a compile-time error occurs.</span></span> <span data-ttu-id="8672b-495">それ以外の場合、`N.I` または `N.I<A1, ..., Ak>` は次のように解決されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-495">Otherwise, `N.I` or `N.I<A1, ..., Ak>` is resolved as follows:</span></span>
    * <span data-ttu-id="8672b-496">`K` がゼロで、`N` が名前空間を参照し、`N` が `I`名前の入れ子になった名前空間を含んでいる場合、 *namespace_or_type_name*はその入れ子になった名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-496">If `K` is zero and `N` refers to a namespace and `N` contains a nested namespace with name `I`, then the *namespace_or_type_name* refers to that nested namespace.</span></span>
    * <span data-ttu-id="8672b-497">それ以外の場合、`N` が名前空間を参照し、`N` 名前 `I` および `K`型パラメーターを持つアクセス可能な型が含まれている場合、 * * は、指定された型引数を使用して構築されたその型を参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-497">Otherwise, if `N` refers to a namespace and `N` contains an accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
    * <span data-ttu-id="8672b-498">それ以外の場合、`N` が (構築された) クラスまたは構造体型を参照し、`N` またはその基本クラスに、名前 `I` および `K`型パラメーターを持つ入れ子になったアクセス可能な型が含まれている場合、 * * は、指定された型引数を使用して構築された型を参照します。</span><span class="sxs-lookup"><span data-stu-id="8672b-498">Otherwise, if `N` refers to a (possibly constructed) class or struct type and `N` or any of its base classes contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="8672b-499">このような型が複数ある場合は、より多くの派生型で宣言された型が選択されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-499">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="8672b-500">`N.I` の意味が `N` の基底クラスの指定の解決の一部として決定される場合、`N` の直接の基底クラスは object ([基底クラス](classes.md#base-classes)) と見なされます。</span><span class="sxs-lookup"><span data-stu-id="8672b-500">Note that if the meaning of `N.I` is being determined as part of resolving the base class specification of `N` then the direct base class of `N` is considered to be object ([Base classes](classes.md#base-classes)).</span></span>
    * <span data-ttu-id="8672b-501">それ以外の場合、`N.I` は無効な*namespace_or_type_name*であり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8672b-501">Otherwise, `N.I` is an invalid *namespace_or_type_name*, and a compile-time error occurs.</span></span>

<span data-ttu-id="8672b-502">*Namespace_or_type_name*は、静的クラス ([静的クラス](classes.md#static-classes)) を参照することが許可されています。</span><span class="sxs-lookup"><span data-stu-id="8672b-502">A *namespace_or_type_name* is permitted to reference a static class ([Static classes](classes.md#static-classes)) only if</span></span>

*  <span data-ttu-id="8672b-503">*Namespace_or_type_name*は `T.I`フォームの*namespace_or_type_name*の `T` です。</span><span class="sxs-lookup"><span data-stu-id="8672b-503">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="8672b-504">*Namespace_or_type_name*は、`typeof(T)`フォームの*typeof_expression* ([引数リスト](expressions.md#argument-lists)1) の `T` です。</span><span class="sxs-lookup"><span data-stu-id="8672b-504">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

### <a name="fully-qualified-names"></a><span data-ttu-id="8672b-505">完全修飾名</span><span class="sxs-lookup"><span data-stu-id="8672b-505">Fully qualified names</span></span>

<span data-ttu-id="8672b-506">すべての名前空間と型には、すべての名前空間または型を一意に識別する***完全修飾名***があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-506">Every namespace and type has a ***fully qualified name***, which uniquely identifies the namespace or type amongst all others.</span></span> <span data-ttu-id="8672b-507">名前空間または型 `N` の完全修飾名は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-507">The fully qualified name of a namespace or type `N` is determined as follows:</span></span>

*  <span data-ttu-id="8672b-508">`N` がグローバル名前空間のメンバーである場合、その完全修飾名は `N`になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-508">If `N` is a member of the global namespace, its fully qualified name is `N`.</span></span>
*  <span data-ttu-id="8672b-509">それ以外の場合、その完全修飾名は `S.N`になります。 `S` は `N` が宣言されている名前空間または型の完全修飾名です。</span><span class="sxs-lookup"><span data-stu-id="8672b-509">Otherwise, its fully qualified name is `S.N`, where `S` is the fully qualified name of the namespace or type in which `N` is declared.</span></span>

<span data-ttu-id="8672b-510">つまり、`N` の完全修飾名は、グローバル名前空間から始まる、`N`につながる識別子の完全な階層パスです。</span><span class="sxs-lookup"><span data-stu-id="8672b-510">In other words, the fully qualified name of `N` is the complete hierarchical path of identifiers that lead to `N`, starting from the global namespace.</span></span> <span data-ttu-id="8672b-511">名前空間または型のすべてのメンバーは一意の名前を持つ必要があるため、名前空間または型の完全修飾名は常に一意であることに従います。</span><span class="sxs-lookup"><span data-stu-id="8672b-511">Because every member of a namespace or type must have a unique name, it follows that the fully qualified name of a namespace or type is always unique.</span></span>

<span data-ttu-id="8672b-512">次の例では、いくつかの名前空間と型の宣言と、それに関連付けられた完全修飾名を示します。</span><span class="sxs-lookup"><span data-stu-id="8672b-512">The example below shows several namespace and type declarations along with their associated fully qualified names.</span></span>
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a><span data-ttu-id="8672b-513">自動メモリ管理</span><span class="sxs-lookup"><span data-stu-id="8672b-513">Automatic memory management</span></span>

<span data-ttu-id="8672b-514">C#自動メモリ管理を使用します。これにより、開発者は、オブジェクトによって占有されるメモリを手動で割り当てたり解放したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-514">C# employs automatic memory management, which frees developers from manually allocating and freeing the memory occupied by objects.</span></span> <span data-ttu-id="8672b-515">自動メモリ管理ポリシーは、***ガベージコレクター***によって実装されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-515">Automatic memory management policies are implemented by a ***garbage collector***.</span></span> <span data-ttu-id="8672b-516">オブジェクトのメモリ管理ライフサイクルは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="8672b-516">The memory management life cycle of an object is as follows:</span></span>

1. <span data-ttu-id="8672b-517">オブジェクトが作成されると、メモリが割り当てられ、コンストラクターが実行され、オブジェクトがライブと見なされます。</span><span class="sxs-lookup"><span data-stu-id="8672b-517">When the object is created, memory is allocated for it, the constructor is run, and the object is considered live.</span></span>
2. <span data-ttu-id="8672b-518">オブジェクト (またはその一部) が、デストラクターを実行するのではなく、実行の継続によってアクセスできない場合、オブジェクトは使用されなくなったと見なされ、破棄の対象になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-518">If the object, or any part of it, cannot be accessed by any possible continuation of execution, other than the running of destructors, the object is considered no longer in use, and it becomes eligible for destruction.</span></span> <span data-ttu-id="8672b-519">C#コンパイラとガベージコレクターは、コードを分析して、オブジェクトへの参照を将来使用できるかどうかを判断することができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-519">The C# compiler and the garbage collector may choose to analyze code to determine which references to an object may be used in the future.</span></span> <span data-ttu-id="8672b-520">たとえば、スコープ内にあるローカル変数がオブジェクトへの既存の参照であるが、そのローカル変数が、プロシージャの現在の実行ポイントからの実行の継続で参照されていない場合、ガベージコレクターが発生する可能性があります (ただし、必須) オブジェクトは使用されなくなったものとして扱います。</span><span class="sxs-lookup"><span data-stu-id="8672b-520">For instance, if a local variable that is in scope is the only existing reference to an object, but that local variable is never referred to in any possible continuation of execution from the current execution point in the procedure, the garbage collector may (but is not required to) treat the object as no longer in use.</span></span>
3. <span data-ttu-id="8672b-521">オブジェクトが破棄されるようになると、指定されていない場合は、そのオブジェクトのデストラクター ([デストラクター](classes.md#destructors)) が実行されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-521">Once the object is eligible for destruction, at some unspecified later time the destructor ([Destructors](classes.md#destructors)) (if any) for the object is run.</span></span> <span data-ttu-id="8672b-522">通常の状況下では、オブジェクトのデストラクターが1回だけ実行されます。ただし、実装固有の Api では、この動作がオーバーライドされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-522">Under normal circumstances the destructor for the object is run once only, though implementation-specific APIs may allow this behavior to be overridden.</span></span>
4. <span data-ttu-id="8672b-523">オブジェクトのデストラクターが実行されると、そのオブジェクト、またはその一部が、デストラクターの実行など、実行の継続によってアクセスできなくなると、オブジェクトはアクセス不可能と見なされ、オブジェクトがコレクションの対象になります。</span><span class="sxs-lookup"><span data-stu-id="8672b-523">Once the destructor for an object is run, if that object, or any part of it, cannot be accessed by any possible continuation of execution, including the running of destructors, the object is considered inaccessible and the object becomes eligible for collection.</span></span>
5. <span data-ttu-id="8672b-524">最後に、オブジェクトがコレクションの対象になると、そのオブジェクトに関連付けられているメモリがガベージコレクターによって解放されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-524">Finally, at some time after the object becomes eligible for collection, the garbage collector frees the memory associated with that object.</span></span>

<span data-ttu-id="8672b-525">ガベージコレクターは、オブジェクトの使用状況に関する情報を保持し、この情報を使用してメモリ管理の決定を行います。たとえば、新しく作成されたオブジェクトをメモリ内に配置する場所、オブジェクトを再配置するタイミング、オブジェクトが使用されていない場合やアクセスできない場合などです。</span><span class="sxs-lookup"><span data-stu-id="8672b-525">The garbage collector maintains information about object usage, and uses this information to make memory management decisions, such as where in memory to locate a newly created object, when to relocate an object, and when an object is no longer in use or inaccessible.</span></span>

<span data-ttu-id="8672b-526">ガベージコレクターの存在を想定している他の言語C#と同様に、は、ガベージコレクターが幅広いメモリ管理ポリシーを実装できるように設計されています。</span><span class="sxs-lookup"><span data-stu-id="8672b-526">Like other languages that assume the existence of a garbage collector, C# is designed so that the garbage collector may implement a wide range of memory management policies.</span></span> <span data-ttu-id="8672b-527">たとえば、でC#は、デストラクターを実行する必要はありません。また、オブジェクトが適格であるか、またはそのデストラクターが特定の順序または特定のスレッドで実行されることもありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-527">For instance, C# does not require that destructors be run or that objects be collected as soon as they are eligible, or that destructors be run in any particular order, or on any particular thread.</span></span>

<span data-ttu-id="8672b-528">ガベージコレクターの動作は、クラス `System.GC`の静的メソッドを使用して、ある程度制御できます。</span><span class="sxs-lookup"><span data-stu-id="8672b-528">The behavior of the garbage collector can be controlled, to some degree, via static methods on the class `System.GC`.</span></span> <span data-ttu-id="8672b-529">このクラスを使用して、コレクションの発生を要求したり、デストラクターを実行 (または実行しない) したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="8672b-529">This class can be used to request a collection to occur, destructors to be run (or not run), and so forth.</span></span>

<span data-ttu-id="8672b-530">ガベージコレクターでは、オブジェクトを収集してデストラクターを実行するタイミングを決定する際に、ワイド緯度が許可されるため、次のコードに示されているものとは異なる出力が、準拠する実装によって生成される場合があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-530">Since the garbage collector is allowed wide latitude in deciding when to collect objects and run destructors, a conforming implementation may produce output that differs from that shown by the following code.</span></span> <span data-ttu-id="8672b-531">プログラム</span><span class="sxs-lookup"><span data-stu-id="8672b-531">The program</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
<span data-ttu-id="8672b-532">クラス `A` のインスタンスと `B`クラスのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="8672b-532">creates an instance of class `A` and an instance of class `B`.</span></span> <span data-ttu-id="8672b-533">変数 `b` に値 `null`が割り当てられている場合、これらのオブジェクトはガベージコレクションの対象になります。この時間が経過すると、ユーザーが記述したコードにアクセスできなくなるためです。</span><span class="sxs-lookup"><span data-stu-id="8672b-533">These objects become eligible for garbage collection when the variable `b` is assigned the value `null`, since after this time it is impossible for any user-written code to access them.</span></span> <span data-ttu-id="8672b-534">出力は、</span><span class="sxs-lookup"><span data-stu-id="8672b-534">The output could be either</span></span>

```console
Destruct instance of A
Destruct instance of B
```
<span data-ttu-id="8672b-535">、または</span><span class="sxs-lookup"><span data-stu-id="8672b-535">or</span></span>
```console
Destruct instance of B
Destruct instance of A
```
<span data-ttu-id="8672b-536">この言語では、オブジェクトがガベージコレクションされる順序に制約はありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-536">because the language imposes no constraints on the order in which objects are garbage collected.</span></span>

<span data-ttu-id="8672b-537">軽微なケースとして、"破棄の対象" と "コレクションの対象" の区別が重要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="8672b-537">In subtle cases, the distinction between "eligible for destruction" and "eligible for collection" can be important.</span></span> <span data-ttu-id="8672b-538">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8672b-538">For example,</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

<span data-ttu-id="8672b-539">上記のプログラムでは、ガベージコレクターが `B`のデストラクターの前に `A` のデストラクターを実行するように選択した場合、このプログラムの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-539">In the above program, if the garbage collector chooses to run the destructor of `A` before the destructor of `B`, then the output of this program might be:</span></span>
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

<span data-ttu-id="8672b-540">`A` のインスタンスは使用されておらず、`A`のデストラクターが実行されていましたが、`A` (この場合は `F`) のメソッドが別のデストラクターから呼び出される可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8672b-540">Note that although the instance of `A` was not in use and `A`'s destructor was run, it is still possible for methods of `A` (in this case, `F`) to be called from another destructor.</span></span> <span data-ttu-id="8672b-541">また、デストラクターを実行すると、メインラインプログラムからオブジェクトが再び使用できるようになる可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8672b-541">Also, note that running of a destructor may cause an object to become usable from the mainline program again.</span></span> <span data-ttu-id="8672b-542">この場合、`B`のデストラクターの実行により、以前に使用されていなかった `A` のインスタンスがライブ参照 `Test.RefA`からアクセスできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="8672b-542">In this case, the running of `B`'s destructor caused an instance of `A` that was previously not in use to become accessible from the live reference `Test.RefA`.</span></span> <span data-ttu-id="8672b-543">`WaitForPendingFinalizers`の呼び出しの後、`B` のインスタンスはコレクションの対象になりますが、`A` のインスタンスは、参照 `Test.RefA`によっては使用できません。</span><span class="sxs-lookup"><span data-stu-id="8672b-543">After the call to `WaitForPendingFinalizers`, the instance of `B` is eligible for collection, but the instance of `A` is not, because of the reference `Test.RefA`.</span></span>

<span data-ttu-id="8672b-544">混乱や予期しない動作を避けるため、一般に、デストラクターは、オブジェクトの独自のフィールドに格納されているデータに対してのみクリーンアップを実行し、参照先のオブジェクトや静的フィールドに対してアクションを実行しないようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8672b-544">To avoid confusion and unexpected behavior, it is generally a good idea for destructors to only perform cleanup on data stored in their object's own fields, and not to perform any actions on referenced objects or static fields.</span></span>

<span data-ttu-id="8672b-545">デストラクターを使用する代わりに、クラスが `System.IDisposable` インターフェイスを実装できるようにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="8672b-545">An alternative to using destructors is to let a class implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="8672b-546">これにより、オブジェクトのクライアントは、オブジェクトのリソースを解放するタイミングを決定できます。通常は、オブジェクトに `using` ステートメント ([using ステートメント](statements.md#the-using-statement)) のリソースとしてアクセスします。</span><span class="sxs-lookup"><span data-stu-id="8672b-546">This allows the client of the object to determine when to release the resources of the object, typically by accessing the object as a resource in a `using` statement ([The using statement](statements.md#the-using-statement)).</span></span>

## <a name="execution-order"></a><span data-ttu-id="8672b-547">実行順序</span><span class="sxs-lookup"><span data-stu-id="8672b-547">Execution order</span></span>

<span data-ttu-id="8672b-548">C#プログラムの実行は、実行中の各スレッドの副作用が重要な実行ポイントで保持されるようになります。</span><span class="sxs-lookup"><span data-stu-id="8672b-548">Execution of a C# program proceeds such that the side effects of each executing thread are preserved at critical execution points.</span></span> <span data-ttu-id="8672b-549">***副作用***は、volatile フィールドの読み取りまたは書き込み、不揮発性の変数への書き込み、外部リソースへの書き込み、および例外のスローとして定義されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-549">A ***side effect*** is defined as a read or write of a volatile field, a write to a non-volatile variable, a write to an external resource, and the throwing of an exception.</span></span> <span data-ttu-id="8672b-550">これらの副作用の順序を保持する必要がある重要な実行ポイントは、volatile フィールド ([volatile フィールド](classes.md#volatile-fields))、`lock` ステートメント ([lock ステートメント](statements.md#the-lock-statement))、およびスレッドの作成と終了を参照することです。</span><span class="sxs-lookup"><span data-stu-id="8672b-550">The critical execution points at which the order of these side effects must be preserved are references to volatile fields ([Volatile fields](classes.md#volatile-fields)), `lock` statements ([The lock statement](statements.md#the-lock-statement)), and thread creation and termination.</span></span> <span data-ttu-id="8672b-551">実行環境では、 C#プログラムの実行順序を自由に変更できます。次の制約が適用されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-551">The execution environment is free to change the order of execution of a C# program, subject to the following constraints:</span></span>

*  <span data-ttu-id="8672b-552">データ依存は、実行スレッド内で保持されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-552">Data dependence is preserved within a thread of execution.</span></span> <span data-ttu-id="8672b-553">つまり、各変数の値は、スレッド内のすべてのステートメントが元のプログラムの順序で実行されたかのように計算されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-553">That is, the value of each variable is computed as if all statements in the thread were executed in original program order.</span></span>
*  <span data-ttu-id="8672b-554">初期化の順序付け規則が保持されます ([フィールド初期化](classes.md#field-initialization)と[変数初期化子](classes.md#variable-initializers))。</span><span class="sxs-lookup"><span data-stu-id="8672b-554">Initialization ordering rules are preserved ([Field initialization](classes.md#field-initialization) and [Variable initializers](classes.md#variable-initializers)).</span></span>
*  <span data-ttu-id="8672b-555">副作用の順序は、揮発性の読み取りと書き込み ([volatile フィールド](classes.md#volatile-fields)) に関して保持されます。</span><span class="sxs-lookup"><span data-stu-id="8672b-555">The ordering of side effects is preserved with respect to volatile reads and writes ([Volatile fields](classes.md#volatile-fields)).</span></span> <span data-ttu-id="8672b-556">さらに、式の値が使用されていないことを推測し、必要な副作用が生成されない場合 (メソッドの呼び出しや volatile フィールドへのアクセスによって発生する問題を含む)、実行環境は式の一部を評価する必要がありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-556">Additionally, the execution environment need not evaluate part of an expression if it can deduce that that expression's value is not used and that no needed side effects are produced (including any caused by calling a method or accessing a volatile field).</span></span> <span data-ttu-id="8672b-557">非同期イベント (別のスレッドによってスローされた例外など) によってプログラムの実行が中断された場合、観測可能な副作用が元のプログラムの順序で表示される保証はありません。</span><span class="sxs-lookup"><span data-stu-id="8672b-557">When program execution is interrupted by an asynchronous event (such as an exception thrown by another thread), it is not guaranteed that the observable side effects are visible in the original program order.</span></span>
