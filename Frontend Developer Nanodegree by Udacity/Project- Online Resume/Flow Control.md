# Flow Control

## If Statements
The syntax for If Statement in JS is 
```js
if (conditional) {
    doSomething();
} else {
    doSomethingElse();
}
```
Javascript gives us evaluators to compare expressions. Here are a number of them: `<`, `>`, `<=`, `>=`, `===`, `!=`. In Javascript equality can use either three equal signs or two. But the three equal sign is safer. 

###Strict equality (===) vs Loose equality (==)

When you use three equal signs, `===`, no type conversion is done prior to the comparison. If the values are different types, for example, a String and a Number, they can't ever be equal. To return true, the values must be equal and the types must be the same.

Loose equality, `==`, checks to see if the two values are the same type and if not, converts to a common type before the conversion. If the types are already the same, there is no difference between the result of === and ==. When they aren't it can cause unexpected results.

Check the [link to an article on Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_when_to_use_them) to see what values get converted into what.

According to Jacques Favreau, the lead front-end engineer at Udacity, you should never use `==`. It's a frequent source of bugs. In fact, if a Udacity engineer tries to commit code with `==`, it automatically gets rejected.

Though it wasn't mentioned in the video, the same conditions apply for strict inequality (`!==`) and loose inequality (`!=`). Loose inequality is more forgiving than loose equality so you might not see strict inequality as often.

## Loops
Help us repeat a piece of code so long as some condition evaluates to true. Once it doesn't equal true we exit the loop. 

### While Loops
```js
while (condition) {
    doSomething();
}
```

### For Loops
```js
for (initialization; condition; mutator) {
    doSomething();
}
```

### For-In Loops
```js
for (item in object) {
    doSomething();
}

// OR (Since Arrays are just special objects)

for (index in array) {
    doSomething();
}

//It is important to note here that this `item` (this iterator in the list) is actually the index of the item in the object, not the value held at the index in the object.
```

Now that you've learned about for-in loops, it's time to stop using them. No, seriously. for-in loops are considered to be general bad practice when writing JavaScript because it has some inconsistent behavior with arrays and objects. Check out these links for more:

* http://stackoverflow.com/questions/500504/why-is-using-for-in-with-array-iteration-a-bad-idea
* http://stackoverflow.com/questions/4261051/javascript-why-for-in-is-a-bad-practice
* https://websanova.com/blog/javascript/why-javascript-for-in-loops-are-bad

### For-Each Loops
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach

## Functions
```js
var myFunc = function(param1, param2) { //parameters are optional
    // code goes here
}

// Or another way to define a function is the following

function myFunc(param1, param2) { //parameters are optional
    // code goes here
}
```

[Anonymous Functions in JS](http://en.wikipedia.org/wiki/Anonymous_function#JavaScript)

## Encapsulation
Functions in JS are objects. Infact Pretty much everything is an object. 
For more information about the special relationship between Functions and Objects, see [here](http://helephant.com/2008/08/19/functions-are-first-class-objects-in-javascript/).

Therefore we can use dotnotation to encapsulate our functions so that they are within objects. 
