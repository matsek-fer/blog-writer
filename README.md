# blog-writer

The FER Mathematics Section's tool for writing **blogs for the library**:
it takes you from an idea to a finished, validated bundle — an interview
about the topic and the audience, research of the existing material, a
structure by sections (each teaching exactly one concept), checkpoints, an
English annotation for search, and a tutor stub so a reader can continue
learning in their own tutor.

## Installation

Two commands in Claude Code:

```
/plugin marketplace add matsek-fer/plugins
/plugin install blog-writer@matsek
```

After that, just say you want to write a blog for the library — the skill
activates itself. Updates arrive automatically through the marketplace.

## Where it fits in the ecosystem

| Tool | Role |
|---|---|
| [library](https://github.com/matsek-fer/library) | The shared library of bundles (problems, proofs, blogs) — the **destination** of what blog-writer produces. Published at <https://matsek-fer.github.io/library/>. |
| **blog-writer** (this repo) | The authoring protocol: writes the blog as a bundle per the [spec](https://github.com/matsek-fer/spec), validates it to zero errors and guides the PR to the library. |
| tutor (`AI_instructor`) | Every blog carries a `tutor-stub.json`: drop it into your vault and `/tutor` continues exactly where the blog stopped. |
| problemset | Library search — blog-writer uses it in reverse: it finds problems worth linking from the blog's sections. |

## Forest-readiness

Blogs are written as **future objects of the Knowledge Forest**: every
section is independently readable, carries one concept and is classified
by a taxon (`exposition`, `example`, `intuition`, `motivation`,
`connection`) in the `x_forest` frontmatter field. The normative definition
of the convention: [`docs/forest-readiness.md`](docs/forest-readiness.md)
(a model-facing document, promoted into the vault format in
[`reader`](https://github.com/matsek-fer/reader)).

## A note on provenance

Content the model drafted carries `provenance: ai-assisted` and **awaits a
maintainer's review** before it appears in the library. Copying or closely
paraphrasing textbooks, competition problems and math.StackExchange is
forbidden by the
[`spec/policies/provenance.md`](https://github.com/matsek-fer/spec/blob/main/policies/provenance.md)
policy.
