# <a name="documentation-comments"></a>ドキュメントのコメント

C# プログラマを XML テキストを含む特殊なコメント構文を使用してコードを文書化するためのメカニズムを提供します。 ソース コード ファイルでこのようなコメントとその直後にあるソース コード要素から XML を生成するツールに指示する特定の形式のコメントを使用できます。 コメントを使用してこのような構文が呼び出される***ドキュメントのコメント***します。 (クラス、デリゲート、またはインターフェイス) などのユーザー定義型またはメンバー (フィールド、イベント、プロパティ、メソッドなど) の直前する必要がありますに。 XML の生成ツールと呼ばれる、***ドキュメント ジェネレーター***します。 (このジェネレーターが必要はありません、c# コンパイラ自体)。ドキュメント ジェネレーターによって生成される出力と呼ばれる、***ドキュメント ファイル***します。 ドキュメント ファイルが入力として使用、***ドキュメント ビューアー***です。 ツールがなんらかの種類の情報とその関連するドキュメントのビジュアル表示を生成するためのものです。

この仕様には一連のドキュメントのコメントで使用されるタグがこれらのタグの使用は必須ではありませんし、その他のタグとして使用できる場合は、必要に応じて、整形式 XML の長時間の規則が適用されます。

## <a name="introduction"></a>はじめに

