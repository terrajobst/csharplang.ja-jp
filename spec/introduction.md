# <a name="introduction"></a>はじめに

C# ("シー シャープ" と読みます) は、シンプルで最新のタイプ セーフなオブジェクト指向のプログラミング言語です。 C# で C ファミリ言語のおよび C、C++、および Java のプログラマになじみがすぐになります。 C# として ECMA international 標準化されて、 ***ecma-334***標準および ISO/IEC としてによって、 ***ISO/IEC 23270***標準。 .NET Framework 用の Microsoft の c# コンパイラは、これらの標準の両方の準拠の実装です。

C# はオブジェクト指向言語ですが、C# にはさらに "***コンポーネント指向***" プログラミングのサポートが含まれています。 最近のソフトウェア設計では、機能を自己完結型および自己記述型パッケージの形式にしたソフトウェア コンポーネントをますます使用するようになっています。 そのようなコンポーネントの鍵となるのは、プロパティ、メソッド、イベントを使用してプログラミング モデルを表すこと、コンポーネントについての宣言的な情報を提供する属性があること、独自のドキュメントが組み込まれていることです。 C# では、これらの概念を直接サポートする言語コンストラクトを行う (C#) を作成し、ソフトウェア コンポーネントを使用するための非常に自然言語。

堅牢で永続的なアプリケーションの構築を支援するいくつかの c# 機能。***ガベージ コレクション***自動的に使用されていないオブジェクトによって占有されているメモリを解放します。***例外処理***エラーの検出と復旧に構造化された拡張可能なアプローチを提供し、***タイプ セーフ***言語の設計では、初期化されていない変数を読み取ったりすることは不可能インデックスには、配列の境界を超える、またはオフを実行するは、キャストを入力します。

C# は "***統合型システム***" を採用しています。 `int` や `double` などのプリミティブ型を含めた C# のすべての型は、ルートとなる 1 つの `object` 型から派生しています。 したがって、すべての型が一般的な操作のセットを共有し、すべての型の値を一貫した方法で格納、転送、操作することができます。 さらに、C# はユーザー定義の参照型と値型の両方をサポートしているため、オブジェクトを動的に割り当てることも、軽量の構造体をインラインで格納することもできます。

C# のプログラムとライブラリを互換性のある方法での時間とともに進化できることを確認するに大きな重点が置かれた***バージョン管理***# の設計にします。 多くのプログラミング言語は、この問題にほとんど注意を払っていません。その結果、それらの言語で書かれたプログラムは、依存するライブラリの新しいバージョンが導入されたときに必要以上に頻繁に中断されてしまいます。 バージョン管理に関する考慮事項の影響を受ける直接 # の設計の側面は、個別の`virtual`と`override`修飾子、メソッドのオーバー ロードの解決の規則と明示的なインターフェイス メンバー宣言をサポートします。

この章の残りの部分では、c# 言語の重要な機能について説明します。 以降の章では、ルールと例外を説明について詳細に、場合によっては数学的に、この章はわかりやすくするため、完全性を犠牲に簡潔にするための努力しています。 目的は、初期プログラムの作成と以降の章の読み取りを容易にする言語を紹介したリーダーを提供します。

## <a name="hello-world"></a>Hello world

"Hello, World" は、プログラミング言語を紹介するために伝統的に使用されているプログラムです。 これを C# で記述すると次のようになります。

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

通常、C# のソース ファイルのファイル拡張子は `.cs` です。 「こんにちは, World」プログラムがファイルに格納されていると仮定`hello.cs`プログラムは、コマンドラインを使用して、Microsoft c# コンパイラでコンパイルすることができます
```
csc hello.cs
```
という名前の実行可能アセンブリを生成する`hello.exe`します。 このアプリケーションは、実行時に生成される出力は、します。
```
Hello, World
```

"Hello, World" プログラムは `System` 名前空間を参照する `using` ディレクティブで始まります。 名前空間は、C# のプログラムとライブラリを階層的に整理するための手段です。 名前空間には、型と他の名前空間が含まれます。たとえば、`System` 名前空間には多数の型 (プログラムで参照される `Console` クラスなど) と、他の多数の名前空間 (`IO` や `Collections` など) が含まれます。 特定の名前空間を参照する `using` ディレクティブを使用すると、その名前空間のメンバーである型を修飾せずに使用できます。 `using` ディレクティブにより、プログラムで `Console.WriteLine` を `System.Console.WriteLine` の省略形として使用できます。

"Hello, World" プログラムで宣言された `Hello` クラスにはメンバーが 1 つあります。`Main` という名前のメソッドです。 `Main`でメソッドが宣言された、`static`修飾子。 インスタンス メソッドが `this` で囲んだ特定のオブジェクト インスタンスを参照できるのに対し、静的メソッドは特定のオブジェクトを参照せずに機能します。 規則により、`Main` という名前の静的メソッドはプログラムのエントリ ポイントとして使用されます。

プログラムの出力は、`System` 名前空間にある `Console` クラスの `WriteLine` メソッドによって生成されます。 このクラスは、既定では、Microsoft c# コンパイラでは参照自動的にされている .NET Framework クラス ライブラリによって提供されます。 C# 自体がないこと、個別のランタイム ライブラリに注意してください。 代わりに、.NET Framework は、c# のランタイム ライブラリです。

## <a name="program-structure"></a>プログラムの構造

C# における主要な組織的概念は、***プログラム***、***名前空間***、***型***、***メンバー***、および***アセンブリ***です。 C# プログラムは、1 つまたは複数のソース ファイルで構成されています。 プログラムは型を宣言します。型にはメンバーが含まれていて、複数の名前空間に編成することができます。 型の例には、クラスおよびインターフェイスがあります。 メンバーの例には、フィールド、メソッド、プロパティ、およびイベントがあります。 C# プログラムはコンパイルされると、物理的にアセンブリにパッケージ化されます。 アセンブリは通常、ファイル拡張子を持つ`.exe`または`.dll`を実装するかに応じて、***アプリケーション***または***ライブラリ***します。

例では、

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
という名前のクラスを宣言します`Stack`という名前空間で`Acme.Collections`します。 このクラスの完全修飾名は `Acme.Collections.Stack` です。 このクラスには複数のメンバーが含まれています: `top` という名前のフィールドが 1 つ、`Push` と `Pop` という名前のメソッドが合わせて 2 つ、そして `Entry` という名前の入れ子になったクラスです。 `Entry` クラスにはさらに、3 つのメンバーが含まれています: `next` という名前のフィールド、`data` という名前のフィールド、およびコンストラクターです。 例のソース コードが `acme.cs` のファイルに保存されていることを前提に、次のコマンド ラインをご覧ください。

```
csc /t:library acme.cs
```
このコマンド ラインは例をライブラリ (`Main` エントリ ポイントがないコード) としてコンパイルし、`acme.dll` という名前のアセンブリを生成します。

アセンブリの形式で実行可能コードを含めることが***中間言語***(IL) の手順、およびシンボル情報の形式で***メタデータ***します。 アセンブリの IL コードは実行前に、.NET 共通言語ランタイムの Just-In-Time (JIT) コンパイラによって、プロセッサ固有のコードに自動的に変換されます。

アセンブリはコードとメタデータの両方を含む自己記述的な機能的単位であるため、`#include` ディレクティブおよびヘッダー ファイルが C# に含まれている必要はありません。 特定のアセンブリに含まれているパブリックの型とメンバーは、単にプログラムのコンパイル中にそのアセンブリを参照することにより、C# プログラムで利用可能になります。 たとえば、このプログラムでは `acme.dll` アセンブリの `Acme.Collections.Stack` クラスを使用しています。

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
プログラムがファイルに格納されている場合`test.cs`ときに、`test.cs`がコンパイルされる、`acme.dll`コンパイラを使用してアセンブリを参照できる`/r`オプション。

