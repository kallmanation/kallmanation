## All you need is NAND, NAND, NAND; NAND is all you need!

_All animations courtesy of my [Wireworld built in Svelte](https://www.kallmanation.com/wireworld-svelte-edition)_

---

If you've spent any time programming, you may recognize the "big three" boolean operators: AND, OR, and NOT (often written `&&`, `||`, `!` in many programming languages). Together these operators are [functionally complete](https://en.wikipedia.org/wiki/Functional_completeness), meaning they can be used to build _any_ boolean logic. Combined with some sort of memory, functionally complete operators can be used to build a [turing complete](https://en.wikipedia.org/wiki/Turing_completeness) system.

What if I told you we need only one operator to be functionally complete? That's right! All you need is NAND (short for Not AND; `!(x && y)`). But don't trust me, let me show you.

# Proving NAND is all you need

## NAND is NOT

You've seen the animation of a NAND in a Wireworld above (NAND-imation anyone?). What would happen if instead of two inputs, we tied them both together as one input?

![Animation of NAND acting as a NOT](https://cdn.hashnode.com/res/hashnode/image/upload/v1643054538294/mRP85DZX1.gif)

Notice how this new operator constantly outputs until an input is received and then it stops (aka NOT logic). In JavaScript, if we had a `nand(x, y)` function, we could make `not()` like this:

```js
const not = (x) => nand(x, x);
```

Look at NAND's truth table where both inputs are the same:

|  X    |  Y    | = |  O    |
| ----- | ----- |---| ----- |
| false | false | = | true  |
| true  | true  | = | false |

Looks a lot like NOT to me! **NAND is NOT.**

## NAND is AND

Look at NAND's full truth table compared to AND:

|  X    |  Y    | = | NAND  | AND   |
| ----- | ----- |---| ----- | ----- |
| false | false | = | true  | false |
| false | true  | = | true  | false |
| true  | false | = | true  | false |
| true  | true  | = | false | true  |

It's perfectly inverse (that is the definition of Not AND after all). If only we could NOT the output of our NAND, we would have AND... if you read the last section, that's exactly what we have! Just put the output of a NAND as both inputs to another NAND and viola! an AND appears:

![Animation of two NANDs acting as an AND](https://cdn.hashnode.com/res/hashnode/image/upload/v1643054540926/Waml3hExF.gif)

Again to relate this to code it might look something like this:

```js
const and = (x, y) => nand(nand(x, y), nand(x, y));
```

## NAND is OR

Perhaps the trickiest one yet. AND and NOT were fairly obvious looking at their truth tables. But OR?

|  X    |  Y    | = | NAND  | OR    |
| ----- | ----- |---| ----- | ----- |
| false | false | = | true  | false |
| false | true  | = | true  | true  |
| true  | false | = | true  | true  |
| true  | true  | = | false | true  |

OR is tantalizingly close to NAND. They both have three outputs of `true` and only one case of `false`. But the tables are backwards! What are we supposed to do with this? What if I showed you a table with the opposite inputs:

| NOT X | NOT Y | = | OR    |
| ----- | ----- |---| ----- |
| true  | true  | = | true  |
| true  | false | = | true  |
| false | true  | = | true  |
| false | false | = | false |

Does that pattern look familiar? If we NOT our inputs first (using the same NAND as NOT trick) and put those as the inputs to our NAND, we'll get an OR!

![Animation of three NANDs acting as an OR](https://cdn.hashnode.com/res/hashnode/image/upload/v1643054545343/Df9kJsGDz.gif)

We'll finish out our code examples:

```js
const or = (x, y) => nand(nand(x, x), nand(y, y));
```

_(We just did [DeMorgan's Theorem](https://en.wikipedia.org/wiki/De_Morgan's_laws) a_ very _cool trick for making boolean code more readable; but we'll talk more about that later)_

## NAND is all you need

If you accept that AND, OR, and NOT are functionally complete, since NAND can build each of those three things, NAND, all by itself, is functionally complete!

So yes, NAND _is_ all you need... but please don't write all your booleans using only NANDs. This is for your edification and education.

# Why should I care?

## Problem Transformations Solve Problems

What is the difference between a Bubble Sort and a Quick Sort? They will both give an equally sorted output after all. The importance comes in performance. Because while they are _logically equivalent_ they are not _operationally equivalent_; one of them will finish faster when a physical computer executes the sort.

Many, _many_ things in the business of software development see drastic improvements from transforming the physical operations while keeping logical equivalence. The better we become at this skill, the better programmers we become. (One way to become better is seeing examples, like this).

## NAND Runs the World

It just so happens that in real-world computer chips NAND is one of the easiest things to build. If you've ever heard of [3D NAND](https://searchstorage.techtarget.com/definition/3D-NAND-flash), you now know what that NAND stands for. And when your code works the first time around, thank a NAND gate.

## It's Just Cool!

Who would have thought that just one operator can run the whole world?