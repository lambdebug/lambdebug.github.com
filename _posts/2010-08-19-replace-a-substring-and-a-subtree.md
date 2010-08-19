---
layout: post
title: Replace a substring and a subtree at the same time
published: false
---
In the debugger part of Lambdebug, the currently executing functions is
displayed with the current sub-expression highlighted.  You would see
something like this
{% highlight clojure %}
>>> (fn [x]
      (if (seq? x)
        "<<< (count x) >>>"
        (inc x)))
{% endhighlight %}
All I want is to get rid of the quotes around the highlight.  Looks
easy?
