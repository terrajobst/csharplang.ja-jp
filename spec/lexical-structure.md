---
ms.openlocfilehash: 4676bcd3f0a92260b4e5e20a0aa5b5ec00bf204e
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704070"
---
# <a name="lexical-structure"></a><span data-ttu-id="2438f-101">字句構造</span><span class="sxs-lookup"><span data-stu-id="2438f-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="2438f-102">Programs</span><span class="sxs-lookup"><span data-stu-id="2438f-102">Programs</span></span>

<span data-ttu-id="2438f-103">C# ***プログラム***は、1つまたは複数の***ソースファイル***で構成され、正式には***コンパイルユニット***([コンパイル単位](namespaces.md#compilation-units)) として知られています。</span><span class="sxs-lookup"><span data-stu-id="2438f-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="2438f-104">ソースファイルは、順序付けられた Unicode 文字のシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="2438f-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="2438f-105">ソースファイルは、通常、ファイルシステム内のファイルと1対1で対応していますが、この対応は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="2438f-106">移植性を最大にするために、ファイルシステム内のファイルを UTF-8 エンコードでエンコードすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2438f-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="2438f-107">概念的に言えば、プログラムは次の3つの手順を使用してコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="2438f-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="2438f-108">変換。特定の文字レパートリーとエンコードスキームのファイルを Unicode 文字のシーケンスに変換します。</span><span class="sxs-lookup"><span data-stu-id="2438f-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="2438f-109">字句分析。 Unicode 入力文字のストリームをトークンのストリームに変換します。</span><span class="sxs-lookup"><span data-stu-id="2438f-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="2438f-110">構文分析。トークンのストリームを実行可能コードに変換します。</span><span class="sxs-lookup"><span data-stu-id="2438f-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="2438f-111">文法</span><span class="sxs-lookup"><span data-stu-id="2438f-111">Grammars</span></span>

<span data-ttu-id="2438f-112">この仕様には、2つC#の文法を使用したプログラミング言語の構文が示されています。</span><span class="sxs-lookup"><span data-stu-id="2438f-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="2438f-113">***字句文法***([字句文法](lexical-structure.md#lexical-grammar)) では、Unicode 文字を組み合わせることによって、行ターミネータ、空白、コメント、トークン、およびプリプロセスディレクティブを形成する方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="2438f-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="2438f-114">構文***文法 (*** 構文[文法](lexical-structure.md#syntactic-grammar)) は、字句文法の結果として得られるトークンをC#組み合わせてプログラムを形成する方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="2438f-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="2438f-115">文法の表記</span><span class="sxs-lookup"><span data-stu-id="2438f-115">Grammar notation</span></span>

<span data-ttu-id="2438f-116">字句文法と構文文法は、ANTLR 文法ツールの表記を使用して、バッカスナウア記法-Backus-naur 形式で表示されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="2438f-117">字句文法</span><span class="sxs-lookup"><span data-stu-id="2438f-117">Lexical grammar</span></span>

<span data-ttu-id="2438f-118">のC#字句文法は、[字句解析](lexical-structure.md#lexical-analysis)、[トークン](lexical-structure.md#tokens)、および[プリプロセスディレクティブ](lexical-structure.md#pre-processing-directives)で表現されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="2438f-119">字句文法のターミナルシンボルは Unicode 文字セットの文字であり、字句文法は、文字を結合してトークン ([トークン](lexical-structure.md#tokens))、空白 ([空白](lexical-structure.md#white-space))、コメント ([コメント](lexical-structure.md#comments))、プリプロセスディレクティブ ([プリプロセス](lexical-structure.md#pre-processing-directives)ディレクティブ) を形成する方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="2438f-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="2438f-120">C#プログラム内のすべてのソースファイルは、字句文法 ([字句解析](lexical-structure.md#lexical-analysis)) の*入力*の実稼働に準拠している必要があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="2438f-121">構文文法</span><span class="sxs-lookup"><span data-stu-id="2438f-121">Syntactic grammar</span></span>

<span data-ttu-id="2438f-122">のC#構文文法は、この章に従って、章と付録に記載されています。</span><span class="sxs-lookup"><span data-stu-id="2438f-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="2438f-123">構文文法のターミナルシンボルは、字句文法によって定義されたトークンです。構文文法では、プログラムを形成C#するためのトークンの結合方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="2438f-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="2438f-124">C#プログラム内のすべてのソースファイルは、構文文法 ([コンパイル単位](namespaces.md#compilation-units)) の*compilation_unit*運用環境に準拠している必要があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="2438f-125">字句解析</span><span class="sxs-lookup"><span data-stu-id="2438f-125">Lexical analysis</span></span>

<span data-ttu-id="2438f-126">*入力*の運用では、 C#ソースファイルの構文構造を定義します。</span><span class="sxs-lookup"><span data-stu-id="2438f-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="2438f-127">C#プログラム内の各ソースファイルは、この字句文法の実稼働に準拠している必要があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

```antlr
input
    : input_section?
    ;

input_section
    : input_section_part+
    ;

input_section_part
    : input_element* new_line
    | pp_directive
    ;

input_element
    : whitespace
    | comment
    | token
    ;
```

<span data-ttu-id="2438f-128">5つの基本C#要素は、ソースファイルの構文構造を構成します。これには、行ターミネータ ([行ターミネータ](lexical-structure.md#line-terminators))、空白 ([空白](lexical-structure.md#white-space))、コメント ([コメント](lexical-structure.md#comments))、トークン ([トークン](lexical-structure.md#tokens))、プリプロセスディレクティブ ([プリプロセスディレクティブ](lexical-structure.md#pre-processing-directives)) があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="2438f-129">これらの基本的な要素の中で、 C#プログラムの構文文法ではトークンのみが重要です ([構文文法](lexical-structure.md#syntactic-grammar))。</span><span class="sxs-lookup"><span data-stu-id="2438f-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="2438f-130">C#ソースファイルの字句処理では、ファイルを一連のトークンに減らすことで、構文分析への入力となります。</span><span class="sxs-lookup"><span data-stu-id="2438f-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="2438f-131">行ターミネータ、空白、コメントはトークンを区切るために使用でき、プリプロセスディレクティブによってソースファイルのセクションがスキップされることがありますが、それ以外の場合、これらの字句要素C#はプログラムの構文構造には影響しません。</span><span class="sxs-lookup"><span data-stu-id="2438f-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="2438f-132">挿入文字列リテラル ([補間文字列](lexical-structure.md#interpolated-string-literals)リテラル) の場合、1つのトークンは字句解析によって最初に生成されますが、複数の入力要素に分割されます。これは、すべての補間文字列リテラルが解決されるまで、字句解析に繰り返し適用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="2438f-133">その後、結果として得られるトークンは、構文分析の入力として機能します。</span><span class="sxs-lookup"><span data-stu-id="2438f-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="2438f-134">複数の字句文法の発行がソースファイル内の文字のシーケンスと一致する場合、構文処理では常に、可能な限り長い字句要素が形成されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="2438f-135">たとえば、文字シーケンス `//` は単一行コメントの先頭として処理されます。これは、その字句要素が1つの `/` トークンより長いためです。</span><span class="sxs-lookup"><span data-stu-id="2438f-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="2438f-136">ラインターミネータ</span><span class="sxs-lookup"><span data-stu-id="2438f-136">Line terminators</span></span>

<span data-ttu-id="2438f-137">行ターミネータは、 C#ソースファイルの文字を行に分割します。</span><span class="sxs-lookup"><span data-stu-id="2438f-137">Line terminators divide the characters of a C# source file into lines.</span></span>

```antlr
new_line
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Carriage return character (U+000D) followed by line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;
```

<span data-ttu-id="2438f-138">ファイルの終端マーカーを追加するソースコード編集ツールとの互換性を確保し、適切に終了した行のシーケンスとしてソースファイルを表示できるようにするには、 C#プログラム内のすべてのソースファイルに次の変換を順番に適用します。</span><span class="sxs-lookup"><span data-stu-id="2438f-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="2438f-139">ソースファイルの最後の文字がコントロール Z 文字 (`U+001A`) の場合、この文字は削除されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="2438f-140">ソースファイルが空ではなく、ソースファイルの最後の文字が復帰 (`U+000D`)、ラインフィード (`U+000A`)、行区切り記号 (`U+2028`)、または段落区切り記号 (`U+2029`) の場合、ソースファイルの末尾に復帰文字 (`U+000D`) が追加されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="2438f-141">コメント</span><span class="sxs-lookup"><span data-stu-id="2438f-141">Comments</span></span>

<span data-ttu-id="2438f-142">1行のコメントと区切られたコメントの2つの形式のコメントがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="2438f-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="2438f-143">***単一行コメント***は `//` 文字で始まり、ソース行の末尾まで拡張されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="2438f-144">***区切ら***れたコメントは `/*` 文字で始まり、`*/`文字で終わります。</span><span class="sxs-lookup"><span data-stu-id="2438f-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="2438f-145">区切られたコメントは複数行にまたがる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-145">Delimited comments may span multiple lines.</span></span>

