# Object Decorator Pattern

When writing software reuse of code is the practice of writing generalised code that can be relied upon to address a variety of different goals. So whenever two parts of the code have similarities, there is an opportunity to factor out the similar aspects of it into some reusable library code so that we don't repeat ourselves in either of the two original places.

## Decorator Functions
When a function takes in an object as it's input and then augments that object with some properties or functionality it is called a decorator. E.G.

```js
// It is common to use adjectives as names for decorators, hence carlike
var carlike = function(obj, loc) {
    obj.loc = loc;
    obj.move = function() {
        obj.loc++;  
    };

    return obj;
};

var amy = carlike({}, 1);
var ben = carlike({}, 9);
```

