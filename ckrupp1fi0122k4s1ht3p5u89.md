## Unconditional FizzBuzz: a Functional approach

If you haven't read the [original challenge](Unconditional Challenge: FizzBuzz without `if`), go ahead and give it a look, think about how you would solve it, then come back. This post will give spoilers and (one of many) answers.

To recap though, we want to make a classic [FizzBuzz](https://en.wikipedia.org/wiki/Fizz_buzz) _without_ using `if` (or ternaries or anything like that).

Let's give this a first run using that fantastic buzz-word, **Functional Programming**. But first...

# What is Functional Programming?

There's the highlights that most everyone talks about:

1. Pure Functions (aka no side-effects, only input and output)
2. Function composition
3. Higher order functions (functions that take functions as parameters)
4. Avoid shared state

But most of those things can be done (or mimicked) in a "non-functional" programming style (and are just downright good ideas IMHO).

So what _really_ is the heart of functional?

You may or may not know that functional programming is rooted in the [Lambda Calculus](http://palmstroem.blogspot.com/2012/05/lambda-calculus-for-absolute-dummies.html). Which is a model where _functions_ are the basic building block of computation.

That's it. In functional programming, all we mean is that functions are what _do_ the program. The looping, the conditionals, the control flow, the data: all functions.

# How to use it to control logic

It's nice to _say_ that functions are all that the program needs. But what does that _mean_. In a practical way; how do we write code that does that?

It all starts with defining the elements we need using the language of `input => output`. And for changes in behavior we lean on changes in implementation instead of built-in statements like `if`.

So what are the behaviors of if?
1. When `true`, evaluate the `then` code
2. When `false`, evaluate the `else` code

Two behaviors, so let's make two functions:
```js
const functionalTrue = (onTrue, onFalse) => onTrue;
const functionalFalse = (onTrue, onFalse) => onFalse;
```

A little simple, but promising. In our functional solution the booleans _are_ the conditionals. To prove that out, let's see what some boolean operations look like; starting with `and`:
```js
const and = (lhs, rhs) => (onTrue, onFalse) => lhs(rhs(onTrue, onFalse), onFalse);
// lhs and rhs for left-hand side and right-hand side
```

Let's work through this right to left:
1. If the left-hand side (lhs) is `false`, then there isn't a point checking the right-hand side (rhs); just return the `onFalse`.
2. If the left-hand side (lhs) is `true`, then also check the right-hand side (rhs); simply passing the parameters unchanged.

In a non-functional approach, this is basically what we've done:
```js
function and(lhs, rhs) {
  if (lhs) {
    if (rhs) {
      return true;
    } else {
      return false;
    }
  } else {
    return false;
  }
}
```

And we can do a similar thing for `or`:
```js
const or = (lhs, rhs) => (onTrue, onFalse) => lhs(onTrue, rhs(onTrue, onFalse));
```

In fact for any truth table, just directly transcribe it into `lhs(rhs(firstCase, secondCase), rhs(thirdCase, fourthCase))` (and remove any `rhs` calls where the two parameters are the same). So for `xor` (exclusive or), the truth table looks something like this:

| lhs | rhs | => | xor |
| --- | --- | --- | --- |
| true | true | => | false |
| true | false | => | true |
| false | true | => | true |
| false | false | => | false |

and the function looks like this:
```js
const xor = (lhs, rhs) => (onTrue, onFalse) => lhs(rhs(onFalse, onTrue), rhs(onTrue, onFalse));
```

# Getting our functional boolean out of a comparison

So we _have_ booleans; but how do we _get_ them from a divisibility check like `42 % 3 == 0`? 

There's multiple tricks to potentially doing this (in fact I think some of the [submitted solutions](https://dev.to/miketalbot/comment/10i9j) used better ones than I did), but the one I will use is to make an array long enough so it will always be longer than the range produced by a modulo, with the first element being my functional `true` and the rest filled with `false`, then simply selecting the element from that array that the modulo result gives.

```js
const isDivisible = (dividend, divisor) => [functionalTrue, ...Array(divisor).fill(functionalFalse)][dividend % divisor];
```

So if the `dividend` is a multiple of `divisor` (`dividend % divisor == 0`) then I will select the first element, which is `true` (otherwise I'll select a `false` element). Just what I need to check if something is a multiple of 3 or 5!

# Making the Fizz Buzz and the Buzz Fizz

We have all the basic building blocks we need now. First, let's check if it's divisible by three:
```js
const divisible_by_three = isDivisible(n, 3);
```

Easy. Next check if it is divisible by five:
```js
const divisible_by_five = isDivisible(n, 5);
```

And lastly, let's extend that little truth table trick into our final output:

| divisible_by_three | divisible_by_five | => | fizz_buzz |
| --- | --- | --- | --- |
| true | true | => | FizzBuzz |
| true | false | => | Fizz |
| false | true | => | Buzz |
| false | false | => | n |

which can be directly transcribed into our function:
```js
divisible_by_three(divisible_by_five("FizzBuzz", "Fizz"), divisible_by_five("Buzz", n));
```

Putting it all together we have our final FizzBuzz solution without using a single conditional!

```js
const functionalTrue = (onTrue, onFalse) => onTrue;
const functionalFalse = (onTrue, onFalse) => onFalse;
const isDivisible = (dividend, divisor) => [functionalTrue, ...Array(divisor).fill(functionalFalse)][dividend % divisor];

const functionalFizzBuzz = (n) => {
  const divisible_by_three = isDivisible(n, 3);
  const divisible_by_five = isDivisible(n, 5);
  return divisible_by_three(divisible_by_five("FizzBuzz", "Fizz"), divisible_by_five("Buzz", n));
};
```

%[https://codepen.io/kallmanation/pen/ExPKVOP]

---

Follow me for the next post in this mini-series: the Object-oriented approach! I'm also planning a final article comparing FP and OOP using these two solutions.