## Σ(Reasonable Requirements) != Reasonable System

Beware! The sum of a few reasonable requirements does not result in a reasonable system; or as I like to call it, reason number five everyone (including me) is bad at system design.

Don't believe me? Just watch:

_Note this example comes from a real system I've worked on. Details have been changed._

Envision a project tracking system. It has an area for estimating how much longer a project will take... reasonable?

# Reasonable requirement #1: Allow changes to the estimate

Very reasonable. The people working the project will have the most knowledge and should be able to record that into a better estimate.

If new challenges or risks are introduced they should be able to increase the estimate. And as things wrap up more quickly than expected, they could decrease the estimate.

I would expect this of even the most rudimentary estimate tracker. This has to be in the software!

# Reasonable requirement #2: Decrease the estimate as people track time on the project

This is a smart system we're building after all. We know when an hour has passed on the project, so we know that the project should be an hour closer to completion making the estimate an hour less than before.

Obvious requirements are obvious. Let's add this to make our users' lives a little easier.

# Reasonable requirement #3: The estimate shouldn't go below zero

What would that even mean? I don't like the idea of negative time; especially when you're telling me how much remaining work there is.

No negative estimates. Reasonable.

# Reasonable requirement #4: Adjust the estimate back up with any misrecorded time that's removed

An obvious extension to the very reasonable requirement #2. We know people make mistakes, so we let them reallocate time recorded (or remove it entirely) for when they start the wrong timer.

Given that, let's be nice and readjust their estimate on these corrections. We don't want to meanly change their estimate incorrectly and just leave it right?

# (un)Reasonable System

As the good little code monkeys we are; we take these requirements and code them up into our quaint little "dark mode" editors; queue the build and release magic and everyone's happy! Until... Until the day comes when **The Edge Case™** comes. Here's how it all went down:

Pete has a project for Pumpkin Protractors.

Pete prophetically set his estimate for three hours to perfect the Pumkin Protractor project performance.

Two hours down and our impeccable system performs the perfect adjustment to the Pumpkin Protractor project estimate.

Pete perfunctorily begins another timer for the Pumpkin Protractor project.

After placidly performing two hours of non-pumkin related productivity training; Pete returns to the Pumpkin Protractors project.

Pete notices the project estimate is imperfect! The shock and horror. But no problem, Pete pessimistically changes the project estimate for two hours remaining on Pumkin Protractors.

Pete now sees the problem present: the timer. Pete changes the time from Pumpkin Protractors to the productivity project. 

And we... we... wait, **what are we supposed to do here?**

| Event | Estimate |
| --- | --- |
| Pete sets the estimate to 3 | 3 hr |
| Pete records a 2 hour timer | 1 hr |
| Pete records another 2 hour timer | 0 hr |
| Pete sets the estimate to 2 | 2 hr |
| Pete removes his second 2 hour timer | ? hr |

The estimate probably shouldn't be adjusted to 4 hours (naive rule #4); but maybe it should be 3 hours (rule #4 + correction for #3)? or should it stay at 2 hours (rule #1)? That is what Pete set it to most recently. Or just change it to 1 hour for some reason (rule #2?)?

We have made an unreasonable system from only reasonable requirements. As written, we literally cannot reason about what should happen; the system is un-reasonable.

---

# The Challenge

Add, Modify, or Remove requirements to make the system "work" (whatever that means). Descriptions, diagrams and psuedo-code welcome.

<details>
<summary>
So what did I do?</summary>
I added another rule: Any changes to timers recorded before the last time the estimate was manually set have no effect.

This rule prefers the judgement of humans over the machine and prefers to keep estimates low rather than "unexpectedly" grow. Other rules are possible though.

So for this example the estimate will remain at 2 hours in the last row. The system has assumed Pete has the best knowledge of the future and has framed that knowledge in a statement irrespective of what corrections to the past need to be made (rather than a statement inclusive of potential corrections of history).
</details>