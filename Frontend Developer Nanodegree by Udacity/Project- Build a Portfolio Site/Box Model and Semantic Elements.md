# Sizing Elements

# Boxes
Websites are made up of boxes. Every element is a box. So `<a>, <div>, <span>, <h2>, <p>, <img>` etc... are all boxes. 

They are multiple ways we can display elements, the two main ones are `display: block` and `display: inline`. the`<p>/<div>` tags are an example of the former, while a `<span>` element is an example of the latter. 

* **Block elements** take up as much width as possible, while as little height as possible. This means that the width is *constraint-based*, while the height is *content-based*. If there is no content then the block element has no height.
* **Inline elements** are only as wide as needed, and also only as high as needed. Therefore both the width and the height of inline elements is *content-based*. Also unlike block elements you cannot set the width and height of inline elements, the box will simply takeup the size of the content. Therefore when the content is too big for a single line, the content wraps into different *line box* which are part of the same inline element.
    - **Line Box**: Each linebox has a top and bottom border but the left border only gets drawn at the beginning of the first line box, while the right linebox only gets drawn at the end of the last line box. Line Boxes can also accept margin and padding. When margin is set for the linebox it pushes the elements away horizontally but not vertically. 

## Box Model
A Box Model defines an HTML elements space on the screen. Every Box Model consists of 4 components: 
* **Margin**: Space between this element and any neighboring elements. Most people think of Margin as the space outside of elements
* **Border**: People usually consider the border and whatever is in it as being inside the element.
* **Padding**: Space between content and the border
* **Content**: The actual content of the element, and not just its spacing/border.


## Border Box
The following CSS changes the default way sizes of boxes are calculated `* { box-sizing: border-box }`. The default version is `* { box-sizing: content-box }`. 
For example if we set the width of an element, as well as it's border and padding like so: 
```css
* { 
    width: 800px; 
    padding: 50px; 
    border: 50px; 
}
```
* With Content-Box: the width refers only to the content, therefore only the content is 800px, we then add the padding of 50px and lastly the border of 50px. So the total width of the element is 50px-50px-800px-50px-50px. This is weird because most people think of anything within the border as being within the element, therefore when we set the width to 800px, they would expect that to be the total width of the content+padding+border as those are within the element, but not the margin since most people think of it as being outside the element.
* With Border-Box: The padding and border along with the content are included in the width and height of the element, as most people consider them to also be inside the element. therefore the element above will have the following width of border-padding-content-padding-border, 50px-50px-600px-50px-50px. However there is a catch, since this has been developed fairly recently so if we want to be sure that older browsers can also display it we need to add **browser specific prefixes**. 
```css
 {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    -ms-box-sizing: border-box;

    box-sizing: border-box;
}
```

## Semantic Elements
HTML is a markup language, meaning it describes a document. If we build websites using only divs, then it would be difficult to know what it is describing, and html would be failing its goal. We can use elements that are more descriptive then divs, these are called semantic elements. The following are the benefits of semantic elements:
* Easier to read and understand
* Makes page easier for search engine bots such as the googlebot which improves ranking and also allows google to show cool previews.
* They are also good for accessibility, people with visual impairments use screen readers and semantic elements makes it easier for screen readers to make sense of the page.

## Definations:
* **User Agent Styles**: These are the default styles of elements. They often include the default properties for Box Models.


