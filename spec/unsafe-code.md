# <a name="unsafe-code"></a><span data-ttu-id="1f2fb-101">アンセーフ コード</span><span class="sxs-lookup"><span data-stu-id="1f2fb-101">Unsafe code</span></span>

<span data-ttu-id="1f2fb-102">前の章で定義されているように、core、c# 言語でのデータ型としてのポインターの省略、C および C++ とは大きく異なります。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="1f2fb-103">代わりに、c# には、参照、およびガベージ コレクターによって管理されているオブジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="1f2fb-104">このデザインは、その他の機能を組み合わせることによって、c# は、C や C++ よりもはるかにより安全な言語にします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="1f2fb-105">Core c# 言語で単にことはできません、初期化されていない変数、「未解決」のポインターまたは配列の境界を越えてのインデックスを作成する式。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="1f2fb-106">バグの全体カテゴリがちな C および C++ プログラムのため削除します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="1f2fb-107">C または C++ で実際上すべてのポインター型のコンス トラクター (C#)、対応する参照型には、それでも、状況は、ポインター型へのアクセスが必要になります。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="1f2fb-108">たとえば、基になるオペレーティング システムとやり取り、メモリ マップト デバイスへのアクセスやタイム クリティカルなアルゴリズムを実装できない可能性があります不可能または非現実的ポインターへのアクセスなし。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="1f2fb-109">このニーズに対処する c# 機能を提供します、書き込む***アンセーフ コード***します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="1f2fb-110">安全でないコードを宣言し、ポインターと、変数のアドレスを整数型の間の変換を実行する、ポインターに使用してなどです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="1f2fb-111">アンセーフ コードを記述することは、ある意味で、c# プログラム内での C コードの記述と似ています。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="1f2fb-112">アンセーフ コードは、実際には、開発者とユーザーの両方の観点からの「安全」機能です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="1f2fb-113">修飾子の付いた、アンセーフ コードを明確にマークする必要があります`unsafe`開発者の安全でない機能を誤って、使用可能性があることはできませんし、実行エンジンは信頼されていない環境でアンセーフ コードを実行できないようにするため、します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="1f2fb-114">Unsafe コンテキスト</span><span class="sxs-lookup"><span data-stu-id="1f2fb-114">Unsafe contexts</span></span>

<span data-ttu-id="1f2fb-115">C# の安全でない機能は、unsafe コンテキストでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="1f2fb-116">Unsafe コンテキストを導入、`unsafe`または使用することによって、型またはメンバーの宣言に修飾子を*unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="1f2fb-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="1f2fb-117">クラス、構造体、インターフェイス、またはデリゲートの宣言を含めることができます、`unsafe`修飾子でその型の宣言 (クラス、構造体、またはインターフェイスの本文を含む) の全体的なテキスト範囲の場合は、unsafe コンテキストと見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="1f2fb-118">フィールド、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンス コンス トラクター、デストラクター、または静的コンス トラクターの宣言を含めることができます、`unsafe`修飾子でそのメンバーの宣言の全体的なテキスト範囲の場合は、安全でないです。コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="1f2fb-119">*Unsafe_statement* 、unsafe コンテキスト内で使用できるように、*ブロック*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="1f2fb-120">全体的なテキスト範囲の関連付けられている*ブロック*は unsafe コンテキストと見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="1f2fb-121">関連付けられている文法は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-121">The associated grammar productions are shown below.</span></span>

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

<span data-ttu-id="1f2fb-122">例</span><span class="sxs-lookup"><span data-stu-id="1f2fb-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="1f2fb-123">`unsafe`構造体の宣言で指定した修飾子により、unsafe コンテキストに構造体の宣言の全体的なテキスト範囲。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="1f2fb-124">したがって、宣言することは、`Left`と`Right`ポインター型にするフィールド。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="1f2fb-125">上記の例も記述できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="1f2fb-126">ここでは、`unsafe`フィールドの宣言に修飾子がそれらの宣言に、unsafe コンテキストと見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="1f2fb-127">したがって、ポインター型の使用を許可する、unsafe コンテキストを確立する以外、`unsafe`修飾子は、型またはメンバーに影響を与えません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="1f2fb-128">例</span><span class="sxs-lookup"><span data-stu-id="1f2fb-128">In the example</span></span>

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

<span data-ttu-id="1f2fb-129">`unsafe`修飾子を`F`メソッド`A`のテキストの範囲が表示されるだけ`F`になる、unsafe コンテキストの言語の安全でない機能を使用できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="1f2fb-130">オーバーライドで`F`で`B`、再指定する必要はありません、`unsafe`修飾子--しない限り、もちろん、`F`メソッド`B`安全でない機能にアクセスする必要自体。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="1f2fb-131">ポインター型がメソッドのシグネチャの一部である場合は、状況は若干異なります</span><span class="sxs-lookup"><span data-stu-id="1f2fb-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

<span data-ttu-id="1f2fb-132">ここでは、ため`F`の署名には、ポインター型が含まれていることができますのみ記述する必要が、unsafe コンテキストでします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="1f2fb-133">ただし、unsafe コンテキストは、いずれかを行うクラス全体、安全でないでは、によって発生する可能性`A`、またはを含めることによって、`unsafe`メソッドの宣言に修飾子の場合と同様`B`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="1f2fb-134">ポインター型</span><span class="sxs-lookup"><span data-stu-id="1f2fb-134">Pointer types</span></span>

