---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483542"
---
# <a name="simplified-null-argument-checking"></a>簡略化された Null 引数のチェック

## <a name="summary"></a>まとめ
この提案では、メソッドの引数を検証するための簡略化された構文を提供し、`ArgumentNullException` を適切にスロー `null` します。

## <a name="motivation"></a>目的
Null 許容の参照型を設計する作業で、`null` 引数の検証に必要なコードを確認しました。 NRT がコード実行開発者に影響を与えない場合でも、完全に `null` クリーンであるプロジェクトでも、`if (arg is null) throw` ボイラープレートコードを追加する必要があります。 これにより、言語で検証 `null` 引数の最小構文を調べることが求められました。 

この `null` パラメーター検証構文は、NRT と頻繁にペアになることが想定されていますが、提案は完全に独立しています。 構文は `#nullable` ディレクティブとは無関係に使用できます。

## <a name="detailed-design"></a>詳細なデザイン 

### <a name="null-validation-parameter-syntax"></a>Null 検証パラメーターの構文
パラメーターリストのパラメーター名の後に、感嘆符演算子 (`!`) を配置できます。これにC#より、コンパイラは、そのパラメーターの標準 `null` チェックコードを出力します。 これは `null` 検証パラメーターの構文と呼ばれます。 次に例を示します。

``` csharp
void M(string name!) {
    ...
}
```

はに変換されます。

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

生成された `null` チェックは、開発者がメソッド内でコードを作成する前に発生します。 複数のパラメーターに `!` 演算子が含まれている場合は、パラメーターが宣言されている順序でチェックが行われます。

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

このチェックは、特に `null`との参照の等価性のために行われますが、`==` またはユーザー定義の演算子は呼び出しません。 また、`!` 演算子は、`null`に対して型が等しいかどうかをテストできるパラメーターにのみ追加できることを意味します。 これは、型が値型であることがわかっているパラメーターでは使用できないことを意味します。

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

コンストラクターの場合は、コンストラクター内の他のコードの前に `null` 検証が行われます。 次のものが含まれます。 

- `this` または `base` を使用した他のコンストラクターへのチェーン 
- コンストラクターで暗黙的に発生するフィールド初期化子

次に例を示します。

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

は、次のように解釈されます。

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

注: これは、有効C#なコードではなく、実装によって実行される内容の概数です。 

`null` 検証パラメーターの構文は、ラムダパラメーターリストでも有効です。 これは、1つのパラメーターの構文で、parens が不足している場合でも有効です。

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

構文は、反復子メソッドのパラメーターでも有効です。 反復子内の他のコードとは異なり、基になる列挙子が開始されたときではなく、反復子メソッドが呼び出されると `null` 検証が行われます。 これは、従来の反復子または `async` 反復子の場合に当てはまります。

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

`!` 演算子は、メソッド本体が関連付けられているパラメーターリストに対してのみ使用できます。 これは、`abstract` メソッド、`interface`、`delegate`、または `partial` メソッドの定義では使用できないことを意味します。

### <a name="extending-is-null"></a>拡張が null
式 `is null` が有効な型は、制約のない型パラメーターを含むように拡張されます。 これにより、`null` チェックが有効になっているすべての型に対して `null` をチェックする目的を満たすことができます。 特に、値型として知られていない型です。 たとえば、`struct` に制約される型パラメーターは、この構文では使用できません。

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

型パラメーターに対する `is null` の動作は、今日 `== null` と同じになります。 型パラメーターが値型としてインスタンス化されている場合、コードは `false`として評価されます。 参照型の場合、コードは適切な `is null` チェックを行います。

### <a name="intersection-with-nullable-reference-types"></a>Null 値が許容される参照型との積集合
名前に `!` 演算子が適用されているすべてのパラメーターは、null 許容の状態が `null`ではなくなります。 これは、パラメーター自体の型が `null`可能性がある場合でも同様です。 これは、`string?`のような明示的な null 許容型、または制約のない型パラメーターを使用して発生する可能性があります。 

パラメーターの `!` 構文をパラメーターの明示的な null 許容型と組み合わせると、コンパイラによって警告が発行されます。

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a>懸案事項を開く
なし

## <a name="considerations"></a>考慮事項

### <a name="constructors"></a>コンストラクター
コンストラクターのコード生成では、現在、標準的な `null` の検証から、`null` 検証パラメーターの構文 (`!`) への移行時に、監視が可能で、監視可能な動作が変更されています。 標準検証の `null` のチェックインは、フィールド初期化子と `base` または `this` の両方の呼び出しの後に発生します。 つまり、開発者は、`null` 検証の100% を新しい構文に必ずしも移行することはできません。 コンストラクターでは、少なくとも検査が必要です。

議論した後で、非常に多くの導入に関する問題が発生する可能性が非常に高いと判断しました。 コンストラクター内のロジックが実行される前に `null` チェックが実行されると、より論理的なものになります。 重大な互換性の問題が検出された場合は、を再実行できます。

### <a name="warning-when-mixing--and-"></a>混合時の警告 そして！
Null 許容型に明示的に型指定されたパラメーターに `!` 構文が適用されている場合に、警告を発行するかどうかについては、時間がかかることがありました。 この画面では、開発者による無意味宣言のように見えますが、型階層で開発者がこのような状況に強制的に適用されることもあります。 

一連のアセンブリに対して次のクラス階層を考えてみます (すべてが `null` チェックが有効になっていることを前提としています)。

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

ここで `C3` の作成者は、パラメーター `o`に検証 `null` を追加することに決定しました。 これは、機能がどのように使用されているかを完全に示すものです。

後で、Assembly2 の作成者が次のオーバーライドを追加することにしたとします。

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

これは、入力位置に対してコントラクトの柔軟性を高めるために有効であるため、null 許容型参照型によって許可されます。 NRT の一般的な機能を使用すると、パラメーターまたは戻り値の null 値の許容範囲に対して、妥当な co/反変性を実現できます ただし、この言語では、元の宣言ではなく、最も限定的なオーバーライドに基づいて、共同/反変性がチェックされます。 つまり、Assembly3 の作成者には、一致しない `o` の種類に関する警告が表示されます。これを回避するには、シグネチャを次のように変更する必要があります。 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

この時点で、Assembly3 の作成者にはいくつかの選択肢があります。

- `object?` と `object` の不一致に関する警告を受け入れたり非表示にしたりすることができます。
- `object?` と `!` の不一致に関する警告を受け入れたり非表示にしたりすることができます。
- `null` 検証チェックを削除する (`!` を削除して、明示的なチェックを行う) だけです。

これは実際のシナリオですが、ここでは警告を使用して先に進むことを考えています。 警告が予想よりも頻繁に発生する場合は、後で削除することができます (逆は true ではありません)。

### <a name="implicit-property-setter-arguments"></a>暗黙的なプロパティ setter 引数
パラメーターの `value` 引数は暗黙的であり、どのパラメーターリストにも表示されません。 つまり、この機能のターゲットにすることはできません。 プロパティ setter 構文を拡張して、`!` 演算子を適用できるようにするパラメーターリストを含めることができます。 しかし、この機能を利用すると、`null` 検証がより簡単になります。 このように、暗黙的な `value` 引数は、この機能では機能しません。

## <a name="future-considerations"></a>将来の注意事項
なし