```
csc /r:acme.dll test.cs
```
これにより `test.exe` という名前の実行可能なアセンブリが作成され、これが実行された場合に、次の出力が生成されます。

```
100
10
1
```
C# では、プログラムのソース テキストを複数のソース ファイルに保存することができます。 複数ファイルの C# プログラムがコンパイルされると、ソース ファイルはすべて同時に処理され、ソース ファイルは自由に相互を参照できるようになります。概念的には、ソース ファイルが処理される前に、すべて 1 つの大きいファイルに連結されるようなものです。 C# では事前宣言をする必要がありません。ごく一部の例外を除いて、宣言の順序は重要でないためです。 C# ではソース ファイルがパブリック型 1 つのみの宣言に制限されません。また、ソース ファイルの名前がソース ファイルで宣言された型に一致する必要もありません。

## <a name="types-and-variables"></a>型と変数

C# には、***値型***と***参照型***という 2 種類の型があります。 値型の変数が直接データを格納するのに対して、参照型の変数はデータへの参照を格納し、後者はオブジェクトとして知られています。 参照型を使用すると 2 つの変数が同じオブジェクトを参照できるため、1 つの変数に対する演算によって、もう一方の変数によって参照されるオブジェクトに影響を与えることができます。 値型の場合、各変数が独自のデータ コピーを保持し、1 つの変数に対する演算で別の変数に影響を与えることはできません (`ref` と `out` のパラメーターの変数の場合を除く)。

# の値の型はさらに分割***単純型***、***列挙型***、***構造体型***、および***null 許容型***と # のリファレンス型はさらに分割***クラス型***、***インターフェイス型***、***配列型***、および***デリゲート型***します。

次の表では、# の型システムの概要を示します。

| __カテゴリ__    |                 | __説明__ |
|-----------------|-----------------|-----------------|
| 値型     | 単純型    | 符号付きの整数: `sbyte`、`short`、`int`、`long` |
|                 |                 | 符号なしの整数: `byte`、`ushort`、`uint`、`ulong` |
|                 |                 | Unicode 文字: `char` |
|                 |                 | IEEE 浮動小数点: `float`、`double` |
|                 |                 | 高精度の 10 進数: `decimal` |
|                 |                 | ブール値: `bool` |
|                 | 列挙型      | `enum E {...}` 形式のユーザー定義型 |
|                 | 構造体の型    | `struct S {...}` 形式のユーザー定義型 |
|                 | Null 許容型  | `null` 値を持つその他すべての値型の拡張子 |
| 参照型 | クラス型     | その他すべての型の最終的な基底クラス: `object` |
|                 |                 | Unicode 文字列: `string` |
|                 |                 | `class C {...}` 形式のユーザー定義型 |
|                 | インターフェイス型 | `interface I {...}` 形式のユーザー定義型 |
|                 | 配列型     | 1 次元または多次元、たとえば `int[]` および `int[,]` |
|                 | デリゲート型  | ユーザー定義型形式の例。 `delegate int  D(...)` |

8 つの整数型は、符号付きまたは符号なしの形式で、8 ビット、16 ビット、32 ビットおよび 64 ビットの値をサポートします。

2 つの浮動小数点型、`float`と`double`、32 ビット単精度と 64 ビット倍精度の IEEE 754 形式を使用して表されます。

`decimal` 型は 128 ビットのデータ型で、財務や通貨の計算に適しています。

# の`bool`型がブール値を表すため、値では、`true`または`false`。

C# における文字および文字列の処理では、Unicode エンコーディングを使用します。 `char` 型は UTF-16 コード単位を表し、`string` 型は一連の UTF-16 コード単位を表します。

次の表では、# の数値型をまとめたものです。


| __カテゴリ__      | __Bits__ | __Type__  | __範囲/有効桁数__ |
|-------------------|----------|-----------|---------------------|
| 符号付き整数   | 8        | `sbyte`   | ...-128 から 127 |
|                   | 16       | `short`   | -32、768... 32、767 |
|                   | 32       | `int`     | -2,147,483、648... 2, 147、483, 647 |
|                   | 64       | `long`    | -9,223,372,036,854,775、808... 9、223、372、036、854、775、807 |
| 符号なしの整数 | 8        | `byte`    | 0... 255 |
|                   | 16       | `ushort`  | 0... 65,535 |
|                   | 32       | `uint`    | 0... 4,294,967,295 |
|                   | 64       | `ulong`   | 0... 18,446,744,073,709,551,615 |
| 浮動小数点数    | 32       | `float`   | 1.5 × 10 ^ − 45 3.4 ~ 10 ^38、7 桁の有効桁数 |
|                   | 64       | `double`  | 5.0 × 10 ^ − 324 1.7 × 10 ^308、15 桁の有効桁数 |
| Decimal (10 進数型)           | 128      | `decimal` | 1.0 × 10 ^ − 28 7.9 × 10 ^28、28 桁の有効桁数 |

C# プログラムでは***型宣言***を使用して新しい型を作成します。 型宣言は、新しい型の名前とメンバーを指定します。 # の種類のカテゴリの 5 つはユーザー定義可能。 クラス型、構造体型、インターフェイス型、列挙型、およびデリゲート型。

クラス型では、データ メンバー (フィールド) および関数メンバー (メソッド、プロパティ、およびその他のユーザー) を格納するデータ構造体を定義します。 クラス型では、単一継承とポリモーフィズムをサポートします。このメカニズムによって派生クラスが基底クラスを拡張して特殊化できます。

構造体の型は、データ メンバーおよび関数メンバーを持つ構造体を表す点において、クラス型に似ています。 ただし、クラスと異なり、構造体は値型で、ヒープ割り当ては必要ありません。 構造体型はユーザー指定の継承をサポートせず、すべての構造体型は暗黙的に `object` 型を継承します。

インターフェイス型では、パブリック関数メンバーの名前付きセットとしてコントラクトを定義します。 クラスまたは構造体、インターフェイスを実装するには、インターフェイスの関数メンバーの実装を提供する必要があります。 複数の基底インターフェイスから継承でき、クラスまたは構造体には、複数のインターフェイスを実装できます。

デリゲート型では、特定のパラメーター リストおよび戻り値の型を持つメソッドへの参照を表します。 デリゲートを使用すると、変数に割り当ててパラメーターとして渡すことのできるエンティティとして、メソッドを処理できます。 デリゲートはまた、他のいくつかの言語にみられる関数ポインターの概念に似ていますが、関数ポインターとは異なり、デリゲートはオブジェクト指向でタイプ セーフです。

クラス、構造体、インターフェイスおよびデリゲートの型パラメーターが他の種類とそれにより、すべてのジェネリックのサポート。

列挙型は、名前付き定数を使用して別個の型です。 すべての列挙型が、基になる型、8 つの整数型のいずれかを指定する必要があります。 列挙型の値のセットでは、基になる型の値のセットと同じです。

C# は、あらゆる型の 1 次元および多次元の配列をサポートしています。 上記の型とは異なり、配列型は使用前に宣言する必要がありません。 代わりに配列型は、角かっこで囲んだ型名を後に付けることにより構成されます。 たとえば、`int[]`の 1 次元配列です`int`、`int[,]`の 2 次元の配列は、`int`と`int[][]`の 1 次元配列の 1 次元配列は、 `int`。

Null 許容型を使用する前に宣言することもはありません。 Null 非許容値型ごとに`T`対応する null 許容型がある`T?`、追加の値を格納する`null`します。 たとえば、`int?`は任意の 32 ビット整数または値が保持できる型です。`null`します。

