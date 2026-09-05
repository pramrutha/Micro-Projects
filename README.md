# 1D Image Blurring and Restoration Using the Heat Equation

**Course:** 24BSE2113-D — Partial Differential Equations, Transforms & Optimization Techniques

A computational micro-project demonstrating how the one-dimensional heat equation models image blurring, using pixel intensity along a 1D scan line as the diffusing quantity.

## Overview

This project treats pixel intensity as a diffusing quantity governed by the 1D heat equation:

```
∂I/∂t = α ∂²I/∂x²
```

with a step-function initial condition (representing a sharp image edge):

```
I(x,0) = 0,  for 0 ≤ x < L/2
I(x,0) = 1,  for L/2 ≤ x ≤ L
```

and homogeneous Neumann boundary conditions:

```
I_x(0,t) = 0,   I_x(L,t) = 0
```

The notebook solves this analytically via Fourier series and verifies the result numerically, showing how heat diffusion smooths a sharp edge over time — the mathematical basis for image blurring.

## Analytical Solution

The Fourier-series solution to the PDE is:

```
I(x,t) = 1/2 - Σ (2/nπ) sin(nπ/2) cos(nπx/L) exp(-α(nπ/L)²t)
```

where the coefficients are derived as `a₀ = 1` and `aₙ = -(2/nπ) sin(nπ/2)`, giving zero for even n and alternating signs for odd n.

## What the Notebook Does

1. **Setup** — defines physical parameters (`L`, `α`) and numerical parameters (`Nx` spatial samples, `N` Fourier modes)
2. **Initial condition** — constructs and plots the step-function image
3. **Fourier coefficients** — computes and prints the first coefficients analytically
4. **Heat equation solver** — implements `heat_solution(x, t, L, alpha, N_modes)`, evaluating the truncated Fourier series
5. **Verification at t = 0** — confirms the truncated series approximates the step function (with Gibbs-phenomenon oscillations near the discontinuity)
6. **Blurring over time** — plots the intensity profile at several time steps, showing the edge progressively smoothing
7. **Low-pass filter analysis** — shows that the decay factor `exp(-α(nπ/L)²t)` suppresses high-frequency modes (large n) faster than low-frequency ones, since the exponent scales with n²
8. **Mode comparison** — compares decay rates of individual modes (e.g., n=1 vs n=3) and quantifies suppression at a fixed time
9. **Truncation effects** — shows how the number of Fourier modes used affects approximation accuracy and ringing artifacts

## Key Result

The heat equation acts as a **low-pass filter**: since sharp edges are composed of high spatial-frequency components, and those components decay fastest (proportional to `exp(-αn²t)`), the diffusion process selectively removes fine detail while preserving coarse structure — producing the visual effect of blurring.

## Requirements

```
numpy
matplotlib
```

## Usage

Open `MICRO-PROJECTS.ipynb` in Jupyter or Google Colab and run all cells sequentially. Key parameters you can adjust:

- `L` — length of the scan line
- `alpha` — diffusion coefficient (controls blur rate)
- `Nx` — number of spatial samples
- `N_modes` — number of Fourier modes used in the truncated series
- `times` — list of time values to visualize edge evolution

## Files

- `MICRO-PROJECTS.ipynb` — full notebook with derivations, code, and plots
- `MICRO_PROJECT.pdf` — exported PDF version of the notebook

## Conclusion

The one-dimensional heat equation successfully models the diffusion of pixel intensity along an image scan line. The Fourier-series solution shows a sharp edge becoming progressively smoother as time increases, with higher spatial-frequency components decaying faster due to the n² term in the exponential — demonstrating the low-pass filtering behavior underlying image blurring.
