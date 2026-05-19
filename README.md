# Design Mockup Loop Skill

Portable Codex skill for turning live page screenshots into AI-generated desktop
and mobile GUI mockups.

The workflow is intentionally simple:

1. Capture clean viewport screenshots of the current page.
2. Analyze hierarchy, spacing, CTA path, trust signals, and visual constraints.
3. Generate two desktop mockup passes and two mobile mockup passes.
4. Choose the strongest direction and write implementation notes.
5. Optionally publish a static HTML review page.

## Install

Clone the repository and link the skill into your Codex skills directory:

```bash
git clone https://github.com/Appraisily/design-mockup-loop-skill.git
mkdir -p ~/.codex/skills
ln -s "$PWD/design-mockup-loop-skill/skills/design-mockup-loop" ~/.codex/skills/design-mockup-loop
```

Then trigger it with:

```text
$design-mockup-loop create desktop and mobile mockups for https://example.com/pricing
```

## Optional Environment

The skill works with local artifacts by default. Set these if you want a stable
output location or public review publishing:

```bash
export DESIGN_MOCKUP_EVIDENCE_ROOT="$PWD/runs"
export DESIGN_MOCKUP_PUBLIC_ROOT="$PWD/public/design-proposals"
export DESIGN_MOCKUP_PUBLIC_BASE_URL="https://example.com/design-proposals"
```

If `DESIGN_MOCKUP_PUBLIC_BASE_URL` is unset, the review page is saved locally and
the final answer should provide local artifact paths.

## Requirements

- A browser automation tool available to Codex, such as `agent-browser`,
  Playwright, or an equivalent browser skill.
- An image-generation capability available to Codex.
- Shell access for creating artifact folders and writing the review page.

No production code is changed by this skill unless a later implementation task
explicitly asks for it.

