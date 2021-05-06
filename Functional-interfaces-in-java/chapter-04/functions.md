## The Functional Interface

[Table of Contents](#table-of-contents)

- [The Functional Interface](#The-functional-interface)
- [Passing a Generic Function to a Method](#Passing-a-Generic-Function-to-a-Method)
- [Passing a Function with Restricted](#Passing-a-Function-with-Restricted)
- [Function Chaining](#Function-Chaining)
- [Functions Which Convert from Primitive Types](#Functions-Which-Convert-from-Primitive-Types)
- [Functions Which Convert to Primitive Types](#Functions-Which-Convert-to-Primitive-Types)
- [Non-generic Specializations of Functions](#Non-generic-Specializations-of-Functions)
- [Binnary Functions](#Binnary-Functions)
- [Using BiFunctions](#Using-BiFunctions)
- [BiFunctions Which Convert to Primitive Types
  ](#BiFunctions-Which-Convert-to-Primitive-Types)

---

### The Functional Interface

- Function is a functional interface :
  - with two type parameters T and R.
  - Its functional method, called apply,
  - takes an argument of type T and returns an object of type R.

```java
@FunctionalInterface
public interface Function<T, R>{
    R apply(T t);
}
```

#### example

```java
Function<String,Integer>f;

f= x-> Integer.parseInt(x);

Integer i = f.apply("100");

Sysout.out.println(i);
```

---

### Passing a Generic Function to a Method

```java
private static <T,R> R transform(T t, Function<T, R> f){
    return f.apply(t);
}
```

```java
class Transformer{
    private static <T,R> R transform(T t, Function<T, R> f)
    {
        return f.apply(t);
    }
    public static void main(String[] args)
    {
        Function<String , Integer> fsi = x -> Integer.parseInt(x);
        Function<Integer, String>  fis = x -> Integer.toString(x);
        Integer i = transform("100", fsi);
        String  s = transform(200  , fis);
        System.out.println(i);
        System.out.println(s);
} }
```

---

### Passing a Function with Restricted

- the type parameters of a function passed to a method are known or can be restricted to certain types.

#### example

- a function to a parse method where the first type is String and
- the second type is constrained to a subclass of Number.

```java

public class NumberParser {
	private static <R extends Number> R parse(String s,Function<String, R>f) {
		return f.apply(s);
	}

	public static void main(String[] args) {
		String [] numbers  = {"10","20","30","40","50","60"};
		List<Function<String, ? extends Number>> list = new ArrayList<>();

		Number [] results = new Number[numbers.length];

		list.add(x-> Byte.parseByte(x));
		list.add(x-> Short.parseShort(x));
		list.add(x-> Integer.parseInt(x));
		list.add(x-> Long.parseLong(x));
		list.add(x-> Float.parseFloat(x));
		list.add(x-> Double.parseDouble(x));

		for (int i = 0; i < numbers.length; i++) {
			results[i]=parse(numbers[i], list.get(i));
			System.out.println(results[i]);
		}
	}
}

```

### Function Chaining

- the Function interface has default methods that return new functions, supporting chains of functions that can perform many conversions in series.

### the andThen Method

```java
default <V> Function<T,V> andThen(Function<? super R>, ? extends V> after);
```

- an additional operation after the operation specified by the apply method completes.

#### example

```java
public class AndThenDemo {
	public static void main(String[] args) {
		Function<String, Boolean> fsb = x -> Boolean.parseBoolean(x);
		var result = fsb.andThen(x-> x? 1:0)
				.apply("true");
		System.out.println(result);
	}
}
```

- The apply method converts the string “true” to Boolean value true,

- which is input to the function executed by the andThen method.

- This function converts Boolean value true to Integer value 1.

---

### the compose Method

```java
default <V> Function<V,R> compose(Function<? super V>, ? extends T> before);
```

- The Function interface’s default compose method
- applies a preliminary operation
- before the operation specified by the apply method is executed.

```java
public class ComposeMethod {
	public static void main(String[] args) {
		Function<Boolean, Integer> fbi= x -> x? 1:0;
		Function<String,Boolean> fsb = s -> Boolean.parseBoolean(s);

		var result = fbi.compose(fsb)
					.apply("true");
		System.out.println(result);
	}
}
```

- the compose method uses Function fsb to convert string “true” to Boolean value true.
- Then, the apply method uses Function fbi to convert true to Integer value 1.

---

### Functions Which Convert from Primitive Types

- The Java API provides :
  - the IntFunction,
  - LongFunction, and
  - DoubleFunction interfaces which convert from int, long, and double primitive types, respectively.

```java
@FunctionalInterface
public interface IntFunction<R> {
    R apply(int value);
}
@FunctionalInterface
public interface LongFunction<R> {
    R apply(long value);
}
@FunctionalInterface
public interface DoubleFunction<R> {
    R apply(long value);
}
```

```java
IntFunction<String> fi = x -> (new Integer(x)).toString();
DoubleFunction<Boolean> fd = x -> x > 5.0? true : false;
LongFunction<Integer> fl = x -> (int)x;
System.out.println(fi.apply(5));
System.out.println(fd.apply(4.5));
System.out.println(fl.apply(20L));
```

---

### Functions Which Convert to Primitive Types

- The Java API provides :
  - the ToIntFunction,
  - ToLongFunction, and
  - ToDoubleFunction
  - interfaces which convert to int, long, and double primitive types, respectively.

```java
@FunctionalInterface
public interface ToIntFunction<T> {
    int applyAsInt(T value);
}
@FunctionalInterface
public interface ToLongFunction<T> {
    long applyAsLong(T value);
}
@FunctionalInterface
public interface ToDoubleFunction<T> {
    double applyAsDouble(T value);
}
```

```java
ToIntFunction<String> ti     = x -> Integer.parseInt(x);
ToLongFunction<Double> tl    = x -> x.longValue();
ToDoubleFunction<Integer> td = x -> (new Integer(x)).doubleValue();
System.out.println(ti.applyAsInt("5"));
System.out.println(tl.applyAsLong(5.1));
System.out.println(td.applyAsDouble(7));
```

---

### Non-generic Specializations of Functions

- The Java API provides :

  - non-generic specializations of the Function interface

  - which convert between int, long, and double primitive types.

  - DoubleToIntFunction,

  - DoubleToLongFunction,

  - IntToDoubleFunction,

  - IntToLongFunction,

  - LongToDoubleFunction, and

  - LongToIntFunction.

```java
DoubleToIntFunction  di = x -> (new Double(x)).intValue();
DoubleToLongFunction dl = x -> (new Double(x)).longValue();
IntToDoubleFunction  id = x -> (new Integer(x)).doubleValue();
IntToLongFunction    il = x -> (new Integer(x)).longValue();
LongToDoubleFunction ld = x -> (new Long(x)).doubleValue();
LongToIntFunction    li = x -> (new Long(x)).intValue();

System.out.println(di.applyAsInt(4.1));
System.out.println(dl.applyAsLong(5.2));
System.out.println(id.applyAsDouble(6));
System.out.println(il.applyAsLong(7));
System.out.println(ld.applyAsDouble(8));
System.out.println(li.applyAsInt(9));
```

---

### Binnary Functions

- Sometimes a conversion requires two input types.
- The BiFunction interface specifies two type parameters for input types in addition to the output type parameter.

```java
@FunctionalInterface
public interface BiFunction<T,U,R> {
    R apply(T t, U u);
}
```

```java
BiFunction<Integer,Character,String> bi = (x, z) -> {
    if (Character.isUpperCase(z))
        return (x%2)==0? "EVEN" : "ODD";

    return (x%2)==0? "even" : "odd";
};
System.out.println(bi.apply(4,'U'));
```

### Using BiFunctions

```java
default <V> BiFunction<T,U,V> andThen(Function<? super R>, ? extends V> after);
```

```java
Function<String,Double> bi2 = x ->
    x.equalsIgnoreCase("even")? 3.0 : 4.0;
Double d = bi.andThen(bi2)  // Function<String,Double>
             .apply(4,'U'); // BiFunction<Integer,Character,String>
System.out.println(d);
```

---

### BiFunctions Which Convert to Primitive Types

- The Java API provides :

  - the ToIntBiFunction,

  - ToLongBiFunction, and

  - ToDoubleBiFunction interfaces which convert to int, long, and double primitive types, respectively.

```java
@FunctionalInterface
public interface ToIntBiFunction<T,U> {
   int applyAsInt(T t, U u);
}
@FunctionalInterface
public interface ToLongBiFunction<T,U> {
   long applyAsLong(T t, U u);
}
@FunctionalInterface
public interface ToDoubleBiFunction<T,U> {
   double applyAsDouble(T t, U u);
}
```

```java
ToIntBiFunction<String, Double> tib = (x,z) ->
    Integer.parseInt(x) + (new Double(z)).intValue();
ToLongBiFunction<Double, String> tlb = (x,z) ->
    x.longValue() + Long.parseLong(z);
ToDoubleBiFunction<Integer, Long> tdb = (x,z) ->
    (new Integer(x)).doubleValue() + (new Long(z)).doubleValue();
System.out.println(tib.applyAsInt("5",4.2));
System.out.println(tlb.applyAsLong(5.1, "6"));
System.out.println(tdb.applyAsDouble(7, 8L));
```

**[⬆ back to top](#table-of-contents)**
