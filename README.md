# Java Maven Build

![Latest Release](https://img.shields.io/github/v/release/p6m-actions/java-maven-build?style=flat-square&label=Latest%20Release&color=blue)

## Description

A composite GitHub Action for building Java projects using Maven with configurable lint, test, build, and package phases. This action provides flexible command customization while maintaining sensible defaults for Java Maven projects.

## Usage

```yaml
- name: Build Java Maven Project
  uses: p6m-actions/java-maven-build@v1
  with:
    run-lint: true
    run-test: true
    run-build: true
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `run-lint` | Whether to run linting | No | `true` |
| `lint-command` | Command to run for linting | No | `mvn checkstyle:check` |
| `lint-args` | Additional arguments for the lint command | No | `""` |
| `run-test` | Whether to run tests | No | `true` |
| `test-command` | Command to run for testing | No | `mvn test` |
| `test-args` | Additional arguments for the test command | No | `""` |
| `run-build` | Whether to run build | No | `true` |
| `build-command` | Command to run for building | No | `mvn compile` |
| `build-args` | Additional arguments for the build command | No | `""` |
| `run-package` | Whether to run package | No | `false` |
| `package-command` | Command to run for packaging | No | `mvn package` |
| `package-args` | Additional arguments for the package command | No | `""` |
| `maven-options` | Maven options for all processes | No | `-B -q` |
| `archive-coverage` | Whether to archive code coverage results | No | `false` |
| `coverage-path` | Path to the coverage reports | No | `target/site/jacoco` |

## Outputs

This action does not provide any outputs.

## Examples

### Basic Usage

```yaml
- name: Setup Java and Maven
  uses: p6m-actions/java-maven-setup@v1
  with:
    java-version: '21'
    cache: true

- name: Build Java Project
  uses: p6m-actions/java-maven-build@v1
```

### Custom Commands

```yaml
- name: Build with Custom Commands
  uses: p6m-actions/java-maven-build@v1
  with:
    lint-command: 'mvn spotbugs:check'
    test-command: 'mvn test'
    test-args: '-Dtest.profile=ci'
    build-command: 'mvn clean compile'
    run-package: true
    package-args: '-DskipTests'
```

### Skip Phases

```yaml
- name: Build without Linting
  uses: p6m-actions/java-maven-build@v1
  with:
    run-lint: false
    run-test: true
    run-build: true
```

### With Coverage Archival

```yaml
- name: Build with Coverage
  uses: p6m-actions/java-maven-build@v1
  with:
    archive-coverage: true
    coverage-path: 'target/site/jacoco'
```

### Complete Workflow Example

```yaml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Java and Maven
      uses: p6m-actions/java-maven-setup@v1
      with:
        java-version: '21'
        cache: true
    
    - name: Build and Test
      uses: p6m-actions/java-maven-build@v1
      with:
        run-lint: true
        run-test: true
        run-build: true
        run-package: true
        archive-coverage: true
```