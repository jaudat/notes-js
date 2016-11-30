# CSS Frameworks, Responsive Layouts


* **Grid Based Design**: Layout of most if not all websites is in grids.
* **Frameworks**: Collections of CSS classes that make page layout easy to implement. 
    - Most if not all css frameworks assume 12 columns, that is because the most common web page layouts have either 2 evenly spaced columns, or 3 evenly spaced columns or 4 evenly spaced columns and this can be done with a 12 based column layout system.
* **Responsive Web Pages**: Screens are of many different sizes. We mean in terms of pixels. We must make sure that the web page looks good in all the various screen resolutions. Therefore the web page isn't fixed to a single size and will look good on any device by responding and resizing correctly to make sure everything looks great and is legible. We can use `max-width` and center it, for large screen resolutions.
* **Adaptive Design**: For devices such as tablets and phones users are using their thumbs, this may require even further changes to the layout of the web page to make it easier to use.
* **Negative Space**: Empty space between elements e.g. between columns so that they are more legible, usually done through margin and padding

## Responsive Design tips
* In order for the website to resize based on the size of the browser, We cannot use pixels to define the width of columns, instead we will have to use percentages
* If we have a lot of content that we want to display in a small area, we can use **overflow** to make sure content in element can scroll by doing the following: `.className { overflow: auto; }`.
* Use **Media Queries** to change the layout based on the device, screen size and color of screen. Here is an example of Media Query:

    ```css
    @media only screen and (max-width: 300px) and (min-width: 100px) {
        /* Do something */
    }
    ```

* CSS Resetting or CSS Normalize, such as Normalize.css are solutions to manage cross browser compatability so that our web page looks same in different browsers and different versions including older browsers. It will make sure that the css styles are interpretted the same across all browsers.

## Resources
* PlaceHolder Images: During mockup and design phase we can use placeholder images on our webpage temporarily, we can look at the following:
    - placehold.it
    - placepuppy.it
    - placekitten.com
* Fonts: We can get a brevy of fonts that we can use for our web page from these resources:
    - google.com/fonts


