# The best way to perform a basic reset using the universal selector

Although browsers have become more consistent in the way they display web pages, they still sometimes apply margins
and paddings differently. To overcome this, we can do a "reset" with the help of the universal selector `*` and force
them to be equal regardless of the displaying browser. Further, we change the box model to `border-box` so that borders
and paddings are no longer added to total height or total width that we specify for a box. 

```CSS
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

# How to set project-wide font definitions

To set project-wide font properties, we do this in the body element selector. This is done because properties related to fonts are
usually inherited in CSS.

```CSS
body {
    font-family: "Lato", "sans-serif";
    font-weight: 400;
    font-size: 16px;
    line-height: 1.7;
    color: #777;
}
```

# How to clip parts of elements using clip-path

With `clip-path` one can specify a polygon in which the element is visisble. To achieve this we use the `polygon()` 
function and add coordinates of the polygon corners.

CSS clip-path maker: https://bennettfeely.com/clippy/

# The easiest way to center anything with `transform`, `top`, `left` properties

To center a child element, we can use the following:

```CSS
.child {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

`top` and `left` refer to the parent element, while `transform: translate` references the element itself.

# Animation

The easiest way to implement CSS animation is to use the `transition` property. `@keyframes` is more complex:

```CSS
@keyframes moveInLeft {
    0% {
        opacity: 0;
    }

    80% {
    
    }

    100% {
        opacity: 1;
    }
}
```

Browsers are generally optimized for two of the properties: `opacity` and `transform`. For it to work, one must then 
specify the animation: `animation: <name> <duration> <timing-function>`. To avoid shaking behavior, add `
backface-visibility: hidden` to parent element.

# CSS In-Depth

## Three Pillars of Good HTML and CSS

1. Responsive Design
    * One website for all screen sizes and devices
    * Fluid Layouts
    * Media Queries
    * Responsive Images
    * Correct Units
    * Desktop-first vs. Mobile-first
2. Maintainable and Scalable Code
    * Clean
    * Easy to understand
    * Growth
    * Reusable
    * How to organize files
    * How to name classes
    * How to structure HTML
3. Web Performance
    * Less HTTP requests
    * Less code
    * Compress code
    * Use a CSS preprocessor
    * Less images
    * Compress images
    
## Basics

A css rule is composed of a selector and body.

```CSS
.class {

}
```

Cascading is a process of combining different stylesheets and resolving conflicts between different CSS rules and 
declarations, when more than one rule applies to a certain element:

Importance (Weight) > Specificity > Source Order

### Importance

1. User `!important` declarations
2. Author `!important` declarations
3. Author declarations
4. User declarations
5. Default browser declarations

* Only use `important` as a last resort. It is better to use correct specificities.

### Specificity

1. Inline Styles
2. IDs
3. Classes, pseudo-classes, attribute
4. Elements, pseudo-elements

* The universal selector `*` has no specificity value (0, 0, 0, 0)

### Source Order

The last declaration in the code will override all other declarations and will be applied.

* Rely more on specificity than on the order of selectors.
* Rely on order when using 3rd-party stylesheets - always put your author stylesheet last.

## Value processing

* Each property has an initial value, used if nothing is declared and if there is no inheritance
* Browsers specify a `root` `font-size` for each page (usually `16 px`)
* Percentages and relative values are always converted to pixels
* Percentages are measured relative to their parent's `font-size`, if used to specify `font-size`
* Percentages are measured relative to their parent's `width`, if used to specify lengths
* `em` are measured relativ to their parent `font-size`, if used to specify `font-size`
* `em` are measured relativ to the current `font-size`, if used to specify lengths
* `rem` are always measured relative to the document's `root` `font-size`
* `vh` and `vw` are simply percentage measurements of the viewport's `height` and `width`.

### Converting from relative to absolute pixels

* `%` Fonts: x% `*` parent's computed font size
* `%` Lengths: x% `*` parent's computed __width__
* `em` Fonts: x * parent's computed font size
* `em` Length: x * current element computed font size
* `rem`: x * `root` computed font-size
* `vh`: x * 1% of viewport height
* `vw`: x * 1% of viewport width

### Inheritance

* Inheritance is a means of propagating values from parent to child elements (more maintainable code)
* Every CSS property must have a value:
    * Yes: specified value = cascaded value
    * No: Is the property inherited? (_specific to each property_)
        * Yes: specified value = computed value of parent element
        * No: specified value = initial value (_specific to each property_)
* Properties related to text are inherited
* The computed value of a property is what gets inherited, not the declared value
* Inheritance of a property only works if no one declares a value for that property
* The `inherit` keyword forces inheritance on a certain property
* The `initial` keyword resets a property to its initial value

## Visual Formatting Model

Algorithm that calculates boxes and determines the layout of these boxes, for each element in the render tree, in order
to determine the final layout of the page.

* Dimensions of boxes: the box model
* Box type: inline, block and inline-block
* Positioning scheme: floats and positioning
* Stacking contexts
* Other elements in render tree
* Viewport size, dimensions of images, etc.

### Box Model

* Content: text, images, etc.
* Padding: transparent area around the content, inside of the box
* Border: goes around the padding and the content 
* Margin: space between boxes
* Fill area: area that gets filled with background color or background image

`total width = right border + right padding + specified width + left padding + left border`
`total height = top border + top padding + specified height + bottom padding + bottom border`

To simpliy, use `box-sizing: border-box`:

`total width = specified width`
`total height = specified height`

### Box Types

#### Block-level boxes

* Elements formatted visually as blocks
* 100% of parent's width
* Vertically, one after another
* Box model applies as showed
* Types:
    * `display: block`
    * `display: flex`
    * `display: list-item`
    * `display: table`
    
#### Inline-block boxes

* A mix of block and inline
* Occupies only content's space
* No line-breaks
* Box model applies as showed
* `display: inline-block`    
    
#### Inline boxes

* Content is distributed in lines
* Occupies only content's space
* No line breaks
* Paddings and margins only horizontal (left and right)
* `display: inline`

### Positioning

#### Normal flow

* Default positioning scheme
* Not floated
* Not absolute positioned
* Elements laid out according to their source order
* `position: relative`

#### Floats

* Element is removed from the normal flow
* Text and inline elements will wrap around the floated element
* The container will not adjust its height to the element (Solution: clearfixes)
* `float: left`
* `float right`

#### Absolute Positioning

* Element is removed from the normal flow
* No impact on surrounding content or elements
* We use `top`, `bottom`, `left` and `right` to offset the element from its relatively positioned container
* `position: absolute`
* `position: fixed`

### Stacking-Context

Element with highest `z-index` will show first.

## CSS Architecture

### Think - Build - Architect Mindset

* Think: Think about the layout of your webpage or web app before writing code
* Build: Build your layout in HTML and CSS with a consistent structure for naming classes
* Architect: Create a logical architecture for your CSS with files and folders 

#### Think: Component Driven Design

* Modular building blocks that make up interfaces
* Held together by the layout of the page
* Re-usable across a project and between different projects
* Independent, allowing us to use them anywhere on the page
* (Atomic Design)

#### Build: Block Element Modifier (BEM)

* BLOCK: standalone component that is meaningful on its own
* ELEMENT: part of a block that has no standalone meaning (low specificity)
* MODIFIER: a different version of a block or an element

```CSS
.block {}
.block__element {}
.block__element--modifier {}
```

#### Architect: 7-1 Pattern

* 7 different folders for partial Sass files
    * base/
    * components/
    * layout/
    * pages/
    * themes/
    * abstracts/
    * vendors/
* 1 main Sass file to import all other files into a compiled CSS stylesheet

# Terminology

* Viewport
* `cover`: Width is always the width of the viewport
* `top`: Always stays at top of container
* `background-image` is always used for gradients
* Pseudo classes are special states of selectors.
