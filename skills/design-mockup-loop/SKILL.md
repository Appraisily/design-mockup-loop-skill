---
name: design-mockup-loop
description: Use for design-improvement workflows that turn live page screenshots into AI-generated GUI mockups. Trigger when the user asks to improve, redesign, critique, modernize, or mock up a web page using browser screenshots, visual analysis, and image generation; especially when they want a repeated loop of two desktop iterations and two mobile iterations mapped to the exact screenshot shape.
---

# Design Mockup Loop

Run a repeatable screenshot-to-mockup workflow: capture the real page, analyze
the visual problems, generate freer ideal-GUI mockups with image generation,
then iterate twice for desktop and twice for mobile.

## Tool Order

1. Use the available browser automation tool for page capture, viewport checks,
   evidence directories, console/network awareness, and screenshot chunking.
   Prefer `agent-browser` when it is available; Playwright or an equivalent
   browser skill is fine.
2. Use the available image-generation tool for GUI mockup generation from the
   captured screenshot references.

If either tool is unavailable, continue with the nearest available fallback and
state the fallback.

## Working Assumption

Before non-trivial work, state:

- the page or URL being improved
- the smallest intended output, usually mockups, a concise design brief, and an
  optional HTML review page
- the verification check, usually screenshot coverage for desktop and mobile,
  visual review of generated mockups, and a local or HTTPS check of the review
  page

Do not edit production code unless the user explicitly asks for implementation
after reviewing the mockups.

## Evidence Setup

Create one run folder:

```bash
RUN_ID="design-mockup-$(date +%Y%m%dT%H%M%S)"
EVIDENCE_ROOT="${DESIGN_MOCKUP_EVIDENCE_ROOT:-./design-mockup-runs}"
EVIDENCE_DIR="$EVIDENCE_ROOT/$RUN_ID"
mkdir -p "$EVIDENCE_DIR"
```

Create a public/review folder when the run is complete:

```bash
PAGE_SLUG="<page-slug>"
PUBLIC_RUN_ID="${PAGE_SLUG}-redesign-$(date +%Y%m%d)"
PUBLIC_ROOT="${DESIGN_MOCKUP_PUBLIC_ROOT:-$EVIDENCE_DIR/public}"
PUBLIC_DIR="$PUBLIC_ROOT/$PUBLIC_RUN_ID"
PUBLIC_BASE_URL="${DESIGN_MOCKUP_PUBLIC_BASE_URL:-}"
mkdir -p "$PUBLIC_DIR"
```

If `PUBLIC_BASE_URL` is set, the review URL is:

```bash
PUBLIC_URL="${PUBLIC_BASE_URL%/}/$PUBLIC_RUN_ID/index.html"
```

If it is unset, publish only to the local `PUBLIC_DIR`.

Use descriptive filenames:

- `desktop-original-y-0.png`
- `desktop-original-y-720.png`
- `desktop-mockup-v1.png`
- `desktop-mockup-v2.png`
- `mobile-original-y-0.png`
- `mobile-mockup-v1.png`
- `mobile-mockup-v2.png`
- `redesign-brief.md`
- `index.html`

Keep screenshots viewport-sized. Avoid full-page screenshots for long pages
because they downscale badly and reduce useful visual signal.

## Capture Workflow

Use a named browser session.

Desktop default with `agent-browser`:

```bash
agent-browser --session-name design-mockup open "<URL>"
agent-browser set viewport 1440 900
agent-browser wait --load networkidle
agent-browser wait 1000
agent-browser screenshot "$EVIDENCE_DIR/desktop-original-y-0.png"
```

Mobile default with `agent-browser`:

```bash
agent-browser --session-name design-mockup-mobile open "<URL>"
agent-browser set viewport 390 844
agent-browser wait --load networkidle
agent-browser wait 1000
agent-browser screenshot "$EVIDENCE_DIR/mobile-original-y-0.png"
```

For long pages, capture viewport chunks with 120-180px overlap. Prioritize the
hero, main decision section, pricing/order/CTA section, and any visually weak
section the user mentioned.

Also collect browser errors and 4xx/5xx network requests if your browser tool
supports it.

Only use annotated screenshots when checking interaction affordances. Use clean
screenshots for design mockups.

## Visual Analysis

Inspect each screenshot before prompting image generation. Write a short
analysis covering:

- layout shape: viewport size, section boundaries, nav/header/footer, sticky
  elements
- content hierarchy: headline, value proposition, trust signals, order/CTA path
- visual issues: spacing, density, contrast, alignment, typography, imagery,
  repeated card patterns
- constraints to preserve: brand, copy that must remain, product truth, page
  purpose, visible CTA behavior
- opportunities for creative freedom: composition, imagery, layout rhythm,
  richer GUI polish

Keep this analysis brief. Its job is to inform the mockup prompt, not to become
a full audit.

## Image Generation Loop

Run exactly four mockup passes unless the user asks otherwise:

1. Desktop v1 from desktop original screenshot.
2. Desktop v2 from desktop original screenshot plus desktop v1, asking for a
   stronger revision.
3. Mobile v1 from mobile original screenshot.
4. Mobile v2 from mobile original screenshot plus mobile v1, asking for a
   stronger revision.

Use the captured screenshot as a reference image, not an edit target. Ask for an
ideal graphical user interface mapped to the same screenshot shape and viewport
ratio.

Do not overconstrain the image model. Preserve only the important invariants:

- same page purpose and rough content order
- same viewport shape and framing
- credible fit for the actual brand/product/domain
- clear primary CTA path
- no fake unsupported claims, fake logos, or unreadable tiny text

Allow the model freedom in visual composition, spacing, imagery treatment,
components, and polish.

Read `references/mockup-prompts.md` when drafting image-generation prompts.

## Selection And Brief

After the four passes, compare mockups against the original screenshots and
choose the strongest direction for desktop and mobile.

Create `redesign-brief.md` with:

- original page URL
- target audience and page purpose
- strongest desktop and mobile directions
- what changed visually
- implementation notes
- risks and constraints

## HTML Review Page

Create `index.html` in `PUBLIC_DIR`. The page should:

- show the page URL, run date, and clear "no production code changed" status
- put the recommended desktop and mobile mockups first
- include all four mockup images as clickable full-size links
- link to the design brief
- use relative image paths so the folder can be moved or archived safely
- avoid external JavaScript and unnecessary third-party assets

Verify locally:

```bash
test -s "$PUBLIC_DIR/index.html"
test -s "$PUBLIC_DIR/desktop-mockup-v2.png"
```

If `PUBLIC_URL` is configured, verify over HTTPS:

```bash
curl -fsSI "$PUBLIC_URL" | head
curl -fsSI "${PUBLIC_URL%/index.html}/desktop-mockup-v2.png" | head
```

Deliver:

- the public review URL, or local review path if no public URL is configured
- saved artifact paths
- which mockup direction is strongest and why
- 5-10 concrete implementation notes that map visual changes back to actual UI
  work
- any risks, such as text hallucination, unrealistic imagery, missing product
  constraints, or elements that would be expensive to implement

## Quality Scale

Use this scale to judge each mockup:

- 1: Decorative only; ignores the page shape or product purpose.
- 2: Some visual polish, but weak hierarchy or unrealistic fit.
- 3: Usable design direction; preserves page intent but needs practical cleanup.
- 4: Strong direction; clear hierarchy, credible brand fit, implementable with
  focused changes.
- 5: Excellent direction; noticeably better UX, strong fit, responsive
  implications are clear, and implementation path is realistic.

Prefer a 4 that can be implemented over a 5-looking concept that depends on
false claims, impossible assets, or fragile layout.

