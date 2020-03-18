---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/01/2020
ms.locfileid: "79484130"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="7e9db-101">進行中の作業を記録する</span><span class="sxs-lookup"><span data-stu-id="7e9db-101">Records Work-in-Progress</span></span>

<span data-ttu-id="7e9db-102">他のレコードの提案とは異なり、これはそれ自体での提案ではありませんが、レコード機能の合意の設計上の決定を記録するように設計された作業の進行状況です。</span><span class="sxs-lookup"><span data-stu-id="7e9db-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="7e9db-103">質問を解決するために、必要に応じて仕様の詳細が追加されます。</span><span class="sxs-lookup"><span data-stu-id="7e9db-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="7e9db-104">レコードの構文は、次のように追加されることが提案されます。</span><span class="sxs-lookup"><span data-stu-id="7e9db-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="7e9db-105">非ターミナル `attributes` では、新しいコンテキスト属性 `data`も許可されます。</span><span class="sxs-lookup"><span data-stu-id="7e9db-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="7e9db-106">パラメーターリストまたは `data` 修飾子で宣言されたクラス (構造体) はレコードクラス (レコード構造体) と呼ばれ、そのいずれかがレコード型です。</span><span class="sxs-lookup"><span data-stu-id="7e9db-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="7e9db-107">パラメーターリストと `data` 修飾子の両方を指定せずにレコードの種類を宣言すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="7e9db-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="7e9db-108">レコードの種類のメンバー</span><span class="sxs-lookup"><span data-stu-id="7e9db-108">Members of a record type</span></span>

<span data-ttu-id="7e9db-109">レコード型には、クラス本体で宣言されたメンバーに加えて、次の追加メンバーがあります。</span><span class="sxs-lookup"><span data-stu-id="7e9db-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="7e9db-110">プライマリコンストラクター</span><span class="sxs-lookup"><span data-stu-id="7e9db-110">Primary Constructor</span></span>

<span data-ttu-id="7e9db-111">レコード型には、型宣言の値パラメーターに対応するシグネチャを持つパブリックコンストラクターがあります。</span><span class="sxs-lookup"><span data-stu-id="7e9db-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="7e9db-112">これは、型のプライマリコンストラクターと呼ばれ、暗黙的に宣言された既定のコンストラクターは抑制されます。</span><span class="sxs-lookup"><span data-stu-id="7e9db-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="7e9db-113">プライマリコンストラクターと、同じシグネチャを持つコンストラクターがクラスに既に存在する場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="7e9db-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="7e9db-114">実行時のプライマリコンストラクター</span><span class="sxs-lookup"><span data-stu-id="7e9db-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="7e9db-115">クラス本体に出現するインスタンスフィールド初期化子を実行します。次に、引数なしで基底クラスのコンストラクターを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7e9db-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="7e9db-116">値パラメーターに対応するプロパティについて、コンパイラによって生成されるバッキングフィールドを初期化します (これらのプロパティがコンパイラによって提供される場合は、「合成された[プロパティ](#Synthesized Properties)」を参照)。</span><span class="sxs-lookup"><span data-stu-id="7e9db-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="7e9db-117">[] TODO: オーバーロードの解決による基本コンストラクターの選択に関する基本呼び出しの構文と仕様を追加します。</span><span class="sxs-lookup"><span data-stu-id="7e9db-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="7e9db-118">Properties</span><span class="sxs-lookup"><span data-stu-id="7e9db-118">Properties</span></span>

<span data-ttu-id="7e9db-119">レコード型宣言の各レコードパラメーターには、対応するパブリックプロパティメンバーがあります。このメンバーの名前と型は、値パラメーターの宣言から取得されます。</span><span class="sxs-lookup"><span data-stu-id="7e9db-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="7e9db-120">Get アクセサーを持つ具象 (つまり非抽象) プロパティが明示的に宣言または継承されていない場合は、コンパイラによって次のように生成されます。</span><span class="sxs-lookup"><span data-stu-id="7e9db-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="7e9db-121">レコード構造体またはレコードクラスの場合:</span><span class="sxs-lookup"><span data-stu-id="7e9db-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="7e9db-122">パブリックの get 専用自動プロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="7e9db-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="7e9db-123">この値は、対応するプライマリコンストラクターパラメーターの値を使用して構築時に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="7e9db-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="7e9db-124">各 "一致する" 継承された抽象プロパティの get アクセサーはオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="7e9db-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="7e9db-125">等値メンバー</span><span class="sxs-lookup"><span data-stu-id="7e9db-125">Equality members</span></span>

<span data-ttu-id="7e9db-126">レコード型は、次のメソッドに対して合成された実装を生成します。</span><span class="sxs-lookup"><span data-stu-id="7e9db-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="7e9db-127">シールされているかユーザーが指定されていない限り、`object.GetHashCode()` オーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="7e9db-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="7e9db-128">シールされているかユーザーが指定されていない限り、`object.Equals(object)` オーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="7e9db-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="7e9db-129">`T Equals(T)` メソッド (`T` が現在の型である場合)</span><span class="sxs-lookup"><span data-stu-id="7e9db-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="7e9db-130">値の等価性を実行するために `T Equals(T)` が指定されています。各プライマリコンストラクターのパラメーターと同じ名前のプロパティを、他の型の対応するプロパティに比較しています。</span><span class="sxs-lookup"><span data-stu-id="7e9db-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="7e9db-131">`object.Equals` は、</span><span class="sxs-lookup"><span data-stu-id="7e9db-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```
