# Apache-Maven-Zero-to-Hero
<img src="https://github.com/bhuvan-raj/Apache-Maven-Zero-to-Hero/blob/main/assets/maven.png" alt="Banner" />


## 📝 What is Apache Maven?

**Apache Maven** is a powerful **build automation** and **project management tool** primarily used for Java projects, though it can be applied to other languages. It was introduced to address the complexities of managing dependencies and standardizing build processes in large Java projects.

The name Maven, meaning **"accumulator of knowledge"** in Yiddish, reflects its core purpose: to manage a project's build, reporting, and documentation from a central piece of information.

### 🔑 Key Concepts:

  * **Convention over Configuration:** Maven provides a set of sensible defaults for project structure and build processes. This means developers spend less time writing lengthy build scripts (like in older tools) and more time coding.
  * **Project Object Model (POM):** The central configuration file, `pom.xml`, which describes the project, its dependencies, and the build process.
  * **Build Lifecycles:** A set of standardized phases for building a project (e.g., compile, test, package).
  * **Repositories:** Centralized locations for storing and retrieving project dependencies and built artifacts.

-----

## 🚀 Benefits of Using Maven Over Older Build Tools (like Apache Ant)

Older tools like **Apache Ant** used a **procedural** or **task-based** approach, where you had to explicitly write scripts (often in XML) detailing *every* step of the build process (e.g., "copy files A to B," "run javac on files in C"). Maven, in contrast, uses a **declarative** and **model-based** approach.

| Feature | Apache Maven (Declarative/Model-Based) | Older Tools (e.g., Ant) (Procedural/Task-Based) |
| :--- | :--- | :--- |
| **Dependency Management** | **Superior:** Automatically resolves, downloads, and manages transitive dependencies from repositories. | **Poor/Manual:** Requires manual download and management of JAR files and their own dependencies. |
| **Project Structure** | **Standardized:** Enforces a consistent directory layout (`src/main/java`, `src/test/java`, etc.), making new projects immediately familiar. | **Flexible/Custom:** Requires developers to define the entire directory structure and write tasks for every path. |
| **Build Process** | **Uniform/Standardized:** Uses pre-defined **lifecycles** (e.g., `compile`, `test`, `package`). Running `mvn package` automatically executes preceding phases. | **Custom:** Requires explicit definition of every build step (target) in the build script. |
| **Project Information** | Generates detailed reports, documentation, dependency lists, and test results automatically. | Requires explicit tasks and often external tools to generate reports and documentation. |
| **Configuration** | **Convention over Configuration:** Minimal configuration needed for standard builds. | **High Configuration:** Requires extensive, often repetitive, scripting for standard tasks. |

-----

# Maven Architecture and Components:

