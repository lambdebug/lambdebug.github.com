---
layout: post
title: Replace a substring and a subtree at the same time
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
All I want is to get rid of the quotes around the highlight.  
Not a big deal.

I can even imagine a simpler version as a code golf challenge.
You get a string representing a nested structure of strings and numbers,
and an index.  The goal is to get the indexth item within that
structure, keeping the whitespaces as they are.
{% highlight clojure %}
=> (nth-str "5 9" 0)
"5"
=> (nth-str "[5 6] 9" 0) ; note the structure
"[5 6]"
=> (nth-str "[5   6] 9" 0) ; note the whitespaces
"[5   6]"
=> (nth-str "\"5 [6] 7\" 8" 0) ; structural elements within string
"\"5 [6] 7\""
{% endhighlight %}
Looks easy?

I even asked about
[Syntax-aware substring replacement] (http://stackoverflow.com/questions/3472346/syntax-aware-substring-replacement).
at StackOverflow.
The answers didn't give a complete solution, but I got some insights.
I'm going to tell you the story in a literate programming way which is
-- as I see it -- a chain of wishful thoughts.

# One level deep is enough, `nth` and `get-in`

The original problem is about getting any subform specified with a path
leading to it.  The function we are looking for could be dubbed
`get-in-str`.  The code golf version already softens this requirement,
asking for only top level elements.  But just as `get-in` can be
expressed applying `nth` recursively, the same holds for `get-in-str`
and `nth-str`.

This approach is actually working well, we could move on.  But it takes
some extra work.  Consider getting the item at `[0 0]` in string
`"[1 2] [3 4]"`.  With the above recursive approach, we first get
the first item, `"[1 2]"`, then we look for the first item within it.
Finding the closing bracket in the first step is not necessary, though.
It would be enough to find the start of the item at `[0 0]`, then look
for the location where it ends.

In either case, our first wishful thought is completed, all we need now
is an implementation of `nth-str`.

_to be continuted_
