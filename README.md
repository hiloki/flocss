**FLOCSS（フロックス）** は、[OOCSS](https://github.com/stubbornella/oocss/wiki)や[SMACSS](https://smacss.com/ja)、[BEM](http://bem.info/)、[SuitCSS](http://suitcss.github.io/)のコンセプトを取り入れた、モジュラーなアプローチのためのCSS構成案です。  
[MCSS](http://operatino.github.io/MCSS/ja/)のレイヤー構成にも大きな影響を受けています。

## 基本原則

FLOCSSは次の3つのレイヤーと、**Object**レイヤーの子レイヤーで構成されます。

1. Foundation - reset/normalize/base...
2. Layout - header/main/sidebar/footer...
3. Object
  1. Component - grid/button/form/media...
  2. Project - articles/ranking/promo...
  3. Utility - clearfix/display/margin...

他のOOCSSに基づくフレームワークおよびスタイルガイドと同様に、
再利用性や拡張性を持たせるために、これらのルールで分類をします。

### Foundation

Reset.cssやNormalize.cssなどを用いたブラウザのデフォルトスタイルの初期化や、プロジェクトにおける基本的なスタイルを定義します。
ページの下地としての全体の背景や、基本的なタイポグラフィなどが該当します。

### Layout

ページを構成するヘッダーやメインのコンテンツエリア、サイドバーやフッターといったプロジェクト共通のコンテナーブロックのスタイルを定義します。

基本的には、ページ単位で唯一の存在である要素となるため、Layoutレイヤーの要素ではIDセレクタを使うことを推奨します。
IDセレクタはセレクタの詳細度を高めてしまうため、他のレイヤーやモジュールでは推奨しません。

*Note:*  
SMACSSにおけるLayoutでは、グリッドレイアウトのためのモジュールも含めます。
FLOCSSでは、グリッドフレームワークとしての定義、具体的にはCSSプリプロセッサによるmixinやfunctionがある場合は、
各所でincludeされることを考慮して、このFoundationレイヤーで扱うべきだと考えています。
例えばそれらは、[Susy](http://susy.oddbird.net/)、[Bourbon Neat](http://neat.bourbon.io/)、[Kite](https://github.com/hiloki/kitecss)のようなレイアウトに関わるフレームワークです。

しかし、これらをクラスとして扱う場合には、それらは Object/Componentレイヤーとして定義させるほうが、多くのレイアウト、グリッドフレームワークをFLOCSSに適用させやすいと考えています。
（!運用の中でこのルールが変わる可能性は大きいです！）

```css
// Foundation
@mixin span-columns($coloums) {
  ...
}

// Layout
#header {
  @include span-columns(12)
}

// Object/Component

.c-grid {
  @include outer-container;
}

.c-grid__cell {
  @include pad();
}

.c-grid__cell--1 {
  @include span-columns(1)
}
```

### Object

OOCSSのコンセプトを元に、プロジェクトにおける繰り返されるビジュアルパターンをすべて**Object**と定義します。

FLOCSSでのObjectは、さらに次の3つのレイヤーに分けられます。

#### 1. Component

再利用できるパターンとして、小さな単位のモジュールを定義します。

一般的によく使われるパターンであり、例えば[BootstrapのComponentカテゴリ](http://getbootstrap.com/components/)などに見られる`button`などが該当します。

出来る限り、最低限の機能を持ったものとして定義されるべきであり、それ自体が固有の幅や色などの特色を持つことは避けるのが望ましいです。

#### 2. Project

プロジェクト固有のパターンであり、いくつかのComponentと、それに該当しない要素によって構成されるものを定義します。

例えば、記事一覧や、ユーザープロフィール、画像ギャラリーなどコンテンツを構成する要素などが該当します。

#### 3. Utility

ComponentとProjectレイヤーのObjectのモディファイアで解決することが難しい・適切では無い、わずかなスタイルの調整のための便利クラスなどを定義します。

Utilityは、Component、ProjectレイヤーのObjectを無尽蔵に増やしてしまうことを防いだり、またこれらのObject自体が持つべきではない`margin`の代わりに`.mbs { margin-bottom: 10px; }`のようなUtility Objectを用いて、隣接するモジュールとの間隔をつくるといった役割を担います。

またclearfixテクニックのためのルールセットが定義されているヘルパークラスも、このレイヤーに含めます。

## 命名規則

### MindBEMding

[BEM](http://bem.info/)システムのシンタックスである、**Block**、**Element**、**Modifier**に分類して構成される規則を採用します。

FLOCSSでは、オリジナルのBEMのシンタックスではなく、[MindBEMding](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)
のアイデアを基本的にそのまま取り入れています。

Modifierの命名の派生パターンとして、JavaScriptで操作されるような「状態」を表すようなModifierについては、SMACSSの**State**パターンの命名を拝借し、'is-*'プレフィックスを付与し、`.is-active`というようにすることもできます。

```html
<button class="c-button is-active">Save</button>
```

```css
.c-button { ... }
.c-button.is-active { ... }
```

このアイデアを採用する場合の原則として、`.is-active`そのものにルールを持たせるのは**禁止**します。これは`.is-active`そのものが持つルールが、
他のモジュールのModifierのスタイルを汚染してしまうのを防ぐためです。


### Objectのプレフィックス

Objectレイヤーの中で分類されるモジュールに対し、役割を明確にするためにプレフィックスをつけることを推奨します。

- Component - `.c-*`
- Project   - `.p-*`
- Utility   - `.u-*`

*Note:*  
これらの命名規則は、あなたのプロジェクトが持つオリジナルの命名規則に従い、キャメルケースなどを組み合わせたものでも構いませんが、必ず**命名に一貫性を保つ**ようにしてください。

## ファイル・ディレクトリ構成

基本原則のレイヤー構成に従い、下記のような構成を前提とします。

```css
/* ==========================================================================
   Foundation
   ========================================================================== */

/* Reset
   ----------------------------------------------------------------- */
 
html, body, div, … { … }
 
/* Base
   ----------------------------------------------------------------- */
 
body { … }


/* ==========================================================================
   Layout
   ========================================================================== */
 
/* Header
   ----------------------------------------------------------------- */
 
#header { … }
 
/* Main
   ----------------------------------------------------------------- */
 
#main { … }


/* ==========================================================================
   Object
   ========================================================================== */

/* Component
   ----------------------------------------------------------------- */
 
/**
 * Button
 *
 * Blah Blah Blah
 */
 
.c-btn { … }
.c-btn--primary {
  ...
}
 
/**
 * Media
 */

.c-media { … }
.c-media__image {
  ...
}

/* Project
   ----------------------------------------------------------------- */
 
/**
 * Articles
 */

.p-articles { … }
.p-article {
  ...
}
.p-article__title {
  ...
}

/* Utitlity
   ----------------------------------------------------------------- */
 
/**
 * Clearfix
 */

.u-clearfix {
  ...
}
 
/**
 * Display
 */

.u-block {
  ...
}
 
/**
 * Margin
 */

.u-mbs { /* Margin-Bottom: Small; */
  ...
}
```

もしSassやStylusのようなCSSプリプロセッサやビルドツールを使ってCSSファイルを結合できる環境にあれば、次のようにディレクトリを分割して管理することを推奨します。
次の例は、Sassを採用した場合の例です。

```
├── foundation
│   ├── _base.scss
│   └── _reset.scss
├── layout
│   ├── _footer.scss
│   ├── _header.scss
│   ├── _main.scss
│   └── _sidebar.scss
└── object
    ├── component
    │   ├── _button.scss
    │   ├── _dialog.scss
    │   ├── _grid.scss
    │   └── _media.scss
    ├── project
    │   ├── _articles.scss
    │   ├── _comments.scss
    │   ├── _gallery.scss
    │   └── _profile.scss
    └── utility
        ├── _align.scss
        ├── _clearfix.scss
        ├── _margin.scss
        ├── _position.scss
        ├── _size.scss
        └── _text.scss
```

モジュール単位でファイルを分割することによって、ページ単位またはプロジェクト単位でのモジュールの追加・削除の管理が容易になります。

これらを統括するための`app.scss`のようなファイルからは次のように参照します。

```scss
// ==========================================================================
// Foundation
// ==========================================================================

@import "foundation/_reset";
@import "foundation/_base";

// ==========================================================================
// Layout
// ==========================================================================

@import "layout/_footer";
@import "layout/_header";
@import "layout/_main";
@import "layout/_sidebar";

// ==========================================================================
// Object
// ==========================================================================

// -----------------------------------------------------------------
// Component
// -----------------------------------------------------------------

@import "object/component/_button";
@import "object/component/_dialog";
@import "object/component/_grid";
@import "object/component/_media";

// -----------------------------------------------------------------
// Project
// -----------------------------------------------------------------

@import "object/project/_articles";
@import "object/project/_comments";
@import "object/project/_gallery";
@import "object/project/_profile";

// -----------------------------------------------------------------
// Utility
// -----------------------------------------------------------------

@import "object/utility/_align";
@import "object/utility/_clearfix";
@import "object/utility/_margin";
@import "object/utility/_position";
@import "object/utility/_size";
@import "object/utility/_text";
```


## カスケーディングと詳細度

カスケーディングは、CSSのもっとも大きな特長のひとつです。運用の中でCSSが破綻していくのは、安易なカスケーディングとセレクタの管理に原因があることが多いでしょう。

FLOCSSでは、後ろのレイヤーになるほど具体的になり、カスケーディングにおいて強いルールである必要があります。そのため、**レイヤーの順序を前後させてはいけません。**

### レイヤー、モジュールのカスケーディング

原則として、モジュール間のカスケーディング、他のモジュールを親とするセレクタを用いたカスケーディングは**禁止**とします。

特に同一レイヤーにおけるモジュール間のカスケーディング、例えば、次のような複数のセレクタを用いたカスケーディングは好ましくありません。

```css
// Component
.c-button {
  ...
}
.c-dialog .c-button {
  ...
}

// Project
.p-comments {
  ...
}
.p-articles .p-comments {
  ...
}
```

なぜならば、そのレイヤーにおいて、特定のモジュールに依存することなく、モジュールとして独立して再利用できるべきであり、混在させることによって他の開発者が予想しない挙動になるべきではないためです。

例外として、レイヤー間におけるカスケーディング、例えば、次のような**ProjectレイヤーがComponentレイヤーのモジュールを変更することは許容**します。

```html
<div class="p-profile c-media">
  <img src="user.jpg" class="c-media__image">
  <div class="c-media__body">...</div>
</div>
```

```css
// Component
.c-media__image {
  float: left;
  margin-left: 10px;
}

.p-profile > .c-media__image {
  float: right;
  margin-left: 0; // Cancel '.c-media__image' value
  margin-right: 10px;
}
```

ただし、このような例であってもセレクタのカスケーディングをおこなう必要はなく、次のようにProjectレイヤーの**Element**や、Componentレイヤーの**Modifier**によって拡張することによって解決することができる場合があります。

#### ProjectのElement

```html
<div class="p-profile c-media">
  <img src="user.jpg" class="p-profile__avatar c-media__image">
  <div class="c-media__body">...</div>
</div>
```

```css
// Component
.c-media__image {
  float: left;
  margin-left: 10px;
}

.p-profile__avatar {
  float: right;
  margin-left: 0; // Cancel 'c-media__image' value
  margin-right: 10px;
}
```

#### ComponentのModifier

```html
<div class="p-profile c-media">
  <img src="user.jpg" class="p-profile__avatar c-media__image c-media__image--rev">
  <div class="c-media__body">...</div>
</div>
```

```css
// Component
.c-media__image {
  float: left;
  margin-left: 10px;
}

.c-media__image--rev { // reverse
  float: right;
  margin-left: 0; // Cancel 'c-media__image' value
  margin-right: 10px;
}
```

このように解決できた場合には、詳細度を強くすることを防ぐことができます。

一方で、対象のデザインが複雑で、パターンが多岐にわたるようなものであった場合、ElementやModifierでコードが溢れてしまうということもあります。  
それによってモジュール自体が煩雑になり、問題を引き起こすようであれば、最初にあげた例のようなProjectレイヤーからのComponentレイヤーのカスケーディングが例外として許容します。

その際には、次のようなルールを課すようにしてください。

- Projectレイヤーモジュール内に、同一のComponentレイヤーモジュールが複数点在する場合を考慮し、出来る限り子供セレクタなどを活用して、適用範囲を限定する。
- 他のモジュールを親セレクタに持つのは**1つ**だけとする。

### モジュール内のカスケーディング

他のレイヤーやモジュールを横断してのカスケーディングとは異なり、モジュール内で完結するカスケーディング、複数のセレクタを用いることは**許容**します。

例えば、次のような`Tabs`モジュールの場合です。

```html
<ul class="c-tabs">
  <li class="c-tab">
    <a>Tab A</a>
  </li>
  <li class="c-tab">
    <a>Tab B</a>
  </li>
  <li class="c-tab">
    <a>Tab C</a>
  </li>
</ul>
```

```css
.c-tabs {
  display: table;
  table-layout: fixed;
}
.c-tab {
  display: table-cell;
}
.c-tab > a { // Use Descendant selector
  display: block;
}
```

この場合も、より堅実な設計を考慮するならば、次にように書くのも良いでしょう。

```html
<ul class="c-tabs">
  <li class="c-tab">
    <a class="c-tab__target">Tab A</a>
  </li>
  <li class="c-tab">
    <a class="c-tab__target">Tab B</a>
  </li>
  <li class="c-tab">
    <a class="c-tab__target">Tab C</a>
  </li>
</ul>
```

```css
.c-tabs {
  display: table;
  table-layout: fixed;
}
.c-tab {
  display: table-cell;
}
.c-tab__target { // Use BEM "Element"
  display: block;
}
```

この例以外にも、リスト系モジュールにおいて隣接する要素間にボーダーが必要、といったケースでは隣接セレクタ'E + E {...}`を用いる場合もありますし、":hover"のような擬似クラスや状態を表すクラスをつける場合もあります。
これらはすべて許容範囲とします。


```html
<ul class="c-tabs">
  <li class="c-tab p-category-nav is-selected">
    <a>Tab A</a>
  </li>
  <li class="c-tab p-category-nav">
    <a>Tab B</a>
  </li>
  <li class="c-tab p-category-nav">
    <a>Tab C</a>
  </li>
</ul>
```

```css
.p-category-nav {
  background-image: url(/img/bg-category-nav.png);
}
.p-category-nav:hover {
  background-image: url(/img/bg-category-nav--highlight.png);
}
.p-category-nav:active,
.p-category-nav.is-selected {
  background-color: #FFFFFF;
  background-image: none;
}
```

## CSSプリプロセッサのExtend

CSSプリプロセッサの多くが持つ、セレクタを継承するためのExtendは、原則そのモジュールで完結する継承以外では利用を禁止します。

レイヤーやモジュールを超えてExtendによる継承をおこなった場合、FLOCSSの構成・設計は破綻し、カスケーディングルールも複雑にしてしまう可能性があるためです。

以下は例外として、許容されるパターンをあげます。

### モジュールで完結するExtend

次のようなbuttonモジュールがあったとします。

```scss
.button {
  display: inline-block;
  padding: 0.5em 1em;
  cursor: pointer;
}
.button--primary {
  background-color: #CCAA00;
}
```

```html
<a href="#save" class="button button--primary">Save</a>
```

FLOCSSの構想では、このようなマルチクラスパターンを基本としていますが、次のようなExtendによってシングルクラスにすることができます。

```scss
.button {
  display: inline-block;
  padding: 0.5em 1em;
  cursor: pointer;
}
.button--primary {
  @extend .button
  background-color: #CCAA00;
  color: #FFFFFF;
}
.button--secondary {
  @extend .button
  background-color: #FFCC00;
}

// Compiled
// .button,.button--primary {
//   display: inline-block;
//   padding: 0.5em 1em;
//   cursor: pointer;
// }
// .button--primary {
//   background-color: #CCAA00;
// }
```

```html
<a href="#save" class="button--primary">Save</a>
```

このようにモジュール内で完結をする限りは、管理が煩雑になりにくいため許容します。