```
╔═══════════════════════════════════════════════════════════════════════════╗
║                          MAVEN ARCHITECTURE                               ║
╚═══════════════════════════════════════════════════════════════════════════╝

                    ┌─────────────────────────────────┐
                    │   CENTRAL/REMOTE REPOSITORIES   │
                    │   (Maven Central, Nexus, etc.)  │
                    │      External Artifacts         │
                    └────────────────┬────────────────┘
                                     │
                                     │ (3) Download
                                     │     Artifacts
                                     ↓
    ┌──────────────────┐      ┌─────────────────────┐
    │  PROJECT OBJECT  │      │  LOCAL REPOSITORY   │
    │   MODEL (POM)    │←────→│  ~/.m2/repository   │
    │    pom.xml       │ (2)  │   Cached Artifacts  │
    └────────┬─────────┘Cache │    & Dependencies   │
             │         Install└──────────┬──────────┘
             │                           │
             │ (1) Defines:              │ (4) Resolves
             │  • Build Config           │     Dependencies
             │  • Dependencies           │
             │  • Plugins                │
             │  • Goals                  │
             ↓                           ↓
    ╔════════════════════════════════════════════════════════╗
    ║         MAVEN BUILD LIFECYCLE (mvn command)            ║
    ╠════════════════════════════════════════════════════════╣
    ║                                                        ║
    ║  ┌──────────────────────────────────────────────┐    ║
    ║  │         DEFAULT LIFECYCLE PHASES             │    ║
    ║  ├──────────────────────────────────────────────┤    ║
    ║  │  validate  →  Validate project structure     │    ║
    ║  │  compile   →  Compile source code            │    ║
    ║  │  test      →  Run unit tests                 │    ║
    ║  │  package   →  Create JAR/WAR                 │    ║
    ║  │  verify    →  Run integration tests          │    ║
    ║  │  install   →  Install to local repo          │    ║
    ║  │  deploy    →  Deploy to remote repo          │    ║
    ║  └──────────────────┬───────────────────────────┘    ║
    ║                     │                                 ║
    ║                     │ (5) Each Phase                  ║
    ║                     │     Executes                    ║
    ║                     ↓                                 ║
    ║  ┌──────────────────────────────────────────────┐    ║
    ║  │              PLUGINS (Workers)               │    ║
    ║  ├──────────────────────────────────────────────┤    ║
    ║  │  maven-compiler-plugin   → compile:compile   │    ║
    ║  │  maven-surefire-plugin   → test:test         │    ║
    ║  │  maven-jar-plugin        → jar:jar           │    ║
    ║  │  maven-install-plugin    → install:install   │    ║
    ║  │  maven-deploy-plugin     → deploy:deploy     │    ║
    ║  └──────────────────────────────────────────────┘    ║
    ║                                                        ║
    ║  ┌──────────────────────────────────────────────┐    ║
    ║  │         PLUGIN GOALS (Specific Tasks)        │    ║
    ║  │  plugin:goal  →  Executes specific operation │    ║
    ║  └──────────────────────────────────────────────┘    ║
    ║                                                        ║
    ╚════════════════════════════════════════════════════════╝
                             │
                             │ (6) Produces
                             ↓
                    ┌─────────────────┐
                    │  BUILD OUTPUT   │
                    │  target/        │
                    │  • Classes      │
                    │  • JARs/WARs    │
                    │  • Reports      │
                    └─────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║                           FLOW EXPLANATION                                ║
╠═══════════════════════════════════════════════════════════════════════════╣
║  1. POM defines project configuration, dependencies, and plugins          ║
║  2. Maven caches/installs artifacts in local repository                   ║
║  3. Maven downloads missing dependencies from remote repositories         ║
║  4. Maven resolves all dependencies from local/remote repositories        ║
║  5. Build lifecycle phases execute bound plugin goals                     ║
║  6. Plugins perform actual build work (compile, test, package, etc.)      ║
╚═══════════════════════════════════════════════════════════════════════════╝
```

This ASCII diagram shows:
- **Repository hierarchy** (Remote → Local)
- **POM as the central configuration**
- **Build lifecycle with phases**
- **Plugins that do the actual work**
- **Clear flow of operations** (numbered steps)
- **Output generation**

The architecture emphasizes Maven's convention-over-configuration approach and how components interact during the build process.

1.  **Project Object Model (POM):** The core of the project. It defines the project's coordinates, dependencies, build settings, plugins, and more.
2.  **Build Lifecycles, Phases, and Goals:**
      * **Lifecycle:** The overall process (e.g., `default`, `clean`, `site`).
      * **Phase:** A step in the lifecycle (e.g., `compile`, `test`, `package`). When you execute a phase, all preceding phases are executed sequentially.
      * **Goal:** A specific task bound to a phase, executed by a **Plugin** (e.g., the `compiler:compile` goal is executed in the `compile` phase).
3.  **Plugins:** The actual workhorses of Maven. They are collections of one or more goals. Maven is essentially a plugin execution framework.
4.  **Repositories:** Storage locations for project artifacts (libraries, JARs, etc.).
      * **Local Repository:** A directory on the developer's machine (`~/.m2/repository`). Maven downloads artifacts from remote repositories to here for local reuse.
      * **Central Repository (Maven Central):** The primary public repository managed by the Apache Software Foundation, containing a massive collection of open-source artifacts.
      * **Remote/Company Repositories:** Private repositories hosted by organizations for custom, proprietary, or mirrored artifacts.

-----
# pom.xml

## Example `pom.xml` File

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
    <!-- POM Model Version -->
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

## 📋 Component Breakdown

