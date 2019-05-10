---
ms.openlocfilehash: 75fcd5b00ea5cac218a9f7809c53b179df97825c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488946"
---
# <a name="exceptions"></a>例外

C# での例外システム レベルとアプリケーション レベルの両方を処理する構造化、統一された、およびタイプ セーフな方法を使用するエラー条件。 C# の例外処理機構は、いくつかの重要な相違点、C に非常に似ています。

*  C# の場合は、すべての例外をから派生したクラス型のインスタンスで表す必要がある`System.Exception`します。 C++ では、例外を表す任意の型の任意の値を使用できます。
*  C# の finally ブロック ([try ステートメント](statements.md#the-try-statement)) 通常の実行と例外条件の両方で実行される終了コードを記述するために使用できます。 このようなコードは C++ で記述するコードを複製しなくても困難です。
*  C# の場合は、オーバーフロー、0 による除算、および null の逆参照などのシステム レベルの例外は、適切に定義の例外クラスとアプリケーション レベルのエラー状態と同等にします。

## <a name="causes-of-exceptions"></a>例外の原因

2 つの方法では、例外をスローできます。

*  A`throw`ステートメント ([throw ステートメント](statements.md#the-throw-statement)) すぐに、無条件で例外をスローします。 制御がすぐに次のステートメントに到達しません、`throw`します。
*  C# のステートメントと式の処理中に発生する特定の例外的な条件では、特定の状況で例外が発生、操作を正常に終了できない場合。 たとえば、整数除算演算を ([除算演算子](expressions.md#division-operator)) がスローされます、`System.DivideByZeroException`分母が 0 の場合。 参照してください[共通例外クラス](exceptions.md#common-exception-classes)この方法で発生するさまざまな例外の一覧についてはします。

## <a name="the-systemexception-class"></a>System.Exception クラス

`System.Exception`クラスは、すべての例外の基本型。 このクラスでは、すべての例外を共有するいくつかの注目すべきプロパティがあります。

*  `Message` 型の読み取り専用プロパティは、`string`例外の原因の人間が判読できる説明を格納しています。
*  `InnerException` 型の読み取り専用プロパティは、`Exception`します。 現在の例外の原因となった例外をその値が null 以外の場合は、参照: catch ブロックで現在の例外が発生したは、処理、`InnerException`します。 それ以外の場合、その値が null の場合、この例外が別の例外によって原因がないことを示します。 この方法で連結例外オブジェクトの数は任意にできます。

これらのプロパティの値は、インスタンス コンス トラクターの呼び出しで指定できます`System.Exception`します。

## <a name="how-exceptions-are-handled"></a>例外の処理方法

例外の処理によって、`try`ステートメント ([try ステートメント](statements.md#the-try-statement))。

例外が発生したときに、システム検索、最も近い`catch`句を例外の実行時の型によって決定される例外を処理することができます。 字句外側の現在のメソッドを検索する最初に、`try`ステートメント、および try ステートメントの関連付けられている catch 句は、順番と見なされます。 現在のメソッドを呼び出したメソッドが構文的囲む検索失敗した場合、`try`ステートメントを囲むの現在のメソッドの呼び出しをポイントします。 この検索はまで継続されます、`catch`によっては、同じクラスまたは基本クラス、スローされる例外の実行時の型の例外クラスの名前を付け、現在の例外を処理できる句が見つかった。 A`catch`を例外クラスを指定しない句は、すべての例外を処理できます。

一致する catch 句が見つかったら、catch 句の最初のステートメントに制御を転送する、システムを準備します。 Catch 句の実行が開始する前に、システム最初に実行される、順番にいずれかの`finally`try ステートメントの詳細に関連付けられていた句はするよりも、例外をキャッチした入れ子になった。

一致する catch 句が見つからない場合は、次の 2 つのいずれかに発生します。

*  一致する catch 句の検索には、静的コンス トラクターに達すると ([静的コンス トラクター](classes.md#static-constructors)) または静的フィールド初期化子、`System.TypeInitializationException`が静的コンス トラクターの呼び出しをトリガーした時点でスローされます。 内部例外、`System.TypeInitializationException`最初にスローされた例外が含まれています。
*  一致する catch 句の検索では、最初のスレッドを開始するコードに達すると、スレッドの実行が終了します。 このような終了の影響は、実装定義です。

デストラクターの実行中に発生する例外は、特に注意する必要があります。 デストラクターの実行中に例外が発生して、その例外はキャッチされず場合、は、そのデストラクターの実行を終了し、(ある場合) は、基底クラスのデストラクターが呼び出されます。 基底クラスが存在しない場合 (の場合と同様、`object`型) または基底クラスのデストラクターがないかどうかは、例外は破棄されます。

## <a name="common-exception-classes"></a>一般的な例外クラス

特定の c# 操作によっては、次の例外がスローされます。

|                                      |                |
|--------------------------------------|----------------|
| `System.ArithmeticException`         | 算術演算中に発生する例外 (`System.DivideByZeroException`、`System.OverflowException` など) の基底クラスです。 | 
| `System.ArrayTypeMismatchException`  | 格納される要素の実際の型は、配列の実際の型と互換性がないために、配列への格納が失敗したときにスローされます。 | 
| `System.DivideByZeroException`       | 整数値を 0 で除算しようとすると、発生したときにスローされます。 | 
| `System.IndexOutOfRangeException`    | 0 未満か、配列の境界の外側であるインデックスを使用して、配列のインデックスを作成するとスローされます。 | 
| `System.InvalidCastException`        | 実行時に基本型またはインターフェイスから派生型への明示的な変換が失敗したときにスローされます。 | 
| `System.NullReferenceException`      | 場合にスローされる、`null`参照が参照先オブジェクトを必要とする方法で使用されています。 | 
| `System.OutOfMemoryException`        | メモリを割り当てようとした場合にスローされます (を使用して`new`) が失敗します。 | 
| `System.OverflowException`           | `checked` コンテキストで算術演算がオーバーフローしたときにスローされます。 | 
| `System.StackOverflowException`      | 保留中のメソッドの呼び出しが多すぎる; することで実行スタックが空になった場合にスローされます。非常に深いか、無限再帰の通常気付く。 | 
| `System.TypeInitializationException` | 静的コンス トラクターが存在し、例外をスローする場合にスロー`catch`キャッチする句が存在します。 | 
