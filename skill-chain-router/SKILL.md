---
name: skill-chain-router
description: >
  Routes multi-skill workflows across the installed craft skills (design, slides,
  docs, Excel, writing, de-slop). Use when a task spans several artifacts, when
  deciding which skill to load for a given phase, or when asked "how should these
  skills be chained". Defines six canonical pipelines, the handoff artifacts
  between phases, and the universal quality gates every deliverable must pass
  before delivery.
version: 1.1.0
---

# Skill Chain Router

How to chain the installed craft skills into pipelines. One skill = one phase.
Artifacts flow forward only. Every deliverable exits through a gate.

---

## 1. Capability inventory

| Capability | Skill | Role in a chain |
|---|---|---|
| UI strategy & design system | `frontend-design-deslop` | Phase 0: lock artifact type + adjectives; produce DESIGN.md tokens |
| Visual audit | `design-taste` | Gate: score/critique rendered UIs, slides, images, posters |
| Interaction & motion | `apple-design` | Craft pass: springs, gestures, fluid transitions for web |
| Brand identity | `brand-identity-design` | Source: six-stage VI workflow → palette, type, logo, guidelines |
| Deck methodology | `elite-ppt-pro` | Research-first narrative: data register, conclusion-as-title |
| Deck engineering | `mck-ppt-design` | Production: python-pptx, 70 layouts, guard rails, charts |
| Excel operations | `mu-excel-toolbox` | Peek → backup → operate; 15 purpose-built scripts |
| Word integrity | `word-docx` | OOXML-aware docx editing, tracked changes, round-trip safety |
| Prose codification | `copywriting-prose-creator` | Run once per brand → PROSE.md house style |
| Voice-aware writing | `Writing` | Draft/edit in a specific human voice; two-failure-mode diagnosis |
| Technical writing | `clear-writing` | Docs, READMEs, API refs, commit messages (Strunk + Divio) |
| Body-text polish | `microtypography` | Publish-bound typography pass (quotes, dashes, spacing) before delivery |
| De-slop rewrite | `humanizer` | Remove AI patterns (Wikipedia signs) + inject personality |
| Edit/detect gate | `anti-ai-slop-writing-gate` | Pre-send gate: silent audit, detect-with-evidence mode |
| Slop density meter | `slop-detector` | Quantitative density scoring per 100 words, clustering |
| Output QC | `Anti-Slop` | Information-density + originality review of any artifact |

Mechanics layer: some environments bundle built-in mechanical skills (`docx`,
`pptx`, `xlsx`, `pdf`, `charts`, `fullstack-dev`, `frontend-styling-expert`);
when present, craft skills layer rules ON TOP, never replace. When absent,
the craft skill's own toolchain IS the mechanics (`word-docx` → OOXML,
`mck-ppt-design` → python-pptx, `mu-excel-toolbox` → its scripts).

This inventory is a snapshot. New skills will appear; re-scan what's actually
installed at routing time and wire them in by role (source / draft / mechanics
/ audit / gate). A missing named skill means substitute by role — never skip
the gate.

---

## 2. Chaining principles

1. **One skill per phase.** Never run two skills of the same niche in the same
   phase. Sequential phases may use different skills (draft with `Writing`,
   then de-slop with `humanizer`, then gate with
   `anti-ai-slop-writing-gate`).
2. **Artifacts flow forward.** Each phase emits a concrete artifact the next
   phase consumes: adjectives → tokens → copy → slides; research register →
   deck outline. Never re-derive upstream artifacts downstream.
