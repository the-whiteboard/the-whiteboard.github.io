---
layout: post
title:  Reasonable code
date:   2016-03-27 11:57:26 -0600
categories: coding debugging
author:	JimmyHoffa
---

There are many things we talk about when assessing code and trying to construct
an analysis of it's quality metrics. We talk about modularity in systems at the
large and small levels; we talk about coupling; we talk about performance
metrics; we talk about "Best Practices"; we talk about style, statefulness,
paradigm, elegance, smells, so many different things.

We rarely talk about *Reasoning* however. It's rarely something mentioned in
interviews, code reviews, architectures, patterns, classes, or anywhere outside
of perhaps formalist maths training (as I understand it).

I would like to speak to this concept, because I think it's something we oft
touch on in everything we as programmers do. Though we frequently interact with
the concept, it's rarely given a label or defined in any sincere sense. Every
time we look at code and automatically find ourselves struggling to make out the
actual semantics, behaviour, and precise functionality of that piece of code,
we're touching on the concept of `Reasonability`. Every time you pick up a piece
of code you've never seen before, and can find all of it's actual behaviours
writ explicitly within that single piece, we are touching on the concept of
`Reasonability`.

Reasoning is something we do every day when we have to look at some code and
decide what it will do, and what it should do. Every time we are writing a piece
of code and trying to make it's behaviour as clear as possible within it's own
scope, we are focusing on making that code easy to reason about.

Reasonable code is what you get when you needn't look across multiple files, you
needn't look across a large section, or ponder with difficulty over many logical
decisions within it.

Let's step back for a moment though.

What is `Reasoning` to begin with? It's so rarely spoken about that I think it's
worth giving a clear definition of, such that we may begin to utilize it as a
skill explicitly- as opposed to by accident or undefined habit.

I would give this definition for `Reasoning` where it relates to logical
analysis:

 > Reasoning: The act of mentally deducing by parts, the complete behaviour of a
 > logical system, such that given any input you can accurately assess all
 > outputs.

By this definition, I would lay claim that any section of code - from a single
block to an entire system is a `Logical System`. As such, we constantly find
ourselves reasoning through these systems - bit by bit - all day every day as
programmers. I believe the reason this is so infrequently mentioned or spoken of
is because the act is not only an obvious necessity to complete any given task
within the scope of being a programmer, but more importantly it generates no
visible outputs. With the entire act being invisible both to the participant and
any observer, it has resulted in making one of the key activities we as
programmers all practice, one which we have nearly no formal or conceptual
analysis of.

However, with the above simple definition of `Reasoning`, I think we can begin
to construct an entirely new approach to assessing code quality. One that takes
into account something we do far more frequently than nearly any other
activity where it regards interacting with code. The best part? While it's
somewhat subjective, while it's something we cannot assess computationally
because it depends on our individual anecdotal experiences interacting with
code, **it's the easiest metric to test code against of any**. You need no
tools. You need nothing but your own mind and a text editor.

To begin to assess the reasonability of code, all you need to do is take a piece
of code, look through it and measure how long it takes you to reason about every
facet of it. Practice doing this with your code, with other peoples code,
practice doing it in the small and in the large. If you are attentive during
this process to the results- you'll begin to notice patterns. These patterns
will stand out in regards to when code is very quickly reasoned through, and
when code takes a long time to reason through, and when you find code that you
simply cannot *completely* reason through.

These patterns are great to learn and recognize! These are *not* design
patterns! These are characteristics of code you will notice, when present effect
how easily you can reason through it.

Here now we get to the nitty gritty. I would like to list the characteristics I
have personally found makes code more `Reasonable`, such that I can complete the
act of `Reasoning` as defined above on the code with greater ease, speed, and
accuracy than code where these characteristics are not present.

 * Small scope
  - When a code section touches very few things outside of it's own scope, up to
    and including nothing outside of it's own scope, this makes reasoning
    through the sections entire behaviour far quicker and my resultant
    understanding of the code vastly more accurate.
 * Dictating instead of deciding
  - When a code section has very few logical decisions, rather is actively dictating
    what it will do without querying data to decide one way or another, the code
    is often far more reasonable.
 * Explicit data use
  - When a code section is very explicit and specific in what data it is taking
    as input, what it is doing to said data, and what data it is outputting, my
    ability to reason through the completeness of it's behaviour is often far
    more accurate than when data is implicitly touched. For instance, if the
    section of code makes a call to another section which implicitly changes
    data, it takes more time for me to reason through the full systemic effect
    of the original section of code.
 * Short stacks
  - When a code section calls into methods which call into methods which call
    into methods until you have a very high chain of behaviours being executed,
    the code begins very difficult to reason about. This goes back to the point
    about `Small scope` - each section of code called by a single section is yet
    another piece of the scope you must understand to fully reason about the
    original section of code.
