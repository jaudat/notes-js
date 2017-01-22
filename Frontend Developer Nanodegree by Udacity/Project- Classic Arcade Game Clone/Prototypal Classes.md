# Prototypal Classes

Instead of using extend to copy all the methods over like in Functional Classes we may be able to use prototype objects to store all the shared methods and make our instances delegate to the shared prototype object.

```js
var Car = function(loc) {
    var obj = Object.create(Car.methods);
    obj.loc = loc;
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

The steps for making the class in this prototypal pattern are pretty clear. All we need is a function that allows us to make instances, a line in that function that generates the new instance object, a delegation from the new object to some prototype object, and some logic for augmenting the object with properties that make it unique from all the other objects of the same class. Since this pattern is so common, the language designer decided to add official conventions to support it. In the example code above we are making a `methods` object and adding it as a property of the `Car` function object, if we plan to use prototypes for all our classes, we will probably be doing this every time. Since building a holder object for methods and attaching it as a property to the constructor function is so common the language does this for us automatically. Whenever a function is created, it will have an object attached to it, that you can use as a container for methods just in case you plan on using that function, to build instances of a class. Although we had chosen to store our handy method container object at `key.methods`, the default object that comes with every function, is stored at the `key.prototype`. 

```js
var Car = function(loc) {
    var obj = Object.create(Car.prototype);
    obj.loc = loc;
    return obj;
};

// Since functions are just specialised objects, they can have properties 
// stored in them too
Car.prototype.move = function() {
    this.loc++;  
};
```

People tend get very tripped out at this point because they tend to imagine that the `.prototype` property has some special rules about it that will influence how this code ought to work. But in short nothing interesting has changed.

We left one thing out when we said that the dot prototype objects that come for free on functions were just as uninteresting as any other new object that we could use to store methods. In fact, these prototype objects come with one extra conveniance property that almost no other object has. Every `.prototype` object has a `.constructor` property which points back to the function it came attached to. Thus, there is a mutual linking between any new function and it's companion `.prototype` object. In the example above `Car` links to the `.prototype` object and prototype links back to the `Car` object. 
```js
console.log(Car.prototype.constructor); //Is Car itself`
console.log(amy.constructor); //Points to Car object
```

The main use for this feature is that we can figure out which constructor function built a certain object. All instances of a class delegate their failed lookups to their prototype, and so they should all report as having the same constructor function. 

Furthermore the `instanceof` operator works by checking to see if the right operand's `.prototype` object can be found anywhere in the left operand's prototype chain.
```js
console.log(amy instanceof Car); //In this case Car.prototype can be found
                                 //somewhere in amy's prototype chain.
```

