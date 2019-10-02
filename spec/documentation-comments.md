---
ms.openlocfilehash: 2026fc1bf9d3576b967cbc2e9a670aa44b7eab3a
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704016"
---
# <a name="documentation-comments"></a><span data-ttu-id="070b7-101">ドキュメント コメント</span><span class="sxs-lookup"><span data-stu-id="070b7-101">Documentation comments</span></span>

<span data-ttu-id="070b7-102">C#XML テキストを含む特殊なコメント構文を使用してコードを文書化するための機構を提供します。</span><span class="sxs-lookup"><span data-stu-id="070b7-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="070b7-103">ソースコードファイルでは、特定のフォームを含むコメントを使用して、そのコメントとソースコード要素の前にあるソースコード要素から XML を生成するようにツールに指示できます。</span><span class="sxs-lookup"><span data-stu-id="070b7-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="070b7-104">このような構文を使用するコメントは、***ドキュメントコメント***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="070b7-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="070b7-105">これらは、ユーザー定義型 (クラス、デリゲート、インターフェイスなど) またはメンバー (フィールド、イベント、プロパティ、メソッドなど) の直前に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="070b7-106">XML 生成ツールは、***ドキュメントジェネレーター***と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="070b7-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="070b7-107">(このジェネレーターはC#コンパイラ自体にすることができますが、そうである必要はありません)。ドキュメントジェネレーターによって生成される出力は、***ドキュメントファイル***と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="070b7-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="070b7-108">ドキュメント***ビューアー***への入力としてドキュメントファイルが使用されます。型情報とそれに関連するドキュメントを視覚的に表示するためのツール。</span><span class="sxs-lookup"><span data-stu-id="070b7-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="070b7-109">この仕様では、ドキュメントコメントで使用されるタグのセットを提示しますが、これらのタグの使用は必須ではなく、適切な形式の XML の規則に従う限り、必要に応じて他のタグを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="070b7-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="070b7-110">概要</span><span class="sxs-lookup"><span data-stu-id="070b7-110">Introduction</span></span>

