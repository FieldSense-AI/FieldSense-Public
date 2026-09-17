# The solution

## What FieldSense is

A handheld, battery-powered instrument. You carry it into a field, take a reading
at several spots, and it produces a map of how the field varies and which patches
need attention — before you leave.

It does not phone anything. There is no app, no account, no subscription, and no
network requirement.

## The walk

A session is a walk, and the device paces it with the operator rather than the
other way round.

1. Switch on. The probe and positioning receiver are found.
2. Push the probe into the ground at the first spot. Press start.
3. The device takes a reading and records where it was taken.
4. Walk to the next spot. Press start again.
5. After the last sample, the device builds the map and shows the result on its
   own screen.

Nothing measures until a person says the probe is in the ground. Each sample is
written to disk the moment it is taken, so an interrupted walk costs one sample
rather than the whole session.

## From points to patches

Readings at a handful of spots are not directly useful. Nobody treats a field
one point at a time.

FieldSense turns them into something actionable in three moves:

**Score.** Each reading is turned into a condition index — a single figure that
says how favourable that spot is, derived from the individual measurements.
The reasoning is described in [Methodology](methodology.md).

**Reconstruct.** You cannot walk every square metre, so the ground *between*
samples is estimated to produce a continuous surface rather than a scatter of
dots. Confidence falls with distance from a real reading, and areas too far from
any sample are left blank rather than guessed at.

**Group.** The surface is divided into contiguous patches that behave similarly.
These are management zones — areas large enough and coherent enough to actually
treat differently. Each zone carries its own primary issue.

The output is not "your field is 67 %". It is "this corner is short of moisture,
that strip is fine, and this patch needs looking at first."

## Guidance, not prescriptions

Each zone receives a recommendation in plain language — what appears to be the
issue and what to review.

**FieldSense never emits a dosage.** It will not tell you to apply a quantity of
anything. That is a deliberate design constraint enforced in code, not a gap
waiting to be filled: quantitative chemical prescriptions require laboratory
buffer testing and local crop-response calibration, and issuing them from a
portable probe would be unsafe.

The device tells you where to look and what seems wrong. A person decides what to
do about it.

## The explanation layer

The last stage is a small language model running on the device itself, which
restates the result in ordinary sentences.

It is the only part of the system that cannot change a number. Every score, zone
and recommendation is final before the model is called; it receives the finished
result and returns prose. Generated text is checked before it is displayed, and
rejected text falls back to a fixed template — with the device reporting which
one you are reading.

The device produces a complete, correct field result with the model switched off
entirely. The language layer is a convenience, not a dependency.

## Why it runs on the device

Every stage — measurement, validation, scoring, reconstruction, zoning, guidance
and explanation — executes on the handheld unit.

That is not a technical preference. A tool for rural fields that depends on
connectivity fails precisely where it is most needed. Offline operation is the
product goal, not a degraded mode.
