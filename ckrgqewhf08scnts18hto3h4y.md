## Dear Slack, Simple is not what you think it is

# Simple

A word bandied about in development circles. Make it simple! That should be simple. We need something simple.

But what does **simple** mean? [The dictionary](https://www.merriam-webster.com/dictionary/simple) gives us some interesting words to chew on:

> - readily understood or performed
> - free of secondary complications
> - not limited or restricted

If something is already complicated and you set out to "make it simple", you are immediately faced with a conundrum: 

- merging secondary concepts into primary ones reduces the surface area, but also complicates those concepts; 
- splitting concepts makes each one easier, but makes the overarching system harder to understand; 
- removing functions makes it easier to understand, but also adds restrictions, and those limitations means the system is no longer a simplified version, just a less useful system.

# Simple is hard

There is no simple way to make something simple if it is not already that way.

It involves transforming a complex concept into an equally powerful construction that is also easier to understand. Let me say that again. To "make something simple" requires three steps that are each incredibly difficult:

1. Understand a complex concept.
2. Reformat one concept into another equal, but distinct description.
3. Discover, among all the equal-but-distinct constructions, one that is simpler than the original.

# Simple is disrespected

By definition, if you've successfully gone through the process above, you have created something simple, easy to understand, powerful. When explained, it is understood. And why wouldn't be, it's so **simple**!

Those who do not know the process, who have not felt the pain, believe that **simple** is **easy**. It was **easy** to understand after all. Even explaining the original complexity has no effect; they easily draw parallels to the complex description from the simple one already understood.

It is **easy** to follow a mental path laid down by another. It is **hard** to blaze a new path through the mind.

# How to measure Simple

And now we come to the prompter of this post, Slack. If you don't know, they've [updated recently](https://slackhq.com/simpler-more-organized-slack) with the moniker of being "simpler". Here's a short clip of their full "transformation" animation:

![Simpler Slack Animation](https://cdn.hashnode.com/res/hashnode/image/upload/v1627068227142/bHyTaV7hG.gif)

Did you see that? Go read the full post for more clarity. Most of the changes to Slack were _additions_ of features. Let's just come out and say it:

**You cannot make something simpler by adding functionality.**

Now, I actually quite like some of these new features (grouping channels anyone?). But they have not made Slack _simpler_. Slack is quite possibly at its most complex and feature dense in its history.

The first facet to measure something's complexity is the **number of concepts** need to be understood at once (or in a UI, how many are presented at once). so what does Slack have? Starting in just the left sidebar:

1. The "meta" menu (profile, preferences, customizations, etc.)
2. New draft
3. Unreads
4. Threads
5. Mentions & reactions
6. Drafts
7. Saved things
8. Channels (browser)
9. People (browser)
10. Apps (management)
11. Files
12. Channels (conversations)
13. People (conversations)
14. Apps (messages to them; not to be confused with 10)

Now, working memory only holds a limited number of things. Anything simultaneously presenting more than what working memory can contain immediately feels much more complex. It's generally accepted that people can hold 7 arbitrary "things" in working memory ([though some measurements say as little as 4](https://en.wikipedia.org/wiki/Working_memory#Capacity)). I'm not a math genius, but I'm fairly certain that 14 is larger than 7.

The second facet of complexity comes in [**connascence**](https://connascence.io/), or how connected the number of concepts are. The more interrelated concepts become, the less simple it is. Let's go down our list again, this time listing out the connections

1. none
2. 6, 12, 13, 14
3. (3, 5 sometimes), 12, 13, 14
4. 3, (5 sometimes), 12, 13, 14
5. 3, (4 sometimes), 12, 13, 14
6. 2, 12, 13, 14
7. 11, 12, 13, 14
8. 12
9. 13
10. 14
11. (7, 12, 13, 14 only moderately)
12. 3, 4, 5, 6, 8
13. 3, 4, 5, 6, 9
14. 3, 4, 5, 6, 10

Do I really need to sit here and tell you how many connections there are? Alright, there's a lot of conceptual connections!

The third facet of complexity is something I usually call **density**; essentially a measure of how much meaningful information per unit is conveyed. In audible and written communication this is how much meaning is hung on each symbol or sound. In visuals this is how differently something should be interpreted for how little visual distinction there is.

Density is much harder to quantify and is much more subjective. Some people come to prefer much denser expressions while others need more space. The best way to tune density is to solicit the response of as many people as possible. The extremes of both sides will tend to guide density into a goldilocks zone of not too much and not too little.

# How to make something Simpler

We've already discussed how to deal with **density**. Fortunately it tends to be fairly easy to move between more and less dense constructions. At least easier than moving **connascense** and **count**.

There are a few tricks though to moving the needle towards simplicity:

### 1. Nesting

Make one general concept a representative of several more nuanced concepts. Here, Slack gives an excellent example in their channel grouping feature. By allowing the user to create their own sections, they allow them to make their own nesting scheme that best suites their concepts.

A good nesting has been made when none of the nested concepts needs to be specifically thought of when relating to other general concepts. (No connascense allowed between non-sibling concepts).

### 2. Grouping

Keep concepts with higher connascense closer together. Easy to say, not easy to do. This can be made more effective by combining with the next trick:

### 3. Reformulating

Separate and merge concepts into new concepts so that their connascense is minimized (both degree and strength). Be careful of falling into mental traps. We easily believe two concepts are separate only because we've only thought of them separately. We easily believe one concept goes together when it actually should be split and absorbed by others.

# Actually making Slack simpler (in my humble opinion)

We've seen the metrics of simplicity and learned the tricks of the trade. Let's see if we can't apply these to make Slack simpler. Starting at the example of the side-bar:

1. The "meta" menu can stay. It already nests other concepts nicely.
2. New drafts, Unreads, Threads, Mentions & reactions, Drafts are all words for the same thing: Activity. Let's make that the title and group concepts below this:
  1. Unreads & reactions
  2. Threads
  3. Drafts
3. Saved things can stay. It's fairly unconnected.
4. Let's move Files right here as it is most conceptually related to Saved things and somewhat related to channels/people/apps.
5. Merge all normal conversations (both people and channels) and the browsing for new conversations into one place: Conversations. There is little difference between the two, and with the sections functionality the user can freely reorganize them to what best suites their needs.
6. Apps (both management and messages)

And there we go! We're now under the accepted magic number of 7 and greatly reduced connascense.

But what do you think? Do you disagree? Leave a comment!