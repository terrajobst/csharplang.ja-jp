---
ms.openlocfilehash: 2026fc1bf9d3576b967cbc2e9a670aa44b7eab3a
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704016"
---
# <a name="documentation-comments"></a>ドキュメント コメント

C#XML テキストを含む特殊なコメント構文を使用してコードを文書化するための機構を提供します。 ソースコードファイルでは、特定のフォームを含むコメントを使用して、そのコメントとソースコード要素の前にあるソースコード要素から XML を生成するようにツールに指示できます。 このような構文を使用するコメントは、***ドキュメントコメント***と呼ばれます。 これらは、ユーザー定義型 (クラス、デリゲート、インターフェイスなど) またはメンバー (フィールド、イベント、プロパティ、メソッドなど) の直前に配置する必要があります。 XML 生成ツールは、***ドキュメントジェネレーター***と呼ばれています。 (このジェネレーターはC#コンパイラ自体にすることができますが、そうである必要はありません)。ドキュメントジェネレーターによって生成される出力は、***ドキュメントファイル***と呼ばれます。 ドキュメント***ビューアー***への入力としてドキュメントファイルが使用されます。型情報とそれに関連するドキュメントを視覚的に表示するためのツール。

この仕様では、ドキュメントコメントで使用されるタグのセットを提示しますが、これらのタグの使用は必須ではなく、適切な形式の XML の規則に従う限り、必要に応じて他のタグを使用することもできます。

## <a name="introduction"></a>はじめに

