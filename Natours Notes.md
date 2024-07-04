**NATOURS PROJECT**
# BUILDING THE HEADER
## Pt 1
>best way to perform a basic reset via universal selector, 
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
removes default margin and padding many elements have and changes default box model
so that now padding and borders are no longer added to total width/height

>how to set project-wide font definitions, 
body{
    font-family: "Lato", sans-serif;
    font-weight: 400;
    font-size: 16px;
    line-height: 1.7;
    color: #777;
}
we do this on body, not the universal selector as to exploit inheritance; most text-relted properties are
inherited.

>how to clip parts of elements using clip-path
.header{
    height: 95vh;
    background-image: linear-gradient(to right bottom, rgba(126, 213, 111, 0.8), rgb(40, 180, 131, 0.8)) ,url(../img/hero.jpg);
    background-size: cover;
    background-position: top;
    clip-path: polygon(0 0, 100% 0, 100% 75vh, 0 100%);
}
background-size: cover, ensures that whatever the width of the viewport, cover will make sure it
    covers the entire container 
background-position: top, here ensures as we change the vh or vw, the top remains in frame and other parts of the img instead get cropped
clip-path this is a new  property that allows you to clip an element to make interestin ghspaes out of it
    each point is relative to the previous
clippy gives you css code for a bunch of different shapes

## pt2
>the easiest way to center anything w the transform, left and top properties
.text-box{
    position: absolute;
    top:50%;
    left:50%;
    transform: translate(-50%, -50%);
}
absolute will center the left and top most edges but thats now tha we want... we want it relative tot he middle of the element.
So, we then translate -50% from X AND Y bc inside this property its relative to the element size.
Thus, it will shift it to the center of the screen when added to the absolute positioning method that is also here.

# Creating Cool CSS ANimations
2 types: transition property and (more advanced) @keyframes
>keyframes
.heading-primary{
    color:#fff;
    text-transform: uppercase;
    backface-visibility: hidden;
    /* there is a bug with animations that sometimes they have a little shake: this is the fix */
/* what this does is fix it, nobody is quite sure why */
}

.heading-primary-main{
    display: block;
    font-size: 60px;
    font-weight: 400;
    letter-spacing: 35px;
    animation-name: moveInLeft;
    /* this is how we place an animation on an element */
    animation-duration: 1s;
    /* this is how we specify how long the entire animation takes */
    /* animation-delay: 3s; */
    /* this is where we specify a delay on the animation*/
    /* animation-iteration-count: 3; */
    /* this is where we specify the amount of times the animation will happen */
    animation-timing-function: ease-out;
    /* this is where we specify timing functions: ease-in, ease-out, linear, etc. */
    /* these all have a shorthand named ANIMATION */
}

@keyframes moveInLeft {
    /* this is where we define a keyframe animation and name it */
    0%{
        opacity: 0;
        transform: translateX(-100px);
    }
    80%{
        transform: translateX(10px);
    }
    100%{
    opacity: 1;
    transform: translateX(0);
    }
}

# BUILDING A COOL COMPLEX BUTTON
## Pt 1
>
.btn:link, .btn:visited{
    text-transform: uppercase;
    text-decoration: none;
    padding: 15px 40px;
    display: inline-block;
    /* remember by default anchors are inline, so for padding to work we change it */
    /* to center we can use text-align on the parent bc anchors are treated as text */
    border-radius: 100px;
    transition: all 0.2s;
}

