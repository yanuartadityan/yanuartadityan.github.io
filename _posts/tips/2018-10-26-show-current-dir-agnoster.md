---
title: 'Show only current directory in Agnoster'
excerpt: 'Making your ZSH a bit cleaner'
category: tips
layout: post
share: false
comments: false
tags: [tips, macos, terminal]
---

`Agnoster` is one of the simplest and cleaner theme for your *oh-my-zsh* mod for your ZSH. However, having the entire *pwd* shown in your prompt would make the terminal less efficient.

To begin with, add these lines in your *.zshrc*:

```
prompt_dir() {
    prompt_segment blue black "${PWD##*/}"
}
```

And do `$ source .zshrc` and you're good to go! VOILA!