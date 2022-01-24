## Quick Command Line Calculator in One Line of Code

I will admit I'm currently a ~~bit of a~~ complete terminal rat. I use [Neovim](https://neovim.io) daily inside of [tmux](https://github.com/tmux/tmux/wiki). My tests are run from the terminal. I use [git](https://git-scm.com) from the terminal. Deploys and releases are done in my terminal. I'll even open web pages with Mac's `open` command.

So I asked myself: why would I open a separate app to make quick calculations when I have a perfectly good terminal blinking right in front of me? I made a little command to "solve" my "problem" and now I've decided to inflict your eyes with my code. (or checkout the [gist](https://gist.github.com/kallmanation/fa535dc9da0cdadaaaf7c9d2630e5e99))

```
c() { echo "$@" | bc -l; }
```

The [`bc`](https://www.gnu.org/software/bc/manual/html_mono/bc.html) command on its own enters an interactive calculator mode. Which is nice but not what I wanted for quick arithmetic. So I use `echo` to pipe my calculation (`"$@"` refers to all the arguments given to this function) into `bc` and have the results calculated and printed with an immediate return to my normal terminal. For fun I also threw in the `-l` flag which gives me access to things like sine and cosine (but I cannot say I've really used them). Named it the shortest mnemonic I could think of, `c` for calculator, and I was done.