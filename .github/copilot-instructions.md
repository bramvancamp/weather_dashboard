# GitHub Copilot Instructions

## Project Overview

This is a single-file vanilla JavaScript weather dashboard (`webpage.html`) that displays
current and historical weather data for Berlin, Amsterdam, and Copenhagen using the
[Open-Meteo API](https://open-meteo.com/) and [Chart.js](https://www.chartjs.org/).

There is no build step, no package manager, and no framework. The entire application —
HTML, CSS, and JavaScript — lives in `webpage.html`.

---

## Code Style

### JavaScript

- Use `const` and `let`; never `var`.
- Use arrow functions, template literals, destructuring, and `async`/`await` throughout.
- Prefer `Promise.all()` for concurrent async operations (e.g., fetching multiple cities).
- Keep state in module-scoped variables (`weatherData`, `currentTab`, `currentSelector`).
- Name helper/utility functions clearly and place them near related logic.
- Avoid third-party libraries — use native browser APIs (`fetch`, DOM APIs, etc.).
- Use `// ───` separator comment lines to delimit logical sections (Config, State,
  Date helpers, Fetch, Render, etc.).

### CSS

- Use the existing colour palette object `C` for all colour values; do not introduce
  raw hex codes outside of that object.
- Use CSS `clamp()` for responsive typography.
- Layout with CSS Grid and Flexbox; avoid absolute positioning unless unavoidable.
- Class names should be kebab-case and semantically named (`.kpi-card`, `.chart-wrap`).

### HTML

- Keep markup minimal and semantic.
- Inline `onclick` handlers are acceptable for event wiring (consistent with existing style).
- Avoid adding external dependencies; load libraries via CDN only if truly necessary and
  pin them to a specific version (e.g., `chart.js@4.4.4`).

---

## Architecture Constraints

- **Single-file rule**: all changes must remain within `webpage.html`. Do not split the
  app into multiple files unless explicitly requested.
- **No build tooling**: do not introduce `npm`, `webpack`, `vite`, or any other build
  system.
- **API**: all weather data comes from Open-Meteo. Do not add other data sources without
  discussion. Always handle API errors with `try/catch` and surface them gracefully in
  the UI.
- **Cities**: Berlin, Amsterdam, Copenhagen are the three supported cities. Adding or
  removing cities requires updating the `CITIES` config array and all dependent rendering
  logic consistently.

---

## Code Review Checklist

When reviewing a pull request, check for:

1. **Correctness**
   - Date range calculations use the existing date-helper utilities.
   - Chart datasets are destroyed/re-created properly to avoid Chart.js memory leaks.
   - API URLs are constructed with correct parameter names as per Open-Meteo docs.

2. **Consistency**
   - New UI elements follow existing KPI card / tab / pill patterns.
   - Colours reference the `C` palette object.
   - Section separators (`// ───`) are used to demarcate new sections.

3. **Performance**
   - API calls are parallelised with `Promise.all()` where possible.
   - DOM updates are batched (set `.innerHTML` once, not in a loop).
   - No unnecessary re-renders on state changes.

4. **Accessibility & UX**
   - Interactive elements are keyboard-accessible.
   - Loading and error states are always visible to the user.
   - Charts have meaningful axis labels and tooltips.

5. **Security**
   - No user input is inserted into the DOM via `innerHTML` without sanitisation.
   - No API keys or secrets are hardcoded.

6. **Simplicity**
   - Avoid over-engineering. Prefer the simplest solution that satisfies the requirement.
   - Do not add abstractions or utilities that are only used once.
