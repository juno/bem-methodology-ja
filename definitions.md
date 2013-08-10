# BEMとは何か？
`BEM` とはブロック（Block）、エレメント（Element）、モディファイア（Modifier）の略語である。
その意味についてはこの記事で明らかにしていこう。

プログラミングにおける方法論の最も一般的なものはオブジェクト指向プログラミング（Object-Oriented Programming）だろう。
その枠組みは多くの言語で具体化されている。
BEMはいくつかの点でオブジェクト指向プログラミングと似ている。
それは現実のものをコードとして表現する方法であり、一連のパターンであり、また使用しているプログラミング言語とは関係なくプログラムの本質について考える方法である。

私たちはBEMの原則をフロントエンド開発のテクニックとツールをつくり上げるために用いたことで、Webサイトを迅速に開発し、長期間に渡ってメンテナンスし続けることができるようになった。

## 統一されたデータドメイン（Unified Data Domain）
次の図のような一般的なWebサイトを思い浮かべてみよう。

<div style="text-align:center">
<img src="http://bem.info/method/definitions/images/site.png"/>
</div>

このようなWebサイトの開発中には、サイトを構成するものを「ブロック」として区別しておくと役立つ。

例えばこの図では、`ヘッダ`、`メインレイアウト`、それに`フッター`ブロックがある。
`ヘッダ`は`ロゴ`、`検索`、`認証ブロック`に`メニュー`で構成されている。
`メインレイアウト`は、`ページタイトル`や`テキストブロック`を含んでいる。

<div style="text-align:center">
<img src="http://bem.info/method/definitions/images/site-marked.png"/>
</div>

ページの各部分に名前を付けるのは、チームでのコミュニケーションにとても役立つ。

プロジェクトマネージャーはこんな風に質問できる:

* `ヘッダー`を大きくしてくれ
* `ヘッダ`の中に`検索`フォームを含まないページを作ってほしい

HTML開発者は仲間のJavaScript開発者にこんな伝え方ができる:

* `認証ブロック`をアニメーションさせてくれ

それでは、BEMを構成する要素について詳しく見ていこう:

### ブロック（Block）
`ブロック`は独立した存在で、アプリケーションの「構成要素」である。
ブロックには単体のものも複合物（他のブロックを含むもの）もある。

**例**

検索フォームブロック

<div style="text-align:center">
<img src="http://bem.info/method/definitions/images/search-block.png"/>
</div>

### エレメント（Element）
`エレメント`は、ブロックの一部分であり特定の働きを持つ。
エレメントは文脈依存であり、そのエレメントが属するブロック内でのみ意味をなす。

**例**

入力フィールドとボタンは検索ブロックのエレメント

<div style="text-align:center">
<img src="http://bem.info/method/definitions/images/search-block-marked.en.png"/>
</div>

## ページとテンプレートを表現する手段
ブロックとエレメントはページコンテンツの構成要素となる。
単にページ上に存在するということ加えて、それらの配置も重要である。

ブロック（またはエレメント）は、決まった順序でお互いに並ぶことができる。

例えば、コマースサイトの商品リスト:

<div style="text-align:center">
<img src="http://bem.info/method/definitions/images/goods-list.png"/>
</div>

…もしくはメニューアイテム:

<div style="text-align:center">
<img src="http://bem.info/method/definitions/images/menu-items.png"/>
</div>

ブロックはその内部に他のブロックを含むこともできる。

例えば、他のブロックを含む`ヘッダーブロック`:

<div style="text-align:center">
<img src="http://bem.info/method/definitions/images/head-marked.png"/>
</div>

これらの構成要素がある一方で、プレーンテキストでページレイアウトを記述することも必要である。
そのためには、すべてのブロックとエレメントはそれを識別するためのキーワードを持つ必要がある。

特定のブロックを指すキーワードは`block name`となる。

例えば、`menu`は`メニューブロック`のキーワードとなり、`head`は`ヘッダー`ブロックのキーワードとなる。

特定のエレメントを指すキーワードは`element name`となる。

例えば、メニューの各アイテムは`menu`ブロックの`item`エレメントである。

ブロック名（`block name`）は、明確にどのブロックを指すかを表現できなければならないため、プロジェクト内で一意である必要がある。
同じブロックの唯一のインスタンスは同じ名前を持つことができる（訳注：シングルトン的な認識をするブロックの場合、ということと思われる）。

