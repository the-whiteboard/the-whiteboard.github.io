---
layout: post
title:  
categories: jekyll update
---

[Project Euler][project-euler] is a collection of math problems designed
to be solved with the aide of a computer. These range from fairly simple
to things that you would likely need a few upper level classes at the
university to solve (or more).

There are different things that a person can get out of doing Project
Euler. At one end of the spectrum, its possible to learn or be challenged
with some advanced math problems. At the other end, its a set of
fairly consistent problems that need some basic familiarity with a
given programming language to solve.

As I'm writing this from a programming perspective, I'm much more
interested in the programing end of the spectrum rather than the math
end of the challenges.

To demonstrate this, lets look at the first Project Euler problem - likely
quite familiar to many programmers as fizz buzz (or a math oriented version
of it). [Multiples of 3 and 5][pe-1].

I'm going to start off with saying that if you are interested in the math
end, there is a O(1) solution to it.  The "problem" with this O(1) solution
to Project Euler 1 is that it doesn't challenge the knowledge of programming
itself.

Its been remarked that a "real programmer can write FORTRAN in any language"
(and they [Don't use PASCAL][real-programmer] or eat quiche). And this is
the primary challenge for a person looking at the programming end of the
spectrum of Project Euler challenges - to _not_ write the language that
is already known with a different syntax.

Lets look at some solutions to Project Euler problem 1 and consider the
variety of options out there and the challenge of writing idiomatic code
appropriate for that language.

I'm going to start out with Java pre-8.

{% highlight java %}
package net.shagie.euler.java.problems;

public class Problem001 {
    public static void main(String[] args) {
        int sum = 0;
        for (int i = 0; i < 1000; i++) {
            if(i % 3 == 0 || i % 5 == 0) {
                sum += i;
            }
        }
        System.out.println(sum);
    }
}
{% endhighlight %}

This is probably how one would write it in C, Pascal, dare I say FORTRAN
or any number of languages that were around in the late 80s. 

Going to Java 8, we get:

{% highlight java %}
package net.shagie.euler.java.problems;

import java.util.stream.IntStream;

public class Problem001 {
    public static void main(String[] args) {
        System.out.println(
            IntStream.range(1,1000)
            .filter(i -> i % 3 == 0 || i % 5 == 0)
            .sum());
    }
}
{% endhighlight %}

One can see that this is a very different solution and approach to
processing. One could certainly write the first code in Java 8, but the
idiomatic Java 8 takes advantage of the streams that are provided with
the language.

In Scala, one can see the same idea as the Java 8 Streams (and in all
fairness, Scala had them first).

{% highlight scala %}
package net.shagie.euler.scala

object Problem001 extends App {
  println((1 to 999).filter((i: Int) => i % 3 == 0 || i % 5 == 0).sum)
}
{% endhighlight %}

And again the idea shows up in Groovy:

{% highlight groovy %}
package net.shagie.euler.groovy

def list = (1 .. 999)

println list.findAll { (it % 3 == 0) || (it % 5 == 0) }.sum()
{% endhighlight %}

Some classic perl5:

{% highlight perl %}
#!/usr/bin/perl

my $sum = 0;
foreach (grep { $_ % 3 == 0 or $_ % 5 == 0 } 1 .. 999) {
    $sum += $_;
}

print $sum;
{% endhighlight %}

And then some perl6:

{% highlight perl %}
#!/usr/local/bin/perl6

(1 ..^ 1000).grep({ ! ($^a % 3 and $^a % 5) }).reduce({ $^a + $^b }).say;
{% endhighlight %}

And one can see how much the language changed (it took me a few readings
to properly understand what all those `^` are doing in the code now).

I really thing that its very useful to also write in a language that
really challenges how you think about code.

And while the various forms of streams show up in Java 8, Scala, Perl6 and
Groovy... sometimes you need something really different.

{% highlight clojure %}
(defn mult-of [n] #(zero? (mod % n)))
(def mult-of-3 (mult-of 3))
(def mult-of-5 (mult-of 5))
(defn mult-of-3-or-5 [n] (or (mult-of-3 n) (mult-of-5 n)))

(println (reduce + (filter mult-of-3-or-5 (range 1000))))
{% endhighlight %}

Yes, the `range`, `filter`, and `reduce` are there - but the underlying
how you think of building the solution to the problem is changed.  Yes,
I'll admit that it could be written more concisely, and as I'm not a
clojure master, this may not even be ideally idiomatic. However, it
does display writing functions to return functions.

It is possible to write that first Java solution in any of these languages.
And to that extent, writing the classic imperative solution to the problem
doesn't help anyone learn more than the syntax of the language while at
the same time missing out on the big picture of the concepts behind
and within the language. Writing FORTRAN solutions to all the problems in
other languages and thinking that you're learning the language is like
making various pizzas each day (not even pizza napoletana) and thinking
you're learning Italian cooking.

The O(1) solution? Lets try some forth.

{% highlight forth %}
: sum ( n -- n )
   dup 1 + * 2 / ;

: div-sum ( n1 n2 -- n)
   swap over / sum * ;

999 3 div-sum
999 5 div-sum
+
999 15 div-sum
-
.s cr
{% endhighlight %}

[fortran-acm]: http://queue.acm.org/detail.cfm?id=1039535
[real-programmer]: http://web.mit.edu/humor/Computers/real.programmers
[pe-1]: https://projecteuler.net/problem=1
[project-euler]: https://projecteuler.net/about
