# __Java Input & Output__


## I / O Basic
-  example of I/O  `print( ) `and `println( )`.
-  Java does provide `strong`, `flexible` support for I / O as it relates to files and networks.
- Java’s I/O system is cohesive and consistent.

## Streams

- Java programs perform `I/O` through streams.

- An abstraction that  produces or consumes information. 

- Linked to a physical device by the Java I/O system.

- The same I/O classes and methods can be applied to different types of devices. 

- An input stream can abstract many different kinds of input:
    - From a disk file
    - A keyboard, or 
    - A network socket.

---- 

## Byte Streams and Character Streams

- Two types of streams: 
    - byte and 
    - character. 

### Bye Streams :

- Handle input and output of bytes. 
- Byte streams are used, for  when reading or writing binary data.

### Charater Streams:

- Handle input and output of characters.

- They use Unicode and can be internationalized. 

- Character streams are more efficient than byte streams

--- 
## The Byte Stream Classes

- Defined With two class hierarchies.

- At the top are two abstract classes: 
    - InputStream and
    - OutputStream.

- Each of these abstract classes has several concrete subclasses that handle the differences among various devices, such as disk files, network connections, and even memory buffers.

## The Character Stream Classes
- Defined with two class hierarchies.
- At the top are two abstract classes: `Reader` and `Writer`. 
- These abstract classes handle Unicode character streams.
- Java has several concrete subclasses of each of these. The character stream classes in java.io.

## The Predefined Streams
- The `java.lang` package defines a class called `System`, which encapsulates several aspects of the run-time environment.
- System  contains three predefined stream variables:
    - `in, out, and err`.
    - These fields are declared as `public`, `static`, and `final` within System.

- `System.out` refers to the standard output stream. 
By default, this is the console. 
- `System.in` refers to standard input, the keyboard by default.
- `System.err` refers to the standard error stream, which also is the console by default.

---
## __Reading Console Input__
