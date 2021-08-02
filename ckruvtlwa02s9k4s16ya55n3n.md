## OOP vs FP: A comparison using "unconditional" FizzBuzz

If you haven't read the subject of this article yet; check out the [original challenge](https://www.kallmanation.com/unconditional-challenge-fizzbuzz-without-if) and look over both the [functional](https://www.kallmanation.com/unconditional-fizzbuzz-a-functional-approach) and [object-oriented](https://www.kallmanation.com/unconditional-fizzbuzz-an-object-oriented-approach) approaches.

# Object-oriented programming and Functional programming

Remember the definitions of [Functional](https://www.kallmanation.com/unconditional-fizzbuzz-a-functional-approach) and [Object-oriented](https://www.kallmanation.com/unconditional-fizzbuzz-an-object-oriented-approach) that we developed in the previous posts.

- Functional programming uses functions to perform computation.
- Object-oriented programming uses the passing of messages between objects to perform computation.

To round out the definitions, let us also define what a "function" and what an "object" and "message" is:
- A function accepts input of a certain format (or none at all) and returns an output of another other form.
- An object is an enumeration of private state and public methods (aka messages it responds to). Each method accepts input of a certain format (or none at all) and combined with the private state returns an output of another form.

Those definitions are (intentionally) very similar. Now we can see how equivalent OOP and FP actually are. Imagine making objects with one universally named method that ignores private state; and we've essentially made a "functional" (but probably annoyingly verbose) language.

# The Similarity

%[https://codepen.io/kallmanation/pen/ExPKVOP]

Both these solutions use the same strategy for removing conditionals:

### 1. Define polymorphic booleans

_(Polymorphism here is just two or more things with potentially different implementations that satisfy the same interface; though there are other formulations and types)_

In functional style, that's two functions:
```js
const functionalTrue = (onTrue, onFalse) => onTrue;
const functionalFalse = (onTrue, onFalse) => onFalse;
```

In Object-oriented style, that's two objects (with some composition for sharing common code):
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

### 2. Use those booleans to construct logic

FP: 
```js
divisible_by_three(divisible_by_five("FizzBuzz", "Fizz"), divisible_by_five("Buzz", n))
```

OOP:
```js
this.three
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
    .evaluate()
```

# The Difference

Verbosity.

Even if you've been skimming up to this point, you've probably seen the OO code samples are _much_ longer than the FP code samples.

Some of you are now congratulating yourselves for yet one more reason FP is superior to OO. But hold the back patting for a minute...

We have gained something through that verbosity: more points of control. We can now better control the order of operations and select what data is cached (and when). Compared to the functional-style where caching and order of operations are up to the language implementation (or large changes in coding style).

Side note that the brain is a funny thing. Too verbose and the signal becomes lost to the noise; too terse and the mind becomes overwhelmed with the density of information. Keep it balanced no matter the programming style.

To be honest, I don't love either of these solutions in this regard. My Functional solution is too terse while the OO solution is too verbose.

# Fusion Dance

I believe the best code comes from a combination of ideals from Functional Programming and Object-oriented Programming:

### 1. Polymorphism

Useful from simple things like making lists of an arbitrary type to making full out boolean logic (as we've seen).

### 2. Composition

Both function composition and object composition. A fantastic way to (re)use code and disperse concerns into distinct areas.

### 3. Encapsulation / Abstraction

Some things are just implementation details. Keep a small, well-defined surface area between units, because bad interfaces cause most software problems (from my experience).

### 4. Avoid shared state

Sometimes sharing state is unavoidable. In those cases, have clear interface and ownership structures; especially control what modifies state, how, and when.

In all other cases, avoid shared state like the plague!

### 5. Purity*

Having no (or few) side-effects and side-influences makes logic much easier to reason through. Do it when you can; especially at the interfaces of abstractions.

_* Eventually, the system needs to have a side-effect: write to a database, display something on screen. Or for performance reasons as modifications can be faster in most languages. Do these intentionally, in controlled locations, and with as little logic intertwined as possible._

# The Conclusion

This article is based on my opinion and experience. I hope you think critically and take everything here with a few grains of salt.

I hope you've learned something from this series. And I hope I've shown that OOP vs FP isn't a question of which is "better" or "more-powerful"; just two different approaches with lessons to be learned from each.

Let me know what you think in the comments!