このようなコメントとその直後にあるソース コード要素から XML を生成するツールに指示する特殊な形式のコメントを使用できます。 このようなコメントは、3 つのスラッシュで始まる単一行コメント (`///`)、またはスラッシュを 2 つの星で始まるコメントの区切り (`/**`)。 ユーザー定義型 (クラス、デリゲート、またはインターフェイス) など、またはこれらの注釈を設定するメンバー (フィールド、イベント、プロパティ、メソッドなど) の直前する必要がありますに。 セクションの属性 ([属性の指定](attributes.md#attribute-specification)) ドキュメントのコメントは、型またはメンバーに適用される属性を付ける必要がありますので、宣言の一部と見なされます。

__構文:__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

*Single_line_doc_comment*がある場合を*空白*文字以下、`///`の各文字、 *single_line_doc_comment*隣接する s現在*single_line_doc_comment*、しを*空白*文字は XML 出力に含まれていません。

区切られた-ドキュメントのコメントに場合 2 つ目の行の最初の空白以外の文字はアスタリスクと同じパターンの省略可能な空白文字とアスタリスク文字が繰り返されます各区切り-ドキュメントのコメント内の行の先頭に繰り返しパターンの文字は、XML 出力には含まれません。 パターンは、後に、および、アスタリスク文字の前に、空白文字を含めることができます。

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

XML の規則に従って、ドキュメントのコメント内のテキストを正しい形式である必要があります (http://www.w3.org/TR/REC-xml)します。 XML の形式申し上げますが場合、は、警告が生成され、ドキュメント ファイルは、エラーが発生したことを示すコメントが含まれます。

推奨される設定が定義されている開発者は、独自のタグのセットを作成できますが、[推奨されるタグ](documentation-comments.md#recommended-tags)します。 推奨されるタグの一部には特別な意味があります。

*  `<param>`タグを使用して、パラメーターを説明します。 このようなタグを使用する場合、指定されたパラメーターが存在して、すべてのパラメーターがドキュメントのコメントに記述されているドキュメント ジェネレーターを確認します必要があります。 このような検証に失敗した場合、ドキュメントの生成は警告を発行します。
*  `cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。 ドキュメント ジェネレーターは、このコード要素が存在することを確認する必要があります。 検証に失敗した場合、ドキュメント ジェネレーターは、警告を発行します。 説明されている名前を検索するときに、`cref`属性、ドキュメント ジェネレーターには、に従って名前空間の可視性が考慮する必要があります`using`ソース コード内に出現するステートメント。 コード要素の一般的な通常の一般的な構文 (つまり"`List<T>`") 無効な XML を生成するために使用することはできません。 中かっこを角かっこではなく使用できます (ie"`List{T}`")、または XML エスケープ構文を使用することができます (つまり"`List&lt;T&gt;`")。
*  `<summary>`タグは、型またはメンバーに関する情報を表示する、ドキュメント ビューアーで使用されます。
*  `<include>`タグには、外部の XML ファイルからの情報が含まれています。

ドキュメント ファイルが、型とメンバーの完全な情報を提供しないことに慎重に注意してください (たとえば、任意の種類の情報を含まれません)。 型またはメンバーに関するこのような情報を取得するには、実際の型またはメンバーのリフレクションと組み合わせてドキュメント ファイルを使用する必要があります。

## <a name="recommended-tags"></a>推奨されるタグ

ドキュメント ジェネレーターは、そのまま使用し、XML の規則に従って有効な任意のタグを処理する必要があります。 次のタグは、ユーザー ドキュメントでよく使用される機能を提供します。 (もちろん、その他のタグは可能) です。


| __タグ__          | __セクション__                                            | __目的__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | コードのようなフォントでテキストを設定します。                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | 1 つまたは複数の行のソース コードまたはプログラムの出力の設定します。 |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | 例を示します。                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | メソッドがスローできる例外を識別します。           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | 外部ファイルから XML が含まれています                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | リストまたはテーブルを作成します。                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | テキストに追加される構造体を許可します。                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | メソッドまたはコンス トラクターのパラメーターについて説明します       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | 単語がパラメーター名であることを示します。               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | メンバーのセキュリティのユーザー補助ドキュメント        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | 型に関する追加情報について説明します           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | メソッドの戻り値について説明します                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | リンクを指定します。                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | 「参照」エントリを生成します。                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | 型または型のメンバーについて説明します                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | プロパティを説明します。                                    |
| `<typeparam>`    |                                                        | ジェネリック型パラメーターについて説明します                      |
| `<typeparamref>` |                                                        | 単語が型パラメーター名であることを示します。          |

### `<c>`

このタグは、コードのブロックを使用するなどの特殊なフォントの説明内のテキストの一部を設定することを示すメカニズムを提供します。 行の実際のコードで使用`<code>`([`<code>`](documentation-comments.md#code))。

__構文:__

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

このタグを使用して、いくつかの特別なフォントで 1 つまたは複数の行のソース コードまたはプログラムの出力を設定します。 物語で小さなコード フラグメントを使用して`<c>`([`<c>`](documentation-comments.md#c))。

__構文:__

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

このタグは、メソッド、またはその他のライブラリ メンバーを使用することがある方法を指定する、コメント内のコード例を使用します。 通常、これも含まれるタグの使用`<code>`([`<code>`](documentation-comments.md#code)) もします。

__構文:__

```xml
<example>description</example>
```

__例:__

参照してください`<code>`([`<code>`](documentation-comments.md#code)) 例についてはします。

### `<exception>`

このタグは、メソッドがスローできる例外を文書化する方法を提供します。

__構文:__

```xml
<exception cref="member">description</exception>
```

where

* `member` メンバーの名前です。 ドキュメント ジェネレーターは、指定したメンバーが存在し、変換を確認します。`member`ドキュメント ファイルで正規要素名にします。
* `description` 例外がスローされた状況の説明。

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

このタグは、ソース コード ファイルの外部にある XML ドキュメントからの情報などを使用します。 外部のファイルは、整形式 XML ドキュメントである必要があり、XPath 式に含めるそのドキュメントでは、どのような XML を指定するには、そのドキュメントに適用されます。 `<include>`タグは、選択した外部のドキュメントから XML に置き換えられます。

__構文:__

```
<include file="filename" path="xpath" />
```

where

* `filename` 外部 XML ファイルのファイル名です。 ファイル名は、インクルード タグを含むファイルを基準に解釈されます。
* `xpath` 外部 XML ファイルで、XML の一部を選択する XPath 式です。

__例:__

場合は、ソース コードには、ような宣言が含まれています。

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

外部のファイル"docs.xml"が、次の内容。

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

同じドキュメントは、ソース コードが含まれている場合、出力を示します。

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

このタグは、リストまたは項目のテーブルの作成に使用されます。 `<listheader>`のテーブルまたは定義の一覧の見出し行を定義するブロック。 (テーブルのエントリのみを定義するときに`term`の見出しを指定する必要があります)。

リスト内の各項目を指定した、`<item>`ブロックします。 定義の一覧を作成するときに両方`term`と`description`指定する必要があります。 ただしのテーブル、箇条書きリスト、または番号付きリスト、のみ`description`指定する必要があります。

__構文:__

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

* `term` この用語を定義するのですが`description`します。
* `description` 箇条書きまたは番号付きリスト内の項目またはの定義、`term`します。

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

このタグは、その他のタグ内で使用できるよう`<summary>`([`<remark>`](documentation-comments.md#remark)) または`<returns>`([`<returns>`](documentation-comments.md#returns))、構造体をテキストに追加することができます。

__構文:__

```xml
<para>content</para>
```

場所`content`段落のテキストです。

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

このタグを使用して、メソッド、コンス トラクター、またはインデクサーのパラメーターを説明します。

__構文:__

```xml
<param name="name">description</param>
```

where

* `name` パラメーターの名前です。
* `description` パラメーターの説明。

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

このタグを使用して、単語がパラメーターであることを示します。 ドキュメント ファイルを処理することで、何らかの方法でこのパラメーターの書式を設定します。

__構文:__

```xml
<paramref name="name"/>
```

場所`name`パラメーターの名前を指定します。

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

このタグは、文書化するメンバーのセキュリティのユーザー補助機能を使用します。

__構文:__

```xml
<permission cref="member">description</permission>
```

where

* `member` メンバーの名前です。 ドキュメントの生成は、指定されたコード要素が存在し、変換を確認します。*メンバー*ドキュメント ファイルで正規要素名にします。
* `description` メンバーへのアクセスの説明を示します。

__例:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

このタグを使用して、型に関する追加情報を指定します。 (使用`<summary>`([`<summary>`](documentation-comments.md#summary))、型自体と、型のメンバーを記述します)。

__構文:__

```xml
<remark>description</remark>
```

場所`description`注釈のテキストです。

__例:__

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remark>Uses polar coordinates</remark>
public class Point 
{
    // ...
}
```

### `<returns>`

このタグを使用して、メソッドの戻り値を説明します。

__構文:__

```xml
<returns>description</returns>
```

場所`description`は戻り値の説明です。

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

このタグは、テキスト内で指定するためのリンクを使用します。 使用`<seealso>`([`<seealso>`](documentation-comments.md#seealso)) では、「参照」セクションに表示されるテキストを示します。

__構文:__

```xml
<see cref="member"/>
```

場所`member`メンバーの名前を指定します。 ドキュメントの生成は、指定されたコード要素が存在し、変更を確認します。*メンバー* 、生成されたドキュメント ファイル内の要素名にします。

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

このタグは、「参照」セクションを生成するエントリをを使用します。 使用`<see>`([`<see>`](documentation-comments.md#see)) からテキスト内のリンクを指定します。

__構文:__

```xml
<seealso cref="member"/>
```

場所`member`メンバーの名前を指定します。 ドキュメントの生成は、指定されたコード要素が存在し、変更を確認します。*メンバー* 、生成されたドキュメント ファイル内の要素名にします。

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

このタグは、型または型のメンバーの説明に使用できます。 使用`<remark>`([`<remark>`](documentation-comments.md#remark))、型自体を記述します。

__構文:__

```xml
<summary>description</summary>
```

場所`description`型またはメンバーの概要を示します。

__例:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

このタグは、説明を取得するプロパティを使用します。

__構文:__

```xml
<value>property description</value>
```

場所`property description`プロパティの説明を示します。

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

このタグを使用して、クラス、構造体、インターフェイス、デリゲート、またはメソッドのジェネリック型パラメーターを説明します。

__構文:__

```xml
<typeparam name="name">description</typeparam>
```

場所`name`、型パラメーターの名前を指定し、`description`はその説明です。

__例:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

このタグを使用して、単語が型パラメーターであることを示します。 ドキュメント ファイルを処理することで、何らかの方法でこの型パラメーターの書式を設定します。

__構文:__

```xml
<typeparamref name="name"/>
```

場所`name`型パラメーターの名前を指定します。

__例:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>ドキュメント ファイルの処理

ドキュメント ジェネレーターは、ドキュメントのコメントとタグ付けされているソース コード内の各要素の ID 文字列を生成します。 この ID 文字列は、ソース要素を一意に識別します。 ドキュメント ビューアーでは、ドキュメントを適用するのに、対応するメタデータ/リフレクション項目を識別するために、ID 文字列を使用できます。

ドキュメント ファイルは、ソース コードの階層表現ではありません。代わりに、各要素に対して生成される ID 文字列をフラットな一覧を示します。

### <a name="id-string-format"></a>ID 文字列の形式

ドキュメント ジェネレーターは、ID 文字列を生成するときに、次の規則を監視します。

*  文字列に空白は配置されません。

*  文字列の最初の部分では、単一の文字の後にコロンを使用して、記述されているメンバーの種類を識別します。 次の種類のメンバーが定義されています。

   | __文字__ | __説明__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | event                                                       |
   | F             | フィールド                                                       |
   | M             | メソッド (コンス トラクター、デストラクター、および演算子を含む) |
   | N             | 名前空間                                                   |
   | P             | プロパティ (インデクサーを含む)                               |
   | T             | (クラス、デリゲート、列挙型、インターフェイス、構造体など) を入力します。 |
   | !             | エラーの文字列。文字列の残りの部分は、エラーに関する情報を提供します。 たとえば、ドキュメントの生成には、解決できないリンクのエラー情報が生成されます。 |

*  文字列の 2 番目の部分は、名前空間のルートから始まり、要素の完全修飾名です。 要素、それを囲む型、および名前空間の名前は、ピリオドで区切られます。 項目自体の名前にピリオドがある場合は、置き換えられる`#(U+0023)`文字。 (これと見なされます要素の名前にはこの文字がありません。)
*  メソッドとプロパティの引数は、引数には、次のように、かっこで囲まれているが一覧表示します。 引数を指定せずに、かっこを省略します。 引数はコンマで区切られます。 各引数のエンコーディング CLI シグネチャと同じとおりです。
   *  引数は、次のように変更された、完全修飾名に基づくドキュメント名で表されます。
      * ジェネリック型を表す引数が末尾にある`` ` ``(アクサン グラーブ) 文字が続く型パラメーターの数
      * 引数、`out`または`ref`修飾子が、`@`に従って、型名。 値渡しまたは経由で渡される引数`params`特殊な表記はあるありません。
      * 引数には、配列として表されます`[lowerbound:size, ... , lowerbound:size]`コンマの数は、1 を引いたランクと下限と各次元のサイズがわかっている場合が 10 進数で表されます。 下限またはサイズが指定されていない場合は省略されます。 下限の境界と、特定のディメンションのサイズを省略した場合、`:`も省略されます。 ジャグ配列は 1 つで表されます`[]`レベルごと。
      * 使用してポインターの型 void 以外の引数が表される、`*`次の型名。 Void ポインターは、の型名を使用して表される`System.Void`します。
      * 型で定義されたジェネリック型パラメーターを参照する引数を使用してエンコード、 `` ` `` (アクサン グラーブ) 文字の後に、型パラメーターの 0 から始まるインデックス。
      * メソッドで定義されているジェネリック型パラメーターを使用する引数を使用して、double バックティック``` `` ```の代わりに、`` ` ``の種類で使用します。
      * 構築されたジェネリック型を参照する引数は、後に、ジェネリック型を使用してエンコードする`{`、型引数のコンマ区切りのリストが続く、続けて`}`します。

### <a name="id-string-examples"></a>ID 文字列の例

次の例については、c# コード、ドキュメントのコメントを持つことのできる各ソース要素から生成される ID 文字列とのフラグメントを示します。

*  型では、汎用的な情報で補完された、完全修飾名を使用して表されます。

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

*  デストラクターです。

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

*  メソッド。

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

*  プロパティとインデクサーです。

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

*  イベント。

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

   使用される単項演算子の関数名の完全なセットのとおりです: `op_UnaryPlus`、 `op_UnaryNegation`、 `op_LogicalNot`、 `op_OnesComplement`、 `op_Increment`、 `op_Decrement`、 `op_True`、および`op_False`します。

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

   使用される二項演算子関数名の完全なセットのとおりです: `op_Addition`、 `op_Subtraction`、 `op_Multiply`、 `op_Division`、 `op_Modulus`、 `op_BitwiseAnd`、 `op_BitwiseOr`、 `op_ExclusiveOr`、 `op_LeftShift`、 `op_RightShift`、`op_Equality`、 `op_Inequality`、 `op_LessThan`、 `op_LessThanOrEqual`、 `op_GreaterThan`、および`op_GreaterThanOrEqual`します。

*  変換演算子は、末尾がある"`~`"後に、戻り値の型。

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

### <a name="c-source-code"></a>C# ソース コード

次の例のソース コードを示しています、`Point`クラス。

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

クラスのソース コードが指定されると、1 つのドキュメント ジェネレーターによって生成される出力を次に示します`Point`、前述のようにします。

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
