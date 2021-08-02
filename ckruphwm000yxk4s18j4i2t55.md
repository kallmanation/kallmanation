## Unconditional Challenge: FizzBuzz without `if`

As the title says, make a classic FizzBuzz function or method without using `if/else` (or equivalents like ternaries, `?:`, sneaky).

Specifically:

1. The function should accept one argument, assume it will always be a positive integer.
2. The function should return a string (or something coercible into a string in loosely typed languages) according to the following rules:
    1. If the given number is divisible by `3`, then return `Fizz`
    2. If the given number is divisible by `5`, then return `Buzz`
    3. If the given number is divisible by both `3` and `5`, then return the combination `FizzBuzz`
    4. If the given number is none of those things, then return the given number

The expected outputs for the first fifteen numbers in order is:
```
1,2,Fizz,4,Buzz,Fizz,7,8,Fizz,Buzz,11,Fizz,13,14,FizzBuzz
```

# Hard mode

Do this without "secret" conditionals like `||` and `&&` (in loosely typed languages like JavaScript) or `null` coalescing operators like `??` or `null` safe operators like `?.` or `&.`.

Also no looping constructs that could be abused into a conditional like `while` or `for`.

# Hint number 1

I tagged `functional` on this post because functional programming can be used to solve this.

# Hint number 2

I tagged `oop` on this post because the original object oriented concepts of message passing can be used to solve this.

---

Post below with your answers!

Think this is impossible? I'll post my own answers in both FP and OOP styles next week.