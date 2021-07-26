## Building a tooltip with only CSS

There's a plenty of libraries (mostly JavaScript) to enable the classic tooltip design pattern. But let's build our own! And let's not pollute our HTML with unnecessary markup and avoid loading JavaScript while we're at it.

# The goals

1. Show an additional snippet of text when hovering.
2. Style in the classic triangular tabbed bubble.
3. Allow delayed/animated appearance.
4. Allow tooltip in each of the four directions.
5. Require no additional DOM nodes and as little config in HTML as possible.
6. Require 0 JavaScript.

# The results

%[https://codepen.io/kallmanation/pen/VwvJQRO]

# How to get there:

## The CSS selectors used

1. [Attribute selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors)  
  a. `[attrubute]` - selects an element with `attribute` (regardless of attribute's value)  
  b. `[attribute^="prefix"]` - selects an element with `attribute` where the value starts with `prefix`  
  c. `[attribute$="postfix"]` - selects an element with `attribute` where the value ends with `postfix`  

2. [Psuedo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) `:hover` - selects an element that has a mouse cursor hovering over it

3. [Psuedo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)  
  a. `::after` - styleable element that doesn't actually exist in the DOM; considered the last child of the selected element  
  b. `::before` - styleable element that doesn't actually exist in the DOM; considered the first child of the selected element  

## Important CSS styles used

1. `content` - used in `::before` and `::after` psuedo-elements to set their content

2. `attr("attribute")` - accepts an argument `attribute`, and can be used to set the `content` of a psuedo-element based on the value of said `attribute` on the element selected

3. `position` (along with `top`, `left`, `right`, `bottom`, and `transform`) - Used to position the tooltip

4. `calc()` - accepts a basic mathematical expression and calculates the final value (what's useful to us is how we can mix units, like `width: calc(40px + 50%)` which will give us the width to 50% of what it could be plus a constant 40 pixels extra)

## Starting out: the HTML

On any HTML tag (`span`, `a`, `div`, ...) let's add two attributes: one to describe which direction we want the tooltip to appear in and one to specify the content of the tooltip. Let's use `aria-label` to contain the content and `data-tooltip` for the tooltip configuration.

```html
<any-tag aria-label="Tooltip Content" data-tooltip="up" >Normal Content</any-tag>
```

## Starting out: the core tooltip CSS

The tooltip will need a couple things:
1. The content we put in `aria-label`
2. To be positionable based on the element it is attached to.
3. To be invisible until hovered

To select our element we can use the attribute selector (at this point it doesn't matter what the content is), let's select elements that have both of our tooltip attributes `[data-tooltip][aria-label]`.

On the element itself, we need a `position: relative`. This is so when we go to `position: absolute` the psuedo-element that is our tooltip it will position itself based on the element and not some other parent.

Finally, to hide the tooltip, let's put both `opacity: 0` and `visibility: none`. (Hiding can also be done with `display: none`, but that cannot be transitioned nicely the way `opacity` + `visibility` can)

```css
[data-tooltip][aria-label] { 
  position: relative;
}
[data-tooltip][aria-label]::before {
  content: attr(aria-label);
  position: absolute;
  opacity: 0;
  visibility: none;
}
```
_note that the codepen above has extra styles for color and spacing, but these are the important parts for making the tooltip work_

## Positioning the tooltip: Up/Down

Now we should select based on particular configuration in the `data-tooltip` attribute. We can select where the value starts with something by `[data-tooltip^="startswiththisvalue"]` (we'll select by the starting value instead of exact value to leave room in the attribute for configuring other things later). We want to do two things for the Up/Down direction of tooltips:
1. Center horizontally
2. Position directly above/below the element plus a small margin (to leave room for the common triangular tab on nice tooltips)

Placement above is simple enough: we will position ourselves from the `bottom` of the element, `100%` (that is, place the bottom of the tooltip `100%` of the elements height away from the bottom of the element), and add the small margin using `calc()`. And for below, the same principle applies, but we use `top` instead of `bottom`.

```css
[data-tooltip^="up"][aria-label]::before {
  bottom: calc(0.5rem + 100%);
}
[data-tooltip^="down"][aria-label]::before {
  top: calc(0.5rem + 100%);
}
```

Centering the tooltip can be thought of as two distinct steps:
1. Place the tooltip's right edge at exactly the center of the element
2. Translate the tooltip to the right exactly half of its own width

And we will do just that with two styles: `right: 50%` (which places the right edge of the tooltip in the center of the element) and `transform: translateX(50%)` which moves the tooltip from that position to the right by 50% of its own width. We'll add this to both Up/Down:

```css
[data-tooltip^="up"][aria-label]::before {
  bottom: calc(0.5rem + 100%);
  right: 50%;
  transform: translateX(50%);
}
[data-tooltip^="down"][aria-label]::before {
  top: calc(0.5rem + 100%);
  right: 50%;
  transform: translateX(50%);
}
```

## Positioning the tooltip: Left/Right

Positioning left and right uses the same concepts as above, but instead of centering vertically we want to center horizontally and instead of positioning at the top and bottom of the element we want to position at the left and right.

```css
[data-tooltip^="left"][aria-label]::before {
  right: calc(1rem + 100%);
  bottom: 50%;
  transform: translateY(50%);
}
[data-tooltip^="right"][aria-label]::before {
  left: calc(1rem + 100%);
  bottom: 50%;
  transform: translateY(50%);
}
```

## Making the tooltip appear: without delay

The last new piece of CSS we need is the [Psuedo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) `:hover`. As the name implies, this will select an element that has a pointer hovering over it (just when we want to display the tooltip!). And all we need to do is change it from the default invisible style we gave it above to a visible style:

```css
[data-tooltip][aria-label]:hover::before {
  visibility: visible;
  opacity: 1;
}
```

And we now have a totally functional (if a little ugly) CSS-only tooltip! You can stop here or keep going to add some niceties you would expect of a tooltip: delay, gentle transition, triangular-tab...

## Bonus points: static delay + transition

We can use the [`transition-*`](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions) styles to delay when the tooltip appears and gently fade in:

```css
[data-tooltip][aria-label]:hover::before {
  visibility: visible;
  opacity: 1;
  transition-property: opacity;
  transition-duration: 0.2s;
  transition-timing-function: ease-in-out;
  transition-delay: 0.5s;
}
```

`transition`ing on the `opacity` gives a nice fade in effect. A duration of 0.2 seconds should make it quick enough to avoid being annoying. And we can set a delay to not immediately show the tooltip (avoids popping information in and out while users move their mouse across the page; also referred to as "hover intent").

## Bonus points: configurable delay + transition

We can do even better! Let's allow the element to configure how long the delay (if any) the tooltip should have. Unfortunately we can't dynamically allow any value in `transition-delay` (`attr()` only works in the `content` style) so we will have to select a few predetermined delay times.

```css
[data-tooltip][aria-label]:hover::before {
  visibility: visible;
  opacity: 1;
  transition-property: opacity;
  transition-duration: 0.2s;
  transition-timing-function: ease-in-out;
  transition-delay: 0s;
}
[data-tooltip$="500"][aria-label]:hover::before {
  transition-delay: 0.5s;
}
```

Here I've used the ending portion (`$=`) of the `data-tooltip` attribute to set the delay (in milliseconds). I hope it is clear how this would be extended to add delays of 0.1 seconds up to 1 second and beyond.

## Bonus points: triangular tabs and other styles

I will actually leave this as an exercise to you, dear reader. (Or you'll just copy from the [codepen](https://codepen.io/kallmanation/pen/VwvJQRO) I guess...). But I will give you hints:
1. You probably need another element to create the triangle; you've already used `::before`, thankfully there's also `::after`...
2. You need to position/show/hide/transition/delay the triangle the same way as the tooltip proper.
3. A triangle can be made using `border`s on a no-width, no-height element. Think about what it would look like if you made thick borders around that element and gave each one a different color (also remember that "color" can mean transparent)

![Alt Text](https://cdn.hashnode.com/res/hashnode/image/upload/v1627308875246/YovX-sHOo.png)

# Wrapping up and side notes

Thanks for reading this far! I hope you learned something or at least enjoyed watching me battle the CSS dragon.

I like sharing and finding things like this where common web design patterns can be done without a massive JavaScript library or framework bogging the page down. Give me some suggestions below on patterns you'd like to see me break-down in CSS/HTML-only and maybe this can become a series.

A side note on selection of `aria-label`. This is fine and keeps things relatively accessible, but the better option is [`title`](https://www.w3schools.com/tags/att_title.asp). This is the most native "tooltip" there is. However, there's no styling options available (even just disabling the default effect, as we could then disable and do what we've done here to make the tooltip). Since we can't disable the default effect, it would make our tooltip appear twice (once the way we styled it to be, and once in the default way).