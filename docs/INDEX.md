# Ports Documentation Index

This directory documents recipes and patches for integrating external software into Wicofe.

## Architecture
- [Ports and upstream integration architecture 0.1](PORTING_INTEGRATION_ARCHITECTURE_0.1.md) — draft lifecycle for source intake, reproducible recipes, cross-compilation, patches, validation and package handoff.

## Scope
- Reproducible upstream build recipes.
- Wicofe-specific patches and patch rationale.
- Upstream version/commit tracking.
- Dependency metadata, build instructions, and port status.
- License, provenance, and security review references.

## Ownership
[WPM](https://github.com/Waizusora/WPM) owns package installation and repository operations. [Wicofe-Build](https://github.com/Waizusora/Wicofe-Build) owns whole-system build orchestration. [Wicofe-SDK](https://github.com/Waizusora/Wicofe-SDK) owns toolchain and sysroot distribution. [Wicofe-ThirdParty](https://github.com/Waizusora/Wicofe-ThirdParty) owns canonical provenance/license inventory and approval records. Ports own integration recipes and patches; link to compliance records rather than duplicating them.

## Porting policy
Prefer minimal, reviewable, upstreamable patches. Pin exact source versions, checksums, tools and dependencies. Declare architecture/runtime support separately and distinguish cross-build success from runtime validation. Do not redistribute code without reviewing its exact terms.

- [Central documentation index](https://github.com/Waizusora/Wicofe-Docs/blob/main/INDEX.md)
- [Repository documentation map](https://github.com/Waizusora/Wicofe-Docs/blob/main/REPOSITORY_DOCUMENTATION_MAP.md)
