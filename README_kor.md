**FLOCSS（フロックス）** は、[OOCSS](https://github.com/stubbornella/oocss/wiki)や[SMACSS](https://smacss.com/ja)、[BEM](http://bem.info/)、[SuitCSS](http://suitcss.github.io/)のコンセプトを取り入れた、モジュラーなアプローチのためのCSS構成案です。  
[MCSS](http://operatino.github.io/MCSS/ja/)のレイヤー構成にも大きな影響を受けています。

**FLOCSS는, [OOCSS](https://github.com/stubbornella/oocss/wiki)와 SMACSS](https://smacss.com/ja)、[BEM](http://bem.info/)、[SuitCSS](http://suitcss.github.io/)의 콘셉을 넣어 모듈러 접근을 위한 CSS구성안입니다.
## 基本原則 기본 원칙
FLOCSSは次の3つのレイヤーと、**Object**レイヤーの子レイヤーで構成されます。
FLOCSS은 다음의 3가지 레이어와 **Object**레이어의 자식 레이어로 구성되어 있습니다.

1. Foundation - reset/normalize/base...
2. Layout - header/main/sidebar/footer...
3. Object
   1. Component - grid/button/form/media...
   2. Project - articles/ranking/promo...
   3. Utility - clearfix/display/margin...

他のOOCSSに基づくフレームワークおよびスタイルガイドと同様に、
再利用性や拡張性を持たせるために、これらのルールで分類をします。
다른 OOCSS에 기초를 둔 프레임 워크 혹은 스타일 가이드와 같은 사양으로 재사용성과 확장성을 가지고 있기 때문에 그에 따른 규칙으로 분류합니다.

### Foundation

Reset.cssやNormalize.cssなどを用いたブラウザのデフォルトスタイルの初期化や、プロジェクトにおける基本的なスタイルを定義します。
ページの下地としての全体の背景や、基本的なタイポグラフィなどが該当します。
Reset.css와 Normalize.css등에 사용된 브라우저의 디폴트 스타일의 초기화나 프로젝트 전반에 있어서 기본적인 스타일을 정의합니다.
페이지의 밑바탕이 되는 전체의 배경과 기본적인 타이포그래피 등이 해당됩니다.





### Layout

ページを構成するヘッダーやメインのコンテンツエリア、サイドバーやフッターといったプロジェクト共通のコンテナーブロックのスタイルを定義します。
페이지를 구성하는 헤더와 메인 콘텐츠 영역, 사이드바와 풋터로 이루어진 프로젝트의 공통 콘텐츠 블럭 스타일을 정의 합니다.

基本的には、ページ単位で唯一の存在である要素となるため、Layoutレイヤーの要素ではIDセレクタを採用することも可能です。  
ただしIDセレクタは高い詳細度を持つため、それを懸念する場合には、`l-*` プレフィックスをつけた命名を採用するか、あるいは `[id="header"]` のような属性セレクタを用いることを推奨します。
기본적으로는 페이지 단위로 유일하게 존재하는 요소이기 때문에 Layout 레이어의 요소에는 ID 셀렉터를 사용할수 있습니다.
단 ID 셀렉터는 높은 상세도(단 한번밖에 사용할수 없는)를 가지고 있기 때문에 그것이 걱정될 경우에는, `l-*`프리픽스를 넣어서 사용하든가 혹은 `[id="header"]`처럼 속성 셀렉터를 사용하는것을 추천합니다.

*Note:*  
SMACSSにおけるLayoutでは、グリッドレイアウトのためのモジュールも含めます。
SMACSS에 의한 Layout에는 그리드 레이아웃을 위한 모듈도 포함됩니다.

FLOCSSでは、グリッドフレームワークとしての定義、具体的にはCSSプリプロセッサによるmixinやfunctionがある場合は、
各所でincludeされることを考慮して、このFoundationレイヤーで扱うべきだと考えています。
例えばそれらは、[Susy](http://susy.oddbird.net/)、[Bourbon Neat](http://neat.bourbon.io/)、[Kite](https://github.com/hiloki/kitecss)のようなレイアウトに関わるフレームワークです。
FLOCSS에는 그리드 프레임 워크로 정의, 
구체적으로는 CSS 프리 프로세서와 비슷한 mixin이나 function이 있는 경우는, 
여러곳에 include 될것을 고려하고 그 Foundation 레이어로 다뤄야 한다고 생각하고 있습니다.
예를 들어 그것들은 [Susy](http://susy.oddbird.net/)、[Bourbon Neat](http://neat.bourbon.io/)、[Kite](https://github.com/hiloki/kitecss)과 같은 레이아웃에 관한 프레임 워크입니다,

SMACSS
しかし、これらをクラスとして扱う場合には、それらは Object/Componentレイヤーとして定義させるほうが、多くのレイアウト、グリッドフレームワークをFLOCSSに適用させやすいと考えています。
（!運用の中でこのルールが変わる可能性は大きいです！）
그러나, 그것들을 클래스로 다룰 경우에는 그것들을 Object/Component 래아어로 정의시키는 쪽이 많은 레이아웃, 그리드 프레임워크를 FLOCSS에 적용시키기 쉽다고 생각합니다.
(!운영중에 그 규칙이 변할 가능성이 큽니다!)

```css
// Foundation
@mixin span-columns($columns) {
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
OOCSS의 콘셉을 기본으로, 프로젝트 전반에 반복되는 비쥬얼 패턴을 모두 **Object**라고 정의합니다.

FLOCSSでのObjectは、さらに次の3つのレイヤーに分けられます。
FLOCSSで의 Object는, 게다가 다음의 세가지 레이어로 나눠집니다.

#### 1. Component

再利用できるパターンとして、小さな単位のモジュールを定義します。
재사용이 가능한 패턴으로 작은 단위의 모듈을 정의합니다.


一般的によく使われるパターンであり、例えば[BootstrapのComponentカテゴリ](http://getbootstrap.com/components/)などに見られる`button`などが該当します。
일반적으로 많이 사용되는 패턴이 있어 예를들어 [Bootstrap의 Component 카테고리](http://getbootstrap.com/components/) 등에 보여지는 `button`등이 해당됩니다. 
出来る限り、最低限の機能を持ったものとして定義されるべきであり、それ自体が固有の幅や色などの特色を持つことは避けるのが望ましいです。
가능한 최소의 기능으로 정의되어야 하고 그 자체가 고유의 폭과 색등의 특색을 지니는 것을 피하는것이 좋습니다.  

#### 2. Project

プロジェクト固有のパターンであり、いくつかのComponentと、それに該当しない要素によって構成されるものを定義します。
프로젝트 고유의 패턴이 있어 몇개의 Component와 그것에 해당하지 않는 요소에 따라 구성되어지는 것을 정의합니다.

例えば、記事一覧や、ユーザープロフィール、画像ギャラリーなどコンテンツを構成する要素などが該当します。
예를 들어, 기사 리스트나 사용자 프로필, 이미지 갤러리 등의 콘텐츠를 구성하는 요소등이 해당됩니다. 
#### 3. Utility

ComponentとProjectレイヤーのObjectのモディファイアで解決することが難しい・適切では無い、わずかなスタイルの調整のための便利クラスなどを定義します。

Utilityは、Component、ProjectレイヤーのObjectを無尽蔵に増やしてしまうことを防いだり、またこれらのObject自体が持つべきではない`margin`の代わりに`.mbs { margin-bottom: 10px; }`のようなUtility Objectを用いて、隣接するモジュールとの間隔をつくるといった役割を担います。

またclearfixテクニックのためのルールセットが定義されているヘルパークラスも、このレイヤーに含めます。

## 命名規則

### MindBEMding

[BEM](http://bem.info/)システムのシンタックスである、**Block**、**Element**、**Modifier**に分類して構成される規則を採用します。

FLOCSSでは、オリジナルのBEMのシンタックスではなく、[MindBEMding](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)
のアイデアを基本的にそのまま取り入れています。

Modifierの命名の派生パターンとして、JavaScriptで操作されるような「状態」を表すようなModifierについては、SMACSSの**State**パターンの命名を拝借し、`is-*`プレフィックスを付与し、`.is-active`というようにすることもできます。

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

/* Utility
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
