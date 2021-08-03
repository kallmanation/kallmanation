## Level up your debugging skills! Putting the Science into Computer Science


Debugging problems differentiates successful programmers from the mediocre. 

The good news? It can be learned and improved! Just follow some basic processes to go from debugging zero to debugging hero!

We'll go over the process, but first, a warning...

# DO NOT JUMP TO CONCLUSIONS

Bugs exist because our assumptions are incorrect. We CANNOT use those same assumptions and hope to make any progress (except by sheer luck).

Act only on **facts** that we can **prove** with **real examples** and we are well on our way to better debugging habits.

The number one problem beginners' have debugging is working from conclusions that aren't based in reality.

# The Scientific Method (for debugging)

1. Formulate a Hypothesis based on the facts you currently know
2. Devise an experiment that could disprove the hypothesis
3. Execute experiment
4. Compare results to the hypothesis
5. If the experiment disproves the hypothesis; restart with a new one at step one
6. If the experiment does not disprove the hypothesis; accept it as a working theory, then:
    1. Use this working theory as a new fact to formulate a new hypothesis
    2. Start a new cycle at step one using that new hypothesis, until:
    3. You have a working theory that describes what portion of the system has a bug and under what conditions

And once you have that final working theory, you have your bug. Fix it! (and don't forget by doing this work you've found the conditions that reproduce the bug; better encode that into a test case to make sure this doesn't break again...)

I personally learn best by example; so let's work through a bug in my own code to a [dev.to daily challenge](https://dev.to/kallmanation/comment/12l2b)...

# An example

_(feel free to inspect element and follow along in the console there)_

The [challenge](https://dev.to/thepracticaldev/daily-challenge-273-remove-duplicates-33mn) was fairly simple:

Remove duplicates from an array of numbers, keeping the rightmost number for ordering.

So `[3,4,4,3,6,3]` should be deduplicated into `[4,6,3]`; `[1,1,4,5,1,2,1]` should become `[4,5,2,1]` and so on.

Here was my first stab at the challenge:
```js
let radixPush = (array, radix, value) => {
  array[radix] = value;
  return array;
};

let solve = (array) => array.reduce(radixPush, []).reduce(radixPush, []).filter((i) => i);
```

Let's try it out against the test cases in the post:

Tests:
```js
solve([3,4,4,3,6,3]) // expected to be [4,6,3]
solve([1,2,1,2,1,2,3]) // expected to be [1,2,3]
solve([1,1,4,5,1,2,1]) // expected to be [4,5,2,1]
solve([1,2,1,2,1,1,3]) // expected to be [2,1,3]
```

Running each of those returns what we expect. Challenge solved! ...right?

Well, I went over to the original [challenge on CodeWars](https://www.codewars.com/kata/5ba38ba180824a86850000f7), submitted my answer and... promptly failed 80% of the test cases.

Without even seeing those failed test cases, we can make progress towards fixing the bug with this scientific method. Let's start with a hypothesis:

## Hypothesis #1: numbers greater than nine are not handled correctly

Looking at the tests I ran manually, something suspicious catches my eye: they are all single digits. Maybe the problem is multi-digit numbers are not handled correctly at some point in my `solve()` function.

Next, step: test our hypothesis! This should be easy, just make an array with large numbers in it:

```js
solve([987654321, 123456789, 1, 123456789, 987654321]) // expected to be [1, 123456789, 987654321]
```

Running our test gives the expected result, disproving our first hypothesis. Back to the drawing board...

## Hypothesis #2: Zero is not handled correctly

Since numbers 1 and greater appear to be handled correctly; let's examine the last non-negative integer that hasn't been tested: `0`.

```js
solve([1, 0, 1]) // expected to be [0, 1]
```

Testing appears to confirm our hypothesis! Instead of the expected `[0, 1]` we've gotten just `[1]`; the `0` was dropped from our array.

But let's confirm these results with an even simpler test case:

```js
solve([0]) // expected to be [0]
```

Our suspicions are confirmed! `solve([0])` returned `[]` instead of `[0]`. We are incorrectly dropping `0` from our solution... but why?

Time to iterate our hypothesis into specific portions of the code:

## Hypothesis #3: zeros are dropped in the `reduce` steps

THe `solve()` method is just two parts: Two `reduce`s and a `filter`. Let's split the problem and start by hypothesizing the bug lies in the `reduce`s.

Defining a `solve_part_one` to test against
```js
let solve_part_one = (array) => array.reduce(radixPush, []).reduce(radixPush, []);
```

We'll give it our most simple test case that reproduces the bug:
```js
solve_part_one([0]) // expected to be [0]
```

Running the test gives the expected result. That leaves one last obvious hypothesis to test:

## Hypothesis #4: zeros are dropped in the `filter` step

```js
let solve_part_two = (array) => array.filter((i) => i);
```

And the test of our hypothesis:
```js
solve_part_two([0]) // expected to be [0]
```

Yeilds `[]` instead of the correct `[0]`. We have found the bug! If you're familiar with JavaScript, you already know what I've done wrong: JavaScript treats `0` as a falsy value...

```js
0 ? "truthy" : "falsy";
```

So, when zero is given to `.filter` my filtering method `(i) => i` returns `0`, which `.filter` sees as `falsy` and therefore an element to be removed from the output array. Fixing the bug should now be fairly easy. Account for `0` in the filter:
```js
solve = (array) => array.reduce(radixPush, []).reduce(radixPush, []).filter((i) => i || i === 0);
```

Testing the new and improved `solve()`
```js
solve([0]) // expected to be [0]
```

Gives the correct result! Bug resolved!

With no instructions on reproducing the problem and only a method to our madness, we have fixed our bug. (and submitting to CodeWars results in all green tests; yay!)

# A Method; not Mindless

There is still skill to be developed and applied here. Selecting a good hypothesis that quickly narrows down the potential problem while still being easy to test requires intelligence, experience, and creativity.

Try to select hypotheses that bisect the problem. For example, I could have formed the third hypothesis as expecting zero to be dropped in the first `.reduce`. However, by testing a larger portion of the code at once; we more quickly narrowed down the potential problem.

# When it doesn't work

There are still weaknesses to this process:
1. Some hypothesis cannot be tested (or are difficult to test)
2. You may not have enough knowledge of the system to form a hypothesis in the first place
3. The tests may appear non-deterministic because of influences you do not know exist (OS, Browser, something running in the background)

These are the cases to seek help from others. Ask someone more experienced with the system or Google or Stack Overflow. You may need to go through several iterations of questions; each answer refining your knowledge of what question you _should_ be asking to get the answer you need.

Debugging may still be frustrating at times, but having a toolkit should help you resolve more problems more quickly. Still sometimes, just walking away and returning with a fresh perspective does a world of good. Happy debugging!
