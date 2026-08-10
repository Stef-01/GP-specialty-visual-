# Care Personas

**Two interactive, animation-first explainers** applying precision-persona segmentation (in the
tradition of Surgo Ventures' vaccine-persona explainer) to health-system design. This repo is the
product's canonical home — a standalone static site, no build step, no framework.

| Page | What it argues | Audience |
|---|---|---|
| `index.html` | Product hub — the shared method | Everyone |
| `specialist-gp-pipeline.html` | **Explainer 01 — The Specialist-GP Pipeline.** Two patient pools, side by side: a GP's book and a cardiologist's referred pool. A complexity screen sorts the specialist pool; its bottom 20% crosses into the GP pool with an eConsult guidance line back. GPs bill more, specialists bill more, the waitlist halves. | Hospital networks, the CMO |
| `matched-family-gp.html` | **Explainer 02 — The Matched Family GP.** Patients matched in a 20-second conversation to doctors who asked for exactly them (culture, food, judgement-free care, wearables, whole-family), then kept by an outbound follow-up engine driven by EHR-predicted need. | Primary-care networks, payers |
| `executive-summary.html` | **One-pager.** Both concepts, their economics and both asks on a single sheet — prints to exactly one A4 page. | The room |

## The animation layer

Each explainer opens with a scroll-triggered **person-figure theatre** (`assets/dotfield.js`,
zero libraries): 200 procedurally drawn people — varied skin and hair, faces that resolve when the
camera zooms close — who take persona colours under a scan sweep, form ranked queues and clusters,
and play out each story's choreography before settling into stat count-ups. **Scroll selects the
scene; crossing a step threshold fires a timed, eased tween (≤1.4 s) that always completes** —
parking the page anywhere settles on a composed scene, never a mid-transition frame (the
interaction model of the reference explainer; adopted after the ten-stage RCA in
`docs/rca-animation-postmortem.md`). Labels are bound to their group's live centroid, so
annotations travel with their people.

It is held to a **numeric clutter budget** derived from the original explainer (Scrollama + D3
animated circles; 24–41-word captions; 5 hues; ~3 element groups per scene) and published
scrollytelling research — one caption at a time, only composed scenes may persist on screen,
≤ 2 camera moves per story, zero idle motion after settle, `prefers-reduced-motion` gets static
composed frames. Full budget table and data model: `docs/plan.md`.

## Testing

`tests/theatre-qa.mjs` is the standing QA gate born of that RCA: it drives **real scrolling**
(not test-API poses) at 390/1280/2000 px, parks 60% into transitions and byte-compares what
settles against the composed target scene, then checks stillness after settle, completed scan
sweeps at rest, one-caption-at-a-time discipline, mobile overflow and reduced-motion stillness.

```sh
node tests/theatre-qa.mjs          # full matrix
node tests/theatre-qa.mjs --quick  # widest viewport only
```

Requires Node ≥ 18 with Playwright + Chromium available (a global install works —
`createRequire` honours `NODE_PATH`).

## Hosting

Deploy the repo root to any static host:

- **Vercel / Netlify:** import this repo, framework preset "Other", no build command.
- **GitHub Pages:** Settings → Pages → deploy from `main` root.

Only external dependency: Google Fonts.

## Method notes

All monetary values are indicative AUD rounded from published MBS schedule fees; all rates are
illustrative modelling assumptions, stated openly — each explainer ends with the trial designed
to measure them. The 5-colour persona palette is validated for colour-vision deficiency
(adjacent-pair CVD ΔE ≥ 8, OKLab×100); sub-3:1-contrast hues are relieved with labels, legend
chips and table views. All demo clinicians are fictional.
