# Building the Move Planner App
## Making AJAX calls
It makes sense to use JQuery to handle the ajax requests for us. This will make our code cross browser, as JQuery will handle the differences between different browsers, and different versions of each one. We will use two JQuery methods to make our AJAX requests:
* jQuery.ajax(...)
* jQuery.getJSON(...)

## Error handling for AJAX Calls
We can chain **jQuery.error()** [which has been deprecated] or **jQuery.fail()** to the ajax requests to handle failure

## Cross-Origin Resource Sharing (CORS)
**CORS** is used to work around the *same-origin policy* of browsers. The same-origin policy was implemented by web browsers to prevent malicious scripts from untrusted domains from running on a website. In other words, it ensures sure that scripts from one website can't insert themselves into another. Developers that maintain server-side APIs can enable CORS on their servers to disable the same-origin policy. CORS is a relatively recent feature added to browsers. When certain headers are returned by the the server, the browser will allow the cross-domain request to occur.

For APIs that don't support CORS, you may need to use another method. The other way around the same-origin policy is JSON-P. JSON-P is a unique trick to allow cross-domain requests. Many APIs allow you to provide a callback function name, and they will generate a JavaScript file that passes the data into that function when it gets run in your browser.

## Handling Error with JSONP Requests.
Unfortunately error handling is not built into JSONP. This is a technical limitation but there are workarounds. One of the workarounds is to set a Timeout before making the ajax request, and in the success case of that ajax call we can clear the timeout. This way if the ajax request is not successful we would launch the timeout callback instead. The following is an example of the code in action:

```js
// Load Data from Wikipedia
    var wikiRequestTimeout = setTimeout(function() {
        $wikiElem.text("Failed to get Wikipedia resources");
    }, 8000);

    $.ajax({
        url: 'https://en.wikipedia.org/w/api.php?action=opensearch&search=' + cityStr + '&format=json&callback=wikiCallback',
        dataType: 'jsonp',
        success: function(data) {
            var wikiLabel = data[1];
            var wikiURL = data[3]
            var wikiSize = data[1].length;

            for (var index = 0; index < wikiSize; index++) {
                $wikiElem.append('<li><a href="'+ wikiURL[index] +'">'+ wikiLabel[index] +'</a></li>');
            }

            clearTimeout(wikiRequestTimeout);
        }
    });


//NOTE: As of jQuery 1.8 the success callback is deprecated. Use done instead.
//Coach JohnMav notes that the right way to do this is:

//$.ajax({
//   //stuff
//}).done(function(){
//   //do math
//});
```

## Closures and Event Listeners
### The problem:
Let's say we're making an element for every item in an array. When each is clicked, it should alert its number. The simple approach would be to use a for loop to iterate over the list elements, and when the click happens, alert the value of num as we iterate over each item of the array. Here's an example:

```js
// clear the screen for testing
document.body.innerHTML = '';
document.body.style.background="white";

var nums = [1,2,3];

// Let's loop over the numbers in our array
for (var i = 0; i < nums.length; i++) {

    // This is the number we're on...
    var num = nums[i];

    // We're creating a DOM element for the number
    var elem = document.createElement('div');
    elem.textContent = num;

    // ... and when we click, alert the value of `num`
    elem.addEventListener('click', function() {
        alert(num);
    });

    // finally, let's add this element to the document
    document.body.appendChild(elem);
};
```

If you run this code on any website, it will clear everything and add a bunch of numbers to the page. Try it! Open a new page, open the console, and run the above code. Then click on the numbers and see what gets alerted. Reading the code, we'd expect the numbers to alert their values when we click on them.

But when we test it, all the elements alert the same thing: the last number. But why?

### What's actually happening
Let's cut out the irrelevant code so we can see what's going on. The comments below have changed, and explain what is actually happening.
```js
var nums = [1,2,3];

for (var i = 0; i < nums.length; i++) {

    // This variable keeps changing every time we iterate!
    //  It's first value is 1, then 2, then finally 3.
    var num = nums[i];

    // On click...
    elem.addEventListener('click', function() {

        // ... alert num's value at the moment of the click!
        alert(num);

        // Specifically, we're alerting the num variable 
        // that's defined outside of this inner function.
        // Each of these inner functions are pointing to the
        // same `num` variable... the one that changes on
        // each iteration, and which equals 3 at the end of 
        // the for loop.  Whenever the anonymous function is
        // called on the click event, the function will
        //  reference the same `num` (which now equals 3).

    });

};
```

That's why regardless of which number we click on, they all alert the last value of num.

### How do we fix it?
The solution involves utilizing closures. We're going to create an inner scope to hold the value of num at the exact moment we add the event listener. There are a number of ways to do this -- here's a good one.

Let's simplify the code to just the lines where we add the event listener.
```js
var num = nums[i];

elem.addEventListener('click', function() {

    alert(num);

});
The num variable changes, so we have to somehow connect it to our event listener function. Here's one way of doing it. First take a look at this code, then I'll explain how it works.

elem.addEventListener('click', (function(numCopy) {
    return function() {
        alert(numCopy)
    };
})(num));
```

The bolded part is the outer function. We immediately invoke it by wrapping it in parentheses and calling it right away, passing in num. This method of wrapping an anonymous function in parentheses and calling it right away is called an IIFE (Immediately-Invoked Function Expression, pronounced like "iffy"). This is where the "magical" part happens.

We're passing the value of num into our outer function. Inside that outer function, the value is known as numCopy -- aptly named, since it's a copy of num in that instant. Now it doesn't matter that num changes later down the line. We stored the value of num in numCopy inside our outer function.

Lastly, the outer function returns the inner function to the event listener. Because of the way JavaScript scope works, that inner function has access to numCopy. In the near future, num will increment, but that doesn't matter. The inner function has access to numCopy, which will never change.

Now, when someone clicks, it'll execute the returned inner function, alerting numCopy.

### The Final Version
Here's our original code, but fixed up with our closure trick. Test it out!
```js
// clear the screen for testing
document.body.innerHTML = '';

var nums = [1,2,3];

// Let's loop over the numbers in our array
for (var i = 0; i < nums.length; i++) {

    // This is the number we're on...
    var num = nums[i];

    // We're creating a DOM element for the number
    var elem = document.createElement('div');
    elem.textContent = num;

    // ... and when we click, alert the value of `num`
    elem.addEventListener('click', (function(numCopy) {
        return function() {
            alert(numCopy);
        };
    })(num));

    document.body.appendChild(elem);
};
```

## Model, View and Octopus (MVO)
* View: Contains everything the user sees and interacts with like DOM elements, input elements, buttons etc. This is the user's interface to the application for both inputting and reading data.
* Model: This where all the data is stored. This includes data from the server and the user. 
* Octopus: This is what connects the model and the view, it ties them together. This is what provides the seperation of concerns that we need. It holds things together while still keeping them seperate enough that we can change specific pieces without upsetting the rest of the application.

This comes in many different sub types such as
* Model, View, Controller
* Model, View, ViewModel
* Model, View, Presenter,
* Model, View, * 
