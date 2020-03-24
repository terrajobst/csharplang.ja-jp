---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483488"
---
# <a name="suppress-emitting-of-localsinit-flag"></a>`localsinit` フラグの出力を抑制します。

* [x] が提案されています
* [] プロトタイプ: 開始されていません
* [] の実装: 開始されていません
* [] 仕様: 開始されていません

## <a name="summary"></a>まとめ
[summary]: #summary

`SkipLocalsInitAttribute` 属性を使用した `localsinit` フラグの生成の抑制を許可します。 

## <a name="motivation"></a>目的
[motivation]: #motivation


### <a name="background"></a>バックグラウンド
参照を含まない CLR 仕様のローカル変数は、VM/JIT によって特定の値に初期化されることはありません。 初期化せずにこのような変数から読み取ることはタイプセーフですが、それ以外の場合、動作は未定義で実装固有です。 通常、初期化されていないローカルには、スタックフレームによって現在使用されているメモリに残っていた値が含まれます。 これにより、非決定的な動作が発生し、バグを再現するのが困難になる可能性があります。 

ローカル変数を "割り当てる" には、次の2つの方法があります。 
- 値を格納するか、 
- `localsinit` フラグを指定することによって、ローカルメモリプールを強制的にゼロ初期化されないようにします。これには、ローカル変数と `stackalloc` データの両方が含まれます。    

初期化されていないデータは使用しないことをお勧めします。検証可能なコードでは使用できません。 フロー分析の手段によって証明される可能性もありますが、検証アルゴリズムを控えめにすることが許可されており、単に `localsinit` が設定されている必要があります。

以前C#のコンパイラでは、ローカルを宣言するすべてのメソッドに `localsinit` フラグが生成されました。

C#では、厳密な割り当て分析を使用します。これは、CLR 仕様C#で必要とされるよりも厳格です (ローカルのスコープを考慮する必要もあります) が、結果のコードが正式に検証可能であるとは限りません。
- CLR およびC#規則は、ローカルのを `out` 引数として渡すことが `use`であるかどうかには同意しません。
- 条件がC#わかっている場合 (定数伝達)、CLR とルールが条件分岐の処理に同意しない場合があります。
- CLR は、許可されているため、単に `localinits`を要求することもできます。  

### <a name="problem"></a>問題
高パフォーマンスのアプリケーションでは、強制ゼロ初期化のコストが顕著になる可能性があります。 `stackalloc` が使用されている場合は特に顕著です。

場合によっては、そのような初期化が後続の割り当てによって "強制終了" されるときに、JIT が個々のローカルの初期ゼロ初期化を解除することがあります。 一部の JITs には、このような最適化に制限があるわけではありません。 `stackalloc`には役立ちません。

問題が実際に発生していることを示すために、`IL` ローカル変数を含まないメソッドに `localsinit` フラグがないという既知のバグがあります。 このバグは、初期化コストを回避するために意図的に `stackalloc` をこのようなメソッドに配置することによって、ユーザーによって既に悪用されています。 しかし、`IL` ローカルがないことは、不安定なメトリックであり、codegen 戦略の変化によって異なる場合があります。 バグは修正する必要があり、ユーザーはフラグを非表示にするために、より文書化された信頼性の高い方法を使用する必要があります。 

## <a name="detailed-design"></a>詳細なデザイン

`localsinit` フラグを出力しないようにコンパイラに指示する方法として、`System.Runtime.CompilerServices.SkipLocalsInitAttribute` を指定できるようにします。
 
この結果、ローカルが JIT によってゼロに初期化されない可能性があります。これは、ほとんどの場合、 C#unobservable にあります。  
それに加えて、`stackalloc` データはゼロ初期化されません。 これは明らかに観測可能ですが、最もスタブのあるシナリオです。

許可および認識される属性ターゲットは次のとおりです: `Method`、`Property`、`Module`、`Class`、`Struct`、`Interface`、`Constructor`。 ただし、コンパイラでは、一覧表示されたターゲットで属性が定義されている必要はありません。また、属性が定義されているアセンブリにも注意してください。 

コンテナー (`class`、`module`、入れ子になったメソッドのメソッドを含む) で属性が指定されている場合、フラグは、コンテナー内に含まれるすべてのメソッドに影響します。

合成されたメソッドは、論理コンテナー/所有者からフラグを "継承" します。 

フラグは、実際のメソッド本体の codegen 戦略にのみ影響します。 つまり、 このフラグは、抽象メソッドには影響しません。メソッドのオーバーライド/実装には反映されません。

これは明示的に **_コンパイラ機能_** で **_あり、言語機能ではありません_** 。  
コンパイラのコマンドラインスイッチと同様に、この機能は特定の codegen 戦略の実装の詳細を制御します。 C#仕様で必要とされる必要はありません。

## <a name="drawbacks"></a>短所
[drawbacks]: #drawbacks

- 古い/その他のコンパイラは、属性に従わない場合があります。
属性は、互換性のある動作として無視されます。 パフォーマンスがわずかに低下する可能性があります。

- `localinits` フラグのないコードは、検証エラーをトリガーする場合があります。
この機能を要求するユーザーは、通常、検証可能性とは関係がありません。 
 
- 属性を個々のメソッドより高いレベルで適用すると、非ローカルの効果があります。これは `stackalloc` が使用されている場合に監視できます。 ただし、これは最も要求の厳しいシナリオです。

## <a name="alternatives"></a>代替
[alternatives]: #alternatives

- メソッドが `unsafe` コンテキストで宣言されている場合は、`localinits` フラグを省略します。 これにより、`stackalloc`の場合に、サイレントで危険な動作が決定的から非決定的に変わる可能性があります。

- 常に `localinits` フラグを省略します。
上記よりも悪くなります。

- メソッドの本体で `stackalloc` が使用されていない場合は、`localinits` フラグを省略します。
は、要求された最も多くのシナリオに対応せず、元に戻すオプションを指定せずにコードをオフにすることができます。

## <a name="unresolved-questions"></a>未解決の質問
[unresolved]: #unresolved-questions

- 属性がメタデータに実際に出力される必要がありますか。 

## <a name="design-meetings"></a>会議のデザイン

まだありません。 