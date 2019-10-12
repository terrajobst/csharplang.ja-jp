---
ms.openlocfilehash: 4676bcd3f0a92260b4e5e20a0aa5b5ec00bf204e
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704070"
---
# <a name="lexical-structure"></a>字句構造

## <a name="programs"></a>Programs

C# ***プログラム***は、1つまたは複数の***ソースファイル***で構成され、正式には***コンパイルユニット***([コンパイル単位](namespaces.md#compilation-units)) として知られています。 ソースファイルは、順序付けられた Unicode 文字のシーケンスです。 ソースファイルは、通常、ファイルシステム内のファイルと1対1で対応していますが、この対応は必要ありません。 移植性を最大にするために、ファイルシステム内のファイルを UTF-8 エンコードでエンコードすることをお勧めします。

概念的に言えば、プログラムは次の3つの手順を使用してコンパイルされます。

1. 変換。特定の文字レパートリーとエンコードスキームのファイルを Unicode 文字のシーケンスに変換します。
2. 字句分析。 Unicode 入力文字のストリームをトークンのストリームに変換します。
3. 構文分析。トークンのストリームを実行可能コードに変換します。

## <a name="grammars"></a>文法

この仕様には、2つC#の文法を使用したプログラミング言語の構文が示されています。 ***字句文法***([字句文法](lexical-structure.md#lexical-grammar)) では、Unicode 文字を組み合わせることによって、行ターミネータ、空白、コメント、トークン、およびプリプロセスディレクティブを形成する方法を定義します。 構文***文法 (*** 構文[文法](lexical-structure.md#syntactic-grammar)) は、字句文法の結果として得られるトークンをC#組み合わせてプログラムを形成する方法を定義します。

### <a name="grammar-notation"></a>文法の表記

字句文法と構文文法は、ANTLR 文法ツールの表記を使用して、バッカスナウア記法-Backus-naur 形式で表示されます。

### <a name="lexical-grammar"></a>字句文法

のC#字句文法は、[字句解析](lexical-structure.md#lexical-analysis)、[トークン](lexical-structure.md#tokens)、および[プリプロセスディレクティブ](lexical-structure.md#pre-processing-directives)で表現されます。 字句文法のターミナルシンボルは Unicode 文字セットの文字であり、字句文法は、文字を結合してトークン ([トークン](lexical-structure.md#tokens))、空白 ([空白](lexical-structure.md#white-space))、コメント ([コメント](lexical-structure.md#comments))、およびを形成する方法を指定します。プリプロセスディレクティブ ([前処理ディレクティブ](lexical-structure.md#pre-processing-directives))。

C#プログラム内のすべてのソースファイルは、字句文法 ([字句解析](lexical-structure.md#lexical-analysis)) の*入力*の実稼働に準拠している必要があります。

### <a name="syntactic-grammar"></a>構文文法

のC#構文文法は、この章に従って、章と付録に記載されています。 構文文法のターミナルシンボルは、字句文法によって定義されたトークンです。構文文法では、プログラムを形成C#するためのトークンの結合方法を指定します。

C#プログラム内のすべてのソースファイルは、構文文法 ([コンパイル単位](namespaces.md#compilation-units)) の*compilation_unit*実稼働に準拠している必要があります。

## <a name="lexical-analysis"></a>字句解析

*入力*の運用では、 C#ソースファイルの構文構造を定義します。 C#プログラム内の各ソースファイルは、この字句文法の実稼働に準拠している必要があります。

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

5つの基本的な要素によって、 C#ソースファイルの構文構造が構成されます。ラインターミネータ ([行ターミネータ](lexical-structure.md#line-terminators))、空白 ([空白](lexical-structure.md#white-space))、コメント ([コメント](lexical-structure.md#comments))、トークン ([トークン](lexical-structure.md#tokens))、および前処理ディレクティブ ([プリプロセスディレクティブ](lexical-structure.md#pre-processing-directives))。 これらの基本的な要素の中で、 C#プログラムの構文文法ではトークンのみが重要です ([構文文法](lexical-structure.md#syntactic-grammar))。

C#ソースファイルの字句処理では、ファイルを一連のトークンに減らすことで、構文分析への入力となります。 行ターミネータ、空白、コメントはトークンを区切るために使用でき、プリプロセスディレクティブによってソースファイルのセクションがスキップされることがありますが、それ以外の場合、これらの字句要素C#はプログラムの構文構造には影響しません。

