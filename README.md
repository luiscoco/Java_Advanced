# Java Advanced (Module A): Functional, Asynchronous and Reactive Programming 

https://www.programiz.com/java-programming/online-compiler/

Modules Summary:

A. Functional Java: lambda expression, functional interfaces, streams

B. Executor framework and Fork Join pool

C. NIO - non-blocking input/output

D. Asynchronous Java (Completable Future)

E. Reactive Streams (Java 9)

F. RxJava 2

G. R2DBC (reactive JDBC replacement)

H. Spring WebFlux (Reactor)

I. Reactive Spring Data JPA

## 1. Lambda Expression, Functional Interfaces, Method References, Streams

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

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/0d359f74-4c5f-45d3-94e7-87bdfb03fac7)

```java
Predicate<Integer> isAdult = age -> age >= 18:
isAdult.test(10);
Consumer<String> printer = p -> System.out.println("Printed: "+p);
printer.accept("hi");
Supplier<String> sayHi = () -> "hi";
sayHi.get(); //hi
```

## Omitting Parameter

```java
Comparator<String> c = (String s1, String s2) -> Integer.compare(s1.length(), s2.length());
```

```java
Comparator<String> c = (s1, s2) -> Integer.compare(s1.length(), s2.length());
```

### 1.3.1. Supplier

A Supplier provides a result of a given type. It does not accept any arguments

Definition of java.util.function.Supplier

```java
package java.util.function;

@FunctionalInterface
public interface Supplier<T> {
  T get();
}
```

Use of a Supplier:

```java
Supplier<Integer> supplier = () -> 100;
System.out.println(supplier.get());
```

Another sample:

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
package java.util.function;

@FunctionalInterface
public interface Predicate<T> {
  boolean test(T t);
}
```

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
package java.util.function;

@FunctionalInterface
public interface Function<T, R> {
  R apply(T t);
}
```

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
package java.util.function;

@FunctionalInterface
public interface Consumer<T> {
  void accept(T t);
}
```

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
@FunctionalInterface
public interface BiPredicate<T, U> {
    boolean test(T t, U u);
}
```

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
@FunctionalInterface
public interface UnaryOperator<T> extends Function<T, T> {
    // Inherits the "apply" method from Function
}
```

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

This **lambda function** s -> s.toLowerCase():

```java
Function<String, String> f = s -> s.toLowerCase();
```
Can be written like **method reference** String::toLowerCase:

```java
Function<String, String> f = String::toLowerCase;
f.apply("Hi") // hi
```

This **lambda expression** s -> System.out.println(s):

```java
Consumer<String> c = s -> System.out.println(s);
```
Can be written like **method reference** System.out::println:

```java
Consumer<String> c = System.out::println;
```

This is the sample for using the above code:

```java
import java.util.List;
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        // Create a list of strings
        List<String> names = List.of("Alice", "Bob", "Charlie", "David");

        // Create a Consumer instance using a method reference
        Consumer<String> printer = System.out::println;

        // Use the Consumer to print each name in the list
        names.forEach(printer);
    }
}
```

This **lambda expression** (i1, i2) -> Integer.compare(i1, i2):

```java
Comparator<Integer> c = (i1, i2) -> Integer.compare(i1, i2);
```

Can be written like **method reference** Integer::compare:

```java
Comparator<Integer> c = Integer::compare;
```

This is a sample for the Comparator code:

```java
import java.util.List;
import java.util.Collections;
import java.util.Comparator;

