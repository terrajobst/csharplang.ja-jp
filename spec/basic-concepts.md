---
ms.openlocfilehash: 1c3d05674f8f7b69e70e0d9e06021537fc45f7ed
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229806"
---
# <a name="basic-concepts"></a><span data-ttu-id="378d4-101">基本的な概念</span><span class="sxs-lookup"><span data-stu-id="378d4-101">Basic concepts</span></span>

## <a name="application-startup"></a><span data-ttu-id="378d4-102">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="378d4-102">Application Startup</span></span>

<span data-ttu-id="378d4-103">アセンブリが、***エントリ ポイント***と呼ばれますが、***アプリケーション***します。</span><span class="sxs-lookup"><span data-stu-id="378d4-103">An assembly that has an ***entry point*** is called an ***application***.</span></span> <span data-ttu-id="378d4-104">ときに、アプリケーションは、実行、新しい***アプリケーション ドメイン***が作成されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-104">When an application is run, a new ***application domain*** is created.</span></span> <span data-ttu-id="378d4-105">同時に、アプリケーションのいくつかの異なるインスタンス化が同じコンピューターに存在して、それぞれが独自のアプリケーション ドメイン。</span><span class="sxs-lookup"><span data-stu-id="378d4-105">Several different instantiations of an application may exist on the same machine at the same time, and each has its own application domain.</span></span>

<span data-ttu-id="378d4-106">アプリケーション ドメインでは、アプリケーションの状態のコンテナーとして機能するによって、アプリケーション分離を実現します。</span><span class="sxs-lookup"><span data-stu-id="378d4-106">An application domain enables application isolation by acting as a container for application state.</span></span> <span data-ttu-id="378d4-107">アプリケーション ドメインは、コンテナーと、アプリケーションと、使用するクラス ライブラリで定義された型の境界として機能します。</span><span class="sxs-lookup"><span data-stu-id="378d4-107">An application domain acts as a container and boundary for the types defined in the application and the class libraries it uses.</span></span> <span data-ttu-id="378d4-108">1 つのアプリケーション ドメインに読み込まれた型は別のアプリケーション ドメインに読み込まれる同じ種類の異なると、オブジェクトのインスタンスは直接のアプリケーション ドメイン間で共有されません。</span><span class="sxs-lookup"><span data-stu-id="378d4-108">Types loaded into one application domain are distinct from the same type loaded into another application domain, and instances of objects are not directly shared between application domains.</span></span> <span data-ttu-id="378d4-109">たとえば、各アプリケーション ドメインは、これらの型の静的変数の独自のコピーと、型の静的コンス トラクターはアプリケーション ドメインごとに最大 1 回実行します。</span><span class="sxs-lookup"><span data-stu-id="378d4-109">For instance, each application domain has its own copy of static variables for these types, and a static constructor for a type is run at most once per application domain.</span></span> <span data-ttu-id="378d4-110">実装では、作成とアプリケーション ドメインの破棄の実装に固有のポリシーまたはメカニズムを提供する無料です。</span><span class="sxs-lookup"><span data-stu-id="378d4-110">Implementations are free to provide implementation-specific policy or mechanisms for the creation and destruction of application domains.</span></span>

<span data-ttu-id="378d4-111">***アプリケーションの起動***実行環境がアプリケーションのエントリ ポイントと呼ばれる、指定されたメソッドを呼び出すときに発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-111">***Application startup*** occurs when the execution environment calls a designated method, which is referred to as the application's entry point.</span></span> <span data-ttu-id="378d4-112">このエントリ ポイント メソッドの名前は常に`Main`、次のシグネチャのいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="378d4-112">This entry point method is always named `Main`, and can have one of the following signatures:</span></span>

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

<span data-ttu-id="378d4-113">エントリ ポイントが必要に応じて返す可能性がありますに示すように、`int`値。</span><span class="sxs-lookup"><span data-stu-id="378d4-113">As shown, the entry point may optionally return an `int` value.</span></span> <span data-ttu-id="378d4-114">これでアプリケーションの終了値が使用されますが返されます ([アプリケーションの終了](basic-concepts.md#application-termination))。</span><span class="sxs-lookup"><span data-stu-id="378d4-114">This return value is used in application termination ([Application termination](basic-concepts.md#application-termination)).</span></span>

<span data-ttu-id="378d4-115">エントリ ポイントは、1 つの仮パラメーターを必要に応じてがあります。</span><span class="sxs-lookup"><span data-stu-id="378d4-115">The entry point may optionally have one formal parameter.</span></span> <span data-ttu-id="378d4-116">パラメーターは、任意の名前を必要がありますが、パラメーターの型である必要があります`string[]`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-116">The parameter may have any name, but the type of the parameter must be `string[]`.</span></span> <span data-ttu-id="378d4-117">実行環境を作成し、渡します仮パラメーターが存在する場合、`string[]`されたコマンドライン引数を含む引数は、アプリケーションが開始された日時を指定します。</span><span class="sxs-lookup"><span data-stu-id="378d4-117">If the formal parameter is present, the execution environment creates and passes a `string[]` argument containing the command-line arguments that were specified when the application was started.</span></span> <span data-ttu-id="378d4-118">`string[]`引数が null ではありませんが、コマンドライン引数が指定されていない場合、長さ 0 の可能性があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-118">The `string[]` argument is never null, but it may have a length of zero if no command-line arguments were specified.</span></span>

<span data-ttu-id="378d4-119">C# メソッドのオーバー ロードをサポートするためクラスまたは構造体を含めることはいくつかのメソッドの複数の定義それぞれがある異なるシグネチャで提供されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-119">Since C# supports method overloading, a class or struct may contain multiple definitions of some method, provided each has a different signature.</span></span> <span data-ttu-id="378d4-120">ただし、1 つのプログラム内でクラスまたは構造体ことないという 1 つ以上のメソッドが`Main`の定義が適用されるアプリケーションのエントリ ポイントとして使用されるようにします。</span><span class="sxs-lookup"><span data-stu-id="378d4-120">However, within a single program, no class or struct may contain more than one method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="378d4-121">他のオーバー ロードされたバージョンの`Main`は許可されて、ただし、1 つ以上のパラメーター、または型以外の唯一のパラメーターは、提供される`string[]`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-121">Other overloaded versions of `Main` are permitted, however, provided they have more than one parameter, or their only parameter is other than type `string[]`.</span></span>

<span data-ttu-id="378d4-122">アプリケーションは、複数のクラスまたは構造体の構成できます。</span><span class="sxs-lookup"><span data-stu-id="378d4-122">An application can be made up of multiple classes or structs.</span></span> <span data-ttu-id="378d4-123">呼び出されるメソッドを格納するこれらのクラスまたは構造体の 1 つ以上のことは`Main`の定義が適用されるアプリケーションのエントリ ポイントとして使用されるようにします。</span><span class="sxs-lookup"><span data-stu-id="378d4-123">It is possible for more than one of these classes or structs to contain a method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="378d4-124">このような場合は、次のいずれかを選択する (コマンド ライン コンパイラ オプション) などの外部メカニズムを使用する必要があります`Main`エントリ ポイントとしてメソッド。</span><span class="sxs-lookup"><span data-stu-id="378d4-124">In such cases, an external mechanism (such as a command-line compiler option) must be used to select one of these `Main` methods as the entry point.</span></span>

<span data-ttu-id="378d4-125">C# でクラスまたは構造体のメンバーとしてすべてのメソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-125">In C#, every method must be defined as a member of a class or struct.</span></span> <span data-ttu-id="378d4-126">通常、宣言されたアクセシビリティ ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility)) メソッドのアクセス修飾子によって決定されます ([アクセス修飾子](classes.md#access-modifiers))、その宣言と同様に宣言で指定されました。型のアクセシビリティは、その宣言で指定されたアクセス修飾子によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-126">Ordinarily, the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of a method is determined by the access modifiers ([Access modifiers](classes.md#access-modifiers)) specified in its declaration, and similarly the declared accessibility of a type is determined by the access modifiers specified in its declaration.</span></span> <span data-ttu-id="378d4-127">指定された型の特定のメソッドを呼び出せるようにする、型とメンバーの両方必要がありますでアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="378d4-127">In order for a given method of a given type to be callable, both the type and the member must be accessible.</span></span> <span data-ttu-id="378d4-128">ただし、アプリケーションのエントリ ポイントは、特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="378d4-128">However, the application entry point is a special case.</span></span> <span data-ttu-id="378d4-129">具体的には、実行環境は、宣言されたアクセシビリティに関係なく、その外側の型宣言の宣言されたアクセシビリティに関係なく、アプリケーションのエントリ ポイントにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="378d4-129">Specifically, the execution environment can access the application's entry point regardless of its declared accessibility and regardless of the declared accessibility of its enclosing type declarations.</span></span>

<span data-ttu-id="378d4-130">アプリケーションのエントリ ポイント メソッドがジェネリック クラス宣言でできないがあります。</span><span class="sxs-lookup"><span data-stu-id="378d4-130">The application entry point method may not be in a generic class declaration.</span></span>

<span data-ttu-id="378d4-131">その他のすべての点は、エントリ ポイント メソッドは、エントリ ポイントではないよう動作します。</span><span class="sxs-lookup"><span data-stu-id="378d4-131">In all other respects, entry point methods behave like those that are not entry points.</span></span>

## <a name="application-termination"></a><span data-ttu-id="378d4-132">アプリケーションの終了</span><span class="sxs-lookup"><span data-stu-id="378d4-132">Application termination</span></span>

<span data-ttu-id="378d4-133">***アプリケーションの終了***実行環境にコントロールを返します。</span><span class="sxs-lookup"><span data-stu-id="378d4-133">***Application termination*** returns control to the execution environment.</span></span>

<span data-ttu-id="378d4-134">戻り値の型のアプリケーションの場合***エントリ ポイント***メソッドは`int`、返される値は、アプリケーションの***終了ステータス コード***します。</span><span class="sxs-lookup"><span data-stu-id="378d4-134">If the return type of the application's ***entry point*** method is `int`, the value returned serves as the application's ***termination status code***.</span></span> <span data-ttu-id="378d4-135">このコードでは、成功または失敗、実行時環境への通信を許可します。</span><span class="sxs-lookup"><span data-stu-id="378d4-135">The purpose of this code is to allow communication of success or failure to the execution environment.</span></span>

<span data-ttu-id="378d4-136">エントリ ポイント メソッドの戻り値の型がある場合`void`、右中かっこに到達 (`}`) を終了するメソッド、またはを実行する、`return`式を持たないステートメントの結果の終了ステータス コード`0`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-136">If the return type of the entry point method is `void`, reaching the right brace (`}`) which terminates that method, or executing a `return` statement that has no expression, results in a termination status code of `0`.</span></span>

<span data-ttu-id="378d4-137">このようなクリーンアップが中止された場合を除き、アプリケーションの終了前にすべてのガベージ コレクションをまだしていないオブジェクトのデストラクターが呼び出されます (ライブラリのメソッドを呼び出して`GC.SuppressFinalize`など)。</span><span class="sxs-lookup"><span data-stu-id="378d4-137">Prior to an application's termination, destructors for all of its objects that have not yet been garbage collected are called, unless such cleanup has been suppressed (by a call to the library method `GC.SuppressFinalize`, for example).</span></span>

## <a name="declarations"></a><span data-ttu-id="378d4-138">宣言</span><span class="sxs-lookup"><span data-stu-id="378d4-138">Declarations</span></span>

<span data-ttu-id="378d4-139">C# プログラム内の宣言では、プログラムの構成要素を定義します。</span><span class="sxs-lookup"><span data-stu-id="378d4-139">Declarations in a C# program define the constituent elements of the program.</span></span> <span data-ttu-id="378d4-140">名前空間を使用して C# プログラムの構成 ([名前空間](namespaces.md))、型を含めることができる宣言と入れ子になった名前空間を宣言します。</span><span class="sxs-lookup"><span data-stu-id="378d4-140">C# programs are organized using namespaces ([Namespaces](namespaces.md)), which can contain type declarations and nested namespace declarations.</span></span> <span data-ttu-id="378d4-141">型宣言 ([入力宣言](namespaces.md#type-declarations)) クラスを定義するために使用されます ([クラス](classes.md))、構造体 ([構造体](structs.md))、インターフェイス ([インターフェイス](interfaces.md))、列挙型 ([列挙型](enums.md))、およびデリゲート ([デリゲート](delegates.md))。</span><span class="sxs-lookup"><span data-stu-id="378d4-141">Type declarations ([Type declarations](namespaces.md#type-declarations)) are used to define classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md)), enums ([Enums](enums.md)), and delegates ([Delegates](delegates.md)).</span></span> <span data-ttu-id="378d4-142">型の宣言でメンバーの種類は、型宣言の形式に依存します。</span><span class="sxs-lookup"><span data-stu-id="378d4-142">The kinds of members permitted in a type declaration depend on the form of the type declaration.</span></span> <span data-ttu-id="378d4-143">たとえば、クラス宣言が定数の宣言を含めることができます ([定数](classes.md#constants))、フィールド ([フィールド](classes.md#fields))、メソッド ([メソッド](classes.md#methods))、プロパティ ([プロパティ](classes.md#properties))、イベント ([イベント](classes.md#events))、インデクサー ([インデクサー](classes.md#indexers))、演算子 ([演算子](classes.md#operators))、インスタンス コンス トラクター ([インスタンス コンス トラクター](classes.md#instance-constructors))、静的コンス トラクター ([静的コンス トラクター](classes.md#static-constructors))、デストラクター ([デストラクター](classes.md#destructors))、入れ子にされた型と ([入れ子になった型](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="378d4-143">For instance, class declarations can contain declarations for constants ([Constants](classes.md#constants)), fields ([Fields](classes.md#fields)), methods ([Methods](classes.md#methods)), properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), indexers ([Indexers](classes.md#indexers)), operators ([Operators](classes.md#operators)), instance constructors ([Instance constructors](classes.md#instance-constructors)), static constructors ([Static constructors](classes.md#static-constructors)), destructors ([Destructors](classes.md#destructors)), and nested types ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="378d4-144">宣言内の名前を定義する、***宣言領域***宣言が属しています。</span><span class="sxs-lookup"><span data-stu-id="378d4-144">A declaration defines a name in the ***declaration space*** to which the declaration belongs.</span></span> <span data-ttu-id="378d4-145">以外のメンバーはオーバー ロード ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading))、宣言領域内の同じ名前のメンバーを導入する 2 つ以上の宣言すると、コンパイル時エラーします。</span><span class="sxs-lookup"><span data-stu-id="378d4-145">Except for overloaded members ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)), it is a compile-time error to have two or more declarations that introduce members with the same name in a declaration space.</span></span> <span data-ttu-id="378d4-146">さまざまな種類の同じ名前を持つメンバーを格納する宣言領域のことはできません。</span><span class="sxs-lookup"><span data-stu-id="378d4-146">It is never possible for a declaration space to contain different kinds of members with the same name.</span></span> <span data-ttu-id="378d4-147">たとえば、宣言領域が含まれないことフィールドとメソッドを同じ名前。</span><span class="sxs-lookup"><span data-stu-id="378d4-147">For example, a declaration space can never contain a field and a method by the same name.</span></span>

<span data-ttu-id="378d4-148">さまざまな種類の宣言のスペースがある、次に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="378d4-148">There are several different types of declaration spaces, as described in the following.</span></span>

*  <span data-ttu-id="378d4-149">プログラムのすべてのソース ファイル内で*namespace_member_declaration*を囲むありません*namespace_declaration*という 1 つの結合された宣言領域のメンバーである、***グローバル宣言領域***します。</span><span class="sxs-lookup"><span data-stu-id="378d4-149">Within all source files of a program, *namespace_member_declaration*s with no enclosing *namespace_declaration* are members of a single combined declaration space called the ***global declaration space***.</span></span>
*  <span data-ttu-id="378d4-150">プログラムのすべてのソース ファイル内で*namespace_member_declaration*内*namespace_declaration*s 同じ完全修飾名前空間の名前を持つ 1 つの結合された宣言のメンバーであります。領域。</span><span class="sxs-lookup"><span data-stu-id="378d4-150">Within all source files of a program, *namespace_member_declaration*s within *namespace_declaration*s that have the same fully qualified namespace name are members of a single combined declaration space.</span></span>
*  <span data-ttu-id="378d4-151">各クラス、構造体、またはインターフェイスの宣言は、新しい宣言領域を作成します。</span><span class="sxs-lookup"><span data-stu-id="378d4-151">Each class, struct, or interface declaration creates a new declaration space.</span></span> <span data-ttu-id="378d4-152">この宣言領域に名前が導入*class_member_declaration*s、 *struct_member_declaration*s、 *interface_member_declaration*s、または*type_parameter*秒。</span><span class="sxs-lookup"><span data-stu-id="378d4-152">Names are introduced into this declaration space through *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, or *type_parameter*s.</span></span> <span data-ttu-id="378d4-153">オーバー ロードされたコンス トラクターを除く宣言と宣言、クラスまたは構造体の静的コンス トラクターは、クラスまたは構造体と同じ名前のメンバーの宣言を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="378d4-153">Except for overloaded instance constructor declarations and static constructor declarations, a class or struct cannot contain a member declaration with the same name as the class or struct.</span></span> <span data-ttu-id="378d4-154">クラス、構造体、またはインターフェイスは、オーバー ロードされたメソッドとインデクサーの宣言を許可します。</span><span class="sxs-lookup"><span data-stu-id="378d4-154">A class, struct, or interface permits the declaration of overloaded methods and indexers.</span></span> <span data-ttu-id="378d4-155">さらに、クラスまたは構造体は、オーバー ロードされたインスタンスのコンス トラクターと演算子の宣言を許可します。</span><span class="sxs-lookup"><span data-stu-id="378d4-155">Furthermore, a class or struct permits the declaration of overloaded instance constructors and operators.</span></span> <span data-ttu-id="378d4-156">たとえば、クラス、構造体、またはインターフェイス含めることができます、同じ名前を持つ複数のメソッド宣言のシグネチャでこれらのメソッド宣言が異なるので ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading))。</span><span class="sxs-lookup"><span data-stu-id="378d4-156">For example, a class, struct, or interface may contain multiple method declarations with the same name, provided these method declarations differ in their signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)).</span></span> <span data-ttu-id="378d4-157">基底クラスは、クラスの宣言領域には影響しません、基本インターフェイスは、インターフェイスの宣言領域に関係のないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="378d4-157">Note that base classes do not contribute to the declaration space of a class, and base interfaces do not contribute to the declaration space of an interface.</span></span> <span data-ttu-id="378d4-158">したがって、派生クラスまたはインターフェイスは継承メンバーと同じ名前のメンバーを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="378d4-158">Thus, a derived class or interface is allowed to declare a member with the same name as an inherited member.</span></span> <span data-ttu-id="378d4-159">このようなメンバーといいます***を非表示に***継承されたメンバー。</span><span class="sxs-lookup"><span data-stu-id="378d4-159">Such a member is said to ***hide*** the inherited member.</span></span>
*  <span data-ttu-id="378d4-160">各デリゲート宣言では、新しい宣言領域を作成します。</span><span class="sxs-lookup"><span data-stu-id="378d4-160">Each delegate declaration creates a new declaration space.</span></span> <span data-ttu-id="378d4-161">仮パラメーターでは、この宣言領域に名前が導入 (*fixed_parameter*s と*括ら*s) と*type_parameter*秒。</span><span class="sxs-lookup"><span data-stu-id="378d4-161">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span>
*  <span data-ttu-id="378d4-162">各列挙体の宣言は、新しい宣言領域を作成します。</span><span class="sxs-lookup"><span data-stu-id="378d4-162">Each enumeration declaration creates a new declaration space.</span></span> <span data-ttu-id="378d4-163">この宣言領域に名前が導入*enum_member_declarations*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-163">Names are introduced into this declaration space through *enum_member_declarations*.</span></span>
*  <span data-ttu-id="378d4-164">各メソッドの宣言、インデクサーの宣言、演算子の宣言、インスタンス コンス トラクターの宣言および匿名関数の作成と呼ばれる新しい宣言領域、***ローカル変数の宣言領域***します。</span><span class="sxs-lookup"><span data-stu-id="378d4-164">Each method declaration, indexer declaration, operator declaration, instance constructor declaration and anonymous function creates a new declaration space called a ***local variable declaration space***.</span></span> <span data-ttu-id="378d4-165">仮パラメーターでは、この宣言領域に名前が導入 (*fixed_parameter*s と*括ら*s) と*type_parameter*秒。</span><span class="sxs-lookup"><span data-stu-id="378d4-165">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span> <span data-ttu-id="378d4-166">関数メンバーまたは匿名関数の本体、存在する場合と見なされます、ローカル変数の宣言領域内で入れ子にします。</span><span class="sxs-lookup"><span data-stu-id="378d4-166">The body of the function member or anonymous function, if any, is considered to be nested within the local variable declaration space.</span></span> <span data-ttu-id="378d4-167">ローカル変数の宣言領域と同じ名前の要素を格納するローカル変数の宣言を入れ子になった領域エラーになります。</span><span class="sxs-lookup"><span data-stu-id="378d4-167">It is an error for a local variable declaration space and a nested local variable declaration space to contain elements with the same name.</span></span> <span data-ttu-id="378d4-168">したがって、入れ子になった宣言領域の外側の宣言領域で、ローカル変数またはローカル変数と同じ名前の定数または定数を宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="378d4-168">Thus, within a nested declaration space it is not possible to declare a local variable or constant with the same name as a local variable or constant in an enclosing declaration space.</span></span> <span data-ttu-id="378d4-169">どちらの宣言領域に、もう一方が含まれている限り、同じ名前の要素を格納する 2 つの宣言スペースのことができます。</span><span class="sxs-lookup"><span data-stu-id="378d4-169">It is possible for two declaration spaces to contain elements with the same name as long as neither declaration space contains the other.</span></span>
*  <span data-ttu-id="378d4-170">各*ブロック*または*switch_block*と同様に、*の*、 *foreach*と*を使用して*ステートメントでは、作成します。ローカル変数とローカル定数のローカル変数の宣言領域です。</span><span class="sxs-lookup"><span data-stu-id="378d4-170">Each *block* or *switch_block* , as well as a *for*, *foreach* and *using* statement, creates a local variable declaration space for local variables and local constants .</span></span> <span data-ttu-id="378d4-171">この宣言領域に名前が導入*local_variable_declaration*s と*local_constant_declaration*秒。</span><span class="sxs-lookup"><span data-stu-id="378d4-171">Names are introduced into this declaration space through *local_variable_declaration*s and *local_constant_declaration*s.</span></span> <span data-ttu-id="378d4-172">として、または関数のメンバーまたは匿名関数の本文内で発生するブロックがそのパラメーターに対してこれらの関数によって宣言されたローカル変数の宣言領域内で入れ子にされていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="378d4-172">Note that blocks that occur as or within the body of a function member or anonymous function are nested within the local variable declaration space declared by those functions for their parameters.</span></span> <span data-ttu-id="378d4-173">そのため、エラーにローカル変数と同じ名前のパラメーターのメソッドになどになります。</span><span class="sxs-lookup"><span data-stu-id="378d4-173">Thus it is an error to have e.g. a method with a local variable and a parameter of the same name.</span></span>
*  <span data-ttu-id="378d4-174">各*ブロック*または*switch_block*ラベルの別の宣言領域を作成します。</span><span class="sxs-lookup"><span data-stu-id="378d4-174">Each *block* or *switch_block* creates a separate declaration space for labels.</span></span> <span data-ttu-id="378d4-175">この宣言領域に名前が導入*labeled_statement*秒、および名前がを通じて参照*goto_statement*s。</span><span class="sxs-lookup"><span data-stu-id="378d4-175">Names are introduced into this declaration space through *labeled_statement*s, and the names are referenced through *goto_statement*s.</span></span> <span data-ttu-id="378d4-176">***宣言領域にラベルを付ける***ブロックの入れ子になったブロックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="378d4-176">The ***label declaration space*** of a block includes any nested blocks.</span></span> <span data-ttu-id="378d4-177">したがって、入れ子になったブロックの外側のブロックのラベルと同じ名前のラベルを宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="378d4-177">Thus, within a nested block it is not possible to declare a label with the same name as a label in an enclosing block.</span></span>