```antlr
comment
    : single_line_comment
    | delimited_comment
    ;

single_line_comment
    : '//' input_character*
    ;

input_character
    : '<Any Unicode character except a new_line_character>'
    ;

new_line_character
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;

delimited_comment
    : '/*' delimited_comment_section* asterisk+ '/'
    ;

delimited_comment_section
    : '/'
    | asterisk* not_slash_or_asterisk
    ;

asterisk
    : '*'
    ;

not_slash_or_asterisk
    : '<Any Unicode character except / or *>'
    ;
```

<span data-ttu-id="2438f-146">コメントは入れ子になっていません。</span><span class="sxs-lookup"><span data-stu-id="2438f-146">Comments do not nest.</span></span> <span data-ttu-id="2438f-147">文字シーケンス `/*` と `*/` は `//` コメント内で特別な意味を持ちません。また、`//` および `/*` の文字シーケンスは、区切られたコメント内で特別な意味を持ちません。</span><span class="sxs-lookup"><span data-stu-id="2438f-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="2438f-148">コメントは、文字リテラルと文字列リテラル内では処理されません。</span><span class="sxs-lookup"><span data-stu-id="2438f-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="2438f-149">例</span><span class="sxs-lookup"><span data-stu-id="2438f-149">The example</span></span>
```csharp
/* Hello, world program
   This program writes "hello, world" to the console
*/
class Hello
{
    static void Main() {
        System.Console.WriteLine("hello, world");
    }
}
```
<span data-ttu-id="2438f-150">区切られたコメントを含みます。</span><span class="sxs-lookup"><span data-stu-id="2438f-150">includes a delimited comment.</span></span>

<span data-ttu-id="2438f-151">例</span><span class="sxs-lookup"><span data-stu-id="2438f-151">The example</span></span>
```csharp
// Hello, world program
// This program writes "hello, world" to the console
//
class Hello // any name will do for this class
{
    static void Main() { // this method must be named "Main"
        System.Console.WriteLine("hello, world");
    }
}
```
<span data-ttu-id="2438f-152">複数の単一行コメントを表示します。</span><span class="sxs-lookup"><span data-stu-id="2438f-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="2438f-153">空白</span><span class="sxs-lookup"><span data-stu-id="2438f-153">White space</span></span>

<span data-ttu-id="2438f-154">空白は、Unicode クラス Zs (空白文字を含む) と、水平タブ文字、垂直タブ文字、およびフォームフィード文字を含む任意の文字として定義されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="2438f-155">トークン</span><span class="sxs-lookup"><span data-stu-id="2438f-155">Tokens</span></span>

<span data-ttu-id="2438f-156">トークンには、識別子、キーワード、リテラル、演算子、および区切り記号のいくつかの種類があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="2438f-157">空白とコメントはトークンではありませんが、トークンの区切り記号として機能します。</span><span class="sxs-lookup"><span data-stu-id="2438f-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

```antlr
token
    : identifier
    | keyword
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | interpolated_string_literal
    | operator_or_punctuator
    ;
```

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="2438f-158">Unicode 文字エスケープシーケンス</span><span class="sxs-lookup"><span data-stu-id="2438f-158">Unicode character escape sequences</span></span>

<span data-ttu-id="2438f-159">Unicode 文字のエスケープシーケンスは、Unicode 文字を表します。</span><span class="sxs-lookup"><span data-stu-id="2438f-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="2438f-160">Unicode 文字のエスケープシーケンスは、識別子 ([識別子](lexical-structure.md#identifiers))、文字リテラル ([文字](lexical-structure.md#character-literals)リテラル)、および通常の文字列リテラル ([文字列リテラル](lexical-structure.md#string-literals)) で処理されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="2438f-161">Unicode 文字のエスケープは、他の場所では処理されません (たとえば、operator、など、または keyword を形成する場合)。</span><span class="sxs-lookup"><span data-stu-id="2438f-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="2438f-162">Unicode エスケープシーケンスは、"`\u`" または "`\U`" 文字の後に続く16進数で形成される1つの Unicode 文字を表します。</span><span class="sxs-lookup"><span data-stu-id="2438f-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="2438f-163">でC#は、文字と文字列値で unicode コードポイントの16ビットエンコードが使用されるため、u + 10000 から u + 10ffff の範囲の unicode 文字は、文字リテラルでは許可されず、文字列リテラルでは unicode サロゲートペアを使用して表されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="2438f-164">0x10FFFF を超えるコードポイントを含む Unicode 文字はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="2438f-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="2438f-165">複数の翻訳は実行されません。</span><span class="sxs-lookup"><span data-stu-id="2438f-165">Multiple translations are not performed.</span></span> <span data-ttu-id="2438f-166">たとえば、"`\u005Cu005C`" という文字列リテラルは、"`\`" ではなく "`\u005C`" に相当します。</span><span class="sxs-lookup"><span data-stu-id="2438f-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="2438f-167">`\u005C` Unicode 値は文字 "`\`" です。</span><span class="sxs-lookup"><span data-stu-id="2438f-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="2438f-168">例</span><span class="sxs-lookup"><span data-stu-id="2438f-168">The example</span></span>
```csharp
class Class1
{
    static void Test(bool \u0066) {
        char c = '\u0066';
        if (\u0066)
            System.Console.WriteLine(c.ToString());
    }        
}
```
<span data-ttu-id="2438f-169">`\u0066`のいくつかの使用例を示します。これは、文字 "`f`" のエスケープシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="2438f-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="2438f-170">プログラムはに相当します。</span><span class="sxs-lookup"><span data-stu-id="2438f-170">The program is equivalent to</span></span>
```csharp
class Class1
{
    static void Test(bool f) {
        char c = 'f';
        if (f)
            System.Console.WriteLine(c.ToString());
    }        
}
```

### <a name="identifiers"></a><span data-ttu-id="2438f-171">識別子</span><span class="sxs-lookup"><span data-stu-id="2438f-171">Identifiers</span></span>

<span data-ttu-id="2438f-172">このセクションで示す識別子の規則は、Unicode 標準の使用方法31で推奨されている規則に正確に対応しています。ただし、アンダースコアは (C プログラミング言語では従来のように) 最初の文字として許可されていますが、識別子としてキーワードを使用できるようにするためのプレフィックスとして "`@`"</span><span class="sxs-lookup"><span data-stu-id="2438f-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

```antlr
identifier
    : available_identifier
    | '@' identifier_or_keyword
    ;

available_identifier
    : '<An identifier_or_keyword that is not a keyword>'
    ;

identifier_or_keyword
    : identifier_start_character identifier_part_character*
    ;

identifier_start_character
    : letter_character
    | '_'
    ;

identifier_part_character
    : letter_character
    | decimal_digit_character
    | connecting_character
    | combining_character
    | formatting_character
    ;

letter_character
    : '<A Unicode character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    | '<A unicode_escape_sequence representing a character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    ;

combining_character
    : '<A Unicode character of classes Mn or Mc>'
    | '<A unicode_escape_sequence representing a character of classes Mn or Mc>'
    ;

decimal_digit_character
    : '<A Unicode character of the class Nd>'
    | '<A unicode_escape_sequence representing a character of the class Nd>'
    ;

connecting_character
    : '<A Unicode character of the class Pc>'
    | '<A unicode_escape_sequence representing a character of the class Pc>'
    ;

formatting_character
    : '<A Unicode character of the class Cf>'
    | '<A unicode_escape_sequence representing a character of the class Cf>'
    ;
```

<span data-ttu-id="2438f-173">上記の Unicode 文字クラスの詳細については、Unicode Standard バージョン3.0 のセクション4.5 を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2438f-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="2438f-174">有効な識別子の例としては、"`identifier1`"、"`_identifier2`"、"`@if`" などがあります。</span><span class="sxs-lookup"><span data-stu-id="2438f-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="2438f-175">準拠するプログラムの識別子は、unicode 規格の使用方法15で定義されているように、Unicode 正規形 C で定義されている正規の形式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="2438f-176">正規形 C ではない識別子に遭遇した場合の動作は、実装によって定義されます。ただし、診断は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="2438f-177">プレフィックス "`@`" を使用すると、キーワードを識別子として使用できるようになります。これは、他のプログラミング言語とのやり取りに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="2438f-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="2438f-178">`@` 文字は実際には識別子の一部ではないため、識別子は、プレフィックスなしで通常の識別子として他の言語で認識される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="2438f-179">`@` プレフィックスを持つ識別子は、逐語的***識別子***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="2438f-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="2438f-180">キーワードではない識別子に対して `@` プレフィックスを使用することは許可されますが、スタイルの問題としては使用しないことを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2438f-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="2438f-181">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2438f-181">The example:</span></span>
```csharp
class @class
{
    public static void @static(bool @bool) {
        if (@bool)
            System.Console.WriteLine("true");
        else
            System.Console.WriteLine("false");
    }    
}

