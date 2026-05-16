# many_faces_proto

Canonical **Protocol Buffer** and **gRPC** contracts shared across Many Faces services (`many_faces_backend`, `many_faces_push`, `many_faces_mailer`, `many_faces_elastic`, `many_faces_ai`, …).

## Layout

```text
proto/
  buf.yaml
  health.proto           # AI HealthService (legacy package name; Buf lint ignored)
  manyfaces/
    push/v1/
    mailer/v1/          # Transactional mail worker API
    search/v1/          # Elasticsearch search-worker API
```

Wire contracts live under **`proto/manyfaces/**`** and **`proto/health.proto`** (AI). Consumers attach this repository as a **git submodule**.

## Submodule strategy (Strategy B — current in Many Faces)

Each gRPC consumer (`many_faces_backend`, `many_faces_push`, `many_faces_mailer`, `many_faces_elastic`, `many_faces_ai`) embeds **this same repository** as a nested git submodule at **`many_faces_proto/`**.

- **Edit contracts only in this repo** (or one nested checkout of it), then **push here first**.
- **Bump the same commit SHA** in every consumer’s nested `many_faces_proto/` before merging a wire change.
- **.NET:** from `many_faces_backend/BeDemo.Api/`, protos are `..\many_faces_proto\proto\...`.
- **Go / Java / Python:** use **`many_faces_proto/proto`** as the include root from the consumer repo.

Monorepo helper: `many_faces_main/scripts/bump-nested-many-faces-proto.sh`. Agent rule: `many_faces_main/.cursor/rules/proto-single-source-submodule.mdc`.

Further detail: monorepo [`docs/prompts/proto-shared-repository-and-submodules-agent-prompt.md`](https://github.com/01laky/many_faces_main/blob/main/docs/prompts/proto-shared-repository-and-submodules-agent-prompt.md).

## Buf (lint)

From the **`proto/`** directory (module root):

```bash
cd proto
buf lint
```

On pull requests, CI runs **`buf lint`** and **`buf breaking`** against `main`.

## Versioning

- Use `package manyfaces.<domain>.v1` (path mirrors directories).
- **Breaking** wire or field-number changes: prefer a new major path (`v2`) or document a coordinated rollout in `CHANGELOG.md`.

## Changelog

See [`CHANGELOG.md`](./CHANGELOG.md).

## License

See [`LICENSE`](./LICENSE).
