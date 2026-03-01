
<div align="center">

![Maven](https://img.shields.io/badge/Apache%20Maven-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![macOS](https://img.shields.io/badge/macOS-000000?style=for-the-badge&logo=apple&logoColor=white)

# Maven Installation

> **Step-by-step installation guide for Linux (Fedora & Ubuntu), Windows, and macOS.**

</div>

---

## 📋 Table of Contents

- [Prerequisites](#-prerequisites)
- [Linux — Fedora / CentOS / RHEL](#-linux--fedora--centos--rhel)
- [Linux — Ubuntu / Debian](#-linux--ubuntu--debian)
- [Windows](#-windows)
- [macOS](#-macos)
- [Verify Your Installation](#-verify-your-installation)
- [Post-Installation Notes](#-post-installation-notes)

---

## ✅ Prerequisites

Before installing Maven, **Java must be installed** on your machine. Maven requires a JDK (not just a JRE) to compile code.

Verify Java is available:

```bash
java -version
```

Expected output:

```
openjdk version "17.0.x" ...
```

If Java is not installed, install it first using the instructions for your OS below — each section includes the Java install step.

---

## 🐧 Linux — Fedora / CentOS / RHEL

The easiest method on Red Hat-based systems is **DNF** — it installs Maven and handles all environment variables automatically.

### Step 1 — Install Java

```bash
sudo dnf install java-17-openjdk-devel -y
```

### Step 2 — Install Maven

```bash
sudo dnf install maven -y
```

### Step 3 — Verify

```bash
mvn -v
```

Expected output:

```
Apache Maven 3.x.x (...)
Maven home: /usr/share/maven
Java version: 17.x.x, vendor: Red Hat, Inc.
```

---

## 🐧 Linux — Ubuntu / Debian

Use **APT** for a quick, managed Maven installation on Debian-based systems.

### Step 1 — Update Package Index

```bash
sudo apt update
```

### Step 2 — Install Java

```bash
sudo apt install openjdk-17-jdk -y
```

### Step 3 — Install Maven

```bash
sudo apt install maven -y
```

### Step 4 — Verify

```bash
mvn -v
```

Expected output:

```
Apache Maven 3.x.x (...)
Maven home: /usr/share/maven
Java version: 17.x.x
```

---

## 🖥️ Windows

Windows requires a manual installation since there is no built-in package manager. Follow each step carefully.

### Step 1 — Install JDK

Download and install JDK 17 (or later) from [https://adoptium.net](https://adoptium.net). During installation, check the option to **set `JAVA_HOME` automatically** if offered.

Verify after installation (open a new Command Prompt):

```bat
java -version
```

### Step 2 — Set JAVA_HOME (if not set automatically)

1. Press `Win + S` → search **Environment Variables** → click **Edit the system environment variables**
2. Click **Environment Variables**
3. Under **System variables**, click **New**
4. Set:
   - **Variable name:** `JAVA_HOME`
   - **Variable value:** Path to your JDK (e.g., `C:\Program Files\Eclipse Adoptium\jdk-17`)
5. Click **OK**

### Step 3 — Download Maven

Go to [https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi) and download the **Binary zip archive** — e.g., `apache-maven-3.9.x-bin.zip`.

### Step 4 — Extract Maven

Extract the ZIP to a clean location. Recommended:

```
C:\Maven\apache-maven-3.9.x\
```

### Step 5 — Set M2_HOME Environment Variable

1. Open **Environment Variables** (same as Step 2)
2. Under **System variables**, click **New**
3. Set:
   - **Variable name:** `M2_HOME`
   - **Variable value:** `C:\Maven\apache-maven-3.9.x`
4. Click **OK**

### Step 6 — Add Maven to PATH

1. In **System variables**, find and select `Path` → click **Edit**
2. Click **New** and add: `%M2_HOME%\bin`
3. Click **OK** on all dialogs

### Step 7 — Verify

Open a **new** Command Prompt or PowerShell window (important — existing windows won't see the updated PATH):

```bat
mvn -v
```

Expected output:

```
Apache Maven 3.x.x (...)
Maven home: C:\Maven\apache-maven-3.9.x
Java version: 17.x.x
```

---

## 🍎 macOS

The simplest way to install Maven on macOS is via **Homebrew** — the most popular macOS package manager.

### Step 1 — Install Homebrew (if not already installed)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Step 2 — Install Java

```bash
brew install openjdk@17
```

After installation, Homebrew will print a message to link Java. Run it:

```bash
sudo ln -sfn $(brew --prefix openjdk@17)/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-17.jdk
```

### Step 3 — Install Maven

```bash
brew install maven
```

Homebrew automatically sets up the required environment variables.

### Step 4 — Verify

```bash
mvn -v
```

Expected output:

```
Apache Maven 3.x.x (...)
Maven home: /opt/homebrew/Cellar/maven/3.x.x/libexec
Java version: 17.x.x
```

---

## ✅ Verify Your Installation

Regardless of OS, run these two commands to confirm everything is set up correctly:

```bash
# Check Maven version and Java version it's using
mvn -v

# Check Maven can find your project (run from inside a Maven project)
mvn validate
```

If `mvn -v` shows both Maven and Java versions, your installation is complete and working.

---

## 📝 Post-Installation Notes

**Always open a new terminal after setting environment variables.** On Windows especially, existing Command Prompt or PowerShell windows will not see newly added PATH entries — you must open a fresh one.

**Package managers handle environment setup automatically.** When using `dnf`, `apt`, or `brew`, Maven's `bin` directory is added to your PATH automatically. Manual installation (Windows) requires setting `M2_HOME` and `PATH` yourself.

**The `~/.m2` directory is created on first use.** The first time you run any Maven command, it creates the `~/.m2/repository/` directory and begins downloading dependencies. This is normal behavior.

**To update Maven later:**

| OS | Command |
|----|---------|
| Fedora / RHEL | `sudo dnf upgrade maven` |
| Ubuntu / Debian | `sudo apt upgrade maven` |
| macOS | `brew upgrade maven` |
| Windows | Download new ZIP, update `M2_HOME` path |

---

<div align="center">

*Part of the [Apache Maven Zero to Hero](../README.md) course*

</div>
