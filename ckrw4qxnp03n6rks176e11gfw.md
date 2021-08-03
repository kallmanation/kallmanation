## Building a Popover with only CSS

Popovers were once firmly in the domain of JavaScript. Today let's examine one of our non-JS options to build them.

# The goals

1. Build a popover (click to open/close)
2. Limit DOM use
3. Require 0 JavaScript

# The results

%[https://codepen.io/kallmanation/pen/gOrPjNO]

# The strategy

1. Use the [`<details>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details) element for the interactive open/close functionality.
2. Style the content to appear in a "popover" box instead of the default inline.

# How to get there

## The CSS selectors used

1. [Attribute selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors) `[attribute]` - selects an element with `attribute` regardless of value

2. [Child combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Child_combinator) `>` - combines two selectors; narrowing the selection on the right-hand side to only those elements that are direct descendants of elements selected by the left-hand side.

3. [Adjacent sibling combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Adjacent_sibling_combinator) `+` - combines two selectors; narrowing the selection on the right-hand side to only those elements that are the immediate, subsequent sibling of elements selected by the left-hand side.

4. [Universal selector]() `*` - selects any element

5. [`:focus` Psuedo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) - Selects an element that currently has "focus" (e.g. last clicked on or tabbed to etc.)

6. [`::-webkit-details-marker` Psuedo-element](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements) - the way to select the default arrow displayed for the summary in Chrome(ish) browsers


## Important CSS styles used

1. `outline` - used to change the default behavior of showing an outline when focused on a `details` element
2. `list-style` - the standard way to control the default arrow displayed beside the summary
3. `text-decoration` - to visually denote what elements have a popover available
4. `transform` and `position` styles - to move the popover into the correct position

## Starting out: the HTML

Step one of our strategy: use the `<details>` tag. We'll put the standard `<summary>` as the text leading to the popover and we'll place a single `<div>` as a sibling to the `<summary>` so that we can easily position the content of the popover as one block.

The only special thing we'll add is a `data-popover` attribute that holds the particular direction we want the popover to appear in.

```html
<details data-popover="up">
  <summary>I'm a popover</summary>
  <div>
    <h1>Popovers can have lots of content!</h1>
    <p>Like paragraphs</p>
    <a href="#">and links</a>
  </div>
</details>
```

## Styling: get the popover popping

First let's remove the default detail styling the same way we started when we [made an accordian in CSS](https://dev.to/kallmanation/building-an-accordion-with-only-css-67h):

```css
details[data-popover] > summary:focus {
  outline: none;
}
details[data-popover] > summary::-webkit-details-marker {
  display: none;
}
details[data-popover] > summary {
  list-style: none;
}
```

Since we've removed any indication that this is clickable to reveal more content, let's add back a visual indicator (a dotted underline seems to be a common pattern).

```css
details[data-popover] > summary {
  list-style: none;
  text-decoration: underline dotted teal;
}
```

Next, let's use `position` to make the popover content pop out of the normal rendering flow:

```css
details[data-popover] {
  position: relative;
}
details[data-popover] > summary + * {
  position: absolute;
}
```

_Note: to make the popover play nicely in paragraphs, as my example shows, add a `display: inline` or `display: inline-block` to the `details[data-popover]` selection._

## Styling: popping into the right position

Positioning will actually be exactly the same as when we positioned our [CSS tooltips](https://dev.to/kallmanation/building-a-tooltip-with-only-css-4k9). I won't totally rehash how this works, but work through one example.

```css
details[data-popover="up"] > summary + * {
  bottom: calc(0.5rem + 100%);
  right: 50%;
  transform: translateX(50%);
}
```

Our popover has already been `position: absolute`'ed, so we can now use `bottom`, `top`, `right`, `left` and `transform` to manipulate the position of our element.

The `bottom` and `right` styles combine to essentially say: place my bottom 100% of my parent's height plus `0.5rem` away from my parent's bottom and place my right 50% of my parent's width from my parent's right.

Then the `transform: translateX` says: from that position, move me 50% of my width to the right.

Combined it becomes a roundabout way of saying: place my center aligned with my parent's center and place me completely above my parent with a little extra space between us.

## Wrapping up and side notes

Thanks for reading! I hope I prompted a little thought on what these building blocks can achieve while building on techniques used from previous "With Only CSS" articles.

The popover could also be done as a hover effect (using `:hover`). Instead of `<details>`, most likely `<div>` or `<span>` would be used, though the DOM would take the same general structure. And the popover should be shown if both the "summary" or the content is being shown (to allow user interaction with the popover).

I like sharing and finding things like this where common web design patterns can be done without a massive JavaScript library or framework bogging the page down.

If you like them too, follow me! I plan to release more posts in this series.