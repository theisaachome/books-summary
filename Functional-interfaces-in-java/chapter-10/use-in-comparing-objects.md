## **Use in Comparing Objects**

-[Table of Contents](#table-of-contents)

- [Use in Comparing Objects](#Use-in-Comparing-Objects)

- [The Comparator Interface](#The-Comparator-Interface)

- [Some Useful Comparator Methods](#Some-Useful-Comparator-Methods)

- [The Comparator comparing Methods](#The-Comparator-comparing-Methods)
- [Specializations of the Comparator comparing Method](#Specializations-of-the-Comparator-comparing-Method)
- [Building Chains of Comparators](#Building-Chains-of-Comparators)

- [Specializing Comparator Chain Components](#Specializing-Comparator-Chain-Components)

- [Using Comparators to Sort Lists](#Using-Comparators-to-Sort-Lists)
- [Using Comparators to Organize Maps](#Using-Comparators-to-Organize-Maps)

---

### **Use in Comparing Objects**

- Functional interfaces have changed the way data is compared in Java.

- This is due to the enhancements :
  - to the Comparator interface and
  - the addition of several methods that utilize it in the Java API.

### **The Comparator Interface**

- To compare two objects.

  - A Comparator is specified with type parameter T.

  - Its functional method compare, takes two arguments of type T and returns an integer.

```java
@FunctionalInterface
public interface Comparator<T>{
    int compare(T o1, T o2);
}
```

#### **Example**

```java
public class ComparatorDemo {
	public static void main(String[] args) {
		Comparator<String> byConstants = (x,y)-> removeVowels(x).compareTo(removeVowels(y));
		System.out.println(byConstants.compare("Larry", "Libby"));
	}

	public static String removeVowels(String s) {
		return s.replaceAll("[aeiou]", "");
	}
}

```

- The compareTo method :

  - returns 1 if the calling object is greater than its argument,

    ```java
        Comparator<Integer> byIntComparator = (x,y) -> x.compareTo(y);
        System.out.println(byIntComparator.compare(1002, 1000));
        // output: 1
    ```

  - returns -1 if the calling object is less than its argument, and

  ```java
      Comparator<Integer> byIntComparator = (x,y) -> x.compareTo(y);
      System.out.println(byIntComparator.compare(1000, 1500));
      // output: -1
  ```

  - returns 0 if they are the same.

  ```java
      Comparator<Integer> byIntComparator = (x,y) -> x.compareTo(y);
      System.out.println(byIntComparator.compare(1000, 1000));
       // output: 0
  ```

---

### Some Useful Comparator Methods

- The following Comparator methods are useful.

```java
static<T extends Comparable<? super T>> Comparator<T> naturalOrder();
static<T extends Comparable<? super T>> Comparator<T> reverseOrder();
static <T> Comparator<T> nullsFirst(Comparator<? super T> comparator);
static <T> Comparator<T> nullsLast(Comparator<? super T> comparator);
default Comparator<T> reversed();
```

#### **naturalOrder Example**

```java
public class NaturalOrderDemo {
	public static void main(String[] args) {
		Comparator<String> natural = Comparator.naturalOrder();
		System.out.println(natural.compare("Larry", "Lirry"));
	}
}

```

---

#### **reverseOrder Example**

```java
public class ReverseOrderDemo {
	public static void main(String[] args) {
		Comparator<String> natural = Comparator.reverseOrder();
		System.out.println(natural.compare("Larry", "Lirry"));
	}
}
```

#### **nullsFirst Example**

- Passing a null string to the Comparator object’s compare method will cause a NullPointerException.
- This can be prevented by passing the comparator to the nullsFirst method which creates a new comparator.

```java
public class NullFirstDemo {
	public static void main(String[] args) {
		Comparator<String> myComparator = (x,y)-> x.compareTo(y);
		System.out.println(Comparator.nullsFirst(myComparator).compare("larry", null));
	}
}
```

- A comparator built by the nullsFirst method
  - treats nulls as being less than non-null objects,

---

#### **nullsLast example**

- nullsLast method creates a comparator that treats nulls as being greater than non-null objects,
- so the following statement displays -1.

```java
public class NullsLastDemo {
	public static void main(String[] args) {
		Comparator<String> myCom = (x,y)-> x.compareTo(y);
		System.out.println(Comparator.nullsLast(myCom).compare("Larry", null));
	}
}
```

#### **reverse example**

```java
Comparator<String> byConstants = (x,y)-> removeVowels(x).compareTo(removeVowels(y));
		System.out.println(byConstants.compare("Larry", "Libby"));
		System.out.println(byConstants.reversed().compare("Larry", "Libby"));
```

- Note: _`the reverseOrder method creates a new comparator that compares in the reverse of the natural ordering. the reverse method takes an existing comparator and reverses the ordering of its comparisons.`_

---

### The Comparator comparing Methods

- The static `comparing()` methods :

  - create comparators that extract the field to be compared (the key) from an object before performing the comparison.

```java
static <T, U extends Comparable<? super U>> Comparator<T>
         comparing(Function<? super T, ? extends U> keyExtractor);
static <T, U> Comparator<T>
         comparing(Function<? super T, ? extends U> keyExtractor,
                   Comparator<? super U> comparator);
```

#### **Example**

- compare students based on grade point average.

```java

public class Student {
	private String name;
	private Integer id;
	private Double gpa;
	public Student(String name, Integer id, Double gpa) {
		super();
		this.name = name;
		this.id = id;
		this.gpa = gpa;
	}
}

```

- The program should write a function that extracts the gpa field from a Student object.
- This will be the key used for comparison of two students.

```java
	Student s1 = new Student("Larry", 1000, 3.82);
	Student s2 = new Student("Libby", 1001, 3.76);
	Function<Student, Double>  gpaKey = x->x.getGpa();
	Comparator<Student> byGpa = Comparator.comparing(gpaKey);
	System.out.println(byGpa.compare(s1, s2));
```

- infering the types of the Function object based on the type of the Comparator object and
- the return type of the function’s lambda,

```java
Comparator<Student> byGpa2 = Comparator.comparing(x -> x.gpa);
```

- compares students based on id

```java
	Comparator<Student> byId = Comparator.comparing(x-> x.getId());
	System.out.println(byId.compare(s1, s2));
```

- compares students based on name.

```java
	Comparator<Student> byName = Comparator.comparing(x->x.getName());
	System.out.println(byName.compare(s1, s2));
```

---

```java
	Comparator<Student> byGpaCeil= Comparator.comparing(x->x.getGpa(),(x,y)->(int)(Math.ceil(x)-Math.ceil(y)));
	System.out.println(byGpaCeil.compare(s1, s2));
```

#### **compare the lists**

- comparator compares two List<Integer> objects by their first elements:

```java
class ListWrapper {
	List<Integer> list;

	public ListWrapper(Integer... i) {
		list = Arrays.asList(i);
	}

}
```

```java
Comparator<List<Integer>> byElement0 =
      (x,y) -> x.get(0).compareTo(y.get(0));
```

```java
ListWrapper list1 = new ListWrapper(2, 4, 6);
ListWrapper list2 = new ListWrapper(1, 3, 5);
Comparator<ListWrapper> byList
   = Comparator.comparing(x -> x.list, byElement0);
System.out.println(byList.compare(list1,list2));
```

---

### Specializations of the Comparator comparing Method

- The Java API provides specializations to the Comparator’s comparing method that extract Doubles, Integers, and Long keys

- from objects before comparing them based on natural ordering.

```java
static <T> Comparator<T>
         comparingDouble(ToDoubleFunction<? super T>keyExtractor);
static <T> Comparator<T>
         comparingInt(ToIntFunction<? super T>keyExtractor);
static <T> Comparator<T>
         comparingLong(ToLongFunction<? super T>keyExtractor);
```

#### **comparingDouble method**

- The comparingDouble method compares two Doubles based on natural ordering using the key extracted by a ToDoubleFunction.

```java
public class ComparingDoubleDemo {
	public static void main(String[] args) {
		Student s1 = new Student("Larry", 1000, 3.82);
		Student s2 = new Student("Libby", 1001, 3.76);

		ToDoubleFunction<Student> byGpa = x->x.getGpa();

		System.out.println(Comparator.comparingDouble(byGpa).compare(s1, s2));
	}
}

```

---

#### **comparingInt method**

- The comparingInt method compares two Integers based on natural ordering using the key extracted by a ToIntFunction.

```java
public class ComparingIntDemo {
	public static void main(String[] args) {
		Student s1 = new Student("Larry", 1000, 3.82);
		Student s2 = new Student("Libby", 1001, 3.76);
		// the compiler would not be able to
		// infer that the type of lambda parameter x is Student,
		ToIntFunction<Student> byId = (Student x)-> x.getId();
		System.out.println(Comparator.comparingInt(byId).compare(s1, s2));
	}
}
```

---

#### **comparingLong Method**

- The comparingLong method compares two Longs based on natural ordering using the key extracted by a ToLongFunction.

```java
class LongWrapper{
	Long l;
	public LongWrapper(long a) {
		l =a;
	}
}
```

```java
public class ComparingLongDemo {
	public static void main(String[] args) {
		LongWrapper l1= new LongWrapper(4L);
		LongWrapper l2= new LongWrapper(4L);
		ToLongFunction<LongWrapper> lKey =  x-> x.l;
		System.out.println(Comparator.comparingLong(lKey).compare(l1, l2));
	}
}
```

---

### Building Chains of Comparators

- The default `thenComparing` methods return a comparator that is used for further comparison if the calling comparator determines that the objects being compared are equal.

```java
default Comparator<T> thenComparing(Comparator<? super T> other);
```

```java
default <U extends Comparable<? super U>> Comparator<T>
thenComparing(Function<? super T, ? extends U> keyExtractor);
```

```java
default <U> Comparator<T>
    thenComparing(Function<? super T, ? extends U> keyExtractor,
                  Comparator<? super U> keyComparator);
```

#### example of using thenComparing

```java
		Student s1 = new Student("Joseph", 1000, 3.82);
		Student s2 = new Student("Joseph", 1002, 3.82);
		Comparator<Student> byName = Comparator.comparing(x -> x.getName());
		Comparator<Student> byId = Comparator.comparing(x -> x.getId());
		Comparator<Student> byGpa = Comparator.comparing(x -> x.getGpa());

		// byName
		System.out.println(byName.compare(s1, s2));

		// thenComparing by gpa
		System.out.println(byName.thenComparing(byGpa).compare(s1, s2));

		// byName->byGpa->byId
		System.out.println(byName.thenComparing(byId).thenComparing(byGpa).compare(s1, s2));

		System.out.println(byName.thenComparing(byGpa).thenComparing(byId).compare(s1, s2));

		System.out.println(byName.thenComparing(x -> x.getId()).thenComparing(x -> x.getGpa()).compare(s1, s2));
```

#### **A third signature of the thenComparing **

- A third signature of the thenComparing method accepts both a function and a comparator

```java
Comparator<Student> byNameConsonants
        = Comparator.comparing( x -> x.name,
                               (x,y) ->
                    removeVowels(x).compareTo(removeVowels(y)));
Comparator<Integer> byDifference = (x,y) -> x - y;
Comparator<Double> byCeil =
        (x,y) -> (int)(Math.ceil(x) - Math.ceil(y));
```

```java
Student s3 = new Student("Jean", 1003, 3.86);
Student s4 = new Student("Jen" , 1005, 3.69);

System.out.println(byNameConsonants.thenComparing(
        x -> x.id,byDifference)
        .thenComparing(x -> x.gpa,byCeil)
        .compare(s3, s4));
```

---

### **Specializing Comparator Chain Components**

- Comparator interface’s thenComparing signature that accepts a specialized function object.

- These specializations extract Doubles, Integers, and Long keys from objects before comparing them based on natural ordering.

```java
default Comparator<T> thenComparingInt(ToIntFunction<? super T>
keyExtractor);
default Comparator<T> thenComparingLong(ToLongFunction<? super T>
keyExtractor);
default Comparator<T> thenComparingDouble(ToDoubleFunction<? super T>
keyExtractor);
```

#### Example

- `thenComparingInt` extracts the id field and compares 1006 to 1007 by natural ordering,
- which results in -1.

```java
Student s5 = new Student("Kaitlyn", 1006, 3.69);
Student s6 = new Student("Jane" , 1007, 3.69);
System.out.println(byGpa.thenComparingInt( x-> x.getId() )
                        .compare(s5,s6));
```

---

#### Example

- `thenComparingDouble` extracts the gpa field and compares 3.86 to 3.69 by natural ordering, which results in 1.

```java
Student s7 = new Student("Robert", 1008, 3.86);
Student s8 = new Student("Robert", 1009, 3.69);
System.out.println(byName.thenComparingDouble( x-> x.getGpa() )
                         .compare(s7,s8));
```

---

### **Using Comparators to Sort Lists**

- Comparators will most frequently be used to sort lists and streams.
- The List<X> interface has a method named sort which accepts a Comparator<X> argument that is used for the comparisons during the sort.

#### **Example**

- sort the following list of students first by GPA ceiling, then by name, and then by id:

```java
List<Student> students = Arrays.asList(
        new Student("Joseph"  , 1623, 3.54),
        new Student("Annie"   , 1923, 2.94),
        new Student("Sharmila", 1874, 1.86),
        new Student("Harvey"  , 1348, 1.78),
        new Student("Grace"   , 1004, 3.90),
        new Student("Annie"   , 1245, 2.87)
);
```

#### the program sorts the list by GPA ceiling only

```java
Comparator<Student> byGpaCeil =
    Comparator.comparing( x -> x.gpa,
                         (x,y) -> (int)(Math.ceil(x)
                                      - Math.ceil(y)));
students.sort(byGpaCeil);
students.forEach(x-> System.out.println(x));
```

#### the program further sorts the list by name.

- This is accomplished by adding a thenComparing call that extracts the student’s name to the end of the comparator chain.

```java
students.sort(byGpaCeil
             .thenComparing(x -> x.getName()));
students.forEach(x-> System.out.println(x));
```

#### the program further sorts the list by id.

```java
students.sort(byGpaCeil
             .thenComparing(x -> x.id)
             .thenComparing(x -> x.name));
students.forEach(x-> System.out.println(x));
```

---

### **Using Comparators to Sort Java Arrays**

- Comparators can operate on Java arrays in a fashion similar to lists through the use of the Arrays class.

```java
Student[] students = {
        new Student("Joseph"  , 1623, 3.54),
        new Student("Annie"   , 1923, 2.94),
        new Student("Sharmila", 1874, 1.86),
        new Student("Harvey"  , 1348, 1.78),
        new Student("Grace"   , 1004, 3.90),
        new Student("Annie"   , 1245, 2.87)
};
```

- The students array is copied in order to perform two different sorts on its elements

```java
Student[] studentsCopy = Arrays.copyOf(students, students.length);
```

- The Array class’s static sort method sorts a Java array using a specified comparator.

```java
Comparator<Student> byGpaCeil =
Comparator.comparing( x -> x.gpa,
                     (x,y) -> (int)(Math.ceil(x)
                                  - Math.ceil(y)));
Arrays.sort(students,
            byGpaCeil
           .thenComparing(x -> x.id)
           .thenComparing(x -> x.name));
for (Student s : students)
     System.out.println(s);
```

- The Array class’s sort method can be used to sort a range of elements.

```java
Arrays.sort(studentsCopy, 2, 5,Comparator.comparing(x -> x.name));
    for (Student s : studentsCopy)
     System.out.println(s);
```

---

### **Using Comparators to Organize Maps**

- Several implementations of the Map interface can be instantiated with a comparator to organize its entries in a particular order.

```java
Comparator<String> byConsonants = (x,y) ->
        removeVowels(x).compareTo(removeVowels(y));
TreeMap<String,String> pets = new TreeMap<>(byConsonants);
pets.put("gerbil", "small cute rodents");
pets.put("guinea pig", "rodents, not pigs");
pets.put("cat", "have nine lives");
pets.put("chicken", "more populous than people");
pets.forEach((x,y) -> System.out.println(x + ", " + y));
```

- Comparators can also be used to compare Map.Entry objects.

#### example

- two Map.Entry objects are compared by key using the comparingByKey method.

```java
Comparator<Map.Entry<String,String>> cmap =
    Map.Entry.comparingByKey();
Map.Entry<String,String> cat = pets.ceilingEntry("cat");
Map.Entry<String,String> chicken = pets.ceilingEntry("chicken");
System.out.println(cmap.compare(cat, chicken));
```

- The comparingByKey method can also accept a comparator.

```java
Comparator<Map.Entry<String,String>> cmapCons =
      Map.Entry.comparingByKey(byConsonants);
System.out.println(cmapCons.compare(cat, chicken));

```

- Map.Entry objects can be compared by value using the comparingByValue method.

```java
Comparator<Map.Entry<String,String>> cval =
      Map.Entry.comparingByValue();
System.out.println(cval.compare(cat, chicken));
```

---

### **Using Comparators in BinaryOperator Methods**

- The BinaryOperator<T> maxBy and minBy methods compare two objects of type X based on a Comparator<T>.

#### **Example**,

- compares two Integers based on absolute value is defined.

- BinaryOperators using the static maxBy and minBy methods

- which accept a comparator.

- The BinaryOperator object’s apply methods now compare two Integers based on their absolute values

```java
Comparator<Integer> abscompare = Comparator.comparing(x -> Math.abs(x));

BinaryOperator<Integer> bigint =  BinaryOperator.maxBy(abscompare);

BinaryOperator<Integer> smallint =BinaryOperator.minBy(abscompare);

System.out.println(bigint.apply(2,-5));
System.out.println(smallint.apply(2,-5));
```
