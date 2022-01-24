## De Morgan's Theorem

In my [last post](https://www.kallmanation.com/all-you-need-is-nand-nand-nand-nand-is-all-you-need), I hinted that De Morgan's Theorem makes "boolean code more readable" but that "we'll talk more about that later". Later is now! Let's talk about De Morgan's Theorem (or [De Morgan's Laws](https://en.wikipedia.org/wiki/De_Morgan's_laws)).

De Morgan's Theorem comes in two parts, each in the same pattern:

```js
!(a && b) == !a || !b
!(a || b) == !a && !b
```

# Why Does This Work?

It may not be immediately clear _why_ this works. Why can we just swap between `and` 'ing, and `or` 'ing, if we twiddle with the `not` 's? One way to think about it is that `and`, and `or` are "twiddled" opposites of each other:

| X     | Y     | = | AND   | OR    |
| ----- | ----- |---| ----- | ----- |
| false | false | = | false | false |
| false | true  | = | false | true  |
| true  | false | = | false | true  |
| true  | true  | = | true  | true  |

`and` has three falses and a true while `or` has three trues and a false, and the order is also "twiddled" to be reversed.

First `not` 'ing the input can be thought of as reversing the order of the output:

| NOT X | NOT Y | = | AND   |
| ----- | ----- |---| ----- |
| true  | true  | = | true  |
| true  | false | = | false |
| false | true  | = | false |
| false | false | = | false |

Now we can see this output is the exact opposite of an `or` against the opposite inputs. So:

```js
!(!a && !b) == a || b
```

Which is just a rearranged version of one of the relationships above:

```js
!a && !b == !(a || b)
```

And giving the same treatment to `or` first instead of `and` first will yield the other relationship.

# But Intuitively Why Does This Work?

Well, if an apple is not red and not green, would you say that apple is also not red or green? You probably did not even question that those two statements are equivalent: `not red and not green` has the same meaning as `not red or green`. But that's exactly the De Morgan's Theorem we worked through above!

```js
!red && !green == !(red || green)
```

And if that apple was not not red nor not green; you may eventually see through my double negatives that I mean to say the apple is red and green.

```js
!(!red || !green) == red && green
```

I propose that one of those statements is much more readily understood (both in code and in english). And why De Morgan's Theorem is worth remembering: cleaning up sometimes confusing boolean expressions.

# What's This To Do With Wireworld?

Look again at the NAND from the [last post](https://www.kallmanation.com/all-you-need-is-nand-nand-nand-nand-is-all-you-need) in this series.

![Image of NAND in Wireworld](https://cdn.hashnode.com/res/hashnode/image/upload/v1643054771409/MPi2-rOehk.png)

Now let me show you what an OR and a NOT look like in Wireworld.

![Image of OR in Wireworld](https://cdn.hashnode.com/res/hashnode/image/upload/v1643054772873/TohXS26Ko.png)
![Image of NOT in Wireworld](https://cdn.hashnode.com/res/hashnode/image/upload/v1643054774307/yLEbqlweJ1.png)

Look back to the NAND and do you see the pattern? My NAND is actually just an OR with its inputs first NOT'ed. That's exactly what De Morgan told us!

```js
!(a && b) == !a || !b
```

