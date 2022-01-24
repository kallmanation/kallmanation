## Get your code sample rejected in 3 easy steps

<span>Cover photo by <a href="https://unsplash.com/@wackomac007?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Mark Rohan</a> on <a href="https://unsplash.com/s/photos/no-sign?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>

---

_Disclaimer: this post should be considered humorous and not a comprehensive guide on submitting a good code sample. While it is based on my experience interviewing at [Root](https://root.engineering/), it does not reflect the company's opinions or stances on how it conducts interviews; these opinions are solely my own._

---

# 1. Include bugs

Do not bother validating that your code sample gives the correct output.

In fact, the more bugs the better! If you show your reviewer that you cannot write bug-free code in a small task you efficiently convince them that you will write bug-riddled code when they hire you.

Letting your reviewer see your lack of attention now lets you get rejected at the code sample instead of at the last interview round (or worse, being hired and then fired later for poor performance).

And the best way to make sure you have plenty of bugs?

# 2. Do not write tests

Tests are a waste of time. You have a lot of code samples to shotgun out to employers; absolutely do not do any quality control. And more tests will make it harder to include bugs, which was step number one!

If you _must_ write a test; do not make it useful. Make it pass even when multiple lines of the function are deleted. And whatever you do: **do not** write an end-to-end or integration test; those things take the most time (even more waste!) and find even the subtlest bugs in your code. Just don't do it.

# 3. Do not include instructions to build & run the code

Remember, you want this sample rejected. If the reviewer cannot run your sample (or can only run after hours of trial and error) they will need to review the code even more carefully.

If you have successfully executed steps one and two, that careful combing of your code will give you the maximum chance of rejection.

Definitely use undocumented dependencies. Even better, depend on something inherent to only your computer (the absolute path to a particular file for instance). Do not use a package manager if it can be avoided (or if you do use it in a non-standard way or use private packages).

---

_P.S. Root has cranked up its hiring machine for 2021. If you would like to practice these 3 steps, go ahead and [apply](https://root.engineering)_