class Class1
{
    static void M() {
        cl\u0061ss.st\u0061tic(true);
    }
}
```
<span data-ttu-id="2438f-182">"`bool`" という名前のパラメーターを受け取る "`static`" という名前の静的メソッドを使用して、"`class`" という名前のクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="2438f-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="2438f-183">キーワードでは Unicode エスケープは許可されていないため、トークン "`cl\u0061ss`" は識別子であり、"`@class`" と同じ識別子であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2438f-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="2438f-184">2つの識別子は、次の変換が適用された後に同一である場合は同じと見なされます。</span><span class="sxs-lookup"><span data-stu-id="2438f-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="2438f-185">プレフィックス "`@`" (使用されている場合) は削除されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="2438f-186">各*unicode_escape_sequence*は、対応する unicode 文字に変換されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="2438f-187">すべての*formatting_character*が削除されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="2438f-188">2つの連続するアンダースコア文字 (`U+005F`) を含む識別子は、実装で使用するために予約されています。</span><span class="sxs-lookup"><span data-stu-id="2438f-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="2438f-189">たとえば、実装では、2つのアンダースコアで始まる拡張キーワードを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2438f-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="2438f-190">キーワード</span><span class="sxs-lookup"><span data-stu-id="2438f-190">Keywords</span></span>

<span data-ttu-id="2438f-191">***キーワード***は、予約されている識別子に似た文字シーケンスであり、`@` 文字で始まる場合を除き、識別子として使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="2438f-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

```antlr
keyword
    : 'abstract' | 'as'       | 'base'       | 'bool'      | 'break'
    | 'byte'     | 'case'     | 'catch'      | 'char'      | 'checked'
    | 'class'    | 'const'    | 'continue'   | 'decimal'   | 'default'
    | 'delegate' | 'do'       | 'double'     | 'else'      | 'enum'
    | 'event'    | 'explicit' | 'extern'     | 'false'     | 'finally'
    | 'fixed'    | 'float'    | 'for'        | 'foreach'   | 'goto'
    | 'if'       | 'implicit' | 'in'         | 'int'       | 'interface'
    | 'internal' | 'is'       | 'lock'       | 'long'      | 'namespace'
    | 'new'      | 'null'     | 'object'     | 'operator'  | 'out'
    | 'override' | 'params'   | 'private'    | 'protected' | 'public'
    | 'readonly' | 'ref'      | 'return'     | 'sbyte'     | 'sealed'
    | 'short'    | 'sizeof'   | 'stackalloc' | 'static'    | 'string'
    | 'struct'   | 'switch'   | 'this'       | 'throw'     | 'true'
    | 'try'      | 'typeof'   | 'uint'       | 'ulong'     | 'unchecked'
    | 'unsafe'   | 'ushort'   | 'using'      | 'virtual'   | 'void'
    | 'volatile' | 'while'
    ;
```

<span data-ttu-id="2438f-192">文法の一部では、特定の識別子は特別な意味を持ちますが、キーワードではありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="2438f-193">このような識別子は、"コンテキストキーワード" と呼ばれることもあります。</span><span class="sxs-lookup"><span data-stu-id="2438f-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="2438f-194">たとえば、プロパティの宣言内では、"`get`" 識別子と "`set`" 識別子には特別な意味 ([アクセサー](classes.md#accessors)) があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="2438f-195">`get` または `set` 以外の識別子は、これらの場所では許可されないため、この使用は、これらの単語を識別子として使用する場合と競合しません。</span><span class="sxs-lookup"><span data-stu-id="2438f-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="2438f-196">他のケースでは、暗黙的に型指定されたローカル変数宣言 ([ローカル変数宣言](statements.md#local-variable-declarations)) に識別子 "`var`" があるなどの場合、コンテキストキーワードは宣言された名前と競合する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="2438f-197">このような場合、宣言された名前は、コンテキストキーワードとして識別子を使用するよりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="2438f-198">リテラル</span><span class="sxs-lookup"><span data-stu-id="2438f-198">Literals</span></span>

<span data-ttu-id="2438f-199">***リテラル***は、値のソースコード表現です。</span><span class="sxs-lookup"><span data-stu-id="2438f-199">A ***literal*** is a source code representation of a value.</span></span>

```antlr
literal
    : boolean_literal
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | null_literal
    ;
```

#### <a name="boolean-literals"></a><span data-ttu-id="2438f-200">ブール型リテラル</span><span class="sxs-lookup"><span data-stu-id="2438f-200">Boolean literals</span></span>

<span data-ttu-id="2438f-201">ブール型リテラル値には、`true` と `false`の2つがあります。</span><span class="sxs-lookup"><span data-stu-id="2438f-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="2438f-202">*Boolean_literal*の型が `bool`。</span><span class="sxs-lookup"><span data-stu-id="2438f-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="2438f-203">整数リテラル</span><span class="sxs-lookup"><span data-stu-id="2438f-203">Integer literals</span></span>

<span data-ttu-id="2438f-204">整数リテラルは、`int`、`uint`、`long`、および `ulong`型の値を書き込むために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="2438f-205">整数リテラルには、10進数と16進数という2つの形式があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

```antlr
integer_literal
    : decimal_integer_literal
    | hexadecimal_integer_literal
    ;

decimal_integer_literal
    : decimal_digit+ integer_type_suffix?
    ;

decimal_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

integer_type_suffix
    : 'U' | 'u' | 'L' | 'l' | 'UL' | 'Ul' | 'uL' | 'ul' | 'LU' | 'Lu' | 'lU' | 'lu'
    ;

hexadecimal_integer_literal
    : '0x' hex_digit+ integer_type_suffix?
    | '0X' hex_digit+ integer_type_suffix?
    ;

hex_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'a' | 'b' | 'c' | 'd' | 'e' | 'f';
```

<span data-ttu-id="2438f-206">整数リテラルの型は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="2438f-207">リテラルにサフィックスがない場合は、`int`、`uint`、`long`、`ulong`の各型の値を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="2438f-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="2438f-208">リテラルのサフィックスが `U` または `u`の場合、`uint`、`ulong`の各型の値を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="2438f-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="2438f-209">リテラルのサフィックスが `L` または `l`の場合、`long`、`ulong`の各型の値を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="2438f-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="2438f-210">リテラルのサフィックスが `UL`、`Ul`、`uL`、`ul`、`LU`、`Lu`、`lU`、または `lu`の場合、型は `ulong`になります。</span><span class="sxs-lookup"><span data-stu-id="2438f-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="2438f-211">整数リテラルによって表される値が `ulong` 型の範囲外にある場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="2438f-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="2438f-212">スタイルの場合は、`long`型のリテラルを記述するときに "`l`" の代わりに "`L`" を使用することをお勧めします。これは、文字 "`l`" を数字の "`1`" と混同しやすいためです。</span><span class="sxs-lookup"><span data-stu-id="2438f-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="2438f-213">可能な限り最小の `int` と `long` 値を10進整数リテラルとして記述できるようにするには、次の2つの規則が存在します。</span><span class="sxs-lookup"><span data-stu-id="2438f-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="2438f-214">値が 2147483648 (2 ^ 31) で*decimal_integer_literal*が、単項マイナス演算子トークン ([単項マイナス演算子](expressions.md#unary-minus-operator)) の直後にトークンとして表示され*integer_type_suffix*ない場合、結果は-2147483648 (-2 ^ 31) の値を持つ `int` 型の定数になります。</span><span class="sxs-lookup"><span data-stu-id="2438f-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="2438f-215">その他のすべての状況では、このような*decimal_integer_literal*は `uint`型になります。</span><span class="sxs-lookup"><span data-stu-id="2438f-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="2438f-216">値が 9223372036854775808 (2 ^ 63) で、 *integer_type_suffix*または*integer_type_suffix* `L` または `l` の*decimal_integer_literal*が単項マイナス演算子トークン ([単項マイナス演算子](expressions.md#unary-minus-operator)) の直後にトークンとして表示される場合、結果は-9223372036854775808 (-2 ^ 63) の値を持つ型の定数になります。`long`</span><span class="sxs-lookup"><span data-stu-id="2438f-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="2438f-217">その他のすべての状況では、このような*decimal_integer_literal*は `ulong`型になります。</span><span class="sxs-lookup"><span data-stu-id="2438f-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="2438f-218">実数リテラル</span><span class="sxs-lookup"><span data-stu-id="2438f-218">Real literals</span></span>

<span data-ttu-id="2438f-219">実数リテラルは、`float`、`double`、および `decimal`型の値を書き込むために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

```antlr
real_literal
    : decimal_digit+ '.' decimal_digit+ exponent_part? real_type_suffix?
    | '.' decimal_digit+ exponent_part? real_type_suffix?
    | decimal_digit+ exponent_part real_type_suffix?
    | decimal_digit+ real_type_suffix
    ;

