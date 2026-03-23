# build-attestations

Demo repo showing how to use [GitHub Artifact Attestations](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations/using-artifact-attestations-to-establish-provenance-for-builds) to create build provenance and SBOM attestations, then verify them.

Artifact attestations create a tamper-proof link between your built artifact and the GitHub Actions workflow that produced it. This lets you (or anyone consuming your artifact) verify that it was genuinely built from a specific repo and workflow — not modified after the fact.

## What this repo demonstrates

The [workflow](.github/workflows/build-attestations.yml) does the following:

1. Builds a .NET class library and packages it as `build.tar.gz`
2. Creates a **build provenance** attestation using `actions/attest@v4`
3. Generates an **SBOM** and creates an SBOM attestation using `actions/attest@v4` with `sbom-path`
4. Verifies both attestations in separate jobs (including a deliberate failure case to show what a bad owner looks like)

## Verifying attestations locally

After downloading the artifact, you can verify it with the [GitHub CLI](https://cli.github.com/) (v2.49.0+):

**Build provenance:**

```sh
gh attestation verify build.tar.gz --owner joshjohanning-org
```

**SBOM:**

```sh
gh attestation verify build.tar.gz \
  --owner joshjohanning-org \
  --predicate-type https://spdx.dev/Document/v2.3
```

### Example output

```
$ gh attestation verify build.tar.gz --owner joshjohanning-org

Loaded digest sha256:c980bfe4d646ff74dd0f074acb3a9bc22b5f6c9b1685e33d160a7c187035a6f1 for file://build.tar.gz
Loaded 1 attestation from GitHub API

The following policy criteria will be enforced:
- Predicate type must match:................ https://slsa.dev/provenance/v1
- Source Repository Owner URI must match:... https://github.com/joshjohanning-org
- Subject Alternative Name must match regex: (?i)^https://github.com/joshjohanning-org/
- OIDC Issuer must match:................... https://token.actions.githubusercontent.com

✓ Verification succeeded!

The following 1 attestation matched the policy criteria

- Attestation #1
  - Build repo:..... joshjohanning-org/build-attestations
  - Build workflow:. .github/workflows/build-attestations.yml@refs/heads/main
  - Signer repo:.... joshjohanning-org/build-attestations
  - Signer workflow: .github/workflows/build-attestations.yml@refs/heads/main
```

> **Note:** In CI/automated contexts, `gh attestation verify` only returns exit code 0 or 1 with no stdout ([the CLI is hardcoded for this](https://github.com/cli/cli/blob/45ffa3c668f7105e92089220d42aa64c9acbd11c/pkg/cmd/attestation/io/handler.go#L45)). Use `--format json` to get structured output.

## Resources

- [Using artifact attestations to establish provenance for builds](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations/using-artifact-attestations-to-establish-provenance-for-builds) — GitHub Docs
- [Introducing Artifact Attestations](https://github.blog/news-insights/product-news/introducing-artifact-attestations-now-in-public-beta/) — GitHub Blog
- [Recent improvements to Artifact Attestations](https://github.blog/changelog/2025-02-18-recent-improvements-to-artifact-attestations/) — Changelog
- [Code-to-cloud traceability and SLSA Build Level 3](https://github.blog/changelog/2026-01-20-strengthen-your-supply-chain-with-code-to-cloud-traceability-and-slsa-build-level-3-security/) — Link artifacts to storage/deployment locations and filter security alerts by production context
- [`actions/attest`](https://github.com/actions/attest) — Unified attestation action (provenance, SBOM, and custom)
- [`gh attestation` CLI reference](https://cli.github.com/manual/gh_attestation)
