# Mockup Prompt Templates

Use these as starting points. Keep prompts loose enough that the image model can
contribute original design ideas.

Replace bracketed placeholders with page-specific context from the screenshot
analysis.

## Desktop V1

```text
Use case: ui-mockup
Asset type: desktop web page redesign mockup
Input images: Image 1 is the current desktop screenshot reference. Use it for viewport shape, rough page structure, and content order.
Primary request: Create an ideal graphical user interface mockup for this [brand/product/domain] page, mapped to the exact same screenshot shape and desktop framing.
Style/medium: polished production web UI mockup, credible for [industry/domain], premium but practical.
Composition/framing: preserve the same viewport ratio and broad section order, but freely improve layout, hierarchy, spacing, imagery, cards, CTA treatment, and trust surfaces.
Text: keep text minimal and legible; do not invent detailed legal, pricing, guarantee, certification, or third-party claims.
Constraints: keep the page purpose clear, keep a strong primary CTA path, preserve product truth, avoid fake third-party logos or unsupported claims.
Avoid: generic SaaS dashboard styling, decorative clutter, illegible tiny text, dark blurred stock-photo hero treatment, one-note purple/blue gradient styling.
```

## Desktop V2

```text
Use case: ui-mockup
Asset type: second-pass desktop web page redesign mockup
Input images: Image 1 is the original desktop screenshot reference. Image 2 is the first generated mockup direction.
Primary request: Create a stronger second-pass desktop mockup that keeps the same screenshot shape and page purpose while improving the first direction.
Style/medium: polished production web UI, more refined hierarchy, clearer CTA path, stronger credibility and trust.
Composition/framing: preserve the original viewport ratio and rough content sequence; improve rhythm, density, alignment, section transitions, image usage, and component polish.
Constraints: keep only realistic claims, avoid fake logos, keep text sparse and readable, preserve implementation realism.
Avoid: over-designed marketing spectacle, illegible text blocks, unrealistic app screens, visual changes that ignore the original page structure.
```

## Mobile V1

```text
Use case: ui-mockup
Asset type: mobile web page redesign mockup
Input images: Image 1 is the current mobile screenshot reference. Use it for exact mobile viewport shape, visible content order, and page purpose.
Primary request: Create an ideal mobile graphical user interface mockup for this [brand/product/domain] page, mapped to the exact same mobile screenshot shape.
Style/medium: polished production mobile web UI, compact, credible, conversion-focused, easy to scan.
Composition/framing: preserve the same mobile viewport ratio and rough section order; freely improve spacing, hierarchy, sticky CTA behavior, cards, imagery, trust cues, and touch target clarity.
Text: keep text minimal, readable, and plausible; do not invent detailed claims.
Constraints: prioritize thumb-friendly CTA access, clear headline hierarchy, readable type, no overlapping sticky elements, realistic product/domain context.
Avoid: cramped cards, clipped text, overlapping UI, tiny buttons, fake app-store style screens, generic template look.
```

## Mobile V2

```text
Use case: ui-mockup
Asset type: second-pass mobile web page redesign mockup
Input images: Image 1 is the original mobile screenshot reference. Image 2 is the first generated mobile mockup direction.
Primary request: Create a stronger second-pass mobile mockup that preserves the exact mobile screenshot shape and improves the first design direction.
Style/medium: polished production mobile web UI, refined spacing, stronger scanning hierarchy, clearer CTA path.
Composition/framing: keep the same viewport ratio and broad content sequence; improve above-the-fold clarity, touch targets, trust cues, section rhythm, and sticky CTA integration.
Constraints: keep the design implementable in a responsive web page; avoid unsupported claims, fake logos, illegible copy, and overlapping text.
Avoid: dense desktop layout squeezed into mobile, decorative clutter, tiny multi-column content, modal-like cards inside cards.
```

