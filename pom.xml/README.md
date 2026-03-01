
<div align="center">

![Maven](https://img.shields.io/badge/Apache%20Maven-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![XML](https://img.shields.io/badge/XML-Config-orange?style=for-the-badge)

# The pom.xml File

> **The single source of truth for your entire Maven project — dependencies, build config, plugins, and more.**

</div>

---

## 📋 Table of Contents

- [What is a POM?](#-what-is-a-pom)
- [Complete pom.xml Example](#-complete-pomxml-example)
- [Section-by-Section Breakdown](#-section-by-section-breakdown)
  - [XML Declaration & Namespaces](#1-xml-declaration--namespaces)
  - [Model Version](#2-model-version)
  - [GAV Coordinates](#3-gav-coordinates--project-identity)
  - [Dependencies](#4-dependencies)
  - [Dependency Scopes](#5-dependency-scopes)
  - [Build Configuration](#6-build-configuration)
  - [Core Plugins](#7-core-plugins-explained)
- [How Maven Uses the POM](#-how-maven-uses-the-pom)

---

## 💡 What is a POM?

**POM** stands for **Project Object Model**. It is the `pom.xml` file at the root of every Maven project and it defines everything Maven needs to know to build, test, and deploy your application.

Think of the `pom.xml` as the blueprint of your project — Maven reads it once, and from that single file it knows what to compile, what libraries to download, what Java version to target, what to name the output file, and where to deploy it.

---

## 📄 Complete pom.xml Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <!-- POM Model Version — always 4.0.0 -->
    <modelVersion>4.0.0</modelVersion>

    <!-- ========== PROJECT COORDINATES (GAV) ========== -->
    <groupId>com.example.myapp</groupId>
    <artifactId>user-management-system</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>

    <!-- ========== PROJECT METADATA ========== -->
    <name>User Management System</name>
    <description>A simple user management application with REST API</description>
    <url>https://github.com/example/user-management</url>

    <!-- ========== PROPERTIES ========== -->
    <properties>
        <spring.version>6.0.0</spring.version>
        <junit.version>5.10.0</junit.version>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

    <!-- ========== DEPENDENCIES ========== -->
    <dependencies>

        <!-- Spring Framework -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- Logging -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>2.0.7</version>
        </dependency>

        <!-- Test Dependencies -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>5.3.1</version>
            <scope>test</scope>
        </dependency>

    </dependencies>

    <!-- ========== BUILD CONFIGURATION ========== -->
    <build>
        <finalName>user-management</finalName>

        <plugins>

            <!-- Compiler Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                </configuration>
            </plugin>

            <!-- Surefire Plugin (for running tests) -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.1.2</version>
            </plugin>

            <!-- JAR Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.3.0</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>com.example.myapp.Main</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>

        </plugins>
    </build>

</project>
```

---

## 🔍 Section-by-Section Breakdown

### 1. XML Declaration & Namespaces

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" ...>
```

The opening lines declare this as an XML file with UTF-8 encoding, and the `xmlns` attributes tell Maven which schema version to validate the POM against. These are boilerplate — copy them into every `pom.xml` as-is.

---

### 2. Model Version

```xml
<modelVersion>4.0.0</modelVersion>
```

This is always `4.0.0` for all current Maven versions. It refers to the version of the POM *format* — not your project's version. Do not change this.

---

### 3. GAV Coordinates — Project Identity

These three elements together form the **GAV coordinates** that uniquely identify your project in the Maven universe — in your local repo, in Maven Central, and in any downstream project that depends on it.

```xml
<groupId>com.example.myapp</groupId>
<artifactId>user-management-system</artifactId>
<version>1.0</version>
<packaging>jar</packaging>
```

| Element | Purpose | Example |
|---------|---------|---------|
| `groupId` | Your organization or namespace — typically reverse domain name | `com.example`, `org.springframework` |
| `artifactId` | The specific project/module name | `user-management-system`, `spring-core` |
| `version` | The release version of this artifact | `1.0`, `2.3.1`, `1.0-SNAPSHOT` |
| `packaging` | Output format — `jar`, `war`, or `pom` | `jar` is the default |

> 💡 **SNAPSHOT versions** (e.g., `1.0-SNAPSHOT`) indicate a project still in active development. Maven treats them as mutable — it will re-download them if they've changed, unlike release versions which are fixed.

---

### 4. Dependencies

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>
```

Each `<dependency>` block tells Maven to download and include a specific library. Maven resolves **transitive dependencies** automatically — if `spring-core` depends on another library, Maven downloads that too without you listing it.

The `${spring.version}` syntax references a property defined in `<properties>`, keeping versions consistent and easy to update in one place.

---

### 5. Dependency Scopes

The `<scope>` element controls **when a dependency is available** during the build lifecycle:

| Scope | Available At Compile | Available At Test | Packaged Into Output | Use Case |
|-------|---------------------|------------------|---------------------|----------|
| `compile` (default) | ✅ | ✅ | ✅ | Standard runtime dependency |
| `test` | ❌ | ✅ | ❌ | JUnit, Mockito — test code only |
| `provided` | ✅ | ✅ | ❌ | Servlet API — provided by the server at runtime |
| `runtime` | ❌ | ✅ | ✅ | JDBC drivers — not needed to compile but required to run |
| `system` | ✅ | ✅ | ❌ | Local JAR not in any repository |

```xml
<!-- Only available during tests — not bundled in the final JAR -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```

---

### 6. Build Configuration

```xml
<build>
    <finalName>user-management</finalName>
    <plugins>...</plugins>
</build>
```

The `<build>` section controls how Maven assembles your project.

`<finalName>` sets the name of the output file. Without it, Maven defaults to `artifactId-version.jar` — e.g., `user-management-system-1.0.jar`. With `<finalName>user-management</finalName>`, the output becomes `user-management.jar`.

---

### 7. Core Plugins Explained

#### maven-compiler-plugin

Controls how Maven compiles your Java code:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.11.0</version>
    <configuration>
        <source>17</source>   <!-- Java version of your source code -->
        <target>17</target>   <!-- Java version of the compiled bytecode -->
    </configuration>
</plugin>
```

Without this, Maven defaults to Java 8 or older — always set the compiler plugin explicitly.

#### maven-surefire-plugin

Discovers and runs your unit tests automatically during the `test` phase:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.1.2</version>
</plugin>
```

It looks for test classes matching `**/*Test.java`, `**/Test*.java`, or `**/*Tests.java` by default.

#### maven-jar-plugin

Packages compiled code into a JAR, and can set the main class for an executable JAR:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.3.0</version>
    <configuration>
        <archive>
            <manifest>
                <mainClass>com.example.myapp.Main</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
```

Setting `<mainClass>` writes the `Main-Class` attribute into the JAR's `MANIFEST.MF`, making it executable with `java -jar your-app.jar`.

---

## 🔗 How Maven Uses the POM

When you run `mvn clean install`, here is what Maven does with your POM:

```
  Reads pom.xml
       │
       ├──▶ Resolves all <dependencies> from repositories
       │
       ├──▶ Applies <properties> values throughout the file
       │
       ├──▶ Runs lifecycle phases in order
       │         Each phase triggers its bound plugin goal
       │
       ├──▶ Compiler plugin reads <source> and <target> settings
       │
       ├──▶ Surefire plugin runs all test classes
       │
       ├──▶ JAR plugin creates target/<finalName>.jar
       │         Sets <mainClass> in MANIFEST.MF if configured
       │
       └──▶ Install plugin copies JAR to ~/.m2/repository/
                 using groupId/artifactId/version as the path
```

---

<div align="center">

*Part of the [Apache Maven Zero to Hero](../README.md) course*

</div>