public class Main {
    public static void main(String[] args) {
        // Create a list of integers
        List<Integer> numbers = List.of(10, 5, 20, 1, 15);

        // Create a Comparator instance using a method reference
        Comparator<Integer> comparator = Integer::compare;

        // Sort the list using the Comparator
        List<Integer> sortedNumbers = new ArrayList<>(numbers); // Creating a modifiable list
        Collections.sort(sortedNumbers, comparator);

        // Print the sorted list
        System.out.println(sortedNumbers);
    }
}
```

**Another Method reference sample**

However, this method to compare the birth dates of two Person instances already exists as Person.compareByAge

You can invoke this method with a **lambda expression**:

```java
Arrays.sort(rosterAsArray, (a, b) -> Person.compareByAge(a, b));
```

```java
Arrays.sort(rosterAsArray, (Person a, Person b) ->
{
    return a.getBirthday().compareTo(b.getBirthday());
});
```

Because this lambda expression invokes an existing method, you can use a **method reference** instead of a **lambda expression**:

```java
Arrays.sort(rosterAsArray, Person::compareByAge);
```

The **method reference Person::compareByAge** is semantically the **same as the lambda expression (a, b) -> Person.compareByAge(a, b)**

This table categorizes and exemplifies the different **types of method references** in Java,

which are used to simplify the syntax of lambda expressions when a method already exists to perform the operation

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/2602e7eb-8a20-4763-b25c-2de77547b72b)

Here are simple Java examples for each **type of method reference** described:

**Reference to a Static Method**

```java
import java.util.function.Function;

public class Utils {
    public static String toUpperCase(String input) {
        return input.toUpperCase();
    }

    public static void main(String[] args) {
        Function<String, String> upperFunc = Utils::toUpperCase;
        System.out.println(upperFunc.apply("hello"));
    }
}
```

**Reference to an Instance Method of a Particular Object**

```java
import java.util.function.Supplier;

public class Example {
    private String message = "Hello, World!";

    public String getMessage() {
        return message;
    }

    public static void main(String[] args) {
        Example example = new Example();
        Supplier<String> messageSupplier = example::getMessage;
        System.out.println(messageSupplier.get());
    }
}
```

**Reference to an Instance Method of an Arbitrary Object of a Particular Type**

```java
import java.util.List;
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Charlie");
        Consumer<String> action = String::toLowerCase;
        names.forEach(name -> System.out.println(action.accept(name)));
    }
}
```

**Note**: This example demonstrates invoking the toLowerCase method on each string element of the list individually. It prints the name in lower case.

**Reference to a Constructor**

```java
import java.util.function.Function;
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        Function<Integer, ArrayList<String>> listCreator = ArrayList::new;
        ArrayList<String> strings = listCreator.apply(10); // Initializes an ArrayList with initial capacity of 10
        System.out.println(strings);
    }
}
```

## 1.5. Data Streams

What is a **Stream**?

•	An object on which one can define operations

•	An object that does not hold any data

•	An object that should not change the data it processes

•	An object able to process data in « one pass »

•	An object optimized from the algorithm point of view, and able to process data in parallel

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/9a493946-01a3-48fd-a9ca-c2618734d01f)

## How to Create a Stream?

Using static method Stream.of():

```java
Stream.of(1,2,3);
```

From array:

```java
String[] arr = {"one","two","three"};
stream = Stream.of(arr);
```		

From collection:

```java
List<Person> persons;
Stream<Person> stream = persons.stream();
```

Using generate():

```java
Stream<String> stream = Stream.generate(() -> "test").limit(10);
```

## 1.6. Map, Filter, Reduce

### 1.6.1. Map

The map operation applies a function to each element in a stream and collects the results into a new stream

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamMapExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("hello", "world", "java", "stream");
        List<String> upperCaseWords = words.stream()
                                           .map(String::toUpperCase)
                                           .collect(Collectors.toList());
        System.out.println(upperCaseWords);
    }
}
```

### 1.6.2. Filter

The filter operation uses a predicate to test each element in the stream and includes only those that pass the test

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamFilterExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        List<Integer> evenNumbers = numbers.stream()
                                           .filter(n -> n % 2 == 0)
                                           .collect(Collectors.toList());
        System.out.println(evenNumbers);
    }
}
```

### 1.6.3. Reduce

The reduce operation combines all elements in the stream to produce a single summary result

This operation takes two parameters: an initial value and a binary operator

```java
import java.util.Arrays;
import java.util.List;

public class StreamReduceExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        int sum = numbers.stream()
                         .reduce(0, Integer::sum);
        System.out.println("Sum of numbers: " + sum);
    }
}
```

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

## 1.8. Consumer Chaining

```java
List<String> list = new ArrayList<>();
Consumer<String> c1 = s -> list.add(s);
Consumer<String> c2 = s -> System.out.println(s);

