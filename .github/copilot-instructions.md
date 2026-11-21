# Copilot Instructions

## Project shape

This repository is a dependency-free, single-page space quiz. The complete application is in `index.html`; there is no build system, package manager, server, or generated output.

- Open `index.html` directly in a browser. It uses a `file://` URL and does not require local hosting.
- External font loading is optional: the page declares Google Fonts in the `<head>` and falls back to system sans-serif fonts.
- The document is divided into embedded CSS and one inline JavaScript application. Keep changes in this file unless the project is intentionally split later.

## Running and smoke testing

There is no build, lint, or test runner to invoke.

For browser verification, open:

```text
file:///C:/Users/Joshua%20Toh/OneDrive%20-%20Republic%20Polytechnic/Desktop/C334/Lesson%207/space-quiz/index.html
```

Verify a focused scenario by completing the quiz in the browser or evaluating the relevant page state in the browser console. A smoke pass should cover:

- Native Tab/Shift+Tab keyboard navigation and Enter/Space activation
- One correct answer updating `#score` and the correct-answer style
- One incorrect answer showing the right answer and leaving the score unchanged
- Progress and question state across all 10 questions
- Final `#finalScore`, result message, and restart behavior
- `#themeToggle` switching `:root.light` on and off

Reload the file after HTML, CSS, or JavaScript changes; no compile or dev-server step exists.

## Architecture

`index.html` contains all application surfaces and behavior:

- `<style>` owns the visual system: CSS custom properties for dark/light themes, the `.app` 560px column, the card, controls, feedback states, and responsive breakpoint.
- `questions` is the in-memory question source. Each entry has `q`, `answers`, and a `correct` answer index.
- `current`, `score`, and `answered` are the state variables controlling quiz progression.
- `renderQuestion()` rewrites the question, progress, and answer buttons.
- `selectAnswer()` enforces one selection per question, applies `.correct`/`.wrong`, updates score, and enables the next control.
- `showResults()` switches from `#quiz` to `#results` and chooses the result tier by score ratio.
- `restartButton` and `#themeToggle` reset in-page state and theme state respectively.

Preserve the DOM IDs used by the JavaScript selectors (`#quiz`, `#answers`, `#questionText`, `#nextButton`, `#feedback`, `#results`, and the result/theme controls) unless the script and markup are updated together.

## Project conventions

- Retain the two-theme CSS-variable system. Theme changes should modify variables or `:root.light`; avoid hard-coded theme-only text colors in component rules.
- Keep the interface centered in the existing narrow `560px` column and preserve the `400px` mobile fallback.
- The quiz and feedback copy use mission-control language and compact emoji accents. Correct answers use `--success`; incorrect selections use `--danger`; score/progress accents use `--warm`, `--accent`, and `--accent-2`.
- Keep answer controls native `<button>` elements. Answer choice content is rendered into `.answer` buttons from `questions`; do not replace it with non-semantic clickable elements.
- Once a question is answered, all answer buttons must remain disabled and the score must not change for subsequent clicks.
- Preserve `aria-live="polite"` feedback and the theme toggle's dynamic `aria-label`.
- Use inline SVG data URLs or existing CSS pseudo-elements for decorative grid effects; keep the page self-contained and avoid introducing asset or build dependencies without a repository-level need.
