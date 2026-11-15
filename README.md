# Tumpel workflows

Reusable GitHub Actions workflows for Tumpel.

## prebuild

`.github/workflows/prebuild.yml` builds a repository's prebuild layer inside the Tumpel guest image and uploads it to the Tumpel API. Tumpel dispatches it through a caller workflow in the repository; the `build` job runs the repository's code without credentials, and the `upload` job holds `id-token: write` and runs no repository code.

A caller workflow references it by tag:

```yaml
jobs:
  prebuild:
    uses: tumpel/workflows/.github/workflows/prebuild.yml@v1
```

`v1` moves to each release that keeps the workflow's inputs. Each release also gets an immutable `vX.Y.Z` tag for callers that pin one.

`examples/tumpel-prebuild.yml` is the full caller workflow to commit at `.github/workflows/tumpel-prebuild.yml`.

This repository is the source of these workflows. Changes land on `main`; `v1` moves once a change is ready.
