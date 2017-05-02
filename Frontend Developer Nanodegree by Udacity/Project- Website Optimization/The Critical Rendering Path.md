# The Critical Rendering Path

The Critical Rendering Path is the sequence of steps,that the browser goes through to convert the HTML, CSS and Javascript into actual pixels on the screen. If we can optimize that then we can make our pages render faster. 

In the CRP first we grab the **HTML** and start building the **Document Object Model**, we then have to fetch the CSS and build the **CSS Object Model**. We combine those two to build the **RenderTree**. We then have to figure out where everything goes on the page, which is the **Layout** step. And then finally we can paint pixels on the actual screen, which happens in the **Paint** step. Javascript is a big and important piece of the performance puzzle but we first we will talk about how we construct the DOM, CSSOM and then we will talk about how JS affects the actual rendering pipeline.
