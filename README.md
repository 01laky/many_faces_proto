# many_faces_proto

Canonical **Protocol Buffer** and **gRPC** contracts shared across Many Faces services (`many_faces_backend`, `many_faces_push`, `many_faces_mailer`, `many_faces_elastic`, `many_faces_ai`, …).

## Layout

```text
proto/
  buf.yaml              # Buf lint + breaking rules (module root)
  manyfaces/
    push/v1/            # FCM push worker API
    mailer/v1/          # Transactional mail worker API
    search/v1/          # Elasticsearch search-worker API
```

Wire contracts live only under `proto/manyfaces/**`. Consumers attach this repository as a **git submodule** (recommended: `many_faces_main/many_faces_proto` — see monorepo [`docs/prompts/proto-shared-repository-and-submodules-agent-prompt.md`](https://github.com/01laky/many_faces_main/blob/main/docs/prompts/proto-shared-repository-and-submodules-agent-prompt.md)).

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
