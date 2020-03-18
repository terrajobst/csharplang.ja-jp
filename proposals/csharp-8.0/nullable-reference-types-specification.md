---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/26/2020
ms.locfileid: "79484106"
---
# <a name="nullable-reference-types-specification"></a>Null 許容の参照型の仕様

これは進行中の作業です。いくつかの部分が不足しているか、不完全です。 ***

## <a name="syntax"></a>構文

### <a name="nullable-reference-types"></a>null 許容参照型

Null 許容型の参照型には、null 許容値型の短い形式と同じ構文 `T?` ますが、対応する長い形式はありません。

この仕様では、現在の `nullable_type` 運用環境の名前が `nullable_value_type`に変更され、`nullable_reference_type` 運用環境が追加されます。

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

`nullable_reference_type` 内の `non_nullable_reference_type` は、null 非許容の参照型 (クラス、インターフェイス、デリゲート、または配列) であるか、null 非許容の参照型 (`class` 制約を通じて、または `object`以外のクラス) であることが制限されている型パラメーターである必要があります。

Null 許容の参照型は、次の位置では発生しません。

- 基底クラスまたはインターフェイスとして
- `member_access` の受信者として
- `object_creation_expression` 内の `type` として
- `delegate_creation_expression` 内の `delegate_type` として
- `is_expression`内の `type` として `catch_clause` または `type_pattern`
- 完全修飾インターフェイスメンバー名の `interface` として

Null 許容の注釈コンテキストが無効になっている `nullable_reference_type` で警告が示されます。

### <a name="nullable-class-constraint"></a>Nullable クラスの制約

`class` 制約には、null 許容の対応する `class?`があります。

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a>Null を許容しない演算子

修正後の `!` 演算子は、null 非厳格演算子と呼ばれます。

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

`primary_expression` は参照型である必要があります。  

後置 `!` 演算子には、ランタイムの影響はありません。基になる式の結果に評価されます。 唯一のロールは、式の null 状態を変更し、使用時に指定された警告を制限することです。

### <a name="nullable-implicitly-typed-local-variables"></a>暗黙的に型指定された null 許容のローカル変数

`var` は、参照型の注釈付きの型を推測します。
たとえば `var s = "";` では、`var` は `string?`として推論されます。

### <a name="nullable-compiler-directives"></a>Null 許容のコンパイラディレクティブ

`#nullable` ディレクティブは、null 値を許容する注釈と警告コンテキストを制御します。

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

`#pragma warning` ディレクティブが拡張され、null 許容の警告コンテキストを変更できるようになりました。また、既定で無効になっている場合でも、で個々の警告を有効にすることができます。

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

`pragma_warning_body` の新しい形式では、`warning_action`ではなく `nullable_action`が使用されていることに注意してください。

## <a name="nullable-contexts"></a>null 許容コンテキスト

ソースコードのすべての行には、 *null 許容の注釈コンテキスト*と*null 許容の警告コンテキスト*があります。 これらは、null 許容の注釈が有効かどうか、および null 値許容の警告を与えるかどうかを制御します。 特定の行の注釈コンテキストが*無効*または*有効に*なっています。 指定された行の警告コンテキストが*無効に*なっ*ているか、有効になって*います。

