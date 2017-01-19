---
layout: post
title:  String literals in Java classes
date:   2017-01-18 20:14:13 -0600
categories: java string design
author:	MichaelT
---

## Some background

More than occasionally I've seen Java code that looks like:

{% highlight java %}
StringBuilder query = new StringBuilder();
query.append("SELECT something, somethingelse");
query.append(" FROM table");
query.append(" WHERE 1=1");
ResultSet rs = stmt.execuateQuery(query.toString());
{% endhighlight %}

or something to that effect.  The key point of this being the
call of several string constants in `append` one right after another.

pmd [complains][pmd] about this, but the reasons why is rather vague
at best.  So, the question of "why shouldn't you do this?"

Lets write some code.  Two classes...

{% highlight java %}
public class Foo {
    public static void main(String... args) {
        String foo = "foo" + "bar";
	System.out.println(foo);
    }
}
{% endhighlight %}

{% highlight java %}
public class Bar {
    public static void main(String... args) {
        StringBuilder foo = new StringBuilder();
	foo.append("foo");
	foo.append("bar");
	System.out.println(foo.toString());
    }
}
{% endhighlight %}

The above are compiled to two separate classes with `javac`.

The first thing to notice is the size of the files:
```
-rw-r--r--  1 shagie  staff  593 Jan 17 19:02 Bar.class
-rw-r--r--  1 shagie  staff  199 Jan 17 19:01 Bar.java
-rw-r--r--  1 shagie  staff  412 Jan 17 19:01 Foo.class
-rw-r--r--  1 shagie  staff  135 Jan 17 19:01 Foo.java
```

So, lets look at those files with the Java disassembler - [javap][javap].

```
~/De/javastr $ javap -v -c Foo.class 
Classfile /Users/shagie/Development/javastr/Foo.class
  Last modified Jan 17, 2017; size 412 bytes
  MD5 checksum 0aa784d3599dd09977b9ad6feab73dea
  Compiled from "Foo.java"
public class Foo
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
   #1 = Methodref          #6.#15         // java/lang/Object."<init>":()V
   #2 = String             #16            // foobar
   #3 = Fieldref           #17.#18        // java/lang/System.out:Ljava/io/PrintStream;
   #4 = Methodref          #19.#20        // java/io/PrintStream.println:(Ljava/lang/String;)V
   #5 = Class              #21            // Foo
   #6 = Class              #22            // java/lang/Object
```

That list is the start of the constant pool of the class file.
It is defined in [Section 4.1 of the jvm spec][jvm]:

> The constant_pool is a table of structures (§4.4) representing various string constants, class and interface names, field names, and other constants that are referred to within the ClassFile structure and its substructures. The format of each constant_pool table entry is indicated by its first "tag" byte.

The thing to note here is that the String `foobar` is one string - not
two.

Further down, when we look at the bytecode invoked for the main
method:

```
  public static void main(java.lang.String...);
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC, ACC_VARARGS
    Code:
      stack=2, locals=2, args_size=1
         0: ldc           #2                  // String foobar
         2: astore_1
         3: getstatic     #3                  // Field java/lang/System.out:Ljava/io/PrintStream;
         6: aload_1
         7: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        10: return
}
```

You can see the `ldc #2` which is the load constant #2 from the
constant pool and its right there.  One constant, one instruction.


Looking at Bar.class now,

```
Classfile /Users/shagie/Development/javastr/Bar.class
  Last modified Jan 17, 2017; size 593 bytes
  MD5 checksum ef1e0f7d6be2af193a44b83bd97751f6
  Compiled from "Bar.java"
public class Bar
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
   #1 = Methodref          #11.#20        // java/lang/Object."<init>":()V
   #2 = Class              #21            // java/lang/StringBuilder
   #3 = Methodref          #2.#20         // java/lang/StringBuilder."<init>":()V
   #4 = String             #22            // foo
   #5 = Methodref          #2.#23         // java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
   #6 = String             #24            // bar
```
In here you can see the two string constants `foo` and `bar` in
slots 4 and 6 of the constant pool.  While I didn't show them all,
Foo.class had 28 constants and Bar.class ahs 41 constants.  These
point to various things in the code such as the method or `UTF-8`,
boring stuff like that.  But its half again as much for the append
approach.

When looking at the bytecode itself,

```
  public static void main(java.lang.String...);
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC, ACC_VARARGS
    Code:
      stack=2, locals=2, args_size=1
         0: new           #2                  // class java/lang/StringBuilder
         3: dup
         4: invokespecial #3                  // Method java/lang/StringBuilder."<init>":()V
         7: astore_1
         8: aload_1
         9: ldc           #4                  // String foo
        11: invokevirtual #5                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        14: pop
        15: aload_1
        16: ldc           #6                  // String bar
        18: invokevirtual #5                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        21: pop
        22: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
        25: aload_1
        26: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
        29: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        32: return
}
```

you can see the StringBuilder getting created, some stack work with
the dup, the invocation of init, storing the value, loading it,
calling load constant once for `foo`, invoking the append, poping
the stack, loading the string builder again, loading the constant,
invoking the virtual method and so on.

The point of all this?  Don't use a StringBuilder to build a
literal string.  `javac` will build that literal string as part
of compilation even if you use `+`.

Chaining the `+` operator on String is the simplest to understand
and most likely correct way to build a String in any situation
outside of a loop (if you're building a String and there's a loop,
use a StringBuilder).

[pmd]: http://pmd.sourceforge.net/pmd-4.3.0/rules/strings.html#ConsecutiveLiteralAppends 
[javap]: http://docs.oracle.com/javase/7/docs/technotes/tools/windows/javap.html
[jvm]: https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-4.html
