# <a name="introduction"></a><span data-ttu-id="f9942-101">はじめに</span><span class="sxs-lookup"><span data-stu-id="f9942-101">Introduction</span></span>

<span data-ttu-id="f9942-102">C# ("シー シャープ" と読みます) は、シンプルで最新のタイプ セーフなオブジェクト指向のプログラミング言語です。</span><span class="sxs-lookup"><span data-stu-id="f9942-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="f9942-103">C# で C ファミリ言語のおよび C、C++、および Java のプログラマになじみがすぐになります。</span><span class="sxs-lookup"><span data-stu-id="f9942-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="f9942-104">C# として ECMA international 標準化されて、 ***ecma-334***標準および ISO/IEC としてによって、 ***ISO/IEC 23270***標準。</span><span class="sxs-lookup"><span data-stu-id="f9942-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="f9942-105">.NET Framework 用の Microsoft の c# コンパイラは、これらの標準の両方の準拠の実装です。</span><span class="sxs-lookup"><span data-stu-id="f9942-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="f9942-106">C# はオブジェクト指向言語ですが、C# にはさらに "***コンポーネント指向***" プログラミングのサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f9942-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="f9942-107">最近のソフトウェア設計では、機能を自己完結型および自己記述型パッケージの形式にしたソフトウェア コンポーネントをますます使用するようになっています。</span><span class="sxs-lookup"><span data-stu-id="f9942-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="f9942-108">そのようなコンポーネントの鍵となるのは、プロパティ、メソッド、イベントを使用してプログラミング モデルを表すこと、コンポーネントについての宣言的な情報を提供する属性があること、独自のドキュメントが組み込まれていることです。</span><span class="sxs-lookup"><span data-stu-id="f9942-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="f9942-109">C# では、これらの概念を直接サポートする言語コンストラクトを行う (C#) を作成し、ソフトウェア コンポーネントを使用するための非常に自然言語。</span><span class="sxs-lookup"><span data-stu-id="f9942-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="f9942-110">堅牢で永続的なアプリケーションの構築を支援するいくつかの c# 機能。***ガベージ コレクション***自動的に使用されていないオブジェクトによって占有されているメモリを解放します。***例外処理***エラーの検出と復旧に構造化された拡張可能なアプローチを提供し、***タイプ セーフ***言語の設計では、初期化されていない変数を読み取ったりすることは不可能インデックスには、配列の境界を超える、またはオフを実行するは、キャストを入力します。</span><span class="sxs-lookup"><span data-stu-id="f9942-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="f9942-111">C# は "***統合型システム***" を採用しています。</span><span class="sxs-lookup"><span data-stu-id="f9942-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="f9942-112">`int` や `double` などのプリミティブ型を含めた C# のすべての型は、ルートとなる 1 つの `object` 型から派生しています。</span><span class="sxs-lookup"><span data-stu-id="f9942-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="f9942-113">したがって、すべての型が一般的な操作のセットを共有し、すべての型の値を一貫した方法で格納、転送、操作することができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="f9942-114">さらに、C# はユーザー定義の参照型と値型の両方をサポートしているため、オブジェクトを動的に割り当てることも、軽量の構造体をインラインで格納することもできます。</span><span class="sxs-lookup"><span data-stu-id="f9942-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="f9942-115">C# のプログラムとライブラリを互換性のある方法での時間とともに進化できることを確認するに大きな重点が置かれた***バージョン管理***# の設計にします。</span><span class="sxs-lookup"><span data-stu-id="f9942-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="f9942-116">多くのプログラミング言語は、この問題にほとんど注意を払っていません。その結果、それらの言語で書かれたプログラムは、依存するライブラリの新しいバージョンが導入されたときに必要以上に頻繁に中断されてしまいます。</span><span class="sxs-lookup"><span data-stu-id="f9942-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="f9942-117">バージョン管理に関する考慮事項の影響を受ける直接 # の設計の側面は、個別の`virtual`と`override`修飾子、メソッドのオーバー ロードの解決の規則と明示的なインターフェイス メンバー宣言をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f9942-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="f9942-118">この章の残りの部分では、c# 言語の重要な機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="f9942-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="f9942-119">以降の章では、ルールと例外を説明について詳細に、場合によっては数学的に、この章はわかりやすくするため、完全性を犠牲に簡潔にするための努力しています。</span><span class="sxs-lookup"><span data-stu-id="f9942-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="f9942-120">目的は、初期プログラムの作成と以降の章の読み取りを容易にする言語を紹介したリーダーを提供します。</span><span class="sxs-lookup"><span data-stu-id="f9942-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="f9942-121">Hello world</span><span class="sxs-lookup"><span data-stu-id="f9942-121">Hello world</span></span>

<span data-ttu-id="f9942-122">"Hello, World" は、プログラミング言語を紹介するために伝統的に使用されているプログラムです。</span><span class="sxs-lookup"><span data-stu-id="f9942-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="f9942-123">これを C# で記述すると次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f9942-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="f9942-124">通常、C# のソース ファイルのファイル拡張子は `.cs` です。</span><span class="sxs-lookup"><span data-stu-id="f9942-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="f9942-125">「こんにちは, World」プログラムがファイルに格納されていると仮定`hello.cs`プログラムは、コマンドラインを使用して、Microsoft c# コンパイラでコンパイルすることができます</span><span class="sxs-lookup"><span data-stu-id="f9942-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="f9942-126">という名前の実行可能アセンブリを生成する`hello.exe`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="f9942-127">このアプリケーションは、実行時に生成される出力は、します。</span><span class="sxs-lookup"><span data-stu-id="f9942-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="f9942-128">"Hello, World" プログラムは `System` 名前空間を参照する `using` ディレクティブで始まります。</span><span class="sxs-lookup"><span data-stu-id="f9942-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="f9942-129">名前空間は、C# のプログラムとライブラリを階層的に整理するための手段です。</span><span class="sxs-lookup"><span data-stu-id="f9942-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="f9942-130">名前空間には、型と他の名前空間が含まれます。たとえば、`System` 名前空間には多数の型 (プログラムで参照される `Console` クラスなど) と、他の多数の名前空間 (`IO` や `Collections` など) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f9942-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="f9942-131">特定の名前空間を参照する `using` ディレクティブを使用すると、その名前空間のメンバーである型を修飾せずに使用できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="f9942-132">`using` ディレクティブにより、プログラムで `Console.WriteLine` を `System.Console.WriteLine` の省略形として使用できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="f9942-133">"Hello, World" プログラムで宣言された `Hello` クラスにはメンバーが 1 つあります。`Main` という名前のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="f9942-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="f9942-134">`Main`でメソッドが宣言された、`static`修飾子。</span><span class="sxs-lookup"><span data-stu-id="f9942-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="f9942-135">インスタンス メソッドが `this` で囲んだ特定のオブジェクト インスタンスを参照できるのに対し、静的メソッドは特定のオブジェクトを参照せずに機能します。</span><span class="sxs-lookup"><span data-stu-id="f9942-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="f9942-136">規則により、`Main` という名前の静的メソッドはプログラムのエントリ ポイントとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="f9942-137">プログラムの出力は、`System` 名前空間にある `Console` クラスの `WriteLine` メソッドによって生成されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="f9942-138">このクラスは、既定では、Microsoft c# コンパイラでは参照自動的にされている .NET Framework クラス ライブラリによって提供されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="f9942-139">C# 自体がないこと、個別のランタイム ライブラリに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f9942-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="f9942-140">代わりに、.NET Framework は、c# のランタイム ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="f9942-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="f9942-141">プログラムの構造</span><span class="sxs-lookup"><span data-stu-id="f9942-141">Program structure</span></span>

<span data-ttu-id="f9942-142">C# における主要な組織的概念は、***プログラム***、***名前空間***、***型***、***メンバー***、および***アセンブリ***です。</span><span class="sxs-lookup"><span data-stu-id="f9942-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="f9942-143">C# プログラムは、1 つまたは複数のソース ファイルで構成されています。</span><span class="sxs-lookup"><span data-stu-id="f9942-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="f9942-144">プログラムは型を宣言します。型にはメンバーが含まれていて、複数の名前空間に編成することができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="f9942-145">型の例には、クラスおよびインターフェイスがあります。</span><span class="sxs-lookup"><span data-stu-id="f9942-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="f9942-146">メンバーの例には、フィールド、メソッド、プロパティ、およびイベントがあります。</span><span class="sxs-lookup"><span data-stu-id="f9942-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="f9942-147">C# プログラムはコンパイルされると、物理的にアセンブリにパッケージ化されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="f9942-148">アセンブリは通常、ファイル拡張子を持つ`.exe`または`.dll`を実装するかに応じて、***アプリケーション***または***ライブラリ***します。</span><span class="sxs-lookup"><span data-stu-id="f9942-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="f9942-149">例では、</span><span class="sxs-lookup"><span data-stu-id="f9942-149">The example</span></span>

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
<span data-ttu-id="f9942-150">という名前のクラスを宣言します`Stack`という名前空間で`Acme.Collections`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="f9942-151">このクラスの完全修飾名は `Acme.Collections.Stack` です。</span><span class="sxs-lookup"><span data-stu-id="f9942-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="f9942-152">このクラスには複数のメンバーが含まれています: `top` という名前のフィールドが 1 つ、`Push` と `Pop` という名前のメソッドが合わせて 2 つ、そして `Entry` という名前の入れ子になったクラスです。</span><span class="sxs-lookup"><span data-stu-id="f9942-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="f9942-153">`Entry` クラスにはさらに、3 つのメンバーが含まれています: `next` という名前のフィールド、`data` という名前のフィールド、およびコンストラクターです。</span><span class="sxs-lookup"><span data-stu-id="f9942-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="f9942-154">例のソース コードが `acme.cs` のファイルに保存されていることを前提に、次のコマンド ラインをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f9942-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="f9942-155">このコマンド ラインは例をライブラリ (`Main` エントリ ポイントがないコード) としてコンパイルし、`acme.dll` という名前のアセンブリを生成します。</span><span class="sxs-lookup"><span data-stu-id="f9942-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="f9942-156">アセンブリの形式で実行可能コードを含めることが***中間言語***(IL) の手順、およびシンボル情報の形式で***メタデータ***します。</span><span class="sxs-lookup"><span data-stu-id="f9942-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="f9942-157">アセンブリの IL コードは実行前に、.NET 共通言語ランタイムの Just-In-Time (JIT) コンパイラによって、プロセッサ固有のコードに自動的に変換されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="f9942-158">アセンブリはコードとメタデータの両方を含む自己記述的な機能的単位であるため、`#include` ディレクティブおよびヘッダー ファイルが C# に含まれている必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f9942-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="f9942-159">特定のアセンブリに含まれているパブリックの型とメンバーは、単にプログラムのコンパイル中にそのアセンブリを参照することにより、C# プログラムで利用可能になります。</span><span class="sxs-lookup"><span data-stu-id="f9942-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="f9942-160">たとえば、このプログラムでは `acme.dll` アセンブリの `Acme.Collections.Stack` クラスを使用しています。</span><span class="sxs-lookup"><span data-stu-id="f9942-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
<span data-ttu-id="f9942-161">プログラムがファイルに格納されている場合`test.cs`ときに、`test.cs`がコンパイルされる、`acme.dll`コンパイラを使用してアセンブリを参照できる`/r`オプション。</span><span class="sxs-lookup"><span data-stu-id="f9942-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="f9942-162">これにより `test.exe` という名前の実行可能なアセンブリが作成され、これが実行された場合に、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="f9942-163">C# では、プログラムのソース テキストを複数のソース ファイルに保存することができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="f9942-164">複数ファイルの C# プログラムがコンパイルされると、ソース ファイルはすべて同時に処理され、ソース ファイルは自由に相互を参照できるようになります。概念的には、ソース ファイルが処理される前に、すべて 1 つの大きいファイルに連結されるようなものです。</span><span class="sxs-lookup"><span data-stu-id="f9942-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="f9942-165">C# では事前宣言をする必要がありません。ごく一部の例外を除いて、宣言の順序は重要でないためです。</span><span class="sxs-lookup"><span data-stu-id="f9942-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="f9942-166">C# ではソース ファイルがパブリック型 1 つのみの宣言に制限されません。また、ソース ファイルの名前がソース ファイルで宣言された型に一致する必要もありません。</span><span class="sxs-lookup"><span data-stu-id="f9942-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="f9942-167">型と変数</span><span class="sxs-lookup"><span data-stu-id="f9942-167">Types and variables</span></span>

<span data-ttu-id="f9942-168">C# には、***値型***と***参照型***という 2 種類の型があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="f9942-169">値型の変数が直接データを格納するのに対して、参照型の変数はデータへの参照を格納し、後者はオブジェクトとして知られています。</span><span class="sxs-lookup"><span data-stu-id="f9942-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="f9942-170">参照型を使用すると 2 つの変数が同じオブジェクトを参照できるため、1 つの変数に対する演算によって、もう一方の変数によって参照されるオブジェクトに影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="f9942-171">値型の場合、各変数が独自のデータ コピーを保持し、1 つの変数に対する演算で別の変数に影響を与えることはできません (`ref` と `out` のパラメーターの変数の場合を除く)。</span><span class="sxs-lookup"><span data-stu-id="f9942-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="f9942-172"># の値の型はさらに分割***単純型***、***列挙型***、***構造体型***、および***null 許容型***と # のリファレンス型はさらに分割***クラス型***、***インターフェイス型***、***配列型***、および***デリゲート型***します。</span><span class="sxs-lookup"><span data-stu-id="f9942-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="f9942-173">次の表では、# の型システムの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-173">The following table provides an overview of C#'s type system.</span></span>

