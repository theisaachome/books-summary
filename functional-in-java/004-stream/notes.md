# Streams

### Table of Contents
- [Streams](#streams)
- [Intermediate and terminal methods](#intermediate-and-terminal-methods)
- [Intermediate methods](#intermediate-methods)
- [Creating Streams](#creating-streams)
    - [Collection interface default method](#collection-interface-default-method)
---
## **`Streams`**

A sequence of elements processed by a series of methods.


### **`The Stream class and its Usage`**

The Stream class in java primarily support the concept of Stream inn java.

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