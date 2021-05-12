## Consumer

### Chapter :06 Consumers

- [The Consumer Interface](#the-consumer-interface)
- [Chains of Consumers ](#Chains-of-Consumers)
- [Non-generic Specializations of Consumers](#Non-generic-Specializations-of-Consumers)
- [The BiConsumer Interface](#The-BiConsumer-Interface)
- [Specializations of BiConsumer](#Specializations-of-BiConsumer)

---

### The Consumer Interface

- Consumer is used to process data.
- A Consumer object is specified with type parameter T.
- Its method accept,
- takes an argument of type T and has return type void.

---

###

```java
@FunctionalInterface
public interface Consumer<T>{
    void accept(T t);
...
}
```

#### example

```java
public class ConsumerDemo {
	private static int sum = 0;
	public static void main(String[] args) {
		Consumer<Integer> con = x-> sum +=x;

		con.accept(5);
		con.accept(4);
		System.out.println(sum);
	}
}

```

---

### Chains of Consumers

- andThen method further processes the input argument after the accept method completes.

```java
default Consumer<T> andThen(Consumer<? super T> after);
```

- computes the sum and the product of 4 and 5.

- consum, is executed first, and the argument 4 is added to sum.

- Then the code in conprod, the consumer passed to the andThen method, is executed,

- and the argument 4 is multiplied by prod.

- The process is repeated for the argument 5.

```java
public class ConsumerAndThenDemo {
	private static int sum = 0;
	private static int prod = 1;

	public static void main(String[] args) {
		Consumer<Integer> consum = x -> sum += x;
		Consumer<Integer> conprod = x -> prod *= x;
		consum.andThen(conprod).accept(4);

		System.out.println("Sum : " + sum);
		consum.andThen(conprod).accept(5);
		System.out.println("Prod : " + prod);
	}
}

```

---

#### more example on chain

```java
public class ComputePolynomial {
	private static int fx=0;
	public static void main(String[] args) {
		Consumer<Integer> poly = x -> fx += 5 * (int)Math.pow(x, 4);

		poly.andThen(x-> fx += 7 * (int)Math.pow(x,3))
		.andThen(x-> fx += 4 * (int)Math.pow(x,2))
		.andThen(x-> fx += 3 * x)
		.andThen(x-> fx += 8)
		.andThen(x->System.out.println(fx))
		.accept(2);

	}
}

```

---

### Non-generic Specializations of Consumers

- The Java API provides
  - IntConsumer,
  - LongConsumer, and
  - DoubleConsumer,
  - which are non- generic specializations of the Consumer interface.
- They process int, long, and double primitive types, respectively.

```java
@FunctionalInterface
public interface IntConsumer{
    void accept(int value);
...
}
@FunctionalInterface
public interface LongConsumer{
    void accept(long value);
...
}
@FunctionalInterface
public interface DoubleConsumer{
    void accept(double value);
...
}
```

#### example

```java
public class ConsumerSpecials{
    private static int    a = 0;
    private static long   b = 0;
    private static double c = 1.0;
    public static void main(String[] args){
        IntConsumer    ic = x -> a = x + 3;
        LongConsumer   lc = x -> b = x / 2L;
        DoubleConsumer dc = x -> c = x * c;
        ic.andThen(x -> System.out.println(a))
          .accept(2);
        lc.andThen(x -> System.out.println(b))
          .accept(6L);
        dc.andThen(x -> System.out.println(c))
          .accept(4.0);
    }
}
```

---

### The BiConsumer Interface

- inputs of two different generic types.

- The BiConsumer functional interface specifies type parameters T and U.

- Its accept method takes arguments of types T and U and has return type void.

```java

@FunctionalInterface
public interface BiConsumer<T,U>{
    void accept(T t, U u);
...
}
```

#### example

```java
public class BiConsumerDemo {
	private static int sum= 0;
	private static String component = "";

	public static void main(String[] args) {
		 BiConsumer<Integer, String> bi = (x,y)->{
			 sum += x;
			 component += y;
		 };

		 bi.andThen((x,y)->System.out.println(x + " " +y))
		 .accept(6, "Term 1");

		 bi.andThen((x,y)->System.out.println( " added "+ x + " " + y + " result = " + sum + " " + component))
		 .accept(7, ",Term 2");
	}
}

```

---

### Specializations of BiConsumer

- The Java API provides :
  - the ObjIntConsumer,
  - ObjLongConsumer, and
  - ObjDoubleConsumer
  - interfaces which specialize the second argument to the apply method.

```java
@FunctionalInterface
public interface ObjIntConsumer<T> {
   void accept(T t, int value);
}
@FunctionalInterface
public interface ObjLongConsumer<T> {
   void accept(T t, long value);
}
@FunctionalInterface
public interface ObjDoubleConsumer<T> {
   void accept(T t, double value);
}
```

#### example

```java
ObjIntConsumer<String> oic    = (x,y) ->
    System.out.println(x + " = " + y);
ObjLongConsumer<String> olc   = (x,y) ->
    System.out.println(Long.parseLong(x) + y);
ObjDoubleConsumer<String> odc = (x,y) ->
    System.out.println(x + (new Double(y)).toString());
oic.accept("Value", 4);
olc.accept("7", 2L);
odc.accept("DBL", 4.1);
```
