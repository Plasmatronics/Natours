## 3 PILLARS OF WRITING GOOD CSS
>Responsive Design
build a website that works for all kinds of devices:
-fluid layouts
-media queries
-reponsive images
-correct units
-desktop-first vs mobile-first
>Maintainable and Scalable Code
-clean, easy to understand, reusable, and easily growable
-pay attention to how we organize filse, name classes, and structure HTML
>Web Performance
-less  HTTP requests
-less code
-compress code
-use a css preprocessor like SASS
-less images
-compress images

## AN OVERVIEW OF BEHIND THE SCENES
what happens to css when we load up a webpage?
html is loaded--->html parsed(line by line)(this is where DOM is created)
in this parsing CSS is also detected and loaded (once it gets to that link line)
CSS is then parsed
two stages of CSS parsing: 
 1). resolving conflicting CSS declarations (cascade)
 2). Process final CSS values (like what 50% means on the used device)
Then the CSS tree is madeL the CSS Object Model (CSSOM)
This CSSOM and the DOM are put together into the Render tree
This render tree is used to render the website via the visual formatting model

## HOW CSS IS PARSED: resolving conflicting CSS declarations (cascade)
>Cascade: THe process of combining different stylesheets and resolving conflicts between differnt CSS Rules and declarations, when more than one rule applies to a certain element
CSS can come from multiple sources as well...
Author CSS: The CSS we write (developers)
User CSS: CSS coming from the user (like when they change default font size)
Browser (use Agent): Styling like default links that we dont change and style

To determine which one will apply they weigh 3 things..
most importantly: 
1). Importance
    these themselves have a nested order of importance
    a). user !important declarations
    b). Author !important declarations
    c). Author declarations
    d). User Declarations
    e). Default Browser Declarations
    *IF ANY DECLARATIONS OF THESE ARE THE SAME, WE'LL MOVE TO THE SPECIFICITY*
2). Specificity
    a). inline styles
    b). IDs
    c). classes, pseudo-classes, attribute
    d). elements, pseudo-elements
Specifity is a 4 value number (a, b, c, d)
Count the number of ocurrences in the seelector and that will make the final rule specififity value
*so lets say we have (0, 0, 1, 0), (0, 1, 2, 2), and (0, 0, 0,1)*
*when comapring rules with different specificty go to each value (a, b, c ,d),
a has more importance than b, b than c, and so on... if any match go to the next value*
*The winning value="cascaded"*
*IF SAME SPECIFICITY MOVE TO SOURCE ORDER*
3). Source Order
the last declaration in the code will override all other declarations and will be applied

>Some Final Notes:
Only use !important as a last resource... better to use correct specificities for more maintainabe code
Inline Styles will always have priority over styles in external stylesheets
The universal selector * has no specificifty value (0, 0, 0, 0) but well get more into that later
Rely more on specififty than order of selectors (increased maintainability)
But, Rely on order when using 3rd party stylesheets... in the html always put your author stylesheet last.

## HOW CSS IS PARSED: Value Processsing
Each time you use a relative unit, it will ultimately be converted to px.
all values on an element start as declred value, then... the cascaded value wins
if there is no cascaded value, it becomes the defaulted value (every element has a default value)
Then the computed value, relative values become absolute.
Then comes the used value, which are the final calculations based on the layout.
THen this used value becomes an actual value (usually rounded a bit and gone through browser and device restrictions)

>Lets go through an example....padding
Lets say we have an element with an UNDEFINED value
it will have no declared value
it will have no cascaded value
it will have a specififed default value of 0px
Since this is already an absolue unit, the computed, used, and actual values all remain 0px.

>Lets go through anothe example...font-size(root)
The value is not decalred anywhere (in our example)
The browser default cascaded value is 16px. Since we have alreay reached absolute units, it trickles down as 16px fir specified, computed, used, and actual values.

>another example... section font size
Well start with a specified section value of 1.5rem font size
no competing values... so it trickles down as the cascaded value and specified value.
Not it turns to 24px at the computed value step (bc here relative values are converted to absolute)
now, weve reached 24px so it trickles down as used and actual value

