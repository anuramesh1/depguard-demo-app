# depguard-demo-app

> **DISCLAIMER: this repository is intentionally vulnerable.** Its `pom.xml` pins old library versions with
> known CVEs (including Log4Shell, CVE-2021-44228) on purpose. It is test data, not an application. Do not copy
> these versions into real projects.

This repo is the target for [DepGuard](https://github.com/anuramesh1/depguard)'s upgrade workflow:

1. A DepGuard user proposes an upgrade (`POST /api/upgrades`). DepGuard reads this `pom.xml`, triages it, and
   stores the proposed version bumps with a diff. Nothing changes here yet.
2. A *different* user with the MAINTAINER role approves it (`POST /api/upgrades/{id}/approve`).
3. DepGuard then opens a pull request here on a `depguard/upgrade-…` branch. A human reviews and merges it, or not.

DepGuard never pushes to `main` and never merges. Its token is scoped to this repository only.

| Dependency | Pinned version | Why it's here |
|---|---|---|
| `org.apache.logging.log4j:log4j-core` | 2.14.1 | Log4Shell: critical, in CISA's Known Exploited list |
| `com.fasterxml.jackson.core:jackson-databind` | 2.13.0 | Denial-of-service advisories |
| `org.apache.commons:commons-text` | 1.9 | Text4Shell (CVE-2022-42889) |
| `junit:junit` | 4.12 | Test-scope only: shows how DepGuard ranks test dependencies lower |
