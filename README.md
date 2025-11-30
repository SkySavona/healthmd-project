# HealthMD

A Vue 3 single page app that demonstrates a patient intake flow for a virtual clinic. It ships with a production ready component set, dark and light themes, WCAG 2.2 AA focused UX, and a Tailwind CSS v4 pipeline powered by Vite. The goal is a fast, accessible, and maintainable intake that captures visit reason, history, contact details, a review step, and a confirmation view.

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Accessibility](#accessibility)
- [Getting Started](#getting-started)
- [Scripts](#scripts)
- [Configuration](#configuration)
- [Development Notes](#development-notes)
- [Testing Checklist](#testing-checklist)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [Roadmap](#roadmap)
- [License](#license)

## Features

- Multi step intake with progress indicator and friendly copy
  - Step 1 Reason for visit
  - Step 2 Health history with selectable conditions and medications
  - Step 3 Contact details with client side validation and examples
  - Step 4 Review and submit with chips and formatted notes
  - Step 5 Confirmation with summary and restart
- Button system with primary and secondary variants, motion safe overlays, and full width behavior on small screens
- Dark and light themes with brand tokens and high contrast defaults
- Motion reduction support for users with reduced motion preference
- Global loader with reduced motion fallback
- Global horizontal overflow guard to prevent side scrolling on mobile
- Component level focus styles that are visible and consistent
- Small bundle size and instant HMR via Vite dev server

## Tech Stack

- Vue 3 with `<script setup>` and Composition API
- Vite 7 for dev and build
- Tailwind CSS v4 with `@tailwindcss/vite` plugin
- Font Awesome for inline icons
- TypeScript in SFCs where helpful
- PostCSS and Autoprefixer

## Project Structure

```
healthmd/
├─ public/
│  └─ favicon.png
├─ src/
│  ├─ assets/
│  │  └─ main.css             # Tailwind entry, base tokens, overflow guard
│  ├─ components/
│  │  ├─ forms/
│  │  │  ├─ IntakeFlow.vue
│  │  │  ├─ IntakeStepReason.vue
│  │  │  ├─ IntakeStepHistory.vue
│  │  │  ├─ IntakeStepDetails.vue
│  │  │  ├─ IntakeStepReview.vue
│  │  │  └─ IntakeStepConfirmation.vue
│  │  ├─ sections/
│  │  │  └─ HeroSection.vue
│  │  └─ ui/
│  │     ├─ Button.vue
│  │     └─ GlobalLoader.vue
│  ├─ App.vue
│  ├─ main.js
│  └─ index.html
├─ package.json
└─ vite.config.js
```

## Accessibility

This project targets WCAG 2.2 AA. Key practices:
- Semantic HTML for sections, legends, lists, and actions
- Programmatic labels and descriptions via `aria-labelledby` and `aria-describedby`
- `role="radiogroup"` with keyboard support for the reason cards
- Visible focus rings with sufficient contrast on light and dark themes
- `prefers-reduced-motion` respected for transitions and the loader
- Error handling reads clearly using `role="alert"` where helpful
- Large touch targets and logical tab order
- Color choices that meet 4.5 to 1 text contrast guidelines for body copy

### Screen Reader and Keyboard Map

- Step groups use legends and helper text and they are referenced in control groups
- First invalid field receives focus on validation errors
- Buttons have clear text and do not rely on color only

## Getting Started

### Prerequisites

- Node 20.19 or 22.12 and higher
- PNPM or NPM. The repo uses NPM scripts by default.

### Install

```bash
npm install
```

### Run in development

```bash
npm run dev
```

The server prints Local and Network URLs. If testing on a phone, use the Network URL.

### Build for production

```bash
npm run build
```

### Preview the production build

```bash
npm run preview
```

## Scripts

- `npm run dev` starts Vite
- `npm run build` outputs a production build to `dist`
- `npm run preview` serves the build for smoke testing

## Configuration

### Tailwind CSS v4

Tailwind is configured via the `@tailwindcss/vite` plugin. The entry file is `src/assets/main.css` which imports Tailwind and defines brand tokens inside `@theme`, plus base styles inside `@layer base`. Utilities are available without a local tailwind.config.js since v4 can infer from usage. If you add new static class strings in non standard files, ensure Vite knows how to scan them.

### Brand Tokens

Defined in `main.css`:

```css
@theme {
  --color-brand-primary: #013047;
  --color-brand-teal: #0081a2;
  --color-brand-accent: #00c1b2;
  --color-brand-deep: #006d86;
  --color-brand-hover: #0082a3;
  --color-brand-chip: #f5fcff;
}
```

### Overflow Guard

To prevent accidental horizontal scroll on small screens:

```css
html, body {
  overflow-x: hidden;
  max-width: 100%;
}
```

## Development Notes

- Buttons stack full width on small screens and switch to row alignment on `sm` and up using `flex-col sm:flex-row` and `w-full sm:w-auto`
- The Button component uses an overlay span that animates on hover only when not disabled. It respects motion reduce by hiding the overlay
- Contact step validates name email and phone. Phone requires at least ten digits after stripping non digits. First invalid field receives focus
- Review step shows all inputs including an optional note
- Confirmation step summarizes contact targets and offers restart
- App boot shows a loader for a short time to simulate an async fetch. The loader animation disables under reduced motion

### Troubleshooting

If you see `Cannot apply unknown utility class 'bg-slate-50'` during dev or build:
1. Ensure you are on Tailwind v4 and using `@tailwindcss/vite` in `devDependencies`
2. Stop and restart Vite after editing the Tailwind entry file
3. Confirm `@import "tailwindcss";` is at the top of `src/assets/main.css`
4. Verify the file path is included in the Vite graph by importing the CSS in `main.js` or `App.vue`

## Testing Checklist

Manual checks that should pass before merging:
- Keyboard only journey through every step without traps
- Visible focus styles on all interactive controls
- Error messages announced by screen readers when they appear
- Reduced motion set in OS yields no animated overlays or loader sweeps
- Mobile layout shows full width buttons and no horizontal scroll
- Desktop layout shows left and right buttons on one row
- Dark mode and light mode readable text and controls

## Deployment

The app is a static SPA. Any static host works. Vercel and Netlify are recommended.
- Build with `npm run build`
- Upload the `dist` directory to your host
- Set the site to serve `index.html` for route fallbacks if you add routing

## Contributing

Use small focused pull requests and include a summary with context, what changed, why it changed, and how to test. Keep the screenshots section short and include before and after where layout or color changed.

## Roadmap

- International phone formatting and validation
- Optional email confirmation field
- Persist submissions to an API with optimistic UI
- Unit tests for validators and Button interactions
- Visual regression snapshots for core screens

## License

MIT. See `LICENSE` for details.
