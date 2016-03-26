---
layout: post
title:  Reasonable code
date:   2016-03-27 11:57:26 -0600
categories: coding debugging
author:	JimmyHoffa
---

There are many things we talk about when looking at code and trying to construct
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
visible outputs.

Now that this is defined, bla bla bla, stuff about what makes one thing easier
to *reason* about than another bla bla bla.
