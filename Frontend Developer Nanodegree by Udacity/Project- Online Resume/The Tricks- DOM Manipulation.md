# The Tricks: DOM Manipulation

Here is the [jQuery API Documentation](http://api.jquery.com/).

Examples of jQuery usage:
* Toggle class: 
```js
var featured = $('.featured');
featured.toggleClass('featured');
```

* Move to sibling in DOM Tree: using `.next()` method.
* We can use the `.attr()` method to get (if we give it one argument) or set a value of an attribute (if we give it two argument) of an element. 
```js
// Setting attribute to an element
var navList = $('.nav-list');
var firstItem = navList.children().first();
link = firstItem.find('a'); //first item is an li element with a nested a tag
link.attr('href', '#1');
```

* The `.css()` method is similar to the `.attr` method above. It modifies the style attribute of a tag, it may be better to modify the style using CSS rather then jQuery.

TO BE FINISHED, DID NOT WRITE NOTES FOR COMPLETE LESSON.

