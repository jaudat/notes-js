# Using an Organizational Library

## KnockoutJS

There is nothing magical about KnockoutJS. It is just a javascript function. We can check this by typing the following into the console `console.dir(ko)`. Since in Javascript functions, just like arrays, are objects, the functions can have their own properties. jQuery is similar to where the jQuery method is a function with prebuilt properties. 

Knockout gives us the following properties:

* **ViewModel**: Knockout's ViewModel is similar to the Octopus. It separates the Model and the View

* **Declarative Bindings**: Bindings allow you to connect the View and Model in a direct and simple way.

* **Automatic UI Refresh**: Knockout's will update the View when the Model changes. And with the right declarative bindings, Knockout can update the Model when elements in the View change (such as input elements, checkboxes, etc).

* **Dependency Tracking**: Knockout allows you to create a relationship between parts of the Model, and will automatically update Model data that depends on other Model data when that other Model data changes.

### Declarative Bindings (Used in View)
Here is an example of declarative bindings:
```html
<div class='liveExample'> 
    
    <div>You've clicked <span data-bind='text: numberOfClicks'>&nbsp;</span> times</div>
     
    <button data-bind='click: registerClick, disable: hasClickedTooManyTimes'>Click me</button>
     
    <div data-bind='visible: hasClickedTooManyTimes'>
        That's too many clicks! Please stop before you wear out your fingers.
        <button data-bind='click: resetClicks'>Reset clicks</button>
    </div>
        
</div>
```

```js
var ClickCounterViewModel = function() {
    this.numberOfClicks = ko.observable(0); //more on this below
 
    this.registerClick = function() {
        this.numberOfClicks(this.numberOfClicks() + 1);
    };
 
    this.resetClicks = function() {
        this.numberOfClicks(0);
    };
 
    this.hasClickedTooManyTimes = ko.computed(function() {
        return this.numberOfClicks() >= 3;
    }, this);
};
 
ko.applyBindings(new ClickCounterViewModel());
```

### Observables, ObservableArrays & Computed Observables (How Models are stored)
Knockout stores our models a bit differently. Instead of using a plain old object or simple value, we use a special kind of knockout object to keep track our data. This object is called an observable. observables are just objects with special functions in them. This is how we make an observable

```js
var faveNum = ko.observable(42);
faveNum(); //returns 42
```

The reason we have to use observables is because there is no great cross browser way to run some code when a value updates normally. Since when we use plain variable and update its value, all we have done is changed its value and no code is run. but when it is in a function like `faveNum()` above, it runs code which not only updates the value but also runs special code which notifies anyone using faveNum that the value has changed. So now we can automatically update values in the view or other models that are using `favNum()`.

We also have **ObservableArrays** which are smart enough to know which data has been changed so Knockout only updates that specific view and not the whole view. We can use an ObservableArray like so:
```js
var foo = ko.observableArray([1,2,3]);
```

Since ObservableArray is also a function which in JS is just an object we can give it other properties such as giving it methods like so
```js
foo.push(5);
foo(); //returns [1,2,3,5]
```

We use Computed Observables when we want to compute a value rather then just gettin it. The following is an example:
```js
function AppViewModel() {
    this.firstName = ko.observable('Bob');
    this.lastName = ko.observable('Smith');

    this.fullName = ko.computed(function() {
        return this.firstName() + " " + this.lastName();
        }, this);
}
```


### Move the Model code from within the ViewModel
We can seperate the model from the ViewModel, so that the code is not within the ViewModel, like so:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Cat Clicker</title>
</head>
<body>
    <ul id="cat-list"></ul>
    <div id="cat">

        <h2 id="cat-name" data-bind="text: selectedCat().catName"></h2>
        <h3 id="cat-level" data-bind="text: selectedCat().catLevel"></h3>
        <div id="cat-count" data-bind="text: selectedCat().catClicksCount"></div>
        <img id="cat-img" src="" alt="cute cat" data-bind="attr: {src: selectedCat().catImg}, click: function() {catCounterIncrementer();catLevelManager();}">

    </div>
    <script src="js/lib/knockout-3.2.0.js"></script>
    <script src="js/app.js"></script>
