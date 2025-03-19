# build-attestations-private

```sh
gh attestation verify attestations.tar.gz --owner joshjohanning-org                                                                                                                                                  ✔ 
Loaded digest sha256:c980bfe4d646ff74dd0f074acb3a9bc22b5f6c9b1685e33d160a7c187035a6f1 for file://attestations.tar.gz
Loaded 1 attestation from GitHub API

The following policy criteria will be enforced:
- Predicate type must match:................ https://slsa.dev/provenance/v1
- Source Repository Owner URI must match:... https://github.com/joshjohanning-org
- Subject Alternative Name must match regex: (?i)^https://github.com/joshjohanning-org/
- OIDC Issuer must match:................... https://token.actions.githubusercontent.com

✓ Verification succeeded!

The following 1 attestation matched the policy criteria

- Attestation #1
  - Build repo:..... joshjohanning-org/build-attestations-private
  - Build workflow:. .github/workflows/build-attestations.yml@refs/heads/main
  - Signer repo:.... joshjohanning-org/build-attestations-private
  - Signer workflow: .github/workflows/build-attestations.yml@refs/heads/main
  ```
