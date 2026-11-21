# Spec Delta

## Purpose

Simulates an astable-mode 555 timer circuit, converting user-chosen component values into a live, physically accurate oscillation that drives all other views.

## ADDED Requirements

### Requirement: Component value controls
The system SHALL provide independent controls for R1, R2, C, and Vcc, each within a range suitable for typical 555 astable experimentation, and the simulation SHALL update continuously as values change.

#### Scenario: Adjusting a resistor
- **WHEN** the user changes R1 or R2
- **THEN** the oscillation frequency and duty cycle update immediately without requiring a restart

#### Scenario: Adjusting the capacitor
- **WHEN** the user changes C
- **THEN** the oscillation updates immediately while the simulation continues running

### Requirement: Accurate astable timing
The system SHALL simulate the standard 555 astable behavior: the capacitor charges through R1 and R2 until it reaches 2/3 Vcc, then discharges through R2 until it falls to 1/3 Vcc, repeating.

#### Scenario: Expected frequency
- **WHEN** components are set to R1 = 1k, R2 = 100k, C = 10uF
- **THEN** the oscillation period SHALL match the standard formula within normal rendering tolerance

#### Scenario: Threshold crossings
- **WHEN** the capacitor voltage reaches 2/3 Vcc
- **THEN** the output transitions low and the capacitor begins discharging toward 1/3 Vcc

### Requirement: Derived timing readouts
The system SHALL display the computed oscillation frequency and duty cycle derived from the current R1, R2, and C values, using the standard formulas.

#### Scenario: Readout follows sliders
- **WHEN** the user changes any timing component
- **THEN** the frequency and duty cycle readouts update immediately to reflect the new values

### Requirement: Pure sandbox behavior
The system SHALL operate as a free-play sandbox with no lessons, challenges, or progression, and SHALL persist no user state between sessions.

#### Scenario: No-obstruction exploration
- **WHEN** the user opens the page
- **THEN** the sandbox is immediately playable with default component values and no gated content
