# Java

# Base package

Package `java.lang` provides classes that are fundamental to the design of the Java programming language. For instance it includes base types as `Intger`, `Float`, `String`, etc.

Official [reference at Oracle](https://docs.oracle.com/javase/8/docs/api/index.html).


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