---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484148"
---
# <a name="async-streams"></a>非同期ストリーム

* [x] が提案されています
* [x] プロトタイプ
* [] の実装
* [] 仕様

## <a name="summary"></a>まとめ
[summary]: #summary

C#では、反復子メソッドと非同期メソッドがサポートされていますが、反復子と非同期のメソッドの両方をサポートしていません。  これを修正するには、`async` 反復子の新しい形式で `await` を使用できるようにします。これにより、`IEnumerable<T>` または `IEnumerator<T>`ではなく `IAsyncEnumerable<T>` または `IAsyncEnumerator<T>` を返し、新しい `IAsyncEnumerable<T>` で使用できるようになります。`await foreach`  `IAsyncDisposable` インターフェイスは、非同期のクリーンアップを有効にするためにも使用されます。

## <a name="related-discussion"></a>関連の説明
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

## <a name="interfaces"></a>インターフェイス

### <a name="iasyncdisposable"></a>IAsyncDisposable

`IAsyncDisposable` (https://github.com/dotnet/roslyn/issues/114) など) については、よく説明されています。  ただし、非同期反復子のサポートを追加するために必要な概念です。  `finally` ブロックには `await`が含まれる場合があり、反復子の破棄の一部として `finally` ブロックを実行する必要があるため、非同期に破棄する必要があります。  また、一般に、リソースのクリーンアップでは、ファイルを閉じる (フラッシュが必要)、コールバックを解除する、登録解除が完了したことを知る方法を提供するなど、時間がかかる場合があります。

次のインターフェイスがコア .NET ライブラリに追加されます (例: System.private.corelib/System)。
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
`Dispose`と同様に、`DisposeAsync` を複数回呼び出すことは可能であり、その後の呼び出しは nops として処理され、同期的に完了したタスクを返す必要があります`DisposeAsync` (ただし、スレッドセーフである必要がありますが、同時呼び出しはサポートされていません)。  さらに、型には `IDisposable` と `IAsyncDisposable`の両方が実装されている場合がありますが、その場合は `Dispose` を呼び出して `DisposeAsync` またはその逆を行うこともできますが、それ以降のいずれかの呼び出しは nop である必要があります。  そのため、型が両方を実装している場合、コンシューマーは1回だけ呼び出すことをお勧めします。また、コンテキストに基づいて関連性の高いメソッドを1回だけ呼び出し、同期コンテキストで `Dispose` し、非同期のメソッドを `DisposeAsync` します。

(`IAsyncDisposable` が `using` とどのように対話するかについて説明しておきます。  `foreach` との相互作用については、この提案で後ほど扱います)。

検討対象の候補:
- _`CancellationToken`を受け入れる`DisposeAsync`_ : 理論的には、すべての非同期を取り消すことができますが、破棄はクリーンアップ、終了、free'ing リソースなど、通常はキャンセルすべきものではないことを意味します。取り消された作業にはクリーンアップが依然として重要です。  実際の作業が取り消される原因となった同じ `CancellationToken` は、通常 `DisposeAsync`に渡されるトークンと同じであり、作業の取り消しによって `DisposeAsync` が nop になる可能性があるため `DisposeAsync` 意味がありません。  破棄を待機しているユーザーがブロックされないようにする場合は、結果の `ValueTask`で待機したり、一定の期間だけ待機したりすることを避けることができます。
- _`Task`を返す`DisposeAsync`_ は、非ジェネリックの `ValueTask` が存在し、`IValueTaskSource`から構築できるようになったので、`ValueTask` から `DisposeAsync` を返すことにより、既存のオブジェクトを `DisposeAsync`の非同期完了を表す promise として再利用できるようになり、`Task` が非同期に完了した場合の `DisposeAsync` 割り当てが保存されます。
- _`bool continueOnCapturedContext` (`ConfigureAwait`) を使用した `DisposeAsync` の構成_: このような概念が `using`、`foreach`、およびこれを使用するその他の言語構造にどのように公開されているかに関連する問題が発生する場合がありますが、インターフェイスの観点からは、実際には何も実行されず、構成するものはありません。`ValueTask` のコンシューマーは、必要に応じてそれを使用できます。`await`
- _`IDisposable`を継承する`IAsyncDisposable`_ : 1 つのまたは一方だけを使用する必要があるため、型を強制的に実装することは意味がありません。
- _`IAsyncDisposable`ではなく`IDisposableAsync`_ : "非同期的なもの" という名前が付けられているのに対し、操作は "非同期的な処理" です。そのため、型はプレフィックスとして "async" を持ち、メソッドはサフィックスとして "async" を持ちます。

### <a name="iasyncenumerable--iasyncenumerator"></a>IAsyncEnumerable / IAsyncEnumerator

コア .NET ライブラリには、次の2つのインターフェイスが追加されます。

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

(追加の言語機能を使用しない) 一般的な消費は次のようになります。

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

破棄されるオプション:
- _`Task<bool> MoveNextAsync(); T current { get; }`_ : `Task<bool>` を使用すると、キャッシュされたタスクオブジェクトを使用して同期的に成功した `MoveNextAsync` 呼び出しを表すことができますが、非同期の完了には割り当てが依然として必要になります。  `ValueTask<bool>`を返すことにより、列挙子オブジェクト自体が `IValueTaskSource<bool>` を実装し、`MoveNextAsync`から返された `ValueTask<bool>` のバッキングとして使用できるようになります。これにより、オーバーヘッドを大幅に削減できます。
- _`ValueTask<(bool, T)> MoveNextAsync();`_ : 使用するのが困難なだけでなく、`T` を共変にすることはできません。
- _`ValueTask<T?> TryMoveNextAsync();`_ : 共変ではありません。
- _`Task<T?> TryMoveNextAsync();`_ : 共変ではなく、すべての呼び出しに対する割り当てなどです。
- _`ITask<T?> TryMoveNextAsync();`_ : 共変ではなく、すべての呼び出しに対する割り当てなどです。
- _`ITask<(bool,T)> TryMoveNextAsync();`_ : 共変ではなく、すべての呼び出しに対する割り当てなどです。
- _`Task<bool> TryMoveNextAsync(out T result);`_ : `out` の結果は、操作が同期的に返されたときに設定する必要があります。これは、今後長時間に及ぶ可能性があるタスクを非同期に完了するときではなく、結果を伝達する手段がないことを意味します。
- _`IAsyncEnumerator<T>` `IAsyncDisposable`を実装していませ_ん。これらを分離することもできます。  ただし、そのようにすることで、提案の他の特定の領域が複雑になります。これにより、コードは列挙子が破棄を提供しない可能性を処理できるようになるため、パターンベースのヘルパーを記述するのが困難になります。  さらに、列挙子が破棄を必要とすることがよくあります (たとえばC# 、finally ブロックを持つ非同期反復子、ほとんどの場合、ネットワーク接続からデータを列挙します)。それ以外の場合は、単純なオーバーヘッドを最小限に抑えて、メソッドを単に `public ValueTask DisposeAsync() => default(ValueTask);` として実装するのが簡単です。
- _ `IAsyncEnumerator<T> GetAsyncEnumerator()`: キャンセルトークンパラメーターがありません。

#### <a name="viable-alternative"></a>実行可能な代替手段:

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

`TryGetNext` は、同期的に使用できる限り、1つのインターフェイス呼び出しで項目を使用するために、内側のループで使用されます。  次の項目を同期的に取得できない場合、false が返され、false が返された場合は、呼び出し元は、次の項目が使用可能になるまで待機するか、別の項目がないことを確認するために `WaitForNextAsync` を呼び出す必要があります。 (追加の言語機能を使用しない) 一般的な消費は次のようになります。

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

これの利点は、2つのフォールドと1つの重要な点です。
- _Minor: 列挙子が複数のコンシューマーをサポートできるように_します。 複数の同時実行コンシューマーをサポートする列挙子にとって価値があるシナリオもあります。  `MoveNextAsync` と `Current` が分離されているため、実装で使用できないようにすることはできません。  これに対し、この方法では、列挙子を前方にプッシュして次の項目を取得することをサポートする1つのメソッド `TryGetNext` が提供されるため、必要に応じて列挙子を有効にすることができます。  ただし、このようなシナリオは、各コンシューマーに、共有されている列挙可能な独自の列挙子を与えることで有効にすることもできます。  さらに、すべての列挙子が同時使用をサポートしないようにすることをお勧めします。これにより、ほとんどの場合、これを必要としない大部分のオーバーヘッドが発生します。つまり、インターフェイスのコンシューマーは一般にこのような方法に依存することはできません。
- _Major: パフォーマンス_。 `MoveNextAsync`の /`Current` アプローチでは、操作ごとに2つのインターフェイス呼び出しが必要ですが、`WaitForNextAsync`/の最適なケースは、ほとんどの反復処理が同期的に完了し、操作ごとに1つのインターフェイス呼び出しのみが可能な、`TryGetNext` を使用して厳密な内側ループを有効にすることです。`TryGetNext`  これは、インターフェイス呼び出しが計算を独占する場合に、大きな影響を与える可能性があります。

ただし、これらを手動で使用する場合の複雑さが大幅に増加し、それらを使用したときにバグが発生する可能性が高くなるなど、重要ではない欠点があります。  また、パフォーマンス上の利点はマイクロベンチマークにも反映されていますが、実際の使用の大部分にインパクトされるとは思えません。  そのようなことが判明した場合は、2番目のインターフェイスセットを軽量な方法で導入できます。

破棄されるオプション:
- `ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` パラメーターを共変にすることはできません。  ここでは、参照型の結果に対してランタイム書き込みバリアが発生する可能性があるという小さな影響もあります (一般的な try パターンの問題)。

#### <a name="cancellation"></a>キャンセル

キャンセルをサポートするには、いくつかの方法が考えられます。
1. `IAsyncEnumerable<T>`の /`IAsyncEnumerator<T>` はキャンセルに依存しません。 `CancellationToken` はどこにも表示されません。  取り消しを行うには、任意の方法で列挙型または列挙子に `CancellationToken` を論理的に焼きます。たとえば、反復子を呼び出すときに、反復子メソッドの引数として `CancellationToken` を渡し、反復子の本体でその他のパラメーターと共に使用します。
2. `IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: `GetAsyncEnumerator`に `CancellationToken` を渡すと、それ以降の `MoveNextAsync` 操作は可能です。
3. `IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: 個々の `MoveNextAsync` の呼び出しに `CancellationToken` を渡します。
4. 1 & & 2: `CancellationToken`s を列挙可能な/列挙子に埋め込み、`CancellationToken`を `GetAsyncEnumerator`に渡すことができます。
5. 1 & & 3: `CancellationToken`s を列挙可能な/列挙子に埋め込み、`CancellationToken`を `MoveNextAsync`に渡すことができます。

純粋な理論上の観点からは、(5) が最も堅牢です。つまり、(a) `CancellationToken` を受け入れる `MoveNextAsync` によって、キャンセルされたものを最も細かく制御できます。 (b) `CancellationToken` は、反復子に引数として渡すことができる他の型、任意の型に埋め込むことができます。

ただし、この方法には複数の問題があります。
- `CancellationToken` がに渡されて、反復子の本体に `GetAsyncEnumerator` される方法を教えてください。  `GetEnumerator`に渡される `CancellationToken` にアクセスするために、をドットで切ることができる新しい `iterator` キーワードを公開できます。しかし、a) これは多くの追加メカニズムです。 b) これは非常にファーストクラスの市民になります。 c) 99% のケースは、反復子を呼び出して `GetAsyncEnumerator` を呼び出すのと同じコードであるように見えます。この場合、`CancellationToken` を引数としてメソッドに渡すだけで済みます。
- `CancellationToken` がメソッドの本体に渡される `MoveNextAsync` に渡す方法を教えてください。  これは、`iterator` ローカルオブジェクトから公開されているかのように、その値が待機中に変化する可能性があるということです。これは、トークンに登録されているすべてのコードが、待機する前にそのトークンを登録解除してから再登録する必要があることを意味します。また、コンパイラによって反復子に実装されているか、開発者によって手動で実装されているかに関係なく、`MoveNextAsync` のすべての呼び出しで登録と登録解除を行う必要がある場合もあります。
- 開発者は `foreach` ループをキャンセルするにはどうすればよいですか。  列挙可能な/列挙子に `CancellationToken` を与えることによって実行される場合、または、列挙子に対して `foreach`' をサポートする必要があります。これにより、これらのクラスがファーストクラスの市民になるようになりました。列挙子 (LINQ メソッドなど) や b などに基づいて構築されたエコシステムについて考える必要があります。次に、返された構造体に `GetAsyncEnumerator` するときに、指定され `CancellationToken` たトークンを格納する `IAsyncEnumerable<T>` の `WithCancellation` 拡張メソッドを使用します `GetAsyncEnumerator`が呼び出されます (そのトークンは無視されます)。  または、foreach の本文にある `CancellationToken` を使用することもできます。
- クエリの包含がサポートされている場合、`GetEnumerator` または `MoveNextAsync` に渡された `CancellationToken` はどのようにして各句に渡されますか?  最も簡単な方法は、単に句をキャプチャすることです。この時点で、`GetAsyncEnumerator`/`MoveNextAsync` に渡されるトークンは無視されます。

