CSS - Cascading Style Sheets
Describes the presentation of web pages

HTML - Content
CSS - Presentation
JavaScript - Behavior

Resources
Mozilla Developer Network (CSS Refernce)
Web Platform Docs
caniuse.com (browser support)

Browsers have User Agent Stylesheet that gives default styling.

Author Styles
- Inline Styles: write the CSS in the HTML file directly in an HTML tag in it's style attribute. lowest level, very poweful and specific, so will override other styles(bad practice)
<body style="background-color: orange;">
- Internal Styles: embedded in the head section of the HTML document
<head><style>p { font-size: 20px; } </style></head>
- External Stylesheet: browser caching
<link rel="stylesheet" href="style.css">

@import - import CSS from other stylesheets; can be placed inside of another stylesheet (must be first in the CSS file), each import statement is another request to the server so having too many of them can be expensive in terms of load time
@import "bass.css";
@import "layout.css";
@import "typography.css"

CSS Rule Sets
selector {declaration block}

selector selects HTML content

* = universal selector

type/element selectors

element can only have one id
a page can only have one element with that ID name

classes lets us select multiple elements iwth the same calss name
elements can have multiple classes

should not share the same properties b/t class and id for an element

descendant selectors
<header class="main-header"><span></span></header>
.main-header span {
  property: value;
}


Pseudo-classes are similar to classes, but they’re not explicitly defined in an element's class attribute. Unlike type, ID and class selectors, pseudo-classes can target elements dynamically based on user interaction, an element’s state, and more. (mozilla pseudo classes)

Comments /* */

Mozilla CSS Data Types

Data Type = type of css values

CSS offers diff units for expressing lengths

relative units - percentage; the percentage value is measure relative to a parent element's length
https://developer.mozilla.org/en-US/docs/Web/CSS/percentage

absolute units - pixels (will not scale regardless of the size of the screen) fixed size in relation to each other

css pixel is an abstract reference unit. px has to do with the px density of the viewing device
https://developer.mozilla.org/en-US/docs/Web/CSS/length
http://inamidst.com/stuff/notes/csspx
https://docs.webplatform.org/wiki/tutorials/understanding-css-units

you can group elements with a comma
element 1, element 2 {

}

Relative length units are relative to other length values. The most commonly used relative unit is the em unit. The em is known as a font-relative unit because it's calculated based on a parent element's font size.

the default font size of most browsers is 16px, so if we set the font-size to 1em, that's the equivalent of setting it to 16px

font-sizes are relative to it's parents (body's font-size is the overall parent)

rem - root em
only relative to the root element of the page (the html element)

colors
keywords

hex (#(red)(green)(blue)) #FF 00 33 --> #F03


rgb functional notation
rgb(r,g,b) rgb(255,169,73) 255 = white, max value; 0 = black, min value

rgba a = alpha
rgba(255,169,73,.4)

cssfontstack.com
if the font name is more than one word, you need to put it in quotes

line-height property, we can increase, or decrease, the vertical gaps between lines of text. (values are normally integers)

font: weight size/line-height font-stack
font: (normal) 1em/1.5 "Helvetica Neue", Helvetica, Arial, sans-serif;
** font size and font family MUST be defined or it will be ignored by the browser. order matters **

A prioritized list of fonts the browser cycles through until it finds a font installed on a user’s computer is called a: font stack

CSS Box Model
- how elements are displayed and interact with each other

block - take an entire line of content and create a new line before and after itself
inline - stay inline w/ the rest of the content

box
- content area
- padding - creates space inside the box
- border
- margin - creates space outside the box

padding: top & bottom left & right;
padding: top left & right bottom;
padding: top right bottom left;

total width = width + padding + border

box-sizing
Alters the default CSS box model used to calculate widths and heights of elements.
* {
  box-sizing: border-box;
}

max-width
Sets the maximum width of an element. It prevents the used value of the width property from becoming larger than the value set for max-width.

img {
  max-width: 100%;
}

http://blog.teamtreehouse.com/take-control-of-the-box-model-with-box-sizing

background-size: 40%; /* relative to the area of the background image */
background-size: cover;

background: color url('') repeat position / size;

cover
The cover value adjusts a background image so that we're able to see the full image, while maintaining its width and height proportions:

Floats are one of the most commonly used methods for laying out a page with CSS. When an element is floated, the element is taken out of the normal flow of the page and placed along the left or right side of its container.

.group:after {
  content: "";
  display: table;
  clear: both;
}

http://nicolasgallagher.com/micro-clearfix-hack/

text-shadow: horizonal-offset vertical-offset optional-blur color;

shadows have no influence on layout

box-shadow: optional-inset horizonal-offset vertical-offset optional-blur spread color;

css gradients
https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Using_CSS_gradients
https://developer.mozilla.org/en-US/docs/Web/CSS/linear-gradient
https://developer.mozilla.org/en-US/docs/Web/CSS/radial-gradient

background-image: linear-gradient(optioal-to top/left 0deg, color, color);
angle direction is clockwise

background-image: radial-gradient(optional-cirle at top, color color-stop%, color color-stop%);

color stops
https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Using_CSS_gradients#Linear_gradients-Color_stops

@font-face {
  font-family: 'Name';
  src: url('');
}

eot - embedded opentype IE
woff - web open font format mozilla/more
ttf - true type format - safari/ios

media queries

@media (max-width: 960px) {
  body {
    background: royalblue;
  }
  p {
    color: white;
  }

} /* default all */

@media (min-width: 481px) and (max-width: 700px) {

}

<meta name="viewport" content="width=device-width">

the cascade: determines which styles are assigned to an element

The cascade follows three main steps to determine which properties get assigned to an element:
1) importance - origin of the stylesheet
user agent stylesheet
user stylesheet
author stylesheet (the one we write) - most importance

2) specificity - resolves the conflict b/t style declarations
Inline Styles
ID
Class
Element


3) source order - priority based on what order they appear
The last one will always take prescedence over the formers

inheritance - copied from it's parent element
keep things consistent without having to repeat

initial - displays a property's designated default value