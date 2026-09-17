# Architecture

A high-level description of how FieldSense is organised. Implementation details,
module contracts and internal interfaces are proprietary and not published here.

![FieldSense system overview](../media/architecture/system_overview.svg)

## Two processors, one hard line

The unit is built on a board carrying both a Linux application processor and a
microcontroller.

The division of labour is deliberate and strict:

| | Responsibility |
| :--- | :--- |
| **Linux side** | Acquires readings, runs the whole decision pipeline, runs the language model, owns storage |
| **MCU side** | Draws the panel, receives positioning data, senses the operator |

**Linux measures and decides. The MCU draws and senses.** Neither crosses into
the other's job.

The link between the two is comparatively slow. That single constraint shaped the
interface: streaming rendered images across it was never viable, so the Linux
side sends a compact description of the result and the microcontroller renders
the panel itself. The screen is drawn locally from data, not received as pixels.

## The pipeline

Processing is a one-way chain. Each stage consumes the previous stage's output
and cannot reach backwards.

### 1 · Sensing

A reading is taken on operator command, paired with the device's position and a
timestamp. Acquisition health is assessed at this point — how cleanly the
measurement was obtained is recorded alongside the measurement itself.

### 2 · Validation

Readings are checked for physical plausibility before they are allowed to
influence anything. A rejected reading never reaches the map.

Rejected readings are **not discarded**. They stay in the session record, marked,
so that a session can be audited afterwards and so the operator can be told
honestly that a sample was thrown away and why.

### 3 · Intelligence

This stage is **deterministic**: the same inputs always produce exactly the same
outputs. There is no randomness, no model inference and no learned component
anywhere in it. That property is what makes the device's behaviour explainable
and testable.

It performs four things in order:

- **Scoring** — individual readings become a condition index.
- **Spatial reconstruction** — sparse sample points become a continuous surface,
  with confidence decreasing away from real readings and unsupported areas left
  blank.
- **Zoning** — the surface is grouped into contiguous management areas, each
  with one primary issue.
- **Guidance** — each zone receives recommendations drawn from fixed rules.
  These rules are structurally incapable of emitting a dosage.

### 4 · Explanation — optional, out of band

A small quantised language model, running locally, restates the finished result
in plain sentences.

It is architecturally downstream of everything. It receives a read-only view of
the deterministic result and returns text. It has no path to alter a score, a
zone, a surface or a recommendation.

Generated text passes a safety filter before display. Text that introduces a
number not present in the result, names a dosage or agrochemical, or contradicts
the figures it describes is rejected and replaced by a deterministic template.
Violations are recorded rather than silently swallowed.

### 5 · Presentation

Two renderers, because a panel in sunlight and a laptop are different jobs:

- **The field panel**, drawn by the microcontroller — the operator's interface.
- **An offline dashboard**, a single self-contained HTML file with no external
  requests of any kind: no CDN, no fonts, no map tiles.

Presentation performs no arithmetic. It reshapes finished results for display and
computes nothing of its own.

## Design properties

**One-way dependency.** Later stages cannot influence earlier ones. The language
model cannot change a number; presentation cannot change a result.

**Deterministic core.** The decision path contains no learned or probabilistic
component. Given the same readings, the device always reaches the same conclusion.

**Degradation is visible.** When a stage cannot deliver — the model is absent, a
reading is rejected, an area has no nearby sample — the system says so in its
output rather than substituting a plausible-looking value.

**Offline by construction.** No stage opens a network connection. This is asserted
by test, not assumed.

**Storage is immediate.** Samples are written as they are taken, not buffered
until the end of a session.

## What is not described here

Module structure, data contracts, interface definitions, algorithm
implementations, parameter values, protocol details and hardware register
handling are proprietary and remain in the private engineering repository.
