# **`Function Composition and Fluent Interfaces`**

### Table of Contents
- [Function composition in Java](#function-composition-in-java)

- [Creating Composition in Java 8](#creating-composition-in-java-8)

---

## **What is Functioon Composition ?**

Function composition is concerned with combining two functions to form a third one.

## **What is Fluent interfaces?**

Fluent interfaces implies a style of programming that flows easily and is more readable than the typical use
of methods.

It applies the output of one method directly to another method without using intermediate variables.

---

## **`Function composition in Java`**


### **`Introduction to function composition`**

Function composition is concerned with combining two functions into one.

```
c(x) = f (g(x));

f(x) = -x;
g(x)= 2 * x;

c(5) = f(g(x)) = f(2 * x) = - ( 2 * 5 )= -10;
```
- the g(x) function is called first.
- Its results are then used as input to the f(x) function.

## **`Creating Composition in Java 8`**


```java
// g(x)= 2 * x ;
Function<Double, Double> doubleFunction = x -> x *2;

// f(x) = -x;
Function<Double, Double> secondFunction = 
	doubleFunction.compose(x -> -x);

System.out.println(secondFunction.apply(5.0));
```

## Using the Function interface for function composition

```
f (x)=(2+x)∗3; 
g (x)= 2+(x∗3);
```

- In the first function, we add 2 to x and the multiply it by 3. 
- In the second function, we multiply it by 3 and then add 2.
 
- If we apply these functions using a value of 5, we get the following results, respectively:

```
f (5) = 21;
g(5)=17
```

```java
Function<Integer, Integer> baseFunction = t -> t + 2;
```

This is like saying, add 2 to the parameter and then multiply
it by 3.

```java
Function<Integer, Integer> afterFunction =
               baseFunction.andThen(t -> t * 3);

System.out.println(afterFunction.apply(5));
```

With compose method:

Here, we are saying multiply the parameter by 3 before you
add 2 to it.

```java
Function<Integer, Integer> beforeFunction =
               baseFunction.compose(t -> t * 3);
System.out.println(beforeFunction.apply(5));
```