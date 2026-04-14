# Time---Series---Forecasting-using-hankelization-and-DMD
# Data-Driven Forecasting of Dynamical Systems: SVD-DMD & Koopman Operator Theory

This repository contains MATLAB implementations for modeling, analyzing, and forecasting complex dynamical systems using time-delay embedding (Hankelization), Singular Value Decomposition (SVD), and Dynamic Mode Decomposition (DMD). It extends standard linear forecasting techniques into highly non-linear and chaotic regimes utilizing Koopman Operator Theory.

## Overview

The core objective of this project is to forecast time series data from the system observables. By projecting high-dimensional phase spaces into low-rank SVD subspaces, the pipeline can analytically reconstruct and predict future system states without relying on iterative numerical integration. 

The project progressively scales in complexity, proving the architecture against known physical baselines before chaotic system.

## Repository Structure

* **`Linear_System_Modelling.mlx`**: Standard DMD pipeline applied to a linear Damped Harmonic Oscillator. Serves as the ground-truth validation for the algorithmic architecture.
* **`Non_Linear_System_Modelling.mlx`**: DMD applied to a Van der Pol Oscillator. Demonstrates the need for higher rank truncation to capture the harmonics of a stable limit cycle.
* **`Lorenz_Visualizer.mlx`**: A standalone interactive script to visualize the continuous 3D phase space geometry of the Lorenz attractor.
* **`Lorenz_Dead_State.mlx`**: Koopman Uplifting applied to the Lorenz system ($\rho = 0.5$).
* **`Lorenz_Steady_Convection.mlx`**: Koopman Uplifting applied to the Lorenz system ($\rho = 15$).
* **`Lorenz_Chaos.mlx`**: Koopman Uplifting applied to the chaotic Lorenz regime ($\rho = 28$). Analyzes the limitations and capabilities of linear operators in predicting the butterfly effect.
* **`outputs/`**: Contains generated phase portraits, time-series forecasting plots, and eigenvalue spectra visualizations.
* **`pipeline_diagrams/`**: Contains visual documentation of the Hankelization and Koopman uplifting processes.

## Mathematical Pipelines

This repository utilizes two distinct data-processing architectures depending on the complexity of the target system.

### 1. The Standard DMD Pipeline (For Linear / Weakly Non-Linear Systems)
1. **Hankelization (Phase Space Reconstruction):** Time-delay embedding of raw 1D time series into an $L \times K$ trajectory matrix (supported by Takens' Theorem).
2. **Data Splitting:** Temporal separation of the matrix into $X$ (Past) and $X'$ (Future).
3. **Dimensionality Reduction:** SVD applied to $X$ to isolate dominant spatial modes and filter noise.
4. **Operator Extraction:** Computation of the reduced-order linear operator $\tilde{A}$.
5. **Spectral Decomposition & Forecasting:** Extraction of discrete eigenvalues and eigenvectors to analytically project the system forward in time.

### 2. The Koopman Uplifting Pipeline (For Chaotic Systems)
Based on Koopman Operator Theory, which maps finite non-linear dynamics to infinite-dimensional linear spaces:
1. **Observable Dictionary Expansion:** Raw physical variables ($x, y, z$) are passed through a non-linear dictionary (e.g., polynomials up to $x^3, y^3, z^3$ and cross-terms like $xy, x^2z$).
2. **Feature Stacking:** The outputs are vertically stacked to create a highly augmented, multi-dimensional state vector (e.g., expanding 3 variables into 15).
3. **Hankelization & DMD:** The augmented matrix is passed through the standard Hankelization and SVD-DMD pipeline.

## Results Summary

* **Linear Systems:** Achieved near-zero error in identifying exact exponential decay envelopes and oscillatory frequencies.
* **Non-Linear Limit Cycles:** Successfully captured the asymmetric growth and decay phases of the Van der Pol oscillator by elevating the SVD rank.
* **Chaotic Regimes:** The 15-term Koopman dictionary successfully localized the stable geometries of the Lorenz dead state and convection state. In the chaotic regime ($\rho=28$), the model accurately maps the short-term trajectory and attractor bounds before diverging, successfully illustrating the butterfly effect (extreme sensitivity to initial conditions).

## How to Run
1. Clone this repository to your local machine.
2. Open MATLAB.
3. Run any of the `.mlx` scripts directly. Each script is self-contained and will automatically generate the corresponding datasets, compute the models, and output the analytical dashboard figures.
