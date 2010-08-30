---
layout: post
title: When to avoid Subversion
published: false
---
A typical software project uses many libraries, and it's an art to pick
the right ones.  They fall into these categories
 * Utilites that could/should be part of the core language.  In the case
  of Java, there are a few alternative implementations for every `java.`
  package to circumvent its design flaws.  Apache Commons belong to this
  category.
 * Libraries that provide a specific feature which is used by your
  project, but not absolutely necessary, like a database importer or a
  report generator.
 * There are dependencies required by some library or by one of its
  dependencies.  It's difficult to hunt down if they are really needed,
  but you'll keep them just to be on the safe side.
 * Finally, there are one or two libraries that are intimately linked to
  your project.  You couldn't move without them, you can consider
  yourself lucky they exist.  An "intimiate" library can be anything,
  depending on your project.  It may be a canvas implementation if
  you're working on a vector drawing program, or a geospatial database
  plugin if you're doing something GPS-related.
