## Use in Collections

### CHAPTER 9 : Use in Collections

- [Table of Contents](#table-of-contents)
- [Use in Collections](#use-in-collections)
- [Removing Elements from a Collection](#Removing-Elements-from-a-Collection)
- [Populating an Array](#Populating-an-Array)
- [Replacing the Elements of a List or a Map](#Replacing-the-Elements-of-a-List-or-a-Map)
- [Parallel Computations on Arrays](#Parallel-Computations-on-Arrays)
- [Map Computations](#Map-Computations)
- [Map Merging](#Map-Merging)
- [Functional Interfaces and Sets](#Functional-Interfaces-and-Sets)

---

### Use in CCollections

- Functional interfaces can be used to perform several important operations on collections.

### Removing Elements from a Collection

- The removeIf method can be used to remove elements from a Collection object.

```java
default boolean removeIf(Predicate<? super E> filter);
```

- If true for an element of the collection, that element is removed from the collection.

#### example

- remove all elements beginning with “S” from the collection.

```java

public class RemoveIfDemo {
	public static void main(String[] args) {
		List<String> list = new ArrayList<String>();
		list.add("Super");
		list.add("Random");
		list.add("Silly");
		list.add("Strings");
		list.removeIf(s->s.charAt(0)=='S');
		list.forEach(System.out::println);

	}
}

```

---

### Populating an Array

- The Arrays class has several setAll methods

- that will populate each element of an array passed as the first argument using an operator or

- function passed as the second argument.

- The first parameter of the operator or function is the subscript corresponding to the array element.

```java
static void setAll(int[] array, IntUnaryOperator generator)
static void setAll(long[] array, IntToLongFunction generator)
static void setAll(double[] array, IntToDoubleFunction generator)
static <X> void setAll(X[] array, IntFunction<? extends X> generator)
```

#### example

- set each element of int array [];

```java
IntUnaryOperator intOperator = x-> x;
	int [] intArrays = new int[4];
	Arrays.setAll(intArrays, intOperator);		 for (int i : intArrays) {
		System.out.println(i);
    }
```

- sets each element of long array[]

```java
 IntToLongFunction gen5 = x -> 5;

	 long[] longArrays= new long[4];
	 Arrays.setAll(longArrays, gen5);
	 for (long l : longArrays) {
	    System.out.println(l);
	}
```

- sets each element of double array darr equal to a random number between 0.0 and 1.0.

```java
IntToDoubleFunction i2d = x-> (new Random()).nextFloat();
double [] darr = new double[4];
Arrays.setAll(darr, i2d);
for (double d : darr) {
    System.out.println(d);
}
```

- populates an array of strings such that each element contains the letter “S” repeated the number of subscript times.

```java
		 IntFunction<String> intFunc = x->{
			 String s= "";
			 for (int i = 0; i <= x; i++) {
				s += "S";
			}return s;
		 };
		 String [] stringArray = new String[4];
		 Arrays.setAll(stringArray, intFunc);

		 for (String string : stringArray) {
			System.out.println(string);
		}
```

---

### Replacing the Elements of a List or a Map

- All the elements in a list can be modified
- using the default replaceAll method and
- a UnaryOperator that specified how to perform the modification.

```java
default void replaceAll(UnaryOperation<E> operator);
```

#### example

- using List

```java

public class ReplaceAllDemo {
	public static void main(String[] args) {
		List<Integer> intList = Arrays.asList(16,12,8,4);
		UnaryOperator<Integer> divBy4 = x -> x/4;
		intList.replaceAll(divBy4);
		intList.forEach(System.out::println);
	}
}

```

- All the element in a map can also be modified using the default replaceAll method and a BiFunction.

```java
default void replaceAll(BiFunction<? super K, ? super V, ? extends V>
function);
```

#### example

```java
public class ReplaceAllMapDemo {
	public static void main(String[] args) {
		Map<String, String> map = new HashMap<String, String>();
		map.put("Smith", "Robert");
		map.put("Jones", "Alex");
		map.put("Isaac", "Home");
		BiFunction<String, String, String> bi = (k,v)->"Mr. " +  v;
		map.replaceAll(bi);
		map.forEach((k,v)->System.out.println(v + " " + k));
	}
}
```

---

### Parallel Computations on Arrays

- Computations on arrays can be performed in parallel to make programs run faster as long as doing so does not change the results of the computations.

```java
static void parallelPrefix(double[] array, int fromIndex, int toIndex,
DoubleBinaryOperator op);
```

```java
static void parallelPrefix(double[] array, DoubleBinaryOperator op);
```

```java
static void parallelPrefix(int[] array, int fromIndex, int toIndex,
IntBinaryOperator op);
```

```java
static void parallelPrefix(int[] array, IntBinaryOperator op);
```

```java
static void parallelPrefix(long[] array, int fromIndex, int toIndex,
LongBinaryOperator op);
```

```java
static void parallelPrefix(long[] array, LongBinaryOperator op);
```

```java
static void parallelPrefix(T[] array, int fromIndex, int toIndex,
BinaryOperator<X> op);
```

```java
static void parallelPrefix(T[] array, BinaryOperator<x> op);
```

#### example

```java
public class ComputateOnArrayDemo {
	public static void main(String[] args) {
		int [] array = {2,3,4,3};
		IntBinaryOperator op =(x,y)-> x *y;
		Arrays.parallelPrefix(array, op);
		for (int i : array) {
			System.out.println(i);
		}
	}
}

```

---

### Map Computations

- the following methods which perform inline computations on an entry in a map.

```java
default V compute<K key, BiFunction<? super K, ? super V, ? extends V>
remappingFunction);
```

```java
default V computeIfAbsent<K key, Function<? super K, ? extends V>
mappingFunction);
```

```java
default V computeIfPresent<K key, BiFunction<? super K, ? super V, ?
extends V>
remappingFunction);
```

#### example

- BiFunction is defined that returns null if the value is null
  (or if the key is not present), and
- returns the value divided by 4 otherwise.

```java

public class MapComputationDemo {
	public static void main(String[] args) {

		BiFunction<String, Integer, Integer> bin = (k, v) -> v == null ? null : v / 4;
		Map<String, Integer> map = new TreeMap<>();
		map.put("RED", 32);
		map.put("GREEN", null);
		map.put("BLUE", 40);
		System.out.println(map.compute("RED", bin));
		System.out.println(map.compute("GREEN", bin));
		System.out.println(map.compute("YELLOW", bin));
		System.out.println(map.compute("BLUE", bin));
		System.out.println();
		map.forEach( (x,y) -> System.out.println(x + " " + y));
	}
}

```

- The first call
  - the value 32 by 4 resulting in 8.
- The second call
  - results in null
  - since the value to the key “GREEN” is null,
    and
  - the entry “GREEN” is removed from the map.
- The third call

  - operates on the key “YELLOW”
  - which is not present and results in null,
  - so nothing is added to the Map.

---

### computeIfAbsent method

- The default computeIfPresent method performs its computation if the entry is present and the value is not null.

```java

public class ComputeIfMethodDemo {
	public static void main(String[] args) {
		Function<String,Integer> fi     = k -> k.length();
		Function<String,Integer> finull = k -> null;
		Map<String,Integer> map = new TreeMap<>();
		map.put("RED", 2);
		map.put("GREEN", null);
		System.out.println(map.computeIfAbsent("RED"   , fi));
		System.out.println(map.computeIfAbsent("GREEN" , fi));
		System.out.println(map.computeIfAbsent("YELLOW", fi));
		System.out.println(map.computeIfAbsent("BLACK" , finull));
		System.out.println();
		map.forEach( (x,y) -> System.out.println(x + " " + y));
	}
}

```

- The first call :
  - no computation since entry “RED” is present and its value is not null.
- The second call :
  - computes a new value 5 which is the length of the key “GREEN”, since the original value was null.
- The third call :
  - computes 6 which is the length of the key “YELLOW” which is absent from the map.
- Although entry “BLACK” is absent from the map,
  - the fourth call does not add an entry for it because mapping function finull results in null.

---

### Map Merging

- used to modify an existing value by merging portions of a new value with it according to a mapping function.

```java
default V merge(K key, V value, BiFunction<? super V, ? super V, ? extends
V> remappingFunction);
```

- If Not entry :
  - a new entry is created with the specified key and value.
- If the mapping function results in a null value, the entry is removed from the map.
- The new value is not allowed to be null.

#### example

```java
class MyClass{
	int i1;
	int i2;
	String s;
	public MyClass(int x, int y , String z) {
		i1=x;
		i2=y;
		s =z;
	}
	@Override
	public String toString() {
		return i1 + " " + i2 + " " + s;
	}
}
```

```java
public class MapDemo {
	public static void main(String[] args) {
		Map<String, MyClass> m = new HashMap<String, MyClass>();
		m.put("k1", new MyClass(1, 2, "Dog"));
		BiFunction<MyClass, MyClass, MyClass> changeI2 = (oldValue,newValue)->new MyClass(oldValue.i1, newValue.i2, oldValue.s);
		BiFunction<MyClass, MyClass, MyClass> changeString = (oldValue,newValue)->new MyClass(oldValue.i1, oldValue.i2, newValue.s);
		System.out.println(m.merge("k1", new MyClass(0, 5, null), changeI2));
		System.out.println(m.merge("k1", new MyClass(0,0,"Cat"), changeString));

		System.out.println(m.merge("k2", new MyClass(6,7,"Bird"), changeString));
		m.forEach((x,y)->System.out.println( x +"  " + y));

	}
}
```

- BiFunction changeI2 and the merge method are :

  - used to change field i1 of entry “k1” to 5.

- BiFunction changeS and the merge method can be used to change field s of entry “k1” to “Cat”.

- Entry “k2” does not exist,

  - so a new entry is created with the specified value without using the mapping function.

---

### Functional Interfaces and Sets

- the Set interface supports the remove operation,
- the removeIf method can be used to remove elements from a set.

#### example

```java

public class SetDemo {
	public static void main(String[] args) {
		Set<String>names = new TreeSet<String>();

		names.add("Jeremy");
		names.add("Javier");
		names.add("Rose");

		names.removeIf((n)->n.charAt(0)=='J');
		names.forEach(System.out::println);
	}
}

```
