# The Basics: the DOM, $, and Selectors

JQuery.js is a Javascript library. Manipulating the DOM with vanilla JS isn't always easy, so JQuery helps with that. JQuery exists so that you can focus on UX, not browser quirks.

When we type `jQuery` on the console we see that it is just a function(object). Infact the `$` sign is just a pointer to the same JS object saw above.

jQuery returns an array-like object which we call a jQuery Collection. The reason we say it's an array-like object is that it's an object that looks and behaves like an array, but also includes some additional methods.

We can pass a string, function, DOM element to the jQuery function. We can even call methods directly on the jQuery object. E.g.

```js
$(string);
$(func);
$(DOMElement);

$.ajax();
```

## DOM
The DOM is a datastructure. It can best be described as a tree. When writing HTML, we create a collection of nested elements that, in the end, resembles a tree structure. 

## Hosting jQuery
* Local: e.g. `<script src="js/jquery.min.js"></script>`
* [jQuery Official](http://jquery.com/download/#using-jquery-with-a-cdn) 
* Content Delivery Network: e.g Hosted by [Google](https://developers.google.com/speed/libraries/devguide#jquery)

## jQuery Selectors
Any valid CSS selector is also valid in jQuery. 
* **Select by Tags**: `$('tag')` the dollar sign is the jQuery object while the argument inside the braces is called the selector. Passing the string of the tag name will result in a jQuery collection of all the elements of a certain tag. 
* **Select by Classes**: `$('.class')` To select all elements of a class name simply pass the class name with a dot in front of the string to the jQuery object.
* **Select by Ids**: `$('#id')` Unlike tags or classes, IDs are specific to a single element, so we should expect to see a jQuery collection with only one DOM element returned. To select by Id simply put a hashtag or pound sign in front of the ID name inside quotes so that we have a string.