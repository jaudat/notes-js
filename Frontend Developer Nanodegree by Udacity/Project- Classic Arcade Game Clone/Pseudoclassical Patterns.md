# Pseudoclassical Patterns

Very closely related to the Prototypal Classes. The pattern is called pseudoclassical because it attempts to resemble the class systems from other languages by adding a thin layer of syntactic conveniences. 

Whenever we choose to use the keyword new in front of a function invocation, our function is going to run in a special mode called Constructor Mode. This mode is basically a way for the interpreter to insert a few lines of operations into our code it knows we are going to need them to be done whenever we are instantiating a new object. It temporarily makes our function run as if there are some extra code at the beginning an end, even though we will never have typed that code. 

```js
// Continuing with our previous example this is how it is written in the 
// Pseudoclassical Pattern
var Car = function(loc) {
    /*
    * Since we use the `new` keyword infront of a function invocation, the 
    * commented out line at the beginning and end of the function are 
    * automatically, we therefore don't need to add the first and last line of 
    * a function like we did with Prototypal Classes
    */



    // this = Object.create(Car.prototype);
    this.loc = loc;
    // return this;
};
Car.prototype.move = function() {
    this.loc++;  
};


var ben = new Car(9); // This is how we invoke a function so that the 
                      // interpretter adds the the lines automatically to the 
                      // function, it is known as the Pseudoclassical Pattern.
```

The Pseudoclassical version is really just a thin layer of syntactic convenience over top of the prototypal pattern. In fact, the primary difference between these two pattern is the number of performance optimizations that the Javascript engines have implemented that only apply when we are using the pseudoclassical pattern.