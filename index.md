---
layout: default
title: Lambdebug, clojure step by step, back and forth
---
# Introduction

Hi, my name is Lambdebug.  I can show you how a Clojure program is
executed step by step.  With a little trick I've learned from
Einstein, I can even travel back in time, and show you the steps
backwards.

It's easy to meet me.  Ask Mr Leiningen to get me from clojars
(including `:dev-dependencies [[lambdebug "0.3.3"]]` in your
`project.clj`)
Then just say these sentences to the REPL,

<div class='repl'><pre><code>
=> (use 'lambdebug)
=> (debug '(+ 1 2))
</code></pre></div>

And voilá, you're right in the middle of my world.  I even made the
first step for you, you can see it highlighted.

<div class='repl'><pre><code>
Function: Given at REPL
Form: ("<span class='hl'>+</span>" 1 2)
Result: #'clojure.core/+
>>>
</code></pre></div>

You can step into this complicated expression typing `i`, and learn the
value of each number, then finally that `(+ 1 2)` equals to `3`.
If you type `h`, you'll see all the directions I can take you.

<div class='repl'><pre><code>
>>> h
Help: choose
    step (i)n, (n)ext, (b)ack, (p)rev, (o)ut
</code></pre></div>

I can do more sophisticated things.  Try a bit contrived example,

<div class='repl'><pre><code>
=> (debug
      '((fn [x]
          (let [y (dec x)]
            (try
              (+ (/ 1 y) 5)
              (catch ArithmeticException e (inc x)))))
        10))
</code></pre></div>

There are more things to notice here.  When stepping into it,
you'll get the result of `(dec x)` within the let expression, then the
value of `(+ (/ 1 y) 5)`.  If, however, you try `1` instead of `10`, you won't get
to adding `5`, but you land in the catch block, evaluating `(inc x)`.

I can see that you are not quite happy yet.  You have written some nifty
functions.
{% highlight clojure %}
(ns nifty)

(defn count-both
  [x y]
  (+ (count x) (count y)))

(defn duplicate
  [x]
  (if (seq? x)
    (concat x x)
    [x x]))
{% endhighlight %}
No problem, I will follow you into any function defined in a file (with
the exception of the `clojure` namespace),

<div class='repl'><pre><code>
=> (use 'nifty)
=> (debug '(count-both [42] (duplicate [42])))
</code></pre></div>

In case you want to know more about me, I am an open source person,
you can come and see me at
<http://github.com/adamschmideg/lambdebug>

# I am not perfect

My limitations are,
 * I don't understand type hints.
 * I respect privacy and I don't mess with private functions.
 * Did I say I hate feeling dizzy?  No `loop`, no `recur`.
 * I am OK with most common macros, but not with trickier ones, like `binding`, `->`, `->>`.
 * I am quite picky about java interop.  I like simpler cases with dot
  notation, but no `proxy`, `doto`, etc, please.
