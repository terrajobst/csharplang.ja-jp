---
ms.openlocfilehash: 7f7abb120d0b3a6abf12beb9daa0d79a975ccce2
ms.sourcegitcommit: 4e3f2e4ea5a50b186b08d1e93d3ffcdb3754596e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2019
ms.locfileid: "56411313"
---
# <a name="lexical-structure"></a><span data-ttu-id="96212-101">字句構造</span><span class="sxs-lookup"><span data-stu-id="96212-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="96212-102">Programs</span><span class="sxs-lookup"><span data-stu-id="96212-102">Programs</span></span>

<span data-ttu-id="96212-103">C#***プログラム***1 つまたは複数から成る***ソース ファイル***正式と呼ばれる、***コンパイル単位***([コンパイル単位](namespaces.md#compilation-units))。</span><span class="sxs-lookup"><span data-stu-id="96212-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="96212-104">ソース ファイルは、Unicode 文字の順序付けられたシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="96212-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="96212-105">ソース ファイルでは、ファイル システムで、ファイルと一対一の対応を通常されましたが、この対応は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="96212-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="96212-106">最大の移植性、お勧めファイル システム内のファイルが、utf-8 でエンコードするエンコーディングします。</span><span class="sxs-lookup"><span data-stu-id="96212-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="96212-107">概念的には、3 つの手順を使用してプログラムのコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="96212-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="96212-108">変換ファイルを特定の文字のレパートリーとエンコード体系を Unicode 文字のシーケンスに変換します。</span><span class="sxs-lookup"><span data-stu-id="96212-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="96212-109">字句解析、トークンのストリームに Unicode の入力文字のストリームを変換します。</span><span class="sxs-lookup"><span data-stu-id="96212-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="96212-110">構文解析、実行可能コードにトークンのストリームを変換します。</span><span class="sxs-lookup"><span data-stu-id="96212-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="96212-111">文法</span><span class="sxs-lookup"><span data-stu-id="96212-111">Grammars</span></span>

<span data-ttu-id="96212-112">この仕様は、C# プログラミング言語の 2 つの文法を使用して構文を表示します。</span><span class="sxs-lookup"><span data-stu-id="96212-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="96212-113">***字句文法***([字句文法](lexical-structure.md#lexical-grammar)) 行終端記号、空白、コメント、トークン、およびプリプロセッサ ディレクティブに Unicode 文字を結合する方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="96212-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="96212-114">***構文文法***([構文文法](lexical-structure.md#syntactic-grammar)) 字句文法に起因するトークンを組み合わせて C# プログラムを構成する方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="96212-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="96212-115">文法の表記</span><span class="sxs-lookup"><span data-stu-id="96212-115">Grammar notation</span></span>

<span data-ttu-id="96212-116">字句および構文の文法は、ANTLR 文章校正ツールの表記を使用してバッカスナウア記法の形式で表示されます。</span><span class="sxs-lookup"><span data-stu-id="96212-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="96212-117">字句文法</span><span class="sxs-lookup"><span data-stu-id="96212-117">Lexical grammar</span></span>

<span data-ttu-id="96212-118">C# の構文文法を示した[字句解析](lexical-structure.md#lexical-analysis)、[トークン](lexical-structure.md#tokens)、および[プリプロセッサ ディレクティブ](lexical-structure.md#pre-processing-directives)します。</span><span class="sxs-lookup"><span data-stu-id="96212-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="96212-119">字句文法の終端記号は、Unicode 文字セットの文字と構文の文法では、トークンを構成する文字を集計する方法を指定します ([トークン](lexical-structure.md#tokens))、空白文字 ([空白](lexical-structure.md#white-space))、コメント ([コメント](lexical-structure.md#comments))、およびプリプロセッサ ディレクティブ ([プリプロセッサ ディレクティブ](lexical-structure.md#pre-processing-directives))。</span><span class="sxs-lookup"><span data-stu-id="96212-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="96212-120">C# プログラムですべてのソース ファイルに準拠する必要があります、*入力*字句文法の実稼働 ([字句解析](lexical-structure.md#lexical-analysis))。</span><span class="sxs-lookup"><span data-stu-id="96212-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="96212-121">構文文法</span><span class="sxs-lookup"><span data-stu-id="96212-121">Syntactic grammar</span></span>

<span data-ttu-id="96212-122">この章に従います付録では、C# の構文の文法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="96212-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="96212-123">構文文法のターミナル シンボルは、構文の文法で定義されているトークンと構文の文法では、トークンを組み合わせて C# プログラムを構成する方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="96212-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="96212-124">すべてのソース ファイルで、C#にプログラムが準拠する必要があります、 *compilation_unit*構文文法の実稼働 ([コンパイル単位](namespaces.md#compilation-units))。</span><span class="sxs-lookup"><span data-stu-id="96212-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="96212-125">字句解析</span><span class="sxs-lookup"><span data-stu-id="96212-125">Lexical analysis</span></span>

<span data-ttu-id="96212-126">*入力*運用 C# ソース ファイルの構文構造を定義します。</span><span class="sxs-lookup"><span data-stu-id="96212-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="96212-127">C# プログラムでは、各ソース ファイルは、この構文の文法の実稼働環境に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="96212-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

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

<span data-ttu-id="96212-128">5 つの基本的な要素は、C# ソース ファイルの構文構造を構成します。行ターミネータ ([行ターミネータ](lexical-structure.md#line-terminators))、空白文字 ([空白](lexical-structure.md#white-space))、コメント ([コメント](lexical-structure.md#comments))、トークン ([トークン](lexical-structure.md#tokens))、およびプリプロセッサ ディレクティブ ([プリプロセッサ ディレクティブ](lexical-structure.md#pre-processing-directives))。</span><span class="sxs-lookup"><span data-stu-id="96212-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="96212-129">これらの基本要素のトークンだけが、C# プログラムの構文の文法で重要です ([構文文法](lexical-structure.md#syntactic-grammar))。</span><span class="sxs-lookup"><span data-stu-id="96212-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="96212-130">C# ソース ファイルの構文の処理、構文分析への入力になるトークンのシーケンスに、ファイルを減らすことで構成されます。</span><span class="sxs-lookup"><span data-stu-id="96212-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="96212-131">トークンを区切る行の終端記号、空白文字、およびコメントを使用できるとプリプロセッサ ディレクティブがスキップされるソース ファイルのセクションが発生することができますが、それ以外の場合これらの構文要素ありません影響 C# プログラムの構文構造に。</span><span class="sxs-lookup"><span data-stu-id="96212-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="96212-132">補間文字列リテラルの場合 ([リテラル文字列の補間](lexical-structure.md#interpolated-string-literals)) 1 つのトークンの字句解析、によって最初に生成されるが、繰り返しの字句解析を対象となるいくつかの入力要素に分割されますまで、すべての補間文字列リテラルが解決されました。</span><span class="sxs-lookup"><span data-stu-id="96212-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="96212-133">結果として得られるトークンは、構文分析への入力として使用します。</span><span class="sxs-lookup"><span data-stu-id="96212-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="96212-134">いくつかの構文文法には、ソース ファイル内の文字のシーケンスが一致すると、最も長い可能な構文要素を形成字句処理は常にします。</span><span class="sxs-lookup"><span data-stu-id="96212-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="96212-135">たとえば、文字シーケンス`//`が、その構文の要素が 1 つを超えるため、単一行コメントの始まりとして処理される`/`トークンです。</span><span class="sxs-lookup"><span data-stu-id="96212-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="96212-136">行ターミネータ</span><span class="sxs-lookup"><span data-stu-id="96212-136">Line terminators</span></span>

