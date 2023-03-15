# Bootstrap

Bootstrap is a library which has prebuilt css classes and those can be used to create good looking websites without any need to create write css code by self.

### Including Bootstrap in website

There are various ways to insert bootstrap into websites but 2 most common ways are:

1. As bootstrap is just precoded css files we can download it and include it manually

2. **Better and most common way** is to link it with the web page

   - ```html
     <!-- CSS Part -->
     <link 
          href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css"
          rel="stylesheet"
          integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" 
          crossorigin="anonymous">
     
     <!-- JS Part -->
         <script
      src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js"  integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4"  crossorigin="anonymous"></script>
     ```

### NavBar with Bootstrap

In bootstrap we have pre-built styles for navbar and one can use those to make a navbar to look like a nav bar. One can get info about navbar by [clicking here](https://getbootstrap.com/docs/5.2/components/navbar/). There are few steps we need to make sure while using bootstrap classes for styling:

1. Navbars require a wrapping `.navbar` with `.navbar-expand{-sm|-md|-lg|-xl|-xxl}` for responsive collapsing
2. Ensure accessibility by using a `<nav>` element or, if using a more generic element such as a `<div>`, add a `role="navigation"` to every navbar to explicitly identify it as a landmark region for users of assistive technologies.
3. we need `navbar-nav` to wrap navbar-items and navbar-items should have a class `nav-item` and if we are using anchor's in `nav-item` as child than we can associate anchors with `nav-link`.

```html
<nav class="navbar navbar-expand-md bg-dark  navbar-dark">
    <a class="navbar-brand ms-1" href="#">Tindog</a>
    <button class="navbar-toggler" 
        type="button" 
        data-bs-toggle="collapse" 
        data-bs-target="#navbarTogglerDemo01" 
        aria-controls="navbarTogglerDemo01" 
        aria-expanded="false" 
        aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse ms-1" id="navbarTogglerDemo01">
        <ul class="navbar-nav ms-auto">
            <li class="nav-item">
                <a class="nav-link active" href="#">Home</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="">Pricing</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="">Download</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="">Contact</a>
            </li>
        </ul>
    </div>
</nav>
```

- [Here](https://getbootstrap.com/docs/5.2/components/buttons/) (colors shown are for buttons but will also work with other properties like background) you can see colors which can be used as bg-{color}. In above example we are using `bg-dark` but we can also use `bg-primary`. 

- `active` is used to specify that we are currently on this page. In above example home that's why it is not faded as Pricing and others.

- `ms-auto` is setting the margin of the class to auto. So it is spacing the elements in the `<ul>` tag. Here m represents margin, s represents start and auto represent that margin start is set to auto. We can find more about [spacing here](https://getbootstrap.com/docs/5.2/utilities/spacing/).

- <strong>we can use nav-bar toggler to make it responsive</strong>. More info on which can be found [here](https://getbootstrap.com/docs/5.2/components/navbar/#toggler).

  1. Create a button with class `navbar-toggler`, set `type` to button, `data-bs-toggle` to "collapse" and `data-bs-target` to id of the wrapper where we have content to fold.
  2. Create the div as wrapper for the content which we wish to fold and set class for this div to be `collapse navbar-collapse`. 
  3. Set id of the wrapper div to the id given to `data-bs-target`.

  - There are many type of togglers on the official [website here](https://getbootstrap.com/docs/5.2/components/navbar/#toggler).

In above code we have a responsive nav bar which will automatically converts to a button when shrunk down. But for that to happen we need to add javascript with bootstrap otherwise button will never responced if we click on it.

### Responsive Design Using row and col

We can use only bootstrap to create a responsive design without help of flexbox for design. Bootstrap uses pre-written flexbox libraries for responsive design.

First create grid layout.

```html
<div class="row">
    <div class="col">
        Hello
    </div>
    <div class="col">
        I am
    </div>
    <div class="col">
        Apple
    </div>
    <div class="col">
        Lover
    </div>
</div>
```

- row will create a row using whole space in horizontal screen.
- col will adjust according to the elements present in the row.
  - for single item in col it will take whole row space
  - for 2 it will divide the space equally by default and same goes for more than 2 elements
  - this will not work with elements like images as those have their own dimensions but they can be forced to have a particular size.

A single row is **divided into 12 parts**. We can set an element to have a specific width by using bootstrap property `col-x` where x is the parts we want to assign to that particular element. As in below example Hello will have 3 times the width of other elements in the same row.

```html
<div class="row">
    <div class="col-6">
        Hello
    </div>
    <div class="col-2">
        I am
    </div>
    <div class="col-2">
        Apple
    </div>
    <div class="col-2">
        Lover
    </div>
</div>
```

Here 2 parts are assigned to 3 div's while 6 parts are assigned to very 1st element.

Now to make it wrap we need have to use `col-{md|lg|sm}` to determine when to wrap around.

```html
<div class="row">
    <div class="col-md-6">wrap-1</div>
    <div class="col-md-3">wrap-2</div>
    <div class="col-md-2">wrap-3</div>
    <div class="col-md-1">wrap-4</div>
</div>
```

Here `md|lg|sm` will represent below which size these should wrap around. So for above code these blocks will wrap around when exposed to medium or smaller sizes.

We can add more classes for better experience:

```html
<div class="row blue">
    <div class="col-md-4 col-lg-3 col-sm-6">wrap-1</div>
    <div class="col-md-4 col-lg-3 col-sm-6">wrap-2</div>
    <div class="col-md-4 col-lg-3 col-sm-6">wrap-3</div>
    <div class="col-md-4 col-lg-3 col-sm-6">wrap-4</div>
</div>
```

According to above code if size of screen is medium it will devide elements in 4 parts each (3 elements in single row), if size is large elements will be divided in 3 parts each (4 elements in single row) and when screen size is small elements will be divided in 6 parts each (2 elements in single row). This is how we will **wrap the elements using bootstrap**.



### Containers

1. Normal Containers

   - These containers have variables sizes as per screen size e.g. 1000px width for large screen size and 800px for medium screen size. These have specific values and can't have values totally dependent on screen size.

   - ```css
     <div class="container">
         <div class="row blue">
             <div class="col-md-4 col-lg-3 col-sm-6">wrap-1</div>
             <div class="col-md-4 col-lg-3 col-sm-6">wrap-2</div>
             <div class="col-md-4 col-lg-3 col-sm-6">wrap-3</div>
             <div class="col-md-4 col-lg-3 col-sm-6">wrap-4</div>
         </div>
     </div>
     ```

2. Fluid Container

   - These containers have variables size too but they have 100% width and automatically resize as per the screen dimensions. They don't have specific values like Normal containers so at every size they have their own dimensions.

   - ```css
     <div class="container-fluid">
         <div class="row blue">
             <div class="col-md-4 col-lg-3 col-sm-6">wrap-1</div>
             <div class="col-md-4 col-lg-3 col-sm-6">wrap-2</div>
             <div class="col-md-4 col-lg-3 col-sm-6">wrap-3</div>
             <div class="col-md-4 col-lg-3 col-sm-6">wrap-4</div>
         </div>
     </div>
     ```

### Buttons

Buttons are simple to modify with the help of bootstrap. There are so many varieties of buttons available with bootstrap and [here](https://getbootstrap.com/docs/5.2/components/buttons/) one can find different types of button and their properties.

**Button Classes**

1. `btn` to specify a class as button.
2. `btn-*` for different styles of buttons. Here '`*`' can be replaced with different types of buttons
   - primary, secondary, success, warning, info, light, dark etc.
3. `btn-outline-*` for different types of outlined buttons * is again filled as per 2nd point.
4. `btn-*` can be used to specify size of button
   - `btn-lg` or `btn-sm`

**Grid Button**

1. To create a grid button we need to wrap it in a parent div.
2. `d-grid`: class for parent to specify that buttons in this container will be grid buttons.
3. `gap-*`: to specify the gap between different grid buttons when in grid format or space between each row of buttons. (parent class property)
4. To make button centered use `mx` (margin-left and margin-right) to have value of auto i.e. `mx-auto`.(parent class property)
5. `col-*` to set the width of buttons. It follows the same principle of 12 part screen.(parent class property).
6. `d-md-*`: **grid, block**. buttons in this type of container will be in grid format or block format (depends upon value passed) when screen size is more than or equal to medium. Again `md` can be replaced with `sm` for small value and `lg` for large value.

**Creating buttons with below configuration:**

1. When screen size is small than buttons should be block elements
2. When screen size is medium or larger buttons should be grid rows.
3. For medium or large screen size button should have a width of 50%.
4. For medium or large screen size button should be centered.

```html
<div class="d-block gap-2 d-md-grid col-md-6 mx-auto">
  <button class="btn btn-primary" type="button">Button</button>
  <button class="btn btn-primary" type="button">Button</button>
  <button class="btn btn-primary" type="button">Button</button>
  <button class="btn btn-primary" type="button">Button</button>
</div>
```

- Here `d-block` is specifying that buttons in this div should be block elements.
- `gap-2` is saying that rows in given div will have a gap of 2.
- `d-md-grid` is saying that buttons in this div should have grid layout when screen size is medium or large.
- `col-md-6` is saying that buttons should have a width of 50% when screen size is medium or large.
- `mx-auto` specifies the margin-left and margin-right to have a value of auto.
- **Here `d-*` represents the display property for a div and * represents it's value.**

### Add icons with font awesome

We need to link font-awesome within html head as we were adding other google fonts. Font awesome has many vector icons which can be used within website by simple means but for that we need to make our website to support [font-awesome](https://fontawesome.com/icons) by linking it in the html file.



### Carousel

A slideshow component for cycling through elements—images or slides of text—like a [carousel](https://getbootstrap.com/docs/5.2/components/carousel/). 

**The `.active` class needs to be added to one of the slides** otherwise the carousel will not be visible. Also be sure to set a unique `id` on the `.carousel` for optional controls, especially if you’re using multiple carousels on a single page. Control and indicator elements must have a `data-bs-target` attribute (or `href` for links) that matches the `id` of the `.carousel` element.

```html
<div id="carouselExampleIndicators" class="carousel slide" data-bs-ride="true" 
        data-bs-interval="2000" data-bs-pause="hover">
    
  <!-- carousel indicators -->
  <div class="carousel-indicators">
    <button type="button" data-bs-target="#carouselExampleIndicators" 
    data-bs-slide-to="0" class="active" aria-current="true" aria-label="Slide 1"></button>
    <button type="button" data-bs-target="#carouselExampleIndicators" 
    data-bs-slide-to="1" aria-label="Slide 2"></button>
    <button type="button" data-bs-target="#carouselExampleIndicators" 
    data-bs-slide-to="2" aria-label="Slide 3"></button>
  </div>
  
  <!-- carousel-items -->
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="..." class="d-block w-100" alt="..." style="background-color:blue;">
    </div>
    <div class="carousel-item">
      <img src="..." class="d-block w-100" alt="..." style="background-color:red;">
    </div>
    <div class="carousel-item">
      <img src="..." class="d-block w-100" alt="..." style="background-color:yellow;">
    </div>
  </div>
  
  <!-- carousel-buttons -->
  <button class="carousel-control-prev" type="button" 
            data-bs-target="#carouselExampleIndicators" data-bs-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Previous</span>
  </button>
  <button class="carousel-control-next" type="button" 
            data-bs-target="#carouselExampleIndicators" data-bs-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Next</span>
  </button>
</div>
```

In above code:

1. carousel-indicators are the indicators in foot of the carousel which represents on which image element we are on.
2. carousel-items are the images on the carousel which are part of slide show.
3. carousel-buttons are buttons on the left and right edge of the carousel to navigate through slides.

More about carousel options  can be [learned here](https://getbootstrap.com/docs/5.2/components/carousel/#options).

### Bootstarp Cards

[Cards](https://getbootstrap.com/docs/5.2/components/card/) are somwhat similar to html code structure itself as they have `header, body and footer` elements.

A basic card:

```html
<div class="card">
  <div class="card-header">
    Featured
  </div>
  <div class="card-body">
    <h5 class="card-title text-muted">Special title treatment</h5>
    <p class="card-text">With supporting text below as a natural lead-in to additional content.</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
</div>
```

Another example of card:

```html
<div class="card text-center">
  <div class="card-header">
    Featured
  </div>
  <div class="card-body">
    <h5 class="card-title">Special title treatment</h5>
    <p class="card-text">With supporting text below as a natural lead-in to additional content.</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
  <div class="card-footer text-muted">
    2 days ago
  </div>
</div>
```

Price Card example: 

```html
<div class="row">
    <div class="col-md-6 col-lg-4 text-center">
        <div class="card h-100 align-bottom">
            <div class="card-header">
                <h3>Chihuahua</h3>
            </div>
            <div class="card-body">
                <h2>Free</h2>
                <p>5 Matches Per Day</p>
                <p>10 Messages Per Day</p>
                <p>Unlimited App Usage</p>
                <button
                        class="btn btn-outline-dark w-100 btn-lg"
                        type="button"
                        >
                    Sign Up
                </button>
            </div>
        </div>
    </div>

    <div class="col-md-6 col-lg-4 text-center">
        <div class="card h-100">
            <div class="card-header">
                <h3>Labrador</h3>
            </div>
            <div class="card-body">
                <h2>$49 / mo</h2>
                <p>Unlimited Matches</p>
                <p>Unlimited Messages</p>
                <p>Unlimited App Usage</p>
                <button
                        class="btn btn-lg btn-dark w-100"
                        type="button"
                        >
                    Sign Up
                </button>
            </div>
        </div>
    </div>

    <div class="col-lg-4 text-center">
        <div class="card h-100">
            <div class="card-header">
                <h3>Mastiff</h3>
            </div>
            <div class="card-body">
                <h2>$99 / mo</h2>
                <p>Pirority Listing</p>
                <p>Unlimited Matches</p>
                <p>Unlimited Messages</p>
                <p>Unlimited App Usage</p>
                <button
                        class="btn btn-lg w-100 btn-dark"
                        type="button"
                        >
                    Sign Up
                </button>
            </div>
        </div>
    </div>
</div>
```



### 

