---
title: 'Design Patterns in the 21st Century: The Abstract Factory Pattern'
author: Samir Talwar
date: 2015-04-14T10:00:00Z

image:
  src: /assets/img/custom/blog/2015-04-13-design-patterns/part-two.png
  attribution:
    href: https://flic.kr/p/8eq5hL
    text: Van Nelle Factory IR, by Vincent van der Pas
canonical:
  href: http://monospacedmonologues.com/post/116370706560/design-patterns-in-the-21st-century-the-abstract
  name: my personal blog
tags:
  - design-patterns
  - functional-programming
  - java

type: blog
layout: post
slug: design-patterns-in-the-21st-century-part-two
aliases: 
  - /2015/04/14/design-patterns-in-the-21st-century-part-two/
---

This is part two of my talk, [Design Patterns in the 21st Century][].

[Design Patterns in the 21st Century]: http://talks.samirtalwar.com/design-patterns-in-the-21st-century.html

---

This pattern is used *everywhere* in Java code, especially in more "enterprisey" code bases. It involves an interface and an implementation. The interface looks something like this:

~~~java
public interface Bakery {
    Pastry bakePastry(Topping topping);
    Cake bakeCake();
}
~~~

And the implementation:

~~~java
public class DanishBakery implements Bakery {
    @Override public Pastry bakePastry(Topping topping) {
        return new DanishPastry(topping);
    }

    @Override public Cake bakeCake() {
        return new Aeblekage(); // mmmm, apple cake...
    }
}
~~~

More generally, the Abstract Factory pattern is usually implemented according to this structure.

{{< blog_image src="/assets/img/custom/blog/2015-04-13-design-patterns/abstract-factory-pattern-uml.png" title="Abstract Factory pattern UML diagram" >}}

In this example, `Pastry` and `Cake` are "abstract products", and `Bakery` is an "abstract factory". Their implementations are the concrete variants.

Now, that's a fairly general example.

In actual fact, most factories only have one "create" method.

~~~java
@FunctionalInterface
public interface Bakery {
    Pastry bakePastry(Topping topping);
}
~~~

Oh look, it's a function.

This denegerate case is pretty common in in the Abstract Factory pattern, as well as many others. While most of them provide for lots of discrete pieces of functionality, and so have lots of methods, we often tend to break them up into single-method types, either for flexibility or because we just don't need more than one thing at a time.

So how would we implement this pastry maker?

~~~java
public class DanishBakery implements Bakery {
    @Override public Pastry apply(Topping topping) {
        return new DanishPastry(Topping topping);
    }
}
~~~

OK, sure, that was easy. It looks the same as the earlier `DanishBakery` except it can't make cake. Delicious apple cake… what's the point of that?

Well, if you remember, `Bakery` has a **Single Abstract Method**. This means it's a **Functional Interface**.

So what's the functional equivalent to this?

~~~java
Bakery danishBakery = topping -> new DanishPastry(topping);
~~~

Or even:

~~~java
Bakery danishBakery = DanishPastry::new;
~~~

Voila. Our `DanishBakery` class has gone.

But we can go further.

~~~java
package java.util.function;
/**
 * Represents a function that
 * accepts one argument and produces a result.
 *
 * @since 1.8
 */
@FunctionalInterface
public interface Function<T, R> {
    /**
     * Applies this function to the given argument.
     */
    R apply(T t);

    ...
}
~~~

We can replace the `Bakery` with `Function<Topping, Pastry>`; they have the same types.

~~~java
Function<Topping, Pastry> danishBakery = DanishPastry::new;
~~~

In this case, we might want to keep it, as it has a name relevant to our business, but often, `Factory`-like objects serve no real domain purpose except to help us decouple our code. (`UserServiceFactory`, anyone?) This is brilliant, but on these occasions, we don't need explicit classes for it—Java 8 has a bunch of interfaces built in, such as `Function`, `Supplier` and many more in the `java.util.function` package, that suit our needs fairly well.

Here's our updated UML diagram:

{{< blog_image src="/assets/img/custom/blog/2015-04-13-design-patterns/abstract-factory-pattern-uml-functional.png" title="Updated Abstract Factory pattern UML diagram" >}}

Aaaaaah. Much better.

---

Tomorrow, we'll be looking at the Adapter pattern.