List<String> list = new ArrayList<>();
Consumer<String> c1 = list::add;
Consumer<String> c2 = System.out::println;
Consumer<String> c3 = c1.andThen(c2);
Stream.of(1,2,3).forEach(c3);
```

## 1.9. Predicates Combination

```java
Predicate<String> p1 = s -> s.length() < 20;
Predicate<String> p2 = s -> s.length() > 10;

Predicate<String> p3 = p1.and(p2);
```

## 1.10. Predicate.isEqual()

```java
Predicate<String> id = Predicate.isEqual(target);
```

## 1.11. Predicates

```java
List<String> list = new ArrayList<>();
Stream<Person> stream = list.stream();
Stream<Person> filtered = stream.filter(person -> person.getAge() > 20);
```

```java
Predicate<Person> p=person->person.getAge()>20;
```

```java
Predicate<Integer> p1=i->i>20;
Predicate<Integer> p2=i->i<30;
Predicate<Integer> p3=i->i==0;


Predicate<Integer> p=p1.and(p2).or(p3); // (p1 && p2) || p3	
Predicate<Integer> p=p3.or(p1).and(p2); // p3 || (p1 && p2)	=>	(p3 OR p1) && p2
```

### Example using Predicate<Person>

First, let's create a Person class and then use the provided ```Predicate<Person>``` to filter out persons over the age of 20

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{" +
               "name='" + name + '\'' +
               ", age=" + age +
               '}';
    }
}

public class PredicatePersonExample {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("John", 22),
            new Person("Sarah", 20),
            new Person("Jane", 19),
            new Person("Greg", 32)
        );

        Predicate<Person> p = person -> person.getAge() > 20;
        List<Person> filteredPeople = people.stream()
                                            .filter(p)
                                            .collect(Collectors.toList());
        System.out.println(filteredPeople);
    }
}
```

### Example using Predicate<Integer>

Now let's apply the Predicate<Integer> to filter a list of integers based on complex logical conditions

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class PredicateIntegerExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(0, 10, 20, 25, 30, 35);

        Predicate<Integer> p1 = i -> i > 20;
        Predicate<Integer> p2 = i -> i < 30;
        Predicate<Integer> p3 = i -> i == 0;

        Predicate<Integer> p = p1.and(p2).or(p3);  // (p1 && p2) || p3
        List<Integer> filteredNumbers = numbers.stream()
                                               .filter(p)
                                               .collect(Collectors.toList());
        System.out.println("Filtered using (p1 && p2) || p3: " + filteredNumbers);

        Predicate<Integer> pAlternative = p3.or(p1).and(p2);  // p3 || (p1 && p2) => (p3 OR p1) && p2
        List<Integer> filteredNumbersAlt = numbers.stream()
                                                  .filter(pAlternative)
                                                  .collect(Collectors.toList());
        System.out.println("Filtered using p3 || (p1 && p2): " + filteredNumbersAlt);
    }
}
```

In these examples:

The PredicatePersonExample filters persons based on their age being greater than 20

The PredicateIntegerExample demonstrates how to chain and combine predicates to create more complex conditions for filtering integers, which include checking whether numbers are between 20 and 30 or equal to 0

### Another Predicate sample

```java
Predicate<String> p = Predicate.isEqual("two") ;
Stream<String> stream1 = Stream.of("one", "two", "three");
Stream<String> stream2 = stream1.filter(p.negate()) ;
```

**First: Creating a Predicate**

```java
Predicate<String> p = Predicate.isEqual("two");
```

This line creates a Predicate<String> instance named p

The Predicate.isEqual("two") method returns a predicate that tests whether an object is equal to the string "two"

In this case, the predicate will return true for any string that exactly matches "two"

**Creating a Stream**

```java
Stream<String> stream1 = Stream.of("one", "two", "three");
```

This line creates a Stream<String> containing three strings: "one", "two", and "three"

Streams are a sequence of elements supporting sequential and parallel aggregate operations

**Applying the Predicate**

```java
Stream<String> stream2 = stream1.filter(p.negate());
```

Here, the filter method is used on the stream1 object, which contains the three strings

The filter method takes a predicate to determine which elements should remain in the resulting stream

In this case, p.negate() is used, which returns a predicate that is the logical negation of the original predicate p

**p.negate() will therefore return true for any string that is not "two"**

Hence, it filters out the string "two" and keeps strings that do not match "two"

## 1.12. peek(Consumer)

In Java, the **peek()** method is often used with **streams** to perform **intermediate operations**

The **peek()** method takes a Consumer functional interface as an argument and applies it to **each element of the stream** as they are processed, without changing the elements of the stream

This method is primarily used for debugging purposes, as it allows you to view elements in the stream without altering the flow of the stream operations

Here's a simple example to demonstrate how **peek()** can be used:

**Example 1: Using peek() for Debugging**

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        long count = numbers.stream()
                .peek(num -> System.out.println("Before filtering: " + num))
                .filter(num -> num % 2 == 1)
                .peek(num -> System.out.println("After filtering: " + num))
                .count();

        System.out.println("Total odd numbers: " + count);
    }
}
```

