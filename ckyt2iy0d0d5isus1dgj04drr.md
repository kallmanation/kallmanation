## Handwritten Code

Now that I have your attention. No, do not hand write code (even in interviews). Besides that cover photo, the last time I wrote code by hand was in college.

Speaking of college, there's a particular page in a particular textbook I think about often. Not for its content, but rather as an analogy to a concept I struggle to express any other way.

I want to dig deep into that concept today. If you came for a squabble on scribbling out software; you may leave, this is not the post you are looking for.

First, the page I think about, page 56 of _Introduction to Graphics Communications for Engineers, Fourth Edition_ by Gary R. Bertoline (ISBN 978-0-07-352264-7)

![Page 56 of Introduction to Graphics Communications for Engineers, describing why legible writing is important in engineering with diagrams for each letter and number to draw them in a consistent and legible way](https://cdn.hashnode.com/res/hashnode/image/upload/v1643051639898/fmgCJ8aZyr.jpeg)

Look through those diagrams of how to write out letters and numbers. That's probably not the way you were taught your lettering and almost certainly not the steps you follow when writing today (I definitely didn't). But how legible is your writing?

# The Crafting _Process_ Directly Correlates to the Result's _Quality_.

The _steps_ and _tools_ used in making leave indelible marks on the _end result_. Really I'm just retelling [Conway's Law](https://en.wikipedia.org/wiki/Conway's_law):

> Any organization that designs a system (defined broadly) will produce a design whose structure is a copy of the organization's communication structure.

> — Melvin E. Conway

But now I have an analogy for _why_ Conway's Law is true. Look back at each of those letters; do you notice a pattern? Pen strokes are limited to going down and to the right (not strictly, but generally). No coincidence those are the directions a right-handed writer has most control over.

Could we write letters just the same without following such a stringent rule set? We could try. And we would have good results for a little while. But once we stopped focusing our handwriting would degrade once again back to our old pattern.

An organization _can_ ship a system designed differently than its own structure. This happens regularly. But it will quickly _devolve_ into matching the organization structure just as our handwriting devolves to the _natural_ forms our grip and strokes encourage. Changing the strokes taken makes it _natural_ to fall into that particular legible style. Changing organization structure makes it _natural_ to produce software of a particular style.

# Beware Labelling Every Cost as a Problem

Go try writing using those instructions for a minute. It will be very slow at first. After a few minutes you should have a little rhythm and won't need to look at the instructions much.

But this stroke pattern will always be slower than others. The added guidelines to only go down and to the right forces the hand to lift and move the pen more often.

This is a **_cost_** not a **_problem_**. Problem's can be solved. Costs are the requirement to obtain what we want. We want legible handwriting. The cost is writing slower to follow the guidelines.

I have often seen discussions start up around all sorts of "problems" looking for any manner of "solution" in the business of software. I can hear them now, "If we could go up and left, we could write letters so much faster!", but that is not a problem; that was intentional!

I truly believe that particular habit, more than anything else, locks larger and older companies into all the odd behavior, poor customer service, and lack of innovation we see of them. They have straight-jacketed themselves with decades of "solutions" to "problems". But those "problems" were just the price of being the best in the business!

# Monitor Output

I know of no way to truly predict the final results of changing a making process. We have gross heuristics (fist gripping a pen will be less precise than gripping with forefingers; agile-ish practices see better utility production than waterfall-ish practices).

But to truly know? Specifically? We must try it and observe the results. In software development, this means the practices of the team (agile, cross-functional team-members, red-green-refactor TDD, etc.). The output to observe being the code written (if the people deciding what practices should be used can't understand the code output*, you have a Big Problem™).

Is the code produced of a similar quality? Maintainable? Extendable? Bug-free? Robust to unexpected situations?

No? Congratulations, the "problem" you solved was actually the price of quality.

_* Don't forget understanding requires communication. Communication is a two-player game. If the bosses can't (or won't) understand the basic technical aspects of the software, you have a Big Problem™. If the code you've written is so poorly documented and described that the bosses can't understand it, you have a Big Problem™._

# Refine Practices

I am now attempting to describe a process of designing a process intent on producing better processes. It's rabbit holes all the way down. But here is the best advice I have found:

1. Constantly adjust, observe, re-orient, and re-adjust yourself
2. Observe others; are they achieving things by doing something "stupid" or "crazy" or "obviously wrong" that "makes no sense"? They probably aren't stupid or crazy or wrong... go back to point one and give it a try.

# Some Examples

## Testing

- **No Tests** - not writing any tests at all encourages a certain type of codebase to develop. Either you know what I mean or you are blessed to not know yet.
- **Test Everything Last** - at least we're writing tests now. Writing last though encourages the lazier developers (aka me) or the pushier product owners to skip some thoroughness in the test suite. Often leading to missing or broken tests.
- **Refactor-Green-Green** - tests are written intermingled with code writing. But often written post-code, meaning the writer never sees them fail. Intermingling leaves less chance for missing tests, but does not help much with broken tests.
- **Red-Green-Refactor** - a minimum set of tests are added, then a minimum code change is made to make all tests pass, then optionally the code is re-arranged into a more readable format. Tests written in this way often have good coverage and are fairly healthy. Tests can become disorganized though.
- **Test Everything First** - write them all; write them now. Tests have good coverage and are fairly healthy; also well organized. Testing this way has the drawback of being "waterfall-y", if something is discovered late in the process to be incorrect a huge amount of time is wasted re-writing tests.

## Release Cycles

- **Months Between Releases** - leads to a lot of formal processes, reviews, approvals, and change management. An incorrect feature release can be devastating, so rather than ship incomplete features, years are wasted waiting (and it needs to be redone anyway because the business has changed by the time it is released).
- **Weekly Releases** - time spent meeting cut drastically. Smaller portions of features are released. Changes are manageable and users don't mind waiting for next week for their request. Still some process to review ensuring a week won't be wasted.
- **Daily Releases** - almost no manual process around releases. "Planning" becomes a loose guideline as changes are requested and executed daily.
- **Continuous (or near-continuous) Releases** - manual processes nearly evaporate. All the time spent on them are re-invested in the actual building of software. Bug fixes are released within hours. Fear of making mistakes is gone because the consequences of mistakes are nearly gone.

## Team Organization

- **Technical Area** - back-end, front-end, database, etc. common technical concerns (e.g. poor DB performance, UI components) are easily dealt with but every new feature must go through multiple teams increasing cost and complexity.
- **Business Area** - invoicing-team, marketing-team, etc. new features have a clear, single owner cutting their development costs but cross cutting concerns fall into a "tragedy of the commons".


