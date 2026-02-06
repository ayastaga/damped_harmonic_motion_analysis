# Damped Harmonic Oscillation Analysis

This project analyzes the motion of a mass-spring system to estimate damping properties from experimental position-time data.

---

## Overview

We process CSV files containing time, position, velocity, and acceleration data. The goal is to identify oscillation peaks and calculate damping coefficients and damping ratios.

## Data

Files are named using the format:

```
<mass>g_trial_<n>.csv
```

Each file contains:

* Time (s)
* Position (m)
* Velocity (m/s)
* Acceleration (m/s²)

## Method

1. Identify peaks in the position data using `scipy.signal.find_peaks`.
2. Filter out small or noise-induced peaks.
3. Compute the damping coefficient from peak decay.
4. Calculate the damping ratio for each trial.
5. Optionally, visualize the position over time with detected peaks.

## Dependencies

* Python ≥ 3.9
* numpy
* pandas
* matplotlib
* scipy

Install with:

```
pip install numpy pandas matplotlib scipy
```

## Outputs

* Average damping coefficient for each mass
* Damping ratio for each trial
* Plots showing position-time data with peaks

---

## Summary

This project provides a simple and reliable way to extract damping characteristics from experimental oscillation data, with visualization to validate peak detection and calculations.
