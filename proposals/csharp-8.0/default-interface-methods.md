---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484082"
---
# <a name="default-interface-methods"></a>既定のインターフェイスメソッド

* [x] が提案されています
* [] プロトタイプ:[実行](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)中
* [] の実装: なし
* [] 仕様: 進行中、以下

## <a name="summary"></a>まとめ
[summary]: #summary

_仮想拡張メソッド_のサポートを追加します。具体的な実装を持つインターフェイスにメソッドを追加します。 このようなインターフェイスを実装するクラスまたは構造体は、インターフェイスメソッドに対して、クラスまたは構造体によって実装されるか、基本クラスまたはインターフェイスから継承される、_最も限定的_な実装を持つ必要があります。 仮想拡張メソッドを使用すると、API 作成者は、そのインターフェイスの既存の実装との間でソースまたはバイナリの互換性を損なうことなく、将来のバージョンでインターフェイスにメソッドを追加できます。

これらは、Java の["既定のメソッド"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)に似ています。

(考えられる実装手法に基づく) この機能では、CLI/CLR で対応するサポートが必要です。 この機能を利用するプログラムは、以前のバージョンのプラットフォームでは実行できません。

## <a name="motivation"></a>目的
[motivation]: #motivation

この機能の主な動機は次のとおりです。

- 既定のインターフェイスメソッドを使用すると、API 作成者は、そのインターフェイスの既存の実装とソースまたはバイナリの互換性を損なうことなく、将来のバージョンでインターフェイスにメソッドを追加できます。
- この機能をC#使用すると、は、同様の機能をサポートする[Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)と[iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)を対象とする api と相互運用できます。
- 結局のところ、既定のインターフェイス実装を追加すると、"特徴" 言語機能 (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>) の要素が提供されます。 特徴は、強力なプログラミング手法 (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>) であることが実証されています。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

インターフェイスの構文は、を許可するように拡張されています。

- 定数、演算子、静的コンストラクター、および入れ子にされた型を宣言するメンバー宣言。
- メソッド、インデクサー、プロパティ、またはイベントアクセサーの*本体*(つまり、"既定" の実装)。
- 静的フィールド、メソッド、プロパティ、インデクサー、およびイベントを宣言するメンバー宣言
- 明示的なインターフェイス実装構文を使用したメンバー宣言そして
- 明示的なアクセス修飾子 (既定のアクセスは `public`)。

本体を持つメンバーは、オーバーライドする実装を提供しないクラスと構造体のメソッドに対して、"既定の" 実装を提供するインターフェイスを許可します。

インターフェイスには、インスタンスの状態を含めることはできません。 静的フィールドが許可されるようになりましたが、インスタンス フィールドはインターフェイスでは許可されません。 インスタンスの自動プロパティは、非表示フィールドを暗黙的に宣言するため、インターフェイスではサポートされません。

静的メソッドとプライベートメソッドでは、インターフェイスのパブリック API を実装するために使用される、役に立つリファクタリングとコードの編成が可能です。

インターフェイスのメソッドオーバーライドでは、明示的なインターフェイスの実装構文を使用する必要があります。

*Variance_annotation*で宣言された型パラメーターのスコープ内で、クラス型、構造体型、または列挙型を宣言すると、エラーになります。  たとえば、次の `C` の宣言はエラーになります。

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a>インターフェイスの具象メソッド

この機能の最も単純な形式は、インターフェイスで*具象メソッド*を宣言する機能です。これは、本体を持つメソッドです。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

このインターフェイスを実装するクラスは、具象メソッドを実装する必要はありません。

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

クラス `C` 内の `IA.M` の最終的なオーバーライドは、`IA`で宣言さ `M` 具象メソッドです。 クラスは、そのインターフェイスからメンバーを継承しないことに注意してください。この機能によって変更されることはありません。

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

インターフェイスのインスタンスメンバー内では、`this` には外側のインターフェイスの型があります。

### <a name="modifiers-in-interfaces"></a>インターフェイス内の修飾子

インターフェイスの構文は、メンバーに対して修飾子を許可するためには緩和されています。 `private`、`protected`、`internal`、`public`、`virtual`、`abstract`、`sealed`、`static`、`extern`、`partial`を使用できます。

