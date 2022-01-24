## One Liner to Create and Move into a Directory (combining mkdir + cd)

Almost every new project seems to start with the same thing:

```sh
$ mkdir new-project
$ cd new-project
```

In a GUI we would be stuck; but in the command line, we can make this one command instead of two!

Let's make a little function called `mcd` (for make and change directory). First we need to make the directory:

```sh
mcd() { mkdir "$@" }
```

The `"$@"` refers to all the arguments given to `mcd` when it is used. At this point, we've basically made a round about `alias` for `mkdir`. So how about adding the `cd` part now?

```sh
mcd() { mkdir "$@" && cd "$@" }
```

And this _kind of_ works. The problem is `mkdir` can receive a whole bunch of flags and even multiple directories to create at once, but `cd` really only expects one argument, the directory to move into. The good thing about how these commands are structured is `mkdir` expects the last argument to be a directory, which makes it easy for us to pick out the one directory that was created (or if many were created, pick out the last one created) using `"$_"`, which refers to the last argument of the previously executed command.

```sh
mcd() { mkdir "$@" && cd "$_" }
```

And we could be done here, but I added one more thing. If `mkdir` prints errors, they will all be written in terms of `mkdir`. But that makes little sense to someone who just ran a command called `mcd`. I fixed that with `sed` and some redirection:

{% gist https://gist.github.com/kallmanation/2027bb23242e59cb90141c803ffe2703 file=mcd.sh %}

For more examples of how this is used, check out [my gist](https://gist.github.com/kallmanation/2027bb23242e59cb90141c803ffe2703).
