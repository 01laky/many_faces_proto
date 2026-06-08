# Changelog

All notable changes to **wire contracts** in this repository will be documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

- **`health.proto`** — AI **HealthService** wire contract (shared with **`many_faces_backend`** C# client and **`many_faces_ai`** Python server).
- **`manyfaces/search/v1/search.proto`** — operator-AI RAG knowledge surface: `KnowledgeDocument`, `SemanticSearch*`, `IndexKnowledge*`, `DeleteKnowledge*`, `KnowledgeIndexStatus*` messages and 4 new `SearchService` RPCs (`IndexKnowledge`, `DeleteKnowledge`, `SemanticSearch`, `KnowledgeIndexStatus`). Backend (C# client) embeds + orchestrates; the Go search-worker owns the ES kNN+BM25 (RRF) query and the versioned-index + alias lifecycle. The existing user-facing search RPCs are unchanged.

### Changed

- **`buf.yaml`** — lint ignore for `health.proto` (legacy `package health` naming).

## [0.1.0] - 2026-05-14

### Added

- Initial import of **push**, **mailer**, and **search** v1 `.proto` definitions (aligned with existing worker packages).
- Buf module under `proto/` with **lint** and **breaking** configuration.
- GitHub Actions workflow for `buf lint` / `buf breaking`.