### 1. **XML Declaration & Namespaces**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"...>
```
- Defines the XML version and encoding
- The `xmlns` attributes specify the XML schema for Maven, ensuring the POM is valid

### 2. **Model Version**
```xml
<modelVersion>4.0.0</modelVersion>
```
- Always set to `4.0.0` for current Maven versions
- Indicates the POM structure version, not your project version

### 3. **Project Coordinates (GAV)**
These three elements uniquely identify your project in the Maven universe:

**`<groupId>`**: `com.example.myapp`
- Typically your organization's reverse domain name
- Groups related projects together
- Think of it like a Java package name

**`<artifactId>`**: `user-management-system`
- The specific name of this project/module
- Should be lowercase with hyphens
- Becomes part of the JAR filename

**`<version>`**: `1.0`
- Your project's version number
  
**`<packaging>`**: `jar`
- Output format: `jar` (Java Archive), `war` (Web Archive), `pom` (parent project)
- Default is `jar` if omitted

### 4. **Dependencies Section**
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>
```
- Lists all external libraries your project needs
- Each dependency has its own GAV coordinates
- **`<scope>`**: Controls when the dependency is available
  - `compile` (default): Available everywhere
  - `test`: Only for test code
  - `provided`: Available at compile time but not packaged (e.g., servlet APIs)
  - `runtime`: Not needed for compilation, but required at runtime
- Maven automatically downloads dependencies from repositories

### 5. **Build Section**
```xml
<build>
    <finalName>user-management</finalName>
    <plugins>...</plugins>
</build>
```

**`<finalName>`**: The name of the output file (without extension)
- Result: `user-management.jar` instead of `user-management-system-1.0-SNAPSHOT.jar`

**Plugins**: Extend Maven's capabilities
- **maven-compiler-plugin**: Compiles Java source code
  - Specifies Java version for source and target
  
- **maven-surefire-plugin**: Runs unit tests during the build
  
- **maven-jar-plugin**: Packages compiled code into a JAR
  - Can specify the main class for executable JARs

---

## 🎯 How It All Works Together

1. **Maven reads the POM** and understands what your project needs
2. **Downloads dependencies** from Maven Central (or other repositories)
3. **Executes plugins** bound to lifecycle phases (compile, test, package, etc.)
4. **Produces the artifact** (JAR file) with the specified configuration

When you run `mvn clean install`, Maven:
- Cleans previous builds
- Compiles your code with Java 17
- Runs JUnit tests
- Packages everything into `user-management.jar`
- Installs it to your local Maven repository

This POM gives you a complete, working Maven project configuration!

# 🚀 Essential Maven Commands - Clean & Simple

Maven commands follow the pattern: `mvn [phase/goal]`

---

## 📦 **Core Build Commands** (In Order of Execution)

### 1. `mvn clean`
**What it does:** Deletes the `target/` folder  
**Why use it:** Removes all old compiled files and build artifacts for a fresh start  
**When to use:** Before a new build to avoid conflicts with old files

---

### 2. `mvn compile`
**What it does:** Compiles your Java source code (`src/main/java/`)  
**Output:** Creates `.class` files in `target/classes/`  
**When to use:** When you want to check if your code compiles without running tests

---

### 3. `mvn test`
**What it does:** Compiles your code + runs all unit tests (`src/test/java/`)  
**Output:** Test reports in `target/surefire-reports/`  
**When to use:** To verify your code works correctly

---

### 4. `mvn package`
**What it does:** Runs compile + test + creates a JAR/WAR file  
**Output:** Your packaged artifact in `target/` (e.g., `my-app-1.0.jar`)  
**When to use:** When you want a distributable file of your project

---

### 5. `mvn install`
**What it does:** Runs package + copies the JAR to your **local Maven repository**  
**Location:** `~/.m2/repository/`  
**When to use:** To make your project available to other projects on your machine

---

### 6. `mvn deploy`
**What it does:** Runs install + uploads the artifact to a **remote repository**  
**When to use:** To share your project with team members or publish it publicly

---

## 🎯 **Special Commands**

### `mvn archetype:generate`
**What it does:** Creates a new Maven project from a template  
**Interactive:** Asks you questions to set up project structure  
**When to use:** Starting a brand new Maven project

**Example:**
```bash
mvn archetype:generate -DgroupId=com.mycompany -DartifactId=my-app
```

---

### `mvn clean install` ⭐ **Most Common**
**What it does:** 
1. Cleans old files
2. Compiles code
3. Runs tests
4. Packages into JAR
5. Installs to local repository

**When to use:** For a complete, fresh build of your project

---

## 📊 **Command Flow Visualization**

```
mvn clean  ───>  mvn compile  ───>  mvn test  ───>  mvn package  ───>  mvn install  ───>  mvn deploy
    ↓                ↓                 ↓                 ↓                  ↓                  ↓
Delete old      Compile Java      Run unit         Create JAR/        Copy JAR to      Upload JAR to
  files           code              tests             WAR file       local repo (~/.m2)  remote repo
```

