#### Nano Hackers Extra Session 2

# Review: Functions

How do we write a function?

```
function greet() {
	alert("Hi, Ricky!");
}
```

How do we call a function?

```
greet();
```

What if we want to greet different people. How do we modify the function?

```
function greet(person) {
	alert("Hi, " + person + "!");
}

greet("Jonah");
```

One new concept: methods. A method is a function that applies to an object like a string. We write the object, a dot, and the method:

```
var name = "Ricky";
var possibleNickname = name.substring(1);
alert(possibleNickname);
```