>another example... paragraphy font size (inherited from above's section font size value)
no value is specified, so declared and cascaded value are null.
The specified default value now is 24px bc it was inherited from the section font size
weve reached absolute value so it trickles down all the way to being the actual value

**Lets Dive a Bit Deepr: HOW UNITS ARE CONVERTED FROM RELATIVE TO ABSOLUTE(Px)**
Lets Look at some different examples:
>% (Fonts)
well take 150% as our example.
this 150% is multiplied by the parents computed size (in our case well say 16px) so it becomes 24px

>% (Lengths)
Well take 10% as our example.
This percent is multiplied by the parents computed*WIDTH* (in our case 1000px) so it becomes 100px

>em (font)
Well take 3em as our example.
This 3em is multiplied by the parent computed font-size (in our case 24px bc of the above calculations) and thus becomes 72px

>em(lengths)
Well take 2em as our example.
this 2em is mulitplied by the CURRENT font size (which is 24px in our case) as reference so it becomes 48px

>rem
Well take 10rem as our example.
REGARDLESS OF LENGTH OR FONT, IT ALWAYS uses the root computed font size (in our case its 16px) so it becomes 160px

>vh and vw 
they take the value and multiply it by 1% of the current viewport width or height

FINAL NOTES: USE % ON HTML INSTEAD OF PX SO USER CAN SCALE ON THEIR END (62.5% of default 16 =10px)

## HOW CSS IS PARSED: Inheritance
Inheritance is how values get propagated from parents to children
Rememver every CSS property mus thave a value.
If cascaded value, specified value= cascaded value
if no cascaded value, is the property inherited? (each property has specific guidelines on this)?
If it is-->specified value=COMPUTED valued of parent element
if not--> specified value becomes initial value (which is specific to each value)

generalyy text-related propertties are inherited
REMEMBER: CALCULATED NOT DECLARED values get inherited
the inherit keyword forces inheritance on a certain propety
the initial keyword resets a property to its initial value
we should use inheritance whenever we can instead of applying things via the universal selcctor
pseudo elements DONT get affected by universal selector, must manually select them too alongside "*"

## HOW CSS RENDERS A WEBSITE: THE VISUAL FORMATTING MODEL
>THE VISUAL FORMATTING MODEL: algorithm that calculates boxes and determines the layout of these boxes, for each element in the render tre, in order to determine the final layot of the page.
Takes into account...
*dimension of boxes (box model)*
-each and every element can be seen as a box and is made up of the obvious things
-in default box model: height/width considers padding and border and is NOT a prt of the height/width
-border-box: height/width only considers content, though padding and border will reduce inner width of content area
height/width INCLUDES padding and border

*box type: inline, block, inline-block*
block level takes up 100% of parents width
inline blocks take up only contents space
for inline, padding and margins only work horizontally
inline-block look like inline but we can apply padding and borders as if it were a block

*positioining scheme: floats and positioning*
normal flow: default, elements are laid out as teheir order in html
floats: element now causes text and inline element to float around it.
FOr floats, the container will not adjust its height to the element(can cause collapses)
absolute positioning: elements pretend like its not even there.

*stacking contexts*
what detemine in which order  in terms of closeness to us, things are rendered.
Most commonly we use z-index, but there are others
but transform, opacity, filter, can effect stacking order... why sometimes z -index doesnt work as expdcted

Other THhins that Effect rendering on a website:
*other elements in the render tree*
*viewport size, dimensions of images, etc.*

## CSS ARCITECTURE, COMPONENTS, AND BEM
**Think** about the layout before writing code, then **build** layout with a consistent structure for naming classes then create a logical**architecture** for files and folder

Think:
component-driven design: 
modular building blocks that make up interfaces
Held together by the layout of the page
reusable accross a project
indepdnednt, allowing us to use any component anywhere(shouldnt depend on parents)

Build:
BEM (block element modifier)
a BLOCK is a standalone component that is meaningful on its own
ELEMENT is a part of the block that has no meanining on its own
MODIFIER is a different version of a block or an element

Arhitecture:
7-1 PATTERN
7 different folders for partial Sass files, and 1 main Sass files to import all other files into a compiled CSS stylesheet
the folders:
-base
-components
-layout
-pages
-themes
-abstracts
-vendors