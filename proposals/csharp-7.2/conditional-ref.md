---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483668"
---
# <a name="conditional-ref-expressions"></a>条件付き参照式

参照変数を1つまたは別の式にバインドするパターンは、条件付きC#で現在は表現できません。

一般的な回避策は、次のようなメソッドを導入することです。

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

これは、すべての引数が呼び出しサイトで評価される必要があるため、三項の正確な置換ではないことに注意してください。

次のものは期待どおりに機能しません。

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

提案された構文は次のようになります。

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

上記の "Choice" の試行は、ref 三項を使用して_正しく_記述することができます。

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

選択肢との違いは、結果と代替式が_本当_に条件付きでアクセスされるため、クラッシュが発生しないことが ```arr == null```

三項参照は、代替と結果の両方が refs である三項です。 結果または代替のオペランドが左辺値である必要があります。 また、結果と代替として、相互に変換できる id を持つ型が必要になります。

式の型は、通常の三項の型と同様に計算されます。 つまり、 結果と代替の id が変換可能であるものの、型が異なる場合は、既存の型マージルールが適用されます。

セーフツーリターンは、条件付きオペランドからの控えめと見なされます。 どちらかが安全でない場合は、を返すことは安全ではありません。

Ref 三項は左辺値であるため、参照渡しで渡すことができます。

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

左辺値である場合は、に割り当てることもできます。 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

Ref 三項は、通常の (非 ref) コンテキストでも使用できます。 標準の三項を使用することもできるので、一般的ではありません。

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

実装に関する注意事項: 

実装の複雑さは、中程度から大規模なバグ修正のサイズであるように見えます。 -I. E コストがかかりません。
構文や解析を変更する必要はないと思います。
メタデータまたは相互運用には影響しません。 この機能は完全に式ベースです。
デバッグ/PDB には影響しません
