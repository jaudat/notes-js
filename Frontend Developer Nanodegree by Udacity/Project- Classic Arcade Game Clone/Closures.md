# Closures

Put simply, every function should have access to all the variables from all the scopes that surround it. A closure is any function that somehow remains available after those outer scopes have returned. 
 
```js
var hero = aHero();
var newSaga = function() {
    var foil = aFoil();
    var saga = function() {
        var deed = aDeed();
        log( hero+deed+foil );
    };
    saga();
    saga();
}
newSaga();
newSaga();
```

For the above code if we wanted to retain access to the `saga` function objects after the `newSaga` function calls that created them had returned, we could do the following:
i) passing to setTimeout.
ii) returning `saga` from `newSaga`.
iii) saving saga to global variable. E.G.
```js
var sagas = []
var hero = aHero();
var newSaga = function() {
    var foil = aFoil();
    var sagas.push( function() {
        var deed = aDeed();
        log( hero+deed+foil );
    });
    // The `saga()` calls that were here can now be moved outside this function
}
newSaga();
// The original `saga()` calls moved outside the function to here 
sagas[0]();  
sagas[0]();

newSaga(); 
// The original `saga()` calls moved outside the function to here 
sagas[1]();
sagas[1]();

// In the cases above, even though the sagas array is stored in the outer
// context, when the function object that is stored in the array is called by
// the following code: `sagas[0]();` it will run in the execution context
// within the `newSaga()` function and not the outer global context. An easy
// way to think about this is that the function object that is run by calling
// `sagas[0]();`must have access to the foil variable which is stored in the 
// `newSaga()` functions context.
```

Therefore we can say a new function always gets created in the same context as its function was defined within.

## Using Closures in our code
This can help improve our code because anytime there is a function with an input parameter that is quite static (i.e. the parameter won't take on a new value the function is called) that is an opportunity to refactor the code such that we store the value in a variable from the outer scope. Because of the way closures work, the inner function will always have access to the outerscope variable even after the outer functions returns.