## Unconditional Fizzbuzz: an Object-oriented approach

If you haven't read the [original challenge](https://www.kallmanation.com/unconditional-challenge-fizzbuzz-without-if) or the [functional solution](https://www.kallmanation.com/unconditional-fizzbuzz-a-functional-approach) go ahead and give those a read before coming back here. Don't worry, I'll wait.

Now that you're back, let's write FizzBuzz (without `if`s) in an Object-oriented style!

# But what is Object-oriented?

Well, its just being oriented around objects, obviously!

Alright, that's not helpful. Here's the answers you'll usually hear:

1. Encapsulation
2. Abstraction
3. Inheritance
4. Composition
5. Polymorphism

Abstraction can easily be done outside of OOP, as can encapsulation. We already mentioned "composition" in our [functional approach](https://www.kallmanation.com/unconditional-fizzbuzz-a-functional-approach). And polymorphism was the crux of solving FizzBuzz without `if` statements (defining functions with the same signature but different behavior).

So the only thing on this list that's truly unique to OO is inheritance, and that's now widely regarded as [over-used and harmful in most situations](https://medium.com/humans-create-software/composition-over-inheritance-cb6f88070205).

And we're left with the same question as when examining Functional programming: what _really_ is the heart?

[Looking back to the history of OOP](https://medium.com/javascript-scene/the-forgotten-history-of-oop-88d71b9b2d9f) the answer is simply this: objects passing messages to one another.

It has nothing to do with classes or inheritance; simply message passing between objects of varying implementation.

# How to use it

Most of what you've written probably isn't OOP (I know mine isn't). My experience with "objects" has mostly been procedural programming using "objects" to store basic state at best or grouping procedures at worst.

So how could we use the concept of message passing to implement conditionals?

Similarly to our functional approach, we lean on differences in implementation.

1. When the object is `true`, return one piece of internal state (previously set)
2. When the object is `false`, return another piece of internal state (previously set)

```js
const baseBoolean = {
  setThen: function(then) { return { ...this, then }; },
  setOtherwise: function(otherwise) { return { ...this, otherwise }; },
};
const objectOrientedTrue = {
  ...baseBoolean,
  evaluate: function() { return this.then; },
};
const objectOrientedFalse = {
  ...baseBoolean,
  evaluate: function() { return this.otherwise; },
};
```

I've chosen to make my objects immutable (that is, every "change" really just returns a new object). This is not necessary to demonstrate OOP; but I think it's usually a good idea for code safety (remember Pure functions from Functional land?).

What we've done to our booleans, let us do to our numbers as well:

```js
const objectOrientedNumber = {
  value: 0,
  isaMultipleCache: [objectOrientedFalse],
  setValue: function(n) { return { ...this, value: n, isaMultipleCache: [objectOrientedTrue, ...Array(n).fill(objectOrientedFalse)] }; },
  isaMultipleOf: function(dividend) { return this.isaMultipleCache[dividend.value % this.value]; }
};
```

_(The `isaMultipleOf` method uses the same trick that I explained in the [functional solution](https://www.kallmanation.com/unconditional-fizzbuzz-a-functional-approach))_

And this is all we need to implement a FizzBuzz:

```js
const objectOrientedFizzBuzz = {
  for: function(n) {
    const number = objectOrientedNumber.setValue(n);
    return this.three
               .isaMultipleOf(number)
               .setThen(
                 this.five
                     .isaMultipleOf(number)
                     .setThen("FizzBuzz")
                     .setOtherwise("Fizz")
                     .evaluate()
               )
               .setOtherwise(
                 this.five
                     .isaMultipleOf(number)
                     .setThen("Buzz")
                     .setOtherwise(number.value)
                     .evaluate()
               )
               .evaluate();
  },
  three: objectOrientedNumber.setValue(3),
  five: objectOrientedNumber.setValue(5),
};
```

If we squint we can see the same basic structure we used in our functional approach, just much more verbose! (you can see both solutions below in the [codepen](https://codepen.io/kallmanation/pen/ExPKVOP))

%[https://codepen.io/kallmanation/pen/ExPKVOP]

---

Follow me for the last post in this series where I'll do a comparison of FP and OOP; I'll see you there!


