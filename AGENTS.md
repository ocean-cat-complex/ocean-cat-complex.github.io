# AGENTS.md - AI Coding Agent Instructions

> YongXiang Chen Research Group Website (chenyx.group)
> Hugo static site - No external theme dependencies

## Build Commands

```bash
# Build site (production)
hugo --minify --gc

# Build with drafts
hugo -D --gc

# Local development server
hugo server -D
# Opens at http://localhost:1313

# Clean build (remove generated files first)
rm -rf public resources && hugo --minify --gc

# Check Hugo version
hugo version
# Requires Hugo Extended (for SCSS support if added later)
```

## Deployment

Automatic via GitHub Actions on push to `main` branch.
- Workflow: `.github/workflows/gh-pages.yml`
- Deploys to GitHub Pages with CNAME `chenyx.group`

## Project Structure

```
.
├── config/_default/
│   ├── config.yaml      # Hugo configuration, menus, build settings
│   └── params.yaml      # Site parameters (contact, SEO, features)
├── content/
│   ├── _index.md        # Homepage content
│   ├── research/        # Research section
│   ├── members/         # Team members section
│   ├── publications/    # Publications section
│   └── authors/         # Individual member profiles (Hugo page bundles)
├── layouts/
│   ├── _default/        # Base templates (baseof.html, single.html, list.html)
│   ├── partials/        # Reusable components (header.html, footer.html)
│   ├── index.html       # Homepage template
│   ├── research/        # Research section templates
│   ├── members/         # Members section templates
│   └── publications/    # Publications section templates
├── static/
│   ├── css/style.css    # All custom CSS
│   ├── js/main.js       # Dark mode, mobile menu, animations
│   └── media/           # Images
└── go.mod               # Go module (no theme dependencies)
```

## Code Style Guidelines

### EditorConfig (`.editorconfig`)

- **Charset**: UTF-8
- **Line endings**: LF (Unix)
- **Indent**: 2 spaces (no tabs)
- **Final newline**: Yes
- **Trailing whitespace**: Trim (except `.md` files)

### HTML Templates (Hugo)

```html
<!-- Use semantic HTML5 elements -->
<section class="section">
  <div class="container">
    <h2 class="section-title">Title</h2>
    <!-- Content -->
  </div>
</section>

<!-- Hugo templating conventions -->
{{ define "main" }}...{{ end }}           <!-- Block definitions -->
{{ partial "header.html" . }}             <!-- Partials with context -->
{{ range .Site.Menus.main }}...{{ end }}  <!-- Iterations -->
{{ with .Params.description }}...{{ end }} <!-- Conditional with context -->

<!-- Inline styles allowed for layout, CSS classes for theming -->
<div style="display: grid; gap: 2rem;">
  <div class="card">...</div>
</div>
```

### CSS Conventions (`static/css/style.css`)

**Theming via CSS Variables**:
```css
:root {
  --color-primary: #1e40af;
  --color-bg: #ffffff;
  --color-text: #1e293b;
  /* ... */
}

.dark {
  --color-primary: #60a5fa;
  --color-bg: #0f172a;
  /* ... */
}
```

**Class Naming**:
- Semantic: `.hero`, `.card`, `.navbar`, `.footer`
- Component-specific: `.member-card`, `.pub-card`, `.research-card`
- Utility-like: `.section-title`, `.text-muted`, `.btn-primary`
- Grid layouts: `.grid-2`, `.grid-3`, `.grid-4`

**Responsive Breakpoints**:
```css
@media (min-width: 640px) { /* sm */ }
@media (min-width: 768px) { /* md */ }
@media (min-width: 1024px) { /* lg */ }
```

**Always use theme variables**:
```css
/* Correct */
color: var(--color-text);
background: var(--color-bg-alt);

/* Incorrect - breaks dark mode */
color: #333;
background: white;
```

### JavaScript (`static/js/main.js`)

```javascript
// Use DOMContentLoaded for initialization
document.addEventListener('DOMContentLoaded', function() {
  // ...
});

// Prefer const/let, never var
const toggle = document.getElementById('dark-toggle');

// Null checks before DOM operations
if (toggle) {
  toggle.addEventListener('click', function() { /* ... */ });
}

// Use arrow functions for callbacks
document.querySelectorAll('.card').forEach(el => {
  observer.observe(el);
});
```

### Content Files (Markdown)

```yaml
---
title: "Page Title"
description: "SEO description"
# Optional front matter fields
---

Content in Markdown...
```

**Author profiles** (`content/authors/*/`):
- Each author is a Hugo page bundle
- Contains `_index.md` with bio and `avatar.jpg`

## Key Technical Decisions

1. **No Tailwind build process** - Vanilla CSS with Tailwind-inspired utilities
2. **No external theme dependencies** - Pure Hugo templates
3. **Publications hardcoded in template** - Could migrate to data files
4. **Dark mode via CSS class** - `.dark` on `<html>` element
5. **Images in static/media/** - Direct file serving

## Common Tasks

### Add a new team member

1. Create `content/authors/Name Surname/`
2. Add `_index.md` with front matter (title, role, bio)
3. Add `avatar.jpg` (square, ~300x300px recommended)
4. Update `layouts/members/list.html` if needed

### Add a new publication

1. Edit `layouts/publications/list.html`
2. Add new `.card.pub-card` block with year, title, journal

### Modify navigation

Edit `config/_default/config.yaml` under `menu.main`

### Add new page section

1. Create `content/sectionname/_index.md`
2. Create `layouts/sectionname/list.html`
3. Add menu item in config

## Validation Checklist

Before committing changes:

- [ ] `hugo --minify` builds without errors
- [ ] Dark mode toggle works on all pages
- [ ] Mobile responsive layout functions
- [ ] All images load correctly
- [ ] Links work (internal and external)

## Files NOT to Modify

- `content.zip` - Archive backup
- `preview.png` - Template preview image
- `netlify.toml` - Legacy Netlify config (unused)
- `theme.toml` - Legacy theme config (unused)
- `README.md` - Original Wowchemy readme (outdated)
