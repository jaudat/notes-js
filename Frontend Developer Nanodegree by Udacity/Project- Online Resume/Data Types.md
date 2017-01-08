# Data Types

`var` declares new variable for all datatypes. It's syntax is the following, var {{variablename}} = {{somevalue}}. Here is an example, `var age = 27;`.
Array, functions and objects also use the same var syntax.

Numbers in JS don't need quotes and there is no need to worry about any of the nuances between floating point numbers and integers, because all numbers are saved as 64 bit floating point.

## Truthy/Falsy 
These values can evaluate to true and false but may not equal true of false, therefore we call these values truthy and falsy.

| Truthy (Evaluate to true) | Falsy (Evaluate to false) |
|---|---|
| true | false |
| non-zero numbers | 0 |
| "Strings of length atleast 1" | "" |
| Objects | Undefined |
| Arrays | Null |
| functions | NaN |

## Arrays
Arrays in Javascript are just like lists. They are zeroth indexed. Here is an example of them: `var myArray = [item1, item2];`. We can also access it's items by the following: `myArray[0];`.

## Object Literal Notations
The reason we can access Array properties such as `myArray.length` is because arrays are actually special kinds of objects in javascript. Objects in Javascript can be declared and defined in different ways, all of which are different from how languages like Python define classes and objects, because there are no classes in javascript. Javascript does not include classes, just objects. There are ways to mimic classes in some respects, but they are still just objects.   

We can define objects with Object Literal Notation, like this: `var myObj = {"name":"Jaudat", age: 27};`. We can now access and set different properties of the object using a different notations. Here are the three:
* Dot Notations: `var myName = myObj.name;` or `myObj.color = "brown";`.
* Bracket Notation: `var myName = myObj[name];` or `myObj[color]="brown";`.
* Function Notation(In JS functions are objects): [Link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)

[Difference between Dot Notation and Bracket Notation](http://www.dev-archive.net/articles/js-dot-notation/)

### JSON
JavaScript Object Notation. JSON is a popular and simple format for storing and transferring nested or hierarchal data. JSON does not permit the inclusion of functions as properties but the syntax is otherwise very similar to javaScript Object Literals.


