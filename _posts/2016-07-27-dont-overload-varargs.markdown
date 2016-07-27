---
layout: post
title:  Don't overload varargs
categories: java debugging warstory
author: MichaelT
---

Don't overload varargs. There's even a cert advisory on this:
[Avoid ambiguous overloading of varargs methods][cert-advisory].
And my tale of woe is one that [reading the docs][spring-docs] with a
better understanding of what _isn't_ explicitly said would have saved
an hour of two of trying to track down the error.

Lets start off by writing some code.

{% highlight java %}
public class Main {
    public static void main(String[] args) {
        Bar obj = new Bar();
        obj.func("foo", "bar");
    }

    static class Foo {
        final public boolean func(Object... args) {
            for(Object arg : args) {
                System.out.println("foo: " + arg.toString());
            }
            return true;
        }
    }

    static class Bar extends Foo {
        final void func(String arg1, String arg2) {
            System.out.println("bar arg1: " + arg1);
            System.out.println("bar arg2: " + arg2);
        }
    }
}
{% endhighlight %}

```
bar arg1: foo
bar arg2: bar
```

This example has a class `Bar` that has a method that happens to
overload a particular set of argument calls - that of two `String`
objects. This isn't great, but it worked for a few years without
incident.

In a recent batch of changes, another argument was added to the
method. And it compiled without warning - after all, it was still all
valid code.

{% highlight java %}
public class Main {
    public static void main(String[] args) {
        Bar obj = new Bar();
        obj.func("foo", "bar");
    }

    static class Foo {
        final public boolean func(Object... args) {
            for(Object arg : args) {
                System.out.println("foo: " + arg.toString());
            }
            return true;
        }
    }

    static class Bar extends Foo {
        final void func(String arg1, String arg2, String arg3) {
            System.out.println("bar arg1: " + arg1);
            System.out.println("bar arg2: " + arg2);
            System.out.println("bar arg3: " + arg3);
        }
    }
}
{% endhighlight %}
```
foo: foo
foo: bar
```

But _now_, the method invoked is that of the varargs one in the
base class that is happily accepting any parameter combination
you send at it.  If you're lucky, it will throw an exception your
way so you can read the stack trace and go "no, that's not right."

This particular debugging was around Spring's [StoredProcedure class][spring-docs].
The class documentation for this reads:

> Superclass for object abstractions of RDBMS stored procedures. This
> class is abstract and it is intended that subclasses will provide a
> typed method for invocation that delegates to the supplied 
> `execute(java.lang.Object...)` method.

... and that's what a previous developer did.  Created a method that
overloaded `execute` with their own typed method.

Further down in the documentation (for the Map invocation) reads:

> Subclasses should define a strongly typed execute method (**with a
> meaningful name**) that invokes this method, populating the input map
> and extracting typed values from the output map. 

The emphasis is mine.

By failing to provide a meaningful name and instead overloading the
`execute` method, a change of the number of parameters didn't result
in a compile time error.  The error that showed up instead was that 
of a runtime exception saying that the number of arguments wasn't right.

Look through the `declareParameter` calls, yep, that matches up with
the PL/SQL argument list. Check the assignments into the parameter map,
yep, that matches up with the declared parameters, and there are five
(not four - like the runtime exception is claiming).

With a bit of "why isn't this working" hitting F3 in eclipse to
find the resolved code... takes me into the no source attached
`StoredProcedure` file?! And then it starts to all make sense.

A developer changed the method definition and either forgot to
or was expecting another developer to make the corresponding changes
in the method invocation.  Add the arguments and everything works.

In the process of going through and refactoring these so that they
would have a meaningful name, I even found one instance where
a parameter list was changed from `execute(String, Set<String>)` to
`execute(String, java.sql.Array)` ... and one of the invocations
was missed in that change and so `execute(idType, idSet)` was
suddenly calling the base class method (because it didn't match
the subclassed method signature). Whats more insidious with this
particular invocation was that the argument list matched, and
the stored procedure _was_ called... but because the method was
expected to be `void` and populating two fields was now quietly
failing (or succeeding?) and throwing away the data. Looking at
svn blame, this appears to have been the case for three years.

These errors are immensely difficult to track down (the worst are
the ones that 'work' and don't throw exceptions - but don't do
what they're supposed to). The amount of time that gets wasted
in tracking down bugs from these is much more than just having
one of those reminders in the back of your head "this is something
that can go very badly."

Don't overload varargs.

---

## So, what could have been done?

The most disappointing thing in this was finding out that `final`
won't help.  A `final` method cannot be overridden. Maybe if Spring
was to have declared their method as `final execute(Object... params)`
the original developer wouldn't have been able to facilitate this bug
in the future.

But alas, this is _overloading_ not _overriding_ and so the with a
more careful reading of what the [JLS][jls-final] says,
`final` _doesn't_ (and can't) help here.

The `final` keyword would prevent someone from declaring a new method
_with the same argument list_ - that of `execute(Object... params)`.
But since `execute(String arg1, String arg2)` isn't the same there
is no way to prevent someone from overloading the parameter list, and
instead all that is there is a weakly worded hint "don't name it
execute - give it a meaningful name."

I'll also point out that neither the Eclipse nor IntelliJ built in
inspections do more than hint at this being a problem.  All I get
are a 'unused method' in the above example. But in a larger library
project with a public method invocation this doesn't clearly show
up.  Neither PMD nor FindBugs identify this potential hazard either.

So there's my cautionary tale. Don't overload a varargs method. If
you do, you're just asking for trouble when someone later does and
wonders why this code isn't getting run and the errors aren't
showing up in the right place - if at all.

[cert-advisory]: https://www.securecoding.cert.org/confluence/display/java/VOID+DCL08-J.+Avoid+ambiguous+overloading+of+varargs+methods
[spring-docs]: http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/jdbc/object/StoredProcedure.html
[jls-final]: https://docs.oracle.com/javase/specs/jls/se7/html/jls-8.html#jls-8.4.3.3