| <span data-ttu-id="f9942-174">__カテゴリ__</span><span class="sxs-lookup"><span data-stu-id="f9942-174">__Category__</span></span>    |                 | <span data-ttu-id="f9942-175">__説明__</span><span class="sxs-lookup"><span data-stu-id="f9942-175">__Description__</span></span> |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="f9942-176">値型</span><span class="sxs-lookup"><span data-stu-id="f9942-176">Value types</span></span>     | <span data-ttu-id="f9942-177">単純型</span><span class="sxs-lookup"><span data-stu-id="f9942-177">Simple types</span></span>    | <span data-ttu-id="f9942-178">符号付きの整数: `sbyte`、`short`、`int`、`long`</span><span class="sxs-lookup"><span data-stu-id="f9942-178">Signed integral: `sbyte`, `short`, `int`, `long`</span></span> |
|                 |                 | <span data-ttu-id="f9942-179">符号なしの整数: `byte`、`ushort`、`uint`、`ulong`</span><span class="sxs-lookup"><span data-stu-id="f9942-179">Unsigned integral: `byte`, `ushort`, `uint`, `ulong`</span></span> |
|                 |                 | <span data-ttu-id="f9942-180">Unicode 文字: `char`</span><span class="sxs-lookup"><span data-stu-id="f9942-180">Unicode characters: `char`</span></span> |
|                 |                 | <span data-ttu-id="f9942-181">IEEE 浮動小数点: `float`、`double`</span><span class="sxs-lookup"><span data-stu-id="f9942-181">IEEE floating point: `float`, `double`</span></span> |
|                 |                 | <span data-ttu-id="f9942-182">高精度の 10 進数: `decimal`</span><span class="sxs-lookup"><span data-stu-id="f9942-182">High-precision decimal: `decimal`</span></span> |
|                 |                 | <span data-ttu-id="f9942-183">ブール値: `bool`</span><span class="sxs-lookup"><span data-stu-id="f9942-183">Boolean: `bool`</span></span> |
|                 | <span data-ttu-id="f9942-184">列挙型</span><span class="sxs-lookup"><span data-stu-id="f9942-184">Enum types</span></span>      | <span data-ttu-id="f9942-185">`enum E {...}` 形式のユーザー定義型</span><span class="sxs-lookup"><span data-stu-id="f9942-185">User-defined types of the form `enum E {...}`</span></span> |
|                 | <span data-ttu-id="f9942-186">構造体の型</span><span class="sxs-lookup"><span data-stu-id="f9942-186">Struct types</span></span>    | <span data-ttu-id="f9942-187">`struct S {...}` 形式のユーザー定義型</span><span class="sxs-lookup"><span data-stu-id="f9942-187">User-defined types of the form `struct S {...}`</span></span> |
|                 | <span data-ttu-id="f9942-188">Null 許容型</span><span class="sxs-lookup"><span data-stu-id="f9942-188">Nullable types</span></span>  | <span data-ttu-id="f9942-189">`null` 値を持つその他すべての値型の拡張子</span><span class="sxs-lookup"><span data-stu-id="f9942-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="f9942-190">参照型</span><span class="sxs-lookup"><span data-stu-id="f9942-190">Reference types</span></span> | <span data-ttu-id="f9942-191">クラス型</span><span class="sxs-lookup"><span data-stu-id="f9942-191">Class types</span></span>     | <span data-ttu-id="f9942-192">その他すべての型の最終的な基底クラス: `object`</span><span class="sxs-lookup"><span data-stu-id="f9942-192">Ultimate base class of all other types: `object`</span></span> |
|                 |                 | <span data-ttu-id="f9942-193">Unicode 文字列: `string`</span><span class="sxs-lookup"><span data-stu-id="f9942-193">Unicode strings: `string`</span></span> |
|                 |                 | <span data-ttu-id="f9942-194">`class C {...}` 形式のユーザー定義型</span><span class="sxs-lookup"><span data-stu-id="f9942-194">User-defined types of the form `class C {...}`</span></span> |
|                 | <span data-ttu-id="f9942-195">インターフェイス型</span><span class="sxs-lookup"><span data-stu-id="f9942-195">Interface types</span></span> | <span data-ttu-id="f9942-196">`interface I {...}` 形式のユーザー定義型</span><span class="sxs-lookup"><span data-stu-id="f9942-196">User-defined types of the form `interface I {...}`</span></span> |
|                 | <span data-ttu-id="f9942-197">配列型</span><span class="sxs-lookup"><span data-stu-id="f9942-197">Array types</span></span>     | <span data-ttu-id="f9942-198">1 次元または多次元、たとえば `int[]` および `int[,]`</span><span class="sxs-lookup"><span data-stu-id="f9942-198">Single- and multi-dimensional, for example, `int[]` and `int[,]`</span></span> |
|                 | <span data-ttu-id="f9942-199">デリゲート型</span><span class="sxs-lookup"><span data-stu-id="f9942-199">Delegate types</span></span>  | <span data-ttu-id="f9942-200">ユーザー定義型形式の例。 `delegate int  D(...)`</span><span class="sxs-lookup"><span data-stu-id="f9942-200">User-defined types of the form e.g. `delegate int  D(...)`</span></span> |

<span data-ttu-id="f9942-201">8 つの整数型は、符号付きまたは符号なしの形式で、8 ビット、16 ビット、32 ビットおよび 64 ビットの値をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f9942-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="f9942-202">2 つの浮動小数点型、`float`と`double`、32 ビット単精度と 64 ビット倍精度の IEEE 754 形式を使用して表されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="f9942-203">`decimal` 型は 128 ビットのデータ型で、財務や通貨の計算に適しています。</span><span class="sxs-lookup"><span data-stu-id="f9942-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="f9942-204"># の`bool`型がブール値を表すため、値では、`true`または`false`。</span><span class="sxs-lookup"><span data-stu-id="f9942-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="f9942-205">C# における文字および文字列の処理では、Unicode エンコーディングを使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="f9942-206">`char` 型は UTF-16 コード単位を表し、`string` 型は一連の UTF-16 コード単位を表します。</span><span class="sxs-lookup"><span data-stu-id="f9942-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="f9942-207">次の表では、# の数値型をまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="f9942-207">The following table summarizes C#'s numeric types.</span></span>


| <span data-ttu-id="f9942-208">__カテゴリ__</span><span class="sxs-lookup"><span data-stu-id="f9942-208">__Category__</span></span>      | <span data-ttu-id="f9942-209">__Bits__</span><span class="sxs-lookup"><span data-stu-id="f9942-209">__Bits__</span></span> | <span data-ttu-id="f9942-210">__Type__</span><span class="sxs-lookup"><span data-stu-id="f9942-210">__Type__</span></span>  | <span data-ttu-id="f9942-211">__範囲/有効桁数__</span><span class="sxs-lookup"><span data-stu-id="f9942-211">__Range/Precision__</span></span> |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="f9942-212">符号付き整数</span><span class="sxs-lookup"><span data-stu-id="f9942-212">Signed integral</span></span>   | <span data-ttu-id="f9942-213">8</span><span class="sxs-lookup"><span data-stu-id="f9942-213">8</span></span>        | `sbyte`   | <span data-ttu-id="f9942-214">...-128 から 127</span><span class="sxs-lookup"><span data-stu-id="f9942-214">-128...127</span></span> |
|                   | <span data-ttu-id="f9942-215">16</span><span class="sxs-lookup"><span data-stu-id="f9942-215">16</span></span>       | `short`   | <span data-ttu-id="f9942-216">-32、768... 32、767</span><span class="sxs-lookup"><span data-stu-id="f9942-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="f9942-217">32</span><span class="sxs-lookup"><span data-stu-id="f9942-217">32</span></span>       | `int`     | <span data-ttu-id="f9942-218">-2,147,483、648... 2, 147、483, 647</span><span class="sxs-lookup"><span data-stu-id="f9942-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="f9942-219">64</span><span class="sxs-lookup"><span data-stu-id="f9942-219">64</span></span>       | `long`    | <span data-ttu-id="f9942-220">-9,223,372,036,854,775、808... 9、223、372、036、854、775、807</span><span class="sxs-lookup"><span data-stu-id="f9942-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="f9942-221">符号なしの整数</span><span class="sxs-lookup"><span data-stu-id="f9942-221">Unsigned integral</span></span> | <span data-ttu-id="f9942-222">8</span><span class="sxs-lookup"><span data-stu-id="f9942-222">8</span></span>        | `byte`    | <span data-ttu-id="f9942-223">0... 255</span><span class="sxs-lookup"><span data-stu-id="f9942-223">0...255</span></span> |
|                   | <span data-ttu-id="f9942-224">16</span><span class="sxs-lookup"><span data-stu-id="f9942-224">16</span></span>       | `ushort`  | <span data-ttu-id="f9942-225">0... 65,535</span><span class="sxs-lookup"><span data-stu-id="f9942-225">0...65,535</span></span> |
|                   | <span data-ttu-id="f9942-226">32</span><span class="sxs-lookup"><span data-stu-id="f9942-226">32</span></span>       | `uint`    | <span data-ttu-id="f9942-227">0... 4,294,967,295</span><span class="sxs-lookup"><span data-stu-id="f9942-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="f9942-228">64</span><span class="sxs-lookup"><span data-stu-id="f9942-228">64</span></span>       | `ulong`   | <span data-ttu-id="f9942-229">0... 18,446,744,073,709,551,615</span><span class="sxs-lookup"><span data-stu-id="f9942-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="f9942-230">浮動小数点数</span><span class="sxs-lookup"><span data-stu-id="f9942-230">Floating point</span></span>    | <span data-ttu-id="f9942-231">32</span><span class="sxs-lookup"><span data-stu-id="f9942-231">32</span></span>       | `float`   | <span data-ttu-id="f9942-232">1.5 × 10 ^ − 45 3.4 ~ 10 ^38、7 桁の有効桁数</span><span class="sxs-lookup"><span data-stu-id="f9942-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="f9942-233">64</span><span class="sxs-lookup"><span data-stu-id="f9942-233">64</span></span>       | `double`  | <span data-ttu-id="f9942-234">5.0 × 10 ^ − 324 1.7 × 10 ^308、15 桁の有効桁数</span><span class="sxs-lookup"><span data-stu-id="f9942-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="f9942-235">Decimal (10 進数型)</span><span class="sxs-lookup"><span data-stu-id="f9942-235">Decimal</span></span>           | <span data-ttu-id="f9942-236">128</span><span class="sxs-lookup"><span data-stu-id="f9942-236">128</span></span>      | `decimal` | <span data-ttu-id="f9942-237">1.0 × 10 ^ − 28 7.9 × 10 ^28、28 桁の有効桁数</span><span class="sxs-lookup"><span data-stu-id="f9942-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="f9942-238">C# プログラムでは***型宣言***を使用して新しい型を作成します。</span><span class="sxs-lookup"><span data-stu-id="f9942-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="f9942-239">型宣言は、新しい型の名前とメンバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="f9942-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="f9942-240"># の種類のカテゴリの 5 つはユーザー定義可能。 クラス型、構造体型、インターフェイス型、列挙型、およびデリゲート型。</span><span class="sxs-lookup"><span data-stu-id="f9942-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="f9942-241">クラス型では、データ メンバー (フィールド) および関数メンバー (メソッド、プロパティ、およびその他のユーザー) を格納するデータ構造体を定義します。</span><span class="sxs-lookup"><span data-stu-id="f9942-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="f9942-242">クラス型では、単一継承とポリモーフィズムをサポートします。このメカニズムによって派生クラスが基底クラスを拡張して特殊化できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="f9942-243">構造体の型は、データ メンバーおよび関数メンバーを持つ構造体を表す点において、クラス型に似ています。</span><span class="sxs-lookup"><span data-stu-id="f9942-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="f9942-244">ただし、クラスと異なり、構造体は値型で、ヒープ割り当ては必要ありません。</span><span class="sxs-lookup"><span data-stu-id="f9942-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="f9942-245">構造体型はユーザー指定の継承をサポートせず、すべての構造体型は暗黙的に `object` 型を継承します。</span><span class="sxs-lookup"><span data-stu-id="f9942-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="f9942-246">インターフェイス型では、パブリック関数メンバーの名前付きセットとしてコントラクトを定義します。</span><span class="sxs-lookup"><span data-stu-id="f9942-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="f9942-247">クラスまたは構造体、インターフェイスを実装するには、インターフェイスの関数メンバーの実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="f9942-248">複数の基底インターフェイスから継承でき、クラスまたは構造体には、複数のインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="f9942-249">デリゲート型では、特定のパラメーター リストおよび戻り値の型を持つメソッドへの参照を表します。</span><span class="sxs-lookup"><span data-stu-id="f9942-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="f9942-250">デリゲートを使用すると、変数に割り当ててパラメーターとして渡すことのできるエンティティとして、メソッドを処理できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="f9942-251">デリゲートはまた、他のいくつかの言語にみられる関数ポインターの概念に似ていますが、関数ポインターとは異なり、デリゲートはオブジェクト指向でタイプ セーフです。</span><span class="sxs-lookup"><span data-stu-id="f9942-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="f9942-252">クラス、構造体、インターフェイスおよびデリゲートの型パラメーターが他の種類とそれにより、すべてのジェネリックのサポート。</span><span class="sxs-lookup"><span data-stu-id="f9942-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="f9942-253">列挙型は、名前付き定数を使用して別個の型です。</span><span class="sxs-lookup"><span data-stu-id="f9942-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="f9942-254">すべての列挙型が、基になる型、8 つの整数型のいずれかを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="f9942-255">列挙型の値のセットでは、基になる型の値のセットと同じです。</span><span class="sxs-lookup"><span data-stu-id="f9942-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="f9942-256">C# は、あらゆる型の 1 次元および多次元の配列をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="f9942-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="f9942-257">上記の型とは異なり、配列型は使用前に宣言する必要がありません。</span><span class="sxs-lookup"><span data-stu-id="f9942-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="f9942-258">代わりに配列型は、角かっこで囲んだ型名を後に付けることにより構成されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="f9942-259">たとえば、`int[]`の 1 次元配列です`int`、`int[,]`の 2 次元の配列は、`int`と`int[][]`の 1 次元配列の 1 次元配列は、 `int`。</span><span class="sxs-lookup"><span data-stu-id="f9942-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="f9942-260">Null 許容型を使用する前に宣言することもはありません。</span><span class="sxs-lookup"><span data-stu-id="f9942-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="f9942-261">Null 非許容値型ごとに`T`対応する null 許容型がある`T?`、追加の値を格納する`null`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="f9942-262">たとえば、`int?`は任意の 32 ビット整数または値が保持できる型です。`null`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="f9942-263"># の型システムは、任意の型の値をオブジェクトとして処理できるように統合されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="f9942-264">C# における型はすべて、直接的または間接的に `object` クラス型から派生し、`object` はすべての型の究極の基底クラスです。</span><span class="sxs-lookup"><span data-stu-id="f9942-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="f9942-265">参照型の値は、値を単純に `object` 型としてみなすことによってオブジェクトとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="f9942-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="f9942-266">値型の値を実行することによってオブジェクトとして扱わ***ボックス化***と***ボックス化解除***操作。</span><span class="sxs-lookup"><span data-stu-id="f9942-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="f9942-267">次の例では、`int` 値は `object` 値に変換され、また `int` に戻されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
<span data-ttu-id="f9942-268">値型の値を型に変換するときに`object`、「ボックス」とも呼ばれる、オブジェクト インスタンスが、値を保持するために割り当てられているし、そのボックスに値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="f9942-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="f9942-269">逆に、ときに、`object`参照が値型にキャスト、参照先オブジェクトが、適切な値型のボックス、チェックが行われた、およびチェックが成功すると、ボックスの値はコピーします。</span><span class="sxs-lookup"><span data-stu-id="f9942-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="f9942-270"># の統一型システムを効果的に値の型"、オンデマンドで"オブジェクトになれること意味します。</span><span class="sxs-lookup"><span data-stu-id="f9942-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="f9942-271">こうした統一性があるため、`object` 型を使用する汎用的なライブラリは、参照型と値型の両方で使用できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="f9942-272">C# には、フィールド、配列要素、ローカル変数、パラメーターなどの、いくつかの種類の***変数***があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="f9942-273">変数は、記憶域の場所を表し、すべての変数ができる値を指定する型に次の表で示すように、変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="f9942-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| <span data-ttu-id="f9942-274">__変数の型__</span><span class="sxs-lookup"><span data-stu-id="f9942-274">__Type of Variable__</span></span>    | <span data-ttu-id="f9942-275">__考えられる内容__</span><span class="sxs-lookup"><span data-stu-id="f9942-275">__Possible Contents__</span></span> |
|-------------------------|-----------------------|
| <span data-ttu-id="f9942-276">null 非許容値型</span><span class="sxs-lookup"><span data-stu-id="f9942-276">Non-nullable value type</span></span> | <span data-ttu-id="f9942-277">型そのものの値</span><span class="sxs-lookup"><span data-stu-id="f9942-277">A value of that exact type</span></span> |
| <span data-ttu-id="f9942-278">null 許容値型</span><span class="sxs-lookup"><span data-stu-id="f9942-278">Nullable value type</span></span>     | <span data-ttu-id="f9942-279">Null 値または正確な型の値</span><span class="sxs-lookup"><span data-stu-id="f9942-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="f9942-280">Null 参照、任意の参照型のオブジェクトへの参照または値型のボックス化された値への参照</span><span class="sxs-lookup"><span data-stu-id="f9942-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="f9942-281">クラス型</span><span class="sxs-lookup"><span data-stu-id="f9942-281">Class type</span></span>              | <span data-ttu-id="f9942-282">そのクラス型から派生した、null 参照、そのクラス型のインスタンスへの参照またはクラスのインスタンスへの参照</span><span class="sxs-lookup"><span data-stu-id="f9942-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="f9942-283">インターフェイスの型</span><span class="sxs-lookup"><span data-stu-id="f9942-283">Interface type</span></span>          | <span data-ttu-id="f9942-284">Null 参照、そのインターフェイス型を実装するクラス型のインスタンスへの参照またはそのインターフェイス型を実装する値型のボックス化された値への参照</span><span class="sxs-lookup"><span data-stu-id="f9942-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="f9942-285">配列型</span><span class="sxs-lookup"><span data-stu-id="f9942-285">Array type</span></span>              | <span data-ttu-id="f9942-286">Null 参照、その配列型のインスタンスへの参照、または互換性のある配列型のインスタンスへの参照</span><span class="sxs-lookup"><span data-stu-id="f9942-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="f9942-287">デリゲート型</span><span class="sxs-lookup"><span data-stu-id="f9942-287">Delegate type</span></span>           | <span data-ttu-id="f9942-288">Null 参照またはそのデリゲート型のインスタンスへの参照</span><span class="sxs-lookup"><span data-stu-id="f9942-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="f9942-289">式</span><span class="sxs-lookup"><span data-stu-id="f9942-289">Expressions</span></span>

