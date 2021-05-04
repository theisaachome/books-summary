## Lambda Expressions

[Table of Contents](#table-of-contents)

- [Lambda Expressions Defined](#Lambda-Expressions-Defined)
- [Represent Functional Interfaces](#Represent-Functional-Interfaces)

---

### Lambda Expressions Defined

- used to represent a statement.
- From Java 8, They were introduced functional programming.

```java
 lambda_argument_list -> lambda_body

 () -> {}
```

---

### Represent Functional Interfaces

- to represent functional interfaces

```java
@FunctionalInterface
interface StringProcessor
{
    String process(String x);
}
```

- implements functional interface StringProcessor:

```java
StringProcessor lambdaSP = x -> x;
System.out.println(lambdaSP.process("Hello"));
```

- a lambda has return type void, the body of the lambda must be an expression that results in a void type.

```java
@FunctionalInterface
interface FIVoid
{
    void method1(int i);
}
// implements
FIVoid LambdaVoid = x -> System.out.println(x);
LambdaVoid.method1(5);
```

---

### The Scope of a Lambda Expression

- Any variables that are in scope at the definition of a lambda expression are in scope for the functional method it represents.
- This include fields and final or effectively final local variables

```java
class TestLambdaScope{
    private static int myField = 2;
    public static void main(String[] args){

        int myLocal = 7;
        FIVoid lambdaVoid =
        x -> System.out.println(x + myField + myLocal);
        lambdaVoid.method1(5);
    }
}
```

---

### Lambda Argument List Variations

- a single argument and the type of the argument can be inferred
- the argument may be specified without parentheses.

```java
 x -> x
 // or A type may be provided
 (String x) -> x
```

---

```java
class A { int i; }

@FunctionalInterface
interface interfaceZ<T>{
    int m(T t);
}

class Inference{
    static <T> void m2(interfaceZ<T> arg) {}
    public static void main(String[] args){
        //m2(x -> x.i); // ERROR
        // the type of lambda argument x fixes the problem.
        m2( (A x) -> x.i); // OK
    }
}
```

- several arguments,
  - each must be separated by a comma, and
  - the list must be enclosed by parentheses.

```java
    (x, y) -> x + y
```

#### example

```java

@FunctionalInterface
interface FI2{
    String method1(String x, int y);
}
```

- infer the types of the two lambda arguments,

```java
FI2 fi2Lambda1 = (a, b) -> a + Integer.toString(b);

//However, types may be provided if desired.
FI2 fi2Lambda2 = (String c, int d) -> c + Integer.toString(d+1);

System.out.println(fi2Lambda1.method1("Hello",1));
System.out.println(fi2Lambda2.method1("Hello",1));
```

- no arguments, it must be specified by an empty set of parentheses.

```java
() -> 5
```

#### example

```java
@FunctionalInterface
interface FI3{
    Integer method1();
}

FI3 fi3Lambda1 = () -> 99;
System.out.println(fi3Lambda1.method1());
```

---

### Lambda Bodies in Block Form

- Enclosing the body of a lambda in curly braces places it in block form.

#### **Expression Form:**

```java
 FIVoid lambda1 = x -> System.out.println(x);
```

#### **BLock Form**

```java
FIVoid lambda2 = x -> { System.out.println(x); };

FIVoid lambdaBlock = x -> {
    x++;
    System.out.println(x);
};
lambdaBlock.method1(5);

```

- block form returns a value,

```java
StringProcessor spBlock = x -> {
    System.out.print(x + " ");
    return x + x;
}; System.out.println(spBlock.process("Hello"));
```

- A lambda expression with exception handling.

```java
int[] array = {11,12,13,14,15};
FIVoid lambdaSubscript = x -> {
    try {
        int value = array[x];
        System.out.println(value);
    } catch (ArrayIndexOutOfBoundsException e) {
        System.out.println("Index " + x + " is out of bounds.");
} };
lambdaSubscript.method1(5);
lambdaSubscript.method1(4);
```

---

### Limitations of Lambda Expressions
