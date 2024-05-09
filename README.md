# Java Advanced: Functional, Asynchronous and Reactive Programming

Summary:

- Functional Java: functional interfaces, streams

- Executor framework and Fork Join pool

- NIO - non-blocking input/output

- Asynchronous Java (Completable Future)

- Reactive Streams (Java 9)

- RxJava 2

- R2DBC (reactive JDBC replacement)

- Spring WebFlux (Reactor)

- Reactive Spring Data JPA

## 1. Functional Interfaces and Method References

### 1.1. A Lambda Expression

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

### 1.2. Several Ways of Writing a Lambda Expression

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

## 1.3. What Is the Type of a Lambda Expression?

What is a functional interface?

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