このドキュメントの以前のバージョンでは、(1) が推奨されていましたが、(4) に切り替えました。

(1) の主な問題は次の2つです。
- キャンセル可能な列挙体のプロデューサーは、いくつかの定型を実装する必要があり、コンパイラによる非同期反復子のサポートのみを利用して `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` メソッドを実装できます。
- 多くのプロデューサーでは、非同期の列挙可能なシグネチャに `CancellationToken` パラメーターを追加することが必要になる可能性があります。これにより、コンシューマーは、`IAsyncEnumerable` の種類を指定したときに必要なキャンセルトークンを渡すことができなくなります。

主に次の2つのシナリオがあります。
1. コンシューマーが非同期反復子メソッドを呼び出す場所 `await foreach (var i in GetData(token)) ...`
2. コンシューマーが特定の `IAsyncEnumerable` インスタンスを処理する `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...`。

非同期ストリームのプロデューサーとコンシューマーの両方にとって便利な方法で両方のシナリオをサポートするための適切な妥協点は、async iterator メソッドで特別に注釈が付けられたパラメーターを使用することです。 この目的には、`[EnumeratorCancellation]` 属性が使用されます。 この属性をパラメーターに配置すると、トークンが `GetAsyncEnumerator` メソッドに渡される場合に、パラメーターに渡された値の代わりにそのトークンを使用する必要があることをコンパイラに指示します。

`IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)` を検討します。 このメソッドの実装者は、メソッドの本体でパラメーターを使用するだけで済みます。 コンシューマーは、上記のいずれかの消費パターンを使用できます。
1. `GetData(token)`を使用すると、トークンは非同期の列挙型に保存され、イテレーションで使用されます。
2. `givenIAsyncEnumerable.WithCancellation(token)`を使用すると、`GetAsyncEnumerator` に渡されたトークンは、非同期の列挙可能なトークンに置き換えられます。

## <a name="foreach"></a>foreach

`foreach` は、`IEnumerable<T>`の既存のサポートに加えて、`IAsyncEnumerable<T>` をサポートするように強化されます。  また、関連するメンバーがパブリックに公開されている場合は、インターフェイスを直接使用することを避けるために、関連するメンバーがパブリックに公開されている場合は、インターフェイスを直接使用する `IAsyncEnumerable<T>` のではなく、`MoveNextAsync` と `DisposeAsync`の戻り値の型として代替の awaitables 使用できるようにします。

### <a name="syntax"></a>構文

次の構文を使用します。

```csharp
foreach (var i in enumerable)
```

C#は、`enumerable` を同期的な列挙可能として処理し続けます。これにより、非同期列挙体 (パターンの公開またはインターフェイスの実装) に関連する Api を公開する場合でも、同期 Api のみが考慮されます。