**Explanation**:

In this example, the peek() method is used twice:

First **peek()** is used before the filter() method. It prints each number as it comes through the stream

Second **peek()** is used after the filter() method to print each number that passes the filter condition (odd numbers in this case)

The **count()** method is a terminal operation that triggers the processing of the stream

It counts the number of elements that have passed through all previous steps

**Example 2: Using peek() to Modify Elements in Mutable Objects**

```java
import java.util.stream.Stream;

public class Main {
    static class Person {
        String name;

        public Person(String name) {
            this.name = name;
        }

        void setName(String name) {
            this.name = name;
        }

        @Override
        public String toString() {
            return "Person{" + "name='" + name + '\'' + '}';
        }
    }

    public static void main(String[] args) {
        Stream.of(new Person("Alice"), new Person("Bob"), new Person("Charlie"))
                .peek(person -> person.setName(person.name.toUpperCase()))
                .forEach(System.out::println);
    }
}
```

**Explanation**:

In this example, Person objects are being processed in a stream

The **peek()** method is used to modify each Person object in the stream by converting their name property to uppercase

The **forEach()** method is a terminal operation that triggers the processing and prints out the modified Person objects

**Note**: While **peek()** is useful for debugging or operations on mutable objects, using it to modify the internal state of an element (as shown in the second example)

is generally discouraged because it can lead to code that is harder to understand and maintain. For such modifications, a map() operation might often be more appropriate and semantically clear

## 1.13. Java's Stream API operations types: intermediate or terminal operations

In Java's Stream API, operations can be classified into two types: **intermediate operations** and **terminal operations**.

**Intermediate operations** are those that transform a stream into another stream. They are used to set up a pipeline of operations, but they do not produce any results by themselves

Instead, they allow for further operations to be performed on the transformed stream

Here's why intermediate operations are essential and how the ones you mentioned function:

**Intermediate vs Terminal Operations**:

**Intermediate operations** are "**lazy**," meaning they don't actually perform any processing until a terminal operation is invoked

They are generally used **to prepare the data or set up certain conditions or transformations**.

**Terminal operations**, like forEach, cause the stream to be processed and results to be produced. They are "eager" because they immediately trigger the stream processing

**forEach(Consumer)**:

**Not Lazy**: This is a terminal operation, not an intermediate operation. It eagerly processes the elements of the stream using the provided Consumer function. Each element is taken from the stream and the Consumer action is applied immediately. Since it processes all the elements in the stream as soon as it's called, it's considered eager or not lazy

**peek(Consumer)**:

**Lazy**: This is an intermediate operation that allows you to perform a specific action on each element of the stream as they are consumed from the resulting stream. However, peek is only executed when a terminal operation is triggered on the stream. This makes it lazy, as the actual computation or action (in this case, the Consumer action in peek) doesn't occur until absolutely necessary

**filter(Predicate)**:

**Lazy**: Also an intermediate operation, filter uses a Predicate to test each element of the stream and only allows elements that pass the predicate test to be included in the resulting stream. Like peek, the filtering action isn't actually performed until a terminal operation triggers the stream processing

The purpose of having lazy intermediate operations is efficiency. This "laziness" allows Java to optimize operations by:

Reducing the number of iterations: Multiple operations can be merged and applied in a single pass

Minimizing work: Operations that might not affect the outcome (like a filter followed by a findFirst) don't need to process all elements

Short-circuiting: Operations can stop early if the terminal operation allows it (like anyMatch, findFirst, etc.)

