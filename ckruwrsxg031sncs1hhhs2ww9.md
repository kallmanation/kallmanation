## Building an Accordion with only CSS

The humble accordion. Plenty of JavaScript ways to build it. But did you know there's an HTML element specifically for this type of design?

The [`<details>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details) element (along with [`<summary>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/summary)) provides an excellent accordion right out of the box.

Of course design called and they want it to look fancy; so let's dig into styling this beast!

# The goals

1. Build a classic "accordion".
2. Style the arrow on the accordion (with subtle animation)
3. Require no additional DOM nodes or configuration (including icons/images)
4. Require 0 JavaScript

# The results

%[https://codepen.io/kallmanation/pen/abdmJNz]

# The strategy

1. Hide the default arrow
2. Make our own arrow (in a [Psuedo-element](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements))

# How to get there

## The CSS selectors used

1. [Type selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Type_selectors) `type` - selects all elements of the given `type` (e.g. `input` will select all `<input ... />` nodes)

2. [Attribute selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors) `[attribute]` - selects an element with `attribute` regardless of value

3. [Psuedo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)  
  a. `:not()` - Inverts the selector given to it  
  b. `:last-child` - Selects an element that is the last child of its parent  
  c. `:focus` - Selects an element that currently has "focus" (e.g. last clicked on or tabbed to etc.)  

4. [Psuedo-element](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)  
  a. `::before` - styleable element that doesn't actually exist in the DOM; considered the first child of the selected element  
  b. `::-webkit-details-marker` - the way to select the default arrow displayed for the summary in Chrome(ish) browsers   

5. [Child combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Child_combinator) `>` - combines two selectors; narrowing the selection on the right-hand side to only those elements that are direct descendants of elements selected by the left-hand side.

## Important CSS styles used

1. `outline` - used to change the default behavior of showing an outline when focused on a `details` element
2. `list-style` - the standard way to control the default arrow displayed beside the summary
3. `border` styles - can be (ab)used into making our arrow; also will use it around the details as an example of styling based on open/closed state
4. `transform` styles - to turn the border into a recognizable arrow
5. `transitiion` - to animate between open and closed states

## Starting out: the HTML

A good `details` element consists of three things:

1. The `<details>` tag enclosing:
2. The `<summary>`; aka the content that should always be shown aka the "title" aka the ... summary
3. The content to be collapsible; can be basically any HTML

```
<details>
  <summary>This shows no matter what</summary>
  <p>This shows only when the details are open</p>
  <p>(as does this)</p>
</details>
```

## First step: hide the default arrow

Going back to our strategy: since we can't animate the default arrow, let's hide it. Unfortunately we'll need to do two things to handle differences between browsers:

```css
details > summary::-webkit-details-marker {
  display: none;
}
details > summary {
  list-style: none;
}
```

And while we're at it, let's also get rid of that default outlining that's done:

```css
details > summary:focus {
  outline: none;
}
```

## Second step: make our own arrow

If you've been following this series, this will probably come as no surprise the trick we're about to do. We can use a `::before` [psuedo-element](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements) to hold our arrow, which gives us an element to play with without having to pollute our DOM!

Now to actually make the arrow we want, I'm going to use two edges of a border. To get it in position, we simply need to rotate it with a `transform`. Like so:

```css
details > summary::before {
  content: "";
  display: inline-block;
  width: 0.5rem;
  height: 0.5rem;
  border: solid 2px black;
  border-left-color: transparent;
  border-bottom-color: transparent;
  margin-right: 0.6rem;
  transform: rotate(-45deg);
}
```

And we obviously want to change the arrow's direction when the details open. We can select that using `details[open]`, and all we have to do is rotate our arrow to a different position:

```css
details[open] > summary::before {
  transform: rotate(135deg);
}
```

## Adding animation

We can animate our arrow pretty easily with a `transition` style. Adding on to the `details > summary::before` styles we just added:

```css
details > summary::before {
  /* all the other styles */
  transition: transform 0.2s;
}
```

Choosing `transform` since that's the style we want to animate (the rotation) and I chose 0.2 seconds, since I like the UX to be pretty snappy.

There is one thing I'm dissatisfied with this so far though: the arrow rotates around the center of the square element we used to make our arrow. Rotating around the visual center of the arrow would feel better (to me).

To do that we can move our box down and to the left (according to the box's definition of "down" and "left"). So let's add that to our `transform` style: `transform: rotate(-45deg)  translate(-20%, 20%);`. Putting it all together now:

```css
details > summary::before {
  content: "";
  display: inline-block;
  width: 0.5rem;
  height: 0.5rem;
  border: solid 2px black;
  border-left-color: transparent;
  border-bottom-color: transparent;
  margin-right: 0.6rem;
  transition: transform 0.2s;
  transform: rotate(-45deg) translate(-20%, 20%);
}
details[open] > summary::before {
  transform: rotate(135deg) translate(-20%, 20%);
}
```

## Adding a border

Let's make it even fancier! How about a rounded border around the entire details (whether open or closed) and a border between the summary and content (if open).

Starting with the summary itself (don't forget the `list-style: none;` we added earlier):

```css
details > summary {
  list-style: none;
  border: solid 1px teal;
  border-radius: 0.5rem;
}
```

That handles the closed state. Now let's handle the open state. Since we haven't put any rules on how many (or what kind of) elements the remaining content could be, we'll need to handle a few things:
1. The summary should not have rounded corners in the bottom when opened
2. All the content elements will need to have only left and right borders to make it appear like one large box
3. The last element of content should handle displaying the bottom border (and rounding the bottom corners)

This is pretty straight forward, we already know how to select based on the details being open. So first let's change those bottom corners on the summary:

```css
details[open] > summary {
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
}
```

Next up, the sides of the box:

```css
details[open] > :not(summary) {
  border-left: solid 1px teal;
  border-right: solid 1px teal;
}
```
_(**Note**: we could have gotten away without the `[open]` qualification here, since any content that is `:not` the `summary` will be hidden anyway if the details are not `[open]`)_

And last but not least, let's close up the bottom of the box when it's open (`:last-child` comes in handy for finding the last element in the details):

```css
details[open] > :last-child {
  border-bottom: solid 1px teal;
  border-bottom-left-radius: 0.5rem;
  border-bottom-right-radius: 0.5rem;
}
```

_(**Note**: this will end up re-rounding the bottom corners of the summary if someone forgot to add content to the details; this may or may not be desirable)_

And we're done! We now have a fancy (and semantic) accordion without any JavaScript or weird `div`s floating around.


# Wrapping up and side notes

Thanks for reading! I like sharing and finding things like this where common web design patterns can be done without a massive JavaScript library or framework bogging the page down. Give me some suggestions below on patterns you'd like to see me break-down in CSS/HTML-only for this series.

Follow me for the next installment in this series: linkable tabs!