exponent_part
    : 'e' sign? decimal_digit+
    | 'E' sign? decimal_digit+
    ;

sign
    : '+'
    | '-'
    ;

real_type_suffix
    : 'F' | 'f' | 'D' | 'd' | 'M' | 'm'
    ;
```

<span data-ttu-id="2438f-220">*Real_type_suffix*が指定されていない場合、実際のリテラルの型は `double`になります。</span><span class="sxs-lookup"><span data-stu-id="2438f-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="2438f-221">それ以外の場合は、実際の型サフィックスによって、次のように実際のリテラルの型が決定されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="2438f-222">`F` または `f` のサフィックスが付いた実際のリテラルは `float`型です。</span><span class="sxs-lookup"><span data-stu-id="2438f-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="2438f-223">たとえば、リテラル `1f`、`1.5f`、`1e10f`、および `123.456F` はすべて型 `float`です。</span><span class="sxs-lookup"><span data-stu-id="2438f-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="2438f-224">`D` または `d` のサフィックスが付いた実際のリテラルは `double`型です。</span><span class="sxs-lookup"><span data-stu-id="2438f-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="2438f-225">たとえば、リテラル `1d`、`1.5d`、`1e10d`、および `123.456D` はすべて型 `double`です。</span><span class="sxs-lookup"><span data-stu-id="2438f-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="2438f-226">`M` または `m` のサフィックスが付いた実際のリテラルは `decimal`型です。</span><span class="sxs-lookup"><span data-stu-id="2438f-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="2438f-227">たとえば、リテラル `1m`、`1.5m`、`1e10m`、および `123.456M` はすべて型 `decimal`です。</span><span class="sxs-lookup"><span data-stu-id="2438f-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="2438f-228">このリテラルは、正確な値を取得することによって `decimal` 値に変換されます。必要に応じて、銀行型丸め ([10 進数型](types.md#the-decimal-type)) を使用して、最も近い表現可能値に丸めます。</span><span class="sxs-lookup"><span data-stu-id="2438f-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="2438f-229">リテラル内のすべての小数点以下桁数は、値が丸められた場合、または値がゼロの場合 (後者の場合は符号と小数点以下桁数が0になります)、保持されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="2438f-230">したがって、リテラル `2.900m` は、符号 `0`、係数 `2900`、およびスケール `3`を含む decimal を形成するために解析されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="2438f-231">指定されたリテラルを指定された型で表すことができない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="2438f-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="2438f-232">`float` または `double` 型の実数リテラルの値は、IEEE "近似値に丸める" モードを使用して決定されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="2438f-233">実際のリテラルでは、小数点の後には常に10進数が必要であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2438f-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="2438f-234">たとえば、`1.3F` は実際のリテラルですが、`1.F` はありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="2438f-235">文字リテラル</span><span class="sxs-lookup"><span data-stu-id="2438f-235">Character literals</span></span>

<span data-ttu-id="2438f-236">文字リテラルは1つの文字を表し、通常は `'a'`のように引用符で囲まれた文字で構成されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="2438f-237">注: ANTLR 文法表記法を使用すると、次のように混乱が生じます。</span><span class="sxs-lookup"><span data-stu-id="2438f-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="2438f-238">ANTLR では、`\'` を記述するときに、単一引用符 `'`を表します。</span><span class="sxs-lookup"><span data-stu-id="2438f-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="2438f-239">また `\\` を作成すると、1つの円記号 `\`を表します。</span><span class="sxs-lookup"><span data-stu-id="2438f-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="2438f-240">したがって、文字リテラルの最初の規則は、1つの引用符、1文字、1つの引用符で始まることを意味します。</span><span class="sxs-lookup"><span data-stu-id="2438f-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="2438f-241">また、11つの単純なエスケープシーケンスは、`\'`、`\"`、`\\`、`\0`、`\a`、`\b`、`\f`、`\n`、`\r`、`\t`、`\v`です。</span><span class="sxs-lookup"><span data-stu-id="2438f-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

```antlr
character_literal
    : '\'' character '\''
    ;

character
    : single_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_character
    : '<Any character except \' (U+0027), \\ (U+005C), and new_line_character>'
    ;

simple_escape_sequence
    : '\\\'' | '\\"' | '\\\\' | '\\0' | '\\a' | '\\b' | '\\f' | '\\n' | '\\r' | '\\t' | '\\v'
    ;

hexadecimal_escape_sequence
    : '\\x' hex_digit hex_digit? hex_digit? hex_digit?;
```

