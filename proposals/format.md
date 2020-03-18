---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "79483938"
---
# <a name="efficient-params-and-string-formatting"></a>効率的な Params と文字列の書式設定

## <a name="summary"></a>まとめ
この機能の組み合わせにより、`string` の値を書式設定したり `params` スタイル引数を渡したりする効率が向上します。

## <a name="motivation"></a>目的
`string` 値の書式設定による割り当てのオーバーヘッドによって、テキストベースのアプリケーションのパフォーマンスが低下することがあります。たとえば、`struct` 型のボックス化の代償、`params` の `object[]` 割り当て、`string` 呼び出し中の中間 `string.Format` 割り当てなどです。 このようなアプリケーションの効率を維持するために、多くの場合、`params` や `string` の補間などの生産性機能を破棄し、標準ではないコード化されたソリューションに移行する必要があります。 

例として MSBuild を考えてみましょう。 これは、パフォーマンスを意識する開発C#者によって、多数の最新機能を使用して作成されます。 ただし、1つの代表的なビルドサンプルでは、最小の詳細度を使用して、26 MB の `string` 割り当てが生成されます。 この1/2 の割り当ては、`string.Format`内の短時間の割り当てです。 これらの機能により、.NET デスクトップでの多くが削除され、.NET Core では、が利用可能になったため、ほぼゼロになります `Span<T>`

ここで説明されている言語機能のセットを使用すると、アプリケーションコードベースを大幅に変更することなく、これらの機能を使用し続けることができ、ほとんどの場合に意図しない割り当てオーバーヘッドが解消されます。

## <a name="detailed-design"></a>詳細なデザイン 
ここでは、次のような結果を得るために使用される一連の機能があります。

- `params` を展開して、より広範なコレクション型のセットをサポートします。
- 開発者が `string` 補間を実現する方法をカスタマイズできるようにします。 
- `string` を補間することで、より効率的な `string.Format` オーバーロードにバインドできます。

### <a name="extending-params"></a>パラメーターの拡張
この言語では、メソッドシグネチャ内の `params` が `Span<T>`、`ReadOnlySpan<T>`、および `IEnumerable<T>`型を持つことができます。 `params T[]`に適用されるこれらの新しい型には、呼び出しの場合と同じ規則が適用されます。

- 唯一の違いが `params` キーワードの場合は、オーバーロードできません。
- は `T` に暗黙的に変換できる一連の引数を渡すことによってを呼び出すことができます。また、1つの `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` 引数に渡すこともできます。
- は、メソッドシグネチャの最後のパラメーターである必要があります。
- その他... 

`Span<T>` と `ReadOnlySpan<T>` のバリアントは、わかりやすくするために以下の `Span<T>` と呼ばれています。 `ReadOnlySpan<T>` の動作が異なる場合は、明示的に呼び出されます。 

`params` の `Span<T>` バリアントの利点は、`Span<T>` 値のバッキングストレージを割り当てる方法がコンパイラによって非常に柔軟になることです。 `params T[]` を使用すると、コンパイラは `params` メソッドを呼び出すたびに新しい `T[]` を割り当てる必要があります。 再使用はできません。これは、呼び出し先が格納され、パラメーターを再利用する必要があるためです。 これにより、多数の `params` 呼び出しがあるメソッドで、大きな非効率性が発生する可能性があります。

指定された `Span<T>` バリアントは `ref struct` 呼び出し先が引数を格納できません。 そのため、コンパイラは、引数の再利用などのアクションを実行して、呼び出しサイトを最適化できます。 これにより、`T[]`の場合と比べて、呼び出しが非常に効率的になります。 ただし、この言語では、このような callsite がどのように最適化されるかについて、特定の保証はありません。 `params Span<T>` メソッドを呼び出すときに、コンパイラが `T[]` 以外の値を自由に使用できることに注意してください。 

このような考えられる実装の1つは次のとおりです。 メソッド本体ですべての `params` 呼び出しを検討します。 コンパイラは、最大の `params` 呼び出しと同じサイズの配列を割り当てることができ、配列上で適切にサイズ設定された `Span<T>` インスタンスを作成することによって、すべての呼び出しでそれを使用します。 次に例を示します。

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

コンパイラは、次のように `Go` の本体を出力するように選択できます。

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

これにより、アプリケーションで割り当てられた配列の数を大幅に減らすことができます。 ランタイムが配列のよりスマートなスタック割り当てを行うためのユーティリティを提供する場合、割り当てをさらに小さくすることができます。

ただし、この最適化は常に適用できません。 呼び出し先は `params` 引数をキャプチャできませんが、`ref` またはそれ自体が `ref struct` 型である `out / ref` パラメーターがある場合は、呼び出し元でキャプチャできます。 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

ただし、これらのケースは静的に検出できます。 `ref` が返された場合、または `out` または `ref`によって渡された `ref struct` パラメーターがある場合に発生する可能性があります。 このような場合、コンパイラは、すべての呼び出しに対して新しい `T[]` を割り当てる必要があります。 

このドキュメントの最後では、いくつかの潜在的な最適化戦略について説明します。