<span data-ttu-id="1f2fb-135">Unsafe コンテキストで、*型*([型](types.md)) 可能性があります、 *pointer_type*と同様に、 *value_type*または*reference_type*.</span><span class="sxs-lookup"><span data-stu-id="1f2fb-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="1f2fb-136">ただし、 *pointer_type*でも使用できます、`typeof`式 ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions))、unsafe コンテキストの外部でそのための使用は安全でないです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="1f2fb-137">A *pointer_type*として書き込まれますが、 *unmanaged_type*またはキーワード`void`、その後に、`*`トークン。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="1f2fb-138">前に指定された型、`*`ポインターの型が呼び出される、***参照先の型***ポインター型のです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="1f2fb-139">ポインター型の値が指す変数の型を表します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="1f2fb-140">(参照型の値) の参照とは異なり、ポインターは、ガベージ コレクターによって追跡されません--ガベージ コレクターがポインターをポイントするデータに関する知識を持たない。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="1f2fb-141">参照を指すポインターをこの理由は許可されていませんまたは構造体への参照を格納するいると、ポインターの参照先の型である必要があります、 *unmanaged_type*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="1f2fb-142">*Unmanaged_type*はない任意の型は、 *reference_type*または構築された型、およびが含まれていない*reference_type*または任意のレベルの型フィールドを構築入れ子にします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="1f2fb-143">つまり、 *unmanaged_type*は、次の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="1f2fb-144">`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、 `decimal`、または`bool`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="1f2fb-145">すべて*enum_type*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="1f2fb-146">すべて*pointer_type*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="1f2fb-147">ユーザー定義*struct_type*構築された型ではないのフィールドを含む*unmanaged_type*にのみです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="1f2fb-148">直感的なルールを混在ポインターと参照は、ポインターを含む参照 (オブジェクト) の参照が許可されますが、参照を格納するポインターの参照は許可されていませんが。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="1f2fb-149">ポインター型の例をいくつかは、次の表で与えられます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="1f2fb-150">__例__</span><span class="sxs-lookup"><span data-stu-id="1f2fb-150">__Example__</span></span> | <span data-ttu-id="1f2fb-151">__説明__</span><span class="sxs-lookup"><span data-stu-id="1f2fb-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="1f2fb-152">ポインター `byte`</span><span class="sxs-lookup"><span data-stu-id="1f2fb-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="1f2fb-153">ポインター `char`</span><span class="sxs-lookup"><span data-stu-id="1f2fb-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="1f2fb-154">ポインターへのポインター `int`</span><span class="sxs-lookup"><span data-stu-id="1f2fb-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="1f2fb-155">ポインターの 1 次元配列 `int`</span><span class="sxs-lookup"><span data-stu-id="1f2fb-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="1f2fb-156">不明な型へのポインター</span><span class="sxs-lookup"><span data-stu-id="1f2fb-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="1f2fb-157">特定の実装では、すべてのポインター型は、同じサイズと形式が必要です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="1f2fb-158">C および C++、c# では、同じ宣言で複数のポインターが宣言されている場合とは異なり、`*`は各ポインター名のプレフィックスなどの区切り記号としてではなく基になる型にのみ、と共に書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="1f2fb-159">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="1f2fb-160">型のポインターの値`T*`型の変数のアドレスを表す`T`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="1f2fb-161">ポインター間接演算子`*`([ポインターの間接参照](unsafe-code.md#pointer-indirection)) この変数にアクセスするために使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="1f2fb-162">たとえば、変数がある`P`型の`int*`、式`*P`表します、`int`変数に含まれるアドレスにある`P`。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="1f2fb-163">オブジェクト参照のようなポインターである可能性があります`null`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="1f2fb-164">間接演算子を適用すること、`null`実装定義の動作の結果のポインター。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="1f2fb-165">値を持つポインター`null`はすべてのビットが 0 で表されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="1f2fb-166">`void*`型が不明な型へのポインターを表します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="1f2fb-167">型のポインターに間接演算子は適用できません参照先の型が不明なため`void*`もこのようなポインターですべての算術演算を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="1f2fb-168">ただし、型のポインター`void*`他のポインター型 (その逆) にキャストすることができます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="1f2fb-169">ポインター型は、別の種類のカテゴリです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="1f2fb-170">継承しないポインター型の参照型と値型とは異なり`object`ポインター型間で変換が存在しないと、`object`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="1f2fb-171">具体的には、ボックス化とボックス化解除 ([ボックス化とボックス化解除](types.md#boxing-and-unboxing)) ポインターはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="1f2fb-172">ただし、変換は、異なるポインター型間で、ポインター型と整数型の間に許可されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="1f2fb-173">「[ポインター変換](unsafe-code.md#pointer-conversions)します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="1f2fb-174">A *pointer_type*型引数として使用することはできません ([構築型](types.md#constructed-types))、や型推論 ([型推論](expressions.md#type-inference)) であると推論されたジェネリック メソッドの呼び出しが失敗した、引数がポインター型を入力します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="1f2fb-175">A *pointer_type* volatile フィールドの型として使用することがあります ([Volatile フィールド](classes.md#volatile-fields))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="1f2fb-176">としてのポインターを渡すことができますが`ref`または`out`パラメーター、呼び出されたメソッドが戻るときに存在しないローカル変数またはその固定オブジェクトを指すポインターがない場合もあるため、未定義の動作が生じることを設定します。ポイントを使用すると、修正が不要になった。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="1f2fb-177">例えば:</span><span class="sxs-lookup"><span data-stu-id="1f2fb-177">For example:</span></span>

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

<span data-ttu-id="1f2fb-178">メソッドは、いくつかの型の値を返すことができ、その型のポインターであることができます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="1f2fb-179">たとえば、ポインターの連続したシーケンスに指定されている場合`int`s、そのシーケンスの要素の数とその他の`int`値、次のメソッドは、一致が発生した場合、その順序でその値のアドレスを返しますを返しますそれ以外の場合。`null`:</span><span class="sxs-lookup"><span data-stu-id="1f2fb-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