# の型システムは、任意の型の値をオブジェクトとして処理できるように統合されます。 C# における型はすべて、直接的または間接的に `object` クラス型から派生し、`object` はすべての型の究極の基底クラスです。 参照型の値は、値を単純に `object` 型としてみなすことによってオブジェクトとして扱われます。 値型の値を実行することによってオブジェクトとして扱わ***ボックス化***と***ボックス化解除***操作。 次の例では、`int` 値は `object` 値に変換され、また `int` に戻されます。

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
値型の値を型に変換するときに`object`、「ボックス」とも呼ばれる、オブジェクト インスタンスが、値を保持するために割り当てられているし、そのボックスに値をコピーします。 逆に、ときに、`object`参照が値型にキャスト、参照先オブジェクトが、適切な値型のボックス、チェックが行われた、およびチェックが成功すると、ボックスの値はコピーします。

# の統一型システムを効果的に値の型"、オンデマンドで"オブジェクトになれること意味します。 こうした統一性があるため、`object` 型を使用する汎用的なライブラリは、参照型と値型の両方で使用できます。

C# には、フィールド、配列要素、ローカル変数、パラメーターなどの、いくつかの種類の***変数***があります。 変数は、記憶域の場所を表し、すべての変数ができる値を指定する型に次の表で示すように、変数に格納します。


| __変数の型__    | __考えられる内容__ |
|-------------------------|-----------------------|
| null 非許容値型 | 型そのものの値 |
| null 許容値型     | Null 値または正確な型の値 |
| `object`                | Null 参照、任意の参照型のオブジェクトへの参照または値型のボックス化された値への参照 |
| クラス型              | そのクラス型から派生した、null 参照、そのクラス型のインスタンスへの参照またはクラスのインスタンスへの参照 |
| インターフェイスの型          | Null 参照、そのインターフェイス型を実装するクラス型のインスタンスへの参照またはそのインターフェイス型を実装する値型のボックス化された値への参照 |
| 配列型              | Null 参照、その配列型のインスタンスへの参照、または互換性のある配列型のインスタンスへの参照 |
| デリゲート型           | Null 参照またはそのデリゲート型のインスタンスへの参照 |

## <a name="expressions"></a>式

***式***は、***オペランド***と***演算子***で構成されます。 式の演算子は、オペランドに適用する演算を表します。 演算子の例として、`+`、`-`、`*`、`/`、および `new` などがあります。 オペランドの例としては、リテラル、フィールド、ローカル変数、式などがあります。

複数の演算子を含む式の場合、演算子の***優先順位***によって各々の演算子が評価される順序が決定されます。 たとえば、式 `x + y * z` の評価は `x + (y * z)` ですが、これは `*` 演算子が `+` 演算子より高い優先順位だからです。

ほとんどの演算子は***オーバーロード***できます。 演算子をオーバーロードすると、ユーザー定義演算子の実装を、1 つまたは両方のオペランドがユーザー定義のクラスまたは構造体型である演算子に指定することができます。

次の表では、# の演算子を高いものから低い方への優先順位の順序で演算子のカテゴリを示します。 同じカテゴリの演算子は、同じ優先順位を持ちます。


| __カテゴリ__                     | __式__    | __説明__ |
|----------------------------------|-------------------|-----------------|
| 1 次式                          | `x.m`             | メンバー アクセス。 |
|                                  | `x(...)`          | メソッドおよびデリゲートの呼び出し。 |
|                                  | `x[...]`          | 配列アクセスおよびインデクサー アクセス。 |
|                                  | `x++`             | 後置インクリメント。 |
|                                  | `x--`             | 後置デクリメント。 |
|                                  | `new T(...)`      | オブジェクトおよびデリゲートの作成。 |
|                                  | `new T(...){...}` | 初期化子によるオブジェクト作成 |
|                                  | `new {...}`       | 匿名オブジェクト初期化子 |
|                                  | `new T[...]`      | 配列の作成 |
|                                  | `typeof(T)`       | 取得`System.Type`オブジェクト `T` |
|                                  | `checked(x)`      | checked コンテキストで式を評価します。 |
|                                  | `unchecked(x)`    | unchecked コンテキストで式を評価します。 |
|                                  | `default(T)`      | 型の既定値を取得します。 `T` |
|                                  | `delegate {...}`  | 匿名関数 (匿名メソッド)。 |
| 単項                            | `+x`              | 同一。 |
|                                  | `-x`              | 否定。 |
|                                  | `!x`              | 論理否定。 |
|                                  | `~x`              | ビットごとの否定。 |
|                                  | `++x`             | 前置インクリメント。 |
|                                  | `--x`             | 前置デクリメント。 |
|                                  | `(T)x`            | 明示的に変換`x`を入力するには `T` |
|                                  | `await x`         | 非同期に待機`x`を完了するには |
| 乗法                   | `x * y`           | 乗算 |
|                                  | `x / y`           | 除算記号 |
|                                  | `x % y`           | 剰余。 |
| 加法                         | `x + y`           | 加算、文字列の連結、デリゲートの組み合わせ。 |
|                                  | `x - y`           | 減算、デリゲートの削除。 |
| シフト                            | `x << y`          | 左シフト。 |
|                                  | `x >> y`          | 右シフト。 |
| 関係式と型検査      | `x < y`           | より小さい |
|                                  | `x > y`           | 次の値より大きい |
|                                  | `x <= y`          | 以下 |
|                                  | `x >= y`          | 以上 |
|                                  | `x is T`          | 返す`true`場合`x`は、 `T`、`false`それ以外の場合 |
|                                  | `x as T`          | 返す`x`として型指定された`T`、または`null`場合`x`されませんが、 `T` |
| 等価比較                         | `x == y`          | 等しい      |
|                                  | `x != y`          | 等しくない |
| 論理 AND                      | `x & y`           | 整数のビットごとの AND、ブール型の論理 AND |
| 論理 XOR                      | `x ^ y`           | 整数のビットごとの XOR、ブール型の論理 XOR。 |
| 論理 OR                       | <code>x &#124; y</code> | 整数のビットごとの OR、ブール型の論理 OR。 |
| 条件 AND                  | `x && y`          | 評価`y`場合にのみ`x`は `true` |
| 条件 OR                   | <code>x &#124;&#124; y</code> | 評価`y`場合にのみ`x`は `false` |
| Null 合体演算子                  | `X ?? y`          | 評価される`y`場合`x`は`null`を`x`それ以外の場合 |
| 条件                      | `x ? y : z`       | 評価`y`場合`x`は`true`、`z`場合`x`は `false` |
| 代入または匿名関数 | `x = y`           | 代入 |
|                                  | `x op= y`         | 複合代入。サポートされている演算子は、します。 `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code> |
|                                  | `(T x) => y`      | 匿名関数 (ラムダ式) |

## <a name="statements"></a>ステートメント

プログラムの処理は、"***ステートメント***" を使用して表されます。 C# はさまざまな種類のステートメントをサポートしており、その多くは埋め込みステートメントとして定義されています。

"***ブロック***" を使用すると、1 つのステートメントしか使用できないコンテキストで複数のステートメントを記述できます。 ブロックは、区切り記号 `{` と `}` の間に記述されたステートメントのリストから成ります。

"***宣言ステートメント***" は、ローカル変数および定数の宣言に使用します。

"***式ステートメント***" は、式の評価に使用します。 ステートメントとして使用できる式には、メソッドの呼び出し、オブジェクトの割り当てを使用して、`new`演算子を使用して割り当て`=`と複合代入演算子、を使用して操作をインクリメントおよびデクリメント`++`と`--`演算子と、await 式。

"***選択ステートメント***" は、式の値に基づいて、実行できる多数のステートメントから 1 つを選択するために使用します。 このグループには `if` および `switch` ステートメントが含まれます。

***繰り返しステートメント***を繰り返し、埋め込みステートメントを実行するために使用します。 このグループには、`while`、`do`、`for`、および `foreach` ステートメントが含まれます。

"***ジャンプ ステートメント***" は、制御を移すために使用します。 このグループには、`break`、`continue`、`goto`、`throw`、`return`、および `yield` ステートメントが含まれます。

`try`...`catch` ステートメントはブロックの実行中に発生した例外をキャッチするために使用し、`try`...`finally` ステートメントは例外が発生したかどうかにかかわらず常に実行される終了処理コードを指定するために使用します。

`checked`と`unchecked`ステートメントを使用して、オーバーフロー チェックを整数型の算術演算と変換のコンテキストを制御します。

`lock` ステートメントは、指定のオブジェクトに対する相互排他ロックを取得し、ステートメントを実行してからロックを解放するために使用します。

`using` ステートメントは、リソースを取得し、ステートメントを実行してからそのリソースを破棄するために使用します。

各種類のステートメントの例を次に示します

__ローカル変数の宣言__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__ローカル定数宣言__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__式ステートメント__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if` ステートメント__

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


