---
ms.openlocfilehash: c9f8417dc68153f02ceb72bb1d51f3615f3c4961
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54272047"
---
# <a name="documentation-comments"></a><span data-ttu-id="efd05-101">ドキュメントのコメント</span><span class="sxs-lookup"><span data-stu-id="efd05-101">Documentation comments</span></span>

<span data-ttu-id="efd05-102">C# プログラマを XML テキストを含む特殊なコメント構文を使用してコードを文書化するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="efd05-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="efd05-103">ソース コード ファイルでこのようなコメントとその直後にあるソース コード要素から XML を生成するツールに指示する特定の形式のコメントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="efd05-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="efd05-104">コメントを使用してこのような構文が呼び出される***ドキュメントのコメント***します。</span><span class="sxs-lookup"><span data-stu-id="efd05-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="efd05-105">(クラス、デリゲート、またはインターフェイス) などのユーザー定義型またはメンバー (フィールド、イベント、プロパティ、メソッドなど) の直前する必要がありますに。</span><span class="sxs-lookup"><span data-stu-id="efd05-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="efd05-106">XML の生成ツールと呼ばれる、***ドキュメント ジェネレーター***します。</span><span class="sxs-lookup"><span data-stu-id="efd05-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="efd05-107">(このジェネレーターが必要はありません、C# コンパイラ自体)。ドキュメント ジェネレーターによって生成される出力と呼ばれる、***ドキュメント ファイル***します。</span><span class="sxs-lookup"><span data-stu-id="efd05-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="efd05-108">ドキュメント ファイルが入力として使用、***ドキュメント ビューアー***です。 ツールがなんらかの種類の情報とその関連するドキュメントのビジュアル表示を生成するためのものです。</span><span class="sxs-lookup"><span data-stu-id="efd05-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="efd05-109">この仕様には一連のドキュメントのコメントで使用されるタグがこれらのタグの使用は必須ではありませんし、その他のタグとして使用できる場合は、必要に応じて、整形式 XML の長時間の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="efd05-110">はじめに</span><span class="sxs-lookup"><span data-stu-id="efd05-110">Introduction</span></span>

<span data-ttu-id="efd05-111">このようなコメントとその直後にあるソース コード要素から XML を生成するツールに指示する特殊な形式のコメントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="efd05-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="efd05-112">このようなコメントは、3 つのスラッシュで始まる単一行コメント (`///`)、またはスラッシュを 2 つの星で始まるコメントの区切り (`/**`)。</span><span class="sxs-lookup"><span data-stu-id="efd05-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="efd05-113">ユーザー定義型 (クラス、デリゲート、またはインターフェイス) など、またはこれらの注釈を設定するメンバー (フィールド、イベント、プロパティ、メソッドなど) の直前する必要がありますに。</span><span class="sxs-lookup"><span data-stu-id="efd05-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="efd05-114">セクションの属性 ([属性の指定](attributes.md#attribute-specification)) ドキュメントのコメントは、型またはメンバーに適用される属性を付ける必要がありますので、宣言の一部と見なされます。</span><span class="sxs-lookup"><span data-stu-id="efd05-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="efd05-115">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="efd05-116">*Single_line_doc_comment*がある場合を*空白*文字以下、`///`の各文字、 *single_line_doc_comment*隣接する s現在*single_line_doc_comment*、しを*空白*文字は XML 出力に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="efd05-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="efd05-117">区切られた-ドキュメントのコメントに場合 2 つ目の行の最初の空白以外の文字はアスタリスクと同じパターンの省略可能な空白文字とアスタリスク文字が繰り返されます各区切り-ドキュメントのコメント内の行の先頭に繰り返しパターンの文字は、XML 出力には含まれません。</span><span class="sxs-lookup"><span data-stu-id="efd05-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="efd05-118">パターンは、後に、および、アスタリスク文字の前に、空白文字を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="efd05-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="efd05-119">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-119">__Example:__</span></span>

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

