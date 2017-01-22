# Pseudoclassical Subclasses

While the previous section was on subclassing when we were using functional pattern, here we will look at subclassing when using Pseudoclassical pattern for our base class.

```js

// This is the superclass in the pseudoclassical pattern
var Car = function(loc) {
    this.loc = loc;
};
Car.prototype.move = function() {
    this.loc++;
};

// This is a subclass in the pseudoclassical pattern
var van = function(loc) {
    Car.call(this, loc); 
};
// We need Van.prototype to delegate to Car.prototype, right now it will 
// delegate to Object.prototype and therefore will not have access to move
// method
Van.prototype = Object.create(Car.prototype);
// Since we replaced the default object that comes with a function at 
// key.prototype, we must also add the .constructor property that is at 
// key.property. Right now it incorrectly will point to the Car object as we 
// used it's prototype object when we replaced the default object.
Van.prototype.constructor = Van;
Van.prototype.grab = function() {
    /* CODE */
};

// invoke the constructor functions 
var zed = new Car(3);
zed.move();

var amy = new Van(9);
amy.move();
amy.grab();
```

**There are a lot of gotchas that I haven't mentioned, it is best to look at the video series for this section to see why we do what we do instead of some other workaround, and what can go wrong if use incorrect techniques.** 

