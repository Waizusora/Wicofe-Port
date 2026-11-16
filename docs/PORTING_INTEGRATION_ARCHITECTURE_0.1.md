# Wicofe Ports and Upstream Integration Architecture 0.1

**Status:** Draft architecture. This document defines how external software is evaluated, built, patched and maintained for Wicofe. It does not claim that any port is complete or approved.

## 1. Purpose

Wicofe-Ports maintains reproducible recipes and narrowly scoped integration patches for external software intended to build for or run on Wicofe. It bridges upstream projects and Wicofe's target ABI, SDK and runtime without becoming a package manager or a second third-party compliance registry.

A port is an integration unit with an exact upstream source identity, build procedure, patch set, feature configuration, target support declaration and validation evidence.

## 2. Ownership boundaries

| Repository | Canonical responsibility |
|---|---|
| Wicofe-Ports | Port recipes, integration patches, upstream tracking, build instructions and port status |
| Wicofe-ThirdParty | Canonical source provenance, license/notice review, dependency compliance and approval records |
| Wicofe-SDK | Compiler/linker distribution, sysroots, target profiles and developer toolchain |
| Wicofe-Build | Whole-system dependency graph, build orchestration, caching and release build provenance |
| WPM | Binary/source package metadata, package repository and install/remove/upgrade transactions |
| Wicofe-ABI | Canonical syscall and userspace ABI definitions |
| Wicofe-Libc / Wicofe-Lib | C runtime and shared Rust userspace libraries |
| Wicofe-Tests | Cross-project test suites and integration validation |
| Component repositories | Their own in-tree dependencies and component-specific build/test configuration |

Wicofe-Ports owns how an upstream project is integrated. Wicofe-ThirdParty owns whether the exact source and license obligations have been reviewed. WPM owns package lifecycle after a built artifact is packaged. These responsibilities must not be duplicated.

## 3. Port lifecycle

Suggested states:
1. **Candidate:** upstream project identified; no compatibility or license claim.
2. **Intake:** exact source, license evidence and dependency inventory being recorded.
3. **Buildable:** recipe builds for a declared host/target configuration.
4. **Patched:** Wicofe-specific patch set applied and documented.
5. **Validated:** required compile, link, runtime and regression tests pass for declared targets.
6. **Approved for packaging:** third-party review and technical validation complete for a specific version/configuration.
7. **Maintained:** updates, security advisories and upstream changes actively tracked.
8. **Deprecated/Removed:** no longer recommended or shipped; history retained.

A successful host build does not imply target compatibility. A successful cross-compile does not prove the program can execute on Wicofe. A port must report the exact validation level.

## 4. Port recipe contract

Each recipe should declare:
- stable port identifier and recipe schema version;
- upstream project URL and exact release/commit;
- immutable source reference and archive checksum;
- patch series and patch ordering;
- host build dependencies and target build dependencies separately;
- supported target architecture/profile and CPU baseline;
- compiler, linker and SDK version constraints;
- configure/build options and enabled/disabled features;
- environment variables and deterministic build settings;
- output artifacts, install paths and runtime dependencies;
- test commands and expected results;
- third-party component record reference;
- license/notice handling and package handoff metadata.

Recipes must not use mutable branch heads for release builds. Unpinned dependencies, undeclared network downloads, host-path leakage and ambient environment dependencies must fail release validation.

## 5. Source intake and provenance

Before integration, create or reference a Wicofe-ThirdParty component record containing exact upstream identity, source version/commit, archive digest, license files, notices, dependency inventory, enabled features, patch context and security status.

Ports must link to that record rather than copying the canonical compliance decision. A changed upstream version, source archive, feature set, patch set or distribution context requires reassessment.

Never infer a license solely from package registry metadata, a repository badge or a file extension. Preserve upstream copyright and notice files in source and packaged outputs as required.

## 6. Build model and cross-compilation

Recipes must distinguish:
- **Build host:** machine executing compiler and build tools.
- **Host tools:** generators and utilities executed during the build.
- **Target:** architecture/ABI for which the output is produced.
- **Target runtime:** libraries and services available when the program executes on Wicofe.

Use Wicofe-SDK target profiles and sysroots. Never allow host headers, libraries, pkg-config results or linker search paths to contaminate target outputs.

Build dependencies that execute on the build host must be compiled for the host; libraries linked into the final program must be built for the target. When upstream build systems conflate these roles, document and test the cross-compilation fix.

Initial project architecture targets x86-64 and AArch64. Each port must declare support independently; do not infer support for both architectures from a generic cross-build.

