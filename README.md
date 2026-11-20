# renovate-config

Renovate organisation config

Usage:

```json
{
  "extends": ["github>allthings/renovate-config"]
}
```

## Docker

- **Dockerfiles** (`FROM` and `COPY --from=image:tag`) are updated. These PRs are never automerged: an image update ships with the next image build. Untagged and `latest` images (e.g. `FROM allthings/serverless`) are skipped.
- **Versions in `ARG`/`ENV`** are updated when the line above names a source ([`customManagers:dockerfileVersions`](https://docs.renovatebot.com/presets-customManagers/#custommanagersdockerfileversions)); the variable name must end in `_VERSION`:

  ```dockerfile
  # renovate: datasource=github-releases depName=segmentio/chamber
  ENV CHAMBER_VERSION=v3.1.4

  # renovate: datasource=go depName=github.com/segmentio/chamber/v3
  ARG CHAMBER_VERSION=v3.1.5

  # renovate: datasource=node-version depName=node
  ARG NODE_VERSION=24.22.3
  ```

- **docker-compose** stays off. A repo opts in with `"docker-compose": { "enabled": true }` (see github-action-tools).
- To turn Docker updates off in a repo, add `"docker:disable"` to its `extends`. Annotated `ARG`/`ENV` lines are a separate custom manager; to stop those too, add `"ignorePresets": ["customManagers:dockerfileVersions"]`.
