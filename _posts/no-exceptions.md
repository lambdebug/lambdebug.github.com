---
layout: default
title: No exceptions
date: 2010/08/13
published: false
---
# Beaten by averages

Do you want your colleagues to check you every half an hour, asking if
you are sure about the piece of code you are just writing?  I bet you
would kill them the second day.  You want to concentrate on your work
and not get distracted.  And you still get distracted without noticing
it by exceptions.

Suppose, I am working on an important part of the code that calculates
of the average word length in each line of a file.  My first, naïve
attempt,
{% highlight java %}
public List<Float> averageOfLines(List<String> lines) {
  List<Float> averages = new ArrayList<Float>();
  for (String line: lines) {
    String[] words = line.split();
    averages.add(averageWordLength(words));
  }
  return averages;
}

public float averageWordLength(String[] words) {
  float total = 0;
  for (String word: words) {
    total += word.size();
  }
  return total / words.length;
}
{% endhighlight %}

I try it on a file, and bang!  An exception is thrown, complaining about
division by zero.  It's actually not easy to access that file manually,
because we are using JEFF, the Java Enterprise File-management Framework.
Anyway, I don't want to throw such an uninformative exception, so I
handle it correctly.
{% highlight java %}
public float averageWordLength(String[] words) {
  float total = 0;
  for (String word: words) {
    total += word.size();
  }
  try {
    return total / words.length;
  }
  catch (ArithmeticException e) {
    // Well, it can mean more things
    if (words.length == 0) {
      throw new WordlessException();
    }
    else {
      throw e;
    }
  }
}
{% endhighlight %}

I am feeling better now.  My code is more robust, my productivity is
high, I just doubled the lines of code in no time.  It just doesn't
compile.  Oops, I have to declare that `averageWordLength` can throw
`WordlessException`.  Good.  Oh, no, it doesn't compile.  Let's shut the
compiler up and declare that `averageOfLines` can also throw
`WordlessException`.

It compiles, it runs -- and bang again.  It's that `WordlessException`
now, and I still don't know which line caused the problem, and I still
don't have my averages.  Here is the final, professional version.
{% highlight java %}
public List<Float> averageOfLines(List<String> lines) {
  List<Float> averages = new ArrayList<Float>();
  for (String line: lines) {
    String[] words = line.split();
    try {
      averages.add(averageWordLength(words));
    }
    catch (WordlessException we) {
      logger.warn("Empty line encountered");
      averages.add(null);
    }
  }
  return averages;
}
{% endhighlight %}


Exceptions are similar to setters-getters. They conform to some
unjustified idea of safety, and they can come very handy.  But they do
nothing most of the time.  If you look into a catch block, you'll
probably find no meaningful code there, but only throwing another
exception, maybe logging something first.  Don't get me wrong, throwing
an exception does convey information.  This mechanism is just a stupid
way to do that.  What does `throw new FooException("oops " + bar)` say?
 - I had an assumption and it turned out to be wrong
 - This assumption can be categorized as `Foo`
 - This assumption is related to `bar` in my context
 - I am helpless, I don't know what to do if this assumption is wrong
 - I want immediate attention.  If you try to ignore me, I'll cry
