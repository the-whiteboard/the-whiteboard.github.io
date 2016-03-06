---
layout: post
title:  Five languages to know
date:   2016-02-19 17:49:26 -0600
categories: language
author:	MichaelT
---

## Five Languages

Many years ago, I took a class titled *theory of programming languages*.
The class quickly touched on four different programming languages that
were archetypical examples of their various paradigms.  The four that
we worked in were C (for procedural), <s>Oak</s> Java (just recently
released for object oriented), ML (for functional) and Prolog for
declarative and to really mess with undergraduate minds (never did
get that program working).

People keep asking "what are the languages I should know" and everyone
has their own lists. So... here's mine.

## Because it is

### C

There isn't too much to be said for C. It is. It is a consistent and
fundamentally *there* programming language. Everywhere you go, you
will find C.  Be it the language documentation for Ruby, or the
implementation specs in an RFC - you will find C.

C is not the best language in the world. But is is the *lingua franca*
of computing.  Many people will assume you know at least some basic C,
and that basic level of knowledge will be expected by many errors
that programs generate or install processes for any developer tools.
And chances are, the language that you are going to work in that *isn't*
C, it was probably written in C.

I said *C* not *C++*.  C is a nice little language. C++ is not so nice
and not so little.

The other point is that unless you start getting into fancy memory
management libraries or decided to dabble in Objective C instead
because you've got a Mac or want to go native iOS - C has manual
memory management. While it is a pain to work with by yourself, it
is important to know how much of a pain it is - and thus be able to
appreciate how much garbage collection does for you.

There is an entire class of problems and debugging that you will
need to understand and refine to work efficiently with C.  That
understanding will serve you will in all other languages you will
find yourself working in.

## An assembly or machine language

### Java Bytecode

Once again drawing from my college days, a prerequisite for many other
classes was *assembly language programming*.  Some of them made very real
use of this information (the compiler class had you write to that assembly).
Others just kind of hinted at it with a "you should remember this from
your previous classes," and would go on letting people who had forgotten
all the material with the final exam flounder for a bit and look up their
TA's office hours.

For this, there are a number of options. You could go with a primitive
classic that is still found in embedded works today.  For example, the
[6502](http://www.westerndesigncenter.com/wdc/) still can be found. It's
still very alive and kicking today.  You could go with something that
is a bit more academic like MIPS with
[SPIM](http://spimsimulator.sourceforge.net) (the reason I say academic
there is that SPIM is often used in college classes).  You could go
esoteric academic and go for MIX or MMIX or MIXAL from *The Art of
Computer Programming*.

However, my crystal ball says that the direction that the computing world
is going is that which Pascal set down decades ago.  Bytecode.  Pascal
had its [p-code machine](https://en.wikipedia.org/wiki/P-code_machine)
which pascal was compiled to, and then the p-code machine was an
interpreter.

LLVM is one way to go. CIL is another. I am going to say I believe there
is significant value in learning the Java bytecode.  I'm also going to
say right up front, this is probably the absolute wrong answer for
someone who is getting into programming. The JVM is not an easy place
to start (go back to my mention of the 6502 or MIPS).

## A Lisp

### Clojure

There are many functional languages out there. Some I still can't
get my head around. Trying to explain what a
[catamorphism](https://wiki.haskell.org/Catamorphisms) is and how
it uses F algebra for the underlying functor... and you've lost me.

The thing that a Lisp gives you is something completely different
than what most programmers have dealt with so far. Even those who
have flirted with functional languages of Scala or Ruby or might have
done a lambda in Python or a closure in Javascript... Going to a
Lisp language takes a commitment of thought and rethinking how
to think about processing.  Thats because it is so easy to say "yea,
I could use a ${functional concept}, but I don't need to."  Lisps
don't let you do otherwise - and you can't even *write* the code to
look like one's "native" language.

## Something for the resume

### Java

You'll note a trend here. I do not work at Oracle. What I do believe
is in the synergy of technologies while at the same time exposing one
to different concepts - but concepts that can work together.

Recently, I've found myself at an employer where my options for
installing software are severely limited. But yet, I *can* launch
other languages that use the JVM without too much difficulty (it is
an Eclipse plug in).

This language choice could just have easily been C#.  And then you'd
have F# and the CIL  as previous recommendations. But I've not touched
a Microsoft stack in two decades and so I can't properly say "you should
learn this."

The reason to learn a corporate language is to get a job. Really.
Programmers tend to be well paid and ones that work in larger organizations
with the job security that allows one to play with ideas and toys when
you get home.  Those jobs tend to not be php and ruby. A hip language
can get you a job at a hip place... and the boring language can get you
a job at a boring place.  But that boring job will be there next year,
and the year after and probably a bit after that too.  Having worked
at a place where the pay check bounced, this is something that I feel
is important.

So learn Java because it will get you a job that has a bit more
job security to it.

## A scripting language

### Groovy

There are two reasons to point at to learn a "scripting" language.

#### For the quick script

You need to quickly do a task. You can do it in whatever shell you
have available... but they all suck.  Or you can do it in whatever
scripting language you have available.

Run through all the files in a directory and do a checksum, and then
list out the ones that don't match that other directory? Yea, you
could do it in shell and it isn't too bad. But anything beyond a
dozen lines or so, and you'll wish you were working in a more
disciplined scripting language.

#### You'll find it other places

I've found Groovy in several places.  The two most recent ones I've
interacted with it are in the aptly named 'Groovy console' in
a CI server and also in writing test cases in Groovy in a testing
application.

Now, I'm sure there are many other places that you'll find other
scripting languages.  The point is to be able to have a language
that lets you think in the scripting rather than the Big E Enterprise
mindset where the first thing you do is create a .pom file and then
get a dependency injection framework and then...

Write something quickly. Scripting focused languages are wonderful
for that.

There are two runner ups I'd like to mention.  JavaScript and Lua.
Both can easily find themselves in this role too.  Just I am
keeping with the JVM languages.

There is a synergy that this set of languages has - being able to
slide from Java to Groovy or Clojure makes sharing code easier.
You can call Java from Clojure or Groovy.  And with only a little
bit of work, you can call Groovy or Clojure from Java.  Having
a common runtime environment for all of these cuts down drastically
on the learning curve - you know Java's String? It is the same class
in other languages too.