---

## 💡 **Quick Reference**

| Need to... | Use this command |
|:-----------|:-----------------|
| Start fresh | `mvn clean` |
| Check if code compiles | `mvn compile` |
| Run tests only | `mvn test` |
| Create a JAR file | `mvn package` |
| **Full local build** | `mvn clean install` ⭐ |
| Share with team | `mvn deploy` |
| Create new project | `mvn archetype:generate` |

---

## 🔑 **Key Concepts**

**Maven runs phases in order:**  
If you run `mvn install`, Maven automatically runs: `compile` → `test` → `package` → `install`

**The `target/` directory:**  
- All build outputs go here
- Safe to delete (Maven recreates it)
- `.gitignore` this folder!

**Local repository (`~/.m2/repository/`):**  
- Where Maven stores downloaded dependencies
- Where `mvn install` puts your project
- Shared across all your Maven projects

---

**Pro Tip:** 90% of the time, you'll use `mvn clean install` for local development! 🎯

## 🛠️ Apache Maven Installation

## 💻 OS-Specific Apache Maven Installation Steps

### 1. 🐧 Linux (Fedora/CentOS/RHEL - using DNF)

For Fedora and other Red Hat-based distributions, the easiest method is using the **DNF** (Dandified YUM) package manager. This automatically handles the download and path setup.

| Step | Command | Notes |
| :--- | :--- | :--- |
| **1. Install JDK** | `sudo dnf install java-latest-openjdk` | Installs the latest OpenJDK (or replace with a specific version). |
| **2. Install Maven** | `sudo dnf install maven` | Installs Maven from the Fedora repositories. |
| **3. Verify** | `mvn -v` | Confirms the installation. |

### 2. 🖥️ Windows (Manual Installation)

Since Windows doesn't have a single integrated package manager like Linux/macOS, a manual install and environment variable configuration is the standard approach.

| Step | Action | Details |
| :--- | :--- | :--- |
| **1. Install JDK & `JAVA_HOME`** | **Prerequisite** | Ensure Java is installed and the `JAVA_HOME` variable is set (e.g., to `C:\Program Files\Java\jdk-17`). |
| **2. Download** | Go to the Apache Maven website | Download the **binary zip archive** (`apache-maven-X.X.X-bin.zip`). |
| **3. Extract** | Extract the archive | Extract to a simple location, like `C:\Maven\apache-maven-X.X.X`. |
| **4. Set `M2_HOME`** | **System Properties** > **Environment Variables** | Create a new **System Variable**: `M2_HOME` with the value set to your Maven directory (e.g., `C:\Maven\apache-maven-X.X.X`). |
| **5. Add to PATH** | **System Properties** > **Environment Variables** | Edit the **System Variable** `Path` and add a new entry: `%M2_HOME%\bin`. |
| **6. Verify** | Open a new **Command Prompt** or **PowerShell** | Run `mvn -v`. |

### 3. 🐘 Linux (Ubuntu/Debian - using APT)

For Debian-based systems like Ubuntu, the **APT** (Advanced Package Tool) package manager is used for a quick and managed installation.

| Step | Command | Notes |
| :--- | :--- | :--- |
| **1. Update Packages** | `sudo apt update` | Always a good practice before installing new software. |
| **2. Install JDK** | `sudo apt install openjdk-17-jdk` | Installs a supported OpenJDK version. |
| **3. Install Maven** | `sudo apt install maven` | Installs Maven from the official Ubuntu repositories. |
| **4. Verify** | `mvn -v` | Confirms the installation. |

### 4. 🍎 macOS (Using Homebrew)

The simplest and recommended way to install Maven on macOS is by using the popular package manager **Homebrew**.

| Step | Command | Notes |
| :--- | :--- | :--- |
| **1. Install Homebrew** | `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"` | (If not already installed) |
| **2. Install JDK** | `brew install openjdk@17` | Installs a specific OpenJDK version (Homebrew may require linking the Java version). |
| **3. Install Maven** | `brew install maven` | Homebrew handles the download and environment configuration automatically. |
| **4. Verify** | `mvn -v` | Confirms the installation. |

---

### ⚠️ Post-Installation Note

When using a package manager (`dnf`, `apt`, `brew`), the installation is often managed and the environment variables are set automatically. For manual installation (common on Windows), make sure to **open a *new* terminal/command prompt** after setting the environment variables for the changes to take effect.
