# Damped Harmonic Oscillation Analysis  
**Peak-Based Estimation of Damping Coefficient and Damping Ratio**

---

## 1. Overview

This project analyzes **damped harmonic motion** of a mass–spring system using experimentally recorded position–time data. The primary objective is to **estimate the damping coefficient** \( c \) and **damping ratio** \( \zeta \) from oscillatory decay using **peak detection and logarithmic decrement methods**.

Two complementary approaches are implemented:

1. **Time-domain exponential decay fitting** using individual peak amplitudes  
2. **Logarithmic decrement averaging** across successive oscillation peaks  

Both methods rely on identifying local maxima in the displacement signal and filtering noise-induced false peaks.

---

## 2. Physical Model

We assume a **linear, underdamped, single-degree-of-freedom system** governed by:

\[
m \ddot{x} + c \dot{x} + k x = 0
\]

where:
- \( m \) = mass (kg)  
- \( c \) = viscous damping coefficient (N·s/m)  
- \( k \) = spring constant (N/m)  
- \( x(t) \) = displacement (m)

---

### 2.1 Underdamped Solution

For \( \zeta < 1 \), the displacement is:

\[
x(t) = A e^{-\frac{c}{2m} t} \cos(\omega_d t + \phi)
\]

where:
\[
\omega_d = \sqrt{\frac{k}{m} - \left(\frac{c}{2m}\right)^2}
\]

The **exponential envelope** governs the decay of oscillation peak amplitudes.

---

## 3. Experimental Data

### 3.1 Input Files

Each dataset is a CSV file named according to:

```
<mass>g_trial_<n>.csv
```

or (for batch processing):

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

Mass units are treated consistently across all trials.

---

## 4. Signal Processing Pipeline

### 4.1 Peak Detection

Oscillation peaks are detected using SciPy:

```python
from scipy.signal import find_peaks
peaks, _ = find_peaks(position)
```

To suppress noise-induced false peaks, a minimum displacement threshold is applied:

\[
x_{\text{peak}} \ge 0.2 \text{ m}
\]

This ensures only physically meaningful oscillation maxima are retained.

---

### 4.2 Peak Amplitude Selection

```python
peak_amplitudes = position[peaks]
```

The reference amplitude is chosen as:

\[
A_0 = \max(A_n)
\]

This compensates for cases where the true initial peak is not detected.

---

## 5. Method 1 — Damping Coefficient from Exponential Decay

For a peak occurring at time \( t_n \) with amplitude \( A_n \):

\[
A_n = A_0 e^{-\frac{c}{2m} t_n}
\]

Solving for the damping coefficient:

\[
\boxed{
 c_n = -\frac{2m}{t_n} \ln\left(\frac{A_n}{A_0}\right)
}
\]

Each detected peak produces an instantaneous estimate of \( c \).

---

### 5.1 Averaging

The final damping coefficient is computed as:

\[
\bar{c} = \frac{1}{N} \sum_{n=1}^{N} c_n
\]

---

### 5.2 Damping Ratio

The damping ratio is obtained via:

\[
\boxed{
 \zeta = \frac{\bar{c}}{2\sqrt{k m}}
}
\]

This provides a normalized measure of damping independent of system mass.

---

## 6. Method 2 — Logarithmic Decrement Method

The logarithmic decrement between successive peaks is defined as:

\[
\delta_n = \ln\left(\frac{A_n}{A_{n+1}}\right)
\]

The mean decrement is:

\[
\bar{\delta} = \frac{1}{N-1} \sum_{n=1}^{N-1} \delta_n
\]

The damping ratio is then calculated as:

\[
\boxed{
 \zeta =
 \frac{\bar{\delta}}{\sqrt{4\pi^2 + \bar{\delta}^2}}
}
\]

This method is robust and does not rely on precise time measurements.

---

## 7. Visualization

Optional diagnostic plots are included to verify peak detection and oscillatory decay.

### 7.1 Position–Time Plot with Peaks

```python
plt.plot(time, position)
plt.scatter(time[peaks], peak_amplitudes)
```

These plots are useful for:
- Validating peak detection accuracy  
- Verifying noise filtering  
- Identifying anomalous trials  

---

## 8. Numerical Stability & Error Considerations

- Peak thresholding reduces noise sensitivity  
- Averaging across multiple peaks minimizes random error  
- Logarithmic decrement avoids dependence on absolute amplitude calibration  
- Early peaks have higher signal-to-noise ratios  

---

## 9. Dependencies

```text
Python ≥ 3.9
numpy
pandas
matplotlib
scipy
```

Install all dependencies with:

```bash
pip install numpy pandas matplotlib scipy
```

---

## 10. Outputs

- Printed average damping coefficient \( \bar{c} \) for each mass  
- Printed damping ratio \( \zeta \) for each trial  
- Optional plots suitable for lab reports  

---

## 11. Summary

This project implements **two independent, theoretically grounded methods** for extracting damping properties from experimental oscillation data. Agreement between exponential decay fitting and logarithmic decrement analysis provides strong validation and aligns with classical vibration theory.

---
