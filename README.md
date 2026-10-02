# Java Maven CI Pipeline

![Java](https://img.shields.io/badge/Java-17-orange)
![Maven](https://img.shields.io/badge/Maven-Build-blue)
![JUnit](https://img.shields.io/badge/JUnit-5-green)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-CI-black)

A hands-on Continuous Integration project demonstrating how to build, test, and package a Java application automatically using **Java 17, Maven, JUnit 5, Git, GitHub, and GitHub Actions**.

## Project Overview

This project contains a simple calculator application with:

- Addition
- Subtraction
- Multiplication
- Division
- Division-by-zero validation

JUnit 5 provides automated unit tests. Maven manages dependencies, compilation, testing, and packaging. GitHub Actions automates the CI workflow on pushes to `main` and pull requests targeting `main`.

## CI Architecture

```text
Developer
   |
   | git push
   v
GitHub Repository
   |
   v
GitHub Actions
   |
   v
Ubuntu Runner
   |
   +--> Checkout source
   +--> Set up Java 17
   +--> Configure Maven cache
   +--> mvn clean test
   |       +--> Compile
   |       +--> Run 5 JUnit tests
   |
   +--> mvn package -DskipTests
           |
           v
       Build complete
```

## Technologies

| Technology | Purpose |
|---|---|
| Java 17 | Application development |
| Maven | Build and dependency management |
| JUnit 5 | Unit testing |
| Git | Version control |
| GitHub | Source-code hosting |
| GitHub Actions | Continuous Integration |
| Ubuntu | CI runner |

## Project Structure

```text
github-actions-java-ci/
├── .github/
│   └── workflows/
│       └── java-ci.yml
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com/example/calculator/
│   │           └── Calculator.java
│   └── test/
│       └── java/
│           └── com/example/calculator/
│               └── CalculatorTest.java
├── .gitignore
├── pom.xml
└── README.md
```

The Maven-generated `target/` directory is excluded through `.gitignore`.

## Application

`Calculator.java` implements:

```java
add()
subtract()
multiply()
divide()
```

The `divide()` method throws `IllegalArgumentException` when the divisor is zero.

## Unit Tests

`CalculatorTest.java` contains five tests:

| Test | Purpose |
|---|---|
| `testAdd()` | Validates addition |
| `testSubtract()` | Validates subtraction |
| `testMultiply()` | Validates multiplication |
| `testDivide()` | Validates division |
| `testDivideByZero()` | Validates exception handling |

Local validation was completed with:

```bash
mvn test
```

Result:

```text
Tests run: 5, Failures: 0, Errors: 0, Skipped: 0
BUILD SUCCESS
```

## Maven Configuration

The `pom.xml` defines:

- Java 17 compiler settings
- JUnit 5 dependency
- Maven Surefire Plugin
- UTF-8 source encoding
- Maven project coordinates

```text
groupId:    com.example
artifactId: calculator-ci
version:    1.0-SNAPSHOT
```

## GitHub Actions Workflow

The workflow is:

```text
.github/workflows/java-ci.yml
```

It runs for:

```yaml
push:
  branches:
    - main

pull_request:
  branches:
    - main
```

### 1. Checkout

```yaml
- name: Checkout source code
  uses: actions/checkout@v5
```

### 2. Set up Java

```yaml
- name: Set up Java
  uses: actions/setup-java@v5
  with:
    distribution: temurin
    java-version: '17'
    cache: maven
```

### 3. Build and Test

```yaml
- name: Build and test with Maven
  run: mvn clean test
```

This cleans previous output, compiles the source and test code, resolves dependencies, and executes the JUnit tests.

### 4. Package

```yaml
- name: Package application
  run: mvn package -DskipTests
```

Tests have already run successfully in the previous step, so this step packages the application without running the tests a second time.

## Successful CI Execution

The GitHub Actions workflow was successfully executed.

```text
Workflow: Java Maven CI Pipeline
Job:      build-and-test
Status:   Success
Runner:   ubuntu-latest
Tests:    5
Failures: 0
Errors:   0
```

GitHub may display an informational notice about a future `ubuntu-latest` runner migration. This is a notice and does not indicate a failure in the pipeline.

## Local Setup

### Prerequisites

- Java JDK 17
- Maven
- Git

Verify:

```bash
java --version
javac --version
mvn --version
```

### Clone

```bash
git clone https://github.com/srinivasupolamuri/github-actions-java-ci.git
cd github-actions-java-ci
```

### Run Tests

```bash
mvn test
```

### Clean and Test

```bash
mvn clean test
```

### Package

```bash
mvn package
```

Generated Maven output is placed in:

```text
target/
```

and is intentionally ignored by Git.

## Git Workflow

```bash
git init
git branch -M main
git add .
git commit -m "Add Java Maven CI pipeline"
git remote add origin https://github.com/srinivasupolamuri/github-actions-java-ci.git
git push -u origin main
```

For later changes:

```bash
git add .
git commit -m "Update Java CI project"
git push
```

## CI Flow

```text
Java Source
    ↓
Git Commit
    ↓
Git Push
    ↓
GitHub
    ↓
GitHub Actions
    ↓
Java 17
    ↓
Maven
    ↓
Compile
    ↓
JUnit 5 Tests
    ↓
Package
    ↓
Success / Failure
```

## Key DevOps Concepts Demonstrated

### Continuous Integration
Every push to `main` automatically starts the pipeline.

### Automated Testing
JUnit tests validate the application without requiring manual test execution.

### Build Automation
Maven handles compilation, dependencies, testing, and packaging.

### CI Runner
GitHub-hosted Ubuntu provides the environment in which the workflow executes.

### Fail-Fast Feedback
A failed test causes the CI job to fail, providing quick feedback about a code change.

## Troubleshooting

### Java not found

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

### Maven not found

```bash
sudo apt update
sudo apt install maven -y
```

### Test failures

Run:

```bash
mvn clean test
```

Maven test reports are available under:

```text
target/surefire-reports/
```

### GitHub Actions failure

Open:

```text
GitHub Repository → Actions → Java Maven CI Pipeline → build-and-test
```

Then inspect the failed step and its logs.

## Interview Explanation

> I developed a Java Maven CI project to practice Continuous Integration using GitHub Actions. I created a calculator application with JUnit 5 unit tests and configured Maven for dependency management, compilation, testing, and packaging. The GitHub Actions workflow runs on an Ubuntu runner, checks out the source code, configures Java 17, executes `mvn clean test`, and packages the application using Maven. The pipeline successfully executed all five tests with zero failures.

## Skills Demonstrated

**Java:** Java 17, packages, methods, exception handling

**Maven:** `pom.xml`, dependencies, lifecycle, compilation, testing, packaging

**Testing:** JUnit 5, assertions, exception testing

**Git/GitHub:** repository initialization, branches, commits, remotes, push

**GitHub Actions:** workflows, jobs, steps, runners, checkout, setup-java, Maven caching, automated testing and builds

## Future Enhancements

This CI project can be extended into a larger DevOps pipeline:

```text
Java
  ↓
Maven Build & Test
  ↓
Docker Image
  ↓
GitHub Container Registry
  ↓
AWS
  ↓
Deployment
```

Possible next enhancements:

- Test multiple Java versions with a matrix strategy
- Add code-quality checks
- Add SonarQube
- Build a Docker image
- Push the image to GitHub Container Registry
- Add deployment automation
- Deploy to AWS

## Project Status

**Completed — Java Maven Continuous Integration pipeline successfully built, tested, packaged, and executed through GitHub Actions.**

## Repository

https://github.com/srinivasupolamuri/github-actions-java-ci

## Author

**Srinivasu Polamuri**

DevOps / IT Support Engineer | Java | Git | GitHub Actions | Maven | AWS | Docker | Terraform | Ansible
