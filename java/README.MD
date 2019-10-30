## Java

* [Read resources from a static context](#read-resources-from-a-static-context)
* [Insert Java variables in a template string](#insert-java-variables-in-a-template-string)

### Read resources from a static context

Obviously we can't execute the code below from a static context :
```java
InputStream is = getClass().getClassLoader().getResourceAsStream("my_resource.properties")
// cannot make a static reference...
```
I've found that the following code found online **did not** work either :
```java
InputStream is = ClassLoader.class.getResourceAsStream("my_resource.properties")
// returns null
```

However this worked :
```java
InputStream is = Thread.currentThread().getContextClassLoader().getResourceAsStream("my_resource.properties")
```

### Insert Java variables in a template string

Say you want to insert some variables in a template string. You can write an inline template, e.g. :
```java
private String myFinalStr = "let myElement = document.getElementById(" + myId + ")";
```

It might work for short templates, but gets unmaintainable for longer strings. Instead use [StringSubstitutor](https://commons.apache.org/proper/commons-text/apidocs/org/apache/commons/text/StringSubstitutor.html):

Create a `myTemplate.js` file in `src/main/resources` and import it per the previous paragraph, then replace the variables with proper values with `sub.replace(templateString)`.

