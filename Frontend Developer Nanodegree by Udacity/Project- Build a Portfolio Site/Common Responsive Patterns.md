# Common Responsive Design Patterns

* **Column Drop**: When the viewport is at its narrowest each element simply stacks vertically on top of one another (in a single column), as the viewport expands and hits its first breakpoint two elements can be placed side by side, and when the viewport expands even further then three elements can be placed side by side etc. Usually limited to three elements side by side i.e. three columns. Generally once the viewport hits a maximum width the columns also hit a maximum size and we start adding margins to the left and right instead. 
    ```html
    <!DOCTYPE html>
    <html>

    <head>
        <title>Column Drop Layout Example Site</title>
        <style type="text/css">
            .container {
                display: flex;
                flex-wrap: wrap;
            }
            .box {
                width: 100%;
            }
            @media screen and (min-width: 450px) {
                .blue {
                    width: 25%;
                }
                .red {
                    width: 75%;
                }
            }
            @media screen and (min-width: 550px) {
                .blue, .green {
                    width: 25%;
                }
                .red {
                    width: 50%;
                }
            }
            @media screen and (min-width: 800px) {
                .container {
                    width: 800px;
                    margin-left: auto;
                    margin-right: auto;
                }
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="box blue" style="background-color:blue"></div>
            <div class="box red" style="background-color:red"></div>
            <div class="box green" style="background-color:green"></div>
        </div>
    </body>

    </html>
    ```
* **Mostly Fluid**: Similar to Column Drop but it tends to be a bit more grid like with more columns and columns fitting in different ways depending on the viewport width. When the viewport is at its narrowest the layout is stacked vertically in a single column, but as the viewport widens the grid pattern appears. And eventually at its widest viewport margins start appearing on the sides.
* **Layout Shifter**: Content instead of reflowing and dropping below other columns the order of the elements themselves may change after each breakpoint.
* **Off Canvas**: In this layout the content comes from off the screen, this usually occurs when we click the hamburger icon. 
    ```html 
    <!DOCTYPE html>
    <html>
    <head>
        <title></title>
        <style type="text/css">

            html, body, main {
                height: 100%;
                width: 100%;
            }
            /* Style for off canvas nav element */
            nav {
                width: 300px; /* Reasonable width so it doesn't take all the viewport */
                height: 100%;
                position: absolute; /* When the navbar comes it just goes on top of the main content and does not push it by making its width narrower to fit in the viewport */
                transform: translate(-300px, 0); /* Move it off screen */
                transition: transform 0.3s ease; /* So it animates nicely */
            }
            nav.open {
                transform: translate(0, 0);
            }

            /* On viewport size > 600px, the nav is shown always no need for hamburger icon */
            @media screen and (min-width: 600px) {
                nav {
                    position: relative;
                    transform: translate(0,0);
                }
                body {
                    display: flex;
                    flex-flow: row nowrap;
                }
                main {
                    width: auto;
                    flex-grow: 1; /* allows the main element to grow and take the full remaining width of the viewport */
                }
            }

        </style>

    </head>
    <body>
        <nav id="drawer" class="dark_blue" style="background-color:DARKBLUE">
            <h2>Off Canvas</h2>
            <p>Click outside the drawer to close</p>
        </nav>

        <main class="light_blue" style="background-color:LIGHTBLUE">
            <div id="menu">
                &#x2261;
            </div>
            <p>Click on the menu icon to open the drawer</p>
        </main>

        <script type="text/javascript">
              /*
               + Open the drawer when the menu ison is clicked.
               */
              var menu = document.querySelector('#menu');
              var main = document.querySelector('main');
              var drawer = document.querySelector('#drawer');

              menu.addEventListener('click', function(e) {
                drawer.classList.toggle('open');
                e.stopPropagation();
              });
              main.addEventListener('click', function() {
                drawer.classList.remove('open');
              });
        </script>
    </body>
    </html>
    ```
