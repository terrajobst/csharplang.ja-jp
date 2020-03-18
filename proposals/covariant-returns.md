---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483458"
---
# <a name="covariant-return-types"></a>共変の戻り値の型

* [x] が提案されています
* [] プロトタイプ: 開始されていません
* [] の実装: 開始されていません
* [] 仕様: 開始されていません

## <a name="summary"></a>まとめ
[summary]: #summary

_共変の戻り値の型_をサポートします。 具体的には、オーバーライドするメソッドに、オーバーライドするメソッドよりも派生型の方が多いことを許可します。

## <a name="motivation"></a>目的
[motivation]: #motivation

これは、オーバーライドされたメソッドと同じ型を返す必要がある言語の制約を回避するために、さまざまなメソッド名を開発する必要がある、コード内の一般的なパターンです。 Roslyn コードベースの例については、以下を参照してください。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

_共変の戻り値の型_をサポートします。 具体的には、オーバーライドするメソッドに、オーバーライドするメソッドよりも派生型の方が多いことを許可します。 これは、メソッドおよびプロパティに適用され、クラスおよびインターフェイスでサポートされます。

これは、ファクトリパターンで役に立ちます。 たとえば、Roslyn コードベースでは、次のようになります。

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

この実装は、オーバーライドするメソッドを、基底クラスのメソッドを非表示にする "new" 仮想メソッドとしてコンパイラによって、派生クラスのメソッドの呼び出しで基本クラスのメソッドを実装する_ブリッジメソッド_として出力することになります。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

- [] すべての言語の変更は、それ自体に対して支払う必要があります。
- [] 詳細継承階層の場合でも、パフォーマンスが妥当であることを確認する必要があります。
- [] 以前のコンパイラから新しい IL を使用している場合でも、翻訳戦略の成果物が言語のセマンティクスに影響しないようにする必要があります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

ソースでは、言語ルールを少し緩和して、

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- [] この機能を使用するようにコンパイルされた Api は、以前のバージョンの言語で動作しますか。

## <a name="design-meetings"></a>会議のデザイン

まだありません。 <https://github.com/dotnet/roslyn/issues/357>にはいくつかの議論がありました。