<span data-ttu-id="f9942-290">***式***は、***オペランド***と***演算子***で構成されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="f9942-291">式の演算子は、オペランドに適用する演算を表します。</span><span class="sxs-lookup"><span data-stu-id="f9942-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="f9942-292">演算子の例として、`+`、`-`、`*`、`/`、および `new` などがあります。</span><span class="sxs-lookup"><span data-stu-id="f9942-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="f9942-293">オペランドの例としては、リテラル、フィールド、ローカル変数、式などがあります。</span><span class="sxs-lookup"><span data-stu-id="f9942-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="f9942-294">複数の演算子を含む式の場合、演算子の***優先順位***によって各々の演算子が評価される順序が決定されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="f9942-295">たとえば、式 `x + y * z` の評価は `x + (y * z)` ですが、これは `*` 演算子が `+` 演算子より高い優先順位だからです。</span><span class="sxs-lookup"><span data-stu-id="f9942-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="f9942-296">ほとんどの演算子は***オーバーロード***できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="f9942-297">演算子をオーバーロードすると、ユーザー定義演算子の実装を、1 つまたは両方のオペランドがユーザー定義のクラスまたは構造体型である演算子に指定することができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="f9942-298">次の表では、# の演算子を高いものから低い方への優先順位の順序で演算子のカテゴリを示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="f9942-299">同じカテゴリの演算子は、同じ優先順位を持ちます。</span><span class="sxs-lookup"><span data-stu-id="f9942-299">Operators in the same category have equal precedence.</span></span>


| <span data-ttu-id="f9942-300">__カテゴリ__</span><span class="sxs-lookup"><span data-stu-id="f9942-300">__Category__</span></span>                     | <span data-ttu-id="f9942-301">__式__</span><span class="sxs-lookup"><span data-stu-id="f9942-301">__Expression__</span></span>    | <span data-ttu-id="f9942-302">__説明__</span><span class="sxs-lookup"><span data-stu-id="f9942-302">__Description__</span></span> |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="f9942-303">1 次式</span><span class="sxs-lookup"><span data-stu-id="f9942-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="f9942-304">メンバー アクセス。</span><span class="sxs-lookup"><span data-stu-id="f9942-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="f9942-305">メソッドおよびデリゲートの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="f9942-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="f9942-306">配列アクセスおよびインデクサー アクセス。</span><span class="sxs-lookup"><span data-stu-id="f9942-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="f9942-307">後置インクリメント。</span><span class="sxs-lookup"><span data-stu-id="f9942-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="f9942-308">後置デクリメント。</span><span class="sxs-lookup"><span data-stu-id="f9942-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="f9942-309">オブジェクトおよびデリゲートの作成。</span><span class="sxs-lookup"><span data-stu-id="f9942-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="f9942-310">初期化子によるオブジェクト作成</span><span class="sxs-lookup"><span data-stu-id="f9942-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="f9942-311">匿名オブジェクト初期化子</span><span class="sxs-lookup"><span data-stu-id="f9942-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="f9942-312">配列の作成</span><span class="sxs-lookup"><span data-stu-id="f9942-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="f9942-313">取得`System.Type`オブジェクト `T`</span><span class="sxs-lookup"><span data-stu-id="f9942-313">Obtain `System.Type` object for `T`</span></span> |
|                                  | `checked(x)`      | <span data-ttu-id="f9942-314">checked コンテキストで式を評価します。</span><span class="sxs-lookup"><span data-stu-id="f9942-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="f9942-315">unchecked コンテキストで式を評価します。</span><span class="sxs-lookup"><span data-stu-id="f9942-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="f9942-316">型の既定値を取得します。 `T`</span><span class="sxs-lookup"><span data-stu-id="f9942-316">Obtain default value of type `T`</span></span> |
|                                  | `delegate {...}`  | <span data-ttu-id="f9942-317">匿名関数 (匿名メソッド)。</span><span class="sxs-lookup"><span data-stu-id="f9942-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="f9942-318">単項</span><span class="sxs-lookup"><span data-stu-id="f9942-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="f9942-319">同一。</span><span class="sxs-lookup"><span data-stu-id="f9942-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="f9942-320">否定。</span><span class="sxs-lookup"><span data-stu-id="f9942-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="f9942-321">論理否定。</span><span class="sxs-lookup"><span data-stu-id="f9942-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="f9942-322">ビットごとの否定。</span><span class="sxs-lookup"><span data-stu-id="f9942-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="f9942-323">前置インクリメント。</span><span class="sxs-lookup"><span data-stu-id="f9942-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="f9942-324">前置デクリメント。</span><span class="sxs-lookup"><span data-stu-id="f9942-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="f9942-325">明示的に変換`x`を入力するには `T`</span><span class="sxs-lookup"><span data-stu-id="f9942-325">Explicitly convert `x` to type `T`</span></span> |
|                                  | `await x`         | <span data-ttu-id="f9942-326">非同期に待機`x`を完了するには</span><span class="sxs-lookup"><span data-stu-id="f9942-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="f9942-327">乗法</span><span class="sxs-lookup"><span data-stu-id="f9942-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="f9942-328">乗算</span><span class="sxs-lookup"><span data-stu-id="f9942-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="f9942-329">除算記号</span><span class="sxs-lookup"><span data-stu-id="f9942-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="f9942-330">剰余。</span><span class="sxs-lookup"><span data-stu-id="f9942-330">Remainder</span></span> |
| <span data-ttu-id="f9942-331">加法</span><span class="sxs-lookup"><span data-stu-id="f9942-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="f9942-332">加算、文字列の連結、デリゲートの組み合わせ。</span><span class="sxs-lookup"><span data-stu-id="f9942-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="f9942-333">減算、デリゲートの削除。</span><span class="sxs-lookup"><span data-stu-id="f9942-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="f9942-334">シフト</span><span class="sxs-lookup"><span data-stu-id="f9942-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="f9942-335">左シフト。</span><span class="sxs-lookup"><span data-stu-id="f9942-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="f9942-336">右シフト。</span><span class="sxs-lookup"><span data-stu-id="f9942-336">Shift right</span></span> |
| <span data-ttu-id="f9942-337">関係式と型検査</span><span class="sxs-lookup"><span data-stu-id="f9942-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="f9942-338">より小さい</span><span class="sxs-lookup"><span data-stu-id="f9942-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="f9942-339">次の値より大きい</span><span class="sxs-lookup"><span data-stu-id="f9942-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="f9942-340">以下</span><span class="sxs-lookup"><span data-stu-id="f9942-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="f9942-341">以上</span><span class="sxs-lookup"><span data-stu-id="f9942-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="f9942-342">返す`true`場合`x`は、 `T`、`false`それ以外の場合</span><span class="sxs-lookup"><span data-stu-id="f9942-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="f9942-343">返す`x`として型指定された`T`、または`null`場合`x`されませんが、 `T`</span><span class="sxs-lookup"><span data-stu-id="f9942-343">Return `x` typed as `T`, or `null` if `x` is not a `T`</span></span> |
| <span data-ttu-id="f9942-344">等価比較</span><span class="sxs-lookup"><span data-stu-id="f9942-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="f9942-345">等しい</span><span class="sxs-lookup"><span data-stu-id="f9942-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="f9942-346">等しくない</span><span class="sxs-lookup"><span data-stu-id="f9942-346">Not equal</span></span> |
| <span data-ttu-id="f9942-347">論理 AND</span><span class="sxs-lookup"><span data-stu-id="f9942-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="f9942-348">整数のビットごとの AND、ブール型の論理 AND</span><span class="sxs-lookup"><span data-stu-id="f9942-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="f9942-349">論理 XOR</span><span class="sxs-lookup"><span data-stu-id="f9942-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="f9942-350">整数のビットごとの XOR、ブール型の論理 XOR。</span><span class="sxs-lookup"><span data-stu-id="f9942-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="f9942-351">論理 OR</span><span class="sxs-lookup"><span data-stu-id="f9942-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="f9942-352">整数のビットごとの OR、ブール型の論理 OR。</span><span class="sxs-lookup"><span data-stu-id="f9942-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="f9942-353">条件 AND</span><span class="sxs-lookup"><span data-stu-id="f9942-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="f9942-354">評価`y`場合にのみ`x`は `true`</span><span class="sxs-lookup"><span data-stu-id="f9942-354">Evaluates `y` only if `x` is `true`</span></span> |
| <span data-ttu-id="f9942-355">条件 OR</span><span class="sxs-lookup"><span data-stu-id="f9942-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="f9942-356">評価`y`場合にのみ`x`は `false`</span><span class="sxs-lookup"><span data-stu-id="f9942-356">Evaluates `y` only if `x` is `false`</span></span> |
| <span data-ttu-id="f9942-357">Null 合体演算子</span><span class="sxs-lookup"><span data-stu-id="f9942-357">Null coalescing</span></span>                  | `X ?? y`          | <span data-ttu-id="f9942-358">評価される`y`場合`x`は`null`を`x`それ以外の場合</span><span class="sxs-lookup"><span data-stu-id="f9942-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="f9942-359">条件</span><span class="sxs-lookup"><span data-stu-id="f9942-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="f9942-360">評価`y`場合`x`は`true`、`z`場合`x`は `false`</span><span class="sxs-lookup"><span data-stu-id="f9942-360">Evaluates `y` if `x` is `true`, `z` if `x` is `false`</span></span> |
| <span data-ttu-id="f9942-361">代入または匿名関数</span><span class="sxs-lookup"><span data-stu-id="f9942-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="f9942-362">代入</span><span class="sxs-lookup"><span data-stu-id="f9942-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="f9942-363">複合代入。サポートされている演算子は、します。 `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span><span class="sxs-lookup"><span data-stu-id="f9942-363">Compound assignment; supported operators are `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span></span> |
|                                  | `(T x) => y`      | <span data-ttu-id="f9942-364">匿名関数 (ラムダ式)</span><span class="sxs-lookup"><span data-stu-id="f9942-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="f9942-365">ステートメント</span><span class="sxs-lookup"><span data-stu-id="f9942-365">Statements</span></span>

