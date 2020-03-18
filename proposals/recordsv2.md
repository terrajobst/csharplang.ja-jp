---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/09/2019
ms.locfileid: "79483926"
---

# <a name="records-v2"></a>レコード v2

これまで、データの操作を可能にする機能として、レコードについて考えました。

"データの操作" は多数のファセットを持つ大規模なグループであるため、それぞれを個別に調べることが重要です。 まず、今日のレコードの例とその欠点を見てみましょう。

たとえば、次のようにして、簡単なレコードを定義します。

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

使用量が読み取られます。

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

このコードには、次のような大きな利点があります。

1. 定義はバージョンの回復性であり、プロパティを簡単に追加または移動できます。
2. プロパティは任意の順序で設定できます。また、初期化内の名前は常にアクセサーと一致します。
3. 既定値を持つプロパティは単にスキップできます。

最初の欠点は、プロパティが変更可能である必要があることです。

# <a name="mutability"></a>可能性

ここでは、オブジェクト初期化C#子で `readonly` メンバーを設定する方法について説明します。
この初期化を念頭に置いて設計されていない型もあるため、オプトインすることもできます。

提案されたソリューションは、プロパティとフィールドに適用できる新しい修飾子 `initonly`です。

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

この codegen は非常に簡単です。読み取り専用フィールドを設定するだけです。
具体的には、下げられたプロパティは次のようになります。

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

CLR では、読み取り専用フィールドの設定は検証不可能ですが、unsafe は考慮されません。 より高度な検証をサポートするために、次の規則が提案されています。読み取り専用フィールドは、`initonly` メソッド内、または CLR スタック上の新しいオブジェクトでのみ変更でき、ストアまたはメソッドの呼び出しによって公開されていません。

これにより、`UserInfo` の例の中でも、さまざまな問題を解決する必要がありますが、複雑で不安定な出力方法は必要ありません。 ただし、不変性によって新しい問題が発生します。変更されたオブジェクトを簡単に構築できます。

# <a name="with-ing"></a>With-ing

不変性を使用してプログラミングする場合は、オブジェクトを直接変更するのではなく、変更を加えたコピーを作成することによって、オブジェクトを変更します。 残念ながら、現在のスタイルのレコードを使用しC#ていても、でこれを行うための便利な方法はありません。 以前は、この機能を実装するレコードに対して、自動生成された "With" メソッドが提供されることが提案されています。 このようなメカニズムがある場合は、`initonly` メンバーと連携することが重要です。 これを実現するために、オブジェクト初期化子に似た `with` 式を追加することを提案しています。 使用例を次に示します。

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

結果として得られる `newUserName` オブジェクトは `userInfo`のコピーであり、`Username` は "angocke" に設定されます。
`with` 式の codegen も、オブジェクト初期化子に似ています。新しいオブジェクトが構築され、`initonly` `Username` setter がメソッド本体で呼び出されます。

もちろん、ここでの違いは、構築される新しいオブジェクトが単純な新しいオブジェクト作成ではなく、元のオブジェクトと重複していることです。 この機能を提供するには、オブジェクトに、重複するオブジェクトを提供する "With constructor" が用意されている必要があります。 サンプルの `With` コンストラクターは次のようになります。

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

特に、`with` 式では、オブジェクト初期化子と同様に `initonly` メンバーが設定されるため、検証をサポートするには、`initonly` メンバーが設定される前にオブジェクトが公開されていないことを確認する必要があります。 これを適用するには、`WithConstructor` 属性 (またはそれと同等の構文) によって、メソッドに新しい規則が適用されます。すべての return ステートメントには、オブジェクト初期化子と共に、場合によってはオブジェクト作成式が直接含まれている必要があります。

`With` コンストラクターで検証が必要な場合、ユーザーはその検証を行うコンストラクターを導入することができます。たとえば、

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

`With` に関連付けられた複雑さの最後の部分は継承です。 レコードが拡張可能な場合は、サブクラスの新しい `With` を指定する必要があります。 これは、次のように実現できます。

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

ここでもう1つ複雑な点に注意してください。派生型で `With` コンストラクターをオーバーライドするには、オーバーライドで共変の戻り値の型もサポートする必要があります。
この[機能については、](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)既に別の提案があります。

