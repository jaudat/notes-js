# Images with Markup

The reality of mobile networking means that the number of file requests can be just as significant as the size of requests. Therefore we should seek to limit the number of image requests and not just the size of image requests. This problem is called **latency** which is the delay between request and response. Every one of these delays can vary significantly and unpredictably. This is because data can't travel faster then the speed of light. Performance expert Ilya Grigorik calls latency the new bottleneck. For many modern web pages bandwidth doesn't matter as much as latency does. [For more info check instructor notes under the video for performance](https://classroom.udacity.com/nanodegrees/nd001/parts/0011345404/modules/273669854375462/lessons/3483659506/concepts/35594095550923). 

Performance is a very important part of responsive design, this can be done through either reducing file size or the number of requests. One of the best ways to do this is to use graphic effects without image files.

## Test as a Graphic
In the old days when we had text over an image we would save the text as a graphic too. We should avoid using Text as a graphic as it has a number of negatives such as:
* When zooming the the text becomes blurry if saved as a raster image
* Text as a graphic cannot be found by search engines
* Text as graphic is not accessible by screen readers and other assistive technology.
* Cannot select text as graphic either. 

Therefore it is preferable to overlay real text over the top of the image. It has the following advantages: 
* Now both the photo and text will scale much better.
* The text can be selected (highlighted). 
* Also it is easier to add effects to the text using CSS
* File size is also smaller as we can save the photo as a nice small JPEG rather than having to use PNG to cope with text. 

Sometimes the best image is no image at all. Think about whether we actually need an image. One of the ways to ensure the website is really responsive is to not have any images at all, but instead rely on beautiful typography, css effects and great layout to achieve really striking graphical effects, with responsive layouts and really fast load times. There are other sites that use images really sparingly. 

In the past, typography on the web was extremely limited by the number and quality of fonts available. With web fonts and modern CSS implementations, thats all changed. We can now have beautiful typography without resorting to graphics.

## CSS Techniques instead of Graphics
As well as adjusting type attributes, [CSS can also be used for other graphical effects](http://udacity.github.io/responsive-images/examples/2-04/divWithCssEffects), especially using CSS for shadows is supported by all modern browsers and much better then using image hacks. Likewise for rounded corners, gradients and animations. There is however a processing and rendering [cost](http://smashingmagazine.com/2013/04/03/build-fast-loading-mobile-website) to using CSS shadows, rounded corners, and so on. And this is even more significant on mobile. So if needed use CSS for the effects and use it sparingly.

CSS also supports background images and this feature can be used to achieve a number of responsive effects. 
* We can use CSS techniques to add a background pattern to element or to the page itself. We can combine these with gradients and other CSS effects. 
* With `background-size: cover;` we can use CSS to add a background image that resizes without squashing or stretching. This is useful for responsive design when we do not know the size of the viewport. 
* The details of a photo can be lost when it is shrunk too much so we can change the phot when the width is less then a particular size. 
* We can use CSS background images for conditional displays of images only when the viewport size is big enough for example. Although there is a much less hacky and much more efficient way to acomplish alternative image loading with the `srcset` and `picture` element.
* We can use the Image Set function of CSS to choose the background image depending on the screen resolution.
* Examples of above:
    - [Div with background image](http://udacity.github.io/responsive-images/examples/2-06/divWithBackgroundImage)
    - [CSS background-size: cover](http://udacity.github.io/responsive-images/examples/2-06/backgroundSizeCover)
    - [Body with background image](http://udacity.github.io/responsive-images/examples/2-06/bodyWithBackgroundImage)
    - [Body with background image and gradient](http://udacity.github.io/responsive-images/examples/2-06/bodyWithBackgroundImageAndGradient)
    - [Body with elaborate background using only CSS](http://udacity.github.io/responsive-images/examples/2-06/bodyWithElaboratePatternPureCSS)
    - [Using CSS background images for conditional image display](http://udacity.github.io/responsive-images/examples/2-06/backgroundImageConditional)
    - [Using CSS background images for alternative images](http://udacity.github.io/responsive-images/examples/2-06/backgroundImageAlternative)
    - [image-set()](http://udacity.github.io/responsive-images/examples/2-06/imageSet)

## Symbol Characters
Another way to keep the site responsive and avoid image files is that whenever we need to use a graphical symbol such as an arrow, star, or heart we can see if it is available as a character on a font. When symbols and icons are achieved using fonts they have all the response advantages of text. I.e.:
* They are infinately scalable
* Amenable to Texts, CSS Effects
* Don't incur extra download.

The Unicode standard defines the universal character set. Over 100,000 characters have been defined so far. Some fonts support many thousands of these.

* [Unicode character sets](http://unicode-table.com/en/sets/)
* [List of Unicode characters](http://en.wikipedia.org/wiki/List_of_Unicode_characters)
* [Here's a big list of unicode characters](http://unicode-table.com/)
* [More on meta tag charsets](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)

*HINT: When using unicode code to get the unicode character, we can just copy that special character back to the source file in place of the unicode code, this is actually recommended as it makes it easier to read and maintain.*

## Icon Fonts
It is possible to build a font familt made of images and icons instead of letters. Icon Fonts provide a fantastic option for the icons and images that often decorate a websites. They have a number of advantages over plain old images, they are vector graphics that can be infinately scaled and an entire set of images can be downloaded in one font. This makes them a great potential solution for responsive designs where we require minimal downloads and maximum scalability.

Icon Fonts: 
* [Zocial](http://zocial.smcllns.com/)
* [Font Awesome](http://fortawesome.github.io/Font-Awesome/)
* [We Love Icon Fonts!](http://weloveiconfonts.com/)
* [Icon fonts on CSS-Tricks](https://css-tricks.com/examples/IconFont/)
* [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)

## Inlining Images with SVG and Data URIs
If we really want to reduce the number of file requests a page makes, we can inline the images using code. Two ways to do that is by using SVG or Data URIs. Inline SVG has great support amongst desktop browsers and mobile phones, and optimization tools can significantly reduce its size. Data URIs are also well supported on desktop and mobile browsers. Data URIs and SVGs can also be inlined in CSS.  


Examples:
* [SVG Data URI in HTML](http://udacity.github.io/responsive-images/examples/2-11/svgDataUri)
* [SVG Data URI in CSS](http://udacity.github.io/responsive-images/examples/2-11/svgDataUriCss)
* [SVG text on a path](http://udacity.github.io/responsive-images/examples/2-11/svgTextOnAPath)
* [SVG optimized and unoptimized](http://udacity.github.io/responsive-images/examples/2-11/svgUnoptimisedAndOptimised)

Read more:
* [Browser support for inline SVG](http://caniuse.com/#feat=svg-html5)
* [Browser support for Data URIs](http://caniuse.com/datauri)
* [SVG Optimiser](http://petercollingridge.appspot.com/svg-optimiser)
* [Trajan's Column SVG example](http://upload.wikimedia.org/wikipedia/commons/6/6c/Trajans-Column-lower-animated.svg)
* [20 examples of SVG that will make your jaw drop](http://www.creativebloq.com/design/examples-svg-7112785)
* [SVG animation examples](http://codepen.io/chrisgannon/)