非同期 Api のみを考慮するように `foreach` を強制するために、`await` は次のように挿入されます。

```csharp
await foreach (var i in enumerable)
```

非同期 Api または同期 Api の使用をサポートする構文は提供されません。開発者は、使用する構文に基づいて選択する必要があります。

破棄されるオプション:
- _`foreach (var i in await enumerable)`_ : これは既に有効な構文です。その意味を変更すると、互換性に影響する変更になります。  つまり、`enumerable`を `await` し、同期的に反復可能なされたものを取得してから、同期的に反復処理します。
- _`foreach (var i await in enumerable)`、`foreach (var await i in enumerable)`、`foreach (await var i in enumerable)`_ : これらはすべて、次の項目を待機していることを示していますが、foreach には他の待機が含まれています。特に、列挙型が `IAsyncDisposable`の場合は、非同期の破棄を `await`します。  この await は、個々の要素に対してではなく foreach のスコープとして使用されます。したがって、`await` キーワードは `foreach` レベルである必要があります。  さらに、`foreach` に関連付けられている場合は、"await foreach" など、別の用語で `foreach` を記述する方法が提供されます。  しかし、さらに重要なのは、`foreach` 構文を `using` 構文と同時に考慮して、相互に一貫性を保ち、`using (await ...)` が既に有効な構文であるようにすることです。
- _`foreach await (var i in enumerable)`_