その場合、ある1つのブロックがページに2回（3回、4回、…）と表れる、と表現することになる。

エレメント名はそのブロックのスコープ内で一意である必要がある。
エレメントは何度も繰り返すことができる。

例えばメニューアイテムの場合:

<div style="text-align:center">
<img src="http://bem.info/method/definitions/images/menu-items.png"/>
</div>

キーワードは特定の順序で表れる必要がある。

ネストをサポートしたデータフォーマットではこうなる:

```xml
<b:page>
  <b:head>
    <b:menu>
      …
    </b:menu>
    <e:column>
      <b:logo/>
    </e:column>
    <e:column>
      <b:search>
        <e:input/>
        <e:button>Search</e:button>
      </b:search>
    </e:column>
    <e:column>
      <b:auth>
        …
      </b:auth>
    <e:column>
  </b:head>
</b:page>
```

このXMLの例では、`b`と`e`の名前空間がエレメントノードとブロックノードを分離している。

同じものをJSONで表現した場合:

```json
{
  block: 'page',
  content: {
    block: 'head',
    content: [
      { block: 'menu', content: … },
      {
        elem: 'column',
        content: { block: 'logo' }
      },
      {
        elem: 'column',
        content: [
          {
            block: 'search',
            content: [
              { elem: 'input' },
              { elem: 'button', content: 'Search' }
            ]
          }
        ]
      },
      {
        elem: 'column',
        content: {
          block: 'auth', content: …
        }
      }
    ]
  }
}
```

上記の例はブロックとエレメントが互いにネストしたオブジェクトモデルを表現している。
この構造では、任意の数のカスタムデータフィールドを含めることができる。

私たちはこの構造を`BEMツリー`と読んでいる（DOMツリーから）。

テンプレートの変換（XSLやJavaScriptを使うなどした）をして生成された、ブラウザに渡されるマークアップはBEMツリーとなる。

ブロックをページ上の別の位置に移動させる必要がある場合、開発者はBEMツリーを変更することで行う。最終的な外観は、テンプレート自身が生成する。

BEMツリーを表現するフォーマットやテンプレートエンジンには、任意のものを使うことができる。

私たちはページを表現するフォーマットにJSONを用いており、BEMHTMLというJavaScriptベースのテンプレートエンジンによってHTMLに変換している。

## ブロックの独立
プロジェクトの成長にともなって、ブロックは追加され、削除され、ページの別な位置に移動されたりしていくものである。
例えば`ロゴ`と`認証ブロック`を入れ替えたり、`メニュー`を`検索ブロック`の下に置いたり、といったことだ。

<div style="text-align: center">
<img src="http://bem.info/method/definitions/images/head.png"/>
</div>

<div style="text-align: center">
<img src="http://bem.info/method/definitions/images/head-changed.png"/>
</div>

この作業を容易にするには、ブロックが`独立`している必要がある。

`独立`したブロックは、自由な配置（ページのどこでも、他のブロックを含んだり含まれたり）ができるような方法で実装されている。

### 独立したCSS
これは、CSSの観点では以下のことを意味する。

* ブロック（やエレメント）は、CSSのルール内で使用可能なユニークな名前（CSSクラス）を持つ
* CSSセレクタにHTML要素などの、本質的に文脈フリーではないセレクタは含まない（`.menu td`のような）
* カスケーティングセレクタは避けるべき

##### 独立したCSSクラスのためのネーミング
適切なCSSクラス名のネーミングルールのひとつがこれだ:

* ブロックのCSSクラスは、その`block name`と同じ

```html
<ul class="menu">
  …
</ul>
```

* エレメントのCSSクラスは、任意のセパレーターで区切られた`block name`と`element name`

```html
<ul class="menu">
  <li class="menu__item">…</li>
  <li class="menu__item">…</li>
</ul>
```

カスケーディングを最小にするためには、エレメントのCSSクラスにブロック名を含めるのが重要である。

ツールやヘルパー（後述）が機械的にエレメントにアクセスできるようにするためには、セパレーターを一貫したものにしておくのも重要である。

