---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483554"
---
# <a name="compiler-intrinsics"></a>コンパイラの組み込み

## <a name="summary"></a>まとめ

この提案は、現在は効率的にアクセスできない、または `ldftn`、`ldvirtftn`、`ldtoken`、および `calli`の下位レベルの IL オペコードを公開する言語コンストラクトを提供します。 これらの低レベルのオペコードは、高パフォーマンスのコードでは重要であり、開発者はそれらにアクセスするための効率的な方法を必要とします。

## <a name="motivation"></a>目的

この機能の動機と背景については、次の問題で説明されています (この機能が実装される可能性があります)。 

https://github.com/dotnet/csharplang/issues/191

この代替設計の提案は、@msjabby によって元の提案のプロトタイプの実装を確認した後、重要なコードベース全体で使用できるようになります。 この設計は、@mjsabby、@tmat および @jkotasからの重要な入力で行われました。

## <a name="detailed-design"></a>詳細なデザイン 

### <a name="allow-address-of-to-target-methods"></a>ターゲットメソッドへのアドレスを許可する

アドレス式の引数として、メソッドグループが許可されるようになりました。 このような式の型が `void*`されます。 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

ここではデリゲート変換がないため、メソッドグループのメンバーをフィルター処理するためのメカニズムは、静的/インスタンスアクセスだけです。 メンバーを区別できない場合は、コンパイル時エラーが発生します。

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

このコンテキストの addressof 式は、次の方法で実装されます。

- ldftn: メソッドが非仮想の場合。
- ldvirtftn: メソッドが仮想である場合。

この機能の制限:

- インスタンスメソッドは、値に対して呼び出し式を使用する場合にのみ指定できます
- ローカル関数は `&`では使用できません。 これらのメソッドの実装の詳細は、言語によって意図的に指定されていません。 これには、静的であるかインスタンスであるか、またはそれらが出力されるシグネチャが含まれます。

### <a name="handleof"></a>handleof

`handleof` コンテキストキーワードは、`ldtoken` 命令を使用して、フィールド、メンバー、または型を、それと等価な `RuntimeHandle` 型に変換します。 式の正確な型は、`handleof`内の名前の種類によって異なります。

- フィールド: `RuntimeFieldHandle`
- 種類: `RuntimeTypeHandle`
- メソッド: `RuntimeMethodHandle`

`handleof` する引数は `nameof`と同じです。 これは、簡易名、修飾名、メンバーアクセス、指定されたメンバーを持つ基本アクセス、または指定されたメンバーを使用したこのアクセスである必要があります。 引数の式はコード定義を識別しますが、評価されることはありません。

`handleof` 式は実行時に評価され、`RuntimeHandle`の戻り値の型があります。 安全なコードや安全ではないコードで実行できます。 

``` 
RuntimeHandle stringHandle = handleof(string);
```

この機能の制限:

- プロパティは `handleof` 式では使用できません。
- スコープに既存の `handleof` 名がある場合、`handleof` 式は使用できません。 たとえば、型、名前空間などです。

### <a name="calli"></a>呼び出し

コンパイラは、`.calli` 命令に効率的に変換する新しい型の `extern` 関数のサポートを追加します。 Extern 属性は、次の図形の属性でマークされます。

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

これにより、開発者は次の形式でメソッドを定義できます。

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

`CallIndirect` 属性が適用されているメソッドに関する制限事項を次に示します。

- `DllImport` 属性を持つことはできません。
- をジェネリックにすることはできません。

## <a name="open-issues"></a>懸案事項を開く

### <a name="callingconvention"></a>CallingConvention

設計された `CallIndirectAttribute` は、マネージ呼び出し規約のエントリを持たない `CallingConvention` 列挙型を使用します。 列挙型は、この呼び出し規約を含めるように拡張する必要があります。または、属性で別の方法を使用する必要があります。

## <a name="considerations"></a>考慮事項

### <a name="disambiguating-method-groups"></a>明確化メソッドグループ

ここでは、アドレス式に渡されるメソッドグループを明確に区別できるようにする機能について説明しました。 たとえば、構文に署名要素を追加する可能性があります。

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

これは、説得力のあるケースを作成できなかったため、または単純な構文がここで想定されていたため、拒否されました。 また、非常に簡単に前進することもできます。単純に明確な別のC#メソッドを定義し、コードを使用して目的の関数を呼び出すことができます。 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

これは、`static` ローカル関数が言語に入る場合にもより簡単になります。 次に、あいまいなアドレス操作を使用したのと同じ関数で、回避策を定義できます。

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a>LoadTypeTokenInt32

コンパイル時にメタデータトークンを `int` 値として読み込むことが許可された元の提案。 基本的には、`handleof` と同じ引数を持つ `tokenof` を持ちますが、コンパイル時に `int` 定数に評価されます。 

これは、IL の書き換え (.NET には多くの場合) で大きな問題が発生するため、拒否されました。 このような再作成者は、多くの場合、これらの値を無効にできるような方法でメタデータテーブルを操作します。 単純な `int` 値として格納されている場合、このような再作成者がこれらの値を更新するための適切な方法はありません。

メタデータエントリの非透過ハンドルを持つ基礎となる概念は、ランタイムチームによって引き続き調査されます。 

## <a name="future-considerations"></a>将来の注意事項

### <a name="static-local-functions"></a>静的ローカル関数

これは、ローカル関数で `static` 修飾子を許可する[提案](https://github.com/dotnet/csharplang/issues/1565)を指します。 このような関数は、ソースコードで正確なシグネチャを指定した `static` として出力されることが保証されます。 このような関数は、ローカル関数の現在の問題を含んでいないため、`&` の有効な引数である必要があります。

### <a name="nativecallableattribute"></a>NativeCallableAttribute

CLR には、ネイティブコードから直接呼び出すことができる方法でマネージメソッドを出力できる機能があります。 これを行うには、メソッドに `NativeCallableAttribute` を追加します。 このようなメソッドはネイティブコードからのみ呼び出すことができるため、シグネチャには blittable 型のみを含める必要があります。 マネージコードからを呼び出すと、ランタイムエラーが発生します。 

この機能は、次のように、この提案に適しています。

- マネージコードで定義されている関数をネイティブコードに渡すことは、マネージコードまたはネイティブコードのオーバーヘッドを発生させることなく、(のアドレスを介して) 関数ポインターとして使用します。 
- ランタイムでは、コンパイル時に呼び出されないように、マネージコードでこのような関数にサイトエラーを使用できます。