引き続き検討してください。
- `foreach` 現在、列挙子の反復処理はサポートされていません。  `IAsyncEnumerator<T>`が渡される方が一般的であると考えられます。したがって、`IAsyncEnumerable<T>` と `IAsyncEnumerator<T>`の両方で `await foreach` のサポートが必要になります。  ただし、このようなサポートを追加すると、`IAsyncEnumerator<T>` がファーストクラスの市民であるかどうか、および列挙体に加えて列挙子を操作する連結子のオーバーロードが必要かどうかの質問が導入されます。    列挙体ではなく列挙子を返すメソッドを推奨しますか。 この点については、引き続き説明します。  サポートしないと判断した場合は、列挙子を引き続き `foreach`できる拡張メソッド `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` を導入することをお勧めします。  サポートを希望する場合は、`await foreach` が列挙子で `DisposeAsync` を呼び出す必要があるかどうかを決定する必要があります。また、回答が "いいえ" であると考えられる場合は、破棄の制御は `GetEnumerator`を呼び出した人によって処理される必要があります。

### <a name="pattern-based-compilation"></a>パターンに基づくコンパイル

コンパイラは、存在する場合はパターンベースの Api にバインドし、インターフェイスを使用してこれらを優先します (パターンはインスタンスメソッドまたは拡張メソッドで満たされる場合があります)。  パターンの要件は次のとおりです。
- 列挙可能なは、引数なしで呼び出される可能性があり、関連するパターンを満たす列挙子を返す `GetAsyncEnumerator` メソッドを公開する必要があります。
- 列挙子は、引数なしで呼び出される可能性のある `MoveNextAsync` メソッドを公開する必要があります。このメソッドは、`await`ed で、`GetResult()` が `bool`を返す可能性があるものを返します。
- 列挙子は、列挙されるデータの種類を表す `T` を返す getter を持つ `Current` プロパティも公開する必要があります。
- 列挙子は、必要に応じて、引数を指定せずに呼び出すことができ、ed `await`できるものを返し、その `GetResult()` が `void`を返す可能性のある `DisposeAsync` メソッドを公開する場合があります。

このコードによって以下が行われます。

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

はに相当するものに変換されます。

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

反復される型が適切パターンを公開していない場合は、インターフェイスが使用されます。

### <a name="configureawait"></a>ConfigureAwait

このパターンに基づくコンパイルでは、`ConfigureAwait` の拡張メソッドを使用して、すべての待機で `ConfigureAwait` を使用できます。

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

これは .NET にも追加する型に基づいて行われます。これは、おそらく、次のようになります。

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

このアプローチでは、パターンベースの列挙体で `ConfigureAwait` を使用できないことに注意してください。ここでも、`ConfigureAwait` は `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` の拡張機能としてのみ公開されており、任意の待機可能には適用できません。これは、タスクに適用した場合 (タスクの継続サポートに実装されている動作を制御します)、待機可能がタスクでない可能性があるパターンを使用する場合には意味が  待機可能を返す人は、このような高度なシナリオで独自のカスタム動作を提供できます。

(スコープまたはアセンブリレベルの `ConfigureAwait` ソリューションをサポートする何らかの方法を使用できる場合は、これは必要ありません)。

## <a name="async-iterators"></a>非同期反復子

言語/コンパイラは、その使用に加えて `IAsyncEnumerable<T>`s と `IAsyncEnumerator<T>`の生成をサポートします。 現在、この言語では次のような反復子の作成がサポートされています。

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

ただし `await` は、これらの反復子の本体では使用できません。  サポートを追加します。

### <a name="syntax"></a>構文

反復子に対する既存の言語サポートは、`yield`が含まれているかどうかに基づいて、メソッドの反復子の性質を推論します。  非同期反復子についても同じことが当てはまります。  このような非同期反復子は、シグネチャに `async` を追加することによって同期反復子と区別され、戻り値の型として `IAsyncEnumerable<T>` または `IAsyncEnumerator<T>` を持つ必要があります。  たとえば、上の例は、次のように、非同期反復子として記述できます。

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

