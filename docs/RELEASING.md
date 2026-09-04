# Releasing terraform-provider-iptrack

This repository publishes signed Terraform provider artifacts for
`registry.terraform.io/f0rkz/iptrack`.

Releases are tag-driven. Push a semantic version tag such as `v0.1.0`, and the
GitHub Actions release workflow builds signed provider archives and publishes a
GitHub release that the Terraform Registry can ingest.

## Required repository setup

Create an RSA GPG signing key dedicated to provider releases. Add these GitHub Actions secrets:

- `GPG_PRIVATE_KEY`: ASCII-armored private key from `gpg --armor --export-secret-keys KEY_ID`
- `PASSPHRASE`: the private-key passphrase

Add the corresponding public key to the Terraform Registry account with `gpg --armor --export KEY_ID`. HashiCorp requires signed provider releases and supports RSA or DSA keys, not the default ECC key type.

Recommended repository settings:

- Require the `Go tests` and `Release configuration` checks before merging to `main`.
- Require pull requests and disallow force pushes on `main`.
- Enable immutable GitHub releases after validating the first release.

## Published outputs

The GitHub release receives ZIP archives for the Terraform provider on Linux, macOS, Windows, and FreeBSD, plus a manifest, SHA-256 checksums, and a detached GPG signature.

## First publish

1. Push the repository to GitHub as `f0rkz/terraform-provider-iptrack`.
2. Push a semantic version tag, for example:

   ```sh
   git tag v0.1.0
   git push origin main --tags
   ```

3. Sign in to the Terraform Registry with GitHub.
4. Add the release signing public key under Terraform Registry signing keys.
5. Publish the provider from the Terraform Registry UI by selecting the
   `terraform-provider-iptrack` repository.

Future GitHub releases should be picked up automatically through the registry
webhook.

## Recovering a partial release

If a tag exists but the release workflow fails after checkout, rerun the
workflow manually and provide the tag name, for example `v0.1.0`, as the
workflow `ref` input.
