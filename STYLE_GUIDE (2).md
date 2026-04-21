# Follett ED Design System — Style Guide

> **Developer reference only.** This file is not linked in the app UI.
> The live interactive version is still accessible at `/style-guide` for local development.
> Copy this file into any new Claude Code project as `STYLE_GUIDE.md` to maintain a consistent design language.

---

## Brand Identity

| Token | Value | Use |
|-------|-------|-----|
| Primary font | `Inter` | All UI text |
| Logo mark | Follett Flame (orange) | App icons, avatars, accent moments |
| Brand voice | Trustworthy, clear, education-first | Copy and microcopy |

---

## Color Tokens

All tokens are CSS custom properties defined in `src/index.css` via `:root {}`.

### Brand Blues (Primary)

| Token | Hex | Use |
|-------|-----|-----|
| `--follett-blue-dark` | `#0F2440` | Dark sidebar background |
| `--follett-blue` | `#1B3A5C` | Page titles, primary text emphasis |
| `--follett-blue-medium` | `#1A6EB5` | Links, active nav, primary actions |
| `--follett-blue-light` | `#2A5A8C` | Hover states |
| `--follett-blue-lighter` | `#E8F0F8` | Subtle backgrounds, highlight rows |

### Orange — Follett Flame (Accent)

| Token | Hex | Use |
|-------|-----|-----|
| `--follett-orange-dark` | `#C45E0E` | Hover on orange buttons |
| `--follett-orange` | `#F47B20` | CTAs, avatars, accent icons |
| `--follett-orange-light` | `#FEF0E6` | Orange badge backgrounds |

### Teal (Secondary Accent)

| Token | Hex | Use |
|-------|-----|-----|
| `--follett-teal` | `#00838F` | Secondary badges, user avatars |
| `--follett-teal-light` | `#4DB6AC` | Teal button hover |
| `--follett-teal-lighter` | `#E0F7FA` | Teal badge backgrounds |

### Status / Semantic Colors

| Token | Hex | Paired BG Token |
|-------|-----|-----------------|
| `--status-success` | `#2E7D32` | `--status-success-bg` `#E8F5E9` |
| `--status-warning` | `#F57F17` | `--status-warning-bg` `#FFF8E1` |
| `--status-error` | `#C62828` | `--status-error-bg` `#FFEBEE` |
| `--status-info` | `#1565C0` | `--status-info-bg` `#E3F2FD` |

### Risk Levels

| Token | Hex | Paired BG Token |
|-------|-----|-----------------|
| `--risk-low` | `#2E7D32` | `--risk-low-bg` `#E8F5E9` |
| `--risk-medium` | `#F57F17` | `--risk-medium-bg` `#FFF8E1` |
| `--risk-high` | `#E65100` | `--risk-high-bg` `#FFF3E0` |
| `--risk-critical` | `#B71C1C` | `--risk-critical-bg` `#FFEBEE` |

### Neutrals (Gray Scale)

| Token | Hex |
|-------|-----|
| `--gray-50` | `#F7F8FA` — App background |
| `--gray-100` | `#F0F2F5` — Hover backgrounds |
| `--gray-200` | `#EEEEEE` — Borders, dividers |
| `--gray-300` | `#E0E0E0` — Stronger borders |
| `--gray-400` | `#BDBDBD` — Disabled / placeholder |
| `--gray-500` | `#9E9E9E` — Secondary text |
| `--gray-600` | `#757575` — Meta / caption text |
| `--gray-700` | `#616161` |
| `--gray-800` | `#424242` — Body text |
| `--gray-900` | `#212121` — Primary text |

### Layout Chrome

| Token | Hex | Use |
|-------|-----|-----|
| `--topbar-bg` | `#C8D9EC` | Top header bar |
| `--sidebar-light-bg` | `#FFFFFF` | Light sidebar |
| `--sidebar-light-active-bg` | `#E4ECF6` | Active nav item (light sidebar) |
| `--sidebar-light-active-color` | `#1A6EB5` | Active nav text (light sidebar) |

---

## Typography

**Font stack:** `'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`
**Token:** `--font-family`

| Role | Size | Weight | Notes |
|------|------|--------|-------|
| Display | 36px | 700 | Hero stat headers only |
| H1 | 28px | 700 | Page headings |
| H2 | 22px | 700 | Section headings |
| H3 | 18px | 600 | Card titles |
| H4 | 16px | 600 | Sub-section labels |
| Body | 14px | 400 | Default body copy |
| Small | 13px | 400 | Secondary text, timestamps |
| Label | 12px | 600 | Uppercase table headers, nav sections (`letter-spacing: 0.5px`) |
| Caption | 11px | 500 | Footnotes, chart labels |

**Stat numbers:** 40px / 32px / 28px at weight 700 in `--follett-blue`

---

## Spacing Scale

| px | Common Use |
|----|-----------|
| 4px | Icon gaps, tight padding |
| 8px | Button padding (compact), small gaps |
| 12px | Default list gaps, inner card padding |
| 16px | Standard padding, form element gaps |
| 20px | Section subheadings margin |
| 24px | Card padding, header padding |
| 32px | Section spacing |
| 48px | Major section breaks |

---

## Border Radius

| Token | Value | Use |
|-------|-------|-----|
| `--radius-sm` | `4px` | Tags, small inputs |
| `--radius-md` | `8px` | Buttons, nav links, cards |
| `--radius-lg` | `12px` | Modals, menus |
| `--radius-xl` | `16px` | Large cards, feature panels |

---

## Shadows

