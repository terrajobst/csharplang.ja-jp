---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281958"
---
# <a name="records-work-in-progress"></a>進行中の作業を記録する

他のレコードの提案とは異なり、これはそれ自体での提案ではありませんが、レコード機能の合意の設計上の決定を記録するように設計された作業の進行状況です。 質問を解決するために、必要に応じて仕様の詳細が追加されます。

レコードの構文は、次のように追加されることが提案されます。

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

非ターミナル `attributes` では、新しいコンテキスト属性 `data`も許可されます。

パラメーターリストまたは `data` 修飾子で宣言されたクラス (構造体) はレコードクラス (レコード構造体) と呼ばれ、そのいずれかがレコード型です。

パラメーターリストと `data` 修飾子の両方を指定せずにレコードの種類を宣言すると、エラーになります。

## <a name="members-of-a-record-type"></a>レコードの種類のメンバー

レコード型には、クラス本体で宣言されたメンバーに加えて、次の追加メンバーがあります。

### <a name="primary-constructor"></a>プライマリコンストラクター

レコード型には、型宣言の値パラメーターに対応するシグネチャを持つパブリックコンストラクターがあります。 これは、型のプライマリコンストラクターと呼ばれ、暗黙的に宣言された既定のコンストラクターは抑制されます。 プライマリコンストラクターと、同じシグネチャを持つコンストラクターがクラスに既に存在する場合、エラーになります。
実行時のプライマリコンストラクター 

1. クラス本体に出現するインスタンスフィールド初期化子を実行します。次に、引数なしで基底クラスのコンストラクターを呼び出します。

1. 値パラメーターに対応するプロパティについて、コンパイラによって生成されるバッキングフィールドを初期化します (これらのプロパティがコンパイラによって提供される場合は、「合成された[プロパティ](#Synthesized Properties)」を参照)。


[] TODO: オーバーロードの解決による基本コンストラクターの選択に関する基本呼び出しの構文と仕様を追加します。

### <a name="properties"></a>プロパティ

レコード型宣言の各レコードパラメーターには、対応するパブリックプロパティメンバーがあります。このメンバーの名前と型は、値パラメーターの宣言から取得されます。 Get アクセサーを持つ具象 (つまり非抽象) プロパティが明示的に宣言または継承されていない場合は、コンパイラによって次のように生成されます。

レコード構造体またはレコードクラスの場合:

* パブリックの get 専用自動プロパティが作成されます。 この値は、対応するプライマリコンストラクターパラメーターの値を使用して構築時に初期化されます。 各 "一致する" 継承された抽象プロパティの get アクセサーはオーバーライドされます。

### <a name="equality-members"></a>等値メンバー

レコード型は、次のメソッドに対して合成された実装を生成します。

* シールされているかユーザーが指定されていない限り、`object.GetHashCode()` オーバーライドします。
* シールされているかユーザーが指定されていない限り、`object.Equals(object)` オーバーライドします。
* `T Equals(T)` メソッド (`T` が現在の型である場合)

値の等価性を実行するために `T Equals(T)` が指定されています。各プライマリコンストラクターのパラメーターと同じ名前のプロパティを、他の型の対応するプロパティに比較しています。
`object.Equals` は、

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a>`with` 式

`with` 式は、次の構文を使用した新しい式です。

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

`with` 式を使用すると、"非破壊的な変異" が可能になります。これは、`anonymous_object_initializer`に示されているプロパティを変更して、レシーバー式のコピーを生成するように設計されています。

有効な `with` 式には、void 以外の型のレシーバーがあります。 レシーバー型には、適切なパラメーターと戻り値の型を持つ `With` というアクセス可能なインスタンスメソッドが含まれている必要があります。 非オーバーライド `With` メソッドが複数ある場合、エラーになります。 複数の `With` オーバーライドがある場合は、対象のメソッドである非オーバーライド `With` メソッドが必要です。 それ以外の場合は、`With` メソッドを1つだけ指定する必要があります。

`with` 式の右側には、代入のシーケンスを持つ `anonymous_object_initializer` が割り当ての左側にある受信側のフィールドまたはプロパティ、右辺の任意の式 (左辺の型に暗黙的に変換可能) が含まれていることが条件となります。

ターゲット `With` メソッドを指定した場合、戻り値の型は、レシーバー式の型の型、またはその基本型である必要があります。 `With` メソッドの各パラメーターに対して、同じ名前および同じ型のレシーバー型の、アクセス可能な対応するインスタンスフィールドまたは読み取り可能なプロパティが必要です。 また、式の右側にある各プロパティまたはフィールドは、`With` メソッドの同じ名前のパラメーターにも対応している必要があります。

有効な `With` メソッドを指定した場合、`with` 式の評価は、左側のプロパティと同じ名前のパラメーターに置き換えられた `anonymous_object_initializer` 内の式を使用して `With` メソッドを呼び出すことと同じです。 `anonymous_object_initializer`内の特定のパラメーターに一致するプロパティがない場合、引数は、受信側で同じ名前のフィールドまたはプロパティを評価します。

副作用の評価の順序は次のとおりです。各式は1回だけ評価されます。

1. レシーバー式

2. `anonymous_object_initializer`内の式 (構文の順序)

3. `With` メソッドパラメーターの定義順に、`With` メソッドのパラメーターに一致するプロパティの評価。