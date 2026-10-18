# Modernization Plan: Java 21 and Spring Boot 4.0.8 Upgrade

**Project**: gs-accessing-data-jpa

## Technical Framework

- **Language**: Java 17; target Java 21
- **Framework**: Spring Boot 3.5.3; target Spring Boot 4.0.8
- **Build Tools**: Maven Wrapper and Gradle Wrapper 8.14.3
- **Database**: H2
- **Key Dependency**: Spring Data JPA
- **Scope**: `complete/` only; review the GitHub Actions CI workflow

## Overview

Upgrade the `complete/` sample to the two explicitly requested targets while preserving its existing behavior and tests. Keep Maven and Gradle build paths aligned, first establishing whether the requested Java, Spring Boot, and Gradle versions are mutually compatible. Review CI coverage for Java 21 and both build systems. This plan describes work for approval only; no implementation is included in this update.

## Assessment Context

- **Report**: `.github/modernize/assessment/reports/report-20261018220828/report.json`
- **Issue**: `Java Version is not the latest LTS` (`azure-java-version-02000`)
- **Assessment location in scope**: `complete/pom.xml`
- The report also flags `initial/` files; those findings are out of scope and will not be addressed.
- The report identifies Gradle, and repository configuration confirms Maven and Gradle builds are both present in `complete/`.

## Scope and Planned Review

- Target Java 17 to Java 21 and Spring Boot 3.5.3 to Spring Boot 4.0.8; do not substitute different target versions without the user's decision.
- Investigate Spring Boot 4.0.8's supported Java versions, Spring Framework/Jakarta requirements, dependency alignment, and any application compatibility impact before proposing changes.
- Assess Gradle 9 compatibility, including the Gradle Wrapper distribution and Spring Boot Gradle Plugin version compatibility; identify deprecated or incompatible Gradle configuration and evaluate `sourceCompatibility` versus Java Toolchain configuration.
- Review Maven Parent/plugin/dependency compatibility and Maven Wrapper-based validation requirements.
- Review `.github/workflows/continuous-integration-build.yml` and its reusable workflow to determine whether Java 21 and both Maven and Gradle builds are actually tested. Record any upstream reusable-workflow limitation and the smallest required CI follow-up.
- After approval and only after compatibility prerequisites are resolved, make the minimum necessary changes within the approved scope, then validate both wrappers and tests and report CI verification/results.
- Do not modify `initial/`, other samples, generated build output, or unrelated files.

## Validation After Approval

1. Verify the requested target versions against authoritative Spring Boot, Gradle, plugin, Java, and dependency compatibility information. Resolve any hard incompatibility before editing; preserve the requested versions and ask the user rather than silently changing either target.
2. Run the Maven Wrapper build and tests for `complete/` on Java 21.
3. Run the Gradle Wrapper build and tests for `complete/` on Java 21, review deprecation/incompatibility warnings, and confirm the selected Gradle 9 and Spring Boot Gradle Plugin combination.
4. Verify GitHub Actions CI covers Java 21 and both build systems; if the reusable workflow prevents verification or requires a change, report that limitation and proposed minimum follow-up.

## Compatibility Gate / Open Question

- The repository's Java upgrade guideline maps Spring Boot 4.x to Java 25 (and lists Java 25 as both the minimum and maximum), which conflicts with the explicitly requested Java 21 + Spring Boot 4.0.8 pair. This guideline is a planning warning, not authorization to change either target. During execution, verify the exact Spring Boot 4.0.8 requirements from authoritative release documentation. If Java 21 is unsupported, stop before implementation and request the user's decision on the incompatible targets.
- Confirm the exact Gradle 9 minimum supported version and plugin compatibility for Spring Boot 4.0.8 before selecting/updating the wrapper. If that combination is not supported, report the constraint and request direction; do not silently pick a different framework target.
- The GitHub Actions workflow delegates to an external reusable workflow, so its Java/build matrix may not be visible in this repository. Verify the referenced workflow and record any inability to validate or change it.

## Approval Boundary

This is a revised plan only. Wait for the user's approval before changing application sources, Maven/Gradle build settings or wrappers, or CI files.