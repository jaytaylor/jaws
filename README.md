# Java API for WordNet Searching (JAWS)

http://lyle.smu.edu/~tspell/jaws/

Version 1.3

[![Release](https://jitpack.io/v/jaytaylor/jaws.svg)](https://jitpack.io/#jaytaylor/jaws)

## How to use this repository

### Maven

Add the https://jitpack.io/ repository to your POM file and declare this github repo as a dependency.

See https://jitpack.io/ for more information and instructions.

Example:

    <?xml version="1.0" encoding="UTF-8"?>
    <project
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <groupId>YourProject</groupId>
        <artifactId>yourproject</artifactId>
        <version>1.0-SNAPSHOT</version>
        <repositories>
            <repository>
                <id>jitpack.io</id>
                <name>jitpack</name>
                <url>https://jitpack.io</url>
            </repository>
        </repositories>
        <dependencies>
            <dependency>
                <groupId>com.github.jaytaylor</groupId>
                <artifactId>jaws</artifactId>
                <version>1.3</version>
            </dependency>
        </dependencies>
    </project>

#### Building / Creating a release

Run:

    mvn package

Then upload the resulting JAR file from the `target/` directory to github.

