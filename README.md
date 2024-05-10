# Java Advanced: Functional, Asynchronous and Reactive Programming

Summary:

1. Functional Java: lambda expression, functional interfaces, streams

2. Executor framework and Fork Join pool

3. NIO - non-blocking input/output

4. Asynchronous Java (Completable Future)

5. Reactive Streams (Java 9)

6. RxJava 2

7. R2DBC (reactive JDBC replacement)

8. Spring WebFlux (Reactor)

9. Reactive Spring Data JPA

## 1. Lambda Expression, Functional Interfaces and Method References

## 1.1. A Lambda Expression

Let’s use an anonymous class

```java
FileFilter fileFilter = new FileFilter() {
  @Override
  public boolean accept(File file)
  {
    return file.getName().endsWith(".java");
  }
};
```

We take the parameters and return:

```java
FileFilter filter = (File file) -> file.getName().endsWith(".java");
```

This is a lambda expression.

## 1.2. Several Ways of Writing a Lambda Expression

The simplest way:

```java
FileFilter filter = (File file) -> file.getName().endsWith(".java");
```

If you have more than one line of code:

```java
Runnable r = () -> {
  for (int i = 0; i < 5; i++) {
     System.out.println("Hello world!");
  }
};
```

If you have more than one argument:

```java
Comparator<String> c = (String s1, String s2) -> Integer.compare(s1.length(), s2.length());
```

## 1.3. Functional Interface

A functional interface is one interface with only one abstract method (methods from class Object do not count)

```java
public interface Runnable {
  run();
}

public interface Comparator<T> {
  int compareTo(T t1, T t2);
}

public interface FileFilter {
  boolean accept(File pathname);
}
```

A functional interface can be annotated:

```java
@FunctionalInterface
public interface MyFunctionalInterface {
  someMethod();
  /**
  * Some more documentation
  */  equals(Object o);
};
```

The annotation is here just for convenience, as the compiler will tell me whether the interface is functional or not

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/17bd8b54-bbd0-40c4-a522-ee01c684015b)

```java
Predicate<Integer> isAdult = age -> age >= 18:
isAdult.test(10);
Consumer<String> printer = p -> System.out.println("Printed: "+p);
printer.accept("hi");
Supplier<String> sayHi = () -> "hi";
sayHi.get(); //hi
```

## Package java.util.function

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/08a6f283-b861-49c1-bf5f-67bee8e98f62)

## Omitting Parameter

```java
Comparator<String> c = (String s1, String s2) -> Integer.compare(s1.length(), s2.length());
```

```java
Comparator<String> c = (s1, s2) -> Integer.compare(s1.length(), s2.length());
```

### 1.3.1. Supplier

A Supplier provides a result of a given type. It does not accept any arguments

```java
import java.util.function.Supplier;

public class SupplierExample {
    public static void main(String[] args) {
        Supplier<String> stringSupplier = () -> "Hello World";
        System.out.println(stringSupplier.get());
    }
}
```

### 1.3.2. Predicate

A Predicate takes one argument and returns a boolean

```java
import java.util.function.Predicate;

public class PredicateExample {
    public static void main(String[] args) {
        Predicate<Integer> isPositive = x -> x > 0;
        System.out.println(isPositive.test(5));  // true
        System.out.println(isPositive.test(-5)); // false
    }
}
```

### 1.3.3. Function

A Function takes one argument and produces a result

```java
import java.util.function.Function;

public class FunctionExample {
    public static void main(String[] args) {
        Function<Integer, String> convert = x -> "Number: " + x;
        System.out.println(convert.apply(5));
    }
}
```

### 1.3.4. Consumer / BiConsumer

A Consumer performs an operation on a single argument, whereas a BiConsumer takes two arguments

```java
import java.util.function.Consumer;
import java.util.function.BiConsumer;

public class ConsumerExample {
    public static void main(String[] args) {
        Consumer<String> printer = System.out::println;
        printer.accept("Hello Consumer!");

        BiConsumer<Integer, Integer> adder = (a, b) -> System.out.println("Sum: " + (a + b));
        adder.accept(5, 3);
    }
}
```

### 1.3.5. BiPredicate

A BiPredicate takes two arguments and returns a boolean

```java
import java.util.function.BiPredicate;

public class BiPredicateExample {
    public static void main(String[] args) {
        BiPredicate<Integer, Integer> compare = (a, b) -> a > b;
        System.out.println(compare.test(5, 3));  // true
        System.out.println(compare.test(2, 3));  // false
    }
}
```

### 1.3.6. Unary Operator

A UnaryOperator takes one argument and returns a result of the same type

```java
import java.util.function.UnaryOperator;

public class UnaryOperatorExample {
    public static void main(String[] args) {
        UnaryOperator<Integer> square = x -> x * x;
        System.out.println(square.apply(5));
    }
}
```

## 1.4. Method References

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/74c2e424-4298-441d-bead-aad422b585b0)

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/20579c83-8af3-4221-9007-f11d20d34fdc)

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/0e334b77-d695-4547-840d-65b5cddadb8c)

## Reference to an Instance Method of an Arbitrary Object of a Particular Type

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/115367ee-7036-4357-9ec5-3ddf8ca6094f)

## Reference to Constructor

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/65dc8eaa-e0b6-4b64-9206-61d61b060429)

## 1.5. Data Streams

What is a **Stream**?

•	An object on which one can define operations

•	An object that does not hold any data

•	An object that should not change the data it processes

•	An object able to process data in « one pass »

•	An object optimized from the algorithm point of view, and able to process data in parallel

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/9a493946-01a3-48fd-a9ca-c2618734d01f)

How to Create a Stream?

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/7b4167d0-bb0b-4727-8dcc-69e01f365611)

## 1.6. Map, Filter, Reduce

Let’s take a list of Person:

```java
List<Person> list = new ArrayList<>() ;
```

Suppose we want to compute the "average of the age of people older than 20"

Let’s first convert it into stream…

```java
List<Person> persons = new ArrayList<>() ;
persons.stream()
```

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/b59c438c-209b-4b4b-aae1-7b15d3aad893)

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/8412180f-b0b1-419d-95c5-fb768a4dc790)

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/3e38bf4b-0834-4937-a58d-036244df804c)

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/5326c0eb-785e-4de3-aca8-dee47edeb515)

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/655d5cd8-c693-4b3d-864a-cdd84877402f)

## 1.7. foreach (consumer)

forEach(Consumer consumer) iterates over all stream elements and applies the consumer

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

Consumer<T> is a functional interface. It can be implemented by a lambda expression

```java
Consumer<T> c = p -> System.out.println(p);
 Consumer<T> c = System.out::println;
 Stream.of(1,2,3).forEach(c);
```





