# Advanced Grading Console — CodeForge V1.0 submission

A client-side grading console. An instructor uploads marks, picks a course, reviews analytics, tunes grade boundaries in a simulator and exports final grades. Everything runs in the browser, so no student data leaves the machine.

## Files
| File | Purpose |
|---|---|
| `index.html` | The whole application (HTML + CSS + JS, no build step) |
| `xlsx.full.min.js` | SheetJS 0.18.5, loaded locally first. If it is missing, the page falls back to cdnjs and then jsDelivr |
| `BUG_FIX_LOG.md` | Bug fix log in the challenge format (26 entries) |
| `samples/` | Test files: valid 3-course file (.xlsx and .xls), second file, file with issues, missing columns, header only, edge cases, empty file, fake .xlsx |

## Deploy (about 2 minutes)
- **Netlify:** go to app.netlify.com/drop and drag this folder in. You get a public URL straight away.
- **GitHub Pages:** push the folder to a repo → Settings → Pages → Deploy from branch `main` / root.
- **Vercel:** `vercel` in this folder (static, no config needed).

## Enhancements (each solves a real instructor problem)
1. **Upload validation with row-level issues.** Detects missing or duplicate columns, empty IDs, duplicate IDs within a course, missing, non-numeric, out-of-range or fractional marks, stray cells and near-duplicate course names. You get a summary (✓ 247 students · ✓ 3 courses · ⚠ issues), a "View issues" table with Excel row numbers, and a downloadable issue list. Marks are never modified. Courses with data errors cannot be exported.
2. **Grade Scheme Simulator.** Every boundary edit is a simulation. It shows current vs simulated distribution, how many students change grade, and clickable grade transitions (e.g. "B → B- · 7") that filter the student table. Nothing becomes official until you press **Apply changes**, and **Cancel simulation** restores the scheme.
3. **Interactive boundary editor.** Draggable, keyboard-accessible boundary handles on a 0–100 scale, with a marks-density strip so you can see students clustered near a cut-off. Auto-link keeps neighbours continuous. Inline validation names the exact overlap or gap ("Gap between A- and A: marks 76–79 are not covered").
4. **Analytics.** Students, average, median, σ, configurable pass rate, min and max, with animated values. The histogram uses correct bins, grade-band shading, a fitted normal curve (safe when σ = 0), a mean line, tooltips and an accessible data table.
5. **Student table.** Search by BITS ID, filter by grade, a "near a boundary" filter (within 2 marks of the next grade), sortable columns, percentile rank, pagination, and a stacked-card layout on mobile.
6. **Final review and safe export.** Summary, distribution, scheme, and "N students will receive different grades from the default scheme / your last export". A checklist and an explicit confirmation gate the export. CSV is escaped and named `grades_<Course>_<date>.csv`. The Excel report has four sheets: Grades, Statistics, Grade Distribution and Validation Report.
7. **Per-course schemes.** A multi-course file keeps a separate scheme, pass grade and finalized status for each course.
8. **UX foundations.** Light/dark theme (animated switch, saved between visits), toasts (errors stay until dismissed), loading steps, empty states, accessible dialogs, visible focus, `prefers-reduced-motion` support, a downloadable template, and a warning before leaving with unexported work.

All original features are kept: instructor name requirement, Excel upload, course selection, analytics, grade range configuration and validation, reset, grade distribution, CSV export, grading timer, and the finalization message.

## Testing
An automated Playwright run covers all 32 scenarios in the brief (63 assertions, 0 failures, no console errors). It includes stats verified against independent calculations, CSV and Excel contents, a mobile width with no horizontal scroll, theme persistence after reload, and reduced motion.
