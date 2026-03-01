<img src="https://github.com/bhuvan-raj/Apache-Maven-Zero-to-Hero/blob/main/assets/maven.png" alt="Banner" />

<div align="center">

![Maven](https://img.shields.io/badge/Apache%20Maven-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)

# Maven Architecture

> **Understand what happens under the hood every time you run `mvn`.**

</div>

---

## 📋 Table of Contents

- [Architecture Overview](#-architecture-overview)
- [Component 1 — Project Object Model (POM)](#-component-1--project-object-model-pom)
- [Component 2 — Build Lifecycles, Phases & Goals](#-component-2--build-lifecycles-phases--goals)
- [Component 3 — Plugins](#-component-3--plugins)
- [Component 4 — Repositories](#-component-4--repositories)
- [How It All Fits Together](#-how-it-all-fits-together)

---

## 🗺️ Architecture Overview

```
╔═══════════════════════════════════════════════════════════════════════════╗
║                          MAVEN ARCHITECTURE                               ║
╚═══════════════════════════════════════════════════════════════════════════╝

                    ┌─────────────────────────────────┐
                    │   CENTRAL / REMOTE REPOSITORY   │
                    │   (Maven Central, Nexus, etc.)  │
                    │      External Artifacts         │
                    └────────────────┬────────────────┘
                                     │
                                     │ (3) Download artifacts
                                     │     if not cached locally
                                     ▼
    ┌──────────────────┐      ┌─────────────────────┐
    │  PROJECT OBJECT  │      │  LOCAL REPOSITORY   │
    │   MODEL (POM)    │◀────▶│  ~/.m2/repository   │
    │    pom.xml       │ (2)  │   Cached artifacts  │
    └────────┬─────────┘      └──────────┬──────────┘
             │                           │
             │ (1) Defines               │ (4) Resolves
             │  • Dependencies           │     dependencies
             │  • Plugins                │     for the build
             │  • Build config           │
             ▼                           ▼
    ╔════════════════════════════════════════════════════════╗
    ║              MAVEN BUILD ENGINE                        ║
    ╠════════════════════════════════════════════════════════╣
    ║                                                        ║
    ║  DEFAULT LIFECYCLE PHASES (in order):                  ║
    ║  ┌──────────────────────────────────────────────┐     ║
    ║  │  validate  →  compile  →  test  →  package   │     ║
    ║  │       →  verify  →  install  →  deploy       │     ║
    ║  └──────────────────────────────────────────────┘     ║
    ║                      │                                 ║
    ║                      │ (5) Each phase triggers         ║
    ║                      │     a bound plugin goal         ║
    ║                      ▼                                 ║
    ║  PLUGINS (the actual workers):                         ║
    ║  ┌──────────────────────────────────────────────┐     ║
    ║  │  maven-compiler-plugin  →  compile:compile   │     ║
    ║  │  maven-surefire-plugin  →  test:test         │     ║
    ║  │  maven-jar-plugin       →  package:jar       │     ║
    ║  │  maven-install-plugin   →  install:install   │     ║
    ║  │  maven-deploy-plugin    →  deploy:deploy     │     ║
    ║  └──────────────────────────────────────────────┘     ║
    ╚════════════════════════════════════════════════════════╝
                             │
                             │ (6) Produces output
                             ▼
                    ┌─────────────────┐
                    │  target/        │
                    │  ├── classes/   │
                    │  ├── app.jar    │
                    │  └── reports/   │
                    └─────────────────┘
```

Maven architecture has four core components that work together on every build.

---

## 📄 Component 1 — Project Object Model (POM)

The **POM** (`pom.xml`) is the central configuration file of every Maven project. It is the single source of truth that tells Maven everything it needs to know about your project.

The POM defines:

- **Project identity** — group, artifact name, version (GAV coordinates)
- **Dependencies** — external libraries your project needs
- **Plugins** — tools Maven will use during the build
- **Build configuration** — compiler version, output name, resource filters
- **Repositories** — where to deploy your artifact after building
- **Profiles** — environment-specific build configurations

Every Maven command starts by reading `pom.xml`. Without it, Maven cannot do anything.

---

## 🔄 Component 2 — Build Lifecycles, Phases & Goals

Maven organizes work into three built-in **lifecycles**, each made up of **phases**, and each phase executes **plugin goals**.

### The Three Lifecycles

| Lifecycle | Purpose |
|-----------|---------|
| `default` | The main lifecycle — compiles, tests, packages, and deploys your project |
| `clean` | Removes build output from previous runs (`target/` directory) |
| `site` | Generates project documentation and reports |

### The Default Lifecycle — Phases in Order

When you run any phase, Maven automatically runs **all preceding phases** first.

| Phase | What Happens |
|-------|-------------|
| `validate` | Checks the project structure and POM is correct |
| `compile` | Compiles source code in `src/main/java/` |
| `test` | Runs unit tests in `src/test/java/` |
| `package` | Packages compiled code into a JAR or WAR |
| `verify` | Runs integration tests and checks quality |
| `install` | Copies the artifact to your local `~/.m2` repository |
| `deploy` | Uploads the artifact to a remote repository |

**Example:** Running `mvn package` automatically triggers `validate → compile → test → package`.

### Goals

A **goal** is a specific task executed by a plugin. Goals are bound to phases by Maven automatically.

```
Phase       Bound Goal (Plugin:Goal)
────────    ──────────────────────────────────
compile  →  maven-compiler-plugin:compile
test     →  maven-surefire-plugin:test
package  →  maven-jar-plugin:jar
install  →  maven-install-plugin:install
deploy   →  maven-deploy-plugin:deploy
```

You can also call a goal directly without going through the lifecycle:

```bash
mvn compiler:compile        # Run just the compiler goal
mvn surefire:test           # Run just the test goal
mvn dependency:tree         # Show the full dependency tree
```

---

## 🔧 Component 3 — Plugins

**Plugins are the actual workers in Maven.** The lifecycle and phases are just the framework — plugins are what actually do the compiling, testing, packaging, and deploying.

Every Maven build step you see is performed by a plugin goal. Maven ships with a core set of default plugins:

| Plugin | Default Phase | What It Does |
|--------|--------------|-------------|
| `maven-compiler-plugin` | `compile` | Compiles Java source code |
| `maven-surefire-plugin` | `test` | Discovers and runs unit tests |
| `maven-jar-plugin` | `package` | Bundles compiled code into a JAR |
| `maven-war-plugin` | `package` | Bundles a web application into a WAR |
| `maven-install-plugin` | `install` | Copies artifact to local repository |
| `maven-deploy-plugin` | `deploy` | Uploads artifact to remote repository |
| `maven-clean-plugin` | `clean` | Deletes the `target/` directory |

You can add third-party plugins to your POM for additional capabilities — Docker builds, SonarQube analysis, code generation, and more.

---

## 🗄️ Component 4 — Repositories

**Repositories** are where Maven stores and retrieves artifacts (JARs, WARs, POMs). There are three types:

### Local Repository

Located at `~/.m2/repository/` on every developer's machine. Maven downloads dependencies here on first use and reuses them for all future builds — no repeated downloads.

```
~/.m2/repository/
└── org/springframework/spring-core/6.0.0/
    ├── spring-core-6.0.0.jar
    └── spring-core-6.0.0.pom
```

### Central Repository (Maven Central)

The public repository at [https://repo1.maven.org/maven2/](https://repo1.maven.org/maven2/) — the default source for all open-source Java libraries. Contains millions of artifacts. Maven checks here automatically when a dependency isn't in your local repository.

### Remote / Corporate Repository

Private repositories hosted by organizations using tools like **Nexus** or **Artifactory**. Used to store:
- Internal/proprietary libraries
- Mirrored dependencies for air-gapped environments
- Your own published artifacts

```
Resolution order when Maven needs a dependency:
  Local (~/.m2)  →  Remote/Corporate  →  Maven Central
```

---

## 🔗 How It All Fits Together

Here is the complete flow of a `mvn clean install` execution:

```
  You run: mvn clean install
          │
          ▼
  1. Maven reads pom.xml
          │
          ▼
  2. Maven checks local repo (~/.m2) for all declared dependencies
          │
          ├── Found locally? ──▶ Use cached version
          │
          └── Not found? ──▶ Download from Maven Central / Remote repo
                                        │
                                        ▼
                              Save to ~/.m2 for future use
          │
          ▼
  3. clean lifecycle:
     maven-clean-plugin deletes target/
          │
          ▼
  4. default lifecycle runs in order:
     validate → compile → test → package → install
          │           │        │        │        │
          │     compiler    surefire   jar    install
          │      plugin      plugin   plugin   plugin
          │
          ▼
  5. Output: target/your-app-1.0.jar
             + artifact installed to ~/.m2
```

---

<div align="center">

*Part of the [Apache Maven Zero to Hero](../README.md) course*

</div>
