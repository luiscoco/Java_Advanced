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

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/e764e1ec-c897-4489-82b7-79a73e34c303)

## Package java.util.function

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/08a6f283-b861-49c1-bf5f-67bee8e98f62)

## Omitting Parameter

![image](https://github.com/luiscoco/Java_Advanced/assets/32194879/eb42d68c-d366-479b-a1d5-260a4ccac7e8)

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