挿入文字列リテラル ([補間文字列リテラル](lexical-structure.md#interpolated-string-literals)) の場合、1つのトークンは字句解析によって最初に生成されますが、複数の入力要素に分割されます。この要素は、すべての補間になるまで字句解析に繰り返し適用されます。文字列リテラルが解決されました。 その後、結果として得られるトークンは、構文分析の入力として機能します。

複数の字句文法の発行がソースファイル内の文字のシーケンスと一致する場合、構文処理では常に、可能な限り長い字句要素が形成されます。 たとえば、文字シーケンス`//`は単一行コメントの先頭として処理されます。これは、その字句要素が1つ`/`のトークンより長いためです。

### <a name="line-terminators"></a>ラインターミネータ

行ターミネータは、 C#ソースファイルの文字を行に分割します。

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

ファイルの終端マーカーを追加するソースコード編集ツールとの互換性を確保し、適切に終了した行のシーケンスとしてソースファイルを表示できるようにするには、 C#プログラム内のすべてのソースファイルに次の変換を順番に適用します。

*  ソースファイルの最後の文字がコントロール z 文字 (`U+001A`) の場合、この文字は削除されます。
*  ソースファイルが空では`U+000D`なく、ソースファイルの最後の文字がキャリッジリターン (`U+000D`)、改行 (`U+000A`)、行区切り記号 (`U+2028`)ではない場合、ソースファイルの末尾に復帰文字()が追加されます。)、または段落区切り記号`U+2029`() です。

### <a name="comments"></a>コメント

1行のコメントと区切られたコメントの2つの形式のコメントがサポートされています。 ***単一行のコメント***は、文字`//`で始まり、ソース行の末尾まで拡張されます。 ***区切ら***れたコメントは、 `/*`文字で始まり、文字`*/`で終わります。 区切られたコメントは複数行にまたがる場合があります。

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

コメントは入れ子になっていません。 `/*`文字シーケンス`//` `//`とは、コメント内では特別な意味を`/*` 持ちません。また、文字シーケンスとは、区切られたコメント内で特別な意味を持ちません。`*/`

コメントは、文字リテラルと文字列リテラル内では処理されません。

例
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
区切られたコメントを含みます。

例
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
複数の単一行コメントを表示します。

### <a name="white-space"></a>空白

空白は、Unicode クラス Zs (空白文字を含む) と、水平タブ文字、垂直タブ文字、およびフォームフィード文字を含む任意の文字として定義されます。

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>トークン

トークンには、識別子、キーワード、リテラル、演算子、および区切り記号のいくつかの種類があります。 空白とコメントはトークンではありませんが、トークンの区切り記号として機能します。

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

### <a name="unicode-character-escape-sequences"></a>Unicode 文字エスケープシーケンス

Unicode 文字のエスケープシーケンスは、Unicode 文字を表します。 Unicode 文字のエスケープシーケンスは、識別子 ([識別子](lexical-structure.md#identifiers))、文字リテラル ([文字](lexical-structure.md#character-literals)リテラル)、および通常の文字列リテラル ([文字列リテラル](lexical-structure.md#string-literals)) で処理されます。 Unicode 文字のエスケープは、他の場所では処理されません (たとえば、operator、など、または keyword を形成する場合)。

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Unicode エスケープシーケンスは、"`\u`" または "`\U`" 文字の後に続く16進数で形成される1つの unicode 文字を表します。 でC#は、文字と文字列値で unicode コードポイントの16ビットエンコードが使用されるため、u + 10000 から u + 10ffff の範囲の unicode 文字は、文字リテラルでは許可されず、文字列リテラルでは unicode サロゲートペアを使用して表されます。 0x10FFFF を超えるコードポイントを含む Unicode 文字はサポートされていません。

複数の翻訳は実行されません。 たとえば、文字列リテラル "`\u005Cu005C`" は、"" ではなく`\`"`\u005C`" に相当します。 Unicode 値`\u005C`は文字 "`\`" です。

例
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
のいくつかの`\u0066`使用方法を示します。これは、文字`f`"" のエスケープシーケンスです。 プログラムはに相当します。
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

### <a name="identifiers"></a>識別子

このセクションで指定されている識別子の規則は、Unicode 標準の説明書31で推奨されている規則に正確に対応しています。ただし、先頭文字 (C プログラミング言語では従来の) としてアンダースコアを使用できますが、Unicode エスケープシーケンスは識別子として許可され`@`、キーワードを識別子として使用できるようにするためのプレフィックスとして "" 文字を使用できます。

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

上記の Unicode 文字クラスの詳細については、Unicode Standard バージョン3.0 のセクション4.5 を参照してください。

有効な識別子の例と`identifier1`して`@if`は`_identifier2`、""、""、"" などがあります。

準拠するプログラムの識別子は、unicode 規格の使用方法15で定義されているように、Unicode 正規形 C で定義されている正規の形式である必要があります。 正規形 C ではない識別子に遭遇した場合の動作は、実装によって定義されます。ただし、診断は必要ありません。

プレフィックス "`@`" を使用すると、キーワードを識別子として使用できるようになります。これは、他のプログラミング言語とのやり取りに役立ちます。 この文字`@`は実際には識別子の一部ではないため、識別子は、プレフィックスなしで通常の識別子として他の言語で認識される可能性があります。 `@`プレフィックスを持つ識別子は、逐語的***識別子***と呼ばれます。 キーワードではない識別子に対してプレフィックスを使用することは許可されますが、スタイルの問題としては使用しないことを強くお勧めします。`@`

次に例を示します。
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
"" という名前`class`のパラメーター`bool`を受け取る "" と`static`いう名前の静的メソッドを使用して、"" という名前のクラスを定義します。 キーワードでは Unicode エスケープは許可されていないため、`cl\u0061ss`トークン "" は識別子であり、"`@class`" と同じ識別子です。

2つの識別子は、次の変換が適用された後に同一である場合は同じと見なされます。

*  プレフィックス "`@`" (使用されている場合) は削除されます。
*  各*unicode_escape_sequence*は、対応する unicode 文字に変換されます。
*  すべての*formatting_character*が削除されます。

2つの連続するアンダースコア文字`U+005F`() を含む識別子は、実装で使用するために予約されています。 たとえば、実装では、2つのアンダースコアで始まる拡張キーワードを使用できます。

### <a name="keywords"></a>キーワード

***キーワード***は、予約されている識別子に似た文字シーケンスであり、 `@`文字で始まる場合を除き、識別子として使用することはできません。

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

文法の一部では、特定の識別子は特別な意味を持ちますが、キーワードではありません。 このような識別子は、"コンテキストキーワード" と呼ばれることもあります。 たとえば、プロパティ宣言内では、"`get`" 識別子と "`set`" 識別子には特別な意味があります ([アクセサー](classes.md#accessors))。 または`get` `set`以外の識別子は、これらの場所では許可されないため、この使用は、これらの単語を識別子として使用する場合と競合しません。 他のケースでは、暗黙的に型指定`var`されたローカル変数宣言 ([ローカル変数宣言](statements.md#local-variable-declarations)) で識別子 "" が使用されているなどの場合、コンテキストキーワードは宣言された名前と競合する可能性があります。 このような場合、宣言された名前は、コンテキストキーワードとして識別子を使用するよりも優先されます。

### <a name="literals"></a>リテラル

***リテラル***は、値のソースコード表現です。

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

#### <a name="boolean-literals"></a>ブール型リテラル

ブール型リテラル値`true`には、と`false`の2つがあります。

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

*Boolean_literal*の型は `bool` です。

#### <a name="integer-literals"></a>整数リテラル

整数リテラルは、、 `int`、および`ulong`型`uint` `long`の値を書き込むために使用されます。 整数リテラルには、10進数と16進数という2つの形式があります。

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

整数リテラルの型は、次のように決定されます。

*  リテラルにサフィックスがない`int`場合は`long`、、 `uint`、、 `ulong`の各型の値を表すことができます。
*  リテラルのサフィックス`U`がまたは`u`の場合は、 `uint`、、 `ulong`の各型の値を表すことができます。
*  リテラルのサフィックス`L`がまたは`l`の場合は、 `long`、、 `ulong`の各型の値を表すことができます。
*  リテラル`UL`のサフィックスが`Ul` `uL`、 、、、、`ul`、、 `ulong`、または`lu`の場合、型はです。 `lU` `LU` `Lu`

整数リテラルによって表される値が`ulong`型の範囲外にある場合は、コンパイル時エラーが発生します。

スタイルの場合は、`L`""`l`の代わりに "" を使用して型`long`のリテラルを作成することをお勧めします。これは、文字`l`"" を数字 "`1`" と混同しやすいためです。

最小値と`long`最小値`int`を10進整数リテラルとして記述できるようにするには、次の2つの規則を使用します。

* 値が 2147483648 (2 ^ 31) で*integer_type_suffix*がない*decimal_integer_literal*が、単項マイナス演算子トークン ([単項マイナス演算子](expressions.md#unary-minus-operator)) の直後にトークンとして表示される場合、結果は `int` 型の定数になります。値-2147483648 (-2 ^ 31)。 その他のすべての状況では、このような*decimal_integer_literal*の型は `uint` です。
* *Decimal_integer_literal*の値が 9223372036854775808 (2 ^ 63) で、 *integer_type_suffix*または @no__t @no__t *integer_type_suffix*がない場合は、単項マイナス演算子トークンの直後にトークンとして表示されます ([単項マイナス演算子](expressions.md#unary-minus-operator)) の結果は、値-9223372036854775808 (-2 ^ 63) の `long` 型の定数になります。 その他のすべての状況では、このような*decimal_integer_literal*の型は `ulong` です。

#### <a name="real-literals"></a>実数リテラル

実数リテラルは、 `float` `double`、、および`decimal`型の値を書き込むために使用されます。

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

*Real_type_suffix*が指定されていない場合、実際のリテラルの型 @no__t は-1 になります。 それ以外の場合は、実際の型サフィックスによって、次のように実際のリテラルの型が決定されます。

*  `F`また`float`は`f`のサフィックスが付いた実数リテラルは型です。 たとえば`1f`、リテラル`float`、、、は`123.456F`すべて型です。 `1e10f` `1.5f`
*  `D`また`double`は`d`のサフィックスが付いた実数リテラルは型です。 たとえば`1d`、リテラル`double`、、、は`123.456D`すべて型です。 `1e10d` `1.5d`
*  `M`また`decimal`は`m`のサフィックスが付いた実数リテラルは型です。 たとえば`1m`、リテラル`decimal`、、、は`123.456M`すべて型です。 `1e10m` `1.5m` このリテラルは、正確な`decimal`値を取得することによって値に変換され、必要に応じて、銀行型丸め ([10 進数型](types.md#the-decimal-type)) を使用して最も近い表現可能な値に丸められます。 リテラル内のすべての小数点以下桁数は、値が丸められた場合、または値がゼロの場合 (後者の場合は符号と小数点以下桁数が0になります)、保持されます。 このため、リテラル`2.900m`は、符号`0`、係数`2900`、および小数点以下桁数`3`の10進数を形成するために解析されます。

指定されたリテラルを指定された型で表すことができない場合、コンパイル時エラーが発生します。

型または`float` `double`型の実数リテラルの値は、IEEE の "近似値に丸める" モードを使用して決定されます。

実際のリテラルでは、小数点の後には常に10進数が必要であることに注意してください。 たとえば、は`1.3F`実際の`1.F`リテラルですが、ではありません。

#### <a name="character-literals"></a>文字リテラル

文字リテラルは1つの文字を表し、通常はのよう`'a'`に引用符で囲まれた文字で構成されます。

メモ:ANTLR 文法表記法を使用すると、次のように混乱が生じます。 ANTLR では、これを`\'`記述すると、1つ`'`の引用符が使用されます。 また、記述`\\`する場合は、1つの`\`円記号を表します。 したがって、文字リテラルの最初の規則は、1つの引用符、1文字、1つの引用符で始まることを意味します。 また、11つの単純なエスケープ`\'`シーケンス`\"`は`\\`、 `\0` `\a` `\b`、、、、、、、、、、 `\t` `\f` `\n` `\r` `\v`.

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

`\`*文字*内の円記号 () の後に続く`'`文字は、、 `"` `\`、、 `b` `0` `a`、、、 `f`のいずれかである必要があります。, `n`, `r`, `t`, `u`, `U`, `x`, `v`. それ以外の場合は、コンパイル時のエラーが発生します。

16進数のエスケープシーケンスは、1つの Unicode 文字を表し、値は "`\x`" の後に16進数で形成されます。

文字リテラルによって表される値がより`U+FFFF`大きい場合、コンパイル時エラーが発生します。

文字リテラル内の unicode 文字エスケープシーケンス ([unicode 文字エスケープ](lexical-structure.md#unicode-character-escape-sequences)シーケンス) は、の範囲`U+0000` `U+FFFF`内である必要があります。

単純なエスケープシーケンスは、次の表に示すように、Unicode 文字エンコーディングを表します。


| __エスケープシーケンス__ | __文字名__ | __Unicode エンコード__ |
|---------------------|--------------------|----------------------|
| `\'`                | 単一引用符       | `0x0027`             | 
| `\"`                | 二重引用符       | `0x0022`             | 
| `\\`                | 円記号          | `0x005C`             | 
| `\0`                | [Null]               | `0x0000`             | 
| `\a`                | 警告              | `0x0007`             | 
| `\b`                | バックスペース          | `0x0008`             | 
| `\f`                | フォーム フィード          | `0x000C`             | 
| `\n`                | 改行           | `0x000A`             | 
| `\r`                | キャリッジ リターン    | `0x000D`             | 
| `\t`                | 水平タブ     | `0x0009`             | 
| `\v`                | 垂直タブ       | `0x000B`             | 

*Character_literal*の型は `char` です。

#### <a name="string-literals"></a>文字列リテラル

C#では、***正規文字列***リテラルと***逐語的文字列リテラル***という2つの形式の文字列リテラルがサポートされています。

通常の文字列リテラルは、のように`"hello"`、二重引用符で囲まれた0個以上の文字で構成されます。また、単純なエスケープシーケンス ( `\t`タブ文字のなど) と16進数および Unicode のエスケープシーケンスの両方を含めることができます。

逐語的文字列リテラルは、 `@`文字の後に二重引用符文字、0個以上の文字、および終わりの二重引用符文字で構成されます。 簡単な例を`@"hello"`次に示します。 逐語的文字列リテラルでは、区切り記号の間の文字は逐語的に解釈されます。唯一の例外は*quote_escape_sequence*です。 特に、単純なエスケープシーケンス、および16進数および Unicode のエスケープシーケンスは、逐語的文字列リテラルでは処理されません。 逐語的文字列リテラルは複数の行にまたがる場合があります。

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

*Regular_string_literal_character*の円記号 (`\`) の後に続く文字は、次の文字のいずれかでなければなりません: `'`、`"`、`\`、`0`、`a`、`b`、`f`、`n`、0、1、@no__-12、3、4、5。 それ以外の場合は、コンパイル時のエラーが発生します。

例
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
さまざまな文字列リテラルを示します。 最後の文字列リテラル`j`は、複数の行にまたがる逐語的文字列リテラルです。 引用符の間の文字 (改行文字などの空白文字を含む) は、そのまま保持されます。

16進数のエスケープシーケンスは数値の16進数を持つことができるため、 `"\x123"`文字列リテラルには16進値123の1文字が含まれます。 16進数値12の後に3文字目が続く文字を含む文字列を作成するに`"\x00123"`は`"\x12" + "3"` 、または代わりにを記述します。

*String_literal*の型は `string` です。

各文字列リテラルは、必ずしも新しい文字列インスタンスになるとは限りません。 文字列等値演算子 (文字列[等値演算子](expressions.md#string-equality-operators)) によって等価である2つ以上の文字列リテラルが同じプログラムに出現する場合、これらの文字列リテラルは同じ文字列インスタンスを参照します。 たとえば、によって生成される出力は、
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
は`True` 、2つのリテラルが同じ文字列インスタンスを参照しているためです。

#### <a name="interpolated-string-literals"></a>挿入文字列リテラル

挿入文字列リテラルは文字列リテラルに似`{`ていますが、と`}`で区切られた穴を含み、式が発生する可能性があります。 実行時には、式は、テキスト形式を、穴が発生した場所の文字列に置き換えることを目的として評価されます。 文字列補間の構文とセマンティクスについては、「」 (挿入[文字列](expressions.md#interpolated-strings)) を参照してください。

文字列リテラルと同様に、補間文字列リテラルは通常の場合も逐語的な場合もあります。 補間された正規文字列`$"`リテラル`"`はとで区切られ、その後の`$@"`文字列`"`リテラルはとで区切られます。

他のリテラルと同様に、補間文字列リテラルの字句解析では、次の文法に従って、最初に1つのトークンが生成されます。 ただし、構文分析の前に、補間文字列リテラルの1つのトークンは、穴を囲む文字列の部分に対して複数のトークンに分割されます。また、穴で発生する入力要素は、構文的に分析されされます。 これにより、より多くの補間文字列リテラルが生成される可能性がありますが、構文が正しければ、最終的には、最終的には構文分析のためにトークンのシーケンスが処理されます。

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

*Interpolated_string_literal*トークンは、 *interpolated_string_literal*で次のように複数のトークンとその他の入力要素として再解釈されます。

* 次の出現箇所は、個別の個別のトークンとして再解釈されます。先頭の `$` sign、 *interpolated_regular_string_whole*、 *interpolated_regular_string_start*、 *interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_whole*、 *interpolated_verbatim_string_start*、 *interpolated_verbatim_string_mid* 、および*interpolated_verbatim_string_end*。
* これらの間での*regular_balanced_text*と*verbatim_balanced_text*の出現は、 *input_section* ([字句分析](lexical-structure.md#lexical-analysis)) として再処理され、結果として得られる入力要素のシーケンスとして再解釈されます。 これらには、再解釈される補間文字列リテラルトークンが含まれている場合があります。

構文分析では、トークンが*interpolated_string_expression* ([補間文字列](expressions.md#interpolated-strings)) に再結合されます。

例 TODO


#### <a name="the-null-literal"></a>Null リテラル

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal*は、参照型または null 許容型に暗黙的に変換できます。

### <a name="operators-and-punctuators"></a>Operators と区切り記号

いくつかの種類の演算子と区切り記号があります。 演算子は、1つ以上のオペランドに関係する操作を記述するために、式で使用されます。 たとえば、という式`a + b`では`+` 、演算子を使用して`a` 、 `b`2 つのオペランドとを追加します。 区切り記号は、グループ化と分離のためのものです。

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

*Right_shift*と*right_shift_assignment*の各生産の縦棒は、構文文法の他の生産とは異なり、トークン間で任意の種類 (偶数ではない) の文字を使用できないことを示すために使用されます。 これらの生産は、 *type_parameter_list*s ([型パラメーター](classes.md#type-parameters)) の適切な処理を可能にするために、特別に処理されます。

## <a name="pre-processing-directives"></a>プリプロセスディレクティブ

プリプロセスディレクティブを使用すると、ソースファイルのセクションを条件付きでスキップしたり、エラーと警告の状態を報告したり、ソースコードの個別の領域を区切ることができます。 "プリプロセスディレクティブ" という用語は、C およびC++プログラミング言語との一貫性を保つためにのみ使用されます。 でC#は、独立した処理前の手順はありません。プリプロセスディレクティブは、字句解析フェーズの一部として処理されます。

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

次のプリプロセスディレクティブを使用できます。

*  `#define`と`#undef`は、それぞれ条件付きコンパイルシンボル ([宣言ディレクティブ](lexical-structure.md#declaration-directives)) を定義および未定義にするために使用されます。
*  `#if`、 `#elif`、 、および`#endif`。ソースコードのセクションを条件付きでスキップするために使用されます ([条件付きコンパイルディレクティブ](lexical-structure.md#conditional-compilation-directives))。 `#else`
*  `#line`。これは、エラーと警告 ([行ディレクティブ](lexical-structure.md#line-directives)) に対して出力される行番号を制御するために使用されます。
*  `#error`エラー `#warning`と警告を発行するために使用されるおよび ([診断ディレクティブ](lexical-structure.md#diagnostic-directives))。
*  `#region`および`#endregion`。ソースコード ([Region ディレクティブ](lexical-structure.md#region-directives)) のセクションを明示的にマークするために使用されます。
*  `#pragma`。コンパイラに省略可能なコンテキスト情報 ([プラグマディレクティブ](lexical-structure.md#pragma-directives)) を指定するために使用されます。

プリプロセスディレクティブは、常にソースコードの個別の行を占有し、常`#`に文字とプリプロセスディレクティブ名で始まります。 文字の`#`前と、 `#`文字とディレクティブ名の間に空白がある場合があります。

`#define` 、`#undef`、 、`#elif`、 、`#else`、、または`#endregion`ディレクティブを含むソース行は、単一行のコメントで終了する場合があります。 `#line` `#if` `#endif` 区切られたコメント`/* */` (コメントのスタイル) は、プリプロセスディレクティブを含むソース行では許可されていません。

プリプロセスディレクティブは、トークンではなく、のC#構文文法の一部ではありません。 ただし、プリプロセスディレクティブを使用して、トークンのシーケンスを含めたり除外したりすることができます。これC#により、プログラムの意味に影響を与えることができます。 たとえば、コンパイルされると、プログラムは次のようになります。
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
結果として、プログラムとまったく同じトークンのシーケンスが生成されます。
```csharp
class C
{
    void F() {}
    void I() {}
}
```

したがって、構文的には、2つのプログラムはまったく異なっていますが、構文は同じです。

### <a name="conditional-compilation-symbols"></a>[条件付きコンパイル シンボル]

`#if` 、`#elif`、 、および`#endif`ディレクティブによって提供される条件付きコンパイル機能は、前処理式 ([処理前の式](lexical-structure.md#pre-processing-expressions)) と条件付きの式によって制御されます。 `#else`コンパイルシンボル。

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

条件付きコンパイルシンボルには、***定義済み***または***未定義***の2つの状態があります。 ソースファイルの字句処理の開始時に、条件付きコンパイルシンボルは、外部機構によって明示的に定義されていない限り、未定義になります (コマンドラインコンパイラオプションなど)。 `#define`ディレクティブが処理されると、そのディレクティブで指定された条件付きコンパイルシンボルが、そのソースファイルで定義されます。 シンボルは、同じシンボルの`#undef`ディレクティブが処理されるまで、またはソースファイルの末尾に到達するまで定義されたままになります。 これは、1つの`#define`ソース`#undef`ファイル内のディレクティブとディレクティブが、同じプログラム内の他のソースファイルに影響を与えないことを意味します。

プリプロセス式で参照されている場合、定義済みの条件付きコンパイルシンボル`true`にはブール値が含まれ、未定義の条件`false`付きコンパイルシンボルにはブール値が含まれます。 条件付きコンパイルシンボルは、事前処理式で参照される前に明示的に宣言する必要はありません。 代わりに、宣言されていないシンボルは単に`false`未定義であるため、値が設定されます。

条件付きコンパイルシンボルの名前空間は、 C#プログラム内の他のすべての名前付きエンティティとは区別されます。 条件付きコンパイルシンボルは`#define` 、および`#undef`ディレクティブおよびプリプロセス式でのみ参照できます。

### <a name="pre-processing-expressions"></a>処理前の式

プリプロセス式は、ディレクティブおよび`#if` `#elif`ディレクティブで発生する可能性があります。 処理前`!`の式`!=`で`&&`は`||` 、、、、およびの各演算子`==`を使用できます。また、かっこを使用してグループ化することもできます。

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

プリプロセス式で参照されている場合、定義済みの条件付きコンパイルシンボル`true`にはブール値が含まれ、未定義の条件`false`付きコンパイルシンボルにはブール値が含まれます。

プリプロセス式の評価では、常にブール値が生成します。 プリプロセス式の評価の規則は、参照できるユーザー定義エンティティが条件付きコンパイルシンボルだけである点を除いて、定数式 ([定数](expressions.md#constant-expressions)式) の評価ルールと同じです。

### <a name="declaration-directives"></a>宣言ディレクティブ

宣言ディレクティブは、条件付きコンパイルシンボルを定義または未定義にするために使用されます。

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

ディレクティブを`#define`処理すると、指定された条件付きコンパイルシンボルが定義されます。これは、ディレクティブの後のソース行から始まります。 同様に、 `#undef`ディレクティブの処理によって、指定された条件付きコンパイルシンボルが未定義になります。これは、ディレクティブの後のソース行から始まります。

`#define`ソースファイル`#undef`内のディレクティブとディレクティブは、ソースファイル内の最初の*トークン* ([トークン](lexical-structure.md#tokens)) の前に実行する必要があります。それ以外の場合は、コンパイル時エラーが発生します。 直感的に言うと`#define` 、 `#undef`とディレクティブは、ソースファイル内の "実際のコード" の前に記述する必要があります。

次に例を示します。
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
は、ソースファイル`#define`内の最初のトークン`namespace` (キーワード) の前にディレクティブが指定されているため有効です。

次の例では、が`#define`実際のコードに従うため、コンパイル時エラーが発生します。
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

は`#define` 、既に定義されている条件付きコンパイルシンボルを定義できます`#undef`が、そのシンボルに介在するものはありません。 次の例では、条件付き`A`コンパイルシンボルを定義し、再度定義します。
```csharp
#define A
#define A
```

は`#undef` 、定義されていない条件付きコンパイルシンボルを "未定義" にすることがあります。 次の例では、条件付き`A`コンパイルシンボルを定義し、2回未定義`#undef`にします。2つ目は無効ですが、有効ではありません。
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>条件付きコンパイルディレクティブ

条件付きコンパイルディレクティブは、ソースファイルの一部を条件付きで含めたり除外したりするために使用されます。

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

構文で示されているように、条件付きコンパイルディレクティブは`#if` 、ディレクティブ、ディレクティブ、0個以上`#elif`のディレクティブ、0個または`#endif` 1 `#else`個のディレクティブ、およびディレクティブで構成されるセットとして記述する必要があります。 ディレクティブは、ソースコードの条件付きセクションです。 各セクションは、直前のディレクティブによって制御されます。 条件付きセクションには、これらのディレクティブが完全なセットを形成する、入れ子になった条件付きコンパイルディレクティブが含まれている場合があります。

*Pp_conditional*は、通常の字句処理のために、含まれている*conditional_section*を1つだけ選択します。

*  @No__t-1 ディレクティブと `#elif` ディレクティブの*pp_expression*は、1つのが @no__t 3 になるまで順番に評価されます。 式によって `true` が生成された場合、対応するディレクティブの*conditional_section*が選択されます。
*  すべての*pp_expression*が `false` を生成し、`#else` ディレクティブが存在する場合は、`#else` ディレクティブの*conditional_section*が選択されます。
*  それ以外の場合、 *conditional_section*は選択されません。

選択された*conditional_section*(存在する場合) は、通常の*input_section*として処理されます。セクションに含まれるソースコードは、字句文法に従う必要があります。トークンは、セクションのソースコードから生成されます。また、セクションのプリプロセスディレクティブには、所定の効果があります。

残りの*conditional_section*s (存在する場合) は、 *skipped_section*s として処理されます。ただし、プリプロセスディレクティブの場合は、セクション内のソースコードが字句文法に準拠している必要はありません。セクションのソースコードからトークンは生成されません。また、セクションのプリプロセスディレクティブは構文的に正しい必要がありますが、それ以外の場合は処理されません。 *Skipped_section*として処理されている*conditional_section*内では、入れ子になった*conditional_section*s (入れ子になった `#if`... `#endif` および `#region`... `#endregion` コンストラクトに含まれています) も skipped_ として処理されます。 *セクションを参照*してください。

次の例は、条件付きコンパイルディレクティブを入れ子にする方法を示しています。
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

プリプロセスディレクティブを除き、スキップされたソースコードは字句解析の対象になりません。 たとえば、次の例では、 `#else`セクションに未終了のコメントがあるにもかかわらず有効です。
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

ただし、ソースコードのスキップされたセクションでも、プリプロセスディレクティブは構文的に正しい必要があることに注意してください。

プリプロセスディレクティブは、複数行の入力要素内に出現する場合は処理されません。 たとえば、次のようなプログラムがあるとします。
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
結果は次のようになります。
```console
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

特別なケースでは、処理される前処理ディレクティブのセットは、 *pp_expression*の評価によって異なる場合があります。 次に例を示します。
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
は`class` 、 `X`が定義されているかどうかに関係なく、常に同じトークンストリーム ( `Q` `{` `}`) を生成します。 が`X`定義されている場合は、 `#if`複数`#endif`行のコメントにより、処理されるディレクティブはとだけです。 が`X`定義されていない場合`#if`、3 `#endif`つのディレクティブ (、 `#else`、) がディレクティブセットの一部になります。

### <a name="diagnostic-directives"></a>診断ディレクティブ

診断ディレクティブは、エラーメッセージと警告メッセージを明示的に生成するために使用されます。このメッセージは、他のコンパイル時のエラーや警告と同じ方法で報告されます。

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

次に例を示します。
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
は常に警告 ("チェックイン前に必要なコードレビュー") を生成し、条件付きシンボル`Debug`とが両方と`Retail`も定義されている場合は、コンパイル時エラー ("ビルドはデバッグとリテールの両方にすることはできません") を生成します。 *Pp_message*には任意のテキストを含めることができることに注意してください。具体的には、`can't` という語の単一引用符で示されているように、適切な形式のトークンが含まれている必要はありません。

### <a name="region-directives"></a>Region ディレクティブ

リージョンディレクティブは、ソースコードの領域を明示的にマークするために使用されます。

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

領域には意味の意味がありません。地域は、プログラマが使用するためのものであり、ソースコードのセクションをマークする自動化ツールによって使用されます。 `#region`または`#endregion`ディレクティブで指定されたメッセージは、同様に意味がありません。リージョンを識別するためにのみ機能します。 @No__t-0 および `#endregion` ディレクティブは、異なる*pp_message*s を持つことができます。

領域の字句処理は次のとおりです。
```csharp
#region
...
#endregion
```
は、次の形式の条件付きコンパイルディレクティブの字句処理に正確に対応します。
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>行ディレクティブ

行ディレクティブを使用すると、コンパイラによって警告やエラーなどの出力で報告され、呼び出し元情報属性 ([呼び出し元情報属性](attributes.md#caller-info-attributes)) で使用される行番号とソースファイル名を変更できます。

行ディレクティブは、他のテキスト入力からソースコードを生成C#するメタプログラミングツールで最もよく使用されます。

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

ディレクティブが`#line`存在しない場合、コンパイラは出力に実際の行番号とソースファイル名を報告します。 @No__t 2 ではない*line_indicator*を含む @no__t 0 ディレクティブを処理する場合、コンパイラは、ディレクティブの後の行を、指定された行番号 (および指定されている場合はファイル名) として処理します。

ディレクティブ`#line default`は、前のすべての #line ディレクティブの効果を反転させます。 コンパイラは、ディレクティブが`#line`処理されていないかのように、後続の行の真の行情報を報告します。

ディレクティブ`#line hidden`は、エラーメッセージで報告されるファイルと行番号には影響しませんが、ソースレベルのデバッグに影響します。 デバッグ時に、 `#line hidden`ディレクティブとそれ以降`#line`の`#line hidden`ディレクティブの間のすべての行に行番号情報がありません。 デバッガーでコードをステップ実行すると、これらの行は完全にスキップされます。

なお、 *file_name*されるエスケープ文字は処理されません通常の文字列リテラルと異なります、"`\`"文字が単純に内での通常のバック スラッシュ文字を指定、 *file_name*。

### <a name="pragma-directives"></a>プラグマディレクティブ

プリ`#pragma`プロセスディレクティブは、コンパイラに対して省略可能なコンテキスト情報を指定するために使用されます。 `#pragma`ディレクティブで指定された情報は、プログラムのセマンティクスを変更することはありません。

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C#コンパイラ`#pragma`の警告を制御するディレクティブを提供します。 言語の将来のバージョンには、 `#pragma`追加のディレクティブが含まれる場合があります。 他のC#コンパイラとの相互運用性を確保C#するために、Microsoft コンパイラは不明`#pragma`なディレクティブに対してコンパイルエラーを発行しません。ただし、このようなディレクティブでは警告が生成されます。

#### <a name="pragma-warning"></a>プラグマの警告

ディレクティブ`#pragma warning`は、後続のプログラムテキストのコンパイル中に、すべてまたは特定の警告メッセージのセットを無効化または復元するために使用されます。

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

警告`#pragma warning`リストを省略するディレクティブは、すべての警告に影響します。 警告`#pragma warning`リストを含むディレクティブは、一覧に指定されている警告のみに影響します。

ディレクティブ`#pragma warning disable`は、すべてまたは特定の警告のセットを無効にします。

ディレクティブ`#pragma warning restore`は、すべてまたは特定の警告のセットを、コンパイル単位の開始時に有効だった状態に復元します。 特定の警告が外部で無効にされた`#pragma warning restore`場合、(すべての警告または特定の警告について) によって警告が再度有効になるわけではないことに注意してください。

次の`#pragma warning`例では、を使用して、古いメンバーが参照されたときに、Microsoft C#コンパイラの警告番号を使用して報告された警告を一時的に無効にします。
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
