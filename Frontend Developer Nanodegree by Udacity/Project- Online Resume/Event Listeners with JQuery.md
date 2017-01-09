# Event Listeners with JQuery

Events give you the power to setup automatic responses based on what the user do on the page.
Everytime we move a mouse, click on a link, submit a form or do anything really the browser makes an anouncement of the action that just took place. In Google Chrome we can use the `monitorEvents` function to see the events as they are taking place. We pass to the function the element on the page that we want to watch for events. This function can only be used in the console on the Chrome DevTools. It will not work from a JS file (will cause reference error).

## Anatomy of a jQuery Event Listener
There are three items we need in order to listen for events and react to them.
* The target element to listen to
* The event we want to react to
* The actions to take in response.
e.g.
```js
/*
Use jQuery to select the input field, next call the on() method. This method
is the primary way jQuery uses to set up event listeners. The first argument to the on method is the event i want to listen to, in this case 'keypress', but could also be 'click', 'change' and 'mouseover' to name a few. Finally we pass a function with the actions that have to happen as a response. This function is called a callback. The callback function is just a regular JS function
*/
$( '#my-input' ).on( 'keypress', function() {
    console.log( 'The event happened!' );
});
```

## The Event Object
Reacting to events often requires knowledge about the event itself, so this is a quick breakdown of the event object which gets passed to an event listener’s callback.

Remember that the target element calls the callback function when the event occurs. When this function is called, jQuery passes an event object to it containing information about the event. This object holds a ton of useful information that can be used in the body of the function. This object, which is usually referenced in JavaScript as `e`, `evt`, or `event`, has several properties that you can use to determine the flow of your code. Try logging the object to see what's available:
```js
$( 'article' ).on( 'click', function( evt ) {
    console.log( evt );
});
```

You should notice a `target` property. The `target` property holds the page element that is the target of the event. This can be extremely useful if an event listener has been set up for a number of elements:
```js
$( 'article' ).on( 'click', function( evt ) {
    $( evt.target ).css( 'background', 'red' );
});
```

In the example above, an event listener is set up for every `article` element on the page. When an article is clicked an object containing information about the event is passed to the callback. The `evt.target` property can be used to access just the clicked on element! jQuery is used to select just that one element from the DOM and update its background to red.

The event object also comes in handy when you want to prevent the default action that the browser would perform. For example, setting up a `click` event listener on an anchor link:
```js
$( '#myAnchor' ).on( 'click', function( evt ) {
    console.log( 'You clicked a link!' );
});
```

Clicking on the `#myAnchor` link will log the message to the console, but it will also navigate to that element's `href` attribute - potentially redirecting to a new page. The event object can be used to prevent the default action:
```js
$( '#myAnchor' ).on( 'click', function( evt ) {
    evt.preventDefault();
    console.log( 'You clicked a link!' );
});
```

In the code above, the `evt.preventDefault();` line instructs the browser not to perform the default action.

Other uses include:

* `event.keyCode` to learn what key was pressed - invaluable if you need to listen for a specific key
* `event.pageX` and `event.pageY` to know where on the page the click occurred - helpful for analytics tracking
* `event.type` to find what event happened - useful if listening to a target for multiple events

The event object can be an incredibly useful tool! Learn more by checking out:
* [jQuery's Event Object](https://api.jquery.com/category/events/event-object/)
* [event.target property](https://api.jquery.com/event.target/)
* [DOM Level 3 Events](http://www.w3.org/TR/DOM-Level-3-Events/)

## Convenience Methods
So far we have been setting up event listeners using jQuery's on method. jQuery also has some convenience methods: 
```js
$( '#my-input' ).on('keypress', function() {
    console.log( 'The event happened!' );
});
// or using the convenience method
$( '#my-input' ).keypress(function() {
    console.log( 'The event happened!' );
});
```

Not every event has a convenience method, we can see the methods from the following page: http://api.jquery.com/category/events/

## Event Delegation
In addition to the jQuery `.on()` method and the convenience methods we have seen before (that were just shortcuts to the `.on()` method) there is another way to set up event listener with jQuery's `on()` method. In Event Delegation jQuery watches one parent element for events on any of it's children.

The jQuery event listener examples we've been looking at so far select the target item(s) using jQuery and then attach an event listener to that target directly. But what about when the target doesn't exist yet? This can happen in a lot of situations. For example, if you have a list of items, and you want to listen to clicks on any of them, what happens if you add an extra list item after your page is done?

Be careful when setting up an event listener and then creating the target item afterwards. For example:
```js
$( 'article' ).on( 'click', function() {
    $( 'body' ).addClass( 'selected' );   
});

$( 'body' ).append( '<article> <h1>Appended Article</h1> <p>Content for the new article </p> </article>' );
```

Clicking on the "appended" article will not add a class to the body because the "appended" article was created after the event listeners were set up. When we targeted the 'article', it didn't exist yet, so jQuery added the click listener to all ZERO of our articles!

But there is a way to make this scenario work by using Event Delegation. We'll listen to events that hit a parent element, and pay attention to the target of those events. Event Delegation with jQuery uses the same code we've been using, but passes an additional argument to the "on" method.
```js
$( '.container' ).on( 'click', 'article', function() { … });
```

...this code tells jQuery to watch the `.container` element for clicks, and then if there are any, check if the click event's target is an `article` element.

Another advantage in using Event Delegation is that you can use it to consolidate the number of event listeners. For example, what if you had 1,000 list items on a page:
```html
<ul id="rooms">
    <li>Room 1</li>
    <li>Room 2</li>
            .
            .
            .
    <li>Room 999</li>
    <li>Room 1000</li>
</ul>
```

The following code would set up an event listener for each 1,000 event listeners - one for each list item...that's 1,000 event listeners!
```js
$( '#rooms li' ).on( 'click', function() {
    ...
});
```

Alternatively, we can use jQuery's event delegation to set the event listener on just one element (the ul#rooms) and check if the target element is a list item;
```js
$( '#rooms' ).on( 'click', 'li', function() {
    ...
});
```

To find out more, check out jQuery's page on [Event Delegation](https://learn.jquery.com/events/event-delegation/).