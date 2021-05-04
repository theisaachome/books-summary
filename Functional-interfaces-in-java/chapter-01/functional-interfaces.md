## Functional Interfaces

- [Interfaces in Java](#Interfaces-in-Java)
- [Interfaces in Java 8 and Java 9](#Interfaces-in-Java-8-and-Java-9)
- [Functional Interfaces Defined](#Functional-Interfaces-Defined)
- [Providing Default Methods in Functional Interfaces](#Providing-Default-Methods-in-Functional-Interfaces)

---

### Interfaces in Java

- An interface specifies one or more methods.

- a contract which must be honored by all implementing classes.

#### example

```java
interface I1
{
    void method1();
    String method2(String x);
}

```

- class that implements an interface must provide
  - implementations for all the methods declared in the interface

```java
class C1 implements I1
{
    @Override
    public void method1() {}
    @Override
    public String method2(String x) { return x; }
}

```

---

### Interfaces in Java 8 and Java 9

- Interfaces are in Java 8 and 9 :

  - allowed to have static and default methods.

  - static method can be called without creation of an object.

  - A default method is an implementation provided by the interface

  - Not need to be overridden by an implementing class.

#### example

```java
interface I2
{
    String s = "I2";
    static void method1()
    {
        System.out.println(s);
    }
    default String method2(String x)
    {
return s + x; }
}
```

```java
class C2 implements I2
{
@Override
    public String method2(String x) { return x; }
}
class C3 implements I2 {}
class TestI2
{
    public static void main(String[] args)
    {
        I2.method1();
        I2 objC2 = new C2();
        I2 objC3 = new C3();
        System.out.println(objC2.method2("Hello"));
        System.out.println(objC3.method2("World"));
} }

```

- In Java 9,
  - interfaces were allowed to have private methods.
  - Private methods are useful to call from default methods.

```java
interface I3{
    private int getNumber() { return (new Random()).nextInt(100); }
    default String M1(String s){
        return s + getNumber();
    }
 }

```

```java
class C4 implements I3
{
}
class TestI3
{
    public static void main(String[] args)
    {
        I3 objC4 = new C4();
        System.out.println(objC4.M1("Hello"));
    }
}
```

- In Java 9,
- private static methods.
- Since the static methods of an interface can be called without creation of an implementing object,
- these methods can only be called by public static methods defined in the interface.

#### example

```java
interface I4
{
    private static String getPrefix(String p){
        return p.equals("male")? "Mr. " : "Ms. ";
    }
    public static String getName(String n, String p){
        return getPrefix(p) + n;
    }
}
```

```java
class TestI4
{
    public static void main(String[] args){
        System.out.println(I4.getName("Smith", "female"));
        System.out.println(I4.getName("Jones", "male"));
    }
}
```

---

### Functional Interfaces Defined

- An interface with a single abstract method

- The @FunctionalInterface
  - annotation instructs that the interface has only one abstract method.

```java
@FunctionalInterface
interface StringProcessor{
       String process(String x);
}
```

### Providing Default Methods in Functional Interfaces

- A functional interface can provide default methods.

- An implementing class can use the default methods or

- provide its own versions.

---

### Providing Static Methods in Functional Interfaces

- A functional interface can also have static methods.
- Static methods are useful
  - to define helper methods that
  - are not meant to be overridden by implementing classes.

### Generic Functional Interfaces

- methods that take two arguments of a type and returns a result of the same type, but both the type and the operation performed can vary.
