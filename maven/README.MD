## Maven
Maven is a build automation tool used for Java projects

* [Various ways to build](#various-ways-to-build)
* [Export dependencies](#export-dependencies)
* [Import a WAR as  JAR](#import-a-war-as-a-jar)

### Various ways to build 
Normal build :

```
mvn clean package
```

Build with sources and javadoc with **maven-source-plugin** and **maven-javadoc-plugin**, with the `pom.xml` below :

```xml
<build>
   <pluginManagement>
      <plugins>
         <plugin>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>2.5.1</version>
            <configuration>
               <source>1.6</source>
               <target>1.6</target>
            </configuration>
         </plugin>
         <plugin>
            <artifactId>maven-javadoc-plugin</artifactId>
         </plugin>
         <plugin>
            <artifactId>maven-source-plugin</artifactId>
         </plugin>
      </plugins>
   </pluginManagement>
</build>
  ```

Run the following commands :

```
mvn javadoc:javadoc
```

```
mvn source:jar
```

### Export dependencies

#### To a list
```bash
mvn dependency:list
```
#### To CSV
The following command has only been tested on Windows.
```bash
mvn -o dependency:list | grep ":.*:.*:compile" | sed "s/\[INFO\]    \([^:]*\):\([^:]*\):jar:\([^:]*\):compile/\1;\2;\3/" | sort -u
```

### Import a WAR as a JAR

Sometimes, a web application can also be used as a library for other apps. In this case, you would like to import its classes as a dependency in another app. Some configuration is needed in `pom.xml` in the [maven WAR plugin](http://maven.apache.org/plugins/maven-war-plugin/war-mojo.html) to compile the classes separately.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<plugin>
   <artifactId>maven-war-plugin</artifactId>
   <version>${version}</version>
   <configuration>
      <packagingExcludes>META-INF/context.xml</packagingExcludes>
      <!-- Attach classes to be imported as a JAR -->
      <attachClasses>true</attachClasses>
   </configuration>
</plugin>
```

This will build a `projectname-classes.jar` package that contains all the classes.

It should be automatically found when importing the dependency through `pom.xml`.
