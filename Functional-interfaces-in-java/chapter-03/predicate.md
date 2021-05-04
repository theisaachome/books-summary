## Predicates

[Table of Contents](#table-of-contents)

- [The Predicate Interface](#The-Predicate-Interface)
- [Passing a Predicate to a Method](#Passing-a-Predicate-to-a-Method)
- [Chains of Functional Interfaces](#Chains-of-Functional-Interfaces)
  - [The OR Operation](#The-OR-Operation)
  - [The AND Operation](#The-AND-Operation)
  - [The Negate Operation](#The-Negate-Operation)
  - [Predicate.isEqual](#Predicate-isEqual)
  - [Using Predicate.not](#Using-Predicate.not)
- [Overriding Predicate Default Methods](#Overriding-Predicate-Default-Methods)
- [Specializations of Predicates](#Specializations-of-Predicates)
- [Binary Predicates](#Binary-Predicates)

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

### **[⬆ back to top](#table-of-contents)**

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

### Predicate isEqual

- the equals method of the Predicate object’s type parameter to check if the argument to the test method is equal to a value.

```java
static <T> Predicate<T> isEqual(Object targetRef);
```

#### example

```java
Predicate<Integer> p2= Predicate.isEqual(3);
		if(p2.test(3)) {
			System.out.println("Predicate is Equal");
}
```

- a composed predicate the logical OR

```java
Predicate<Integer> p1= x->x>7;
var result = p1.or(Predicate.isEqual(3)).test(3);
System.out.println(result);
```

---

### Using Predicate.not

```java
static <T> Predicate<T> not(Predicate<T> target); [JAVA 11]s
```

- Predicate.not method

  - Reverses the result of the calculation performed by its predicate argument.

- While the negate method changes the result of an existing predicate,

- the not method changes the result of a predicate argument.

#### example

```java
Predicate<Integer> p1 = x->x >7;
var result = p1.and(Predicate.not(x->x%2==1)).test(8);
System.out.println(result);
```

---

### Overriding Predicate Default Methods

- Normal Use case

#### example

- check if a string starts with the character “a” and
- is greater than four characters in length.

```java
Predicate<String> lengthGr4 = x->x.length()> 4;
Predicate<String> char0isA= x -> x.charAt(0) == 'a';
var result = lengthGr4.and(char0isA).test("alpha");
System.out.println(result);
```

- will print error if String is null

```java
 var resultError = lengthGr4.and(char0isA)
                                .test(null);
  System.out.println(resultError);
```

#### To Null Check overrides the methods

```java
Predicate<String> nullProtectedLengthGr4 = new Predicate<String>() {
	@Override
	public boolean test(String x) {
		return x.length() > 4;
	}
	@Override
	public Predicate<String> and(Predicate<? super String> p){
		return x->x == null? false:test(x) && p.test(x);
	}
};

System.out.println(nullProtectedLengthGr4.and(char0isA).test("alpha"));
System.out.println(nullProtectedLengthGr4.and(char0isA).test(null));
```

---

### Specializations of Predicates

- Java API provides
- the non-generic
  - IntPredicate,
  - LongPredicate, and
  - DoublePredicate interfaces
  - which can be used to test Integers, Longs, and Doubles, respectively.

```java

@FunctionalInterface
public interface IntPredicate{
    boolean test(int value);
}

@FunctionalInterface
public interface LongPredicate{
    boolean test(long value);
}

@FunctionalInterface
public interface DoublePredicate{
    boolean test(double value);
}

```

#### examples

```java
IntPredicate i = x -> x > 5;
LongPredicate l = y -> y%2 == 0;
DoublePredicate d = z -> z > 8.0;
System.out.println(i.test(2));
System.out.println(l.or(a -> a == 6L).test(10L));
System.out.println(d.and(b -> b < 9.0).test(8.5));
```

---

### Binary Predicates

- To create a single predicate of two different types.

- The BiPredicate interface specifies two type parameters.

```java
@FunctionalInterface
public interface BiPredicate<T, U>{
    boolean test(T t, U u);
}
```

#### example

- With type parameters String and Integer

  - Tests if a string equals “Manager” and

  - An integer is greater than 100000.

```java
BiPredicate<String, Integer> bi =
		(x,y)->x.equals("Manager") && y > 1000;
String position = "Manager";
int salary = 1500;
var result = bi.test(position, salary);
System.out.println(result);
```
