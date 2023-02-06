# Jutil

When we work with integrated systems, sometimes we feel the need to send and
receive data in **JSON (Javascript Simple Object Notation)** format and unfortunately the
**Java** does not come with a standard library to convert Java objects
for String's in JSON format.

There are open-source libraries on the market that solve this problem. Although they provide
solutions to the conversion problem, create another one:

**Complexity** - either because they have
a complex API or by storing the information in an unwanted data structure like
for example the **Map**.

We usually don't want to know the implementation details, or worry about adapting the code to adapt to the library's API or even exercise control over objects
(resources) of the library. To convert to JSON, the ideal would be:

1. As it is a statically typed language, create the type (class) for the expected data.
2. Create an instance of the previously created class (typing).
3. Call a utility method that will return a JSON and pass the object as an argument.

Or to convert to Java:

1. As it is a statically typed language, create the type (class) for the expected data.
2. Get the JSON (string) and the Class object from the previously created typing.
3. Call a utility method that returns an object passing JSON and the class of the desired object as arguments.

A clean, intuitive and easy way to work with JSON and Java is **Jutil** **philosophy**!

### Requirements

**Jutil** is a **utility** and not an Artificial Intelligence (**AI**). For this reason some requirements must be met for it to work properly.

**Jutil** is based on **DTO** (Data Transfer Object) classes because they are used where JSON
is required. Serialized classes must follow the following **code writing convention**:

* The class must have at least one constructor with the same number of arguments as the supported attributes.
* The declaration order of the attributes must be the same as the parameters in the constructor.
* Transient or static attributes will not be serialized.

```java
public class Person {
     private final String name;
     private final String sex;

     public Person(String name, String sex) {
         this.name = name;
         this.sex = sex;
     }
    
     public String name() {
         return name;
     }

     public String sex() {
         return sex;
     }
}
```
Note that in the class above, we have the attributes declared in the order `name` and `sex`. We have a builder with
2 parameters and in the same order `name` and `sex`. The above class can be made even easier when we use
a `record`:

```java
public record Person(String name, String sex) {
}
```

We have the same result as the person class above, but this time without the risk of not following the convention of the
**Jutil**.

### API

**Jutil** brings us only two public API methods:

#### stringify

Receives the object to be converted into JSON. The type of this object must meet [requirements](#requirements).
```java
public class Test {
    
     public static void main(String[] args) {
         Person person = new Person("John Doe", "Male");
         String json = JSON.stringify(person);
         System.out.println(json);
     }
    
}
```

#### parse

It receives the JSON and the type of the object for which an instance is to be created. This type must meet the
[requirements](#requirements).

```java
import javax.util.JSON;

public class Test {

    public static void main(String[] args) {
        Person person = JSON.parse("{\"name\":\"John Doe\",\"sex\":\"Male\"}", Person.class);
        System.out.println(person); // if is record will print the content
    }

}
```