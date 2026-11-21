# Proposal

## Why

Novices learning electronics struggle to understand the 555 timer: the pin map is intimidating, the timing math (`f = 1.44 / ((R1+2R2)C)`) feels like magic, and datasheets cannot show the chip's inner state evolving in real time. The repo has no application code yet, making this the right moment to establish the project as a beginner-friendly, zero-friction learning tool.

## What Changes

- Add a zero-build static web page (`index.html` + hand-written JS/CSS, no bundler, no dependencies) that simulates a 555 timer wired in **astable mode**
- Provide a pure-sandbox experience: sliders for R1, R2, C, and Vcc — no guided lessons or challenges
- Render live waveforms: capacitor voltage (bouncing between 1/3 and 2/3 Vcc) and the output square wave
- Show computed readouts of frequency and duty cycle as controls change
- Include an LED indicator blinking at the true simulated output frequency
- Provide a "See inside" toggle: by default the chip is a black box; toggling reveals the internal view (comparators tripping at the 1/3 and 2/3 thresholds, SR flip-flop state, discharge transistor switching) animating in sync with the waveforms
- Provide audio feedback via the Web Audio API: an oscillator tracking the true simulated frequency, muted by default with an unmute toggle, clamped to the audible range (~20 Hz–20 kHz) while readouts always show the true computed frequency
- Support static hosting (open from disk or GitHub Pages) with no server component

## Capabilities

### New Capabilities
- `timer-simulation`: Simulating the astable-mode 555 circuit — component values, timing math, and waveform generation
- `waveform-display`: Visual presentation of the simulation — live waveforms, frequency/duty readouts, LED indicator, and the "See inside" internal chip view
- `audio-feedback`: Audible feedback of the simulated output frequency via the Web Audio API, with muting and audible-range clamping

### Modified Capabilities
- (none — no existing capabilities in this project)

## Impact

- New static assets at the repo root (`index.html`, JS, CSS); no existing files affected
- No dependencies, no build step, no server, no APIs
- Deployable to GitHub Pages as-is
