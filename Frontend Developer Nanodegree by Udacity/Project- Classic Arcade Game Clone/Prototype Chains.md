# Prototype Chains

Prototype chains are a mechanism for making objects that resemble other objects. When we want two objects to have all the same properties, either to save memory or to avoid code duplication, we may decide to copy every property over from one object to another. But as an alternative, javascript provides the option of prototype chains. This makes one object behave like it has all the proeprties of the other object, by delegating the failed lookups from the first object to the second one.

```js

var gold = {a:1};
console.log(gold.a); //1

// 1-TIME PROPERTY COPYING
/*
 There are helper functions that can help copy all of the properties from one
 object onto another, we can do the same thing with a simple for loop
*/
var blue = extend({}, gold);
console.log(blue.a) //1

/*
This copies all the properties from gold to blue, although the effect of this copying operation on the blue object will last indefinately, it is important to remember that the copying happens just at one moment during the program's execution. it is not an ongoing copying back and forth behavior that keeps the two objects in sync.
*/ 


// ONGOING LOOKUP-TIME DELEGATION
/* 
Lets make another replica of the gold object, this time using a different strategy to achieve taht similarity. Rather the copying the properties one by one, the idea would be to give the rose object some linkage to the gold object. Such that whenever a requested property can't be found on rose, it uses gold as sort of a fallback source of properties. The `Object.create()` function can create objects that have this delegation lookup feature. 
*/
var rose = Object.create(gold);
console.log(rose.a);

/*
The apparent similarity between rose and gold is achieved at the very moment of lookup and not as a result of some earlier copying process. The main differnce between 1-TIME PROPERTY COPYING and ONGOING LOOKUP-TIME DELEGATION relates to what moment we would expect the value present on gold to influence either blue or rose objects. With blue it is a single moment of copying, while with rose it happens at every look up event 
*/
```

## The Object prototype
There is a top level object that every javascript object eventually delegates to. This is where all the basic methods are provided for all objects. We call it the object prototype because it provides the shared properties of all objects in the entire system. 

### Constructor property
The `.constructor()` property of the object prototype makes it easy to tell what function was used to create a certain object. Object prototype has other useful properties too such as the `.toString()` property.

### Array Prototype
Without any special steps most new objects that are created will delegate to the Object prototype, but some special objects that we make in JS have other features above and beyond the basic characteristics of all objects. Arrays, for example, have methods like `.indexOf` and `.slice`. These array methods are stored in another prototype called the Array prototype. The Array prototype then itself delegates to the Object prototype.

