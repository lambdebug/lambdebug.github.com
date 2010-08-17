---
layout: default
title: My dreams of exception handling
---
A short foreword about love and hate,
{% hightlight clojure %}
=> (first []) ; I love it
nil
=> (nth [] 0) ; I hate it
java.lang.IndexOutOfBoundsException
{% endhighlight %}
Clojure corrected some stupidity of Java, but it also inherited some of
them.

# Dream 1, reproducible stacktrace

I don't like stacktraces as they are.  They say very little about the
context where they were thrown.  I cannot call `(catch
FileNotFoundException e (.getFile e))`, I can only call its `getMessage`
which may contain some information burried in a string.  Besides,
stacktraces say almost nothing about the nested contexts along the call
chain leading to the exception.

Here is what I dream of
{% hightlight clojure %}
=> (reproduce-contexts (call-something-nasty 666))
('(call-something-nasty 666)
 '(one-level-deeper-function 666 nil)
 '(this-is-the-nasty-one [666 nil]))
{% endhighlight %}
I would get a list of each function called with their actual arguments.
I could pick any item in it, evaluate it, and get the same exception.
It would make it easier to localize what went wrong and to test a fix to
it.

It seems doable with a macro that surrounds the body of every function
with a try-catch block.  It's just a rough idea, but hey, I am dreaming.

# Dream 2, throwing exceptions disabled by default

I think, an exception is just another way to write an assertion.
{% hightlight clojure %}
(defn average-with-exception
  [xs]
  (if (seq xs)
    (/ (reduce + xs) (count xs))
    (throw (EmptyCollectionException.))))

(defn average-with-assertion
  [xs]
  (do
    (assert (seq xs))
    (/ (reduce + xs) (count xs))))
{% endhighlight %}
Their intention is the same, the only difference is that an assertion
error cannot have a type and variables.  But this could be overcome
easily having `(defmacro smart-ass-ert [exp type & vars])`.

What I like about assertions is that they are not forced on you, but you
can enable or disable them.  You can even enable them for one package
and disable for another one.  And they are disabled by default.  My
dream is to be able to write `(nth [] 999)` and get a modest `nil` until
I really want to deal with my indexes.