検討対象の候補:
- _シグネチャで `async` を使用しない_: `async` を使用すると、そのコンテキストで `await` が有効かどうかを判断するために使用されるため、コンパイラによって技術的に要求される可能性があります。  ただし、必要でない場合でも、`await` が `async`としてマークされたメソッドでのみ使用されることがあり、一貫性を保つことが重要であるように思えました。
- _`IAsyncEnumerable<T>`に対してカスタムビルダーを有効_にすると、これが将来のものになりますが、機械は複雑になり、対応する同期に対してはサポートされません。
- _シグネチャに `iterator` キーワードを_指定すると、非同期反復子はシグネチャで `async iterator` を使用し、`yield` は `iterator`を含む `async` メソッドでのみ使用できます。`iterator` は、同期反復子でも省略可能になります。  パースペクティブによっては、`yield` が許可されているかどうか、メソッドが `yield` を使用するかどうかに基づいて、コンパイラによる製造ではなく `IAsyncEnumerable<T>` 型のインスタンスを実際に返すかどうかを、メソッドのシグネチャによって明確にするという利点があります。  ただし、これは同期反復子とは異なり、要求を要求することはできません。  さらに、開発者によっては、余分な構文が気に入らない場合もあります。  最初からデザインする場合は、これが必要になることがありますが、この時点で、非同期反復子を同期反復子の近くに維持することには、より多くの価値があります。

## <a name="linq"></a>LINQ

`System.Linq.Enumerable` クラスには ~ 200 のオーバーロードがあり、これらはすべて `IEnumerable<T>`の観点で機能します。これらのうちのいくつかは `IEnumerable<T>`を受け入れますが、そのうちのいくつかは `IEnumerable<T>`を生成します。  `IAsyncEnumerable<T>` の LINQ サポートを追加すると、このようなオーバーロードをすべて複製することが必要になる可能性があります。これは、別の ~ 200 です。  `IAsyncEnumerator<T>` は、同期の世界にいる `IEnumerator<T>` よりも、非同期環境ではスタンドアロンエンティティとして一般的になる可能性が高いため、`IAsyncEnumerator<T>`で動作する別の ~ 200 のオーバーロードが必要になる可能性があります。  さらに、多くのオーバーロードは、述語 (`Func<T, bool>`を受け取る `Where` など) を処理します。また、同期述語と非同期述語の両方を処理する `IAsyncEnumerable<T>`ベースのオーバーロードを使用することをお勧めします (`Func<T, bool>`に加えて `Func<T, ValueTask<bool>>` など)。  これは、現在 ~ 400 の新しいオーバーロードには適用されませんが、大まかな計算として、半分に適用できることが挙げられます。これは、合計で600の新しいメソッドに対して、別の ~ 200 のオーバーロードを意味します。

これは、対話的な拡張機能 (Ix) のような拡張ライブラリが検討されている場合に、さらに多くの Api を使用できるようになります。  ただし、Ix にはこれらの多くの実装が既に存在しているので、その作業を複製するのに大きな理由があるとは思えません。代わりに、開発者が `IAsyncEnumerable<T>`で LINQ を使用する場合には、community を改善し、それを推奨することをお勧めします。

クエリの読解構文にも問題があります。  クエリの包含のパターンベースの性質により、いくつかの演算子を使用することができます。たとえば、Ix に次のようなメソッドが用意されているとします。

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

次にC# 、このコードは "作業のみ" になります。

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

ただし、次の例のように、句での `await` の使用をサポートするクエリの読解構文はありません。

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

次に、これは "仕事のみ" になります。

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

ただし、`select` 句に `await` インラインで書き込む方法はありません。  別の作業として、`async { ... }` 式を言語に追加してみましょう。この時点で、クエリの包含での使用を許可できます。上記の式は、次のように記述できます。

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

または、`async from`のサポートなどによって `await` を式で直接使用できるようにします。  ただし、ここでは、機能セットの他の部分に影響を与える可能性はほとんどありません。これは、現時点では特に投資が重要な価値の高いものではないため、ここでは何も追加しないことを提案します。

## <a name="integration-with-other-asynchronous-frameworks"></a>他の非同期フレームワークとの統合

`IObservable<T>` およびその他の非同期フレームワーク (リアクティブストリームなど) との統合は、言語レベルではなくライブラリレベルで実行されます。  たとえば、`IAsyncEnumerator<T>` のすべてのデータを `IObserver<T>` にパブリッシュするには、列挙子に対して "ing" を `await foreach`し、データをオブザーバーに `OnNext`するだけです。そのため、`AsObservable<T>` の拡張メソッドを使用できます。  `await foreach` で `IObservable<T>` を使用するには、データのバッファリングが必要です (前の項目がまだ処理されている間に別の項目がプッシュされた場合)。ただし、このようなプッシュプルアダプターを簡単に実装して、`IObservable<T>` を `IAsyncEnumerator<T>`でプルできるようにすることができます。  等. Rx/Ix は、このような実装のプロトタイプを既に提供しています。また、 https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels などのライブラリでは、さまざまな種類のバッファリングデータ構造を提供しています。  この段階では、言語を使用する必要はありません。
