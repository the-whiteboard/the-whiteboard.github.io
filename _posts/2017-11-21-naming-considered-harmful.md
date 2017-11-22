---
layout: post
title: Naming considered harmful
date:   2017-11-21 18:30:00
author: MichaelT
---

> There are two hard problems in computer science: naming, cache invalidation.  
> -- Phil Karlton

On the whole, the profession of software development creates things at an
astonishing rate.  A day writing a [CRUD][crud] application, and you've
got a few dozen class files - each of which has a name.  Each class file
has a few (I hope - just a few, no more) fields - each of which has a
name.  The objects are exposed through some endpoint - with a name.

Every library that we use has a name.  There's the joke of the
[web dev drinking][drinking] game:

1. Think of a noun
2. Google "<noun>.js"
3. If a library with that name exists - drink

There are too many names out there.  The instinct to name everything that
we touch from design patterns to projects.  Everything seems to need to
have a name.

## STOP IT.

Avoid naming things that don't need to have names.  Unless you are going
to refer to something more than a handful of times and need to save that
extra little bit of mental capacity and time to not describe what it is,
it doesn't need a name.  If you are only going to see something once
(hopefully all that awful code that someone else wrote falls into this
category), it doesn't need a name beyond "bad code."

Naming is a shorthand for communication.  Good names and things that need
names make it easier to communicate.  Bad names require swapping in rarely
used name lookup tables into our mind to bring all of that additional
"what was that again" - that takes time.  Just think of all the mental
cache misses we have for trying to remember the name of a co-worker's
child - we need to minimize that mental overhead in our programming.

Some years ago, when my coworkers and I were trying to make a few million
lines of third party code work, we stumbled across some code in the
"how did the register get the credit card number?" section:

{% highlight java %}
int source = register.getSource();
boolean keyed;
boolean swiped;
boolean nfc;
switch (source) {
    case 0:
        keyed = false;
        swiped = false;
        nfc = false;
        break;
    case 1:
        keyed = true;
        swiped = false;
        nfc = false;
        break;
    case 2:
        keyed = false;
        swiped = true;
        nfc = false;
        break;
    case 3:
        keyped = true;
        swiped = true;
        nfc = false;
        break;
    // you can see where this is going
}
{% endhighlight %}

This was bad code.  We gave it a name - the "bit switch" and joked about
it.  Now, here's the thing - I still remember that bad code and its name.
Its not code I will ever write, nor is it code that I'll ever let go past
my editor prompt unchanged.  But now, I've got a name for it... and all
that mental encumbrance that comes with having something with a name.
It is not a good thing.  This code already had a name before we christened
it the "bit switch" - it is Bad Code.  That's all that needed to be said.
Things that you only see once or should only be seen once aren't
deserving of a name - they cost too much.

Next time you think you have a novel design pattern - you probably don't.
Next time you see someone write something and say "ahh! That's an
anti-pattern, it deserves a name," - it doesn't. 

Use names for things that you want to remember, for things that you will
use to communicate with your coworkers on a daily basis.  Use the same
name as things in blogs when they mean the same thing - don't make a up
a new one.

Many of the subjects that we deal with as programmers are big complex
ones.  There is a lot of complexity hidden behind the name Factory and
how that differs from the name Builder.  We use these names to quickly
capture all of that complexity in one word.  And thus the lure of the
Name.  When designing software, we often encounter significant chunks
of complexity in the code and want to know the short hand for it - the
incantation to type into a search engine, quickly communicate it to
another programmer, and seem smart when talking about it.

The danger of this is that the wrong one, or the misunderstood Pattern
greatly increase the cost of explaining something to another person.
If someone is talking about the Foo pattern they've "discovered", and
the other person has no idea what the Foo pattern is, that increases the
confusion.  There are countless times online where someone is asking a
question about a problem they are having with Dependency Injection and
someone more experienced has to point out that what they're doing is not
DI and they have no idea what the asker is talking about because they've
wrapped up a significant bit of complexity in the misnamed DI and are
assuming that everyone else understands it.

When encountering a chunk of complexity, don't seek to name it first.
Doing so, is like catching animals, putting them in a cage in the zoo
and making up a name for it on the placard - expecting everyone else to
understand it.  Make sure that a name is understood before applying it
to a particular problem - otherwise you end up with a tapir in the pig
exhibit.  It is far better in both cases to explain the complexity rather
than the short hand for a name that could be misunderstood.  Furthermore,
when searching the web for information, searching for the problem or goal
frequently leads to better information than searching for the
term - remember that Patterns are about solving problems to reach some
goal rather than simply components of a larger system.

[crud]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete
[drinking]: https://twitter.com/ironshay/status/370525864523743232