<span data-ttu-id="96212-137">行ターミネータは、C# ソース ファイルの文字を行に分割します。</span><span class="sxs-lookup"><span data-stu-id="96212-137">Line terminators divide the characters of a C# source file into lines.</span></span>

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

<span data-ttu-id="96212-138">ソースとの互換性をコード ファイルの終わりのマーカーを追加している編集ツールとのシーケンスとして正しく表示するファイルをソースを有効にするには、行を終了した、C# プログラムでソース ファイルごとに順番に、次の変換が適用します。</span><span class="sxs-lookup"><span data-stu-id="96212-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="96212-139">ソース ファイルの最後の文字がコントロール Z の文字 (`U+001A`)、この文字を削除します。</span><span class="sxs-lookup"><span data-stu-id="96212-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="96212-140">復帰文字 (`U+000D`) とそのソース ファイルが空でない場合は、ソース ファイルの最後の文字は、キャリッジ リターンがない場合は、ソース ファイルの末尾に追加されます (`U+000D`)、ライン フィード (`U+000A`)、行区切り記号 (`U+2028`)、または段落区切り記号 (`U+2029`)。</span><span class="sxs-lookup"><span data-stu-id="96212-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="96212-141">コメント</span><span class="sxs-lookup"><span data-stu-id="96212-141">Comments</span></span>

<span data-ttu-id="96212-142">コメントの 2 つの形式がサポートされています。 単一行コメントおよび区切り記号付きのコメント。</span><span class="sxs-lookup"><span data-stu-id="96212-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="96212-143">***単一行コメント***最初の文字`//`およびソース行の末尾に拡張します。</span><span class="sxs-lookup"><span data-stu-id="96212-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="96212-144">***コメントの区切り***最初の文字`/*`文字で終わる`*/`します。</span><span class="sxs-lookup"><span data-stu-id="96212-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="96212-145">区切り記号付きコメントは、複数行にわたる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="96212-145">Delimited comments may span multiple lines.</span></span>

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

<span data-ttu-id="96212-146">コメントを入れ子にしないでください。</span><span class="sxs-lookup"><span data-stu-id="96212-146">Comments do not nest.</span></span> <span data-ttu-id="96212-147">文字シーケンス`/*`と`*/`内で特別な意味があるない、`//`コメント、および文字のシーケンス`//`と`/*`区切られたコメントの内部で特別な意味があるありません。</span><span class="sxs-lookup"><span data-stu-id="96212-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="96212-148">コメントは、文字と文字列リテラル内では処理されません。</span><span class="sxs-lookup"><span data-stu-id="96212-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="96212-149">例では、</span><span class="sxs-lookup"><span data-stu-id="96212-149">The example</span></span>
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
<span data-ttu-id="96212-150">区切り記号付きのコメントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="96212-150">includes a delimited comment.</span></span>

<span data-ttu-id="96212-151">例では、</span><span class="sxs-lookup"><span data-stu-id="96212-151">The example</span></span>
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
<span data-ttu-id="96212-152">いくつかの単一行コメントを示しています。</span><span class="sxs-lookup"><span data-stu-id="96212-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="96212-153">空白</span><span class="sxs-lookup"><span data-stu-id="96212-153">White space</span></span>

<span data-ttu-id="96212-154">空白文字は Unicode クラス (Zs の空白文字を含む) を使用して任意の文字だけでなく、水平タブ文字、垂直タブ文字、およびフォーム フィード文字として定義されます。</span><span class="sxs-lookup"><span data-stu-id="96212-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="96212-155">トークン</span><span class="sxs-lookup"><span data-stu-id="96212-155">Tokens</span></span>

<span data-ttu-id="96212-156">トークンのいくつかの種類があります。 識別子、キーワード、リテラル、演算子、および区切り記号。</span><span class="sxs-lookup"><span data-stu-id="96212-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="96212-157">空白とコメントは、トークンが、トークンの区切り記号として。</span><span class="sxs-lookup"><span data-stu-id="96212-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

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

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="96212-158">Unicode 文字のエスケープ シーケンス</span><span class="sxs-lookup"><span data-stu-id="96212-158">Unicode character escape sequences</span></span>

<span data-ttu-id="96212-159">Unicode 文字のエスケープ シーケンスは、Unicode 文字を表します。</span><span class="sxs-lookup"><span data-stu-id="96212-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="96212-160">Unicode 文字のエスケープ シーケンスは、識別子で処理されます ([識別子](lexical-structure.md#identifiers))、文字リテラル ([文字リテラル](lexical-structure.md#character-literals))、および通常の文字列リテラル ([リテラル文字列](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="96212-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="96212-161">Unicode 文字のエスケープは、その他の場所 (たとえば、演算子などの区切り記号、またはキーワードを形成する) では処理されません。</span><span class="sxs-lookup"><span data-stu-id="96212-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="96212-162">Unicode エスケープ シーケンスは、16 進数の数値以下で構成される 1 つの Unicode 文字を表す、"`\u`「または」`\U`"文字です。</span><span class="sxs-lookup"><span data-stu-id="96212-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="96212-163">C# 文字と文字列値では Unicode コード ポイントの 16 ビットのエンコーディングを使用するため、u+10000 U + 10 ffff の範囲の Unicode 文字は文字リテラルでは許可されていませんし、Unicode サロゲート ペアを使用して、文字列リテラルで表されます。</span><span class="sxs-lookup"><span data-stu-id="96212-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="96212-164">0x10ffff まで上記のコード ポイントを使用した Unicode 文字がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="96212-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="96212-165">複数の翻訳は行われません。</span><span class="sxs-lookup"><span data-stu-id="96212-165">Multiple translations are not performed.</span></span> <span data-ttu-id="96212-166">たとえば、文字列リテラル"`\u005Cu005C`「は等価である」`\u005C`「なく」`\`"。</span><span class="sxs-lookup"><span data-stu-id="96212-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="96212-167">Unicode 値`\u005C`文字"`\`"。</span><span class="sxs-lookup"><span data-stu-id="96212-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="96212-168">例では、</span><span class="sxs-lookup"><span data-stu-id="96212-168">The example</span></span>
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
<span data-ttu-id="96212-169">いくつか示されて`\u0066`、これは、文字のエスケープ シーケンス"`f`"。</span><span class="sxs-lookup"><span data-stu-id="96212-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="96212-170">プログラムと同じです。</span><span class="sxs-lookup"><span data-stu-id="96212-170">The program is equivalent to</span></span>
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

### <a name="identifiers"></a><span data-ttu-id="96212-171">識別子</span><span class="sxs-lookup"><span data-stu-id="96212-171">Identifiers</span></span>

<span data-ttu-id="96212-172">このセクションで指定された識別子の規則は、点を除いて、アンダー スコアが Unicode のエスケープ シーケンスは、(同様に、C プログラミング言語で従来は) 初期文字として許可されると正確に、Unicode Standard Annex 31 で、推奨される対応します。識別子で許可されていると、"`@`"を識別子として使用するキーワードを有効にプレフィックスとして使用できる文字。</span><span class="sxs-lookup"><span data-stu-id="96212-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

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

