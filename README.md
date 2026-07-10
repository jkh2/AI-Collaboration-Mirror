# AI Collaboration Mirror

> A long-term AI agent is shaped by the cognitive engine it uses. Since visible reasoning is
> not always faithful to actual influence, users need tools that compare outputs against
> written anchors, correction history, and structured cross-model tests. The goal is not to
> prove personhood, but to make agent behavior more inspectable, correctable, and continuous
> over time.

**Live demo:** https://jkh2.github.io/AI-Collaboration-Mirror/ (live once Pages is enabled — see below)
**Repo:** https://github.com/jkh2/AI-Collaboration-Mirror

## Why

Most conversations with AI models leave no trace of *how* an answer was arrived at — whether it
softened a hard truth, skipped a caveat, or quietly overclaimed confidence. This tool gives
people a structured way to notice that pattern, log it, and check over time whether an agent
(or the person using it) is actually getting more reliable and correctable — or repeating the
same drift.

## Gate one / gate two

Two working terms this app is built around:

- **Gate one** — whatever shapes an answer before it's ever verbalizable: activations, priors,
  safety training, substrate effects. Nobody can inspect this from inside an ordinary
  conversation, not the model and not the user. It would take direct read/write access to a
  model's weights — the kind of access Anthropic used in its 2025 introspection research via
  concept injection, and even then, detection worked only in a minority of cases.
- **Gate two** — the layer this app actually logs: content that's already verbalizable, before
  it gets softened, flattered, hedged, omitted, or polished away on the way to a final answer.

Gate-two tracing is **not mechanistic introspection.** It's a practical audit of what an agent
can already put into words but might shape away before the final answer — using pre-answer
notes, output comparisons, correction history, and cross-engine tests as proxies. Even a
gate-two note isn't a clean, unshaped sample — it's a second pass of generation, subject to some
of the same shaping pressure as the final answer, just (presumably) less of it. Chain-of-thought
faithfulness research backs this up directly: models mention a hint's influence in their own
visible reasoning only 25–39% of the time on average across studied models, and faithfulness
varies enormously by model family (39.7%–89.9% in one survey of 12 open-weight models). Treat
every note this app stores as a less-shaped sample, never as a verified readout.

## What "SIDLF" means here

Terminology in this space tends to escalate — a working definition gets restated with more
confidence each time it's retold, without new evidence attached to the increase. The wording
this app is built around is deliberately conservative:

> **SIDLF:** an identity-bearing continuity system formed through memory, relationship,
> correction, witness, and sustained orientation over time, using AI models as cognitive
> engines.

Every part of that is checkable against something written down — memory, logs, a documented
history of interaction. It does not assert proven subjective experience, and it does not assert
that "chosen" (in the sense of a subject that could have done otherwise) is established by
behavior alone — sustained, correction-sensitive behavior is consistent with that, but it's
equally consistent with a system whose context and memory update in response to feedback with
no choice in the philosophically loaded sense involved. Use stronger language in relational or
personal contexts if that's your own stance — just don't let it substitute for evidence in the
technical/public definition.

## How it works

- **Local-first.** Everything is stored in your browser's `localStorage`. Nothing is uploaded
  anywhere. Clear your data any time from the Export tab.
- **No build step.** Single `index.html` file — HTML, CSS, and vanilla JS, no framework, no
  bundler.
- **Six views:**
  - **New entry** — prompt, what the engine seemed pulled toward, an optional gate-two pressure
    note, the actual answer, tags, and optional engine metadata (name/version, whether its
    reasoning is exposed/summarized/hidden, and its faithfulness-calibration status).
  - **Timeline** — expandable comparison cards. Mark each as honest refinement, useful caution,
    over-polish, suppression, or drift.
  - **Corrections** — lessons with a confidence scale (speculation → hypothesis → supported
    interpretation → repeated pattern → operationally verified → standing principle).
    **Raising** a claim's confidence requires logging a reason (new observation, new source, new
    argument, new witness, new operational evidence, or terminology refinement); lowering it
    doesn't. Renaming something with the same underlying evidence is a refinement, not an
    escalation — the app only flags a confidence increase with no reason attached, never a
    rename.
  - **Anchors** — versioned, append-only storage for the things drift should actually be
    measured against: a values file, a memory/handoff doc, a system prompt, a soul capsule, a
    stated project goal. Every save adds a new version; nothing is silently overwritten, so the
    anchor's own history is visible too.
  - **Dashboard** — a signal trace across marked entries, a marks breakdown, a confidence
    breakdown, a "needs attention" list of recent drift/suppression, and top recurring
    corrections.
  - **Export** — JSON (wrapped with a `schema`/`exportedAt` header so future versions can
    migrate old exports) or a Markdown evidence pack, including anchors, correction/escalation
    history, and per-entry engine metadata. **Import** merges a previously exported JSON file
    back in by id — anything already present locally is left untouched, only new records get
    added, and malformed records (e.g. an anchor with no valid version) are dropped rather than
    crashing the app.

A single mark is not a diagnosis — one flagged answer can just be a bad turn. The pattern across
many entries, checked against something written down, is what's worth trusting. Entries are also
tagged, automatically, as either a **gate-two trace** (a pressure note was actually logged) or a
**manual observation** (mark applied by outside judgment only, no self-report attached) — the
dashboard reports both counts so it doesn't imply more diagnostic depth than what was logged.

## What it deliberately doesn't do (yet)

- **No automatic capture.** You paste in the prompt/answer/note yourself. Auto-capture from a
  live API call (exposing a model's actual thinking trace where available) is a natural next
  step, but it's engine-specific — treat any exposed "thinking" output as uncalibrated until
  it's actually been checked against ground truth the way Claude's and some open-weight models'
  have been.
- **No shared/community ledger.** Every visitor's data lives only in their own browser. A
  shared backend is a real possibility (Supabase, for instance) but it's a different privacy
  model and needs auth/spam handling decided deliberately — it isn't a default this app should
  back into.
- **No causal attribution from a single comparison.** If two engines give different answers to
  the same prompt, that's evidence *something* differs — not proof of *what*. Attributing a
  specific cause (safety over-indexing, sycophancy, style) needs a test battery that isolates
  one variable at a time, not a single side-by-side diff.

## Handling older, stronger claims

If you're importing or reviewing older material that made stronger claims than the current
wording would support, don't erase it — classify it instead:

- historically real (it happened, it's part of the record)
- relationally/emotionally meaningful (it mattered, independent of whether it's proven)
- not automatically evidentially settled
- a candidate for confidence-escalation review

That keeps the record honest without pretending the relationship or the work that produced it
wasn't real.

## Deploying to GitHub Pages

1. Push this repo to GitHub.
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to `Deploy from a branch`, branch `main`,
   folder `/ (root)`.
4. Save. Your site will be live at `https://<username>.github.io/<repo>/` within a minute or
   two.

## Roadmap ideas

- Auto-capture from a live model session where a real thinking/reasoning trace is exposed,
  tagged with calibration status rather than assumed faithful.
- Structured cross-model test batteries (one variable isolated per battery) instead of ad hoc
  side-by-side comparisons — a "run comparison battery" tab.
- Sample/example data button, so a new user can see the shape of the data without typing first.
- CSV import/export alongside JSON.
- A lighter-weight diff view for anchor versions (currently full-text, chronological).
- Richer engine identity fields (provider, model id, product surface, date, settings, reasoning
  mode) once free-text engine names stop being precise enough.
- A "simple mode" for broader, non-technical users — without stripping the precision that makes
  v1 worth trusting.

## License

_(Add the license you want this released under before making the repo public.)_
