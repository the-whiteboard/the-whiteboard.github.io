---
layout: post
title:  On best practices and how to use them
date:   2017-08-13 18:16:32 -0600
categories: bestpractice
author: MichaelT
---


While clicking about the net, I came across someone asking a question
along the lines of:

> I want to make some changes in some code.  Only modify what I'm changing?
> Update it all? Keep the same style for new code? What is the best practice?

The answer to this type of question is "well, it really depends on you
and your code. What are you trying to do? What is your workload? What
is your skill level?" and countless other questions to try to narrow
down the possible paths to the acceptable paths.

Once all of this is thought about, its the exceptional case that matches
the best practice - not the common case.

The idea behind the best practice comes from the realm of policy and
legislated standards.  Those do not always match the superior design.
Consulting firms often try to package up best practices (and claim that
they follow them) to either provide canned solutions or billable hours.

When one starts mentioning best practice, the realm of policy analysis
starts becoming relevant, and then the name Eugene Bardach starts popping
out in the literature.

There are certainly things that upon investigation have low cost and
little risk in implementing, though these tend to vary.  The "best
practice" of 'run the latest version of the framework' may have low cost
and little risk when the application is frequently used, well tested,
and fairly close to the latest version of the framework already.  On
the other hand, the less used application that is a major revision
behind the latest - an upgrade wouldn't be low cost or little risk.

The best practice solves some problem or advances some goal.  The
best practice of policy is very much like the design pattern of software
development - well known tools that can serve as a blueprint for a solution.
Neither best practices in policy nor design patterns are themselves a
solution.

As there are countless ways to have problems, there are similarly
countless ways to address the problem.  Grabbing a handful of best
practices off of tech blogs and incorporating them into one's application
is ignoring design steps - and especially the "does it actually solve a
real problem?"

People asking for a best practice are in effect trying to outsource the
decision making and problem solving to a third party (the person who
published the best practice).  In software development, this is lazy to
an extreme.  The very core of the job of a software developer is problem
solving.  This is even emphasized in many titles that include the word
"analyst".

People asking for best practices as a starting point are often trying to
skip the majority of design.  In doing this, it misses the alternative
options that might themselves be lower cost (in a Java EE application,
Spring MVC with Hibernate is a 'best practice' - which completely ignores
the myriad of other solution options that might run faster or be maintained
with lower overhead).

Often these approaches don't seem to do any harm in the design, but when
one comes back to the application a year or two down the road the clutter
of "why did the author do this?" and getting "it was a best practice
advocated on some blog" as a response makes it harder to fix and maintain.

In policy analysis, the best practice has [many definitions][hhs].  They are
things that have been occasionally found to be useful for solving certain
problems.  And that is also true of software development.  The best practices
are things that have been "this worked for me", and without much more.

Best practices differ from the documentation that is provided. Reading the
fine manual is often a necessary step to solving problems and should be
undertaken before one tries casting a net for solutions to problems that
one doesn't have.

Until a problem is had, asking "what are the best practices for building
some application" is premature design from which cargo cult programming
follows.

------

Ok, so you've gotten to this point and are not discouraged at finding a
best practice.  Lets now look at the Eightfold Path that Bardach proposes
in working with best practices.

## 1. Define the Problem

Until one can start looking for what path to follow, it is necessary to know
what the ultimate goal is.  The approaches declared to be best practices are
well worn trails through the forest.  However, unless you have a compass to
determine which way you want to go, following a "best practice" may lead you
completely in the opposite direction from what you are trying to achieve.

Recognize also what problems that are being encountered and the conditions that
cause them.  It is possible that there are organizational reasons that some
best practice is not at all practical no matter how "best" it is.  At a previous
employer it was recognized that using a particular framework that solves 90% of
the problem would be the best practice... but that framework was sponsored by
a subsidiary of our arch-competitor and any attempt to use it would be a
non-starter.

## 2. Assemble some evidence

