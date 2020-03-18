---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483482"
---
# <a name="readonly-locals-and-parameters"></a>readonly ローカルおよびパラメーター

* [x] が提案されています
* [] プロトタイプ
* [] の実装
* [] 仕様

## <a name="summary"></a>まとめ
[summary]: #summary

ローカルおよびパラメーターに読み取り専用として注釈を付けて、これらのローカルとパラメーターが緩やかに変化しないようにします。

## <a name="motivation"></a>目的
[motivation]: #motivation

今日では、`readonly` キーワードをフィールドに適用できます。これには、構築時に (静的フィールドの場合は静的な構築、インスタンスフィールドの場合はインスタンスの構築時には) フィールドを確実に書き込むことができるという効果があります。これにより、開発者は、変更する必要のない状態を誤って上書きして間違いを防ぐことができます。 ただし、開発者が値を変換しないようにする必要があるのは、フィールドだけではありません。 特に、一時的な状態を格納するためのローカル変数を作成するのは一般的ですが、誤って計算やそのようなバグが発生する可能性があります。そのような場合は特に、このようなリフトされたフィールドを `readonly`としてマークする方法はありません。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

ローカル変数は、宣言時にのみ設定されるようにコンパイラによって `readonly` として注釈が付けられます ( C# ' foreach ' ループ内の反復変数や ' using ' ブロックの使用されている変数など、の特定のローカル変数は既に暗黙的に読み取り専用ですが、現在の開発者は他のローカルを `readonly`としてマークする このような `readonly` ローカルには初期化子が必要です。

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

`readonly var`の短縮形として、既存のコンテキストキーワード `let` を使用することもできます。たとえば、

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

初期化子の機能に関する特別な制約はありません。また、現在、ローカルの初期化子として有効なすべてのものにすることができます。たとえば、

```csharp
readonly T data = arg1 ?? arg2;
```

ローカルでの `readonly` は、ラムダとクロージャを操作する場合に特に役立ちます。 匿名メソッドまたはラムダが外側のスコープからローカル状態にアクセスすると、その状態は、"display class" によって表されるコンパイラによってクロージャにキャプチャされます。  キャプチャされた各 "ローカル" はこのクラスのフィールドですが、コンパイラはこのフィールドを代わりに生成するため、ラムダが "ローカル" に誤って書き込まれないようにするために `readonly` として注釈を付けることはできません (少なくとも結果の MSIL にはないため、引用符で囲まれています)。 `readonly` ローカル変数を使用すると、コンパイラはラムダがローカルに書き込まれるのを防ぐことができます。これは、間違った書き込みが発生する可能性がありますが、同時実行のバグが発生する可能性がありますが、非常に複雑な場合は、マルチスレッドが関係するシナリオで特に役立ちます。

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

特別な形式のローカルでは、パラメーターも `readonly`として注釈付けされます。 これは、メソッドの呼び出し元がパラメーターに渡すことができることには影響しません (`readonly` フィールドに格納できる値に関する制約はありません) が、`readonly` ローカルの場合と同様に、コンパイラは宣言後のパラメーターへのコードの書き込みを禁止します。つまり、メソッドの本体でパラメーターへの書き込みが禁止されます。

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

`readonly` パラメーターは、そのメソッドのコンパイラによって出力される署名/メタデータには影響しません。また、メソッドの本体のコンパイルをコンパイラが処理する方法にのみ影響します。 したがって、たとえば、基本の仮想メソッドは `readonly` パラメーターを持つことができ、そのパラメーターをオーバーライドで書き込み可能にすることができます。

フィールドの場合と同様に、ローカルおよびパラメーターの `readonly` は浅いため、ストレージの場所に影響しますが、オブジェクトグラフに推移的に影響することはありません。 ただし、フィールドと同様に、`readonly` ローカル/パラメーター構造体でメソッドを呼び出すと、実際には構造体のコピーが作成され、そのコピーに対してメソッドが呼び出され、`this`の内部的な変更を回避できます。

`ref readonly` もサポートされている場合を除き、`ref` または `out` 引数として `readonly` ローカルパラメーターとパラメーターを渡すことはできません。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

- `val` は、`let`に代わる代替手段として使用できます。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- `readonly ref` / `ref readonly` / `readonly ref readonly`: この提案とは別に、`ref readonly` を処理する方法についての質問を残しました。
- この提案では、読み取り専用の構造体/変更できない型は解決されません。 これは別の提案のために残されています。

## <a name="design-meetings"></a>会議のデザイン

- 2015年1月21日 (<https://github.com/dotnet/roslyn/issues/98>) について簡単に説明します。
