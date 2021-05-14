## **Use In Optionals**

- [Table of Contents](#table-of-contents)

- [Use In Optionals](#Use-In-Optionals)

- [Creating an Optional](#Creating-an-Optional)

- [Determining If an Optional Is Present](#Determining-If-an-Optional-Is-Present)

- [Retrieving the Contents of an Optional](#Retrieving-the-Contents-of-an-Optional)

- [Creating Chains of Optionals](#Creating-Chains-of-Optionals)

- [Printing the Contents of an Optional](#Printing-the-Contents-of-an-Optional)

- [Filtering Optionals](#Filtering-Optionals)

- [Optional Chains Involving map and flatmap](#Optional-Chains-Involving-map-and-flatmap)

---

### **Use In Optionals**

- It is useful to wrap an object inside an Optional to avoid checking for nullness.

- Optionals can also be used inside method chains to simplify the logic of a program.

---

### **Creating an Optional**

- The Optional class wraps an object of type parameter T.

```java
public final class Optional <T> {}
```

- Optional Class Static methods which are used to create
  Optionals of type T:

```java
  static <T> Optional<T> of(T value);
```

```java
  static <T> Optional<T> ofNullable(T value);
```

```java
  static <T> Optional<T> empty();
```

#### **The of method**

```java
public class OptionalCreationDemo {
	public static void main(String[] args) {
		try {
			Optional<String> op1 = Optional.of(null);

		} catch (NullPointerException e) {
			System.out.println("NullPonterException.");
		}
		Optional<String> op2 = Optional.of("Hello World");
		System.out.println(op2);
	}
}
```

---

#### **The ofNullable method**

- If the object wrapped by an Optional can be null, the ofNullable method should be used.

```java
public class NullableMethodDemo {
	public static void main(String[] args) {
		Optional<String> op= Optional.ofNullable(null);
		System.out.println("OK");
		Optional<String> op2 = Optional.ofNullable("Hello World");
		System.out.println(op2.get());
	}
}
```

---

#### **The empty method**

- The empty method can also be used to create an Optional that wraps a null object.

```java
public class TheEmptyMethodDemo {
	public static void main(String[] args) {
		Optional<String> op= Optional.empty();
		System.out.println(op);
	}
}
```

---

### **Determining If an Optional Is Present**

#### **The isPresent method**

- returns true if the Optional contains a non-null object and
- returns false otherwise.

```java
boolean isPresent();
```

#### example

```java
public class IsPresentDemo {
	public static void main(String[] args) {
		Optional<String> op = Optional.empty();
		if (op.isPresent()) {
			System.out.println("Optional is non-null");
		} else {
			System.out.println("Optional is null");
		}

	}
}
```

---

#### the isEmpty method

```java
bool isEmpty();
```

- The isEmpty method was added.
- This method returns true if the Optional wraps a null object and false otherwise.
- This method gives the opposite result of isPresent.

#### Example

```java
public class IsEmptyDemo {
	public static void main(String[] args) {
		Optional<String> op= Optional.ofNullable(null);
		if(op.isEmpty()) {
			System.out.println("Optional is Empty");
		}else {
			System.out.println("Optional is Not Empty");
		}
	}
}
```

---

### **Retrieving the Contents of an Optional**

- The get method returns the object wrapped by an Optional.

```java
T get();
```

- if called on an Optional that contains null.
- throws a NoSuchElementException

#### Example

```java
public class GetMethodDemo {
	public static void main(String[] args) {
		Optional<String> op1 = Optional.of("I am Optional Value");
		Optional<String> op2 =Optional.ofNullable(null);

		try {
			op2.get();
		} catch (NoSuchElementException e) {
			System.out.println("NoSuchElementException.");
		}
		System.out.println(op1.get());
	}
}

```

---

### **Creating Chains of Optionals**

- The Optional class provides the following methods that can be used to create chains of
  Optionals:

```java
T orElse(T other);
T orElseGet(Supplier<? extends T> supplier);
Optional<T> or(Supplier<? extends Optional<? extends T> > supplier); <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier);
T orElseThrow();
```

#### Example

- Pick the first String that is non-null.

```java
// normal logic
	String t =null;
	String u = "Hello";

	String s= t;
	if(t==null) {
		s=u;
	}
```

#### **The orElse method**

- The Optional class’s orElse method provides a value to use if the Optional contains a null.

```java
public class OptionalChainDemo {
	public static void main(String[] args) {
		String t =null;
		String u = "Hello";
		String s= Optional.ofNullable(t).orElse(u);
		System.out.println(s);
	}
}
```

---

### **The orElseGet method**

- The orElseGet method accepts a supplier which provides the value.

```java
public class OrElseGetMethodDemo {
	public static void main(String[] args) {
		String t = null;
		String u = "Hello World";
		String s=Optional.ofNullable(t).orElseGet(()->u);
		System.out.println(s);

	}
}
```

---

#### **The orElseThrow method**

- The orElseThrow method accepts a supplier which provides an exception which is thrown if the value is not present.

```java
public class OrElseThrowDemo {
	public static void main(String[] args) {
		String s = null;
		try {
			String s2=Optional.
					ofNullable(s)
					.orElseThrow(()-> new Exception("Null Optional"));
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
}
```

---

#### An overloaded version of the orElseThrow method

- the orElseThrow method throws a NoSuchElementException.
- The exception is caught and “No value present” is displayed.

```java
public class OrElseThrowDemo {
	public static void main(String[] args) {
		String s = null;
		try {
			String result = Optional.ofNullable(s)
					.orElseThrow();
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
}
```

---

#### **Note:**

- The orElse, orElseGet, and orElseThrow methods return a value of type X.

---

#### **The or method**

- The or method returns an Optional<X>

- that can be followed by additional links in an Optional chain

```java
public class OrMethodDemo {
	public static void main(String[] args) {
		Supplier<Optional<String>> supplier = ()->{
			System.out.print("Enter a String: ");
			return Optional.of(new Scanner(System.in).nextLine());
		};

		String s = null;
		Optional<String> os = Optional.ofNullable(s).or(supplier);

		if(os.isPresent()) {
			System.out.println(os.get());
		}
	}
}
```

---

### **Printing the Contents of an Optional**

- The Optional class’s ifPresent method accepts a consumer.

- Used To print the object that is wrapped by an Optional.

```java
void ifPresent(Consumer<? super T> action);
```

- Since ifPresent returns void,

- it must be the last method called if used in an Optional chain.

```java
public class IfPresentDemo {
	public static void main(String[] args) {
		Supplier<Optional<String>> supplier = ()->{
			System.out.print("Enter a String: ");
			return Optional.of(new Scanner(System.in).nextLine());
		};
		String s = null;
		Optional.ofNullable(s).or(supplier).ifPresent(System.out::print);
	}
}
```

---

### **Filtering Optionals**

- The Optional class’s filter method accepts a predicate and returns an Optional.

- So it can be used anywhere within an Optional chain.

```java
 Optional<T> filter(Predicate<? super T> predicate);
```

- if the predicate is true returns the Optional .

- else returns an empty Optional .

- only called on non-null Optionals.

#### Example

```java
public class FilterMethodDemo {
	public static void main(String[] args) {
			String name = "Isaachome";
			Optional.ofNullable(name)
			.filter(n->n.length()> 6)
			.ifPresent((n)->System.out.println(n));
	}
}
```

- build chains of Optionals which result in the logical AND of their predicates.

```java
public class FilterMethodDemo {
	public static void main(String[] args) {
        	String name = "Isaachome";
			Optional.of(name)
			.filter(x->x.charAt(0)=='I')
			.filter(x->x.length()>2)
			.filter(x->x.charAt(1)=='s')
			.ifPresent(System.out::println);
	}
}

```

- the first predicate is false, so the second and third predicates do not execute and no output is produced.

```java
Optional.of("IsaacHome")    // Optional(IsaacHome)
    .filter(x -> x.charAt(0) == 'i')    // Optional(null)
    .filter(x -> x.length() > 2)    // doesn't execute
    .filter(x -> x.charAt(1) == 'e')    // doesn't execute
    .ifPresent(x -> System.out.println(x));
```

---

#### **Using the Predicate class’s or method**

- Filter conditions can be logically OR’ed using the Predicate class’s or method along a predicate chain.

- filters the Optional using the logical OR of predicates which check if the first character is either “i” or “H.”

```java
public class FilterMethodDemo {
	public static void main(String[] args) {
			Predicate<String> p = x -> x.charAt(0)=='i';
			Optional.of("Isaachome")
					.filter(p.or(x -> x.charAt(0)=='I'))
					.ifPresent(System.out::print);
	}
}

```

---

### **Optional Chains Involving map and flatmap**

- The Optional class’s map method accepts a function that converts a non-null Optional<X> to an Optional<Y>.

```java
<U> Optional<U> map(Function<? super T, ? extends U> mapper);
```

#### **example**

- converts an Optional<String> to an Optional<Integer> and
- then applies integer arithmetic operations in the predicates that follow.

```java
public class MapDemo {
	public static void main(String[] args) {
		Optional.of("100")
			.map((data) -> Integer.parseInt(data))
			.filter(x -> x >50)
			.filter(x -> x%2 == 0)
			.ifPresent(System.out::print);
	}
}

```

- Since the ifPresent method returns void, any modifications to the underlying object will not persist.

```java
public class MapDemo {
	public static void main(String[] args) {
			Optional<Integer> op = Optional.of(2);
			op.ifPresent(x -> ++x);
			op.ifPresent(x-> System.out.println(x));

	}
}
```

- The map method can be used to modify the underlying object, since it returns a new Optional.

```java
public class MapDemo {
	public static void main(String[] args) {
		Optional.of(2)                                  // Optional(2)
	        .map(x -> ++x)                          // Optional(3)
	        .ifPresent(x -> System.out.println(x));
	}
}
```

- The flatMap method is similar to the map method, except that

- the result of its function is already wrapped in an Optional.

```java
public class FlatmapDmeo {
	public static void main(String[] args) {
		Optional.of("4")
			.flatMap((x)->Optional.of(Integer.parseInt(x)))
			.ifPresent(System.out::println);
	}
}
```

- An Optional chain can be used to track consumption of a resource.

```java
class Resource{
	static int count = 3;

	public Resource(){
		count --;
		System.out.println("Resource consumed, " + count + " remaining.");
	}
}
```

```java

public class FlatmapDmeo {
	public static void main(String[] args) {
		Optional.of(new Resource())
			.filter(x -> x.count>0)
			.map(x->new Resource())
			.filter(x->x.count>0)
			.map(x-> new Resource())
			.filter(x -> x.count>0)
			.map(x->new Resource());
	}
}

```
