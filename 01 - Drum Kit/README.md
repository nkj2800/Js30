# Exercise 1: JavaScript Drum Kit

An HTML page displays a collection of `div` elements, each containing a letter
  that corresponds with a key on the keyboard, and the name of the soundclip to be
  played when that button is clicked. When a user presses a key that matches
  one of the letters displayed in the `div` elements, the page should play 
  the corresponding soundclip and place a temporary 'highlight' (or border) 
  around the `div`. Write the JavaScript code necessary to add this functionality.

## Guide

We are provided with the HTML, CSS, and sound clips necessary to create this
  page/application. Let's go over the provided files and look at the pieces
  that we can utilize to fulfill the requirements:

- HTML `data-*` attributes: Introduced in HTML5, `data-*` attributes (where * can
    be anything you want) allow us to store _custom data_ on any HTML element. Each
    `div.key` (`<div class="key" data-key="...">`) and `audio` element in the 
    provided HTML file has a `data-key` attribute which corresponds with a keyboard button.

- CSS `playing` class & pre-defined style: The provided CSS file already has a `playing`
    class defined with some rules in it. We will apply this class to the correct
    element, depending on the key pressed by the user, and remove it once animation
    is finished.

And that's...pretty much all we should need from the HTML & CSS files. We can use
  the `data-key` attributes to match up the correct audio clip with the div element,
  and we can use the `playing` class to add that temporary highlight/border.

**Steps:**

1. Add an _event listener_ to the entire window object that is listening for a 
  `keydown` event; the function that we will provide as the _callback_ will be
  defined next. For now, decide on what the function name will be and provide
  that name as the second argument for our event listener.

2. Create a function with the name that you decided on in step 1. 
    - The function should accept one parameter, the _event_ which the function is going to handle. 
    - In the body of the function, declare and define two variables that will 
      reference the  _div and audio elements_ that correspond with the keypress which 
      triggered the function (if such elements exist). 
    - If the key pressed doesn't match one of the keys defined on our page we don't 
      need to proceed any further, but if it does then we can use the variable 
      referencing the `div` element to add the `playing` class to it, and we can use 
      the variable referencing the `audio` element to play the audio file.

3. Declare & define a variable which will reference all the HTML elements on our
  page with a class `key`.

4. Iterate through the HTML elements and add an _event listener_ to each one that will
  fire on the `transitionend` event. Provide another function (name of your choice)
  as the second argument which will be responsible for **removing** the `playing`
  class.

5. Create a function with the name that was used in step 4.
    - This function (event handler) will fire when the `playing` class is added
      to an element and the transition animation completes. The event provided
      to the function as an argument will contain a reference to the `TransitionEvent`
      (or events) with the CSS property's name which was transitioned into.
    - The function should only be concerned with the `transform` property, as this
      property defines the time the animation will take, and we don't want to remove
      the class until the transformation is complete.
    - If there is an event with a property name `transform`, target the _function 
      context_ (the HTML element to which the event listener was attached) and remove
      the `playing` class.



### Bind an event to our keys when they are pressed.

`window.addEventListener('keydown', playSound)`

- `playSound()` is a listener for `keydown` events registered using `window.addEventListener`.
- `window` is the global object in a browser, or the root object of the DOM. And  `document` stands for DOM.

### `data-key` has its own value on `<div>`s and `<audio>`s in HTML

```
const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
const key = document.querySelector(`div[data-key="${e.keyCode}"]`);
```

- `keyCode` property is the *KEY* to connect our buttons(`<div>`s) and sounds(`<audio>`s).
- `keyCode` 's value is same as `ASCII` code (in lowercase letter ), check keycodes [here](http://keycode.info/).
- `data-key` is set for mapping buttons and audios to get the `keyCode`s via `keydown` event.
- the whole `querySelector` expression has to be in back ticks (```).
- `${}` is syntactic sugar for template literals, read more about `Expression interpolation` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

### About playing sounds

How do we prevent delay playing sound, if we keep hitting a key?

just add `audio.currentTime = 0;` before `audio.play();`

### Toggling styles

- use `item.classList.add('className')` to add class when key pressed. (same as `element.addClass('className')` in jQuery)

- use `transitionend` event to remove `play` class. since we want to just remove `transform` property, so add a condition to skip others.

```
if(e.propertyName != 'transform') return;
this.classList.remove('playing'); // `event.target.classList.remove('playing');`
```

### forEach and Arrow function

- `items.forEach()` instead of just `forEach`, which means it's a property of an array.

- Arrow functions is ES6 syntax,

```
keys.forEach(key => key.addEventListener());
```


