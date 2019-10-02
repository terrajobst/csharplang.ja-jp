---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704096"
---
# <a name="delegates"></a>デリゲート

デリゲートを使用するとC++、、Pascal、Modula などの他の言語が関数ポインターでアドレス指定されるシナリオを実現できます。 ただしC++ 、関数ポインターとは異なり、デリゲートは完全なオブジェクト指向C++であり、メンバー関数へのポインターとは異なり、デリゲートはオブジェクトインスタンスとメソッドの両方をカプセル化します。

デリゲート宣言では、クラスから派生したクラス `System.Delegate` を定義します。 デリゲートインスタンスは、1つまたは複数のメソッドのリストである呼び出しリストをカプセル化します。各メソッドは、呼び出し可能なエンティティと呼ばれます。 インスタンスメソッドの場合、呼び出し可能なエンティティは、インスタンスと、そのインスタンスのメソッドで構成されます。 静的メソッドの場合、呼び出し可能なエンティティはメソッドだけで構成されます。 適切な引数のセットを使用してデリゲートインスタンスを呼び出すと、指定された一連の引数を使用してデリゲートの呼び出し可能なエンティティが呼び出されます。

デリゲートインスタンスの興味深い便利なプロパティは、カプセル化するメソッドのクラスについて関知しないことです。これらのメソッドは、デリゲートの型と互換性がある ([デリゲート宣言](delegates.md#delegate-declarations)) ことだけが重要です。 これにより、デリゲートは "匿名" 呼び出しに適しています。

## <a name="delegate-declarations"></a>デリゲート宣言

*Delegate_declaration*は、新しいデリゲート型を宣言する*type_declaration* ([型宣言](namespaces.md#type-declarations)) です。

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

デリゲート宣言で同じ修飾子が複数回出現する場合、コンパイル時エラーになります。

@No__t-0 修飾子は、別の型で宣言されたデリゲートでのみ許可されます。この場合、[新しい修飾子](classes.md#the-new-modifier)で説明されているように、このようなデリゲートは、継承されたメンバーを同じ名前で隠ぺいすることを指定します。

@No__t-0、`protected`、`internal`、および `private` の各修飾子は、デリゲート型のアクセシビリティを制御します。 デリゲート宣言が発生したコンテキストによっては、これらの修飾子の一部が許可されない場合があります ([アクセシビリティの宣言](basic-concepts.md#declared-accessibility))。

デリゲートの型名は*identifier*です。

省略可能な*formal_parameter_list*は、デリゲートのパラメーターを指定し、[*週*] はデリゲートの戻り値の型を示します。

省略可能な*variant_type_parameter_list* ([バリアント型パラメーターリスト](interfaces.md#variant-type-parameter-lists)) は、デリゲート自体の型パラメーターを指定します。

デリゲート型の戻り値の型は、`void` または出力セーフ (差異の[安全性](interfaces.md#variance-safety)) のいずれかである必要があります。

デリゲート型のすべての仮パラメーター型は、入力セーフである必要があります。 また、`out` または `ref` パラメーター型も、出力セーフである必要があります。 基になる実行プラットフォームの制限により、`out` のパラメーターも入力セーフである必要があることに注意してください。

のC#デリゲート型は同じ名前であり、構造的に同等ではありません。 具体的には、同じパラメーターリストと戻り値の型を持つ2つの異なるデリゲート型は、異なるデリゲート型と見なされます。 ただし、2つの異なるが構造的に等価なデリゲート型のインスタンスは、等しいと見なされる場合があります ([デリゲート等値演算子](expressions.md#delegate-equality-operators))。

以下に例を示します。

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

@No__t-0 および `B.M1` のメソッドは、同じ戻り値の型とパラメーターリストを持っているため、`D1` と `D2` の両方のデリゲート型と互換性があります。ただし、これらのデリゲート型は2つの異なる型であるため、交換することはできません。 @No__t-0、`B.M3`、`B.M4` の各メソッドは、異なる戻り値の型またはパラメーターリストを持っているため、デリゲート型 `D1` および `D2` と互換性がありません。

他のジェネリック型宣言と同様に、構築されたデリゲート型を作成するには、型引数を指定する必要があります。 構築されたデリゲート型のパラメーターの型と戻り値の型は、デリゲート宣言の型パラメーターごとに、構築されたデリゲート型の対応する型引数に代入することによって作成されます。 結果の戻り値の型とパラメーターの型は、構築されたデリゲート型と互換性のあるメソッドを決定するために使用されます。 以下に例を示します。

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

メソッド `X.F` はデリゲート型 `Predicate<int>` と互換性があり、メソッド `X.G` はデリゲート型 `Predicate<string>` と互換性があります。

デリゲート型を宣言する唯一の方法は、 *delegate_declaration*を使用することです。 デリゲート型は `System.Delegate` から派生したクラス型です。 デリゲート型は暗黙的に @no__t 0 であるため、デリゲート型から型を派生させることはできません。 また、`System.Delegate` から非デリゲートクラス型を派生させることもできません。 @No__t-0 はそれ自体がデリゲート型ではないことに注意してください。これは、すべてのデリゲート型の派生元であるクラス型です。

C#デリゲートのインスタンス化と呼び出しのための特殊な構文を提供します。 インスタンス化を除き、クラスまたはクラスのインスタンスに適用できる操作は、それぞれデリゲートクラスまたはインスタンスに適用することもできます。 特に、通常のメンバーアクセス構文を使用して `System.Delegate` 型のメンバーにアクセスすることができます。

デリゲートインスタンスによってカプセル化されるメソッドのセットは、呼び出しリストと呼ばれます。 デリゲートインスタンスが1つのメソッドから作成 ([デリゲート互換性](delegates.md#delegate-compatibility)) されると、そのメソッドがカプセル化され、その呼び出しリストにはエントリが1つだけ含まれます。 ただし、null 以外の2つのデリゲートインスタンスを結合すると、2つ以上のエントリを含む新しい呼び出しリストを形成するために、その呼び出しリストが連結されます。

デリゲートは、バイナリ `+` ([加算演算子](expressions.md#addition-operator)) と `+=` 演算子 ([複合代入](expressions.md#compound-assignment)) を使用して結合されます。 デリゲートは、バイナリ `-` ([減算演算子](expressions.md#subtraction-operator)) と `-=` 演算子 ([複合代入](expressions.md#compound-assignment)) を使用して、デリゲートの組み合わせから削除できます。 デリゲートは、等価性 ([デリゲート等値演算子](expressions.md#delegate-equality-operators)) を比較できます。

次の例は、さまざまなデリゲートのインスタンス化と、それらに対応する呼び出しリストを示しています。

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

@No__t-0 および `cd2` がインスタンス化されると、それぞれが1つのメソッドをカプセル化します。 @No__t-0 がインスタンス化されると、その順序で `M1` と @no__t 2 の2つのメソッドの呼び出しリストがあります。 `cd4` の呼び出しリストには、その順序で `M1`、`M2`、および `M1` が含まれています。 最後に、`cd5` の呼び出しリストには、その順序で `M1`、`M2`、`M1`、`M1`、および `M2` が含まれています。 デリゲートの結合 (および削除) のその他の例については、「[デリゲートの呼び出し](delegates.md#delegate-invocation)」を参照してください。

## <a name="delegate-compatibility"></a>デリゲートの互換性

次のすべての条件に該当する場合、メソッドまたはデリゲート `M` はデリゲート @no__t 型と***互換性***があります。

*  `D` および `M` のパラメーター数は同じで、`D` の各パラメーターには、`M` の対応するパラメーターと同じ `ref` または `out` の修飾子が指定されています。
*  各値パラメーター (@no__t 0 または `out` 修飾子のないパラメーター) については、`D` のパラメーターの型から、id 変換 ([id 変換](conversions.md#identity-conversion)) または暗黙の参照変換 ([暗黙的な参照](conversions.md#implicit-reference-conversions)変換) が存在します。`M` の対応するパラメーターの型。
*  @No__t-0 または `out` の各パラメーターについて、`D` のパラメーターの型は `M` のパラメーターの型と同じです。
*  @No__t-0 の戻り値の型から `D` の戻り値の型への id または暗黙の参照変換が存在します。

## <a name="delegate-instantiation"></a>デリゲートのインスタンス化

デリゲートのインスタンスは、 *delegate_creation_expression* ([デリゲート作成式](expressions.md#delegate-creation-expressions)) またはデリゲート型への変換によって作成されます。 新しく作成されたデリゲートインスタンスは、次のいずれかを参照します。

*  *Delegate_creation_expression*で参照される静的メソッド。
*  *Delegate_creation_expression*で参照されるターゲットオブジェクト (@no__t 0 にすることはできません) とインスタンスメソッド。
*  別のデリゲート。

以下に例を示します。

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

インスタンス化されると、デリゲートインスタンスは常に同じターゲットオブジェクトとメソッドを参照します。 2つのデリゲートが結合されている場合、または1つが別のデリゲートから削除された場合は、新しいデリゲートの結果が独自の呼び出しリストになります。結合または削除されたデリゲートの呼び出しリストは変更されません。

## <a name="delegate-invocation"></a>デリゲートの呼び出し

C#デリゲートを呼び出すための特別な構文を提供します。 呼び出しリストに1つのエントリが含まれている null 以外のデリゲートインスタンスが呼び出されると、指定されたのと同じ引数を使用して1つのメソッドが呼び出され、参照先のメソッドと同じ値が返されます。 (デリゲート呼び出しの詳細については、「[デリゲートの呼び出し](expressions.md#delegate-invocations)」を参照してください)。このようなデリゲートの呼び出し中に例外が発生し、その例外が呼び出されたメソッド内でキャッチされない場合、そのメソッドが直接を呼び出したかのように、デリゲートを呼び出したメソッドで例外の catch 句の検索が続行されます。デリゲートが参照されたメソッド。

呼び出しリストに複数のエントリが含まれているデリゲートインスタンスの呼び出しは、呼び出しリスト内の各メソッドを同期的に順番に呼び出すことによって続行されます。 を呼び出す各メソッドには、デリゲートインスタンスに指定されたものと同じ引数セットが渡されます。 このようなデリゲート呼び出しに参照パラメーター ([参照パラメーター](classes.md#reference-parameters)) が含まれている場合、各メソッド呼び出しは同じ変数への参照を使用して発生します。呼び出しリストの1つのメソッドによってその変数に加えられた変更は、呼び出しリストの下位にあるメソッドから参照できます。 デリゲート呼び出しに出力パラメーターまたは戻り値が含まれている場合、最終的な値はリスト内の最後のデリゲートの呼び出しから取得されます。

このようなデリゲートの呼び出しの処理中に例外が発生し、呼び出されたメソッド内で例外がキャッチされない場合は、デリゲートを呼び出したメソッドで例外の catch 句を検索し、すべてのメソッドをさらに下に移動します。呼び出しリストは呼び出されません。

値が null のデリゲートインスタンスを呼び出そうとすると、`System.NullReferenceException` 型の例外が発生します。

次の例は、デリゲートのインスタンス化、結合、削除、および呼び出しを行う方法を示しています。

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

ステートメント `cd3 += cd1;` に示されているように、デリゲートは呼び出しリスト内に複数回存在できます。 この場合、単に1回だけ呼び出されます。 このような呼び出しリストでは、そのデリゲートが削除されると、呼び出しリストで最後に出現したものが実際に削除されたものになります。

最後のステートメントを実行する直前 (`cd3 -= cd1;`)、デリゲート `cd3` は空の呼び出しリストを参照します。 空のリストからデリゲートを削除しようとしている (または、空でないリストから存在しないデリゲートを削除する) と、エラーにはなりません。

生成される出力は次のとおりです。

```console
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
