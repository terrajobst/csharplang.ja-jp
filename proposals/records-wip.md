---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281958"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="1d617-101">進行中の作業を記録する</span><span class="sxs-lookup"><span data-stu-id="1d617-101">Records Work-in-Progress</span></span>

<span data-ttu-id="1d617-102">他のレコードの提案とは異なり、これはそれ自体での提案ではありませんが、レコード機能の合意の設計上の決定を記録するように設計された作業の進行状況です。</span><span class="sxs-lookup"><span data-stu-id="1d617-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="1d617-103">質問を解決するために、必要に応じて仕様の詳細が追加されます。</span><span class="sxs-lookup"><span data-stu-id="1d617-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="1d617-104">レコードの構文は、次のように追加されることが提案されます。</span><span class="sxs-lookup"><span data-stu-id="1d617-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="1d617-105">非ターミナル `attributes` では、新しいコンテキスト属性 `data`も許可されます。</span><span class="sxs-lookup"><span data-stu-id="1d617-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="1d617-106">パラメーターリストまたは `data` 修飾子で宣言されたクラス (構造体) はレコードクラス (レコード構造体) と呼ばれ、そのいずれかがレコード型です。</span><span class="sxs-lookup"><span data-stu-id="1d617-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="1d617-107">パラメーターリストと `data` 修飾子の両方を指定せずにレコードの種類を宣言すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="1d617-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="1d617-108">レコードの種類のメンバー</span><span class="sxs-lookup"><span data-stu-id="1d617-108">Members of a record type</span></span>

<span data-ttu-id="1d617-109">レコード型には、クラス本体で宣言されたメンバーに加えて、次の追加メンバーがあります。</span><span class="sxs-lookup"><span data-stu-id="1d617-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="1d617-110">プライマリコンストラクター</span><span class="sxs-lookup"><span data-stu-id="1d617-110">Primary Constructor</span></span>