<span data-ttu-id="f9942-366">プログラムの処理は、"***ステートメント***" を使用して表されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="f9942-367">C# はさまざまな種類のステートメントをサポートしており、その多くは埋め込みステートメントとして定義されています。</span><span class="sxs-lookup"><span data-stu-id="f9942-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="f9942-368">"***ブロック***" を使用すると、1 つのステートメントしか使用できないコンテキストで複数のステートメントを記述できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="f9942-369">ブロックは、区切り記号 `{` と `}` の間に記述されたステートメントのリストから成ります。</span><span class="sxs-lookup"><span data-stu-id="f9942-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="f9942-370">"***宣言ステートメント***" は、ローカル変数および定数の宣言に使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="f9942-371">"***式ステートメント***" は、式の評価に使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="f9942-372">ステートメントとして使用できる式には、メソッドの呼び出し、オブジェクトの割り当てを使用して、`new`演算子を使用して割り当て`=`と複合代入演算子、を使用して操作をインクリメントおよびデクリメント`++`と`--`演算子と、await 式。</span><span class="sxs-lookup"><span data-stu-id="f9942-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="f9942-373">"***選択ステートメント***" は、式の値に基づいて、実行できる多数のステートメントから 1 つを選択するために使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="f9942-374">このグループには `if` および `switch` ステートメントが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f9942-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="f9942-375">***繰り返しステートメント***を繰り返し、埋め込みステートメントを実行するために使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="f9942-376">このグループには、`while`、`do`、`for`、および `foreach` ステートメントが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f9942-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="f9942-377">"***ジャンプ ステートメント***" は、制御を移すために使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="f9942-378">このグループには、`break`、`continue`、`goto`、`throw`、`return`、および `yield` ステートメントが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f9942-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="f9942-379">`try`...`catch` ステートメントはブロックの実行中に発生した例外をキャッチするために使用し、`try`...`finally` ステートメントは例外が発生したかどうかにかかわらず常に実行される終了処理コードを指定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="f9942-380">`checked`と`unchecked`ステートメントを使用して、オーバーフロー チェックを整数型の算術演算と変換のコンテキストを制御します。</span><span class="sxs-lookup"><span data-stu-id="f9942-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="f9942-381">`lock` ステートメントは、指定のオブジェクトに対する相互排他ロックを取得し、ステートメントを実行してからロックを解放するために使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="f9942-382">`using` ステートメントは、リソースを取得し、ステートメントを実行してからそのリソースを破棄するために使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="f9942-383">各種類のステートメントの例を次に示します</span><span class="sxs-lookup"><span data-stu-id="f9942-383">Below are examples of each kind of statement</span></span>

<span data-ttu-id="f9942-384">__ローカル変数の宣言__</span><span class="sxs-lookup"><span data-stu-id="f9942-384">__Local variable declarations__</span></span>

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


<span data-ttu-id="f9942-385">__ローカル定数宣言__</span><span class="sxs-lookup"><span data-stu-id="f9942-385">__Local constant declaration__</span></span>

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


<span data-ttu-id="f9942-386">__式ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-386">__Expression statement__</span></span>

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

<span data-ttu-id="f9942-387">__`if` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-387">__`if` statement__</span></span>

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


<span data-ttu-id="f9942-388">__`switch` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-388">__`switch` statement__</span></span>

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

<span data-ttu-id="f9942-389">__`while` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-389">__`while` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


<span data-ttu-id="f9942-390">__`do` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-390">__`do` statement__</span></span>

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

<span data-ttu-id="f9942-391">__`for` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-391">__`for` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="f9942-392">__`foreach` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-392">__`foreach` statement__</span></span>

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="f9942-393">__`break` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-393">__`break` statement__</span></span>

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="f9942-394">__`continue` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-394">__`continue` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="f9942-395">__`goto` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-395">__`goto` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

<span data-ttu-id="f9942-396">__`return` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-396">__`return` statement__</span></span>

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

<span data-ttu-id="f9942-397">__`yield` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-397">__`yield` statement__</span></span>

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

<span data-ttu-id="f9942-398">__`throw` `try`ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-398">__`throw` and `try` statements__</span></span>

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

<span data-ttu-id="f9942-399">__`checked` `unchecked`ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-399">__`checked` and `unchecked` statements__</span></span>

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

<span data-ttu-id="f9942-400">__`lock` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-400">__`lock` statement__</span></span>

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

<span data-ttu-id="f9942-401">__`using` ステートメント__</span><span class="sxs-lookup"><span data-stu-id="f9942-401">__`using` statement__</span></span>

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="f9942-402">クラスとオブジェクト</span><span class="sxs-lookup"><span data-stu-id="f9942-402">Classes and objects</span></span>

<span data-ttu-id="f9942-403">"***クラス***" は C# の最も基本的な型です。</span><span class="sxs-lookup"><span data-stu-id="f9942-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="f9942-404">クラスは、状態 (フィールド) とアクション (メソッドおよびその他の関数メンバー) を 1 つの単位としてまとめたデータ構造です。</span><span class="sxs-lookup"><span data-stu-id="f9942-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="f9942-405">クラスは動的に作成された "***インスタンス***" の定義を提供し、"***オブジェクト***" とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="f9942-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="f9942-406">クラスでは、"***継承***"と "***ポリモーフィズム***" をサポートします。これによって "***派生クラス***" が "***基底クラス***" を拡張して特殊化できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="f9942-407">新しいクラスはクラス宣言を使用して作成されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-407">New classes are created using class declarations.</span></span> <span data-ttu-id="f9942-408">クラス宣言は、クラスの属性と修飾子、クラスの名前、基底クラス (指定されている場合)、およびクラスによって実装されるインターフェイスを指定するヘッダーで開始します。</span><span class="sxs-lookup"><span data-stu-id="f9942-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="f9942-409">ヘッダーの後にはクラス本体が続きます。これは、区切り記号 `{` と `}` の間に記述するメンバー宣言のリストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="f9942-410">`Point` という名前の単純なクラスの宣言を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-410">The following is a declaration of a simple class named `Point`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="f9942-411">クラスのインスタンスは `new` 演算子を使用して作成されます。この演算子は新しいインスタンスのメモリを割り当て、コンストラクターを呼び出してインスタンスを初期化し、インスタンスへの参照を返します。</span><span class="sxs-lookup"><span data-stu-id="f9942-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="f9942-412">次のステートメントは、2 つ作成`Point`オブジェクトし、それらのオブジェクトへの参照を 2 つの変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="f9942-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="f9942-413">オブジェクトによって占有されているメモリは、オブジェクトの使用が不要になったときに自動的に解放されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="f9942-414">C# では、オブジェクトの割り当てを明示的に解除する必要がなく、また解除することもできません。</span><span class="sxs-lookup"><span data-stu-id="f9942-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="f9942-415">メンバー</span><span class="sxs-lookup"><span data-stu-id="f9942-415">Members</span></span>

<span data-ttu-id="f9942-416">クラスのメンバーは***静的メンバー***または***インスタンス メンバー***します。</span><span class="sxs-lookup"><span data-stu-id="f9942-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="f9942-417">静的メンバーはクラスに属しており、インスタンス メンバーはオブジェクト (クラスのインスタンス) に属しています。</span><span class="sxs-lookup"><span data-stu-id="f9942-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="f9942-418">次の表では、クラスに格納できるメンバーの種類の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| <span data-ttu-id="f9942-419">__Member__</span><span class="sxs-lookup"><span data-stu-id="f9942-419">__Member__</span></span>   | <span data-ttu-id="f9942-420">__説明__</span><span class="sxs-lookup"><span data-stu-id="f9942-420">__Description__</span></span> |
|------------  |-----------------|
| <span data-ttu-id="f9942-421">定数</span><span class="sxs-lookup"><span data-stu-id="f9942-421">Constants</span></span>    | <span data-ttu-id="f9942-422">クラスに関連付けられている定数値</span><span class="sxs-lookup"><span data-stu-id="f9942-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="f9942-423">フィールド</span><span class="sxs-lookup"><span data-stu-id="f9942-423">Fields</span></span>       | <span data-ttu-id="f9942-424">クラスの変数</span><span class="sxs-lookup"><span data-stu-id="f9942-424">Variables of the class</span></span> |
| <span data-ttu-id="f9942-425">メソッド</span><span class="sxs-lookup"><span data-stu-id="f9942-425">Methods</span></span>      | <span data-ttu-id="f9942-426">クラスによって実行可能な計算とアクション</span><span class="sxs-lookup"><span data-stu-id="f9942-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="f9942-427">プロパティ</span><span class="sxs-lookup"><span data-stu-id="f9942-427">Properties</span></span>   | <span data-ttu-id="f9942-428">クラスの名前付きプロパティの読み取りと書き込みに関連付けられているアクション</span><span class="sxs-lookup"><span data-stu-id="f9942-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="f9942-429">インデクサー</span><span class="sxs-lookup"><span data-stu-id="f9942-429">Indexers</span></span>     | <span data-ttu-id="f9942-430">配列など、クラスのインスタンスのインデックス作成に関連付けられているアクション</span><span class="sxs-lookup"><span data-stu-id="f9942-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="f9942-431">イベント</span><span class="sxs-lookup"><span data-stu-id="f9942-431">Events</span></span>       | <span data-ttu-id="f9942-432">クラスによって生成可能な通知</span><span class="sxs-lookup"><span data-stu-id="f9942-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="f9942-433">演算子</span><span class="sxs-lookup"><span data-stu-id="f9942-433">Operators</span></span>    | <span data-ttu-id="f9942-434">クラスによってサポートされている変換と式の演算子</span><span class="sxs-lookup"><span data-stu-id="f9942-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="f9942-435">コンストラクター</span><span class="sxs-lookup"><span data-stu-id="f9942-435">Constructors</span></span> | <span data-ttu-id="f9942-436">クラスのインスタンスまたはクラス自体を初期化するために必要なアクション</span><span class="sxs-lookup"><span data-stu-id="f9942-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="f9942-437">デストラクター</span><span class="sxs-lookup"><span data-stu-id="f9942-437">Destructors</span></span>  | <span data-ttu-id="f9942-438">クラスのインスタンスが完全に破棄される前に実行するアクション</span><span class="sxs-lookup"><span data-stu-id="f9942-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="f9942-439">種類</span><span class="sxs-lookup"><span data-stu-id="f9942-439">Types</span></span>        | <span data-ttu-id="f9942-440">クラスで宣言される、入れ子にされた型</span><span class="sxs-lookup"><span data-stu-id="f9942-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="f9942-441">ユーザー補助</span><span class="sxs-lookup"><span data-stu-id="f9942-441">Accessibility</span></span>

