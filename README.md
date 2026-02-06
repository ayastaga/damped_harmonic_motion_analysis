# Damped Harmonic Oscillation Analysis  
**Peak-Based Estimation of Damping Coefficient and Damping Ratio**

---

## 1. Overview

This project analyzes **damped harmonic motion** of a mass–spring system using experimentally recorded position–time data. The primary objective is to **estimate the damping coefficient** and **damping ratio** from oscillatory decay using **peak detection** and **logarithmic decrement** methods.

All mathematical expressions are rendered using **Codecogs equation images** to ensure correct display on GitHub and other Markdown renderers.

---

## 2. Physical Model

We assume a **linear, underdamped, single-degree-of-freedom mass–spring–damper system** governed by:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\m%5Cddot%7Bx%7D%2Bc%5Cdot%7Bx%7D%2Bkx%3D0)

where:
- **m** — mass (kg)  
- **c** — viscous damping coefficient (N·s/m)  
- **k** — spring constant (N/m)  
- **x(t)** — displacement (m)

---

### 2.1 Underdamped Solution

For an underdamped system (ζ < 1), the displacement response is:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\x%28t%29%3DAe%5E%7B-%5Cfrac%7Bc%7D%7B2m%7Dt%7D%5Ccos%28%5Comega_d%20t%2B%5Cphi%29)

where the damped natural frequency is:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\%5Comega_d%3D%5Csqrt%7B%5Cfrac%7Bk%7D%7Bm%7D-%5Cleft%28%5Cfrac%7Bc%7D%7B2m%7D%5Cright%29%5E2%7D)

The exponential term defines the **decay envelope** used for damping estimation.

---

## 3. Experimental Data

### 3.1 Input Files

Each dataset is stored as a CSV file using the naming convention:

```
<mass>g_trial_<n>.csv
```

or, for batch processing:

```
physics_data/<mass>g_trial_<n>.csv
```

Each file contains four columns:

| Column        | Description              |
|--------------|--------------------------|
| Time         | Time (s)                 |
| Position     | Displacement (m)         |
| Velocity     | Velocity (m/s)           |
| Acceleration | Acceleration (m/s²)      |

---

### 3.2 Constants Used

```python
k = 5886  # Spring constant (N/m)
masses = [40, 60, 80, 100, 120, 140]  # grams
```

---

## 4. Signal Processing Pipeline

### 4.1 Peak Detection

Oscillation peaks are identified using SciPy’s `find_peaks` function. To suppress noise-induced artifacts, peaks below a minimum displacement threshold are discarded:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\x_%7Bpeak%7D%5Cge0.2%5Ctext%7Bm%7D)

Only physically meaningful oscillation maxima are retained for analysis.

---

## 5. Method 1 — Damping Coefficient via Exponential Decay

For a detected peak at time **tₙ** with amplitude **Aₙ**, the decay envelope satisfies:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\A_n%3DA_0e%5E%7B-%5Cfrac%7Bc%7D%7B2m%7Dt_n%7D)

Solving for the damping coefficient yields:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\c_n%3D-%5Cfrac%7B2m%7D%7Bt_n%7D%5Cln%5Cleft%28%5Cfrac%7BA_n%7D%7BA_0%7D%5Cright%29)

Each oscillation peak produces an instantaneous estimate of **c**.

### 5.1 Averaging

The final damping coefficient is computed as the arithmetic mean:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\%5Cbar%7Bc%7D%3D%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bn%3D1%7D%5ENc_n)

### 5.2 Damping Ratio

The damping ratio is obtained from:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\%5Czeta%3D%5Cfrac%7B%5Cbar%7Bc%7D%7D%7B2%5Csqrt%7Bkm%7D%7D)

---

## 6. Method 2 — Logarithmic Decrement Method

The logarithmic decrement between successive peaks is defined as:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\%5Cdelta_n%3D%5Cln%5Cleft%28%5Cfrac%7BA_n%7D%7BA_%7Bn%2B1%7D%7D%5Cright%29)

The mean logarithmic decrement is:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\%5Cbar%7B%5Cdelta%7D%3D%5Cfrac%7B1%7D%7BN-1%7D%5Csum_%7Bn%3D1%7D%5E%7BN-1%7D%5Cdelta_n)

The damping ratio follows as:

![equation](https://latex.codecogs.com/gif.latex?\bg_white\%5Czeta%3D%5Cfrac%7B%5Cbar%7B%5Cdelta%7D%7D%7B%5Csqrt%7B4%5Cpi%5E2%2B%5Cbar%7B%5Cdelta%7D%5E2%7D%7D)

---

## 7. Visualization

Position–time plots with detected peaks are used for validation and diagnostics:

```python
plt.plot(time, position)
plt.scatter(time[peaks], peak_amplitudes)
```

---

## 8. Numerical Stability & Error Considerations

- Noise thresholding reduces false peak detection  
- Averaging across peaks minimizes random measurement error  
- Logarithmic decrement avoids dependence on absolute amplitude calibration  
- Early oscillation peaks have the highest signal-to-noise ratio

---

## 9. Dependencies

```text
Python ≥ 3.9
numpy
pandas
matplotlib
scipy
```

---

## 10. Outputs

- Average damping coefficient for each mass  
- Damping ratio for each experimental trial  
- Diagnostic plots suitable for lab reports

---

## 11. Summary

This project implements **two independent, physics-consistent techniques** for extracting damping parameters from experimental oscillation data. Using Codecogs-rendered equations ensures full compatibility with GitHub Markdown while preserving mathematical rigor.

---
