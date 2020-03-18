---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483974"
---
# <a name="pattern-based-using-and-using-declarations"></a>"パターンベースの使用" と "宣言の使用"

## <a name="summary"></a>まとめ

この言語では、リソース管理をより簡単にするために、`using` ステートメントの周囲に2つの新機能が追加されています。 `using` は `IDisposable` に加えて破棄可能なパターンを認識し、言語に `using` 宣言を追加します。

## <a name="motivation"></a>目的

`using` ステートメントは、今日のリソース管理のための効果的なツールですが、かなりの手続きが必要です。 多数のリソースを管理するメソッドでは、一連の `using` ステートメントを使用して、構文的に行き詰まるを取得できます。 この構文の負担は、ほとんどのコーディングスタイルのガイドラインで、このシナリオで中かっこを囲む例外が明示的に設定されているためです。 

`using` 宣言では、この方法の多くを削除C#し、リソース管理ブロックを含む他の言語と同等のものを取得します。 さらに、パターンベースの `using` を使用すると、開発者はここで使用できる型のセットを展開できます。 多くの場合、`using` のステートメントで値を使用できるようにするためにのみ存在するラッパー型を作成する必要はありません。 

これらの機能を組み合わせることにより、開発者は `using` を適用できるシナリオを簡略化および拡張できます。

## <a name="detailed-design"></a>詳細なデザイン 

### <a name="using-declaration"></a>using 宣言

この言語では、`using` をローカル変数宣言に追加できます。 このような宣言は、同じ場所にある `using` ステートメントで変数を宣言するのと同じ効果があります。

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

`using` ローカルの有効期間は、宣言されているスコープの末尾まで拡張されます。 `using` ローカル変数は、宣言された順序と逆の順序で破棄されます。 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

`goto`、または `using` 宣言の表面でのその他の制御フローコンストラクトに関する制限はありません。 代わりに、コードは同等の `using` ステートメントの場合と同様に動作します。

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

`using` ローカル宣言で宣言されたローカルは、暗黙的に読み取り専用になります。 これは、`using` ステートメントで宣言されたローカルの動作と一致します。 

`using` 宣言の言語文法は次のようになります。

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

`using` 宣言に関する制限事項は次のとおりです。

- `case` ラベル内に直接指定することはできませんが、代わりに `case` ラベル内のブロック内にある必要があります。
- `out` 変数宣言の一部として表示されない場合があります。 
- 各宣言子の初期化子が必要です。
- ローカル型は `IDisposable` に暗黙的に変換できるか、または `using` パターンを満たす必要があります。

### <a name="pattern-based-using"></a>パターンベースの使用

この言語は、アクセス可能な `Dispose` インスタンスメソッドを持つ型である、破棄可能なパターンの概念を追加します。 破棄可能なパターンに適合する型は、`IDisposable`を実装する必要がない `using` のステートメントまたは宣言に含めることができます。 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

これにより、開発者はいくつかの新しいシナリオで `using` を活用できます。

- `ref struct`: これらの型は現在、インターフェイスを実装することができないため、`using` ステートメントに参加することはできません。
- 拡張メソッドを使用すると、開発者は、`using` ステートメントに参加する他のアセンブリの型を拡張できます。

型を暗黙的に `IDisposable` に変換して、破棄可能なパターンに適合させることができる場合、`IDisposable` が優先されます。 これは `foreach` の反対のアプローチ (インターフェイスよりも優先されるパターン) を使用しますが、下位互換性を確保するために必要です。

従来の `using` ステートメントと同じ制限がここでも適用されます。 `using` で宣言されたローカル変数は読み取り専用で、`null` の値によって例外がスローされることはありません。などです。コード生成が異なるのは、Dispose を呼び出す前に `IDisposable` するキャストがないということだけです。

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

破棄可能なパターンに合わせるには、`Dispose` メソッドにアクセスできる必要があります。また、パラメーター化されていない `void` 戻り値の型を持つ必要があります。 その他の制限はありません。 これは、明示的に拡張メソッドを使用できることを意味します。

## <a name="considerations"></a>考慮事項

### <a name="case-labels-without-blocks"></a>ブロックのない case ラベル

実際の有効期間に関係があるため、`using declaration` は `case` ラベル内では直接無効です。 考えられる解決策の1つは、同じ場所の `out var` と同じ有効期間を与えることです。 これは、機能の実装について複雑さが増し、回避策 (`case` ラベルにブロックを追加するだけで済みます) がこのルートの作成には合わないと見なされました。

## <a name="future-expansions"></a>将来の展開

### <a name="fixed-locals"></a>固定ローカル

`fixed` ステートメントには、`using` ローカルを持つことを実現する `using` ステートメントのすべてのプロパティがあります。 この機能を拡張してローカル `fixed` する場合も考慮する必要があります。 有効期間と順序の規則は、ここで `using` と `fixed` にも同様に適用する必要があります。