<span data-ttu-id="f9942-442">クラスの各メンバーにはアクセシビリティが関連付けられています。アクセシビリティは、メンバーへのアクセスが可能なプログラムのテキストの範囲を制御します。</span><span class="sxs-lookup"><span data-stu-id="f9942-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="f9942-443">アクセシビリティには 5 つの有効な形式があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="f9942-444">次の表に、これらのレポートをまとめます。</span><span class="sxs-lookup"><span data-stu-id="f9942-444">These are summarized in the following table.</span></span>


| <span data-ttu-id="f9942-445">__ユーザー補助__</span><span class="sxs-lookup"><span data-stu-id="f9942-445">__Accessibility__</span></span>    | <span data-ttu-id="f9942-446">__意味__</span><span class="sxs-lookup"><span data-stu-id="f9942-446">__Meaning__</span></span> |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="f9942-447">アクセスは制限されません。</span><span class="sxs-lookup"><span data-stu-id="f9942-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="f9942-448">アクセスは、このクラスまたはこのクラスから派生したクラスに制限されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="f9942-449">アクセスはこのプログラムに制限されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="f9942-450">アクセスは、このプログラムまたはこのクラスから派生したクラスに制限されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="f9942-451">アクセスはこのクラスに制限されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="f9942-452">型パラメーター</span><span class="sxs-lookup"><span data-stu-id="f9942-452">Type parameters</span></span>

