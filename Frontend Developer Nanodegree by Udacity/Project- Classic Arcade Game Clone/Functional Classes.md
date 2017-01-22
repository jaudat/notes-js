# Functional Classes

This is the simplest possible implementation of a **class**, which is a powerful form of functions that can be used to manufacture fleets of similar objects that all conform to roughly the same interface. The functions that produce these fleets of similar objects are called **Constructor Functions**, because their job is to construct the objects that will qualify as members of the class. So to recap, the class is the notion of a category of things that we would like to build and all of the entailed code that supports that category, wheras the constructor is simply the function that you use to produce a new instance of that class. The objects that get returned from these constructor function invocations are called **instances**, instances of the class. So when we call a Constructor Function in order to create an instance, that operation is known as **Instantiating**.

The only difference between a decorator function and a class is that a class builds the object that it is going to augment, whereas a decorator accepts the object it is going to augment as an input.

E.g.
```js
// It is conventional to name a class with a capitalized noun, almost like a 
// proper noun for a category of things. 
var Car = function(loc) {
    var obj = {loc: loc};
    obj.move = function() {
        obj.loc++;  
    };

    return obj;
};

var amy = Car(1);
var ben = Car(9);
```


## Functional Class patterns with shared methods
The following is the functional class pattern with shared methods, or simply the functional shared pattern. 
```js

/* 
The reason to move the `move()` function outside the class was beacuse the line adding `.move()` as a property of `Car` creates a brand new function every time we invoke the Car constructor function (i.e. every instance of the car class has it's own unique version of the move method). It may be preferable to have it be the case that one function object is shared across all `Car` objects to save memory. To avoid having so many duplicate versions of the `.move()` method, we'll need to move that code defines the move method outside the body of the `Car` constructor function. In this way the interpreter will only visit that code one time, and therefore only build one such function to be shared across all the instances, rather than building a new one every time we invoke `Car`.
*/

var Car = function(loc) {
    var obj = {loc: loc};
    obj.move = move

    return obj;
};

var move = function() {
    this.loc++;  
};
```

However we can do better, for example if there are multiple methods and not just `.move()` as was above, then it will become tedious as we will need to add the function definations outside the constructor function, and then point to them in the constructor function (when we decide to assign them over to every instance). So there would be two lists for all the methods. This is problamatic as we can easily find ourselves editing one of the lists and forgetting to edit the other. It would be simpler to iterate over all the relavent methods and then add them automatically in the constructor defination. However since there isn't necessarily a way to iterate over all the variables in a scope, it may be wiser to store all these methods in an object, this way it possible to iterate over the object programmatically and add them all in a single line.

```js
var Car = function(loc) {
    var obj = {loc: loc};
    extend(obj, methods);
    return obj;
};

var methods = {
    move = function() {
        this.loc++;  
    },
    on: function() { /* CODE */ },
    off: function() {/* CODE */ }
};
```

One problem with this code is the name `methods`, doesn't seem to make it very clear that the methods object is affliated in some way with the `Car` class. We can easily confuse it as being a method container for some other class. Furthermore in the current code it is a global variable, which is something we should avoid. Therefore we can add the `methods` as a property of `Car`, this makes the `method` object clearly encapsulated along with the `Car` class function, such that the two of them were bundled up together nice and neat. 

```js
var Car = function(loc) {
    var obj = {loc: loc};
    extend(obj, Car.methods);
    return obj;
};

// Since functions are just specialised objects, they can have properties 
// stored in them too
Car.methods = {
    move = function() {
        this.loc++;  
    },
    on: function() { /* CODE */ },
    off: function() {/* CODE */ }
};

```

A Javascript class is just a function that is capable of creating many similar objects. Anytime a function produces objects that all conform to roughly the same interface of methods and properties, it can be called a function. Although there is controversy on this (basically classes shouldn't be boiled down so simply), and in other languages this is certainly the case but in Javascript this defination provides the most value for us. 