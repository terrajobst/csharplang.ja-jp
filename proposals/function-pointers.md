---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108390"
---
# <a name="function-pointers"></a>関数ポインター

## <a name="summary"></a>要約

この提案には、現在のC#ところ効率的にアクセスできない IL オペコードを公開する言語構造、つまり `ldftn` および `calli`が用意されています。 これらの IL オペコードは、高パフォーマンスのコードでは重要であり、開発者はそれらにアクセスするための効率的な方法を必要とします。

## <a name="motivation"></a>目的

この機能の動機と背景については、次の問題で説明されています (この機能が実装される可能性があります)。

https://github.com/dotnet/csharplang/issues/191

これは、[コンパイラの組み込み](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)に対する別の設計提案です。

## <a name="detailed-design"></a>詳細なデザイン

### <a name="function-pointers"></a>関数ポインター

この言語では、`delegate*` 構文を使用して関数ポインターを宣言できます。 完全な構文については次のセクションで詳しく説明しますが、`Func` および `Action` 型宣言で使用される構文に似ています。

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

これらの型は、ECMA-335 で説明されているように、関数ポインター型を使用して表されます。 つまり、`delegate*` の呼び出しでは `calli` が使用され、`delegate` の呼び出しでは、`Invoke` メソッドで `callvirt` が使用されます。
構文的には、どちらのコンストラクトでも呼び出しは同じです。

メソッドポインターの ECMA-335 定義には、型シグネチャ (セクション 7.1) の一部として呼び出し規約が含まれています。
既定の呼び出し規約は `managed`になります。 代替形式を指定するには、`delegate*` 構文の後に適切な修飾子を追加します。 `managed`、`cdecl`、`stdcall`、`thiscall`、または `unmanaged`です。 例:

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

`delegate*` 型間の変換は、呼び出し規約を含むシグネチャに基づいて行われます。

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

`delegate*` 型はポインター型です。これは、標準ポインター型のすべての機能と制限が含まれていることを意味します。

- `unsafe` コンテキストでのみ有効です。
- `delegate*` パラメーターまたは戻り値の型を含むメソッドは、`unsafe` のコンテキストからのみ呼び出すことができます。
- を `object`に変換することはできません。
- を汎用引数として使用することはできません。
- `delegate*` を `void*`に暗黙的に変換できます。
- `void*` から `delegate*`に明示的に変換できます。

制限:

- カスタム属性は、`delegate*` またはその要素には適用できません。
- `delegate*` パラメーターを `params` としてマークすることはできません
- `delegate*` 型には、通常のポインター型のすべての制限があります。

### <a name="function-pointer-syntax"></a>関数ポインターの構文

関数ポインターの完全な構文は、次の文法で表されます。

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

`unmanaged` 呼び出し規約は、現在のプラットフォームでのネイティブコードの既定の呼び出し規約を表し、winapi としてエンコードされます。
すべての `calling_convention`s は、`delegate*`が前にある場合のコンテキストキーワードです。

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a>関数ポインターの変換