<span data-ttu-id="1d617-111">レコード型には、型宣言の値パラメーターに対応するシグネチャを持つパブリックコンストラクターがあります。</span><span class="sxs-lookup"><span data-stu-id="1d617-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="1d617-112">これは、型のプライマリコンストラクターと呼ばれ、暗黙的に宣言された既定のコンストラクターは抑制されます。</span><span class="sxs-lookup"><span data-stu-id="1d617-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="1d617-113">プライマリコンストラクターと、同じシグネチャを持つコンストラクターがクラスに既に存在する場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="1d617-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="1d617-114">実行時のプライマリコンストラクター</span><span class="sxs-lookup"><span data-stu-id="1d617-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="1d617-115">クラス本体に出現するインスタンスフィールド初期化子を実行します。次に、引数なしで基底クラスのコンストラクターを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1d617-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="1d617-116">値パラメーターに対応するプロパティについて、コンパイラによって生成されるバッキングフィールドを初期化します (これらのプロパティがコンパイラによって提供される場合は、「合成された[プロパティ](#Synthesized Properties)」を参照)。</span><span class="sxs-lookup"><span data-stu-id="1d617-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="1d617-117">[] TODO: オーバーロードの解決による基本コンストラクターの選択に関する基本呼び出しの構文と仕様を追加します。</span><span class="sxs-lookup"><span data-stu-id="1d617-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="1d617-118">プロパティ</span><span class="sxs-lookup"><span data-stu-id="1d617-118">Properties</span></span>

<span data-ttu-id="1d617-119">レコード型宣言の各レコードパラメーターには、対応するパブリックプロパティメンバーがあります。このメンバーの名前と型は、値パラメーターの宣言から取得されます。</span><span class="sxs-lookup"><span data-stu-id="1d617-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="1d617-120">Get アクセサーを持つ具象 (つまり非抽象) プロパティが明示的に宣言または継承されていない場合は、コンパイラによって次のように生成されます。</span><span class="sxs-lookup"><span data-stu-id="1d617-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="1d617-121">レコード構造体またはレコードクラスの場合:</span><span class="sxs-lookup"><span data-stu-id="1d617-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="1d617-122">パブリックの get 専用自動プロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1d617-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="1d617-123">この値は、対応するプライマリコンストラクターパラメーターの値を使用して構築時に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="1d617-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="1d617-124">各 "一致する" 継承された抽象プロパティの get アクセサーはオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="1d617-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="1d617-125">等値メンバー</span><span class="sxs-lookup"><span data-stu-id="1d617-125">Equality members</span></span>

<span data-ttu-id="1d617-126">レコード型は、次のメソッドに対して合成された実装を生成します。</span><span class="sxs-lookup"><span data-stu-id="1d617-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="1d617-127">シールされているかユーザーが指定されていない限り、`object.GetHashCode()` オーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="1d617-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="1d617-128">シールされているかユーザーが指定されていない限り、`object.Equals(object)` オーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="1d617-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="1d617-129">`T Equals(T)` メソッド (`T` が現在の型である場合)</span><span class="sxs-lookup"><span data-stu-id="1d617-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="1d617-130">値の等価性を実行するために `T Equals(T)` が指定されています。各プライマリコンストラクターのパラメーターと同じ名前のプロパティを、他の型の対応するプロパティに比較しています。</span><span class="sxs-lookup"><span data-stu-id="1d617-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="1d617-131">`object.Equals` は、</span><span class="sxs-lookup"><span data-stu-id="1d617-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="1d617-132">`with` 式</span><span class="sxs-lookup"><span data-stu-id="1d617-132">`with` expression</span></span>

<span data-ttu-id="1d617-133">`with` 式は、次の構文を使用した新しい式です。</span><span class="sxs-lookup"><span data-stu-id="1d617-133">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="1d617-134">`with` 式を使用すると、"非破壊的な変異" が可能になります。これは、`anonymous_object_initializer`に示されているプロパティを変更して、レシーバー式のコピーを生成するように設計されています。</span><span class="sxs-lookup"><span data-stu-id="1d617-134">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="1d617-135">有効な `with` 式には、void 以外の型のレシーバーがあります。</span><span class="sxs-lookup"><span data-stu-id="1d617-135">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="1d617-136">レシーバー型には、適切なパラメーターと戻り値の型を持つ `With` というアクセス可能なインスタンスメソッドが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d617-136">The receiver type must contain an accessible instance method called `With` with the appropriate parameters and return type.</span></span> <span data-ttu-id="1d617-137">非オーバーライド `With` メソッドが複数ある場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="1d617-137">It is an error if there are multiple non-override `With` methods.</span></span> <span data-ttu-id="1d617-138">複数の `With` オーバーライドがある場合は、対象のメソッドである非オーバーライド `With` メソッドが必要です。</span><span class="sxs-lookup"><span data-stu-id="1d617-138">If there are multiple `With` overrides, there must be a non-override `With` method, which is the target method.</span></span> <span data-ttu-id="1d617-139">それ以外の場合は、`With` メソッドを1つだけ指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d617-139">Otherwise, there must be exactly one `With` method.</span></span>

<span data-ttu-id="1d617-140">`with` 式の右側には、代入のシーケンスを持つ `anonymous_object_initializer` が割り当ての左側にある受信側のフィールドまたはプロパティ、右辺の任意の式 (左辺の型に暗黙的に変換可能) が含まれていることが条件となります。</span><span class="sxs-lookup"><span data-stu-id="1d617-140">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments with a field or property of the receiver on the left-hand side of the assignment, and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the left-hand side.</span></span>

<span data-ttu-id="1d617-141">ターゲット `With` メソッドを指定した場合、戻り値の型は、レシーバー式の型の型、またはその基本型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d617-141">Given a target `With` method, the return type must be the type of the receiver expression type, or a base type thereof.</span></span> <span data-ttu-id="1d617-142">`With` メソッドの各パラメーターに対して、同じ名前および同じ型のレシーバー型の、アクセス可能な対応するインスタンスフィールドまたは読み取り可能なプロパティが必要です。</span><span class="sxs-lookup"><span data-stu-id="1d617-142">For each parameter of the `With` method, there must be an accessible corresponding instance field or readable property on the receiver type with the same name and the same type.</span></span> <span data-ttu-id="1d617-143">また、式の右側にある各プロパティまたはフィールドは、`With` メソッドの同じ名前のパラメーターにも対応している必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d617-143">Each property or field in the right-hand side of the With expression must also correspond to a parameter of the same name in the `With` method.</span></span>

<span data-ttu-id="1d617-144">有効な `With` メソッドを指定した場合、`with` 式の評価は、左側のプロパティと同じ名前のパラメーターに置き換えられた `anonymous_object_initializer` 内の式を使用して `With` メソッドを呼び出すことと同じです。</span><span class="sxs-lookup"><span data-stu-id="1d617-144">Given a valid `With` method, the evaluation of a `with` expression is equivalent to calling the `With` method with the expressions in the `anonymous_object_initializer` substituted for the parameter of the same name as the property on the left hand side.</span></span> <span data-ttu-id="1d617-145">`anonymous_object_initializer`内の特定のパラメーターに一致するプロパティがない場合、引数は、受信側で同じ名前のフィールドまたはプロパティを評価します。</span><span class="sxs-lookup"><span data-stu-id="1d617-145">If there is no matching property for a given parameter in the `anonymous_object_initializer`, the argument is the evaluation of the field or property of the same name on the receiver.</span></span>

<span data-ttu-id="1d617-146">副作用の評価の順序は次のとおりです。各式は1回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="1d617-146">The order of evaluation of side effects is as follows, with each expression evaluated exactly once:</span></span>

1. <span data-ttu-id="1d617-147">レシーバー式</span><span class="sxs-lookup"><span data-stu-id="1d617-147">Receiver expression</span></span>

2. <span data-ttu-id="1d617-148">`anonymous_object_initializer`内の式 (構文の順序)</span><span class="sxs-lookup"><span data-stu-id="1d617-148">Expressions in the `anonymous_object_initializer`, in lexical order</span></span>

3. <span data-ttu-id="1d617-149">`With` メソッドパラメーターの定義順に、`With` メソッドのパラメーターに一致するプロパティの評価。</span><span class="sxs-lookup"><span data-stu-id="1d617-149">The evaluation of any properties matching the `With` method parameters, in order of definition of the `With` method parameters.</span></span>