Thus, intermediate operations provide a powerful way to build up complex chains of transformations without wasting resources, executing only when the final results are needed

## 1.14. Stream mapping

**Stream mapping** in Java is an operation performed with the **Stream API**, which was introduced in **Java 8**

It allows you to **transform each element in a stream into another form**, usually via the **map** method

This transformation is a common task when working with collections or any set of elements that you wish to modify or extract certain details from

**Using map Function**

The **map** method is used to apply a function to each element of the stream, resulting in a stream of the results of those functions

Here are the key points to understand:

**Transformation**: Converts each element into another form based on the provided function

**Function as a parameter**: The function can be a lambda expression, a method reference, or an instance of a functional interface

**Type Change**: The resultant stream does not necessarily have to be of the same type as the input stream

Here are some practical examples to illustrate these concepts:

**Example 1: Converting a list of names to uppercase**

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        List<String> upperNames = names.stream()
                                       .map(String::toUpperCase)
                                       .collect(Collectors.toList());
        System.out.println(upperNames);
    }
}
```

This example creates a list of names, converts each name to uppercase using the map method with a method reference String::toUpperCase, and collects the results back into a list

**Example 2: Converting objects**

Suppose we have a list of User objects and we want to extract their email addresses

```java
class User {
    String name;
    String email;

    User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public String getEmail() {
        return email;
    }
}

public class StreamExample {
    public static void main(String[] args) {
        List<User> users = Arrays.asList(
            new User("Alice", "alice@example.com"),
            new User("Bob", "bob@example.com")
        );

        List<String> emails = users.stream()
                                   .map(User::getEmail)
                                   .collect(Collectors.toList());
        System.out.println(emails);
    }
}
```

In this case, the **map** operation extracts the email from each User object by using the getEmail method reference

**Summary**

Stream mapping is a powerful tool in Java for data transformation

By using the map function, you can transform a stream of items into another form efficiently, enabling cleaner and more maintainable code for operations that involve data manipulation

## 1.15. Mapping operations

The **Function<T, R>** interface is a **built-in interface** in **Java**, which is part of the **java.util.function** package

This package includes several functional interfaces that are intended for use with **Java 8's** lambda expressions and method references

The Function interface is designed to represent a function that accepts one argument and produces a result

The methods you mentioned—apply, compose, andThen, and identity—are all part of its standard definition in Java. Here’s a brief rundown of these methods:

**apply(T t)**: This is the primary abstract method of the interface, taking an input of type T and returning a result of type R

**compose(Function<V, T> before)**: This default method allows for function composition, where the function passed as an argument is applied first, and then the function on which compose is called is applied to the result

**andThen(Function<R, V> after)**: This default method works opposite to compose. The function on which andThen is called is applied first, and then the function passed as an argument is applied

**identity()**: This static method returns a function that always returns its input argument

It is useful when a functional transformation needs to effectively do nothing to the passed argument

These methods make Function a versatile interface for creating and chaining together transformations on data in a functional style, which can be particularly powerful when working with streams or other API that leverage functional programming concepts

**Some samples**

**apply Method Example**

The **apply** method is the **primary abstract method of the Function interface**

It takes one argument and returns a result

```java
Function<Integer, Double> squareRoot = x -> Math.sqrt(x);
Double result = squareRoot.apply(16);  // result will be 4.0
System.out.println(result);
```

**compose Method Example**

The **compose** method allows you to **combine functions**

It returns a new function that first applies the given function and then applies the current function to the result

```java
import java.util.function.Function;

public class FunctionCompositionApp {
    public static void main(String[] args) {
        // Function to multiply its input by 2
        Function<Integer, Integer> multiplyBy2 = x -> x * 2;

        // Function to subtract 3 from its input
        Function<Integer, Integer> subtract3 = x -> x - 3;

        // Compose the functions: first subtract 3, then multiply by 2
        Function<Integer, Integer> combinedFunction = multiplyBy2.compose(subtract3);

        // Apply the composed function to the input 10
        Integer resultCompose = combinedFunction.apply(10); // (10 - 3) * 2 = 14

        // Print the result
        System.out.println(resultCompose);
    }
}
```

**andThen Method Example**

The **andThen** method **chains the current function to another one**

The current function is applied first, followed by the specified function

```java
import java.util.function.Function;

