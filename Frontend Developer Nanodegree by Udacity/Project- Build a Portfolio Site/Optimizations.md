# Optimizations

**Other things that are important to responsive design include images, tables and typography**

## Images
In order to do responsive design we need to consider images. If the rest of the page is changing based on device characteristics, so should the images. 
Some cases of this are: 
* Using the same image but changing the resolution for example a standard image for laptop but a 2X image for a high DPI display like a Chromebook Pixel or Retina iMac. The best way to do this is to use a `source set` attribute on an `img` tag. With `source set` the browser will choose which file it wants and only download that one.
* If we want to crop images for instance because it doesn't make sense to use show a big wide image on a narrow portrait phone. This is called *Art Direction* and the new `picture` element will be used here. The `picture` element uses media queries to select which image to use

## Tables
A table can overflow a narrow screen if it has more then a few columns, forcing horizontal scrolling. Here are three options to fix this:

### Hidden Columns
This essentially hides columns based on their importance as the viewport gets narrower. The biggest problem with this method is that you are hiding information from the user. If possible use abbriviated data instead of hiding it completely.

### No More Tables
Below a certain viewport width the table is collapsed and resembles a long list instead of a table data. 
```html
<!DOCTYPE html>
<html>
<head>
<title></title>

<style type="text/css">
  table {
    border: 1px solid #ddd;
  }
  tr:nth-child(odd) {
    background-color: #f9f9f9;
  }

  /* Below a certain viewport width it shouldn't display as a table */
  @media screen and (max-width: 500px) {
    table, thead, tbody, th, td, tr {
      display: block;
    }
    /* Hides the table header by positioning them way off screen, we could set `display: none;` but this could affect accessibility for people using screen readers as the browser won't tell them the column headers */
    thead tr {
      position: absolute;
      top: -9999px;
      left: -9999px;
    }
    /* Since the table cells are now displayed as full block elements add some padding and also set the position to relative */
    td { 
      position: relative;
      padding-left: 50%; 
    }
    /* add row labels */
    td:before { 
      position: absolute;
      left: 6px;
      content: attr(data-th);
      font-weight: bold;
    }
    td:first-of-type {
      font-weight: bold;
    }
  }
</style>

</head>
<body>
<table>
  <thead>
    <tr>
      <th>Team</th>
      <th>1st</th>
      <th>2nd</th>
      <th>3rd</th>
      <th>4th</th>
      <th>5th</th>
      <th>6th</th>
      <th>7th</th>
      <th>8th</th>
      <th>9th</th>
      <th>Final</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Team">Toronto</td>
      <td data-th="1st">0</td>
      <td data-th="2nd">0</td>
      <td data-th="3rd">0</td>
      <td data-th="4th">4</td>
      <td data-th="5th">0</td>
      <td data-th="6th">1</td>
      <td data-th="7th">0</td>
      <td data-th="8th">0</td>
      <td data-th="9th">0</td>
      <td data-th="Final">5</td>
    </tr>
    <tr>
      <td data-th="Team">San Francisco</td>
      <td data-th="1st">0</td>
      <td data-th="2nd">0</td>
      <td data-th="3rd">0</td>
      <td data-th="4th">4</td>
      <td data-th="5th">0</td>
      <td data-th="6th">0</td>
      <td data-th="7th">0</td>
      <td data-th="8th">0</td>
      <td data-th="9th">0</td>
      <td data-th="Final">4</td>
    </tr>
  </tbody>
</table>

</body>
</html>
```

### Contained Tables
We can also wrap the table in a `div` and  add the following style to it:
```css
div.contained_table {
    width: 100%;
    overflow-x: auto;
}
```
Then instead of breaking out of the viewport the table will instead take up the same width but will be scrollable within the viewport.

## Typography
If the number of words in a line is too small then it is awkward to read, if the number of words per line is too much then people might get lost when they move to the next line (hard to find beginning of next line) and end up reading the same thing over and over again, or they may get lazy and read the first part of the line but by the end they end up skimming. There has been a lot of research on the ideal **measure** of a line. The research indicates that the ideal is somewhere between 45cpl (characters per line) to 90cpl, depending on the font used, if it is on print, if it is projected, or on a computer screen. For the web there seems to be a pretty solid concensus on **65cpl**.  It is not a hard and fast rule but it is a good place to start.

We need to consider how people read as we create our designs as it affects our layout. That is why we should consider line length when building sites and should take them into consideration when choosing breakpoints. 

Fonts should be big enough to be read on any device. 14px can sometimes not be big enough. Therefore we could look into setting the base font to atleast 16px and atleast a 1.2em line height. On text heavy sites the line height and font size could even be increased.