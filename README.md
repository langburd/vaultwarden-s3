# vaultwarden-s3

Multi-arch (`linux/amd64`, `linux/arm64`) build of upstream
[Vaultwarden](https://github.com/dani-garcia/vaultwarden) with the `s3` cargo
feature, so `DATA_FOLDER` can point at S3-compatible object storage. The official
images ship only `sqlite,mysql,postgresql`.

Image: `ghcr.io/langburd/vaultwarden-s3:<upstream-version>`

Cargo features: `mysql,s3,enable_mimalloc` (no SQLite or PostgreSQL).

## How it works

- `VERSION` holds the upstream release tag. Renovate opens a PR when upstream releases.
- `.github/workflows/build.yaml` checks out upstream at that tag and builds its
  `docker/Dockerfile.alpine` with `DB=mysql,s3,enable_mimalloc`.
  Pull requests build both architectures without pushing; a push to `master` pushes the tag.
- Tags are never moved by hand: a rebuild of the same version overwrites the tag.

Used by the `vaultwarden` addon in the `ddyy-oke` GitOps repo.
