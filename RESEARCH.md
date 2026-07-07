# Competitive Landscape Research

Research pass on who else is building in this space, run 2026-07-07 via a
multi-agent web research harness (5 parallel search angles, 15 sources
fetched, each claim adversarially re-verified by 3 independent votes before
being kept). Findings below are the claims that survived verification;
refuted claims are listed separately in [Caveats](#caveats) so they don't get
quietly cited later.

**This is a fast-moving space.** Several of the funding, pricing, and
feature-set facts below are from mid-to-late 2025 and June 2026. Re-verify
anything load-bearing before acting on it if this doc is being read more than
a month or two after the date above.

## Summary

The landscape for "canvas-first design tool + live preview + AI-generated
semantic HTML/CSS" is crowded and consolidating fast. The two closest
analogs — **Onlook** (open-source, canvas-first, code-as-source-of-truth
editor) and **Anthropic's own Claude Design** (code-powered, live-preview
prototypes, built on Opus 4.7) — already cover large parts of the proposed
feature set. AI-native "vibe coding" tools (Lovable, v0, Bolt) have proven
the broader category is enormous and heavily funded, but they're
chat/prompt-first rather than canvas-first, and mostly target React app
scaffolding rather than shipping static marketing-site HTML/CSS. Design-to-
code conversion tools show two different codegen architectures already in
production — deterministic compilers vs. LLM-generated markup with cleanup
overhead — which means "an LLM that writes production-clean semantic HTML
reliably" is a real, not-yet-fully-solved claim, if it can actually be
delivered.

The single biggest strategic risk is **platform risk**: Anthropic, the API
vendor this project would depend on, already ships an adjacent
code-generation, live-preview, HTML-exporting product built on its own
flagship model. That narrows the window for a third party to build
essentially the same thing on top of the Claude API before Anthropic's
first-party tool matures into direct overlap.

## Direct analogs

### Onlook — closest existing analog
Open-source (Apache 2.0, ~17.6k GitHub stars), canvas-based visual editor for
React/Next.js + Tailwind, where **code is the literal source of truth**, not
a design file that gets translated to code later. Free self-hosted, or a
paid cloud tier (Free / Pro $25/mo / Enterprise). Users can AI-prompt whole
features in natural language ("design me an inventory tracker website"), and
the tool also supports direct visual DOM manipulation — drag/drop, a
Tailwind style toolbar. Build-time attributes bidirectionally link canvas
elements back to AST-parsed code. Onlook explicitly markets itself as *"an
open-source alternative to Bolt.new, Lovable, V0, Replit Agent, Figma Make,
Webflow."*

