---
title: 'Hide Working Directory in Terminal'
excerpt: "No one likes long working directory shown in shell."
category: tips
layout: post
comments: true
tags: [tips, unix]
---

Do you wonder how to hide your current working directory in terminal? If you're in `~/Workspace/firstDir/secondDir/thirdDir`, then it would show terminal as something like this:

<pre>
januaditya@janu-rmbp:~/Workspace/firstDir/secondDir/thirdDir$
</pre>

If you want the shell to show just the last directory, which in this case is `thirdDir`, you can add this to `~/.bash_profile`:

<pre>
export PS1="\W \$"
</pre>

Then, source the bash using:

<pre>
$ source ~/.bash_profile
</pre>

Now, your terminal would look like this:

<pre>
thirdDir $
</pre>

Look what you can do [more](http://www.ibm.com/developerworks/linux/library/l-tip-prompt/) on command prompt (UNIX/Linux).


