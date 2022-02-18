# Streams

### Table of Contents
- [Streams](#streams)
- [Intermediate and terminal methods](#intermediate-and-terminal-methods)
- [Intermediate methods](#intermediate-methods)
- [Creating Streams](#creating-streams)
	- [Fixed length](#fixed-length-streams)
    - [Collection interface default method](#collection-interface-default-method)
	- [Infinites Stream](#infinite-stream)
	- [Iterate Method](#iterate-method)
	- [Generate Method](#generate-method)
- [Stream class methods](#stream-class-methods)
	- [Filter Method](#filter-methods)
		- [filter( )](#the-filter-method)
		- [skip( )](#the-skip-method)
	- [Sorting streams](#sorting-streams)
	- [Mapping Methods](#mapping-methods)
	- [map method](#map-method)
	- [flatMap method](#flatmap-method)

---
## **`Streams`**

A sequence of elements processed by a series of methods.


### **`The Stream class and its Usage`**

The Stream class in java primarily support the concept of Stream in java.

Some specialized classes:

- DoubleStream
- IntStream
- LongStream

The Collection interface support the creation of Stream.

A Stream supports a finite or an infinites sequence of elements.

The methods of stream can classified in a number of ways such as :
- Mapping
- filtering
- sorting types method.


Example
```java
int [] inums =  {3,6,8,8,4,6,3,3,5,6,9,4,3,6};
int totalSum = Arrays.stream(inums)
		.distinct()
		.sum();
System.out.println(totalSum);
```
- A Source data is converted into a stream.
- A series of methods are executed against the stream followed by a terminating operation.

- One intermediate method `distinct().`
- One terminal method `sum().`

---


## **`Intermediate and terminal methods`**


### **`Intermediate methods`**

The methods that will perform some operation on the elements of the stream and then return the stream are called intermediate method.

The intermedidate methods always return a stream and do not actually modify the stream.

But create new stream to return.

Example
```
distinct()
```


### **`Terminal methods`**

There are also terminal methods that do not return the stream and effectively end the processing sequence.

The process of stream start when terminal operation start and stop when the terminal method completes.

Example:
```
sum()
```


Once a Stream has been used, it can not be used again.

Example
```java
int [] inums =  {3,6,8,8,4,6,3,3,5,6,9,4,3,6};
int totalSum = Arrays.stream(inums)
		.distinct()
		.sum();
int total = Arrays.stream(inums)
        .sum();
```

---

## **`Creating Streams`**

A Stream can be of fixed or infinite length.


### **`Fixed length streams`**

Stream with Integers.
```java
int [] inums =  {3,6,8,8,4,6,3,3,5,6,9,4,3,6};
IntStream iStream = Arrays.stream(inums);
```

Stream with Objects

```java
Employee [] eArray = {
		new Employee("Aung Aung", "aungaung@gmail", "yangon"),
		new Employee("Maw maw", "mawimawi@gmail", "Mdy"),
		new Employee("Than zin Oo", "thazinoo@gmail", "Thai"),
		new Employee("Lay Phyu", "layphyu@gmail", "USA"),
		new Employee("Min Maw Thant", "minmawthant@gmail", "yangon"),
	};

Stream<Employee> eStream= Arrays.stream(eArray);
eStream.forEach(e->System.out.println(e.getName()));
```

---

## **`Collection interface default method`**

The stream method also exists as a default method in the Collection interface.

The method can be used with classes such as ArrayList, Hashset, and TreeSet. 

```java
List<String> cities = new ArrayList<String>();
cities.add("London");
cities.add("NewYork");
cities.add("Liverpool");
cities.add("Aston Villa");
cities.stream().forEach(System.out::println);
```

---
## **`Infinite Stream`**

Some stream may be conceptually unlimited in length.
A vidoe or audio length might be indeterminate length.

These types of Streams can be represented by the Stream class.


There are two Stream methods used to create infinite streams:

- The iterate method
- The generate method


## Iterate Method

```java
 static <T> Stream<T> iterate(T seed, UnaryOperator<T> f)
```
- The seed is the initial value used.
- The UnaryOperator instance, frequently implemented using a lambda expression, uses the seed value to create a new value.


Example
```java
IntStream.iterate(0, n->n+1)
.limit(10)
.forEach(System.out::println);;
```

The `limit` method provides simple way of controlling the number of elements produced by a stream.

Example 2
```java
	String [] subjects = {"Cat","Dog","monkey","bat"};
	String [] verbs = {"chased","ate","lost","swatted"};
	String [] object = {"ball","rat","donughnut","tamale"};
	Random random = new Random();
	
	Stream.iterate("", 
			m -> subjects[random.nextInt(subjects.length)]
			+ " " + verbs[random.nextInt(3)]
			+ " " + object[random.nextInt(object.length)])
	.limit(5)
	.forEach(System.out::println);
```

---

## **`Generate Method`**


The generate method use a supplier interface as its argument.

```java
static <T>Stream<T> generate(Supplier<T> s);
```

Example
```java
Supplier intSupplier = () -> 0;
Stream.generate(intSupplier)
        .limit(5)
        .forEach(System.out::println);
```

Example
- Generate a stream based on the previoud value.
```java
static int num =0;
public static int nextInt() {
	return num++;
}	
Stream.generate(()->nextInt())
.limit(5)
.forEach(System.out::println);

```

Use  method reference to generate random method.

```java
Supplier<Double> doubleRandom = Math::random;
Stream.generate(doubleRandom)
.limit(5)
.forEach(System.out::println);
	
```

---
## **`Stream class methods`**

Stream class methods are useful for:
- Tranforming
- Filtering
- Reducing elements.


## **`Filter Methods`**

Filtering involves iterating over a sequence and eliminating elements that are no longer needed.

Three types of filter methods:

-  **`filter`**: This leaves out elements
- **`distinct`**: This leaves out duplicates
- **`skip`**: This skips over elements

Each of theses removes elements from the streams. Actually elements are not removed, but rather select certain elements for further processing.

---

## **`the filter method`**

- The `filter` method takes an argument that implements the Predicate interface. 

- This argument must implement the interface's test method.

- It is passed a single value and returns a Boolean result.

Example:
```java
List<Employee> empList = Employee.getList();
		empList.stream()
		.filter((e)->e.getAge()>20)
		.forEach(System.out::println);
```

## **`the skip method`**

An intermediate operation that discards the first n elements of a stream.

The n parameter can't be negative, and if it's higher than the size of the stream, skip() returns an empty stream.



Example:
```java
Stream.of(1,2,3,4,5,6,7,8,9,10)
		.filter(n-> n%2==0)
		.skip(3)
		.forEach(System.out::println);
// since we skip first 3 whihc are filtered out.
// 8 10 
```

The skip method acts similar to the filter method except that the number of elements, to ignore, are specified by a long argument. 


They can be used to represent payroll data, grades, a package's dimensions, and any number of other uses.

Example
```java
int[] numbers = {3, 6, 8, 8, 4, 6, 3, 3, 5, 6, 9, 4, 3, 6};
IntStream stream = Arrays.stream(numbers);
```

The `IntSummaryStatistics` class serves
to collect and generate these types of statistics.

```java

 IntSummaryStatistics states = stream
 		.skip(5).summaryStatistics();
 System.out.println("Average : " + states.getAverage());
 System.out.println("Count : " + states.getCount());
 System.out.println("Min : " + states.getMin());
 System.out.println("Max : " + states.getMax());
 System.out.println("Sum : " + states.getSum());
```

---

## **`Sorting streams`**

Sorting is a very common and useful technique.

The overload sorted method is used in stream.

Example:
```java
int [] inums = {3,3,3,6,6,4,5,9,8,10,23,12};
Arrays.stream(inums)
 .sorted()
 .forEach(System.out::println);
```

### Sort with lambda Expression

```java
Arrays.asList("9", "A", "Z", "1", "B", "Y", "4", "a", "c")
 .stream()
 .sorted((o1,o2)->o1.compareTo(o2))
 .forEach(System.out::println);;
```

Sort a List with `Comparator.reverseOrder()`

```java
List<String> lists =
Arrays.asList("9", "A", "Z", "1", "B", "Y", "4", "a", "c")
		.stream()
		.sorted(Comparator.reverseOrder())
		.collect(Collectors.toList());
	lists.forEach(System.out::println);
```

Sort List Object
```java
emplist.stream()
		.sorted(Comparator.comparingInt(Employee::getAge))
		.forEach(System.out::println);


// .sorted(Comparator.comparing(Employee::getName))
```

---

## **`Mapping Methods`**

Mapping operations transform an element in a stream.

The `map` and `flatMap` methods perform this type of operation.


The Map mehod transforms an element into a different value.

The FlatMap method transform an element but it is intended to collapse a series of streams into one stream.

## **`map method`**

```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper)
```

Example
```java
Employee.getList()
		.stream()
		.map(e->e.getName().toUpperCase())
		.forEach(System.out::println);
```
Transform the age
```java
Employee.getList()
		.stream()
		.map(e->e.getAge()*2)
		.collect(Collectors.toList())
		.forEach(System.out::println);
```
Reduce method
```java
int total =Employee.getList()
		.stream()
		.map(e->e.getAge()*2)
		.reduce(0,(r,s)-> r+s);
		System.out.println(total);
```

---

## **`flatMap Method`**

The flatMap method is useful when multiple streams need to be combined into one stream.

```java
<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper)
```


Example
```java
	// Creating a List of Strings
List<String> list = Arrays.asList("5.6", "7.4", "4", "1", "2.3");
 list.stream()
 .flatMap(s->Stream.of(s))
 .forEach(System.out::println);
```
Example
```java
    // Creating a list of Prime Numbers
List<Integer> PrimeNumbers = Arrays.asList(5, 7, 11,13);
  
// Creating a list of Odd Numbers
List<Integer> OddNumbers = Arrays.asList(1, 3, 5);
  
// Creating a list of Even Numbers
List<Integer> EvenNumbers = Arrays.asList(2, 4, 6, 8);
List<List<Integer>> listOfListofInts =
        Arrays.asList(PrimeNumbers, OddNumbers, EvenNumbers);
System.out.println("The Structure before flattening is : " +
                                          listOfListofInts);
  
// Using flatMap for transformating and flattening.
List<Integer> listofInts  = listOfListofInts.stream()
                            .flatMap(list -> list.stream())
                            .collect(Collectors.toList());
System.out.println("The Structure after flattening is : " +
                                                 listofInts);
```
