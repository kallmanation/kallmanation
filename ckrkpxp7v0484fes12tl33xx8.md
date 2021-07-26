## Styling a checkbox with only CSS

This seems like an obvious statement; styling should be done with CSS. But unfortunately in the case of checkboxes, there's basically no support for directly styling the checkbox.

Because of that, I've seen several JavaScript implementations to change between displaying an unchecked box and a checked box based on state in order to fake a styled checkbox.

But let's not go so quickly to JS to solve a fundamentally styling problem! It can be done with just CSS, we just need to shift our thinking a little.

# The goals

1. Style the checkbox
2. Style the label
3. Require no additional DOM nodes or configuration (including icons/images)
4. Require 0 JavaScript

# The results

%[https://codepen.io/kallmanation/pen/wvMawrj]

# The strategy

Since we cannot style the checkbox input directly with something like `checkbox-style: awesome;`, we'll need to take a different approach:
1. Hide the actual checkbox input
2. Show a styled element that looks like an empty checkbox when the input is unchecked
3. Show a styled element that looks like a checked checkbox when the input is checked

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

2. `display` - specifically `none` to hide elements and `inline-block` to make our otherwise inline checkbox able to have a consistent width and height

3. `width` / `height` - does what you think: sets width and height of the element

4. `color` - sets the color of the text

5. `text-align` / `vertical-align` - used for adjusting the position of our check/checkbox to its label

6. `border` styles - How we'll form and color the checkbox

7. `background` - sets the background color (used to fill in the checkbox when it is checked)

## Starting out: the HTML

Let's set up our checkbox as a child of its `label` element with a sibling of a `span` of the label text:

```html
<label>
  <input type="checkbox" name="key" value="value" />
  <span>I am a checkbox</span>
</label>
```

This structure allows clicking on the label text (`I am a checkbox`) to toggle the checkbox without needing `for` or unique `id` attributes. Placing the text in a `span` directly after the `input` will allow us to select it in CSS.

## First step: hide the unstyleable checkbox

Going back to our strategy: since we can't do anything with the native checkbox, we'll have to hide it and do our own thing.

```css
label > input[type="checkbox"] {
  display: none;
}
```

We'll select the checkbox (`input[type="checkbox"]`) and make sure it's labelled the way we need it to be (`label >`). Then just `display: none` to get it off our screens.

## Second step: make our own checkbox

Making an empty square is easy with CSS, just put a border around an element with no content and set its width and height. Let's also put it on the `::before` psuedo-element to avoid adding unnecessary HTML.

```css
label > input[type="checkbox"] + *::before {
  content: "";
  display: inline-block;
  vertical-align: bottom;
  width: 1rem;
  height: 1rem;
  border-radius: 10%;
  border-style: solid;
  border-width: 0.1rem;
  border-color: gray;
}
```

Building from the selection in the previous section; we add `+ *` to select any element as long as it is the direct subsequent sibling to the checkbox of interest and then `::before` to select the said psuedo-element for making the box.

## Last step: make our checkbox change when checked

So far, even though the checkbox has been working, it doesn't _look_ like it's working. Let's change that by making the checkbox checked when it is `:checked`. Also, for the proof-of-concept, let's change styles on the label text itself (while I'll only change the text color, you can imagine changing any style to build any sort of check/uncheck select/unselect interface).

```css
label > input[type="checkbox"]:checked + *::before {
  content: "✓";
  color: white;
  text-align: center;
  background: teal;
  border-color: teal;
}
label > input[type="checkbox"]:checked + * {
  color: teal;
}
```

The important part here being `:checked` is placed after the `input[type="checkbox"]` (since that is the element that is checked or not). And fortunately for us, there's a checkmark character that we can set as the content of our checkbox: `✓`.

## Bonus points: extend this to radio buttons

As a challenge to you, reader, this same strategy applies to making a styled radio button. Using this as a guide, are you able to transform this for the `type="radio"` input? (or just follow me as that will be the next installment in this series)

# Wrapping up and side notes

Thanks for reading! I like sharing and finding things like this where common web design patterns can be done without a massive JavaScript library or framework bogging the page down. Give me some suggestions below on patterns you'd like to see me break-down in CSS/HTML-only for this series.

While this example had "no unnecessary DOM"; it is also perfectly valid to include an additional `span` (or two) to hold svg/font-awesome icons for more precise/exotic checkbox designs. Then `:checked` should be used to alternatively `display: none` and `display: inline-block` the icons and `+` will need to become `~` to select all the siblings.