This is the highest feature-overlap competitor found: canvas UX + AI codegen
+ code transparency + live sync are all present. The gap: it's scoped to
React/Next.js component code, not static marketing-site HTML/CSS.
— confidence: high · [onlook.com](https://www.onlook.com/) ·
[github.com/onlook-dev/onlook](https://github.com/onlook-dev/onlook) ·
[architecture docs](https://docs.onlook.com/developers/architecture)

### Claude Design (Anthropic)
Anthropic ships its own tool for building "code-powered prototypes" with
live preview, voice/video/shaders/3D support, and exportable standalone
HTML — running on Anthropic's flagship vision model (Opus 4.7), not just the
general-purpose API. Important nuance: it's **conversational and
comment/slider-driven, not a Figma-style drag-and-drop canvas**, and targets
a broader non-designer audience (slides, one-pagers, general prototypes)
rather specifically shippable marketing sites. So the overlap is real on the
code-transparency/live-preview axis, but only partial on canvas UX and
narrow use-case fit. A stronger claim — that Claude Design fully replicates
canvas + AI codegen + code editor + build handoff — was checked and
**refuted**; don't overstate the overlap.
— confidence: medium (partial-overlap framing), high (platform-risk framing)
· [anthropic.com/news/claude-design-anthropic-labs](https://www.anthropic.com/news/claude-design-anthropic-labs)
· [anthropic.com/news/claude-opus-4-7](https://www.anthropic.com/news/claude-opus-4-7)

## AI-native "vibe coding" / prompt-to-app tools

### Lovable
The category's clearest proof that this space is enormous and investable:
**$330M Series B at a $6.6B valuation** (CapitalG + Menlo Ventures'
Anthology fund, plus Khosla, Salesforce Ventures, Databricks Ventures,
NVentures, Atlassian Ventures, HubSpot Ventures). Reported to have crossed
**$100M ARR within 8 months of launch** and **$200M ARR four months after
that** — one of the fastest revenue ramps recorded for a software startup
(medium confidence on the exact ARR figures; the funding round itself is
high confidence). Lovable is chat/prompt-first, targeting full React app
generation rather than canvas-driven static site design.
— confidence: high (funding), medium (ARR) ·
[lovable.dev/blog/series-b](https://lovable.dev/blog/series-b) ·
[TechCrunch](https://techcrunch.com/2025/12/18/vibe-coding-startup-lovable-raises-330m-at-a-6-6b-valuation/)

*v0 (Vercel), Bolt.new (StackBlitz), and Replit Agent* are named in this
category by the original research brief but no claim about them survived
3-vote verification in this pass — treat their inclusion here as a known gap
(see [Open Questions](#open-questions)), not an endorsement or dismissal.

## Design-to-code conversion tools

Two genuinely different codegen architectures are already in production
here — worth internalizing before picking one for this project:

### Subframe — deterministic compiler
Canvas-based React/Tailwind design tool where code generation is a **fixed,
predictable transform** (a compiler step), not LLM re-generation at export
time. AI assists earlier, during design/placement, but the design-state → code
mapping itself is deterministic. Subframe's own docs describe generation as
"deterministic and purely presentational."
— confidence: medium · [subframe.com](https://www.subframe.com/tips/best-visual-editors-react-tailwind) ·
[docs.subframe.com](https://docs.subframe.com/concepts/design-to-code)

### Builder.io Visual Copilot — hybrid AI model + compiler
Converts Figma designs to code using a specialized AI model (trained on 2M+
data points) combined with the open-source **Mitosis** compiler, targeting
8+ frameworks (React, Vue, Svelte, Angular, Qwik, Solid, HTML, plus mobile).
Includes AI-driven component mapping that links Figma components to a user's
*existing* codebase components instead of regenerating everything from
scratch — a meaningfully different (and harder) problem than greenfield
generation. A separate claim about proprietary-SDK vendor lock-in was
checked and refuted.
— confidence: high · [builder.io/blog/figma-to-code-visual-copilot](https://www.builder.io/blog/figma-to-code-visual-copilot) ·
[builder.io/blog/mitosis-get-started](https://www.builder.io/blog/mitosis-get-started)

### Anima — LLM output, known failure mode
Figma-to-code plugin that produces flat HTML with absolute/semi-absolute CSS
positioning, requiring substantial manual refactoring before production
use — positioning it as a prototyping/handoff tool, not a direct-to-
production generator. This is corroborated by Anima's own support forum and
implicitly by Anima's own blog post introducing "Auto-Flexbox" specifically
to address it. This is a **concrete, named failure mode** ("div soup," lack
of semantic/flex structure) that a real semantic-HTML-first generator could
differentiate directly against — if it can reliably avoid the same trap.
— confidence: high · [Anima forum](https://forum.animaapp.com/t/auto-layout-from-figma-still-shows-absolute-positioning/229) ·
[Anima's own Auto-Flexbox post](https://www.animaapp.com/blog/design-to-code/introducing-auto-flexbox)

### TeleportHQ — LLM prompt-to-site, budget pricing
Generates layouts/components from natural-language prompts using an LLM
(ChatGPT-powered, plus a newer conversational "Copilot" editing mode).
Output quality depends on prompt clarity. Professional tier: $18/mo ($9/mo
billed annually), custom-priced Agency tier, 7-day free trial. Useful as a
**low-end pricing anchor** for this category.
— confidence: medium (LLM-generation claim), high (pricing) ·
[teleporthq.io/pricing](https://teleporthq.io/pricing)

## Open Questions

Categories from the original research scope that returned **no claims
surviving verification** — real gaps in this pass, not a signal that these
products don't matter:

- **Uizard, Visily, Framer AI** (AI UI/UX generators) — uncovered.
- **Webflow, Figma Dev Mode, Figma-to-code plugins broadly, Locofy.ai as a
  whole, Replit Agent** — uncovered. Webflow in particular is a named
  reference point in the project brief ("faster than Webflow's learning
  curve") and deserves a dedicated follow-up pass.
- **Static HTML/CSS vs. component frameworks**: nearly every competitor
  found (Onlook, Builder.io, Lovable) targets React/Next.js/Tailwind output,
  not plain static HTML/CSS for marketing sites. Is "static-HTML-first" an
  underserved niche, or a niche nobody wants because frameworks have already
  won? Unresolved — worth validating with actual target users before betting
  the architecture on it.
- **Anthropic's roadmap intent for Claude Design**: staying a general
  prototyping tool for non-designers, or is there any signal it grows into a
  canvas-first tool that fully subsumes this project's idea? Unknown from
  this pass.
- **Which codegen architecture actually produces cleaner semantic HTML for
  a *static marketing site* specifically** — deterministic compiler
  (Subframe, Builder.io/Mitosis) vs. LLM generation (Anima, TeleportHQ)?
  None of the sources tested this narrower use case directly; this is
  probably the single most important open technical question for this
  project's architecture decision (see `diagrams/05-code-generation-pipeline.html`).

## Synthesis: where's the actual gap?

1. **Static, semantic HTML/CSS output (not a JS framework) is the least
   contested part of the map.** Every strong competitor found targets
   React/Next.js/Tailwind component output. If the target user genuinely
   wants a plain HTML/CSS site they can drop on any host with no build step,
   that's real whitespace — but it's unvalidated whitespace (see Open
   Questions above), not a confirmed opportunity.
2. **"LLM writes clean semantic markup reliably" is a real, unsolved
   problem, not a solved one.** Anima's known "div soup" failure and the
   deterministic-vs-LLM split among incumbents both point the same
   direction: nobody has convincingly nailed AI-generated *production-grade*
   semantic HTML yet. That's a legitimate technical bet to make — but it's a
   hard bet, not a differentiation freebie.
3. **Canvas-first UX is not unclaimed territory.** Onlook already occupies
   almost exactly this position (canvas + live code sync + AI codegen +
   open transparency), just for component frameworks instead of static
   sites. Positioning against Onlook specifically, not just against
   Webflow/Lovable in the abstract, should shape the pitch.

## Biggest risks

- **Platform risk is the sharpest one.** The project's core dependency
  (Claude API) is made by a company that already ships an adjacent
  code-generation, live-preview product. If Anthropic extends Claude Design
  toward canvas-first drag-and-drop, that's not a competitor — it's the
  platform vendor absorbing the product.
- **Capital and speed.** Lovable's funding and revenue trajectory show
  incumbents in the broader "AI builds your site/app" category are moving
  and scaling fast. A new entrant needs a genuinely different wedge (static
  HTML output, code transparency, design-tool-grade canvas fidelity), not
  just "the same thing, but newer."
- **Unvalidated core assumption.** The "static HTML/CSS is underserved"
  thesis this whole project leans on hasn't been tested against real target
  users — it's inferred from the absence of direct competitors, which is
  suggestive, not proof of demand.

## Caveats

Claims that were checked and **explicitly refuted** during verification —
listed here so they don't quietly get treated as fact later:

- Claude Design as a full clone of the proposed feature set (canvas + AI
  codegen + code editor + build handoff) — refuted 1-2. The defensible
  version is *partial* overlap (code transparency + live preview only).
- Any claim that Figma-to-code tools uniformly use deterministic-only
  generation, or that they hit a specific completion percentage (e.g.
  "~75% complete," "20-40% manual refinement") — refuted. Treat any
  precise completion-rate figure for this category as unverified until
  sourced directly.
- Lovable's "100,000+ projects/day, 25M+ total projects" usage-scale
  claim — refuted, don't cite it.
- Builder.io proprietary-SDK vendor lock-in claim — refuted (0-3).

Several sources behind medium-confidence findings are vendor blogs or single
third-party comparison sites (makerstack.co, sixtythirtyten.co, sitegrade.io,
aidesigner.ai) rather than primary documentation — that's reflected in the
confidence ratings above, not hidden.

---
*Generated via a multi-agent research harness: 5 parallel search angles → 15
sources fetched → adversarial 3-vote verification per claim → synthesis.
Full methodology in the harness itself, not reproduced here.*
