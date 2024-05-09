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



