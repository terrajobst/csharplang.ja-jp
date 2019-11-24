---
ms.openlocfilehash: 2026fc1bf9d3576b967cbc2e9a670aa44b7eab3a
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704016"
---
# <a name="documentation-comments"></a><span data-ttu-id="75e86-101">ドキュメント コメント</span><span class="sxs-lookup"><span data-stu-id="75e86-101">Documentation comments</span></span>

<span data-ttu-id="75e86-102">C#XML テキストを含む特殊なコメント構文を使用してコードを文書化するための機構を提供します。</span><span class="sxs-lookup"><span data-stu-id="75e86-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="75e86-103">ソースコードファイルでは、特定のフォームを含むコメントを使用して、そのコメントとソースコード要素の前にあるソースコード要素から XML を生成するようにツールに指示できます。</span><span class="sxs-lookup"><span data-stu-id="75e86-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="75e86-104">このような構文を使用するコメントは、***ドキュメントコメント***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="75e86-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="75e86-105">これらは、ユーザー定義型 (クラス、デリゲート、インターフェイスなど) またはメンバー (フィールド、イベント、プロパティ、メソッドなど) の直前に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="75e86-106">XML 生成ツールは、***ドキュメントジェネレーター***と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="75e86-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="75e86-107">(このジェネレーターはC#コンパイラ自体にすることができますが、そうである必要はありません)。ドキュメントジェネレーターによって生成される出力は、***ドキュメントファイル***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="75e86-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="75e86-108">ドキュメント***ビューアー***への入力としてドキュメントファイルが使用されます。型情報とそれに関連するドキュメントを視覚的に表示するためのツール。</span><span class="sxs-lookup"><span data-stu-id="75e86-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="75e86-109">この仕様では、ドキュメントコメントで使用されるタグのセットを提示しますが、これらのタグの使用は必須ではなく、適切な形式の XML の規則に従う限り、必要に応じて他のタグを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="75e86-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="75e86-110">はじめに</span><span class="sxs-lookup"><span data-stu-id="75e86-110">Introduction</span></span>