Measure what is desired to be fixed.  If you are going to go down the path of
a best practice ("using MVC is a best practice"), measure the complexity of
the code that is currently being dealt with (there are many complexity measures
out there - cyclomatic complexity is probably the best well known, but there
are [many more][complexity].  There are some that nicely measure... well...
[crap][crap].

The reason to measure things is that it is actually important to see if you
have met the goal.  Change for change's sake isn't a good thing in software.

Once a measurement of what you want to improve (performance? complexity?
how fast code is written?), then it is time to do a literature review.  This
means more than casting a net for "what are the best practices" out into search
engines and forums.

Make sure that as part of this literature review, one reads about *negative*
reviews too and take them into account.  Want to switch from a relational
database to NoSQL? Make sure that the perils of this are read too.  That
said, if you already have the solution of "we *will* switch to a NoSQL
database", then trying to research the best practices for how to solve a
problem is rather moot.  Best practices are useful for deciding upon the
path to take.  They are much less useful when someone has already charted
a course.

## 3. Construct Alternatives

You want to improve performance? You have found a few different ways to do it?
Recognize those alternatives as ways.  Bounce the ideas off of the rest of the
team.  And don't be afraid to have something that isn't on the list (make up
your own best practices - after all, you know your system better than those
tech bloggers).

## 4. Select Criteria

Looking at your options, evaluate them.  Make sure that the criteria is
actually evaluative criteria - it is something that you can measure (and
thus the measurements previously done).  These are often tests of efficiency
or costs (time and money often find their way here).

Make sure that the options are also legal (following the licenses properly,
following the proper regulations and following the proper laws set down).
HIPAA and PCI often get raised.  These are things that are to be ignored
at your own peril.

## 5. Project Outcomes

This is project the verb - not the noun.  The project (noun) hasn't
started yet and it is necessary to project (verb) what the possible
outcomes will be.

Be realistic about what this can do.  Being too optimistic about using
"best practices" and may result in future projects being "nope, we
tried those 'best practices' before and ended up with more work than
we saved."

Identify what the use of these best practices may cause it to fail.
The following is not unheard of:

> We switched to using Hibernate for an ORM and while the SQL queries were
> removed from our codebase, the resulting annotations were difficult to
> manage.  The additional work from on boarding new hires added an additional
> week of time.  The resulting ORM, not being exactly familiar with the
> underlying data resulted in doing more queries than necessary and impacted
> performance.  ... but we're using the best practices now.

## 6. Confront the trade-offs

When looking at the options presented as best practices, it is often the
case that no one best practice is *the* Best Practice.  This builds upon
the projected outcomes.  What are you actually trying to accomplish and
are those side effects of selecting that best practice acceptable to the
business.

Make sure that the best practice is also compared to the option of doing
nothing and maintaining the status quo.  Following a best practice for
"we are going to improve logging using..." may spend 16h on an application
that is going to be turned off in a year and would only save 8h of support
time.

## 7. Decide

Make sure that the selected option is the best course of action.  Sell
yourself on the plausibility of this.  If you can't sell yourself on
that this is the best thing to do, trying to convince others (and
a skeptical manager) that this is the right thing to do is that much
more challenging.

Consider also the question of "if this is such a great idea, why wasn't it
done this way the first time?"

And now that you've decided and implemented it (or decided on the
status quo)...

## 8. Tell your story

Congratulations - you've implemented a best practice in your organization.
Write about it.  Make sure that other people are able to find your material
when they do their best practice resource search back in step 2.

-------

But if you're not going to actually do all of this to follow the best
practice... that you've already decided upon a solution and are just looking
for shortcuts and snags along the path?

Then don't ask for a best practice. It's not what you want.  You are
instead looking for an implementation guide or hiring a consultant who will
guide you down this path.

Without actually taking the time to measure, compare, and decide; following
a "best practice" that is out there is no better than throwing a dart on a
wall of buzzwords.  If you haven't measured what you are trying to accomplish
and been able to define what is best, anything works.  If you aren't going
to compare multiple options including the status quo, then getting options
that you aren't going to consider is a waste of time.  If you have already
decided on the destination, then stop woolgathering and start designing.

[hhs]: https://en.wikipedia.org/wiki/Best_practice#Use_in_health_and_human_services
[complexity]: https://en.wikipedia.org/wiki/Programming_complexity
[crap]: http://www.crap4j.org
