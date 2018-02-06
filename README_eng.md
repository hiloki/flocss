**FLOCSS** is a CSS architecture methodology that adopts concepts from [OOCSS](https://github.com/stubbornella/oocss/wiki),
 [SMACSS](https://smacss.com/), BEM](http://bem.info/) and [SuitCSS](http://suitcss.github.io/). It has also been heavily influenced by the layer structures of [MCSS](http://operatino.github.io/MCSS/). 

## Basic Principles

FLOCSS categorizes layers into the following 3 layers and 3 child layers of the  **Object** layer.

1. Foundation - reset/normalize/base...
2. Layout - header/main/sidebar/footer...
3. Object
   1. Component - grid/button/form/media...
   2. Project - articles/ranking/promo...
   3. Utility - clearfix/display/margin...

In order to enhance reusability and scalability, we categorize our CSS using the above rule.

### Foundation

The Foundation layer defines the defaults. It includes resetting browser stylings such as Reset.css and Normalize.css and defines some basic styling in the project. The background color and basic typography of the project should be included in this layer.

### Layout

The layout layer defines the styles of large common containers such as the header area, main contents area, sidebar and footer that divides the page into large sections.

Usually, elements defined in the layout layer is unique within each page. Thus, it is possible to use an ID selector on elements in the layout layer. However, since ID selectors have high specificity, using the prefix `.l-`or an attribute selector `[id="header"]` is recommended. 

*Note:*  
The Layout layer in SMACSS includes CSS grid Layout modules. However, in FLOCSS, when defining the grid framework, or more specifically when defining functions or mixins for CSS preprocessors, it is recommended to include it in the Foundation layer as these may be included in other parts of your project. These could include frameworks that define layouts such as [Susy](http://susy.oddbird.net/)、[Bourbon Neat](http://neat.bourbon.io/) and [Kite](https://github.com/hiloki/kitecss).

However, if defining these as classes, it is better to include them in the Object/Component layer. This way, it may be easier to apply more grid frameworks and more layouts to FLOCSS.
(This may change according to CSS maintaining methods)

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

Based on the concept of OOCSS, the Object layer define all visual patterns that repeat within a project.

FLOCSS separates this Object layer into 3 sub-layers.

#### 1. Component

Defines small scale modules as patterns for re-use.

This includes patterns commonly used such as `button` in the [Bootstrap Component category](http://getbootstrap.com/components/).

Modules defined in this layer should have minimal styling. For example, try to avoid defining a set width or color in this layer.

#### 2. Project

Defines a pattern that is unique to a project which consists of several components and other elements.

Elements that make up contents for your project comes under this layer, such as an article list, user profile, image gallery.

#### 3. Utility

Utility classes define small and simple styles used to adjust minimal style changes that are difficult to or not appropriate to define in the Object modifiers of Component and Project layer.

Utility classes can prevent Object modifiers in the Component and Project layer from increasing inexhaustibly. In addition, the Utility classes can be used to apply margins such as `.mbs{ margin-bottom: 10px; }` that the Object itself should not hold.

In addition, helper classes that define the rule sets such as the clearfix technique should also be included in this layer.

## Naming Rules

### MindBEMding

FLOCSS adopts rules from [BEM](http://bem.info/) where naming is structured by categorizing names into **Blocks**、**Elements** and **Modifiers**.

FLOCSS does not use an original BEM syntax. Instead, it follows the ideas of [MindBEMding](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/).

Combining the **State** pattern from SMACSS, the 'is-*' prefix can be used as a modifier-style pattern to describe a state that may be controlled by JavaScript.

```html
<button class="c-button is-active">Save</button>
```

```css
.c-button { ... }
.c-button.is-active { ... }
```

During the use of this idea, the '.is-active' should not hold a style of itself. This is in order to prevent the '.is-active' style from interrupting with styles of other modifier modules.

### Namespacing of the Object

In order to clarify the role of each module, it is advised to add a prefix to each module categorized within the object layer. 

- Component - `.c-*`
- Project   - `.p-*`
- Utility   - `.u-*`

*Note:*  
These naming rules can be changed to allow camel cases according to the naming rules of your project. However, these naming rules need to be **consistent**.

## File structure

File structure should be organized as below.

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

It is advisable to organize your files as follows when using CSS preprocessor or build tools such as Sass or Stylus.

The example below uses Sass.

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

Categorizing the above files using modules makes it easy to organize inserting and deleting page level and project level modules.

All these files can be referred by one file such as `app.scss` and may look like below.

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


## Cascading and Specificity

Cascading is one of the most important characteristics of CSS. When CSS fails, it quite often can be related to poor cascading or selector management.

FLOCSS structures its layers so that the latter layers have more specificity and is stronger in terms of cascading. For this reason **avoid changing the orders of the layers**. 

### Cascading layers and modules

As a general rule it is **not acceptable** to cascade between different modules, and cascade using a parent selector from another module.

It is especially bad practice to cascade between modules in the same layer, such as the one below with several selectors.

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

This rule exists so that each layer is independent of other modules. Thi way, developers are able to be reuse modules without worrying about unexpected behavior.

Cascading between layers is an exception. For example, **it is acceptable for the project layer module to make changes to the component layer modules** as shown below.

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

However, in some cases the same result can be achieved without cascading selectors but by expanding the **Element** of the Project Layer or the **Modifier** of the Component Layer as shown below.

#### Element of the Project Layer

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

#### Modifier of the Component Layer

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

As shown above, one can avoid increasing specificity by these two methods.

On the other hand, complicated designs with various patterns may flood the code with Elements and Modifiers.

This could lead to the complication of your modules and may cause problems. In this case, it would be acceptable for the project layer module to cascade component layer modules.

In cases like the one the above, one should impose the rules below.

- In consideration of cases where the same component layer module is used multiple times across the project layer module, limit the scope of your CSS by using child selectors.
-  Limit the number of modules that have a different module as it's parent selector to one module.

### Cascading inside a module

Unlike casicading across multiple layers or modules, it is **acceptable** to cascade within a module or use multiple selectors.

For example, the `Tabs` module may look as follows:

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

Even in the case above, it may be better to write as below for a more robust architecture.

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

Apart from this case, there may be cases where one may use the adjacent selctor `E + E {...}` to add borders to list modules or use the ":hover" pseudo-classes to add styles in different states.
In all the above cases, it is  **acceptable** to cascade within a module or use multiple selectors.

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

## Using an Extend within a CSS preprocessor
Extend, a function that CSS preprocessors have which allows one to inherit certain selectors, should not be used unless it completes within a certain module.

This is because if an Extend is used across layers or modules, it may fail the architecture of FLOCSS and complicate cascading rules.

The below are some examples of Extend that can be used.

### Extend that completes within a certain module

Please refer to the button module below.

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

In FLOCSS, one should use multi-class patterns like the above. However, one may use Extend to make this a single class pattern as shown below.

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

FLOCSS tolerated such cases as the above where an Extend completes within a module, as it rarely leads to complicated rules.