<span data-ttu-id="75e86-111">特別な形式のコメントを使用すると、そのコメントとソースコード要素の前にあるソースコード要素から XML を生成するようにツールに指示できます。</span><span class="sxs-lookup"><span data-stu-id="75e86-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="75e86-112">このようなコメントは、3つのスラッシュ (`///`) で始まる単一行のコメントか、スラッシュと2つの星 (`/**`) で始まる区切り記号で区切られたコメントです。</span><span class="sxs-lookup"><span data-stu-id="75e86-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="75e86-113">これらは、ユーザー定義型 (クラス、デリゲート、インターフェイスなど)、または注釈を付けるメンバー (フィールド、イベント、プロパティ、メソッドなど) の直前に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="75e86-114">属性セクション ([属性の指定](attributes.md#attribute-specification)) は宣言の一部と見なされるため、ドキュメントコメントは、型またはメンバーに適用される属性の前に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="75e86-115">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="75e86-116">*Single_line_doc_comment*では、現在の*single_line_doc_comment*に隣接する各*single_line_doc_comment*の `///` 文字の後に*空白*文字がある場合、その*空白*文字は XML 出力に含まれません。</span><span class="sxs-lookup"><span data-stu-id="75e86-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="75e86-117">区切られた doc コメントでは、2行目の空白以外の最初の文字がアスタリスクで、省略可能な空白文字のパターンが同じで、区切り文字の先頭にアスタリスク文字が繰り返されている場合は、その後、繰り返されるパターンの文字は XML 出力に含まれません。</span><span class="sxs-lookup"><span data-stu-id="75e86-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="75e86-118">パターンには、アスタリスク文字の前と同様に、空白文字を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="75e86-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="75e86-119">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-119">__Example:__</span></span>

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

<span data-ttu-id="75e86-120">ドキュメントコメント内のテキストは、XML (https://www.w3.org/TR/REC-xml)の規則に従って適切な形式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="75e86-121">XML の形式が正しくない場合は、警告が生成され、ドキュメントファイルにはエラーが発生したことを示すコメントが含まれます。</span><span class="sxs-lookup"><span data-stu-id="75e86-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="75e86-122">開発者は独自のタグセットを自由に作成できますが、推奨されるセットは推奨される[タグ](documentation-comments.md#recommended-tags)で定義されています。</span><span class="sxs-lookup"><span data-stu-id="75e86-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="75e86-123">推奨されるタグの一部には特別な意味があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="75e86-124">`<param>` タグは、パラメーターを記述するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="75e86-125">このようなタグが使用されている場合、ドキュメントジェネレーターは、指定されたパラメーターが存在すること、およびすべてのパラメーターがドキュメントコメントに記述されていることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="75e86-126">このような検証が失敗した場合は、ドキュメントジェネレーターによって警告が発行されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="75e86-127">`cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。</span><span class="sxs-lookup"><span data-stu-id="75e86-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="75e86-128">ドキュメントジェネレーターは、このコード要素が存在することを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="75e86-129">検証が失敗した場合は、ドキュメントジェネレーターによって警告が発行されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="75e86-130">`cref` 属性で記述されている名前を検索する場合、ドキュメントジェネレーターは、ソースコード内に表示される `using` ステートメントに従って、名前空間の可視性を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="75e86-131">ジェネリックであるコード要素の場合、通常のジェネリック構文 (つまり "`List<T>`") を使用することはできません。これは、無効な XML が生成されるためです。</span><span class="sxs-lookup"><span data-stu-id="75e86-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="75e86-132">角かっこ (つまり "`List{T}`") の代わりに中かっこを使用することも、XML エスケープ構文を使用することもできます (つまり、"`List&lt;T&gt;`")。</span><span class="sxs-lookup"><span data-stu-id="75e86-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="75e86-133">`<summary>` タグは、ドキュメントビューアーが型またはメンバーに関する追加情報を表示するために使用することを目的としています。</span><span class="sxs-lookup"><span data-stu-id="75e86-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="75e86-134">`<include>` タグには、外部 XML ファイルからの情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="75e86-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="75e86-135">ドキュメントファイルでは、型とメンバーに関する完全な情報が提供されていないことに注意してください (たとえば、型情報が含まれていない場合)。</span><span class="sxs-lookup"><span data-stu-id="75e86-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="75e86-136">型またはメンバーに関する情報を取得するには、ドキュメントファイルを実際の型またはメンバーのリフレクションと組み合わせて使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="75e86-137">推奨されるタグ</span><span class="sxs-lookup"><span data-stu-id="75e86-137">Recommended tags</span></span>

<span data-ttu-id="75e86-138">ドキュメントジェネレーターは、XML の規則に従って有効なすべてのタグを受け入れて処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="75e86-139">次のタグは、ユーザードキュメントで一般的に使用される機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="75e86-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="75e86-140">(もちろん、他のタグも使用できます)。</span><span class="sxs-lookup"><span data-stu-id="75e86-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="75e86-141">__番号__</span><span class="sxs-lookup"><span data-stu-id="75e86-141">__Tag__</span></span>          | <span data-ttu-id="75e86-142">__Section__</span><span class="sxs-lookup"><span data-stu-id="75e86-142">__Section__</span></span>                                            | <span data-ttu-id="75e86-143">__目的__</span><span class="sxs-lookup"><span data-stu-id="75e86-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="75e86-144">コードに似たフォントでテキストを設定する</span><span class="sxs-lookup"><span data-stu-id="75e86-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="75e86-145">ソースコードまたはプログラム出力の1行以上の行を設定する</span><span class="sxs-lookup"><span data-stu-id="75e86-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="75e86-146">例を示します。</span><span class="sxs-lookup"><span data-stu-id="75e86-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="75e86-147">メソッドがスローできる例外を識別します。</span><span class="sxs-lookup"><span data-stu-id="75e86-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="75e86-148">外部ファイルから XML をインクルードします。</span><span class="sxs-lookup"><span data-stu-id="75e86-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="75e86-149">リストまたはテーブルを作成する</span><span class="sxs-lookup"><span data-stu-id="75e86-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="75e86-150">テキストへの構造の追加を許可する</span><span class="sxs-lookup"><span data-stu-id="75e86-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="75e86-151">メソッドまたはコンストラクターのパラメーターの記述</span><span class="sxs-lookup"><span data-stu-id="75e86-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="75e86-152">単語がパラメーター名であることを識別する</span><span class="sxs-lookup"><span data-stu-id="75e86-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="75e86-153">メンバーのセキュリティアクセシビリティを文書化する</span><span class="sxs-lookup"><span data-stu-id="75e86-153">Document the security accessibility of a member</span></span>        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | <span data-ttu-id="75e86-154">型に関する追加情報の記述</span><span class="sxs-lookup"><span data-stu-id="75e86-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="75e86-155">メソッドの戻り値の説明</span><span class="sxs-lookup"><span data-stu-id="75e86-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="75e86-156">リンクの指定</span><span class="sxs-lookup"><span data-stu-id="75e86-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="75e86-157">関連項目を生成する</span><span class="sxs-lookup"><span data-stu-id="75e86-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="75e86-158">型または型のメンバーの記述</span><span class="sxs-lookup"><span data-stu-id="75e86-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="75e86-159">プロパティの説明</span><span class="sxs-lookup"><span data-stu-id="75e86-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="75e86-160">ジェネリック型パラメーターの記述</span><span class="sxs-lookup"><span data-stu-id="75e86-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="75e86-161">単語が型パラメーター名であることを識別する</span><span class="sxs-lookup"><span data-stu-id="75e86-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="75e86-162">このタグは、記述に含まれるテキストのフラグメントを、コードブロックに使用される特殊なフォントで設定する必要があることを示す機構を提供します。</span><span class="sxs-lookup"><span data-stu-id="75e86-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="75e86-163">実際のコード行の場合は、`<code>` ([`<code>`](documentation-comments.md#code)) を使用します。</span><span class="sxs-lookup"><span data-stu-id="75e86-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="75e86-164">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="75e86-165">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="75e86-166">このタグは、ソースコードまたはプログラム出力の1つ以上の行を特殊なフォントで設定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="75e86-167">ナレーションの小さなコードフラグメントの場合は、`<c>` ([`<c>`](documentation-comments.md#c)) を使用します。</span><span class="sxs-lookup"><span data-stu-id="75e86-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="75e86-168">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="75e86-169">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-169">__Example:__</span></span>

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

<span data-ttu-id="75e86-170">このタグでは、コメント内のコード例を使用して、メソッドまたはその他のライブラリメンバーの使用方法を指定できます。</span><span class="sxs-lookup"><span data-stu-id="75e86-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="75e86-171">通常は、タグ `<code>` ([`<code>`](documentation-comments.md#code)) も使用します。</span><span class="sxs-lookup"><span data-stu-id="75e86-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="75e86-172">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="75e86-173">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-173">__Example:__</span></span>

<span data-ttu-id="75e86-174">例については、「`<code>` ([`<code>`](documentation-comments.md#code))」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="75e86-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="75e86-175">このタグは、メソッドがスローできる例外を文書化する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="75e86-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="75e86-176">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="75e86-177">where</span><span class="sxs-lookup"><span data-stu-id="75e86-177">where</span></span>

* <span data-ttu-id="75e86-178">`member` は、メンバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="75e86-178">`member` is the name of a member.</span></span> <span data-ttu-id="75e86-179">ドキュメントジェネレーターは、指定されたメンバーが存在することを確認し、`member` をドキュメントファイル内の正規要素名に変換します。</span><span class="sxs-lookup"><span data-stu-id="75e86-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="75e86-180">`description` は、例外がスローされる状況の説明です。</span><span class="sxs-lookup"><span data-stu-id="75e86-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="75e86-181">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-181">__Example:__</span></span>

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

<span data-ttu-id="75e86-182">このタグを使用すると、ソースコードファイルの外部にある XML ドキュメントの情報を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="75e86-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="75e86-183">外部ファイルは整形式の XML ドキュメントである必要があり、そのドキュメントに含まれる XML を指定するために XPath 式がそのドキュメントに適用されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="75e86-184">次に、`<include>` タグが、外部ドキュメントから選択された XML に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="75e86-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="75e86-185">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-185">__Syntax:__</span></span>

```xml
<include file="filename" path="xpath" />
```

<span data-ttu-id="75e86-186">where</span><span class="sxs-lookup"><span data-stu-id="75e86-186">where</span></span>

* <span data-ttu-id="75e86-187">`filename` は、外部 XML ファイルのファイル名です。</span><span class="sxs-lookup"><span data-stu-id="75e86-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="75e86-188">ファイル名は、include タグを含むファイルに対して相対的に解釈されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="75e86-189">`xpath` は、外部 XML ファイル内の XML の一部を選択する XPath 式です。</span><span class="sxs-lookup"><span data-stu-id="75e86-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="75e86-190">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-190">__Example:__</span></span>

<span data-ttu-id="75e86-191">ソースコードに次のような宣言が含まれている場合:</span><span class="sxs-lookup"><span data-stu-id="75e86-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="75e86-192">外部ファイル "docs .xml" には次の内容が含まれていました。</span><span class="sxs-lookup"><span data-stu-id="75e86-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="75e86-193">次に、同じドキュメントが、ソースコードに含まれているものとして出力されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="75e86-194">このタグは、項目のリストまたはテーブルを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="75e86-195">テーブルまたは定義の一覧の見出し行を定義するために `<listheader>` ブロックが含まれている場合があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="75e86-196">(テーブルを定義する場合は、見出し内の `term` のエントリだけを指定する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="75e86-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="75e86-197">リスト内の各項目には、`<item>` ブロックが指定されています。</span><span class="sxs-lookup"><span data-stu-id="75e86-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="75e86-198">定義リストを作成する場合は、`term` と `description` の両方を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="75e86-199">ただし、テーブル、箇条書きリスト、番号付きリストの場合は、`description` だけを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="75e86-200">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-200">__Syntax:__</span></span>

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

<span data-ttu-id="75e86-201">where</span><span class="sxs-lookup"><span data-stu-id="75e86-201">where</span></span>

* <span data-ttu-id="75e86-202">`term` は定義する用語で、`description`に定義されています。</span><span class="sxs-lookup"><span data-stu-id="75e86-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="75e86-203">`description` は、箇条書きまたは番号付きリストの項目、または `term`の定義のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="75e86-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="75e86-204">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-204">__Example:__</span></span>

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

<span data-ttu-id="75e86-205">このタグは、`<summary>` ([`<remarks>`](documentation-comments.md#remarks)) や `<returns>` ([`<returns>`](documentation-comments.md#returns)) などの他のタグの内部で使用され、構造をテキストに追加することを許可します。</span><span class="sxs-lookup"><span data-stu-id="75e86-205">This tag is for use inside other tags, such as `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="75e86-206">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="75e86-207">ここで `content` は段落のテキストです。</span><span class="sxs-lookup"><span data-stu-id="75e86-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="75e86-208">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-208">__Example:__</span></span>

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

<span data-ttu-id="75e86-209">このタグは、メソッド、コンストラクター、またはインデクサーのパラメーターを記述するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="75e86-210">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="75e86-211">where</span><span class="sxs-lookup"><span data-stu-id="75e86-211">where</span></span>

* <span data-ttu-id="75e86-212">`name` は、パラメーターの名前です。</span><span class="sxs-lookup"><span data-stu-id="75e86-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="75e86-213">`description` は、パラメーターの説明です。</span><span class="sxs-lookup"><span data-stu-id="75e86-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="75e86-214">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-214">__Example:__</span></span>

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

<span data-ttu-id="75e86-215">このタグは、単語がパラメーターであることを示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="75e86-216">ドキュメントファイルを処理して、このパラメーターを別の方法で書式設定することができます。</span><span class="sxs-lookup"><span data-stu-id="75e86-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="75e86-217">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="75e86-218">ここで `name` はパラメーターの名前です。</span><span class="sxs-lookup"><span data-stu-id="75e86-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="75e86-219">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-219">__Example:__</span></span>

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

<span data-ttu-id="75e86-220">このタグにより、メンバーのセキュリティアクセシビリティを文書化することができます。</span><span class="sxs-lookup"><span data-stu-id="75e86-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="75e86-221">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="75e86-222">where</span><span class="sxs-lookup"><span data-stu-id="75e86-222">where</span></span>

* <span data-ttu-id="75e86-223">`member` は、メンバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="75e86-223">`member` is the name of a member.</span></span> <span data-ttu-id="75e86-224">ドキュメントジェネレーターは、指定されたコード要素が存在することを確認し、*メンバー*をドキュメントファイル内の正規要素名に変換します。</span><span class="sxs-lookup"><span data-stu-id="75e86-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="75e86-225">`description` は、メンバーへのアクセスの説明です。</span><span class="sxs-lookup"><span data-stu-id="75e86-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="75e86-226">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

<span data-ttu-id="75e86-227">このタグは、型に関する追加情報を指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="75e86-228">(`<summary>` ([`<summary>`](documentation-comments.md#summary)) を使用して、型自体と型のメンバーを記述します。</span><span class="sxs-lookup"><span data-stu-id="75e86-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="75e86-229">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-229">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="75e86-230">ここで `description` はコメントのテキストです。</span><span class="sxs-lookup"><span data-stu-id="75e86-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="75e86-231">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-231">__Example:__</span></span>

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

<span data-ttu-id="75e86-232">このタグは、メソッドの戻り値を記述するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="75e86-233">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="75e86-234">ここで `description` は戻り値の説明です。</span><span class="sxs-lookup"><span data-stu-id="75e86-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="75e86-235">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="75e86-236">このタグを使用すると、テキスト内でリンクを指定できます。</span><span class="sxs-lookup"><span data-stu-id="75e86-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="75e86-237">`<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) を使用して、「関連項目」セクションに表示されるテキストを指定します。</span><span class="sxs-lookup"><span data-stu-id="75e86-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="75e86-238">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="75e86-239">ここで `member` はメンバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="75e86-239">where `member` is the name of a member.</span></span> <span data-ttu-id="75e86-240">ドキュメントジェネレーターは、指定されたコード要素が存在することを確認し、生成されたドキュメントファイル内の要素名に*メンバー*を変更します。</span><span class="sxs-lookup"><span data-stu-id="75e86-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="75e86-241">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-241">__Example:__</span></span>

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

<span data-ttu-id="75e86-242">このタグを使用すると、[参照] セクションのエントリも生成されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="75e86-243">`<see>` ([`<see>`](documentation-comments.md#see)) を使用して、テキスト内からリンクを指定します。</span><span class="sxs-lookup"><span data-stu-id="75e86-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="75e86-244">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="75e86-245">ここで `member` はメンバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="75e86-245">where `member` is the name of a member.</span></span> <span data-ttu-id="75e86-246">ドキュメントジェネレーターは、指定されたコード要素が存在することを確認し、生成されたドキュメントファイル内の要素名に*メンバー*を変更します。</span><span class="sxs-lookup"><span data-stu-id="75e86-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="75e86-247">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-247">__Example:__</span></span>

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

このタグは、型または型のメンバーを記述するために使用できます。 <span data-ttu-id="75e86-249">型自体を記述するには、`<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) を使用します。</span><span class="sxs-lookup"><span data-stu-id="75e86-249">Use `<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) to describe the type itself.</span></span>

<span data-ttu-id="75e86-250">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="75e86-251">ここで `description` は、型またはメンバーの概要です。</span><span class="sxs-lookup"><span data-stu-id="75e86-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="75e86-252">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="75e86-253">このタグを使用すると、プロパティを記述できます。</span><span class="sxs-lookup"><span data-stu-id="75e86-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="75e86-254">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="75e86-255">ここで `property description` はプロパティの説明です。</span><span class="sxs-lookup"><span data-stu-id="75e86-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="75e86-256">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="75e86-257">このタグは、クラス、構造体、インターフェイス、デリゲート、またはメソッドのジェネリック型パラメーターを記述するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="75e86-258">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="75e86-259">ここで `name` は型パラメーターの名前、`description` はその説明です。</span><span class="sxs-lookup"><span data-stu-id="75e86-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="75e86-260">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="75e86-261">このタグは、単語が型パラメーターであることを示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="75e86-262">ドキュメントファイルを処理して、この型パラメーターを別の方法で書式設定することができます。</span><span class="sxs-lookup"><span data-stu-id="75e86-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="75e86-263">__文__</span><span class="sxs-lookup"><span data-stu-id="75e86-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="75e86-264">ここで `name` は型パラメーターの名前です。</span><span class="sxs-lookup"><span data-stu-id="75e86-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="75e86-265">__例:__</span><span class="sxs-lookup"><span data-stu-id="75e86-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="75e86-266">ドキュメントファイルを処理しています</span><span class="sxs-lookup"><span data-stu-id="75e86-266">Processing the documentation file</span></span>

<span data-ttu-id="75e86-267">ドキュメントジェネレーターは、ドキュメントコメントでタグ付けされたソースコード内の各要素に対して ID 文字列を生成します。</span><span class="sxs-lookup"><span data-stu-id="75e86-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="75e86-268">この ID 文字列は、ソース要素を一意に識別します。</span><span class="sxs-lookup"><span data-stu-id="75e86-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="75e86-269">ドキュメントビューアーでは、ID 文字列を使用して、ドキュメントが適用される、対応するメタデータ/リフレクション項目を識別できます。</span><span class="sxs-lookup"><span data-stu-id="75e86-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="75e86-270">ドキュメントファイルは、ソースコードの階層的な表現ではありません。代わりに、要素ごとに生成された ID 文字列を含む単純なリストになります。</span><span class="sxs-lookup"><span data-stu-id="75e86-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="75e86-271">ID 文字列の形式</span><span class="sxs-lookup"><span data-stu-id="75e86-271">ID string format</span></span>

<span data-ttu-id="75e86-272">ドキュメントジェネレーターは、ID 文字列を生成するときに、次の規則を監視します。</span><span class="sxs-lookup"><span data-stu-id="75e86-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="75e86-273">文字列に空白は配置されません。</span><span class="sxs-lookup"><span data-stu-id="75e86-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="75e86-274">文字列の最初の部分では、ドキュメント化されているメンバーの種類を、1つの文字の後にコロンを使用して識別します。</span><span class="sxs-lookup"><span data-stu-id="75e86-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="75e86-275">次の種類のメンバーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="75e86-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="75e86-276">__文字__</span><span class="sxs-lookup"><span data-stu-id="75e86-276">__Character__</span></span> | <span data-ttu-id="75e86-277">__説明__</span><span class="sxs-lookup"><span data-stu-id="75e86-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="75e86-278">E</span><span class="sxs-lookup"><span data-stu-id="75e86-278">E</span></span>             | <span data-ttu-id="75e86-279">イベント</span><span class="sxs-lookup"><span data-stu-id="75e86-279">Event</span></span>                                                       |
   | <span data-ttu-id="75e86-280">F</span><span class="sxs-lookup"><span data-stu-id="75e86-280">F</span></span>             | <span data-ttu-id="75e86-281">フィールド</span><span class="sxs-lookup"><span data-stu-id="75e86-281">Field</span></span>                                                       |
   | <span data-ttu-id="75e86-282">M</span><span class="sxs-lookup"><span data-stu-id="75e86-282">M</span></span>             | <span data-ttu-id="75e86-283">メソッド (コンストラクター、デストラクター、および演算子を含む)</span><span class="sxs-lookup"><span data-stu-id="75e86-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="75e86-284">N</span><span class="sxs-lookup"><span data-stu-id="75e86-284">N</span></span>             | <span data-ttu-id="75e86-285">Namespace</span><span class="sxs-lookup"><span data-stu-id="75e86-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="75e86-286">P</span><span class="sxs-lookup"><span data-stu-id="75e86-286">P</span></span>             | <span data-ttu-id="75e86-287">プロパティ (インデクサーを含む)</span><span class="sxs-lookup"><span data-stu-id="75e86-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="75e86-288">T</span><span class="sxs-lookup"><span data-stu-id="75e86-288">T</span></span>             | <span data-ttu-id="75e86-289">型 (クラス、デリゲート、列挙型、インターフェイス、構造体など)</span><span class="sxs-lookup"><span data-stu-id="75e86-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="75e86-290">!</span><span class="sxs-lookup"><span data-stu-id="75e86-290">!</span></span>             | <span data-ttu-id="75e86-291">エラー文字列;文字列の残りの部分は、エラーに関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="75e86-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="75e86-292">たとえば、ドキュメントジェネレーターは、解決できないリンクのエラー情報を生成します。</span><span class="sxs-lookup"><span data-stu-id="75e86-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="75e86-293">文字列の2番目の部分は、要素の完全修飾名であり、名前空間のルートから始まります。</span><span class="sxs-lookup"><span data-stu-id="75e86-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="75e86-294">要素の名前、それを囲む型、および名前空間は、ピリオドで区切られます。</span><span class="sxs-lookup"><span data-stu-id="75e86-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="75e86-295">アイテムの名前にピリオドが含まれている場合は、`#(U+0023)` 文字に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="75e86-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="75e86-296">(この文字が名前に含まれている要素がないことを前提としています)。</span><span class="sxs-lookup"><span data-stu-id="75e86-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="75e86-297">引数を持つメソッドとプロパティについては、かっこで囲まれた引数リストが続きます。</span><span class="sxs-lookup"><span data-stu-id="75e86-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="75e86-298">引数を指定しない場合、かっこは省略されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="75e86-299">引数はコンマで区切られます。</span><span class="sxs-lookup"><span data-stu-id="75e86-299">The arguments are separated by commas.</span></span> <span data-ttu-id="75e86-300">各引数のエンコーディングは、次のように CLI シグネチャと同じです。</span><span class="sxs-lookup"><span data-stu-id="75e86-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="75e86-301">引数は、次のように変更された完全修飾名に基づいて、ドキュメント名によって表されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="75e86-302">ジェネリック型を表す引数には `` ` `` (バックティック) 文字が追加され、その後に型パラメーターの数が続きます。</span><span class="sxs-lookup"><span data-stu-id="75e86-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="75e86-303">`out` または `ref` 修飾子を持つ引数には、型名の後に `@` があります。</span><span class="sxs-lookup"><span data-stu-id="75e86-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="75e86-304">値または `params` によって渡される引数には、特別な表記はありません。</span><span class="sxs-lookup"><span data-stu-id="75e86-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="75e86-305">配列である引数は `[lowerbound:size, ... , lowerbound:size]` として表されます。ここで、コンマの数は1未満の値になり、各次元の下限とサイズ (既知の場合) は10進数で表されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="75e86-306">下限またはサイズが指定されていない場合は省略されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="75e86-307">特定の次元の下限とサイズが省略されている場合は、`:` も省略されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="75e86-308">ジャグ配列は、レベルごとに1つの `[]` で表されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="75e86-309">Void 以外のポインター型を持つ引数は、型名の後の `*` を使用して表されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="75e86-310">Void ポインターは、`System.Void`の型名を使用して表されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="75e86-311">型に対して定義されているジェネリック型パラメーターを参照する引数は、`` ` `` (バックティック) 文字の後に型パラメーターの0から始まるインデックスを使用してエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="75e86-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="75e86-312">メソッドで定義されているジェネリック型パラメーターを使用する引数は、型に使用される `` ` `` ではなく、バックティック ``` `` ``` を使用します。</span><span class="sxs-lookup"><span data-stu-id="75e86-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="75e86-313">構築されたジェネリック型を参照する引数は、ジェネリック型を使用してエンコードされ、その後に `{`が続き、コンマ区切りの型引数リストが続き、その後に `}`が続きます。</span><span class="sxs-lookup"><span data-stu-id="75e86-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="75e86-314">ID 文字列の例</span><span class="sxs-lookup"><span data-stu-id="75e86-314">ID string examples</span></span>

<span data-ttu-id="75e86-315">次の例では、各ソースC#要素から生成された、ドキュメントコメントを持つことができる ID 文字列と共に、コードのフラグメントを示しています。</span><span class="sxs-lookup"><span data-stu-id="75e86-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="75e86-316">型は、完全修飾名を使用して表され、汎用情報で補完されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="75e86-317">フィールドは、完全修飾名で表されます。</span><span class="sxs-lookup"><span data-stu-id="75e86-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="75e86-318">コンストラクター。</span><span class="sxs-lookup"><span data-stu-id="75e86-318">Constructors.</span></span>

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

*  <span data-ttu-id="75e86-319">デストラクタ.</span><span class="sxs-lookup"><span data-stu-id="75e86-319">Destructors.</span></span>

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

*  <span data-ttu-id="75e86-320">メソッド.</span><span class="sxs-lookup"><span data-stu-id="75e86-320">Methods.</span></span>

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

*  <span data-ttu-id="75e86-321">プロパティとインデクサー。</span><span class="sxs-lookup"><span data-stu-id="75e86-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="75e86-322">記録.</span><span class="sxs-lookup"><span data-stu-id="75e86-322">Events.</span></span>

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

*  <span data-ttu-id="75e86-323">単項演算子。</span><span class="sxs-lookup"><span data-stu-id="75e86-323">Unary operators.</span></span>

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

   <span data-ttu-id="75e86-324">使用される単項演算子の関数名の完全なセットは、`op_UnaryPlus`、`op_UnaryNegation`、`op_LogicalNot`、`op_OnesComplement`、`op_Increment`、`op_Decrement`、`op_True`、`op_False`のようになります。</span><span class="sxs-lookup"><span data-stu-id="75e86-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="75e86-325">二項演算子。</span><span class="sxs-lookup"><span data-stu-id="75e86-325">Binary operators.</span></span>

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

   <span data-ttu-id="75e86-326">使用されるバイナリ演算子の関数名の完全なセットは、`op_Addition`、`op_Subtraction`、`op_Multiply`、`op_Division`、`op_Modulus`、`op_BitwiseAnd`、`op_BitwiseOr`、`op_ExclusiveOr`、`op_LeftShift`、`op_RightShift`、`op_Equality`、`op_Inequality`、`op_LessThan`、`op_LessThanOrEqual`、`op_GreaterThan`、`op_GreaterThanOrEqual`です。</span><span class="sxs-lookup"><span data-stu-id="75e86-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="75e86-327">変換演算子には、末尾に "`~`" があり、その後に戻り値の型が続きます。</span><span class="sxs-lookup"><span data-stu-id="75e86-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="75e86-328">使用例</span><span class="sxs-lookup"><span data-stu-id="75e86-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="75e86-329">C#ソースコード</span><span class="sxs-lookup"><span data-stu-id="75e86-329">C# source code</span></span>

<span data-ttu-id="75e86-330">次の例は、`Point` クラスのソースコードを示しています。</span><span class="sxs-lookup"><span data-stu-id="75e86-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="75e86-331">結果の XML</span><span class="sxs-lookup"><span data-stu-id="75e86-331">Resulting XML</span></span>

<span data-ttu-id="75e86-332">次に示すのは、上記のクラス `Point`のソースコードが指定されている場合に、1つのドキュメントジェネレーターによって生成される出力です。</span><span class="sxs-lookup"><span data-stu-id="75e86-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
