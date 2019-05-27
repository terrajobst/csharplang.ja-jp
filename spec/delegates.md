---
ms.openlocfilehash: 994b22f5375d57cfc4c7537c64345a27ddf3e416
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193886"
---
# <a name="delegates"></a>デリゲート

デリゲートがシナリオの他の言語を有効にする、C、pascal 形式、および Modula--などの関数ポインターとしました。 ただし、C++ の関数ポインターとは異なりデリゲートはオブジェクト指向では完全に、C++ メンバー関数へのポインターとは異なり、デリゲートにはオブジェクトのインスタンスとメソッドの両方をカプセル化します。

デリゲートの宣言は、クラスから派生したクラスを定義します。`System.Delegate`します。 デリゲートのインスタンスでは、呼び出し可能なエンティティと呼ばれますがそれぞれ 1 つまたは複数のメソッドの一覧は、呼び出しリストをカプセル化します。 インスタンスのメソッドを呼び出し可能なエンティティから成るインスタンスとそのインスタンスのメソッド。 静的メソッドの呼び出し可能なエンティティは、メソッドだけで構成されます。 適切な一連の引数を含むデリゲート インスタンスを呼び出すと、指定した引数のセットを呼び出すときに、デリゲートの呼び出し可能なエンティティ。

Delegate インスタンスの興味深く有用なプロパティを把握したりしない気にカプセル化します。 メソッドのクラスこれらのメソッドを互換性のあることが重要な ([デリゲートの宣言](delegates.md#delegate-declarations)) で、デリゲートの型。 これは、ため、デリゲートを「匿名」の呼び出しに最適です。

## <a name="delegate-declarations"></a>デリゲートの宣言

A *delegate_declaration*は、 *type_declaration* ([入力宣言](namespaces.md#type-declarations)) 新しいデリゲート型を宣言します。

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

同じ修飾子をデリゲート宣言内で複数回のコンパイル時エラーになります。

`new`修飾子がデリゲートでのみ許可されている別の種類内で宣言されている場合、そのようなデリゲートを指定しますを非表示に継承されたメンバーを同じ名前で」の説明に従って[new 修飾子](classes.md#the-new-modifier)します。

`public`、 `protected`、 `internal`、および`private`修飾子はデリゲート型のアクセシビリティを制御します。 デリゲートの宣言が発生したコンテキストに応じてこれらの修飾子の一部は使用できません ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))。

デリゲートの型名が*識別子*します。

省略可能な*formal_parameter_list* 、デリゲートのパラメーターを指定し、 *return_type*デリゲートの戻り値の型を示します。

省略可能な*variant_type_parameter_list* ([バリアント型パラメーター リスト](interfaces.md#variant-type-parameter-lists))、デリゲート自体の型パラメーターを指定します。

デリゲート型の戻り値の型はいずれかである必要があります`void`、または出力セーフ ([差異の安全性](interfaces.md#variance-safety))。

デリゲート型のすべての正式なパラメーター型は、入力の安全なである必要があります。 また、`out`または`ref`パラメーターの型は出力セーフにもあります。 注も`out`パラメーターを基になる実行プラットフォームの制限により、入力セーフである必要があります。

C# のデリゲート型がそれに相当しない構造的に等しい名前を付けます。 具体的には、同じパラメーターを持つ 2 つの異なるデリゲート型では、一覧表示し、型が異なるデリゲート型と見なされますを返します。 ただし、2 つの異なるが、構造的に等しいデリゲート型のインスタンスが等しいと見なさ比較可能性があります ([デリゲート等値演算子](expressions.md#delegate-equality-operators))。

例:

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

メソッド`A.M1`と`B.M1`デリゲート型と互換性のある`D1`と`D2`があるため、型とパラメーター リストが同じ戻り値ただし、これらのデリゲート型が解除されるため、2 つのさまざまな種類を、。交換できます。 メソッド`B.M2`、 `B.M3`、および`B.M4`デリゲート型と互換性がない`D1`と`D2`異なる戻り値の型またはパラメーター リストがあるため、します。

その他のジェネリック型の宣言のように構築されたデリゲート型を作成する型引数を指定する必要があります。 パラメーターの型と構築されたデリゲート型の戻り値の型は、構築されたデリゲート型の対応する型引数をデリゲートの宣言で型パラメーターごとの代入することによって作成されます。 結果として得られる戻り値の型とパラメーターの型は、構築されたデリゲート型と互換性のあるどのような方法の決定に使用されます。 例:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

メソッド`X.F`デリゲート型と互換性が`Predicate<int>`メソッドと`X.G`デリゲート型と互換性が`Predicate<string>`します。

デリゲート型を宣言する唯一の方法を使用して、 *delegate_declaration*します。 デリゲートの型から派生したクラス型は、`System.Delegate`します。 デリゲート型は暗黙的に`sealed`ので、任意の型をデリゲート型から派生することはできません。 デリゲート以外のクラス型を派生させるにはも`System.Delegate`します。 なお`System.Delegate`デリゲートの型自体は、すべてのデリゲート型の派生元クラス型です。

C# のデリゲートの特別な構文を提供しますインスタンス化および呼び出し。 インスタンス化を除き、クラスまたはクラスのインスタンスに適用できるすべての操作も適用できますデリゲート クラスまたはインスタンスにそれぞれします。 特にのメンバーにアクセスすることは、`System.Delegate`通常メンバー アクセス構文を使用しました。

デリゲートのインスタンスによってカプセル化されるメソッドのセットには、呼び出しリストは呼び出されます。 デリゲートのインスタンスが作成されたとき ([デリゲートの互換性](delegates.md#delegate-compatibility)) 1 つのメソッドから、そのメソッドをカプセル化し、呼び出しリストには、1 つのエントリが含まれています。 ただし、2 つの null でないデリゲート インスタンスを組み合わせると、それぞれの呼び出しリストは--の順序で連結左のオペランドから - 右側のオペランドを 2 つ以上のエントリが含まれる新しい呼び出しリストを形成します。

バイナリを使用してデリゲートを組み合わせる`+`([加算演算子](expressions.md#addition-operator)) と`+=`演算子 ([複合代入](expressions.md#compound-assignment))。 デリゲートは、バイナリを使用して、デリゲートの組み合わせから削除できる`-`([減算演算子](expressions.md#subtraction-operator)) と`-=`演算子 ([複合代入](expressions.md#compound-assignment))。 デリゲートが等しいかどうかを比較できます ([デリゲート等値演算子](expressions.md#delegate-equality-operators))。

次の例は、いくつかのデリゲートのインスタンスを作成し、対応する呼び出しリストします。

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

ときに`cd1`と`cd2`はそれぞれ 1 つのメソッドをカプセル化をインスタンス化します。 ときに`cd3`は 2 つのメソッドの呼び出しリストをインスタンス化が`M1`と`M2`点で、注文します。 `cd4`呼び出しリストが含まれています`M1`、 `M2`、および`M1`点で、注文します。 最後に、`cd5`の呼び出しリストが含まれています`M1`、 `M2`、 `M1`、 `M1`、および`M2`点で、注文します。 (場合によっては、削除も) のデリゲートを組み合わせることの例については、次を参照してください。[デリゲート呼び出し](delegates.md#delegate-invocation)します。

## <a name="delegate-compatibility"></a>デリゲートの互換性

メソッドまたはデリゲート`M`は***互換性のある***デリゲート型で`D`次のすべてに該当する場合。

*  `D` `M`パラメーター、および内の各パラメーターの個数が同じ`D`同じ`ref`または`out`との対応するパラメーター修飾子`M`します。
*  各値パラメーターの (パラメーターなしで`ref`または`out`修飾子)、id 変換 ([恒等変換](conversions.md#identity-conversion)) または暗黙的な参照変換 ([の暗黙の参照変換](conversions.md#implicit-reference-conversions))パラメーター型から存在`D`で対応するパラメーターの型に`M`します。
*  各`ref`または`out`パラメーター、パラメーター入力`D`でパラメーターの型と同じです`M`します。
*  戻り値の型からの id 列または暗黙的な参照変換が存在する`M`の戻り値の型を`D`します。

## <a name="delegate-instantiation"></a>デリゲートのインスタンス化

によって、デリゲートのインスタンスが作成された、 *delegate_creation_expression* ([デリゲート作成式](expressions.md#delegate-creation-expressions)) またはデリゲート型に変換します。 新しく作成されたデリゲートのインスタンスは、いずれかを参照します。

*  参照される静的メソッド、 *delegate_creation_expression*、または
*  ターゲット オブジェクト (することはできませんが`null`) インスタンス メソッドで参照されていると、 *delegate_creation_expression*、または
*  他のデリゲート。

例:

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

インスタンス化すると、同じターゲット オブジェクトとメソッドを常にデリゲート インスタンス参照してください。 2 つのデリゲートを組み合わせると、または 1 つは、別の独自の呼び出しリスト持つ新しいデリゲート結果から削除する場合に、注意してください。結合または削除のデリゲートの呼び出しリストが変更されません。

## <a name="delegate-invocation"></a>デリゲートの呼び出し

C#、デリゲートの呼び出しの特別な構文を提供します。 呼び出しリスト持つにはには、1 つのエントリが含まれています。 null でないデリゲート インスタンスが呼び出されると、同じ引数が与えられているし、参照先と同じ値を返しますでは、1 つのメソッドが呼び出されますメソッドにします。 (を参照してください[デリゲートの呼び出し](expressions.md#delegate-invocations)デリゲートの呼び出しの詳細についてはします)。そのメソッドを直接呼び出した場合とに、デリゲートが呼び出されるメソッドに例外 catch 句の検索を続行場合は、このようなデリゲートの呼び出し中に例外が発生して、その例外が呼び出されたメソッド内でキャッチされない、委任するメソッドと呼ばれます。

順序で同期的に、呼び出しリストのメソッド呼び出すことによって、呼び出しリスト持つにはには、複数のエントリが含まれています。 デリゲートのインスタンスの呼び出しが行われます。 そのために呼び出される各メソッドには、同じように、デリゲート インスタンスに指定された引数のセットが渡されます。 このようなデリゲートの呼び出しには、参照パラメーターが含まれている場合 ([パラメーターを参照して](classes.md#reference-parameters))、同じ変数への参照で各メソッドの呼び出しが行われます呼び出しリスト内の 1 つのメソッドでは、その変数への変更になります。呼び出しリスト メソッドをさらに表示されます。 デリゲートの呼び出しには、出力パラメーターまたは戻り値が含まれている場合、最終的な値は、一覧の最後のデリゲートの呼び出しから取得されます。

場合は、このようなデリゲートの呼び出しの処理中に例外が発生して、その例外が呼び出されたメソッド内でキャッチされない、デリゲートを呼び出したメソッドで例外 catch 句の検索を続行して、メソッドの後の部分呼び出しリストには呼び出されません。

値が null の結果の種類の例外では、デリゲート インスタンスを起動しようとしています。`System.NullReferenceException`します。

次の例では、インスタンス化、結合、削除、およびデリゲートを呼び出す方法を示します。

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

ステートメントに示すように`cd3 += cd1;`デリゲートで使用できる呼び出しリストを複数回です。 この場合は、単に呼び出されます 1 回出現ごと。 このなどの呼び出しリストをそのデリゲートを削除すると、呼び出しリストで、最後に見つかったが実際に削除します。

最後のステートメントの実行前にすぐに`cd3 -= cd1;`、デリゲート`cd3`空の呼び出しリストを参照します。 空のリストからデリゲートを削除する (または空のリストから、存在しないデリゲートを削除する) をしようとしていますが、エラーではありません。

生成される出力は次のとおりです。

```
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
