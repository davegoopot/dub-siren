# Spec Delta

## Purpose

Lets the user hear the simulated oscillation as a tone whose pitch tracks the computed frequency, with safe defaults so audio never plays without explicit user action.

## ADDED Requirements

### Requirement: Muted by default
The system SHALL start with audio muted and SHALL only produce sound after the user explicitly unmutes via a toggle.

#### Scenario: Page load
- **WHEN** the page loads
- **THEN** no sound is produced until the user enables audio

### Requirement: Tone tracks simulated frequency
The system SHALL play a tone whose frequency tracks the simulated output frequency derived from the current R1, R2, and C values.

#### Scenario: Raising pitch
- **WHEN** audio is unmuted and the user decreases R2
- **THEN** the tone's pitch rises to follow the new simulated frequency in real time

### Requirement: Audible-range clamping
The system SHALL clamp the audio tone to the audible range (approximately 20 Hz to 20 kHz) while the on-screen frequency readout always shows the true computed value.

#### Scenario: Sub-audible frequency
- **WHEN** components produce a 2 Hz oscillation and audio is unmuted
- **THEN** the readout shows 2 Hz and the tone is clamped to the lower audible bound

#### Scenario: Ultrasonic frequency
- **WHEN** components produce a frequency above 20 kHz and audio is unmuted
- **THEN** the readout shows the true frequency and the tone is clamped to the upper audible bound
