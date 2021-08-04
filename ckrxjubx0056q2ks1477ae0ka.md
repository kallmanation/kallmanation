## Learn Three Languages

_Cover photo by [Artem Kniaz](https://unsplash.com/@artem_kniaz?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/stacked-rocks?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText)_

---

These thoughts I've had reading the thousands of `Which language should I learn first?` posts crystalized after a [comparison to human language](https://dev.to/jana/is-coding-a-lenguage-like-english-or-portuguese-519h) (for some reason). So here's my insight for today:

# Don't Learn One Language; Learn Three

Why three? Isn't that a lot for a newbie? Maybe. And maybe learning three "at the same time" means focussing on one each for a month or more. But I believe the biggest improvements to my individual programming ability came when I was exposed to different languages and their paradigms. I wish I had done this sooner.

Three should be enough to cover some major distinctions in the language world:

# Functional vs Object-oriented vs Procedural

The big one everyone talks about.

Procedural being the simplest and first, reading top to bottom if this then that then regardless another thing and lastly a finishing statement. Not much more than assembly languages.

[Object-oriented](https://kallmanation.com/unconditional-fizzbuzz-an-object-oriented-approach) where Objects both hold and organize the data while controlling the program flow.

[Functional](https://kallmanation.com/unconditional-fizzbuzz-a-functional-approach) where functions control the program flow.

_(Even I talk about these; I have an [article detailing the differences](https://kallmanation.com/oop-vs-fp-a-comparison-using-unconditional-fizzbuzz) in my opinion)_

Pick languages to learn each of these styles. For example, bash (or any shell script) is heavily procedural; Java is an object-oriented language; and Haskell is a functional language. Or languages like JavaScript that can be written in all three (if choosing one with multiple paradigms; make sure to spend time writing in only one style to learn about it before mixing styles).

# Strongly typed vs Loosely typed

The second big one. Strongly typed languages require types be declared and used correctly according to the language. Loosely typed languages allow types to be coerced and change on a dime `"2"` can equal `2` and `null` is treated like `false` in boolean conditions.

Learn each of these to see their advantages and disadvantages.

# Strict vs Lazy evaluation

The difference less discussed. Strict evaluation (the much more popular option) has programs executing every expression in preparation for the next. Lazy (aka non-strict) evaluation only evaluates expressions when needed (which means it can operate on "infinite" data, as long as the operation yields a finite result).

Learn languages (or at least techniques in a language) that expose the difference. Haskell is the often cited example of lazy evaluation.

---

Learn three languages, because the opposing ideas from each paradigm helps when writing in the other.

After all this, some of you are just looking for a shortlist of languages to learn. I'll temper this by saying the best languages to learn are the ones you are interested in. But here's my list:

1. [JavaScript](https://medium.com/coderbyte/50-resources-to-help-you-start-learning-javascript-in-2017-4c70b222a3b9) - Object-oriented, Functional, and Procedural; loosely typed; and strictly evaluated. Truly massive breadth of applications and depth of material to learn it with.
2. [C#](https://docs.microsoft.com/en-us/dotnet/csharp/getting-started/) - Object-oriented (in a different way than JS); strongly typed; and strictly evaluated. A good dual to JS while still being widely used.
3. [Haskell](http://learnyouahaskell.com/introduction#about-this-tutorial) - Functional; strongly typed (in a different way than C#); and lazily evaluated. The one that will break multiple "common" concepts learned from JS and C#. May be difficult to use in a professional setting, but the lessons learned will transform the way you view any other language you program within.

