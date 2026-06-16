# Figma Build Plan — "Prompt A" execution (queued)

This is exactly what I will run via the `figma-generate-library` + `figma-use` skills the moment the `use_figma` MCP tool is connected. It already completes the skill's **Phase 0 (Discovery)** from your real code; on connect I resume at the Phase 0 checkpoint → Phase 1.

## Phase 0 — Discovery (DONE from codebase; Figma-side pending)
- ✅ Codebase analyzed: `tokens.scss`, `typography.scss`, 12 components.
- ✅ Token set + component list locked (see `tokens.dtcg.json`, `component-spec.md`).
- ⏳ On connect: inspect the target Figma file (pages, existing variables/styles, naming conventions) and reconcile any conflicts before writing.
- ✋ Checkpoint: confirm scope, then proceed.

## Phase 1 — Foundations (variables first, always)
Collections + modes:
1. **Primitives** (mode: `Value`) — the `primitive.*` palette. Scope `[]` (hidden).
2. **Color** (modes: `Dark`, `Light`, `High Contrast`, `Light High Contrast`) — the 23 `color.*` semantics, each mode value aliased to a Primitive. Scopes: backgrounds → `FRAME_FILL, SHAPE_FILL`; text/eyebrow → `TEXT_FILL`; borders → `STROKE_COLOR`; glow → `EFFECT_COLOR`; gradient stops → `FRAME_FILL, SHAPE_FILL`.
3. **Spacing** (mode `Value`) — `spacing.*`. Scope `GAP, WIDTH_HEIGHT`.
4. **Radius** (mode `Value`) — `radius.*`. Scope `CORNER_RADIUS`.
5. **Sizing** (modes: 4, for mode-aware `border-width.metallic`) — + `border-width.hairline`, `layout.*`. Scope `STROKE_FLOAT` / `WIDTH_HEIGHT`.
6. **Typography weight** (modes: 4) — `typography.weight.eyebrow` (700/900). Scope `FONT_WEIGHT`.

Then:
- **Text styles** — 6 from `typography.style.*` (display, eyebrow, heading, subtitle, body, caption). Two desktop/mobile sizes per style (Figma can't store clamp); load `DM Sans` (900/700/600/500) + `Lora` (400) via `listAvailableFontsAsync` before writing.
- **Effect style** — `effect.accent-glow` bound to `color.accent-glow`.
- **Code syntax** — WEB `var(--…)` on every variable (from each token's `$extensions.web`).
- ✋ Checkpoint: variable summary (6 collections, ~40 vars, 4 modes, 6 text styles, 1 effect).

## Phase 2 — File structure
Pages: `Cover` → `Getting Started` → `Foundations` (color swatches × 4 modes, type specimen, spacing/radius scales) → `---` → component pages → `---` → `Templates`.
- ✋ Checkpoint: page list + screenshot.

## Phase 3 — Components (one page each, atoms → molecules → organisms)
Order: TextLink → Tag → IconButton → ActionCall → TagList → ArticleCard → CrossSection → NavigationBar → (Templates: Hero/About/Experience/Projects/Contact).
Per component: build base w/ auto-layout + full token bindings → variant matrix (see `component-spec.md`, all ≤30) → component properties → validate (`get_metadata` + `screenshot`) → ✋ checkpoint.

## Phase 4 — Integration + QA
Code Connect mapping back to `src/app/components/*`, contrast audit (the 4 modes already target WCAG AA+), naming audit, unresolved-binding audit, final screenshots.

---

### To connect the Figma MCP
The `use_figma` toolset (Figma's plugin-based agent MCP) isn't registered in this Claude Code instance and isn't in the connector registry. Add it via your Figma MCP setup, then tell me "Figma is connected" and I'll run this plan from Phase 0 checkpoint. Run `/mcp` to see what's currently wired up.
