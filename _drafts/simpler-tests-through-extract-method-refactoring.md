---
layout: post
title: Simpler Tests through “Extract Method” Refactoring
categories: testing
author: Lukas Atkinson
---

I'm currently refactoring a huge method into smaller parts. It is stock full of nested loops, maintains a complex state machine with more variables than I have fingers, and is the kind of code where I have to ask myself how I could ever think this would have been a good idea. So obviously, I'm splitting that function into smaller, independent chunks with the *Extract Method* refactoring technique.[^1] Since the control flow is now simplified, the code has also become easier to test – as long as I'm comfortable with testing private methods. Why?

  [^1]: See <http://c2.com/cgi/wiki?ExtractMethod> and <http://refactoring.com/catalog/extractMethod.html>.

The number of test cases needed for full *path coverage* corresponds directly to the *McCabe complexity* of the code under test. Since many simple functions often have lower total complexity than one convoluted function, the overall required testing effort is reduced. As this reduction can be substantial, there is a strong incentive to test the extracted methods directly, instead of testing only through the public interface.

The [*McCabe Complexity* or *Cyclomatic Complexity*][wen-complexity] is a software quality metric that counts all paths through a piece of code (e.g. a function). Experience shows that a complexity of 10 should not be exceeded. In general, the complexity grows exponentially with the number of conditionals in a function. A function with no branches has a complexity of 1:

  [wen-complexity]: https://en.wikipedia.org/wiki/Cyclomatic_complexity

{% highlight python %}
def foo():
    something()
    return x
{% endhighlight %}

With a single branch, we get 2 paths:

{% highlight python %}
def bar():
    if x:
        something()
    return x
{% endhighlight %}

With two branches, we have 4 paths:

{% highlight python %}
def qux():
    if x:
        something()
    if y:
        return x
    return z
{% endhighlight %}

With three conditionals we get up to 8 paths, and so on. We stay under this number if the conditionals are nested or have an “early return”. So using big-O notation, we might state that “the complexity <i>c</i> is in <i>O</i>(2<sup><i>n</i></sup>) where <i>n</i> is the number of conditionals”.

So let's do a little thought experiment with a function that contains 7 independent conditionals. This function has a cyclomatic complexity of up to 2<sup>7</sup> = 128. Remember the sensible limit of 10 paths? This function is way past that.

What happens if we extract each conditional into its own function?[^2] Now we have 7 functions with one conditional each, giving each function a complexity score of 2. And of course we  need top level function to tie them all together (no branches, so complexity is 1). By splitting the function into smaller parts, the source code now has a total complexity as low as 7·2 + 1 = 15. That is a whopping complexity reduction by 88% down from 128!

  [^2]: Please note I'm not suggesting that this refactoring technique should be applied mechanistically to game the complexity metric. Taken to the extreme, that would lead to a haystack of function calls hiding a needle of relevant functionality. Instead, I'm trying to demonstrate  the potential of “Extract Method” in the context of unit testing.

With the *Extract Method* refactoring, the code becomes easier to understand for humans. The extracted functions need to receive any  data they operate on as parameters, thus making the data flow more explicit[^3]. In the top-level function, this allows us to easily trace which extracted functions affect each other.

  [^3]: Compare *Reasonable Code* by Jimmy Hoffa (<https://the-whiteboard.github.io/coding/debugging/2016/04/08/reasonable-code.html>). Extract Method helps achieving small scopes, explicit data use, dictating instead of deciding, and explicit outputs. However, it trades in short stacks for these advantages.

However, these benefits do not translate automatically to unit tests. Unit tests are typically expressed in terms of the public interface – our top level function after the refactoring. Since the refactoring did not change the behaviour of the code, existing tests will continue to pass. In particular, the control flow is essentially unchanged. We reduced the complexity of the *source code* on a function-by-function level, but the *control flow graph* still has an effective complexity score of 128.

The good news is that we now need at most 2× the current number of tests to move from full statement coverage on the original function to full path coverage on the refactored function and its extracted helpers. This effort is manageable, and might have a high payoff since path coverage-guided test case generation tends to have a high rate of defect discovery \[citation needed\].

Our test cases can make full use of the refactoring if we test each extracted method in isolation. The main advantage of testing extracted methods directly is that we typically need less setup code than when testing indirectly through the top-level function. We can use this opportunity to think about the extracted method's contract, its pre- and postconditions, its edge cases, …. This careful analysis and review would have been much more difficult in the large, original function.

Starting to test implementation details requires some strategy. A danger of testing the private, extracted functions is that these tests are *fragile*. Changes to the structure of the code will break the test cases, even when the behaviour stays the same. For each case, we have to find an individual balance between the advantages of testing private methods (simplicity, high defect detection rate) against its drawbacks (fragility).

One possibility of doing this is focusing your higher-level tests on positive test cases, playing through complete usage scenarios, and verifying requirements. These tests assure that your code works whether it was refactored or not, and are likely to break if there is a problem in the interaction between internal components. In contrast, tests for the private implementation details can focus on systematic white-box testing, edge cases, and error cases.

In case the code is refactored again[^4], these tests should not be thrown away.[^5] The private tests still encode guarantees and assumptions that were provided and relied on by the previous code. Instead, they deserve to be adapted to the new structure to make sure we did think about those guarantees and edge cases during the refactoring. This isn't fragility, this is a pre-flight check list.

  [^4]: … which, for high-quality code, is a rather big “if” …

  [^5]: In his article *Code Testing and its Role in Teaching* (<http://www.cs.princeton.edu/~bwk/testing.html>), Brian Kernighan urges us to “Never throw away a test”. This has two aspects: avoiding regressions, but also getting the most value from any (informal) testing: “presumably there was some testing anyway, so make sure it's preserved”.

To summarize: The *Extract Method* refactoring has the potential to not only simplify your code, but also make your tests easier. With moderate effort, you will be able to provide very high test coverage for your code. And thoroughly tested code leads to robust applications that your customers love, and that is the thing that matters in the end.

*This article was first published as [Simpler Tests thanks to “Extract Method” Refactoring](http://LukasAtkinson.de/2016/simpler-tests-thanks-to-extract-method-refactoring/) on [LukasAtkinson.de](http://LukasAtkinson.de/).*

****