__`switch` ステートメント__

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

__`while` ステートメント__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do` ステートメント__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for` ステートメント__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach` ステートメント__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break` ステートメント__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue` ステートメント__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto` ステートメント__

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

__`return` ステートメント__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield` ステートメント__

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

__`throw` `try`ステートメント__

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

__`checked` `unchecked`ステートメント__

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

__`lock` ステートメント__

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

__`using` ステートメント__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a>クラスとオブジェクト

"***クラス***" は C# の最も基本的な型です。 クラスは、状態 (フィールド) とアクション (メソッドおよびその他の関数メンバー) を 1 つの単位としてまとめたデータ構造です。 クラスは動的に作成された "***インスタンス***" の定義を提供し、"***オブジェクト***" とも呼ばれます。 クラスでは、"***継承***"と "***ポリモーフィズム***" をサポートします。これによって "***派生クラス***" が "***基底クラス***" を拡張して特殊化できます。

新しいクラスはクラス宣言を使用して作成されます。 クラス宣言は、クラスの属性と修飾子、クラスの名前、基底クラス (指定されている場合)、およびクラスによって実装されるインターフェイスを指定するヘッダーで開始します。 ヘッダーの後にはクラス本体が続きます。これは、区切り記号 `{` と `}` の間に記述するメンバー宣言のリストで構成されます。

`Point` という名前の単純なクラスの宣言を次に示します。

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
クラスのインスタンスは `new` 演算子を使用して作成されます。この演算子は新しいインスタンスのメモリを割り当て、コンストラクターを呼び出してインスタンスを初期化し、インスタンスへの参照を返します。 次のステートメントは、2 つ作成`Point`オブジェクトし、それらのオブジェクトへの参照を 2 つの変数に格納します。

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
オブジェクトによって占有されているメモリは、オブジェクトの使用が不要になったときに自動的に解放されます。 C# では、オブジェクトの割り当てを明示的に解除する必要がなく、また解除することもできません。

### <a name="members"></a>メンバー

クラスのメンバーは***静的メンバー***または***インスタンス メンバー***します。 静的メンバーはクラスに属しており、インスタンス メンバーはオブジェクト (クラスのインスタンス) に属しています。

次の表では、クラスに格納できるメンバーの種類の概要を示します。


| __Member__   | __説明__ |
|------------  |-----------------|
| 定数    | クラスに関連付けられている定数値 |
| フィールド       | クラスの変数 |
| メソッド      | クラスによって実行可能な計算とアクション |
| プロパティ   | クラスの名前付きプロパティの読み取りと書き込みに関連付けられているアクション |
| インデクサー     | 配列など、クラスのインスタンスのインデックス作成に関連付けられているアクション |
| イベント       | クラスによって生成可能な通知 |
| 演算子    | クラスによってサポートされている変換と式の演算子 |
| コンストラクター | クラスのインスタンスまたはクラス自体を初期化するために必要なアクション |
| デストラクター  | クラスのインスタンスが完全に破棄される前に実行するアクション |
| 種類        | クラスで宣言される、入れ子にされた型 |

### <a name="accessibility"></a>ユーザー補助

クラスの各メンバーにはアクセシビリティが関連付けられています。アクセシビリティは、メンバーへのアクセスが可能なプログラムのテキストの範囲を制御します。 アクセシビリティには 5 つの有効な形式があります。 次の表に、これらのレポートをまとめます。


| __ユーザー補助__    | __意味__ |
|----------------------|-----------------|
| `public`             | アクセスは制限されません。 |
| `protected`          | アクセスは、このクラスまたはこのクラスから派生したクラスに制限されます。 |
| `internal`           | アクセスはこのプログラムに制限されます。 |
| `protected internal` | アクセスは、このプログラムまたはこのクラスから派生したクラスに制限されます。 |
| `private`            | アクセスはこのクラスに制限されます。 |

### <a name="type-parameters"></a>型パラメーター

クラス定義では、クラス名の後に型パラメーター名のリストを山かっこで囲むことで、型パラメーターのセットを指定できます。 型パラメーターは、クラス宣言の本体で使用して、クラスのメンバーを定義します。 次の例では、`Pair` の型パラメーターは `TFirst` と `TSecond` です。

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
型パラメーターのように宣言されているクラス型には、ジェネリック クラスの型が呼び出されます。 構造体、インターフェイス、およびデリゲートの型もジェネリックです。

ジェネリック クラスを使用する場合は、それぞれの型パラメーターの型引数を指定する必要があります。

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
提供されるような型引数を持つジェネリック型`Pair<int,string>
    `と呼ばれる構築型は、上。

### <a name="base-classes"></a>基底クラス

クラス宣言では、クラス名と型パラメーターの後にコロンと基底クラスの名前を入力することで、基底クラスを指定できます。 基底クラスの指定の省略は、`object` 型からの派生と同じです。 次の例では、`Point3D` の基底クラスは `Point` であり、`Point` の基底クラスは `object` です。

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
クラスは、その基底クラスのメンバーを継承します。 継承では、クラスが、インスタンスおよび静的コンス トラクター、および基底クラスのデストラクターを除く、基底クラスのすべてのメンバーには暗黙的に含まれていることを意味します。 派生クラスは、継承するメンバーに新しいメンバーを追加できますが、継承されたメンバーの定義を削除することはできません。 前述の例では、`Point3D` は、`Point` から `x` フィールドと `y` フィールドを継承します。各 `Point3D` インスタンスには、`x`、`y`、`z` の 3 つのフィールドが含まれています。

暗黙的な変換は、クラス型からその基底クラス型のいずれかに存在します。 そのため、クラス型の変数は、そのクラスのインスタンスまたは任意の派生クラスのインスタンスを参照できます。 たとえば、前述のクラス宣言では、`Point` 型の変数が `Point` または `Point3D` を参照できます。

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>フィールド

フィールドとは、クラスまたはクラスのインスタンスに関連付けられている変数です。

宣言されたフィールド、`static`修飾子を定義、***静的フィールド***します。 静的フィールドは、格納場所を 1 つだけ識別します。 クラスのインスタンスがいくつ作成されても、静的フィールドのコピーは 1 つだけです。

なし、フィールドで宣言された、`static`修飾子を定義、***インスタンス フィールド***します。 クラスの各インスタンスには、そのクラスのすべてのインスタンス フィールドの個別のコピーが含まれています。

次の例では、`Color` クラスの各インスタンスに、インスタンス フィールド `r`、`g`、`b` の個別のコピーが含まれていますが、静的フィールド `Black`、`White`、`Red`、`Green`、`Blue` のコピーは 1 つだけです。

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
前述の例のように、`readonly` 修飾子を使用して "***読み取り専用フィールド***" を宣言できます。 割り当て、`readonly`フィールドは、同じクラスのコンス トラクター、フィールドの宣言の一部としてのみ実行できます。

### <a name="methods"></a>メソッド

"***メソッド***" は、オブジェクトまたはクラスによって実行可能な計算またはアクションを実装するメンバーです。 "***静的メソッド***" にはクラスを通じてアクセスします。 "***インスタンス メソッド***" にはクラスのインスタンスを通じてアクセスします。

メソッドは、(場合によっては空) の一覧を持つ***パラメーター***、値またはメソッドに渡される変数の参照を表します、***型を返す***が計算され、によって返される値の型を指定します。メソッド。 メソッドの戻り値の型は`void`値を返さない場合。

型と同様に、メソッドには型パラメーターのセットを含めることができます。その場合、メソッドの呼び出し時に型引数を指定する必要があります。 型引数は、型とは異なり、多くの場合メソッド呼び出しの引数から推論できます。型引数を明示的に指定する必要はありません。

メソッドの "***シグネチャ***" は、メソッドが宣言されているクラス内で一意である必要があります。 メソッドのシグネチャは、メソッドの名前、型パラメーターの数、およびメソッドのパラメーターの数、修飾子、型で構成されます。 メソッドのシグネチャに戻り値の型は含まれません。

#### <a name="parameters"></a>パラメーター

パラメーターは、値または変数参照をメソッドに渡すために使用されます。 メソッドのパラメーターは、メソッドの呼び出し時に指定する "***引数***" から実際の値を取得します。 値パラメーター、参照パラメーター、出力パラメーター、およびパラメーター配列の 4 種類のパラメーターがあります。

"***値パラメーター***" は入力パラメーターの引き渡しに使用します。 値パラメーターは、パラメーターに渡された引数からその初期値を取得するローカル変数に相当します。 値パラメーターに対する変更は、パラメーターに渡された引数には影響しません。

値パラメーターは省略可能であり、既定値を指定すると、対応する引数を省略できます。

"***参照パラメーター***" は入力パラメーターと出力パラメーターの引き渡しに使用します。 参照パラメーターに渡す引数は変数である必要があり、メソッドの実行中に、参照パラメーターは引数の変数と同じ格納場所を表します。 参照パラメーターは、`ref` 修飾子で宣言されます。 `ref` パラメーターの使用例を次に示します。

```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
"***出力パラメーター***" は出力パラメーターの引き渡しに使用します。 呼び出し元が提供する引数の初期値が重要でないことを除き、出力パラメーターは参照パラメーターと同様です。 出力パラメーターは、`out` 修飾子で宣言されます。 `out` パラメーターの使用例を次に示します。

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
"***パラメーター配列***" は、引数の変数の数をメソッドに渡せるようにします。 パラメーター配列は、`params` 修飾子で宣言されます。 パラメーター配列として使用できるのは、メソッドの最後のパラメーターのみです。パラメーター配列の型は、1 次元配列の型である必要があります。 `Write`と`WriteLine`のメソッド、`System.Console`クラスは、パラメーター配列の使用方法の良い例です。 これらのメソッドは次のように宣言されます。

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
パラメーター配列を使用するメソッド内では、パラメーター配列は、配列型の通常のパラメーターとまったく同じように動作します。 ただし、パラメーター配列を使用するメソッドの呼び出しでは、パラメーター配列の型の 1 つの引数またはパラメーター配列の要素型の任意の数の引数を渡すことができます。 後者の場合、配列インスタンスが自動的に作成され、指定した引数を使用して初期化されます。 次のような例があるとします。

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
これは、次の記述と同じです。

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>メソッドの本体とローカル変数

