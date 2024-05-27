# CSS

Work in progress

## Content

- [BEM Methodology](#bem-methodology)
- [TWP Starter Theme common SASS usage](#twp-starter-theme-common-sass-usage)
- [Additional notes](#additional-notes)
- [Other style guides](#other-style-guides)

<br>
<br>

---

## BEM methodology

By using the Block-Element-Modifier (BEM) CSS methodology, the chances of undesired and conflicting style overrides is virtually eliminated, and the need for nesting is diminished (which usually results in better performance, by eliminating excessive selector depth).

### Block, element, modifier basics

```scss
.block {
}

.block__element {
}

.block__element--modifier {
}

.block--modifier {
}
```

A good example is a simple card:

```scss
// Product block
.product {
  color: black;
}

// Product elements
.product__image {
  opacity: 1;
}

// Product modifier
.product--out-of-stock {
  color: red;

  .product__image {
    opacity: 0.5;
  }
}
```

```html
<!-- Normal product / color is black -->
<div class="product">
  <img class="product__image" src="image.jpg" /> <!-- image is 100% opaque -->
</div>

<!-- Out of stock product / color is red -->
<div class="product product--out-of-stock">
  <img class="product__image" src="image.jpg" /> <!-- image is 50% opaque -->
</div>
```

### Do not use SASS `&__` or `&--`

That will make it much harder to debug and find the line of code.

```scss
// Wrong
.product {
  color: black;

  &__image {
    opacity: 1;
  }
}

// Right
.product {
  color: black;
}

.product__image {
  opacity: 1;
}
```

### Avoid over-qualified selectors by (unnecessary) nesting

That means the following code should be avoided. Not only because it produces more code, it also makes it harder to keep track of the parent-children chain, and harder to debug in some cases. The longer the rules, the harder it gets to keep track of the parent when deep nesting is used.

```scss
// Unnecessary nesting
.product {
  color: black;

  .product__image {
    opacity: 1;
  }

  &.product--out-of-stock {
    color: red;

    .product__image {
      opacity: 0.5;
    }
  }
}

// Would result in:
.product { color: black }
.product .product__image { opacity: 1 }
.product.product--out-of-stock { color: red }
.product.product--out-of-stock .product__image { opacity: 0.5 }

// As opposed to how it should be (without the unnecessary nesting):
.product { color: black }
.product__image { opacity: 1 }
.product--out-of-stock { color: red }
.product--out-of-stock .product__image { opacity: 0.5 }
```

In the above example, `.product .product__image` is an over-qualification, as `.product__image` itself is sufficient to target the correct element.

### Blocks can also be elements of other blocks

In slightly more complex situations, blocks can also be elements of other blocks:

```scss
// Price block
.price {
  font-weight: 700;
}

// Product price element
.product__price {
  font-weight: 400;
}
```

```html
<!-- Price used isolated / 700 -->
<p><span class="price">$100</span></p>

<!-- Price used as a product element / 400 -->
<div class="product">
  <p><span class="price product__price">$100</span></p>
</div>
```

<br>
<br>

---

## TWP Starter Theme common SASS usage

### Use `$spacing-x` variables for defining margins and paddings

```scss
.block {
  padding: $spacing-md;
}
```

### Use `media-min` and `media-max` mixins for breakpoints

```scss
.block {
  font-size: rem(12px);

  @include media-max($large) {
    font-size: rem(16px);
  }

  @include media-min($large) {
    font-size: rem(24px);
  }
}
```

<br>
<br>

---

## Additional notes

### Prefer `data-attributes` as selectors in JS

Whenever possible and where it makes sense.

```html
<!-- HTML -->
<button class="button button--primary" data-toggle-button>
  Open menu
</button>
```

```css
/* CSS */
.button {
}

.button--primary {
}
```

```js
// JS
const toggleButton = container.querySelector('[data-toggle-button]');
```

<br>
<br>
<br>

---

## Other style guides

1. [Core Principles](/README.md)
2. [Javascript](/JAVASCRIPT.md)
3. CSS
4. [Liquid & HTML](/LIQUID-HTML.md)