</body>
</html>
```

```js
var Cat = function() {
  this.catName = ko.observable("Catster");
  this.catImg = ko.observable("img/434164568_fea0ad4013_z.jpg");
  this.catClicksCount = ko.observable(0);
  this.catLevel = ko.observable("infant");
}

var MyViewModel = function() {
  this.selectedCat = ko.observable(new Cat());

  this.catCounterIncrementer = function() {
    this.selectedCat().catClicksCount( this.selectedCat().catClicksCount() + 1);
  };

  this.catLevelManager = function() {
    if (this.selectedCat().catClicksCount() < 3) this.selectedCat().catLevel("infant");
    else if (this.selectedCat().catClicksCount() < 12) this.selectedCat().catLevel("master");
    else if (this.selectedCat().catClicksCount() < 20) this.selectedCat().catLevel("teen");
    else if (this.selectedCat().catClicksCount() < 40) this.selectedCat().catLevel("young");
    else if (this.selectedCat().catClicksCount() < 60) this.selectedCat().catLevel("middle aged");
    else this.selectedCat().catLevel("old");
  };

}

ko.applyBindings(new MyViewModel());
```

Since in the above example writing `selectedCat()` over and over again can be tedious, KnockoutJS provides `with` to bind the context so we don't have to do that. So we can have a better looking HTML like so:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Cat Clicker</title>
</head>
<body>
    <ul id="cat-list"></ul>
    <div id="cat" data-bind="with:selectedCat">

        <h2 id="cat-name" data-bind="text: catName"></h2>
        <h3 id="cat-level" data-bind="text: catLevel"></h3>
        <div id="cat-count" data-bind="text: catClicksCount"></div>
        <img id="cat-img" src="" alt="cute cat" data-bind="attr: {src: catImg}, click: function() {$parent.catCounterIncrementer();$parent.catLevelManager();}">

    </div>
    <script src="js/lib/knockout-3.2.0.js"></script>
    <script src="js/app.js"></script>
</body>
</html>
```

```js
var Cat = function() {
  this.catName = ko.observable("Catster");
  this.catImg = ko.observable("img/434164568_fea0ad4013_z.jpg");
  this.catClicksCount = ko.observable(0);
  this.catLevel = ko.observable("infant");
}

// Since we are using the `with` keyword above in the HTML all the nested 
// functions have the scope binded to selectedCat and not the ViewModel

// There are two things we can do:
// 1) Bind this to that so that we are calling from the ViewModels context and 
//    use it, e.g. 
var MyViewModel = function() {
  var that = this;
  this.selectedCat = ko.observable(new Cat());

  this.catCounterIncrementer = function() {
    that.selectedCat().catClicksCount( that.selectedCat().catClicksCount() + 1);
  };

  this.catLevelManager = function() {
    if (that.selectedCat().catClicksCount() < 3) that.selectedCat().catLevel("infant");
    else if (that.selectedCat().catClicksCount() < 12) that.selectedCat().catLevel("master");
    else if (that.selectedCat().catClicksCount() < 20) that.selectedCat().catLevel("teen");
    else if (that.selectedCat().catClicksCount() < 40) that.selectedCat().catLevel("young");
    else if (that.selectedCat().catClicksCount() < 60) that.selectedCat().catLevel("middle aged");
    else that.selectedCat().catLevel("old");
  };

  // 2) or just remove the selectedCat() as we are already in its context
  var MyViewModel = function() {
  var that = this;
  this.selectedCat = ko.observable(new Cat());

  this.catCounterIncrementer = function() {
    this.catClicksCount( this.catClicksCount() + 1);
  };

  this.catLevelManager = function() {
    if (this.catClicksCount() < 3) this.catLevel("infant");
    else if (this.catClicksCount() < 12) this.catLevel("master");
    else if (this.catClicksCount() < 20) this.catLevel("teen");
    else if (this.catClicksCount() < 40) this.catLevel("young");
    else if (this.catClicksCount() < 60) this.catLevel("middle aged");
    else this.catLevel("old");
  };

}

ko.applyBindings(new MyViewModel());
```