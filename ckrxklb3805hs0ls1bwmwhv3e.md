## The Conflicting Requirements of Voting

_Cover photo by [Element5 Digital](https://unsplash.com/@element5digital?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/vote?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText)_

---

To non-technical people it may seem odd that voting is not just done from a mobile app or website. All other sensitive activities like banking, paying bills, insurance, and medical records are all available and interacted with through these mediums.

So why is something as "simple" as voting not done this way? Let's look through some requirements that all voting systems have:

# Not everyone can vote

Now hold on! Put down those pitchforks. I'm not talking about racist/sexist/other-ist disenfranchisement; "Everyone should vote" ... except small children (my 9 month old shouldn't be asked if they support Trump or Biden), except deceased people, except fictional characters, except people living in different countries or states...

So while I believe "everyone" should have the right to vote. There is a distinct, discrete list of people who may vote.

# Each voter has a limited number of votes

It's only fair that each voter has a limited (often to one) number of votes. Otherwise the "election" is just a competition of who can stuff the ballot box more than the others.

# Any one voter's vote cannot be traced to them

The integrity of the vote must be impeccable. If anyone knows how voters have voted they can bribe or coerce the voters. And anyone who dares vote against will face the consequences.

In no way can a particular vote be traced back to a particular voter.

_(this is why there was a big deal made around voters "choosing" to take pictures of their ballots)_

# A single average voter should be able to confirm the results of the whole vote

(At least to a reasonable degree)

This may require some level of trust in some organization's collection of data (as a single voter may not have the resources to do primary investigations on all votes). But any one voter should be able to confirm that both their vote and the votes of their peers are accurately reflected in the final tally.

Anything less and the voters cannot be sure that they actually voted and did not merely participate in a theatrical performance.

# The Conflict

So here we have four rules:
1. Only some persons can vote
2. Voters have limited votes
3. Anonymity in source of votes
4. Easily understood and confirmed by an average voter

Each are reasonable (and necessary). But if you've read my [previous article](https://dev.to/kallmanation/reasonable-requirements-reasonable-system-efp), you'll know this doesn't guarantee a reasonable system.

But let us start implementing anyway:

The first rule can be easily solved with a simple user account system. Identity services are the foundation of any online activity with well established best-practices. Have all eligible voters register in the system. At simplest, if a voter is physically capable of accessing their account then they are allowed to vote. This already handles deceased persons, persons too young to vote, and fictional persons; a gatekeeping mechanism would be needed for blocking registration from aliens, but for this most states already have confirmable records of people living within their borders.

A simple addition of attaching each voters' vote record to their account handily solves rule two. If a voter's account already has a vote associated to it then they cannot vote again.

And this also solves rule four. While the average voter may not understand every technical detail; in general this system can easily be explained to them and the system's behavior confirms it to the voter.

**But** we have absolutely destroyed rule number three. There is no anonymity in this system. We might visually hide things to some voters, but at the core the people operating this system (could) know how each voter voted.

Let us try again:

Starting at the third rule. Make all votes anonymous! But now how do we limit any individual voter's number of votes? We don't really know how many times any person has voted because everyone is anonymous.

We need votes to be both anonymous and not anonymous...

So we wave the magic wand of tech and somehow solved rules one through three!

But is the system we've created easily explained to an average voter? Enough to give them confidence it works correctly and we aren't just pulling the wool over their eyes? Doubtful.

# The Resolution

Physical objects! Yes, I believe voting is one problem computers have not (really) solved (yet).

Physical objects however have shown quite useful in solving this problem. A physical object can be made such that any person has great confidence it was made by the authority another person claims made it (see modern currency).

These physical objects can then be distributed by whatever body determines who may vote and how many times they may vote.

The physical object can then be marked or changed in a way to indicate the voter's vote. As long as the voter leaves no personally identifiable marks on the object and the object is not observed in the voter's possession, there will be no way to trace the vote to the voter, making it anonymous.

However, because the object can be confirmed to have been created by the authority and that authority only issues these objects to people allowed to vote and in the amount they are permitted to vote in, anyone can confirm the vote has not been tampered with.

Also, any particular voter can be confident their vote will be seen by those counting in the way they intended, without distortion (that's just how physical objects work; what you see is what you get).

And so physical objects can solve this dilemma remarkably well. Perhaps one day a system based in computation will be able to solve it as well; but until then, use a physical object to vote.

---

_Psst, if you like posts like this, consider supporting me by [buying me one pixel](https://www.buymeacoffee.com/kallmanation) closer to a new monitor._
