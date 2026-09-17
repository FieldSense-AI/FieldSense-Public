# Hardware

The physical stack and what each part contributes.

> Wiring, pin assignments, register maps, electrical specifications, power-domain
> details and the bill of materials are proprietary and are not published here.

## The unit

FieldSense is a single handheld assembly, powered from batteries, that contains
everything needed to take a reading and produce an answer.

| Subsystem | Component | Role |
| :--- | :--- | :--- |
| **Compute** | Arduino UNO Q — Qualcomm QRB2210 application processor running Debian, with an STM32U585 microcontroller on the same board | The whole instrument: acquisition, decision pipeline, language model, panel and operator input |
| **Soil sensing** | 7-in-1 soil probe, industrial serial bus | Nitrogen, phosphorus, potassium, pH, conductivity, moisture, temperature |
| **Bus interface** | Differential serial transceiver | Carries the probe's protocol reliably over cable length in the field |
| **Positioning** | GNSS receiver | Tags each sample with a position |
| **Display** | 2.8" SPI TFT, 320 × 240, with resistive touch | The operator's only interface |
| **Power** | Separate supplies for the logic and the probe | The probe's requirement differs from the board's |
| **Storage** | Onboard flash | Session records written as samples are taken |

## Why this compute platform

The UNO Q carries two processors on one board, and FieldSense uses both for what
each is good at.

The application processor runs a full Linux system, which is what makes an
on-device language model and a standard-library Python pipeline practical on a
handheld instrument. The microcontroller provides deterministic, immediate
control of the display and the operator input — timing behaviour that a
general-purpose operating system does not guarantee.

Splitting the work this way means the panel stays responsive while the pipeline
is working, and the operator's press is never missed because something else was
scheduled.

## Why a 7-in-1 probe

Seven measurements from a single insertion is the property that makes a walking
survey viable. Separate instruments per measurement would multiply both the
per-point time and the number of things to carry, and sampling density is exactly
what FieldSense exists to increase.

The trade-off is honest: a single multi-parameter field probe is less accurate
than dedicated laboratory instrumentation. That is the deliberate exchange —
density and immediacy in place of absolute precision. See
[Methodology](methodology.md) for what that means for the results.

## Why an industrial serial bus

The probe communicates over a differential industrial bus rather than a simple
logic-level connection. In a field this matters: differential signalling tolerates
cable length and electrical noise far better, and the protocol carries integrity
checking so a corrupted reading is detected rather than silently used.

## Why a separate positioning receiver

Every sample must be tagged with where it was taken, and a dedicated receiver on
the unit keeps that independent of any network. It is also what lets the device
judge whether the operator has genuinely moved to a new location between samples,
rather than trusting that they did.

## Why this display

A 2.8-inch panel is large enough to show a map, a score and guidance at once, and
small enough to stay in a handheld enclosure with reasonable power draw. Touch
makes the whole session operable from the glass — start, retry, page through
results, begin again — without a laptop or a network connection.

## Enclosure

The prototype enclosure is exactly what it appears to be in the photographs:
a field-expedient housing built to get the electronics outdoors and take real
readings. It is a test fixture, not a product design. A proper enclosure is on
the [roadmap](roadmap.md).

<div align="center">
<img src="../media/prototype/enclosure_in_field.jpg" width="300" alt="The unit in its field enclosure, held over planted soil">
</div>

## Verification status

Each subsystem was brought up and verified individually on real hardware before
integration. Component-level verification is complete; the outcomes of full-system
field runs are in [Validation](validation.md).

One known hardware fault is documented there: position-accurate touch is
unavailable on the current unit due to a wiring problem on the touch controller.
Press detection works and the whole session is operable; only coordinate-accurate
hit-testing is disabled, and it re-enables itself if the wiring is repaired.