public class FunctionChainingApp {
    public static void main(String[] args) {
        // Function to add 5 to its input
        Function<Integer, Integer> add5 = x -> x + 5;

        // Function to multiply its input by 3
        Function<Integer, Integer> multiplyBy3 = x -> x * 3;

        // Combine the two functions: first add 5, then multiply by 3
        Function<Integer, Integer> combinedAndThen = add5.andThen(multiplyBy3);

        // Apply the combined function to the input 4
        Integer resultAndThen = combinedAndThen.apply(4);  // (4 + 5) * 3 = 27

        // Print the result
        System.out.println(resultAndThen);
    }
}
```

**identity Method Example**

The **identity** function returns a function that **returns its input argument**

```java
import java.util.function.Function;     // Import the Function interface
import java.util.function.UnaryOperator; // Import the UnaryOperator interface

class HelloWorld {
    public static void main(String[] args) {
        // Using Function.identity() to create an identity function
        Function<String, String> identityFunction = Function.identity();
        // Applying the identity function to a string
        String resultIdentity = identityFunction.apply("Hello World");
        // Outputting the result
        System.out.println(resultIdentity);  // Outputs "Hello World"

        // Example usage of the print method with a UnaryOperator
        print(10, x -> x + 5);  // This will output 15
        print(10, UnaryOperator.identity());  // This will output 10
    }

    // A helper method to print the result of applying the UnaryOperator to a given value
    private static void print(int value, UnaryOperator<Integer> operator) {
        System.out.println(operator.apply(value));
    }
}
```

**Another sample**

```java
// Online Java Compiler
// Use this editor to write, compile and run your Java code online
import java.util.function.UnaryOperator;

class HelloWorld {
    public static void main(String[] args) {
        UnaryOperator<Integer> f1 = a -> a + 1;
        UnaryOperator<Integer> f2 = a -> a * 2;
        int x = 1;

        System.out.println(f1.andThen(f2).apply(x));  // f2(f1(x)) = (1+1)*2 = 4
        System.out.println(f1.compose(f2).apply(x));  // f1(f2(x)) = 2*1 + 1 = 3

        UnaryOperator<Integer> f3 = UnaryOperator.identity();
        print(10, f1);
        print(10, UnaryOperator.identity());
        print(10, z -> z);
    }

    // A helper method to print the result of applying the UnaryOperator to a given value
    private static void print(int value, UnaryOperator<Integer> operator) {
        System.out.println(operator.apply(value));
    }
}
```

This is the output:

```
4
3
11
10
10
```

## 1.16. map and flatmap methods from the Stream interface in Java

**Using map Method**

The **map** method is used to **apply a function to each element of a stream** and returns a stream consisting of the results

Below is a simple Java application that demonstrates using map to square each number in a list of integers:

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;
import java.util.function.Function;

public class MapExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        // Using map to square each element in the list
        List<Integer> squaredNumbers = numbers.stream()
                                              .map(x -> x * x)
                                              .collect(Collectors.toList());

        // Print the result
        System.out.println(squaredNumbers);
    }
}
```

This is the output:

```
[1, 4, 9, 16, 25]
```

**Using flatMap Method**

The **flatMap** method is used to flatten streams resulting from mapping each element of the original stream into zero or more elements

Here's a Java application that demonstrates using flatMap to flatten a list of lists of integers:

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;
import java.util.function.Function;

public class FlatMapExample {
    public static void main(String[] args) {
        List<List<Integer>> listOfLists = Arrays.asList(
            Arrays.asList(1, 2),
            Arrays.asList(3, 4, 5),
            Arrays.asList(6, 7, 8, 9)
        );

        // Using flatMap to flatten a list of lists into a single list
        List<Integer> flatList = listOfLists.stream()
                                            .flatMap(List::stream)
                                            .collect(Collectors.toList());

        // Print the result
        System.out.println(flatList);
    }
}
```

This is the output:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

**Explanation**

**map Method**: It takes a function ```Function<T, R>``` that **transforms each element T of the stream into an element R**. In the example, we square each integer

**flatMap Method**: It takes a function ```Function<T, Stream<R>>``` that **transforms each element T of the stream into a Stream<R>**. This method is very useful for flattening nested structures like lists of lists

## 1.17. Aggregation and Reduction

In Java, **aggregation** and **reduction** are powerful operations typically performed on data structures like arrays, collections, and especially streams

These operations allow you to **summarize or reduce a set of values to a single value**

Here are some examples demonstrating these concepts using Java's Stream API

**Example 1: Summing a List of Integers (Reduction)**

This example demonstrates how to use the reduce method to calculate the sum of a list of integers

The reduce method is a general-purpose reduction operation

```java
import java.util.Arrays;
import java.util.List;

