# Validation

What the outdoor runs actually established, including the ones that failed.

> Exact field coordinates, raw sample data, session records and internal fault
> references are withheld. The results below are reported without them.

## 2026-09-04 — first real multi-location run

The run the project had been working toward: five samples, five different
locations, a position fix on each, and a map that means something.

| | |
| :--- | :--- |
| Source | On-device hardware, fully offline |
| Position | Fixed on every sample |
| Samples | **5 taken · 5 valid · 0 rejected** |
| Field extent | Approximately 26 × 23 m |
| Coverage | 100 % of the mapped area |
| Soil condition index | **0.73 — HEALTHY** |
| Zones | 1, low severity, primary issue moisture, high confidence |
| Evidence level | Limited |
| Methodology | Prototype, unvalidated |

This is the first genuine spatial map the device produced from real ground. Cells
near a sample were well supported; the corners of the area were extrapolated from
further away, which is precisely what the limited evidence level is reporting.

**The same run exposed a failure.** The language layer took over two minutes and
the consistency check rejected what it produced anyway — it had asserted a
rejected-sample count that contradicted the actual result. The device fell back to
its deterministic template and reported that it had done so. The field result was
correct throughout; only the prose was discarded.

## 2026-09-07 — the run that was thrown away

Five samples. The probe read every register cleanly on every sample, with no read
errors, returning live and varying soil values.

**The panel reported 0 of 5 usable.** A whole walk, discarded.

The cause was arithmetic, not soil. A single acquisition-quality figure was
folding a positioning measure into what was supposed to be a question about the
soil reading. With a modest number of satellites visible, that combined figure
could not reach the acceptance level no matter what the probe returned — every
sample was effectively decided before the probe entered the ground.

The fix was to separate the two questions rather than to loosen the threshold.
**Nothing was relaxed to recover this run.** One sample from that session is still
rejected as implausible, and should be.

This is recorded because it is the most useful kind of failure: every component
test passed, the hardware was faultless, and the system was still wrong.

## 2026-09-08 — map pages reach the screen

Until this date the map pages had never been seen running on hardware. The record
format had been tested off-target; the drawing had not, and several flashes that
reported success had never actually reached the microcontroller.

<div align="center">
<img src="../media/field/panel_gps_map.jpg" width="260" alt="The device panel showing the map page with sample positions joined by the walk line">
</div>

## 2026-09-15 — in the enclosure, in a field

The whole instrument carried one-handed and operated over planted ground, with
the probe entering through the base of the enclosure.

<div align="center">
<img src="../media/prototype/enclosure_in_field.jpg" width="260" alt="The unit in its field enclosure, held over planted soil">
</div>

This was the first time the unit was *a thing you carry* rather than a bench
spread, and that changed what mattered: cable strain where the probe lead enters,
whether the screen is readable at arm's length in sunlight, and whether a touch
gesture works while holding a box. All three fed later work on the touch
interface.

## 2026-08-25 — bench bring-up

Before the enclosure existed, every subsystem was brought up and verified
individually on the bench — probe acquisition, positioning, display and the
link between the two processors — and none of them were integrated until each
had been confirmed working on its own.

That order mattered. Several of the faults found later were interactions between
subsystems that each worked correctly in isolation, and having verified them
separately first is what made those interactions identifiable rather than
mysterious.

## What these runs establish

**Established.** The sensor chain works on real ground — probe acquisition,
positioning, collection, spatial reconstruction, zoning, dashboard, panel, and an
operator driving an entire session from the touchscreen. Multi-location mapping on
real ground is done, not theoretical.

**Not established.** That the agronomic interpretation is correct.

This distinction is the most important thing on the page. *"The sensor chain
works"* and *"the soil advice is correct"* are different claims, and only the
first is currently evidenced. Every hardware test above can pass while the
interpretation remains unproven — because the reference bands and weightings the
interpretation rests on have never been checked against field-trial data.

See [Limitations](limitations.md).

## Not yet measured

| | |
| :--- | :--- |
| On-target pipeline timing | Never measured on the device under field conditions |
| Power draw and battery life | Never measured |
| Agronomic accuracy against laboratory reference | Never tested |
| Repeatability across operators | Never tested |
| Behaviour across soil types, crops and seasons | Never tested |
