# Building Up

## Media Queries

There are three ways to apply media queries:

* Add them as an attribute to the link tag. With this we usually have many requests where each request tend to have small file sizes.

    ```html
    <html>
    <head>
        <title></title>
        
        <link rel="stylesheet" type="text/css" href="styles.css">
        
        <!-- Only apply following css when width of screen is over 500px wide -->
        <link rel="stylesheet" type="text/css" href="over500.css" media="screen and (min-width:500px)">
        
    </head>
    
    <body> </body>
    </html>
    ```

* Embed them with an `@media` tag. With this we tend to have fewer requests but the file sizes tend to be big. We will need to compare the performance of this with the linked tag one above.

    ```html
    <html>
    <head>
        <title></title>

        <link rel="stylesheet" type="text/css" href="styles.css">

        <!-- Only apply following css when width of screen is over 500px wide -->
        <style>
            @media screen and (min-width:500px) {
                body { background-color:green; }
            } 
        </style>

    </head>

    <body> </body>
    </html>
    ```

* Embed them with an `@import` tag. FOR PERFORMANCE REASONS DON'T USE THIS METHOD. #PERFMATTERS
    ```html
    <html>
    <head>
        <title></title>

        <link rel="stylesheet" type="text/css" href="styles.css">

        <!-- Only apply following css when width of screen is over 500px wide -->
        <style>
            @import url("over500.css") only screen and (min-width:500px);
        </style>

    </head>
    
    <body> </body>
    </html>
    ```

### Notes
* Also only stick with `screen` and `print` (if users are going to want to print the page). Other media types like `handheld`, `projected` or `embossed` never gained traction and don't do anything.
* The most common media queries used are `min-width` and `max-width`. We can also use `min-device-width` and `max-device-width` but this is strongly discouraged. As `min-width`/`max-width` are based on the size of the browser window while `min-device-width`/`max-device-width` are based on the size of the device screen. Also some browsers like the legacy android browser may report the wrong value for `min-device-width`/`max-device-width`.

## Breakpoint
This is the point at which the page changes layout. 

*"We shouldn't change breakpoints at all. Instead we should find them using our content as guide. - Scott Yale"*

## Complex Media Queries
```html 
<!DOCTYPE html>
<html>
<head>
    <style>
        @media screen and (min-width: 500px) {
            .yes {
                opacity: 1;
            }
            .no {
                opacity: 0;
            }
        } 
    </style>
    <title></title>
</head>
<body>

</body>
</html>
```

```html 
<!DOCTYPE html>
<html>
<head>
    <style>
        @media screen and (min-width: 500px) and (max-width: 600px) {
            .body {background-color: green;}
        }
    </style>
    <title></title>
</head>
<body>

</body>
</html>
```

## Grids
**Grid Fluid System:** Columns end up wrapping to the next line as the browser width starts getting smaller

### Flexbox
In normal block layout each element is positioned underneath the other in a container, but when we change the property of the container to `display: flex`, the elemenets will be shown in a row. This is because the default flex direction is row. Therefore by default the elements fit in a single row. So no matter what the width of the elements are they won't wrap, instead the browser will resize them to fit within the viewport on a single row. 

This behavior can be changed by adding `flex-wrap: wrap` to the container div. Doing this will make the browser wrap the elements to the next line and will only resize the elements if it has too as there are no other options.

We can also change the order of the elements that are in the container, by changing the `order` attribute of each child element of the flexbox container. The default order is the order of their markup. 




 
