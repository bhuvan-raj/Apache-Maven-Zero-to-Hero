<img src="https://github.com/bhuvan-raj/Apache-Maven-Zero-to-Hero/blob/main/assets/maven.png" alt="Banner" />

<div align="center">

![Maven](https://img.shields.io/badge/Apache%20Maven-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)

# What is Apache Maven?

> **"Accumulator of knowledge" — a build tool that manages your entire Java project lifecycle so you don't have to.**

</div>

---

## 📋 Table of Contents

- [Introduction](#-introduction)
- [Key Concepts](#-key-concepts)
- [Maven vs Ant — Why Maven Won](#-maven-vs-ant--why-maven-won)
- [Summary](#-summary)

---

## 💡 Introduction

**Apache Maven** is a powerful **build automation** and **project management tool** primarily used for Java projects. It was created to address the growing complexity of managing dependencies and standardizing build processes across large Java codebases.

The name **Maven** means *"accumulator of knowledge"* in Yiddish — and that reflects its purpose precisely: to centralize all the knowledge about your project's build, dependencies, reporting, and documentation into a single source of truth called the `pom.xml`.

Before Maven existed, Java developers had to manually download JAR files, track versions, write custom build scripts for every project, and deal with the chaos of dependency conflicts. Maven solved all of this.

---

## 🔑 Key Concepts

Maven is built on four foundational ideas that define how it works:

### Convention over Configuration

Maven provides **sensible defaults** for project structure and build processes. You don't write lengthy scripts explaining *how* to compile — Maven already knows. As long as you follow the standard layout, everything works out of the box.

```
Standard Maven Project Structure:

your-project/
├── pom.xml                        ← Project configuration
├── src/
│   ├── main/
│   │   └── java/                  ← Application source code
│   │       └── com/example/
│   │           └── App.java
│   └── test/
│       └── java/                  ← Test source code
│           └── com/example/
│               └── AppTest.java
└── target/                        ← Build output (auto-generated)
    ├── classes/
    └── your-app-1.0.jar
```

Any Maven developer who opens your project for the first time immediately knows where everything lives — no documentation needed.

---

### Project Object Model (POM)

The **`pom.xml`** is the heart of every Maven project. It is a single XML file that describes:

- What your project is (name, version, group)
- What it depends on (external libraries)
- How it should be built (plugins, goals, configurations)
- Where to deploy it (repositories)

Everything Maven does flows from reading this file.

---

### Build Lifecycles

Maven defines **standardized lifecycles** — ordered sequences of phases that represent the build process. When you tell Maven to run a phase, it automatically runs every phase that comes before it too.

For example, running `mvn package` automatically triggers: `validate → compile → test → package`.

---

### Repositories

Maven uses **repositories** as centralized storage for project dependencies and artifacts. When your project needs a library, Maven fetches it from a repository and caches it locally — you never manually download JARs again.

---

## ⚖️ Maven vs Ant — Why Maven Won

Before Maven, **Apache Ant** was the dominant Java build tool. Ant used a *procedural* approach — you had to explicitly write every step of your build process as XML tasks. Maven introduced a *declarative* model where you describe *what* your project needs and Maven figures out *how* to build it.

| Feature | Apache Maven | Apache Ant |
|---------|-------------|-----------|
| **Approach** | Declarative / Model-based | Procedural / Task-based |
| **Dependency Management** | Automatic — resolves and downloads transitive dependencies | Manual — you download and manage every JAR yourself |
| **Project Structure** | Standardized — every Maven project looks the same | Custom — you define your own directory layout and file paths |
| **Build Process** | Pre-defined lifecycle with automatic phase chaining | You write every build step explicitly |
| **Configuration Effort** | Minimal — convention handles most things | High — extensive scripting required for standard tasks |
| **Reporting** | Automatic — generates test reports, dependency lists, docs | Manual — requires extra tools and tasks |
| **Learning Curve** | Moderate (understand conventions) | High (write everything from scratch) |
| **Reusability** | High — standard lifecycle is reused across all projects | Low — scripts are often project-specific |

**The bottom line:** In Ant, you could spend days writing build scripts just to compile, test, and package a Java app. In Maven, you create a `pom.xml` with your dependencies, run `mvn package`, and it's done — Maven handles the rest.

---

## 📌 Summary

Apache Maven is the standard build tool for Java projects because it:

- **Eliminates manual JAR management** by automatically resolving and downloading dependencies
- **Standardizes project structure** so every developer is immediately at home in any Maven project
- **Defines a consistent build lifecycle** — you don't write build scripts, you just declare what you need
- **Integrates with everything** — Jenkins, GitHub Actions, Nexus, SonarQube, Docker, and more all have native Maven support

---

<div align="center">

*Part of the [Apache Maven Zero to Hero](../README.md) course*

</div>
