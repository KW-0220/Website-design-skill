---
name: website-design
description: One entry point for designing and building websites, landing pages and web UI. Use whenever the user asks to design, redesign, style, polish, audit or screenshot a site, page or component, pick a visual direction ("make it look like Linear/Stripe/Apple"), add a 3D/Three.js hero or object, or invokes /website-design. Routes the work through a bundled library of 84 DESIGN.md design systems and the installed taste-skill, ui-ux-pro-max, impeccable, playwright and img2threejs skills.
---

# Website design

This skill groups six tools into one workflow. It owns the order of work; each step hands off
to the specialised skill named there. Invoke that skill with the Skill tool when you reach the
step — don't load them all up front.

| Tool | Where it lives | Used for |
|---|---|---|
| **DESIGN.md library** (VoltAgent/awesome-design-md) | bundled: `design-md/` next to this file | A ready design language: tokens + rules from 84 real sites |
| **taste-skill** | plugin `taste-skill@taste-skill`, skill `design-taste-frontend` | Anti-generic taste: layout, type and motion choices that don't look AI-made |
| **ui-ux-pro-max** | plugin `ui-ux-pro-max@ui-ux-pro-max-skill`, skills `ui-ux-pro-max`, `design-system`, `ui-styling`, `brand`, `banner-design` | Searchable database of styles, palettes, font pairings, stack-specific guidance |
| **impeccable** | plugin `impeccable@impeccable`, skill `impeccable` + `npx impeccable detect` | Design critique/audit; its hook scans UI files after every edit |
| **Playwright** | `npx --yes @playwright/cli` (skill `playwright-cli` if installed), plus the Playwright MCP tools when present | Open the page, snapshot, screenshot at breakpoints, verify interactions |
| **img2threejs** | user skill `img2threejs` (install below if missing) | Rebuild an object from a reference image as procedural Three.js code |

## Missing tools (other repos / new sessions)
This skill may run in a project that doesn't have the other tools. Before step 1, check the
skill list; for each missing piece, install it at user scope (Claude Code), then tell the user to
start a new session so the new skills load — or read the installed SKILL.md directly to continue now:

```bash
claude plugin marketplace add leonxlnx/taste-skill && claude plugin install taste-skill@taste-skill --scope user
claude plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill && claude plugin install ui-ux-pro-max@ui-ux-pro-max-skill --scope user
claude plugin marketplace add pbakaus/impeccable && claude plugin install impeccable@impeccable --scope user
npx --yes img2threejs@latest install --host claude
```
Playwright needs no install: use `npx --yes @playwright/cli` (or Playwright MCP tools).
Where installing isn't possible (claude.ai chat, no shell), follow the workflow using the
bundled `design-md/` library and the rules written here.

## Workflow

### 1. Brief
Pin down, asking only what you can't infer: purpose of the page, audience, primary action,
content available, stack (check `package.json`, framework config and `components.json`), and
any reference site or mood. If the user named a site or vibe, go to step 2a; otherwise 2b.

### 2. Choose the design language
**2a — from a reference.** Open `design-md/INDEX.md`, pick the closest 1–2 entries, read
`design-md/<slug>.md`. Copy the chosen file to the project root as `DESIGN.md` (adapt name,
brand colour and fonts to the user's brand — never ship another company's logo, name, imagery
or copy). If the font is proprietary, substitute the closest free font and note it.

**2b — from scratch.** Invoke `ui-ux-pro-max` (or its `design-system` skill) to search styles,
palettes and font pairings for the product type, then write the result as `DESIGN.md` in the
same token format as the library files.

Either way, `DESIGN.md` at the project root is the single source of truth for tokens from here on.

### 3. Set direction and taste
Invoke `design-taste-frontend` (taste-skill) before writing UI. Apply its rules on hierarchy,
spacing rhythm, typography scale, motion and avoiding generic AI patterns on top of `DESIGN.md`.
Where they conflict, `DESIGN.md` wins on tokens; taste-skill wins on composition.

### 4. Build
- Map `DESIGN.md` tokens to CSS variables / Tailwind theme once; never hard-code colours.
- Use `ui-styling` (ui-ux-pro-max) for stack-specific component patterns (shadcn/ui, Tailwind).
- Responsive from the start: 360, 768, 1280 px.
- Accessibility: contrast ≥ 4.5:1 for body text, visible focus, semantic landmarks, alt text.

**3D / Three.js elements.** If the page needs a 3D object built from a reference image (product
hero, mascot, model viewer), invoke `img2threejs`. If it isn't in the skill list, install it:
`npx --yes img2threejs@latest install --host claude` (needs Node ≥ 18, Git, Python ≥ 3.10),
then start a new session or read `~/.claude/skills/img2threejs/SKILL.md` directly. Mount the
generated model in a lazily-loaded component so it doesn't block first paint.

### 5. Verify in a real browser
Start the dev server (`npm run dev`, default http://localhost:5173), then with `npx --yes @playwright/cli`
(or Playwright MCP tools):
1. `npx --yes @playwright/cli open http://localhost:5173` → `snapshot` to check structure.
2. Screenshot at 360, 768 and 1280 widths (`resize`, then `screenshot`); look at each image.
3. Click through the primary action and any interactive pieces; check the console for errors.
`close` when done. External sites may fail through the cloud proxy; localhost works.

### 6. Audit and polish
- Run `npx impeccable detect src/` (source rules) and `npx impeccable detect http://localhost:5173`
  (rendered rules: contrast, tiny text). Exit code 2 = findings to fix.
- Invoke `impeccable` for a critique pass ("audit", "polish") and fix what it raises.
- Re-screenshot after fixes. Done = no impeccable findings you haven't consciously accepted,
  layouts hold at all three widths, and the page matches `DESIGN.md`.

## Quick routes
- "Make it look like X" → 2a → 4 → 5.
- "Review/improve this page" → 5 → 6 (skip build).
- "Pick colours/fonts" → 2b only.
- "Turn this image into 3D" → `img2threejs` directly.
