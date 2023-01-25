---
title: "Squish the Internet Explorer Z-Index Bug"
date: "2016-12-03"
slug: "squash-your-zindex-bugs"
---

## The Problem

One day a long time ago I was working on a website; I had everything working
fine until I decided to check how things looked in IE. Usually IE plays pretty
well, but in this instance there was a crucial problem. I had an absolutely
positioned div (hover menu) and under it I had a transparent PNG that was also
absolutely positioned. It looked like following:

<p><img src="/images/posts/z-index-bug.png" alt="IE Z-index"></p>

```
<div>
    <div style="position:absolute; z-index:1000;">
        <a href="#">Page</a>
        ...
    </div>
</div>
<img style="position:absolute" src="myimage.png" />
```

## The Solution

The absolutely positioned div had a z-index of 1000, but I soon found out that
IE doesn’t use z-index properly. I came across an article that explained the
flaw in detail:

> _In Internet Explorer positioned elements generate a new stacking context,
> starting with a z-index value of 0. Therefore z-index doesn’t work correctly_

The above article does not directly contain a workaround but in the comments a
fellow said the following:

> _giving the parent element a higher z-index actual fixes the bug_

I then used something like the following code on my site:

```
<div style="position: relative; z-index: 3000">
    <div style="position:absolute; z-index:1000;">
        <a href="#">Page</a>
        ...
    </div>
</div>
<img style="position:absolute" src="myimage.png" />
```

This gave me the result I was looking for.

<p><img src="/images/posts/z-index-bug-fixed.png" alt="CSS z-index"></p>

I would suggest you visit
<a href="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context">Understanding
CSS z-index: The stacking context</a> for more information.
