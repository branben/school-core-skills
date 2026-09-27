# Linking HTML Artifacts

## tot.page URL Structure (CRITICAL)

**The tot.page URL is just the hash — the filename is NOT part of the path.**

- Correct: `https://tot.page/XXXXX`
- Wrong: `https://tot.page/XXXXX/file.html` (returns 404)

When linking between separate tot.page pages, use bare absolute URLs
(`https://tot.page/YYYYY`), never append filenames.

## Workflow for Multi-file tot.page

1. Build all pages in the same directory
2. Publish each: `tot page-a.html`, `tot page-b.html`
3. Copy the returned absolute URLs
4. Paste absolute URLs into the main page's links
5. Re-publish the main page with updated links
6. **After ANY link change, invalidate the CDN cache** with `tot update`:
   ```bash
   tot update https://tot.page/XXXXX file-a.html
   tot update https://tot.page/YYYYY file-b.html
   ```
   Without `tot update`, the CDN serves stale content with old links.

## Table-Row Links

When linking from a table cell to a sub-page:

1. **Wrap only the title text** in `<a>`, not the entire `<tr>`
2. **Use absolute URLs** so links resolve on published hosts
3. **Add visual cue** (underline-on-hover) so readers know only text is clickable

```html
<td>
  <strong><a href="https://tot.page/XXXXX" class="plan-link">Title</a></strong>
  <br><span class="small">Subtitle</span>
</td>
```

CSS for the visual cue:

```css
.plan-link {
  color: var(--text);
  text-decoration: none;
  display: inline-block;
  border-bottom: 1px solid transparent;
  transition: border-color 0.2s, color 0.2s;
}
.plan-link:hover {
  color: var(--accent);
  border-bottom-color: var(--accent);
}
```

## Back Links

Each sub-page should link back to the main explainer with a "← Back" link
at the top. Use the main page's absolute URL (bare hash, no filename).