どちらのコンテキストも、プロジェクトレベル (ソースコードのC#外) で指定することも、`#nullable` プリプロセッサディレクティブを使用してソースファイル内の任意の場所で指定することもできます。 プロジェクトレベルの設定が指定されていない場合、既定では、両方のコンテキストが*無効*になります。

`#nullable` ディレクティブは、ソーステキスト内の注釈と警告コンテキストを制御し、プロジェクトレベルの設定よりも優先されます。

ディレクティブは、後続のコード行に対して制御するコンテキストを、別のディレクティブがオーバーライドするまで、またはソースファイルの末尾まで設定します。

ディレクティブの効果は次のとおりです。

- `#nullable disable`: null 許容の注釈と警告コンテキストを*無効*に設定します。
- `#nullable enable`: null 許容の注釈と警告コンテキストを*有効*に設定します
- `#nullable restore`: null 許容の注釈と警告コンテキストをプロジェクトの設定に復元します
- `#nullable disable annotations`: null 許容の注釈コンテキストを*無効*に設定します
- `#nullable enable annotations`: null 許容の注釈コンテキストを*有効*に設定します
- `#nullable restore annotations`: null 許容の注釈コンテキストをプロジェクト設定に復元します
- `#nullable disable warnings`: null 許容の警告コンテキストを*無効*に設定します
- `#nullable enable warnings`: null 許容の警告コンテキストを*有効*に設定します
- `#nullable restore warnings`: プロジェクト設定に null 許容の警告コンテキストを復元します

## <a name="nullability-of-types"></a>型の null 値の許容

指定された型は、*無関係*、 *null 値*、 *nullable* 、 *unknown*の4つの nullabilities のいずれかを持つことができます。 

潜在的な `null` 値が割り当てられている場合、 *null 値*および*不明な*型によって警告が発生する可能性があります。 ただし、*無関係*と*null 許容*型は "*null 割り当て*可能" であり、警告なしで `null` 値を割り当てることができます。 

*無関係*および*null 値*型は、警告なしで逆参照または割り当てできます。 ただし、null 値が*許容*される型と*不明な*型の値は "*null*を生成する" ものであり、適切な null チェックを行わずに逆参照または割り当てを行うと警告が発生する可能性があります。 

Null 値を生成する型の*既定の null 状態*は、"null になる可能性があります" です。 Null 以外の型の既定の null 状態は、"not null" です。

型の種類と、それが発生する null 許容の注釈コンテキストは、null 値を許容するかどうかを決定します。

- Null 値値型 `S` は常に*null 値*
- Null 許容型 `S?` は常に*null*値を許容します。
- *無効*な注釈コンテキストで `C` 注釈が付けられていない参照型は、*無関係*です。
- *有効な*注釈コンテキストで `C` 注釈が付けられていない参照型は、 *null 値*
- Null 許容の参照型 `C?` *null*値は許容されますが、*無効*な注釈コンテキストで警告が生成される可能性があります)

さらに、型パラメーターによって、制約が考慮されるようになります。

- すべての制約 (存在する場合) が null を生成する型 (null 値が*許容*されるか*不明*) であるか、または `class?` 制約が*不明*である `T` 型パラメーター。
- 型パラメーター `T`、少なくとも1つの制約が*無関係*または*null 値*のいずれかであるか、またはいずれかの `struct` または `class` 制約がです。
    - *無効*な注釈コンテキストでの*無関係*
    - *有効な*注釈コンテキスト内の*null 値*
- Null 許容型パラメーター `T?` `T`の制約の少なくとも1つが*無関係*または*null 値*であるか、または `struct` または `class` の制約の1つがです。
    - *無効*な注釈コンテキストでは null 値が*許容*されますが、警告が生成されます。
    - *enabled*注釈コンテキストで null 値を*許容*

型パラメーター `T`の場合、`T?` は、`T` が値型であることがわかっているか、参照型であることがわかっている場合にのみ許可されます。

### <a name="oblivious-vs-nonnullable"></a>無関係 vs null 値

型の最後のトークンがそのコンテキスト内にある場合、`type` は特定の注釈コンテキストで発生すると見なされます。

ソースコードで `C` 特定の参照型が無関係または null 値として解釈されるかどうかは、そのソースコードの注釈コンテキストに依存します。 ただし、いったん確立されると、その型の一部と見なされ、"これを使用して" 移動します (ジェネリック型引数の置換中など)。 型には `?` のような注釈がありますが、非表示です。

## <a name="constraints"></a>制約

Null 許容の参照型は、ジェネリック制約として使用できます。 さらに `object` が明示的な制約として有効になりました。 制約が存在しない場合は、(`object`ではなく) `object?` 制約に相当します `object` が、明示的な制約として `object?` は禁止されています。

`class?` は、"null 値が許容される可能性があります" という新しい制約で、`class` は "null 値 reference type" を表します。

型引数または制約の null 値の許容は、型が制約を満たしているかどうかに影響しません。ただし、現在のケースが既に存在する場合を除きます (null 許容値型は `struct` 制約を満たしません)。 ただし、型引数が制約の null 値許容の要件を満たしていない場合は、警告が表示されることがあります。

## <a name="null-state-and-null-tracking"></a>Null 状態と null 追跡

特定のソースの場所にあるすべての式には*null 状態*があり、null として評価される可能性があるかどうかが示されます。 Null 状態は、"not null" または "null の可能性があります" のいずれかです。 Null 状態は、null unsafe 変換と逆参照について警告を指定するかどうかを判断するために使用されます。

