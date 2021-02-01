# Crafting CSS

A practical guide to crafting better CSS. Highly opinionated.

## Questions

- How to enforce visual consistency, i.e. a visual grammar made of visual patterns?
- How no to repeat oneself?
- How to ensure component's CSS code isolation/avoid leakages?
- How to minimize coupling between structure (think `div`) and style?

For sure, there will be trade-offs.

## Summary

- [Define styles in a dedicated stylesheet](#define-styles-in-a-dedicated-stylesheet)
- [Do not refer to elements](#do-not-refer-to-elements)
- [Do not refer to IDs](#do-not-refer-to-ids)
- [Separate layout and skin](#separate-layout-and-skin)
- [Separate container and content](#separate-container-and-content)
- [Do not use location-dependent styles](#do-not-use-location-dependent-styles)
- [Create small objects](#create-small-objects)
- [Use a CSS framework](#use-a-css-framework)
- [Create components with your favorite library/framework](#create-components-with-your-favorite-libraryframework)
- [The Block\_\_Element--modifier naming convention](#the-block__element--modifier-naming-convention)
- [Should I use a preprocessor after all?](#should-i-use-a-preprocessor-after-all)
- [What should I do now?](#what-should-i-do-now)
- [Further reading](#further-reading)

## Define styles in a dedicated stylesheet

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

### Why

High coupling. In the first place, CSS was designed to decouple structure and style. But in fact, that's a matter of choice (avoiding coupling is hard, so some developers prefer not even try to avoid it, hence CSS-in-JS).

[Summary](#summary)

---

## Do not refer to elements

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

### Why

High coupling. Separating structure and skin allows to:

- apply different styles to a given element, or no style at all
- apply the same style to other elements
- use another more suited element without breaking the style

[Summary](#summary)

---

## Do not refer to IDs

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

### Why

Overly specific. There's simply no reason to use IDs. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) and they are not reusable. IDs are primary used for anchoring (see: [Linking to an element on the same page](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#Examples)).

[Summary](#summary)

---

## Separate layout and skin

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

### Why

Layout refers to properties that determine the size and the positionning of an element, and consequently the overall aspect of the page. Skin refers to properties relative to colors, fonts, text alignment and so on. Skin is much more likely than layout to be reused.

[Summary](#summary)

---

## Separate container and content

In fact, the previous code could (should?) be refactored!

### Don't

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

### Why

An element should not be responsible for its own positionning!

[Summary](#summary)

---

## Do not use location-dependent styles

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

### Why

An object should look the same no matter where you put it. Otherwise, there is a coupling between structure (`.html`) and style (`.css`). As a consequence, you should avoid [combinators](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors#Combinators) and nesting with preprocessors.

Oh, by the way, are you sure you need a preprocessor?

[Summary](#summary)

---

## Create small objects

### An example, please!

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

### Why

Doing so, you define a visual grammar of styles once and for all, in an `index.css` or `common.css` file for instance. If you do that in a large extent, remember that that's basically what CSS frameworks do and jump to the next paragraph.

Ok, `top-fixed-container`, `content-horizontally-centered` and `standard-padding` have finally took place in our `.html`, as soldiers with the Trojan horse, but we knew that some compromises had to be made. Here, we agree on not separating concerns (structure and style) in favor of a reduced coupling.

### Limitations

The object's granularity is up to you. We usually refer to an extreme granularity as ["Atomic CSS"](https://acss.io/), which leads to writing such things:

```css
.background-blue {
  background-color: blue;
}
```

Doing so, is it still usefull to define CSS classes? Should you prefer a red backgroud, then the naming should be adapted accordingly. Which basically means defining and refering to a new class. Heading in this direction, why not simply write CSS in HTML or [CSS in JS](https://cssinjs.org/)?

We strongly discourage this approach because it stands down on all [the ambitions we had](#questions).

[Summary](#summary)

---

## Use a CSS framework

The choice is yours, between [Bootstrap](https://getbootstrap.com/) and [Material Design](https://getmdl.io/) for the main ones, as well as [Pure CSS](https://purecss.io/), [Bulma](https://bulma.io/) or [Tailwind](https://tailwindcss.com/) for emerging ones. See [9 Best CSS Frameworks in 2021](https://athemes.com/collections/best-css-frameworks/) for their respective pros and cons.

[Summary](#summary)

---

## Create components with your favorite library/framework

### Don't

```html
<!-- HTML -->
```

```css
/* CSS */
```

### Do

```html
<!-- HTML -->
```

```css
/* CSS */
```

### Why

[As the React documentation suggests](https://create-react-app.dev/docs/adding-a-sass-stylesheet/), if you find yourself always the same and same styles, maybe you should create a component for that.

[Summary](#summary)

---

## The Block\_\_Element--modifier naming convention

For those components where you need to write some specific CSS code, BEM is your friend. In essence, it's a useful naming convention and a simple way to avoid leakages. That's what we're going to use it for, although some people use it as a broader methodology.

Please refer to the [BEM documentation](https://en.bem.info/methodology/key-concepts/#Unified-Data-Domain).

[Summary](#summary)

---

## Should I use a preprocessor after all?

We've seen before that [nesting should be avoided as much as possible](#do-not-use-location-dependent-styles) and that [small objects are a valid alternative to preprocessors when it comes to creating a visual vocabulary](#create-small-objects).

This makes preprocessors ([LESS](http://lesscss.org/), [SASS](https://sass-lang.com/)) much less usefull, though maybe not useless in some very specific cases.

[Summary](#summary)

---

## What should I do now?

TODO

[Summary](#summary)

---

## Further reading

- [Modern CSS Explained For Dinosaurs, by Peter Jang](https://medium.com/actualize-network/modern-css-explained-for-dinosaurs-5226febe3525)
- [CSS Utility Classes and "Separation of Concerns", by Adam Wathan](https://adamwathan.me/css-utility-classes-and-separation-of-concerns/)

[Summary](#summary)