<span data-ttu-id="1f2fb-180">Unsafe コンテキストでは、いくつかの構成要素はポインターに対する操作のために使用できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="1f2fb-181">`*`ポインターの間接参照を実行する演算子を使用できます ([ポインターの間接参照](unsafe-code.md#pointer-indirection))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="1f2fb-182">`->`ポインターを通じて構造体のメンバーにアクセスする演算子を使用できます ([ポインターのメンバー アクセス](unsafe-code.md#pointer-member-access))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="1f2fb-183">`[]`演算子はポインターのインデックスを使用できる可能性があります ([ポインターの要素へのアクセス](unsafe-code.md#pointer-element-access))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="1f2fb-184">`&`変数のアドレスを取得する演算子を使用できます ([address-of 演算子](unsafe-code.md#the-address-of-operator))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="1f2fb-185">`++`と`--`にポインターをインクリメントおよびデクリメント演算子を使用する ([ポインターのインクリメントとデクリメント](unsafe-code.md#pointer-increment-and-decrement))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="1f2fb-186">`+`と`-`ポインターの算術演算を実行する演算子を使用する ([ポインターの算術演算](unsafe-code.md#pointer-arithmetic))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="1f2fb-187">`==`、 `!=`、 `<`、 `>`、 `<=`、および`=>`ポインターを比較する演算子を使用する ([ポインター比較](unsafe-code.md#pointer-comparison))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="1f2fb-188">`stackalloc`演算子を使用する呼び出しスタックからメモリを割り当てる ([固定サイズ バッファー](unsafe-code.md#fixed-size-buffers))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="1f2fb-189">`fixed`ステートメントは、一時的にそのアドレスを取得するために変数を解決するために使用可能性があります ([fixed ステートメント](unsafe-code.md#the-fixed-statement))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="1f2fb-190">固定と移動可能変数</span><span class="sxs-lookup"><span data-stu-id="1f2fb-190">Fixed and moveable variables</span></span>

<span data-ttu-id="1f2fb-191">Address-of 演算子 ([address-of 演算子](unsafe-code.md#the-address-of-operator)) および`fixed`ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) 2 つのカテゴリ変数に分割: ***の変数を固定***と***移動可能変数***します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="1f2fb-192">固定変数は、ガベージ コレクターの操作によって影響を受けません記憶域の場所に存在します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="1f2fb-193">(固定変数の例は、ローカル変数、パラメーターの値、およびポインターの逆参照によって作成された変数含まれます)。その一方で、移動可能な変数は、再配置やガベージ コレクターによって破棄対象である記憶域の場所に存在します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="1f2fb-194">(移動可能な変数の例に、オブジェクトと配列の要素にあるフィールドが含める)。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="1f2fb-195">`&`演算子 ([address-of 演算子](unsafe-code.md#the-address-of-operator)) に制限なしに取得する固定の変数のアドレスを許可します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="1f2fb-196">ただし、移動可能な変数は再配置やガベージ コレクターによって破棄されるため、移動可能な変数のアドレスのみを使って取得できます、`fixed`ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement))、およびそのアドレスその期間に対してのみ有効なまま`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="1f2fb-197">正確には、固定の変数は、次のいずれか。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="1f2fb-198">変数の結果から、 *simple_name* ([簡易名](expressions.md#simple-names)) 匿名関数で変数をキャプチャしない限り、ローカル変数または値を持つパラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="1f2fb-199">変数の結果から、 *member_access* ([メンバー アクセス](expressions.md#member-access)) フォームの`V.I`ここで、`V`の固定変数、 *struct_type*。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="1f2fb-200">変数の結果から、 *pointer_indirection_expression* ([ポインターの間接参照](unsafe-code.md#pointer-indirection)) フォームの`*P`、 *pointer_member_access* ([ポインターのメンバー アクセス](unsafe-code.md#pointer-member-access)) フォームの`P->I`、または*pointer_element_access* ([ポインターの要素へのアクセス](unsafe-code.md#pointer-element-access)) フォームの`P[E]`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="1f2fb-201">その他のすべての変数は、移動可能な変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="1f2fb-202">静的フィールドが移動可能な変数として分類されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="1f2fb-203">また、`ref`または`out`パラメーターは、パラメーターの指定された引数が固定の変数である場合でも、移動可能変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="1f2fb-204">最後に、ポインターを逆参照によって生成された変数は常に固定変数として分類することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="1f2fb-205">ポインター変換</span><span class="sxs-lookup"><span data-stu-id="1f2fb-205">Pointer conversions</span></span>

<span data-ttu-id="1f2fb-206">Unsafe コンテキストでは、使用可能な暗黙的な変換のセット ([暗黙的な変換](conversions.md#implicit-conversions)) が拡張され、次の暗黙的なポインター変換。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="1f2fb-207">いずれかから*pointer_type*型に`void*`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="1f2fb-208">`null`いずれかにリテラル*pointer_type*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="1f2fb-209">さらに、使用できる明示的な変換のセットを unsafe コンテキストで ([明示的な変換](conversions.md#explicit-conversions)) が拡張され、次の明示的なポインター変換。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="1f2fb-210">いずれかから*pointer_type*他*pointer_type*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="1f2fb-211">`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、または`ulong`いずれかに*pointer_type*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="1f2fb-212">いずれかから*pointer_type*に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、または`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="1f2fb-213">最後に、標準の暗黙的な変換のセットを unsafe コンテキストで ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) 次のポインター変換が含まれています。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="1f2fb-214">いずれかから*pointer_type*型に`void*`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="1f2fb-215">2 つのポインター型間の変換は、実際のポインター値を変更することはありません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="1f2fb-216">つまり、1 つのポインター型から別の変換は、ポインターによって指定された基になるアドレスへの影響を与えません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="1f2fb-217">Pointed-to 型に対して、結果のポインターが正しくアラインされない場合、1 つのポインター型は、変換するときに、動作がいる場合、結果を逆参照します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="1f2fb-218">一般に、「正しくアライン」という概念は推移的な: 型へのポインターの場合`A`型へのポインターが正しくアライン`B`であり、さらの型へのポインターの配置が正しく`C`、型へのポインター、`A`型へのポインターが正しくアライン`C`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="1f2fb-219">次のさまざまな型へのポインターを使用して 1 つの型を持つ変数にアクセスする場合を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="1f2fb-220">ポインター型が変換される場合、バイトへのポインター変数のアドレス指定された最下位バイトを結果のポイント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="1f2fb-221">一連の変数のサイズまで、結果のインクリメントでは、その変数の残りのバイトへのポインターを生成します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="1f2fb-222">たとえば、次のメソッドは、double 8 バイトの各 16 進数の値として表示します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="1f2fb-223">もちろん、生成される出力はエンディアンに依存します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="1f2fb-224">ポインターと整数間のマッピングでは、実装定義です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="1f2fb-225">ただし、32 の \* とリニア アドレス空間と 64 ビット CPU アーキテクチャ、整数型のポインターの変換通常動作への変換とまったく同じように`uint`または`ulong`にそれぞれ、または整数型の値。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="1f2fb-226">ポインターの配列</span><span class="sxs-lookup"><span data-stu-id="1f2fb-226">Pointer arrays</span></span>

<span data-ttu-id="1f2fb-227">Unsafe コンテキストでは、ポインターの配列を構築することができます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="1f2fb-228">ポインターの配列では、その他の配列型に適用される変換の一部のみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="1f2fb-229">暗黙的な参照変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions)) いずれかから*array_type*に`System.Array`も実装するインターフェイスは、ポインターの配列に適用されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="1f2fb-230">配列の要素にアクセスしようとするただし、`System.Array`またはインターフェイスの実装は、例外が実行時に、ポインター型に変換できない`object`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="1f2fb-231">暗黙的および明示的な参照変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions)、[明示的な参照変換](conversions.md#explicit-reference-conversions)) 1 次元配列型から`S[]`に`System.Collections.Generic.IList<T>`とジェネリックの基本インターフェイスは、型の引数としてポインター型を使用することはできませんし、ポインター型から非ポインター型への変換がないために、ポインターの配列に適用しません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="1f2fb-232">明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) から`System.Array`いずれかに、インターフェイスを実装および*array_type*ポインターの配列に適用されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="1f2fb-233">明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) から`System.Collections.Generic.IList<S>`1 次元の配列型をその基本インターフェイスと`T[]`ポインター型にすることはできませんので、ポインターの配列には適用されません使用される引数を入力として、ポインター型から非ポインター型への変換はありません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="1f2fb-234">これらの制限を意味するの拡張、`foreach`で説明されているステートメントで配列に対する[foreach ステートメント](statements.md#the-foreach-statement)ポインターの配列には適用できません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="1f2fb-235">代わりに、フォームの foreach ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f2fb-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="1f2fb-236">場所の種類`x`形式の配列型は、 `T[,,...,]`、`N`ディメンションから 1 を引いた数と`T`または`V`ポインター型は、次のように for ループで入れ子になったを使用して展開します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

<span data-ttu-id="1f2fb-237">変数`a`、 `i0`、 `i1`,...,`iN`を表示またはアクセスではない`x`または*embedded_statement*またはプログラムの他のソース コード。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="1f2fb-238">変数`v`は埋め込みステートメントでは読み取り専用です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="1f2fb-239">明示的な変換がない場合 ([ポインター変換](unsafe-code.md#pointer-conversions)) から`T`(要素の型) に`V`あり、エラーが生成されるため、以降の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="1f2fb-240">場合`x`、値を持つ`null`、`System.NullReferenceException`が実行時にスローされます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="1f2fb-241">式のポインター</span><span class="sxs-lookup"><span data-stu-id="1f2fb-241">Pointers in expressions</span></span>

<span data-ttu-id="1f2fb-242">Unsafe コンテキストでは、式は、ポインター型の結果を生成可能性がありますが、unsafe コンテキストの外側はポインター型の式のコンパイル時エラー。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="1f2fb-243">正確には、unsafe コンテキストの外部コンパイル時エラーが発生する場合は、すべて*simple_name* ([簡易名](expressions.md#simple-names))、 *member_access* ([メンバーへのアクセス](expressions.md#member-access))、 *invocation_expression* ([Invocation 式](expressions.md#invocation-expressions))、または*element_access* ([要素へのアクセス](expressions.md#element-access)) ポインター型です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="1f2fb-244">Unsafe コンテキストで、 *primary_no_array_creation_expression* ([一次式](expressions.md#primary-expressions)) と*unary_expression* ([の単項演算子](expressions.md#unary-operators))運用では、次の追加の構成要素を許可します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

<span data-ttu-id="1f2fb-245">これらのコンストラクトは、次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="1f2fb-246">優先順位と安全でない演算子の結合規則を文法で暗黙的に指定するとします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="1f2fb-247">ポインターの間接参照</span><span class="sxs-lookup"><span data-stu-id="1f2fb-247">Pointer indirection</span></span>

<span data-ttu-id="1f2fb-248">A *pointer_indirection_expression*アスタリスクで構成されます (`*`) 後に、 *unary_expression*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="1f2fb-249">単項`*`演算子はポインターの間接参照を表し、ポインターが指す変数を取得するために使用します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="1f2fb-250">評価結果`*P`ここで、`P`ポインター型の式は、 `T*`、型の変数は、`T`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="1f2fb-251">単項を適用すると、コンパイル時エラー`*`型の式に演算子`void*`またはポインター型のない式にします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="1f2fb-252">単項を適用する効果`*`演算子を`null`ポインターは、実装定義です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="1f2fb-253">具体的には、この操作でスローされる保証はありません、`System.NullReferenceException`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="1f2fb-254">無効な値はポインターでは、単項の動作に割り当てられている場合`*`演算子は定義されていません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="1f2fb-255">単項ポインターを逆参照に無効な値の間で`*`演算子が不適切な配置の種類が指すアドレス (例を参照してください[ポインター変換](unsafe-code.md#pointer-conversions)) と後に変数のアドレス、存続期間が終了します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="1f2fb-256">形式の式を評価することによって確実な代入分析のために、変数が生成される`*P`は最初に割り当てられていると見なされます ([変数を最初に割り当てられた](variables.md#initially-assigned-variables))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="1f2fb-257">ポインターのメンバー アクセス</span><span class="sxs-lookup"><span data-stu-id="1f2fb-257">Pointer member access</span></span>

<span data-ttu-id="1f2fb-258">A *pointer_member_access*から成る、 *primary_expression*、その後に、"`->`"トークン、その後に、*識別子*と省略可能な*type_argument_list*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="1f2fb-259">フォームのポインターのメンバー アクセスで`P->I`、`P`以外のポインター型の式を指定する必要があります`void*`、および`I`型のアクセス可能なメンバーを表す必要があります`P`ポイント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="1f2fb-260">フォームのポインターのメンバー アクセス`P->I`とまったく同じ評価`(*P).I`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="1f2fb-261">ポインター間接演算子の説明については (`*`) を参照してください[ポインターの間接参照](unsafe-code.md#pointer-indirection)します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="1f2fb-262">メンバー アクセス演算子の説明については (`.`) を参照してください[メンバー アクセス](expressions.md#member-access)。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="1f2fb-263">例</span><span class="sxs-lookup"><span data-stu-id="1f2fb-263">In the example</span></span>

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

<span data-ttu-id="1f2fb-264">`->`フィールドにアクセスし、ポインターを介して構造体のメソッドを呼び出す演算子を使用します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="1f2fb-265">操作`P->I`とまったく同じ`(*P).I`、`Main`メソッドも同じように記述できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a><span data-ttu-id="1f2fb-266">ポインターの要素へのアクセス</span><span class="sxs-lookup"><span data-stu-id="1f2fb-266">Pointer element access</span></span>

<span data-ttu-id="1f2fb-267">A *pointer_element_access*から成る、 *primary_no_array_creation_expression*で囲まれた式の後に"`[`「と」`]`"。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="1f2fb-268">ポインターの要素には、フォームへのアクセスで`P[E]`、`P`以外のポインター型の式を指定する必要があります`void*`、および`E`に暗黙的に変換できる式を指定する必要があります`int`、 `uint`、 `long`、または`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="1f2fb-269">ポインターの要素には、フォームへのアクセス`P[E]`とまったく同じ評価`*(P + E)`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="1f2fb-270">ポインター間接演算子の説明については (`*`) を参照してください[ポインターの間接参照](unsafe-code.md#pointer-indirection)します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="1f2fb-271">ポインターの加算演算子の説明については (`+`) を参照してください[ポインターの算術演算](unsafe-code.md#pointer-arithmetic)します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="1f2fb-272">例</span><span class="sxs-lookup"><span data-stu-id="1f2fb-272">In the example</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

<span data-ttu-id="1f2fb-273">ポインターの要素へのアクセスが内で文字バッファーを初期化するために使用される、`for`ループします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="1f2fb-274">操作`P[E]`とまったく同じ`*(P + E)`例では、同じように記述できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

<span data-ttu-id="1f2fb-275">ポインターの要素アクセス演算子は範囲外のチェックせずエラーと、動作にアクセスする場合、要素が定義されている範囲外です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="1f2fb-276">これは、C および C++ と同じです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="1f2fb-277">Address-of 演算子</span><span class="sxs-lookup"><span data-stu-id="1f2fb-277">The address-of operator</span></span>

<span data-ttu-id="1f2fb-278">*Addressof_expression*アンパサンドで構成されます (`&`) 後に、 *unary_expression*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="1f2fb-279">式が指定`E`一種の`T`固定変数として分類されます ([固定属性と移動可能変数](unsafe-code.md#fixed-and-moveable-variables))、コンストラクト`&E`によって指定される変数のアドレスを計算`E`.</span><span class="sxs-lookup"><span data-stu-id="1f2fb-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="1f2fb-280">結果の型が`T*`値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="1f2fb-281">コンパイル時エラーが発生します`E`場合、変数として分類されない`E`は読み取り専用のローカル変数として分類または`E`移動可能な変数を表します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="1f2fb-282">最後の場合、fixed ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement))、変数のアドレスを取得する前に、変数を一時的に「修正」するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="1f2fb-283">説明したように[メンバー アクセス](expressions.md#member-access)のインスタンス コンス トラクターまたは構造体を定義するクラスの静的コンス トラクターの外部、`readonly`フィールドに、そのフィールドは変数ではなく、値と見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="1f2fb-284">そのため、そのアドレスを取得することはできません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="1f2fb-285">同様に、定数のアドレスを取得することはできません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="1f2fb-286">`&`演算子が明示的に代入が次の引数を必要としない、`&`操作、演算子が適用される変数と見なされます、操作が発生する実行パスで明示的に代入します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="1f2fb-287">変数の適切な初期化を確認するプログラマの責任は実際にこのような状況を実行をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="1f2fb-288">例</span><span class="sxs-lookup"><span data-stu-id="1f2fb-288">In the example</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="1f2fb-289">`i` 明示的に代入と見なされます、`&i`初期化するために使用される操作`p`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="1f2fb-290">代入`*p`有効な初期化`i`が、この初期化を含めることは、プログラマの責任と割り当てが削除された場合はコンパイル時エラーが発生しません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="1f2fb-291">確実な代入ルール、`&`演算子存在することでローカル変数の冗長な初期化を回避できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="1f2fb-292">たとえば、外部 Api の多くはその API によって格納される構造へのポインターです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="1f2fb-293">このような Api の呼び出しは通常、構造体のローカル変数のアドレスを渡すし、規則がない構造体変数の冗長な初期化が必要になります。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="1f2fb-294">ポインターのインクリメントとデクリメント</span><span class="sxs-lookup"><span data-stu-id="1f2fb-294">Pointer increment and decrement</span></span>

<span data-ttu-id="1f2fb-295">Unsafe コンテキストで、`++`と`--`演算子 ([置インクリメント演算子と前置デクリメント演算子](expressions.md#postfix-increment-and-decrement-operators)と[前置インクリメントとデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)) ポインターに適用できます除くすべての型の変数`void*`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="1f2fb-296">したがって、すべてのポインター型の`T*`、次の演算子が暗黙的に定義されています。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="1f2fb-297">演算子と同じ結果を生成する`x + 1`と`x - 1`、それぞれ ([ポインターの算術演算](unsafe-code.md#pointer-arithmetic))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="1f2fb-298">つまり、型のポインター変数の`T*`、`++`演算子を追加`sizeof(T)`、変数に含まれるアドレスを`--`演算子が減算`sizeof(T)`変数に含まれるアドレスから。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="1f2fb-299">ポインターのインクリメントまたはデクリメント演算がポインター型のドメインをオーバーフロー、実装定義になりますが、例外は生成されません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="1f2fb-300">ポインターの算術演算</span><span class="sxs-lookup"><span data-stu-id="1f2fb-300">Pointer arithmetic</span></span>

<span data-ttu-id="1f2fb-301">Unsafe コンテキストでは、`+`と`-`演算子 ([加算演算子](expressions.md#addition-operator)と[減算演算子](expressions.md#subtraction-operator)) を除くすべてのポインター型の値に適用できる`void*`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="1f2fb-302">したがって、すべてのポインター型の`T*`、次の演算子が暗黙的に定義されています。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

<span data-ttu-id="1f2fb-303">式が指定`P`ポインター型の`T*`と式`N`型の`int`、 `uint`、 `long`、または`ulong`、式`P + N`と`N + P`コンピューティング、型のポインター値`T*`を加算した結果を`N * sizeof(T)`によって指定されたアドレスに`P`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="1f2fb-304">式では同様に、`P - N`型のポインター値を計算`T*`を減算した結果の`N * sizeof(T)`によって指定されたアドレスから`P`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="1f2fb-305">2 つの式を指定された`P`と`Q`、ポインター型の`T*`、式`P - Q`で指定されたアドレスの差を計算`P`と`Q`でその違いを除算し、`sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="1f2fb-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="1f2fb-306">結果の型は常に`long`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-306">The type of the result is always `long`.</span></span> <span data-ttu-id="1f2fb-307">実際には、`P - Q`として計算されます`((long)(P) - (long)(Q)) / sizeof(T)`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="1f2fb-308">例えば:</span><span class="sxs-lookup"><span data-stu-id="1f2fb-308">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

<span data-ttu-id="1f2fb-309">これには、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-309">which produces the output:</span></span>

```
p - q = -14
q - p = 14
```

<span data-ttu-id="1f2fb-310">ポインターの算術演算がポインター型のドメインをオーバーフローした場合は、実装で定義された方法で、結果が切り捨てられますが、例外は生成されません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="1f2fb-311">ポインターの比較</span><span class="sxs-lookup"><span data-stu-id="1f2fb-311">Pointer comparison</span></span>

<span data-ttu-id="1f2fb-312">Unsafe コンテキストで、 `==`、 `!=`、 `<`、 `>`、 `<=`、および`=>`演算子 ([関係式と型検査演算子](expressions.md#relational-and-type-testing-operators)) すべての値に適用できますポインターの型。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="1f2fb-313">ポインターの比較演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="1f2fb-314">ポインター型からの暗黙的な変換が存在するので、`void*`型、任意のポインター型のオペランドは、これらの演算子を使用して比較できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="1f2fb-315">比較演算子は、符号なし整数として 2 つのオペランドで指定されるアドレスを比較します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="1f2fb-316">Sizeof 演算子</span><span class="sxs-lookup"><span data-stu-id="1f2fb-316">The sizeof operator</span></span>

<span data-ttu-id="1f2fb-317">`sizeof`演算子は、指定した型の変数によって占有されるバイト数を返します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="1f2fb-318">オペランドとして指定された型`sizeof`必要があります、 *unmanaged_type* ([ポインター型](unsafe-code.md#pointer-types))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="1f2fb-319">結果、`sizeof`演算子は、型の値を`int`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="1f2fb-320">特定の組み込み型の場合、`sizeof`演算子は、次の表に示すように、定数値を生成します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="1f2fb-321">__式__</span><span class="sxs-lookup"><span data-stu-id="1f2fb-321">__Expression__</span></span>   | <span data-ttu-id="1f2fb-322">__結果__</span><span class="sxs-lookup"><span data-stu-id="1f2fb-322">__Result__</span></span> |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

<span data-ttu-id="1f2fb-323">その他の種類の結果を`sizeof`演算子の実装で定義および定数ではなく、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="1f2fb-324">メンバーが構造体にパックされている順序では、指定されていません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="1f2fb-325">配置のためにある可能性がありますしない埋め込み構造体、内の構造体の先頭に、構造体の最後に名前付き。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="1f2fb-326">埋め込み文字として使用されるビットの内容は不確定です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="1f2fb-327">構造体型のオペランドに適用する場合の埋め込みを含む、その型の変数のバイト数の合計数になります。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="1f2fb-328">Fixed ステートメント</span><span class="sxs-lookup"><span data-stu-id="1f2fb-328">The fixed statement</span></span>

<span data-ttu-id="1f2fb-329">Unsafe コンテキストで、 *embedded_statement* ([ステートメント](statements.md)) 運用により、追加の構築、`fixed`ステートメントでは、移動可能な変数を「修正」するために使用するよう、ステートメントの実行中にアドレスが定数のままになります。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

<span data-ttu-id="1f2fb-330">各*fixed_pointer_declarator*のローカル変数を宣言して、指定された*pointer_type* 、対応することによって計算されたアドレスでそのローカル変数を初期化*fixed_pointer_initializer*します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="1f2fb-331">宣言されたローカル変数、`fixed`ステートメントがいずれかでアクセス可能な*fixed_pointer_initializer*秒と、その変数の宣言の右側に発生している、 *embedded_statement*の`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="1f2fb-332">宣言されたローカル変数、`fixed`ステートメントは読み取り専用と見なされます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="1f2fb-333">埋め込みステートメントをこのローカル変数を変更しようとすると、コンパイル時エラーが発生します (割り当てまたは`++`と`--`演算子) として渡すか、`ref`または`out`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="1f2fb-334">A *fixed_pointer_initializer*次のいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="1f2fb-335">トークン"`&`"続けて、 *variable_reference* ([確実な代入を決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) 移動可能な変数に ([固定属性と移動可能変数](unsafe-code.md#fixed-and-moveable-variables))アンマネージ型の`T`、型指定された`T*`指定されたポインター型に暗黙的に変換されて、`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="1f2fb-336">この場合、初期化子は、指定された変数のアドレスを計算し、変数の間の固定アドレスで維持することが保証されます、`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="1f2fb-337">式、 *array_type*アンマネージ型の要素を含む`T`、型指定された`T*`指定されたポインター型に暗黙的に変換できる、`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="1f2fb-338">この場合、初期化子は、配列の最初の要素のアドレスを計算し、配列全体の期間の固定アドレスで維持することが保証されます、`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="1f2fb-339">配列式が null の場合、または配列の要素が 0 の場合は、初期化子は、アドレスに値が 0 を計算します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="1f2fb-340">型の式`string`、型指定された`char*`指定されたポインター型に暗黙的に変換されて、`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="1f2fb-341">この場合、初期化子は、文字列の最初の文字のアドレスを計算し、文字列全体の期間の固定アドレスで維持することが保証されます、`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="1f2fb-342">動作、`fixed`ステートメントは、実装で定義された文字列式が null の場合。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="1f2fb-343">A *simple_name*または*member_access*指定された固定サイズ バッファーのメンバーの型が指定されたポインター型に暗黙的に変換、移動可能な変数の固定サイズ バッファーのメンバーを参照します。`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="1f2fb-344">この場合、初期化子が固定サイズ バッファーの最初の要素へのポインターを計算 ([式で固定サイズ バッファー](unsafe-code.md#fixed-size-buffers-in-expressions))、固定サイズ バッファー、の間の固定アドレスで維持することが保証されます`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="1f2fb-345">によって計算された各アドレスに対して、 *fixed_pointer_initializer* 、`fixed`ステートメントにより再配置やの間、ガベージ コレクターによって破棄される、アドレスによって参照される変数がありませんが、`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="1f2fb-346">によってアドレスが計算された場合など、 *fixed_pointer_initializer*オブジェクトのフィールドまたは配列インスタンスの要素を参照して、`fixed`ステートメントでは、オブジェクト インスタンスが再配置されないことが保証されますかステートメントの有効期間中に破棄します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="1f2fb-347">ポインターがによって作成されたことを確認するプログラマの役目です`fixed`ステートメントは、これらのステートメントの実行終了後は保持されません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="1f2fb-348">ポインターが作成した場合など、`fixed`ステートメントは、外部 Api に渡されるはプログラマの Api がこれらのポインターのメモリを保持しないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="1f2fb-349">(ため、それらを移動することはできません) 固定オブジェクト ヒープの断片化が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="1f2fb-350">そのため、絶対に必要な場合にのみオブジェクトは固定する必要があり、復元可能なの最短時間に対してのみです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="1f2fb-351">例では、</span><span class="sxs-lookup"><span data-stu-id="1f2fb-351">The example</span></span>

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

<span data-ttu-id="1f2fb-352">いくつかの用途を示します、`fixed`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="1f2fb-353">最初のステートメントを修正し、静的フィールドのアドレスを取得、2 番目のステートメントを修正インスタンス フィールドのアドレスを取得と 3 番目のステートメントを修正し、配列の要素のアドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="1f2fb-354">各ケースにされていると、正規表現を使用するとエラー`&`演算子ので、変数がすべて移動可能な変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="1f2fb-355">4 番目`fixed`ステートメント上の例で、3 番目に同様の結果を生成します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="1f2fb-356">この例の`fixed`ステートメント使用`string`:</span><span class="sxs-lookup"><span data-stu-id="1f2fb-356">This example of the `fixed` statement uses `string`:</span></span>

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

<span data-ttu-id="1f2fb-357">インデックスから始まって増加のインデックス順で unsafe コンテキストでは 1 次元配列の配列要素が格納されている`0`インデックスで終わる`Length - 1`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="1f2fb-358">多次元配列、右端の次元のインデックスを最初に、増加するように要素が格納された配列の次次ディメンション、という具合に左、左。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="1f2fb-359">内で、`fixed`ポインターを取得するステートメント`p`配列インスタンスへ`a`、ポインター値から`p`に`p + a.Length - 1`配列内の要素のアドレスを表します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="1f2fb-360">同様に、範囲変数`p[0]`に`p[a.Length - 1]`実際の配列要素を表します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="1f2fb-361">配列が格納されている方法を指定するには、扱うことができます任意の次元の配列線形ものとして。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="1f2fb-362">例えば:</span><span class="sxs-lookup"><span data-stu-id="1f2fb-362">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

<span data-ttu-id="1f2fb-363">これには、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-363">which produces the output:</span></span>

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="1f2fb-364">例</span><span class="sxs-lookup"><span data-stu-id="1f2fb-364">In the example</span></span>

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

<span data-ttu-id="1f2fb-365">`fixed`ステートメントを使用して配列を修正するため、そのアドレスは、ポインターを受け取るメソッドに渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="1f2fb-366">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-366">In the example:</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

<span data-ttu-id="1f2fb-367">fixed ステートメントは、そのアドレスをポインターとして使用できるように、構造体の固定サイズ バッファーの修正に使用されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="1f2fb-368">A`char*`修正は文字列のインスタンスは常に null で終わる文字列へのポインターによって生成された値。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="1f2fb-369">ポインターを取得する固定ステートメント内で`p`文字列インスタンスを`s`、ポインター値から`p`に`p + s.Length - 1`、文字列、およびポインターの値の文字のアドレスを表す`p + s.Length`常に null 文字を指します (値を持つ文字`'\0'`)。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="1f2fb-370">固定されたポインターを通じてマネージ型のオブジェクトを変更するには、未定義の動作の結果ことができます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="1f2fb-371">たとえば、文字列は変更できないため、固定文字列を指すポインターにより参照されている文字が変更されていないことを確認するはプログラマの役目は。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="1f2fb-372">文字列の null の自動終了は、"C"スタイルの文字列を期待する外部 Api を呼び出すときに便利です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="1f2fb-373">ただし、文字列のインスタンスが null 文字を許可されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="1f2fb-374">Null で終わるとして扱われた場合に、このような null 文字が存在する場合、文字列は切り詰められて表示されます`char*`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="1f2fb-375">固定サイズ バッファー</span><span class="sxs-lookup"><span data-stu-id="1f2fb-375">Fixed size buffers</span></span>

<span data-ttu-id="1f2fb-376">固定サイズ バッファーを使用して、構造体のメンバーとして「C スタイル」行で配列を宣言しは主にアンマネージ Api とのやり取りに使用します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="1f2fb-377">固定サイズ バッファーの宣言</span><span class="sxs-lookup"><span data-stu-id="1f2fb-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="1f2fb-378">A***固定サイズ バッファー***記憶域の指定された型の変数の固定長バッファーを表すメンバーします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="1f2fb-379">固定サイズ バッファーの宣言では、指定された要素型の 1 つまたは複数の固定サイズ バッファーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="1f2fb-380">固定サイズ バッファーは構造体の宣言でのみ許可されますが、unsafe コンテキストでのみ発生することができます ([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

<span data-ttu-id="1f2fb-381">固定サイズ バッファーの宣言は、一連の属性を含めることができます ([属性](attributes.md))、`new`修飾子 ([修飾子](classes.md#modifiers))、有効な 4 つのアクセス修飾子の組み合わせ ([型パラメーターや制約](classes.md#type-parameters-and-constraints)) および`unsafe`修飾子 ([Unsafe コンテキスト](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="1f2fb-382">属性と修飾子は、すべての固定サイズ バッファーの宣言で宣言されたメンバーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="1f2fb-383">同じ修飾子を固定サイズ バッファーの宣言内で複数回のエラーになります。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="1f2fb-384">固定サイズ バッファーの宣言を含めることはできません、`static`修飾子。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="1f2fb-385">バッファーの固定サイズ バッファーの宣言の要素型では、宣言によって導入された、バッファーの要素の型を指定します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="1f2fb-386">バッファー要素の型は、定義済みの型のいずれかを指定する必要があります`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、または`bool`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="1f2fb-387">バッファー要素の型には、新しいメンバーを追加の固定サイズ バッファーの宣言子のリストが続きます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="1f2fb-388">固定サイズ バッファーの宣言子から成る識別子で囲まれた定数式の後に、メンバーの名前を示す`[`と`]`トークンです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="1f2fb-389">定数式は、その固定サイズ バッファーの宣言子によって導入されるメンバー内の要素の数を表しています。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="1f2fb-390">定数式の型は、型に暗黙的に変換可能である必要があります`int`値は 0 以外の正の整数である必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="1f2fb-391">固定サイズ バッファーの要素にメモリ内で順番にレイアウトすることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="1f2fb-392">複数の固定サイズ バッファーを宣言する固定サイズ バッファーの宣言は、同じ属性および要素型を持つ 1 つの固定サイズ バッファーの宣言の複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="1f2fb-393">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="1f2fb-394">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="1f2fb-395">式で固定サイズ バッファー</span><span class="sxs-lookup"><span data-stu-id="1f2fb-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="1f2fb-396">メンバー ルックアップ ([演算子](expressions.md#operators)) 固定サイズのバッファーのメンバーがフィールドのメンバーの検索とまったく同じように進みます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="1f2fb-397">固定サイズ バッファーを使用する式で参照できます、 *simple_name* ([型推論](expressions.md#type-inference)) または*member_access* ([コンパイル時のチェック動的なオーバー ロードの解決](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="1f2fb-398">形式のメンバー アクセスと同じ効果は、固定サイズ バッファーのメンバーは、簡易名として参照された場合、`this.I`ここで、`I`は固定サイズ バッファーのメンバーです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="1f2fb-399">形式のメンバー アクセスで`E.I`場合は、`E`は構造体の型とメンバーを参照`I`構造体の型が固定サイズ メンバーを識別に`E.I`は分類および次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="1f2fb-400">場合、式`E.I`は発生しません、unsafe コンテキストでは、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="1f2fb-401">場合`E`コンパイル時エラーが発生した、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="1f2fb-402">の場合`E`移動可能な変数は、([固定属性と移動可能変数](unsafe-code.md#fixed-and-moveable-variables)) と式`E.I`でない、 *fixed_pointer_initializer* ([固定ステートメント](unsafe-code.md#the-fixed-statement))、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="1f2fb-403">それ以外の場合、`E`固定変数参照式の結果は固定サイズ バッファーのメンバーの最初の要素へのポインターと`I`で`E`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="1f2fb-404">結果は型`S*`ここで、`S`の要素の型は、 `I`、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="1f2fb-405">固定サイズ バッファーの後続の要素は、最初の要素からポインター操作を使用してアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="1f2fb-406">、配列へのアクセスとは異なり、固定サイズ バッファーの要素へのアクセスは、安全でない操作は、し、範囲のチェックではありません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="1f2fb-407">次の例では、宣言し、固定サイズ バッファーのメンバーを持つ構造体を使用します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a><span data-ttu-id="1f2fb-408">確実な代入をチェック</span><span class="sxs-lookup"><span data-stu-id="1f2fb-408">Definite assignment checking</span></span>

<span data-ttu-id="1f2fb-409">固定サイズ バッファーは確実な代入をチェック対象になりません ([確実な代入](variables.md#definite-assignment)) と確実な代入の構造体の型の変数のチェックの目的での固定サイズ バッファーのメンバーは無視されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="1f2fb-410">固定サイズ バッファーのメンバーの最も外側にある含む構造体変数は、静的変数、クラスのインスタンス、または配列要素のインスタンス変数が、固定サイズ バッファーの要素は、既定値に自動的に初期化 ([既定値](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="1f2fb-411">その他のすべての場合は、固定サイズ バッファーの初期コンテンツは定義されません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="1f2fb-412">スタック割り当て</span><span class="sxs-lookup"><span data-stu-id="1f2fb-412">Stack allocation</span></span>

<span data-ttu-id="1f2fb-413">Unsafe コンテキストでは、ローカル変数宣言 ([ローカル変数宣言](statements.md#local-variable-declarations)) が、呼び出し履歴からメモリを割り当てられます。 スタック割り当ての初期化子を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="1f2fb-414">*Unmanaged_type*新しく割り当てられた場所に格納される項目の種類を示す、*式*これらの項目の数を示します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="1f2fb-415">これらの必要な割り当てサイズを指定、まとめて扱わします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="1f2fb-416">として項目の数を指定すると、コンパイル時エラーがスタック割り当てのサイズは、負の値にすることはできません、ため、 *constant_expression*負の値に評価されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="1f2fb-417">フォームのスタック割り当ての初期化子`stackalloc T[E]`必要があります`T`アンマネージ型にする ([ポインター型](unsafe-code.md#pointer-types)) と`E`型の式を指定する`int`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="1f2fb-418">構造`E * sizeof(T)`呼び出しからのバイトがスタックが作成され、型のポインターを返します`T*`、新しく割り当てられたブロックにします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="1f2fb-419">場合`E`負の値が、動作は未定義です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="1f2fb-420">場合`E`0、し、割り当てが行われていないと返されるポインターは、実装定義です。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="1f2fb-421">特定のサイズのブロックを割り当てることができる十分なメモリがない場合、`System.StackOverflowException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="1f2fb-422">新しく割り当てられたメモリの内容は定義されません。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="1f2fb-423">スタック割り当ての初期化子で許可されない`catch`または`finally`ブロック ([try ステートメント](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="1f2fb-424">使用して割り当てられたメモリを明示的に解放する方法はありません`stackalloc`します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="1f2fb-425">その関数のメンバーが返されるときに、関数メンバーの実行中に作成されたすべてのスタックが割り当てられたメモリ ブロックが自動的に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="1f2fb-426">これに対応して、`alloca`関数では、C および C++ の実装では一般的な拡張機能。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="1f2fb-427">例</span><span class="sxs-lookup"><span data-stu-id="1f2fb-427">In the example</span></span>

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

<span data-ttu-id="1f2fb-428">`stackalloc`で初期化子が使用される、`IntToString`スタックに 16 文字のバッファーを割り当てるためのメソッド。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="1f2fb-429">バッファーは、メソッドが戻るときに自動的に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="1f2fb-430">動的メモリの割り当て</span><span class="sxs-lookup"><span data-stu-id="1f2fb-430">Dynamic memory allocation</span></span>

<span data-ttu-id="1f2fb-431">を除き、`stackalloc`演算子 (C#) は用意されていません定義済みの構造には、ガベージ収集されたメモリを管理します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="1f2fb-432">そのようなサービスは通常のクラス ライブラリをサポートすることによって提供されるまたは、基になるオペレーティング システムから直接インポートします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="1f2fb-433">たとえば、`Memory`次に示すクラスは可能性があります、基になるオペレーティング システムのヒープ関数を c# からアクセスする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

<span data-ttu-id="1f2fb-434">使用する例を`Memory`クラスのとおりです。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

<span data-ttu-id="1f2fb-435">例では、割り当ての 256 バイトのメモリを`Memory.Alloc`0 から 255 までの値でメモリ ブロックを初期化します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="1f2fb-436">次に、256 の要素のバイト配列を使用して`Memory.Copy`バイト配列にメモリ ブロックの内容をコピーします。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="1f2fb-437">メモリ ブロックが解放を使用して最後に、`Memory.Free`し、バイト配列の内容は、コンソールに出力します。</span><span class="sxs-lookup"><span data-stu-id="1f2fb-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
