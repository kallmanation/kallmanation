## The Git Interpolator!

If you're anything like me, when you use command line `git`, you find yourself looking at something like this more than you'd care to admit:

```bash
$ gi tpull
bash: command not found: gi
```

# D'oh!

How many times do I type `git` in a day? For three simple letters, you might think it would be easier to type.

You would be wrong.

You might also be thinking that this isn't really a problem; to which I say, "Wrong again". If I take 3 seconds to fix this, and make this mistake 10 times a day, [Mr. Munroe tells me](https://xkcd.com/1205/) I can spend a whopping 12 hours fixing this and still break even!

# Just give me teh codez

Okay: [github/kallmanation/gi](https://github.com/kallmanation/gi#installation)

# How does it work?

### 1. `aliases` 

These can be more or less depending on your specific shell, but for our purposes an `alias` can be thought of as just another name for a command. Just like many Williams go by Bill; when I say "Bill", everyone knows I'm actually talking about William. `alias` is a way the command line does the same thing; when I say "nickname" for a command, it knows I actually mean "fullname" of the command. The syntax is simple:

```bash
alias nickname=fullname
```

Let's set up a couple aliases for some common misspellings of `git`:

```bash
alias it="git"
alias gti="git"
```

Now if I accidentally type `it` or `gti`, my command line knows I actually meant `git`. Great! 

But that won't solve my first example of `gi tpull`. For that, we'll need something a little more advanced:

### 2. shell functions

For now, just think of a shell function like any other command. (In reality, they are slightly more different, but we don't need to get into that today.)

Every command that a shell runs is given a list of parameters based on the space separated values typed into the command line. In most shell scripting languages, those parameters are mapped to variables: `$0`, `$1`, `$2`, etc. where `$0` refers to the actual string used to execute the command, with the rest being other parameters. Also all the variables `$1`, `$2`, etc. are all mapped into the single variable `$@`. Let's do an example, at the command prompt you enter:

```bash
git commit -m 'The best commit message evar'
```

If `git` were a shell script, the parameter variables would look like this:
```bash
$0 = git
$1 = commit
$2 = -m
$3 = 'The best commit message evar'
$@ = commit -m 'The best commit message evar'
```

So now the wheels might be spinning. We need to take something that looks like this:
```bash
$0 = gi
$1 = tcommit
$2 = -m
$3 = 'The best commit message evar'
```

And turn it into the previous example.

We'll use two commands: [`sed`](https://linux.die.net/man/1/sed) and [`shift`](https://www.systutorials.com/docs/linux/man/1p-shift/)

`sed` is a stream editor, meaning you pump text into it with instructions on how to edit it and it pumps the edited text back out. We'll use `echo` to pump the first parameter into into `sed` and give `sed` a regular expression to edit with (don't worry, this one will be simple, I promise)

```bash
echo "$1" | sed 's/^t//'
```

`sed` takes a parameter of the form `s/regular-expression-to-search-with/text-to-replace-matches-with/`. Above we give it the regular expression `^t`: `^` means "the beginning of the string" and `t` just means "t"; so effectively we've instructed `sed` to find a `t` only at the beginning of the line and replace it with nothing (aka remove it).

So what about `shift`? That simply moves all those indexed parameters forward one, `$2` becomes `$1`, `$3` becomes `$2`, etc. and `$1` is just removed which gets rid of that pesky `tcommit` from `$@` in our example.

Now to put it together, we will do things in three steps:
1. Correct the first parameter to remove the leading `t` and stash the result for use later.
2. Remove the incorrect first parameter from the list of parameters.
3. Call `git` with the corrected first parameter from 1 and the remaining parameters (`$@`)

And don't forget our function needs to be called `gi` for the command line to know to call our interpolator to come save the day from typos, which looks like this:

```bash
gi() {
  first_param_without_leading_t=$(echo "$1" | sed 's/^t//')
  shift
  git "$first_param_without_leading_t" "$@"
}
```

And this will do it! If we accidentally type `gi tcommit -m 'The best commit message evar'` our function will be called and instead call `git commit -m 'The best commit message evar'`.

(I'll leave it to the reader to build the very similar `gt` for even worse typos like `gt irebase -i`; or you can [cheat and look at the code](https://github.com/kallmanation/gi#gi))

And that's great! We now auto-fixup all sorts of typos! But, there's one more class of mistakes you could make. What if you put the space too late instead of too early? Like `gitp ull` or `gitp ush`? Well:

### 3. `eval`

It's easy to see how to fix the example I gave using what we now know about shell functions. Just make a `gitp` function:

```bash
gitp() {
  git "p$@"
}
```

That's even easier than all the nonsense we had to do to get `gi` working! _But_, it doesn't help at all if I type `gitr ebase` and what about `gitc ommit`? Should we just whip up 26 functions like `gitp` for each letter of the alphabet?

No!

We're programmers! We can program the program!

Our command line has this nice little command, [`eval`](https://unix.stackexchange.com/questions/23111/what-is-the-eval-command-in-bash). It takes parameters, pretends it's a command and executes it. Is it dangerous? Yes. Is it basically evil spelled differently? Yes. Should we use it? Watch me.

Let's start with a loop over the letters `a` to `z`, with that we'll `eval` an expression to create a function for each letter exactly like the function we wrote above: `gitx() { git "x$@"; }`. Throwing in the proper escaping to make sure it gets evaluated correctly:

```bash
for x in {a..z}; do
  eval "git$x() { git \"$x\$@\"; }"
end
```

And that's it! Three easy steps to making a neat little git interpolator.

%[https://github.com/kallmanation/gi]

Let me know what you think!
