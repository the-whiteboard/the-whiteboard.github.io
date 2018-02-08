---
layout: post
title:  Debugging - The third intro to CS assignment
date:   2016-03-26 18:07:26 -0500
categories: coding debugging
author:	MichaelT
---

I'll admit to being a bit hazy about the exact homework problems and lectures in intro to CS all those decades ago.  I'll even admit to it being in Pascal (the other choices were C or Fortran 77).  I suspect the first homework problem was a "get familiar with writing in the IDE" and the second assignment was your typical "basic control structures."

If I could go back in time, I know what that third assignment would be. An intro to the debugger.

Learning how to use a debugger was something that came *much* latter for me (a few breakpoints in gdb while trying to write part of a compiler for a class - I was still much more a printf and examine the logs type - not that there's anything wrong with that either). And it wasn't until many years later when working on a very large piece of third party software that I became proficient in using the debugger.

In my early years of programming the programs were relatively small things. A CGI to do something. A script to read some logs. A webservice to read something out of a database. They fit on a page or two of a printout of need be, or not too large when looking at them in the editor. They were easy to understand.

Now, programs tend to be much larger. While small modular code is an ideal, [only Heaven and textbooks are ideal](http://www.scientificamerican.com/article/john-updike-poem-1969/). Every single bit of code you will find out there when doing actual work will be cringeworthy when reexamined later. Making sure you use modularization in the code will put this off for awhile, and make it easier to grasp the essence of smaller blocks of code.  Writing testable code will help you insure that things are working right when they are working right.

But when code becomes complex, and it will, you need to debug it. You really need to learn how to use a debugger.  It is the most useful, accessible, and powerful of all of the debugging tools available. I would go so far as to say that the neglect by professionals learning how to use the debugger borders on incompetence.  It would be just as bad for a chef to not know how to use a thermometer or craftsman not knowing how to use a ruler. These are basic and necessary tools of the trade; and while the debugger is a bit more complex than either of those examples it is just as fundamental.

I keep going back to an essay I read awhile ago.  [How to be a programmer - learn to debug](https://github.com/braydie/HowToBeAProgrammer/blob/master/en/1-Beginner/Personal-Skills/01-Learn-To-Debug.md).  This is the first item of the first set of skills to know. I am half glad that it wasn't written until 2002 so that I have a good excuse for not knowing of it for a decade (though I also wish I could have read it back then).

Learning to debug is in part learning how to use a debugger. That is just one tool of the many that learning to debug entails.  Debugging means in part being able to read the code and understand what it does - be it working correctly, or incorrectly. It means being able to look at some code and describing the business logic that it encodes. It means being able to look at some code and consider the possible edge conditions that might cause problems.

While the creative process of writing code is something that we got into programming for, it is the understanding and extending of existing code along with debugging that is the daily task of programmers everywhere.

My biggest lament when reading questions on programming sites is what appears to be either the willful disregard of the tools available, or the expectation that someone else can do it. At times, it can feel like talking to my niece and nephew. With my niece at times it was a challenge to get her to read it rather than hand off something that she should be able to read to an adult. While my nephew (a bit younger) comes up to you with the story you've read dozens of times before and asking you to read a story - again. This is ok for people in their first half decade upon this world (though a bit trying at times). On the other hand, for someone who is going into programming to essentially act the same as my niece and nephew with "debug this code for me" or "tell me what this does" is unacceptable. It makes me wonder how long it will take for me to get the next new hire to be able to debug code on their own without needing to go to someone and ask how to sound out another word.
