---
title: 'Git Add and Commit in Single Command'
excerpt: "Who doesn't love one liner?"
category: tips
layout: post
comments: true
tags: [tips, git, hack]
---

If you are asking if there is a way to make these commands:

<pre>
$ git add .
$ git commit -m 'commit message'
</pre>

in a single command, then there is. Git developers are so smart they always think about this. Basically, you predefine the one line command that will perform both command `add` and `commit` using Git global alias.

<pre>
$ git config --global alias.addcommit '!git add . && git commit'
</pre>

Then use it like this,

<pre>
$ git addcommit -m 'commit message'
</pre>

Neat huh?

<br>
Happy coding and cheers!
