# Roadmap

Where FieldSense goes next, in the order that matters.

## 1 · Agronomic validation — the one that unblocks everything

**This is the single most important open item**, and nothing else on this list
changes what the device can honestly claim until it is done.

The scoring bands and weightings are prototype values that have never been checked
against field-trial data. Until they are, the device can report that patches of a
field *differ* but cannot stand behind the absolute number it prints.

What this requires:

- Paired sampling against certified laboratory analysis, across a range of soils
- Enough repeat visits to establish repeatability
- Crop- and region-specific reference bands rather than one general-purpose set
- A documented uncertainty figure attached to the reported index

Until this work is done the evidence level stays limited, and it should.

## 2 · Measure what has never been measured

Several properties of the instrument are simply unknown, and claiming anything
about them would be guessing:

- **Power draw and battery life** — never measured. This determines how long a
  walk can be, which is a basic property of a field instrument.
- **On-target pipeline timing** under real field conditions.
- **Probe drift and calibration stability** over time and across insertions.

## 3 · Repair and harden the hardware

- **Fix the touch wiring fault** so coordinate-accurate touch works and
  hit-testing re-enables.
- **A real enclosure.** The current housing is a test fixture. A field instrument
  needs weather resistance, sensible cable strain relief at the probe entry, and
  a screen readable in direct sunlight.
- **Power architecture review** once actual draw is known.

## 4 · The explanation layer

The on-device language model currently fails its consistency check for the field
summary, so that section falls back to a deterministic template.

- Understand *why* it fails rather than loosening the check
- Reduce generation time, which is currently on the order of minutes
- Evaluate whether a small model earns its place at all, or whether well-written
  deterministic templates are simply the better answer

That last question is open and genuinely might resolve against the model.

## 5 · Sampling and mapping

- **Guidance on where to sample.** The device currently maps what you sampled;
  it does not help you choose good sampling points.
- **Denser walks** and how reconstruction quality changes with sample count.
- **Revisit comparison** — showing how a field has changed between sessions.

## 6 · If it goes further

Beyond the prototype, and dependent on validation succeeding:

- Multi-field session management
- Export in formats farm management software can read
- Trials with real growers on real decisions, which is the only way to learn
  whether the output is genuinely actionable

## What is deliberately not on this roadmap

**Dosage recommendations.** The device will not tell you how much of anything to
apply. This is a permanent design constraint, not a deferred feature. Quantitative
chemical prescriptions require laboratory buffer testing and local crop-response
calibration, and issuing them from a portable probe would be unsafe regardless of
how good the instrument becomes.

**Cloud processing.** Offline operation is the product goal. A tool for rural
fields that depends on connectivity fails exactly where it is needed.

**Open-sourcing the implementation.** This repository is a public showcase. The
implementation remains proprietary.
