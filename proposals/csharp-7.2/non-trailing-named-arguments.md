---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483644"
---
# <a name="non-trailing-named-arguments"></a>末尾以外の名前付き引数

## <a name="summary"></a>まとめ
[summary]: #summary
名前付き引数は、正しい位置で使用されている限り、末尾以外の位置で使用できます。 (例: `DoSomething(isEmployed:true, name, age);`)。

## <a name="motivation"></a>目的
[motivation]: #motivation

主な動機は、冗長な情報を入力しないようにすることです。 通常は、引数を順序どおりに渡すのではなく、コードを明確にするために、リテラル (`null`、`true`など) に名前を付けます。
次のすべての引数にも名前が付いている場合を除き、現在のところ (`CS1738`) は許可されていません。

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

その他の例:
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

これは、params でも機能します。
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

7\.5.1 (引数リスト) では、現在の仕様では次のように示されています。
> 引数*名*を指定*した引数*は、__名前付き引数__と呼ばれますが、引数*名*のない*引数*は__位置引数__と呼ばれます。 *引数リスト*の名前付き引数の後に位置引数を指定すると、エラーになります。

この提案では、このエラーを削除し、引数に対応するパラメーター (7.5.1.1 を参照) を検索するためのルールを更新します。

引数: インスタンスコンストラクター、メソッド、インデクサー、およびデリゲートのリスト。
- [既存のルール]
- 無名の名前付き引数または名前付き params 引数の後にある場合、名前のない引数はパラメーターなしに対応します。

特に、これにより `M(c: false, valueB);`で `void M(bool a = true, bool b = true, bool c = true, );` を呼び出すことができなくなります。 最初の引数は、位置を指定しないで使用されます (引数は最初の位置で使用されますが、"c" という名前のパラメーターは3番目の位置にあります)。したがって、次の引数はという名前にする必要があります。

言い換えると、末尾以外の名前付き引数は、名前と位置が同じ対応パラメーターを検索する場合にのみ許可されます。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

この提案では、オーバーロードの解決で名前付き引数を使用して、既存の微妙な exacerbates を行います。 次に例を示します。

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

この状況は、次のようにパラメーターを交換することで取得できます。

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

同様に、`void M(int a, int b)` と `void M(int x, string y)`の2つのメソッドがある場合、誤った呼び出し `M(x: 1, 2)` によって、2番目のオーバーロードに基づく診断が生成されます ("int ' から ' string ' に変換することはできません)。 名前付き引数が末尾の位置で使用されている場合、この問題は既に存在しています。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

考慮すべき選択肢がいくつかあります。

- 現状
- 中央に特定の名前を入力するときに、末尾の引数のすべての名前を入力するための IDE サポートを提供します。

どちらも、引数リストの先頭にリテラルの名前が1つ必要な場合でも、複数の名前付き引数が導入されるため、詳細度が高くなります。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>会議のデザイン
[ldm]: #ldm
この機能は、LDM では、2017年5月16日に承認され、原則 (提案/プロトタイプに移行することは ok) について簡単に説明しました。 また、2017年6月28日にも簡単に説明しました。

Championed の問題に関連する最初のディスカッションに関連 https://github.com/dotnet/csharplang/issues/518 https://github.com/dotnet/csharplang/issues/570
