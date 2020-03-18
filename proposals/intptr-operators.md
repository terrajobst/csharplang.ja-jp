---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483548"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a>`System.IntPtr` と `System.UIntPtr` では、演算子を公開する必要があります

* [x] が提案されています
* [] プロトタイプ: 開始されていません
* [] の実装: 開始されていません
* [] 仕様: 開始されていません

## <a name="summary"></a>まとめ
[summary]: #summary

CLR では、`System.IntPtr` 型と `System.UIntPtr` 型 (`native int`) の一連の演算子がサポートされています。 これらの演算子は、共通言語基盤仕様 (`ECMA-335`) の `III.1.5` で確認できます。 ただし、これらの演算子はでC#はサポートされていません。

`System.IntPtr` と `System.UIntPtr`でサポートされている演算子の完全なセットに対しては、言語サポートを提供する必要があります。 これらの演算子には、`Add`、`Divide`、`Multiply`、`Remainder`、`Subtract`、`Negate`、`Equals`、`Compare`、`And`、`Not`、`Or`、`XOr`、`ShiftLeft`、`ShiftRight`があります。

## <a name="motivation"></a>目的
[motivation]: #motivation

現在、ユーザーは、さまざまC#なツールやフレームワーク (`Xamarin`、`.NET Core`、`Mono`など) を使用して、複数のプラットフォームを対象とするアプリケーションを簡単に作成できます。

クロスプラットフォームコードを記述する場合は、特定の方法で特定のターゲットプラットフォームと対話する相互運用コードを記述することが必要になることがよくあります。 これには、グラフィックスコードの作成、システム API の呼び出し、または既存のネイティブライブラリとの対話が含まれます。

この相互運用コードでは、多くの場合、ハンドル、アンマネージメモリ、またはプラットフォーム固有のサイズの整数だけを処理する必要があります。

ランタイムは、`native int` (`System.IntPtr`) プリミティブ型と `native unsigned int` (`System.UIntPtr`) プリミティブ型で使用できる演算子のセットを定義することで、こののサポートを提供します。

C#ではこれらの演算子がサポートされていないため、ユーザーは問題を回避する必要があります。 多くの場合、コードの複雑さが増し、コードの保守性が低下します。

そのため、これらの要件をより適切にサポートするために、言語を進めるためにこれらの演算子のサポートを開始する必要があります。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

サポートされている演算子の完全なセットは、共通言語基盤仕様 (`ECMA-335`) の `III.1.5` で定義されています。 この仕様については、「 [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf)」を参照してください。

* これらの演算子の概要については、以下で簡単に説明します。
* CLI 仕様で定義されている検証不可能な演算子は記載されていないため、現在この提案に含まれていません (ただし、これらも考慮する価値があります)。
* キーワード (`nint` や `nuint`など) を指定したり、`System.IntPtr` と `System.UIntPtr` (0n など) に対してリテラルを宣言する方法を提供したりすることは、この提案には含まれていません (ただし、これらも考慮する価値があります)。

### <a name="unary-plus-operator"></a>単項プラス演算子

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a>単項マイナス演算子

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a>ビットごとの補数演算子

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a>キャスト演算子

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a>乗算演算子

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a>除算演算子

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a>剰余演算子

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a>加算演算子

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a>減算演算子

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a>シフト演算子

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a>整数の比較演算子

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a>整数の論理演算子

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

これらの演算子の実際の使用は、下位レベルのライブラリまたは相互運用コードを記述しているエンドユーザーに対しては、小規模である場合があります。 ほとんどのエンドユーザーは、ネイティブサイズの整数、ハンドル、および相互運用コードが抽象化されている下位レベルのライブラリ自体を使用している可能性があります。 そのため、オペレーター自体が必要になることはありません。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

必要な演算子を実装している場合は、直接 IL に記述します。 さらに、ランタイムは、フレームワークによって定義された演算子の組み込みサポートを提供するため、最終的なパフォーマンスをより適切に最適化することができます。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

設計のどの部分も未定ですか。

## <a name="design-meetings"></a>会議のデザイン

この提案に影響を与えるデザインメモにリンクし、それぞれの変更内容について1つの文で説明します。