.btn:hover{
    transform: translateY(-3px);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.btn:active{
    transform: translateY(-1px);
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}

notice how we reduce distance on shadow and blur to make it looks like we click and then get it closer to the ground.

## Pt2
.btn::after{
    content: "";
    display: inline-block;
    height: 100%;
    width: 100%;
    border-radius: 100px;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    transition: all 0.4s;
}

.btn-white::after{
    content: "";
    display: inline-block;
    background-color: #fff;
}

.btn:hover::after{
    transform: scaleX(1.4) scaleY(1.6);
    opacity: 0;
    <!-- we animate this element to fade out with this and the transition property on the origin class sleector -->
}
We can also add a pseudo element after a pseudo class

.btn-animated{
    animation: moveInBottom 0.5s ease-out 0.75s;
    animation-fill-mode: backwards;
    /*animation defined at a specific keyframe will be applied before animation begins  */
}

# WHAT IS SASS
Sass is a CSS preprocessor, an extension of css that adds power and elegance to the basic language.
We use SASS to fix common CSS Problems, without changing the fundamental way of how css works.
We instead write SASS Source Code, which goes through a Sass compiler, and ends up as compiled CSS code.

**Features:**
variabls: for reusable values such as colors, font-sizes, and spacing
nesting: to nest selectors inside of one another, allowing us to write less code.
operators: for mathematical operations right inside css
partials and imports: to write css in different files and importing them all into one single file.
Mixins: to write reusable pieces of CSS code.
functions: similar to mixings, but they produce a value that can then be used.
extend: to make differnt selectors inherit declarations that are common to all of them
control directives: for writing complex code using coniditionals and loops (not covered in this course)... usually not needed in a real world project.

Sass syntax and SCSS syntax... this code exists in two different syntaxes.
Sass is the original one and DOESNT USE curly braces and is indent-sensitive
SCSS is written more like CSS and is easier to learn. SO well use this one.

## VARIABLES AND NESTING
*variables*
a variable is just like a variable in any other code language... we define it then we can use it all over our code
$color-primary: red

now we can add this anywhere in our code. THis can be decalred outside any rule.
.nav{
    background-color: color-primary;
}
variables MUST start with a $ in SCSS and SASS.

*nesting*
before we probaly would have
.navigation li{
    {DECLARATION}
}

but now with SASS we can do better...
.navigation{
    {DECLARATION for navigation}
    li{
    {DECLARATION for navigation list elements}
    }
}
now we can literally nest it.

lets go even further...
.navigation{
    {DECLARATION for navigation}
    li{
    {DECLARATION for navigation list elements}
        &:first-child{
         {DECLARATION for navigation list element(only the first child)}
        }
    }
}
WHat is this **&**?
it just means the nesting path up to that point... in the example above it would equal .navigatoin/li

We can nest as much as we like but try not to do more than 3 layers... it gets unreadable at that point

random: 
**LIGHTEN() and DARKEN()**
but we can darken a color with darken($color-primary, 15%)
The same also exists for lighten

## MIXINS, EXTENDS, and FUNCTIONS
**MIXINS**
a mixin is like a variable but for a whole rule.
@mixin clearfix{
    &::after{
        content: "";
        clear: both;
        display:table
    }
}

now we can include this anywhere with @include

.navigation{
    @include clearfix
}

they are even more powerful than that...
they allow us to follow the dry principle: DONT REPEAT YOURSELF!
we can pass arguements into mixins... just like a color... lets do this with a color variable
@mixin style-link-text($color){
    text-decoration: none;
    text-transform: uppercase;
    color: $color
}

a:link{
    @include style-link-text($color-text-light)
    <!-- here we are calling the mixing with the color $color-text-light (which obviously is a variable) -->
}

**functions**
@function divide($a, $b){
    @return $a/$b
}

<!-- they arent really useful, but we declare with @, and arguements are declared with $ -->
<!-- they return a value again with the @ declaration -->

nav{
    margin: divide(60, 2)*1px
}
<!-- in this case we need this 1pc bc otherwise unit is unkown -->

**extends**
% decalres a placeholder
%btn-placeholder{
    {A BUNCH OF DECLARATIONS}
}

.nav{
    @extend %btn-placeholder
}

and just like that we extend...
why not use a mixin?....
IDK


# BACK TO NATOURS: BUILDING A FLOAT LAYOUT
*will learn how :not works, how calc() works, how to architect and build a grid system, and how the attribute selector works*

:not({pseudo element}) applies declarations to all elements except the specified pseudo element. good for margin bottom with a list insteda of using :last child you would use :not(:last-child)

calc() allows for calculations insteaed of a value, native to vanilla css. Allows for mixing of units.

    .col-1-of-2{
        width: calc(
            ((100% - #{$gutter-horizontal})/2)
        );
    }
#{SCSS VARIABLE} this #{} is what enables a css variable to be used inside calc()

The [] (attribute selector) can select any attribute/attribute value (including class)... so lets select all classes nested in row than start with "col"...
[class^="col="]
Thats is... class^= means class that start with {value}, in this case col.

*= means any classes that "contains" col-, in our case

$= would mean any classes that "ends" with col-, in our case.

% 0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000

# BUILDING THE ABT SECTION
## pt1
-we'll learn how and why to use utility classes, 
we use utility classes to imporove reusabillity... if we have a heading we want to reuse for the future but want to add something mutable to it like margin, we can add a second class with, say "u-margin-bottom" to add the margin bottom so the original class stays clean

-how to use the bg-clip property,
    .heading-secondary{
        display: inline-block;
        background-image: linear-gradient(to right, $color-primary-light, $color-primary-dark);
        background-clip: text;
        color: transparent
    }
    background-clip: text, means the background gets clipped EXACTLY where the text is, which allows for a lot of cool text coloring effects.
    we dont ACTUALLY want to see the text.... we want to see the clipped background image thats in the shape of text, so lets set color to transparent
    if we didnt do this we would just see the text bc its sitting EXACTLY on top of the clipped bg

## pt2

## pt3
-how to use outline-offset, 
outline is very simliar to border but it has some quirks we can leverage, that would otherwise be impossible with border...
outline-offset on the PARENT creates a space between the content and a border so the border is not on top of the image.

-how to style elements that are NOT hovered, while others are.
    &:hover &__photo:not(:hover){
        transform: scale(.95);
    }
    <!-- using the not selector, once again -->


# BUILDING THE FEATURES SECTION
-another way of creating the "skewed section" design
we can also do the cut out background img with
trasnform:skew(XDEG) as this will skew the whole section... we will then need to skew everything you dont want skewed, skewed in the opposite direction.

-when to use the direct-child selector,
looks like ">"
so "& > *" this means the FIRST direct child starting from this point(&)

# BUILDING THE TOURS SECTION
## pt1
how to build an amazing, rotating card,
we can use the transform: rotate(X OR Y)(DEG) declaration to rotate in the direction we wish.
Look at the html we use:
                    <div class="col-1-of-3">
                        <div class="card">
                            <div class="card__side card__side--front">
                                Front
                            </div>
                            <div class="card__side card__side--back card__side--back-1">
                                Back
                            </div>
                        </div>
                    </div>

This is the CSS:
.card{
    perspective: 150rem;
    -moz-perspective: 150rem;
    position: relative;
    height: 50rem;

    &__side{
    color: #fff;
    font-size: 2rem;

    height: 50rem;
    transition: all .8s ease;
    position:absolute;
    top:0;
    left:0;
    width: 100%;
    backface-visibility: hidden;
    border-radius: 3px;
    box-shadow: 0 1.5rem 4rem ($color-black, .15);

    &--front{
        background-color: $color-white;
    }

    &--back{
        transform: rotateY(180deg);

        &-1{
            background-image: linear-gradient(to right bottom, $color-secondary-light, $color-secondary-dark);
        }
    }
    }

    &:hover &__side--front{
        transform: rotateY(-180deg);
    }

    &:hover &__side--back{
        transform: rotateY(0);
    }

how to use perspective in css,
USE ON PARENT!
we need a really high value--->something like 1500px, bc the lower the value the crazier the effect

how to use backface-visibility property,
this determines if when we turn an element around 180degrees, if we can see the back... this is useful for our cards in our situation.
Because what we want to do is have two cards on top of each other and have one show on the front and one show on the back.
so well set the default of one at being rotateY at 180 and one at 0 so one is showing the front and one is showing the back and then upon hover we will switch the oritentations.

## pt2
using the bg blend modes,
background-blend-mode needs to be applied on the parent.... like photoshop blend modes.
how and when to use box-decoration-break
makes a forced line break due to width apply to all the boxes that are created by an element... so if our text spills into another part we can apply the same effects to both with line-decoration-break: clone;

# BUILDING THE STORIES SECTION
# pt1 
-how to make text flow around shapes via shape-outside and float,
shape-outside WILL ONLY WORK... if the following are true:
element is floated AND has a specified height+width.
Now other elements will float around the shape-outside element AS IF its a circle... to actuallt visually make the element a circle we will now use clip-path

TIP: to affect positionig relatve to other elements ON A FLOATED ELEMENT
lets just use transform and move it rather than touching margins/padding.

-how to apply a filter to images,
you can use the filter property, and like transform, add multiple filters within one filter decalratoin

# pt2
-how to create a bg video covering an entire section,
.bg-video{
    position: absolute;
    top:0;
    left: 0;
    height: 100%;
    width: 100%;
    z-index:-10;
    opacity: 25%;
    overflow: hidden;

    &__content{
        heighT:100%;
        width: 100%;
        object-fit: cover;
    }
}
we gave the video nest 100% height and width and set overflow: hidden on its parent, while giving that 100% height and width.

how to use the <video> html element,
                <div class="bg-video">
                    <video class="bg-video__content" autoplay muted loop>
                        <source src="img/video.mp4" type="video/mp4"/>
                        <source src="img/video.webm" type="video/webm"/>
                        Your browser is not supported!
                    </video>
                </div>
we have a video element as a container for all possible video sources.
on video we can specify some attributes, here we specified autoplay, muted, and loop.

how and when to use the object-fit property,
object-fit: cover, will fill the entire parents' width and height while still maintaing the object's own aspect ratio. The parts that wont fit will be clipped.

# BUILDING THE BOOKING SECTION

## pt1
how to implement "solid-color gradients"
background-image: 
    linear-gradient(105deg, rgba($color-white, .9) 0%, rgba($color-white, .9) 50%, transparent 50%), 
    url(../../img/nat-10.jpg);

so we can do something similar to a clip of a container via "solid-color gradients"
we can specify the deg at the beginning rather than direction.
We can also have more than 2 colors in our gradient and we can specify just after color at what percent we want this color to stop at
if we have two colors at the same percentage it will create a hard color stop that will look similar to if something was clipped.

# pt2
how the general and adjacent sibling selectors work and why we need them
"+" between two elements in a selector is called the adjeacent sibling selector
A + B{}
B MUST COME AFTER A

it selects THE FIRST elements that are siblings of element type B that are siblings of element type A
"~" between two elements in a selector is called the general sibling selector
it selects ALL sibling that that is the specified 2nd element
A~B{} MUST COME AFTER A

how to use ::input-placeholder
we can style the placeholder of the input of the form like this, very new.

how and when to use :focus, 
can style elements when an input is focused

:invalid, 
can style input when google thinks the input is invalid based on the input's "type" property

:placeholder-shown,
you can style an input based on the fact that there is no text there, therefore placeholder will be shown.
One of those CSS pseudo-logical selectors

and :checked
allows you to style an element based on if its radio box is checked

# pt3

    &__radio-input:checked ~ &__radio-label &__radio-button:after{

    }
woooow what does all this mean???
So...
On the checking of the radio input, we select the sibling __radio-label element, and then within that __radio-label, we select the __radio-button element and apply styles to its :after pseudo-element.

techniques to build custom radio buttons.

# BUILDING THE NAVIGATION
what the checkbox hack is and how it works,
gives us something to click on and create elements based on if that checkbox is clicked... essentially gives us the ability to create a button that does something.
a lot goes into this so _navigation.scss is where you should look at the code.

how to create custom animation timing funvtions usnigs bezier curves,
        transition: all .8s cubic-bezier(0.68, -.55, 0.265, 1.55);;
viola... there are websites that will help you make custom curves if you need

how to animate "solid-color gradients"
    background-image: linear-gradient(120deg, transparent 0%, transparent 50%, $color-white 50%);
        background-size: 230%;
        transition: all .4s;

This bg size is what were actually animating... it gets bigger behind the scenes but visually to us it looks like its moving left to right accross the element... so animating this craetes a nice left-to-right animation.

how and why to use transform-origin,
we didnt use it but it describes where the transformation happens.
describes where the center of the trasnformation happens... like anchor point in premiere pro.

# BUILDING THE PURE CSS POPUP WITHOUT CHECKBOX HACK
-how to bulid a nice popup with only CSS,
mix of everything below but mostly leveraging :target pseudo class

-how to use the :target pseudo-class,
the:target psuedo class becomes available on an element anytime its id gets "targeted". That is to say anytime an anchor with an href of that element's id gets clicked on.... this is how we can create a popup window by upping opacity and visibility upon the target element of a previously invisible element geting activated

-how to create boexes with equal height using display: table-cell,
i absolutely hate this but here is how you do it:

   &__content{
        @include absCenter;
        width: 75%;
        box-shadow: 0 2rem 4rem rgba($color-black, .2);
        border-radius: 3px;
        display: table;
        overflow: hidden;
        opacity: 0;
        transform: translate(-50%, -50%) scale(.2);
        transition: all .4s .2s;
    }
    &__left{
        width: 33.333333333%;
        display: table-cell;
    }
    &__right{
        width: 66.666666666%;
        display: table-cell;
        vertical-align: middle;
        padding: 3rem 5rem;
        background-color: $color-white;
    }
    &__img{
        display: block;
        width: 100%;
    }
    &__text{
        font-size: 1.4rem;
        margin-bottom: 4rem;

        column-count: 2;
        column-gap: 4rem;
        column-rule: 1px solid $color-grey-light-2;

        hyphens: auto;
    }

-how to create CSS text columns,
we can set display table on a parent and display: table-cell on children to create equal boxes... once we do this we have a bunch of table-related propertties avaiable, like "vertical-align".

SEE CODE ABOVE TOO

How to automatically hyphentae words using hyphens,
hyphens: auto

column rule gives us the power to craete a line between columns, like a border

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
## MAKING OUR WEBSITE RESPONSIVE
# Using Sass Mixins to write all media queries
how to use sass mixins to write all our media queries,
@mixin respond($breakpoint){
    @if ($breakpoint == phone) {
        @media(max-width: 37.5em){
            @content;
        }
    }
    @if ($breakpoint == tab-port) {
        @media(max-width: 56.25em){
            @content;
        }
    }
    @if ($breakpoint == tab-land) {
        @media(max-width: 75em){
            @content;
        }
    }
    @if ($breakpoint == big-desktop) {
        @media(min-width: 112.5em){
            @content;
        }
    }
}

We pass a breakpoint into this mixin then call it in base.scss
html{
    font-size: 62.5%;
    @include respond(tab-land){
        font-size: 56.25%;
    }
    @include respond(tab-port){
        font-size: 50%;
        // 1 rem=8px=50%
    }
    @include respond(big-desktop){
        font-size: 75%;
    }
}
then we call each mixin function with a different arguement to include all devices. This will complete an the if statement for weach device if-statement in the mixin.


how to use the @content and @if sass directives
@if works just like an if statement in a function in React or Js or some logic-based lanaguage.

@content acts as a tool for code replacement...
we use it in a mixin and then somewhere else in our code with @include.
@include {CODE} will replace the @Content in the mixin when the code is called

WE HAVE to call the media queieres in this order in this example...
this is because you want particular media queries to override other ones, the one most recent will be the cascaded values if multiple apply

# LETS WRITE SOME QUERIES: base typography and layout
Generally Jonas writes queries in this order:
base + typography > generaly layout + grid > page layout > components

## WHAT ARE RESPONSIVE IMGS?
they are crucial for web performance.... serve the right screen size to the right device, which will avoid downloading uneessary large images on smaller screens.
Smaller images should go to smaller screens.

When to use?
resolution siwtching: same img, smaller version for a smaller device
density switching: we reduce pixel density... 
desktops are low res bc they use 1px to display 1pc
smarthpones are high res bc they use 2px to display 1px...

density switching serves one image to high res and one to low res

art direction: we give a whole different image for a smaller screen size... for example a diff image or a cropped image from the original.

# Lets Look at density switching and art direction in depth
-how to use srcset attrbte on the img and source elements, together with density descriptors,
<img srcset="img/logo-green-1x.png 1x, img/logo-green-2x.png 2x" alt="Full Logo" class="footer__logo">
here we use srcset in place of src and give the url then a space then the density descriptor (1x, 2x) and the browser will use the appropariate img density for the apprioraite device.


-how and why to use the picture element for art direction,
                <picture class="footer__logo">
                    <source srcset="img/logo-green-small-1x.png 1x, img/logo-green-small-2x.png 2x," media="(max-width: 37.5em)">
                    <img srcset="img/logo-green-1x.png 1x, img/logo-green-2x.png 2x" alt="Full Logo" class="footer__logo">
                </picture>
inside picture we must have source and img elements.
inside the source we can write a media query and srcset which will be used if the query is met.
If the img is not met, it will use the img provided within the picture element.
Let's include src too in case user is using an older browser, srcset will not work and it can fallback on src

-how to use media queries in HTML,
some elements have the media attribtue available to them, like source

<source srcset="img/logo-green-small-1x.png 1x, img/logo-green-small-2x.png 2x," media="(max-width: 37.5em)">

# Lets Look at resolution switching in depth
-how to allow the browser to decide the best img to download, using the srcset attrbte, width descriptors, and the sizes attrbte of the <img> element

<img srcset="img/nat-1.jpg 300w,img/nat-1-large.jpg 1000w"
                            sizes="
                            (max-width: 900px) 20vw
                            (max-width: 600px) 30vw
                            <!--THESE SHOULD BE IN EM  -->
                            300px"
                            alt="Photo of nature"
                            class="composition__photo composition__photo--p1"
                            src="img/nat-1-large.jpg">

now we can also give the img the ability to choose different imgs with different DPR (device pixel ratio)
in any img we can also ur srcset and then specify the width of the images in px, but this is not enough
we should also add the sizes attribute and say at different queries, what the img on screen currently looks like in vw units, and then a fallback. In this case we will say 300px.

# RESPONSIVE IMGS IN CSS
-how to implement responsive imgs in CSS,

-how to use resolution media queries to target high-resolution screens with 2x,
up to this point weve only ever used media queries to target max-width/min-wdith, but there are other types of media queries.
lets use one to target resolution

    @media (min-resolution: 192dpi) and (min-width: 37.5em), (min-width: 125em){
    background-image: linear-gradient
    (to right bottom, 
    rgba($color-primary-light, 0.8), 
    rgba($color-primary-dark, 0.8)), 
    url(../img/hero.jpg);
    }

why the value is 192dpi (dots per inch), bc thats the resolution of the apple retina screen, which is conventionally used as a refernce here.
whenever dpi is higher than this, the code in the block will apply.

-how to combine multiple conditions in media queries,
here we switch out the img and have media query apply on MULTIPLE conditions.
"and" is && and "," is ||

## TESTING FOR BROWSER SUPPORT WITH @SUPPORT
graceful degredation: providing shiny new features for up-to-date browsers but giving support for out of date browsrss too with a differnt UI.

    @supports ((backdrop-filter: blur(10px) or (-webkit-backdrop-filter: blur(10px)))){
        backdrop-filter: blur(10px);
        -webkit-backdrop-filter: blur(10px);
        background-color: rgba($color-black, .3);
    }

we can make an @support query that tests if a declaration exists on a browser,
if it exists it will apply the block, if not it will not.
on default it will apply a feature outside the block, but if this feature exists, we can use it to overwrite and apply a newer, more modern feature.

# Setting up a Simple Build Process with NPM Scripts
lets make a build process: a sequence of tasks we autmoatically to make one final file that is ready for production.

lets add more scripts: 
one to compile our sass (one file compiling)
one to cocatenate all necessary files thus far (lets install concat through npm to aid w this)
one to prefix our css with webkit, moz, etc....
now we want to have a compress file (in our case this cuts our file size by around 70%)
finally we have a build command that does all of the following.
So this concludes our PRODUCTION (build) workflow:
  "scripts": {
    "compile:sass": "node-sass sass/main.scss css/style.comp.css",
    "concat:css": "concat -o css/style.concat.css css/icon-font.css css/style.comp.css",
    "prefix:css": "postcss --use autoprefixer -b \"last 10 versions\" css/style.comp.css -o css/style.prefix.css",   
    "compress:css": "node-sass css/style.prefix.css css/style.css --output-style compressed",
    "build:css": "npm-run-all compile:sass concat:css prefix:css compress:css"
  },

But lets also make a development workflow for while we work on the application...
now lets add these 3 scripts to the previous workflow...
  "scripts": {
    "watch:sass": "node-sass -rw sass/ -o css/",
    "devserver":"live-server",
    "start":"npm-run-all --parallel devserver watch:sass",
  },
now we have npm run start that will both watch our css and start the live server all ine one command.
We have to use the "--parellel" keyword in there in order to run these two AT THE SAME TIME.
in the other workflow, we wanted to run sequentially, not both at once

# FINAL CONSIDERATIONS
lets change what happens when you select/highlight text via ::selection. There.

::selection{
    background-color: $color-primary;
    color: $color-white;
}

Now lets add "only screen" to our media queries.

@mixin respond($breakpoint){
    @if ($breakpoint == phone) {
        @media only screen and (max-width: 37.5em){
            @content;
        }
    }
}

Now the media query only gets applied to screens... so for example if somebody trys to print out screen it wont work.

Now lets make sure the meta viewport tag is included to ensure that the browser doesnt zoom out.

Now lets adjust out media queries for a special case that if there is no hover option (only non-mouse devices).... (hover:none)....

    @media only screen and (max-width: 56.25em), only screen and (hover:none){
        height: auto;
    }
    
