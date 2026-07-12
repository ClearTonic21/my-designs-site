# Component Spec — ClearTonic UI Kit base

Source of truth for the Figma (or claude.ai/design) component build. Twelve components extracted from `src/app/components/`. Build in dependency order: **atoms → molecules → organisms → templates**. Every fill / stroke / radius / gap / padding binds to a token from `tokens.dtcg.json` — no hardcoded values.

Legend: **Props** = component-property API to expose in Figma (TEXT / BOOLEAN / INSTANCE_SWAP / VARIANT). **States** = interaction states to build as variants or document.

---

## Atoms

### TextLink  (`text-link`)
The global `.link-underline` primitive — text turns accent + underline wipes in on hover.
- **Props:** `label` (TEXT), `leadingIcon` (INSTANCE_SWAP, optional), `trailingIcon` (INSTANCE_SWAP, optional), `external` (BOOLEAN → adds ArrowUpRight + `rel="noopener noreferrer"` semantics).
- **Variants:** State = Default / Hover / Focus-visible.
- **Tokens:** text → `color.text-primary`; hover text + underline → `color.accent`; focus ring → `color.accent` (2px, offset 3px).

### Tag / Pill  (child of `tag-list`)
- **Props:** `label` (TEXT).
- **Variants:** none (single visual).
- **Tokens:** bg `color.background-subtle`, text `color.text-secondary`, border `color.border-default` (1px), radius `radius.pill`, padding 4px / `spacing.2`, type `typography.style.caption` (0.68rem, weight 600, tracking 0.08em).

### IconButton  (`icon-button`)
Square icon-only control (used by nav for theme / contrast / collapse).
- **Props:** `icon` (INSTANCE_SWAP), `aria-label` (TEXT — required, documented), `pressed` (BOOLEAN → `aria-pressed`).
- **Variants:** State = Default / Hover / Focus-visible / Pressed.
- **Tokens:** icon `color.text-primary` → `color.accent` on hover; focus ring `color.accent`. Min target 44×44 (a11y).

### ActionCall  (`action-call`)  — the primary button
"Grab my Resume ↗" CTA. Maps to `.button-primary` / `.button-ghost`.
- **Props:** `label` (TEXT), `arrowIcon` (BOOLEAN → trailing ArrowUpRight), `variant` (VARIANT: Primary / Ghost), `external` (BOOLEAN → `target="_blank"` semantics).
- **Variants:** Variant = Primary / Ghost × State = Default / Hover / Focus-visible / Disabled  (cap ≤ 30 — this is 8).
- **Tokens:** Primary — bg `color.accent`, text `color.text-inverted`, border 2px `color.accent`, radius `radius.pill`, padding 12px / 28px; Hover → bg transparent, text `color.accent`. Ghost — bg transparent, text `color.accent`, hover bg `color.accent-glow`. Type: DM Sans 700, 0.82rem, tracking 0.12em, uppercase.

---

## Molecules

### TagList  (`tag-list`)
- **Props:** `categoryLabel` (TEXT, `typography.style.eyebrow`), `tags` (instance list of Tag).
- **Layout:** auto-layout column; label + wrapped row of Tag pills, gap `spacing.1`.

### ArticleCard  (`article-card`)
Used by both **Projects** and **Experience** (timeline). The metallic-border card.
- **Props:** `eyebrow` (TEXT), `title` (TEXT, `typography.style.heading`), `image` (INSTANCE_SWAP / fill, optional → placeholder when empty), `body` (TEXT, `typography.style.body`), `tagSlot` (INSTANCE_SWAP → TagList), `ctaSlot` (INSTANCE_SWAP → TextLink/ActionCall), `fullPage` (BOOLEAN → 100% width), `hasImage` (BOOLEAN).
- **Variants:** State = Default / Hover (gold→teal metallic border swap, translateY −3px).
- **Tokens:** surface `color.background-surface`, metallic border via `gold-fade/mid/peak` → `teal-fade/mid/peak` gradient at `border-width.metallic`, radius `radius.lg`, inner padding `spacing.4`. Hover border → teal stops; glow `effect.accent-glow`.

### CrossSection  (`cross-section`)
Section wrapper / shell — owns the section background.
- **Props:** `glass` (BOOLEAN → frosted overlay + gold top/bottom borders that brighten to teal on hover/scroll-in-view), `content` (slot).
- **Variants:** Glass = True / False.
- **Tokens:** glass overlay `color.background-overlay` + `backdrop-filter: blur(12px)`, borders `gold-*` → `teal-*`. Full-bleed; `layout.content-max-width` inner.

### NavigationBar  (`navigation-bar`)
- **Props:** `navLinks` (list), plus embedded IconButtons (theme, contrast, reduced-motion toggle), monogram (→ `#hero`).
- **Variants:** Breakpoint = Desktop (left sidebar `layout.sidebar-width`, collapsible) / Mobile (top bar, in-flow dropdown, hides on scroll-down past 100px).
- **Tokens:** bg `color.background-overlay` + blur(12px); links `.link-underline`; motion toggle = Circle/CircleDashed pair with `aria-pressed`.

---

## Organisms → Templates (page sections — build as example frames, not library components)

These compose the atoms/molecules above into the five page sections. In a UI kit they belong on a **Templates** page as reference layouts, not as reusable components.

- **Hero** (`hero`) — `.type-display` name with brand-underscore accent on `PHILPOTT_`; ActionCall (resume); pixel-noise SVG overlay @ 5%.
- **About** (`about`) — two-column (60/40): Lora body + skill TagLists (Design / Frontend / Backend / Tooling / Game Dev).
- **Experience** (`experience`) — vertical timeline, accent line + dots, ArticleCards alternating L/R (desktop) / stacked (mobile).
- **Projects** (`projects`) — wrapping flex row of ArticleCards (50% desktop / 100% mobile).
- **Contact** (`contact`) — centered; headline with accent word; three TextLinks (Email/LinkedIn/GitHub); ActionCall (resume); footer.

---

## PWA expansion set (new — to design *into* the kit for the next project)

Not in the portfolio today; add these so the kit serves a full PWA. All consume the same tokens.

- **App shell / scaffold** — app bar + content + bottom-nav (mobile) / sidebar (desktop).
- **State components** — loading skeleton, empty state, error state, **offline banner**.
- **Install-prompt banner** — PWA "Add to home screen".
- **Toast / Snackbar** — transient feedback (use `color.accent` / `color.error`).
- **Modal + Bottom-sheet** — overlay `color.background-overlay` + blur.
- **Form inputs** — text field, textarea, select, toggle, checkbox, radio (states: default/focus/error/disabled).
- **List + ListItem**, **Avatar**, **Badge**, **Tabs**, **Settings row** (the existing theme/contrast/motion toggles are the prototype for this).
