---
name: security-report
description: Build a client-ready security plan or report as a branded 4-page HTML + PDF, with a one-page CISO risk view (story, RAG grid, board lines) and engineer pages (diagram, timeline, checklists). Use when asked for a report, plan, briefing or one-pager for a CISO, board or engineering team.
---

# Security Report

Template: `reports/machine-speed-containment-90-day-plan.html`. Copy it and keep its CSS and components; change only the content.

## Page 1: CISO (one page, no exceptions)

In this order:
1. **Title**: what we'll achieve and by when.
2. **The story**: how the landscape changed and the gap it creates. A visual (attacker vs. our response) plus three short "what changed" points with one sourced number each.
3. **Decision bar**: the one decision needed, with a date.
4. **Risk grid (RAG)**: 5–7 risks taken from the threat this report addresses, shown today and at each milestone. Each row gets a few words on what moves it.
   - Drop rows that stay red the whole time, and drop adjacent topics.
   - Keep it honest: a CISO has to defend this grid in front of a board.
5. **For the board**: 3 lines, one of them the risk-reduction %. Only use a % you can defend, and say how it will be measured.
6. **Requirements**: outcomes the plan guarantees (e.g. "every automated action is reversible"), not mechanisms.
7. **Asks** with dates, and **targets** with numbers.

## Engineer pages

- **Diagram**: the simplest flow that explains the system (inline SVG, ≤ 6 boxes). Check that labels never overlap lines.
- **Timeline chart**: rows in plain language that make sense without the repo. Show build, testing and live phases.
- **Checklists by phase**: one line per task with a checkbox, and an exit condition per phase.
- **Reference**: detection-to-response map using chips and mode labels, plus thresholds as short key: value lines.

## Never include

- Prose paragraphs, marketing lines or slogans
- Subtext and footnotes that don't change a decision
- Internal IDs (F-01, A-03), repo file paths or effort points
- Dense text-in-cell tables. Use chips, color, short labels and cards instead
- Client names or identifying details in the generic repo

## Render and check

```powershell
& "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --headless=new --disable-gpu --no-pdf-header-footer --virtual-time-budget=8000 "--print-to-pdf=<out.pdf>" "file:///<path to html>"
```

- Pages are fixed letter size and clip anything that overflows in print. Measure each `.page` at 1000px viewport width: the last child's bottom must be under ~1016px.
- Take a headless screenshot at `--window-size=880,<height>`, crop each page, and look at every page before sending.
