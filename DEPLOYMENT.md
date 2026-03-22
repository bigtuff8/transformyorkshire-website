# Transform Yorkshire — Deployment & Quality Guidelines
**Version:** 1.0
**Last updated:** 22 March 2026
**Applies to:** transformyorkshire.uk (GitHub Pages)

---

## Branch Strategy

| Branch | Purpose | Deploys to |
|--------|---------|------------|
| `dev` | All active development work | Local preview only |
| `master` | Production — the live website | transformyorkshire.uk |

### Rules
1. **ALL changes are made on `dev`** — never commit directly to `master`
2. Changes are **reviewed and approved by James** before merging
3. Merges to `master` happen via **Pull Request only**
4. Every PR must pass the **Pre-Deployment Checklist** below before merge
5. Claude runs automated checks and provides results in the PR description

### Workflow
```
1. Claude makes changes on `dev` branch
2. Claude runs pre-deployment checklist (automated)
3. Claude opens the dev version locally for James to review
4. James approves or requests changes
5. Claude creates a PR from dev → master with checklist results
6. James confirms → Claude merges → site is live
```

---

## Pre-Deployment Checklist

**This checklist MUST be completed and documented in every PR before merging to master.**

### 1. Accessibility (WCAG 2.1 AA)
- [ ] All images have meaningful `alt` text
- [ ] Colour contrast ratios meet AA standard (4.5:1 for text, 3:1 for large text)
- [ ] All interactive elements are keyboard accessible (Tab, Enter, Escape)
- [ ] Focus indicators are visible on all focusable elements
- [ ] Page has a logical heading hierarchy (h1 → h2 → h3, no skips)
- [ ] Form fields have associated `<label>` elements
- [ ] ARIA labels present where needed (nav, landmarks, buttons)
- [ ] No content relies solely on colour to convey information
- [ ] Skip-to-content link present (or page is single-page with anchor nav)
- [ ] Text is resizable to 200% without loss of content

### 2. Mobile & Responsive
- [ ] Page renders correctly at 320px width (minimum mobile)
- [ ] Page renders correctly at 768px (tablet)
- [ ] Page renders correctly at 1024px (small laptop)
- [ ] Page renders correctly at 1920px (desktop)
- [ ] No horizontal scrolling on any breakpoint (except intentional carousels)
- [ ] Touch targets are minimum 44x44px
- [ ] Text is readable without zooming on mobile
- [ ] Navigation is usable on mobile (hamburger menu or equivalent)
- [ ] Forms are usable on mobile (input types, keyboard appropriate)

### 3. Data Protection & Regulatory (UK GDPR / ICO)
- [ ] Cookie consent banner is present and functional
- [ ] Analytics only fire AFTER consent is granted
- [ ] Cookie consent choice is stored and respected on return visits
- [ ] Decline option genuinely prevents tracking (not just cosmetic)
- [ ] Privacy information is accessible (inline or linked)
- [ ] Contact form does not collect unnecessary data
- [ ] Form data is transmitted securely (HTTPS)
- [ ] No personal data stored in client-side code, localStorage, or exposed in URLs
- [ ] Company registration number displayed in footer
- [ ] Email address for data requests available

### 4. Performance
- [ ] Page loads in under 3 seconds on 3G connection
- [ ] Images are optimised (compressed, appropriate format, lazy-loaded where applicable)
- [ ] No render-blocking resources that delay first paint
- [ ] Total page weight under 5MB (ideally under 2MB)
- [ ] Fonts are preloaded and use `display: swap`

### 5. Security
- [ ] HTTPS enforced (no mixed content)
- [ ] No sensitive data in HTML source, comments, or JavaScript
- [ ] External scripts are from trusted sources only (Google Analytics, Formspree)
- [ ] Form submission uses POST, not GET
- [ ] No inline event handlers that could be exploited (CSP consideration)
- [ ] No API keys or secrets exposed in client-side code
- [ ] Formspree endpoint is the only external data destination

### 6. SEO & Technical
- [ ] Page has a unique, descriptive `<title>`
- [ ] Meta description is present and under 160 characters
- [ ] Open Graph tags present (og:title, og:description, og:type, og:url)
- [ ] Canonical URL is correct
- [ ] All internal links work (no broken anchors)
- [ ] All external links open in new tab with `rel="noopener noreferrer"`
- [ ] Favicon is present
- [ ] Semantic HTML used (header, nav, main, section, footer)
- [ ] Robots can crawl the page (no accidental noindex)

### 7. Cross-Browser
- [ ] Chrome (latest) — tested
- [ ] Firefox (latest) — tested
- [ ] Safari (latest) — tested (or noted if unavailable)
- [ ] Edge (latest) — tested
- [ ] Mobile Safari (iOS) — tested (or noted)
- [ ] Mobile Chrome (Android) — tested (or noted)

### 8. Content
- [ ] No spelling or grammatical errors
- [ ] All placeholder/test content removed
- [ ] Company details are accurate (name, number, email)
- [ ] Copyright year is current
- [ ] All claims are accurate and defensible

---

## Automated Testing

Claude will run the following automated checks before presenting changes for review:

### HTML Validation
```bash
# Validate HTML structure
npx html-validate index.html
```

### Accessibility Audit
```bash
# Run axe-core accessibility checks via Lighthouse
npx lighthouse <url> --only-categories=accessibility --output=json
```

### Performance Audit
```bash
# Lighthouse performance score
npx lighthouse <url> --only-categories=performance --output=json
```

### Link Checking
```bash
# Check all links resolve
npx linkinator <url> --recurse
```

### Mobile Responsiveness
```bash
# Screenshot at multiple viewports (Playwright)
npx playwright screenshot --viewport-size=320,568 <url>   # iPhone SE
npx playwright screenshot --viewport-size=768,1024 <url>   # iPad
npx playwright screenshot --viewport-size=1920,1080 <url>  # Desktop
```

> **Note:** Where automated tools are unavailable, Claude will manually inspect the HTML/CSS against each checklist item and document findings.

---

## Rollback Procedure

If a deployment causes issues:
1. Identify the last known good commit: `git log --oneline master`
2. Revert: `git revert <bad-commit>` (creates a new commit, preserves history)
3. Push to master: `git push origin master`
4. Verify live site is restored

**Never use `git reset --hard` or `git push --force` on master.**

---

## Image Guidelines

- All images stored locally in `/images/` — no external CDN dependencies in production
- Maximum individual image size: 500KB (compress before committing)
- Preferred format: JPEG for photos, SVG for graphics/icons, WebP where supported
- All images must have `loading="lazy"` unless above the fold
- Wikimedia Commons images require CC attribution — maintain a credits file if used

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 22 March 2026 | Initial deployment guidelines |
