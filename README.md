# YARD IQ — Gate-In

**An AI-powered container gate-in flow that keeps the human accountable.**
A self-initiated product design concept · Dhaval Sukharamwala

---

Gate-In is the operator-facing mobile flow that governs how a truck and its container
enter a terminal yard: identity, inspection, human judgment, supervisor approval and
gatepass release — with photographic evidence at every step.

**The thesis:** AI proposes. Humans decide. Every decision leaves evidence.

| | |
|---|---|
| **Role** | Product strategy · UX architecture · Interaction & UI design · Design system · Prototyping |
| **Type** | Self-initiated concept · solo |
| **Platform** | Android (rugged shared tablet) |
| **Scope** | Gate-In module · 24 screens designed, 2 planned |
| **Status** | Design complete · Aug 2026 |

## View it

Single self-contained HTML file — no build step, no dependencies, works offline.

- **Live:** `https://<your-username>.github.io/yardiq-case-study/`
- **Local:** open `index.html` in any browser

---

## The problem

Manual gate-in inspection is a terminal's bottleneck and its weakest evidence link.
An operator walks each truck with a clipboard while the lane queues behind it —
observed peak waits of **18–47 minutes**. Container IDs are transcribed by hand from an
11-character ISO 6346 code, in sun glare, off a moving vehicle. Damage discovered later
— a cut seal, a dented panel — becomes a liability dispute between yard, carrier and
shipping line, with no time-stamped record of the container's condition at entry.

So the real problem isn't speed. It's **trust**: nobody can prove what the container
looked like when it came through the gate.

## The four goals

1. **Cut gate cycle time** — AI reads plate, container ID and seal; the operator supervises rather than transcribes.
2. **Make every entry evidence-grade** — time-stamped photos of all six surfaces, AI findings, and a named human decision on each.
3. **Keep the human in charge** — AI proposes, the operator disposes; high-severity findings additionally require supervisor sign-off.
4. **Degrade gracefully** — camera outages, failed reads, missing pre-advice and delayed approvals never block the lane or destroy completed work.

**Non-goals (v1):** Gate-Out, billing, yard slot optimisation, driver self-service.

## Who it's for

| Persona | Role | Core fear |
|---|---|---|
| **Marcus D'Souza** | Gate operator · primary user | Being blamed for damage he didn't miss — or for a call a supervisor silently overturned |
| **James Okafor** | Shift supervisor · approver | Signing a release that later surfaces in a shipping-line dispute |
| **Ramesh Patel** | Truck driver · subject | Unpaid minutes at the gate; repeat inspections |
| **Yard control** | Operations desk · observer | Unreported camera faults and blocked lanes |

## Design targets

These are **success criteria for the concept**, not measured results.

| | Target | Baseline / mechanism |
|---|---|---|
| Gate cycle time | **≤ 6 min** | vs ~12 min manual median, arrival → gatepass |
| Hands-free ID rate | **≥ 85%** | plate + container + seal read with zero correction |
| Evidence-gap disputes | **→ 0** | per 1,000 entries; six time-stamped surfaces + signed decision |
| Approval turnaround | **≤ 15 min** | median supervisor response; escalation at 18 min |

**Cycle-time budget per truck:** trigger 0:30 · identify 1:00 · AI scan 1:30 ·
review 1:30 · approval 1:00 · release 0:30. Approval applies to high-severity holds
only; clean entries release directly after review.

## Designed for the bad days

Of **27 frames**, 14 are happy path, 10 are edge and failure states, and 3 are overlay
sheets — **37% of the flow exists for things going wrong.**

| Condition | Behaviour |
|---|---|
| Lane camera offline | Fault banner; manual check-in path opens; yard control auto-notified |
| AI can't read the container | Retry scan or enter manually; gate timestamps preserved either way |
| No pre-advice record | Non-blocking notice; captured plate + seal carried forward |
| Empty queue | "All clear for now" with inbound-alert promise; camera stays live |
| Supervisor delayed | Truck stays parked, gatepass on hold; escalation to backup keeps the inspection intact |
| Sent back for re-check | Supervisor's reason in their own words; operator's confirmation stays on record — not an override |

## Architecture

Gate-In is a **corridor, not a web**. An operator mid-inspection can move forward, retry,
or back out one level — never sideways into another truck's context.

Seven stages: **A** shift start → **B** lane → **C** waiting → **D** identify →
**E** AI inspection → **F** review findings → **G** submit + release.

Structural rules:

- **Header carries context, sheet carries work.** Amber chrome holds the breadcrumb and stage stepper; the light sheet below holds all interactive content.
- **The truck's plate becomes the breadcrumb** from identification onward — the flow is anchored to a vehicle, not a screen name.
- **Edge states are siblings, not modals.** Camera-offline is a variant of Waiting, so it's shareable, linkable and survives an app restart.
- **Loops are explicit** — retry, undo, re-scan and next-truck all return to a named screen, never an ambiguous "back".

## Hand-off contracts

Operations fail at hand-offs, not screens — so each one carries an explicit expectation.

| Hand-off | Expectation carried |
|---|---|
| Driver → AI | Loop sensor + camera; no operator action while automation is healthy |
| AI → Operator | Every claim ships with confidence % and for/against evidence — never a bare verdict |
| Operator → Supervisor | Est. response stated up front; truck parks; delay has a named escalation that preserves the inspection |
| Supervisor → Operator | In the supervisor's own words; disagreement is a re-work request, never a silent overwrite |
| Operator → Driver | Machine-verifiable QR + explicit placement instruction |
| Driver → Yard control | Placement confirmation closes the entry record |

**Operating principle:** the AI appears twice in the swimlane, and both times it hands
work *to a human* — never to the gatepass. Automation accelerates the corridor but never
shortens the chain of accountability.

## The emotional low point isn't a failure state

Mapped across one truck, the operator's low point is the **approval hold** — where he
loses agency, not where anything breaks. That's why the pending and delayed screens
over-invest in expectation-setting (estimated response, the supervisor's own words, an
explicit "your inspection stays intact") instead of showing a spinner.

## Design system

82 tokens. Sunlight-legible neutrals, one working amber, and one strict rule:

> **Purple appears only when a machine is judging.**

Amber is chrome, status colours belong to content, and all touch targets are ≥ 44 px
for gloved outdoor use. Type is Geist / Geist Mono.

## The honest part

This is a concept project. There was no client and no primary user research — it is
built on analysis of the existing manual process, domain research and documented
operational constraints. All figures above are **design targets, not measured outcomes**,
and the supervisor's side of the flow is assumed rather than designed. The case study
says so in its own words.

## Tech

Single-file HTML with inlined CSS and JavaScript. Screens are embedded as base64 JPEGs,
so the file is fully portable — one file, no asset folder, works offline. Scroll-driven
progress, chart draw-ins and count-ups are plain JS; everything respects
`prefers-reduced-motion`.

## Enabling GitHub Pages

1. Push this repo to GitHub
2. **Settings → Pages**
3. Source: **Deploy from a branch** → branch `main`, folder `/ (root)` → **Save**
4. The URL appears within a minute or two

## Credits

Design, writing and build — **Dhaval Sukharamwala**, Senior UI/UX & Product Designer

## Licence

Case study content, design work and screens © Dhaval Sukharamwala. All rights reserved.
