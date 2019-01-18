---
ms.openlocfilehash: f797692cbd6aeb6035aa7c8ed3139740466c6e42
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229841"
---
# <a name="lexical-structure"></a>字句構造

## <a name="programs"></a>Programs

C#***プログラム***1 つまたは複数から成る***ソース ファイル***正式と呼ばれる、***コンパイル単位***([コンパイル単位](namespaces.md#compilation-units))。 ソース ファイルは、Unicode 文字の順序付けられたシーケンスです。 ソース ファイルでは、ファイル システムで、ファイルと一対一の対応を通常されましたが、この対応は必要ありません。 最大の移植性、お勧めファイル システム内のファイルが、utf-8 でエンコードするエンコーディングします。

概念的には、3 つの手順を使用してプログラムのコンパイルします。

1. 変換ファイルを特定の文字のレパートリーとエンコード体系を Unicode 文字のシーケンスに変換します。
2. 字句解析、トークンのストリームに Unicode の入力文字のストリームを変換します。
3. 構文解析、実行可能コードにトークンのストリームを変換します。

## <a name="grammars"></a>文法

この仕様は、c# プログラミング言語の 2 つの文法を使用して構文を表示します。 ***字句文法***([字句文法](lexical-structure.md#lexical-grammar)) 行終端記号、空白、コメント、トークン、およびプリプロセッサ ディレクティブに Unicode 文字を結合する方法を定義します。 ***構文文法***([構文文法](lexical-structure.md#syntactic-grammar)) 字句文法に起因するトークンを組み合わせて c# プログラムを構成する方法を定義します。

### <a name="grammar-notation"></a>文法の表記

字句および構文の文法は、ANTLR 文章校正ツールの表記を使用してバッカスナウア記法の形式で表示されます。

### <a name="lexical-grammar"></a>字句文法

C# の構文文法を示した[字句解析](lexical-structure.md#lexical-analysis)、[トークン](lexical-structure.md#tokens)、および[プリプロセッサ ディレクティブ](lexical-structure.md#pre-processing-directives)します。 字句文法の終端記号は、Unicode 文字セットの文字と構文の文法では、トークンを構成する文字を集計する方法を指定します ([トークン](lexical-structure.md#tokens))、空白文字 ([空白](lexical-structure.md#white-space))、コメント ([コメント](lexical-structure.md#comments))、およびプリプロセッサ ディレクティブ ([プリプロセッサ ディレクティブ](lexical-structure.md#pre-processing-directives))。

C# プログラムですべてのソース ファイルに準拠する必要があります、*入力*字句文法の実稼働 ([字句解析](lexical-structure.md#lexical-analysis))。

### <a name="syntactic-grammar"></a>構文文法

この章に従います付録では、c# の構文の文法が表示されます。 構文文法のターミナル シンボルは、構文の文法で定義されているトークンと構文の文法では、トークンを組み合わせて c# プログラムを構成する方法を指定します。

すべてのソース ファイルで、C#にプログラムが準拠する必要があります、 *compilation_unit*構文文法の実稼働 ([コンパイル単位](namespaces.md#compilation-units))。

## <a name="lexical-analysis"></a>字句解析

*入力*運用 c# ソース ファイルの構文構造を定義します。 C# プログラムでは、各ソース ファイルは、この構文の文法の実稼働環境に従う必要があります。

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

5 つの基本的な要素は、c# ソース ファイルの構文構造を構成します。行ターミネータ ([行ターミネータ](lexical-structure.md#line-terminators))、空白文字 ([空白](lexical-structure.md#white-space))、コメント ([コメント](lexical-structure.md#comments))、トークン ([トークン](lexical-structure.md#tokens))、およびプリプロセッサ ディレクティブ ([プリプロセッサ ディレクティブ](lexical-structure.md#pre-processing-directives))。 これらの基本要素のトークンだけが、c# プログラムの構文の文法で重要です ([構文文法](lexical-structure.md#syntactic-grammar))。

C# ソース ファイルの構文の処理、構文分析への入力になるトークンのシーケンスに、ファイルを減らすことで構成されます。 トークンを区切る行の終端記号、空白文字、およびコメントを使用できるとプリプロセッサ ディレクティブがスキップされるソース ファイルのセクションが発生することができますが、それ以外の場合これらの構文要素ありません影響 c# プログラムの構文構造に。

