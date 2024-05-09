# Java Advanced: Functional, Asynchronous and Reactive Programming

Summary:

- Functional Java: lambda expression, functional interfaces, streams

- Executor framework and Fork Join pool

- NIO - non-blocking input/output

- Asynchronous Java (Completable Future)

- Reactive Streams (Java 9)

- RxJava 2

- R2DBC (reactive JDBC replacement)

- Spring WebFlux (Reactor)

- Reactive Spring Data JPA

## 1. Lambda Expression, Functional Interfaces and Method References

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
