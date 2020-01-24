# Crafting CSS

```
/!\ WIP /!\
```

A practical guide to crafting better CSS. Inspired by [OOCSS](https://github.com/stubbornella/oocss/wiki#separate-container-and-content) and [BEM](https://en.bem.info/methodology/key-concepts/#Unified-Data-Domain), but not only. Highly opinionated.

Summary:

- []()
- []()

## Questions

- Coupling
- Reusability / consistency / visual grammar / visual patterns
- isolation / components

## Principles

- Separate layout and skin (a.k.a. structure and skin)
- Namespace

## Define styles in a dedicated stylesheet

Implements: `separate-layout-and-skin`

### Don't

```html
<!-- HTML -->
<p style="color: red;">Lorem ipsum dolor sit amet.</p>
```

### Do

```html
<!-- HTML -->
<p class="danger">Lorem ipsum dolor sit amet.</p>
```

```css
/* CSS */
.danger {
  color: red;
}
```

[See result](./assets/b79201fd-8f35-484e-8bde-71be182234b6.png)

### Why

High coupling. In the first place, CSS was designed to decouple structure and style. But in fact, that's a matter of choice (avoiding coupling is hard, so some developers prefer not even try to avoid it, hence CSS-in-JS).

---

## Do not refer to elements

Implements: `separate-layout-and-skin`

### Don't

```html
<!-- HTML -->
<p>Lorem ipsum dolor sit amet.</p>
```

```css
/* CSS */
p {
  color: red;
}
```

### Do

```html
<!-- HTML -->
<p class="danger">Lorem ipsum dolor sit amet.</p>
```

```css
/* CSS */
.danger {
  color: red;
}
```

[See result](./assets/b79201fd-8f35-484e-8bde-71be182234b6.png)

### Why

High coupling. Separating structure and skin allows to:

- apply different styles to a given element, or no style at all
- apply the same style to other elements
- use another more suited element without breaking the style

---

## Do not refer to IDs

Implements: `separate-layout-and-skin`

### Don't

```html
<!-- HTML -->
<p id="danger">Lorem ipsum dolor sit amet.</p>
```

```css
/* CSS */
#danger {
  color: red;
}
```

### Do

```html
<!-- HTML -->
<p class="danger">Lorem ipsum dolor sit amet.</p>
```

```css
/* CSS */
.danger {
  color: red;
}
```

[See result](./assets/b79201fd-8f35-484e-8bde-71be182234b6.png)

### Why

Overly specific. There's simply no reason to use IDs. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) and they are not reusable. IDs are primary used for anchoring (see: [Linking to an element on the same page](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#Examples)).

---

## Separate layout and skin properties

Implements: `separate-layout-and-skin`

### Don't

```html
<!-- HTML -->
<div class="danger">Lorem ipsum dolor sit amet.</div>
```

```css
/* CSS */
.danger {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  box-sizing: border-box;
  padding: 1rem;
  background-color: red;
  text-align: center;
  color: white;
  font-weight: bold;
}
```

### Do

```html
<!-- HTML -->
<div class="danger-layout danger">Lorem ipsum dolor sit amet.</div>
```

```css
/* CSS */
.danger-layout {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
}

.danger {
  box-sizing: border-box;
  padding: 1rem;
  background-color: red;
  text-align: center;
  color: white;
  font-weight: bold;
}
```

[See result](./assets/8de7ffee-6032-4161-a0b0-405e4895f355.png)

### Why

Layout refers to properties that determine the size and the positionning of an element, and consequently the overall aspect of the page. Skin refers to properties relative to colors, fonts and so on. Skin is much more likely than layout to be reused.

In the example above, `padding: 1rem;` may rise questions: should it be considered as layout or skin? As a first approximation, the padding has no impact on the computed size of the element thanks to `box-sizing: border-box;` ([Cf. MDN](https://developer.mozilla.org/fr/docs/Web/CSS/box-sizing)). This, of course, would be false in case of a low width combined with a high margin.

---

## Separate container and content

Implements: `?`

### Don't

In fact, the [code](#do-3) we wrote just before could be refactored!

### Do

```html
<!-- HTML -->
<div class="container">
  <div class="danger">Lorem ipsum dolor sit amet.</div>
</div>
```

```css
/* CSS */
.container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
}

.danger {
  box-sizing: border-box;
  padding: 1rem;
  background-color: red;
  text-align: center;
  color: white;
  font-weight: bold;
}
```

[See result](./assets/8de7ffee-6032-4161-a0b0-405e4895f355.png)

### Why

---

## An element should not be responsible for its own positionning

Implements: `?`

### Don't

```html
<!-- HTML -->
<div class="container-layout container__content">
  <div class="danger-layout danger">Lorem ipsum dolor sit amet.</div>
</div>
```

```css
/* CSS */
.container-layout {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
}

.container__content {
  text-align: center;
}

.danger-layout {
  display: inline-block;
}

.danger {
  box-sizing: border-box;
  padding: 1rem;
  background-color: red;
  color: white;
  font-weight: bold;
}
```

### Do

```html
<!-- HTML -->
<div class="container-layout container__content">
  <div class="danger-layout danger">Lorem ipsum dolor sit amet.</div>
</div>
```

```css
/* CSS */
.container-layout {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
}

.container__content {
  display: flex;
  justify-content: center;
}

.danger {
  box-sizing: border-box;
  padding: 1rem;
  background-color: red;
  color: white;
  font-weight: bold;
}
```

[See result](./assets/07813908-c78d-4a18-8dcd-15ac9681f58a.png)

### Why

For the same reason as before: an object should look the same no matter where you put it. [Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) and [Grid](https://css-tricks.com/snippets/css/complete-guide-grid/) are your friends!

---

## Do not use location-dependent styles

Implements: `?`

### Don't

```html
<!-- HTML -->
<div class="container">
  <p class="content">Lorem ipsum dolor sit amet.</p>
</div>
<p class="content">Lorem ipsum dolor sit amet.</p>
```

```css
/* CSS */
.container .content {
  color: red;
}
```

Or:

```scss
// SASS
.container {
  & .content {
    color: red;
  }
}
```

### Do

```html
<!-- HTML -->
<div class="container">
  <p class="content danger">Lorem ipsum dolor sit amet.</p>
</div>
<p class="content">Lorem ipsum dolor sit amet.</p>
```

```css
/* CSS */
.danger {
  color: red;
}
```

[See result](./assets/25f75bd9-251e-4c3c-abb0-316a65d9c381.png)

### Why

An object should look the same no matter where you put it. As a consequence, you should avoid [combinators](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors#Combinators) and nesting with preprocessors.

---

## Create small objects

Implements: `?`

### Using CSS

```html
<!-- HTML -->
<div class="top-fixed-container content-horizontally-centered">
  <div class="danger standard-padding">Lorem ipsum dolor sit amet.</div>
</div>
```

```css
/* CSS */
.top-fixed-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
}

.content-horizontally-centered {
  display: flex;
  justify-content: center;
}

.danger {
  background-color: red;
  color: white;
  font-weight: bold;
}

.standard-padding {
  box-sizing: border-box;
  padding: 1rem;
}
```

### Or using preprocessors

```html
<!-- HTML -->
<div class="container-layout container__content">
  <div class="danger-layout danger">Lorem ipsum dolor sit amet.</div>
</div>
```

```scss
// SASS
@mixin top-fixed-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
}

@mixin content-horizontally-centered {
  display: flex;
  justify-content: center;
}

@mixin danger {
  background-color: red;
  color: white;
  font-weight: bold;
}

@mixin standard-padding {
  box-sizing: border-box;
  padding: 1rem;
}
```

```css
/* CSS */
.container-layout {
  @include top-fixed-container();
}

.container__content {
  @include content-horizontally-centered();
}

.danger {
  @include danger();
  @include standard-padding();
}
```

### Why

- Le concept de OOCSS est de repérer des « objets CSS », c'est-à-dire des « patterns visuels » qui se répètent, et de définir ainsi des classes réutilisables. La méthode consiste à prendre le design comme point de départ : on repère des répétitions visuelles puis on les nomme. Exceptionnellement, des classes CSS nommées selon l'apparence sont autorisées à partir du moment où elles sont génériques. C'est une grammaire.

Beware: https://create-react-app.dev/docs/adding-a-sass-stylesheet/

---

### BEM

The [BEM documentation](https://en.bem.info/methodology/key-concepts/#Unified-Data-Domain) describes the basics of the methodology.

BEM emphasises on the relation between entities (HTML elements or components): in "Block, Element, Modifier", the key part is the Element, defined as "a constituent part of a block that can't be used outside of it". As seen [previously](), an element should not be responsible for its own positionning, that's the responsibility of its parent. The element (`MyComponent__Message` in the example below) is the way to implement this responsibility.

BEM also avoids CSS leaks between components, since the bloc name and its derivatives are unique. As such, it acts as a namespace. Please note that BEM is a this naming convention in which only the concepts matter: `my-component__message` would also be a valid syntax, as well as any syntax using any delimiter as long as it implements a structure in blocks, elements and modifiers.

```html
<!-- HTML -->
<div class="MyComponent top-fixed-container content-horizontally-centered">
  <div class="MyComponent__Message danger standard-padding">
    Lorem ipsum dolor sit amet.
  </div>
</div>
```

or, in the perspective of defining a dedicated component:

```html
<!-- HTML -->
<div class="MyComponent top-fixed-container content-horizontally-centered">
  <div class="MyComponent__Message">
    <div class="danger standard-padding">
      Lorem ipsum dolor sit amet.
    </div>
  </div>
</div>
```

```css
/* CSS */
.MyComponent {
  box-sizing: border-box;
  padding: 10px;
}

.MyComponent__Message {
  width: 50%;
}
```

[See result](./assets/b2f12eaf-965b-4d5d-9403-c3293da57cda.png)

## Should you use a preprocessor after all?

We've seen before that [nesting should be avoided as much as possible]() and that [OOCSS is a valid alternative to preprocessors when it comes to creating a visual vocabulary](). This makes preprocessors ([LESS](http://lesscss.org/), [SASS](https://sass-lang.com/)) much less usefull, though not useless.

Indeed, preprocessors bring powerful functionalities like using variables and adding behavior, that can be leveraged at the step of defining the visual vocabulary mentionned above. In some components involving complex positionning, preprocessors remain a must-have.

In other words, use them for the right thing. And keep it (stupid) simple, as always.
