---
title: 'Change Tree View Font Size in Atom'
excerpt: "I love this text editor and it's keep growing"
category: tips
layout: post
comments: true
tags: [tips, atom, hack]
---

If you're wondering how to change Atom side bar (tree view) font size then you just need to add this stuff on `styles.less` file in `~/.atom/`.

{% highlight css linenos %}
@font-size: 10px;
.tree-view, .tab-bar .tab
{
  font-size: @font-size;
}
{% endhighlight %}

Check it out with font-size 10 (side bar) and editor font-size 9.

<p style="text-align: center;">
  <img src="{{site.url}}/images/post-mac-tips-atom.png">
</p>

Now each window can fit on my 15" rMBP. Perfect!

<br>
Cheers and happy coding.