<span data-ttu-id="2438f-242">*文字*内の円記号 (`\`) の後に続く文字は、`'`、`"`、`\`、`0`、`a`、`b`、`f`、`n`、`r`、`t`、`u`、`U`、`x`のいずれかである必要があります。`v`</span><span class="sxs-lookup"><span data-stu-id="2438f-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="2438f-243">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="2438f-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="2438f-244">16進数のエスケープシーケンスは、1つの Unicode 文字を表します。値は、"`\x`" の後の16進数で形成されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="2438f-245">文字リテラルによって表される値が `U+FFFF`よりも大きい場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="2438f-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="2438f-246">文字リテラル内の Unicode 文字エスケープシーケンス ([unicode 文字エスケープ](lexical-structure.md#unicode-character-escape-sequences)シーケンス) は、`U+FFFF`する `U+0000` の範囲内である必要があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="2438f-247">単純なエスケープシーケンスは、次の表に示すように、Unicode 文字エンコーディングを表します。</span><span class="sxs-lookup"><span data-stu-id="2438f-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="2438f-248">__エスケープシーケンス__</span><span class="sxs-lookup"><span data-stu-id="2438f-248">__Escape sequence__</span></span> | <span data-ttu-id="2438f-249">__文字名__</span><span class="sxs-lookup"><span data-stu-id="2438f-249">__Character name__</span></span> | <span data-ttu-id="2438f-250">__Unicode エンコード__</span><span class="sxs-lookup"><span data-stu-id="2438f-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="2438f-251">単一引用符</span><span class="sxs-lookup"><span data-stu-id="2438f-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="2438f-252">二重引用符</span><span class="sxs-lookup"><span data-stu-id="2438f-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="2438f-253">円記号</span><span class="sxs-lookup"><span data-stu-id="2438f-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="2438f-254">Null</span><span class="sxs-lookup"><span data-stu-id="2438f-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="2438f-255">アラート</span><span class="sxs-lookup"><span data-stu-id="2438f-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="2438f-256">バックスペース</span><span class="sxs-lookup"><span data-stu-id="2438f-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="2438f-257">フォーム フィード</span><span class="sxs-lookup"><span data-stu-id="2438f-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="2438f-258">改行</span><span class="sxs-lookup"><span data-stu-id="2438f-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="2438f-259">キャリッジ リターン</span><span class="sxs-lookup"><span data-stu-id="2438f-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="2438f-260">水平タブ</span><span class="sxs-lookup"><span data-stu-id="2438f-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="2438f-261">垂直タブ</span><span class="sxs-lookup"><span data-stu-id="2438f-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="2438f-262">*Character_literal*の型が `char`。</span><span class="sxs-lookup"><span data-stu-id="2438f-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="2438f-263">文字列リテラル</span><span class="sxs-lookup"><span data-stu-id="2438f-263">String literals</span></span>

<span data-ttu-id="2438f-264">C#では、***正規文字列***リテラルと***逐語的文字列リテラル***という2つの形式の文字列リテラルがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="2438f-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="2438f-265">通常の文字列リテラルは、`"hello"`のように、二重引用符で囲まれた0個以上の文字で構成されます。また、単純なエスケープシーケンス (タブ文字の `\t` など) と16進数および Unicode のエスケープシーケンスを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="2438f-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="2438f-266">逐語的文字列リテラルは、`@` 文字の後に二重引用符文字、0個以上の文字、および終わりの二重引用符で構成されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="2438f-267">単純な例として、`@"hello"`があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="2438f-268">逐語的文字列リテラルでは、区切り記号の間の文字は逐語的に解釈されます。唯一の例外は*quote_escape_sequence*です。</span><span class="sxs-lookup"><span data-stu-id="2438f-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="2438f-269">特に、単純なエスケープシーケンス、および16進数および Unicode のエスケープシーケンスは、逐語的文字列リテラルでは処理されません。</span><span class="sxs-lookup"><span data-stu-id="2438f-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="2438f-270">逐語的文字列リテラルは複数の行にまたがる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-270">A verbatim string literal may span multiple lines.</span></span>

```antlr
string_literal
    : regular_string_literal
    | verbatim_string_literal
    ;

regular_string_literal
    : '"' regular_string_literal_character* '"'
    ;

regular_string_literal_character
    : single_regular_string_literal_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_regular_string_literal_character
    : '<Any character except " (U+0022), \\ (U+005C), and new_line_character>'
    ;

verbatim_string_literal
    : '@"' verbatim_string_literal_character* '"'
    ;

verbatim_string_literal_character
    : single_verbatim_string_literal_character
    | quote_escape_sequence
    ;

single_verbatim_string_literal_character
    : '<any character except ">'
    ;

quote_escape_sequence
    : '""'
    ;
```

<span data-ttu-id="2438f-271">*Regular_string_literal_character*内の円記号 (`\`) の後に続く文字は、`'`、`"`、`\`、`0`、`a`、`b`、`f`、`n`、`r`、`t`のいずれかである必要があります。`u``U``x``v`</span><span class="sxs-lookup"><span data-stu-id="2438f-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="2438f-272">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="2438f-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="2438f-273">例</span><span class="sxs-lookup"><span data-stu-id="2438f-273">The example</span></span>
```csharp
string a = "hello, world";                   // hello, world
string b = @"hello, world";                  // hello, world

string c = "hello \t world";                 // hello      world
string d = @"hello \t world";                // hello \t world

string e = "Joe said \"Hello\" to me";       // Joe said "Hello" to me
string f = @"Joe said ""Hello"" to me";      // Joe said "Hello" to me

string g = "\\\\server\\share\\file.txt";    // \\server\share\file.txt
string h = @"\\server\share\file.txt";       // \\server\share\file.txt

string i = "one\r\ntwo\r\nthree";
string j = @"one
two
three";
```
<span data-ttu-id="2438f-274">さまざまな文字列リテラルを示します。</span><span class="sxs-lookup"><span data-stu-id="2438f-274">shows a variety of string literals.</span></span> <span data-ttu-id="2438f-275">最後の文字列リテラル `j`は、複数行にわたる逐語的文字列リテラルです。</span><span class="sxs-lookup"><span data-stu-id="2438f-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="2438f-276">引用符の間の文字 (改行文字などの空白文字を含む) は、そのまま保持されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="2438f-277">16進数のエスケープシーケンスには数値の16進数字を使用できますが、文字列リテラル `"\x123"` には、16進数値123を持つ1文字が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2438f-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="2438f-278">16進数値12の後に3文字目が続く文字を含む文字列を作成するには、代わりに `"\x00123"` または `"\x12" + "3"` を書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="2438f-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="2438f-279">*String_literal*の型が `string`。</span><span class="sxs-lookup"><span data-stu-id="2438f-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="2438f-280">各文字列リテラルは、必ずしも新しい文字列インスタンスになるとは限りません。</span><span class="sxs-lookup"><span data-stu-id="2438f-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="2438f-281">文字列等値演算子 (文字列[等値演算子](expressions.md#string-equality-operators)) によって等価である2つ以上の文字列リテラルが同じプログラムに出現する場合、これらの文字列リテラルは同じ文字列インスタンスを参照します。</span><span class="sxs-lookup"><span data-stu-id="2438f-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="2438f-282">たとえば、によって生成される出力は、</span><span class="sxs-lookup"><span data-stu-id="2438f-282">For instance, the output produced by</span></span>
```csharp
class Test
{
    static void Main() {
        object a = "hello";
        object b = "hello";
        System.Console.WriteLine(a == b);
    }
}
```
<span data-ttu-id="2438f-283">2つのリテラルが同じ文字列インスタンスを参照しているため `True`。</span><span class="sxs-lookup"><span data-stu-id="2438f-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="2438f-284">挿入文字列リテラル</span><span class="sxs-lookup"><span data-stu-id="2438f-284">Interpolated string literals</span></span>

<span data-ttu-id="2438f-285">挿入文字列リテラルは文字列リテラルに似ていますが、式が発生する可能性がある `{` および `}`で区切られた穴を含みます。</span><span class="sxs-lookup"><span data-stu-id="2438f-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="2438f-286">実行時には、式は、テキスト形式を、穴が発生した場所の文字列に置き換えることを目的として評価されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="2438f-287">文字列補間の構文とセマンティクスについては、「」 (挿入[文字列](expressions.md#interpolated-strings)) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2438f-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="2438f-288">文字列リテラルと同様に、補間文字列リテラルは通常の場合も逐語的な場合もあります。</span><span class="sxs-lookup"><span data-stu-id="2438f-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="2438f-289">補間された正規文字列リテラルは `$"` と `"`で区切られ、その後の文字列リテラルは `$@"` および `"`によって区切られます。</span><span class="sxs-lookup"><span data-stu-id="2438f-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="2438f-290">他のリテラルと同様に、補間文字列リテラルの字句解析では、次の文法に従って、最初に1つのトークンが生成されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="2438f-291">ただし、構文分析の前に、補間文字列リテラルの1つのトークンは、穴を囲む文字列の部分に対して複数のトークンに分割されます。また、穴で発生する入力要素は、構文的に分析されされます。</span><span class="sxs-lookup"><span data-stu-id="2438f-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="2438f-292">これにより、より多くの補間文字列リテラルが生成される可能性がありますが、構文が正しければ、最終的には、最終的には構文分析のためにトークンのシーケンスが処理されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

```antlr
interpolated_string_literal
    : '$' interpolated_regular_string_literal
    | '$' interpolated_verbatim_string_literal
    ;

interpolated_regular_string_literal
    : interpolated_regular_string_whole
    | interpolated_regular_string_start  interpolated_regular_string_literal_body interpolated_regular_string_end
    ;

interpolated_regular_string_literal_body
    : regular_balanced_text
    | interpolated_regular_string_literal_body interpolated_regular_string_mid regular_balanced_text
    ;

interpolated_regular_string_whole
    : '"' interpolated_regular_string_character* '"'
    ;

interpolated_regular_string_start
    : '"' interpolated_regular_string_character* '{'
    ;

interpolated_regular_string_mid
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '{'
    ;

interpolated_regular_string_end
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '"'
    ;

interpolated_regular_string_characters_after_brace
    : interpolated_regular_string_character_no_brace
    | interpolated_regular_string_characters_after_brace interpolated_regular_string_character
    ;

interpolated_regular_string_character
    : single_interpolated_regular_string_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;

interpolated_regular_string_character_no_brace
    : '<Any interpolated_regular_string_character except close_brace_escape_sequence and any hexadecimal_escape_sequence or unicode_escape_sequence designating } (U+007D)>'
    ;

single_interpolated_regular_string_character
    : '<Any character except \" (U+0022), \\ (U+005C), { (U+007B), } (U+007D), and new_line_character>'
    ;

open_brace_escape_sequence
    : '{{'
    ;

close_brace_escape_sequence
    : '}}'
    ;
    
regular_balanced_text
    : regular_balanced_text_part+
    ;

regular_balanced_text_part
    : single_regular_balanced_text_character
    | delimited_comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' regular_balanced_text ')'
    | '[' regular_balanced_text ']'
    | '{' regular_balanced_text '}'
    ;
    
single_regular_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B), } (U+007D) and new_line_character>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
    
interpolation_format
    : interpolation_format_character+
    ;
    
interpolation_format_character
    : '<Any character except \" (U+0022), : (U+003A), { (U+007B) and } (U+007D)>'
    ;
    
interpolated_verbatim_string_literal
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_literal_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_literal_body
    : verbatim_balanced_text
    | interpolated_verbatim_string_literal_body interpolated_verbatim_string_mid verbatim_balanced_text
    ;
    
interpolated_verbatim_string_whole
    : '@"' interpolated_verbatim_string_character* '"'
    ;
    
interpolated_verbatim_string_start
    : '@"' interpolated_verbatim_string_character* '{'
    ;
    
interpolated_verbatim_string_mid
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '{'
    ;
    
interpolated_verbatim_string_end
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '"'
    ;
    
interpolated_verbatim_string_characters_after_brace
    : interpolated_verbatim_string_character_no_brace
    | interpolated_verbatim_string_characters_after_brace interpolated_verbatim_string_character
    ;
    
interpolated_verbatim_string_character
    : single_interpolated_verbatim_string_character
    | quote_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;
    
interpolated_verbatim_string_character_no_brace
    : '<Any interpolated_verbatim_string_character except close_brace_escape_sequence>'
    ;
    
single_interpolated_verbatim_string_character
    : '<Any character except \" (U+0022), { (U+007B) and } (U+007D)>'
    ;
    
verbatim_balanced_text
    : verbatim_balanced_text_part+
    ;

verbatim_balanced_text_part
    : single_verbatim_balanced_text_character
    | comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' verbatim_balanced_text ')'
    | '[' verbatim_balanced_text ']'
    | '{' verbatim_balanced_text '}'
    ;
    
single_verbatim_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B) and } (U+007D)>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
```

<span data-ttu-id="2438f-293">*Interpolated_string_literal*トークンは、 *interpolated_string_literal*で次のように複数のトークンとその他の入力要素として再解釈されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="2438f-294">次の項目は、個別の個別のトークンとして再解釈され*ます。これ*は、先頭の `$` 署名、 *interpolated_regular_string_whole*、 *interpolated_regular_string_start*、 *interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_whole*、 *interpolated_verbatim_string_start、interpolated_verbatim_string_mid、interpolated_verbatim_string_end*です。</span><span class="sxs-lookup"><span data-stu-id="2438f-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="2438f-295">これらの間の*regular_balanced_text*と*verbatim_balanced_text*の発生は*input_section* ([字句分析](lexical-structure.md#lexical-analysis)) として再処理され、結果として得られる入力要素のシーケンスとして再解釈されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="2438f-296">これらには、再解釈される補間文字列リテラルトークンが含まれている場合があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="2438f-297">構文分析は、トークンを*interpolated_string_expression* (挿入[文字列](expressions.md#interpolated-strings)) に再結合します。</span><span class="sxs-lookup"><span data-stu-id="2438f-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="2438f-298">例 TODO</span><span class="sxs-lookup"><span data-stu-id="2438f-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="2438f-299">Null リテラル</span><span class="sxs-lookup"><span data-stu-id="2438f-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="2438f-300">*Null_literal*は、参照型または null 許容型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="2438f-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="2438f-301">Operators と区切り記号</span><span class="sxs-lookup"><span data-stu-id="2438f-301">Operators and punctuators</span></span>

<span data-ttu-id="2438f-302">いくつかの種類の演算子と区切り記号があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="2438f-303">演算子は、1つ以上のオペランドに関係する操作を記述するために、式で使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="2438f-304">たとえば、式 `a + b` では、`+` 演算子を使用して、2つのオペランド `a` および `b`を追加します。</span><span class="sxs-lookup"><span data-stu-id="2438f-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="2438f-305">区切り記号は、グループ化と分離のためのものです。</span><span class="sxs-lookup"><span data-stu-id="2438f-305">Punctuators are for grouping and separating.</span></span>

```antlr
operator_or_punctuator
    : '{'  | '}'  | '['  | ']'  | '('   | ')'  | '.'  | ','  | ':'  | ';'
    | '+'  | '-'  | '*'  | '/'  | '%'   | '&'  | '|'  | '^'  | '!'  | '~'
    | '='  | '<'  | '>'  | '?'  | '??'  | '::' | '++' | '--' | '&&' | '||'
    | '->' | '==' | '!=' | '<=' | '>='  | '+=' | '-=' | '*=' | '/=' | '%='
    | '&=' | '|=' | '^=' | '<<' | '<<=' | '=>'
    ;

right_shift
    : '>>'
    ;

right_shift_assignment
    : '>>='
    ;
```

<span data-ttu-id="2438f-306">*Right_shift*と*right_shift_assignment*の生産の縦棒を使用して、構文文法内の他の生産とは異なり、トークン間ではどのような種類の文字も使用できないことを示します。</span><span class="sxs-lookup"><span data-stu-id="2438f-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="2438f-307">これらの生産は、 *type_parameter_list*s ([型パラメーター](classes.md#type-parameters)) の適切な処理を可能にするために、特別に処理されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="2438f-308">プリプロセスディレクティブ</span><span class="sxs-lookup"><span data-stu-id="2438f-308">Pre-processing directives</span></span>

<span data-ttu-id="2438f-309">プリプロセスディレクティブを使用すると、ソースファイルのセクションを条件付きでスキップしたり、エラーと警告の状態を報告したり、ソースコードの個別の領域を区切ることができます。</span><span class="sxs-lookup"><span data-stu-id="2438f-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="2438f-310">"プリプロセスディレクティブ" という用語は、C およびC++プログラミング言語との一貫性を保つためにのみ使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="2438f-311">でC#は、独立した処理前の手順はありません。プリプロセスディレクティブは、字句解析フェーズの一部として処理されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

```antlr
pp_directive
    : pp_declaration
    | pp_conditional
    | pp_line
    | pp_diagnostic
    | pp_region
    | pp_pragma
    ;
```

<span data-ttu-id="2438f-312">次のプリプロセスディレクティブを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2438f-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="2438f-313">`#define` と `#undef`、それぞれ条件付きコンパイルシンボル ([宣言ディレクティブ](lexical-structure.md#declaration-directives)) を定義および未定義にするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="2438f-314">`#if`、`#elif`、`#else`、および `#endif`。ソースコードのセクションを条件付きでスキップするために使用されます ([条件付きコンパイルディレクティブ](lexical-structure.md#conditional-compilation-directives))。</span><span class="sxs-lookup"><span data-stu-id="2438f-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="2438f-315">`#line`。エラーと警告 ([行ディレクティブ](lexical-structure.md#line-directives)) に対して出力される行番号を制御するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="2438f-316">エラーと警告を発行するために使用される `#error` と `#warning`([診断ディレクティブ](lexical-structure.md#diagnostic-directives))。</span><span class="sxs-lookup"><span data-stu-id="2438f-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="2438f-317">`#region` および `#endregion`。ソースコード ([Region ディレクティブ](lexical-structure.md#region-directives)) のセクションを明示的にマークするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="2438f-318">`#pragma`。コンパイラに省略可能なコンテキスト情報 ([プラグマディレクティブ](lexical-structure.md#pragma-directives)) を指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="2438f-319">プリプロセスディレクティブは、常に個別のソースコード行を占有し、常に `#` 文字とプリプロセスディレクティブ名で始まります。</span><span class="sxs-lookup"><span data-stu-id="2438f-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="2438f-320">`#` 文字の前、および `#` 文字とディレクティブ名の間に空白が出現する場合があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="2438f-321">`#define`、`#undef`、`#if`、`#elif`、`#else`、`#endif`、`#line`、または `#endregion` ディレクティブを含むソース行は、単一行のコメントで終了する場合があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="2438f-322">区切られたコメント (コメントの `/* */` スタイル) は、プリプロセスディレクティブを含むソース行では許可されていません。</span><span class="sxs-lookup"><span data-stu-id="2438f-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="2438f-323">プリプロセスディレクティブは、トークンではなく、のC#構文文法の一部ではありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="2438f-324">ただし、プリプロセスディレクティブを使用して、トークンのシーケンスを含めたり除外したりすることができます。これC#により、プログラムの意味に影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="2438f-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="2438f-325">たとえば、コンパイルされると、プログラムは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2438f-325">For example, when compiled, the program:</span></span>
```csharp
#define A
#undef B

class C
{
#if A
    void F() {}
#else
    void G() {}
#endif

#if B
    void H() {}
#else
    void I() {}
#endif
}
```
<span data-ttu-id="2438f-326">結果として、プログラムとまったく同じトークンのシーケンスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="2438f-327">したがって、構文的には、2つのプログラムはまったく異なっていますが、構文は同じです。</span><span class="sxs-lookup"><span data-stu-id="2438f-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="2438f-328">[条件付きコンパイル シンボル]</span><span class="sxs-lookup"><span data-stu-id="2438f-328">Conditional compilation symbols</span></span>

<span data-ttu-id="2438f-329">`#if`、`#elif`、`#else`、および `#endif` ディレクティブによって提供される条件付きコンパイル機能は、事前処理式 ([処理前の式](lexical-structure.md#pre-processing-expressions)) および条件付きコンパイルシンボルによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="2438f-330">条件付きコンパイルシンボルには、***定義済み***または***未定義***の2つの状態があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="2438f-331">ソースファイルの字句処理の開始時に、条件付きコンパイルシンボルは、外部機構によって明示的に定義されていない限り、未定義になります (コマンドラインコンパイラオプションなど)。</span><span class="sxs-lookup"><span data-stu-id="2438f-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="2438f-332">`#define` ディレクティブが処理されると、そのディレクティブで指定された条件付きコンパイルシンボルが、そのソースファイルで定義されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="2438f-333">シンボルは、同じシンボルの `#undef` ディレクティブが処理されるまで、またはソースファイルの末尾に到達するまで定義されたままになります。</span><span class="sxs-lookup"><span data-stu-id="2438f-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="2438f-334">これは、1つのソースファイル内の `#define` ディレクティブと `#undef` ディレクティブが、同じプログラム内の他のソースファイルに影響を与えないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="2438f-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="2438f-335">プリプロセス式で参照されている場合、定義済みの条件付きコンパイルシンボルには `true`ブール値が含まれ、未定義の条件付きコンパイルシンボルには `false`ブール値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="2438f-336">条件付きコンパイルシンボルは、事前処理式で参照される前に明示的に宣言する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="2438f-337">代わりに、宣言されていないシンボルは単に未定義であるため、`false`値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="2438f-338">条件付きコンパイルシンボルの名前空間は、 C#プログラム内の他のすべての名前付きエンティティとは区別されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="2438f-339">条件付きコンパイルシンボルを参照できるのは、`#define` および `#undef` ディレクティブと、プリプロセス式の中でのみです。</span><span class="sxs-lookup"><span data-stu-id="2438f-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="2438f-340">処理前の式</span><span class="sxs-lookup"><span data-stu-id="2438f-340">Pre-processing expressions</span></span>

<span data-ttu-id="2438f-341">プリプロセス式は `#if` と `#elif` ディレクティブで発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="2438f-342">演算子 `!`、`==`、`!=`、`&&` および `||` は、事前処理式で許可されており、かっこを使用してグループ化することができます。</span><span class="sxs-lookup"><span data-stu-id="2438f-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

```antlr
pp_expression
    : whitespace? pp_or_expression whitespace?
    ;

pp_or_expression
    : pp_and_expression
    | pp_or_expression whitespace? '||' whitespace? pp_and_expression
    ;

pp_and_expression
    : pp_equality_expression
    | pp_and_expression whitespace? '&&' whitespace? pp_equality_expression
    ;

pp_equality_expression
    : pp_unary_expression
    | pp_equality_expression whitespace? '==' whitespace? pp_unary_expression
    | pp_equality_expression whitespace? '!=' whitespace? pp_unary_expression
    ;

pp_unary_expression
    : pp_primary_expression
    | '!' whitespace? pp_unary_expression
    ;

pp_primary_expression
    : 'true'
    | 'false'
    | conditional_symbol
    | '(' whitespace? pp_expression whitespace? ')'
    ;
```

<span data-ttu-id="2438f-343">プリプロセス式で参照されている場合、定義済みの条件付きコンパイルシンボルには `true`ブール値が含まれ、未定義の条件付きコンパイルシンボルには `false`ブール値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="2438f-344">プリプロセス式の評価では、常にブール値が生成します。</span><span class="sxs-lookup"><span data-stu-id="2438f-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="2438f-345">プリプロセス式の評価の規則は、参照できるユーザー定義エンティティが条件付きコンパイルシンボルだけである点を除いて、定数式 ([定数](expressions.md#constant-expressions)式) の評価ルールと同じです。</span><span class="sxs-lookup"><span data-stu-id="2438f-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="2438f-346">宣言ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="2438f-346">Declaration directives</span></span>

<span data-ttu-id="2438f-347">宣言ディレクティブは、条件付きコンパイルシンボルを定義または未定義にするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="2438f-348">`#define` ディレクティブを処理すると、指定された条件付きコンパイルシンボルが定義されます。これは、ディレクティブの後のソース行から始まります。</span><span class="sxs-lookup"><span data-stu-id="2438f-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="2438f-349">同様に、`#undef` ディレクティブを処理すると、指定された条件付きコンパイルシンボルが未定義になります。これは、ディレクティブの後のソース行から始まります。</span><span class="sxs-lookup"><span data-stu-id="2438f-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="2438f-350">`#define`ソースファイル`#undef`内のディレクティブとディレクティブは、ソースファイル内の最初の*トークン* ([トークン](lexical-structure.md#tokens)) の前に実行する必要があります。それ以外の場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="2438f-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="2438f-351">直感的に言うと、`#define` および `#undef` ディレクティブは、ソースファイル内の "実際のコード" の前に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="2438f-352">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2438f-352">The example:</span></span>
```csharp
#define Enterprise

#if Professional || Enterprise
    #define Advanced
#endif

namespace Megacorp.Data
{
    #if Advanced
    class PivotTable {...}
    #endif
}
```
<span data-ttu-id="2438f-353">は、ソースファイル内の最初のトークン (`namespace` キーワード) の前に `#define` ディレクティブがあるため、有効です。</span><span class="sxs-lookup"><span data-stu-id="2438f-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="2438f-354">次の例では、`#define` が実際のコードに従うため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="2438f-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
```csharp
#define A
namespace N
{
    #define B
    #if B
    class Class1 {}
    #endif
}
```

<span data-ttu-id="2438f-355">`#define` は、既に定義されている条件付きコンパイルシンボルを定義できますが、そのシンボルに対して介在する `#undef` はありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="2438f-356">次の例では、`A` 条件付きコンパイルシンボルを定義してから、再度定義します。</span><span class="sxs-lookup"><span data-stu-id="2438f-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="2438f-357">`#undef` は、定義されていない条件付きコンパイルシンボルを "未定義" にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="2438f-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="2438f-358">次の例では、`A` 条件付きコンパイルシンボルを定義し、そのシンボルを2回未定義にします。2番目の `#undef` は無効ですが、まだ有効ではありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="2438f-359">条件付きコンパイルディレクティブ</span><span class="sxs-lookup"><span data-stu-id="2438f-359">Conditional compilation directives</span></span>

<span data-ttu-id="2438f-360">条件付きコンパイルディレクティブは、ソースファイルの一部を条件付きで含めたり除外したりするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

```antlr
pp_conditional
    : pp_if_section pp_elif_section* pp_else_section? pp_endif
    ;

pp_if_section
    : whitespace? '#' whitespace? 'if' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_elif_section
    : whitespace? '#' whitespace? 'elif' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_else_section:
    | whitespace? '#' whitespace? 'else' pp_new_line conditional_section?
    ;

pp_endif
    : whitespace? '#' whitespace? 'endif' pp_new_line
    ;

conditional_section
    : input_section
    | skipped_section
    ;

skipped_section
    : skipped_section_part+
    ;

skipped_section_part
    : skipped_characters? new_line
    | pp_directive
    ;

skipped_characters
    : whitespace? not_number_sign input_character*
    ;

not_number_sign
    : '<Any input_character except #>'
    ;
```

<span data-ttu-id="2438f-361">構文で示されているように、条件付きコンパイルディレクティブは、`#if` ディレクティブ、0個以上の `#elif` ディレクティブ、0個または1個の `#else` ディレクティブ、および `#endif` ディレクティブで構成されるセットとして記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="2438f-362">ディレクティブは、ソースコードの条件付きセクションです。</span><span class="sxs-lookup"><span data-stu-id="2438f-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="2438f-363">各セクションは、直前のディレクティブによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="2438f-364">条件付きセクションには、これらのディレクティブが完全なセットを形成する、入れ子になった条件付きコンパイルディレクティブが含まれている場合があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="2438f-365">*Pp_conditional*は、通常の字句処理で、含まれている*conditional_section*s のうち最大1つを選択します。</span><span class="sxs-lookup"><span data-stu-id="2438f-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="2438f-366">`#if` ディレクティブと `#elif` ディレクティブの*pp_expression*のは、1つが `true`を生成するまで順番に評価されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="2438f-367">式によって `true`が生成された場合は、対応するディレクティブの*conditional_section*が選択されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="2438f-368">すべての*pp_expression*が `false`であり、`#else` ディレクティブが存在する場合は、`#else` ディレクティブの*conditional_section*が選択されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="2438f-369">それ以外の場合、 *conditional_section*は選択されません。</span><span class="sxs-lookup"><span data-stu-id="2438f-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="2438f-370">選択した*conditional_section*(存在する場合) は、通常の*input_section*として処理されます。セクションに含まれているソースコードは、字句文法に従う必要があります。トークンは、セクションのソースコードから生成されます。また、セクションのプリプロセスディレクティブには、所定の効果があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="2438f-371">残りの*conditional_section*s (存在する場合) は*skipped_section*s として処理されます。ただし、プリプロセスディレクティブの場合を除き、セクション内のソースコードは字句文法に準拠している必要はありません。セクションのソースコードからトークンは生成されません。また、セクションのプリプロセスディレクティブは構文的に正しい必要がありますが、それ以外の場合は処理されません。</span><span class="sxs-lookup"><span data-stu-id="2438f-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="2438f-372">*Skipped_section*として処理されている*conditional_section*内では、入れ子になった*conditional_section*s (入れ子になった `#if`...`#endif` と `#region`...`#endregion` コンストラクトに含まれています) も*skipped_section*s として処理されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="2438f-373">次の例は、条件付きコンパイルディレクティブを入れ子にする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="2438f-373">The following example illustrates how conditional compilation directives can nest:</span></span>
```csharp
#define Debug       // Debugging on
#undef Trace        // Tracing off

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
            #if Trace
                WriteToLog(this.ToString());
            #endif
        #endif
        CommitHelper();
    }
}
```

<span data-ttu-id="2438f-374">プリプロセスディレクティブを除き、スキップされたソースコードは字句解析の対象になりません。</span><span class="sxs-lookup"><span data-stu-id="2438f-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="2438f-375">たとえば、次のコードは、`#else` セクションの未終了のコメントにかかわらず有効です。</span><span class="sxs-lookup"><span data-stu-id="2438f-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
```csharp
#define Debug        // Debugging on

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
        #else
            /* Do something else
        #endif
    }
}
```

<span data-ttu-id="2438f-376">ただし、ソースコードのスキップされたセクションでも、プリプロセスディレクティブは構文的に正しい必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2438f-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="2438f-377">プリプロセスディレクティブは、複数行の入力要素内に出現する場合は処理されません。</span><span class="sxs-lookup"><span data-stu-id="2438f-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="2438f-378">たとえば、次のようなプログラムがあるとします。</span><span class="sxs-lookup"><span data-stu-id="2438f-378">For example, the program:</span></span>
```csharp
class Hello
{
    static void Main() {
        System.Console.WriteLine(@"hello, 
#if Debug
        world
#else
        Nebraska
#endif
        ");
    }
}
```
<span data-ttu-id="2438f-379">結果は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2438f-379">results in the output:</span></span>
```console
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="2438f-380">特別なケースでは、処理される前処理ディレクティブのセットは、 *pp_expression*の評価に依存する場合があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="2438f-381">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2438f-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="2438f-382">では、`X` が定義されているかどうかに関係なく、常に同じトークンストリーム (`class` `Q` `{` `}`) が生成されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="2438f-383">`X` が定義されている場合、複数行のコメントにより、処理されたディレクティブは `#if` と `#endif`だけです。</span><span class="sxs-lookup"><span data-stu-id="2438f-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="2438f-384">`X` が定義されていない場合、3つのディレクティブ (`#if`、`#else`、`#endif`) がディレクティブセットの一部になります。</span><span class="sxs-lookup"><span data-stu-id="2438f-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="2438f-385">診断ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="2438f-385">Diagnostic directives</span></span>

<span data-ttu-id="2438f-386">診断ディレクティブは、エラーメッセージと警告メッセージを明示的に生成するために使用されます。このメッセージは、他のコンパイル時のエラーや警告と同じ方法で報告されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

```antlr
pp_diagnostic
    : whitespace? '#' whitespace? 'error' pp_message
    | whitespace? '#' whitespace? 'warning' pp_message
    ;

pp_message
    : new_line
    | whitespace input_character* new_line
    ;
```

<span data-ttu-id="2438f-387">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2438f-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="2438f-388">は常に警告 ("チェックイン前に必要なコードレビュー") を生成し、コンパイル時エラーを生成します ("ビルドはデバッグとリテールの両方にすることはできません")。条件付きシンボル `Debug` および `Retail` が両方とも定義されている場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="2438f-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="2438f-389">*Pp_message*には任意のテキストを含めることができることに注意してください。具体的には、`can't`語の単一引用符で示されているように、適切な形式のトークンが含まれている必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="2438f-390">Region ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="2438f-390">Region directives</span></span>

<span data-ttu-id="2438f-391">リージョンディレクティブは、ソースコードの領域を明示的にマークするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-391">The region directives are used to explicitly mark regions of source code.</span></span>

```antlr
pp_region
    : pp_start_region conditional_section? pp_end_region
    ;

pp_start_region
    : whitespace? '#' whitespace? 'region' pp_message
    ;

pp_end_region
    : whitespace? '#' whitespace? 'endregion' pp_message
    ;
```

<span data-ttu-id="2438f-392">領域には意味の意味がありません。地域は、プログラマが使用するためのものであり、ソースコードのセクションをマークする自動化ツールによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="2438f-393">`#region` または `#endregion` ディレクティブで指定されているメッセージは、同様に意味がありません。リージョンを識別するためにのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="2438f-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="2438f-394">一致する `#region` ディレクティブと `#endregion` ディレクティブには*pp_message*が異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="2438f-395">領域の字句処理は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2438f-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="2438f-396">は、次の形式の条件付きコンパイルディレクティブの字句処理に正確に対応します。</span><span class="sxs-lookup"><span data-stu-id="2438f-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="2438f-397">行ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="2438f-397">Line directives</span></span>

<span data-ttu-id="2438f-398">行ディレクティブを使用すると、コンパイラによって警告やエラーなどの出力で報告され、呼び出し元情報属性 ([呼び出し元情報属性](attributes.md#caller-info-attributes)) で使用される行番号とソースファイル名を変更できます。</span><span class="sxs-lookup"><span data-stu-id="2438f-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="2438f-399">行ディレクティブは、他のテキスト入力からソースコードを生成C#するメタプログラミングツールで最もよく使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

```antlr
pp_line
    : whitespace? '#' whitespace? 'line' whitespace line_indicator pp_new_line
    ;

line_indicator
    : decimal_digit+ whitespace file_name
    | decimal_digit+
    | 'default'
    | 'hidden'
    ;

file_name
    : '"' file_name_character+ '"'
    ;

file_name_character
    : '<Any input_character except ">'
    ;
```

<span data-ttu-id="2438f-400">`#line` ディレクティブが存在しない場合、コンパイラは出力に実際の行番号とソースファイル名を報告します。</span><span class="sxs-lookup"><span data-stu-id="2438f-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="2438f-401">`default`ない*line_indicator*を含む `#line` ディレクティブを処理する場合、コンパイラは、ディレクティブの後の行を、指定された行番号 (および指定されている場合はファイル名) として処理します。</span><span class="sxs-lookup"><span data-stu-id="2438f-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="2438f-402">`#line default` ディレクティブは、前のすべての #line ディレクティブの効果を反転させます。</span><span class="sxs-lookup"><span data-stu-id="2438f-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="2438f-403">コンパイラは、`#line` ディレクティブが処理されていないかのように、以降の行の真の行情報を正確に報告します。</span><span class="sxs-lookup"><span data-stu-id="2438f-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="2438f-404">`#line hidden` ディレクティブは、エラーメッセージに報告されたファイルと行番号には影響しませんが、ソースレベルのデバッグには影響します。</span><span class="sxs-lookup"><span data-stu-id="2438f-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="2438f-405">デバッグ時に、`#line hidden` ディレクティブとそれ以降の `#line` ディレクティブの間のすべての行に `#line hidden`は、行番号情報がありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="2438f-406">デバッガーでコードをステップ実行すると、これらの行は完全にスキップされます。</span><span class="sxs-lookup"><span data-stu-id="2438f-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="2438f-407">なお、 *file_name*されるエスケープ文字は処理されません通常の文字列リテラルと異なります、"`\`"文字が単純に内での通常のバック スラッシュ文字を指定、 *file_name*。</span><span class="sxs-lookup"><span data-stu-id="2438f-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="2438f-408">プラグマディレクティブ</span><span class="sxs-lookup"><span data-stu-id="2438f-408">Pragma directives</span></span>

<span data-ttu-id="2438f-409">`#pragma` 前処理ディレクティブは、コンパイラに対して省略可能なコンテキスト情報を指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="2438f-410">`#pragma` ディレクティブで指定された情報は、プログラムのセマンティクスを変更することはありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="2438f-411">C#コンパイラの警告を制御する `#pragma` ディレクティブを提供します。</span><span class="sxs-lookup"><span data-stu-id="2438f-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="2438f-412">言語の将来のバージョンには、追加の `#pragma` ディレクティブが含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2438f-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="2438f-413">他のC#コンパイラとの相互運用性を確保C#するために、Microsoft コンパイラは不明な `#pragma` ディレクティブに対してコンパイルエラーを発行しません。ただし、このようなディレクティブは警告を生成します。</span><span class="sxs-lookup"><span data-stu-id="2438f-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="2438f-414">プラグマの警告</span><span class="sxs-lookup"><span data-stu-id="2438f-414">Pragma warning</span></span>

<span data-ttu-id="2438f-415">`#pragma warning` ディレクティブは、後続のプログラムテキストのコンパイル時に、すべてまたは特定の警告メッセージのセットを無効化または復元するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2438f-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

```antlr
pragma_warning_body
    : 'warning' whitespace warning_action
    | 'warning' whitespace warning_action whitespace warning_list
    ;

warning_action
    : 'disable'
    | 'restore'
    ;

warning_list
    : decimal_digit+ (whitespace? ',' whitespace? decimal_digit+)*
    ;
```

<span data-ttu-id="2438f-416">警告リストを省略する `#pragma warning` ディレクティブは、すべての警告に影響します。</span><span class="sxs-lookup"><span data-stu-id="2438f-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="2438f-417">警告リストを含む `#pragma warning` ディレクティブは、一覧に指定されている警告のみに影響します。</span><span class="sxs-lookup"><span data-stu-id="2438f-417">A `#pragma warning` directive that includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="2438f-418">`#pragma warning disable` ディレクティブは、すべてまたは特定の警告のセットを無効にします。</span><span class="sxs-lookup"><span data-stu-id="2438f-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="2438f-419">`#pragma warning restore` ディレクティブは、すべてまたは特定の警告のセットを、コンパイル単位の開始時に有効だった状態に復元します。</span><span class="sxs-lookup"><span data-stu-id="2438f-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="2438f-420">特定の警告が外部で無効にされた場合は、`#pragma warning restore` (すべての警告または特定の警告について) でその警告が再度有効になることはありません。</span><span class="sxs-lookup"><span data-stu-id="2438f-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="2438f-421">次の例では、`#pragma warning` を使用して、Microsoft C#コンパイラの警告番号を使用して、古いメンバーが参照されたときに報告される警告を一時的に無効にします。</span><span class="sxs-lookup"><span data-stu-id="2438f-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
```csharp
using System;

class Program
{
    [Obsolete]
    static void Foo() {}

    static void Main() {
#pragma warning disable 612
    Foo();
#pragma warning restore 612
    }
}
```
