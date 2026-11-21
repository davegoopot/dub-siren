# Design

## Context

Greenfield repo: no application code exists, only a README and the OpenSpec scaffolding. The product locked in during exploration is a single-page astable 555 sandbox (see proposal.md), so there are no integration constraints beyond the zero-build, static-hosting requirements.

## Goals / Non-Goals

**Goals:**
- A single render loop that all views consume: one simulation state per frame, drawn by the scope, chip diagram, LED, and readouts
- Physically faithful astable math, cheap enough to run at 60 fps on modest hardware
- A page that works when opened directly from disk (`file://`) and on GitHub Pages

**Non-Goals:**
- Monostable or bistable modes, SPICE-level accuracy, tolerances/E12 series realism
- Any persistence of user settings, analytics, or server components

## Decisions

**Zero-build, hand-written ES modules.**
`index.html` + `main.js` + a few small modules (`sim.js`, `scope.js`, `chip.js`, `audio.js`, `style.css`) loaded via `<script type="module">`.
*Why not Vite/a framework:* the state model is one small object updated per frame; a reactive framework adds a toolchain for no benefit. *Why not a single giant file:* ES modules are native to the browser even over `file://`, and separate files keep the sim math hackable for novices reading the repo.

**Analytic simulation, not numeric integration.**
Compute the capacitor voltage analytically from the exponential charge/discharge equations against the phase of the current cycle:
- charging: `Vc(t) = Vcc - (Vcc - V1/3) * e^(-t / ((R1+R2) * C))`
- discharging: `Vc(t) = (2/3 Vcc) * e^(-t / (R2 * C))`
When a voltage step from `requestAnimationFrame` would cross a threshold, clamp to the threshold, flip the output state, and carry the remaining time into the next sub-cycle. This survives arbitrary frame rates and slow oscillations (a 0.1 Hz blink still shows correct phase minutes in) without accumulating drift.
*Alternative rejected:* fixed dt steps (drifts, or needs catch-up loops).

**SVG for the circuit/chip diagram, Canvas for the scope traces.**
SVG suits the chip view: boxes, pins, and wires that need hover/labels and discrete state coloring (comparator tripping, flip-flop, transistor), plus the "see inside" show/hide is pure DOM. Canvas suits the continuous scrolling traces. Two renderers, one render call per frame.

**Web Audio: one persistent oscillator, retuned on parameter changes.**
Create a single `OscillatorNode` through a `GainNode` once on first unmute (browser gesture requirement). Slider changes just update `oscillator.frequency.setTargetAtTime(...)`.
Readouts always show the true `f = 1.44 / ((R1 + 2*R2) * C)`; the tone is clamped to ~20 Hz–20 kHz. Alternatives (gating a square-wave buffer per cycle) rejected as needless for the learning goal: hearing pitch track the math is the point.

**Simulation time decoupled from wall time.**
The sim exposes `state(t)` where `t` is sim time; a parameter change re-phases proportionally so the user sees continuity rather than a reset.

## Risks / Trade-offs

- [Very low frequencies demand long traces] -> the scope shows a fixed time window (a few periods) and rescales its time axis to the current frequency, rather than scrolling at real-time rate for 0.1 Hz
- [Very high frequencies alias in the display] -> the scope is a teaching tool; when periods fall below the pixel clock the trace degrades gracefully (fast toggling looks fast), readouts stay exact
- [Browser tab throttling pauses `requestAnimationFrame`] -> on resume, recompute sim time from `performance.now()` deltas with a cap (e.g., skip at most a few seconds), so the trace resumes consistently
- [`file://` restriction bugs] -> no fetch/XHR of local assets at runtime; everything inlined or referenced relatively

## Migration Plan

All new files; nothing to migrate or roll back — revert = delete the new files.
