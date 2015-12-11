#### Nano Hackers Extra Session 1

# Functions!

> "Well-designed computational systems, like well-designed automobiles or nuclear reactors, are designed in a modular manner, so that the parts can be constructed, replaced, and debugged separately." — Abelson & Sussman, _Structure and Interpretation of Computer Programs_

## Warmup

We've seen functions before. Can anyone give some examples?

`alert`:

```
alert("Oy vey!");
```

`jQuery` aka `$` aka "The money function" (thanks Sean):

```
$("#foo");
```

`parseInt`:

```
parseInt("5");
```

So we've used functions. We have a feeling for what they are. But today we're going to dive deeper. We're going to learn how to write functions and some ways they can help us write better code.

## Names

Functions let us give a name to a block of code. This helps make our programs more readable. For example, contrast these two programs:

```
var x = -5;
var result;

if (x > 0) {
	result = x;
} else {
	result = -x;
}

alert(result);
```

```
function absoluteValue(x) {
	if (x > 0) {
		return x;
	} else {
		return -x;
	}
}

alert(absoluteValue(-5));
```

In the bottom example, it's obvious what we're doing to `x`: we're taking the absolute value! We don't have to decode that gnarly piece of code.

When you're writing more complex programs or working with other people, good naming becomes even more important.

## Reuse

Once we have `absoluteValue`, we can use it anywhere. We never have to write that _if_ statement again.

Suppose, for example, that we had this bit of code to compute the area of a rectangle:

```
var p1 = {x: 0, y: 0};
var p2 = {x: 2, y: 2};

var width = p1.x - p2.x;
var height = p1.y - p2.y;

width = (width > 0) ? width : -width;
height = (height > 0) ? height : -height;

var area = width * height;
```

With `absoluteValue`, the code becomes shorter and more readable:

```
var p1 = {x: 0, y: 0};
var p2 = {x: 2, y: 2};

var area = absoluteValue(p1.x - p2.x) * absoluteValue(p1.y - p2.y);
```

## Abstraction

Do you know how `$` works? Do you need to? If someone told you about `absoluteValue`, without showing you the code, could you use it?

This phenomenon — where you use a function without knowing what's going on inside — is called _abstraction_.

Abstraction lets us think at a higher level. It helps us keep our programs simple by separating different parts so that changes to one part don't affect the others.

Imagine we think of a better way to implement `absoluteValue`:

```
function absoluteValue(x) {
	return (x > 0) ? x : -x;
}
```

Do we have to change `area`?

## JavaScript Syntax

How do we define a function in JavaScript?

```
function <name>(<parameter1>, <parameter2>, ...) {
	<body>
}
```

The `return` statement in the body is optional. If there's no `return` or the `return` isn't followed by a value, the function returns `undefined`.

## Lab 1

1. Write a "number explorer" app. When you type a number, the app displays (1) the square, (2) the absolute value, and (3) whether the number is even or odd. Define a function for each property.
2. Extend the number explorer to display the cube. See if you can modify the "square" function to handle any power.
3. Extend the number explorer to display the next highest square number. For example, for 8, the next highest square number would be 9 (3 * 3). Re-use your existing functions.

---

## Recursion

Does anyone know what factorial is? It's the product of all the numbers 1 to _n_. We write the factorial with an exclamation point after _n_:

```
1! = 1
2! = 1 * 2 = 2
3! = 1 * 2 * 3 = 6
4! = ?
```

Suppose we have _3!_ and we want to find _4!_. How do we do that? We multiply _3!_ by 4.

Can we generalize this? Suppose we have _(n - 1)!_. How do we find _n!_.

Let's write this in code:


```
function factorial(n) {
	return n * factorial(n - 1);
}
```

This function is called _recursive_. In other words, it calls itself. We can use recursive functions to solve problems that can be split into similar sub-problems.

But wait! What happens when we call `factorial`? We need to make sure that it terminates at some point:

```
function factorial(n) {
	if (n === 1) {
		return 1;
	} else {
		return n * factorial(n - 1);
	}
}
```

That's better!

## Lab 2

1. Extend the number explorer to display the factorial
2. Extend it to display the _sum_ of numbers 1 to _n_
3. Extend it to display _Fibonacci(n)_

---

## Aside: First-Class Objects

In programming, a "first-class object" is something that participates in all the basic operations of a programming language. In JavaScript, strings are first-class objects. Numbers are first-class objects. What else?

Functions!

We can assign functions to variables:

```
var greet = function() {
	alert("Hello, World!");
};

greet();
```

Pass them to other functions:

```
function perform(action) {
	action();
}

perform(greet);
```

And even _return_ them from functions:

```
function makeAdder(amount) {
	var adder = function(n) {
		return n + amount;
	}
	
	return adder;
}

var addOne = makeAdder(1);
addOne(1);
```

## Higher-Order Functions

Consider two functions:

```
function doubleElements(array) {
	var i;
	var result = [];
	
	for (i = 0; i < array.length; ++i) {
		result.push(array[i] * 2);		
	}
	
	return result;
}
```

```
function squareElements(array) {
	var i;
	var result = [];
	
	for (i = 0; i < array.length; ++i) {
		result.push(array[i] * array[i]);
	}
	
	return result;
}
```

What do they have in common? They both take an array, loop through it, and do something to each of the elements.

What's wrong with this code? That old scourge—duplication. What's duplicated? The _procedure_ of looping and doing something to each element. Now that we know functions are first-class objects, how might we reduce this duplication?

```
function map(array, operation) {
	var result = [];

	for (i = 0; i < array.length; ++i) {
		result.push(operation(array[i]));
	}
	
	return result;
}

map([1, 2, 3], function(x) { return x * 2 });
map([1, 2, 3], function(x) { return x * x });
```

Functions like this, which take a function as a parameter, are called _higher-order functions_.

## Lab 3/Homework: Mobster

In this homework, you'll create an app called Mobster that lets you view, sort, and filter a database of Minecraft mobs.

#### Setup

Find the `mobs.js` file in the `01-functions` folder of the `jonahb/nano-extra` repo on GitHub. (You can clone the repo or get the file through GitHub in your browser.) This file is a database of the mobs you'll be using in the homework.

#### Part 1

Create a page that displays a list of all the mobs from `mobs.js`. When the page loads, it should call a function called `updateList` loops through the mobs and creates a list item for each one.

#### Part 2

Add a sort button. The handler for the button should call a new function, `sort`, that sorts the mobs by name. Then it should update the list (using `updateList`). If you need help writing `sort`, google "selection sort" or "sort algorithms" (or ask on Slack).

#### Part 3

Add two other sort buttons: One that sorts by health and one that sorts by type (critter or monster). Modify `sort` so that it can sort by any attribute. (Hint: You'll be making `sort` a higher-order function.)

#### Extra

* Make the mob list pretty. There are image URLs in `mobs.js`.
* Add filters so that you can filter the list to just critters or monsters.
* Use positioning and jQuery animation to animate the sort (hard!).