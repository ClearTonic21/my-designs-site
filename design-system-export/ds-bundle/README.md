# ClearTonic UI Kit — ds-bundle

A portable HTML/CSS component library generated from the Eli Philpott portfolio's design system. Doubles as (1) a living styleguide you can open in any browser, (2) the foundation layer for the next ClearTonic PWA, and (3) an upload-ready bundle for **claude.ai/design** (DesignSync).

## Structure

```
ds-bundle/
├── index.html              # Living styleguide — every component + live 4-theme switcher
├── tokens.css              # All 4 themes as CSS custom properties (dark / light / HC / light-HC)
├── kit.css                 # Every component as a portable class (imports tokens.css)
├── README.md
├── foundations/
│   ├── colors.html         # @dsCard — semantic palette
│   ├── typography.html     # @dsCard — type ramp
│   └── spacing.html        # @dsCard — spacing grid + radius
└── components/
    ├── action-call.html    # @dsCard — primary / ghost button
    ├── text-link.html      # @dsCard — underline-wipe link
    ├── icon-button.html    # @dsCard — icon-only controls
    ├── tag-list.html       # @dsCard — tags + tag groups
    ├── article-card.html   # @dsCard — metallic-border card (gold → teal on hover)
    └── navigation-bar.html # @dsCard — nav bar + glass section
```

## Reuse in the new PWA

Copy `tokens.css` + `kit.css` into the PWA. `tokens.css` is the foundation (themeable via
`data-theme="light"` and `data-high-contrast="true"` on `<html>`); `kit.css` gives every
component class. No translation step — these are the real, shipping styles.

## The `@dsCard` convention

Each preview file's **first line** is `<!-- @dsCard group="…" name="…" -->`. claude.ai/design's
Design System pane builds its card index from these markers automatically (compiled into
`_ds_manifest.json` by the app's self-check). Cards are self-contained so they render in
isolation regardless of how the preview pane bases relative URLs.

## Uploading to claude.ai/design (DesignSync)

Requires a Claude session authenticated to a claude.ai account **with design-system access**.
A session started from a fixed `CLAUDE_CODE_OAUTH_TOKEN` (e.g. desktop local-agent mode) cannot
get design scopes — use the standalone `claude` terminal CLI and `/login` first. Then:

1. `list_projects` → confirm a writable design-system project, or `create_project` (e.g. "ClearTonic UI Kit").
2. `finalize_plan` with `localDir` = this `ds-bundle/` folder and `writes` = `**/*.html`, `*.css`, `README.md`.
3. `write_files` (paths from the plan) → the bundle uploads; cards appear in the Design System pane grouped by `@dsCard group`.
