# BECM

BECM is inspired by the [BEM naming](http://getbem.com/naming) guidelines.
BECM stands for:
-   Blocks
-   Elements
-   Chained modifiers
-   Modifiers

Below are all 4 types described and their best practices. All examples are written in [`SASS`](http://sass-lang.com/).

> 🚧 It is advised to create a prefix for your project. In the examples below did i use the prefix `becm` as an example.

## Blocks

A block is a standalone entity that does not depend on any above DOM structure. Blocks can be nested within each other but should not depend on one another.

> It is advised to use a prefix for each block in the examples below do we use the prefix `becm` as an example

**Example**

```sass
.becm-button {
}
```

```html
<div class="becm-button"></div>
```

## Element

Elements are part of a block. These elements are concatenated with the block element seperated by a single underscore (`_`).
This isolates the child element and enables us to reuse it later on ([reusing/extending classes](#reusingextending-classes)).
A element can contain other elements or mutations and so forth.

**Example**

```sass
.becm-button {
  &_icon {
  }
}
```

```html
<div class="becm-button"><i class="becm-button_icon"></i> Submit</div>
```

## Modifier

A modifier modifies a block and it's elements. A modifier should always modify more then one element.
If you want to create a modifier that modifies only the given element, use a [chained modifier](#chainedmodifiers).

Below is a example of the modifier `-large` that changes the `font-size` from `18px` to `24px` of the `becm-button`.
I also included the generated CSS.

```sass
.becm-button {
  background: silver;
  font-size: 18px;

  &_icon {
  }

  &-large {
    @extend .becm-button;
    font-size: 24px;

    &_icon {
      display: none;
    }
  }
}
```

**Generated CSS**

```css
.becm-button,
.becm-button-large {
  background: green;
  font=size: 18px;
}

.becm-button_icon,
.becm-button-large_icon {
}

.becm-button-large {
  font-size: 24px;
}

.becm-button-large_icon {
  display: none;
}
```

```html
<div class="becm-button"><i class="becm-button_icon"></i> Submit</div>
<div class="becm-button-large"><i class="becm-button-large_icon"></i> Submit</div>
```

## Chained Modifiers

A chained modifier is a modifier that only changes the style of the given element. Multiple chained modifiers can be applied to a element at the same time.

> ⚠️ Remember a chained modifier should only change the element it is applied to and not other child element(s).

Every chained modifier should start with one of the following prefixes:
`has-`, `is-` or `no-`

**Example**

```sass
.becm-button {
  background: green;
  font-size: 18px;

  &.has-shadow {
    box-shadow: ...;
  }
}
```

```html
<div class="becm-button has-shadow"></div>
```

## Reusing/extending classes

One of the main reasons of using the BECM naming guide is of the reusability that it offers. Every block/child element should be isolated and have the plausibility to be used as a standalone element. Below are we creating a `form` and `contact` block where the `input` fields can be used in a standalone scenario.

**Example**

```sass
.becm-form { // <- form block
  &_input {
    background: green;
  }
}

.becm-contact { // <- contact block
  &_input {
    background: orange;
  }
}
```

```html
<div class="becm-form">
  <div class="becm-form_input"></div>
</div>

<div class="becm-contact>
  <div class="becm-contact_input"></div>
</div>

<!-- The input field from the form block is here used in a standalone case -->
<div class="becm-form_input"></div>
```

This is a very simple example of how you could reuse the same input field without relying on any previous DOM structure. Below is another example of how you could reuse blocks/child elements with the `@extend` method. `@extend` extends the given class with the scope class name. Again will we create a small form but will we use the `becm-form_input` as base for `becm-contact_input`.

**Example**

```sass
.becm-form { // <- form block
  &_input {
    background: green;
    font-size: 16px;
  }
}

.becm-contact { // <- contact block
  &_input {
    @extend .becm-form_input;
    background: orange;
  }
}
```

**Generated CSS**

```css
.becm-form_input,
.becm-contact_input {
  background: green;
  font-size: 16px;
}

.becm-contact_input {
  background: orange;
}
```

The result stays the same but by using extend is the `font-size` of `becm-form_input` also applied to `becm-contact_input`.
