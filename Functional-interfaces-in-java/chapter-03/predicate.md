## Predicates

[Table of Contents](#table-of-contents)

- [The Predicate Interface](#The-Predicate-Interface)
- [Passing a Predicate to a Method](#Passing-a-Predicate-to-a-Method)

---

### The Predicate Interface

- An functional interface with test method,

- evaluates a condition on an input variable of a generic type.

- returns true if the condition is true, and false otherwise.

#### example

```java
Predicate<Integer> p1 = x -> x >7;
		if(p1.test(9)) {
			System.out.println("Predicate x > 7 is true for x==9");
		}else {
			System.out.println("Predicate x > is false for z ==9");
}
```

---

### Passing a Predicate to a Method

- When a predicate is passed to a method and the Predicate object’s test method is called inside the method,

- whatever condition that was associated with the predicate will be executed.

#### example

- accept Predicate<Integer> objects.

```java
public static void result(Predicate<Integer> p, Integer arg) {
	if(p.test(arg)) {
		System.out.println("The Predicate is true for " + arg);
	}else {
		System.out.println("The Predicate is false for " + arg);
	}
}
```

-- calling the methods

```java
	Predicate<Integer> p = x ->x == 5;
	result(p, 5);
	result(y -> y%2==0,5);
```

- Generic by defining a type parameter

```java
public static <T> void result(Predicate<T> p,T arg) {
	if(p.test(arg)) {
		System.out.println("The Predicate is ture for " + arg);
	}else {
		System.out.println("The Predicate is false for" + arg);
	}
}
```

- Using generic predicate

```java
    Predicate<Integer> p = x->x>2;
	result(p, 5);
	Predicate<String> p1= s->s.charAt(0)=='H';
	result(p1, "Hello");
```

---

### Chains of Functional Interfaces

- Many of the functional interfaces in the java.util.function package
- have default methods that return new functional interface objects,

- whose methods can, in turn, be called down the method chain.

- Using this technique, long chains of functional interfaces can be used to perform series of calculations and to inline the logic of your program

### The OR Operation

```js
default Predicate<T> or(Predicate<? super T> other);
```

- to evaluate the logical expression x > 7 || x < 3.
- OR operation on two predicates,
- one with condition x > 7 and
- another with condition x < 3.

#### example

```java
public class OROperationDemo {
	public static void main(String[] args) {
		Predicate<Integer> p = x-> x >7;
		var result = p.or(x -> x<10).test(9);
		System.out.println(result);
	}
}
```

- the first call to result is short-circuited because condition 9 > 7 is true.
- The predicates in the second and third calls are not short-circuited, and condition 9 < 3 is evaluated.

### The AND Operation

```java
default Predicate<T> and(Predicate<? super T> other);
```

- to evaluate the logical expression x > 7 && x%2 == 1.
- The default And method accomplishes
- creating a composed predicate
- the logical AND of the existing predicate’s condition with the condition of its argume.

#### example

```java
public class ANDOperationDemo {
	public static void main(String[] args) {
		Predicate<Integer>  p1 = x->x >7;
		var result = p1.and(x->x%2==0).test(10);

		System.out.println(result);
	}
}
```

- both conditions are true.

---

### The Negate Operation

```js
default Predicate<T> negate();
```

- default negate method reverses the result of the current predicate.

#### example

```java
Predicate<Integer> p = x-> x>7;
var result = p.negate().test(10);
System.out.println(result);
```

- In a chain of predicates, the position of negate matters.

- result : return true

```java
var orderResult = p.and(x->x%2==1)
	.negate().test(8);
```

- result : return false

```java
var secondResult = p.negate()
		.and(x->x%2==1)
		.test(9);
```

---

### Predicate.isEqual

- the equals method of the Predicate object’s type parameter to check if the argument to the test method is equal to a value.
