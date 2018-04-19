**FLOCSS는, [OOCSS](https://github.com/stubbornella/oocss/wiki)와 [SMACSS](https://smacss.com/)、[BEM](http://bem.info/)、[SuitCSS](http://suitcss.github.io/)의 콘셉을 도입한 모듈러 접근을 위한 CSS 구성안입니다.

[MCSS](http://operatino.github.io/MCSS/)의 레이어 구성에도 큰 영향을 받고 있습니다.

## 기본 원칙
FLOCSS은 다음의 3가지 레이어와 **Object**레이어의 자식 레이어로 구성되어 있습니다.

1. Foundation - reset/normalize/base...
2. Layout - header/main/sidebar/footer...
3. Object
   1. Component - grid/button/form/media...
   2. Project - articles/ranking/promo...
   3. Utility - clearfix/display/margin...


다른 OOCSS에 기초를 둔 프레임 워크 혹은 스타일 가이드와 같은 사양으로 재사용성과 확장성을 가지고 있기 때문에 그에 따른 규칙으로 분류합니다.

### Foundation

Reset.css와 Normalize.css등에 사용된 브라우저의 디폴트 스타일의 초기화나 프로젝트 전반에 있어서 기본적인 스타일을 정의합니다.

페이지의 밑바탕이 되는 전체의 배경이나 기본적인 타이포그래피 등이 해당됩니다.





### Layout

페이지를 구성하는 헤더와 메인 콘텐츠 영역, 사이드바와 풋터로 이루어진 프로젝트의 공통 콘텐츠 블럭 스타일을 정의 합니다.

기본적으로는 페이지 단위로 유일하게 존재하는 요소이기 때문에 Layout 레이어의 요소에는 ID 셀렉터를 사용할수 있습니다.
단 ID 셀렉터는 높은 상세도를 가지고 있기(단 한번밖에 사용할수 없기) 때문에 그것이 걱정될 경우에는, `l-*`프리픽스를 넣어서 사용하든가 혹은 `[id="header"]`처럼 속성 셀렉터를 사용하는것을 권장 합니다.

*Note:*  
SMACSS에 의한 Layout에는 그리드 레이아웃을 위한 모듈도 포함됩니다.

FLOCSS에는 그리드 프레임 워크로 정의, 
구체적으로는 CSS 프리 프로세서와 비슷한 mixin이나 function이 있는 경우는, 여러 곳에 include 될 것을 고려하고 그 Foundation 레이어로 다뤄야 한다고 생각하고 있습니다.
예를 들어 그것들은 [Susy](http://susy.oddbird.net/)、[Bourbon Neat](http://neat.bourbon.io/)、[Kite](https://github.com/hiloki/kitecss)과 같은 레이아웃에 관한 프레임 워크입니다,

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

OOCSS의 콘셉을 기본으로, 프로젝트 전반에 반복되는 비쥬얼 패턴을 모두 **Object**라고 정의합니다.

FLOCSSで의 Object는, 추가로 다음의 세가지 레이어로 나눠집니다.

#### 1. Component

재사용이 가능한 패턴으로 작은 단위의 모듈을 정의합니다.


일반적으로 많이 사용되는 패턴이 있어, 예를들어 [Bootstrap의 Component 카테고리](http://getbootstrap.com/components/) 등에 보여지는 `button`등이 해당됩니다. 
가능한 최소의 기능으로 정의되어야 하고 그 자체가 고유의 폭이나 색등의 특색을 지니는 것은 피하는것이 좋습니다.  

#### 2. Project

프로젝트 고유의 패턴인 몇몇의 Component와 그것에 해당하지 않는 요소로 구성되어지는 것을 정의합니다.

예를 들어, 기사 리스트나 사용자 프로필, 이미지 갤러리 등의 콘텐츠를 구성하는 요소등이 해당됩니다. 

#### 3. Utility

Component와 Project 레이어의 Object modify로 해결하는것이 어렵다・적절한게 없다, 소소한 스타일의 조정을 위한 편리한 클래스등을 정의합니다.

Utility는 Component、Project 레이어의 Object를 무진장 늘어나는 것을 방지하거나, 또 이것의 Object 자체가 가지고 있지 않아야 할 `margin`의 대신에 `.mbs { margin-bottom: 10px; }`같은 Utility Object를 이용해, 인접한 모둘과의 간격을 만들겠다는 역활을 담당합니다.

또한 clearfix 테그닉을 위한 룰세트가 정의된 헬퍼 클래스도 이 레이어에 포함됩니다.

## 네이밍 규칙

### MindBEMding

[BEM](http://bem.info/) 시스템의 문법에 있는 **Block**、**Element**、**Modifier**로 분류하고 구성된 규칙을 채용 합니다.

FLOCSS에는 오리지날의 BEM의 문법이 아닌 [MindBEMding](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)의 아이디어를 기본적으로 그대로 도입하고 있습니다.

Modifier의 네이밍의 파생 패턴으로, JavaScript로 조작되는 [상태]를 나타내는 Modifier에 대해서는 SMACSS의 **State** 패턴의 네이밍을 빌리고, `is-*` 프리픽스를 부여하고, `.is-active`같은 것도 가능합니다.

```html
<button class="c-button is-active">Save</button>
```

```css
.c-button { ... }
.c-button.is-active { ... }
```

이 아이디어를 채용하는 경우의 원칙으로써 `.is-active` 란 곳에 룰을 가지게 하는 것은 **금지**합니다. 이것은 `.is-active` 그것이 가진 룰이 다른 모튤의 Modifier의 스타일을 지져분하게 하는 것을 방지하기 위함입니다.

### Object의 프리픽스 

Object 레이어 안에 분류된 모듈에 대해 역활을 명확하게 하기 위해서 프리픽스를 붙히는 것을 권장 합니다.
- Component - `.c-*`
- Project   - `.p-*`
- Utility   - `.u-*`

*Note:*  
이것을의 네이밍 규칙은, 당신의 프로젝트가 가진 오리지날의 네이밍 규칙에 따라, 카멜케이스 등에 따라가도 상관없습니다만 꼭 **네이밍에 일관성을 유지한다**로 부탁합니다.

## 파일・디렉토리 구성

기본 원칙의 레이어 구성에 따라 아래와 같이 구성을 전제하고 있습니다.

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


혹시 Sass나 Stylus 같은 CSS 프리 프로세서나 빌드 툴을 사용한 CSS 파일을 결합해야 할 환경이라면, 아래처럼 디렉토리를 나눠서 관리하는것을 추천합니다.

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

모듈 단위로 파일을 분할함으로써, 페이지 단위 혹은 프로젝트 단위의 모듈의 추가・삭제의 관리가 쉬워집니다.

이것들을 통괄하기 위해 `app.scss`같은 파일에서는 다음과 같이 참조합니다.


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


## 캐스팅과 세밀도

캐스팅은 CSS의 가장 큰 특징 중에 하나입니다. 운영중에 CSS가 망가지는것은, 섵부른 캐스팅과 셀렉터의 관리에 원인이 있는게 많습니다.

FLOCSS에는 마지막에 오는 레이어가 될 정도로 구체적이 되거나, 캐스팅에 의한 강한 규칙이 필요합니다. 그 때문에, **레이어의 순서를 앞뒤로 놓지 않습니다.**


### 레이어, 모듈의 캐스팅

원칙적으로 모듈 사이의 캐스팅, 다른 모듈을 부모로 한 셀렉터를 사용한 캐스팅은 **금지**합니다.
특히 동일의 레이어에 의한 모듈사이의 캐스팅, 예를 들어 다음과 같은 복수의 셀렉터를 이용한 캐스팅은 바람직하지 않습니다.

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

왜냐면, 그 레이어에 의한 특정이 모듈에 존재하는것이 없이, 모듈로써 독립해 재사용 되어야 하고, 혼재시킴으로써 다른 개발자가 예상하지 않은 동작이 되면 안되기 때문입니다. 

예외로써, 레이어 사이에 걸친 캐스팅, 예를 들어 다음과 같은 **Project 레이어가 Component 레이어 모듈을 변경하는 것은 허용** 합니다.

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


다만 이런 예가 있어도 셀렉터의 캐스팅을 할 필요는 없고, 다음과 같이 Project 레이어의 **Element**이나, Component 레이어의 **Modifier**에 따라 확장할 경우에 따라 해결되는 경우가 있습니다.


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

이처럼 해결된 경우에는, 세밀도를 강하게 키우는 것을 막을수 있습니다.

한편, 대상의 디자인이 복잡하고 패턴이 다방면으로 걸친 것 같은 것이 있는 경우, Element나 Modifier로 코드가 넘쳐나 버리는 경우도 있습니다.
그 경우 모듈 자체가 번잡해 지는 문제가 야기되면 처음에 알려준 샘플 같은 Project 레이어에서의 Component 레이어의 캐스팅이 그 예외로써 허용합니다.
그 때에는 다음과 같은 룰을 부과 하도록 하세요.

- Project 레이어 모듈내에, 동일의 Component 레이어 모듈이 복수로 위치하는 경우를 고려해 가능한 한 자식 셀렉터등을 활용하고 적용범위를 한정한다.
- 다른 모듈을 부모 셀렉터로 가지는 것은 **하나**만 한다.


### 모듈 내의 캐스팅

다른 레이어나 모듈을 횡단하는 캐스팅과는 다르게, 모듈 내에 완결하는 캐스팅, 복수이 셀렉터를 사용하는 것은 **허용**합니다.
예를 들어, 다음과 같은 `Tabs`모듈의 경우입니다.

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

이 경우에도, 보다 건실한 설계를 고려한다면 다음과 같이 쓰는것이 좋을거 같습니다.

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


이 샘플 외에도, 리스트 계열 모듈에 의한 인접한 요소간의 보더가 필요, 한 케이스에는 인접 셀렉터 'E + E {...}`를 이용한 경우도 있고 ":hover"와 같은 유사 글래스나 상태를 표시하는 클래스를 붙히는 경우도 있습니다.


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

## CSS 프리 프로세서의 Extend

CSS 프리 프로세서의 대부분 갖고 있는 셀렉터를 계승하기 위한 Extend는, 원칙 그 모듈로 완결되어 계승 밖에서는 사용을 금지합니다.

레이어나 모듈을 넘어 Extend에 따라 계승을 시킬 경우, FLOCSS의 구성・설계는 망가져 캐스팅 룰도 복잡해져 버릴 가능성이 있기 때문입니다.

다음은 예외로서 허용되는 패턴입니다.

### 모듈로 완결한 Extend

다음과 같은 button 모듈도 있습니다.

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

FLOCSS의 구상에는, 이처럼 멀티 클래스 패턴을 기본으로 하고 있습니다만, 다음과 같이 Extend에 따른 싱글 클라스에도 가능합니다.

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

이처럼 모둘안에서 완결 하는 한, 관리가 번거롭고 복잡해질수 있기 때문에 허용합니다.

