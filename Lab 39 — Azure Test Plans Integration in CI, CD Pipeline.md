# Lab 39 — Azure Test Plans Integration in CI/CD Pipeline

> [!IMPORTANT]
> This lab demonstrates how to integrate Azure Test Plans with Azure DevOps CI/CD pipelines. You will create test plans, connect automated tests with test cases, and publish automated test execution results during pipeline runs.

---

# Objective

Integrate testing into the CI/CD workflow by:

* 🧪 Creating Azure Test Plans.
* 📋 Managing test suites and test cases.
* 🔗 Linking automated tests with test cases.
* 🚀 Publishing automated test results from pipelines.
* 📊 Reviewing test execution reports.

---

# Prerequisites

Before starting, ensure:

* ✅ Azure DevOps project available.
* ✅ CI/CD pipeline configured.
* ✅ Automated test framework available.
* ✅ Test execution output generated.
* ✅ Required Azure DevOps permissions enabled.

---

# Test Integration Workflow Overview

```text id="8m3q7v"
Developer Commit
        |
        ↓
Azure Pipeline Trigger
        |
        ↓
Build Application
        |
        ↓
Execute Automated Tests
        |
        ↓
Publish Test Results
        |
        ↓
Azure Test Plans
        |
        ↓
Test Reports & Analytics
```

---

# Part 1 — Create Azure Test Plan

## Step 1 — Create Test Plan

Navigate:

```text id="5q8m2v"
Azure DevOps
        ↓
Test Plans
        ↓
New Test Plan
```

Create:

```text id="9v4m3q"
Sprint-1-Tests
```

Purpose:

* Organize testing activities.
* Track validation progress.
* Link manual and automated tests.

---

# Part 2 — Create Test Suite and Test Cases

## Step 2 — Add Test Suite

Create:

```text id="2m7q5v"
Test Suite
```

Add:

```text id="6q8m3v"
Test Cases
```

Each test case should include:

* Test scenario.
* Execution steps.
* Expected results.
* Validation criteria.

Example:

```text id="7m4q9v"
Test Case:
Login Functionality

Step 1:
Open application login page

Expected:
Login page loads successfully


Step 2:
Enter valid credentials

Expected:
User authentication succeeds
```

---

# Part 3 — Link Automated Tests

## Step 3 — Associate Automated Tests with Test Cases

Link automated tests using:

```text id="4q9m2v"
Test Method Attribute
```

Purpose:

* Connects code-based tests with Azure Test Plans.
* Enables automated test tracking.
* Provides execution history.

Workflow:

```text id="8v2m5q"
Automated Test Code
        |
        ↓
Test Method Attribute
        |
        ↓
Azure Test Case
```

---

# Part 4 — Add Test Execution to Pipeline

## Step 4 — Update Pipeline YAML

Add automated test publishing tasks:

```yaml id="3m7q8v"
- task: VSTest@2

  inputs:

    testSelector: 'testAssemblies'

    testAssemblyVer2: '**\*test*.dll'

    platform: '$(buildPlatform)'

    configuration: '$(buildConfiguration)'

    publishRunAttachments: true


- task: PublishTestResults@2

  inputs:

    testResultsFormat: 'JUnit'

    testResultsFiles: 'test-results/**/*.xml'

    mergeTestResults: true

    testRunTitle: 'Automated Tests - $(Build.BuildNumber)'
```

---

# Pipeline Test Task Explanation

## VSTest Task

Task:

```yaml id="6p8m3q"
VSTest@2
```

Purpose:

* Executes automated tests.
* Supports Visual Studio test assemblies.
* Publishes execution attachments.

---

## Publish Test Results Task

Task:

```yaml id="9m5q2v"
PublishTestResults@2
```

Purpose:

* Uploads test reports.
* Displays test results in Azure DevOps.
* Links results with pipeline runs.

Configuration:

| Setting             | Purpose                   |
| ------------------- | ------------------------- |
| Test Results Format | Defines report format     |
| Test Results Files  | Location of test reports  |
| Merge Results       | Combines multiple reports |
| Test Run Title      | Identifies execution run  |

---

# Part 5 — Review Automated Test Results

## Step 5 — View Test Execution Results

After pipeline execution:

Navigate:

```text id="5v8m2q"
Azure DevOps
        ↓
Test Plans
        ↓
Runs
        ↓
Automated Test Results
```

Review:

* Passed tests.
* Failed tests.
* Execution duration.
* Linked test cases.
* Attachments.

---

# CI/CD Testing Lifecycle

```text id="3q7m9v"
Code Change
     |
     ↓
Build
     |
     ↓
Unit Tests
     |
     ↓
Integration Tests
     |
     ↓
Publish Results
     |
     ↓
Release Approval
     |
     ↓
Deployment
```

---

# Best Practice Tips

> [!TIP]
> Follow these practices to maintain reliable automated testing.

---

## 📊 Enforce Test Coverage Quality Gates

Configure pipeline rules:

```text id="8m5q3v"
Test Coverage Threshold
        |
        ↓
Pipeline Validation
```

Benefits:

* Prevents quality regression.
* Ensures sufficient code coverage.
* Improves release confidence.

---

## 🐞 Track Test Flakiness

Use:

```text id="2v7m9q"
Test Plans Analytics
```

Monitor:

* Frequently failing tests.
* Unstable test environments.
* Timing-related failures.

Recommendation:

```text id="6q4m8v"
Fix flaky tests before adding more tests.
```

---

## ⚡ Use Test Impact Analysis

Enable:

```text id="9m3q5v"
Test Impact Analysis
```

Benefits:

* Runs only affected tests.
* Reduces pipeline execution time.
* Improves developer productivity.

---

## 📄 Export Test Reports

Generate:

```text id="4m8q2v"
PDF Test Reports
```

Use for:

* Release approvals.
* Stakeholder reviews.
* Compliance documentation.

---

# Validation Checklist

Verify:

* ✅ Test Plan created.
* ✅ Test Suite configured.
* ✅ Test Cases added.
* ✅ Automated tests linked.
* ✅ Pipeline executes test tasks.
* ✅ Test results published.
* ✅ Azure Test Plans displays execution history.

---

# Summary

In this lab, you:

* Created Azure Test Plans.
* Added test suites and test cases.
* Integrated automated tests with CI/CD.
* Published test execution results.
* Learned testing best practices for enterprise delivery.

This integration provides a complete quality validation workflow from code commit through automated testing and release readiness.
