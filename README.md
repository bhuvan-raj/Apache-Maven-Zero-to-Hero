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

## 📁 Project Object Model (POM) - File Explanation (`pom.xml`)

The `pom.xml` file is the fundamental unit of work in Maven. It is an XML file containing all configuration and information for the project.

### Key Elements in `pom.xml`:

| Element | Description | Example |
| :--- | :--- | :--- |
| `<project>` | The root element of the POM. | `<project>...</project>` |
| `<modelVersion>` | The current version of the POM model. (Always `4.0.0`) | `<modelVersion>4.0.0</modelVersion>` |
| **Project Coordinates (GAV)** | The unique identifier for the project artifact. | |
| `<groupId>` | The unique ID of the organization or group. (e.g., reverse domain name) | `<groupId>com.mycompany.app</groupId>` |
| `<artifactId>` | The unique ID of the project/module within the group. | `<artifactId>my-app</artifactId>` |
| `<version>` | The version of the project artifact. (`-SNAPSHOT` indicates a work-in-progress version). | `<version>1.0-SNAPSHOT</version>` |
| `<packaging>` | The project's output type (e.g., `jar`, `war`, `pom`). Default is `jar`. | `<packaging>jar</packaging>` |
| `<properties>` | Defines variables used within the POM (e.g., version numbers). | `<java.version>17</java.version>` |
| `<dependencies>` | A list of libraries (artifacts) your project needs to compile, test, or run. | Defines a `junit` dependency. |
| `<build>` | Configures the build process, primarily through binding plugins to lifecycle phases. | Defines the `maven-compiler-plugin`. |

-----

## ⌨️ Essential Maven Commands

Maven commands are executed using the `mvn` executable, typically followed by a lifecycle phase or a plugin goal.

| Command | Lifecycle Phase / Goal | Description |
| :--- | :--- | :--- |
| `mvn clean` | Clean Lifecycle | Deletes the `target` directory, removing all previously compiled and built artifacts. |
| `mvn compile` | Default Lifecycle Phase | Compiles the project's main source code (`src/main/java`) and places the class files in `target/classes`. |
| `mvn test` | Default Lifecycle Phase | Executes the unit tests (`src/test/java`). Precedes this by running `compile`. |
| `mvn package` | Default Lifecycle Phase | Takes the compiled code and packages it into its distributable format (e.g., a `.jar` or `.war` file) in the `target` directory. |
| `mvn install` | Default Lifecycle Phase | Runs all phases up to `package`, then installs the packaged artifact into the **local repository** for use by other local projects. |
| `mvn deploy` | Default Lifecycle Phase | Installs the artifact to the local repository and then copies the final artifact to the **remote repository**. |
| `mvn archetype:generate` | Plugin Goal | Creates a new project structure based on a template (archetype). |
| `mvn clean install` | Combined Command | Cleans the project first, then builds, tests, packages, and installs the artifact locally. This is a common command for a full local build. |

-----

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
