## Styling a radio button with only CSS

As I hinted to in the [previous installment](https://www.kallmanation.com/styling-a-checkbox-with-only-css) of this series; a radio button is in the same situation as checkboxes on the web: no options for styling the native component, but the same strategy we used for checkboxes can be applied to radio buttons as well.

# The goals

1. Style the radio button
2. Style the label
3. Require no additional DOM nodes or configuration (including icons/images)
4. Require 0 JavaScript

# The results

%[https://codepen.io/kallmanation/pen/BajyWmp]

# The strategy

1. Hide the actual radio input
2. Show a styled element that looks like an empty radio button when the input is unchecked
3. Show a styled element that looks like a selected radio button when the input is checked

# How to get there

## The CSS selectors used

1. [Type selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Type_selectors) `type` - selects all elements of the given `type` (e.g. `input` will select all `<input ... />` nodes)

2. [Attribute selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors) `[attribute="value"]` - selects an element with `attribute` where its value is exactly `value`

3. [Psuedo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) `:checked` - selects checkbox/radio input types or `option`s in a `select` that are selected/checked/on/active

4. [Psuedo-element](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements) `::before` - styleable element that doesn't actually exist in the DOM; considered the first child of the selected element

5. [Universal selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Universal_selectors) `*` - selects anything/everything

6. [Child combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Child_combinator) `>` - combines two selectors; narrowing the selection on the right-hand side to only those elements that are direct descendants of elements selected by the left-hand side.

7. [Adjacent sibling combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Adjacent_sibling_combinator) `+` - combines two selectors; narrowing the selection on the right-hand side to only those elements that are the siblings immediately after the elements on the left-hand side

## Important CSS styles used

1. `content` - used in the `::before` psuedo-element to set its content

2. `display` - specifically `none` to hide elements and `inline-block` to make our otherwise inline radio button able to have a consistent width and height

3. `width` / `height` - does what you think: sets width and height of the element

4. `color` - sets the color of the text

5. `text-align` / `vertical-align` - used for adjusting the position of our radio button to its label

6. `border` styles - How we'll form and color the radio button

7. `radial-gradient` - used in `background` to fill in the radio button in the classic "half-full" style

## Starting out: the HTML

Let's set up our radio button as a child of its `label` element with a sibling of a `span` of the label text:

```html
<label>
  <input type="radio" name="key" value="value" checked />
  <span>I am a radio button</span>
</label>
<label>
  <input type="radio" name="key" value="another-value" />
  <span>I am another radio button</span>
</label>
```

This structure allows clicking on the label text to select the radio without needing `for` or unique `id` attributes (Note it's important the `name` is the same for at least 2 or more radio buttons. This creates the "radio group".). Placing the text in a `span` directly after the `input` will allow us to select it in CSS.

## First step: hide the unstyleable radio

Going back to our strategy: since we can't do anything with the native radio button, we'll have to hide it and do our own thing.

```css
label > input[type="radio"] {
  display: none;
}
```

We'll select the radio button (`input[type="radio"]`) and make sure it's labelled the way we need it to be (`label >`). Then just `display: none` to get it off our screens.

## Second step: make our own radio button

Making an empty circle is easy with CSS, just put a border around a square element and give it a border radius of `50%`. As with the checkbox, we'll put this on the `::before` psuedo-element to avoid extra markup.

```css
label > input[type="radio"] + *::before {
  content: "";
  display: inline-block;
  vertical-align: bottom;
  width: 1rem;
  height: 1rem;
  margin-right: 0.3rem;
  border-radius: 50%;
  border-style: solid;
  border-width: 0.1rem;
  border-color: gray;
}
```

## Last step: make our radio change when checked

Again like the checkbox, using `:checked` we can style our radio button to make it appear different when selected.

```css
label > input[type="radio"]:checked + *::before {
  background: radial-gradient(teal 0%, teal 40%, transparent 50%, transparent);
  border-color: teal;
}
label > input[type="radio"]:checked + * {
  color: teal;
}
```
The main difference between the radio button and the checkbox is the content and background. Here we have no difference in content and instead use a `radial-gradient` to fill only a portion of the circle in.

Going more in depth on the `radial-gradient`:
1. We start with `teal 0%`, since `radial-gradient` is defined from the center outward, we've basically said the center should start with teal. 
2. Next `teal 40%` means "40% of the way to the edge should be teal". Since we started with teal, `radial-gradient` makes a gradient from teal to teal, also known as solid teal.
3. Then `transparent 50%` will cause a blending from teal to transparent between the 40 and 50 percent marks. 
4. And the final `transparent` results in solid transparency until the edge.

Putting all that together, we've essentially made a solid teal dot occupying 40% of its element (with 5% of gentle fading away on each side).

# Wrapping up and side notes

Thanks for reading! I like sharing and finding things like this where common web design patterns can be done without a massive JavaScript library or framework bogging the page down. Give me some suggestions below on patterns you'd like to see me break-down in CSS/HTML-only for this series.

Follow me for the next installment in this series: accordians!

While this example had "no unnecessary DOM"; it is also perfectly valid to include an additional `span` (or two) to hold svg/font-awesome icons for more precise/exotic radio button designs. Then `:checked` should be used to alternatively `display: none` and `display: inline-block` the icons and `+` will need to become `~` to select all the siblings.