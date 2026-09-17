# Limitations

What FieldSense does not do, cannot currently claim, and should not be trusted
for. This page is deliberately blunt.

## The limitation that matters most

**The agronomic interpretation is unvalidated.**

The reference bands and weightings that turn readings into a condition index are
prototype values. They are general-purpose — not specific to a crop, a soil type,
a region or a season — and they have never been checked against field-trial data.

Every result the device produces carries a limited evidence level for this reason.

What follows from that:

- **Relative comparison within one field is the trustworthy output.** Which
  patches differ, and in which measurement, is what the device is currently good
  for.
- **The absolute percentage is not an agronomic verdict.** It should be read as
  "this scored lower than that", not as a statement about the soil's condition in
  any externally meaningful sense.
- **Comparisons between different fields are unsupported.** Nothing has been
  established about whether the index is meaningful across sites.

Every hardware test can pass while this remains true. A working sensor chain and
correct soil advice are different claims.

## Measurement limitations

**A field probe is not a laboratory.** A single multi-parameter probe is less
accurate than dedicated laboratory instrumentation. That is the deliberate trade:
sampling density and immediacy instead of absolute precision. FieldSense does not
replace laboratory analysis and is not trying to.

**A reading is one moment at one depth.** Soil moisture in particular changes
through the day and with recent weather. A walk captures a snapshot.

**Sample placement is the operator's judgement.** The device maps what you
sampled. A walk that misses a corner produces a map with no information about
that corner — which the device reports as blank rather than filling in.

**Interpolation is an estimate.** Ground between samples is reconstructed, not
measured. Confidence decreases with distance from a real reading, and the device
reports that rather than hiding it.

## Known hardware faults

**Position-accurate touch is unavailable on the current unit.** This is a wiring
fault on the touch controller, not a firmware problem. Press detection works and
the entire session is operable from the glass; only coordinate-accurate
hit-testing is disabled. It re-enables itself if the wiring is repaired.

**The enclosure is a test fixture.** The prototype housing exists to get the
electronics outdoors. It is not weatherproof, not ruggedised, and not a product
design.

## Known software limitations

**The language model's field summary is currently rejected.** It repeatedly fails
the consistency check on the device, so that section is served by a deterministic
template. The device reports which one you are reading. This is the safety
mechanism working as designed, but it means the model is not currently earning
its place for that section.

**Language model output is slow on-device.** Generation takes on the order of
minutes, not seconds.

## Never measured

| | |
| :--- | :--- |
| On-target pipeline timing under field conditions | Not measured |
| Power draw and battery life | Never measured |
| Agronomic accuracy against laboratory reference | Never tested |
| Repeatability across operators or repeat visits | Never tested |
| Behaviour across soil types, crops, seasons or regions | Never tested |
| Long-term probe drift or calibration stability | Never tested |

## What the device deliberately will not do

**It will not give you a dosage.** No rule in the system can emit a quantity, a
rate or a volume. This is not a missing feature — it is a safety constraint built
into the structure. Quantitative chemical prescriptions require laboratory buffer
testing and local crop-response calibration, and issuing them from a portable
probe would be unsafe.

**It will not let the language model change a number.** Scores, zones and
recommendations are final before the model is called.

**It will not guess at ground it has no data for.** Areas too far from any sample
are left blank rather than extrapolated.

## Scope

FieldSense is a **working student research prototype**. It has been operated
outdoors, it has produced real spatial maps, and it has failed in the field and
been fixed. It is not a product, it is not certified for any agronomic purpose,
and no decision with financial or environmental consequences should rest on its
output in its current state.
