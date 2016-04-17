---
layout: post
title:  OOP is not about real-world objects
date:   2016-04-17 23:50:00 -0000
categories: coding
author:	Ixrec
---

Traditional OOP education likes to talk about `Dog` inheriting from `Animal` so that `myAnimal->makeSound()`
prints out "Bark!" instead of "Unspecified animal sound!". Unfortunately, students often see far more of
these toy examples than they do more realistic and useful examples of classes, objects and inheritance. Thus,
one of the most common and preventable mistakes I see newbies make is starting a project by thinking about
the various real-world objects the program might be imitating, rather than thinking about what the program will actually *do*.

I will attempt to demonstrate what I mean with the hopefully-not-too-trivial toy example of a command line tool
for ordering pizza. Let's say we want to enter commands like `pizza add "hawaiian 9 inch"` and `pizza address
"1234 Fake Street"` to prepare the order, and then `pizza deliver` will print out a summary of the order plus
ASCII art of the pizzas.

The typical "real-world objects" approach might start out by producing some psuedocode like this:
```
class Pizza {
	private Topping[];
	...
}

class Customer {
	private int id;
	private long creditCardNumber;
	...
}

class DeliveryVan {
	private long serialNumber;
	private Person driver;
	...
}

class House {
	private std::string streetAddress;
	...
}

// and we all know that a pepperoni pizza "is-a" pizza, so...
class PepperoniPizza : public Pizza {
    private Topping[] toppings = { Topping::PEPPERONI };
	...
}
```
After all, that's basically all of the major objects involved in delivering a real-world pizza, right?

Now imagine this is how far you've gotten in your design when someone asks you one of these questions:

- How do I actually order a pizza?
- How would we add non-pizza items like drinks or ice cream or movie rentals to this ordering system?
- How do we ensure we never get asked to deliver to an address over half an hour from our nearest store?

At the moment, there's simply nothing in our pseudocode that would help answer these questions. We'd have to either say we don't know or start making guesses about code we haven't written yet. You might object that we've only jotted down about 30 lines of pseudocode so far, so it's no surprise we can't answer any questions about the system yet. That's why I'm going to show the alternative right now.

If you instead start by thinking about what the program has to do, and sketch what functions will get called with what arguments, you'll might get pseudocode more like this:
```
int main() {
	Order currentOrder;
	Address deliveryAddress;
	// wait for commands to come in from stdin and call appropriate handlers
}

void handleAddPizzaToOrderCommand(std::string& input, Order& order) {
	// parse input...
	Pizza pizza( ... );
	order.addPizza(pizza);
}

void handleSetDeliveryAddress(std::string& input, Address& address) {
	// parsing logic...
	address.street = parsedStreet;
	// other fields...
}

void deliverOrder(const Order& order) {
	// summarize contents
	// iterate over each of the pizzas {
		std::cout << currentPizza.toASCIIArt() << std::endl;
	}
}
```
Now let's take a look at those hypothetical questions again.

* **Q:** How do I actually order a pizza?  
  **A:** You call handleAddPizzaToOrderCommand(), then handleSetDeliveryAddress(), then deliverOrder(). Done.

* **Q:** How would we add non-pizza items like drinks or ice cream or movie rentals to this ordering system?  
  **A:** We would add more fields to the Order class, a new handler for non-pizza items, and some extra logic
  to deliverOrder() to print those items.

* **Q:** How do we ensure we never get asked to deliver to an address over half an hour from our nearest store?  
  **A:** We'd need to add some code in handleSetDeliveryAddress() that knows where our stores are, and knows how
  to calculate distances between our stores and arbitrary addresses.

Is this the best possible way to design a command line pizza app? Probably not. These are just the first things I
scribbled down that seemed to get the point across. Are these the best possible answers to these hypothetical questions?
No. In fact, the second answer shows that we'd have to change several parts of the code to introduce a new item,
which is a sign we could improve the design significantly.

My point is that **by starting with what the program will do, instead of what the real world has in it, you
immediately get meaningful pseudocode and design ideas that you can do useful work on**. The second pseudocode
sample is something we can evaluate, critique, change, improve, and even implement fairly easily. The first sample
doesn't allow us to do any of those things (the closest we can get is unhelpful philosophical debates such as whether
a DeliveryVan should know what Pepperoni is), and that's because it's just a bunch of classes without any code
using them. In fact, the second sample would take at most a few minutes to flesh out into a compilable, executable
program that you could demonstrate to your manager for feedback, start writing unit/integration tests for, and all
those other practical things that programmers are supposed to be doing.

The term "object-oriented" does *not* mean that all your thinking is supposed to revolve around classes and objects.
Your thinking should always revolve around information and behavior (or "data structures" and "algorithms" if you
prefer). What "object-oriented" ought to mean is that whenever we see some behavior that's always tied to a particular
type of information, we should consider bundling that behavior and that information into a single entity called an object.

