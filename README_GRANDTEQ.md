# GRANDTEQ

## A Next-Generation Physically Modelled Piano

> **Sampled realism without sampled behaviour.  
> Modelled behaviour without modelled timbre.**

GRANDTEQ is a research concept for a new generation of virtual piano.

The goal is to combine:

- the tonal realism and natural timbre of a high-end sampled piano,
- the continuous, stateful behaviour of physical modelling,
- modern differentiable DSP and machine learning,
- and a clean-sheet architecture unconstrained by legacy compatibility.

The initial objective is **not** to beat Pianoteq as a finished commercial product.

The first goal is much more focused:

> **Can we break the traditional compromise between realistic sampled timbre and realistic modelled behaviour?**

If the answer is yes, the project could later evolve into a commercial virtual instrument.

---

## Table of Contents

1. [The Core Problem](#1-the-core-problem)
2. [Short A-Z Roadmap](#2-short-a-z-roadmap)
3. [Detailed Development Plan](#3-detailed-development-plan)
4. [Six-Month Prototype Schedule](#4-six-month-prototype-schedule)
5. [Success Criteria](#5-success-criteria)
6. [Long-Term Product Vision](#6-long-term-product-vision)

---

# 1. The Core Problem

Current virtual pianos tend to fall into two broad categories.

### Sampled pianos

High-end sampled instruments can reproduce the basic tone of a real piano extremely convincingly because the sound originates from a real instrument.

Their weakness is behaviour.

Individual notes and sparse textures can feel static or "dead" because the piano is not truly behaving as one continuously coupled physical system.

### Physically modelled pianos

Physical modelling can reproduce:

- sympathetic resonance,
- continuous pedal behaviour,
- evolving harmonics,
- note interaction,
- state-dependent decay,
- dynamic response.

But modelled pianos often retain a recognisably synthetic tonal signature.

Different piano models may still share a common underlying character.

A frequently perceived problem is a slightly closed, veiled or "behind a blanket" presentation compared with the openness, depth and spatial complexity of a real grand piano.

### The target

GRANDTEQ aims to combine:

**the natural timbre of sampling**

with

**the living behaviour of physical modelling.**

---

# 2. Short A-Z Roadmap

## 1. Define the perceptual problem
Identify exactly what makes current modelled pianos sound synthetic and what makes sampled pianos feel static.

## 2. Build a reference listening set
Compare a real Steinway, Pianoteq, Roland modelling and a top sampled library such as VI Labs Modern D.

## 3. Create a minimal research engine
Build only:

`MIDI -> model -> audio`

No GUI, plugin format, licensing system or commercial features.

## 4. Build the physical core
Model hammer excitation, strings, inharmonicity, unisons, damping, decay and basic coupling.

## 5. Treat the piano as a stateful system
The instrument must remember what is already vibrating and how previous notes, pedals and dampers affect new notes.

## 6. Separate behaviour from timbre
Let physical modelling control interaction and dynamics while a learned model improves acoustic realism.

## 7. Add differentiable parameter fitting
Automatically fit model parameters against recordings instead of tuning everything manually.

## 8. Add a neural residual or learned acoustic renderer
Train a neural model to reproduce what the simplified physical model fails to capture.

## 9. Use a Steinway Spirio for controlled measurements
Use repeatable performances on a professionally prepared acoustic grand piano.

## 10. Measure only what answers specific questions
Do not create a huge dataset blindly. Design measurements around known model weaknesses.

## 11. Focus heavily on spatial radiation and openness
Investigate soundboard radiation, high-frequency detail, transients and spatial behaviour.

## 12. Add advanced piano interactions
Sympathetic resonance, pedal behaviour, repeated notes, key release, una corda and dense textures.

## 13. Run blind listening tests
Compare against real recordings, sampled pianos and existing physical models.

## 14. Iterate based on perceptual bottlenecks
Fix what listeners can reliably hear first, not what is mathematically most elegant.

## 15. Decide whether the technology deserves commercial development
Only after the core idea works should development move toward VST/AU, GUI, presets, licensing and productisation.

---

# 3. Detailed Development Plan

## 3.1 Define the perceptual problem

Development should begin with listening, not coding.

The key question is:

> **What still makes a physically modelled piano recognisably synthetic?**

Current modelling systems often reproduce musical behaviour extremely well.

However, they may retain a persistent tonal fingerprint that survives across different piano models.

Sampled pianos have the opposite weakness: their tone can be highly convincing while their behaviour remains comparatively static.

The project should explicitly target this gap.

---

## 3.2 Build a reference listening set

Create a controlled comparison set using:

- a professionally prepared Steinway,
- Steinway Spirio recordings where possible,
- VI Labs Modern D or another high-end sampled piano,
- Pianoteq,
- Roland physical modelling,
- the GRANDTEQ prototype.

The test material should include:

- isolated notes,
- pianissimo,
- fortissimo,
- long decays,
- unisons,
- repeated notes,
- pedal transitions,
- sympathetic resonance,
- chords,
- dense textures.

The purpose is not simply to ask:

> "Which sounds better?"

Instead, identify **which specific characteristics make each approach convincing or artificial**.

---

## 3.3 Create a minimal research engine

Do not begin by building a commercial plugin.

The first system can be extremely simple:

```text
MIDI
  |
  v
Physical / Hybrid Model
  |
  v
Audio Output
```

The research engine initially needs only:

- MIDI note input,
- velocity,
- pedals,
- offline WAV rendering,
- parameter control,
- logging,
- analysis tools,
- A/B comparison.

Real-time playback can follow.

Avoid spending early development time on:

- VST3,
- Audio Units,
- polished GUI,
- preset browsers,
- installers,
- DRM,
- licensing,
- DAW compatibility.

The first objective is to determine whether the underlying architecture works.

---

## 3.4 Build the physical core

Start with a small but meaningful physical model.

Core elements should include:

- hammer excitation,
- nonlinear hammer/string interaction,
- string vibration,
- inharmonicity,
- multiple strings per note,
- slight detuning between unison strings,
- damping,
- decay,
- bridge coupling,
- simplified soundboard behaviour.

Development should progress approximately as:

```text
single string
-> one piano note
-> unison group
-> representative notes across registers
-> full keyboard
```

A smaller model that can be measured and understood is more useful than a huge model whose errors are impossible to diagnose.

---

## 3.5 Model the piano as a stateful system

The piano must not behave as 88 independent sound generators.

The internal state should persist over time.

The engine should track things such as:

- which strings are vibrating,
- their energy,
- their phase,
- which dampers are raised,
- pedal position,
- current soundboard activity,
- sympathetic coupling,
- interactions between new notes and existing vibration.

This continuous internal state is one of the major advantages of modelling over sample playback.

---

## 3.6 Separate behaviour from timbre

One of the central ideas of the project is that **one technique does not need to solve everything**.

### Physical modelling should primarily control:

- causality,
- timing,
- energy transfer,
- resonance,
- note interaction,
- pedal behaviour,
- dynamic evolution.

### Learned processing should primarily improve:

- timbral realism,
- transient complexity,
- fine spectral irregularities,
- soundboard radiation,
- spatial character.

A useful conceptual split is:

```text
Physical model
= what the piano is doing

Learned acoustic renderer
= how that physical state sounds
```

---

## 3.7 Add differentiable parameter fitting

Traditional physical modelling requires a large amount of manual parameter tuning.

A modern system should make as much of the synthesis chain differentiable as practical.

An optimiser could then fit parameters automatically by minimising the difference between model output and real recordings.

Potential fitted parameters include:

- string damping,
- stiffness,
- hammer characteristics,
- detuning,
- coupling strength,
- modal amplitudes,
- decay constants,
- soundboard modes,
- nonlinear coefficients.

This could dramatically reduce the amount of manual tuning required.

---

## 3.8 Add a neural residual or learned acoustic renderer

A neural residual learns the difference between:

```text
real piano audio
-
physical model output
=
missing acoustic information
```

Instead of asking a neural network to generate the entire piano sound from scratch, the physical model provides the structured behaviour.

The neural system learns only what the model fails to reproduce.

A more ambitious architecture would provide the neural model with the internal physical state:

- string energies,
- partial amplitudes,
- phases,
- soundboard modal activity,
- damper states,
- pedal states.

The neural system would then learn:

> **How would this physical state radiate from a real grand piano?**

This learned acoustic renderer may be the key to combining realistic timbre with genuinely interactive behaviour.

---

## 3.9 Use a Steinway Spirio as a controlled measurement source

Access to a professionally prepared Steinway Spirio would be extremely valuable.

The major benefit is **repeatability**.

Controlled note sequences, velocities and dynamic patterns can be reproduced far more consistently than with a human performer.

A recording studio would be useful, but it is not mandatory.

Useful measurements could include:

- close microphones,
- several microphone positions,
- contact microphones,
- accelerometers,
- measurements near the bridge,
- soundboard vibration measurements,
- room response measurements.

The acoustic room should ideally be treated as a separate stage of the system.

---

## 3.10 Measure only what answers specific questions

Do not begin by recording thousands of samples without a hypothesis.

The preferred workflow is:

```text
build model
-> identify failure
-> design measurement
-> collect data
-> improve model
-> test again
```

Examples:

### If decay sounds wrong
Record controlled long-decay notes.

### If unisons sound artificial
Measure individual and combined string behaviour.

### If pedal resonance is wrong
Design controlled sustain and sympathetic-resonance sequences.

### If the sound feels spatially closed
Measure radiation from several positions around the instrument.

This keeps measurement sessions efficient and scientifically useful.

---

## 3.11 Improve spatial radiation and openness

This should receive unusually high priority.

A real grand piano soundboard is a large, complex radiating surface.

Different modes:

- radiate differently by frequency,
- have different spatial patterns,
- interact with the case,
- interact with the lid,
- change the perceived spectrum depending on listening position.

A simplified system such as:

```text
modal response -> EQ -> stereo output
```

may reproduce the frequency spectrum while still failing perceptually.

This may be one reason why some modelled pianos sound slightly veiled or spatially flat.

GRANDTEQ should investigate whether a learned acoustic radiation model can solve this problem.

---

## 3.12 Add advanced piano interactions

Once the core sound is convincing, expand the physical system to include:

- sympathetic string resonance,
- sustain pedal,
- continuous / partial pedal,
- damper behaviour,
- key release,
- repeated notes,
- una corda,
- interactions between simultaneous notes,
- dense textures.

The system should remain continuous rather than switching between pre-recorded states.

---

## 3.13 Run blind listening tests

Listening tests should be part of development, not merely the final evaluation.

Compare:

- real Steinway,
- high-end sample library,
- Pianoteq,
- Roland modelling,
- GRANDTEQ.

Useful questions include:

- Which version sounds synthetic?
- Which sounds static?
- Which attack sounds most natural?
- Which decay sounds convincing?
- Which has the greatest sense of depth?
- Which sounds most spatially open?
- Can listeners reliably identify the modelled version?

The purpose is to find recurring perceptual failures.

---

## 3.14 Iterate based on perceptual bottlenecks

Do not optimise every subsystem equally.

If listeners consistently identify the prototype because of one characteristic, that characteristic becomes the next research priority.

The development loop should be:

```text
listen
-> identify perceptual failure
-> design measurement
-> improve model
-> fit parameters
-> blind test
-> repeat
```

The goal is to avoid creating a mathematically elegant model that still sounds artificial.

---

# 4. Six-Month Prototype Schedule

## Month 1

- literature review,
- reference listening set,
- minimal MIDI/audio framework,
- analysis tools,
- first single-note model.

## Month 2

- improved hammer/string model,
- unisons,
- decay and damping,
- initial soundboard/modal model,
- comparisons against reference instruments.

## Month 3

- Spirio measurement session,
- parameter identification,
- automated fitting,
- representative notes across the keyboard.

## Month 4

- neural residual or learned timbre renderer,
- improved transients,
- radiation/spatial experiments,
- early blind tests.

## Month 5

- expansion toward full keyboard,
- pedal system,
- sympathetic resonance,
- repeated notes,
- dense textures.

## Month 6

- systematic A/B and ABX testing,
- profiling,
- real-time optimisation,
- identification of remaining characteristic artefacts,
- decision on whether the architecture has commercial potential.

---

# 5. Success Criteria

Success after six months does **not** mean:

> "We have built a better commercial product than Pianoteq."

Success means:

1. The prototype behaves like a continuously modelled instrument.
2. It retains the depth and interaction missing from conventional sampling.
3. Its basic timbre does not immediately reveal the usual physical-modelling signature.
4. Blind listeners find it meaningfully harder to identify as synthetic.
5. The architecture shows a credible path toward a complete instrument.

If these conditions are met, the project can move from research into product development.

---

# 6. Long-Term Product Vision

A future commercial GRANDTEQ could use a clean-sheet architecture with:

- modern CPU/GPU requirements,
- no obligation to maintain compatibility with decades-old modelling choices,
- larger models where acoustically useful,
- physical modelling for interaction,
- learned rendering for realism,
- model-specific acoustic behaviour,
- multiple virtual microphone positions,
- continuous pedal behaviour,
- editable piano parameters,
- room and microphone modelling.

The product should not be positioned merely as:

> "another physically modelled piano."

The goal is a new class of virtual instrument combining the strongest properties of both sampling and physical modelling.

---

## Working Concept

### GRANDTEQ

**Sampled realism without sampled behaviour.  
Modelled behaviour without modelled timbre.**
