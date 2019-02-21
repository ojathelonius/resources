## Maven
Maven is a build automation tool used for Java projects

* [Build](#build)
* [Export dependencies](#export-dependencies)

### Build 
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