特別な形式のコメントを使用すると、そのコメントとソースコード要素の前にあるソースコード要素から XML を生成するようにツールに指示できます。 このようなコメントは、3つのスラッシュ (`///`) で始まる単一行のコメントか、スラッシュと2つの星 (`/**`) で始まる区切り記号で区切られたコメントです。 これらは、ユーザー定義型 (クラス、デリゲート、インターフェイスなど)、または注釈を付けるメンバー (フィールド、イベント、プロパティ、メソッドなど) の直前に配置する必要があります。 属性セクション ([属性の指定](attributes.md#attribute-specification)) は宣言の一部と見なされるため、ドキュメントコメントは、型またはメンバーに適用される属性の前に記述する必要があります。

__文__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

*Single_line_doc_comment*では、現在の*single_line_doc_comment*に隣接する各*single_line_doc_comment*の `///` 文字の後に*空白*文字がある場合、その*空白*文字は XML 出力に含まれません。

区切られた doc コメントでは、2行目の空白以外の最初の文字がアスタリスクで、省略可能な空白文字のパターンが同じで、区切り文字の先頭にアスタリスク文字が繰り返されている場合は、その後、繰り返されるパターンの文字は XML 出力に含まれません。 パターンには、アスタリスク文字の前と同様に、空白文字を含めることができます。

__例:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>
///
public class Point 
{
    /// <summary>method <c>draw</c> renders the point.</summary>
    void draw() {...}
}
```

ドキュメントコメント内のテキストは、XML (https://www.w3.org/TR/REC-xml)の規則に従って適切な形式である必要があります。 XML の形式が正しくない場合は、警告が生成され、ドキュメントファイルにはエラーが発生したことを示すコメントが含まれます。

開発者は独自のタグセットを自由に作成できますが、推奨されるセットは推奨される[タグ](documentation-comments.md#recommended-tags)で定義されています。 推奨されるタグの一部には特別な意味があります。

*  `<param>` タグは、パラメーターを記述するために使用されます。 このようなタグが使用されている場合、ドキュメントジェネレーターは、指定されたパラメーターが存在すること、およびすべてのパラメーターがドキュメントコメントに記述されていることを確認する必要があります。 このような検証が失敗した場合は、ドキュメントジェネレーターによって警告が発行されます。
*  `cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。 ドキュメントジェネレーターは、このコード要素が存在することを確認する必要があります。 検証が失敗した場合は、ドキュメントジェネレーターによって警告が発行されます。 `cref` 属性で記述されている名前を検索する場合、ドキュメントジェネレーターは、ソースコード内に表示される `using` ステートメントに従って、名前空間の可視性を考慮する必要があります。 ジェネリックであるコード要素の場合、通常のジェネリック構文 (つまり "`List<T>`") を使用することはできません。これは、無効な XML が生成されるためです。 角かっこ (つまり "`List{T}`") の代わりに中かっこを使用することも、XML エスケープ構文を使用することもできます (つまり、"`List&lt;T&gt;`")。
*  `<summary>` タグは、ドキュメントビューアーが型またはメンバーに関する追加情報を表示するために使用することを目的としています。
*  `<include>` タグには、外部 XML ファイルからの情報が含まれています。

ドキュメントファイルでは、型とメンバーに関する完全な情報が提供されていないことに注意してください (たとえば、型情報が含まれていない場合)。 型またはメンバーに関する情報を取得するには、ドキュメントファイルを実際の型またはメンバーのリフレクションと組み合わせて使用する必要があります。

## <a name="recommended-tags"></a>推奨されるタグ

ドキュメントジェネレーターは、XML の規則に従って有効なすべてのタグを受け入れて処理する必要があります。 次のタグは、ユーザードキュメントで一般的に使用される機能を提供します。 (もちろん、他のタグも使用できます)。


| __番号__          | __Section__                                            | __目的__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | コードに似たフォントでテキストを設定する                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | ソースコードまたはプログラム出力の1行以上の行を設定する |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | 例を示します。                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | メソッドがスローできる例外を識別します。           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | 外部ファイルから XML をインクルードします。                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | リストまたはテーブルを作成する                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | テキストへの構造の追加を許可する                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | メソッドまたはコンストラクターのパラメーターの記述       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | 単語がパラメーター名であることを識別する               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | メンバーのセキュリティアクセシビリティを文書化する        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | 型に関する追加情報の記述           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | メソッドの戻り値の説明                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | リンクの指定                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | 関連項目を生成する                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | 型または型のメンバーの記述                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | プロパティの説明                                    |
| `<typeparam>`    |                                                        | ジェネリック型パラメーターの記述                      |
| `<typeparamref>` |                                                        | 単語が型パラメーター名であることを識別する          |

### `<c>`

このタグは、記述に含まれるテキストのフラグメントを、コードブロックに使用される特殊なフォントで設定する必要があることを示す機構を提供します。 実際のコード行の場合は、`<code>` ([`<code>`](documentation-comments.md#code)) を使用します。

__文__

```xml
<c>text</c>
```

__例:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

このタグは、ソースコードまたはプログラム出力の1つ以上の行を特殊なフォントで設定するために使用されます。 ナレーションの小さなコードフラグメントの場合は、`<c>` ([`<c>`](documentation-comments.md#c)) を使用します。

__文__

```xml
<code>source code or program output</code>
```

__例:__

```csharp
/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// <example>For example:
/// <code>
///    Point p = new Point(3,5);
///    p.Translate(-1,3);
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>

public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}   
```

### `<example>`

このタグでは、コメント内のコード例を使用して、メソッドまたはその他のライブラリメンバーの使用方法を指定できます。 通常は、タグ `<code>` ([`<code>`](documentation-comments.md#code)) も使用します。

__文__

```xml
<example>description</example>
```

__例:__

例については、「`<code>` ([`<code>`](documentation-comments.md#code))」を参照してください。

### `<exception>`

このタグは、メソッドがスローできる例外を文書化する方法を提供します。

__文__

```xml
<exception cref="member">description</exception>
```

where

* `member` は、メンバーの名前です。 ドキュメントジェネレーターは、指定されたメンバーが存在することを確認し、`member` をドキュメントファイル内の正規要素名に変換します。
* `description` は、例外がスローされる状況の説明です。

__例:__

```csharp
public class DataBaseOperations
{
    /// <exception cref="MasterFileFormatCorruptException"></exception>
    /// <exception cref="MasterFileLockedOpenException"></exception>
    public static void ReadRecord(int flag) {
        if (flag == 1)
            throw new MasterFileFormatCorruptException();
        else if (flag == 2)
            throw new MasterFileLockedOpenException();
        // ...
    } 
}
```

### `<include>`

このタグを使用すると、ソースコードファイルの外部にある XML ドキュメントの情報を含めることができます。 外部ファイルは整形式の XML ドキュメントである必要があり、そのドキュメントに含まれる XML を指定するために XPath 式がそのドキュメントに適用されます。 次に、`<include>` タグが、外部ドキュメントから選択された XML に置き換えられます。

__文__

```xml
<include file="filename" path="xpath" />
```

where

* `filename` は、外部 XML ファイルのファイル名です。 ファイル名は、include タグを含むファイルに対して相対的に解釈されます。
* `xpath` は、外部 XML ファイル内の XML の一部を選択する XPath 式です。

__例:__

ソースコードに次のような宣言が含まれている場合:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

外部ファイル "docs .xml" には次の内容が含まれていました。

```xml
<?xml version="1.0"?>
<extradoc>
  <class name="IntList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
  <class name="StringList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
</extradoc>
```

次に、同じドキュメントが、ソースコードに含まれているものとして出力されます。

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

このタグは、項目のリストまたはテーブルを作成するために使用されます。 テーブルまたは定義の一覧の見出し行を定義するために `<listheader>` ブロックが含まれている場合があります。 (テーブルを定義する場合は、見出し内の `term` のエントリだけを指定する必要があります)。

リスト内の各項目には、`<item>` ブロックが指定されています。 定義リストを作成する場合は、`term` と `description` の両方を指定する必要があります。 ただし、テーブル、箇条書きリスト、番号付きリストの場合は、`description` だけを指定する必要があります。

__文__

```xml
<list type="bullet" | "number" | "table">
   <listheader>
      <term>term</term>
      <description>*description*</description>
   </listheader>
   <item>
      <term>term</term>
      <description>*description*</description>
   </item>
    ...
   <item>
      <term>term</term>
      <description>description</description>
   </item>
</list>
```

where

* `term` は定義する用語で、`description`に定義されています。
* `description` は、箇条書きまたは番号付きリストの項目、または `term`の定義のいずれかです。

__例:__

```csharp
public class MyClass
{
    /// <summary>Here is an example of a bulleted list:
    /// <list type="bullet">
    /// <item>
    /// <description>Item 1.</description>
    /// </item>
    /// <item>
    /// <description>Item 2.</description>
    /// </item>
    /// </list>
    /// </summary>
    public static void Main () {
        // ...
    }
}
```

### `<para>`

このタグは、`<summary>` ([`<remarks>`](documentation-comments.md#remarks)) や `<returns>` ([`<returns>`](documentation-comments.md#returns)) などの他のタグの内部で使用され、構造をテキストに追加することを許可します。

__文__

```xml
<para>content</para>
```

ここで `content` は段落のテキストです。

__例:__

```csharp
/// <summary>This is the entry point of the Point class testing program.
/// <para>This program tests each method and operator, and
/// is intended to be run after any non-trivial maintenance has
/// been performed on the Point class.</para></summary>
public static void Main() {
    // ...
}
```

### `<param>`

このタグは、メソッド、コンストラクター、またはインデクサーのパラメーターを記述するために使用されます。

__文__

```xml
<param name="name">description</param>
```

where

* `name` は、パラメーターの名前です。
* `description` は、パラメーターの説明です。

__例:__

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <param name="xor">the new x-coordinate.</param>
/// <param name="yor">the new y-coordinate.</param>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<paramref>`

このタグは、単語がパラメーターであることを示すために使用されます。 ドキュメントファイルを処理して、このパラメーターを別の方法で書式設定することができます。

__文__

```xml
<paramref name="name"/>
```

ここで `name` はパラメーターの名前です。

__例:__

```csharp
/// <summary>This constructor initializes the new Point to
///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
/// <param name="xor">the new Point's x-coordinate.</param>
/// <param name="yor">the new Point's y-coordinate.</param>

public Point(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<permission>`

このタグにより、メンバーのセキュリティアクセシビリティを文書化することができます。

__文__

```xml
<permission cref="member">description</permission>
```

where

* `member` は、メンバーの名前です。 ドキュメントジェネレーターは、指定されたコード要素が存在することを確認し、*メンバー*をドキュメントファイル内の正規要素名に変換します。
* `description` は、メンバーへのアクセスの説明です。

__例:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

このタグは、型に関する追加情報を指定するために使用されます。 (`<summary>` ([`<summary>`](documentation-comments.md#summary)) を使用して、型自体と型のメンバーを記述します。

__文__

```xml
<remarks>description</remarks>
```

ここで `description` はコメントのテキストです。

__例:__

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remarks>Uses polar coordinates</remarks>
public class Point 
{
    // ...
}
```

### `<returns>`

このタグは、メソッドの戻り値を記述するために使用されます。

__文__

```xml
<returns>description</returns>
```

ここで `description` は戻り値の説明です。

__例:__

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

このタグを使用すると、テキスト内でリンクを指定できます。 `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) を使用して、「関連項目」セクションに表示されるテキストを指定します。

__文__

```xml
<see cref="member"/>
```

ここで `member` はメンバーの名前です。 ドキュメントジェネレーターは、指定されたコード要素が存在することを確認し、生成されたドキュメントファイル内の要素名に*メンバー*を変更します。

__例:__

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <see cref="Translate"/>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}

/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}
```

### `<seealso>`

このタグを使用すると、[参照] セクションのエントリも生成されます。 `<see>` ([`<see>`](documentation-comments.md#see)) を使用して、テキスト内からリンクを指定します。

__文__

```xml
<seealso cref="member"/>
```

ここで `member` はメンバーの名前です。 ドキュメントジェネレーターは、指定されたコード要素が存在することを確認し、生成されたドキュメントファイル内の要素名に*メンバー*を変更します。

__例:__

```csharp
/// <summary>This method determines whether two Points have the same
///    location.</summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals(object o) {
    // ...
}
```

### `<summary>`

このタグは、型または型のメンバーを記述するために使用できます。 型自体を記述するには、`<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) を使用します。

__文__

```xml
<summary>description</summary>
```

ここで `description` は、型またはメンバーの概要です。

__例:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

このタグを使用すると、プロパティを記述できます。

__文__

```xml
<value>property description</value>
```

ここで `property description` はプロパティの説明です。

__例:__

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

このタグは、クラス、構造体、インターフェイス、デリゲート、またはメソッドのジェネリック型パラメーターを記述するために使用されます。

__文__

```xml
<typeparam name="name">description</typeparam>
```

ここで `name` は型パラメーターの名前、`description` はその説明です。

__例:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

このタグは、単語が型パラメーターであることを示すために使用されます。 ドキュメントファイルを処理して、この型パラメーターを別の方法で書式設定することができます。

__文__

```xml
<typeparamref name="name"/>
```

ここで `name` は型パラメーターの名前です。

__例:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>ドキュメントファイルを処理しています

ドキュメントジェネレーターは、ドキュメントコメントでタグ付けされたソースコード内の各要素に対して ID 文字列を生成します。 この ID 文字列は、ソース要素を一意に識別します。 ドキュメントビューアーでは、ID 文字列を使用して、ドキュメントが適用される、対応するメタデータ/リフレクション項目を識別できます。

ドキュメントファイルは、ソースコードの階層的な表現ではありません。代わりに、要素ごとに生成された ID 文字列を含む単純なリストになります。

### <a name="id-string-format"></a>ID 文字列の形式

ドキュメントジェネレーターは、ID 文字列を生成するときに、次の規則を監視します。

*  文字列に空白は配置されません。

*  文字列の最初の部分では、ドキュメント化されているメンバーの種類を、1つの文字の後にコロンを使用して識別します。 次の種類のメンバーが定義されています。

   | __文字__ | __説明__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | イベント                                                       |
   | F             | フィールド                                                       |
   | M             | メソッド (コンストラクター、デストラクター、および演算子を含む) |
   | N             | Namespace                                                   |
   | P             | プロパティ (インデクサーを含む)                               |
   | T             | 型 (クラス、デリゲート、列挙型、インターフェイス、構造体など) |
   | !             | エラー文字列;文字列の残りの部分は、エラーに関する情報を提供します。 たとえば、ドキュメントジェネレーターは、解決できないリンクのエラー情報を生成します。 |

*  文字列の2番目の部分は、要素の完全修飾名であり、名前空間のルートから始まります。 要素の名前、それを囲む型、および名前空間は、ピリオドで区切られます。 アイテムの名前にピリオドが含まれている場合は、`#(U+0023)` 文字に置き換えられます。 (この文字が名前に含まれている要素がないことを前提としています)。
*  引数を持つメソッドとプロパティについては、かっこで囲まれた引数リストが続きます。 引数を指定しない場合、かっこは省略されます。 引数はコンマで区切られます。 各引数のエンコーディングは、次のように CLI シグネチャと同じです。
   *  引数は、次のように変更された完全修飾名に基づいて、ドキュメント名によって表されます。
      * ジェネリック型を表す引数には `` ` `` (バックティック) 文字が追加され、その後に型パラメーターの数が続きます。
      * `out` または `ref` 修飾子を持つ引数には、型名の後に `@` があります。 値または `params` によって渡される引数には、特別な表記はありません。
      * 配列である引数は `[lowerbound:size, ... , lowerbound:size]` として表されます。ここで、コンマの数は1未満の値になり、各次元の下限とサイズ (既知の場合) は10進数で表されます。 下限またはサイズが指定されていない場合は省略されます。 特定の次元の下限とサイズが省略されている場合は、`:` も省略されます。 ジャグ配列は、レベルごとに1つの `[]` で表されます。
      * Void 以外のポインター型を持つ引数は、型名の後の `*` を使用して表されます。 Void ポインターは、`System.Void`の型名を使用して表されます。
      * 型に対して定義されているジェネリック型パラメーターを参照する引数は、`` ` `` (バックティック) 文字の後に型パラメーターの0から始まるインデックスを使用してエンコードされます。
      * メソッドで定義されているジェネリック型パラメーターを使用する引数は、型に使用される `` ` `` ではなく、バックティック ``` `` ``` を使用します。
      * 構築されたジェネリック型を参照する引数は、ジェネリック型を使用してエンコードされ、その後に `{`が続き、コンマ区切りの型引数リストが続き、その後に `}`が続きます。

### <a name="id-string-examples"></a>ID 文字列の例

次の例では、各ソースC#要素から生成された、ドキュメントコメントを持つことができる ID 文字列と共に、コードのフラグメントを示しています。

*  型は、完全修飾名を使用して表され、汎用情報で補完されます。

   ```csharp
   enum Color { Red, Blue, Green }

   namespace Acme
   {
       interface IProcess {...}

       struct ValueType {...}

       class Widget: IProcess
       {
           public class NestedClass {...}
           public interface IMenuItem {...}
           public delegate void Del(int i);
           public enum Direction { North, South, East, West }
       }

       class MyList<T>
       {
           class Helper<U,V> {...}
       }
   }

   "T:Color"
   "T:Acme.IProcess"
   "T:Acme.ValueType"
   "T:Acme.Widget"
   "T:Acme.Widget.NestedClass"
   "T:Acme.Widget.IMenuItem"
   "T:Acme.Widget.Del"
   "T:Acme.Widget.Direction"
   "T:Acme.MyList`1"
   "T:Acme.MyList`1.Helper`2"
   ```

*  フィールドは、完全修飾名で表されます。

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           private int total;
       }
   
       class Widget: IProcess
       {
           public class NestedClass
           {
               private int value;
           }
   
           private string message;
           private static Color defaultColor;
           private const double PI = 3.14159;
           protected readonly double monthlyAverage;
           private long[] array1;
           private Widget[,] array2;
           private unsafe int *pCount;
           private unsafe float **ppValues;
       }
   }

   "F:Acme.ValueType.total"
   "F:Acme.Widget.NestedClass.value"
   "F:Acme.Widget.message"
   "F:Acme.Widget.defaultColor"
   "F:Acme.Widget.PI"
   "F:Acme.Widget.monthlyAverage"
   "F:Acme.Widget.array1"
   "F:Acme.Widget.array2"
   "F:Acme.Widget.pCount"
   "F:Acme.Widget.ppValues"
   ```

*  コンストラクター。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           static Widget() {...}
           public Widget() {...}
           public Widget(string s) {...}
       }
   }

   "M:Acme.Widget.#cctor"
   "M:Acme.Widget.#ctor"
   "M:Acme.Widget.#ctor(System.String)"
   ```

*  デストラクタ.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           ~Widget() {...}
       }
   }
   
   "M:Acme.Widget.Finalize"
   ```

*  メソッド.

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           public void M(int i) {...}
       }

       class Widget: IProcess
       {
           public class NestedClass
           {
               public void M(int i) {...}
           }

           public static void M0() {...}
           public void M1(char c, out float f, ref ValueType v) {...}
           public void M2(short[] x1, int[,] x2, long[][] x3) {...}
           public void M3(long[][] x3, Widget[][,,] x4) {...}
           public unsafe void M4(char *pc, Color **pf) {...}
           public unsafe void M5(void *pv, double *[][,] pd) {...}
           public void M6(int i, params object[] args) {...}
       }

       class MyList<T>
       {
           public void Test(T t) { }
       }

       class UseList
       {
           public void Process(MyList<int> list) { }
           public MyList<T> GetValues<T>(T inputValue) { return null; }
       }
   }

   "M:Acme.ValueType.M(System.Int32)"
   "M:Acme.Widget.NestedClass.M(System.Int32)"
   "M:Acme.Widget.M0"
   "M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
   "M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
   "M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
   "M:Acme.Widget.M4(System.Char*,Color**)"
   "M:Acme.Widget.M5(System.Void*,System.Double*[0:,0:][])"
   "M:Acme.Widget.M6(System.Int32,System.Object[])"
   "M:Acme.MyList`1.Test(`0)"
   "M:Acme.UseList.Process(Acme.MyList{System.Int32})"
   "M:Acme.UseList.GetValues``(``0)"
   ```

*  プロパティとインデクサー。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public int Width { get {...} set {...} }
           public int this[int i] { get {...} set {...} }
           public int this[string s, int i] { get {...} set {...} }
       }
   }

   "P:Acme.Widget.Width"
   "P:Acme.Widget.Item(System.Int32)"
   "P:Acme.Widget.Item(System.String,System.Int32)"
   ```

*  記録.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public event Del AnEvent;
       }
   }

   "E:Acme.Widget.AnEvent"
   ```

*  単項演算子。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
   ```

   使用される単項演算子の関数名の完全なセットは、`op_UnaryPlus`、`op_UnaryNegation`、`op_LogicalNot`、`op_OnesComplement`、`op_Increment`、`op_Decrement`、`op_True`、`op_False`のようになります。

*  二項演算子。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x1, Widget x2) {...}
       }
   }

   "M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
   ```

   使用されるバイナリ演算子の関数名の完全なセットは、`op_Addition`、`op_Subtraction`、`op_Multiply`、`op_Division`、`op_Modulus`、`op_BitwiseAnd`、`op_BitwiseOr`、`op_ExclusiveOr`、`op_LeftShift`、`op_RightShift`、`op_Equality`、`op_Inequality`、`op_LessThan`、`op_LessThanOrEqual`、`op_GreaterThan`、`op_GreaterThanOrEqual`です。

*  変換演算子には、末尾に "`~`" があり、その後に戻り値の型が続きます。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static explicit operator int(Widget x) {...}
           public static implicit operator long(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
   "M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
   ```

## <a name="an-example"></a>使用例

### <a name="c-source-code"></a>C#ソースコード

次の例は、`Point` クラスのソースコードを示しています。

```csharp
namespace Graphics
{

/// <summary>Class <c>Point</c> models a point in a two-dimensional plane.
/// </summary>
public class Point 
{

    /// <summary>Instance variable <c>x</c> represents the point's
    ///    x-coordinate.</summary>
    private int x;

    /// <summary>Instance variable <c>y</c> represents the point's
    ///    y-coordinate.</summary>
    private int y;

    /// <value>Property <c>X</c> represents the point's x-coordinate.</value>
    public int X
    {
        get { return x; }
        set { x = value; }
    }

    /// <value>Property <c>Y</c> represents the point's y-coordinate.</value>
    public int Y
    {
        get { return y; }
        set { y = value; }
    }

    /// <summary>This constructor initializes the new Point to
    ///    (0,0).</summary>
    public Point() : this(0,0) {}

    /// <summary>This constructor initializes the new Point to
    ///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
    /// <param><c>xor</c> is the new Point's x-coordinate.</param>
    /// <param><c>yor</c> is the new Point's y-coordinate.</param>
    public Point(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location to
    ///    the given coordinates.</summary>
    /// <param><c>xor</c> is the new x-coordinate.</param>
    /// <param><c>yor</c> is the new y-coordinate.</param>
    /// <see cref="Translate"/>
    public void Move(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location by
    ///    the given x- and y-offsets.
    /// <example>For example:
    /// <code>
    ///    Point p = new Point(3,5);
    ///    p.Translate(-1,3);
    /// </code>
    /// results in <c>p</c>'s having the value (2,8).
    /// </example>
    /// </summary>
    /// <param><c>xor</c> is the relative x-offset.</param>
    /// <param><c>yor</c> is the relative y-offset.</param>
    /// <see cref="Move"/>
    public void Translate(int xor, int yor) {
        X += xor;
        Y += yor;
    }

    /// <summary>This method determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>o</c> is the object to be compared to the current object.
    /// </param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="operator=="/>
    /// <seealso cref="operator!="/>
    public override bool Equals(object o) {
        if (o == null) {
            return false;
        }

        if (this == o) {
            return true;
        }

        if (GetType() == o.GetType()) {
            Point p = (Point)o;
            return (X == p.X) && (Y == p.Y);
        }
        return false;
    }

    /// <summary>Report a point's location as a string.</summary>
    /// <returns>A string representing a point's location, in the form (x,y),
    ///    without any leading, training, or embedded whitespace.</returns>
    public override string ToString() {
        return "(" + X + "," + Y + ")";
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator!="/>
    public static bool operator==(Point p1, Point p2) {
        if ((object)p1 == null || (object)p2 == null) {
            return false;
        }

        if (p1.GetType() == p2.GetType()) {
            return (p1.X == p2.X) && (p1.Y == p2.Y);
        }

        return false;
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points do not have the same location and the
    ///    exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator=="/>
    public static bool operator!=(Point p1, Point p2) {
        return !(p1 == p2);
    }

    /// <summary>This is the entry point of the Point class testing
    /// program.
    /// <para>This program tests each method and operator, and
    /// is intended to be run after any non-trivial maintenance has
    /// been performed on the Point class.</para></summary>
    public static void Main() {
        // class test code goes here
    }
}
}
```

### <a name="resulting-xml"></a>結果の XML

次に示すのは、上記のクラス `Point`のソースコードが指定されている場合に、1つのドキュメントジェネレーターによって生成される出力です。

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <summary>Class <c>Point</c> models a point in a two-dimensional
            plane.
            </summary>
        </member>

        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>

        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>

        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
        (0,0).</summary>
        </member>

        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
            <param><c>xor</c> is the new Point's x-coordinate.</param>
            <param><c>yor</c> is the new Point's y-coordinate.</param>
        </member>

        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>xor</c> is the new x-coordinate.</param>
            <param><c>yor</c> is the new y-coordinate.</param>
            <see cref="M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>

        <member
            name="M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by
            the given x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>xor</c> is the relative x-offset.</param>
            <param><c>yor</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>

        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the same
            location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.
            </param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
            <seealso
      cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y),
            without any leading, training, or embedded whitespace.</returns>
        </member>

        <member
       name="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
     cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member
      name="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the
            exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trivial maintenance has
            been performed on the Point class.</para></summary>
        </member>

        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>

        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
