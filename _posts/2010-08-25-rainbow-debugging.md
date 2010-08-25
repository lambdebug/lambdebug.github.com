---
layout: post
title: Rainbow debugging
---
Debugging is a recursive process to find out
* the value of a variable or expression
* what gets executed, or mostly, which branch of a conditional gets executed

When I don't get what I expected, I have to go deeper and deeper into
the misbehaving expression, explore expressions and their
subexpressions.  The simplest debugging technique prints out values
(case #1) and messages, like _I was here_ (case #2).  Stepping through
the code in a debugger does almost the same, without having to modify
the code when I change focus, and want to explore another value.  But
they are both linear approaches.

# Synopsis, or seeing all at once

I want to see everything in a context at once.  Most debuggers display
the value of variables in the current context.  To display values of
nested expressions, they would need a time-machine, either to remember
expressions already evaluated, or to foresee an expression to be
evaluated.  Let me give an example,

<div class='repl'><code>
<span style='color:red'>(if</span><br>
&#160;&#160;<span style='color:orange'>(and</span><br>
&#160;&#160;&#160;&#160;<span style='color:yellow'>(number?</span>
  <span style='color:green'>x</span><span style='color:yellow'>)</span><br>
&#160;&#160;&#160;&#160;<span style='color:white'>(even? x)</span><span style='color:orange'>)</span><br>
&#160;&#160;<span style='color:white'>"We are even"</span><br>
&#160;&#160;<span style='color:blue'>"It's odd"</span><span style='color:red'>)</span><br>
<br>
&#160;<span style='color:red'>"It's odd"</span><br>
&#160;<span style='color:orange'>false</span><br>
&#160;<span style='color:yellow'>false</span><br>
&#160;<span style='color:green'>nil</span><br>
&#160;<span style='color:blue'>"It's odd"</span><br>
</code></div>

and compare it to this

<div class='repl'><code>
<span style='color:red'>(if</span><br>
&#160;&#160;<span style='color:orange'>(and</span><br>
&#160;&#160;&#160;&#160;<span style='color:yellow'>(number?</span>
  <span style='color:green'>x</span><span style='color:yellow'>)</span><br>

&#160;&#160;&#160;&#160;<span style='color:blue'>(even?</span>
  <span style='color:indigo'>x</span><span style='color:blue'>)</span><span style='color:orange'>)</span><br>
&#160;&#160;<span style='color:violet'>"We are even"</span><br>
&#160;&#160;<span style='color:white'>"It's odd"</span><span style='color:red'>)</span><br>
<br>
&#160;<span style='color:red'>"We are even"</span><br>
&#160;<span style='color:orange'>true</span><br>
&#160;<span style='color:yellow'>true</span><br>
&#160;<span style='color:green'>42</span><br>
&#160;<span style='color:blue'>true</span><br>
&#160;<span style='color:indigo'>42</span><br>
&#160;<span style='color:violet'>"We are even"</span><br>
</code></div>

I can see what gets executed (colored) and what doesn't (white).  I look
at the first case, and without checking the return values, I already
know that `(and ...)` returns false because of shortcircuiting.  If I
want to know the value of a subexpression, I just look at the return
values with the same color.

This is where the development of lambdebug is just heading now.

A final note about the name.  When I first had this idea, I didn't know
about [Rainbox parenthesis](http://www.vim.org/scripts/script.php?script_id=1561),
the vim plugin to display lisp code in a colorful way.  When I bumped into it,
I liked its name, so I borrowed it.
