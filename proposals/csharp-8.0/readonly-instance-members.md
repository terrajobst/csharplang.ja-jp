---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79484370"
---
# <a name="readonly-instance-members"></a>Readonly インスタンスのメンバー

Championed の問題: <https://github.com/dotnet/csharplang/issues/1710>

## <a name="summary"></a>まとめ
[summary]: #summary

インスタンスメンバーが状態を変更しないように指定するのと同じ `readonly struct` 方法で、構造体で個別のインスタンスメンバーを指定する方法を提供します。

`readonly instance member`! = `pure instance member`であることに注意してください。 `pure` インスタンスメンバーは、状態が変更されないことを保証します。 `readonly` インスタンスメンバーは、インスタンスの状態が変更されないことのみを保証します。

`readonly struct` のインスタンスメンバーはすべて、暗黙的に `readonly instance members`と見なすことができます。 非 readonly 構造体で宣言された明示的な `readonly instance members` は、同じ方法で動作します。 たとえば、インスタンスメンバー (現在のインスタンスまたはインスタンスのフィールド) を呼び出したが、それ自体が非 readonly であった場合でも、非表示のコピーが作成されます。

## <a name="motivation"></a>目的
[motivation]: #motivation

現在、ユーザーは `readonly struct` 型を作成できます。これにより、コンパイラは、すべてのフィールドが readonly であることを強制します。また、拡張によって、インスタンスメンバーが状態を変更することはありません。 ただし、アクセス可能なフィールドを公開する既存の API がある場合や、変化するメンバーと変化しないメンバーが混在している場合があります。 このような状況では、型を `readonly` としてマークすることはできません (これは互換性に影響する変更です)。

これは通常、`in` パラメーターの場合を除き、あまり影響を与えません。 読み取り専用でない構造体に対して `in` パラメーターを使用すると、呼び出しによって内部状態が変更されないことを保証できないため、コンパイラはインスタンスメンバーの呼び出しごとにパラメーターのコピーを作成します。 これにより、構造体を値によって直接渡された場合よりも、多数のコピーとパフォーマンスが悪化する可能性があります。 例については、 [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)で次のコードを参照してください。

非表示のコピーが発生する可能性があるその他のシナリオとして、`static readonly fields` と `literals`があります。 将来サポートされている場合、`blittable constants` は同じボートで終了します。つまり、構造体が `readonly`としてマークされていない場合、すべてのユーザーが (インスタンスメンバーの呼び出し時に) 完全コピーを行うことができます。

## <a name="design"></a>デザイン
[design]: #design

インスタンスメンバーが `readonly` であることをユーザーが指定し、インスタンスの状態を変更しないようにします (もちろん、コンパイラによって実行されるすべての適切な検証を使用します)。 次に例を示します。

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

Readonly は、アクセサーで `this` が変換されないことを示すために、プロパティアクセサーに適用できます。 次の例では、これらのアクセサーはメンバーフィールドの状態を変更しますが、メンバーフィールドの値は変更しないため、readonly setter を持ちます。

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

プロパティの構文に `readonly` が適用されている場合は、すべてのアクセサーが `readonly`ことを意味します。

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

Readonly は、包含する型を変化させないアクセサーにのみ適用できます。

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

Readonly は、一部の自動実装プロパティに適用できますが、意味のある効果はありません。 コンパイラは、`readonly` キーワードが存在するかどうかにかかわらず、自動実装されたすべての getter を readonly として扱います。

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

Readonly は、フィールドに似たイベントではなく、手動で実装されたイベントに適用できます。 個別のイベントアクセサー (追加/削除) に Readonly を適用することはできません。

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

その他の構文例を次に示します。

* 式の形式のメンバー: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`
* ジェネリック制約: `public static readonly void GenericMethod<T>(T value) where T : struct { }`

コンパイラは、通常どおりにインスタンスメンバーを出力し、さらに、インスタンスメンバーが状態を変更しないことを示すコンパイラ認識属性を出力します。 これにより、非表示の `this` パラメーターが `ref T`ではなく `in T` になります。

これにより、コンパイラがコピーを作成しなくても、ユーザーは安全にインスタンスメソッドを呼び出すことができます。

制限事項は次のとおりです。

* `readonly` 修飾子は、静的メソッド、コンストラクター、またはデストラクターには適用できません。
* `readonly` 修飾子はデリゲートに適用できません。
* `readonly` 修飾子は、クラスまたはインターフェイスのメンバーには適用できません。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

現在、`readonly struct` メソッドと同じ欠点があります。 一部のコードでは、非表示のコピーが発生する可能性があります。

## <a name="notes"></a>メモ
[notes]: #notes

属性または別のキーワードを使用することもできます。

この提案は、`functional purity` または `constant expressions`(またはその両方) に関連しています (ただし、これらには既存の提案がいくつかあります)。
