---
layout: post
title: When picking a library, check how it's version-controlled
published: false
---
Even if you just want write a modest desktop version of Hello World,
you'll realize that you need tens of external libraries.  You may end up
using tons of them.  Picking the right library is an art.  You have to
carefully balance between many different aspects.  How mature is it?
How actively is it developped?  Does it have a community?  If hopefully
so, how responsive is it?  Are they nice and competent people?  Does it
have a fancy website?  Does it conform to the most recent 2003 W3C
recommendation?  There are many more questions to answer.  I want to
explore a single, less emphasized aspect.  What version control system
does it use?

Depending on your project, libraries fall into one of four categories.
 * Spades, or batteries that should be included
 * Diamonds, or the fancy stuff
 * Clubs where noone knows what's going on
 * Hearts that lie closest to you

A library may fall into one category for a certain project and into
another one for a different project.  Let's have a closer look at these
suits.

## Spades, or batteries that should be included

These are the libraries that should be part of your development
platform.  Well, they may be included in the next release.  Or a simpler
version is already included, but you need an extra feature supported
by this library only.  Or it just provides a usable API instead of the
flawed one in your platform -- like you can find an alternative
implementation for almost any package in the `java.*` namespace.  Apache
Commons usually belongs to this category.

## Diamonds, or the fancy stuff

Libraries that provide a specific feature which is used by your
project, but not absolutely necessary, like a database importer or a
report generator.

## Hearts that lie closest to you

Finally, there are one or two libraries that are intimately linked to
your project.  You couldn't move without them, you can consider
yourself lucky they exist.  An "intimiate" library can be anything,
depending on your project.  It may be a canvas implementation if
you're working on a vector drawing program, or a geospatial database
plugin if you're doing something GPS-related.

## Clubs where noone knows what's going on

There are dependencies required by some library or by one of its
dependencies.  It's difficult to hunt down if they are really needed,
but you'll keep them just to be on the safe side.

## OK, but how is it version-controlled?


