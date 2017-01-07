# Units, Formats, Enviroments

We must look to optimize our images so that they take up as little bandwidth as possible while still looking great.

With images: 
Total File Size = No. of Pixels * No. of bits it takes to store each pixel
or shorter way to write the same formula is:
Total bits = pixels * bits per pixel.

Therefore we should seek to keep images as small as possible and compression as high as possible to improve performance.

[An average web page makes 56 requests for images](http://httparchive.org/trends.php#bytesImg&reqImg). Every one of those requests has a cost in terms of page loads. Even very small delays in page loading means a significant loss in terms of traffic and revenue. A [study by Google]() shows that a 0.4second to a 0.9second increase in page load meant that traffic and ad revenues were down 20%. Another [study by Amazon](https://news.ycombinator.com/item?id=273900) indicates that every 100ms increase in page load time means a loss of sales of 1% for them. 


## Relative Sizing
* For devices such as laptops and desktop monitors don't assume the window size and screen size is the same. Also don't assume the window size will stay the same. 
* `max-width: 100%;` is a good way to respond gracefully to a change to a larger viewport. For example if we give an image a fixed size then on a smaller window the image gets cropped and on a mobile the image is larger then the screen so we have to scroll horizizontally. If we set the image to a relative size by using a percentage, the image will look great on a phone or a small window but on a large window the image will become pixelated and blurry. One solution to this is to set the image's `max-width` to a 100%. This way the image will resize smoothly but only uptil it's natural width. 

### Calc
This allows us to do simple calculations in CSS values, and is a great way to combine absolute and relative values. So for example if we wanted to display three images side by side with 10px whitespace between them we do the following: 
```css
/* First way of doing this */
img {
    max-width: 426px;
    width: 33.3%;
    margin-right: 10px;
}

/* Another way of doing the same using calc */
img {
    margin-right: 10px;
    max-width: 426px;
    width: calc((100%-20px)/3); /* Combining percentage width with fixed margin */
}

img:last-of-type {
    margin-right: 0; /* Make sure the last/final image does not have a margin to the right */
}
```

## Portrait and Landscape
Desktop Monitors and Laptops are usually used in landscape orientation while phone and tablets are often used in portrait orientation. Even though we can't change the window size on phone and tablets we can switch from portrait to landscape orientation. 

## Less Well Known CSS Units
If we want an image that responsively covers the whole height of the viewport. We could set the img `height` to 100% but this only works if the height of the HTML and body elements is also 100%. An easier way is to do the following `height: 100vh; /* 1 VH unit corresponds to 1% of the viewport height */`, we can also do something similar for the `width` using the VW unit.

If there is a case where we want the image to fit the smaller of the viewport height or width of the viewport then we can use the vmin unit like the following: 
```css
element.style {
    /* VMin unit stands for the viewport minimum and each unit corresponds to 1% of the viewport height or width, whichever is smaller. */
    width: 100vmin;
    height: 100vmin;
}
```

Likewise if there is a case where we want the image to cover the whole viewport but without streatching or squashing we can do the following:
```css
element.style {
    /* VMax stands for viewport maximum and each unit corresponds to 1% of the viewport width or height, whichever is greater */
    width: 100vmax;
    height: 100vmax;
}
```

## Raster and Vector
* Raster Images: Photographs and other images represented as a grid of individual dots of color. Might come from camera, scanner or be created with the HTML canvas element.
* Vector Images: Images such as logos and line art, which can be defined as a set of curves, lines, shapes, fill colors and gradients. Can be created from programs such as Adobe Illustrator and Inkscape or from using a vector format such as SVG. (SVG makes it possible to include responsive vector graphics on a web page). The advantage vector images have over raster images is that the browser can render a vector image of any size. This is because vector formats describe the geometry of the image (how it is constructed from lines, curves, colors and gradients) not individual dots of color.

### File Formats
When using Raster Images, use JPEG for photographiuc images (smaller file size then png) also browsers such as Chrome also support other formats such as WebP (which delivers better performance and features. WebP supports alpha transparency animation along with lossy and lossless compression). If possible use SVG for vector images, and for vector art and solid color graphics, such as logo and line art, use PNG if unable to use SVG. (Do use PNG instead of GIF, more colors, better compression and no licensing issues)

[Image formats](https://litmus.com/blog/png-gif-or-jpeg-which-ones-should-you-use-in-email)
[Image optimization](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization)
[More about WebP](https://developers.google.com/speed/webp/?csw=1)
[Browser support for WebP](http://caniuse.com/#feat=webp)

## Compression, Optimization and Automation
One of the best known tools for batch processign raster and vector images is **ImageMagick**. It enables us to automate pretty much anything we might normally do with a GUI based image editor (such as converting formats or applying filters). For responsive images we can use ImageMagick to automate the process of creating multiple versions of the same image with different sizes or formats. We can incorporate ImageMagick in the build process using Grunt tasks. There are a couple of really useful apps that combine other tools for image optimization. For example, **ImageOptim** enables lossless image optimization using a number of open source tools PngOut, PngCrush, JpegOptim, GifTical and so on. **ImageAlpha** uses PngQuant and other tools for compression. It can reduce 24-bit PNG files by applying lossy compression and converting to PNG-8 plus alpha format. Tools like ImageOptim and ImageAlpha can also be run from the command line and incorporated into the build process.

* ImageMagick:
  - [ImageMagick](http://www.imagemagick.org/script/index.php)
  - [Simple ImageMagick installer for Mac](http://cactuslab.com/imagemagick/)
  - [GraphicsMagick](http://www.graphicsmagick.org/) (a fork of ImageMagick)
* Grunt:
  - Udacity webcast for setting up Grunt: [Front End version](https://classroom.udacity.com/nanodegrees/nd001/parts/0011345404/modules/273669854375463/lessons/5827851013/concepts/58278802730923) and [Full Stack version]().
  - [Getting started with Grunt](http://gruntjs.com/getting-started)
  - [Grunt for People Who Think Things Like Grunt are Weird and Hard](https://24ways.org/2013/grunt-is-not-weird-and-hard/)
  - [Generate multi-resolution images with Grunt](https://addyosmani.com/blog/generate-multi-resolution-images-for-srcset-with-grunt/)
  - [grunt-responsive-images plugin for generating multiple images](https://github.com/andismith/grunt-responsive-images)
  - [grunt-respimg plugin for a responsive image workflow](https://www.npmjs.com/package/grunt-respimg)
* Files used in scripting examples:
  - [convert.sh](http://udacity.github.io/responsive-images/convert.sh) (includes instructions)
  - [Gruntfile.js](http://udacity.github.io/responsive-images/Gruntfile.js) (remove line 7, engine: 'im', on Windows)
  - [Imager.js](https://github.com/BBC-News/Imager.js/): responsive image loading developed for BBC News
* Image processing tools:
  - [ImageOptim](https://imageoptim.com/mac) (Mac only)
  - [Trimage](https://trimage.org/) - Similar to ImageOptim (Windows, Mac, Linux)
  - [ImageAlpha](https://github.com/pornel/ImageAlpha)

### Image Compression
We can use PageSpeed Insights to check if a page from our website has been optimized including all the images in the page. We can use PageSpeed Insight from their website, we could also run it through Chrome DevTools, there is also a PageSpeed Insight API that lets us incorporate the checks into our workflow. There is even a grunt plugin for PageSpeed Insights. 