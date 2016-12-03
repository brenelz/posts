---
extends: _layouts.post
section: postContent
title: "Blackboxing Scripts in Chrome Developer Tools"
date: "November 27, 2016"
---

If you are anything like me, you spend a good deal of your time in chrome developer tools trying to debug your Javascript and CSS.

One little known tip that I picked up from a recent <a href="https://frontendmasters.com/courses/chrome-dev-tools/">Frontend Masters workshop</a> is that you can blackbox certain scripts.

## What does Blackboxing mean?

Blackboxing basically means that you are telling the debugger to ignore a certain file when you are stepping through your code. I know when I step through my code I always seem to end up in some kind of minified jQuery file.

You can blackbox a script in a few ways, but what I usually do is right click on the file in the `Sources` tab and hit `Blackbox Script`.

<p><img src="/images/posts/chrome-developer-blackbox.png"></p>

Now if you `step over` or `step into` your code it will know to ignore your blackboxed script since your probably trying to debug your own code and don't care about anything within your libraries.

If you would like to learn a bit more about this feature, head on over to this <a href="https://developers.google.com/web/tools/chrome-devtools/javascript/step-code#blackbox_third-party_code">google article</a>.