### <a name="null-tracking-for-variables"></a>変数の Null 追跡

変数またはプロパティを示す特定の式の場合、null 状態は、それらに対する割り当て、それらに対して実行されるテスト、およびそれらの間の制御フローに基づいて追跡されます。 これは、変数の代入を確実に追跡する方法に似ています。 追跡対象の式は、次の形式のものです。

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

ここで、識別子はフィールドまたはプロパティを表します。

***明確な代入に似た null 状態遷移の説明***

### <a name="null-state-for-expressions"></a>式の Null 状態

式の null 状態は、その形式と型、およびそれに関連する変数の null 状態から派生します。

### <a name="literals"></a>リテラル

`null` リテラルの null 状態は、"null になる可能性があります" です。 Null 値値型ではないことがわかっている型に変換される `default` リテラルの null 状態は、"null になる可能性があります" です。 他のリテラルの null 状態は、"not null" です。

### <a name="simple-names"></a>簡易名

`simple_name` が値として分類されていない場合、その null 状態は "not null" になります。 それ以外の場合は追跡対象の式であり、null 状態はこのソースの場所で追跡された null 状態になります。

### <a name="member-access"></a>メンバー アクセス。

`member_access` が値として分類されていない場合、その null 状態は "not null" になります。 それ以外の場合、追跡対象の式の場合、null 状態はこのソースの場所で追跡された null 状態になります。 それ以外の場合、null 状態はその型の既定の null 状態になります。

### <a name="invocation-expressions"></a>Invocation 式

特殊な null 動作に対して1つ以上の属性を使用して宣言されたメンバーを `invocation_expression` が呼び出す場合、null 状態はそれらの属性によって決定されます。 それ以外の場合、式の null 状態はその型の既定の null 状態になります。

### <a name="element-access"></a>要素アクセス

特殊な null 動作に対して1つ以上の属性を使用して宣言されたインデクサーを `element_access` が呼び出す場合、null 状態はそれらの属性によって決定されます。 それ以外の場合、式の null 状態はその型の既定の null 状態になります。

### <a name="base-access"></a>基本アクセス

`B` が外側の型の基本型を表している場合、`base.I` は `((B)this).I` と同じ null 状態であり、`base[E]` は `((B)this)[E]`と同じ null 状態になります。

### <a name="default-expressions"></a>既定式

`T` が null 値値型であることがわかっている場合、`default(T)` は null 以外の状態になります。 それ以外の場合、null 状態は "null の可能性があります" になります。

### <a name="null-conditional-expressions"></a>Null 条件式

`null_conditional_expression` の状態が null である可能性があります。

### <a name="cast-expressions"></a>キャスト式

キャスト式 `(T)E` がユーザー定義の変換を呼び出す場合、式の null 状態はその型の既定の null 状態になります。 それ以外の場合、`T` が null (*null*値が許容または*不明*) の場合、null 状態は "null になる可能性があります" になります。 それ以外の場合、null 状態は `E`の null 状態と同じになります。

### <a name="await-expressions"></a>Await 式

`await E` の null 状態は、その型の既定の null 状態です。

### <a name="the-as-operator"></a>`as` 演算子

`as` 式の状態が null である可能性があります。

### <a name="the-null-coalescing-operator"></a>Null 合体演算子

`E1 ?? E2` には、と同じ null 状態があり `E2`

### <a name="the-conditional-operator"></a>条件演算子

`E2` と `E3` の両方の null 状態が "not null" の場合、`E1 ? E2 : E3` の null 状態は "not null" になります。 それ以外の場合は、"null の可能性があります" になります。

### <a name="query-expressions"></a>クエリ式

クエリ式の null 状態は、その型の既定の null 状態です。

### <a name="assignment-operators"></a>代入演算子

暗黙の変換が適用された後、`E1 = E2` と `E1 op= E2` は `E2` と同じ null 状態になります。

### <a name="unary-and-binary-operators"></a>単項演算子と二項演算子

単項演算子または二項演算子が、特殊な null 動作で1つ以上の属性を使用して宣言されたユーザー定義演算子を呼び出す場合、null 状態はこれらの属性によって決定されます。 それ以外の場合、式の null 状態はその型の既定の null 状態になります。

***文字列とデリゲートに対するバイナリ `+` に特別なことはありますか。***

### <a name="expressions-that-propagate-null-state"></a>Null 状態を反映する式