<span data-ttu-id="f9942-453">クラス定義では、クラス名の後に型パラメーター名のリストを山かっこで囲むことで、型パラメーターのセットを指定できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="f9942-454">型パラメーターは、クラス宣言の本体で使用して、クラスのメンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="f9942-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="f9942-455">次の例では、`Pair` の型パラメーターは `TFirst` と `TSecond` です。</span><span class="sxs-lookup"><span data-stu-id="f9942-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="f9942-456">型パラメーターのように宣言されているクラス型には、ジェネリック クラスの型が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="f9942-457">構造体、インターフェイス、およびデリゲートの型もジェネリックです。</span><span class="sxs-lookup"><span data-stu-id="f9942-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="f9942-458">ジェネリック クラスを使用する場合は、それぞれの型パラメーターの型引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="f9942-459">提供されるような型引数を持つジェネリック型`Pair<int,string>
    `と呼ばれる構築型は、上。</span><span class="sxs-lookup"><span data-stu-id="f9942-459">A generic type with type arguments provided, like `Pair<int,string>
    ` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="f9942-460">基底クラス</span><span class="sxs-lookup"><span data-stu-id="f9942-460">Base classes</span></span>

<span data-ttu-id="f9942-461">クラス宣言では、クラス名と型パラメーターの後にコロンと基底クラスの名前を入力することで、基底クラスを指定できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="f9942-462">基底クラスの指定の省略は、`object` 型からの派生と同じです。</span><span class="sxs-lookup"><span data-stu-id="f9942-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="f9942-463">次の例では、`Point3D` の基底クラスは `Point` であり、`Point` の基底クラスは `object` です。</span><span class="sxs-lookup"><span data-stu-id="f9942-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
<span data-ttu-id="f9942-464">クラスは、その基底クラスのメンバーを継承します。</span><span class="sxs-lookup"><span data-stu-id="f9942-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="f9942-465">継承では、クラスが、インスタンスおよび静的コンス トラクター、および基底クラスのデストラクターを除く、基底クラスのすべてのメンバーには暗黙的に含まれていることを意味します。</span><span class="sxs-lookup"><span data-stu-id="f9942-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="f9942-466">派生クラスは、継承するメンバーに新しいメンバーを追加できますが、継承されたメンバーの定義を削除することはできません。</span><span class="sxs-lookup"><span data-stu-id="f9942-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="f9942-467">前述の例では、`Point3D` は、`Point` から `x` フィールドと `y` フィールドを継承します。各 `Point3D` インスタンスには、`x`、`y`、`z` の 3 つのフィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f9942-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="f9942-468">暗黙的な変換は、クラス型からその基底クラス型のいずれかに存在します。</span><span class="sxs-lookup"><span data-stu-id="f9942-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="f9942-469">そのため、クラス型の変数は、そのクラスのインスタンスまたは任意の派生クラスのインスタンスを参照できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="f9942-470">たとえば、前述のクラス宣言では、`Point` 型の変数が `Point` または `Point3D` を参照できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="f9942-471">フィールド</span><span class="sxs-lookup"><span data-stu-id="f9942-471">Fields</span></span>

<span data-ttu-id="f9942-472">フィールドとは、クラスまたはクラスのインスタンスに関連付けられている変数です。</span><span class="sxs-lookup"><span data-stu-id="f9942-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="f9942-473">宣言されたフィールド、`static`修飾子を定義、***静的フィールド***します。</span><span class="sxs-lookup"><span data-stu-id="f9942-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="f9942-474">静的フィールドは、格納場所を 1 つだけ識別します。</span><span class="sxs-lookup"><span data-stu-id="f9942-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="f9942-475">クラスのインスタンスがいくつ作成されても、静的フィールドのコピーは 1 つだけです。</span><span class="sxs-lookup"><span data-stu-id="f9942-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="f9942-476">なし、フィールドで宣言された、`static`修飾子を定義、***インスタンス フィールド***します。</span><span class="sxs-lookup"><span data-stu-id="f9942-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="f9942-477">クラスの各インスタンスには、そのクラスのすべてのインスタンス フィールドの個別のコピーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f9942-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="f9942-478">次の例では、`Color` クラスの各インスタンスに、インスタンス フィールド `r`、`g`、`b` の個別のコピーが含まれていますが、静的フィールド `Black`、`White`、`Red`、`Green`、`Blue` のコピーは 1 つだけです。</span><span class="sxs-lookup"><span data-stu-id="f9942-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
<span data-ttu-id="f9942-479">前述の例のように、`readonly` 修飾子を使用して "***読み取り専用フィールド***" を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="f9942-480">割り当て、`readonly`フィールドは、同じクラスのコンス トラクター、フィールドの宣言の一部としてのみ実行できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="f9942-481">メソッド</span><span class="sxs-lookup"><span data-stu-id="f9942-481">Methods</span></span>

<span data-ttu-id="f9942-482">"***メソッド***" は、オブジェクトまたはクラスによって実行可能な計算またはアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="f9942-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="f9942-483">"***静的メソッド***" にはクラスを通じてアクセスします。</span><span class="sxs-lookup"><span data-stu-id="f9942-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="f9942-484">"***インスタンス メソッド***" にはクラスのインスタンスを通じてアクセスします。</span><span class="sxs-lookup"><span data-stu-id="f9942-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="f9942-485">メソッドは、(場合によっては空) の一覧を持つ***パラメーター***、値またはメソッドに渡される変数の参照を表します、***型を返す***が計算され、によって返される値の型を指定します。メソッド。</span><span class="sxs-lookup"><span data-stu-id="f9942-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="f9942-486">メソッドの戻り値の型は`void`値を返さない場合。</span><span class="sxs-lookup"><span data-stu-id="f9942-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="f9942-487">型と同様に、メソッドには型パラメーターのセットを含めることができます。その場合、メソッドの呼び出し時に型引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="f9942-488">型引数は、型とは異なり、多くの場合メソッド呼び出しの引数から推論できます。型引数を明示的に指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f9942-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="f9942-489">メソッドの "***シグネチャ***" は、メソッドが宣言されているクラス内で一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="f9942-490">メソッドのシグネチャは、メソッドの名前、型パラメーターの数、およびメソッドのパラメーターの数、修飾子、型で構成されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="f9942-491">メソッドのシグネチャに戻り値の型は含まれません。</span><span class="sxs-lookup"><span data-stu-id="f9942-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="f9942-492">パラメーター</span><span class="sxs-lookup"><span data-stu-id="f9942-492">Parameters</span></span>

<span data-ttu-id="f9942-493">パラメーターは、値または変数参照をメソッドに渡すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="f9942-494">メソッドのパラメーターは、メソッドの呼び出し時に指定する "***引数***" から実際の値を取得します。</span><span class="sxs-lookup"><span data-stu-id="f9942-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="f9942-495">値パラメーター、参照パラメーター、出力パラメーター、およびパラメーター配列の 4 種類のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="f9942-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="f9942-496">"***値パラメーター***" は入力パラメーターの引き渡しに使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="f9942-497">値パラメーターは、パラメーターに渡された引数からその初期値を取得するローカル変数に相当します。</span><span class="sxs-lookup"><span data-stu-id="f9942-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="f9942-498">値パラメーターに対する変更は、パラメーターに渡された引数には影響しません。</span><span class="sxs-lookup"><span data-stu-id="f9942-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="f9942-499">値パラメーターは省略可能であり、既定値を指定すると、対応する引数を省略できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="f9942-500">"***参照パラメーター***" は入力パラメーターと出力パラメーターの引き渡しに使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="f9942-501">参照パラメーターに渡す引数は変数である必要があり、メソッドの実行中に、参照パラメーターは引数の変数と同じ格納場所を表します。</span><span class="sxs-lookup"><span data-stu-id="f9942-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="f9942-502">参照パラメーターは、`ref` 修飾子で宣言されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="f9942-503">`ref` パラメーターの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-503">The following example shows the use of `ref` parameters.</span></span>

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
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
<span data-ttu-id="f9942-504">"***出力パラメーター***" は出力パラメーターの引き渡しに使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="f9942-505">呼び出し元が提供する引数の初期値が重要でないことを除き、出力パラメーターは参照パラメーターと同様です。</span><span class="sxs-lookup"><span data-stu-id="f9942-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="f9942-506">出力パラメーターは、`out` 修飾子で宣言されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="f9942-507">`out` パラメーターの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-507">The following example shows the use of `out` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
<span data-ttu-id="f9942-508">"***パラメーター配列***" は、引数の変数の数をメソッドに渡せるようにします。</span><span class="sxs-lookup"><span data-stu-id="f9942-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="f9942-509">パラメーター配列は、`params` 修飾子で宣言されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="f9942-510">パラメーター配列として使用できるのは、メソッドの最後のパラメーターのみです。パラメーター配列の型は、1 次元配列の型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="f9942-511">`Write`と`WriteLine`のメソッド、`System.Console`クラスは、パラメーター配列の使用方法の良い例です。</span><span class="sxs-lookup"><span data-stu-id="f9942-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="f9942-512">これらのメソッドは次のように宣言されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="f9942-513">パラメーター配列を使用するメソッド内では、パラメーター配列は、配列型の通常のパラメーターとまったく同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="f9942-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="f9942-514">ただし、パラメーター配列を使用するメソッドの呼び出しでは、パラメーター配列の型の 1 つの引数またはパラメーター配列の要素型の任意の数の引数を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="f9942-515">後者の場合、配列インスタンスが自動的に作成され、指定した引数を使用して初期化されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="f9942-516">次のような例があるとします。</span><span class="sxs-lookup"><span data-stu-id="f9942-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="f9942-517">これは、次の記述と同じです。</span><span class="sxs-lookup"><span data-stu-id="f9942-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="f9942-518">メソッドの本体とローカル変数</span><span class="sxs-lookup"><span data-stu-id="f9942-518">Method body and local variables</span></span>

<span data-ttu-id="f9942-519">メソッドの本文には、メソッドが呼び出されたときに実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="f9942-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="f9942-520">メソッドの本体は、メソッドの呼び出しに固有の変数を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="f9942-521">このような変数は "***ローカル変数***" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="f9942-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="f9942-522">ローカル変数宣言は、型名、変数名、および (場合によっては) 初期値を指定します。</span><span class="sxs-lookup"><span data-stu-id="f9942-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="f9942-523">次の例では、初期値 0 を使用してローカル変数 `i` を宣言し、初期値を使用せずにローカル変数 `j` を宣言します。</span><span class="sxs-lookup"><span data-stu-id="f9942-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
<span data-ttu-id="f9942-524">C# では、ローカル変数の値を取得する前に、ローカル変数を "***明示的に割り当てる***" 必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="f9942-525">たとえば、前述の `i` の宣言に初期値が含まれていなかった場合、コンパイラは以降の `i` の使用に対するエラーを報告します。これは、プログラム内のそれらのポイントで `i` が明示的に割り当てられていないためです。</span><span class="sxs-lookup"><span data-stu-id="f9942-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="f9942-526">メソッドでは、`return` ステートメントを使用して、呼び出し元に制御を戻すことができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="f9942-527">`void` を返すメソッドの場合、`return` ステートメントは式を指定できません。</span><span class="sxs-lookup"><span data-stu-id="f9942-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="f9942-528">以外を返すメソッドで`void`、`return`ステートメントは、戻り値を計算する式を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="f9942-529">静的メソッドとインスタンス メソッド</span><span class="sxs-lookup"><span data-stu-id="f9942-529">Static and instance methods</span></span>

<span data-ttu-id="f9942-530">宣言されたメソッド、`static`修飾子は、***静的メソッド***します。</span><span class="sxs-lookup"><span data-stu-id="f9942-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="f9942-531">静的メソッドは、特定のインスタンスでは動作せず、静的メンバーにのみ直接アクセスできます。</span><span class="sxs-lookup"><span data-stu-id="f9942-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="f9942-532">なしで宣言されたメソッド、`static`修飾子は、***インスタンス メソッド***します。</span><span class="sxs-lookup"><span data-stu-id="f9942-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="f9942-533">インスタンス メソッドは、特定のインスタンスで動作し、静的メンバーとインスタンス メンバーの両方にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="f9942-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="f9942-534">インスタンス メソッドが呼び出されたインスタンスには、`this` として明示的にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="f9942-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="f9942-535">静的メソッドで `this` を参照するとエラーになります。</span><span class="sxs-lookup"><span data-stu-id="f9942-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="f9942-536">次の `Entity` クラスには、静的メンバーとインスタンス メンバーの両方があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-536">The following `Entity` class has both static and instance members.</span></span>

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
<span data-ttu-id="f9942-537">各 `Entity` インスタンスには、シリアル番号 (およびここに表示されていないその他の情報) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f9942-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="f9942-538">`Entity` コンストラクターは (インスタンス メソッドと同様に)、次に使用可能なシリアル番号を持つ新しいインスタンスを初期化します。</span><span class="sxs-lookup"><span data-stu-id="f9942-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="f9942-539">コンストラクターはインスタンス メンバーであるため、`serialNo` インスタンス フィールドと `nextSerialNo` 静的フィールドの両方にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="f9942-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="f9942-540">静的メソッドである `GetNextSerialNo` と `SetNextSerialNo` は `nextSerialNo` 静的フィールドにアクセスできますが、`serialNo` インスタンス フィールドに直接アクセスするとエラーになります。</span><span class="sxs-lookup"><span data-stu-id="f9942-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="f9942-541">次の例では、使用、`Entity`クラス。</span><span class="sxs-lookup"><span data-stu-id="f9942-541">The following example shows the use of the `Entity` class.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
<span data-ttu-id="f9942-542">静的メソッドである `SetNextSerialNo` と `GetNextSerialNo` はクラスで呼び出されますが、`GetSerialNo` インスタンス メソッドはクラスのインスタンスで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="f9942-543">仮想メソッド、オーバーライド メソッド、および抽象メソッド</span><span class="sxs-lookup"><span data-stu-id="f9942-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="f9942-544">インスタンス メソッドの宣言に `virtual` 修飾子が含まれている場合、そのメソッドは "***仮想メソッド***" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="f9942-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="f9942-545">ない場合`virtual`修飾子が存在する、メソッドはモード、***非仮想メソッド***します。</span><span class="sxs-lookup"><span data-stu-id="f9942-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="f9942-546">仮想メソッドが呼び出されると、その呼び出しが行われるインスタンスの "***実行時の型***" によって、呼び出す実際のメソッドの実装が決定します。</span><span class="sxs-lookup"><span data-stu-id="f9942-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="f9942-547">非仮想メソッドの呼び出しでは、インスタンスの "***コンパイル時の型***" が決定要因です。</span><span class="sxs-lookup"><span data-stu-id="f9942-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="f9942-548">仮想メソッドは派生クラスで "***オーバーライド***" できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="f9942-549">インスタンス メソッドの宣言が含まれています、`override`修飾子、メソッドは、同じシグネチャを持つ継承された仮想メソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="f9942-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="f9942-550">仮想メソッドの宣言には新しいメソッドが導入されていますが、オーバーライド メソッドの宣言では、そのメソッドの新しい実装を提供することで既存の継承された仮想メソッドを特殊化します。</span><span class="sxs-lookup"><span data-stu-id="f9942-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="f9942-551">***抽象***メソッドは実装のない仮想メソッドです。</span><span class="sxs-lookup"><span data-stu-id="f9942-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="f9942-552">抽象メソッドが宣言された、`abstract`修飾子も宣言されているクラスだけでは使用`abstract`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="f9942-553">抽象メソッドは、すべての非抽象派生クラスでオーバーライドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="f9942-554">次の例では、式ツリー ノードを表す抽象クラス `Expression`、および定数、変数参照、算術演算の式ツリー ノードを実装する 3 つの派生クラス `Constant`、`VariableReference`、`Operation` を宣言します </span><span class="sxs-lookup"><span data-stu-id="f9942-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="f9942-555">(これに似ていますと混同しないように式ツリーの型がで導入された[式ツリー型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="f9942-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
<span data-ttu-id="f9942-556">前述の 4 つのクラスは、算術式をモデル化するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="f9942-557">たとえば、これらのクラスのインスタンスを使用して、式 `x + 3` を次のように表すことができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="f9942-558">`Expression` インスタンスの `Evaluate` メソッドが呼び出され、指定された式を評価して `double` 値を生成します。</span><span class="sxs-lookup"><span data-stu-id="f9942-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="f9942-559">メソッドが引数として受け取り、 `Hashtable` (エントリのキー) として変数名と (エントリの値) として値を格納します。</span><span class="sxs-lookup"><span data-stu-id="f9942-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="f9942-560">`Evaluate`メソッドは、仮想抽象メソッド、つまり、実際の実装を提供することを非抽象派生クラスでオーバーライドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="f9942-561">`Evaluate` の `Constant` の実装は、格納された定数を単に返します。</span><span class="sxs-lookup"><span data-stu-id="f9942-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="f9942-562">A`VariableReference`の実装は、ハッシュ テーブル内の変数の名前と、結果の値を返します。</span><span class="sxs-lookup"><span data-stu-id="f9942-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="f9942-563">`Operation` の実装は、(`Evaluate` メソッドを再帰的に呼び出すことによって) まず左と右のオペランドを評価し、指定された算術演算を実行します。</span><span class="sxs-lookup"><span data-stu-id="f9942-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="f9942-564">次のプログラムでは、`Expression` クラスを使用して、式 `x * (y + 2)` の異なる値の `x` と `y` を評価します。</span><span class="sxs-lookup"><span data-stu-id="f9942-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a><span data-ttu-id="f9942-565">メソッドのオーバーロード</span><span class="sxs-lookup"><span data-stu-id="f9942-565">Method overloading</span></span>

<span data-ttu-id="f9942-566">メソッドの "***オーバーロード***" では、メソッドのシグネチャが一意であれば、同じクラス内の複数のメソッドに同じ名前を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="f9942-567">オーバーロードされたメソッドの呼び出しをコンパイルする場合、コンパイラは "***オーバーロードの解決***" を使用して、呼び出すメソッドを決定します。</span><span class="sxs-lookup"><span data-stu-id="f9942-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="f9942-568">オーバーロードの解決では、引数に最も一致する 1 つのメソッドが特定されます。最も一致するメソッドが見つからない場合は、エラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="f9942-569">次の例は、オーバーロードの解決が有効な場合を示しています。</span><span class="sxs-lookup"><span data-stu-id="f9942-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="f9942-570">`Main` メソッド内の各呼び出しのコメントは、実際に呼び出されるメソッドを示しています。</span><span class="sxs-lookup"><span data-stu-id="f9942-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
<span data-ttu-id="f9942-571">この例に示すように、パラメーターの厳密な型に引数を明示的にキャストするか、または型引数を明示的に指定することにより、特定のメソッドを常に選択できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="f9942-572">その他の関数メンバー</span><span class="sxs-lookup"><span data-stu-id="f9942-572">Other function members</span></span>

<span data-ttu-id="f9942-573">実行可能コードが含まれるメンバーは、クラスの "***関数メンバー***" と総称されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="f9942-574">前のセクションでは、関数メンバーの主な種類であるメソッドについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="f9942-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="f9942-575">このセクションは、c# でサポートされている関数メンバーの他の種類を説明します。 コンス トラクター、プロパティ、インデクサー、イベント、演算子、およびデストラクター。</span><span class="sxs-lookup"><span data-stu-id="f9942-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="f9942-576">次のコードと呼ばれるジェネリック クラスを示しています。 `List<T>`、拡張可能なオブジェクトの一覧を実装します。</span><span class="sxs-lookup"><span data-stu-id="f9942-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="f9942-577">このクラスには、最も一般的な種類の関数メンバーの例がいくつか含まれています。</span><span class="sxs-lookup"><span data-stu-id="f9942-577">The class contains several examples of the most common kinds of function members.</span></span>


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a><span data-ttu-id="f9942-578">コンストラクター</span><span class="sxs-lookup"><span data-stu-id="f9942-578">Constructors</span></span>

<span data-ttu-id="f9942-579">C# は、インスタンス コンストラクターと静的コンストラクターの両方をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f9942-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="f9942-580">"***インスタンス コンストラクター***" は、クラスのインスタンスを初期化するために必要なアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="f9942-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="f9942-581">"***静的コンストラクター***" は、クラスを最初に読み込むときに、そのクラス自体を初期化するために必要なアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="f9942-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="f9942-582">コンストラクターは、戻り値の型がなく、含んでいるクラスと同じ名前を持つメソッドのように宣言されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="f9942-583">コンス トラクターの宣言が含まれている場合、`static`修飾子は、静的コンス トラクターを宣言します。</span><span class="sxs-lookup"><span data-stu-id="f9942-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="f9942-584">それ以外の場合は、インスタンス コンストラクターが宣言されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="f9942-585">インスタンス コンス トラクターはオーバー ロードできます。</span><span class="sxs-lookup"><span data-stu-id="f9942-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="f9942-586">たとえば、`List<T>
` クラスは、2 つの (1 つはパラメーターなし、もう 1 つは `int` パラメーターを受け取る) インスタンス コンストラクターを宣言します。</span><span class="sxs-lookup"><span data-stu-id="f9942-586">For example, the `List<T>
` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="f9942-587">インスタンス コンストラクターは、`new` 演算子を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="f9942-588">次のステートメントは、2 個割り当てる`List<string>
`インスタンスのコンス トラクターのそれぞれを使用して、`List`クラス。</span><span class="sxs-lookup"><span data-stu-id="f9942-588">The following statements allocate two `List<string>
` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="f9942-589">他のメンバーとは異なり、インスタンス コンストラクターは継承されず、クラスには、そのクラスで実際に宣言された以外のインスタンス コンストラクターがありません。</span><span class="sxs-lookup"><span data-stu-id="f9942-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="f9942-590">クラスのインスタンス コンストラクターが指定されていない場合は、パラメーターなしの空のコンストラクターが自動的に指定されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="f9942-591">プロパティ</span><span class="sxs-lookup"><span data-stu-id="f9942-591">Properties</span></span>

<span data-ttu-id="f9942-592">"***プロパティ***" は、フィールドが自然に拡張したものです。</span><span class="sxs-lookup"><span data-stu-id="f9942-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="f9942-593">フィールドとプロパティはどちらも型が関連付けられている名前付きのメンバーであり、それらにアクセスするための構文は同じです。</span><span class="sxs-lookup"><span data-stu-id="f9942-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="f9942-594">ただし、フィールドとは異なり、プロパティは格納場所を表しません。</span><span class="sxs-lookup"><span data-stu-id="f9942-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="f9942-595">その代わりに、プロパティには、値の読み取りまたは書き込みの際に実行されるステートメントを指定する "***アクセサー***" があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="f9942-596">宣言が終了する点を除いて、フィールドのようにプロパティが宣言されている、`get`アクセサーおよび`set`区切り記号の間に記述されたアクセサー`{`と`}`セミコロンで終わるのではなく。</span><span class="sxs-lookup"><span data-stu-id="f9942-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="f9942-597">両方を持つプロパティを`get`アクセサーと`set`アクセサーが、***読み取り/書き込みプロパティ***、のみを持つプロパティを`get`アクセサーが、***読み取り専用プロパティ***、およびプロパティのみを持つ、`set`アクセサーは、***書き込み専用プロパティ***します。</span><span class="sxs-lookup"><span data-stu-id="f9942-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="f9942-598">A`get`アクセサーはプロパティの型の値を返すパラメーターなしのメソッドに相当します。</span><span class="sxs-lookup"><span data-stu-id="f9942-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="f9942-599">代入のターゲット プロパティが式で参照されている場合を除き、`get`プロパティのアクセサーが呼び出され、プロパティの値を計算します。</span><span class="sxs-lookup"><span data-stu-id="f9942-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="f9942-600">A`set`アクセサーがという名前のパラメーターを 1 つのメソッドに対応`value`と戻り値の型ではありません。</span><span class="sxs-lookup"><span data-stu-id="f9942-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="f9942-601">プロパティが参照されると、割り当ての対象として、またはのオペランドとして`++`または`--`、`set`アクセサーは新しい値を提供する引数を指定して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="f9942-602">`List<T>
`クラスは、2 つのプロパティを宣言します。`Count`と`Capacity`、これは読み取り専用と読み取り/書き込み、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="f9942-602">The `List<T>
` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="f9942-603">これらのプロパティの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="f9942-604">フィールドおよびメソッドと同様に、C# はインスタンス プロパティと静的プロパティの両方をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f9942-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="f9942-605">宣言されている静的プロパティ、`static`せずに、修飾子、およびインスタンスのプロパティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="f9942-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="f9942-606">プロパティのアクセサーは仮想にすることができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="f9942-607">プロパティの宣言に `virtual`、`abstract`、または `override` の各修飾子が含まれている場合、その宣言はプロパティのアクセサーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="f9942-608">インデクサー</span><span class="sxs-lookup"><span data-stu-id="f9942-608">Indexers</span></span>

<span data-ttu-id="f9942-609">"***インデクサー***" は、配列と同じ方法でオブジェクトのインデックスを作成できるようにするメンバーです。</span><span class="sxs-lookup"><span data-stu-id="f9942-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="f9942-610">メンバーの名前がある点が、インデクサーはプロパティのように宣言が`this`区切り記号の間に記述されたパラメーター リストが続く`[`と`]`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="f9942-611">パラメーターは、インデクサーのアクセサーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="f9942-612">プロパティと同様に、読み取り/書き込み、読み取り専用、および書き込み専用のインデクサーを使用できます。また、インデクサーのアクセサーを仮想にすることができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="f9942-613">`List` クラスは、`int` パラメーターを受け取る 1 つの読み取り/書き込みインデクサーを宣言します。</span><span class="sxs-lookup"><span data-stu-id="f9942-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="f9942-614">インデクサーを使用すると、`int` 値を持つ `List` インスタンスのインデックスを作成できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="f9942-615">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-615">For example</span></span>

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
<span data-ttu-id="f9942-616">インデクサーはオーバーロードできます。つまり、パラメーターの数または型が異なる限り、クラスは複数のインデクサーを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="f9942-617">イベント</span><span class="sxs-lookup"><span data-stu-id="f9942-617">Events</span></span>

<span data-ttu-id="f9942-618">"***イベント***" は、クラスまたはオブジェクトで通知を提供できるようにするメンバーです。</span><span class="sxs-lookup"><span data-stu-id="f9942-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="f9942-619">宣言が含まれている点を除いて、フィールドのようにイベントが宣言されている、`event`キーワードと型がデリゲート型にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="f9942-620">イベント メンバーを宣言するクラス内では、イベントはデリゲート型のフィールドと同じように動作します (イベントが抽象イベントでなく、アクセサーを宣言しない場合)。</span><span class="sxs-lookup"><span data-stu-id="f9942-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="f9942-621">フィールドは、イベントに追加されたイベント ハンドラーを表すデリゲートへの参照を格納します。</span><span class="sxs-lookup"><span data-stu-id="f9942-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="f9942-622">イベント ハンドルが存在しない場合、フィールドは`null`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="f9942-623">`List<T>
` クラスは、`Changed` という 1 つのイベント メンバーを宣言します。このメンバーは新しい項目がリストに追加されたことを示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-623">The `List<T>
` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="f9942-624">`Changed`イベントは、`OnChanged`仮想メソッドで、最初は、イベントは、かどうかを確認します。 `null` (つまり、ハンドラーが存在しないこと)。</span><span class="sxs-lookup"><span data-stu-id="f9942-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="f9942-625">イベントを発生させるという概念は、イベントによって表されるデリゲートの呼び出しとまったく同じです。したがって、イベントを発生させるための特殊な言語コンストラクトはありません。</span><span class="sxs-lookup"><span data-stu-id="f9942-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="f9942-626">クライアントは、"***イベント ハンドラー***" を使用してイベントに対応します。</span><span class="sxs-lookup"><span data-stu-id="f9942-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="f9942-627">イベント ハンドラーは、`+=` 演算子を使用してアタッチされ、`-=` 演算子を使用して削除されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="f9942-628">次の例では、`List<string>
` の `Changed` イベントにイベント ハンドラーをアタッチします。</span><span class="sxs-lookup"><span data-stu-id="f9942-628">The following example attaches an event handler to the `Changed` event of a `List<string>
`.</span></span>

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
<span data-ttu-id="f9942-629">イベントの基になる記憶域の制御が求められる高度なシナリオでは、イベントの宣言で `add` アクセサーと `remove` アクセサーを明示的に指定できます。これらは、プロパティの `set` アクセサーにある程度似ています。</span><span class="sxs-lookup"><span data-stu-id="f9942-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="f9942-630">演算子</span><span class="sxs-lookup"><span data-stu-id="f9942-630">Operators</span></span>

<span data-ttu-id="f9942-631">"***演算子***" は、クラスのインスタンスに特定の式の演算子を適用する意味を定義するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="f9942-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="f9942-632">単項演算子、2 項演算子、および変換演算子の 3 種類を定義できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="f9942-633">すべての演算子は `public` および `static` として宣言する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="f9942-634">`List<T>
` クラスは 2 つの演算子 (`operator==` と `operator!=`) を宣言し、`List` インスタンスにこれらの演算子を適用する式に新しい意味を持たせます。</span><span class="sxs-lookup"><span data-stu-id="f9942-634">The `List<T>
` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="f9942-635">具体的には、演算子が 2 つの等しいかどうかを定義`List<T>
`インスタンスを使用して含まれているオブジェクトの各比較として、`Equals`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f9942-635">Specifically, the operators define equality of two `List<T>
` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="f9942-636">次の例では、`==` 演算子を使用して 2 つの `List<int>
` インスタンスを比較します。</span><span class="sxs-lookup"><span data-stu-id="f9942-636">The following example uses the `==` operator to compare two `List<int>
` instances.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

<span data-ttu-id="f9942-637">最初の `Console.WriteLine` は `True` を出力します。これは、2 つのリストに、同じ値を持つ同じ数のオブジェクトが同じ順序で含まれているためです。</span><span class="sxs-lookup"><span data-stu-id="f9942-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="f9942-638">`List<T>
` で `operator==` が定義されていない場合は、`a` と `b` が異なる `List<int>
` インスタンスを参照するため、最初の `Console.WriteLine` は `False` を出力します。</span><span class="sxs-lookup"><span data-stu-id="f9942-638">Had `List<T>
` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>
` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="f9942-639">デストラクター</span><span class="sxs-lookup"><span data-stu-id="f9942-639">Destructors</span></span>

