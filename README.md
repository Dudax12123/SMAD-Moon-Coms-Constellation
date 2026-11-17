# Lunar Communications Constellation — Space Mission Analysis and Design

Pre-Phase A design study for a lunar communications infrastructure delivering broadband service to
20,000 distributed users with 100% lunar surface coverage. The baseline architecture is a
42-satellite constellation in 7 polar planes at 1000 km altitude, deployed over three launch
campaigns.

Developed for the Space Mission Analysis and Design course of the MSc in Aerospace Engineering at
Instituto Superior Técnico, Universidade de Lisboa, 2025.

## About this repository

This is a **team project**, carried out by a group of nine students. The work — architecture
trade-offs, trajectory design, spacecraft sizing, budgets, launcher selection, cost and risk — was
shared across the group.

This repository holds the computational side of the study. Most of the code here was written by
[Eduardo de Almeida Helena](https://github.com/Dudax12123); `git log` shows the commit history and
authorship for every file. The repository was originally hosted on a teammate's account and the full
history, including all contributors' commits, is preserved here unchanged.

## Mission design results

| Quantity | Value |
|---|---|
| Total mission ΔV | 3.815 km/s |
| Earth departure (TLI) | 3.131 km/s |
| Lunar orbit insertion | 0.684 km/s |
| Transfer duration | 83.8 h |
| Lunar approach eccentricity | 1.398 (hyperbolic) |
| Insertion orbit | 123 × 1500 km |
| Final orbit | 1500 km circular, 62.8° inclination |
| Launch site | Andøya, Norway |
| Satellite mass | 187.89 kg dry · 406.04 kg wet |

## Contents

### `Appendix_C/Orbit_Propagator`

Two-body orbital propagators with verification and error analysis:

- **RK4** — classical fourth-order Runge-Kutta, fixed step
- **RK8** — adaptive Dormand-Prince 8(7) embedded pair with step-size control
- **Kepler** — analytical propagation, used as the reference solution
- Orbital element ↔ state vector conversion utilities
- **J₂-perturbed propagation**, validated against analytical secular rates for RAAN and argument of
  perigee

Verification covers step-size convergence studies, observed order of convergence, Richardson
extrapolation for a zero-step reference, identification of the truncation-to-round-off transition,
time-reversal tests and drift monitoring in conserved quantities.

### `Appendix_C/lambert_solvers`

Lambert's problem in the **universal variable formulation** — prograde and retrograde, handling
elliptical, parabolic and hyperbolic transfers, with a convergence tolerance of 1e-8. Includes the
inverse-Lambert routine via Kepler's equations, solver precision analysis, singular cases
(Δθ = 180°) and validity limits.

Also generates **pork-chop diagrams** — departure, arrival and total-ΔV plots over Earth–Moon
transfer windows, with optimal-transfer identification. A legacy variant uses real ephemerides
through Skyfield.

### `Project/Transfer_Simulation`

Modular trans-lunar transfer tool (`core` / `operations` / `analysis`) with a menu-driven interface:

- Single-trajectory calculation from a given departure state
- Parametric studies over the sphere-of-influence arrival parameter λ₁ across 0–360°
- Multi-parameter optimisation of (V₀, γ₀, λ₁) to minimise total mission ΔV
- Fixed-V₀ optimisation

### `Project/Satellite_Deployment`

Deployment simulation from the 123 × 1500 km insertion ellipse: circularisation at apoapsis,
staggered release of 7 carriers of 6 satellites each, per-carrier inclination change to a 90° polar
orbit, and final deployment to 1000 km.

## Tools

Python · MATLAB · GMAT (constellation modelling and ground tracks) · LaTeX

## A note on repository size

The `results/` directories contain `.npz` files of propagator output, some of them large. They are
simulation products rather than source, and are kept here for reproducibility of the figures in the
report.
