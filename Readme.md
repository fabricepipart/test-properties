# Maven, system path, properties and multi environment

The purpose of this project is to show a minimal working example of a problem we encountered.
We have dependencies on artefacts that defined a system path for their own dependencies. This was done using properties that by default refer to a Windows path.

To build on Linux, we need to override this property. But despite that, we get the following message from Maven during build:

```
The POM for <groupId>:<artifatctId>:jar:<version> is invalid, transitive dependencies (if any) will not be available
```

## Build the dependency


### On Windows

```
cd 1-installme/
mvn clean install
```

### On Linux

```
cd 1-installme/
mvn clean install -Dcom.sun.tools.path=/usr/lib/jvm/java-8-openjdk-amd64/lib/tools.jar
```

## Build the sample

### On Windows

```
cd ../2-buildme/
mvn dependency:tree
```

You should get:

```
[INFO] --- maven-dependency-plugin:2.1:tree (default-cli) @ buildme ---
[INFO] test.properties:buildme:pom:1.0-SNAPSHOT
[INFO] \- test.properties.installme:module:pom:1.0-SNAPSHOT:compile
```

### On Linux

```
cd ../2-buildme/
mvn dependency:tree -Dcom.sun.tools.path=/usr/lib/jvm/java-8-openjdk-amd64/lib/tools.jar
```

You should get:

```
[INFO] --- maven-dependency-plugin:2.1:tree (default-cli) @ buildme ---
[WARNING] Invalid POM for test.properties.installme:module:pom:1.0-SNAPSHOT, transitive dependencies (if any) will not be available, enable debug logging for more details
[INFO] test.properties:buildme:pom:1.0-SNAPSHOT
[INFO] \- test.properties.installme:module:pom:1.0-SNAPSHOT:compile
```