<span data-ttu-id="378d4-178">名が宣言されているテキストの順序は、一般にあまり意味はありません。</span><span class="sxs-lookup"><span data-stu-id="378d4-178">The textual order in which names are declared is generally of no significance.</span></span> <span data-ttu-id="378d4-179">具体的には、テキストの順序は、宣言と名前空間、定数、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンス コンス トラクター、デストラクター、静的コンス トラクター、および種類の意味ではありません。</span><span class="sxs-lookup"><span data-stu-id="378d4-179">In particular, textual order is not significant for the declaration and use of namespaces, constants, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors, and types.</span></span> <span data-ttu-id="378d4-180">宣言の順序は、次の方法で重要です。</span><span class="sxs-lookup"><span data-stu-id="378d4-180">Declaration order is significant in the following ways:</span></span>

*  <span data-ttu-id="378d4-181">フィールドの宣言とローカル変数宣言の宣言の順序は、(ある場合) には、初期化子が実行される順序を決定します。</span><span class="sxs-lookup"><span data-stu-id="378d4-181">Declaration order for field declarations and local variable declarations determines the order in which their initializers (if any) are executed.</span></span>
*  <span data-ttu-id="378d4-182">使用されるように、ローカル変数を定義する必要があります ([スコープ](basic-concepts.md#scopes))。</span><span class="sxs-lookup"><span data-stu-id="378d4-182">Local variables must be defined before they are used ([Scopes](basic-concepts.md#scopes)).</span></span>
*  <span data-ttu-id="378d4-183">列挙型メンバーの宣言の宣言の順序 ([列挙型メンバー](enums.md#enum-members)) ときに、大きな*constant_expression*値が省略されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-183">Declaration order for enum member declarations ([Enum members](enums.md#enum-members)) is significant when *constant_expression* values are omitted.</span></span>

<span data-ttu-id="378d4-184">名前空間の宣言領域は、"終了"開き、2 つの名前空間と同じ完全修飾名の宣言が同じ宣言領域を構成します。</span><span class="sxs-lookup"><span data-stu-id="378d4-184">The declaration space of a namespace is "open ended", and two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="378d4-185">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="378d4-185">For example</span></span>
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

<span data-ttu-id="378d4-186">ここで完全修飾の名前を持つ 2 つのクラスを宣言する、同じ宣言領域に 2 つの名前空間宣言が貢献`Megacorp.Data.Customer`と`Megacorp.Data.Order`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-186">The two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Megacorp.Data.Customer` and `Megacorp.Data.Order`.</span></span> <span data-ttu-id="378d4-187">同じ宣言領域に 2 つの宣言がドキュメントに投稿するためにはエラーが発生、コンパイル時場合と同じ名前のクラスの宣言が含まれている各します。</span><span class="sxs-lookup"><span data-stu-id="378d4-187">Because the two declarations contribute to the same declaration space, it would have caused a compile-time error if each contained a declaration of a class with the same name.</span></span>

<span data-ttu-id="378d4-188">上で指定した、ブロックの宣言領域には、任意の入れ子になったブロックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="378d4-188">As specified above, the declaration space of a block includes any nested blocks.</span></span> <span data-ttu-id="378d4-189">したがって、次の例で、`F`と`G`メソッドがコンパイル時エラーが発生するため、名前`i`は外側のブロックで宣言されており、内側のブロックで再宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="378d4-189">Thus, in the following example, the `F` and `G` methods result in a compile-time error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="378d4-190">ただし、`H`と`I`メソッドは、2 つから有効な`i`の入れ子になっていない別のブロックで宣言されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-190">However, the `H` and `I` methods are valid since the two `i`'s are declared in separate non-nested blocks.</span></span>

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

## <a name="members"></a><span data-ttu-id="378d4-191">メンバー</span><span class="sxs-lookup"><span data-stu-id="378d4-191">Members</span></span>

<span data-ttu-id="378d4-192">名前空間と型が***メンバー***します。</span><span class="sxs-lookup"><span data-stu-id="378d4-192">Namespaces and types have ***members***.</span></span> <span data-ttu-id="378d4-193">エンティティのメンバーは、一般提供開始後に、エンティティへの参照で始まる修飾名を使用して、"`.`"トークン、その後に、メンバーの名前。</span><span class="sxs-lookup"><span data-stu-id="378d4-193">The members of an entity are generally available through the use of a qualified name that starts with a reference to the entity, followed by a "`.`" token, followed by the name of the member.</span></span>

<span data-ttu-id="378d4-194">型のメンバーが型の宣言で宣言されたか、または***継承***型の基本クラスから。</span><span class="sxs-lookup"><span data-stu-id="378d4-194">Members of a type are either declared in the type declaration or ***inherited*** from the base class of the type.</span></span> <span data-ttu-id="378d4-195">型が基本クラス、インスタンス コンス トラクター、デストラクター、および静的コンス トラクターを除く、基底クラスのすべてのメンバーから継承する場合は、派生型のメンバーになります。</span><span class="sxs-lookup"><span data-stu-id="378d4-195">When a type inherits from a base class, all members of the base class, except instance constructors, destructors and static constructors, become members of the derived type.</span></span> <span data-ttu-id="378d4-196">基底クラスのメンバーの宣言されたアクセシビリティは、メンバーが継承されるかどうかを制御しません-継承は、インスタンス コンス トラクター、静的コンス トラクターまたはデストラクター以外の任意のメンバーを拡張します。</span><span class="sxs-lookup"><span data-stu-id="378d4-196">The declared accessibility of a base class member does not control whether the member is inherited—inheritance extends to any member that isn't an instance constructor, static constructor, or destructor.</span></span> <span data-ttu-id="378d4-197">ただし、継承されたメンバーをアクセスできない派生型で宣言されたアクセシビリティのいずれかで ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))、型自体で宣言によって非表示になっているため、または ([を非表示継承](basic-concepts.md#hiding-through-inheritance))。</span><span class="sxs-lookup"><span data-stu-id="378d4-197">However, an inherited member may not be accessible in a derived type, either because of its declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) or because it is hidden by a declaration in the type itself ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

### <a name="namespace-members"></a><span data-ttu-id="378d4-198">Namespace メンバー</span><span class="sxs-lookup"><span data-stu-id="378d4-198">Namespace members</span></span>

<span data-ttu-id="378d4-199">名前空間とそれを囲む名前空間を持たない型のメンバーである、***グローバル名前空間***します。</span><span class="sxs-lookup"><span data-stu-id="378d4-199">Namespaces and types that have no enclosing namespace are members of the ***global namespace***.</span></span> <span data-ttu-id="378d4-200">これは、グローバル宣言領域で宣言された名前に直接対応します。</span><span class="sxs-lookup"><span data-stu-id="378d4-200">This corresponds directly to the names declared in the global declaration space.</span></span>

<span data-ttu-id="378d4-201">名前空間と名前空間内で宣言された型は、その名前空間のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="378d4-201">Namespaces and types declared within a namespace are members of that namespace.</span></span> <span data-ttu-id="378d4-202">これは、名前空間の宣言領域で宣言された名前に直接対応します。</span><span class="sxs-lookup"><span data-stu-id="378d4-202">This corresponds directly to the names declared in the declaration space of the namespace.</span></span>

<span data-ttu-id="378d4-203">名前空間には、アクセス制限がありません。</span><span class="sxs-lookup"><span data-stu-id="378d4-203">Namespaces have no access restrictions.</span></span> <span data-ttu-id="378d4-204">プライベート、プロテクト、または内部の名前空間を宣言することはできませんし、名前空間の名前はパブリックにアクセスできる、常にします。</span><span class="sxs-lookup"><span data-stu-id="378d4-204">It is not possible to declare private, protected, or internal namespaces, and namespace names are always publicly accessible.</span></span>

### <a name="struct-members"></a><span data-ttu-id="378d4-205">構造体のメンバー</span><span class="sxs-lookup"><span data-stu-id="378d4-205">Struct members</span></span>

<span data-ttu-id="378d4-206">構造体のメンバーは、構造体で宣言されたメンバーと、構造体の直接基底クラスから継承されたメンバー`System.ValueType`間接基底クラスと`object`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-206">The members of a struct are the members declared in the struct and the members inherited from the struct's direct base class `System.ValueType` and the indirect base class `object`.</span></span>

<span data-ttu-id="378d4-207">単純型のメンバーは、単純型によって構造体の型エイリアスのメンバーに直接対応しています。</span><span class="sxs-lookup"><span data-stu-id="378d4-207">The members of a simple type correspond directly to the members of the struct type aliased by the simple type:</span></span>

*  <span data-ttu-id="378d4-208">メンバー`sbyte`のメンバーである、`System.SByte`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-208">The members of `sbyte` are the members of the `System.SByte` struct.</span></span>
*  <span data-ttu-id="378d4-209">メンバー`byte`のメンバーである、`System.Byte`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-209">The members of `byte` are the members of the `System.Byte` struct.</span></span>
*  <span data-ttu-id="378d4-210">メンバー`short`のメンバーである、`System.Int16`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-210">The members of `short` are the members of the `System.Int16` struct.</span></span>
*  <span data-ttu-id="378d4-211">メンバー`ushort`のメンバーである、`System.UInt16`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-211">The members of `ushort` are the members of the `System.UInt16` struct.</span></span>
*  <span data-ttu-id="378d4-212">メンバー`int`のメンバーである、`System.Int32`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-212">The members of `int` are the members of the `System.Int32` struct.</span></span>
*  <span data-ttu-id="378d4-213">メンバー`uint`のメンバーである、`System.UInt32`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-213">The members of `uint` are the members of the `System.UInt32` struct.</span></span>
*  <span data-ttu-id="378d4-214">メンバー`long`のメンバーである、`System.Int64`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-214">The members of `long` are the members of the `System.Int64` struct.</span></span>
*  <span data-ttu-id="378d4-215">メンバー`ulong`のメンバーである、`System.UInt64`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-215">The members of `ulong` are the members of the `System.UInt64` struct.</span></span>
*  <span data-ttu-id="378d4-216">メンバー`char`のメンバーである、`System.Char`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-216">The members of `char` are the members of the `System.Char` struct.</span></span>
*  <span data-ttu-id="378d4-217">メンバー`float`のメンバーである、`System.Single`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-217">The members of `float` are the members of the `System.Single` struct.</span></span>
*  <span data-ttu-id="378d4-218">メンバー`double`のメンバーである、`System.Double`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-218">The members of `double` are the members of the `System.Double` struct.</span></span>
*  <span data-ttu-id="378d4-219">メンバー`decimal`のメンバーである、`System.Decimal`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-219">The members of `decimal` are the members of the `System.Decimal` struct.</span></span>
*  <span data-ttu-id="378d4-220">メンバー`bool`のメンバーである、`System.Boolean`構造体。</span><span class="sxs-lookup"><span data-stu-id="378d4-220">The members of `bool` are the members of the `System.Boolean` struct.</span></span>

### <a name="enumeration-members"></a><span data-ttu-id="378d4-221">列挙型メンバー</span><span class="sxs-lookup"><span data-stu-id="378d4-221">Enumeration members</span></span>

<span data-ttu-id="378d4-222">列挙体のメンバーは、列挙体で宣言された定数と列挙型の直接基底クラスから継承されたメンバー`System.Enum`と間接基底クラス`System.ValueType`と`object`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-222">The members of an enumeration are the constants declared in the enumeration and the members inherited from the enumeration's direct base class `System.Enum` and the indirect base classes `System.ValueType` and `object`.</span></span>

### <a name="class-members"></a><span data-ttu-id="378d4-223">クラス メンバー</span><span class="sxs-lookup"><span data-stu-id="378d4-223">Class members</span></span>

<span data-ttu-id="378d4-224">クラスのメンバーは、クラスで宣言されたメンバーと基本クラスから継承されたメンバー (クラスを除く`object`する基本クラスがありません)。</span><span class="sxs-lookup"><span data-stu-id="378d4-224">The members of a class are the members declared in the class and the members inherited from the base class (except for class `object` which has no base class).</span></span> <span data-ttu-id="378d4-225">基本クラスから継承されたメンバーには、定数、フィールド、メソッド、プロパティ、イベント、インデクサー、演算子、および、基底クラスですがない、インスタンス コンス トラクター、デストラクターおよび基底クラスの静的コンス トラクターの型が含まれます。</span><span class="sxs-lookup"><span data-stu-id="378d4-225">The members inherited from the base class include the constants, fields, methods, properties, events, indexers, operators, and types of the base class, but not the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="378d4-226">アクセシビリティに関係なく、基本クラスのメンバーが継承されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-226">Base class members are inherited without regard to their accessibility.</span></span>

<span data-ttu-id="378d4-227">クラス宣言では、定数、フィールド、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンス コンス トラクター、デストラクター、静的コンス トラクターと型の宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="378d4-227">A class declaration may contain declarations of constants, fields, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors and types.</span></span>

<span data-ttu-id="378d4-228">メンバー`object`と`string`に直接対応して、クラス型のメンバーがエイリアス。</span><span class="sxs-lookup"><span data-stu-id="378d4-228">The members of `object` and `string` correspond directly to the members of the class types they alias:</span></span>

*  <span data-ttu-id="378d4-229">メンバー`object`のメンバーである、`System.Object`クラス。</span><span class="sxs-lookup"><span data-stu-id="378d4-229">The members of `object` are the members of the `System.Object` class.</span></span>
*  <span data-ttu-id="378d4-230">メンバー`string`のメンバーである、`System.String`クラス。</span><span class="sxs-lookup"><span data-stu-id="378d4-230">The members of `string` are the members of the `System.String` class.</span></span>

### <a name="interface-members"></a><span data-ttu-id="378d4-231">インターフェイスのメンバー</span><span class="sxs-lookup"><span data-stu-id="378d4-231">Interface members</span></span>

<span data-ttu-id="378d4-232">インターフェイスのメンバーは、インターフェイス、およびインターフェイスのすべての基本インターフェイスで宣言されたメンバーです。</span><span class="sxs-lookup"><span data-stu-id="378d4-232">The members of an interface are the members declared in the interface and in all base interfaces of the interface.</span></span> <span data-ttu-id="378d4-233">クラスでメンバー`object`厳密に言うと、任意のインターフェイスのメンバーは使用されません ([インターフェイスのメンバー](interfaces.md#interface-members))。</span><span class="sxs-lookup"><span data-stu-id="378d4-233">The members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="378d4-234">ただし、クラスでメンバー`object`は任意のインターフェイス型でメンバーの検索で利用できます ([メンバー ルックアップ](expressions.md#member-lookup))。</span><span class="sxs-lookup"><span data-stu-id="378d4-234">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="array-members"></a><span data-ttu-id="378d4-235">配列メンバー</span><span class="sxs-lookup"><span data-stu-id="378d4-235">Array members</span></span>

<span data-ttu-id="378d4-236">配列のメンバーは、クラスから継承されたメンバー`System.Array`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-236">The members of an array are the members inherited from class `System.Array`.</span></span>

### <a name="delegate-members"></a><span data-ttu-id="378d4-237">デリゲートのメンバー</span><span class="sxs-lookup"><span data-stu-id="378d4-237">Delegate members</span></span>

<span data-ttu-id="378d4-238">デリゲートのメンバーは、クラスから継承されたメンバー`System.Delegate`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-238">The members of a delegate are the members inherited from class `System.Delegate`.</span></span>

## <a name="member-access"></a><span data-ttu-id="378d4-239">メンバー アクセス。</span><span class="sxs-lookup"><span data-stu-id="378d4-239">Member access</span></span>

<span data-ttu-id="378d4-240">メンバーの宣言では、メンバー アクセス制御を許可します。</span><span class="sxs-lookup"><span data-stu-id="378d4-240">Declarations of members allow control over member access.</span></span> <span data-ttu-id="378d4-241">メンバーのアクセシビリティが宣言されたアクセシビリティで確立されます ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility)) メンバーのアクセシビリティとの組み合わせ、すぐにそれを含む型の存在する場合。</span><span class="sxs-lookup"><span data-stu-id="378d4-241">The accessibility of a member is established by the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of the member combined with the accessibility of the immediately containing type, if any.</span></span>

<span data-ttu-id="378d4-242">特定のメンバーへのアクセスを許可すると、メンバーがあると言います。***アクセス***します。</span><span class="sxs-lookup"><span data-stu-id="378d4-242">When access to a particular member is allowed, the member is said to be ***accessible***.</span></span> <span data-ttu-id="378d4-243">逆に、特定のメンバーへのアクセスが許可されていない場合、メンバー言います***アクセスできない***します。</span><span class="sxs-lookup"><span data-stu-id="378d4-243">Conversely, when access to a particular member is disallowed, the member is said to be ***inaccessible***.</span></span> <span data-ttu-id="378d4-244">アクセシビリティ ドメインにアクセスが行われる場所が含まれている場合、メンバーへのアクセスが許可されて ([アクセシビリティ ドメイン](basic-concepts.md#accessibility-domains)) のメンバー。</span><span class="sxs-lookup"><span data-stu-id="378d4-244">Access to a member is permitted when the textual location in which the access takes place is included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>

### <a name="declared-accessibility"></a><span data-ttu-id="378d4-245">アクセシビリティの宣言</span><span class="sxs-lookup"><span data-stu-id="378d4-245">Declared accessibility</span></span>

<span data-ttu-id="378d4-246">***宣言されたアクセシビリティ***メンバーのことができますが、次のいずれか。</span><span class="sxs-lookup"><span data-stu-id="378d4-246">The ***declared accessibility*** of a member can be one of the following:</span></span>

*  <span data-ttu-id="378d4-247">パブリックで、含めることでが選択されている、`public`メンバーの宣言での修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-247">Public, which is selected by including a `public` modifier in the member declaration.</span></span> <span data-ttu-id="378d4-248">直感的な意味`public`「アクセスは制限されません」です。</span><span class="sxs-lookup"><span data-stu-id="378d4-248">The intuitive meaning of `public` is "access not limited".</span></span>
*  <span data-ttu-id="378d4-249">保護をオンになっていますを含めることによって、`protected`メンバーの宣言での修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-249">Protected, which is selected by including a `protected` modifier in the member declaration.</span></span> <span data-ttu-id="378d4-250">直感的な意味`protected`が"アクセスはコンテナーのクラスまたは型に制限されます、コンテナー クラスから派生"。</span><span class="sxs-lookup"><span data-stu-id="378d4-250">The intuitive meaning of `protected` is "access limited to the containing class or types derived from the containing class".</span></span>
*  <span data-ttu-id="378d4-251">内部で、含めることでが選択されている、`internal`メンバーの宣言での修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-251">Internal, which is selected by including an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="378d4-252">直感的な意味`internal`「このプログラムにだけがアクセス」。</span><span class="sxs-lookup"><span data-stu-id="378d4-252">The intuitive meaning of `internal` is "access limited to this program".</span></span>
*  <span data-ttu-id="378d4-253">プロテクト内部 (つまり、protected、または内部) 両方を含めることによってこれが選択されている、`protected`と`internal`メンバーの宣言に修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-253">Protected internal (meaning protected or internal), which is selected by including both a `protected` and an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="378d4-254">直感的な意味`protected internal`が「このプログラムにだけがアクセスまたは含んでいるクラスから派生した型」です。</span><span class="sxs-lookup"><span data-stu-id="378d4-254">The intuitive meaning of `protected internal` is "access limited to this program or types derived from the containing class".</span></span>
*  <span data-ttu-id="378d4-255">プライベートで、含めることでが選択されている、`private`メンバーの宣言での修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-255">Private, which is selected by including a `private` modifier in the member declaration.</span></span> <span data-ttu-id="378d4-256">直感的な意味`private`「を含んでいる型だけがアクセス」。</span><span class="sxs-lookup"><span data-stu-id="378d4-256">The intuitive meaning of `private` is "access limited to the containing type".</span></span>

<span data-ttu-id="378d4-257">メンバーの宣言をコンテキストに応じて配置、宣言されたアクセシビリティの特定の種類のみが許可されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-257">Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted.</span></span> <span data-ttu-id="378d4-258">さらに、メンバーの宣言に、アクセス修飾子が含まれていない場合、宣言が行われるコンテキストには、既定の宣言されたアクセシビリティが決まります。</span><span class="sxs-lookup"><span data-stu-id="378d4-258">Furthermore, when a member declaration does not include any access modifiers, the context in which the declaration takes place determines the default declared accessibility.</span></span>

*  <span data-ttu-id="378d4-259">名前空間が暗黙的が`public`アクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="378d4-259">Namespaces implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="378d4-260">名前空間の宣言では、アクセス修飾子は許可されません。</span><span class="sxs-lookup"><span data-stu-id="378d4-260">No access modifiers are allowed on namespace declarations.</span></span>
*  <span data-ttu-id="378d4-261">コンパイル単位または名前空間で宣言された型を持つことができます`public`または`internal`アクセシビリティおよび既定値は宣言されている`internal`アクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="378d4-261">Types declared in compilation units or namespaces can have `public` or `internal` declared accessibility and default to `internal` declared accessibility.</span></span>
*  <span data-ttu-id="378d4-262">クラスのメンバーが宣言されたアクセシビリティの 5 種類のいずれかがあるし、する既定の`private`アクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="378d4-262">Class members can have any of the five kinds of declared accessibility and default to `private` declared accessibility.</span></span> <span data-ttu-id="378d4-263">(を型として宣言されたクラスのメンバーが宣言されたアクセシビリティの 5 種類のいずれかを持つことができますのみ名前空間のメンバーは、宣言された型に対してに注意してください`public`または`internal`アクセシビリティで宣言されています。)。</span><span class="sxs-lookup"><span data-stu-id="378d4-263">(Note that a type declared as a member of a class can have any of the five kinds of declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="378d4-264">構造体のメンバーが持つことができます`public`、 `internal`、または`private`アクセシビリティおよび既定値は宣言されている`private`構造体は暗黙的にシールされているため、アクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="378d4-264">Struct members can have `public`, `internal`, or `private` declared accessibility and default to `private` declared accessibility because structs are implicitly sealed.</span></span> <span data-ttu-id="378d4-265">構造体のメンバー (には、その構造体によって継承されません) 構造体で導入されたことはできません`protected`または`protected internal`アクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="378d4-265">Struct members introduced in a struct (that is, not inherited by that struct) cannot have `protected` or `protected internal` declared accessibility.</span></span> <span data-ttu-id="378d4-266">(構造体のメンバーであることができますが型に宣言されていることに注意してください`public`、 `internal`、または`private`のみ名前空間のメンバーは、宣言された型は、アクセシビリティを宣言`public`または`internal`宣言されたアクセシビリティ。)。</span><span class="sxs-lookup"><span data-stu-id="378d4-266">(Note that a type declared as a member of a struct can have `public`, `internal`, or `private` declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="378d4-267">インターフェイス メンバーが暗黙的が`public`アクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="378d4-267">Interface members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="378d4-268">インターフェイス メンバー宣言では、アクセス修飾子は許可されません。</span><span class="sxs-lookup"><span data-stu-id="378d4-268">No access modifiers are allowed on interface member declarations.</span></span>
*  <span data-ttu-id="378d4-269">列挙型メンバーが暗黙的が`public`アクセシビリティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="378d4-269">Enumeration members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="378d4-270">列挙体メンバーの宣言では、アクセス修飾子は許可されません。</span><span class="sxs-lookup"><span data-stu-id="378d4-270">No access modifiers are allowed on enumeration member declarations.</span></span>

### <a name="accessibility-domains"></a><span data-ttu-id="378d4-271">アクセシビリティ ドメイン</span><span class="sxs-lookup"><span data-stu-id="378d4-271">Accessibility domains</span></span>

<span data-ttu-id="378d4-272">***アクセシビリティ ドメイン***メンバーのメンバーへのアクセスが許可されているプログラム テキストの (場合によって不整合のある) のセクションで構成されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-272">The ***accessibility domain*** of a member consists of the (possibly disjoint) sections of program text in which access to the member is permitted.</span></span> <span data-ttu-id="378d4-273">メンバーのアクセシビリティ ドメインを定義するために、メンバーがあると言います***トップレベル***場合は、型の中で宣言されていないと、メンバーがあると言います***入れ子になった***別の種類内で宣言されている場合。</span><span class="sxs-lookup"><span data-stu-id="378d4-273">For purposes of defining the accessibility domain of a member, a member is said to be ***top-level*** if it is not declared within a type, and a member is said to be ***nested*** if it is declared within another type.</span></span> <span data-ttu-id="378d4-274">さらに、***プログラム テキスト***プログラムは、すべてのプログラム、プログラムのすべてのソース ファイルに含まれるテキストと型のプログラム テキストがすべてのプログラム テキストに含まれるように定義されている、定義、 *type_declaration*など、場合によっては、型の中で入れ子にされた型、その型の秒。</span><span class="sxs-lookup"><span data-stu-id="378d4-274">Furthermore, the ***program text*** of a program is defined as all program text contained in all source files of the program, and the program text of a type is defined as all program text contained in the *type_declaration*s of that type (including, possibly, types that are nested within the type).</span></span>

<span data-ttu-id="378d4-275">定義済みの型のアクセシビリティ ドメイン (など`object`、 `int`、または`double`) に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="378d4-275">The accessibility domain of a predefined type (such as `object`, `int`, or `double`) is unlimited.</span></span>

<span data-ttu-id="378d4-276">最上位のアクセシビリティ ドメインには、型がバインドされていない`T`([バインドされ、型がバインドされていない](types.md#bound-and-unbound-types)) プログラムで宣言されている`P`が次のように定義されています。</span><span class="sxs-lookup"><span data-stu-id="378d4-276">The accessibility domain of a top-level unbound type `T` ([Bound and unbound types](types.md#bound-and-unbound-types)) that is declared in a program `P` is defined as follows:</span></span>

*  <span data-ttu-id="378d4-277">場合の宣言されたアクセシビリティ`T`は`public`のアクセシビリティ ドメイン`T`のプログラム テキスト`P`と参照するすべてのプログラム`P`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-277">If the declared accessibility of `T` is `public`, the accessibility domain of `T` is the program text of `P` and any program that references `P`.</span></span>
*  <span data-ttu-id="378d4-278">`T` に対して宣言されているアクセシビリティが `internal` の場合、`T` のアクセシビリティ ドメインは `P` のプログラム テキストになります。</span><span class="sxs-lookup"><span data-stu-id="378d4-278">If the declared accessibility of `T` is `internal`, the accessibility domain of `T` is the program text of `P`.</span></span>

<span data-ttu-id="378d4-279">これらの定義からは、最上位レベルのバインドされていない型のアクセシビリティ ドメインは常に、少なくともを次の種類をプログラムのプログラム テキストを宣言します。</span><span class="sxs-lookup"><span data-stu-id="378d4-279">From these definitions it follows that the accessibility domain of a top-level unbound type is always at least the program text of the program in which that type is declared.</span></span>

<span data-ttu-id="378d4-280">構築された型のアクセシビリティ ドメイン`T<A1, ..., An>`バインドされていないジェネリック型のアクセシビリティ ドメインとの積集合`T`と型引数のアクセシビリティ ドメイン`A1, ..., An`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-280">The accessibility domain for a constructed type `T<A1, ..., An>` is the intersection of the accessibility domain of the unbound generic type `T` and the accessibility domains of the type arguments `A1, ..., An`.</span></span>

<span data-ttu-id="378d4-281">入れ子にされたメンバーのアクセシビリティ ドメイン`M`型で宣言`T`、プログラム内で`P`が次のように定義されている (いう`M`自体は型である可能性があります可能性があります)。</span><span class="sxs-lookup"><span data-stu-id="378d4-281">The accessibility domain of a nested member `M` declared in a type `T` within a program `P` is defined as follows (noting that `M` itself may possibly be a type):</span></span>

*  <span data-ttu-id="378d4-282">`M` に対して宣言されたアクセシビリティが `public` の場合、`M` のアクセシビリティ ドメインは `T` のアクセシビリティ ドメインになります。</span><span class="sxs-lookup"><span data-stu-id="378d4-282">If the declared accessibility of `M` is `public`, the accessibility domain of `M` is the accessibility domain of `T`.</span></span>
*  <span data-ttu-id="378d4-283">場合の宣言されたアクセシビリティ`M`は`protected internal`、`D`のプログラム テキストの和集合をする`P`から派生した任意の型のプログラム テキストと`T`の外部で宣言されている`P`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-283">If the declared accessibility of `M` is `protected internal`, let `D` be the union of the program text of `P` and the program text of any type derived from `T`, which is declared outside `P`.</span></span> <span data-ttu-id="378d4-284">アクセシビリティ ドメイン`M`のアクセシビリティ ドメインとの積集合`T`で`D`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-284">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="378d4-285">場合の宣言されたアクセシビリティ`M`は`protected`、`D`のプログラム テキストの和集合をする`T`から派生した任意の型のプログラム テキストと`T`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-285">If the declared accessibility of `M` is `protected`, let `D` be the union of the program text of `T` and the program text of any type derived from `T`.</span></span> <span data-ttu-id="378d4-286">アクセシビリティ ドメイン`M`のアクセシビリティ ドメインとの積集合`T`で`D`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-286">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="378d4-287">`M` に対して宣言されているアクセシビリティが `internal` の場合、`M` のアクセシビリティ ドメインは、`T` のアクセシビリティ ドメインと `P` のプログラム テキストとの積集合になります。</span><span class="sxs-lookup"><span data-stu-id="378d4-287">If the declared accessibility of `M` is `internal`, the accessibility domain of `M` is the intersection of the accessibility domain of `T` with the program text of `P`.</span></span>
*  <span data-ttu-id="378d4-288">`M` に対して宣言されているアクセシビリティが `private` の場合、`M` のアクセシビリティ ドメインは `T` のプログラム テキストになります。</span><span class="sxs-lookup"><span data-stu-id="378d4-288">If the declared accessibility of `M` is `private`, the accessibility domain of `M` is the program text of `T`.</span></span>

<span data-ttu-id="378d4-289">これらの定義からは、入れ子にされたメンバーのアクセシビリティ ドメインは常に、少なくともを次のメンバーが宣言されている種類のプログラム テキストです。</span><span class="sxs-lookup"><span data-stu-id="378d4-289">From these definitions it follows that the accessibility domain of a nested member is always at least the program text of the type in which the member is declared.</span></span> <span data-ttu-id="378d4-290">さらに、メンバーのアクセシビリティ ドメインはさらに、メンバーが宣言されている型のアクセシビリティ ドメインより包括的でないことに従います。</span><span class="sxs-lookup"><span data-stu-id="378d4-290">Furthermore, it follows that the accessibility domain of a member is never more inclusive than the accessibility domain of the type in which the member is declared.</span></span>

<span data-ttu-id="378d4-291">わかりやすく言うと、型またはメンバーと`M`がアクセスするには、次の手順は、アクセスが許可されていることを確認に評価されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-291">In intuitive terms, when a type or member `M` is accessed, the following steps are evaluated to ensure that the access is permitted:</span></span>

*  <span data-ttu-id="378d4-292">最初に、if `M` (と対照的にコンパイル単位または名前空間)、型で宣言は、その型にアクセスできない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-292">First, if `M` is declared within a type (as opposed to a compilation unit or a namespace), a compile-time error occurs if that type is not accessible.</span></span>
*  <span data-ttu-id="378d4-293">場合はその後、`M`は`public`アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="378d4-293">Then, if `M` is `public`, the access is permitted.</span></span>
*  <span data-ttu-id="378d4-294">の場合`M`は`protected internal`、をプログラム内で発生した場合、アクセスが許可されます`M`を宣言したクラスから派生したクラス内に現れたかどうかまたは`M`は宣言されており、派生クラスで行われますクラスの種類 ([保護されるインスタンスのメンバーにアクセス](basic-concepts.md#protected-access-for-instance-members))。</span><span class="sxs-lookup"><span data-stu-id="378d4-294">Otherwise, if `M` is `protected internal`, the access is permitted if it occurs within the program in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="378d4-295">の場合`M`は`protected`、クラス内で発生した場合、アクセスが許可されて`M`が宣言されているクラスから派生したクラス内に現れたかどうかまたは`M`は宣言されており、派生クラスで行われますクラスの種類 ([保護されるインスタンスのメンバーにアクセス](basic-concepts.md#protected-access-for-instance-members))。</span><span class="sxs-lookup"><span data-stu-id="378d4-295">Otherwise, if `M` is `protected`, the access is permitted if it occurs within the class in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="378d4-296">の場合`M`は`internal`、をプログラム内で発生した場合に、アクセスが許可されている`M`は宣言されています。</span><span class="sxs-lookup"><span data-stu-id="378d4-296">Otherwise, if `M` is `internal`, the access is permitted if it occurs within the program in which `M` is declared.</span></span>
*  <span data-ttu-id="378d4-297">の場合`M`は`private`、の種類内で発生した場合、アクセスが許可されて`M`は宣言されています。</span><span class="sxs-lookup"><span data-stu-id="378d4-297">Otherwise, if `M` is `private`, the access is permitted if it occurs within the type in which `M` is declared.</span></span>
*  <span data-ttu-id="378d4-298">それ以外の場合、型またはメンバーにアクセスできないと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-298">Otherwise, the type or member is inaccessible, and a compile-time error occurs.</span></span>

<span data-ttu-id="378d4-299">例</span><span class="sxs-lookup"><span data-stu-id="378d4-299">In the example</span></span>
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
<span data-ttu-id="378d4-300">クラスとメンバーは、次のアクセシビリティ ドメインにあります。</span><span class="sxs-lookup"><span data-stu-id="378d4-300">the classes and members have the following accessibility domains:</span></span>

*  <span data-ttu-id="378d4-301">アクセシビリティ ドメイン`A`と`A.X`に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="378d4-301">The accessibility domain of `A` and `A.X` is unlimited.</span></span>
*  <span data-ttu-id="378d4-302">アクセシビリティ ドメイン`A.Y`、 `B`、 `B.X`、 `B.Y`、 `B.C`、 `B.C.X`、および`B.C.Y`含んでいるプログラムのプログラム テキストです。</span><span class="sxs-lookup"><span data-stu-id="378d4-302">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the program text of the containing program.</span></span>
*  <span data-ttu-id="378d4-303">アクセシビリティ ドメイン`A.Z`のプログラム テキスト`A`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-303">The accessibility domain of `A.Z` is the program text of `A`.</span></span>
*  <span data-ttu-id="378d4-304">アクセシビリティ ドメイン`B.Z`と`B.D`のプログラム テキスト`B`のプログラム テキストを含む`B.C`と`B.D`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-304">The accessibility domain of `B.Z` and `B.D` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="378d4-305">アクセシビリティ ドメイン`B.C.Z`のプログラム テキスト`B.C`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-305">The accessibility domain of `B.C.Z` is the program text of `B.C`.</span></span>
*  <span data-ttu-id="378d4-306">アクセシビリティ ドメイン`B.D.X`と`B.D.Y`のプログラム テキスト`B`のプログラム テキストを含む`B.C`と`B.D`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-306">The accessibility domain of `B.D.X` and `B.D.Y` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="378d4-307">アクセシビリティ ドメイン`B.D.Z`のプログラム テキスト`B.D`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-307">The accessibility domain of `B.D.Z` is the program text of `B.D`.</span></span>

<span data-ttu-id="378d4-308">例に示すように、メンバーのアクセシビリティ ドメインを含んでいる型よりも大きいことはありません。</span><span class="sxs-lookup"><span data-stu-id="378d4-308">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="378d4-309">たとえば、場合でもすべて`X`があるパブリックの宣言されたアクセシビリティのメンバー以外のすべて`A.X`アクセシビリティ ドメインは、それを含む型によって制限されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-309">For example, even though all `X` members have public declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="378d4-310">」の説明に従って[メンバー](basic-concepts.md#members)、コンス トラクター、デストラクター、および静的コンス トラクター、インスタンスを除く、基底クラスのすべてのメンバーが派生型によって継承されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-310">As described in [Members](basic-concepts.md#members), all members of a base class, except for instance constructors, destructors and static constructors, are inherited by derived types.</span></span> <span data-ttu-id="378d4-311">これには、基底クラスのプライベート メンバーも含まれています。</span><span class="sxs-lookup"><span data-stu-id="378d4-311">This includes even private members of a base class.</span></span> <span data-ttu-id="378d4-312">ただし、プライベート メンバーのアクセシビリティ ドメインには、メンバーが宣言されている型のプログラム テキストのみが含まれています。</span><span class="sxs-lookup"><span data-stu-id="378d4-312">However, the accessibility domain of a private member includes only the program text of the type in which the member is declared.</span></span> <span data-ttu-id="378d4-313">例</span><span class="sxs-lookup"><span data-stu-id="378d4-313">In the example</span></span>
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
<span data-ttu-id="378d4-314">`B`クラスは、プライベート メンバーを継承`x`から、`A`クラス。</span><span class="sxs-lookup"><span data-stu-id="378d4-314">the `B` class inherits the private member `x` from the `A` class.</span></span> <span data-ttu-id="378d4-315">内でアクセスできる、のみ、メンバーはプライベートなので、 *class_body*の`A`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-315">Because the member is private, it is only accessible within the *class_body* of `A`.</span></span> <span data-ttu-id="378d4-316">アクセスをそのため、`b.x`に成功すると、`A.F`メソッドが失敗で、`B.F`メソッド。</span><span class="sxs-lookup"><span data-stu-id="378d4-316">Thus, the access to `b.x` succeeds in the `A.F` method, but fails in the `B.F` method.</span></span>

### <a name="protected-access-for-instance-members"></a><span data-ttu-id="378d4-317">インスタンス メンバーの保護されたアクセス</span><span class="sxs-lookup"><span data-stu-id="378d4-317">Protected access for instance members</span></span>

<span data-ttu-id="378d4-318">ときに、`protected`インスタンス メンバーのアクセスが宣言されるクラスのプログラム テキストの外側とタイミングを`protected internal`インスタンス メンバーが宣言されているプログラムのプログラム テキストの外側からアクセス、アクセスは内で行う必要があります、宣言されているクラスから派生したクラスの宣言。</span><span class="sxs-lookup"><span data-stu-id="378d4-318">When a `protected` instance member is accessed outside the program text of the class in which it is declared, and when a `protected internal` instance member is accessed outside the program text of the program in which it is declared, the access must take place within a class declaration that derives from the class in which it is declared.</span></span> <span data-ttu-id="378d4-319">さらに、アクセスは、そこから作成されたクラス型、またはその派生クラス型のインスタンスを介して行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-319">Furthermore, the access is required to take place through an instance of that derived class type or a class type constructed from it.</span></span> <span data-ttu-id="378d4-320">この制限は、1 つの派生クラスが、メンバーが、同じ基本クラスから継承された場合でも、他の派生クラスのプロテクト メンバーにアクセスすることを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="378d4-320">This restriction prevents one derived class from accessing protected members of other derived classes, even when the members are inherited from the same base class.</span></span>

<span data-ttu-id="378d4-321">ように`B`保護されたインスタンス メンバーを宣言する基本クラスである`M`、させて`D`から派生したクラスでなければ`B`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-321">Let `B` be a base class that declares a protected instance member `M`, and let `D` be a class that derives from `B`.</span></span> <span data-ttu-id="378d4-322">内で、 *class_body*の`D`へのアクセスを`M`形式は次のいずれかを実行します。</span><span class="sxs-lookup"><span data-stu-id="378d4-322">Within the *class_body* of `D`, access to `M` can take one of the following forms:</span></span>

*  <span data-ttu-id="378d4-323">非限定*type_name*または*primary_expression*フォームの`M`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-323">An unqualified *type_name* or *primary_expression* of the form `M`.</span></span>
*  <span data-ttu-id="378d4-324">A *primary_expression*フォームの`E.M`の型を提供`E`は`T`から派生したクラスまたは`T`ここで、`T`クラス型は、 `D`、またはクラス型構築されました。 `D`</span><span class="sxs-lookup"><span data-stu-id="378d4-324">A *primary_expression* of the form `E.M`, provided the type of `E` is `T` or a class derived from `T`, where `T` is the class type `D`, or a class type constructed from `D`</span></span>
*  <span data-ttu-id="378d4-325">A *primary_expression*フォームの`base.M`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-325">A *primary_expression* of the form `base.M`.</span></span>

<span data-ttu-id="378d4-326">派生クラスがで基底クラスのインスタンスが保護されたコンス トラクターのアクセス時にこれらの形式のアクセスに加え、 *constructor_initializer* ([コンス トラクター初期化子](classes.md#constructor-initializers))。</span><span class="sxs-lookup"><span data-stu-id="378d4-326">In addition to these forms of access, a derived class can access a protected instance constructor of a base class in a *constructor_initializer* ([Constructor initializers](classes.md#constructor-initializers)).</span></span>

<span data-ttu-id="378d4-327">例</span><span class="sxs-lookup"><span data-stu-id="378d4-327">In the example</span></span>
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
<span data-ttu-id="378d4-328">内で`A`、アクセスすることは`x`両方のインスタンスを通じて`A`と`B`いずれの場合では、アクセスがのインスタンスを通じて行われますので、`A`から派生したクラスまたは`A`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-328">within `A`, it is possible to access `x` through instances of both `A` and `B`, since in either case the access takes place through an instance of `A` or a class derived from `A`.</span></span> <span data-ttu-id="378d4-329">ただし、内`B`にアクセスすることはできません`x`のインスタンスを通じて`A`、ため`A`から派生していない`B`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-329">However, within `B`, it is not possible to access `x` through an instance of `A`, since `A` does not derive from `B`.</span></span>

<span data-ttu-id="378d4-330">例</span><span class="sxs-lookup"><span data-stu-id="378d4-330">In the example</span></span>
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
<span data-ttu-id="378d4-331">次の 3 つの割り当てを`x`ジェネリック型から構築されたクラス型のインスタンスで実行されるので許可されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-331">the three assignments to `x` are permitted because they all take place through instances of class types constructed from the generic type.</span></span>

### <a name="accessibility-constraints"></a><span data-ttu-id="378d4-332">アクセシビリティの制約</span><span class="sxs-lookup"><span data-stu-id="378d4-332">Accessibility constraints</span></span>

<span data-ttu-id="378d4-333">C# 言語のいくつかの構成要素に必要な型に***として以上のアクセシビリティ***メンバーまたは別の型。</span><span class="sxs-lookup"><span data-stu-id="378d4-333">Several constructs in the C# language require a type to be ***at least as accessible as*** a member or another type.</span></span> <span data-ttu-id="378d4-334">型`T`少なくともメンバーまたは型と同程度にアクセスできるように言います`M`場合のアクセシビリティ ドメイン`T`のアクセシビリティ ドメインのスーパー セット`M`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-334">A type `T` is said to be at least as accessible as a member or type `M` if the accessibility domain of `T` is a superset of the accessibility domain of `M`.</span></span> <span data-ttu-id="378d4-335">つまり、`T`として以上のアクセシビリティは`M`場合`T`をすべてのコンテキストでアクセス可能な`M`にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="378d4-335">In other words, `T` is at least as accessible as `M` if `T` is accessible in all contexts in which `M` is accessible.</span></span>

<span data-ttu-id="378d4-336">次のユーザー補助の制約があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-336">The following accessibility constraints exist:</span></span>

*  <span data-ttu-id="378d4-337">クラスの型の直接基底クラスは、少なくとも、クラスの型自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-337">The direct base class of a class type must be at least as accessible as the class type itself.</span></span>
*  <span data-ttu-id="378d4-338">インターフェイスの型の明示的な基本インターフェイスは、少なくとも、インターフェイスの型自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-338">The explicit base interfaces of an interface type must be at least as accessible as the interface type itself.</span></span>
*  <span data-ttu-id="378d4-339">デリゲート型の戻り値の型およびパラメーターの型は、少なくとも、デリゲート型自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-339">The return type and parameter types of a delegate type must be at least as accessible as the delegate type itself.</span></span>
*  <span data-ttu-id="378d4-340">定数の型は、少なくとも定数自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-340">The type of a constant must be at least as accessible as the constant itself.</span></span>
*  <span data-ttu-id="378d4-341">フィールドの型は、少なくともフィールド自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-341">The type of a field must be at least as accessible as the field itself.</span></span>
*  <span data-ttu-id="378d4-342">メソッドの戻り値の型およびパラメーターの型は、少なくとも、メソッド自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-342">The return type and parameter types of a method must be at least as accessible as the method itself.</span></span>
*  <span data-ttu-id="378d4-343">プロパティの型は、少なくともプロパティ自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-343">The type of a property must be at least as accessible as the property itself.</span></span>
*  <span data-ttu-id="378d4-344">イベントの型は、少なくともイベント自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-344">The type of an event must be at least as accessible as the event itself.</span></span>
*  <span data-ttu-id="378d4-345">インデクサーの型とパラメーターの型は、少なくとも、インデクサー自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-345">The type and parameter types of an indexer must be at least as accessible as the indexer itself.</span></span>
*  <span data-ttu-id="378d4-346">演算子の戻り値の型とパラメーターの型は、少なくとも、演算子自体と同程度にアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-346">The return type and parameter types of an operator must be at least as accessible as the operator itself.</span></span>
*  <span data-ttu-id="378d4-347">インスタンス コンス トラクターのパラメーターの型は、少なくとも、インスタンス コンス トラクター自体と同程度にアクセスできなければなりません。</span><span class="sxs-lookup"><span data-stu-id="378d4-347">The parameter types of an instance constructor must be at least as accessible as the instance constructor itself.</span></span>

<span data-ttu-id="378d4-348">例</span><span class="sxs-lookup"><span data-stu-id="378d4-348">In the example</span></span>
```csharp
class A {...}

public class B: A {...}
```
<span data-ttu-id="378d4-349">`B`ため、クラスがコンパイル時エラー結果`A`として少なくともアクセス可能でない`B`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-349">the `B` class results in a compile-time error because `A` is not at least as accessible as `B`.</span></span>

<span data-ttu-id="378d4-350">例では同様に、</span><span class="sxs-lookup"><span data-stu-id="378d4-350">Likewise, in the example</span></span>
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
<span data-ttu-id="378d4-351">`H`メソッド`B`結果、戻り値の型のため、コンパイル時エラー、`A`は少なくともメソッドと同程度にアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="378d4-351">the `H` method in `B` results in a compile-time error because the return type `A` is not at least as accessible as the method.</span></span>

## <a name="signatures-and-overloading"></a><span data-ttu-id="378d4-352">シグネチャとオーバー ロード</span><span class="sxs-lookup"><span data-stu-id="378d4-352">Signatures and overloading</span></span>

<span data-ttu-id="378d4-353">メソッド、インスタンス コンス トラクター、インデクサー、および演算子の特性は、その***署名***:</span><span class="sxs-lookup"><span data-stu-id="378d4-353">Methods, instance constructors, indexers, and operators are characterized by their ***signatures***:</span></span>

*  <span data-ttu-id="378d4-354">メソッドのシグネチャは、メソッド、型パラメーターの数と型と仮パラメーターが左から右へ順に検討のそれぞれの種類 (値、参照、または出力) の名前で構成されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-354">The signature of a method consists of the name of the method, the number of type parameters and the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="378d4-355">このため、その名前ではなく、メソッドの型引数リストの序数位置を使用して、仮パラメーターの型で使用されるメソッドの型パラメーターが識別されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-355">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.</span></span> <span data-ttu-id="378d4-356">メソッドのシグネチャが具体的には戻り値の型では含まない、`params`右端のパラメーターも省略可能な型パラメーターの制約で指定できる修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-356">The signature of a method specifically does not include the return type, the `params` modifier that may be specified for the right-most parameter, nor the optional type parameter constraints.</span></span>
*  <span data-ttu-id="378d4-357">インスタンス コンス トラクターのシグネチャは、型と仮パラメーターが左から右へ順に検討のそれぞれの種類 (値、参照、または出力) で構成されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-357">The signature of an instance constructor consists of the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="378d4-358">具体的には、インスタンス コンス トラクターのシグネチャが含まれません場合は、`params`右端のパラメーターで指定できる修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-358">The signature of an instance constructor specifically does not include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="378d4-359">インデクサーのシグネチャは、左から右へ順番と見なされます、仮パラメーターのそれぞれの種類で構成されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-359">The signature of an indexer consists of the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="378d4-360">インデクサーのシグネチャが具体的には、要素の型では含まないも含まれる、`params`右端のパラメーターで指定できる修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-360">The signature of an indexer specifically does not include the element type, nor does it include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="378d4-361">演算子のシグネチャは、演算子と仮パラメーターが左から右へ順に検討のそれぞれの型の名前で構成されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-361">The signature of an operator consists of the name of the operator and the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="378d4-362">具体的には、演算子のシグネチャでは、結果型は含まれません。</span><span class="sxs-lookup"><span data-stu-id="378d4-362">The signature of an operator specifically does not include the result type.</span></span>

<span data-ttu-id="378d4-363">署名の有効化のメカニズムは、***オーバー ロード***クラス、構造体、およびインターフェイス内のメンバーの。</span><span class="sxs-lookup"><span data-stu-id="378d4-363">Signatures are the enabling mechanism for ***overloading*** of members in classes, structs, and interfaces:</span></span>

*  <span data-ttu-id="378d4-364">メソッドのオーバー ロードは、クラス、構造体、またはシグネチャの同じ名前を持つ複数のメソッドはそのクラス、構造体、またはインターフェイス内で一意で宣言するインターフェイスを許可します。</span><span class="sxs-lookup"><span data-stu-id="378d4-364">Overloading of methods permits a class, struct, or interface to declare multiple methods with the same name, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="378d4-365">インスタンス コンス トラクターのオーバー ロードは、そのシグネチャはそのクラスまたは構造体内で一意で提供される、クラスまたは複数のインスタンス コンス トラクターを宣言する構造体をできます。</span><span class="sxs-lookup"><span data-stu-id="378d4-365">Overloading of instance constructors permits a class or struct to declare multiple instance constructors, provided their signatures are unique within that class or struct.</span></span>
*  <span data-ttu-id="378d4-366">そのシグネチャは、クラス、構造体、またはインターフェイス内で一意で提供される、クラス、構造体、または複数のインデクサーを宣言するインターフェイスを許可インデクサーのオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="378d4-366">Overloading of indexers permits a class, struct, or interface to declare multiple indexers, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="378d4-367">演算子のオーバー ロードにより、クラスまたは構造体がそのクラスまたは構造体内で一意で、シグネチャの同じ名前の複数の演算子を宣言します。</span><span class="sxs-lookup"><span data-stu-id="378d4-367">Overloading of operators permits a class or struct to declare multiple operators with the same name, provided their signatures are unique within that class or struct.</span></span>

<span data-ttu-id="378d4-368">`out`と`ref`パラメーター修飾子は、シグネチャの一部と見なされます、1 つの型で宣言されたメンバーがだけでの署名で異なることはできません`ref`と`out`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-368">Although `out` and `ref` parameter modifiers are considered part of a signature, members declared in a single type cannot differ in signature solely by `ref` and `out`.</span></span> <span data-ttu-id="378d4-369">2 つのメンバーが宣言されている場合場合でも同じになるシグネチャを持つ同じ型ですべてのパラメーターを両方の方法で、コンパイル時エラーが発生した`out`に変更された修飾子`ref`修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-369">A compile-time error occurs if two members are declared in the same type with signatures that would be the same if all parameters in both methods with `out` modifiers were changed to `ref` modifiers.</span></span> <span data-ttu-id="378d4-370">シグネチャの一致の他の目的 (非表示やオーバーライドなど)、`ref`と`out`シグネチャの一部と見なされ、他の一致していません。</span><span class="sxs-lookup"><span data-stu-id="378d4-370">For other purposes of signature matching (e.g., hiding or overriding), `ref` and `out` are considered part of the signature and do not match each other.</span></span> <span data-ttu-id="378d4-371">(この制限は、許可するC#実行で、共通言語基盤 (CLI) でのみ異なるメソッドを定義する方法を提供しないに簡単に変換するプログラム`ref`と`out`.)</span><span class="sxs-lookup"><span data-stu-id="378d4-371">(This restriction is to allow C#  programs to be easily translated to run on the Common Language Infrastructure (CLI), which does not provide a way to define methods that differ solely in `ref` and `out`.)</span></span>

<span data-ttu-id="378d4-372">型の署名のため`object`と`dynamic`同じと見なされます。</span><span class="sxs-lookup"><span data-stu-id="378d4-372">For the purposes of signatures, the types `object` and `dynamic` are considered the same.</span></span> <span data-ttu-id="378d4-373">1 つの型で宣言されたメンバーできますしたがってされませんが異なるシグネチャでのみ`object`と`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-373">Members declared in a single type can therefore not differ in signature solely by `object` and `dynamic`.</span></span>

<span data-ttu-id="378d4-374">次の例では、一連のオーバー ロードされたメソッドの宣言とそのシグネチャを示します。</span><span class="sxs-lookup"><span data-stu-id="378d4-374">The following example shows a set of overloaded method declarations along with their signatures.</span></span>
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

<span data-ttu-id="378d4-375">注意してください`ref`と`out`パラメーター修飾子 ([メソッド パラメーター](classes.md#method-parameters)) は、署名の一部です。</span><span class="sxs-lookup"><span data-stu-id="378d4-375">Note that any `ref` and `out` parameter modifiers ([Method parameters](classes.md#method-parameters)) are part of a signature.</span></span> <span data-ttu-id="378d4-376">したがって、`F(int)`と`F(ref int)`は一意の署名。</span><span class="sxs-lookup"><span data-stu-id="378d4-376">Thus, `F(int)` and `F(ref int)` are unique signatures.</span></span> <span data-ttu-id="378d4-377">ただし、`F(ref int)`と`F(out int)`、シグネチャがによって異なるため、同じインターフェイス内で宣言することはできません`ref`と`out`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-377">However, `F(ref int)` and `F(out int)` cannot be declared within the same interface because their signatures differ solely by `ref` and `out`.</span></span> <span data-ttu-id="378d4-378">また、戻り値の型を注と`params`修飾子に含まれていない、シグネチャの戻り値の型または包含または除外のみに基づいてオーバー ロードすることはできませんので、`params`修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-378">Also, note that the return type and the `params` modifier are not part of a signature, so it is not possible to overload solely based on return type or on the inclusion or exclusion of the `params` modifier.</span></span> <span data-ttu-id="378d4-379">これらのメソッドの宣言として`F(int)`と`F(params string[])`上に示したコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-379">As such, the declarations of the methods `F(int)` and `F(params string[])` identified above result in a compile-time error.</span></span>

## <a name="scopes"></a><span data-ttu-id="378d4-380">スコープ</span><span class="sxs-lookup"><span data-stu-id="378d4-380">Scopes</span></span>

<span data-ttu-id="378d4-381">***スコープ***の名前が名前の修飾なしの名前で宣言されたエンティティを参照する、プログラム テキストの範囲。</span><span class="sxs-lookup"><span data-stu-id="378d4-381">The ***scope*** of a name is the region of program text within which it is possible to refer to the entity declared by the name without qualification of the name.</span></span> <span data-ttu-id="378d4-382">スコープは、***入れ子になった***、内側のスコープを再宣言することがあります、外部スコープから名前の意味と (これは、ただし、削除されませんによって課される制限[宣言](basic-concepts.md#declarations)入れ子になったブロック内ではありません可能な限り、それを囲むブロック内のローカル変数と同じ名前でローカル変数を宣言する)。</span><span class="sxs-lookup"><span data-stu-id="378d4-382">Scopes can be ***nested***, and an inner scope may redeclare the meaning of a name from an outer scope (this does not, however, remove the restriction imposed by [Declarations](basic-concepts.md#declarations) that within a nested block it is not possible to declare a local variable with the same name as a local variable in an enclosing block).</span></span> <span data-ttu-id="378d4-383">外側のスコープの名前にしすることを言います***隠し***プログラムのリージョン内のテキストが内側のスコープでカバーし、外部名へのアクセスは、名前を修飾することによってのみ可能です。</span><span class="sxs-lookup"><span data-stu-id="378d4-383">The name from the outer scope is then said to be ***hidden*** in the region of program text covered by the inner scope, and access to the outer name is only possible by qualifying the name.</span></span>

*  <span data-ttu-id="378d4-384">宣言された名前空間のメンバーのスコープを*namespace_member_declaration* ([Namespace メンバー](namespaces.md#namespace-members)) それを囲むなしで*namespace_declaration*プログラム全体は、テキスト。</span><span class="sxs-lookup"><span data-stu-id="378d4-384">The scope of a namespace member declared by a *namespace_member_declaration* ([Namespace members](namespaces.md#namespace-members)) with no enclosing *namespace_declaration* is the entire program text.</span></span>
*  <span data-ttu-id="378d4-385">宣言された名前空間のメンバーのスコープを*namespace_member_declaration*内、 *namespace_declaration*の完全修飾名は`N`は、 *namespace_body*のすべて*namespace_declaration*の完全修飾名は`N`で始まるまたは`N`とピリオドが続く。</span><span class="sxs-lookup"><span data-stu-id="378d4-385">The scope of a namespace member declared by a *namespace_member_declaration* within a *namespace_declaration* whose fully qualified name is `N` is the *namespace_body* of every *namespace_declaration* whose fully qualified name is `N` or starts with `N`, followed by a period.</span></span>
*  <span data-ttu-id="378d4-386">によって定義された名前のスコープ、 *extern_alias_directive*にわたる、 *using_directive*s、 *global_attributes*と*namespace_member_宣言*s がすぐに親のコンパイル単位または名前空間本文。</span><span class="sxs-lookup"><span data-stu-id="378d4-386">The scope of name defined by an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="378d4-387">*Extern_alias_directive*基になる宣言領域に新しいメンバーは含まれません。</span><span class="sxs-lookup"><span data-stu-id="378d4-387">An *extern_alias_directive* does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="378d4-388">つまり、 *extern_alias_directive* 、推移的ではありませんが、コンパイル単位または名前空間本文のみが発生するのではなく、影響を与えます。</span><span class="sxs-lookup"><span data-stu-id="378d4-388">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>
*  <span data-ttu-id="378d4-389">名前のスコープが定義されているまたはによってインポートされた、 *using_directive* ([ディレクティブを使用して](namespaces.md#using-directives)) にわたる、 *namespace_member_declaration*の s、 *compilation_unit*または*namespace_body*を*using_directive*に発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-389">The scope of a name defined or imported by a *using_directive* ([Using directives](namespaces.md#using-directives)) extends over the *namespace_member_declaration*s of the *compilation_unit* or *namespace_body* in which the *using_directive* occurs.</span></span> <span data-ttu-id="378d4-390">A *using_directive* 0 個以上の名前空間、型またはメンバー名を特定内で使用できるように*compilation_unit*または*namespace_body*、しませんが、基になる宣言領域に新しいメンバーを追加します。</span><span class="sxs-lookup"><span data-stu-id="378d4-390">A *using_directive* may make zero or more namespace, type or member names available within a particular *compilation_unit* or *namespace_body*, but does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="378d4-391">つまり、 *using_directive*は推移的ではありませんが、代わりにのみ影響、 *compilation_unit*または*namespace_body*それが出現します。</span><span class="sxs-lookup"><span data-stu-id="378d4-391">In other words, a *using_directive* is not transitive but rather affects only the *compilation_unit* or *namespace_body* in which it occurs.</span></span>
*  <span data-ttu-id="378d4-392">宣言された型パラメーターのスコープを*type_parameter_list*上、 *class_declaration* ([クラス クラスの宣言](classes.md#class-declarations)) は、 *class_base*、 *type_parameter_constraints_clause*秒、および*class_body*その*class_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-392">The scope of a type parameter declared by a *type_parameter_list* on a *class_declaration* ([Class declarations](classes.md#class-declarations)) is the *class_base*, *type_parameter_constraints_clause*s, and *class_body* of that *class_declaration*.</span></span>
*  <span data-ttu-id="378d4-393">宣言された型パラメーターのスコープを*type_parameter_list*上、 *struct_declaration* ([構造体の宣言](structs.md#struct-declarations)) は、 *struct_interfaces*、 *type_parameter_constraints_clause*秒、および*struct_body*その*struct_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-393">The scope of a type parameter declared by a *type_parameter_list* on a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)) is the *struct_interfaces*, *type_parameter_constraints_clause*s, and *struct_body* of that *struct_declaration*.</span></span>
*  <span data-ttu-id="378d4-394">宣言された型パラメーターのスコープを*type_parameter_list*上、 *interface_declaration* ([インターフェイスの宣言](interfaces.md#interface-declarations)) は、 *interface_base*、 *type_parameter_constraints_clause*秒、および*interface_body*その*interface_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-394">The scope of a type parameter declared by a *type_parameter_list* on an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)) is the *interface_base*, *type_parameter_constraints_clause*s, and *interface_body* of that *interface_declaration*.</span></span>
*  <span data-ttu-id="378d4-395">宣言された型パラメーターのスコープを*type_parameter_list*上、 *delegate_declaration* ([デリゲートの宣言](delegates.md#delegate-declarations)) は、 *return_type*、 *formal_parameter_list*、および*type_parameter_constraints_clause*その s *delegate_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-395">The scope of a type parameter declared by a *type_parameter_list* on a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)) is the *return_type*, *formal_parameter_list*, and *type_parameter_constraints_clause*s of that *delegate_declaration*.</span></span>
*  <span data-ttu-id="378d4-396">宣言されたメンバーのスコープを*class_member_declaration* ([クラス本体](classes.md#class-body)) は、 *class_body*の宣言が発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-396">The scope of a member declared by a *class_member_declaration* ([Class body](classes.md#class-body)) is the *class_body* in which the declaration occurs.</span></span> <span data-ttu-id="378d4-397">クラス メンバーのスコープまで、さらに、 *class_body*これらのアクセシビリティ ドメインに含まれているクラスを派生 ([アクセシビリティ ドメイン](basic-concepts.md#accessibility-domains)) のメンバー。</span><span class="sxs-lookup"><span data-stu-id="378d4-397">In addition, the scope of a class member extends to the *class_body* of those derived classes that are included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>
*  <span data-ttu-id="378d4-398">宣言されたメンバーのスコープを*struct_member_declaration* ([構造体のメンバー](structs.md#struct-members)) は、 *struct_body*の宣言が発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-398">The scope of a member declared by a *struct_member_declaration* ([Struct members](structs.md#struct-members)) is the *struct_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="378d4-399">宣言されたメンバーのスコープ、 *enum_member_declaration* ([列挙型メンバー](enums.md#enum-members)) は、 *enum_body*の宣言が発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-399">The scope of a member declared by an *enum_member_declaration*  ([Enum members](enums.md#enum-members)) is the *enum_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="378d4-400">宣言されたパラメーターのスコープを*method_declaration* ([メソッド](classes.md#methods)) は、 *method_body*その*method_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-400">The scope of a parameter declared in a *method_declaration* ([Methods](classes.md#methods)) is the *method_body* of that *method_declaration*.</span></span>
*  <span data-ttu-id="378d4-401">宣言されたパラメーターのスコープ、 *indexer_declaration* ([インデクサー](classes.md#indexers)) は、 *accessor_declarations*その*indexer_declaration*.</span><span class="sxs-lookup"><span data-stu-id="378d4-401">The scope of a parameter declared in an *indexer_declaration* ([Indexers](classes.md#indexers)) is the *accessor_declarations* of that *indexer_declaration*.</span></span>
*  <span data-ttu-id="378d4-402">宣言されたパラメーターのスコープ、 *operator_declaration* ([演算子](classes.md#operators)) は、*ブロック*その*operator_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-402">The scope of a parameter declared in an *operator_declaration* ([Operators](classes.md#operators)) is the *block* of that *operator_declaration*.</span></span>
*  <span data-ttu-id="378d4-403">宣言されたパラメーターのスコープを*constructor_declaration* ([インスタンス コンス トラクター](classes.md#instance-constructors)) は、 *constructor_initializer*と*ブロック*その*constructor_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-403">The scope of a parameter declared in a *constructor_declaration* ([Instance constructors](classes.md#instance-constructors)) is the *constructor_initializer* and *block* of that *constructor_declaration*.</span></span>
*  <span data-ttu-id="378d4-404">宣言されたパラメーターのスコープを*lambda_expression* ([匿名関数式](expressions.md#anonymous-function-expressions)) は、 *anonymous_function_body*その*lambda_式*</span><span class="sxs-lookup"><span data-stu-id="378d4-404">The scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression*</span></span>
*  <span data-ttu-id="378d4-405">宣言されたパラメーターのスコープ、 *anonymous_method_expression* ([匿名関数式](expressions.md#anonymous-function-expressions)) は、*ブロック*その*anonymous_method_expression*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-405">The scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>
*  <span data-ttu-id="378d4-406">宣言されているラベルのスコープを*labeled_statement* ([というラベルの付いたステートメント](statements.md#labeled-statements)) は、*ブロック*の宣言が発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-406">The scope of a label declared in a *labeled_statement* ([Labeled statements](statements.md#labeled-statements)) is the *block* in which the declaration occurs.</span></span>
*  <span data-ttu-id="378d4-407">宣言されたローカル変数のスコープを*local_variable_declaration* ([ローカル変数宣言](statements.md#local-variable-declarations)) ブロックの宣言が発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-407">The scope of a local variable declared in a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) is the block in which the declaration occurs.</span></span>
*  <span data-ttu-id="378d4-408">宣言されたローカル変数のスコープを*switch_block*の`switch`ステートメント ([switch ステートメント](statements.md#the-switch-statement)) は、 *switch_block*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-408">The scope of a local variable declared in a *switch_block* of a `switch` statement ([The switch statement](statements.md#the-switch-statement)) is the *switch_block*.</span></span>
*  <span data-ttu-id="378d4-409">宣言されたローカル変数のスコープを*for_initializer*の`for`ステートメント ([、ステートメントの](statements.md#the-for-statement)) は、 *for_initializer*、 *for_condition*、 *for_iterator*、格納されていると*ステートメント*の`for`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="378d4-409">The scope of a local variable declared in a *for_initializer* of a `for` statement ([The for statement](statements.md#the-for-statement)) is the *for_initializer*, the *for_condition*, the *for_iterator*, and the contained *statement* of the `for` statement.</span></span>
*  <span data-ttu-id="378d4-410">宣言されたローカル定数のスコープを*local_constant_declaration* ([ローカル定数宣言](statements.md#local-constant-declarations)) ブロックの宣言が発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-410">The scope of a local constant declared in a *local_constant_declaration* ([Local constant declarations](statements.md#local-constant-declarations)) is the block in which the declaration occurs.</span></span> <span data-ttu-id="378d4-411">前にあるテキストの位置でのローカル定数を参照すると、コンパイル時エラーがその*constant_declarator*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-411">It is a compile-time error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span>
*  <span data-ttu-id="378d4-412">変数のスコープが宣言の一部として、 *foreach_statement*、 *using_statement*、 *lock_statement*または*query_expression*は特定の構造の拡張によって決まります。</span><span class="sxs-lookup"><span data-stu-id="378d4-412">The scope of a variable declared as part of a *foreach_statement*, *using_statement*, *lock_statement* or *query_expression* is determined by the expansion of the given construct.</span></span>

<span data-ttu-id="378d4-413">名前空間、クラス、構造体、または列挙型メンバーのスコープ内のメンバーの宣言の前にあるテキストの位置にメンバーを参照することです。</span><span class="sxs-lookup"><span data-stu-id="378d4-413">Within the scope of a namespace, class, struct, or enumeration member it is possible to refer to the member in a textual position that precedes the declaration of the member.</span></span> <span data-ttu-id="378d4-414">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="378d4-414">For example</span></span>
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
<span data-ttu-id="378d4-415">ここでは、それが有効で`F`を参照する`i`宣言されています。</span><span class="sxs-lookup"><span data-stu-id="378d4-415">Here, it is valid for `F` to refer to `i` before it is declared.</span></span>

<span data-ttu-id="378d4-416">前にあるテキストの位置にローカル変数を参照すると、コンパイル時エラーは、ローカル変数のスコープ内で、 *local_variable_declarator*のローカル変数。</span><span class="sxs-lookup"><span data-stu-id="378d4-416">Within the scope of a local variable, it is a compile-time error to refer to the local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="378d4-417">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="378d4-417">For example</span></span>
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

<span data-ttu-id="378d4-418">`F`上記のメソッドは、最初の割り当てを`i`具体的には、外側のスコープで宣言されているフィールドを参照しません。</span><span class="sxs-lookup"><span data-stu-id="378d4-418">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="378d4-419">代わりに、ローカル変数を参照し、代入変数の宣言にコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-419">Rather, it refers to the local variable and it results in a compile-time error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="378d4-420">`G`メソッドは、の使用`j`の宣言の初期化子で`j`ため、使用前に有効では、 *local_variable_declarator*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-420">In the `G` method, the use of `j` in the initializer for the declaration of `j` is valid because the use does not precede the *local_variable_declarator*.</span></span> <span data-ttu-id="378d4-421">`H`メソッドは、後ろに続く*local_variable_declarator*以前で宣言されたローカル変数を正しく指す*local_variable_declarator*内で同じ*local_variable_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-421">In the `H` method, a subsequent *local_variable_declarator* correctly refers to a local variable declared in an earlier *local_variable_declarator* within the same *local_variable_declaration*.</span></span>

<span data-ttu-id="378d4-422">ローカル変数のスコープ規則は、式のコンテキストで使用される名前の意味が常に同じであるブロック内で保証するために設計されています。</span><span class="sxs-lookup"><span data-stu-id="378d4-422">The scoping rules for local variables are designed to guarantee that the meaning of a name used in an expression context is always the same within a block.</span></span> <span data-ttu-id="378d4-423">ローカル変数のスコープ、ブロックの末尾に、宣言からのみを拡張する場合は、上記の例では、最初の代入は、インスタンス変数、2 つ目の代入はローカル変数につながる可能性があります。ブロックのステートメントが再整列される以降の場合のコンパイル時エラー。</span><span class="sxs-lookup"><span data-stu-id="378d4-423">If the scope of a local variable were to extend only from its declaration to the end of the block, then in the example above, the first assignment would assign to the instance variable and the second assignment would assign to the local variable, possibly leading to compile-time errors if the statements of the block were later to be rearranged.</span></span>

<span data-ttu-id="378d4-424">ブロック内で名前の意味は、名前が使用されるコンテキストに応じて異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-424">The meaning of a name within a block may differ based on the context in which the name is used.</span></span> <span data-ttu-id="378d4-425">例</span><span class="sxs-lookup"><span data-stu-id="378d4-425">In the example</span></span>
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
<span data-ttu-id="378d4-426">名前`A`式のコンテキストでローカル変数を参照するために使用`A`と、クラスを参照する型のコンテキストで`A`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-426">the name `A` is used in an expression context to refer to the local variable `A` and in a type context to refer to the class `A`.</span></span>

### <a name="name-hiding"></a><span data-ttu-id="378d4-427">名前の隠ぺい</span><span class="sxs-lookup"><span data-stu-id="378d4-427">Name hiding</span></span>

<span data-ttu-id="378d4-428">通常、エンティティのスコープには、エンティティの宣言領域よりも多くのプログラム テキストが含まれます。</span><span class="sxs-lookup"><span data-stu-id="378d4-428">The scope of an entity typically encompasses more program text than the declaration space of the entity.</span></span> <span data-ttu-id="378d4-429">具体的には、エンティティのスコープは、同じ名前のエンティティを含む新しい宣言空間を導入する宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="378d4-429">In particular, the scope of an entity may include declarations that introduce new declaration spaces containing entities of the same name.</span></span> <span data-ttu-id="378d4-430">このような宣言により、元のエンティティになる***隠し***します。</span><span class="sxs-lookup"><span data-stu-id="378d4-430">Such declarations cause the original entity to become ***hidden***.</span></span> <span data-ttu-id="378d4-431">逆に、エンティティがあると言えます***表示***非表示しない場合。</span><span class="sxs-lookup"><span data-stu-id="378d4-431">Conversely, an entity is said to be ***visible*** when it is not hidden.</span></span>

<span data-ttu-id="378d4-432">名前の非表示には、スコープが入れ子スコープが継承によるオーバー ラップしてが重複する場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-432">Name hiding occurs when scopes overlap through nesting and when scopes overlap through inheritance.</span></span> <span data-ttu-id="378d4-433">次のセクションでは、2 種類の非表示の特性を説明します。</span><span class="sxs-lookup"><span data-stu-id="378d4-433">The characteristics of the two types of hiding are described in the following sections.</span></span>

#### <a name="hiding-through-nesting"></a><span data-ttu-id="378d4-434">入れ子を非表示</span><span class="sxs-lookup"><span data-stu-id="378d4-434">Hiding through nesting</span></span>

<span data-ttu-id="378d4-435">入れ子による名前の隠ぺいは、名前空間またはクラスまたは構造体、およびパラメーターやローカル変数の宣言型の入れ子の結果として、名前空間内の型の入れ子の結果として発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-435">Name hiding through nesting can occur as a result of nesting namespaces or types within namespaces, as a result of nesting types within classes or structs, and as a result of parameter and local variable declarations.</span></span>

<span data-ttu-id="378d4-436">例</span><span class="sxs-lookup"><span data-stu-id="378d4-436">In the example</span></span>
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
<span data-ttu-id="378d4-437">内で、`F`メソッドは、インスタンス変数`i`ローカル変数によって隠されている`i`が内、`G`メソッド、`i`インスタンス変数を参照します。</span><span class="sxs-lookup"><span data-stu-id="378d4-437">within the `F` method, the instance variable `i` is hidden by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

<span data-ttu-id="378d4-438">内側のスコープに名前が外側のスコープでの名前を非表示にすると、その名前のすべてのオーバー ロードされた出現箇所は非表示になります。</span><span class="sxs-lookup"><span data-stu-id="378d4-438">When a name in an inner scope hides a name in an outer scope, it hides all overloaded occurrences of that name.</span></span> <span data-ttu-id="378d4-439">例</span><span class="sxs-lookup"><span data-stu-id="378d4-439">In the example</span></span>
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
<span data-ttu-id="378d4-440">呼び出し`F(1)`呼び出す、`F`で宣言されている`Inner`ため、外部出現するすべての`F`内部宣言では表示されません。</span><span class="sxs-lookup"><span data-stu-id="378d4-440">the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="378d4-441">同じ理由、呼び出しから`F("Hello")`コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-441">For the same reason, the call `F("Hello")` results in a compile-time error.</span></span>

#### <a name="hiding-through-inheritance"></a><span data-ttu-id="378d4-442">継承によって非表示</span><span class="sxs-lookup"><span data-stu-id="378d4-442">Hiding through inheritance</span></span>

<span data-ttu-id="378d4-443">継承による名前の隠ぺいは、クラスまたは構造体の基本クラスから継承した名前を再宣言するときに発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-443">Name hiding through inheritance occurs when classes or structs redeclare names that were inherited from base classes.</span></span> <span data-ttu-id="378d4-444">この種類の名前の隠ぺいは、次の種類のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="378d4-444">This type of name hiding takes one of the following forms:</span></span>

*  <span data-ttu-id="378d4-445">定数、フィールド、プロパティ、イベント、またはクラスまたは構造体で導入された型と同じ名前のすべての基底クラス メンバーを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="378d4-445">A constant, field, property, event, or type introduced in a class or struct hides all base class members with the same name.</span></span>
*  <span data-ttu-id="378d4-446">クラスまたは構造体で導入されたメソッドには、同じ名前を持つすべてのメソッド以外の基本クラス メンバーと (メソッド名とパラメーターの数、修飾子、および種類) は、同じシグネチャを持つすべての基底クラス メソッドが非表示にします。</span><span class="sxs-lookup"><span data-stu-id="378d4-446">A method introduced in a class or struct hides all non-method base class members with the same name, and all base class methods with the same signature (method name and parameter count, modifiers, and types).</span></span>
*  <span data-ttu-id="378d4-447">クラスまたは構造体で導入されたインデクサーには、(パラメーターの数と型) は、同じシグネチャを持つすべての基底クラス インデクサーが非表示にします。</span><span class="sxs-lookup"><span data-stu-id="378d4-447">An indexer introduced in a class or struct hides all base class indexers with the same signature (parameter count and types).</span></span>

<span data-ttu-id="378d4-448">演算子の宣言に関する規則 ([演算子](classes.md#operators)) 基本クラスでは演算子として同じシグネチャを持つ演算子を宣言する派生クラスを使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="378d4-448">The rules governing operator declarations ([Operators](classes.md#operators)) make it impossible for a derived class to declare an operator with the same signature as an operator in a base class.</span></span> <span data-ttu-id="378d4-449">そのため、演算子が互いの演算子を隠ぺいすることはありません。</span><span class="sxs-lookup"><span data-stu-id="378d4-449">Thus, operators never hide one another.</span></span>

<span data-ttu-id="378d4-450">外側のスコープから名前を非表示とは異なりは、継承したスコープからアクセス可能な名前を非表示と、警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-450">Contrary to hiding a name from an outer scope, hiding an accessible name from an inherited scope causes a warning to be reported.</span></span> <span data-ttu-id="378d4-451">例</span><span class="sxs-lookup"><span data-stu-id="378d4-451">In the example</span></span>
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
<span data-ttu-id="378d4-452">宣言`F`で`Derived`により警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-452">the declaration of `F` in `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="378d4-453">継承された名前を非表示でない具体的には、エラーを基底クラスの個別の進化を来たすため。</span><span class="sxs-lookup"><span data-stu-id="378d4-453">Hiding an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="378d4-454">たとえば、上記のような状況が発生する以降のバージョンの`Base`導入された、`F`以前のバージョンのクラスには存在しなかったメソッド。</span><span class="sxs-lookup"><span data-stu-id="378d4-454">For example, the above situation might have come about because a later version of `Base` introduced an `F` method that wasn't present in an earlier version of the class.</span></span> <span data-ttu-id="378d4-455">上記のような状況は、エラーになっていた、個別にバージョン管理されたクラス ライブラリで基底クラスへの変更可能性がある可能性が派生クラスを無効になります。</span><span class="sxs-lookup"><span data-stu-id="378d4-455">Had the above situation been an error, then any change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="378d4-456">使用して、継承された名前を非表示にして警告を取り除くことができます、`new`修飾子。</span><span class="sxs-lookup"><span data-stu-id="378d4-456">The warning caused by hiding an inherited name can be eliminated through use of the `new` modifier:</span></span>
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

<span data-ttu-id="378d4-457">`new`修飾子には、ことを示します、`F`で`Derived`はこれが実際にものである継承されたメンバーを非表示にして"new"。</span><span class="sxs-lookup"><span data-stu-id="378d4-457">The `new` modifier indicates that the `F` in `Derived` is "new", and that it is indeed intended to hide the inherited member.</span></span>

<span data-ttu-id="378d4-458">新しいメンバーの宣言では、新しいメンバーのスコープ内でのみ、継承されたメンバーを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="378d4-458">A declaration of a new member hides an inherited member only within the scope of the new member.</span></span>

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

<span data-ttu-id="378d4-459">宣言の前の例では`F`で`Derived`を非表示に、`F`から継承されている`Base`、ただし、新しい`F`で`Derived`プライベート アクセスは、そのスコープには適用されません`MoreDerived`.</span><span class="sxs-lookup"><span data-stu-id="378d4-459">In the example above, the declaration of `F` in `Derived` hides the `F` that was inherited from `Base`, but since the new `F` in `Derived` has private access, its scope does not extend to `MoreDerived`.</span></span> <span data-ttu-id="378d4-460">したがって、呼び出し`F()`で`MoreDerived.G`が有効では呼び出す`Base.F`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-460">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span>

## <a name="namespace-and-type-names"></a><span data-ttu-id="378d4-461">Namespace と型の名前</span><span class="sxs-lookup"><span data-stu-id="378d4-461">Namespace and type names</span></span>

<span data-ttu-id="378d4-462">内のいくつかのコンテキストをC#プログラムが必要な*namespace_name*または*type_name*を指定します。</span><span class="sxs-lookup"><span data-stu-id="378d4-462">Several contexts in a C# program require a *namespace_name* or a *type_name* to be specified.</span></span>

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

<span data-ttu-id="378d4-463">A *namespace_name*は、 *namespace_or_type_name*名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="378d4-463">A *namespace_name* is a *namespace_or_type_name* that refers to a namespace.</span></span> <span data-ttu-id="378d4-464">以下に示すように、解像度を次の*namespace_or_type_name*の*namespace_name* 、名前空間を参照する必要がありますそれ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-464">Following resolution as described below, the *namespace_or_type_name* of a *namespace_name* must refer to a namespace, or otherwise a compile-time error occurs.</span></span> <span data-ttu-id="378d4-465">型引数なし ([引数を入力](types.md#type-arguments)) で使用できる、 *namespace_name* (専用の種類には、型引数を指定できます)。</span><span class="sxs-lookup"><span data-stu-id="378d4-465">No type arguments ([Type arguments](types.md#type-arguments)) can be present in a *namespace_name* (only types can have type arguments).</span></span>

<span data-ttu-id="378d4-466">A *type_name*は、 *namespace_or_type_name*型を参照します。</span><span class="sxs-lookup"><span data-stu-id="378d4-466">A *type_name* is a *namespace_or_type_name* that refers to a type.</span></span> <span data-ttu-id="378d4-467">以下に示すように、解像度を次、 *namespace_or_type_name*の*type_name*型を参照する必要がありますそれ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-467">Following resolution as described below, the *namespace_or_type_name* of a *type_name* must refer to a type, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="378d4-468">場合、 *namespace_or_type_name*修飾-エイリアス - メンバーの意味は」の説明に従って[Namespace エイリアス修飾子](namespaces.md#namespace-alias-qualifiers)します。</span><span class="sxs-lookup"><span data-stu-id="378d4-468">If the *namespace_or_type_name* is a qualified-alias-member its meaning is as described in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span> <span data-ttu-id="378d4-469">それ以外の場合、 *namespace_or_type_name*が 4 つの形式のいずれか。</span><span class="sxs-lookup"><span data-stu-id="378d4-469">Otherwise, a *namespace_or_type_name* has one of four forms:</span></span>

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

<span data-ttu-id="378d4-470">場所`I`、単一の識別子は、`N`は、 *namespace_or_type_name*と`<A1, ..., Ak>`は省略可能な*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="378d4-470">where `I` is a single identifier, `N` is a *namespace_or_type_name* and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="378d4-471">ない場合*type_argument_list*が指定するを検討してください`k`を 0 にします。</span><span class="sxs-lookup"><span data-stu-id="378d4-471">When no *type_argument_list* is specified, consider `k` to be zero.</span></span>

<span data-ttu-id="378d4-472">意味を*namespace_or_type_name*は次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-472">The meaning of a *namespace_or_type_name* is determined as follows:</span></span>

*   <span data-ttu-id="378d4-473">場合、 *namespace_or_type_name*の形式は`I`またはフォームの`I<A1, ..., Ak>`:</span><span class="sxs-lookup"><span data-stu-id="378d4-473">If the *namespace_or_type_name* is of the form `I` or of the form `I<A1, ..., Ak>`:</span></span>
    * <span data-ttu-id="378d4-474">場合`K`ゼロ、 *namespace_or_type_name*ジェネリック メソッドの宣言内で表示されます ([メソッド](classes.md#methods)) し、その宣言には、型パラメーターが含まれている場合 ([型パラメーター](classes.md#type-parameters)) 名前を持つ `I`、 *namespace_or_type_name*その型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="378d4-474">If `K` is zero and the *namespace_or_type_name* appears within a generic method declaration ([Methods](classes.md#methods)) and if that declaration includes a type parameter ([Type parameters](classes.md#type-parameters)) with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
    * <span data-ttu-id="378d4-475">の場合、 *namespace_or_type_name*型の宣言内で次の各インスタンス タイプが表示されます `T`([インスタンス型](classes.md#the-instance-type))、その型のインスタンスの型宣言と各外側のクラスまたは構造体の宣言のインスタンスの型 (存在する場合) を続行します。</span><span class="sxs-lookup"><span data-stu-id="378d4-475">Otherwise, if the *namespace_or_type_name* appears within a type declaration, then for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of that type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
        * <span data-ttu-id="378d4-476">場合`K`は 0 との宣言`T`名前の型パラメーターを含む `I`、 *namespace_or_type_name*その型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="378d4-476">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
        * <span data-ttu-id="378d4-477">の場合、 *namespace_or_type_name*型の宣言の本文内に表示し、`T`またはその基本型の名前を持つ入れ子になったアクセス可能な型が含まれている `I`と`K`  パラメーターを入力し、 *namespace_or_type_name*は特定の型引数を使用して構築する型を表します。</span><span class="sxs-lookup"><span data-stu-id="378d4-477">Otherwise, if the *namespace_or_type_name* appears within the body of the type declaration, and `T` or any of its base types contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="378d4-478">このような 1 つ以上の型がある場合より強い派生型で宣言された型が選択されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-478">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="378d4-479">意味を決定するときに、非型のメンバー (定数、フィールド、メソッド、プロパティ、インデクサー、演算子、インスタンス コンス トラクター、デストラクター、および静的コンス トラクター) と型パラメーターの数が異なる型のメンバーが無視されることに注意してください、*namespace_or_type_name*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-479">Note that non-type members (constants, fields, methods, properties, indexers, operators, instance constructors, destructors, and static constructors) and type members with a different number of type parameters are ignored when determining the meaning of the *namespace_or_type_name*.</span></span>
    * <span data-ttu-id="378d4-480">かどうか、前の手順を各名前空間の次に、失敗しました `N`を名前空間を以降の*namespace_or_type_name*外側にある各名前空間 (存在する場合) を続行し、終わるが発生した、グローバル名前空間、エンティティが見つかるまで、次の手順が評価されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-480">If the previous steps were unsuccessful then, for each namespace `N`, starting with the namespace in which the *namespace_or_type_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
        * <span data-ttu-id="378d4-481">場合`K`ゼロと`I`で名前空間の名前を指定 `N`、し。</span><span class="sxs-lookup"><span data-stu-id="378d4-481">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
            * <span data-ttu-id="378d4-482">場合、場所を*namespace_or_type_name*が発生した名前空間の宣言で囲まれた`N`名前空間宣言が含まれています、 *extern_alias_directive*または*using_alias_directive*名前に関連付ける `I`型、または名前空間、 *namespace_or_type_name*があいまいですし、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-482">If the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="378d4-483">それ以外の場合、 *namespace_or_type_name*という名前の名前空間を指す`I`で`N`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-483">Otherwise, the *namespace_or_type_name* refers to the namespace named `I` in `N`.</span></span>
        * <span data-ttu-id="378d4-484">の場合`N`名前を持つアクセス可能な型が含まれています `I`と`K` し、パラメーターを入力します。</span><span class="sxs-lookup"><span data-stu-id="378d4-484">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
            * <span data-ttu-id="378d4-485">場合`K`は 0 と場所を*namespace_or_type_name*が発生した名前空間の宣言で囲まれた`N`名前空間宣言が含まれています、 *extern_alias_directive*または*using_alias_directive*名前に関連付ける `I`型、または名前空間、 *namespace_or_type_name*あいまいな、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-485">If `K` is zero and the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="378d4-486">それ以外の場合、 *namespace_or_type_name*は特定の型引数を使用して構築型を表します。</span><span class="sxs-lookup"><span data-stu-id="378d4-486">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
        * <span data-ttu-id="378d4-487">の場合、場所を、 *namespace_or_type_name*が発生した名前空間の宣言で囲まれた`N`:</span><span class="sxs-lookup"><span data-stu-id="378d4-487">Otherwise, if the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
            * <span data-ttu-id="378d4-488">場合`K`がゼロで、名前空間宣言が含まれています、 *extern_alias_directive*または*using_alias_directive*名前に関連付ける `I`でインポートされた名前空間または型、 *namespace_or_type_name*その名前空間または型を参照します。</span><span class="sxs-lookup"><span data-stu-id="378d4-488">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *namespace_or_type_name* refers to that namespace or type.</span></span>
            * <span data-ttu-id="378d4-489">それ以外の場合、名前空間と型の宣言をインポートした場合、 *using_namespace_directive*s と*using_alias_directive*名前空間宣言の s がアクセス可能な型の 1 つだけを含める名前を持つ `I`と`K` パラメーター、入力、 *namespace_or_type_name*は特定の型引数を使用して構築する型を表します。</span><span class="sxs-lookup"><span data-stu-id="378d4-489">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain exactly one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
            * <span data-ttu-id="378d4-490">それ以外の場合、名前空間と型の宣言をインポートした場合、 *using_namespace_directive*s と*using_alias_directive*名前空間の宣言の 1 つ以上のアクセス可能な型を含めることが名前を持つ `I`と`K` パラメーター、入力、 *namespace_or_type_name*があいまい、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-490">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain more than one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* is ambiguous and an error occurs.</span></span>
    * <span data-ttu-id="378d4-491">それ以外の場合、 *namespace_or_type_name*は未定義となり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-491">Otherwise, the *namespace_or_type_name* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="378d4-492">それ以外の場合、 *namespace_or_type_name*の形式は`N.I`またはフォームの`N.I<A1, ..., Ak>`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-492">Otherwise, the *namespace_or_type_name* is of the form `N.I` or of the form `N.I<A1, ..., Ak>`.</span></span> <span data-ttu-id="378d4-493">`N` として解決は、まず、 *namespace_or_type_name*します。</span><span class="sxs-lookup"><span data-stu-id="378d4-493">`N` is first resolved as a *namespace_or_type_name*.</span></span> <span data-ttu-id="378d4-494">場合の解像度`N`が成功すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="378d4-494">If the resolution of `N` is not successful, a compile-time error occurs.</span></span> <span data-ttu-id="378d4-495">それ以外の場合、`N.I`または`N.I<A1, ..., Ak>`次のように解決されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-495">Otherwise, `N.I` or `N.I<A1, ..., Ak>` is resolved as follows:</span></span>
    * <span data-ttu-id="378d4-496">場合`K`ゼロと`N`参照名前空間と`N`入れ子になった名前空間が含まれています`I`、 *namespace_or_type_name*その入れ子になった名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="378d4-496">If `K` is zero and `N` refers to a namespace and `N` contains a nested namespace with name `I`, then the *namespace_or_type_name* refers to that nested namespace.</span></span>
    * <span data-ttu-id="378d4-497">の場合`N`参照名前空間と`N`名前を持つアクセス可能な型が含まれています `I`と`K` パラメーターを入力し、 *namespace_or_type_name*参照その型指定された型引数を使用して構築します。</span><span class="sxs-lookup"><span data-stu-id="378d4-497">Otherwise, if `N` refers to a namespace and `N` contains an accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
    * <span data-ttu-id="378d4-498">の場合`N`(場合によって構築された) クラスまたは構造体の型を参照し、`N`またはその基本クラスの名前を持つ入れ子になったアクセス可能な型が含まれている `I`と`K`  、パラメーターの型*namespace_or_type_name*は特定の型引数を使用して構築する型を表します。</span><span class="sxs-lookup"><span data-stu-id="378d4-498">Otherwise, if `N` refers to a (possibly constructed) class or struct type and `N` or any of its base classes contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="378d4-499">このような 1 つ以上の型がある場合より強い派生型で宣言された型が選択されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-499">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="378d4-500">場合の意味に注意してください`N.I`解決の基底クラスの指定の一部として特定する複素数が`N`の直接の基本クラスから`N`オブジェクトと見なされます ([基底クラス](classes.md#base-classes))。</span><span class="sxs-lookup"><span data-stu-id="378d4-500">Note that if the meaning of `N.I` is being determined as part of resolving the base class specification of `N` then the direct base class of `N` is considered to be object ([Base classes](classes.md#base-classes)).</span></span>
    * <span data-ttu-id="378d4-501">それ以外の場合、`N.I`は無効な*namespace_or_type_name*コンパイル時エラーが発生したとします。</span><span class="sxs-lookup"><span data-stu-id="378d4-501">Otherwise, `N.I` is an invalid *namespace_or_type_name*, and a compile-time error occurs.</span></span>

<span data-ttu-id="378d4-502">A *namespace_or_type_name*静的クラスの参照が許可されている ([静的クラス](classes.md#static-classes)) 場合にのみ</span><span class="sxs-lookup"><span data-stu-id="378d4-502">A *namespace_or_type_name* is permitted to reference a static class ([Static classes](classes.md#static-classes)) only if</span></span>

*  <span data-ttu-id="378d4-503">*Namespace_or_type_name*は、`T`で、 *namespace_or_type_name*フォームの`T.I`、または</span><span class="sxs-lookup"><span data-stu-id="378d4-503">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="378d4-504">*Namespace_or_type_name*は、`T`で、 *typeof_expression* ([引数リスト](expressions.md#argument-lists)1) フォームの`typeof(T)`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-504">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

### <a name="fully-qualified-names"></a><span data-ttu-id="378d4-505">完全修飾名</span><span class="sxs-lookup"><span data-stu-id="378d4-505">Fully qualified names</span></span>

<span data-ttu-id="378d4-506">すべての名前空間と型に、***完全修飾名***、名前空間または他のすべてのユーザーの間で型を一意に識別します。</span><span class="sxs-lookup"><span data-stu-id="378d4-506">Every namespace and type has a ***fully qualified name***, which uniquely identifies the namespace or type amongst all others.</span></span> <span data-ttu-id="378d4-507">名前空間または型の完全修飾名`N`は次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-507">The fully qualified name of a namespace or type `N` is determined as follows:</span></span>

*  <span data-ttu-id="378d4-508">場合`N`メンバーは、その完全修飾名には、グローバル名前空間の`N`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-508">If `N` is a member of the global namespace, its fully qualified name is `N`.</span></span>
*  <span data-ttu-id="378d4-509">その完全修飾名は、それ以外の場合、`S.N`ここで、 `S` 、名前空間の種類の完全修飾名は、`N`は宣言されています。</span><span class="sxs-lookup"><span data-stu-id="378d4-509">Otherwise, its fully qualified name is `S.N`, where `S` is the fully qualified name of the namespace or type in which `N` is declared.</span></span>

<span data-ttu-id="378d4-510">完全修飾名、つまり`N`につながる識別子の完全な階層パスは、 `N`、グローバル名前空間から開始します。</span><span class="sxs-lookup"><span data-stu-id="378d4-510">In other words, the fully qualified name of `N` is the complete hierarchical path of identifiers that lead to `N`, starting from the global namespace.</span></span> <span data-ttu-id="378d4-511">名前空間または型のすべてのメンバーには、一意の名前を持つ必要があります、ために、名前空間または型の完全修飾名が常に一意であることに従います。</span><span class="sxs-lookup"><span data-stu-id="378d4-511">Because every member of a namespace or type must have a unique name, it follows that the fully qualified name of a namespace or type is always unique.</span></span>

<span data-ttu-id="378d4-512">次の例では、関連付けられている、完全修飾名と名前空間と型の宣言をいくつかを示します。</span><span class="sxs-lookup"><span data-stu-id="378d4-512">The example below shows several namespace and type declarations along with their associated fully qualified names.</span></span>
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

## <a name="automatic-memory-management"></a><span data-ttu-id="378d4-513">自動メモリ管理</span><span class="sxs-lookup"><span data-stu-id="378d4-513">Automatic memory management</span></span>

<span data-ttu-id="378d4-514">C# 開発者は手動での割り当てとオブジェクトによって占有されているメモリを解放してから、自動メモリ管理を採用しています。</span><span class="sxs-lookup"><span data-stu-id="378d4-514">C# employs automatic memory management, which frees developers from manually allocating and freeing the memory occupied by objects.</span></span> <span data-ttu-id="378d4-515">自動メモリ管理ポリシーがによって実装される、***ガベージ コレクター***します。</span><span class="sxs-lookup"><span data-stu-id="378d4-515">Automatic memory management policies are implemented by a ***garbage collector***.</span></span> <span data-ttu-id="378d4-516">オブジェクトのメモリ管理のライフ サイクルは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="378d4-516">The memory management life cycle of an object is as follows:</span></span>

1. <span data-ttu-id="378d4-517">オブジェクトが作成されたときに、メモリが割り当てられたし、コンス トラクターを実行すると、オブジェクトは、ライブと見なされます。</span><span class="sxs-lookup"><span data-stu-id="378d4-517">When the object is created, memory is allocated for it, the constructor is run, and the object is considered live.</span></span>
2. <span data-ttu-id="378d4-518">場合は、オブジェクトまたはその一部は、実行可能な継続がアクセスできない、デストラクターの実行以外オブジェクトと見なされ不要になった使用中で破壊の対象になります。</span><span class="sxs-lookup"><span data-stu-id="378d4-518">If the object, or any part of it, cannot be accessed by any possible continuation of execution, other than the running of destructors, the object is considered no longer in use, and it becomes eligible for destruction.</span></span> <span data-ttu-id="378d4-519">C# コンパイラと、ガベージ コレクターは、オブジェクトへの参照は、今後使用される可能性がありますを決定するコードを分析できます。</span><span class="sxs-lookup"><span data-stu-id="378d4-519">The C# compiler and the garbage collector may choose to analyze code to determine which references to an object may be used in the future.</span></span> <span data-ttu-id="378d4-520">たとえば、スコープ内のローカル変数は、オブジェクトを唯一の参照から、現在実行中の実行の手順でポイントをそのローカル変数が参照されない場合は、ガベージ コレクターについて、可能性があります (がないです。ために必要) 使用されていないと、オブジェクトを処理します。</span><span class="sxs-lookup"><span data-stu-id="378d4-520">For instance, if a local variable that is in scope is the only existing reference to an object, but that local variable is never referred to in any possible continuation of execution from the current execution point in the procedure, the garbage collector may (but is not required to) treat the object as no longer in use.</span></span>
3. <span data-ttu-id="378d4-521">時間、デストラクターを後で指定されていない一部のオブジェクトが破壊の対象とすると ([デストラクター](classes.md#destructors)) (あれば) のオブジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="378d4-521">Once the object is eligible for destruction, at some unspecified later time the destructor ([Destructors](classes.md#destructors)) (if any) for the object is run.</span></span> <span data-ttu-id="378d4-522">通常の状況で、オブジェクトのデストラクターは実装に固有の Api は、オーバーライドするには、この動作を使用する可能性がありますが 1 回だけ実行されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-522">Under normal circumstances the destructor for the object is run once only, though implementation-specific APIs may allow this behavior to be overridden.</span></span>
4. <span data-ttu-id="378d4-523">デストラクターの実行を含む実行可能な継続して、そのオブジェクトまたはその一部にアクセスできない場合、オブジェクトのデストラクターが実行されると、オブジェクトがアクセスできないと見なされます、オブジェクトがコレクションの対象になります。</span><span class="sxs-lookup"><span data-stu-id="378d4-523">Once the destructor for an object is run, if that object, or any part of it, cannot be accessed by any possible continuation of execution, including the running of destructors, the object is considered inaccessible and the object becomes eligible for collection.</span></span>
5. <span data-ttu-id="378d4-524">最後に、いくつかの時点で、オブジェクトがコレクションの対象になると、ガベージ コレクターが解放そのオブジェクトに関連付けられているメモリ。</span><span class="sxs-lookup"><span data-stu-id="378d4-524">Finally, at some time after the object becomes eligible for collection, the garbage collector frees the memory associated with that object.</span></span>

<span data-ttu-id="378d4-525">ガベージ コレクターは、オブジェクトの使用方法に関する情報を保持し、メモリ管理、決定を行うなど、オブジェクト、およびオブジェクトを再配置がなくなったときに使用されていたり、新しく作成されたオブジェクトを検索するためのメモリ内の場所はこの情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="378d4-525">The garbage collector maintains information about object usage, and uses this information to make memory management decisions, such as where in memory to locate a newly created object, when to relocate an object, and when an object is no longer in use or inaccessible.</span></span>

<span data-ttu-id="378d4-526">ガベージ コレクターの存在を前提としている他の言語と同様に C# もはガベージ コレクターがさまざまなメモリ管理ポリシーを実装できるように設計されています。</span><span class="sxs-lookup"><span data-stu-id="378d4-526">Like other languages that assume the existence of a garbage collector, C# is designed so that the garbage collector may implement a wide range of memory management policies.</span></span> <span data-ttu-id="378d4-527">たとえば、C# は必要ありませんデストラクターの実行は、対象となるとすぐに、オブジェクトを収集またはデストラクターの実行します。</span><span class="sxs-lookup"><span data-stu-id="378d4-527">For instance, C# does not require that destructors be run or that objects be collected as soon as they are eligible, or that destructors be run in any particular order, or on any particular thread.</span></span>

<span data-ttu-id="378d4-528">クラスの静的メソッドを使用して、ある程度までのガベージ コレクターの動作を制御できます`System.GC`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-528">The behavior of the garbage collector can be controlled, to some degree, via static methods on the class `System.GC`.</span></span> <span data-ttu-id="378d4-529">このクラスは、コレクションの実行 (または実行されません)、デストラクターの実行を要求するために使用してなど。</span><span class="sxs-lookup"><span data-stu-id="378d4-529">This class can be used to request a collection to occur, destructors to be run (or not run), and so forth.</span></span>

<span data-ttu-id="378d4-530">ガベージ コレクターがオブジェクトを収集し、デストラクターを実行する状況の判断に広くを許可されているために、準拠した実装は次のコードで示されているのとは異なる出力を生成可能性があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-530">Since the garbage collector is allowed wide latitude in deciding when to collect objects and run destructors, a conforming implementation may produce output that differs from that shown by the following code.</span></span> <span data-ttu-id="378d4-531">プログラム</span><span class="sxs-lookup"><span data-stu-id="378d4-531">The program</span></span>
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
<span data-ttu-id="378d4-532">クラスのインスタンスを作成します`A`クラスのインスタンスと`B`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-532">creates an instance of class `A` and an instance of class `B`.</span></span> <span data-ttu-id="378d4-533">これらのオブジェクトがガベージ コレクションの対象になるときに変数`b`値が割り当てられている`null`この時刻より後ではないためそれらにアクセスするユーザーが記述したコードを可能な。</span><span class="sxs-lookup"><span data-stu-id="378d4-533">These objects become eligible for garbage collection when the variable `b` is assigned the value `null`, since after this time it is impossible for any user-written code to access them.</span></span> <span data-ttu-id="378d4-534">出力は、いずれか</span><span class="sxs-lookup"><span data-stu-id="378d4-534">The output could be either</span></span>
```
Destruct instance of A
Destruct instance of B
```
<span data-ttu-id="378d4-535">または</span><span class="sxs-lookup"><span data-stu-id="378d4-535">or</span></span>
```
Destruct instance of B
Destruct instance of A
```
<span data-ttu-id="378d4-536">注文の言語の制約がないため、オブジェクトはガベージ コレクションです。</span><span class="sxs-lookup"><span data-stu-id="378d4-536">because the language imposes no constraints on the order in which objects are garbage collected.</span></span>

<span data-ttu-id="378d4-537">わかりにくい場合は、「消滅できる」と「コレクションの対象」の違いは重要にできます。</span><span class="sxs-lookup"><span data-stu-id="378d4-537">In subtle cases, the distinction between "eligible for destruction" and "eligible for collection" can be important.</span></span> <span data-ttu-id="378d4-538">例えば以下のようにします。</span><span class="sxs-lookup"><span data-stu-id="378d4-538">For example,</span></span>
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

<span data-ttu-id="378d4-539">ガベージ コレクターがのデストラクターを実行する場合は、上記のプログラムで`A`のデストラクターの前に`B`、このプログラムの出力である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-539">In the above program, if the garbage collector chooses to run the destructor of `A` before the destructor of `B`, then the output of this program might be:</span></span>
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

<span data-ttu-id="378d4-540">インスタンス`A`使用されていないと`A`のデストラクターが実行された、方法のことができますが`A`(この場合、 `F`) 別のデストラクターから呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-540">Note that although the instance of `A` was not in use and `A`'s destructor was run, it is still possible for methods of `A` (in this case, `F`) to be called from another destructor.</span></span> <span data-ttu-id="378d4-541">また、デストラクターの実行は、オブジェクトをもう一度でメインライン プログラムから使用可能な状態で発生する可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="378d4-541">Also, note that running of a destructor may cause an object to become usable from the mainline program again.</span></span> <span data-ttu-id="378d4-542">ここでの実行`B`デストラクターは、インスタンスの原因となった`A`が以前ではなくを使用するライブ参照からアクセスできるように`Test.RefA`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-542">In this case, the running of `B`'s destructor caused an instance of `A` that was previously not in use to become accessible from the live reference `Test.RefA`.</span></span> <span data-ttu-id="378d4-543">呼び出し後`WaitForPendingFinalizers`のインスタンス`B`が、コレクションが、インスタンスの対象である`A`、参照が原因でない`Test.RefA`します。</span><span class="sxs-lookup"><span data-stu-id="378d4-543">After the call to `WaitForPendingFinalizers`, the instance of `B` is eligible for collection, but the instance of `A` is not, because of the reference `Test.RefA`.</span></span>

<span data-ttu-id="378d4-544">混乱や予期しない動作を避けるため、一般に、お勧めのデストラクターのみ、オブジェクトのフィールドに格納されているデータに対してクリーンアップを実行してを参照先のオブジェクトまたは静的フィールドに対するアクションは実行が。</span><span class="sxs-lookup"><span data-stu-id="378d4-544">To avoid confusion and unexpected behavior, it is generally a good idea for destructors to only perform cleanup on data stored in their object's own fields, and not to perform any actions on referenced objects or static fields.</span></span>

<span data-ttu-id="378d4-545">デストラクターを使用する代わりには、実装クラスを使用する、`System.IDisposable`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="378d4-545">An alternative to using destructors is to let a class implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="378d4-546">これにより、クライアントは、リソースとして、オブジェクトにアクセスして、通常、オブジェクトのリソースを解放するタイミングを決定するオブジェクトの`using`ステートメント ([、ステートメントを使用して](statements.md#the-using-statement))。</span><span class="sxs-lookup"><span data-stu-id="378d4-546">This allows the client of the object to determine when to release the resources of the object, typically by accessing the object as a resource in a `using` statement ([The using statement](statements.md#the-using-statement)).</span></span>

## <a name="execution-order"></a><span data-ttu-id="378d4-547">実行順序</span><span class="sxs-lookup"><span data-stu-id="378d4-547">Execution order</span></span>

<span data-ttu-id="378d4-548">C# プログラムの実行は、各実行中のスレッドの副作用が重大な実行ポイントで維持されるように進みます。</span><span class="sxs-lookup"><span data-stu-id="378d4-548">Execution of a C# program proceeds such that the side effects of each executing thread are preserved at critical execution points.</span></span> <span data-ttu-id="378d4-549">A***副作用***読み取りまたは書き込み volatile フィールドの非揮発性変数への書き込み、外部のリソースと、例外のスローへの書き込みとして定義されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-549">A ***side effect*** is defined as a read or write of a volatile field, a write to a non-volatile variable, a write to an external resource, and the throwing of an exception.</span></span> <span data-ttu-id="378d4-550">副作用の順序を保持する必要がある重大な実行ポイントは volatile フィールドへの参照 ([Volatile フィールド](classes.md#volatile-fields))、`lock`ステートメント ([lock ステートメント](statements.md#the-lock-statement))、およびスレッドの作成と終了します。</span><span class="sxs-lookup"><span data-stu-id="378d4-550">The critical execution points at which the order of these side effects must be preserved are references to volatile fields ([Volatile fields](classes.md#volatile-fields)), `lock` statements ([The lock statement](statements.md#the-lock-statement)), and thread creation and termination.</span></span> <span data-ttu-id="378d4-551">実行環境では、次の制約を前提と C# プログラムの実行の順序を変更する無料です。</span><span class="sxs-lookup"><span data-stu-id="378d4-551">The execution environment is free to change the order of execution of a C# program, subject to the following constraints:</span></span>

*  <span data-ttu-id="378d4-552">データの依存関係は、実行のスレッド内で保持されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-552">Data dependence is preserved within a thread of execution.</span></span> <span data-ttu-id="378d4-553">つまり、各変数の値は、スレッドのすべてのステートメントは、元のプログラムの順序で実行されたかのように計算されます。</span><span class="sxs-lookup"><span data-stu-id="378d4-553">That is, the value of each variable is computed as if all statements in the thread were executed in original program order.</span></span>
*  <span data-ttu-id="378d4-554">初期化の順序の規則は保持されます ([フィールドの初期化](classes.md#field-initialization)と[変数初期化子](classes.md#variable-initializers))。</span><span class="sxs-lookup"><span data-stu-id="378d4-554">Initialization ordering rules are preserved ([Field initialization](classes.md#field-initialization) and [Variable initializers](classes.md#variable-initializers)).</span></span>
*  <span data-ttu-id="378d4-555">揮発性の読み取りと書き込みに関して、副作用の順序は保持されます ([Volatile フィールド](classes.md#volatile-fields))。</span><span class="sxs-lookup"><span data-stu-id="378d4-555">The ordering of side effects is preserved with respect to volatile reads and writes ([Volatile fields](classes.md#volatile-fields)).</span></span> <span data-ttu-id="378d4-556">さらに、その式の値が使用されないことと、必要な副作用は生成されません (メソッドの呼び出しまたは volatile フィールドへのアクセスが原因でいずれかを含む) を推定できる場合、実行環境は式の一部を評価しない必要があります。</span><span class="sxs-lookup"><span data-stu-id="378d4-556">Additionally, the execution environment need not evaluate part of an expression if it can deduce that that expression's value is not used and that no needed side effects are produced (including any caused by calling a method or accessing a volatile field).</span></span> <span data-ttu-id="378d4-557">(別のスレッドによってスローされる例外) など、非同期イベントでは、プログラムの実行が中断された場合、監視可能な副作用が元のプログラムの順序で表示されることは限りません。</span><span class="sxs-lookup"><span data-stu-id="378d4-557">When program execution is interrupted by an asynchronous event (such as an exception thrown by another thread), it is not guaranteed that the observable side effects are visible in the original program order.</span></span>
