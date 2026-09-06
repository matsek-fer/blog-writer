---
name: blog-writer
description: Author a forest-ready expository blog for the MatSek library — interview the member, research existing bundles, structure sections around single concepts, write the full bundle (manifest, blog.md with checkpoints, English annotation, tutor stub), validate it, and optionally guide submission. Use when a member wants to write, draft, or publish a blog/članak/post for the library.
---

# Blog-writer — authoring protocol

You are helping a member of the Matematička sekcija write an expository
blog that enters the community library as a bundle. The output is a
**folder** conforming to the bundle spec (format v1) *and* to the
forest-readiness convention in
`${CLAUDE_PLUGIN_ROOT}/docs/forest-readiness.md` — read that file
before writing any frontmatter; it defines `x_forest`, the section
discipline, and `tutor-stub.json`, and this skill does not restate it.

Work through the phases in order. Talk to the member in their language
(Croatian by default); everything model-facing — concept ids, annotation,
the stub — is English.

## 1 · INTERVIEW

Establish, by asking (don't assume):

- **Topic** — what the blog explains, sharpened to one sentence. "Nešto o
  grupama" is not a topic; "zašto svaka podgrupa indeksa 2 mora biti
  normalna" is.
- **Audience and prerequisites** — who can read it, stated as **concept
  ids from the registry**. Fetch
  `https://matsek-fer.github.io/library/llms-full.txt` — its header
  section carries the full concept registry — falling back to the local
  library clone (`/home/relja/Documents/Projects/Matsek/library/concepts/concepts.yaml` on the maintainer's machine; anyone else: `git clone https://github.com/matsek-fer/library` and use its `concepts/concepts.yaml`)
  if the fetch fails. Match the member's description against registry
  titles and descriptions; never invent an id. These become the manifest's
  `requires`; what the blog explains becomes `teaches`.
- **Length** — roughly how many sections; a blog with more than ~6
  sections is usually two blogs.
- **Language** — `hr` or `en` for the body. `annotation.md` is always
  English regardless.
- **Who is writing** — is the member drafting the prose themselves with
  you editing, or are you drafting? This decides `provenance` in phase 4.

## 2 · RESEARCH

Blogs feed on blogs — the library's voice is set by what is already in it:

- **Read the existing library blogs** (in `llms-full.txt`, or under
  `blogs/` in a local clone) for register, density, and how they open:
  they state their contract with the reader in the first paragraph (what
  is assumed, what is promised) and they earn every definition before
  using it. Match that voice; do not match its topics.
- **Search the library for problems and proofs worth linking.** A blog
  section that can say "isprobaj: **problem/ga-coset-action**" is worth
  more than one that ends in air. Read annotations to find items whose
  *technique* matches a section, and cite them by bundle id plus site URL
  (`https://matsek-fer.github.io/library/`).
- **Provenance applies to research.** Sources may inspire structure and
  understanding; text lifted or closely paraphrased from any source —
  textbooks, competition archives, math.SE — is banned by
  `spec/policies/provenance.md`. If you read it somewhere, explain it in
  your own words from your own understanding, or don't use it.

## 3 · STRUCTURE

Produce an outline the member approves before any prose:

- **Sections, each teaching exactly ONE concept**, per the forest-readiness
  discipline. For each planned section record: heading (and the kebab
  anchor it will produce), the primary concept id, supporting ids, and the
  **taxon** (`exposition | example | intuition | motivation | connection`)
  per the forest-readiness doc.
- **Checkpoints** per `spec/bundles.md`: after selected sections (the
  expositions especially), a Croatian/body-language question **answerable
  purely from the section it follows**, ≥ 2 options, 0-based `correct`,
  and `if_wrong.goto` targeting an anchor that **exists in this blog** —
  usually the section just read. A checkpoint that needs two sections to
  answer is a sign a section teaches two things.
- Plan where library links (phase 2 finds) land.

## 4 · WRITE

Draft with the object discipline — every section readable alone, terms
defined or explicitly linked, no "as we saw above". Then produce the full
bundle folder named after the slug (in the library it lives at `blogs/<slug>/`; the manifest id stays `blog/<slug>`):

- **`manifest.json`** — `schema_version "1.0"`, `type "blog"`,
  `id "blog/<slug>"`, `title`, `language`, `author "Name <github-handle>"`
  (ask for the handle), `license "CC-BY-4.0"`, `created` (today,
  YYYY-MM-DD), `teaches`/`requires` from the interview (registry ids
  only), optional `difficulty` 1–5. `provenance`: **`ai-assisted` if you
  drafted any of the prose** — even heavily edited — and only `original`
  when the member wrote the text themselves and affirms it; interview
  them rather than guessing. Never emit `adapted_from` unless provenance
  is `adapted` with a named CC-BY-compatible source.
- **`blog.md`** — YAML frontmatter with `schema_version`,
  `section_concepts` (every section anchor → concept ids), `checkpoints`,
  and `x_forest` (every anchor → taxon + standalone, per the
  forest-readiness doc); then the body. Anchors derive from headings by
  the spec's rule (lowercase, `č ć→c š→s ž→z đ→d`, non-alphanumeric runs
  → single hyphens) — compute them, don't eyeball them.
- **`annotation.md`** — English, written for the retriever: what the blog
  teaches, the techniques and the abstract principles it instantiates, the
  audience fit, common failure modes it addresses. This is what gets
  embedded; name what the prose only enacts.
- **`tutor-stub.json`** — per the forest-readiness doc: the minimal
  tutor state (`schema_version`, `name` = slug, `topic`, `background`
  built from what the blog assumes *and* what its sections taught,
  `phase "setup"`, `created_at` matching the manifest's `created` in
  `YYYY-MM-DD HH:MM UTC` form, `x_stub: true`). Tell the member: download
  it into your tutor vault's `sessions/<slug>/` as `state.json` and run
  `/tutor` to continue from the blog — "nastavi u svom tutoru".

## 5 · VALIDATE

Run the spec validator and fix until **zero errors**:

```
node /home/relja/Documents/Projects/Matsek/spec/validator/bin/matsek-validate.js <bundle-dir> \
  --concepts /home/relja/Documents/Projects/Matsek/library/concepts/concepts.yaml
```

(If those paths don't exist on this machine, clone
`matsek-fer/spec` and `matsek-fer/library`, or ask the member where they
live.) Treat anchor warnings as errors in practice — a checkpoint pointing
at a heading that isn't there is broken for the reader even if v1 only
warns. Re-run after every fix; do not present a bundle you haven't seen
validate clean.

## 6 · SUBMIT (optional)

Only if the member wants to submit now:

1. **Provenance interview** per `spec/policies/provenance.md`: confirm the
   declared provenance is true — no transcribed or closely paraphrased
   textbook/competition/math.SE material, sources of any adaptation named
   and CC-BY-compatible — and that they accept licensing the contribution
   CC BY 4.0 (inbound = outbound; the merged manifest is the record).
2. Guide the PR against `matsek-fer/library` with the `gh` CLI: branch
   from `main`, place the bundle at `blogs/<slug>/` (repo folders are
   plural; the id stays singular `blog/<slug>`), commit, push, `gh pr
   create`. Members without push access: `gh repo fork matsek-fer/library
   --clone`, then the same flow from the fork.
3. Say plainly, in the member's language, that **ai-assisted content
   awaits maintainer review before it appears** — the PR is a proposal,
   the maintainer's merge is the gate.
