---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483572"
---
# <a name="fixed-sized-buffers"></a>固定サイズのバッファー

* [x] が提案されています
* [] プロトタイプ: 開始されていません
* [] の実装: 開始されていません
* [] 仕様: 開始されていません

## <a name="summary"></a>まとめ
[summary]: #summary

固定サイズのバッファーをC#言語に宣言するための汎用的な安全な機構を提供します。

## <a name="motivation"></a>目的
[motivation]: #motivation

現在、ユーザーは、安全でないコンテキストで固定サイズのバッファーを作成することができます。 ただし、この場合は、ユーザーがポインターを処理し、手動で範囲チェックを実行する必要があります。また、サポートされている型の種類 (`bool`、`byte`、`char`、`short`、`int`、`long`、`sbyte`、`ushort`、`uint`、`ulong`、`float`、`double`) は限られています。

最も一般的な問題は、固定サイズのバッファーに安全なコードでインデックスを作成できないことです。 2番目の型を使用することはできません。

いくつかの小さな微調整で、任意の型をサポートする汎用の固定サイズのバッファーを提供し、安全なコンテキストで使用し、自動的に境界チェックを実行できます。

## <a name="detailed-design"></a>詳細なデザイン
[design]: #detailed-design

1つは、次のものを使用して、安全な固定サイズのバッファーを宣言することです。

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

宣言は、次のようなコンパイラによって内部表現に変換されます。

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

このような固定サイズのバッファーでは `fixed`を使用する必要がなくなるため、任意の要素の型を許可することが理にかなっています。  

> 注: `fixed` は引き続きサポートされますが、要素型が `blittable` の場合のみ

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

* 旧バージョンとの互換性に関するいくつかの問題がある場合もありますが、既存の固定サイズのバッファーがプリミティブ型の選択でのみ機能する場合は、ユーザーが固定バッファーをとして処理した場合に、コンパイラが "単に機能している" ままになる可能性があります。pointer.
* 互換性のないコンストラクトでは、古いコンパイラのフィールドを非表示にするために、若干異なる `v2` エンコードを使用する必要がある場合があります。
* パッキングは、ジェネリック型の IL 仕様では適切に定義されていません。 このアプローチは機能しますが、ドキュメントに記載されていない動作に境界ます。 このドキュメントを文書化し、他の JITs と同じ Mono が動作することを確認してください。
* すべての長さに対して個別の型を指定する (サポートされている場合、`readonly` フィールドに別の型を指定すると、メタデータに影響します)。 この値は、特定のアプリのさまざまなサイズの配列の数によってバインドされます。
* `ref` の数値演算は、安全ではないため、正式に検証できません。 検証規則を更新して、使用に問題がないことを確認する必要があります。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

手動で構造体を宣言し、unsafe コードを使用してインデクサーを構築します。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- `readonly`を許可する必要がありますか。  (readonly インデクサーを含む)
- 配列初期化子を許可する必要がありますか。
- `fixed` キーワードは必須ですか?
- `foreach`?
- 構造体のインスタンスフィールドのみ

## <a name="design-meetings"></a>会議のデザイン

この提案に影響を与えるデザインメモにリンクし、それぞれの変更内容について1つの文で説明します。