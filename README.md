# Wicofe-Ports

Port recipes and integration patches for bringing external software to Wicofe OS.

## Scope

- Reproducible build recipes for upstream projects.
- Wicofe-specific patches and configuration.
- Upstream version tracking and patch rationale.
- Dependency metadata and build instructions.
- Integration status and runtime validation evidence.
- Links to canonical license, provenance, and security review records.

## Porting policy

Prefer minimal, reviewable patches and upstreamable changes. Pin exact source versions or commits, checksums, build tools and dependencies. Record enabled features, target architectures, patch origins and validation level. Do not copy or redistribute source without verifying its exact terms and source provenance.

A host build or successful cross-compilation does not prove that a program can run on Wicofe. Declare build, link, static-validation and runtime support separately.

## Boundaries

WPM owns package installation and repository operations. Wicofe-Build owns whole-system build orchestration. Wicofe-SDK owns developer toolchain and sysroot distribution. Wicofe-ThirdParty owns canonical source provenance, license/notice inventory and compliance approval records. Wicofe-Ports owns port recipes, integration patches and upstream maintenance; it links to third-party records rather than duplicating compliance decisions.

## Status

Initial repository baseline; no official port collection is claimed.

## License

Each port retains its upstream license. Recipe and patch licensing must be recorded individually. Original Wicofe-authored material follows the project's MPL-2.0 licensing policy.

## Documentation

See [docs/INDEX.md](docs/INDEX.md) for the [ports integration architecture](docs/PORTING_INTEGRATION_ARCHITECTURE_0.1.md) and technical documentation. The [central repository documentation map](https://github.com/Waizusora/Wicofe-Docs/blob/main/REPOSITORY_DOCUMENTATION_MAP.md) identifies canonical ownership and cross-repository boundaries.

## Wicofe-wide principles

Wicofe is an independent Unix-like, POSIX-compatible operating system and software ecosystem, designed as an alternative to established and widely adopted software ecosystems and technologies, including GNU. Native components must use Wicofe interfaces and must not require Linux libc or GNU compatibility layers. Prefer permissive licenses; avoid GPL/AGPL, and consider LGPL only when no suitable permissive alternative exists. Reuse or fork upstream software when implementing it from scratch would be disproportionately difficult. See the [canonical project principles](https://github.com/Waizusora/Wicofe-Docs/blob/main/PROJECT_PRINCIPLES.md).