Unsafe コンテキストでは、次の暗黙的なポインター変換を含むように、使用可能な暗黙の変換 (暗黙の変換) のセットが拡張されます。
- [_既存の変換_](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- 次のすべての条件に該当する場合は、ファンク_ptr\_型_`F0` を別のファンク_ptr\_型_`F1`に指定します。
    - `F0` と `F1` は同じ数のパラメーターを持ち、`F0` 内の各 `D0n` パラメーターには `ref`の対応するパラメーター `out`と同じ `in`、`D1n`、または `F1`の修飾子があります。
    - 各値パラメーター (`ref`、`out`、または `in` 修飾子のないパラメーター) では、`F0` のパラメーター型から `F1`の対応するパラメーター型に、id 変換、暗黙の参照変換、または暗黙的なポインター変換が存在します。
    - `ref`、`out`、または `in` パラメーターごとに、`F0` のパラメーターの型は `F1`の対応するパラメーターの型と同じになります。
    - 戻り値の型が値渡し (`ref` も `ref readonly`) である場合、`F1` の戻り値の型から `F0`の戻り値の型に、id、暗黙の参照、または暗黙的なポインターの変換が存在します。
    - 戻り値の型が参照渡し (`ref` または `ref readonly`) の場合、`F1` の戻り値の型と `ref` 修飾子は `ref` の戻り値の型と `F0`修飾子と同じになります。
    - `F0` の呼び出し規約は、`F1`の呼び出し規約と同じです。

### <a name="allow-address-of-to-target-methods"></a>ターゲットメソッドへのアドレスの許可

アドレス式の引数として、メソッドグループが許可されるようになりました。 このような式の型は、ターゲットメソッドとマネージ呼び出し規約の等価のシグネチャを持つ `delegate*` になります。

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

Unsafe コンテキストでは、メソッド `M` は、次のすべてに該当する場合に `F` 関数ポインター型と互換性があります。
- `M` と `F` は同じ数のパラメーターを持ち、`D` 内の各パラメーターには、`out`内の対応するパラメーターと同じ `ref`、`in`、または `F`修飾子があります。
- 各値パラメーター (`ref`、`out`、または `in` 修飾子のないパラメーター) では、`M` のパラメーター型から `F`の対応するパラメーター型に、id 変換、暗黙の参照変換、または暗黙的なポインター変換が存在します。
- `ref`、`out`、または `in` パラメーターごとに、`M` のパラメーターの型は `F`の対応するパラメーターの型と同じになります。
- 戻り値の型が値渡し (`ref` も `ref readonly`) である場合、`F` の戻り値の型から `M`の戻り値の型に、id、暗黙の参照、または暗黙的なポインターの変換が存在します。
- 戻り値の型が参照渡し (`ref` または `ref readonly`) の場合、`F` の戻り値の型と `ref` 修飾子は `ref` の戻り値の型と `M`修飾子と同じになります。
- `M` の呼び出し規約は、`F`の呼び出し規約と同じです。
- `M` は静的メソッドです。

Unsafe コンテキストでは、次に示すように、ターゲットがメソッドグループであり、互換性のある関数ポインター `F` 型に `E` メソッドグループであるか、`F`のパラメーターの型と修飾子を使用して構築された引数リストに適用可能なメソッドが少なくとも1つ `E` 含まれている場合は、暗黙的な変換が存在します。
- フォーム `E(A)` のメソッド呼び出しに対応する1つのメソッド `M` が、次のように変更されました。
   - 引数リスト `A` は式の一覧であり、それぞれが変数として分類され、\_の対応する_正式\_パラメーター `D`リスト_の型および修飾子 (`ref`、`out`、または `in`) を使用します。
   - 候補となるメソッドは、通常の形式で適用可能なメソッドのみであり、拡張された形式では適用できません。
   - 候補となるメソッドは、静的メソッドのみです。
- メソッド呼び出しのアルゴリズムによってエラーが発生した場合、コンパイル時エラーが発生します。 それ以外の場合、`F` と同じ数のパラメーターを持ち、変換が存在していると見なされる `M`、1つの最適なメソッドが生成されます。
- 選択されたメソッド `M` は、関数ポインター型 `F`と互換性がある (前述のとおり) 必要があります。 それ以外の場合は、コンパイル時のエラーが発生します。
- 変換の結果は `F`型の関数ポインターになります。

`E`に `M` 静的メソッドが1つしかない場合は、ターゲットがメソッドグループ `void*` `E` であるアドレス式から暗黙的な変換が存在します。
1つの静的メソッドがある場合は、`E` の1つの最適なメソッドが `M`ます。
それ以外の場合は、コンパイル時のエラーが発生します。

これは、開発者がアドレス演算子と組み合わせて動作するために、オーバーロードの解決規則に依存することを意味します。

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

アドレス演算子は `ldftn` 命令を使用して実装されます。

この機能の制限:

- `static`としてマークされたメソッドにのみ適用されます。
- `static` 以外のローカル関数は、`&`では使用できません。 これらのメソッドの実装の詳細は、言語によって意図的に指定されていません。 これには、静的であるかインスタンスであるか、またはそれらが出力されるシグネチャが含まれます。

### <a name="better-function-member"></a>より優れた関数メンバー

より適切な関数メンバーの指定は、次の行を含むように変更されます。

> `delegate*` の方がより具体的 `void*`

つまり、`void*` と `delegate*` でオーバーロードしても、sensibly 演算子を使用することができます。

## <a name="open-issues"></a>懸案事項を開く

### <a name="nativecallableattribute"></a>NativeCallableAttribute

これは、を呼び出すときにマネージドからネイティブへのプロローグを回避するために CLR によって使用される属性です。 この属性でマークされたメソッドは、ネイティブコードからのみ呼び出すことができます。マネージ (メソッドを呼び出したり、デリゲートを作成したりすることはできません) などです。 この属性は mscorlib に特別ではありません。ランタイムは、この名前を持つすべての属性を同じセマンティクスで扱います。

ランタイムと言語が連携して、これを完全にサポートすることができます。 この言語では、指定された呼び出し規約を持つ `delegate*` として、`NativeCallable` 属性を持つアドレス `static` メンバーを扱うことができます。

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

また、言語も次のようにします。

- `NativeCallable` でタグ付けされたメソッドに対するマネージ呼び出しに対して、エラーとしてフラグを付けます。 関数をマネージコードから呼び出すことができない場合、コンパイラは、開発者がこのような呼び出しを試行できないようにする必要があります。
- メソッドが `NativeCallable`にタグ付けされている場合に、メソッドグループの変換を `delegate` できないようにします。

ただし、`NativeCallable` をサポートするためには必要ありません。 コンパイラは、既存の構文を使用するのと同様に、`NativeCallable` 属性をサポートできます。 このプログラムは、正しい `delegate*` シグネチャにキャストする前に、`void*` にキャストする必要があります。 これは現在のサポートよりも悪くはありません。

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a>アンマネージ呼び出し規約の拡張可能なセット

現在の ECMA-335 エンコーディングでサポートされているアンマネージ呼び出し規約のセットは古くなっています。 アンマネージ呼び出し規約のサポートを追加するように要求されました。次に例を示します。

- [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120
- StdCall と明示的にこの https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750

この機能を設計するには、今後、必要に応じてアンマネージ呼び出し規約のセットを拡張できるようにする必要があります。 この問題には、エンコード呼び出し規則のための制限された領域 (16 個の値から `IMAGE_CEE_CS_CALLCONV_MASK`) と、新しい呼び出し規則を追加するために触れる必要がある場所の数が含まれます。 解決策として、 [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention)列挙型を使用して呼び出し規約を表す新しいエンコーディングを導入することが考えられます。

参照用に、 https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h には、LLVM でサポートされている呼び出し規約の一覧があります。 .NET では、すべてをサポートする必要があるとは考えられませんが、呼び出し規約の領域が非常に豊富であることを示しています。

## <a name="considerations"></a>考慮事項

### <a name="allow-instance-methods"></a>インスタンスメソッドを許可する

提案は、`EXPLICITTHIS` CLI の呼び出し規約 (コードでC#は `instance` と呼ばれます) を利用して、インスタンスメソッドをサポートするように拡張できます。 この形式の CLI 関数ポインターは、関数ポインター構文の明示的な最初のパラメーターとして `this` パラメーターを格納します。

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

これはサウンドですが、提案にはいくつかの複雑なものが追加されます。 特に、呼び出し規約 `instance` と `managed` が異なる関数ポインターは、同じC#シグネチャを持つマネージメソッドを呼び出すために使用される場合でも、互換性がないことになります。 また、どのような場合でも、`static` のローカル関数を使用することによって、これがどのような場合に役立ちます。

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a>宣言で unsafe を必要としない

`delegate*`を使用するたびに `unsafe` を要求するのではなく、メソッドグループが `delegate*`に変換される時点でのみ必要になります。 ここで、主要な安全性の問題が発生します (値が生きている間は、含んでいるアセンブリをアンロードできないことがわかっています)。 他の場所での `unsafe` の要求は、過剰に表示されることがあります。

これは、当初の設計の意図です。 しかし、結果として得られる言語規則は非常に厄介です。 これはポインター値であるという事実を隠すことはできず、`unsafe` キーワードを使用しなくてもピークを維持することはできません。 たとえば、`object` への変換は許可できません。 `class`のメンバーにすることはできません...このC#設計では、すべてのポインターでを使用するために `unsafe` が必要になります。したがって、この設計はそれに従っています。

開発者は、現在の通常のポインター型と同じように、`delegate*` の値の上に_安全_なラッパーを提示することもできます。 検討事項:

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a>デリゲートの使用

新しい構文要素 `delegate*`使用する代わりに、型に続く `*` で既存の `delegate` 型を使用するだけです。

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

呼び出し規約の処理は、`CallingConvention` 値を指定する属性を使用して `delegate` 型に注釈を付けることによって行うことができます。 属性がない場合は、マネージ呼び出し規約を示します。

IL でこれをエンコードすると、問題が発生します。 基になる値はポインターとして表す必要がありますが、次のことも必要です。

1. 異なる関数ポインター型のオーバーロードに対して許可する一意の型を持つ。
1. アセンブリの境界を越えて、OHI を使用する場合と同じです。

最後の点は特に問題になります。 これは、`Func<int>*` がアセンブリ内で定義されていても制御できない場合でも、`Func<int>*` を使用するすべてのアセンブリが同等の型をメタデータでエンコードする必要があることを意味します。
また、mscorlib ではないアセンブリ内の `System.Func<T>` 名前で定義されているその他の型は、mscorlib で定義されているバージョンとは異なる必要があります。

探索された1つのオプションは、このようなポインターを `mod_req(Func<int>) void*`として生成していました。 これは、`mod_req` が `TypeSpec` にバインドできず、ジェネリックのインスタンス化をターゲットにできないため、機能しません。

### <a name="named-function-pointers"></a>名前付き関数ポインター

関数ポインターの構文は、特に、入れ子になった関数ポインターのような複雑なケースでは、煩雑になることがあります。 開発者は、`delegate`で実行されるように、言語で関数ポインターの名前付き宣言を使用できるようになるたびに署名を入力するのではなく、

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

ここでの問題の一部は、基になる CLI プリミティブには名前がないためC# 、これは純粋に発明であり、有効にするために多少のメタデータ作業が必要になります。 これは取り上げですが、作業の重要な部分です。 基本的に、 C#これらの名前の型 def テーブルに対するコンパニオンが必要です。

また、名前付き関数ポインターの引数を調べると、他の多くのシナリオにも同様に適用される可能性があります。 たとえば、名前付きタプルを宣言するだけで、すべての場合に完全な署名を入力する必要性を減らすことができます。

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

説明した後、`delegate*` 型の名前付き宣言を許可しないことにしました。 お客様の利用状況に関するフィードバックに基づいて、このことに大きなニーズがあることがわかった場合は、関数ポインター、タプル、ジェネリックなどに対して機能する名前付けソリューションを調査します。これは、言語でのフル `typedef` サポートなどの他の提案と同様に似ている可能性があります。

## <a name="future-considerations"></a>将来の注意事項

### <a name="static-local-functions"></a>静的ローカル関数

これは、ローカル関数で `static` 修飾子を許可する[提案](https://github.com/dotnet/csharplang/issues/1565)を指します。 このような関数は、ソースコードで正確なシグネチャを指定した `static` として出力されることが保証されます。 このような関数は、ローカル関数の現在の問題を含んでいないため、`&` の有効な引数である必要があります。

### <a name="static-delegates"></a>静的デリゲート

これは、`static` のメンバーのみを参照できる `delegate` 型の宣言を許可する[提案](https://github.com/dotnet/csharplang/issues/302)を指します。 このような `delegate` インスタンスの利点は、パフォーマンスを重視するシナリオでは、自由に割り当てできます。

関数ポインター機能が実装されている場合、`static delegate` の提案は終了する可能性があります。この機能の利点として、割り当ての自由な特性が挙げられます。 ただし、最近の調査では、アセンブリのアンロードによって実現できないことがわかりました。 アセンブリがアンロードされないようにするには、`static delegate` から参照するメソッドまでの厳密なハンドルが必要です。

すべての `static delegate` インスタンスを維持するには、提案の目標に対してカウンターを実行する新しいハンドルを割り当てる必要があります。 いくつかの設計では、割り当てを1つの呼び出しサイトに1回の割り当てに償却できましたが、それは少し複雑で、トレードオフではないと思われました。

つまり、開発者は、基本的に次のトレードオフを決定する必要があります。

1. アセンブリのアンロード時の安全性: これには割り当てが必要であるため、`delegate` は既に十分なオプションです。
1. アセンブリのアンロードに関して安全ではありません: `delegate*`を使用します。 これは、コードの残りの部分で `unsafe` コンテキスト以外の使用を許可するために `struct` にラップできます。
