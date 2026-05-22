# Animated Todo Redesign Design

## Goal

Replace the current historical-theme todo UI with a visually striking animated
experience while keeping the implementation inside
`fastapi-app/templates/index.html`.

The result should feel alive on first load, remain legible during repeated task
work, and preserve the existing FastAPI todo workflow.

## Current Context

- FastAPI serves a single HTML template from `fastapi-app/templates/index.html`.
- The frontend calls the existing `/todos` collection and item endpoints.
- The current template already supports task creation, completion, deletion,
  filtering, search, sorting, progress statistics, export, and a theme toggle.
- The backend model currently persists `id`, `title`, `description`,
  `completed`, `due_date`, and `priority`. The redesign must not require backend
  changes.

## Recommended Direction

Use a hybrid "Pulse Board" design.

The background becomes a responsive HTML5 canvas scene with ambient particles,
light connections, and pointer-aware motion. The task interface stays above it
as a crisp glass-like work surface with strong hierarchy, controlled animation,
and readable task metadata.

This balances the user's request for an innovative moving design with the
practical constraint that a todo list still needs fast scanning and reliable
controls.

## Visual Structure

### Ambient Canvas Layer

- A full-viewport canvas sits behind the application surface.
- Particles drift continuously and form short-range luminous links.
- Pointer and touch movement nudge nearby particles and produce a soft glow.
- Task completion can trigger a brief pulse or burst emitted near the todo
  surface without obscuring controls.
- Motion respects reduced-motion preferences by lowering or disabling ambient
  animation intensity.

### Task Surface

- The foreground uses a dashboard-like composition rather than a decorative
  historical sheet.
- The header shows the product identity, active workload, completion progress,
  and a short live status summary.
- The composer area keeps task entry compact and supports title, description,
  due date, and priority values accepted by the current API.
- Search, filter, sort, export, and bulk cleanup stay visible as utility
  controls.
- Todo items present priority, deadline state, description, completion control,
  and delete action with clear separation.

## Interaction Design

- Initial load uses subtle staged entrance motion for the header, composer, and
  task list.
- Filter and sort changes animate list state without hiding the final result.
- Completing a task updates stats immediately and produces a satisfying canvas
  pulse plus card-state transition.
- Overdue and urgent tasks gain stronger visual emphasis through color and
  metadata treatment, not constant distracting motion.
- Empty, filtered-empty, API-error, and export feedback states remain explicit
  through visible messaging or toast feedback.

## Behavior And Data Flow

1. The template loads and starts the canvas scene.
2. Existing JavaScript fetches `/todos`.
3. Todo records are normalized for rendering with defaults for optional fields.
4. Foreground controls update local view state for search, filter, and sort.
5. Create, update, and delete operations use the existing FastAPI endpoints.
6. The rendered list and summary stats refresh after mutations.

The redesign must not add new backend endpoints or change persistence format.

## Accessibility And Performance

- Foreground text and controls maintain sufficient contrast over the animated
  background.
- Buttons, inputs, filter tabs, and task actions remain keyboard reachable.
- Canvas remains decorative and does not carry required task information.
- Animation uses `requestAnimationFrame`, caps particle count by viewport size,
  pauses or reduces work when the page is hidden, and avoids layout thrashing.
- Small screens collapse the dashboard into a compact single-column layout.

## Implementation Scope

In scope:

- Replace CSS, markup structure, and inline JavaScript inside
  `fastapi-app/templates/index.html`.
- Preserve existing todo actions and utility functions.
- Add canvas animation, animated progress/status treatment, richer empty states,
  completion pulse effects, and responsive foreground layout.

Out of scope:

- Backend changes in `main.py`.
- New static asset files, external build tooling, or JavaScript frameworks.
- Database or todo schema changes.
- Drag-and-drop persistence or task editing that would need new API behavior.

## Verification

- Load the FastAPI root page and confirm the redesigned interface renders.
- Verify create, complete, delete, search, filter, sort, export, and bulk delete
  flows against the current API behavior.
- Check desktop and narrow mobile layouts for overlap, readability, and usable
  controls.
- Confirm canvas animation is visible when motion is allowed and subdued when
  reduced motion is requested.
- Run existing project tests after the template change.
