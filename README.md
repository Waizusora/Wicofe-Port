# Wicofe-Ports

Port recipes and integration patches for bringing external software to Wicofe OS.

## Scope

- Reproducible build recipes for upstream projects.
- Wicofe-specific patches and configuration.
- Upstream version tracking and patch rationale.
- Dependency metadata and build instructions.
- License, provenance, and security review records.

## Porting policy

Prefer minimal, reviewable patches and upstreamable changes. Record source URLs, exact versions or commits, checksums, license identifiers, patch origins, and known security issues. Do not copy source without verifying its redistribution terms.

## Boundaries

WPM owns package installation and repository operations. Wicofe-Build owns build orchestration. Wicofe-ThirdParty, when created, may centralize provenance or source integration records; the two repositories must have clearly separated responsibilities.

## Status

Initial repository baseline; no official port collection is claimed.

## License

Each port retains its upstream license. Recipe and patch licensing must be recorded individually.

## Documentation

See [docs/INDEX.md](docs/INDEX.md) for this repository's technical documentation. The [central repository documentation map](https://github.com/Waizusora/Wicofe-Docs/blob/main/REPOSITORY_DOCUMENTATION_MAP.md) identifies canonical ownership and cross-repository boundaries.