<span data-ttu-id="96212-173">上記で説明した Unicode 文字クラスについては、Unicode 標準、バージョン 3.0、4.5 のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="96212-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="96212-174">有効な識別子の例として、"`identifier1`「,」`_identifier2`"、および"`@if`"。</span><span class="sxs-lookup"><span data-stu-id="96212-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="96212-175">準拠したプログラムで識別子は、Unicode Standard Annex 15 で定義されている Unicode 正規形 C で定義された正規の形式でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="96212-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="96212-176">正規形 C ではなく識別子が検出されたときの動作は実装で定義されます。ただし、診断は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="96212-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="96212-177">プレフィックス"`@`"他のプログラミング言語と接続した場合に便利です、識別子としてのキーワードの使用を有効にします。</span><span class="sxs-lookup"><span data-stu-id="96212-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="96212-178">文字`@`他の言語では、プレフィックスなしの通常の識別子として認識されるため、識別子の一部ではありません。</span><span class="sxs-lookup"><span data-stu-id="96212-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="96212-179">識別子、`@`プレフィックスと呼ばれる、***逐語的識別子***します。</span><span class="sxs-lookup"><span data-stu-id="96212-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="96212-180">使用、`@`キーワードではない識別子のプレフィックスは許可されていますが、スタイルの問題としてお勧めします。</span><span class="sxs-lookup"><span data-stu-id="96212-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="96212-181">例:</span><span class="sxs-lookup"><span data-stu-id="96212-181">The example:</span></span>
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
<span data-ttu-id="96212-182">という名前のクラスを定義します。"`class`という名前の静的メソッド"と"`static`"をという名前のパラメーターを受け取る"`bool`"。</span><span class="sxs-lookup"><span data-stu-id="96212-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="96212-183">Unicode エスケープするため、トークンのキーワードでは使用できませんに注意してください"`cl\u0061ss`「識別子、およびと同じ識別子が」`@class`"。</span><span class="sxs-lookup"><span data-stu-id="96212-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="96212-184">順序で、次の変換が適用される結果が同一の場合は、2 つの識別子は同じで考えられます。</span><span class="sxs-lookup"><span data-stu-id="96212-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="96212-185">プレフィックス"`@`"を使用する場合は、削除されます。</span><span class="sxs-lookup"><span data-stu-id="96212-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="96212-186">各*unicode_escape_sequence*対応する Unicode 文字に変換されます。</span><span class="sxs-lookup"><span data-stu-id="96212-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="96212-187">すべて*formatting_character*s に削除されます。</span><span class="sxs-lookup"><span data-stu-id="96212-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="96212-188">識別子を含む 2 つの連続するアンダー スコア文字 (`U+005F`) 実装で使用するために予約されています。</span><span class="sxs-lookup"><span data-stu-id="96212-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="96212-189">たとえば、実装は、2 つのアンダー スコアで始まる拡張のキーワード。</span><span class="sxs-lookup"><span data-stu-id="96212-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="96212-190">キーワード</span><span class="sxs-lookup"><span data-stu-id="96212-190">Keywords</span></span>

<span data-ttu-id="96212-191">A***キーワード***識別子のような文字シーケンスは、予約されていると、前に付けない限り識別子として使用できませんが、`@`文字。</span><span class="sxs-lookup"><span data-stu-id="96212-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

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

