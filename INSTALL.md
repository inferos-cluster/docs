# Inferos Installation Guide

> [!IMPORTANT]
> Inferos is in early development. This guide will evolve as modules are built and released.

## Prerequisites

### Host Operating System

Inferos clusters are designed to run on **Alpine Linux**.

- **Why Alpine:** Minimal footprint (~5MB base), musl libc, security-hardened, and designed for high-performance server workloads.
- **Download:** [alpinelinux.org](https://alpinelinux.org/downloads/)
- **Minimum version:** Alpine 3.18+

## Core Module

Every Inferos cluster **requires** the Station Citadel to function.

| Module              | Code                | Required |
|---------------------|---------------------|----------|
| **Station Citadel** | CLST-MDL-ST-CITADEL | Yes      |

This is the first module that must be installed and running before any other component can connect to the cluster.
