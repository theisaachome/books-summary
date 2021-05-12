## Use in Traversing Object

- [Use in Traversing Object](#Use-in-Traversing-Object)

- [Traversing Java Arrays of Primitive Types](#Traversing-Java-Arrays-of-Primitive-Types)
- [Using Specializations of PrimitiveIterator](#Using-Specializations-of-PrimitiveIterator)
- [Using Specializations of PrimitiveIterator](#Using-Specializations-of-PrimitiveIterator)

- [Traversing Objects Using Spliterators](#Traversing-Objects-Using-Spliterators)

---

### Use in Traversing Object

- The traversal of data structures has been simplified
- through the use of consumers which can be used to replace the while loops associated with iterators.

### example

```java
class Car {
    private String make;
    private String model;
    public Car(String ma, String mo)
    {
make = ma;
model = mo; }
@Override
    public String toString() {return make + " " + model; }
}

```

```java
List<Car> cars = Arrays.asList(
    new Car("Nissan"   , "Sentra" ),
    new Car("Chevrolet", "Vega" ),
    new Car("Hyundai " , "Elantra")
);
```

#### The traditional way to traverse an object

```java
Iterator<Car> it = cars.iterator();
while (it.hasNext())
    System.out.println(it.next());
```

#### Java 8: the default forEachRemaining method which accepts a consumer

```java
public interface Iterator<E>{
    default void forEachRemaining(Consumer<? super E> action);
...
}
```

```java
cars.iterator().forEachRemaining(x -> System.out.println(x));
```

---

### Traversing Java Arrays of Primitive Types

- An Iterator<E> can be used to traverse a Collection<E> or any object that implements the Iterable<E> interface.

- The PrimitiveIterator interface can be used to traverse a Java array of certain primitive types.

- It is generic for two types :
  - T and T_CONS. Type T must be Integer, Long, or Double, and
  - type T_CONS must be the corresponding specialization of Consumer.

```java
public interface PrimitiveIterator<T,T_CONS> extends Iterator<T>{
    void forEachRemaining(T_CONS action);
}
```

#### A program needs a class that can traverse a Java array of ints.

- by implementing the PrimitiveIterator interface

- where the first type is Integer and

- the second type is IntConsumer.

#### example

```java
class IntIteratorGen implements
PrimitiveIterator<Integer,IntConsumer>{

    //The class should have a copy of the array and a cursor as fields.
    private int[] array;
    private int cursor;
    public IntIteratorGen(int... a){
    cursor = 0;
        array = Arrays.copyOf(a,a.length);
    }

    @Override
    public void forEachRemaining(IntConsumer c){
        while (hasNext())
        {
            c.accept(array[cursor]);
    cursor++; }
    }
    @Override
    public boolean hasNext() { return cursor < array.length; }
    @Override
    public Integer next(){
        int i = 0;
        if (hasNext()){
            i = array[cursor];
            cursor++;
        }
    return i;
    }
}
```

```java
class TestPrimitiveIteratorGen{
    public static void main(String[] args)
    {
        IntIteratorGen it = new IntIteratorGen(1, 2, 3, 4, 5);
        it.forEachRemaining((IntConsumer)x -> System.out.println(x));
    }
}
```

---

### Using Specializations of PrimitiveIterator

- Non-generic specializations for :
  - Integer,
  - Long, and
  - Double are available
  - as nested interfaces of PrimitiveIterator.

```java
public static interface PrimitiveIterator.OfInt
extends PrimitiveIterator<Integer, IntConsumer>{
     int nextInt();
    ...
}
```

```java
public static interface PrimitiveIterator.OfLong
extends PrimitiveIterator<Long, LongConsumer>{
     long nextLong();
    ...
}
```

```java
public static interface PrimitiveIterator.OfDouble
extends PrimitiveIterator<Double, DoubleConsumer>{
     double nextDouble();
    ...
}
```

#### example

- **intIterator**

```java
class IntIterator implements PrimitiveIterator.OfInt{
    private int[] array;
    private int cursor;
    public IntIterator(int... a){
    cursor = 0;
        array = Arrays.copyOf(a,a.length);
    }
    @Override
    public boolean hasNext() { return cursor < array.length; }
    @Override
    public int nextInt(){
        int i = 0;
        if (hasNext()){
            i = array[cursor];
        cursor++;
    }
    return i;
    }
}
```

---

#### example

- **LongIterator**

```java

class LongIterator implements PrimitiveIterator.OfLong{
    private long[] array;
    private int cursor;
    public LongIterator(long... a){
    cursor = 0;
        array = Arrays.copyOf(a,a.length);
    }
    @Override
    public boolean hasNext() { return cursor < array.length; }
    @Override
    public long nextLong(){
        long l = 0;
        if (hasNext())
        {
            l = array[cursor];
    cursor++; }
    return l;
    }
}
```

---

#### example

- **DoubleIterator**

```java
class DoubleIterator implements PrimitiveIterator.OfDouble{
    private double[] array;
    private int cursor;
    public DoubleIterator(double... a){
    cursor = 0;
        array = Arrays.copyOf(a,a.length);
    }
    @Override
    public boolean hasNext() { return cursor < array.length; }
    @Override
    public double nextDouble(){
        double d = 0;
        if (hasNext())
        {
            d = array[cursor];
    cursor++;
    }
    return d;
    }
}

```

```java
class TestPrimitiveIteratorSpecializations{
    public static void main(String[] args){
        IntIterator iit = new IntIterator(1, 2, 3, 4, 5);
        iit.forEachRemaining((IntConsumer)x ->
                             System.out.println(x));
        System.out.println();
        LongIterator lit = new LongIterator(6, 7, 8, 9, 10);
        lit.forEachRemaining((LongConsumer)x ->
                             System.out.println(x));
        System.out.println();
        DoubleIterator dit = new DoubleIterator(
                                 20.1, 21.2, 22.3, 23.4, 24.5);
        dit.forEachRemaining((DoubleConsumer)x ->
                     System.out.println(x));
}
}

```

---

### Traversing Objects Using Spliterators

- The Spliterator interface defines a default forEachRemaining method
- which accepts a consumer.
- Spliterator is useful for partitioning a collection into components through the use of its trySplit method.

```java
public interface Spliterator<T>{
  default void forEachRemaining(Consumer<? super T> action);
  Spliterator<T> trySplit();
  ...
}
```

#### example

- Traverse the list through the use of the spliterator’s forEachRemaining method.

```java
	List<Car> cars = Arrays.asList(
		new Car("Nissan"   , "Sentra" ),
		new Car("Chevrolet", "Vega" ),
		new Car("Hyundai " , "Elantra"),
		new Car("Buick"    , "Regal"));
	Spliterator<Car> spliterator = cars.spliterator();
	spliterator.forEachRemaining(car->System.out.println("In Spliterator: " + car));

```

### trySplit Method

- trySplit method can be used to partition the list.

- The result of the trySplit method is placed in a new spliterator named firstHalf.

- The original spliterator contains the remainder of the list elements (the second half of the list).

#### example

```java

public class TrySplitDemo {
	public static void main(String[] args) {
		Spliterator<Car> spliterator = Car.getCars().spliterator();
		Spliterator<Car> firstHalf = spliterator.trySplit();
		firstHalf.forEachRemaining(x->System.out.println("In 1st half : " + x));
		spliterator.forEachRemaining(x ->System.out.println("In 2nd half : " + x));
	}
}
```
