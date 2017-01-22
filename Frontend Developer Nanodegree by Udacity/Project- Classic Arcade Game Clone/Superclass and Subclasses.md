# Superclass and Subclasses

We know a great deal about all three class patterns: functional, prototypal and pseudoclassical. Lets move on to thinking about the slightly more advanced code-sharing techniques of subclassing. In the following example we will explore how to achieve this goal using a functional pattern. 

```js
// Car is a Superclass of Van and Cop
var Car = function(loc) {
    // Code that is same in both Van and Car can be placed here
    // This stops duplicate code in both Van and Cop functions.
    var obj = {loc: loc};
    obj.move = function() {
        obj.loc++;
    };
    return obj;
}

// Van and Cop are Subclasses of Car constructor function
var Van = function(loc){
    var obj = Car(loc);
    obj.grab = function() { /* CODE */ };
    return obj;
}

var Cop = function(loc){
    var obj = Car(loc);
    obj.call = function() { /* CODE */ };
    return obj;
}


// Invoke the constructor functions to create objects
var amy = Van(1);
amy.move();
var ben = Van(9);
ben.move();
var cal = Cop(2);
cal.move();
cal.call();
```

In the example above where we used to have object creation we can simply call the car function now and it will produce an object that can act as the starting point for the rest of the subclass. 