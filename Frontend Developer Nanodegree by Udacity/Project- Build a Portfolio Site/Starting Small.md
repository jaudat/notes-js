# Starting Small

When we have trouble with content flowing off the page so we may have to pinch and zoom to see anything on a mobile device the problem is usually with the viewport not being set properly.

**Viewport:** Defines the area of the screen the browser can render content to. On desktops The viewport is only as wide as the browser and not as wide as the screen itself (if the browser is not maximised to take up the full screen size).

## Pixels

Instead of reporting the width in Hardware Pixels, the browser reports the width in Device Independent Pixels (dip).

**Hardware Pixels:** Is the actual number of pixels on the display. When we look at tech specs for the screen it is refering to Hardware Pixels.

**Device Independent Pixels (dip)**: Is a unit of measurement that relates pixels to a real distance. The idea being a dip will take up the same amount of space on a display regardless of the pixel density of the display.

Therefore if the pixels on the width of the screen (i.e. Hardware pixels) are 2560 and the browser reports the pixels (ie. the dips) to be 1280, then that means the screen has a **device pixel ratio (dpr)** of 2.

The viewport width in the above example is the number of dips that is 1280px. The 1280DPIs gets scaled up to 2560 hardware pixels, when the page is rendered on the screen.

## Setting Viewport

If we do not tell the browser that our website was designed to work on a small screen it will assume that you're viewing a big desktop experience and that you want to see all of it. It will then render the page as if the screen was 980 DIPs wide shoe-horning everything into its little display. That content will then be scaled to fit on a screen that is in the case of the Nexus 5 only 360 DIPs wide (A Nexus 5 has 1080px Hardware Pixels, Device Pixel Ratio of 3, and 360 DIP). It will be scaled to less then half in the above example, we will then have to pinch to zoom to be able to use the website. The browser will then try to make the content better by doing **font-boosting**, where the browser tries to guess the important content of the web page and increase its font, so some fonts are large and easy to read while others remain small. 

We can set the viewport by adding the following line within the `<head>` tag
```<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
</head>
<body> </body>
</html>
```

Only use this if the webpage is specifically designed to be responsive otherwise it will make it worse. [Article on MDN about the the viewport](https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag)

`width=device-width`: Instructs the page to match the screens width in DIP. This allows the page to reflow content to match the screen sizes.
`initial-scale=1`: Instructs the browser to establish a one to one (1:1) relationship between DIPs and CSS Pixels. Without this some browsers will keep the page width constant when rotating to landscape mode. The browser feels obliged to zoom in, in order to match the width of the page. They also scale the content rather then allowing it to reflow.

**CSS Pixels**: Is what we work with most of the time. 

## Settign Widths
* Use relative widths rather then fixed widths to prevent elements from accidently overflowing the viewports. Although setting a fixed-width of less then 320px is considered safe since allmost all devices have 320px or larger screen width.
* Since CSS allows the content to overflow its container, so if we don't specify the size and the content (for example image) is larger then the container then it will overflow. To prevent this we can set a catch-all such as the following just to be safe:
    ```css
    img, embed, object, video {
        max-width: 100%;
    }
    ```

## Tap Targets
Anything that a user might touch, tap, click, or try and do input on need to be big enough and easy to hit as well as being spaced so that your not going to accidently hit two at the same time. Since our fingers are about 10 millimeters wide or about half an inch. This is about 40 CSS pixels. We should therefore make the buttons around 48 pixels wide and 48 pixels tall. This makes sure that there is enough room between buttons for people with fat fingers and fat thumbs. Although it is ok to make some tap targets a little smaller although we should atleast leave 40px of room between any two tap targets, to make sure that users don't hit two elements or two buttons at the same time or miss it completly.

Therefore we can add code such as the following to make sure that the tap targets are easy to hit
```css
nav a, button {
    min-width: 48px;
    min-height: 48px;
    <!-- Using width and height alone is not recommended because it will not allow the element to resize if the screen is bigger. -->
}
```


## Responsive Design

Start with the smallest form factor usually a phone, and then move on to the next once that is complete. After each design is finished decide if there is a need for a design for a wider screen. After a point there is no need for a bigger design. The reasons to do this are: 
* Starting small makes us prioritize content. When going from big to small it is too easy to just hide or cut important info. Instead by prioritizing content the key content is always on the page.
* We also code from from small to large. This results in our styles working on any device, even on legacy browsers that don't support media queries
* Also starting from the smallest viewport first makes us consider performance from the beginning #PerfMatters