<span data-ttu-id="96212-192">文法でいくつかの場所では、特定の識別子は、特殊な意味があるが、キーワードではありません。</span><span class="sxs-lookup"><span data-stu-id="96212-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="96212-193">このような識別子は、「コンテキスト キーワード」とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="96212-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="96212-194">プロパティの宣言内など、"`get`「と」`set`"識別子が特別な意味を持ちます ([アクセサー](classes.md#accessors))。</span><span class="sxs-lookup"><span data-stu-id="96212-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="96212-195">以外の識別子`get`または`set`識別子としてこれらの単語の使用量がこのような使用が競合しないようにこれらの場所では許可されません。</span><span class="sxs-lookup"><span data-stu-id="96212-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="96212-196">それ以外の場合によっては、識別子と"`var`"暗黙的に型指定されたローカル変数宣言で ([ローカル変数宣言](statements.md#local-variable-declarations))、コンテキスト キーワードは宣言された名前と競合することです。</span><span class="sxs-lookup"><span data-stu-id="96212-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="96212-197">このような場合は、宣言された名前は、コンテキスト キーワードとして識別子の使用に優先します。</span><span class="sxs-lookup"><span data-stu-id="96212-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="96212-198">リテラル</span><span class="sxs-lookup"><span data-stu-id="96212-198">Literals</span></span>

<span data-ttu-id="96212-199">A***リテラル***値のソース コードの表現です。</span><span class="sxs-lookup"><span data-stu-id="96212-199">A ***literal*** is a source code representation of a value.</span></span>

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

#### <a name="boolean-literals"></a><span data-ttu-id="96212-200">ブール型リテラル</span><span class="sxs-lookup"><span data-stu-id="96212-200">Boolean literals</span></span>

<span data-ttu-id="96212-201">2 つのブール型リテラル値がある:`true`と`false`します。</span><span class="sxs-lookup"><span data-stu-id="96212-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="96212-202">型を*boolean_literal*は`bool`します。</span><span class="sxs-lookup"><span data-stu-id="96212-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="96212-203">整数リテラル</span><span class="sxs-lookup"><span data-stu-id="96212-203">Integer literals</span></span>

<span data-ttu-id="96212-204">整数リテラルの型の値を記述に使用`int`、 `uint`、 `long`、および`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="96212-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="96212-205">整数リテラルがある 2 つの可能な形式: 10 進数と 16 進数。</span><span class="sxs-lookup"><span data-stu-id="96212-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

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

<span data-ttu-id="96212-206">整数リテラルの型は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="96212-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="96212-207">リテラルにサフィックスがあるない場合は、最初の値を表す型。 `int`、 `uint`、 `long`、`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="96212-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="96212-208">リテラルが付いている場合`U`または`u`、最初の値を表す型があります: `uint`、`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="96212-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="96212-209">リテラルが付いている場合`L`または`l`、最初の値を表す型があります: `long`、`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="96212-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="96212-210">リテラルが付いている場合`UL`、 `Ul`、 `uL`、 `ul`、 `LU`、 `Lu`、 `lU`、または`lu`、型である`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="96212-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="96212-211">範囲の整数リテラルで表される値が、`ulong`入力すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="96212-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="96212-212">スタイルの問題、としては推奨される"`L`「の代わりに使用する」`l`"型のリテラルの書き込み時に`long`文字を混同しないでくださいであるため、"`l`数字の"と"`1`"。</span><span class="sxs-lookup"><span data-stu-id="96212-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="96212-213">値の最小値を許可するように`int`と`long`、10 進整数リテラルとして記述する、次の 2 つの規則が存在する値。</span><span class="sxs-lookup"><span data-stu-id="96212-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="96212-214">ときに、 *decimal_integer_literal* 2147483648 値 (2 ^31) および no *integer_type_suffix*単項マイナス演算子トークンの直後に続くトークンとして表示されます ([単項マイナス演算子](expressions.md#unary-minus-operator))、結果は型の定数`int`値-2147483648 (-2 ^31)。</span><span class="sxs-lookup"><span data-stu-id="96212-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="96212-215">その他のすべての状況でこのような*decimal_integer_literal*の種類は`uint`します。</span><span class="sxs-lookup"><span data-stu-id="96212-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="96212-216">ときに、 *decimal_integer_literal* 9223372036854775808 値 (2 ^63) および no *integer_type_suffix*または*integer_type_suffix* `L`または`l`単項マイナス演算子トークンの直後に続くトークンとして表示されます ([単項マイナス演算子](expressions.md#unary-minus-operator))、結果は型の定数`long`値-9223372036854775808 (-2 ^63)。</span><span class="sxs-lookup"><span data-stu-id="96212-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="96212-217">その他のすべての状況でこのような*decimal_integer_literal*の種類は`ulong`します。</span><span class="sxs-lookup"><span data-stu-id="96212-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="96212-218">実際のリテラル</span><span class="sxs-lookup"><span data-stu-id="96212-218">Real literals</span></span>

<span data-ttu-id="96212-219">実際のリテラルの型の値を記述に使用`float`、 `double`、および`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="96212-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

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

<span data-ttu-id="96212-220">ない場合は*real_type_suffix*を指定すると、実際のリテラルの型は`double`します。</span><span class="sxs-lookup"><span data-stu-id="96212-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="96212-221">それ以外の場合、実数のリテラルの種類を次のように決定実数型のサフィックス。</span><span class="sxs-lookup"><span data-stu-id="96212-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="96212-222">実数のリテラル サフィックス`F`または`f`の種類は`float`します。</span><span class="sxs-lookup"><span data-stu-id="96212-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="96212-223">たとえば、リテラルは、 `1f`、 `1.5f`、`1e10f`と`123.456F`すべて型`float`。</span><span class="sxs-lookup"><span data-stu-id="96212-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="96212-224">実数のリテラル サフィックス`D`または`d`の種類は`double`します。</span><span class="sxs-lookup"><span data-stu-id="96212-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="96212-225">たとえば、リテラルは、 `1d`、 `1.5d`、`1e10d`と`123.456D`すべて型`double`。</span><span class="sxs-lookup"><span data-stu-id="96212-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="96212-226">実数のリテラル サフィックス`M`または`m`の種類は`decimal`します。</span><span class="sxs-lookup"><span data-stu-id="96212-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="96212-227">たとえば、リテラルは、 `1m`、 `1.5m`、`1e10m`と`123.456M`すべて型`decimal`。</span><span class="sxs-lookup"><span data-stu-id="96212-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="96212-228">このリテラルに変換を`decimal`、正確な値を取得し、必要に応じてを使用して最も近い表現可能な値に丸め値銀行型丸めが ([10 進数型](types.md#the-decimal-type))。</span><span class="sxs-lookup"><span data-stu-id="96212-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="96212-229">値が丸められますか、値が 0 (この後者の場合、記号と小数点 0 になります) しない限り、リテラルのスケールが保持されます。</span><span class="sxs-lookup"><span data-stu-id="96212-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="96212-230">そのため、リテラル`2.900m`記号と小数点以下のフォームに解析`0`、係数`2900`、有効桁数と小数点`3`します。</span><span class="sxs-lookup"><span data-stu-id="96212-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="96212-231">指定されたリテラルは、示された型で表すことができない、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="96212-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="96212-232">型の実数のリテラル値`float`または`double`IEEE を使用して決定されます"round を最も近い"モードです。</span><span class="sxs-lookup"><span data-stu-id="96212-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="96212-233">実数のリテラルに桁が常に必要である、小数点より後に注意してください。</span><span class="sxs-lookup"><span data-stu-id="96212-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="96212-234">たとえば、`1.3F`が実数値リテラルですが`1.F`はありません。</span><span class="sxs-lookup"><span data-stu-id="96212-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="96212-235">文字リテラル</span><span class="sxs-lookup"><span data-stu-id="96212-235">Character literals</span></span>

<span data-ttu-id="96212-236">文字リテラルが単一の文字を表し、通常ように、引用符で囲まれた文字で構成されます`'a'`します。</span><span class="sxs-lookup"><span data-stu-id="96212-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="96212-237">メモ:ANTLR 文法の表記では、次を混乱に!</span><span class="sxs-lookup"><span data-stu-id="96212-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="96212-238">記述するときに、ANTLR`\'`の単一引用符現時点では`'`します。</span><span class="sxs-lookup"><span data-stu-id="96212-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="96212-239">記述するときと`\\`現時点では、1 つの円記号の`\`します。</span><span class="sxs-lookup"><span data-stu-id="96212-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="96212-240">そのため、単一引用符、文字、単一引用符で始まるリテラル文字の最初の規則を意味します。</span><span class="sxs-lookup"><span data-stu-id="96212-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="96212-241">11 個の可能な単純なエスケープ シーケンスと`\'`、 `\"`、 `\\`、 `\0`、 `\a`、 `\b`、 `\f`、 `\n`、 `\r`、 `\t`、 `\v`.</span><span class="sxs-lookup"><span data-stu-id="96212-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

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

<span data-ttu-id="96212-242">円記号に続く文字 (`\`) で、*文字*、次の文字のいずれかを指定する必要があります: `'`、 `"`、 `\`、 `0`、 `a`、 `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="96212-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="96212-243">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="96212-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="96212-244">16 進数のエスケープ シーケンスは、16 進数の数値以下で構成された値を 1 つの Unicode 文字を表します"`\x`"。</span><span class="sxs-lookup"><span data-stu-id="96212-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="96212-245">リテラル文字で表される値がより大きいかどうか`U+FFFF`コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="96212-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="96212-246">Unicode 文字のエスケープ シーケンス ([Unicode 文字のエスケープ シーケンス](lexical-structure.md#unicode-character-escape-sequences)) リテラル文字の範囲で指定する必要があります`U+0000`に`U+FFFF`します。</span><span class="sxs-lookup"><span data-stu-id="96212-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="96212-247">次の表で説明されているように、単純なエスケープ シーケンスは、Unicode 文字のエンコーディングを表します。</span><span class="sxs-lookup"><span data-stu-id="96212-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="96212-248">__エスケープ シーケンス__</span><span class="sxs-lookup"><span data-stu-id="96212-248">__Escape sequence__</span></span> | <span data-ttu-id="96212-249">__文字の名前__</span><span class="sxs-lookup"><span data-stu-id="96212-249">__Character name__</span></span> | <span data-ttu-id="96212-250">__Unicode エンコーディング__</span><span class="sxs-lookup"><span data-stu-id="96212-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="96212-251">単一引用符</span><span class="sxs-lookup"><span data-stu-id="96212-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="96212-252">二重引用符</span><span class="sxs-lookup"><span data-stu-id="96212-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="96212-253">円記号</span><span class="sxs-lookup"><span data-stu-id="96212-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="96212-254">Null</span><span class="sxs-lookup"><span data-stu-id="96212-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="96212-255">警告</span><span class="sxs-lookup"><span data-stu-id="96212-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="96212-256">バックスペース</span><span class="sxs-lookup"><span data-stu-id="96212-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="96212-257">フォーム フィード</span><span class="sxs-lookup"><span data-stu-id="96212-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="96212-258">改行</span><span class="sxs-lookup"><span data-stu-id="96212-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="96212-259">キャリッジ リターン</span><span class="sxs-lookup"><span data-stu-id="96212-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="96212-260">水平タブ</span><span class="sxs-lookup"><span data-stu-id="96212-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="96212-261">垂直タブ</span><span class="sxs-lookup"><span data-stu-id="96212-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="96212-262">型を*character_literal*は`char`します。</span><span class="sxs-lookup"><span data-stu-id="96212-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="96212-263">文字列リテラル</span><span class="sxs-lookup"><span data-stu-id="96212-263">String literals</span></span>

<span data-ttu-id="96212-264">C# では、文字列リテラルの 2 つの形式:***標準文字列リテラル***と***verbatim 文字列リテラル***します。</span><span class="sxs-lookup"><span data-stu-id="96212-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="96212-265">標準リテラル文字列としての二重引用符で囲まれた 0 個以上の文字から成る`"hello"`、両方の単純なエスケープ シーケンスを含めることができ、(など`\t`でタブ文字)、16 進数と Unicode のエスケープ シーケンス。</span><span class="sxs-lookup"><span data-stu-id="96212-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="96212-266">Verbatim 文字列リテラルから成る、`@`文字の後に二重引用符文字、0 個以上の文字と終わりの二重引用符文字。</span><span class="sxs-lookup"><span data-stu-id="96212-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="96212-267">簡単な例は、`@"hello"`します。</span><span class="sxs-lookup"><span data-stu-id="96212-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="96212-268">逐語的文字列リテラル、区切り記号の間の文字は、そのまま解釈されている唯一の例外を*quote_escape_sequence*します。</span><span class="sxs-lookup"><span data-stu-id="96212-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="96212-269">具体的には、単純なエスケープ シーケンス、および 16 進数、および Unicode のエスケープ シーケンスは、verbatim 文字列リテラルでは処理されません。</span><span class="sxs-lookup"><span data-stu-id="96212-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="96212-270">Verbatim 文字列リテラルは、複数行にわたる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="96212-270">A verbatim string literal may span multiple lines.</span></span>

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

<span data-ttu-id="96212-271">円記号に続く文字 (`\`) で、 *regular_string_literal_character* 、次の文字のいずれかを指定する必要があります: `'`、 `"`、 `\`、 `0`、 `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="96212-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="96212-272">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="96212-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="96212-273">例では、</span><span class="sxs-lookup"><span data-stu-id="96212-273">The example</span></span>
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
<span data-ttu-id="96212-274">さまざまな文字列リテラルを示しています。</span><span class="sxs-lookup"><span data-stu-id="96212-274">shows a variety of string literals.</span></span> <span data-ttu-id="96212-275">最後の文字列リテラル、 `j`、逐語的文字列リテラルは複数の行にまたがるには。</span><span class="sxs-lookup"><span data-stu-id="96212-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="96212-276">改行文字などの空白を含む、引用符の間の文字をそのまま維持されます。</span><span class="sxs-lookup"><span data-stu-id="96212-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="96212-277">16 進数のエスケープ シーケンスは、文字列リテラル、16 進数が変数を持つことができますので`"\x123"`16 進値 123 の 1 つの文字が含まれています。</span><span class="sxs-lookup"><span data-stu-id="96212-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="96212-278">を 16 進値 12 が 3 文字が続く文字を含む文字列を作成する 1 つ記述`"\x00123"`または`"\x12" + "3"`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="96212-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="96212-279">型を*string_literal*は`string`します。</span><span class="sxs-lookup"><span data-stu-id="96212-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="96212-280">各文字列リテラルは、新しい文字列インスタンスで必ずしも発生するされません。</span><span class="sxs-lookup"><span data-stu-id="96212-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="96212-281">2 つ以上の文字列リテラルが場合に応じて、文字列の等値演算子と同じです ([文字列等値演算子](expressions.md#string-equality-operators)) これらの文字列リテラルは、同じ文字列インスタンスを参照してください、同じプログラムで表示されます。</span><span class="sxs-lookup"><span data-stu-id="96212-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="96212-282">によって生成された出力インスタンス、</span><span class="sxs-lookup"><span data-stu-id="96212-282">For instance, the output produced by</span></span>
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
<span data-ttu-id="96212-283">`True`のため、2 つのリテラルは、同じ文字列インスタンスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="96212-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="96212-284">補間文字列リテラル</span><span class="sxs-lookup"><span data-stu-id="96212-284">Interpolated string literals</span></span>

<span data-ttu-id="96212-285">補間文字列リテラルは、文字列リテラルに似ていますで区切られた穴が含まれて`{`と`}`式に、発生することができます。</span><span class="sxs-lookup"><span data-stu-id="96212-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="96212-286">実行時に穴が発生した位置に文字列に代入されるテキストがフォームが目的に設定された式を評価します。</span><span class="sxs-lookup"><span data-stu-id="96212-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="96212-287">構文とセマンティクスの文字列補間がセクションで説明されている ([文字列補間](expressions.md#interpolated-strings))。</span><span class="sxs-lookup"><span data-stu-id="96212-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="96212-288">文字列リテラルのように、補間文字列リテラルは正規表現または逐語的のいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="96212-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="96212-289">挿入の標準文字列リテラルを区切ることによって`$"`と`"`、補間の逐語的文字列リテラルを区切ることによって、`$@"`と`"`します。</span><span class="sxs-lookup"><span data-stu-id="96212-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="96212-290">など、他のリテラル、補間文字列リテラルの字句解析は、最初に以下の文法に従って、1 つのトークンの結果します。</span><span class="sxs-lookup"><span data-stu-id="96212-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="96212-291">ただし、構文解析前に、補間文字列リテラルの 1 つのトークンはセキュリティ ホールを囲む文字列の部品用のいくつかのトークンに分割、ホールで発生している入力の要素がもう一度分析構文。</span><span class="sxs-lookup"><span data-stu-id="96212-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="96212-292">これにより、処理するには、修正は、構文の解析を処理するためのトークンのシーケンスに最終的につながる場合、構文的に補間文字列リテラルよりさらに生成可能性があります。</span><span class="sxs-lookup"><span data-stu-id="96212-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

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

<span data-ttu-id="96212-293">*Interpolated_string_literal*トークンが複数のトークンおよびその他の入力要素を次のように内に見つかった位置の順序で解釈、 *interpolated_string_literal*:</span><span class="sxs-lookup"><span data-stu-id="96212-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="96212-294">次の出現回数が個別の個々 のトークンとして再解釈: 先頭`$`記号、 *interpolated_regular_string_whole*、 *interpolated_regular_string_start*、*interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_whole*、 *interpolated_verbatim_string_start*、 *interpolated_verbatim_string_mid*と*interpolated_verbatim_string_end*します。</span><span class="sxs-lookup"><span data-stu-id="96212-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="96212-295">出現*regular_balanced_text*と*verbatim_balanced_text*としてこれらの間は再処理する*input_section* ([字句解析](lexical-structure.md#lexical-analysis)) と、入力要素の結果として得られるシーケンスとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="96212-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="96212-296">加え、補間文字列リテラル トークンに含まれます。</span><span class="sxs-lookup"><span data-stu-id="96212-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="96212-297">構文解析にトークンを再結合、 *interpolated_string_expression* ([文字列補間](expressions.md#interpolated-strings))。</span><span class="sxs-lookup"><span data-stu-id="96212-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="96212-298">TODO の例</span><span class="sxs-lookup"><span data-stu-id="96212-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="96212-299">Null リテラル</span><span class="sxs-lookup"><span data-stu-id="96212-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="96212-300">*Null_literal*参照型または null 許容型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="96212-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="96212-301">演算子と区切り記号</span><span class="sxs-lookup"><span data-stu-id="96212-301">Operators and punctuators</span></span>

<span data-ttu-id="96212-302">これには演算子と区切り記号のいくつかの種類があります。</span><span class="sxs-lookup"><span data-stu-id="96212-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="96212-303">演算子は、1 つまたは複数のオペランドに関連する操作を記述する式で使用されます。</span><span class="sxs-lookup"><span data-stu-id="96212-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="96212-304">たとえば、式`a + b`を使用して、 `+` 2 つのオペランドを追加する演算子`a`と`b`します。</span><span class="sxs-lookup"><span data-stu-id="96212-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="96212-305">区切り記号はグループ化および分離することです。</span><span class="sxs-lookup"><span data-stu-id="96212-305">Punctuators are for grouping and separating.</span></span>

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

<span data-ttu-id="96212-306">垂直のバーで、 *right_shift*と*right_shift_assignment*生産を使用するには、構文の文法では、任意の種類の文字がないのでは、他の生産とは異なり (あって空白文字) は、トークンの間に許可します。</span><span class="sxs-lookup"><span data-stu-id="96212-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="96212-307">それらの生産がの適切な処理を有効にするために特別に扱われます*type_parameter_list*s ([パラメーター入力](classes.md#type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="96212-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="96212-308">プリプロセッサ ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="96212-308">Pre-processing directives</span></span>

<span data-ttu-id="96212-309">プリプロセッサ ディレクティブは、条件付きでソース ファイル、レポートのエラーおよび警告の条件のセクションをスキップして、ソース コードの異なる領域を区切るために機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="96212-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="96212-310">「プリプロセッサ ディレクティブ」という用語は、C および C++ のプログラミング言語との整合性のみ使用されます。</span><span class="sxs-lookup"><span data-stu-id="96212-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="96212-311">C# の場合は、別の前処理ステップ; はありません。プリプロセッサ ディレクティブは、字句解析フェーズの一環として処理されます。</span><span class="sxs-lookup"><span data-stu-id="96212-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

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

<span data-ttu-id="96212-312">次のプリプロセッサ ディレクティブを使用できます。</span><span class="sxs-lookup"><span data-stu-id="96212-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="96212-313">`#define` `#undef`を定義し、それぞれ、条件付きコンパイル シンボルを未定義に使用されます ([宣言ディレクティブ](lexical-structure.md#declaration-directives))。</span><span class="sxs-lookup"><span data-stu-id="96212-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="96212-314">`#if`、 `#elif`、 `#else`、および`#endif`、条件付きでソース コードのセクションをスキップする ([条件付きコンパイル ディレクティブ](lexical-structure.md#conditional-compilation-directives))。</span><span class="sxs-lookup"><span data-stu-id="96212-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="96212-315">`#line`、行番号のエラーと警告の表示の制御に使用される ([ディレクティブの行](lexical-structure.md#line-directives))。</span><span class="sxs-lookup"><span data-stu-id="96212-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="96212-316">`#error` `#warning`、それぞれ、エラーと警告の発行に使用する ([診断ディレクティブ](lexical-structure.md#diagnostic-directives))。</span><span class="sxs-lookup"><span data-stu-id="96212-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="96212-317">`#region` `#endregion`、ソース コードのセクションを明示的にマークに使用されます ([領域ディレクティブ](lexical-structure.md#region-directives))。</span><span class="sxs-lookup"><span data-stu-id="96212-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="96212-318">`#pragma`、コンパイラ オプションのコンテキスト情報の指定に使用される ([プラグマ ディレクティブ](lexical-structure.md#pragma-directives))。</span><span class="sxs-lookup"><span data-stu-id="96212-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="96212-319">プリプロセッサ ディレクティブが常にソース コードの別の行を占有し、常に始まり、`#`文字と前処理ディレクティブ名。</span><span class="sxs-lookup"><span data-stu-id="96212-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="96212-320">空白文字が前に発生する、`#`文字、および、`#`文字とディレクティブの名前。</span><span class="sxs-lookup"><span data-stu-id="96212-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="96212-321">含むソース行、 `#define`、 `#undef`、 `#if`、 `#elif`、 `#else`、 `#endif`、 `#line`、または`#endregion`ディレクティブは、単一行コメントとなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="96212-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="96212-322">コメントの区切り (、`/* */`形式のコメント) は、プリプロセッサ ディレクティブを含むソース行では許可されていません。</span><span class="sxs-lookup"><span data-stu-id="96212-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="96212-323">プリプロセッサ ディレクティブは、トークンは、C# の構文の文法の一部ではないです。</span><span class="sxs-lookup"><span data-stu-id="96212-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="96212-324">ただし、プリプロセッサ ディレクティブまたはトークンのシーケンスを除外するために使用して、その方法でに影響する C# プログラムの意味。</span><span class="sxs-lookup"><span data-stu-id="96212-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="96212-325">たとえば、コンパイルされると、プログラム。</span><span class="sxs-lookup"><span data-stu-id="96212-325">For example, when compiled, the program:</span></span>
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
<span data-ttu-id="96212-326">プログラムとトークンの正確な順序で結果:</span><span class="sxs-lookup"><span data-stu-id="96212-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="96212-327">つまり、2 つのプログラムは、構文的には、まったく異なる構文的には同じです。</span><span class="sxs-lookup"><span data-stu-id="96212-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="96212-328">[条件付きコンパイル シンボル]</span><span class="sxs-lookup"><span data-stu-id="96212-328">Conditional compilation symbols</span></span>

<span data-ttu-id="96212-329">条件付きコンパイルの機能、 `#if`、 `#elif`、 `#else`、および`#endif`ディレクティブは処理前の式によって制御されます ([プリプロセス式](lexical-structure.md#pre-processing-expressions))条件付きコンパイル シンボルを選択します。</span><span class="sxs-lookup"><span data-stu-id="96212-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="96212-330">条件付きコンパイル シンボルが 2 つの状態:***定義***または***未定義***します。</span><span class="sxs-lookup"><span data-stu-id="96212-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="96212-331">ソース ファイルの構文の処理の先頭には、(コマンド ライン コンパイラ オプション) などの外部機構によって明示的に定義されている場合を除き、条件付きコンパイル シンボルは定義されません。</span><span class="sxs-lookup"><span data-stu-id="96212-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="96212-332">ときに、`#define`ディレクティブが処理されると、そのソース ファイルでそのディレクティブで、条件付きコンパイル シンボルが定義されています。</span><span class="sxs-lookup"><span data-stu-id="96212-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="96212-333">シンボルは削除されるまで、`#undef`同じシンボルが処理されるか、ソース ファイルの末尾に到達するまでディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="96212-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="96212-334">この意味は`#define`と`#undef`同じプログラム内で他のソース ファイルに 1 つのソース ファイルでディレクティブの影響がありません。</span><span class="sxs-lookup"><span data-stu-id="96212-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="96212-335">処理前の式で参照されている、定義済みの条件付きコンパイル シンボルがブール値`true`、ブール値であり、未定義の条件付きコンパイル シンボル`false`します。</span><span class="sxs-lookup"><span data-stu-id="96212-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="96212-336">要件はありません、条件付きコンパイル シンボルを明示的に宣言処理前の式で参照される前にします。</span><span class="sxs-lookup"><span data-stu-id="96212-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="96212-337">宣言されていないシンボルが代わりに、単に定義されていませんし、値があるため`false`します。</span><span class="sxs-lookup"><span data-stu-id="96212-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="96212-338">条件付きコンパイル シンボルの名前空間と区別され、C# プログラムの他のすべての名前付きエンティティから分離します。</span><span class="sxs-lookup"><span data-stu-id="96212-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="96212-339">条件付きコンパイル シンボルでのみ参照できます`#define`と`#undef`ディレクティブと、処理前の式で。</span><span class="sxs-lookup"><span data-stu-id="96212-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="96212-340">処理前の式</span><span class="sxs-lookup"><span data-stu-id="96212-340">Pre-processing expressions</span></span>

<span data-ttu-id="96212-341">処理前の式が発生することが`#if`と`#elif`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="96212-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="96212-342">演算子`!`、 `==`、 `!=`、`&&`と`||`処理前の式では許可されてあり、グループ化かっこを使用できます。</span><span class="sxs-lookup"><span data-stu-id="96212-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

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

<span data-ttu-id="96212-343">処理前の式で参照されている、定義済みの条件付きコンパイル シンボルがブール値`true`、ブール値であり、未定義の条件付きコンパイル シンボル`false`します。</span><span class="sxs-lookup"><span data-stu-id="96212-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="96212-344">常に、処理前の式の評価は、ブール値を生成します。</span><span class="sxs-lookup"><span data-stu-id="96212-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="96212-345">処理前の式の評価の規則は、定数式の場合と同じ ([定数式](expressions.md#constant-expressions))、参照できるのみ、ユーザー定義エンティティは、条件付きコンパイル シンボル.</span><span class="sxs-lookup"><span data-stu-id="96212-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="96212-346">宣言ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="96212-346">Declaration directives</span></span>

<span data-ttu-id="96212-347">宣言のディレクティブは、定義または条件付きコンパイル シンボルを未定義に使用されます。</span><span class="sxs-lookup"><span data-stu-id="96212-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="96212-348">処理、`#define`ディレクティブにより、定義する特定の条件付きコンパイル シンボル ディレクティブに続くソース行から開始します。</span><span class="sxs-lookup"><span data-stu-id="96212-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="96212-349">同様の処理、`#undef`ディレクティブとディレクティブを次のソース行で始まる、未定義になる特定の条件付きコンパイル シンボル。</span><span class="sxs-lookup"><span data-stu-id="96212-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="96212-350">すべて`#define`と`#undef`ソース ファイルでディレクティブは、最初の前に出現する必要があります*トークン*([トークン](lexical-structure.md#tokens)) は、ソース ファイルでそれ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="96212-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="96212-351">直感的な用語で`#define`と`#undef`ディレクティブがソース ファイルの「実際のコード」を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="96212-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="96212-352">例:</span><span class="sxs-lookup"><span data-stu-id="96212-352">The example:</span></span>
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
<span data-ttu-id="96212-353">有効ではため、`#define`ディレクティブの前に、最初のトークン (、`namespace`キーワード) でソース ファイル。</span><span class="sxs-lookup"><span data-stu-id="96212-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="96212-354">次の例では、コンパイル時エラーのため、結果を`#define`実際のコードします。</span><span class="sxs-lookup"><span data-stu-id="96212-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
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

<span data-ttu-id="96212-355">A`#define`既に定義されている、すべての介在が存在せず、条件付きコンパイル シンボルを定義することがあります`#undef`そのシンボル。</span><span class="sxs-lookup"><span data-stu-id="96212-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="96212-356">次の例は、条件付きコンパイル シンボルを定義します。`A`し、もう一度定義します。</span><span class="sxs-lookup"><span data-stu-id="96212-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="96212-357">A`#undef`可能性があります""条件付きコンパイル シンボルを未定義に定義されていません。</span><span class="sxs-lookup"><span data-stu-id="96212-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="96212-358">次の例は、条件付きコンパイル シンボルを定義します。`A`してし、未定義に 2 回ですが、2 つ目`#undef`、影響を与えません有効であります。</span><span class="sxs-lookup"><span data-stu-id="96212-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="96212-359">条件付きコンパイル ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="96212-359">Conditional compilation directives</span></span>

<span data-ttu-id="96212-360">条件付きコンパイル ディレクティブは、条件付きで含めるか、ソース ファイルの部分を除外に使用されます。</span><span class="sxs-lookup"><span data-stu-id="96212-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

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

<span data-ttu-id="96212-361">、の順でから成るセットとして示されるように、構文、条件付きコンパイル ディレクティブを書き込む必要があります、`#if`ディレクティブ、0 個以上`#elif`ディレクティブ、0 または 1 個`#else`ディレクティブ、および`#endif`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="96212-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="96212-362">ディレクティブの間、ソース コードの条件付きセクションです。</span><span class="sxs-lookup"><span data-stu-id="96212-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="96212-363">各セクションは、直前のディレクティブによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="96212-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="96212-364">条件付きセクションを含めることができます自体入れ子になった条件付きコンパイル ディレクティブ提供、これらのディレクティブは、完全なセットを形成します。</span><span class="sxs-lookup"><span data-stu-id="96212-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="96212-365">A *pp_conditional*の格納されている 1 つだけ選択*conditional_section*の通常の構文処理。</span><span class="sxs-lookup"><span data-stu-id="96212-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="96212-366">*Pp_expression*の s、`#if`と`#elif`ディレクティブは、1 つを生成するまで順序で評価されます`true`します。</span><span class="sxs-lookup"><span data-stu-id="96212-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="96212-367">式の結果が場合`true`、 *conditional_section*の対応するディレクティブが選択されています。</span><span class="sxs-lookup"><span data-stu-id="96212-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="96212-368">すべて*pp_expression*s yield `false`、場合に、`#else`ディレクティブが存在する、 *conditional_section*の`#else`ディレクティブが選択されています。</span><span class="sxs-lookup"><span data-stu-id="96212-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="96212-369">それ以外の場合、no *conditional_section*が選択されています。</span><span class="sxs-lookup"><span data-stu-id="96212-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="96212-370">選択した*conditional_section*として、通常、いずれかが処理される場合、 *input_section*: セクションに含まれるソース コードは、構文、文法に従う必要があります元のトークンが生成されます。セクションのコードプリプロセッサ ディレクティブのセクションでは、所定の影響を与えます。</span><span class="sxs-lookup"><span data-stu-id="96212-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="96212-371">残りの*conditional_section*として処理されているのであれば、 *skipped_section*プリプロセッサ ディレクティブを除く: %s、セクション内のソース コードが必要に準拠していない、字句文法; なしセクションで、ソース コードからトークンが生成されます。プリプロセッサ ディレクティブのセクションでは構文的に正しくなければなりませんが、それ以外の場合は処理されません。</span><span class="sxs-lookup"><span data-stu-id="96212-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="96212-372">内で、 *conditional_section*として処理されますが、 *skipped_section*を入れ子になった*conditional_section*s (に含まれている入れ子になった`#if`...`#endif`と`#region`.`#endregion`を構築します) として処理されます。 また*skipped_section*秒。</span><span class="sxs-lookup"><span data-stu-id="96212-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="96212-373">次の例は、条件付きコンパイル ディレクティブを入れ子を示しています。</span><span class="sxs-lookup"><span data-stu-id="96212-373">The following example illustrates how conditional compilation directives can nest:</span></span>
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

<span data-ttu-id="96212-374">プリプロセッサ ディレクティブを除くスキップされたソース コードが字句解析の対象です。</span><span class="sxs-lookup"><span data-stu-id="96212-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="96212-375">たとえば、未終了のコメントに関係なく有効では、次の`#else`セクション。</span><span class="sxs-lookup"><span data-stu-id="96212-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
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

<span data-ttu-id="96212-376">ただし、プリプロセッサ ディレクティブがソース コードのセクションをスキップしたであっても正しい構文的に必要なことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="96212-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="96212-377">複数行の入力要素内に存在する場合、プリプロセッサ ディレクティブは処理されません。</span><span class="sxs-lookup"><span data-stu-id="96212-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="96212-378">たとえば、プログラムします。</span><span class="sxs-lookup"><span data-stu-id="96212-378">For example, the program:</span></span>
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
<span data-ttu-id="96212-379">出力結果:</span><span class="sxs-lookup"><span data-stu-id="96212-379">results in the output:</span></span>
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="96212-380">評価に特有の場合、プリプロセッサ ディレクティブの処理は、セットが依存可能性があります、 *pp_expression*します。</span><span class="sxs-lookup"><span data-stu-id="96212-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="96212-381">例:</span><span class="sxs-lookup"><span data-stu-id="96212-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="96212-382">常に同じトークン ストリームを生成します (`class` `Q` `{` `}`) かどうかに関係なく、`X`が定義されています。</span><span class="sxs-lookup"><span data-stu-id="96212-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="96212-383">場合`X`が定義するには、ディレクティブだけが処理されます`#if`と`#endif`複数行のコメントにしている。</span><span class="sxs-lookup"><span data-stu-id="96212-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="96212-384">場合`X`が未定義の場合、3 つのディレクティブ (`#if`、 `#else`、 `#endif`) ディレクティブのセットの一部であります。</span><span class="sxs-lookup"><span data-stu-id="96212-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="96212-385">診断ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="96212-385">Diagnostic directives</span></span>

<span data-ttu-id="96212-386">診断のディレクティブを使用すると、エラー メッセージとその他のコンパイル時エラーと警告と同じ方法で報告される警告メッセージを明示的に生成します。</span><span class="sxs-lookup"><span data-stu-id="96212-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

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

<span data-ttu-id="96212-387">例:</span><span class="sxs-lookup"><span data-stu-id="96212-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="96212-388">常に (「コード レビューでは、チェックイン前に必要な」)、警告を生成し、コンパイル時エラーを生成します (「をビルドすることはできませんデバッグし、製品版」) 場合、条件付きシンボル`Debug`と`Retail`の両方が定義されています。</span><span class="sxs-lookup"><span data-stu-id="96212-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="96212-389">なお、 *pp_message*任意のテキストを含めることができます。 具体的には、含める必要はありません、適切な形式のトークン、word で単一引用符で示すように`can't`します。</span><span class="sxs-lookup"><span data-stu-id="96212-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="96212-390">領域ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="96212-390">Region directives</span></span>

<span data-ttu-id="96212-391">領域ディレクティブを使用して、ソース コードの領域を明示的にマークします。</span><span class="sxs-lookup"><span data-stu-id="96212-391">The region directives are used to explicitly mark regions of source code.</span></span>

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

<span data-ttu-id="96212-392">領域に接続されているセマンティックな意味がありません。リージョンが使用するため、プログラマや自動化ツール ソース コードのセクションをマークします。</span><span class="sxs-lookup"><span data-stu-id="96212-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="96212-393">指定されたメッセージ、`#region`または`#endregion`ディレクティブはセマンティックな意味は無効同様に、領域を識別するためにのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="96212-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="96212-394">一致する`#region`と`#endregion`ディレクティブが異なる場合がありますが*pp_message*秒。</span><span class="sxs-lookup"><span data-stu-id="96212-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="96212-395">領域の処理の構文:</span><span class="sxs-lookup"><span data-stu-id="96212-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="96212-396">フォームの条件付きコンパイル ディレクティブの構文の処理に正確に対応しています。</span><span class="sxs-lookup"><span data-stu-id="96212-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="96212-397">行ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="96212-397">Line directives</span></span>

<span data-ttu-id="96212-398">行ディレクティブは、行番号とが報告された警告やエラーなどの出力で、コンパイラによって、呼び出し元情報属性で使用されるソース ファイル名を変更するために使用可能性があります ([呼び出し元情報属性](attributes.md#caller-info-attributes))。</span><span class="sxs-lookup"><span data-stu-id="96212-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="96212-399">行ディレクティブは、他のテキスト入力からの C# ソース コードを生成するメタ プログラミング ツールで最もよく使用されます。</span><span class="sxs-lookup"><span data-stu-id="96212-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

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

<span data-ttu-id="96212-400">ない場合`#line`ディレクティブが存在する、コンパイラは実際の行番号とその出力でのソース ファイル名を報告します。</span><span class="sxs-lookup"><span data-stu-id="96212-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="96212-401">処理するときに、`#line`ディレクティブが含まれる、 *line_indicator*でない`default`コンパイラは、特定の行番号 (および指定した場合、ファイル名) を持つものとしてディレクティブの後の行を扱います。</span><span class="sxs-lookup"><span data-stu-id="96212-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="96212-402">A`#line default`ディレクティブ上のすべての #line ディレクティブの効果を反転させます。</span><span class="sxs-lookup"><span data-stu-id="96212-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="96212-403">コンパイラが正確にない場合、後続の行の場合は true。 行情報を報告`#line`ディレクティブは処理されている必要がありました。</span><span class="sxs-lookup"><span data-stu-id="96212-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="96212-404">A`#line hidden`ディレクティブには、ファイルへの影響はありませんし、行番号のエラー報告は、メッセージが、ソース レベルのデバッグは影響します。</span><span class="sxs-lookup"><span data-stu-id="96212-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="96212-405">すべての行の間でのデバッグ時に、`#line hidden`ディレクティブとそれに続く`#line`ディレクティブ (でない`#line hidden`) 行番号情報があるありません。</span><span class="sxs-lookup"><span data-stu-id="96212-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="96212-406">デバッガーでコードをステップ実行時に、これらの行はすべてスキップされます。</span><span class="sxs-lookup"><span data-stu-id="96212-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="96212-407">なお、 *file_name*されるエスケープ文字は処理されません通常の文字列リテラルと異なります、"`\`"文字が単純に内での通常のバック スラッシュ文字を指定、 *file_name*。</span><span class="sxs-lookup"><span data-stu-id="96212-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="96212-408">プラグマ ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="96212-408">Pragma directives</span></span>

<span data-ttu-id="96212-409">`#pragma`プリプロセッサ ディレクティブを使用すると、コンパイラにコンテキスト情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="96212-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="96212-410">情報の指定、`#pragma`ディレクティブには、プログラムのセマンティクスは変わりません。</span><span class="sxs-lookup"><span data-stu-id="96212-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="96212-411">C# では`#pragma`コンパイラの警告を制御するためのディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="96212-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="96212-412">将来のバージョンの言語は、追加で含めることができます`#pragma`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="96212-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="96212-413">他の C# コンパイラとの相互運用のために、Microsoft C# コンパイラを発行しません不明なコンパイル エラー`#pragma`ディレクティブはこのようなディレクティブは、ただし警告を生成します。</span><span class="sxs-lookup"><span data-stu-id="96212-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="96212-414">Pragma 警告</span><span class="sxs-lookup"><span data-stu-id="96212-414">Pragma warning</span></span>

<span data-ttu-id="96212-415">`#pragma warning`ディレクティブを使用して無効にするか、またはすべてを復元する、またはそれ以降のプログラム テキストのコンパイル中にメッセージを特定の警告のセット。</span><span class="sxs-lookup"><span data-stu-id="96212-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

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

<span data-ttu-id="96212-416">A`#pragma warning`警告の一覧の指定を省略するディレクティブがすべての警告に影響します。</span><span class="sxs-lookup"><span data-stu-id="96212-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="96212-417">A`#pragma warning`ディレクティブが含まれています、警告の一覧の一覧で指定されている警告のみに影響を与えます。</span><span class="sxs-lookup"><span data-stu-id="96212-417">A `#pragma warning` directive the includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="96212-418">A`#pragma warning disable`ディレクティブ無効にします。 すべてまたは指定した警告のセット。</span><span class="sxs-lookup"><span data-stu-id="96212-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="96212-419">A`#pragma warning restore`ディレクティブ復元のすべてまたは特定の一連のコンパイル単位の先頭で有効にされた状態に警告します。</span><span class="sxs-lookup"><span data-stu-id="96212-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="96212-420">特定の警告が外部で無効になっている場合、 `#pragma warning restore` (すべてのかどうか、または特定の警告) 警告を再度有効にはありません。</span><span class="sxs-lookup"><span data-stu-id="96212-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="96212-421">次の例は、の使用を示しています。 `#pragma warning` 、警告を一時的に無効にする場合に報告廃止 Microsoft C# コンパイラから警告番号を使用して、メンバーが参照します。</span><span class="sxs-lookup"><span data-stu-id="96212-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
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
