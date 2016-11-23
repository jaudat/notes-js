# Writing Test Suites

**We will be using Jasmine as our testing framework.**

The results are outputted in a formatted HTML file. 

Example code:
```js

describe("Player", function() {
  var player;
  var song;

  beforeEach(function() {
    player = new Player();
    song = new Song();
  });

  it("should be able to play a Song", function() {
    // if all the expects passes then this specification returns true
    //otherwise if one of the expects fails then the spec also fails.
    player.play(song);
    expect(player.currentlyPlayingSong).toEqual(song);

    // demonstrates use of custom matcher
    expect(player).toBePlaying(song);

    describe("when song has been paused", function() {
      beforeEach(function() {
        player = new Player();
        song = new Song();
      });
    });
        // more code continues

  });
});

```

**KEYWORDS**

* `describe` an `it` are used to create an outline. They are just used to organise information
    - `it`: Specification(a container for a test), bunch of statements and expressions that test for specific thing. Are shown in HTML page in green.
    - `describe`: Suites(a group of related specs). Are shown in HTML page in black. It is an organizational tool, a level of indentation.
* `expect(...)`: is the launching point of any test, it is what starts the process. Every test begins with an `expect(...)`, which accepts a single argument called the *actual* that we want to test. 
* `toBe(...)`: Is called the matcher. It indicates what kind of comparison method we want to use against the *actual*. The matcher takes a single argument which is the expected value of the *actual*.  We can also use negation by chaining not to the matcher e.g. `expect(add(2,3)).not.toBe(9);`


## Red-Green-Refactor Cycle
It is recommended to write the tests first (Test Driven Development). It is also known as the Red Green Refactor cycle, as first we write the test that will obviously fail. then we right the code that will make it pass hence Green. Now we can safely refactor our code as we add new features.

## Testing Asynchronous Code
e.g.: 
```js
describe('Initial Entries', function() {
    /* Write a test that ensures when the loadFeed
     * function is called and completes its work, there is at least
     * a single .entry element within the .feed container.
     * Remember, loadFeed() is asynchronous so this test will require
     * the use of Jasmine's beforeEach and asynchronous done() function.
     */

     beforeEach(function(done) {
        loadFeed(0, function() {
            done();
        });
        // Since the callback function (the second argument of loadFeed) 
        // contains only one call to done(), we can shorten the code to the 
        // following:
        // loadFeed(0, done);
     });

     it('are not 0', function() {
        expect($(".feed .entry").length).toBeTruthy();
     });
     // The meaning of done() is something like this: When you pass done to a 
     // function as an argument, jasmine will not run the rest of the code 
     // until you call done()
     // So when you call an async function (usually in beforeEach) that needs 
     // time to finish its jobs, it's a good tool to defer the execution of 
     // the remaining tests. Since the function above doesn't have any async 
     // calls, you can simply remove it and your test will still pass (but 
     // don't forget to remove from the function argument list too, because if // you don't, jasmine will wait for your call to it!).

     it('should be first', function(done) {
        makeAsyncCall(done);
     });
     // Since the function above also makes another async call we use done. 
});
```