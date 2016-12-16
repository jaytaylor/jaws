# Java API for WordNet Searching (JAWS)

Current Version: 1.3.1

[![Release](https://jitpack.io/v/jaytaylor/jaws.svg)](https://jitpack.io/#jaytaylor/jaws)

## How this came to be

JAWS was originally written and created by Brett Spell.

I ([Jay Taylor](http://jaytaylor.com)) have done my best to resurrect this code and its corresponding documentation (with the help of archive.org).  There was no license included or referenced anywhere that I found.

## How to use this repository

### Maven

Add the https://jitpack.io/ repository to your POM file and declare this github repo as a dependency.

See the jitpack [JAWS package page](https://jitpack.io/#jaytaylor/jaws) for more information and instructions for other dependency managers (e.g. SBT, gradle, etc).

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
                <version>1.3.1</version>
            </dependency>
        </dependencies>
    </project>

#### Building / Creating a release

Run:

    mvn package

Then upload the resulting JAR file from the `target/` directory to github.


**Original Documentation Follows**

[http://lyle.smu.edu/~tspell/jaws/](http://web.archive.org/web/http://lyle.smu.edu/~tspell/jaws/)

## Overview

As its name implies, the Java API for WordNet Searching (JAWS) is an API that provides Java applications with the ability to retrieve data from the [WordNet](http://wordnet.princeton.edu/) database. It is a simple and fast API that is compatible with both the 2.1 and 3.0 versions of the WordNet database files and can be used with Java 1.4 and later.

JAWS was created by [Brett Spell](mailto:tbspell@verizon.net), an adjunct member of the faculty in the Computer Science and Engineering ([CSE](http://web.archive.org/web/http://lyle.smu.edu/cse/)) department at [Southern Methodist University](http://www.smu.edu/).

On this page you'll find the following information:

* [How to use JAWS in your application](#using-jaws-with-your-application)
* [Getting Started With the API](#getting-started-with-the-api)
* [Example program](#example-program)
* [Changes to the API](#changes)

### Using JAWS With Your Application

To use JAWS in your application you must do the following:

1.  Obtain a copy of the WordNet database files, which can be accomplished by [downloading](http://wordnet.princeton.edu/wordnet/download/) and installing WordNet (you must download the full version of WordNet and not just the database files).
2.  Download the Java Archive (JAR) file containing the compiled JAWS code (available [here](https://github.com/jaytaylor/jaws/releases)).
3.  When starting your application you must:
    * Include in your Java Virtual Machine's class path the JAR file you downloaded.
    * Use the `wordnet.database.dir` system property to specify where the WordNet database files are located (see below).

#### Specifying the Database Directory

The WordNet database files are found in the `dict` subdirectory below your WordNet installation directory.  For example, if you installed WordNet in `C:\WordNet-3.0\` the database files will be located in the `C:\WordNet-3.0\dict\` directory.

You can either set the `wordnet.database.dir` property within your code or it can be done externally. To set it from within your code you must use the [setProperty()](http://web.archive.org/web/http://java.sun.com/j2se/1.4.2/docs/api/java/lang/System.html#setProperty%28java.lang.String,%20java.lang.String%29) method in the [System](http://web.archive.org/web/http://java.sun.com/j2se/1.4.2/docs/api/java/lang/System.html) class as in the following example:

**`System.setProperty("wordnet.database.dir", "C:\WordNet-3.0\dict\");`**

The technique for setting the `wordnet.database.dir` property externally (outside your code) depends on how you're executing your application. If you're running your code from within an Integrated Development Environment (IDE) such as [Eclipse](http://www.eclipse.org/) you'll need to use that IDE's support for setting system properties. For example, Eclipse allows you to specify "VM Arguments" and you would need to include an entry like the following in the list of arguments to use when running your code:

**`-Dwordnet.database.dir=C:\WordNet-3.0\dict\`**

Alternatively, if you're running your application from the command line you would need to use the `-D` option as shown in the next section of these instructions.

#### Starting Your Application

Let's assume the following:

* You've downloaded the JAR file containing the JAWS executable code to a Windows machine and saved it as `jaws-bin.jar` in your `C:\mywork\code` directory.
* You installed WordNet to a directory named `C:\WordNet-3.0\` which in turn would mean that the database files are located in `C:\WordNet-3.0\dict\`.

In this case, you could start a Java Virtual Machine from the command line like the one shown below, which assumes that you also have defined a class called `MyApp` that contains a `main()` method:

**java -classpath .;C:\mywork\code\jaws-bin.jar -Dwordnet.database.dir=C:\WordNet-3.0\dict MyApp**

### Getting Started With the API

From within the application you started you can use JAWS by first obtaining an instance of [WordNetDatabase](http://web.archive.org/web/http://lyle.smu.edu/%7Etspell/jaws/doc/edu/smu/tspell/wordnet/WordNetDatabase.html) with code like the following, which assumes that you've performed an `import` of the classes in the [edu.smu.tspell.wordnet](http://web.archive.org/web/http://lyle.smu.edu/%7Etspell/jaws/doc/edu/smu/tspell/wordnet/package-summary.html) package:

`**WordNetDatabase database = WordNetDatabase.getFileInstance();**`

Once you've done so, you can begin to retrieve synsets from the database as shown in the example below. This code retrieves all noun synsets for "fly" and loops through each one printing its first word form, its description, and the number of hyponyms associated with that noun synset:

`**
NounSynset nounSynset;
NounSynset[] hyponyms;

WordNetDatabase database = WordNetDatabase.getFileInstance();
Synset[] synsets = database.getSynsets("fly", SynsetType.NOUN);
for (int i = 0; i < synsets.length; i++) {
    nounSynset = (NounSynset)(synsets[i]);
    hyponyms = nounSynset.getHyponyms();
    System.err.println(nounSynset.getWordForms()[0] +
            ": " + nounSynset.getDefinition() + ") has " + hyponyms.length + " hyponyms");
}**`

For more information on how to use JAWS you can browse the [API documentation](http://web.archive.org/web/http://lyle.smu.edu/%7Etspell/jaws/doc/overview-summary.html), although in most cases your application should only need to refer to the types defined in the [edu.smu.tspell.wordnet](http://web.archive.org/web/http://lyle.smu.edu/%7Etspell/jaws/doc/edu/smu/tspell/wordnet/package-summary.html) package.

### Example Program

A small sample program is available for download [here](TestJAWS.java) that demonstrates how to use the API. It displays some of the attributes of the synset(s), if any, that contain the word form specified as the first argument specified when the program is executed.

### Changes

* December 24, 2009 -- Updated example to show how to retrieve hyponyms.
* June 20, 2009 -- Bug fix and new convenience methods for head and satellite synsets.
* July 21, 2008 -- Support derivational form retrieval for all synset types.
* February 13, 2008 -- Removed unused code

