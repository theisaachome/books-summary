## Suppliers

### Chapter:07 => Suppliers

- [The supplier Interface](#the-supplier-interface)
- [Wrapping User Prompts in a Supplier](#Wrapping-User-Prompts-in-a-Supplier)
- [Non-generic Specializations of Suppliers](#Non-generic-Specializations-of-Suppliers)

---

### The supplier Interface

- Supplier is used to generate data.
- A Supplier object is specified with type parameter T.
- Its functional method, called get,
- takes no arguments and returns an object of type T.

```java
@FunctionalInterface
public interface Supplier<T>{
    T get();
}

```

#### example

```java
public class SupplierDemo {
	public static void main(String[] args) {
		Supplier<String> generateString = ()->{
			Scanner scan = new Scanner(System.in);
			System.out.print("Enter A String:");
			return scan.nextLine();
		};
		Supplier<Integer> generateInteger =()->{
			Random rand = new Random();
			return rand.nextInt(100);
		};

		System.out.println(generateInteger.get());
		System.out.println(generateInteger.get());

		System.out.println(generateString.get());
		System.out.println(generateString.get());
	}
}

```

---

### Wrapping User Prompts in a Supplier

```java

```

### Non-generic Specializations of Suppliers

- The Java API provides :
  - BooleanSupplier,
  - IntSupplier,
  - LongSupplier, and
  - DoubleSupplier,
  - which are non-generic specializations of the Supplier interface.
  - They generate boolean, int, long, and double primitive types, respectively.

```java
@FunctionalInterface
public interface BooleanSupplier {
   boolean getAsBoolean();
}
@FunctionalInterface
public interface IntSupplier {
   int getAsInt();
}
@FunctionalInterface
public interface LongSupplier {
   long getAsLong();
}
@FunctionalInterface
public interface DoubleSupplier {
   double getAsDouble();
}
```

---

#### example

```java
class TestSpecials{
    public static Random rand = new Random();
    public static void main(String[] args)
    {
        BooleanSupplier genBol = () ->
            (rand.nextInt(2) == 1)? true:false;
        IntSupplier     genInt = () -> rand.nextInt();
        LongSupplier    genLng = () -> rand.nextLong();
        DoubleSupplier  genDbl = () -> rand.nextDouble();
        System.out.println(genBol.getAsBoolean());
        System.out.println(genInt.getAsInt());
        System.out.println(genLng.getAsLong());
        System.out.println(genDbl.getAsDouble());
} }

```
