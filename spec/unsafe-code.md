---
ms.openlocfilehash: 4faef9a12bdff54fa59a55a0206fa72bda4ea585
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704060"
---
# <a name="unsafe-code"></a><span data-ttu-id="fcd7c-101">アンセーフ コード</span><span class="sxs-lookup"><span data-stu-id="fcd7c-101">Unsafe code</span></span>

<span data-ttu-id="fcd7c-102">コアC#言語は、前の章で定義されているように、 C++ C と、データ型としてのポインターの省略によって異なります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="fcd7c-103">代わりに、 C#には、ガベージコレクターによって管理されるオブジェクトを作成するための参照と機能が用意されています。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="fcd7c-104">この設計は、他の機能と組み合わせC#て、C またはC++よりもはるかに安全な言語になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="fcd7c-105">コアC#言語では、初期化されていない変数、"ぶら下がり" ポインター、または配列がその境界を越えてインデックスを作成する式を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="fcd7c-106">このため、C とC++プログラムが定期的に消滅するバグのカテゴリ全体が除外されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="fcd7c-107">これに対して、C のすべてのC++ポインター型の構造体やC#、に対応する参照型がありますが、ポインター型へのアクセスが必要になる状況もあります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="fcd7c-108">たとえば、基になるオペレーティングシステムとのやり取り、メモリマップトデバイスへのアクセス、または時間の重要なアルゴリズムの実装は、ポインターにアクセスしなくても不可能であるか、実用的でない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="fcd7c-109">このニーズに対処するC#ために、には***unsafe コード***を記述する機能が用意されています。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="fcd7c-110">アンセーフコードでは、ポインターを宣言して操作したり、ポインターと整数型の間の変換を実行したり、変数のアドレスを取得したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="fcd7c-111">わかりやすいように、unsafe コードを記述することは、 C#プログラム内で C コードを記述することとよく似ています。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="fcd7c-112">アンセーフコードは、実際には開発者とユーザーの両方から見た "安全な" 機能です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="fcd7c-113">アンセーフコードは、修飾子 @no__t 0 に設定する必要があります。そのため、開発者は安全でない機能を誤って使用することはできません。また、安全でないコードを信頼されていない環境で実行できないように実行エンジンが動作します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="fcd7c-114">Unsafe コンテキスト</span><span class="sxs-lookup"><span data-stu-id="fcd7c-114">Unsafe contexts</span></span>

<span data-ttu-id="fcd7c-115">のC#安全でない機能は、unsafe コンテキストでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="fcd7c-116">Unsafe コンテキストは、型またはメンバーの宣言に @no__t 0 修飾子を含めるか、 *unsafe_statement*を使って追加することによって導入されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="fcd7c-117">クラス、構造体、インターフェイス、またはデリゲートの宣言には @no__t 0 修飾子を含めることができます。この場合、その型宣言のテキスト範囲全体 (クラス、構造体、またはインターフェイスの本体を含む) は、安全でないコンテキストと見なされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="fcd7c-118">フィールド、メソッド、プロパティ、イベント、インデクサー、演算子、インスタンスコンストラクター、デストラクター、または静的コンストラクターの宣言には @no__t 0 修飾子を含めることができます。この場合、そのメンバー宣言のテキスト範囲全体が unsafe コンテキストと見なされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="fcd7c-119">*Unsafe_statement*を使用すると、*ブロック*内で unsafe コンテキストを使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="fcd7c-120">関連付けられた*ブロック*のテキスト範囲全体は、unsafe コンテキストと見なされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="fcd7c-121">関連付けられている文法の生産は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-121">The associated grammar productions are shown below.</span></span>

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

<span data-ttu-id="fcd7c-122">この例では、</span><span class="sxs-lookup"><span data-stu-id="fcd7c-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="fcd7c-123">構造体の宣言で指定された @no__t 0 修飾子は、構造体宣言のテキスト範囲全体が unsafe コンテキストになります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="fcd7c-124">したがって、`Left` および `Right` フィールドをポインター型として宣言できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="fcd7c-125">上記の例は、次のように記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="fcd7c-126">ここでは、フィールド宣言の @no__t 0 修飾子を使用すると、これらの宣言が安全でないコンテキストと見なされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="fcd7c-127">Unsafe コンテキストを確立する以外に、ポインター型の使用を許可する以外に、@no__t 0 修飾子は型またはメンバーに影響を与えません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="fcd7c-128">この例では、</span><span class="sxs-lookup"><span data-stu-id="fcd7c-128">In the example</span></span>

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

<span data-ttu-id="fcd7c-129">`A` の `F` メソッドの @no__t 0 修飾子を使用すると、単に `F` のテキスト範囲が安全でないコンテキストになり、その言語の安全でない機能が使用される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="fcd7c-130">@No__t-1 の `F` のオーバーライドでは、`unsafe` 修飾子を再指定する必要はありません。ただし、もちろん、@no__t 内の @no__t 3 のメソッドでは、安全でない機能へのアクセスが必要になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="fcd7c-131">ポインター型がメソッドのシグネチャの一部である場合、状況は若干異なります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

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

<span data-ttu-id="fcd7c-132">ここでは、`F` のシグネチャにポインター型が含まれているため、unsafe コンテキストでのみ書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="fcd7c-133">ただし、unsafe コンテキストを導入するには、@no__t 0 の場合と同様に、クラス全体を安全ではないようにするか、`B` の場合と同様にメソッド宣言に `unsafe` 修飾子を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="fcd7c-134">ポインター型</span><span class="sxs-lookup"><span data-stu-id="fcd7c-134">Pointer types</span></span>

