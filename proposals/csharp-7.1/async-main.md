---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483752"
---
# <a name="async-main"></a>Async Main

* [x] が提案されています
* [] プロトタイプ
* [] の実装
* [] 仕様

## <a name="summary"></a>まとめ
[summary]: #summary

Entrypoint が `Task` / `Task<int>` を返し、`async`としてマークできるようにすることで、アプリケーションの Main/entrypoint メソッドで `await` を使用できるようにします。

## <a name="motivation"></a>目的
[motivation]: #motivation

これはC#、コンソールベースのユーティリティを記述する場合や、メインから `async` メソッドを呼び出して `await` するために小さなテストアプリを作成する場合によく見られます。  今日では、このような `await`を強制的に別の非同期メソッドで実行するように強制することで、複雑さのレベルを追加しています。そのため、開発者は、作業を開始するために次のような定型を作成する必要があります。

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

この定型句の必要性を排除して、`await`を使用できるように、メイン自体を `async` できるようにするだけで簡単に開始できるようになります。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

現在、次のシグネチャは entrypoints が許可されています。

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

次のように、許可されている entrypoints の一覧を拡張します。

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

互換性のリスクを回避するために、これらの新しい署名は、前のセットのオーバーロードが存在しない場合にのみ有効な entrypoints と見なされます。
言語/コンパイラでは、entrypoint を `async`としてマークする必要はありませんが、ほとんどの用途はそのようにマークされることが予想されます。

これらのいずれかがエントリポイントとして識別されると、コンパイラは、次のコード化されたメソッドのいずれかを呼び出す実際の entrypoint メソッドを合成します。
- ```static Task Main()``` によって、コンパイラはと同等のものを出力し ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task Main(string[])``` によって、コンパイラはと同等のものを出力し ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```
- ```static Task<int> Main()``` によって、コンパイラはと同等のものを出力し ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task<int> Main(string[])``` によって、コンパイラはと同等のものを出力し ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```

使用例:

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

主な欠点は、単に追加のエントリポイントシグネチャをサポートする複雑さです。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

考慮されたその他のバリアント:

`async void`を許可します。  このセマンティクスは、直接呼び出すコードでも同じにしておく必要があります。これにより、生成された entrypoint が呼び出しを行うのが困難になります (タスクは返されません)。  この問題を解決するには、次の2つのメソッドを生成します。

```csharp
public static async void Main()
{
   ... // await code
}
```

→

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

`async void`の使用を促進することにも懸念事項があります。

名前として "Main" ではなく "MainAsync" を使用します。  タスクを返すメソッドには非同期サフィックスを使用することをお勧めしますが、主にライブラリ機能に関するものであり、Main はサポートされておらず、"Main" の後に追加のエントリポイント名をサポートすることは価値がありません。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

該当なし

## <a name="design-meetings"></a>会議のデザイン

該当なし
