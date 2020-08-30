---
title: Java 8 Stream to Single Element
description: Java 8 stream Collector to return a single element as either an Optional (toOptional()) or Nullable (toNullable())
layout: post
categories: [java]
---

A situation I find myself in from time to time is needing to filter a `List` down to what I expect to be either a single element or nothing.

What I'm aiming for is to be able to do something like this ...

```java
return states.stream()
        .filter(State::isActive())
        .collect(toOptional())
        .orElseThrow();
```

Lets get started. I'm going to implement this as a `static` helper ...

```java
public class StreamUtils {
    private StreamUtils() { }

    // TODO
}
```

Clearly we're going to need to be able to reduce a `Collection` down to a single `Optional` element ...

```java
private static <T> Optional<T> findOnly(Collection<T> collection) {
    if (collection.size() > 1) {
        throw new IllegalStateException("Expected zero or one element");
    }
    return collection.stream().findFirst();
}
```

Now we need to define a `Collector`, something like [`toList()`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toList--) ...

```java
public static <T> Collector<T, ?, Optional<T>> toOptional() {
    // TODO
}
```

Right, now what? Well we have a `stream` and a method that will take a `Collection` and reduce it to a single element, so all thats left is to tie the two together. One way to do this is to use `toList` and then operate on the result - which is exactly what [`collectingAndThen`](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#collectingAndThen-java.util.stream.Collector-java.util.function.Function-) will allow us to do ...

```java
public static <T> Collector<T, ?, Optional<T>> toOptional() {
    return collectingAndThen(toList(), StreamUtils::findOnly);
}
```

And there we have it - done!

#### Bonus Round

For cases where `null` would be preferable to an empty `Optional` ...

```java
public static <T> Collector<T, ?, T> toNullable() {
    return collectingAndThen(toList(), l -> findOnly(l).orElse(null));
}
```
