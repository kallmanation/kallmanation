## Double-entry Bookkeeping for Programmers

This is the article I wished existed when I needed to support accounting functions. Double-entry Bookkeeping may seem esoteric and unnecessarily complicated, but actually describes a simple framework for maintaining a correctible and auditable record of state changes.

This is not a practical guide on how to actually do modern accounting. It only looks at some mechanics of Accounting, not practical operating concerns.

# Defining words

**Credits** and **Debits**. Forget anything you think you know about what these words "mean". One of them isn't "positive" or "negative". Neither are "good" or "bad". They may as well be called left and right. They don't "mean" anything. They are only mirrors of one another. They only have words assigned to know their chirality.

Next up, **Accounts**. A confusing word from thousands of years of accounting. But I'll forgive them, naming things is hard. If a programmer were inventing accounting it would probably be called a type or category or collection. An Account is just a bucket giving a name to some money (distinguishing it from the other money being accounted for... oh wait, maybe that's why they're called accounts... who knew).

**Accounts** come in two flavors: **Credit-based** and **Debit-based**. Describing which way all the Credits and Debits against the account should be added up. Credit-based Accounts subtract the sum of Debits from the sum of Credits (Σ Credits - Σ Debits). And following chirality, Debit-based Accounts subtract the sum of Credits from the sum of Debits (Σ Debits - Σ Credits). Note in accounting practice accountants often subdivide these two flavors into [~5 main accounts](https://en.wikipedia.org/wiki/Double-entry_bookkeeping#Accounting_equation_approach) which are put into [the accounting equation](https://en.wikipedia.org/wiki/Accounting_equation). By convention, the asset-ish accounts are Debit-based and liability-ish accounts are Credit-based. Again, this article is not meant to delve into accounting practice; knowing the fundamental mechanics suffices for today.

To summarize:

- **Credit:** The opposite of a Debit
- **Debit:**  The opposite of a Credit
- **Account:** A label for certain Credits & Debits to be summarized together
- **Credit-based Account:** Account whose Total = Σ Credits - Σ Debits
- **Debit-based Account:**  Account whose Total = Σ Debits - Σ Credits

Hopefully Double-entry Bookkeeping just became slightly more approachable. Slightly. Three words in five concepts form our core. But now let's actually use these things...

# Double-entry Bookkeeping is NOT a Table

Crack open any accounting book or introductory blog post. Not long into the text there will be tables looking like this:

| Account | Credit | Debit |
| ------- | ------ | ----- |
| Cash    |     42 |       |
| Loan    |        |    42 |

This is how double-entry bookkeeping gets its name; it takes two entries to record a single action. It may also be the most unintuitive way to present financial activity. 

A layman's description would be that $42 was taken from Cash and given to (or paid to) the Loan. To and From... hmmm, let's rewrite that table:

| From Account | To Account | Amount |
| ------------ | ---------- | ------ |
| Cash         | Loan       |     42 |

This format hints at a new way to break our thinking free from our table-based accounting mindset. What if we thought of our record as a graph?

# Double-entry Bookkeeping is a Graph

Make each account a node and each credit/debit pair a single arrow (aka directed edge; credits/debits start/end the "arrow" respectively) with the amount (aka weight) and each account (aka node) involved. Here's what that weighted directed graph would look like:

![A graph with two nodes, labeled Cash and Loan, with an arrow pointing from the former to the latter labeled $42](https://cdn.hashnode.com/res/hashnode/image/upload/v1653850418582/5uiDAZ26B.png align="left")

Now you may have heard the rule of Double-entry Bookkeeping: "Every Credit must have a Debit and every Debit must have a Credit". Obviously, an arrow always has a start and an end (or to and from). So the graphical form of Double-entry Bookkeeping makes following the rules inescapable.

But how do we represent external movement of money? We do get paid for our work after all. Not only that, but I made five whole dollars from this blog. For a moment, let us relax that rule and let money come from nothing into our Cash account.

![A graph of two nodes, Cash and Loan, with two arrows coming from nothing entering Cash labeled $1000 and $100; also still having the arrow between Cash and Loan labeled $42](https://cdn.hashnode.com/res/hashnode/image/upload/v1653851081168/zEz5Wsi56.png align="left")

This works, but there is some unhelpful lack of information here. If we back this graph into a table, we might label the "from" account of those two new actions as `null`.

| From Account | To Account | Amount |
| ------------ | ---------- | ------ |
| Cash         | Loan       |     42 |
| null         | Cash       |     100 |
| null         | Cash       |     1000 |

We don't have to label it as `null` though. In graph theory, we could label `null` as anything and the graph would be equivalent. Choosing something appropriately vague, we can draw an equivalent graph of our situation looking like this:

![A graph of three nodes, Outside, Cash, and Loan, with two arrows between Outside and Cash labeled $1000 and $100; also still having the arrow between Cash and Loan labeled $42](https://cdn.hashnode.com/res/hashnode/image/upload/v1653851100310/dsG6qMSH3.png align="left")

While these accounts are not particularly helpful (why did I get $1000? why the $100?) we have answered the question of how to represent money moving to and from external sources. Simply make up a new node (aka account)! Instead of one, vaguely named, "Outside" account, make one for each source we need to account for. And to round out our example, buy some food and set aside some money for later:

![A graph of six nodes, labeled Employer, Side Hustle, Cash, Savings, McTaco King and Loan, with an arrow between Employer and Cash labeled $1000, an arrow between Side Hustle and Cash labeled $100, an arrow between Cash and Savings labeled $50, an arrow between Cash and McTaco King labeled $12, and an arrow between Cash and Loan labeled $42](https://cdn.hashnode.com/res/hashnode/image/upload/v1653851135064/at828bF-k.png align="left")

Representing external parties as their own nodes brings some interesting qualities: Adding up our Employer and Side Hustle accounts results in total income while looking at them separately tells me which pays better. Same for expenses (McTaco King's looking a little over-visited); but also internal money maneuvering. Keeping an emergency fund, for example, can be viewed as a "loan" that is "repaid" when the fund is used and rebuilt; graphically there is no difference between a loan from an "internal" account and an "external" account (besides interest; I gave myself quite the deal).

# But Keep the Double-entry Book in a Table Anyway

While it is useful to _think_ about our accounting as a graph it will not be useful to _see_ our accounting as a graph after a year when we have 24 edges between `Employer` and `Cash` and 200 between `Cash` and `McTaco King` (don't judge me). 

How about we backtrack our graph into tabular representations and see what we get out of it:

| From Account | To Account | Amount |
| ------------ | ---------- | ------ |
| Cash         | Loan       |     42 |
| Employer     | Cash       |   1000 |
| Side Hustle  | Cash       |    100 |
| Cash         | McTacoKing |     12 |
| Cash         | Savings    |     50 |

Being a table, shoving this data into SQL (or CSV or Excel) becomes trivial. But this format takes a little trickery to group and add up the account values correctly. If we return to the "classic" table form:

| Account     | Credit | Debit |
| ----------- | ------ | ----- |
| Cash        |     42 |       |
| Loan        |        |    42 |
| Employer    |   1000 |       |
| Cash        |        |  1000 |
| Side Hustle |    100 |       |
| Cash        |        |   100 |
| Cash        |     12 |       |
| McTaco King |        |    12 |
| Cash        |     50 |       |
| Savings     |        |    50 |

We can easily total this table's accounts using either `sum(Credit) - sum(Debit)` or `sum(Debit) - sum(Credit)` and grouping by the account. But we have traded the easier reading of what any one action has done by needing two lines instead of one for this easier aggregation.

_Rabbit hole: if you want some psuedo-SQL on how this second table could be made from the first, it would probably look like this:_

```sql
SELECT
   FromAccount AS Account,
   Amount AS Credit,
   NULL AS Debit
FROM actions

UNION

SELECT
  ToAccount AS Account,
  NULL AS Credit,
  Amount AS Debit
FROM actions
```

_And what trickery queries these two tables for aggregation? Consider these two queries, which is simpler?_

```sql
-- To aggregate the "classic" table for the Cash account
SELECT
  SUM(COALESCE(Debit, 0)) - SUM(COALESCE(Credit, 0)) AS CashTotal
FROM actions
WHERE Account = 'Cash'

-- To aggregate the former table for the Cash account
SELECT
  SUM(
    CASE
      WHEN ToAccount = 'Cash' THEN Amount
      ELSE -Amount
    END
  ) AS CashTotal
FROM actions
WHERE ToAccount = 'Cash'
OR FromAccount = 'Cash'
```

What format should your financial system store these in? The answer (like many things) is it depends. But the beauty of our modern computing machines means we don't have to choose! We can store our history in one format and automatically transform it to any other whenever we find it convenient.

# Why Use Double-entry Bookkeeping at All?

You've read this whole article and have a better grasp of _what_ Double-entry Bookkeeping is; but I haven't really spoken to _why_ we want to use it. Sure we need to store history to report our finances and make sure we aren't going broke, but why not just keep a snapshot of each account's value at each change? We'll even record which external thing made the change, so we should be fine and can answer all of our accountant's questions, right? I'm here to tell you **don't do that**. **Please do not do that**. Double-entry Bookkeeping has been independently invented and re-invented across multiple cultures spanning up to 2000 years! Those inventors found multiple benefits from writing this way:

1. Double-entries protect against mis-writing
2. Double-entries protect against mis-reading
3. Double-entries protect against arithmetic mistakes
4. Double-entries protect against damaged or incomplete records

But all these advantages I believe are mostly erased by modern databases and computers; automatic-checksums and data integrity algorithms, automatic calculations that never mis-read or make mistakes with built-in error-correcting codes, machines counting cash, machines reading checks, machines communicating financial actions directly with one another. But I see benefits that still make record keeping this way a good idea:

1. Correcting historical mistakes
2. Hypothetical alternate histories
3. Hypothetical futures
4. Flexible storage and migration options

Breaking news: people still make mistakes. I guarantee that one day the accountants will come to you and say they've mis-recorded something and now they **need** it changed and that correction **needs to be in the past**. What will we do if we've only been keeping a snapshot of the account value at each step? Sure we insert the one new value in the past, but that makes **all the subsequent history wrong!** So then we have to correct **all** the ones following that correction; don't forget these accounts are live and being updated constantly. Good luck with that.

In a double-entry system, resolution is child's play: find the mistaken action and insert another action with the exact opposite to and from (or credit and debit) and mark that inserted action as happening at the same time as the original. Poof, it's like the mistake never happened (and yet, when asked, we can confirm that indeed that mistake was made and corrected appropriately).

And by its nature of never using mutation ("updates" are done with the insertion of new records), Double-entry Bookkeeping makes moving our data about simple. Have SQL and want NoSQL? Just copy the rows into documents and done. Things changed since the migration? Just check for the new records and copy those. Export to Excel; no problem. Even a text file with lines appended to it satisfies the requirements of our storage needs.

By making our data easy to migrate, we can copy data into a staging environment with  hypothetical records to test fixing history or to test potential futures. All made possible by Double-entry. And to those of you saying "just add X" or "modify Y" to a snapshot history, congratulations, you are the latest in a long line to re-invent double-entry bookkeeping. But go ahead, leave that comment, as if you needed my permission.

_(Psst, if you want to practice these accounting principles, I am accepting donations at my [Buy Me a Coffee](https://www.buymeacoffee.com/kallmanation). Here's a little graph of what happens: `[You]-$4->[Me]`)_

# Additional Reading

- [Accounting for Computer Scientists](https://martin.kleppmann.com/2011/03/07/accounting-for-computer-scientists.html)
- [On Double-Entry Bookkeeping:
The Mathematical Treatment](https://arxiv.org/pdf/1407.1898.pdf)
- [Wikipedia: Debits and Credits](https://en.wikipedia.org/wiki/Debits_and_credits)
- [Wikipedia: Accounting equation approach](https://en.wikipedia.org/wiki/Double-entry_bookkeeping#Accounting_equation_approach)
- [Wikipedia: Accounting equation](https://en.wikipedia.org/wiki/Accounting_equation)
- [Wikipedia: Double-entry Bookkeeping history](https://en.wikipedia.org/wiki/Double-entry_bookkeeping#History)