<span data-ttu-id="f9942-640">A***デストラクター***はクラスのインスタンスを消滅させるために必要なアクションを実装するメンバーです。</span><span class="sxs-lookup"><span data-stu-id="f9942-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="f9942-641">デストラクターは、パラメーターを含めることはできません、アクセシビリティ修飾子は使用できません、明示的に呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="f9942-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="f9942-642">インスタンスのデストラクターは、ガベージ コレクション中に自動的に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="f9942-643">ガベージ コレクターには、かなり自由に決定するオブジェクトを収集し、デストラクターの実行が許可されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="f9942-644">具体的には、デストラクターの呼び出しのタイミングは確定的でないと、デストラクターは、任意のスレッドで実行する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="f9942-645">これらとその他の理由は、他のソリューションが実現できない場合にのみ、クラスはデストラクターを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="f9942-646">`using` ステートメントは、オブジェクトを破棄するためのより適切な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="f9942-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="f9942-647">構造体</span><span class="sxs-lookup"><span data-stu-id="f9942-647">Structs</span></span>

<span data-ttu-id="f9942-648">***構造体***は、クラスと同様に、データ メンバーおよび関数メンバーを含むことができるデータ構造ですが、値型でありヒープ割り当てを必要としない点でクラスと異なります。</span><span class="sxs-lookup"><span data-stu-id="f9942-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="f9942-649">構造体型の変数は、構造体のデータを直接格納しますが、クラス型の変数は、動的に割り当てられたオブジェクトへの参照を格納します。</span><span class="sxs-lookup"><span data-stu-id="f9942-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="f9942-650">構造体型はユーザー指定の継承をサポートしていませんし、すべての構造体型が暗黙的に `object` 型を継承します。</span><span class="sxs-lookup"><span data-stu-id="f9942-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="f9942-651">構造体は、値セマンティクスを持つ小規模なデータ構造に特に便利です。</span><span class="sxs-lookup"><span data-stu-id="f9942-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="f9942-652">構造体の主な例としては、複素数、座標系のポイント、ディクショナリのキーと値のペアなどがあります。</span><span class="sxs-lookup"><span data-stu-id="f9942-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="f9942-653">小規模なデータ構造には、クラスよりむしろ構造体を使用するほうが、アプリケーションが実行するメモリ割り当ての数を大幅に減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="f9942-654">たとえば、次のプログラムでは、100 個のポイントの配列を作成し初期化します。</span><span class="sxs-lookup"><span data-stu-id="f9942-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="f9942-655">`Point` をクラスとして実装すると、101 個の別々のオブジェクトがインスタンス化されます。配列に1 個、残りは 100 個の要素に 1 個ずつです。</span><span class="sxs-lookup"><span data-stu-id="f9942-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
<span data-ttu-id="f9942-656">別の方法はさせる`Point`構造体。</span><span class="sxs-lookup"><span data-stu-id="f9942-656">An alternative is to make `Point` a struct.</span></span>

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="f9942-657">ここでは 1 つのオブジェクトのみがインスタンス化されます。すなわち、配列に 1 個です。そして、`Point` インスタンスがその配列内にインラインで格納されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="f9942-658">構造体コンストラクターは `new` 演算子を使って呼び出されますが、メモリが割り当てられていることを意味するものではありません。</span><span class="sxs-lookup"><span data-stu-id="f9942-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="f9942-659">オブジェクトを動的に割り当てそこへの参照を返す代わりに、構造体コンストラクターは単に構造体の値自体 (通常、スタック上の一時的な場所) を返し、この値は必要に応じてコピーされます。</span><span class="sxs-lookup"><span data-stu-id="f9942-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="f9942-660">クラスを使用すると、2 つの変数が同じオブジェクトを参照できるため、1 つの変数に対する操作によって、もう一方の変数によって参照されるオブジェクトに影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="f9942-661">構造体を使用すると、各々の変数がデータのコピーを各々で持ち、1 つに対する操作がもう一方に影響を与えることはできません。</span><span class="sxs-lookup"><span data-stu-id="f9942-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="f9942-662">たとえば、次のコード フラグメントによって生成される出力がかどうかに依存`Point`がクラスまたは構造体。</span><span class="sxs-lookup"><span data-stu-id="f9942-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="f9942-663">場合`Point`クラスで、出力は`20`ため`a`と`b`同じオブジェクトを参照します。</span><span class="sxs-lookup"><span data-stu-id="f9942-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="f9942-664">場合`Point`、構造体は、出力は、`10`ための割り当て`a`に`b`、値のコピーを作成し、このコピーはへの後続の割り当てによって影響を受けません`a.x`。</span><span class="sxs-lookup"><span data-stu-id="f9942-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="f9942-665">前述の例では、構造体の 2 つの制限事項が強調されています。</span><span class="sxs-lookup"><span data-stu-id="f9942-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="f9942-666">1 つめは、構造体全体をコピーすることは通常、オブジェクト参照をコピーするよりも非効率であり、割り当てと値パラメーターの引き渡しは参照型よりも構造体のほうが手がかかるということです。</span><span class="sxs-lookup"><span data-stu-id="f9942-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="f9942-667">2 つめは、`ref` および `out` パラメーターを除いて、構造体への参照を作成することはできず、そのために構造体を使用できない状況が数多くあるということです。</span><span class="sxs-lookup"><span data-stu-id="f9942-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="f9942-668">配列</span><span class="sxs-lookup"><span data-stu-id="f9942-668">Arrays</span></span>