<span data-ttu-id="efd05-120">XML の規則に従って、ドキュメントのコメント内のテキストを正しい形式である必要があります (https://www.w3.org/TR/REC-xml)します。</span><span class="sxs-lookup"><span data-stu-id="efd05-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="efd05-121">XML の形式申し上げますが場合、は、警告が生成され、ドキュメント ファイルは、エラーが発生したことを示すコメントが含まれます。</span><span class="sxs-lookup"><span data-stu-id="efd05-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="efd05-122">推奨される設定が定義されている開発者は、独自のタグのセットを作成できますが、[推奨されるタグ](documentation-comments.md#recommended-tags)します。</span><span class="sxs-lookup"><span data-stu-id="efd05-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="efd05-123">推奨されるタグの一部には特別な意味があります。</span><span class="sxs-lookup"><span data-stu-id="efd05-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="efd05-124">`<param>`タグを使用して、パラメーターを説明します。</span><span class="sxs-lookup"><span data-stu-id="efd05-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="efd05-125">このようなタグを使用する場合、指定されたパラメーターが存在して、すべてのパラメーターがドキュメントのコメントに記述されているドキュメント ジェネレーターを確認します必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd05-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="efd05-126">このような検証に失敗した場合、ドキュメントの生成は警告を発行します。</span><span class="sxs-lookup"><span data-stu-id="efd05-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="efd05-127">`cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。</span><span class="sxs-lookup"><span data-stu-id="efd05-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="efd05-128">ドキュメント ジェネレーターは、このコード要素が存在することを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd05-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="efd05-129">検証に失敗した場合、ドキュメント ジェネレーターは、警告を発行します。</span><span class="sxs-lookup"><span data-stu-id="efd05-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="efd05-130">説明されている名前を検索するときに、`cref`属性、ドキュメント ジェネレーターには、に従って名前空間の可視性が考慮する必要があります`using`ソース コード内に出現するステートメント。</span><span class="sxs-lookup"><span data-stu-id="efd05-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="efd05-131">コード要素の一般的な通常の一般的な構文 (つまり、"`List<T>`") 無効な XML を生成するために使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="efd05-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="efd05-132">中かっこを角かっこではなく使用できます (つまり、"`List{T}`")、または XML エスケープ構文を使用することができます (つまり、"`List&lt;T&gt;`")。</span><span class="sxs-lookup"><span data-stu-id="efd05-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="efd05-133">`<summary>`タグは、型またはメンバーに関する情報を表示する、ドキュメント ビューアーで使用されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="efd05-134">`<include>`タグには、外部の XML ファイルからの情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="efd05-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="efd05-135">ドキュメント ファイルが、型とメンバーの完全な情報を提供しないことに慎重に注意してください (たとえば、任意の種類の情報を含まれません)。</span><span class="sxs-lookup"><span data-stu-id="efd05-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="efd05-136">型またはメンバーに関するこのような情報を取得するには、実際の型またはメンバーのリフレクションと組み合わせてドキュメント ファイルを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd05-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="efd05-137">推奨されるタグ</span><span class="sxs-lookup"><span data-stu-id="efd05-137">Recommended tags</span></span>

<span data-ttu-id="efd05-138">ドキュメント ジェネレーターは、そのまま使用し、XML の規則に従って有効な任意のタグを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd05-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="efd05-139">次のタグは、ユーザー ドキュメントでよく使用される機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="efd05-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="efd05-140">(もちろん、その他のタグは可能) です。</span><span class="sxs-lookup"><span data-stu-id="efd05-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="efd05-141">__タグ__</span><span class="sxs-lookup"><span data-stu-id="efd05-141">__Tag__</span></span>          | <span data-ttu-id="efd05-142">__セクション__</span><span class="sxs-lookup"><span data-stu-id="efd05-142">__Section__</span></span>                                            | <span data-ttu-id="efd05-143">__目的__</span><span class="sxs-lookup"><span data-stu-id="efd05-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="efd05-144">コードのようなフォントでテキストを設定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="efd05-145">1 つまたは複数の行のソース コードまたはプログラムの出力の設定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="efd05-146">例を示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="efd05-147">メソッドがスローできる例外を識別します。</span><span class="sxs-lookup"><span data-stu-id="efd05-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="efd05-148">外部ファイルから XML が含まれています</span><span class="sxs-lookup"><span data-stu-id="efd05-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="efd05-149">リストまたはテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="efd05-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="efd05-150">テキストに追加される構造体を許可します。</span><span class="sxs-lookup"><span data-stu-id="efd05-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="efd05-151">メソッドまたはコンス トラクターのパラメーターについて説明します</span><span class="sxs-lookup"><span data-stu-id="efd05-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="efd05-152">単語がパラメーター名であることを示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="efd05-153">メンバーのセキュリティのユーザー補助ドキュメント</span><span class="sxs-lookup"><span data-stu-id="efd05-153">Document the security accessibility of a member</span></span>        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | <span data-ttu-id="efd05-154">型に関する追加情報について説明します</span><span class="sxs-lookup"><span data-stu-id="efd05-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="efd05-155">メソッドの戻り値について説明します</span><span class="sxs-lookup"><span data-stu-id="efd05-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="efd05-156">リンクを指定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="efd05-157">「参照」エントリを生成します。</span><span class="sxs-lookup"><span data-stu-id="efd05-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="efd05-158">型または型のメンバーについて説明します</span><span class="sxs-lookup"><span data-stu-id="efd05-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="efd05-159">プロパティを説明します。</span><span class="sxs-lookup"><span data-stu-id="efd05-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="efd05-160">ジェネリック型パラメーターについて説明します</span><span class="sxs-lookup"><span data-stu-id="efd05-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="efd05-161">単語が型パラメーター名であることを示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="efd05-162">このタグは、コードのブロックを使用するなどの特殊なフォントの説明内のテキストの一部を設定することを示すメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="efd05-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="efd05-163">行の実際のコードで使用`<code>`([`<code>`](documentation-comments.md#code))。</span><span class="sxs-lookup"><span data-stu-id="efd05-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="efd05-164">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="efd05-165">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="efd05-166">このタグを使用して、いくつかの特別なフォントで 1 つまたは複数の行のソース コードまたはプログラムの出力を設定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="efd05-167">物語で小さなコード フラグメントを使用して`<c>`([`<c>`](documentation-comments.md#c))。</span><span class="sxs-lookup"><span data-stu-id="efd05-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="efd05-168">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="efd05-169">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-169">__Example:__</span></span>

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

<span data-ttu-id="efd05-170">このタグは、メソッド、またはその他のライブラリ メンバーを使用することがある方法を指定する、コメント内のコード例を使用します。</span><span class="sxs-lookup"><span data-stu-id="efd05-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="efd05-171">通常、これも含まれるタグの使用`<code>`([`<code>`](documentation-comments.md#code)) もします。</span><span class="sxs-lookup"><span data-stu-id="efd05-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="efd05-172">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="efd05-173">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-173">__Example:__</span></span>

<span data-ttu-id="efd05-174">参照してください`<code>`([`<code>`](documentation-comments.md#code)) 例についてはします。</span><span class="sxs-lookup"><span data-stu-id="efd05-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="efd05-175">このタグは、メソッドがスローできる例外を文書化する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="efd05-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="efd05-176">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="efd05-177">where</span><span class="sxs-lookup"><span data-stu-id="efd05-177">where</span></span>

* <span data-ttu-id="efd05-178">`member` メンバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="efd05-178">`member` is the name of a member.</span></span> <span data-ttu-id="efd05-179">ドキュメント ジェネレーターは、指定したメンバーが存在し、変換を確認します。`member`ドキュメント ファイルで正規要素名にします。</span><span class="sxs-lookup"><span data-stu-id="efd05-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="efd05-180">`description` 例外がスローされた状況の説明。</span><span class="sxs-lookup"><span data-stu-id="efd05-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="efd05-181">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-181">__Example:__</span></span>

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

<span data-ttu-id="efd05-182">このタグは、ソース コード ファイルの外部にある XML ドキュメントからの情報などを使用します。</span><span class="sxs-lookup"><span data-stu-id="efd05-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="efd05-183">外部のファイルは、整形式 XML ドキュメントである必要があり、XPath 式に含めるそのドキュメントでは、どのような XML を指定するには、そのドキュメントに適用されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="efd05-184">`<include>`タグは、選択した外部のドキュメントから XML に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="efd05-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="efd05-185">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="efd05-186">where</span><span class="sxs-lookup"><span data-stu-id="efd05-186">where</span></span>

* <span data-ttu-id="efd05-187">`filename` 外部 XML ファイルのファイル名です。</span><span class="sxs-lookup"><span data-stu-id="efd05-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="efd05-188">ファイル名は、インクルード タグを含むファイルを基準に解釈されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="efd05-189">`xpath` 外部 XML ファイルで、XML の一部を選択する XPath 式です。</span><span class="sxs-lookup"><span data-stu-id="efd05-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="efd05-190">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-190">__Example:__</span></span>

<span data-ttu-id="efd05-191">場合は、ソース コードには、ような宣言が含まれています。</span><span class="sxs-lookup"><span data-stu-id="efd05-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="efd05-192">外部のファイル"docs.xml"が、次の内容。</span><span class="sxs-lookup"><span data-stu-id="efd05-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="efd05-193">同じドキュメントは、ソース コードが含まれている場合、出力を示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="efd05-194">このタグは、リストまたは項目のテーブルの作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="efd05-195">`<listheader>`のテーブルまたは定義の一覧の見出し行を定義するブロック。</span><span class="sxs-lookup"><span data-stu-id="efd05-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="efd05-196">(テーブルのエントリのみを定義するときに`term`の見出しを指定する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="efd05-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="efd05-197">リスト内の各項目を指定した、`<item>`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="efd05-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="efd05-198">定義の一覧を作成するときに両方`term`と`description`指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd05-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="efd05-199">ただしのテーブル、箇条書きリスト、または番号付きリスト、のみ`description`指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd05-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="efd05-200">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-200">__Syntax:__</span></span>

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

<span data-ttu-id="efd05-201">where</span><span class="sxs-lookup"><span data-stu-id="efd05-201">where</span></span>

* <span data-ttu-id="efd05-202">`term` この用語を定義するのですが`description`します。</span><span class="sxs-lookup"><span data-stu-id="efd05-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="efd05-203">`description` 箇条書きまたは番号付きリスト内の項目またはの定義、`term`します。</span><span class="sxs-lookup"><span data-stu-id="efd05-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="efd05-204">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-204">__Example:__</span></span>

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

<span data-ttu-id="efd05-205">このタグは、その他のタグ内で使用できるよう`<summary>`([`<remark>`](documentation-comments.md#remark)) または`<returns>`([`<returns>`](documentation-comments.md#returns))、構造体をテキストに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="efd05-205">This tag is for use inside other tags, such as `<summary>` ([`<remark>`](documentation-comments.md#remark)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="efd05-206">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="efd05-207">場所`content`段落のテキストです。</span><span class="sxs-lookup"><span data-stu-id="efd05-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="efd05-208">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-208">__Example:__</span></span>

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

<span data-ttu-id="efd05-209">このタグを使用して、メソッド、コンス トラクター、またはインデクサーのパラメーターを説明します。</span><span class="sxs-lookup"><span data-stu-id="efd05-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="efd05-210">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="efd05-211">where</span><span class="sxs-lookup"><span data-stu-id="efd05-211">where</span></span>

* <span data-ttu-id="efd05-212">`name` パラメーターの名前です。</span><span class="sxs-lookup"><span data-stu-id="efd05-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="efd05-213">`description` パラメーターの説明。</span><span class="sxs-lookup"><span data-stu-id="efd05-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="efd05-214">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-214">__Example:__</span></span>

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

<span data-ttu-id="efd05-215">このタグを使用して、単語がパラメーターであることを示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="efd05-216">ドキュメント ファイルを処理することで、何らかの方法でこのパラメーターの書式を設定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="efd05-217">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="efd05-218">場所`name`パラメーターの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="efd05-219">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-219">__Example:__</span></span>

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

<span data-ttu-id="efd05-220">このタグは、文書化するメンバーのセキュリティのユーザー補助機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="efd05-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="efd05-221">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="efd05-222">where</span><span class="sxs-lookup"><span data-stu-id="efd05-222">where</span></span>

* <span data-ttu-id="efd05-223">`member` メンバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="efd05-223">`member` is the name of a member.</span></span> <span data-ttu-id="efd05-224">ドキュメントの生成は、指定されたコード要素が存在し、変換を確認します。*メンバー*ドキュメント ファイルで正規要素名にします。</span><span class="sxs-lookup"><span data-stu-id="efd05-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="efd05-225">`description` メンバーへのアクセスの説明を示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="efd05-226">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

<span data-ttu-id="efd05-227">このタグを使用して、型に関する追加情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="efd05-228">(使用`<summary>`([`<summary>`](documentation-comments.md#summary))、型自体と、型のメンバーを記述します)。</span><span class="sxs-lookup"><span data-stu-id="efd05-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="efd05-229">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-229">__Syntax:__</span></span>

```xml
<remark>description</remark>
```

<span data-ttu-id="efd05-230">場所`description`注釈のテキストです。</span><span class="sxs-lookup"><span data-stu-id="efd05-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="efd05-231">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-231">__Example:__</span></span>

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

<span data-ttu-id="efd05-232">このタグを使用して、メソッドの戻り値を説明します。</span><span class="sxs-lookup"><span data-stu-id="efd05-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="efd05-233">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="efd05-234">場所`description`は戻り値の説明です。</span><span class="sxs-lookup"><span data-stu-id="efd05-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="efd05-235">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="efd05-236">このタグは、テキスト内で指定するためのリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="efd05-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="efd05-237">使用`<seealso>`([`<seealso>`](documentation-comments.md#seealso)) では、「参照」セクションに表示されるテキストを示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="efd05-238">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="efd05-239">場所`member`メンバーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-239">where `member` is the name of a member.</span></span> <span data-ttu-id="efd05-240">ドキュメントの生成は、指定されたコード要素が存在し、変更を確認します。*メンバー* 、生成されたドキュメント ファイル内の要素名にします。</span><span class="sxs-lookup"><span data-stu-id="efd05-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="efd05-241">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-241">__Example:__</span></span>

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

<span data-ttu-id="efd05-242">このタグは、「参照」セクションを生成するエントリをを使用します。</span><span class="sxs-lookup"><span data-stu-id="efd05-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="efd05-243">使用`<see>`([`<see>`](documentation-comments.md#see)) からテキスト内のリンクを指定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="efd05-244">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="efd05-245">場所`member`メンバーの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-245">where `member` is the name of a member.</span></span> <span data-ttu-id="efd05-246">ドキュメントの生成は、指定されたコード要素が存在し、変更を確認します。*メンバー* 、生成されたドキュメント ファイル内の要素名にします。</span><span class="sxs-lookup"><span data-stu-id="efd05-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="efd05-247">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-247">__Example:__</span></span>

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

このタグは、型または型のメンバーの説明に使用できます。 <span data-ttu-id="efd05-249">使用`<remark>`([`<remark>`](documentation-comments.md#remark))、型自体を記述します。</span><span class="sxs-lookup"><span data-stu-id="efd05-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) to describe the type itself.</span></span>

<span data-ttu-id="efd05-250">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="efd05-251">場所`description`型またはメンバーの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="efd05-252">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="efd05-253">このタグは、説明を取得するプロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="efd05-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="efd05-254">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="efd05-255">場所`property description`プロパティの説明を示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="efd05-256">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="efd05-257">このタグを使用して、クラス、構造体、インターフェイス、デリゲート、またはメソッドのジェネリック型パラメーターを説明します。</span><span class="sxs-lookup"><span data-stu-id="efd05-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="efd05-258">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="efd05-259">場所`name`、型パラメーターの名前を指定し、`description`はその説明です。</span><span class="sxs-lookup"><span data-stu-id="efd05-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="efd05-260">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="efd05-261">このタグを使用して、単語が型パラメーターであることを示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="efd05-262">ドキュメント ファイルを処理することで、何らかの方法でこの型パラメーターの書式を設定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="efd05-263">__構文:__</span><span class="sxs-lookup"><span data-stu-id="efd05-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="efd05-264">場所`name`型パラメーターの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="efd05-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="efd05-265">__例:__</span><span class="sxs-lookup"><span data-stu-id="efd05-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="efd05-266">ドキュメント ファイルの処理</span><span class="sxs-lookup"><span data-stu-id="efd05-266">Processing the documentation file</span></span>

<span data-ttu-id="efd05-267">ドキュメント ジェネレーターは、ドキュメントのコメントとタグ付けされているソース コード内の各要素の ID 文字列を生成します。</span><span class="sxs-lookup"><span data-stu-id="efd05-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="efd05-268">この ID 文字列は、ソース要素を一意に識別します。</span><span class="sxs-lookup"><span data-stu-id="efd05-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="efd05-269">ドキュメント ビューアーでは、ドキュメントを適用するのに、対応するメタデータ/リフレクション項目を識別するために、ID 文字列を使用できます。</span><span class="sxs-lookup"><span data-stu-id="efd05-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="efd05-270">ドキュメント ファイルは、ソース コードの階層表現ではありません。代わりに、各要素に対して生成される ID 文字列をフラットな一覧を示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="efd05-271">ID 文字列の形式</span><span class="sxs-lookup"><span data-stu-id="efd05-271">ID string format</span></span>

<span data-ttu-id="efd05-272">ドキュメント ジェネレーターは、ID 文字列を生成するときに、次の規則を監視します。</span><span class="sxs-lookup"><span data-stu-id="efd05-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="efd05-273">文字列に空白は配置されません。</span><span class="sxs-lookup"><span data-stu-id="efd05-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="efd05-274">文字列の最初の部分では、単一の文字の後にコロンを使用して、記述されているメンバーの種類を識別します。</span><span class="sxs-lookup"><span data-stu-id="efd05-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="efd05-275">次の種類のメンバーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="efd05-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="efd05-276">__文字__</span><span class="sxs-lookup"><span data-stu-id="efd05-276">__Character__</span></span> | <span data-ttu-id="efd05-277">__説明__</span><span class="sxs-lookup"><span data-stu-id="efd05-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="efd05-278">E</span><span class="sxs-lookup"><span data-stu-id="efd05-278">E</span></span>             | <span data-ttu-id="efd05-279">event</span><span class="sxs-lookup"><span data-stu-id="efd05-279">Event</span></span>                                                       |
   | <span data-ttu-id="efd05-280">F</span><span class="sxs-lookup"><span data-stu-id="efd05-280">F</span></span>             | <span data-ttu-id="efd05-281">フィールド</span><span class="sxs-lookup"><span data-stu-id="efd05-281">Field</span></span>                                                       |
   | <span data-ttu-id="efd05-282">M</span><span class="sxs-lookup"><span data-stu-id="efd05-282">M</span></span>             | <span data-ttu-id="efd05-283">メソッド (コンス トラクター、デストラクター、および演算子を含む)</span><span class="sxs-lookup"><span data-stu-id="efd05-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="efd05-284">N</span><span class="sxs-lookup"><span data-stu-id="efd05-284">N</span></span>             | <span data-ttu-id="efd05-285">名前空間</span><span class="sxs-lookup"><span data-stu-id="efd05-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="efd05-286">P</span><span class="sxs-lookup"><span data-stu-id="efd05-286">P</span></span>             | <span data-ttu-id="efd05-287">プロパティ (インデクサーを含む)</span><span class="sxs-lookup"><span data-stu-id="efd05-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="efd05-288">T</span><span class="sxs-lookup"><span data-stu-id="efd05-288">T</span></span>             | <span data-ttu-id="efd05-289">(クラス、デリゲート、列挙型、インターフェイス、構造体など) を入力します。</span><span class="sxs-lookup"><span data-stu-id="efd05-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="efd05-290">!</span><span class="sxs-lookup"><span data-stu-id="efd05-290">!</span></span>             | <span data-ttu-id="efd05-291">エラーの文字列。文字列の残りの部分は、エラーに関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="efd05-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="efd05-292">たとえば、ドキュメントの生成には、解決できないリンクのエラー情報が生成されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="efd05-293">文字列の 2 番目の部分は、名前空間のルートから始まり、要素の完全修飾名です。</span><span class="sxs-lookup"><span data-stu-id="efd05-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="efd05-294">要素、それを囲む型、および名前空間の名前は、ピリオドで区切られます。</span><span class="sxs-lookup"><span data-stu-id="efd05-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="efd05-295">項目自体の名前にピリオドがある場合は、置き換えられる`#(U+0023)`文字。</span><span class="sxs-lookup"><span data-stu-id="efd05-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="efd05-296">(これと見なされます要素の名前にはこの文字がありません。)</span><span class="sxs-lookup"><span data-stu-id="efd05-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="efd05-297">メソッドとプロパティの引数は、引数には、次のように、かっこで囲まれているが一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="efd05-298">引数を指定せずに、かっこを省略します。</span><span class="sxs-lookup"><span data-stu-id="efd05-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="efd05-299">引数はコンマで区切られます。</span><span class="sxs-lookup"><span data-stu-id="efd05-299">The arguments are separated by commas.</span></span> <span data-ttu-id="efd05-300">各引数のエンコーディング CLI シグネチャと同じとおりです。</span><span class="sxs-lookup"><span data-stu-id="efd05-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="efd05-301">引数は、次のように変更された、完全修飾名に基づくドキュメント名で表されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="efd05-302">ジェネリック型を表す引数が末尾にある`` ` ``(アクサン グラーブ) 文字が続く型パラメーターの数</span><span class="sxs-lookup"><span data-stu-id="efd05-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="efd05-303">引数、`out`または`ref`修飾子が、`@`に従って、型名。</span><span class="sxs-lookup"><span data-stu-id="efd05-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="efd05-304">値渡しまたは経由で渡される引数`params`特殊な表記はあるありません。</span><span class="sxs-lookup"><span data-stu-id="efd05-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="efd05-305">引数には、配列として表されます`[lowerbound:size, ... , lowerbound:size]`コンマの数は、1 を引いたランクと下限と各次元のサイズがわかっている場合が 10 進数で表されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="efd05-306">下限またはサイズが指定されていない場合は省略されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="efd05-307">下限の境界と、特定のディメンションのサイズを省略した場合、`:`も省略されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="efd05-308">ジャグ配列は 1 つで表されます`[]`レベルごと。</span><span class="sxs-lookup"><span data-stu-id="efd05-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="efd05-309">使用してポインターの型 void 以外の引数が表される、`*`次の型名。</span><span class="sxs-lookup"><span data-stu-id="efd05-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="efd05-310">Void ポインターは、の型名を使用して表される`System.Void`します。</span><span class="sxs-lookup"><span data-stu-id="efd05-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="efd05-311">型で定義されたジェネリック型パラメーターを参照する引数を使用してエンコード、 `` ` `` (アクサン グラーブ) 文字の後に、型パラメーターの 0 から始まるインデックス。</span><span class="sxs-lookup"><span data-stu-id="efd05-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="efd05-312">メソッドで定義されているジェネリック型パラメーターを使用する引数を使用して、double バックティック``` `` ```の代わりに、`` ` ``の種類で使用します。</span><span class="sxs-lookup"><span data-stu-id="efd05-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="efd05-313">構築されたジェネリック型を参照する引数は、後に、ジェネリック型を使用してエンコードする`{`、型引数のコンマ区切りのリストが続く、続けて`}`します。</span><span class="sxs-lookup"><span data-stu-id="efd05-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="efd05-314">ID 文字列の例</span><span class="sxs-lookup"><span data-stu-id="efd05-314">ID string examples</span></span>

<span data-ttu-id="efd05-315">次の例については、C# コード、ドキュメントのコメントを持つことのできる各ソース要素から生成される ID 文字列とのフラグメントを示します。</span><span class="sxs-lookup"><span data-stu-id="efd05-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="efd05-316">型では、汎用的な情報で補完された、完全修飾名を使用して表されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="efd05-317">フィールドは、完全修飾名で表されます。</span><span class="sxs-lookup"><span data-stu-id="efd05-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="efd05-318">コンストラクター。</span><span class="sxs-lookup"><span data-stu-id="efd05-318">Constructors.</span></span>

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

*  <span data-ttu-id="efd05-319">デストラクターです。</span><span class="sxs-lookup"><span data-stu-id="efd05-319">Destructors.</span></span>

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

*  <span data-ttu-id="efd05-320">メソッド。</span><span class="sxs-lookup"><span data-stu-id="efd05-320">Methods.</span></span>

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

*  <span data-ttu-id="efd05-321">プロパティとインデクサーです。</span><span class="sxs-lookup"><span data-stu-id="efd05-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="efd05-322">イベント。</span><span class="sxs-lookup"><span data-stu-id="efd05-322">Events.</span></span>

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

*  <span data-ttu-id="efd05-323">単項演算子。</span><span class="sxs-lookup"><span data-stu-id="efd05-323">Unary operators.</span></span>

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

   <span data-ttu-id="efd05-324">使用される単項演算子の関数名の完全なセットのとおりです: `op_UnaryPlus`、 `op_UnaryNegation`、 `op_LogicalNot`、 `op_OnesComplement`、 `op_Increment`、 `op_Decrement`、 `op_True`、および`op_False`します。</span><span class="sxs-lookup"><span data-stu-id="efd05-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="efd05-325">二項演算子。</span><span class="sxs-lookup"><span data-stu-id="efd05-325">Binary operators.</span></span>

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

   <span data-ttu-id="efd05-326">使用される二項演算子関数名の完全なセットのとおりです: `op_Addition`、 `op_Subtraction`、 `op_Multiply`、 `op_Division`、 `op_Modulus`、 `op_BitwiseAnd`、 `op_BitwiseOr`、 `op_ExclusiveOr`、 `op_LeftShift`、 `op_RightShift`、`op_Equality`、 `op_Inequality`、 `op_LessThan`、 `op_LessThanOrEqual`、 `op_GreaterThan`、および`op_GreaterThanOrEqual`します。</span><span class="sxs-lookup"><span data-stu-id="efd05-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="efd05-327">変換演算子は、末尾がある"`~`"後に、戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="efd05-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="efd05-328">使用例</span><span class="sxs-lookup"><span data-stu-id="efd05-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="efd05-329">C# ソース コード</span><span class="sxs-lookup"><span data-stu-id="efd05-329">C# source code</span></span>

<span data-ttu-id="efd05-330">次の例のソース コードを示しています、`Point`クラス。</span><span class="sxs-lookup"><span data-stu-id="efd05-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="efd05-331">結果の XML</span><span class="sxs-lookup"><span data-stu-id="efd05-331">Resulting XML</span></span>

<span data-ttu-id="efd05-332">クラスのソース コードが指定されると、1 つのドキュメント ジェネレーターによって生成される出力を次に示します`Point`、前述のようにします。</span><span class="sxs-lookup"><span data-stu-id="efd05-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
