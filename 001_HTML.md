# HTML

HTML is a markup language and it uses tags to mark words or lines i.e. `<p> This is paragraph <\p>`. We can use [codepen](https://codepen.io/).

### HTML Tags

- `<h1> This tag is used for main heading </h1>`. We have few heading numbered from 1 to 6. i.e. `h2, h3, .. h6`. We can get more information on heading on [this](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements) mozilla devlopers website. It contains almost all the information about heading tags we use in web. **One can also visit [w3schools](https://www.w3schools.com/html/html_headings.asp)** to learn more about html tags. This also contains some common practices and **do's and don't**  which can be very helpful. 

  - Using `id` with heading tags can be useful for navigating. `<h1 id="My Id">Information</h1>`.

  - Using `style` attribute to set font size property using css property `<h1 style="font-size:30px;"> Heading 1 </h1>`

- One can visit an organised website like [devdocs](https://devdocs.io/) to get information about a particular tag or element.
- `<br>` is used to create break a line or create spacing among different elements. It doesn't need a closing tag.
- `<hr>` tag is used to create a horizontal row and this is also a self-closing tag.
  - We can use **size** to set the thickness of horizontal line i.e. `<hr size="3">`. 
  - [Here](https://devdocs.io/html/element/hr) we can get more info about hr tag.
- wrap the content which we want to center in the `<center>` tag and close when required content is over with `</center>`.



### HTML Boilerplate

```html
<!DOCTYPE html>
<html>
    <!-- head tag doesn't contain any content of webpage
		 it only contains thigs which are useful for rendring
		 of the webpage -->
    <head>
        <meta charset="UTF-8">
        <!-- title tag is used to show the title of the tab 
			 current webpage -->
        <title>My website</title>
    </head>
    
    <!-- body tag contains most of the structure of the webpage 
		 like images, links, paragraphs etc. -->
    <body>
        
    </body>
</html>
```

This is the basic structure of a website and most of the rendring part is processed in head tag while most of the content is displayed through body tag.



### Miscellaneous Elements

- Italisize text: For that we need to wrap the text in `<em>` tag or `<i>` tag.
  - `<em>` tag will not only italicize the text but also tell the browser to empasis it. whereas `<i>` tag will only italicize the text.
  - `<em>` tag represents stress on a words it wraps in while `<i>` just takes some word and italicize it.
- Bold text: For that we can also use 2 different tags `<strong>` or `<b>` tag.
  - `<strong>` tag will visually set the text to bold but it also markes importance of words which are wrapped in it while `<b>` tag only makes visual difference.
  - These reason may not make much difference visually but for a screen reader it will make too much of difference.

### Lists

`<li>` tag is used to represent an item in the list, be it ordered list or unordered list.

We can use 2 types of list -> Order or unorder list.

1. `Ordered list` is created with `<ol>` tag and `Unordered list` is created with `<ul>` tag.
2. For `<ol>` items will be listed in decimal format or format specified by `type` attribute for `<ol type=1>`. There are various values of type which can represent different forms for list. To know more about these we can follow [this link](https://devdocs.io/html/element/li)
3. `type` attribute is deprecated so we have to use css element `list-style-type: armenian`. For type attribute there are also many values which one can look up at [this link](https://developer.mozilla.org/en-US/docs/Web/CSS/list-style-type).
4. An `unordered list` will be represented as a bullet list but this can be changed for list items using css attributes for `li` i.e. `list-style-type: disc` other than disc one can use **circle and square** too.
5. `Ordered list` have attributes like `reversed, start`. Former is a boolean attribute and will reverse the numbering on list, later will start the list from a particular value.

### Images

Images can be added by using `<img>` tag with attribute `src` pointing to image source and `alt` tag to represent text when somehow image fails to load.

Images can be loaded from local storage and also from web. To load the image from web we need to set `src` to the web address of the image and for loading image from local storage we need to load image from relative path of the image.

```html
<img src="https://www.google.com/myimage" alt="Failed to load image"/>
<img src="../Data/myLocalImage.png" alt="Failed to load image"/>
```

- `loading` attribute can be used to tell browser whether to load image immediately or to load it when user is scrolling toward it.
- `isMap` is used to send the co-ordinates of the image back to server.
- Setting img as a class than we can set properties like `height, width or vertical-alignment` from css file.

### Anchors

Anchors are used to link one page to another in html. The most important attribute to `<a>` tag is `href` which is used to link current page with another page. i.e. 

```html
<a href="www.google.com">Link to Google</a>
```

 These tags can be used to connect to a page already on internet or can also be used to connect page created by ourselves to connect.

```html
<a href="contact_me.html" target="_blank" rel="noopener noreferrer">Contact Me</a>
```

setting `target` to `_blank` will open the webpage in new tab or as configured in browser settings. To prevent [tabnabbing](https://en.wikipedia.org/wiki/Tabnabbing). 

We can also link files with anchor tab for downloading using `download` attribute. 

```html
<a href="Images/java.png" download="File Name">Download Java Image</a>
```

if we don't give any value to `download` than it will simply download the image with default name i.e. **java.png** here.



### Tables

Tables can be used to layout elements on a webpage.

```html
<table>
    <caption>
        Test Table
    </caption>
    <tr>
        <th>Roll No.</th>
        <th>Name</th>
        <th>Phone</th>
    </tr>
    <tr>
        <td>38</td>
        <td>John</td>
        <td>74843292</td>
    </tr>
    <tr>
        <td>18</td>
        <td>Phillock</td>
        <td>47394101</td>
    </tr>
    <tr>
        <td>80</td>
        <td>Borris</td>
        <td>84329472</td>
    </tr>
</table>
```

A table is created using `<table>` tag. 

- `caption` tag is used to name the table
- `<tr>` is used to create a row in the table
- `<th>` is the header of the table, we can put title of particular column or row in this
- `<td>` is used to put data in table cell or we can say `<td>` represents table cell.
- We can also structure table by using `thead, tbody, tfoot` tags to represent table head, table body and table footer respectively
- `rowspan, colspan` is used to set the span of a particular cell in the table.

By creating table as such we won't get any boundry in the table. For that we need to define `border` attribute of table i.e. `<table border="2px">`. But a better way is to use css to beautify the table like 

```css
td, th {
    padding: 10px;
    border: 2px solid black;
}

table {
    border-collapse: collapse;
}

hr {
    border: none;
    border-top: 5px double rgb(10,10,10);
    color: rbg(256, 256, 256);
    overflow: visible;
    text-align: center;
    height: 0px;
}
```



### Input

Inputs can be used in many ways in web. A single input tag can do many different works like taking input with **text, file, date, time, radio button, checkboxes etc.** 

```html
<body>
  <form action="" method="get" action="nextpage.html">
    <label for="name">Enter UserName: </label>
    <input type="text" name="name" id="name" placeholder="username"  autocapitalize="characters" size="10" required>
    <br />
    <label for="pass">Enter Password</label>
    <input type="password" name="pass" id="pass" minlength="6" maxlength="8" required>
    <br />
    <input type="file">
    <br />
    <label for="age">Select your age</label>
    <input type="range" name="age" min="10" max="40" value="20" step="10" oninput="this.nextElementSibling.value=this.value">
    <output>20</output>
    <br />
    <label for="dob">Your Date of Birth</label>
    <input type="date" name="dob" min="1998-01-01" max="2010-02-22" value="2001-02-01">
    <br />
    <label for="phone">Phone Number: </label>
    <input type="tel" id="phone" name="phone" placeholder="123-45-678" id="phone"
  pattern="[0-9]{3}-[0-9]{2}-[0-9]{3}">
    <br />
      <textarea name="message" id="msg" cols="20" rows="5"></textarea>
    <br />
    <input type="submit" value="Create User">
  </form>
</body>
```

- Form's `action` attribute will execute the action's value on submitting the form.
- Label is used to just show a label with the text field, one can also use placeholder as it is better and clean way to do it.
- `type` is the most important attribute because it will set the behaviour of input field. To get all the values of type [this](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#:~:text=are%20as%20follows%3A-,Type,Basic%20Examples,-button) will be helpful.
- **The `name` attribute is needed to reference the form data after the form is submitted (if you omit the `name` attribute, no data from the text area will be submitted).**
- **The `id` attribute is needed to associate the text area with a label. **
- `value` will set the default value of the input field.
- `placeholder` can be used to set a hint for the input field.
- One can have `radio` buttons, `checkboxes`, `email`, `url` etc. fields by just using `type` attribute.
- `tel` type can be used to fill phone number, one can pass `pattern` to set the pattern for the input type. Value of `pattern` will be a regex.
- `textarea` can be used to write text in a field. To know more about `textarea` [click here](https://www.w3schools.com/tags/tag_textarea.asp).



### Divs

Divs can be treated as divisions which can be used to link different elements together as a single element. Div's can be very useful to divide our structure in a given website.

```html
<div>
    <h1>This is the heading</h1>
    <p>This is how a paragraph should be written in HTML.</p>
</div>
```



