---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/01/2020
ms.locfileid: "79484130"
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

### <a name="properties"></a>Properties

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
