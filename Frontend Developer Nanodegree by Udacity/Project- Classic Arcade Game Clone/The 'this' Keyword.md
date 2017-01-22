# The 'this' Keyword
Most if not all object oriented language's have a way to dynamically refer to the current object. In javascript we use the parameter `this`. We are very careful to refer to it as the 'parameter `this`', and this is because it behaves almost exactly like a regular parameter with pretty much only two major exceptions. The first is that we don't get to pick the name of the parameter `this`. The second is that we go about binding values to the parameter `this` a bit differently from how we bind values to other parameters.
There is in fact five different ways to do that. We will go over all of them later on.

`this` is an identifier that gets a value bound to it, much like a variable. But instead of identifying the values explicitly in code, `this` gets bound to the correct object automatically. Now mostly, the rules for how the interpreter determines what the correct binding is, resembles the rules for positional function parameters. The differences between positional function parameters and the keyword `this` are designed to support our intuition about which object should be focal when invoking a constructor or method. 

## List of things the parameter `this` won't be bound to
```js
var fn = function(a,b) {
    console.log(this);
};

/*
* We know that in the execution context a function object gets created in 
* memory, containing the defination from the letter f (start of function 
* keyword) all the way to the closing curly braces, some may conclude that the 
* keyword `this` would be bound to that function object, but it is not.
*/ 

/*
* Some people think that an instance of that function is created and the
* parameter `this` refers to that. This is true in certain circumstances, but * it is rare, and for now can be taken as false (generally speaking)
*/

var ob2 = {method: fn};

/*
* Some people think that for the keyword `this` to mean anything it must be in 
* function that is contained within some other object as a property. Therefore 
* this object (between the two curly braces) creates an object in memory and
* maybe that in-memory object where the function is a property would be the 
* thing the parameter `this` is bound to. This is also not the case. The 
* easiest way to reason about this is to say for instance there is another 
* object that also refers to the same function ( i.e. var ob3 = {method: fn}; )
* so now the same function is the property of two seperate objects ob2 and ob3,
* but in such a case the parameter `this` in the fn defination would be forced 
* to choose between ob2 and ob3.
*/



var obj = {
    fn: function(a,b) {
        console.log(this);
    }
};
/* 
* We can take this a step further and claim that the parameter `this` will 
* appear inside a function, and this function must appear inside the curly 
* braces of some object literal, or some other form of definign a function. So 
* that object literal that surrounds the function defination might create an 
* in-memory object and that could be what `this` is bound to. But this is also 
* not correct.
*/

obj.fn(3,4);
/*
* Lastly we know we will eventually call the function, and as we pass values 
* in, to be bound to the positional parameters of the function, we could 
* imagine that the invocation creates a set of bindings, or a scope. This scope
* could be modeled somewhere in memory (the execution context), and that the 
* parameter `this` refers to that in-memory object. However this is also not 
* the case (we do not have memory reference access to the execution context 
* constructs/objects from js code)
*/
```

## List of things the parameter `this` is bound to
As we do not expect the positional parameters of a function to be bound to the same values on every single invocation of the function, the same is true for the parameter `this`. In Javascript, the keyword `this` behaves like a parameter in most of the important ways. So the intutions about how to pass values into a function and how they will get bound to the arguments being passed at call time, will hold true for the parameter `this` as well.

```js

var fn = function(a,b) {
    console.log(this);
};


var obj = {method: fn};
obj.method(3,4);


var obj2 = {
    fn: function(a,b) {
        console.log(this);
    }
};
obj2.fn(3,4);

/*
* When there is a dot the left of a function invocation, meaning it was looked
* up as a peoperty of an object, we can look to the left of the dot to see the 
* object it was looked up upon. The object a function is being looked up upon
* when it is being invoked that object is what the parameter `this` is bound 
* to. This approximate defination will work for 90% of the case. Incase we use
* brackets instead of dot, the concept will remain the same.
*/

fn(3,4);
/* 
* If there is no dot to the left of the function and no object to the left of 
* the dot (i.e. there is no object the function is being looked up upon) then
* `this` would refer to the global object
*/

fn.call(r,g,b);
/*
* By using a function's `.call()` method we get to override the method access 
* rules and therefore overrides the bindings to `this`. We pass in one extra 
* value when using `.call()` and the first parameter of the `.call()` method 
* gets bound to the parameter `this`.
*/

obj.method.call(r,g,b);
/*
* In cases like this the first parameter to the `.call()` method will have
* precedence over the object that is to the left of the dot, and therefore the
* value of r in this example will be bound to `this`. (It overrides method 
* access rules)
*/

setTimeout(fn,1000);
/* 
* Assuming the code for setTimeout is as follows:
* var setTimeout = function(cb, ms) {
*     waitSomehow(ms);
*     cb() //maybe?
* }
* When functions get passed as callbacks, the parameter `this` will be bound
* to the global object as when the function is being invoked there is no object
* to the left of it, so no value is being supplied for `this`, since there is 
* no property access being done to get access to the callback function
*/

setTimeout(obj.method, 1000);
/* 
* Assuming the code for setTimeout is as follows:
* var setTimeout = function(cb, ms) {
*     waitSomehow(ms);
*     cb() //maybe?
* }
* To know what is bound to the parameter `this`, we should not look at the 
* function defination (in this case defination of fn) nor is it where the 
* function is looked up on the object (in this case the obj.method), it is 
* where the function is actually invoked (in this case the cb() line). Since
* the last line of setTimeout is still a free function invocation and not a 
* method invocation with a dot, therefore the parameter `this` is bound to the 
* global object. 
* Only the moment of call time influences how the parameter `this` will get
* bound.
* Despite the fact that we see the object on the left of the dot when we pass
* the function in to setTimeout, this object will not be passed along as the 
* binding for the parameter `this`. When the systen we passed it to (in this 
* case setTimeout) eventually calls the callback function
*/
/*
* Any function that takes another function as callback e.g. setTimeout, may 
* actually call that function differently then you expected. Callback 
* functions are inherently designed so that they will be inherently invoked by 
* the system you pass them to. Thus you generally have very little control 
* over what the bindings will be for the parameters of the functions that you 
* passed in. For this reason you have to be careful of all the parameter 
* bindings including the parameter `this`, whenever you pass a function as an 
* input to another function.
*/


setTimeout(function() {
    obj.method();
    }, 1000)
/* 
* One way to pass a callback without complicating the parameter binding 
* situation is to pass a different function, one that doesn't recieve any 
* parameters at all including the parameter `this`, then you just make room in 
* the function body for custom code where we can reference the method and make 
* the invocation ourselves, passing along whatever bindings we want.
*/ 

console.log(this);
/* 
* The language has historically been set up so that we could refer to `this`
* from the global scope and doing so gives the default binding, which is the 
* global object. This behavior was removed from the more modern specifications 
* of the language.
*/

new obj.method();
/*
* This new keyword will have an effect on how `this` recieves it's binding. The
* parameter `this` will be bound to an entirely new object that creates 
* automatically in the background. In support of certain object oriented 
* paradigms another object will be generated everytime you make a call to  
* obj.method() with the keyword `new` in front of it.
* The positional parameters are unaffected by the keyword new, they get passed 
* along in exactly the same way as they always are
*/

```

`this` behaves almost exactly like an input parameter, but rather then being bound to the value that is supplied between the parenthesis, it is bound to the value that is to the left of the dot at call time.

So the keyword `this` makes it possible for us to build just one function object and use that as a method on a bunch of other objects. And everytime we call the method, it will have access to whatever object it is being called on. This can be very useful for conserving memory 