> ***TODO***: 他の修飾子が存在するかどうかを確認してください。

宣言が本体を含むインターフェイスメンバーは、`sealed` または `private` 修飾子が使用されていない限り、`virtual` メンバーです。 `virtual` 修飾子は、暗黙的に `virtual`される関数メンバーで使用できます。 同様に、本体のないインターフェイスメンバーの既定値は `abstract` ですが、修飾子を明示的に指定することもできます。 非仮想メンバーは、`sealed` キーワードを使用して宣言できます。

`private` またはインターフェイスの `sealed` 関数メンバーが本文を持たない場合はエラーになります。 `private` 関数メンバーに修飾子 `sealed`を含めることはできません。

アクセス修飾子は、許可されているすべての種類のメンバーのインターフェイスメンバーで使用できます。 `public` アクセスレベルは既定値ですが、明示的に指定することもできます。

> ***懸案事項を開く:***`protected` や `internal`などのアクセス修飾子の正確な意味と、これらの宣言 (派生インターフェイスの場合) または実装しない宣言 (インターフェイスを実装するクラス) を指定する必要があります。

インターフェイスは、入れ子にされた型、メソッド、インデクサー、プロパティ、イベント、および静的コンストラクターを含む `static` メンバーを宣言できます。 すべてのインターフェイスメンバーの既定のアクセスレベルは `public`です。

インターフェイスでは、インスタンスコンストラクター、デストラクター、またはフィールドを宣言できません。

> ***終了済みの問題:*** インターフェイスで演算子宣言を許可する必要がありますか。 変換演算子ではありませんが、他の演算子についてはどうでしょうか。 ***Decision***: 変換演算子、等値演算子、および非等値演算子を*除き*、演算子を使用できます。

> ***終了済みの問題:*** 基底インターフェイスのメンバーを非表示にするインターフェイスメンバー宣言では、`new` 許可する必要がありますか。 ***決定***: はい。

> ***終了済みの問題:*** 現在、インターフェイスまたはそのメンバーの `partial` は許可されていません。 これには別の提案が必要です。 ***決定***: はい。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a>インターフェイスでのオーバーライド

オーバーライド宣言 (つまり、`override` 修飾子を含む) を使用すると、プログラマは、コンパイラまたはランタイムがそれ以外の方法で仮想メンバーを見つけることができないようにすることができます。 また、スーパーインターフェイスの抽象メンバーを、派生インターフェイスの既定のメンバーに変えることもできます。 オーバーライド宣言では、インターフェイス名で宣言を修飾することによって、特定の基底インターフェイスメソッドを*明示的*にオーバーライドできます (この場合、アクセス修飾子は許可されません)。 暗黙のオーバーライドは許可されていません。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

インターフェイスのオーバーライド宣言を `sealed`として宣言することはできません。

インターフェイスのパブリック `virtual` 関数メンバーは、派生インターフェイスで明示的にオーバーライドすることができます (最初にメソッドを宣言したインターフェイス型でオーバーライド宣言内の名前を修飾し、アクセス修飾子を省略します)。

インターフェイス内の `virtual` 関数メンバーは、派生インターフェイスで明示的に (暗黙的にではなく) オーバーライドできます。また、`public` されていないメンバーは、明示的に (暗黙的にではなく) クラスまたは構造体にのみ実装できます。 どちらの場合も、オーバーライドまたは実装されたメンバーは、オーバーライドされた場所に*アクセス*できる必要があります。

### <a name="reabstraction"></a>再抽象化

インターフェイスで宣言された仮想 (具象) メソッドは、派生インターフェイスで抽象になるようにオーバーライドできます。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

`abstract` 修飾子は、`IB.M` の宣言 (インターフェイスの既定値) には必要ありませんが、オーバーライド宣言では明示的にすることをお勧めします。

これは、メソッドの既定の実装が不適切で、実装クラスによってより適切な実装を提供する必要がある派生インターフェイスで役立ちます。

> ***懸案事項を開く:*** 再抽象化を許可する必要がありますか。

### <a name="the-most-specific-override-rule"></a>最も限定的な上書きルール