public class ReductionExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        // Using reduce to sum all numbers
        int sum = numbers.stream()
                         .reduce(0, (a, b) -> a + b);

        // Print the result
        System.out.println("Sum: " + sum);
    }
}
```

**Example 2: Finding Maximum in a List of Integers (Reduction)**

This example uses the reduce method to find the maximum value in a list of integers

```java
import java.util.Arrays;
import java.util.List;

public class FindMaxExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 3, 10, 40, 2);

        // Using reduce to find the maximum element
        int max = numbers.stream()
                         .reduce(Integer.MIN_VALUE, Integer::max);

        // Print the result
        System.out.println("Maximum: " + max);
    }
}
```

**Example 3: Averaging Numbers (Aggregation)**

This example uses the IntStream to calculate the average of a list of integers

```java
import java.util.Arrays;
import java.util.IntSummaryStatistics;
import java.util.List;

public class AveragingExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 10, 15, 20, 25);

        // Calculating average using IntStream
        double average = numbers.stream()
                                .mapToInt(Integer::intValue)
                                .average()
                                .orElse(0.0);

        // Print the result
        System.out.println("Average: " + average);
    }
}
```

**Example 4: Grouping and Summing (Aggregation)***

This example demonstrates grouping a list of items by a classification function and then summing up one of their attributes

```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class GroupingAndSummingExample {
    static class Product {
        String category;
        int price;

        Product(String category, int price) {
            this.category = category;
            this.price = price;
        }
    }

    public static void main(String[] args) {
        List<Product> products = Arrays.asList(
            new Product("electronics", 100),
            new Product("electronics", 150),
            new Product("clothing", 80),
            new Product("clothing", 120)
        );

        // Grouping products by category and summing their prices
        Map<String, Integer> categorySum = products.stream()
                                                   .collect(Collectors.groupingBy(p -> p.category,
                                                       Collectors.summingInt(p -> p.price)));

        // Print the results
        System.out.println("Total prices by category: " + categorySum);
    }
}
```

## 1.18. Optional class in Java

Java's Optional class is a container object used to contain not-null objects

It is especially useful for handling cases where a value may be absent without resorting to checking for null

I'll provide you with several Java application samples demonstrating various ways to use Optional along with the isPresent method and other common patterns

**Example 1: Basic Use of Optional**

This example demonstrates the basic use of Optional to handle the presence or absence of a string value.

```java
import java.util.Optional;

public class OptionalBasicExample {
    public static void main(String[] args) {
        Optional<String> opt = Optional.of("Hello World");

        if (opt.isPresent()) {
            String s = opt.get();
            System.out.println("Value is present: " + s);
        } else {
            System.out.println("Value is absent");
        }

        // Example of an empty Optional
        Optional<String> emptyOpt = Optional.empty();
        if (emptyOpt.isPresent()) {
            System.out.println("This won't print");
        } else {
            System.out.println("Value is absent");
        }
    }
}
```

**Example 2: Using Optional with Default Value**

This example shows how to use Optional with a default value if the original Optional is empty. This approach is often cleaner than using isPresent() and get()

```java
import java.util.Optional;

public class OptionalWithDefault {
    public static void main(String[] args) {
        Optional<String> opt = Optional.empty();

        // Using orElse to provide a default value if the Optional is empty
        String s = opt.orElse("Default Value");
        System.out.println(s);  // Output: Default Value
    }
}
```

**Example 3: Using Optional in Conditional Operations**

This example demonstrates how Optional can be integrated into a more functional style of programming, reducing the need for explicit conditional checks

```java
import java.util.Optional;