補間文字列リテラルの場合 ([リテラル文字列の補間](lexical-structure.md#interpolated-string-literals)) 1 つのトークンの字句解析、によって最初に生成されるが、繰り返しの字句解析を対象となるいくつかの入力要素に分割されますまで、すべての補間文字列リテラルが解決されました。 結果として得られるトークンは、構文分析への入力として使用します。

いくつかの構文文法には、ソース ファイル内の文字のシーケンスが一致すると、最も長い可能な構文要素を形成字句処理は常にします。 たとえば、文字シーケンス`//`が、その構文の要素が 1 つを超えるため、単一行コメントの始まりとして処理される`/`トークンです。

### <a name="line-terminators"></a>行ターミネータ

行ターミネータは、c# ソース ファイルの文字を行に分割します。

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

ソースとの互換性をコード ファイルの終わりのマーカーを追加している編集ツールとのシーケンスとして正しく表示するファイルをソースを有効にするには、行を終了した、c# プログラムでソース ファイルごとに順番に、次の変換が適用します。

*  ソース ファイルの最後の文字がコントロール Z の文字 (`U+001A`)、この文字を削除します。
*  復帰文字 (`U+000D`) とそのソース ファイルが空でない場合は、ソース ファイルの最後の文字は、キャリッジ リターンがない場合は、ソース ファイルの末尾に追加されます (`U+000D`)、ライン フィード (`U+000A`)、行区切り記号 (`U+2028`)、または段落区切り記号 (`U+2029`)。

### <a name="comments"></a>コメント

コメントの 2 つの形式がサポートされています。 単一行コメントおよび区切り記号付きのコメント。 ***単一行コメント***最初の文字`//`およびソース行の末尾に拡張します。 ***コメントの区切り***最初の文字`/*`文字で終わる`*/`します。 区切り記号付きコメントは、複数行にわたる可能性があります。

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
    : '/*' delimited_comment_section* asterisk* '/'
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

コメントを入れ子にしないでください。 文字シーケンス`/*`と`*/`内で特別な意味があるない、`//`コメント、および文字のシーケンス`//`と`/*`区切られたコメントの内部で特別な意味があるありません。

コメントは、文字と文字列リテラル内では処理されません。

例では、
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
区切り記号付きのコメントが含まれています。

例では、
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
いくつかの単一行コメントを示しています。

### <a name="white-space"></a>空白

空白文字は Unicode クラス (Zs の空白文字を含む) を使用して任意の文字だけでなく、水平タブ文字、垂直タブ文字、およびフォーム フィード文字として定義されます。

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>トークン

トークンのいくつかの種類があります。 識別子、キーワード、リテラル、演算子、および区切り記号。 空白とコメントは、トークンが、トークンの区切り記号として。

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

### <a name="unicode-character-escape-sequences"></a>Unicode 文字のエスケープ シーケンス

Unicode 文字のエスケープ シーケンスは、Unicode 文字を表します。 Unicode 文字のエスケープ シーケンスは、識別子で処理されます ([識別子](lexical-structure.md#identifiers))、文字リテラル ([文字リテラル](lexical-structure.md#character-literals))、および通常の文字列リテラル ([リテラル文字列](lexical-structure.md#string-literals)). Unicode 文字のエスケープは、その他の場所 (たとえば、演算子などの区切り記号、またはキーワードを形成する) では処理されません。

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Unicode エスケープ シーケンスは、16 進数の数値以下で構成される 1 つの Unicode 文字を表す、"`\u`「または」`\U`"文字です。 C# 文字と文字列値では Unicode コード ポイントの 16 ビットのエンコーディングを使用するため、u+10000 U + 10 ffff の範囲の Unicode 文字は文字リテラルでは許可されていませんし、Unicode サロゲート ペアを使用して、文字列リテラルで表されます。 0x10ffff まで上記のコード ポイントを使用した Unicode 文字がサポートされていません。

複数の翻訳は行われません。 たとえば、文字列リテラル"`\u005Cu005C`「は等価である」`\u005C`「なく」`\`"。 Unicode 値`\u005C`文字"`\`"。

例では、
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
いくつか示されて`\u0066`、これは、文字のエスケープ シーケンス"`f`"。 プログラムと同じです。
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

このセクションで指定された識別子の規則は、点を除いて、アンダー スコアが Unicode のエスケープ シーケンスは、(同様に、C プログラミング言語で従来は) 初期文字として許可されると正確に、Unicode Standard Annex 31 で、推奨される対応します。識別子で許可されていると、"`@`"を識別子として使用するキーワードを有効にプレフィックスとして使用できる文字。

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

上記で説明した Unicode 文字クラスについては、Unicode 標準、バージョン 3.0、4.5 のセクションを参照してください。

有効な識別子の例として、"`identifier1`「,」`_identifier2`"、および"`@if`"。

準拠したプログラムで識別子は、Unicode Standard Annex 15 で定義されている Unicode 正規形 C で定義された正規の形式でなければなりません。 正規形 C ではなく識別子が検出されたときの動作は実装で定義されます。ただし、診断は必要ありません。

プレフィックス"`@`"他のプログラミング言語と接続した場合に便利です、識別子としてのキーワードの使用を有効にします。 文字`@`他の言語では、プレフィックスなしの通常の識別子として認識されるため、識別子の一部ではありません。 識別子、`@`プレフィックスと呼ばれる、***逐語的識別子***します。 使用、`@`キーワードではない識別子のプレフィックスは許可されていますが、スタイルの問題としてお勧めします。

例:
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
という名前のクラスを定義します。"`class`という名前の静的メソッド"と"`static`"をという名前のパラメーターを受け取る"`bool`"。 Unicode エスケープするため、トークンのキーワードでは使用できませんに注意してください"`cl\u0061ss`「識別子、およびと同じ識別子が」`@class`"。

順序で、次の変換が適用される結果が同一の場合は、2 つの識別子は同じで考えられます。

*  プレフィックス"`@`"を使用する場合は、削除されます。
*  各*unicode_escape_sequence*対応する Unicode 文字に変換されます。
*  すべて*formatting_character*s に削除されます。

識別子を含む 2 つの連続するアンダー スコア文字 (`U+005F`) 実装で使用するために予約されています。 たとえば、実装は、2 つのアンダー スコアで始まる拡張のキーワード。

### <a name="keywords"></a>キーワード

A***キーワード***識別子のような文字シーケンスは、予約されていると、前に付けない限り識別子として使用できませんが、`@`文字。

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

文法でいくつかの場所では、特定の識別子は、特殊な意味があるが、キーワードではありません。 このような識別子は、「コンテキスト キーワード」とも呼ばれます。 プロパティの宣言内など、"`get`「と」`set`"識別子が特別な意味を持ちます ([アクセサー](classes.md#accessors))。 以外の識別子`get`または`set`識別子としてこれらの単語の使用量がこのような使用が競合しないようにこれらの場所では許可されません。 それ以外の場合によっては、識別子と"`var`"暗黙的に型指定されたローカル変数宣言で ([ローカル変数宣言](statements.md#local-variable-declarations))、コンテキスト キーワードは宣言された名前と競合することです。 このような場合は、宣言された名前は、コンテキスト キーワードとして識別子の使用に優先します。

### <a name="literals"></a>リテラル

A***リテラル***値のソース コードの表現です。

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

2 つのブール型リテラル値がある:`true`と`false`します。

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

型を*boolean_literal*は`bool`します。

#### <a name="integer-literals"></a>整数リテラル

整数リテラルの型の値を記述に使用`int`、 `uint`、 `long`、および`ulong`します。 整数リテラルがある 2 つの可能な形式: 10 進数と 16 進数。

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

*  リテラルにサフィックスがあるない場合は、最初の値を表す型。 `int`、 `uint`、 `long`、`ulong`します。
*  リテラルが付いている場合`U`または`u`、最初の値を表す型があります: `uint`、`ulong`します。
*  リテラルが付いている場合`L`または`l`、最初の値を表す型があります: `long`、`ulong`します。
*  リテラルが付いている場合`UL`、 `Ul`、 `uL`、 `ul`、 `LU`、 `Lu`、 `lU`、または`lu`、型である`ulong`します。

範囲の整数リテラルで表される値が、`ulong`入力すると、コンパイル時エラーが発生します。

スタイルの問題、としては推奨される"`L`「の代わりに使用する」`l`"型のリテラルの書き込み時に`long`文字を混同しないでくださいであるため、"`l`数字の"と"`1`"。

値の最小値を許可するように`int`と`long`、10 進整数リテラルとして記述する、次の 2 つの規則が存在する値。

* ときに、 *decimal_integer_literal* 2147483648 値 (2 ^31) および no *integer_type_suffix*単項マイナス演算子トークンの直後に続くトークンとして表示されます ([単項マイナス演算子](expressions.md#unary-minus-operator))、結果は型の定数`int`値-2147483648 (-2 ^31)。 その他のすべての状況でこのような*decimal_integer_literal*の種類は`uint`します。
* ときに、 *decimal_integer_literal* 9223372036854775808 値 (2 ^63) および no *integer_type_suffix*または*integer_type_suffix* `L`または`l`単項マイナス演算子トークンの直後に続くトークンとして表示されます ([単項マイナス演算子](expressions.md#unary-minus-operator))、結果は型の定数`long`値-9223372036854775808 (-2 ^63)。 その他のすべての状況でこのような*decimal_integer_literal*の種類は`ulong`します。

#### <a name="real-literals"></a>実際のリテラル

実際のリテラルの型の値を記述に使用`float`、 `double`、および`decimal`します。

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

ない場合は*real_type_suffix*を指定すると、実際のリテラルの型は`double`します。 それ以外の場合、実数のリテラルの種類を次のように決定実数型のサフィックス。

*  実数のリテラル サフィックス`F`または`f`の種類は`float`します。 たとえば、リテラルは、 `1f`、 `1.5f`、`1e10f`と`123.456F`すべて型`float`。
*  実数のリテラル サフィックス`D`または`d`の種類は`double`します。 たとえば、リテラルは、 `1d`、 `1.5d`、`1e10d`と`123.456D`すべて型`double`。
*  実数のリテラル サフィックス`M`または`m`の種類は`decimal`します。 たとえば、リテラルは、 `1m`、 `1.5m`、`1e10m`と`123.456M`すべて型`decimal`。 このリテラルに変換を`decimal`、正確な値を取得し、必要に応じてを使用して最も近い表現可能な値に丸め値銀行型丸めが ([10 進数型](types.md#the-decimal-type))。 値が丸められますか、値が 0 (この後者の場合、記号と小数点 0 になります) しない限り、リテラルのスケールが保持されます。 そのため、リテラル`2.900m`記号と小数点以下のフォームに解析`0`、係数`2900`、有効桁数と小数点`3`します。

指定されたリテラルは、示された型で表すことができない、コンパイル時エラーが発生します。

型の実数のリテラル値`float`または`double`IEEE を使用して決定されます"round を最も近い"モードです。

実数のリテラルに桁が常に必要である、小数点より後に注意してください。 たとえば、`1.3F`が実数値リテラルですが`1.F`はありません。

#### <a name="character-literals"></a>文字リテラル

文字リテラルが単一の文字を表し、通常ように、引用符で囲まれた文字で構成されます`'a'`します。

メモ:ANTLR 文法の表記では、次を混乱に! 記述するときに、ANTLR`\'`の単一引用符現時点では`'`します。 記述するときと`\\`現時点では、1 つの円記号の`\`します。 そのため、単一引用符、文字、単一引用符で始まるリテラル文字の最初の規則を意味します。 11 個の可能な単純なエスケープ シーケンスと`\'`、 `\"`、 `\\`、 `\0`、 `\a`、 `\b`、 `\f`、 `\n`、 `\r`、 `\t`、 `\v`.

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

円記号に続く文字 (`\`) で、*文字*、次の文字のいずれかを指定する必要があります: `'`、 `"`、 `\`、 `0`、 `a`、 `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. それ以外の場合は、コンパイル時のエラーが発生します。

16 進数のエスケープ シーケンスは、16 進数の数値以下で構成された値を 1 つの Unicode 文字を表します"`\x`"。

リテラル文字で表される値がより大きいかどうか`U+FFFF`コンパイル時エラーが発生します。

Unicode 文字のエスケープ シーケンス ([Unicode 文字のエスケープ シーケンス](lexical-structure.md#unicode-character-escape-sequences)) リテラル文字の範囲で指定する必要があります`U+0000`に`U+FFFF`します。

次の表で説明されているように、単純なエスケープ シーケンスは、Unicode 文字のエンコーディングを表します。


| __エスケープ シーケンス__ | __文字の名前__ | __Unicode エンコーディング__ |
|---------------------|--------------------|----------------------|
| `\'`                | 単一引用符       | `0x0027`             | 
| `\"`                | 二重引用符       | `0x0022`             | 
| `\\`                | 円記号          | `0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | 警告              | `0x0007`             | 
| `\b`                | バックスペース          | `0x0008`             | 
| `\f`                | フォーム フィード          | `0x000C`             | 
| `\n`                | 改行           | `0x000A`             | 
| `\r`                | キャリッジ リターン    | `0x000D`             | 
| `\t`                | 水平タブ     | `0x0009`             | 
| `\v`                | 垂直タブ       | `0x000B`             | 

型を*character_literal*は`char`します。

#### <a name="string-literals"></a>文字列リテラル

C# では、文字列リテラルの 2 つの形式:***標準文字列リテラル***と***verbatim 文字列リテラル***します。

標準リテラル文字列としての二重引用符で囲まれた 0 個以上の文字から成る`"hello"`、両方の単純なエスケープ シーケンスを含めることができ、(など`\t`でタブ文字)、16 進数と Unicode のエスケープ シーケンス。

Verbatim 文字列リテラルから成る、`@`文字の後に二重引用符文字、0 個以上の文字と終わりの二重引用符文字。 簡単な例は、`@"hello"`します。 逐語的文字列リテラル、区切り記号の間の文字は、そのまま解釈されている唯一の例外を*quote_escape_sequence*します。 具体的には、単純なエスケープ シーケンス、および 16 進数、および Unicode のエスケープ シーケンスは、verbatim 文字列リテラルでは処理されません。 Verbatim 文字列リテラルは、複数行にわたる可能性があります。

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

円記号に続く文字 (`\`) で、 *regular_string_literal_character* 、次の文字のいずれかを指定する必要があります: `'`、 `"`、 `\`、 `0`、 `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. それ以外の場合は、コンパイル時のエラーが発生します。

例では、
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
さまざまな文字列リテラルを示しています。 最後の文字列リテラル、 `j`、逐語的文字列リテラルは複数の行にまたがるには。 改行文字などの空白を含む、引用符の間の文字をそのまま維持されます。

16 進数のエスケープ シーケンスは、文字列リテラル、16 進数が変数を持つことができますので`"\x123"`16 進値 123 の 1 つの文字が含まれています。 を 16 進値 12 が 3 文字が続く文字を含む文字列を作成する 1 つ記述`"\x00123"`または`"\x12" + "3"`代わりにします。

型を*string_literal*は`string`します。

各文字列リテラルは、新しい文字列インスタンスで必ずしも発生するされません。 2 つ以上の文字列リテラルが場合に応じて、文字列の等値演算子と同じです ([文字列等値演算子](expressions.md#string-equality-operators)) これらの文字列リテラルは、同じ文字列インスタンスを参照してください、同じプログラムで表示されます。 によって生成された出力インスタンス、
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
`True`のため、2 つのリテラルは、同じ文字列インスタンスを参照してください。

#### <a name="interpolated-string-literals"></a>補間文字列リテラル

補間文字列リテラルは、文字列リテラルに似ていますで区切られた穴が含まれて`{`と`}`式に、発生することができます。 実行時に穴が発生した位置に文字列に代入されるテキストがフォームが目的に設定された式を評価します。 構文とセマンティクスの文字列補間がセクションで説明されている ([文字列補間](expressions.md#interpolated-strings))。

文字列リテラルのように、補間文字列リテラルは正規表現または逐語的のいずれかを指定できます。 挿入の標準文字列リテラルを区切ることによって`$"`と`"`、補間の逐語的文字列リテラルを区切ることによって、`$@"`と`"`します。

など、他のリテラル、補間文字列リテラルの字句解析は、最初に以下の文法に従って、1 つのトークンの結果します。 ただし、構文解析前に、補間文字列リテラルの 1 つのトークンはセキュリティ ホールを囲む文字列の部品用のいくつかのトークンに分割、ホールで発生している入力の要素がもう一度分析構文。 これにより、処理するには、修正は、構文の解析を処理するためのトークンのシーケンスに最終的につながる場合、構文的に補間文字列リテラルよりさらに生成可能性があります。

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

*Interpolated_string_literal*トークンが複数のトークンおよびその他の入力要素を次のように内に見つかった位置の順序で解釈、 *interpolated_string_literal*:

* 次の出現回数が個別の個々 のトークンとして再解釈: 先頭`$`記号、 *interpolated_regular_string_whole*、 *interpolated_regular_string_start*、*interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_whole*、 *interpolated_verbatim_string_start*、 *interpolated_verbatim_string_mid*と*interpolated_verbatim_string_end*します。
* 出現*regular_balanced_text*と*verbatim_balanced_text*としてこれらの間は再処理する*input_section* ([字句解析](lexical-structure.md#lexical-analysis)) と、入力要素の結果として得られるシーケンスとして解釈されます。 加え、補間文字列リテラル トークンに含まれます。

構文解析にトークンを再結合、 *interpolated_string_expression* ([文字列補間](expressions.md#interpolated-strings))。

TODO の例


#### <a name="the-null-literal"></a>Null リテラル

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal*参照型または null 許容型に暗黙的に変換できます。

### <a name="operators-and-punctuators"></a>演算子と区切り記号

これには演算子と区切り記号のいくつかの種類があります。 演算子は、1 つまたは複数のオペランドに関連する操作を記述する式で使用されます。 たとえば、式`a + b`を使用して、 `+` 2 つのオペランドを追加する演算子`a`と`b`します。 区切り記号はグループ化および分離することです。

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

垂直のバーで、 *right_shift*と*right_shift_assignment*生産を使用するには、構文の文法では、任意の種類の文字がないのでは、他の生産とは異なり (あって空白文字) は、トークンの間に許可します。 それらの生産がの適切な処理を有効にするために特別に扱われます*type_parameter_list*s ([パラメーター入力](classes.md#type-parameters))。

## <a name="pre-processing-directives"></a>プリプロセッサ ディレクティブ

プリプロセッサ ディレクティブは、条件付きでソース ファイル、レポートのエラーおよび警告の条件のセクションをスキップして、ソース コードの異なる領域を区切るために機能を提供します。 「プリプロセッサ ディレクティブ」という用語は、C および C++ のプログラミング言語との整合性のみ使用されます。 C# の場合は、別の前処理ステップ; はありません。プリプロセッサ ディレクティブは、字句解析フェーズの一環として処理されます。

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

次のプリプロセッサ ディレクティブを使用できます。

*  `#define` `#undef`を定義し、それぞれ、条件付きコンパイル シンボルを未定義に使用されます ([宣言ディレクティブ](lexical-structure.md#declaration-directives))。
*  `#if`、 `#elif`、 `#else`、および`#endif`、条件付きでソース コードのセクションをスキップする ([条件付きコンパイル ディレクティブ](lexical-structure.md#conditional-compilation-directives))。
*  `#line`、行番号のエラーと警告の表示の制御に使用される ([ディレクティブの行](lexical-structure.md#line-directives))。
*  `#error` `#warning`、それぞれ、エラーと警告の発行に使用する ([診断ディレクティブ](lexical-structure.md#diagnostic-directives))。
*  `#region` `#endregion`、ソース コードのセクションを明示的にマークに使用されます ([領域ディレクティブ](lexical-structure.md#region-directives))。
*  `#pragma`、コンパイラ オプションのコンテキスト情報の指定に使用される ([プラグマ ディレクティブ](lexical-structure.md#pragma-directives))。

プリプロセッサ ディレクティブが常にソース コードの別の行を占有し、常に始まり、`#`文字と前処理ディレクティブ名。 空白文字が前に発生する、`#`文字、および、`#`文字とディレクティブの名前。

含むソース行、 `#define`、 `#undef`、 `#if`、 `#elif`、 `#else`、 `#endif`、 `#line`、または`#endregion`ディレクティブは、単一行コメントとなる可能性があります。 コメントの区切り (、`/* */`形式のコメント) は、プリプロセッサ ディレクティブを含むソース行では許可されていません。

プリプロセッサ ディレクティブは、トークンは、c# の構文の文法の一部ではないです。 ただし、プリプロセッサ ディレクティブまたはトークンのシーケンスを除外するために使用して、その方法でに影響する c# プログラムの意味。 たとえば、コンパイルされると、プログラム。
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
プログラムとトークンの正確な順序で結果:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

つまり、2 つのプログラムは、構文的には、まったく異なる構文的には同じです。

### <a name="conditional-compilation-symbols"></a>[条件付きコンパイル シンボル]

条件付きコンパイルの機能、 `#if`、 `#elif`、 `#else`、および`#endif`ディレクティブは処理前の式によって制御されます ([プリプロセス式](lexical-structure.md#pre-processing-expressions))条件付きコンパイル シンボルを選択します。

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

条件付きコンパイル シンボルが 2 つの状態:***定義***または***未定義***します。 ソース ファイルの構文の処理の先頭には、(コマンド ライン コンパイラ オプション) などの外部機構によって明示的に定義されている場合を除き、条件付きコンパイル シンボルは定義されません。 ときに、`#define`ディレクティブが処理されると、そのソース ファイルでそのディレクティブで、条件付きコンパイル シンボルが定義されています。 シンボルは削除されるまで、`#undef`同じシンボルが処理されるか、ソース ファイルの末尾に到達するまでディレクティブ。 この意味は`#define`と`#undef`同じプログラム内で他のソース ファイルに 1 つのソース ファイルでディレクティブの影響がありません。

処理前の式で参照されている、定義済みの条件付きコンパイル シンボルがブール値`true`、ブール値であり、未定義の条件付きコンパイル シンボル`false`します。 要件はありません、条件付きコンパイル シンボルを明示的に宣言処理前の式で参照される前にします。 宣言されていないシンボルが代わりに、単に定義されていませんし、値があるため`false`します。

条件付きコンパイル シンボルの名前空間と区別され、c# プログラムの他のすべての名前付きエンティティから分離します。 条件付きコンパイル シンボルでのみ参照できます`#define`と`#undef`ディレクティブと、処理前の式で。

### <a name="pre-processing-expressions"></a>処理前の式

処理前の式が発生することが`#if`と`#elif`ディレクティブ。 演算子`!`、 `==`、 `!=`、`&&`と`||`処理前の式では許可されてあり、グループ化かっこを使用できます。

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

処理前の式で参照されている、定義済みの条件付きコンパイル シンボルがブール値`true`、ブール値であり、未定義の条件付きコンパイル シンボル`false`します。

常に、処理前の式の評価は、ブール値を生成します。 処理前の式の評価の規則は、定数式の場合と同じ ([定数式](expressions.md#constant-expressions))、参照できるのみ、ユーザー定義エンティティは、条件付きコンパイル シンボル.

### <a name="declaration-directives"></a>宣言ディレクティブ

宣言のディレクティブは、定義または条件付きコンパイル シンボルを未定義に使用されます。

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

処理、`#define`ディレクティブにより、定義する特定の条件付きコンパイル シンボル ディレクティブに続くソース行から開始します。 同様の処理、`#undef`ディレクティブとディレクティブを次のソース行で始まる、未定義になる特定の条件付きコンパイル シンボル。

すべて`#define`と`#undef`ソース ファイルでディレクティブは、最初の前に出現する必要があります*トークン*([トークン](lexical-structure.md#tokens)) は、ソース ファイルでそれ以外の場合、コンパイル時エラーが発生します。 直感的な用語で`#define`と`#undef`ディレクティブがソース ファイルの「実際のコード」を付ける必要があります。

例:
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
有効ではため、`#define`ディレクティブの前に、最初のトークン (、`namespace`キーワード) でソース ファイル。

次の例では、コンパイル時エラーのため、結果を`#define`実際のコードします。
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

A`#define`既に定義されている、すべての介在が存在せず、条件付きコンパイル シンボルを定義することがあります`#undef`そのシンボル。 次の例は、条件付きコンパイル シンボルを定義します。`A`し、もう一度定義します。
```csharp
#define A
#define A
```

A`#undef`可能性があります""条件付きコンパイル シンボルを未定義に定義されていません。 次の例は、条件付きコンパイル シンボルを定義します。`A`してし、未定義に 2 回ですが、2 つ目`#undef`、影響を与えません有効であります。
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>条件付きコンパイル ディレクティブ

条件付きコンパイル ディレクティブは、条件付きで含めるか、ソース ファイルの部分を除外に使用されます。

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

、の順でから成るセットとして示されるように、構文、条件付きコンパイル ディレクティブを書き込む必要があります、`#if`ディレクティブ、0 個以上`#elif`ディレクティブ、0 または 1 個`#else`ディレクティブ、および`#endif`ディレクティブ。 ディレクティブの間、ソース コードの条件付きセクションです。 各セクションは、直前のディレクティブによって制御されます。 条件付きセクションを含めることができます自体入れ子になった条件付きコンパイル ディレクティブ提供、これらのディレクティブは、完全なセットを形成します。

A *pp_conditional*の格納されている 1 つだけ選択*conditional_section*の通常の構文処理。

*  *Pp_expression*の s、`#if`と`#elif`ディレクティブは、1 つを生成するまで順序で評価されます`true`します。 式の結果が場合`true`、 *conditional_section*の対応するディレクティブが選択されています。
*  すべて*pp_expression*s yield `false`、場合に、`#else`ディレクティブが存在する、 *conditional_section*の`#else`ディレクティブが選択されています。
*  それ以外の場合、no *conditional_section*が選択されています。

選択した*conditional_section*として、通常、いずれかが処理される場合、 *input_section*: セクションに含まれるソース コードは、構文、文法に従う必要があります元のトークンが生成されます。セクションのコードプリプロセッサ ディレクティブのセクションでは、所定の影響を与えます。

残りの*conditional_section*として処理されているのであれば、 *skipped_section*プリプロセッサ ディレクティブを除く: %s、セクション内のソース コードが必要に準拠していない、字句文法; なしセクションで、ソース コードからトークンが生成されます。プリプロセッサ ディレクティブのセクションでは構文的に正しくなければなりませんが、それ以外の場合は処理されません。 内で、 *conditional_section*として処理されますが、 *skipped_section*を入れ子になった*conditional_section*s (に含まれている入れ子になった`#if`...`#endif`と`#region`.`#endregion`を構築します) として処理されます。 また*skipped_section*秒。

次の例は、条件付きコンパイル ディレクティブを入れ子を示しています。
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

プリプロセッサ ディレクティブを除くスキップされたソース コードが字句解析の対象です。 たとえば、未終了のコメントに関係なく有効では、次の`#else`セクション。
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

ただし、プリプロセッサ ディレクティブがソース コードのセクションをスキップしたであっても正しい構文的に必要なことに注意してください。

複数行の入力要素内に存在する場合、プリプロセッサ ディレクティブは処理されません。 たとえば、プログラムします。
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
出力結果:
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

評価に特有の場合、プリプロセッサ ディレクティブの処理は、セットが依存可能性があります、 *pp_expression*します。 例:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
常に同じトークン ストリームを生成します (`class` `Q` `{` `}`) かどうかに関係なく、`X`が定義されています。 場合`X`が定義するには、ディレクティブだけが処理されます`#if`と`#endif`複数行のコメントにしている。 場合`X`が未定義の場合、3 つのディレクティブ (`#if`、 `#else`、 `#endif`) ディレクティブのセットの一部であります。

### <a name="diagnostic-directives"></a>診断ディレクティブ

診断のディレクティブを使用すると、エラー メッセージとその他のコンパイル時エラーと警告と同じ方法で報告される警告メッセージを明示的に生成します。

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

例:
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
常に (「コード レビューでは、チェックイン前に必要な」)、警告を生成し、コンパイル時エラーを生成します (「をビルドすることはできませんデバッグし、製品版」) 場合、条件付きシンボル`Debug`と`Retail`の両方が定義されています。 なお、 *pp_message*任意のテキストを含めることができます。 具体的には、含める必要はありません、適切な形式のトークン、word で単一引用符で示すように`can't`します。

### <a name="region-directives"></a>領域ディレクティブ

領域ディレクティブを使用して、ソース コードの領域を明示的にマークします。

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

領域に接続されているセマンティックな意味がありません。リージョンが使用するため、プログラマや自動化ツール ソース コードのセクションをマークします。 指定されたメッセージ、`#region`または`#endregion`ディレクティブはセマンティックな意味は無効同様に、領域を識別するためにのみ機能します。 一致する`#region`と`#endregion`ディレクティブが異なる場合がありますが*pp_message*秒。

領域の処理の構文:
```csharp
#region
...
#endregion
```
フォームの条件付きコンパイル ディレクティブの構文の処理に正確に対応しています。
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>行ディレクティブ

行ディレクティブは、行番号とが報告された警告やエラーなどの出力で、コンパイラによって、呼び出し元情報属性で使用されるソース ファイル名を変更するために使用可能性があります ([呼び出し元情報属性](attributes.md#caller-info-attributes))。

行ディレクティブは、他のテキスト入力からの c# ソース コードを生成するメタ プログラミング ツールで最もよく使用されます。

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

ない場合`#line`ディレクティブが存在する、コンパイラは実際の行番号とその出力でのソース ファイル名を報告します。 処理するときに、`#line`ディレクティブが含まれる、 *line_indicator*でない`default`コンパイラは、特定の行番号 (および指定した場合、ファイル名) を持つものとしてディレクティブの後の行を扱います。

A`#line default`ディレクティブ上のすべての #line ディレクティブの効果を反転させます。 コンパイラが正確にない場合、後続の行の場合は true。 行情報を報告`#line`ディレクティブは処理されている必要がありました。

A`#line hidden`ディレクティブには、ファイルへの影響はありませんし、行番号のエラー報告は、メッセージが、ソース レベルのデバッグは影響します。 すべての行の間でのデバッグ時に、`#line hidden`ディレクティブとそれに続く`#line`ディレクティブ (でない`#line hidden`) 行番号情報があるありません。 デバッガーでコードをステップ実行時に、これらの行はすべてスキップされます。

なお、 *file_name*されるエスケープ文字は処理されません通常の文字列リテラルと異なります、"`\`"文字が単純に内での通常のバック スラッシュ文字を指定、 *file_name*。

### <a name="pragma-directives"></a>プラグマ ディレクティブ

`#pragma`プリプロセッサ ディレクティブを使用すると、コンパイラにコンテキスト情報を指定します。 情報の指定、`#pragma`ディレクティブには、プログラムのセマンティクスは変わりません。

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C# では`#pragma`コンパイラの警告を制御するためのディレクティブ。 将来のバージョンの言語は、追加で含めることができます`#pragma`ディレクティブ。 他の c# コンパイラとの相互運用のために、Microsoft c# コンパイラを発行しません不明なコンパイル エラー`#pragma`ディレクティブはこのようなディレクティブは、ただし警告を生成します。

#### <a name="pragma-warning"></a>Pragma 警告

`#pragma warning`ディレクティブを使用して無効にするか、またはすべてを復元する、またはそれ以降のプログラム テキストのコンパイル中にメッセージを特定の警告のセット。

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

A`#pragma warning`警告の一覧の指定を省略するディレクティブがすべての警告に影響します。 A`#pragma warning`ディレクティブが含まれています、警告の一覧の一覧で指定されている警告のみに影響を与えます。

A`#pragma warning disable`ディレクティブ無効にします。 すべてまたは指定した警告のセット。

A`#pragma warning restore`ディレクティブ復元のすべてまたは特定の一連のコンパイル単位の先頭で有効にされた状態に警告します。 特定の警告が外部で無効になっている場合、 `#pragma warning restore` (すべてのかどうか、または特定の警告) 警告を再度有効にはありません。

次の例は、の使用を示しています。 `#pragma warning` 、警告を一時的に無効にする場合に報告廃止 Microsoft c# コンパイラから警告番号を使用して、メンバーが参照します。
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