すべてのインターフェイスとクラスは、型またはその直接および間接インターフェイスに表示されるオーバーライドのすべての仮想メンバーに対して、*最も限定的なオーバーライド*を持つ必要があります。 *最も具体的*なオーバーライドは、他のすべての上書きよりも固有の一意のオーバーライドです。 オーバーライドがない場合、メンバー自体は最も具体的なオーバーライドと見なされます。

`M1` が型 `T1`で宣言されていて、`M2` が型 `T2`で宣言されている場合は、1つのオーバーライド `M1` が別のオーバーライドよりも*具体的*に `M2` と見なされます。

1. `T1` には、直接または間接のインターフェイスの `T2` が含まれています。
2. `T2` はインターフェイス型ですが、`T1` はインターフェイス型ではありません。

次に例を示します。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

最も限定的なオーバーライド規則では、競合が発生した時点でプログラマが明示的に競合を解決します (つまり、ダイヤモンドの継承によって生じるあいまいさが生じます)。

インターフェイスでは明示的な抽象オーバーライドがサポートされているため、クラスでもこれを行うことができます。

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> ***懸案事項を開く***: クラスの明示的なインターフェイス抽象オーバーライドをサポートする必要がありますか。

さらに、クラス宣言内で、一部のインターフェイスメソッドの最も限定的なオーバーライドが、インターフェイスで宣言された抽象オーバーライドである場合は、エラーになります。 これは、新しい用語を使用した既存のルール反映先です。

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

インターフェイスで宣言された仮想プロパティは、1つのインターフェイスで `get` アクセサーに対して最も限定的なオーバーライドを持つことができます。また、別のインターフェイスの `set` アクセサーに対して最も具体的なオーバーライドを行うこともできます。 これは、*最も限定的な上書き*ルールの違反と見なされます。

### <a name="static-and-private-methods"></a>`static` メソッドおよび `private` メソッド

インターフェイスには実行可能コードが含まれている可能性があるため、共通のコードをプライベートメソッドと静的メソッドに抽象すると便利です。 これらはインターフェイスで許可されるようになりました。

> ***終了***済みの問題: プライベートメソッドをサポートする必要がありますか。 静的メソッドをサポートする必要がありますか。 **決定: はい**

> ***懸案事項を開く***: インターフェイスメソッドが `protected` または `internal` またはその他のアクセスを許可する必要がありますか。 その場合、どのような意味があるのでしょうか。 既定では `virtual` ますか? そうでない場合、非仮想化を行う方法はありますか。

> ***未解決の問題***: 静的メソッドをサポートしている場合は、(静的) 演算子をサポートする必要がありますか。

### <a name="base-interface-invocations"></a>基本インターフェイスの呼び出し

既定のメソッドを使用してインターフェイスから派生した型のコードでは、そのインターフェイスの "基本" 実装を明示的に呼び出すことができます。

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

インスタンス (非静的) メソッドは、`base(Type).M`構文を使用して名前を付けることによって、直接基底インターフェイスのアクセス可能なインスタンスメソッドの実装を呼び出すことができます。 これは、ダイヤモンド継承によって提供される必要があるオーバーライドが、1つの特定の基本実装に委任することによって解決される場合に便利です。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

構文 `base(Type).M`を使用して `virtual` または `abstract` のメンバーにアクセスする場合、`Type` には `M`に対して*最も固有*の一意のオーバーライドが含まれている必要があります。

### <a name="binding-base-clauses"></a>基本句のバインド

インターフェイスに型が含まれるようになりました。  これらの型は、基本のインターフェイスとしてベース句で使用できます。  ベース句をバインドするときに、これらの型をバインドする基本インターフェイスのセットを知る必要がある場合があります (たとえば、参照して保護されたアクセスを解決する場合)。  そのため、インターフェイスのベース句の意味が循環的に定義されています。  サイクルを中断するには、クラスに対して既に設定されている同様の規則に対応する新しい言語規則を追加します。

インターフェイスの*interface_base*の意味を判断する際、基本インターフェイスは一時的に空であると見なされます。 直感的に言えば、ベース句の意味を再帰的に依存させることはできません。 

**ここでは、次の規則を使用しました。**

"クラス B がクラス A から派生した場合、が B に依存している場合は、コンパイル時にエラーが発生します。クラスは、直接基底クラス (存在する場合)**に依存**し、そのクラスがすぐに入れ子になっている~~**クラス**~~**に直接依存**します (存在する場合)。 この定義により、クラスが依存する~~**クラス**~~の完全なセットは、リレーションシップ**に直接依存**する再帰的および推移的なクロージャです。 "

