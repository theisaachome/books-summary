# Chapter-05 : Operators

- [Operators](#Operators)
- [The UnaryOperator Interface](#the-unaryoperator-interface)
- [Specializations of UnaryOperator](#Specializations-of-UnaryOperator)
- [Chains Involving UnaryOperator Specializations](#Chains-Involving-UnaryOperator-Specializations)
- [The BinaryOperator Interface](#The-BinaryOperator-Interface)
- [Non-generic Specializations of BinaryOperator](#Non-generic-Specializations-of-BinaryOperator)

---

## Operators

- When a Function object’s
- input type is the same as its output type,
- an Operator interface can be used in place of a Function.

---

## The UnaryOperator Interface

- a functional interface with
- one type parameter UnaryOperator<T>
- inherits Function<T, T>.

```java
@FunctionalInterface
public interface UnaryOperator<T>
extends Function<T,T> {
    static <T> UnaryOperator<T> identity();
}

```

### example

```java
  UnaryOperator<String> concat =x->x +x;
  UnaryOperator<Integer> increment = x -> ++x;
  UnaryOperator<Long> decrement = x-> --x;

  System.out.println(concat.apply("Isaac"));
  System.out.println(increment.apply(10));
  System.out.println(decrement.apply(10L));
```

---

### Specializations of UnaryOperator

- To perform a single operation on an int, long, or double primitive argument, respectively.

- These interfaces include IntUnaryOperator, DoubleUnaryOperator, and LongUnaryOperator.

```java
@FunctionalInterface
public interface IntUnaryOperator {
   int applyAsInt(int operand);
     ...
}
@FunctionalInterface
public interface LongUnaryOperator {
   long applyAsLong(long operand);
     ...
}
@FunctionalInterface
public interface DoubleUnaryOperator {
   double applyAsDouble(double operand);
     ...
}
```

##### example

```java
IntUnaryOperator    iuo = x -> x + 5;
LongUnaryOperator   luo = x -> x / 3L;
DoubleUnaryOperator duo = x -> x * 2.1;
System.out.println(iuo.applyAsInt(4));
System.out.println(luo.applyAsLong(9L));
System.out.println(duo.applyAsDouble(4.1));
```

---

### Chains Involving UnaryOperator Specializations

- The UnaryOperator specializations provide andThen, compose, and identity methods that use UnaryOperator specializations as arguments and return types.

```java
System.out.println(iuo.andThen(x -> x * 2)
                      .applyAsInt(4));
System.out.println(luo.compose(x -> x * 6)
                      .applyAsLong(4));
System.out.println(duo.andThen(DoubleUnaryOperator.identity())
                      .applyAsDouble(4.1));
```

---

### The BinaryOperator Interface

- BinaryOperator is a functional interface with one type parameter such that BinaryOperator<T> inherits BiFunction<T, T, T>.

```java
@FunctionalInterface
public interface BinaryOperator<T>extends BiFunction<T,T,T> {
   // some static methods
  ...
}
```

example

```java
BinaryOperator<String>  concat   = (x,y) -> x + y;
BinaryOperator<Integer> subtract = (x,y) -> x - y;
BinaryOperator<Long>    multiply = (x,y) -> x * y;
System.out.println(concat.apply("AB", "CD"));
System.out.println(subtract.apply(4, 1));
System.out.println(multiply.apply(4L, 3L));
```

---

### Non-generic Specializations of BinaryOperator

- The Java API provides

- non-generic specializations of the BinaryOperator interface which

- perform a single operation on two int, long, and double primitive arguments, respectively.

- These interfaces include
  - IntBinaryOperator,
  - DoubleBinaryOperator, and
  - LongBinaryOperator.

#### Example

```java
@FunctionalInterface
public interface IntBinaryOperator {
   int applyAsInt(int left,int right);
}
@FunctionalInterface
public interface LongBinaryOperator {
   long applyAsLong(long left, long right);
}
@FunctionalInterface
public interface DoubleBinaryOperator {
   double applyAsDouble(double left, double right);
}
```

```java
IntBinaryOperator    ibo = (x,y) -> x + y + 5;
LongBinaryOperator   lbo = (x,y) -> (x + y)/ 3L;
DoubleBinaryOperator dbo = (x,y) -> x * y * 0.5;
System.out.println(ibo.applyAsInt(4,2));
System.out.println(lbo.applyAsLong(9L,3L));
System.out.println(dbo.applyAsDouble(4.0,6.0));
```