メソッドの本文には、メソッドが呼び出されたときに実行するステートメントを指定します。

メソッドの本体は、メソッドの呼び出しに固有の変数を宣言できます。 このような変数は "***ローカル変数***" と呼ばれます。 ローカル変数宣言は、型名、変数名、および (場合によっては) 初期値を指定します。 次の例では、初期値 0 を使用してローカル変数 `i` を宣言し、初期値を使用せずにローカル変数 `j` を宣言します。

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
C# では、ローカル変数の値を取得する前に、ローカル変数を "***明示的に割り当てる***" 必要があります。 たとえば、前述の `i` の宣言に初期値が含まれていなかった場合、コンパイラは以降の `i` の使用に対するエラーを報告します。これは、プログラム内のそれらのポイントで `i` が明示的に割り当てられていないためです。

メソッドでは、`return` ステートメントを使用して、呼び出し元に制御を戻すことができます。 `void` を返すメソッドの場合、`return` ステートメントは式を指定できません。 以外を返すメソッドで`void`、`return`ステートメントは、戻り値を計算する式を含める必要があります。

#### <a name="static-and-instance-methods"></a>静的メソッドとインスタンス メソッド

宣言されたメソッド、`static`修飾子は、***静的メソッド***します。 静的メソッドは、特定のインスタンスでは動作せず、静的メンバーにのみ直接アクセスできます。

なしで宣言されたメソッド、`static`修飾子は、***インスタンス メソッド***します。 インスタンス メソッドは、特定のインスタンスで動作し、静的メンバーとインスタンス メンバーの両方にアクセスできます。 インスタンス メソッドが呼び出されたインスタンスには、`this` として明示的にアクセスできます。 静的メソッドで `this` を参照するとエラーになります。

次の `Entity` クラスには、静的メンバーとインスタンス メンバーの両方があります。

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
各 `Entity` インスタンスには、シリアル番号 (およびここに表示されていないその他の情報) が含まれています。 `Entity` コンストラクターは (インスタンス メソッドと同様に)、次に使用可能なシリアル番号を持つ新しいインスタンスを初期化します。 コンストラクターはインスタンス メンバーであるため、`serialNo` インスタンス フィールドと `nextSerialNo` 静的フィールドの両方にアクセスできます。

静的メソッドである `GetNextSerialNo` と `SetNextSerialNo` は `nextSerialNo` 静的フィールドにアクセスできますが、`serialNo` インスタンス フィールドに直接アクセスするとエラーになります。

次の例では、使用、`Entity`クラス。

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
静的メソッドである `SetNextSerialNo` と `GetNextSerialNo` はクラスで呼び出されますが、`GetSerialNo` インスタンス メソッドはクラスのインスタンスで呼び出されます。

#### <a name="virtual-override-and-abstract-methods"></a>仮想メソッド、オーバーライド メソッド、および抽象メソッド

インスタンス メソッドの宣言に `virtual` 修飾子が含まれている場合、そのメソッドは "***仮想メソッド***" と呼ばれます。 ない場合`virtual`修飾子が存在する、メソッドはモード、***非仮想メソッド***します。

仮想メソッドが呼び出されると、その呼び出しが行われるインスタンスの "***実行時の型***" によって、呼び出す実際のメソッドの実装が決定します。 非仮想メソッドの呼び出しでは、インスタンスの "***コンパイル時の型***" が決定要因です。

仮想メソッドは派生クラスで "***オーバーライド***" できます。 インスタンス メソッドの宣言が含まれています、`override`修飾子、メソッドは、同じシグネチャを持つ継承された仮想メソッドをオーバーライドします。 仮想メソッドの宣言には新しいメソッドが導入されていますが、オーバーライド メソッドの宣言では、そのメソッドの新しい実装を提供することで既存の継承された仮想メソッドを特殊化します。

***抽象***メソッドは実装のない仮想メソッドです。 抽象メソッドが宣言された、`abstract`修飾子も宣言されているクラスだけでは使用`abstract`します。 抽象メソッドは、すべての非抽象派生クラスでオーバーライドする必要があります。

