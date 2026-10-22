# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"Times Table Star" is a multiplication-table practice site for children. It's meant to be fun and rewarding. The whole site is one static file, `index.html`, with inline CSS and JS. There's no build step, no package manager, and no tests.

**Run:** open `index.html` directly in a browser. There's nothing to build or install. The only external resource is the Google Fonts "Fredoka" stylesheet.

## Architecture (inside `index.html`)

- **State:** module-level variables `table` (`'mix'` or 1–10), `a`, `b` (current factors), `streak`, `stars`, and `locked`. `locked` blocks double-submits during the 900ms pause after a correct answer. Nothing is saved between visits.
- **Flow:**
  - `next()` generates a question and restarts the `.q.enter` animation by removing the class, forcing a reflow (`void el.offsetWidth`), then adding it back. It also resets the card, clears the input and focuses it.
  - The form `onsubmit` handler checks the answer:
    - Correct: green `.card.good` state, a random cheer from `praise`, and `next()` after 900ms.
    - Wrong: the `.card.bad` shake. The streak resets and the question stays.
- **Table picker:** the chips are generated in JS. The selected chip is marked with `aria-pressed`, which the CSS uses for styling.
- **Confetti:** `confetti()` uses the Web Animations API and fires only on every 5th streak, on purpose, so the reward stays special. It does nothing when the user prefers reduced motion.

## Design conventions (Emil Kowalski design-engineering style)

Keep new UI consistent with these:
- Use the custom easing `--ease-out: cubic-bezier(0.23, 1, 0.32, 1)`. Never use `ease-in`. Keep UI animations at 300ms or less.
- Animate only `transform`, `opacity` and `filter`. List the exact properties in `transition`, never `transition: all`.
- Pressable elements get press feedback on `:active` (a `scale(0.95–0.98)` or a translate).
- Put hover styles inside `@media (hover: hover) and (pointer: fine)`.
- Entrance animations start from a slight scale/translate plus opacity, never `scale(0)`.
- Every new animation needs a `prefers-reduced-motion` fallback: keep fades, drop movement.
- Colors are CSS variables on `:root`. Reuse them rather than hard-coding colors.

# Working Rules

## Planning
- Before planning, grill me with questions until the requirements are clear.
- Always plan first, then execute. Do not write code before the plan is approved.
- Use Opus 5.5 for planning and Sonnet 5.5 for execution.

## Git and GitHub
- The project lives in a GitHub repository: https://github.com/galitbental/times-table-star
- Every new feature is developed on its own new branch, never directly on main.

## Testing
After every implementation, run and report the actual results:
- E2E tests on the full user flow
- Regression tests, to confirm existing features still work
- Edge cases (empty, missing, very long or invalid input)

## Security
- After every stage of execution, run a security review: injection, prompt injection, input validation, and secrets or API keys in code.

## Documentation
- A hook updates Obsidian after every change. Keep the project notes there up to date.

## Design
- Create the UX/UI design with the Stitch MCP.