私たちは、長い名前のセパレーターにハイフンを使い（`block-name`など）、ブロックとエレメントの区切りには２つのアンダースコアを使っている（`block-name__element-name`）。

好きなセパレーターを使うこともできる。

例:
* `block-name--element-name`
* `blockName-elementName`

### 独立したテンプレート
テンプレートエンジンの観点からは、ブロックの独立は以下のことを意味する:

* ブロックとエレメントは入力データを表現しなければならない
   * ブロック（またはエレメント）は、「`メニュー`はここに配置すべき」といったことをテンプレート内で表現できるようにするため、ユニークな「名前」を持たなくてはならない
* ブロックはBEMツリーのどこにでも出現できる

#### ブロックのための独立したテンプレート
テンプレート内にブロックがあらわれた場合、テンプレートエンジンはそれを一義的にHTMLに変換することができるはずである。
したがって、すべてのブロックはそのためのテンプレートを持つべきである。

例えば、XSLでのテンプレートはこのようになる:

```html
<xsl:template match="b:menu">
  <ul class="menu">
    <xsl:apply-templates/>
  </ul>
</xsl:template>

<xsl:template match="b:menu/e:item">
  <li class="menu__item">
    <xsl:apply-templates/>
  </li>
<xsl:template>
```

私たちは徐々にプロダクトからXSLTを捨て去っており、自分たちで開発したJavaScriptベースのテンプレートエンジン [XJST](https://github.com/veged/xjst) に移行しつつある。
このテンプレートエンジンは、私たちが考えるXSLTの好きなところを持ち合わせつつ（私たちは宣言型プログラミングのファンだ）、クライアントとサーバーサイドの両方での生産性向上のためにJavaScriptで実装されている。

私たちはテンプレートを、XJSTベースのBEMHTMLと呼ばれるドメイン固有言語（DSL）で記述している。
[BEMHTMLの概要](http://bem.info/articles/bemhtml-reference/)

[BEMHTML](https://github.com/bem/bem-bl/tree/master/blocks-common/i-bem/__html)

## ブロックの反復
2つ目の`メニューブロック`は`フッターブロック`内に配置することもできる。
もしくは、`テキストブロック`は広告によって分割された2つの箇所に配置することもできる。

あるブロックが単一のものとして開発されていたとしても、同じものをあらゆるタイミングでページに配置することができる。

CSS的に表現するとこうなる:
* IDベースのCSSクラスを使ってはならない
   * 「一意なものにはしない」という我々の要求を満たすため、クラスセレクタのみを使う。

JavaScript的にはこういうことだ:

* 同様の振る舞いを持つブロックは同様に検出される: それらは同じCSSクラスを持つ
   * CSSクラスセレクタを使うことで、動的な振る舞いを必要とする同じ名前のすべてのブロックを抽出することができる

## ブロックのためのモディファイア
既存のものと似ているが、見栄えや振る舞いが少しだけ違うブロックを作りたくなることはよくある。

こんなタスクがあるとしよう:
* `フッター`に別な`メニュー`を**異なるレイアウト**で追加する。

<div style="text-align: center">
<img src="http://bem.info/method/definitions/images/site-footer-menu.png"/>
</div>

既存のものとわずかしか違わないブロックを作るのではなく、`モディファイア`を使うことができる。

`モディファイア`は、見栄えや振る舞いが少し違うブロックやエレメントのプロパティである。

モディファイアは名前と値を持つ。複数のモディファイアは同時に使用できる。

**例**

背景色を指定するブロックモディファイア

<div style="text-align: center">
<img src="http://bem.info/method/definitions/images/search-background.png"/>
</div>

**例**

「現在」のアイテムの見た目を変えるエレメントモディファイア

<div style="text-align: center">
<img src="http://bem.info/method/definitions/images/menu-current-item.png"/>
</div>

### 入力データの観点から
BEMツリーにおいてモディファイアは、ブロックやエレメントを表現するエンティティのプロパティである。

例えば、XMLの場合は属性ノードとして表現できる:

```xml
<b:menu m:size="big" m:type="buttons">
  …
</b:menu>
```

同じものがJSONではこうなる:

```json
{
  block: 'menu',
  mods: [
   { size: 'big' },
   { type: 'buttons' }
  ]
}
```

### HTML/CSSの観点から
モディファイアは、ブロックやエレメントに追加するCSSクラスである。

```html
<ul class="menu menu_size_big menu_type_buttons">
  …
</ul>
```

```css
.menu_size_big {
  // 高さを指定するCSSコード
}
.menu_type_buttons .menu__item {
  // アイテムの見栄えを変えるCSSコード
}
```

## エレメントモディファイア
エレメントのモディファイアも同じように実装される。

かさねて言うが、CSSを手書きする場合には、機械的にアクセスできるように一貫したセパレーターを使うことがとても重要だ。

例えば、現在のメニューアイテムは以下のモディファイアでマークアップできる:

```xml
<b:menu>
  <e:item>Index<e:item>
  <e:item m:state="current">Products</e:item>
  <e:item>Contact<e:item>
</b:menu>
```

```json
{
  block: 'menu',
  content: [
    { elem: 'item', content: 'Index' },
    {
      elem: 'item',
      mods: { 'state' : 'current' },
      content: 'Products'
    },
    { elem: 'item', content: 'Contact' }
  ]
}
```

```css
.menu__item_state_current
{
  font-weight: bold;
}
```

これらは以下のようなHTMLとして表現される:

```html
<ul class="menu">
  <li class="menu__item">Index</li>
  <li class="menu__item menu__item_state_current">Products</li>
  <li class="menu__item">Contact</li>
</ul>
```

メニュークラスを、そのレイアウト実装の詳細から独立させることもできる:

```html
<div class="menu">
  <ul class="menu__layout">
    <li class="menu__layout-unit">
      <div class="menu__item">Index</div>
    </li>
    <li class="menu__layout-unit">
      <div class="menu__item menu__item_state_current">Products</div>
    </li>
    <li class="menu__layout-unit">
      <div class="menu__item">Contact</div>
    </li>
  </ul>
</div>
```

```html
<div class="menu">
  <table class="menu__layout">
  <tr>
    <td class="menu__layout-unit">
      <div class="menu__item">Index</div>
    </td>
    <td class="menu__layout-unit">
      <div class="menu__item menu__item_state_current">Products</div>
    </td>
    <td class="menu__layout-unit">
      <div class="menu__item">Contact</div>
    </td>
  </tr>
  </table>
</div>
```

## 主題の抽象化
多くの人びとがプロジェクトで作業する場合、メンバーはデータドメインに関して合意を持ち、ブロックやエレメントの命名にはそのドメインを使うべきである。

例えば、`タグクラウド`ブロックは常に`tags`という名前になり、そのエレメントは`tag`となる、というようなことだ。この決まりはCSS、JavaScript、XSLなどすべての言語に対して同じだ。

開発プロセスの観点から:
* すべてのメンバーは同じ用語を扱うようになる

CSSの観点から:
* ブロックとエレメントに対するCSSを、命名規約に準じたCSSにコンパイルされる擬似言語で記述することができる

```css
  .menu {
    __layout {
      display: inline;
    }
    __layout-item {
      display: inline-block;
      …
    }
    __item {
      _state_current {
        font-weight: bold;
      }
    }
  }
``

JavaScriptの観点から:
* クラスセレクタを使ってDOM要素を直接探す代わりに、特別なヘルパーライブラリを使うことができる:

```javascript
$('menu__item').click( … );
$('menu__item').addClass('menu__item_state_current');
$('menu').toggle('menu_size_big').toggle('menu_size_small');
```

ブロックとエレメントのCSSクラスの命名規約は時とともに変わるものである。
ブロックとエレメントにアクセスするのに特別なJavaScript関数を使い、その処理にはモディファイアを使うことで、命名規約が変わった場合でもそれらの関数を変更するだけでよくなる。

```javascript
block('menu').elem('item').click( … );
block('menu').elem('item').setMod('state', 'current');
block('menu').toggleMod('size', 'big', 'small');
```

上記のコードは抽象的なものである。
私たちは実際には、`bem-bl`ブロックライブラリに含まれる`i-bem`ブロックのJavaScriptコアを使っている。
 [http://bem.github.com/bem-bl/sets/common-desktop/i-bem/i-bem.en.html](http://bem.github.com/bem-bl/sets/common-desktop/i-bem/i-bem.en.html)

## ブロックの一貫性
サイトには、ある動的な振る舞いを持った`ボタン`ブロックがある。

<div style="text-align: center">
<img src="http://bem.info/method/definitions/images/button.png"/>
</div>

ブロックにカーソルを重ねた時、その見栄えが変わる。

<div style="text-align: center">
<img src="http://bem.info/method/definitions/images/button-cursor.png"/>
</div>

マネージャーは、同じボタンを他のページでも使えるかどうか聞きたくなる。

ブロックのCSS実装だけでは不十分だ。ブロックの再利用とは、JavaScriptによる振る舞いも再利用できるということを意味する。

ブロックは自身に関するすべてを「知って」いなければならない。ブロックを実装するために、使用しているすべての技術を用いて見栄えと振る舞いを表現する。私たちはそれを`多言語主義（multilingualism）`と呼ぶ。

`多言語（Multilingual）`プレゼンテーションとは、ブロックの見た目と機能を実装するのに必要となるすべてのプログラミング言語でブロックを表現したものである。

ブロックをUI要素としてページに配置するには、以下のような技術でブロックを実装する必要がある:
* ブロックの宣言をHTMLコードに変換するテンプレート（XSL、TT2、JavaScriptなど）
* ブロックの見栄えを記述したCSS
* ブロックが動的な振る舞いを持つ場合は、ブロックのJavaScript実装
* 画像
* ドキュメント

ブロックを構成するすべてのものが、ブロック技術である。

## 実際の例
[Yandex](http://company.yandex.ru) はサービスの開発にBEM方法論を採用している大きな（ほとんどがロシア人）企業である。

BEM方法論は、何か固有のフレームワークを使うよう求めることはない。
あなたは、ページで使っているすべての技術についてBEMを適用する必要もない（しかし、そうしたほうが効率的ではある）。

[Yandexのすべてのサービス](http://www.yandex.ru/all)は、CSSとJavaScriptコード、それにページのXSLテンプレートでBEMを用いている。
例:
 * [Yandex.Maps](http://maps.yandex.ru/?text=%D0%A0%D0%BE%D1%81%D1%81%D0%B8%D1%8F%2C%20%D0%9C%D0%BE%D1%81%D0%BA%D0%B2%D0%B0&sll=37.609218%2C55.753559&ll=37.609218%2C55.753563&spn=2.570801%2C0.884460&z=9&l=map)
 * [Yandex.Images](http://images.yandex.ru/yandsearch?text=Yandex+office&rpt=image)
   ネット上の画像を検索するサービス。
 * [Yandex.Video](http://video.yandex.ru/#search?text=yac%202011)
   動画をホスティングし検索するサービス。
 * [Yandex.Auto](http://auto.yandex.ru/)
 * [Turkish Yandex](http://www.yandex.com.tr/)

いくつかのサービスではXSLテンプレートを使わず、（前述の）私たちの新しいテンプレートエンジンである `bemhtml` を使っている。
そのサービスは以下のとおり:
 * [Yandex Search](http://yandex.ru/yandsearch?text=BEM+methodology+front-end&lr=213)
   または [Yandex Search in English](http://yandex.com/yandsearch?text=%22What+is+BEM%3F%22+front-end&lr=213)
 * [Mobile Apps Search](http://apps.yandex.ru/)
   スマートフォン向け。

他の企業でもBEM方法論が用いられている。

例えば [Mail.ru](http://mail.ru) の人たちは部分的にBEMを適用している。彼らのページのいくつかのブロックはCSSがBEMベースとなっている。また、彼らは独自のC++テンプレートエンジンも持っており、ブロックテンプレートを方法論にしたがって記述している。

その他の例:
 * [Rambler.News](http://beta.news.rambler.ru/)
 * [HeadHunter](http://hh.ru/)
 * [TNK Racing Team](http://futurecolors.ru/tnkracing/)

[bem-bl](http://bem.github.com/bem-bl/index.ru.html) ブロックライブラリを使っているサイトに興味があれば:
 * [BEM based web form with JZ validation](http://form.dev.eot.su/)
 * [Mikhail Troshev vCard](http://mishanga.pro/)
   ソースコードはGitHubに公開されている: [https://github.com/mishanga/bem-vcard](https://github.com/mishanga/bem-vcard)
