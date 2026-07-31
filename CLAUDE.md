# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A single-page HTML/CSS résumé (CV) for Phan Trần Hoài My, styled as a printable A4 document. There is no build system, package manager, or JavaScript — just static files rendered directly by a browser. Content is in Vietnamese.

Published at https://phanmy03.github.io/resume/ (GitHub Pages, served from this repo).

## Files

- [index.html](index.html) — all résumé content: photo, contact info, education, skills, career goals, languages in the `.sidebar`; name, personal summary, and work experience in the `.content` column.
- [style.css](style.css) — all styling, including print-specific rules (`@page`, `@media print`).
- [avatar.jpg](avatar.jpg) — profile photo referenced by `index.html`.
- [codeswing.json](codeswing.json) — config for the CodeSwing VS Code extension (live HTML/CSS/JS playground); not part of the deployed site.

## Architecture notes

- Layout is a single `.cv` card (A4-sized, `210mm` wide) that's a flat two-column flex row spanning the full height — no shared top header. `.sidebar` (34%, green-gradient background with a diagonal-stripe texture) holds the photo and all "reference" info; `.content` (flex: 1, white background) holds the name and the two prose/experience sections. There is no `#mainLeft`/`#mainRight` split anymore.
- Each `<section>` carries a semantic class (`.contact`, `.edu`, `.skills`, `.goals`, `.languages` in `.sidebar`; `.about`, `.experience` in `.content`) that `style.css` targets directly — section order in the HTML is not load-bearing for styling.
- `body`'s base `font-size` (currently `11.6px`) is tuned so the single-page content lands just under the 297mm A4 height with a small safety margin — most spacing/type-scale in this file is `em`-based off that value, so changing it (or any section's font-size/margins) can push the résumé onto a second page. After edits that add or resize text, verify with a real height measurement (e.g. a headless-browser script comparing `.cv`'s rendered height to `297mm` in px), not just a visual glance — text reflow near a line-wrap boundary can jump several lines' worth of height for a `0.1px` font-size change.
- Section headings are rendered as a "pill" badge: `<h2><span>Label</span></h2>`. In `.sidebar`, the `h2` is centered (`display: table; margin: 0 auto`) with a solid cream pill. In `.content`, `section > h2` additionally draws a full-width divider line behind the pill via `::before`, so the badge appears to interrupt a horizontal rule — don't add icons or extra children inside these `h2`s without checking that positioning still works.
- Each work-experience entry in `.experience` is wrapped in a `.job` div (company `<p class="job-company">` with an inline `fa-solid fa-building` icon, `<h3>` title/dates, `<ul>`); the dashed divider between jobs comes from `.experience .job:not(:first-child)`, not from counting `<p>` siblings.
- Fonts: "Be Vietnam Pro" (body) and "Playfair Display" (name heading), loaded from Google Fonts via CDN `<link>` tags. Font Awesome 6 loaded from cdnjs, used only for the contact icons (`fa fa-phone`/`fa-envelope`/`fa-map`) and the job-entry building icon — section heading pills are icon-free.

## Working with this repo

- There is no build, lint, or test tooling — verify changes by opening [index.html](index.html) directly in a browser (or via a local static file server).
- To check print/PDF layout specifically, use the browser's print preview, since `style.css` has dedicated `@media print` and `@page` rules that differ from screen styles.
