# Publishing & Linking HTML Artifacts

## The `tot` CLI

Publish any HTML file as a shareable, frozen page:

```bash
tot path/to/artifact.html
# → https://tot.page/<id>
```

The URL is frozen to a specific commit hash — edit the file and re-publish for a
new URL. The old URL keeps working.

## tot.page URL Structure

**The URL is just the hash — the filename is NOT part of the path:**

- Correct: `https://tot.page/XXXXX`
- Wrong: `https://tot.page/XXXXX/file.html` (returns 404)

When linking between separate tot.page pages, use bare absolute URLs
(`https://tot.page/YYYYY`), never append filenames.

## CDN Cache Invalidation (CRITICAL)

After changing any link between tot.page pages, you MUST invalidate the CDN cache
with `tot update`:

```bash
tot update https://tot.page/XXXXX file.html
```

Without `tot update`, the CDN serves stale content — the user sees old links.
This is the #1 cause of "links don't work" complaints with multi-file tot.page.

## Rules for publishable artifacts

1. **Self-contained.** No external CSS, fonts, JS, or images. Everything must be
   inline or embedded. `tot` uploads a single file and does not follow `<link>`,
   `<script src>`, or `url()` references.
2. **No lorem ipsum.** Real content from draft one. Placeholders signal
   unfinished work.
3. **Theme support.** Always include both dark and light via `data-theme`
   attribute on `<html>`. Default to dark for technical subjects.
4. **Mobile breakpoint.** 768px for multi-column → single column. 44px minimum
   touch targets.

## Linking artifacts together

When building a main explainer + sub-pages (e.g. per-fix-option plans):

- Main page links to sub-pages via absolute tot.page URLs
- Sub-pages link back: `<a href="https://tot.page/XXXXX">← Back</a>`
- All share the same CSS design tokens (copy the `<style>` block)
- Each page publishes independently with `tot`

## Workflow

```bash
# 1. Build
tot main-explainer.html
tot plan-a.html
tot plan-b.html

# 2. Get URLs, update links in main page
# 3. Re-publish main page
tot update https://tot.page/XXXXX main-explainer.html

# 4. After ANY link change, update all affected pages:
tot update https://tot.page/YYYYY plan-a.html
```

## After publishing

Ask the user if they want to share the URL publicly. `tot` pages are
link-accessible to anyone who has the URL. Never publish credential-bearing
pages.