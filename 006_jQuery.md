# JQuery

JQuery library comes with a bundle of things which can be done with DOM but in easy and shorter way. For the same things to happen you can write a much shorter code with JQuery than javascript. For example `document.querySelectorAll("h1")` we cas use `jquery("h1")` or better `$("h1")`. This library can be loaded like bootstrap or google-fonts etc. using simple script tag.

There are different versions of jquery from different organisations. One from google can be accessed from [here](https://developers.google.com/speed/libraries#jquery). By using `<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.3/jquery.min.js"></script>`, we can import jquery to our website/project.



## Selecting Elements

`$/jquery` will act like `document.querySelectorAll` but it is different as we can simply change the properties of all elements at once, instead of looping through all values returned by `document.querySelectorAll`.

```javascript
// below line is to make sure that before running the function inside it
// html page is loaded otherwise it won't change a thing. We can avoid it
// by simply keeping our loading tag for jquery at end of document but 
// before javascript file.
$(document).ready( () => {
    $("h1").css("color", "red");
})
```

Above code will change all the heading red, but to achieve same with DOM we need below code.

```javascript
let k = document.querySelectorAll("h1");
k.forEach(h => h.style.color = "red");
```

We can also use type for selecting elements like below:

```html
<input type="button" value="press me!"/>
<input type="submit" value="Submit"/>
<input type="text"/>

<script>
	$(":button").css("color", "blue");
    $(":submit").hide(600);
</script>
```

`:submit` will select all the elements with type value `submit` from whole html document. We can simply use `:type` to select all the elements with a certain type.

**Select elements using their attributes**.

```html
<a href="https://www.google.com" />
<a href="https://www.yahoo.com" />
<script>
	$("[href]").css("color", "red");
</script>
```





## Getting/Setting styles

`$("h1").css("color")` will give the value of the first h1 tag in html document. But `$("h1").css("color", "blue")` will set the color of all heading to blue.



## Adding/Removing Classes

`$("h1").addClass("class_name")` to add a class and `"$("h1").removeClass("class_name)` to remove the class from h1. To add multiple classes we can use `$("h1").addClass("class1 class2 class3")`, same is possible with removeClass.

**To check if element has a particular class or not** we can use `$("h1").hasClass("className")` and it will return a boolean value.

```javascript
$("h1").addClass("class1");
$("h1").removeClass("class1");
$("h1").toggleClass("class1");
```



## Adding text/html

Like `$("h1").style("color", "red")` setting text or html for element using same procedure will also affect all elements with h1 tag.

```javascript
// set text for all h1's
$("h1").text("New Text");

// set html for all buttons
$("button").html("<em>My Text</em>")
```



```javascript
// to get the values of text inside all h1's
$("h1").text();

$("button").text();
// if we have 3 buttons (Submit, login, Register) than
// it will return SubmitloginRegister (without spaces)
```



## Setting Attributes

```javascript
// setting attribute for an element
$('a').attr("target", "_blank");

// getting attribute for an element
$('img').attr("src")

// removing attribute for an element.
$(a).removeAttr("target");
```



## Wraping elements in another element

```html
<h1>Heading 1</h1>
<h1>Heading 2</h1>

<script>
    // This will wrap h1's in their own div's
	$("h1").wrap("<div>")
    
    // This will wrap all h1's in one single div.
    $("h1").wrapAll("<div>");
</script>
```





## Adding EventListener

This will add eventlistener to all the h1's present in the html document

```javascript
$("h1").click(function() {
    // do something.
})

// This will watch out for keyboard presses
$("h1").keyDown(function() {
    // do something
})
```

Another way to add event listeners 

```javascript
$("h1").on("click", (e) => {
    // do something
})
```



```javascript
// Event on double click
$("button.btn").dblclick(callback_function);

// Event on hover
$("button.btn").hover(callback_function); 
// hover is not a real event so using it with on
// $(button.btn).on('hover', callback_function) will
// not work. It is a shorthand for mouseenter and
// mouseleave.

// Event on mouse leave
$("button.btn").on('mousemove', callback_function);

// Event on mouse down (holding right mouse button)
$("button.btn").on('mousedowm', callback_function);

// Event on mouse up (releasing mouse button)
$("button.btn").on('mouseup', callback_function);

// Focus and blur(opposite of focus) event
$("input#name").focus(function() {
    // do something
})

$("input#name").blur(function() {
    // do something else
})

// dropdown menu changes
$("select#gender").change(callback_function)
```

To remove the event listener we need to use `unbind()` method on element

```javascript
$("h1").unbind();
```



## Adding and Removing elements with jquery

There are different ways to add elements:

- before("<html code>")
- after("<html code>")
- prepend("<html code>")
- append("<html code>")

```javascript
// Will add a h1 tag before p tag.
$("p").before("<h1>Heading</h1>");

// Will add a h1 tag after p tag.
$("p").after("<h1>Heading</h1>");

// Will add a h1 tag right after opening p tag
$("p").prepend("<h1>Heading</h1>");

// Will add h1 tag right before closing p tag.
$("p").append("<h1>Heading</h1>");

// Will remove all the elements inside a given element
$("ul").empty();	// will remove all <li>'s from unordered list.
// it will still keep <ul> only remove content inside it.

// Remove an element completely
$("ul").detach();
```

To remove an element we simply use remove method. `$("p").remove()`, will remove all the paragraph elements from our website.



## Hide/Show/Toggle Elements

```javascript
// Hide an element
$("button").hide();
$("button").fadeOut(500);	// time of the animation can be passed
$("button").slideUp(400);	// otherwise it will be 400 by default.

// Show an element
$("button").show();
$("button").fadeIn(1000);
$("button").slideDown(300);

// toggle an element
$("button").toggle();
$("button").fadeToggle();
$("button").slideToggle();
```



## Animate Elements

There are different methods which can be used for animations using jquery. [Here](https://www.w3schools.com/jquery/jquery_ref_effects.asp) is a list of these elements. `animate` method can be used to give animation to these elements.

```javascript
$("h1").animate({opacity: 0.5})
```

In animate function we define some css properties which will affect the structure of element and will be used as animation. But with animate we can only use numeric values.

We can also chain different animations like

```javascript
$("h1").fadeOut().fadeIn().animate({opacity: 0.5})
```

These will run in order.

These function take 2 arguments which are optional. 1st being animation time(milli seconds) and other being callback function when animation is completed.

```javascript
$('button#hide').click(function() {
    $('img#dogs').fadeOut(1000, function() {
        $('button#hide').text("Hidden");
    })
})
```

Animation can be paused somewhere in it's transite period by calling stop method on element at which it's been applied at.