インターフェイスがそれ自体から直接または間接的に継承する場合、コンパイル時エラーになります。
インターフェイスの**基本インターフェイス**は、明示的な基本インターフェイスとその基本インターフェイスです。 つまり、基本インターフェイスのセットは、明示的な基本インターフェイス、その明示的な基本インターフェイスなどの完全な推移的なクロージャです。

**次のように調整します。**

クラス B がクラス A から派生した場合、A が B に依存していると、コンパイル時にエラーになります。クラスは、直接の基底クラス (存在する場合)**に依存**し、すぐに入れ子になっている _**型**_ **に直接**依存します (存在する場合)。

インターフェイス IB によって IA が拡張されると、IA が IB に依存している場合、コンパイル時エラーになります。 インターフェイスは、直接の基本インターフェイス (存在する場合)**に依存**し、直接的に入れ子になっている型 (存在する場合)**に**依存します。

これらの定義を指定すると、型が依存する**型**の完全なセットは、リレーションシップ**に直接依存**します。

### <a name="effect-on-existing-programs"></a>既存のプログラムへの影響

ここに示すルールは、既存のプログラムの意味に影響を与えないことを目的としています。

例 1:

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

例 2:

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

同じ規則によって、既定のインターフェイスメソッドを含む同様の状況に似た結果が得られます。

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> ***終了***済みの問題: これが仕様の意図した結果であることを確認します。 **決定: はい**

### <a name="runtime-method-resolution"></a>ランタイムメソッドの解決

> ***終了済みの問題:*** この仕様では、インターフェイスの既定のメソッドの面でのランタイムメソッドの解決アルゴリズムを記述する必要があります。 セマンティクスと言語のセマンティクスが一致していることを確認する必要があります。たとえば、宣言されたメソッドが、`internal` メソッドをオーバーライドしたり実装したりしないようにします。

### <a name="clr-support-api"></a>CLR サポート API

コンパイラがこの機能をサポートするランタイムに対してコンパイルされているかどうかを検出するために、このようなランタイムのライブラリは <https://github.com/dotnet/corefx/issues/17116>で説明されている API を介してその事実を提供するように変更されます。 次のように追加します。

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> ***懸案事項を開く***: *CLR*機能の最適な名前 CLR 機能は、それだけではありません (たとえば、緩和されの保護制約、インターフェイスでのオーバーライドのサポートなど)。 "インターフェイス内の具象メソッド" や "特徴" などを呼び出す必要があるかもしれません。

### <a name="further-areas-to-be-specified"></a>指定する領域の追加

- [] 既定のインターフェイスメソッドとオーバーライドを既存のインターフェイスに追加することによって発生するソースとバイナリの互換性の影響の種類をカタログ化すると便利です。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

この提案には、CLR 仕様の調整された更新が必要です (インターフェイスとメソッドの解決法の具象メソッドをサポートするため)。 そのため、非常に "高額" なので、CLR の変更が必要になることも予想される他の機能と組み合わせて使用する価値があります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

[なし] :

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- 未解決の質問は、上記の提案の中でも呼び出されます。
- 開いている質問の一覧については、「<https://github.com/dotnet/csharplang/issues/406>」も参照してください。
- 詳細な仕様では、呼び出される正確なメソッドを選択するために、実行時に使用される解決機構を記述する必要があります。
- 新しいコンパイラによって生成され、以前のコンパイラによって使用されるメタデータの相互作用は、詳しく説明する必要があります。 たとえば、以前のコンパイラによってコンパイルされたときに、そのインターフェイスを実装する既存のクラスを中断するために、使用するメタデータ表現によってインターフェイスに既定の実装が追加されないようにする必要があります。 これは、使用できるメタデータ表現に影響を与える可能性があります。
- この設計では、他の言語と他の言語の既存のコンパイラとの相互運用を検討する必要があります。

## <a name="resolved-questions"></a>解決済みの質問

### <a name="abstract-override"></a>抽象オーバーライド

以前のドラフト仕様には、継承されたメソッドを "再 abstract" する機能が含まれていました。

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

