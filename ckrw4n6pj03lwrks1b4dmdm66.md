## Building Linkable Tabs with only CSS

The venerable Tab. Some of us have too many on our browsers... Before using JavaScript to build more, consider what just HTML and CSS can do.

There's actually many ways to build tabs with just HTML and CSS (we'll explore an alternative in another post; follow me to be notified when that comes out). In this article, we're going to use the `:target` selector.

# The goals

1. Build a classic tab system
2. Make the current tab stay the current tab on refresh 
3. Make the tabs linkable
4. Require 0 JavaScript

# The results

%[https://codepen.io/kallmanation/pen/YzwQBbp]

# The strategy

1. Make a section for each tab with a unique id
2. Make the tab navigation be links to fragments corresponding to each unique id
3. Use the `:target` selector to show the current tab content while hiding the others

# How to get there

## The CSS selectors used

1. [Psuedo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)  

    a. `:target` - selects the element with the unique `id` corresponding to the URI's [fragment](https://en.wikipedia.org/wiki/URI_fragment)  
    b. `:last-child` - selects the element that is the last child of its parent  

2. [General sibling combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/General_sibling_combinator) `~` - combines that selector on the right-hand side to limit to only elements that are also subsequent siblings of elements selected by the left-hand side

3. [Class selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Class_selectors) `.` - selects all elements with the given class name

## Important CSS styles used

1. `display`  
    a. `block` or [anything](https://developer.mozilla.org/en-US/docs/Web/CSS/display) besides `none` to render the tab content  
    b. `none` - removes the element from having any effect on the rendered page  

2. `color` / `border-color` / your-fancy-tab-styling-here -whatever styles should change on the tabs to indicate being active/inactive for the particular design.

## Starting out: the HTML

Our tabs will be `section`s with a class of `tab` (for selecting) as siblings of one another. These tabs depend on the [URI fragment](https://en.wikipedia.org/wiki/URI_fragment); if you intend to show a default tab if there is no fragment (I do), it is important to order the tabs in the HTML so that the default tab appears last (I want the first tab to be default open, so I place my tabs in reverse order).

Inside each tab, the first element will be our navigation. Unfortunately, to style the tab navigation requires duplicating the navigation in each tab. This could be avoided, but would require other compromises, which we will discuss later.

```html
<section class="tab" id="last-tab">
  <nav>
    <a href="#first-tab">First Tab</a>
    <a href="#last-tab" class="active">Last Tab</a>
  </nav>
  <p>Last tab content</p>
</section>
<section class="tab" id="first-tab">
  <nav>
    <a href="#first-tab" class="active">First Tab</a>
    <a href="#last-tab">Last Tab</a>
  </nav>
  <p>First tab content</p>
</section>
```

## Show the targeted tab

Start off by hiding all the tabs:

```css
.tab { display: none; }
```

Now show the one tab whose `id` matches the URI fragment using the `:target` selector:

```css
.tab { display: none; }
.tab:target { display: block; }
```

Let's also go ahead and throw in the style to show which tab is "active":

```css
.active {
  color: teal;
  border-bottom-color: teal;
}
.tab { display: none; }
.tab:target { display: block; }
```

And we now have a totally functioning tab system that is:
1. linkable
2. survives page refresh
3. uses only CSS!

But, visiting the page without a current fragment will result in _none_ of the tabs showing (until one is clicked)... let's fix that.

## Show the default tab

We placed our default tab as the last in the HTML, so let's use `:last-child`:

```css
.tab:last-child { display: block; }
```

At this point, you are probably wondering why we didn't just order them normally and use `:first-child`?

We can't _only_ display the default tab. We also need to _hide_ the default tab if _any other_ tab has been targeted.

So how do we do that?

With the [general sibling combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/General_sibling_combinator)!

Take a moment to read the description of what `~` does. A key thing to understand is that it only selects _subsequent_ siblings. That is: it only selects _downward_ through the HTML; not upward to previous elements.

That is why we have to place our default tab last. We want to select and hide the default tab if it is a sibling to another tab that is currently targeted by the fragment. Since `~` only works downward, our default has to be last.

```css
.tab:last-child { display: block; }
.tab:target ~ .tab:last-child { display: none; }
```

Put it all together (and throw in some additional layout) and we have a quite nice, linkable, tab system:

```css
.active {
  color: teal;
  border-bottom-color: teal;
}
.tab { display: none; }
.tab:target { display: block; }
.tab:last-child { display: block; }
.tab:target ~ .tab:last-child { display: none; }
```

# Compromises

I chose to duplicate my navigation within each tab in order to allow styling of the active tab.

This may not be the right decision for you. There are other options:

1. Do not style the link to the tab. This allows moving them outside and above to deduplicate the HTML.
2. Specifically target each tab by unique classes in CSS. This moves duplication from HTML into CSS.
3. Use fixed height tab content. Mix the tab content and navigation links in a way they can be selected with the [adjacent sibling combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Adjacent_sibling_combinator), `+`. Absolutely position the tab content into the reserved area.

and I am sure there are more...

And, of course: use a little JavaScript. When needed it can be the right tool. This is just an experiment to make you think about when it actually is the "right" tool, or just a hammer beating your design into submission.

# Wrapping up

Thanks for reading! I like sharing and finding things like this where common web design patterns can be done without a massive JavaScript library or framework bogging the page down. Give me some suggestions below on patterns you'd like to see me break-down in CSS/HTML-only for this series.

