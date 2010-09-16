---
layout: default
title: Lambdebug, clojure step by step, back and forth
menu: <a href='/blog/'>=&gt; Blog </a>
---
# Laziness

I distinguish between lazy and eager expressions.  I fully evaluate lazy ones, but in one gulp,
while I step into the subexpressions of eager ones.  So stepping into `(map #(* 2 %) [1 2 3])` will give you the result
`[2 4 6]` without showing you each multiplication.  But `(doseq [i [1 2 3]] (print i))` will step into `(print i)` for each value.
