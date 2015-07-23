# Celery CSS / LESS Styleguide

*A mostly reasonable approach to CSS and LESS, modified from Airbnb*

## Table of Contents

  1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
  1. [Files](#files)
    - [Naming](#naming)
  1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [SUIT](#suit)
    - [ID Selectors](#id-selectors)
    - [JavaScript Hooks](#javascript-hooks)
  1. [LESS](#less)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Nested Selectors](#nested-selectors)

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## Files

### Naming

* Use kebab case.

**Bad**

```
components
  awesome_modal.less
  button.less
  filterLabel.less
other-stuff
  Stuff.less
```

**Good**

```
components
  awesome-modal.less
  button.less
  filter-label.less
other-stuff
  stuff.less
```

## CSS

### Formatting

* Use soft tabs (2 spaces) for indentation
* Prefer dashes over camelCasing in class names. Use Capitalized names for components. (see [SUIT](#suit) below).
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in LESS-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

### SUIT

[SUIT](https://suitcss.github.io/) is heavily influenced by OOCS and BEM. We encourage SUIT for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

> SUIT CSS is a reliable and testable styling methodology for component-based UI development. A collection of CSS packages and build tools are available as modules. SUIT CSS plays well with React, Ember, Angular, and other component-based approaches to UI development.

  * [SUIT Documentation](https://github.com/suitcss/suit/blob/master/doc/README.md)

**Example**

```html
<article class="ProductCard ProductCard--featured is-expanded">

  <h1 class="ProductCard-title">Electronic Doormat</h1>

  <div class="ProductCard-content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>

  <div class="ProductCard-superImportantContent">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>

</article>
```

```css
.ProductCard { }
.ProductCard.is-expanded { }
.ProductCard--featured { }
.ProductCard-title { }
.ProductCard-content { }
.ProductCard-superImportantContent { }
```

  * `.ProductCard` is the “block” and represents the higher-level component
  * `.ProductCard.is-expanded` is a representation of the state of the component.
  * `.ProductCard-title` is an “element” and represents a descendant of `.ProductCard` that helps compose the block as a whole.
  * `.ProductCard--featured` is a “modifier” and represents a different state or variation on the `.ListingCard` block.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

## LESS

### Syntax

* Order your `extend` and regular CSS declarations logically (see below)
* Variable names should use kebab case, like file names.
* Variable names should generally follow `componentname-elementname-variant-state-property` order. Only `property` is required.
* `@*-color` refers to text color by default. If by itself, it should be called `@text-color`.

With a component `MetricPanel`:

**Bad**

```less
@metricpanel-color: @blue-dark;

@MetricPanel-header-color: @gray;
@metric-panel-header-hover-color: @blue;
```

**Good**

```less
@metric-panel-color: @blue-dark;

@metric-panel-header-color: @gray;
@metric-panel-header-color-hover: @blue;
```

### Ordering of property declarations

1. `extend` declarations

    Just as in other OOP languages, it's helpful to know right away that this “class” inherits from another.

    ```less
    .MetricPanel {
      &:extend(.Panel);
      // ...
    }
    ```

1. Property declarations

    Now list all standard property declarations, anything that isn't an `extend`, `@include`, or a nested selector.

    ```less
    .MetricPanel {
      &:extend(.Panel);
      background: green;
      font-weight: bold;
      // ...
    }
    ```

1. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```less
    .MetricPanel {
      &:extend(.Panel);
      background: green;
      font-weight: bold;

      .icon {
        margin-right: 10px;
      }
    }
    ```
### Nested selectors

**Do not nest selectors more than three levels deep!**

```less
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.