public class OptionalFunctionalStyle {
    public static void main(String[] args) {
        Optional<String> opt = Optional.ofNullable(null);

        // Using ifPresent to perform an action if the value is present
        opt.ifPresent(s -> System.out.println("Value: " + s));

        // Using orElseGet to compute a default value via a supplier function
        String result = opt.orElseGet(() -> "Computed Default");
        System.out.println(result);  // Output: Computed Default
    }
}
```

**Example 4: Combining Optional with Stream API**

This example shows how Optional can be used with Java's Stream API to handle potential null values gracefully in a data processing pipeline

```java
import java.util.Optional;
import java.util.stream.Stream;

public class OptionalWithStream {
    public static void main(String[] args) {
        Optional<String> opt = Optional.of("Filtered Value");

        // Using Optional with Stream for conditional processing
        Stream<String> stream = opt.map(Stream::of).orElseGet(Stream::empty);
        stream.filter(s -> s.contains("Value"))
              .forEach(System.out::println);  // Output: Filtered Value
    }
}
```

## 1.19. Collectors

Below are some Java code samples demonstrating the use of Collectors, which is a utility class provided in the Java Stream API to facilitate various **mutable reduction operations on stream elements**

These operations include grouping elements, summarizing elements, and more. Here are a few examples:

**Example 1: Collecting a List**

This example demonstrates how to **collect Stream elements into a list**

```java
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Stream.of(1, 2, 3, 4, 5)
                                      .collect(Collectors.toList());
        System.out.println(numbers);
    }
}
```

**Example 2: Grouping By**

This example shows how to **group elements of a stream by a classifier function**

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<String> words = List.of("apple", "bat", "apricot", "banana", "cherry");
        Map<Character, List<String>> groupedByFirstLetter = words.stream()
            .collect(Collectors.groupingBy(word -> word.charAt(0)));
        System.out.println(groupedByFirstLetter);
    }
}
```

**Example 3: Counting**

This example illustrates how to **count the number of elements in each group after grouping**

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<String> words = List.of("apple", "bat", "apricot", "banana", "cherry");
        Map<Character, Long> countByFirstLetter = words.stream()
            .collect(Collectors.groupingBy(word -> word.charAt(0), Collectors.counting()));
        System.out.println(countByFirstLetter);
    }
}
```

**Example 4: Joining**

This example shows how to **concatenate elements of a stream into a single string**

```java
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Main {
    public static void main(String[] args) {
        String sentence = Stream.of("Java", "is", "cool")
                                .collect(Collectors.joining(" "));
        System.out.println(sentence);
    }
}
```

**Example 5: Summarizing**

This example demonstrates how to get summary statistics from a collection of numbers

```java
import java.util.DoubleSummaryStatistics;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Main {
    public static void main(String[] args) {
        DoubleSummaryStatistics stats = Stream.of(1.5, 2.3, 3.7)
                                              .collect(Collectors.summarizingDouble(Double::doubleValue));
        System.out.println(stats);
    }
}
```

These examples cover a variety of uses of the Collectors class, providing a strong foundation for understanding how to manipulate collections using Java streams

**Example 6: Collecting in a List**

```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

// Define the Person class with age and lastName properties
class Person {
    private int age;
    private String lastName;

    public Person(int age, String lastName) {
        this.age = age;
        this.lastName = lastName;
    }

    public int getAge() {
        return age;
    }

    public String getLastName() {
        return lastName;
    }
}

public class Main {
    public static void main(String[] args) {
        // Create a list of persons
        List<Person> persons = new ArrayList<>();
        persons.add(new Person(25, "Smith"));
        persons.add(new Person(19, "Johnson"));
        persons.add(new Person(30, "Williams"));
        persons.add(new Person(18, "Jones"));
        persons.add(new Person(22, "Brown"));

        // Filter persons older than 20 and collect their last names into a list
        List<String> result = persons.stream()
                                     .filter(person -> person.getAge() > 20)
                                     .map(Person::getLastName)
                                     .collect(Collectors.toList());

        // Print the result
        System.out.println(result);
    }
}
```

**Example 7: Collecting in a String**

```java

```

**Example 8: Collecting in a Map**

```java

```

## 1.20. 
