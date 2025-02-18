

# 08 - Fun with HTML5 Canvas

Canvas, HSL, mouse events

### Canvas

[`Canvas`](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) is added in HTML5, the HTML `<canvas>` element can be used to draw graphics via scripting in JavaScript. It's also used by WebGL to draw hardware-accelerated 3D.

- Implementing basic Canvas

  - in HTML

    ```
    <canvas id="draw" width="800" height="800"></canvas>
    ```

  - in JavaScript

    ```
    var canvas = document.getElementById('canvas');
    var ctx = canvas.getContext('2d');
    ```

in our tutorial we use these:

- Properties
  - `ctx.lineCap`: the shape of the stroke, `round` | `butt` | `square`.
  - `ctx.lineJoin`: determines how two connecting segments (of lines, arcs or curves) with non-zero lengths in a shape are joined together), `bevel` | `round` | `miter`.
  - `ctx.lineWidth`: sets the thickness of lines in space units.
  - `ctx.strokeStyle`: specifies the color or style to use for the lines around shapes. The default is `#000` (black).
  - `ctx.fillStyle`: specifies the color or style to use inside shapes. The default is `#000` (black).

- Methods
  - `ctx.beginPath()`: starts a new path by emptying the list of sub-paths. Call this method when you want to create a new path.
  - `ctx.stroke()`: strokes the current or given path with the current stroke style using the non-zero winding rule.
  -`ctx.moveTo()`: moves the starting point of a new sub-path to the (x, y) coordinates.
  -`ctx.lineTo()`: connects the last point in the sub-path to the x, y coordinates with a straight line(but does not actually draw it).

### HSL

[mothereffinghsl.com](http://mothereffinghsl.com/) website shows you the figure of HSL.
The [HSL](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value)(seel the "hsl()" section) is the Hue-saturation-lightness model using the `hsl()` function notation.

- H (hue): is represented as an angle of the color circle.
  - value `0~360`
  - red = 0 = 360
  - green = 120
  - blue = 240
- S (saturation): represented as percentages.
  - value `0~1` or percentages
  - 100% is full saturation
  - 0% is a shade of grey
- L (lightness): represented as percentages.
  - value `0~1` or percentages
  - 100% lightness is white
  - 0% lightness is black
  - 50% lightness is "normal"

```
hsl(0,  100%,50%)    /* red  */
hsl(120,100%,50%)    /* green */
hsl(240,100%,50%)    /* blue */
```

The advantage of HSL over RGB is that it is far more intuitive: you can guess at the colors you want, and then tweak. It is also easier to create sets of matching colors (by keeping the hue the same and varying the lightness/darkness, and saturation).

in our tutorial
- how to implement a rainbow-like gredient color?

```
ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`;
hue++;
if (hue >= 360) {
  hue = 0;
}
```

↑↑↑ just to restore its value when it is more than 360 to 0 to re-accumulate.

### Dealing with Drawing

- register eventListeners

```
let isDrawing = false;

canvas.addEventListener('mousedown', isDrawing = true);        // ready to draw when mouse down
canvas.addEventListener('mousemove', draw);                    // drawing when mouse move
canvas.addEventListener('mouseup', () => isDrawing = false);   // stop drawing when mouse up
canvas.addEventListener('mouseout', () => isDrawing = false);  // stop drawing when mouse out of canvas
```

- defining drawing lines

```
ctx.beginPath();
// start from
ctx.moveTo(lastX, lastY);
// go to
ctx.lineTo(e.offsetX, e.offsetY);
ctx.stroke();

[lastX, lastY] = [e.offsetX, e.offsetY];
```
**[NOTICE]**: `[lastX, lastY] = [e.offsetX, e.offsetY]`
  - it must be at the bottom of "go to" section in the function, or it will have a slight problem occurs.
  - this is in the ES6 syntax to define multiple variables in one statement, it's also equals like:

    ```
    lastX = e.offsetX;
    lastY = e.offsetY;
    ```
  - ↑↑↑ this way called [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) (see "Assignment seperate from declaration" section)
  - example:

    ```
    var a, b;

    [a, b] = [1, 2];
    console.log(a); // 1
    console.log(b); // 2
    ```

- controlling line width of stroke

```
if (ctx.lineWidth >= 50 || ctx.lineWidth <= 1) {
  direction = !direction;
}

if(direction) {
  ctx.lineWidth++;
} else {
  ctx.lineWidth--;
}
```


- drawing on mobile?

try

```
// dealing with touch screen
if (e.type != "mousemove") {
  x = e.changedTouches[0].clientX;
  y = e.changedTouches[0].clientY;
}
```

## Guide

If you look at the provided answer key and compare it with my solution, you'll see that
  they're quite different even though the results are identical. Why did I approach it
  differently? Quite simply, I just don't like Wes' approach to this particular problem.
  I'm not critiquing his code or whatever, it's really just a matter of preference. I'm
  not big on imperative programming and having to baby my programs through every step.
  Too many `if` statements, too many variable declarations, too much code. 

My 'lazy' approach is, in my opinion, easier to read and easier to understand. We start
  by defining an _anonymous function_ that will contain the rest of our application logic.
  By doing so, we are creating a _closure_ which allows us to safely execute our code
  without having to worry about polluting the global namespace (`window` object). Within
  the body of the anonymous function, we'll declare: variable references to the 
  `canvas` HTML element and it's context, variables defined as objects storing
  the previous and current positions of the mouse, and two variables defined
  as a numerical and boolean value for the hue and line width, respectively. 
  We will then declare two functions which will be used as _event handlers_; one which
  will draw on the `canvas`, and one which will update the variables referencing the 
  current & previous mouse positions. All that's left now is to attach event handlers 
  to the `canvas` element for the following events: `'mousemove'`, `'mouseup'`, 
  `'mousedown'`, and `'mouseout'`.

**Steps:**

- Variable declarations & definitions:

  1. Declare & define a variable reference to `canvas` HTMLElement with `width` and
    `height` properties reflecting the _inner window size_.
  2. Declare & define a variable reference to the canvas' `context`, and set
    the necessary property values so that lines end and connect smoothly.
  3. Declare two variables which will hold the current and previous mouse location
    coordinates, and define them both as objects containing `x` and `y` properties
    with a value of `0`.
  4. Declare two variables that will be responsible for maintaining the line
  hue (should be a number value between 0 and 360) and line width (a boolean set
  to `true`).

- Event Handler Function Declarations & Event Listeners

  1. Declare a function which will be responsible for drawing on the canvas:

      - Set the `strokeStyle` property of the _canvas context_ to be an `hsl`
        color property. Ex: ```ctx.strokeStyle = `hsl(${hue}, 100%, 50%)` ```;
      - Begin/reset the current path and move the starting point of the path to the
        `x` and `y` properties of the object referencing the _previous mouse location_.
      - Set the end point of the path as the `x` and `y` properties of the object
        referencing the _current mouse location_.
      - Connect and _create the path between the two points_, and then draw it out.
      - Increment the `hue` value until it reaches `360`, at which point reset it
        and start the process again.
      - Increment the line width as long as the value stays between 1 & 100.
  
  2. Declare a function which will be responsible for updating the _previous & current mouse
  coordinates_.

  3. Attach 4 event listeners to the `canvas` object:

      - On a 'mousemove' event, call the function which will update the mouse coordinate values.
      - On a 'mousedown' event, attach ANOTHER event listener to the 'mousemove' event that
        will call the drawing function.
      - On a 'mouseup' event, **REMOVE** the event listener attached in the previous step.
      - On a 'mouseout' event, repeat the previous step

