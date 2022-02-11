# **`Functions in Functional Programming`**

- ### [Higher Order Functions](#higher-order-functions)
- ### [Return Function](#return-function)

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
- Example to perform upperCase

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
## **`Return Function`**

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