<span data-ttu-id="070b7-111">特別な形式のコメントを使用すると、そのコメントとソースコード要素の前にあるソースコード要素から XML を生成するようにツールに指示できます。</span><span class="sxs-lookup"><span data-stu-id="070b7-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="070b7-112">このようなコメントは、3つのスラッシュ (`///`) で始まる単一行のコメントか、スラッシュと2つの星 (`/**`) で始まる区切られたコメントです。</span><span class="sxs-lookup"><span data-stu-id="070b7-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="070b7-113">これらは、ユーザー定義型 (クラス、デリゲート、インターフェイスなど)、または注釈を付けるメンバー (フィールド、イベント、プロパティ、メソッドなど) の直前に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="070b7-114">属性セクション ([属性の指定](attributes.md#attribute-specification)) は宣言の一部と見なされるため、ドキュメントコメントは、型またはメンバーに適用される属性の前に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="070b7-115">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="070b7-116">*Single_line_doc_comment*では、現在の*single_line_doc_comment*に隣接する各*single_line_doc_comment*の文字に @no__t 2 文字の後に*空白*文字がある場合、その空白文字が含まれます。文字は XML 出力に含まれません。</span><span class="sxs-lookup"><span data-stu-id="070b7-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="070b7-117">区切られた doc コメントでは、2行目の空白以外の最初の文字がアスタリスクで、省略可能な空白文字のパターンが同じで、区切り文字の先頭にアスタリスク文字が繰り返されている場合は、その後、繰り返されるパターンの文字は XML 出力に含まれません。</span><span class="sxs-lookup"><span data-stu-id="070b7-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="070b7-118">パターンには、アスタリスク文字の前と同様に、空白文字を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="070b7-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="070b7-119">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-119">__Example:__</span></span>

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

<span data-ttu-id="070b7-120">ドキュメントコメント内のテキストは、XML の規則に従って適切な形式 https://www.w3.org/TR/REC-xml) である必要があります (「」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="070b7-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="070b7-121">XML の形式が正しくない場合は、警告が生成され、ドキュメントファイルにはエラーが発生したことを示すコメントが含まれます。</span><span class="sxs-lookup"><span data-stu-id="070b7-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="070b7-122">開発者は独自のタグセットを自由に作成できますが、推奨されるセットは推奨される[タグ](documentation-comments.md#recommended-tags)で定義されています。</span><span class="sxs-lookup"><span data-stu-id="070b7-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="070b7-123">推奨されるタグの一部には特別な意味があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="070b7-124">タグ`<param>`は、パラメーターを記述するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="070b7-125">このようなタグが使用されている場合、ドキュメントジェネレーターは、指定されたパラメーターが存在すること、およびすべてのパラメーターがドキュメントコメントに記述されていることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="070b7-126">このような検証が失敗した場合は、ドキュメントジェネレーターによって警告が発行されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="070b7-127">`cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。</span><span class="sxs-lookup"><span data-stu-id="070b7-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="070b7-128">ドキュメントジェネレーターは、このコード要素が存在することを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="070b7-129">検証が失敗した場合は、ドキュメントジェネレーターによって警告が発行されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="070b7-130">`cref`属性で記述されている名前を検索する場合、ドキュメントジェネレーターは、ソースコード`using`内に出現するステートメントに従って、名前空間の可視性を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="070b7-131">ジェネリックであるコード要素の場合、通常のジェネリック構文 (つまり "`List<T>`") を使用することはできません。これは、無効な XML が生成されるためです。</span><span class="sxs-lookup"><span data-stu-id="070b7-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="070b7-132">かっこ (`List{T}`"") の代わりに中かっこを使用することも、XML エスケープ構文を使用することもできます (つまり`List&lt;T&gt;`、"")。</span><span class="sxs-lookup"><span data-stu-id="070b7-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="070b7-133">タグ`<summary>`は、ドキュメントビューアーが型またはメンバーに関する追加情報を表示するために使用することを目的としています。</span><span class="sxs-lookup"><span data-stu-id="070b7-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="070b7-134">タグ`<include>`には、外部 XML ファイルからの情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="070b7-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="070b7-135">ドキュメントファイルでは、型とメンバーに関する完全な情報が提供されていないことに注意してください (たとえば、型情報が含まれていない場合)。</span><span class="sxs-lookup"><span data-stu-id="070b7-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="070b7-136">型またはメンバーに関する情報を取得するには、ドキュメントファイルを実際の型またはメンバーのリフレクションと組み合わせて使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="070b7-137">推奨されるタグ</span><span class="sxs-lookup"><span data-stu-id="070b7-137">Recommended tags</span></span>

<span data-ttu-id="070b7-138">ドキュメントジェネレーターは、XML の規則に従って有効なすべてのタグを受け入れて処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="070b7-139">次のタグは、ユーザードキュメントで一般的に使用される機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="070b7-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="070b7-140">(もちろん、他のタグも使用できます)。</span><span class="sxs-lookup"><span data-stu-id="070b7-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="070b7-141">__番号__</span><span class="sxs-lookup"><span data-stu-id="070b7-141">__Tag__</span></span>          | <span data-ttu-id="070b7-142">__セクション__</span><span class="sxs-lookup"><span data-stu-id="070b7-142">__Section__</span></span>                                            | <span data-ttu-id="070b7-143">__目的__</span><span class="sxs-lookup"><span data-stu-id="070b7-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="070b7-144">コードに似たフォントでテキストを設定する</span><span class="sxs-lookup"><span data-stu-id="070b7-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="070b7-145">ソースコードまたはプログラム出力の1行以上の行を設定する</span><span class="sxs-lookup"><span data-stu-id="070b7-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="070b7-146">例を示します。</span><span class="sxs-lookup"><span data-stu-id="070b7-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="070b7-147">メソッドがスローできる例外を識別します。</span><span class="sxs-lookup"><span data-stu-id="070b7-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="070b7-148">外部ファイルから XML をインクルードします。</span><span class="sxs-lookup"><span data-stu-id="070b7-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="070b7-149">リストまたはテーブルを作成する</span><span class="sxs-lookup"><span data-stu-id="070b7-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="070b7-150">テキストへの構造の追加を許可する</span><span class="sxs-lookup"><span data-stu-id="070b7-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="070b7-151">メソッドまたはコンストラクターのパラメーターの記述</span><span class="sxs-lookup"><span data-stu-id="070b7-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="070b7-152">単語がパラメーター名であることを識別する</span><span class="sxs-lookup"><span data-stu-id="070b7-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="070b7-153">メンバーのセキュリティアクセシビリティを文書化する</span><span class="sxs-lookup"><span data-stu-id="070b7-153">Document the security accessibility of a member</span></span>        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | <span data-ttu-id="070b7-154">型に関する追加情報の記述</span><span class="sxs-lookup"><span data-stu-id="070b7-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="070b7-155">メソッドの戻り値の説明</span><span class="sxs-lookup"><span data-stu-id="070b7-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="070b7-156">リンクの指定</span><span class="sxs-lookup"><span data-stu-id="070b7-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="070b7-157">関連項目を生成する</span><span class="sxs-lookup"><span data-stu-id="070b7-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="070b7-158">型または型のメンバーの記述</span><span class="sxs-lookup"><span data-stu-id="070b7-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="070b7-159">プロパティの説明</span><span class="sxs-lookup"><span data-stu-id="070b7-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="070b7-160">ジェネリック型パラメーターの記述</span><span class="sxs-lookup"><span data-stu-id="070b7-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="070b7-161">単語が型パラメーター名であることを識別する</span><span class="sxs-lookup"><span data-stu-id="070b7-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="070b7-162">このタグは、記述に含まれるテキストのフラグメントを、コードブロックに使用される特殊なフォントで設定する必要があることを示す機構を提供します。</span><span class="sxs-lookup"><span data-stu-id="070b7-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="070b7-163">実際のコード行の場合は`<code>` 、[`<code>`](documentation-comments.md#code)() を使用します。</span><span class="sxs-lookup"><span data-stu-id="070b7-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="070b7-164">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="070b7-165">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="070b7-166">このタグは、ソースコードまたはプログラム出力の1つ以上の行を特殊なフォントで設定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="070b7-167">ナレーションの小さなコードフラグメントの場合は`<c>` 、[`<c>`](documentation-comments.md#c)() を使用します。</span><span class="sxs-lookup"><span data-stu-id="070b7-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="070b7-168">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="070b7-169">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-169">__Example:__</span></span>

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

<span data-ttu-id="070b7-170">このタグでは、コメント内のコード例を使用して、メソッドまたはその他のライブラリメンバーの使用方法を指定できます。</span><span class="sxs-lookup"><span data-stu-id="070b7-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="070b7-171">通常は、タグ`<code>` ([`<code>`](documentation-comments.md#code)) も使用します。</span><span class="sxs-lookup"><span data-stu-id="070b7-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="070b7-172">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="070b7-173">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-173">__Example:__</span></span>

<span data-ttu-id="070b7-174">例`<code>`に[`<code>`](documentation-comments.md#code)ついては、() を参照してください。</span><span class="sxs-lookup"><span data-stu-id="070b7-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="070b7-175">このタグは、メソッドがスローできる例外を文書化する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="070b7-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="070b7-176">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="070b7-177">パラメーターの説明</span><span class="sxs-lookup"><span data-stu-id="070b7-177">where</span></span>

* <span data-ttu-id="070b7-178">`member`メンバーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="070b7-178">`member` is the name of a member.</span></span> <span data-ttu-id="070b7-179">ドキュメントジェネレーターは、指定されたメンバーが存在`member`することを確認し、ドキュメントファイル内の正規要素名に変換します。</span><span class="sxs-lookup"><span data-stu-id="070b7-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="070b7-180">`description`例外がスローされる状況の説明です。</span><span class="sxs-lookup"><span data-stu-id="070b7-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="070b7-181">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-181">__Example:__</span></span>

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

<span data-ttu-id="070b7-182">このタグを使用すると、ソースコードファイルの外部にある XML ドキュメントの情報を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="070b7-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="070b7-183">外部ファイルは整形式の XML ドキュメントである必要があり、そのドキュメントに含まれる XML を指定するために XPath 式がそのドキュメントに適用されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="070b7-184">`<include>`タグは、外部ドキュメントから選択された XML に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="070b7-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="070b7-185">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-185">__Syntax:__</span></span>

```xml
<include file="filename" path="xpath" />
```

<span data-ttu-id="070b7-186">パラメーターの説明</span><span class="sxs-lookup"><span data-stu-id="070b7-186">where</span></span>

* <span data-ttu-id="070b7-187">`filename`外部 XML ファイルのファイル名を指定します。</span><span class="sxs-lookup"><span data-stu-id="070b7-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="070b7-188">ファイル名は、include タグを含むファイルに対して相対的に解釈されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="070b7-189">`xpath`外部 XML ファイル内の XML の一部を選択する XPath 式です。</span><span class="sxs-lookup"><span data-stu-id="070b7-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="070b7-190">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-190">__Example:__</span></span>

<span data-ttu-id="070b7-191">ソースコードに次のような宣言が含まれている場合:</span><span class="sxs-lookup"><span data-stu-id="070b7-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="070b7-192">外部ファイル "docs .xml" には次の内容が含まれていました。</span><span class="sxs-lookup"><span data-stu-id="070b7-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="070b7-193">次に、同じドキュメントが、ソースコードに含まれているものとして出力されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="070b7-194">このタグは、項目のリストまたはテーブルを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="070b7-195">テーブルまたは定義`<listheader>`リストの見出し行を定義するブロックが含まれている場合があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="070b7-196">(テーブルを定義する場合は、見出し内`term`ののエントリだけを指定する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="070b7-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="070b7-197">リスト内の各項目には、 `<item>`ブロックが指定されています。</span><span class="sxs-lookup"><span data-stu-id="070b7-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="070b7-198">定義リストを作成する場合は`term` 、 `description`との両方を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="070b7-199">ただし、テーブル、箇条書きリスト、番号付きリストの場合は、 `description`のみ指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="070b7-200">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-200">__Syntax:__</span></span>

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

<span data-ttu-id="070b7-201">パラメーターの説明</span><span class="sxs-lookup"><span data-stu-id="070b7-201">where</span></span>

* <span data-ttu-id="070b7-202">`term`定義を定義する用語を指定します。 `description`定義はにあります。</span><span class="sxs-lookup"><span data-stu-id="070b7-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="070b7-203">`description`は、箇条書きまたは番号付きリストの項目、またはの`term`定義のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="070b7-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="070b7-204">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-204">__Example:__</span></span>

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

<span data-ttu-id="070b7-205">このタグは`<summary>` 、([`<remarks>`](documentation-comments.md#remarks)) や`<returns>` ([`<returns>`](documentation-comments.md#returns)) などの他のタグ内で使用するためのもので、テキストに構造を追加することを許可します。</span><span class="sxs-lookup"><span data-stu-id="070b7-205">This tag is for use inside other tags, such as `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="070b7-206">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="070b7-207">ここ`content`で、は段落のテキストです。</span><span class="sxs-lookup"><span data-stu-id="070b7-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="070b7-208">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-208">__Example:__</span></span>

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

<span data-ttu-id="070b7-209">このタグは、メソッド、コンストラクター、またはインデクサーのパラメーターを記述するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="070b7-210">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="070b7-211">パラメーターの説明</span><span class="sxs-lookup"><span data-stu-id="070b7-211">where</span></span>

* <span data-ttu-id="070b7-212">`name`パラメーターの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="070b7-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="070b7-213">`description`パラメーターの説明を指定します。</span><span class="sxs-lookup"><span data-stu-id="070b7-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="070b7-214">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-214">__Example:__</span></span>

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

<span data-ttu-id="070b7-215">このタグは、単語がパラメーターであることを示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="070b7-216">ドキュメントファイルを処理して、このパラメーターを別の方法で書式設定することができます。</span><span class="sxs-lookup"><span data-stu-id="070b7-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="070b7-217">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="070b7-218">ここ`name`で、はパラメーターの名前です。</span><span class="sxs-lookup"><span data-stu-id="070b7-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="070b7-219">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-219">__Example:__</span></span>

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

<span data-ttu-id="070b7-220">このタグにより、メンバーのセキュリティアクセシビリティを文書化することができます。</span><span class="sxs-lookup"><span data-stu-id="070b7-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="070b7-221">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="070b7-222">パラメーターの説明</span><span class="sxs-lookup"><span data-stu-id="070b7-222">where</span></span>

* <span data-ttu-id="070b7-223">`member`メンバーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="070b7-223">`member` is the name of a member.</span></span> <span data-ttu-id="070b7-224">ドキュメントジェネレーターは、指定されたコード要素が存在することを確認し、*メンバー*をドキュメントファイル内の正規要素名に変換します。</span><span class="sxs-lookup"><span data-stu-id="070b7-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="070b7-225">`description`メンバーへのアクセスの説明を示します。</span><span class="sxs-lookup"><span data-stu-id="070b7-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="070b7-226">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

<span data-ttu-id="070b7-227">このタグは、型に関する追加情報を指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="070b7-228">(( `<summary>` [`<summary>`](documentation-comments.md#summary)) を使用して、型自体と型のメンバーを記述します)。</span><span class="sxs-lookup"><span data-stu-id="070b7-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="070b7-229">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-229">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="070b7-230">ここ`description`で、はコメントのテキストです。</span><span class="sxs-lookup"><span data-stu-id="070b7-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="070b7-231">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-231">__Example:__</span></span>

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

<span data-ttu-id="070b7-232">このタグは、メソッドの戻り値を記述するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="070b7-233">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="070b7-234">ここ`description`で、は戻り値の説明です。</span><span class="sxs-lookup"><span data-stu-id="070b7-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="070b7-235">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="070b7-236">このタグを使用すると、テキスト内でリンクを指定できます。</span><span class="sxs-lookup"><span data-stu-id="070b7-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="070b7-237">`<seealso>` [(`<seealso>`](documentation-comments.md#seealso)) を使用して、[参照] セクションに表示されるテキストを指定します。</span><span class="sxs-lookup"><span data-stu-id="070b7-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="070b7-238">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="070b7-239">ここ`member`で、はメンバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="070b7-239">where `member` is the name of a member.</span></span> <span data-ttu-id="070b7-240">ドキュメントジェネレーターは、指定されたコード要素が存在することを確認し、生成されたドキュメントファイル内の要素名に*メンバー*を変更します。</span><span class="sxs-lookup"><span data-stu-id="070b7-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="070b7-241">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-241">__Example:__</span></span>

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

<span data-ttu-id="070b7-242">このタグを使用すると、[参照] セクションのエントリも生成されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="070b7-243">`<see>` [(`<see>`](documentation-comments.md#see)) を使用して、テキスト内からリンクを指定します。</span><span class="sxs-lookup"><span data-stu-id="070b7-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="070b7-244">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="070b7-245">ここ`member`で、はメンバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="070b7-245">where `member` is the name of a member.</span></span> <span data-ttu-id="070b7-246">ドキュメントジェネレーターは、指定されたコード要素が存在することを確認し、生成されたドキュメントファイル内の要素名に*メンバー*を変更します。</span><span class="sxs-lookup"><span data-stu-id="070b7-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="070b7-247">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-247">__Example:__</span></span>

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

このタグは、型または型のメンバーを記述するために使用できます。 <span data-ttu-id="070b7-249">型`<remarks>`自体[`<remarks>`](documentation-comments.md#remarks)を記述するには、() を使用します。</span><span class="sxs-lookup"><span data-stu-id="070b7-249">Use `<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) to describe the type itself.</span></span>

<span data-ttu-id="070b7-250">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="070b7-251">ここ`description`で、は型またはメンバーの概要です。</span><span class="sxs-lookup"><span data-stu-id="070b7-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="070b7-252">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="070b7-253">このタグを使用すると、プロパティを記述できます。</span><span class="sxs-lookup"><span data-stu-id="070b7-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="070b7-254">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="070b7-255">ここ`property description`で、はプロパティの説明です。</span><span class="sxs-lookup"><span data-stu-id="070b7-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="070b7-256">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="070b7-257">このタグは、クラス、構造体、インターフェイス、デリゲート、またはメソッドのジェネリック型パラメーターを記述するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="070b7-258">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="070b7-259">ここ`name`で、は型パラメーター `description`の名前、は説明です。</span><span class="sxs-lookup"><span data-stu-id="070b7-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="070b7-260">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="070b7-261">このタグは、単語が型パラメーターであることを示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="070b7-262">ドキュメントファイルを処理して、この型パラメーターを別の方法で書式設定することができます。</span><span class="sxs-lookup"><span data-stu-id="070b7-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="070b7-263">__文__</span><span class="sxs-lookup"><span data-stu-id="070b7-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="070b7-264">ここ`name`で、は型パラメーターの名前です。</span><span class="sxs-lookup"><span data-stu-id="070b7-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="070b7-265">__例:__</span><span class="sxs-lookup"><span data-stu-id="070b7-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="070b7-266">ドキュメントファイルを処理しています</span><span class="sxs-lookup"><span data-stu-id="070b7-266">Processing the documentation file</span></span>

<span data-ttu-id="070b7-267">ドキュメントジェネレーターは、ドキュメントコメントでタグ付けされたソースコード内の各要素に対して ID 文字列を生成します。</span><span class="sxs-lookup"><span data-stu-id="070b7-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="070b7-268">この ID 文字列は、ソース要素を一意に識別します。</span><span class="sxs-lookup"><span data-stu-id="070b7-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="070b7-269">ドキュメントビューアーでは、ID 文字列を使用して、ドキュメントが適用される、対応するメタデータ/リフレクション項目を識別できます。</span><span class="sxs-lookup"><span data-stu-id="070b7-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="070b7-270">ドキュメントファイルは、ソースコードの階層的な表現ではありません。代わりに、要素ごとに生成された ID 文字列を含む単純なリストになります。</span><span class="sxs-lookup"><span data-stu-id="070b7-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="070b7-271">ID 文字列の形式</span><span class="sxs-lookup"><span data-stu-id="070b7-271">ID string format</span></span>

<span data-ttu-id="070b7-272">ドキュメントジェネレーターは、ID 文字列を生成するときに、次の規則を監視します。</span><span class="sxs-lookup"><span data-stu-id="070b7-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="070b7-273">文字列に空白は配置されません。</span><span class="sxs-lookup"><span data-stu-id="070b7-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="070b7-274">文字列の最初の部分では、ドキュメント化されているメンバーの種類を、1つの文字の後にコロンを使用して識別します。</span><span class="sxs-lookup"><span data-stu-id="070b7-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="070b7-275">次の種類のメンバーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="070b7-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="070b7-276">__文字__</span><span class="sxs-lookup"><span data-stu-id="070b7-276">__Character__</span></span> | <span data-ttu-id="070b7-277">__[説明]__</span><span class="sxs-lookup"><span data-stu-id="070b7-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="070b7-278">E</span><span class="sxs-lookup"><span data-stu-id="070b7-278">E</span></span>             | <span data-ttu-id="070b7-279">イベント</span><span class="sxs-lookup"><span data-stu-id="070b7-279">Event</span></span>                                                       |
   | <span data-ttu-id="070b7-280">F</span><span class="sxs-lookup"><span data-stu-id="070b7-280">F</span></span>             | <span data-ttu-id="070b7-281">フィールド</span><span class="sxs-lookup"><span data-stu-id="070b7-281">Field</span></span>                                                       |
   | <span data-ttu-id="070b7-282">M</span><span class="sxs-lookup"><span data-stu-id="070b7-282">M</span></span>             | <span data-ttu-id="070b7-283">メソッド (コンストラクター、デストラクター、および演算子を含む)</span><span class="sxs-lookup"><span data-stu-id="070b7-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="070b7-284">N</span><span class="sxs-lookup"><span data-stu-id="070b7-284">N</span></span>             | <span data-ttu-id="070b7-285">Namespace</span><span class="sxs-lookup"><span data-stu-id="070b7-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="070b7-286">P</span><span class="sxs-lookup"><span data-stu-id="070b7-286">P</span></span>             | <span data-ttu-id="070b7-287">プロパティ (インデクサーを含む)</span><span class="sxs-lookup"><span data-stu-id="070b7-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="070b7-288">T</span><span class="sxs-lookup"><span data-stu-id="070b7-288">T</span></span>             | <span data-ttu-id="070b7-289">型 (クラス、デリゲート、列挙型、インターフェイス、構造体など)</span><span class="sxs-lookup"><span data-stu-id="070b7-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="070b7-290">!</span><span class="sxs-lookup"><span data-stu-id="070b7-290">!</span></span>             | <span data-ttu-id="070b7-291">エラー文字列;文字列の残りの部分は、エラーに関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="070b7-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="070b7-292">たとえば、ドキュメントジェネレーターは、解決できないリンクのエラー情報を生成します。</span><span class="sxs-lookup"><span data-stu-id="070b7-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="070b7-293">文字列の2番目の部分は、要素の完全修飾名であり、名前空間のルートから始まります。</span><span class="sxs-lookup"><span data-stu-id="070b7-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="070b7-294">要素の名前、それを囲む型、および名前空間は、ピリオドで区切られます。</span><span class="sxs-lookup"><span data-stu-id="070b7-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="070b7-295">項目の名前にピリオドが含まれている場合は、文字`#(U+0023)`で置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="070b7-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="070b7-296">(この文字が名前に含まれている要素がないことを前提としています)。</span><span class="sxs-lookup"><span data-stu-id="070b7-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="070b7-297">引数を持つメソッドとプロパティについては、かっこで囲まれた引数リストが続きます。</span><span class="sxs-lookup"><span data-stu-id="070b7-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="070b7-298">引数を指定しない場合、かっこは省略されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="070b7-299">引数はコンマで区切られます。</span><span class="sxs-lookup"><span data-stu-id="070b7-299">The arguments are separated by commas.</span></span> <span data-ttu-id="070b7-300">各引数のエンコーディングは、次のように CLI シグネチャと同じです。</span><span class="sxs-lookup"><span data-stu-id="070b7-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="070b7-301">引数は、次のように変更された完全修飾名に基づいて、ドキュメント名によって表されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="070b7-302">ジェネリック型を表す引数には、 `` ` ``追加された (バックティック) 文字の後に型パラメーターの数が続きます。</span><span class="sxs-lookup"><span data-stu-id="070b7-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="070b7-303">または`out` `ref`修飾子を持つ引数の`@`型名は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="070b7-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="070b7-304">値または via `params`によって渡される引数には、特別な表記はありません。</span><span class="sxs-lookup"><span data-stu-id="070b7-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="070b7-305">配列として`[lowerbound:size, ... , lowerbound:size]`指定される引数は、コンマの数が1未満の値で表され、各次元の下限とサイズ (既知の場合) が decimal で表されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="070b7-306">下限またはサイズが指定されていない場合は省略されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="070b7-307">特定の次元の下限とサイズが省略されている場合`:`は、も省略されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="070b7-308">ジャグ配列は、1レベル`[]`につき1つので表されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="070b7-309">Void 以外のポインター型を持つ引数は、次の`*`型名を使用して表されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="070b7-310">Void ポインターは、型名`System.Void`を使用して表されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="070b7-311">型に対して定義されているジェネリック型パラメーターを参照`` ` ``する引数は、(バックティック) 文字の後に型パラメーターの0から始まるインデックスを使用してエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="070b7-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="070b7-312">メソッドで定義されているジェネリック型パラメーターを使用する引数``` `` ```は、型`` ` ``に使用されるではなく、二重バックティックを使用します。</span><span class="sxs-lookup"><span data-stu-id="070b7-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="070b7-313">構築されたジェネリック型を参照する引数は、ジェネリック型を使用`{`してエンコードされ、それに続いて、の後に`}`型引数のコンマ区切りリストが続きます。</span><span class="sxs-lookup"><span data-stu-id="070b7-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="070b7-314">ID 文字列の例</span><span class="sxs-lookup"><span data-stu-id="070b7-314">ID string examples</span></span>

<span data-ttu-id="070b7-315">次の例では、各ソースC#要素から生成された、ドキュメントコメントを持つことができる ID 文字列と共に、コードのフラグメントを示しています。</span><span class="sxs-lookup"><span data-stu-id="070b7-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="070b7-316">型は、完全修飾名を使用して表され、汎用情報で補完されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="070b7-317">フィールドは、完全修飾名で表されます。</span><span class="sxs-lookup"><span data-stu-id="070b7-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="070b7-318">コンストラクター。</span><span class="sxs-lookup"><span data-stu-id="070b7-318">Constructors.</span></span>

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

*  <span data-ttu-id="070b7-319">デストラクタ.</span><span class="sxs-lookup"><span data-stu-id="070b7-319">Destructors.</span></span>

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

*  <span data-ttu-id="070b7-320">メソッド.</span><span class="sxs-lookup"><span data-stu-id="070b7-320">Methods.</span></span>

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

*  <span data-ttu-id="070b7-321">プロパティとインデクサー。</span><span class="sxs-lookup"><span data-stu-id="070b7-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="070b7-322">記録.</span><span class="sxs-lookup"><span data-stu-id="070b7-322">Events.</span></span>

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

*  <span data-ttu-id="070b7-323">単項演算子。</span><span class="sxs-lookup"><span data-stu-id="070b7-323">Unary operators.</span></span>

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

   <span data-ttu-id="070b7-324">使用される単項演算子関数名の完全なセットは`op_UnaryPlus` `op_Decrement` `op_UnaryNegation` `op_LogicalNot`、、、 `op_OnesComplement`、 `op_Increment` `op_True`、、、、 `op_False`およびです。</span><span class="sxs-lookup"><span data-stu-id="070b7-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="070b7-325">二項演算子。</span><span class="sxs-lookup"><span data-stu-id="070b7-325">Binary operators.</span></span>

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

   <span data-ttu-id="070b7-326">使用される二項演算子関数名の完全なセットを次`op_Addition`に示します。 `op_Modulus` `op_Multiply`、 `op_BitwiseAnd` `op_Subtraction`、 `op_BitwiseOr`、 `op_ExclusiveOr` `op_Division` `op_LeftShift` `op_RightShift`、、、、、、、`op_Equality` 、`op_Inequality`、 、`op_GreaterThan`、、および。`op_GreaterThanOrEqual` `op_LessThan` `op_LessThanOrEqual`</span><span class="sxs-lookup"><span data-stu-id="070b7-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="070b7-327">変換演算子には、末尾`~`に "" の後に戻り値の型があります。</span><span class="sxs-lookup"><span data-stu-id="070b7-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="070b7-328">使用例</span><span class="sxs-lookup"><span data-stu-id="070b7-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="070b7-329">C#ソースコード</span><span class="sxs-lookup"><span data-stu-id="070b7-329">C# source code</span></span>

<span data-ttu-id="070b7-330">次の例は、 `Point`クラスのソースコードを示しています。</span><span class="sxs-lookup"><span data-stu-id="070b7-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="070b7-331">結果の XML</span><span class="sxs-lookup"><span data-stu-id="070b7-331">Resulting XML</span></span>

<span data-ttu-id="070b7-332">次に示すのは、上記のクラス`Point`のソースコードを指定した場合に、1つのドキュメントジェネレーターによって生成される出力です。</span><span class="sxs-lookup"><span data-stu-id="070b7-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