次の例では、式ツリー ノードを表す抽象クラス `Expression`、および定数、変数参照、算術演算の式ツリー ノードを実装する 3 つの派生クラス `Constant`、`VariableReference`、`Operation` を宣言します  (これに似ていますと混同しないように式ツリーの型がで導入された[式ツリー型](types.md#expression-tree-types))。

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
前述の 4 つのクラスは、算術式をモデル化するために使用できます。 たとえば、これらのクラスのインスタンスを使用して、式 `x + 3` を次のように表すことができます。

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
`Expression` インスタンスの `Evaluate` メソッドが呼び出され、指定された式を評価して `double` 値を生成します。 メソッドが引数として受け取り、 `Hashtable` (エントリのキー) として変数名と (エントリの値) として値を格納します。 `Evaluate`メソッドは、仮想抽象メソッド、つまり、実際の実装を提供することを非抽象派生クラスでオーバーライドする必要があります。

`Evaluate` の `Constant` の実装は、格納された定数を単に返します。 A`VariableReference`の実装は、ハッシュ テーブル内の変数の名前と、結果の値を返します。 `Operation` の実装は、(`Evaluate` メソッドを再帰的に呼び出すことによって) まず左と右のオペランドを評価し、指定された算術演算を実行します。

次のプログラムでは、`Expression` クラスを使用して、式 `x * (y + 2)` の異なる値の `x` と `y` を評価します。

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a>メソッドのオーバーロード

メソッドの "***オーバーロード***" では、メソッドのシグネチャが一意であれば、同じクラス内の複数のメソッドに同じ名前を付けることができます。 オーバーロードされたメソッドの呼び出しをコンパイルする場合、コンパイラは "***オーバーロードの解決***" を使用して、呼び出すメソッドを決定します。 オーバーロードの解決では、引数に最も一致する 1 つのメソッドが特定されます。最も一致するメソッドが見つからない場合は、エラーが報告されます。 次の例は、オーバーロードの解決が有効な場合を示しています。 `Main` メソッド内の各呼び出しのコメントは、実際に呼び出されるメソッドを示しています。

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
この例に示すように、パラメーターの厳密な型に引数を明示的にキャストするか、または型引数を明示的に指定することにより、特定のメソッドを常に選択できます。

### <a name="other-function-members"></a>その他の関数メンバー

実行可能コードが含まれるメンバーは、クラスの "***関数メンバー***" と総称されます。 前のセクションでは、関数メンバーの主な種類であるメソッドについて説明しました。 このセクションは、c# でサポートされている関数メンバーの他の種類を説明します。 コンス トラクター、プロパティ、インデクサー、イベント、演算子、およびデストラクター。

次のコードと呼ばれるジェネリック クラスを示しています。 `List<T>`、拡張可能なオブジェクトの一覧を実装します。 このクラスには、最も一般的な種類の関数メンバーの例がいくつか含まれています。


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a>コンストラクター

C# は、インスタンス コンストラクターと静的コンストラクターの両方をサポートします。 "***インスタンス コンストラクター***" は、クラスのインスタンスを初期化するために必要なアクションを実装するメンバーです。 "***静的コンストラクター***" は、クラスを最初に読み込むときに、そのクラス自体を初期化するために必要なアクションを実装するメンバーです。

コンストラクターは、戻り値の型がなく、含んでいるクラスと同じ名前を持つメソッドのように宣言されます。 コンス トラクターの宣言が含まれている場合、`static`修飾子は、静的コンス トラクターを宣言します。 それ以外の場合は、インスタンス コンストラクターが宣言されます。

インスタンス コンス トラクターはオーバー ロードできます。 たとえば、`List<T>
` クラスは、2 つの (1 つはパラメーターなし、もう 1 つは `int` パラメーターを受け取る) インスタンス コンストラクターを宣言します。 インスタンス コンストラクターは、`new` 演算子を使用して呼び出されます。 次のステートメントは、2 個割り当てる`List<string>
`インスタンスのコンス トラクターのそれぞれを使用して、`List`クラス。

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
他のメンバーとは異なり、インスタンス コンストラクターは継承されず、クラスには、そのクラスで実際に宣言された以外のインスタンス コンストラクターがありません。 クラスのインスタンス コンストラクターが指定されていない場合は、パラメーターなしの空のコンストラクターが自動的に指定されます。

#### <a name="properties"></a>プロパティ

"***プロパティ***" は、フィールドが自然に拡張したものです。 フィールドとプロパティはどちらも型が関連付けられている名前付きのメンバーであり、それらにアクセスするための構文は同じです。 ただし、フィールドとは異なり、プロパティは格納場所を表しません。 その代わりに、プロパティには、値の読み取りまたは書き込みの際に実行されるステートメントを指定する "***アクセサー***" があります。

宣言が終了する点を除いて、フィールドのようにプロパティが宣言されている、`get`アクセサーおよび`set`区切り記号の間に記述されたアクセサー`{`と`}`セミコロンで終わるのではなく。 両方を持つプロパティを`get`アクセサーと`set`アクセサーが、***読み取り/書き込みプロパティ***、のみを持つプロパティを`get`アクセサーが、***読み取り専用プロパティ***、およびプロパティのみを持つ、`set`アクセサーは、***書き込み専用プロパティ***します。

A`get`アクセサーはプロパティの型の値を返すパラメーターなしのメソッドに相当します。 代入のターゲット プロパティが式で参照されている場合を除き、`get`プロパティのアクセサーが呼び出され、プロパティの値を計算します。

A`set`アクセサーがという名前のパラメーターを 1 つのメソッドに対応`value`と戻り値の型ではありません。 プロパティが参照されると、割り当ての対象として、またはのオペランドとして`++`または`--`、`set`アクセサーは新しい値を提供する引数を指定して呼び出されます。

`List<T>
`クラスは、2 つのプロパティを宣言します。`Count`と`Capacity`、これは読み取り専用と読み取り/書き込み、それぞれします。 これらのプロパティの使用例を次に示します。

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
フィールドおよびメソッドと同様に、C# はインスタンス プロパティと静的プロパティの両方をサポートします。 宣言されている静的プロパティ、`static`せずに、修飾子、およびインスタンスのプロパティを宣言します。

プロパティのアクセサーは仮想にすることができます。 プロパティの宣言に `virtual`、`abstract`、または `override` の各修飾子が含まれている場合、その宣言はプロパティのアクセサーに適用されます。

#### <a name="indexers"></a>インデクサー

"***インデクサー***" は、配列と同じ方法でオブジェクトのインデックスを作成できるようにするメンバーです。 メンバーの名前がある点が、インデクサーはプロパティのように宣言が`this`区切り記号の間に記述されたパラメーター リストが続く`[`と`]`します。 パラメーターは、インデクサーのアクセサーで使用できます。 プロパティと同様に、読み取り/書き込み、読み取り専用、および書き込み専用のインデクサーを使用できます。また、インデクサーのアクセサーを仮想にすることができます。

`List` クラスは、`int` パラメーターを受け取る 1 つの読み取り/書き込みインデクサーを宣言します。 インデクサーを使用すると、`int` 値を持つ `List` インスタンスのインデックスを作成できます。 次に例を示します。

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
インデクサーはオーバーロードできます。つまり、パラメーターの数または型が異なる限り、クラスは複数のインデクサーを宣言できます。

#### <a name="events"></a>イベント

"***イベント***" は、クラスまたはオブジェクトで通知を提供できるようにするメンバーです。 宣言が含まれている点を除いて、フィールドのようにイベントが宣言されている、`event`キーワードと型がデリゲート型にする必要があります。

イベント メンバーを宣言するクラス内では、イベントはデリゲート型のフィールドと同じように動作します (イベントが抽象イベントでなく、アクセサーを宣言しない場合)。 フィールドは、イベントに追加されたイベント ハンドラーを表すデリゲートへの参照を格納します。 イベント ハンドルが存在しない場合、フィールドは`null`します。

`List<T>
` クラスは、`Changed` という 1 つのイベント メンバーを宣言します。このメンバーは新しい項目がリストに追加されたことを示します。 `Changed`イベントは、`OnChanged`仮想メソッドで、最初は、イベントは、かどうかを確認します。 `null` (つまり、ハンドラーが存在しないこと)。 イベントを発生させるという概念は、イベントによって表されるデリゲートの呼び出しとまったく同じです。したがって、イベントを発生させるための特殊な言語コンストラクトはありません。

クライアントは、"***イベント ハンドラー***" を使用してイベントに対応します。 イベント ハンドラーは、`+=` 演算子を使用してアタッチされ、`-=` 演算子を使用して削除されます。 次の例では、`List<string>
` の `Changed` イベントにイベント ハンドラーをアタッチします。

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
イベントの基になる記憶域の制御が求められる高度なシナリオでは、イベントの宣言で `add` アクセサーと `remove` アクセサーを明示的に指定できます。これらは、プロパティの `set` アクセサーにある程度似ています。

#### <a name="operators"></a>演算子

"***演算子***" は、クラスのインスタンスに特定の式の演算子を適用する意味を定義するメンバーです。 単項演算子、2 項演算子、および変換演算子の 3 種類を定義できます。 すべての演算子は `public` および `static` として宣言する必要があります。

`List<T>
` クラスは 2 つの演算子 (`operator==` と `operator!=`) を宣言し、`List` インスタンスにこれらの演算子を適用する式に新しい意味を持たせます。 具体的には、演算子が 2 つの等しいかどうかを定義`List<T>
`インスタンスを使用して含まれているオブジェクトの各比較として、`Equals`メソッド。 次の例では、`==` 演算子を使用して 2 つの `List<int>
` インスタンスを比較します。

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

最初の `Console.WriteLine` は `True` を出力します。これは、2 つのリストに、同じ値を持つ同じ数のオブジェクトが同じ順序で含まれているためです。 `List<T>
` で `operator==` が定義されていない場合は、`a` と `b` が異なる `List<int>
` インスタンスを参照するため、最初の `Console.WriteLine` は `False` を出力します。

#### <a name="destructors"></a>デストラクター

A***デストラクター***はクラスのインスタンスを消滅させるために必要なアクションを実装するメンバーです。 デストラクターは、パラメーターを含めることはできません、アクセシビリティ修飾子は使用できません、明示的に呼び出すことはできません。 インスタンスのデストラクターは、ガベージ コレクション中に自動的に呼び出されます。

ガベージ コレクターには、かなり自由に決定するオブジェクトを収集し、デストラクターの実行が許可されます。 具体的には、デストラクターの呼び出しのタイミングは確定的でないと、デストラクターは、任意のスレッドで実行する可能性があります。 これらとその他の理由は、他のソリューションが実現できない場合にのみ、クラスはデストラクターを実装する必要があります。

`using` ステートメントは、オブジェクトを破棄するためのより適切な方法を提供します。

## <a name="structs"></a>構造体

***構造体***は、クラスと同様に、データ メンバーおよび関数メンバーを含むことができるデータ構造ですが、値型でありヒープ割り当てを必要としない点でクラスと異なります。 構造体型の変数は、構造体のデータを直接格納しますが、クラス型の変数は、動的に割り当てられたオブジェクトへの参照を格納します。 構造体型はユーザー指定の継承をサポートしていませんし、すべての構造体型が暗黙的に `object` 型を継承します。

構造体は、値セマンティクスを持つ小規模なデータ構造に特に便利です。 構造体の主な例としては、複素数、座標系のポイント、ディクショナリのキーと値のペアなどがあります。 小規模なデータ構造には、クラスよりむしろ構造体を使用するほうが、アプリケーションが実行するメモリ割り当ての数を大幅に減らすことができます。 たとえば、次のプログラムでは、100 個のポイントの配列を作成し初期化します。 `Point` をクラスとして実装すると、101 個の別々のオブジェクトがインスタンス化されます。配列に1 個、残りは 100 個の要素に 1 個ずつです。

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
別の方法はさせる`Point`構造体。

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
ここでは 1 つのオブジェクトのみがインスタンス化されます。すなわち、配列に 1 個です。そして、`Point` インスタンスがその配列内にインラインで格納されます。

構造体コンストラクターは `new` 演算子を使って呼び出されますが、メモリが割り当てられていることを意味するものではありません。 オブジェクトを動的に割り当てそこへの参照を返す代わりに、構造体コンストラクターは単に構造体の値自体 (通常、スタック上の一時的な場所) を返し、この値は必要に応じてコピーされます。

クラスを使用すると、2 つの変数が同じオブジェクトを参照できるため、1 つの変数に対する操作によって、もう一方の変数によって参照されるオブジェクトに影響を与えることができます。 構造体を使用すると、各々の変数がデータのコピーを各々で持ち、1 つに対する操作がもう一方に影響を与えることはできません。 たとえば、次のコード フラグメントによって生成される出力がかどうかに依存`Point`がクラスまたは構造体。

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
場合`Point`クラスで、出力は`20`ため`a`と`b`同じオブジェクトを参照します。 場合`Point`、構造体は、出力は、`10`ための割り当て`a`に`b`、値のコピーを作成し、このコピーはへの後続の割り当てによって影響を受けません`a.x`。

前述の例では、構造体の 2 つの制限事項が強調されています。 1 つめは、構造体全体をコピーすることは通常、オブジェクト参照をコピーするよりも非効率であり、割り当てと値パラメーターの引き渡しは参照型よりも構造体のほうが手がかかるということです。 2 つめは、`ref` および `out` パラメーターを除いて、構造体への参照を作成することはできず、そのために構造体を使用できない状況が数多くあるということです。

## <a name="arrays"></a>配列

***配列***はデータ構造で、算出されたインデックスを介してアクセスされる多くの変数を含みます。 配列に含まれる変数は配列の***要素***とも呼ばれ、すべて同じ型であり、この型は配列の***要素型***と呼ばれます。

配列型は参照型で、配列変数の宣言は、配列インスタンスへの参照の領域を確保します。 使用して実行時に実際の配列インスタンスが動的に作成された、`new`演算子。 `new`操作を指定します、***長さ***新しい配列のインスタンスは、インスタンスの有効期間は固定です。 配列の要素のインデックスは、`0` から `Length - 1` までです。 `new` 演算子は配列の要素を自動的に既定値に初期化します。たとえば、すべての数値型はゼロ、すべての参照型は `null` です。

次の例では、`int` 要素の配列を作成し、その配列を初期化し、配列の内容を出力します。

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
この例は ***1 次元配列***を作成し、操作対象とします。 C# はさらに***多次元配列***をサポートします。 配列型の次元数は、配列型の***ランク***とも呼ばれ、配列型の角かっこ内に記述されたコンマの数に 1 を追加したものです。 次の例では、1 次元、2 次元および 3 次元の配列を割り当てます。

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
`a1` 配列は 10 の要素、`a2` 配列は 50 (10 × 5) の要素、`a3` 配列は 100 (10 × 5 × 2) の要素を含みます。

配列の要素型には、配列型を含む任意の型を指定できます。 配列型の要素を持つ配列は、***ジャグ配列***と呼ばれることがあります。要素の配列の長さがすべて同じである必要がないからです。 次の例では、`int` の配列の配列を割り当てます。

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
最初の行で 3 つの要素を持つ配列を作成しますが、各々は `int[]` 型で `null` 初期値を持ちます。 それ以降の行では、3 つの要素を、それぞれ異なる長さの配列インスタンスへの参照で初期化します。

`new`演算子を使用して指定する配列要素の初期値を許可する、***配列初期化子***、区切り記号の間に記述された式の一覧は`{`と`}`します。 次の例は、`int[]` を割り当て 3 つの要素で初期化します。

```csharp
int[] a = new int[] {1, 2, 3};
```
配列の長さが間にある式の数から推論されることに注意してください。`{`と`}`します。 ローカル変数およびフィールド宣言をさらに短縮して、配列型を再起動する必要がないようにできます。

```csharp
int[] a = {1, 2, 3};
```
前述の例はどちらも、次の例と同等です。

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>インターフェイス

***インターフェイス***は、クラスと構造体によって実装できるコントラクトを定義します。 1 つのインターフェイスには、メソッド、プロパティ、イベント、およびインデクサーが含まれる場合があります。 インターフェイスでは、定義するメンバーの実装は行いません。インターフェイスを実装するクラスまたは構造体によって提供される必要があるメンバーを指定するだけです。

インターフェイスは***多重継承***を使用する場合があります。 次の例では、インターフェイス `IComboBox` は `ITextBox` と `IListBox` の両方から継承します。

```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
クラスと構造体は、複数のインターフェイスを実装できます。 次の例では、クラス `EditBox` は `IControl` と `IDataBound` の両方を実装しています。

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
クラスまたは構造体が特定のインターフェイスを実装する場合、そのクラスまたは構造体のインスタンスはそのインターフェイス型に暗黙的に変換できます。 次に例を示します。

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
インスタンスが特定のインターフェイスを静的に実装しないとわかっている場合は、動的な型キャストを使用できます。 たとえば、次のステートメント オブジェクトの取得する動的な型キャストを使用して、`IControl`と`IDataBound`インターフェイスの実装。 オブジェクトの実際の型であるため`EditBox`キャストは成功します。

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
前の`EditBox`クラス、`Paint`からメソッド、`IControl`インターフェイスと`Bind`からメソッド、`IDataBound`を使用してインターフェイスを実装`public`メンバー。 C# もサポート***明示的なインターフェイス メンバーの実装***、しており、クラスまたは構造体は、メンバーにすることを回避できます`public`します。 明示的なインターフェイス メンバーの実装は、完全修飾のインターフェイス メンバー名を使用して書き込まれます。 たとえば、`EditBox` クラスは、次のように明示的なインターフェイス メンバーの実装を使用して、`IControl.Paint` メソッドおよび `IDataBound.Bind` メソッドを実装できます。

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
明示的なインターフェイス メンバーは、インターフェイス型を経由してのみアクセスできます。 実装ではたとえば、`IControl.Paint`前によって提供される`EditBox`クラスは、最初に変換することによってのみ呼び出すことができます、`EditBox`への参照、`IControl`インターフェイス型。

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>列挙体

***列挙型***は、一連の名前付き定数を使用する固有の値の型です。 次の例を宣言してという名前の列挙型を使用する`Color`3 つの定数値を持つ`Red`、 `Green`、および`Blue`します。

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
各列挙型が、対応する整数型と呼ばれる、***基になる型***の列挙型。 列挙型を基になる型を明示的に宣言しないが、基になる型の`int`します。 列挙型のストレージ形式と使用可能な値の範囲については、基になる型によって決まります。 列挙型上で実行できる値のセットは、その列挙型のメンバーでは制限されません。 具体的には、enum の基になる型の任意の値は列挙型にキャストできるし、その列挙型の個別の有効な値は、します。

次の例は、という名前の列挙型を宣言`Alignment`の基になる種類で`sbyte`します。

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
前の例に示す、列挙型メンバーの宣言は、メンバーの値を指定する定数式を含めることができます。 各列挙型メンバーの定数値は、列挙型の基になる型の範囲でなければなりません。 列挙型メンバーの宣言が明示的に値を指定しない場合に、値 0 (列挙型の最初のメンバーの場合) または直前の列挙型のメンバーと 1 つの値、メンバーが与えられます。

列挙型の値は整数に変換された値と型キャストを使用してその逆にできます。 次に例を示します。

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
任意の列挙型の既定値は、列挙型に変換された 0 の整数の値です。 変数に自動的に既定値に初期化する場合、これは、列挙型の変数に渡された値です。 簡単に使用できる列挙型リテラルの既定値の順序で`0`任意の列挙型に暗黙的に変換します。 したがって、次の例も許可されます。

```csharp
Color c = 0;
```

## <a name="delegates"></a>デリゲート

***デリゲート型***は、特定のパラメーター リストおよび戻り値を使用してメソッドへの参照を表します。 デリゲートを使用すれば、変数に割り当ててパラメーターとして渡すことのできるエンティティとして、メソッドを処理できます。 デリゲートはまた、他のいくつかの言語にみられる関数ポインターの概念に似ていますが、関数ポインターとは異なり、デリゲートはオブジェクト指向でタイプ セーフです。

次の例では、`Function` という名前のデリゲート型を宣言して使用します。

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
`Function` デリゲート型のインスタンスは、`double` 引数を取得して `double` 値を返す任意のメソッドを参照できます。 `Apply`メソッドが適用されます、特定`Function`の要素に、 `double[]`、返される、`double[]`結果。 `Main` メソッドでは、`Apply` は `double[]` に 3 つの異なる関数を適用するために使用されます。

デリゲートは、静的メソッド (前述の例の `Square` や `Math.Sin` など) またはインスタンス メソッド (前述の例の `m.Multiply` など) のいずれかを参照できます。 インスタンス メソッドを参照するデリゲートはまた、特定のオブジェクトを参照し、インスタンス メソッドがデリゲートから呼び出されると、そのオブジェクトは呼び出しで `this` になります。

その場で作成される「インライン メソッド」である匿名関数を使用してデリゲートを作成することもできます。 匿名関数では、周囲のメソッドのローカル変数を確認できます。 したがってなどを使用せずを使用して、上記の乗数の例をより簡単に記述する、`Multiplier`クラス。

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
デリゲートの興味深く有用な点は、参照するメソッドのクラスを把握せず必要ともしないことです。重要なのは、参照されるメソッドは同じパラメーターを持ち、デリゲートとして型を返すということです。

## <a name="attributes"></a>属性

C# プログラムにおける型、メンバー、およびその他のエンティティは、動作の特定の側面を制御する修飾子をサポートします。 たとえばメソッドのアクセシビリティは、`public`、`protected`、`internal`、および `private` 修飾子を使用して制御されます。 C# はこの機能を一般化し、宣言情報のユーザー定義型をプログラム エンティティに追加して実行時に取得できるようにします。 プログラムでは、***属性***を定義して使用することにより、この追加の宣言情報を指定します。

次の例では、プログラムのエンティティに配置して関連するドキュメントへのリンクを提供することができる `HelpAttribute` 属性を宣言しています。

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
すべての属性クラスから派生、`System.Attribute`基本 .NET Framework で提供されるクラス。 属性は、関連付けられた宣言の直前に、名前を任意の変数とともに角かっこで囲んで与えることにより、適用できます。 属性の名前で終わる場合`Attribute`属性が参照されている場合、名前の部分を省略できます。 たとえば、`HelpAttribute` 属性は次のように使用できます。

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
この例ではアタッチ、`HelpAttribute`を`Widget`クラスと別`HelpAttribute`を`Display`クラスのメソッド。 属性クラスのパブリック コンストラクターは、属性がプログラム エンティティにアタッチされたときに提供する必要がある情報を制御します。 その属性クラスのパブリックの読み取り/書き込みプロパティを参照することにより (`Topic` プロパティへの参照のような)、追加情報を提供することができます。

次の例は、実行時に指定されたプログラム エンティティの属性情報を取得する方法を示しています。 リフレクションを使用します。

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
リフレクションによって特定の属性が要求されると、プログラム ソースで提供される情報で属性クラスのコンストラクターが呼び出され、結果の属性インスタンスが返されます。 追加情報がプロパティを通じて提供された場合、属性インスタンスが返される前に、これらのプロパティは指定された値に設定されます。
