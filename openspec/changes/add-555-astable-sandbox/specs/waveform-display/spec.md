# Spec Delta

## Purpose

Presents the simulation visually so a novice can see cause and effect: live waveforms, timing readouts, an LED indicator, and an optional view inside the chip itself.

## ADDED Requirements

### Requirement: Live waveform display
The system SHALL render the capacitor voltage and the output square wave as continuously scrolling traces, with visual reference lines at 1/3 Vcc and 2/3 Vcc on the capacitor trace.

#### Scenario: Watching an oscillation
- **WHEN** the simulation is running
- **THEN** the capacitor trace visibly bounces between the 1/3 Vcc and 2/3 Vcc reference lines in sync with the output square wave

#### Scenario: Waveforms follow parameter changes
- **WHEN** the user changes R1, R2, C, or Vcc
- **THEN** the traces reflect the new timing within one frame and reference lines move if Vcc changed

### Requirement: LED output indicator
The system SHALL display an LED indicator that is lit while the output is high, blinking at the true simulated frequency so slow oscillations are visible.

#### Scenario: Slow blink
- **WHEN** components produce a frequency below roughly 10 Hz
- **THEN** the LED visibly blinks on and off with the output state

### Requirement: See-inside toggle
The system SHALL show the chip as a black box by default and provide a toggle that reveals the internal view: the two comparators tripping at the 1/3 and 2/3 Vcc thresholds, the SR flip-flop state, and the discharge transistor state, all animating in sync with the waveforms.

#### Scenario: Default black box
- **WHEN** the page loads
- **THEN** the chip is displayed as a black box with pins only, and internals are hidden

#### Scenario: Revealing internals
- **WHEN** the user enables the "See inside" toggle
- **THEN** the internal blocks are shown animating in sync with the capacitor trace

### Requirement: Novice-friendly pin presentation
The system SHALL present the 8 pins with both their standard names and plain-language labels so a novice can connect the schematic to datasheet pinouts.

#### Scenario: Reading a pin
- **WHEN** the internal or external view is shown
- **THEN** each pin displays its standard name (e.g., TRIG, THRES) alongside a plain-language description
