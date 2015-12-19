#### Nano Hackers Extra Session 2

# Canvas

> "Rectangles with rounded corners are everywhere!" — Steve Jobs


A canvas is for drawing! With the HTML `canvas` element, we can draw rectangles, circles, shapes, curves, text — anything you can imagine. It's way easier than drawing with DIVs, and it's fast enough that we can do animation.

## Getting Started

First, create and save a new HTML page, and open it in your browser.

```
<html>
	<body>
	</body>
</html>
```

Now make a canvas with the `canvas` element:

```
<html>
 	<body>
   		<canvas>
   		</canvas>
 	</body>
</html>
```

We don't see anything! Let's give our canvas a border:

```
<html>
 	<body>
   		<canvas style="border: 1px solid lightgray;">
   		</canvas>
 	</body>
</html>
```

Now let's give it a width and a height. We'll also give it an ID so we can get at it from JavaScript later:

```
<html>
 	<body>
 		<canvas id="mainCanvas" width="500" height="500" style="border: 1px solid lightgray;">
 		</canvas>
 	</body>
</html>
```

OK, let's draw. We're going to start with the simplest shape: a rectangle. To draw that we'll need a bit of JavaScript:

```
<html>
 	<body>
   		<canvas id="mainCanvas" width="500" height="500" style="border: 1px solid black;">
   		</canvas>

	    <script>
	    	// What does "mainCanvas" refer to?
	        var canvas = document.getElementById("mainCanvas");

			// A "context" is what we draw on
	        var context = canvas.getContext("2d");

			// The parameters are x, y, width, height
	        context.fillRect(10, 10, 100, 100);
	    </script>
 	</body>
</html>
```

Everyone got it?

## Coordinates

When we graph things, we usually put the origin (0, 0) at the middle. Positive X is to the right and positive Y is up.

Canvas is a little different. (0, 0) is at the top left. Positive X is to the right and positive Y is _down_.

Try moving the rectangle down. To the right?

## Fills and Strokes

When we drew the rectangle, it came out black. Let's change the color:

```
var canvas = document.getElementById("mainCanvas");
var context = canvas.getContext("2d");

// fillStyle is a CSS color string
context.fillStyle = "pink";

context.fillRect(10, 10, 100, 100);
```

What are some other ways we could specify the fill style?

```
"rgb(255, 0, 0)"
"rgba(255, 0, 0, 0.5)"
"#ff0000"
```

We can also draw an unfilled rectangle with `strokeRect` (think of "stroke" as in "brush stroke"):

```
context.strokeRect(0, 0, 100, 100);
```

We set the stroke color with `strokeStyle`:

```
context.strokeStyle = "maroon";
```

And the width with `lineWidth`:

```
context.lineWidth = 5;
```

## Lab 1: Color Palette

1. Use `canvas`, `fillRect`, and `strokeRect` to make a color palette — a 3-by-3 grid of your favorite colors. Generate the grid with 9 `fillRect` calls or, better yet, a loop.

2. Make your palette dynamic. Have the user input a color, and generate 9 colors based on this input.

## Paths

To draw things other than rectangles, we use `paths`. A path is basically a shape that we can stroke or fill.

### Lines

Let's start super simple. Say we want to draw a line. First we _begin_ the path:

```
context.beginPath();
```

Now we move our "pen" to the start of the line:

```
context.moveTo(20, 20);
```

Now we trace the line!

```
context.lineTo(100, 100);
```

But we don't see anything! We have to _stroke_ the path:

```
context.stroke();
```

How would we draw an "x"? A triangle?

### Arcs

What's an arc? Part of an ellipse. And what's an ellipse? An oval, basically. A circle is a kind of ellipse. So if we can draw arcs, we can draw circles and other curved shapes.

Just like lines, arcs are parts of paths:

```
context.beginPath();

// centerX, centerY, radius, startAngle, endAngle
context.arc(100, 100, 50, 0, Math.PI * 2)

context.fill();
```

When you're working with canvas, you specify angles in radians. 360 degrees equals 2 pi radians.

How would we draw Pac-Man?

```
context.beginPath();
context.moveTo(100, 100);
context.arc(100, 100, 50, Math.PI / 8, Math.PI * 15 / 8);
context.closePath();

context.fillStyle = "gold";
context.lineWidth = 5;
context.fill();
context.stroke();
```

### Lab 2: Emoji

1. Use paths to draw your favorite emoji — or invent one!

2. Write a function that creates a circular path. (`stroke` or `fill` will then be used to draw the circle.) For example: `function circle(context, x, y, radius) { ... }`

3. It's 1982 and Steve Jobs is your boss. Write a function to create a rounded- rectangle path. (`stroke` or `fill` will then be used to draw the path.) For example: `function roundRect(context, x, y, width, height, cornerRadius) { ... }`
```

### Homework

1. Create a drawing or animation to share with your fellow nanos.

2. Check out the [Mozilla canvas tutorial]([https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial]).

3. Incorporate some things we didn't cover into your drawing: animation gradients, patterns, text, transforms, Bezier curves, etc.