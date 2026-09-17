# Methodology

How FieldSense reaches a conclusion, and how far that conclusion should be
trusted.

> This document describes the **reasoning**. The reference bands, weightings,
> thresholds and parameter values that implement it are proprietary and are not
> published.

## What is measured

Seven quantities at each sampling point:

| Measurement | Why it is here |
| :--- | :--- |
| Nitrogen, phosphorus, potassium | The primary nutrients driving crop nutrition |
| pH | Governs whether nutrients present in the soil are actually available |
| Electrical conductivity | A proxy for salinity, which suppresses uptake |
| Moisture | Water availability at the time of sampling |
| Temperature | Recorded for context |

Position and a timestamp are recorded alongside every reading.

## From a reading to a score

Each measurement is converted to a value between 0 and 1 describing how
favourable it is — not how large it is.

The conversion depends on the shape of the quantity's relationship to soil
condition, and three shapes are used:

**Optimum range.** Some quantities are best inside a band, and worse in either
direction. pH is the clearest case: too acidic locks up some nutrients, too
alkaline locks up others. Values inside the favourable band score at the top;
values outside it fall away on both sides.

**Adequacy with a penalty for excess.** Nutrients behave asymmetrically. Too
little is straightforwardly bad. Too much is *also* bad — it is wasted money and
an environmental cost — but it is not as immediately damaging to the crop as
deficiency. The scoring reflects that asymmetry rather than treating excess as
equivalent to absence.

**Upper limit.** Some quantities are only a problem above a level. Salinity is
one: below a threshold it is not an issue at all, and beyond it the penalty grows.

**Temperature is recorded but not scored.** There is no soil temperature at which
the device could recommend a useful action, so scoring it would add a number
without adding information.

## From scores to one figure

The individual scores are combined into a single soil-condition index using a
fixed weighting — a deterministic calculation, not a learned model.

The weighting is not uniform. Quantities with a broader influence on crop
outcomes carry more of the result than those with a narrower one. The index is
then reported as a percentage with a plain-language band: healthy, moderate or
poor.

**The specific weights and band boundaries are proprietary.**

There is a second composite index describing longer-term soil condition, built
from the same scores under a separate weighting using a multi-criteria approach.

## From points to a surface

Sampling produces values at a handful of locations. A field needs a value
everywhere.

FieldSense estimates the ground between samples using distance-based
interpolation: nearby readings influence an estimate more than distant ones.

Two properties matter more than the technique:

- **Confidence is tracked, not assumed.** Every estimated point records how far
  the nearest real reading was and how many readings supported it.
- **Unsupported ground is left blank.** Beyond a support radius the device
  produces *no value* rather than an extrapolated guess. A blank area is an
  honest statement that the walk did not cover it.

## From a surface to zones

The continuous surface is divided into contiguous regions that behave similarly
— connected areas, not statistical clusters scattered across the field. A
management zone you cannot walk to as a single patch is not useful.

Very small fragments are not promoted to zones. Each zone is assigned one primary
issue: the single measurement most responsible for its condition, chosen by a
fixed priority ordering so that the answer is stable and explainable rather than
dependent on which difference happened to be largest.

## Guidance

Each zone's primary issue selects from fixed rule tables, producing a short
recommendation about what to review.

**No rule can emit a quantity.** The tables have no path to producing a dose, a
rate or a volume. Quantitative chemical prescriptions require laboratory buffer
testing and local crop-response calibration; issuing them from a portable probe
would be unsafe, so the capability is absent by construction rather than
disabled by configuration.

## How far to trust this

This is the section that matters most, and it is the one most projects leave out.

### What is solid

The arithmetic is deterministic and covered by an automated test suite. The same
readings always produce the same result. The validation gate genuinely rejects
implausible readings. Interpolation confidence is tracked honestly and
unsupported ground is genuinely left blank. The language layer genuinely cannot
alter a number.

### What is not

**The reference bands and weightings are prototype values.** They are
general-purpose — not specific to a crop, a soil type, a region or a season — and
they have **not been validated against field-trial data**.

Concretely, that means:

- The **relative** picture is the trustworthy output. Which patches of a field
  differ, and in which measurement, is what the device is currently good for.
- The **absolute** percentage should not be read as an agronomic verdict. A
  figure of 43 % means "this scored lower than that" and not "this soil is 43 %
  healthy" in any externally meaningful sense.
- Comparisons **between different fields** are not supported by any evidence.

Every result the device produces is labelled with a limited evidence level for
exactly this reason.

### The one-sentence version

**Trust the device to tell you which parts of a field differ and in which
measurement. Do not yet read the absolute number as an agronomic verdict.**

Validating the bands and weights against real trial data is the single largest
open item on the [roadmap](roadmap.md).

## Why deterministic

A learned model could plausibly produce better numbers. It was not used, for
three reasons:

1. **Explainability.** A farmer is entitled to know why a patch was flagged. A
   fixed calculation can be explained; a learned score often cannot.
2. **Testability.** Deterministic output can be asserted exactly in tests. That
   is what makes a refactor safe on a device that goes into a field.
3. **Honest data requirements.** Training a credible model needs validated
   field-trial data across soils and crops — the same data the current bands are
   waiting on. Building a learned model on top of unvalidated assumptions would
   hide the uncertainty instead of stating it.
