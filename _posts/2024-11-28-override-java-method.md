---
layout: post
title:  "Override any Java method at runtime"
date:   2024-11-28
categories: java
description: Override any Java method at runtime. From your codebase or from external library
---
Using the awesome `LibCustom` library, you can override any function at runtime.
This is especially useful in tests where you have external dependencies that you cannot easily mock.
Also useful when your codebase is not very modular and there are parts of the logic that calls other methods that you want to override with mocks.

First add `LibCustom` library to your `pom.xml`:

{% highlight xml %}
<dependency>
  <groupId>io.github.jleblanc64</groupId>
  <artifactId>lib-custom</artifactId>
  <version>1.0.4</version>
</dependency>
{% endhighlight %}

Say you have a class `A`, with a method `add`:

{% highlight java %}
class A {

  static int add(int a, int b) {
    return a + b;
  }
}
{% endhighlight %}

As is, the method returns `A.add(2,3) = 5`. Let's say that we want to override `add` with `multiply` at runtime:

{% highlight java %}
LibCustom.override(A.class, "add", args -> {
  var a = args[0];
  var b = args[1];
  return a * b;
})
{% endhighlight %}

Now, the method returns `A.add(2,3) = 6`