3. **Mechanics first, craft second.** For file production: use the tested
   mechanical path (a built-in `docx`/`pptx`/`xlsx` skill if the environment
   has one, otherwise the craft skill's own toolchain), load the craft skill
   for rules, then write.
4. **No gate, no delivery.** Text exits through G1; visuals through G3; data
   claims through G4. A gate failure sends the artifact back one phase only.
5. **Lazy references.** Read the SKILL.md body; open `references/*.md` only
   when the current phase's instructions point to it.
6. **No re-editing gated output.** Once a gate approves, deliver as-is. If new
   facts arrive, rerun the full pipeline, not a spot edit.
7. **Open inventory.** The table in §1 is a snapshot, not a closed set. When a
   new skill is installed, slot it into the pipeline whose role it fills and
   extend the inventory; when one is removed, substitute by role. Never treat
   a name in this file as proof the skill exists — verify against the
   installed set first.

---

## 3. Decision tree — which pipeline?

| Task is… | Pipeline |
|---|---|
| A web page, app, dashboard, landing page | **A — Web UI** |
| A consulting/strategy/investor deck | **B — Deck** |
| A report, memo, proposal, article, docx/pdf | **C — Document** |
| Spreadsheet ops, data cleanup, analysis | **D — Data & Excel** |
| Brand, logo, VI system | **E — Brand** (then A/C reuse its tokens) |
| House writing style for a team/brand | **F — Voice** (run once, then feed C) |
| Only text to fix (already written) | C-lite: humanizer → detector → gate |
| Unclear | Ask one routing question; default to C with G1–G4 |

---

## 4. Canonical pipelines

### Pipeline A — Web UI / App
```
A0 DISCOVER   frontend-design-deslop Phase 0
              → OUT: artifact type, audience, 3–5 adjectives, DESIGN.md tokens
A1 BRAND      (only if no brand exists) brand-identity-design stages 1–3
              → OUT: palette + typography vocabulary feeding A0 adjectives
A2 BUILD      frontend implementation honoring DESIGN.md; built-in
              fullstack-dev skill if present
A3 MOTION     apple-design pass IF the UI is gesture/animation-heavy
A4 AUDIT      design-taste review_site loop: fix → re-audit until clean
A5 COPY GATE  all human-facing strings → humanizer → gate (G1)
```

### Pipeline B — Consulting deck
```
B0 RESEARCH   elite-ppt-pro Step 1 (mandatory, never skipped)
              → OUT: ≥15 data points, ≥5 sources, source register
B1 NARRATIVE  elite-ppt-pro rules: conclusion-as-title outline, page plan
B2 PRODUCE    mck-ppt-design: layouts, guard rails, BLOCK_ARC charts (python-pptx)
B3 QC         verify every number by code; design-taste pass on rendered slides
B4 TEXT GATE  body copy → humanizer → anti-ai-slop-writing-gate (G1)
```

### Pipeline C — Document (docx / pdf / article)
```
C0 INTENT     Writing (prose) OR clear-writing (docs/technical)
              diagnose: reader doesn't get it (structure) vs doesn't believe
              it's you (voice). Pick the fix, not both.
C1 VOICE      if PROSE.md exists (from F) it governs; if not, offer to build it
C2 DRAFT      write the full draft with the C0 skill's rules loaded
C3 DE-SLOP    humanizer rewrite pass (patterns + personality)
C4 GATE       anti-ai-slop-writing-gate pre-send mode → Clean Version
C5 DENSITY    slop-detector report on long docs (>1,500 words)
C6 PRODUCE    word-docx rules (OOXML); microtypography polish for published
              body text; render-verify pages
C7 FINAL QC   Anti-Slop pass: information density + originality (G4)
```
C-lite (fixing existing text only): C3 → C5 → C4. Never re-draft.

### Pipeline D — Data & Excel
```
D0 SAFETY     mu-excel-toolbox IRON LAW: peek structure → backup → operate
              streaming mode for files >100k rows; absolute paths only
D1 ANALYZE    toolbox scripts (merge/split/vlookup/dedup/validate)
D2 EXPLAIN    clear-writing for the summary sheet / README
D3 VERIFY     every reported number recomputed by code; Anti-Slop pass (G4)
```

### Pipeline E — Brand identity
```
E0 SIX STAGES brand-identity-design: soul discovery → concept → logo →
              color → typography → touchpoint manual; per-stage quality gates
E1 HANDOFF    export adjectives + palette + type as DESIGN.md and PROSE.md seeds
E2 REUSE      every later A/B/C pipeline consumes E1 artifacts; never re-invent
```

### Pipeline F — Voice codification (once per brand)
```
F0 BUILD      copywriting-prose-creator BUILD mode: interview + SOUL/TONE
              → PROSE.md (lexicon, sentence-length targets, banned words)
F1 AUDIT      AUDIT mode on an existing corpus before codifying, if corpus >50 pieces
F2 GOVERN     all future C/B writing tasks load PROSE.md first
```

---

## 5. Universal gates

| # | Gate | Applies to | Skill | Pass condition |
|---|---|---|---|---|
| G1 | Pre-send text gate | every substantive human-facing reply/deliverable text | `anti-ai-slop-writing-gate` | Clean Version, zero pattern findings, voice preserved |
| G2 | De-slop pass | AI-drafted prose before G1 | `humanizer` | Wikipedia-signs list clean; personality present |
| G3 | Visual audit | rendered pages, slides, posters, images | `design-taste` | No slop fingerprint; intentional not templated |
| G4 | Fact & density QC | numbers, claims, long documents | `Anti-Slop` + code recompute | Every sentence carries info; facts sourced; numbers exact |

Order for text: G2 → G1 → (G4 if long). Order for visuals: G3, then G1 on the
copy inside the visual.

---

## 6. Composition rules & anti-patterns

**Do**
- Name the phase out loud before running it ("A0 discover…").
- Pass artifacts explicitly: paste the adjectives/tokens/PROSE.md into the next
  phase's context rather than assuming it persists.
- Prefer the narrowest skill: motion question → `apple-design`, not the full
  design pipeline.
- For mixed tasks, split by artifact type and run one pipeline per artifact,
  sharing E/F outputs.

**Don't**
- Don't stack `humanizer` and gate-Edit mode back-to-back as editors —
  humanize first, then let the gate audit silently (pre-send mode).
- Don't use stylistic slop findings to accuse text of being AI-written
  (gate's own rule — evaluation of writing, not authorship).
- Don't let a craft skill overwrite tested mechanics (e.g., don't hand-roll
  OOXML when `word-docx` has a tested path).
- Don't skip B0 research for speed; an unsourced deck fails G4 by definition.
- Don't run Excel writes without D0 backup — data loss is unrecoverable.

---

## 7. Worked example — "launch package for a startup"

Request: brand + landing page + investor deck + one-pager.

1. **E0**: brand-identity-design → soul: "precise, warm, unbothered";
   palette: deep green + bone; type: serif display + humanist sans.
2. **F0**: copywriting-prose-creator BUILD → PROSE.md: short declarative
   sentences, no "revolutionary", 2nd person.
3. **A0–A5**: frontend-design-deslop locks landing-page tokens from E0 →
   build → apple-design hover/spring pass → design-taste audit (2 findings,
   fixed) → hero copy through G2+G1.
4. **B0–B4**: elite-ppt-pro research register (18 datapoints, 6 sources) →
   outline with conclusion-as-title → mck-ppt-design produces 14 slides →
   numbers recomputed by code → design-taste slide pass → speaker notes
   through G2+G1.
5. **C0–C7**: Writing drafts one-pager under PROSE.md → humanizer → gate →
   detector (density 0.8 markers/100w, clean) → docx produced via word-docx
   rules → render-verified → Anti-Slop final QC.
6. Deliver all four artifacts + the reusable DESIGN.md and PROSE.md.
