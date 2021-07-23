## No/Low Code - Why hasn't it "won"?

# No Code / Low Code

It's not a new idea. (despite what their sales pitches say) 

The first [HTML WYSIWYGS](https://thehistoryoftheweb.com/moment-time-editors/) appeared in the 90s. 

[COBOL](https://www.bushido.codes/cobol-lang) was designed as a high level scripting language for business people, not engineers (COmmon **Business Oriented** Language; probably the first "low code" attempt). 

Visual coding languages have been around for decades. 

And now there seems to be an explosion of startups trying to make the latest, greatest [n|l]o(w) code platform that will "revolutionize the way apps are built" and "empower citizen developers and non-technical people" and "make traditional developers unnecessary". 

But they all sound like a new cover on the same story.

# Why haven't they won yet?

So if these ideas have had 30 to 60 years to develop; why are there still so many "traditional" developers running around, typing text at their computers all day? Looking at the modern offerings, they all advertise themselves around one (flawed) idea: syntax is the hard part of programming.

### Syntax is not the hard part of programming

Let's say that again. _Syntax is **not the hard part** of programming_. I argue it is actually one of the easiest. (Not to say it is easy; like any language it needs to be learned over time.) Calling syntax the hard part of programming is no different than calling spelling the hard part of writing. True that the project won't get far without it, but far more important things lay beneath the surface. It's just the tip of the iceberg.

They haven't won because they solve a non-problem while sacrificing solutions to more difficult aspects of software development.

# What's actually hard about software (that no/low code platforms don't do)?

## 1. Testing

How do "traditional" developers gain confidence that each change they make:
  1. Actually does what they want it to do; and
  2. Didn't break something else already built.

Tests.

Testing. Lots of testing. Testing each unit in isolation. Testing the integration of several units working together. Testing everything from start to end. (also things like type-checking and linting provide a base level of confirmation that the software is assembled at least reasonably, if not correctly; but mostly testing)

For most no or low code options the only "testing" that can be done is by a human manually going through the app.

## 2. Version control (and collaboration)

How do entire teams of software developers work on the same project every day without constantly accidentally undoing each others' work? Few practices are universally accepted in software; [`git`](https://git-scm.com/) is one of the few found on almost any job requirement.

If a no/low code platform does offer versioning it often takes the form of a simple list of "revisions". These offer versions; but none of the control. When more than one or two people work on the software the "versioning" offered will quickly break down.

## 3. Non-proprietary or specialized viewing/editing

Take any program written in Python (or Javascript or C). With even the most rudimentary text editor I can view (and change) software written in these languages. I don't need to install `JavascriptBuilder 12` or visit `use.python.org`. Even with the lowest powered device without consistent internet access I can still create software.

This means if I believe I have a better way to write software I don't have to wait for a release from the language maintainer, I need only make my own editor or utility to do it.

It also means anyone can post snippets on Gists or StackOverflow or their personal websites to reuse, document, or pattern off of later.

## 4. Managing complexity

Software has unlimited complexity. Unlike anything else people have made that are bounded by these three physical dimensions; there is nothing constraining software except the minds of those making it.

### This is the hard part

This is what employs the developers, programmers, coders, hackers, and software engineers around the globe.

This is what commands those eye-watering salaries.

This is why we make new languages; new libraries; new package managers; new frameworks; new version control systems; new development environments.

This is the heart of every principle, best practice, and programming paradigm.

This is the primary vocation of those writing software.

_How in this world do we manage all this complexity?_

This cannot be solved by accident. The solution cannot be reached by the accretion of new features and functionality. 

This is why no code and low code lose.

They do not improve on the heart of software creation. In fact, while solving trivial problems at the edges, they often make the hard part harder.

# So how can no/low code win?

Solve the complexity. It could be written in Klingon saved to jpegs; if it better manages the complexity of modern software systems it will be used.