## 7. Patch policy

Prefer minimal, reviewable patches that can be submitted upstream. Each patch must include:
- rationale and affected upstream version;
- affected files and behavior;
- target-specific scope;
- security and compatibility impact;
- upstream submission URL/status where applicable;
- license/notice implications;
- regression test evidence.

Avoid carrying patches that silently disable security checks, change cryptographic behavior, weaken memory safety or bypass Wicofe ABI boundaries. Such changes require explicit review and a documented threat/compatibility rationale.

Keep generated patches reproducible and separate mechanical formatting changes from semantic modifications.

## 8. Compatibility and feature policy

A port must list required kernel ABI, libc, Rust runtime (if applicable), shared libraries, services, filesystem features and hardware interfaces.

Disable optional upstream features that cannot be supported or validated. Do not claim POSIX compatibility merely because a project builds against POSIX-like headers. Document unsupported syscalls, signals, threading, sockets, locale, dynamic linking and other runtime assumptions.

For Rust software, distinguish no_std-compatible libraries, host-side Rust tools and target std applications. Native Wicofe std support must not be assumed before SDK/runtime prerequisites are complete.

## 9. Output and package handoff

Wicofe-Ports produces buildable source integration and artifacts suitable for packaging. It does not define WPM package format or repository semantics.

Handoff metadata should include:
- port ID and exact upstream version;
- source and patch digests;
- target architecture/ABI;
- build recipe and toolchain identity;
- output artifact hashes;
- runtime dependency declarations;
- license/notice references;
- validation report references.

WPM consumes approved outputs and package metadata. Rebuilding the same port with changed inputs must produce a distinct build identity and updated provenance.

## 10. Reproducibility and supply-chain security

Pin source archives, patches, host tools, SDK versions and all build dependencies. Verify checksums/signatures where available. Build in isolated workspaces with bounded resources and no unnecessary credentials.

Record environment inputs that affect output. Normalize timestamps, locale, timezone, file ordering and build paths where possible. Compare independent builds and report nondeterministic fields rather than hiding them.

Do not execute upstream build scripts with elevated host privileges. Treat configure scripts, code generators, package metadata and vendored sources as untrusted inputs.

## 11. Testing and validation levels

Report these levels separately:
- **L0 — Intake:** source and compliance records exist.
- **L1 — Recipe:** build recipe parses and dependencies resolve.
- **L2 — Cross-build:** target objects and binaries compile/link.
- **L3 — Static validation:** ABI, symbols, dependencies, file layout and security checks pass.
- **L4 — Runtime:** program executes and tests pass on Wicofe or an explicitly supported emulator/runtime.
- **L5 — Release:** provenance, license inventory, package metadata and release reproducibility checks pass.

A level must not be inferred from a lower level. If no Wicofe runtime is available, report the port as cross-built but not runtime-validated.

Required test coverage includes recipe schema, source digest mismatch, patch applicability, host contamination, architecture mismatch, undeclared dependency, deterministic rebuild, package metadata and runtime regression tests where feasible.

## 12. Maintenance and upstream tracking

Track upstream release cadence, supported branches, end-of-life status, security advisories, patch divergence and local maintainer. Define update triggers and deprecation criteria.

On every upstream update:
1. verify new source provenance and license files;
2. regenerate dependency/feature inventory;
3. rebase and review patches;
4. rebuild each declared target;
5. rerun tests and security checks;
6. update third-party review and package handoff metadata.

Do not silently replace a source version under an unchanged port revision.

## 13. Suggested repository organization

```text
ports/
  <port-id>/
    port.toml
    patches/
    files/
    tests/
docs/
  INDEX.md
  PORT_RECIPE_SCHEMA_0.1.md
  PORT_STATUS.md
```

This is a suggested structure, not a statement that these files or ports already exist. The machine-readable recipe format and schema version remain open.

## 14. Open decisions

- Canonical recipe serialization and schema evolution.
- Port identifier and version/revision naming policy.
- Source mirror and cache ownership.
- Build sandbox and network policy.
- Target sysroot and dependency resolver interface.
- Patch submission and maintenance expectations.
- Runtime test environment and emulator support.
- Port status/approval authority and review cadence.
- WPM package handoff contract.
- Security advisory triage and deprecation SLA.

## 15. Completion criteria

A production port requires immutable source identity, reviewed third-party record, pinned reproducible recipe, explicit target/runtime requirements, documented patch series, architecture-specific build evidence, runtime tests for any claimed runtime support, and complete package/provenance handoff. Until those exist, a port remains a candidate or draft integration.
