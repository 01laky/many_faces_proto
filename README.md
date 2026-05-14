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

## Submodule strategy (Strategy A)

The **many_faces_main** monorepo pins this repo at **`many_faces_proto/`** (sibling of `many_faces_backend/`, `many_faces_push/`, …). Use **`git clone --recursive`** or **`git submodule update --init --recursive`** so the contracts are present before building workers or the backend.

- **.NET:** `BeDemo.Api.csproj` references `..\..\many_faces_proto\proto\...` from `many_faces_backend/BeDemo.Api/` (worker APIs under `manyfaces/`, AI under `health.proto`).
- **Go / Java:** from a consumer submodule root, use **`../many_faces_proto/proto`** as the `protoc` / Gradle include root when developing inside the monorepo.
- **Standalone clone** of a single worker repo: clone **`many_faces_proto`** next to it (or shallow-clone in CI) so relative paths or `MANY_FACES_PROTO_DIR` match that repo’s README.

Strategy **B** (nested `many_faces_proto` submodule inside each consumer) is optional for repos that must build outside the monorepo; keep submodule SHAs in lockstep if you adopt it.

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
