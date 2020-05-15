## Maven
Maven is a build automation tool used for Java projects

* [Various ways to build](#various-ways-to-build)
* [Export dependencies](#export-dependencies)
* [Import a WAR as  JAR](#import-a-war-as-a-jar)
* [Extract resources from a JAR](#extract-resources-from-a-jar)
* [Define source directory](#define-source-directory)
* [Include dependencies in JAR](#include-dependencies-in-jar)
* [Hotfix and release with jgitflow without pushing](#hotfix-and-release-with-jgitflow-without-pushing)

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

It should be automatically found when importing the dependency through `pom.xml`, using the following dependency configuration :

```xml
<dependency>
   ...
   <classifier>classes</classifier>
   ...
</dependency>
```

**Note**: there is an [Eclipse bug](https://bugs.eclipse.org/bugs/show_bug.cgi?id=502349) that prevents it from finding updated packaged dependencies such as above. Instead, run :

```bash
mvn clean install -U
```

### Extract resources from a JAR

This can be useful to include default config files in a WAR, from a given JAR.

For this purpose we can use the [Jetspeed unpack plugin](https://portals.apache.org/jetspeed-2/buildguide/jetspeed-unpack-plugin.html).

```xml
<plugin>
	<groupId>org.apache.portals.jetspeed-2</groupId>
	<artifactId>jetspeed-unpack-maven-plugin</artifactId>
	<executions>
		<execution>
			<id>unpack-jetspeed-properties</id>
			<goals>
				<goal>unpack</goal>
			</goals>
			<phase>generate-resources</phase>
			<configuration>
				<unpack>
					<artifact>myartifact:jar</artifact>
					<targetDirectory>${project.build.directory}</targetDirectory>
					<resources combine.children="append">
						<resource>
							<path>path/to/classes</path>
							<destination>/files</destination>
							<flat>true</flat>
							<include>**/*.xml</include>
						</resource>
					</resources>
				</unpack>
			</configuration>
		</execution>
	</executions>
	<dependencies>
		<dependency>
			<groupId>my.groupid</groupId>
			<artifactId>myartifact</artifactId>
			<version>0.0.0</version>
		</dependency>
	</dependencies>
</plugin>
```

Files can then be copied into different folders by including them in the build : 

```xml
<build>
   <resources>
      <filtering>true</filtering>
         <directory>${project.build.directory}/files</directory>
         <includes>
            <include>**/*.xml</include>
         </includes>
   </resources>
</build>
```

Remaining, unused files can be deleted from the WAR using :

```xml
<packagingExcludes>unused.xml</packagingExcludes>
```

### Define source directory

If you have a single source directory, use :

```xml
<build>
    <sourceDirectory>../src/main/java</sourceDirectory>
</build>
```

If you have several, use `build-helper-maven-plugin` :

```xml
<plugin>
	<groupId>org.codehaus.mojo</groupId>
	<artifactId>build-helper-maven-plugin</artifactId>
	<executions>
		<execution>
			<id>add-source</id>
			<phase>generate-sources</phase>
			<goals>
				<goal>add-source</goal>
			</goals>
			<configuration>
				<sources>
					<source>${basedir}/src/main/gen/</source>
				</sources>
			</configuration>
		</execution>
	</executions>
</plugin>
```

### Include dependencies in JAR

```xml
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-assembly-plugin</artifactId>
	<version>2.6</version>
	<executions>
		<execution>
			<phase>package</phase>
			<goals>
				<goal>single</goal>
			</goals>
			<configuration>
				<archive>
					<manifest>
						<addClasspath>true</addClasspath>
						<classpathPrefix>lib/</classpathPrefix>
						<mainClass>com.my.class.Class</mainClass>
					</manifest>
				</archive>
				<descriptorRefs>
					<descriptorRef>jar-with-dependencies</descriptorRef>
				</descriptorRefs>
			</configuration>
		</execution>
	</executions>
</plugin>
```

### Hotfix and release with jgitflow without pushing

By default, jgitflow pushes all branches and tags and can lead to half-assed releases due to build or merge errors. Prevent this with :

#### Hotfix

```bash
mvn jgitflow:hotfix-start -Djgitflow.allowSnapshots=false -DlocalOnly=true -DpushHotfixes=false -Djgitflow.pushHotfixes=false -DnoHotfixBuild=true -DnoReleaseMerge=false -DnoHotfixMerge=false

mvn jgitflow:hotfix-finish -Djgitflow.allowSnapshots=false -Dmaven.test.skip=true -DnoDeploy=false -DpushHotfixes=false -Djgitflow.pushHotfixes=false -Djgitflow.localOnly=true -DlocalOnly=true -DnoHotfixBuild=true -DnoHotfixMerge=false
```


#### Release

```bash
mvn jgitflow:release-start -Djgitflow.allowSnapshots=true -DlocalOnly=true -DpushReleases=false -Djgitflow.pushReleases=false

mvn jgitflow:release-finish -Djgitflow.allowSnapshots=true -Dmaven.test.skip=true -DnoDeploy=false -DpushReleases=false -Djgitflow.pushReleases=false -Djgitflow.localOnly=true -DlocalOnly=true -DnoReleaseBuild=true
```

