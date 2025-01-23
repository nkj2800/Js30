# Exercise 3: CSS Variables

The web page provided in this exercise displays an image, and has 3 form inputs
  from which the user can manipulate the padding, blur amount, and background
  color of the image. Update the CSS and write the JavaScript code necessary to
  bring functionality to the inputs.

## Guide

The purpose of this exercise is to gain experience using _CSS3 variables_. These are
  **different** from Sass-style variables; Sass variables are defined in the Sass file,
  but once compiled to CSS the values cannot be updated. _CSS3 variables_ can have
  their values updated through the use of JavaScript. The `input` _HTML elements_
  have a `name` property that corresponds with the CSS properties we want to update.
  We can create _CSS3 variable references_ and attach them to the root element, provide
  them with some default values, and utilize JavaScript to attach _event listeners_
  to the `input` _HTML elements_ that will call upon an _event handler_ function
  whenever the input values have been changed by the user. We will define the function
  to target the _entire document_ and update the values of the CSS variables
  from there.

**Steps:**

- CSS:

  1. Declare a new style for the `:root` element and declare three variables inside
    the style definition for `:root` with the same names as the `input` _HTML elements_.
    _CSS3 variables_ are declared in the following syntax format:
      ```CSS
      /* Two hyphens (--) followed by the variable name */

      :root {
        --base: #ffc600;
        --blur: 10px;
        --padding: 10px;
      }
      ```

  1. Declare a new style for the `img` element and set the `background`, `filter`, and
    `padding` properties to the variables we defined at the root element:
      ```CSS
      /* 'var(--variableName)' to use previously defined CSS properties */

      img {
        background: var(--base);
        filter: blur(var(--blur));
        padding: var(--padding);
      }
      ```

  1. Declare a new style for the `.hl` class and set the color to the `base` variable.

- JavaScript:

  1. Declare & define a variable as a reference to all of the inputs on the page.

  1. Iterate through the _HTML Node Elements_ that the variable is referencing and
    attach _event listeners_ to each one that will call on an _event handler_ whenever
    the input value has been changed (the `change` event).

  1. Repeat step 2, listening for mouse movements on the inputs instead of value
    changes (the `mousemove` event).

  1. Define a function that will be used as the _event handler_. It will update
    the value of the _CSS3 variable_ **at the root document level** corresponding with
    the `name` property of the `input` element which called this function.
        - Minor 'gotcha': Properties like `padding` and `blur` won't update because
      the value from the input does not include the type of measurement we are using
      ('px', 'em', etc.). The `input` _HTML elements_ also have a `data-sizing` property if
      they require a suffix. We can use this to attach the correct suffix to the
      value if necessary.




`data-` attribute, `:root`, CSS Variables definition `var(--xxx)`, `filter: blur()`, `change` event and `mousemove` event

- [`dataset`](https://developer.mozilla.org/zh-TW/docs/Web/API/HTMLElement/dataset) property allows to custom data attributes like `data-xxx` on the element, either in HTML or in the DOM. It's a map of **DOMString**, one entry for each custom data attribute.

- `:root` selector matches the document's root element is always the html element and it's also where we declare the variable for the base element in HTML.

- once we declare CSS Variables, then we can add it to our specific elements, like `img` below, check how to declare it [here](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables).

- CSS Variable declare syntax is `--`, just like `$` in SASS.

```
:root {
  --spacing: 10px;
}

img {
  padding: var(--spacing);
}
```

- CSS `filter` provides such as `blur`, `bightness` and so on, take a look at it [here](https://developer.mozilla.org/en-US/docs/Web/CSS/filter).

- NodeList v.s. Array : NodeList is **NOT** an Array. You can open the `proto` in dev tool and  see its methods, there are `forEach()`, `keys()`..., and Array's prototype has `map()`, `pop()`...etc.

### Handling suffix with dataset

use `dataset` to deal with suffix `px` by adding `data-sizing: px` as an attribute on input element.

```
<input type="range" name="blur" min="0" max="25" value="10" data-sizing="px">
```

and the get the suffix by `dataset.sizing` via JS

```
const suffix = this.dataset.sizing || '';
```

and don't forget a condition with `|| ''` for `<input type=color>` which has no `px`.

### Changing CSS property via JS

`document.documentElement` is the root element in JS, so we can change the global CSS variables by JS is just `setProperty` to `style` like so:

```
document.documentElement.style.setProperty('--base', '#000');
```
