---
title: "Computational Plasma Acceleration and Autoresonance Research"
summary: "Built numerical models and designed particle-in-cell simulations to study electromagnetic-wave propagation and autoresonant control in plasma beat-wave acceleration with UCLA's Laser-Plasma Group."
date: 2025-08-01
org: "UCLA Plasma Accelerator / Laser-Plasma Interactions Group"
tags: ["Computational Physics", "Python", "OSIRIS", "Particle-in-Cell Simulation"]
featured: true
order: 2
image: "/images/projects/plasma-autoresonance-research/pendulum-graph.png"
---

## Background

Particle accelerators are used in medical treatments, X-ray generation, and physics research, but traditional designs span kilometers. Plasma-based accelerators could shrink that to meters. My research focused on a key challenge: keeping the laser frequency matched to the plasma wave as it grows, a problem called **autoresonance**.

To build toward that goal, I combined reduced numerical models of nonlinear wave growth with OSIRIS particle-in-cell simulations of electromagnetic waves in plasma. This let me study plasma dispersion and cutoff behavior while developing the simulation and analysis workflow needed for larger plasma-acceleration studies.

## What I Did

- Built **numerical models** of a driven pendulum oscillator to simulate autoresonant wave amplitude growth under different drive constants, validating behavior against theoretical predictions.
- Designed and ran **OSIRIS particle-in-cell simulations** above and below the plasma-frequency cutoff, iterating wave, plasma, grid, and particle parameters.
- Wrote a **Python/HDF5 analysis pipeline** using FFT spectral analysis and Hilbert-transform envelope detection to extract wavenumber and wave-packet dynamics from simulation outputs.

**Tools:** OSIRIS (UCLA-developed particle-in-cell simulation framework), Python (SciPy, NumPy, matplotlib, h5py)

## Results

**Auto-resonant control of a nonlinear pendulum**

- **Simulated** a driven nonlinear oscillator across four drive strengths to characterize autoresonant behavior.
- **Analyzed** the effects of drive strength and frequency chirping on phase-locking and amplitude growth.
- **Validated** the numerical model by reproducing the expected autoresonance threshold and growth behavior reported in published research.

![Pendulum oscillator autoresonance simulation across drive constants](/images/projects/plasma-autoresonance-research/pendulum-graph.png)

**Auto-resonant control of PWBA using laser chirping**

- **Modeled** plasma beat-wave dynamics with different chirp rates.
- **Compared** chirped and unchirped wave growth.
- **Validated** sustained amplitude growth under autoresonance.

![Plasma beat wave acceleration under three chirp modes](/images/projects/plasma-autoresonance-research/chirp-graph.png)

**OSIRIS particle-in-cell simulation and spectral analysis**

- **Designed and iterated** one-dimensional OSIRIS scenarios above and below the plasma-frequency cutoff, selecting the wave frequency, plasma geometry, grid resolution, and particles per cell to balance numerical accuracy and signal quality.
- **Reconstructed** electric-field and plasma-density histories from HDF5 outputs to visualize pulse propagation, partial reflection and transmission above cutoff, and reflection below cutoff.
- **Applied a Hann-windowed FFT** to spatial electric-field data to isolate the dominant spectral peak, extract wavenumbers in vacuum and plasma, and compare them with the electromagnetic dispersion relation.
- **Developed additional diagnostics** using Hilbert-transform envelope tracking, linear fitting, and field-amplitude measurements to analyze group velocity, penetration depth, and electric-to-magnetic-field relationships.

![OSIRIS particle-in-cell simulation of an electromagnetic pulse propagating through a finite plasma region](/images/projects/plasma-autoresonance-research/em-wave.gif)

## What I Learned

This was my first exposure to a research-grade simulation workflow. I learned how numerical models and large-scale simulations cross-validate each other, and how to extract meaningful signal from noisy raw data.

![Research poster: Auto-Resonance in 1D Plasma Beat-Wave Acceleration](/images/projects/plasma-autoresonance-research/poster.jpg)
