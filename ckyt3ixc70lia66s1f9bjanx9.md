## Wireworld! Svelte Edition

This is Wireworld! Sorry, you don't know what a Wireworld is? How dare you not intimately know something I learned a few minutes ago!

A Wireworld is similar to the famous [Game of Life](https://playgameoflife.com). An infinite world of square cells, each in a distinct, finite state. But unlike boring life that has only two states: alive and dead; Wireworld has four! That's like... twice as good?

A Wireworld cell could either be: nothing, a conductor (wire), an electron tail, or an electron head. An electron head always becomes an electron tail which always becomes a wire, while a wire will stay a wire unless exactly one or two neighbors are an electron head, in which case it will follow suit (and nothing continues to be nothing).

You can see those rules in action in the animation above. You can also play with those rules over here: [wireworld.klmntn.com](https://wireworld.klmntn.com) (warning, it's only somewhat useable on mobile)

# The Making Of

Every three months, [Root](https://root.engineering) sets aside three days (called "hack days") for all its engineers to work on something of their choice. This time around I wanted to have some fun and also learn a little about [Svelte](https://svelte.dev) (Root mostly uses React & React Native). So I chose to make a browser-based Wireworld using Svelte! ([repo](https://github.com/kallmanation/wireworld))

## Decisions, Decisions, Decisions

### Framework

How did I settle on Svelte? I'm already working in React and Vue and have worked a little with Ember long ago (I've even played with the now abandoned [Cell.js](https://github.com/intercellular/cell)). Angular seems to be a different flavor of the React/Vue/Ember gang. Svelte though looks to have some novel ideas that I wanted to expose myself to.

### Graphics

There's really only three options for displaying anything on web:

1. HTML + CSS
2. SVG (+ light CSS)
3. Canvas

The nature of a Wireworld's rendering requirements makes HTML + CSS a no-go. Canvas honestly might be the most appropriate as it can be optimized for high-frequency re-rendering. But future things I'd like to build would work well in SVG and I've already played with Canvas in the past, so I wanted to learn about graphics in SVG!

### World Loop

At the base of the simulation, something will need to decide what the next state should be based on the current state. This could be done in a procedural way with a switch / ifs or functional way or object-oriented. I've [written about the similarities and differences before](https://www.kallmanation.com/oop-vs-fp-a-comparison-using-unconditional-fizzbuzz). I chose an object-oriented approach where each cell will be an object that responds to `nextState`; call `nextState` on all the cells and the world's next state has been found.

## What I Learned

### SVG Just Works

And by this I mean two things. First, SVG does not present a lot to learn above and beyond HTML + CSS (compared to the whole drawing API of a Canvas). I just put SVG tags right into Svelte components and, _bang,_ graphics.

Second, SVG solves some of my biggest pains of drawing on Canvas. On Canvas, everything needs to be constantly erased and redrawn and if I ever want to move my viewport I'll need to do all the math to scale and translate my graphics (or learn and use another library to do it for me). With SVG, one `viewBox` attribute on the top `<svg>` tag handles all the scaling and translations (written by people who know a lot more about graphics than I do _and_ offloaded to the browser so no JS needs spend time on those calculations).

Unless you have a very high paced game or some 3D graphics to render, I would recommend going down the SVG road.

### Svelte Stores are Great

I've always heard that Svelte is good because it compiles down to vanilla JavaScript not needing virtual DOM, making it faster. But the state management available with Svelte's stores is fantastic (suck it Redux). The derived stores open even more possibilities. But by far my favorite are the custom stores: I absolutely love the patterns that opens up.

### The State of Capturing Input Sucks

I had no idea how bad listening for things like key presses and dragging events are today. Given how nice and fairly standard a lot of the APIs across browsers and platforms have become, I was shocked at how rough this space is. I think if I had to do this again, this will be one area where I defer to a library (like [hammer.js](https://hammerjs.github.io)).

### Svelte Seems to be Lacking Tutorials

There's plenty of _examples_ over on the [REPL](https://svelte.dev/repl/hello-world) site. But those examples have next to no explanation on how they work; nearly every search I tried led me to one of those examples, so it was a bit of work piecing the things together, looking at docs, and doing experiments to get things working.

### Wireworlds Like to Light On Fire

Very often a misplaced wire or extra spark will cause my whole creation to devolve into closely packed electrons shooting every which way. This happens shockingly easily and I think makes a wonderful allegory to why our real computers are so hard to make and keep working correctly.

# Things To Make and Do in a Wireworld

If you just want to go play with it now: [wireworld.klmntn.com](https://wireworld.klmntn.com). First, go check out the few [examples](https://wireworld.klmntn.com/examples) already included. A main building block in Wireworlds is the "transistor":

![Animation of a Wireworld Transistor](https://cdn.hashnode.com/res/hashnode/image/upload/v1643053316731/ICtSIufaH.gif)

Like a real [P-type](http://www.cburch.com/logisim/docs/2.7/en/html/libs/wiring/transist.html) transistor; our Wireworld transistor allows the signal to pass when nothing is on the gate, but blocks the signal when the gate is "on" (it even looks like a transistor diagram).

The next piece used in most designs is a signal generator:

![Animation of a Signal Generator in a Wireworld](https://cdn.hashnode.com/res/hashnode/image/upload/v1643053319030/I8zP-FGAh.gif)

Any loop of any shape with an electron moving around it can continuously emit electrons at a regular interval.

And go have fun! Export your creations and comment below.