#### Nano Hackers Extra Session 6

# Parallax

> As the Earth orbits around the Sun, relatively near-by stars appear to move with respect to the fixed, very distant stars ...

### Intro

Illustration

- [Tree & Mountain](http://www.eg.bucknell.edu/physics/astronomy/astr101/specials/para-mountain2.gif)

Examples

- [Kittens](http://prinzhorn.github.io/skrollr/examples/classic.html)
- [Main](http://prinzhorn.github.io/skrollr/)
- [SVG](http://prinzhorn.github.io/skrollr/examples/path.html)
- [The Boat](http://www.sbs.com.au/theboat/)
- [Apple Watch](http://www.apple.com/watch/)

Any others?

### Setup

Today we're going to use a JavaScript library called _skrollr_ that makes scroll-based animations easy. Let's get skrollr set up.

Create `setup.html` and make sure there's no errors in the console.

	<html>
	  <body>
	    <script type="text/javascript" src="http://jonahburke.com/skrollr.js"></script>
	    <script type="text/javascript">
	      var s = skrollr.init();
	    </script>
	  </body>
	</html>

### Animation

First let's make a blue box:

	<div style="width: 100px; height: 100px; background-color: rgb(0, 0, 255);"></div>

Does everyone remember RGB colors?

Now, let's change the text color when we scroll down:

	<div id="spacer" style="height: 500px;"></div>
	<div
		style="width: 100px; height: 100px;"
		data-0="background-color:rgb(0, 0, 255);"
		data-500="background-color:rgb(255, 0, 0);">
	</div>

The `data-` attributes are the secret sauce. `data-0` says, "this is the style when the scroll bar is at position 0." Skrollr _interpolates_ the style between `data-0` and `data-200`. This means it fills in the middle values automatically.

Now let's rotate our `div`. For this, we use the CSS `rotate` style. First let's check out `rotate` without skrollr:

	<div style="width: 100px; height: 100px; background-color: rgb(0, 0, 255); transform:rotate(45deg);"></div>

Now what if we want the box to spin when we scroll?

	<div id="spacer" style="height: 500px;"></div>
	<div
		style="width: 100px; height: 100px; background-color: rgb(0, 0, 255);"
		data-0="transform:rotate(0deg);"
		data-500="transform:rotate(360deg);">
	</div>

### Lab 1

See if you can duplicate the effect used in the "skrollr" box in the main example. Use an `img` or a `div`. Play with:

- `opacity` (CSS property)
- `transform-origin` (CSS property)
- `transform` (CSS property) and `rotate` (transform type)

### Scrolling Backgrounds

Let's create the typical parallax effect: a background that scrolls at a different rate from the content.

We'll start with a page with lots of text:

	<html>
		<head>
			<style>
				body {
				  font-size: 24px;
				  font-family: sans-serif;
				}
			</style>
		</head>
		<body>
			Lorem ipsum ...
		</body>
	    <script type="text/javascript" src="http://jonahburke.com/skrollr.js"></script>
	    <script type="text/javascript">
	      var s = skrollr.init();
	    </script>
	</html>

Now let's add a background:

	background:url(bubbles.png) repeat;
	
But notice that the background scrolls with the content. That's no fun. Let's _fix_ the background, i.e. tell it to stay in place:

	background:url(bubbles.png) repeat fixed;

Now we can treat the background just like our `div` above. To set the position of the background, we use the `background-position` CSS property:

	background-position: 0px -10px;

Did you see the background shift up a little bit?

Now, let's animate our `background-position` with skrollr:

	<body data-0="background-position: 0px 0px;" data-5000="background-position: 0px -2500;">
		...
	</body>

### Lab2

Did you notice that the skrollr example actually has different layers of bubbles scrolling at different rates. See if you can duplicate this! Keep in mind that `body` is not the only element that can have `background` and `background-position`.

### SVG Paths

TBD; Create an SVG path of a signature using, e.g., Inkscape and animate drawing of the path with the `stroke-dashoffset` property.