# **`Functions in Functional Programming`**

- [Higher Order Functions](#higher-order-functions)

  - [Returning a function](#returning-a-function)

- [First-class functions](#first-class-functions)
- [The Pure function](#the-pure-function)

---

---

## **`Higher Order Functions`**

A Higher order function takes a function as argument or returns a function as a value.

### **`Example`**

**`The higher-order function using an imperative.`**

```java
List<String> names = Arrays.asList("Aung Aung","Naw Naw","Aung La");
for (String s : names) {
	System.out.println(s.toLowerCase());
}
```

**`The higher-order function using a functional approach.`**

- create a method

```java
public  String processString(
    Function<String, String> operation,String target) {
    return operation.apply(target);
}
```

```java
names.forEach(
    s->System.out.println(
    processString(t->t.toLowerCase(), s)
));
```

**`Method References`**

```java
names.forEach(
    s->System.out.println(
	processString(String::toLowerCase, s)
));
```

### Why functional

- flexible
- reusable

Example to perform upperCase

```java
names.forEach(
    s->System.out.println(
    processString(t->t.toUpperCase(), s)
));
```

## More Example

```java
List<String> numString = Arrays.asList("10","20","30","40","50");
List<Integer> numbers = new ArrayList<Integer>();
Function<List<String>, List<Integer>> numFunction = s->{
	s.stream()
	.forEach(
		n->numbers.add(Integer.valueOf(n))
	);
	return numbers;
};
System.out.println(numFunction.apply(numString));
```

### **The real power comes passing these functions to other function.**

```java
Arrays.asList(numString)
    .stream()
    .map(numFunction)
    .forEach(s->System.out.println(s));
```

---

## **`Returning a function`**

- When a value is returned from a function or method, it is intended to be used elsewhere in the application.
- Sometimes, the return value is used to determine how subsequent computations should proceed.

### employee type

```java
enum EMPLOYEE_TYPE {
	Hourly,
	Salary,
	Sales
}
```

### functional method that return by

```java
public  static BiFunction<Integer, Float, Float> calculatePay(EMPLOYEE_TYPE emType){
	switch (emType) {
	case Hourly :return (hour,payRate) -> hour * payRate;
	case Salary : return (hour,payRate)->40 * payRate;
	case Sales:return (hour,payRate)-> 500f * 0.15f * payRate;
	default:
		return null;
	}
	}
```

### It can be invoked as :

```java
int [] hoursOfWorks = {8,12,8,6,6,5,6,0};
    int totalHoursOfWorks= Arrays.stream(hoursOfWorks).sum();
    var result= EmployeeService.calculatePay(EMPLOYEE_TYPE.Salary)
    		.apply(totalHoursOfWorks, 15.0f);
    System.out.println(result);
```

---

## **`First-class functions`**

- Assign a function to a variable (lambda expression).

```java
Function<String,String> toLowerFunction= t -> t.toLowerCase();
```

- Assign a lambda expression to this variable.

```java
Consumer<String> consumer;
consumer = s -> System.out.println(toLowerFunction.apply(s));
```

- use the `consumer` variable as the argument of the forEach method:

```java
list.forEach(consumer);
```

---

## **`The Pure function`**

- A function that has no side effects.
  - the function does not modify nonlocal variables and does not perform I/O.

A pure function with no side effects

```java
public class SimpleMath {
    public static int square(int x) {
		return x * x;
 	}
}
```


Its use is shown here and will display the result, 25:
```java
System.out.println(SimpleMath.square(5));
```

An equivalent lambda expression is shown here:
```java
Function<Integer,Integer> squareFunction = x -> x*x;
System.out.println(squareFunction.apply(5));
```


### The advantages of pure functions include the following:
1. They can be invoked repeatedly producing the same results

	- Using the same arguments will produce the same results.

	- the operation does not depend on other external values, re-executing the code with the same arguments will return the same results.

    ```java
	System.out.println(SimpleMath.square(5));
	```
2. There are no dependencies between functions that impact the order they can be executed.

	- When dependencies between functions are eliminated, then more flexibility in the order of execution is possible.

	```java
	 BiFunction<Integer, Double, Double> computeHourly =
           (hours, rate) -> hours * rate;

    Function<Double, Double> computeSalary = rate -> rate * 40.0;
    BiFunction<Double, Double, Double> computeSales =
           (rate, commission) -> rate * 40.0 + commission;
	```
	- These functions can be executed, and their results are assigned to variables.

	```java
	 double hourlyPay = computeHourly.apply(35, 12.75);
     double salaryPay = computeSalary.apply(25.35);
     double salesPay = computeSales.apply(8.75, 2500.0);
	```
	- These are pure functions as they do not use external values to perform their computations.

	```java
	System.out.println(computeHourly.apply(35, 12.75));
    System.out.println(computeSalary.apply(25.35));
    System.out.println(computeSales.apply(8.75, 2500.0));
	```
3. They support lazy evaluation
	- When this code sequence is executed with an `hourly value of false`, there is no need to execute the computeHourly function since it is not used.

	```java
       double total = 0.0;
       boolean hourly = ...;
       if(hourly) {
           total = hourlyPay;
       } else {
           total = salaryPay + salesPay;
       }
       System.out.println(total);
	```
	- The runtime system could conceivably choose not to execute any of the lambda expressions until it knows which one is actually used.
---

## **`Closure in Java`**

- A closure is a function that uses the context within which it was defined.

- By context, we mean the variables within its scope.

- This sometimes is referred to as variable capture.