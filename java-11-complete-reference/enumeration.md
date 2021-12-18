# Enumerations

##  What is Enumeration in Java?
-  A list of named constants that define a new data type and its legal values. 
- An enumeration object can hold only a value that was declared in the list. 
- An enumeration can have constructors, methods, and instance variables.

```java 
```

## Enumeration Fundamentals
- An enumeration is created using the enum keyword.

```java
public enum Color{
    GREEN,RED, BLUE,
}
```
- The identifiers `GREEN, RED` are called `enumeration constants`. 

- Create a variable of that type enumeration.

```java
 Color green;
 // assign the constant type.
 green = Color.GREEN;
 system.out.println(green);
```