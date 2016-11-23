# Rethinking Testing

We should identify the expectations for the function we are testing

```js
function add(a, b) {
    return a + b;
}

add(2, 3) //5
// So for example just by doing this we have set expectations, for example we 
// expect that the function add exists by the time this test runs which may not
// be trivial if the function is in a different file

add(2, 3) //6
// this would mean that the function was wrong and we need to modify it's body 

add('2', '3') //'23'
// we had expectation to only add numbers but is JS the + operator can also 
// concotonate strings 
```

It is important to identify and test as many expectations as possible during testing.