<span data-ttu-id="f9942-669">***配列***はデータ構造で、算出されたインデックスを介してアクセスされる多くの変数を含みます。</span><span class="sxs-lookup"><span data-stu-id="f9942-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="f9942-670">配列に含まれる変数は配列の***要素***とも呼ばれ、すべて同じ型であり、この型は配列の***要素型***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="f9942-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="f9942-671">配列型は参照型で、配列変数の宣言は、配列インスタンスへの参照の領域を確保します。</span><span class="sxs-lookup"><span data-stu-id="f9942-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="f9942-672">使用して実行時に実際の配列インスタンスが動的に作成された、`new`演算子。</span><span class="sxs-lookup"><span data-stu-id="f9942-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="f9942-673">`new`操作を指定します、***長さ***新しい配列のインスタンスは、インスタンスの有効期間は固定です。</span><span class="sxs-lookup"><span data-stu-id="f9942-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="f9942-674">配列の要素のインデックスは、`0` から `Length - 1` までです。</span><span class="sxs-lookup"><span data-stu-id="f9942-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="f9942-675">`new` 演算子は配列の要素を自動的に既定値に初期化します。たとえば、すべての数値型はゼロ、すべての参照型は `null` です。</span><span class="sxs-lookup"><span data-stu-id="f9942-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="f9942-676">次の例では、`int` 要素の配列を作成し、その配列を初期化し、配列の内容を出力します。</span><span class="sxs-lookup"><span data-stu-id="f9942-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
<span data-ttu-id="f9942-677">この例は ***1 次元配列***を作成し、操作対象とします。</span><span class="sxs-lookup"><span data-stu-id="f9942-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="f9942-678">C# はさらに***多次元配列***をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f9942-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="f9942-679">配列型の次元数は、配列型の***ランク***とも呼ばれ、配列型の角かっこ内に記述されたコンマの数に 1 を追加したものです。</span><span class="sxs-lookup"><span data-stu-id="f9942-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="f9942-680">次の例では、1 次元、2 次元および 3 次元の配列を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="f9942-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="f9942-681">`a1` 配列は 10 の要素、`a2` 配列は 50 (10 × 5) の要素、`a3` 配列は 100 (10 × 5 × 2) の要素を含みます。</span><span class="sxs-lookup"><span data-stu-id="f9942-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="f9942-682">配列の要素型には、配列型を含む任意の型を指定できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="f9942-683">配列型の要素を持つ配列は、***ジャグ配列***と呼ばれることがあります。要素の配列の長さがすべて同じである必要がないからです。</span><span class="sxs-lookup"><span data-stu-id="f9942-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="f9942-684">次の例では、`int` の配列の配列を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="f9942-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="f9942-685">最初の行で 3 つの要素を持つ配列を作成しますが、各々は `int[]` 型で `null` 初期値を持ちます。</span><span class="sxs-lookup"><span data-stu-id="f9942-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="f9942-686">それ以降の行では、3 つの要素を、それぞれ異なる長さの配列インスタンスへの参照で初期化します。</span><span class="sxs-lookup"><span data-stu-id="f9942-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="f9942-687">`new`演算子を使用して指定する配列要素の初期値を許可する、***配列初期化子***、区切り記号の間に記述された式の一覧は`{`と`}`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="f9942-688">次の例は、`int[]` を割り当て 3 つの要素で初期化します。</span><span class="sxs-lookup"><span data-stu-id="f9942-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="f9942-689">配列の長さが間にある式の数から推論されることに注意してください。`{`と`}`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="f9942-690">ローカル変数およびフィールド宣言をさらに短縮して、配列型を再起動する必要がないようにできます。</span><span class="sxs-lookup"><span data-stu-id="f9942-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="f9942-691">前述の例はどちらも、次の例と同等です。</span><span class="sxs-lookup"><span data-stu-id="f9942-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="f9942-692">インターフェイス</span><span class="sxs-lookup"><span data-stu-id="f9942-692">Interfaces</span></span>

<span data-ttu-id="f9942-693">***インターフェイス***は、クラスと構造体によって実装できるコントラクトを定義します。</span><span class="sxs-lookup"><span data-stu-id="f9942-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="f9942-694">1 つのインターフェイスには、メソッド、プロパティ、イベント、およびインデクサーが含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="f9942-695">インターフェイスでは、定義するメンバーの実装は行いません。インターフェイスを実装するクラスまたは構造体によって提供される必要があるメンバーを指定するだけです。</span><span class="sxs-lookup"><span data-stu-id="f9942-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="f9942-696">インターフェイスは***多重継承***を使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="f9942-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="f9942-697">次の例では、インターフェイス `IComboBox` は `ITextBox` と `IListBox` の両方から継承します。</span><span class="sxs-lookup"><span data-stu-id="f9942-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

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
<span data-ttu-id="f9942-698">クラスと構造体は、複数のインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="f9942-699">次の例では、クラス `EditBox` は `IControl` と `IDataBound` の両方を実装しています。</span><span class="sxs-lookup"><span data-stu-id="f9942-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
<span data-ttu-id="f9942-700">クラスまたは構造体が特定のインターフェイスを実装する場合、そのクラスまたは構造体のインスタンスはそのインターフェイス型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="f9942-701">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="f9942-702">インスタンスが特定のインターフェイスを静的に実装しないとわかっている場合は、動的な型キャストを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="f9942-703">たとえば、次のステートメント オブジェクトの取得する動的な型キャストを使用して、`IControl`と`IDataBound`インターフェイスの実装。</span><span class="sxs-lookup"><span data-stu-id="f9942-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="f9942-704">オブジェクトの実際の型であるため`EditBox`キャストは成功します。</span><span class="sxs-lookup"><span data-stu-id="f9942-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="f9942-705">前の`EditBox`クラス、`Paint`からメソッド、`IControl`インターフェイスと`Bind`からメソッド、`IDataBound`を使用してインターフェイスを実装`public`メンバー。</span><span class="sxs-lookup"><span data-stu-id="f9942-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="f9942-706">C# もサポート***明示的なインターフェイス メンバーの実装***、しており、クラスまたは構造体は、メンバーにすることを回避できます`public`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="f9942-707">明示的なインターフェイス メンバーの実装は、完全修飾のインターフェイス メンバー名を使用して書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="f9942-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="f9942-708">たとえば、`EditBox` クラスは、次のように明示的なインターフェイス メンバーの実装を使用して、`IControl.Paint` メソッドおよび `IDataBound.Bind` メソッドを実装できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="f9942-709">明示的なインターフェイス メンバーは、インターフェイス型を経由してのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="f9942-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="f9942-710">実装ではたとえば、`IControl.Paint`前によって提供される`EditBox`クラスは、最初に変換することによってのみ呼び出すことができます、`EditBox`への参照、`IControl`インターフェイス型。</span><span class="sxs-lookup"><span data-stu-id="f9942-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="f9942-711">列挙体</span><span class="sxs-lookup"><span data-stu-id="f9942-711">Enums</span></span>

<span data-ttu-id="f9942-712">***列挙型***は、一連の名前付き定数を使用する固有の値の型です。</span><span class="sxs-lookup"><span data-stu-id="f9942-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="f9942-713">次の例を宣言してという名前の列挙型を使用する`Color`3 つの定数値を持つ`Red`、 `Green`、および`Blue`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
<span data-ttu-id="f9942-714">各列挙型が、対応する整数型と呼ばれる、***基になる型***の列挙型。</span><span class="sxs-lookup"><span data-stu-id="f9942-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="f9942-715">列挙型を基になる型を明示的に宣言しないが、基になる型の`int`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="f9942-716">列挙型のストレージ形式と使用可能な値の範囲については、基になる型によって決まります。</span><span class="sxs-lookup"><span data-stu-id="f9942-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="f9942-717">列挙型上で実行できる値のセットは、その列挙型のメンバーでは制限されません。</span><span class="sxs-lookup"><span data-stu-id="f9942-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="f9942-718">具体的には、enum の基になる型の任意の値は列挙型にキャストできるし、その列挙型の個別の有効な値は、します。</span><span class="sxs-lookup"><span data-stu-id="f9942-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="f9942-719">次の例は、という名前の列挙型を宣言`Alignment`の基になる種類で`sbyte`します。</span><span class="sxs-lookup"><span data-stu-id="f9942-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="f9942-720">前の例に示す、列挙型メンバーの宣言は、メンバーの値を指定する定数式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="f9942-721">各列挙型メンバーの定数値は、列挙型の基になる型の範囲でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="f9942-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="f9942-722">列挙型メンバーの宣言が明示的に値を指定しない場合に、値 0 (列挙型の最初のメンバーの場合) または直前の列挙型のメンバーと 1 つの値、メンバーが与えられます。</span><span class="sxs-lookup"><span data-stu-id="f9942-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="f9942-723">列挙型の値は整数に変換された値と型キャストを使用してその逆にできます。</span><span class="sxs-lookup"><span data-stu-id="f9942-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="f9942-724">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f9942-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="f9942-725">任意の列挙型の既定値は、列挙型に変換された 0 の整数の値です。</span><span class="sxs-lookup"><span data-stu-id="f9942-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="f9942-726">変数に自動的に既定値に初期化する場合、これは、列挙型の変数に渡された値です。</span><span class="sxs-lookup"><span data-stu-id="f9942-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="f9942-727">簡単に使用できる列挙型リテラルの既定値の順序で`0`任意の列挙型に暗黙的に変換します。</span><span class="sxs-lookup"><span data-stu-id="f9942-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="f9942-728">したがって、次の例も許可されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="f9942-729">デリゲート</span><span class="sxs-lookup"><span data-stu-id="f9942-729">Delegates</span></span>

<span data-ttu-id="f9942-730">***デリゲート型***は、特定のパラメーター リストおよび戻り値を使用してメソッドへの参照を表します。</span><span class="sxs-lookup"><span data-stu-id="f9942-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="f9942-731">デリゲートを使用すれば、変数に割り当ててパラメーターとして渡すことのできるエンティティとして、メソッドを処理できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="f9942-732">デリゲートはまた、他のいくつかの言語にみられる関数ポインターの概念に似ていますが、関数ポインターとは異なり、デリゲートはオブジェクト指向でタイプ セーフです。</span><span class="sxs-lookup"><span data-stu-id="f9942-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="f9942-733">次の例では、`Function` という名前のデリゲート型を宣言して使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-733">The following example declares and uses a delegate type named `Function`.</span></span>

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
<span data-ttu-id="f9942-734">`Function` デリゲート型のインスタンスは、`double` 引数を取得して `double` 値を返す任意のメソッドを参照できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="f9942-735">`Apply`メソッドが適用されます、特定`Function`の要素に、 `double[]`、返される、`double[]`結果。</span><span class="sxs-lookup"><span data-stu-id="f9942-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="f9942-736">`Main` メソッドでは、`Apply` は `double[]` に 3 つの異なる関数を適用するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="f9942-737">デリゲートは、静的メソッド (前述の例の `Square` や `Math.Sin` など) またはインスタンス メソッド (前述の例の `m.Multiply` など) のいずれかを参照できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="f9942-738">インスタンス メソッドを参照するデリゲートはまた、特定のオブジェクトを参照し、インスタンス メソッドがデリゲートから呼び出されると、そのオブジェクトは呼び出しで `this` になります。</span><span class="sxs-lookup"><span data-stu-id="f9942-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="f9942-739">その場で作成される「インライン メソッド」である匿名関数を使用してデリゲートを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="f9942-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="f9942-740">匿名関数では、周囲のメソッドのローカル変数を確認できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="f9942-741">したがってなどを使用せずを使用して、上記の乗数の例をより簡単に記述する、`Multiplier`クラス。</span><span class="sxs-lookup"><span data-stu-id="f9942-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="f9942-742">デリゲートの興味深く有用な点は、参照するメソッドのクラスを把握せず必要ともしないことです。重要なのは、参照されるメソッドは同じパラメーターを持ち、デリゲートとして型を返すということです。</span><span class="sxs-lookup"><span data-stu-id="f9942-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="f9942-743">属性</span><span class="sxs-lookup"><span data-stu-id="f9942-743">Attributes</span></span>

<span data-ttu-id="f9942-744">C# プログラムにおける型、メンバー、およびその他のエンティティは、動作の特定の側面を制御する修飾子をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f9942-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="f9942-745">たとえばメソッドのアクセシビリティは、`public`、`protected`、`internal`、および `private` 修飾子を使用して制御されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="f9942-746">C# はこの機能を一般化し、宣言情報のユーザー定義型をプログラム エンティティに追加して実行時に取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f9942-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="f9942-747">プログラムでは、***属性***を定義して使用することにより、この追加の宣言情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="f9942-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="f9942-748">次の例では、プログラムのエンティティに配置して関連するドキュメントへのリンクを提供することができる `HelpAttribute` 属性を宣言しています。</span><span class="sxs-lookup"><span data-stu-id="f9942-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
<span data-ttu-id="f9942-749">すべての属性クラスから派生、`System.Attribute`基本 .NET Framework で提供されるクラス。</span><span class="sxs-lookup"><span data-stu-id="f9942-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="f9942-750">属性は、関連付けられた宣言の直前に、名前を任意の変数とともに角かっこで囲んで与えることにより、適用できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="f9942-751">属性の名前で終わる場合`Attribute`属性が参照されている場合、名前の部分を省略できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="f9942-752">たとえば、`HelpAttribute` 属性は次のように使用できます。</span><span class="sxs-lookup"><span data-stu-id="f9942-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="f9942-753">この例ではアタッチ、`HelpAttribute`を`Widget`クラスと別`HelpAttribute`を`Display`クラスのメソッド。</span><span class="sxs-lookup"><span data-stu-id="f9942-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="f9942-754">属性クラスのパブリック コンストラクターは、属性がプログラム エンティティにアタッチされたときに提供する必要がある情報を制御します。</span><span class="sxs-lookup"><span data-stu-id="f9942-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="f9942-755">その属性クラスのパブリックの読み取り/書き込みプロパティを参照することにより (`Topic` プロパティへの参照のような)、追加情報を提供することができます。</span><span class="sxs-lookup"><span data-stu-id="f9942-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="f9942-756">次の例は、実行時に指定されたプログラム エンティティの属性情報を取得する方法を示しています。 リフレクションを使用します。</span><span class="sxs-lookup"><span data-stu-id="f9942-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
<span data-ttu-id="f9942-757">リフレクションによって特定の属性が要求されると、プログラム ソースで提供される情報で属性クラスのコンストラクターが呼び出され、結果の属性インスタンスが返されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="f9942-758">追加情報がプロパティを通じて提供された場合、属性インスタンスが返される前に、これらのプロパティは指定された値に設定されます。</span><span class="sxs-lookup"><span data-stu-id="f9942-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