| Token | Value |
|-------|-------|
| `--shadow-sm` | `0 1px 2px rgba(0,0,0,0.06)` |
| `--shadow-md` | `0 4px 6px rgba(0,0,0,0.07)` |
| `--shadow-lg` | `0 10px 15px rgba(0,0,0,0.1)` |
| `--shadow-xl` | `0 20px 25px rgba(0,0,0,0.12)` |

---

## Layout Dimensions

| Token | Value |
|-------|-------|
| `--sidebar-width` | `232px` |
| `--header-height` | `64px` |

---

## Component Classes

### Buttons

```html
<!-- Variants -->
<button class="btn btn-primary">Primary</button>
<button class="btn btn-secondary">Secondary</button>
<button class="btn btn-teal">Teal</button>
<button class="btn btn-orange">Orange</button>
<button class="btn btn-danger">Danger</button>
<button class="btn btn-ghost">Ghost</button>
<button class="btn btn-action">Actions ▾</button>

<!-- Sizes -->
<button class="btn btn-primary btn-sm">Small</button>
<button class="btn btn-primary">Default</button>
<button class="btn btn-primary btn-lg">Large</button>
```

### Badges

```html
<!-- Risk -->
<span class="badge badge-low">Low Risk</span>
<span class="badge badge-medium">Medium Risk</span>
<span class="badge badge-high">High Risk</span>
<span class="badge badge-critical">Critical</span>

<!-- Workflow Status -->
<span class="badge badge-approved">Approved</span>
<span class="badge badge-denied">Denied</span>
<span class="badge badge-draft">Draft</span>
<span class="badge badge-review">In Review</span>
<span class="badge badge-archived">Archived</span>
<span class="badge badge-submitted">Submitted</span>
<span class="badge badge-follett">Follett Verified</span>

<!-- Urgency -->
<span class="badge badge-urgency-low">Low</span>
<span class="badge badge-urgency-medium">Medium</span>
<span class="badge badge-urgency-high">High</span>
```

### Alerts

```html
<div class="alert alert-info">…</div>
<div class="alert alert-success">…</div>
<div class="alert alert-warning">…</div>
<div class="alert alert-error">…</div>
```

### Avatars

```html
<!-- Square avatars (initials) -->
<div class="avatar-square orange lg">JS</div>   <!-- 40px -->
<div class="avatar-square blue">MA</div>         <!-- 32px default -->
<div class="avatar-square teal sm">PD</div>      <!-- 24px -->
<!-- Color modifiers: orange | blue | teal -->
<!-- Size modifiers: sm (24px) | default (32px) | lg (40px) -->
```

### Sidebar Variants

```html
<!-- Dark (default) -->
<aside class="sidebar">…</aside>

<!-- Light (current production variant) -->
<aside class="sidebar sidebar-light">…</aside>
```

### Top Header Variants

```html
<!-- Branded blue-gray (current) -->
<header class="top-header">…</header>

<!-- Alternative branded variant -->
<header class="top-header top-header-branded">…</header>
```

### Cards & Layout

```html
<div class="card">
  <div class="card-body">…</div>
</div>

<div class="stats-grid">
  <!-- Static stat card -->
  <div class="stat-card">
    <div class="stat-icon blue">…</div>  <!-- blue | orange | red | green -->
    <div class="stat-content">
      <h4>142</h4>
      <p>AI Tools Registered</p>
    </div>
  </div>

  <!-- Clickable/navigable stat card — adds hover style + arrow affordance -->
  <div class="stat-card stat-card-link" onClick={…}>
    <div class="stat-icon red">…</div>
    <div class="stat-content">
      <h4>8</h4>
      <p>Vendor Risk Alerts</p>
    </div>
    <ChevronRight size={16} className="stat-link-arrow" />
  </div>
</div>
```

> **`.stat-card-link` behavior:** on hover, the card background shifts to `--follett-blue-lighter`, border turns `--follett-blue-medium`, subtitle text picks up the blue, and the arrow nudges 3px right. Use this class any time a stat card navigates somewhere. Always pair with a `ChevronRight` icon carrying the `.stat-link-arrow` class.

### Tables

```html
<div class="table-container">
  <table>
    <thead><tr><th>Column</th></tr></thead>
    <tbody>
      <tr class="clickable"><td>…</td></tr>
    </tbody>
  </table>
</div>
```

### Forms

```html
<div class="form-row">
  <div class="form-group">
    <label class="form-label">Field Name</label>
    <input class="form-input" type="text" placeholder="…" />
  </div>
</div>
<div class="form-group">
  <select class="form-select">…</select>
</div>
<div class="form-group">
  <textarea class="form-textarea" />
</div>
<label class="form-checkbox">
  <input type="checkbox" /> Label text
</label>
```

### Tabs

```html
<div class="tabs">
  <button class="tab active">Tab One</button>
  <button class="tab">Tab Two</button>
</div>
```

---

## Design Principles

**Clarity over cleverness.** Every screen should communicate status and action at a glance — especially for district admins who spend seconds, not minutes, reviewing dashboards.

**Consistent density.** Use `14px` body text and `24px` card padding as defaults. Only go tighter in data-dense tables; only go larger in hero/stat moments.

**Color carries meaning.** Orange = attention/action, Blue = structure/trust, Teal = secondary highlights, Red = risk/error. Don't use these colors decoratively in ways that conflict with their semantic roles.

**Accessible contrast.** All primary text on light backgrounds meets WCAG AA. Status colors are always paired with text labels — never rely on color alone.

---

## Live Reference

The interactive style guide component lives at `src/pages/StyleGuide.jsx` and is accessible at `/style-guide` in local development. It renders all tokens from `index.css` in a tabbed browser view and is useful for visual QA when updating the design system.

> **Note:** The route is registered in `App.jsx` but intentionally hidden from the sidebar navigation. Reach it by typing the URL directly in the browser.
