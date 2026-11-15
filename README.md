# Tumpel workflows

Reusable GitHub Actions workflows for Tumpel.

## prebuild

`.github/workflows/prebuild.yml` builds a repository's prebuild layer inside the Tumpel guest image and uploads it to the Tumpel API. A Tumpel prebuild start commits a caller workflow in the repository through the Tumpel GitHub App, or dispatches it when it is already there. The `inputs` job reads the prebuild's inputs from the API with the job's OIDC token, the `build` job runs the repository's code without credentials, and the `upload` job holds `id-token: write` and runs no repository code.

A caller workflow references it by tag and passes only the API origin:

```yaml
jobs:
  prebuild:
    uses: tumpel/workflows/.github/workflows/prebuild.yml@v2
    with:
      apiOrigin: https://api.tumpel.dev
```

`v2` moves to each release that keeps the workflow's inputs. Each release also gets an immutable `vX.Y.Z` tag. `v1` stays at the release whose caller passed every input through `workflow_dispatch`.

`examples/tumpel-prebuild.yml` is the caller workflow Tumpel commits at `.github/workflows/tumpel-prebuild.yml`.

This repository is the source of these workflows. Changes land on `main`; the major tag moves once a change is ready.