`IEnumerable<T>` バリアントは、単に便利なオーバーロードです。 `IEnumerable<T>` が頻繁に使用されていても `params` 使用量が多いシナリオでは便利です。 `T` argument 形式で呼び出されると、現在の `params T[]` の場合と同様に、バッキングストレージが `T[]` として割り当てられます。

### <a name="params-overload-resolution-changes"></a>params のオーバーロードの解決の変更
この提案では、言語に4つのバリアント `params` があることを意味します。 メソッドでは、`params` 宣言の型のみが異なるメソッドのオーバーロードを定義するのが賢明です。 

`StringBuilder.AppendFormat` によって、`params object[]`に加えて `params ReadOnlySpan<object>` オーバーロードが確実に追加されることに注意してください。 これにより、呼び出し元のコードを変更しなくてもコレクションの割り当てを減らすことで、パフォーマンスを大幅に向上させることができます。 

これを容易にするために、この言語では、次のオーバーロードの解決規則に違反する規則が導入されています。 候補のメソッドが `params` パラメーターのみで異なる場合、候補は次の順序で優先されます。

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

この順序は、一般的なケースで最も効率的ではありません。

### <a name="variant"></a>Variant
CoreFX は、[バリアント](https://github.com/dotnet/corefxlab/pull/2595)という名前の新しいマネージ型のプロトタイプを作成しています。 この型は、異種値を必要とするものの、パラメーターとして `object` を使用することによってオーバーヘッドを発生させたくない Api で使用することを意図しています。 `Variant` 型はユニバーサルストレージを提供しますが、最も一般的に使用される型に対してボックス化割り当てを回避します。 この型を `string.Format` のような Api で使用すると、ほとんどの場合にボックス化オーバーヘッドがなくなります。

この型自体は、必ずしも言語に固有のものではありません。 このドキュメントでは、提案の他の部分の実装について詳しく説明します。 

### <a name="efficient-interpolated-strings"></a>効率的な挿入文字列
補間文字列は、でC#はまだ非効率的な機能です。 `string`として補間 `string` を使用する最も一般的な構文は `string.Format(string, params object[])` 呼び出しに変換されます。 これによって、すべての値型に対するボックス化割り当てが発生します。また、実装では、`string.Format`の "高速" オーバーロードで引数の数がパラメーターの数を超えた場合に、配列の割り当てに加えて、書式設定に `object.ToString` を使用することが多いため、中間 `string` 割り当てが発生します。 

言語によって、`string.Format`の代替オーバーロードが考慮されるように補間が変化します。 `string.Format(string, params)` のすべての形式を考慮し、引数の型を満たす "最適" のオーバーロードを選択します。
"Best" `params` のオーバーロードは、前述の規則によって決定されます。 これは、補間 `string` が `string.Format(string format, params ReadOnlySpan<Variant> args)`などの非常に効率的なオーバーロードにバインドできるようになったことを意味します。 多くの場合、これによりすべての中間割り当てが削除されます。

### <a name="customizable-interpolated-strings"></a>カスタマイズ可能な挿入文字列
開発者は、`FormattableString`を使用して、挿入文字列の動作をカスタマイズできます。 これには、挿入文字列に変換されるデータが含まれます。形式 `string` と引数を配列として指定します。 ただし、この場合も、ボックス化と引数の配列割り当てと `FormattableString` (`abstract class`) の割り当てがあります。 したがって、`string` の書式設定に大きな割り当てを行うアプリケーションにはほとんど使用しません。

挿入文字列の書式設定を効率的にするために、言語は `System.ValueFormattableString`新しい型を認識します。 すべての補間文字列は、この型への変換対象の型に変換されます。 これは、現在 `FormattableString.Create` の場合とまったく同じように、挿入文字列を呼び出し `ValueFormattableString.Create` に変換することによって実装されます。 言語は、このドキュメントで説明されているすべての `params` オプションをサポートしており、最適な `ValueFormattableString.Create` 方法を探しています。 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

オーバーロードの解決規則は、引数が挿入文字列の場合に `string` より `ValueFormattableString` を優先するように変更されます。 つまり、`string` と `ValueFormattableString`のみが異なるオーバーロードを使用することが重要になります。 現時点では、`FormattableString` によるオーバーロードは、コンパイラが常に (開発者が明示的なキャストを使用しない限り) `string` バージョンを優先するため、価値がありません。 

## <a name="open-issues"></a>懸案事項を開く

### <a name="valueformattablestring-breaking-change"></a>ValueFormattableString の重大な変更
`string` に対するオーバーロードの解決中に `ValueFormattableString` 優先される変更は、互換性に影響する変更です。 開発者は `ValueFormattableString` 今日と呼ばれる型を定義し、`string`でメソッドのオーバーロードで使用することができます。 この提案された変更により、この一連の機能が実装された後、コンパイラは異なるオーバーロードを選択します。 

このような可能性は適度に低いと思われます。 型には完全な名前 `System.ValueFormattableString` が必要であり、`Create`という名前の `static` メソッドが必要になります。 開発者が `System` 名前空間の型を定義しないことを強くお勧めしないので、このブレークは妥当な妥協のように見えます。

### <a name="expanding-to-more-types"></a>その他の型への展開
ここでは、`params` がサポートされている一連のコレクションに `IList<T>`、`ICollection<T>` および `IReadOnlyList<T>` を追加することを検討してください。 実装に関しては、ここでは他の作業について若干のコストがかかります。

LDM では、言語の複雑さを考慮する必要があるかどうかを判断する必要があります。 `IEnumerable<T>` を追加すると、非常に具体的な摩擦ポイントが削除されます。 この `params` ソリューションがない場合、多くのお客様は `params` メソッドを呼び出すときに `IEnumerable<T>` から `T[]` を割り当てようとしました。 ただし、`IEnumerable<T>` を追加することによって修正されます。 ここでは、他のインターフェイスが修正する特定の摩擦点はありません。 

## <a name="considerations"></a>考慮事項

### <a name="variant2-and-variant3"></a>Variant2 と Variant3
CoreFX チームは、最大3つの `Variant` 引数に対して、割り当てられていないストレージ型のセットも持っています。 これらは、1つの `Variant`、`Variant2` と `Variant3`です。 すべてには、`CreateSpan` と `KeepAlive`の割り当てを自由に `Span<Variant>` するためのメソッドのペアがあります。 これは、最大3つの引数の `params Span<Variant>` に対して、呼び出しサイトが完全に解放される可能性があることを意味します。

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

`Go` メソッドは、次のように下げることができます。

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

これには、`params Span<T>` の呼び出し間で `T[]` を再利用するために、提案の上で非常に少ない作業が必要になります。 コンパイラは、呼び出しごとに一時的なを管理する必要があります。また、の後で作業をクリーンアップする必要があります (1 つのケースの場合でも、内部一時を free としてマークするだけです)。 

注: `KeepAlive` 関数は、デスクトップでのみ必要です。 .NET Core では、メソッドは使用できないため、コンパイラは呼び出しを生成しません。

### <a name="clr-stack-allocation-helpers"></a>CLR スタック割り当てヘルパー
CLR は、連続したメモリのスタック割り当てに対してのみ[localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2)を提供します。 この命令は `unmanaged` 型に対してのみ機能することに制限されています。 これは、`params 
 Span<T>`のバッキングストレージを効率的に割り当てるためのユニバーサルソリューションとして使用できないことを意味します。 

ただし、この制限は基本的な制限事項ではありませんが、履歴の成果物が多くなります。 CLR は、ユニバーサルスタック割り当てを提供する新しい op コード/組み込みを追加することができます。 その後、これらを使用して、ほとんどの `params Span<T>` 呼び出しのバッキングストレージを割り当てることができます。

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

`Go` メソッドは、次のように下げることができます。

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

このアプローチはヒープ効率が非常に高いのに対し、余分なスタック使用が発生します。 ディープスタックと `params` の使用量が多いアルゴリズムでは、単純な `T[]` 割り当てが成功すると、`StackOverflowException` が生成される可能性があります。 

残念C#ながら、メソッド間分析の種類については設定されていませんが、呼び出しでは `params`のスタックまたはヒープ割り当てを使用するかどうかを教育的に判断できるようになっています。 実際には、それぞれの呼び出しを独自に検討するだけで済みます。

CLR は、実行時にこの種類の決定を行うのに最適な設定です。 このため、ランタイムには、ユニバーサルスタック割り当てに対して2つのメソッドが用意されている可能性があります。

1. `Span<T> StackAlloc<T>(int length)`: これは、`localloc` の動作および制限と同じですが、`T`の型で動作することはできません。 
1. `Span<T> MaybeStackAlloc<T>(int length)`: このランタイムは、スタックまたはヒープの割り当てを行うことによって、これを実装することを選択できます。 ランタイムは、呼び出し元の実行コンテキストを使用して、`Span<T>` の割り当て方法を決定できます。 ただし、呼び出し元は、スタック割り当てされているかのように、常にそれを処理します。

1 ~ 2 個の引数など、非常に単純なC#ケースでは、コンパイラは常に `StackAlloc<T>` variant を使用できます。 ほとんどの場合、これはスタック枯渇に多大な影響を与えることはほとんどありません。 その他のケースでは、コンパイラは代わりに `MaybeStackAlloc<T>` を使用し、ランタイムが呼び出しを行うようにすることもできます。

どのように選択するかは、実際のアプリをより深く調査し、調査することが必要になる可能性があります。 ただし、これらの新しい組み込みを利用できる場合は、このような柔軟性が得られます。

### <a name="why-not-varargs"></a>Varargs でないのはなぜですか。 
既存の[varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017)機能は、考えられる解決策として検討されました。 ただし、この機能は主にC++/cli のシナリオに使用され、他のシナリオでは既知のホールを持ちます。 さらに、Unix への移植にも大きなコストがかかります。 そのため、これは実用的な解決策としては見えませんでした。

## <a name="related-issues"></a>関連する問題
この仕様は、次の問題に関連しています。 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

