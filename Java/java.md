# Java

- [Java](#java)
  - [Base package](#base-package)
  - [Compilation](#compilation)
  - [Java Archives](#java-archives)
    - [Create a JAR file](#create-a-jar-file)
    - [Read the content of a JAR file](#read-the-content-of-a-jar-file)
- [Glossary](#glossary)
  - [Anonymous Inner Class in Java](#anonymous-inner-class-in-java)

## Base package

Package `java.lang` provides classes that are fundamental to the design of the Java programming language. For instance it includes base types as `Intger`, `Float`, `String`, etc.

Official [reference at Oracle](https://docs.oracle.com/javase/8/docs/api/index.html).

## Compilation

```
javac -d .\out .\src\com\my-domain\my-project\*.java
```

## Java Archives

### Create a JAR file
Suppose the compilation output has been redirected to directory `out\` abd the manifest-change file `MANIFEST.MF` is in the root path.
```
src\
|_ com\my-domain\my-app\
   |- *.java
out\
|_ com\my-domain\my-app\
   |- *.class
MANIFEST.MF
```
The content of `MANIFEST.MF` is:
```ini
Manifest-Version: 1.0
Main-Class: com.my-domain.my-app.App
```

Change to `out\` then create the JAR with the `jar` command:
```
> jar cfm ..\my-app.jar ..\MANIFEST.MF com\my-domain\my-app\*.class
```

Here the main class is `App` in `App.java`. This class must have an *entry point* that is the method with signature `public static void main(String[] args)`.

### Read the content of a JAR file

```
> jar tf archive.jar
```


# Glossary

## Anonymous Inner Class in Java

It is an [inner class](https://www.geeksforgeeks.org/nested-classes-java/) without a name and for which only a single object is created. An **anonymous inner class** can be useful when making an instance of an object with certain “extras” such as overloading methods of a class or interface, without having to actually subclass a class.

Anonymous inner classes are useful in writing implementation classes for listener interfaces in graphics programming.

Anonymous inner class are mainly created in two ways:


* Class (may be abstract or concrete)
* Interface

Syntax: The syntax of an anonymous class expression is like the invocation of a constructor, except that there is a class definition contained in a block of code.

```java
// Test can be interface,abstract/concrete class
Test t = new Test()
{
   // data members and methods
   public void test_method()
   {
      ........
      ........
    }
};
```