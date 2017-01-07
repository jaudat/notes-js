# Full Responsiveness
The image size we would want to serve on a large display size on a big defination tv is not appropriate for a smartwatch. Instead of using techniques such as CSS background images in media queries to display different images for different viewport sizes we should use srcset and picture elements. Using media queries is an attempt ot guess at build time what image file will run best at run time. We are forcing image choice on browser rather than giving the browser information to make the best choice possible. The other problem with media queries is that they only refer to the viewport dimensions, not the actual display size of the image. 

## srcset
We may have images that works fine on low-resolution desktop monitor but would look terrible on high DPI display. We can solve this problem by serving a larger file, but now the problem is that we are serving a larger file to everyone whether they need it or not. On a slow network, the results would be terrible. The problem with `img` src attribute is that we can only have one url that points to one image. Therefore if we want to provide alternative images so that the browser can choose the best option for the viewport size and device capabilities, we should use `srcset`. Here is an example of it in action: 
`<img src="wallaby_1x.jpg" srcset="wallaby_1x.jpg 1x, wallaby_2x.jpg 2x" alt="Wallaby">`
The syntax above simply means that the browser should choose the high resolution wallaby_2x.jpg for a high DPI display or the lower resolution wallaby_1x.jpg otherwise. The 1x, 2x syntax is called the **Pixel Density Descriptor**. The `src` attribute in the above case is a fallback.

Different screens have different pixel densities, i.e. more or less dots of color per square inch, the physical pixels. The more dots per inch the higher resolution the display and the greater the pixel density. Standard laptops and desktops are regarded as being 1x displays, whereas high spec laptops and phones can be 2x or more. In the Chrome DevTools we can use the console and type the following command `window.devicePixelRatio` to get the device's pixel density.

If the browser does not support the srcset attribute, then it just gets ignored. 

Srcset can also be used with width (W) unit. This is useful because for a browser there is a catch-22. When it wants to choose a image the browser needs to know the dimensions of each image, but it cant know that without downloading each image to check. Here is an exampke of it:
`<img src="small.jpg" srcset="small.jpg 500w, medium.jpg 1000w, large.jpg 1500w" alt="Wallaby">`
The `w` unit tells the browser the width of each image. This allows the browser to retrieve the right image based on the screen pixel density and the viewport size. So if we have a browser window size of 500px wide on a 2x Display then an image that is 1000px wide would be adequate for any display size in that window. Therefore we are enabling the browser to make the right choice since at runtime as the browser knows the screen size and the pixel density but not the image size. 

There has been discussions on having an `h` unit similar to the `w` unit above.   
Links:
* [A fun article on srcset (Warning: A little bit of NSFW language)](http://ericportis.com/posts/2014/srcset-sizes/)
* [Device pixel density list](http://pixensity.com/list/phone)
* [More information about working with pixel density](http://www.html5rocks.com/en/mobile/high-dpi/)
* [Working with h units](https://github.com/ResponsiveImagesCG/picture-element/issues/86)
* [Wikipedia wallabies](https://en.wikipedia.org/wiki/Wallaby)

In the above cases since the browser doesn't know anything about the display size of the image it defaults to assuming the image will be the full size of the viewport, and then downloads the image based on the size of the viewport. If the image is not displayed at 100% width of the viewport, this could mean we are still transferring files that are larger then they need to be. The browser can figure out the width to display the image when it parses the CSS, but the browser parses the HTML and starts image preloading before the CSS is parsed. Therefore at that point it does not know anything about image display size. This is where the `sizes` attribute comes in as it tells the browser the sizes at which the image will be displayed. So now while parsing HTML the browser can work out which image file to request. Adding the sizes values to HTML ensures the browser can fetch images as soon as possible, the right image for the right image display size and device capability.
As the browser will now know all of the following:
* screen resolution
* image dimensions (from srcset w values)
* image display size (from sizes value)
*The sizes attribute does not resize the image.*
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
        img {
            transition: width 0.5s;
            /* vw stands for viewport width and indicates the image will be 50% of the total viewport width.*/
            width: 50vw;
        }
        @media screen and (max-width: 250px) {
            img {
                width: 100vw;
            }
        }
    </style>
</head>
<body>
    <img src="small.jpg" srcset="small.jpg 500w, medium.jpg 1000w, large.jpg 1500w" sizes="(max-width: 250px) 100vw, 50vw" alt="Wallaby">
    <!-- In this case as an example if the viewport is 400px wide then the image display size would be 200px wide. If the screen has a 2x display then we need an image that is atleast 400px wide to look ok, since the small is 500px wide that should suffice in this hypothetical situation -->
</body>
</html>
```

*TIP: We can get the source of the image by highlighting the image element in Chrome DevTools and typing the following into the console: `$0.currentSrc`.*

## Picture Elements

Here is an example of a `picture` element. 
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <picture>
        <source srcset="kittens.webp" type="image/webp">
        <source srcset="kittens.jpeg" type="type/jpeg">
        <img src="kittens.jpg" alt="two grey tabby kittens">
    </picture>
</body>
</html>
```

The source elements above provide optional file sources, if the browser can usew the first source it will otherwise it will keep looking by going to the next source etc. Now the browser can choose the source based on the device capabilities. The fallback is the plain old `img` element at the end. This must be included and if the browser does not support the `picture` element, or the formats of the sources then it will use this fallback option. Infact the `img` element is what actually displays the image. The picture element scaffolding so to speak, simply tells the `img` what source to choose.

[Info on WebP format](https://developers.google.com/speed/webp/?csw=1)


We can take this a step further and display a crop of an image or even a different image based on the viewport size. This is called **art direction**. 
Here is an example:
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <picture>
        <source media="(min-width: 650px)" srcset="kitten-large.jpg">
        <source media="(min-width: 465px)" srcset="kitten-medium.jpg">
        <img src="kitten-small.jpg" alt="Cute Kitten">
    </picture>
</body>
</html>
```

In the example below we combine the picture element with the sourceset to specify images for smaller and larger viewports with different images for different pixel densities.
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <picture>
        <source media="(min-width: 650px)" srcset="kitten-large_1x.jpg 1x, kitten-large_2x.jpg 2x">
        <source media="(min-width: 465px)" srcset="kitten-medium_1x.jpg 1x, kitten-medium_2x.jpg 2x">
        <img src="kitten-small.jpg" alt="Cute Kitten">  <!-- fallback -->
    </picture>
</body>
</html>
```

The safari does not support the picture element still, but we can still use it by using the [Picturefill polyfill library](http://scottjehl.github.io/picturefill/) which allows us to use source set and the other new responsive image elements and attributes on browsers that don't yet support them.

## Accessibility
Not all people surfing the web have the ability to see images, for the visually impaired screen readers are essential to make sense of the web. Lynx is an all text browser that we can use to see what the content looks like without images. Although usually these users won't even be reading it but instead they would be hearing it thorugh a screen reader like ChromeVox. Therefore we should look to use alt attibutes responsibly so that the visually impaired could still make sense of the content. Here are some tips to do that:
* Alt tags should be descriptive for important images. 
* Alt tags should be empty for all images that are just decorations
* Alt tags should be set on every image. 