`(E)`、`checked(E)`、および `unchecked(E)` はすべて `E`と同じ null 状態になります。

### <a name="expressions-that-are-never-null"></a>Null にならない式

次の式形式の null 状態は常に "not null" です。

- `this` アクセス
- 挿入文字列
- `new` 式 (オブジェクト、デリゲート、匿名オブジェクト、および配列作成式)
- `typeof` 式
- `nameof` 式
- 匿名関数 (匿名メソッドとラムダ式)
- null-寛容な式
- `is` 式

## <a name="type-inference"></a>型の推論

### <a name="type-inference-for-var"></a>`var` の型の推論

`var` で宣言されたローカル変数に対して推論される型は、初期化式の null 状態によって通知されます。

```csharp
var x = E;
```

`E` の型が null 許容の参照型で `C?`、`E` の null 状態が "not null" の場合、`x` に対して推論される型は `C`になります。 それ以外の場合、推論された型は `E`の型になります。

`x` に対して推論される型の null 値の許容属性は、型がその位置に明示的に指定されているかのように、前に説明したように、`var`の注釈コンテキストに基づいて決定されます。

### <a name="type-inference-for-var"></a>`var?` の型の推論

`var?` で宣言されたローカル変数に対して推論される型は、初期化式の null 状態には依存しません。

```csharp
var? x = E;
```

`E` の型 `T` が null 許容値型または null 許容の参照型である場合、`x` に対して推論される型は `T`です。 それ以外の場合、`T` が null 値値型である場合 `S` 推論される型は `S?`です。 それ以外の場合、`T` が null 値参照型である場合 `C` 推論される型は `C?`です。 それ以外の場合、宣言は無効です。

`x` に対して推論される型の null 値の許容属性は、常に null 値を*許容*します。

### <a name="generic-type-inference"></a>ジェネリック型の推論

ジェネリック型の推論は、推論された参照型が null 許容かどうかを判断するのに役立ちます。 これはベストエフォートであり、ではなく、それ自体が警告を生成することはありませんが、選択されたオーバーロードの推定型が引数に適用されると、null 許容の警告が発生する可能性があります。

型推論は、受信型の注釈コンテキストに依存しません。 代わりに、明示的に表現されている場合は、その独自の注釈コンテキストを取得する `type` が推論されます。 これにより、独自に作成したものの便宜を目的として、型の推定の役割が決まります。

より正確に言えば、推論された型引数の注釈のコンテキストは、`<...>` 型パラメーターリストが後に存在する可能性のあるトークンのコンテキストになります。つまり、呼び出されるジェネリックメソッドの名前。 このような呼び出しに変換されるクエリ式の場合、コンテキストは、呼び出しの生成元であるクエリ句の最初のコンテキストキーワードから取得されます。

### <a name="the-first-phase"></a>最初のフェーズ

