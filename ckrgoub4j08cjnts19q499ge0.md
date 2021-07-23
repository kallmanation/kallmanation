## `$` - A command for those who copy-paste from tutorials


So there I am, following along in the tutorial (as one does to learn).

I get to the step, they tell me: "Just run this command and you'll be done!" `$ cure covid19`

So I dutifully highlight the command. I copy each valuable character. I open my terminal. I paste. I hit enter!

And `-bash: $: command not found`.... ugh.

Let's fix it!

First, create a new file called `$` somewhere in your path (You can check your path by running `echo $PATH`. I will be using `~/bin/`, you might have to `mkdir ~/bin`)

```bash
$ touch ~/bin/$
$ chmod +x ~/bin/$
```

If you've done the above correctly, you'll be able to run the command `$` and nothing will happen. No errors!

But we want more, we want the command we copied to actually run. Easy enough, let's open the file called `$` that we just made and add some code in it:

```bash
#!/bin/sh

"$@"
```

`$@` simply refers to all the arguments the command is given (aka the command we wanted to run from our tutorial)

Now we should be able to do the following:
```bash
$ $ echo 'hello, hello, hello!'
hello, hello, hello!
```

We can be done here. Congratulations on making your shell script! But let's do more. It seems kind of dangerous to be copy-pasting random commands off the internet... Let's ask for confirmation before running the command, back in our file again:

```bash
#!/bin/sh

echo "You are about to run the command:"
echo "$@"
read -n 1 -p "Do you want to continue? [yN]: " answer
echo ""

if [ "$answer" == "y" ]; then
  echo "--- executing ---"
  "$@"
fi
```

Now back to our test run:
```bash
$ $ echo 'hello, hello, hello!'
You are about to run the command:
echo hello, hello, hello!
Do you want to continue? [yN]: y
--- executing ---
hello, hello, hello!
```

And now we have a command that asks for confirmation before running other commands! Which could be useful on its own. (and to those that copied too many dollar signs in that example, you got to go through two approvals, which might give you a few ideas...)

If you just want to copy/paste and/or share this code with your friends, [here's the gist.](https://gist.github.com/kallmanation/a93d85b14b49575c3f416c7dd3d7bd46)