<span data-ttu-id="fcd7c-135">Unsafe コンテキストでは、*型*([型](types.md)) は*pointer_type*だけでなく、 *value_type*または*reference_type*でもかまいません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="fcd7c-136">ただし、unsafe コンテキストの外部で `typeof` 式 ([匿名オブジェクト作成式](expressions.md#anonymous-object-creation-expressions)) に*pointer_type*を使用することもできます。このような使用方法は安全ではありません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="fcd7c-137">*Pointer_type*は、 *unmanaged_type*またはキーワード `void`、その後に `*` トークンとして書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="fcd7c-138">ポインター型の `*` の前に指定された型は、ポインター型の指示***型***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="fcd7c-139">ポインター型の値が指す変数の型を表します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="fcd7c-140">参照 (参照型の値) とは異なり、ポインターはガベージコレクターによって追跡されません。ガベージコレクターは、ポインターと、ポインターが指し示すデータを認識しません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="fcd7c-141">このため、ポインターは参照または参照を含む構造体へのポインターを指すことができず、ポインターの参照型は*unmanaged_type*である必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="fcd7c-142">*Unmanaged_type*は、 *reference_type*または構築された型ではない任意の型であり、 *reference_type*または構築された型のフィールドは入れ子のレベルには含まれません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="fcd7c-143">言い換えると、 *unmanaged_type*は次のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="fcd7c-144">`sbyte`、`byte`、`short`、`ushort`、@no__t 4、`uint`、`long`、`ulong`、`char`、`float`、0、1、または 2。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="fcd7c-145">任意の*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="fcd7c-146">任意の*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="fcd7c-147">構築された型ではなく、 *unmanaged_type*s のフィールドのみを含むユーザー定義*struct_type* 。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="fcd7c-148">ポインターと参照を混在させる直感的なルールとして、参照 (オブジェクト) の referents にはポインターを含めることができますが、ポインターの referents には参照を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="fcd7c-149">次の表に、ポインター型の例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="fcd7c-150">__例__</span><span class="sxs-lookup"><span data-stu-id="fcd7c-150">__Example__</span></span> | <span data-ttu-id="fcd7c-151">__[説明]__</span><span class="sxs-lookup"><span data-stu-id="fcd7c-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="fcd7c-152">@No__t へのポインター-0</span><span class="sxs-lookup"><span data-stu-id="fcd7c-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="fcd7c-153">@No__t へのポインター-0</span><span class="sxs-lookup"><span data-stu-id="fcd7c-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="fcd7c-154">@No__t へのポインターへのポインター-0</span><span class="sxs-lookup"><span data-stu-id="fcd7c-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="fcd7c-155">@No__t へのポインターの1次元配列-0</span><span class="sxs-lookup"><span data-stu-id="fcd7c-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="fcd7c-156">不明な型へのポインター</span><span class="sxs-lookup"><span data-stu-id="fcd7c-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="fcd7c-157">特定の実装では、すべてのポインター型のサイズと表現が同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="fcd7c-158">同じ宣言C#でC++複数のポインターが宣言されている場合、C ととは異なり、`*` は基になる型だけと共に書き込まれ、各ポインター名にプレフィックスなどとして記述されることはありません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="fcd7c-159">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="fcd7c-160">型 `T*` を持つポインターの値は、型 `T` の変数のアドレスを表します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="fcd7c-161">ポインター間接演算子 `*` ([ポインター間接](unsafe-code.md#pointer-indirection)参照) を使用してこの変数にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="fcd7c-162">たとえば、型 `int*` の変数 `P` の場合、式 `*P` は @no__t に含まれるアドレスで見つかった @no__t 3 変数を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="fcd7c-163">オブジェクト参照と同様に、ポインターは @no__t 0 になることがあります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="fcd7c-164">@No__t 0 ポインターに間接演算子を適用すると、実装定義の動作になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="fcd7c-165">値が 0 @no__t のポインターは、すべて-ビット-ゼロで表されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="fcd7c-166">@No__t-0 型は、不明な型へのポインターを表します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="fcd7c-167">参照型は不明なので、`void*` 型のポインターに間接演算子を適用することはできません。また、このようなポインターに対して算術演算を実行することもできません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="fcd7c-168">ただし、`void*` 型のポインターは、他のポインター型 (およびその逆) にキャストできます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="fcd7c-169">ポインター型は、型の別のカテゴリです。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="fcd7c-170">参照型と値型とは異なり、ポインター型は `object` から継承せず、ポインター型と `object` の間の変換は存在しません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="fcd7c-171">特に、ボックス化とボックス化解除 ([ボックス化と](types.md#boxing-and-unboxing)ボックス化解除) は、ポインターではサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="fcd7c-172">ただし、異なるポインター型と、ポインター型と整数型の間で変換が許可されています。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="fcd7c-173">これについては、「[ポインター変換](unsafe-code.md#pointer-conversions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="fcd7c-174">*Pointer_type*を型引数 (構築された[型](types.md#constructed-types)) として使用することはできません。型の推定 (型の[推定](expressions.md#type-inference)) は、型引数がポインター型であると推論されたジェネリックメソッド呼び出しで失敗します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="fcd7c-175">*Pointer_type*は、volatile フィールド ([volatile フィールド](classes.md#volatile-fields)) の型として使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="fcd7c-176">ポインターは `ref` または `out` のパラメーターとして渡すことができますが、これを行うと、未定義の動作が発生する可能性があります。これは、呼び出されたメソッドが返されるときに存在しなくなったローカル変数、またはポイントに使用された固定オブジェクトを指すようにポインターを設定できるためです。は修正されていません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="fcd7c-177">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-177">For example:</span></span>

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

<span data-ttu-id="fcd7c-178">メソッドは、何らかの型の値を返すことができ、その型はポインターにすることができます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="fcd7c-179">たとえば、連続する一連の @no__t 0、そのシーケンスの要素数、およびその他の `int` 値へのポインターを指定した場合、次のメソッドは、一致が発生した場合にその値のアドレスをそのシーケンスで返します。それ以外の場合は `null` を返します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

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

<span data-ttu-id="fcd7c-180">Unsafe コンテキストでは、ポインターを操作するためにいくつかの構造体を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="fcd7c-181">@No__t-0 演算子は、ポインターの間接参照 ([ポインター間接](unsafe-code.md#pointer-indirection)参照) を実行するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="fcd7c-182">@No__t-0 演算子は、ポインター ([ポインターメンバーアクセス](unsafe-code.md#pointer-member-access)) を介して構造体のメンバーにアクセスするために使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="fcd7c-183">@No__t-0 演算子は、ポインター ([ポインター要素アクセス](unsafe-code.md#pointer-element-access)) にインデックスを設定するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="fcd7c-184">@No__t-0 演算子は、変数のアドレス ([アドレス演算子](unsafe-code.md#the-address-of-operator)) を取得するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="fcd7c-185">@No__t-0 および `--` 演算子は、ポインターのインクリメントとデクリメント ([ポインターのインクリメントとデクリメント](unsafe-code.md#pointer-increment-and-decrement)) に使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="fcd7c-186">@No__t-0 および `-` 演算子は、ポインターの算術演算 ([ポインター演算](unsafe-code.md#pointer-arithmetic)) を実行するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="fcd7c-187">@No__t-0、`!=`、`<`、`>`、`<=`、および `=>` の各演算子を使用して、ポインター ([ポインター比較](unsafe-code.md#pointer-comparison)) を比較できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="fcd7c-188">@No__t-0 演算子を使用すると、呼び出し履歴 ([固定サイズバッファー](unsafe-code.md#fixed-size-buffers)) からメモリを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="fcd7c-189">@No__t-0 ステートメントを使用して変数を一時的に修正し、そのアドレスを取得できるようにすることができます ([fixed ステートメント](unsafe-code.md#the-fixed-statement))。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="fcd7c-190">固定変数と移動可能変数</span><span class="sxs-lookup"><span data-stu-id="fcd7c-190">Fixed and moveable variables</span></span>

<span data-ttu-id="fcd7c-191">アドレス演算子 ([アドレス演算子](unsafe-code.md#the-address-of-operator)) と `fixed` ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) は、変数を2つのカテゴリに分割します。***変数***と移動可能な***変数***を修正します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="fcd7c-192">固定変数は、ガベージコレクターの操作の影響を受けないストレージの場所に存在します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="fcd7c-193">(固定変数の例としては、ローカル変数、値パラメーター、およびポインターの逆参照によって作成される変数などがあります)。一方、移動可能な変数は、ガベージコレクターによって再配置または破棄される可能性があるストレージの場所に存在します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="fcd7c-194">移動可能な変数の例としては、オブジェクト内のフィールドや配列の要素などがあります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="fcd7c-195">@No__t-0 演算子 ([アドレス演算子](unsafe-code.md#the-address-of-operator)) を使用すると、固定変数のアドレスを制限なく取得できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="fcd7c-196">ただし、移動可能な変数はガベージコレクターによって再配置または破棄される可能性があるため、移動可能な変数のアドレスは、@no__t 0 のステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) を使用してのみ取得できます。このアドレスは、`fixed` ステートメントの期間。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="fcd7c-197">正確に言うと、固定変数は次のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="fcd7c-198">変数が匿名関数によってキャプチャされていない限り、ローカル変数または値パラメーターを参照する*simple_name* ([簡易名](expressions.md#simple-names)) によって生成される変数。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="fcd7c-199">@No__t-2 の形式の*member_access* ([メンバーアクセス](expressions.md#member-access)) によって生成される変数。 `V` は、 *struct_type*の固定変数です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="fcd7c-200">@No__t-2、@no__t フォームの*pointer_member_access* ([ポインターメンバーアクセス](unsafe-code.md#pointer-member-access))、または*pointer_element_access*の形式の*pointer_indirection_expression* ([ポインターの間接](unsafe-code.md#pointer-indirection)参照) によって生成される変数。[ポインター要素へのアクセス](unsafe-code.md#pointer-element-access))`P[E]` の形式。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="fcd7c-201">その他のすべての変数は、移動可能な変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="fcd7c-202">静的フィールドは移動可能な変数として分類されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="fcd7c-203">また、パラメーターに指定された引数が固定変数の場合でも、`ref` または `out` パラメーターは移動可能な変数として分類されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="fcd7c-204">最後に、ポインターを逆参照することによって生成される変数は、常に固定変数として分類されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="fcd7c-205">ポインター変換</span><span class="sxs-lookup"><span data-stu-id="fcd7c-205">Pointer conversions</span></span>

<span data-ttu-id="fcd7c-206">Unsafe コンテキストでは、次の暗黙的なポインター変換を含むように、使用可能な暗黙の変換 ([暗黙の変換](conversions.md#implicit-conversions)) のセットが拡張されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="fcd7c-207">任意の*pointer_type*から型 `void*` になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="fcd7c-208">@No__t-0 リテラルから任意の*pointer_type*になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="fcd7c-209">また、unsafe コンテキストでは、次の明示的なポインター変換を含むように、使用可能な明示的な変換 ([明示的な変換](conversions.md#explicit-conversions)) のセットが拡張されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="fcd7c-210">任意の*pointer_type*から他の*pointer_type*に。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="fcd7c-211">@No__t-0、`byte`、`short`、`ushort`、`int`、`uint`、`long`、または `ulong` を任意の*pointer_type*にします。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="fcd7c-212">任意の*pointer_type*から `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、または `ulong`。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="fcd7c-213">最後に、unsafe コンテキストでは、標準の暗黙的な変換 ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) のセットに次のポインター変換が含まれています。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="fcd7c-214">任意の*pointer_type*から型 `void*` になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="fcd7c-215">2つのポインター型の間の変換では、実際のポインター値が変更されることはありません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="fcd7c-216">つまり、あるポインター型から別のポインター型への変換は、ポインターによって指定された基になるアドレスには影響しません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="fcd7c-217">あるポインター型が別のポインター型に変換されると、結果のポインターがポイント先の型に対して適切にアラインされていない場合、結果が逆参照されても動作は未定義になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="fcd7c-218">一般に、"適切にアラインされた" 概念は推移的であり、`A` 型へのポインターが @no__t 型へのポインターに対して正しく調整されている場合 (つまり、型 `C` へのポインターに対して適切にアラインされた場合)、型 `A` へのポインターは正しくアラインされます型 @no__t へのポインター-4。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="fcd7c-219">次の例では、ある型を持つ変数が、別の型へのポインターを介してアクセスされているとします。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="fcd7c-220">ポインター型が byte へのポインターに変換されると、結果は変数の最小のアドレス指定バイトを指します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="fcd7c-221">結果の連続したインクリメント (変数のサイズまで) によって、その変数の残りのバイトへのポインターが生成されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="fcd7c-222">たとえば、次のメソッドは、2つの8バイトを16進数値として表示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

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

<span data-ttu-id="fcd7c-223">もちろん、生成される出力は、エンディアンに依存します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="fcd7c-224">ポインターと整数の間のマッピングは、実装によって定義されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="fcd7c-225">ただし、リニアアドレス空間を使用する 32 \* および64ビットの CPU アーキテクチャでは、通常、整数型との間のポインターの変換は、整数型との間で `uint` または @no__t 値の変換と同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="fcd7c-226">ポインター配列</span><span class="sxs-lookup"><span data-stu-id="fcd7c-226">Pointer arrays</span></span>

<span data-ttu-id="fcd7c-227">Unsafe コンテキストでは、ポインターの配列を構築できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="fcd7c-228">ポインター配列では、他の配列型に適用される変換の一部のみが許可されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="fcd7c-229">任意の*array_type*から `System.Array` への暗黙的な参照変換 ([暗黙の参照](conversions.md#implicit-reference-conversions)変換) と、それが実装するインターフェイスもポインター配列に適用されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="fcd7c-230">ただし、ポインター型は `object` に変換できないため、`System.Array` またはそれが実装するインターフェイスを介して配列要素にアクセスしようとすると、実行時に例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="fcd7c-231">1次元配列型から @no__t `S[]` への暗黙的な参照変換と明示的な[参照](conversions.md#explicit-reference-conversions)変換 ([暗黙](conversions.md#implicit-reference-conversions)の参照変換) は、ポインター配列には適用されません。ポインター型は型引数として使用できないため、ポインター型から非ポインター型への変換はありません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="fcd7c-232">@No__t-1*からの明示*的な参照変換 ([明示的な参照](conversions.md#explicit-reference-conversions)変換) と、それが実装するインターフェイスは、ポインター配列に適用されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="fcd7c-233">ポインター型を型引数として使用することはできないため、`System.Collections.Generic.IList<S>` とその基本インターフェイスから1次元配列 @no__t 型への明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions)) は、ポインター配列には適用されません。ポインター型から非ポインター型への変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="fcd7c-234">これらの制限は、 [foreach ステートメント](statements.md#the-foreach-statement)で記述された配列に対する `foreach` ステートメントの展開をポインター配列に適用できないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="fcd7c-235">代わりに、フォームの foreach ステートメント</span><span class="sxs-lookup"><span data-stu-id="fcd7c-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="fcd7c-236">@no__t 0 の型がフォームの配列型である場合 `T[,,...,]`、`N` は次元の数-@no__t 1、@no__t がポインター型で、次のように入れ子になった for ループを使用して展開されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

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

<span data-ttu-id="fcd7c-237">変数 `a`、`i0`、`i1`、...、`iN` は、`x`、 *embedded_statement* 、またはプログラムのその他のソースコードからは参照できないか、アクセスできません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="fcd7c-238">埋め込み`v`ステートメントでは、変数は読み取り専用です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="fcd7c-239">@No__t-1 (要素型) から `V` への明示的な変換 ([ポインター変換](unsafe-code.md#pointer-conversions)) が行われていない場合は、エラーが生成され、それ以上の手順は実行されません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="fcd7c-240">に`x`値`null`がある場合は、実行時にがスロー`System.NullReferenceException`されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="fcd7c-241">式におけるポインター</span><span class="sxs-lookup"><span data-stu-id="fcd7c-241">Pointers in expressions</span></span>

<span data-ttu-id="fcd7c-242">Unsafe コンテキストでは、式はポインター型の結果を生成する可能性がありますが、unsafe コンテキストの外部では、式がポインター型である場合のコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="fcd7c-243">厳密に言うと、unsafe コンテキストの外部では、 *simple_name* ([simple names](expressions.md#simple-names))、 *member_access* ([メンバーアクセス](expressions.md#member-access))、 *invocation_expression* ([呼び出し式](expressions.md#invocation-expressions))*のいずれかに該当する場合、コンパイル時エラーが発生します。element_access* ([要素アクセス](expressions.md#element-access)) はポインター型です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="fcd7c-244">Unsafe コンテキストでは、 *primary_no_array_creation_expression* ([プライマリ式](expressions.md#primary-expressions)) と*unary_expression* ([単項演算子](expressions.md#unary-operators)) の各生産で、次の追加の構成体が許可されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

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

<span data-ttu-id="fcd7c-245">これらの構成要素については、次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="fcd7c-246">Unsafe 演算子の優先順位と結合規則は、文法によって暗黙的に示されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="fcd7c-247">ポインターの間接参照</span><span class="sxs-lookup"><span data-stu-id="fcd7c-247">Pointer indirection</span></span>

<span data-ttu-id="fcd7c-248">*Pointer_indirection_expression*は、アスタリスク (`*`) とそれに続く*unary_expression*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="fcd7c-249">単項 `*` 演算子はポインターの間接参照を表し、ポインターが指す変数を取得するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="fcd7c-250">@No__t-0 を評価した結果 (`P` はポインター型の式 `T*`) は `T` 型の変数です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="fcd7c-251">単項 `*` 演算子を、`void*` 型の式、またはポインター型ではない式に適用するコンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="fcd7c-252">単項 `*` 演算子を @no__t のポインターに適用した場合の効果は、実装によって定義されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="fcd7c-253">特に、この操作で `System.NullReferenceException` がスローされる保証はありません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="fcd7c-254">ポインターに無効な値が割り当てられている場合、単項 `*` 演算子の動作は定義されていません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="fcd7c-255">単項 `*` 演算子によってポインターを逆参照するための無効な値の中には、ポインターが指す型に対して不適切にアラインされたアドレス (「[ポインターの変換](unsafe-code.md#pointer-conversions)」の例を参照) と、有効期間が終了した後の変数のアドレスがあります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="fcd7c-256">明確な代入分析を目的として、`*P` という形式の式を評価することによって生成される変数は、最初に割り当てられた変数 ([最初に割り当てられた変数](variables.md#initially-assigned-variables)) と見なされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="fcd7c-257">ポインターメンバーアクセス</span><span class="sxs-lookup"><span data-stu-id="fcd7c-257">Pointer member access</span></span>

<span data-ttu-id="fcd7c-258">*Pointer_member_access*は、 *primary_expression*、その後に "`->`" トークン、*識別子*と省略可能な*type_argument_list*で構成されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="fcd7c-259">@No__t-0 の形式のポインターメンバーアクセスでは、`P` は `void*` 以外のポインター型の式である必要があります。また、`I` は、@no__t 4 ポイントがある型のアクセス可能なメンバーを示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="fcd7c-260">@No__t-0 という形式のポインターメンバーアクセスは `(*P).I` と正確に評価されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="fcd7c-261">ポインター間接演算子 (@no__t 0) の説明については、「[ポインターの間接](unsafe-code.md#pointer-indirection)参照」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="fcd7c-262">メンバーアクセス演算子 (`.`) の説明については、「[メンバーアクセス](expressions.md#member-access)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="fcd7c-263">この例では、</span><span class="sxs-lookup"><span data-stu-id="fcd7c-263">In the example</span></span>

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

<span data-ttu-id="fcd7c-264">`->` 演算子は、フィールドにアクセスし、ポインターを介して構造体のメソッドを呼び出すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="fcd7c-265">操作 `P->I` は `(*P).I` とまったく同じであるため、`Main` メソッドも同様に記述されている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

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

### <a name="pointer-element-access"></a><span data-ttu-id="fcd7c-266">ポインター要素へのアクセス</span><span class="sxs-lookup"><span data-stu-id="fcd7c-266">Pointer element access</span></span>

<span data-ttu-id="fcd7c-267">*Pointer_element_access*は、 *primary_no_array_creation_expression*の後に "`[`" と "`]`" で囲まれた式で構成されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="fcd7c-268">@No__t-0 の形式のポインター要素アクセスでは、`P` は `void*` 以外のポインター型の式である必要があります。また、`E` は、`int`、`uint`、`long`、または @no__t に暗黙的に変換できる式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="fcd7c-269">@No__t-0 という形式のポインター要素へのアクセスは、`*(P + E)` と正確に評価されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="fcd7c-270">ポインター間接演算子 (@no__t 0) の説明については、「[ポインターの間接](unsafe-code.md#pointer-indirection)参照」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="fcd7c-271">ポインター加算演算子 (@no__t 0) の説明については、「[ポインター演算](unsafe-code.md#pointer-arithmetic)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="fcd7c-272">この例では、</span><span class="sxs-lookup"><span data-stu-id="fcd7c-272">In the example</span></span>

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

<span data-ttu-id="fcd7c-273">ポインター要素アクセスは、@no__t 0 のループで文字バッファーを初期化するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="fcd7c-274">操作 `P[E]` は `*(P + E)` とまったく同じであるため、この例は同様に記述できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

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

<span data-ttu-id="fcd7c-275">ポインター要素アクセス演算子は、範囲外のエラーを確認しません。また、範囲外の要素にアクセスする場合の動作は定義されていません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="fcd7c-276">これは、C およびC++と同じです。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="fcd7c-277">アドレス演算子</span><span class="sxs-lookup"><span data-stu-id="fcd7c-277">The address-of operator</span></span>

<span data-ttu-id="fcd7c-278">*Addressof_expression*は、アンパサンド (`&`) の後に*unary_expression*を付けたもので構成されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="fcd7c-279">式 `T` の @no__t 型であり、固定変数 (固定変数と移動可能な[変数](unsafe-code.md#fixed-and-moveable-variables)) として分類されている場合は、`&E` が指定された変数のアドレスが `E` によって計算されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="fcd7c-280">結果の型は `T*` で、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="fcd7c-281">@No__t-1 が読み取り専用のローカル変数として分類されている場合、または @no__t が移動可能な変数を表している場合、`E` が変数として分類されていないと、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="fcd7c-282">最後の例では、fixed ステートメント ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) を使用して、そのアドレスを取得する前に変数を一時的に "修正" できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="fcd7c-283">「[メンバーアクセス](expressions.md#member-access)」に示されているように、`readonly` フィールドを定義する構造体またはクラスのインスタンスコンストラクターまたは静的コンストラクターの外側では、そのフィールドは変数ではなく値と見なされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="fcd7c-284">そのため、そのアドレスを取得することはできません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="fcd7c-285">同様に、定数のアドレスを取得することはできません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="fcd7c-286">@No__t-0 演算子では、引数が確実に代入される必要はありませんが、`&` の操作の後に、演算子が適用される変数は、操作が行われる実行パスで確実に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="fcd7c-287">プログラマは、変数の正しい初期化がこの状況で実際に行われるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="fcd7c-288">この例では、</span><span class="sxs-lookup"><span data-stu-id="fcd7c-288">In the example</span></span>

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

<span data-ttu-id="fcd7c-289">`i` は、`p` の初期化に使用される `&i` の操作に従って確実に割り当てられていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="fcd7c-290">@No__t-0 への割り当ては、`i` を初期化しますが、この初期化はプログラマが行う必要があります。割り当てが削除された場合、コンパイル時のエラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="fcd7c-291">@No__t-0 演算子の明確な割り当て規則は、ローカル変数の冗長な初期化を回避できるようにするために存在します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="fcd7c-292">たとえば、多くの外部 Api は、API によって入力された構造体へのポインターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="fcd7c-293">このような Api の呼び出しは、通常、ローカルの構造体変数のアドレスを渡し、規則がないと、構造体変数の冗長な初期化が必要になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="fcd7c-294">ポインターのインクリメントとデクリメント</span><span class="sxs-lookup"><span data-stu-id="fcd7c-294">Pointer increment and decrement</span></span>

<span data-ttu-id="fcd7c-295">Unsafe コンテキストでは、`++` および `--` 演算子 ([後置インクリメントおよびデクリメント](expressions.md#postfix-increment-and-decrement-operators)演算子、[前置インクリメントおよびデクリメント演算子](expressions.md#prefix-increment-and-decrement-operators)) を、`void*` を除くすべての型のポインター変数に適用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="fcd7c-296">したがって、`T*` のすべてのポインター型に対して、次の演算子が暗黙的に定義されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="fcd7c-297">これらの演算子は、`x + 1` および `x - 1` と同じ結果を生成します ([ポインター演算](unsafe-code.md#pointer-arithmetic))。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="fcd7c-298">つまり、`T*` 型のポインター変数の場合、`++` 演算子は変数に格納されているアドレスに `sizeof(T)` を加算し、`--` 演算子は変数に格納されているアドレスから `sizeof(T)` を減算します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="fcd7c-299">ポインターインクリメントまたはデクリメント操作がポインター型のドメインをオーバーフローした場合、結果は実装定義になりますが、例外は生成されません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="fcd7c-300">ポインター算術</span><span class="sxs-lookup"><span data-stu-id="fcd7c-300">Pointer arithmetic</span></span>

<span data-ttu-id="fcd7c-301">Unsafe コンテキストでは、`+` および `-` 演算子 ([加算演算子](expressions.md#addition-operator)および[減算演算子](expressions.md#subtraction-operator)) を、`void*` を除くすべてのポインター型の値に適用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="fcd7c-302">したがって、`T*` のすべてのポインター型に対して、次の演算子が暗黙的に定義されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

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

<span data-ttu-id="fcd7c-303">@No__t-1 のポインター型の 0 @no__t 式が指定されていて、`int`、`uint`、`long`、または @no__t の型の式 `N` である場合、式 `P + N` と @no__t は、`T*`0 をアドレスに追加した結果として返される型のポインター値を計算します。1 によって指定されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="fcd7c-304">同様に、式 `P - N` は、`P` によって指定されたアドレスから `N * sizeof(T)` を減算した結果 @no__t 型のポインター値を計算します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="fcd7c-305">2つの式を指定すると、@no__t 0 と `Q` のポインター型 `T*` の場合、式 `P - Q` によって指定されたアドレスの差が `P` と `Q` によって計算され、その差が `sizeof(T)` で除算されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="fcd7c-306">結果の型は常に `long` です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-306">The type of the result is always `long`.</span></span> <span data-ttu-id="fcd7c-307">実際には、`P - Q` は `((long)(P) - (long)(Q)) / sizeof(T)` として計算されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="fcd7c-308">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-308">For example:</span></span>

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

<span data-ttu-id="fcd7c-309">次のような出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-309">which produces the output:</span></span>

```console
p - q = -14
q - p = 14
```

<span data-ttu-id="fcd7c-310">ポインターの算術演算がポインター型のドメインにオーバーフローした場合、結果は実装定義の形式で切り捨てられますが、例外は生成されません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="fcd7c-311">ポインターの比較</span><span class="sxs-lookup"><span data-stu-id="fcd7c-311">Pointer comparison</span></span>

<span data-ttu-id="fcd7c-312">Unsafe コンテキストでは、@no__t 0、`!=`、`<`、`>`、`<=`、および `=>` 演算子 ([関係演算子と型検査演算子](expressions.md#relational-and-type-testing-operators)) をすべてのポインター型の値に適用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="fcd7c-313">ポインター比較演算子は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="fcd7c-314">ポインター型から @no__t 0 型への暗黙的な変換が存在するため、ポインター型のオペランドは、これらの演算子を使用して比較できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="fcd7c-315">比較演算子は、2つのオペランドによって指定されたアドレスを符号なし整数として比較します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="fcd7c-316">Sizeof 演算子</span><span class="sxs-lookup"><span data-stu-id="fcd7c-316">The sizeof operator</span></span>

<span data-ttu-id="fcd7c-317">`sizeof` は、指定された型の変数が占有しているバイト数を返します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="fcd7c-318">@No__t-0 のオペランドとして指定された型は、 *unmanaged_type* ([ポインター型](unsafe-code.md#pointer-types)) である必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="fcd7c-319">@No__t-0 演算子の結果は `int` 型の値になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="fcd7c-320">定義済みの型によっては、次の表に示すように、@no__t 0 演算子によって定数値が生成されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="fcd7c-321">__[式]__</span><span class="sxs-lookup"><span data-stu-id="fcd7c-321">__Expression__</span></span>   | <span data-ttu-id="fcd7c-322">__結果__</span><span class="sxs-lookup"><span data-stu-id="fcd7c-322">__Result__</span></span> |
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

<span data-ttu-id="fcd7c-323">それ以外のすべての型では、`sizeof` 演算子の結果は実装によって定義され、定数ではなく値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="fcd7c-324">メンバーを構造体にパックする順序は指定されていません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="fcd7c-325">配置のために、構造体の先頭に名前のない埋め込み、構造体の内部、および構造体の末尾に存在する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="fcd7c-326">埋め込みとして使用されるビットの内容は不確定です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="fcd7c-327">構造体型のオペランドに適用された場合、結果は、その型の変数に含まれるバイト数の合計 (埋め込みを含む) になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="fcd7c-328">Fixed ステートメント</span><span class="sxs-lookup"><span data-stu-id="fcd7c-328">The fixed statement</span></span>

<span data-ttu-id="fcd7c-329">Unsafe コンテキストでは、 *embedded_statement* ([ステートメント](statements.md)) の実稼働では、追加の構成体である `fixed` ステートメントを使用できます。これは、移動可能な変数を "修正" するために使用されます。これは、ステートメントの実行中は、そのアドレスが一定のままであることを示します.</span><span class="sxs-lookup"><span data-stu-id="fcd7c-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

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

<span data-ttu-id="fcd7c-330">各*fixed_pointer_declarator*は、指定された*pointer_type*のローカル変数を宣言し、対応する*fixed_pointer_initializer*によって計算されたアドレスを使用してローカル変数を初期化します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="fcd7c-331">@No__t-0 ステートメントで宣言されたローカル変数には、その変数の宣言の右側に出現するすべての*fixed_pointer_initializer*と、`fixed` ステートメントの*embedded_statement*でアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="fcd7c-332">@No__t-0 ステートメントで宣言されたローカル変数は読み取り専用と見なされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="fcd7c-333">埋め込みステートメントがこのローカル変数を変更しようとした場合、コンパイル時にエラーが発生します (代入または `++` および `--` 演算子を使用) か、`ref` または `out` パラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="fcd7c-334">*Fixed_pointer_initializer*には、次のいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="fcd7c-335">"@No__t-0" の後に、アンマネージ型の移動可能な変数 ([固定および](unsafe-code.md#fixed-and-moveable-variables)移動可能な変数) に対して、型 `T*` が指定されている場合、その後に*variable_reference* ([明確な割り当てを決定するための正確な規則](variables.md#precise-rules-for-determining-definite-assignment)) が @no__t ます。`fixed` ステートメントで指定されたポインター型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="fcd7c-336">この場合、初期化子は指定された変数のアドレスを計算します。変数は、`fixed` ステートメントの実行中、固定アドレスで保持されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="fcd7c-337">型 @no__t @no__t が指定されているアンマネージ型の要素を含む*array_type*の式は、`fixed` ステートメントで指定されたポインター型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="fcd7c-338">この場合、初期化子は配列内の最初の要素のアドレスを計算します。配列全体は、`fixed` ステートメントの実行中に固定アドレスに保持されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="fcd7c-339">配列式が null の場合、または配列の要素がゼロの場合、初期化子は0と等しいアドレスを計算します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="fcd7c-340">型 `char*` を指定した @no__t 型の式は、`fixed` ステートメントで指定されたポインター型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="fcd7c-341">この場合、初期化子は文字列内の最初の文字のアドレスを計算します。また、文字列全体は、`fixed` ステートメントの実行中に固定アドレスで保持されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="fcd7c-342">文字列式が null の場合、`fixed` ステートメントの動作は実装によって定義されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="fcd7c-343">固定サイズバッファーのメンバーの型が、`fixed` ステートメントで指定されたポインター型に暗黙的に変換できる場合、 *simple_name*または*member_access*は移動可能な変数の固定サイズバッファーメンバーを参照します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="fcd7c-344">この場合、初期化子は、固定サイズバッファー ([式の固定サイズ](unsafe-code.md#fixed-size-buffers-in-expressions)バッファー) の最初の要素へのポインターを計算します。固定サイズバッファーは、`fixed` ステートメントの実行中に固定アドレスで保持されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="fcd7c-345">*Fixed_pointer_initializer*によって計算された各アドレスに対して、`fixed` ステートメントでは、そのアドレスによって参照される変数が、`fixed` ステートメントの実行中にガベージコレクターによって再配置または破棄されることを保証します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="fcd7c-346">たとえば、 *fixed_pointer_initializer*によって計算されたアドレスがオブジェクトのフィールドまたは配列インスタンスの要素を参照している場合、`fixed` ステートメントによって、親オブジェクトのインスタンスが再配置または破棄されないことが保証されます。ステートメントの有効期間。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="fcd7c-347">@No__t-0 ステートメントで作成されたポインターがこれらのステートメントの実行後も保持されないようにするのは、プログラマの責任です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="fcd7c-348">たとえば、`fixed` ステートメントによって作成されたポインターが外部 Api に渡される場合、Api がこれらのポインターのメモリを保持しないようにするのはプログラマの責任です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="fcd7c-349">固定オブジェクトを使用すると、ヒープの断片化が発生する可能性があります (移動できないため)。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="fcd7c-350">そのため、オブジェクトは、絶対に必要な場合にのみ修正してから、できるだけ最短の時間だけ使用するようにしてください。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="fcd7c-351">例</span><span class="sxs-lookup"><span data-stu-id="fcd7c-351">The example</span></span>

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

<span data-ttu-id="fcd7c-352">`fixed` ステートメントのいくつかの使用方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="fcd7c-353">最初のステートメントは、静的フィールドのアドレスを修正して取得します。2番目のステートメントは、インスタンスフィールドのアドレスを修正して取得し、3番目のステートメントは配列要素のアドレスを修正して取得します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="fcd7c-354">どちらの場合も、変数はすべて移動可能な変数として分類されるため、通常の `&` 演算子を使用するとエラーになります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="fcd7c-355">上の例の4番目の `fixed` ステートメントでは、3番目のと同様の結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="fcd7c-356">@No__t-0 ステートメントのこの例では、`string` を使用します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-356">This example of the `fixed` statement uses `string`:</span></span>

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

<span data-ttu-id="fcd7c-357">Unsafe コンテキストでは、1次元配列の配列要素は、インデックス `0` からインデックス `Length - 1` で終わるインデックスの順に増加して格納されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="fcd7c-358">多次元配列の場合は、配列要素が格納されます。これにより、右端の次元のインデックスが最初に増加し、次に左の次元になるようになります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="fcd7c-359">配列 @no__t インスタンスに対して-1 @no__t ポインターを取得する @no__t 0 ステートメント内では、`p` から `p + a.Length - 1` までの範囲のポインター値は、配列内の要素のアドレスを表します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="fcd7c-360">同様に、`p[0]` から `p[a.Length - 1]` までの範囲の変数は、実際の配列要素を表します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="fcd7c-361">配列を格納する方法を考えると、任意の次元の配列を線形として扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="fcd7c-362">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-362">For example:</span></span>

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

<span data-ttu-id="fcd7c-363">次のような出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-363">which produces the output:</span></span>

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="fcd7c-364">この例では、</span><span class="sxs-lookup"><span data-stu-id="fcd7c-364">In the example</span></span>

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

<span data-ttu-id="fcd7c-365">配列を修正するには、@no__t 0 ステートメントを使用します。これにより、ポインターを受け取るメソッドにそのアドレスを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="fcd7c-366">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-366">In the example:</span></span>

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

<span data-ttu-id="fcd7c-367">fixed ステートメントは、そのアドレスをポインターとして使用できるように、構造体の固定サイズバッファーを修正するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="fcd7c-368">文字列インスタンスを修正することによって生成される @no__t 0 値は、常に null で終わる文字列を指します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="fcd7c-369">ポインターを取得する固定ステートメント内では、-0 @no__t 文字列インスタンス `s` の場合、@no__t ~ @no__t 2 の範囲のポインター値は文字列内の文字のアドレスを表し、ポインター値 @no__t は常に null 文字を指します (値が `'\0'`) の文字。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="fcd7c-370">固定ポインターを使用してマネージ型のオブジェクトを変更すると、未定義の動作が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="fcd7c-371">たとえば、文字列は不変であるため、プログラマは、固定文字列へのポインターで参照される文字が変更されないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="fcd7c-372">文字列の自動 null 終了は、"C スタイル" の文字列を必要とする外部 Api を呼び出すときに特に便利です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="fcd7c-373">ただし、文字列インスタンスには null 文字を含めることが許可されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="fcd7c-374">このような null 文字が存在する場合、null で終わる @no__t 0 として処理されると、文字列は切り捨てられます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="fcd7c-375">固定サイズ バッファー</span><span class="sxs-lookup"><span data-stu-id="fcd7c-375">Fixed size buffers</span></span>

<span data-ttu-id="fcd7c-376">固定サイズバッファーは、"C スタイル" のインライン配列を構造体のメンバーとして宣言するために使用されます。これは、主にアンマネージ Api とのやり取りに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="fcd7c-377">固定サイズバッファーの宣言</span><span class="sxs-lookup"><span data-stu-id="fcd7c-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="fcd7c-378">***固定サイズバッファー***は、指定された型の変数の固定長バッファーのストレージを表すメンバーです。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="fcd7c-379">固定サイズバッファーの宣言では、特定の要素の型の1つ以上の固定サイズのバッファーが導入されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="fcd7c-380">固定サイズバッファーは、構造体宣言でのみ許可され、unsafe コンテキスト ([unsafe コンテキスト](unsafe-code.md#unsafe-contexts)) でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

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

<span data-ttu-id="fcd7c-381">固定サイズのバッファー宣言には、一連の属性 ([属性](attributes.md))、@no__t 1 つの修飾子 ([修飾子](classes.md#modifiers))、4つのアクセス修飾子 ([型パラメーターと制約](classes.md#type-parameters-and-constraints)) の有効な組み合わせ、および `unsafe` 修飾子 (Unsafe) を含めることができます。[コンテキスト](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="fcd7c-382">属性と修飾子は、固定サイズのバッファー宣言によって宣言されたすべてのメンバーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="fcd7c-383">固定サイズのバッファー宣言で同じ修飾子が複数回出現する場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="fcd7c-384">固定サイズのバッファー宣言には、@no__t 0 修飾子を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="fcd7c-385">固定サイズバッファーの宣言のバッファー要素型は、宣言によって導入されるバッファーの要素の種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="fcd7c-386">バッファー要素の型は、定義済みの型 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、0、または 1 のいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="fcd7c-387">バッファー要素型の後に固定サイズバッファー宣言子のリストが続き、それぞれに新しいメンバーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="fcd7c-388">固定サイズバッファー宣言子は、メンバーに名前を付け、その後に `[` および `]` トークンで囲まれた定数式で構成されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="fcd7c-389">定数式は、固定サイズバッファー宣言子によって導入されたメンバー内の要素の数を表します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="fcd7c-390">定数式の型は `int` 型に暗黙的に変換できる必要があり、値は0でない正の整数である必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="fcd7c-391">固定サイズバッファーの要素は、メモリに順番に配置されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="fcd7c-392">複数の固定サイズバッファーを宣言する固定サイズのバッファー宣言は、同じ属性と要素の型を持つ単一の固定サイズのバッファー宣言の複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="fcd7c-393">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="fcd7c-394">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="fcd7c-395">式でのサイズバッファーの固定</span><span class="sxs-lookup"><span data-stu-id="fcd7c-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="fcd7c-396">固定サイズのバッファーメンバーのメンバー参照 ([演算子](expressions.md#operators)) は、フィールドのメンバー参照とまったく同じように処理します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="fcd7c-397">固定サイズバッファーは、 *simple_name* ([型の推論](expressions.md#type-inference)) または*member_access* ([動的なオーバーロードの解決のコンパイル時チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) を使用して、式で参照できます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="fcd7c-398">固定サイズバッファーのメンバーが単純名として参照されている場合、その効果は `this.I` という形式のメンバーアクセスと同じになります。 `I` は固定サイズバッファーのメンバーです。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="fcd7c-399">@No__t-0 の形式のメンバーアクセスでは、`E` が構造体型であり、その構造体型で @no__t のメンバー参照が固定サイズのメンバーを識別する場合、`E.I` は次のように分類されたを評価します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="fcd7c-400">式 `E.I` が unsafe コンテキストで発生しない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="fcd7c-401">@No__t-0 が値として分類されている場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="fcd7c-402">それ以外の場合、`E` が移動可能な変数 ([固定変数および](unsafe-code.md#fixed-and-moveable-variables)移動可能変数) で、式 @no__t が*fixed_pointer_initializer* ([fixed ステートメント](unsafe-code.md#the-fixed-statement)) ではない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="fcd7c-403">それ以外の場合、`E` は固定変数を参照し、式の結果は `E` の固定サイズバッファーメンバー `I` の最初の要素へのポインターになります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="fcd7c-404">結果の型は `S*` です。ここで `S` は `I` の要素の型で、は値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="fcd7c-405">固定サイズバッファーの後続の要素は、最初の要素からのポインター操作を使用してアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="fcd7c-406">配列へのアクセスとは異なり、固定サイズバッファーの要素へのアクセスは安全でない操作であり、範囲チェックされません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="fcd7c-407">次の例では、固定サイズのバッファーメンバーを持つ構造体を宣言して使用します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

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

### <a name="definite-assignment-checking"></a><span data-ttu-id="fcd7c-408">明確な代入のチェック</span><span class="sxs-lookup"><span data-stu-id="fcd7c-408">Definite assignment checking</span></span>

<span data-ttu-id="fcd7c-409">固定サイズバッファーは、明示代入のチェック ([明確な代入](variables.md#definite-assignment)) の対象にはなりません。固定サイズバッファーのメンバーは、構造体型の変数の明確な割り当てチェックを目的として無視されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="fcd7c-410">固定サイズバッファーのメンバーの最も外側に含まれる構造体変数が、静的変数、クラスインスタンスのインスタンス変数、または配列要素の場合、固定サイズバッファーの要素は自動的に既定値に初期化されます (既定値)。[値](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="fcd7c-411">それ以外の場合は、固定サイズバッファーの初期コンテンツは未定義です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="fcd7c-412">スタック割り当て</span><span class="sxs-lookup"><span data-stu-id="fcd7c-412">Stack allocation</span></span>

<span data-ttu-id="fcd7c-413">Unsafe コンテキストでは、ローカル変数宣言 ([ローカル変数宣言](statements.md#local-variable-declarations)) に、呼び出し履歴からメモリを割り当てるスタック割り当て初期化子を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="fcd7c-414">*Unmanaged_type*は、新しく割り当てられた場所に格納される項目の種類を示し、*式*はこれらの項目の数を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="fcd7c-415">これらは共に、必要な割り当てサイズを指定します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="fcd7c-416">スタック割り当てのサイズを負の値にすることはできません。そのため、項目数を*constant_expression*として指定すると、負の値に評価されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="fcd7c-417">@No__t-0 の形式のスタック割り当て初期化子は、`T` がアンマネージ型 ([ポインター型](unsafe-code.md#pointer-types)) であり、`E` が @no__t 型の式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="fcd7c-418">このコンストラクトは、呼び出し履歴から `E * sizeof(T)` バイトを割り当て、@no__t 型のポインターを新しく割り当てられたブロックに返します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="fcd7c-419">@No__t-0 が負の値の場合、動作は定義されていません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="fcd7c-420">@No__t-0 が0の場合、割り当ては行われず、返されるポインターは実装定義になります。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="fcd7c-421">指定されたサイズのブロックを割り当てるために十分なメモリがない場合は、`System.StackOverflowException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="fcd7c-422">新しく割り当てられたメモリの内容は未定義です。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="fcd7c-423">スタック割り当て初期化子は、`catch` または `finally` ブロック ([try ステートメント](statements.md#the-try-statement)) では許可されていません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="fcd7c-424">@No__t-0 を使用して割り当てられたメモリを明示的に解放する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="fcd7c-425">関数メンバーの実行中に作成されたすべてのスタック割り当てメモリブロックは、その関数メンバーがを返すと自動的に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="fcd7c-426">これは、C および実装で一般的に検出される拡張機能であるC++ @no__t 0 関数に対応しています。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="fcd7c-427">この例では、</span><span class="sxs-lookup"><span data-stu-id="fcd7c-427">In the example</span></span>

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

<span data-ttu-id="fcd7c-428">@no__t 0 初期化子は、16文字のバッファーをスタックに割り当てるために、`IntToString` メソッドで使用されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="fcd7c-429">メソッドがを返した場合、バッファーは自動的に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="fcd7c-430">動的メモリ割り当て</span><span class="sxs-lookup"><span data-stu-id="fcd7c-430">Dynamic memory allocation</span></span>

<span data-ttu-id="fcd7c-431">@No__t-0 演算子を除き、はC# 、ガベージコレクションではないメモリを管理するための定義済みの構成体を提供しません。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="fcd7c-432">通常、このようなサービスは、サポートクラスライブラリによって提供されるか、基になるオペレーティングシステムから直接インポートされます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="fcd7c-433">たとえば、次の `Memory` クラスは、基になるオペレーティングシステムのヒープ関数がからC#アクセスされる方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

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

<span data-ttu-id="fcd7c-434">@No__t-0 クラスを使用する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-434">An example that uses the `Memory` class is given below:</span></span>

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

<span data-ttu-id="fcd7c-435">この例では、`Memory.Alloc` を使用して256バイトのメモリを割り当て、0から255までの値を使用してメモリブロックを初期化します。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="fcd7c-436">次に、256要素のバイト配列を割り当て、`Memory.Copy` を使用してメモリブロックの内容をバイト配列にコピーします。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="fcd7c-437">最後に、`Memory.Free` を使用してメモリブロックが解放され、バイト配列の内容がコンソールに出力されます。</span><span class="sxs-lookup"><span data-stu-id="fcd7c-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