Null 許容の参照型は、以下で説明するように、初期式からの境界にフローします。 さらに、2つの新しい種類の境界 (`null` と `default` が導入されています。 その目的は、入力式で `null` または `default` を実行することです。これにより、推論された型は、そうでない場合でも null 許容になります。 これは、推論プロセスで "nullness" を取得するように強化された null 許容*値*型に対しても機能します。

最初のフェーズで追加する境界の決定は、次のように強化されています。

引数 `Ei` に参照型がある場合、推論に使用される型 `U` は、`Ei` の null 状態と宣言された型によって異なります。
- 宣言された型が null 値参照型 `U0` または null 許容の参照型の場合 `U0?`
    - `Ei` の null 状態が "not null" の場合、`U` は `U0`
    - `Ei` の null 状態が "おそらく null" の場合、`U` は `U0?`
- それ以外の場合 `Ei` が宣言された型である場合、`U` はその型になります。
- それ以外の場合、`Ei` が `null` 場合、`U` は特殊なバインド `null`
- それ以外の場合、`Ei` が `default` 場合、`U` は特殊なバインド `default`
- それ以外の場合、推論は行われません。

### <a name="exact-upper-bound-and-lower-bound-inferences"></a>正確、上限、下限の推論

`U` 型*から*型 `V`*へ*の推論では、`V` が null 許容の参照型 `V0?`である場合、次の句では `V0` ではなく `V` が使用されます。
- `V` が固定されていない型の変数の1つである場合、`U` は、のように正確な値、上限、または下限として追加されます。
- それ以外の場合、`U` が `null` または `default`の場合、推論は行われません。
- それ以外の場合、`U` が null 許容の参照型 `U0?`である場合は、後続の句で `U` の代わりに `U0` が使用されます。

基本的に、いずれかの固定されていない型の変数に直接関連する null 値の許容属性は、その境界に保持されます。 逆に、ソースとターゲットの種類に再帰的に再帰する推論の場合、null 値の許容は無視されます。 これは、一致しない場合と一致しない場合がありますが、それ以外の場合は、オーバーロードが選択されて適用されると、後で警告が発行されます。

### <a name="fixing"></a>写真の

現在、この仕様では、複数の境界が相互に変換可能な id であるが異なる場合に何が起こるかを説明するのは適切ではありません。 これは、`object` と `dynamic`の間、要素名だけが異なるタプル型の間で、その型の間、および参照型の `C` と `C?` の間でも発生する可能性があります。

さらに、入力式から結果の型に "nullness" を伝達する必要があります。 

これらの処理を行うには、修正するフェーズを追加します。これは次のようになります。

1. すべての境界内のすべての型を候補として収集し、null 値を許容する参照型であるすべてから `?` を削除します。
2. 正確、下限、上限の要件に基づいて候補を排除する (`null` と `default` の範囲を維持する)
3. 他のすべての候補に暗黙的に変換されない候補を除外する
4. 残りの候補がすべての id を相互に変換していない場合、型の推定は失敗します。
5. 次に示すように、残りの候補を*マージ*します。
6. 結果として得られる候補が参照型または null 値値型であり、*すべて*の完全な*境界または下限の境界*が null 許容の値型、null 許容の参照型、`null` または `default`である場合、`?` が結果の候補に追加され、null 許容値型または参照型になります。

*マージ*は、2つの候補の種類の間で記述されます。 これは推移的なものであり、同じ最終的な結果を含む任意の順序で候補をマージできます。 2つの候補の型が相互に変換可能な id ではない場合、これは未定義です。

*Merge*関数は、次の2つの候補の型と方向 ( *+* または *-* ) を受け取ります。

- *Merge*(`T`、`T`、 *d*) = t
- *Merge (* `S`、`T?`、 *+* *) = merge (`S?`* 、`T`、 *+* ) = *merge*(`S`、`T`、 *+* )`?`
- *Merge (* `S`、`T?`、 *-* *) = merge (`S?`* 、`T`、 *-* ) = *merge*(`S`、`T`、 *-* )
- *Merge (* `C<S1,...,Sn>`、`C<T1,...,Tn>`、 *+* *) `C<`= merge (* `S1`、`T1`、 *d1*)`,...,`*merge*(`Sn`、`Tn`、 *dn*)`>`、 *where*
    - `C<...>` の `i`' th type パラメーターが共変である場合に *+* を `di` = 
    - `C<...>` の `i`' th type パラメーターが負またはインバリアントの場合は、`di` =  *-* 。
- *Merge (* `C<S1,...,Sn>`、`C<T1,...,Tn>`、 *-* *) `C<`= merge (* `S1`、`T1`、 *d1*)`,...,`*merge*(`Sn`、`Tn`、 *dn*)`>`、 *where*
    - `C<...>` の `i`' th type パラメーターが共変である場合に *-* を `di` = 
    - `C<...>` の `i`' th type パラメーターが負またはインバリアントの場合は、`di` =  *+* 。
- *Merge (* `(S1 s1,..., Sn sn)`、`(T1 t1,..., Tn tn)`、 *d* *) `(`= merge (* `S1`、`T1`、 *d)* `n1,...,`*merge*(`Sn`、 *`Tn`、d*) `nn)`、 *where*
    - `si` と `ti` が異なる場合、または両方が存在しない場合、`ni` は存在しません。
    - `si` と `ti` が同じ場合、`ni` は `si`
- *Merge*(`object`, `dynamic`) = *merge*(`dynamic`, `object`) = `dynamic`

## <a name="warnings"></a>警告

### <a name="potential-null-assignment"></a>可能性のある null 代入

### <a name="potential-null-dereference"></a>可能性のある null の逆参照

### <a name="constraint-nullability-mismatch"></a>制約の null 値の許容が一致しません

### <a name="nullable-types-in-disabled-annotation-context"></a>無効な注釈コンテキスト内の null 許容型

## <a name="attributes-for-special-null-behavior"></a>特殊な null 動作の属性


