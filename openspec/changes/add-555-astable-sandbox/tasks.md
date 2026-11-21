# Tasks

## 1. Page shell and styling

- [ ] 1.1 Create `index.html` with layout regions (controls, chip diagram, scope, readouts, audio toggle) and `style.css`; verify the page opens from `file://` with no console errors and a sensible layout

## 2. Simulation engine

- [ ] 2.2 Implement `sim.js` astable engine: analytic charge/discharge between 1/3 and 2/3 Vcc with threshold-crossing clamping and re-phasing on parameter change; verify frequency/duty readouts match `f = 1.44/((R1+2R2)C)` and `D = (R1+R2)/(R1+2R2)` for several R/C combinations via a quick console check
- [ ] 2.1 Add R1, R2, C, Vcc sliders in `main.js` wired to the sim; verify dragging any slider updates frequency/duty readouts immediately without restarting the sim

## 3. Waveform display

- [ ] 3.1 Implement `scope.js` canvas rendering of capacitor voltage and output traces with 1/3 and 2/3 Vcc reference lines and a window scaled to a few periods; verify the cap trace visibly bounces between the reference lines in sync with the output
- [ ] 3.2 Add the LED indicator lit while output is high; verify a slow (>0.5 s period) setup produces a visible blink and readouts still show the true frequency
- [ ] 3.3 Implement the SVG chip diagram (`chip.js`) as a black box with 8 pins labeled with standard and plain-language names; verify all pins render matching the datasheet pinout order

## 4. See-inside view

- [ ] 4.1 Add the "See inside" toggle revealing internal comparators, SR flip-flop, and discharge transistor, state-colored per frame; verify internals animate in sync with the cap trace at a slow frequency and hide again when toggled off

## 5. Audio feedback

- [ ] 5.1 Implement `audio.js`: single OscillatorNode created on first unmute, muted by default, frequency tracking the sim with clamping to 20 Hz–20 kHz; verify no sound on load, tone starts only after unmute, and pitch follows slider changes while the readout shows the unclamped value

## 6. Integration and delivery

- [ ] 6.1 Exercise the full page end to end (file:// and a served static copy): adjust all sliders across extreme values, toggle see-inside, toggle audio, confirm no console errors; verify each spec scenario in `specs/*/spec.md` is observably satisfied
- [ ] 6.2 Update README with what the tool is, how to run it locally, and GitHub Pages deployment note; verify instructions work as written

## Workflow follow-up

- Archive the change after the project's review requirements are satisfied.