2017-03-20 のメモでは、このことを許可しないことにしました。 ただし、少なくとも2つのユースケースがあります。

1. この機能の一部のユーザーが相互運用を期待する Java Api は、この機能に依存しています。
2. この点からの*特徴*によるプログラミング。 Reabstraction は、"特徴" 言語機能 (https://en.wikipedia.org/wiki/Trait_(computer_programming))の要素の1つです。 クラスでは、次のものを使用できます。

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

残念ながら、このコードは、許可されている場合を除き、インターフェイス (特徴) のセットとしてリファクタリングすることはできません。 このような場合は、 *jared の原則*により、これを許可する必要があります。

> ***終了済みの問題:*** 再抽象化を許可する必要がありますか。 イエスメモが間違っています。 [LDM note](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)は、インターフェイスで reabstraction が許可されていることを示しています。 クラスにありません。

### <a name="virtual-modifier-vs-sealed-modifier"></a>仮想修飾子と Sealed 修飾子

[Aleksey Tsingz](https://github.com/AlekseyTs)から:

> インターフェイスメンバーの一部を禁止する理由がない限り、修飾子を明示的に宣言することにしました。 これにより、仮想修飾子に関する興味深い質問が発生します。 既定の実装のメンバーで必要になるかどうかを確認します。
>
> 次のようなことが考えられます。
>
> - 実装がなく、仮想と sealed のどちらも指定されていない場合、メンバーは abstract であると想定されます。
> - 実装があり、abstract と sealed のどちらも指定されていない場合は、メンバーが virtual であると想定します。
> - シール修飾子は、メソッドを仮想でも abstract にもできないようにするために必要です。
>
> または、仮想メンバーに仮想修飾子が必要であるということも考えられます。 つまり、仮想修飾子で明示的にマークされていない実装を持つメンバーがある場合は、仮想でも抽象でもありません。 この方法では、メソッドをクラスからインターフェイスに移動したときに、より優れたエクスペリエンスが得られる可能性があります。
>
> - 抽象メソッドは抽象のままです。
> - 仮想メソッドは仮想のままです。
> - 修飾子のないメソッドは、仮想でも abstract にもとどまりません。
> - シールされた修飾子は、オーバーライドではないメソッドには適用できません。
>
> どう思いますか。

> ***終了済みの問題:*** 具象メソッド (実装を含む) を暗黙的に `virtual`する必要がありますか。 イエス

***決定:*** LDM 2017-04-05 で作成されました。

1. 非仮想は、`sealed` または `private`を通じて明示的に表現する必要があります。
2. `sealed` は、インターフェイスインスタンスのメンバーを非仮想ボディで作成するためのキーワードです。
3. インターフェイス内のすべての修飾子を許可します。  
4. インターフェイスメンバーの既定のアクセシビリティは、入れ子になった型を含むパブリックです。
5. インターフェイスのプライベート関数メンバーは暗黙的にシールされているため、`sealed` は使用できません。
6. プライベートクラス (インターフェイス内) は許可されており、シールすることができます。これは、シールされたクラスの意味でシールされていることを意味します。
7. 適切な提案がない場合でも、インターフェイスまたはメンバーでは部分的な使用は許可されません。

### <a name="binary-compatibility-1"></a>バイナリ互換性1

ライブラリが既定の実装を提供する場合

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

`C` での `I1.M` の実装は `I1.M`ことを理解しています。 `I2` を含むアセンブリが次のように変更され、再コンパイルされた場合

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

ただし `C` は再コンパイルされません。 プログラムを実行するとどうなりますか。 `(C as I1).M()` の呼び出し

1. 実行 `I1.M`
2. 実行 `I2.M`
3. 何らかの種類のランタイムエラーをスローします。

***決定:*** 2017-04-11: 実行時に明確に最も限定的なオーバーライドである `I2.M`を実行します。

### <a name="event-accessors-closed"></a>イベントアクセサー (終了)

> ***終了済みの問題:***"区分" というイベントを上書きできますか。

次の場合を考えてみましょう。

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

このイベントの "部分的な" 実装は許可されていません。これは、クラスのように、イベント宣言の構文ではアクセサーが1つしか許可されていないためです。両方 (またはどちらも) を指定する必要があります。 次のように、構文の abstract remove アクセサーを、本文がないことによって暗黙的に抽象化することを許可することで、同じことを実現できます。

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

*これは新しい (提案された) 構文*であることに注意してください。 現在の文法では、イベントアクセサーには必須の本文があります。

> ***終了済みの問題:*** イベントアクセサーは、本体の省略によって (暗黙的に) 抽象化できます。これは、インターフェイスとプロパティアクセサーのメソッドが (暗黙的に) 本体の省略によって抽象化される方法と同じです。

***決定:*** (2017-04-18) いいえ。イベント宣言には、具象アクセサー (またはその両方) が必要です。

### <a name="reabstraction-in-a-class-closed"></a>クラス (closed) での再抽象化

***終了済みの問題:*** これが許可されていることを確認する必要があります (それ以外の場合、既定の実装を追加すると、互換性に影響する変更になります)。

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

***決定:*** (2017-04-18) はい。インターフェイスメンバーの宣言に本体を追加して、C を中断することはできません。

### <a name="sealed-override-closed"></a>シールされたオーバーライド (終了)

前の質問では、`sealed` 修飾子をインターフェイスの `override` に適用できることを暗黙的に想定しています。 これはドラフト仕様と矛盾しています。 上書きの封印を許可しますか? 封印のソースとバイナリの互換性の影響を考慮する必要があります。

> ***終了済みの問題:*** 上書きの封印を許可する必要がありますか。

***決定:*** (2017-04-18) インターフェイスのオーバーライドで `sealed` できないようにします。 インターフェイスメンバーに対して `sealed` を使用する唯一の方法は、初期宣言で非仮想にすることです。

### <a name="diamond-inheritance-and-classes-closed"></a>ダイヤモンド継承とクラス (閉じている)

提案のドラフトでは、ひし形の継承シナリオで、クラスがインターフェイスのオーバーライドに優先されます。

> すべてのインターフェイスとクラスは、型またはその直接および間接インターフェイスに表示されるオーバーライドのすべてのインターフェイスメソッドに対して、*最も限定的なオーバーライド*を持つ必要があります。 *最も具体的*なオーバーライドは、他のすべての上書きよりも固有の一意のオーバーライドです。 オーバーライドがない場合、メソッド自体は最も限定的なオーバーライドと見なされます。
>
> `M1` が型 `T1`で宣言されていて、`M2` が型 `T2`で宣言されている場合は、1つのオーバーライド `M1` が別のオーバーライドよりも*具体的*に `M2` と見なされます。
>
> 1. `T1` には、直接または間接のインターフェイスの `T2` が含まれています。
> 2. `T2` はインターフェイス型ですが、`T1` はインターフェイス型ではありません。

このシナリオは次のようになります。

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

この動作を確認する必要があります (または他の方法を決定します)。

> ***終了済みの問題:*** 上記のドラフト仕様を、混合クラスおよびインターフェイス (クラスはインターフェイスよりも優先されます) に適用される*ほとんどの特定のオーバーライド*について確認します。 [https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>) をご覧ください。

### <a name="interface-methods-vs-structs-closed"></a>インターフェイスメソッドと構造体 (closed)

既定のインターフェイスメソッドと構造体の間には、よくある相互作用があります。

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

インターフェイスメンバーは継承されないことに注意してください。

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

そのため、クライアントは、インターフェイスメソッドを呼び出すように構造体をボックスにする必要があります。

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

このようにボックス化すると、`struct` の種類の主な利点が損なわれます。 さらに、変異メソッドは、構造体のボックス化された*コピー*で動作しているため、明らかに効果がありません。

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> ***終了済みの問題:*** これについては、次のことを行うことができます。
>
> 1. `struct` が既定の実装を継承することを禁止します。 すべてのインターフェイスメソッドは、`struct`で abstract として扱われます。 その後、後でより適切に動作させる方法を決定するために時間がかかることがあります。
> 2. ボックス化を回避する何らかの種類のコード生成方法を考えてください。 `IB.Increment`のようなメソッドの内部では、`this` の型は `IB`に制約された型パラメーターに類似している可能性があります。 これと共に、呼び出し元でのボックス化を避けるために、非抽象メソッドはインターフェイスから継承されます。 これにより、コンパイラと CLR の実装が大幅に増加する可能性があります。
> 3. 心配せず、wart として残してください。
> 4. その他のアイデア

***決定:*** 心配せず、wart として残してください。 [https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>) をご覧ください。

### <a name="base-interface-invocations-closed"></a>基本インターフェイス呼び出し (closed)

ドラフト仕様は、Java: `Interface.base.M()`によって使用される基本インターフェイスの呼び出しの構文を提案します。 少なくとも最初のプロトタイプについて、構文を選択する必要があります。 お気に入りは `base<Interface>.M()`。

> ***終了済みの問題:*** 基本メンバー呼び出しの構文は何ですか。

***決定:*** 構文は `base(Interface).M()`です。 [https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>) をご覧ください。 このため、という名前のインターフェイスは基本インターフェイスである必要がありますが、直接の基本インターフェイスである必要はありません。

> ***懸案事項を開く:*** 基底インターフェイスの呼び出しをクラスメンバーで許可する必要がありますか。

***決定***: はい。 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a>パブリックでないインターフェイスメンバーのオーバーライド (closed)

インターフェイスでは、基本インターフェイスの非パブリックメンバーは `override` 修飾子を使用してオーバーライドされます。 メンバーを含むインターフェイスに名前を明示的にオーバーライドする場合、アクセス修飾子は省略されます。

> ***終了済みの問題:*** インターフェイスに名前が付けられていない "暗黙" のオーバーライドである場合、アクセス修飾子は一致している必要がありますか。

***決定:*** 暗黙的にオーバーライドできるのはパブリックメンバーだけで、アクセスは一致している必要があります。 [https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>) をご覧ください。

> ***懸案事項を開く:***`override void IB.M() {}`などの明示的なオーバーライドでは、アクセス修飾子が必須、省略可能、または省略されているかどうかを示します。

> ***懸案事項を開く:***`void IB.M() {}`などの明示的なオーバーライドでは `override` 必須、省略可能、または省略されていますか?

1つは、クラスにパブリックでないインターフェイスメンバーを実装する方法ですか。 明示的に実行する必要があるかもしれません。

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> ***終了済みの問題:*** 1つは、クラスにパブリックでないインターフェイスメンバーを実装する方法ですか。

***決定:*** 非パブリックインターフェイスメンバーを明示的に実装することはできません。 [https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>) をご覧ください。

***Decision***: インターフェイスメンバーに `override` キーワードは使用できません。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a>バイナリ互換性 2 (終了)

それぞれの型が別のアセンブリにある次のコードについて考えてみます。

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

`C` での `I1.M` の実装は `I2.M`ことを理解しています。 `I3` を含むアセンブリが次のように変更され、再コンパイルされた場合

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

ただし `C` は再コンパイルされません。 プログラムを実行するとどうなりますか。 `(C as I1).M()` の呼び出し

1. 実行 `I1.M`
2. 実行 `I2.M`
3. 実行 `I3.M`
4. 2または 3 (決定的)
5. 何らかの種類のランタイム例外をスローします。

***Decision***: 例外をスローします (5)。 [https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>) をご覧ください。

### <a name="permit-partial-in-interface-closed"></a>インターフェイスで `partial` を許可しますか? 開閉

抽象クラスの使用方法に似た方法でインターフェイスが使用されている場合は、それらを `partial`宣言すると便利な場合があります。 これは、ジェネレーターの面で特に便利です。

> ***提案:*** インターフェイスおよびインターフェイスのメンバーが `partial`として宣言されていない可能性のある言語制限を削除します。

***決定***: はい。 [https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>) をご覧ください。

### <a name="main-in-an-interface-closed"></a>インターフェイスで `Main` しますか? 開閉

> ***懸案事項を開く:*** インターフェイス内の `static Main` メソッドは、プログラムのエントリポイントになる候補ですか。

***決定***: はい。 [https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>) をご覧ください。

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a>パブリックな非仮想メソッドをサポートすることを確認する (終了)

インターフェイスで非仮想パブリックメソッドを許可するかどうかを確認 (または反転) できますか。

```csharp
interface IA
{
    public sealed void M() { }
}
```

> ***半終了した問題:*** (2017-04-18) は役に立つと思われますが、それに戻ります。 これは、メンタルモデルのトリップブロックです。

***決定***: はい。 [https://login.microsoftonline.com/consumers/](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>)

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a>インターフェイス内の `override` は新しいメンバーを導入しますか。 開閉

オーバーライド宣言に新しいメンバーが導入されているかどうかを確認するには、いくつかの方法があります。

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> ***懸案事項を開く:*** インターフェイス内のオーバーライド宣言によって新しいメンバーが導入されるか。 開閉

クラスでは、オーバーライドするメソッドはいくつかの意味で "visible" になります。 たとえば、パラメーターの名前は、オーバーライドされたメソッドのパラメーターの名前よりも優先されます。 常に最も限定的なオーバーライドがあるため、インターフェイスでその動作を複製できる可能性があります。 しかし、その動作を複製する必要はありますか。

また、オーバーライドメソッドを "オーバーライド" できますか。 議論

***Decision***: インターフェイスメンバーに `override` キーワードは使用できません。 [https://login.microsoftonline.com/consumers/](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>)

### <a name="properties-with-a-private-accessor-closed"></a>プライベートアクセサーを持つプロパティ (closed)

プライベートメンバーは仮想ではなく、仮想とプライベートの組み合わせは許可されていません。 しかし、プライベートアクセサーを持つプロパティについてはどうでしょうか。

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

これは可能ですか? ここでは `set` アクセサー `virtual` ですか? アクセス可能な場所で上書きできますか。 次のは、暗黙的に `get` アクセサーのみを実装しますか。

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

IA が原因で、次のようなエラーが発生することがあります。P. set は仮想ではなく、アクセスできないためにも設定されていますか。

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

***Decision***: 最初の例は有効ですが、最後の例は有効ではありません。 これは、でC#既に動作していると同様に解決されます。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a>基本インターフェイス呼び出し、round 2 (closed)

基本呼び出しを処理する方法に対する以前の "解決策" は、実際には十分な表現力を提供しません。 Java とは異なり、 C#と CLR では、メソッド宣言を含むインターフェイスと呼び出す実装の場所の両方を指定する必要があります。

ここでは、インターフェイスの基本呼び出しに対して次の構文を提案します。 私は気に入っていませんが、どのような構文を表現できる必要があるかを示しています。

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

あいまいさがない場合は、簡単に記述できます。

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

または

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

または

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

***Decision***: `base(N.I1<T>).M(s)`に対して決定されます。呼び出しバインドがある場合は、後で問題が発生する可能性があります。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a>構造体が既定のメソッドを実装していない場合の警告 開閉

@vancem アサートするには、インターフェイスからメソッドの実装を継承する場合でも、値型の宣言がインターフェイスメソッドのオーバーライドに失敗した場合に警告を生成することを真剣に検討する必要があります。 これは、ボックス化と損なうの制約付き呼び出しを引き起こすためです。

***決定***: これは、アナライザーにより適しているように見えます。 また、この警告は、既定のインターフェイスメソッドが呼び出されず、ボックス化が行われない場合でも起動されるため、この警告が発生する可能性があります。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a>インターフェイス静的コンストラクター (closed)

インターフェイスの静的コンストラクターを実行する場合  現在の CLI ドラフトでは、最初の静的メソッドまたはフィールドにアクセスしたときに発生することが提案されています。 これらのいずれも存在しない場合、実行されない可能性がありますか?

[2018-10-09 CLR チームは、valuetypes に対して行うことをミラー化する (各インスタンスメソッドへのアクセスに対しては、.cctor チェック) を提案します。]

***Decision***: 静的コンストラクターは、インスタンスメソッドへのエントリでも実行されます。この場合、静的コンストラクターが `beforefieldinit`されていないと、最初の静的フィールドにアクセスする前に静的コンストラクターが実行されます。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a>会議のデザイン

[2017-03-08 Ldm 会議ノート](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 ldm のノート](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 Meeting "既定のインターフェイスメソッドに対する CLR の動作"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 Ldm 会議のメモ](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 ldm 会議](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)の[メモ
2017-04-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) LDM 会議のメモ
2017-04-19 [ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)会議のメモ
[2017-05-17 ldm 会議のメモ](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 ldm のメモ](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
2017-06-14 [ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-11-14 ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)会議メモの会議ノート
[2018-10-17 ldm 会議](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)ノート
