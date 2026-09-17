# Field workflow

What happens between switching the unit on and reading the answer.

The device asks for one thing at a time and never advances on its own. Every step
waits for the operator.

## The sequence

| The panel says | What it means | You do |
| :--- | :--- | :--- |
| `FIELDSENSE` / `STARTING` | The probe and positioning receiver are being found | Wait |
| `PLACE PROBE - PRESS START` | Ready for a reading | Push the probe into the ground, then press start — the board button or a tap on the glass |
| `MEASURING - PLEASE WAIT` | Taking the reading | Hold still. Taps during a measurement are discarded, not banked |
| `SAMPLE n SAVED` | Written to disk | Live readings are shown |
| `RESEAT PROBE - RETRY SAMPLE n` | That reading was rejected as implausible | Reseat the probe and press start — same sample index, nothing lost |
| `MOVE TO NEXT LOCATION` | This point is done | Walk to the next spot, press start there |
| `PROCESSING - PLEASE WAIT` | Building the map | Wait |
| `FIELD STATUS` + result · `COMPLETE - HOLD FOR NEW RUN` | Finished | Read the result. A tap turns the page; only a deliberate hold starts a new run |

A whole session runs from the glass: start, retry, advance, begin again. No
laptop, no network, and no board button required — though the button still works
and feeds the same press.

## Why a hold, not a tap, starts a new run

Because coordinate-accurate touch is unavailable on the current unit (see
[Limitations](limitations.md)), a stray brush against the screen registers as a
press. If a tap started a new run, a result nobody had read could be discarded by
accident — including a failed walk's screen, which carries the reason it failed.

So the result screen holds until it is deliberately dismissed.

## Why the state machine, rather than a loop

Three properties that a simple counted loop did not provide:

**The operator sets the pace.** Nothing measures until a person says the probe is
in the ground. The device never decides on its own that it is time to sample.

**A power cut costs one sample, not the walk.** Each sample is written to disk
the moment it is taken, under its own index. An interrupted session loses the
sample in progress and nothing else.

**Position is judged, not assumed.** Every sample records whether the device had
actually moved far enough for this to be a genuinely different place. Positioning
jitter while standing still is never mistaken for a second sampling site.

Illegal transitions raise an error rather than being quietly absorbed. A device
that silently ends up in the wrong state stores samples under the wrong index,
and nobody finds out until the session is inspected afterwards.

## Rejected readings

A reading that fails the plausibility check does not advance the walk. The panel
asks for the same sample index again.

Rejected readings are kept in the session record, marked as rejected. They are
excluded from the map but available for audit, so a session can be reviewed
afterwards and the operator is told honestly that something was thrown away.

## Reading the result

The result screen pages through, one tap at a time:

- Overall condition score and the zone bar
- Individual measurements
- The map of the walk, with sample positions joined in order
- Plain-language guidance

<div align="center">

| Result screen | Map page |
| :---: | :---: |
| <img src="../media/field/panel_result.jpg" width="230" alt="Result screen showing samples complete, score and zone bar"> | <img src="../media/field/panel_gps_map.jpg" width="230" alt="Map page showing sample positions joined by the walk line"> |

</div>

## Offline verification

A field session is verified to open no off-board network connection. This is
asserted by automated test, not assumed — and the procedure for confirming it on
real hardware is to turn the radios off, cold-boot the unit, and run a complete
walk with no laptop attached.