**デメリット**

- `WithConstructor`s 内のすべての return ステートメントに新しいオブジェクト式を含めることは、制限がありません。
  これは、新しいオブジェクトがメソッドをエスケープしないようにするフロー分析によって軽減される可能性があります。
- コンパイラのテクニックを使用したオーバーライドの分散をサポートするには、スタブメソッドが必要です。これにより、継承の深さで quadratically が拡張されます。 スタブメソッドの必要性は、シグネチャを厳密に一致させるランタイム要件によるものです。 ランタイム要件が緩和の場合、スタブメソッドはまったく必要ありません。
- フォーム `Type(Type original)` のチェーンコンストラクターを使用すると、パターンを使用するためにそのコンストラクターを効果的に予約できます。 コンストラクターのセマンティクスは一意であるため、名前を変更することはできません。


## <a name="wrapping-it-all-up-records"></a>すべてをラップする: レコード

上記の機能を使用すると、以前は非常に難しいプログラミングのスタイルを実現できます。 しかし、新しい機能を使用する場合でも、非常に冗長になり、すべての注釈に注釈を付けることができます。 また、既に記述されている Equals や GetHashCode のようないくつかの項目もありますが、手間がかかります。
さらに、これらの新しいプリミティブの上に等価性を実装する場合の大きな欠点として、構造の等価性は、新しいデータを追加するときにデータ型によって変わることがありますが、手動で処理する場合は、これらの問題が同期されない可能性があります。

そのため、新しい機能をC#提供するのではなく、レコードの新しい構文をサポートするように提案されていますが、既定値を設定し、レコードで使用するように設計されたコードを生成することもできます。 構文の例は次のようになります。

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

このクラス用に生成されたコードは、すべてのパブリックフィールドと自動プロパティをレコードの構造メンバーとして扱うことになります。 レコードメンバーは、メンバーの追加または除外に使用できる新しい `RecordMember(bool)` 属性を使用してカスタマイズできます。 レコードメンバーは既定で `initonly` され、レコードメンバーに基づいて、クラスに対して等値が自動生成されます。 どの時点でも、これらのメンバーの動作は、ソースで宣言するだけでカスタマイズできます。 ユーザーが記述した実装は、すべてのパターン使用法で既定の実装を置き換えます。

継承の面での等価性は複雑ですが、[他のレコードの提案](records.md)では適切に解決されているように見えます。

## <a name="primary-constructors"></a>プライマリコンストラクター

前のレコードの提案には、型自体のパラメーターリストの新しい構文も含まれています。例:

```C#
class Point(int X, int Y);
```

新しい設計では、パラメーターリストは、レコードと完全C#に統合される可能性がある直交特徴です。 プライマリコンストラクターがレコードに含まれている場合、パブリックフィールドや自動プロパティと同様に、新しい既定値が設定されます。プライマリコンストラクターのパラメーターは、同じ名前のパブリックレコードメンバープロパティを生成するために使用されます。 また、プライマリコンストラクターを使用して、deconstructor を自動生成できるようになりました。

たとえば、次のレコードのプライマリコンストラクターがあるとします。

```C#
data class Point(int X, int Y);
```

はと同じです。

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

また、上記の最終的な生成結果は、

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

プライマリコンストラクターを使用するデータクラスについては、他にもいくつかの情報を取得しました。生成されたプロテクトコンストラクター内でプライマリフィールドを設定する代わりに、プライマリコンストラクターにデリゲートします。 Point クラスに別の非プライマリレコードメンバーがある場合 (例:

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

これにより、生成された保護コンストラクターが次のように変更されます。

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

特に、プライマリコンストラクターを使用したレコードの継承については、何を行うかについては説明しません。 たとえば、

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

任意の方法で解決するのではなく、より明示的な方法では、パラメーターリストを基本リストと共に指定する必要があります。たとえば、

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

基本リストのパラメーターリストは、生成されたプライマリコンストラクター内の `base` の呼び出しに適用されます。

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

1つのプライマリコンストラクターがレコードの外部にあるということは、引き続き、今後の提案のために開いています。
