---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "79484118"
---
# <a name="simple-programs"></a>単純なプログラム

* [x] が提案されています
* [x] プロトタイプ: 開始しました
* [] の実装: 開始されていません
* [] 仕様: 開始されていません

## <a name="summary"></a>まとめ
[summary]: #summary

*Compilation_unit*の*namespace_member_declaration*s の直前 (つまりソースファイル) に、*ステートメント*のシーケンスを実行できるようにします。

セマンティクスは、このような一連の*ステートメント*が存在する場合、実際の型名とメソッド名をモジュロする次の型宣言が生成されることを意味します。

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

https://github.com/dotnet/csharplang/issues/3117 も参照してください。

## <a name="motivation"></a>目的
[motivation]: #motivation

明示的な `Main` 方法が必要になるため、プログラムの中でも最も単純なものがあります。 これは、言語学習とプログラムのわかりやすさを実現するためのものです。 この機能の主な目的は、学習者C#やコードの明瞭さを避けるために、不要な定型句を使用しないプログラムを許可することです。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

### <a name="syntax"></a>構文

追加の構文では、コンパイル単位で*ステートメント*のシーケンスを許可するだけで、 *namespace_member_declaration*s の直前になります。

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

1つを除くすべての*compilation_unit* *ステートメント*s は、すべてローカル関数宣言である必要があります。 

例:

``` c#
// File 1 - any statements
if (args.Length == 0
    || !int.TryParse(args[0], out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

// File 2 - only local functions
(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a>Semantics

最上位レベルのステートメントがプログラムのコンパイル単位に存在する場合、次のように、その意味は、グローバル名前空間の `Program` クラスの `Main` メソッドのブロック本体で結合されているかのようになります。

``` c#
static class Program
{
    static async Task Main()
    {
        // File 1 statements
        // File 2 local functions
        // ...
    }
}
```

"Program" と "Main" という名前は、図の目的でのみ使用されます。コンパイラで使用される実際の名前は実装に依存しており、メソッドをソースコードから名前で参照することはできません。

メソッドは、プログラムのエントリポイントとして指定されます。 規約によって明示的に宣言されたメソッドは、エントリポイントの候補と見なされることがあります。 警告は、発生した場合に報告されます。 `-main:<type>` コンパイラスイッチを指定するとエラーになります。

1つのコンパイル単位にローカル関数宣言以外のステートメントがある場合、そのコンパイル単位のステートメントが最初に実行されます。 これにより、あるファイルのローカル関数が別のファイル内のローカル変数を参照することが有効になります。 他のコンパイル単位からのステートメントの投稿 (すべてローカル関数) の順序は定義されていません。

非同期操作は、通常の非同期エントリポイントメソッド内のステートメントで許可されるレベルまで、最上位レベルのステートメントで許可されます。 ただし、`await` 式やその他の非同期操作を省略すると、警告は生成されません。 代わりに、生成されたエントリポイントメソッドのシグネチャはと同じになります。 
``` c#
    static void Main()
```

上記の例では、次の `$Main` メソッド宣言が生成されます。

``` c#
static class $Program
{
    static void $Main()
    {
        // Statements from File 1
        if (args.Length == 0
            || !int.TryParse(args[0], out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        // Local functions from File 2
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

同時に、次のような例があります。
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

次のようになります。
``` c#
static class $Program
{
    static async Task $Main()
    {
        // Statements from File 1
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>最上位レベルのローカル変数とローカル関数のスコープ

最上位のローカル変数と関数は、生成されたエントリポイントメソッドに "ラップ" されますが、プログラム全体のスコープ内に存在している必要があります。
単純な名前の評価を目的として、グローバル名前空間に到達した場合は、次のようになります。
- 最初に、生成されたエントリポイントメソッド内で名前を評価しようとしましたが、この試行が失敗した場合にのみ、 
- グローバル名前空間宣言内の "regular" 評価が実行されます。 

これにより、グローバル名前空間内で宣言されている名前空間と型、およびインポートされた名前のシャドウに名前が付けられる可能性があります。

単純な名前の評価が最上位レベルのステートメントの外部で発生し、評価でトップレベルのローカル変数または関数が生成される場合、エラーが発生します。

この方法では、"最上位の関数" (https://github.com/dotnet/csharplang/issues/3117)でのシナリオ 2) をより適切に解決できるようになります。また、サポートされていると誤解したユーザーに役立つ診断を提供できます。

