# <a name="namespaces"></a><span data-ttu-id="01826-101">名前空間</span><span class="sxs-lookup"><span data-stu-id="01826-101">Namespaces</span></span>

<span data-ttu-id="01826-102">C# プログラムの名前空間を使用して構成します。</span><span class="sxs-lookup"><span data-stu-id="01826-102">C# programs are organized using namespaces.</span></span> <span data-ttu-id="01826-103">名前空間は、"external"組織のシステムおよびプログラムは、「内部」組織システムとして使用されます-その他のプログラムに公開されているプログラム要素を表すため。</span><span class="sxs-lookup"><span data-stu-id="01826-103">Namespaces are used both as an "internal" organization system for a program, and as an "external" organization system—a way of presenting program elements that are exposed to other programs.</span></span>

<span data-ttu-id="01826-104">Using ディレクティブ ([ディレクティブを使用して](namespaces.md#using-directives)) 名前空間の使用を容易に提供されます。</span><span class="sxs-lookup"><span data-stu-id="01826-104">Using directives ([Using directives](namespaces.md#using-directives)) are provided to facilitate the use of namespaces.</span></span>

## <a name="compilation-units"></a><span data-ttu-id="01826-105">コンパイル単位</span><span class="sxs-lookup"><span data-stu-id="01826-105">Compilation units</span></span>

<span data-ttu-id="01826-106">A *compilation_unit*ソース ファイルの全体的な構造を定義します。</span><span class="sxs-lookup"><span data-stu-id="01826-106">A *compilation_unit* defines the overall structure of a source file.</span></span> <span data-ttu-id="01826-107">0 個以上のコンパイル単位で構成されます*using_directive*s が 0 個以上続く*global_attributes* 0 個以上続く*namespace_member_declaration*s.</span><span class="sxs-lookup"><span data-stu-id="01826-107">A compilation unit consists of zero or more *using_directive*s followed by zero or more *global_attributes* followed by zero or more *namespace_member_declaration*s.</span></span>

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

<span data-ttu-id="01826-108">C# プログラムは 1 つまたは複数のコンパイル単位で、別のソース ファイルに含まれている各。</span><span class="sxs-lookup"><span data-stu-id="01826-108">A C# program consists of one or more compilation units, each contained in a separate source file.</span></span> <span data-ttu-id="01826-109">C# プログラムがコンパイルされると、すべてのコンパイル単位が一度に処理されます。</span><span class="sxs-lookup"><span data-stu-id="01826-109">When a C# program is compiled, all of the compilation units are processed together.</span></span> <span data-ttu-id="01826-110">そのため、コンパイル単位は互いに依存して、可能性があります、循環形式で。</span><span class="sxs-lookup"><span data-stu-id="01826-110">Thus, compilation units can depend on each other, possibly in a circular fashion.</span></span>

<span data-ttu-id="01826-111">*Using_directive*コンパイル ユニットに影響の s、 *global_attributes*と*namespace_member_declaration*、そのコンパイル ユニットの s があるない影響その他のコンパイル ユニット。</span><span class="sxs-lookup"><span data-stu-id="01826-111">The *using_directive*s of a compilation unit affect the *global_attributes* and *namespace_member_declaration*s of that compilation unit, but have no effect on other compilation units.</span></span>

<span data-ttu-id="01826-112">*Global_attributes* ([属性](attributes.md)) コンパイル単位のターゲット アセンブリとモジュール属性の指定を許可します。</span><span class="sxs-lookup"><span data-stu-id="01826-112">The *global_attributes* ([Attributes](attributes.md)) of a compilation unit permit the specification of attributes for the target assembly and module.</span></span> <span data-ttu-id="01826-113">アセンブリとモジュールは、型の物理的なコンテナーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="01826-113">Assemblies and modules act as physical containers for types.</span></span> <span data-ttu-id="01826-114">いくつかの物理的に独立したモジュールのアセンブリがあります。</span><span class="sxs-lookup"><span data-stu-id="01826-114">An assembly may consist of several physically separate modules.</span></span>

<span data-ttu-id="01826-115">*Namespace_member_declaration*プログラムのコンパイル単位ごとの s がグローバル名前空間と呼ばれる 1 つの宣言領域にメンバーを投稿します。</span><span class="sxs-lookup"><span data-stu-id="01826-115">The *namespace_member_declaration*s of each compilation unit of a program contribute members to a single declaration space called the global namespace.</span></span> <span data-ttu-id="01826-116">例えば:</span><span class="sxs-lookup"><span data-stu-id="01826-116">For example:</span></span>

<span data-ttu-id="01826-117">ファイル`A.cs`:</span><span class="sxs-lookup"><span data-stu-id="01826-117">File `A.cs`:</span></span>
```csharp
class A {}
```

<span data-ttu-id="01826-118">ファイル`B.cs`:</span><span class="sxs-lookup"><span data-stu-id="01826-118">File `B.cs`:</span></span>
```csharp
class B {}
```

<span data-ttu-id="01826-119">この場合、完全修飾名を持つ 2 つのクラスを宣言する 1 つのグローバル名前空間を 2 つのコンパイル単位が投稿`A`と`B`します。</span><span class="sxs-lookup"><span data-stu-id="01826-119">The two compilation units contribute to the single global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="01826-120">同じ宣言領域に 2 つのコンパイル単位がドキュメントに投稿されているとエラーと同じ名前のメンバーの宣言をそれぞれに含まれる場合。</span><span class="sxs-lookup"><span data-stu-id="01826-120">Because the two compilation units contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="namespace-declarations"></a><span data-ttu-id="01826-121">名前空間の宣言</span><span class="sxs-lookup"><span data-stu-id="01826-121">Namespace declarations</span></span>

<span data-ttu-id="01826-122">A *namespace_declaration*キーワードから成る`namespace`、名前空間の名前と本文で後に、必要に応じて後にセミコロンが。</span><span class="sxs-lookup"><span data-stu-id="01826-122">A *namespace_declaration* consists of the keyword `namespace`, followed by a namespace name and body, optionally followed by a semicolon.</span></span>

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

<span data-ttu-id="01826-123">A *namespace_declaration*で最上位レベルの宣言と発生する可能性があります、 *compilation_unit*または別のメンバーの宣言として*namespace_declaration*します。</span><span class="sxs-lookup"><span data-stu-id="01826-123">A *namespace_declaration* may occur as a top-level declaration in a *compilation_unit* or as a member declaration within another *namespace_declaration*.</span></span> <span data-ttu-id="01826-124">ときに、 *namespace_declaration*で最上位レベルの宣言と発生する、 *compilation_unit*、名前空間はグローバル名前空間のメンバーになります。</span><span class="sxs-lookup"><span data-stu-id="01826-124">When a *namespace_declaration* occurs as a top-level declaration in a *compilation_unit*, the namespace becomes a member of the global namespace.</span></span> <span data-ttu-id="01826-125">ときに、 *namespace_declaration*別内で発生*namespace_declaration*、内部の名前空間が外側の名前空間のメンバーになります。</span><span class="sxs-lookup"><span data-stu-id="01826-125">When a *namespace_declaration* occurs within another *namespace_declaration*, the inner namespace becomes a member of the outer namespace.</span></span> <span data-ttu-id="01826-126">どちらの場合、名前空間の名前が含まれる名前空間内で一意であります。</span><span class="sxs-lookup"><span data-stu-id="01826-126">In either case, the name of a namespace must be unique within the containing namespace.</span></span>

<span data-ttu-id="01826-127">名前空間は、暗黙的に`public`と名前空間の宣言は、アクセス修飾子を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="01826-127">Namespaces are implicitly `public` and the declaration of a namespace cannot include any access modifiers.</span></span>

<span data-ttu-id="01826-128">内で、 *namespace_body*、省略可能な*using_directive*s が他の名前空間、型を修飾名ではなく直接参照できるように、メンバーの名前をインポートします。</span><span class="sxs-lookup"><span data-stu-id="01826-128">Within a *namespace_body*, the optional *using_directive*s import the names of other namespaces, types and members, allowing them to be referenced directly instead of through qualified names.</span></span> <span data-ttu-id="01826-129">省略可能な*namespace_member_declaration*s が、名前空間の宣言領域にメンバーを投稿します。</span><span class="sxs-lookup"><span data-stu-id="01826-129">The optional *namespace_member_declaration*s contribute members to the declaration space of the namespace.</span></span> <span data-ttu-id="01826-130">なおすべて*using_directive*s は、メンバー宣言の前に表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="01826-130">Note that all *using_directive*s must appear before any member declarations.</span></span>

<span data-ttu-id="01826-131">*Qualified_identifier*の*namespace_declaration* 、単一の識別子またはシーケンスで区切られた識別子の場合があります"`.`"トークンです。</span><span class="sxs-lookup"><span data-stu-id="01826-131">The *qualified_identifier* of a *namespace_declaration* may be a single identifier or a sequence of identifiers separated by "`.`" tokens.</span></span> <span data-ttu-id="01826-132">後者の形式は、構文的に複数の名前空間宣言を入れ子せず、入れ子になった名前空間を定義するプログラムを許可します。</span><span class="sxs-lookup"><span data-stu-id="01826-132">The latter form permits a program to define a nested namespace without lexically nesting several namespace declarations.</span></span> <span data-ttu-id="01826-133">たとえば、オブジェクトに適用された</span><span class="sxs-lookup"><span data-stu-id="01826-133">For example,</span></span>

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
<span data-ttu-id="01826-134">同じ意味には</span><span class="sxs-lookup"><span data-stu-id="01826-134">is semantically equivalent to</span></span>
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

<span data-ttu-id="01826-135">名前空間は拡張可能なと同じ完全修飾名で 2 つの名前空間宣言が同じ宣言領域に投稿 ([宣言](basic-concepts.md#declarations))。</span><span class="sxs-lookup"><span data-stu-id="01826-135">Namespaces are open-ended, and two namespace declarations with the same fully qualified name contribute to the same declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="01826-136">例</span><span class="sxs-lookup"><span data-stu-id="01826-136">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
<span data-ttu-id="01826-137">ここで完全修飾の名前を持つ 2 つのクラスを宣言する、同じ宣言領域に 2 つの名前空間宣言が貢献`N1.N2.A`と`N1.N2.B`します。</span><span class="sxs-lookup"><span data-stu-id="01826-137">the two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span> <span data-ttu-id="01826-138">2 つの宣言は、同じ宣言領域に追加、ためにされていた場合は、各エラーには、同じ名前のメンバーの宣言が含まれています。</span><span class="sxs-lookup"><span data-stu-id="01826-138">Because the two declarations contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="extern-aliases"></a><span data-ttu-id="01826-139">Extern エイリアス</span><span class="sxs-lookup"><span data-stu-id="01826-139">Extern aliases</span></span>

<span data-ttu-id="01826-140">*Extern_alias_directive*名前空間のエイリアスとして機能する識別子が導入されています。</span><span class="sxs-lookup"><span data-stu-id="01826-140">An *extern_alias_directive* introduces an identifier that serves as an alias for a namespace.</span></span> <span data-ttu-id="01826-141">エイリアスの名前空間の仕様では、プログラムのソース コードの外部にあるし、別名の名前空間の入れ子になった名前空間にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="01826-141">The specification of the aliased namespace is external to the source code of the program and applies also to nested namespaces of the aliased namespace.</span></span>

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

<span data-ttu-id="01826-142">スコープ、 *extern_alias_directive*にわたる、 *using_directive*s、 *global_attributes*と*namespace_member_declaration*s がすぐに親のコンパイル単位または名前空間本文。</span><span class="sxs-lookup"><span data-stu-id="01826-142">The scope of an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span>

<span data-ttu-id="01826-143">含むコンパイル単位または名前空間本体内で、 *extern_alias_directive*で導入された識別子、 *extern_alias_directive*エイリアスの名前空間を参照するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="01826-143">Within a compilation unit or namespace body that contains an *extern_alias_directive*, the identifier introduced by the *extern_alias_directive* can be used to reference the aliased namespace.</span></span> <span data-ttu-id="01826-144">コンパイル時エラーには、*識別子*という単語を`global`します。</span><span class="sxs-lookup"><span data-stu-id="01826-144">It is a compile-time error for the *identifier* to be the word `global`.</span></span>

<span data-ttu-id="01826-145">*Extern_alias_directive*エイリアス使用可能な特定のコンパイル単位または名前空間本体の内部は、基になる宣言領域に新しいメンバーは含まれませんが、します。</span><span class="sxs-lookup"><span data-stu-id="01826-145">An *extern_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="01826-146">つまり、 *extern_alias_directive* 、推移的ではありませんが、コンパイル単位または名前空間本文のみが発生するのではなく、影響を与えます。</span><span class="sxs-lookup"><span data-stu-id="01826-146">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>

<span data-ttu-id="01826-147">次のプログラムを宣言し、2 つの extern エイリアスを使用して`X`と`Y`、それぞれの個別の名前空間階層のルートを表します。</span><span class="sxs-lookup"><span data-stu-id="01826-147">The following program declares and uses two extern aliases, `X` and `Y`, each of which represent the root of a distinct namespace hierarchy:</span></span>
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

<span data-ttu-id="01826-148">プログラムは、extern の存在のエイリアスを宣言します`X`と`Y`がエイリアスの実際の定義は、外部プログラムにします。</span><span class="sxs-lookup"><span data-stu-id="01826-148">The program declares the existence of the extern aliases `X` and `Y`, but the actual definitions of the aliases are external to the program.</span></span> <span data-ttu-id="01826-149">同じ名前を持つ`N.B`クラスとして参照できます`X.N.B`と`Y.N.B`、または、名前空間エイリアス修飾子を使って`X::N.B`と`Y::N.B`します。</span><span class="sxs-lookup"><span data-stu-id="01826-149">The identically named `N.B` classes can now be referenced as `X.N.B` and `Y.N.B`, or, using the namespace alias qualifier, `X::N.B` and `Y::N.B`.</span></span> <span data-ttu-id="01826-150">エラーは、プログラムは、外部定義が提供 extern エイリアスを宣言する場合に発生しません。</span><span class="sxs-lookup"><span data-stu-id="01826-150">An error occurs if a program declares an extern alias for which no external definition is provided.</span></span>

## <a name="using-directives"></a><span data-ttu-id="01826-151">using ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="01826-151">Using directives</span></span>

<span data-ttu-id="01826-152">***Using ディレクティブ***名前空間および他の名前空間で定義された型の使用を容易にします。</span><span class="sxs-lookup"><span data-stu-id="01826-152">***Using directives*** facilitate the use of namespaces and types defined in other namespaces.</span></span> <span data-ttu-id="01826-153">ディレクティブへの影響の名前解決プロセスを使用して*namespace_or_type_name*s ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names)) と*simple_name*s ([簡易名](expressions.md#simple-names))、ディレクティブを使用して、宣言とは異なり、新しいメンバーのコンパイル単位または使用される名前空間の基になる宣言スペースには影響しません。</span><span class="sxs-lookup"><span data-stu-id="01826-153">Using directives impact the name resolution process of *namespace_or_type_name*s ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) and *simple_name*s ([Simple names](expressions.md#simple-names)), but unlike declarations, using directives do not contribute new members to the underlying declaration spaces of the compilation units or namespaces within which they are used.</span></span>

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

<span data-ttu-id="01826-154">A *using_alias_directive* ([Using エイリアス ディレクティブ](namespaces.md#using-alias-directives)) 名前空間または型のエイリアスが導入されています。</span><span class="sxs-lookup"><span data-stu-id="01826-154">A *using_alias_directive* ([Using alias directives](namespaces.md#using-alias-directives)) introduces an alias for a namespace or type.</span></span>

<span data-ttu-id="01826-155">A *using_namespace_directive* ([名前空間ディレクティブを使用して](namespaces.md#using-namespace-directives)) 名前空間の型のメンバーをインポートします。</span><span class="sxs-lookup"><span data-stu-id="01826-155">A *using_namespace_directive* ([Using namespace directives](namespaces.md#using-namespace-directives)) imports the type members of a namespace.</span></span>

<span data-ttu-id="01826-156">A *using_static_directive* ([Using static ディレクティブ](namespaces.md#using-static-directives))、入れ子にされた型と型の静的メンバーをインポートします。</span><span class="sxs-lookup"><span data-stu-id="01826-156">A *using_static_directive* ([Using static directives](namespaces.md#using-static-directives)) imports the nested types and static members of a type.</span></span>

<span data-ttu-id="01826-157">スコープを*using_directive*にわたる、 *namespace_member_declaration*s がすぐに親のコンパイル単位または名前空間本文。</span><span class="sxs-lookup"><span data-stu-id="01826-157">The scope of a *using_directive* extends over the *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="01826-158">スコープを*using_directive*具体的には、ピアは含まれません*using_directive*秒。</span><span class="sxs-lookup"><span data-stu-id="01826-158">The scope of a *using_directive* specifically does not include its peer *using_directive*s.</span></span> <span data-ttu-id="01826-159">したがって、ピア*using_directive*s が、互いに影響しませんし、記述されている順序が重要ではありません。</span><span class="sxs-lookup"><span data-stu-id="01826-159">Thus, peer *using_directive*s do not affect each other, and the order in which they are written is insignificant.</span></span>

### <a name="using-alias-directives"></a><span data-ttu-id="01826-160">Using エイリアス ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="01826-160">Using alias directives</span></span>

<span data-ttu-id="01826-161">A *using_alias_directive*名前空間またはすぐ外側のコンパイル単位または名前空間の本文内の型のエイリアスとして機能する識別子が導入されています。</span><span class="sxs-lookup"><span data-stu-id="01826-161">A *using_alias_directive* introduces an identifier that serves as an alias for a namespace or type within the immediately enclosing compilation unit or namespace body.</span></span>

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

<span data-ttu-id="01826-162">メンバー宣言が含まれるコンパイル単位または名前空間本文内で、 *using_alias_directive*で導入された識別子、 *using_alias_directive*使用できる参照を特定名前空間または型。</span><span class="sxs-lookup"><span data-stu-id="01826-162">Within member declarations in a compilation unit or namespace body that contains a *using_alias_directive*, the identifier introduced by the *using_alias_directive* can be used to reference the given namespace or type.</span></span> <span data-ttu-id="01826-163">例えば:</span><span class="sxs-lookup"><span data-stu-id="01826-163">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

<span data-ttu-id="01826-164">上記のメンバー宣言内で、`N3`名前空間、`A`の別名です`N1.N2.A`、クラス`N3.B`クラスから派生`N1.N2.A`します。</span><span class="sxs-lookup"><span data-stu-id="01826-164">Above, within member declarations in the `N3` namespace, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="01826-165">別名を作成して、同じ効果を取得できる`R`の`N1.N2`し、参照する`R.A`:</span><span class="sxs-lookup"><span data-stu-id="01826-165">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

<span data-ttu-id="01826-166">*識別子*の*using_alias_directive*コンパイル単位またはすぐに含む名前空間の宣言領域内で一意である必要があります、 *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="01826-166">The *identifier* of a *using_alias_directive* must be unique within the declaration space of the compilation unit or namespace that immediately contains the *using_alias_directive*.</span></span> <span data-ttu-id="01826-167">例えば:</span><span class="sxs-lookup"><span data-stu-id="01826-167">For example:</span></span>
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

<span data-ttu-id="01826-168">上記`N3`メンバーが既に含まれています`A`ので、コンパイル時エラーには、 *using_alias_directive*その識別子を使用します。</span><span class="sxs-lookup"><span data-stu-id="01826-168">Above, `N3` already contains a member `A`, so it is a compile-time error for a *using_alias_directive* to use that identifier.</span></span> <span data-ttu-id="01826-169">同様に、2 つ以上のコンパイル時エラーが*using_alias_directive*同じコンパイル単位または名前空間に同じ名前のエイリアスを宣言する本体で s。</span><span class="sxs-lookup"><span data-stu-id="01826-169">Likewise, it is a compile-time error for two or more *using_alias_directive*s in the same compilation unit or namespace body to declare aliases by the same name.</span></span>

<span data-ttu-id="01826-170">A *using_alias_directive*エイリアス使用可能な特定のコンパイル単位または名前空間本体の内部は、基になる宣言領域に新しいメンバーは含まれませんが、します。</span><span class="sxs-lookup"><span data-stu-id="01826-170">A *using_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="01826-171">つまり、 *using_alias_directive*は推移的ではありませんが、コンパイル単位または名前空間本文のみが発生するのではなく影響を与えます。</span><span class="sxs-lookup"><span data-stu-id="01826-171">In other words, a *using_alias_directive* is not transitive but rather affects only the compilation unit or namespace body in which it occurs.</span></span> <span data-ttu-id="01826-172">例</span><span class="sxs-lookup"><span data-stu-id="01826-172">In the example</span></span>
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
<span data-ttu-id="01826-173">スコープ、 *using_alias_directive*導入する`R`のみが含まれる名前空間のメンバー宣言に対する拡張ように`R`が 2 つ目の名前空間宣言で不明です。</span><span class="sxs-lookup"><span data-stu-id="01826-173">the scope of the *using_alias_directive* that introduces `R` only extends to member declarations in the namespace body in which it is contained, so `R` is unknown in the second namespace declaration.</span></span> <span data-ttu-id="01826-174">ただし、配置、 *using_alias_directive*含むコンパイル単位が両方の名前空間宣言内で使用可能になるエイリアス。</span><span class="sxs-lookup"><span data-stu-id="01826-174">However, placing the *using_alias_directive* in the containing compilation unit causes the alias to become available within both namespace declarations:</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

<span data-ttu-id="01826-175">通常のメンバーと同様で導入された名前*using_alias_directive*s は入れ子になったスコープの同じ名前のメンバーによって非表示にします。</span><span class="sxs-lookup"><span data-stu-id="01826-175">Just like regular members, names introduced by *using_alias_directive*s are hidden by similarly named members in nested scopes.</span></span> <span data-ttu-id="01826-176">例</span><span class="sxs-lookup"><span data-stu-id="01826-176">In the example</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
<span data-ttu-id="01826-177">参照を`R.A`の宣言で`B`ので、コンパイル時エラーが発生`R`を指す`N3.R`ではなく、`N1.N2`します。</span><span class="sxs-lookup"><span data-stu-id="01826-177">the reference to `R.A` in the declaration of `B` causes a compile-time error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="01826-178">順序*using_alias_directive*有意性、およびの解像度を持たない s が書き込まれる、 *namespace_or_type_name*によって参照される、 *using_alias_directive*は影響されません、 *using_alias_directive*自体またはその他*using_directive*コンパイル単位または名前空間の本体は、コンテナーで s。</span><span class="sxs-lookup"><span data-stu-id="01826-178">The order in which *using_alias_directive*s are written has no significance, and resolution of the *namespace_or_type_name* referenced by a *using_alias_directive* is not affected by the *using_alias_directive* itself or by other *using_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="01826-179">つまり、 *namespace_or_type_name*の*using_alias_directive*がコンパイル単位または名前空間の本体は、すぐに親があるないかのように解決される*using_directive*秒。</span><span class="sxs-lookup"><span data-stu-id="01826-179">In other words, the *namespace_or_type_name* of a *using_alias_directive* is resolved as if the immediately containing compilation unit or namespace body had no *using_directive*s.</span></span> <span data-ttu-id="01826-180">A *using_alias_directive*しかし、影響を受ける*extern_alias_directive*コンパイル単位または名前空間の本体は、コンテナー内。</span><span class="sxs-lookup"><span data-stu-id="01826-180">A *using_alias_directive* may however be affected by *extern_alias_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="01826-181">例</span><span class="sxs-lookup"><span data-stu-id="01826-181">In the example</span></span>
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
<span data-ttu-id="01826-182">最後の*using_alias_directive*最初に影響を受けないため、コンパイル時エラー結果*using_alias_directive*します。</span><span class="sxs-lookup"><span data-stu-id="01826-182">the last *using_alias_directive* results in a compile-time error because it is not affected by the first *using_alias_directive*.</span></span> <span data-ttu-id="01826-183">最初の*using_alias_directive* extern エイリアスのスコープから、エラーが発生しない`E`が含まれています、 *using_alias_directive*します。</span><span class="sxs-lookup"><span data-stu-id="01826-183">The first *using_alias_directive* does not result in an error since the scope of the extern alias `E` includes the *using_alias_directive*.</span></span>

<span data-ttu-id="01826-184">A *using_alias_directive*任意の名前空間またはが表示される名前空間を含む、型の別名を作成し、任意の名前空間または型は、その名前空間内に入れ子にします。</span><span class="sxs-lookup"><span data-stu-id="01826-184">A *using_alias_directive* can create an alias for any namespace or type, including the namespace within which it appears and any namespace or type nested within that namespace.</span></span>

<span data-ttu-id="01826-185">名前空間または型にエイリアスを使ってアクセスするには、その宣言された名前をその名前空間または型へのアクセスとまったく同じ結果が得られます。</span><span class="sxs-lookup"><span data-stu-id="01826-185">Accessing a namespace or type through an alias yields exactly the same result as accessing that namespace or type through its declared name.</span></span> <span data-ttu-id="01826-186">たとえば、</span><span class="sxs-lookup"><span data-stu-id="01826-186">For example, given</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
<span data-ttu-id="01826-187">名前`N1.N2.A`、 `R1.N2.A`、および`R2.A`の完全修飾名がクラスを同等とすべて参照`N1.N2.A`します。</span><span class="sxs-lookup"><span data-stu-id="01826-187">the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="01826-188">別名を使用して構築されたクローズ型の名前をことができますが、型引数を指定せずに、バインドされていないジェネリック型の宣言の名前をことはできません。</span><span class="sxs-lookup"><span data-stu-id="01826-188">Using aliases can name a closed constructed type, but cannot name an unbound generic type declaration without supplying type arguments.</span></span> <span data-ttu-id="01826-189">例えば:</span><span class="sxs-lookup"><span data-stu-id="01826-189">For example:</span></span>
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a><span data-ttu-id="01826-190">名前空間ディレクティブを使用します。</span><span class="sxs-lookup"><span data-stu-id="01826-190">Using namespace directives</span></span>

<span data-ttu-id="01826-191">A *using_namespace_directive*修飾なしで使用するには、各型の識別子を有効にするとすぐにそれを囲むコンパイル単位または名前空間の本体に含まれている名前空間型をインポートします。</span><span class="sxs-lookup"><span data-stu-id="01826-191">A *using_namespace_directive* imports the types contained in a namespace into the immediately enclosing compilation unit or namespace body, enabling the identifier of each type to be used without qualification.</span></span>

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

<span data-ttu-id="01826-192">メンバー宣言が含まれるコンパイル単位または名前空間本文内で、 *using_namespace_directive*、指定した名前空間に含まれる型を直接参照できます。</span><span class="sxs-lookup"><span data-stu-id="01826-192">Within member declarations in a compilation unit or namespace body that contains a *using_namespace_directive*, the types contained in the given namespace can be referenced directly.</span></span> <span data-ttu-id="01826-193">例えば:</span><span class="sxs-lookup"><span data-stu-id="01826-193">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

<span data-ttu-id="01826-194">上記のメンバー宣言内で、`N3`名前空間、型のメンバーの`N1.N2`直接利用して、クラス`N3.B`クラスから派生`N1.N2.A`します。</span><span class="sxs-lookup"><span data-stu-id="01826-194">Above, within member declarations in the `N3` namespace, the type members of `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="01826-195">A *using_namespace_directive*指定した名前空間に含まれる型をインポートしますが、具体的には入れ子になった名前空間をインポートしません。</span><span class="sxs-lookup"><span data-stu-id="01826-195">A *using_namespace_directive* imports the types contained in the given namespace, but specifically does not import nested namespaces.</span></span> <span data-ttu-id="01826-196">例</span><span class="sxs-lookup"><span data-stu-id="01826-196">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
<span data-ttu-id="01826-197">*using_namespace_directive*に含まれている型をインポート`N1`で入れ子になった名前空間ではありませんが、`N1`します。</span><span class="sxs-lookup"><span data-stu-id="01826-197">the *using_namespace_directive* imports the types contained in `N1`, but not the namespaces nested in `N1`.</span></span> <span data-ttu-id="01826-198">参照をそのため、`N2.A`の宣言で`B`メンバーが指定されていないため、コンパイル時エラー結果`N2`スコープ内にあります。</span><span class="sxs-lookup"><span data-stu-id="01826-198">Thus, the reference to `N2.A` in the declaration of `B` results in a compile-time error because no members named `N2` are in scope.</span></span>

<span data-ttu-id="01826-199">異なり、 *using_alias_directive*、 *using_namespace_directive*の識別子が外側のコンパイル単位または名前空間の本文内で既に定義されている型をインポートすることがあります。</span><span class="sxs-lookup"><span data-stu-id="01826-199">Unlike a *using_alias_directive*, a *using_namespace_directive* may import types whose identifiers are already defined within the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="01826-200">によって名前が実際には、インポート、 *using_namespace_directive*外側のコンパイル単位または名前空間の本体で似た名前のメンバーでは表示されません。</span><span class="sxs-lookup"><span data-stu-id="01826-200">In effect, names imported by a *using_namespace_directive* are hidden by similarly named members in the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="01826-201">例えば:</span><span class="sxs-lookup"><span data-stu-id="01826-201">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

<span data-ttu-id="01826-202">ここでは、メンバー宣言内で、`N3`名前空間、`A`を指す`N3.A`なく`N1.N2.A`します。</span><span class="sxs-lookup"><span data-stu-id="01826-202">Here, within member declarations in the `N3` namespace, `A` refers to `N3.A` rather than `N1.N2.A`.</span></span>

<span data-ttu-id="01826-203">によって、1 つ以上の名前空間または型がインポートするときに*using_namespace_directive*s または*using_static_directive*同じコンパイル単位または名前空間の本体では、同じ名前でへの参照型を含めることがその名前として、 *type_name*あいまいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="01826-203">When more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types by the same name, references to that name as a *type_name* are considered ambiguous.</span></span> <span data-ttu-id="01826-204">例</span><span class="sxs-lookup"><span data-stu-id="01826-204">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
<span data-ttu-id="01826-205">両方`N1`と`N2`、メンバーを含んで`A`、ため`N3`を参照する、両方をインポート`A`で`N3`はコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="01826-205">both `N1` and `N2` contain a member `A`, and because `N3` imports both, referencing `A` in `N3` is a compile-time error.</span></span> <span data-ttu-id="01826-206">このような状況で、競合はへの参照の修飾を使用するか解決できる`A`、または導入することで、 *using_alias_directive*特定を取得する`A`します。</span><span class="sxs-lookup"><span data-stu-id="01826-206">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing a *using_alias_directive* that picks a particular `A`.</span></span> <span data-ttu-id="01826-207">例えば:</span><span class="sxs-lookup"><span data-stu-id="01826-207">For example:</span></span>
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

<span data-ttu-id="01826-208">さらに、インポートすると 1 つ以上の名前空間または型で*using_namespace_directive*s または*using_static_directive*同じコンパイル単位または名前空間の本体では、型またはメンバーを含めることが、同じ名前の参照としてその名前に、 *simple_name*あいまいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="01826-208">Furthermore, when more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types or members by the same name, references to that name as a *simple_name* are considered ambiguous.</span></span> <span data-ttu-id="01826-209">例</span><span class="sxs-lookup"><span data-stu-id="01826-209">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
<span data-ttu-id="01826-210">`N1` 型のメンバーが含まれています`A`と`C`静的メソッドを含む`A`、ためと`N2`を参照する、両方をインポート`A`として、 *simple_name*あいまいな、コンパイル時にエラーがあります。</span><span class="sxs-lookup"><span data-stu-id="01826-210">`N1` contains a type member `A`, and `C` contains a static method `A`, and because `N2` imports both, referencing `A` as a *simple_name* is ambiguous and a compile-time error.</span></span> 

<span data-ttu-id="01826-211">ように、 *using_alias_directive*、 *using_namespace_directive*コンパイル単位または名前空間の基になる宣言領域に新しいメンバーは含まれませんが、代わりにのみ影響しますコンパイル単位または名前空間の本体が表示されます。</span><span class="sxs-lookup"><span data-stu-id="01826-211">Like a *using_alias_directive*, a *using_namespace_directive* does not contribute any new members to the underlying declaration space of the compilation unit or namespace, but rather affects only the compilation unit or namespace body in which it appears.</span></span>

<span data-ttu-id="01826-212">*Namespace_name*によって参照される、 *using_namespace_directive*と同じ方法で解決されて、 *namespace_or_type_name*によって参照される、 *using_alias_directive*します。</span><span class="sxs-lookup"><span data-stu-id="01826-212">The *namespace_name* referenced by a *using_namespace_directive* is resolved in the same way as the *namespace_or_type_name* referenced by a *using_alias_directive*.</span></span> <span data-ttu-id="01826-213">したがって、 *using_namespace_directive*同じコンパイル単位または名前空間の本体で互いには影響しませんし、任意の順序で書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="01826-213">Thus, *using_namespace_directive*s in the same compilation unit or namespace body do not affect each other and can be written in any order.</span></span>

### <a name="using-static-directives"></a><span data-ttu-id="01826-214">Using static ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="01826-214">Using static directives</span></span>

<span data-ttu-id="01826-215">A *using_static_directive*入れ子にされた型とする各メンバーと型の識別子を有効にする静的メンバーをすぐにそれを囲むコンパイル単位または名前空間の本体に型宣言に直接含まれているインポート修飾なしで使用します。</span><span class="sxs-lookup"><span data-stu-id="01826-215">A *using_static_directive* imports the nested types and static members contained directly in a type declaration into the immediately enclosing compilation unit or namespace body, enabling the identifier of each member and type to be used without qualification.</span></span>

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

<span data-ttu-id="01826-216">メンバー宣言が含まれるコンパイル単位または名前空間本文内で、 *using_static_directive*型と静的メンバー (拡張メソッド) を除くの宣言に直接含まれている、アクセス可能な入れ子にします指定した型を直接参照できます。</span><span class="sxs-lookup"><span data-stu-id="01826-216">Within member declarations in a compilation unit or namespace body that contains a *using_static_directive*, the accessible nested types and static members (except extension methods) contained directly in the declaration of the given type can be referenced directly.</span></span> <span data-ttu-id="01826-217">例えば:</span><span class="sxs-lookup"><span data-stu-id="01826-217">For example:</span></span>

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

<span data-ttu-id="01826-218">上記のメンバー宣言内で、`N2`名前空間、静的メンバーと入れ子にされた型の`N1.A`直接使用できますが、およびそのため、メソッド`N`両方を参照することは、`B`と`M`のメンバー`N1.A`.</span><span class="sxs-lookup"><span data-stu-id="01826-218">Above, within member declarations in the `N2` namespace, the static members and nested types of `N1.A` are directly available, and thus the method `N` is able to reference both the `B` and `M` members of `N1.A`.</span></span>

<span data-ttu-id="01826-219">A *using_static_directive*具体的には、静的メソッドとして直接拡張メソッドをインポートできませんが、拡張メソッド呼び出しの使用可能になります ([拡張メソッド呼び出し](expressions.md#extension-method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="01826-219">A *using_static_directive* specifically does not import extension methods directly as static methods, but makes them available for extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="01826-220">例</span><span class="sxs-lookup"><span data-stu-id="01826-220">In the example</span></span>

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
<span data-ttu-id="01826-221">*using_static_directive*拡張メソッドをインポート`M`に含まれている`N1.A`、拡張メソッドとしてのみです。</span><span class="sxs-lookup"><span data-stu-id="01826-221">the *using_static_directive* imports the extension method `M` contained in `N1.A`, but only as an extension method.</span></span> <span data-ttu-id="01826-222">したがって、最初に参照される`M`の本体で`B.N`メンバーが指定されていないため、コンパイル時エラー結果`M`スコープ内にあります。</span><span class="sxs-lookup"><span data-stu-id="01826-222">Thus, the first reference to `M` in the body of `B.N` results in a compile-time error because no members named `M` are in scope.</span></span>

<span data-ttu-id="01826-223">A *using_static_directive*のみをインポート メンバーと型指定された型で直接宣言された基本クラスで宣言されたメンバーと型ではありません。</span><span class="sxs-lookup"><span data-stu-id="01826-223">A *using_static_directive* only imports members and types declared directly in the given type, not members and types declared in base classes.</span></span>

<span data-ttu-id="01826-224">TODO: 例</span><span class="sxs-lookup"><span data-stu-id="01826-224">TODO: Example</span></span>

<span data-ttu-id="01826-225">複数のあいまいさ*using_namespace_directives*と*using_static_directives*は、後ほど[名前空間ディレクティブを使用して](namespaces.md#using-namespace-directives)します。</span><span class="sxs-lookup"><span data-stu-id="01826-225">Ambiguities between multiple *using_namespace_directives* and *using_static_directives* are discussed in [Using namespace directives](namespaces.md#using-namespace-directives).</span></span>

## <a name="namespace-members"></a><span data-ttu-id="01826-226">Namespace メンバー</span><span class="sxs-lookup"><span data-stu-id="01826-226">Namespace members</span></span>

<span data-ttu-id="01826-227">A *namespace_member_declaration*か、 *namespace_declaration* ([Namespace 宣言](namespaces.md#namespace-declarations)) または*type_declaration* ([入力宣言](namespaces.md#type-declarations))。</span><span class="sxs-lookup"><span data-stu-id="01826-227">A *namespace_member_declaration* is either a *namespace_declaration* ([Namespace declarations](namespaces.md#namespace-declarations)) or a *type_declaration* ([Type declarations](namespaces.md#type-declarations)).</span></span>

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

<span data-ttu-id="01826-228">コンパイル単位または名前空間の本文を含めることができます*namespace_member_declaration*と、このような宣言は、新しいメンバーを含むコンパイル単位または名前空間の本体の基になる宣言領域を投稿します。</span><span class="sxs-lookup"><span data-stu-id="01826-228">A compilation unit or a namespace body can contain *namespace_member_declaration*s, and such declarations contribute new members to the underlying declaration space of the containing compilation unit or namespace body.</span></span>

## <a name="type-declarations"></a><span data-ttu-id="01826-229">型の宣言</span><span class="sxs-lookup"><span data-stu-id="01826-229">Type declarations</span></span>

<span data-ttu-id="01826-230">A *type_declaration*は、 *class_declaration* ([クラス クラスの宣言](classes.md#class-declarations))、 *struct_declaration* ([構造体宣言](structs.md#struct-declarations))、 *interface_declaration* ([インターフェイスの宣言](interfaces.md#interface-declarations))、 *enum_declaration* ([列挙型宣言](enums.md#enum-declarations))、または*delegate_declaration* ([デリゲートの宣言](delegates.md#delegate-declarations))。</span><span class="sxs-lookup"><span data-stu-id="01826-230">A *type_declaration* is a *class_declaration* ([Class declarations](classes.md#class-declarations)), a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)), an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)), an *enum_declaration* ([Enum declarations](enums.md#enum-declarations)), or a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)).</span></span>

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

<span data-ttu-id="01826-231">A *type_declaration*コンパイル単位内の最上位レベルの宣言、または名前空間、クラスまたは構造体内でメンバーの宣言として発生することができます。</span><span class="sxs-lookup"><span data-stu-id="01826-231">A *type_declaration* can occur as a top-level declaration in a compilation unit or as a member declaration within a namespace, class, or struct.</span></span>

<span data-ttu-id="01826-232">型の型宣言`T`新しく宣言された型の完全修飾名が単には、コンパイル単位内の最上位レベルの宣言と発生`T`します。</span><span class="sxs-lookup"><span data-stu-id="01826-232">When a type declaration for a type `T` occurs as a top-level declaration in a compilation unit, the fully qualified name of the newly declared type is simply `T`.</span></span> <span data-ttu-id="01826-233">型の型宣言`T`クラスまたは構造体、新しく宣言された型の完全修飾名が、名前空間内で発生`N.T`ここで、`N`含んでいる名前空間、クラスまたは構造体の完全修飾の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="01826-233">When a type declaration for a type `T` occurs within a namespace, class, or struct, the fully qualified name of the newly declared type is `N.T`, where `N` is the fully qualified name of the containing namespace, class, or struct.</span></span>

<span data-ttu-id="01826-234">クラス内で宣言された型または構造体には、入れ子にされた型が呼び出された ([入れ子になった型](classes.md#nested-types))。</span><span class="sxs-lookup"><span data-stu-id="01826-234">A type declared within a class or struct is called a nested type ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="01826-235">使用できるアクセス修飾子と型の宣言の既定のアクセスを宣言が行われるコンテキストに依存 ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="01826-235">The permitted access modifiers and the default access for a type declaration depend on the context in which the declaration takes place ([Declared accessibility](basic-concepts.md#declared-accessibility)):</span></span>

*  <span data-ttu-id="01826-236">コンパイル単位または名前空間で宣言された型を持つことができます`public`または`internal`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="01826-236">Types declared in compilation units or namespaces can have `public` or `internal` access.</span></span> <span data-ttu-id="01826-237">既定値は`internal`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="01826-237">The default is `internal` access.</span></span>
*  <span data-ttu-id="01826-238">クラスで宣言された型を持つことができます`public`、 `protected internal`、 `protected`、 `internal`、または`private`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="01826-238">Types declared in classes can have `public`, `protected internal`, `protected`, `internal`, or `private` access.</span></span> <span data-ttu-id="01826-239">既定値は`private`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="01826-239">The default is `private` access.</span></span>
*  <span data-ttu-id="01826-240">構造体で宣言された型を持つことができます`public`、 `internal`、または`private`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="01826-240">Types declared in structs can have `public`, `internal`, or `private` access.</span></span> <span data-ttu-id="01826-241">既定値は`private`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="01826-241">The default is `private` access.</span></span>

## <a name="namespace-alias-qualifiers"></a><span data-ttu-id="01826-242">Namespace エイリアス修飾子</span><span class="sxs-lookup"><span data-stu-id="01826-242">Namespace alias qualifiers</span></span>

<span data-ttu-id="01826-243">***名前空間エイリアス修飾子***`::`型名の検索が新しい型とメンバーの導入によって影響を受けるいないことを保証することになります。</span><span class="sxs-lookup"><span data-stu-id="01826-243">The ***namespace alias qualifier*** `::` makes it possible to guarantee that type name lookups are unaffected by the introduction of new types and members.</span></span> <span data-ttu-id="01826-244">名前空間エイリアス修飾子は、左辺と右辺の識別子と呼ばれる 2 つの識別子の間で常に表示されます。</span><span class="sxs-lookup"><span data-stu-id="01826-244">The namespace alias qualifier always appears between two identifiers referred to as the left-hand and right-hand identifiers.</span></span> <span data-ttu-id="01826-245">通常とは異なり`.`修飾子の左側の識別子、`::`修飾子は、参照が、extern、またはエイリアスを使用してとしてのみです。</span><span class="sxs-lookup"><span data-stu-id="01826-245">Unlike the regular `.` qualifier, the left-hand identifier of the `::` qualifier is looked up only as an extern or using alias.</span></span>

<span data-ttu-id="01826-246">A *qualified_alias_member*が次のように定義されています。</span><span class="sxs-lookup"><span data-stu-id="01826-246">A *qualified_alias_member* is defined as follows:</span></span>

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

<span data-ttu-id="01826-247">A *qualified_alias_member*として使用できる、 *namespace_or_type_name* ([Namespace と型の名前](basic-concepts.md#namespace-and-type-names)) またはの左辺のオペランドとして、 *member_access*([メンバー アクセス](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="01826-247">A *qualified_alias_member* can be used as a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) or as the left operand in a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="01826-248">A *qualified_alias_member*が 2 つの形式のいずれか。</span><span class="sxs-lookup"><span data-stu-id="01826-248">A *qualified_alias_member* has one of two forms:</span></span>

*  <span data-ttu-id="01826-249">`N::I<A1, ..., Ak>`、、`N`と`I`、識別子を表すと`<A1, ..., Ak>`が型引数リスト。</span><span class="sxs-lookup"><span data-stu-id="01826-249">`N::I<A1, ..., Ak>`, where `N` and `I` represent identifiers, and `<A1, ..., Ak>` is a type argument list.</span></span> <span data-ttu-id="01826-250">(`K`は常に少なくとも 1 つです)。</span><span class="sxs-lookup"><span data-stu-id="01826-250">(`K` is always at least one.)</span></span>
*  <span data-ttu-id="01826-251">`N::I`、、`N`と`I`識別子を表します。</span><span class="sxs-lookup"><span data-stu-id="01826-251">`N::I`, where `N` and `I` represent identifiers.</span></span> <span data-ttu-id="01826-252">(この場合、 `K` 0 と見なされます)。</span><span class="sxs-lookup"><span data-stu-id="01826-252">(In this case, `K` is considered to be zero.)</span></span>

<span data-ttu-id="01826-253">意味、この表記を使用して、 *qualified_alias_member*は次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="01826-253">Using this notation, the meaning of a *qualified_alias_member* is determined as follows:</span></span>

*  <span data-ttu-id="01826-254">場合`N`識別子`global`、グローバル名前空間を検索し、 `I`:</span><span class="sxs-lookup"><span data-stu-id="01826-254">If `N` is the identifier `global`, then the global namespace is searched for `I`:</span></span>
   * <span data-ttu-id="01826-255">グローバル名前空間には、名前空間が含まれている場合`I`と`K`0 の場合は、次に、 *qualified_alias_member*その名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="01826-255">If the global namespace contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
   * <span data-ttu-id="01826-256">それ以外の場合、グローバル名前空間には、という名前の非ジェネリック型が含まれている場合`I`と`K`0 の場合は、次に、 *qualified_alias_member*はその型を表します。</span><span class="sxs-lookup"><span data-stu-id="01826-256">Otherwise, if the global namespace contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
   * <span data-ttu-id="01826-257">それ以外の場合、グローバル名前空間には、という名前の型が含まれている場合`I`を持つ`K`パラメーター、入力、 *qualified_alias_member*は特定の型引数を使用して構築する型を表します。</span><span class="sxs-lookup"><span data-stu-id="01826-257">Otherwise, if the global namespace contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
   * <span data-ttu-id="01826-258">それ以外の場合、 *qualified_alias_member*は未定義となり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="01826-258">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

*  <span data-ttu-id="01826-259">それ以外の場合、名前空間の宣言で始まる ([Namespace 宣言](namespaces.md#namespace-declarations)) 含まれている、 *qualified_alias_member* (ある場合)、それを囲む各名前空間宣言で継続的(ある場合) を含むコンパイル単位で終わる、 *qualified_alias_member*エンティティが見つかるまで、次の手順が評価されます。</span><span class="sxs-lookup"><span data-stu-id="01826-259">Otherwise, starting with the namespace declaration ([Namespace declarations](namespaces.md#namespace-declarations)) immediately containing the *qualified_alias_member* (if any), continuing with each enclosing namespace declaration (if any), and ending with the compilation unit containing the *qualified_alias_member*, the following steps are evaluated until an entity is located:</span></span>

   * <span data-ttu-id="01826-260">名前空間の宣言またはコンパイル ユニットに含まれる場合、 *using_alias_directive*を関連付ける`N`型を持つ、 *qualified_alias_member*が定義されていないと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="01826-260">If the namespace declaration or compilation unit contains a *using_alias_directive* that associates `N` with a type, then the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
   * <span data-ttu-id="01826-261">それ以外の場合、名前空間の宣言またはコンパイル ユニットに含まれる場合、 *extern_alias_directive*または*using_alias_directive*を関連付ける`N`名前空間、しに。</span><span class="sxs-lookup"><span data-stu-id="01826-261">Otherwise, if the namespace declaration or compilation unit contains an *extern_alias_directive* or *using_alias_directive* that associates `N` with a namespace, then:</span></span>
     * <span data-ttu-id="01826-262">名前空間に関連付けられている場合`N`という名前空間が含まれています`I`と`K`0 の場合は、次に、 *qualified_alias_member*その名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="01826-262">If the namespace associated with `N` contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
     * <span data-ttu-id="01826-263">それ以外の場合、名前空間に関連付けられている場合`N`という名前の非ジェネリック型が含まれています`I`と`K`0 の場合は、次に、 *qualified_alias_member*はその型を表します。</span><span class="sxs-lookup"><span data-stu-id="01826-263">Otherwise, if the namespace associated with `N` contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
     * <span data-ttu-id="01826-264">それ以外の場合、名前空間に関連付けられている場合`N`という名前の型を含む`I`を持つ`K`パラメーター、入力、 *qualified_alias_member*型を指定して構築された型を参照引数。</span><span class="sxs-lookup"><span data-stu-id="01826-264">Otherwise, if the namespace associated with `N` contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
     * <span data-ttu-id="01826-265">それ以外の場合、 *qualified_alias_member*は未定義となり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="01826-265">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="01826-266">それ以外の場合、 *qualified_alias_member*は未定義となり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="01826-266">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="01826-267">型を参照するエイリアスで名前空間エイリアス修飾子を使用して、コンパイル時エラーが発生に注意してください。</span><span class="sxs-lookup"><span data-stu-id="01826-267">Note that using the namespace alias qualifier with an alias that references a type causes a compile-time error.</span></span> <span data-ttu-id="01826-268">いる場合に、id をメモ`N`は`global`、使用してエイリアスに関連付けることがある場合でも、グローバル名前空間で検索を実行し、`global`型または名前空間。</span><span class="sxs-lookup"><span data-stu-id="01826-268">Also note that if the identifier `N` is `global`, then lookup is performed in the global namespace, even if there is a using alias associating `global` with a type or namespace.</span></span>

### <a name="uniqueness-of-aliases"></a><span data-ttu-id="01826-269">エイリアスの一意性</span><span class="sxs-lookup"><span data-stu-id="01826-269">Uniqueness of aliases</span></span>

<span data-ttu-id="01826-270">各コンパイル単位と名前空間の本体がの extern エイリアスを別の宣言領域とエイリアスを使用します。</span><span class="sxs-lookup"><span data-stu-id="01826-270">Each compilation unit and namespace body has a separate declaration space for extern aliases and using aliases.</span></span> <span data-ttu-id="01826-271">そのため、エイリアスを使用してまたは extern エイリアスの名前は、extern エイリアスのセット内で一意である必要があります、すぐに親のコンパイル単位または名前空間の本体で宣言されたエイリアスを使用して、エイリアスが型または名前空間の長さと同じ名前を持つことができますはt はでのみ使用、`::`修飾子。</span><span class="sxs-lookup"><span data-stu-id="01826-271">Thus, while the name of an extern alias or using alias must be unique within the set of extern aliases and using aliases declared in the immediately containing compilation unit or namespace body, an alias is permitted to have the same name as a type or namespace as long as it is used only with the `::` qualifier.</span></span>

<span data-ttu-id="01826-272">例</span><span class="sxs-lookup"><span data-stu-id="01826-272">In the example</span></span>
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
<span data-ttu-id="01826-273">名前`A`ため、2 つ目の名前空間の 2 つの可能性のある意味を持つクラスにも`A`を使用して、およびエイリアス`A`スコープ内にあります。</span><span class="sxs-lookup"><span data-stu-id="01826-273">the name `A` has two possible meanings in the second namespace body because both the class `A` and the using alias `A` are in scope.</span></span> <span data-ttu-id="01826-274">このため、使用`A`修飾名で`A.Stream`があいまいであり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="01826-274">For this reason, use of `A` in the qualified name `A.Stream` is ambiguous and causes a compile-time error to occur.</span></span> <span data-ttu-id="01826-275">ただしの使用`A`で、`::`修飾子エラーではないため、`A`名前空間の別名としてのみを検索します。</span><span class="sxs-lookup"><span data-stu-id="01826-275">However, use of `A` with the `::` qualifier is not an error because `A` is looked up only as a namespace alias.</span></span>
