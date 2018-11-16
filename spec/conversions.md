# <a name="conversions"></a>変換

A***変換***式を特定の型として扱うことができます。 変換を別の種類を持つものとして扱う特定の型の式が発生する可能性があります。 または型を取得する型のない式があります。 変換***暗黙的な***または***明示的な***、および明示的なキャストが必要かどうかを指定します。 型からの変換、`int`入力`long`が暗黙的な型の式をその`int`型として暗黙的に処理できる`long`。 型からの逆の変換`long`入力`int`は明示的なため、明示的なキャストが必要です。

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

一部の変換は言語によって定義されます。 プログラムは、独自の型変換を定義も可能性があります ([ユーザー定義の変換](conversions.md#user-defined-conversions))。

## <a name="implicit-conversions"></a>暗黙の変換

次の変換は、暗黙的な変換として分類されます。

*  恒等変換
*  暗黙的な数値変換
*  列挙体の暗黙的な変換です。
*  Null 許容型の暗黙的な変換
*  Null リテラルの変換
*  暗黙的な参照変換
*  ボックス化変換
*  動的の暗黙的な変換
*  定数式が暗黙的な変換
*  ユーザー定義の暗黙的な変換
*  匿名関数の変換
*  メソッド グループ変換

暗黙的な変換は、さまざまな関数メンバーの呼び出しをなどの状況で発生することができます ([コンパイル時の動的なオーバー ロードの解決チェック](expressions.md#compile-time-checking-of-dynamic-overload-resolution))、キャスト式 ([キャスト式](expressions.md#cast-expressions))、割り当て ([代入演算子](expressions.md#assignment-operators))。

定義済みの暗黙的な変換では、常に成功しがスローされる例外は発生しません。 適切にデザインされたユーザー定義の暗黙的な変換では、これらの特性もを示す必要があります。

型の変換のための`object`と`dynamic`同等と見なされます。

ただし、動的な変換 ([動的の暗黙的な変換](conversions.md#implicit-dynamic-conversions)と[動的の明示的な変換](conversions.md#explicit-dynamic-conversions)) 型の式にのみ適用されます`dynamic`([動的な型](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Id 変換

Id 変換は、任意の型から同じ型に変換します。 この変換には、その型に変換可能であるエンティティを既に必要な型を持つことが言えますようにが存在します。

*  Id の間で変換があるオブジェクトと動的同等と見なされるため、`object`と`dynamic`、間のすべての出現を置換するときに、同じ構築された型および`dynamic`で`object`。

### <a name="implicit-numeric-conversions"></a>暗黙的な数値変換

暗黙的な数値変換は次のとおりです。

*  `sbyte`に`short`、 `int`、 `long`、 `float`、 `double`、または`decimal`します。
*  `byte`に`short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `float`、 `double`、または`decimal`します。
*  `short`に`int`、 `long`、 `float`、 `double`、または`decimal`します。
*  `ushort`に`int`、 `uint`、 `long`、 `ulong`、 `float`、 `double`、または`decimal`します。
*  `int`に`long`、 `float`、 `double`、または`decimal`します。
*  `uint`に`long`、 `ulong`、 `float`、 `double`、または`decimal`します。
*  `long`に`float`、 `double`、または`decimal`します。
*  `ulong`に`float`、 `double`、または`decimal`します。
*  `char`に`ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `float`、 `double`、または`decimal`します。
*  `float`に`double`します。

変換`int`、 `uint`、 `long`、または`ulong`に`float`との間`long`または`ulong`に`double`により、有効桁数が失われる可能性がありますが、1 桁の損失しない原因は。 その他の暗黙的な数値変換情報は失われません。

暗黙的な変換がない、`char`型であるために、その他の整数型の値は自動的に変換されない、`char`型。

### <a name="implicit-enumeration-conversions"></a>列挙体の暗黙的な変換

列挙体の暗黙的な変換を許可、 *decimal_integer_literal* `0`いずれかに変換する*enum_type*しに*nullable_type*が基になる型は、 *enum_type*します。 後者の場合、変換は、基に変換することによって評価*enum_type*結果をラップし、([null 許容型](types.md#nullable-types))。

### <a name="implicit-interpolated-string-conversions"></a>補間文字列の暗黙的な変換

暗黙の補間文字列変換により、 *interpolated_string_expression* ([文字列補間](expressions.md#interpolated-strings)) に変換する`System.IFormattable`または`System.FormattableString`(を実装する`System.IFormattable`).

この変換が適用されるときに文字列値はありません補間文字列から構成されます。 代わりに、インスタンスの`System.FormattableString`作成は、その詳細に説明[文字列補間](expressions.md#interpolated-strings)します。

### <a name="implicit-nullable-conversions"></a>Null 許容型の暗黙的な変換

Null 非許容値型を操作する定義済みの暗黙的な変換は、これらの型の null 許容の形式でも使用できます。 定義済みの暗黙的な id と null 非許容値型から変換する数値変換の各`S`null 非許容値型に`T`、次の暗黙的な null 許容型の変換が存在します。

*  暗黙的な変換`S?`に`T?`します。
*  暗黙的な変換`S`に`T?`します。

Null 許容型の暗黙的な変換の評価がから基になる変換に基づく`S`に`T`次のように進みます。

*  場合は、null 許容型の変換から`S?`に`T?`:
    * 元の値が null の場合 (`HasValue`プロパティが false)、結果は、型の null 値`T?`します。
    * ラップ解除とそれ以外の場合、変換の評価は`S?`に`S`からの変換を基になると、その後`S`に`T`、その後に、ラップ ([null 許容型](types.md#nullable-types))から`T`に`T?`します。

*  場合は、null 許容型の変換から`S`に`T?`から基になる変換と変換の評価は`S`に`T`からの折り返しを続けて`T`に`T?`します。

### <a name="null-literal-conversions"></a>Null リテラルの変換

暗黙的な変換が存在する、`null`リテラルを任意の null 許容型。 この変換には、null 値が生成されます ([null 許容型](types.md#nullable-types)) の指定された null 許容型。

### <a name="implicit-reference-conversions"></a>暗黙的な参照変換

暗黙の参照変換は次のとおりです。

*  いずれかから*reference_type*に`object`と`dynamic`します。
*  いずれかから*class_type* `S`いずれかに*class_type* `T`に用意されている`S`から派生`T`します。
*  いずれかから*class_type* `S`いずれかに*interface_type* `T`に用意されている`S`実装`T`します。
*  いずれかから*interface_type* `S`いずれかに*interface_type* `T`に用意されている`S`から派生`T`します。
*  *Array_type* `S`要素型が`SE`を*array_type* `T`要素型が`TE`true は、次のすべての提供。
    * `S` `T`要素の型のみが異なります。 つまり、`S`と`T`同じ次元数を持ちます。
    * 両方`SE`と`TE`は*reference_type*秒。
    * 暗黙の参照変換が存在する`SE`に`TE`します。
*  いずれかから*array_type*に`System.Array`とインターフェイスを実装します。
*  1 次元配列型から`S[]`に`System.Collections.Generic.IList<T>`とからの暗黙的な id または参照変換が提供される、基本インターフェイス`S`に`T`します。
*  いずれかから*delegate_type*に`System.Delegate`とインターフェイスを実装します。
*  いずれかに null リテラルから*reference_type*します。
*  いずれかから*reference_type*を*reference_type* `T`への暗黙的な id または参照変換がある場合、 *reference_type* `T0`と`T0`が恒等変換を`T`します。
*  いずれかから*reference_type* 、インターフェイスまたはデリゲート型に`T`インターフェイスまたはデリゲート型への暗黙的な id または参照変換がある場合`T0`と`T0`(の差異に変換できるは[分散変換](interfaces.md#variance-conversion)) に`T`します。
*  使用する暗黙的な変換では、参照型にすることがわかっているパラメーターを入力します。 参照してください[型パラメーターを使用する暗黙的な変換](conversions.md#implicit-conversions-involving-type-parameters)型パラメーターを使用する暗黙的な変換の詳細についてはします。

暗黙の参照変換が変換の間で*reference_type*を常に成功することが保証され、そのため、実行時のチェックは必要ありません。

参照変換、暗黙的または明示的には、変換されるオブジェクトの参照 id を変更することはありません。 つまり、参照変換は、参照の種類を変更することが、変更しません、型または参照されるオブジェクトの値。

### <a name="boxing-conversions"></a>ボックス化変換

ボックス化変換では、 *value_type*参照型に暗黙的に変換します。 いずれかからボックス化変換が存在する*non_nullable_value_type*に`object`と`dynamic`を`System.ValueType`しに*interface_type*によって実装される、 *non_nullable_value_type*します。 さらに、 *enum_type*型に変換できる`System.Enum`します。

ボックス化変換が存在する、 *nullable_type* 、参照型をボックス化変換場合にのみが存在する、基礎となる*non_nullable_value_type*参照型にします。

値の型がインターフェイス型にボックス化変換`I`かどうかは、インターフェイス型にボックス化変換`I0`と`I0`が恒等変換を`I`します。

値の型がインターフェイス型にボックス化変換`I`、インターフェイスまたはデリゲート型にボックス化変換がある場合`I0`と`I0`差異に変換できるは ([分散変換](interfaces.md#variance-conversion))に`I`.

値をボックス化、 *non_nullable_value_type*オブジェクト インスタンスの割り当てとコピーから成る、 *value_type*値をそのインスタンスにします。 構造体は、型にボックス化される`System.ValueType`すべての構造体の基本クラスでは、([継承](structs.md#inheritance))。

値をボックス化、 *nullable_type*次のように進みます。

*  元の値が null の場合 (`HasValue`プロパティが false)、対象の型の参照を null になります。
*  それ以外の場合、結果は、ボックス化されたへの参照を`T`ラップの解除と、元の値をボックス化によって生成されました。

ボックス化変換の詳細については[ボックス化変換](types.md#boxing-conversions)します。

### <a name="implicit-dynamic-conversions"></a>動的の暗黙的な変換

型の式から動的の暗黙的な変換が存在する`dynamic`任意の型を`T`します。 変換が動的にバインドされている ([動的バインド](expressions.md#dynamic-binding))、実行時に式の実行時の型からの暗黙的な変換が求められますことを意味する`T`します。 変換が見つからない場合は、実行時に例外がスローされます。

この暗黙の変換は、の先頭でアドバイスを一見違反に注意してください。[暗黙的な変換](conversions.md#implicit-conversions)暗黙的な変換では、例外は発生しません。 ただしない自体には、変換が、*検索*の変換、例外が発生しました。 実行時の例外のリスクは、動的バインドの使用に伴うです。 変換の動的バインドが望ましくない場合、式最初に変換できる`object`とし、目的の型。

次の例は、動的の暗黙的な変換を示しています。

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

割り当て`s2`と`i`両方で操作のバインディングが実行時まで中断されている、動的な暗黙的な変換を使用します。 実行時の型から実行時に、暗黙的な変換のシーク`d`  --  `string` --ターゲットの型にします。 変換が検出された`string`には`int`します。

### <a name="implicit-constant-expression-conversions"></a>定数式が暗黙的な変換

定数式が暗黙的な変換を次の変換を許可します。

*  A *constant_expression* ([定数式](expressions.md#constant-expressions)) 型の`int`型に変換できます`sbyte`、 `byte`、 `short`、 `ushort`、 `uint`、または`ulong`の値を提供、 *constant_expression*が変換先の型の範囲内です。
*  A *constant_expression*型の`long`型に変換できます`ulong`の値を提供、 *constant_expression*が負でないです。

### <a name="implicit-conversions-involving-type-parameters"></a>型パラメーターを伴う暗黙の変換

次の暗黙的な変換が特定の型パラメーターの存在`T`:

*  `T`効果的な基底クラスに`C`から`T`の任意の基本クラスを`C`との間`T`によって実装されるインターフェイスを任意に`C`します。 At 実行時に、if`T`値の型は、ボックス化変換と変換を実行します。 それ以外の場合、変換は暗黙的な参照変換または id 変換として実行されます。
*  `T`インターフェイス型に`I`で`T`の有効なインターフェイスのセットとの間`T`の任意の基本インターフェイスを`I`します。 At 実行時に、if`T`値の型は、ボックス化変換と変換を実行します。 それ以外の場合、変換は暗黙的な参照変換または id 変換として実行されます。
*  `T`型パラメーターに`U`に用意されている`T`異なります`U`([パラメーターの制約入力](classes.md#type-parameter-constraints))。 実行時に、if`U`値の型には`T`と`U`必ずしも同じ型では、変換は行われません。 の場合`T`値の型がボックス化変換と変換を実行します。 それ以外の場合、変換は暗黙的な参照変換または id 変換として実行されます。
*  Null リテラルをから`T`に用意されている`T`参照型があることがわかっています。
*  `T`参照型に`I`参照型への暗黙的な変換があるかどうかは`S0`と`S0`が恒等変換を`S`します。 実行時に、変換はへの変換と同様な実行`S0`します。
*  `T`インターフェイス型に`I`インターフェイスまたはデリゲート型への暗黙的な変換がある場合`I0`と`I0`差異に変換できるに`I`([分散変換](interfaces.md#variance-conversion)). At 実行時に、if`T`値の型は、ボックス化変換と変換を実行します。 それ以外の場合、変換は暗黙的な参照変換または id 変換として実行されます。

場合`T`参照型がわかっている ([パラメーターの制約入力](classes.md#type-parameter-constraints))、上記の変換はすべて暗黙的な参照変換として分類 ([暗黙の参照変換](conversions.md#implicit-reference-conversions))。 場合`T`は上記の変換は参照型であるとわかっていない、ボックス化変換に分類されます ([ボックス化変換](conversions.md#boxing-conversions))。

### <a name="user-defined-implicit-conversions"></a>ユーザー定義の暗黙的な変換

省略可能な標準的な暗黙的な変換、省略可能な別の標準的な暗黙的な変換の後に、暗黙的な変換のユーザー定義演算子の実行後に、ユーザー定義の暗黙的な変換で構成されます。 ユーザー定義の暗黙的な変換を評価するための正確な規則については、「[暗黙的な変換のユーザー定義の処理](conversions.md#processing-of-user-defined-implicit-conversions)します。

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>匿名関数の変換とメソッド グループ変換

匿名関数とメソッドのグループ自体の型がありませんが、デリゲート型または式ツリー型に暗黙的に変換可能性があります。 匿名関数の変換がで詳しく説明されている[匿名関数の変換](conversions.md#anonymous-function-conversions)とで、メソッド グループ変換[メソッド グループ変換](conversions.md#method-group-conversions)します。

## <a name="explicit-conversions"></a>明示的な変換

次の変換は、明示的な変換として分類されます。

*  すべての暗黙的な変換です。
*  明示的な数値変換します。
*  明示的な列挙値の変換。
*  明示的な null 許容変換します。
*  明示的な参照変換します。
*  変換の明示的なインターフェイスです。
*  ボックス化解除変換します。
*  明示的な動的な変換
*  ユーザー定義の明示的な変換です。

明示的な変換はキャスト式で実行できます ([キャスト式](expressions.md#cast-expressions))。

明示的な変換のセットには、すべての暗黙的な変換が含まれています。 これは、冗長なキャスト式が許可されることを意味します。

暗黙的な変換ではない明示的な変換は、明示的なメリットを十分にさまざまな種類のドメイン間で変換を常に成功を証明することはできません、情報が失われる可能性があることがわかっている変換および変換表記法。

### <a name="explicit-numeric-conversions"></a>明示的な数値変換

明示的な数値変換は、変換を*numeric_type*間*numeric_type*を暗黙的な数値変換 ([暗黙的な数値変換](conversions.md#implicit-numeric-conversions))既に存在しません。

*  `sbyte`に`byte`、 `ushort`、 `uint`、 `ulong`、または`char`します。
*  `byte`に`sbyte`と`char`します。
*  `short`に`sbyte`、 `byte`、 `ushort`、 `uint`、 `ulong`、または`char`します。
*  `ushort`に`sbyte`、 `byte`、 `short`、または`char`します。
*  `int`に`sbyte`、 `byte`、 `short`、 `ushort`、 `uint`、 `ulong`、または`char`します。
*  `uint`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、または`char`します。
*  `long`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `ulong`、または`char`します。
*  `ulong`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、または`char`します。
*  `char`に`sbyte`、 `byte`、または`short`します。
*  `float`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、または`decimal`します。
*  `double`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、または`decimal`します。
*  `decimal`に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、または`double`します。

いずれかから変換することは常に明示的な変換では、すべての明示的および暗黙的な数値変換が含まれる、ため*numeric_type*他*numeric_type*キャスト式 (を使用します。[キャスト式](expressions.md#cast-expressions))。

明示的な数値変換では、情報が失われる可能性がありますかがスローされる例外が発生する可能性があります。 明示的な数値変換は、次のように処理されます。

*  整数型から別の整数型への変換では、処理は、オーバーフロー チェック コンテキストに依存 ([checked と unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) 移動変換では、配置します。
    * `checked`コンテキスト、ソース オペランドの値が変換先の型の範囲内にもスローする場合、変換に成功した、`System.OverflowException`ソース オペランドの値が変換先の型の範囲外にある場合。
    * `unchecked`コンテキスト変換常に成功し、次のように進みます。
        * 変換元の型が変換先の型より大きい場合、変換元の値はその "余分な" 最上位ビットを破棄することで切り詰められます。 結果は変換先の型の値として扱われます。
        * 変換元の型が変換先の型より小さい場合、変換元の値は変換先の型と同じサイズになるように符号かゼロが拡張されます。 変換元の型に符号が付いている場合は符号拡張が利用され、符号が付いていない場合はゼロ拡張が利用されます。 結果は変換先の型の値として扱われます。
        * 変換元の型が変換先の型と同じサイズの場合、変換元の値は変換先の型の値として扱われます。
*  変換の`decimal`、整数型にソース値が最も近い整数値を 0 方向に丸められ、この整数値の変換の結果になります。 結果の整数値が変換先の型の範囲外の場合、`System.OverflowException`がスローされます。
*  変換の`float`または`double`を整数型、処理は、オーバーフロー チェック コンテキストに依存 ([checked と unchecked 演算子](expressions.md#the-checked-and-unchecked-operators)) 変換が、配置。
    * `checked`コンテキスト、変換処理次のようにします。
        * オペランドの値が NaN、無限、またはの場合、`System.OverflowException`がスローされます。
        * それ以外の場合、ソース オペランドは最も近い整数値を 0 方向に丸められます。 場合、この整数値を変換先の型の範囲内でこの値は変換の結果です。
        * それ以外の場合は、`System.OverflowException` がスローされます。
    * `unchecked`コンテキスト変換常に成功し、次のように進みます。
        * オペランドの値が NaN、無限の変換の結果は、変換先の型の値が指定されていない場合。
        * それ以外の場合、ソース オペランドは最も近い整数値を 0 方向に丸められます。 場合、この整数値を変換先の型の範囲内でこの値は変換の結果です。
        * それ以外の場合、変換の結果は、先の型の値は指定されません。
*  変換の`double`に`float`、`double`値に丸められます、最も近い`float`値。 場合、`double`として表す値が小さすぎる、`float`結果が正のゼロまたは負のゼロ。 場合、`double`として表す値が大きすぎて、`float`結果は正の無限大または負の無限大になります。 場合、`double`値が NaN の結果は、NaN ではも場合、します。
*  変換の`float`または`double`に`decimal`、ソース値に変換されます`decimal`表現し、必要な場合、28 小数点後近似値に丸められます ([10 進数型](types.md#the-decimal-type)). ソース値が小さすぎてとして表すかどうか、`decimal`結果はゼロになります。 場合、元の値が NaN の場合、無限大、または大きすぎてとして表す、 `decimal`、`System.OverflowException`がスローされます。
*  変換の`decimal`に`float`または`double`、`decimal`値に丸められます、最も近い`double`または`float`値。 この変換は、有効桁数を失う可能性があります、中には、ことはありませんがスローされる例外が発生します。

### <a name="explicit-enumeration-conversions"></a>明示的な列挙値の変換

明示的な列挙値の変換は次のとおりです。

*  `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、または`decimal`任意に*enum_type*します。
*  いずれかから*enum_type*に`sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 `long`、 `ulong`、 `char`、 `float`、 `double`、または`decimal`します。
*  いずれかから*enum_type*他*enum_type*します。

2 つの型の間の明示的な列挙型変換が任意参加を扱うことにより処理される*enum_type*を基になる型として*enum_type*、してから、暗黙的または明示的なを実行します。結果として得られる型間の数値変換します。 たとえば、 *enum_type* `E`での種類を基になると`int`からの変換`E`に`byte`明示的な数値変換として処理されます ([明示的な数値変換](conversions.md#explicit-numeric-conversions)) から`int`に`byte`からの変換と`byte`に`E`暗黙的な数値変換として処理されます ([暗黙的な数値変換](conversions.md#implicit-numeric-conversions))`byte`に`int`します。

### <a name="explicit-nullable-conversions"></a>明示的な null 許容型の変換

***明示的な null 許容変換***定義済みの型の null 許容の形式でも使用する null 非許容値型を操作する明示的な変換を許可します。 Null 非許容値型から変換する定義済みの明示的な変換の各`S`null 非許容値型に`T`([恒等変換](conversions.md#identity-conversion)、 [暗黙的な数値変換](conversions.md#implicit-numeric-conversions)、[列挙体の暗黙的な変換](conversions.md#implicit-enumeration-conversions)、[明示的な数値変換](conversions.md#explicit-numeric-conversions)、および[列挙値の明示的な変換](conversions.md#explicit-enumeration-conversions))、以下null 許容型の変換が存在します。

*  明示的な変換を`S?`に`T?`します。
*  明示的な変換を`S`に`T?`します。
*  明示的な変換を`S?`に`T`します。

Null 許容型の変換の評価がから基になる変換に基づく`S`に`T`次のように進みます。

*  場合は、null 許容型の変換から`S?`に`T?`:
    * 元の値が null の場合 (`HasValue`プロパティが false)、結果は、型の null 値`T?`します。
    * ラップ解除とそれ以外の場合、変換の評価は`S?`に`S`からの変換を基になると、その後`S`に`T`、その後に、ラップをから`T`に`T?`します。
*  場合は、null 許容型の変換から`S`に`T?`から基になる変換と変換の評価は`S`に`T`からの折り返しを続けて`T`に`T?`します。
*  場合は、null 許容型の変換から`S?`に`T`からラップ解除すると、変換の評価は`S?`に`S`から基になる変換`S`に`T`。

値の場合、null 許容値のラップを解除しようとするは例外をスローことに注意してください。`null`します。

### <a name="explicit-reference-conversions"></a>明示的な参照変換

明示的な参照変換は次のとおりです。

*  `object`と`dynamic`他*reference_type*します。
*  いずれかから*class_type* `S`いずれかに*class_type* `T`に用意されている`S`の基本クラスである`T`します。
*  いずれかから*class_type* `S`いずれかに*interface_type* `T`に用意されている`S`シールおよび提供されない`S`実装しない`T`します。
*  いずれかから*interface_type* `S`いずれかに*class_type* `T`に用意されている`T`シールまたは指定されていない`T`実装`S`します。
*  いずれかから*interface_type* `S`いずれかに*interface_type* `T`に用意されている`S`から派生していない`T`します。
*  *Array_type* `S`要素型が`SE`を*array_type* `T`要素型が`TE`true は、次のすべての提供。
    * `S` `T`要素の型のみが異なります。 つまり、`S`と`T`同じ次元数を持ちます。
    * 両方`SE`と`TE`は*reference_type*秒。
    * 明示的な参照変換が存在する`SE`に`TE`します。
*  `System.Array`いずれかに、インターフェイスを実装および*array_type*します。
*  1 次元配列型から`S[]`に`System.Collections.Generic.IList<T>`とからの明示的な参照変換が提供される、基本インターフェイス`S`に`T`します。
*  `System.Collections.Generic.IList<S>` 1 次元の配列型をその基本インターフェイスと`T[]`からの明示的な id または参照変換はその`S`に`T`します。
*  `System.Delegate`いずれかに、インターフェイスを実装および*delegate_type*します。
*  参照型への参照型から`T`参照型への明示的な参照変換があるかどうかは`T0`と`T0`が恒等変換`T`します。
*  インターフェイスまたはデリゲートの型への参照型から`T`インターフェイスまたはデリゲート型への明示的な参照変換がある場合`T0`、`T0`差異に変換できるに`T`または`T`は変換できる分散`T0`([分散変換](interfaces.md#variance-conversion))。
*  `D<S1...Sn>`に`D<T1...Tn>`場所`D<X1...Xn>`が汎用デリゲート型では、`D<S1...Sn>`と互換性があるかと同じでない`D<T1...Tn>`、およびそれぞれの型パラメーター`Xi`の`D`次の保留。
    * 場合`Xi`しバリアントでは`Si`ヲェヒェケェ ・`Ti`します。
    * 場合`Xi`共変性を持つ、暗黙的または明示的な id または参照からの変換は`Si`に`Ti`。
    * 場合`Xi`は反変、`Si`と`Ti`は同一と両方の参照型。
*  使用する明示的な変換では、参照型にすることがわかっているパラメーターを入力します。 型パラメーターを使用する明示的な変換の詳細については、次を参照してください。[型パラメーターを使用する明示的な変換](conversions.md#explicit-conversions-involving-type-parameters)します。

明示的な参照変換は、実行時のチェックが正しいことを確認を必要とする参照型の間で変換のことです。

明示的な参照変換を実行時に正常にするには、ソース オペランドの値がある必要があります`null`、またはソース オペランドによって参照されるオブジェクトの実際の型の暗黙的な参照によって変換先の型に変換できる型でなければなりません変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions)) またはボックス化変換 ([ボックス化変換](conversions.md#boxing-conversions))。 明示的な参照変換に失敗した場合、`System.InvalidCastException`がスローされます。

参照変換、暗黙的または明示的には、変換されるオブジェクトの参照 id を変更することはありません。 つまり、参照変換は、参照の種類を変更することが、変更しません、型または参照されるオブジェクトの値。

### <a name="unboxing-conversions"></a>ボックス化解除変換

ボックス化解除の変換では参照型に明示的に変換する、 *value_type*します。 型からボックス化解除の変換が存在する`object`、`dynamic`と`System.ValueType`いずれかに*non_nullable_value_type*、いずれかから*interface_type*いずれかに*non_nullable_value_type*を実装する、 *interface_type*します。 さらに入力`System.Enum`いずれかにボックス化解除できます*enum_type*します。

参照型をボックス化解除の変換が存在する、 *nullable_type* 、基になる参照型のアンボックス変換が存在する場合は*non_nullable_value_type*の*nullable_type*します。

値型`S`がインターフェイス型型からをボックス化解除変換`I`インターフェイス型型からのボックス化解除の変換があるかどうかは`I0`と`I0`が恒等変換を`I`します。

値型`S`がインターフェイス型型からをボックス化解除変換`I`インターフェイスまたはデリゲートの型からボックス化解除の変換がある場合`I0`、`I0`差異に変換できるに`I`または`I`差異に変換できるに`I0`([分散変換](interfaces.md#variance-conversion))。

ボックス化解除の操作は、オブジェクトのインスタンスのボックス化された値は、最初に確認の指定された*value_type*、し、そのインスタンスから値をコピーします。 Null 参照がボックス化解除、 *nullable_type*の null 値を生成、 *nullable_type*します。 構造体の型からボックス化が解除できます`System.ValueType`すべての構造体の基本クラスでは、([継承](structs.md#inheritance))。

ボックス化解除変換の詳細については[ボックス化解除変換](types.md#unboxing-conversions)します。

### <a name="explicit-dynamic-conversions"></a>明示的な動的な変換

型の式から動的の明示的な変換が存在する`dynamic`任意の型を`T`します。 変換が動的にバインドされている ([動的バインド](expressions.md#dynamic-binding))、つまり、実行時に式の実行時の型からの明示的な変換が求められますこと`T`します。 変換が見つからない場合は、実行時に例外がスローされます。

変換の動的バインドが望ましくない場合、式最初に変換できる`object`とし、目的の型。

次のクラスが定義されていると仮定します。
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

次の例は、明示的な動的な変換を示しています。
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

最適な変換`o`に`C`が記載された明示的な参照変換をコンパイルします。 に、実行時に、これが失敗した`"1"`ファクトでは、`C`します。 変換`d`に`C`実行時、ユーザーがの実行時の型からの変換を定義するが、明示的な動的変換としてがただし、中断されて`d`  --  `string` --に`C`が見つかると、成功したとします。

### <a name="explicit-conversions-involving-type-parameters"></a>型パラメーターを使用する明示的な変換

次の明示的な変換が指定された型パラメーターの存在`T`:

*  有効な基本クラスから`C`の`T`に`T`との任意の基本クラスから`C`に`T`します。 実行時に、if`T`値型である、変換、アンボックス変換として実行します。 それ以外の場合、変換は、明示的な参照変換または id 変換として実行されます。
*  任意のインターフェイス型から`T`します。 実行時に、if`T`値型である、変換、アンボックス変換として実行します。 それ以外の場合、変換は、明示的な参照変換または id 変換として実行されます。
*  `T`いずれかに*interface_type* `I`既にからの暗黙的な変換がない`T`に`I`します。 実行時に、if`T`値型である明示的な参照変換の後にボックス化変換と変換を実行します。 それ以外の場合、変換は、明示的な参照変換または id 変換として実行されます。
*  型パラメーターから`U`に`T`に用意されている`T`異なります`U`([パラメーターの制約入力](classes.md#type-parameter-constraints))。 実行時に、if`U`値の型には`T`と`U`必ずしも同じ型では、変換は行われません。 の場合`T`値型である、変換、アンボックス変換として実行します。 それ以外の場合、変換は、明示的な参照変換または id 変換として実行されます。

場合`T`はわかっていますが、参照型である、上記の変換はすべてとして分類明示的な参照変換 ([明示的な参照変換](conversions.md#explicit-reference-conversions))。 場合`T`は上記の変換は参照型であるとわかっていない、ボックス化解除変換に分類されます ([ボックス化解除変換](conversions.md#unboxing-conversions))。

上記の規則は、制約のない型パラメーターから、インターフェイスではない型への直接明示的な変換許可されていません驚くべきことが必要があります。 このルールの理由は、このような変換をオフのセマンティクスを混乱を避けるためです。 たとえば、次のような宣言があるとします。
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

場合の直接の明示的な変換`t`に`int`が許可されているいずれかが簡単に期待する`X<int>.F(7)`を返します `7L`。 ただし、設定されませんでしたが、標準の数値の変換は、バインド時に数値型がわかっている場合にのみと見なされるためです。 セマンティクスを作成するには明確で上記の例では、代わりに記述する必要があります。
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

このコードはコンパイルされますが、実行時`X<int>.F(7)`ボックス化された後の実行時に、例外をスローしは`int`に直接変換することはできません、`long`します。

### <a name="user-defined-explicit-conversions"></a>ユーザー定義の明示的な変換

ユーザー定義の明示的な変換は、省略可能な標準的な明示的な変換後に省略可能な別の標準的な明示的な変換の後に、ユーザー定義の暗黙的または明示的な変換演算子の実行で構成されます。 ユーザー定義の明示的な変換を評価するための正確な規則については、「[明示的な変換のユーザー定義の処理](conversions.md#processing-of-user-defined-explicit-conversions)します。

## <a name="standard-conversions"></a>標準変換

標準の変換は、ユーザー定義の変換の一部として発生する変換の定義済みです。

### <a name="standard-implicit-conversions"></a>標準の暗黙的な変換

次の暗黙的な変換は、標準の暗黙的な変換として分類されます。

*  恒等変換 ([恒等変換](conversions.md#identity-conversion))
*  暗黙的な数値変換 ([暗黙的な数値変換](conversions.md#implicit-numeric-conversions))
*  Null 許容型の暗黙的な変換 ([null 許容型の暗黙的な変換](conversions.md#implicit-nullable-conversions))
*  暗黙的な参照変換 ([暗黙の参照変換](conversions.md#implicit-reference-conversions))
*  ボックス化変換 ([ボックス化変換](conversions.md#boxing-conversions))
*  定数式が暗黙的な変換 ([動的の暗黙的な変換](conversions.md#implicit-dynamic-conversions))
*  型パラメーターを伴う暗黙の変換 ([型パラメーターを使用する暗黙的な変換](conversions.md#implicit-conversions-involving-type-parameters))

具体的には、標準の暗黙的な変換は、ユーザー定義の暗黙的な変換を除外します。

### <a name="standard-explicit-conversions"></a>標準の明示的な変換

標準の明示的な変換は、すべての標準的な暗黙的な変換と反対の標準的な暗黙的な変換が存在する明示的な変換のサブセットです。 つまり、標準の暗黙的な変換が存在する場合、型から`A`型に`B`、型から標準の明示的な変換が存在し、`A`入力`B`型との間`B`入力`A`。

## <a name="user-defined-conversions"></a>ユーザー定義の変換

C# では、事前に定義された明示的および暗黙的な変換を拡張して***ユーザー定義の変換***します。 ユーザー定義の変換が変換演算子を宣言することで導入されました ([変換演算子](classes.md#conversion-operators)) クラスと構造体の型。

### <a name="permitted-user-defined-conversions"></a>ユーザー定義の変換を許可

C# でのみ特定のユーザー定義変換を宣言します。 具体的には、既存の暗黙的または明示的な変換を再定義することはできません。

指定したソースの種類`S`ターゲットの種類と`T`場合は、`S`または`T`は null 許容型は、ように`S0`と`T0`それ以外の場合、基になる型を参照してください`S0`と`T0`は等しい`S`と`T`それぞれします。 ソースの種類からの変換を宣言するクラスまたは構造体が許可されている`S`をターゲットの型`T`次のすべてに当てはまる場合にのみ。

*  `S0` `T0`はさまざまな種類。
*  いずれか`S0`または`T0`演算子宣言が行われるクラスまたは構造体の型です。
*  どちらも`S0`も`T0`は、 *interface_type*します。
*  ユーザー定義の変換を除く、変換が存在しないから`S`に`T`またはから`T`に`S`します。

ユーザー定義の変換に適用される制限については、説明でさらに[変換演算子](classes.md#conversion-operators)します。

### <a name="lifted-conversion-operators"></a>リフト変換演算子

Null 非許容値型に変換するユーザー定義の変換演算子を指定された`S`null 非許容値型に`T`、***変換演算子のリフト***から変換が存在する`S?`に`T?`. ラップ解除、このリフト変換演算子を実行します`S?`に`S`からユーザー定義の変換後に`S`に`T`からの折り返しを続けて`T`に`T?`点を除き、null値を持つ`S?`値は null に直接変換`T?`します。

リフト変換演算子が、その基になるユーザー定義の変換演算子として同じ暗黙的または明示的な分類します。 「ユーザー定義の変換」は、両方の使用に適用期間は、ユーザー定義し、リフト変換演算子。

### <a name="evaluation-of-user-defined-conversions"></a>ユーザー定義の変換の評価

ユーザー定義の変換と呼ばれる、その型からの値の変換、***ソースの種類***、という名前の別の型に、***ターゲットの種類***します。 ユーザー定義の変換の評価を確認する方法に中央揃え、***最も具体的***特定のソースとターゲットの種類のユーザー定義変換演算子。 この決定は、いくつかの手順に分かれています。

*  クラスと元のユーザー定義の変換演算子を考慮する構造体のセットを検索します。 このセットは、ソースの種類とその基本クラスとターゲットの種類 (クラスと構造体のみがユーザー定義の演算子を宣言でき、クラス以外の型が基底クラスを含むなしの暗黙の想定) をその基本クラスで構成されます。 ソースまたはターゲットのいずれかの型がある場合、この手順の目的で、 *nullable_type*、その基になる型は代わりに使用されます。
*  型が設定されるユーザー定義して、リフト変換演算子の決定に適用されます。 変換演算子を適用する場合にの標準変換を実行する場合があります ([標準変換](conversions.md#standard-conversions)) オペランドのソースの種類からは、演算子の種類で標準変換を実行できる必要があります対象の型に演算子の結果の型。
*  該当するユーザー定義演算子のセットからには、演算子が明確に最も固有を決定します。 一般的な用語では、最も固有の演算子はオペランドの型がソースの種類に「最も近い」で、結果の型は、ターゲット型に「最も近い」演算子が。 ユーザー定義変換演算子は、リフト変換演算子より優先されます。 最も固有のユーザー定義変換演算子を確立するための正確な規則は、次のセクションで定義されます。

特定のユーザー定義の変換演算子が識別されると、ユーザー定義の変換の実際の実行に最大 3 つの手順が含まれます。

*  最初に、必要な場合は、ソースの種類から、またはユーザー定義の変換の変換演算子のオペランドの型への標準変換を実行します。
*  次に、変換を実行するユーザー定義またはリフトの変換演算子を呼び出しています。
*  最後に、必要な場合、またはユーザー定義の変換の変換演算子の結果の型から対象の型への標準変換を実行します。

ユーザー定義変換しないの評価には、1 つ以上のユーザー定義またはリフトの変換演算子が含まれます。 つまり、型から変換`S`を入力する`T`からユーザー定義の変換は実行しない最初`S`に`X`からユーザー定義の変換を実行し、`X`に`T`します。

ユーザー定義の暗黙的または明示的な変換の評価の正確な定義は、次のセクションで提供されます。 定義の次の用語を使用します。

*  場合、標準の暗黙的な変換 ([標準の暗黙的な変換](conversions.md#standard-implicit-conversions)) 型からが存在する`A`型`B`、どちらの場合と`A`も`B`は*interface_type*s し`A`と呼ばれます***に包含***`B`と`B`といいます***encompass*** `A`します。
*  ***最も外側の型***一連の型では、セット内の他のすべての型を包含している 1 つの型。 その他のすべての型を包含する 1 つの型の場合セットに最も外側の型がありません。 最も外側の型は、セットの「最大」の型をより直感的な用語で、先の他の種類ごとに暗黙的に変換できる 1 つの型。
*  ***最も内側の型***一連の型では、セット内の他のすべての種類に包含されている 1 つの型。 他のすべての型によって 1 つの型が含まれていない場合、セットが最も内側の型。 直感的な用語は、最も内側の型は、セット内の「最小」の型などの他の種類ごとに暗黙的に変換できる 1 つの型。

### <a name="processing-of-user-defined-implicit-conversions"></a>暗黙的な変換のユーザー定義の処理

ユーザー定義型から暗黙的な変換`S`を入力する`T`ように処理されます。

*  種類を特定する`S0`と`T0`します。 場合`S`または`T`は null 許容型は、`S0`と`T0`の基になる型をそれ以外の場合は`S0`と`T0`と等しい`S`と`T`それぞれします。
*  型のセットを見つける`D`、どのユーザー定義の変換演算子を考慮します。 このセットから成る`S0`(場合`S0`がクラスまたは構造体) の基本クラス`S0`(場合`S0`クラスは、)、および`T0`(場合`T0`がクラスまたは構造体)。
*  該当するユーザー定義およびリフト変換演算子のセットを見つける`U`します。 このセットは、クラスまたは構造体で宣言されているユーザー定義およびリフトの暗黙的な変換演算子で構成されます`D`包括的な型から変換する`S`型によって包含`T`します。 場合`U`は空の場合、変換が定義されていると、コンパイル時エラーが発生します。
*  特定のソースの種類を検索`SX`、演算子の`U`:
    * いずれかの演算子の`U`から変換`S`、し`SX`は`S`します。
    * それ以外の場合、`SX`の演算子のソースの種類の結合されたセット内の最も内側の型は、`U`します。 型が見つからない最も内側の 1 つだけの場合に、変換があいまいなをコンパイル時エラーが発生します。
*  最も固有のターゲット型を見つける`TX`、演算子の`U`:
    * いずれかの演算子の場合`U`変換`T`、し`TX`は`T`。
    * それ以外の場合、`TX`で一連の演算子のターゲット タイプの最も外側の型は、`U`します。 正確に 1 つの最も外側の型が見つからない場合、変換はあいまいですしとコンパイル時エラーが発生します。
*  最も固有の変換演算子を検索します。
    * 場合`U`から変換する 1 つだけのユーザー定義変換演算子が含まれています`SX`に`TX`、これは、最も固有の変換演算子。
    * の場合`U`から変換する 1 つだけのリフト変換演算子が含まれています`SX`に`TX`、これは、最も固有の変換演算子。
    * それ以外の場合、変換はあいまいですし、コンパイル時エラーが発生します。
*  最後に、変換が適用されます。
    * 場合`S`ない`SX`から、標準、暗黙的な変換し、`S`に`SX`が実行されます。
    * 変換する最も固有の変換演算子が呼び出される`SX`に`TX`します。
    * 場合`TX`ない`T`から、標準、暗黙的な変換し、`TX`に`T`が実行されます。

### <a name="processing-of-user-defined-explicit-conversions"></a>明示的な変換のユーザー定義の処理

型からユーザー定義の明示的な変換`S`入力`T`ように処理されます。

*  種類を特定する`S0`と`T0`します。 場合`S`または`T`は null 許容型は、`S0`と`T0`の基になる型をそれ以外の場合は`S0`と`T0`と等しい`S`と`T`それぞれします。
*  型のセットを見つける`D`、どのユーザー定義の変換演算子を考慮します。 このセットから成る`S0`(場合`S0`がクラスまたは構造体) の基本クラス`S0`(場合`S0`クラスは、)、 `T0` (場合`T0`がクラスまたは構造体)、およびの基本クラス`T0`(場合`T0`クラスです)。
*  該当するユーザー定義およびリフト変換演算子のセットを見つける`U`します。 このセットは、ユーザー定義およびリフト暗黙的または明示的な変換演算子は、クラスまたは構造体で宣言されている`D`を包括的な型から変換またはによって包含`S`を包括的なまたはに包含型`T`. 場合`U`は空の場合、変換が定義されていると、コンパイル時エラーが発生します。
*  特定のソースの種類を検索`SX`、演算子の`U`:
    * いずれかの演算子の`U`から変換`S`、し`SX`は`S`します。
    * それ以外の場合、いずれかの演算子の`U`包含する型から変換`S`、し`SX`これらの演算子のソースの種類の結合されたセット内の最も内側にある型です。 型が見つからない、最も内側の場合に、変換があいまいなをコンパイル時エラーが発生します。
    * それ以外の場合、`SX`の演算子のソースの種類の結合されたセット内の最も外側の型は、`U`します。 正確に 1 つの最も外側の型が見つからない場合、変換はあいまいですしとコンパイル時エラーが発生します。
*  最も固有のターゲット型を見つける`TX`、演算子の`U`:
    * いずれかの演算子の場合`U`変換`T`、し`TX`は`T`。
    * それ以外の場合、いずれかの演算子の`U`によって包含する型に変換`T`、し`TX`はこれらの演算子のターゲット型の結合されたセット内の最も外側の型。 正確に 1 つの最も外側の型が見つからない場合、変換はあいまいですしとコンパイル時エラーが発生します。
    * それ以外の場合、`TX`で一連の演算子のターゲット タイプの最も内側の型は、`U`します。 型が見つからない、最も内側の場合に、変換があいまいなをコンパイル時エラーが発生します。
*  最も固有の変換演算子を検索します。
    * 場合`U`から変換する 1 つだけのユーザー定義変換演算子が含まれています`SX`に`TX`、これは、最も固有の変換演算子。
    * の場合`U`から変換する 1 つだけのリフト変換演算子が含まれています`SX`に`TX`、これは、最も固有の変換演算子。
    * それ以外の場合、変換はあいまいですし、コンパイル時エラーが発生します。
*  最後に、変換が適用されます。
    * 場合`S`ない`SX`から標準の明示的な変換、`S`に`SX`が実行されます。
    * 変換する最も固有のユーザー定義変換演算子が呼び出される`SX`に`TX`します。
    * 場合`TX`ない`T`から標準の明示的な変換、`TX`に`T`が実行されます。

## <a name="anonymous-function-conversions"></a>匿名関数の変換

*Anonymous_method_expression*または*lambda_expression*匿名関数として分類されます ([匿名関数式](expressions.md#anonymous-function-expressions))。 式は、型がありませんが、互換性のあるデリゲート型または式ツリー型に暗黙的に変換できます。 具体的には、匿名関数`F`デリゲート型と互換性が`D`提供します。

*  場合`F`が含まれています、 *anonymous_function_signature*、し`D`と`F`同じ数のパラメーターがあります。
*  場合`F`が含まれていない、 *anonymous_function_signature*、し`D`のパラメーターなしに限り、あらゆる種類の 0 個以上のパラメーターがあります。`D`が、`out`パラメーター修飾子。
*  場合`F`、明示的に型指定されたパラメーターのリスト内の各パラメーターを持つ`D`の対応するパラメーターとして、同じ型および修飾子を持つ`F`します。
*  場合`F`、暗黙的に型指定されたパラメーター リストを持つ`D`にない`ref`または`out`パラメーター。
*  場合の本文`F`が、式で、かつ`D`が、`void`型を返すまたは`F`は async と`D`戻り値の型を持つ`Task`、その後の各パラメーター`F`の型を指定、対応するパラメーターの`D`の本文`F`有効な式です (wrt[式](expressions.md)) として許可されている場合、 *statement_expression* ([式ステートメント](statements.md#expression-statements))。
*  場合の本文`F`がステートメント ブロックで、かつ`D`が、`void`型を返すまたは`F`は async と`D`戻り値の型を持つ`Task`、その後の各パラメーター`F`の型を指定対応するパラメーター`D`の本文`F`は有効なステートメント ブロックです (wrt[ブロック](statements.md#blocks)) いない`return`ステートメント式を指定します。
*  場合の本文`F`式であると*か*`F`は非同期ではないと`D`非 void の戻り値の型を持つ`T`、*または*`F`は async と`D`戻り値の型を持つ`Task<T>`、その後の各パラメーター`F`の対応するパラメーターの型が指定`D`の本文`F`有効な式です (wrt [式](expressions.md)) に暗黙的に変換可能`T`します。
*  場合の本文`F`ステートメント ブロックと*か*`F`は非同期ではないと`D`非 void の戻り値の型を持つ`T`、*または*`F`は async と`D`戻り値の型を持つ`Task<T>`、その後の各パラメーター`F`の対応するパラメーターの型が指定`D`の本文`F`は有効なステートメント ブロックです (wrt[ブロック](statements.md#blocks)) 各が到達可能な終了点で`return`ステートメントは、暗黙的に変換できる式を指定します。`T`します。

このセクションで、簡潔さを優先するためにタスクの種類の短い形式を使用して`Task`と`Task<T>`([非同期関数](classes.md#async-functions))。

ラムダ式`F`式ツリー型と互換性が`Expression<D>`場合`F`デリゲート型と互換性が`D`します。 匿名メソッド、ラムダ式のみをこの適用されないことに注意してください。

特定のラムダ式を式ツリー型に変換できません: 場合でも、変換*が存在する*コンパイル時に失敗します。 これは、場合は、ラムダ式。

*  *ブロック*本文
*  単純型または複合代入演算子が含まれています
*  動的にバインドされている式が含まれています
*  非同期には

次の例は、汎用デリゲート型を使用して、`Func<A,R>`型の引数を受け取る関数を表す`A`型の値を返す`R`:
```csharp
delegate R Func<A,R>(A arg);
```

割り当て
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
各匿名関数のパラメーターと戻り値の型は、匿名関数が割り当てられている変数の型から決まります。

最初の割り当てが正常に匿名関数をデリゲート型に変換`Func<int,int>`ため、`x`型が指定`int`、`x+1`型に暗黙的に変換できる有効な式です`int`します。

同様に、2 つ目の割り当てを正常に変換関数を匿名デリゲートの型に`Func<int,double>`ため、結果の`x+1`(型の`int`) 型に暗黙的に変換できる`double`します。

しかし、3 番目の割り当てが、コンパイル時エラー、`x`型が指定`double`の結果`x+1`(型の`double`) 型に暗黙的に変換可能でない`int`します。

4 番目の割り当てが正常に匿名の非同期関数をデリゲート型に変換`Func<int, Task<int>>`ため、結果の`x+1`(型の`int`) 結果の型に暗黙的に変換できる`int`タスクの種類の`Task<int>`.

匿名関数はオーバー ロードの解決に影響を与える可能性があり、型の推定に参加します。 参照してください[関数メンバー](expressions.md#function-members)の詳細。

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>デリゲート型への匿名関数の変換の評価

匿名関数のデリゲート型への変換では、匿名関数とキャプチャされた外部変数の評価時にアクティブになっている一連の (場合によっては空) を参照するデリゲート インスタンスを生成します。 デリゲートが呼び出されたときに、匿名関数の本体が実行されます。 デリゲートによって参照される外部変数をキャプチャのセットを使用して、本体内のコードが実行されます。

匿名関数によって生成されたデリゲートの呼び出しリストには、1 つのエントリが含まれています。 正確なターゲット オブジェクトと、デリゲートのターゲット メソッドが指定されていません。 具体的には、指定されていないかどうか、デリゲートのターゲット オブジェクトは、 `null`、`this`外側の関数メンバー、またはその他のオブジェクトの値。

同じデリゲート型への外部変数のキャプチャされたインスタンスの同じ (場合によっては空) のセットの意味的に同一の匿名関数の変換は許可されている (ただし必要ありません) を同じデリゲート インスタンスを返します。 匿名関数の実行、すべてのケースで、表示されるのと同じ引数を指定したのと同じ効果を意味する意味的に同一の用語がここで使用されます。 このルールは、最適化するのには、次のようなコードを許可します。

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

2 つの関数を匿名デリゲートを同じ (空) と匿名関数は意味が同じであるため、キャプチャされた外部変数の設定があるため、コンパイラがデリゲートに同じターゲット メソッドを参照してください。 許可されています。 実際には、コンパイラは、両方の匿名関数式から、まったく同じデリゲート インスタンスを返すに許可されます。

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>式ツリー型への匿名関数の変換の評価

匿名関数の式ツリー型への変換、式ツリーを生成します ([式ツリー型](types.md#expression-tree-types))。 正確には、匿名関数の変換の評価は、匿名関数自体の構造を表すオブジェクトの構造の構築につながります。 それを作成するための正確なプロセスと同様に、式ツリーの厳密な構造は、実装定義です。

### <a name="implementation-example"></a>実装例

このセクションでは、その他の c# コンストラクトの観点から匿名関数の変換の実装について説明します。 ここで説明されている実装 Microsoft c# コンパイラで使用される同じ原則に基づいていますが、1 つだけのことも、必須の実装ではではありません。 ごく簡単に、この仕様の範囲外の正確なセマンティクスは、式ツリーへの変換を紹介します。

このセクションの残りの部分では、異なる特性を持つ匿名関数を含むコードのいくつかの例を示します。 それぞれの例については、のみ c# の他のコンストラクトを使用するコードに対応する翻訳が提供されます。 識別子の例についてで`D`次のデリゲート型で表すと見なされます。
```csharp
public delegate void D();
```

匿名関数の最も単純な形式は、外部変数をキャプチャしません。
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

これは、匿名関数のコードが置かれているコンパイラによって生成された静的メソッドを参照するデリゲートのインスタンス化に書き換えることができます。
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

匿名関数がのインスタンス メンバーを参照する次の例では、 `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

これは、匿名関数のコードを含むコンパイラによって生成されたインスタンス メソッドに書き換えることができます。
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

この例では、匿名関数は、ローカル変数をキャプチャします。
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

ローカル変数の有効期間を少なくとも、関数を匿名デリゲートの有効期間まで延長する必要があります。 これは、コンパイラによって生成されたクラスのフィールドに、ローカル変数を「ホイスト」で実現できます。 ローカル変数のインスタンス化 ([ローカル変数のインスタンス化](expressions.md#instantiation-of-local-variables))、コンパイラによって生成されたクラスのインスタンス内のフィールドへのアクセスに対応するローカル変数にアクセスするのインスタンスを作成するのには対応し、コンパイラによって生成されたクラスです。 さらに、匿名関数には、コンパイラによって生成されたクラスのインスタンス メソッドがようになります。
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

最後に、キャプチャの関数は次の匿名`this`と有効期間の異なる 2 つのローカル変数。
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

ここでは、コンパイラによって生成されたクラスを作成する for each ステートメントで、さまざまな要素でローカル変数は、独立した有効期間を持つことができますをどのローカル変数がキャプチャをブロックします。 インスタンス`__Locals2`、inner ステートメント ブロックのコンパイラによって生成されたクラスには、ローカル変数が含まれています。`z`のインスタンスを参照するフィールドと`__Locals1`します。  インスタンス`__Locals1`、外側のステートメント ブロックのコンパイラによって生成されたクラスには、ローカル変数が含まれています。`y`を参照するフィールドと`this`外側の関数メンバーの。 アクセスには、これらのデータ構造ですべてのキャプチャされた外部変数のインスタンスを通じて`__Local2`、および匿名関数のコードはそのクラスのインスタンス メソッドとして実装できます。

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

ここで適用されるローカル変数をキャプチャする同じの方法は、匿名関数を式ツリーに変換するときにも使用できます: 式ツリーで、コンパイラによって生成されたオブジェクトへの参照を格納できるし、ローカル変数にアクセスできますこれらのオブジェクトのフィールドにアクセスするように表されます。 このアプローチの利点は、デリゲート、式ツリーの間で共有する「リフト」ローカル変数でできることです。

## <a name="method-group-conversions"></a>メソッド グループ変換

暗黙的な変換 ([暗黙的な変換](conversions.md#implicit-conversions))、メソッド グループからが存在する ([式の分類](expressions.md#expression-classifications)) 互換性のあるデリゲート型にします。 デリゲート型を指定して`D`と式`E`メソッド グループに分類されるからの暗黙的な変換が存在する`E`に`D`場合`E`はその通常の形式 (該当する少なくとも 1 つのメソッドが含まれています[適用可能な関数メンバー](expressions.md#applicable-function-member))、引数リストへの修飾子をパラメーターの型の使用して構築された`D`以下で説明するようにします。

メソッド グループからの変換のコンパイル時のアプリケーション`E`をデリゲート型に`D`については、次で説明します。 なおからの暗黙的な変換が存在する`E`に`D`変換のコンパイル時のアプリケーションがエラーなしに成功するには保証されません。

*  1 つのメソッド`M`が選択されているメソッドの呼び出しに対応する ([メソッドの呼び出し](expressions.md#method-invocations)) フォームの`E(A)`、次の変更。
    * 引数リスト`A`式、変数として、型および修飾子を使用して、各分類済みの一覧を示します (`ref`または`out`) に対応するパラメーターの*formal_parameter_list* の`D`.
    * 候補となるメソッドと見なされますが、標準形式で適用されるメソッドのみ ([適用可能な関数メンバー](expressions.md#applicable-function-member))、拡張形式でのみ適用されません。
*  場合、アルゴリズムの[メソッドの呼び出し](expressions.md#method-invocations)コンパイル時エラーが発生し、エラーが発生します。 それ以外の場合、アルゴリズムが最適な 1 つのメソッドが生成されます`M`同じ同数のパラメーターを持つ`D`と変換が存在すると見なされます。
*  選択したメソッド`M`互換である必要があります ([デリゲートの互換性](delegates.md#delegate-compatibility)) デリゲート型と`D`、またはそれ以外の場合、コンパイル時エラーが発生しました。
*  場合、選択したメソッド`M`、インスタンス メソッドに関連付けられたインスタンス式である`E`デリゲートのターゲット オブジェクトを決定します。
*  インスタンス式でメンバー アクセスによって表される拡張メソッドを選択したメソッド M には、そのインスタンス式は、デリゲートの対象オブジェクトを決定します。
*  変換の結果は、型の値 `D`、新しく作成されたデリゲート メソッドとターゲットの選択したオブジェクトを指す namely します。
*  このプロセスが拡張メソッド、デリゲートの作成を簡単になることができる場合のアルゴリズム[メソッドの呼び出し](expressions.md#method-invocations)インスタンス メソッドの検索に失敗したが、処理の呼び出しに成功すると、`E(A)`拡張機能としてメソッドの呼び出し ([拡張メソッド呼び出し](expressions.md#extension-method-invocations))。 こうして作成したデリゲートでは、拡張メソッドと、最初の引数をキャプチャします。

次の例では、メソッド グループ変換を示しています。
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

代入`d1`メソッド グループを暗黙的に変換します`F`型の値に`D1`します。

代入`d2`を弱い派生 (反変) パラメーターの型を持つメソッドにデリゲートを作成することはしより (共変性) の戻り値の型を派生する方法を示します。

代入`d3`表示変換が存在しないかどうか、メソッドは使用できません。

代入`d4`メソッドが、標準形式で適用する必要がありますの方法を示します。

代入`d5`参照型に対してのみ異なるデリゲートとメソッドのパラメーターと戻り値の型を許可する方法を示しています。

すべて、他の明示的および暗黙的な変換と明示的にメソッド グループ変換を実行するキャスト演算子を使用できます。 例では、そのため、
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
代わりに書き込まれる可能性があります。
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

メソッド グループは、オーバー ロードの解決に影響を与えるし、型の推定に参加可能性があります。 参照してください[関数メンバー](expressions.md#function-members)の詳細。

メソッド グループ変換の実行時の評価は、次のように処理されます。

*  関連付けられたインスタンス式からデリゲートのターゲット オブジェクトを確認するコンパイル時に選択されているメソッドがインスタンス メソッド、またはインスタンス メソッドとしてアクセスされる拡張メソッドは、 `E`:
    * インスタンス式が評価されます。 この評価は、例外を発生させ、その後の手順は実行されません。
    * インスタンス式がある場合、 *reference_type*、ターゲット オブジェクトをインスタンス式で計算された値になります。 選択したメソッドがインスタンス メソッドと、ターゲット オブジェクトがかどうか`null`、`System.NullReferenceException`がスローされます、以降の手順は実行されません。
    * インスタンス式がある場合、 *value_type*、ボックス化操作 ([ボックス化変換](types.md#boxing-conversions)) は、オブジェクトに値を変換する実行し、このオブジェクトがターゲット オブジェクトになります。
*  それ以外の場合、選択したメソッドは静的メソッドの呼び出しの一部ですが、デリゲートのターゲット オブジェクトと`null`します。
*  デリゲート型の新しいインスタンス`D`が割り当てられます。 新しいインスタンスを割り当てることができる十分なメモリがない場合、`System.OutOfMemoryException`がスローされます、以降の手順は実行されません。
*  新しいデリゲート インスタンスがコンパイル時に決定されたメソッドへの参照で初期化され、上、ターゲット オブジェクトへの参照が計算されます。
