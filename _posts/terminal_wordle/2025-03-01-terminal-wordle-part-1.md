---
title: "Terminal Wordle Part 1"
date: 2025-03-01
categories: [games, terminal-wordle]
tags: [java, maven]
---

In this part the setup and intial project structure is discussed.

## Setup

To build the terminal wordle application, we first need to prerequisites installed in the system. The mentioned steps below are for a Linux based operating systems and disto being used is `Ubuntu`. The Text editor being used is vim, vs-code or eclipse ide as well. 

### Java

The application is developed using java programming language. So we need to install java first. The version being used here is version 17. To install java we enter the following command in the terminal application

``` bash
sudo apt install open-jdk-17
```

After this you can check where its installed or not using command `java --version`. Next we set the `JAVA_HOME` environment variable. First we need to find the path to location where the java files are installed first in order set the variable. The location of all the installed java version files can be found out using the command

``` bash
update-alternatives --config java
```

This command gives us option to set the java version and also along gives us the location's of the all the java executables installed in the system. The prompt will ask for selecting the java version to use but one can exit the prompt if it's already set to 17 by clicking `Ctrl + C`.

An example of the location output is `/usr/lib/jvm/java-17-openjdk-amd64/bin/java`. For the `JAVA_HOME` variable we need to location value part till `/usr/lib/jvm/java-17-openjdk-amd64`. We add a new entry in the `/etc/environment` file. This will require super user permissions

``` /etc/environment
JAVA_HOME="<java_path_for_system>"
```
Once the entry is added the file must be refreshed for the changes to reflect. To do this enter in the terminal 

``` bash
source /etc/environment
```
or one can close the terminal and again open it. Now to test whether `JAVA_HOME` is setup in the terminal enter

``` bash
echo $JAVA_HOME
```
and this should give us the location set earlier. 

This posts are written with assumption that some knowledge in java programming concepts such `jar`, `.class` files and what it means and other are known.

### Maven

The project uses apache maven buid system. The binary zip for installation is provided on the apache maven website. After downloading the binary zip package, unzip it. Inside the `bin` folder exists. We need to add the location for this folder to the Path variable in the `/etc/environment` file. The syntax for the path variable is 

```
PATH=location1:location2:location3.....:locationn
```
The `bin` folder location can be added at any place. Once added we need to close the terminal and open it again to reflect the changes or else the command

``` bash
source /etc/environment
```
To check whether maven is installed or not enter the command given below

``` bash
mvn --version
```

## Project Structure

To start with maven build system is used to set up project template. To do this first we need to decide what the `groupId`, `artifactId`. The `groupId` is organization domain name in reverse (standard practice but could be anything since in this case we are not publishing) whereas `artifactId` represents the project name. Next we enter following command in terminal taken from [tutorial](https://maven.apache.org/guides/getting-started/index.html)

``` bash
mvn archetype:generate -DgroupId=<group id value> -DartifactId=<artifact id value> -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.5 -DinteractiveMode=fals
```

Enter this command at the location where you want to generate the project folder. The basic template generated contains `App.java` which can execute directly by compiling and executing the bytecode `.class` file generated.

In order to do this the maven way, we can compile the code but it will only generate a jar file under `target/` folder but we it can only be used as library. In order to make a terminal application what we need to do is make the jar executable. To do this we add the classpath for class containing the static main method in it.

We update the `maven-jar-plugin` in the pom configuration. Here is the updates that needs to be added

``` xml
<plugin>
  <artifactId>maven-jar-plugin</artifactId>
  <version>"pregenerated value"</version>
  <configuration>
    <archive>
      <manifest>
        <mainClass>"path to class containing the main method"</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
```

The main commands (executed from the root location of the project) to remember with maven build system are

- compile checks

``` bash
mvn compile
```

- compile checks and generate jar

``` bash
mvn clean install
```
Once the jar is generated you can execute the jar using command

``` bash
java -jar target/<jar name>
```
As no changes have been done in the main method logic, after executing the above command and it should print "Hello World!" which marks a successful completion for part 1 of this series.
