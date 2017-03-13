---
layout: post
title:  On the dangers of deserializing to Object
categories: java debugging warstory
author: MichaelT
---

I got bit by a bug from a number in JSON that was really a date and
the generic deserializer that provides an Object that you'll need to
cast to something to use it.  Lets start off with the code and the
error.

{% highlight java %}
package net.shagie;

import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.IOException;
import java.util.*;

public class Christmas {
    public static void main(String[] args) throws IOException {
        List<Date> cals = new ArrayList<Date>(11);
        for (int year = 1965; year <= 1975; year++) {
            Calendar cal = Calendar.getInstance();
            cal.clear();
            cal.set(year,  Calendar.DECEMBER, 25);
            cals.add(cal.getTime());
        }
        ObjectMapper om = new ObjectMapper();

        String json = om.writeValueAsString(cals);

        System.out.println(json);

        List<Object> fromJson = om.readValue(json, ArrayList.class);
        for(Object value : fromJson) {
            Date date = new Date((Long) value);
            System.out.println(date);
        }
    }
}
{% endhighlight %}

This generates the list of Dates for Christmas for the years
1965 to 1975.  It then prints out the JSON, and then converts
the list back to a list of Dates and prints out that date.

```
[-126813600000,-95277600000,-63741600000,-32119200000,-583200000,30952800000,62488800000,94111200000,125647200000,157183200000,188719200000]
Sat Dec 25 00:00:00 CST 1965
Sun Dec 25 00:00:00 CST 1966
Mon Dec 25 00:00:00 CST 1967
Wed Dec 25 00:00:00 CST 1968
Exception in thread "main" java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.Long
	at net.shagie.Christmas.main(Christmas.java:25)
```

Ok, something happened here.  Somehow, that list of Longs had an
Integer sneak in there for December 25, 1969.  That should catch
some eyes because its rather close to a very special date - January
1, 1970.  The time for midnight UTC January 1, 1970 is the epoch time
for Unix - the 0 time.  

Java uses the number of milliseconds since the epoch for its internal
date representation.

The deserializer for JSON doesn't know that this is a Date underneath
and so is just pulling it into a List of number types.  It checks to
see if the number is possibly an Integer (between -2<sup>31</sup> and
2<sup>31</sup> - 1).
If not, it tries to put it in a Long instead.

2<sup>31</sup> milliseconds is 24.85 days. So, from the 7th of December, 1969
to  the 25th of January, 1970; the Java internal representation of the time
will fit in an Integer.  And Jackson nicely puts it in as an Integer.

There are a two ways to fix this.  One is to tell Jackson to
have always use a Long.  `DeserializationFeature.USE_LONG_FOR_INTS`

And indeed, tossing the following line into the above code will
let it run:

{% highlight java %}
om.enable(DeserializationFeature.USE_LONG_FOR_INTS);
{% endhighlight %}

But this ignores that the data is really a Date behind that number.
It may work in this instance where the data is simple, but in situations
where there are actually Integers and Longs (and BigInteger too) all side
by side this will cause more problems than it is worth. The thing is, these
aren't really Long numbers.  They are Dates that happen to be serialized as
a Long.

Thus, the proper approach to this is not to allow a sloppy Object to be tossed
around (however convenient it may be to not specify the serialization contract
and handle the parse exceptions), but rather to properly deserialize back to
the Date from which it came.

{% highlight java %}
List<Date> listFromJson = om.readValue(json, new TypeReference<List<Date>>() {});
Date[] arrayFromJson = om.readValue(json, Date[].class);
{% endhighlight %}

Those are the approaches for the List or array types.  The type erasure from
Java necessitates the [TypeReference][typeref] if you want a `List`, but an
[array type][array-jls] is its own type and doesn't need such contortions.

This is a problem that is particularly notorious in dates and times when
going through JSON, which lacks a Date typed object.  Thus, different
languages have taken different approaches to serializing Dates through JSON.
Some Java and C# like to use milliseconds since the epoch.  Others may
use [ISO 8601][iso-8601] dates.  There are differing opinions as to which
one is more right or least wrong.

Many (if not all) serialization libraries will properly handle the round
trip serialization of a Date (or however it is named in the language).
However, this depends on the hint (or outright statement) to the library that
the object is a Date.  Not using the library for both sides of the
serialization, and you've good a non-zero chance that someone has a date
that will break the assumptions about what a Date is.

The bugs that may occur with the improper deserialization of data are 
subtle and they come out at runtime with nuances of the deserializer that
someone may not be aware of.  The simplest and best approach to this is to
maintain the type system that the language provides - give the deserializer
every bit of information that it may want in order to produce the proper
object.  For a few more seconds of work to deserialize to the contracted
object, hours of debugging and maintenance are saved.

[typeref]: http://fasterxml.github.io/jackson-core/javadoc/2.0.0/com/fasterxml/jackson/core/type/TypeReference.html
[array-jls]: https://docs.oracle.com/javase/specs/jls/se7/html/jls-10.html#jls-10.8
[iso-8601]: https://en.wikipedia.org/wiki/ISO_8601