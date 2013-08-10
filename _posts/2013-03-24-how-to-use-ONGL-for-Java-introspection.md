---
layout: post
title: How to use OGNL (Object Graph Navigation Library) for Java introspection
summary: Does Java reflection and introspection scare you? Learn how to use OGNL, an easy to use library to tap into the power of reflection in Java.
---

Java, like other modern programming languages, lets you create new classes, inspect objects, and get and set values at runtime. This is a great feature of the language but learning how to do this can be daunting when you rarely use this feature of the language. In addition, to do this from scratch can take quite a bit of boilerplate coding as well as thorough testing.

In cases like these it is best to use open source libraries like OGNL. This library can help you to:

  * get and set values
  * call methods (with parameters and get the return results)
  * create new objects

OGNL is quite an easy library to use. Once you have included the library into your project (manually, or using Maven) then there are some simple ways for you to get and set values as well as call methods with parameters. Lets look at how to do all of these things with some helper methods you can use in your own classes:

## Getting a value using OGNL
OGNL makes getting a value from an object very easy. You just pass in the object and tell it what property you want it to get. The object must have public property getters and setters for this to work.

```java
public static Object ognlGet(Object rootClass, String propertyName) {

    try {
        return Ognl.getValue(propertyName, rootClass);
    } catch (OgnlException e) {
        log.error(String.format("Could not get property on object '%s'", propertyName), e);
    }
    return null;
}
```

Thats all well and good, but how do you use this? Lets say we have an `Order` object and a `Person` object. This is a very simple example that you might not use OGNL (or reflection) for, but it demonstrates the concept.

```java
class Order {
	int orderNumber = 0;
	Person person = new Person();

	/* Getters and setters here ... */
}

class Person {
	String firstName;
	String lastName;

	/* Getters and Setters here ... */
}
```

For example, a valid `propertyName` expression might be “person.firstName” on the `Order` object. This would get the `firstName` value on the `Person` object.

```java
String firstName = (String)ognlGet( order, “person.firstName”);
```

As you can see, you are not limited to calling just property methods on the object you passed in. You can also get properties from sub objects.

## Setting a value using OGNL
Setting a property is just like getting a property but we pass in the value we want to set. There is no return value on setters.

```java
public static void ognlSet(Object rootClass, String propertyName, Object value) {
    try {
        Ognl.setValue(propertyName, rootClass, value);
    } catch (OgnlException e) {
        log.error(String.format("Could not set property on object '%s'", propertyName), e);
    }
}
```

## Calling a method using OGNL
Calling a method with parameters is a little more advanced, but it might be something you would need to do. In this case we need the object to apply the expression on, the OGNL expression to the method name, and an array of arguments to pass to the method.

We first want to create a context using the root object, once we have the OGNL context we can use the static `callMethod` function on the `OgnlRuntime` class passing in the context, object to operate on, the ognl expression, and the array of arguments.

```java
public static Object ognlCallMethod(String ognlMethodName, Object rootClass, Object[] args) {
    try {
        OgnlContext ognlContext = (OgnlContext) Ognl.createDefaultContext(rootClass);
        return OgnlRuntime.callMethod(ognlContext, rootClass, ognlMethodName, args);
    } catch (OgnlException e) {
        log.error("Could not call " + ognlMethodName, e);
    }
    return null;
}
```

## Calling a constructor with arguments using OGNL
Creating an object is simply a matter of calling the constructor method. You can pass in any arguments and OGNL will find the correctly matching constructor it needs to call. This makes creating new objects with constructor arguments very easy.

```java
public static Object ognlCreateClass(String className, Object[] args) {
    try {
        OgnlContext ognlContext = (OgnlContext)Ognl.createDefaultContext(null);
        return OgnlRuntime.callConstructor(ognlContext, className, args);
    } catch (OgnlException e) {
        log.error(String.format("Could create new object for class %s", className), e);
    }
    return null;
}
```

## Creating a new Object using OGNL
Since we already know how to call a constructor on a class we can create a basic object simply by calling the default (no-argument) constructor. In this case we just pass in an empty object array.

```java
public static Object ognlCreateClass(String className) {
    return ognlCreateClass(className, new Object[]{});
}
```

## Summary
OGNL is an easy and powerful library to use, especially in systems where need to use a lot of the reflection capabilities of Java. Not only can you get and set properties, but you can create objects and call methods with parameters.

