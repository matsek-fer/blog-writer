# Forest-readiness — the convention for MatSek blogs

**Status: normative** for every blog the blog-writer produces, and for any
blog submitted to the library that wants to opt in. It is defined once,
here; skills and tools reference this file instead of restating it.

**Why it exists.** The section plans a *Knowledge Forest*: a graph of small,
independently addressable explanation objects a reader (or a tutor) can
walk in any order their prerequisites allow. Blogs are the richest source
of such objects — but only if each section can survive being torn out of
its blog. This convention makes every new blog decomposable into forest
objects *later* without rewriting it *then*: the writing discipline makes
sections self-contained, the metadata says what kind of object each section
is, and the tutor stub turns "keep reading" into "keep learning".

Everything below is legal under bundle format v1 today: the spec
(`spec/bundles.md`) reserves every `^x_`-prefixed key as tool-private
extension space that all conforming tools preserve. Nothing here requires a
spec change to use.

## 1 · The writing discipline: sections are future objects

Each section of a forest-ready blog is written as if it will be read
**alone**, with no scrollback:

- **One primary concept per section.** The section's entry in
  `section_concepts` may list supporting ids, but the section *teaches*
  one thing. If an outline section teaches two, it is two sections.
- **Self-contained prose.** Every term the section leans on is either
  defined in the section or explicitly linked (`[entropija](#anchor)` or a
  link to another bundle). Never "as we saw above", "recall from the
  previous section", "using the same trick" — a forest reader has no
  above, no previous, no same.
- **Stable kebab anchors.** The heading anchor (spec rules: lowercase,
  Croatian diacritics transliterated, non-alphanumerics to single hyphens)
  is the section's future object id. Renaming a heading after publication
  breaks checkpoints, `x_forest`, external links, and the future forest —
  treat anchors as permanent the way bundle ids are.
- **Notation restated, not assumed.** If a section uses $H(X)$, the
  section says what $H$ and $X$ are, in one clause if that is all it takes.

Self-containment costs a sentence or two of redundancy per section. That
is the price of the forest, and it also makes the blog better to skim.

## 2 · The `x_forest` frontmatter field

`blog.md` frontmatter carries, beside the spec's `section_concepts` and
`checkpoints`, an `x_forest` map from **every** section anchor to a
classification:

```yaml
x_forest:
  "#surprise-as-a-number":
    taxon: exposition
    standalone: true
  "#entropy-as-a-guessing-game":
    taxon: intuition
    standalone: false
```

Rules:

- **Every section that appears in the body appears in `x_forest`** — a
  section the author cannot classify is a section the author does not yet
  understand the role of.
- `taxon` is exactly one of the five values below.
- `standalone` is the author's honest claim that the section passes the
  discipline of §1 — readable alone. `true` is the **goal** for every
  `exposition` and `connection`; an `intuition` or `example` that only
  works as a coda may honestly say `false`, and the forest will then keep
  it attached to its exposition.

### The taxa

| Taxon | The section's job | Forest role |
|---|---|---|
| `exposition` | Defines and develops the concept itself — the load-bearing explanation. | A primary teaching object; the node other objects attach to. |
| `example` | Works a concrete instance of an already-stated idea. | Attachable to any exposition of the same concept, including ones from other blogs. |
| `intuition` | Builds the mental picture — analogy, visualization, "what it feels like". | Optional enrichment; served when a learner says "I can compute it but I don't get it". |
| `motivation` | Says why anyone cares — the problem the concept answers, historical or practical stakes. | An entry point; often the first object a forest walk serves. |
| `connection` | Relates this concept to another one (a bridge, an equivalence, a contrast). | An edge-object between two concept nodes; must name both ends explicitly. |

Assign the taxon by what the section *does*, not what it mentions: a
section that defines entropy through a story is still `exposition` if the
definition is what the reader leaves with.

## 3 · The tutor stub: `tutor-stub.json`

A forest-ready blog bundle may include `tutor-stub.json` — a minimal,
AI_instructor-compatible session state file. A member downloads it, drops
it into a `sessions/<slug>/` folder of their tutor vault as `state.json`,
and runs `/tutor`: the probe starts exactly where the blog left them,
because `background` tells the tutor what the blog already covered.

The shape (field semantics from the tutor's
`.claude/skills/tutor/reference/state-format.md`, which is authoritative;
unknown keys round-trip there, so `x_stub` survives):

```json
{
  "schema_version": "1.0",
  "name": "pf-entropy-without-measure",
  "topic": "Shannon entropy of discrete distributions",
  "background": "Read the blog 'Entropy without measure theory': knows surprise log2(1/p), entropy as expected surprise, why the logarithm (additivity over independent experiments), and the guessing-game meaning. Assumes basic finite probability and expectation; no measure theory.",
  "phase": "setup",
  "created_at": "2026-09-04 12:00 UTC",
  "x_stub": true
}
```

- `name` — the blog's slug (it becomes the session directory name).
- `topic` — what a follow-up session should be about, in English (the
  tutor is model-facing).
- `background` — the crucial field: what the blog *assumes* plus what its
  sections *taught*, so the probe neither re-teaches the blog nor assumes
  more than it gave. Write it from `section_concepts` and the manifest's
  `requires`/`teaches`.
- `phase` — always `"setup"`: the stub is a session that has not started.
- `created_at` — the blog's creation timestamp in the tutor's
  `CREATED_AT_FORMAT`: `YYYY-MM-DD HH:MM UTC` (e.g. `2026-09-04 12:00 UTC`). When deriving from a date-only manifest `created` field, use `12:00` as the time.
- `x_stub: true` — marks the file as a stub, so tools can distinguish a
  downloaded starting point from a session someone actually ran. The tutor
  preserves it verbatim across saves.

No other state keys are included: the tutor initializes ladders, records
and the DAG itself once the session starts. The stub is an extra non-content
file in the bundle; per the spec, tools that do not recognize it ignore it.

## 4 · Promotion path: spec format v2

This convention is a **candidate for promotion into the bundle spec at
format v2**. If adopted, `x_forest` would become a schema-validated
frontmatter field (likely renamed without the `x_` prefix), the taxa would
become a closed enum in `blog-frontmatter.schema.json`, and the stub would
be specified alongside session bundles. Until then it rides on the `^x_`
rule, which is exactly what that rule is for: proving an extension in the
field before hardening it in the contract. Tools should therefore not
*require* `x_forest` on blogs they read — its absence means an older or
opted-out blog, not an invalid one.
