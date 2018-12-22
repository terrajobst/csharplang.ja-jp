# <a name="interfaces"></a>インターフェイス

インターフェイスは、コントラクトを定義します。 クラスまたはインターフェイスを実装する構造体は、そのコントラクトに従う必要があります。 複数の基底インターフェイスから継承でき、クラスまたは構造体には、複数のインターフェイスを実装できます。

インターフェイスは、メソッド、プロパティ、イベント、およびインデクサーに含めることができます。 インターフェイス自体では、定義するメンバーの実装は提供されません。 インターフェイスには、クラスまたはインターフェイスを実装する構造体を渡す必要があるメンバーだけを指定します。

## <a name="interface-declarations"></a>インターフェイスの宣言

*Interface_declaration*は、 *type_declaration* ([入力宣言](namespaces.md#type-declarations)) 新しいインターフェイスの型を宣言します。

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

*Interface_declaration*オプションのセットから成る*属性*([属性](attributes.md)) オプションのセットと、その後*interface_modifier*s ([修飾子をインターフェイス](interfaces.md#interface-modifiers)) と省略可能なその後`partial`の後に、キーワード修飾子`interface`と*識別子*インターフェイスの名前を示す続けて、省略可能な*variant_type_parameter_list*仕様 ([バリアント型パラメーター リスト](interfaces.md#variant-type-parameter-lists)) と省略可能なその後*interface_base*仕様 ([基本インターフェイス](interfaces.md#base-interfaces)) と省略可能なその後*type_parameter_constraints_clause*s 仕様 ([パラメーターの制約入力](classes.md#type-parameter-constraints))、続けて、 *interface_body* ([インターフェイス本体](interfaces.md#interface-body))、セミコロンで必要に応じてその後にします。

### <a name="interface-modifiers"></a>修飾子をインターフェイスします。

*Interface_declaration*インターフェイス修飾子のシーケンスを必要に応じて含めることができます。

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

同じ修飾子をインターフェイスの宣言内で複数回のコンパイル時エラーになります。

`new`修飾子がクラス内で定義されているインターフェイスでのみ使用できます。 指定します、インターフェイスが、同じ名前で、継承されたメンバーを非表示にする」の説明に従って[new 修飾子](classes.md#the-new-modifier)します。

`public`、 `protected`、 `internal`、および`private`修飾子は、インターフェイスのアクセシビリティを制御します。 インターフェイスの宣言が発生したコンテキストに応じてこれらの修飾子の一部のみできない場合があります ([宣言されたアクセシビリティ](basic-concepts.md#declared-accessibility))。

### <a name="partial-modifier"></a>Partial 識別子

`partial`修飾子には、ことを示しますこの*interface_declaration*部分型の宣言です。 指定された規則に従って、それを囲む名前空間または型の宣言内に同じ名前を持つ複数の部分的なインターフェイス宣言は、フォームの 1 つのインターフェイスの宣言に結合、[部分型](classes.md#partial-types)します。

### <a name="variant-type-parameter-lists"></a>バリアント型パラメーター リスト

バリアント型パラメーター リストは、インターフェイスとデリゲート型でのみ実行できます。 通常の違い*type_parameter_list*s は省略可能な*variance_annotation*型パラメーターごとにします。

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

分散注釈が場合`out`、型パラメーターはモード***共変***します。 分散注釈が場合`in`、型パラメーターはモード***反変***します。 分散注釈がない場合は、型パラメーターと呼ばれます***インバリアント***します。

例
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
`X` 共変性を持つ`Y`は反変、`Z`バリアントではありません。

#### <a name="variance-safety"></a>差異の安全性

型の型パラメーター リスト内の分散注釈の発生は、型宣言内で種類できますが発生した場所を制限します。

型`T`は***出力の安全でない***次のいずれかが保持している場合。

*  `T` 反変の型パラメーターには
*  `T` 出力の安全でない要素の型の配列型には
*  `T` インターフェイスまたはデリゲート型`S<A1,...,Ak>`ジェネリック型から構築された`S<X1,...,Xk>`少なくとも 1 つの where`Ai`次のいずれかを保持します。
   * `Xi` 共変またはインバリアントと`Ai`は出力の安全でないです。
   * `Xi` 反変か不変条件と`Ai`入力セーフです。
   
型`T`は***入力の安全でない***次のいずれかが保持している場合。

*  `T` 共変の型パラメーターには
*  `T` 入力の安全でない要素の型の配列型には
*  `T` インターフェイスまたはデリゲート型`S<A1,...,Ak>`ジェネリック型から構築された`S<X1,...,Xk>`少なくとも 1 つの where`Ai`次のいずれかを保持します。
   * `Xi` 共変またはインバリアントと`Ai`は入力の安全でないです。
   * `Xi` 反変か不変条件と`Ai`は出力の安全でないです。

直感的な出力の安全でない型が、出力位置で禁止されているし、入力位置に、入力の安全でない型は禁止されています。

型が***出力セーフ***出力 unsafe でない場合、***入力の安全な***ない入力の安全でない場合。

#### <a name="variance-conversion"></a>差異の変換

分散注釈では、インターフェイスとデリゲート型に (ただし、まだタイプ セーフ) より緩やかな変換を提供します。 この暗黙の定義を終了します ([暗黙的な変換](conversions.md#implicit-conversions)) と明示的な変換 ([明示的な変換](conversions.md#explicit-conversions)) ことを次のように定義されている分散でもの概念の使用します。

型`T<A1,...,An>`分散変換できる型に`T<B1,...,Bn>`場合`T`バリアント型パラメーターでは、インターフェイスまたはデリゲート型宣言`T<X1,...,Xn>`、および各バリアント型パラメーターの`Xi`次のいずれか保持されています。

*  `Xi` 共変およびから暗黙の参照または id 変換が存在する`Ai`に `Bi`
*  `Xi` 反変、暗黙的な参照またはから id 変換が存在する`Bi`に `Ai`
*  `Xi` 不変性は、id からの変換が存在する`Ai`に `Bi`

### <a name="base-interfaces"></a>基底インターフェイス

インターフェイスと呼ばれる、0 個以上のインターフェイス型から継承できます、***基底インターフェイスの明示的な***のインターフェイス。 インターフェイスの 1 つまたは複数の明示的な基本インターフェイスの場合、そのインターフェイスの宣言インターフェイス識別子の後にコロンとコンマ区切り基底インターフェイスの種類の一覧。

```antlr
interface_base
    : ':' interface_type_list
    ;
```

ジェネリック型の宣言に基本インターフェイスの明示的な宣言を取得し、それぞれを代入して構築されたインターフェイス型の場合、明示的な基本インターフェイスが形成されます*type_parameter*基本インターフェイスで対応する宣言*type_argument*構築された型のです。

インターフェイスの明示的な基本インターフェイスは、少なくともインターフェイス自体と同程度にアクセスである必要があります ([アクセシビリティ制約](basic-concepts.md#accessibility-constraints))。 たとえば、指定すると、コンパイル時エラーが、`private`または`internal`インターフェイスで、 *interface_base*の`public`インターフェイス。

直接または間接的には、それ自体から継承するインターフェイスのコンパイル時エラーになります。

***基本インターフェイス***のインターフェイスは、明示的な基本インターフェイスとその基本インターフェイスです。 つまり、基本インターフェイスのセットは、明示的な基本インターフェイス、明示的な基本インターフェイス、およびなどの完全な推移閉包です。 インターフェイスは、その基本インターフェイスのすべてのメンバーを継承します。 例
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
インターフェイスの基本`IComboBox`は`IControl`、 `ITextBox`、および`IListBox`します。

つまり、`IComboBox`上記のインターフェイス メンバーを継承する`SetText`と`SetItems`だけでなく`Paint`します。

インターフェイスのすべての基本インターフェイスは出力セーフである必要があります ([差異の安全性](interfaces.md#variance-safety))。 クラスまたは構造体も暗黙的にインターフェイスを実装するには、すべてのインターフェイスの基本インターフェイスを実装します。

### <a name="interface-body"></a>インターフェイスの本文

*Interface_body*インターフェイスのインターフェイスのメンバーを定義します。

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a>インターフェイスのメンバー

インターフェイスのメンバーが基底インターフェイスから継承されたメンバーとメンバーがインターフェイス自体で宣言します。

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

インターフェイスの宣言には、0 個以上のメンバーを宣言できます。 インターフェイスのメンバーは、メソッド、プロパティ、イベント、またはインデクサーである必要があります。 インターフェイスは、定数、フィールド、演算子、インスタンス コンス トラクター、デストラクター、または、型を含めることはできません。 また、インターフェイスは、任意の種類の静的メンバーを含めることができます。

すべてのインターフェイス メンバーは、暗黙的にパブリック アクセスを持ちます。 インターフェイス メンバーの宣言に修飾子を含めるのコンパイル時エラーになります。 具体的には、修飾子を使用してインターフェイス メンバーを宣言することはできません`abstract`、 `public`、 `protected`、 `internal`、 `private`、 `virtual`、 `override`、または`static`します。

例では、
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
メンバーの可能な種類の 1 つずつ含むインターフェイスを宣言します。メソッド、プロパティ、イベント、およびインデクサーです。

*Interface_declaration*新しい宣言領域を作成します ([宣言](basic-concepts.md#declarations))、および*interface_member_declaration*ですぐに格納されている*interface_declaration*この宣言領域に新しいメンバーを導入します。 次の規則に適用されます*interface_member_declaration*: %s

*  メソッドの名前は、すべてのプロパティと同じインターフェイスで宣言されたイベントの名前とは異なる必要があります。 さらに、署名 ([シグネチャとオーバー ロード](basic-concepts.md#signatures-and-overloading)) のメソッドと同じインターフェイスで宣言されている他のすべてのメソッドのシグネチャは異なるし、同じインターフェイスで宣言されている 2 つの方法で署名がない可能性がありますがのみが異なる`ref`と`out`します。
*  プロパティまたはイベントの名前は、同じインターフェイスで宣言されているその他のすべてのメンバーの名前とは異なる必要があります。
*  インデクサーのシグネチャは、同じインターフェイスで宣言されている他のすべてのインデクサーの署名とは異なる必要があります。

インターフェイスの継承されたメンバーは、具体的には含まれないインターフェイスの宣言領域です。 したがって、インターフェイスは継承されたメンバーと同じ名前またはシグネチャを持つメンバーを宣言できます。 この問題が発生すると、派生インターフェイスのメンバーは、基本インターフェイスのメンバーを非表示にすると言います。 継承されたメンバーを非表示で、エラーはありませんが、コンパイラ警告を発し。 警告を抑制するには、派生インターフェイス メンバーの宣言を含める必要があります、`new`修飾子を派生メンバーでは、基本メンバーを非表示にするものであるかを示します。 このトピックの説明でさらに[継承による隠ぺい](basic-concepts.md#hiding-through-inheritance)します。

場合、`new`継承されたメンバーを隠ぺいしない宣言に修飾子を含めると、警告は、その結果を発行します。 削除することによってこの警告が抑制され、`new`修飾子。

なおクラスでメンバー`object`厳密に言うと、任意のインターフェイスのメンバーは使用されません ([インターフェイスのメンバー](interfaces.md#interface-members))。 ただし、クラスでメンバー`object`は任意のインターフェイス型でメンバーの検索で利用できます ([メンバー ルックアップ](expressions.md#member-lookup))。

### <a name="interface-methods"></a>インターフェイス メソッド

使用してインターフェイス メソッドが宣言された*interface_method_declaration*: %s

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

*属性*、 *return_type*、*識別子*、および*formal_parameter_list*のインターフェイスのメソッドの宣言が同じであります。クラスでメソッドの宣言のものと意味 ([メソッド](classes.md#methods))。 メソッドの本体を指定する、インターフェイス メソッド宣言は許可されていませんし、宣言したがって常にはセミコロンで終わます。

各インターフェイス メソッドの仮パラメーターの型は、入力の安全なをする必要があります ([差異の安全性](interfaces.md#variance-safety))、戻り値の型は、いずれかを指定する必要があります`void`または出力セーフ。 さらに、各クラス型制約、インターフェイス型制約、およびメソッドの型パラメーターの型パラメーターの制約は、入力の安全なをする必要があります。

これらのルールにより、共変のインターフェイスの使用状況を反変、タイプ セーフまたはします。 例えば以下のようにします。
```csharp
interface I<out T> { void M<U>() where U : T; }
```
無効ための使用状況`T`で型パラメーター制約として`U`入力セーフではありません。

配置でこの制限はなかったことが可能に次のようにタイプ セーフに違反します。
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
これは、実際に呼び出しを`C.M<E>`します。 呼び出しである必要がありますが、その`E`から派生`D`ので、ここでタイプ セーフに違反することです。

### <a name="interface-properties"></a>インターフェイスのプロパティ

使用してインターフェイスのプロパティが宣言された*interface_property_declaration*: %s

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

*属性*、*型*、および*識別子*、インターフェイス プロパティ宣言のクラスでのプロパティの宣言と同じ意味がある ([プロパティ](classes.md#properties))。

インターフェイス プロパティ宣言のアクセサーがクラスのプロパティ宣言のアクセサーに対応 ([アクセサー](classes.md#accessors)) ただし、アクセサーの本体は、セミコロンに常にあります。 したがって、アクセサーは、プロパティが読み取り/書き込み、読み取り専用または書き込み専用のかどうかを示すだけです。

インターフェイス プロパティの型は、get アクセサーがある場合の出力セーフである必要があり、入力の安全な set アクセサーがある場合があります。

### <a name="interface-events"></a>インターフェイスのイベント

インターフェイス イベント宣言を使用して*interface_event_declaration*: %s

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

*属性*、*型*、および*識別子*インターフェイス イベント宣言には、クラスでイベントの宣言のものと同じ意味を持ちます ([イベント](classes.md#events)).

インターフェイス イベントの種類は、入力の安全なである必要があります。

### <a name="interface-indexers"></a>インターフェイスのインデクサー

インターフェイスのインデクサーを宣言*interface_indexer_declaration*: %s

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

*属性*、*型*、および*formal_parameter_list*インターフェイスのインデクサーの宣言のクラス (でインデクサーの宣言のと同じ意味があります。[インデクサー](classes.md#indexers))。

インターフェイスのインデクサーの宣言のアクセサーがクラスのインデクサーの宣言のアクセサーに対応 ([インデクサー](classes.md#indexers)) ただし、アクセサーの本体は、セミコロンに常にあります。 したがって、アクセサーは、インデクサーが読み取り/書き込み、読み取り専用または書き込み専用のかどうかを示すだけです。

インターフェイスのインデクサーのすべての正式なパラメーター型は、入力の安全なである必要があります。 さらに、すべて`out`または`ref`仮パラメーターの型は出力セーフにもあります。 注も`out`パラメーターを基になる実行プラットフォームの制限により、入力セーフである必要があります。

インターフェイスのインデクサーの型は、get アクセサーがある場合の出力セーフである必要があり、入力の安全な set アクセサーがある場合があります。

### <a name="interface-member-access"></a>インターフェイス メンバーへのアクセス

インターフェイス メンバーはメンバー アクセスを通じてアクセス ([メンバー アクセス](expressions.md#member-access)) アクセスおよびインデクサー アクセス ([インデクサー アクセス](expressions.md#indexer-access)) 形式の式`I.M`と`I[A]`ここで、 `I`インターフェイス型は、`M`メソッド、プロパティ、またはそのインターフェイス型のイベントと`A`インデクサーの引数リストです。

厳密にはインターフェイスの単一継承 (継承チェーン内の各インターフェイスは、正確に 0 個または 1 つの直接基底インターフェイスを持つ)、メンバー検索の効果 ([メンバー ルックアップ](expressions.md#member-lookup))、メソッドの呼び出し ([メソッドの呼び出し](expressions.md#method-invocations))、およびインデクサー アクセス ([インデクサー アクセス](expressions.md#indexer-access)) ルールはまったく同じクラスと構造体です。メンバーの非表示には、同じ名前またはシグネチャを持つ派生メンバーを少なくする強い派生します。 ただし、多重継承インターフェイスでは、2 つに、あいまいさが発生することがまたは以上の関連付けられていない基本インターフェイスが同じ名前またはシグネチャを持つメンバーを宣言します。 このセクションでは、このような状況のいくつかの例を示します。 すべてのケースで、あいまいさを解決するのには明示的なキャストを使用できます。

例
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
最初の 2 つのステートメントのコンパイル時エラーが発生するため、メンバー検索 ([メンバー検索](expressions.md#member-lookup)) の`Count`で`IListCounter`があいまいです。 キャストすることで、あいまいさが解決されるように、例に示すように、`x`基底インターフェイスの適切な型にします。 このようなキャストには実行時のコストが必要ない、コンパイル時に弱い派生型として、インスタンスを表示するだけで構成されます。

例
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
呼び出し`n.Add(1)`選択`IInteger.Add`のオーバー ロード解決規則を適用することで[オーバー ロードの解決](expressions.md#overload-resolution)します。 同様に、呼び出し`n.Add(1.0)`選択`IDouble.Add`します。 明示的なキャストが挿入されると、メソッドとなるに 1 つのみの候補があります。

例
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
`IBase.F`によって隠されているメンバー、`ILeft.F`メンバー。 呼び出し`d.F(1)`ため選択`ILeft.F`場合でも、`IBase.F`を通じて潜在顧客のアクセス パスで非表示にしないよう`IRight`。

多重継承インターフェイスで非表示の直感的なルールは、これには単純です。メンバーが任意のアクセス パスで非表示の場合は、すべてのアクセス パスで表示されません。 からのアクセス パス`IDerived`に`ILeft`に`IBase`を非表示に`IBase.F`からのアクセス パスで、メンバーは非表示も`IDerived`に`IRight`に`IBase`します。

## <a name="fully-qualified-interface-member-names"></a>完全修飾インターフェイス メンバーの名前

インターフェイス メンバーはによって呼ばその***完全修飾名***します。 インターフェイス メンバーの完全修飾名をメンバーが宣言されている、続けて、ドット、後に、メンバーの名前のインターフェイスの名前で構成されます。 メンバーの完全修飾名では、メンバーが宣言されているインターフェイスを参照します。 たとえば、宣言があります。
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
完全修飾名`Paint`は`IControl.Paint`との完全修飾名`SetText`は`ITextBox.SetText`します。

上記の例ではないから参照できる`Paint`として`ITextBox.Paint`します。

インターフェイスは、名前空間の一部が、インターフェイス メンバーの完全修飾名には、名前空間の名前が含まれています。 次に例を示します。
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

ここでは、の完全修飾名、`Clone`メソッドは`System.ICloneable.Clone`します。

## <a name="interface-implementations"></a>インターフェイスの実装

クラスと構造体でインターフェイスを実装することがあります。 クラスまたは構造体を直接インターフェイスを実装するかを示す、クラスまたは構造体の基底クラス リストにインターフェイス識別子が含まれます。 例えば:
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

クラスまたは直接も直接インターフェイスを実装する構造体はインターフェイスの基本インターフェイスのすべて暗黙的に実装します。 これは、クラスまたは構造体により明示的に基底クラス リスト内のすべての基底インターフェイスが表示されない場合でも当てはまります。 例えば:
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

ここでは、クラス`TextBox`両方を実装`IControl`と`ITextBox`します。

クラスと`C`C から派生したすべてのクラス、インターフェイスを実装するインターフェイスを暗黙的に実装も直接します。 クラス宣言で指定された基本インターフェイスが構築されたインターフェイス型を指定できます ([構築型](types.md#constructed-types))。 スコープ内の型パラメーターを伴うことができますが、基本インターフェイスは、独自の型パラメーターにすることはできません。 次のコードは、クラスの実装および構築された型を拡張する方法を示しています。
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

ジェネリック クラス宣言の基本インターフェイスは、一意性の規則を満たす必要があります[実装されたインターフェイスの一意性](interfaces.md#uniqueness-of-implemented-interfaces)します。

### <a name="explicit-interface-member-implementations"></a>明示的なインターフェイス メンバーの実装

クラスまたは構造体を宣言できますインターフェイスを実装するために、***明示的なインターフェイス メンバーの実装***します。 明示的なインターフェイス メンバーの実装は、完全修飾インターフェイス メンバー名を参照するメソッド、プロパティ、イベント、またはインデクサーの宣言です。 次に例を示します。
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

ここで`IDictionary<int,T>.this`と`IDictionary<int,T>.Add`明示的なインターフェイスは、メンバーの実装です。

場合によっては、インターフェイス メンバーの名前のインターフェイス メンバーの場合可能性がありますが実装される明示的なインターフェイス メンバーの実装を使用して、実装するクラスの適切なしない場合があります。 ファイル アブストラクションを実装するクラスの実装は可能性があります、`Close`メンバー関数をファイル リソースが解放の効果があり、実装、`Dispose`のメソッド、`IDisposable`インターフェイスの明示的なインターフェイスを使用します。メンバーの実装:
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

メソッドの呼び出し、プロパティ アクセス、またはインデクサー アクセスで、完全修飾名を使って明示的なインターフェイス メンバーの実装にアクセスすることはできません。 明示的なインターフェイス メンバーの実装では、インターフェイス インスタンスからのみアクセスできるし、そのメンバーの名前だけで参照される場合。

アクセス修飾子を含めるには、明示的なインターフェイス メンバーの実装と、コンパイル時エラーし、修飾子を含めると、コンパイル時エラー `abstract`、 `virtual`、 `override`、または`static`します。

明示的なインターフェイス メンバーの実装では、他のメンバーとさまざまなアクセシビリティの特性があります。 明示的なインターフェイス メンバーの実装がメソッドの呼び出しまたはプロパティ アクセスで、完全修飾名からアクセスできることはありませんのでは意味がプライベートにあります。 ただし、インターフェイス インスタンスを通じてアクセスできる、ので意味もパブリックでは。

明示的なインターフェイス メンバーの実装では、2 つの主な目的を果たします。

*  明示的なインターフェイス メンバーの実装がクラスまたは構造体のインスタンスを通じてアクセス可能でないため、クラスまたは構造体のパブリック インターフェイスから除外するインターフェイスの実装が可能です。 これは、クラスの場合に特に役立ちます。 または構造体は、そのクラスまたは構造体のコンシューマーに関係のないのは内部インターフェイスを実装します。
*  明示的なインターフェイス メンバーの実装では、同じシグネチャを持つインターフェイス メンバーのあいまいさ排除を許可します。 明示的なインターフェイス メンバーの実装を含まないことはできませんクラスまたは構造体のさまざまな実装のインターフェイスのメンバーは同じシグネチャを持つことはできません、クラスまたは構造体の可能な任意の実装としての型を取得するにはまったく同じシグネチャではなく、さまざまなインターフェイス メンバーの型を返します。

明示的なインターフェイス メンバーの実装を有効にするには、クラスまたは構造体する必要がありますインターフェイスの名前、その基底クラス リストが完全修飾名、型、およびパラメーターの型と正確に一致明示的なインターフェイス メンバーのメンバーを含む実装です。 したがって、次のクラスで
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
宣言`IComparable.CompareTo`ため、コンパイル時エラー結果`IComparable`の基底クラス リストにも記載されていない`Shape`の基本インターフェイスでないと`ICloneable`します。 同様に、宣言内
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
宣言`ICloneable.Clone`で`Ellipse`ため、コンパイル時エラー結果`ICloneable`の基底クラス リストで明示的ににない`Ellipse`します。

インターフェイス メンバーの完全修飾名には、メンバーが宣言されたインターフェイスを参照する必要があります。 したがって、宣言内
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
明示的なインターフェイス メンバーの実装の`Paint`として記述する必要があります`IControl.Paint`します。

### <a name="uniqueness-of-implemented-interfaces"></a>実装されたインターフェイスの一意性

ジェネリック型の宣言によって実装されるインターフェイスは、すべての可能な構築された型に対して一意である必要があります。 この規則がないできなくなる正しいメソッドを呼び出して構築された特定の種類を特定できます。 たとえば、次のように書き込まれるジェネリック クラス宣言が許可されたとします。
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

これは許可された、できなくなる場合は、次を実行するコードを判断すること。
```csharp
I<int> x = new X<int,int>();
x.F();
```

インターフェイスのジェネリック型の宣言の一覧が有効であるを確認するのには、次の手順が実行されます。

*  ように`L`ジェネリック クラス、構造体、またはインターフェイスの宣言で直接指定するインターフェイスのリストである`C`します。
*  追加`L`任意の基本インターフェイスのインターフェイスを既に`L`します。
*  すべての重複を削除`L`します。
*  可能性のあるすべての構築型から作成された場合`C`は、型引数に置き換え後`L`、2 つのインターフェイスで`L`は同じでの宣言では、`C`が無効です。 すべての可能な構築された型を決定するときに、宣言の制約は考慮されません。

クラス宣言で`X`インターフェイス リストの上`L`から成る`I<U>`と`I<V>`します。 いずれかを持つ型が構築されたため、宣言は無効です`U`と`V`これら 2 つのインターフェイスと同じ型にすることによって、同じ型の中します。

別の継承レベルを統一する指定されたインターフェイスのことができます。
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

このコードは有効な場合でも`Derived<U,V>`両方を実装`I<U>`と`I<V>`します。 コード
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
メソッドを呼び出します`Derived`、ため`Derived<int,int>`効果的に再実装`I<int>`([インターフェイスの再実装](interfaces.md#interface-re-implementation))。

### <a name="implementation-of-generic-methods"></a>ジェネリック メソッドの実装

ジェネリック メソッドを暗黙的にメソッドを実装するインターフェイス、メソッド型パラメーターごとにする必要がありますと同等の両方の宣言で (任意のインターフェイス型のパラメーターが適切な型引数に置き換えられます) 後で指定される制約メソッド型パラメーターは、左から右に序数の位置によって識別されます。

ジェネリック メソッドは、インターフェイス メソッドを明示的に実装する、ただし、制約は許可されませんメソッドの実装にします。 代わりに、制約がインターフェイス メソッドから継承します。

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

メソッド`C.F<T>`は暗黙的に実装`I<object,C,string>.F<T>`します。 この場合、`C.F<T>`がないために必要な (も許可されている)、制約を指定する`T:object`ため`object`暗黙的な型のすべてのパラメーターの制約します。 メソッド`C.G<T>`は暗黙的に実装`I<object,C,string>.G<T>`制約に合わせてインターフェイスで、インターフェイスの型パラメーターは、対応する型引数に置き換えられます後ためです。 メソッドの制約`C.H<T>`、sealed 型であるために、エラー (`string`ここで) 制約として使用することはできません。 制約を省略すると、暗黙的なインターフェイス メソッドの実装の制約と一致する必要があるためエラーがあります。 そのため、暗黙的に実装することはできません`I<object,C,string>.H<T>`します。 このインターフェイス メソッドは、明示的なインターフェイス メンバーの実装を使用してのみ実装できます。
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

この例では、明示的なインターフェイス メンバーの実装は、厳密に弱い制約を持つパブリック メソッドを呼び出します。 なおから代入された`t`に`s`以降が正しく`T`の制約を継承`T:string`この制約はソース コードで表現できる場合でも。

### <a name="interface-mapping"></a>インターフェイスの割り当て

クラスまたは構造体には、クラスまたは構造体の基底クラス リストに記載されているインターフェイスのすべてのメンバーの実装を提供する必要があります。 実装するクラスまたは構造体でインターフェイス メンバーの実装を検索するプロセスと呼びます***インターフェイス マップ***します。

クラスまたは構造体のインターフェイス マップ`C`の基底クラス リストで指定された各インターフェイスの各メンバーの実装を探します`C`します。 特定のインターフェイス メンバーの実装`I.M`ここで、`I`をインターフェイスには、メンバー`M`が宣言されている場合は、各クラスまたは構造体を調べることで決定されます`S`以降の`C`と連続する各基底クラスの繰り返し`C`一致が見つかるまで。

*  場合`S`と一致する明示的なインターフェイス メンバーの実装の宣言を含む`I`と`M`、このメンバーは、実装の`I.M`します。
*  の場合`S`と一致する非静的なパブリック メンバーの宣言を含む`M`、このメンバーは、実装の`I.M`します。 メンバーの実装は、1 つのメンバーの一致よりも多く指定がない場合`I.M`します。 このような状況はできる場合にのみ発生`S`が構築された型をジェネリック型で宣言されている 2 つのメンバーが異なるシグネチャを持つが、型引数を指定すること、署名は同じです。

コンパイル時エラーは、実装は、の基底クラス リストで指定されたすべてのインターフェイスのすべてのメンバーが見つからない場合に発生します。`C`します。 インターフェイスのメンバーが基底インターフェイスから継承されたメンバーを含めることに注意してください。

インターフェイス マップ、クラス メンバーの目的で`A`インターフェイスのメンバーと一致する`B`とき。

*  `A` `B`メソッド、および、名前、種類、および仮パラメーター リスト`A`と`B`は同じです。
*  `A` `B` 、プロパティ、名前および種類の`A`と`B`同じですと`A`と同じアクセサーを持つ`B`(`A`明示的なインターフェイスでない場合は、追加のアクセサーを持つことができますメンバーの実装の場合)。
*  `A` `B`イベントは、名前と種類の`A`と`B`は同じです。
*  `A` `B`インデクサーでは、型と仮パラメーター リストは`A`と`B`同じですと`A`と同じアクセサーを持つ`B`(`A`でない場合は、追加のアクセサーを持つことができます、明示的なインターフェイス メンバーの実装)。

インターフェイス マップのアルゴリズムの主な特性は次のとおりです。

*  明示的なインターフェイス メンバーの実装は、同じクラスまたは構造体の他のメンバーに優先される、インターフェイス メンバーを実装するクラスまたは構造体のメンバーを決定するときにします。
*  非パブリックも静的でもないメンバーは、インターフェイスのマッピングに参加します。

例
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
`ICloneable.Clone`のメンバー`C`の実装になります`Clone`で`ICloneable`理由は、明示的なインターフェイス メンバーの実装が他のメンバーより優先順位。

同じ名前、種類、およびパラメーターの型を持つメンバーを格納している以上のインターフェイスをクラスまたは構造体は 2 つを実装している場合は、1 つのクラスまたは構造体メンバー上にこれらのインターフェイス メンバーの各をマップすること勧めします。 次に例を示します。
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

ここでは、`Paint`の両方のメソッド`IControl`と`IForm`にマップされて、`Paint`メソッド`Page`します。 もちろんも 2 つのメソッドの別の明示的なインターフェイス メンバーの実装を含めることは可能です。

クラスまたは構造体は、非表示のメンバーを格納しているインターフェイスを実装する場合は、明示的なインターフェイス メンバーの実装を通じて、一部のメンバーとは限りません実装する必要があります。 次に例を示します。
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

このインターフェイスの実装に少なくとも 1 つの明示的なインターフェイス メンバーの実装では、必要し、なります。 形式は次のいずれか
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

クラスは、同じ基本インターフェイスを持つ複数のインターフェイスを実装するときに、基本インターフェイスの 1 つだけの実装があります。 例
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
別の実装を持つことはできません、`IControl`基底クラス リストで、名前付き、`IControl`によって継承`ITextBox`、および`IControl`によって継承`IListBox`。 実際には、これらのインターフェイスに別々 の id の概念はありません。 実装ではなく、`ITextBox`と`IListBox`の同じ実装を共有`IControl`、および`ComboBox`は単に 3 つのインターフェイスを実装すると見なされます`IControl`、 `ITextBox`、および`IListBox`します。

基底クラスのメンバーは、インターフェイスのマッピングに参加します。 例
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
メソッド`F`で`Class1`で使用されて`Class2`の実装の`Interface1`します。

### <a name="interface-implementation-inheritance"></a>インターフェイス実装の継承

クラスは、その基本クラスによって提供されるすべてのインターフェイスの実装を継承します。

せずに明示的に***再実装する***インターフェイス、派生クラス何らかの方法で変更できませんその基本クラスから継承するインターフェイス マップ。 たとえば、宣言内
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
`Paint`メソッド`TextBox`を非表示に、`Paint`メソッド`Control`のマッピングは変更されませんが、`Control.Paint`に`IControl.Paint`への呼び出しと`Paint`クラスとインターフェイスのインスタンスが、次の効果します。
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

ただし、インターフェイス メソッドがクラスでの仮想メソッドにマッピングされると、仮想メソッドをオーバーライドし、インターフェイスの実装を変更する派生クラスです。 たとえば、上記の宣言の書き換え
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
次の効果は観察されたようになりました
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

明示的なインターフェイス メンバーの実装を virtual と宣言することはできません、ために、明示的なインターフェイス メンバーの実装をオーバーライドすることはできません。 ただし、ことは、別のメソッドを呼び出す明示的なインターフェイス メンバーの実装は有効ですし、オーバーライドするクラスを派生する他のメソッドを許可する仮想宣言することができます、します。 次に例を示します。
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

ここから派生したクラス`Control`の実装を特化できる`IControl.Paint`オーバーライドすることで、`PaintControl`メソッド。

### <a name="interface-re-implementation"></a>インターフェイスの再実装

インターフェイスの実装を継承するクラスが許可されている***再実装***基底クラス リストに含めることによってインターフェイス。

インターフェイスの再実装では、まったく同じインターフェイス マッピング規則として、インターフェイスの初期の実装に従います。 そのため、継承されたインターフェイスの割り当ては、インターフェイスの再実装用に確立されたインターフェイス マップに一切影響を与えません。 たとえば、宣言内
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
事実を`Control`マップ`IControl.Paint`に`Control.IControl.Paint`で再実装には影響しません`MyControl`、マップを`IControl.Paint`に`MyControl.Paint`します。

パブリック メンバーの宣言と明示的なインターフェイスの継承されたメンバーの宣言が再実装されたインターフェイスのインターフェイス マップ プロセスに参加を継承します。 次に例を示します。
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

ここでは、の実装`IMethods`で`Derived`にインターフェイス メソッドのマップ`Derived.F`、 `Base.IMethods.G`、 `Derived.IMethods.H`、および`Base.I`します。

クラス インターフェイスを実装するときに、暗黙的にもすべてのインターフェイスの基本インターフェイスを実装します。 同様に、インターフェイスの再実装も暗黙的にすべてのインターフェイスの基本インターフェイスの再実装できます。 次に例を示します。
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

ここでは、の再実装`IDerived`も再実装`IBase`マッピング`IBase.F`に`D.F`します。

### <a name="abstract-classes-and-interfaces"></a>抽象クラスとインターフェイス

抽象クラスは、非抽象クラスと同様に、クラスの基底クラス リストに記載されているインターフェイスのすべてのメンバーの実装を提供する必要があります。 ただし、抽象クラスは、インターフェイス メソッドを抽象メソッドにマップする許可されます。 次に例を示します。
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

ここでは、の実装`IMethods`マップ`F`と`G`を抽象メソッドは、上にから派生した非抽象クラスでオーバーライドする必要があります`C`します。

明示的なインターフェイス メンバーの実装を抽象にすることはできませんが、明示的なインターフェイス メンバーの実装が抽象メソッドの呼び出しを許可はもちろんことに注意してください。 次に例を示します。
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

ここでは、非抽象クラスから派生した`C`をオーバーライドする必要があります`FF`と`GG`、したがっての実際の実装を提供する`IMethods`します。
