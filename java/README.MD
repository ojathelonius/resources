## Java

* [Read resources from a static context](#read-resources-from-a-static-context)

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


